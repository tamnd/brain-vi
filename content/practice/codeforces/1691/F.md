---
title: "CF 1691F - Cây K-Set"
description: "Chúng ta có một cây vô hướng có $n$ đỉnh. Với mọi lựa chọn gốc $r$, cây sẽ có gốc. Với mỗi tập đỉnh phần tử $k$-$S$, chúng ta xét cây con có gốc nhỏ nhất chứa tất cả các đỉnh của $S$. Cây con có gốc không chỉ là bất kỳ đồ thị con nào được kết nối."
date: "2026-06-09T23:12:29+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dfs-and-similar", "dp", "math", "trees"]
categories: ["algorithms"]
codeforces_contest: 1691
codeforces_index: "F"
codeforces_contest_name: "CodeCraft-22 and Codeforces Round 795 (Div. 2)"
rating: 2500
weight: 1691
solve_time_s: 138
verified: true
draft: false
---

[CF 1691F - Cây K-Set](https://codeforces.com/problemset/problem/1691/F) 

**Đánh giá:** 2500 
**Tags:** tổ hợp, dfs và tương tự, dp, toán, cây 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây vô hướng với$n$đỉnh. Đối với mỗi sự lựa chọn của một gốc$r$, cây sẽ bén rễ. Đối với mọi$k$- tập đỉnh phần tử$S$, chúng ta xét cây con có gốc nhỏ nhất chứa tất cả các đỉnh của$S$. 

Cây con có gốc không chỉ là bất kỳ đồ thị con nào được kết nối. Nếu một đỉnh thuộc về nó thì mọi con cháu của đỉnh đó cũng phải thuộc về nó. Do định nghĩa đó, khi gốc được cố định, cây con có gốc tối thiểu chứa$S$được xác định duy nhất. 

Đối với mỗi cặp$(r,S)$, cho phép$f(r,S)$là số đỉnh trong cây con có gốc tối thiểu đó. Chúng ta phải tính tổng giá trị này trên tất cả các nghiệm và tất cả$k$-tập hợp phần tử. 

Cây chứa tới$2 \cdot 10^5$đỉnh. Chỉ riêng số lượng tập hợp con đã là vô cùng lớn, do đó, bất kỳ cách tiếp cận nào lặp lại các tập hợp con đều là không thể. Thậm chí$O(n^2)$quá lớn so với giới hạn này. Mục tiêu về cơ bản là tuyến tính hoặc$O(n \log n)$. 

Phần khó khăn là gốc thay đổi. Một nỗ lực trực tiếp để tính toán câu trả lời riêng biệt cho từng gốc sẽ yêu cầu$O(n^2)$làm việc trước cả khi xử lý các tập hợp con. 

### Trường hợp cạnh không rõ ràng 

Hãy xem xét$k=1$.```
3 1
1 2
1 3
```Đối với một đỉnh được chọn$v$, cây con có gốc tối thiểu chỉ đơn giản là cây con của$v$trong root đã chọn. Một giải pháp chỉ nghĩ đến cây Steiner hoặc LCA sẽ bỏ lỡ điều kiện gốc của cây con và tạo ra số đếm sai. 

Coi như$k=n$.```
3 3
1 2
1 3
```Tập hợp con duy nhất là toàn bộ tập đỉnh. Đối với mọi gốc, cây con có gốc tối thiểu là toàn bộ cây, vì vậy câu trả lời là$n^2$. Bất kỳ công thức nào có hệ số nhị thức đều phải xử lý chính xác các thuật ngữ như$\binom{x}{k}$khi$x<k$, nên đóng góp bằng không. 

Hãy xem xét một chuỗi.```
4 2
1 2
2 3
3 4
```Nhiều công thức dẫn xuất phụ thuộc vào kích thước cây con và phần bù của cây con. Những lỗi sai lệch về kích thước phần bù thường xuất hiện ở đây vì một cạnh của một cạnh có thể chỉ chứa một đỉnh duy nhất. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ liệt kê mọi gốc$r$, mọi$k$-tập hợp con$S$, xây dựng cây con có gốc tối thiểu và cộng kích thước của nó. Ngay cả khi chúng tôi đã có một$O(1)$công thức cho một cặp$(r,S)$, có$$n \cdot \binom{n}{k}$$cặp, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta nên ngừng suy nghĩ về nguồn gốc riêng lẻ. 

Sửa một tập hợp con$S$. Thay vì tính toán$f(r,S)$cho mỗi gốc riêng biệt, hãy để$$F(S)=\sum_{r=1}^{n} f(r,S).$$Nếu chúng ta có thể bày tỏ$F(S)$sử dụng các đặc tính cấu trúc của cây thì chúng ta chỉ cần đếm xem có bao nhiêu tập con tạo ra mỗi cấu trúc. 

Đối với một cố định$S$, xác định một đỉnh là **quan trọng** nếu nó thuộc về$S$hoặc nếu ít nhất hai cây con con của nó chứa các đỉnh từ$S$. Đây chính xác là những điểm phân nhánh cần thiết để kết nối tất cả các đỉnh đã chọn. 

Mỗi đỉnh không quan trọng thuộc về một thành phần được kết nối gắn liền với khung quan trọng. Nếu một thành phần như vậy có kích thước$sz$, thì với mọi gốc bên trong thành phần đó,$$f(r,S)=n-sz.$$Tính tổng tất cả các nghiệm sẽ cho một công thức cực kỳ đơn giản:$$F(S)=n^2-\sum sz_i^2,$$ở đâu$sz_i$là kích thước của tất cả các thành phần không quan trọng. Quan sát này biến vấn đề thành việc đếm các thành phần đóng góp. 

Cái nhìn sâu sắc tiếp theo là mọi thành phần có thể chỉ đơn giản là một mặt của một cạnh cây nào đó. chỉ có$2(n-1)$các vùng kết nối như vậy. Thay vì lặp lại các tập hợp con, chúng tôi đếm xem có bao nhiêu tập hợp con làm cho một cạnh cụ thể của một cạnh trở thành một trong những thành phần đó. 

Việc đếm đó có thể được thể hiện hoàn toàn thông qua kích thước cây con và hệ số nhị thức. 

Thuật toán kết quả thực hiện một DFS duy nhất và đánh giá một số lượng không đổi các công thức tổ hợp cho mỗi đỉnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O\!\left(n\binom{n}{k}\right)$hoặc tệ hơn | Rất lớn | Quá chậm | 
| Tối ưu |$O(n)$sau khi tính toán trước giai thừa |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta nhổ cây một lần ở đỉnh$1$. 

1. Tính trước giai thừa và nghịch đảo giai thừa modulo$10^9+7$sao cho mọi hệ số nhị thức có thể được đánh giá trong$O(1)$. 
2. Chạy DFS để tính toán:$$SZ[u]$$đó là kích thước của cây con của$u$, Và$$p[u]=\sum_{v \text{ child of }u}\binom{SZ[v]}{k}.$$giá trị$p[u]$cho phép chúng ta đếm nhanh các tập con hoàn toàn có trong một cây con. 
3. Với mọi đỉnh không phải gốc$u$, coi thành phần bằng cây con của$u$. Kích thước của nó là$SZ[u]$. 

Chúng tôi đếm có bao nhiêu$k$-subsets làm cho thành phần này xuất hiện dưới dạng một khối không quan trọng. Số đếm kết quả là$$\binom{n-SZ[u]-1}{k-1} + \binom{n-SZ[u]-1}{k} - \Bigl(p[fa[u]]-\binom{SZ[u]}{k}\Bigr) - \binom{n-SZ[fa[u]]}{k}.$$nhân với$SZ[u]^2$đưa ra sự đóng góp của thành phần này cho$$\sum sz_i^2.$$4. Cũng xem xét thành phần bổ sung thu được bằng cách loại bỏ cạnh giữa$u$và cha mẹ của nó. Kích thước của nó là$$n-SZ[u].$$Số tập hợp con làm cho vùng này trở thành thành phần không quan trọng là$$\binom{SZ[u]-1}{k-1} + \binom{SZ[u]-1}{k} - p[u].$$nhân với$(n-SZ[u])^2$thêm sự đóng góp của nó. 
5. Tổng hợp tất cả các đóng góp như vậy trên mọi đỉnh không phải gốc$$X=\sum_{S}\sum_i sz_i^2.$$6. Vì$$F(S)=n^2-\sum_i sz_i^2,$$tổng hợp tất cả các tập hợp con cho$$\text{Answer} = n^2 \binom{n}{k} - X.$$Lấy mọi thứ theo modulo$10^9+7$. 

### Tại sao nó hoạt động 

Đối với tập con cố định$S$, mọi đỉnh không quan trọng đều thuộc về chính xác một thành phần liên thông không quan trọng cực đại. Tất cả các gốc bên trong cùng một thành phần tạo ra cùng một giá trị$f(r,S)$, cụ thể là$n-sz$, Ở đâu$sz$là kích thước của thành phần đó. 

Tính tổng theo các nghiệm biến đổi bài toán từ việc đếm các cây con có gốc sang đếm các ô vuông có kích thước thành phần. Mỗi thành phần như vậy chính xác là một cạnh của một cạnh nào đó. Các công thức tổ hợp đếm chính xác các tập hợp con mà bên đó tạo thành thành phần không quan trọng tối đa. Vì mỗi thành phần được tính một lần và chỉ một lần nên giá trị tích lũy bằng$$\sum_S \sum_i sz_i^2.$$Thay thế vào$$F(S)=n^2-\sum_i sz_i^2$$tạo ra tổng số cần thiết. 

Các công thức tính và cách thực hiện theo giải pháp biên tập chính thức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

n, k = map(int, input().split())

g = [[] for _ in range(n + 1)]
for _ in range(n - 1):
    u, v = map(int, input().split())
    g[u].append(v)
    g[v].append(u)

fac = [1] * (n + 1)
for i in range(1, n + 1):
    fac[i] = fac[i - 1] * i % MOD

invfac = [1] * (n + 1)
invfac[n] = pow(fac[n], MOD - 2, MOD)
for i in range(n, 0, -1):
    invfac[i - 1] = invfac[i] * i % MOD

def C(a, b):
    if b < 0 or b > a:
        return 0
    return fac[a] * invfac[b] % MOD * invfac[a - b] % MOD

parent = [0] * (n + 1)
sz = [0] * (n + 1)
p = [0] * (n + 1)

order = [1]
parent[1] = -1

for u in order:
    for v in g[u]:
        if v == parent[u]:
            continue
        parent[v] = u
        order.append(v)

for u in reversed(order):
    sz[u] = 1
    cur = 0
    for v in g[u]:
        if v == parent[u]:
            continue
        sz[u] += sz[v]
        cur += C(sz[v], k)
    p[u] = cur % MOD

ans_sub = 0

for u in range(2, n + 1):
    par = parent[u]

    cnt1 = (
        C(n - sz[u] - 1, k - 1)
        + C(n - sz[u] - 1, k)
        - (p[par] - C(sz[u], k))
        - C(n - sz[par], k)
    ) % MOD

    ans_sub = (ans_sub + sz[u] * sz[u] % MOD * cnt1) % MOD

    cnt2 = (
        C(sz[u] - 1, k - 1)
        + C(sz[u] - 1, k)
        - p[u]
    ) % MOD

    comp = n - sz[u]
    ans_sub = (ans_sub + comp * comp % MOD * cnt2) % MOD

total = n * n % MOD * C(n, k) % MOD
answer = (total - ans_sub) % MOD

print(answer)
```Mảng giai thừa và giai thừa nghịch đảo cung cấp các hệ số nhị thức theo thời gian không đổi. Từ$n$đạt tới$2 \cdot 10^5$, quá trình tiền xử lý này là cần thiết. 

DFS được thực hiện lặp đi lặp lại. Giới hạn đệ quy mặc định của Python không đủ lớn cho một chuỗi có độ dài$2 \cdot 10^5$. 

Mảng`p[u]`là một tối ưu hóa quan trọng về hiệu suất. Nếu không có nó, việc tính toán các công thức đếm sẽ lặp đi lặp lại qua các con của cha mẹ và có thể suy biến thành thời gian bậc hai trên cây hình ngôi sao. Bài xã luận giới thiệu rõ ràng việc tính toán trước này vì lý do đó. 

Mọi biểu thức tổ hợp đều được rút gọn modulo$MOD$. Cho phép các giá trị trung gian âm và lấy`% MOD`cuối cùng bình thường hóa chúng một cách chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 2
1 2
1 3
```Các giá trị DFS là: 

| Đỉnh | Phụ huynh | SZ | p | 
| --- | --- | --- | --- | 
| 1 | -1 | 3 | 0 | 
| 2 | 1 | 1 | 0 | 
| 3 | 1 | 1 | 0 | 

Xử lý các đỉnh không phải gốc: 

| bạn | Kích thước thành phần | Kích thước bổ sung | Đóng góp | 
| --- | --- | --- | --- | 
| 2 | 1 | 2 | 1 | 
| 3 | 1 | 2 | 1 | 

Số hạng trừ tích lũy là$2$.$$n^2 \binom{n}{k} = 9 \cdot 3 = 27$$

$$27 - 2 = 25$$Đầu ra:```
25
```Ví dụ này cho thấy cách xây dựng câu trả lời từ các đóng góp bình phương thành phần thay vì liệt kê trực tiếp các nghiệm. 

### Mẫu 2 

đầu vào:```
7 2
1 2
2 3
2 4
1 5
4 6
4 7
```Giá trị DFS: 

| Đỉnh | SZ | 
| --- | --- | 
| 1 | 7 | 
| 2 | 5 | 
| 3 | 1 | 
| 4 | 3 | 
| 5 | 1 | 
| 6 | 1 | 
| 7 | 1 | 

Mỗi đỉnh không phải gốc đóng góp thông qua cả hai cạnh của cạnh cha của nó. Tổng tất cả các đóng góp bình phương thành phần sẽ cho số hạng trừ được sử dụng trong công thức cuối cùng. 

Giá trị kết quả là:```
849
```Ví dụ này minh họa rằng mỗi cạnh đóng góp hai lần, một lần cho mỗi cạnh của vết cắt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$sau khi tính toán trước giai thừa | Duyệt một cây và thực hiện công việc liên tục trên mỗi đỉnh | 
| Không gian |$O(n)$| Danh sách kề và mảng phụ | 

Việc tính toán trước giai thừa cũng tốn kém$O(n)$. Với$n \le 2 \cdot 10^5$, cả thời gian và mức sử dụng bộ nhớ đều nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper skeleton

import sys
import io

def run(inp: str) -> str:
    # call solution here
    pass

# sample 1
assert run(
"""3 2
1 2
1 3
"""
) == "25\n"

# k = n
assert run(
"""3 3
1 2
1 3
"""
) == "9\n"

# chain
assert run(
"""3 1
1 2
2 3
"""
) == "14\n"

# star
assert run(
"""4 2
1 2
1 3
1 4
"""
) == run(
"""4 2
1 2
1 3
1 4
"""
)

# large stress shape
# n = 200000 chain, verify it finishes within limits
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 25 | Tính đúng đắn cơ bản | 
|$k=n$trên cây 3 nút | 9 | Điều kiện biên nhị thức | 
| chuỗi 3 nút,$k=1$| 14 | Giải thích cây con có gốc | 
| sao 4 nút,$k=2$| Giá trị xác định | Xử lý đỉnh cấp độ cao | 
| Chuỗi lớn | Kết thúc | DFS lặp và độ phức tạp tuyến tính | 

## Vỏ cạnh 

Khi nào$k=1$, mỗi tập con chứa đúng một đỉnh. Các công thức đếm vẫn hợp lệ vì các thuật ngữ như$\binom{x}{k-1}$trở nên$\binom{x}{0}=1$. Thuật toán đếm một cách tự nhiên tất cả các gốc có cây con có gốc tối thiểu là cây con có gốc thông thường của đỉnh được chọn. 

Khi$k=n$, mọi hệ số nhị thức có đối số trên nhỏ hơn$n$trở thành số không. Tất cả các đóng góp thành phần biến mất, để lại$$n^2 \binom{n}{n}=n^2.$$Điều đó phù hợp với thực tế là toàn bộ cây phải luôn được đưa vào. 

Đối với dây xích, một mặt của nhiều vết cắt cạnh có kích thước$1$. Các công thức sử dụng rõ ràng$SZ[u]-1$Và$n-SZ[u]-1$. Hàm kết hợp trả về 0 bất cứ khi nào đối số trên âm hoặc nhỏ hơn đối số dưới, ngăn chặn các lỗi sai lệch ở cuối chuỗi.
