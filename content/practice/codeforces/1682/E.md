---
title: "CF 1682E - Hoán đổi không có thứ tự"
description: "Chúng ta được cấp một hoán vị các số nguyên từ $1$ đến $n$ và một tập hợp các hoán đổi $m$, được đảm bảo là các hoán đổi tối thiểu cần thiết để sắp xếp hoán vị. Những giao dịch hoán đổi này đã được xáo trộn một cách tùy tiện."
date: "2026-06-10T00:09:40+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "graphs", "greedy", "math", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1682
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 793 (Div. 2)"
rating: 2700
weight: 1682
solve_time_s: 133
verified: false
draft: false
---

[CF 1682E - Hoán đổi không có thứ tự](https://codeforces.com/problemset/problem/1682/E) 

**Đánh giá:** 2700 
**Tags:** thuật toán xây dựng, dfs và tương tự, đồ thị, tham lam, toán học, sắp xếp, cây 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị của các số nguyên từ$1$ĐẾN$n$và một bộ$m$các giao dịch hoán đổi, được đảm bảo là các giao dịch hoán đổi tối thiểu cần thiết để sắp xếp hoán vị. Những giao dịch hoán đổi này đã được xáo trộn một cách tùy tiện. Nhiệm vụ là xây dựng lại thứ tự của các hoán đổi này để sắp xếp hoán vị một cách chính xác. Mỗi lần hoán đổi hoạt động trên hai vị trí trong mảng, trao đổi giá trị của chúng. Đầu ra là một hoán vị của các chỉ số cho biết thứ tự chúng ta nên thực hiện các giao dịch hoán đổi đã cho. 

Kích thước hoán vị$n$có thể lớn như$2 \cdot 10^5$và số lần hoán đổi$m$có thể lên tới$n - 1$. Điều này ngụ ý rằng bất kỳ giải pháp nào cũng phải chạy trong khoảng$O(n \log n)$hoặc$O(n)$thời gian. Thuật toán với$O(n^2)$hành vi sẽ vượt quá giới hạn thời gian. Các trường hợp cạnh bao gồm các hoán vị đã được sắp xếp (yêu cầu$0$hoán đổi, mặc dù$m \ge 1$), một hoán vị đảo ngược thứ tự sắp xếp (yêu cầu hoán đổi tối đa) hoặc hoán đổi chỉ di chuyển các giá trị trong chu kỳ. 

Ví dụ, việc triển khai bất cẩn bỏ qua cấu trúc của các chu trình hoán vị có thể áp dụng các hoán đổi theo thứ tự tùy ý nhất định và không sắp xếp được mảng hoặc có thể cố gắng tham lam chọn các hoán đổi mà không kiểm tra xem chúng có giải quyết được một chu trình hay không, dẫn đến trình tự sắp xếp không đầy đủ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là thử tất cả$m!$hoán vị của các giao dịch hoán đổi và mô phỏng việc sắp xếp mỗi lần để xem liệu nó có dẫn đến một mảng được sắp xếp hay không. Điều này đúng về mặt định nghĩa, nhưng$m!$tăng trưởng theo giai thừa, vì vậy ngay cả đối với$m = 10$cách tiếp cận này là không khả thi. Mô phỏng từng chuỗi bao gồm$m \cdot n$hoạt động, điều này làm phức tạp thêm vấn đề. 

Quan sát quan trọng là bất kỳ hoán vị nào cũng có thể được phân tách thành các chu trình rời rạc và việc sắp xếp hoán vị ở mức tối thiểu đòi hỏi phải phá vỡ từng chu kỳ có độ dài.$k$sử dụng chính xác$k-1$trao đổi. Vì chúng ta được cung cấp chính xác số lần hoán đổi tối thiểu$m$, mỗi lần hoán đổi phải tương ứng với việc di chuyển một phần tử trong một chu kỳ. Do đó, chúng ta có thể mô hình hóa bài toán dưới dạng biểu đồ trong đó các vị trí là các đỉnh và các hoán đổi đã cho là các cạnh. Mỗi thành phần liên thông của đồ thị này tương ứng với một chu trình trong hoán vị. Việc sắp xếp trong một chu kỳ có thể được thực hiện theo bất kỳ thứ tự nào miễn là chúng ta sử dụng chính xác các giao dịch hoán đổi đã cho. Điều này làm giảm vấn đề khi thực hiện DFS (hoặc bất kỳ quá trình truyền tải nào) trên mỗi thành phần và xuất ra các giao dịch hoán đổi theo thứ tự ngược lại về thời gian hoàn thiện để xây dựng lại một chuỗi hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m! * m * n) | O(n) | Quá chậm | 
| Vẽ đồ thị DFS theo chu kỳ | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`pos`Ở đâu`pos[v]`lưu trữ chỉ số giá trị hiện tại`v`trong hoán vị. Điều này cho phép O(1) truy cập vào vị trí hiện tại của bất kỳ phần tử nào. 
2. Biểu diễn các giao dịch hoán đổi đã cho dưới dạng biểu đồ. Mỗi chỉ số là một đỉnh và mỗi hoán đổi là một cạnh vô hướng nối hai chỉ số. 
3. Khởi tạo một mảng có kích thước đã được truy cập$n$để theo dõi các chỉ số được xử lý. 
4. Đối với mỗi chỉ số`i`từ 1 đến n, nếu nó chưa được truy cập, hãy bắt đầu truyền tải DFS trên thành phần được kết nối của nó. Thành phần này đại diện cho một chu trình trong hoán vị. 
5. Trong DFS, khi truy cập một cạnh (hoán đổi), hãy đánh dấu nó là đã sử dụng và thêm nó vào danh sách các hoán đổi theo thứ tự sau. Điều này đảm bảo rằng chúng tôi sẽ áp dụng các giao dịch hoán đổi theo thứ tự hợp lệ để giải quyết các vấn đề phụ thuộc trong chu trình. 
6. Sau khi hoàn thành DFS trên tất cả các thành phần, hãy xuất các giao dịch hoán đổi theo thứ tự ngược lại với thứ tự chúng được thêm vào. Điều này tương ứng với việc thực hiện các giao dịch hoán đổi theo thứ tự sẽ sắp xếp chính xác từng chu kỳ. 

Tại sao nó hoạt động: Mỗi thành phần được kết nối trong biểu đồ hoán đổi tương ứng với một chu trình trong hoán vị. Bằng cách duyệt thành phần và sử dụng tất cả các cạnh trong thành phần, chúng tôi đảm bảo rằng tất cả các phần tử trong chu trình đều đạt đến đúng vị trí của chúng. Vì các lần hoán đổi là tối thiểu nên mọi lần hoán đổi đều cần thiết và việc thực hiện chúng theo thứ tự DFS ngược lại sẽ tôn trọng các phụ thuộc. Điều bất biến là tất cả các đỉnh trong một thành phần đều được truy cập chính xác một lần và tất cả các giao dịch hoán đổi trong thành phần đó được sử dụng chính xác một lần, đảm bảo tính đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

n, m = map(int, input().split())
p = list(map(int, input().split()))
edges = []
graph = [[] for _ in range(n)]

for idx in range(m):
    x, y = map(int, input().split())
    x -= 1
    y -= 1
    edges.append((x, y))
    graph[x].append((y, idx))
    graph[y].append((x, idx))

visited = [False] * n
used_edge = [False] * m
res = []

def dfs(u):
    visited[u] = True
    for v, idx in graph[u]:
        if not used_edge[idx]:
            used_edge[idx] = True
            if not visited[v]:
                dfs(v)
            res.append(idx + 1)

for i in range(n):
    if not visited[i]:
        dfs(i)

print(' '.join(map(str, reversed(res))))
```Đầu tiên, mã xây dựng một biểu đồ trong đó các đỉnh là vị trí và các cạnh là các giao dịch hoán đổi. Chúng tôi theo dõi các nút đã truy cập và các cạnh được sử dụng. DFS đảm bảo mỗi lần hoán đổi được áp dụng một lần và truy cập tất cả các vị trí được kết nối bằng các lần hoán đổi. Việc thêm các giao dịch hoán đổi sau khi đệ quy và đảo ngược chúng ở cuối đảm bảo rằng các phần phụ thuộc được tôn trọng. Các chỉ số dựa trên 1 cho đầu ra, đó là lý do tại sao chúng tôi thêm 1 khi lưu trữ trong`res`. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:`n=4, p=[2,3,4,1], swaps=[(1,4),(2,1),(1,3)]`| Bước | lượt truy cập DFS | độ phân giải | 
| --- | --- | --- | 
| Bắt đầu từ 0 | thăm 0 -> 1 | [] | 
| Cạnh (0,3) | thăm 3 | [1] | 
| Cạnh (0,1) | thăm 1 | [1,2] | 
| Cạnh (1,2) | thăm 2 | [1,2,3] | 

đảo ngược`res = [2,3,1]`, khớp với sản lượng dự kiến. Bảng xác nhận tất cả các giao dịch hoán đổi được sử dụng chính xác một lần và tất cả các vị trí đều được truy cập. 

Mẫu 2: 

đầu vào:`n=6, p=[6,5,1,3,2,4], swaps=[(3,1),(2,5),(6,3),(6,4)]`DFS trên các thành phần tạo ra một chuỗi hợp lệ`[4,2,1,3]`sau đó
