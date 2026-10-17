---
title: "CF 105494H - Hệ thống phân cấp"
description: "Chúng ta có một tập hợp các nhân viên trong đó mỗi nhân viên thuộc về chính xác một công ty và mỗi công ty được cấu trúc theo hệ thống phân cấp bắt nguồn từ một Giám đốc điều hành."
date: "2026-06-23T21:03:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105494
codeforces_index: "H"
codeforces_contest_name: "2024-2025 ICPC NERC, Kyrgyzstan Qualification Contest"
rating: 0
weight: 105494
solve_time_s: 60
verified: true
draft: false
---

[CF 105494H - Hệ thống phân cấp](https://codeforces.com/problemset/problem/105494/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các nhân viên trong đó mỗi nhân viên thuộc về chính xác một công ty và mỗi công ty được cấu trúc theo hệ thống phân cấp bắt nguồn từ một Giám đốc điều hành. Nếu chúng ta mô hình hóa nhân viên dưới dạng các đỉnh và báo cáo các mối quan hệ dưới dạng các cạnh thì mỗi công ty sẽ tạo thành một cái cây và toàn bộ hệ thống là một rừng những cây như vậy. 

Nhiệm vụ là xác định quy mô của mỗi công ty, nghĩa là đối với mỗi nhân viên, chúng tôi muốn biết có bao nhiêu người trong hệ thống phân cấp được kết nối của họ. Vì tất cả nhân viên trong cùng một cây đều có cùng quy mô công ty nên điều này tương đương với việc tính toán kích thước của từng thành phần được kết nối trong một khu rừng vô hướng. 

Mặc dù câu chuyện được diễn đạt dưới góc độ CEO và cấp dưới, cấu trúc tính toán chỉ đơn giản là một đồ thị vô hướng không có chu trình. Mỗi cây tương ứng với một công ty và câu trả lời cho bất kỳ nút nào là kích thước của cây của nó. 

Các ràng buộc trong loại bài toán này thường cho phép tối đa khoảng 200.000 nút và cạnh, điều này ngay lập tức loại trừ mọi phương pháp bậc hai, chẳng hạn như kiểm tra khả năng kết nối giữa mỗi cặp nút. Bất kỳ giải pháp nào cố gắng lặp lại DFS hoặc BFS từ mọi nút một cách độc lập sẽ giảm xuống O(n^2) trong trường hợp xấu nhất, điều này vượt xa mức có thể chấp nhận được. Chúng ta cần một đường truyền tuyến tính hoặc gần tuyến tính trên cấu trúc. 

Trường hợp cạnh tinh tế xuất hiện khi khu rừng chứa nhiều cây nút đơn. Ví dụ: nếu có các nút không có cạnh nào cả, mỗi nút trong số chúng tạo thành một công ty có kích thước 1. Một trường hợp góc khác là một chuỗi lớn duy nhất, trong đó việc duyệt qua lặp lại ngây thơ có thể quay lại các đường dẫn dài nhiều lần nếu không được đánh dấu cẩn thận. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là coi mỗi nút là một nút gốc tiềm năng và chạy DFS hoặc BFS để tính toán xem có bao nhiêu nút có thể truy cập được từ nút đó. Điều này đúng vì khả năng tiếp cận trong biểu đồ vô hướng xác định chính xác các thành phần được kết nối. Tuy nhiên, thực hiện việc này một cách độc lập cho mỗi nút sẽ gây ra việc tính toán lại rất lớn. Trong một chuỗi có độ dài n, một DFS đã có giá O(n) và việc lặp lại nó n lần sẽ dẫn đến các hoạt động O(n^2), điều này không khả thi. 

Quan sát chính là cấu trúc biểu đồ không thay đổi giữa các truy vấn. Mỗi thành phần được kết nối có kích thước cố định và mọi nút bên trong nó đều có cùng một câu trả lời. Điều này có nghĩa là chúng ta chỉ cần tính toán từng thành phần một lần, sau đó truyền kích thước của nó đến tất cả các nút trong thành phần đó. 

Có hai cách tiêu chuẩn để đạt được điều này một cách hiệu quả. Một là truyền tải đồ thị bằng DFS hoặc BFS. Chúng tôi truy cập mỗi nút chính xác một lần, tích lũy kích thước thành phần của nó trong quá trình truyền tải và gán kích thước đó cho mọi nút được truy cập. Thứ hai là Disjoint Set Union, nơi chúng tôi hợp nhất các điểm cuối của các cạnh và duy trì kích thước thành phần trong gốc DSU. 

Cả hai cách tiếp cận đều dựa trên cùng một ý tưởng: nén rừng thành các tập hợp riêng biệt và tính toán kích thước trên mỗi tập hợp thay vì trên mỗi nút. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DFS từ mỗi nút | O(n^2) | O(n) | Quá chậm | 
| DFS/BFS mỗi thành phần | O(n) | O(n) | Đã chấp nhận | 
| DSU với liên minh theo quy mô | O(n α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi mô tả cách tiếp cận DFS/BFS vì đây là cách giải thích trực tiếp nhất về vấn đề.

1. Xây dựng danh sách kề cho rừng từ các cạnh cho trước. Biểu diễn này cho phép chúng ta duyệt qua các nút lân cận của mỗi nút một cách hiệu quả mà không cần quét các cặp không liên quan. 
2. Duy trì mảng đã truy cập được khởi tạo thành false cho tất cả các nút. Điều này đảm bảo mỗi nút được xử lý chính xác một lần, ngăn chặn việc truyền tải lặp lại trên cùng một thành phần. 
3. Lặp qua tất cả các nút từ 1 đến n. Khi chúng tôi tìm thấy một nút chưa được truy cập, chúng tôi bắt đầu truyền tải từ nút đó. Nút này đóng vai trò là điểm vào của cây công ty mới. 
4. Trong quá trình truyền tải, chúng tôi thực hiện DFS hoặc BFS, thu thập tất cả các nút có thể truy cập được từ nút bắt đầu. Mỗi nút đã truy cập được đánh dấu để tránh truy cập lại. 
5. Trong khi duyệt một thành phần, chúng tôi đếm xem có bao nhiêu nút được phát hiện. Con số này là quy mô của công ty hiện tại. 
6. Sau khi kết thúc quá trình truyền tải một thành phần, chỉ định kích thước được tính toán cho mọi nút được thu thập trong quá trình truyền tải đó. 

Cách tiếp cận DSU tuân theo logic tương tự nhưng thay thế việc truyền tải bằng các phép toán hợp. Mỗi cạnh hợp nhất hai bộ và mỗi gốc của bộ duy trì một bộ đếm kích thước. Sau khi xử lý tất cả các cạnh, câu trả lời cho mỗi nút chỉ đơn giản là kích thước của đại diện của nó. 

Tại sao nó hoạt động: đánh dấu đã truy cập trong DFS hoặc con trỏ đại diện trong DSU, đảm bảo rằng mỗi thành phần được kết nối được xác định chính xác một lần. Vì khả năng kết nối phân chia biểu đồ thành các tập hợp rời rạc nên mỗi nút thuộc về chính xác một thành phần và mỗi thành phần được tính chính xác một lần. Không có nút nào có thể được tính hai lần vì một khi nó được gán cho một thành phần hoặc được hợp nhất dưới gốc, nó sẽ không bao giờ được xử lý lại dưới dạng một thực thể riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    g = [[] for _ in range(n + 1)]
    
    for _ in range(m):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)
    
    vis = [False] * (n + 1)
    ans = [0] * (n + 1)
    
    sys.setrecursionlimit(10**7)
    
    def dfs(start):
        stack = [start]
        comp = []
        vis[start] = True
        
        while stack:
            u = stack.pop()
            comp.append(u)
            for v in g[u]:
                if not vis[v]:
                    vis[v] = True
                    stack.append(v)
        
        size = len(comp)
        for node in comp:
            ans[node] = size
    
    for i in range(1, n + 1):
        if not vis[i]:
            dfs(i)
    
    print(*ans[1:])

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên là xây dựng danh sách lân cận để mỗi nhân viên biết kết nối trực tiếp của họ trong hệ thống phân cấp. DFS được triển khai lặp đi lặp lại bằng cách sử dụng một ngăn xếp để tránh các vấn đề về độ sâu đệ quy, đặc biệt quan trọng khi hệ thống phân cấp thoái hóa thành một chuỗi dài. 

Mỗi lần chúng tôi gặp một nút chưa được truy cập, chúng tôi sẽ khám phá toàn bộ thành phần được kết nối của nút đó. Danh sách comp lưu trữ tất cả các nút trong thành phần đó để sau khi truyền tải, chúng ta có thể gán cho chúng cùng kích thước được tính toán. Điều này tránh việc tính toán lại kích thước nhiều lần. 

Mảng ans lưu trữ kích thước công ty cuối cùng trên mỗi nút và chúng tôi in tất cả các giá trị ngoại trừ chỉ số 0. 

## Ví dụ đã hoạt động 

Hãy xem xét một khu rừng nhỏ có hai thành phần: một chuỗi gồm ba nút và một nút cô lập. 

đầu vào:```
4 2
1 2
2 3
```| Bước | Nút | Hành động | Thành phần | 
| --- | --- | --- | --- | 
| 1 | 1 | bắt đầu DFS | [1, 2, 3] | 
| 2 | 4 | DFS mới | [4] | 

Sau khi xử lý, các nút 1, 2, 3 có kích thước 3, nút 4 có kích thước 1. 

Điều này chứng tỏ cách truyền tải tự động phân tách các cấu trúc bị ngắt kết nối và gán kích thước cho mỗi thành phần thay vì cho mỗi nút. 

Bây giờ hãy xem xét một biểu đồ bị ngắt kết nối hoàn toàn: 

đầu vào:```
3 0
```| Bước | Nút | Hành động | Thành phần | 
| --- | --- | --- | --- | 
| 1 | 1 | DFS | [1] | 
| 2 | 2 | DFS | [2] | 
| 3 | 3 | DFS | [3] | 

Mỗi nút tạo thành công ty riêng của mình, xác nhận rằng các đỉnh bị cô lập được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi nút và cạnh được xử lý một lần trong DFS | 
| Không gian | O(n + m) | Cấu trúc đồ thị lưu trữ danh sách kề và mảng phụ trợ | 

Thuật toán dễ dàng phù hợp với các ràng buộc điển hình lên tới 200.000 nút và cạnh vì nó chỉ thực hiện công việc tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque

    data = inp.strip().split()
    if not data:
        return ""
    
    n, m = map(int, data[:2])
    g = [[] for _ in range(n + 1)]
    
    idx = 2
    for _ in range(m):
        u = int(data[idx]); v = int(data[idx+1]); idx += 2
        g[u].append(v)
        g[v].append(u)
    
    vis = [False] * (n + 1)
    ans = [0] * (n + 1)
    
    sys.setrecursionlimit(10**7)
    
    def dfs(s):
        stack = [s]
        comp = []
        vis[s] = True
        while stack:
            u = stack.pop()
            comp.append(u)
            for v in g[u]:
                if not vis[v]:
                    vis[v] = True
                    stack.append(v)
        size = len(comp)
        for x in comp:
            ans[x] = size
    
    for i in range(1, n + 1):
        if not vis[i]:
            dfs(i)
    
    return " ".join(map(str, ans[1:]))

# single node
assert run("1 0") == "1"

# chain
assert run("4 3\n1 2\n2 3\n3 4") == "4 4 4 4"

# two components
assert run("5 3\n1 2\n2 3\n4 5") == "3 3 3 2 2"

# isolated nodes
assert run("3 0") == "1 1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 1 | thành phần nút đơn | 
| chuỗi 4 nút | 4 4 4 4 | nhân giống cây tuyến tính | 
| chia thành phần | 3 3 3 2 2 | nhiều cây | 
| không có cạnh | 1 1 1 | tất cả các nút bị cô lập | 

## Vỏ cạnh 

Đối với biểu đồ một nút, thuật toán ngay lập tức đánh dấu nút đó là đã truy cập và gán cho nó kích thước thành phần là một. Không có hàng xóm nào để xử lý, do đó ngăn xếp DFS sẽ trống ngay lập tức và tính chính xác sẽ xuất hiện ngay từ khi khởi tạo. 

Đối với biểu đồ bị ngắt kết nối hoàn toàn, mỗi lần lặp của vòng lặp bên ngoài sẽ kích hoạt lệnh gọi DFS mới. Vì không tồn tại cạnh nào nên mỗi DFS chỉ chạm vào một nút duy nhất, tạo ra các phép gán kích thước chính xác một cách độc lập. 

Đối với một chuỗi dài, ngăn xếp lặp đảm bảo chúng ta không bao giờ vượt quá giới hạn đệ quy. Mỗi nút được đẩy và bật chính xác một lần, do đó, ngay cả trong cấu trúc có độ lệch tồi tệ nhất, quá trình truyền tải vẫn tuyến tính mà không có rủi ro tràn ngăn xếp.
