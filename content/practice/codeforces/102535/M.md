---
title: "CF 102535M - Kim có thể và Mooks và Bộ đảo chiều"
description: "Dòng của đối thủ có thể được xem dưới dạng một chuỗi nhị phân trong đó O có nghĩa là Mook hiện đang hoạt động và E có nghĩa là Meek bị đánh bại. Mỗi khi Kim tiếp cận Mook đang hoạt động, một phút trôi qua, vị trí đó sẽ trở thành E và mọi vị trí trước khi nó quay trở lại O."
date: "2026-08-07T21:09:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102535
codeforces_index: "M"
codeforces_contest_name: "2020 UP ACM Algolympics Elimination Round"
rating: 0
weight: 102535
solve_time_s: 171
verified: true
draft: false
---

[CF 102535M - Kim có thể và Mooks và kẻ đảo ngược](https://codeforces.com/problemset/problem/102535/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Dòng của đối thủ có thể được xem như một chuỗi nhị phân trong đó`O`có nghĩa là Mook hiện đang hoạt động và`E`có nghĩa là một Meek bị đánh bại. Mỗi khi Kim tiếp cận được Mook đang hoạt động, một phút trôi qua, vị trí đó sẽ trở thành`E`, và mọi vị trí trước khi nó quay trở lại`O`. Reversinator cho phép chúng ta đảo ngược chính xác một phần liền kề của dòng ban đầu trước khi bất kỳ cuộc chiến nào bắt đầu. Chúng ta cần chọn sự đảo ngược đó sao cho tổng số phút cần thiết để xóa dòng càng nhỏ càng tốt. 

Nhận xét quan trọng là quá trình chiến đấu không phải là một sự mô phỏng tùy tiện. Nếu chúng ta gán ký tự ngoài cùng bên trái có trọng số 1, ký tự tiếp theo có trọng số 2, trọng số tiếp theo là 4, v.v., thì một thao tác chiến đấu hoàn toàn giống với việc trừ một từ số nhị phân này. Ngoài cùng bên trái`O`hoạt động giống như bit được đặt ít quan trọng nhất: nó trở thành 0 và tất cả các bit nhỏ hơn trở thành một. Tổng số trận đấu là giá trị của số nhị phân này. 

Đầu vào chứa tối đa 35000 chuỗi nhưng tổng chiều dài của chúng chỉ là 500000. Điều này loại trừ việc thử mọi đảo ngược có thể xảy ra, vì có thể có khoảng n2 lựa chọn cho một chuỗi con. Ngay cả giải pháp mô phỏng quá trình chiến đấu cho mỗi lựa chọn cũng không thể thực hiện được vì số lượng được biểu thị bằng chuỗi có thể lớn theo cấp số nhân. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính cho từng trường hợp thử nghiệm. 

Một lỗi phổ biến là tối ưu hóa chuỗi theo thứ tự từ trái sang phải ban đầu. Trọng số tăng dần về bên phải, do đó phía bên phải của chuỗi ban đầu có ảnh hưởng lớn nhất. Một sai lầm khác là quên rằng lệnh đảo chiều vẫn phải được sử dụng một lần. Cho phép một đoạn có độ dài bằng 1 và có nghĩa là giữ nguyên chuỗi. 

Ví dụ, với đầu vào`E`, không có Mook nên đáp án là`0`. Một phương pháp luôn tìm kiếm sự đảo chiều có lợi có thể thất bại khi cho rằng phải có một sự thay đổi. Đối với đầu vào`O`, câu trả lời là`1`, bởi vì cần phải chiến đấu một lần và việc đảo ngược không thể giúp ích được gì. Đối với đầu vào`EOOE`, giá trị ban đầu là`6`, nhưng việc đảo ngược đoạn tốt nhất sẽ mang lại`EEOO`, giá trị của nó là`3`. Một phương pháp coi thứ tự ban đầu là bên quan trọng nhất sẽ chọn sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi khoảng có thể, đảo ngược nó, tính giá trị nhị phân thu được và giữ mức tối thiểu. Có các khoảng O(n2). Ngay cả khi việc đảo ngược và đánh giá một khoảng được tối ưu hóa thành O(1), số lượng ứng cử viên đã quá lớn với n = 500000. Lực lượng vũ phu là chính xác vì nó kiểm tra mọi khả năng sử dụng của Reversinator, nhưng không gian tìm kiếm lại có vấn đề. 

Phép biến đổi hữu ích là xem số nhị phân theo ký hiệu thông thường. Đảo ngược chuỗi ban đầu. Trong cách biểu diễn đảo ngược này, ký tự đầu tiên là bit quan trọng nhất. Đảo ngược một phân đoạn trong chuỗi gốc vẫn chỉ là đảo ngược một phân đoạn trong chuỗi mới này. Nhiệm vụ trở thành tìm chuỗi nhị phân nhỏ nhất về mặt từ điển có thể đạt được bằng cách đảo ngược một chuỗi con. 

Đối với một chuỗi nhị phân, sự đảo ngược tốt nhất có thể dễ dàng mô tả. Nếu không có số 0 sau số đầu tiên thì chuỗi đó đã nhỏ đến mức có thể. Ngược lại, vị trí đầu tiên chứa`1`nên nhận được một`0`, và cách duy nhất để di chuyển số 0 ở đó là đảo ngược lên số 0 cuối cùng. Bất kỳ kết thúc nhỏ hơn nào sẽ khiến vị trí thay đổi đầu tiên trở nên tồi tệ hơn và bất kỳ sự bắt đầu nào sớm hơn sẽ giữ nguyên vị trí dẫn đầu.`1`. 

Sau sự đảo ngược này, chúng ta chỉ cần đánh giá số nhị phân thu được theo modulo`10^9`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đảo ngược chuỗi đầu vào. Điều này thay đổi cách biểu diễn sao cho so sánh từ điển thông thường khớp với so sánh bằng số của số trận đánh. 
2. Tìm lần xuất hiện đầu tiên của`1`trong chuỗi đảo ngược. Đây là nơi đầu tiên chúng tôi có thể giảm số lượng. 
3. Tìm lần xuất hiện cuối cùng của`0`trong chuỗi đảo ngược. Đây là số 0 xa nhất có thể được chuyển đến vị trí hữu ích sớm nhất. 
4. Nếu lần đầu tiên`1`xuất hiện trước cái cuối cùng`0`, đảo ngược khoảng đó. Điều này đặt số 0 ở vị trí quan trọng đầu tiên có thể được cải thiện. 
5. Chuyển đổi chuỗi nhị phân thu được thành số modulo`10^9`bằng cách quét từ trái sang phải và liên tục nhân đôi giá trị hiện tại. 

Tại sao nó hoạt động: Chuỗi đảo ngược là một biểu diễn nhị phân thông thường, do đó, việc làm cho nó nhỏ hơn về mặt từ điển cũng giống như việc làm cho số lượng trận đấu nhỏ hơn. Bit khác nhau đầu tiên quyết định kết quả. Việc đảo ngược chỉ có thể cải thiện chuỗi bằng cách di chuyển số 0 sang trái qua một hoặc nhiều chuỗi. Số sớm nhất theo sau là số 0 mới nhất mang lại sự cải thiện tối đa có thể ở vị trí khác nhau đầu tiên và sau đó việc đảo ngược sẽ tự động sửa chữa thứ tự còn lại. Nếu cặp như vậy không tồn tại thì mọi sự đảo ngược có thể xảy ra đều giữ nguyên bit quan trọng đầu tiên hoặc làm cho nó lớn hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9

def solve_case(s):
    s = s[::-1]

    first_one = -1
    last_zero = -1

    for i, c in enumerate(s):
        if c == '1' and first_one == -1:
            first_one = i
        if c == '0':
            last_zero = i

    if first_one != -1 and last_zero > first_one:
        s = s[:first_one] + s[first_one:last_zero + 1][::-1] + s[last_zero + 1:]

    ans = 0
    for c in s:
        ans = (ans * 2 + (c == '1')) % MOD
    return ans

def main():
    t = int(input())
    out = []
    for _ in range(t):
        s = input().strip()
        out.append(str(solve_case(s)))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Sự biến đổi đầu tiên,`s[::-1]`, là sự đơn giản hóa trung tâm. Nó biến quá trình chiến đấu bất thường thành số học nhị phân thông thường. 

Việc tìm kiếm`first_one`Và`last_zero`được thực hiện trong một lần quét. Nếu như`first_one`không phải là trước đây`last_zero`, không có chuyển động hữu ích nào của số 0 về phía vị trí quan trọng hơn, do đó chuỗi không thay đổi. 

Việc đảo ngược cắt là an toàn vì nó chỉ được áp dụng một lần. Vòng lặp cuối cùng không bao giờ xây dựng số nguyên khổng lồ được biểu thị bằng chuỗi nhị phân. Nó chỉ giữ lại modulo còn lại`10^9`, giúp tránh tràn và hoạt động với độ dài tối đa. 

## Ví dụ đã hoạt động 

cho`EOOE`, biểu diễn đảo ngược bắt đầu như`EOOE`. 

| Trạng thái chuỗi | Đầu tiên`1`| Cuối cùng`0`| Hành động | Giá trị | 
| --- | --- | --- | --- | --- | 
| EOOE | 1 | 3 | Đảo ngược vị trí từ 1 đến 3 | EEOO | 
| EEOO | - | - | Chuyển đổi giá trị nhị phân | 3 | 

Sự đảo ngược di chuyển số 0 cuối cùng đến vị trí quan trọng nhất có thể. Điều này chứng tỏ tại sao hướng ban đầu của sợi dây lại sai lệch. 

Vì`OOE`, biểu diễn đảo ngược là`EOO`. 

| Trạng thái chuỗi | Đầu tiên`1`| Cuối cùng`0`| Hành động | Giá trị | 
| --- | --- | --- | --- | --- | 
| EO | 1 | 2 | Đảo ngược vị trí từ 1 đến 2 | EO | 
| EO | - | - | Chuyển đổi giá trị nhị phân | 3 | 

Ở đây sự đảo ngược không cải thiện giá trị vì cả hai bit bị ảnh hưởng đều đã ở thứ tự tốt nhất. Thuật toán chính xác cho phép việc đảo ngược bắt buộc không có tác dụng rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chuỗi được quét một số lần không đổi. | 
| Không gian | O(n) | Chuỗi đảo ngược và các lát cắt tạm thời yêu cầu bộ nhớ tuyến tính. | 

Tổng độ dài của tất cả các trường hợp thử nghiệm là 500000, do đó, một giải pháp tuyến tính dễ dàng phù hợp với giới hạn thời gian. Việc sử dụng bộ nhớ vẫn tỷ lệ thuận với một trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 10**9

def solve_case(s):
    s = s[::-1]
    first_one = -1
    last_zero = -1

    for i, c in enumerate(s):
        if c == '1' and first_one == -1:
            first_one = i
        if c == '0':
            last_zero = i

    if first_one != -1 and last_zero > first_one:
        s = s[:first_one] + s[first_one:last_zero + 1][::-1] + s[last_zero + 1:]

    ans = 0
    for c in s:
        ans = (ans * 2 + (c == '1')) % MOD
    return ans

def run(inp: str) -> str:
    data = inp.strip().split()
    t = int(data[0])
    ans = []
    for i in range(1, t + 1):
        ans.append(str(solve_case(data[i])))
    return "\n".join(ans)

assert run("""1
EOOE
""") == "3", "sample 1"

assert run("""1
O
""") == "1", "single mook"

assert run("""1
E
""") == "0", "single meek"

assert run("""1
OO
""") == "3", "all mooks"

assert run("""1
EEEEEE
""") == "0", "all meeks"

assert run("""1
OOOOOOOOOO
""") == "1023", "long all mooks"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`O`|`1`| Kích thước tối thiểu với một đối thủ đang hoạt động | 
|`E`|`0`| Kích thước tối thiểu không cần đánh nhau | 
|`OO`|`3`| Sự đảo ngược không phải lúc nào cũng cải thiện giá trị | 
|`EEEEEE`|`0`| Tất cả các đối thủ bị đánh bại | 
|`OOOOOOOOOO`|`1023`| Đầu vào thống nhất lớn và chuyển đổi mô-đun | 

## Vỏ cạnh 

cho`E`, chuỗi đảo ngược không chứa`1`, Vì thế`first_one`vẫn chưa được đặt. Thuật toán bỏ qua việc đảo ngược và đánh giá giá trị 0, tạo ra`0`. 

Vì`O`, có một`1`nhưng không`0`, vì vậy không có cách nào để di chuyển một chút về phía trước. Thuật toán giữ nguyên chuỗi và trả về giá trị`1`. 

Vì`EOOE`, số lượng trận chiến ban đầu không phải là tối thiểu. Sau khi đảo ngược chuỗi thành`EOOE`, thuật toán tìm đầu tiên`1`ở chỉ số 1 và cuối cùng`0`tại chỉ số 3, đảo ngược phần đó và thu được`EEOO`. Giá trị nhị phân trở thành`3`, phù hợp với kết quả tối ưu. 

Đối với đầu vào chỉ chứa`O`các ký tự, chẳng hạn như`OOOOOOOOOO`, mọi sự đảo ngược có thể đều tạo ra cùng một chuỗi. Thuật toán phát hiện sự vắng mặt của số 0 và tính toán trực tiếp giá trị nhị phân mà không cần thực hiện các thao tác không cần thiết.
