---
title: "CF 102386K - \u041c\u0430\u043b\u044b\u0448 \u0438 \u041a\u0430\u0440\u043b\u0441\u043e\u043d"
description: "Chúng ta có một đa giác lồi hoàn toàn có các đỉnh được cho ngược chiều kim đồng hồ và có tọa độ nguyên. Chúng ta cần vẽ một đường thẳng chia đa giác thành hai vùng có diện tích bằng nhau."
date: "2026-08-15T18:57:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102386
codeforces_index: "K"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0442\u0443\u0440 \u0423\u0440\u0430\u043b\u044c\u0441\u043a\u043e\u0433\u043e \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b\u0430 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u043c\u0438\u0440\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2019"
rating: 0
weight: 102386
solve_time_s: 549
verified: false
draft: false
---

[CF 102386K - \u041c\u0430\u043b\u044b\u0448 \u0438 \u041a\u0430\u0440\u043b\u0441\u043e\u043d](https://codeforces.com/problemset/problem/102386/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 9 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đa giác lồi hoàn toàn có các đỉnh được cho ngược chiều kim đồng hồ và có tọa độ nguyên. Chúng ta cần vẽ một đường thẳng chia đa giác thành hai vùng có diện tích bằng nhau. Bản thân dòng phải chứa hai điểm tọa độ nguyên riêng biệt và tọa độ của chúng phải nằm trong phạm vi từ (-10^{18}) đến (10^{18}). 

Thuộc tính hình học hữu ích là đa giác có thể được tạo tam giác từ bất kỳ đỉnh nào của nó. Chọn đỉnh đầu tiên (V_0) và kết nối nó với mọi đỉnh khác. Điều này tạo ra (N-2) tam giác có diện tích nhân đôi là số nguyên vì mọi tọa độ đều là số nguyên. Toàn bộ vấn đề sau đó trở thành việc tìm ra nơi mà một nửa tổng diện tích nhân đôi rơi vào chuỗi hình tam giác này. Vấn đề ban đầu sử dụng (N\le 1000), tọa độ được giới hạn bởi (10^5) và giới hạn một giây. Nó đủ nhỏ cho một thuật toán tuyến tính hoặc bậc hai, nhưng không có lý do gì để thực hiện bất cứ điều gì bậc hai một khi tam giác quạt được nhận ra. Số học số nguyên chính xác cũng thích hợp hơn dấu phẩy động vì đẳng thức cần thiết là chính xác. 

Tìm kiếm toàn diện trực tiếp trên tất cả các cặp tọa độ nguyên (A, B) là hữu hạn vì tọa độ bị giới hạn nhưng hoàn toàn vô dụng. Có khoảng ((4\cdot10^{36})^2/2\approx8\cdot10^{72}) cặp điểm mạng không có thứ tự và việc kiểm tra một dòng so với tất cả các cạnh đa giác sẽ thêm một hệ số khác của (N). Với (N=1000), đó là thứ tự của (10^{75}) phép toán hình học cơ bản. Các cách tiếp cận mạnh mẽ hơn có vẻ hợp lý hơn, chẳng hạn như thử các đường đi qua các cặp đỉnh đa giác, cũng không đủ vì một đường cắt hợp lệ không cần phải đi qua hai đỉnh đa giác. 

Có một số trường hợp ranh giới mà việc triển khai bất cẩn có thể xử lý sai. Đối với hình tam giác```
3
0 0
4 0
0 2
```câu trả lời là trung tuyến từ ((0,0)) đến trung điểm của cạnh đối diện. Bản thân điểm giữa không cần phải có tọa độ nguyên, vì vậy việc tìm kiếm một điểm nguyên trên cạnh có thể không thành công. Công thức của chúng ta nhân điểm hữu tỉ với mẫu số của nó và thu được một điểm nguyên trên cùng một dòng. 

Đối với hình vuông```
4
0 0
2 0
2 2
0 2
```tam giác quạt đầu tiên đã có đúng một nửa diện tích gấp đôi của đa giác. Việc triển khai bất cẩn chỉ xử lý trường hợp một nửa nằm hoàn toàn bên trong một hình tam giác có thể chuyển sang hình tam giác tiếp theo và truy cập các chỉ mục không hợp lệ. Trường hợp đẳng thức phải được xử lý ngay và đường chéo từ ((0,0)) đến ((2,2)) là đáp án hợp lệ. 

Mẫu được cung cấp là một trường hợp hữu ích khác:```
4
0 3
3 0
3 6
0 7
```Điểm nửa diện tích nằm hoàn toàn bên trong tam giác hình quạt thứ nhất. Việc triển khai dấu phẩy động có thể gần đúng với điểm cắt, nhưng trình kiểm tra yêu cầu sự bằng nhau chính xác. Thay vào đó, chúng ta xây dựng một điểm nguyên trên cùng một dòng bằng cách sử dụng phép nhân số nguyên. 

Dưới những ràng buộc đã nêu, một giải pháp luôn tồn tại, do đó`-1`đầu ra không bao giờ cần thiết. Cấu trúc dưới đây tạo ra một cách rõ ràng cho mỗi đầu vào hợp lệ. 

## Phương pháp tiếp cận 

Giải pháp brute-force theo nghĩa đen nhất sẽ tìm kiếm trên các điểm nguyên và kiểm tra các dòng ứng cử viên. Điều đó là không thể nếu chỉ xét riêng tọa độ giới hạn, vì mạng chứa khoảng (4\cdot10^{36}) điểm. Ngay cả việc hạn chế tìm kiếm theo các dòng thông qua các cặp đỉnh đa giác cũng để lại các ứng cử viên (O(N^2)) và việc kiểm tra từng ứng cử viên theo đa giác cũng mất (O(N)) thời gian. Trường hợp xấu nhất là về (N^3/2), khoảng (5\cdot10^8) phép toán cạnh cho (N=1000). Quan trọng hơn, việc tìm kiếm hạn chế đó chưa hoàn tất, vì đường được yêu cầu có thể đáp ứng ranh giới đa giác tại hai điểm nằm hoàn toàn bên trong các cạnh. 

Quan sát quan trọng là chúng ta không cần phải tìm kiếm chỉ đường. Sửa đỉnh đa giác đầu tiên (V_0) và tam giác hóa đa giác như 

[ 
(V_0,V_1,V_2),\quad 
(V_0,V_2,V_3),\quad 
\dots,\quad 
(V_0,V_{N-2},V_{N-1}). 
] 

Diện tích nhân đôi của mỗi tam giác là một số nguyên. Khi chúng ta đi qua các hình tam giác này, diện tích tích lũy bắt đầu từ 0 và kết thúc ở diện tích gấp đôi của toàn bộ đa giác. Do đó, có một tam giác đầu tiên mà việc bao gồm nó làm cho diện tích tích lũy đạt hoặc vượt quá một nửa tổng diện tích. 

Nếu diện tích tích lũy chính xác bằng một nửa sau một tam giác hoàn chỉnh nào đó thì đường chéo từ (V_0) tới đỉnh cuối cùng của tam giác đó đã là đường cắt mong muốn. 

Ngược lại, một nửa tổng diện tích nằm hoàn toàn bên trong một tam giác (V_0BC). Bên trong tam giác đó, mọi đường thẳng đi qua (V_0) và một điểm (P) trên (BC) cắt một tam giác (V_0BP). Diện tích của nó thay đổi tuyến tính khi (P) di chuyển dọc theo (BC), vì vậy chúng ta có thể chọn tỷ lệ chính xác cần thiết trên (BC). 

Khó khăn còn lại là (P) có thể là hữu tỉ chứ không phải là tích phân. Đây là lúc giới hạn đầu ra lớn bất thường (10^{18}) trở nên hữu ích. Nếu 

[ 
P=\frac{(2T-d)B+dC}{2T}, 
] 

chúng ta có thể đơn giản sử dụng 

[ 
Q=(2T-d)B+dC. 
] 

Điểm (Q) có tọa độ nguyên và nằm trên cùng một tia từ (V_0) với (P). Do đó đường (V_0Q) chính xác là đường cắt yêu cầu. Không có phép chia và không có số học dấu phẩy động ở bất cứ đâu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^3)) sau khi giới hạn ứng viên ở các cặp đỉnh | (O(N)) | Quá chậm và không đầy đủ | 
| Tối ưu | (O(N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chọn đỉnh đa giác đầu tiên (V_0) làm đỉnh chung của tam giác quạt. Dịch mọi đỉnh khác bằng cách trừ (V_0), do đó (V_0) trở thành gốc tọa độ. Bản dịch không thay đổi vùng hoặc dòng qua (V_0). 
2. Với mọi cặp liên tiếp (V_i,V_{i+1}), với (1\le i<N-1), hãy tính 

[ 
T_i=\left|\operatorname{cross}(V_i,V_{i+1})\right|. 
] 

Bằng hai lần diện tích tam giác (V_0V_iV_{i+1}). Bởi vì tất cả tọa độ đều là số nguyên nên mọi (T_i) đều là số nguyên. 

1. Tổng tất cả (T_i) để được (S), diện tích gấp đôi của toàn bộ đa giác. Chúng tôi cố tình làm việc với diện tích nhân đôi để mục tiêu chính xác là (S/2) mà không đưa vào phân số. 
2. Đi qua các hình tam giác trong khi duy trì diện tích gấp đôi tiền tố của chúng. Tìm tam giác đầu tiên (V_0BC) mà tiền tố mới thỏa mãn (2\cdot\text{prefix}\ge S). Tính lồi đảm bảo rằng tất cả các tam giác quạt đều nằm bên trong đa giác và có diện tích dương, do đó tam giác như vậy luôn tồn tại. 
3. Nếu (2\cdot\text{prefix}=S), xuất ra (V_0) và đỉnh cuối cùng của tam giác hiện tại. Các tam giác quạt trước đường chéo này có đúng một nửa tổng diện tích. 
4. Nếu không, hãy để`before`là diện tích gấp đôi của tất cả các tam giác quạt trước (V_0BC) và gọi (T) là diện tích gấp đôi của (V_0BC). Xác định 

[ 
d=S-2\cdot\text{trước}. 
] 

Phần mong muốn của tam giác hiện tại phải có diện tích gấp đôi (d/2). Vì tam giác hiện tại là tam giác đầu tiên vượt qua ranh giới nửa diện tích, 

[ 
0<d<2T. 
] 

1. Cho (B=V_i) và (C=V_{i+1}). Một điểm trên (BC) có thể được viết là 

[ 
P=\frac{(2T-d)B+dC}{2T}. 
] 

Tam giác (V_0BP) có diện tích gấp đôi 

# \frac{d}{2T}\operatorname{cross}(B,C) 

\frac d2. 
] 

Do đó diện tích trước tam giác này cộng với diện tích của (V_0BP) chính xác là (S/2). 

1. Chúng tôi không xuất ra (P), vì nó có thể là phân số. Thay vào đó hãy tính 

[ 
Q=(2T-d)B+dC. 
] 

Mọi tọa độ của (Q) đều là số nguyên. Vì (Q=2T\cdot P), các điểm (V_0,P,Q) thẳng hàng nên đường thẳng đi qua (V_0) và (Q) là đường cắt cần thiết. 

Sau các bước được đánh số, bất biến là các tam giác hình quạt tích lũy biểu thị chính xác diện tích trên một cạnh của đường chéo ứng cử viên. Trước tam giác đã chọn, diện tích này hoàn toàn dưới một nửa và sau khi thêm tam giác đã chọn, diện tích này ít nhất là một nửa. Nếu sự bằng nhau xảy ra ở ranh giới của tam giác thì đường chéo tương ứng sẽ giải quyết được vấn đề. Mặt khác, số lượng được yêu cầu nằm hoàn toàn trong khoảng từ 0 đến toàn bộ diện tích của tam giác đã chọn, do đó điểm duy nhất (P) được mô tả ở trên nằm hoàn toàn bên trong cạnh của tam giác đó. Điểm nguyên tỷ lệ (Q) nằm trên cùng một đường thẳng, chứng tỏ rằng đường đầu ra có các điểm nguyên và chia đôi chính xác đa giác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(ax, ay, bx, by):
    return ax * by - ay * bx

def solve():
    n = int(input())
    p = [tuple(map(int, input().split())) for _ in range(n)]

    ox, oy = p[0]

    # Translate the polygon so p[0] becomes (0, 0).
    q = [(x - ox, y - oy) for x, y in p]
    q[0] = (0, 0)

    # Doubled areas of the fan triangles.
    areas = []
    total = 0

    for i in range(1, n - 1):
        ax, ay = q[i]
        bx, by = q[i + 1]
        t = abs(cross(ax, ay, bx, by))
        areas.append(t)
        total += t

    prefix = 0

    for i, t in enumerate(areas, start=1):
        prefix += t

        # The current fan triangle ends at q[i + 1].
        if prefix * 2 == total:
            x, y = q[i + 1]
            print(ox, oy)
            print(ox + x, oy + y)
            return

        if prefix * 2 > total:
            before = prefix - t
            d = total - 2 * before

            # Current triangle is q[0], q[i], q[i + 1].
            bx, by = q[i]
            cx, cy = q[i + 1]

            # Q = (2T-d) * B + d * C.
            #
            # Q is a scaled version of the exact rational
            # point on BC, so OQ is the same cutting line.
            qx = (2 * t - d) * bx + d * cx
            qy = (2 * t - d) * by + d * cy

            print(ox, oy)
            print(ox + qx, oy + qy)
            return

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai sẽ dịch đa giác theo đỉnh đầu tiên. Điều này làm cho công thức tính diện tích trở nên đặc biệt đơn giản, vì mỗi tam giác quạt có gốc tọa độ là một đỉnh. 

các`cross`hàm là nguyên hàm hình học duy nhất được yêu cầu. Đối với hai vectơ (u) và (v), giá trị tuyệt đối của nó là diện tích gấp đôi của tam giác tạo bởi gốc tọa độ và các vectơ đó. Số nguyên Python có độ chính xác tùy ý, do đó, các sản phẩm trung gian vẫn an toàn mặc dù đầu ra được xây dựng có thể lớn hơn nhiều so với tọa độ đầu vào. 

Vòng lặp đầu tiên tính diện tích tam giác quạt và tổng diện tích của chúng. Vòng lặp thứ hai tìm kiếm tiền tố đầu tiên vượt qua một nửa tổng số đó. Nhánh đẳng thức là riêng biệt vì trong trường hợp đó đường mong muốn đã là đường chéo đa giác. 

Trong nhánh vượt qua nghiêm ngặt,`before`là diện tích nhân đôi đã được tính. số lượng`d = total - 2 * before`gấp đôi diện tích còn lại cần thiết của tam giác hiện tại. các hệ số`2 * t - d`Và`d`đều dương, do đó điểm được xây dựng nằm trên đoạn giữa hai đỉnh đa giác hiện tại trước khi chia tỷ lệ. 

Mã không bao giờ chia cho`2 * t`. Đó là thủ thuật thực hiện trung tâm. Điểm hữu tỷ được nhân với mẫu số của nó, tạo ra điểm nguyên`Q`trên cùng một dòng. Với tọa độ đầu vào được giới hạn bởi (10^5), mọi tọa độ dịch có độ lớn tối đa (2\cdot10^5), trong khi (2T\le8\cdot10^{10}) đối với một tam giác riêng lẻ. Do đó, tọa độ được xây dựng nằm ở mức dưới (10^{18}). 

## Ví dụ đã hoạt động 

Đối với mẫu được cung cấp,```
4
0 3
3 0
3 6
0 7
```bản dịch theo đỉnh đầu tiên sẽ cho ((0,0),(3,-3),(3,3),(0,4)). Chiếc quạt bao gồm hai hình tam giác. 

| Tam giác | Các đỉnh liên quan đến (V_0) | Diện tích nhân đôi | Tiền tố | 
| --- | --- | --- | --- | 
| 1 | ((0,0),(3,-3),(3,3)) | 18 | 18 | 
| 2 | ((0,0),(3,3),(0,4)) | 12 | 30 | 

Tổng diện tích nhân đôi là (30), nên mục tiêu là (15). Tam giác đầu tiên đã vượt qua mục tiêu. Đây`before = 0`, (T=18) và (d=30). Điểm chia tỷ lệ là 

[ 
Q=(36-30)(3,-3)+30(3,3)=(108,72). 
] 

Sau khi dịch ngược lại, chương trình xuất ra```
0 3
108 75
```Đường cắt cắt đa giác tại ((0,3)) và ((3,5)) và tam giác thu được có diện tích (15/2), chính xác bằng một nửa diện tích của đa giác. Đầu ra của mẫu là khác nhau, nhưng được phép có nhiều câu trả lời hợp lệ. 

Đối với ví dụ thứ hai, hãy xem xét hình vuông```
4
0 0
2 0
2 2
0 2
```| Tam giác | Các đỉnh liên quan đến (V_0) | Diện tích nhân đôi | Tiền tố | 
| --- | --- | --- | --- | 
| 1 | ((0,0),(2,0),(2,2)) | 4 | 4 | 
| 2 | ((0,0),(2,2),(0,2)) | 4 | 8 | 

Tổng diện tích nhân đôi là (8), nên một nửa là (4). Tiền tố đầu tiên đã chính xác là (4), tiền tố này sẽ kích hoạt nhánh đẳng thức. Đầu ra của thuật toán```
0 0
2 2
```Đường chéo chia hình vuông thành hai hình tam giác có diện tích (2) mỗi hình. 

Một ví dụ nhỏ thứ ba là hình tam giác```
3
0 0
4 0
0 2
```Chỉ có một tam giác quạt nên diện tích nhân đôi của nó là (8). Mục tiêu là (4), tương ứng với điểm giữa của cạnh đối diện. Việc xây dựng mang lại 

[ 
Q=8(4,0)+8(0,2)=(32,16), 
] 

nên đường ra là đường đi qua ((0,0)) và ((32,16)), tương đương (y=x/2). Nó đi qua trung điểm ((2,1)) của cạnh đối diện và chính xác là đường trung tuyến của tam giác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N)) | Mỗi đỉnh đa giác được xử lý một số lần không đổi. | 
| Không gian | (O(N)) | Các vùng đa giác và tam giác hình quạt được lưu trữ rõ ràng. | 

Đối với (N\le1000), thời gian tuyến tính thấp hơn nhiều so với giới hạn sẵn có. Các giá trị số nguyên lớn nhất phát sinh từ các tích chéo và từ việc chia tỷ lệ điểm trên một cạnh đa giác, nhưng các số nguyên có độ chính xác tùy ý của Python xử lý chúng một cách chính xác. Tọa độ được xây dựng cuối cùng nằm ở mức dưới (10^{18}), do đó giới hạn đầu ra cũng được thỏa mãn. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây sử dụng hình học số nguyên chính xác để xác thực dòng trả về. Vì các bài toán hình học thường cho phép có nhiều kết quả đầu ra khác nhau nên các bài kiểm tra sẽ kiểm tra tính chất toán học của kết quả đầu ra thay vì yêu cầu một cặp điểm cụ thể.```python
# helper: run solution on input string, return output string
import sys
import io
from fractions import Fraction
import math

def cross(ax, ay, bx, by):
    return ax * by - ay * bx

def solve_data(data):
    it = iter(data.strip().split())
    n = int(next(it))
    p = [(int(next(it)), int(next(it))) for _ in range(n)]

    ox, oy = p[0]
    q = [(x - ox, y - oy) for x, y in p]

    areas = []
    total = 0

    for i in range(1, n - 1):
        ax, ay = q[i]
        bx, by = q[i + 1]
        t = abs(cross(ax, ay, bx, by))
        areas.append(t)
        total += t

    prefix = 0

    for i, t in enumerate(areas, start=1):
        prefix += t

        if prefix * 2 == total:
            x, y = q[i + 1]
            return f"{ox} {oy}\n{ox + x} {oy + y}\n"

        if prefix * 2 > total:
            before = prefix - t
            d = total - 2 * before

            bx, by = q[i]
            cx, cy = q[i + 1]

            qx = (2 * t - d) * bx + d * cx
            qy = (2 * t - d) * by + d * cy

            return f"{ox} {oy}\n{ox + qx} {oy + qy}\n"

    return "-1\n"

def run(inp: str) -> str:
    return solve_data(inp)

def polygon_double_area(p):
    s = 0
    n = len(p)
    for i in range(n):
        x1, y1 = p[i]
        x2, y2 = p[(i + 1) % n]
        s += x1 * y2 - y1 * x2
    return abs(s)

def line_value(a, b, p):
    ax, ay = a
    bx, by = b
    x, y = p
    return (bx - ax) * (y - ay) - (by - ay) * (x - ax)

def clip_halfplane(poly, a, b, keep_positive):
    if not poly:
        return []

    result = []

    def inside(v):
        return v >= 0 if keep_positive else v <= 0

    for i in range(len(poly)):
        p = poly[i]
        q = poly[(i + 1) % len(poly)]
        fp = line_value(a, b, p)
        fq = line_value(a, b, q)
        inp = inside(fp)
        inq = inside(fq)

        if inp:
            result.append(p)

        if inp != inq:
            den = fq - fp
            t = Fraction(-fp, den)
            x = p[0] + t * (q[0] - p[0])
            y = p[1] + t * (q[1] - p[1])
            result.append((x, y))

    return result

def double_area_fraction(poly):
    if len(poly) < 3:
        return Fraction(0)

    s = Fraction(0)
    for i in range(len(poly)):
        x1, y1 = poly[i]
        x2, y2 = poly[(i + 1) % len(poly)]
        s += x1 * y2 - y1 * x2
    return abs(s)

def valid_cut(inp, out):
    tokens = out.strip().split()
    if len(tokens) == 1 and tokens[0] == "-1":
        return False

    if len(tokens) != 4:
        return False

    a = (int(tokens[0]), int(tokens[1]))
    b = (int(tokens[2]), int(tokens[3]))

    if a == b:
        return False

    it = iter(inp.strip().split())
    n = int(next(it))
    poly = [(int(next(it)), int(next(it))) for _ in range(n)]

    total = Fraction(polygon_double_area(poly))

    left = clip_halfplane(poly, a, b, True)
    right = clip_halfplane(poly, a, b, False)

    return (
        double_area_fraction(left) * 2 == total
        and double_area_fraction(right) * 2 == total
        and abs(a[0]) <= 10**18
        and abs(a[1]) <= 10**18
        and abs(b[0]) <= 10**18
        and abs(b[1]) <= 10**18
    )

# Provided sample.
sample1 = """\
4
0 3
3 0
3 6
0 7
"""
assert valid_cut(sample1, run(sample1)), "sample 1"

# Minimum-size polygon.
triangle = """\
3
0 0
4 0
0 2
"""
assert run(triangle) == "0 0\n32 16\n", "minimum-size triangle"

# Equal fan areas, exercising the exact-half branch.
square = """\
4
0 0
2 0
2 2
0 2
"""
assert run(square) == "0 0\n2 2\n", "exact prefix half"

# Coordinates at the input boundary.
boundary_triangle = """\
3
100000 100000
-100000 100000
-100000 -100000
"""
assert valid_cut(boundary_triangle, run(boundary_triangle)), "coordinate boundary"

# A nontrivial polygon where half the area lies strictly inside a fan triangle.
pentagon = """\
5
0 0
4 0
5 2
3 5
0 4
"""
assert run(pentagon) == "0 0\n144 145\n", "interior fan triangle"

# Maximum-size stress test.
# Points are sampled from a large circle and slightly perturbed radially.
# The radius is large enough that rounding preserves strict convexity.
n = 1000
pts = []
for i in range(n):
    angle = 2.0 * math.pi * i / n
    r = 90000 + (i % 7)
    x = int(round(r * math.cos(angle)))
    y = int(round(r * math.sin(angle)))
    pts.append((x, y))

# Rotate the generated order if necessary so it is counterclockwise.
area = polygon_double_area(pts)
if area < 0:
    pts.reverse()

max_case = str(n) + "\n" + "\n".join(f"{x} {y}" for x, y in pts) + "\n"
assert valid_cut(max_case, run(max_case)), "maximum-size stress test"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | Bất kỳ dòng số nguyên chia đôi diện tích chính xác nào | Đường cắt độc đáo bên trong tam giác quạt đầu tiên | 
|`3 / 0 0 / 4 0 / 0 2`|`0 0`Và`32 16`| Đa giác tối thiểu và chia tỷ lệ điểm giữa hợp lý | 
|`4 / 0 0 / 2 0 / 2 2 / 0 2`|`0 0`Và`2 2`| Tiền tố chính xác bằng một nửa | 
| Tam giác biên có tọa độ (\pm100000) | Bất kỳ dòng số nguyên hợp lệ nào | Tọa độ đầu vào lớn và số nguyên được xây dựng lớn | 
| Đa giác năm đỉnh |`0 0`Và`144 145`| Thi công tam giác quạt nội thất nghiêm ngặt | 
| Đã tạo đa giác 1000 đỉnh | Bất kỳ dòng số nguyên hợp lệ nào | Tối đa (N), số học số nguyên và truyền tải thời gian tuyến tính | 

## Vỏ cạnh 

Đối với tam giác tối thiểu```
3
0 0
4 0
0 2
```thuật toán có đúng một tam giác quạt có diện tích gấp đôi (8). Tiền tố ngay lập tức bằng tổng, nhưng điều kiện nửa diện tích đạt được bên trong tam giác đó chứ không phải sau toàn bộ tam giác. Đây`before = 0`, (T=8) và (d=8). Điểm nguyên được xây dựng là (Q=8(4,0)+8(0,2)=(32,16)). Đường thẳng từ ((0,0)) đến ((32,16)) là đường trung tuyến nên cả hai phần đều có diện tích (4). 

Đối với trường hợp tiền tố chính xác```
4
0 0
2 0
2 2
0 2
```tam giác quạt thứ nhất có diện tích gấp đôi (4), trong khi tổng diện tích gấp đôi là (8). Bài kiểm tra bình đẳng diễn ra trước nhánh vượt qua nghiêm ngặt. Đường chéo đầu ra ((0,0)) đến ((2,2)) chia hình vuông thành hai hình tam giác bằng nhau. Trường hợp này phát hiện ra cả lỗi ngẫu nhiên trong việc chọn tam giác hiện tại và cách triển khai giả định điểm nửa diện tích luôn nằm trong một cạnh. 

Đối với mẫu được cung cấp```
4
0 3
3 0
3 6
0 7
```tam giác đầu tiên có diện tích gấp đôi (18), trong khi toàn bộ đa giác có diện tích gấp đôi (30). Vì (18>15) nên mục tiêu nằm trong tam giác đó. Điểm chia tỷ lệ chính xác là ((108,72)) trong tọa độ so với đỉnh đầu tiên, cho điểm đầu ra ((108,75)). Đường thẳng đi qua ((0,3)) và ((108,75)) lại cắt đa giác tại ((3,5)) và tam giác thu được có diện tích (7,5), chính xác bằng một nửa diện tích của đa giác (15). 

Đối với trường hợp tọa độ biên```
3
100000 100000
-100000 100000
-100000 -100000
```đầu vào sử dụng cường độ tọa độ lớn nhất được phép. Sau khi dịch theo đỉnh đầu tiên, các điểm còn lại là ((-200000,0)) và ((-200000,-200000)). Việc xây dựng chia tỷ lệ điểm giữa cạnh đối diện bằng diện tích nhân đôi của tam giác và tạo ra một điểm có độ lớn khoảng (10^{13}), vẫn thấp hơn nhiều (10^{18}). Không cần thực hiện phép toán dấu phẩy động nên không làm mất độ chính xác ở biên. 

Trường hợp cạnh tinh vi nhất là điểm cắt hợp lý mà bản thân nó không phải là điểm nguyên. Thuật toán không bao giờ cố gắng làm tròn điểm đó. Thay vào đó, nó biểu diễn điểm dưới dạng tổ hợp affine hữu tỉ của hai đỉnh nguyên và nhân toàn bộ tổ hợp đó với mẫu số của nó. Chia tỷ lệ một vectơ từ đỉnh nguyên cố định (V_0) sẽ thay đổi độ dài của nó nhưng không thay đổi hướng của nó, do đó điểm nguyên thu được sẽ xác định chính xác cùng một đường cắt. Đây là lý do việc xây dựng hoạt động cho mọi đa giác lồi tọa độ nguyên hợp lệ thay vì chỉ cho các đa giác có nửa diện tích bị cắt đi qua một điểm mạng hiện có.
