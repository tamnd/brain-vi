---
title: "CF 1167C - Phân phối tin tức"
description: "Chúng ta có một mạng xã hội với n người dùng và m nhóm bạn. Mỗi nhóm chứa một danh sách những người dùng là bạn bè của nhau."
date: "2026-06-12T02:11:02+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dsu", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1167
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 65 (Rated for Div. 2)"
rating: 1400
weight: 1167
solve_time_s: 198
verified: true
draft: false
---

[CF 1167C - Phân phối tin tức](https://codeforces.com/problemset/problem/1167/C) 

**Đánh giá:** 1400 
**Tags:** dfs và tương tự, dsu, đồ thị 
**Thời gian giải:** 3 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một mạng xã hội với`n`người dùng và`m`các nhóm bạn. Mỗi nhóm chứa một danh sách những người dùng là bạn bè của nhau. Tình bạn có tính chất bắc cầu thông qua các nhóm này: nếu hai người dùng trong cùng một nhóm thì họ là bạn bè và tin tức sẽ lan truyền qua chuỗi tình bạn. Đối với mỗi người dùng, chúng tôi được yêu cầu tính toán xem có bao nhiêu người dùng cuối cùng sẽ biết một mẩu tin tức nếu người dùng đó là nguồn ban đầu duy nhất. 

Về mặt khái niệm, đây là một vấn đề đồ thị. Mỗi người dùng là một nút và có một cạnh giữa hai người dùng nếu họ thuộc cùng một nhóm. Tin tức lan truyền dọc theo các thành phần được kết nối của biểu đồ này. Do đó, số lượng người dùng cuối cùng biết tin tức bắt đầu từ người dùng`x`bằng với kích thước của thành phần được kết nối có chứa`x`. 

Các ràng buộc rất cao:`n`Và`m`có thể đi lên`5 * 10^5`, và tổng số thành viên nhóm cũng lên tới`5 * 10^5`. Bất kỳ giải pháp nào tính toán rõ ràng các lân cận cho từng người dùng và chạy BFS hoặc DFS từ mọi nút sẽ quá chậm vì nó có thể yêu cầu`O(n*(n+m))`hoạt động trong trường hợp xấu nhất. Điều này buộc chúng ta phải tìm ra một cách tiếp cận tránh việc duyệt lặp lại cùng một thành phần. 

Các trường hợp đặc biệt không rõ ràng bao gồm các nhóm trống và nhóm người dùng đơn lẻ. Một nhóm với`0`người dùng nên bị bỏ qua vì nó không kết nối với ai. Một nhóm có một người dùng không tăng khả năng kết nối nhưng không được phá vỡ logic hợp nhất. Ví dụ: một đầu vào trong đó tất cả các nhóm trống sẽ tạo ra đầu ra là`1`cho mỗi người dùng, vì mỗi người dùng tạo thành một thành phần đơn lẻ. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là xây dựng danh sách kề của tất cả người dùng, sau đó chạy BFS hoặc DFS từ mọi người dùng để đếm kích thước của thành phần được kết nối. Điều này đúng nhưng không hiệu quả. Vì`n = 5*10^5`, đang chạy BFS`n`thời gian cực kỳ chậm vì các hoạt động tích lũy có thể vượt quá`10^{11}`trong trường hợp xấu nhất. 

Cái nhìn sâu sắc quan trọng là kết nối có tính đối xứng và bắc cầu. Người dùng trong cùng một thành phần được kết nối sẽ luôn có cùng một câu trả lời cuối cùng. Điều này gợi ý rằng chúng ta chỉ cần tính kích thước của từng thành phần được kết nối một lần. Cấu trúc tự nhiên cho việc này là Disjoint Set Union (DSU), còn được gọi là Union-Find. Chúng ta có thể lặp qua từng nhóm và hợp nhất tất cả các thành viên của nhóm đó thành một tập hợp. Sau khi xử lý tất cả các nhóm, mỗi bộ đại diện cho một thành phần được kết nối và kích thước của mỗi bộ sẽ đưa ra câu trả lời cho tất cả các thành viên của nó. Điều này làm giảm độ phức tạp về thời gian xuống gần tuyến tính trong tổng số thành viên nhóm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force BFS/DFS từ mọi người dùng | O(n*(n+m)) | O(n+m) | Quá chậm | 
| DSU / Union-Tìm trên nhóm | O(n + m + tổng kích thước nhóm * α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo DSU với`n`các phần tử, trong đó mỗi người dùng ban đầu thuộc về nhóm riêng của họ. Mỗi bộ theo dõi kích thước của nó. 
2. Lặp lại từng nhóm. Đối với một nhóm có quy mô`k`, hợp nhất tất cả`k`người dùng vào một bộ. Chúng tôi có thể thực hiện việc này bằng cách hợp nhất tuần tự người dùng đầu tiên với từng người dùng còn lại. 
3. Sau khi tất cả các nhóm được xử lý, mỗi thành phần được kết nối đã được hợp nhất thành một bộ và kích thước của bộ đại diện cho số lượng người dùng có thể truy cập từ bất kỳ thành viên nào. 
4. Đối với mỗi người dùng từ`1`ĐẾN`n`, xuất ra kích thước của tập hợp mà chúng thuộc về. Đây là số lượng người dùng sẽ biết tin tức nếu người dùng đó bắt đầu phân phối nó. 

Tại sao nó hoạt động: Cấu trúc tìm liên kết đảm bảo rằng tất cả người dùng có thể liên hệ với nhau thông qua bất kỳ chuỗi nhóm nào đều được hợp nhất thành một nhóm duy nhất. Bằng cách theo dõi kích thước đã đặt, chúng tôi biết chính xác số lượng người dùng mà mỗi người dùng ban đầu có thể tiếp cận. Nén đường dẫn đảm bảo rằng`find`hoạt động nhanh chóng, tạo ra độ phức tạp tổng thể gần như tuyến tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**6)

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        x_root = self.find(x)
        y_root = self.find(y)
        if x_root == y_root:
            return
        if self.size[x_root] < self.size[y_root]:
            x_root, y_root = y_root, x_root
        self.parent[y_root] = x_root
        self.size[x_root] += self.size[y_root]

n, m = map(int, input().split())
dsu = DSU(n)

for _ in range(m):
    line = list(map(int, input().split()))
    k = line[0]
    if k == 0:
        continue
    users = [x-1 for x in line[1:]]
    first = users[0]
    for u in users[1:]:
        dsu.union(first, u)

result = [dsu.size[dsu.find(i)] for i in range(n)]
print(*result)
```Mã này xác định một DSU tiêu chuẩn có nén đường dẫn. Đối với mỗi nhóm, nó chuyển đổi ID người dùng thành các chỉ mục dựa trên 0 và liên kết tất cả các thành viên của nhóm. Sau tất cả các phép hợp, kích thước của tập hợp chứa mỗi người dùng sẽ được in ra. Sử dụng tính năng nén đường dẫn đảm bảo rằng`find`cuộc gọi nhanh chóng ngay cả đối với đầu vào lớn. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
7 5
3 2 5 4
0
2 1 2
1 1
2 6 7
```| Bước | Nhóm đã xử lý | Bang DSU (mẹ) | Kích thước thành phần | 
| --- | --- | --- | --- | 
| ban đầu | - | 0 1 2 3 4 5 6 | 1 1 1 1 1 1 1 | 
| nhóm1 2,5,4 | hợp nhất(2,5), hợp nhất(2,4) | 0 1 2 2 2 2 6 | 1 1 3 3 3 3 1 | 
| nhóm2 trống | - | không thay đổi | không thay đổi | 
| nhóm3 1,2 | hợp nhất (1,2) | 0 2 2 2 2 2 6 | 4 4 4 4 3 3 1 | 
| nhóm4 1 | độc thân | không thay đổi | không thay đổi | 
| nhóm5 6,7 | hợp nhất (5,6) | 0 2 2 2 2 5 5 | 4 4 4 4 4 2 2 | 

Đầu ra:`4 4 1 4 4 2 2`Dấu vết xác nhận rằng mỗi người dùng thuộc về một thành phần được kết nối có kích thước chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m + tổng kích thước nhóm * α(n)) | Mỗi hợp và tìm gần như không đổi; chúng tôi xử lý từng thành viên nhóm một lần | 
| Không gian | O(n) | Mảng kích thước và cha mẹ DSU lưu trữ một số nguyên cho mỗi người dùng | 

Với`n, m`lên đến`5*10^5`và tổng quy mô nhóm lên tới`5*10^5`, thuật toán dễ dàng nằm gọn trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    
    class DSU:
        def __init__(self, n):
            self.parent = list(range(n))
            self.size = [1]*n
        def find(self, x):
            if self.parent[x] != x:
                self.parent[x] = self.find(self.parent[x])
            return self.parent[x]
        def union(self, x, y):
            x_root = self.find(x)
            y_root = self.find(y)
            if x_root == y_root: return
            if self.size[x_root] < self.size[y_root]:
                x_root, y_root = y_root, x_root
            self.parent[y_root] = x_root
            self.size[x_root] += self.size[y_root]

    n, m = map(int, input().split())
    dsu = DSU(n)
    for _ in range(m):
        line = list(map(int, input().split()))
        k = line[0]
        if k == 0: continue
        users = [x-1 for x in line[1:]]
        first = users[0]
        for u in users[1:]:
            dsu.union(first, u)

    return ' '.join(str(dsu.size[dsu.find(i)]) for i in range(n))

# Provided sample
assert run("7 5\n3 2 5 4\n0\n2 1 2\n1 1\n2 6 7\n") == "4 4 1 4 4 2 2", "sample 1"

# Custom cases
assert run("5 0\n0\n0\n0\n0\n0\n") == "1 1 1 1 1", "all empty groups"
assert run("3 3\n1 1\n1 2\n1 3\n") == "1 1 1", "singletons"
assert run("4 2\n2 1 2\n2 3
```
