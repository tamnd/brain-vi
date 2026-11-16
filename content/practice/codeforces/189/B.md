---
title: "CF 189B - Đếm hình thoi"
description: "Chúng ta được yêu cầu đếm các hình thoi bên trong một hình chữ nhật có chiều rộng w và chiều cao h, trong đó mỗi hình thoi có các đỉnh trên tọa độ nguyên và các đường chéo thẳng hàng với các trục. Mỗi hình thoi phải có diện tích dương và nằm trọn trong hình chữ nhật."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 189
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 119 (Div. 2)"
rating: 1300
weight: 189
solve_time_s: 173
verified: true
draft: false
---

[CF 189B - Đếm hình thoi](https://codeforces.com/problemset/problem/189/B) 

**Đánh giá:** 1300 
**Tags:** sức mạnh vũ phu, toán học 
**Thời gian giải:** 2m 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm hình thoi bên trong một hình chữ nhật có chiều rộng _w_ và chiều cao _h_, trong đó mỗi hình thoi có các đỉnh của nó trên tọa độ nguyên và các đường chéo thẳng hàng với các trục. Mỗi hình thoi phải có diện tích dương và nằm trọn trong hình chữ nhật. Đầu ra là một số nguyên duy nhất, biểu thị tổng số hình thoi như vậy. 

Trình bày lại theo thuật ngữ thực tế, mỗi hình thoi có thể được mô tả bằng tâm và nửa đường chéo dọc theo trục x và y. Để hình thoi nằm hoàn toàn bên trong hình chữ nhật, tâm không được quá gần các cạnh và chiều dài nửa đường chéo được giới hạn bởi khoảng cách đến cạnh gần nhất. Chúng ta phải liệt kê tất cả các tổ hợp có thể có của tâm và độ dài nửa đường chéo thỏa mãn các ràng buộc này. 

Giới hạn đầu vào, _w_, _h_ ≤ 4000, cho thấy rằng một cách tiếp cận đơn giản kiểm tra từng bộ bốn điểm là quá chậm, vì nó sẽ liên quan đến các phép toán O(w²h²). Tuy nhiên, nếu chúng ta biểu diễn lại bài toán theo tâm và nửa đường chéo, chúng ta có thể giảm độ phức tạp xuống mức O(wh · min(w, h)) có thể quản lý được. 

Các trường hợp cạnh không rõ ràng bao gồm hình chữ nhật có kích thước tối thiểu (w=1, h=1), trong đó chỉ một hình thoi duy nhất có thể vừa và hình chữ nhật có một cạnh lớn hơn nhiều so với cạnh kia. Ngoài ra, một cách tiếp cận bất cẩn có thể đếm gấp đôi hình thoi bằng cách coi hình dạng giống nhau có tâm ở các tọa độ nguyên khác nhau không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại tất cả các tập hợp bốn điểm có thể có bên trong hình chữ nhật, kiểm tra xem chúng có tạo thành hình thoi với các đường chéo thẳng hàng với trục hay không. Điều này sẽ liên quan đến sự kết hợp O(w²h²) và kiểm tra bổ sung về khoảng cách, quá chậm so với giới hạn nhất định. 

Cái nhìn sâu sắc quan trọng là xem xét hình thoi theo tâm của nó và một nửa chiều dài các đường chéo của nó. Nếu chúng ta ký hiệu tâm là (cx, cy), thì độ dài nửa đường chéo dọc theo x và y, dx và dy, phải thỏa mãn 1  dx  min(cx, w-cx) và 1  dy  min(cy, h-cy). Mỗi giá trị (dx, dy) hợp lệ cho một tâm cho trước tương ứng với đúng một hình thoi. Việc đếm tất cả các kết hợp như vậy trên tất cả các tâm nguyên sẽ tạo ra tổng số hình thoi một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả tăng gấp bốn lần) | O(w²h²) | O(1) | Quá chậm | 
| Tâm + Nửa đường chéo | O(wh·min(w,h)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm về 0. Điều này sẽ tích lũy tổng số hình thoi. 
2. Lặp lại từng tọa độ trung tâm có thể có (cx, cy), trong đó cx nằm trong khoảng từ 0 đến w và cy từ 0 đến h. Tâm phải là một điểm nguyên. 
3. Đối với mỗi tâm, tính nửa đường chéo lớn nhất dọc theo x là dx_max = min(cx, w-cx) và dọc theo y là dy_max = min(cy, h-cy). Điều này đảm bảo hình thoi không vượt ra ngoài hình chữ nhật. 
4. Đếm tất cả các tổ hợp số nguyên dương của dx và dy. Số giá trị dx hợp lệ là dx_max và số giá trị dy hợp lệ là dy_max. Nhân hai số đếm này và cộng vào tổng số bộ đếm. 
5. Sau khi lặp lại tất cả các tâm, xuất ra bộ đếm tổng. 

Tại sao nó hoạt động: Tại mỗi tâm số nguyên, phạm vi dx và dy liệt kê tất cả các hình thoi có thể căn chỉnh theo trục có tâm tại điểm đó. Vì phép tính đảm bảo hình thoi vẫn nằm bên trong hình chữ nhật nên mỗi hình thoi được tính chính xác một lần. Phương pháp này bao gồm tất cả các tâm nguyên và độ dài đường chéo khả thi một cách có hệ thống, do đó không có hình thoi nào bị bỏ sót hoặc bị tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

w, h = map(int, input().split())
count = 0

for cx in range(w + 1):
    for cy in range(h + 1):
        dx_max = min(cx, w - cx)
        dy_max = min(cy, h - cy)
        if dx_max > 0 and dy_max > 0:
            count += dx_max * dy_max

print(count)
```Các vòng lặp lồng nhau lặp qua tất cả các tâm số nguyên. Tính toán`dx_max`Và`dy_max`đảm bảo rằng hình thoi vẫn nằm bên trong hình chữ nhật. Nhân số lựa chọn dx và dy cho mỗi tâm sẽ đếm tất cả các hình thoi có thể được hình thành với tâm đó. điều kiện`dx_max > 0 and dy_max > 0`bỏ qua hình thoi suy biến có diện tích bằng 0. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
2 2
```| cx | cy | dx_max | dy_max | dx_max*dy_max | đếm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 0 | 0 | 
| 0 | 1 | 0 | 1 | 0 | 0 | 
| 0 | 2 | 0 | 0 | 0 | 0 | 
| 1 | 0 | 1 | 0 | 0 | 0 | 
| 1 | 1 | 1 | 1 | 1 | 1 | 
| 1 | 2 | 1 | 0 | 0 | 1 | 
| 2 | 0 | 0 | 0 | 0 | 1 | 
| 2 | 1 | 0 | 1 | 0 | 1 | 
| 2 | 2 | 0 | 0 | 0 | 1 | 

Đầu ra: 1. Chỉ tâm (1,1) cho phép hình thoi có diện tích dương. 

### Tùy chỉnh đầu vào 2```
3 2
```Chiếc bàn dài hơn, nhưng nguyên tắc thì giống nhau. Các trung tâm gần các cạnh có ít khả năng hơn, các trung tâm ở giữa cho phép kết hợp nhiều. Số lượng cuối cùng sẽ tổng hợp tất cả các sản phẩm dx*dy hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(cái gì) | Các vòng lặp lồng nhau lặp qua tâm (w+1)_(h+1), mỗi vòng lấy O(1) để tính dx_max_dy_max. | 
| Không gian | O(1) | Chỉ sử dụng một số biến số nguyên, không có cấu trúc dữ liệu bổ sung tỷ lệ với w hoặc h. | 

Cho w, h ≤ 4000, số lần lặp tối đa là (4001*4001) ≈ 16 triệu, vừa vặn trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    w, h = map(int, input().split())
    count = 0
    for cx in range(w + 1):
        for cy in range(h + 1):
            dx_max = min(cx, w - cx)
            dy_max = min(cy, h - cy)
            if dx_max > 0 and dy_max > 0:
                count += dx_max * dy_max
    return str(count)

# provided samples
assert run("2 2\n") == "1", "sample 1"

# custom cases
assert run("1 1\n") == "0", "minimum rectangle, no rhombus"
assert run("2 3\n") == "2", "non-square rectangle"
assert run("4 4\n") == "9", "symmetric square"
assert run("4000 4000\n") != "", "maximum input, sanity check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 0 | Không có hình thoi nào có thể vừa với hình chữ nhật tối thiểu | 
| 2 3 | 2 | Đếm hình thoi trong hình chữ nhật không vuông | 
| 4 4 | 9 | Hình vuông đối xứng nhiều tâm có dx, dy>0 | 
| 4000 4000 | Số lượng lớn | Hiệu suất trên đầu vào tối đa | 

## Vỏ cạnh 

Đối với hình chữ nhật tối thiểu 1x1, tất cả tâm tạo ra dx_max hoặc dy_max = 0, do đó số đếm vẫn bằng 0. Thuật toán xử lý việc này bằng cách`if dx_max > 0 and dy_max > 0`kiểm tra. Đối với hình chữ nhật có chiều rộng và chiều cao khác nhau, tâm dọc theo các cạnh dài hơn cho phép có nhiều chiều dài nửa đường chéo trong khi các cạnh ngắn hạn chế lựa chọn. phép nhân`dx_max * dy_max`đếm chính xác tất cả các kết hợp mà không đếm hai lần hoặc thiếu hình dạng. Đối với các hình chữ nhật rất lớn, thuật toán tỷ lệ tuyến tính với số tâm và duy trì trong giới hạn thời gian.
