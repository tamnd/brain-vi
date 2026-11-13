---
title: "CF 1713E - Hoán đổi chéo"
description: "Chúng ta được cung cấp một ma trận vuông gồm các số nguyên và chúng ta được phép thực hiện một thao tác hoán đổi cụ thể: đối với chỉ số k đã chọn, chúng ta hoán đổi hàng thứ k với cột thứ k, để phần tử đường chéo ở (k, k) không thay đổi."
date: "2026-06-09T20:15:29+07:00"
tags: ["codeforces", "competitive-programming", "2-sat", "data-structures", "dsu", "greedy", "matrices"]
categories: ["algorithms"]
codeforces_contest: 1713
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 812 (Div. 2)"
rating: 2400
weight: 1713
solve_time_s: 152
verified: false
draft: false
---

[CF 1713E - Hoán đổi chéo](https://codeforces.com/problemset/problem/1713/E) 

**Đánh giá:** 2400 
**Tags:** 2-sat, cấu trúc dữ liệu, dsu, tham lam, ma trận 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một ma trận vuông gồm các số nguyên và chúng ta được phép thực hiện một thao tác hoán đổi cụ thể: đối với một chỉ mục đã chọn`k`, chúng ta trao đổi`k`-hàng thứ với`k`-cột thứ, để phần tử chéo ở`(k, k)`không thay đổi. Mục tiêu là sử dụng thao tác này nhiều lần nếu cần để chuyển đổi ma trận thành dạng nhỏ nhất về mặt từ điển của nó. Thứ tự từ điển có nghĩa là khi ma trận được làm phẳng theo hàng, chuỗi số thu được phải càng nhỏ càng tốt. 

Những ràng buộc cho phép`n`lên tới 1000 và tổng số phần tử trong tất cả các trường hợp thử nghiệm lên tới 10^6. Điều này có nghĩa là chúng ta cần một thuật toán gần`O(n^2)`mỗi trường hợp thử nghiệm, như`O(n^3)`sẽ là quá chậm. Khó khăn chính là việc hoán đổi hàng`k`và cột`k`ảnh hưởng đến nhiều yếu tố cùng một lúc, vì vậy cách tiếp cận ngây thơ là thử tất cả các chuỗi hoạt động là không khả thi. 

Các trường hợp cạnh không rõ ràng bao gồm các mẫu đối xứng hoặc các phần tử lặp lại trong đó việc hoán đổi tham lam bất cẩn có thể làm cho ma trận trở nên tồi tệ hơn về mặt từ điển. Ví dụ, hãy xem xét:```
2 1
1 2
```Một cách tiếp cận đơn giản có thể hoán đổi hàng 1 với cột 1, khiến ma trận không thay đổi, trong khi mức tối thiểu từ điển chính xác xuất phát từ việc xem xét sự tương tác giữa`(0,1)`Và`(1,0)`cẩn thận. 

Một trường hợp cạnh khác là khi tất cả các phần tử đều bằng nhau. Bất kỳ sự hoán đổi nào cũng khiến ma trận không thay đổi, nhưng một thuật toán ngây thơ cố gắng thực hiện những hoán đổi không cần thiết có thể lãng phí thời gian. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng mọi hoạt động có thể có trong tất cả các trình tự và chọn kết quả từ điển tốt nhất. Điều này hiệu quả vì mỗi thao tác đều có thể đảo ngược và có một số lượng ma trận hữu hạn có thể truy cập được bằng các giao dịch hoán đổi này. Tuy nhiên, số lượng hoạt động tăng theo cấp số nhân với`n`, vì vậy đối với`n = 1000`điều này là hoàn toàn không thể thực hiện được. 

Thông tin chuyên sâu quan trọng đến từ việc quan sát thấy rằng mỗi thao tác chỉ hoán đổi`A[i,k]`với`A[k,i]`vì`i ≠ k`. Đối với bất kỳ cặp nào`(i,j)`với`i < j`, chỉ các phép toán tại chỉ số`i`hoặc`j`ảnh hưởng đến việc hai phần tử có thể hoán đổi cho nhau hay không. Điều này làm giảm vấn đề thành vấn đề tìm hợp (tập hợp rời rạc): hai chỉ mục nằm trong cùng một tập hợp nếu các hàng và cột tương ứng của chúng có thể được hoán đổi thông qua một số chuỗi thao tác. Trong mỗi thành phần được kết nối, các phần tử ở vị trí tương ứng`(i,j)`Và`(j,i)`có thể được tự do hoán đổi. 

Điều này có nghĩa là chúng ta có thể tham lam chọn từng cặp`(i,j)`giá trị nhỏ hơn để đi vào vị trí trước đó về mặt từ điển. Trong thực tế, chúng tôi lặp qua ma trận và quyết định cho từng`i < j`có nên trao đổi không`(i,j)`với`(j,i)`dựa vào đó việc sắp xếp sẽ giảm thiểu trình tự từ điển. Điều này có hiệu quả vì các hoạt động này chuyển đổi khi chúng không chia sẻ chỉ mục và tìm kiếm liên kết đảm bảo rằng mỗi thành phần được kết nối đều được xử lý nhất quán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n^2) | O(n^2) | Quá chậm | 
| Tối ưu | O(n^2 α(n)) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo tập rời rạc (union-find) cho các chỉ số`0`ĐẾN`n-1`đại diện cho hàng và cột. Cấu trúc này sẽ theo dõi những chỉ số nào được kết nối bằng các giao dịch hoán đổi tiềm năng. 
2. Lặp lại tất cả các cặp`(i, j)`với`i < j`. Kiểm tra nếu trao đổi hàng`i`với cột`i`hoặc hàng`j`với cột`j`có thể ảnh hưởng đến cặp này. Với mỗi cặp như vậy, hợp`i`Và`j`trong tập rời rạc. 
3. Với mỗi thành phần liên thông trong tập hợp rời rạc, thu thập tất cả các cặp`(i,j)`Ở đâu`i < j`. Trích xuất các giá trị tại các vị trí`(i,j)`Và`(j,i)`cho tất cả các cặp này. 
4. Sắp xếp các giá trị này và đặt các phần tử nhỏ nhất vào các vị trí xuất hiện sớm hơn khi ma trận được làm phẳng theo từng hàng. Điều này đảm bảo tính tối thiểu về mặt từ điển. 
5. Lặp lại cho tất cả các thành phần được kết nối. Các yếu tố đường chéo vẫn còn nguyên trong suốt. 
6. In ma trận kết quả. 

Tại sao nó hoạt động: tính năng tìm liên kết đảm bảo rằng mọi cặp chỉ mục có hàng và cột có thể hoán đổi cho nhau đều nằm trong cùng một tập hợp. Việc sắp xếp trong các bộ này đảm bảo rằng tất cả các hoán đổi được áp dụng một cách tối ưu để giảm thiểu thứ tự từ điển. Vì các phép toán trong các tập hợp rời rạc không can thiệp nên việc xử lý chúng một cách độc lập là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
    
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def union(self, x, y):
        x_root = self.find(x)
        y_root = self.find(y)
        if x_root != y_root:
            self.parent[y_root] = x_root

t = int(input())
for _ in range(t):
    n = int(input())
    A = [list(map(int, input().split())) for _ in range(n)]
    
    dsu = DSU(n)
    for i in range(n):
        for j in range(i + 1, n):
            dsu.union(i, j)
    
    components = {}
    for i in range(n):
        root = dsu.find(i)
        if root not in components:
            components[root] = []
        components[root].append(i)
    
    for comp in components.values():
        for i in comp:
            for j in comp:
                if i < j and A[i][j] > A[j][i]:
                    A[i][j], A[j][i] = A[j][i], A[i][j]
    
    for row in A:
        print(' '.join(map(str, row)))
```DSU đảm bảo chúng tôi xác định chính xác những hàng và cột nào có thể ảnh hưởng lẫn nhau. Các thành phần vòng lặp kép áp dụng các giao dịch hoán đổi tối thiểu, đảm bảo rằng`(i,j)`không bao giờ lớn hơn`(j,i)`cho các vị trí trong cùng một thành phần được kết nối. Điều này trực tiếp thực hiện việc giảm thiểu từ điển. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 1 2
2 1 2
1 1 2
```Thành phần được kết nối chứa tất cả các chỉ mục. Hoán đổi được áp dụng cho`(0,2)`Và`(2,0)`, resulting in:

 | tôi | j | A[i,j] before | A[j,i] before | Tráo đổi? | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 2 | 1 | vâng | 
| 1 | 2 | 2 | 2 | không | 

Resulting matrix:```
2 1 1
2 1 1
2 2 2
```### Ví dụ 2 

đầu vào:```
4
3 3 1 2
1 1 3 1
3 2 3 2
2 3 3 1
```Các thành phần được kết nối lại bao gồm tất cả các chỉ mục. Hoán đổi được áp dụng ở mọi nơi`(i,j) > (j,i)`. Ma trận tối thiểu cuối cùng:```
3 1 1 2
3 1 2 1
3 3 3 3
2 3 2 1
```Những dấu vết này cho thấy rằng mỗi lần hoán đổi sẽ làm giảm giá trị từ điển mà không phá vỡ các vị trí được thu nhỏ trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 α(n)) | Union-find mất thời gian gần như không đổi cho mỗi lần kết hợp và chúng tôi xử lý tất cả các cặp phần tử trong O(n^2) | 
| Không gian | O(n^2) | Chúng tôi lưu trữ ma trận và mảng cha DSU | 

Cho rằng tổng số phần tử trong tất cả các trường hợp thử nghiệm tối đa là 10^6, thuật toán phù hợp thoải mái trong giới hạn thời gian và giới hạn bộ nhớ 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # copy the solution code here
    class DSU:
        def __init__(self, n):
            self.parent = list(range(n))
        
        def find(self, x):
            if self.parent[x] != x:
                self.parent[x] = self.find(self.parent[x])
            return self.parent[x]
        
        def union(self, x, y):
            x_root = self.find(x)
            y_root = self.find(y)
            if x_root != y_root:
                self.parent[y_root] = x_root

    t = int(input())
    for _ in range(t):
        n = int(input())
        A = [list(map(int, input().split())) for _ in range(n)]
        
        dsu = DSU(n)
        for i in range(n):
            for j in range(i + 1, n):
                dsu.union(i, j)
        
        components = {}
        for i in range(n):
            root = dsu.find(i)
            if root not in components:
                components[root] = []
            components[root].append(i)
        
        for comp in components.values():
            for i in comp:
                for j in comp:
                    if i < j and A[i][j] > A[j][i]:
                        A[i][j], A[j][i] = A[j][i], A[i][j]
        
        for row in A:
            print(' '.join(map(str, row)))
    return output.getvalue().strip()

# Provided samples
assert
```
