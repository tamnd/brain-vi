---
title: "CF 208E - Anh Em Họ Máu"
description: "Chúng ta được cấp một khu rừng cội nguồn, nơi mỗi người có nhiều nhất một cha mẹ. Nếu chúng ta đi theo con trỏ cha mẹ đi lên, cuối cùng chúng ta sẽ đạt đến gốc hoặc rời khỏi cấu trúc. Điều này xác định một bộ sưu tập cây. “Tổ tiên thứ k” có nghĩa là áp dụng quan hệ cha mẹ k lần."
date: "2026-06-03T17:24:34+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dfs-and-similar", "trees"]
categories: ["algorithms"]
codeforces_contest: 208
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 130 (Div. 2)"
rating: 2100
weight: 208
solve_time_s: 110
verified: false
draft: false
---

[CF 208E - Anh em họ máu](https://codeforces.com/problemset/problem/208/E) 

**Xếp hạng:** 2100 
**Thẻ:** tìm kiếm nhị phân, cấu trúc dữ liệu, dfs và tương tự, cây 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một khu rừng cội nguồn, nơi mỗi người có nhiều nhất một cha mẹ. Nếu chúng ta đi theo con trỏ cha mẹ đi lên, cuối cùng chúng ta sẽ đạt đến gốc hoặc rời khỏi cấu trúc. Điều này xác định một bộ sưu tập cây. 

“Tổ tiên thứ k” có nghĩa là áp dụng quan hệ cha mẹ k lần. Vì vậy, tổ tiên 1 là cha mẹ, tổ tiên 2 là cha mẹ của cha mẹ, v.v., miễn là chuỗi tồn tại. 

Đối với mỗi truy vấn bao gồm một người v và khoảng cách p, chúng tôi được yêu cầu đếm xem có bao nhiêu người khác có cùng tổ tiên thứ p với v. Nói cách khác, chúng tôi xem xét nút u có được bằng cách leo p bước từ v và chúng tôi đếm có bao nhiêu nút trong toàn bộ khu rừng cũng có u là tổ tiên thứ p của chúng. Câu trả lời loại trừ chính v. 

Các ràng buộc đẩy chúng tôi ra khỏi mọi quá trình truyền tải theo mỗi truy vấn. Với tối đa 100000 nút và 100000 truy vấn, giải pháp tăng p bước cho mỗi truy vấn sẽ trở thành phương trình bậc hai trong trường hợp xấu nhất. Ngay cả một giải pháp tính toán trước tất cả tổ tiên một cách rõ ràng cũng sẽ cần quá nhiều bộ nhớ. 

Cấu trúc chính là các mối quan hệ tổ tiên được cố định theo độ sâu trong cây, do đó các nút có chung tổ tiên ở khoảng cách p tạo thành một nhóm liền kề theo thứ tự DFS. Điều này gợi ý việc xử lý trước các cấu trúc cây con để chúng ta có thể trả lời “có bao nhiêu nút ở một độ sâu nhất định tồn tại trong một cây con”. 

Một trường hợp cạnh tinh vi xuất phát từ các nút mà tổ tiên thứ p không tồn tại. Nếu v quá nông thì đáp án phải bằng 0. Một trường hợp khác là khi tồn tại nhiều gốc, vì đầu vào là một khu rừng chứ không phải một cây đơn lẻ. Mọi giải pháp dựa trên DFS đều phải xử lý nhiều gốc một cách chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp tính toán tổ tiên thứ p của mỗi nút truy vấn bằng cách đi lên các con trỏ, sau đó quét toàn bộ tập hợp nút để đếm các kết quả trùng khớp. Điều này đúng nhưng cực kỳ chậm. Mỗi truy vấn có thể lấy O(n) trong trường hợp xấu nhất, dẫn đến các hoạt động O(nm), không khả thi ở tỷ lệ 10^10. 

Một viễn cảnh tốt hơn đến từ việc đảo ngược vấn đề. Thay vì hỏi “ai chia sẻ tổ tiên thứ p của tôi”, chúng ta có thể sửa một nút u và hỏi: có bao nhiêu nút có u là tổ tiên thứ p của chúng? Điều này phụ thuộc vào cấu trúc của cây con gốc tại u và sự phân bổ độ sâu bên trong nó. 

Nếu chúng tôi chạy DFS từ mỗi gốc, chúng tôi có thể ghi lại thời gian nhập và phạm vi cây con. Chúng tôi cũng tính toán độ sâu của mỗi nút. Sau đó, với bất kỳ nút u nào, tất cả các nút trong cây con của nó xuất hiện trong một đoạn liền kề theo thứ tự chuyến tham quan Euler. Trong số này, các nút ở độ sâu sâu[u] + p chính xác là những nút có tổ tiên thứ p là u. 

Vì vậy chúng ta cần truy vấn nhanh có dạng: trong cây con của u, có bao nhiêu nút có độ sâu chính xác là d. Điều này trở thành một vấn đề nhóm ngoại tuyến cổ điển. Chúng tôi lưu trữ các nút được nhóm theo độ sâu và duy trì cho mỗi độ sâu một danh sách được sắp xếp về thời gian nhập DFS. Sau đó, mỗi truy vấn giảm xuống còn hai tìm kiếm nhị phân trong danh sách đó được giới hạn trong khoảng cây con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tổ tiên vũ phu đi bộ | O(nm) | O(n) | Quá chậm | 
| DFS + nhóm độ sâu + tìm kiếm nhị phân | O((n + m) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng danh sách kề cho forest bằng mảng cha. Mỗi nút có cha 0 được coi là nút gốc. Điều này đảm bảo chúng ta có thể duyệt từng cây một cách độc lập. 
2. Chạy DFS từ mỗi nút gốc, chỉ định độ sâu cho mỗi nút và ghi lại thời gian vào và thời gian thoát của nó. Thứ tự DFS đảm bảo rằng cây con của bất kỳ nút nào tạo thành một khoảng liên tục trong hành trình Euler. 
3. Đối với mỗi giá trị độ sâu, hãy duy trì danh sách thời gian vào của các nút xuất hiện ở độ sâu đó. Điều này nhóm các nút theo cấp độ theo cách duy trì thứ tự DFS. 
4. Đối với truy vấn (v, p), hãy tính u = tổ tiên p các bước trên v bằng cách sử dụng bảng nâng nhị phân được tính toán trước. Nếu u không tồn tại, trả về 0 ngay lập tức vì không nút nào có thể chia sẻ tổ tiên không tồn tại. 
5. Khi u được tìm thấy, chúng ta muốn đếm các nút trong cây con(u) có độ sâu bằng độ sâu[u] + p. Chúng tôi lấy danh sách tương ứng với độ sâu mục tiêu đó và đếm số lần nhập nằm bên trong [tin[u], tout[u]] bằng cách sử dụng tìm kiếm nhị phân. 
6. Trả về số đếm này trừ 1 nếu chính v được bao gồm trong phạm vi, vì bài toán yêu cầu “những người khác”, không bao gồm v. 

Tại sao nó hoạt động dựa trên hai thuộc tính cấu trúc của DFS trên cây. Đầu tiên, tất cả các nút trong cây con tương ứng chính xác với một đoạn liền kề theo thứ tự chuyến tham quan Euler. Thứ hai, độ sâu không đổi dọc theo sự khác biệt giữa tổ tiên và con cháu, do đó các nút có tổ tiên thứ p là u phải nằm chính xác ở mức p bên dưới u và bên trong cây con của nó. Hai ràng buộc này đặc trưng duy nhất cho các nút hợp lệ, do đó việc đếm chúng thông qua giao điểm của nhóm độ sâu và khoảng cây con không thể đếm quá mức hoặc bỏ sót bất kỳ nút hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(200000)

n = int(input())
parent = list(map(int, input().split()))

g = [[] for _ in range(n)]
roots = []

for i, p in enumerate(parent):
    if p == 0:
        roots.append(i)
    else:
        g[p - 1].append(i)

LOG = 17
up = [[-1] * n for _ in range(LOG)]
depth = [0] * n
tin = [0] * n
tout = [0] * n
timer = 0

depth_nodes = {}

def dfs(v, p):
    global timer
    tin[v] = timer
    timer += 1

    up[0][v] = p
    for i in range(1, LOG):
        if up[i - 1][v] != -1:
            up[i][v] = up[i - 1][up[i - 1][v]]

    d = depth[v]
    if d not in depth_nodes:
        depth_nodes[d] = []
    depth_nodes[d].append(tin[v])

    for to in g[v]:
        depth[to] = d + 1
        dfs(to, v)

    tout[v] = timer - 1

for r in roots:
    dfs(r, -1)

def lift(v, k):
    for i in range(LOG):
        if v == -1:
            return -1
        if k & (1 << i):
            v = up[i][v]
    return v

m = int(input())
out = []

for _ in range(m):
    v, p = map(int, input().split())
    v -= 1
    u = lift(v, p)

    if u == -1:
        out.append("0")
        continue

    target_depth = depth[u] + p
    arr = d
```
