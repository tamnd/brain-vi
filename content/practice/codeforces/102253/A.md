---
title: "CF 102253A - Thêm số không"
description: "Siêu máy tính có thể biểu thị mọi số nguyên từ 0 đến 2^m - 1. Tuy nhiên, cậu bé chỉ muốn làm việc với phạm vi từ 1 đến 10^k. Chúng ta cần k lớn nhất mà mọi số trong phạm vi thập phân đó đều có thể biểu diễn được."
date: "2026-08-17T21:22:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102253
codeforces_index: "A"
codeforces_contest_name: "2017 Chinese Multi-University Training, BeihangU Contest"
rating: 0
weight: 102253
solve_time_s: 72
verified: true
draft: false
---

[CF 102253A - Thêm số không khác](https://codeforces.com/problemset/problem/102253/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Siêu máy tính có thể biểu diễn mọi số nguyên từ`0`bởi vì`2^m - 1`. Tuy nhiên, chàng trai trẻ chỉ muốn làm việc với phạm vi từ`1`bởi vì`10^k`. Chúng tôi cần lớn nhất`k`mà mọi số trong phạm vi thập phân đó đều có thể biểu diễn được. 

Vì số lượng lớn nhất mà cậu bé sử dụng là`10^k`, toàn bộ phạm vi phù hợp chính xác khi`10^k <= 2^m - 1`. 

Vì vậy, mỗi trường hợp thử nghiệm cho`m`và đầu ra là số nguyên lớn nhất`k`thỏa mãn bất đẳng thức này. Số vụ việc cũng phải được in ra, bắt đầu từ`1`. Tuyên bố chính thức xác nhận những hạn chế lên tới khoảng`10^5`trường hợp thử nghiệm với`1 <= m <= 10^5`. 

Số lượng ca kiểm thử đủ lớn để một giải pháp thực hiện tỷ lệ thuận với`m`vì mọi trường hợp đều đã quá đắt rồi. Với`10^5`trường hợp và`m`lớn như`10^5`, điều đó có thể đạt tới khoảng`10^10`lần lặp lại. Chúng ta cần giảm từng trường hợp thử nghiệm về thời gian không đổi. 

Có hai chi tiết ranh giới thường gây ra sai sót. Đầu tiên, khi`m = 1`, máy chỉ đại diện cho`0`Và`1`, Vì thế`10^0 = 1`phù hợp nhưng`10^1 = 10`không. Câu trả lời là`0`, không`1`.```
Input:
1

Output:
Case #1: 0
```Việc thực hiện bất cẩn khi tính toán số chữ số thập phân của`2^m`có thể trở lại`1`, nhưng giá trị tối đa của máy là`2^m - 1`, không`2^m`. 

Ranh giới thứ hai là sự khác biệt rõ ràng giữa`2^m`Và`2^m - 1`. Vì`m = 10`, máy hỗ trợ các giá trị thông qua`1023`. Từ`1000 <= 1023`,`k = 3`là hợp lệ.```
Input:
10

Output:
Case #1: 3
```Một giải pháp sai lầm đòi hỏi`10^k < 2^m - 1`sẽ xảy ra để bác bỏ một số trường hợp ranh giới chính xác. Cách rõ ràng để giải quyết vấn đề này là sử dụng thực tế là cả hai vế đều là số nguyên và biến đổi điều kiện thành một bất đẳng thức nghiêm ngặt bao gồm`2^m`. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp có thể thử các giá trị của`k`bắt đầu từ số 0 và kiểm tra xem`10^k <= 2^m - 1`. Điều này đúng vì các giá trị hợp lệ của`k`tạo thành một phạm vi liên tiếp bắt đầu từ 0, vì vậy giá trị thất bại đầu tiên lớn hơn câu trả lời một đơn vị. Vì`m = 100000`, tuy nhiên, câu trả lời là xung quanh`30102`, vì vậy một trường hợp thử nghiệm yêu cầu khoảng`30103`kiểm tra ứng viên. Với`10^5`những trường hợp như vậy, đó là về`3,010,300,000`kiểm tra trước cả khi xem xét chi phí thao tác các số nguyên ngày càng lớn. Điều đó vượt xa những gì giới hạn một giây có thể hỗ trợ. 

Quan sát hữu ích là bất đẳng thức chứa lũy thừa của hai và mười, do đó logarit loại bỏ cả hai số mũ ngay lập tức. Bắt đầu từ`10^k <= 2^m - 1`, 

ta có, vì cả hai vế đều là số nguyên,`10^k < 2^m`. 

Lấy logarit cơ số 10 cho`k < m * log10(2)`. 

Số nguyên tối đa hoàn toàn nhỏ hơn`m * log10(2)`là giá trị sàn của nó, với điều kiện bản thân giá trị này không phải là số nguyên. Nó không thể là số nguyên dương`m`. Nếu như`m * log10(2) = r`, sau đó`2^m = 10^r`, Nhưng`10^r`chứa yếu tố`5`trong khi`2^m`không, đó là điều không thể. 

Do đó, câu trả lời có dạng cực kỳ đơn giản`answer = floor(m * log10(2))`. 

Điều này thay đổi vấn đề từ khả năng hàng chục nghìn lần lặp cho mỗi trường hợp thử nghiệm thành một phép nhân dấu phẩy động và một phép toán sàn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m) mỗi trường hợp | O(1) | Quá chậm | 
| Tối ưu | O(1) mỗi trường hợp | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số`m`cho trường hợp thử nghiệm hiện tại. Chúng tôi chỉ cần một giá trị này vì lũy thừa thập phân có thể sử dụng tối đa phụ thuộc trực tiếp vào độ rộng bit của máy. 
2. Tính toán`m * log10(2)`. Đây là ranh giới có giá trị thực thu được bằng cách lấy logarit cơ số 10 của`2^m`. 
3. Lấy sàn giá trị đó. Từ`m * log10(2)`không thể là số nguyên dương`m`, đây chính xác là số nguyên lớn nhất`k`thỏa mãn`k < m * log10(2)`. 
4. In kết quả bằng cách sử dụng`Case #x: y`định dạng. 

Tại sao nó hoạt động có thể được thể hiện dưới dạng một chuỗi các điều kiện tương đương. Một giá trị của`k`có giá trị chính xác khi`10^k <= 2^m - 1`. Vì cả hai đại lượng đều là số nguyên nên điều này tương đương với`10^k < 2^m`. Lấy logarit cho`k < m log10(2)`. Số nguyên lớn nhất thỏa mãn bất đẳng thức đó là`floor(m log10(2))`. Mã tính toán chính xác số lượng đó nên mọi câu trả lời được in ra đều tối đa và hợp lệ. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

LOG10_2 = math.log10(2.0)

def solve():
    t = int(input())
    out = []

    for case in range(1, t + 1):
        m = int(input())
        ans = int(m * LOG10_2)
        out.append(f"Case #{case}: {ans}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```

`LOG10_2`được tính toán một lần thay vì một lần cho mỗi trường hợp thử nghiệm. Sau đó, mỗi trường hợp thử nghiệm chỉ thực hiện một phép nhân và chuyển đổi số nguyên, đó là công thức thời gian không đổi từ thuật toán. 

Sự chuyển đổi`int(m * LOG10_2)`thực hiện mức sàn được yêu cầu ở đây vì giá trị là dương. của Python`int`cắt ngắn về 0, tương tự như lấy giá trị sàn cho số dương. 

các`2^m - 1`ranh giới cũng không yêu cầu xây dựng rõ ràng`2^m`hoặc`10^k`. Đạo hàm logarit đã xử lý ranh giới đó. Số nguyên Python cũng có độ chính xác tùy ý, nhưng việc tránh việc xây dựng số nguyên lớn giúp việc triển khai vừa đơn giản vừa nhanh hơn. 

Đối với ràng buộc đã cho`m <= 100000`, kết quả lớn nhất chỉ có khoảng`30102`. Độ chính xác kép có độ chính xác cao hơn nhiều so với mức cần thiết cho phạm vi này và`m * log10(2)`không phải là số nguyên cho bất kỳ số nguyên dương nào`m`, do đó không có ranh giới logarit số nguyên chính xác để gây nhầm lẫn cho hoạt động sàn. 

Mẫu chính thức có`m = 1`sản xuất`0`Và`m = 64`sản xuất`19`, phù hợp với công thức 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,`m = 1`. 

| m |`m * log10(2)`| sàn | trả lời | 
| --- | --- | --- | --- | 
| 1 | khoảng`0.3010`| 0 | 0 | 

Giá trị đại diện lớn nhất của máy là`2^1 - 1 = 1`. Bạn trẻ có thể sử dụng`1 = 10^0`, nhưng không thể sử dụng`10 = 10^1`, Vì thế`k = 0`là đúng. 

Đối với mẫu thứ hai,`m = 64`. 

| m |`m * log10(2)`| sàn | trả lời | 
| --- | --- | --- | --- | 
| 64 | khoảng`19.2659`| 19 | 19 | 

Ở đây máy hỗ trợ các giá trị thông qua`2^64 - 1`, lớn hơn`10^19`nhưng ít hơn`10^20`. Do đó toàn bộ phạm vi từ`1`bởi vì`10^19`phù hợp, trong khi phạm vi thông qua`10^20`không. Kết quả là`19`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm sử dụng một số phép tính số học không đổi. | 
| Không gian | O(t) | Các chuỗi đầu ra được lưu trữ trước lần ghi cuối cùng. | 

Với khoảng`10^5`trường hợp thử nghiệm, thuật toán chỉ thực hiện khoảng`10^5`tính toán liên tục theo thời gian. Điều này thoải mái trong giới hạn một giây và 128 MB đã nêu. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

LOG10_2 = math.log10(2.0)

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []

    for case in range(1, t + 1):
        m = int(input())
        ans = int(m * LOG10_2)
        out.append(f"Case #{case}: {ans}")

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run("2\n1\n64\n") == (
    "Case #1: 0\n"
    "Case #2: 19"
), "provided sample"

# Minimum input
assert run("1\n1\n") == "Case #1: 0", "m = 1"

# Boundary where 10^3 first becomes possible
assert run("2\n9\n10\n") == (
    "Case #1: 2\n"
    "Case #2: 3"
), "boundary around k = 3"

# Several equal values, checking case numbering as well
assert run("4\n10\n10\n10\n10\n") == (
    "Case #1: 3\n"
    "Case #2: 3\n"
    "Case #3: 3\n"
    "Case #4: 3"
), "repeated values"

# Maximum allowed m
assert run("1\n100000\n") == "Case #1: 30102", "maximum m"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n`|`Case #1: 0`| tối thiểu`m`và`2^m - 1`ranh giới | 
|`2\n9\n10\n`|`Case #1: 2`,`Case #2: 3`| Chuyển đổi từng bước một trong đó`1000`trở nên đại diện | 
|`4\n10\n10\n10\n`| Bốn trường hợp có câu trả lời`3`| Đầu vào lặp đi lặp lại và đánh số trường hợp chính xác | 
|`1\n100000\n`|`Case #1: 30102`| Ràng buộc tối đa và kết quả logarit lớn | 

## Vỏ cạnh 

cho`m = 1`, công thức cho`floor(log10(2)) = 0`. Thuật toán không bao giờ cố gắng xây dựng`10^1`, do đó nó xử lý được máy nhỏ nhất có thể một cách tự nhiên. Đầu vào chính xác là`1`, và đầu ra là`Case #1: 0`. 

Sự chuyển tiếp xung quanh`m = 10`nắm bắt được lỗi có thể xảy ra nhất. Vì`m = 9`, giá trị máy tối đa là`511`, Vì thế`100`phù hợp nhưng`1000`không, đưa ra`2`. Vì`m = 10`, tối đa là`1023`, Vì thế`1000`phù hợp và câu trả lời trở thành`3`. Các giá trị logarit xấp xỉ`2.7093`Và`3.0103`, sản xuất chính xác những tầng đó. 

Đối với các giá trị lặp lại, chẳng hạn như bốn trường hợp thử nghiệm đều chứa`10`, mọi câu trả lời đều phải`3`, nhưng số ca vẫn phải tăng từ`1`bởi vì`4`. Vì mỗi dòng đầu vào được xử lý độc lập và bộ đếm trường hợp được tăng lên sau mỗi lần lặp, nên việc triển khai sẽ bảo toàn cả hai thuộc tính. 

Để có đầu vào tối đa`m = 100000`, công thức cho`floor(100000 * log10(2)) = 30102`. Thuật toán không xây dựng`2^100000`, đây sẽ là một số nguyên lớn và không lặp qua ba mươi nghìn lũy thừa ứng cử viên của mười. Nó thực hiện phép tính theo thời gian không đổi giống như đối với`m = 1`, đó là lý do chính khiến giải pháp mở rộng theo phạm vi đầu vào đầy đủ.
