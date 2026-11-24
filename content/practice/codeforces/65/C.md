---
title: "CF 65C - Harry Potter và trái Snitch vàng"
description: "Snitch di chuyển dọc theo một đường polyline cố định trong không gian 3D. Nó bắt đầu ở đỉnh đầu tiên và di chuyển từng đoạn với tốc độ không đổi so với Harry bắt đầu ở một điểm khác và có thể di chuyển theo bất kỳ hướng nào với tốc độ không đổi vp, trong đó vp = vs."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "geometry"]
categories: ["algorithms"]
codeforces_contest: 65
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 60"
rating: 2100
weight: 65
solve_time_s: 131
verified: true
draft: false
---

[CF 65C - Harry Potter và trái Snitch vàng](https://codeforces.com/problemset/problem/65/C) 

**Đánh giá:** 2100 
**Tags:** tìm kiếm nhị phân, hình học 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Snitch di chuyển dọc theo một đường polyline cố định trong không gian 3D. Nó bắt đầu ở đỉnh đầu tiên và di chuyển từng đoạn với tốc độ không đổi`vs`. Harry bắt đầu ở một điểm khác và có thể di chuyển theo bất kỳ hướng nào với tốc độ không đổi`vp`, Ở đâu`vp >= vs`. 

Chúng ta cần thời gian sớm nhất để Harry và quả snitch có thể chiếm giữ chính xác cùng một điểm. Nếu điều đó không bao giờ xảy ra trước khi snitch kết thúc việc đi qua đường đa tuyến, chúng ta sẽ in`"NO"`. 

Hình học là liên tục. Quả snitch không nhảy giữa các đỉnh mà di chuyển nhịp nhàng dọc theo từng đoạn. Harry cũng được tự do di chuyển liên tục trong không gian. Thử thách là tìm ra điểm có thể tiếp cận đầu tiên trên quỹ đạo snitch. 

Đường đi chứa nhiều nhất`10000`các phân đoạn, ngay lập tức loại trừ bất kỳ mô phỏng cực kỳ chi tiết nào. Cách tiếp cận theo bước thời gian ngây thơ với mức tăng nhỏ sẽ bỏ lỡ câu trả lời do độ chính xác của dấu phẩy động hoặc yêu cầu hàng tỷ lần lặp để đạt được đủ độ chính xác. 

Mỗi tọa độ tối đa là`10^4`, do đó khoảng cách có thể quản lý được bằng số. Số học dấu phẩy động có độ chính xác gấp đôi là đủ. 

Phần khó khăn là quỹ đạo của quả snitch là tuyến tính từng phần. Điểm bắt sớm nhất có thể xảy ra ngay bên trong một đoạn chứ không phải ở một đỉnh. Giải pháp chỉ kiểm tra các đỉnh là sai. 

Hãy xem xét ví dụ này:```
1
0 0 0
10 0 0
2 1
5 0 0
```Snitch di chuyển từ`(0,0,0)`ĐẾN`(10,0,0)`với tốc độ`1`. Harry bắt đầu lúc`(5,0,0)`với tốc độ`2`. 

Harry bắt được trái snitch đúng lúc`5/3`, tại điểm`(5/3,0,0)`. Chỉ kiểm tra điểm cuối sẽ hoàn toàn bỏ lỡ điều này. 

Một sai lầm dễ mắc phải khác là cho rằng Harry luôn có thể bắt được quả snitch vì`vp >= vs`. 

Ví dụ:```
1
0 0 0
10 0 0
1 1
100 0 0
```Snitch kết thúc đúng lúc`10`. Harry cần`90`giây chỉ để đạt đến điểm cuối. Không thể bắt được khi quả snitch đang di chuyển, vì vậy câu trả lời đúng là`"NO"`. 

Ngoài ra còn có một cạm bẫy chính xác gần ranh giới phân khúc. Giả sử thời điểm bắt quả bóng xảy ra chính xác khi quả snitch đạt tới một đỉnh. Tìm kiếm nhị phân độc lập bên trong các phân đoạn mà không xử lý khoảng thời gian cẩn thận có thể vô tình bỏ qua điểm cuối chính xác. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Tham số hóa vị trí quả snitch theo thời gian, sau đó tìm kiếm nhị phân trên toàn cầu đúng thời gian để tìm ra thời điểm đầu tiên khi Harry có thể đạt được vị trí quả snitch. 

Trong một thời gian nhất định`t`, chúng ta có thể tính toán vị trí snitch dọc theo đường đa tuyến. Harry có thể đạt đến điểm đó nếu$$\text{distance}(P, S(t)) \le vp \cdot t$$Ở đâu`P`là vị trí bắt đầu của Harry và`S(t)`là vị trí snitch tại thời điểm đó`t`. 

Tình trạng này là đơn điệu. Một khi Harry có khả năng tiếp cận trái snitch, anh ấy vẫn có khả năng cho tất cả những lần sau đó vì anh ấy chỉ có thể chờ đợi. 

Vì vậy, tìm kiếm nhị phân toàn cầu theo thời gian hoạt động. 

Khó khăn là đánh giá`S(t)`. Snitch di chuyển ngang qua`10000`phân đoạn. Nếu chúng tôi quét tất cả các phân đoạn cho mỗi lần lặp tìm kiếm nhị phân, độ phức tạp sẽ trở nên gần như:$$O(n \cdot \log(\text{precision}))$$điều này đã được chấp nhận ở đây, nhưng chúng ta có thể đơn giản hóa lý do hơn nữa bằng cách khai thác cấu trúc của từng phân đoạn. 

Bên trong một đoạn duy nhất, vị trí snitch thay đổi tuyến tính theo thời gian. Điều kiện khả thi cũng đơn điệu trong khoảng đó. Điều đó có nghĩa là chúng tôi có thể xử lý từng phân đoạn một và chỉ tìm kiếm nhị phân bên trong phân đoạn đầu tiên nơi có thể bắt được. 

Quan sát chính là thế này: 

Nếu Harry không thể bắt được trái snitch khi nó kết thúc một đoạn thì thời điểm bắt được sớm nhất phải muộn hơn. 

Đối với phân khúc`i`, cho phép:

-`t0`là thời điểm snitch bước vào phân đoạn 
-`t1`là thời điểm nó rời khỏi phân khúc 

Nếu Harry không thể đến được điểm cuối của đoạn theo thời gian`t1`, thì không có gì sớm hơn trong các phân đoạn trong tương lai là quan trọng. 

Khi chúng tôi tìm thấy đoạn đầu tiên có thể truy cập kịp thời điểm cuối, thời điểm bắt sớm nhất phải nằm ở đâu đó bên trong đoạn đó. Sau đó tìm kiếm nhị phân đúng thời gian bên trong`[t0, t1]`đưa ra câu trả lời chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm nhị phân toàn cầu Brute Force với quét phân đoạn | O(n độ chính xác của nhật ký) | O(n) | Đã chấp nhận | 
| Tìm kiếm nhị phân theo phân đoạn | O(n + độ chính xác của nhật ký) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các đỉnh đa tuyến và tính toán trước độ dài của mỗi đoạn. 

Quả snitch di chuyển với tốc độ không đổi nên thời gian di chuyển của một đoạn chỉ đơn giản là:$$\frac{\text{segment length}}{vs}$$2. Duy trì thời gian tích lũy hiện tại`cur_t`. 

Điều này thể hiện thời điểm snitch đến điểm bắt đầu phân đoạn hiện tại. 
3. Xử lý các phân đoạn theo thứ tự. 

Giả sử đoạn hiện tại đi từ`A`ĐẾN`B`. 
4. Tính thời gian truyền đoạn. 

Nếu độ dài đoạn là`d`, sau đó:$$\Delta t = \frac{d}{vs}$$Snitch đạt tới`B`vào thời điểm:$$t_1 = cur_t + \Delta t$$5. Kiểm tra xem Harry có thể đến được điểm cuối không`B`theo thời gian`t1`. 

Harry cần:$$\frac{\text{distance}(P, B)}{vp}$$giây để đến đó. 

Nếu điều này vượt quá`t1`, thì Harry không thể bắt được snitch ở bất cứ đâu cho đến điểm cuối này, vì vậy hãy tiếp tục đến đoạn tiếp theo. 
6. Sau khi có thể truy cập kịp thời điểm cuối, điểm đánh bắt sớm nhất sẽ nằm trong phân đoạn này. 

Khi bắt đầu phân đoạn, việc bắt kịp là không thể. Ở cuối phân khúc, điều đó là có thể. Điều kiện thay đổi đơn điệu nên có đúng một điểm chuyển tiếp. 
7. Tìm kiếm nhị phân đúng thời gian trong khoảng thời gian hiện tại. 

Đặt phạm vi tìm kiếm là`[cur_t, t1]`. 
8. Đối với thời gian ứng tuyển`mid`, tính toán vị trí snitch trên đoạn đó. 

Nếu tham số phân đoạn là:$$r = \frac{mid - cur_t}{\Delta t}$$thì vị trí là nội suy tuyến tính:$$S(mid) = A + r(B - A)$$9. Kiểm tra tính khả thi. 

Harry có thể bắt snitch đúng lúc`mid`nếu như:$$\text{distance}(P, S(mid)) \le vp \cdot mid$$Nếu khả thi, hãy di chuyển tìm kiếm nhị phân sang trái. Nếu không thì di chuyển sang phải. 
10. Sau khi lặp lại đủ số lần, xuất ra thời gian và vị trí cuối cùng. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là tính đơn điệu. 

Định nghĩa:$$f(t) = \text{distance}(P, S(t)) - vp \cdot t$$Harry có thể bắt quả snitch chính xác vào thời điểm nào`f(t) <= 0`. 

Khi thời gian tăng lên, bán kính có thể tiếp cận của Harry tăng tuyến tính. Quả snitch di chuyển liên tục với tốc độ tối đa bằng tốc độ của Harry. Một khi trái snitch đi vào vùng mà Harry có thể tiếp cận, nó sẽ không bao giờ có thể thoát khỏi vùng đó vĩnh viễn. 

Điều đó có nghĩa là tính khả thi chỉ thay đổi một lần, từ không thể thành có thể. Tìm kiếm nhị phân là hợp lệ. 

Việc xử lý các phân đoạn theo thứ tự đảm bảo chúng tôi xác định được phân đoạn sớm nhất chứa thời gian hợp lệ và tìm kiếm nhị phân bên trong phân đoạn đó sẽ khôi phục thời điểm khả thi sớm nhất. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def dist(a, b):
    dx = a[0] - b[0]
    dy = a[1] - b[1]
    dz = a[2] - b[2]
    return math.sqrt(dx * dx + dy * dy + dz * dz)

def solve():
    n = int(input())

    pts = [tuple(map(float, input().split())) for _ in range(n + 1)]

    vp, vs = map(float, input().split())
    P = tuple(map(float, input().split()))

    cur_t = 0.0

    for i in range(n):
        A = pts[i]
        B = pts[i + 1]

        seg_len = dist(A, B)
        seg_time = seg_len / vs

        end_t = cur_t + seg_time

        if dist(P, B) > vp * end_t + 1e-12:
            cur_t = end_t
            continue

        lo = cur_t
        hi = end_t

        for _ in range(100):
            mid = (lo + hi) / 2.0

            ratio = (mid - cur_t) / seg_time

            x = A[0] + (B[0] - A[0]) * ratio
            y = A[1] + (B[1] - A[1]) * ratio
            z = A[2] + (B[2] - A[2]) * ratio

            snitch = (x, y, z)

            if dist(P, snitch) <= vp * mid:
                hi = mid
            else:
                lo = mid

        t = hi
        ratio = (t - cur_t) / seg_time

        x = A[0] + (B[0] - A[0]) * ratio
        y = A[1] + (B[1] - A[1]) * ratio
        z = A[2] + (B[2] - A[2]) * ratio

        print("YES")
        print(f"{t:.10f}")
        print(f"{x:.10f} {y:.10f} {z:.10f}")
        return

    print("NO")

solve()
```Hàm trợ giúp đầu tiên tính toán khoảng cách Euclide trong không gian 3D. Mọi thứ trong bài toán đều có tính chất hình học nên thao tác này xuất hiện lặp đi lặp lại. 

Vòng lặp chính xử lý một phân đoạn tại một thời điểm.`cur_t`lưu trữ khi snitch đến đoạn bắt đầu. Từ độ dài đoạn và tốc độ snitch, chúng tôi tính toán thời gian truyền tải. 

Kiểm tra khả năng tiếp cận điểm cuối là bước cắt tỉa quan trọng. Nếu Harry vẫn không thể đến được điểm cuối của đoạn đó vào thời điểm quả snitch đến đó thì cũng không có điểm nào sớm hơn trong đoạn đó có thể hoạt động được. Điều đó cho phép chúng tôi bỏ qua toàn bộ phân khúc. 

Khi đã tìm thấy điểm cuối khả thi, câu trả lời phải nằm trong phân khúc đó. Tìm kiếm nhị phân sử dụng thời gian làm biến tìm kiếm. Việc chuyển đổi thời gian thành một điểm trên đoạn cần nội suy tuyến tính với tham số`ratio`. 

Tìm kiếm nhị phân chạy cho`100`lần lặp lại. Điều đó là quá đủ cho`1e-6`độ chính xác vì mỗi lần lặp lại giảm một nửa khoảng thời gian. 

Epsilon nhỏ trong:```
dist(P, B) > vp * end_t + 1e-12
```tránh sự mất ổn định của dấu phẩy động khi các giá trị cực kỳ gần nhau. 

Một chi tiết tinh tế đang được sử dụng`hi`là câu trả lời cuối cùng sau khi tìm kiếm nhị phân.`hi`luôn lưu trữ một thời gian khả thi, trong khi`lo`lưu trữ một cái không thể thực hiện được. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
0 0 0
0 10 0
10 10 0
10 0 0
0 0 0
1 1
5 5 25
```Snitch di chuyển dọc theo một hình vuông trong mặt phẳng`z = 0`, trong khi Harry bắt đầu ở phía trên trung tâm. 

| Phân đoạn | Thời gian bắt đầu | Thời Gian Kết Thúc | Điểm cuối có thể truy cập được? | 
| --- | --- | --- | --- | 
| (0,0,0) → (0,10,0) | 0 | 10 | Không | 
| (0,10,0) → (10,10,0) | 10 | 20 | Không | 
| (10,10,0) → (10,0,0) | 20 | 30 | Có | 

Phân khúc khả thi sớm nhất là phân khúc thứ ba. 

Tìm kiếm nhị phân bên trong`[20, 30]`hội tụ đến: 

| Lặp lại | Giữa Giờ | Khả thi? | 
| --- | --- | --- | 
| 1 | 25.0 | Không | 
| 2 | 27,5 | Có | 
| 3 | 26.25 | Có | 
| ... | ... | ... | 
| Cuối cùng | 25,5 | Có | 

Điểm bắt trở thành: 

| x | y | z | 
| --- | --- | --- | 
| 10 | 4,5 | 0 | 

Ví dụ này chứng minh rằng câu trả lời có thể xảy ra hoàn toàn bên trong một đoạn chứ không phải ở một đỉnh. 

### Ví dụ tùy chỉnh 

đầu vào:```
1
0 0 0
10 0 0
1 1
100 0 0
```| Phân đoạn | Thời gian bắt đầu | Thời Gian Kết Thúc | Điểm cuối có thể truy cập được? | 
| --- | --- | --- | --- | 
| (0,0,0) → (10,0,0) | 0 | 10 | Không | 

Harry cần`90`giây chỉ để đạt được`(10,0,0)`, nhưng có lúc snitch ngừng di chuyển`10`. 

Không có phân đoạn nào khả thi nên thuật toán đưa ra:```
NO
```Điều này khẳng định rằng tốc độ bằng nhau không đảm bảo thành công. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + độ chính xác của nhật ký) | Mỗi phân đoạn được xử lý một lần và thực hiện chính xác một tìm kiếm nhị phân | 
| Không gian | O(n) | Lưu trữ tất cả các đỉnh đa tuyến | 

Với`n <= 10000`, điều này dễ dàng nằm gọn trong giới hạn. Tìm kiếm nhị phân thực hiện một số lần lặp không đổi, do đó thời gian chạy bị chi phối bởi quá trình quét tuyến tính qua các phân đoạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from math import isclose

def run(inp: str) -> str:
    import math

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def dist(a, b):
        dx = a[0] - b[0]
        dy = a[1] - b[1]
        dz = a[2] - b[2]
        return math.sqrt(dx * dx + dy * dy + dz * dz)

    out = io.StringIO()
    sys.stdout = out

    n = int(input())

    pts = [tuple(map(float, input().split())) for _ in range(n + 1)]

    vp, vs = map(float, input().split())
    P = tuple(map(float, input().split()))

    cur_t = 0.0

    for i in range(n):
        A = pts[i]
        B = pts[i + 1]

        seg_len = dist(A, B)
        seg_time = seg_len / vs

        end_t = cur_t + seg_time

        if dist(P, B) > vp * end_t + 1e-12:
            cur_t = end_t
            continue

        lo = cur_t
        hi = end_t

        for _ in range(100):
            mid = (lo + hi) / 2.0

            ratio = (mid - cur_t) / seg_time

            x = A[0] + (B[0] - A[0]) * ratio
            y = A[1] + (B[1] - A[1]) * ratio
            z = A[2] + (B[2] - A[2]) * ratio

            if dist(P, (x, y, z)) <= vp * mid:
                hi = mid
            else:
                lo = mid

        t = hi
        ratio = (t - cur_t) / seg_time

        x = A[0] + (B[0] - A[0]) * ratio
        y = A[1] + (B[1] - A[1]) * ratio
        z = A[2] + (B[2] - A[2]) * ratio

        print("YES")
        print(f"{t:.10f}")
        print(f"{x:.10f} {y:.10f} {z:.10f}")
        return out.getvalue()

    print("NO")
    return out.getvalue()

# sample 1
res = run("""4
0 0 0
0 10 0
10 10 0
10 0 0
0 0 0
1 1
5 5 25
""")
assert res.startswith("YES")

# minimum size, immediate catch
res = run("""1
0 0 0
10 0 0
2 1
0 0 0
""")
assert res.startswith("YES")

# impossible case
assert run("""1
0 0 0
10 0 0
1 1
100 0 0
""").strip() == "NO"

# catch exactly at endpoint
res = run("""1
0 0 0
10 0 0
2 1
20 0 0
""")
assert res.startswith("YES")

# 3D diagonal movement
res = run("""1
0 0 0
10 10 10
3 1
5 5 20
""")
assert res.startswith("YES")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bắt ngay khi bắt đầu | CÓ | Xử lý đúng thời gian`0`| 
| Xa Harry | KHÔNG | Bắt có thể là không thể | 
| Bắt ở điểm cuối | CÓ | Xử lý ranh giới phân khúc | 
| Đoạn chéo 3D | CÓ | Hình học 3D chính xác | 
| Mẫu 1 | CÓ | Điểm bắt nội thất | 

## Vỏ cạnh 

Hãy xem xét trường hợp việc đánh bắt xảy ra chính xác tại một đỉnh. 

đầu vào:```
1
0 0 0
10 0 0
2 1
20 0 0
```Snitch đạt tới`(10,0,0)`vào thời điểm đó`10`. Harry cũng cần chính xác`10`giây để đạt đến điểm đó. 

Thuật toán xử lý phân đoạn và kiểm tra tính khả thi của điểm cuối:$$\text{distance} = 10,\quad vp \cdot t = 20$$nên phân khúc này là khả thi. 

Tìm kiếm nhị phân hội tụ đến điểm cuối. Vì tính khả thi được kiểm tra bằng`<=`, trường hợp đẳng thức chính xác được chấp nhận chính xác. 

Bây giờ hãy xem xét một cú đánh bắt bên trong. 

đầu vào:```
1
0 0 0
10 0 0
2 1
5 0 0
```Vào thời điểm`0`, quả snitch ở quá xa bên trái. Vào thời điểm`10`, Harry chắc chắn có thể đạt đến điểm cuối. 

Tìm kiếm nhị phân thu hẹp dần về thời gian khả thi đầu tiên: 

| Thời gian | Vị trí Snitch | Bán kính có thể tiếp cận của Harry | Khả thi | 
| --- | --- | --- | --- | 
| 1 | 1 | 2 | Không | 
| 2 | 2 | 4 | Có | 

Quá trình chuyển đổi chính xác xảy ra tại:$$t = \frac{5}{3}$$Điều này xác nhận rằng thuật toán xử lý chính xác các câu trả lời một cách nghiêm ngặt bên trong các phân đoạn. 

Cuối cùng, hãy xem xét một trường hợp hoàn toàn không thể xảy ra. 

đầu vào:```
1
0 0 0
10 0 0
1 1
100 0 0
```Điểm cuối của phân đoạn đạt được vào thời điểm`10`, nhưng Harry cần`90`giây để đến đó. 

Kiểm tra tính khả thi của điểm cuối không thành công nên thuật toán không bao giờ thực hiện tìm kiếm nhị phân và in`"NO"`. 

Điều này ngăn chặn các kết quả dương tính giả do giả sử tốc độ bằng nhau có nghĩa là cuối cùng sẽ bị chặn.
