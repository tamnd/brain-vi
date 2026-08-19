---
title: "CF 102215L - Vòng tròn nội tiếp"
description: "Chúng ta có hai đĩa, mỗi đĩa được mô tả bằng tâm và bán kính dương. Các chu vi của chúng cắt nhau tại đúng hai điểm, do đó không có đĩa nào chứa đĩa kia và giao điểm của chúng là vùng hình thấu kính không suy biến."
date: "2026-08-18T22:13:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102215
codeforces_index: "L"
codeforces_contest_name: "2019, XII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102215
solve_time_s: 525
verified: false
draft: false
---

[CF 102215L - Vòng tròn nội tiếp](https://codeforces.com/problemset/problem/102215/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8 phút 45 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai đĩa, mỗi đĩa được mô tả bằng tâm và bán kính dương. Các chu vi của chúng cắt nhau tại đúng hai điểm, do đó không có đĩa nào chứa đĩa kia và giao điểm của chúng là vùng hình thấu kính không suy biến. 

Chúng ta cần tìm vòng tròn lớn nhất có thể vừa khít hoàn toàn bên trong thấu kính này. Đầu ra là tâm của vòng tròn đó và bán kính của nó, với độ chính xác đủ để đáp ứng sai số tuyệt đối hoặc tương đối là (10^{-9}). 

Tọa độ và bán kính có độ lớn tối đa là (1000), do đó tất cả các khoảng cách có liên quan đều nằm trong phạm vi dấu phẩy động thông thường. Cũng không có kích thước đầu vào lớn: bài toán chỉ chứa hai vòng tròn. Do đó, giới hạn 2 giây không phải là vấn đề đáng lo ngại đối với nghiệm hình học (O(1)). Ngay cả một phương pháp số lặp với vài trăm lần lặp cũng đủ nhanh, nhưng hình học cho phép chúng ta tránh hoàn toàn việc lặp lại. 

Trường hợp tinh tế đầu tiên là khi các vòng tròn có bán kính bằng nhau. Ví dụ,```
0 0 5
6 0 5
```Câu trả lời là```
3 0 2
```Việc triển khai bất cẩn có thể cho rằng trung tâm tối ưu là một trong những trung tâm ban đầu, nhưng trung tâm chính xác lại nằm ở giữa chúng. Tổng quát hơn, tính đối xứng đặt đáp án trên đường nối hai tâm đường tròn, không nhất thiết phải ở điểm giữa của nó. 

Một trường hợp khác là khi bán kính khác nhau:```
0 0 5
7 0 3
```Ở đây các vòng tròn giao nhau vì (2 < 7 < 8). Tâm tối ưu nằm ở khoảng cách 

[ 
\frac{7+5-3}{2}=4.5 
] 

từ tâm đầu tiên và bán kính kết quả là 

[ 
\frac{5+3-7}{2}=0,5. 
] 

Một lỗi phổ biến là sử dụng điểm giữa của tâm bất kể bán kính. Điều đó sẽ đưa ra câu trả lời sai vì vòng tròn lớn hơn có thể điều chỉnh tâm tối ưu xa hơn về phía vòng tròn nhỏ hơn. 

Trường hợp cạnh cuối cùng liên quan đến các đường tròn có ranh giới giao nhau rất gần với tiếp tuyến. Ví dụ,```
0 0 1
1.999999 0 1
```Câu trả lời có bán kính rất nhỏ, xấp xỉ (5\times10^{-7}). Việc triển khai sử dụng số học số nguyên, độ chính xác không đủ hoặc các công thức liên quan đến phép trừ số lượng gần bằng nhau một cách bất cẩn có thể làm mất độ chính xác. Công thức trực tiếp sử dụng khoảng cách trung tâm vẫn đủ ổn định với độ chính xác gấp đôi của Python đối với lỗi yêu cầu. 

## Phương pháp tiếp cận 

Một cách tiếp cận hình học đơn giản có thể tìm kiếm các vị trí trung tâm có thể có trên một lưới mịn. Đối với mỗi điểm ứng cử viên, chúng ta sẽ tính toán bán kính có thể được đặt ở đó là bao nhiêu, cụ thể là khoảng cách nhỏ hơn của nó đến ranh giới hai vòng tròn. Để đạt được độ chính xác vị trí (10^{-9}) trên phạm vi tọa độ khoảng (2000), một lưới thống nhất sẽ yêu cầu theo thứ tự 

[ 
2000^2 / 10^{-18} = 4\cdot10^{24} 
] 

điểm ứng viên. Ngay cả một phép tính thời gian không đổi cho mỗi điểm cũng là vô vọng. 

Cách tiếp cận số hợp lý hơn sẽ giảm việc tìm kiếm xuống một chiều và sử dụng tìm kiếm ba chiều. Tính đối xứng của thấu kính có nghĩa là tâm tối ưu nằm trên đường nối hai tâm ban đầu. Chúng ta có thể tham số hóa đường đó và tối đa hóa bán kính khả thi về mặt số lượng. Một vài trăm lần lặp lại là đủ, vì vậy cách tiếp cận này thực sự đủ nhanh, nhưng nó không cần thiết và đưa ra các chi tiết hội tụ và chính xác mà hình học chính xác tránh được. 

Quan sát quan trọng là đối với một điểm trên đoạn nối các tâm, khoảng cách đến tâm thứ nhất tăng đúng như khoảng cách đến tâm thứ hai giảm. Giả sử các tâm là (C_1,C_2), khoảng cách của chúng là (d) và tâm ứng cử viên (P) nằm ở khoảng cách (t) từ (C_1). Khi đó nó là (d-t) từ (C_2). 

Một đường tròn có tâm tại (P) vừa khít bên trong đĩa thứ nhất có bán kính lớn nhất 

[ 
r_1-t, 
] 

và nó vừa với đĩa thứ hai có bán kính nhiều nhất 

[ 
r_2-(d-t). 
] 

Do đó bán kính khả thi tối đa của nó tại (P) là 

[ 
f(t)=\min(r_1-t,\ r_2-d+t). 
] 

Biểu thức đầu tiên giảm theo (t), trong khi biểu thức thứ hai tăng theo (t). Mức tối đa của mức tối thiểu của chúng xảy ra chính xác ở nơi chúng bằng nhau. Giải quyết 

[ 
r_1-t=r_2-d+t 
] 

cho 

[ 
t=\frac{d+r_1-r_2}{2}. 
] 

Việc thay thế giá trị này sẽ cho 

[ 
r=\frac{r_1+r_2-d}{2}. 
] 

Bài toán đảm bảo rằng các đường tròn có đúng hai điểm chung. 

[ 
|r_1-r_2|<d<r_1+r_2. 
] 

Do đó, (t) tính toán nằm hoàn toàn giữa (0) và (d) và bán kính tính toán hoàn toàn dương. Sau đó chúng ta có thể đặt câu trả lời tại điểm tương ứng trên đường nối giữa hai trung tâm. 

Tìm kiếm vũ phu hoạt động hiệu quả vì nó đánh giá trực tiếp bán kính khả thi của các trung tâm ứng cử viên, nhưng nó lãng phí gần như toàn bộ công việc khám phá một vùng liên tục hai chiều. Quan sát rằng tâm tối ưu phải nằm trên trục từ tâm đến tâm làm giảm bài toán xuống một chiều, và thực tế là hai bán kính giới hạn là các hàm tuyến tính làm giảm tối ưu hóa một chiều đó để giải một phương trình. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(1/\varepsilon^2)) mẫu lưới | (O(1)) | Quá chậm | 
| Tìm kiếm ternary số | (O(I)) | (O(1)) | Được chấp nhận nhưng không cần thiết | 
| Công thức hình học | (O(1)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tâm (C_1=(x_1,y_1)), (C_2=(x_2,y_2)) và bán kính (r_1,r_2). Đại lượng hình học duy nhất chúng ta cần ban đầu là khoảng cách giữa hai tâm. 
2. Tính toán 

[ 
d=\sqrt{(x_2-x_1)^2+(y_2-y_1)^2}. 
] 

Vì các đường tròn cắt nhau tại hai điểm (d>0), nên hướng từ (C_1) đến (C_2) được xác định rõ. 

1. Xác định khoảng cách từ tâm mong muốn (C_1): 

[ 
t=\frac{d+r_1-r_2}{2}. 
] 

Điều này xuất phát từ việc cân bằng hai khoảng cách ranh giới sẵn có. Nếu câu trả lời gần với (C_1), thì vòng tròn đầu tiên sẽ cho phép bán kính lớn hơn trong khi vòng tròn thứ hai sẽ là vòng tròn giới hạn. Nếu nó ở xa hơn về phía (C_2) thì tình thế sẽ đảo ngược. Mức tối đa chính xác là ở điểm cân bằng. 

1. Chuyển khoảng cách (t) thành tọa độ. Vectơ đơn vị từ (C_1) đến (C_2) là

[ 
\left(\frac{x_2-x_1}{d},\frac{y_2-y_1}{d}\right). 
] 

Vì vậy, trung tâm câu trả lời là 

[ 
x=x_1+\frac{x_2-x_1}{d}t, 
\qquad 
y=y_1+\frac{y_2-y_1}{d}t. 
] 

1. Tính bán kính bằng đường tròn giới hạn: 

[ 
r=r_1-t. 
] 

Sau khi thay thế giá trị của (t), giá trị này cũng có thể được viết là 

[ 
r=\frac{r_1+r_2-d}{2}. 
] 

1. In tâm và bán kính bằng nhiều chữ số sau dấu thập phân. của Python`float`là IEEE-754 kép 64 bit, cung cấp độ chính xác cao hơn đáng kể so với lỗi (10^{-9}) bắt buộc. 

### Tại sao nó hoạt động 

Đối với bất kỳ đường tròn nào có tâm tại một điểm (P) bên trong thấu kính, bán kính của nó không thể vượt quá khoảng cách từ (P) đến chu vi ban đầu. Tâm tối ưu có thể được chọn trên đường nối các tâm ban đầu vì việc phản chiếu bất kỳ đường tròn khả thi nào qua đường đó sẽ bảo toàn cả hai đĩa gốc và thấu kính đối xứng qua đường đó. Trên trục này, hai bán kính khả dụng là (r_1-t) và (r_2-d+t). Một cái giảm khi tâm di chuyển về phía (C_2), trong khi cái kia tăng. Mức tối thiểu của chúng được tối đa hóa chính xác tại giao điểm của chúng. Thuật toán tính toán giao điểm đó và đặt tâm ở đó, do đó không có điểm nào khác có thể thừa nhận một vòng tròn lớn hơn. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    x1, y1, r1 = map(int, input().split())
    x2, y2, r2 = map(int, input().split())

    dx = x2 - x1
    dy = y2 - y1

    d = math.hypot(dx, dy)

    t = (d + r1 - r2) / 2.0

    x = x1 + dx * t / d
    y = y1 + dy * t / d

    r = (r1 + r2 - d) / 2.0

    print(f"{x:.15f} {y:.15f} {r:.15f}")

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc hai vòng tròn và tính toán`dx`Và`dy`, độ dịch chuyển từ tâm thứ nhất đến tâm thứ hai.`math.hypot(dx, dy)`tính khoảng cách tâm mà không yêu cầu chúng ta viết căn bậc hai một cách rõ ràng. 

Biến`t`là khoảng cách từ tâm đầu tiên đến tâm tối ưu. Sự đảm bảo về hai giao điểm có nghĩa là`t`hoàn toàn dương và hoàn toàn nhỏ hơn`d`, do đó chia cho`d`là an toàn. 

Tọa độ thu được bằng cách di chuyển từ tâm thứ nhất theo hướng của tâm thứ hai một cách chính xác.`t`. Viết phép tính dưới dạng`dx * t / d`Và`dy * t / d`tránh việc xây dựng một bộ vectơ đơn vị một cách riêng biệt. 

Bán kính sử dụng công thức đối xứng`(r1 + r2 - d) / 2`. Tính trực tiếp thì đơn giản hơn tính`r1 - t`, và nó tránh được một phép trừ thêm không cần thiết. 

Không có vấn đề tràn số nguyên trong Python vì số nguyên có độ chính xác tùy ý. Các phép toán dấu phẩy động duy nhất liên quan đến các giá trị có độ lớn tối đa là vài nghìn, do đó độ chính xác gấp đôi có thể xử lý chúng một cách thoải mái. 

In mười lăm chữ số sau dấu thập phân mang lại độ chính xác cao hơn nhiều so với yêu cầu. Thẩm phán so sánh các giá trị số, không phải định dạng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
0 0 5
6 0 5
```Các biến chính phát triển như sau. 

| Biến | Giá trị | 
| --- | --- | 
| (dx) | (6) | 
| (dy) | (0) | 
| (d) | (6) | 
| (t=(d+r_1-r_2)/2) | (3) | 
| (x= x_1+dx\cdot t/d) | (3) | 
| (y= y_1+dy\cdot t/d) | (0) | 
| (r=(r_1+r_2-d)/2) | (2) | 

Bán kính có được từ hai đường tròn ban đầu bằng nhau tại điểm giữa, cả hai đều bằng (5-3=2). Di chuyển một trong hai hướng sẽ làm cho một trong số chúng nhỏ hơn, vì vậy điểm giữa là tối ưu. 

### Mẫu 2 

Đầu vào là```
-12 34 56
78 -90 123
```Giá trị dịch chuyển và dẫn xuất xấp xỉ 

| Biến | Giá trị | 
| --- | --- | 
| (dx) | (90) | 
| (dy) | (-124) | 
| (d) | (153.2239) | 
| (t=(d+56-123)/2) | (43.1119) | 
| (x) | (13.3222578218559) | 
| (y) | (-0.8884441101126) | 
| (r=(56+123-d)/2) | (12.8906010988208) | 

Ở đây bán kính khá khác nhau nên tâm tối ưu không nằm giữa các tâm ban đầu. Nó nằm gần tâm của vòng tròn nhỏ hơn, đúng như phương trình cân bằng dự đoán. 

Hai khoảng cách biên từ tâm kết quả đều xấp xỉ (12,8906011) sau khi trừ đi khoảng cách tâm thích hợp từ bán kính ban đầu tương ứng. Điều này xác nhận bất biến trung tâm rằng cả hai vòng tròn ban đầu đều chặt chẽ ở mức tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(1)) | Một số lượng không đổi các phép tính số học và một căn bậc hai được thực hiện. | 
| Không gian | (O(1)) | Chỉ có hai vòng tròn và một số giá trị trung gian không đổi được lưu trữ. | 

Đầu vào chỉ chứa hai vòng tròn, do đó, giải pháp (O(1)) thấp hơn nhiều so với giới hạn 2 giây và 256 MB. Quan trọng hơn, công thức này tránh được bất kỳ sự điều chỉnh độ chính xác lặp đi lặp lại nào, làm cho hành vi số có thể dự đoán được. 

## Trường hợp thử nghiệm 

Đối với đầu ra dấu phẩy động, việc so sánh chuỗi đầu ra hoàn chỉnh là không phù hợp vì nhiều cách biểu diễn thập phân khác nhau có thể mô tả cùng một câu trả lời. Trình trợ giúp kiểm tra bên dưới phân tích cú pháp ba số và kiểm tra chúng với sai số tuyệt đối và tương đối nghiêm ngặt.```python
import sys
import io
import math

def solve():
    input = sys.stdin.readline

    x1, y1, r1 = map(int, input().split())
    x2, y2, r2 = map(int, input().split())

    dx = x2 - x1
    dy = y2 - y1

    d = math.hypot(dx, dy)

    t = (d + r1 - r2) / 2.0

    x = x1 + dx * t / d
    y = y1 + dy * t / d

    r = (r1 + r2 - d) / 2.0

    print(f"{x:.15f} {y:.15f} {r:.15f}")

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

def assert_answer(inp: str, expected, message: str):
    out = run(inp).split()
    got = list(map(float, out))

    assert len(got) == 3, message

    for a, b in zip(got, expected):
        assert math.isclose(a, b, rel_tol=1e-9, abs_tol=1e-9), (
            f"{message}: got {got}, expected {expected}"
        )

# Provided sample 1
assert_answer(
    "0 0 5\n6 0 5\n",
    (3.0, 0.0, 2.0),
    "sample 1"
)

# Provided sample 2
assert_answer(
    "-12 34 56\n78 -90 123\n",
    (13.322257821855908, -0.888444110112585, 12.890601098820779),
    "sample 2"
)

# Minimum radii, equal circles, center distance 1
assert_answer(
    "0 0 1\n1 0 1\n",
    (0.5, 0.0, 0.5),
    "minimum-size circles"
)

# Equal values and diagonal center displacement
# d = sqrt(2), so t = sqrt(2)/2 and the answer is (0.5, 0.5).
assert_answer(
    "0 0 1\n1 1 1\n",
    (0.5, 0.5, (2.0 - math.sqrt(2.0)) / 2.0),
    "equal circles on a diagonal"
)

# Very close to external tangency
assert_answer(
    "0 0 1\n1.999999 0 1\n",
    (0.9999995, 0.0, 0.0000005),
    "near-tangent circles"
)

# Unequal radii, catches midpoint assumptions
assert_answer(
    "0 0 5\n7 0 3\n",
    (4.5, 0.0, 0.5),
    "unequal radii"
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 1`Và`1 0 1`|`(0.5, 0, 0.5)`| Bán kính tối thiểu và trường hợp đối xứng | 
|`0 0 1`Và`1 1 1`|`(0.5, 0.5, (2-sqrt(2))/2)`| Hướng chéo và chuẩn hóa khoảng cách | 
|`0 0 1`Và`1.999999 0 1`|`(0.9999995, 0, 0.0000005)`| Độ chính xác gần tiếp tuyến | 
|`0 0 5`Và`7 0 3`|`(4.5, 0, 0.5)`| Bán kính không bằng nhau và tối ưu không ở điểm giữa | 

## Vỏ cạnh 

Trường hợp bán kính bằng nhau```
0 0 1
1 0 1
```có (d=1), vậy 

[ 
t=\frac{1+1-1}{2}=0,5 
] 

và 

[ 
r=\frac{1+1-1}{2}=0,5. 
] 

Trung tâm là`(0.5, 0)`. Việc triển khai dựa trên điểm giữa sẽ hoạt động ở đây, nhưng công thức giải thích lý do tại sao nó hoạt động và cũng khái quát hóa cho các bán kính không bằng nhau. 

Trường hợp đường chéo```
0 0 1
1 1 1
```có (d=\sqrt2). Công thức cho 

[ 
t=\frac{\sqrt2}{2}. 
] 

Đơn vị hướng từ tâm thứ nhất đến tâm thứ hai là 

[ 
\left(\frac1{\sqrt2},\frac1{\sqrt2}\right), 
] 

do đó nhân với (t) được`(0.5, 0.5)`. Điều này nắm bắt các triển khai vô tình sử dụng`dx`Và`dy`trực tiếp như thể khoảng cách trung tâm luôn nằm ngang. 

Trường hợp gần tiếp tuyến```
0 0 1
1.999999 0 1
```có 

[ 
d=1,999999 
] 

và do đó 

[ 
r=\frac{2-1.999999}{2}=0,0000005. 
] 

Trung tâm tối ưu là`(0.9999995, 0)`. Bán kính dương nhưng cực kỳ nhỏ, kiểm tra xem việc triển khai có bảo toàn đủ độ chính xác của dấu phẩy động hay không. 

Cuối cùng, trường hợp bán kính không bằng nhau```
0 0 5
7 0 3
```cho 

[ 
t=\frac{7+5-3}{2}=4.5 
] 

và 

[ 
r=\frac{5+3-7}{2}=0,5. 
] 

Trung tâm là`(4.5, 0)`, không`(3.5, 0)`. Đây là phép thử hữu ích nhất chống lại giả định phổ biến nhưng không chính xác rằng tính đối xứng luôn đặt đáp án ở trung điểm của hai tâm ban đầu.
