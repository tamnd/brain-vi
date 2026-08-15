---
title: "CF 102439F - Số nguyên tố hoặc số"
description: "Chúng ta được cho một số nguyên không âm n, với 1 <= n <= 10^18. Thay vì phép nhân thông thường, chúng ta được yêu cầu sử dụng bitwise OR làm phép toán kết hợp hai số."
date: "2026-08-12T08:12:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102439
codeforces_index: "F"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Semifinal"
rating: 0
weight: 102439
solve_time_s: 66
verified: true
draft: false
---

[CF 102439F - Số nguyên tố hoặc số](https://codeforces.com/problemset/problem/102439/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên không âm`n`, với`1 <= n <= 10^18`. Thay vì phép nhân thông thường, chúng ta được yêu cầu sử dụng bitwise OR làm phép toán kết hợp hai số. 

số`n`được coi là số nguyên tố trong hoạt động này khi`n > 1`và không tồn tại số nguyên`x`Và`y`thỏa mãn`1 < x <= y < n`Và```
x OR y = n
```Nhiệm vụ là in`Yes`khi`n`có tài sản chính đặc biệt này và`No`nếu không thì. 

Giới hạn trên của`10^18`ngay lập tức loại trừ bất cứ điều gì lặp qua tất cả các giá trị có thể lên đến`n`. Thậm chí một`O(n)`giải pháp sẽ yêu cầu lên tới`10^18`lặp đi lặp lại, vượt xa giới hạn một giây. Tìm kiếm brute-force trên các cặp gần như là bậc hai, xung quanh`5 * 10^35`cặp trong trường hợp xấu nhất. Cấu trúc hữu ích phải đến từ biểu diễn nhị phân của`n`, chỉ chứa khoảng 60 bit. 

Có ba trường hợp đáng được quan tâm. Đầu tiên,`n = 1`phải sản xuất`No`, bởi vì định nghĩa yêu cầu một ứng cử viên chính phải lớn hơn`1`; không có số nguyên tố hợp lệ theo định nghĩa này tại`1`. Thứ hai, sức mạnh của hai như`8`sản xuất`Yes`. Việc thực hiện bất cẩn có thể tìm kiếm một ước số thông thường và kết luận điều gì đó dựa trên tính nguyên tố thông thường, nhưng phép nhân thông thường không liên quan gì đến phép toán ở đây. Cuối cùng, một số có chính xác hai bit được đặt, chẳng hạn như`6 = 110₂`, tạo ra`No`: chúng ta có thể chọn`x = 2`Và`y = 4`, Và`2 OR 4 = 6`, với cả hai toán hạng nằm giữa`1`Và`6`. 

Do đó, sự khác biệt quan trọng không phải là liệu`n`là một số nguyên tố thông thường. Đó là bao nhiêu`1`các bit xảy ra ở dạng biểu diễn nhị phân của nó. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp tuân theo định nghĩa theo nghĩa đen. Chúng ta có thể liệt kê mọi`x`từ`2`bởi vì`n - 1`, mọi`y`từ`x`bởi vì`n - 1`, và kiểm tra xem`x OR y`bằng`n`. Nếu có một cặp như vậy tồn tại thì câu trả lời là`No`; nếu mọi cặp đều thất bại, câu trả lời là`Yes`. Điều này đúng vì nó kiểm tra chính xác các cặp bị cấm theo định nghĩa. 

Vấn đề là kích thước của không gian tìm kiếm. Đối với một giá trị gần`10^18`, có khoảng```
(n - 2)(n - 1) / 2
```các cặp ứng cử viên Tại`n = 10^18`, đây là khoảng`5 * 10^35`cặp, điều này hoàn toàn không thể thực hiện được. 

Quan sát mở ra vấn đề xuất phát trực tiếp từ hành vi của bitwise OR. Mọi`1`một chút`x`hoặc`y`cũng phải là một`1`một chút`n`khi`x OR y = n`. Ngược lại, mỗi`1`một chút`n`phải xảy ra ở ít nhất một trong các`x`hoặc`y`. 

Giả định`n`có ít nhất hai bit được đặt. Chọn một bộ bit và đặt nó vào`x`, trong khi đặt tất cả các bit còn lại vào`y`. Cả hai số đều là mặt nạ con thích hợp của`n`, vì vậy cả hai đều nhỏ hơn`n`và OR của họ chính xác là`n`. 

Ví dụ,```
n = 10 = 1010₂

x = 2  = 0010₂
y = 8  = 1000₂

x OR y = 1010₂ = 10
```Cả hai toán hạng đều lớn hơn`1`, Vì thế`10`không phải là số nguyên tố trong phép toán OR. 

Ngoại lệ duy nhất là khi`n`có chính xác một bit được đặt. Sau đó`n`là sức mạnh của hai. Mọi số dương có bit chứa trong`n`là một trong hai`0`hoặc`n`chính nó. Không có toán hạng hợp lệ nào nằm giữa`1`Và`n`, nên không có cặp nào có thể thỏa mãn định nghĩa. Như vậy mọi lũy thừa của hai lớn hơn`1`là số nguyên tố dưới OR. 

Chúng tôi đã giảm toàn bộ vấn đề để kiểm tra xem`n`là lũy thừa của hai, với yêu cầu bổ sung là`n > 1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`và trước tiên hãy kiểm tra xem`n <= 1`. Giá trị như vậy không thể thỏa mãn định nghĩa về một ứng cử viên chính, vì vậy hãy in`No`. 
2. Kiểm tra xem`n`có chính xác một bit được đặt. Kiểm tra bitwise tiêu chuẩn cho việc này là```
n & (n - 1) == 0
```Đối với một số nguyên dương có đúng một`1`bit, trừ đi một thay đổi bit đó thành`0`và thay đổi mọi bit thấp hơn thành`1`. AND của họ do đó bằng không. Nếu có ít nhất hai bit được đặt, AND vẫn khác 0. 

1. In`Yes`nếu thử nghiệm thành công và`No`nếu không thì. Một thử nghiệm thành công có nghĩa là`n`là lũy thừa của hai lớn hơn`1`, chính xác là tập hợp các số nguyên tố OR. 

### Tại sao nó hoạt động 

Điều bất biến là mọi phân tách hợp lệ`x OR y = n`phải phân phối các bit đã đặt của`n`giữa hai toán hạng. Nếu như`n`có ít nhất hai bit set, người ta luôn có thể xây dựng một phân tách như vậy bằng cách đặt một bit set vào`x`và tất cả các bit được đặt khác vào`y`. Cả hai toán hạng đều lớn hơn`1`và nhỏ hơn`n`, Vì thế`n`là hợp số theo OR. 

Nếu như`n`có chính xác một bit được đặt, mọi mặt nạ con của`n`là một trong hai`0`hoặc`n`. Không có số nguyên nào nằm giữa`1`Và`n`có thể được sử dụng làm toán hạng, do đó không tồn tại sự phân tách bị cấm. Do đó, trong số các giá trị lớn hơn`1`, các số nguyên tố OR chính xác là lũy thừa của hai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    if n > 1 and (n & (n - 1)) == 0:
        print("Yes")
    else:
        print("No")

solve()
```Điều kiện đầu tiên,`n > 1`, xử lý ranh giới theo yêu cầu của định nghĩa. Không có nó,`n = 1`sẽ vượt qua bài kiểm tra lũy thừa hai một cách không chính xác vì`1 & 0`là số không. 

Điều kiện thứ hai kiểm tra xem có chính xác một bit được đặt hay không. Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn mặc dù đầu vào có thể lớn bằng`10^18`. 

Biểu thức sử dụng`n - 1`trước thao tác AND. Ví dụ,`8`là`1000₂`, trong khi`7`là`0111₂`, Vì thế`8 & 7`là số không. Vì`10 = 1010₂`,`9 = 1001₂`, Và`10 & 9 = 1000₂`, khác không. 

Không lặp lại giá trị của`n`là cần thiết. Toàn bộ quyết định bao gồm một số lượng không đổi các phép toán số nguyên. 

## Ví dụ đã hoạt động 

Vì định dạng câu lệnh được cung cấp ở đây bỏ qua các giá trị số từ đầu vào mẫu được hiển thị nên chúng ta có thể theo dõi các giá trị đại diện tương ứng với hai kết quả mẫu. 

Vì`n = 8`, số đó là`1000₂`. 

|`n`|`n > 1`|`n - 1`|`n & (n - 1)`| Đầu ra | 
| --- | --- | --- | --- | --- | 
| 8 | đúng | 7 (`0111₂`) | 0 | Có | 

Kết quả bằng 0 xác nhận rằng`8`có chính xác một bit được đặt. Không có toán hạng hợp lệ nào nằm giữa`1`Và`8`bit của nó được chứa trong`8`, Vì thế`8`không thể được biểu diễn dưới dạng OR của hai toán hạng nhỏ hơn được phép. 

Vì`n = 6`, biểu diễn nhị phân là`110₂`. 

|`n`|`n > 1`|`n - 1`|`n & (n - 1)`| Đầu ra | 
| --- | --- | --- | --- | --- | 
| 6 | đúng | 5 (`101₂`) | 4 (`100₂`) | Không | 

Kết quả khác 0 có nghĩa là`6`có nhiều hơn một bit được đặt. Thực vậy,`2 OR 4 = 6`, và cả hai`2`Và`4`thỏa mãn`1 < x <= y < 6`. Sự phân hủy cần thiết tồn tại, vì vậy`6`không phải là số nguyên tố trong OR. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Các hoạt động bitwise hoạt động trên khoảng`log₂ n`bit của`n`. | 
| Không gian | O(1) | Chỉ có một số lượng biến số nguyên không đổi được lưu trữ. | 

Với`n <= 10^18`, biểu diễn nhị phân có tối đa 60 bit. Do đó, giải pháp chỉ thực hiện một lượng công việc không đổi trên một biểu diễn số nguyên rất nhỏ, dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    n = int(input())

    if n > 1 and (n & (n - 1)) == 0:
        print("Yes")
    else:
        print("No")

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue() if False else ""
    finally:
        sys.stdin = old_stdin
        input = old_input
```Đối với khai thác kiểm tra dựa trên xác nhận có thể thực thi trực tiếp, sẽ rõ ràng hơn nếu nắm bắt rõ ràng đầu ra tiêu chuẩn:```python
import sys
import io

def solve():
    n = int(input())

    if n > 1 and (n & (n - 1)) == 0:
        print("Yes")
    else:
        print("No")

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

# Representative sample 1
assert run("8\n") == "Yes\n", "power of two"

# Representative sample 2
assert run("6\n") == "No\n", "two set bits"

# Minimum-size input
assert run("1\n") == "No\n", "1 is not greater than 1"

# Smallest valid OR-prime
assert run("2\n") == "Yes\n", "2 is a power of two"

# Maximum input
assert run("1000000000000000000\n") == "No\n", "maximum bound"

# Power of two near the maximum
assert run("576460752303423488\n") == "Yes\n", "2^59"

# Boundary between one and two set bits
assert run("3\n") == "No\n", "1 OR 2 = 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`No`| Đầu vào tối thiểu và`n > 1`ranh giới | 
|`2`|`Yes`| Số nguyên tố OR hợp lệ nhỏ nhất | 
|`3`|`No`| Số nhỏ nhất có hai bit được đặt | 
|`576460752303423488`|`Yes`| Sức mạnh lớn của hai, kiểm tra dải trên | 
|`1000000000000000000`|`No`| Đầu vào tối đa được phép | 
|`6`|`No`| Phân rã rõ ràng thành`2 OR 4`| 

Dây nịt đầu tiên được trình bày ở trên minh họa mục đích dự định`run`cấu trúc nhưng không nắm bắt được đầu ra, vì vậy khai thác thứ hai là khai thác được sử dụng khi thực sự chạy các xác nhận. Bản thân việc gửi chương trình cạnh tranh chỉ cần giải pháp ngắn hơn nhiều so với phần trước. 

## Vỏ cạnh 

cho`n = 1`, đầu vào là`1`và thuật toán đầu tiên đánh giá`n > 1`, điều đó là sai. Nó in ngay`No`. Điều này ngăn cản sự biểu hiện`n & (n - 1)`từ việc phân loại sai`1`là số nguyên tố hợp lệ trong OR đơn giản vì`1`có một bit được đặt. 

Vì`n = 2`, biểu diễn nhị phân là`10₂`. biểu hiện`2 & 1`bằng 0 và`2 > 1`, do đó thuật toán in`Yes`. Không có số nguyên`x`với`1 < x < 2`, điều này làm cho việc phân hủy không thể thực hiện được. 

Vì`n = 6`, biểu diễn nhị phân là`110₂`. biểu hiện`6 & 5`cho`4`, do đó số có nhiều bit được đặt và thuật toán sẽ in`No`. Nhân chứng thực sự là`x = 2`,`y = 4`, từ`2 OR 4 = 6`. 

Đối với một sức mạnh lớn của hai như`576460752303423488 = 2^59`, chính xác một bit được thiết lập. Trừ một sẽ tạo ra một số có tập hợp 59 bit thấp hơn, do đó AND của chúng bằng 0. Thuật toán in`Yes`, chứng tỏ rằng phương pháp không phụ thuộc vào các giá trị nhỏ. 

Vì`n = 10^18`, biểu diễn nhị phân chứa nhiều bit được đặt, vì vậy`n & (n - 1)`là khác không và câu trả lời là`No`. Trường hợp này cũng chứng minh tại sao cách tiếp cận dựa trên việc liệt kê các toán hạng có thể không thể hoạt động: ngay cả quét tuyến tính cũng sẽ yêu cầu số lượng thao tác không khả thi, trong khi đặc tính bitwise xử lý giá trị ngay lập tức.
