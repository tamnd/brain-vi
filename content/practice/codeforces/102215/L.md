---
title: "CF 102215L - Vòng tròn nội tiếp"
description: "Chúng ta có hai vòng tròn, mỗi vòng được mô tả bởi tâm và bán kính của nó. Các chu vi của chúng cắt nhau tại đúng hai điểm, do đó không có đường tròn nào chứa đường tròn kia và hai đĩa chồng lên nhau trong một vùng hình thấu kính thích hợp."
date: "2026-08-17T23:53:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102215
codeforces_index: "L"
codeforces_contest_name: "2019, XII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102215
solve_time_s: 139
verified: false
draft: false
---

[CF 102215L - Vòng tròn nội tiếp](https://codeforces.com/problemset/problem/102215/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai vòng tròn, mỗi vòng được mô tả bởi tâm và bán kính của nó. Các chu vi của chúng cắt nhau tại đúng hai điểm, do đó không có đường tròn nào chứa đường tròn kia và hai đĩa chồng lên nhau trong một vùng hình thấu kính thích hợp. Chúng ta cần hình tròn lớn nhất nằm hoàn toàn bên trong phần chồng lên nhau đó và chúng ta phải in tâm và bán kính của nó. Giới hạn đầu vào và độ chính xác yêu cầu là những giới hạn được đưa ra trong báo cáo vấn đề ban đầu. 

Đặt tâm là (O_1=(x_1,y_1)) và (O_2=(x_2,y_2)), với bán kính (r_1) và (r_2). hãy để 

[ 
d=|O_1O_2| 
] 

là khoảng cách giữa các tâm. Có chính xác hai điểm giao nhau sẽ có bất đẳng thức chặt chẽ 

[ 
|r_1-r_2|<d<r_1+r_2. 
] 

Giới hạn trên đảm bảo sự chồng chéo tích cực, trong khi giới hạn dưới ngăn không cho một đĩa nằm hoàn toàn bên trong đĩa kia. Những bất đẳng thức chặt chẽ này đặc biệt hữu ích vì chúng đảm bảo rằng bán kính cuối cùng là dương và hai tâm khác nhau. 

Giới hạn tọa độ chỉ là ([-1000,1000]), do đó không có kích thước đầu vào tổ hợp ở đây. Thách thức là độ chính xác về mặt hình học chứ không phải thời gian chạy. Một giải pháp thực hiện số lượng phép tính dấu phẩy động không đổi dễ dàng nằm trong giới hạn 2 giây và 256 MB, trong khi việc tìm kiếm bằng số hoặc liệt kê dày đặc là không cần thiết. 

Việc triển khai bất cẩn có thể thất bại khi các vòng tròn có bán kính bằng nhau nhưng không tập trung vào đường ngang. Ví dụ,```
0 0 5
3 4 5
```có (d=5), nên đáp án là đường tròn bán kính (2,5) có tâm tại ((1,5,2)). Một giải pháp chỉ thay đổi tọa độ (x) hoặc giả sử rằng các tâm luôn nằm trên trục (x), sẽ tạo ra tâm sai. 

Bán kính không bằng nhau là một nguồn sai lầm phổ biến khác. Coi như```
0 0 5
6 0 9
```Ở đây (d=6) và câu trả lời được tập trung tại ((1,0)) với bán kính (4). Trung tâm không phải là trung điểm của hai trung tâm ban đầu. Sử dụng điểm giữa một cách mù quáng sẽ cho bán kính (3), mặc dù hình tròn nhỏ hơn vẫn còn chỗ trống ở một bên. 

Trường hợp gần tiếp tuyến cũng có ý nghĩa về mặt số học. Ví dụ,```
-1000 0 1000
999 0 1000
```có (d=1999), nên đáp án có bán kính (0,5) và tâm ((-0,5,0)). Bán kính yêu cầu nhỏ ngay cả khi giá trị đầu vào lớn, do đó, các phép tính phải được thực hiện ở dạng dấu phẩy động và được in với đủ chữ số. 

## Phương pháp tiếp cận 

Một phương pháp vũ phu theo nghĩa đen sẽ thử các vị trí có thể có ở trung tâm và giữ vòng tròn lớn nhất còn lại bên trong cả hai đĩa. Đối với tâm ứng cử viên (P), bán kính lớn nhất được phép bởi vòng tròn đầu tiên là (r_1-|PO_1|) và bán kính lớn nhất được phép bởi vòng tròn thứ hai là (r_2-|PO_2|). Vì vậy bán kính ứng cử viên là tối thiểu của họ. 

Vấn đề là trung tâm là một điểm liên tục nên lực lượng vũ phu cần có sự rời rạc hóa. Nếu chúng tôi cố gắng kiểm tra mọi tọa độ trên một lưới có khoảng cách (10^{-9}) trên phạm vi tọa độ có thể có ([-2000,2000]), thì sẽ có (4\cdot10^{12}) vị trí dọc theo mỗi trục hoặc 

[ 
(4\cdot10^{12})^2=1.6\cdot10^{25} 
] 

trung tâm ứng viên. Điều đó không chỉ là quá chậm trong 2 giây mà còn là một cách khó để đảm bảo độ chính xác được yêu cầu. 

Một phép tìm kiếm số phức tạp hơn có thể giảm đáng kể công việc, nhưng hình học cho chúng ta một nghiệm chính xác theo thời gian không đổi. Phương pháp vũ phu hoạt động vì tính hợp lệ của trung tâm ứng viên có thể được kiểm tra trực tiếp. Quan sát quan trọng là trung tâm tốt nhất hoàn toàn không cần tìm kiếm hai chiều. 

Lấy bất kỳ tâm ứng cử viên (P) nào bên trong ống kính. Chiếu (P) lên đường thẳng (O_1O_2), thu được (Q). Phép chiếu không thể tăng khoảng cách lên (O_1) hoặc (O_2). Do đó, 

[ 
r_1-|QO_1|\ge r_1-|PO_1| 
] 

và 

[ 
r_2-|QO_2|\ge r_2-|PO_2|. 
] 

Vì vậy, việc di chuyển tâm lên đường nối hai tâm ban đầu không bao giờ làm cho bán kính nội tiếp có thể nhỏ hơn. Chúng tôi có thể hạn chế toàn bộ tối ưu hóa ở một chiều. 

Bây giờ đặt tâm ứng cử viên (P) giữa (O_1) và (O_2). Nếu (t=|O_1P|), thì (|PO_2|=d-t). Hai vòng tròn cho phép bán kính 

[ 
r_1-t 
] 

và 

[ 
r_2-(d-t). 
] 

Cực đại của cực tiểu của chúng xảy ra chính xác khi hai đại lượng này bằng nhau. Ngược lại, nếu một nhỏ hơn, chúng ta có thể di chuyển (P) một chút về phía vòng tròn tương ứng và tăng số lượng nhỏ hơn. 

Giải quyết 

[ 
r_1-t=r_2-d+t 
] 

cho 

[ 
t=\frac{d+r_1-r_2}{2}. 
] 

Việc thay thế biểu thức này vào biểu thức bán kính sẽ cho 

[ 
R=\frac{r_1+r_2-d}{2}. 
] 

Khi đã biết (t), tâm chỉ đơn giản là điểm ở khoảng cách (t) từ (O_1) theo hướng (O_2). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(G^2)) cho lưới tọa độ (G\times G) | (O(1)) | Quá chậm và không thể đảm bảo độ chính xác (10^{-9}) một cách tự nhiên | 
| Tối ưu | (O(1)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai tâm đường tròn và bán kính của chúng. Tính toán 

[ 
dx=x_2-x_1,\qquad dy=y_2-y_1 
] 

và 

[ 
d=\sqrt{dx^2+dy^2}. 
] 

Việc đảm bảo hai điểm giao nhau có nghĩa là (d>0), nên phép chia cho (d) là an toàn. 
2. Tính bán kính hình tròn lớn nhất là 

[ 
R=\frac{r_1+r_2-d}{2}. 
] 

Điều này xuất phát từ việc làm cho khoảng cách từ tâm mới đến hai chu vi ban đầu bằng cùng một bán kính. 
3. Tính khoảng cách từ (O_1) đến tâm mới: 

[ 
t=\frac{d+r_1-r_2}{2}. 
] 

Đây là điểm duy nhất trên đoạn (O_1O_2) mà cả hai đường tròn ban đầu đều để lại chính xác (R) đơn vị khe hở hướng tâm. 
4. Chuyển khoảng cách đó thành chuyển vị tọa độ. Vectơ đơn vị từ (O_1) tới (O_2) là 

[ 
\left(\frac{dx}{d},\frac{dy}{d}\right). 
] 

Vì vậy trung tâm mới là 

[ 
x=x_1+\frac{dx}{d}t, 
\qquad 
y=y_1+\frac{dy}{d}t. 
] 
5. In (x), (y), (R) có nhiều chữ số. Mười lăm chữ số sau dấu thập phân mang lại độ chính xác cao hơn đáng kể so với yêu cầu (10^{-9}).

Bất biến chính là tâm được chọn nằm trên (O_1O_2) và có khoảng cách hoàn toàn giống nhau từ cả hai ranh giới đường tròn. Bất kỳ vòng tròn hợp lệ nào được căn giữa ở nơi khác đều có thể được chiếu lên đường này mà không làm giảm bán kính có thể có của nó. Dọc theo đường thẳng, bán kính khả dụng của vòng tròn thứ nhất giảm khi tâm di chuyển ra xa (O_1), trong khi bán kính khả dụng của vòng tròn thứ hai tăng lên. Mức tối thiểu của chúng được tối đa hóa chính xác tại điểm giao nhau của chúng. Như vậy vòng tròn tính toán vừa khả thi vừa tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    x1, y1, r1 = map(int, input().split())
    x2, y2, r2 = map(int, input().split())

    dx = x2 - x1
    dy = y2 - y1

    d = (dx * dx + dy * dy) ** 0.5

    radius = (r1 + r2 - d) / 2.0
    dist_from_first = (d + r1 - r2) / 2.0

    x = x1 + dx * dist_from_first / d
    y = y1 + dy * dist_from_first / d

    print(f"{x:.15f} {y:.15f} {radius:.15f}")

if __name__ == "__main__":
    solve()
```Hai dòng đầu tiên của`solve`đọc hai vòng tròn chính xác như đã cho. Tất cả các giá trị đầu vào là số nguyên, vì vậy`dx * dx + dy * dy`cũng được tính toán chính xác trước khi lấy căn bậc hai. 

giá trị`d`là khoảng cách giữa các tâm ban đầu. Vì các đường tròn có đúng hai điểm chung nên tâm không thể trùng nhau, nên`d`là hoàn toàn tích cực. Không cần phải kiểm tra epsilon nhân tạo hoặc trường hợp đặc biệt đối với các tâm trùng khớp. 

biểu hiện`(r1 + r2 - d) / 2`là bán kính cuối cùng. Điều kiện giao chặt chẽ đảm bảo rằng tử số của nó là dương. 

Biến`dist_from_first`là khoảng cách từ tâm đầu tiên đến tâm đáp án. Nhân vectơ chuẩn hóa`(dx / d, dy / d)`bằng khoảng cách này sẽ đặt câu trả lời vào đúng vị trí dọc theo đường nối giữa các trung tâm ban đầu. 

Không thể tràn số nguyên trong Python và ngay cả trong các ngôn ngữ có chiều rộng cố định, sự khác biệt tọa độ bình phương ở đây rất nhỏ so với giới hạn số nguyên 64 bit. Các phép tính cuối cùng sử dụng dấu phẩy động vì câu trả lời có thể không hợp lý. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
0 0 5
6 0 5
```Các giá trị trung gian quan trọng là: 

| Biến | Giá trị | 
| --- | --- | 
| (dx) | (6) | 
| (dy) | (0) | 
| (d) | (6) | 
| (R) | (2) | 
| (t) | (3) | 
| (x) | (3) | 
| (y) | (0) | 

Vì bán kính bằng nhau nên giao điểm của hai hàm bán kính sẵn có là trung điểm của các tâm. Vòng tròn cuối cùng có tâm tại ((3,0)) với bán kính (2), khớp với đầu ra mẫu. 

### Mẫu 2 

Đầu vào là```
-12 34 56
78 -90 123
```Các tính toán trung gian xấp xỉ: 

| Biến | Giá trị | 
| --- | --- | 
| (dx) | (90) | 
| (dy) | (-124) | 
| (d) | (153.21814\ldots) | 
| (R) | (12.8906010988\ldots) | 
| (t) | (43.1093989012\ldots) | 
| (x) | (13.3222578219\ldots) | 
| (y) | (-0.8884441101\ldots) | 

Ở đây bán kính khác nhau nên trung tâm câu trả lời không phải là điểm giữa. Tâm mới gần với hình tròn đầu tiên hơn vì bán kính của nó nhỏ hơn. Các giá trị được tính toán khớp với mẫu thứ hai trong phạm vi dung sai dấu phẩy động được yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(1)) | Một số lượng không đổi các phép tính số học và một căn bậc hai được thực hiện | 
| Không gian | (O(1)) | Chỉ một số biến vô hướng cố định được lưu trữ | 

Đầu vào chỉ chứa hai vòng tròn, do đó không phụ thuộc vào kích thước đầu vào như (N). Giải pháp thực hiện một số phép tính số học và sử dụng bộ nhớ không đổi, để lại một khoảng cách rất lớn dưới giới hạn 2 giây và 256 MB. 

## Trường hợp thử nghiệm 

Các xác nhận bên dưới so sánh các giá trị dấu phẩy động với dung sai thay vì so sánh các chuỗi được định dạng. Điều đó phản ánh điều kiện đánh giá thực tế, trong đó nhiều cách biểu diễn thập phân khác nhau có thể đúng.```python
import sys
import io
import math

def solve():
    input = sys.stdin.readline

    x1, y1, r1 = map(int, input().split())
    x2, y2, r2 = map(int, input().split())

    dx = x2 - x1
    dy = y2 - y1
    d = (dx * dx + dy * dy) ** 0.5

    radius = (r1 + r2 - d) / 2.0
    t = (d + r1 - r2) / 2.0

    x = x1 + dx * t / d
    y = y1 + dy * t / d

    print(f"{x:.15f} {y:.15f} {radius:.15f}")

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

def check(inp: str, expected):
    out = run(inp).split()
    got = list(map(float, out))

    assert len(got) == 3
    for a, b in zip(got, expected):
        assert math.isclose(a, b, rel_tol=1e-12, abs_tol=1e-12), (
            f"got {got}, expected {expected}"
        )

# Provided samples
check(
    """0 0 5
6 0 5
""",
    (3.0, 0.0, 2.0),
)

check(
    """-12 34 56
78 -90 123
""",
    (13.322257821855908, -0.888444110112585, 12.890601098820779),
)

# Minimum radii, equal circles, simple horizontal placement
check(
    """0 0 1
1 0 1
""",
    (0.5, 0.0, 0.5),
)

# Equal circles with a non-horizontal center line
check(
    """0 0 5
3 4 5
""",
    (1.5, 2.0, 2.5),
)

# Unequal radii, catches the incorrect-midpoint solution
check(
    """0 0 5
6 0 9
""",
    (1.0, 0.0, 4.0),
)

# Maximum coordinate values while still having two intersections
check(
    """-1000 0 1000
999 0 1000
""",
    (-0.5, 0.0, 0.5),
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 1`/`1 0 1`|`(0.5, 0, 0.5)`| Bán kính tối thiểu và hình tròn bằng nhau | 
|`0 0 5`/`3 4 5`|`(1.5, 2, 2.5)`| Đường tâm không nằm ngang | 
|`0 0 5`/`6 0 9`|`(1, 0, 4)`| Bán kính không bằng nhau và tâm không ở giữa | 
|`-1000 0 1000`/`999 0 1000`|`(-0.5, 0, 0.5)`| Tọa độ biên và đường tròn gần tiếp tuyến | 

## Vỏ cạnh 

Đối với trường hợp kích thước tối thiểu```
0 0 1
1 0 1
```chúng tôi nhận được (d=1). Bán kính là 

[ 
R=\frac{1+1-1}{2}=0,5, 
] 

và khoảng cách từ tâm thứ nhất cũng là (0,5). Hướng là ((1,0)), nên câu trả lời chính xác là ((0,5,0,0,5)). Thuật toán không bao giờ cần một nhánh đặc biệt cho bán kính (1). 

Đối với trường hợp không nằm ngang```
0 0 5
3 4 5
```khoảng cách trung tâm là (5). Bán kính bằng nhau cho 

[ 
R=\frac{5+5-5}{2}=2,5 
] 

và 

[ 
t=\frac{5+5-5}{2}=2,5. 
] 

Đơn vị hướng từ tâm thứ nhất đến tâm thứ hai là ((3/5,4/5)) nên đáp án là tâm 

[ 
(0,0)+2.5\left(\frac35,\frac45\right)=(1.5,2). 
] 

Điều này phát hiện các triển khai xử lý hình học không chính xác dưới dạng một chiều dọc theo trục (x). 

Đối với bán kính không bằng nhau,```
0 0 5
6 0 9
```chúng ta có (d=6). Bán kính là 

[ 
R=\frac{5+9-6}{2}=4, 
] 

trong khi 

[ 
t=\frac{6+5-9}{2}=1. 
] 

Do đó tâm là ((1,0)). Hai khoảng trống đều là (4): 

[ 
5-1=4 
] 

và 

[ 
9-(6-1)=4. 
] 

Sự bằng nhau của hai đại lượng này chính là điều kiện tối ưu. 

Đối với trường hợp ranh giới gần tiếp tuyến,```
-1000 0 1000
999 0 1000
```các trung tâm cách nhau (1999) các đơn vị. Bán kính cuối cùng là 

[ 
R=\frac{1000+1000-1999}{2}=0,5. 
] 

Trung tâm câu trả lời nằm ở giữa hai trung tâm ban đầu, tại 

[ 
\frac{-1000+999}{2}=-0,5. 
] 

Vì vậy, đầu ra là```
-0.500000000000000 0.000000000000000 0.500000000000000
```đến sai số cho phép. Trường hợp này chứng minh tại sao việc triển khai không được giả định rằng bán kính câu trả lời được tách biệt một cách thoải mái với 0. Đảm bảo đầu vào cung cấp bán kính dương, nhưng nó có thể nhỏ tùy ý so với độ lớn tọa độ và bán kính.
