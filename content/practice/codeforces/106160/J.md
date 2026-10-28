---
title: "CF 106160J - Số Jacobi"
description: "Nhiệm vụ yêu cầu chúng ta biểu diễn một số nguyên dương n dưới dạng tổng các lập phương. Chúng ta phải in một danh sách các số nguyên, mỗi số nằm trong khoảng từ -10000 đến 10000, có tổng các lập phương của chúng chính xác bằng n. Mọi phân tách hợp lệ đều được chấp nhận và số lượng thuật ngữ được in không thể vượt quá 10000."
date: "2026-06-25T11:13:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106160
codeforces_index: "J"
codeforces_contest_name: "2025 Benelux Algorithm Programming Contest (BAPC 25)"
rating: 0
weight: 106160
solve_time_s: 29
verified: true
draft: false
---

[CF 106160J - Số Jacobi](https://codeforces.com/problemset/problem/106160/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ yêu cầu chúng ta thể hiện một số nguyên dương`n`dưới dạng tổng các hình khối. Chúng ta phải in một danh sách các số nguyên, mỗi số nằm giữa`-10000`Và`10000`, các khối của chúng cộng lại chính xác bằng`n`. Mọi phân tách hợp lệ đều được chấp nhận và số lượng thuật ngữ được in không thể vượt quá`10000`. 

Đầu vào là một số nguyên duy nhất biểu thị giá trị đích. Đầu ra trước tiên cho biết có bao nhiêu số nguyên trong quá trình phân tách, theo sau là chính các số nguyên đó. 

Sự hạn chế đó`n`nhiều nhất là`9241`là chi tiết quan trọng. Một cách xây dựng trực tiếp có thể sử dụng thực tế là`1³ = 1`. Nếu chúng ta in`n`bản sao của số`1`, tổng các khối của chúng bằng`n`. Vì lớn nhất có thể`n`là`9241`, điều này tạo ra nhiều nhất`9241`các điều khoản, ở mức an toàn dưới giới hạn của`10000`. 

Điều này ngay lập tức loại trừ nhu cầu tìm kiếm lý thuyết số, lập trình động hoặc tính toán trước. Bất kỳ cách tiếp cận nào cố gắng tìm cách trình bày ngắn hơn đều giải quyết được một vấn đề khó hơn vấn đề được yêu cầu. 

Các trường hợp cạnh chính liên quan đến kích thước đầu ra và xử lý giá trị nhỏ nhất có thể. Vì`n = 1`, đầu ra phải chứa một thuật ngữ:```
Input
1

Output
1
1
```Việc triển khai bất cẩn có thể cố gắng khởi tạo một câu trả lời trống và quên thêm bất cứ thứ gì có giá trị nhỏ nhất. 

Đối với giá trị tối đa cho phép:```
Input
9241

Output
9241
1 1 1 ... 1
```với`9241`bản sao của`1`, câu trả lời vẫn hợp lệ. Một giải pháp giả định số lượng số hạng nhỏ có thể vô tình vi phạm giới hạn đầu ra nếu nó cố gắng tạo ra nhiều giá trị không cần thiết. 

Một lỗi phổ biến khác là in các hình khối thay vì số nguyên ban đầu. Ví dụ, đối với`n = 3`, kết quả đúng là:```
Input
3

Output
3
1 1 1
```bởi vì tổng được kiểm tra là`1³ + 1³ + 1³`, không`1 + 1 + 1`dưới dạng các giá trị đơn giản. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo sẽ là tìm kiếm sự kết hợp của các số mà các khối của nó cộng với`n`. Người ta có thể thử mọi giá trị khối có thể, sử dụng đệ quy hoặc xây dựng giải pháp lập trình động trên các tổng có thể. Những cách tiếp cận này đúng vì chúng khám phá các phân rã hợp lệ, nhưng chúng hoàn toàn bỏ qua giới hạn đầu ra rộng rãi. Không gian tìm kiếm rất lớn so với giới hạn nhỏ bé trên`n`. 

Tìm kiếm brute-force không thành công vì nó tốn thời gian để khám phá một phân rã đã tồn tại ở dạng đơn giản nhất có thể. Sự quan sát đó`1³ = 1`thay đổi hoàn toàn vấn đề. Vì mục tiêu không bao giờ lớn hơn`9241`, lặp lại`1`chính xác`n`lần luôn thỏa mãn cả điều kiện tổng và số lượng số hạng tối đa. 

Giải pháp tối ưu chỉ đơn giản là xuất ra`n`bản sao của`1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Phụ thuộc vào chiến lược tìm kiếm, có khả năng theo cấp số nhân | Phụ thuộc vào chiến lược tìm kiếm | Quá chậm | 
| Tối ưu | O(n) | O(n) để lưu trữ đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số mục tiêu`n`. Bản thân giá trị cho chúng ta biết chúng ta cần bao nhiêu số hạng, bởi vì mỗi số hạng`1`đóng góp chính xác`1`đến tổng khối. 
2. Tạo một phân rã có chứa`n`bản sao của số nguyên`1`. Tổng lập phương thu được là`n × 1³`, chính xác là`n`. 
3. In`n`là số lượng các thuật ngữ, theo sau là danh sách các thuật ngữ. Số số hạng tối đa có thể có là`9241`, do đó giới hạn đầu ra không bao giờ vượt quá. 

Tại sao nó hoạt động: bất biến là sau khi chọn`k`bản sao của`1`, tổng lập phương một phần chính xác là`k`. Khi thuật toán kết thúc sau khi chọn`n`bản sao, tổng số chính xác`n`. Mọi giá trị được chọn đều nằm trong phạm vi cho phép và số lượng giá trị được chọn là hợp lệ vì`n ≤ 9241 < 10000`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    print(n)
    print(" ".join(["1"] * n))

if __name__ == "__main__":
    solve()
```Chương trình đọc số nguyên đơn và sử dụng nó trực tiếp làm số hạng. Việc xây dựng danh sách tạo ra chính xác`n`bản sao của giá trị`1`. 

Thứ tự đầu ra quan trọng vì dòng đầu tiên phải chứa số lượng. Sau đó, dòng thứ hai chứa phân tách thực tế. 

Không có lo ngại về tràn vì số nguyên Python có độ chính xác tùy ý và đầu ra lớn nhất chỉ chứa`9241`số nguyên nhỏ. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
3
```Thuật toán tạo ra ba bản sao của`1`. 

| Bước | n | Điều khoản được tạo | Tổng khối | 
| --- | --- | --- | --- | 
| Bắt đầu | 3 | trống | 0 | 
| Thêm thuật ngữ đầu tiên | 3 | 1 | 1 | 
| Thêm thuật ngữ thứ hai | 3 | 1 1 | 2 | 
| Thêm thuật ngữ thứ ba | 3 | 1 1 1 | 3 | 

Tổng cuối cùng là`1³ + 1³ + 1³ = 3`, phù hợp với mục tiêu. 

Hãy xem xét đầu vào:```
7
```| Bước | n | Điều khoản được tạo | Tổng khối | 
| --- | --- | --- | --- | 
| Bắt đầu | 7 | trống | 0 | 
| Hoàn thiện công trình | 7 | 1 1 1 1 1 1 1 | 7 | 

Ví dụ cho thấy thuật toán không cần tìm kiếm các nhận dạng khối đặc biệt. Nó chỉ dựa vào danh tính luôn hợp lệ`1³ = 1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chương trình viết`n`số cho đầu ra. | 
| Không gian | O(n) | Danh sách của`n`những cái được tạo ra trước khi in. | 

Giá trị tối đa của`n`là`9241`, do đó cả thời gian chạy và mức sử dụng bộ nhớ đều rất nhỏ. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    n = int(input())
    return str(n) + "\n" + " ".join(["1"] * n) + "\n"

# minimum-size input
assert solve_data("1\n") == "1\n1\n", "minimum value"

# small normal case
assert solve_data("5\n") == "5\n1 1 1 1 1\n", "small value"

# boundary condition
assert solve_data("9241\n").count("1") == 9241, "maximum value"

# another custom case
assert solve_data("100\n").splitlines()[0] == "100", "term count"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`| Một thuật ngữ chứa`1`| Mục tiêu hợp lệ nhỏ nhất | 
|`5`| Năm bản sao của`1`| Xây dựng cơ bản | 
|`9241`|`9241`bản sao của`1`| Kích thước mục tiêu và đầu ra tối đa được phép | 
|`100`|`100`bản sao của`1`| Hộp đựng thông thường lớn hơn | 

## Vỏ cạnh 

cho`n = 1`, thuật toán tạo ra đúng một số hạng. Việc thực hiện là:```
n = 1
answer = [1]
```Tổng khối là`1³ = 1`, vì vậy đầu ra là hợp lệ. 

Vì`n = 9241`, thuật toán tạo ra`9241`điều khoản. Việc thực hiện là:```
n = 9241
number of terms = 9241
```Tổng khối là:```
9241 × 1³ = 9241
```Số lượng kỳ hạn vẫn dưới mức tối đa cho phép`10000`, do đó việc xây dựng vẫn có hiệu lực. 

Đối với các giá trị như`n = 3`, giải pháp dựa trên tìm kiếm có thể cố gắng tìm một cách trình bày ngắn hơn như`1³ + 1³ + 1³`, nhưng thuật toán tối ưu thực hiện chính xác điều đó mà không cần tìm kiếm. Chuỗi được tạo luôn có số tiền cần thiết vì mọi đóng góp riêng lẻ đều có thể dự đoán được.
