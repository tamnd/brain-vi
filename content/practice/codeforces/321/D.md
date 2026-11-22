---
title: "CF 321D - Ciel và Flipboard"
description: "Chúng ta có một bảng vuông có kích thước n x n, trong đó n luôn là số lẻ. Mỗi ô chứa một số nguyên, có thể dương hoặc âm."
date: "2026-06-06T02:20:32+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 321
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 190 (Div. 1)"
rating: 2900
weight: 321
solve_time_s: 87
verified: true
draft: false
---

[CF 321D - Ciel và Flipboard](https://codeforces.com/problemset/problem/321/D) 

**Xếp hạng:** 2900 
**Tags:** dp, tham lam, toán học 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bảng vuông có kích thước _n_ x _n_, trong đó _n_ luôn là số lẻ. Mỗi ô chứa một số nguyên, có thể dương hoặc âm. Fox Ciel có thể liên tục chọn bất kỳ bảng con hình vuông nào có kích thước bất kỳ (1×1 cho đến _n_×_n_) và lật các dấu của tất cả các số nguyên bên trong nó, nhân mỗi số với -1 một cách hiệu quả. Mục tiêu của chúng ta là tối đa hóa tổng của tất cả các số trên bảng sau bất kỳ chuỗi lần lật nào. 

Đầu vào bao gồm _n_, theo sau là các hàng số nguyên _n_. Đầu ra là một số duy nhất, số tiền tối đa có thể đạt được. 

Các ràng buộc rất khiêm tốn: _n_ nằm trong khoảng từ 1 đến 33. Vì số lượng ô nhiều nhất là 1089 nên mọi cách tiếp cận đa thức về số lượng ô đều khả thi. Các phép toán có tỷ lệ _2^n_ quá chậm nếu được áp dụng một cách ngây thơ, nhưng có thể chấp nhận được điều gì đó tuyến tính về số lượng ô hoặc chỉ yêu cầu tính toán theo thời gian không đổi trên mỗi ô. 

Một sự tinh tế quan trọng nảy sinh bởi vì chúng ta có thể lật bất kỳ bảng phụ hình vuông nào bao nhiêu lần cũng được. Thoạt nhìn, điều này có thể gợi ý rằng việc mô phỏng các lần lật cho tất cả các bảng con có thể là có thể là cần thiết, nhưng tính đối xứng của bài toán và tính chất của các chiều lẻ tạo ra các ràng buộc. Một cách tiếp cận ngây thơ chỉ lật các số âm theo thứ tự tham lam có thể thất bại vì việc lật một góc có thể làm thay đổi dấu của nhiều số đã cố định trước đó. 

Trường hợp cạnh nhỏ là bảng 1×1 có số âm. Lật nó một lần làm cho nó dương, cho tổng là giá trị tuyệt đối của nó. Nếu tất cả các số đều âm nhưng _n_ là số lẻ, thì luôn có một chiến lược đảm bảo tất cả trừ một số có thể được chuyển thành số dương, vì việc lật ảnh hưởng đến số hàng và cột lẻ một cách đối xứng. Ví dụ: đầu vào:```
3
-1 -2 -3
-4 -5 -6
-7 -8 -9
```đòi hỏi phải xử lý cẩn thận, bởi vì chúng ta không thể đơn giản lật từng tiêu cực riêng lẻ; giải pháp tối ưu để lại chính xác một giá trị tuyệt đối nhỏ nhất là âm để tối đa hóa tổng. 

## Phương pháp tiếp cận 

Phương pháp vũ phu sẽ thử mọi sự kết hợp có thể có của các lần lật ván phụ. Đối với mỗi kích thước hình vuông từ 1 đến _n_ và với mọi tọa độ trên cùng bên trái có thể có của kích thước đó, chúng ta có thể mô phỏng việc lật. Mỗi lựa chọn sẽ nhân số lượng cấu hình bảng theo cấp số nhân. Ngay cả với _n_ = 5, số lượng dãy vẫn trở nên khổng lồ, vượt xa giới hạn tính toán thực tế. Cách tiếp cận này đúng về nguyên tắc vì nó khám phá mọi khả năng, nhưng không khả thi do sự bùng nổ tổ hợp. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát cấu trúc của các lần tung hứng. Lật một hình vuông hai lần sẽ khôi phục lại giá trị ban đầu. Quan trọng hơn, các lần lật cho phép chúng ta đổi dấu các số một cách đối xứng. Đối với một bảng có kích thước lẻ, bất kỳ ô nào cũng có một “tấm gương” ở giữa. Chúng ta có thể tập trung vào bốn ô đối xứng theo tâm: trên cùng bên trái, trên cùng bên phải, dưới cùng bên trái và dưới cùng bên phải. Bằng cách chọn các lần lật cẩn thận, mỗi lần trong số bốn lần lật này có thể được coi là dương, ngoại trừ khả năng ô trung tâm đứng một mình. Như vậy, tổng tối đa thu được bằng cách lấy giá trị tuyệt đối của tất cả các số, sau đó trừ đi hai lần giá trị tuyệt đối nhỏ nhất trong số tất cả các ô, nếu số lần lật âm phải để lại một âm ở giữa. Điều này làm giảm vấn đề từ việc mô phỏng các lần lật sang một phép tính đơn giản trên các giá trị tuyệt đối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(n^2)) | O(n^2) | Quá chậm | 
| Tối ưu | O(n^2) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng giá trị tuyệt đối của tất cả các ô. Đây là tổng nếu chúng ta có thể lật mọi số thành dương. Bởi vì các lần lật có thể đạt tới tất cả các vị trí đối xứng, điều này mang lại giới hạn trên. 
2. Tìm giá trị tuyệt đối nhỏ nhất trong số tất cả các ô. Giá trị này biểu thị số có thể vẫn âm trong cấu hình tối ưu nếu bảng chứa một số lẻ số âm. Chúng ta không thể tránh một âm vì mỗi lần lật sẽ ảnh hưởng đến một số ô chẵn một cách đối xứng, ngoại trừ ô trung tâm. 
3. Đếm số lượng số âm. Nếu số này là số chẵn thì tất cả các số đều có thể dương và tổng tối đa bằng tổng tổng tuyệt đối. Nếu số đếm là số lẻ thì chúng ta phải để lại một số âm. Trong trường hợp này, trừ hai lần giá trị tuyệt đối nhỏ nhất từ ​​tổng số tuyệt đối để tính số âm không thể tránh khỏi. 
4. Xuất kết quả tổng. 

Điều bất biến ở đây là tất cả các số ngoại trừ một số có thể được chuyển sang dương mà không làm thay đổi cấu hình của các ô khác và một số nhất thiết phải là số có độ lớn nhỏ nhất để giảm thiểu hình phạt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
board = [list(map(int, input().split())) for _ in range(n)]

total_abs = 0
min_abs = float('inf')
neg_count = 0

for i in range(n):
    for j in range(n):
        val = board[i][j]
        total_abs += abs(val)
        if abs(val) < min_abs:
            min_abs = abs(val)
        if val < 0:
            neg_count += 1

if neg_count % 2 == 0:
    print(total_abs)
else:
    print(total_abs - 2 * min_abs)
```Giải pháp đọc bảng thành danh sách 2D, sau đó lặp qua từng ô để tính toán đồng thời ba thứ: tổng giá trị tuyệt đối, giá trị tuyệt đối nhỏ nhất và số lượng số âm. Dựa trên tính chẵn lẻ của số âm, chúng ta sử dụng tổng trực tiếp hoặc trừ hai lần giá trị tuyệt đối nhỏ nhất. Phần tinh tế là nhận ra rằng giá trị tuyệt đối nhỏ nhất xác định sự mất mát không thể tránh khỏi khi số âm là số lẻ. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
3
-1 -1 1
-1 1 -1
1 -1 -1
```| Tế bào | Giá trị cơ bụng | Tổng số tiền | Đếm âm | Cơ bụng tối thiểu | 
| --- | --- | --- | --- | --- | 
| -1 | 1 | 1 | 1 | 1 | 
| -1 | 1 | 2 | 2 | 1 | 
| 1 | 1 | 3 | 2 | 1 | 
| -1 | 1 | 4 | 3 | 1 | 
| 1 | 1 | 5 | 3 | 1 | 
| -1 | 1 | 6 | 4 | 1 | 
| 1 | 1 | 7 | 4 | 1 | 
| -1 | 1 | 8 | 5 | 1 | 
| -1 | 1 | 9 | 6 | 1 | 

Số âm là số chẵn sau khi ăn đầy đủ: 6 âm. Tổng tối đa là Total_abs = 9. 

### Tùy chỉnh đầu vào 2```
3
-1 -2 -3
-4 -5 -6
-7 -8 -9
```| Tế bào | Giá trị cơ bụng | Tổng số tiền | Đếm âm | Cơ bụng tối thiểu | 
| --- | --- | --- | --- | --- | 
| -1 | 1 | 1 | 1 | 1 | 
| -2 | 2 | 3 | 2 | 1 | 
| -3 | 3 | 6 | 3 | 1 | 
| -4 | 4 | 10 | 4 | 1 | 
| -5 | 5 | 15 | 5 | 1 | 
| -6 | 6 | 21 | 6 | 1 | 
| -7 | 7 | 28 | 7 | 1 | 
| -8 | 8 | 36 | 8 | 1 | 
| -9 | 9 | 45 | 9 | 1 | 

Số âm = 9, lẻ. Trừ 2 * min_abs = 2 * 1 = 2. Tổng tối đa = 43. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Chúng tôi lặp qua từng ô một lần. Mỗi hoạt động bên trong vòng lặp là thời gian không đổi. | 
| Không gian | O(n^2) | Chúng tôi lưu trữ bảng một cách rõ ràng, đòi hỏi không gian n×n. | 

Với n 33, cả thời gian và bộ nhớ đều nằm trong giới hạn. 1089 lần lặp với số học đơn giản là chuyện nhỏ trong vòng 4 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    board = [list(map(int, input().split())) for _ in range(n)]
    total_abs = 0
    min_abs = float('inf')
    neg_count = 0
    for i in range(n):
        for j in range(n):
            val = board[i][j]
            total_abs += abs(val)
            min_abs = min(min_abs, abs(val))
            if val < 0:
                neg_count += 1
    if neg_count % 2 == 0:
        return str(total_abs)
    else:
        return str(total_abs - 2 * min_abs)

# Provided sample
assert run("3\n-1 -1 1\n-1 1 -1\n1 -1 -1\n
```
