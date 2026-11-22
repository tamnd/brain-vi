---
title: "CF 285A - Hoán vị giảm nhẹ"
description: "Chúng ta được yêu cầu xây dựng một hoán vị các số từ 1 đến n có chính xác k vị trí trong đó số lớn hơn số ngay sau nó. Nói cách khác, chúng ta cần một chuỗi có độ dài n trong đó xuất hiện chính xác k \"hậu duệ\"."
date: "2026-06-05T09:45:27+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 285
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 175 (Div. 2)"
rating: 1100
weight: 285
solve_time_s: 127
verified: false
draft: false
---

[CF 285A - Giảm nhẹ hoán vị](https://codeforces.com/problemset/problem/285/A) 

**Đánh giá:** 1100 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một hoán vị các số từ 1 đến _n_ có chính xác _k_ vị trí trong đó một số lớn hơn số ngay sau nó. Nói cách khác, chúng ta cần một chuỗi có độ dài _n_ trong đó xảy ra chính xác _k_ "hạ xuống". Đầu vào bao gồm hai số nguyên, _n_ và _k_, và đầu ra là bất kỳ hoán vị nào thỏa mãn thuộc tính này. Các số trong hoán vị là khác nhau và nằm trong khoảng từ 1 đến _n_. 

Các ràng buộc cho phép _n_ lên tới 10^5 và _k_ nhỏ hơn _n_. Điều này ngụ ý rằng chúng ta không thể mua được các thuật toán có độ phức tạp bậc hai hoặc cao hơn. Một giải pháp phải hoạt động trong thời gian tuyến tính hoặc gần tuyến tính. Việc sử dụng bộ nhớ rất rộng rãi nhưng nên tránh các cấu trúc dữ liệu bổ sung không cần thiết. 

Các trường hợp cạnh bao gồm _k_ bằng 0, trong đó hoán vị phải tăng hoàn toàn. Một trường hợp khác là _k_ bằng _n_-1, trong đó hoán vị giảm hoàn toàn. Bất kỳ việc triển khai bất cẩn nào xây dựng trình tự bằng cách thử và sai hoặc hoán đổi mà không có cấu trúc đều có thể thất bại ở những thái cực này. Ví dụ, với đầu vào`5 0`, một chiến lược giảm dần đầu tiên ngây thơ sẽ tạo ra`[5,4,3,2,1]`, có bốn phần giảm dần thay vì số không. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tất cả các hoán vị của các số từ 1 đến _n_ và đếm số phần giảm dần cho đến khi tìm thấy một hoán vị có chính xác số phần trăm _k_. Số hoán vị là _n_! và việc đếm số lần giảm mất O(n), do đó tổng công là O(n! × n), điều này không thể thực hiện được ngay cả đối với _n_ nhỏ, như 10! đã là 3.628.800 rồi. 

Điểm mấu chốt của một giải pháp tối ưu là mỗi lần giảm dần xảy ra khi một số lớn hơn ngay sau đó là một số nhỏ hơn. Để tạo chính xác các phần giảm _k_, chúng ta có thể chọn các số _k_ đầu tiên là các số _k_ lớn nhất theo thứ tự giảm dần, đảm bảo giảm dần ở mọi vị trí, sau đó nối các số còn lại theo thứ tự tăng dần để tránh các phần giảm thêm. Việc xây dựng tham lam này đảm bảo chính xác _k_ sự sụt giảm mà không cần thử và sai. 

Cách tiếp cận tham lam có hiệu quả vì mỗi lần giảm được điều khiển trực tiếp bằng cách chọn một số lớn hơn số tiếp theo. Khi chúng tôi đặt các số _k_ đầu tiên theo thứ tự giảm dần, mọi số tiếp theo được thêm vào theo thứ tự tăng dần sẽ không thể tạo thêm phần giảm bớt, đảm bảo hệ số giảm chính xác là _k_. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! × n) | O(n) | Quá chậm | 
| Xây dựng tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai con trỏ:`left = 1`Và`right = n`. Chúng đại diện cho số nhỏ nhất và lớn nhất có sẵn để đặt trong hoán vị. 
2. Tạo danh sách trống`perm`để giữ hoán vị cuối cùng. 
3. Lặp lại _k_ lần: 

- Nối thêm`right`ĐẾN`perm`. Điều này đặt số lượng lớn nhất có sẵn để đảm bảo giảm dần. 
- Giảm bớt`right`bằng 1. Thao tác này sẽ xóa số khỏi tập hợp có sẵn. 
4. Lặp lại cho các số _n - k_ còn lại: 

- Nối thêm`left`ĐẾN`perm`. Điều này đặt số lượng nhỏ nhất có sẵn, không duy trì thêm phần giảm bớt nào. 
- Tăng`left`bằng 1. Thao tác này sẽ xóa số khỏi tập hợp có sẵn. 
5. Xuất ra`perm`danh sách. 

Tại sao nó hoạt động: Mỗi lần giảm đều được tạo ra bằng cách đặt số lớn hơn trước số nhỏ hơn. Bằng cách lấy _k_ số lớn nhất theo thứ tự giảm dần, chúng tôi đảm bảo chính xác _k_ giảm dần khi bắt đầu. Việc đặt các số còn lại theo thứ tự tăng dần sẽ đảm bảo không có sự giảm thêm nào. Điều bất biến là sau mỗi bước, số lần giảm xuống được đặt bằng số lần chúng ta đã chọn từ`right`con trỏ. Phương pháp này không thể sản xuất quá mức hoặc sản xuất quá ít chất làm khô. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())

perm = []
left, right = 1, n

for _ in range(k):
    perm.append(right)
    right -= 1

for _ in range(n - k):
    perm.append(left)
    left += 1

print(" ".join(map(str, perm)))
```Vòng lặp đầu tiên đảm bảo chúng ta nhận được chính xác _k_ phần rút gọn bằng cách chọn số lớn nhất trước. Vòng lặp thứ hai điền vào các số còn lại theo thứ tự tăng dần để ngăn chặn bất kỳ sự sụt giảm bổ sung nào. Cách tiếp cận hai con trỏ tránh được các thao tác sắp xếp hoặc tập hợp bổ sung, khiến nó trở thành O(n). 

## Ví dụ đã hoạt động 

**Mẫu 1**:`n = 5, k = 2`| Bước | trái | đúng | uốn | 
| --- | --- | --- | --- | 
| 0 | 1 | 5 | [] | 
| 1 | 1 | 4 | [5] | 
| 2 | 1 | 3 | [5, 4] | 
| 3 | 1 | 3 | [5, 4, 1] | 
| 4 | 2 | 3 | [5, 4, 1, 2] | 
| 5 | 3 | 3 | [5, 4, 1, 2, 3] | 

Điều này tạo ra`[5,4,1,2,3]`với hai điểm hạ xuống ở vị trí 1→2 và 2→3. 

**Mẫu 2**:`n = 4, k = 0`| Bước | trái | đúng | uốn | 
| --- | --- | --- | --- | 
| 0 | 1 | 4 | [] | 
| 1 | 1 | 4 | [1] | 
| 2 | 2 | 4 | [1,2] | 
| 3 | 3 | 4 | [1,2,3] | 
| 4 | 4 | 4 | [1,2,3,4] | 

Điều này tạo ra`[1,2,3,4]`với độ dốc bằng không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi số được thêm chính xác một lần trong một lần truyền tuyến tính | 
| Không gian | O(n) | Chúng tôi lưu trữ hoán vị cuối cùng có kích thước n | 

Với _n_ ≤ 10^5, thuật toán thực thi thoải mái trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    perm = []
    left, right = 1, n
    for _ in range(k):
        perm.append(right)
        right -= 1
    for _ in range(n - k):
        perm.append(left)
        left += 1
    return " ".join(map(str, perm))

# provided sample
assert run("5 2\n") == "5 4 1 2 3", "sample 1"

# custom cases
assert run("4 0\n") == "1 2 3 4", "increasing permutation, k=0"
assert run("4 3\n") == "4 3 2 1", "fully decreasing permutation, k=n-1"
assert run("1 0\n") == "1", "single element, k=0"
assert run("6 2\n") == "6 5 1 2 3 4", "mixed descending/ascending"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 0 | 1 2 3 4 | Xử lý k=0 đúng cách | 
| 4 3 | 4 3 2 1 | Xử lý k=n-1 chính xác | 
| 1 0 | 1 | Vỏ cạnh đơn | 
| 6 2 | 6 5 1 2 3 4 | Vị trí thích hợp của k phần giảm dần còn lại tăng dần | 

## Vỏ cạnh 

cho`k=0`với đầu vào`4 0`, thuật toán không đặt số nào từ con trỏ bên phải và tất cả các số được lấy từ bên trái, tạo ra`[1,2,3,4]`. Điều này một cách chính xác tránh bất kỳ hậu duệ. 

Vì`k=n-1`với đầu vào`4 3`, thuật toán lấy ba số lớn nhất`[4,3,2]`từ bên phải và nhỏ nhất còn lại`[1]`từ bên trái, sản xuất`[4,3,2,1]`, đưa ra chính xác ba lần rút gọn. 

Vì`n=1`, nhập`1 0`, các vòng lặp thực hiện lần lượt 0 và một lần, tạo ra`[1]`, thỏa mãn yêu cầu một cách tầm thường. 

Điều này xác nhận cách tiếp cận tham lam xử lý chính xác các trường hợp kích thước tối thiểu, kích thước tối đa và ranh giới.
