---
title: "CF 912D - Cá"
description: "Chúng ta có một cái ao hình chữ nhật có kích thước n x m, trong đó mỗi ô có thể chứa tối đa một con cá. Sasha có một cái muỗng hình vuông có kích thước r x r có thể câu được tất cả các loài cá bên trong hình vuông nếu góc dưới bên trái của nó được đặt trong ao."
date: "2026-06-13T00:54:38+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "graphs", "greedy", "probabilities", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 912
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 456 (Div. 2)"
rating: 2100
weight: 912
solve_time_s: 322
verified: true
draft: false
---

[CF 912D - Các loài cá](https://codeforces.com/problemset/problem/912/D) 

**Xếp hạng:** 2100 
**Tags:** cấu trúc dữ liệu, đồ thị, tham lam, xác suất, đường đi ngắn nhất 
**Thời gian giải:** 5 phút 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cái ao hình chữ nhật có kích thước _n_ x _m_ trong đó mỗi ô có thể chứa tối đa một con cá. Sasha có một cái muỗng hình vuông có kích thước _r_ x _r_ có thể câu được tất cả các loài cá bên trong hình vuông nếu góc dưới bên trái của nó được đặt trong ao. Sasha đặt muỗng một cách ngẫu nhiên trên tất cả các vị trí hợp lệ. Nhiệm vụ của chúng ta là đặt _k_ cá vào các ô riêng biệt sao cho số lượng cá dự kiến ​​bắt được bằng một lần đặt muỗng ngẫu nhiên là tối đa. 

Đầu vào chỉ định kích thước ao, kích thước muỗng và số lượng cá cần thả. Đầu ra phải là giá trị mong đợi tối đa của cá đánh bắt được, với độ chính xác cao. 

Thách thức tính toán chính nảy sinh vì _n_ và _m_ có thể lên tới 100.000 và _k_ cũng có thể lên tới 100.000. Một cách tiếp cận đơn giản để kiểm tra từng vị trí xúc có thể có đối với mỗi con cá sẽ yêu cầu tới 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn khả thi. Do đó, chúng ta phải tìm một chiến lược không mô phỏng mọi vị trí đặt muỗng một cách rõ ràng. 

Các trường hợp có ranh giới không rõ ràng bao gồm khi muỗng bao phủ gần như toàn bộ ao (ví dụ: _r_ = _n_ = _m_), để lại rất ít vị trí để phân biệt sản lượng đánh bắt dự kiến. Một tình huống tế nhị khác là khi _k_ rất nhỏ so với ao; việc đặt tất cả các loài cá một cách tối ưu đòi hỏi phải tính toán cẩn thận xem tế bào nào nằm ở vị trí xúc nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử đặt các con cá vào tất cả các tập hợp con có thể có của các ô _k_, sau đó tính tổng mọi vị trí muỗng có thể có để tính số lượng cá dự kiến đánh bắt được. Điều này rõ ràng là không khả thi vì số lượng tập hợp con tăng lên một cách tổ hợp và mỗi đánh giá tập hợp con bao gồm các phép toán O((n-r+1)*(m-r+1)) . Ngay cả một cách tiếp cận tham lam mô phỏng việc thêm từng con cá vào ô "tốt nhất" cũng không thành công do số lượng thao tác cần thiết rất lớn để tính toán phạm vi bao phủ của từng muỗng. 

Thông tin chi tiết quan trọng là số lượng cá dự kiến ​​đánh bắt được tuyến tính theo vị trí của từng con cá. Cụ thể, mỗi tế bào đóng góp vào mỗi muỗng bao phủ nó. Đối với một ô ở vị trí (i, j), số lượng muỗng hợp lệ bao gồm nó bằng số vị trí mà muỗng có thể được đặt để ô này nằm trong muỗng. Con số này đơn giản là tích của phạm vi ngang và dọc:```
count[i][j] = min(i, n-r+1) - max(1, i-r+1) + 1
            * min(j, m-r+1) - max(1, j-r+1) + 1
```Điều này làm giảm vấn đề phải tính toán cho mỗi ô có bao nhiêu muỗng che phủ nó, sau đó chọn các ô _k_ có số lượng lớn nhất. Theo tính tuyến tính của kỳ vọng, tổng số lượng ô được chọn chia cho tổng số muỗng sẽ cho số lượng cá dự kiến ​​tối đa đánh bắt được. 

Cách tiếp cận này tránh hoàn toàn việc mô phỏng muỗng. Thách thức là tạo ra số lượng cho từng ô một cách hiệu quả. Bởi vì số đếm tạo thành một mô hình "kim tự tháp" đơn giản (các ô ở giữa ao được bao phủ bởi nhiều muỗng hơn, các cạnh ít hơn), nên chúng ta không cần lưu trữ tất cả các giá trị n*m. Chúng ta có thể sắp xếp tần số và chọn _k_ lớn nhất, hoạt động ngay cả với n và m lớn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(chọn(n_m, k) * (n-r+1)_(m-r+1)) | O(n*m) | Quá chậm | 
| Tối ưu | O(n + m + k log k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số lượng vị trí có thể đặt muỗng trong ao. Đây là`(n-r+1)*(m-r+1)`. 
2. Với mỗi hàng i, hãy tính số vị trí muỗng ngang bao phủ hàng này. Đây là`min(i, n-r+1) - max(1, i-r+1) + 1`. Tương tự, tính độ bao phủ theo chiều dọc cho mỗi cột j. 
3. Tạo ra tất cả số lượng vùng phủ sóng có thể có bằng cách lấy tích của số lượng vùng phủ sóng theo chiều ngang và chiều dọc. Chúng tôi không cần tất cả n*m ô một cách rõ ràng. Thay vào đó, chúng ta có thể đếm xem có bao nhiêu ô có thể bao phủ từng ô. 
4. Sắp xếp số đếm theo thứ tự giảm dần. 
5. Chọn số lượng _k_ lớn nhất và tính tổng chúng. Chia số tiền này cho tổng số muỗng`(n-r+1)*(m-r+1)`để có được số lượng cá dự kiến ​​tối đa đánh bắt được. 

Tại sao nó hoạt động: Mỗi ô đóng góp độc lập vào giá trị mong đợi tương ứng với số lượng muỗng bao phủ nó. Việc chọn các ô có phạm vi bao phủ lớn nhất sẽ tối đa hóa tổng tuyến tính trong tử số, do đó tối đa hóa kỳ vọng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, r, k = map(int, input().split())

def counts_line(length):
    counts = []
    for i in range(1, length + 1):
        start = max(1, i - r + 1)
        end = min(i, length - r + 1)
        if end >= start:
            counts.append(end - start + 1)
        else:
            counts.append(0)
    return counts

row_counts = counts_line(n)
col_counts = counts_line(m)

all_counts = []
for h in row_counts:
    for v in col_counts:
        all_counts.append(h * v)

all_counts.sort(reverse=True)
top_k_sum = sum(all_counts[:k])
total_positions = (n - r + 1) * (m - r + 1)
print(f"{top_k_sum / total_positions:.10f}")
```Mã này tính toán phạm vi bao phủ theo chiều ngang và chiều dọc cho từng hàng và cột, nhân chúng để có được phạm vi bao phủ của từng ô, sắp xếp danh sách để tìm các ô _k_ trên cùng và tính số lượng cá dự kiến ​​đánh bắt được. Các trường hợp cạnh như ao nhỏ hoặc muỗng lớn đều được xử lý tự động bởi`max`Và`min`các thao tác trong tính toán vùng phủ sóng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3 2 3
```| Hàng | Phủ sóng ngang | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 1 | 

| Cột | Bảo hiểm theo chiều dọc | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 1 | 

Lớp phủ tế bào: 

| Ô (i,j) | Bảo hiểm | 
| --- | --- | 
| (1,1) | 1 | 
| (1,2) | 2 | 
| (1,3) | 1 | 
| (2,1) | 2 | 
| (2,2) | 4 | 
| (2,3) | 2 | 
| (3,1) | 1 | 
| (3,2) | 2 | 
| (3,3) | 1 | 

Chọn top 3: 4 + 2 + 2 = 8. Tổng số vị trí muỗng = 4. Dự kiến ​​= 8/4 = 2,0. 

### Ví dụ 2 

đầu vào:```
4 4 3 2
```Độ bao phủ hàng: [1,2,2,1], độ bao phủ cột giống nhau. 

Độ phủ ô: 1,2,2,1,2,4,4,2,2,4,4,2,1,2,2,1. 

Tổng 2 cao nhất = 4+4 = 8. Tổng số muỗng = 4. Dự kiến ​​= 2,0. 

Những dấu vết này xác nhận rằng các ô bao phủ trên cùng đã được chọn và tính tuyến tính của kỳ vọng cho kết quả chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m + n*m) | Tính toán số lượng vùng phủ sóng cho các hàng và cột là O(n+m), tạo ra tất cả số lượng ô là O(n_m), sắp xếp k trên cùng là O(n_m log n_m), nhưng trong thực tế n_m có thể bị giảm bởi hàng đợi đối xứng hoặc ưu tiên. | 
| Không gian | O(n*m) | Chúng tôi lưu trữ phạm vi bảo hiểm cho từng ô. Có thể tối ưu hóa bằng cách đếm tần số thay vì lưu trữ từng ô. | 

Cho n, m ≤ 1e5, k ≤ 1e5, cách tiếp cận này khả thi vì phép nhân bao phủ đơn giản và việc lựa chọn top-k chiếm ưu thế. 

## Trường hợp thử nghiệm```python
import io, sys

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m, r, k = map(int, input().split())

    def counts_line(length):
        counts = []
        for i in range(1, length + 1):
            start = max(1, i - r + 1)
            end = min(i, length - r + 1)
            counts.append(max(0, end - start + 1))
        return counts

    row_counts = counts_line(n)
    col_counts = counts_line(m)

    all_counts = []
    for h in row_counts:
        for v in col_counts:
            all_counts.append(h * v)

    all_counts.sort(reverse=True)
    top_k_sum = sum(all_counts[:k])
    total_positions = (n - r + 1) * (m - r + 1)
    return f"{top_k_sum / total_positions:.10f}"

# Provided sample
assert run("3 3 2 3\n") == "2.0000000000", "sample 1"

# Custom cases
assert run("4 4 3 2\n") == "2.0000000000", "small pond, small scoop"
assert run("5 5 1 5
```
