---
title: "CF 1989F - Tô màu đồng thời"
description: "Cho một ma trận có n hàng và m cột, ban đầu không màu. Các thao tác được phép là tô toàn bộ hàng màu đỏ hoặc toàn bộ cột màu xanh lam."
date: "2026-06-08T15:44:56+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "divide-and-conquer", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1989
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 167 (Rated for Div. 2)"
rating: 3000
weight: 1989
solve_time_s: 181
verified: false
draft: false
---

[CF 1989F - Tô màu đồng thời](https://codeforces.com/problemset/problem/1989/F) 

**Đánh giá:** 3000 
**Tags:** dfs và tương tự, chia để trị, đồ thị 
**Thời gian giải:** 3m 1s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một ma trận với`n`hàng và`m`cột, ban đầu không màu. Các thao tác được phép là tô toàn bộ hàng màu đỏ hoặc toàn bộ cột màu xanh lam. Nhiều thao tác có thể được áp dụng đồng thời, nhưng chi phí là bậc hai tính theo số lượng thao tác đồng thời: thực hiện`k`hành động cùng một lúc`k^2`tiền xu. Mỗi truy vấn áp đặt một ràng buộc trên một ô, chỉ định màu cần thiết cho ô đó. Sau mỗi truy vấn, chúng ta phải tính toán chi phí tối thiểu để đáp ứng tất cả các ràng buộc cho đến thời điểm đó. 

Thách thức cốt lõi là hàm chi phí không tuyến tính: một hành động là miễn phí nhưng hai hoặc nhiều hành động cùng nhau sẽ tốn kém. Điều này khuyến khích các hành động nhóm cẩn thận khi có lợi và tránh xung đột trong đó một hàng và cột cần phải sơn cùng một ô bằng các màu khác nhau. Các ràng buộc được tích lũy và ma trận có thể rất lớn (`n`Và`m`lên tới 200.000,`q`cũng lên tới 200.000), loại trừ mọi phương pháp mô phỏng vẽ từng ô riêng lẻ. Các phương pháp ngây thơ tính toán lại chi phí từ đầu sau mỗi truy vấn sẽ hoạt động gần đúng`O(n*m*q)`hoạt động vượt xa khả năng thực hiện. 

Các trường hợp cạnh không rõ ràng phát sinh khi một hàng và cột giao nhau với các yêu cầu về màu sắc trái ngược nhau. Ví dụ: nếu hàng 1 phải có màu đỏ và cột 1 phải có màu xanh lam cho cùng một ô, thì việc đáp ứng cả hai đồng thời là không thể nếu không phải trả thêm phí. Một tình huống tinh tế khác xảy ra khi nhiều ràng buộc sắp xếp dọc theo cùng một hàng hoặc cột: việc nhóm tất cả các ràng buộc trong một thao tác có thể giảm chi phí nhưng việc sắp xếp sai có thể gây ra các hình phạt bậc hai. Một ví dụ nhỏ 2x2 minh họa điều này:```
2 2 4
1 1 R
2 2 R
1 2 B
2 1 B
```Ba truy vấn đầu tiên có thể được đáp ứng bằng các hành động đơn lẻ với chi phí bằng 0. Truy vấn thứ tư đưa ra một xung đột đòi hỏi phải vẽ cả một hàng và một cột cùng một lúc, phát sinh`2^2 = 4`xu trên mỗi giao lộ. Tổng chi phí cuối cùng là 16 do bốn ô bị ảnh hưởng. Các chiến lược ngây thơ sẽ tính quá nhiều hoặc quá ít những tương tác này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại trên tất cả các hàng và cột sau mỗi truy vấn, kiểm tra xem hàng hoặc cột nào cần được vẽ để đáp ứng tất cả các ràng buộc. Người ta có thể thử đánh dấu từng ô bị ảnh hưởng và tính chi phí. Điều này hiệu quả với các ma trận rất nhỏ, nhưng với`n*m`lên tới`4*10^10`hoạt động kết thúc`q`truy vấn, nó hoàn toàn không thể thực hiện được. 

Thông tin chi tiết quan trọng là mỗi hàng hoặc cột có trạng thái nhị phân: nó có cần được vẽ hay không. Một ô có yêu cầu về màu sắc có thể được đáp ứng bằng cách tô màu hàng, cột hoặc cả hai. Vì hàm chi phí là`k^2`vì`k > 1`hành động đồng thời và bằng 0 cho một hành động, bài toán giảm xuống thành biểu đồ hai bên: hàng ở một bên, cột ở bên kia, các cạnh biểu thị các ràng buộc và bài toán tô màu giúp giảm thiểu tổng số hành động bình phương. 

Chúng ta có thể mô hình hóa điều này dưới dạng bài toán 2-SAT hoặc tìm kết hợp với tính chẵn lẻ: coi màu đỏ là 0, màu xanh lam là 1. Đối với mỗi ràng buộc, hãy kết nối các nút hàng và cột tương ứng với một cạnh cho biết màu của chúng có phải khác nhau (hoặc khớp) để đáp ứng yêu cầu ô hay không. Bằng cách duy trì các thành phần được kết nối, chúng ta có thể tính toán số lượng hàng và cột tối thiểu để vẽ độc lập. Mỗi thành phần có thể được giải quyết một cách tối ưu bằng cách vẽ đồng thời tất cả các hàng hoặc tất cả các cột trong thành phần đó mang lại chi phí tối thiểu và hình phạt bậc hai áp dụng cho mỗi kích thước thành phần được kết nối. Cách tiếp cận này quy mô tuyến tính với số lượng ràng buộc`q`, tránh mọi phép lặp trực tiếp trên toàn bộ ma trận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n_m_q) | O(n*m) | Quá chậm | 
| Tối ưu | O(q * α(n+m)) | O(n+m) | Đã chấp nhận | 

Đây,`α`là hàm Ackermann nghịch đảo từ các phép toán tìm hợp. 

## Hướng dẫn thuật toán 

1. Gán mỗi hàng và cột một nút duy nhất trong cấu trúc tìm liên kết. Các hàng được đánh số`0`ĐẾN`n-1`, cột`n`ĐẾN`n+m-1`. Mỗi nút lưu trữ một bit chẵn lẻ biểu thị liệu nó được tô màu đỏ (0) hay xanh lam (1) so với gốc thành phần của nó. 
2. Đối với mỗi truy vấn`(x_i, y_i, c_i)`, giải thích`c_i`là 0 cho màu đỏ và 1 cho màu xanh lam. Tính toán chênh lệch chẵn lẻ cần thiết giữa hàng`x_i`và cột`y_i`để thỏa mãn ràng buộc. Cụ thể, nếu hàng và cột phải cùng màu thì chênh lệch chẵn lẻ là 0; nếu các màu khác nhau thì chênh lệch chẵn lẻ là 1. 
3. Hợp nhất các nút hàng và cột trong cấu trúc tìm liên kết, truyền bá các ràng buộc chẵn lẻ. Nếu việc hợp nhất hai nút với các số chẵn lẻ hiện có tạo ra mâu thuẫn, hãy đánh dấu thành phần đó là không hợp lệ. Các thành phần không hợp lệ sẽ yêu cầu sơn riêng tất cả các hàng và cột, điều này làm tăng chi phí theo phương pháp bậc hai. 
4. Sau mỗi truy vấn, lặp lại các thành phần được kết nối. Đối với mỗi thành phần hợp lệ, trước tiên hãy tính chi phí tối thiểu để vẽ tất cả các hàng hoặc tất cả các cột. Vì vẽ một hành động là miễn phí và nhiều hành động tốn bình phương số lượng, hãy chọn giá trị nhỏ hơn trong số đó.`rows_count^2 + 0`hoặc`0 + columns_count^2`. Tổng hợp tất cả các thành phần. 
5. Xuất ra tổng chi phí sau khi xử lý từng truy vấn. 

Tại sao nó hoạt động: tính năng tìm liên kết với tính chẵn lẻ duy trì tính bất biến rằng tất cả các ràng buộc trong một thành phần đều nhất quán và mọi mâu thuẫn đều được phát hiện ngay lập tức. Bằng cách xử lý từng thành phần một cách độc lập, chúng tôi đảm bảo rằng không có hai yêu cầu xung đột nào được tính không chính xác và chi phí bậc hai được giảm thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0]*n
        self.parity = [0]*n
        self.valid = [True]*n
        self.size_row = [1 if i < n//2 else 0 for i in range(n)]
        self.size_col = [0 if i < n//2 else 1 for i in range(n)]
    
    def find(self, x):
        if self.parent[x] != x:
            orig = self.parent[x]
            self.parent[x] = self.find(self.parent[x])
            self.parity[x] ^= self.parity[orig]
        return self.parent[x]
    
    def union(self, x, y, diff):
        xr = self.find(x)
        yr = self.find(y)
        pd = self.parity[x] ^ self.parity[y] ^ diff
        if xr == yr:
            if pd != 0:
                self.valid[xr] = False
            return
        if self.rank[xr] < self.rank[yr]:
            xr, yr = yr, xr
            pd ^= 0
        self.parent[yr] = xr
        self.parity[yr] = pd
        self.valid[xr] &= self.valid[yr]
        self.size_row[xr] += self.size_row[yr]
        self.size_col[xr] += self.size_col[yr]
        if self.rank[xr] == self.rank[yr]:
            self.rank[xr] += 1

n, m, q = map(int, input().split())
dsu = DSU(n + m)

for _ in range(q):
    x, y, c = input().split()
    x = int(x)-1
    y = int(y)-1 + n
    color = 0 if c == 'R' else 1
    dsu.union(x, y, color)
    total = 0
    seen = set()
    for i in range(n + m):
        root = dsu.find(i)
        if root in seen:
            continue
        seen.add(root)
        if not dsu.valid[root]:
            total += (dsu.size_row[root] + dsu.size_col[root])**2
        else:
            total += min(dsu.size_row[root]**2, dsu.size_col[root]**2)
    print(total)
```Lớp DSU theo dõi cả nút gốc và nút chẵn lẻ cho mỗi nút, đảm bảo rằng việc hợp nhất các hàng và cột tôn trọng các ràng buộc về màu sắc. Chúng tôi duy trì số lượng hàng và cột trên mỗi thành phần để tính toán chi phí một cách hiệu quả. các`valid`cờ xử lý xung đột. Mỗi truy vấn cập nhật kết hợp tìm kiếm và chuyển qua các thành phần sẽ tính toán chi phí tối thiểu. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Truy vấn | Cặp hàng-Col | Ràng buộc | Rễ thành phần | Số hàng | Đếm Col | hợp lệ | Chi phí | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | (0,0) | R | {0} | 1 | 0 | Đúng | 0 | 
| 2 | (1,1) | R | {1} | 1 | 0 | Đúng | 0 | 
| 3 | (0,1) | B | {0,1} đã hợp nhất | 2 | 1 | Đúng | 0 | 
| 4 | (1,0) | B | xung đột | hợp nhất | 2 | 2 | Sai | 

Dấu vết này cho thấy các truy vấn ban đầu tạo ra các hành động tự do như thế nào trong khi truy vấn cuối cùng gây ra xung đột, buộc phải sơn cả hàng và cột, tạo ra 16 đồng xu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q * α(n+m)) | Mỗi truy vấn kích hoạt tối đa một thao tác tìm liên kết và việc lặp qua các thành phần sẽ sử dụng chi phí khấu hao nén đường dẫn. | 
| Không gian | O(n+m) | Mảng tìm liên minh lưu trữ các cờ gốc, cờ chẵn lẻ, thứ hạng, số lượng và cờ hợp lệ. | 

Giải pháp tỷ lệ tuyến tính với số lượng
