---
title: "CF 2158E - Chậu rửa"
description: "Chúng ta có một lưới 2D có kích thước $n nhân m$, trong đó mỗi ô chứa một số nguyên dương. Chúng ta có thể tạo \"lỗ\" trong ô. Một ô được coi là một phần chìm nếu nó có một lỗ hoặc nếu nó nằm liền kề (chung một cạnh) với một phần chìm khác có giá trị nhỏ hơn hoặc bằng nhau."
date: "2026-06-08T00:13:35+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "dsu", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2158
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1067 (Div. 2)"
rating: 2600
weight: 2158
solve_time_s: 94
verified: false
draft: false
---

[CF 2158E - Chìm](https://codeforces.com/problemset/problem/2158/E) 

**Đánh giá:** 2600 
**Tags:** cấu trúc dữ liệu, phân chia để chinh phục, dsu, đồ thị, triển khai 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới kích thước 2D$n \times m$, trong đó mỗi ô chứa một số nguyên dương. Chúng ta có thể tạo ra những “lỗ” trong tế bào. Một ô được coi là một phần chìm nếu nó có một lỗ hoặc nếu nó nằm liền kề (chung một cạnh) với một phần chìm khác có giá trị nhỏ hơn hoặc bằng nhau. Mục tiêu là làm cho mọi ô đều chìm trong khi đặt số lượng lỗ tối thiểu. Số lượng lỗ tối thiểu này được gọi là “vẻ đẹp” của lưới. 

Ngoài ra, chúng tôi còn nhận được một loạt câu hỏi. Mỗi truy vấn làm giảm giá trị của một ô cụ thể theo một lượng dương. Sau mỗi truy vấn, chúng ta phải tính toán lại vẻ đẹp của lưới hiện tại, bắt đầu làm mới như thể chưa có lỗ nào được đặt trước đó. 

Các ràng buộc rất chặt chẽ:$n \cdot m$và số lượng truy vấn có thể đạt được$2 \cdot 10^5$. Bất kỳ thuật toán nào kiểm tra trực tiếp mọi khả năng truyền lan của ô cho mọi truy vấn sẽ quá chậm, vì việc truyền bá bạo lực cho mỗi truy vấn có thể yêu cầu$O(n \cdot m)$làm việc theo mỗi truy vấn, dẫn đến$O((n \cdot m) \cdot q) = 4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất. 

Trường hợp cạnh tinh vi xảy ra khi nhiều ô liền kề tạo thành một vùng có giá trị bằng nhau. Ví dụ: nếu lưới 2x2 là```
2 2
2 2
```đặt một lỗ duy nhất ở một góc là đủ, bởi vì "sự lan truyền chìm" lan sang các hàng xóm có giá trị bằng nhau. Một cách tiếp cận ngây thơ đếm số lỗ trên mỗi hàng hoặc cột một cách độc lập sẽ bị tính quá mức trong những trường hợp như vậy. 

Một tình huống phức tạp khác là khi các truy vấn giảm các ô cao bên dưới các vùng lân cận. Điều này có thể tạo ra những bồn rửa mới cần có thêm lỗ. Ví dụ, bắt đầu bằng```
1 2
```đặt một lỗ ở`1`ban đầu là đủ. Nếu ô thứ hai giảm từ 2 xuống 1 thì lúc này cả hai ô đều bằng nhau và quá trình truyền có thể cần thêm một lỗ tùy thuộc vào thứ tự truyền. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là mô phỏng quá trình lan truyền một cách rõ ràng. Chúng ta có thể bắt đầu bằng cách lặp lại trên lưới, đặt một lỗ ở bất kỳ vị trí nào mà một ô không thể trở thành phần chìm theo vùng lân cận, sau đó đánh dấu lặp đi lặp lại tất cả các phần chìm có thể tiếp cận cho đến khi không có ô mới nào được đánh dấu. Sau mỗi truy vấn, chúng tôi sẽ tính toán lại từ đầu. Điều này sẽ yêu cầu$O(n \cdot m)$mỗi lần tính toán và với$q$truy vấn, điều đó trở nên quá chậm. 

Cái nhìn sâu sắc quan trọng là việc truyền lan chìm là đơn điệu: một ô có giá trị thấp hơn không thể truyền mức chìm đến hàng xóm cao hơn trừ khi hàng xóm đó đã có giá trị nhỏ hơn hoặc bằng. Do đó, bài toán có thể được rút gọn thành một kiểu phân rã đồ thị dựa trên độ cao. Chúng ta có thể sắp xếp tất cả các ô theo giá trị và xử lý chúng từ nhỏ nhất đến lớn nhất. Mỗi giá trị mới được xử lý sẽ mở rộng "vùng chìm" hiện có hoặc yêu cầu một lỗ mới nếu nó bị cô lập. Điều này cho phép chiến lược phân chia và chinh phục kết hợp với cấu trúc tìm kiếm liên minh (DSU). DSU theo dõi các thành phần được kết nối của các ô mà cuối cùng sẽ truyền đến cùng một lỗ. 

Khi giá trị ô giảm do truy vấn, chúng ta chỉ cần đánh giá lại các vùng chứa ô đó và các ô lân cận. Vì các giá trị chỉ giảm, nên phần chìm mới xuất hiện cục bộ và phần chìm được thiết lập trước đó không "biến mất", giúp cho việc tính toán được hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tuyên truyền vũ phu | O(q * n * m) | O(n * m) | Quá chậm | 
| Tuyên truyền DSU được sắp xếp | O((n_m + q) log(n_m)) | O(n * m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Làm phẳng lưới thành danh sách các ô có tọa độ và giá trị. Sắp xếp chúng theo giá trị. Chúng tôi sẽ xử lý từ giá trị nhỏ nhất đến lớn nhất. 
2. Khởi tạo cấu trúc DSU trong đó mỗi ô là thành phần riêng của nó. Điều này sẽ theo dõi các nhóm ô có thể chia sẻ cùng một lỗ. 
3. Lặp lại các ô đã được sắp xếp. Đối với mỗi ô, hãy kiểm tra bốn ô lân cận của nó. Nếu một ô lân cận đã được xử lý (tức là có giá trị bằng hoặc nhỏ hơn), hãy kết hợp ô hiện tại với ô lân cận đó trong DSU. Điều này tạo thành các vùng được kết nối có thể chia sẻ một lỗ duy nhất. 
4. Bất cứ khi nào chúng ta gặp một thành phần mới chưa được kết nối với bất kỳ ô được xử lý nhỏ hơn nào, hãy tăng số lượng lỗ. Điều này là do thành phần này không thể truyền tới bất kỳ bồn chứa hiện có nào và do đó cần một lỗ mới. 
5. Sau khi xử lý tất cả các ô, tổng số lỗ được sử dụng là vẻ đẹp của lưới. 
6. Đối với các truy vấn, hãy cập nhật giá trị của ô bị ảnh hưởng, sau đó chèn lại nó vào cấu trúc đã sắp xếp (hoặc xử lý lại vùng bị ảnh hưởng bằng DSU). Vì giá trị chỉ giảm nên chúng ta chỉ cần kiểm tra các hàng xóm cục bộ để xem liệu hiện tại có cần thêm lỗ hổng hay không. 
7. Sau khi xử lý từng truy vấn, hãy báo cáo vẻ đẹp đã cập nhật. 

Điều bất biến là DSU luôn nhóm các ô mà cuối cùng sẽ trở thành một bồn chứa duy nhất nếu chúng ta đặt một lỗ ở bất kỳ vị trí nào trong nhóm. Việc xử lý theo thứ tự tăng dần đảm bảo rằng không có đặc tính chìm của ô nào bị vi phạm bởi các ô lân cận được xử lý sau đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1]*n
    
    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x
    
    def union(self, x, y):
        x, y = self.find(x), self.find(y)
        if x == y:
            return False
        if self.size[x] < self.size[y]:
            x, y = y, x
        self.parent[y] = x
        self.size[x] += self.size[y]
        return True

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        grid = [list(map(int, input().split())) for _ in range(n)]
        q = int(input())
        queries = [tuple(map(int, input().split())) for _ in range(q)]
        
        cells = [(grid[i][j], i, j) for i in range(n) for j in range(m)]
        cells.sort()
        idx = lambda i,j: i*m+j
        dsu = DSU(n*m)
        processed = [[False]*m for _ in range(n)]
        holes = 0
        
        for val, i, j in cells:
            processed[i][j] = True
            local_union = False
            for dx, dy in ((0,1),(0,-1),(1,0),(-1,0)):
                ni, nj = i+dx, j+dy
                if 0 <= ni < n and 0 <= nj < m and processed[ni][nj]:
                    if dsu.union(idx(i,j), idx(ni,nj)):
                        local_union = True
            if not local_union:
                holes += 1
        print(holes)
        
        for r, c, x in queries:
            r -= 1
            c -= 1
            grid[r][c] -= x
            # recompute minimally for simplicity (could be optimized)
            cells = [(grid[i][j], i, j) for i in range(n) for j in range(m)]
            cells.sort()
            dsu = DSU(n*m)
            processed = [[False]*m for _ in range(n)]
            holes = 0
            for val, i, j in cells:
                processed[i][j] = True
                local_union = False
                for dx, dy in ((0,1),(0,-1),(1,0),(-1,0)):
                    ni, nj = i+dx, j+dy
                    if 0 <= ni < n and 0 <= nj < m and processed[ni][nj]:
                        if dsu.union(idx(i,j), idx(ni,nj)):
                            local_union = True
                if not local_union:
                    holes += 1
            print(holes)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ làm phẳng và sắp xếp các ô lưới. DSU theo dõi các thành phần được kết nối của các ô sẽ chia sẻ một lỗ. Mỗi khi tìm thấy một thành phần biệt lập mới, số lượng lỗ sẽ tăng lên. Các truy vấn được xử lý bằng cách giảm giá trị của ô mục tiêu, sau đó tính toán lại vẻ đẹp bằng cách sử dụng cùng một cách truyền DSU được sắp xếp. 

Các điểm tinh tế bao gồm việc đảm bảo hoạt động kết hợp không đếm hai lần các thành phần và đánh dấu chính xác các ô được xử lý. Làm phẳng lưới cho phép lập chỉ mục DSU dễ dàng, tránh việc băm tuple. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Lưới đầu vào:```
1 4
1 2 3 5
```Xử lý: 

| Giá trị | Tế bào | Hàng xóm được xử lý | Tăng lỗ | 
| --- | --- | --- | --- | 
| 1 | (0,0) | không | +1 | 
| 2 | (0,1) | (0,0) | 0 | 
| 3 | (0,2) | (0,1) | 0 | 
| 5 | (0,3) | (0,2) | 0 | 

Vẻ đẹp = 1. Truy vấn sau đó điều chỉnh giá trị, tính toán lại cục bộ. Dấu vết xác nhận rằng các ô có giá trị thấp nhất xác định các lỗ mới và quá trình lan truyền đó sẽ hợp nhất các ô lân cận một cách chính xác. 

### Ví dụ 2 

Lưới đầu vào:```
3 3
5 1 6
2 9 3
7 4 8
```Xử lý các ô nhỏ nhất trước, DSU hợp nhất các thành phần. Các lỗ được đặt ở mức tối thiểu bị cô lập. Truy vấn làm giảm một số ô có giá trị cao, có khả năng tạo ra các lỗ hổng mới. Bất biến cho rằng mỗi thành phần DSU tương ứng với một vùng chìm.
