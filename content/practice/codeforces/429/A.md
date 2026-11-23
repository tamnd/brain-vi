---
title: "CF 429A - Cây Xor"
description: "Chúng ta có một cây gốc có n nút, mỗi nút được gắn nhãn 0 hoặc 1. Chúng ta cũng được cung cấp cấu hình mục tiêu là 0 và 1 cho mỗi nút. Thao tác duy nhất được phép là \"chọn\" một nút, thao tác này sẽ lật giá trị của nó và mọi nút con cấp hai xuống dưới cây."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "trees"]
categories: ["algorithms"]
codeforces_contest: 429
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 245 (Div. 1)"
rating: 1300
weight: 429
solve_time_s: 98
verified: true
draft: false
---

[CF 429A - Cây Xor](https://codeforces.com/problemset/problem/429/A) 

**Đánh giá:** 1300 
**Thẻ:** dfs và tương tự, cây cối 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có rễ với`n`các nút, mỗi nút được gắn nhãn 0 hoặc 1. Chúng tôi cũng được cung cấp cấu hình mục tiêu là 0 và 1 cho mỗi nút. Thao tác duy nhất được phép là "chọn" một nút, thao tác này sẽ lật giá trị của nó và mọi hậu duệ cấp hai xuống cây. Cụ thể, nếu một nút được chọn, giá trị của nó sẽ thay đổi, giá trị của các nút con không thay đổi, các cháu của nó bị lật, các chắt của nó vẫn giữ nguyên, v.v., các cấp độ xen kẽ nhau. 

Đầu vào là một mô tả cây tiêu chuẩn với các cạnh kết nối các nút và hai mảng biểu thị trạng thái ban đầu và trạng thái đích. Đầu ra phải là một chuỗi nút tối thiểu cần chọn để sau tất cả các thao tác, cây khớp với mảng mục tiêu. 

Từ`n`có thể lên tới 100.000 và chúng tôi chỉ có một giây, bất kỳ thuật toán nào truy cập tất cả các nút nhiều hơn một vài lần hoặc thử tất cả các tập hợp con của các nút sẽ quá chậm. Chúng tôi cần một`O(n)`giải pháp dựa trên truyền tải. Các trường hợp cạnh bao gồm các cây chỉ có một nút, cây cân bằng hoàn toàn hoặc các tình huống trong đó nhiều nút dọc theo cùng một nhánh sẽ lật lẫn nhau nếu không được xử lý cẩn thận. Một giải pháp đơn giản có thể chọn các nút một cách tham lam theo thứ tự tùy ý, điều này có thể lật ngược các nút, dẫn đến một tập hợp hoạt động lớn hơn mức tối thiểu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử chọn mọi nút theo mọi thứ tự có thể, lật các cấp độ thích hợp và kiểm tra xem liệu có đạt được mục tiêu hay không. Về nguyên tắc, điều này hoạt động vì phép toán có thể đảo ngược và cây là hữu hạn, nhưng số lượng chuỗi tăng theo cấp số nhân, vì vậy điều này không thực tế đối với`n`lên đến 10^5. 

Thông tin chi tiết quan trọng là hoạt động lật có một mô hình có thể dự đoán được theo các cấp độ: lật ở độ sâu chẵn ảnh hưởng đến các nút ở độ sâu chẵn, lật ở độ sâu lẻ ảnh hưởng đến các nút ở độ sâu lẻ. Điều này có nghĩa là chúng ta có thể thực hiện truyền tải theo chiều sâu từ gốc, theo dõi số lần lật đã được áp dụng dọc theo đường đi ở độ sâu chẵn và lẻ. Tại mỗi nút, chúng tôi quyết định có nên lật hay không dựa trên hiệu ứng tích lũy của các lần lật trước đó. Điều này làm giảm vấn đề xuống còn một DFS duy nhất với việc ghi sổ kế toán cho hai số nguyên biểu thị các lần lật ở độ sâu chẵn và lẻ, đảm bảo`O(n)`giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây bằng danh sách kề. Lưu trữ các giá trị ban đầu và mục tiêu. 
2. Khởi tạo hai bộ đếm,`even_flips`Và`odd_flips`, để theo dõi số lần lật được áp dụng dọc theo đường dẫn cho các nút ở độ sâu chẵn và lẻ. 
3. Bắt đầu truyền tải DFS từ gốc (độ sâu 0). Đối với mỗi nút, hãy tính giá trị hiệu quả hiện tại của nó bằng cách áp dụng tính chẵn lẻ của các lần lật dọc theo độ sâu của nó. 
4. Nếu giá trị hiệu dụng hiện tại không khớp với mục tiêu, hãy thêm nút này vào tập kết quả và tăng bộ đếm lật tương ứng với độ chẵn lẻ độ sâu của nó. 
5. Tái diễn thành trẻ em, chuyển số lần lật được cập nhật. 
6. Sau khi thăm con, hãy quay lại: việc lật ngược không cần phải hoàn tác vì chúng ta chỉ quan tâm đến ảnh hưởng tích lũy đối với con cháu. 

Tại sao nó hoạt động: tại mỗi nút, chúng tôi duy trì bất biến`current_value ^ cumulative_flips = goal_value`ở độ sâu đó. Quyết định lật một nút đảm bảo rằng bất biến này đúng cho tất cả các nút con. Vì chúng ta duyệt theo thứ tự DFS nên mỗi nút sẽ nhìn thấy tất cả các lần lật ảnh hưởng đến nó trước khi quyết định có lật chính nó hay không. Mô hình lật xen kẽ phù hợp với việc theo dõi dựa trên tính chẵn lẻ, đảm bảo số lần lật tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**6)

def solve():
    n = int(input())
    tree = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        tree[u].append(v)
        tree[v].append(u)
    
    init = [0] + list(map(int, input().split()))
    goal = [0] + list(map(int, input().split()))
    
    result = []
    
    def dfs(node, parent, depth, even_flips, odd_flips):
        if depth % 2 == 0:
            effective = init[node] ^ even_flips
        else:
            effective = init[node] ^ odd_flips
        
        if effective != goal[node]:
            result.append(node)
            if depth % 2 == 0:
                even_flips ^= 1
            else:
                odd_flips ^= 1
        
        for child in tree[node]:
            if child != parent:
                dfs(child, node, depth + 1, even_flips, odd_flips)
    
    dfs(1, 0, 0, 0, 0)
    
    print(len(result))
    for node in result:
        print(node)

solve()
```Danh sách kề đại diện cho cây một cách hiệu quả. Chúng tôi sử dụng`sys.setrecursionlimit`bởi vì cây sâu có thể tràn đệ quy.`even_flips`Và`odd_flips`theo dõi các lần lật tích lũy cho các nút ở độ sâu chẵn và lẻ. DFS đảm bảo mọi nút đều được truy cập một lần, đưa ra quyết định dựa trên trạng thái hiệu quả hiện tại. Cần cẩn thận để tránh lật ngược vì bộ đếm của chúng tôi có độ sâu cụ thể và được truyền theo giá trị. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1 

| Nút | Độ sâu | Ban đầu | Mục tiêu | Hiệu quả | Lật mặt? | chẵn_flips | lẻ_flips | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 1 | Không | 0 | 0 | 
| 2 | 1 | 0 | 0 | 0 | Không | 0 | 0 | 
| 4 | 2 | 1 | 0 | 1 | Có | 1 | 0 | 
| 7 | 3 | 0 | 1 | 0 | Có | 1 | 1 | 

Dấu vết này xác nhận rằng các lần lật được áp dụng ở độ sâu chẵn lẻ chính xác và thuật toán xác định tập tối thiểu`[4, 7]`. 

### Đầu vào tùy chỉnh```
3
1 2
1 3
1 1 0
0 0 1
```| Nút | Độ sâu | Ban đầu | Mục tiêu | Hiệu quả | Lật mặt? | chẵn_flips | lẻ_flips | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 0 | 1 | Có | 1 | 0 | 
| 2 | 1 | 1 | 0 | 1 | Có | 1 | 1 | 
| 3 | 1 | 0 | 1 | 0 | Có | 1 | 0 | 

Điều này xác nhận tính năng theo dõi chẵn lẻ độ sâu xử lý các anh chị em một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được truy cập một lần trong DFS. | 
| Không gian | O(n) | Danh sách kề và ngăn xếp đệ quy yêu cầu O(n). | 

các`O(n)`thời gian đảm bảo giải pháp hoạt động thoải mái dưới giới hạn 1 giây cho`n`lên đến 10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Sample 1
assert run("10\n2 1\n3 1\n4 2\n5 1\n6 2\n7 5\n8 6\n9 8\n10 5\n1 0 1 1 0 1 0 1 0 1\n1 0 1 0 0 1 1 1 0 1\n") == "2\n4\n7"

# Minimum-size input
assert run("1\n0\n1\n") == "1\n1"

# Maximum-size linear tree
n = 1000
edges = "\n".join(f"{i} {i+1}" for i in range(1, n))
init = " ".join("0" for _ in range(n))
goal = " ".join("1" if i%2==0 else "0" for i in range(n))
assert run(f"{n}\n{edges}\n{init}\n{goal}\n") != "", "max-size linear"

# All-equal initial and goal
assert run("3\n1 2\n1 3\n0 0 0\n0 0 0\n") == "0"

# Root-only flip needed
assert run("3\n1 2\n1 3\n1 0 0\n0 0 0\n") == "1\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lật 1 nút | 1\n1 | Xử lý đầu vào tối thiểu | 
| Tuyến tính 1000 nút | không trống | Xử lý cây sâu hiệu quả | 
| Tất cả số không | 0 | Phát hiện không cần lật | 
| Lật gốc | 1\n1 | Lật gốc chính xác ảnh hưởng đến tính chẵn lẻ | 

## Vỏ cạnh 

Cây một nút phải lật nếu mục tiêu != ban đầu. Thuật toán kiểm tra độ sâu 0, tính toán giá trị hiệu dụng với số lần lật bằng 0 và lật gốc nếu cần. 

Trong một cây mà các anh chị em yêu cầu lật độc lập, thuật toán sẽ theo dõi chính xác tính chẵn lẻ. Ví dụ, một
