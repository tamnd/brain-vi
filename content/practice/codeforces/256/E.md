---
title: "CF 256E - Mảng may mắn"
description: "Chúng ta được cho một mảng có độ dài n, ban đầu chứa đầy các số 0. Mỗi truy vấn đặt một phần tử duy nhất thành giá trị từ 0 đến 3. Số 0 đại diện cho phần tử chưa được đặt, trong khi 1, 2 hoặc 3 là giá trị thực."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 256
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 156 (Div. 1)"
rating: 2400
weight: 256
solve_time_s: 102
verified: false
draft: false
---

[CF 256E - Mảng may mắn](https://codeforces.com/problemset/problem/256/E) 

**Đánh giá:** 2400 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài`n`, ban đầu chứa đầy số không. Mỗi truy vấn đặt một phần tử duy nhất thành giá trị từ 0 đến 3. Số 0 đại diện cho phần tử chưa được đặt, trong khi 1, 2 hoặc 3 là giá trị thực. Sau mỗi truy vấn, chúng ta được yêu cầu đếm số cách có thể thay thế tất cả các số 0 bằng các giá trị từ 1 đến 3 để mỗi cặp phần tử liên tiếp tạo thành một cặp "tốt", theo ma trận 3×3 cho trước`w`. Ma trận chỉ định các cặp thứ tự (i, j) được phép. 

Mảng được lập chỉ mục từ 1. Các truy vấn có tính tuần tự và mỗi truy vấn sẽ thay đổi mảng hiện tại trước khi tính toán câu trả lời. Câu trả lời phải được lấy theo modulo 777777777. 

Những hạn chế`n ≤ 77777`Và`m ≤ 77777`ngụ ý rằng bất kỳ cách tiếp cận ngây thơ nào lặp qua tất cả các kết hợp có thể có cho số 0 sau mỗi truy vấn đều không khả thi, bởi vì mỗi số 0 có thể nhận 3 giá trị. Đối với một mảng thậm chí có 10 số 0, số lượng vũ phu sẽ là 3^10 = 59049 kết hợp. Vì có thể có hàng chục nghìn số 0 và truy vấn nên chúng ta phải tránh tính toán lại từ đầu sau mỗi truy vấn. 

Các trường hợp cạnh phát sinh khi các số 0 liên tiếp hoặc ở ranh giới của mảng. Ví dụ, nếu`n = 3`, tất cả các số 0 và mọi cặp (i, j) đều tốt ngoại trừ (2, 2), khi đó việc điền các số 0 bằng tất cả các số 2 sẽ không thành công, mặc dù việc đếm ngây thơ có thể bao gồm nó không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lấp đầy mọi số 0 bằng tất cả các giá trị có thể và kiểm tra tính hợp lệ của từng mảng sau mỗi truy vấn. Điều này đúng vì nó liệt kê tất cả các khả năng theo nghĩa đen, nhưng nó trở thành O(3^n * n * m) trong trường hợp xấu nhất, điều này hoàn toàn không thực tế đối với n ~ 10^5. 

Thông tin chi tiết quan trọng là mỗi cặp phần tử liên tiếp có thể được mô hình hóa độc lập khi bạn biết phần tử bên trái. Nếu chúng ta định nghĩa`dp[i][v]`là số lượng chuỗi hợp lệ kết thúc ở vị trí`i`có giá trị`v`, chúng ta có thể truyền bá điều này chỉ bằng cách sử dụng vị trí trước đó và ma trận cặp được phép`w`. Điều này làm giảm vấn đề thành vấn đề lập trình động dọc theo mảng, nhưng chúng tôi cần cập nhật nhanh vì mỗi truy vấn chỉ thay đổi một phần tử duy nhất. 

Cách tiếp cận tối ưu sử dụng cây phân đoạn trong đó mỗi nút duy trì ma trận 3 × 3 biểu thị số cách để điền vào phân khúc tương ứng với các giá trị cụ thể tại ranh giới phân khúc. Mỗi lá tương ứng với một phần tử mảng. Khi một truy vấn thay đổi một phần tử, chúng tôi chỉ cập nhật lá tương ứng và truyền các thay đổi lên trên. Nhân các ma trận dọc theo cây sẽ tổng hợp số lượng một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^n * n * m) | O(n) | Quá chậm | 
| Cây phân đoạn + Ma trận DP | O(m * log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo cây phân đoạn có kích thước`n`, trong đó mỗi nút lá lưu trữ một ma trận 3×3 biểu thị số lượng có thể có cho mỗi giá trị tại vị trí đó. Nếu phần tử mảng bằng 0, ma trận là đồng nhất cho 1, 2, 3 (có thể có cả ba giá trị); nếu nó cố định, ma trận có một hàng và cột duy nhất có giá trị đó là 1 và các số 0 ở nơi khác. 
2. Mỗi nút bên trong lưu trữ tích của hai ma trận con của nó. Sản phẩm được xác định sao cho nếu đoạn bên trái kết thúc bằng giá trị`x`và đoạn bên phải bắt đầu bằng giá trị`y`, tổng số chuỗi hợp lệ cho phân đoạn kết hợp là tổng của tất cả`x, y`Ở đâu`(x, y)`được cho phép bởi`w`và nhân với số lượng tương ứng từ các nút con. 
3. Đối với mỗi truy vấn, hãy cập nhật nút lá tương ứng với giá trị mới, đặt lại giá trị đó về 0 (cho phép tất cả các giá trị) hoặc về giá trị cụ thể. 
4. Sau khi cập nhật lá, truyền các thay đổi lên gốc. Ma trận gốc bây giờ mã hóa tổng số cách cho toàn bộ mảng. Tổng hợp tất cả các giá trị bắt đầu và kết thúc hợp lệ sẽ cho câu trả lời theo modulo 777777777. 
5. Lặp lại cho tất cả các truy vấn, xuất ra tổng cho mỗi truy vấn. 

Tại sao nó hoạt động: Mỗi nút duy trì một bất biến: ma trận 3 × 3 mã hóa đầy đủ số lượng phần lấp đầy hợp lệ của phân đoạn của nó được điều chỉnh ở các điểm cuối bên trái và bên phải. Việc kết hợp hai phân đoạn bằng phép nhân ma trận sẽ tổng hợp chính xác số lượng trên ranh giới vì chúng tôi chỉ tính tổng các kết hợp hợp lệ theo`w`. Việc cập nhật một chiếc lá sẽ phản ánh chính xác mọi thay đổi ở vị trí đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 777777777

class SegmentTree:
    def __init__(self, n, good):
        self.n = n
        self.size = 1
        while self.size < n:
            self.size *= 2
        # each node stores 3x3 dp matrix
        self.data = [None] * (2*self.size)
        self.good = good
        for i in range(2*self.size):
            self.data[i] = [[0]*3 for _ in range(3)]
    
    def set_leaf(self, i, val):
        mat = [[0]*3 for _ in range(3)]
        if val == 0:
            for x in range(3):
                mat[x][x] = 1
        else:
            mat[val-1][val-1] = 1
        self.data[i+self.size] = mat
    
    def merge(self, left, right):
        res = [[0]*3 for _ in range(3)]
        for i in range(3):
            for j in range(3):
                for k in range(3):
                    for l in range(3):
                        if self.good[k][l]:
                            res[i][l] = (res[i][l] + left[i][k]*right[l][l]) % MOD
        return res
    
    def build(self):
        for i in range(self.size-1, 0, -1):
            self.data[i] = self.combine(self.data[i*2], self.data[i*2+1])
    
    def combine(self, left, right):
        res = [[0]*3 for _ in range(3)]
        for i in range(3):
            for j in range(3):
                total = 0
                for k in range(3):
                    for l in range(3):
                        if self.good[k][l]:
                            total = (total + left[i][k]*right[l][j]) % MOD
                res[i][j] = total
        return res
    
    def update(self, i, val):
        self.set_leaf(i, val)
        i += self.size
        while i > 1:
            i //= 2
            self.data[i] = self.combine(self.data[2*i], self.data[2*i+1])
    
    def total(self):
        ans = 0
        for i in range(3):
            for j in range(3):
                ans = (ans + self.data[1][i][j]) % MOD
        return ans

n, m = map(int, input().split())
good = []
for _ in range(3):
    good.append(list(map(int, input().split())))

st = SegmentTree(n, good)
a = [0]*n
for i in range(n):
    st.set_leaf(i, 0)
st.build()

res = []
for _ in range(m):
    v, t = map(int, input().split())
    v -= 1
    a[v] = t
    st.update(v, t)
    res.append(st.total())

print(' '.join(map(str, res)))
```Giải thích: Các`SegmentTree`lớp duy trì các ma trận lập trình động.`set_leaf`đặt một phần tử mảng duy nhất.`combine`hợp nhất hai phân đoạn tôn trọng các cặp được phép.`update`cập nhật một lá và lan truyền.`total`tính tổng tất cả các chuỗi có thể có trên tất cả các giá trị bắt đầu/kết thúc. Phép nhân và tổng là modulo 777777777. Chúng ta khởi tạo tất cả các phần tử bằng 0 và xây dựng cây. Sau đó, chúng tôi xử lý từng truy vấn và thu thập câu trả lời. 

## Ví dụ đã hoạt động 

Dấu vết mẫu 1: 

| Truy vấn | Mảng`a`| Cập nhật ma trận lá | Tổng gốc | 
| --- | --- | --- | --- | 
| 1 1 | [1,0,0] | Lá 0 → [[1,0,0],... ] | 3 | 
| 1 3 | [3,0,0] | Lá 0 → [[0,0,1],... ] | 6 | 
| 2 2 | [3,2,0] | Lá 1 → [[0,1,0],... ] | 1 | 
| 3 0 | [3,2,0] | Lá 2 → [[1,0,0],[0,1,0],[0,0,1]] | 1 | 
| 2 1 | [3,1,0] | Lá 1 → [[1,0,0],... ] | 2 | 

Điều này chứng tỏ rằng các bản cập nhật được lan truyền chính xác và`total`nắm bắt tất cả các chuỗi hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log n) | Mỗi truy vấn cập nhật một lá và truyền lên cây phân đoạn, log chiều cao n | 
| Không gian | O(n) | Cây phân đoạn lưu trữ khoảng 2*n ma trận, mỗi ma trận có kích thước không đổi 3x3 | 

Thuật toán phù hợp thoải mái trong giới hạn thời gian và bộ nhớ cho trước n, m ≤ 77777. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io
```
