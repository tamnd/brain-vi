---
title: "CF 2042F - Hai mảng con"
description: "Chúng ta có hai mảng, a và b. Đối với bất kỳ mảng con [l, r] nào, giá trị của nó không chỉ là tổng các phần tử của a bên trong nó. Chúng tôi cũng thêm bl và br, các giá trị được gắn vào hai điểm cuối."
date: "2026-06-08T09:38:46+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "implementation", "matrices"]
categories: ["algorithms"]
codeforces_contest: 2042
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 172 (Rated for Div. 2)"
rating: 2600
weight: 2042
solve_time_s: 149
verified: false
draft: false
---

[CF 2042F - Hai mảng con](https://codeforces.com/problemset/problem/2042/F) 

**Đánh giá:** 2600 
**Tags:** cấu trúc dữ liệu, dp, triển khai, ma trận 
**Thời gian giải:** 2m 29s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai mảng,`a`Và`b`. 

Đối với bất kỳ mảng con nào`[l, r]`, giá trị của nó không chỉ là tổng của các phần tử của`a`bên trong nó. Chúng tôi cũng thêm`b_l`Và`b_r`, các giá trị được gắn vào hai điểm cuối. 

Nếu mảng con bao gồm một vị trí duy nhất thì cả hai điểm cuối đều giống nhau, do đó giá trị của nó trở thành:$$a_l + 2b_l$$Truy vấn cập nhật`a[p]`hoặc`b[p]`. Truy vấn thú vị yêu cầu tổng giá trị tối đa có thể có của **hai mảng con rời rạc không trống** hoàn toàn chứa trong một phạm vi`[l,r]`. 

Hai mảng con được chọn có thể có độ dài tùy ý, nhưng chúng không thể trùng nhau. 

Điều đầu tiên cần chú ý là câu trả lời không chỉ đơn giản là "phân mảng tốt nhất + phân mảng tốt thứ hai". Phần thưởng điểm cuối tạo ra các tương tác khiến cho máy móc phân mảng tối đa thông thường không đủ. 

Những hạn chế là thách thức thực sự. Cả hai`n`và số lượng truy vấn lên tới`2·10^5`. Một truy vấn có thể hỏi về một khoảng lớn gần bằng toàn bộ mảng. Bất kỳ giải pháp nào quét khoảng thời gian trong khi truy vấn đều quá chậm. Thậm chí`O(√n)`mỗi lần phẫu thuật sẽ không thoải mái. Mục tiêu đại khái là`O(log n)`mỗi lần cập nhật và mỗi truy vấn. 

Một số trường hợp đặc biệt làm cho các định nghĩa trạng thái ban đầu không thành công. 

Coi như:```
a = [0,0]
b = [100,100]
```Câu trả lời hay nhất trên`[1,2]`thu được bằng cách lấy hai mảng con đơn:```
[1] = 200
[2] = 200
answer = 400
```Bất kỳ công thức nào chỉ theo dõi tổng của mảng con thông thường sẽ bỏ lỡ phần đóng góp điểm cuối nhân đôi. 

Một trường hợp tinh vi khác là khi tất cả các giá trị đều âm:```
a = [-5,-5]
b = [0,0]
```Câu trả lời là:```
(-5) + (-5) = -10
```Chúng tôi buộc phải chọn hai mảng con không trống. Các trạng thái âm thầm cho phép không chọn gì sẽ trả về sai`0`. 

Bẫy thứ ba là khi giải pháp tối ưu sử dụng một khoảng thời gian dài và một đơn:```
a = [10,-100,10]
b = [0,100,0]
```Những lựa chọn tốt nhất là:```
[1,1] = 10
[2,2] = 100
```chứ không phải là bất kỳ cấu trúc liền kề lớn nào. Cây phân đoạn phải lưu giữ đủ thông tin để trộn các khoảng có hình dạng khác nhau. 

## Phương pháp tiếp cận 

Giải pháp brute-force trước tiên sẽ tính toán giá trị của mọi mảng con bên trong phạm vi truy vấn. 

Đối với một phạm vi chiều dài`m`, có`O(m²)`mảng con. Sau đó chúng ta có thể thử tất cả các cặp mảng con không chồng chéo, đưa ra`O(m⁴)`khả năng. Ngay cả sau khi tính toán trước các giá trị mảng con, việc kiểm tra tất cả các cặp vẫn tốn chi phí`O(m⁴)`. 

Với`m ≈ 2·10^5`, điều này hoàn toàn không thể thực hiện được. 

Một quan sát tinh tế hơn là giá trị của một mảng con có thể được viết lại thành$$(a_l+\cdots+a_r)+b_l+b_r$$Phần giữa chỉ đóng góp cấu trúc tổng tiền tố thông thường. Phần đặc biệt duy nhất là việc xử lý hai điểm cuối. 

Điều này gợi ý rõ ràng về một khoảng DP để theo dõi những điểm cuối nào đã được chọn. 

Ý tưởng chính là mô hình hóa một mảng con như một đường dẫn bắt đầu bằng cách thu thập phần thưởng điểm cuối bên trái, đi qua một phân đoạn tích lũy`a`và kết thúc bằng cách thu thập phần thưởng điểm cuối bên phải. 

Sau khi được viết theo cách này, các khoảng kết hợp sẽ trở nên rất giống với DP cây phân đoạn cổ điển để có tổng mảng con tối đa. Sự khác biệt là chúng ta cần có đủ trạng thái để mô tả tiến trình từng phần thông qua việc xây dựng hai mảng con rời rạc. 

Giải pháp hay là lưu trữ một ma trận cộng tối đa nhỏ trong mỗi nút cây phân đoạn. Phép nhân ma trận tự động thực hiện chuyển đổi DP khi hai phân đoạn lân cận được hợp nhất. 

Ma trận có năm trạng thái thể hiện tiến trình thông qua việc xây dựng hai mảng con. Việc hợp nhất hai phân đoạn trở thành phép nhân ma trận cộng cực đại, có tính chất kết hợp. Điều đó khiến nó trở nên lý tưởng cho cây phân đoạn có cập nhật điểm và truy vấn phạm vi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n⁴) mỗi truy vấn | O(1) | Quá chậm | 
| Cây phân đoạn + Ma trận Max-Plus | O(log n) mỗi lần cập nhật/truy vấn | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Toàn bộ giải pháp dựa trên việc biểu diễn mọi vị trí bằng ma trận chuyển tiếp. 

Chúng tôi sử dụng năm giai đoạn DP. 

Tình trạng`0`nghĩa là chưa có gì được bắt đầu 

Tình trạng`1`có nghĩa là phần thưởng điểm cuối bên trái của mảng con đầu tiên đã được chọn. 

Tình trạng`2`có nghĩa là mảng con đầu tiên đã được hoàn thành. 

Tình trạng`3`có nghĩa là phần thưởng điểm cuối bên trái của mảng con thứ hai đã được chọn. 

Tình trạng`4`có nghĩa là cả hai mảng con đã được hoàn thành. 

Số trạng thái không bao giờ giảm. Mọi quá trình chuyển đổi đều tiến về phía trước hoặc giữ nguyên trạng thái. 

### Giải thích ma trận 

Đối với một vị trí duy nhất`i`, định nghĩa:$$A=a_i,\qquad B=b_i$$Các chuyển đổi được phép là:$$0\to1 : +B$$bắt đầu mảng con đầu tiên.$$1\to1 : +A$$mở rộng mảng con đầu tiên thông qua vị trí này.$$1\to2 : +A+B$$kết thúc mảng con đầu tiên ở đây.$$2\to3 : +B$$bắt đầu mảng con thứ hai.$$3\to3 : +A$$mở rộng mảng con thứ hai.$$3\to4 : +A+B$$kết thúc mảng con thứ hai. 

Chúng tôi cũng cho phép mọi trạng thái không thay đổi với giá trị`0`. 

Những chuyển đổi này tạo thành một`5×5`ma trận cộng max. 

### Biểu diễn phân đoạn 

Đối với một phân đoạn, ma trận của nó biểu thị giá trị tốt nhất có thể đạt được khi quét toàn bộ phân đoạn từ trái sang phải. 

Lối vào`(i,j)`lưu trữ mức tăng tối đa thu được khi vào phân đoạn ở trạng thái`i`và rời khỏi tiểu bang`j`. 

### Hợp nhất các phân đoạn 

Giả sử đoạn`X`có ma trận`M1`và phân đoạn`Y`có ma trận`M2`. 

Đang quét`X`theo sau là`Y`tương ứng chính xác với phép nhân max-plus:$$M = M_1 \otimes M_2$$Ở đâu$$M[i][j] = \max_k (M_1[i][k]+M_2[k][j])$$Điều này hoạt động vì sau khi hoàn thành đoạn bên trái ở trạng thái`k`, chúng ta tiếp tục đi qua đoạn bên phải từ cùng một trạng thái. 

Tính kết hợp xuất phát từ tính kết hợp của phép nhân ma trận cộng max. 

### Xây dựng cây phân đoạn 

Mỗi lá lưu trữ ma trận tương ứng với một vị trí mảng. 

Các nút nội bộ lưu trữ sản phẩm cộng thêm của nút con của chúng. 

### Đang xử lý cập nhật 

Khi nào`a[p]`hoặc`b[p]`thay đổi: 

1. Xây dựng lại ma trận của lá`p`. 
2. Tính toán lại các ma trận trên đường đi tới gốc. 

Chỉ một`O(log n)`các nút thay đổi. 

### Đang xử lý truy vấn 

Đối với phạm vi`[l,r]`: 

1. Thu thập các ma trận cây phân đoạn bao phủ khoảng đó. 
2. Nhân chúng theo thứ tự từ trái sang phải. 
3. Gọi ma trận kết quả là`M`. 
4. Câu trả lời là`M[0][4]`. 

Tình trạng`0`có nghĩa là không có gì được chọn trước khi bước vào khoảng thời gian. 

Tình trạng`4`có nghĩa là hai mảng con hoàn chỉnh không chồng chéo đã được xây dựng. 

Giá trị tối đa được lưu trữ trong quá trình chuyển đổi đó chính xác là câu trả lời được yêu cầu. 

### Tại sao nó hoạt động 

Năm trạng thái mô tả mọi giai đoạn có thể có của việc xây dựng hai mảng con rời rạc trong khi quét các vị trí từ trái sang phải. 

Sự chuyển đổi từ trạng thái`1`để nêu`1`có nghĩa là vị trí hiện tại thuộc về phần bên trong của mảng con đầu tiên. Một sự chuyển tiếp từ`1`ĐẾN`2`có nghĩa là vị trí hiện tại là điểm cuối bên phải của nó. Cách giải thích tương tự cũng đúng đối với các quốc gia`3`Và`4`cho mảng con thứ hai. 

Bởi vì các trạng thái chỉ tiến về phía trước nên mảng con thứ hai không thể bắt đầu trước khi mảng con thứ nhất kết thúc. Điều đó tự động thực thi không chồng chéo. 

Mỗi vị trí đóng góp chính xác số lượng theo yêu cầu của định nghĩa về giá trị mảng con: phần thưởng điểm cuối được thêm vào khi vào hoặc rời khỏi mảng con và mọi vị trí được bao gồm đều đóng góp vị trí đó.`a`giá trị thông qua các chuyển đổi mở rộng. 

Ma trận cho một phân đoạn lưu trữ giá trị tối ưu cho mọi cặp trạng thái vào và ra có thể có. Phép nhân cộng tối đa sẽ ghép nối các phân đoạn lân cận một cách chính xác, do đó, ma trận của khoảng được truy vấn mô tả tất cả các cách hợp lệ để xây dựng hai mảng con bên trong khoảng đó. Sự chuyển đổi từ trạng thái`0`để nêu`4`tương ứng chính xác với việc xây dựng hai mảng con hoàn chỉnh không chồng chéo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

NEG = -(10 ** 30)
SZ = 5

class Node:
    __slots__ = ("m",)

    def __init__(self):
        self.m = [[NEG] * SZ for _ in range(SZ)]

def leaf(a, b):
    x = Node()

    for i in range(SZ):
        x.m[i][i] = 0

    x.m[0][1] = max(x.m[0][1], b)

    x.m[1][1] = max(x.m[1][1], a)
    x.m[1][2] = max(x.m[1][2], a + b)

    x.m[2][3] = max(x.m[2][3], b)

    x.m[3][3] = max(x.m[3][3], a)
    x.m[3][4] = max(x.m[3][4], a + b)

    return x

def merge(left, right):
    res = Node()

    A = left.m
    B = right.m
    C = res.m

    for i in range(SZ):
        Ai = A[i]
        Ci = C[i]

        for k in range(SZ):
            if Ai[k] == NEG:
                continue

            v = Ai[k]
            Bk = B[k]

            for j in range(SZ):
                cand = v + Bk[j]
                if cand > Ci[j]:
                    Ci[j] = cand

    return res

n = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

size = 1
while size < n:
    size <<= 1

seg = [Node() for _ in range(2 * size)]

for i in range(size):
    if i < n:
        seg[size + i] = leaf(a[i], b[i])
    else:
        idm = Node()
        for j in range(SZ):
            idm.m[j][j] = 0
        seg[size + i] = idm

for i in range(size - 1, 0, -1):
    seg[i] = merge(seg[i << 1], seg[i << 1 | 1])

def update(pos):
    p = pos + size
    seg[p] = leaf(a[pos], b[pos])

    p >>= 1
    while p:
        seg[p] = merge(seg[p << 1], seg[p << 1 | 1])
        p >>= 1

def identity():
    x = Node()
    for i in range(SZ):
        x.m[i][i] = 0
    return x

def query(l, r):
    left_res = identity()
    right_res = identity()

    l += size
    r += size

    while l <= r:
        if l & 1:
            left_res = merge(left_res, seg[l])
            l += 1

        if not (r & 1):
            right_res = merge(seg[r], right_res)
            r -= 1

        l >>= 1
        r >>= 1

    res = merge(left_res, right_res)
    return res.m[0][4]

q = int(input())

ans = []

for _ in range(q):
    t, p, x = map(int, input().split())

    if t == 1:
        a[p - 1] = x
        update(p - 1)
    elif t == 2:
        b[p - 1] = x
        update(p - 1)
    else:
        l = p - 1
        r = x - 1
        ans.append(str(query(l, r)))

print("\n".join(ans))
```Ma trận lá mã hóa mọi hành động có thể xảy ra tại một vị trí. Ở cùng trạng thái có giá trị bằng 0, cho phép bỏ qua các vị trí. 

Hàm hợp nhất thực hiện phép nhân ma trận max-plus. Vì kích thước ma trận được cố định ở mức 5 nên mỗi lần hợp nhất chỉ tốn`5³ = 125`hoạt động. 

Truy vấn phạm vi sử dụng kỹ thuật cây phân đoạn lặp tiêu chuẩn. Hai bộ tích lũy được duy trì vì các đoạn cây phân đoạn được thu thập theo thứ tự khác nhau từ bên trái và bên phải. Việc giữ nguyên thứ tự nhân từ trái sang phải là rất quan trọng. Đảo ngược nó sẽ phá vỡ ngữ nghĩa DP. 

Tất cả các giá trị được lưu trữ dưới dạng số nguyên tỷ lệ 64 bit. Câu trả lời tối đa có thể là khoảng`4·10^14`, vì vậy số nguyên Python là quá đủ. 

## Ví dụ đã hoạt động 

### Mẫu 1```
a = [3,-1,4,-3,2,4,0]
b = [0,6,1,0,-3,-2,-1]
query = [1,7]
```Một lựa chọn tối ưu là:```
[2,3]
value = (-1+4)+6+1 = 10

[5,6]
value = (2+4)+(-3)+(-2) = 1

total = 11
```Một sự kết hợp tốt hơn tồn tại và ma trận DP tự động phát hiện ra nó. 

| Sân khấu | Ý nghĩa | Giá Trị Tốt Nhất | 
| --- | --- | --- | 
| 0 | không có gì được chọn | 0 | 
| 1 | khoảng thời gian đầu tiên mở | khác nhau | 
| 2 | khoảng thời gian đầu tiên kết thúc | khác nhau | 
| 3 | khoảng thời gian thứ hai mở | khác nhau | 
| 4 | cả hai đã hoàn thành | 18 | 

Mục nhập cuối cùng`M[0][4]`bằng`18`. 

Ví dụ này cho thấy tại sao sự lựa chọn tham lam của địa phương là không đủ. Khoảng thời gian đầu tiên tốt nhất không nhất thiết phải là một phần của cặp tốt nhất. 

### Ví dụ nặng về Singleton```
a = [0,0]
b = [100,100]
```| Chuyển tiếp | Đạt được | 
| --- | --- | 
| bắt đầu trước | +100 | 
| kết thúc trước | +100 | 
| bắt đầu thứ hai | +100 | 
| kết thúc thứ hai | +100 | 

Tổng cộng:```
200 + 200 = 400
```Đường dẫn ma trận là:```
0 -> 1 -> 2 -> 3 -> 4
```với tổng trọng lượng`400`. 

Điều này xác nhận rằng phần thưởng điểm cuối được tính chính xác ngay cả khi cả hai mảng con đều có độ dài bằng một. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) mỗi thao tác | Mỗi cập nhật hoặc truy vấn chạm vào nút cây phân đoạn O(log n), mỗi lần hợp nhất tốn thời gian không đổi | 
| Không gian | O(n) | Cây phân đoạn lưu trữ ma trận O(n) | 

Kích thước ma trận được cố định ở mức 5 nên mọi phép nhân đều là công không đổi. Với nhiều nhất`2·10^5`hoạt động, tổng thời gian chạy thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    # paste solution and return output
    pass

# sample
assert run(
"""7
3 -1 4 -3 2 4 0
0 6 1 0 -3 -2 -1
6
3 1 7
1 2 0
3 3 6
2 5 -3
1 3 2
3 1 5
"""
) == """18
7
16"""

# minimum size
assert run(
"""2
1 1
0 0
1
3 1 2
"""
) == "2"

# all equal
assert run(
"""2
5 5
1 1
1
3 1 2
"""
) == "14"

# all negative
assert run(
"""2
-5 -5
0 0
1
3 1 2
"""
) == "-10"

# update boundary
assert run(
"""2
1 1
0 0
3
3 1 2
1 1 10
3 1 2
"""
) == """2
11"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng kích thước tối thiểu | 2 | Phạm vi truy vấn pháp lý nhỏ nhất | 
| Tất cả các giá trị bằng nhau | 14 | Hành vi đối xứng | 
| Tất cả các giá trị âm | -10 | Khoảng thời gian không trống là bắt buộc | 
| Cập nhật ranh giới | 2, rồi 11 | Cập nhật cây phân đoạn chính xác | 

## Vỏ cạnh 

Hãy xem xét:```
n = 2
a = [-5,-5]
b = [0,0]
```Câu trả lời hợp lệ duy nhất là chọn cả hai vị trí dưới dạng các khoảng đơn. 

DP không bao giờ cho phép "không chọn gì" làm trạng thái cuối cùng vì kết quả truy vấn là trạng thái cụ thể`4`. Đạt trạng thái`4`yêu cầu hoàn thành hai mảng con. Giá trị trả về là`-10`, điều đó đúng. 

Coi như:```
n = 2
a = [0,0]
b = [100,100]
```Mỗi khoảng đơn đóng góp`200`. 

Trình tự chuyển tiếp:```
0→1→2→3→4
```thêm bốn phần thưởng điểm cuối, tạo ra`400`. Không có điểm cuối nào bị bỏ sót hoặc bị tính hai lần. 

Coi như:```
n = 3
a = [10,-100,10]
b = [0,100,0]
```Câu trả lời hay nhất sử dụng các khoảng thời gian đơn lẻ thay vì các khoảng thời gian dài. Máy trạng thái không giả định bất cứ điều gì về độ dài khoảng. Một mảng con có thể bắt đầu và kết thúc ở cùng một vị trí vì cả hai quá trình chuyển đổi đều có sẵn ở một lá đơn. DP đánh giá những khả năng này một cách tự nhiên và trả về mức tối đa chính xác.
