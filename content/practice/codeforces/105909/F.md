---
title: "CF 105909F - \u4e0d\u6b7b\u56fd\u7684\u751f\u547d\u6811"
description: "Chúng ta được cấp một cây có gốc với các giá trị nút. Mỗi nút mang một nhãn số nguyên và nút 1 là nút gốc. Một truy vấn chọn hai nút $s$ và $t$, trong đó $t$ được đảm bảo là nút tổ tiên của $s$, do đó đường đi giữa chúng là đường đi lên duy nhất từ ​​$s$ đến $t$."
date: "2026-06-25T14:07:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105909
codeforces_index: "F"
codeforces_contest_name: "The 9th Hebei Collegiate Programming Contest"
rating: 0
weight: 105909
solve_time_s: 51
verified: true
draft: false
---

[CF 105909F - \u4e0d\u6b7b\u56fd\u7684\u751f\u547d\u6811](https://codeforces.com/problemset/problem/105909/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có gốc với các giá trị nút. Mỗi nút mang một nhãn số nguyên và nút 1 là nút gốc. Một truy vấn chọn hai nút$s$Và$t$, Ở đâu$t$được đảm bảo là tổ tiên của$s$, do đó đường đi giữa chúng là đường đi duy nhất đi lên từ$s$ĐẾN$t$. 

Trên đường đi lên đó, chúng ta tưởng tượng các nút đi qua bắt đầu từ$s$. Lúc đầu chúng ta “giữ” giá trị của$s$. Chúng tôi được phép mở rộng những gì chúng tôi có bằng cách chọn các nút bổ sung dọc theo đường dẫn, nhưng chỉ theo một quy tắc nghiêm ngặt: chúng tôi có thể lấy một nút có giá trị là$x$chỉ khi chúng ta đã có một nút có giá trị$x-1$. Mỗi giá trị có thể xuất hiện nhiều nhất một lần trong tập hợp được thu thập. 

Vì vậy dọc theo con đường từ$s$ĐẾN$t$, chúng tôi đang cố gắng trích xuất một chuỗi các nút có giá trị tạo thành một chuỗi như$$a[s], a[s] + 1, a[s] + 2, \dots$$và các nút phải xuất hiện theo thứ tự hướng lên dọc theo đường đi của cây. Mục tiêu cho mỗi truy vấn là độ dài tối đa có thể có của chuỗi giá trị liên tiếp bắt đầu tại$s$và hoàn toàn ở trên con đường hướng tới$t$. 

Đầu vào mô tả một cây, các giá trị trên các nút và nhiều truy vấn có dạng này. Mỗi truy vấn yêu cầu độ dài của “chuỗi giá trị liên tiếp” dài nhất mà chúng tôi có thể xây dựng khi chuyển từ$s$hướng lên trên nhưng không vượt qua phía trên$t$. 

Từ quan điểm phức tạp, kích thước cây đủ lớn để bất kỳ giải pháp nào thực hiện truyền tải đường dẫn theo truy vấn đều quá chậm. Một đường dẫn duy nhất có thể$O(n)$và có thể có nhiều truy vấn, vì vậy cách tiếp cận đơn giản sẽ dẫn đến$O(nq)$, điều đó là không thể chấp nhận được. Chúng tôi buộc phải tính toán trước mối quan hệ giữa các giá trị và tổ tiên để mỗi truy vấn có thể được trả lời theo thời gian logarit hoặc gần như không đổi. 

Một số trường hợp đặc biệt quan trọng đối với tính chính xác: 

Một trường hợp góc là khi không thể mở rộng được nữa. Ví dụ: nếu đường dẫn là$s \rightarrow t$và không có tổ tiên của$s$có giá trị$a[s]+1$, thì câu trả lời luôn là 1. Quá trình quét tham lam ngây thơ có thể bỏ qua các nút một cách không chính xác hoặc cố gắng "nhảy" các giá trị không tồn tại trên đường dẫn. 

Một trường hợp tinh tế khác là khi chuỗi tồn tại nhưng mở rộng lên trên$t$. Giả sử chuỗi đi lên đầy đủ từ$s$sẽ tiếp tục vượt ra ngoài$t$, nhưng chúng tôi không được phép vượt qua$t$. Một giải pháp đúng phải cắt bớt chuỗi chính xác tại$t$, không tính các nút ở trên nó. 

Cuối cùng, nhiều lần xuất hiện của cùng một giá trị trong các nhánh khác nhau không thành vấn đề, vì chỉ tổ tiên trên đường dẫn cụ thể mới có liên quan. Bất kỳ giải pháp nào bỏ qua tổ tiên và chỉ theo dõi giá trị liền kề toàn cầu sẽ bị tính quá mức không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý từng truy vấn một cách độc lập bằng cách đi lên từ$s$cho đến khi đạt được$t$, thu thập các giá trị thành một mảng rồi tính toán tiền tố dài nhất tạo thành chuỗi tăng dần liên tiếp bắt đầu từ$a[s]$. Điều này hiệu quả vì đường dẫn được biết rõ ràng và chúng tôi có thể kiểm tra xem từng giá trị tiếp theo có tồn tại theo thứ tự hay không. 

Tuy nhiên, mỗi truy vấn có thể yêu cầu đi qua một đường dẫn có độ dài$O(n)$. Với nhiều truy vấn, điều này dẫn đến độ phức tạp trong trường hợp xấu nhất$O(nq)$, tốc độ này quá chậm khi cả hai$n$Và$q$lớn. 

Quan sát quan trọng là cấu trúc không thực sự là về các chuỗi con tùy ý, mà là về “các bước nhảy giá trị” xác định. Từ bất kỳ nút nào$u$, chỉ có nhiều nhất một bước hữu ích tiếp theo: tổ tiên gần nhất của$u$giá trị của nó là$a[u] + 1$. Nếu chúng ta biết tổ tiên đó một cách hiệu quả, chúng ta có thể “nén” toàn bộ đường dẫn thành một chuỗi chức năng trong đó mỗi nút có nhiều nhất một nút kế tiếp theo nghĩa chuỗi giá trị. 

Điều này biến bài toán cây thành một thứ gì đó gần giống với đồ thị hàm số hơn: mọi nút đều trỏ đến nút hợp lệ tiếp theo của nó trong chuỗi giá trị liên tiếp dọc theo đường dẫn gốc. Khi cấu trúc con trỏ này được xây dựng, việc trả lời một truy vấn sẽ mô phỏng khoảng cách chúng ta có thể theo dõi các con trỏ này lên trên, dừng lại khi chúng ta vượt qua phía trên$t$. 

The main difficulty is building these next pointers efficiently. This is solved by maintaining, during a DFS from the root, a stack (or map of stacks) of nodes by value along the current root-to-node path. Khi chúng tôi đến một nút$u$, chúng ta có thể xác định ngay tổ tiên gần nhất có giá trị$a[u]+1$, bởi vì ngăn xếp của giá trị đó cho chúng ta biết chính xác nút nào hiện đang hoạt động trên đường dẫn. 

Sau khi xây dựng “cây con trỏ tiếp theo” này, chúng ta có thể tính toán trước việc nâng nhị phân trên nó để mỗi truy vấn có thể nhảy theo các bước logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Truyền tải đường dẫn Brute Force cho mỗi truy vấn |$O(n)$mỗi truy vấn |$O(1)$| Quá chậm | 
| DFS + con trỏ tiếp theo + nâng nhị phân |$O((n + q)\log n)$|$O(n\log n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi xây dựng một cấu trúc cho chúng tôi biết, đối với mỗi nút, bước tiếp theo trong chuỗi giá trị liên tiếp của nó nằm ở đâu. 

1. Chạy DFS từ gốc trong khi vẫn duy trì một từ điển ánh xạ từng giá trị tới một chồng nút hiện có trên đường dẫn từ nút gốc đến nút hiện tại. 

Khi vào một nút$u$, chúng ta đẩy nó vào ngăn xếp tương ứng với$a[u]$. Điều này đảm bảo rằng tại bất kỳ thời điểm nào, đỉnh ngăn xếp của một giá trị là tổ tiên gần nhất (bao gồm cả chính nó) với giá trị đó. 
2. Đối với nút hiện tại$u$, chúng tôi muốn tìm người kế nhiệm của nó trong chuỗi, là tổ tiên gần nhất có giá trị$a[u] + 1$. Chúng ta nhìn vào đống$a[u] + 1$. Nếu nó không trống thì phần tử trên cùng của nó chính xác là phần tử tổ tiên gần nhất. Chúng tôi xác định:$$nxt[u] = \text{top of stack}[a[u] + 1]$$Nếu không có ngăn xếp như vậy tồn tại,$nxt[u] = 0$. 

Bước này nén “giá trị liền kề dọc theo tổ tiên” thành một con trỏ duy nhất trên mỗi nút. 
3. Sau khi xử lý con của$u$, chúng tôi bật$u$từ ngăn xếp giá trị của nó để khôi phục trạng thái chính xác cho các nhánh khác. Điều này giữ cho các ngăn xếp nhất quán với đường dẫn DFS hiện tại. 
4. Một lần tất cả$nxt[u]$con trỏ được xây dựng, chúng tôi coi chúng như một đồ thị hàm số trong đó mỗi nút có nhiều nhất một cạnh đi ra. Chúng tôi xây dựng các bàn nâng nhị phân để có thể nhảy$2^k$bước dọc theo chuỗi này một cách nhanh chóng. 
5. Đối với mỗi truy vấn$(s, t)$, chúng tôi liên tục nhảy dọc theo$nxt$con trỏ bắt đầu từ$s$, nhưng chúng ta phải đảm bảo rằng chúng ta không bao giờ di chuyển lên trên$t$trong cây. Từ$t$là tổ tiên của$s$, điều kiện này tương đương với việc yêu cầu bất kỳ nút truy cập nào có độ sâu ít nhất$\text{depth}[t]$. 

Chúng ta tham lam thực hiện bước nhảy nhị phân lớn nhất có thể để giữ chúng ta trong giới hạn độ sâu hợp lệ, tích lũy số bước đã thực hiện. 

### Tại sao nó hoạt động 

Tại bất kỳ nút nào$u$, định nghĩa của$nxt[u]$đảm bảo rằng nếu một chuỗi hợp lệ tồn tại, nó phải sử dụng chính xác giá trị xuất hiện tiếp theo$a[u] + 1$. Bất kỳ tổ tiên nào khác có cùng giá trị sẽ cao hơn và phá vỡ yêu cầu gần nhất theo thứ tự đường dẫn, ngăn các giá trị trung gian hợp lệ tồn tại giữa chúng. 

Điều này làm cho chuỗi có tính xác định: bắt đầu từ$s$, có chính xác một đường dẫn có giá trị liên tiếp tối đa có thể đi lên theo các lựa chọn nút. Nâng cấp nhị phân duy trì tính quyết định này trong khi tăng tốc quá trình truyền tải. Ràng buộc độ sâu thực thi ranh giới tại$t$, đảm bảo chúng tôi chỉ xem xét các tiền tố của chuỗi đó nằm bên trong phân đoạn truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    n, q = map(int, input().split())
    a = [0] + list(map(int, input().split()))

    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    parent = [0] * (n + 1)
    depth = [0] * (n + 1)

    nxt = [0] * (n + 1)

    from collections import defaultdict
    st = defaultdict(list)

    def dfs(u, p):
        parent[u] = p
        for v in g[u]:
            if v == p:
                continue
            depth[v] = depth[u] + 1
            dfs(v, u)

    dfs(1, 0)

    order = []

    def build(u, p):
        order.append(u)
        st[a[u]].append(u)

        val = a[u] + 1
        if st[val]:
            nxt[u] = st[val][-1]

        for v in g[u]:
            if v == p:
                continue
            build(v, u)

        st[a[u]].pop()

    build(1, 0)

    LOG = 20
    up = [[0] * (n + 1) for _ in range(LOG)]
    for i in range(1, n + 1):
        up[0][i] = nxt[i]

    for k in range(1, LOG):
        for i in range(1, n + 1):
            up[k][i] = up[k - 1][up[k - 1][i]]

    def query(s, t):
        limit = depth[t]
        cur = s
        ans = 1

        for k in reversed(range(LOG)):
            nx = up[k][cur]
            if nx and depth[nx] >= limit:
                cur = nx
                ans += 1 << k

        return ans

    for _ in range(q):
        s, t = map(int, input().split())
        print(query(s, t))

if __name__ == "__main__":
    solve()
```Giải pháp được chia thành hai giai đoạn. Giai đoạn DFS xây dựng độ sâu và cấu trúc gốc, sau đó DFS thứ hai xây dựng`nxt`con trỏ sử dụng ngăn xếp giá trị hoạt động. Bàn nâng nhị phân`up[k][u]`cho phép nhảy nhanh dọc theo chuỗi giá trị. 

Một điểm tinh tế là kiểm tra độ sâu bên trong các truy vấn. Không thực thi`depth[nx] >= depth[t]`, thuật toán sẽ vui vẻ nhảy lên trên điểm cuối được phép và đếm quá mức. Một chi tiết nữa đó là`nxt[u]`được coi như một cạnh chức năng, do đó việc nâng nhị phân là an toàn vì mỗi nút có nhiều nhất một chuyển đổi đi ra. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây nhỏ có giá trị tăng dọc theo một nhánh: 

đầu vào:```
1 2
5
2 1
3 2
```| Bước | Nút hiện tại | Giá trị | nxt | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 5 | 2 | bắt đầu | 
| 2 | 3 | 5 | 2 | không có 6 tồn tại | 

Điều này cho thấy một chuỗi tầm thường có độ dài 1. 

Bây giờ hãy xem xét một chuỗi nơi các giá trị được căn chỉnh: 

đầu vào:```
1 4
1 2 3 4
1 2
2 3
3 4
1 4
```| Bước | cur | giá trị | nxt | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | 1 | 
| 2 | 2 | 2 | 3 | 2 | 
| 3 | 3 | 3 | 4 | 3 | 
| 4 | 4 | 4 | 0 | 4 | 

Điều này chứng tỏ rằng chuỗi tuân theo các giá trị liên tiếp chính xác và dừng lại khi không tồn tại giá trị tiếp theo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + q)\log n)$| DFS xây dựng con trỏ theo thời gian tuyến tính, mỗi truy vấn sử dụng nâng nhị phân qua cấu trúc nhảy sâu log | 
| Không gian |$O(n\log n)$| bàn nâng nhị phân cộng với ngăn xếp liền kề và phụ trợ | 

Độ phức tạp phù hợp thoải mái trong các ràng buộc trong đó cả nút và truy vấn đều lớn, vì mỗi truy vấn giảm xuống một số lượng nhỏ các bước nhảy logarit thay vì đi theo một đường dẫn đầy đủ trên cây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    out = io.StringIO()
    _stdout = _sys.stdout
    _sys.stdout = out
    solve()
    _sys.stdout = _stdout
    return out.getvalue().strip()

# minimal chain
assert run("""1 1
5
""") == "1"

# simple linear chain
assert run("""1 4
1 2 3 4
1 2
2 3
3 4
1 4
""") == "4"

# broken chain
assert run("""1 3
10 1 2
1 2
2 3
1 3
""") == "1"

# branching tree, chain only along one path
assert run("""1 5
1 2 3 100 4
1 2
1 3
3 4
4 5
3 1
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | trường hợp tối thiểu | 
| chuỗi 1-2-3-4 | 4 | đường dẫn đầy đủ liên tiếp | 
| không có phần mở rộng liên tiếp | 1 | chấm dứt sớm | 
| nhiễu nhánh | 2 | chỉ có con đường tổ tiên mới quan trọng | 

## Vỏ cạnh 

Khi không có phần mở rộng hợp lệ nào tồn tại, chẳng hạn như một nút có giá trị bị cô lập trên đường dẫn của nó, DFS vẫn gán`nxt[u] = 0`. Trong một truy vấn, việc nâng cấp nhị phân ngay lập tức dừng lại do không có chuyển đổi đi lên và câu trả lời vẫn là 1. 

Khi chuỗi liên tiếp đầy đủ vượt ra ngoài ranh giới truy vấn$t$, điều kiện độ sâu chặn bước nhảy vượt lên trên$t$. Thuật toán chỉ đếm các nút có độ sâu ít nhất`depth[t]`, do đó kết quả sẽ tự động bị cắt bớt thành tiền tố hợp lệ của chuỗi. 

Khi nhiều nút chia sẻ cùng một giá trị trong các nhánh khác nhau, chỉ có ngăn xếp đường dẫn DFS hiện tại được hiển thị, do đó`nxt[u]`luôn đề cập đến tổ tiên chính xác, không bao giờ là nút từ cây con khác.
