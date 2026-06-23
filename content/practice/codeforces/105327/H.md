---
title: "CF 105327H - Sóng hài có nhiễu"
description: "Chúng ta có hai chuỗi nhị phân được truyền cùng nhau, nhưng một số vị trí có thể đã bị hỏng thành ký hiệu ký tự đại diện. Một chuỗi đại diện cho một số nhị phân lớn, chuỗi còn lại đại diện cho một số nhị phân nhỏ hơn nhiều."
date: "2026-06-22T12:37:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105327
codeforces_index: "H"
codeforces_contest_name: "2024-2025 ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 105327
solve_time_s: 71
verified: true
draft: false
---

[CF 105327H - Sóng hài có nhiễu](https://codeforces.com/problemset/problem/105327/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi nhị phân được truyền cùng nhau, nhưng một số vị trí có thể đã bị hỏng thành ký hiệu ký tự đại diện`*`. Một chuỗi đại diện cho một số nhị phân lớn, chuỗi còn lại đại diện cho một số nhị phân nhỏ hơn nhiều. Quá trình truyền ban đầu có đặc tính là giá trị được mã hóa bởi chuỗi đầu tiên chia hết cho giá trị được mã hóa bởi chuỗi thứ hai. 

Nhiệm vụ của chúng ta là xây dựng lại bất kỳ chuỗi nhị phân gốc hợp lệ nào cho số đầu tiên, nhất quán với thông tin từng phần nhận được, sao cho tồn tại ít nhất một phần hoàn thành của chuỗi thứ hai làm cho điều kiện chia hết là đúng. Một số vị trí được cố định như`0`hoặc`1`, và một số chưa được biết. Tổng số bit chưa biết trên cả hai chuỗi là nhỏ, nhiều nhất là 16, mặc dù bản thân chuỗi đầu tiên có thể dài tới 500 bit. 

Cấu trúc này ngụ ý một sự bất đối xứng mạnh mẽ. Số lượng lớn có thể có nhiều vị trí chưa biết, nhưng tổng cộng chỉ có một số lượng nhỏ trong số đó tồn tại. Số thứ hai ngắn, nhiều nhất là 16 bit, do đó giá trị số của nó luôn đủ nhỏ để phép tính mô-đun trên nó là khả thi. Điều kiện chia hết cho thấy chúng ta cần suy luận về số dư thay vì số nguyên đầy đủ. 

Khó khăn chính là việc tái cấu trúc cả hai chuỗi sẽ dẫn đến sự phân nhánh theo cấp số nhân lên tới 16 bit chưa biết. Đó là nhiều nhất$2^{16} = 65536$các khả năng có thể xảy ra ở mức giới hạn nhưng vẫn khả thi nếu mỗi ứng viên được kiểm tra một cách hiệu quả. 

Một trường hợp cạnh tinh tế phát sinh khi số thứ hai cực kỳ nhỏ, đặc biệt là khi nó có giá trị bằng 1 hoặc 2. Trong những trường hợp đó, các ràng buộc về khả năng chia hết trở nên tầm thường hoặc suy biến và việc triển khai bất cẩn giả định tính khả nghịch hoặc hành vi mô đun khác 0 có thể thất bại. Một trường hợp khác là khi phân bố ký tự đại diện bị lệch nhiều về phía chuỗi lớn, buộc hầu hết sự phân nhánh xảy ra theo đóng góp vị trí thay vì trực tiếp trong mô đun. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi cách để thay thế mọi`*`trong cả hai chuỗi với`0`hoặc`1`, tính các số nguyên thu được và kiểm tra tính chia hết. Vì có tổng cộng tối đa 16 bit chưa biết nên điều này tạo ra tối đa$2^{16}$ứng viên. Mỗi ứng cử viên yêu cầu xây dựng các số nguyên có khả năng 500 bit, nhưng điều đó có thể được xử lý thông qua đánh giá nhị phân gia tăng hoặc số học mô-đun. Điều này đúng nhưng sẽ trở nên dễ hỏng nếu được thực hiện trực tiếp trên các số nguyên lớn nhiều lần, đặc biệt nếu chúng ta tính toán lại các giá trị đầy đủ mỗi lần. 

Quan sát quan trọng là chúng ta không bao giờ cần giá trị đầy đủ của số lớn, chỉ phần còn lại của nó modulo số nhỏ được xây dựng. Sau khi cố định ứng cử viên cho số thứ hai, chúng ta có thể tính giá trị của nó$N$, sau đó diễn giải chuỗi lớn dưới dạng số nhị phân theo modulo$N$trong một lần đi qua. Điều này làm giảm mỗi lần kiểm tra thành thời gian tuyến tính theo chiều dài của chuỗi lớn. 

Cấu trúc của bài toán do đó trở thành một tìm kiếm hai cấp độ. Đầu tiên, chúng tôi liệt kê tất cả các phần hoàn thành hợp lệ của chuỗi nhỏ, vì nó chứa tối đa 16 bit chưa biết và dù sao cũng ngắn. Sau đó, đối với mỗi mô đun ứng cử viên như vậy, chúng tôi tính toán xem liệu có tồn tại sự hoàn thành của chuỗi lớn phù hợp với nó mà mang lại phần còn lại bằng 0 hay không. Vì các bit chưa biết bị giới hạn trên toàn cầu nên chúng ta có thể coi tất cả các vị trí chưa biết là một nhóm các quyết định nhị phân duy nhất và phân phối chúng giữa các vị trí theo cách đảm bảo tính nhất quán. 

Brute Force hoạt động vì không gian không xác định nhỏ nhưng sẽ không thành công nếu chúng ta liên tục tính toán lại các số nguyên đầy đủ hoặc coi chuỗi lớn là cần liệt kê đầy đủ. Quan sát rằng số học mô-đun nén việc đánh giá chuỗi lớn cho phép mỗi phép gán ứng viên được xác thực theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các bài tập với tính toán lại đầy đủ |$O(2^{16} \cdot n)$với hằng số lớn |$O(1)$| Quá chậm / dễ vỡ | 
| Liệt kê các bài tập chuỗi nhỏ + kiểm tra mô-đun |$O(2^{k} \cdot n)$,$k \le 16$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành bằng cách tách cấu trúc chưa biết thành hai giai đoạn: chọn số kiểm soát và xác thực thông báo. 

1. Liệt kê tất cả các khả năng thay thế của`*`trong chuỗi thứ hai. Mỗi bit chưa biết là 0 hoặc 1, vì vậy chúng tôi tạo ra mọi số nhị phân ứng cử viên cho số chia. Điều này khả thi vì tổng số bit chưa biết nhiều nhất là 16, vì vậy ngay cả khi tất cả đều nằm trong chuỗi này thì tổng số kết hợp vẫn bị giới hạn. 
2. Với mỗi chuỗi thứ hai ứng cử viên, hãy tính giá trị nguyên của nó$N$. Nếu như$N = 0$, hãy bỏ qua nó, vì tính chia hết là không xác định và bài toán cũng đảm bảo ít nhất một`1`tồn tại nên các ứng viên hợp lệ luôn tồn tại. 
3. Đối với cố định$N$, cố gắng xây dựng sự hoàn thành hợp lệ của chuỗi đầu tiên. Chúng tôi coi chuỗi đầu tiên là số nhị phân và đánh giá nó theo modulo$N$. Thay vì xây dựng số nguyên đầy đủ, chúng tôi quét từ trái sang phải để duy trì phần còn lại. Tại mỗi vị trí, nếu bit cố định thì chúng ta sử dụng nó; nếu nó là`*`, chúng tôi thử cả hai khả năng, nhưng chúng tôi thực hiện việc này một cách có kiểm soát bằng cách nhớ rằng tổng số phân nhánh bị giới hạn trên toàn cầu. 
4. Để tránh hiện tượng bùng nổ theo cấp số nhân trên chuỗi lớn, chúng tôi khai thác thực tế là các bit chưa xác định trên cả hai chuỗi đều bị giới hạn trên toàn cầu. Chúng ta gán các quyết định cho những vị trí chưa biết này dưới dạng một không gian trạng thái duy nhất. Mỗi bài tập đầy đủ sẽ sửa cả hai chuỗi cùng một lúc. 
5. Đối với mỗi phép gán đầy đủ, chúng tôi tính toán các giá trị nguyên của cả hai chuỗi theo thời gian tuyến tính và kiểm tra xem liệu$M \bmod N = 0$. Nếu nó giữ, chúng tôi xuất kết quả được xây dựng$M$ngay lập tức. 
6. Nếu không có bài tập nào phù hợp với một lựa chọn cụ thể$N$, chúng ta tiếp tục với ứng viên tiếp theo cho đến khi tìm được cặp hợp lệ. 

Tính chính xác phụ thuộc vào thực tế là mọi khả năng hoàn thành của chuỗi nhận được đều tương ứng với chính xác một phép gán trong số tối đa 16 bit ký tự đại diện, do đó việc liệt kê tất cả các phép gán đảm bảo bao phủ tất cả các lần truyền ban đầu hợp lệ. 

### Tại sao nó hoạt động 

Không gian trạng thái không chắc chắn hoàn toàn được chứa trong các vị trí ký tự đại diện. Mỗi vị trí như vậy đóng góp chính xác một quyết định nhị phân. Thuật toán liệt kê tất cả các quyết định như vậy và đối với mỗi quyết định sẽ đánh giá xem cặp cảm ứng có thỏa mãn ràng buộc chia hết hay không. Vì mọi lần truyền ban đầu hợp lệ đều tương ứng với một trong các phép gán này nên việc tìm kiếm không thể bỏ lỡ lời giải. Đồng thời, không có sự hoàn thành không hợp lệ nào có thể vượt qua kiểm tra mô-đun vì số học được tính toán chính xác cho từng ứng viên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_candidates(s):
    pos = [i for i, c in enumerate(s) if c == '*']
    k = len(pos)
    res = []

    for mask in range(1 << k):
        arr = list(s)
        for i in range(k):
            arr[pos[i]] = '1' if (mask >> i) & 1 else '0'
        res.append(''.join(arr))
    return res

def to_int(bin_str):
    val = 0
    for c in bin_str:
        val = (val << 1) + (c == '1')
    return val

def check(M, N):
    if N == 0:
        return False
    val = 0
    for c in M:
        val = (val << 1) + (c == '1')
        val %= N
    return val == 0

def main():
    M_raw = input().strip()
    N_raw = input().strip()

    N_candidates = build_candidates(N_raw)
    M_candidates = build_candidates(M_raw)

    for N_str in N_candidates:
        N_val = to_int(N_str)
        if N_val == 0:
            continue

        for M_str in M_candidates:
            if check(M_str, N_val):
                print(M_str)
                return

if __name__ == "__main__":
    main()
```Giải pháp trước tiên mở rộng tất cả các lần hoàn thành ký tự đại diện cho cả hai chuỗi. Điều này an toàn vì tổng số ký tự đại diện nhiều nhất là 16, do đó không gian tìm kiếm kết hợp vẫn bị giới hạn. Mỗi ước số ứng viên được chuyển đổi thành một số nguyên, sau đó mỗi thông báo ứng cử viên được kiểm tra bằng cách tính phần dư của nó theo modulo của ước số đó. 

Chi tiết triển khai chính là đánh giá mô-đun của chuỗi nhị phân lớn. Thay vì xây dựng các số nguyên lớn, mã duy trì phần dư luân phiên bằng cách sử dụng dịch chuyển bit, đảm bảo mức sử dụng bộ nhớ không đổi và thời gian tuyến tính cho mỗi lần kiểm tra. 

Một lỗi phổ biến là tính toán lại các giá trị số nguyên nhiều lần cho mỗi cặp, việc này không cần thiết nhưng vẫn có thể chấp nhận được trong các điều kiện ràng buộc; tuy nhiên, việc sử dụng tích lũy mô-đun giúp giải pháp ổn định và nhanh chóng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
111*
1*
```Chúng tôi liệt kê các khả năng hoàn thành của chuỗi thứ hai. Nó có thể trở thành`10`hoặc`11`. Những giá trị này tương ứng với giá trị 2 và 3. 

| Bước | ứng cử viên N | Ứng viên M | Giá trị M (nhị phân) | M mod N | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 10 (2) | 1110 | 14 | 0 | vâng | 

Cặp hợp lệ đầu tiên xuất hiện ngay khi chọn`N = 10`Và`M = 1110`. 

Điều này xác nhận rằng thuật toán khám phá chính xác cả hai nhánh không chắc chắn và xác định cặp nhất quán đầu tiên. 

### Mẫu 2 

đầu vào:```
101**
11
```Ở đây số thứ hai được cố định là`11`, bằng 3. Số đầu tiên có hai bit chưa biết, tạo ra bốn ứng cử viên. 

| Ứng viên M | Giá trị | Giá trị mod 3 | hợp lệ | 
| --- | --- | --- | --- | 
| 10100 | 20 | 2 | không | 
| 10101 | 21 | 0 | vâng | 
| 10110 | 22 | 1 | không | 
| 10111 | 23 | 2 | không | 

Sự hoàn thành hợp lệ duy nhất là`10101`và thuật toán tìm thấy nó bằng cách kiểm tra tất cả các lần hoàn thành theo mô-đun 3. 

Điều này chứng tỏ cách kiểm tra mô-đun lọc không gian ứng cử viên một cách hiệu quả mà không yêu cầu bất kỳ lý luận cấu trúc nào về các mẫu chia hết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(2^{k} \cdot n)$| Mỗi phép gán tối đa 16 ký tự đại diện được kiểm tra bằng cách quét các chuỗi một lần | 
| Không gian |$O(1)$| Chỉ các chuỗi tạm thời và phần còn lại được lưu trữ | 

Ràng buộc tổng số bit chưa biết tối đa là 16 đảm bảo hệ số mũ vẫn bị giới hạn. Quét tuyến tính tối đa 500 bit cho mỗi lần kiểm tra vừa vặn thoải mái trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins

    output = []

    def fake_print(*args):
        output.append(" ".join(map(str, args)))

    global print
    old_print = print
    print = fake_print
    try:
        main()
    finally:
        print = old_print

    return "\n".join(output).strip()

# provided samples
assert run("111*\n1*\n") == "1110", "sample 1"
assert run("101**\n11\n") == "10101", "sample 2"

# custom cases
assert run("1*\n1*\n") in {"10", "11"}, "minimum size"
assert run("0*\n1*\n") in {"00", "01"}, "leading zero handling"
assert run("*\n1*\n") in {"0", "1"}, "single bit edge"
assert run("111111\n1\n") == "111111", "no wildcard case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1* / 1*`|`10 or 11`| độ chính xác phân nhánh tối thiểu | 
|`0* / 1*`|`00 or 01`| xử lý số 0 hàng đầu | 
|`* / 1*`|`0 or 1`| cấu trúc đầu vào nhỏ nhất | 
|`111111 / 1`|`111111`| con đường không có ký tự đại diện xác định | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi số thứ hai cực kỳ nhỏ, chẳng hạn như`1`hoặc`10`. Nếu nó là`1`, mọi số đều chia hết nên mọi sự hoàn thành của chuỗi đầu tiên đều hợp lệ. Thuật toán vẫn hoạt động vì nó kiểm tra tất cả các phép gán và phép gán hợp lệ đầu tiên sẽ được trả về ngay lập tức. 

Một trường hợp khác là khi tất cả các ký tự đại diện đều nằm trong chuỗi thứ hai. Trong trường hợp đó, vòng lặp bên ngoài liệt kê tất cả các ước số trước tiên. Đối với mỗi ước số, chuỗi đầu tiên được kiểm tra nguyên trạng. Điều này tránh mọi sự phụ thuộc giữa hai chuỗi và đảm bảo tính chính xác ngay cả khi số chia thay đổi đáng kể giữa các ứng cử viên. 

Trường hợp cạnh cuối cùng là khi các số 0 đứng đầu xuất hiện trong các chuỗi được xây dựng lại. Vì giải thích nhị phân cho phép các số 0 đứng đầu mà không thay đổi giá trị hợp lệ nên thuật toán không loại bỏ chúng. Bất kỳ sự hoàn thành nào như vậy vẫn nhất quán với việc kiểm tra tính chia hết, do đó việc trả về bất kỳ chuỗi hợp lệ nào là đủ.
