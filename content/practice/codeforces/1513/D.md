---
title: "CF 1513D - GCD và MST"
description: "Chúng tôi được yêu cầu tính cây bao trùm tối thiểu (MST) của biểu đồ có trọng số được xây dựng từ một mảng các số nguyên dương."
date: "2026-06-10T18:46:27+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dsu", "graphs", "greedy", "number-theory", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1513
codeforces_index: "D"
codeforces_contest_name: "Divide by Zero 2021 and Codeforces Round 714 (Div. 2)"
rating: 2000
weight: 1513
solve_time_s: 144
verified: true
draft: false
---

[CF 1513D - GCD và MST](https://codeforces.com/problemset/problem/1513/D) 

**Xếp hạng:** 2000 
**Tags:** thuật toán xây dựng, dsu, đồ thị, tham lam, lý thuyết số, sắp xếp 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tính cây bao trùm tối thiểu (MST) của biểu đồ có trọng số được xây dựng từ một mảng các số nguyên dương. Mỗi phần tử mảng tương ứng với một đỉnh và các cạnh tồn tại giữa các phần tử liền kề có trọng số cố định`p`hoặc giữa bất kỳ cặp đỉnh nào`(i, j)`nếu ước chung lớn nhất (GCD) của tất cả các phần tử từ`a[i]`ĐẾN`a[j]`bằng mức tối thiểu trong mảng con đó. Trọng số của cạnh đó là giá trị nhỏ nhất của mảng con. Mục đích không phải là xây dựng rõ ràng tất cả các cạnh mà là tính toán trọng số MST một cách hiệu quả. 

Những hạn chế cho thấy`n`có thể lên tới 2·10^5 cho mỗi trường hợp thử nghiệm và tổng của`n`trên tất cả các trường hợp thử nghiệm cũng là 2·10^5. Điều này có nghĩa là bất kỳ cách tiếp cận nào có thời gian O(n^2) cho mỗi trường hợp thử nghiệm đều không khả thi ngay lập tức vì nó sẽ yêu cầu khoảng 4·10^10 thao tác. Chúng ta cần một thuật toán gần tuyến tính hoặc tuyến tính trong`n`. 

Trường hợp cạnh tinh tế xảy ra khi tất cả các phần tử mảng bằng hoặc nhỏ hơn`p`. Ví dụ, nếu`n = 3`,`p = 5`, Và`a = [3, 3, 3]`, quá trình xử lý kề cận đơn giản có thể cố gắng tạo ra các cạnh giữa các đỉnh không liền kề một cách không cần thiết hoặc bỏ lỡ các cạnh rẻ hơn. Tương tự, nếu mảng có một phần tử nhỏ được bao quanh bởi các phần tử lớn hơn, các kết nối tham lam phải xác định chính xác rằng việc kết nối qua phần tử nhỏ này có thể giảm chi phí MST bên dưới`p`mỗi cạnh. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force theo nghĩa đen là tạo ra tất cả các cạnh ứng cử viên. Đối với mỗi cặp`(i, j)`bạn tính GCD của`a[i..j]`và mức tối thiểu trong phạm vi đó. Nếu chúng bằng nhau thì thêm một cạnh có trọng số`min(a[i..j])`. Các đỉnh liền kề luôn có cạnh có trọng số`p`. Sau khi tạo các cạnh, chạy thuật toán Kruskal hoặc Prim để tính MST. Điều này đúng vì nó tuân theo chính xác các quy tắc của bài toán, nhưng đó là O(n^2 log n) trong trường hợp xấu nhất đối với việc tạo và sắp xếp cạnh, quá chậm đối với`n = 2·10^5`. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần tất cả các cạnh. Bất kỳ cạnh nào có trọng số lớn hơn`p`không bao giờ rẻ hơn việc sử dụng liền kề`p`các cạnh trong MST. Vì vậy, chúng ta có thể hạn chế sự chú ý đến các cạnh nơi`min(a[i..j]) < p`. Đối với những điều đó, hãy xem xét từng yếu tố`a[k]`làm ứng cử viên để kết nối các phạm vi liền kề trong đó nó là mức tối thiểu và chia tất cả các phần tử trong phạm vi đó. Điều này biến vấn đề thành một cấu trúc tham lam: bắt đầu từ các phần tử nhỏ nhất và mở rộng sang trái và phải trong khi GCD vẫn chia hết cho phần tử đó. Các phạm vi này cung cấp các cạnh mà chúng ta có thể thêm vào MST bằng cách sử dụng tập hợp rời rạc (DSU). Điều này cho phép chúng tôi tránh tính toán GCD rõ ràng cho tất cả các cặp và giảm vấn đề xuống còn việc sắp xếp đại khái mảng và thực hiện các phép toán tìm hợp, tạo ra độ phức tạp O(n log n) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 log n) | O(n^2) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`,`p`, và mảng`a`. 
2. Khởi tạo cấu trúc tập hợp rời rạc (DSU) cho`n`các phần tử. Điều này sẽ theo dõi những đỉnh nào đã được kết nối trong MST. 
3. Tạo danh sách chỉ mục`idx`sắp xếp theo các giá trị mảng tương ứng`a[i]`. Điều này cho phép chúng ta xử lý các mức tối thiểu tiềm năng theo thứ tự tăng dần. 
4. Khởi tạo`mst_weight = 0`Và`edges_used = 0`. Những thứ này sẽ tích lũy tổng trọng số MST và theo dõi số cạnh chúng ta đã thêm vào. 
5. Lặp lại các chỉ số đã được sắp xếp. Đối với mỗi`i`: 

- Bỏ qua nếu`a[i] >= p`bởi vì các cạnh như vậy không bao giờ rẻ hơn cạnh cố định`p`các cạnh. 
- Thử mở rộng sang bên trái: trong khi phần tử trước tồn tại, chưa được kết nối trong DSU và chia hết cho`a[i]`, hợp nhất chúng và thêm`a[i]`ĐẾN`mst_weight`. 
- Tương tự, mở rộng sang bên phải, nối các phần tử chia liền kề. 
- Mỗi phép hợp tương ứng với việc thêm một cạnh MST, do đó tăng dần`edges_used`. 
6. Sau khi xử lý tất cả các phần tử nhỏ, có thể còn lại các cặp chưa được kết nối. Với mọi cặp liền kề`(i, i+1)`không nằm trong cùng thành phần DSU, hãy hợp nhất chúng và thêm`p`ĐẾN`mst_weight`. 
7. In`mst_weight`cho trường hợp thử nghiệm. 

Tại sao nó hoạt động: Bằng cách xử lý các phần tử mảng từ nhỏ nhất đến lớn nhất, chúng tôi đảm bảo rằng bất cứ khi nào chúng tôi tạo một cạnh MST, đó là thành phần rẻ nhất có thể. Tính chia hết đảm bảo rằng điều kiện GCD được thỏa mãn đối với các phạm vi liền kề. Các cạnh liền kề còn lại có trọng lượng`p`xử lý các trường hợp không có phần tử nhỏ hơn nào có thể nối hai đỉnh, đảm bảo MST hoàn chỉnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
    
    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x
    
    def union(self, x, y):
        x_root, y_root = self.find(x), self.find(y)
        if x_root == y_root:
            return False
        self.parent[y_root] = x_root
        return True

def solve():
    t = int(input())
    for _ in range(t):
        n, p = map(int, input().split())
        a = list(map(int, input().split()))
        dsu = DSU(n)
        idx = sorted(range(n), key=lambda i: a[i])
        mst_weight = 0
        edges_used = 0
        
        for i in idx:
            if a[i] >= p:
                break
            j = i - 1
            while j >= 0 and a[j] % a[i] == 0 and dsu.union(i, j):
                mst_weight += a[i]
                edges_used += 1
                j -= 1
            j = i + 1
            while j < n and a[j] % a[i] == 0 and dsu.union(i, j):
                mst_weight += a[i]
                edges_used += 1
                j += 1

        for i in range(n - 1):
            if dsu.union(i, i + 1):
                mst_weight += p
                edges_used += 1

        print(mst_weight)

if __name__ == "__main__":
    solve()
```Mã đầu tiên xác định một DSU với khả năng nén đường dẫn để kết hợp và tìm kiếm hiệu quả. Việc sắp xếp các chỉ mục đảm bảo chúng tôi xử lý các cạnh MST tiềm năng rẻ nhất trước tiên. Các phần mở rộng trái-phải hợp các phần tử chia liền kề nhau, tương ứng với các cạnh thỏa mãn điều kiện GCD. Cuối cùng, các cạnh liền kề còn lại của trọng lượng`p`đảm bảo một MST được kết nối. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
n = 2, p = 5
a = [10, 10]
```| Bước | Hành động | phụ huynh DSU | trọng lượng MST | Các cạnh được sử dụng | 
| --- | --- | --- | --- | --- | 
| 1 | Sắp xếp chỉ số theo giá trị | [0, 1] | 0 | 0 | 
| 2 | i=0, a[i]=10 >= p, bỏ qua | [0,1] | 0 | 0 | 
| 3 | Nối cặp liền kề còn lại (0,1) với p=5 | [0,0] | 5 | 1 | 

Đầu ra là`5`. 

Đầu vào mẫu 2:```
n = 2, p = 5
a = [3, 3]
```| Bước | Hành động | phụ huynh DSU | trọng lượng MST | Các cạnh được sử dụng | 
| --- | --- | --- | --- | --- | 
| 1 | Sắp xếp chỉ số theo giá trị | [0,1] | 0 | 0 | 
| 2 | i=0, a[i]=3 < p | liên minh trái/phải không cần thiết | [0,0] | 3 | 
| 3 | Không cần các cạnh còn lại | - | - | - | 

Đầu ra là`3`. 

Những dấu vết này cho thấy các phần tử nhỏ được sử dụng một cách tham lam cho các kết nối dựa trên GCD và các cạnh kề còn lại sẽ xử lý phần còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Các chỉ số sắp xếp lấy O(n log n), tìm liên kết với nén đường dẫn gần như tuyến tính. | 
| Không gian | O(n) | DSU lưu trữ mảng cha và mảng chỉ mục sử dụng O(n) | 

Với tổng ràng buộc là n 2·10^5, thuật toán này chạy hiệu quả trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("4\n2 5\n10 10\n2 5\n3 3\n4 5\n5 2 4 9\n8 8\n5 3 3 6 10 100 9 15\n") == "5\n3\n12\n46"

# Custom cases
assert run("1\n2 1\n1 1\n") == "1", "minimum values, p=1"
assert run("1\n3 10\n5 5
```
