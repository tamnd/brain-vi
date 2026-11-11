---
title: "CF 1575B - Xây dựng khu vui chơi giải trí"
description: "Chúng tôi muốn đặt một công viên giải trí hình tròn sao cho nó chạm vào gốc tọa độ. Nếu công viên có bán kính $r$ thì tâm của nó phải nằm chính xác $r$ đơn vị tính từ điểm gốc, vì điểm gốc nằm trên ranh giới của công viên. Mỗi môi trường sống của chim là một điểm trên mặt phẳng."
date: "2026-06-10T10:53:56+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "geometry"]
categories: ["algorithms"]
codeforces_contest: 1575
codeforces_index: "B"
codeforces_contest_name: "COMPFEST 13 - Finals Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 2300
weight: 1575
solve_time_s: 318
verified: false
draft: false
---

[CF 1575B - Xây dựng công viên giải trí](https://codeforces.com/problemset/problem/1575/B) 

**Đánh giá:** 2300 
**Tags:** tìm kiếm nhị phân, hình học 
**Thời gian giải:** 5 phút 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi muốn đặt một công viên giải trí hình tròn sao cho nó chạm vào gốc tọa độ. Nếu công viên có bán kính$r$, thì tâm của nó phải nằm chính xác$r$đơn vị tính từ điểm gốc, vì điểm gốc nằm trên ranh giới của vườn. 

Mỗi môi trường sống của chim là một điểm trên mặt phẳng. Một môi trường sống được coi là được che phủ nếu nó nằm bên trong hoặc trên ranh giới của vòng tròn đã chọn. Trong số tất cả các đường tròn tiếp xúc với gốc tọa độ, chúng ta cần bán kính nhỏ nhất cho phép ít nhất$k$môi trường sống cần được che phủ. 

Đầu vào chứa tối đa$10^5$môi trường sống. Bất kỳ thuật toán nào so sánh tất cả các cặp điểm hoặc tìm kiếm rõ ràng trên tất cả các tâm đường tròn có thể sẽ bị loại trừ ngay lập tức. Thậm chí một$O(n^2)$thủ tục sẽ yêu cầu khoảng$10^{10}$hoạt động trong trường hợp xấu nhất. Độ phức tạp của mục tiêu phải gần với$O(n \log n)$theo từng giai đoạn chính. 

Ràng buộc hình học là phần thú vị. Trung tâm không phải là tùy tiện. Đối với bán kính cố định$r$, tâm được giới hạn trong đường tròn có tâm tại gốc tọa độ với bán kính$r$. Điều này chuyển đổi vấn đề thành vấn đề góc một chiều. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét một môi trường sống tại nguồn gốc:```
1 1
0 0
```Câu trả lời đúng là$0$. Khi$r=0$, trung tâm duy nhất có thể có cũng là nguồn gốc và môi trường sống được bao phủ. Một thủ tục quyết định phân chia một cách mù quáng bởi$r$sẽ sụp đổ hoặc tạo ra NaN. 

Hãy xem xét một môi trường sống ở khoảng cách chính xác$2r$từ nguồn gốc. Ví dụ:```
1 1
2 0
```Bán kính tối thiểu là$1$. Khoảng góc tương ứng thu gọn thành một góc duy nhất. Sử dụng bất đẳng thức nghiêm ngặt thay vì bất đẳng thức bao gồm sẽ bác bỏ trường hợp này một cách không chính xác. 

Hãy xem xét các khoảng vượt qua$0$-ranh giới góc. Giả sử một môi trường sống tạo ra một khoảng thời gian từ$350^\circ$ĐẾN$10^\circ$. Việc xử lý các góc như các đoạn đường thông thường sẽ làm mất phần được bao bọc và các phần chồng chéo bị đếm thiếu. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là cố định bán kính$r$, liệt kê các trung tâm ứng cử viên, đếm xem có bao nhiêu môi trường sống được bao phủ và sau đó tìm kiếm nhị phân trên$r$. Khó khăn là tâm có thể là một điểm bất kỳ trên một đường tròn liên tục nên có vô số ứng viên. Ngay cả khi chúng ta rời rạc mạnh mẽ thì kết quả cũng không chính xác. 

Quan sát quan trọng là đối với bán kính cố định$r$, tâm bị ràng buộc vào đường tròn$$|C| = r.$$Lấy một môi trường sống$P$. Chúng tôi hỏi vị trí nào của$C$che nó lại. Điều kiện là$$|P - C| \le r.$$Từ$C$đã nằm trên đường tròn$|C|=r$, các tâm hợp lệ tạo thành một cung trên đường tròn đó. Mỗi môi trường sống đóng góp một khoảng góc. Chúng ta chỉ cần biết liệu một góc nào đó có nằm trong ít nhất$k$khoảng thời gian. 

Điều đó biến bài toán hình học thành bài toán chồng lấp cực đại cổ điển trên một đường tròn. 

Bán kính cố định là khả thi hoặc không khả thi. Nếu bán kính$r$hoạt động, mọi bán kính lớn hơn cũng hoạt động. Tính đơn điệu này cho phép tìm kiếm nhị phân trên câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm trung tâm Brute Force | Trạng thái lũy thừa / liên tục | Lớn | Không thể | 
| Tìm kiếm nhị phân + quét khoảng thời gian |$O(\log A \cdot n \log n)$|$O(n)$| Đã chấp nhận | 

Đây$A$là phạm vi tìm kiếm của bán kính. 

## Hướng dẫn thuật toán 

### Hình học cho một môi trường sống 

Hãy để môi trường sống được$P=(x,y)$, và để$$d = |P|.$$Đối với bán kính cố định$r>0$, tâm là$$C = r(\cos\phi,\sin\phi).$$Môi trường sống được bao phủ khi$$|P-C| \le r.$$Bình phương hai vế:$$d^2 + r^2 - 2rd\cos(\phi-a) \le r^2,$$Ở đâu$a$là góc cực của$P$. 

Sau khi đơn giản hóa:$$\cos(\phi-a) \ge \frac{d}{2r}.$$Nếu như$d>2r$, không có góc nào thỏa mãn bất đẳng thức này. 

Nếu không thì,$$|\phi-a| \le \arccos\!\left(\frac{d}{2r}\right).$$Vì vậy, môi trường sống đóng góp một khoảng góc có tâm ở góc$a$. 

### Kiểm tra tính khả thi của tìm kiếm nhị phân 

1. Nếu$r=0$, đếm xem chính xác có bao nhiêu sinh cảnh ở nơi xuất phát. Bán kính là khả thi nếu số lượng đó ít nhất là$k$. 
2. Với mỗi môi trường sống, hãy tính$d=\sqrt{x^2+y^2}$. 
3. Nếu$d>2r$, môi trường sống này không bao giờ có thể được che phủ và không đóng góp được gì. 
4. Nếu$d=0$, khoảng bao phủ toàn bộ vòng tròn. Tăng bộ đếm cơ sở toàn cầu. 
5. Ngược lại tính toán$$\alpha=\arccos\!\left(\frac{d}{2r}\right).$$Các góc ở tâm hợp lệ là$$[a-\alpha,\ a+\alpha].$$1. Chuyển đổi mọi khoảng thời gian tròn thành các sự kiện quét. Khoảng thời gian vượt qua$0$radian được chia thành hai khoảng thông thường. 
2. Sắp xếp tất cả các sự kiện theo góc độ. Xử lý các sự kiện bắt đầu trước khi các sự kiện kết thúc xảy ra ở cùng một góc độ. 
3. Trong quá trình quét, duy trì số lần chồng lấp hiện tại cộng với số khoảng thời gian đầy đủ của vòng tròn. Nếu sự chồng chéo tối đa đạt đến$k$, bán kính$r$là khả thi. 
4. Tìm kiếm nhị phân bán kính khả thi nhỏ nhất. 

### Tại sao nó hoạt động 

Đối với bán kính cố định$r$, mọi môi trường sống đều tương ứng chính xác với tập hợp các vị trí trung tâm bao phủ nó. Đạo hàm trên chuyển đổi thiết lập thành một khoảng góc trên đường tròn$|C|=r$. Góc ở tâm bao phủ ít nhất$k$môi trường sống khi và chỉ nếu góc đó thuộc ít nhất$k$khoảng thời gian. Quá trình quét tính toán chính xác khoảng thời gian chồng lấp tối đa. 

Vị từ khả thi là đơn điệu. Việc tăng bán kính sẽ mở rộng mọi tập hợp vị trí trung tâm khả thi, không bao giờ thu hẹp nó. Do đó tìm kiếm nhị phân trên$r$tìm bán kính khả thi tối thiểu. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    twopi = 2.0 * math.pi
    eps = 1e-12

    def check(r):
        if r < eps:
            cnt = 0
            for x, y in pts:
                if x == 0 and y == 0:
                    cnt += 1
            return cnt >= k

        base = 0
        events = []

        for x, y in pts:
            d = math.hypot(x, y)

            if d > 2.0 * r + 1e-10:
                continue

            if d < 1e-12:
                base += 1
                continue

            ratio = d / (2.0 * r)
            if ratio > 1.0:
                ratio = 1.0

            alpha = math.acos(ratio)
            a = math.atan2(y, x)

            l = a - alpha
            rr = a + alpha

            l %= twopi
            rr %= twopi

            if l <= rr:
                events.append((l, 1))
                events.append((rr, -1))
            else:
                events.append((0.0, 1))
                events.append((rr, -1))
                events.append((l, 1))
                events.append((twopi, -1))

        events.sort(key=lambda e: (e[0], -e[1]))

        cur = base
        best = cur

        for _, typ in events:
            cur += typ
            if cur > best:
                best = cur

        return best >= k

    lo = 0.0
    hi = 200000.0

    for _ in range(50):
        mid = (lo + hi) / 2.0
        if check(mid):
            hi = mid
        else:
            lo = mid

    print("{:.10f}".format(hi))

solve()
```Tìm kiếm nhị phân duy trì một khoảng chứa bán kính tối ưu. Năm mươi lần lặp là quá đủ để đạt được lỗi dưới đây$10^{-4}$. 

chức năng`check(r)`thực hiện phép giảm hình học. Mỗi môi trường sống trở thành một khoảng góc. Môi trường sống xa hơn$2r$từ nguồn gốc không thể đóng góp vì vòng tròn$|C|=r$không bao giờ đi vào bán kính của họ-$r$đĩa phủ sóng. 

Chi tiết triển khai tinh vi nhất là xử lý các khoảng thời gian bao quanh$0$radian. Những khoảng thời gian như vậy được chia thành hai khoảng thời gian thông thường trước khi quét. Một điểm tinh tế khác là thứ tự sự kiện. Các sự kiện bắt đầu được xử lý trước các sự kiện kết thúc ở cùng một góc, giúp xử lý chính xác các khoảng thời gian có điểm cuối trùng nhau. 

## Ví dụ đã hoạt động 

### Mẫu 1```
8 4
-3 1
-4 4
1 5
2 2
2 -2
-2 -4
-1 -1
-6 0
```Bán kính tối ưu là khoảng$\sqrt{10}$. 

Đối với bán kính đó, các khoảng được tạo có cấu trúc sau. 

| Môi trường sống | Khoảng cách từ điểm xuất phát | Đóng góp khoảng thời gian? | 
| --- | --- | --- | 
| (-3,1) | 3.1623 | Có | 
| (-4,4) | 5.6569 | Có | 
| (1,5) | 5.0990 | Có | 
| (2,2) | 2.8284 | Có | 
| (2,-2) | 2.8284 | Có | 
| (-2,-4) | 4.4721 | Có | 
| (-1,-1) | 1.4142 | Có | 
| (-6,0) | 6,0000 | Có | 

Quá trình quét phát hiện ra một điểm trên vòng tròn trung tâm nơi có bốn khoảng chồng lên nhau. Bất kỳ bán kính nhỏ hơn nào sẽ giảm độ chồng lấp tối đa xuống dưới bốn. 

Ví dụ này thể hiện ý tưởng cốt lõi: vấn đề không phải là chọn một điểm trên mặt phẳng mà là chọn một góc trên đường tròn có thể có các tâm. 

### Ví dụ tùy chỉnh```
3 2
0 0
2 0
-2 0
```Câu trả lời là$1$. 

Vì$r=1$: 

| Môi trường sống | d | Khoảng thời gian | 
| --- | --- | --- | 
| (0,0) | 0 | Toàn bộ vòng tròn | 
| (2,0) | 2 | Góc đơn$0$| 
| (-2,0) | 2 | Góc đơn$\pi$| 

Môi trường sống nguồn gốc đóng góp một sự chồng chéo vĩnh viễn ở khắp mọi nơi. Ở góc$0$, môi trường sống thứ hai cũng bị che phủ, tạo ra sự chồng chéo$2$. Bán kính là khả thi. 

Ví dụ này thực hiện cả các khoảng vòng tròn đầy đủ và các khoảng suy biến có độ rộng bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log A \cdot n \log n)$| Tìm kiếm nhị phân, mỗi lần kiểm tra sắp xếp$O(n)$sự kiện | 
| Không gian |$O(n)$| Danh sách sự kiện | 

Phạm vi tìm kiếm được giới hạn bởi$2 \cdot 10^5$. Khoảng 50 lần lặp tìm kiếm nhị phân là đủ để có độ chính xác cao. Độ phức tạp thu được vừa khít với giới hạn của bài toán. 

## Trường hợp thử nghiệm```python
# helper skeleton

import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    # paste solve() here and return captured output
    return ""

# sample from statement
# answer checked with tolerance in real judge

# minimum size
# 1 habitat at origin, answer = 0
inp = """\
1 1
0 0
"""

# single non-origin point
inp = """\
1 1
2 0
"""

# expected answer = 1

# all habitats at origin
inp = """\
5 5
0 0
0 0
0 0
0 0
0 0
"""

# expected answer = 0

# boundary case d = 2r
inp = """\
1 1
10 0
"""

# expected answer = 5

# wrap-around interval case
inp = """\
2 1
100 1
100 -1
"""

# should succeed with almost identical radii for both points
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một điểm xuất phát | 0 | Xử lý bán kính bằng 0 | 
| Một điểm tại (2,0) | 1 | Khoảng suy biến | 
| Tất cả các điểm tại gốc | 0 | Khoảng thời gian đầy đủ của vòng tròn | 
| Chỉ vào khoảng cách 10 | 5 | ranh giới$d=2r$| 
| Điểm gần góc bọc | Khả thi | Chia khoảng tròn | 

## Vỏ cạnh 

### Bán kính bằng 0 

đầu vào:```
3 2
0 0
0 0
5 0
```Trung tâm duy nhất có thể là nguồn gốc. Hai môi trường sống đã được bao phủ nên câu trả lời là chính xác$0$. 

Thuật toán xử lý việc này trong nhánh đặc biệt bên trong`check(r)`. Không có phép tính lượng giác nào được thực hiện. 

### Môi trường sống chính xác ở khoảng cách xa$2r$đầu vào:```
1 1
10 0
```Đối với bán kính$5$, chúng tôi nhận được$$\frac{d}{2r} = 1, \qquad \alpha = \arccos(1) = 0.$$Khoảng thời gian sụp đổ thành một góc duy nhất. Quá trình quét vẫn tính chính xác vì sự kiện bắt đầu được xử lý trước sự kiện kết thúc. 

### Khoảng vượt qua ranh giới góc 

đầu vào:```
2 1
100 1
100 -1
```Khoảng góc hợp lệ cho một trong các điểm có thể cắt từ các góc gần$2\pi$quay lại góc gần$0$. Thuật toán chia các khoảng như vậy thành$$[l, 2\pi]$$Và$$[0, r].$$Quá trình quét sau đó sẽ nhìn thấy cấu trúc chồng chéo chính xác. Không có phạm vi bảo hiểm bị mất ở ranh giới.
