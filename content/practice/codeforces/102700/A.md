---
title: "CF 102700A - Phương pháp tiếp cận"
description: "Có hai người bạn cùng di chuyển trên máy bay. Chuyến đầu tiên xuất phát tại điểm (A) và đi thẳng về phía (B). Người thứ hai bắt đầu tại (C) và đi thẳng về phía (D)."
date: "2026-08-08T08:09:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102700
codeforces_index: "A"
codeforces_contest_name: "2020 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102700
solve_time_s: 228
verified: true
draft: false
---

[CF 102700A - Phương pháp tiếp cận](https://codeforces.com/problemset/problem/102700/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 48 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có hai người bạn cùng di chuyển trên máy bay. Chuyến đầu tiên xuất phát tại điểm (A) và đi thẳng về phía (B). Người thứ hai bắt đầu tại (C) và đi thẳng về phía (D). Vận tốc của họ bằng nhau nên nếu ta chọn vận tốc chung là (1) thì thời gian mỗi người cần đi chính xác là độ dài quãng đường của họ. Khi một người bạn đến đích, người bạn đó dừng lại ở đó. 

Dữ liệu đầu vào chứa bốn tọa độ của (A) và (B) trên dòng đầu tiên, tiếp theo là bốn tọa độ của (C) và (D) trên dòng thứ hai. Đầu ra cần thiết là khoảng cách Euclide nhỏ nhất giữa hai người bạn tại bất kỳ thời điểm nào kể từ khi họ khởi hành đồng thời cho đến khi cả hai đều đến đích. 

Tọa độ có thể có giá trị tuyệt đối lên tới (10^9). Điều này làm cho khoảng cách bình phương lớn đến khoảng (10^{18}), do đó việc triển khai cần số học có thể biểu thị các giá trị đó một cách an toàn. Số nguyên Python có độ chính xác tùy ý và dấu phẩy động chỉ cần thiết khi chúng ta tính toán thời gian thu nhỏ thực tế hoặc căn bậc hai cuối cùng. Vì chỉ có bốn điểm và không có tham số nào phụ thuộc vào kích thước đầu vào nên giải pháp mong muốn sẽ mất thời gian không đổi. 

Một lỗi phổ biến là chỉ so sánh khoảng cách ban đầu và khoảng cách cuối cùng. Ví dụ,```
0 0 10 0
5 5 5 -5
```Khoảng cách ban đầu là (5\sqrt{2}), trong khi khoảng cách cuối cùng cũng là (5\sqrt{2}), nhưng những người bạn sẽ tiến lại gần hơn khi di chuyển. Tại thời điểm (5), bạn thứ nhất ở ((5,0)) và bạn thứ hai ở ((5,0)) nên đáp án đúng là (0). Chỉ kiểm tra các điểm cuối sẽ bỏ lỡ mức tối thiểu thực tế. 

Một trường hợp khác xảy ra khi một người bạn đến sớm hơn. Ví dụ,```
0 0 1 0
2 0 2 10
```Người bạn đầu tiên đến ((1,0)) sau một đơn vị thời gian và ở lại đó. Người bạn thứ hai tiếp tục đi được mười đơn vị. Khoảng cách tối thiểu là (1), đạt được tại thời điểm người bạn đầu tiên đến đích. Việc coi cả hai quỹ đạo như thể chúng tiếp tục vô tận sẽ xem xét các vị trí mà người bạn đầu tiên chưa bao giờ thực sự chiếm giữ và có thể tạo ra một câu trả lời không hợp lệ. 

Trường hợp thứ ba là khi vận tốc tương đối bằng 0 trong một khoảng thời gian. Ví dụ,```
0 0 10 0
0 1 10 1
```Hai người bạn chuyển động song song với cùng vận tốc nên khoảng cách của họ luôn bằng (1). Một công thức chia cho vận tốc tương đối bình phương mà không kiểm tra xem nó có bằng 0 hay không sẽ chia cho 0 mặc dù bản thân vấn đề đã được xác định hoàn toàn rõ ràng. 

Cuối cùng, mức tối thiểu có thể xảy ra chính xác khi một người bạn đến đích. Ví dụ,```
0 0 2 0
1 1 1 -1
```Tại thời điểm (1), người bạn thứ hai đến ((1,-1)), trong khi người bạn thứ nhất ở ((1,0)), cho khoảng cách (1). Vì khoảng thời gian liên quan được đóng tại thời điểm đến nên việc triển khai phải cho phép thời gian giảm thiểu bằng với một trong hai điểm cuối. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng chuyển động theo từng khoảng thời gian rất nhỏ, tính toán cả hai vị trí ở mỗi khoảng tăng và giữ khoảng cách nhỏ nhất tìm được. Về mặt khái niệm, điều này dễ dàng vì quỹ đạo thực tế chỉ là hai đoạn thẳng theo sau là các điểm dừng. Vấn đề là không có bước mô phỏng cố định có ý nghĩa. Với tọa độ lên tới (10^9), một tuyến đường có thể có độ dài theo thứ tự (10^9), do đó, ngay cả một mô phỏng sử dụng một triệu hoặc một tỷ mẫu thời gian cũng sẽ quá không chính xác hoặc quá chậm. Quan trọng hơn, mô phỏng số có thể bỏ qua thời điểm chính xác khi mức tối thiểu xảy ra, do đó nó không thể cung cấp độ chính xác cần thiết (10^{-6}) một cách đáng tin cậy. 

Thay vào đó, lực lượng vũ phu có thể lấy mẫu các điểm (K) trên mỗi quỹ đạo. Việc đánh giá khoảng cách đó tốn (O(K)), nhưng (K) phải được chọn đủ lớn để đảm bảo độ chính xác được yêu cầu cho mọi cấu hình tọa độ có thể có. Vì cực tiểu có thể xảy ra trong một vùng hẹp tùy ý khi tọa độ lớn nên không có giá trị cố định thực tế (K) nào đảm bảo như vậy. 

Quan sát quan trọng là chuyển động tuyến tính từng đoạn theo thời gian. Trước khi một trong hai người bạn đến đích, cả hai vị trí đều là hàm tuyến tính của thời gian. Sau khi một người bạn đến, vị trí của người bạn đó trở nên không đổi trong khi vị trí của người bạn kia vẫn tuyến tính. Sau khi cả hai đến nơi, cả hai vị trí đều không đổi. 

Hãy xem xét bất kỳ khoảng thời gian nào mà hai vị trí có thể được viết là 

[ 
P_1(t)=P+tV_1 
] 

và 

[ 
P_2(t)=Q+tV_2. 
] 

Khi đó vị trí tương đối của chúng là 

[ 
R(t)=(P-Q)+t(V_1-V_2). 
] 

Vậy trên khoảng đó, bình phương khoảng cách là 

[ 
f(t)=|R(t)|^2. 
] 

Đây là hàm bậc hai của (t). Một bậc hai có dạng 

[ 
tại^2+bt+c 
] 

với (a\geq0) đạt cực tiểu tại đỉnh của nó hoặc tại điểm cuối của khoảng. Chúng ta có thể tính toán đỉnh trực tiếp thay vì thời gian lấy mẫu. 

Chỉ có ba giai đoạn chuyển động có thể xảy ra. Trước khi đến lần đầu tiên, cả hai người bạn đều di chuyển. Giữa lần thứ nhất và lần thứ hai, một người chuyển động, còn người kia đứng yên. Sau lần đến thứ hai, cả hai không di chuyển nên khoảng cách không đổi. Chúng ta có thể xử lý các khoảng này một cách độc lập và lấy khoảng cách nhỏ nhất. 

Phương pháp brute-force hoạt động vì mỗi thời gian lấy mẫu đều cho một cặp vị trí hợp lệ, nhưng nó thất bại vì không có mật độ lấy mẫu hữu hạn đảm bảo độ chính xác cần thiết. Việc quan sát thấy rằng mỗi pha có chuyển động tương đối tuyến tính làm giảm toàn bộ bài toán để giảm thiểu tối đa ba phương trình bậc hai lồi một chiều, đòi hỏi thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | (O(K)) | (O(1)) | Quá chậm và không đảm bảo chính xác | 
| Tối thiểu hóa bậc hai từng phần tối ưu | (O(1)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bốn điểm và tính các vectơ chuyển động (B-A) và (D-C). Độ dài của họ là tổng thời gian di chuyển vì cả hai người bạn đều có tốc độ chung (1). 
2. Biểu diễn vị trí của mỗi người bạn tại thời điểm (t) bằng vectơ vận tốc. Trước khi đến, vận tốc là hướng chuẩn hóa hướng tới đích. Sau khi đến, vận tốc bằng không. 

Việc chuẩn hóa là thuận tiện vì tốc độ vật lý bằng nhau có nghĩa là cả hai vectơ vận tốc phải có độ lớn (1), bất kể đường đi tương ứng của chúng dài bao nhiêu. 
3. Đặt (T_1=|B-A|) và (T_2=|D-C|). Chia dòng thời gian tại (0), (\min(T_1,T_2)) và (\max(T_1,T_2)). Trên mỗi khoảng thời gian kết quả, vận tốc của mỗi người bạn là cố định. 
4. Đối với một khoảng ([L,R]), hãy viết vị trí tương đối là 

[ 
X(t)=X_0+tV, 
] 

trong đó (X_0) là vị trí tương đối tại thời điểm 0 và (V) là vận tốc tương đối. 

Bình phương khoảng cách là 

[ 
|X_0+tV|^2. 
] 

Việc mở rộng nó mang lại

[ 
|V|^2t^2+2(X_0\cdot V)t+|X_0|^2. 
] 
5. Nếu (V\neq0), đạo hàm bậc hai. Mức tối thiểu không bị ràng buộc của nó xảy ra tại 

[ 
t^*=-\frac{X_0\cdot V}{|V|^2}. 
] 

Vì chúng ta chỉ được phép sử dụng thời gian bên trong ([L,R]), hãy kẹp giá trị này vào khoảng đó. Thời gian ứng tuyển là 

[ 
\max(L,\min(R,t^*)). 
] 

Kẹp xử lý trường hợp phương trình bậc hai tiếp tục giảm cho đến khi một người bạn đến đích. 
6. Nếu (V=0), vị trí tương đối không thay đổi trong suốt khoảng thời gian, do đó mọi thời điểm đều có khoảng cách như nhau. Chúng ta có thể chỉ cần đánh giá một điểm cuối. 
7. Đánh giá mức tối thiểu trên tất cả các giai đoạn chuyển động và lấy khoảng cách bình phương nhỏ nhất. Cuối cùng, lấy căn bậc hai của nó và in nó với độ chính xác vừa đủ. 

### Tại sao nó hoạt động 

Điều bất biến là, trên mỗi khoảng thời gian được xử lý, hai công thức vị trí mô tả chính xác vị trí của bạn bè vào mọi thời điểm trong khoảng thời gian đó. Vị trí tương đối của chúng là tuyến tính theo thời gian, nên bình phương khoảng cách của chúng là phương trình bậc hai lồi. Điểm cực tiểu của một phương trình bậc hai lồi trên một khoảng đóng là đỉnh của nó khi đỉnh đó nằm trong khoảng, nếu không thì là một trong các điểm cuối của khoảng. Bước kẹp sẽ kiểm tra chính xác điểm đó. Vì dòng thời gian hoàn chỉnh được phân chia tại mọi thời điểm khi vận tốc của một người bạn thay đổi nên mọi thời gian có thể đều được bao phủ bởi một trong những khoảng thời gian này. Do đó, việc chọn ứng viên nhỏ nhất trong tất cả các khoảng sẽ cho khoảng cách tối thiểu toàn cục. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def dot(a, b):
    return a[0] * b[0] + a[1] * b[1]

def norm(a):
    return math.hypot(a[0], a[1])

def position_at(p, v, t):
    return (p[0] + v[0] * t, p[1] + v[1] * t)

def solve():
    ax, ay, bx, by = map(int, input().split())
    cx, cy, dx, dy = map(int, input().split())

    a = (float(ax), float(ay))
    b = (float(bx), float(by))
    c = (float(cx), float(cy))
    d = (float(dx), float(dy))

    ab = (b[0] - a[0], b[1] - a[1])
    cd = (d[0] - c[0], d[1] - c[1])

    t1 = norm(ab)
    t2 = norm(cd)

    if t1 > 0:
        v1 = (ab[0] / t1, ab[1] / t1)
    else:
        v1 = (0.0, 0.0)

    if t2 > 0:
        v2 = (cd[0] / t2, cd[1] / t2)
    else:
        v2 = (0.0, 0.0)

    def state(t):
        if t < t1:
            p1 = position_at(a, v1, t)
        else:
            p1 = b

        if t < t2:
            p2 = position_at(c, v2, t)
        else:
            p2 = d

        return (
            p1[0] - p2[0],
            p1[1] - p2[1],
        )

    def distance_sq(t):
        r = state(t)
        return r[0] * r[0] + r[1] * r[1]

    # The only times where velocities can change are the arrival times.
    cuts = sorted(set([0.0, t1, t2]))

    ans = float("inf")

    # Before the first arrival, both move.
    # Between arrival times, one may be stationary.
    # The final interval is constant, but processing it is harmless.
    for i in range(len(cuts)):
        if i + 1 < len(cuts):
            l = cuts[i]
            r = cuts[i + 1]
        else:
            # There is no need to search after both have arrived.
            continue

        mid = (l + r) * 0.5

        if mid < t1:
            vv1 = v1
        else:
            vv1 = (0.0, 0.0)

        if mid < t2:
            vv2 = v2
        else:
            vv2 = (0.0, 0.0)

        rel_v = (vv1[0] - vv2[0], vv1[1] - vv2[1])

        rel_l = state(l)

        vv2_norm_sq = dot(rel_v, rel_v)

        if vv2_norm_sq == 0.0:
            candidate = l
        else:
            optimum = -dot(rel_l, rel_v) / vv2_norm_sq
            candidate = max(l, min(r, optimum))

        ans = min(ans, distance_sq(candidate))

    # Both friends are stationary from max(t1, t2) onward.
    ans = min(ans, distance_sq(max(t1, t2)))

    print(f"{math.sqrt(ans):.12f}")

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai sẽ tính toán độ dài của hai tuyến đường và chuyển đổi mỗi tuyến đường thành một vectơ vận tốc đơn vị. Điều này diễn ra trực tiếp từ việc chọn tốc độ đi bộ phổ biến như (1). Nếu một tuyến đường có độ dài bằng 0 thì người bạn tương ứng đã ở đích đến, do đó vận tốc của nó được đặt thành 0 thay vì cố gắng chia cho độ dài bằng 0. 

các`state`chức năng thực hiện chuyển động từng phần. Việc so sánh với thời gian đến sẽ xác định xem một người bạn còn đang di chuyển hay đã đến đích. Bản thân điểm cuối được xử lý như một vị trí đích, giống hệt về mặt toán học với công thức di chuyển tại thời điểm chính xác đó. 

các`cuts`mảng chứa mọi thời điểm khi vận tốc có thể thay đổi. Có nhiều nhất ba thời điểm khác nhau nên vòng lặp chỉ thực hiện một số lần lặp không đổi. Đối với mỗi khoảng, điểm giữa chỉ được sử dụng để xác định vận tốc nào hoạt động trong suốt khoảng mở đó. Điểm giữa không phải là một con số gần đúng cho câu trả lời.`rel_l`là vị trí tương đối tại điểm cuối bên trái của khoảng. Vì vận tốc tương đối không đổi trong suốt khoảng thời gian nên vị trí tương đối tại bất kỳ thời điểm (t) nào trong khoảng thời gian đó là 

[ 
\text{rel__l+(t-L)\text{rel__v. 
] 

Thay vào đó, mã sử dụng biểu thức thời gian toàn cầu tương đương khi lấy đỉnh. Bởi vì`rel_l`được đánh giá tại thời điểm (L), đỉnh chính xác theo thời gian toàn cầu phải là 

[ 
L-\frac{\text{rel__l\cdot\text{rel__v}{|\text{rel__v|^2}. 
] 

Vì vậy việc thực hiện nên sử dụng hình thức đó. Việc thực hiện sửa chữa hoàn chỉnh là dưới đây.```python
import sys
import math

input = sys.stdin.readline

def solve():
    ax, ay, bx, by = map(int, input().split())
    cx, cy, dx, dy = map(int, input().split())

    a = (float(ax), float(ay))
    b = (float(bx), float(by))
    c = (float(cx), float(cy))
    d = (float(dx), float(dy))

    ab = (b[0] - a[0], b[1] - a[1])
    cd = (d[0] - c[0], d[1] - c[1])

    t1 = math.hypot(ab[0], ab[1])
    t2 = math.hypot(cd[0], cd[1])

    v1 = (0.0, 0.0) if t1 == 0.0 else (ab[0] / t1, ab[1] / t1)
    v2 = (0.0, 0.0) if t2 == 0.0 else (cd[0] / t2, cd[1] / t2)

    def position(p, v, t, total, destination):
        if t >= total:
            return destination
        return (p[0] + v[0] * t, p[1] + v[1] * t)

    def relative_position(t):
        p1 = position(a, v1, t, t1, b)
        p2 = position(c, v2, t, t2, d)
        return (p1[0] - p2[0], p1[1] - p2[1])

    def dist_sq(t):
        x, y = relative_position(t)
        return x * x + y * y

    cuts = sorted(set((0.0, t1, t2)))
    ans = float("inf")

    for i in range(len(cuts) - 1):
        l = cuts[i]
        r = cuts[i + 1]

        mid = (l + r) * 0.5

        active_v1 = v1 if mid < t1 else (0.0, 0.0)
        active_v2 = v2 if mid < t2 else (0.0, 0.0)

        rv = (
            active_v1[0] - active_v2[0],
            active_v1[1] - active_v2[1],
        )

        rel_l = relative_position(l)

        rv_sq = rv[0] * rv[0] + rv[1] * rv[1]

        if rv_sq == 0.0:
            best_t = l
        else:
            # rel(t) = rel_l + (t-l) * rv
            # Minimize |rel(t)|^2.
            tau = -(rel_l[0] * rv[0] + rel_l[1] * rv[1]) / rv_sq
            best_t = max(l, min(r, l + tau))

        ans = min(ans, dist_sq(best_t))

    ans = min(ans, dist_sq(max(t1, t2)))

    print(f"{math.sqrt(ans):.12f}")

if __name__ == "__main__":
    solve()
```Phiên bản thứ hai là phiên bản để gửi. Sự khác biệt giữa`tau`Và`best_t`là một chi tiết triển khai hữu ích.`tau`đo thời gian so với thời điểm bắt đầu khoảng thời gian hiện tại, trong khi`best_t`là giờ toàn cầu thực tế. Việc trộn lẫn hai hệ tọa độ này là nguyên nhân điển hình của việc thu nhỏ không chính xác. 

Không xảy ra tràn số nguyên trong Python vì số nguyên có độ chính xác tùy ý. Các tọa độ được chuyển đổi thành dấu phẩy động trước khi chuẩn hóa vận tốc, vì các vectơ chuẩn hóa và độ dài tuyến đường yêu cầu số học thực. Khả năng chịu lỗi cuối cùng của (10^{-6}) được xử lý thoải mái bằng cách in mười hai chữ số sau dấu thập phân. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
0 0 1 0
2 0 2 1
```Người bạn đầu tiên đi một đơn vị theo chiều ngang nên (T_1=1). Vật thứ hai di chuyển một đơn vị theo phương thẳng đứng nên (T_2=1). Do đó, cả hai đều di chuyển trong cùng một khoảng ([0,1]). 

Vận tốc là ((1,0)) và ((0,1)), do đó vận tốc tương đối là ((1,-1)). Vị trí tương đối ban đầu là ((-2,0)). 

| Khoảng thời gian | Vị trí tương đối ở bên trái | Vận tốc tương đối | Thời gian tương đối không được kẹp | Thời điểm đã chọn | Khoảng cách | 
| --- | --- | --- | --- | --- | --- | 
| ([0,1]) | ((-2,0)) | ((1,-1)) | (1) | (1) | (\sqrt{2}) | 

Tại thời điểm (1), các bạn lần lượt ở ((1,0)) và ((2,1)). Hiệu của chúng là ((-1,-1)), có độ dài là (\sqrt{2}). Do đó, đầu ra xấp xỉ (1.414213562373). 

Ví dụ này minh họa trường hợp thông thường trong đó cả hai người bạn đều di chuyển trong toàn bộ khoảng thời gian có liên quan và cực tiểu bậc hai xảy ra chính xác tại ranh giới khoảng. 

### Mẫu 2 

Đầu vào là```
-2 -4 -2 -2
-3 -5 -3 -2
```Cả hai người bạn đều di chuyển theo chiều dọc lên trên. Tuyến đầu tiên có chiều dài (2), trong khi tuyến thứ hai có chiều dài (3). Vận tốc của chúng đều bằng ((0,1)), nên vận tốc tương đối của chúng bằng 0 trong hai đơn vị thời gian đầu tiên. 

| Khoảng thời gian | Vị trí tương đối ở bên trái | Vận tốc tương đối | Thời điểm đã chọn | Khoảng cách | 
| --- | --- | --- | --- | --- | 
| ([0,2]) | ((1,1)) | ((0,0)) | (0) | (\sqrt{2}) | 
| ([2,3]) | ((1,1)) | ((0,-1)) | (3) | (1) | 

Trong khoảng thời gian đầu tiên, cả hai người bạn đều di chuyển cùng nhau nên sự tách biệt của họ vẫn còn (\sqrt{2}). Tại thời điểm (2), người bạn thứ nhất dừng lại ở ((-2,-2)), còn người thứ hai dừng lại ở ((-3,-3)). Giây thứ hai tiếp tục đi lên, đạt ((-3,-2)) tại thời điểm (3), khi khoảng cách trở thành (1). Do đó, câu trả lời là (1). 

Dấu vết này xác nhận cụ thể rằng vận tốc tương đối bằng 0 phải được xử lý mà không phân chia và mức tối thiểu có thể xảy ra sau khi một người bạn đã dừng lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(1)) | Có nhiều nhất ba ranh giới dòng thời gian riêng biệt và hai khoảng thời gian di chuyển cần giảm thiểu. | 
| Không gian | (O(1)) | Chỉ có một số lượng điểm, vectơ và giá trị vô hướng không đổi được lưu trữ. | 

Giới hạn tọa độ không ảnh hưởng đến số lượng phép toán vì thuật toán không bao giờ lấy mẫu mặt phẳng tọa độ hoặc trục thời gian. Mặc dù tọa độ có thể lớn bằng (10^9), nhưng chỉ có một số lượng phép toán số học không đổi được thực hiện, do đó, giải pháp dễ dàng nằm gọn trong giới hạn thời gian một giây và giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def solve():
    input = sys.stdin.readline

    ax, ay, bx, by = map(int, input().split())
    cx, cy, dx, dy = map(int, input().split())

    a = (float(ax), float(ay))
    b = (float(bx), float(by))
    c = (float(cx), float(cy))
    d = (float(dx), float(dy))

    ab = (b[0] - a[0], b[1] - a[1])
    cd = (d[0] - c[0], d[1] - c[1])

    t1 = math.hypot(ab[0], ab[1])
    t2 = math.hypot(cd[0], cd[1])

    v1 = (0.0, 0.0) if t1 == 0 else (ab[0] / t1, ab[1] / t1)
    v2 = (0.0, 0.0) if t2 == 0 else (cd[0] / t2, cd[1] / t2)

    def position(p, v, t, total, destination):
        if t >= total:
            return destination
        return (p[0] + v[0] * t, p[1] + v[1] * t)

    def relative_position(t):
        p1 = position(a, v1, t, t1, b)
        p2 = position(c, v2, t, t2, d)
        return p1[0] - p2[0], p1[1] - p2[1]

    def dist_sq(t):
        x, y = relative_position(t)
        return x * x + y * y

    cuts = sorted(set((0.0, t1, t2)))
    ans = float("inf")

    for i in range(len(cuts) - 1):
        l, r = cuts[i], cuts[i + 1]
        mid = (l + r) * 0.5

        active_v1 = v1 if mid < t1 else (0.0, 0.0)
        active_v2 = v2 if mid < t2 else (0.0, 0.0)

        rv = (
            active_v1[0] - active_v2[0],
            active_v1[1] - active_v2[1],
        )

        rel_l = relative_position(l)
        rv_sq = rv[0] * rv[0] + rv[1] * rv[1]

        if rv_sq == 0.0:
            best_t = l
        else:
            tau = -(rel_l[0] * rv[0] + rel_l[1] * rv[1]) / rv_sq
            best_t = max(l, min(r, l + tau))

        ans = min(ans, dist_sq(best_t))

    ans = min(ans, dist_sq(max(t1, t2)))

    print(f"{math.sqrt(ans):.12f}")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def assert_close(actual: str, expected: float, message: str):
    value = float(actual)
    assert abs(value - expected) <= 1e-6, (
        f"{message}: got {value}, expected {expected}"
    )

# Provided sample 1.
assert_close(
    run("0 0 1 0\n2 0 2 1\n"),
    math.sqrt(2),
    "sample 1"
)

# Provided sample 2.
assert_close(
    run("-2 -4 -2 -2\n-3 -5 -3 -2\n"),
    1.0,
    "sample 2"
)

# Both friends start and finish at the same places.
assert_close(
    run("0 0 0 0\n0 0 0 0\n"),
    0.0,
    "both stationary at the same point"
)

# Both move with exactly the same velocity, so the distance never changes.
assert_close(
    run("0 0 10 0\n0 1 10 1\n"),
    1.0,
    "parallel identical motion"
)

# The friends meet exactly when the first friend reaches the destination.
assert_close(
    run("0 0 10 0\n5 5 5 -5\n"),
    0.0,
    "minimum exactly at an arrival boundary"
)

# One friend is already at its destination while the other moves.
assert_close(
    run("0 0 0 0\n2 0 2 10\n"),
    2.0,
    "zero-length first route"
)

# Large coordinates, checking arithmetic and precision.
assert_close(
    run("1000000000 1000000000 -1000000000 1000000000\n"
        "1000000000 1000000001 -1000000000 1000000001\n"),
    1.0,
    "large coordinates"
)

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 0 0`Và`0 0 0 0`|`0`| Cả hai người bạn đều đứng yên ở cùng một vị trí. | 
|`0 0 10 0`Và`0 1 10 1`|`1`| Vận tốc tương đối bằng 0 nên không có sự chia cho 0. | 
|`0 0 10 0`Và`5 5 5 -5`|`0`| Mức tối thiểu xảy ra chính xác tại một ranh giới pha. | 
|`0 0 0 0`Và`2 0 2 10`|`2`| Một tuyến đường có độ dài bằng 0 và người bạn tương ứng đứng yên ngay từ đầu. | 
| Tọa độ độ lớn (10^9) |`1`| Tọa độ lớn và độ chính xác của dấu phẩy động. | 

## Vỏ cạnh 

### Những người bạn gặp nhau ở ranh giới nơi đến 

Hãy xem xét```
0 0 10 0
5 5 5 -5
```Cả hai tuyến đường mất (10) đơn vị thời gian. Tại (t=5), người bạn thứ nhất ở ((5,0)), trong khi người thứ hai cũng ở ((5,0)). Vị trí tương đối chính xác bằng 0, nên đáp án là (0). 

Việc giảm thiểu khoảng tính toán mức tối thiểu bậc hai không bị ràng buộc tại (t=5). Vì (5) nằm trong khoảng đóng ([0,10]) nên nó được chấp nhận trực tiếp. Điều này nắm bắt các triển khai chỉ kiểm tra nghiêm ngặt các điểm bên trong hoặc chỉ các vị trí cuối cùng. 

### Một người bạn đến trước người kia 

Hãy xem xét```
0 0 1 0
2 0 2 10
```Người bạn đầu tiên đến ((1,0)) tại (t=1) và ở lại đó. Người bạn thứ hai đi lên từ ((2,0)). Trong giai đoạn thứ hai, vị trí tương đối được xác định bởi người bạn thứ nhất đứng yên và người bạn thứ hai đang chuyển động. Điểm gần nhất là tại (t=1), với khoảng cách (1). 

Dòng thời gian được chia ra ở (t=1), do đó thuật toán không vô tình tiếp tục vận tốc của người bạn đầu tiên sau khi đến. 

### Vận tốc tương đối bằng không 

Hãy xem xét```
0 0 10 0
0 1 10 1
```Cả hai người bạn đều có vận tốc ((1,0)). Vận tốc tương đối của chúng là ((0,0)) và vị trí tương đối của chúng luôn là ((0,-1)). Do đó khoảng cách luôn là (1). 

Thuật toán phát hiện vận tốc tương đối bình phương bằng 0 và bỏ qua công thức đỉnh. Bất kỳ điểm nào trong khoảng đều là tối ưu, do đó việc đánh giá điểm cuối bên trái là đủ. 

###Một người bạn đứng yên ngay từ đầu 

Hãy xem xét```
0 0 0 0
2 0 2 10
```Người bạn đầu tiên có lộ trình dài bằng 0 và không bao giờ di chuyển. Vật thứ hai bắt đầu ở khoảng cách (2) so với vật thứ nhất và di chuyển theo phương thẳng đứng, do đó khoảng cách tối thiểu là (2) tại (t=0). 

Việc kiểm tra độ dài tuyến đường tạo ra vận tốc bằng 0 cho người bạn đầu tiên. Chuyển động của người bạn thứ hai sau đó được xử lý bình thường, với người bạn đầu tiên được coi là đứng yên trong suốt khoảng thời gian liên quan. 

### Giá trị tối thiểu là ở điểm cuối khoảng 

Giả sử hàm khoảng cách bậc hai giảm trong suốt một khoảng và sẽ đạt đến đỉnh của nó sau khi khoảng đó kết thúc. Đỉnh đó tương ứng với thời điểm mà một trong những người bạn đã thay đổi vận tốc nên không thể sử dụng nó cho pha đó. 

Hoạt động kẹp thay đổi một đỉnh ngoài phạm vi thành (L) hoặc (R). Vì mọi thời gian thay đổi vận tốc đều được bao gồm một cách rõ ràng trong`cuts`, điểm cuối chính xác vẫn được đánh giá là một phần của pha liền kề. Điều này ngăn ngừa lỗi phổ biến khi chấp nhận mức tối thiểu bậc hai hợp lệ về mặt toán học tại thời điểm vận tốc giả định không còn hợp lệ.
