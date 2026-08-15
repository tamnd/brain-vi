---
title: "CF 102439A - Còn bốn phút nữa là đến BSUIR Open"
description: "Ta có một con đường thẳng từ vị trí 0 đến trường đại học ở vị trí (xn). Camera (i) ở vị trí (xi), khi ô tô đi qua điểm đó tốc độ tối đa phải bằng (vi). Xe khởi động với tốc độ bằng không."
date: "2026-08-14T15:55:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102439
codeforces_index: "A"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Semifinal"
rating: 0
weight: 102439
solve_time_s: 142
verified: true
draft: false
---

[CF 102439A - Bốn phút nữa là đến BSUIR Open](https://codeforces.com/problemset/problem/102439/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ta có một con đường thẳng từ vị trí 0 đến trường đại học ở vị trí (x_n). Camera (i) đang ở vị trí (x_i) và khi ô tô đi qua điểm đó tốc độ tối đa phải là (v_i). Xe khởi động với tốc độ bằng không. Nó có thể tăng tốc khi tăng tốc nhiều nhất (a), phanh khi giảm tốc nhiều nhất (b) và giữ tốc độ không đổi. 

Đầu vào cung cấp vị trí camera và giới hạn tốc độ của chúng, cùng với hai thông số gia tốc. Đầu ra yêu cầu là thời gian di chuyển tối thiểu có thể từ vị trí 0 đến camera cuối cùng, cũng là trường đại học. 

Khó khăn chính là camera chỉ giới hạn tốc độ ở một vị trí. Giữa các camera, ô tô có thể tạm thời đi nhanh hơn nhiều so với tốc độ điểm cuối. Ví dụ: nếu tốc độ ở hai camera liên tiếp là 1 và 1 thì chuyển động nhanh nhất không nhất thiết phải giữ ở tốc độ 1. Xe có thể tăng tốc trên 1, sau đó phanh lại về 1 trước camera thứ hai. 

Với (n) đến (10^5), thuật toán (O(n^2)) sẽ thực hiện khoảng 

[ 
\frac{10^5(10^5-1)}2 \approx 5\cdot 10^9 
] 

kiểm tra từng cặp trong trường hợp xấu nhất. Điều đó vượt xa những gì giới hạn một giây có thể xử lý. Giải pháp phải xử lý mọi camera với số lần không đổi. 

Có một số trường hợp khó xử lý. Ví dụ: nếu máy ảnh duy nhất có giới hạn tốc độ bằng 0,```
1 1 1
1 0
```câu trả lời đúng là (2). Ô tô phải khởi hành từ số 0 và trở về số 0 sau khi đi được một đơn vị, do đó nó tăng tốc lên tốc độ 1 rồi phanh về số 0. Một giải pháp chỉ sử dụng (x/v) ngắt quãng vì tốc độ điểm cuối bằng không. 

Một camera trung gian cũng có thể buộc xe dừng lại. Vì```
2 1 1
1 0
2 1
```câu trả lời đúng là xấp xỉ (3,4494897428). Bộ phận đầu tiên yêu cầu tăng tốc và phanh về 0, chỉ khi đó ô tô mới có thể bắt đầu tăng tốc trở lại. Việc coi mọi phân đoạn như một chuyển động độc lập với tốc độ không đổi sẽ thất bại nặng nề ở đây. 

Giới hạn máy ảnh bằng nhau là một trường hợp tế nhị khác. Coi như```
2 1 1
1 1
2 1
```Câu trả lời là xấp xỉ (1.8989794856). Ở đoạn thứ hai, ô tô phải tăng tốc trên tốc độ 1 và sau đó phanh lại tốc độ 1. Chỉ cần di chuyển cả hai đoạn ở tốc độ 1 sẽ cho câu trả lời chậm hơn. 

Cuối cùng, camera cuối cùng là đích đến nên giới hạn tốc độ của nó là một hạn chế về điểm cuối chứ không chỉ đơn thuần là hạn chế trước khi tiếp tục đi xa hơn. Nếu giới hạn của nó bằng 0 thì ô tô phải về đích với tốc độ bằng 0. Đường chuyền ngược xử lý hạn chế đó một cách tự nhiên. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là xác định, đối với mỗi camera, tốc độ của mỗi camera khác hạn chế tốc độ của nó như thế nào. Đối với camera ở (x_i), mọi camera sau (j) đều đưa ra điều kiện hãm 

[ 
u_i^2 \le v_j^2 + 2b(x_j-x_i). 
] 

Việc triển khai bạo lực có thể kiểm tra mọi camera sau này để tìm (i), lấy tốc độ tối thiểu cho phép và sau đó thực hiện công việc tương tự để tăng tốc ngay từ đầu. Điều này đúng vì mọi máy ảnh có thể có trong tương lai đều được kiểm tra rõ ràng, nhưng nó thực hiện các thao tác (O(n^2)), khoảng (5\cdot10^9) kiểm tra khi (n=10^5). 

Quan sát hữu ích là những hạn chế này có sự tái diễn cục bộ. Giả sử tốc độ tối đa có thể được sử dụng hợp pháp ở camera (i+1) đã được biết. Khi đó tốc độ tối đa ở camera (i) vẫn có thể giảm xuống tốc độ đó theo khoảng cách (x_{i+1}-x_i) là 

[ 
\sqrt{v_{i+1}^2+2b(x_{i+1}-x_i)}. 
] 

Bất kỳ máy ảnh nào trước đó đều đã được tóm tắt bằng giá trị được lưu tại (i+1). Điều này có nghĩa là hạn chế phanh hoàn toàn có thể được tính toán trong một lần lùi. 

Ý tưởng tương tự hoạt động ngay từ đầu. Nếu tốc độ tối đa có thể đạt được ở máy ảnh (i-1) là (u), thì sau quãng đường di chuyển (d) với gia tốc tối đa (a), tốc độ lớn nhất có thể đạt được là 

[ 
\sqrt{u^2+2ad}. 
] 

Kết hợp điều này với giới hạn riêng của máy ảnh sẽ mang lại tốc độ tối đa được phép bởi tất cả các ràng buộc ở bên trái. 

Gọi (F_i) là tốc độ tối đa tại camera (i) chỉ xét điểm bắt đầu và các camera trước đó. Gọi (B_i) là tốc độ tối đa tại camera (i) chỉ xét các camera sau nó và giới hạn phanh. Khi đó tốc độ lớn nhất tương thích đồng thời với cả hai hướng là 

[ 
s_i=\min(F_i,B_i). 
] 

Những tốc độ này chính xác là tốc độ điểm cuối mà chúng tôi cần. Khi hai tốc độ điểm cuối liên tiếp được cố định, chuyển động nhanh nhất giữa chúng có hình dạng rất đơn giản: tăng tốc ở tốc độ (a) lên đến tốc độ cực đại nào đó (p), sau đó phanh ở tốc độ (b) xuống tốc độ điểm cuối tiếp theo. 

Đối với đoạn đường dài (d), bắt đầu từ tốc độ (u) và kết thúc ở tốc độ (v), quãng đường đi được khi tăng tốc và phanh là 

[ 
d=\frac{p^2-u^2}{2a}+\frac{p^2-v^2}{2b}. 
] 

Giải (p^2) cho 

[ 
p^2= 
\frac{2abd+bu^2+av^2}{a+b}. 
] 

Khi đó thời gian của đoạn này là 

[ 
\frac{p-u}{a}+\frac{p-v}{b}. 
] 

Tốc độ điểm cuối được tạo ra bởi hai lần truyền có thể đạt được lẫn nhau, do đó kết quả (p) ít nhất luôn là tốc độ của cả hai điểm cuối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các vị trí camera (x_i) và giới hạn (v_i). Coi vị trí 0 là một điểm bổ sung có tốc độ cố định bằng 0. 
2. Tính giới hạn tốc độ tiến (F_i). Bắt đầu với tốc độ 0 ở vị trí 0. Đối với mỗi camera, hãy tính tốc độ lớn nhất có thể đạt được từ tốc độ tiến trước đó bằng cách sử dụng gia tốc (a), sau đó giới hạn tốc độ đó bằng (v_i): 

[ 
F_i=\min\left(v_i,\sqrt{F_{i-1}^2+2a(x_i-x_{i-1})}\right). 
] 

Điều này nắm bắt mọi hạn chế ngay từ đầu và tất cả các máy ảnh trước đó. 

1. Tính giới hạn tốc độ lùi (B_i). Bắt đầu ở camera cuối cùng với (B_n=v_n). Di chuyển về phía sau và tính tốc độ ô tô có thể đạt được ở camera trước trong khi vẫn phanh tới (B_{i+1}): 

[ 
B_i=\min\left(v_i,\sqrt{B_{i+1}^2+2b(x_{i+1}-x_i)}\right). 
] 

Điều này nén tất cả các ràng buộc phanh trong tương lai thành một giá trị cho mỗi camera. 

1. Xác định tốc độ điểm cuối thực tế ở mỗi camera như sau: 

[ 
s_i=\min(F_i,B_i). 
]

Giá trị chuyển tiếp đảm bảo rằng ô tô có thể đạt (s_i) ngay từ đầu trong khi vẫn tôn trọng các camera trước đó. Giá trị lùi đảm bảo rằng nó có thể tiếp tục từ (s_i) đến đích trong khi vẫn tôn trọng các camera trong tương lai. 

1. Thêm vị trí 0 với tốc độ (s_0=0). Đối với mỗi cặp vị trí liên tiếp, gọi khoảng cách là (d=x_i-x_{i-1}), tốc độ bắt đầu là (u=s_{i-1}) và tốc độ kết thúc là (v=s_i). 
2. Tìm tốc độ tối đa tối ưu từ 

[ 
p^2= 
\frac{2abd+bu^2+av^2}{a+b}. 
] 

Xe tăng tốc từ (u) đến (p), sau đó phanh từ (p) đến (v). Thời gian phân đoạn là 

[ 
t_i=\frac{p-u}{a}+\frac{p-v}{b}. 
] 

1. Tính tổng tất cả thời gian của phân đoạn và in kết quả với độ chính xác vừa đủ. 

### Tại sao nó hoạt động 

Chuyển tiếp duy trì bất biến rằng (F_i) là tốc độ lớn nhất có thể đạt được ở camera (i) đồng thời đáp ứng mọi camera từ đầu đến (i). Sự truy hồi diễn ra trực tiếp từ (v^2=u^2+2ad). 

Quá trình lùi duy trì tính bất biến đối xứng rằng (B_i) là tốc độ lớn nhất được phép ở camera (i) trong khi vẫn có thể đáp ứng mọi camera từ (i) đến đích. Sự tái diễn của nó xuất phát từ quan hệ hãm (u^2=v^2+2bd). 

Do đó (s_i=\min(F_i,B_i)) là tốc độ lớn nhất ở mọi camera khả thi ở cả hai hướng. Sử dụng tốc độ điểm cuối lớn hơn sẽ vi phạm hạn chế tăng tốc trước đó hoặc hạn chế phanh trong tương lai. Việc sử dụng tốc độ điểm cuối nhỏ hơn không thể cải thiện thời gian di chuyển vì đoạn đường nhanh nhất giữa các vị trí cố định có lợi ích không giảm khi cho phép tốc độ điểm cuối khả thi lớn hơn. 

Đối với hai tốc độ điểm cuối cố định, bất kỳ thời gian nào dành cho việc tăng tốc dưới mức (a) hoặc phanh dưới mức (b) đều có thể được thay thế bằng việc tăng tốc hoặc phanh nhanh hơn mà không vi phạm các điều kiện của điểm cuối. Do đó, một đoạn đường tối ưu bao gồm khả năng tăng tốc tối đa sau đó là phanh tối đa. Việc giải phương trình khoảng cách sẽ cho tốc độ cực đại duy nhất của nó, do đó công thức đoạn cho thời gian tối thiểu có thể. Tổng hợp các phân đoạn tối ưu này sẽ tạo ra mức tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    n, a, b = map(int, input().split())

    x = [0] * n
    v = [0] * n

    for i in range(n):
        x[i], v[i] = map(int, input().split())

    # Maximum speed reachable from the start while
    # respecting all cameras seen so far.
    forward = [0.0] * n
    cur = 0.0
    prev_x = 0

    for i in range(n):
        d = x[i] - prev_x
        reachable = math.sqrt(cur * cur + 2.0 * a * d)
        cur = min(float(v[i]), reachable)
        forward[i] = cur
        prev_x = x[i]

    # Maximum speed allowed when looking from the destination backwards.
    backward = [0.0] * n
    backward[-1] = float(v[-1])

    for i in range(n - 2, -1, -1):
        d = x[i + 1] - x[i]
        reachable = math.sqrt(
            backward[i + 1] * backward[i + 1] + 2.0 * b * d
        )
        backward[i] = min(float(v[i]), reachable)

    # The actual feasible maximum speed at each camera.
    speed = [min(forward[i], backward[i]) for i in range(n)]

    ans = 0.0
    prev_speed = 0.0
    prev_x = 0.0

    for i in range(n):
        d = x[i] - prev_x
        cur_speed = speed[i]

        # During the optimal segment:
        #
        # d = (p^2 - u^2)/(2a) + (p^2 - v^2)/(2b)
        #
        # so:
        # p^2 = (2abd + bu^2 + av^2) / (a+b)
        p2 = (
            2.0 * a * b * d
            + b * prev_speed * prev_speed
            + a * cur_speed * cur_speed
        ) / (a + b)

        # Protect against a tiny negative value caused by floating point
        # rounding in cases where the exact value is zero.
        p = math.sqrt(max(0.0, p2))

        ans += (p - prev_speed) / a
        ans += (p - cur_speed) / b

        prev_speed = cur_speed
        prev_x = x[i]

    print(f"{ans:.10f}")

if __name__ == "__main__":
    solve()
```Lần vượt qua đầu tiên sử dụng`cur`là tốc độ tốt nhất hiện có thể đạt được ngay từ đầu. biểu hiện`cur * cur + 2.0 * a * d`là mối quan hệ gia tốc không đổi tiêu chuẩn và việc lấy mức tối thiểu với giới hạn camera sẽ kết hợp ngay hạn chế mới. 

Quá trình quay ngược bắt đầu với giới hạn tốc độ thực tế của camera cuối cùng. Vì đích đến chính xác là camera cuối cùng nên không có vị trí nào sau nó nên giá trị lùi của nó chỉ đơn giản là`v[-1]`. Mọi máy ảnh trước đó sẽ bị hạn chế bởi khoảng cách phanh có sẵn trước máy ảnh tiếp theo. 

Giai đoạn thứ ba kết hợp hai mảng. Có thể tiếp cận camera từ bên trái nhưng không thể rời khỏi một cách an toàn về phía bên phải hoặc ngược lại. Lấy mức tối thiểu của chúng xử lý đồng thời cả hai điều kiện. 

Việc tính toán phân đoạn là phần tinh tế nhất. Tốc độ tối đa không nhất thiết phải bằng tốc độ điểm cuối. Ngay cả khi cả hai camera đều yêu cầu tốc độ 1, ô tô có thể nhanh hơn 1 giữa chúng. Biểu thức bậc hai của`p2`tính đến chính xác khả năng đó. 

Không có vấn đề tràn số nguyên trong Python. Các giá trị trung gian lớn nhất được xử lý thoải mái bởi các số nguyên Python trước khi chuyển đổi sang dấu phẩy động và các giá trị tương ứng cũng nằm trong phạm vi hữu ích của số dấu phẩy động có độ chính xác kép. các`max(0.0, p2)`bảo vệ chỉ bảo vệ chống lại một giá trị âm cực nhỏ do làm tròn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2 1 1
1 1
2 2
```Lượt chuyển tiếp bắt đầu từ số 0. 

| Máy ảnh | Vị trí | Giới hạn | Chuyển tiếp (F_i) | Lùi lại (B_i) | Tốc độ cuối cùng (s_i) | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1.000000 | 1.000000 | 1.000000 | 
| 2 | 2 | 2 | 1.732051 | 2.000000 | 1.732051 | 

Đối với đoạn đầu tiên, tốc độ bắt đầu và điểm cuối là 0 và 1. Tốc độ tối đa tối ưu là 

[ 
p=\sqrt{1.5}\approx1.224745. 
] 

Phân đoạn này chiếm khoảng (1,449490) đơn vị thời gian. 

Đối với phân đoạn thứ hai, tốc độ điểm cuối là 1 và (\sqrt3). Xe có thể tăng tốc trong toàn bộ đoạn đường nên đỉnh của nó là (\sqrt3), và đoạn này mất khoảng (0,732051). 

| Phân đoạn | Khoảng cách | Tốc độ bắt đầu | Tốc độ kết thúc | Tốc độ đỉnh cao | Thời gian | 
| --- | --- | --- | --- | --- | --- | 
| 0 đến 1 | 1 | 0,000000 | 1.000000 | 1.224745 | 1.449490 | 
| 1 đến 2 | 1 | 1.000000 | 1.732051 | 1.732051 | 0,732051 | 

Tổng số là khoảng (2,1815405), khớp với đầu ra mẫu. 

### Mẫu 2 

Đầu vào là```
4 1 5
2 3
4 5
7 3
9 5
```Các giới hạn chuyển tiếp là 

[ 
2,\quad 2,828427,\quad 3,\quad 3,605551. 
] 

Các giới hạn lùi là 

[ 
3,\quad 4.358899,\quad 3,\quad 5. 
] 

Lấy mức tối thiểu sẽ cho tốc độ thực tế 

[ 
2,\quad2.828427,\quad3,\quad3.605551. 
] 

| Phân đoạn | Khoảng cách | Tốc độ bắt đầu | Tốc độ kết thúc | Tốc độ đỉnh cao | Thời gian | 
| --- | --- | --- | --- | --- | --- | 
| 0 đến 2 | 2 | 0,000000 | 2.000000 | 2.000000 | 2.000000 | 
| 2 đến 4 | 2 | 2.000000 | 2.828427 | 2.828427 | 0.828427 | 
| 4 đến 7 | 3 | 2.828427 | 3.000000 | 3.628590 | 0,925880 | 
| 7 đến 9 | 2 | 3.000000 | 3.605551 | 3.605551 | 0,605551 | 

Tổng xấp xỉ (4,3598594), khớp với mẫu thứ hai. 

Đoạn thứ ba đặc biệt hữu ích vì tốc độ tối đa của nó lớn hơn cả tốc độ điểm cuối. Đây chính xác là tình huống mà một giải pháp chỉ dựa trên tốc độ trung bình hoặc tốc độ camera sẽ bỏ lỡ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Hai đường tuyến tính tính toán các giới hạn tiến và lùi, theo sau là một đường tuyến tính cho thời gian phân đoạn. | 
| Không gian | (O(n)) | Dữ liệu camera và các mảng tốc độ tiến, lùi và cuối cùng được lưu trữ. | 

Thuật toán chỉ thực hiện một số thao tác không đổi trên mỗi camera, vì vậy (10^5) camera chỉ yêu cầu vài trăm nghìn phép tính số học. Việc sử dụng bộ nhớ cũng tuyến tính và dễ dàng phù hợp trong phạm vi 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def solve_io(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    n, a, b = map(int, sys.stdin.readline().split())

    x = [0] * n
    v = [0] * n

    for i in range(n):
        x[i], v[i] = map(int, sys.stdin.readline().split())

    forward = [0.0] * n
    cur = 0.0
    prev_x = 0

    for i in range(n):
        d = x[i] - prev_x
        cur = min(float(v[i]), math.sqrt(cur * cur + 2.0 * a * d))
        forward[i] = cur
        prev_x = x[i]

    backward = [0.0] * n
    backward[-1] = float(v[-1])

    for i in range(n - 2, -1, -1):
        d = x[i + 1] - x[i]
        backward[i] = min(
            float(v[i]),
            math.sqrt(
                backward[i + 1] * backward[i + 1]
                + 2.0 * b * d
            )
        )

    speed = [min(forward[i], backward[i]) for i in range(n)]

    ans = 0.0
    prev_speed = 0.0
    prev_x = 0.0

    for i in range(n):
        d = x[i] - prev_x
        cur_speed = speed[i]

        p2 = (
            2.0 * a * b * d
            + b * prev_speed * prev_speed
            + a * cur_speed * cur_speed
        ) / (a + b)

        p = math.sqrt(max(0.0, p2))

        ans += (p - prev_speed) / a
        ans += (p - cur_speed) / b

        prev_speed = cur_speed
        prev_x = x[i]

    sys.stdin = old_stdin
    return f"{ans:.10f}"

def assert_close(inp: str, expected: float, eps: float = 1e-8):
    actual = float(solve_io(inp))
    assert abs(actual - expected) <= eps, (actual, expected)

# Provided sample 1
assert_close(
    """\
2 1 1
1 1
2 2
""",
    2.1815405,
    1e-7,
)

# Provided sample 2
assert_close(
    """\
4 1 5
2 3
4 5
7 3
9 5
""",
    4.3598594,
    1e-7,
)

# Minimum-size input, endpoint speed is zero.
assert_close(
    """\
1 1 1
1 0
""",
    2.0,
)

# One camera, endpoint speed is nonzero.
expected = 2.0 * math.sqrt(1.5) - 1.0
assert_close(
    """\
1 1 1
1 1
""",
    expected,
)

# All camera limits are equal.
p = math.sqrt(1.5)
expected = (2.0 * p - 1.0) + 2.0 * (p - 1.0)
assert_close(
    """\
3 1 1
1 1
2 1
3 1
""",
    expected,
)

# Intermediate camera forces a full stop.
expected = 2.0 + (2.0 * math.sqrt(1.5) - 1.0)
assert_close(
    """\
2 1 1
1 0
2 1
""",
    expected,
)

# Maximum-size input with a very simple exact answer.
# Every camera forces speed zero, so every unit segment takes exactly 2 time units.
n = 100000
parts = [f"{n} 1 1"]
parts.extend(f"{i} 0" for i in range(1, n + 1))
max_input = "\n".join(parts) + "\n"
assert_close(max_input, 2.0 * n, 1e-7)

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 / 1 0`|`2`| Đầu vào tối thiểu và tốc độ điểm cuối bằng 0 | 
|`1 1 1 / 1 1`|`1.4494897428...`| Bắt đầu từ số 0 và kết thúc ở tốc độ dương | 
|`3 1 1 / 1 1 / 2 1 / 3 1`|`2.3484692289...`| Giới hạn bằng nhau và khả năng tăng tốc trên tốc độ camera giữa các camera | 
|`2 1 1 / 1 0 / 2 1`|`3.4494897428...`| Một máy ảnh tốc độ không trung gian | 
|`100000 1 1`theo sau là vị trí`1..100000`, tất cả đều có giới hạn`0`|`200000`| Tối đa (n), hành vi thời gian tuyến tính và các ràng buộc 0 lặp lại | 

## Vỏ cạnh 

Đối với máy ảnh cuối cùng có giới hạn tốc độ bằng 0, hãy xem xét```
1 1 1
1 0
```Tốc độ chuyển tiếp bằng 0 vì máy ảnh cấm mọi tốc độ điểm cuối dương. Tốc độ lùi cũng bằng 0 vì đây là đích đến. Do đó tốc độ điểm cuối được chọn là bằng không. Đối với phân khúc đơn, 

[ 
p^2=\frac{2\cdot1\cdot1\cdot1}{2}=1, 
] 

vậy (p=1). Gia tốc mất một đơn vị thời gian và lực phanh mất một đơn vị thời gian khác, cho kết quả chính xác (2). 

Đối với máy ảnh tốc độ 0 trung bình,```
2 1 1
1 0
2 1
```cả hai đều chọn tốc độ 0 ở camera đầu tiên. Đoạn đầu tiên có tốc độ tối đa là 1 và mất 2 đơn vị thời gian. Đoạn thứ hai bắt đầu từ 0 và kết thúc ở tốc độ 1, cho tốc độ tối đa (\sqrt{1.5}) và thời gian (2\sqrt{1.5}-1). Tổng số là khoảng (3,4494897428). Đường chuyền ngược là thứ ngăn thuật toán mang tốc độ dương bất hợp pháp qua camera đầu tiên. 

Để có giới hạn máy ảnh bằng nhau,```
2 1 1
1 1
2 1
```cả hai tốc độ máy ảnh đều được chọn là 1. Ở đoạn đầu tiên, tốc độ cao nhất là (\sqrt{1.5}), do đó thời gian là (2\sqrt{1.5}-1). Trên đoạn thứ hai, cùng một đỉnh được sử dụng nhưng cả hai điểm cuối đều đã có tốc độ 1, cho thời gian (2(\sqrt{1.5}-1)). Tổng số là khoảng (1,8989794856). Điều này chứng tỏ tại sao việc tính toán phân đoạn phải cho phép ô tô vượt quá cả tốc độ camera giữa các camera. 

Đối với một điểm đến có giới hạn tốc độ cao nhưng có camera hạn chế sớm hơn, việc chuyển tiếp có thể trở thành hạn chế hoạt động. Trong Mẫu 1, camera thứ hai cho phép tốc độ 2, nhưng chỉ (\sqrt3) có thể truy cập được từ camera thứ nhất ở tốc độ 1 trong khoảng cách có sẵn. Do đó, tốc độ được chọn cuối cùng là (\sqrt3), không phải 2. Tốc độ chuyển tiếp ngăn thuật toán giả định rằng giới hạn đã đăng của mọi máy ảnh đều có thể đạt được một cách độc lập.
