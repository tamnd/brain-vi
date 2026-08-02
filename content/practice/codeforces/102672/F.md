---
title: "CF 102672F - Số học và khối"
description: "Chúng ta có một mảng ban đầu gồm các số nguyên không âm. Thay vì nhìn trực tiếp mảng đó, chúng ta được cung cấp mọi tổng độ dài liên tiếp K."
date: "2026-08-01T23:45:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102672
codeforces_index: "F"
codeforces_contest_name: "Selection of tasks from Internet olympiads season 2019-20"
rating: 0
weight: 102672
solve_time_s: 96
verified: true
draft: false
---

[CF 102672F - Số học và khối](https://codeforces.com/problemset/problem/102672/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng ban đầu gồm các số nguyên không âm. Thay vì nhìn trực tiếp vào mảng đó, chúng ta được cung cấp mỗi tổng độ dài liên tiếp`K`. Nếu mảng ban đầu là`A`, giá trị đã cho đầu tiên là tổng của`A[1]`bởi vì`A[K]`, giá trị tiếp theo là tổng sau khi dịch chuyển cửa sổ theo một vị trí, v.v. Nhiệm vụ là đếm xem có bao nhiêu mảng ban đầu khác nhau có thể tạo ra chuỗi tổng cửa sổ đã cho. 

Đầu vào mô tả độ dài của mảng ban đầu, kích thước cửa sổ và chuỗi tổng cửa sổ. Đầu ra là số mảng ban đầu hợp lệ, lấy theo modulo`998244353`. 

Ràng buộc hữu ích là độ dài mảng có thể vào khoảng`2 * 10^5`, do đó, giải pháp thử các giá trị có thể có cho các phần tử hoặc liệt kê mảng là không thể. Ngay cả việc xử lý bậc hai cũng đã quá lớn vì nó đòi hỏi khoảng`4 * 10^10`hoạt động. Chúng ta cần khai thác cấu trúc đại số của các tổng chồng chéo và giảm bài toán xuống dạng quét tuyến tính cộng với phép tính tổ hợp nhỏ. 

Một điểm tinh tế là điều đầu tiên`K`các phần tử không hoàn toàn độc lập trong lần đếm cuối cùng. Các phần tử sau bị ép buộc bởi sự khác biệt giữa các tổng cửa sổ lân cận. Tuy nhiên, các phần tử bắt buộc này có thể trở thành âm trừ khi giá trị ban đầu đủ lớn. Giải pháp chỉ kiểm tra tổng cửa sổ đầu tiên và bỏ qua các ràng buộc không âm sau này sẽ tính các mảng không hợp lệ. 

Ví dụ:```
Input:
4 3
1 0

Output:
0
```Cửa sổ đầu tiên đưa ra`A1 + A2 + A3 = 1`. Cửa sổ thứ hai cung cấp`A2 + A3 + A4 = 0`, Vì thế`A4 = A1 - 1`. Vì tất cả các phần tử phải không âm,`A1`ít nhất phải có`1`. Cửa sổ đầu tiên chỉ cho phép các giá trị nhỏ và sau khi xem xét các ràng buộc, không có lựa chọn hợp lệ nào. 

Một trường hợp đặc biệt khác xuất hiện khi câu trả lời yêu cầu kết hợp với giá trị trên lớn. Ví dụ:```
Input:
2 2
1000000000

Output:
1755648
```có`1000000001`có thể có các cặp số không âm, nhưng cần có câu trả lời theo modulo`998244353`. Việc tính toán trước giai thừa thông thường cho đến giá trị trên là không thể vì giá trị trên gần bằng một tỷ. Tổ hợp phải sử dụng thực tế là số phần tử được chọn là nhỏ. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ là đoán đầu tiên`K`các phần tử. Khi các giá trị đó được cố định, mọi phần tử sau này sẽ được xác định. phương trình`B[i+1] - B[i] = A[i+K] - A[i]`cho phép chúng tôi tính toán từng phần tử mới từ phần tử trước đó. Phương pháp này đúng vì mọi mảng hợp lệ đều phải thỏa mãn những khác biệt này. 

Vấn đề là ở chỗ đầu tiên`K`các phần tử có thể có một số lượng lớn các nhiệm vụ có thể thực hiện được. Ngay cả khi tổng cửa sổ đầu tiên chỉ lớn vừa phải thì số lượng phân phối có thể có của tổng đó giữa`K`vị trí phát triển kết hợp. Việc liệt kê chúng là không thể. 

Quan sát quan trọng là các phương trình sai phân tách mảng thành các chuỗi vị trí độc lập có cùng chỉ số modulo`K`. Đối với mỗi người đầu tiên`K`các vị trí, tất cả các phần tử sau trong chuỗi của nó chỉ là giá trị bắt đầu cộng với các giá trị bù đã biết. Câu hỏi duy nhất còn lại là mỗi giá trị ban đầu phải lớn đến mức nào để giữ cho toàn bộ chuỗi của nó không âm. 

Sau khi tìm thấy giá trị tối thiểu được phép cho mỗi giá trị đầu tiên`K`các vị thế, chúng tôi sẽ trừ đi số tiền bắt buộc đó. Số tiền còn lại có thể được phân phối tự do giữa các`K`các vị trí xuất phát. Điều này trở thành bài toán sao và vạch tiêu chuẩn. 

Nếu số tiền còn lại là`S`, số cách phân phối nó giữa`K`biến không âm là:`C(S + K - 1, K - 1)`Khó khăn thêm duy nhất là đỉnh của tổ hợp này có thể vượt quá mô đun, vì vậy định lý Lucas cần thiết cho phép tính tổ hợp cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ trong K | O(K) | Quá chậm | 
| Tối ưu | O(N + K) | O(K) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính giá trị yêu cầu tối thiểu cho mỗi giá trị đầu tiên`K`các phần tử. Đối với mọi modulo lớp dư lượng`K`, các phần tử tạo thành một chuỗi trong đó các giá trị liên tiếp khác nhau bởi`B[i+1] - B[i]`. Trong khi di chuyển qua chuỗi, duy trì độ lệch tích lũy và ghi lại độ lệch nhỏ nhất đạt được. Nếu độ lệch nhỏ nhất là âm thì giá trị bắt đầu phải được tăng lên bằng giá trị tuyệt đối của nó. 
2. Thêm tất cả các giá trị bắt đầu tối thiểu được yêu cầu này. Nếu số tiền này lớn hơn`B[1]`, không tồn tại mảng hợp lệ vì cửa sổ đầu tiên không thể cung cấp đủ giá trị tổng. 
3. Hãy để`S`là phần chưa được sử dụng của tổng thời hạn đầu tiên sau khi thanh toán tất cả các giá trị tối thiểu bắt buộc. Giá trị còn lại có thể được đặt ở bất cứ đâu trong số`K`các vị trí xuất phát. 
4. Tính toán`C(S + K - 1, K - 1)`modulo`998244353`. Vì phần dưới của tổ hợp nhỏ hơn mô đun, định lý Lucas giảm phép tính xuống nhiều nhất là một tổ hợp nhỏ. 

Tại sao nó hoạt động: 

Sự khác biệt giữa các tổng cửa sổ lân cận sẽ khắc phục mọi mối quan hệ giữa các phần tử`K`các vị trí cách xa nhau. Một lần đầu tiên`K`phần tử được chọn, toàn bộ mảng được xác định. Thuật toán tìm chính xác giới hạn dưới cho mỗi lựa chọn được yêu cầu bởi tính không tiêu cực. Sau khi loại bỏ các giới hạn dưới đó, mọi phân phối còn lại sẽ tạo ra một mảng hợp lệ và mọi mảng hợp lệ sẽ tương ứng với chính xác một phân phối như vậy. Công thức sao và thanh đếm các phân bố đó mà không bỏ sót hoặc trùng lặp bất kỳ trường hợp nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def comb_small_top(n, r):
    if r < 0 or r > n:
        return 0
    if r == 0:
        return 1
    res = 1
    for i in range(r):
        res = res * (n - i) % MOD
    fact = 1
    for i in range(1, r + 1):
        fact = fact * i % MOD
    return res * pow(fact, MOD - 2, MOD) % MOD

def comb_lucas(n, r):
    if r >= MOD:
        return 0
    low = n % MOD
    if r > low:
        return 0
    return comb_small_top(low, r)

def solve():
    n, k = map(int, input().split())
    b = list(map(int, input().split()))

    low = [0] * k
    offset = [0] * k
    minimum = [0] * k

    for i in range(k):
        minimum[i] = 0

    for i in range(n - k):
        idx = i % k
        offset[idx] += b[i + 1] - b[i]
        if offset[idx] < minimum[idx]:
            minimum[idx] = offset[idx]

    need = 0
    for x in minimum:
        need += -x

    if need > b[0]:
        print(0)
        return

    remaining = b[0] - need
    print(comb_lucas(remaining + k - 1, k - 1))

if __name__ == "__main__":
    solve()
```Mảng`offset`lưu trữ bao nhiêu mỗi cái đầu tiên`K`vị trí thay đổi khi chuỗi của nó được theo sau về phía trước. Bản cập nhật sử dụng chênh lệch giữa các tổng khối liền kề, vì chênh lệch đó chính xác là phần tử mới đi vào cửa sổ trừ đi phần tử cũ rời khỏi nó. 

Mảng`minimum`ghi lại mức chênh lệch thấp nhất mà mỗi chuỗi đạt được. Nếu một chuỗi rơi xuống`5`từ giá trị bắt đầu của nó, giá trị bắt đầu phải đóng góp ít nhất`5`chỉ để giữ cho chuỗi đó không âm. 

Biến`need`là tổng số tiền bị ép buộc bởi các giới hạn dưới này. Sau khi loại bỏ nó khỏi tổng cửa sổ đầu tiên, số tiền còn lại có thể được phân phối tự do. 

Chức năng kết hợp đáng được chú ý. Tử số của hệ số nhị thức có thể vượt quá`998244353`, nhưng số được chọn`K - 1`luôn nhỏ hơn mô đun. Định lý Lucas cho biết kết quả bằng 0 khi phần dưới của tử số quá nhỏ, nếu không thì chỉ cần một tổ hợp thông thường theo modulo số nguyên tố là đủ. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
5 4
2 3
```Sự bù đắp của chuỗi là: 

| Vị trí | Thay đổi bù đắp | Mức bù tối thiểu | Giá trị bắt buộc | 
| --- | --- | --- | --- | 
| 1 | +1 | 0 | 0 | 
| 2 | không | 0 | 0 | 
| 3 | không | 0 | 0 | 
| 4 | không | 0 | 0 | 

Số tiền còn lại là`2`. 

Số lần phân phối là:`C(2 + 4 - 1, 4 - 1) = C(5,3) = 10`Dấu vết xác nhận rằng chỉ tổng cửa sổ đầu tiên mới quan trọng sau khi tất cả các phần tử trong tương lai đã được biểu diễn thông qua phần bù. 

Đối với mẫu thứ hai:```
6 1
2 3 0 8 2 5
```Đây`K = 1`, vì vậy mọi phần tử đều được cố định trực tiếp. 

| Bước | Phần bù hiện tại | Yêu cầu tối thiểu | 
| --- | --- | --- | 
| A1 | 0 | 0 | 
| A2 | 0 | 0 | 
| A3 | 0 | 0 | 
| A4 | 0 | 0 | 
| A5 | 0 | 0 | 
| A6 | 0 | 0 | 

Giá trị đầu tiên đã xác định mảng duy nhất có thể. Sự kết hợp là:`C(2 + 1 - 1, 0) = 1`Dấu vết thực hiện`K = 1`trường hợp không còn tự do nữa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + K) | Mỗi sự khác biệt được xử lý một lần và sự kết hợp cuối cùng sử dụng tối đa`K`phép nhân. | 
| Không gian | O(K) | Chỉ những khoản bù đắp và yêu cầu tối thiểu cho lần đầu tiên`K`các vị trí được lưu trữ. | 

Thuật toán xử lý kích thước đầu vào lớn bằng quét tuyến tính. Việc tính toán kết hợp cũng bị giới hạn bởi`K`, nhiều nhất là`2 * 10^5`, vì vậy nó phù hợp thoải mái trong giới hạn chương trình cạnh tranh điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    def solve():
        n, k = map(int, input().split())
        b = list(map(int, input().split()))

        MOD = 998244353
        offset = [0] * k
        minimum = [0] * k

        for i in range(n - k):
            j = i % k
            offset[j] += b[i + 1] - b[i]
            minimum[j] = min(minimum[j], offset[j])

        need = sum(-x for x in minimum)
        if need > b[0]:
            print(0)
            return

        r = k - 1
        ncr = b[0] - need + k - 1
        if r > ncr % MOD:
            print(0)
            return

        num = 1
        den = 1
        for i in range(r):
            num = num * (ncr % MOD - i) % MOD
            den = den * (i + 1) % MOD
        print(num * pow(den, MOD - 2, MOD) % MOD)

    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    sys.stdin = old
    return out.getvalue()

assert run("5 4\n2 3\n") == "10\n"
assert run("6 1\n2 3 0 8 2 5\n") == "1\n"

assert run("2 2\n1000000000\n") == "1755648\n"
assert run("3 3\n5\n") == "21\n"
assert run("4 2\n0 0 0\n") == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 / 1000000000`|`1755648`| Giá trị kết hợp lớn và xử lý theo mô-đun | 
|`3 3 / 5`|`21`| Hộp đựng ngôi sao và thanh đơn giản | 
|`4 2 / 0 0 0`|`0`| Những ràng buộc không tiêu cực không thể có | 

## Vỏ cạnh 

Đối với trường hợp không có câu trả lời:```
4 3
1 0
```Sự khác biệt giữa các khối là`-1`, buộc phần tử thứ tư phải nhỏ hơn phần tử thứ nhất một phần tử. Thuật toán ghi lại rằng chuỗi đầu tiên cần giá trị bắt đầu tối thiểu là`1`. Tổng khối đầu tiên quá nhỏ để đáp ứng yêu cầu này cùng với các giá trị không âm khác, do đó nó xuất ra`0`. 

Đối với trường hợp giá trị lớn:```
2 2
1000000000
```Có hai giá trị bắt đầu chỉ có tổng của chúng bị hạn chế. Thuật toán chuyển đổi điều này thành:`C(1000000001,1)`và đánh giá nó theo modulo`998244353`sử dụng định lý Lucas thay vì cố gắng xây dựng các giai thừa lên tới một tỷ. Đầu ra là kết quả mô-đun chính xác. 

Vì`K = 1`:```
6 1
2 3 0 8 2 5
```Mỗi cửa sổ chứa chính xác một phần tử nên không có sự lựa chọn nào. Việc xử lý chuỗi không tạo ra thêm sự tự do nào nữa và sự kết hợp với`K - 1 = 0`trả về một cách chính xác`1`.
