---
title: "CF 1578D - Đường Cong Rồng"
description: "Mọi hình vuông đơn vị có các góc nguyên đều bị cắt bởi đúng một đoạn của một trong bốn đường cong rồng vô hạn. Đối với hình vuông truy vấn có các góc đối diện $(x,y)$ và $(x+1,y+1)$, chúng ta phải xác định hai điều."
date: "2026-06-10T10:35:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 1578
codeforces_index: "D"
codeforces_contest_name: "ICPC WF Moscow Invitational Contest - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 3200
weight: 1578
solve_time_s: 130
verified: false
draft: false
---

[CF 1578D - Đường cong rồng](https://codeforces.com/problemset/problem/1578/D) 

**Đánh giá:** 3200 
**Thẻ:** - 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mọi hình vuông đơn vị có các góc nguyên đều bị cắt bởi đúng một đoạn của một trong bốn đường cong rồng vô hạn. Đối với hình vuông truy vấn có các góc đối diện$(x,y)$Và$(x+1,y+1)$, chúng ta phải xác định hai điều. 

Đầu tiên là đường nào trong bốn đường chứa đoạn cắt qua hình vuông đó. 

Thứ hai là chỉ số của đoạn đó dọc theo đường cong, trong đó đoạn đầu tiên của mỗi đường cong có chỉ số$1$. 

Tọa độ lớn như$10^9$, và có tới$2 \cdot 10^5$truy vấn. Bất kỳ cách tiếp cận nào tạo ra đường cong một cách rõ ràng đều vô vọng. Ngay cả việc tạo ra một đường cong rồng hữu hạn đủ lớn để đạt được tọa độ có độ lớn đó sẽ cần nhiều phân đoạn theo cấp số nhân. Chúng tôi cần một giải pháp logarit cho mỗi truy vấn. 

Phần khó khăn là đường cong rồng tự giống. Một hình vuông rất xa gốc tọa độ vẫn thuộc về một cấu trúc đệ quy trông giống như một đường cong rồng nhỏ hơn. Giải pháp dự định liên tục ánh xạ một ô truy vấn từ một tỷ lệ của fractal sang tỷ lệ nhỏ hơn tiếp theo cho đến khi đạt đến điểm gốc. Mỗi bước chia tọa độ khoảng hai, do đó chỉ cần khoảng 31 lần lặp để có tọa độ tối đa$10^9$. 

Một lỗi phổ biến là cho rằng sau khi thu nhỏ lại, thứ tự phân đoạn được giữ nguyên. Đôi khi đúng như vậy, nhưng đôi khi thứ tự bên trong đoạn lớn hơn tương ứng bị đảo ngược. Ví dụ: hai hình vuông liền kề thuộc cùng một phân đoạn có tỷ lệ lớn có thể xuất hiện theo thứ tự ngược lại sau một lần rút gọn đệ quy. Giải pháp phải theo dõi rõ ràng những sự đảo ngược này. 

Một trường hợp tinh tế khác là xử lý tọa độ âm. Phép biến đổi tọa độ đệ quy dựa trên các phép dịch số học và các công thức được lựa chọn cẩn thận. Việc thay thế chúng bằng phép chia đơn giản có thể phá vỡ ánh xạ cho các hình vuông trong góc phần tư âm. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra từng đoạn đường cong rồng cho đến khi đạt được ô vuông mục tiêu. Điều này đơn giản về mặt khái niệm vì mọi đoạn thẳng đều cắt chính xác một hình vuông. Thật không may, số lượng các đoạn tăng theo cấp số nhân theo thứ tự của đường cong. Tọa độ gần$10^9$tương ứng với độ sâu đệ quy khoảng 30, bao hàm hàng tỷ phân đoạn. Cách tiếp cận này hoàn toàn không khả thi. 

Quan sát chính là sự tự tương tự. 

Hãy xem xét hai phiên bản của đường cong rồng. Gọi một đơn hàng ban đầu$n+1$và một cái khác thu được bằng cách xoay nó bằng$45^\circ$ngược chiều kim đồng hồ và chia tỷ lệ theo$1/\sqrt2$, đặt hàng$n$. Mỗi đoạn của đường cong nhỏ hơn sẽ mở rộng thành đúng hai đoạn của đường cong lớn hơn. Điều này mang lại mối quan hệ đệ quy trực tiếp giữa các ô vuông trong mặt phẳng và các ô vuông ở quy mô nhỏ hơn tiếp theo. 

Mặt phẳng có thể được chia thành$2 \times 2$khối. Tùy thuộc vào tính chẵn lẻ, khối như vậy có thể là "dọc" hoặc "ngang". Từ loại khối, chúng ta có thể rút ra một phép biến đổi xác định ánh xạ hình vuông hiện tại sang hình vuông tương ứng trong đường cong rồng nhỏ hơn. Việc lặp lại thao tác này sẽ tiếp tục thu nhỏ tọa độ. 

Trong khi thu hẹp, chúng tôi cũng duy trì vị trí của đoạn bên trong đoạn được mở rộng đệ quy. Mỗi lần giảm sẽ nhân đôi độ dài đoạn được biểu diễn. Đôi khi thứ tự cục bộ bị đảo ngược nên chúng ta thay thế$$pos \leftarrow (2^k-1)-pos.$$Khi hình vuông cuối cùng trở thành một trong bốn hình vuông liền kề với gốc tọa độ, số đường cong được biết ngay lập tức và vị trí tích lũy sẽ trở thành câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Giảm đệ quy tối ưu | (O(\log(\max( | x | , | 

## Hướng dẫn thuật toán 

1. Bắt đầu với tọa độ vuông được truy vấn$(x,y)$. 
2. Duy trì hai giá trị:`bit`, số lượng phân đoạn ban đầu được đại diện bởi phân đoạn đệ quy hiện tại.`pos`, vị trí bên trong đoạn đó, ban đầu bằng không. 
3. Trong khi hình vuông không phải là một trong bốn hình vuông liền kề với gốc tọa độ, hãy thực hiện một phép rút gọn đệ quy. 
4. Gấp đôi`bit`bởi vì một đoạn của đường cong nhỏ hơn tương ứng với hai đoạn của đường cong lớn hơn. Sau đó$k$giảm, độ dài đoạn được biểu thị là$2^k$. 
5. Kiểm tra xem mức giảm này có đảo ngược trật tự cục bộ hay không. Điều kiện cần có là```
(x ^ y ^ ((x ^ y) >> 1)) & 1
```Nếu đúng thì thay thế```
pos = bit - 1 - pos
```Điều này phản ánh vị trí bên trong phân khúc hiện tại. 
6. Xác định xem dòng điện$2 \times 2$khối nằm ngang hoặc dọc. 

Nếu như```
((x ^ y) >> 1) & 1
```là đúng, hãy sử dụng phép biến đổi theo chiều ngang:```
x, y =
    (x >> 1) + ((y + 1) >> 1),
    ((y + 1) >> 1) - (x >> 1) - 1
```Mặt khác, sử dụng phép biến đổi dọc:```
x, y =
    ((x + 1) >> 1) + (y >> 1),
    (y >> 1) - ((x + 1) >> 1)
```Các công thức này ánh xạ hình vuông thành hình vuông tương ứng trong đường cong rồng nhỏ hơn tiếp theo. 
7. Tiếp tục cho đến khi$(x,y)$thuộc về$2 \times 2$lân cận xung quanh điểm gốc, cụ thể là```
-1 <= x <= 0 and -1 <= y <= 0
```8. Xác định số đường cong từ tọa độ cuối cùng:```
curve = (x & 1 ^ y & 3) + 1
```9. Chỉ số phân đoạn là`pos + 1`bởi vì đầu ra dựa trên 1. 

### Tại sao nó hoạt động 

Mỗi mức giảm ánh xạ hình vuông được truy vấn từ một tỷ lệ của đường cong rồng sang tỷ lệ nhỏ hơn tiếp theo. Cấu trúc hình học của đường cong rồng đảm bảo rằng mỗi hình vuông tương ứng với chính xác một hình vuông trong biểu diễn rút gọn. Phép biến đổi tọa độ bảo toàn sự tương ứng đó. Đồng thời,`bit`ghi lại có bao nhiêu phân đoạn ban đầu đã được hợp nhất vào phân đoạn đệ quy hiện tại và`pos`ghi lại phân đoạn nào chứa truy vấn. Bất cứ khi nào cấu trúc đệ quy đảo ngược hướng, phép biến đổi gương sẽ cập nhật`pos`tương ứng. Sau khi rút gọn đủ, truy vấn sẽ đến một trong bốn đoạn cơ sở liền kề với điểm gốc. Tại thời điểm đó, cả nhận dạng đường cong và vị trí đoạn chính xác đều được xác định duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def dragon_curve(x, y):
    bit = 1
    pos = 0

    while not (-1 <= x <= 0 and -1 <= y <= 0):
        bit <<= 1

        if (x ^ y ^ ((x ^ y) >> 1)) & 1:
            pos = bit - 1 - pos

        if ((x ^ y) >> 1) & 1:
            x, y = (
                (x >> 1) + ((y + 1) >> 1),
                ((y + 1) >> 1) - (x >> 1) - 1
            )
        else:
            x, y = (
                ((x + 1) >> 1) + (y >> 1),
                (y >> 1) - ((x + 1) >> 1)
            )

    curve = ((x & 1) ^ (y & 3)) + 1
    return curve, pos + 1

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        x, y = map(int, input().split())
        c, p = dragon_curve(x, y)
        ans.append(f"{c} {p}")

    sys.stdout.write("\n".join(ans))

solve()
```Việc thực hiện tuân theo việc giảm đệ quy theo nghĩa đen. 

Vòng lặp là trái tim của giải pháp. Mỗi lần lặp sẽ thu nhỏ tọa độ theo hệ số hai, do đó, ngay cả tọa độ lớn nhất cũng chỉ cần khoảng ba mươi lần lặp. 

Biến`bit`luôn bằng độ dài của đoạn đệ quy hiện tại được đo trong các đoạn ban đầu. Đây là lý do tại sao nó tăng gấp đôi trước khi xử lý mức giảm hiện tại. 

Cập nhật đảo ngược rất dễ bị sai. Công thức sử dụng giá trị mới của`bit`, không phải cái cũ. Nếu thứ tự lật bên trong một đoạn có độ dài$L$, chức vụ$p$trở thành$L-1-p$. 

Các phép biến đổi tọa độ phải sử dụng phép dịch số học phải chính xác như được hiển thị. Các ca làm việc của Python hoạt động chính xác đối với các số nguyên âm, phù hợp với hành vi dự định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hình vuông đầu vào:```
(0, 0)
```| x | y | chút | tư thế | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | 0 | 

Hình vuông đã liền kề với điểm gốc nên vòng lặp không bao giờ chạy. 

Giá trị cuối cùng: 

| đường cong | vị trí | 
| --- | --- | 
| 1 | 1 | 

Đầu ra:```
1 1
```Ví dụ này cho thấy trường hợp cơ bản của đệ quy. 

### Ví dụ 2 

Hình vuông đầu vào:```
(-2, 0)
```| Lặp lại | x | y | chút | tư thế | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | -2 | 0 | 1 | 0 | 
| Sau khi giảm | -1 | -1 | 2 | 1 | 

Vòng lặp dừng lại vì$(-1,-1)$liền kề với gốc. 

Giá trị cuối cùng: 

| đường cong | vị trí | 
| --- | --- | 
| 2 | 2 | 

Đầu ra:```
2 2
```Ví dụ này thể hiện sự đảo ngược vị thế. Vị trí cuối cùng trở thành đoạn thứ hai chứ không phải đoạn đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\log(\max( | x | 
| Không gian |$O(1)$| Chỉ có một vài biến số nguyên được lưu trữ | 

Với tọa độ giới hạn bởi$10^9$, mỗi truy vấn thực hiện tối đa khoảng 31 phép rút gọn đệ quy. Vì$2 \cdot 10^5$truy vấn, tổng công việc chỉ là vài triệu phép tính số nguyên đơn giản, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_io(inp: str) -> str:
    data = io.StringIO(inp)
    out = io.StringIO()

    input = data.readline

    def dragon_curve(x, y):
        bit = 1
        pos = 0

        while not (-1 <= x <= 0 and -1 <= y <= 0):
            bit <<= 1

            if (x ^ y ^ ((x ^ y) >> 1)) & 1:
                pos = bit - 1 - pos

            if ((x ^ y) >> 1) & 1:
                x, y = (
                    (x >> 1) + ((y + 1) >> 1),
                    ((y + 1) >> 1) - (x >> 1) - 1
                )
            else:
                x, y = (
                    ((x + 1) >> 1) + (y >> 1),
                    (y >> 1) - ((x + 1) >> 1)
                )

        return ((x & 1) ^ (y & 3)) + 1, pos + 1

    t = int(input())
    for _ in range(t):
        x, y = map(int, input().split())
        c, p = dragon_curve(x, y)
        out.write(f"{c} {p}\n")

    return out.getvalue()

# provided sample
assert solve_io(
"""5
0 0
-2 0
-7 -7
5 -9
9 9
"""
) == (
"""1 1
2 2
3 189
4 186
2 68
"""
)

# base squares around origin
assert solve_io(
"""4
0 0
-1 0
-1 -1
0 -1
"""
) == (
"""1 1
2 1
3 1
4 1
"""
)

# negative coordinate handling
assert solve_io(
"""1
-2 0
"""
) == (
"""2 2
"""
)

# large coordinate stress
solve_io(
"""1
1000000000 1000000000
"""
)

# mixed quadrants
solve_io(
"""4
10 -7
-10 7
-10 -7
7 10
"""
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`(0,0)`|`1 1`| Trường hợp cơ sở | 
|`(-1,0)`|`2 1`| Nhận dạng đường cong gần gốc | 
|`(-1,-1)`|`3 1`| Đường cong cơ sở thứ ba | 
|`(0,-1)`|`4 1`| Đường cong cơ sở thứ tư | 
|`(-2,0)`|`2 2`| Logic đảo ngược vị trí | 
| Tọa độ lớn | Câu trả lời hợp lệ | Độ sâu logarit | 

## Vỏ cạnh 

Xét hình vuông tại`(0,0)`. 

Điều kiện vòng lặp ngay lập tức thất bại vì hình vuông đã thuộc về vùng lân cận cơ sở. Không cần giảm đệ quy. Thuật toán tính toán trực tiếp đường cong`1`và vị trí`1`. Một giải pháp luôn thực hiện ít nhất một lần giảm sẽ di chuyển khỏi cấu hình cơ sở một cách không chính xác. 

Xét hình vuông tại`(-2,0)`. 

Lần giảm đầu tiên tăng gấp đôi`bit`từ`1`ĐẾN`2`. Kiểm tra tính chẵn lẻ chỉ ra rằng thứ tự cục bộ bị đảo ngược, do đó`pos`thay đổi từ`0`ĐẾN`1`. Sau khi biến đổi tọa độ, hình vuông trở thành`(-1,-1)`, đó là một hình vuông cơ sở. Câu trả lời cuối cùng là`2 2`. Bất kỳ triển khai nào bỏ qua việc đảo ngược sẽ trả về vị trí`1`, điều đó là sai. 

Hãy xem xét một tọa độ âm lớn như`(-10^9,-10^9)`. 

Các công thức đệ quy liên tục áp dụng các phép dịch số học cho các giá trị âm. Python bảo toàn dấu trong khi dịch chuyển sang phải, phù hợp với mức giảm hình học dự định. Thay thế`>> 1`với phép chia thông thường và cắt ngắn về 0 sẽ tạo ra các tọa độ trung gian khác nhau và cuối cùng là đường cong sai. Thuật toán tránh được vấn đề này bằng cách sử dụng các phép biến đổi chính xác bắt nguồn từ cấu trúc fractal.
