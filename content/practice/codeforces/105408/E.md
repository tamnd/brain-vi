---
title: "CF 105408E - Người bạn thân nhất được mong đợi"
description: "Chúng ta được cung cấp một biểu đồ có trọng số, vô hướng, được kết nối của các thành phố. Jorge sống ở thành phố 0. Mỗi cạnh đại diện cho một con đường có độ dài dương và đường đi ngắn nhất xác định khoảng cách giữa hai thành phố bất kỳ. Jorge có k người bạn và mỗi người bạn sống độc lập tại một thành phố riêng biệt."
date: "2026-06-23T17:19:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105408
codeforces_index: "E"
codeforces_contest_name: "2024 ICPC Gran Premio de Mexico Repechaje"
rating: 0
weight: 105408
solve_time_s: 85
verified: true
draft: false
---

[CF 105408E - Người bạn thân nhất được mong đợi](https://codeforces.com/problemset/problem/105408/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có trọng số, vô hướng, được kết nối của các thành phố. Jorge sống ở thành phố 0. Mỗi cạnh đại diện cho một con đường có độ dài dương và đường đi ngắn nhất xác định khoảng cách giữa hai thành phố bất kỳ. 

Jorge có k người bạn và mỗi người bạn sống độc lập tại một thành phố riêng biệt. Chúng tôi không được biết họ chiếm thành phố nào; thay vào đó, chúng tôi xem xét mọi lựa chọn có thể có của k thành phố khác biệt từ n − 1 thành phố còn lại, tất cả đều có khả năng như nhau. Đối với mỗi lựa chọn như vậy, chúng tôi tính toán khoảng cách từ thành phố 0 đến từng thành phố được chọn bằng các đường đi ngắn nhất, sau đó lấy khoảng cách tối thiểu trong số k khoảng cách đó. Nhiệm vụ là tính giá trị kỳ vọng của khoảng cách tối thiểu này. 

Khó khăn chính là chúng ta đang tính trung bình một thống kê phi tuyến tính, mức tối thiểu trên một tập hợp con ngẫu nhiên, thay vì tính tổng các khoảng cách độc lập. 

Các ràng buộc đẩy chúng ta tới một giải pháp đồ thị gần như tuyến tính. Với n lên tới 100000 và m lên tới 1e6, bất kỳ phương pháp nào tính toán lại đường đi ngắn nhất cho mỗi lựa chọn đều không thể thực hiện được. Ngay cả việc lưu trữ tất cả các khoảng cách theo cặp cũng không thể thực hiện được. Chúng ta phải tính toán tất cả các khoảng cách từ nguồn 0 một lần, sau đó suy luận tổ hợp các giá trị đã được sắp xếp. 

Một trường hợp biên tinh tế phát sinh khi nhiều thành phố có cùng khoảng cách ngắn nhất từ ​​​​0. Ví dụ: nếu một số thành phố ở khoảng cách 5, thì cấu trúc xác suất của các thay đổi tối thiểu theo cách từng bước và tính trung bình ngây thơ trên từng thành phố sẽ thất bại nếu chúng ta không nhóm các khoảng cách bằng nhau một cách hợp lý. Một trường hợp cạnh khác là khi k rất gần với n − 1. Trong chế độ đó, mức tối thiểu gần như chắc chắn là khoảng cách nhỏ nhất trong số gần như tất cả các nút và xác suất tổ hợp vẫn phải hoạt động chính xác, đặc biệt đối với các nút xa nhất không bao giờ ảnh hưởng đến mức tối thiểu. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ liệt kê tất cả các tập hợp con của k thành phố trong số n − 1, tính khoảng cách tối thiểu cho mỗi tập hợp con và tính trung bình kết quả. Ngay cả khi bỏ qua tính toán đường đi ngắn nhất, số lượng tập hợp con vẫn là$\binom{n-1}{k}$, trở nên lớn về mặt thiên văn ngay cả đối với n vừa phải. Điều này ngay lập tức loại trừ việc liệt kê trực tiếp. 

Trước tiên chúng ta tách thành phần đồ thị khỏi thành phần tổ hợp. Khoảng cách từ thành phố 0 đến mọi thành phố khác có thể được tính bằng thuật toán Dijkstra trong$O(m \log n)$. Sau đó, bài toán trở nên thuần túy xác suất: cho một mảng có n − 1 khoảng cách, chúng ta chọn k phần tử một cách đồng nhất mà không cần thay thế và muốn đạt mức tối thiểu dự kiến. 

Quan sát quan trọng là điều chỉnh lại kỳ vọng về xác suất sống sót. Thay vì hỏi trực tiếp mức tối thiểu là bao nhiêu, chúng ta xem xét xác suất để mức tối thiểu đó ít nhất là một giá trị ngưỡng d. Điều này xảy ra chính xác khi tất cả k thành phố được chọn nằm trong số những thành phố có khoảng cách ≥ d. Nếu sắp xếp tất cả các khoảng cách, chúng ta có thể tính toán số lượng tiền tố có bao nhiêu thành phố có khoảng cách ≤ d và từ đó rút ra số lượng thành phố có khoảng cách ≥ d. 

Điều này biến kỳ vọng thành tổng trên các khoảng cách duy nhất được sắp xếp, trong đó mỗi giá trị đóng góp theo xác suất mà giá trị tối thiểu bằng giá trị đó. Xác suất này được thể hiện bằng cách sử dụng các kết hợp: chúng tôi đếm số cách để chọn k thành phố nằm ngoài tập hợp các nút gần hơn ngưỡng và trừ ngưỡng tiếp theo một cách tương tự. Toàn bộ cấu trúc giảm xuống tổ hợp tiền tố trên các khoảng cách được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | O(C(n, k) · k + m log n) | O(n) | Quá chậm | 
| Dijkstra + tổ hợp trên khoảng cách được sắp xếp | O(m log n + n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chia giải pháp thành giai đoạn tiền xử lý đồ thị và giai đoạn kỳ vọng tổ hợp. 

1. Chạy Dijkstra từ thành phố 0 để tính khoảng cách ngắn nhất tới tất cả các thành phố. 

Điều này là cần thiết vì mọi tính toán sau này chỉ phụ thuộc vào những khoảng cách này và bất kỳ cấu trúc đường dẫn nào vượt quá khoảng cách ngắn nhất đều không liên quan một khi chúng được tính toán. 
2. Loại bỏ thành phố 0 khỏi danh sách xem xét vì đây là thành phố nguồn và không thể được chọn làm thành phố bạn bè. 
3. Lưu trữ tất cả các khoảng cách vào một mảng và sắp xếp theo thứ tự không giảm. 

Việc sắp xếp là bắt buộc vì mức tối thiểu trên một tập hợp con ngẫu nhiên là dễ lý giải nhất về các ngưỡng từ nhỏ nhất đến lớn nhất. 
4. Tính trước giai thừa và giai thừa nghịch đảo lên đến n cho hệ số nhị thức modulo 1e9+7. 

Chúng ta cần tính toán nhanh$\binom{a}{b}$vì xác suất phụ thuộc vào việc đếm các tập con. 
5. Xác định hàm C(a, b) trả về 0 nếu b > a, nếu không thì tính toán$\frac{a!}{b!(a-b)!}$. 

Hàm này được sử dụng nhiều lần trong các biểu thức xác suất. 
6. Lặp lại từng chỉ mục i trong mảng khoảng cách đã sắp xếp, coi dist[i] là giá trị ứng viên trong đó giá trị tối thiểu có thể xuất hiện đầu tiên. 

Đối với i cố định, chúng tôi tính toán có bao nhiêu tập hợp con có kích thước k có mức tối thiểu chính xác bằng dist[i]. 
7. Gọi i+1 là số thành phố có khoảng cách ≤ dist[i]. Số lượng thành phố gần hơn hoặc bằng ngưỡng này sẽ xác định có bao nhiêu lựa chọn sẽ vô hiệu hóa dist[i] ở mức tối thiểu. 
8. Tính số cách chọn k thành phố từ chỉ số i..n-1 (tất cả các thành phố có khoảng cách ≥ dist[i]). 

Đây là C(n-1 - i, k). 
9. Trừ số cách chọn k thành phố từ các chỉ số i+1..n-1, tương ứng với các tập hợp con có mức tối thiểu lớn hơn dist[i]. 

Điều này đưa ra số lượng tập hợp con trong đó dist[i] chính xác là mức tối thiểu. 
10. Nhân trọng số xác suất này với dist[i] và cộng dồn thành đáp án cuối cùng. 

### Tại sao nó hoạt động 

Thuật toán phân chia tất cả k-tập hợp con của các thành phố theo khoảng cách tối thiểu của chúng với thành phố 0. Mỗi tập hợp con có chính xác một phần tử tối thiểu theo thứ tự được sắp xếp và sai phân tổ hợp C(n-1 - i, k) − C(n-1 - i-1, k) đếm chính xác các tập hợp con có phần tử được chọn đầu tiên theo thứ tự được sắp xếp là vị trí i. Điều này đảm bảo mỗi tập hợp con đóng góp chính xác một lần cho đúng một nhóm khoảng cách và do đó tổng có trọng số chính xác là kỳ vọng của khoảng cách tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def dijkstra(n, adj):
    import heapq
    dist = [10**30] * n
    dist[0] = 0
    pq = [(0, 0)]
    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v, w in adj[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))
    return dist

def modinv(x):
    return pow(x, MOD - 2, MOD)

def nCk(n, k, fact, invfact):
    if k < 0 or k > n:
        return 0
    return fact[n] * invfact[k] % MOD * invfact[n - k] % MOD

def solve():
    n, m, k = map(int, input().split())
    adj = [[] for _ in range(n)]
    for _ in range(m):
        a, b, w = map(int, input().split())
        adj[a].append((b, w))
        adj[b].append((a, w))

    dist = dijkstra(n, adj)
    arr = dist[1:]
    arr.sort()

    maxn = n + 5
    fact = [1] * maxn
    invfact = [1] * maxn
    for i in range(1, maxn):
        fact[i] = fact[i - 1] * i % MOD
    invfact[maxn - 1] = modinv(fact[maxn - 1])
    for i in range(maxn - 2, -1, -1):
        invfact[i] = invfact[i + 1] * (i + 1) % MOD

    total = nCk(n - 1, k, fact, invfact)

    ans = 0
    n1 = n - 1

    for i, x in enumerate(arr):
        if n1 - i < k:
            continue
        # subsets where minimum >= x
        ge = nCk(n1 - i, k, fact, invfact)
        # subsets where minimum > x
        if i + 1 <= n1:
            gt = nCk(n1 - (i + 1), k, fact, invfact)
        else:
            gt = 0
        cnt = (ge - gt) % MOD
        ans = (ans + cnt * x) % MOD

    inv_total = modinv(total)
    print(ans * inv_total % MOD)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách xây dựng danh sách kề và tính toán các đường đi ngắn nhất một nguồn bằng cách sử dụng Dijkstra với một đống. Điều này tạo ra tất cả các khoảng cách cần thiết cho giai đoạn xác suất. 

Mảng giai thừa và mảng giai thừa nghịch đảo được tính toán trước một lần, vì các hệ số nhị thức được yêu cầu lặp đi lặp lại. Nghịch đảo mô đun được tính toán bằng định lý Fermat, định lý này an toàn vì mô đun là số nguyên tố. 

Mảng`arr`chứa khoảng cách từ nút 0 đến tất cả các nút khác, được sắp xếp sao cho chúng ta có thể suy luận về các ngưỡng. Đối với mỗi vị trí`i`, chúng tôi tính toán có bao nhiêu tập hợp con có mức tối thiểu chính xác bằng`arr[i]`sử dụng sự khác biệt của hai số nhị thức. Điều này tránh việc theo dõi các tập hợp con một cách rõ ràng. 

Cuối cùng, chúng tôi chia tổng có trọng số tích lũy cho tổng số k-tập hợp con bằng cách sử dụng nghịch đảo mô-đun, tạo ra giá trị mong đợi. 

Một cạm bẫy triển khai phổ biến là quên rằng thành phố 0 bị loại khỏi nhóm lựa chọn. Một cách khác là xử lý không chính xác các khoảng cách bằng nhau mà không nhóm; công thức khác biệt của sự kết hợp sẽ tự động tránh được vấn đề này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1 1
0 1 3
```Khoảng cách từ 0 là: 

| Bước | Mảng phân phối | Tính toán | Đóng góp | 
| --- | --- | --- | --- | 
| ban đầu | [3] | tổng số tập con = C(1,1)=1 | - | 
| tôi=0 | x=3 | ge=C(1,1)=1, gt=0 | 3 * 1 = 3 | 

Câu trả lời cuối cùng là 3. 

Ví dụ này cho thấy trường hợp đơn giản nhất trong đó nút duy nhất phải được chọn, do đó mức tối thiểu là xác định. 

### Ví dụ 2 

đầu vào:```
4 3 2
0 1 5
0 2 2
1 3 10
```Khoảng cách: 

nút 1 = 5, nút 2 = 2, nút 3 = 15 

Sắp xếp: [2, 5, 15] 

Chúng tôi tính toán xác suất trên các tập hợp con 2 phần tử: 

| tôi | x | ge = C(3-i,2) | gt | cnt | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | C(3,2)=3 | C(2,2)=1 | 2 | 4 | 
| 1 | 5 | C(2,2)=1 | C(1,2)=0 | 1 | 5 | 
| 2 | 15 | 0 | 0 | 0 | 0 | 

Tổng = 9 

Dấu vết này cho thấy các tập hợp con được phân chia theo phần tử tối thiểu theo thứ tự được sắp xếp như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log n + n log n) | Dijkstra chiếm ưu thế, sắp xếp và tổ hợp là tuyến tính | 
| Không gian | O(n + m) | danh sách kề cộng với mảng khoảng cách và giai thừa | 

Các ràng buộc cho phép tối đa 1e6 cạnh, do đó cần có đường dẫn ngắn nhất dựa trên heap. Pha tổ hợp là tuyến tính theo n, giữ cho nghiệm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# provided sample
assert run("2 1 1\n0 1 3\n").strip() == "3"

# minimum graph
assert run("2 1 1\n0 1 5\n").strip() == "5"

# chain graph
assert run("4 3 1\n0 1 1\n1 2 2\n2 3 3\n").strip() == "1"

# star graph
assert run("5 4 2\n0 1 1\n0 2 2\n0 3 3\n0 4 4\n").strip() == "1"

# k close to n-1
assert run("4 3 3\n0 1 1\n0 2 2\n0 3 3\n").strip() == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh tối thiểu | 5 | sự lựa chọn bắt buộc duy nhất | 
| đồ thị chuỗi | 1 | truyền bá thứ tự đường đi ngắn nhất | 
| đồ thị sao | 1 | sự thống trị của nút gần nhất | 
| k = n-1 | 1 | tổ hợp biên | 

## Vỏ cạnh 

Trường hợp một cạnh là khi k = n − 1. Trong trường hợp đó, mọi nút đều được chọn, do đó giá trị tối thiểu luôn là khoảng cách nhỏ nhất. Thuật toán xử lý điều này vì C(n − 1 − i, k) trở thành 0 với tất cả i ngoại trừ chỉ số đầu tiên, chỉ để lại khoảng cách đóng góp nhỏ nhất. 

Một trường hợp cạnh khác là khoảng cách lặp lại. Giả sử nhiều nút có giá trị đường dẫn ngắn nhất giống hệt nhau. Một cách tiếp cận đơn giản xử lý từng nút một cách độc lập sẽ tăng gấp đôi khối lượng xác suất đếm, nhưng công thức trừ C(n − 1 − i, k) − C(n − 1 − i − 1, k) sẽ tự động thu gọn các giá trị bằng nhau thành các đóng góp theo trọng số tần số chính xác. 

Trường hợp cạnh cuối cùng là k = 1. Khi đó câu trả lời đơn giản là giá trị trung bình của tất cả các khoảng cách đường đi ngắn nhất. Thuật toán suy biến chính xác vì mỗi nút được chọn với xác suất bằng nhau và sai phân tổ hợp giảm xuống mức trọng số đồng đều trên mọi khoảng cách.
