---
title: "CF 1494D - Dogeforce"
description: "Chúng tôi được yêu cầu xây dựng lại hệ thống phân cấp của công ty từ một phần thông tin về lương. Cụ thể, chúng ta biết mức lương của tất cả các nhân viên cấp thấp nhất và đối với mỗi cặp trong số họ, mức lương của người giám sát chung thấp nhất của họ."
date: "2026-06-10T22:10:48+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "dfs-and-similar", "divide-and-conquer", "dsu", "greedy", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1494
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 105 (Rated for Div. 2)"
rating: 2300
weight: 1494
solve_time_s: 162
verified: false
draft: false
---

[CF 1494D - Dogeforces](https://codeforces.com/problemset/problem/1494/D) 

**Đánh giá:** 2300 
**Thẻ:** thuật toán xây dựng, cấu trúc dữ liệu, dfs và tương tự, phân chia và chinh phục, dsu, tham lam, sắp xếp, cây cối 
**Thời gian giải:** 2m 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xây dựng lại hệ thống phân cấp của công ty từ một phần thông tin về lương. Cụ thể, chúng ta biết mức lương của tất cả các nhân viên cấp thấp nhất và đối với mỗi cặp trong số họ, mức lương của người giám sát chung thấp nhất của họ. Hệ thống phân cấp là một cây trong đó mỗi nhân viên không thuộc lá có ít nhất hai cấp dưới và mỗi người giám sát có mức lương cao hơn tất cả cấp dưới trực tiếp của họ. Người đứng đầu công ty là gốc cây. Mục tiêu của chúng tôi là xây dựng lại cây này, gán số cho tất cả nhân viên bao gồm cả những nhân viên không có lá và đưa ra mức lương cùng với các mối quan hệ với người giám sát. 

Đầu vào cung cấp một$n \times n$ma trận ở đâu$a_{i,j}$là mức lương của người quản lý chung thấp nhất của nhân viên cấp dưới$i$Và$j$. Đường chéo$a_{i,i}$là tiền lương của người lao động$i$chính nó. Các hạn chế ở mức vừa phải:$n$có thể lên tới 500 và mức lương là số nguyên lên tới 5000. Điều này cho thấy rằng một$O(n^3)$cách tiếp cận có thể hầu như không phù hợp nhưng$O(n^4)$sẽ là quá chậm. 

Các trường hợp khó khăn không rõ ràng bao gồm tình huống nhiều cặp có cùng mức lương giám sát. Một cách tiếp cận đơn giản kết nối từng cặp một cách độc lập có thể tạo ra các nút hoặc chu trình trùng lặp. Ví dụ, nếu ba nhân viên$i, j, k$có$a_{i,j} = a_{i,k} = a_{j,k}$, thuật toán phải nhận ra rằng họ có chung một người giám sát chứ không phải ba người giám sát riêng biệt. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ là thử tất cả các cấu trúc cây có thể có và xác nhận mức lương của người giám sát dựa trên ma trận. Vì$n=500$, điều này rõ ràng là không khả thi vì số lượng cây tăng theo cấp số nhân. 

Điểm mấu chốt là điều này tương đương với việc xây dựng cây phân cụm theo thứ bậc (sơ đồ dendro) sử dụng tiêu chí "liên kết tối đa" hoặc "liên kết hoàn chỉnh", trong đó mức lương của người giám sát tương ứng với mức lương nhỏ nhất có thể kết nối một nhóm nhân viên. Sắp xếp tất cả các mức lương duy nhất theo thứ tự tăng dần cho phép chúng tôi dần dần hợp nhất các nhân viên thành các nhóm dưới sự giám sát chung với mức lương đó. Khi một nhóm được thành lập với mức lương$s$, nó không thể sáp nhập vào một nhóm với mức lương thấp hơn, đảm bảo chúng tôi tôn trọng điều kiện giám sát ngày càng nghiêm ngặt. 

Vì vậy, cách tiếp cận tối ưu là một thuật toán kiểu tìm kết hợp mang tính xây dựng. Mỗi nhân viên cấp thấp hơn bắt đầu như một nút riêng. Chúng tôi xem xét tất cả các cặp$(i,j)$để tăng lương giám sát viên và bất cứ khi nào một cặp thuộc các nhóm riêng biệt, chúng tôi hợp nhất chúng dưới một nút giám sát mới với mức lương nhất định. Bằng cách lặp lại từ mức lương nhỏ nhất đến mức lương lớn nhất, chúng tôi đảm bảo rằng cấp dưới luôn thấp hơn người giám sát của họ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Dendrogram mang tính xây dựng / Union-Find | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo từng nhân viên cấp thấp hơn$1$ĐẾN$n$như một tập hợp đơn lẻ trong cấu trúc tìm liên kết. Hãy ấn định mức lương của họ theo đường chéo của ma trận. 
2. Thu thập tất cả các cặp$(i,j)$với mức lương của người giám sát của họ$a_{i,j}$. Sắp xếp các cặp theo mức lương tăng dần. Điều này đảm bảo trước tiên chúng tôi xử lý các mức lương nhỏ hơn, tương ứng với các cấp độ sâu hơn của cây. 
3. Khởi tạo bộ đếm để gán mã số nhân viên mới bắt đầu từ$n+1$dành cho người giám sát. 
4. Cho mỗi cặp$(i,j)$Thứ tự tăng lương giám sát: 

- Tìm các đại diện gốc hiện tại của$i$Và$j$. 
- Nếu người đại diện khác nhau: 

- Nếu không có mức lương tương đương$a_{i,j}$, tạo nút giám sát mới với mức lương$a_{i,j}$và hợp nhất hai bộ dưới nút mới này. 
- Nếu một trong những người đại diện đã có lương$a_{i,j}$, hợp nhất tập hợp khác bên dưới nó. 
- Sử dụng Union-find để duy trì nhóm hiệu quả. 
5. Sau khi xử lý tất cả các cặp, chỉ còn một nút chưa được hợp nhất. Nút này là người đứng đầu công ty. 
6. Thu thập danh sách cuối cùng về lương và mối quan hệ với người giám sát. 

Tại sao nó hoạt động: ở mỗi bước, chúng tôi chỉ hợp nhất các nhóm có người giám sát chung thấp nhất phù hợp với mức lương nhất định. Việc sắp xếp theo mức tăng lương đảm bảo rằng mọi sự sáp nhập đều tôn trọng điều kiện người giám sát kiếm được nhiều hơn cấp dưới. Union-find đảm bảo không có chu kỳ nào được đưa ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = [list(map(int, input().split())) for _ in range(n)]

parent = list(range(2*n))  # union-find parent
salary = [0]*(2*n)
edges = []

for i in range(n):
    salary[i] = a[i][i]

next_node = n

def find(x):
    while parent[x] != x:
        parent[x] = parent[parent[x]]
        x = parent[x]
    return x

pairs = []
for i in range(n):
    for j in range(i+1, n):
        pairs.append((a[i][j], i, j))
pairs.sort()

for s, u, v in pairs:
    ru, rv = find(u), find(v)
    if ru == rv:
        continue
    if salary[ru] == s:
        parent[rv] = ru
        edges.append((rv+1, ru+1))
    elif salary[rv] == s:
        parent[ru] = rv
        edges.append((ru+1, rv+1))
    else:
        salary[next_node] = s
        parent[ru] = next_node
        parent[rv] = next_node
        edges.append((ru+1, next_node+1))
        edges.append((rv+1, next_node+1))
        next_node += 1

k = next_node
roots = [i for i in range(k) if parent[i]==i]
head = roots[0]+1

print(k)
print(' '.join(str(salary[i]) for i in range(k)))
print(head)
for u,v in edges:
    print(u,v)
```Giải pháp đầu tiên khởi tạo nhân viên và tiền lương của họ. Nó tạo ra một cấu trúc tìm kiếm liên minh để hợp nhất các nhóm. Sắp xếp các cặp lương đảm bảo việc sáp nhập tôn trọng mệnh lệnh của người giám sát. Khi cần người giám sát mới, chúng tôi sẽ chỉ định dần dần số nút mới. Phần đầu cuối cùng là phần gốc còn lại. 

## Ví dụ đã hoạt động 

Mẫu 1 Đầu vào:```
3
2 5 7
5 1 7
7 7 4
```| Bước | Cặp | Mức lương | Đại diện | Hành động | Cạnh | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (1,2) | 5 | 1,2 | hợp nhất dưới nút mới 4 | 1->4, 2->4 | 
| 2 | (1,3) | 7 | 4,3 | hợp nhất dưới nút mới 5 | 4->5,3->5 | 
| 3 | (2,3) | 7 | 5,5 | cùng nhóm | bỏ qua | 

Số nút cuối cùng: 5 

Các cạnh: 1-4, 2-4, 4-5, 3-5 

Đầu: 5 

Dấu vết này chứng tỏ rằng mức lương của người giám sát nhỏ nhất sẽ tạo ra các nút mới trước tiên và việc hợp nhất tôn trọng các ràng buộc của ma trận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Chúng tôi xử lý tất cả các cặp n nhân viên (≈ n^2/2) và mỗi thao tác tìm liên kết gần như không đổi | 
| Không gian | O(n^2) | Lưu trữ ma trận lương và mảng tìm công đoàn | 

Cho n 500, các phép toán n^3 ≈ 1.25e8 có thể được chấp nhận trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    output = io.StringIO()
    sys.stdout = output
    # call the main code
    n = int(input())
    a = [list(map(int, input().split())) for _ in range(n)]

    parent = list(range(2*n))
    salary = [0]*(2*n)
    edges = []

    for i in range(n):
        salary[i] = a[i][i]

    next_node = n

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    pairs = []
    for i in range(n):
        for j in range(i+1, n):
            pairs.append((a[i][j], i, j))
    pairs.sort()

    for s, u, v in pairs:
        ru, rv = find(u), find(v)
        if ru == rv:
            continue
        if salary[ru] == s:
            parent[rv] = ru
            edges.append((rv+1, ru+1))
        elif salary[rv] == s:
            parent[ru] = rv
            edges.append((ru+1, rv+1))
        else:
            salary[next_node] = s
            parent[ru] = next_node
            parent[rv] = next_node
            edges.append((ru+1, next_node+1))
            edges.append((rv+1, next_node+1))
            next_node += 1

    k = next_node
    roots = [i for i in range(k) if parent[i]==i]
    head = roots[0]+1

    print(k)
    print(' '.join(str(salary[i]) for i in range(k)))
    print(head
```
