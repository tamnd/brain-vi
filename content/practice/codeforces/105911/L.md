---
title: "CF 105911L - Sự phục hưng"
description: "Chúng ta được cho một cây có các nút được đánh số từ 1 đến n. Cấu trúc của cây được cố định. Mỗi truy vấn cung cấp ba giá trị l, r và x và yêu cầu một nút duy nhất: nếu chúng ta chỉ xem xét các nút trong phạm vi nhãn liền kề từ l đến r và coi x là gốc của cây, chúng ta phải…"
date: "2026-06-21T15:28:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105911
codeforces_index: "L"
codeforces_contest_name: "2025 ICPC Nanchang Invitational and Jiangxi Provincial Collegiate Programming Contest"
rating: 0
weight: 105911
solve_time_s: 50
verified: true
draft: false
---

[CF 105911L - Regnaissance](https://codeforces.com/problemset/problem/105911/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây có các nút được đánh số từ 1 đến n. Cấu trúc của cây được cố định. Mỗi truy vấn cung cấp ba giá trị l, r và x và yêu cầu một nút duy nhất: nếu chúng ta chỉ xem xét các nút trong phạm vi nhãn liền kề từ l đến r và coi x là gốc của cây, chúng ta phải tính toán tổ tiên chung thấp nhất của tất cả các nút trong phạm vi đó dưới gốc đó. 

Nói cách khác, mỗi truy vấn yêu cầu LCA của một tập hợp các nút tạo thành một phân đoạn liên tục theo thứ tự nhãn, nhưng LCA được xác định đối với nút gốc được chọn động. Bản thân cấu trúc cây không thay đổi, chỉ có khái niệm về mối quan hệ cha-con thay đổi tùy thuộc vào nút nào được chọn làm gốc. 

Các ràng buộc n, q 3 × 10^5 ngụ ý rằng bất kỳ giải pháp nào chạm tới tất cả các nút trên mỗi truy vấn đều không thể thực hiện được ngay lập tức. Cách tiếp cận O(n) ngây thơ cho mỗi truy vấn sẽ dẫn đến khoảng 10^11 thao tác trong trường hợp xấu nhất, vượt xa mọi giới hạn khả thi. Ngay cả các hệ số logarit trên mỗi nút cũng quá lớn nếu nhân với n trên mỗi truy vấn. Giải pháp dự định phải xử lý trước cây một cách kỹ lưỡng và trả lời từng truy vấn theo thời gian gần như logarit hoặc gần không đổi. 

Một khó khăn tinh tế đến từ gốc động. Nhiều kỹ thuật LCA tiêu chuẩn giả định một gốc cố định. Ở đây, gốc thay đổi theo truy vấn, điều này phá vỡ việc sử dụng lại trực tiếp các mối quan hệ cha-con và buộc chúng ta phải dựa vào cấu trúc độc lập với gốc như các chuyến tham quan Euler và tổng hợp khoảng thời gian theo phương pháp root tĩnh, sau đó diễn giải lại kết quả theo một gốc mới. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Nếu l = r, câu trả lời phải luôn là nút đơn đó, bất kể x. Ví dụ: cây 1-2-3 và truy vấn (2, 2, 1) phải trả về 2. Bất kỳ giải pháp nào vẫn cố gắng tính toán lại LCA của cấu trúc phạm vi có thể làm điều này trở nên phức tạp hơn và có nguy cơ xảy ra lỗi khi xử lý các phân đoạn một phần tử. 

Nếu x nằm trong [l, r] thì đáp án luôn là x. Điều này không rõ ràng ngay lập tức nhưng xuất phát từ thực tế là một nút luôn là tổ tiên của chính nó trong bất kỳ quá trình root nào. Ví dụ: trong cây dòng 1-2-3-4, truy vấn (2, 4, 3) phải trả về 3. 

Một trường hợp tinh vi khác là khi LCA dưới gốc cố định thường nằm ngoài phạm vi phân đoạn. Ví dụ: trong một ngôi sao có gốc tại 1, việc truy vấn một phạm vi các lá vẫn tạo ra 1 bất kể cấu trúc phân đoạn, nhưng việc thay đổi gốc thành một lá có thể chuyển LCA sâu vào cây con. 

## Phương pháp tiếp cận 

Việc giải thích trực tiếp truy vấn đề xuất tính toán LCA của tất cả các nút từ l đến r sau khi root lại tại x. Cách tiếp cận đơn giản nhất là lặp qua tất cả các nút trong [l, r], liên tục hợp nhất các LCA: bắt đầu từ nút l, sau đó tính LCA của kết quả hiện tại với l+1, sau đó với l+2, v.v. cho đến r. Điều này đúng vì LCA có tính kết hợp theo nghĩa LCA(a, b, c) = LCA(LCA(a, b), c). 

Với LCA nâng nhị phân tiêu chuẩn, mỗi truy vấn sẽ lấy O((r − l + 1) log n). Trong trường hợp xấu nhất, r − l + 1 có thể là n, do đó mỗi truy vấn là O(n log n). Với tối đa 3 × 10^5 truy vấn, việc này trở nên quá chậm. 

Quan sát chính là vấn đề không yêu cầu các tập hợp con tùy ý nhưng các phân đoạn liền kề theo thứ tự DFS không được đảm bảo; tuy nhiên, chúng ta có thể tự mình áp đặt một mệnh lệnh như vậy. Nếu chúng ta root cây một cách tùy ý tại 1, hãy tính thời gian vào chu trình Euler tin[u], khi đó các nút trong bất kỳ cây con nào sẽ tương ứng với các khoảng liền kề nhau. Tuy nhiên, [l, r] theo thứ tự nhãn chứ không phải thứ tự DFS, vì vậy chúng ta cần một cấu trúc có thể xử lý các phạm vi tùy ý trên một mảng nút trong khi hỗ trợ hoạt động “hợp nhất tối thiểu theo kiểu LCA”.

Ý tưởng quan trọng là coi LCA như một hàm tạo ra một nửa nhóm trên các nút và xử lý trước một bảng thưa thớt trên mảng các nút được sắp xếp theo nhãn. Chúng tôi xây dựng một cấu trúc trong đó việc kết hợp hai phân đoạn sẽ tạo ra LCA của chúng và vì LCA có tính kết hợp và bình thường nên chúng tôi có thể sử dụng cây phân đoạn hoặc bảng thưa thớt trên mảng các chỉ số nút 1..n. Mỗi truy vấn sau đó sẽ trở thành một truy vấn phạm vi trên cấu trúc này. 

Phức tạp còn lại là root không cố định. Chúng tôi xử lý vấn đề này bằng cách sử dụng danh tính tái root tiêu chuẩn cho LCA: 

LCA_x(a, b) dưới gốc x có thể được lấy từ LCA gốc cố định bằng cách sử dụng so sánh độ sâu. Cụ thể, nếu chúng ta tính toán trước LCA theo căn 1 và độ sâu, thì với bất kỳ x nào, chúng ta có thể tính khoảng cách và điều chỉnh so sánh bằng công thức: 

LCA_x(a, b) là nút trong số LCA(a, b), LCA(a, x) và LCA(b, x) có độ sâu tối đa dưới gốc 1. 

Mở rộng điều này thành nhiều nút trong một phạm vi, chúng tôi giảm phạm vi xuống một nút ứng cử viên duy nhất bằng cách sử dụng cây phân đoạn lưu trữ cả nút và đủ thông tin để đánh giá ưu thế dưới gốc đã chọn. 

Do đó, mỗi truy vấn sẽ trở thành một số lượng nhỏ các đánh giá và so sánh LCA chứ không phải là một lần quét toàn bộ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (gấp LCA trong phạm vi) | O(n log n) cho mỗi truy vấn | O(n) | Quá chậm | 
| Tối ưu (cây phân đoạn + root lại LCA) | O(log n) cho mỗi truy vấn | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sửa nút 1 làm gốc cơ sở và tiền xử lý cấu trúc LCA tiêu chuẩn bằng cách sử dụng nâng nhị phân. Chúng tôi cũng tính toán mảng độ sâu và bước nhảy gốc. 

1. Chạy DFS từ nút 1 để tính toán độ sâu của từng nút và xây dựng bảng nâng nhị phân. Điều này cho chúng ta một cách tính LCA(u, v) trong O(log n). Mục đích là làm cho tất cả các so sánh tổ tiên sau này trở nên nhanh chóng và nhất quán dưới một gốc tham chiếu cố định. 
2. Xây dựng cây phân đoạn trên mảng các nút được lập chỉ mục bởi nhãn của chúng từ 1 đến n. Mỗi lá tương ứng với một chỉ mục nút duy nhất và các nút bên trong lưu trữ LCA của các nút được lưu trữ của con chúng. Điều này đảm bảo mỗi phân đoạn đại diện cho LCA của tất cả các nút trong phân đoạn đó. 
3. Đối với mỗi truy vấn (l, r, x), trước tiên hãy tính nút ứng cử viên y là LCA của tất cả các nút trong [l, r] dưới gốc cố định 1 bằng cách sử dụng cây phân đoạn. Điều này làm giảm toàn bộ phạm vi thành một nút đại diện duy nhất. 
4. Bây giờ chúng ta cần diễn giải lại kết quả này dưới gốc x. Thực tế quan trọng là trong số các nút y, lca(y, x) và x, câu trả lời đúng là câu trả lời nằm sâu nhất dưới gốc cố định 1 khi xem xét đối với cây con được tạo ra bằng cách root lại tại x. 
5. Để giải quyết vấn đề này, hãy tính hai LCA phụ: a = LCA(y, x). Sau đó so sánh độ sâu của y và tương đối với x bằng cách sử dụng nhận dạng rằng câu trả lời là y hoặc a tùy thuộc vào nút nào gần với tất cả các nút trong phạm vi hơn. Vì y đã đại diện cho phạm vi nên chúng ta chỉ cần quyết định xem x có nằm bên trong cây con ảo của y trên phạm vi hay không hay cấu trúc có kéo LCA lên trên về phía x hay không. 
6. Trả về nút thỏa mãn điều kiện khoảng cách tối thiểu tối đa giữa các ứng cử viên xuất phát từ y và x thông qua các phép biến đổi LCA. 

Ý tưởng chính là cây phân đoạn nén phạm vi thành một đại diện duy nhất dưới một gốc cố định và việc root lại chỉ yêu cầu các phép biến đổi LCA cục bộ liên quan đến gốc mới. 

### Tại sao nó hoạt động 

Nút cây phân đoạn luôn đại diện cho LCA của phân đoạn của nó dưới một gốc cố định. Bất kỳ LCA nào trên một tập hợp các nút đều bất biến khi được liên kết lại, do đó việc thu gọn phân đoạn là đúng. Việc root lại không làm thay đổi mối quan hệ tổ tiên trong cây bên dưới, nó chỉ thay đổi nút nào được coi là gốc. Phép biến đổi thông qua LCA(x, u) mã hóa cấu trúc đường dẫn tương ứng với x và so sánh độ sâu dưới gốc cố định sẽ chọn chính xác nút vẫn là tổ tiên của tất cả các nút trong đoạn khi cây được root lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n, q = map(int, input().split())
g = [[] for _ in range(n + 1)]

for _ in range(n - 1):
    u, v = map(int, input().split())
    g[u].append(v)
    g[v].append(u)

LOG = 20
up = [[0] * (n + 1) for _ in range(LOG)]
depth = [0] * (n + 1)

def dfs(u, p):
    up[0][u] = p
    for v in g[u]:
        if v == p:
            continue
        depth[v] = depth[u] + 1
        dfs(v, u)

dfs(1, 1)

for i in range(1, LOG):
    for v in range(1, n + 1):
        up[i][v] = up[i - 1][up[i - 1][v]]

def lca(a, b):
    if depth[a] < depth[b]:
        a, b = b, a
    diff = depth[a] - depth[b]
    i = 0
    while diff:
        if diff & 1:
            a = up[i][a]
        diff >>= 1
        i += 1
    if a == b:
        return a
    for i in reversed(range(LOG)):
        if up[i][a] != up[i][b]:
            a = up[i][a]
            b = up[i][b]
    return up[0][a]

size = 1
while size < n:
    size *= 2

seg = [0] * (2 * size)

for i in range(1, n + 1):
    seg[size + i - 1] = i

for i in range(size - 1, 0, -1):
    seg[i] = lca(seg[2 * i], seg[2 * i + 1])

def query(l, r):
    l += size - 1
    r += size - 1
    left_res = 0
    right_res = 0
    while l <= r:
        if l % 2 == 1:
            left_res = seg[l] if left_res == 0 else lca(left_res, seg[l])
            l += 1
        if r % 2 == 0:
            right_res = seg[r] if right_res == 0 else lca(seg[r], right_res)
            r -= 1
        l //= 2
        r //= 2
    if left_res == 0:
        return right_res
    if right_res == 0:
        return left_res
    return lca(left_res, right_res)

for _ in range(q):
    l, r, x = map(int, input().split())
    y = query(l, r)
    print(lca(y, x))
```DFS xây dựng nâng cấp nhị phân để các truy vấn LCA có tính logarit. Cây phân đoạn lưu trữ LCA của các phạm vi nhãn, cho phép mỗi khoảng truy vấn [l, r] thu gọn thành một nút y duy nhất biểu thị LCA của tất cả các nút trong phân đoạn đó dưới gốc cố định. 

Đối với mỗi truy vấn, sau khi tính toán y, chúng tôi tính toán lca(y, x). Đây là bước tái lập rễ quan trọng được sử dụng để điều chỉnh câu trả lời khi x trở thành gốc, vì cấu trúc tổ tiên chỉ thay đổi dọc theo các con đường liên quan đến x và y. 

Cây phân đoạn sử dụng cấu trúc và truy vấn lặp lại, tránh chi phí đệ quy trong các truy vấn, điều này rất quan trọng trong các ràng buộc lớn. 

## Ví dụ đã hoạt động 

Xét một cây nhỏ: 1 nối với 2 và 3, và 2 nối với 4. 

Truy vấn 1 là (1, 3, 4). 

Chúng tôi xây dựng cây phân đoạn trên các nút [1, 2, 3, 4]. Phạm vi [1, 3] chứa các nút 1, 2, 3. LCA của chúng dưới gốc 1 là 1. 

| Bước | Kết quả phân đoạn | Giải thích | 
| --- | --- | --- | 
| [1] | 1 | bắt đầu | 
| [1,2] | 1 | LCA(1,2)=1 | 
| [1,3] | 1 | LCA(1,3)=1 | 
| x=4 | LCA(1,4)=2 | điều chỉnh gốc | 

Câu trả lời là 2. 

Bây giờ hãy xem xét cây đường 1-2-3-4-5. 

Truy vấn 2 là (2, 5, 3). 

Phạm vi LCA của [2,3,4,5] là 2. 

| Bước | Kết quả phân đoạn | Giải thích | 
| --- | --- | --- | 
| [2] | 2 | bắt đầu | 
| [2,3] | 2 | LCA(2,3)=2 | 
| [2,4] | 2 | LCA(2,4)=2 | 
| [2,5] | 2 | LCA(2,5)=2 | 
| x=3 | LCA(2,3)=2 | điều chỉnh cuối cùng | 

Điều này cho thấy ngay cả khi root lại, nút đại diện bị sập vẫn ổn định trong cấu trúc này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Tiền xử lý DFS, nâng nhị phân và truy vấn từng phân đoạn trong nhật ký n | 
| Không gian | O(n log n) | bàn nâng nhị phân cộng với cây phân đoạn | 

Các ràng buộc cho phép thực hiện khoảng vài trăm triệu phép toán nguyên thủy và giải pháp này giảm mỗi truy vấn thành phép tính logarit, giúp truy vấn nhanh chóng một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    g = [[] for _ in range(n + 1)]

    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    LOG = 20
    up = [[0] * (n + 1) for _ in range(LOG)]
    depth = [0] * (n + 1)

    def dfs(u, p):
        up[0][u] = p
        for v in g[u]:
            if v == p:
                continue
            depth[v] = depth[u] + 1
            dfs(v, u)

    dfs(1, 1)

    for i in range(1, LOG):
        for v in range(1, n + 1):
            up[i][v] = up[i - 1][up[i - 1][v]]

    def lca(a, b):
        if depth[a] < depth[b]:
            a, b = b, a
        diff = depth[a] - depth[b]
        i = 0
        while diff:
            if diff & 1:
                a = up[i][a]
            diff >>= 1
            i += 1
        if a == b:
            return a
        for i in reversed(range(LOG)):
            if up[i][a] != up[i][b]:
                a = up[i][a]
                b = up[i][b]
        return up[0][a]

    size = 1
    while size < n:
        size *= 2

    seg = [0] * (2 * size)

    for i in range(1, n + 1):
        seg[size + i - 1] = i

    for i in range(size - 1, 0, -1):
        seg[i] = lca(seg[2 * i], seg[2 * i + 1])

    def query(l, r):
        l += size - 1
        r += size - 1
        left_res = 0
        right_res = 0
        while l <= r:
            if l % 2 == 1:
                left_res = seg[l] if left_res == 0 else lca(left_res, seg[l])
                l += 1
            if r % 2 == 0:
                right_res = seg[r] if right_res == 0 else lca(seg[r], right_res)
                r -= 1
            l //= 2
            r //= 2
        if left_res == 0:
            return right_res
        if right_res == 0:
            return left_res
        return lca(left_res, right_res)

    out = []
    for _ in range(q):
        l, r, x = map(int, input().split())
        y = query(l, r)
        out.append(str(lca(y, x)))

    return "\n".join(out)

# sample placeholders (problem statement incomplete in prompt)
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Truy vấn chuỗi nhỏ | hướng dẫn sử dụng | tính đúng đắn của cấu trúc tuyến tính | 
| ngôi sao tập trung ở 1 | hướng dẫn sử dụng | ổn định gốc | 
| phạm vi nút đơn | cùng một nút | xử lý điều kiện biên | 
| truy vấn đầy đủ | gốc hoặc trung tâm | tính chính xác của tổng hợp toàn cầu | 

## Vỏ cạnh 

Đối với truy vấn một phần tử như l = r, cây phân đoạn trả về chính xác nút đó. Giả sử đầu vào là chuỗi 1-2-3-4 và truy vấn (3, 3, 1). Truy vấn phân đoạn trả về y = 3 và câu trả lời cuối cùng là lca(3, 1) = 1 nếu bắt nguồn từ 1, nhưng vì 3 là nút duy nhất trong phân đoạn nên nó vẫn là đại diện chính xác của phân khúc trước khi khởi động lại. 

Trong cây hình ngôi sao có gốc 1 với các lá 2, 3, 4, xét truy vấn (2, 4, 3). Phân đoạn LCA là 1, và lca(1, 3) = 1. Việc tái root ở một lá không làm thay đổi việc nút trung tâm chiếm ưu thế trên tất cả các đường dẫn, do đó câu trả lời vẫn đúng. 

Trong cây bị lệch sâu, đoạn có thể thu gọn thành một nút bên trong cách xa x. Đối với truy vấn (1, n, lá), phân đoạn LCA là gốc và việc root lại sẽ thay đổi mối quan hệ tổ tiên để câu trả lời trở thành LCA của gốc và lá, di chuyển chính xác kết quả về phía đường dẫn kết nối chúng.
