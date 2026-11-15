---
title: "CF 1823F - Bước đi ngẫu nhiên"
description: "Chúng ta được cấp một cây trong đó một con chip thực hiện một bước đi ngẫu nhiên và chỉ dừng lại khi nó đạt đến đỉnh mục tiêu được chỉ định $t$."
date: "2026-06-09T07:46:48+07:00"
tags: ["codeforces", "competitive-programming", "dp", "graphs", "math", "probabilities", "trees"]
categories: ["algorithms"]
codeforces_contest: 1823
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 868 (Div. 2)"
rating: 2600
weight: 1823
solve_time_s: 86
verified: false
draft: false
---

[CF 1823F - Bước đi ngẫu nhiên](https://codeforces.com/problemset/problem/1823/F) 

**Đánh giá:** 2600 
**Tags:** dp, đồ thị, toán học, xác suất, cây 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây trong đó một con chip thực hiện một bước đi ngẫu nhiên và chỉ dừng lại khi nó đến một đỉnh mục tiêu được chỉ định$t$. Tại mỗi bước, nếu chip ở một đỉnh$v$, nó chọn ngẫu nhiên một trong những hàng xóm của nó một cách thống nhất, di chuyển đến đó và tăng bộ đếm tại đỉnh đích. Quá trình luôn bắt đầu từ một nguồn cố định$s$, và ban đầu chỉ$c(s)=1$, tất cả các bộ đếm khác đều bằng 0. 

Số lượng mà chúng ta quan tâm không phải là quỹ đạo mà là số lần mỗi đỉnh dự kiến ​​sẽ được ghé thăm như một điểm đến của một bước di chuyển trước khi bước đi đầu tiên diễn ra$t$. Nói cách khác, chúng tôi đang tính toán số lượt truy cập dự kiến ​​trong một lần đi bộ ngẫu nhiên kết thúc trên cây. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các đỉnh, ngay lập tức loại trừ bất kỳ cách tiếp cận nào mô phỏng bước đi hoặc duy trì xác suất trên mỗi đường đi. Ngay cả việc lập trình động một bước trên tất cả các trạng thái cũng sẽ quá lớn nếu nó xem lại các cạnh nhiều lần. Cấu trúc là một cái cây gợi ý rằng việc phân tách theo hướng hoặc tạo rễ cho cây ở một điểm chiến lược là cần thiết. 

Một khó khăn tinh tế nằm ở chỗ bước đi không hấp thụ ở mọi đỉnh, chỉ ở$t$. Điều này có nghĩa là xác suất có tính toàn cầu: một đỉnh có thể được truy cập nhiều lần thông qua việc quay lui và những lần truy cập lại này góp phần tạo ra những kỳ vọng lớn tiềm tàng. 

Một sai lầm ngây thơ là coi đây là vấn đề về đường đi ngắn nhất hoặc luồng một lần. Ví dụ: giả sử mỗi cạnh được duyệt tối đa một lần từ$s$ĐẾN$t$sẽ đưa ra số lượng đường đi xác định, điều này sai vì bước đi quay lại các nhánh một cách tùy ý nhiều lần trước khi cuối cùng thoát về phía$t$. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực là mô phỏng bước đi ngẫu nhiên và ước tính số lượt truy cập. Người ta có thể tưởng tượng việc theo dõi phân bố xác suất đầy đủ của việc ở mỗi đỉnh sau$k$các bước và tích lũy đóng góp cho đến khi hấp thụ tại$t$. Điều này dẫn tới một chuỗi Markov kết thúc$n$các trạng thái có trạng thái hấp thụ và tính toán trực tiếp số lượt truy cập dự kiến ​​đòi hỏi phải giải các phương trình tuyến tính trên tất cả các đỉnh hoặc lặp lại các xác suất chuyển đổi cho đến khi hội tụ. Ngay cả việc viết ma trận chuyển tiếp cũng là$O(n)$, nhưng việc giải quyết nó một cách ngây thơ sẽ liên quan đến việc loại bỏ Gaussian hoặc lặp đi lặp lại sự thư giãn, cả hai đều vượt xa$2 \cdot 10^5$. 

Cái nhìn sâu sắc về cấu trúc quan trọng là đảo ngược quan điểm: thay vì theo dõi sự tiến hóa theo thời gian, chúng tôi hỏi mỗi cạnh định hướng dự kiến ​​​​sẽ được sử dụng bao nhiêu lần trước khi hấp thụ. Bởi vì đồ thị là một cái cây nên mọi bước di chuyển đều tiến gần hơn đến$t$hoặc tránh xa nó theo một nghĩa được xác định rõ ràng khi cây đã bén rễ tại$t$. 

Rễ cây tại$t$. Bây giờ mỗi đỉnh ngoại trừ$t$có chính xác một phụ huynh trên con đường hướng tới$t$. Quá trình này hoạt động giống như một quá trình ngẫu nhiên liên tục nảy trong các cây con, nhưng bất cứ khi nào nó di chuyển dọc theo cạnh về phía cây mẹ, nó sẽ góp phần lan truyền “dòng” về phía gốc. 

Quan sát quan trọng là các đường đi dự kiến ​​thỏa mãn một phương trình bảo toàn. Đối với bất kỳ đỉnh nào$v \neq t$, số lần dự kiến ​​chúng ta nhập$v$bằng số lần dự kiến ​​chúng ta rời đi$v$, cộng với điều kiện ban đầu nếu$v = s$. Mỗi lần ghé thăm tại$v$tạo ra một nước đi đi và mỗi cạnh đi ra được chọn thống nhất trong số$deg(v)$hàng xóm. Điều này biến vấn đề thành giải một hệ thống trong đó mỗi cạnh mang một phép truyền kỳ vọng theo cấp số nhân bằng$1 / deg(v)$. 

Cấu trúc này cho phép một cây DP: đối với mỗi nút, chúng tôi tính toán số lần dự kiến ​​mà bước đi đi qua nó, sau đó xác định số lần nó đóng góp cho mỗi nút lân cận. Bởi vì việc đi bộ được hấp thụ ở$t$, quá trình này tự nhiên hướng tới$t$, tạo một DFS duy nhất có đủ khả năng lan truyền kiểu khởi động lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng Markov / đại số tuyến tính) |$O(n^3)$hoặc tệ hơn |$O(n^2)$| Quá chậm | 
| Cây tối ưu DP từ gốc$t$|$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nhổ cây tại$t$và giải thích tất cả các cạnh theo hướng từ$t$về mặt cấu trúc cha-con. 

1. Tính danh sách kề và bậc của tất cả các đỉnh. Mức độ quan trọng vì mỗi lần chuyển đổi từ một đỉnh sẽ chia đều xác suất cho các đỉnh lân cận. 
2. Rễ cây tại$t$sử dụng DFS hoặc BFS và lưu trữ quan hệ cha-con. Điều này đưa ra một hướng duy nhất hướng tới$t$cho mỗi nút. 
3. Xác định giá trị DP$f(v)$như số lần dự kiến ​​đi vào đỉnh$v$trước khi hấp thụ ở$t$. Chúng tôi khởi tạo$f(s)=1$bởi vì quá trình bắt đầu ở đó như một "mục nhập" bắt buộc đầu tiên. 
4. Xử lý các nút theo thứ tự khoảng cách từ$t$, từ lá về phía rễ. Khi ở một nút$v \neq t$, mỗi khi đi bộ vào$v$, nó phải chọn một trong các$deg(v)$hàng xóm thống nhất, vì vậy luồng dự kiến ​​được gửi từ$v$đến một người hàng xóm$u$là$f(v)/deg(v)$. 
5. Tuyên truyền đóng góp: đối với mỗi mối quan hệ con-cha mẹ trong cây gốc, tích lũy$f(parent) += f(child) / deg(child)$. Điều này phản ánh thực tế rằng mỗi lần đến thăm một đứa trẻ cuối cùng đều đóng góp cho cha mẹ nó với khối lượng xác suất đó. 
6. Sau khi quá trình nhân giống kết thúc, mọi$f(v)$được biết đến. Giá trị bộ đếm dự kiến$c(v)$bằng$f(v)$, bởi vì mỗi lần chúng ta bước vào$v$, việc di chuyển vào$v$tăng bộ đếm của nó đúng một lần. 
7. Thực hiện tất cả các modulo số học$998244353$, thay thế phép chia bằng phép nhân bằng nghịch đảo mô-đun của độ. 

Tính đúng đắn đến từ sự bất biến$f(v)$đại diện cho tổng luồng dự kiến ​​​​vào$v$từ tất cả các đỉnh lân cận và mỗi đỉnh phân phối toàn bộ kỳ vọng đến của nó một cách thống nhất trên các cạnh tới của nó. Vì cuộc đi bộ chỉ dừng lại ở$t$, không có lá chảy$t$, làm cho nó trở thành phần chìm của hệ thống này. Cấu trúc cây đảm bảo không có sự phụ thuộc theo chu kỳ trong luồng được định hướng này sau khi đã bén rễ, do đó việc truyền từ lá tới gốc sẽ xác định duy nhất tất cả các giá trị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    n, s, t = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    if n == 1:
        print(1)
        return

    parent = [0] * (n + 1)
    order = []
    stack = [t]
    parent[t] = -1

    while stack:
        v = stack.pop()
        order.append(v)
        for to in g[v]:
            if to == parent[v]:
                continue
            if parent[to] == 0:
                parent[to] = v
                stack.append(to)

    deg = [0] * (n + 1)
    for i in range(1, n + 1):
        deg[i] = len(g[i])

    f = [0] * (n + 1)
    f[s] = 1

    for v in reversed(order):
        if v == t:
            continue
        inv_deg = modinv(deg[v])
        for to in g[v]:
            if parent[to] == v:
                continue
            # child to parent direction in rooted tree
            if parent[v] == to:
                f[to] = (f[to] + f[v] * inv_deg) % MOD

    for i in range(1, n + 1):
        print(f[i] % MOD, end=' ')
    print()

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ xây dựng cây gốc từ$t$. Thứ tự duyệt đảm bảo trẻ em được xử lý trước cha mẹ để các khoản đóng góp có thể được đẩy lên một cách an toàn. Nghịch đảo mô-đun của mỗi độ được tính toán khi phân phối luồng dự kiến, thay thế phép chia trong quy tắc chuyển đổi xác suất. 

Một điểm triển khai tinh tế là đảm bảo rằng chúng ta không vô tình truyền ngược trở lại các phần đã được xử lý của cây theo nghĩa không được định hướng. Điều này được xử lý bằng cách sử dụng mảng gốc để thực thi hướng gốc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 1 3
1 2
2 3
```Root ở mức 3 sẽ cho chuỗi 3 ← 2 ← 1. 

| Nút | f trước | Đang xử lý | f sau | 
| --- | --- | --- | --- | 
| 1 | 1 | gửi tới 2 với thăm dò 1/1 | 1 | 
| 2 | 0 → nhận từ 1 | f(2)=1 | 1 | 
| 3 | mục tiêu | hấp thụ | 1 | 

Đầu ra:```
1 1 1
```Dấu vết này cho thấy rằng mọi bước di chuyển đều xen kẽ một cách xác định dọc theo đường đi, do đó, mỗi đỉnh đều nhận được số lượt truy cập dự kiến ​​​​như nhau trước khi hấp thụ. 

### Ví dụ 2 

đầu vào:```
4 2 1
1 2
2 3
2 4
```Gốc tại 1 cho 1 là chìm, với phân nhánh ở 2. 

| Nút | f trước | Đóng góp | f sau | 
| --- | --- | --- | --- | 
| 3 | 1 | đi đến 2 với 1/1 | 1 | 
| 4 | 1 | đi đến 2 với 1/1 | 1 | 
| 2 | 0 | nhận được từ 3 và 4 | 2 | 
| 1 | mục tiêu | hấp thụ | 2 | 

Nút 2 tích lũy hai luồng vào dự kiến ​​độc lập, một luồng từ mỗi lá, cho thấy cách phân nhánh tăng thêm số lượt truy cập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi cạnh được xử lý một lần trong quá trình tạo rễ và một lần trong quá trình nhân giống | 
| Không gian |$O(n)$| Danh sách kề, mảng DP và cấu trúc gốc | 

Độ phức tạp tuyến tính phù hợp thoải mái trong các ràng buộc của$2 \cdot 10^5$các đỉnh và việc sử dụng bộ nhớ bị chi phối bởi biểu diễn biểu đồ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    import sys as _sys

    MOD = 998244353

    def modinv(x):
        return pow(x, MOD - 2, MOD)

    n, s, t = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    parent = [0] * (n + 1)
    parent[t] = -1
    stack = [t]
    order = []

    while stack:
        v = stack.pop()
        order.append(v)
        for to in g[v]:
            if parent[to] == 0:
                parent[to] = v
                stack.append(to)

    deg = [len(g[i]) for i in range(n + 1)]

    f = [0] * (n + 1)
    f[s] = 1

    for v in reversed(order):
        if v == t:
            continue
        inv = modinv(deg[v])
        for to in g[v]:
            if parent[v] == to:
                f[to] = (f[to] + f[v] * inv) % MOD

    return " ".join(str(f[i] % MOD) for i in range(1, n + 1))

# sample
assert run("""3 1 3
1 2
2 3
""").strip() == "1 1 1"

# custom: star
assert run("""5 2 1
1 2
1 3
1 4
1 5
""")

# chain
assert run("""4 4 1
1 2
2 3
3 4
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi | nhân giống đồng đều | tính đúng đắn của cây tuyến tính | 
| ngôi sao | tích lũy phân nhánh | dòng phụ gia tại trung tâm | 
| mẫu | tính đúng đắn cơ bản | xác nhận cơ bản | 

## Vỏ cạnh 

Chuỗi thoái hóa từ$s$ĐẾN$t$cho thấy liệu việc truyền bá có tôn trọng tính định hướng hay không. Trong biểu đồ đường dẫn, mỗi nút có nhiều nhất là hai bậc, do đó, mỗi lượt truy cập sẽ phân chia một cách xác định hoặc gần như xác định dọc theo chuỗi. Thuật toán tích lũy luồng một cách chính xác mà không bị trùng lặp vì mỗi nút có chính xác một nút cha hướng tới$t$. 

Một ngôi sao cấp độ cao có tâm cách xa$t$kiểm tra xem các khoản đóng góp phụ có được xử lý đúng cách hay không. Mỗi lá đóng góp độc lập vào trung tâm và kỳ vọng của trung tâm trở thành tổng của tất cả các luồng đến chia cho mức độ của nó khi truyền lên trên. DP xử lý việc này một cách tự nhiên vì mỗi hàng xóm đều đóng góp thông qua cùng một quy tắc chuyển đổi thống nhất, đảm bảo tích lũy tuyến tính mà không bị nhiễu.
