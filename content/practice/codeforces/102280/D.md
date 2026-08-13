---
title: "CF 102280D - \u0422\u0430\u0440\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u044f"
description: "Một tuyến đường có giá trị k. Nếu một hành khách muốn đi qua n điểm dừng thì giá vé là số thứ n k. Với k = 4, đây là các số vuông, với k = 3, chúng là các số hình tam giác và giá trị k lớn hơn mô tả các hình đa giác đều có nhiều cạnh hơn."
date: "2026-08-13T09:45:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102280
codeforces_index: "D"
codeforces_contest_name: "2010, \u0422\u0440\u0435\u043d\u0438\u0440\u043e\u0432\u043a\u0430 \u0421\u0413\u0410\u0423 aka \u041a\u043e\u043d\u0442\u0435\u0441\u0442 \u043f\u0440\u043e \u043c\u0430\u0440\u0448\u0440\u0443\u0442\u043a\u0438"
rating: 0
weight: 102280
solve_time_s: 367
verified: true
draft: false
---

[CF 102280D - \u0422\u0430\u0440\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u044f](https://codeforces.com/problemset/problem/102280/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 7 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một tuyến đường có một giá trị`k`. Nếu hành khách muốn đi qua`n`điểm dừng, giá vé là`n`-th`k`-số phương. Vì`k = 4`, đây là những số bình phương, vì`k = 3`chúng là các số tam giác và các giá trị lớn hơn của`k`mô tả các hình đa giác đều có nhiều cạnh hơn. 

Đầu vào chứa hai số nguyên,`n`Và`k`. Đây`n`là số điểm dừng mà hành khách muốn đi, và`k`xác định chuỗi số đa giác mà tuyến đường sử dụng. Chúng ta cần xuất ra`n`-th`k`-số phương. 

Công thức then chốt của`n`-số đa giác thứ là 

[ 
P_k(n)=\frac{(k-2)n^2-(k-4)n}{2}. 
] 

Những ràng buộc cho phép`n`để đạt được (10^8) và`k`để đạt được`100`. Đầu vào chỉ chứa một cặp giá trị, do đó giải pháp mong muốn không nên phụ thuộc vào`n`. Một vòng lặp chạy một lần cho mỗi điểm dừng sẽ yêu cầu tối đa (10^8) lần lặp, điều này gây tốn kém không cần thiết nếu dưới giới hạn 0,5 giây. Công thức đưa ra lời giải theo thời gian không đổi chỉ với một vài phép tính số học. 

Có hai trường hợp ranh giới rất dễ xử lý sai. Đầu tiên,`n = 0`phải tạo ra số không. Ví dụ, đầu vào`0 4`có đầu ra`0`. Một công thức hoặc cách thực hiện giả định đánh số đa giác bắt đầu từ`n = 1`và thực hiện một số khởi tạo đặc biệt có thể vô tình trả về`1`. 

Thứ hai, phép chia phải được thực hiện sau khi đã hình thành xong tử số. Ví dụ,`4 5`cho 

# \frac{48-4}{2} 

1. 

] 

Việc triển khai bất cẩn sử dụng công thức số tam giác không chính xác hoặc bỏ số hạng tuyến tính sẽ tạo ra giá trị sai. Trường hợp hình vuông cung cấp một kiểm tra hữu ích khác:`4 4`tạo ra (4^2=16). 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là xây dựng hình đa giác theo từng lớp. Số đa giác đầu tiên là`1`và mỗi lớp tiếp theo sẽ thêm một số điểm được xác định bởi`k`và lớp hiện tại. Điều này đúng vì số đa giác được xác định chính xác bằng cách cộng các lớp hình học liên tiếp. 

Vấn đề là một mô phỏng như vậy cần một lần lặp cho mỗi giá trị lên đến`n`. Với`n = 10^8`, trường hợp xấu nhất là theo thứ tự (10^8) lần lặp. Trong Python, điều này vượt xa giới hạn cuộc thi 0,5 giây có thể cho phép một cách hợp lý và ngay cả trong ngôn ngữ nhanh hơn, nó vẫn giải quyết được một vấn đề có biểu thức dạng đóng. 

Quan sát hữu ích là kích thước lớp tạo thành một cấp số cộng. Đối với một`k`-chuỗi hàm, mức tăng từ số hạng này sang số hạng tiếp theo là 

[ 
(k-2)n-(k-3). 
] 

Bắt đầu bằng (P_k(1)=1), việc tính tổng các số gia số học này sẽ cho một biểu thức bậc hai trong`n`. Sau khi đơn giản hóa tổng, chúng ta có được 

[ 
P_k(n)=\frac{(k-2)n^2-(k-4)n}{2}. 
] 

Phương pháp brute-force hoạt động vì nó thực hiện phép tính tổng này một cách rõ ràng, nhưng nó thất bại khi`n`là lớn. Dạng đóng thực hiện phép tính tổng tương tự về mặt đại số, giảm toàn bộ quá trình tính toán về thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`k`. Chúng xác định hoàn toàn số đa giác được yêu cầu nên không cần cấu trúc dữ liệu bổ sung. 
2. Tính tử số 

[ 
(k-2)n^2-(k-4)n. 
] 

Đây là dạng đóng tiêu chuẩn thu được bằng cách tính tổng cấp số cộng của các kích thước lớp. 
3. Chia tử số cho`2`và in kết quả. Số đa giác là số nguyên nên phép chia là chính xác. 

### Tại sao nó hoạt động 

các`k`-chuỗi giá trị bắt đầu bằng (P_k(1)=1) và các sai phân liên tiếp của nó tạo nên cấp số cộng 

[ 
k-2,\ 2(k-2)-(k-4),\ 3(k-2)-(k-4),\ldots 
] 

điều đó đơn giản hóa thành 

[ 
(k-2)n-(k-3). 
] 

Tổng hợp những khác biệt này từ số hạng đầu tiên đến`n`-th hạn cho 

# \frac{n((k-2)n-(k-4))}{2} 

\frac{(k-2)n^2-(k-4)n}{2}. 
] 

Thuật toán đánh giá chính xác định nghĩa toán học này, vì vậy giá trị được in là giá vé bắt buộc cho mọi giá trị hợp lệ.`n`Và`k`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())

answer = ((k - 2) * n * n - (k - 4) * n) // 2
print(answer)
```Dòng đầu tiên đọc cặp đầu vào duy nhất. Không có trường hợp kiểm thử nào phải lặp lại vì câu lệnh cung cấp chính xác một trường hợp kiểm thử`n`và một`k`. 

Biểu thức theo sau dạng đóng trực tiếp. Máy tính`n * n`trước khi nhân với`k - 2`giữ sự tương ứng với công thức rõ ràng. Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn ngay cả ở kích thước đầu vào tối đa. 

các`// 2`hoạt động an toàn vì tử số luôn chẵn đối với số nguyên`n`Và`k`. Việc sử dụng phép chia số nguyên cũng đảm bảo rằng kết quả đầu ra được in dưới dạng số nguyên chứ không phải dưới dạng giá trị dấu phẩy động. 

Vụ án`n = 0`không yêu cầu chi nhánh đặc biệt. Cả hai số hạng trong tử số đều chứa`n`, do đó biểu thức tự nhiên có giá trị bằng 0. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,`n = 4`Và`k = 4`. Một giá trị của`4`có nghĩa là số bình phương, vì vậy kết quả mong đợi là (4^2=16). 

| Bước |`n`|`k`|`(k - 2) * n * n`|`(k - 4) * n`| Tử số | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 4 | 32 | 0 | 32 | 16 | 

Công thức giảm xuống (2n^2/2=n^2) khi`k = 4`, xác nhận rằng công thức tổng quát phù hợp với bình phương thông thường. 

Đối với mẫu thứ hai,`n = 4`Và`k = 5`. Đây là những số ngũ giác. 

| Bước |`n`|`k`|`(k - 2) * n * n`|`(k - 4) * n`| Tử số | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 5 | 48 | 4 | 44 | 22 | 

Kết quả là`22`, số ngũ giác thứ tư. Phép tính tương tự cũng cho thấy tại sao số hạng hiệu chỉnh tuyến tính liên quan đến`k - 4`không thể bỏ qua. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số phép tính số học cố định được thực hiện. | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ`n`,`k`, và số nguyên thu được. | 

Tối đa`n`là (10^8), nhưng thuật toán không bao giờ lặp đến`n`. Nó thực hiện một số thao tác không đổi, do đó nó dễ dàng phù hợp với giới hạn thời gian 0,5 giây. Các số nguyên có độ chính xác tùy ý của Python cũng xử lý một cách an toàn kết quả lớn nhất có thể, tức là khoảng (4,9\cdot10^{17}) cho`n = 10^8`Và`k = 100`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    n, k = map(int, input().split())
    answer = ((k - 2) * n * n - (k - 4) * n) // 2
    print(answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("4 4\n") == "16\n", "sample 1"
assert run("4 5\n") == "22\n", "sample 2"

# Minimum n
assert run("0 3\n") == "0\n", "zero stops"

# Minimum polygon size, triangular numbers
assert run("1 3\n") == "1\n", "first triangular number"

# Square numbers, catches the linear-term handling
assert run("5 4\n") == "25\n", "fifth square number"

# Maximum values
assert run("100000000 100\n") == "489999995200000000\n", "maximum constraints"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 3`|`0`| tối thiểu`n`và không xử lý | 
|`1 3`|`1`| Số đa giác đầu tiên và ranh giới chỉ số | 
|`5 4`|`25`| Trường hợp đặc biệt của số vuông | 
|`100000000 100`|`489999995200000000`| Ràng buộc tối đa và số học số nguyên lớn | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là`n = 0`. Đối với đầu vào`0 4`, thuật toán tính toán 

# \frac{0}{2} 

1. 

] 

Không có điều kiện đặc biệt được yêu cầu, và đầu ra là`0`. Việc triển khai bắt đầu từ lớp hình học đầu tiên và giả sử có ít nhất một lớp tồn tại có thể trả về không chính xác`1`. 

Trường hợp cạnh thứ hai là giá trị hợp lệ đầu tiên,`n = 1`. Đối với đầu vào`1 3`, phép tính là 

# \frac{1+1}{2} 

1. 

] 

Điều này xác nhận rằng chuỗi được lập chỉ mục theo cách tiêu chuẩn, với số đa giác đầu tiên bằng`1`. Thay vào đó, việc triển khai từng bước một dựa trên số lượng lớp được thêm vào có thể vô tình tạo ra số hình tam giác thứ hai. 

Trường hợp hình vuông là một cách kiểm tra ranh giới hữu ích khác. Đối với đầu vào`5 4`, công thức trở thành 

# \frac{50}{2} 

1. 

] 

Thuật ngữ tuyến tính biến mất chính xác khi`k = 4`, để lại công thức bình phương quen thuộc. 

Cuối cùng, hãy xem xét đầu vào lớn nhất có thể,`100000000 100`. Thuật toán không thực hiện vòng lặp nào cả. Nó đánh giá 

489999995200000000. 
] 

Kết quả phù hợp thoải mái với biểu diễn số nguyên của Python và thời gian chạy vẫn không đổi mặc dù`n`lớn bằng (10^8).
