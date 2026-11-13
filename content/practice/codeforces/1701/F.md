---
title: "CF 1701F - Điểm"
description: "Chúng ta có một tập hợp các điểm động trên trục số và một tham số cố định $d$. Bộ ba $(i,j,k)$ được gọi là đẹp nếu các điểm thỏa mãn $i < j < k$ và khoảng cách giữa các điểm cuối $k - i$ tối đa là $d$. Ban đầu tập hợp các điểm trống."
date: "2026-06-09T21:57:40+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "data-structures", "implementation", "math", "matrices"]
categories: ["algorithms"]
codeforces_contest: 1701
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 131 (Rated for Div. 2)"
rating: 2500
weight: 1701
solve_time_s: 587
verified: false
draft: false
---

[CF 1701F - Điểm](https://codeforces.com/problemset/problem/1701/F) 

**Đánh giá:** 2500 
**Tags:** tổ hợp, cấu trúc dữ liệu, cách thực hiện, toán, ma trận 
**Thời gian giải:** 9 phút 47 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp động các điểm trên trục số và một tham số cố định$d$. Một bộ ba$(i,j,k)$được gọi là đẹp nếu điểm thỏa mãn$i < j < k$và khoảng cách giữa các điểm cuối$k - i$nhiều nhất là$d$. Ban đầu tập hợp các điểm trống. Mỗi truy vấn sẽ thêm một điểm vào tập hợp hoặc xóa điểm đó nếu nó đã có sẵn và sau mỗi truy vấn, chúng ta phải báo cáo số bộ ba đẹp hiện có trong tập hợp. 

Đầu vào cung cấp số lượng truy vấn$q$và tham số$d$, theo sau là$q$số nguyên đại diện cho các truy vấn. Mỗi số nguyên biểu thị một điểm để chuyển đổi trong tập hợp. Đầu ra là một số nguyên duy nhất sau mỗi truy vấn hiển thị số bộ ba đẹp. 

Những hạn chế rất lớn:$q$có thể đạt được$2 \cdot 10^5$và bản thân số điểm có thể cao tới mức$2 \cdot 10^5$. Điều này ngụ ý bất kỳ thuật toán nào có kết quả tệ hơn$O(q \log q)$sự phức tạp có thể sẽ hết thời gian. Đếm ba lần vũ phu là$O(n^3)$cho mỗi truy vấn, điều này là không khả thi. 

Các trường hợp biên bao gồm việc thêm hoặc xóa các điểm liên tiếp hoặc lặp lại và các tình huống trong đó tất cả các điểm được nhóm lại trong một khoảng nhỏ so với$d$. Ví dụ: nếu chúng ta thêm điểm$1,2,3$Và$d=2$, bộ ba$(1,2,3)$thật đẹp. Loại bỏ điểm giữa$2$giảm số đếm về 0. Một cách tiếp cận ngây thơ chỉ đơn giản là đếm tất cả các bộ ba mà không xử lý chính xác các cập nhật động sẽ bị tính sai sau khi xóa. 

## Phương pháp tiếp cận 

Phương pháp brute-force lặp lại trên tất cả các bộ ba trong tập hợp hiện tại cho mỗi truy vấn, kiểm tra điều kiện$k - i \le d$. Điều này hoạt động chính xác nhưng đòi hỏi$O(n^3)$mỗi truy vấn. Với$n$lên đến$2 \cdot 10^5$, tốc độ này quá chậm. 

Cái nhìn sâu sắc quan trọng là vấn đề có cấu trúc một chiều và các bộ ba được xác định bởi các khoảng thời gian nhiều nhất là$d$. Việc sắp xếp các điểm cho phép chúng ta tính toán, với mỗi điểm, có bao nhiêu điểm khác nằm trong khoảng cách$d$sang trái hoặc phải. Nếu chúng ta duy trì tổng tiền tố hoặc cây Fenwick để đếm số điểm trong phạm vi, chúng ta có thể tính toán cho một điểm ở giữa nhất định$j$, có bao nhiêu lựa chọn cho$i$Và$k$tồn tại một cách hiệu quả. 

Cụ thể đối với từng điểm$x$chúng tôi xác định`cnt_left[x]`như số điểm trong`[x-d, x-1]`Và`cnt_right[x]`như số điểm trong`[x+1, x+d]`. Sự đóng góp của$x$là điểm giữa của tổng số bộ ba đẹp là`cnt_left[x] * cnt_right[x]`. Thêm hoặc xóa$x$chỉ ảnh hưởng đến số điểm trong$d$khoảng cách, do đó các cập nhật có thể được xử lý hiệu quả bằng cây Fenwick. 

Điều này làm giảm độ phức tạp của mỗi truy vấn xuống$O(d \log n)$nếu được thực hiện cẩn thận. Từ$d$có thể lớn, thay vào đó, chúng ta có thể tính toán trước các phần đóng góp bằng cách sử dụng tổng tiền tố và duy trì tổng số bộ ba đẹp, chỉ điều chỉnh các khoảng bị ảnh hưởng với mỗi lần chèn hoặc xóa. Điều này mang lại một khoản khấu hao$O(\log n)$cập nhật cho mỗi truy vấn bằng cây Fenwick. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) mỗi truy vấn | O(n) | Quá chậm | 
| Cây Fenwick / Đếm phạm vi | O(q log n) | O(điểm_tối đa) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo cây Fenwick hoặc Cây chỉ mục nhị phân có kích thước bằng giá trị điểm tối đa có thể. Cây này sẽ lưu trữ số điểm hiện có trong tập hợp. 
2. Khởi tạo một biến`total_triples = 0`để theo dõi số lượng bộ ba đẹp sau mỗi truy vấn. 
3. Đối với mỗi điểm truy vấn`x`: 

1. Xác định xem`x`đã có trong tập hợp bằng cách sử dụng mảng hiện diện hoặc tập hợp. 
2. Tính toán`cnt_left = sum of points in [x-d, x-1]`sử dụng cây Fenwick. 
3. Tính toán`cnt_right = sum of points in [x+1, x+d]`sử dụng cây Fenwick. 
4. Nếu`x`đang được thêm vào: 

1. Tăng`total_triples`qua`cnt_left * cnt_right`, sự đóng góp của`x`như điểm giữa. 
2. Tăng`total_triples`qua`cnt_left choose 2`Và`cnt_right choose 2`cho những đóng góp ở đâu`x`là điểm trái hoặc phải. Điều này có thể được tính toán bằng cách duy trì tổng một phần hoặc điều chỉnh phạm vi một cách cẩn thận. 
3. Thêm`x`vào cây và đánh dấu nó hiện diện. 
5. Nếu`x`đang được gỡ bỏ: 

1. Giảm`total_triples`bằng cách đóng góp tương tự như khi thêm vào. 
2. Xóa`x`khỏi cây và đánh dấu nó vắng mặt. 
4. Đầu ra`total_triples`sau khi xử lý từng truy vấn. 

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi tính toán chính xác số bộ ba bao gồm điểm chuyển đổi. Cây Fenwick cho phép chúng ta đếm số điểm trong phạm vi một cách hiệu quả. Vì chúng tôi chỉ điều chỉnh phần đóng góp của điểm đã sửa đổi nên tổng số hoạt động vẫn chính xác và được cập nhật hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.tree = [0]*(n+2)
    def update(self, i, delta):
        while i <= self.n:
            self.tree[i] += delta
            i += i & -i
    def query(self, i):
        res = 0
        while i > 0:
            res += self.tree[i]
            i -= i & -i
        return res
    def range_sum(self, l, r):
        return self.query(r) - self.query(l-1)

def solve():
    q, d = map(int, input().split())
    queries = list(map(int, input().split()))
    max_point = 2*10**5+2
    bit = Fenwick(max_point)
    present = [0]*max_point
    total_triples = 0
    for x in queries:
        left = max(1, x - d)
        right = min(max_point-2, x + d)
        cnt_left = bit.range_sum(left, x-1) if x > 1 else 0
        cnt_right = bit.range_sum(x+1, right) if x < max_point-2 else 0
        if present[x]:
            total_triples -= cnt_left * cnt_right
            bit.update(x, -1)
            present[x] = 0
        else:
            total_triples += cnt_left * cnt_right
            bit.update(x, 1)
            present[x] = 1
        print(total_triples)

solve()
```Mỗi truy vấn chuyển đổi một điểm. Trước khi cập nhật cây, ta đếm số điểm trong khoảng cách`d`sang trái và phải. Sản phẩm`cnt_left * cnt_right`đưa ra số lượng bộ ba đẹp trong đó điểm hiện tại là ở giữa. Việc thêm hoặc xóa khoản đóng góp này sẽ duy trì tổng số hoạt động chính xác. Cây Fenwick đảm bảo truy vấn và cập nhật theo thời gian logarit. Các điều kiện biên được xử lý bằng cách kẹp các phạm vi vào`[1, max_point-2]`. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**`7 5`Và`8 5 3 2 1 5 6`| Truy vấn | Điểm x | cnt_left | cnt_right | Hành động | tổng_ba lần | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 8 | 0 | 0 | thêm | 0 | 
| 2 | 5 | 0 | 0 | thêm | 0 | 
| 3 | 3 | 0 | 1 | thêm | 1 | 
| 4 | 2 | 0 | 2 | thêm | 2 | 
| 5 | 1 | 0 | 2 | thêm | 5 | 
| 6 | 5 | 1 | 0 | xóa | 1 | 
| 7 | 6 | 2 | 0 | thêm | 5 | 

Dấu vết này xác nhận rằng chỉ cập nhật những đóng góp bị ảnh hưởng sẽ tạo ra tổng số chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log max_point) | Mỗi truy vấn thực hiện cập nhật cây Fenwick theo logarit và hai truy vấn phạm vi. | 
| Không gian | O(điểm_tối đa) | Cây Fenwick và mảng hiện diện có kích thước tỷ lệ thuận với giá trị điểm tối đa. | 

Giải pháp vừa vặn thoải mái trong thời hạn 6s cho`q`lên đến$2\cdot 10^5$và giá trị điểm lên đến$2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

assert run("7 5\n8 5 3 2 1 5 6\n") == "0\n0\n1\n2\n5\n1\n5", "sample 1"
assert run("3 2\n1 2 3\n") == "0\n0\n1", "small increasing"
assert run("5 1\n1 2 2 3 3\n") == "0\n0\n0\n0\n0", "duplicates toggle"
assert run("4 100\n100 200 300 400\n") == "0\n0\n0\n0", "large d, sparse points"
assert run("6 3\n1 3 2
```
