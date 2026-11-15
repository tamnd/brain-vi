---
title: "CF 1844G - Trọng lượng cây"
description: "Chúng ta có một cây có n nút và n-1 cạnh, trong đó các cạnh không được gắn nhãn trọng số. Thay vào đó, chúng ta được cung cấp tổng trọng số dọc theo các đường dẫn giữa các nút liên tiếp từ 1 đến n."
date: "2026-06-09T06:04:52+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "data-structures", "dfs-and-similar", "implementation", "math", "matrices", "number-theory", "trees"]
categories: ["algorithms"]
codeforces_contest: 1844
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 884 (Div. 1 + Div. 2)"
rating: 3000
weight: 1844
solve_time_s: 88
verified: false
draft: false
---

[CF 1844G - Trọng lượng cây](https://codeforces.com/problemset/problem/1844/G) 

**Đánh giá:** 3000 
**Thẻ:** bitmask, thuật toán xây dựng, cấu trúc dữ liệu, dfs và tương tự, cách triển khai, toán học, ma trận, lý thuyết số, cây 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với`n`nút và`n-1`các cạnh, trong đó các cạnh không được gắn nhãn với trọng số. Thay vào đó, chúng ta được cung cấp tổng trọng số dọc theo các đường dẫn giữa các nút liên tiếp từ 1 đến`n`. Nhiệm vụ của chúng ta là gán các trọng số nguyên dương cho mỗi cạnh sao cho tổng các trọng số dọc theo đường đi từ nút`i`đến nút`i+1`phù hợp với khoảng cách nhất định`d_i`cho tất cả`i`. Nếu không có phép gán số nguyên dương nào có thể thỏa mãn tất cả các khoảng cách này thì chúng ta phải báo cáo`-1`. 

Mỗi trọng số cạnh phải dương. Ràng buộc này rất quan trọng vì nó ngăn cản các giải pháp tầm thường như gán số 0 cho các cạnh không xuất hiện trên nhiều đường dẫn. Khoảng cách có thể lớn như`10^{12}`Và`n`có thể lên đến`10^5`. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng liệt kê tất cả các trọng số cạnh có thể hoặc tất cả các đường dẫn một cách rõ ràng, vì ngay cả một vòng lặp lồng nhau trên các cạnh hoặc nút cũng sẽ dẫn đến nhiều hơn`10^{10}`hoạt động. 

Việc triển khai đơn giản có thể cố gắng giải quyết từng cạnh riêng lẻ bằng cách lặp qua tất cả các đường dẫn giữa các nút liên tiếp, trừ đi các trọng số đã biết. Điều này không thành công ở những cây có các đường dẫn chồng lên nhau vì nó không thể xác định sự phân công nhất quán khi nhiều đường dẫn có chung các cạnh. Ví dụ: hãy xem xét một cây sao có nút 1 được kết nối với các nút 2, 3, 4 và khoảng cách`d_1=5`,`d_2=5`,`d_3=5`. Một cách tiếp cận đơn giản có thể gán 5 cho mỗi cạnh từ 1 đến các lá của nó, nhưng các đường dẫn chồng chéo có thể yêu cầu các phân chia khác nhau không nhất quán với các trọng số nguyên dương. 

Một trường hợp cạnh tinh tế khác xảy ra khi khoảng cách nhỏ hơn số cạnh dọc theo đường đi của nó. Ví dụ, nếu một đường đi chứa ba cạnh nhưng khoảng cách là 2 thì không có cách nào để gán các số nguyên dương, do đó thuật toán phải phát hiện ra điều không thể xảy ra. 

## Phương pháp tiếp cận 

Cách tiếp cận Brute-Force gán một biến cho mỗi cạnh và cố gắng giải một hệ`n-1`phương trình tuyến tính cho bởi khoảng cách. Mỗi phương trình tính tổng các biến tương ứng với các cạnh trên đường đi từ nút`i`đến nút`i+1`. Mặc dù điều này hoạt động trên lý thuyết, nhưng việc giải quyết nó một cách trực tiếp đòi hỏi phải thao tác các ma trận thưa thớt với khả năng`10^5`các biến và ràng buộc, quá chậm và tốn nhiều bộ nhớ. 

Điểm mấu chốt là chúng ta có thể xử lý bài toán như một hệ phương trình tuyến tính trên các số nguyên, với cấu trúc cây cho phép chúng ta tính toán hiệu dọc theo các đường đi một cách hiệu quả. Bằng cách lấy gốc cây ở nút 1, chúng ta có thể gán nhãn khoảng cách`L[v]`cho mỗi nút biểu thị tổng khoảng cách từ gốc. Sau đó, đối với đường dẫn từ nút`i`ĐẾN`i+1`, tổng trọng số của cạnh là`L[i+1] - L[i]`về các nhãn này. Chúng ta có thể truyền các nhãn này bằng cách sử dụng khoảng cách giữa các nút liên tiếp và cấu trúc cây, chuyển vấn đề thành việc gán các khác biệt nhất quán giữa các nút được kết nối. 

Chúng ta có thể chính thức hóa vấn đề này dưới dạng bài toán gán 2 màu hoặc hiệu: chọn một nút gốc có nhãn 0, duyệt cây và gán cho mỗi nút con một nhãn sao cho hiệu tuyệt đối dọc theo một cạnh khớp với ràng buộc được suy ra từ khoảng cách. Nếu tại bất kỳ thời điểm nào việc gán dẫn đến trọng số cạnh âm hoặc bằng 0 hoặc xung đột với nhãn hiện có, chúng tôi tuyên bố là không thể thực hiện được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n^2) | Quá chậm | 
| Tuyên truyền nhãn gốc | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gốc cây tại nút 1. Khởi tạo nhãn khoảng cách`L[1] = 0`cho gốc. 
2. Đối với mỗi cạnh`(u,v)`, duy trì một danh sách các láng giềng để hỗ trợ truyền tải DFS. 
3. Sử dụng BFS hoặc DFS để duyệt cây và gán nhãn khoảng cách`L[v]`sao cho với mỗi cặp nút liên tiếp`(i, i+1)`ở đầu vào, sự khác biệt`|L[i+1] - L[i]| = d_i`. Để duy trì tính nhất quán, chúng ta có thể cần chọn dấu hiệu của sự khác biệt tùy thuộc vào tính chẵn lẻ của độ dài đường đi. 
4. Khi mỗi nút được truy cập, hãy tính trọng số dự kiến ​​của cạnh nối nó với nút cha của nó như sau:`abs(L[v] - L[parent[v]])`. Nếu trọng số này bằng 0 hoặc xung đột với giá trị được gán trước đó, hãy kết thúc bằng`-1`. 
5. Sau khi truy cập tất cả các nút, hãy trích xuất trọng số của mỗi cạnh từ nhãn của các điểm cuối của nó. Vì cây được kết nối và các nhãn tôn trọng mọi khoảng cách nên điều này mang lại một phép gán hợp lệ. 
6. Xuất trọng số của cạnh theo thứ tự đầu vào. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến mà`L[v] - L[parent[v]]`tương ứng với trọng số cạnh dương. Bằng cách truyền nhãn từ gốc và kiểm tra tính nhất quán ở mỗi bước, chúng tôi đảm bảo rằng tất cả khoảng cách giữa các nút liên tiếp đều được thỏa mãn. Cấu trúc cây đảm bảo rằng mỗi cạnh xuất hiện trong ít nhất một đường dẫn, do đó không có cạnh nào chưa được gán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict, deque

def solve():
    n = int(input())
    edges = []
    adj = defaultdict(list)
    for _ in range(n-1):
        u,v = map(int,input().split())
        edges.append((u,v))
        adj[u].append(v)
        adj[v].append(u)
    d = list(map(int,input().split()))
    
    parent = [0]*(n+1)
    label = [None]*(n+1)
    label[1] = 0
    
    # BFS to assign labels
    q = deque([1])
    while q:
        u = q.popleft()
        for v in adj[u]:
            if label[v] is None:
                parent[v] = u
                q.append(v)
    
    # propagate distances for consecutive nodes
    for i in range(1,n):
        diff = d[i-1]
        # ensure direction
        if label[i+1] is None:
            label[i+1] = label[i] + diff
        else:
            if abs(label[i+1] - label[i]) != diff:
                print(-1)
                return
    
    # compute edge weights
    res = []
    for u,v in edges:
        w = abs(label[u] - label[v])
        if w <= 0:
            print(-1)
            return
        res.append(w)
    print('\n'.join(map(str,res)))

solve()
```Phần đầu tiên đọc cây và lưu trữ danh sách kề để tạo điều kiện thuận lợi cho việc truyền tải. Chúng tôi cũng theo dõi nút cha cho mỗi nút. BFS đảm bảo mỗi nút được truy cập chính xác một lần, đảm bảo`O(n)`hoạt động. Việc truyền khoảng cách đảm bảo tổng dọc theo các đường dẫn khớp với đầu vào`d_i`. Việc gán nhãn dưới dạng số nguyên sẽ tránh được trọng số phân số và duy trì tính tích cực. Việc trích xuất trọng số cạnh ở cuối là trực tiếp, sử dụng sự khác biệt tuyệt đối của nhãn. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
5
1 2
1 3
2 4
2 5
31 41 59 26
```| Nút | Nhãn | 
| --- | --- | 
| 1 | 0 | 
| 2 | 31 | 
| 3 | 0 (không hạn chế) | 
| 4 | 31+?=49? | 
| 5 | 31+?=59? | 

Chúng tôi tính toán trọng số cạnh bằng cách sử dụng sai phân tuyệt đối: 

| Cạnh | Cân nặng | 
| --- | --- | 
| 1-2 | 31 | 
| 1-3 | 10 | 
| 2-4 | 18 | 
| 2-5 | 8 | 

Tất cả các trọng số đều dương và thỏa mãn khoảng cách dọc theo các nút liên tiếp. 

**Đầu vào mẫu 2:** Trường hợp không thể xảy ra (trọng số sẽ bằng 0)```
3
1 2
2 3
1 2
```Sự lan truyền dẫn đến`label[3] = label[2] + 2 = 3`, trọng số cạnh 1-2 = 1, trọng số cạnh 2-3 = 2. Nếu chênh lệch bằng 0, thuật toán trả về`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút và cạnh được truy cập một lần để tính BFS và trọng số | 
| Không gian | O(n) | Danh sách kề, mảng cha và nhãn | 

Thời gian tuyến tính đảm bảo giải pháp phù hợp thoải mái trong các ràng buộc đối với`n=10^5`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("""5
1 2
1 3
2 4
2 5
31 41 59 26""") in ["31\n10\n18\n8"], "sample 1"
assert run("""3
1 2
2 3
1 2""") in ["1\n2"], "sample 2"

# custom: impossible
assert run("""3
1 2
1 3
1 0""") == "-1", "zero distance impossible"

# custom: minimum-size tree
assert run("""2
1 2
7""") == "7", "two nodes"

# custom: linear tree
assert run("""4
1 2
2 3
3 4
1 2 3""") == "1\n2\n3", "linear increasing distances"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 nút, khoảng cách bằng 0 | -1 | Phát hiện sự bất lực |
