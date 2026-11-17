---
title: "CF 1948G - MST có khớp nối"
description: "Chúng ta được cho một đồ thị có trọng số vô hướng được kết nối với tối đa 20 đỉnh. Chúng ta phải chọn một cây bao trùm. Giá của cây đó có hai phần. Phần đầu tiên là tiêu chuẩn, tổng trọng số của cạnh được chọn. Phần thứ hai phụ thuộc vào cấu trúc của cây."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "dsu", "graph-matchings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1948
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 163 (Rated for Div. 2)"
rating: 3100
weight: 1948
solve_time_s: 79
verified: true
draft: false
---

[CF 1948G - MST có tính năng So khớp](https://codeforces.com/problemset/problem/1948/G) 

**Đánh giá:** 3100 
**Thẻ:** bitmasks, lực lượng vũ phu, dsu, khớp đồ thị, cây 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có trọng số vô hướng được kết nối với tối đa 20 đỉnh. Chúng ta phải chọn một cây bao trùm. Giá của cây đó có hai phần. 

Phần đầu tiên là tiêu chuẩn, tổng trọng số của cạnh được chọn. 

Phần thứ hai phụ thuộc vào cấu trúc của cây. Đối với cây bao trùm đã chọn, chúng tôi tính toán kích thước khớp tối đa của nó, nghĩa là số lượng lớn nhất các cạnh rời nhau theo cặp đỉnh có thể được chọn từ cây. Nếu kích thước phù hợp là$k$, chúng tôi trả thêm tiền$k \cdot c$. 

Mục tiêu của chúng ta là tìm cây khung có tổng chi phí nhỏ nhất. 

Đồ thị có số đỉnh nhỏ,$n \le 20$, nhưng có khả năng dày đặc vì mọi cặp đỉnh đều có thể có một cạnh. Một cây bao trùm có$n-1$các cạnh, nhưng số lượng cây bao trùm có thể là rất lớn. Ngay cả đối với đồ thị hoàn chỉnh, công thức Cayley cho$n^{n-2}$cây trải dài. Vì$n=20$, điều đó đã xảy ra rồi$2.6 \times 10^{23}$, hoàn toàn không thể kể hết được. 

Giá trị nhỏ của$n$đề nghị mạnh mẽ rằng giải pháp nên sử dụng mặt nạ bit. Xung quanh sự phức tạp$O(2^n \cdot \text{poly}(n))$là thực tế bởi vì$2^{20}\approx 10^6$. Một cái gì đó giống như$O(2^n n^2)$đại khái là$4\times 10^8$các hoạt động nguyên thủy ở số lượng lý thuyết tồi tệ nhất, nhưng với các hằng số rất nhỏ và việc thực hiện cẩn thận, nó có thể được chấp nhận trong giới hạn 6 giây. Bất cứ điều gì liên quan đến việc liệt kê tất cả các cây hoặc tất cả các kết quả phù hợp bên trong tất cả các cây đều là vô vọng. 

Phần nguy hiểm của vấn đề là thuật ngữ phù hợp. Một ý tưởng đơn giản là tạo ra các cây bao trùm ứng cử viên và tính toán các kết quả phù hợp tối đa của chúng. Kích thước phù hợp phụ thuộc vào toàn bộ cấu trúc cây, vì vậy không rõ cách tối ưu hóa nó cùng với việc xây dựng MST. 

Có một số tình huống dễ bỏ lỡ. 

Xét một đường đi trên bốn đỉnh:```
1 - 2 - 3 - 4
```Kích thước khớp tối đa là 2 vì chúng ta có thể lấy các cạnh$(1,2)$Và$(3,4)$. 

đầu vào:```
4 100
0 1 0 0
1 0 1 0
0 1 0 1
0 0 1 0
```Một giải pháp chỉ giảm thiểu trọng số cạnh sẽ chọn cây này và thu được tổng cạnh$3$, nhưng chi phí thực sự là$3+2\cdot100=203$. 

Một trường hợp tế nhị khác là một ngôi sao:```
    2
    |
3 - 1 - 4
```Kích thước khớp tối đa chỉ là 1 vì mọi cạnh đều chạm vào tâm. 

đầu vào:```
4 100
0 1 1 1
1 0 0 0
1 0 0 0
1 0 0 0
```Trọng lượng cây là 3 và hình phạt tương ứng chỉ là$100$, do đó tổng chi phí là 103. Một giải pháp giả định "nhiều cạnh xung quanh một đỉnh có nghĩa là kết quả khớp lớn hơn" sẽ hoàn toàn sai. 

Bẫy cấu trúc quan trọng nhất là kích thước phù hợp tối đa trong cây không phải là thuộc tính cục bộ. Các cây khác nhau có tổng trọng lượng cạnh giống nhau có thể có các hình phạt khớp khác nhau. Việc tối ưu hóa phải xử lý đồng thời cả hai thuật ngữ. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Liệt kê mọi cây bao trùm của biểu đồ, tính tổng trọng số cạnh của nó, tính kích thước khớp tối đa của nó và giữ lại câu trả lời tối thiểu. 

Điều này hiệu quả vì mục tiêu được xác định trực tiếp trên cây bao trùm. Nếu chúng ta có thể kiểm tra từng cái cây thì sự đúng đắn sẽ có ngay lập tức. 

Vấn đề là số lượng cây bao trùm. Trong một đồ thị đầy đủ có 20 đỉnh có$$20^{18}$$cây bao trùm khác nhau. Ngay cả việc kiểm tra một triệu cây mỗi giây cũng không tạo ra ảnh hưởng gì trong không gian tìm kiếm đó. 

Vì vậy chúng ta cần hiểu thuật ngữ phù hợp một cách sâu sắc hơn. 

Quan sát quan trọng xuất phát từ một định lý cổ điển. 

Cây nào cũng có hai phần. 

Đối với đồ thị lưỡng cực,$$\text{maximum matching} = \text{minimum vertex cover}.$$Đây là định lý Kőnig. 

Thay vì nghĩ đến việc so khớp, hãy nghĩ đến bìa đỉnh. 

Giả sử kích thước phù hợp tối đa của cây cuối cùng là$k$. Khi đó cây có đỉnh có kích thước$k$. 

Đặt đỉnh đó là bìa$S$. 

Vì mọi cạnh của cây đều phải được che phủ nên mỗi cạnh của cây phải có ít nhất một điểm cuối bên trong$S$. 

Điều đó có nghĩa là cây chỉ sử dụng các cạnh có điểm cuối không nằm ngoài$S$. 

Điều này thay đổi hoàn toàn vấn đề. 

Sửa một số tập hợp con$S$. Cho rằng$S$sẽ là đỉnh bao phủ của cây cuối cùng. 

Khi đó mọi cạnh của cây được phép phải thỏa mãn:$$u\in S \quad\text{or}\quad v\in S.$$Nói cách khác, các cạnh giữa hai đỉnh bên ngoài$S$bị cấm. 

Trong số tất cả các cây bao trùm chỉ sử dụng các cạnh như vậy, chúng ta chỉ muốn cây có trọng số tối thiểu. Đó chỉ là vấn đề MST trên biểu đồ bị hạn chế. 

Nếu đồ thị giới hạn đó bị ngắt kết nối thì không tồn tại cây bao trùm cho lựa chọn này.$S$. 

Nếu đồ thị giới hạn được kết nối, cây bao trùm rẻ nhất tương thích với$S$chính xác là MST của nó. 

Tổng chi phí của nó trở thành$$\text{MST weight} + |S|\cdot c.$$Tại sao điều này là đủ? 

Bởi vì nếu một cây có kích thước phù hợp tối đa$k$, thì nó có một số đỉnh có kích thước$k$. Khi chúng ta liệt kê lớp phủ đó, cây trở nên khả thi trong biểu đồ giới hạn tương ứng. MST của đồ thị bị hạn chế đó chỉ có thể rẻ hơn cây đó. Do đó xét mỗi tập con$S$không bao giờ bỏ lỡ sự tối ưu. 

Bây giờ vấn đề trở thành: 

Liệt kê mọi tập hợp con$S$, xây dựng đồ thị chỉ chứa các cạnh chạm nhau$S$, tính trọng lượng MST của nó và giảm thiểu$$\text{MST weight}+|S|c.$$Từ$n\le 20$, có$2^n$tập hợp con. Với mỗi tập con chúng ta có thể chạy thuật toán Prim trong$O(n^2)$. 

Điều đó mang lại$O(2^n n^2)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên cây bao trùm | Số mũ của số cây | Rất lớn | Quá chậm | 
| Liệt kê các bìa đỉnh + MST |$O(2^n n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc biểu đồ và thay thế mọi cạnh bị thiếu bằng một giá trị vô hạn. 
2. Liệt kê mọi bitmask$mask$từ$0$ĐẾN$2^n-1$. 
3. Giải thích các bit đã đặt của$mask$như một bìa đỉnh ứng cử viên$S$. 
4. Xây dựng đồ thị giới hạn ngầm trong đó một cạnh$(u,v)$chỉ được phép nếu có ít nhất một điểm cuối thuộc về$S$. 

Một cạnh có điểm cuối đều ở bên ngoài$S$không thể xuất hiện trong một cái cây được bao phủ bởi$S$. 
5. Chạy thuật toán Prim trên đồ thị giới hạn này. 

Trong khi nới lỏng các cạnh, bỏ qua mọi cạnh có hai điểm cuối đều ở bên ngoài$S$. 
6. Nếu Prim không thể tới được tất cả các đỉnh thì đồ thị giới hạn bị ngắt kết nối. 

Trong trường hợp đó tập hợp con này$S$không thể tạo ra cây bao trùm nên hãy loại bỏ nó. 
7. Nếu không thì lấy trọng lượng MST$W$. 
8. Tính toán$$W + c \cdot \text{popcount}(mask).$$1. Thu nhỏ câu trả lời trên tất cả các mặt nạ. 

### Tại sao nó hoạt động 

Đối với bất kỳ cây bao trùm nào$T$, cho phép$k$là kích thước phù hợp tối đa của nó. 

Vì mọi cây đều có hai phần nên định lý Kőnig cho ta một bìa đỉnh$S$kích thước$k$. 

Mỗi cạnh của$T$có ít nhất một điểm cuối trong$S$, Vì thế$T$hoàn toàn được chứa bên trong đồ thị giới hạn được xác định bởi$S$. 

MST của đồ thị giới hạn đó có trọng số lớn nhất bằng$T$, bởi vì$T$là một cây khung hợp lệ của cùng một đồ thị. 

Do đó đối với tập hợp con cụ thể này$S$,$$\text{MST}(S)+|S|c \le \text{weight}(T)+kc.$$Do đó, chi phí của cây tối ưu là ít nhất một giá trị được bảng liệt kê của chúng tôi xem xét. 

Ngược lại, với mọi tập con$S$, bất kỳ cây bao trùm nào của đồ thị bị giới hạn đều có tất cả các cạnh được bao phủ bởi$S$. Vì thế$S$là bìa đỉnh của cây đó, vì vậy kích thước phù hợp tối đa của nó nhiều nhất là$|S|$. Chi phí chúng tôi tính toán,$$\text{MST}(S)+|S|c,$$có thể đạt được bằng chính MST và không bao giờ đánh giá thấp mục tiêu thực sự. 

Hai hướng cùng nhau cho thấy rằng giá trị tối thiểu trên tất cả các tập hợp con chính xác là câu trả lời. 

Việc giảm từ khớp đến che phủ đỉnh là cầu nối quan trọng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10 ** 18

def prim(mask, n, g):
    dist = [INF] * n
    used = [False] * n
    dist[0] = 0

    total = 0

    for _ in range(n):
        v = -1

        for i in range(n):
            if not used[i] and dist[i] < INF:
                if v == -1 or dist[i] < dist[v]:
                    v = i

        if v == -1:
            return INF

        used[v] = True
        total += dist[v]

        for to in range(n):
            if used[to]:
                continue

            if ((mask >> v) & 1) == 0 and ((mask >> to) & 1) == 0:
                continue

            w = g[v][to]
            if w < dist[to]:
                dist[to] = w

    return total

def solve():
    n, c = map(int, input().split())

    g = [[INF] * n for _ in range(n)]

    for i in range(n):
        row = list(map(int, input().split()))
        for j in range(n):
            if i == j:
                g[i][j] = INF
            elif row[j] != 0:
                g[i][j] = row[j]

    ans = INF

    for mask in range(1 << n):
        mst_cost = prim(mask, n, g)
        if mst_cost == INF:
            continue

        cur = mst_cost + c * mask.bit_count()
        if cur < ans:
            ans = cur

    print(ans)

solve()
```Việc thực hiện tuân theo bằng chứng gần như theo nghĩa đen. 

chức năng`prim`tính toán trọng số MST bên trong biểu đồ giới hạn tương ứng với mặt nạ. Thay vì xây dựng một biểu đồ mới cho mỗi tập hợp con một cách rõ ràng, nó sẽ kiểm tra giới hạn trong quá trình thư giãn. Một cạnh có thể sử dụng được nếu có ít nhất một điểm cuối thuộc về mặt nạ. 

Trở về`INF`khi không tồn tại đỉnh có thể truy cập chưa được truy cập là quan trọng. Đó là cách chúng tôi phát hiện ra rằng đồ thị bị hạn chế bị ngắt kết nối. 

Đồ thị sử dụng`INF`đối với các cạnh không tồn tại. Điều này khiến Prim bỏ qua chúng một cách tự nhiên vì những giá trị như vậy không bao giờ cải thiện được khoảng cách. 

biểu hiện```
((mask >> v) & 1) == 0 and ((mask >> to) & 1) == 0
```kiểm tra xem cả hai điểm cuối có nằm ngoài bìa đỉnh ứng cử viên hay không. Các cạnh như vậy bị cấm và bị bỏ qua. 

của Python`bit_count()`đưa ra kích thước của tập hợp con, tương ứng chính xác với hình phạt che phủ đỉnh. 

Một điểm tinh tế là cho phép mặt nạ trống. Nó đơn giản có nghĩa là không có đỉnh nào được chọn. Thông thường, biểu đồ bị hạn chế trở nên mất kết nối, nhưng trong các biểu đồ rất nhỏ, việc kiểm tra nó một cách thống nhất vẫn có giá trị. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 10
0 1 8 0
1 0 1 0
8 1 0 2
0 0 2 0
```Mặt nạ tối ưu là`{3}`trong lập chỉ mục dựa trên 1. 

| Bước | Mặt nạ | Trọng lượng MST được phép | Kích thước bìa | Tổng chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | {3} | 11 | 1 | 21 | 
| 2 | {2,3} | 4 | 2 | 24 | 
| 3 | {1,2,3} | 4 | 3 | 34 | 

Giá trị tối thiểu là 21. 

Cây tương ứng sử dụng các cạnh$(1,3)$,$(2,3)$,$(3,4)$. Mọi cạnh đều chạm vào đỉnh 3, do đó lớp phủ được chọn có kích thước 1. Hình phạt phù hợp trở nên nhỏ hơn nhiều so với MST hình đường dẫn. 

### Mẫu 2 

đầu vào:```
4 5
0 1 8 0
1 0 1 0
8 1 0 2
0 0 2 0
```| Bước | Mặt nạ | Trọng lượng MST được phép | Kích thước bìa | Tổng chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | {3} | 11 | 1 | 16 | 
| 2 | {2,3} | 4 | 2 | 14 | 
| 3 | {1,2,3} | 4 | 3 | 19 | 

Bây giờ hệ số phạt đã nhỏ hơn nên việc trả tiền cho một kết quả phù hợp lớn hơn sẽ trở nên đáng giá. 

Câu trả lời tối ưu là 14, đạt được bằng đường dẫn$1-2-3-4$. 

Ví dụ này thể hiện sự đánh đổi. Đôi khi chúng ta bỏ thêm tiền phạt phù hợp để có được một cây rẻ hơn nhiều. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(2^n n^2)$| Liệt kê tất cả các mặt nạ và chạy Prim trong$O(n^2)$| 
| Không gian |$O(n^2)$| Lưu trữ ma trận đồ thị | 

Với$n=20$, có$2^{20}=1,048,576$mặt nạ. Bản thân đồ thị rất nhỏ. Giải pháp dự định dựa vào số lượng đỉnh nhỏ và vừa vặn trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    INF = 10 ** 18

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, c = map(int, input().split())

    g = [[INF] * n for _ in range(n)]

    for i in range(n):
        row = list(map(int, input().split()))
        for j in range(n):
            if i != j and row[j] != 0:
                g[i][j] = row[j]

    def prim(mask):
        dist = [INF] * n
        used = [False] * n
        dist[0] = 0

        total = 0

        for _ in range(n):
            v = -1

            for i in range(n):
                if not used[i] and dist[i] < INF:
                    if v == -1 or dist[i] < dist[v]:
                        v = i

            if v == -1:
                return INF

            used[v] = True
            total += dist[v]

            for to in range(n):
                if used[to]:
                    continue

                if ((mask >> v) & 1) == 0 and ((mask >> to) & 1) == 0:
                    continue

                if g[v][to] < dist[to]:
                    dist[to] = g[v][to]

        return total

    ans = INF

    for mask in range(1 << n):
        cur = prim(mask)
        if cur != INF:
            ans = min(ans, cur + c * mask.bit_count())

    return str(ans) + "\n"

# provided samples
assert run(
"""4 10
0 1 8 0
1 0 1 0
8 1 0 2
0 0 2 0
"""
) == "21\n", "sample 1"

assert run(
"""4 5
0 1 8 0
1 0 1 0
8 1 0 2
0 0 2 0
"""
) == "14\n", "sample 2"

# minimum size
assert run(
"""2 7
0 3
3 0
"""
) == "10\n", "n = 2"

# star graph, matching size should be 1
assert run(
"""4 100
0 1 1 1
1 0 0 0
1 0 0 0
1 0 0 0
"""
) == "103\n", "star structure"

# all equal weights
assert run(
"""3 5
0 2 2
2 0 2
2 2 0
"""
) == "9\n", "all equal weights"

# path graph
assert run(
"""4 1
0 1 0 0
1 0 1 0
0 1 0 1
0 0 1 0
"""
) == "5\n", "matching size 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 đỉnh, một cạnh | 10 | Đồ thị pháp lý nhỏ nhất | 
| Đồ thị sao | 103 | Kết quả phù hợp tối đa bằng 1 | 
| Tất cả trọng lượng bằng nhau | 9 | Lựa chọn đối xứng | 
| Con đường đơn giản | 5 | Kích thước phù hợp đạt 2 | 
| Mẫu chính thức | 21, 14 | Tính đúng đắn so với các ví dụ về tuyên bố | 

## Vỏ cạnh 

Hãy xem xét đồ thị ngôi sao:```
4 100
0 1 1 1
1 0 0 0
1 0 0 0
1 0 0 0
```Một sai lầm phổ biến là cho rằng hình phạt phù hợp sẽ tăng theo cấp độ của trung tâm. Trong thực tế, mọi cạnh đều có chung một đỉnh nên chỉ có một cạnh có thể thuộc về một cạnh phù hợp. 

Thuật toán xử lý vấn đề này thông qua công thức che phủ đỉnh. Chỉ chọn tâm làm bìa sẽ có kích thước mặt nạ là 1. Đồ thị bị giới hạn đã chứa tất cả các cạnh của cây, do đó Prim trả về trọng số 3 và câu trả lời trở thành$$3 + 1\cdot100 = 103.$$Bây giờ hãy xem xét đường dẫn:```
4 100
0 1 0 0
1 0 1 0
0 1 0 1
0 0 1 0
```Kích thước khớp tối đa là 2. Bìa đỉnh tối thiểu cũng có kích thước 2, cụ thể là đỉnh 2 và 3. 

Khi thuật toán liệt kê mặt nạ đó, mọi cạnh của đường dẫn đều được phép. Prim đạt được trọng lượng cây là 3, tạo ra$$3 + 2\cdot100 = 203.$$Không có mặt nạ có kích thước 1 nào có thể bao phủ cả ba cạnh, vì vậy thuật toán bác bỏ chính xác giả định không thể chấp nhận được rằng hình phạt phù hợp có thể là 100. 

Cuối cùng, hãy xem xét một mặt nạ ngắt kết nối biểu đồ.```
4 1
0 1 1 1
1 0 1 1
1 1 0 1
1 1 1 0
```Lấy mặt nạ`{1}`. 

Các cạnh giữa các đỉnh`{2,3,4}`đều bị cấm vì cả hai điểm cuối đều không thuộc về trang bìa. Đồ thị bị giới hạn trở thành ngôi sao có tâm ở đỉnh 1, đỉnh này vẫn được kết nối. 

Thay vào đó, nếu chúng ta lấy mặt nạ trống, mọi cạnh sẽ bị cấm. Prim không thể đạt tới tất cả các đỉnh và quay trở lại`INF`. Thuật toán sẽ tự động loại bỏ tập hợp con đó. 

Việc kiểm tra kết nối này là cần thiết. Nhiều mặt nạ không tương ứng với bất kỳ cây bao trùm hợp lệ nào và việc coi chúng là hợp lệ sẽ tạo ra câu trả lời sai.
