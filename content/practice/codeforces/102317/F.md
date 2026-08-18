---
title: "CF 102317F - Chấm chữ i và gạch chéo chữ T"
description: "Nhiệm vụ là hình học. Mỗi trường hợp kiểm thử cho một tập hợp các điểm phân biệt trên mặt phẳng và chúng ta phải đếm xem có bao nhiêu nhóm bốn điểm khác nhau có thể được sắp xếp thành chữ T in hoa."
date: "2026-08-17T03:46:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102317
codeforces_index: "F"
codeforces_contest_name: "UCF Locals 2016"
rating: 0
weight: 102317
solve_time_s: 80
verified: true
draft: false
---

[CF 102317F - Chấm chữ i và gạch chéo chữ T](https://codeforces.com/problemset/problem/102317/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là hình học. Mỗi trường hợp kiểm thử đưa ra một tập hợp các điểm phân biệt trên mặt phẳng và chúng ta phải đếm xem có bao nhiêu nhóm bốn điểm khác nhau có thể được sắp xếp thành chữ hoa`T`. 

Cho bốn điểm`A`,`M`,`B`, Và`C`, phần nằm ngang của`T`là phân khúc`AB`, với`M`chính xác ở trung điểm của nó. Điểm còn lại`C`là điểm cuối của thân cây. Thân cây phải có cùng chiều dài với`AB`, và nó phải vuông góc với`AB`Tại`M`. Tương đương, nếu`A`Và`B`thì đã biết`M`bị ép buộc và chỉ có hai địa điểm khả thi cho`C`. 

Đầu vào chứa tối đa 100 trường hợp thử nghiệm. Mỗi bộ chứa từ 4 đến 50 điểm riêng biệt và mọi tọa độ đều nằm giữa`-1000`Và`1000`. Giới hạn thời gian chính thức là 1 giây và giới hạn bộ nhớ là 256 MB. 

Giới hạn nhỏ 50 điểm đủ lớn để loại trừ việc liệt kê bốn điểm đơn giản khi có thể có 100 trường hợp thử nghiệm. Việc liệt kê mọi lựa chọn có thứ tự của bốn điểm phân biệt cần`50 * 49 * 48 * 47 = 5,527,200`lựa chọn cho một trường hợp thử nghiệm có kích thước tối đa hoặc lên tới 552.720.000 lựa chọn trên 100 trường hợp như vậy. Một nghiệm gần với thời gian bậc ba thì thoải mái hơn, trong khi nghiệm bậc hai thậm chí còn tốt hơn. 

So sánh dấu phẩy động là vấn đề số chính. Câu lệnh xác định hai giá trị bằng nhau khi hiệu của chúng lớn nhất`10^-6`. So sánh tọa độ với`==`do đó có thể từ chối một điểm hợp lệ vì số học như`(x1 + x2) / 2`có thể không được đại diện chính xác. 

Ví dụ, bốn điểm```
(0, 0)
(2, 0)
(1, 0)
(1, 2)
```hình thành một`T`, vậy câu trả lời là`1`. Việc triển khai tính điểm giữa và so sánh tọa độ dấu phẩy động với đẳng thức chính xác đang dựa vào một thuộc tính mà vấn đề không đảm bảo. 

Trường hợp cạnh thứ hai cũng tương tự`T`có thể được mô tả với`A`Và`B`trao đổi. Vì```
(0, 0)
(2, 0)
(1, 0)
(1, 2)
```các cặp`(A,B)`Và`(B,A)`mô tả cùng một thanh ngang. Đếm các cặp có thứ tự sẽ tạo ra`2`thay vì câu trả lời đúng`1`. 

Trường hợp cạnh thứ ba xảy ra khi một thanh ngang ứng cử viên chỉ có một trong hai điểm cuối thân có thể có. Vì```
(0, 0)
(2, 0)
(1, 0)
(1, 2)
(5, 5)
```vẫn còn chính xác một`T`. Chúng ta phải kiểm tra cả hai hướng vuông góc nhưng chỉ đếm những điểm thực sự tồn tại. 

Tuyên bố chính thức của UCF đưa ra định nghĩa hình học tương tự và`10^-6`khoan dung bình đẳng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là chọn bốn điểm riêng biệt và kiểm tra mọi khả năng gán các điểm đó cho`A`,`M`,`B`, Và`C`. Việc kiểm tra diễn ra trực tiếp từ định nghĩa: xác minh rằng`M`là trung điểm của`AB`, cái đó`CM`có cùng độ dài với`AB`và hai đoạn gặp nhau tại`M`là vuông góc. 

Cách tiếp cận đó đúng vì mọi giá trị hợp lệ`T`bao gồm bốn điểm, do đó, việc kiểm tra mọi nhiệm vụ có thể thực hiện cuối cùng sẽ xem xét vai trò chính xác của nó. Vấn đề là số lượng lựa chọn. Với 50 điểm, ngay cả việc liệt kê các bộ tứ theo thứ tự cũng cần 5.527.200 lựa chọn cho mỗi trường hợp thử nghiệm và 100 trường hợp có kích thước tối đa có thể đạt tới 552.720.000 lựa chọn trước khi thực hiện kiểm tra hình học. 

Cấu trúc hình học cho chúng ta cách tổ chức tìm kiếm tốt hơn nhiều. Thay vì chọn bốn điểm, hãy chọn hai điểm cuối`A`Và`B`của xà ngang. Một khi hai điểm này được cố định thì không còn tự do ở hai vị trí còn lại. 

Trung điểm là`M = ((Ax + Bx) / 2, (Ay + By) / 2)`. 

Cho vectơ từ`A`ĐẾN`B`là`(dx, dy)`. Một vectơ vuông góc có cùng độ dài là`(-dy, dx)`hoặc`(dy, -dx)`. Do đó, điểm cuối gốc duy nhất có thể là`C1 = M + (-dy, dx)`Và`C2 = M + (dy, -dx)`. 

Vì vậy, mọi cặp điểm không có thứ tự đều tạo ra tối đa hai điểm thứ tư ứng cử viên. Chúng ta chỉ cần kiểm tra xem các tọa độ ứng cử viên đó có xuất hiện trong đầu vào hay không. 

Với tối đa 50 điểm, ngay cả việc thực hiện đơn giản là quét tất cả các điểm để xác định từng ứng viên cũng chỉ mất`O(p^3)`thời gian. có`O(p^2)`sự lựa chọn của`A,B`, hai địa điểm ứng cử viên và một`O(p)`quét cho từng ứng viên. Điều này dễ dàng đủ nhỏ cho các giới hạn nhất định. 

Phương pháp brute-force hoạt động vì mọi sự phân công vai trò có thể đều được kiểm tra, nhưng không thành công vì nó khám phá các kết hợp mà hình dạng của chúng có thể được xác định ngay lập tức. Quan sát cho thấy các điểm cuối của thanh ngang xác định duy nhất điểm giữa và cả hai thân có thể sẽ loại bỏ toàn bộ chiều tìm kiếm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(p^4) | O(1) | Quá chậm trong trường hợp xấu nhất | 
| Tối ưu | O(p^3) | O(p) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`p`các điểm của test case hiện tại và lưu trữ tọa độ của chúng. 
2. Liệt kê từng cặp điểm không có thứ tự`A`Và`B`. Chỉ sử dụng các cặp có`i < j`là cần thiết vì việc trao đổi điểm cuối của thanh ngang không tạo ra sự khác biệt`T`. 
3. Tính điểm giữa`M`của`A`Và`B`. Điểm giữa bị ép buộc bởi điều kiện đầu tiên của định nghĩa, do đó không cần chọn điểm đầu vào nào khác cho`M`một cách độc lập. 
4. Tính toán`dx = Bx - Ax`Và`dy = By - Ay`. Vectơ`(dx, dy)`mô tả thanh ngang. 
5. Xây dựng hai điểm cuối gốc có thể. Một vectơ vuông góc với`(dx, dy)`với độ dài chính xác như nhau là`(-dy, dx)`, và điều ngược lại của nó là`(dy, -dx)`. Thêm một trong hai vectơ vào`M`đưa ra một vị trí có thể cho`C`. 
6. Đối với mỗi ứng viên`C`, quét các điểm đầu vào và kiểm tra xem cả hai tọa độ của nó có nằm trong`10^-6`của tọa độ ứng cử viên. Nếu một điểm phù hợp, hãy tăng câu trả lời. 
7. Xử lý từng cặp và cả hai hướng vuông góc, sau đó in kết quả đếm theo yêu cầu`Set #k: answer`định dạng theo sau là một dòng trống. 

Việc so sánh số được áp dụng độc lập cho hai tọa độ. Một điểm được coi là có mặt khi cả hai độ lệch tọa độ lớn nhất`10^-6`, phù hợp với quy tắc đẳng thức từ câu lệnh. 

### Tại sao nó hoạt động 

Đối với mọi hợp lệ`T`, hai điểm cuối thanh ngang của nó là một cặp không có thứ tự`A,B`trong đầu vào. Khi thuật toán đạt đến cặp đó, phép tính điểm giữa của nó sẽ tạo ra chính xác điểm cần thiết`M`. Vectơ`AB`xác định chính xác hai vectơ vuông góc có cùng độ dài bằng`AB`, do đó hai ứng cử viên được tạo chính xác là hai vị trí có thể có về mặt hình học cho`C`. 

Như vậy hợp lệ`C`của`T`phải được tìm thấy bằng cách quét. Ngược lại, bất cứ khi nào một ứng cử viên được tạo khớp với một điểm đầu vào, điều kiện trung điểm, điều kiện độ dài bằng nhau và điều kiện vuông góc đều được giữ nguyên theo cách xây dựng, do đó bốn điểm đó tạo thành một điểm hợp lệ.`T`. 

sử dụng`i < j`có nghĩa là mỗi thanh ngang được xem xét một lần. Đối với một thanh ngang cố định, mỗi điểm cuối gốc có thể được xem xét một lần, do đó không có nhóm hợp lệ nào được tính nhiều hơn một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

EPS = 1e-6

def same_point(x1, y1, x2, y2):
    return abs(x1 - x2) <= EPS and abs(y1 - y2) <= EPS

def solve_case(points):
    n = len(points)
    ans = 0

    for i in range(n):
        ax, ay = points[i]

        for j in range(i + 1, n):
            bx, by = points[j]

            dx = bx - ax
            dy = by - ay

            mx = (ax + bx) / 2.0
            my = (ay + by) / 2.0

            # Two perpendicular vectors having the same length as AB.
            candidates = (
                (mx - dy, my + dx),
                (mx + dy, my - dx),
            )

            for cx, cy in candidates:
                for px, py in points:
                    if same_point(cx, cy, px, py):
                        ans += 1
                        break

    return ans

def main():
    t = int(input())

    out = []

    for case_no in range(1, t + 1):
        p = int(input())
        points = [tuple(map(float, input().split())) for _ in range(p)]

        ans = solve_case(points)

        out.append(f"Set #{case_no}: {ans}")
        out.append("")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```các`solve_case`chức năng thực hiện tìm kiếm dựa trên cặp từ hướng dẫn. Hai vòng bên ngoài chọn`A`Và`B`với`i < j`, do đó thanh ngang không bao giờ bị trùng lặp. 

Điểm giữa sử dụng phép chia cho`2.0`vì tọa độ là số thực. Hai điểm ứng cử viên sau đó được lấy trực tiếp từ phép biến đổi vuông góc. Nếu như`(dx, dy)`là vectơ thanh ngang,`(-dy, dx)`quay nó 90 độ mà không thay đổi chiều dài của nó. 

Quá trình quét bên trong có chủ ý sử dụng so sánh epsilon thay vì từ điển được khóa bằng tọa độ dấu phẩy động thô. Một từ điển dựa trên các giá trị dấu phẩy động chính xác sẽ làm cho kết quả phụ thuộc vào chi tiết biểu diễn. Chỉ với 50 điểm, việc quét đơn giản và đủ nhanh. 

các`break`sau khi tìm được điểm phù hợp cũng là điều cần thiết. Các điểm đầu vào được đảm bảo là khác biệt nên một ứng cử viên có thể tương ứng với nhiều nhất một điểm đầu vào. Nếu không có`break`, tọa độ trùng lặp trong tương lai sẽ tăng số lượng không chính xác, mặc dù câu lệnh loại trừ các bản sao. 

Số nguyên Python không bị tràn và câu trả lời nhiều nhất là`2 * C(50, 2) = 2450`, do đó không cần xử lý số nguyên đặc biệt. 

Dòng trống bắt buộc sau mỗi câu trả lời được tạo bằng cách thêm một chuỗi trống sau mỗi trường hợp. Tuyên bố chính thức chỉ định định dạng đầu ra này. 

## Ví dụ đã hoạt động 

Trang vấn đề Codeforces liên kết đến bộ vấn đề UCF ban đầu, có câu lệnh được lập chỉ mục cung cấp định nghĩa hình học hoàn chỉnh và đặc tả đầu vào/đầu ra. Văn bản bài toán có thể truy cập không hiển thị các trường hợp mẫu trong phần trích xuất được lập chỉ mục của nó, vì vậy sau đây là hai dấu vết cụ thể được xây dựng trực tiếp từ định nghĩa thay vì được trình bày dưới dạng mẫu chính thức. 

### Mẫu 1 

Hãy xem xét:```
1
4
0 0
2 0
1 0
1 2
```Sản lượng dự kiến ​​​​là:```
Set #1: 1
```Đối với cặp`(0,0)`Và`(2,0)`, vectơ thanh ngang là`(2,0)`. Trung điểm của nó là`(1,0)`và hai điểm cuối gốc ứng cử viên là`(1,2)`Và`(1,-2)`. Chỉ một`(1,2)`tồn tại. 

| A | B | M | Ứng viên C | Tìm thấy | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
|`(0,0)`|`(2,0)`|`(1,0)`|`(1,2)`| vâng | 1 | 
|`(0,0)`|`(2,0)`|`(1,0)`|`(1,-2)`| không | 1 | 
|`(0,0)`|`(1,0)`|`(0.5,0)`|`(0.5,1)`| không | 1 | 
|`(0,0)`|`(1,2)`|`(0.5,1)`|`(-1.5,1.5)`| không | 1 | 
|`(2,0)`|`(1,0)`|`(1.5,0)`|`(1.5,-1)`| không | 1 | 
| cặp còn lại | | | | không | 1 | 

Phần quan trọng của dấu vết là giá trị hợp lệ`T`được tìm thấy chỉ từ cặp thanh ngang của nó. Các cặp khác không cần bất kỳ sự đối xử đặc biệt nào và chỉ cần tạo ra những ứng cử viên vắng mặt. 

### Mẫu 2 

Hãy xem xét:```
1
5
0 0
2 0
1 0
1 2
1 -2
```Sản lượng dự kiến ​​​​là:```
Set #1: 2
```Hiện tại, cùng một thanh ngang có cả hai điểm cuối gốc có thể có. 

| A | B | M | Ứng viên C | Tìm thấy | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
|`(0,0)`|`(2,0)`|`(1,0)`|`(1,2)`| vâng | 1 | 
|`(0,0)`|`(2,0)`|`(1,0)`|`(1,-2)`| vâng | 2 | 
|`(0,0)`|`(1,0)`|`(0.5,0)`|`(0.5,1)`| không | 2 | 
|`(0,0)`|`(1,2)`|`(0.5,1)`|`(-1.5,1.5)`| không | 2 | 
|`(2,0)`|`(1,0)`|`(1.5,0)`|`(1.5,1)`| không | 2 | 
| cặp còn lại | | | | không | 2 | 

Ví dụ này thực hiện phần hai hướng của công trình. Giải pháp chỉ kiểm tra một hướng vuông góc sẽ trả về sai`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(p^3) | O(p²) cặp thanh ngang không có thứ tự, hai ứng cử viên cho mỗi cặp và quét điểm O(p) | 
| Không gian | O(p) | Tập hợp điểm đầu vào được lưu trong bộ nhớ | 

Với`p <= 50`, trường hợp xấu nhất chỉ là về`50² * 50 = 125,000`so sánh điểm trên mỗi trường hợp thử nghiệm với các hệ số không đổi hoặc khoảng 12,5 triệu so sánh trên 100 trường hợp có kích thước tối đa. Con số này nhỏ hơn một cách thoải mái so với hàng trăm triệu bài tập bốn điểm được xem xét bằng vũ lực. Các giới hạn chính thức đủ nhỏ để việc quét dấu phẩy động đơn giản được ưu tiên hơn là đưa ra cấu trúc tra cứu không gian phức tạp hơn. 

## Trường hợp thử nghiệm 

Bộ dây thử nghiệm sau đây sử dụng cùng một`solve_case`logic như sự trình bày. Các trường hợp tùy chỉnh bao gồm số điểm tối thiểu, cả hướng gốc có thể có, tọa độ dấu phẩy động và một tập hợp lớn hơn chứa một số điểm không liên quan.```python
import sys
import io

EPS = 1e-6

def same_point(x1, y1, x2, y2):
    return abs(x1 - x2) <= EPS and abs(y1 - y2) <= EPS

def solve_case(points):
    n = len(points)
    ans = 0

    for i in range(n):
        ax, ay = points[i]

        for j in range(i + 1, n):
            bx, by = points[j]

            dx = bx - ax
            dy = by - ay

            mx = (ax + bx) / 2.0
            my = (ay + by) / 2.0

            candidates = (
                (mx - dy, my + dx),
                (mx + dy, my - dx),
            )

            for cx, cy in candidates:
                for px, py in points:
                    if same_point(cx, cy, px, py):
                        ans += 1
                        break

    return ans

def solution(data):
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(data)

    t = int(input())
    out = []

    for case_no in range(1, t + 1):
        p = int(input())
        points = [tuple(map(float, input().split())) for _ in range(p)]
        out.append(f"Set #{case_no}: {solve_case(points)}")
        out.append("")

    sys.stdin = old_stdin
    return "\n".join(out)

def run(inp: str) -> str:
    return solution(inp)

# Constructed sample 1
assert run("""1
4
0 0
2 0
1 0
1 2
""") == "Set #1: 1\n", "one T"

# Constructed sample 2
assert run("""1
5
0 0
2 0
1 0
1 2
1 -2
""") == "Set #1: 2\n", "both stem directions"

# Minimum-size case with no T
assert run("""1
4
0 0
1 0
0 1
1 1
""") == "Set #1: 0\n", "minimum size, no valid T"

# All points form two T's around the same crossbar
assert run("""1
6
-1 0
1 0
0 0
0 2
0 -2
3 3
""") == "Set #1: 2\n", "unrelated point must not matter"

# Floating-point coordinates
assert run("""1
4
0.1 0.1
2.1 0.1
1.1 0.1
1.1 2.1
""") == "Set #1: 1\n", "floating-point midpoint"

# Several unrelated points and a diagonal crossbar
assert run("""1
7
0 0
2 2
1 1
-1 3
3 -1
10 10
-5 -4
""") == "Set #1: 2\n", "diagonal crossbar with both stems"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bốn điểm tạo thành một trục thẳng hàng`T`|`Set #1: 1`| Xây dựng cơ bản | 
| Năm điểm có cả hai thân vuông góc |`Set #1: 2`| Cả hai hướng ứng cử viên | 
| Bốn góc của một hình vuông đơn vị |`Set #1: 0`| Trường hợp kích thước tối thiểu không có giá trị`T`| 
| Có hiệu lực`T`cộng điểm không liên quan |`Set #1: 2`| Điểm không liên quan không ảnh hưởng đến việc tính điểm | 
| Tọa độ thập phân |`Set #1: 1`| Xử lý điểm giữa dấu phẩy động | 
| Thanh ngang chéo có điểm không liên quan |`Set #1: 2`| Logic xoay cho hình học không thẳng hàng | 

Trường hợp cuối cùng đặc biệt hữu ích vì giải pháp chỉ được viết cho các phân đoạn ngang hoặc dọc sẽ không thành công. Công thức vectơ vuông góc hoạt động giống hệt nhau cho mọi hướng. 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là tính trùng lặp từ việc trao đổi`A`Và`B`. Vì```
1
4
0 0
2 0
1 0
1 2
```cặp đôi`(0,0),(2,0)`sản xuất`(1,2)`như một điểm cuối gốc hợp lệ. Khi các vòng lặp sau này xem xét`(2,0),(0,0)`, họ không xử lý nó vì thuật toán yêu cầu`i < j`. Câu trả lời vẫn còn`1`, thay vì trở thành`2`. 

Trường hợp cạnh thứ hai là đẳng thức dấu phẩy động. Vì```
1
4
0.1 0.1
2.1 0.1
1.1 0.1
1.1 2.1
```trung điểm của hai điểm đầu tiên là`(1.1, 0.1)`. Thân ứng cử viên là`(1.1, 2.1)`, hiện có. Việc so sánh sử dụng dung sai tuyệt đối của`10^-6`trên cả hai tọa độ, do đó các lỗi biểu diễn nhỏ không làm thay đổi kết quả. Đầu ra là`Set #1: 1`. 

Trường hợp cạnh thứ ba là một thanh ngang có thể có hai thân hợp lệ. Vì```
1
5
0 0
2 0
1 0
1 2
1 -2
```trung điểm là`(1,0)`, trong khi hai vectơ vuông góc là`(0,2)`Và`(0,-2)`. Cả hai điểm được tạo đều có mặt, do đó thuật toán tăng câu trả lời hai lần và đưa ra kết quả`Set #1: 2`. 

Trường hợp cạnh thứ tư là một thanh ngang chéo. Coi như```
1
7
0 0
2 2
1 1
-1 3
3 -1
10 10
-5 -4
```Vectơ thanh ngang là`(2,2)`, trung điểm của nó là`(1,1)`và hai vectơ vuông góc có độ dài bằng nhau là`(-2,2)`Và`(2,-2)`. Các điểm cuối gốc tương ứng là`(-1,3)`Và`(3,-1)`, cả hai đều có trong đầu vào. Câu trả lời là`2`. Phép tính sử dụng phép quay vectơ thay vì giả định về tọa độ ngang hoặc dọc, do đó, lý do tương tự xử lý các hướng tùy ý.
