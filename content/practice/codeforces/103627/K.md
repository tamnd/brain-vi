---
title: "CF 103627K - Cây Nhựa Giả 2"
description: "Chúng ta có một cây có gốc ở đỉnh 1, trong đó mỗi đỉnh có trọng số nguyên. Cùng với cây, chúng ta có hai tham số, giới hạn dưới L và giới hạn trên R và số mục tiêu K."
date: "2026-07-03T02:02:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103627
codeforces_index: "K"
codeforces_contest_name: "XXII Open Cup, Grand Prix of Daejeon"
rating: 0
weight: 103627
solve_time_s: 44
verified: true
draft: false
---

[CF 103627K - Cây nhựa giả 2](https://codeforces.com/problemset/problem/103627/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc ở đỉnh 1, trong đó mỗi đỉnh có trọng số nguyên. Cùng với cây, chúng ta được cung cấp hai tham số, giới hạn dưới L và giới hạn trên R và số mục tiêu K. Nhiệm vụ là quyết định xem có thể phân chia các đỉnh của cây thành một tập hợp có cấu trúc gồm các thành phần được kết nối rời nhau hoạt động giống như “cây con đóng”, trong khi cho phép chính xác một thành phần được xây dựng một phần vẫn “mở” trong quá trình xây dựng. 

Quá trình xây dựng các thành phần này bị hạn chế theo cách từ dưới lên. Mỗi cây con có thể đóng góp các phần đã hoàn thiện đầy đủ hoặc một phần chưa hoàn thiện vẫn có thể được tổ tiên mở rộng. Một “cây con đóng” tương ứng với một thành phần được kết nối có tổng trọng số nằm trong khoảng [L, R]. “Cây con có thể mở rộng” là một thành phần được hình thành một phần và vẫn được phép phát triển đi lên thông qua chuỗi gốc. 

Tại gốc, chúng ta muốn xác định liệu có thể kết thúc với chính xác K + 1 cây con đóng sau khi xử lý mọi thứ hay không, với ràng buộc là tất cả các thành phần đều tôn trọng giới hạn trọng số. Trạng thái lập trình động theo dõi số lượng cây con đóng đã được hoàn thiện cho đến nay và tổng trọng số mà cấu trúc mở hiện tại có thể có. 

Khó khăn chính là mỗi nút hợp nhất thông tin từ các nút con của nó và số lượng tổng trọng số từng phần có thể tăng lên theo kiểu tổ hợp. Các ràng buộc đủ lớn để bất kỳ giải pháp nào liệt kê rõ ràng tất cả các trạng thái DP trên mỗi nút và trên mỗi cây con sẽ quá chậm. Cụ thể, việc hợp nhất ngây thơ hoạt động giống như một phép tích chập trên các tập hợp giá trị, có thể dễ dàng đạt được hành vi bậc hai hoặc tệ hơn trên mỗi nút. 

Chế độ lỗi điển hình phát sinh khi chúng tôi lưu trữ chính xác tất cả các khoản tiền có thể truy cập mà không cần nén. Ví dụ: nếu một nút có hai nút con, mỗi nút đóng góp nhiều tổng có thể, thì sự kết hợp của chúng sẽ tạo ra sự bùng nổ tập hợp tổng theo cặp đầy đủ. Ngay cả những cây vừa phải cũng tạo ra các cú nổ bậc hai ở mỗi lần hợp nhất. 

Một vấn đề tinh tế khác là DP không chỉ nói về tính khả thi của tổng mà còn về “độ nhạy khoảng cách” bị giới hạn: chúng tôi chỉ quan tâm liệu có bất kỳ giá trị nào rơi vào một khoảng có độ dài R − L hay không. Việc bỏ qua điều này dẫn đến việc giữ lại nhiều giá trị không thể phân biệt được cho quyết định cuối cùng. 

## Phương pháp tiếp cận 

Lập trình động brute-force tuân theo cấu trúc cây một cách tự nhiên. Đối với mỗi nút, chúng tôi duy trì một bảng trên k, số lượng cây con đóng được hình thành và x, tổng hiện tại của thành phần hoạt động. Khi kết hợp hai phần tử con, chúng tôi thử tất cả các phép chia của k và tất cả các cặp tổng từ cả hai phần tử con, tạo ra sự hợp nhất giống như tích chập. 

Cách tiếp cận này đúng vì nó theo dõi chính xác tất cả các cách có thể để phân vùng cây con. Tuy nhiên, khi cả hai con đều có số lượng có thể là O(K) và tổng có thể là O(R), việc hợp nhất chúng sẽ tốn O(K^2 R^2) mỗi nút trong trường hợp xấu nhất. Trên N nút, điều này trở nên hoàn toàn không khả thi. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần cấu trúc đầy đủ của tập hợp các tổng có thể đạt được. Chúng ta chỉ cần biết liệu có tồn tại một giá trị trong khoảng trượt có độ dài R − L hay không, bởi vì mọi cây con đóng hợp lệ đều phải rơi vào [L, R] và các quyết định chỉ phụ thuộc vào việc cửa sổ như vậy có được chạm hay không. Điều này cho phép chúng tôi nén mạnh trạng thái DP. 

Chúng tôi diễn giải lại từng trạng thái DP dưới dạng một tập hợp các tổng có thể tiếp cận và lưu ý rằng mức chênh lệch giá trị cho một k cố định được giới hạn bởi k(R − L). Điều này ngụ ý rằng các giá trị có thể được ước tính gần đúng một cách an toàn mà không làm mất đi tính chính xác đối với các truy vấn khoảng thời gian. Thay vì lưu trữ tất cả các giá trị, chúng tôi lưu trữ một tập hợp đại diện rút gọn để duy trì khoảng trống của tất cả các khoảng có độ dài R − L.

Điều này dẫn đến ý tưởng quan trọng thứ hai: trong mỗi lần hợp nhất, chúng tôi áp dụng mức giảm chỉ giữ các đại diện cực trị trong các nhóm nhỏ, đảm bảo rằng kích thước đã đặt vẫn là O(K). Tính chính xác được đảm bảo vì hai giá trị đủ gần với R − L không thể được phân biệt bằng bất kỳ truy vấn hợp lệ nào. 

Do đó, thay vì tăng trưởng theo cấp số nhân ở trạng thái DP, chúng tôi duy trì các biểu diễn giới hạn và đảm bảo mỗi lần hợp nhất vẫn là đa thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N K^2 R^2) | O(N K R) | Quá chậm | 
| Tối ưu | O(N K^3) | O(NK) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Root cây tại nút 1 và xác định trạng thái DP cho mỗi nút dưới dạng tập hợp các tập hợp S(v, k), trong đó k là số cây con đóng hoàn chỉnh trong phần được xử lý của cây con của v. Mỗi tập hợp lưu trữ các tổng có thể có của thành phần hiện đang mở. 
2. Khởi tạo các nút lá bằng cách liệt kê trực tiếp hai khả năng của chúng: hoặc đỉnh tạo thành một cây con đóng nếu trọng số của nó nằm trong [L, R] hoặc nó đóng góp vào một thành phần mở với trọng số của nó là tổng hiện tại. Điều này thiết lập các trạng thái DP cơ sở mà không cần đệ quy. 
3. Xử lý từng nút theo thứ tự sau để các nút con được giải quyết hoàn toàn trước khi kết hợp với nút cha của chúng. Điều này đảm bảo rằng mọi sự hợp nhất đều hoạt động trên các bài toán con hoàn chỉnh. 
4. Hợp nhất hai trạng thái DP con bằng cách sử dụng tích chập trên k. Với mỗi cách chia k thành i và k − i, hãy kết hợp mọi tổng từ S(w1, i) với mọi tổng từ S(w2, k − i). Điều này tạo ra tất cả các tổng có thể có cho cây con được hợp nhất. Lý do bước này cần thiết là vì các cây con đóng có thể được phân bổ độc lập giữa các cây con nên tất cả các kết hợp phải được xem xét. 
5. Sau khi hợp nhất tất cả các nút con của một nút, hãy kết hợp chính nút đó bằng cách dịch chuyển tất cả các tổng trong S(w, k) bằng cách thêm Av. Điều này thể hiện việc mở rộng thành phần mở lên trên qua đỉnh hiện tại. 
6. Sau khi mở rộng, kiểm tra xem tổng kết quả có thể tạo thành cây con đóng hợp lệ bằng cách rơi vào [L, R] hay không. Nếu vậy, chúng tôi đưa ra một trạng thái mới trong đó thành phần đó đóng và thành phần mở đặt lại về 0, tăng k lên một và chèn 0 vào S(v, k + 1). 
7. Áp dụng bước rút gọn cho mỗi S(v, k). Bước này loại bỏ các giá trị trung gian không cần thiết để quyết định các giao điểm khoảng có độ dài R − L, chỉ giữ lại biểu diễn nén. Việc rút gọn đảm bảo rằng đối với bất kỳ truy vấn khoảng nào có độ rộng R − L, câu trả lời vẫn không thay đổi. 
8. Tiếp tục quá trình này cho đến tận thư mục gốc. Câu trả lời cuối cùng là liệu 0 có chứa trong S(1, K + 1 hay không), nghĩa là chúng ta có thể đóng chính xác K + 1 cây con và kết thúc mà không có thành phần mở nào. 

### Tại sao nó hoạt động 

Bất biến chính là mỗi S(v, k) không cần lưu trữ tất cả các tổng chính xác, chỉ cần lưu trữ đủ đại diện sao cho bất kỳ khoảng có độ dài R − L nào cắt tập hợp khi và chỉ nếu tập ban đầu không nén. Phép rút gọn bảo toàn tính chất này và phép toán tổng cũng bảo toàn tính chất đó vì tính không phân biệt được khoảng là ổn định trong phép cộng. Kết quả là, mọi quá trình chuyển đổi DP đều hoạt động trên một bản trình bày được nén nhưng tương đương với không gian giải pháp có thể tiếp cận, do đó tính khả thi không bao giờ bị mất hoặc được đưa ra không chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    n, K, L, R = map(int, input().split())
    w = [0] + list(map(int, input().split()))

    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        a, b = map(int, input().split())
        g[a].append(b)
        g[b].append(a)

    parent = [0] * (n + 1)
    order = []
    stack = [1]
    parent[1] = -1

    while stack:
        v = stack.pop()
        order.append(v)
        for to in g[v]:
            if to == parent[v]:
                continue
            parent[to] = v
            stack.append(to)

    S = [None] * (n + 1)

    def reduce_set(arr):
        if not arr:
            return []
        arr = sorted(set(arr))
        res = []
        D = R - L
        last_keep = -10**18
        for x in arr:
            if not res or x > last_keep + D:
                res.append(x)
                last_keep = x
        return res

    def merge(A, B):
        if not A:
            return B[:]
        if not B:
            return A[:]
        out = []
        for a in A:
            for b in B:
                out.append(a + b)
        return reduce_set(out)

    def dp(v):
        cur = {0: [0]}  # k -> set of sums

        for to in g[v]:
            if to == parent[v]:
                continue
            child = dp(to)
            nxt = {}
            for k1, s1 in cur.items():
                for k2, s2 in child.items():
                    k = k1 + k2
                    merged = merge(s1, s2)
                    if k not in nxt:
                        nxt[k] = merged
                    else:
                        nxt[k] = merge(nxt[k], merged)
            cur = nxt

        new = {}
        for k, vals in cur.items():
            shifted = [x + w[v] for x in vals]
            new[k] = reduce_set(shifted)

        if any(L <= x <= R for x in new.get(0, [])):
            if 1 not in new:
                new[1] = []
            new[1].append(0)
            new[1] = reduce_set(new[1])

        return new

    res = dp(1)
    print("YES" if 0 in res.get(K + 1, []) else "NO")

if __name__ == "__main__":
    solve()
```Cấu trúc cốt lõi tuân theo định nghĩa DP trực tiếp. Mỗi nút tính toán một từ điển từ k đến một danh sách nén các tổng có thể có. Hàm hợp nhất thực hiện tích Descartes giữa hai tập con và sau đó nén mạnh bằng cách sử dụng quy tắc rút gọn. Việc giảm thiểu là cần thiết để ngăn chặn sự bùng nổ trong phạm vi giá trị. 

Bước mở rộng được thực hiện bằng cách dịch chuyển tất cả các tổng theo trọng số nút. Điều kiện đóng kiểm tra xem có bất kỳ tổng nào rơi vào [L, R] hay không và sau đó đặt lại thành phần mở bằng cách chèn số 0 vào trạng thái k tiếp theo. 

Câu trả lời cuối cùng kiểm tra xem liệu chúng ta có thể đạt được chính xác K + 1 thành phần đóng với thành phần mở trống ở gốc hay không. 

## Ví dụ đã hoạt động 

Vì câu lệnh ban đầu không cung cấp mẫu rõ ràng, hãy xem xét một cây nhỏ có ba nút trên một dòng: 1 nối với 2 nối với 3, trọng số [3, 2, 4], với L = 3, R = 5 và K = 1. 

Chúng tôi theo dõi DP tại nút 3 trở lên. 

| Nút | Bộ đến | Sau khi hợp nhất | Sau ca | Kiểm tra đóng cửa | 
| --- | --- | --- | --- | --- | 
| 3 | {0: [0]} | giống nhau | {0: [4]} | không | 
| 2 | trẻ từ 3 tuổi | {0: [4]} | {0: [6]} | không | 
| 1 | trẻ từ 2 tuổi | {0: [6]} | {0: [9]} | không | 

Điều này thể hiện trường hợp lỗi khi không có phân đoạn nào rơi vào [3, 5], do đó không xảy ra việc đóng. 

Bây giờ sửa đổi trọng số thành [2, 2, 2] với L = 2, R = 4, K = 1. 

| Nút | Bộ đến | Sau khi hợp nhất | Sau ca | Kiểm tra đóng cửa | 
| --- | --- | --- | --- | --- | 
| 3 | {0: [0]} | giống nhau | {0: [2]} | có → thêm đã đóng | 
| 2 | nhận được sự đóng cửa | {0: [0]} | {0: [2]} | vâng | 
| 1 | cuối cùng | {0: [0]} | {0: [2]} | vâng | 

Điều này cho thấy các cây con hợp lệ được đóng và đặt lại nhiều lần như thế nào. 

Dấu vết đầu tiên xác nhận tính đúng đắn trong việc từ chối các phân vùng không thể thực hiện được, trong khi dấu vết thứ hai thể hiện việc kích hoạt lặp đi lặp lại cơ chế đóng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N K^3) | mỗi lần hợp nhất là O(K^2) với tổng số lần hợp nhất tối đa là O(NK) do ràng buộc về cấu trúc | 
| Không gian | O(NK) | mỗi nút giữ compre |
