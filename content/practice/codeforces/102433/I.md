---
title: "CF 102433I - Sửa lỗi"
description: "Chúng tôi có một tập hợp (N) từ riêng biệt. Mỗi từ sử dụng chính xác cùng một bộ chữ cái, vì vậy mỗi từ chỉ đơn giản là một hoán vị khác nhau của cùng một chữ cái. Không có chữ cái nào xuất hiện hai lần trong một từ."
date: "2026-08-12T07:36:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102433
codeforces_index: "I"
codeforces_contest_name: "2019-2020 ACM-ICPC Pacific Northwest Regional Contest (Div. 1)"
rating: 0
weight: 102433
solve_time_s: 90
verified: true
draft: false
---

[CF 102433I - Sửa lỗi](https://codeforces.com/problemset/problem/102433/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một tập hợp (N) từ riêng biệt. Mỗi từ sử dụng chính xác cùng một bộ chữ cái, vì vậy mỗi từ chỉ đơn giản là một hoán vị khác nhau của cùng một chữ cái. Không có chữ cái nào xuất hiện hai lần trong một từ. 

Hai từ xung đột nếu một từ có thể thu được từ từ kia bằng cách hoán đổi chính xác hai vị trí. Hai vị trí không cần phải liền kề nhau. Chúng ta cần giữ lại càng nhiều từ càng tốt trong khi vẫn đảm bảo mọi cặp từ được giữ lại không bị xung đột. 

Một cách hữu ích để suy nghĩ về vấn đề là sử dụng biểu đồ. Mỗi từ là một đỉnh và hai đỉnh được kết nối khi các từ của chúng khác nhau bởi một lần hoán đổi. Câu trả lời là tập hợp độc lập tối đa của đồ thị này. 

Ràng buộc (N \le 500) đủ nhỏ cho các thuật toán đồ thị có thời gian chạy khoảng (O(N^2)), nhưng nó quá lớn để liệt kê tất cả các tập hợp con. Có thể có (2^{500}) tập hợp con, do đó việc tìm kiếm tập hợp độc lập tối đa trực tiếp là hoàn toàn không khả thi. Các từ chỉ chứa các chữ cái viết thường, do đó độ dài của chúng tối đa là 26, vì không có chữ cái nào có thể xuất hiện hai lần. Độ dài từ nhỏ này mang lại cho chúng ta một giới hạn hữu ích khác: từ bất kỳ từ nào cũng có nhiều nhất (\binom{26}{2}=325) kết quả hoán đổi đơn khác nhau. 

Có một số trường hợp nghiêm trọng mà việc triển khai bất cẩn có thể dẫn đến xử lý sai. Ví dụ: nếu (N=1), đầu vào```
1
a
```có đáp án 1. Không có từ nào khác đi kèm`a`có thể xung đột. Một giải pháp giả sử mọi đỉnh đều có một đỉnh lân cận đối diện có thể trả về 0 không chính xác. 

Một từ có độ dài bằng hai chứng tỏ rằng việc hoán đổi vị trí liền kề không phải là một hoạt động đặc biệt. Vì```
2
ab
ba
```câu trả lời là 1 vì hai từ được kết nối bằng cách hoán đổi hai vị trí duy nhất của chúng. Một giải pháp chỉ kiểm tra một số nhóm hoán đổi bị hạn chế sẽ bỏ lỡ cạnh này. 

Trường hợp tinh tế khác là khi có nhiều từ tồn tại nhưng không có từ nào trong số chúng có thể hoán đổi một cách riêng biệt. Ví dụ,```
3
abc
bca
cab
```có câu trả lời 3. Cả ba hoán vị đều có cùng một hoán vị chẵn lẻ, trong khi một hoán đổi luôn thay đổi tính chẵn lẻ. Một giải pháp giả sử đồ thị phải chứa nhiều cạnh sẽ loại bỏ các đỉnh một cách không cần thiết. 

Cuối cùng, thực tế là tất cả các từ đều là những vấn đề độc nhất. Chúng ta không bao giờ cần phải xử lý một cạnh từ một từ tới chính nó. Hoán đổi hai vị trí riêng biệt sẽ làm thay đổi từ vì mỗi chữ cái đều khác biệt. 

Kết quả đầu ra mẫu chính thức là 3 cho Mẫu 1, 8 cho Mẫu 2 và 4 cho Mẫu 3. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực trực tiếp nhất là xây dựng biểu đồ xung đột và sau đó thử từng tập hợp con các từ, kiểm tra xem tập hợp con đó có chứa cặp xung đột hay không. Điều này đúng vì mọi câu trả lời có thể có của ứng viên đều được xem xét rõ ràng. Đối với một tập hợp con gồm (N) đỉnh, việc kiểm tra tất cả các cặp có giá (O(N^2)) và có (2^N) tập hợp con, cho (O(2^N N^2)) thời gian trong trường hợp xấu nhất. Với (N=500), đây là kiểm tra cặp (2^{500}\cdot250000), điều này không thực tế chút nào. 

Ngay cả việc xây dựng biểu đồ bằng cách so sánh từng cặp từ cũng đã có (O(N^2L)), trong đó (L\le26). Ở mức giới hạn tối đa có nghĩa là lên đến 

[ 
\binom{500}{2}\cdot26 = 3.243.500 
] 

so sánh ký tự trong trường hợp xấu nhất. Phần đó có thể quản lý được, nhưng vấn đề tập hợp độc lập tối đa chung vẫn theo cấp số nhân. 

Quan sát quan trọng là mọi cạnh đều có tác động rất cụ thể đến tính chẵn lẻ hoán vị. Gán cho mỗi từ một sự chẵn lẻ tùy theo hoán vị của các chữ cái là chẵn hay lẻ. Hoán đổi hai vị trí sẽ thay đổi tính chẵn lẻ hoán vị chính xác một lần, do đó, mọi cạnh xung đột đều kết nối một từ chẵn với một từ lẻ. 

Do đó, biểu đồ xung đột là lưỡng cực. 

Điều này thay đổi vấn đề hoàn toàn. Đối với đồ thị hai bên, kích thước của tập hợp độc lập tối đa là 

[ 
|V|-\text{độ che phủ đỉnh tối thiểu}. 
] 

Theo định lý König, bìa đỉnh tối thiểu trong biểu đồ hai bên có cùng kích thước với kết quả khớp tối đa. Vì vậy câu trả lời chỉ đơn giản là 

[ 
N-\text{khớp tối đa}. 
] 

Một quan sát hữu ích khác là chúng ta không cần so sánh từng cặp từ để tìm ra các cạnh. Từ một từ có độ dài (L), mọi hàng xóm có thể hoán đổi một lần có thể được tạo ra bằng cách chọn hai vị trí và hoán đổi chúng. Chỉ có (\binom L2) ứng cử viên như vậy. Bảng băm cho chúng ta biết trong thời gian dự kiến ​​không đổi liệu từ kết quả có thực sự xuất hiện trong đầu vào hay không. 

Lực lượng vũ phu hoạt động vì việc kiểm tra từng cặp sẽ đưa ra biểu đồ xung đột chính xác, nhưng không thành công khi chúng ta cố gắng giải trực tiếp tập độc lập tối đa. Quan sát tính chẵn lẻ biến biểu đồ đó thành biểu đồ lưỡng cực, trong đó việc so khớp sẽ đưa ra câu trả lời trong thời gian đa thức. Việc tạo ra các hàng xóm bằng cách hoán đổi thực tế cũng tránh được việc so sánh từ (N^2) không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^N N^2)) | (O(N^2)) | Quá chậm | 
| Tối ưu | (O(NL^2 + E\sqrt N)) | (O(NL^2 + N)) | Đã chấp nhận | 

Ở đây (L\le26) là độ dài từ và (E) là số cạnh xung đột. Vì một đỉnh có nhiều nhất (\binom L2\le325) lân cận, (E\le O(NL^2)). 

## Hướng dẫn thuật toán 

1. Lưu trữ mọi từ đầu vào trong từ điển ánh xạ từ đó tới chỉ mục đỉnh của nó. Điều này cho phép chúng tôi kiểm tra xem từ một lần hoán đổi được tạo có thuộc về đầu vào trong thời gian không đổi dự kiến ​​hay không. 
2. Chọn các chữ cái của từ đầu tiên theo thứ tự sắp xếp và gán thứ hạng cho mỗi chữ cái. Thay thế mỗi từ bằng chuỗi cấp bậc tương ứng. Vì tất cả các từ đều chứa chính xác các chữ cái riêng biệt giống nhau nên mỗi từ bây giờ là một hoán vị của cùng một chuỗi thứ hạng. 
3. Tính tính chẵn lẻ của mỗi hoán vị bằng cách đếm các nghịch đảo. Một từ có số đảo ngược chẵn thuộc về bên trái của biểu đồ hai bên và một từ có số lẻ thuộc về bên phải. 
4. Với mỗi từ chẵn lẻ, hãy thử từng cặp vị trí (i<j). Hoán đổi hai ký tự đó và tra từ kết quả trong từ điển. Nếu nó tồn tại, hãy thêm đỉnh tương ứng vào danh sách kề của từ hiện tại.

Việc hoán đổi hai vị trí bất kỳ sẽ làm thay đổi tính chẵn lẻ của hoán vị, do đó, một vị trí lân cận được tạo ra sẽ tự động ở phía đối diện. Chúng tôi chỉ tạo các cạnh từ một phía, cung cấp biểu diễn tiêu chuẩn cần thiết cho việc so khớp hai bên. 
5. Chạy Hopcroft-Karp trên biểu đồ hai bên này. Duy trì sự khớp cho mọi đỉnh bên trái và mọi đỉnh bên phải. Tìm kiếm theo chiều rộng sẽ tìm thấy các lớp đường dẫn tăng cường có thể có và tìm kiếm theo chiều sâu sẽ gửi một số đường dẫn tăng cường qua các lớp đó. 
6. Đặt kích thước phù hợp kết quả là (M). Trở lại (N-M). Định lý König nói rằng (M) cũng là kích thước của một bìa đỉnh tối thiểu, và việc loại bỏ một bìa như vậy sẽ để lại một tập hợp độc lập gồm chính xác các đỉnh (N-M). 

### Tại sao nó hoạt động 

Bất biến trung tâm là mọi cạnh xung đột đều nối các từ có tính chẵn lẻ hoán vị ngược lại. Một chuyển vị đơn lẻ làm thay đổi tính chẵn lẻ của một hoán vị, do đó biểu đồ xung đột là lưỡng cực với tính chẵn lẻ là hai cạnh của nó. 

Trong bất kỳ biểu đồ nào, phần bù của một bìa đỉnh là một tập hợp độc lập, do đó, bìa đỉnh tối thiểu có kích thước (C) sẽ cho một tập hợp kích thước độc lập (N-C). Ngược lại, phần bù của bất kỳ tập độc lập nào đều là bìa đỉnh, vì vậy tập độc lập lớn nhất có kích thước chính xác (N-C), trong đó (C) là kích thước bìa đỉnh tối thiểu. 

Đối với đồ thị hai bên, định lý König cho (C=M), trong đó (M) là kích thước khớp tối đa. Thuật toán xây dựng chính xác biểu đồ xung đột, tìm kết quả khớp tối đa và trả về (N-M), do đó đây là tập hợp không có trao đổi lớn nhất có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    words = [input().strip() for _ in range(n)]

    index = {word: i for i, word in enumerate(words)}

    # Every word is a permutation of the same distinct letters.
    base = sorted(words[0])
    rank = {ch: i for i, ch in enumerate(base)}

    parity = [0] * n
    left = []

    for u, word in enumerate(words):
        a = [rank[ch] for ch in word]
        inv_parity = 0

        for i in range(len(a)):
            for j in range(i + 1, len(a)):
                inv_parity ^= (a[i] > a[j])

        parity[u] = inv_parity
        if inv_parity == 0:
            left.append(u)

    # adj[u] contains only vertices on the odd-parity side.
    adj = [[] for _ in range(n)]

    length = len(words[0])

    for u in left:
        s = list(words[u])

        for i in range(length):
            for j in range(i + 1, length):
                s[i], s[j] = s[j], s[i]
                v = index.get(''.join(s))

                if v is not None:
                    adj[u].append(v)

                s[i], s[j] = s[j], s[i]

    # Hopcroft-Karp maximum matching.
    pair_u = [-1] * n
    pair_v = [-1] * n
    dist = [-1] * n

    from collections import deque

    def bfs():
        q = deque()
        found = False

        for u in left:
            if pair_u[u] == -1:
                dist[u] = 0
                q.append(u)
            else:
                dist[u] = -1

        while q:
            u = q.popleft()

            for v in adj[u]:
                pu = pair_v[v]

                if pu == -1:
                    found = True
                elif dist[pu] == -1:
                    dist[pu] = dist[u] + 1
                    q.append(pu)

        return found

    sys.setrecursionlimit(2000)

    def dfs(u):
        for v in adj[u]:
            pu = pair_v[v]

            if pu == -1 or (
                dist[pu] == dist[u] + 1 and dfs(pu)
            ):
                pair_u[u] = v
                pair_v[v] = u
                return True

        dist[u] = -1
        return False

    matching = 0

    while bfs():
        for u in left:
            if pair_u[u] == -1 and dfs(u):
                matching += 1

    print(n - matching)

if __name__ == "__main__":
    solve()
```Từ điển`index`là cầu nối giữa việc tạo hoán vị và xây dựng đồ thị. Sau khi hoán đổi hai ký tự trong một từ,`index.get()`ngay lập tức cho chúng ta biết từ đó có thuộc tập hợp đã cho hay không. 

Tính toán đảo ngược chỉ sử dụng tính chẵn lẻ, không sử dụng số lần đảo ngược đầy đủ. Thao tác XOR là đủ vì chỉ số lẻ hay số chẵn mới quan trọng. Vì độ dài từ nhiều nhất là 26 nên phép tính bậc hai đơn giản khá nhỏ. 

Danh sách tạm thời`s`được sửa đổi tại chỗ cho mỗi lần hoán đổi ứng viên. Các ký tự được hoán đổi trở lại ngay sau khi tra cứu từ điển, vì vậy mọi cặp vị trí đều bắt đầu từ từ gốc. Việc quên hoán đổi lần thứ hai sẽ khiến các ứng viên sau phụ thuộc vào các ứng viên trước đó và làm hỏng biểu đồ. 

Các mảng phù hợp có kích thước (N), mặc dù chỉ các đỉnh ở phía chẵn lẻ thích hợp mới được lặp lại thành các đỉnh bên trái. Việc sử dụng các chỉ mục đỉnh ban đầu sẽ giữ cho từ điển, danh sách kề và các mảng khớp nhất quán. 

BFS xây dựng các lớp khoảng cách từ tất cả các đỉnh bên trái hiện chưa được so sánh. DFS chỉ đi theo các cạnh tôn trọng các lớp đó, vì vậy mỗi DFS thành công sẽ tăng cường kết quả khớp dọc theo đường tăng cường ngắn nhất có sẵn. Khi BFS không còn có thể tìm thấy điểm cuối bên phải chưa từng có thể truy cập được từ đỉnh bên trái miễn phí, thì không còn đường dẫn tăng cường nào và mức độ khớp là tối đa. 

Không có số nguyên nào có thể tràn trong Python và câu trả lời luôn nằm trong khoảng từ 0 đến 500. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Sáu từ đều là hoán vị của`abc`. Tính chẵn lẻ nghịch đảo của chúng chia chúng thành hai nhóm. 

| Lời | Chẵn lẻ | Hàng xóm xung đột được tạo ra | Trạng thái phù hợp | 
| --- | --- | --- | --- | 
|`abc`| thậm chí |`acb`,`bac`,`cba`| phù hợp với`acb`| 
|`acb`| lẻ |`abc`,`cab`,`bca`| phù hợp với`abc`| 
|`cab`| thậm chí |`cba`,`abc`,`bca`| phù hợp với`cba`| 
|`cba`| lẻ |`cab`,`bac`,`abc`| phù hợp với`cab`| 
|`bac`| thậm chí |`bca`,`cba`,`acb`| chưa từng có | 
|`bca`| lẻ |`bac`,`acb`,`cab`| chưa từng có | 

Kết quả khớp tối đa có kích thước 3. Biểu đồ được cân bằng giữa hai lớp chẵn lẻ và có thể chọn ba cạnh xung đột rời nhau. Do đó, bộ không có trao đổi tối đa có kích thước (6-3=3), khớp với đầu ra mẫu. 

Dấu vết thể hiện tính bất biến chẵn lẻ trung tâm. Mọi hàng xóm được liệt kê đều nằm ở phía đối diện, vì vậy biểu đồ xung đột ban đầu thực sự là lưỡng cực. 

### Mẫu 2 

Đối với mười một`alerts`hoán vị, sự phân chia chẵn lẻ không đồng đều. Thuật toán so khớp không cần tìm một tập hợp độc lập một cách rõ ràng. Nó chỉ cần tìm xem phải loại bỏ bao nhiêu đỉnh để loại bỏ mọi xung đột. 

| Số lượng | Giá trị | 
| --- | --- | 
| Số từ | 11 | 
| Kết hợp tối đa | 3 | 
| Bìa đỉnh tối thiểu | 3 | 
| Bộ miễn phí trao đổi tối đa | 8 | 

Việc so khớp chứa ba cạnh xung đột rời nhau. Sau khi loại bỏ bìa đỉnh tối thiểu gồm ba đỉnh, tám từ còn lại không chứa cạnh xung đột. Vì vậy, câu trả lời là (11-3=8), phù hợp với mẫu chính thức. 

Ví dụ này cho thấy tại sao chỉ lấy lớp chẵn lẻ nhỏ hơn không phải lúc nào cũng đủ. Một tập hợp con tùy ý của một chẵn lẻ luôn không có khả năng hoán đổi, nhưng tối ưu có thể chứa các đỉnh từ cả hai lớp chẵn lẻ khi biểu đồ xung đột không khớp với tất cả một bên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(NL^2 + E\sqrt N)) | Mỗi từ tạo ra (O(L^2)) hoán đổi và Hopcroft-Karp nhận (O(E\sqrt N)) | 
| Không gian | (O(NL^2 + N)) | Đồ thị kề lưu trữ tối đa (O(NL^2)) cạnh | 

Ở đây (N\le500) và (L\le26). Một đỉnh có tối đa 325 đỉnh lân cận có thể hoán đổi một lần, do đó biểu đồ có nhiều nhất khoảng (81{,}250) mục kề cận có hướng trong biểu diễn một phía. Điều này dễ dàng nằm trong giới hạn dự định, trong khi pha so khớp là đa thức và tránh tìm kiếm theo cấp số nhân (2^N). 

## Trường hợp thử nghiệm```python
import sys
import io
from itertools import permutations

def solve():
    n = int(input())
    words = [input().strip() for _ in range(n)]

    index = {word: i for i, word in enumerate(words)}

    base = sorted(words[0])
    rank = {ch: i for i, ch in enumerate(base)}

    parity = [0] * n
    left = []

    for u, word in enumerate(words):
        a = [rank[ch] for ch in word]
        p = 0

        for i in range(len(a)):
            for j in range(i + 1, len(a)):
                p ^= (a[i] > a[j])

        parity[u] = p
        if p == 0:
            left.append(u)

    length = len(words[0])
    adj = [[] for _ in range(n)]

    for u in left:
        s = list(words[u])

        for i in range(length):
            for j in range(i + 1, length):
                s[i], s[j] = s[j], s[i]

                v = index.get(''.join(s))
                if v is not None:
                    adj[u].append(v)

                s[i], s[j] = s[j], s[i]

    from collections import deque

    pair_u = [-1] * n
    pair_v = [-1] * n
    dist = [-1] * n

    def bfs():
        q = deque()
        found = False

        for u in left:
            if pair_u[u] == -1:
                dist[u] = 0
                q.append(u)
            else:
                dist[u] = -1

        while q:
            u = q.popleft()

            for v in adj[u]:
                pu = pair_v[v]

                if pu == -1:
                    found = True
                elif dist[pu] == -1:
                    dist[pu] = dist[u] + 1
                    q.append(pu)

        return found

    def dfs(u):
        for v in adj[u]:
            pu = pair_v[v]

            if pu == -1 or (
                dist[pu] == dist[u] + 1 and dfs(pu)
            ):
                pair_u[u] = v
                pair_v[v] = u
                return True

        dist[u] = -1
        return False

    matching = 0

    while bfs():
        for u in left:
            if pair_u[u] == -1 and dfs(u):
                matching += 1

    print(n - matching)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

assert run("""6
abc
acb
cab
cba
bac
bca
""") == "3\n", "sample 1"

assert run("""11
alerts
alters
artels
estral
laster
ratels
salter
slater
staler
stelar
talers
""") == "8\n", "sample 2"

assert run("""6
ates
east
eats
etas
sate
teas
""") == "4\n", "sample 3"

assert run("""1
a
""") == "1\n", "minimum size"

assert run("""2
ab
ba
""") == "1\n", "single conflict edge"

assert run("""3
abc
bca
cab
""") == "3\n", "all vertices have the same parity"

# Maximum N = 500.
# Every selected permutation is even, so no two selected words can be
# connected by one swap. The answer must be 500.
even_words = []

for p in permutations("abcdefgh"):
    inv = 0
    for i in range(8):
        for j in range(i + 1, 8):
            inv += p[i] > p[j]

    if inv % 2 == 0:
        even_words.append(''.join(p))

    if len(even_words) == 500:
        break

max_input = "500\n" + "\n".join(even_words) + "\n"
assert run(max_input) == "500\n", "maximum N"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / a`| 1 | Tối thiểu (N), độ dài từ 1 và một đỉnh bị cô lập | 
|`2 / ab, ba`| 1 | Biểu đồ xung đột nhỏ nhất có thể và khả năng phát hiện một lần hoán đổi chính xác | 
|`3 / abc, bca, cab`| 3 | Tất cả các đỉnh có cùng tính chẵn lẻ và không có cạnh xung đột | 
| 500 hoán vị chẵn của`abcdefgh`| 500 | Tối đa (N), đầu vào đồ thị lớn và thực tế là các từ giống nhau luôn tương thích | 

## Vỏ cạnh 

Trường hợp kích thước tối thiểu là```
1
a
```Thuật toán đặt từ duy nhất ở một bên chẵn lẻ, không tạo ra sự hoán đổi hữu ích vì từ đó có độ dài bằng 1 và thu được kết quả khớp có kích thước bằng 0. Giá trị trả về là (1-0=1). 

Xung đột nhỏ nhất có thể xảy ra là```
2
ab
ba
```từ`ab`thậm chí còn có tính chẵn lẻ và`ba`có tính chẵn lẻ. Hoán đổi vị trí 0 và 1 trong`ab`sản xuất`ba`, do đó đồ thị có một cạnh. Hopcroft-Karp khớp hai đỉnh đó, cho ra kết quả phù hợp với kích thước 1 và câu trả lời (2-1=1). 

Một trường hợp có nhiều từ nhưng không có xung đột```
3
abc
bca
cab
```Cả ba hoán vị đều chẵn. Mỗi lần hoán đổi đơn lẻ sẽ thay đổi tính chẵn lẻ, vì vậy không từ nào trong số ba từ này có thể được chuyển đổi thành một từ được liệt kê khác bằng cách sử dụng một lần hoán đổi. Biểu đồ kề trống, kết quả khớp có kích thước bằng 0 và thuật toán trả về 3. 

Trường hợp tối đa-(N) sử dụng 500 hoán vị chẵn của`abcdefgh`. Mọi hàng xóm một lần hoán đổi được tạo ra đều là số lẻ, nhưng không có từ lẻ nào xuất hiện trong đầu vào. Do đó, đồ thị kề vẫn trống mặc dù có 500 đỉnh. Kết quả khớp vẫn bằng 0 và câu trả lời là 500. Điều này phát hiện các triển khai vô tình cho rằng đầu vào lớn phải chứa xung đột. 

Ranh giới chiều dài cũng được xử lý một cách tự nhiên. Một từ có thể chứa tối đa 26 chữ cái vì các chữ cái không thể lặp lại và chỉ có chữ cái tiếng Anh viết thường. Thuật toán thử tất cả các cặp (\binom L2), do đó, với (L=26), nó chỉ thực hiện 325 lần hoán đổi mỗi từ. Không có vấn đề riêng biệt nào xung quanh ký tự cuối cùng vì cả hai vòng lặp đều sử dụng`range(length)`và yêu cầu (i<j).
