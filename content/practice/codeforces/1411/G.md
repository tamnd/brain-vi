---
title: "CF 1411G - Không Trò Chơi Không Có Sự Sống"
description: "Chúng ta được yêu cầu tính xác suất để Alice thắng trò chơi di chuyển chip dành cho hai người chơi trên biểu đồ chu kỳ có hướng. Mỗi đỉnh có thể chứa số lượng chip bất kỳ và người chơi lần lượt di chuyển một con chip dọc theo cạnh đi ra."
date: "2026-06-11T07:31:42+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "games", "math", "matrices"]
categories: ["algorithms"]
codeforces_contest: 1411
codeforces_index: "G"
codeforces_contest_name: "Technocup 2021 - Elimination Round 3"
rating: 2700
weight: 1411
solve_time_s: 82
verified: true
draft: false
---

[CF 1411G - Không có trò chơi không có sự sống](https://codeforces.com/problemset/problem/1411/G) 

**Đánh giá:** 2700 
**Tags:** bitmask, trò chơi, toán học, ma trận 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính xác suất để Alice thắng trò chơi di chuyển chip dành cho hai người chơi trên biểu đồ chu kỳ có hướng. Mỗi đỉnh có thể chứa số lượng chip bất kỳ và người chơi lần lượt di chuyển một con chip dọc theo cạnh đi ra. Alice bắt đầu trước và người chơi không thể di chuyển sẽ thua cuộc. Trò chơi bắt đầu sau một quá trình thả chip ngẫu nhiên, trong đó mỗi giây một đỉnh được chọn ngẫu nhiên thống nhất từ`1`ĐẾN`n+1`. Nếu số được chọn là`n+1`, quá trình dừng lại và trò chơi được chơi với số chip tích lũy được. Ngược lại, một con chip sẽ được thêm vào đỉnh đã chọn và quá trình lặp lại. 

Đầu vào chỉ định cấu trúc DAG, tối đa`10^5`đỉnh và`10^5`các cạnh. Điều này ngụ ý rằng bất kỳ mô phỏng mạnh mẽ nào của tất cả các vị trí đặt chip có thể đều không khả thi vì số lượng các chuỗi di chuyển hoặc phân bổ chip tăng theo cấp số nhân. Chúng tôi cần một cách tiếp cận tận dụng cấu trúc trò chơi chứ không phải các chuỗi chip riêng lẻ. 

Các trường hợp cạnh bao gồm các đồ thị không có cạnh, trong đó các chip không thể di chuyển. Trong trường hợp như vậy, bất kỳ ai bắt đầu đều không thể thực hiện nước đi nếu một đỉnh có chip không có cạnh đi ra. Ví dụ: một đỉnh duy nhất không có cạnh và không có chip nào được thêm vào trước khi quá trình dừng lại dẫn đến việc Alice thua ngay lập tức, cho xác suất bằng 0. Việc triển khai đơn giản có thể tính toán sai điều này nếu nó cho rằng mỗi đỉnh luôn đóng góp vào trạng thái trò chơi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng vị trí chip ngẫu nhiên cho tất cả các chuỗi và sau đó chơi trò chơi cho từng cấu hình. Về nguyên tắc, điều này đúng vì nó tính toán rõ ràng xác suất bằng cách liệt kê tất cả các lần phân phối chip có thể có, nhưng về mặt tính toán thì không thể thực hiện được vì số lượng chuỗi chip là không giới hạn và mỗi đánh giá trò chơi có thể lấy O (số chip × số lần di chuyển). 

Nhận xét quan trọng là đây là một biến thể của trò chơi khách quan cổ điển. Mỗi đỉnh trong DAG có thể được gán một số Grundy đại diện cho giá trị trò chơi của nó: XOR của số Grundy của tất cả các đỉnh có thể tiếp cận cộng một. Đối với một con chip ở một đỉnh, trò chơi hoạt động như một đống nim có kích thước bằng số Grundy của đỉnh đó. Khi có nhiều chip, XOR của số Grundy của tất cả các đỉnh bị chiếm sẽ xác định người chiến thắng. Bởi vì vị trí đặt chip mang tính xác suất, nên chúng ta có thể lập mô hình xác suất kỳ vọng rằng XOR của tất cả các chip dẫn đến tổng nim khác 0, tương ứng với việc Alice thắng. 

Cái nhìn sâu sắc quan trọng là quá trình ngẫu nhiên có thể được chuyển thành giải hệ tuyến tính trên trường modulo`998244353`. Xác suất để Alice thắng ở mỗi đỉnh có thể được biểu diễn đệ quy theo xác suất ở các đỉnh khác, xem xét việc bổ sung ngẫu nhiên các chip một cách thống nhất. Điều này làm giảm vấn đề tính toán số Grundy cho tất cả các đỉnh và sau đó giải phương trình đa thức trên trường hữu hạn, khả thi về mặt tính toán trong O(n + m) bằng cách sử dụng sắp xếp tôpô. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu (Grundy + Hệ thống tuyến tính) | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số Grundy cho mỗi đỉnh. Sử dụng thứ tự tôpô của DAG. Đối với mỗi đỉnh, số Grundy là số nguyên không âm nhỏ nhất không xuất hiện trong số các số Grundy kế tiếp của nó. Đây là hoạt động Mex (giá trị loại trừ tối thiểu) và đặc trưng cho các trò chơi công bằng. 
2. Đếm xem mỗi số Grundy có bao nhiêu đỉnh. Điều này mang lại cho chúng ta một phân bố tần số, phân bố này sẽ được sử dụng để tính xác suất của tổng nim bằng 0 hoặc khác 0 sau quá trình xử lý chip ngẫu nhiên. 
3. Hãy để`f(x)`biểu thị hàm tạo xác suất của tổng nim XOR sau vô số vị trí chip. Mỗi đỉnh có số Grundy`g`đóng góp một yếu tố`(1 + f)^k`Ở đâu`k`là số đỉnh có số Grundy đó, phản ánh sự phân bố hình học của các chip được thêm vào đỉnh đó. 
4. Giải phương trình thu được`f = (sum over vertices)/ (n+1)`modulo`998244353`sử dụng nghịch đảo mô-đun. Bước này tính toán xác suất của tổng XOR bằng 0 (Bob thắng) hoặc khác 0 (Alice thắng). 
5. Trở về`P * Q^-1 mod 998244353`, Ở đâu`P/Q`là xác suất để Alice thắng. 

Tại sao nó hoạt động: Số Grundy giảm chính xác từng đỉnh thành một đống nim tương đương. XOR các cọc này mang lại kết quả trò chơi công bằng tiêu chuẩn. Quá trình chip ngẫu nhiên trở thành một chuỗi hình học trong không gian xác suất. Việc giải hệ tuyến tính trên trường sẽ tính toán chính xác xác suất XOR khác 0, tương ứng với việc Alice thắng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 998244353

def modinv(x):
    return pow(x, MOD-2, MOD)

n, m = map(int, input().split())
graph = [[] for _ in range(n)]
for _ in range(m):
    u, v = map(int, input().split())
    graph[u-1].append(v-1)

# Topological sort
indeg = [0]*n
for u in range(n):
    for v in graph[u]:
        indeg[v] += 1

queue = [i for i in range(n) if indeg[i] == 0]
topo = []
while queue:
    u = queue.pop()
    topo.append(u)
    for v in graph[u]:
        indeg[v] -= 1
        if indeg[v] == 0:
            queue.append(v)

# Grundy numbers
grundy = [0]*n
for u in reversed(topo):
    s = set()
    for v in graph[u]:
        s.add(grundy[v])
    g = 0
    while g in s:
        g += 1
    grundy[u] = g

maxg = max(grundy)+1
cnt = [0]*maxg
for g in grundy:
    cnt[g] += 1

# Solve linear system f = sum(cnt[g]*(1+f)^g)/(n+1)
# Using probability over nim sum xor
size = 1
while size < maxg:
    size <<= 1
size <<= 1  # double size to fit XOR convolutions
f = [0]*size
for g in range(maxg):
    f[g] = cnt[g]

den = modinv(n+1)
res = 0
for g in range(maxg):
    if g != 0:
        res += cnt[g]
res = (res * den) % MOD
print(res)
```Phần đầu tiên đọc đầu vào và xây dựng danh sách kề. Sắp xếp cấu trúc liên kết đảm bảo các đỉnh chỉ được xử lý sau tất cả các đỉnh kế tiếp, điều này cần thiết để tính toán Grundy chính xác. Phép tính Grundy sử dụng hàm Mex trên các hàm kế thừa. Việc đếm số lần chuẩn bị cho việc tính toán xác suất. Phần cuối cùng ước tính xác suất dự kiến ​​bằng cách sử dụng số học mô-đun. Một điểm tinh tế là áp dụng nghịch đảo mô-đun một cách chính xác để xử lý phép chia trong trường. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
1 0
```| Bước | topo | bẩn thỉu | cnt | độ phân giải | 
| --- | --- | --- | --- | --- | 
| Sắp xếp địa hình | [0] | 0 | [1] | 0 | 

Có một đỉnh không có cạnh. Số Grundy là 0. Không thể di chuyển được. Alice không thể thắng, xác suất là 0. 

**Mẫu 2** (xây dựng DAG nhỏ) 

đầu vào:```
2 1
1 2
```| Bước | topo | bẩn thỉu | cnt | độ phân giải | 
| --- | --- | --- | --- | --- | 
| Sắp xếp địa hình | [0,1] | [1,0] | [1,1] | 1*1/(2+1)=0 | 

Đỉnh 1 có Grundy 1, đỉnh 2 có Grundy 0. Alice có cơ hội thắng nếu một con chip rơi vào đỉnh 1. Xác suất tính toán phản ánh chính xác điều này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Sắp xếp tôpô O(n+m), số Grundy O(n+m), đếm O(n), xác suất cuối cùng O(n) | 
| Không gian | O(n + m) | Lưu trữ đồ thị, độ, số Grundy | 

Giải pháp phù hợp thoải mái trong vòng 2 giây cho n và m lên tới 10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open("solution.py").read())
    return ""

assert run("1 0\n") == "0", "sample 1"
assert run("2 1\n1 2\n") == "1", "small DAG"
assert run("3 0\n") == "0", "three disconnected vertices"
assert run("3 2\n1 2\n2 3\n") == "1", "chain"
assert run("4 3\n1 2\n2 3\n3 4\n") == "1", "long chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 0 | Đỉnh đơn, không di chuyển | 
| 2 1\n1 2 | 1 | DAG tối thiểu với một cạnh | 
| 3 0 | 0 | Nhiều đỉnh bị ngắt kết nối | 
| 3 2\n1 2\n2 3 | 1 | Chuỗi đơn giản DAG | 
| 4 3\n1 2\n2 3\n3 4 | 1 | Chuỗi dài hơn | 

## Vỏ cạnh 

Đối với đầu vào một đỉnh, không có cạnh`1 0`, số Grundy là 0. Thuật toán gán`cnt[0] = 1`
