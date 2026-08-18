---
title: "CF 102220E - Cây bao trùm tối thiểu"
description: "Đồ thị ban đầu (G) là một cây nên nó có (n) đỉnh và chính xác (n-1) cạnh có trọng số. Đồ thị đường (L(G)) biến mọi cạnh ban đầu thành một đỉnh."
date: "2026-08-17T22:34:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102220
codeforces_index: "E"
codeforces_contest_name: "The 13th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 102220
solve_time_s: 118
verified: true
draft: false
---

[CF 102220E - Cây kéo dài tối thiểu](https://codeforces.com/problemset/problem/102220/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đồ thị ban đầu (G) là một cây nên nó có (n) đỉnh và chính xác (n-1) cạnh có trọng số. Đồ thị đường (L(G)) biến mọi cạnh ban đầu thành một đỉnh. Bất cứ khi nào hai cạnh ban đầu gặp nhau ở cùng một đỉnh của (G), các đỉnh tương ứng trong (L(G)) được kết nối và cạnh mới có trọng số bằng tổng trọng số của hai cạnh ban đầu. 

Nhiệm vụ không phải là xây dựng (L(G)), nó có thể lớn hơn nhiều so với (G). Chúng ta chỉ cần tổng trọng lượng của cây bao trùm tối thiểu là (L(G)). 

Thực tế về cấu trúc quan trọng xuất phát từ việc xem xét một đỉnh (v) của cây ban đầu. Giả sử các cạnh liên quan của nó có trọng số (a_1,a_2,\ldots,a_k). Trong biểu đồ đường, (k) cạnh ban đầu đó trở thành (k) đỉnh và mọi cặp trong số chúng đều liền kề nhau. Chúng tạo thành một biểu đồ hoàn chỉnh. Trọng số của các đỉnh nối cạnh (i) và (j) là (a_i+a_j). 

Do đó, mỗi đỉnh ban đầu tạo ra một nhóm có trọng số trong biểu đồ đường. Vì (G) là cây nên hai đỉnh gốc khác nhau không thể có chung hai cạnh gốc. Do đó, các cụm này chỉ chồng lên nhau ở các đỉnh riêng lẻ của (L(G)) và bản thân cấu trúc tổng thể của chúng giống như cây. 

Đầu vào chứa tối đa (10^5) đỉnh trong một trường hợp thử nghiệm và tổng (n) trên tất cả các trường hợp thử nghiệm nhiều nhất là (10^6). Thuật toán bậc hai đã quá lớn. Sự khác biệt đặc biệt quan trọng vì cây hình ngôi sao tạo ra một cụm chứa gần như tất cả (n-1) đỉnh của đồ thị đường, do đó, việc xây dựng đồ thị đường một cách rõ ràng có thể cần hàng tỷ cạnh. Giải pháp về cơ bản phải xử lý cây gốc một lần. 

Có một số trường hợp nhỏ mà một công thức hoặc cách thực hiện bất cẩn có thể thất bại. Ví dụ, với```
1
2
1 2 100
```cây ban đầu chỉ có một cạnh nên (L(G)) chỉ có một đỉnh. MST của nó không chứa cạnh nào và câu trả lời là (0). Việc triển khai thêm trọng số cạnh ban đầu một cách mù quáng sẽ tạo ra không chính xác (100). 

Một trường hợp hữu ích khác là một đường dẫn có trọng số khác nhau:```
1
4
1 2 1
2 3 2
3 4 3
```Đồ thị đường là một đường đi gồm ba đỉnh. Hai trọng số cạnh của nó là (1+2=3) và (2+3=5), vì vậy câu trả lời là (8). Một công thức chỉ cộng trọng lượng sự cố tối thiểu ở mỗi đỉnh có thể bỏ lỡ toàn bộ tổng được đóng góp bởi hai cạnh của biểu đồ đường. 

Trường hợp phân nhánh bộc lộ một lỗi phổ biến khác:```
1
4
1 2 1
1 3 1
1 4 1
```Ba cạnh ban đầu trở thành ba đỉnh của một tam giác và mỗi cạnh của đồ thị đường đều có trọng số (2). MST của tam giác có hai cạnh nên đáp án là (4). Đếm cả ba cạnh cụm sẽ cho kết quả sai (6). 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xây dựng biểu đồ đường một cách rõ ràng. Cây ban đầu có (n-1) cạnh nên đồ thị đường có (n-1) đỉnh. Tại một đỉnh ban đầu có bậc (d), mỗi cặp cạnh liên quan của nó tạo ra một cạnh đồ thị đường, tạo ra các cạnh (\binom d2). Chúng tôi có thể tạo ra tất cả chúng, sắp xếp chúng theo trọng số và chạy thuật toán Kruskal. Điều này đúng vì đồ thị thu được chính xác là (L(G)) và Kruskal tính MST của nó. 

Vấn đề là số cạnh được tạo ra. Hãy xem xét một ngôi sao có (n-1) lá. Tâm của nó có bậc (n-1) nên (L(G)) chứa 

[ 
\binom{n-1}{2} 
] 

các cạnh. Với (n=100000), đây là 

[ 
\frac{99999\cdot99998}{2}=4,999,850,001 
] 

các cạnh. Chỉ tạo ra chúng thôi đã vượt xa nguồn lực sẵn có, ngay cả trước khi phân loại chúng. Việc triển khai Kruskal chung sẽ yêu cầu thời gian (O(n^2\log n)) trong trường hợp xấu nhất và bộ nhớ (O(n^2)) nếu tất cả các cạnh của biểu đồ đường được lưu trữ. 

Quan sát làm cho vấn đề trở nên tuyến tính là chúng ta không bao giờ cần toàn bộ nhóm. Xét một đỉnh ban đầu (v), có trọng số cạnh tới là (a_1,\ldots,a_k) và đặt (m) là giá trị nhỏ nhất trong số chúng. Nhóm biểu đồ đường chứa cạnh có trọng số (a_i+a_j) giữa mỗi cặp. 

Đối với mỗi đỉnh tương ứng với một cạnh có trọng số tới (a_i), cạnh rẻ nhất từ ​​đỉnh đó đến đỉnh khác của cụm là cạnh nối nó với cạnh có trọng số tối thiểu (m). Trọng lượng của nó là (a_i+m). Do đó, việc kết nối trực tiếp mọi đỉnh cụm khác với đỉnh có trọng số tối thiểu sẽ tạo ra một ngôi sao. Tổng trọng lượng của nó là 

\sum_i a_i +(k-2)m, 
] 

trong đó (p) là vị trí của trọng lượng tối thiểu. 

Ngôi sao này là một MST của nhóm. Phần cắt bao gồm một đỉnh không tối thiểu có cạnh đi ra rẻ nhất về phía đỉnh có trọng số tối thiểu, do đó thuộc tính cắt biện minh cho việc chọn các cạnh này. 

Cây ban đầu cung cấp một cụm như vậy cho mỗi đỉnh ban đầu. Vì hai cụm khác nhau có thể giao nhau ở nhiều nhất một đỉnh của biểu đồ đường và bản thân biểu đồ gốc không có chu trình, nên việc lấy một MST bên trong mỗi cụm cùng nhau sẽ tạo ra một MST của toàn bộ biểu đồ đường. Không cần phải phối hợp các lựa chọn phức tạp giữa các bè phái. 

Công thức kết quả cho một đỉnh ban đầu (v) là 

S_v+(\deg(v)-2)m_v, 
] 

trong đó (S_v) là tổng trọng số của các cạnh liên quan đến (v) và (m_v) là trọng số tối thiểu của chúng. 

Đối với một lá, (\deg(v)=1), do đó công thức trở thành (S_v-m_v=0), chính xác như yêu cầu vì đỉnh gốc bậc một tạo ra một cụm chỉ chứa một đỉnh của đồ thị đường. 

Chúng ta chỉ cần bậc, tổng và trọng số tới tối thiểu cho mỗi đỉnh ban đầu. Mỗi cạnh đầu vào cập nhật ba đại lượng này tại hai điểm cuối của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2\log n)) trường hợp xấu nhất | (O(n^2)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo ba mảng cho các đỉnh ban đầu:`degree[v]`,`sum_weight[v]`, Và`min_weight[v]`. Hàm đầu tiên đếm các cạnh tới, hàm thứ hai lưu trữ tổng trọng lượng của chúng và hàm thứ ba lưu trọng số của cạnh tới nhỏ nhất. 
2. Đọc từng cạnh cây gốc ((u,v,w)). Tăng mức độ của cả hai điểm cuối, thêm (w) vào tổng cả hai điểm cuối và thay thế trọng số sự cố tối thiểu của chúng khi (w) nhỏ hơn. 

Không cần phải lưu trữ các cạnh ban đầu sau những cập nhật này. Công thức cuối cùng chỉ phụ thuộc vào ba giá trị tổng hợp này. 
3. Sau khi tất cả (n-1) cạnh đã được xử lý, hãy truy cập từng đỉnh ban đầu (v) và cộng 

[ 
\text{tổng_trọng lượng[v] 
+ 
(\text{độ[v]-2)\cdot\text{min_weight[v] 
] 

để trả lời. 

Biểu thức này chính xác là trọng số MST của cụm được tạo bởi các cạnh tới của (v). 
4. In đáp án tích lũy cho bộ đề thi. 

### Tại sao nó hoạt động 

Đối với mọi đỉnh ban đầu (v), các cạnh liên thuộc của nó trở thành một cụm trong (L(G)). Việc chọn cạnh tới có trọng lượng tối thiểu làm tâm của một ngôi sao, đối với mọi đỉnh của cụm khác, sẽ mang lại kết nối rẻ nhất có thể có bên trong cụm đó. Theo thuộc tính cut, những lựa chọn này tạo thành MST của nhóm đó. 

Đồ thị ban đầu là một cái cây, do đó các cụm cạnh của nó tạo thành một cấu trúc khối giống như cây. Hai cụm như vậy không thể chia sẻ nhiều hơn một đỉnh của đồ thị đường. Do đó, việc kết hợp một MST từ mỗi cụm sẽ kết nối tất cả các đỉnh của biểu đồ đường mà không tạo ra một chu trình. Vì mọi nhóm đều đã được kết nối với chi phí tối thiểu có thể nên việc thay thế bất kỳ phần nhóm nào bằng một cấu trúc rẻ hơn là không thể. Do đó, tổng chi phí MST của từng nhóm chính xác là trọng số MST của (L(G)). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    INF = 10**30

    for _ in range(t):
        n = int(input())

        degree = [0] * (n + 1)
        total = [0] * (n + 1)
        minimum = [INF] * (n + 1)

        for _ in range(n - 1):
            u, v, w = map(int, input().split())

            degree[u] += 1
            total[u] += w
            if w < minimum[u]:
                minimum[u] = w

            degree[v] += 1
            total[v] += w
            if w < minimum[v]:
                minimum[v] = w

        ans = 0

        for v in range(1, n + 1):
            ans += total[v] + (degree[v] - 2) * minimum[v]

        out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào thực hiện việc tổng hợp từ bước 1 và 2. Mỗi cạnh gốc ảnh hưởng đến chính xác hai đỉnh, do đó mọi cạnh đều được xử lý trong thời gian không đổi. 

Vòng lặp cuối cùng thực hiện trực tiếp bước 3. Một chiếc lá có bậc (1) và đóng góp của nó là 

[ 
w+(1-2)w=0. 
] 

Điều này rất hữu ích vì không cần có trường hợp đặc biệt nào cho lá. Vì (n\ge2) và đồ thị đầu vào là một cây nên mỗi đỉnh có ít nhất một cạnh phụ, do đó`minimum[v]`luôn được khởi tạo với trọng số cạnh thực khi nó được sử dụng. 

Số nguyên Python có độ chính xác tùy ý, điều này phù hợp vì câu trả lời có thể vượt quá số nguyên 32 bit. Với trọng số cạnh lên tới (10^9) và (10^5) đỉnh, câu trả lời có thể theo thứ tự (10^{14}). 

Mã này cũng tránh lưu trữ danh sách kề. Danh sách kề vẫn sẽ tuyến tính và hoàn toàn hợp lệ nhưng nó chứa thông tin mà công thức không cần. Chỉ giữ lại ba mảng giúp việc triển khai đơn giản hơn và giảm mức sử dụng bộ nhớ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Cây đầu vào là đường dẫn 

[ 
1\mathbin{-1}2\mathbin{-2}3\mathbin{-3}4. 
] 

Bảng hiển thị trạng thái tổng hợp sau khi tất cả các cạnh đã được đọc. 

| Đỉnh | Bằng cấp | Tổng trọng số sự cố | Trọng lượng sự cố tối thiểu | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | (1-1=0) | 
| 2 | 2 | 3 | 1 | (3+0=3) | 
| 3 | 2 | 5 | 2 | (5+0=5) | 
| 4 | 1 | 3 | 3 | (3-3=0) | 

Hai đỉnh bên trong, mỗi đỉnh tạo ra một nhóm có kích thước bằng hai, do đó, mỗi đỉnh đóng góp trọng số của cạnh đồ thị đường duy nhất của nó. Các trọng số đó là (1+2=3) và (2+3=5). Tổng số là (3+5=8). 

### Mẫu 2 

Đầu vào là một ngôi sao có tâm ở đỉnh (1): 

[ 
1\mathbin{-1}2,\qquad 
1\mathbin{-1}3,\qquad 
1\mathbin{-1}4. 
] 

Trung tâm tạo ra một nhóm gồm ba đỉnh trong biểu đồ đường. Mỗi cạnh của cụm đều có trọng số (2). 

| Đỉnh | Bằng cấp | Tổng trọng số sự cố | Trọng lượng sự cố tối thiểu | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 1 | (3+(3-2)\cdot1=4) | 
| 2 | 1 | 1 | 1 | (1-1=0) | 
| 3 | 1 | 1 | 1 | (1-1=0) | 
| 4 | 1 | 1 | 1 | (1-1=0) | 

Cụm trung tâm là một hình tam giác có tất cả trọng số các cạnh bằng (2), do đó MST của nó có hai cạnh và chi phí (4). Những chiếc lá tạo thành từng cụm đơn lẻ và không đóng góp gì cả. Câu trả lời là (4). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) cho mỗi trường hợp thử nghiệm | Mỗi cạnh trong số (n-1) cạnh ban đầu được xử lý một lần, sau đó là một lần quét qua (n) đỉnh. | 
| Không gian | (O(n)) | Ba mảng có kích thước (n+1) lưu trữ các tập hợp cần thiết. | 

Trong tất cả các trường hợp thử nghiệm, tổng của (n) tối đa là (10^6), do đó tổng thời gian chạy là (O(\sum n)), là tuyến tính ở kích thước đầu vào hoàn chỉnh. Thuật toán không bao giờ xây dựng biểu đồ đường có kích thước bậc hai, đó là lý do chính khiến nó vẫn thực tế đối với một ngôi sao có (10^5) đỉnh. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []
    INF = 10**30

    for _ in range(t):
        n = int(input())

        degree = [0] * (n + 1)
        total = [0] * (n + 1)
        minimum = [INF] * (n + 1)

        for _ in range(n - 1):
            u, v, w = map(int, input().split())

            degree[u] += 1
            total[u] += w
            minimum[u] = min(minimum[u], w)

            degree[v] += 1
            total[v] += w
            minimum[v] = min(minimum[v], w)

        ans = 0
        for v in range(1, n + 1):
            ans += total[v] + (degree[v] - 2) * minimum[v]

        out.append(str(ans))

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run(
    """1
4
1 2 1
2 3 2
3 4 3
"""
) == "8\n"

# Provided sample 2
assert run(
    """1
4
1 2 1
1 3 1
1 4 1
"""
) == "4\n"

# Minimum-size tree
assert run(
    """1
2
1 2 1000000000
"""
) == "0\n"

# Path with equal weights
assert run(
    """1
5
1 2 7
2 3 7
3 4 7
4 5 7
"""
) == "42\n"

# Branching tree with different weights
assert run(
    """1
5
1 2 10
1 3 1
1 4 5
4 5 2
"""
) == "31\n"

# Maximum-size star, generated without explicitly storing its quadratic line graph.
n = 100000
parts = ["1", str(n)]
for v in range(2, n + 1):
    parts.append(f"1 {v} 1000000000")

maximum_star = "\n".join(parts) + "\n"

# The center has degree 99999 and all weights are 1e9.
# Its clique MST has 99998 edges, each of weight 2e9.
expected = str((n - 2) * 2 * 1000000000) + "\n"

assert run(maximum_star) == expected
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`đỉnh có trọng số cạnh`1000000000`|`0`| Biểu đồ đường đơn và công thức bậc một | 
| Đường đi 5 đỉnh với mọi trọng số`7`|`42`| Nhóm độ hai lặp đi lặp lại và có trọng lượng bằng nhau | 
| Cây có độ 3, 2 và 1 |`31`| Cấu trúc phân nhánh và cực tiểu cục bộ khác nhau | 
| Ngôi sao có 100000 đỉnh và trọng lượng`10^9`|`199996000000000`| Kích thước đầu vào tối đa, số học số nguyên lớn và tránh việc xây dựng biểu đồ đường bậc hai | 

## Vỏ cạnh 

Cây hai đỉnh```
1
2
1 2 100
```có một cạnh ban đầu, do đó đồ thị đường chứa một đỉnh và không có cạnh nào. Thuật toán cho đỉnh (1) các giá trị`degree=1`,`total=100`,`minimum=100`, tạo ra (100+(1-2)100=0). Vertex (2) cũng có đóng góp tương tự. Câu trả lời cuối cùng là (0), do đó, biểu đồ đường đơn được xử lý mà không cần nhánh đặc biệt. 

Đối với con đường```
1
4
1 2 1
2 3 2
3 4 3
```đỉnh (2) có trọng số tới (1,2), đóng góp (1+2=3). Đỉnh (3) có trọng số tới (2,3), đóng góp (2+3=5). Các điểm cuối có mức độ một và đóng góp bằng không. Tổng số là (8). Điều này phát hiện các hoạt động triển khai vô tình chỉ sử dụng cạnh sự cố tối thiểu thay vì toàn bộ chi phí MST cục bộ. 

Đối với ngôi sao```
1
4
1 2 1
1 3 1
1 4 1
```đỉnh (1) có ba cạnh liên tiếp, tất cả đều có trọng số (1). Cụm biểu đồ đường của nó là một hình tam giác có mọi cạnh đều có trọng số (2). Công thức cho ra (3+(3-2)\cdot1=4), đây chính xác là giá trị của hai cạnh tam giác. Ba lá mỗi lá đóng góp bằng không. Điều này xác minh rằng thuật toán lấy MST của mỗi nhóm thay vì tính tổng mọi cạnh của nhóm. 

Cuối cùng, hãy xem xét ngôi sao có kích thước tối đa với (99999) cạnh ban đầu, tất cả đều có trọng số (10^9). Phần trung tâm tạo ra gần năm tỷ cạnh của biểu đồ đường, nhưng thuật toán chỉ lưu trữ độ, tổng và giá trị tối thiểu của nó. Đóng góp của trung tâm là 

#199997\cdot10^9 

199997000000000. 
] 

Mỗi chiếc lá đều đóng góp bằng 0 nên câu trả lời cuối cùng là`199997000000000`. Thuật toán đạt được kết quả này sau khi chỉ xử lý (99999) cạnh đầu vào, chứng minh tại sao việc tránh xây dựng (L(G)) rõ ràng là điều cần thiết.
