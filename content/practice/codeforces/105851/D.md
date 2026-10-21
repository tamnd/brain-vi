---
title: "CF 105851D - \u6700\u8fd1\u516c\u5171\u7956\u5148"
description: "Nhiệm vụ là duy trì và truy vấn các mối quan hệ tổ tiên trong cây có gốc. Chúng tôi được cung cấp một biểu đồ tuần hoàn được kết nối và chúng tôi hiểu một nút là gốc của cấu trúc."
date: "2026-06-22T01:59:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105851
codeforces_index: "D"
codeforces_contest_name: "2025\u5e74\u5317\u4eac\u5e02\u5927\u5b66\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\u66a8\u201c\u5c0f\u7c73\u676f\u201d\u5168\u56fd\u9080\u8bf7\u8d5b"
rating: 0
weight: 105851
solve_time_s: 50
verified: true
draft: false
---

[CF 105851D - \u6700\u8fd1\u516c\u5171\u7956\u5148](https://codeforces.com/problemset/problem/105851/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là duy trì và truy vấn các mối quan hệ tổ tiên trong cây có gốc. Chúng tôi được cung cấp một biểu đồ tuần hoàn được kết nối và chúng tôi hiểu một nút là gốc của cấu trúc. Mỗi truy vấn yêu cầu tổ tiên chung thấp nhất của hai nút, nghĩa là nút sâu nhất trong cây gốc nằm trên cả hai đường dẫn từ gốc đến nút. 

Nói một cách cụ thể hơn, hãy tưởng tượng mỗi nút là một điểm trong hệ thống phân cấp. Đối với hai nút bất kỳ, đường dẫn tới nút gốc của chúng cuối cùng sẽ hợp nhất. Nút đầu tiên nơi đường đi của chúng gặp nhau khi di chuyển lên trên là câu trả lời cho truy vấn. 

Đầu vào có thể được hiểu là một cây được mô tả bởi các cạnh của nó, theo sau là nhiều truy vấn. Mỗi truy vấn cung cấp hai nút và đầu ra là một nút duy nhất đại diện cho tổ tiên chung gần nhất của chúng. 

Ý nghĩa hạn chế chính trong loại vấn đề này là cây có thể lớn, thường lên tới 10^5 nút và các truy vấn có độ lớn tương tự. Một giải pháp tính toán lại các đường đi lên cho mỗi truy vấn sẽ liên tục đi qua các chuỗi dài, dẫn đến hành vi bậc hai trong trường hợp xấu nhất. Với 10^5 truy vấn, ngay cả việc truyền tải tuyến tính cho mỗi truy vấn cũng trở nên quá chậm, vì vậy chúng tôi cần một phương pháp tiền xử lý giúp giảm từng truy vấn về thời gian logarit hoặc không đổi. 

Một số trường hợp đặc biệt quan trọng trong cài đặt này. Khi một nút được truy vấn là nút tổ tiên của nút kia, câu trả lời về cơ bản chính là nút tổ tiên. Ví dụ: trong chuỗi 1-2-3-4 bắt nguồn từ 1, truy vấn (2, 4) sẽ trả về 2. Việc triển khai bước đi lên đơn giản có thể dễ dàng bỏ lỡ điều này nếu giả định rằng cả hai nút phải được nâng lên như nhau trước khi so sánh. 

Một trường hợp cạnh khác là cây bị lệch, trong đó cấu trúc thoái hóa thành danh sách liên kết. Ví dụ: 1-2-3-4-5-6. Nếu không xử lý trước, việc truyền tải đi lên lặp đi lặp lại sẽ trở thành tuyến tính trong trường hợp xấu nhất cho mỗi truy vấn, điều này phá vỡ sự đảm bảo về hiệu suất ngay cả khi tính chính xác vẫn được giữ nguyên. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp xử lý từng truy vấn một cách độc lập bằng cách đi từ cả hai nút trở lên về phía gốc cho đến khi đường dẫn của chúng giao nhau. Điều này có thể được thực hiện bằng cách di chuyển liên tục một nút đến nút gốc của nó cho đến khi cả hai nút khớp với độ sâu và sau đó di chuyển cả hai lên trên cùng nhau cho đến khi chúng trùng nhau. Điều này hiệu quả vì trong một cây, các đường dẫn đến gốc là duy nhất, do đó nút chia sẻ đầu tiên gặp phải là nút tổ tiên chung thấp nhất. 

Tính chính xác thì đơn giản nhưng tính kém hiệu quả lại xuất phát từ việc truyền tải lặp đi lặp lại. Mỗi truy vấn có thể yêu cầu các bước O(n) trong cây suy biến và với nhiều truy vấn, điều này dẫn đến O(nq), quá chậm khi cả n và q đều lớn. 

Quan sát quan trọng là các truy vấn tổ tiên lặp lại trên cùng một cấu trúc. Thay vì tính toán lại các bước nhảy lên trên từng bước, chúng ta có thể tính toán trước các bước nhảy có lũy thừa kích thước là 2 cho mỗi nút. Điều này biến chuyển động đi lên thành bước nhảy logarit. Khi mỗi nút biết tổ tiên thứ 1, 2, 4, 8, v.v., chúng ta có thể nâng các nút lên cùng độ sâu trong O (log n), sau đó đồng thời nâng chúng cho đến khi tổ tiên của chúng khớp với nhau. 

Kỹ thuật này thường được gọi là nâng nhị phân. Cây được xử lý trước một lần bằng tìm kiếm theo chiều sâu hoặc BFS để tính toán cha mẹ ngay lập tức, sau đó lập trình động sẽ xây dựng tổ tiên cao hơn. Mỗi truy vấn sau đó được giải quyết bằng cách so sánh tổ tiên từ quyền lực cao nhất trở xuống. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đi lên | O(n) mỗi truy vấn | O(n) | Quá chậm | 
| Nâng nhị phân | O(log n) cho mỗi truy vấn, tiền xử lý O(n log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giả sử cây có gốc ở nút 1 trừ khi có quy định khác.

1. Xây dựng danh sách kề từ danh sách cạnh để có thể duyệt cây một cách hiệu quả. Bước này chuẩn bị cấu trúc để truyền tải mà không cần quét cạnh lặp lại. 
2. Chạy tìm kiếm theo chiều sâu hoặc tìm kiếm theo chiều rộng từ gốc để tính toán độ sâu và nút cha trực tiếp của mỗi nút. Độ sâu là cần thiết để căn chỉnh các nút trước khi so sánh tổ tiên. 
3. Tính toán trước một bảng trong đó`up[v][k]`đại diện cho tổ tiên thứ 2^k của nút v. Cột đầu tiên đã được biết từ quá trình truyền tải và các giá trị cao hơn được điền bằng cách sử dụng phép lặp`up[v][k] = up[up[v][k-1]][k-1]`. Điều này hiệu quả vì bước nhảy 2^k có thể được chia thành hai lần nhảy 2^{k-1}. 
4. Đối với mỗi truy vấn (u, v), trước tiên hãy đảm bảo u là nút sâu hơn. Nếu không, hãy trao đổi chúng. Điều này đảm bảo rằng logic nâng là nhất quán. 
5. Nâng nút sâu hơn lên trên cho đến khi cả hai nút đều có cùng độ sâu. Điều này được thực hiện bằng cách kiểm tra các bit có độ chênh lệch độ sâu và áp dụng các bước nhảy tương ứng. 
6. Nếu sau khi căn chỉnh các nút bằng nhau thì nút đó là LCA và chúng ta có thể trả về ngay. Điều này xử lý trường hợp một nút là tổ tiên của nút kia. 
7. Nếu không, hãy di chuyển đồng thời cả hai nút lên trên từ lũy thừa cao nhất của hai xuống 0. Bất cứ khi nào tổ tiên 2^k của chúng khác nhau, hãy nâng cả hai nút lên tổ tiên đó. Điều này đảm bảo chúng tôi không vượt quá LCA. 
8. Sau quá trình này, cả hai nút đều ở ngay dưới tổ tiên chung thấp nhất của chúng, do đó việc trả về một trong hai nút cha sẽ đưa ra câu trả lời đúng. 

Tính chính xác dựa trên việc duy trì tính bất biến khóa: sau bước 5, cả hai nút đều ở cùng độ sâu và sau bước 7, chúng vẫn ở các cây con khác nhau của LCA nhưng có chung chuỗi cha mẹ trực tiếp phía trên chúng. Bảng nâng nhị phân đảm bảo rằng chúng ta không bao giờ bỏ qua tổ tiên thực sự vì các bước nhảy chỉ được thực hiện khi tổ tiên khác nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LOG = 20

def solve():
    n, q = map(int, input().split())
    g = [[] for _ in range(n + 1)]

    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    up = [[0] * LOG for _ in range(n + 1)]
    depth = [0] * (n + 1)

    sys.setrecursionlimit(10**7)

    def dfs(v, p):
        up[v][0] = p
        for k in range(1, LOG):
            up[v][k] = up[up[v][k - 1]][k - 1]

        for to in g[v]:
            if to == p:
                continue
            depth[to] = depth[v] + 1
            dfs(to, v)

    dfs(1, 0)

    def lift(v, d):
        for k in range(LOG):
            if d & (1 << k):
                v = up[v][k]
        return v

    def lca(a, b):
        if depth[a] < depth[b]:
            a, b = b, a

        a = lift(a, depth[a] - depth[b])

        if a == b:
            return a

        for k in reversed(range(LOG)):
            if up[a][k] != up[b][k]:
                a = up[a][k]
                b = up[b][k]

        return up[a][0]

    out = []
    for _ in range(q):
        a, b = map(int, input().split())
        out.append(str(lca(a, b)))

    print("\n".join(out))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from io import StringIO
    old_stdout = sys.stdout
    sys.stdout = StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

if __name__ == "__main__":
    solve()
```DFS xây dựng cả cấp độ sâu và cấp độ gốc ngay lập tức trong một lần truyền tải và đồng thời lấp đầy bảng nâng nhị phân cho mỗi nút. các`lift`Hàm áp dụng các bước nhảy nhị phân dựa trên biểu diễn bit của chênh lệch độ cao, giúp tránh leo tuyến tính. 

các`lca`Trước tiên, hàm bình thường hóa độ sâu, sau đó kiểm tra xem một nút có trở thành tổ tiên của nút kia sau khi căn chỉnh hay không. Vòng nâng đồng bộ hướng lên cuối cùng được viết cẩn thận từ công suất cao đến công suất thấp để chúng tôi tối đa hóa số lần nhảy mà không vượt qua LCA. 

Một chi tiết triển khai tinh tế đang được khởi tạo`up[1][k]`và xử lý`0`với tư cách là cha mẹ vô giá trị. Điều này ngăn ngừa lỗi chỉ mục khi nâng vượt quá thư mục gốc. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây đơn giản có gốc ở tỷ lệ 1: 1 được kết nối với 2 và 3, và 2 được kết nối với 4 và 5. Các truy vấn yêu cầu LCA(4, 5) và LCA(4, 3). 

| Bước | một | b | Hành động | 
| --- | --- | --- | --- | 
| Ban đầu | 4 | 5 | độ sâu bằng nhau | 
| Căn chỉnh thang máy | 4 | 5 | không cần thay đổi | 
| So sánh trở lên | 2 | 2 | cả hai đều chuyển sang cha mẹ | 
| Kết quả | 2 | 2 | Tìm thấy LCA | 

Điều này cho thấy trường hợp cả hai nút đều nằm trong cùng một cây con và hội tụ tại nút cha của chúng. 

Đối với truy vấn thứ hai: 

| Bước | một | b | Hành động | 
| --- | --- | --- | --- | 
| Ban đầu | 4 | 3 | độ sâu khác nhau | 
| Căn chỉnh thang máy | 4 | 3 | 3 đã cạn | 
| So sánh trở lên | 2 | 1 | di chuyển cho đến khi không khớp | 
| Cha mẹ cuối cùng | 1 | 1 | LCA | 

Điều này chứng tỏ trường hợp LCA gần nút gốc hơn cả hai nút. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | tiền xử lý xây dựng 2^k tổ tiên, mỗi truy vấn sử dụng bước nhảy nhị phân | 
| Không gian | O(n log n) | lưu trữ bảng tổ tiên cho mỗi nút | 

Hệ số logarit giữ cho lời giải nằm trong giới hạn đối với cây lớn và số lượng truy vấn điển hình trong loại bài toán này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    import sys, io
    sys.stdin = io.StringIO(inp)
    from io import StringIO
    old_stdout = sys.stdout
    sys.stdout = StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# simple tree
assert run("""5 2
1 2
1 3
2 4
2 5
4 5
4 3
""") == "2\n1"

# chain tree
assert run("""4 2
1 2
2 3
3 4
4 3
2 4
""") == "3\n2"

# star tree
assert run("""5 3
1 2
1 3
1 4
1 5
2 3
4 5
2 5
""") == "1\n1\n1"

# identical nodes
assert run("""3 1
1 2
1 3
2 2
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây nhỏ | hỗn hợp | tính đúng đắn cơ bản | 
| chuỗi | hỗn hợp | nâng sâu | 
| ngôi sao | 1 | LCA nặng về gốc | 
| các nút giống hệt nhau | nút chính nó | trường hợp tự LCA | 

## Vỏ cạnh 

Chuỗi lệch như 1-2-3-4-5 kiểm tra sự thoái hóa cấu trúc tồi tệ nhất. Thuật toán vẫn tính toán tổ tiên trong quá trình tiền xử lý O(n log n) và mỗi truy vấn chỉ sử dụng bước nhảy logarit, do đó, ngay cả các truy vấn như LCA(5, 1) cũng giải quyết bằng cách nâng nút 5 lên trên thông qua các bước nhị phân: 5 → 3 → 1, hội tụ chính xác mà không cần chạm vào mọi nút trung gian. 

Khi một nút là tổ tiên của nút khác, chẳng hạn như LCA(2, 5) trong chuỗi 1-2-3-4-5, bước căn chỉnh chỉ nâng nút sâu hơn. Sau khi nâng lên, kiểm tra đẳng thức sẽ kích hoạt ngay lập tức, trả về 2 mà không cần vào vòng so sánh cuối cùng.
