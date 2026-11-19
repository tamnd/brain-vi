---
title: "CF 2078D - Quảng cáo trò chơi lừa đảo"
description: "Chúng tôi có hai làn đường. Mỗi làn đường bắt đầu với đúng một người. Cấp độ bao gồm n vòng. Trong mỗi vòng có một cổng trái và một cổng phải. Một cổng có thể thêm một số lượng người cố định hoặc nhân số lượng người hiện có trong làn đường của nó."
date: "2026-06-09T03:42:15+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2078
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1008 (Div. 2)"
rating: 1800
weight: 2078
solve_time_s: 153
verified: true
draft: false
---

[CF 2078D - Quảng cáo trò chơi lừa đảo](https://codeforces.com/problemset/problem/2078/D) 

**Đánh giá:** 1800 
**Tags:** dp, tham lam, thực hiện 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai làn đường. Mỗi làn đường bắt đầu với đúng một người. 

Mức độ bao gồm`n`vòng. Trong mỗi vòng có một cổng trái và một cổng phải. Một cổng có thể thêm một số lượng người cố định hoặc nhân số lượng người hiện có trong làn đường của nó. 

Chi tiết quan trọng là các cổng không trực tiếp sửa đổi số làn đường. Thay vào đó, mỗi cổng tạo ra một số lượng người mới. 

Nếu một cổng là`+ a`, nó tạo ra chính xác`a`những người mới. 

Nếu một cổng là`x a`, và làn đường hiện có chứa`k`mọi người, nó tạo ra`(a - 1) * k`những người mới. 

Sau khi cả hai cổng của vòng đều tạo ra người mới, tất cả những người mới được tạo sẽ được gộp lại với nhau. Chúng tôi có thể phân bổ những người mới đó vào giữa hai làn đường theo cách chúng tôi muốn. Những người hiện tại không thể được di chuyển, chỉ những người mới được tạo mới có thể được chuyển nhượng. 

Nhiệm vụ là tối đa hóa tổng số người ở cả hai làn sau tất cả các vòng. 

Số vòng nhiều nhất là 30, rất ít. Thách thức không phải là quy mô`n`, nhưng có rất nhiều cách có thể để phân bổ những người mới được tạo giữa các làn đường. Tìm kiếm trực tiếp trên tất cả các phân bổ sẽ bùng nổ theo kiểu kết hợp. 

Một điểm tinh tế là cổng nhân chỉ phụ thuộc vào số lượng người đã có mặt ở làn riêng của mình trước khi vòng đấu bắt đầu. Những người được tạo trong cùng một vòng không thể tăng hiệu ứng nhân lên đó. Bất kỳ giải pháp nào xử lý phân bổ và nhân không đúng thứ tự sẽ bị tính quá mức. 

Coi như:```
1
1
x 3 x 3
```Ban đầu cả hai làn đều có một người. 

Hai cổng tạo ra`2 + 2 = 4`những người mới. Chúng ta có thể phân bổ bốn người đó theo cách chúng ta muốn, nhưng tổng số cuối cùng luôn là`2 + 4 = 6`. 

Một cách tiếp cận bất cẩn có thể cho rằng việc đưa những người được tạo ra vào một làn đường có thể ngay lập tức giúp nhân số khác trong cùng một vòng, tạo ra giá trị lớn hơn. Điều đó không được phép. 

Một sai lầm dễ mắc phải khác là cho rằng việc cân bằng làn đường luôn là tối ưu. 

Coi như:```
1
2
+ 100 + 0
x 3 + 0
```Tất cả 100 người được tạo nên được xếp vào làn bên trái vì làn đó sau này sẽ được nhân lên. Chia đều chúng sẽ mất giá trị. Các vòng trong tương lai sẽ xác định lợi nhuận hiện tại nên được đầu tư vào đâu. 

Khó khăn cốt lõi là quyết định xem những người mới được tạo trong mỗi vòng sẽ đi đâu, có tính đến tất cả các phép nhân trong tương lai. 

## Phương pháp tiếp cận 

Chế độ xem brute-force rất hữu ích để hiểu cấu trúc. 

Giả sử chúng ta biết dân số hiện tại`(l, r)`. Một vòng tạo ra một số số`g`của những người mới. Chúng ta có thể chọn bất kỳ sự phân chia nào:```
x people to the left
g - x people to the right
```với mọi số nguyên`0 ≤ x ≤ g`. 

Khám phá đệ quy tất cả các lựa chọn như vậy là đúng vì nó kiểm tra mọi chiến lược phân bổ có thể. Vấn đề là ở chỗ đó`g`có thể trở nên rất lớn sau vài lần nhân. Ngay cả khi bằng cách nào đó chúng ta giới hạn số lượng lựa chọn mỗi vòng, hệ số phân nhánh vẫn sẽ tăng theo cấp số nhân với`n`. 

Quan sát quan trọng là trò chơi hoàn toàn tuyến tính. 

Hãy tưởng tượng chúng ta đang đứng trước một vòng nào đó`i`. Giả sử câu trả lời cuối cùng có thể đạt được tối đa từ thời điểm đó trở đi có thể được viết là```
A * l + B * r + C
```Ở đâu`l`Và`r`là số lượng làn đường hiện tại. 

Vào cuối trò chơi, điều này rõ ràng là đúng:```
final = l + r
```vì vậy ban đầu```
A = 1
B = 1
C = 0
```Bây giờ hãy xem xét một vòng trước đó. 

Cho phép`g`là tổng số người mới được tạo trong vòng đó. Nếu chúng tôi gửi`x`của họ sang làn bên trái, giá trị tương lai sẽ trở thành```
A(l + x) + B(r + g - x) + C
```đơn giản hóa để```
Al + Br + C + Bg + (A - B)x
```Biến duy nhất là`x`. 

Nếu như`A > B`, mỗi người thêm có giá trị hơn ở làn bên trái, vì vậy tất cả những người mới được tạo nên đi bên trái. 

Nếu như`B > A`, tất cả những người mới được tạo nên đi đúng. 

Nếu chúng bằng nhau thì sự phân chia nào cũng là tối ưu. 

Đây là bước đột phá. Chúng ta không bao giờ cần phải xem xét phân phối một phần. Số người được tạo ra trong mỗi vòng sẽ được đầu tư hoàn toàn vào bất kỳ làn đường nào có hệ số tương lai lớn hơn. 

Khi đã biết điều này, chúng ta có thể xử lý các vòng ngược và cập nhật các hệ số`(A, B, C)`. 

Bởi vì các cổng nhân đóng góp một giá trị tỷ lệ thuận với số lượng làn đường hiện tại và các cổng bổ sung đóng góp một hằng số nên dạng tuyến tính được giữ nguyên trong toàn bộ quá trình quét ngược. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định hàm giá trị cho hậu tố của vòng tròn như sau:```
F(l, r) = A * l + B * r + C
```Ở đâu`(A, B, C)`mô tả mức thu nhập tối ưu trong tương lai. 
2. Bắt đầu từ trạng thái sau vòng cuối cùng. 

Tại thời điểm đó:```
F(l, r) = l + r
```vì vậy hãy khởi tạo:```
A = 1
B = 1
C = 0
```3. Xử lý các vòng từ phải sang trái. 
4. Hãy:```
m = max(A, B)
```Bất kỳ người mới được tạo nào cũng phải được chỉ định hoàn toàn vào làn đường có hệ số`m`, bởi vì điều đó mang lại sự đóng góp lớn nhất trong tương lai. 
5. Thể hiện mỗi cổng dưới dạng đóng góp tuyến tính. 

Đối với một cổng`+ a`:```
gain = a
```tương ứng với:```
0 * lane + a
```Đối với một cổng`x a`:```
gain = (a - 1) * lane
```tương ứng với:```
(a - 1) * lane + 0
```6. Hãy để cổng trái đóng góp```
cL * l + dL
```và cổng bên phải đóng góp```
cR * r + dR
```Tổng số người được tạo ra là```
g = cL * l + cR * r + dL + dR
```7. Vì mỗi người được tạo ra đều có giá trị`m`trong tương lai, hãy thêm`m * g`đến hàm giá trị hiện tại. 

Điều này tạo ra:```
A' = A + m * cL
B' = B + m * cR
C' = C + m * (dL + dR)
```8. Thay thế`(A, B, C)`qua`(A', B', C')`và tiếp tục. 
9. Sau khi tất cả các vòng đã được xử lý, trạng thái ban đầu là`(1, 1)`. 

Câu trả lời là:```
A + B + C
```### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý một hậu tố vòng, câu trả lời cuối cùng tối ưu từ thời điểm đó luôn có thể được biểu diễn dưới dạng hàm tuyến tính```
A * l + B * r + C.
```Trường hợp cơ bản là đúng vì điểm cuối cùng chỉ đơn giản là`l + r`. 

Giả sử bất biến giữ cho vòng`i + 1`. Trong vòng đấu`i`, quyết định duy nhất là làm thế nào để phân phối những người mới được tạo ra. Vì giá trị tương lai là tuyến tính nên mỗi người thêm vào đều đóng góp hoặc`A`hoặc`B`tùy theo làn đường mà nhận được nó. Gửi một người đi làn đường có hệ số lớn hơn ít nhất luôn tốt như bất kỳ lựa chọn nào khác. Vì vậy mỗi người được tạo ra đều có giá trị`max(A, B)`. 

Việc thêm giá trị này vào hàm tuyến tính hiện tại sẽ tạo ra một hàm tuyến tính khác, bảo toàn bất biến. Bằng quy nạp, bất biến giữ nguyên cho mọi vòng, kể cả khi bắt đầu trò chơi. Giá trị kết quả tại`(1, 1)`là câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        gates = []

        for _ in range(n):
            op1, a1, op2, a2 = input().split()
            gates.append((op1, int(a1), op2, int(a2)))

        A = 1
        B = 1
        C = 0

        for op1, a1, op2, a2 in reversed(gates):
            m = max(A, B)

            if op1 == '+':
                cL, dL = 0, a1
            else:
                cL, dL = a1 - 1, 0

            if op2 == '+':
                cR, dR = 0, a2
            else:
                cR, dR = a2 - 1, 0

            A = A + m * cL
            B = B + m * cR
            C = C + m * (dL + dR)

        print(A + B + C)

solve()
```Các biến`A`,`B`, Và`C`lưu trữ các hệ số của hàm giá trị tối ưu cho hậu tố đã được xử lý. 

Đối với mỗi vòng đầu tiên chúng tôi tính toán`m = max(A, B)`. Điều này thể hiện giá trị tương lai của một người mới được tạo ra, bởi vì tất cả những người được tạo ra phải được đầu tư vào làn đường có lợi hơn. 

Mỗi cổng được chuyển đổi thành dạng:```
c * lane + d
```điều này làm cho việc cập nhật đại số trở nên đơn giản. 

Bản cập nhật phải sử dụng giá trị cũ của`m`. Tính toán lại`m`sau khi cập nhật`A`hoặc`B`sẽ không chính xác vì quyết định phân bổ thuộc về vòng hiện tại chứ không phải vòng tương lai. 

Các số nguyên của Python tự động tăng đến kích thước tùy ý, điều này là cần thiết vì các phép nhân lặp lại có thể làm cho câu trả lời lớn hơn nhiều so với giới hạn 64 bit. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
+ 4 x 2
x 3 x 3
+ 7 + 4
```Xử lý ngược: 

| Vòng | Một trước | B trước | C trước | m | Sau | B sau | C sau | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Kết thúc | 1 | 1 | 0 | - | 1 | 1 | 0 | 
| +7 +4 | 1 | 1 | 0 | 1 | 1 | 1 | 11 | 
| x3 x3 | 1 | 1 | 11 | 1 | 3 | 3 | 11 | 
| +4 x2 | 3 | 3 | 11 | 3 | 3 | 6 | 23 | 

Câu trả lời cuối cùng:```
A + B + C = 3 + 6 + 23 = 32
```Ví dụ này cho thấy hàm giá trị vẫn tuyến tính trong suốt quá trình quét ngược. 

### Mẫu 4 

đầu vào:```
5
x 3 x 3
x 2 x 2
+ 21 + 2
x 2 x 3
+ 41 x 3
```Xử lý ngược: 

| Vòng | Một trước | B trước | C trước | m | Sau | B sau | C sau | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Kết thúc | 1 | 1 | 0 | - | 1 | 1 | 0 | 
| +41 x3 | 1 | 1 | 0 | 1 | 1 | 3 | 41 | 
| x2 x3 | 1 | 3 | 41 | 3 | 4 | 9 | 41 | 
| +21 +2 | 4 | 9 | 41 | 9 | 4 | 9 | 248 | 
| x2 x2 | 4 | 9 | 248 | 9 | 13 | 18 | 248 | 
| x3 x3 | 13 | 18 | 248 | 18 | 49 | 54 | 248 | 

Câu trả lời cuối cùng:```
49 + 54 + 248 = 351
```Dấu vết này nêu bật vai trò của`m = max(A, B)`. Khi làn đường bên phải trở nên có giá trị hơn, tất cả những người mới được tạo ra đều được coi là khoản đầu tư cho làn đường bên phải trong tương lai một cách hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một đường chuyền ngược qua các vòng đấu | 
| Không gian | O(1) | Chỉ có một vài hệ số được lưu trữ | 

Mỗi trường hợp thử nghiệm thực hiện một lượng công việc không đổi trong mỗi vòng. Từ`n ≤ 30`, thời gian chạy rất nhỏ. Việc sử dụng bộ nhớ không đổi bất kể kích thước đầu vào, dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        gates = []

        for _ in range(n):
            op1, a1, op2, a2 = input().split()
            gates.append((op1, int(a1), op2, int(a2)))

        A = B = 1
        C = 0

        for op1, a1, op2, a2 in reversed(gates):
            m = max(A, B)

            if op1 == '+':
                cL, dL = 0, a1
            else:
                cL, dL = a1 - 1, 0

            if op2 == '+':
                cR, dR = 0, a2
            else:
                cR, dR = a2 - 1, 0

            A = A + m * cL
            B = B + m * cR
            C = C + m * (dL + dR)

        out.append(str(A + B + C))

    return "\n".join(out)

# provided sample
assert run(
"""4
3
+ 4 x 2
x 3 x 3
+ 7 + 4
4
+ 9 x 2
x 2 x 3
+ 9 + 10
x 2 + 1
4
x 2 + 1
+ 9 + 10
x 2 x 3
+ 9 x 2
5
x 3 x 3
x 2 x 2
+ 21 + 2
x 2 x 3
+ 41 x 3
"""
) == """32
98
144
351"""

# minimum size
assert run(
"""1
1
+ 1 + 1
"""
) == "4"

# single multiplication pair
assert run(
"""1
1
x 3 x 3
"""
) == "6"

# all additions
assert run(
"""1
2
+ 5 + 5
+ 5 + 5
"""
) == "22"

# multiplication only chain
assert run(
"""1
2
x 2 x 2
x 2 x 2
"""
) == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một vòng,`+1 +1`| 4 | Kích thước đầu vào tối thiểu | 
| Một vòng,`x3 x3`| 6 | Công thức tính nhân | 
| Hai vòng bổ sung | 22 | Đóng góp liên tục tích lũy chính xác | 
| Hai vòng`x2 x2`| 8 | Tuyên truyền hệ số lặp lại | 

## Vỏ cạnh 

Hãy xem xét mức độ nhỏ nhất có thể:```
1
1
+ 1 + 1
```Khởi tạo ngược mang lại:```
A = 1
B = 1
C = 0
```Vòng đóng góp của hai người không đổi. Từ`m = 1`:```
C = 2
```Câu trả lời trở thành:```
1 + 1 + 2 = 4
```phù hợp với mô phỏng trực tiếp. 

Bây giờ hãy xem xét một phép nhân thuần túy:```
1
1
x 3 x 3
```Hai cổng tạo ra:```
2 * l + 2 * r
```Với`A = B = 1`, chúng tôi có`m = 1`, Vì thế:```
A = 3
B = 3
C = 0
```Câu trả lời là:```
3 + 3 = 6
```Điều này xác nhận rằng những người mới được tạo ra không được phép tác động đến phép nhân trong cùng một vòng. 

Cuối cùng, hãy xem xét trường hợp một làn đường trở nên có giá trị hơn nhiều:```
1
2
+ 100 + 0
x 3 + 0
```Sản lượng xử lý ngược:```
After second round: A = 3, B = 1
```Từ`A > B`, mọi người được tạo ra ở vòng đầu tiên phải được xếp vào làn bên trái. Thuật toán nắm bắt điều này một cách tự động thông qua```
m = max(A, B) = 3.
```Một chiến lược luôn cân bằng các làn đường sẽ bỏ lỡ cơ hội này và tạo ra một câu trả lời nhỏ hơn. Quyết định dựa trên hệ số xác định chính xác nơi mà các phép nhân trong tương lai tạo ra khoản đầu tư có lợi nhất.
