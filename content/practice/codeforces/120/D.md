---
title: "CF 120D - Ba Con Trai"
description: "Chúng ta có một cánh đồng ngô hình chữ nhật được biểu diễn dưới dạng lưới có kích thước n × m, trong đó mỗi ô chứa một số tấn ngô nhất định. Người cha muốn chia thửa ruộng này cho ba người con trai sao cho mỗi người con trai nhận được chính xác một lượng ngô xác định trước: tấn A, B hoặc C."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 120
codeforces_index: "D"
codeforces_contest_name: "School Regional Team Contest, Saratov, 2011"
rating: 1400
weight: 120
solve_time_s: 95
verified: true
draft: false
---

[CF 120D - Ba đứa con trai](https://codeforces.com/problemset/problem/120/D) 

**Đánh giá:** 1400 
**Tags:** vũ lực 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cánh đồng ngô hình chữ nhật được biểu diễn dưới dạng lưới có kích thước _n_ × _m_, trong đó mỗi ô chứa một số tấn ngô nhất định. Người cha muốn chia thửa ruộng này cho ba người con trai sao cho mỗi người con trai nhận được chính xác một lượng ngô xác định trước: _A_, _B_, hoặc _C_ tấn. Việc phân chia phải được thực hiện bằng hai đường thẳng song song, ngang hoặc dọc, nằm giữa các hàng hoặc cột. Mỗi phần kết quả phải bao gồm ít nhất một hình vuông. 

Dữ liệu đầu vào bao gồm kích thước của cánh đồng, số lượng ngô trong mỗi ô và tổng mục tiêu của ba người con trai. Kết quả đầu ra là số cách đặt hai đường thẳng song song sao cho ba phần kết quả chứa các số chính xác _A_, _B_ và _C_, theo bất kỳ thứ tự nào. 

Cho rằng _n_ và _m_ đều có nhiều nhất là 50, một cách tiếp cận mạnh mẽ để kiểm tra mọi cặp đường có thể là khả thi nếu chúng ta tính tổng một cách hiệu quả. Vì tổng trong bất kỳ hình chữ nhật nào có thể được tính theo thời gian không đổi với tổng tiền tố, nên chúng ta có thể kiểm tra tất cả các phân vùng có thể có mà không vượt quá giới hạn thời gian. Trường hợp cạnh tinh vi xảy ra khi một trong các giá trị mong muốn bằng 0, đặc biệt nếu trường chứa số 0. Một trường hợp đặc biệt khác là khi tất cả các ô có giá trị bằng nhau và số lượng mong muốn giống hệt nhau, điều này có thể dẫn đến nhiều phân vùng hợp lệ có thể bị bỏ sót do các triển khai ngây thơ giả sử các tổng duy nhất. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực ngây thơ lặp đi lặp lại trên tất cả các cặp đường ngang hoặc dọc. Đối với các dòng ngang, chúng tôi thử đặt dòng đầu tiên giữa hàng 1 và _n-1_ và dòng thứ hai giữa dòng đầu tiên +1 và _n-1_. Đối với đường thẳng đứng, chúng ta thực hiện tương tự dọc theo cột. Đối với mỗi cặp dòng ứng cử viên, chúng tôi tính tổng ngô trong ba phần kết quả và kiểm tra xem chúng có khớp với nhiều tập hợp {_A_, _B_, _C_} hay không. Phương pháp này hoạt động vì trường nhỏ nhưng nếu chúng ta tính tổng của từng phần bằng cách lặp qua các ô thì độ phức tạp về thời gian sẽ trở thành O(n2·m + m2·n), không hiệu quả đối với lưới 50×50 trong trường hợp xấu nhất. 

Tối ưu hóa chính là sử dụng tổng tiền tố. Bằng cách tính toán trước mảng tổng tiền tố 2D có kích thước n×m, chúng ta có thể tính tổng của bất kỳ phần hình chữ nhật nào trong thời gian không đổi. Đối với các phân vùng ngang, tổng của các hàng r1..r2 chỉ đơn giản là chênh lệch tổng tiền tố và tương tự đối với các phân vùng dọc có tổng cột. Điều này làm giảm độ phức tạp xuống O(n2 + m2), có thể chấp nhận được vì n2 + m2 502 + 502 = 5000. Sau khi tính tổng hiệu quả, chúng ta chỉ cần kiểm tra tất cả các hoán vị của {_A_, _B_, _C_} so với tổng của ba phần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force không có tiền tố | O(n2·m + m2·n) | O(1) | Quá chậm | 
| Tối ưu hóa với tổng tiền tố | O(n2 + m2) | O(n·m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các kích thước _n_, _m_ và lưới trường. Đọc số lượng ngô mục tiêu _A_, _B_, _C_. 
2. Tính mảng tổng tiền tố 2D`ps`Ở đâu`ps[i][j]`đại diện cho tổng của hình chữ nhật từ góc trên bên trái đến ô (i,j). Công thức là`ps[i][j] = field[i][j] + ps[i-1][j] + ps[i][j-1] - ps[i-1][j-1]`. 
3. Khởi tạo bộ đếm về 0 để theo dõi các phân vùng hợp lệ. 
4. Kiểm tra phân vùng ngang. Với mỗi cặp đường`r1`Và`r2`(1 ≤ r1 < r2 < n), tính tổng ngô trong ba dải ngang bằng cách sử dụng tổng tiền tố: hàng trên cùng 0..r1-1, hàng giữa r1..r2-1, hàng dưới cùng r2..n-1. Nếu tập hợp các tổng này khớp với {_A_, _B_, _C_}, hãy tăng bộ đếm. 
5. Kiểm tra các phân vùng dọc tương tự. Đối với mỗi cặp cột`c1`Và`c2`(1 ≤ c1 < c2 < m), tính tổng ngô trong ba dải dọc bằng cách sử dụng tổng tiền tố: cột bên trái 0..c1-1, cột giữa c1..c2-1, cột bên phải c2..m-1. Nếu tập hợp các tổng này khớp với {_A_, _B_, _C_}, hãy tăng bộ đếm. 
6. In bộ đếm. 

Tại sao nó hoạt động: Mảng tổng tiền tố đảm bảo rằng mọi tổng vùng hình chữ nhật đều có thể được tính theo O(1). Bằng cách lặp lại tất cả các cặp dòng hợp lệ và kiểm tra tổng theo tất cả các hoán vị của {_A_, _B_, _C_}, chúng tôi đảm bảo rằng tất cả các phép chia hợp lệ có thể có đều được tính chính xác một lần. Mỗi phân vùng được đảm bảo chứa ít nhất một hàng hoặc cột do giới hạn vòng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
field = [list(map(int, input().split())) for _ in range(n)]
A, B, C = map(int, input().split())

# compute prefix sums
ps = [[0]*m for _ in range(n)]
for i in range(n):
    for j in range(m):
        ps[i][j] = field[i][j]
        if i > 0:
            ps[i][j] += ps[i-1][j]
        if j > 0:
            ps[i][j] += ps[i][j-1]
        if i > 0 and j > 0:
            ps[i][j] -= ps[i-1][j-1]

def rect_sum(r1, c1, r2, c2):
    res = ps[r2][c2]
    if r1 > 0:
        res -= ps[r1-1][c2]
    if c1 > 0:
        res -= ps[r2][c1-1]
    if r1 > 0 and c1 > 0:
        res += ps[r1-1][c1-1]
    return res

targets = sorted([A, B, C])
count = 0

# horizontal partitions
for r1 in range(1, n):
    for r2 in range(r1+1, n):
        s1 = rect_sum(0, 0, r1-1, m-1)
        s2 = rect_sum(r1, 0, r2-1, m-1)
        s3 = rect_sum(r2, 0, n-1, m-1)
        if sorted([s1, s2, s3]) == targets:
            count += 1

# vertical partitions
for c1 in range(1, m):
    for c2 in range(c1+1, m):
        s1 = rect_sum(0, 0, n-1, c1-1)
        s2 = rect_sum(0, c1, n-1, c2-1)
        s3 = rect_sum(0, c2, n-1, m-1)
        if sorted([s1, s2, s3]) == targets:
            count += 1

print(count)
```Tính toán tổng tiền tố đảm bảo các truy vấn hình chữ nhật có thời gian không đổi. Chúng tôi cẩn thận xử lý các ranh giới để tránh lỗi riêng lẻ. Việc sắp xếp các tổng trước khi so sánh xử lý thực tế là thứ tự của _A_, _B_, _C_ không quan trọng. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
3 3
1 1 1
1 1 1
1 1 1
3 3 3
```| r1 | r2 | s1 | s2 | s3 | trận đấu? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | 3 | 3 | vâng | 

| c1 | c2 | s1 | s2 | s3 | trận đấu? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | 3 | 3 | vâng | 

Count = 2, phù hợp với kết quả mong đợi. 

**Đầu vào tùy chỉnh**```
3 3
1 2 3
4 5 6
7 8 9
6 15 24
```Phân vùng ngang mang lại tổng`[1+2+3+4+5+6]=21`v.v. chỉ`r1=1,r2=2`sản xuất`[6, 15, 24]`sau khi sắp xếp. Phân vùng dọc cũng không tạo ra kết quả phù hợp mới. Đầu ra = 1. 

Những dấu vết này xác nhận thuật toán xem xét chính xác tất cả các vị trí dòng và sử dụng tổng tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n2 + m2) | Hai vòng lặp lồng nhau cho các vị trí dòng dọc theo hàng và cột; truy vấn tổng hình chữ nhật là O(1) | 
| Không gian | O(n·m) | Mảng tổng tiền tố 2D có kích thước n×m | 

Cho n,m 50, trong trường hợp xấu nhất là 2500 thao tác trên mỗi hướng, tổng số lần kiểm tra là ~5000, trong vòng 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    field = [list(map(int, input().split())) for _ in range(n)]
    A, B, C = map(int, input
```
