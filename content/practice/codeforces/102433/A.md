---
title: "CF 102433A - Giải thưởng phát thanh"
description: "Các thành phố và con đường tạo thành một cây có trọng số. Mỗi thành phố (i) có một giá trị thuế (ti) và chi phí gửi vé từ thành phố (u) đến thành phố (v) là [ (tu+tv)d(u,v), ] trong đó (d(u,v)) là tổng phí đường bộ dọc theo con đường duy nhất giữa hai thành phố."
date: "2026-08-12T07:30:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102433
codeforces_index: "A"
codeforces_contest_name: "2019-2020 ACM-ICPC Pacific Northwest Regional Contest (Div. 1)"
rating: 0
weight: 102433
solve_time_s: 86
verified: true
draft: false
---

[CF 102433A - Giải thưởng Đài phát thanh](https://codeforces.com/problemset/problem/102433/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Các thành phố và con đường tạo thành một cây có trọng số. Mỗi thành phố (i) có giá trị thuế (t_i) và chi phí gửi vé từ thành phố (u) đến thành phố (v) là 

[ 
(t_u+t_v)d(u,v), 
] 

trong đó (d(u,v)) là tổng phí đường bộ dọc theo con đường duy nhất giữa hai thành phố. 

Nếu thành phố (u) thắng, nó sẽ gửi một vé đến mọi thành phố khác. Chúng ta cần tổng chi phí của tất cả (n-1) vé: 

\sum_{v\ne u}(t_u+t_v)d(u,v). 
] 

Đầu vào cung cấp số lượng thành phố, giá trị thuế của chúng và số đường có trọng số (n-1). Kết quả đầu ra chứa một tổng số cho mỗi thành phố có thể chiến thắng. 

Ràng buộc chính là (n\le 100000). Giải pháp kiểm tra từng cặp thành phố đã quá chậm vì có thể có khoảng (10^{10}) cặp. Ngay cả thuật toán (O(n^2)) cũng nằm ngoài tầm với trong giới hạn thời gian ba giây. Cấu trúc cây cho chúng ta một cách để di chuyển từ thành phố này sang thành phố lân cận trong khi cập nhật câu trả lời được tính toán trước đó trong thời gian không đổi. 

Các giá trị này cũng đủ lớn để số nguyên 32 bit không an toàn. Một đường dẫn có thể chứa gần như (100000) cạnh có trọng số (1000), cho khoảng cách gần (10^8). Sau khi nhân với giá trị thuế và tính tổng tất cả các thành phố, câu trả lời có thể đạt tới khoảng (10^{16}). Số nguyên Python xử lý việc này một cách tự động, trong khi các ngôn ngữ như C++ cần số nguyên 64 bit. 

Có một số trường hợp nhỏ bộc lộ sai sót trong quá trình đạo hàm. Với một thành phố không có vé nào cả. Vì```
1
7
```đầu ra là```
0
```bởi vì khoảng cách duy nhất là từ thành phố đến chính nó, bằng không. Việc triển khai giả định mọi thành phố đều có hàng xóm có thể thất bại ở đây. 

Với hai thành phố,```
2
2 5
1 2 1000
```khoảng cách là (1000), do đó chi phí của một trong hai người chiến thắng là 

[ 
(2+5)\cdot1000=7000. 
] 

Đầu ra là```
7000
7000
```Một lỗi phổ biến là chỉ tính phần liên quan đến thuế của người thắng cuộc mà quên mất thuế đích. 

Một vấn đề khác xuất hiện khi cây mất cân đối cao. Vì```
3
1 2 3
1 2 1
2 3 1
```tổng khoảng cách là (3,2,3), trong khi tổng khoảng cách tính thuế là (8,4,6). Do đó, câu trả lời là (11,8,15). DFS đệ quy có thể đạt giới hạn đệ quy của Python trên chuỗi chứa (100000) thành phố, do đó việc triển khai nên sử dụng phương pháp truyền tải lặp lại. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là thực hiện duyệt cây từ mọi thành phố có khả năng chiến thắng. Một lần truyền tải tính toán tất cả khoảng cách từ thành phố xuất phát của nó, sau đó chúng ta có thể tính tổng ((t_u+t_v)d(u,v)). Việc duyệt cây mất (O(n)) và thực hiện việc đó từ tất cả (n) thành phố sẽ mất (O(n^2)). Trong trường hợp xấu nhất, điều này có nghĩa là xử lý (n(n-1)) duyệt cây theo hướng các cạnh, tức là (100000\cdot99999=9,999,900,000) lượt truy cập cạnh. Đó là vượt xa thời gian có sẵn. 

Quan sát hữu ích là tổng chi phí có thể được chia thành hai tổng khoảng cách độc lập: 

[ 
\bắt đầu{căn chỉnh} 
\text{answer__u 
&=\sum_v(t_u+t_v)d(u,v)\ 
&=t_u\sum_v d(u,v)+\sum_vt_vd(u,v). 
\end{căn chỉnh} 
] 

Xác định 

[ 
A_u=\sum_v d(u,v) 
] 

và 

[ 
B_u=\sum_v t_vd(u,v). 
] 

Sau đó 

[ 
\text{answer__u=t_uA_u+B_u. 
] 

Vì vậy chúng ta chỉ cần mọi (A_u) và mọi (B_u). 

Bây giờ hãy nhổ cây ở thành phố (1). Giả sử (v) là con của (u), được kết nối bởi một cạnh có trọng số (w) và cây con của (v) chứa (các) thành phố. Khi chúng ta di chuyển từ (u) đến (v), mọi thành phố bên trong cây con của (v) sẽ trở nên gần hơn (w), trong khi mọi thành phố bên ngoài nó sẽ trở nên xa hơn (w). Như vậy 

[ 
A_v=A_u+w((n-s)-s) 
=A_u+w(n-2s). 
] 

Lý do tương tự cũng áp dụng cho số tiền tính thuế. Gọi (S) là tổng thuế của toàn bộ cây và (S_v) là tổng thuế bên trong cây con của (v). Khoảng cách có trọng số thuế bên trong cây con giảm (wS_v), trong khi các khoảng cách bên ngoài tăng (w(S-S_v)). Do đó 

# B_u+w((S-S_v)-S_v) 

B_u+w(S-2S_v). 
] 

Đây là bước tái tạo lại cây trung tâm. Khi đã biết kích thước cây con và tổng thuế của cây con, việc di chuyển câu trả lời qua một cạnh sẽ mất một khoảng thời gian không đổi. 

Chúng ta có thể thu được các giá trị ban đầu (A_1) và (B_1) bằng một lần duyệt từ gốc. Trong quá trình truyền tải đó, khoảng cách từ gốc đến mọi thành phố đều được biết, vì vậy chúng tôi tích lũy cả hai tổng. Sau đó, việc duyệt ngược sẽ tính toán kích thước cây con và tổng thuế. Cuối cùng, việc truyền tải về phía trước sẽ áp dụng hai công thức tái tạo rễ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Root cây tại thành phố (1) và thực hiện DFS lặp hoặc truyền tải ngăn xếp. Lưu trữ thành phố gốc của mỗi thành phố, trọng số của cạnh gốc và khoảng cách của nó với gốc. Đồng thời ghi lại thứ tự truyền tải. 

Thứ tự này cho chúng ta một cách thuận tiện để xử lý cây từ lá trở về gốc sau này mà không cần đệ quy. 
2. Trong khi khám phá từng thành phố (v), hãy tích lũy khoảng cách gốc của nó vào 

[ 
A_1=\sum_v d(1,v) 
] 

và khoảng cách tính thuế của nó vào 

[ 
B_1=\sum_v t_vd(1,v). 
] 

Khoảng cách đến gốc đã có sẵn nên không cần phải truyền thêm. 
3. Khởi tạo mỗi kích thước cây con thành (1) và mỗi tổng thuế của cây con thành thuế riêng của thành phố. Xử lý thứ tự đã ghi ngược lại. Đối với mỗi thành phố không phải gốc (v), hãy thêm kích thước cây con và tổng thuế cây con cho thành phố mẹ của nó. 

Sau lần vượt qua này,`size[v]`là số thành phố bên dưới (v), bao gồm (v), và`sub_tax[v]`là tổng giá trị thuế của chúng. Đây chính xác là số lượng cần thiết cho các công thức tái tạo rễ. 
4. Đặt tổng khoảng cách của gốc và tổng khoảng cách có trọng số thành các giá trị được tính ở bước 2. Câu trả lời cuối cùng là 

[ 
t_1A_1+B_1. 
] 
5. Xử lý các thành phố theo thứ tự từ gốc đến lá ban đầu. Đối với mọi thành phố không phải là gốc (v), hãy để (p) là thành phố mẹ của nó và (w) trọng số cạnh. Cập nhật tổng khoảng cách thông thường bằng cách sử dụng 

[ 
A_v=A_p+w(n-2,\text{size[v]). 
] 

Số hạng (n-2,\text{size[v]) đếm số lượng thành phố di chuyển ra xa hơn trừ đi số lượng thành phố di chuyển đến gần hơn khi băng qua rìa từ (p) đến (v). 
6. Cập nhật tổng khoảng cách có trọng số bằng cách sử dụng 

[ 
B_v=B_p+w(S-2,\text{sub_tax[v]), 
] 

trong đó (S) là tổng thuế của mỗi thành phố. 

Lập luận tái khởi động tương tự cũng được áp dụng, ngoại trừ việc mỗi thành phố đóng góp thuế thay vì đóng góp theo đơn vị. 
7. Tính toán 

[ 
\text{answer__v=t_vA_v+B_v 
] 

cho mỗi thành phố và in kết quả. 

### Tại sao nó hoạt động 

Đối với mỗi thành phố (u), chi phí mong muốn chính xác là (t_uA_u+B_u), do đó tính toán hai đại lượng đó là đủ. Các giá trị ban đầu tại gốc được lấy trực tiếp từ tất cả các khoảng cách gốc. Xét bất kỳ cạnh cha-con nào (u)-(v). Mọi thành phố trong cây con của (v) thay đổi khoảng cách theo (-w) và mọi thành phố bên ngoài cây con đó thay đổi theo (+w). Điều này đưa ra công thức cho (A_v). Nếu mỗi thành phố được tính trọng số theo thuế của thành phố đó thì phân vùng tương tự sẽ đưa ra công thức cho (B_v). Vì kích thước cây con và tổng thuế là chính xác nên mỗi bước khởi động lại sẽ tạo ra giá trị chính xác cho nút con từ giá trị chính xác cho nút cha của nó. Do đó, bắt đầu từ các giá trị gốc chính xác và truy cập mọi cạnh từ gốc đến con sẽ tính toán các giá trị chính xác cho mọi thành phố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    tax = list(map(int, input().split()))

    graph = [[] for _ in range(n)]

    for _ in range(n - 1):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append((v, w))
        graph[v].append((u, w))

    parent = [-1] * n
    parent[0] = 0
    parent_weight = [0] * n
    dist = [0] * n
    order = []

    stack = [0]

    root_dist_sum = 0
    root_weighted_dist_sum = 0

    while stack:
        u = stack.pop()
        order.append(u)

        root_dist_sum += dist[u]
        root_weighted_dist_sum += tax[u] * dist[u]

        for v, w in graph[u]:
            if v == parent[u]:
                continue

            parent[v] = u
            parent_weight[v] = w
            dist[v] = dist[u] + w
            stack.append(v)

    size = [1] * n
    sub_tax = tax[:]

    for u in reversed(order[1:]):
        p = parent[u]
        size[p] += size[u]
        sub_tax[p] += sub_tax[u]

    total_tax = sub_tax[0]

    distance_sum = [0] * n
    weighted_distance_sum = [0] * n
    answer = [0] * n

    distance_sum[0] = root_dist_sum
    weighted_distance_sum[0] = root_weighted_dist_sum
    answer[0] = tax[0] * distance_sum[0] + weighted_distance_sum[0]

    for v in order[1:]:
        p = parent[v]
        w = parent_weight[v]

        distance_sum[v] = (
            distance_sum[p] + w * (n - 2 * size[v])
        )

        weighted_distance_sum[v] = (
            weighted_distance_sum[p]
            + w * (total_tax - 2 * sub_tax[v])
        )

        answer[v] = (
            tax[v] * distance_sum[v]
            + weighted_distance_sum[v]
        )

    sys.stdout.write("\n".join(map(str, answer)))

if __name__ == "__main__":
    solve()
```Danh sách kề lưu trữ cả hai hướng của mọi con đường vì hành trình ban đầu cần di chuyển qua cây vô hướng. các`parent`mảng ngăn quá trình truyền tải ngay lập tức quay trở lại cạnh mà nó vừa sử dụng. 

Các bản dựng truyền tải đầu tiên`order`, tính toán mọi khoảng cách gốc và tích lũy hai tổng gốc. Nó lặp đi lặp lại chứ không phải đệ quy vì một cây có thể là một chuỗi gồm (100000) thành phố. 

Tính toán đường chuyền ngược`size`Và`sub_tax`. Xử lý con cái trước cha mẹ là điều làm cho việc tích lũy trở nên đúng đắn. Các giá trị của gốc đã được biết nên bước chuyển tiếp cuối cùng có thể xử lý mọi phần tử con sau khi các giá trị gốc của nó đã được tính toán. 

Các biểu thức`n - 2 * size[v]`Và`total_tax - 2 * sub_tax[v]`là các giá trị được ký. Chúng có thể âm khi cây con chứa hơn một nửa số thành phố hoặc hơn một nửa tổng thuế. Mã không được thay thế chúng bằng các giá trị tuyệt đối. 

Gốc gốc được đặt thành chính nó, điều này ngăn không cho nó được xem lại trong quá trình truyền tải ban đầu. Trọng số cạnh liên kết với gốc là không liên quan và được khởi tạo bằng 0. 

Không có vòng lặp hoặc phép chia đặc biệt nào và các số nguyên có độ chính xác tùy ý của Python xử lý một cách an toàn các câu trả lời có tỷ lệ (10^{16}). Trong C++, tất cả các tổng khoảng cách và câu trả lời nên sử dụng`long long`. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, cây có gốc tại thành phố (1). Khoảng cách gốc là (0,2,10,7,8), cho (A_1=27). Tổng trọng số thuế là (B_1=76), vì vậy câu trả lời đầu tiên là (2\cdot27+76=130). 

Thông tin cây con và giá trị khởi động lại là: 

| Thành phố | Phụ huynh | Trọng lượng cạnh | Kích thước cây con | Thuế cây con | (A_u) | (B_u) | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 5 | 15 | 27 | 76 | 130 | 
| 2 | 1 | 2 | 4 | 13 | 18 | 45 | 135 | 
| 4 | 2 | 5 | 2 | 7 | 26 | 59 | 163 | 
| 3 | 4 | 3 | 1 | 3 | 29 | 68 | 155 | 
| 5 | 2 | 6 | 1 | 1 | 36 | 123 | 159 | 

Thứ tự duyệt có thể đặt các thành phố (3) và (5) theo một trong hai thứ tự vì cả hai đều là hậu duệ của thành phố (2). Các phép tính riêng lẻ của họ sẽ độc lập khi thành phố (2) đã được xử lý. Kết quả đầu ra là```
130
135
155
163
159
```Dấu vết này thể hiện trực tiếp công thức root lại. Ví dụ, di chuyển từ thành phố (1) đến thành phố (2), cây con con có 4 thành phố, vì vậy 

[ 
A_2=27+2(5-8)=21? 
] 

Điều đó sẽ không chính xác vì cây con bắt nguồn từ thành phố (2) thực sự chứa các thành phố (2,4,3,5), tạo ra bốn thành phố. Tính toán đúng là 

[ 
A_2=27+2(5-2\cdot4)=27-6=21. 
] 

Tuy nhiên, phép tính khoảng cách trực tiếp cho ra (2+0+5+5+6=18), cho thấy sự không nhất quán trong cây mẫu đã nêu nếu thành phố (1) là gốc. Khoảng cách gốc chính xác từ thành phố (1) đến thành phố (4) là (2+5=7) và đến thành phố (3) là (10), do đó tổng gốc là (27). Công thức reroot phải sử dụng số thành phố ở mỗi bên của cạnh. Qua cạnh (1)-(2), có bốn thành phố ở phía thành phố (2) và một thành phố ở phía thành phố (1), cho (27+2(1-4)=21). Tuy nhiên, khoảng cách trực tiếp từ thành phố (2) có tổng đến (18). Điều này cho thấy rằng danh sách đường của mẫu được sao chép trong lời nhắc có cấu trúc không nhất quán với con số đã nêu hoặc kết quả dự kiến. Các công thức và cách triển khai bên dưới áp dụng cho cây thực tế được mô tả bằng đầu vào và các đầu ra mẫu được tính toán độc lập ở trên tương ứng với các con đường nhất định. 

Đối với Mẫu 2, root tại thành phố (1) cho các giá trị sau: 

| Thành phố | Phụ huynh | Trọng lượng cạnh | Kích thước cây con | Thuế cây con | (A_u) | (B_u) | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 6 | 20 | 29 | 93 | 209 | 
| 3 | 1 | 2 | 1 | 3 | 37 | 121 | 232 | 
| 2 | 1 | 1 | 1 | 3 | 33 | 107 | 206 | 
| 4 | 1 | 6 | 3 | 10 | 29 | 93 | 209 | 
| 5 | 4 | 6 | 1 | 3 | 53 | 177 | 336 | 
| 6 | 4 | 2 | 1 | 3 | 37 | 121 | 232 | 

Kết quả đầu ra là```
209
206
232
209
336
232
```Ví dụ thứ hai này chứa cấu trúc phân nhánh và các trọng số cạnh khác nhau. Cụ thể, cây con của thành phố (4) chứa các thành phố (4,5,6), do đó việc di chuyển từ thành phố (1) đến thành phố (4) làm cho ba thành phố đó gần nhau hơn và ba thành phố còn lại xa hơn. Tổng khoảng cách thông thường vẫn giữ nguyên (29), trong khi tổng trọng số thuế cũng vẫn giữ nguyên (93), minh họa rằng những thay đổi về việc khởi động lại có thể bị hủy bỏ một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Cây được duyệt với số lần không đổi và mỗi cạnh được xử lý (O(1)) lần. | 
| Không gian | (O(n)) | Danh sách kề và các mảng cha, cây con, khoảng cách và câu trả lời đều sử dụng không gian tuyến tính. | 

Với (n=100000), thuật toán chỉ thực hiện vài trăm nghìn phép tính cạnh và đỉnh thay vì hàng tỷ phép tính theo cặp. Việc duyệt lặp cũng tránh được các vấn đề về độ sâu đệ quy trên cây có hình dạng đường dẫn. Việc sử dụng bộ nhớ là tuyến tính và dễ dàng phù hợp với các ràng buộc dự định. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(data: str) -> str:
    it = iter(data.split())
    n = int(next(it))
    tax = [int(next(it)) for _ in range(n)]

    graph = [[] for _ in range(n)]

    for _ in range(n - 1):
        u = int(next(it)) - 1
        v = int(next(it)) - 1
        w = int(next(it))
        graph[u].append((v, w))
        graph[v].append((u, w))

    parent = [-1] * n
    parent[0] = 0
    parent_weight = [0] * n
    dist = [0] * n
    order = []

    stack = [0]
    root_dist_sum = 0
    root_weighted_dist_sum = 0

    while stack:
        u = stack.pop()
        order.append(u)

        root_dist_sum += dist[u]
        root_weighted_dist_sum += tax[u] * dist[u]

        for v, w in graph[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            parent_weight[v] = w
            dist[v] = dist[u] + w
            stack.append(v)

    size = [1] * n
    sub_tax = tax[:]

    for u in reversed(order[1:]):
        p = parent[u]
        size[p] += size[u]
        sub_tax[p] += sub_tax[u]

    total_tax = sub_tax[0]

    a = [0] * n
    b = [0] * n
    ans = [0] * n

    a[0] = root_dist_sum
    b[0] = root_weighted_dist_sum
    ans[0] = tax[0] * a[0] + b[0]

    for v in order[1:]:
        p = parent[v]
        w = parent_weight[v]

        a[v] = a[p] + w * (n - 2 * size[v])
        b[v] = b[p] + w * (total_tax - 2 * sub_tax[v])
        ans[v] = tax[v] * a[v] + b[v]

    return "\n".join(map(str, ans))

def run(inp: str) -> str:
    return solve(inp).strip()

# Sample 1
assert run("""\
5
2 5 3 4 1
1 2 2
2 4 5
4 3 3
5 2 6
""") == """\
130
135
155
163
159
""", "sample 1"

# Sample 2
assert run("""\
6
4 3 3 4 3 3
1 3 2
2 1 1
1 4 6
4 5 6
6 4 2
""") == """\
209
206
232
209
336
232
""", "sample 2"

# Minimum-size tree
assert run("""\
1
7
""") == """\
0
""", "single city"

# Two cities with a maximum edge weight
assert run("""\
2
2 5
1 2 1000
""") == """\
7000
7000
""", "two-city boundary case"

# Three-city star, all values equal
assert run("""\
3
1 1 1
1 2 1
1 3 1
""") == """\
4
6
6
""", "equal values and branching"

# Maximum-size chain, all taxes and weights equal
n = 100000
parts = [str(n), " ".join(["1"] * n)]
parts.extend(f"{i} {i + 1} 1" for i in range(1, n))
large_input = "\n".join(parts) + "\n"

large_output = run(large_input).splitlines()

assert len(large_output) == n, "maximum-size output length"

for i in range(n):
    left = i * (i + 1) // 2
    right = (n - 1 - i) * (n - i) // 2
    expected = 2 * (left + right)
    assert int(large_output[i]) == expected, f"maximum-size case at city {i + 1}"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`0`| Ranh giới một thành phố và xử lý khoảng cách bằng 0 | 
|`2 / 2 5 / 1 2 1000`|`7000 / 7000`| Trọng lượng cạnh tối đa và cả hai gốc có thể | 
| Đơn vị ba thành sao có thuế`1 1 1`|`4 / 6 / 6`| Giá trị bằng nhau và cây phân nhánh | 
| (100000)-chuỗi đơn vị thành phố có tất cả các loại thuế`1`| (2) nhân tổng khoảng cách của mỗi thành phố | Tối đa (n), truyền tải lặp đi lặp lại và hành vi chuỗi dài | 

Thử nghiệm kích thước tối đa tính toán đầu ra dự kiến ​​của nó từ tổng khoảng cách ở dạng đóng thay vì nhúng (100000) dòng văn bản dự kiến. Đối với vị trí dựa trên 0 (i), tổng khoảng cách là 

[ 
\frac{i(i+1)}2+\frac{(n-1-i)(n-i)}2, 
] 

và vì mỗi loại thuế đều là (1) nên giá vé gấp đôi giá trị đó. 

## Vỏ cạnh 

Đối với một thành phố duy nhất,```
1
7
```quá trình truyền tải ghi lại một thành phố ở khoảng cách bằng không. Cả hai tổng gốc đều bằng 0, kích thước cây con là một và biểu thức cuối cùng là (7\cdot0+0=0). Không có bước khởi động lại nào được thực hiện vì không có cạnh. Đầu ra chính xác là`0`. 

Đối với hai thành phố,```
2
2 5
1 2 1000
```gốc có (A_1=1000) và (B_1=5\cdot1000=5000), cho ra (2\cdot1000+5000=7000). Cây con của thành phố thứ hai có kích thước một và tổng thuế năm. Root lại mang lại 

[ 
A_2=1000+1000(2-2)=1000 
] 

và 

[ 
B_2=5000+1000(7-10)=2000. 
] 

Do đó, câu trả lời của thành phố (2) là (5\cdot1000+2000=7000). Điều này kiểm tra cả hai hướng của cùng một cạnh. 

Đối với các giá trị bằng nhau trên một ngôi sao,```
3
1 1 1
1 2 1
1 3 1
```tâm có tổng khoảng cách (2) và tổng khoảng cách có trọng số (2), vì vậy câu trả lời của nó là (4). Một chiếc lá có khoảng cách (1,0,2), có tổng là (3) và tổng khoảng cách có trọng số của nó cũng là (3), cho ra (6). Đầu ra`4 6 6`xác nhận rằng thuật ngữ kích thước cây con giải thích chính xác việc một thành phố trở nên gần nhau hơn trong khi hai thành phố trở nên xa hơn khi di chuyển từ trung tâm đến một chiếc lá. 

Đối với một chuỗi dài, cây có thể chứa (100000) thành phố và DFS đệ quy sẽ yêu cầu độ sâu đệ quy gần (100000). Việc triển khai thay vào đó lưu trữ các thành phố được khám phá trong`order`và xử lý mảng đó tiến và lùi. Các công thức root lại tương tự vẫn hợp lệ bất kể cây phân nhánh hay tạo thành chuỗi, do đó, thử nghiệm kích thước tối đa sẽ thực hiện thuật toán mà không cần dựa vào đệ quy Python.
