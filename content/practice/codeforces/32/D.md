---
title: "CF 32D - Chòm sao"
description: "Chúng ta được cung cấp một lưới 2D có kích thước _n_ x _m_, mỗi ô chứa một ngôi sao * hoặc trống .. Nhiệm vụ là xác định vị trí các chòm sao có hình chữ thập."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 32
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 32 (Div. 2, Codeforces format)"
rating: 1600
weight: 32
solve_time_s: 59
verified: true
draft: false
---
[CF 32D - Chòm sao](https://codeforces.com/problemset/problem/32/D) 

**Đánh giá:** 1600 
**Thẻ:** triển khai 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới 2D có kích thước _n_ x _m_, mỗi ô chứa một ngôi sao`*`hoặc trống rỗng`.`. Nhiệm vụ là xác định vị trí các chòm sao có hình chữ thập. Một hình chữ thập được đặt ở giữa một ngôi sao và mở rộng chính xác _x_ hình vuông theo chiều dọc và chiều ngang theo cả bốn hướng, với _x_ là số nguyên dương bất kỳ sao cho tất cả năm ô đều tồn tại và chứa các ngôi sao. Chúng ta được yêu cầu tìm chòm sao thứ k theo thứ tự cụ thể: đầu tiên theo bán kính (nhỏ nhất trước), sau đó theo hàng của ngôi sao trung tâm (trên cùng trước), sau đó theo cột (ngoài cùng bên trái trước). 

Giới hạn là _n_, _m_ 300 và _k_ 30 triệu. Kích thước bản đồ ở mức vừa phải, do đó thuật toán O(n2 m2) có thể chấp nhận được, nhưng O(n m max(n,m)) sẽ bắt đầu đẩy giới hạn. Với lưới 300×300, các nỗ lực bạo lực trên mọi bán kính có thể dẫn đến hàng chục triệu lượt kiểm tra, đây là giới hạn trên của những gì chúng tôi có thể xử lý trong 2 giây. Các trường hợp cạnh bao gồm các bản đồ không có ngôi sao, giao nhau ở các cạnh, giao nhau với bán kính 1 so với bán kính lớn hơn và yêu cầu chòm sao thứ k khi tồn tại ít hơn k. 

Việc triển khai bất cẩn có thể lặp lại các đường chéo tiềm năng mà không xác thực giới hạn đầu tiên. Ví dụ: nếu ngôi sao trung tâm ở (1,1), một phép kiểm tra đơn giản có thể cố gắng truy cập (0,1) cho cánh tay trên, dẫn đến lỗi chỉ mục. Tương tự, một bản đồ chứa đầy các ngôi sao ngoại trừ ngôi sao ở giữa bị thiếu có thể đánh lừa thuật toán đếm bán kính ngây thơ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản về mặt khái niệm: đối với mỗi ngôi sao trên bản đồ, hãy thử mọi bán kính có thể bắt đầu từ 1 cho đến khi đường chéo mở rộng ra ngoài giới hạn. Đối với mỗi bán kính, hãy kiểm tra cả bốn nhánh để xem liệu các ngôi sao có tồn tại hay không. Ghi lại chòm sao nếu hợp lệ, sau đó sắp xếp tất cả các chòm sao tìm thấy theo bán kính, hàng và cột. Điều này đúng nhưng không hiệu quả. Với n = m = 300, một ngôi sao ở trung tâm có thể có bán kính lên tới 150 và việc kiểm tra từng ngôi sao cho mọi bán kính sẽ nhân các phép toán, cho ra O(n m min(n,m)) trong trường hợp xấu nhất. Với 90.000 ô và 150 lượt kiểm tra bán kính, chúng tôi đã vượt quá 10 triệu hoạt động, gần đến mức giới hạn. 

Thông tin chi tiết quan trọng là chúng ta chỉ cần biết độ dài cánh tay tối đa theo mỗi hướng đối với mỗi ngôi sao. Tính toán trước, đối với mỗi ô, số lượng sao liên tiếp theo hướng lên, xuống, trái và phải bằng cách sử dụng lập trình động: lặp từ trên xuống dưới cho mảng "lên", từ dưới lên trên cho mảng "xuống", từ trái sang phải cho "trái" và từ phải sang trái cho "phải". Bán kính chéo tối đa ở mỗi ngôi sao khi đó là giá trị tối thiểu trong bốn giá trị được tính toán trước này. Khi biết điều này, chúng tôi có thể tạo tất cả các đường chéo hợp lệ mà không cần lặp lại trên từng bán kính riêng lẻ. Điều này làm giảm việc kiểm tra dư thừa và làm cho thuật toán đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n m phút(n,m)) | O(1) | Quá chậm ở giới hạn trên | 
| Tính toán trước độ dài cánh tay | O(n m) | O(n m × 4) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới và khởi tạo bốn ma trận có kích thước n×m: lên, xuống, trái, phải. Mỗi ma trận sẽ lưu trữ các ngôi sao liên tiếp theo hướng đó bắt đầu từ ô hiện tại. 
2. Điền vào ma trận "lên" bằng cách lặp lại từng cột từ trên xuống dưới. Nếu một ô chứa dấu sao, hãy thiết lập[i][j] = up[i-1][j] + 1 (hoặc 1 nếu hàng trên cùng). Điều này đếm có bao nhiêu ngôi sao liên tiếp tồn tại hướng lên trên bao gồm cả ô hiện tại. 
3. Tương tự, điền "xuống" bằng cách lặp từ dưới lên trên, "trái" bằng cách lặp từ trái sang phải và "phải" bằng cách lặp từ phải sang trái. 
4. Đối với mỗi ô chứa một ngôi sao, hãy tính bán kính chéo tối đa của nó như sau`r = min(up[i][j], down[i][j], left[i][j], right[i][j]) - 1`. Trừ 1 vì ngôi sao trung tâm được tính theo mỗi hướng. 
5. Với mỗi bán kính từ 1 đến r, ghi một bộ giá trị`(radius, row, col)`đại diện cho một chữ thập hợp lệ. Lưu trữ tất cả các bộ dữ liệu trong một danh sách. 
6. Sắp xếp danh sách các đường chéo theo bán kính, rồi hàng, rồi cột. 
7. Nếu k vượt quá số dấu thập, ghi -1. Ngược lại, lấy bộ dữ liệu thứ k và xuất ra tọa độ của các ngôi sao ở giữa, trên, dưới, trái và phải. 

Tại sao nó hoạt động: quá trình tính toán trước đảm bảo rằng đối với mỗi ô, chúng tôi biết phạm vi tiếp cận tối đa theo cả bốn hướng. Bằng cách lấy mức tối thiểu, chúng tôi tránh tạo ra các đường chéo không hợp lệ. Việc sắp xếp đảm bảo chòm sao thứ k được yêu cầu chính xác là chòm sao thỏa mãn các quy tắc xếp hạng của bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, k = map(int, input().split())
grid = [input().strip() for _ in range(n)]

up = [[0]*m for _ in range(n)]
down = [[0]*m for _ in range(n)]
left = [[0]*m for _ in range(n)]
right = [[0]*m for _ in range(n)]

# Precompute consecutive stars
for i in range(n):
    for j in range(m):
        if grid[i][j] == '*':
            up[i][j] = 1 + (up[i-1][j] if i > 0 else 0)
            left[i][j] = 1 + (left[i][j-1] if j > 0 else 0)

for i in reversed(range(n)):
    for j in reversed(range(m)):
        if grid[i][j] == '*':
            down[i][j] = 1 + (down[i+1][j] if i < n-1 else 0)
            right[i][j] = 1 + (right[i][j+1] if j < m-1 else 0)

crosses = []
for i in range(n):
    for j in range(m):
        if grid[i][j] == '*':
            radius = min(up[i][j], down[i][j], left[i][j], right[i][j]) - 1
            for r in range(1, radius+1):
                crosses.append((r, i, j))

crosses.sort()
if k > len(crosses):
    print(-1)
else:
    r, i, j = crosses[k-1]
    print(i+1, j+1)
    print(i+1-r, j+1)
    print(i+1+r, j+1)
    print(i+1, j+1-r)
    print(i+1, j+1+r)
```Giải pháp này trước tiên tính toán trước độ giãn tối đa theo từng hướng để tránh việc kiểm tra bán kính dư thừa. Việc trừ 1 trong bán kính đảm bảo chúng ta chỉ đếm các ngôi sao nằm ngoài tâm. Sắp xếp các bộ dữ liệu`(radius, i, j)`trực tiếp tôn trọng thứ tự của vấn đề. Kiểm tra ranh giới là ngầm định vì quá trình tính toán trước dừng ở các cạnh lưới. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
5 6 1
....*.
...***
....*.
..*...
.***..
```| tôi | j | lên | xuống | trái | đúng | min-1 (bán kính) | thánh giá được ghi lại | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 1 | 1 | 1 | 1 | 0 | - | 
| 2 | 4 | 1 | 1 | 1 | 2 | 0 | - | 
| 2 | 5 | 2 | 2 | 2 | 2 | 1 | bán kính 1 chữ thập | 

Dấu vết xác nhận thuật toán tính toán chính xác bán kính và tạo ra đường chéo đầu tiên. 

Ví dụ thứ hai, trường hợp cạnh với bản đồ tối thiểu:```
3 3 1
.*.
***
.*.
```Ngôi sao trung tâm tại (2,2) có bán kính 1. Kết quả:```
2 2
1 2
3 2
2 1
2 3
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n m) | Việc tính toán trước bốn ma trận và lặp lại các phép thập là tuyến tính về số lượng ô; các đường chéo sắp xếp thêm O(n m log(n m)) nhưng được chấp nhận với n*m 90000 | 
| Không gian | O(n m) | Ma trận bốn hướng cộng với danh sách các đường chéo | 

Giải pháp phù hợp trong giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB. Việc sắp xếp không phải là nút cổ chai vì số lượng dấu thập tối đa bị giới hạn bởi tổng số sao nhân với phút(n,m). 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m, k = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    up = [[0]*m for _ in range(n)]
    down = [[0]*m for _ in range(n)]
    left = [[0]*m for _ in range(n)]
    right = [[0]*m for _ in range(n)]

    for i in range(n):
        for j in range(m):
            if grid[i][j] == '*':
                up[i][j] = 1 + (up[i-1][j] if i > 0 else
```
