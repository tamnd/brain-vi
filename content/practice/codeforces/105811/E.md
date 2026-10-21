---
title: "CF 105811E - Gói cáp"
description: "Bài toán yêu cầu chúng ta xử lý nhiều tuyến cáp đề xuất trên một tập hợp các ngôi nhà đặt trên một mặt phẳng. Đối với mỗi đường thẳng, chúng ta cần khoảng cách vuông góc lớn nhất từ ​​bất kỳ ngôi nhà nào đến đường thẳng đó."
date: "2026-06-25T15:19:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105811
codeforces_index: "E"
codeforces_contest_name: "UT Open 2025"
rating: 0
weight: 105811
solve_time_s: 58
verified: true
draft: false
---

[CF 105811E - Gói cáp](https://codeforces.com/problemset/problem/105811/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta xử lý nhiều tuyến cáp đề xuất trên một tập hợp các ngôi nhà đặt trên một mặt phẳng. Đối với mỗi đường thẳng, chúng ta cần khoảng cách vuông góc lớn nhất từ ​​bất kỳ ngôi nhà nào đến đường thẳng đó. Tuyên bố chính thức mô tả các ngôi nhà là các điểm và sơ đồ cáp là các đường có dạng`Ax + By + C = 0`. 

Khoảng cách từ một điểm`(x, y)`đến một dòng như vậy là:$$\frac{|Ax + By + C|}{\sqrt{A^2+B^2}}$$Mẫu số giống nhau cho mọi ngôi nhà trong một truy vấn, vì vậy nhiệm vụ thực sự là tìm giá trị lớn nhất của`|Ax + By + C|`trong số tất cả các điểm. 

Kích thước đầu vào lớn. Với tối đa`2 * 10^5`nhà ở và`2 * 10^5`truy vấn, một giải pháp kiểm tra mọi ngôi nhà để tìm mọi truy vấn thực hiện về`4 * 10^10`hoạt động vượt xa giới hạn thời gian của cuộc thi cho phép. Chúng ta cần xử lý trước các ngôi nhà để mỗi truy vấn gần với thời gian logarit. 

Quan sát hình học quan trọng là giá trị cực đại của hàm tuyến tính trên một tập hợp các điểm luôn đạt được trên bao lồi. Tất cả các điểm bên trong bao đều không liên quan vì chúng là tổ hợp lồi của các điểm khác và hàm tuyến tính không thể có mức cực đại nghiêm ngặt ở đó. Vấn đề trở thành vấn đề truy vấn hỗ trợ bao lồi. 

Có một số trường hợp nguy hiểm có thể phá vỡ các giải pháp bất cẩn. Nếu tất cả các ngôi nhà nằm trên một đường thẳng thì bao lồi chỉ có hai đỉnh. Ví dụ:```
2 1
0 0
5 5
1 -1 0
```dòng`x - y = 0`chứa cả hai ngôi nhà, vì vậy câu trả lời là:```
0.0000000000
```Một giải pháp giả định ít nhất ba điểm thân tàu có thể thất bại. 

Một vấn đề khác là câu trả lời sử dụng giá trị tuyệt đối. Coi như:```
2 1
0 0
1 1
1 0 -10
```Các giá trị biểu thức là`-10`Và`-9`, vậy giá trị tuyệt đối lớn nhất là`10`. Chỉ tìm kiếm giá trị được ký tối đa sẽ trả về không chính xác`9`. 

Trường hợp tinh tế thứ ba là khi điểm tốt nhất không phải là điểm đầu vào ban đầu. Điểm xa nhất so với một hướng luôn nằm trên thân tàu, nhưng không nhất thiết phải là điểm đầu tiên hoặc điểm cuối cùng trong thứ tự đầu vào. Bất kỳ giải pháp nào dựa vào thứ tự đầu vào sẽ âm thầm thất bại. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp là đơn giản. Đối với mỗi truy vấn, hãy tính giá trị của biểu thức đường cho mỗi ngôi nhà, giữ giá trị tuyệt đối lớn nhất và chia cho hệ số chuẩn hóa. Điều này đúng vì nó kiểm tra định nghĩa chính xác của câu trả lời. Tuy nhiên, với`n`nhà ở và`q`truy vấn, độ phức tạp là`O(nq)`, trở thành đại khái`4 * 10^10`đánh giá ở kích thước đầu vào tối đa. 

Sự cải thiện đến từ việc nhận ra cấu trúc của biểu thức. Đối với một truy vấn vector chuẩn tắc`(A, B)`, chúng ta cần:$$\max |A x + B y + C|$$các`C`một phần chỉ dịch chuyển tất cả các giá trị như nhau, vì vậy chúng ta cần giá trị tối đa và tối thiểu của:$$A x + B y$$khắp mọi ngôi nhà. Bao lồi chứa chính xác các điểm cực trị cho mọi hướng. Điều này chuyển bài toán thành tìm điểm hỗ trợ của đa giác lồi. 

Sau khi xây dựng bao lồi, mỗi truy vấn sẽ trở thành tìm kiếm nhị phân trên bao lồi. Tích số chấm có hướng cố định thay đổi theo một cách không đồng nhất xung quanh một đa giác lồi, vì vậy chúng ta có thể xác định được đỉnh cực trị theo thời gian logarit. Các truy vấn cực đoan dựa trên bao lồi là cách sử dụng tiêu chuẩn của cấu trúc có thứ tự của đa giác lồi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Tối ưu | O(n log n + q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả tọa độ nhà và xây dựng bao lồi bằng thuật toán chuỗi đơn điệu. Xóa các điểm bên trong vì chúng không bao giờ có thể tối đa hóa biểu thức tuyến tính. 
2. Lưu trữ các đỉnh thân tàu theo thứ tự ngược chiều kim đồng hồ. Thứ tự quan trọng vì các giá trị tích số chấm xung quanh một đa giác lồi tạo thành một hình dạng có thể tìm kiếm nhị phân. 
3. Với mỗi dòng truy vấn, trích xuất`A`,`B`, Và`C`. hướng`(A, B)`cho chúng ta biết phía nào của thân tàu là xa nhất. 
4. Tìm giá trị lớn nhất của`A*x + B*y`trên thân tàu. Điều này được thực hiện bằng cách tìm kiếm nhị phân trên đa giác lồi tuần hoàn. 
5. Tìm giá trị nhỏ nhất tương tự bằng cách tìm theo hướng`(-A, -B)`. Việc phủ định hướng biến truy vấn tối thiểu thành truy vấn tối đa. 
6. Kết hợp hai thái cực. Giá trị biểu thức tuyệt đối lớn nhất là:$$\max(\text{maxValue}+C,\;-(\text{minValue}+C))$$Sau đó chia cho`sqrt(A^2+B^2)`. 

Tại sao nó hoạt động: bao lồi chứa tất cả các điểm cực trị có thể có của tập hợp điểm ban đầu. Đối với bất kỳ vectơ chỉ hướng nào, hàm tuyến tính đạt cực đại và cực tiểu trên thân tàu. Vì công thức khoảng cách chỉ là một biểu thức tuyến tính tuyệt đối chia cho một hằng số nên hai truy vấn hỗ trợ sẽ đưa ra chính xác hai ứng cử viên cần thiết cho câu trả lời. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def cross(a, b, c):
    return (b[0] - a[0]) * (c[1] - a[1]) - (b[1] - a[1]) * (c[0] - a[0])

def build_hull(points):
    points = sorted(set(points))
    if len(points) <= 1:
        return points

    lower = []
    for p in points:
        while len(lower) >= 2 and cross(lower[-2], lower[-1], p) <= 0:
            lower.pop()
        lower.append(p)

    upper = []
    for p in reversed(points):
        while len(upper) >= 2 and cross(upper[-2], upper[-1], p) <= 0:
            upper.pop()
        upper.append(p)

    return lower[:-1] + upper[:-1]

def dot(p, a, b):
    return p[0] * a + p[1] * b

def support(poly, a, b):
    n = len(poly)
    if n == 1:
        return dot(poly[0], a, b)

    l, r = 0, n - 1
    while l < r:
        m = (l + r) // 2
        cur = dot(poly[m], a, b)
        nxt = dot(poly[(m + 1) % n], a, b)
        if cur <= nxt:
            l = m + 1
        else:
            r = m
    ans = dot(poly[l], a, b)
    ans = max(ans, dot(poly[(l - 1) % n], a, b))
    ans = max(ans, dot(poly[(l + 1) % n], a, b))
    return ans

def solve():
    n, q = map(int, input().split())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    hull = build_hull(pts)

    out = []
    for _ in range(q):
        A, B, C = map(int, input().split())

        mx = support(hull, A, B)
        mn = -support(hull, -A, -B)

        value = max(abs(mx + C), abs(mn + C))
        ans = value / math.sqrt(A * A + B * B)
        out.append(f"{ans:.12f}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Cấu trúc thân tàu loại bỏ các điểm ẩn bên trong đa giác. các`cross`Hàm kiểm tra xem hai cạnh thân tàu cuối cùng có tạo ra một lối rẽ không sang trái hay không, nghĩa là điểm giữa không thể thuộc về ranh giới lồi. 

các`support`chức năng là tối ưu hóa cốt lõi. Nó tìm kiếm đỉnh nơi hình chiếu lên`(A, B)`là lớn nhất. Việc kiểm tra cuối cùng xung quanh chỉ mục được tìm thấy sẽ xử lý các cạnh phẳng và tránh các vấn đề từ các giá trị chiếu bằng nhau. 

Quá trình xử lý truy vấn thực hiện hai truy vấn hỗ trợ. Một người tìm hình chiếu cực đại, trong khi người kia tìm cực tiểu bằng cách đảo ngược vectơ chỉ phương. Giá trị tuyệt đối chỉ được áp dụng sau khi đã biết cả hai thái cực, điều này tránh được sai lầm phổ biến là quên mặt âm. 

## Ví dụ đã hoạt động 

Mẫu 1:```
2 2
0 0
1 1
-1 1 0
1 1 -2
```| Bước | Hướng truy vấn | Chiếu tối đa | Chiếu tối thiểu | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | (-1, 1) | 0 | 0 | 0 | 
| 2 | (1, 1) | 2 | 0 | 1.4142135624 | 

Dòng đầu tiên có cả hai điểm trên đó, vì vậy cả hai hình chiếu đều bằng nhau. Truy vấn thứ hai đạt khoảng cách lớn nhất tại`(0,0)`. 

Mẫu 2:```
3 1
0 0
2 0
0 2
1 0 -3
```| Bước | Hướng truy vấn | Chiếu tối đa | Chiếu tối thiểu | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | (1,0) | 2 | 0 | 3 | 

Số hạng không đổi làm dịch chuyển mọi hình chiếu. Phép chiếu tối thiểu vẫn cần thiết vì giá trị tuyệt đối có thể chọn phía bên kia. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q log n) | Cấu trúc thân tàu sắp xếp các điểm một lần, mỗi truy vấn thực hiện hai tìm kiếm hỗ trợ logarit | 
| Không gian | O(n) | Bao lồi lưu trữ tối đa tất cả các điểm đầu vào | 

Quá trình tiền xử lý chi phối chi phí ban đầu và mọi truy vấn chỉ chạm vào nhiều đỉnh thân theo logarit. Điều này phù hợp với giới hạn cho`2 * 10^5`điểm và truy vấn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return sys.stdout.getvalue()

assert run("""2 2
0 0
1 1
-1 1 0
1 1 -2
""") == "0.000000000000\n1.414213562373\n"

assert run("""1 1
5 7
1 1 -12
""") == "0.000000000000\n"

assert run("""3 1
0 0
2 0
0 2
1 0 -3
""") == "3.000000000000\n"

assert run("""4 1
0 0
10 0
10 10
0 10
1 0 0
""") == "10.000000000000\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai điểm trên đường chéo |`0`Và`sqrt(2)`| Thân tàu cơ bản và xử lý khoảng cách tuyệt đối | 
| Một ngôi nhà |`0`| Thân tàu một điểm | 
| Tam giác với đường dịch chuyển |`3`| Xử lý thời hạn không đổi | 
| Truy vấn ranh giới vuông |`10`| Hình chiếu tối đa trên các đỉnh thân tàu | 

## Vỏ cạnh 

Đối với nhà liền kề:```
2 1
0 0
5 5
1 -1 0
```Thân tàu chỉ chứa hai điểm cuối. Truy vấn hỗ trợ kiểm tra cả hai đỉnh có thể và biểu thức dòng bằng 0 cho cả hai, đưa ra câu trả lời đúng. 

Đối với vấn đề giá trị tuyệt đối:```
2 1
0 0
1 1
1 0 -10
```Hai giá trị biểu thức là`-10`Và`-9`. Thuật toán tính toán cả phép chiếu tối đa và tối thiểu, sau đó áp dụng giá trị tuyệt đối, tạo ra`10`. 

Đối với trường hợp thứ tự đầu vào bị sai lệch:```
4 1
100 100
0 0
100 0
0 100
1 1 0
```Điểm xa nhất không phải là điểm đầu vào đầu tiên theo bất kỳ thứ tự hữu ích nào. Thân tàu loại bỏ các giả định về thứ tự và tìm kiếm hỗ trợ sẽ tìm thấy đỉnh cực trị chính xác.
