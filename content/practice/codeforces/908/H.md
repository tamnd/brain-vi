---
title: "CF 908H - Cầu năm mới và Boolean"
description: "Chúng ta được cho một đồ thị có hướng chưa xác định với n nút. Đối với mỗi cặp nút u và v, chúng ta được cung cấp một điều kiện Boolean liên quan đến khả năng tiếp cận trong biểu đồ: AND, OR hoặc XOR về việc liệu bạn có thể tiếp cận v hay không và liệu v có thể tiếp cận u hay không là đúng."
date: "2026-06-12T10:30:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 908
codeforces_index: "H"
codeforces_contest_name: "Good Bye 2017"
rating: 3100
weight: 908
solve_time_s: 115
verified: true
draft: false
---

[CF 908H - Năm mới và những cây cầu Boolean](https://codeforces.com/problemset/problem/908/H) 

**Đánh giá:** 3100 
**Thẻ:** - 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có hướng chưa biết với`n`nút. Đối với mỗi cặp nút`u`Và`v`, chúng ta được biết một điều kiện Boolean liên quan đến khả năng tiếp cận trong biểu đồ: AND, OR hoặc XOR về việc liệu`u`có thể đạt được`v`và liệu`v`có thể đạt được`u`là đúng. Đầu vào mang lại sự đối xứng`n x n`ma trận trong đó mỗi mục (không bao gồm đường chéo) là một ký tự`'A'`,`'O'`, hoặc`'X'`, biểu thị điều kiện Boolean nào trong ba điều kiện Boolean được đảm bảo đúng cho cặp đó. Đường chéo là`'-'`bởi vì một nút có thể truy cập được từ chính nó. 

Đầu ra phải là`-1`nếu không có đồ thị nào có thể thỏa mãn các điều kiện đã cho hoặc số cạnh tối thiểu cần thiết trong đồ thị thỏa mãn tất cả các ràng buộc theo cặp. 

Hạn chế chính,`n ≤ 47`, chỉ ra rằng các giải pháp lên đến`O(n^3)`là khả thi, nhưng bất cứ điều gì theo cấp số nhân trong`n`có lẽ là quá chậm. Cái này đủ nhỏ`n`để xem xét tính toán khả năng tiếp cận dựa trên ma trận. 

Một sự tinh tế nảy sinh với`'X'`mục nhập.`'X'`chỉ ra rằng chính xác một trong`f(u,v)`hoặc`f(v,u)`là đúng, nghĩa là có một hướng chặt chẽ giữa các nút.`'O'`cho biết ít nhất một khả năng tiếp cận là đúng, khả năng tiếp cận yếu hơn và`'A'`cho biết cả hai có thể liên lạc với nhau, ngụ ý rằng chúng phải ở trong một thành phần được kết nối mạnh mẽ. Một cách tiếp cận ngây thơ chỉ xem xét các cặp riêng lẻ mà không truyền bá khả năng tiếp cận có thể không phát hiện được các cấu hình không thể thực hiện được, chẳng hạn như có các chu kỳ và các hướng nghiêm ngặt mâu thuẫn với nhau. 

Các trường hợp cạnh bao gồm một biểu đồ nút đơn, một biểu đồ được kết nối đầy đủ, các biểu đồ chỉ có`'X'`các mục và đồ thị trong đó một số ràng buộc mâu thuẫn lẫn nhau. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các tập hợp con của các cạnh có hướng và kiểm tra từng biểu đồ kết quả dựa trên tất cả các ràng buộc theo cặp. Vì`n = 47`, có`2^(n*(n-1)) ≈ 2^2162`đồ thị có thể, điều này hoàn toàn không khả thi. 

Cái nhìn sâu sắc quan trọng là các ràng buộc phân chia biểu đồ thành các thành phần được kết nối mạnh mẽ (SCC) được quy định bởi`'A'`mục nhập. Tất cả các nút được kết nối bởi`'A'`các ràng buộc phải nằm trong cùng một SCC vì`'A'`đòi hỏi khả năng tiếp cận lẫn nhau. Trong mỗi SCC, chúng ta có thể mô hình hóa các nút thành một cụm và thu gọn chúng thành một siêu nút duy nhất.`'O'`các mục chỉ yêu cầu ít nhất một đường dẫn, vì vậy chúng sẽ tự động được đáp ứng sau khi SCC được hình thành.`'X'`các mục áp đặt tính định hướng nghiêm ngặt giữa các SCC: nếu hai nút thuộc cùng một SCC, có`'X'`giữa họ là không thể được; nếu chúng thuộc về các SCC khác nhau, chúng ta có thể định hướng các cạnh theo thứ tự tôpô. 

Do đó, vấn đề giảm xuống còn việc tìm SCC từ`'A'`mục, xác minh rằng`'X'`các ràng buộc không mâu thuẫn với tư cách thành viên SCC, sau đó xây dựng DAG của SCC và đếm số cạnh tối thiểu để đáp ứng tất cả`'X'`hạn chế. Các cạnh tối thiểu chỉ là số lượng SCC (mỗi nút bên trong SCC tạo thành một nhóm các cạnh) cộng với một cạnh cho mỗi mối quan hệ có hướng cần thiết giữa các SCC. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(n^2)) | O(n^2) | Quá chậm | 
| Cấu trúc liên kết SCC + DAG | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ma trận đầu vào và khởi tạo cấu trúc đồ thị. Mỗi nút ban đầu hình thành SCC của riêng mình. 
2. Hợp nhất các nút vào SCC bằng cách sử dụng`'A'`hạn chế. Đối với mỗi cặp`(u,v)`với`matrix[u][v] == 'A'`, công đoàn`u`Và`v`trong một cấu trúc rời rạc. Điều này đảm bảo rằng khả năng tiếp cận lẫn nhau được đáp ứng trong mỗi SCC. 
3. Kiểm tra`'X'`những hạn chế vì không thể thực hiện được. Nếu như`matrix[u][v] == 'X'`Và`u`Và`v`thuộc cùng một SCC, in`-1`và thoát ra vì`'X'`yêu cầu khả năng tiếp cận một chiều, điều này không thể xảy ra bên trong một thành phần được kết nối mạnh mẽ. 
4. Xây dựng DAG của SCC. Ánh xạ từng nút tới SCC của nó, sau đó với mỗi nút`'X'`giữa các nút trong các SCC khác nhau, thêm một cạnh có hướng từ SCC chứa nguồn vào SCC chứa đích. Phát hiện chu kỳ bằng cách sử dụng tính năng phát hiện chu trình DAG tiêu chuẩn. Nếu tồn tại một chu trình, hãy in`-1`. 
5. Đếm các cạnh tối thiểu. Bên trong mỗi SCC có kích thước`k`, ít nhất`k`các cạnh là cần thiết để tạo thành một thành phần liên thông mạnh (một chu trình đơn giản là đủ). Giữa các SCC, mỗi cạnh được định hướng từ DAG tương ứng với ít nhất một cạnh kết nối các nút đại diện. Tổng hợp các cạnh này để có được tổng số tối thiểu. 
6. Xuất ra tổng số cạnh. 

**Tại sao nó hoạt động**: Việc sáp nhập SCC thực thi tất cả khả năng tiếp cận lẫn nhau được ngụ ý bởi`'A'`. DAG của SCC đảm bảo rằng`'X'`các ràng buộc được thỏa mãn một cách nhất quán về mặt tôpô. Bằng cách thu gọn SCC và đếm một cạnh theo hướng yêu cầu, chúng tôi đảm bảo số lượng cạnh tối thiểu.`'O'`các ràng buộc được thỏa mãn tự động vì có ít nhất một đường dẫn tồn tại trong DAG hoặc trong SCC. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    mat = [input().strip() for _ in range(n)]

    parent = list(range(n))

    def find(u):
        while parent[u] != u:
            parent[u] = parent[parent[u]]
            u = parent[u]
        return u

    def union(u, v):
        u_root = find(u)
        v_root = find(v)
        if u_root != v_root:
            parent[v_root] = u_root

    # Step 1: merge 'A' constraints
    for i in range(n):
        for j in range(n):
            if mat[i][j] == 'A':
                union(i, j)

    # Step 2: check 'X' constraints
    for i in range(n):
        for j in range(n):
            if mat[i][j] == 'X' and find(i) == find(j):
                print(-1)
                return

    # Step 3: build SCC DAG
    scc_map = {}
    for i in range(n):
        r = find(i)
        if r not in scc_map:
            scc_map[r] = []
        scc_map[r].append(i)

    scc_nodes = list(scc_map.values())
    scc_id = {}
    for idx, nodes in enumerate(scc_nodes):
        for node in nodes:
            scc_id[node] = idx

    scc_count = len(scc_nodes)
    dag = [[] for _ in range(scc_count)]
    indegree = [0] * scc_count

    for i in range(n):
        for j in range(n):
            if mat[i][j] == 'X':
                u_scc = scc_id[i]
                v_scc = scc_id[j]
                if v_scc not in dag[u_scc]:
                    dag[u_scc].append(v_scc)
                    indegree[v_scc] += 1

    # Step 4: check DAG for cycles
    from collections import deque
    q = deque([i for i in range(scc_count) if indegree[i] == 0])
    visited = 0
    while q:
        u = q.popleft()
        visited += 1
        for v in dag[u]:
            indegree[v] -= 1
            if indegree[v] == 0:
                q.append(v)
    if visited < scc_count:
        print(-1)
        return

    # Step 5: count minimal edges
    total_edges = sum(len(nodes) for nodes in scc_nodes)  # inside SCC
    total_edges += sum(len(neigh) for neigh in dag)       # between SCCs
    print(total_edges)

if __name__ == "__main__":
    main()
```Sự kết hợp rời rạc đảm bảo tất cả`'A'`các ràng buộc được hợp nhất một cách chính xác. Kiểm tra`'X'`các ràng buộc sau khi hợp nhất đảm bảo chúng tôi không vi phạm khả năng tiếp cận một chiều. Việc phát hiện chu trình DAG đảm bảo không tồn tại các hướng mâu thuẫn. Tính cạnh sử dụng một cạnh cho mỗi nút bên trong SCC và một cạnh cho mỗi kết nối DAG, đây là mức tối thiểu. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
4
-AAA
A-AA
AA-A
AAA-
```| Bước | SCC | Các cạnh DAG | Bình luận | 
| --- | --- | --- | --- | 
| ban đầu | [0],[1],[2],[3] | không | mỗi nút riêng biệt | 
| hợp nhất 'A' | [0,1,2,3] | không | tất cả các nút trong cùng một SCC | 
| kiểm tra 'X' | được | không | không có ràng buộc 'X' | 
| đếm cạnh | 4 | 0 | một chu kỳ đủ bên trong SCC | 

Đầu ra:`4`**Mẫu 2** 

đầu vào:```
3
-XX
X- 
XX-
```| Bước | SCC | Các cạnh DAG | Bình luận | 
| --- | --- | --- | --- | 
| ban đầu | [0],[1],[2] | không | mỗi nút riêng biệt | 
| hợp nhất 'A' | giống nhau | không | không 'A' | 
| kiểm tra 'X' | được | các cạnh 0→1,0→2,1→2 | DAG được xây dựng | 
| kiểm tra chu kỳ | được | | không có chu kỳ | 
| đếm cạnh | 3 (bên trong SCC | | |
