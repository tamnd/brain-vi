---
title: "CF 139B - Giấy dán tường"
description: "Chúng ta được yêu cầu tính chi phí tối thiểu để dán giấy cho tất cả các bức tường trong một căn hộ nơi mỗi phòng là một hình lăng trụ chữ nhật. Đối với mỗi phòng, chiều dài, chiều rộng và chiều cao được đưa ra. Chu vi của căn phòng quyết định số lượng giấy dán tường cần thiết."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 139
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 99 (Div. 2)"
rating: 1600
weight: 139
solve_time_s: 87
verified: true
draft: false
---

[CF 139B - Hình nền](https://codeforces.com/problemset/problem/139/B) 

**Đánh giá:** 1600 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính chi phí tối thiểu để dán giấy cho tất cả các bức tường trong một căn hộ nơi mỗi phòng là một hình lăng trụ chữ nhật. Đối với mỗi phòng, chiều dài, chiều rộng và chiều cao được đưa ra. Chu vi của căn phòng quyết định số lượng giấy dán tường cần thiết. Mỗi loại giấy dán tường có dạng cuộn có chiều rộng và chiều dài cố định và cuộn không thể xoay được. Các dải được cắt theo chiều dọc dọc theo cuộn và mỗi phòng phải được dán một loại giấy dán tường duy nhất. Cho phép cuộn một phần nhưng không thể chia sẻ cuộn giữa các phòng. Mục tiêu của chúng tôi là chọn loại giấy dán tường và số lượng cuộn cho mỗi phòng để giảm thiểu tổng chi phí. 

Các ràng buộc rất khiêm tốn: số lượng phòng và loại hình nền nhiều nhất là 500. Kích thước và giá được giới hạn ở mức 500. Điều này cho phép các thuật toán có tối đa khoảng$O(n \cdot m)$sự phức tạp. Bởi vì tất cả các số đầu vào đều là số nguyên nhỏ nên không cần lo lắng về độ chính xác của dấu phẩy động, nhưng cần phải tính toán số nguyên cẩn thận để tránh lỗi sai một khi tính toán số dải trên mỗi cuộn. 

Trường hợp cần chú ý là khi chiều rộng cuộn lớn hơn chu vi phòng, do đó chỉ cần một cuộn là đủ. Một trường hợp khác là khi chiều dài cuộn chỉ cho phép một số lượng nhỏ dải, cần có nhiều cuộn. Ví dụ, một căn phòng có chu vi 6 mét và cao 5 mét, sử dụng một cuộn dài 4 mét và rộng 2 mét, cần tính toán cẩn thận để đảm bảo chúng ta mua hai cuộn thay vì mua nhầm một cuộn. 

## Phương pháp tiếp cận 

Cách tiếp cận mạnh mẽ rất đơn giản: đối với mỗi phòng, lặp lại từng loại giấy dán tường và tính toán số cuộn cần thiết để che phủ toàn bộ căn phòng. Nhân số cuộn với giá để có được giá thành cho sự kết hợp đó. Sau đó chọn loại giấy dán tường rẻ nhất cho căn phòng. Tính tổng các chi phí tối thiểu này cho tất cả các phòng. Điều này hiệu quả vì mỗi phòng đều độc lập; tuy nhiên, chúng ta phải tính toán cẩn thận số lượng dải trên mỗi cuộn và cần bao nhiêu cuộn. Với$n \le 500$phòng và$m \le 500$các loại hình nền, cách tiếp cận này mang lại nhiều nhất$500 \cdot 500 = 250,000$hoạt động, có thể chấp nhận được trong giới hạn thời gian 2 giây. 

Quan sát quan trọng giúp đơn giản hóa tính toán là số dải mà một cuộn có thể cung cấp là chia số nguyên của chiều dài cuộn cho chiều cao phòng và số dải cần thiết là chia số nguyên của chu vi phòng cho chiều rộng cuộn, được làm tròn lên. Với hai cách chia số nguyên này, chúng ta có thể tính toán số lượng cuộn tối thiểu cần thiết cho bất kỳ sự kết hợp loại giấy dán tường nào trong phòng. Không cần lập trình động hoặc thuật toán đồ thị vì các phòng không tương tác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n * m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các giá trị đầu vào: số phòng, kích thước phòng, số loại giấy dán tường, thông số kỹ thuật cuộn kèm theo giá. 
2. Khởi tạo một biến`total_cost`về không. 
3. Với mỗi phòng, hãy tính chu vi như sau:`2 * (length + width)`và lưu trữ chiều cao. 
4. Khởi tạo`min_cost`cho căn phòng hiện tại đến vô tận. 
5. Đối với mỗi loại giấy dán tường, hãy xác định số lượng dải dọc mà một cuộn giấy dán tường cung cấp bằng cách chia chiều dài cuộn cho chiều cao phòng. 
6. Xác định tổng số dải cần thiết cho căn phòng bằng cách chia chu vi cho chiều rộng cuộn và làm tròn lên. 
7. Tính số cuộn cần thiết làm trần nhà`required_strips / strips_per_roll`. 
8. Nhân số cuộn với giá của loại giấy dán tường để có được tổng chi phí cho loại giấy dán tường này. 
9. Cập nhật`min_cost`nếu chi phí này thấp hơn hiện tại`min_cost`. 
10. Sau khi xem xét tất cả các loại giấy dán tường cho căn phòng, hãy thêm`min_cost`ĐẾN`total_cost`. 
11. In`total_cost`. 

Tính chính xác dựa trên hai sự thật: mỗi phòng được bao phủ bởi chính xác một loại giấy dán tường và các dải giấy dán tường không thể được chia sẻ giữa các cuộn hoặc các phòng. Việc tính toán số dải và cuộn bằng số học số nguyên đảm bảo chúng ta không đánh giá thấp số cuộn cần thiết. Việc xem xét tất cả các loại giấy dán tường đảm bảo chi phí tối thiểu được tìm thấy cho từng phòng một cách độc lập, do đó, việc tổng hợp các phòng sẽ mang lại mức tối thiểu chung. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

n = int(input())
rooms = []
for _ in range(n):
    l, w, h = map(int, input().split())
    rooms.append((l, w, h))

m = int(input())
wallpapers = []
for _ in range(m):
    roll_length, roll_width, price = map(int, input().split())
    wallpapers.append((roll_length, roll_width, price))

total_cost = 0

for length, width, height in rooms:
    perimeter = 2 * (length + width)
    min_cost = float('inf')
    for roll_length, roll_width, price in wallpapers:
        strips_per_roll = roll_length // height
        if strips_per_roll == 0:
            continue
        strips_needed = (perimeter + roll_width - 1) // roll_width
        rolls_needed = (strips_needed + strips_per_roll - 1) // strips_per_roll
        cost = rolls_needed * price
        min_cost = min(min_cost, cost)
    total_cost += min_cost

print(total_cost)
```Mã sẽ đọc tất cả các phòng và hình nền trước để tránh việc xen kẽ quá trình xử lý đầu vào với quá trình tính toán. Phép chia số nguyên đảm bảo chúng tôi tính toán chính xác các dải trên mỗi cuộn và các cuộn cần thiết. Việc bổ sung`roll_width - 1`Và`strips_per_roll - 1`trước khi phép chia số nguyên mô phỏng phép toán trần, giúp tránh số học dấu phẩy động và đảm bảo tính chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
5 5 3
3
10 1 100
15 2 320
3 19 500
```| Phòng | Chu vi | Hình nền | Dải/Cuộn | Dải cần thiết | Bánh cuộn | Chi phí | Chi phí tối thiểu | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 5x5x3 | 20 | 10x1.100 | 3 | 20 | 7 | 700 | 640 | 
| | | 15x2.320 | 5 | 10 | 2 | 640 | 640 | 
| | | 3x19.500 | 1 | 20 | 2 | 1000 | 640 | 

Thuật toán chọn đúng loại hình nền thứ hai với tổng chi phí là 640. 

### Ví dụ 2 

đầu vào:```
2
3 4 2
6 5 3
2
10 2 200
6 1 100
```| Phòng | Chu vi | Hình nền | Dải/Cuộn | Dải cần thiết | Bánh cuộn | Chi phí | Chi phí tối thiểu | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 3x4x2 | 14 | 10x2.200 | 5 | 7 | 2 | 400 | 400 | 
| | | 6x1.100 | 3 | 14 | 5 | 500 | 400 | 
| 6x5x3 | 22 | 10x2.200 | 3 | 11 | 4 | 800 | 800 | 
| | | 6x1.100 | 2 | 22 | 11 | 1100 | 800 | 

Tổng chi phí = 400 + 800 = 1200. 

Điều này chứng tỏ rằng thuật toán tính toán chính xác các dải trên mỗi cuộn và các cuộn cần thiết cho nhiều phòng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m) | Đối với mỗi phòng trong số n phòng, chúng tôi kiểm tra m loại hình nền, tính toán một vài số nguyên cho mỗi lần lặp. | 
| Không gian | O(n + m) | Chúng tôi lưu trữ kích thước phòng và thông số kỹ thuật của giấy dán tường. | 

Với$n, m \le 500$, tổng số thao tác tối đa là 250.000, dễ dàng phù hợp với giới hạn thời gian 2 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    rooms = [tuple(map(int, input().split())) for _ in range(n)]
    m = int(input())
    wallpapers = [tuple(map(int, input().split())) for _ in range(m)]
    
    total_cost = 0
    for length, width, height in rooms:
        perimeter = 2 * (length + width)
        min_cost = float('inf')
        for roll_length, roll_width, price in wallpapers:
            strips_per_roll = roll_length // height
            if strips_per_roll == 0:
                continue
            strips_needed = (perimeter + roll_width - 1) // roll_width
            rolls_needed = (strips_needed + strips_per_roll - 1) // strips_per_roll
            cost = rolls_needed * price
            min_cost = min(min_cost, cost)
        total_cost += min_cost
    return str(total_cost)

# provided sample
assert run("1\n5 5 3\n3\n10 1 100\n15 2 320\n3 19 500\n") == "640", "sample 1"

# minimum input
assert run("1\n1 1 1\n1\n1 1 1\n") == "1", "minimum input"

# equal dimensions and prices
assert run("2\n2 2 2\n2 2 2\n2\n4 1 2\n4 2 3\n") ==
```
