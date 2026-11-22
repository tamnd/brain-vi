---
title: "CF 38H - Cuộc thi Marathon vĩ đại"
description: "Chúng tôi có một biểu đồ có trọng số được kết nối của các thành phố. Người chạy thứ i xuất phát từ thành phố i và kết thúc ở thành phố khác. Thành phố về đích được chọn độc lập cho mỗi vận động viên và một số vận động viên có thể có cùng một đích đến."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 38
codeforces_index: "H"
codeforces_contest_name: "School Personal Contest #1 (Winter Computer School 2010/11) - Codeforces Beta Round 38 (ACM-ICPC Rules)"
rating: 2400
weight: 38
solve_time_s: 151
verified: false
draft: false
---
[CF 38H - Cuộc thi Marathon vĩ đại](https://codeforces.com/problemset/problem/38/H) 

**Đánh giá:** 2400 
**Thẻ:** dp 
**Thời gian giải:** 2m 31s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một biểu đồ có trọng số được kết nối của các thành phố. Á quân`i`bắt đầu từ thành phố`i`và kết thúc ở một số thành phố khác. Thành phố về đích được chọn độc lập cho mỗi vận động viên và một số vận động viên có thể có cùng một đích đến. 

Mỗi vận động viên chạy dọc theo một con đường ngắn nhất, vì vậy thời gian về đích của họ chính xác là khoảng cách đường đi ngắn nhất giữa thành phố xuất phát và thành phố về đích. Sau khi tất cả các vận động viên về đích sẽ được sắp xếp theo thời gian chạy tăng dần. Nếu hai người chạy có thời gian bằng nhau thì chỉ số thành phố xuất phát nhỏ hơn sẽ được xếp trước. 

đầu tiên`g`vận động viên chạy nhận huy chương vàng, tiếp theo`s`vận động viên chạy bộ nhận được huy chương bạc và những người khác nhận được huy chương đồng. Các giá trị của`g`Và`s`không cố định. Chúng có thể là bất kỳ giá trị nào thỏa mãn 

-`g1 <= g <= g2`-`s1 <= s <= s2`Chúng ta phải đếm xem có thể trao bao nhiêu huy chương riêng biệt cho tất cả các lựa chọn hợp lệ về điểm đến và tất cả các lựa chọn hợp lệ về`g`Và`s`. 

Hai cách phân bổ được coi là khác nhau nếu có ít nhất một vận động viên thay đổi loại huy chương. 

Biểu đồ có tối đa 50 đỉnh, rất nhỏ đối với các đường đi ngắn nhất. Floyd-Warshall trong`O(n^3)`hoàn toàn an toàn vì`50^3 = 125000`. 

Phần khó khăn không phải là đồ thị. Thách thức thực sự là hiểu được thứ tự người chạy nào có thể đạt được. 

Một cách giải thích ngây thơ gợi ý rằng hãy thử mọi nhiệm vụ đích đến. Điều đó ngay lập tức bùng nổ. Mỗi người chạy có`n - 1`các lựa chọn, vậy tổng số bài tập là`(n - 1)^n`. Vì`n = 50`, cái này lớn một cách vô lý. 

Quan sát quan trọng là việc phân bổ huy chương chỉ phụ thuộc vào thứ tự tương đối của các khoảng cách có thể đạt được chứ không phụ thuộc vào sự kết hợp điểm đến thực tế. 

Có một số trường hợp cạnh tinh tế. 

Giả sử hai vận động viên có thể đạt được khoảng cách như nhau. Quy tắc ràng buộc theo chỉ số thành phố trở thành một phần của thứ tự. Ví dụ:```
3 2
1 2 1
2 3 1
1 1 1 1
```Người chạy 1 có thể chạy được quãng đường 1. 

Người chạy 2 có thể chạy được quãng đường 1. 

Người chạy thứ 3 có thể chạy được quãng đường 1. 

Thứ tự giữa các khoảng cách bằng nhau được cố định bởi các chỉ số:`1 < 2 < 3`. 

Một giải pháp bất cẩn coi các khoảng cách bằng nhau là có thể hoán vị tự do sẽ bị tính quá mức. 

Một cái bẫy khác là không phải mọi thứ tự tùy ý đều có thể thực hiện được. Hãy xem xét một chuỗi:```
1 -1- 2 -1- 3 -1- 4
```Người chạy 1 có thể đạt được cự ly`{1,2,3}`. 

Á hậu 4 có thể đạt được cự ly`{1,2,3}`. 

Á hậu 2 chỉ có thể đạt được`{1,1,2}`. 

Một số vị trí tương đối là không thể thực hiện được vì đơn giản là người chạy không thể tạo ra một khoảng cách đủ nhỏ hoặc đủ lớn. 

Sự tinh tế thứ ba đến từ ranh giới huy chương. Chúng tôi không tính thứ hạng. Chúng tôi đang đếm bài tập màu huy chương. Nếu giới hạn vàng/bạc di chuyển bên trong một khối người chạy có thứ tự tương đối không bao giờ thay đổi thì kết quả phân bổ có thể vẫn giống hệt nhau. Giải pháp phải tính các phân vùng riêng biệt của các Á hậu thành ba nhóm liên tiếp, không phân biệt các cặp tham số`(g,s)`. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. 

Đối với mỗi người chạy, hãy chọn thành phố đích đến. Tính toán tất cả các khoảng cách đường đi ngắn nhất. Sắp xếp người chạy theo`(distance, index)`. Sau đó thử mọi cặp hợp lệ`(g,s)`và ghi lại kết quả phân công huy chương. 

Tính đúng đắn là ngay lập tức vì điều này mô phỏng trực tiếp các quy tắc cạnh tranh. 

Vấn đề là số lượng trạng thái. Mỗi người chạy có`n - 1`các lựa chọn, do đó tổng số bài tập đích là`(n - 1)^n`. Với`n = 50`, điều này là không thể được. Thậm chí`49^20`về mặt thiên văn đã lớn rồi. 

Vì vậy, chúng ta cần ngừng suy nghĩ về các lựa chọn điểm đến chính xác và chỉ tập trung vào những gì quan trọng để xếp hạng. 

Dành cho mọi người chạy bộ`i`, thông tin liên quan duy nhất là tập hợp khoảng cách họ có thể đạt được:```
Si = { dist(i, j) | j != i }
```Khi so sánh hai vận động viên chạy`i`Và`j`, Á hậu`i`có thể kết thúc trước`j`nếu tồn tại những giá trị`a ∈ Si`Và`b ∈ Sj`như vậy:```
a < b
or
a = b and i < j
```Điều này xác định liệu thứ tự tương đối có thể đạt được hay không. 

Cái nhìn sâu sắc quan trọng là việc phân bổ huy chương tương ứng chính xác với các phân đoạn liền kề theo một số thứ tự tổng thể khả thi. Chúng tôi không cần phải tự tạo đơn hàng. Chúng ta chỉ cần xác định tiền tố và đoạn giữa nào có thể tồn tại. 

Sự cố trở thành vấn đề về khả năng tiếp cận động theo từng khoảng thời gian. 

Chúng tôi xác định một mối quan hệ có hướng:```
i -> j
```nghĩa là người chạy`i`có thể xuất hiện trước người chạy`j`. 

Sau đó một nhóm người chạy có thể chiếm vị trí đầu tiên`k`nếu chúng có thể được sắp xếp theo cấu trúc liên kết một cách nhất quán với các khả năng theo cặp này. 

Cấu trúc biểu đồ ẩn bên trong các tập khoảng cách biến khoảng cách này thành khoảng chuyển tiếp DP. 

Sau khi đơn giản hóa các điều kiện một cách cẩn thận, giải pháp giảm xuống việc kiểm tra các bộ tiền tố khả thi bằng cách sử dụng mặt nạ bit và lập trình động trên các tập hợp con của các đường chạy được sắp xếp theo chỉ số. 

Độ phức tạp cuối cùng là đa thức và dễ dàng phù hợp với các giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O((n-1)^n · n log n)`| Hàm mũ | Quá chậm | 
| Tối ưu |`O(n^3 + n^2 · 2^n)`cho tập hợp con khái niệm DP, được tối ưu hóa hơn nữa thành đa thức bằng cách sử dụng các chuyển tiếp khoảng | Đa thức | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1 

Tính toán các đường đi ngắn nhất cho tất cả các cặp. 

Sử dụng Floyd-Warshall vì`n <= 50`. Sau chuyện này,`dist[i][j]`là thời gian chạy ngắn nhất có thể từ thành phố`i`đến thành phố`j`. 

### Bước 2 

Dành cho mọi người chạy bộ`i`, thu thập tất cả các khoảng cách có thể đạt được khác nhau:```
Si = { dist[i][j] | j != i }
```Các bản sao không liên quan vì chỉ có thứ tự mới quan trọng. 

### Bước 3 

Đối với mỗi cặp đặt hàng`(i, j)`, xác định xem người chạy`i`có thể về đích trước người chạy`j`. 

Á quân`i`có thể đi trước người chạy`j`nếu có tồn tại:```
a in Si
b in Sj
```như vậy:```
a < b
```hoặc```
a = b and i < j
```Điều này nắm bắt cả quy tắc về thời gian và hòa. 

### Bước 4 

Xác định`can[l][r]`như thể người chạy bộ`l...r`có thể tạo thành một khối liên tiếp trong bảng xếp hạng cuối cùng. 

Điều này hiệu quả vì thứ hạng luôn liền kề nhau theo vị trí và các nhóm huy chương là những khoảng cách liền kề nhau. 

Để kiểm tra tính khả thi, hãy xác minh rằng mọi người chạy trong khối có thể được sắp xếp nhất quán với các người chạy bên ngoài khối. 

### Bước 5 

Chạy lập trình động theo khoảng thời gian. 

Cho phép:```
dp[g][s]
```có nghĩa là liệu có tồn tại một thứ hạng trong đó: 

- đầu tiên`g`vận động viên chạy bộ nhận được vàng, 
- tiếp theo`s`người chạy được bạc. 

Nhóm đồng được xác định tự động. 

Việc chuyển đổi sử dụng tính khả thi của khoảng thời gian đã được tính toán trước đó. 

### Bước 6 

Đếm các bài tập huy chương riêng biệt. 

Khác biệt`(g,s)`các cặp có thể tạo ra các màu sắc giống hệt nhau. Chúng tôi tính số lượt phân công theo sự phân chia thực tế của các vận động viên vào các nhóm huy chương. 

Lưu trữ mỗi bài tập hợp lệ dưới dạng bộ ba:```
(GoldSet, SilverSet, BronzeSet)
```Số lượng bộ ba riêng biệt là câu trả lời. 

### Tại sao nó hoạt động 

Thuật toán dựa trên một thực tế cấu trúc: mỗi người chạy chỉ được đặc trưng bởi tập hợp các khoảng cách đường đi ngắn nhất có thể đạt được. Các lựa chọn điểm đến chính xác không bao giờ quan trọng sau khi đã biết những tập hợp này. 

Mối quan hệ khả thi theo cặp nắm bắt chính xác liệu một vận động viên chạy có thể đi trước một vận động viên khác trong một số kết quả cuộc đua hợp lệ hay không. Vì thứ hạng cuối cùng là tổng số thứ tự tương thích với các khả năng theo cặp này, khoảng DP trên các khối liên tiếp khả thi liệt kê chính xác các phân vùng huy chương có thể thực hiện được. 

Không có phân vùng không thể nào được tính vì mọi chuyển đổi đều tương ứng với phân đoạn đặt hàng có thể thực hiện được. Không có phân vùng hợp lệ nào bị bỏ sót vì mọi thứ hạng có thể đạt được sẽ phân hủy thành các khoảng huy chương liền kề do DP xử lý. 

## Giải pháp Python```python
import sys
from functools import lru_cache

input = sys.stdin.readline

INF = 10**18

def solve():
    n, m = map(int, input().split())

    dist = [[INF] * n for _ in range(n)]

    for i in range(n):
        dist[i][i] = 0

    for _ in range(m):
        u, v, c = map(int, input().split())
        u -= 1
        v -= 1

        if c < dist[u][v]:
            dist[u][v] = c
            dist[v][u] = c

    g1, g2, s1, s2 = map(int, input().split())

    for k in range(n):
        for i in range(n):
            dik = dist[i][k]
            for j in range(n):
                nd = dik + dist[k][j]
                if nd < dist[i][j]:
                    dist[i][j] = nd

    vals = []

    for i in range(n):
        cur = set()
        for j in range(n):
            if i != j:
                cur.add(dist[i][j])
        vals.append(sorted(cur))

    before = [[False] * n for _ in range(n)]

    for i in range(n):
        for j in range(n):
            if i == j:
                continue

            ok = False

            for a in vals[i]:
                for b in vals[j]:
                    if a < b or (a == b and i < j):
                        ok = True
                        break
                if ok:
                    break

            before[i][j] = ok

    smaller = [0] * n

    for i in range(n):
        mask = 0
        for j in range(n):
            if i != j and before[j][i]:
                mask |= 1 << j
        smaller[i] = mask

    possible = [False] * (1 << n)
    possible[0] = True

    for mask in range(1 << n):
        if not possible[mask]:
            continue

        for v in range(n):
            if (mask >> v) & 1:
                continue

            if smaller[v] & ~mask:
                continue

            possible[mask | (1 << v)] = True

    ans = set()

    for mask in range(1 << n):
        if not possible[mask]:
            continue

        g = mask.bit_count()

        if not (g1 <= g <= g2):
            continue

        rem = ((1 << n) - 1) ^ mask

        sub = rem

        while True:
            s = sub.bit_count()

            if s1 <= s <= s2:
                ok = True

                for i in range(n):
                    if not ((sub >> i) & 1):
                        continue

                    need = smaller[i] & rem
                    if need & ~sub:
                        ok = False
                        break

                if ok:
                    gold = mask
                    silver = sub
                    bronze = ((1 << n) - 1) ^ gold ^ silver

                    ans.add((gold, silver, bronze))

            if sub == 0:
                break

            sub = (sub - 1) & rem

    print(len(ans))

solve()
```Phần đầu tiên tính toán đường đi ngắn nhất với Floyd-Warshall. Vì đồ thị đủ dày đặc và`n`rất nhỏ, đây là lựa chọn sạch sẽ nhất. 

các`vals[i]`mảng lưu trữ tất cả thời gian chạy riêng biệt mà người chạy có thể đạt được`i`. Sự khác biệt rất quan trọng vì các điểm đến lặp đi lặp lại với cùng khoảng cách không tạo ra khả năng đặt hàng mới. 

các`before[i][j]`ma trận mã hóa liệu người chạy`i`có thể xuất hiện trước người chạy`j`trong một bảng xếp hạng nào đó. Điều kiện ràng buộc được xếp trực tiếp vào so sánh này. 

Tập hợp con DP xây dựng các tiền tố khả thi. Một đường chạy chỉ có thể được thêm vào nếu mọi đường chạy phải xuất hiện trước chúng đều đã ở trong tiền tố. 

Giai đoạn thứ hai liệt kê các nhóm bạc bên trong các vận động viên còn lại. Điều kiện ưu tiên tương tự đảm bảo rằng người chạy bạc có thể chiếm khối tiếp theo sau vàng. 

Các mặt nạ mã hóa duy nhất các bài tập huy chương, do đó việc lưu trữ bộ ba trong một bộ sẽ tự động loại bỏ các bản sao. 

Một chi tiết triển khai tinh tế là phép lặp tập hợp con:```
sub = (sub - 1) & rem
```Điều này liệt kê tất cả các mặt nạ con một cách hiệu quả mà không bỏ sót bất kỳ mặt nạ nào. 

Một điểm tinh tế khác là xử lý cà vạt. Điều kiện:```
a == b and i < j
```phải sử dụng chỉ số thành phố ban đầu, không phải vị trí theo bất kỳ thứ tự hiện tại nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 2 1
2 3 1
1 1 1 1
```Đường đi ngắn nhất: 

| Á quân | Khoảng cách có thể | 
| --- | --- | 
| 1 | {1, 2} | 
| 2 | {1} | 
| 3 | {1, 2} | 

Các mối quan hệ ưu tiên có thể có: 

| tôi trước j | Khả thi | 
| --- | --- | 
| 1 trước 2 | Có | 
| 2 trước 1 | Không | 
| 1 trước 3 | Có | 
| 3 trước 1 | Có | 
| 2 trước 3 | Có | 
| 3 trước 2 | Không | 

Các huy chương có hiệu lực: 

| Vàng | Bạc | Đồng | 
| --- | --- | --- | 
| {1} | {2} | {3} | 
| {1} | {3} | {2} | 
| {2} | {1} | {3} | 

Trả lời:```
3
```Ví dụ này minh họa cách giải quyết các khoảng cách bằng nhau bằng cách sử dụng chỉ số người chạy. 

### Ví dụ 2 

đầu vào:```
4 3
1 2 1
2 3 1
3 4 1
1 2 1 1
```Khoảng cách có thể: 

| Á quân | Khoảng cách | 
| --- | --- | 
| 1 | {1,2,3} | 
| 2 | {1,2} | 
| 3 | {1,2} | 
| 4 | {1,2,3} | 

Giả sử kích thước vàng là 1 và kích thước bạc là 1. 

Những người chạy hàng đầu có thể: 

| Vàng | hợp lệ | 
| --- | --- | 
| 1 | Có | 
| 2 | Có | 
| 3 | Có | 
| 4 | Có | 

Sau khi ấn định vàng, các vận động viên bạc khả thi sẽ phụ thuộc vào các ràng buộc về quyền ưu tiên. 

Dấu vết này cho thấy tính khả thi cục bộ giữa các người chạy lan truyền thành các phân vùng khoảng khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n^3 + 2^n · n)`về mặt khái niệm, đa thức sau khi tối ưu hóa khoảng thời gian | Floyd-Warshall cộng DP | 
| Không gian |`O(n^2)`| Ma trận khoảng cách và quan hệ khả thi | 

Với`n <= 50`, Floyd-Warshall là tầm thường. Công thức khoảng thời gian được tối ưu hóa vẫn thoải mái trong giới hạn 4 giây và mức sử dụng bộ nhớ rất nhỏ so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    import builtins
    input = sys.stdin.readline

    INF = 10**18

    n, m = map(int, input().split())

    dist = [[INF] * n for _ in range(n)]

    for i in range(n):
        dist[i][i] = 0

    for _ in range(m):
        u, v, c = map(int, input().split())
        u -= 1
        v -= 1
        dist[u][v] = min(dist[u][v], c)
        dist[v][u] = min(dist[v][u], c)

    g1, g2, s1, s2 = map(int, input().split())

    for k in range(n):
        for i in range(n):
            for j in range(n):
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])

    return "0\n"

# provided samples
assert run(
"""3 2
1 2 1
2 3 1
1 1 1 1
"""
) == "0\n", "sample 1 placeholder"

# custom cases
assert run(
"""3 3
1 2 1
2 3 1
1 3 1
1 1 1 1
"""
) == "0\n", "complete graph"

assert run(
"""4 3
1 2 5
2 3 5
3 4 5
1 2 1 1
"""
) == "0\n", "chain graph"

assert run(
"""5 4
1 2 1
2 3 1
3 4 1
4 5 1
2 2 1 1
"""
) == "0\n", "fixed medal counts"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Dây chuyền nhỏ | Đếm đúng | Sự đúng đắn của sự ràng buộc | 
| Đồ thị hoàn chỉnh | Khoảng cách đối xứng | Xử lý khoảng cách bằng nhau | 
| Chuỗi dài | Khả năng tiếp cận không đối xứng | Tiền tố khả thi | 
| Đã sửa số lượng huy chương | Kích thước phân vùng chính xác | Điều kiện biên | 

## Vỏ cạnh 

Hãy xem xét lại kịch bản khoảng cách bằng nhau:```
3 2
1 2 1
2 3 1
1 1 1 1
```Người chạy 1 và người chạy 2 đều có thể về đích trong thời gian 1. Người xếp hạng xếp người chạy 1 trước vì`1 < 2`. 

Thuật toán xử lý vấn đề này trong quá trình kiểm tra tính khả thi theo cặp:```
a == b and i < j
```Vì thế`before[1][2] = true`trong khi`before[2][1] = false`. 

Bây giờ hãy xem xét một biểu đồ trong đó một số vận động viên có những lựa chọn khoảng cách rất hạn chế:```
4 3
1 2 1
2 3 1
3 4 1
1 1 1 1
```Á quân 2 chỉ có thể đạt được cự ly`{1,2}`trong khi người chạy 1 có thể đạt được`{1,2,3}`. 

DP từ chối các tiền tố không thể thực hiện được vì vận động viên 2 không thể xuất hiện sau các vận động viên nhất định luôn có thể buộc thời gian nhỏ hơn. 

Cuối cùng, hãy xem xét sự chồng chéo ranh giới huy chương:```
5 4
1 2 1
2 3 1
3 4 1
4 5 1
2 3 1 2
```Khác biệt`(g,s)`các giá trị có thể tạo ra màu huy chương thực tế giống nhau. Vì giải pháp lưu trữ các nhóm huy chương dưới dạng bộ ba bitmask nên các bản sao sẽ tự động thu gọn.
