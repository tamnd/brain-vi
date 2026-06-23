---
title: "CF 105271A - tam giác ACC"
description: "Chúng ta được cung cấp hai loại tài nguyên: một loại đại diện cho các quân cờ “A” đơn lẻ và loại kia đại diện cho các quân cờ “C”. Bằng cách sử dụng những thứ này, chúng ta muốn xây dựng một cấu trúc hình tam giác trong đó hàng i chứa chính xác một chữ A theo sau là i−1 C. Vì vậy, cấu trúc phát triển từng hàng."
date: "2026-06-23T06:57:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105271
codeforces_index: "A"
codeforces_contest_name: "Almaty Code Cup 2024"
rating: 0
weight: 105271
solve_time_s: 52
verified: true
draft: false
---

[CF 105271A - Tam giác ACC](https://codeforces.com/problemset/problem/105271/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp hai loại tài nguyên: một loại đại diện cho các quân cờ “A” đơn lẻ và loại kia đại diện cho các quân cờ “C”. Bằng cách sử dụng những thứ này, chúng ta muốn xây dựng một cấu trúc hình tam giác trong đó hàng i chứa chính xác một chữ A theo sau là i−1 C. 

Vì vậy, cấu trúc phát triển từng hàng. Hàng đầu tiên cần 1 A và 0 C. Hàng thứ hai cần 1 A và 1 C. Hàng thứ ba cần 1 A và 2 C, v.v. Mỗi hàng bổ sung sẽ tăng số lượng C cần thiết, trong khi luôn tiêu thụ chính xác một A. 

Nhiệm vụ là xác định chiều cao h tối đa sao cho có thể xây dựng tất cả các hàng từ 1 đến h mà không vượt quá x A và y C có sẵn. 

Ràng buộc x, y 10^18 buộc lời giải phải tránh mọi mô phỏng trên các hàng. Trong trường hợp xấu nhất, một cấu trúc tuyến tính sẽ yêu cầu tới 10^18 lần lặp, điều này là không thể trong một giây. Ngay cả chi phí logarit cho mỗi bài kiểm tra cũng ổn, nhưng mọi thứ phụ thuộc trực tiếp vào h thì không. 

Một trường hợp thất bại tinh tế xuất phát từ việc xây dựng hàng loạt tham lam mà không có kế hoạch. Ví dụ: nếu x lớn nhưng y nhỏ, một cách tiếp cận đơn giản có thể cố gắng xây dựng cho đến khi hết C trên mỗi hàng, nhưng lại tính sai mức sử dụng C tích lũy. Một vấn đề khác là quên rằng mức sử dụng A hoàn toàn tuyến tính về chiều cao trong khi mức sử dụng C tăng theo phương trình bậc hai. 

Ví dụ: nếu x = 10 và y = 3, cách tiếp cận từng hàng đơn giản có thể thử 4 hàng vì A cho phép, nhưng hàng 4 sẽ yêu cầu tổng cộng 6 C, điều này là không thể. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp xây dựng từng hàng một. Đối với mỗi hàng i, chúng tôi trừ đi một A và i−1 C từ nhóm có sẵn. Điều này hiệu quả vì cấu trúc mang tính quyết định, vì vậy chúng ta có thể tham lam xây dựng cho đến khi cạn kiệt tài nguyên. Tuy nhiên, trong trường hợp xấu nhất, số lượng hàng có thể lớn tới 10^9 hoặc hơn, khiến điều này không thể thực hiện được. 

Quan sát quan trọng là mức tiêu thụ tài nguyên chỉ phụ thuộc vào độ cao h chứ không phụ thuộc vào các lựa chọn trung gian. Sau khi tạo h hàng, tổng mức sử dụng A chính xác là h và tổng mức sử dụng C là tổng 0 + 1 + 2 + ... + (h−1), bằng h(h−1)/2. Điều này biến vấn đề thành việc kiểm tra tính khả thi của một h. 

Bây giờ nhiệm vụ sẽ là tìm h tối đa sao cho cả hai ràng buộc đều đúng: h ≤ x và h(h−1)/2 ≤ y. Ràng buộc đầu tiên là tuyến tính, ràng buộc thứ hai là bậc hai. Ràng buộc bậc hai có thể được giải trực tiếp bằng cách sử dụng biểu thức dạng đóng xuất phát từ bất đẳng thức, do đó không cần tìm kiếm nhị phân. 

Chúng tôi tính h lớn nhất thỏa mãn h(h−1)/2 ≤ y bằng cách sử dụng công thức bậc hai và sau đó lấy giá trị tối thiểu bằng x. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(h) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính độ cao lớn nhất có thể có chỉ bị giới hạn bởi điểm A. Đây chỉ đơn giản là x, vì mỗi hàng tiêu thụ chính xác một A. 
2. Tính độ cao lớn nhất mà C cho phép bằng cách giải bất đẳng thức h(h−1)/2 ≤ y. Điều này xuất phát từ việc tính tổng số C cần thiết trên tất cả các hàng. 
3. Sắp xếp lại bất đẳng thức thành h² − h − 2y ≤ 0, đây là ràng buộc bậc hai tiêu chuẩn. 
4. Tính căn dương của phương trình tương ứng h = (1 + sqrt(1 + 8y)) / 2 và lấy giá trị sàn của nó. Điều này cho ra số nguyên h lớn nhất không vi phạm ràng buộc C. 
5. Câu trả lời cuối cùng là giới hạn nhỏ hơn trong hai giới hạn: một từ A và một từ C. 

### Tại sao nó hoạt động 

Cấu trúc bắt buộc mỗi hàng tiêu thụ chính xác một A một cách độc lập, vì vậy việc sử dụng A có chiều cao tuyến tính. Yêu cầu C tích lũy một cách xác định dưới dạng tổng tiền tố, do đó, mọi chiều cao hợp lệ đều được mô tả đầy đủ bằng một bất đẳng thức vô hướng duy nhất. Bởi vì cả hai ràng buộc đều đơn điệu trong h, nên khi một chiều cao trở nên không hợp lệ thì tất cả các độ cao lớn hơn cũng không hợp lệ, đảm bảo mức tối đa được tính toán là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def solve():
    x, y = map(int, input().split())
    
    # limit from A's
    h_a = x
    
    # limit from C's: h(h-1)/2 <= y
    # solve h^2 - h - 2y <= 0
    disc = 1 + 8 * y
    h_c = (1 + math.isqrt(disc)) // 2
    
    print(min(h_a, h_c))

if __name__ == "__main__":
    solve()
```Việc thực hiện tách biệt hai ràng buộc một cách rõ ràng. Ràng buộc A là trực tiếp vì mỗi hàng tiêu thụ chính xác một A. Ràng buộc C được xử lý bằng căn bậc hai số nguyên để tránh các vấn đề về độ chính xác của dấu phẩy động, vì y có thể lớn tới 10^18. sử dụng`math.isqrt`đảm bảo số học số nguyên chính xác. 

trận chung kết`min`kết hợp cả hai ràng buộc vì một tam giác hợp lệ phải thỏa mãn cả hai ràng buộc cùng một lúc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đầu vào: x = 4, y = 6 

| Bước | h từ A | h từ C | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| tính toán giới hạn | 4 | giải h(h−1)/2 ≤ 6 → h = 4 | 4 | 

Ở đây, cả hai ràng buộc đều cho phép chiều cao 4. Kiểm tra mức sử dụng C: 0+1+2+3 = 6, khớp chính xác với C có sẵn, vì vậy có thể đạt được chiều cao đầy đủ. 

### Ví dụ 2 

Đầu vào: x = 4, y = 5 

| Bước | h từ A | h từ C | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| tính toán giới hạn | 4 | giải h(h−1)/2 ≤ 5 → h = 3 | 3 | 

Mặc dù A cho phép có 4 hàng nhưng C lại bị phá vỡ ở độ cao 4 vì nó yêu cầu 6 hàng C. Do đó, hệ số giới hạn là C và câu trả lời là 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các phép tính số học và một căn bậc hai số nguyên | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung | 

Giải pháp này vừa vặn thoải mái trong các ràng buộc vì nó thực hiện tính toán theo thời gian không đổi bất kể kích thước đầu vào lên tới 10^18. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isqrt

    x, y = map(int, input().split())
    h_a = x
    disc = 1 + 8 * y
    h_c = (1 + isqrt(disc)) // 2
    return str(min(h_a, h_c))

# provided samples (interpreted)
assert run("4 6") == "4"
assert run("4 5") == "3"

# minimum case
assert run("1 0") == "1"

# only C limits
assert run("1000000000000000000 0") == "1"

# tight triangular C bound
assert run("10 45") == "10"

# x limits strongly
assert run("3 1000000000000000000") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 1 | nguồn lực tối thiểu | 
| 10^18 0 | 1 | C = 0 trường hợp cực đoan | 
| 10 45 | 10 | số tam giác hoàn hảo | 
| 3 10^18 | 3 | Một hạn chế chiếm ưu thế | 

## Vỏ cạnh 

Trường hợp một cạnh là khi y = 0. Trong trường hợp này, chỉ có thể có các hàng không yêu cầu C. Điều đó có nghĩa là chỉ có hàng đầu tiên có thể tồn tại bất kể x. Công thức cho disc = 1, do đó h_c = 1, và đáp án cuối cùng là min(x, 1), kết quả này đúng. 

Một trường hợp cạnh khác là khi x nhỏ hơn nhiều so với chiều cao giới hạn C. Ví dụ x = 2 và y là cực kỳ lớn. Công thức C có thể mang lại h_c lớn, nhưng câu trả lời cuối cùng chính xác giới hạn ở mức 2 vì A được tiêu thụ tuyến tính và ngay lập tức trở thành nút thắt cổ chai. 

Trường hợp thứ ba là khi y khớp chính xác với một số tam giác. Với y = 6, công thức mang lại h_c = 4 chính xác vì 1 + 2 + 3 = 6, xác nhận rằng ranh giới đẳng thức được xử lý mà không có lỗi sai sót nào do hành vi sàn căn bậc hai số nguyên.
