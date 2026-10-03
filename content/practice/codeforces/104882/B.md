---
title: "CF 104882B - Trước cuộc thi"
description: "Hai người chơi ấn định điểm số cho cùng một bài toán bằng hai công thức khác nhau. Một cái lấy độ dài của câu lệnh và nâng nó lên lũy thừa của độ dài mã, trong khi cái còn lại hoán đổi vai trò."
date: "2026-06-28T09:17:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104882
codeforces_index: "B"
codeforces_contest_name: "Voronezh State University - Sitronics contest II"
rating: 0
weight: 104882
solve_time_s: 51
verified: true
draft: false
---

[CF 104882B - Trước cuộc thi](https://codeforces.com/problemset/problem/104882/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hai người chơi ấn định điểm số cho cùng một bài toán bằng hai công thức khác nhau. Một cái lấy độ dài của câu lệnh và nâng nó lên lũy thừa của độ dài mã, trong khi cái còn lại hoán đổi vai trò. Với mỗi cặp số nguyên dương x và y, chúng ta phải quyết định xem x nâng lên y nhỏ hơn, bằng hay lớn hơn y nâng lên x. 

Đầu vào là một cặp số nguyên. Mỗi số nguyên có thể lớn tới một tỷ, điều này ngay lập tức loại trừ mọi phép tính lũy thừa trực tiếp. Ngay cả một phép tính công suất đơn giản cũng sẽ vượt quá phạm vi số nguyên tiêu chuẩn và mất thời gian theo cấp số nhân đối với số chữ số nếu được thực hiện thông qua phép nhân lặp lại. Điều này đẩy giải pháp tới việc so sánh các biểu thức mà không đánh giá chúng một cách rõ ràng. 

Một khó khăn tinh vi xuất hiện khi các giá trị nhỏ hoặc bằng nhau theo cách phá vỡ trực giác đơn điệu. Ví dụ: (2, 4) và (4, 2) tạo ra các giá trị bằng nhau mặc dù các cơ số khác nhau, vì cả hai đều có giá trị bằng 16. Một trường hợp khác là khi một trong các số bằng 1. Nếu x bằng 1 và y lớn thì 1^y luôn bằng 1, trong khi y^1 bằng y, do đó việc so sánh được xác định hoàn toàn bằng việc liệu y có vượt quá 1 hay không. Về mặt đối xứng, nếu y bằng 1, phép so sánh sẽ đảo ngược. 

Những trường hợp đặc biệt này quan trọng vì bất kỳ kỹ thuật xấp xỉ chung nào như logarit đều phải nhất quán với các so sánh chính xác trong các tình huống biên này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ tính toán x^y và y^x một cách rõ ràng và so sánh kết quả. Điều này đúng về mặt khái niệm vì nó khớp chính xác với định nghĩa của vấn đề. Vấn đề là ngay cả việc biểu thị x^y cũng không thể thực hiện được đối với đầu vào lớn, vì kết quả tăng vượt xa giới hạn 64-bit gần như ngay lập tức và việc tính toán nó yêu cầu phép nhân y. 

Đối với trường hợp xấu nhất khi x và y đều lớn, điều này dẫn đến phép nhân xấp xỉ O(y + x), điều này hoàn toàn không khả thi khi mỗi phép nhân có thể lên tới 10^9. 

Quan sát quan trọng là chúng ta không cần các giá trị chính xác mà chỉ cần thứ tự của chúng. Việc so sánh độ lớn của các biểu thức hàm mũ có thể được giảm bớt bằng cách áp dụng phép biến đổi đơn điệu. Vì logarit tăng nghiêm ngặt nên so sánh x^y và y^x tương đương với so sánh y * log(x) và x * log(y). Điều này loại bỏ hoàn toàn phép lũy thừa và thay thế nó bằng số học theo thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Tuy nhiên, phép biến đổi này giả định cả hai số đều lớn hơn 1. Khi một trong hai giá trị bằng 1, logarit hoạt động kém về mặt số học và biểu thức gốc được đơn giản hóa một cách trực tiếp, do đó những trường hợp đó phải được xử lý riêng. Sự kết hợp giữa suy luận trực tiếp cho các giá trị cạnh nhỏ và so sánh logarit cho trường hợp tổng quát mang lại một giải pháp hoàn chỉnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(x + y) | O(1) | Quá chậm | 
| So sánh logarit | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Ý tưởng là giảm sự so sánh với thứ gì đó không tăng theo x hoặc y. 

1. Đọc hai số nguyên x và y. Chúng xác định hai biểu thức hàm mũ mà chúng ta không bao giờ tính toán rõ ràng. 
2. Nếu x bằng y, trả về ngay đẳng thức. Cả hai biểu thức đều trở nên giống hệt nhau vì việc hoán đổi cơ số và số mũ không thay đổi bất cứ điều gì khi các giá trị giống nhau. 
3. Nếu x bằng 1 thì x^y luôn bằng 1 bất kể y. Việc so sánh quy về việc kiểm tra xem y có lớn hơn 1 hay không, vì y^1 bằng y. 
4. Nếu y bằng 1 thì tình huống là đối xứng. y^x bằng 1, trong khi x^y bằng x, do đó kết quả phụ thuộc vào việc x có vượt quá 1 hay không. 
5. Nếu cả hai giá trị đều không bằng 1, hãy so sánh đại lượng y * log(x) và x * log(y). Tích lớn hơn tương ứng với biểu thức hàm mũ ban đầu lớn hơn vì logarit bảo toàn thứ tự theo lũy thừa. 
6. Trả về kết quả so sánh xem bên nào lớn hơn.

Ý tưởng quan trọng là chúng ta chuyển đổi việc so sánh các số cực lớn thành so sánh hai biểu thức có giá trị thực vẫn ổn định theo số học dấu phẩy động tiêu chuẩn. 

### Tại sao nó hoạt động 

Phép biến đổi phụ thuộc vào tính đơn điệu của hàm logarit. Vì nhật ký tăng nghiêm ngặt đối với các đối số tích cực nên việc áp dụng nó sẽ duy trì thứ tự. Do đó, x^y và y^x có thể được so sánh thông qua logarit của chúng: log(x^y) = y log x và log(y^x) = x log y. Sự tương đương này đúng trong số học thực và trường hợp duy nhất mà tính không ổn định về số có thể quan trọng là khi x hoặc y bằng 1, trong đó nhật ký tiến gần đến 0 và các biểu thức thu gọn thành các phép so sánh số nguyên tầm thường được xử lý riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import math

def solve():
    x, y = map(int, input().split())

    if x == y:
        print("=")
        return

    if x == 1:
        print("<" if y > 1 else "=")
        return

    if y == 1:
        print(">" if x > 1 else "=")
        return

    left = y * math.log(x)
    right = x * math.log(y)

    if abs(left - right) < 1e-12:
        print("=")
    elif left > right:
        print(">")
    else:
        print("<")

if __name__ == "__main__":
    solve()
```Cấu trúc của mã phản ánh sự phân rã hợp lý của vấn đề. Việc kiểm tra đẳng thức ngay từ đầu sẽ tránh được các thao tác dấu phẩy động không cần thiết. Hai nhánh cho x bằng 1 hoặc y bằng 1 loại bỏ các trường hợp logarit suy biến trong đó so sánh số sẽ không ổn định. Phép so sánh cuối cùng sử dụng logarit tự nhiên vì mọi cơ số nhất quán đều hoạt động và cấu trúc nhân sẽ bị loại bỏ. 

Ngưỡng dung sai xử lý lỗi làm tròn dấu phẩy động, vì các giá trị như 2^4 và 4^2 phải khớp chính xác nhưng có thể khác nhau một chút epsilon khi tính toán thông qua nhật ký. 

## Ví dụ đã hoạt động 

Đầu tiên hãy xem xét dữ liệu đầu vào 3 5. Chúng ta tính 3^5 so với 5^3. 

| Bước | trái = y log x | đúng = x log y | Quyết định | 
| --- | --- | --- | --- | 
| Ban đầu | 5 nhật ký 3 | 3 nhật ký 5 | So sánh | 
| Đánh giá | khoảng 5,493 | khoảng 4,828 | trái > phải | 

Vì phía bên trái lớn hơn nên đầu ra lớn hơn, nghĩa là 3^5 vượt quá 5^3. Điều này phù hợp với tính toán trực tiếp vì 243 lớn hơn 125. 

Bây giờ hãy xem xét 7 7. 

| Bước | x | y | Quyết định | 
| --- | --- | --- | --- | 
| Kiểm tra sự bình đẳng | 7 | 7 | Bằng nhau ngay lập tức | 

Không cần tính toán thêm vì cả hai biểu thức đều giống hệt nhau. 

Trường hợp thứ hai này xác nhận rằng việc chấm dứt sớm sẽ tránh được công việc có dấu phẩy động không cần thiết và duy trì tính chính xác chính xác khi đầu vào khớp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ thực hiện số học theo thời gian không đổi và một số lượng đánh giá logarit cố định | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Việc tính toán dễ dàng phù hợp trong giới hạn vì mỗi trường hợp thử nghiệm chỉ thực hiện một số thao tác dấu phẩy động. Ngay cả với nhiều đầu vào, cách tiếp cận này vẫn duy trì thời gian không đổi cho mỗi trường hợp và tránh mọi sự tăng trưởng đối với x hoặc y. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    x, y = map(int, sys.stdin.readline().split())

    if x == y:
        return "="

    if x == 1:
        return "<" if y > 1 else "="

    if y == 1:
        return ">" if x > 1 else "="

    left = y * math.log(x)
    right = x * math.log(y)

    if abs(left - right) < 1e-12:
        return "="
    elif left > right:
        return ">"
    else:
        return "<"

assert run("3 5") == ">", "sample 1"
assert run("7 7") == "=", "sample 2"

assert run("1 10") == "<", "x is 1"
assert run("10 1") == ">", "y is 1"
assert run("2 4") == "=", "classic equality case"
assert run("5 2") == ">", "reverse comparison"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 10 | < | đế là ốp 1 cạnh | 
| 10 1 | > | trường hợp cạnh đối xứng | 
| 2 4 | = | trường hợp bình đẳng không tầm thường | 
| 5 2 | > | đặt hàng chung đúng đắn | 

## Vỏ cạnh 

Khi x bằng 1 và y lớn hơn, thuật toán ngay lập tức trả về ít hơn. Đối với đầu vào 1 10, quá trình kiểm tra sẽ kích hoạt ở bước thứ ba và đưa ra kết quả là 1^10 nhỏ hơn 10^1. Nhánh logarit không bao giờ được nhập, điều này tránh tính toán nhật ký (1) và ngăn chặn các so sánh dấu phẩy động không cần thiết. 

Khi y bằng 1 và x lớn hơn, chẳng hạn như 10 1, nhánh đối xứng đảm bảo thứ tự chính xác mà không cần gọi logarit. Điều này ngăn chặn sự phụ thuộc không chính xác vào nhật ký (1), sẽ bằng 0 và làm sai lệch so sánh. 

Khi cả x và y đều nhỏ nhưng được sắp xếp không tầm thường, chẳng hạn như 2 4, phép so sánh logarit nắm bắt chính xác đẳng thức vì cả hai biểu thức đều có giá trị bằng 16. Các giá trị được tính toán của y log x và x log y khớp nhau và ngưỡng epsilon phân loại chúng bằng nhau, duy trì tính chính xác khi làm tròn dấu phẩy động.
