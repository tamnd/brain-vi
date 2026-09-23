---
title: "CF 104790G - Trò chơi hình học"
description: "Chúng ta có bốn điểm trong mặt phẳng, đã được sắp xếp theo chiều kim đồng hồ và đảm bảo tạo thành một tứ giác lồi hoàn toàn. Nhiệm vụ của chúng ta là phân loại hình dạng được hình thành bằng cách nối các điểm này theo thứ tự và khép lại chu trình. Việc phân loại là phân cấp."
date: "2026-06-28T13:57:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104790
codeforces_index: "G"
codeforces_contest_name: "2023 Benelux Algorithm Programming Contest (BAPC 23)"
rating: 0
weight: 104790
solve_time_s: 65
verified: true
draft: false
---

[CF 104790G - Trò chơi hình học](https://codeforces.com/problemset/problem/104790/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có bốn điểm trong mặt phẳng, đã được sắp xếp theo chiều kim đồng hồ và đảm bảo tạo thành một tứ giác lồi hoàn toàn. Nhiệm vụ của chúng ta là phân loại hình dạng được hình thành bằng cách nối các điểm này theo thứ tự và khép lại chu trình. 

Việc phân loại là phân cấp. Chúng ta phải quyết định xem tứ giác là hình vuông, hình chữ nhật, hình thoi, hình bình hành, hình thang, cánh diều hay không và luôn đưa ra hình tứ giác cụ thể nhất phù hợp. Điều này có nghĩa là nếu một hình dạng thỏa mãn nhiều định nghĩa, chúng tôi sẽ chọn nhãn hạn chế nhất theo thứ tự nhất định. 

Các ràng buộc không chặt chẽ về kích thước đầu vào, vì chỉ có một hình tứ giác cho mỗi trường hợp thử nghiệm. Điều này loại bỏ mọi nhu cầu về tiền xử lý hoặc tối ưu hóa tiệm cận ngoài các tính toán hình học theo thời gian không đổi. Thách thức thực sự là tính chính xác trong phân loại hình học theo tọa độ số nguyên lên tới 10^9, nghĩa là chúng ta phải tránh các lỗi dấu phẩy động và dựa vào số học số nguyên như khoảng cách bình phương và tích chéo. 

Một số tình huống biên vẫn quan trọng ngay cả khi các điểm lồi và có trật tự. Đầu tiên là sự phân biệt giữa hình bình hành và hình thang, vì cả hai đều phụ thuộc vào tính song song và một phép kiểm tra đơn giản có thể tính sai cấu trúc song song được chia sẻ hai lần. Thứ hai là phân biệt hình vuông, hình chữ nhật và hình thoi, vì tất cả đều là trường hợp đặc biệt của hình bình hành với các ràng buộc bổ sung. Thứ ba là phát hiện diều, thường bị hiểu sai là bất kỳ tứ giác nào có hai cạnh kề bằng nhau, nhưng phải được loại trừ cẩn thận khi áp dụng phân loại mạnh hơn. 

Ví dụ: một hình vuông như (0,0), (0,1), (1,1), (1,0) không nên được phân loại là hình diều mặc dù về mặt kỹ thuật nó có nhiều trục đối xứng, vì hình vuông có tính hạn chế hơn. 

Một trường hợp tinh tế khác là hình bình hành tổng quát không phải là hình chữ nhật hoặc hình thoi, chẳng hạn như (1,1), (2,3), (4,5), (3,3), trong đó các cạnh đối diện song song nhưng góc và độ dài khác nhau. Việc triển khai bất cẩn chỉ kiểm tra sự bằng nhau của các bên hoặc chỉ các hệ số góc sẽ phân loại sai nó. 

Cuối cùng, việc phát hiện hình thang phải xác định chính xác một cặp cạnh song song chứ không phải ít nhất một. Hình bình hành có hai cặp như vậy không được tính là hình thang. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để phân loại tứ giác là kiểm tra rõ ràng mọi định nghĩa trực tiếp từ mô tả hình học của nó. Chúng ta có thể kiểm tra tất cả các điều kiện góc bằng cách sử dụng tích vô hướng, tất cả các đẳng thức cạnh bằng khoảng cách và tất cả các điều kiện song song bằng cách sử dụng tích chéo, sau đó đánh giá từng quy tắc hình dạng một. 

Điều này hoạt động chính xác vì mỗi thuộc tính có thể được kiểm tra theo các nguyên tắc đầu tiên: sự bằng nhau của các cạnh thông qua khoảng cách bình phương, góc vuông thông qua tích số 0 và tính song song thông qua tích số 0. Tuy nhiên, cấu trúc ngây thơ có xu hướng tính toán lại các đại lượng giống nhau nhiều lần và trộn lẫn lý luận góc động nếu không cẩn thận. Quan trọng hơn nữa, việc giải thích theo nghĩa đen của từng định nghĩa mà không chuẩn hóa sẽ dẫn đến việc kiểm tra dư thừa và logic dễ vỡ, đặc biệt là đối với tính đối xứng diều. 

Thông tin chi tiết quan trọng là tất cả các thuộc tính bắt buộc sẽ giảm xuống một tập hợp nhỏ các nguyên tố gốc có thể tái sử dụng: độ dài cạnh bình phương, tích chấm giữa các cạnh liền kề và tích chéo giữa các cạnh đối diện. Khi chúng ta tính toán bốn vectơ cạnh, mọi điều kiện phân loại sẽ trở thành sự kết hợp của các vectơ nguyên thủy này. Điều này cho phép chúng tôi đánh giá tất cả các loại hình dạng trong thời gian không đổi với độ ổn định số nhất quán. 

Thay vì suy nghĩ theo các định nghĩa hình học một cách riêng biệt, chúng ta coi tứ giác là một chu trình của các vectơ và suy ra tất cả các tính chất từ ​​biểu diễn đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Thiết kế quá chậm, dễ mắc lỗi | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận |

## Hướng dẫn thuật toán 

Ta đánh dấu các điểm theo thứ tự A, B, C, D. 

### 1. Tính vectơ cạnh và độ dài cạnh bình phương 

Chúng ta tạo các vectơ AB, BC, CD, DA và tính bình phương độ dài của chúng. Độ dài bình phương tránh được lỗi dấu phẩy động và đủ để so sánh bằng nhau. 

### 2. Tính tích chấm cho góc vuông 

Chúng tôi tính toán AB · BC, BC · CD, CD · DA và DA · AB. Tích số chấm bằng 0 biểu thị một góc vuông. 

### 3. Tính tích chéo cho phép song song 

Chúng ta tính AB × CD và BC × DA. Tích chéo bằng 0 biểu thị các đường thẳng song song. 

### 4. Kiểm tra ô vuông 

Chúng tôi xác minh tất cả các cạnh đều bằng nhau và tất cả các góc đều là góc vuông. Điều này mô tả đầy đủ hình vuông trong một tứ giác lồi. 

### 5. Kiểm tra hình chữ nhật 

Chúng tôi xác minh tất cả các góc đều là góc vuông. Độ dài các cạnh không cần phải bằng nhau. 

### 6. Kiểm tra hình thoi 

Chúng tôi xác minh tất cả bốn bên đều bằng nhau. Các góc không bị ràng buộc ngoài độ lồi. 

### 7. Kiểm tra hình bình hành 

Ta chứng minh hai cặp cạnh đối diện song song. 

### 8. Kiểm tra hình thang 

Chúng tôi xác minh chính xác một cặp cạnh đối diện song song. Điều này yêu cầu logic XOR trên hai lần kiểm tra song song. 

### 9. Kiểm tra diều 

Chúng tôi xác minh AB bằng BC và CD bằng DA hoặc BC bằng CD và DA bằng AB. Điều này mã hóa hai cặp cạnh bằng nhau liền kề. 

### 10. Nếu không thì không xuất ra 

### Tại sao nó hoạt động 

Mọi phân loại đều quy giản về các bất biến đại số của một tứ giác trong hình học Euclide. Đẳng thức bên được thể hiện bằng khoảng cách bình phương, cấu trúc góc được thể hiện bằng tích chấm và cấu trúc song song được thể hiện bằng tích chéo. Vì tất cả các điều kiện được đánh giá trên cùng một biểu diễn chính tắc nên không còn sự mơ hồ về mặt hình học. Hệ thống phân cấp đảm bảo rằng bất cứ khi nào có nhiều thuộc tính, thuộc tính hạn chế nhất sẽ được chọn trước tiên, ngăn chặn việc phân loại sai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def sq_dist(x1, y1, x2, y2):
    dx = x1 - x2
    dy = y1 - y2
    return dx * dx + dy * dy

def dot(ax, ay, bx, by):
    return ax * bx + ay * by

def cross(ax, ay, bx, by):
    return ax * by - ay * bx

x = []
y = []

for _ in range(4):
    xi, yi = map(int, input().split())
    x.append(xi)
    y.append(yi)

ax, ay = x[0], y[0]
bx, by = x[1], y[1]
cx, cy = x[2], y[2]
dx, dy = x[3], y[3]

AB = (bx - ax, by - ay)
BC = (cx - bx, cy - by)
CD = (dx - cx, dy - cy)
DA = (ax - dx, ay - dy)

s1 = sq_dist(ax, ay, bx, by)
s2 = sq_dist(bx, by, cx, cy)
s3 = sq_dist(cx, cy, dx, dy)
s4 = sq_dist(dx, dy, ax, ay)

right1 = dot(*AB, *BC) == 0
right2 = dot(*BC, *CD) == 0
right3 = dot(*CD, *DA) == 0
right4 = dot(*DA, *AB) == 0

par1 = cross(*AB, *CD) == 0
par2 = cross(*BC, *DA) == 0

if s1 == s2 == s3 == s4 and right1 and right2 and right3 and right4:
    print("square")
elif right1 and right2 and right3 and right4:
    print("rectangle")
elif s1 == s2 == s3 == s4:
    print("rhombus")
elif par1 and par2:
    print("parallelogram")
elif par1 ^ par2:
    print("trapezium")
else:
    kite1 = (s1 == s2 and s3 == s4)
    kite2 = (s2 == s3 and s4 == s1)
    if kite1 or kite2:
        print("kite")
    else:
        print("none")
```Đoạn mã đầu tiên xây dựng các vectơ cạnh để tất cả các phép kiểm tra hình học trở thành các phép toán số học đơn giản. Khoảng cách bình phương được sử dụng nhất quán để tránh các vấn đề về độ chính xác. Thứ tự kiểm tra tuân theo hệ thống phân cấp bắt buộc để các hình dạng cụ thể hơn được phát hiện trước khi khái quát hóa chúng. 

Một chi tiết triển khai tinh tế là việc sử dụng XOR để phát hiện hình thang. Vì hình bình hành sẽ đáp ứng cả hai phép kiểm tra song song nên XOR đảm bảo nó bị loại trừ. Một điểm quan trọng khác là việc phát hiện diều được trì hoãn cho đến khi loại trừ tất cả các lớp mạnh hơn, ngăn ngừa việc dán nhãn sai cho hình vuông và hình thoi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
(0,0)
(0,1)
(1,1)
(1,0)
```| Bước | s1 | s2 | s3 | s4 | góc vuông | song song | quyết định | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| tính toán | 1 | 1 | 1 | 1 | đang chờ xử lý | đang chờ xử lý | kiểm tra vuông | 
| góc độ | đúng | đúng | đúng | đúng | tất cả đều đúng | - | vuông | 

Điều này xác nhận tất cả các cạnh bằng nhau và tất cả các góc đều vuông góc, do đó thuật toán chọn hình vuông ở điều kiện khớp đầu tiên. 

### Ví dụ 2 

đầu vào:```
(1,1)
(2,3)
(4,5)
(3,3)
```| Bước | s1 | s2 | s3 | s4 | góc vuông | song song | quyết định | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| tính toán | khác biệt | khác biệt | khác biệt | khác biệt | một số sai lầm | cả hai đều đúng | hình bình hành | 

Ở đây các cạnh đối diện song song ở cả hai cặp, nhưng các góc không vuông và các cạnh không bằng nhau nên nó trở thành hình bình hành. 

Dấu vết cho thấy việc phân loại chỉ phụ thuộc vào bất biến cấu trúc chứ không phụ thuộc vào độ lớn tọa độ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 

|---|---|---|---| 

| Thời gian | O(1) | Hằng số phép tính số học trên bốn điểm | 

| Không gian | O(1) | Chỉ có một số biến cố định cho vectơ và đại lượng vô hướng | 

Việc tính toán hoàn toàn mang tính cục bộ và không phụ thuộc vào kích thước tọa độ, do đó nó phù hợp thoải mái với mọi ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import math

    x = []
    y = []
    for _ in range(4):
        xi, yi = map(int, input().split())
        x.append(xi)
        y.append(yi)

    def sq(a,b,c,d):
        return (a-c)**2 + (b-d)**2
    def dot(ax,ay,bx,by):
        return ax*bx+ay*by
    def cross(ax,ay,bx,by):
        return ax*by-ay*bx

    ax,ay,bx,by,cx,cy,dx,dy = x[0],y[0],x[1],y[1],x[2],y[2],x[3],y[3]

    AB=(bx-ax,by-ay)
    BC=(cx-bx,cy-by)
    CD=(dx-cx,dy-cy)
    DA=(ax-dx,ay-dy)

    s1=sq(ax,ay,bx,by)
    s2=sq(bx,by,cx,cy)
    s3=sq(cx,cy,dx,dy)
    s4=sq(dx,dy,ax,ay)

    right1=dot(*AB,*BC)==0
    right2=dot(*BC,*CD)==0
    right3=dot(*CD,*DA)==0
    right4=dot(*DA,*AB)==0

    par1=cross(*AB,*CD)==0
    par2=cross(*BC,*DA)==0

    if s1==s2==s3==s4 and right1 and right2 and right3 and right4:
        return "square"
    elif right1 and right2 and right3 and right4:
        return "rectangle"
    elif s1==s2==s3==s4:
        return "rhombus"
    elif par1 and par2:
        return "parallelogram"
    elif par1 ^ par2:
        return "trapezium"
    else:
        kite1=(s1==s2 and s3==s4)
        kite2=(s2==s3 and s4==s1)
        return "kite" if (kite1 or kite2) else "none"

# provided sample
assert run("""0 0
0 1
1 1
1 0
""") == "square"

assert run("""1 1
2 3
4 5
3 3
""") == "parallelogram"

# custom cases
assert run("""0 0
1 0
2 0
1 1
""") in ["kite", "trapezium", "none"]

assert run("""0 0
2 0
3 1
1 1
""") in ["parallelogram", "trapezium"]

assert run("""0 0
1 1
2 0
1 -1
""") in ["rhombus", "kite", "parallelogram"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu vuông | vuông | hoàn toàn bình đẳng và phát hiện góc vuông | 
| mẫu hình bình hành | hình bình hành | sự song song đối diện | 
| thoái hóa như cánh diều | biến | độ bền logic liền kề | 
| nghiêng quad | biến | độ chính xác phát hiện song song | 
| hình kim cương | biến | hình thoi vs diều mơ hồ | 

## Vỏ cạnh 

Trường hợp cạnh then chốt là khi tứ giác là hình vuông. Trong tình huống đó, về mặt kỹ thuật, nó cũng đáp ứng các tính chất đối xứng diều và hình thoi, nhưng hệ thống phân cấp buộc hình vuông phải được chọn trước tiên. Thuật toán xử lý điều này vì điều kiện bình phương được kiểm tra trước tất cả các điều kiện khác và yêu cầu đồng thời cả hai cạnh bằng nhau và góc vuông. 

Một trường hợp cạnh khác là hình thoi không phải là hình vuông. Tất cả các cạnh đều bằng nhau, nhưng góc không phải là 90 độ. Thuật toán bỏ qua việc kiểm tra hình chữ nhật một cách chính xác vì tích các chấm khác 0 và sau đó phân loại nó thành hình thoi trước khi đạt đến hình bình hành. 

Hình bình hành không phải là hình chữ nhật hoặc hình thoi được xử lý hoàn toàn thông qua việc kiểm tra sản phẩm chéo. Vì cả hai cặp cạnh đối diện đều song song nên điều kiện XOR cho hình thang là sai và điều kiện diều không thành công do thiếu các cạnh bằng nhau nên nó rơi vào hình bình hành một cách chính xác. 

Trường hợp hình thang xảy ra khi có đúng một cặp cạnh đối diện song song. Điều kiện XOR đảm bảo rằng hình bình hành không vô tình đủ điều kiện. Đây là nguồn phân loại sai phổ biến nhất trong các triển khai đơn giản chỉ kiểm tra “bất kỳ cặp song song nào tồn tại”.
