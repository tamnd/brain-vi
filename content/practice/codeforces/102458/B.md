---
title: "CF 102458B - Daniel và gameshow"
description: "Đồ thị là một cây vì nó có (n) đỉnh và chính xác (n-1) cạnh khi được kết nối. Mỗi cạnh đều có độ cứng (a), nghĩa là Daniel có thể vượt qua cạnh đó nhiều nhất (a) lần trong lượt của mình. Anh ta chọn một đỉnh bắt đầu (R), hoặc khi (R=0), anh ta có thể chọn bất kỳ đỉnh nào."
date: "2026-08-08T10:29:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102458
codeforces_index: "B"
codeforces_contest_name: "Hanoi final contest"
rating: 0
weight: 102458
solve_time_s: 138
verified: true
draft: false
---

[CF 102458B - Daniel và gameshow](https://codeforces.com/problemset/problem/102458/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đồ thị là một cây vì nó có (n) đỉnh và chính xác (n-1) cạnh khi được kết nối. Mỗi cạnh đều có độ cứng (a), nghĩa là Daniel có thể vượt qua cạnh đó nhiều nhất (a) lần trong lượt của mình. Anh ta chọn một đỉnh bắt đầu (R), hoặc khi (R=0), anh ta có thể chọn bất kỳ đỉnh nào. Anh ấy có thể dừng lại bất cứ lúc nào và điểm số của anh ấy chỉ đơn giản là số lần vượt biên mà anh ấy đã thực hiện. 

Khó khăn chính là một cạnh của cây chỉ có một con đường để đi vào phía xa của nó và chỉ có một con đường để quay trở lại. Nếu Daniel về đích ở cùng phía mà anh ấy đã vào, thì cạnh đó phải được vượt qua với số lần chẵn. Nếu Daniel về đích ở phía bên kia, nó phải được vượt qua một số lần lẻ. Do đó, vấn đề nằm ở việc chọn nơi kết thúc của cuộc đi bộ, đồng thời tối đa hóa số lượng đường giao nhau có thể sử dụng được trong mỗi cây con được khám phá. 

Các ràng buộc chính thức cho phép (n) lên tới (10^5), với độ bền cạnh lớn bằng (10^9) và giới hạn thời gian là 2 giây. Điều này loại trừ bất cứ điều gì bậc hai trong (n). Một giải pháp sử dụng (O(n)) công cho mọi đỉnh bắt đầu có thể sẽ thực hiện khoảng (10^{10}) thao tác trong trường hợp xấu nhất. Giải pháp dự định chỉ xử lý cây với số lần không đổi. 

Có một số trường hợp cạnh trong đó tổng dung lượng cạnh đơn giản cho câu trả lời sai. Với một đỉnh thì không có cạnh nào cả:```
1
1
```Câu trả lời là`0`. Một công thức giả định mọi cạnh đều đóng góp một thứ gì đó mà không xử lý cây trống sẽ thất bại ngay lập tức. 

Một cạnh có độ dẻo dai có thể được sử dụng một lần nếu Daniel dừng lại ở điểm cuối còn lại:```
2
1 2 1
1
```Câu trả lời là`1`. Một giải pháp bất cẩn cho rằng mọi cạnh đi qua phải được sử dụng hai lần sẽ trả về 0. 

Một cạnh dẻo dai cũng có thể hoạt động như một cổng một chiều vào cây con lớn. Coi như:```
3
1 2 1
2 3 9
1
```Daniel có thể vượt qua (1)-(2) một lần, sau đó vượt qua (2)-(3) chín lần và kết thúc ở đỉnh 3, với số điểm là`10`. Một giải pháp chỉ đếm các cây con có thể được nhập và trả về hoàn toàn sẽ bỏ lỡ toàn bộ cạnh thứ hai. 

Giá trị (R=0) giới thiệu một sự tinh tế khác vì đỉnh bắt đầu tốt nhất không cần phải là gốc tùy ý được sử dụng khi triển khai. Ví dụ:```
3
1 2 3
1 3 3
0
```Bắt đầu từ đỉnh 2, Daniel có thể sử dụng cả hai cạnh ba lần, kết thúc ở đỉnh 3, với số điểm là`6`. Bắt đầu từ trung tâm chỉ mang lại`5`, nên chỉ lấy rễ ở đỉnh 1 và lấy đáp án ở đó là chưa đủ. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là cố định một đỉnh bắt đầu và chạy một cây DP để quyết định xem mỗi cây con con có được truy cập và trả về từ đó hay không, hay bước đi cuối cùng có đi vào cây con đó hay không. DP này đúng vì mỗi bước đi trên cây đều có một hướng cuối cùng duy nhất tính từ điểm bắt đầu và mọi nhánh đã khám phá khác đều phải được vào và ra khỏi cây nhiều lần như nhau. Nếu chúng ta lặp lại tính toán này một cách độc lập cho mọi đỉnh bắt đầu có thể, thì mỗi lần chạy sẽ mất (O(n)), tạo ra (O(n^2)) công. Tại (n=10^5), đó là khoảng (10^{10}) thao tác, vượt xa giới hạn thời gian. 

Quan sát hữu ích là cùng một DP có thể được biểu diễn dưới dạng thông điệp giữa hai đỉnh liền kề. Loại bỏ một cạnh (uv). Cây chia thành hai thành phần độc lập. Từ phía (u), chúng ta chỉ cần hai thông tin: điểm tốt nhất của bước đi bắt đầu và kết thúc tại (u) và điểm tốt nhất của bước đi bắt đầu tại (u) và kết thúc ở bất kỳ đâu trong thành phần đó. 

Gọi các giá trị này là (F(u\to v)) và (G(u\to v)). Khi tất cả các tin nhắn đi vào một đỉnh đều được biết đến, tin nhắn được gửi tới bất kỳ người hàng xóm nào sẽ nhận được bằng cách loại trừ sự đóng góp của người hàng xóm đó. Đây chính xác là cấu trúc mà việc root lại DP được thiết kế. 

Để có được độ dẻo dai (a), hãy xác định cách sử dụng đồng đều tốt nhất và cách sử dụng lẻ tốt nhất của nó. Cách sử dụng chẵn tốt nhất là (a) khi (a) chẵn và (a-1) khi (a) lẻ. Nếu (a=1), giá trị này bằng 0 và cạnh đó hoàn toàn không thể được sử dụng trong một chuyến tham quan khép kín. Cách sử dụng số lẻ tốt nhất là (a) khi (a) số lẻ và (a-1) khi (a) số chẵn. 

Giả sử một thành phần lân cận cung cấp giá trị đóng (F). Nếu cạnh được sử dụng như một chuyến đi khứ hồi, đóng góp của nó là mức sử dụng chẵn tốt nhất cộng (F), ngoại trừ khi mức sử dụng chẵn tốt nhất bằng 0, trong trường hợp đó thành phần không thể được nhập và trả về. Nếu cạnh được sử dụng làm đường dẫn cuối cùng tới điểm cuối thì đóng góp của nó là cách sử dụng lẻ tốt nhất cộng với giá trị (G) lân cận. 

Điều này cung cấp cả hai thông báo theo thời gian tuyến tính sau khi khởi động lại. Việc duyệt cây đầu tiên sẽ tính toán các thông điệp từ con tới cha mẹ. Quá trình truyền tải thứ hai cung cấp các thông điệp từ cha mẹ đến con cái và tại mỗi đỉnh, đánh giá bước đi tốt nhất bắt đầu từ đó. Nếu (R) cố định, chúng ta lấy giá trị cho đỉnh đó. Nếu (R=0), chúng ta lấy giá trị lớn nhất trên tất cả các đỉnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Tái root tối ưu DP | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gốc cây tạm thời tại đỉnh 1. Gốc này chỉ là một công cụ thực hiện. Nó không liên quan gì đến đỉnh bắt đầu cần thiết của Daniel. 
2. Đối với mọi cạnh của cây có hướng (u\to v), hãy xác định (F(u\to v)) là điểm tối đa của một bước đi khép kín bắt đầu và kết thúc tại (u), chỉ sử dụng thành phần chứa (u) sau khi loại bỏ (uv). Xác định (G(u\to v)) là điểm tối đa của chuyến đi bộ bắt đầu tại (u) và kết thúc ở bất kỳ đâu trong cùng thành phần đó. 

Hai giá trị này là đủ vì một chuyến tham quan con khép kín có thể được gắn với một chuyến đi ở đỉnh hiện tại, trong khi nhiều nhất một con có thể chứa điểm cuối cuối cùng của một chuyến đi mở. 
3. Đối với hàng xóm (w) được nối bằng cạnh dẻo (a), hãy tính đóng góp đóng của nó 

[ 
C = 
\bắt đầu{trường hợp} 
a + F(w\to u), & a\text{ chẵn},\ 
a-1 + F(w\to u), & a\text{ lẻ và }a>1,\ 
0, & a=1. 
\end{trường hợp} 
] 

Lý do cho trường hợp đặc biệt (a=1) là việc vào thành phần sẽ yêu cầu một lần đi qua, nhưng việc quay lại sẽ yêu cầu lần đi qua thứ hai không tồn tại. 
4. Tính đóng góp mở tương ứng 

[ 
O = \operatorname{odd}(a) + G(w\to u), 
] 

trong đó (\operatorname{odd}(a)) là số lẻ lớn nhất không vượt quá (a).

Việc chọn phần đóng góp này có nghĩa là cuộc đi bộ cuối cùng sẽ kết thúc bên trong thành phần của hàng xóm, do đó cạnh kết nối phải được vượt qua một số lần lẻ. 
5. Xử lý cây có gốc từ dưới lên. Với mỗi đỉnh (u), tính tổng các đóng góp đóng của tất cả các phần tử con. Điều này mang lại (F(u\to parent[u])). Trong số trẻ em, hãy tìm giá trị lớn nhất của (O-C). Việc thay thế một chuyến tham quan khép kín của trẻ bằng một chuyến tham quan mở sẽ mang lại 

F(u\to parent[u])+\max(0,\max(O-C)). 
] 

Chỉ một phần tử con có thể chứa điểm cuối cuối cùng, do đó chỉ có thể thay thế một đóng góp đóng. 
6. Xử lý cây từ trên xuống. Tại đỉnh (u), tất cả các tin nhắn đến từ nút cha và nút con của nó đều có sẵn. Tổng hợp những đóng góp khép kín của họ để có được bước đi khép kín tốt nhất bắt đầu và kết thúc tại (u). 
7. Với mỗi cạnh tới, hãy tính (O-C). Bước đi tốt nhất bắt đầu từ (u) vẫn đóng hoặc chọn chính xác một cạnh tới mà qua đó đạt đến điểm cuối cuối cùng. Do đó 

đã đóng[u]+\max(0,\max(O-C)). 
] 
8. Khi đưa ra thông báo (u\to child), hãy loại trừ phần đóng góp của đứa trẻ đó khỏi tổng số. Chúng tôi cần (O-C) tốt nhất trong số tất cả các hàng xóm khác, vì vậy việc triển khai giữ hai giá trị lớn nhất. Nếu giá trị tốt nhất đến từ đứa trẻ bị loại trừ thì giá trị tốt thứ hai sẽ được sử dụng. 
9. Nếu (R\neq0), xuất ra`answer[R]`. Nếu (R=0), xuất ra mức tối đa`answer[u]`trên mọi đỉnh, vì Daniel được tự do lựa chọn điểm xuất phát của mình. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ bước đi hợp lệ nào bắt đầu từ một đỉnh cố định. Vì đồ thị là một cây nên mọi cạnh không nằm trên đường đi từ đỉnh đầu đến đỉnh cuối cùng phải được vượt qua một số chẵn lần. Mỗi cạnh trên đường đi đó phải được vượt qua một số lẻ lần. Đối với cạnh ngoài đường dẫn, lựa chọn tốt nhất có thể chính xác là đóng góp đóng (C). Đối với cạnh đường dẫn, lựa chọn tốt nhất có thể chính xác là đóng góp mở (O). Tại mỗi đỉnh, chỉ một hướng sự cố có thể chứa điểm cuối cuối cùng, do đó DP cần thay thế tối đa một đóng góp đã đóng bằng một đóng góp mở. Các giá trị (F) và (G) nắm bắt chính xác hai khả năng này bên trong mỗi thành phần được định hướng. Việc khởi động lại làm cho cùng một câu lệnh có sẵn từ mọi đỉnh bắt đầu có thể, vì vậy`answer[u]`là tối ưu cho lần khởi đầu đó và lấy mức tối đa là chính xác khi (R=0). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    graph = [[] for _ in range(n)]

    for _ in range(n - 1):
        u, v, a = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append((v, a))
        graph[v].append((u, a))

    R = int(input()) - 1

    # Root the tree at vertex 0.
    parent = [-1] * n
    parent_w = [0] * n
    order = [0]
    parent[0] = -2

    for v in order:
        for to, w in graph[v]:
            if to == parent[v]:
                continue
            parent[to] = v
            parent_w[to] = w
            order.append(to)

    # down_f[v] = F(v -> parent[v])
    # down_g[v] = G(v -> parent[v])
    down_f = [0] * n
    down_g = [0] * n

    for v in reversed(order):
        total = 0
        best_delta = 0

        for to, w in graph[v]:
            if parent[to] != v:
                continue

            even = w - (w & 1)

            if even == 0:
                closed = 0
            else:
                closed = even + down_f[to]

            odd = w if (w & 1) else w - 1
            opened = odd + down_g[to]

            total += closed
            delta = opened - closed
            if delta > best_delta:
                best_delta = delta

        down_f[v] = total
        down_g[v] = total + best_delta

    # up_f[v] = F(parent[v] -> v)
    # up_g[v] = G(parent[v] -> v)
    up_f = [0] * n
    up_g = [0] * n

    answer = [0] * n

    for v in order:
        total = 0

        # Store the two largest O - C values.
        best1 = 0
        best2 = 0
        best_source = -1

        for to, w in graph[v]:
            if to == parent[v]:
                even = w - (w & 1)

                if even == 0:
                    closed = 0
                else:
                    closed = even + up_f[v]

                odd = w if (w & 1) else w - 1
                opened = odd + up_g[v]
            else:
                even = w - (w & 1)

                if even == 0:
                    closed = 0
                else:
                    closed = even + down_f[to]

                odd = w if (w & 1) else w - 1
                opened = odd + down_g[to]

            total += closed
            delta = opened - closed

            if delta > best1:
                best2 = best1
                best1 = delta
                best_source = to
            elif delta > best2:
                best2 = delta

        answer[v] = total + best1

        # Build messages from v to each child.
        for to, w in graph[v]:
            if parent[to] != v:
                continue

            even = w - (w & 1)

            if even == 0:
                child_closed = 0
            else:
                child_closed = even + down_f[to]

            out_f = total - child_closed

            if best_source == to:
                best_delta = best2
            else:
                best_delta = best1

            out_g = out_f + best_delta

            up_f[to] = out_f
            up_g[to] = out_g

    if R == -1:
        print(max(answer))
    else:
        print(answer[R])

if __name__ == "__main__":
    solve()
```Danh sách kề lưu trữ từng cạnh của cây theo cả hai hướng. Gốc tạm thời là đỉnh 1, và`parent`cùng với`order`đưa ra một lệnh DFS lặp lại. Việc lặp lại thay vì DFS đệ quy sẽ tránh được các vấn đề về độ sâu đệ quy của Python trên đường dẫn chứa (10^5) đỉnh. 

Tính toán truyền tải ngược đầu tiên`down_f`Và`down_g`. Đối với một cạnh con,`closed`đại diện cho việc lấy cây con con như một chuyến đi khứ hồi, trong khi`opened`đại diện cho việc làm cho điểm cuối cuối cùng nằm bên trong cây con đó. Sự khác biệt của chúng cho chúng ta biết chính xác sẽ đạt được bao nhiêu khi chọn đứa trẻ đó làm hướng đi cuối cùng. 

Lần duyệt thứ hai tính toán các thông điệp có hướng ngược lại. Tại mỗi đỉnh,`total`chứa tất cả các chuyến du ngoạn khép kín từ mọi hướng sự cố. Hai cái lớn nhất`delta = opened - closed`các giá trị được giữ lại vì khi xây dựng thông điệp hướng tới một đứa trẻ thì đứa trẻ đó phải bị loại trừ. Giữ hai cực đại sẽ tránh quét riêng biệt tất cả các lân cận khác, duy trì độ phức tạp tuyến tính. 

Tất cả các điểm có thể vượt quá phạm vi số nguyên 32 bit. Trên thực tế, có thể có (10^5-1) cạnh và mỗi độ bền có thể gần bằng (10^9), do đó, các số nguyên có độ chính xác tùy ý của Python rất thuận tiện ở đây. Việc tính toán chẵn lẻ sử dụng`w & 1`, Và`w - (w & 1)`cho giá trị chẵn lớn nhất không vượt quá`w`. 

biểu thức`answer[v] = total + best1`cũng an toàn khi`best1`là số không. Điều đó tương ứng với việc kết thúc ở đỉnh hiện tại thay vì nhập thành phần khác. Điều này xử lý các lá bài và các trường hợp mà mọi hướng mở có thể sẽ khiến điểm số trở nên tồi tệ hơn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Cây là một ngôi sao có tâm ở đỉnh 1, có độ bền cạnh (3,4,3,4). Daniel có thể chọn đỉnh xuất phát của mình vì (R=0). 

Đối với trung tâm, mọi chi nhánh đều có thể được sử dụng như một chuyến tham quan khép kín. Cạnh độ dẻo dai-3 đóng góp 2 khi Daniel quay trở lại, trong khi cạnh độ dẻo dai-4 đóng góp 4. Do đó, giá trị đóng ở tâm là (2+4+2+4=12). Chọn nhánh có độ dẻo dai-3 làm hướng cuối cùng sẽ thay đổi mức đóng góp của nó từ 2 lên 3, tăng điểm lên 1. 

| Đỉnh | Giá trị đóng | Tốt nhất (OC) | Điểm khởi đầu tốt nhất | 
| --- | --- | --- | --- | 
| 1 | 12 | 1 | 13 | 
| 2 | 12 | 2 | 14 | 
| 3 | 12 | 0 | 12 | 
| 4 | 12 | 2 | 14 | 
| 5 | 12 | 0 | 12 | 

Tối đa là 14. Một bước đi tối ưu bắt đầu ở đỉnh 2, đi qua cạnh (2-1) ba lần và sử dụng các nhánh còn lại làm chuyến du ngoạn khép kín. Tính toán root lại tìm thấy khả năng này mặc dù việc triển khai ban đầu đã root cây ở đỉnh 1. Mẫu chính thức đưa ra kết quả`14`. 

### Mẫu 2 

Ở đây cái cây là một con đường có độ cứng (2,1,2,1) và Daniel phải bắt đầu từ đỉnh 1. 

Đóng góp khép kín của cạnh độ dẻo dai-2 là 2. Cạnh độ dẻo dai-1 đóng góp bằng 0 cho một hành trình khép kín vì nó không thể vượt qua hai lần. Bắt đầu từ đỉnh 1, cạnh đầu tiên có thể được gạch chéo hai lần, cho hai điểm. Sau đó, Daniel có thể vượt qua ranh giới độ dẻo dai-1 một lần và dừng lại, ghi thêm một điểm. Ngoài ra, sau khi đạt đến đỉnh 3, anh ta có thể sử dụng cạnh dẻo dai-2 theo hướng thích hợp, đạt mức tối ưu tương tự là 4. 

| Cạnh | Độ dẻo dai | Cách sử dụng đồng đều tốt nhất | Cách sử dụng lẻ tốt nhất | 
| --- | --- | --- | --- | 
| 1-2 | 2 | 2 | 1 | 
| 2-3 | 1 | 0 | 1 | 
| 3-4 | 2 | 2 | 1 | 
| 4-5 | 1 | 0 | 1 | 

Để bắt đầu cố định ở đỉnh 1, DP thu được`answer[1] = 4`. Đầu ra mẫu chính thức là`4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Cây được duyệt với số lần không đổi và mỗi mục lân cận được xử lý với số lần không đổi. | 
| Không gian | (O(n)) | Danh sách kề và các mảng cha, DP, thông báo và trả lời đều yêu cầu không gian tuyến tính. | 

Với (n\le10^5), xử lý tuyến tính chỉ có nghĩa là vài triệu phép toán kề, phù hợp với giới hạn 2 giây. Việc sử dụng bộ nhớ cũng tuyến tính và thoải mái dưới giới hạn 512 MB do sự cố chỉ định. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây sử dụng cách triển khai khởi động lại tương tự như giải pháp đã gửi và kiểm tra các mẫu chính thức cùng với các trường hợp nhắm mục tiêu chẵn lẻ, độ bền, vị trí bắt đầu tự do và kích thước cây tối đa được phép.```python
import sys
import io

def solve_data(data: str) -> str:
    it = iter(data.split())
    n = int(next(it))

    graph = [[] for _ in range(n)]

    for _ in range(n - 1):
        u = int(next(it)) - 1
        v = int(next(it)) - 1
        w = int(next(it))
        graph[u].append((v, w))
        graph[v].append((u, w))

    R = int(next(it)) - 1

    parent = [-1] * n
    parent_w = [0] * n
    order = [0]
    parent[0] = -2

    for v in order:
        for to, w in graph[v]:
            if to == parent[v]:
                continue
            parent[to] = v
            parent_w[to] = w
            order.append(to)

    down_f = [0] * n
    down_g = [0] * n

    for v in reversed(order):
        total = 0
        best = 0

        for to, w in graph[v]:
            if parent[to] != v:
                continue

            even = w - (w & 1)
            closed = 0 if even == 0 else even + down_f[to]

            odd = w if (w & 1) else w - 1
            opened = odd + down_g[to]

            total += closed
            best = max(best, opened - closed)

        down_f[v] = total
        down_g[v] = total + best

    up_f = [0] * n
    up_g = [0] * n
    ans = [0] * n

    for v in order:
        total = 0
        best1 = 0
        best2 = 0
        source = -1

        for to, w in graph[v]:
            if to == parent[v]:
                even = w - (w & 1)
                closed = 0 if even == 0 else even + up_f[v]

                odd = w if (w & 1) else w - 1
                opened = odd + up_g[v]
            else:
                even = w - (w & 1)
                closed = 0 if even == 0 else even + down_f[to]

                odd = w if (w & 1) else w - 1
                opened = odd + down_g[to]

            total += closed
            delta = opened - closed

            if delta > best1:
                best2 = best1
                best1 = delta
                source = to
            elif delta > best2:
                best2 = delta

        ans[v] = total + best1

        for to, w in graph[v]:
            if parent[to] != v:
                continue

            even = w - (w & 1)
            child_closed = 0 if even == 0 else even + down_f[to]

            out_f = total - child_closed
            best_delta = best2 if source == to else best1
            out_g = out_f + best_delta

            up_f[to] = out_f
            up_g[to] = out_g

    if R == -1:
        return str(max(ans))
    return str(ans[R])

def run(inp: str) -> str:
    return solve_data(inp).strip()

# Official samples
assert run("""5
1 2 3
1 3 4
1 4 3
1 5 4
0
""") == "14", "sample 1"

assert run("""5
1 2 2
2 3 1
3 4 2
4 5 1
1
""") == "4", "sample 2"

assert run("""7
1 2 1
1 3 1
2 4 9
2 5 9
3 6 9
3 7 9
1
""") == "18", "sample 3"

assert run("""1
1
""") == "0", "sample 4"

# Custom: a single toughness-one edge can be crossed once.
assert run("""2
1 2 1
1
""") == "1", "toughness one"

# Custom: a toughness-one gateway can lead to a large final path.
assert run("""3
1 2 1
2 3 9
1
""") == "10", "one-way gateway"

# Custom: all equal even capacities, with free choice of start.
assert run("""5
1 2 2
1 3 2
1 4 2
1 5 2
0
""") == "8", "all equal capacities"

# Maximum-size test: 100000 vertices, every edge has toughness one.
# Starting at one leaf, Daniel can go through the center to another leaf.
n = 100000
parts = [str(n)]
for v in range(2, n + 1):
    parts.append(f"1 {v} 1")
parts.append("0")
large_input = "\n".join(parts) + "\n"

assert run(large_input) == "2", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`0`| Cây có kích thước tối thiểu không có cạnh | 
|`2 / 1 2 1 / R=1`|`1`| Trường hợp ranh giới trong đó một cạnh chỉ có thể được sử dụng một lần | 
|`3 / 1-2:1, 2-3:9 / R=1`|`10`| Một cạnh dẻo dai dẫn đến một cây con hữu ích | 
| Ngôi sao bốn cạnh, mọi sự dẻo dai`2`,`R=0`|`8`| Năng lực bằng nhau và chọn đỉnh xuất phát tốt nhất | 
| Ngôi sao 100000 đỉnh, tất cả đều dẻo dai`1`,`R=0`|`2`| Tối đa (n), đầu vào lớn và root lại trên nhiều hàng xóm | 

## Vỏ cạnh 

Đối với cây một đỉnh```
1
1
```danh sách kề trống. Cả hai giá trị DP đóng và mở đều bằng 0, vì vậy`answer[0]`là số không. Vì (R=1), chương trình in`0`. Không cần nhánh trường hợp đặc biệt nào ngoài DP thông thường. 

Vì```
2
1 2 1
1
```cạnh duy nhất có mức sử dụng chẵn tốt nhất bằng 0 và cạnh có mức sử dụng lẻ tốt nhất. Bắt đầu từ đỉnh 1, giá trị đóng bằng 0, trong khi giá trị mở qua cạnh là 1. DP chọn hướng mở, tạo ra`1`. Đây chính xác là lý do tại sao độ bền của một cạnh không được loại bỏ một cách đơn giản. 

Vì```
3
1 2 1
2 3 9
1
```cạnh (1-2) không thể được sử dụng như một chuyến tham quan khép kín, do đó đóng góp đóng của nó bằng 0. Đóng góp mở của nó là (1+G(2\to1)). Bên trong thành phần có gốc tại đỉnh 2, cạnh độ bền thứ chín có thể được sử dụng chín lần làm cạnh cuối cùng, cho ra (G(2\to1)=9). Giá trị kết quả ở đỉnh 1 là (1+9=10). DP nắm bắt được thực tế rằng một cạnh vô dụng cho một chuyến đi khứ hồi vẫn có thể cực kỳ có giá trị như một phần của con đường cuối cùng. 

Vì```
3
1 2 3
1 3 3
0
```trung tâm có hai độ dẻo dai-ba cạnh. Nếu bước đi bắt đầu ở giữa và kết thúc ở một chiếc lá, một cạnh có thể được sử dụng ba lần trong khi cạnh kia chỉ có thể được sử dụng hai lần, tức là năm lần. Nếu bước đi bắt đầu ở một lá và kết thúc ở lá kia, thì cả hai cạnh đều nằm trên đường dẫn điểm cuối và mỗi cạnh có thể được sử dụng ba lần, cho ra sáu. Chỉ riêng thao tác từ dưới lên đầu tiên không thể phát hiện ra điều này vì nó có gốc ở đỉnh 1, nhưng thao tác tái tạo lại từ trên xuống sẽ tính toán thông báo từ tâm tới mỗi lá. Kết quả`answer`đối với lá là sáu và (R=0) chọn mức tối đa đó. 

Mẫu thứ ba được cung cấp chứa hai cạnh độ bền - một cạnh ngay bên dưới đỉnh bắt đầu và bốn cạnh độ bền - chín cạnh bên dưới chúng. Một cạnh dẻo dai không thể hỗ trợ một chuyến đi trở về, nhưng nó có thể là cạnh đầu tiên của con đường cuối cùng. Một khi Daniel vượt qua nó, anh ấy có thể sử dụng chín cạnh chín lần và kết thúc ở đó. DP rerooting cho số điểm yêu cầu là`18`, phù hợp với mẫu chính thức.
