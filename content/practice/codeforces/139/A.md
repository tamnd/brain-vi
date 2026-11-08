---
title: "CF 139A - Petr và Sách"
description: "Petr có một cuốn sách có n trang. Bắt đầu từ thứ Hai, anh ấy đọc một số trang cố định mỗi ngày trong tuần. Đầu vào cung cấp bảy khả năng đọc hàng ngày theo thứ tự từ Thứ Hai đến Chủ Nhật. Chúng ta cần xác định xem Petr hoàn thành cuốn sách vào ngày nào."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 139
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 99 (Div. 2)"
rating: 1000
weight: 139
solve_time_s: 86
verified: true
draft: false
---

[CF 139A - Petr và Sách](https://codeforces.com/problemset/problem/139/A) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Petr có một cuốn sách với`n`trang. Bắt đầu từ thứ Hai, anh ấy đọc một số trang cố định mỗi ngày trong tuần. Đầu vào cung cấp bảy khả năng đọc hàng ngày theo thứ tự từ Thứ Hai đến Chủ Nhật. 

Chúng ta cần xác định xem Petr hoàn thành cuốn sách vào ngày nào. Câu trả lời không phải là tổng số ngày mà là số ngày trong tuần từ`1`ĐẾN`7`, Ở đâu`1`có nghĩa là thứ Hai và`7`có nghĩa là chủ nhật. 

Những hạn chế là rất nhỏ. Số trang nhiều nhất là`1000`và mỗi giá trị hàng ngày cũng tối đa`1000`. Ngay cả việc mô phỏng trực tiếp hàng ngày cũng hoàn toàn đủ nhanh. Trong trường hợp xấu nhất, Petr chỉ đọc một trang mỗi tuần, do đó mô phỏng có thể chạy trong khoảng`1000`ngày. Điều đó vẫn không đáng kể đối với giới hạn 2 giây. 

Phần khó khăn không phải là hiệu suất, mà là xử lý chính xác tình trạng dừng. 

Một lỗi phổ biến là kiểm tra xem các trang còn lại có chính xác bằng 0 chỉ sau khi hoàn thành cả tuần hay không. Hãy xem xét đầu vào này:```
1
1 0 0 0 0 0 0
```Câu trả lời đúng là`1`vì Petr sẽ kết thúc ngay vào thứ Hai. Việc triển khai bất cẩn lặp đi lặp lại hàng tuần có thể vô tình tiếp tục sang tuần tiếp theo. 

Một trường hợp tinh tế khác xảy ra khi một số ngày không có khả năng đọc.```
2
1 0 0 0 0 0 0
```Câu trả lời đúng vẫn là`1`. Petr đọc trang 1 vào thứ Hai đầu tiên, sau đó đọc trang 2 vào thứ Hai thứ hai. Những ngày không có trang nào vẫn phải được xử lý vì Petr không bỏ qua chúng. 

Một cái bẫy cuối cùng xuất hiện khi Petr kết thúc đúng vào cuối ngày.```
10
5 5 1 1 1 1 1
```Câu trả lời là`2`, không`3`. Sau thứ Ba, các trang còn lại trở thành số 0 nên thứ Ba là ngày kết thúc. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng quá trình đọc của Petr từng ngày một. Chúng tôi lặp đi lặp lại bảy ngày trong tuần, trừ đi số trang Petr có thể đọc trong ngày đó từ các trang còn lại. Ngay khi số còn lại trở thành số 0 hoặc âm, chúng ta sẽ trả về ngày trong tuần hiện tại. 

Điều này có tác dụng vì bản thân quá trình được mô tả trong bài toán là tuần tự. Petr đọc theo thứ tự thời gian và không bao giờ bỏ qua ngày, do đó việc mô phỏng chính xác những gì xảy ra một cách tự nhiên sẽ tạo ra kết quả chính xác. 

Vì cuốn sách có nhiều nhất`1000`các trang, ngay cả lịch đọc chậm nhất có thể vẫn kết thúc sau một số lần lặp có thể quản lý được. Ví dụ: nếu Petr chỉ đọc một trang vào thứ Hai hàng tuần và không đọc trang nào vào tất cả các ngày khác thì mô phỏng sẽ thực hiện khoảng`7000`kiểm tra hàng ngày. Đó là nhỏ bé. 

Ngoài ra còn có một quan sát rõ ràng hơn một chút. Chúng ta không cần phải đếm số tuần đầy đủ một cách rõ ràng. Chúng tôi chỉ quan tâm đến ngày đầu tiên mà số trang đọc tích lũy đạt hoặc vượt quá`n`. Điều đó có nghĩa là một lần duyệt lặp lại trong các ngày trong tuần là đủ. 

Các phương pháp tiếp cận bạo lực và tối ưu cuối cùng trông rất giống nhau vì các ràng buộc là nhỏ. Phiên bản tối ưu chỉ đơn giản là mô phỏng rõ ràng nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số ngày mô phỏng) | O(1) | Đã chấp nhận | 
| Tối ưu | O(số ngày mô phỏng) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tổng số trang`n`. 
2. Đọc mảng bảy số nguyên biểu thị số trang Petr đọc từ thứ Hai đến Chủ nhật. 
3. Bắt đầu một vòng lặp vô hạn vào các ngày trong tuần vì lịch trình lặp lại hàng tuần. 
4. Đối với mỗi ngày trong tuần từ chỉ mục`0`ĐẾN`6`, trừ khả năng đọc của ngày hôm đó khỏi`n`. 
5. Sau khi trừ đi, kiểm tra xem`n <= 0`. 
6. Nếu điều kiện đúng, trả về số ngày trong tuần hiện tại bằng cách sử dụng`index + 1`bởi vì vấn đề sử dụng đánh số dựa trên 1. 

Lý do điều này có hiệu quả là vì thời điểm này`n`trở nên không tích cực chính xác là thời điểm Petr đọc xong trang cuối cùng. Mỗi ngày trước đó vẫn còn một số trang chưa đọc. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến đơn giản: trước khi xử lý một ngày,`n`bằng số trang chưa đọc còn lại. Sau khi trừ đi số lượng đọc của ngày hiện tại,`n`trở thành những trang chưa đọc còn lại sau khi ngày đó kết thúc. 

Ngày đầu tiên ở đâu`n <= 0`chính xác là ngày mà số trang đọc tích lũy ít nhất đạt đến khổ sách gốc. Vì các ngày được xử lý theo thứ tự thời gian nên không có ngày nào sớm hơn có thể hoàn thành cuốn sách. Điều đó đảm bảo ngày trong tuần được trả về là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    days = list(map(int, input().split()))

    while True:
        for i in range(7):
            n -= days[i]

            if n <= 0:
                print(i + 1)
                return

solve()
```Chương trình trực tiếp tuân theo mô phỏng được mô tả trong thuật toán. 

các`while True`vòng lặp đại diện cho tuần lặp lại. Bên trong nó,`for`vòng lặp lặp lại từ thứ Hai đến Chủ nhật theo thứ tự. 

Sau khi trừ đi số lượng đã đọc của ngày hiện tại, chúng ta kiểm tra ngay xem cuốn sách đã đọc xong chưa. Thứ tự quan trọng. Nếu chúng tôi kiểm tra trước khi trừ, chúng tôi sẽ trì hoãn câu trả lời một ngày một cách không chính xác. 

Điều kiện sử dụng`n <= 0`thay vì`n == 0`. Petr có thể đọc nhiều trang hơn số trang còn lại trong cuốn sách vào ngày cuối cùng. Ví dụ, nếu`3`các trang vẫn còn và dung lượng ngày nay là`10`, giá trị còn lại trở thành`-7`, nhưng ngày kết thúc vẫn là hôm nay. 

Đầu ra sử dụng`i + 1`bởi vì mảng Python được lập chỉ mục 0 trong khi các ngày trong tuần trong bài toán được đánh số từ`1`ĐẾN`7`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
100
15 20 20 15 10 30 45
```| Ngày | Trang đã đọc | Các trang còn lại | 
| --- | --- | --- | 
| Thứ hai | 15 | 85 | 
| Thứ ba | 20 | 65 | 
| Thứ tư | 20 | 45 | 
| Thứ năm | 15 | 30 | 
| Thứ Sáu | 10 | 20 | 
| thứ bảy | 30 | -10 | 

Các trang còn lại trở nên không tích cực vào thứ Bảy, vì vậy câu trả lời là`6`. 

Dấu vết này cho thấy lý do tại sao thuật toán kiểm tra`n <= 0`ngay sau khi trừ. Petr có thể đọc xong trong nửa ngày. 

### Mẫu 2 

đầu vào:```
2
1 0 0 0 0 0 0
```| Ngày | Trang đã đọc | Các trang còn lại | 
| --- | --- | --- | 
| Thứ hai | 1 | 1 | 
| Thứ ba | 0 | 1 | 
| Thứ tư | 0 | 1 | 
| Thứ năm | 0 | 1 | 
| Thứ Sáu | 0 | 1 | 
| thứ bảy | 0 | 1 | 
| chủ nhật | 0 | 1 | 
| Thứ hai | 1 | 0 | 

Câu trả lời là`1`. 

Ví dụ này chứng minh rằng lịch trình lặp lại hàng tuần và những ngày không đọc sách vẫn được tính là ngày theo lịch. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) |`k`là số ngày mô phỏng cho đến khi hết sách | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Ngay cả trong trường hợp xấu nhất, mô phỏng chỉ thực hiện vài nghìn lần lặp, điều này không đáng kể trong giới hạn đã cho. Việc sử dụng bộ nhớ không đổi vì thuật toán chỉ lưu trữ bảy giá trị ngày trong tuần và một vài bộ đếm. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    days = list(map(int, input().split()))

    while True:
        for i in range(7):
            n -= days[i]

            if n <= 0:
                print(i + 1)
                return

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue()

# provided sample
assert run(
    "100\n15 20 20 15 10 30 45\n"
) == "6\n", "sample 1"

# custom cases
assert run(
    "1\n1 0 0 0 0 0 0\n"
) == "1\n", "minimum input"

assert run(
    "10\n5 5 1 1 1 1 1\n"
) == "2\n", "exact finish on Tuesday"

assert run(
    "7\n1 1 1 1 1 1 1\n"
) == "7\n", "all equal values"

assert run(
    "1000\n1000 0 0 0 0 0 0\n"
) == "1\n", "maximum pages finished immediately"

assert run(
    "2\n0 0 0 0 0 0 2\n"
) == "7\n", "finish on Sunday"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 0 0 0 0 0 0`|`1`| Đầu vào kích thước tối thiểu | 
|`10 / 5 5 1 1 1 1 1`|`2`| Kết thúc chính xác vào cuối ngày | 
|`7 / 1 1 1 1 1 1 1`|`7`| Lịch đọc thống nhất | 
|`1000 / 1000 0 0 0 0 0 0`|`1`| Giá trị lớn được xử lý chính xác | 
|`2 / 0 0 0 0 0 0 2`|`7`| Lập chỉ mục đúng ngày chủ nhật | 

## Vỏ cạnh 

Xét trường hợp Petr về đích ngay trong ngày đầu tiên. 

đầu vào:```
1
1 0 0 0 0 0 0
```Thuật toán trừ giá trị của ngày Thứ Hai từ`n`, làm`n = 0`. Từ`n <= 0`, nó lập tức quay trở lại`1`. Điều này tránh được sai lầm phổ biến là tiếp tục sang ngày hoặc tuần khác. 

Bây giờ hãy xem xét một lịch trình có nhiều ngày không đọc sách. 

đầu vào:```
2
1 0 0 0 0 0 0
```Sau ngày thứ Hai đầu tiên, vẫn còn một trang. Thuật toán vẫn xử lý từ Thứ Ba đến Chủ Nhật mặc dù chúng trừ 0. Vào thứ Hai tiếp theo, giá trị còn lại trở thành 0 và thuật toán trả về chính xác`1`. 

Cuối cùng, hãy xem xét trường hợp Petr đọc nhiều trang hơn số trang còn lại. 

đầu vào:```
3
10 0 0 0 0 0 0
```Sau thứ Hai, các trang còn lại sẽ trở thành`-7`. Thuật toán vẫn chấp nhận điều này vì nó kiểm tra`n <= 0`còn hơn là`n == 0`. Câu trả lời đúng là`1`vì Petr đã đọc xong trong buổi đọc sách hôm thứ Hai.
