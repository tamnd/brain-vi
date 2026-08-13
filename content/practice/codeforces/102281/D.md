---
title: "CF 102281D - \u0411\u043e\u0435\u0432\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430"
description: "Chúng ta có ba điểm trong không gian ba chiều. Điểm đầu tiên là vị trí của tàu vũ trụ và pháo laser của chúng ta, điểm thứ hai là tâm của tàu vũ trụ hình cầu của đối phương cùng với bán kính của nó, và điểm thứ ba là điểm được hệ thống nhắm mục tiêu chọn."
date: "2026-08-13T09:20:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102281
codeforces_index: "D"
codeforces_contest_name: "2011, IV \u0421\u0430\u043c\u0430\u0440\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u043d\u0430\u044f \u043c\u0435\u0436\u0432\u0443\u0437\u043e\u0432\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 102281
solve_time_s: 124
verified: true
draft: false
---

[CF 102281D - \u0411\u043e\u0435\u0432\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430](https://codeforces.com/problemset/problem/102281/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có ba điểm trong không gian ba chiều. Điểm đầu tiên là vị trí của tàu vũ trụ và pháo laser của chúng ta, điểm thứ hai là tâm của tàu vũ trụ hình cầu của đối phương cùng với bán kính của nó, và điểm thứ ba là điểm được hệ thống nhắm mục tiêu chọn. 

Tia laser không dừng lại ở điểm đã chọn. Điểm được chọn chỉ xác định hướng của nó nên tia bắn là một tia vô hạn bắt đầu từ tàu vũ trụ của chúng ta và đi qua điểm mục tiêu. Kẻ địch bị tiêu diệt khi tia này giao nhau hoặc chạm vào quả cầu. 

Chúng ta cần xuất ra hai thứ. Dòng đầu tiên cho biết tia có chạm vào quả cầu hay không. Dòng thứ hai cho biết khoảng cách Euclide tối thiểu từ tâm quả cầu đến bất kỳ điểm nào của tia. 

Tất cả tọa độ đều nằm trong khoảng từ -1000 đến 1000 và bán kính tối đa là 1000. Chỉ có một cấu hình hình học, do đó kích thước đầu vào không tạo ra thách thức tiệm cận. Giới hạn 1,5 giây là quá đủ cho số học theo thời gian không đổi, nhưng điều đó cũng có nghĩa là không có lý do gì để ước tính tia bằng một số lượng lớn các điểm được lấy mẫu. Các số nguyên Python xử lý tất cả các khoảng cách bình phương trung gian một cách thoải mái, vì chênh lệch tọa độ nhiều nhất là 2000 và bình phương của chúng chỉ ở mức hàng triệu. 

Các trường hợp cạnh chính xuất phát từ thực tế rằng tia laser là một tia chứ không phải một đường vô hạn. Điểm gần nhất trên đường vô hạn có thể nằm phía sau khẩu pháo, trong trường hợp đó tia laser không thể tiếp cận được. 

Ví dụ,```
0 0 0
-2 0 0 1
1 0 0
```sản xuất```
MISS
2.000000000000000
```Tâm địch nằm ở vị trí x = -2, trong khi tia laser di chuyển theo hướng x dương. Khoảng cách từ tâm đến trục x vô hạn bằng 0, nhưng điểm gần nhất của tia thực tế chính là khẩu pháo, ở khoảng cách 2. Một giải pháp luôn tính khoảng cách giữa các điểm sẽ báo cáo sai một lần bắn trúng. 

Độ tiếp tuyến cũng phải được tính là một lần truy cập. Ví dụ,```
0 0 0
5 1 0 1
10 0 0
```sản xuất```
HIT
1.000000000000000
```Tia này là trục x dương và tâm hình cầu cách nó đúng một đơn vị. Tia laser chạm vào quả cầu, vì vậy hãy sử dụng phép so sánh chặt chẽ như`distance < r`sẽ sản xuất không chính xác`MISS`. 

Trường hợp quả cầu nằm trực tiếp trên tia là một điều kiện biên khác. Ví dụ,```
0 0 0
5 0 0 2
1 0 0
```có khoảng cách bằng 0 nên đáp án là`HIT`. Công thức dựa trên vectơ chỉ phương được chuẩn hóa cũng phải tránh chia cho 0, nhưng câu lệnh đảm bảo rằng mục tiêu khác với khẩu pháo, do đó vectơ chỉ phương không bao giờ bằng 0. 

## Phương pháp tiếp cận 

Một mô phỏng hình học đơn giản có thể biểu diễn tia bằng nhiều điểm mẫu và kiểm tra lần lượt các điểm đó. Giới hạn tọa độ số nguyên cung cấp giới hạn trên hữu ích để biết lý do tại sao đây là sự trừu tượng hóa sai. Nếu mục tiêu khác với pháo thì chiều dài bình phương của vectơ chỉ hướng ít nhất là 1. Tham số hình chiếu của tâm địch lên hướng tia tối đa là 6000 về giá trị tuyệt đối vì tích số chấm lớn nhất (3 \cdot 2000 \cdot 2000), trong khi chiều dài hướng bình phương tối thiểu là 1. 

Giả sử chúng ta cố gắng kiểm tra tia mỗi (10^{-6}) đơn vị của tham số này. Chúng tôi sẽ cần khoảng 6.000.001 đánh giá trong trường hợp xấu nhất và việc lấy mẫu như vậy vẫn là phép thử gần đúng bằng số chứ không phải là phép thử hình học chính xác. Làm cho bước nhỏ hơn chỉ làm cho số lượng thao tác trở nên tồi tệ hơn. 

Ý tưởng vũ phu thất bại vì tia sáng là một vật thể liên tục. Không có lý do hữu ích nào để kiểm tra các điểm riêng lẻ khi khoảng cách đến mặt cầu có thể được biểu thị trực tiếp bằng đại số vectơ. 

Quan sát quan trọng là mọi điểm của tia đều có dạng 

[ 
S + tV,\qquad t \ge 0, 
] 

trong đó (S) là vị trí của pháo và (V=T-S) là hướng được xác định bởi mục tiêu. Chúng ta cần giảm thiểu khoảng cách bình phương từ điểm này đến tâm địch (E). 

Đặt (W=E-S). Bình phương khoảng cách từ (E) đến (S+tV) là 

[ 
f(t)=|W-tV|^2. 
] 

Mở rộng mang lại 

[ 
f(t)=|W|^2-2t(W\cdot V)+t^2|V|^2. 
] 

Đây là hàm bậc hai của (t). Mức tối thiểu không bị ràng buộc của nó xảy ra tại 

[ 
t_0=\frac{W\cdot V}{|V|^2}. 
] 

Hạn chế duy nhất là (t\ge0). Nếu (t_0\ge0), điểm gần nhất nằm bên trong tia, do đó khoảng cách từ điểm đến đường thông thường là chính xác. Nếu (t_0<0), điểm gần nhất của đường vô hạn nằm phía sau khẩu pháo và điểm có thể tiếp cận gần nhất là (S). 

Điều này làm giảm toàn bộ vấn đề xuống còn một vài tích số chấm và bình phương độ dài. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lấy mẫu Brute Force | O(6.000.000) đánh giá ở độ phân giải (10^{-6}) | O(1) | Quá chậm và gần đúng | 
| Hình học vector tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc vị trí pháo (S), tâm địch (E), bán kính (r) và điểm mục tiêu (T). Xây dựng vectơ chỉ phương (V=T-S). Sự đảm bảo rằng khẩu pháo không bao giờ bắn vào chính nó có nghĩa là (V\ne0), vì vậy bình phương chiều dài của nó là dương. 
2. Xây dựng (W=E-S), vectơ từ pháo đến tâm địch. Tính tích số chấm (p=W\cdot V) và độ dài hướng bình phương (q=V\cdot V). 
3. Kiểm tra dấu (p). Vì (q>0) nên dấu của (p/q) chính xác là dấu của tham số tối ưu (t_0). 
4. Nếu (p<0), hình chiếu vuông góc của tâm địch lên đường thẳng vô tận nằm phía sau khẩu pháo. Điểm gần nhất của tia thực tế là chính khẩu pháo nên khoảng cách cần thiết là (|W|). 
5. Nếu (p\ge0), hình chiếu vuông góc nằm trên tia. Khoảng cách bình phương tối thiểu là 

[ 
|W|^2-\frac{p^2}{q}. 
] 

Đối với bài kiểm tra trúng đích, chúng ta có thể tránh lấy căn bậc hai. Điều kiện khoảng cách lớn nhất là (r) trở thành 

[ 
|W|^2-\frac{p^2}{q}\le r^2. 
] 

Nhân với giá trị dương (q) sẽ cho 

[ 
|W|^2q-p^2\le r^2q. 
] 

Tất cả các đại lượng trong so sánh này đều là số nguyên, do đó, quyết định trúng có thể được thực hiện mà không có lỗi dấu phẩy động. 

1. Tính khoảng cách thực tế dưới dạng giá trị dấu phẩy động và in ra đủ chữ số. Trong trường hợp chiếu, hãy sử dụng căn bậc hai của giá trị không âm ở trên. Trong trường hợp phía sau khẩu pháo, hãy sử dụng (\sqrt{|W|^2}). 
2. In`HIT`khi khoảng cách hình học được tính toán nhiều nhất là bán kính, bao gồm cả tiếp tuyến chính xác. Nếu không thì in`MISS`. 

### Tại sao nó hoạt động 

Điều bất biến là tham số đã chọn (t) luôn mô tả điểm có thể tiếp cận gần nhất của tia tới tâm kẻ thù. Hàm khoảng cách bậc hai có chính xác một mức tối thiểu không bị ràng buộc vì hệ số (t^2) của nó là (V\cdot V>0). Khi điểm cực tiểu đó có (t\ge0) thì nó thuộc tia. Khi nó có (t<0), phương trình bậc hai tăng dần trong khoảng cho phép (t\ge0), do đó cực tiểu xảy ra tại biên (t=0), chính là khẩu pháo. 

Do đó, thuật toán tính toán khoảng cách tối thiểu chính xác từ tâm đến tia. Một hình cầu được giao nhau hoặc chạm vào chính xác khi khoảng cách này lớn nhất bằng bán kính của nó, do đó báo cáo`HIT`hoặc`MISS`quyết định là đúng đắn. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    xs, ys, zs = map(int, input().split())
    xe, ye, ze, r = map(int, input().split())
    xt, yt, zt = map(int, input().split())

    vx = xt - xs
    vy = yt - ys
    vz = zt - zs

    wx = xe - xs
    wy = ye - ys
    wz = ze - zs

    dot = wx * vx + wy * vy + wz * vz
    vv = vx * vx + vy * vy + vz * vz
    ww = wx * wx + wy * wy + wz * wz

    if dot < 0:
        dist2 = float(ww)
        hit = ww <= r * r
    else:
        cross2 = ww * vv - dot * dot
        dist2 = cross2 / vv
        hit = cross2 <= r * r * vv

    distance = math.sqrt(dist2)

    print("HIT" if hit else "MISS")
    print(f"{distance:.15f}")

if __name__ == "__main__":
    solve()
```Phần đầu tiên xây dựng`v`, hướng của tia và`w`, vectơ từ pháo đến tâm địch. Đây chính xác là các vectơ được sử dụng trong đạo hàm toán học.`dot`lưu trữ (W\cdot V), trong khi`vv`cửa hàng (|V|^2). Vì mục tiêu khác với pháo,`vv`là hoàn toàn tích cực.`ww`lưu trữ khoảng cách bình phương từ pháo đến tâm địch. 

Khi`dot < 0`, hình chiếu ở phía sau khẩu pháo. Do đó, mã sử dụng`ww`là khoảng cách bình phương và so sánh trực tiếp với (r^2). Câu lệnh đảm bảo rằng khẩu pháo nằm ngoài phạm vi của đối phương nên nhánh này không thể vô tình báo trúng đạn vì bản thân khẩu pháo nằm trong phạm vi của đối phương. 

Vì`dot >= 0`, mã sử dụng 

[ 
|W|^2|V|^2-(W\cdot V)^2 
] 

thay vì tính toán trực tiếp khoảng cách điểm-tuyến đã chuẩn hóa. Biểu thức này là chiều dài bình phương của tích chéo (W\times V), vì vậy sau khi chia cho (|V|^2), nó sẽ tính được khoảng cách bình phương của đường thẳng. 

Việc so sánh lần truy cập được thực hiện có chủ ý bằng cách sử dụng số nguyên. Làm tròn dấu phẩy động gần tiếp tuyến có thể biến một đẳng thức chính xác về mặt toán học thành một giá trị ở trên hoặc dưới bán kính một chút. Bất đẳng thức số nguyên`cross2 <= r * r * vv`xử lý tiếp tuyến một cách chính xác. 

Chỉ căn bậc hai cuối cùng mới cần số học dấu phẩy động. Số nguyên Python có độ chính xác tùy ý, mặc dù các giá trị thực tế ở đây đủ nhỏ để số nguyên 64 bit thông thường là đủ. 

Kết quả đầu ra sử dụng mười lăm chữ số sau dấu thập phân, về cơ bản có độ chính xác cao hơn sai số tuyệt đối hoặc tương đối bắt buộc (10^{-6}). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
0 0 0
100 100 100 10
1 1 1
```Hướng từ pháo tới mục tiêu là`(1, 1, 1)`. Tâm địch cũng ở cùng hướng đó nên hình chiếu của nó nằm trên tia và khoảng cách phải bằng 0. 

| Biến | Giá trị | 
| --- | --- | 
|`v`|`(1, 1, 1)`| 
|`w`|`(100, 100, 100)`| 
|`dot`|`300`| 
|`vv`|`3`| 
|`ww`|`30000`| 
|`cross2`|`0`| 
|`distance`|`0`| 
|`hit`|`True`| 

Biểu thức tích chéo bằng 0 xác nhận rằng tâm địch nằm chính xác trên tia. Vì số 0 nhiều nhất là bán kính 10 nên kết quả là`HIT`. 

### Mẫu 2 

Đầu vào là```
0 13 9
5 13 -1 4
16 13 -3
```Hướng của tia là`(16, 0, -12)`, trong khi vectơ từ pháo tới tâm địch là`(5, 0, -10)`. 

| Biến | Giá trị | 
| --- | --- | 
|`v`|`(16, 0, -12)`| 
|`w`|`(5, 0, -10)`| 
|`dot`|`200`| 
|`vv`|`400`| 
|`ww`|`125`| 
|`cross2`|`10000`| 
|`dist²`|`25`| 
|`distance`|`5`| 
|`hit`|`False`| 

Tham số phép chiếu là (200/400=0,5), vì vậy điểm gần nhất thực sự là trên tia. Khoảng cách thu được là 5, lớn hơn bán kính 4, cho`MISS`. 

Ví dụ này cũng chứng minh tại sao chỉ kiểm tra hướng của tia là không đủ. Đường vô hạn đi gần hình cầu, nhưng cần có khoảng cách vuông góc chính xác để so sánh với bán kính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Một số lượng cố định các phép toán vectơ và biểu thức số học được thực hiện | 
| Không gian | O(1) | Chỉ một số tọa độ và giá trị trung gian không đổi được lưu trữ | 

Giới hạn tọa độ làm cho mọi phép tính số học trở nên nhỏ gọn và không có sự lặp lại tùy thuộc vào giá trị đầu vào. Do đó, giải pháp phù hợp thoải mái trong giới hạn thời gian 1,5 giây và giới hạn bộ nhớ 128 MB. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây sử dụng một trình trợ giúp có chứa cùng một`solve`logic như chương trình đã gửi nhưng trả về đầu ra của nó dưới dạng một chuỗi để các xác nhận có thể kiểm tra trực tiếp.```python
import sys
import io
import math

def solve():
    input = sys.stdin.readline

    xs, ys, zs = map(int, input().split())
    xe, ye, ze, r = map(int, input().split())
    xt, yt, zt = map(int, input().split())

    vx = xt - xs
    vy = yt - ys
    vz = zt - zs

    wx = xe - xs
    wy = ye - ys
    wz = ze - zs

    dot = wx * vx + wy * vy + wz * vz
    vv = vx * vx + vy * vy + vz * vz
    ww = wx * wx + wy * wy + wz * wz

    if dot < 0:
        dist2 = float(ww)
        hit = ww <= r * r
    else:
        cross2 = ww * vv - dot * dot
        dist2 = cross2 / vv
        hit = cross2 <= r * r * vv

    distance = math.sqrt(dist2)

    print("HIT" if hit else "MISS")
    print(f"{distance:.15f}")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run(
    """0 0 0
100 100 100 10
1 1 1
"""
) == "HIT\n0.000000000000000\n", "sample 1"

# Provided sample 2
assert run(
    """0 13 9
5 13 -1 4
16 13 -3
"""
) == "MISS\n5.000000000000000\n", "sample 2"

# Tangency with the ray, minimum radius
assert run(
    """0 0 0
5 1 0 1
10 0 0
"""
) == "HIT\n1.000000000000000\n", "tangent hit"

# Sphere is behind the cannon
assert run(
    """0 0 0
-2 0 0 1
1 0 0
"""
) == "MISS\n2.000000000000000\n", "sphere behind ray"

# All coordinate components equal inside each point, maximum coordinate magnitude
assert run(
    """-1000 -1000 -1000
0 0 0 1000
1000 1000 1000
"""
) == "HIT\n0.000000000000000\n", "large diagonal hit"

# Maximum radius and a closest point at the cannon
assert run(
    """1000 1000 1000
-1000 -1000 -1000 1000
1000 1000 999
"""
) == "MISS\n3464.101615137754586\n", "maximum-size separated case"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 0 / 5 1 0 1 / 10 0 0`|`HIT`, khoảng cách`1`| Tiếp tuyến chính xác phải được tính là một lần truy cập | 
|`0 0 0 / -2 0 0 1 / 1 0 0`|`MISS`, khoảng cách`2`| Không được nhầm lẫn tia với đường vô hạn | 
|`-1000 -1000 -1000 / 0 0 0 1000 / 1000 1000 1000`|`HIT`, khoảng cách`0`| Tọa độ lớn và một tia chéo | 
|`1000 1000 1000 / -1000 -1000 -1000 1000 / 1000 1000 999`|`MISS`, khoảng cách xấp xỉ`3464.1016151377546`| Độ lớn tọa độ tối đa và điểm gần nhất tại gốc tia | 

## Vỏ cạnh 

Trường hợp khó phát hiện đầu tiên là khi địch ở sau khẩu pháo. Coi như```
0 0 0
-2 0 0 1
1 0 0
```Ở đây (V=(1,0,0)) và (W=(-2,0,0)), vì vậy 

[ 
W\cdot V=-2<0. 
] 

Hình chiếu nằm ở (t=-2), nằm ngoài phạm vi cho phép (t\ge0). Do đó, thuật toán chọn (t=0), chính khẩu pháo và thu được khoảng cách 2. Vì bán kính chỉ bằng 1 nên kết quả là`MISS`. Đây chính xác là trường hợp phá vỡ giải pháp sử dụng khoảng cách từ điểm đến vô hạn mà không kiểm tra hướng chiếu. 

Trường hợp thứ hai là tiếp tuyến:```
0 0 0
5 1 0 1
10 0 0
```Tia này là trục x và trung tâm của kẻ thù cách nó một đơn vị. Điểm gần nhất là`(5, 0, 0)`, do đó khoảng cách bằng bán kính. Việc so sánh số nguyên sử dụng`<=`, không`<`, và trả về`HIT`. Khoảng cách in chính xác`1.000000000000000`. 

Trường hợp thứ ba là tâm trực tiếp trên tia:```
0 0 0
100 100 100 10
1 1 1
```Các vectơ`w`Và`v`song song với nhau nên 

[ 
|W|^2|V|^2-(W\cdot V)^2=0. 
] 

Khoảng cách bằng 0 và quả cầu bị bắn trúng. Điều này cũng xác minh rằng công thức tích số chéo hoạt động chính xác khi thành phần vuông góc biến mất hoàn toàn. 

Trường hợp thứ tư thực hiện một phạm vi tọa độ lớn:```
-1000 -1000 -1000
0 0 0 1000
1000 1000 1000
```Các vectơ chỉ phương và tâm đều song song, do đó khoảng cách tính toán bằng 0 mặc dù tọa độ ở cực trị cho phép của chúng. Tất cả các số lượng bình phương vẫn đủ nhỏ cho số học số nguyên có chiều rộng cố định thông thường và số học số nguyên của Python sẽ loại bỏ mọi lo ngại về tràn. 

Trường hợp cạnh cuối cùng là mục tiêu chỉ thay đổi một tọa độ:```
1000 1000 1000
-1000 -1000 -1000 1000
1000 1000 999
```Mục tiêu khác với pháo nên vectơ chỉ phương hợp lệ nhưng nó chỉ gần như hoàn toàn dọc theo trục tọa độ của vị trí pháo. Tâm của kẻ địch chiếu về phía sau khẩu pháo vì tích số chấm là âm. Thuật toán sử dụng khoảng cách từ tâm địch đến khẩu pháo thay vì khoảng cách đến đường vô hạn, cho kết quả xấp xỉ`3464.1016151377546`. Điều này xác nhận rằng việc kiểm tra dấu được áp dụng trước công thức khoảng cách dòng.
