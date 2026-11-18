---
title: "CF 2041A - Cuộc phiêu lưu hộp Bento"
description: "Bài toán trình bày một tình huống trong đó một người ghé thăm một nhà hàng mỗi ngày từ thứ Hai đến thứ Năm, chọn một nhà hàng khác nhau mỗi ngày từ một bộ năm nhà hàng có thể có. Dữ liệu đầu vào đưa ra trình tự bốn nhà hàng riêng biệt được ghé thăm, mỗi ngày một nhà hàng."
date: "2026-06-08T09:40:19+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2041
codeforces_index: "A"
codeforces_contest_name: "2024 ICPC Asia Taichung Regional Contest (Unrated, Online Mirror, ICPC Rules, Preferably Teams)"
rating: 1300
weight: 2041
solve_time_s: 85
verified: true
draft: false
---

[CF 2041A - Cuộc phiêu lưu hộp Bento](https://codeforces.com/problemset/problem/2041/A) 

**Đánh giá:** 1300 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán trình bày một tình huống trong đó một người ghé thăm một nhà hàng mỗi ngày từ thứ Hai đến thứ Năm, chọn một nhà hàng khác nhau mỗi ngày từ một bộ năm nhà hàng có thể có. Dữ liệu đầu vào đưa ra trình tự bốn nhà hàng riêng biệt được ghé thăm, mỗi ngày một nhà hàng. Nhiệm vụ là xác định nhà hàng nào chưa được ghé thăm và nên chọn vào thứ Sáu. 

Về mặt hình thức, đầu vào bao gồm bốn số nguyên từ 1 đến 5, tất cả đều khác nhau. Đầu ra là một số nguyên từ 1 đến 5 bị thiếu trong danh sách này. Với kích thước đầu vào nhỏ, hiệu quả không phải là mối quan tâm lớn nhưng tính chính xác là điều tối quan trọng. 

Các trường hợp có biên không rõ ràng bao gồm các tình huống trong đó nhà hàng bị thiếu là số thấp nhất (1) hoặc số cao nhất (5). Việc triển khai bất cẩn có thể cho rằng số bị thiếu luôn nằm ở giữa phạm vi, điều này sẽ không thành công đối với các đầu vào như`2 3 4 5`(thiếu 1) hoặc`1 2 3 4`(thiếu 5). 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực đơn giản là lặp qua các số từ 1 đến 5 và kiểm tra từng số theo danh sách đã truy cập. Nếu không tìm thấy số, nó sẽ được trả về dưới dạng câu trả lời. Điều này hiệu quả vì danh sách các nhà hàng đã ghé thăm được đảm bảo chứa chính xác bốn số riêng biệt, do đó, chính xác một số sẽ bị thiếu. Phép lặp brute-force kiểm tra tối đa năm số, điều này không đáng kể về mặt tính toán. 

Cách tiếp cận tối ưu tận dụng đặc tính mà các số từ 1 đến 5 tạo thành một bộ hoàn chỉnh. Bằng cách tính tổng của tập hợp`{1, 2, 3, 4, 5}`và trừ tổng số nhà hàng đã ghé thăm, chúng ta sẽ có ngay số còn thiếu. Điều này sử dụng số học cơ bản thay vì lặp lại và chia tỷ lệ một cách tự nhiên nếu phạm vi hoặc số lượng nhà hàng thay đổi, mặc dù trong bài toán này, kích thước không đổi khiến cả hai phương pháp đều có hiệu quả tức thời. 

Thông tin chi tiết quan trọng là nhận ra rằng tổng của năm số nguyên dương đầu tiên là cố định (15) và việc trừ đi tổng của bốn nhà hàng đã ghé thăm sẽ tách biệt nhà hàng chưa ghé thăm. Điều này tránh việc kiểm tra không cần thiết hoặc cấu trúc dữ liệu phức tạp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(5 * 4) = O(20) | O(1) | Đã chấp nhận | 
| Tính Tổng | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bốn số nguyên từ đầu vào, đại diện cho các nhà hàng đã ghé thăm từ thứ Hai đến thứ Năm. Lưu trữ chúng trong một danh sách`visited`. 
2. Tính tổng của năm số nguyên dương đầu tiên là`15`. 
3. Tính tổng của bốn nhà hàng đã ghé thăm. 
4. Trừ tổng số nhà hàng đã ghé thăm từ`15`. Kết quả là số nhà hàng chưa được ghé thăm. 
5. Xuất kết quả. 

Điều này hiệu quả vì tổng của một tập hợp hoàn chỉnh trừ đi tổng của một tập hợp con để lại chính xác phần tử bị thiếu. Thuật toán không phụ thuộc vào thứ tự, vị trí của các số đầu vào, đảm bảo tính chính xác cho tất cả các đầu vào hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

visited = list(map(int, input().split()))
all_sum = 15
visited_sum = sum(visited)
print(all_sum - visited_sum)
```Dòng đầu tiên đảm bảo việc đọc đầu vào nhanh chóng. Chúng tôi ánh xạ bốn số nguyên được phân tách bằng dấu cách vào một danh sách. Tính tổng của bốn số và trừ đi 15 sẽ cho ra nhà hàng còn thiếu. Không cần vòng lặp hoặc điều kiện nào ngoài việc phân tích cú pháp đầu vào, giảm thiểu chỗ cho các lỗi sai sót một. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:`1 3 2 5`| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| 1 | đã ghé thăm | [1, 3, 2, 5] | 
| 2 | tất cả_sum | 15 | 
| 3 | đã truy cập_sum | 1 + 3 + 2 + 5 = 11 | 
| 4 | kết quả | 15 - 11 = 4 | 

Đầu ra:`4`Điều này chứng tỏ rằng việc trừ đi tổng số nhà hàng đã ghé thăm sẽ tách biệt chính xác nhà hàng còn thiếu. 

**Ví dụ tùy chỉnh 2** 

đầu vào:`2 3 4 5`| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| 1 | đã ghé thăm | [2, 3, 4, 5] | 
| 2 | tất cả_sum | 15 | 
| 3 | đã truy cập_sum | 2 + 3 + 4 + 5 = 14 | 
| 4 | kết quả | 15 - 14 = 1 | 

Đầu ra:`1`Điều này xác nhận thuật toán xác định chính xác số còn thiếu nhỏ nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có bốn số được tính tổng và phép trừ được thực hiện, không phụ thuộc vào thứ tự đầu vào. | 
| Không gian | O(1) | Chỉ lưu trữ danh sách bốn số nguyên và một vài biến vô hướng. | 

Với những hạn chế, giải pháp này thực thi trong thời gian và bộ nhớ không đáng kể, trong giới hạn 1 giây, 1024 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    visited = list(map(int, input().split()))
    all_sum = 15
    visited_sum = sum(visited)
    return str(all_sum - visited_sum)

# Provided sample
assert run("1 3 2 5") == "4", "sample 1"

# Custom cases
assert run("2 3 4 5") == "1", "missing smallest"
assert run("1 2 3 4") == "5", "missing largest"
assert run("1 2 4 5") == "3", "missing middle"
assert run("1 3 4 5") == "2", "missing lower-middle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 3 2 5 | 4 | Vỏ tiêu chuẩn | 
| 2 3 4 5 | 1 | Thiếu số nhỏ nhất | 
| 1 2 3 4 | 5 | Thiếu số lớn nhất | 
| 1 2 4 5 | 3 | Thiếu số giữa | 
| 1 3 4 5 | 2 | Thiếu trung dưới | 

## Vỏ cạnh 

Đối với đầu vào`2 3 4 5`, thuật toán tính toán chính xác`15 - (2+3+4+5) = 1`. Nó không cho rằng các số bị thiếu nằm ở giữa. Đối với đầu vào`1 2 3 4`, nó tính toán`15 - 10 = 5`, cho thấy nó cũng xử lý ranh giới trên. Bất kỳ hoán vị nào của bốn số riêng biệt từ 1 đến 5 sẽ được xử lý giống hệt nhau, xác nhận tính chắc chắn.
