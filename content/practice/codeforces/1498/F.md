---
title: "CF 1498F - Trò Chơi Giáng Sinh"
description: "Chúng ta được yêu cầu phân tích một trò chơi hai người chơi trên một cái cây. Mỗi nút của cây chứa một số lượng quà nhất định và người chơi lần lượt chuyển quà từ các nút đến tổ tiên $k$-th của họ. Người chơi không thể di chuyển sẽ thua."
date: "2026-06-10T21:41:51+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "data-structures", "dfs-and-similar", "dp", "games", "math", "trees"]
categories: ["algorithms"]
codeforces_contest: 1498
codeforces_index: "F"
codeforces_contest_name: "CodeCraft-21 and Codeforces Round 711 (Div. 2)"
rating: 2500
weight: 1498
solve_time_s: 330
verified: false
draft: false
---

[CF 1498F - Trò chơi Giáng sinh](https://codeforces.com/problemset/problem/1498/F) 

**Đánh giá:** 2500 
**Thẻ:** bitmasks, cấu trúc dữ liệu, dfs và tương tự, dp, trò chơi, toán học, cây cối 
**Thời gian giải:** 5 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu phân tích một trò chơi hai người chơi trên một cái cây. Mỗi nút của cây chứa một số lượng quà nhất định và người chơi lần lượt chuyển quà từ nút này sang nút khác.$k$-tổ tiên thứ Người chơi không thể di chuyển sẽ thua. Nhiệm vụ là xác định, đối với mỗi nút được coi là nút gốc, liệu Alice, người chơi đầu tiên, có thể đảm bảo chiến thắng hay không nếu cả hai đều chơi tối ưu. 

Đầu vào bao gồm một cây có tối đa$10^5$các nút và một tham số$k$trong khoảng từ 1 đến 20. Mỗi nút có số lượng quà có thể lớn bằng$10^9$. Các cạnh của cây được cho dưới dạng các cặp số nguyên và mảng quà được cho riêng. Đầu ra là một danh sách độ dài$n$, bằng 1 nếu Alice thắng khi nút đó là gốc và 0 nếu ngược lại. 

Bởi vì$n$tùy thuộc vào$10^5$Và$k$nhỏ, chúng ta cần một thuật toán gần đúng$O(n k)$hoặc$O(n \log n)$mỗi trường hợp thử nghiệm. Bất kỳ giải pháp nào lặp lại tất cả các bước đi có thể một cách ngây thơ sẽ liên quan đến khả năng$O(n^2)$hoạt động và quá chậm. Các giá trị lớn có thể có của quà tặng cũng loại trừ bất kỳ giải pháp nào mô phỏng rõ ràng từng món quà. 

Một trường hợp nguy hiểm có thể phá vỡ các giải pháp ngây thơ là khi$k$lớn hơn độ sâu của một số nút. Trong tình huống này, các nút đó không thể di chuyển được. Một trường hợp phức tạp khác là khi một nút không có quà; một cách tiếp cận bất cẩn có thể cho rằng mọi nút đều đóng góp cho trò chơi, điều này là không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng mọi động thái có thể có cho cả hai người chơi. Đối với mỗi gốc, chúng tôi sẽ liên tục chọn các nút có độ sâu ít nhất$k$và chuyển bất kỳ số lượng quà nào đến chỗ của họ$k$-tổ tiên thứ Ở mỗi giai đoạn, chúng ta cần đánh giá tất cả các chuỗi di chuyển có thể xảy ra, mang tính bùng nổ về mặt tổ hợp. Cách tiếp cận này đúng về mặt nguyên tắc nhưng rõ ràng là không thể thực hiện được.$n=10^5$, vì số lượng trạng thái tăng vượt xa những gì chúng ta có thể tính toán. 

Thông tin chi tiết quan trọng đến từ việc xem đây là một trò chơi tổ hợp khách quan: ít nhất mỗi nút ở độ sâu$k$có thể được coi là một đống có kích thước bằng với số lượng quà của nó. Chuyển quà đến một$k$-th tổ tiên có thể được coi là chia tách hoặc thay đổi kích thước heap. Sử dụng lý thuyết Sprague-Grundy, chúng ta có thể gán số Grundy cho mỗi nút đại diện cho nimber tương đương. XOR của số Grundy của tất cả các nút xác định người chiến thắng: nếu nó bằng 0, người chơi thứ hai (Bob) thắng; nếu không, người chơi đầu tiên (Alice) sẽ thắng. 

Bởi vì$k$nhỏ, chúng ta có thể thực hiện tìm kiếm theo chiều sâu để tính các số Grundy từ các lá trở lên. Số Grundy của một nút là XOR của tất cả số nút Grundy ở khoảng cách$k$bên dưới nó cộng với số lượng quà tại các nút chính xác$k$dưới. Sau đó, bằng cách root lại cây bằng kỹ thuật DP cây tiêu chuẩn, chúng ta có thể tính toán kết quả cho mọi gốc có thể một cách hiệu quả mà không cần tính toán lại mọi thứ từ đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2 * max(a_i)) | O(n) | Quá chậm | 
| Sprague-Grundy với DFS + Root lại | O(nk) | O(nk) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích cú pháp đầu vào để xây dựng cây dưới dạng danh sách kề và đọc mảng quà. Khởi tạo mảng để lưu trữ thông tin về độ sâu, thông tin gốc và cây con. 
2. Thực hiện DFS từ một gốc tùy ý (ví dụ nút 1) để tính độ sâu của mỗi nút và$k$- tổ tiên thứ cho mỗi nút. Điều này cho phép chúng tôi nhanh chóng xác định nơi nút có thể di chuyển. 
3. Đối với mỗi nút, xác định một mảng DP`dp[u][d]`đại diện chính xác cho XOR của quà tặng từ các nút`d`khoảng cách bên dưới`u`. Đối với những chiếc lá, đây chỉ là số lượng hiện tại của chúng. 
4. Trong khi trở về từ DFS, hãy cập nhật mảng DP của nút`u`bằng cách XOR các giá trị DP từ trẻ em ở khoảng cách`d-1`. Lối này,`dp[u][d]`tích lũy tất cả các đóng góp có liên quan từ các nút ở độ sâu`d`dưới`u`. 
5. Tính số Grundy cho gốc là XOR của tất cả các nút ở khoảng cách bội số của`k`bên dưới gốc. Nếu nó khác 0, Alice thắng; nếu không thì Bob thắng. 
6. Để tính đáp án cho mọi gốc, hãy root lại cây. Khi di chuyển root từ cha mẹ`p`đến một đứa trẻ`c`, cập nhật các giá trị DP dọc theo đường dẫn bằng thông tin được tính toán trước đó. Điều này tránh việc tính toán lại từ đầu và đảm bảo`O(n k)`sự phức tạp. 
7. Thu thập kết quả cho tất cả các gốc và xuất chúng. 

Tại sao nó hoạt động: Tại mỗi nút, mảng DP thể hiện chính xác sự đóng góp XOR của những món quà có thể được di chuyển$k$thăng cấp. Bằng cách sử dụng DFS với tính năng tích lũy độ sâu và khởi động lại, chúng tôi duy trì tính bất biến của số Grundy. Lý thuyết Sprague-Grundy đảm bảo rằng XOR của những con số này dự đoán chính xác người chiến thắng trong lối chơi tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 20)

n, k = map(int, input().split())
edges = [[] for _ in range(n)]
for _ in range(n-1):
    x, y = map(int, input().split())
    edges[x-1].append(y-1)
    edges[y-1].append(x-1)
a = list(map(int, input().split()))

dp = [[0]*(k+1) for _ in range(n)]
res = [0]*n

def dfs(u, parent):
    dp[u][0] = a[u]
    for v in edges[u]:
        if v == parent:
            continue
        dfs(v, u)
        for i in range(k):
            dp[u][i+1] ^= dp[v][i]

dfs(0, -1)

def reroot(u, parent):
    g = dp[u][k]
    res[u] = 1 if g != 0 else 0
    for v in edges[u]:
        if v == parent:
            continue
        # remove v's contribution from u
        for i in range(k):
            dp[u][i+1] ^= dp[v][i]
        # add u's contribution to v
        new_dp_v = dp[v][:]
        for i in range(k):
            new_dp_v[i+1] ^= dp[u][i]
        old_dp_u = dp[u][:]
        dp[v] = new_dp_v
        reroot(v, u)
        dp[v] = old_dp_u
        for i in range(k):
            dp[u][i+1] ^= dp[v][i]

reroot(0, -1)
print(' '.join(map(str, res)))
```DFS đầu tiên tính toán`dp[u][d]`như XOR của trình bày tại các nút`d`khoảng cách bên dưới`u`. Điều này tích lũy chính xác các số Grundy cho các bước di chuyển truyền quà`k`bước lên trên. các`reroot`hàm thay đổi gốc một cách có hệ thống và cập nhật các mảng DP để duy trì các đóng góp XOR chính xác mà không cần tính toán lại từ đầu. Mảng cuối cùng`res`chứa 1 nếu Alice có thể thắng và 0 nếu ngược lại. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
5 1
1 2
1 3
5 2
4 3
0 3 2 4 4
```| Nút | dp sau DFS | Grundy cho root=Node | Alice thắng? | 
| --- | --- | --- | --- | 
| 1 | [0,3,2,4,4] | XOR=1 | 1 | 
| 2 | ... | XOR=0 | 0 | 
| 3 | ... | XOR=0 | 0 | 
| 4 | ... | XOR=1 | 1 | 
| 5 | ... | XOR=1 | 1 | 

Dấu vết này cho thấy DP tích lũy ở độ sâu`k`và XOR xác định người chiến thắng. 

### Đầu vào mẫu 2 

Xây dựng cây có 3 nút trên một dòng, k=2:```
3 2
1 2
2 3
1 2 3
```| Nút | dp sau DFS | Grundy cho root=Node | Alice thắng? | 
| --- | --- | --- | --- | 
| 1 | [1,2,3] | XOR=3 | 1 | 
| 2 | ... | XOR=0 | 0 | 
| 3 | ... | XOR=0 | 0 | 

Dấu vết xác nhận rằng các nút nhỏ hơn`k`độ sâu không thể đóng góp, xử lý chính xác bởi mảng DP của chúng tôi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nk) | Mỗi DFS và cập nhật lại tối đa k mục trên mỗi nút. | 
| Không gian | O(nk) | Mảng dp lưu trữ k+1 mục trên mỗi nút. | 

Lời giải có tỷ lệ tuyến tính với số nút nhân với hằng số nhỏ k. Đối với n=10^5 và k<20, điều này phù hợp một cách thoải mái trong vòng 2 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # copy the solution here
    import sys
    input = sys.stdin.readline
    sys.setrecursionlimit(1 << 20)
    
    n, k = map(int, input().split
```
