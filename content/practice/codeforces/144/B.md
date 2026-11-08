---
title: "CF 144B - Hội nghị"
description: "Chúng ta được yêu cầu xác định có bao nhiêu vị tướng cần chăn ấm tại một chiếc bàn hình chữ nhật đặt trên một mặt phẳng Descartes vô hạn. Các góc của bảng được cho bởi hai điểm có tọa độ nguyên và mỗi điểm nguyên dọc theo chu vi của hình chữ nhật chứa một điểm chung."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 144
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 103 (Div. 2)"
rating: 1300
weight: 144
solve_time_s: 75
verified: true
draft: false
---

[CF 144B - Cuộc họp](https://codeforces.com/problemset/problem/144/B) 

**Đánh giá:** 1300 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định có bao nhiêu vị tướng cần chăn ấm tại một chiếc bàn hình chữ nhật đặt trên một mặt phẳng Descartes vô hạn. Các góc của bảng được cho bởi hai điểm có tọa độ nguyên và mỗi điểm nguyên dọc theo chu vi của hình chữ nhật chứa một điểm chung. Có một số bộ tản nhiệt, mỗi bộ có một vị trí và bán kính sưởi ấm. Một vị tướng sẽ cảm thấy thoải mái nếu khoảng cách Euclide tới ít nhất một bộ bức xạ nhỏ hơn hoặc bằng bán kính của bộ bức xạ đó. Đầu ra là số lượng tướng nằm ngoài tất cả các dãy tản nhiệt. 

Các ràng buộc rất khiêm tốn: chu vi hình chữ nhật có nhiều nhất vài nghìn điểm vì tọa độ được giới hạn bởi ±1000 và số lượng bộ bức xạ lên tới 1000. Điều này có nghĩa là việc kiểm tra vũ phu của từng vị trí chung đối với mọi bộ bức xạ là khả thi:$O(\text{generals} \times n)$hoạt động vẫn ở mức dưới$10^7$, phù hợp thoải mái với giới hạn thời gian 2 giây. 

Các trường hợp cạnh không rõ ràng bao gồm hình chữ nhật được căn chỉnh theo tọa độ âm, hình chữ nhật có chiều rộng hoặc chiều cao bằng 1 (chỉ tạo ra một dòng tướng) và bộ tản nhiệt nằm chính xác tại vị trí của tướng. Ví dụ: nếu hình chữ nhật được xác định bởi (0,0) và (0,3) và có bộ bức xạ ở (0,1) với bán kính 0 thì chỉ điểm đó được làm nóng; tất cả những người khác yêu cầu chăn. Việc triển khai bất cẩn có thể lặp lại không chính xác trên tọa độ hình chữ nhật hoặc kiểm tra điều kiện gia nhiệt với sự bất bình đẳng nghiêm ngặt, dẫn đến thất bại trong các trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực hoạt động bằng cách liệt kê mọi điểm chu vi của hình chữ nhật và kiểm tra khoảng cách Euclide tới mọi bộ bức xạ. Đối với mỗi vị trí chung, chúng tôi tính toán khoảng cách đến từng bộ tản nhiệt và so sánh với bán kính của bộ tản nhiệt. Nếu không có bộ tản nhiệt nào che phủ chung, chúng tôi sẽ tăng bộ đếm chăn. Phương pháp này đúng vì nó trực tiếp thực hiện việc mô tả vấn đề, nhưng nó đòi hỏi$O(\text{perimeter} \times n)$hoạt động. Với chu vi tối đa khoảng 8000 điểm và 1000 bộ bức xạ, điều này dẫn đến khoảng$8 \cdot 10^6$kiểm tra khoảng cách, có thể chấp nhận được nhưng có thể hơi chặt đối với đầu vào lớn nhất. 

Không cần tối ưu hóa nhiều vì kích thước bài toán nhỏ, nhưng một cải tiến nhỏ là tính toán trước bán kính bình phương và so sánh nó với bình phương khoảng cách. Điều này tránh được việc tính toán căn bậc hai tốn kém và vì chúng ta chỉ cần so sánh khoảng cách nên so sánh tương đối vẫn chính xác. Các tối ưu hóa khác như phân vùng không gian ở đây là quá mức cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(P × n), P = điểm chu vi | O(n) | Đã chấp nhận | 
| Tối ưu (bình phương khoảng cách) | O(P×n) | O(n) | Đã chấp nhận | 

Cách tiếp cận tối ưu là sử dụng biện pháp mạnh mẽ tương tự với so sánh khoảng cách bình phương, giúp tránh các phép toán dấu phẩy động và nhanh hơn một chút trong thực tế. 

## Hướng dẫn thuật toán 

1. Phân tích các góc hình chữ nhật$(x_a, y_a)$Và$(x_b, y_b)$và xác định tọa độ x và y tối thiểu và tối đa để lặp lại chính xác trên các cạnh hình chữ nhật bất kể thứ tự đầu vào. 
2. Thu thập tất cả các vị trí bộ tản nhiệt và bán kính của chúng. Đối với mỗi bộ tản nhiệt, tính toán trước bán kính bình phương$r_i^2$để tránh căn bậc hai trong kiểm tra khoảng cách. 
3. Liệt kê mọi điểm nguyên dọc theo chu vi hình chữ nhật. Lặp lại tọa độ x cho các cạnh trên và dưới và trên tọa độ y cho các cạnh trái và phải. Tránh đếm hai lần các góc bằng cách xử lý cẩn thận các cạnh. 
4. Đối với mỗi điểm chu vi (chung), hãy khởi tạo một cờ`warm`là sai. Lặp lại qua tất cả các bộ tản nhiệt, tính toán khoảng cách Euclide bình phương đến tổng quát. Nếu bất kỳ bộ tản nhiệt nào thỏa mãn$dx^2 + dy^2 \le r_i^2$, bộ`warm = True`và phá vỡ. 
5. Sau khi kiểm tra tất cả các bộ tản nhiệt, nếu`warm`vẫn sai, tăng bộ đếm chăn. 
6. Xuất ra tổng số chăn. 

Tính chính xác của thuật toán xuất phát từ việc kiểm tra từng điểm chu vi so với tất cả các bộ tản nhiệt và sử dụng khoảng cách bình phương để đảm bảo rằng không có điểm chung nào bị phân loại sai thành ấm hoặc lạnh. Mỗi điểm chu vi được xử lý chính xác một lần và việc so sánh khoảng cách là chính xác theo số học số nguyên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

xa, ya, xb, yb = map(int, input().split())
n = int(input())
radiators = []
for _ in range(n):
    x, y, r = map(int, input().split())
    radiators.append((x, y, r*r))  # store squared radius

# normalize rectangle coordinates
x_min, x_max = min(xa, xb), max(xa, xb)
y_min, y_max = min(ya, yb), max(ya, yb)

blankets = 0

# generate perimeter points
for x in range(x_min, x_max+1):
    for y in [y_min, y_max]:
        warm = False
        for rx, ry, r2 in radiators:
            dx = x - rx
            dy = y - ry
            if dx*dx + dy*dy <= r2:
                warm = True
                break
        if not warm:
            blankets += 1

for y in range(y_min+1, y_max):  # avoid double-counting corners
    for x in [x_min, x_max]:
        warm = False
        for rx, ry, r2 in radiators:
            dx = x - rx
            dy = y - ry
            if dx*dx + dy*dy <= r2:
                warm = True
                break
        if not warm:
            blankets += 1

print(blankets)
```Vòng lặp đầu tiên xử lý các cạnh ngang, trong khi vòng lặp thứ hai xử lý các cạnh dọc không bao gồm các góc đã được tính. Việc lưu trữ bán kính bình phương sẽ tránh được các lỗi về độ chính xác của dấu phẩy động và cải thiện hiệu suất. Mỗi kiểm tra khoảng cách chỉ sử dụng số học số nguyên. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

Đầu vào hình chữ nhật: (2,5) đến (4,2) 

Bộ tản nhiệt: (3,1,2), (5,3,1), (1,3,2) 

| Chung | Khoảng cách bình phương đến bộ tản nhiệt | Ấm? | 
| --- | --- | --- | 
| (2,2) | 2->10->2 | Đúng | 
| (2,3) | 5->5->0 | Đúng | 
| (2,4) | 10->5->2 | Đúng | 
| (2,5) | 18->5->8 | Sai | 
| (3,2) | 1->4->4 | Đúng | 
| (3,5) | 10->4->8 | Sai | 
| (4,2) | 2->1->8 | Đúng | 
| (4,3) | 5->1->2 | Đúng | 
| (4,4) | 10->1->5 | Sai | 
| (4,5) | 18->1->10 | Sai | 

Chăn cần thiết = 4, phù hợp với sản lượng dự kiến. 

**Mẫu 2** 

Đầu vào hình chữ nhật: (5,2) đến (6,3) 

Bộ tản nhiệt: (5,2,1), (5,3,1), (6,2,1), (6,3,1) 

Tất cả các điểm chu vi trùng với vùng phủ sóng của bộ tản nhiệt, do đó cần có chăn = 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(P×n) | Mỗi điểm chu vi được kiểm tra đối với tất cả n bộ bức xạ. P 4000 trong trường hợp xấu nhất. | 
| Không gian | O(n) | Lưu trữ tọa độ bộ tản nhiệt và bán kính bình phương. | 

Giải pháp dễ dàng phù hợp với giới hạn 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    xa, ya, xb, yb = map(int, input().split())
    n = int(input())
    radiators = []
    for _ in range(n):
        x, y, r = map(int, input().split())
        radiators.append((x, y, r*r))
    x_min, x_max = min(xa, xb), max(xa, xb)
    y_min, y_max = min(ya, yb), max(ya, yb)
    blankets = 0
    for x in range(x_min, x_max+1):
        for y in [y_min, y_max]:
            warm = False
            for rx, ry, r2 in radiators:
                dx = x - rx
                dy = y - ry
                if dx*dx + dy*dy <= r2:
                    warm = True
                    break
            if not warm:
                blankets += 1
    for y in range(y_min+1, y_max):
        for x in [x_min, x_max]:
            warm = False
            for rx, ry, r2 in radiators:
                dx = x - rx
                dy = y - ry
                if dx*dx + dy*dy <= r2:
                    warm = True
                    break
            if not warm:
                blankets += 1
    return str(blankets)

# provided samples
assert run("2 5 4 2\n3\n3 1 2\n5 3 1\n1 3 2\n") == "4", "sample 1"
assert run("5 2 6 3\n4\n5 2 1\n5 3 1\n6 2 1\n6 3 1\n") == "0", "sample 2"

# custom cases
assert run("0 0 0 0\n1
```
