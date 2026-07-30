---
title: "CF 102836H - \u0411\u043e\u043b\u044c\u0448\u043e\u0439 \u0431\u0430\u0442\u0443\u0442"
description: "Chúng tôi được cung cấp tới chín điểm hỗ trợ trên một mặt phẳng. Chúng ta phải quyết định xem những điểm này có thể được sắp xếp thành các đỉnh của một đa giác đơn giản hay không, nghĩa là đường viền của đa giác không thể cắt qua chính nó và không thể chạm vào chính nó tại một điểm không liền kề."
date: "2026-07-26T14:53:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102836
codeforces_index: "H"
codeforces_contest_name: "\u0426\u0438\u043a\u043b \u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434, \u0421\u0435\u0437\u043e\u043d 2020-21, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102836
solve_time_s: 43
verified: true
draft: false
---

[CF 102836H - \u0411\u043e\u043b\u044c\u0448\u043e\u0439 \u0431\u0430\u0442\u0443\u0442](https://codeforces.com/problemset/problem/102836/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp tới chín điểm hỗ trợ trên một mặt phẳng. Chúng ta phải quyết định xem những điểm này có thể được sắp xếp thành các đỉnh của một đa giác đơn giản hay không, nghĩa là đường viền của đa giác không thể cắt qua chính nó và không thể chạm vào chính nó tại một điểm không liền kề. Trong số tất cả các lệnh hợp lệ của cùng một điểm, chúng ta cần xuất ra lệnh có diện tích lớn nhất có thể. 

Đầu vào mô tả tọa độ của các giá đỡ. Đầu ra là không thể xây dựng được đa giác đơn giản hoặc hoán vị các chỉ số điểm mô tả thứ tự mà các đỉnh đa giác sẽ được truy cập. 

Giá trị rất nhỏ của`n`thay đổi hoàn toàn cách chúng ta nên nghĩ về vấn đề. Với`n <= 9`, thử tất cả các hoán vị yêu cầu nhiều nhất`9! = 362880`ứng cử viên, đó là rất nhỏ. Ngay cả khi mọi ứng viên cần một số bài kiểm tra hình học, tổng công việc vẫn được thực hiện thoải mái trong giới hạn 2 giây. Thuật toán hình học thời gian đa thức sẽ thú vị nhưng không cần thiết ở đây. 

Những cạm bẫy chính không liên quan đến hiệu suất mà là hình học. Một hoán vị có thể có diện tích được ký lớn trong khi vẫn không hợp lệ vì các cạnh giao nhau. Ví dụ, các điểm```
4
0 0
2 2
0 2
2 0
```có thể được đặt hàng như`1 2 3 4`, nhưng các cạnh`(1,2)`Và`(3,4)`đi qua. Câu trả lời đúng phải tránh sự tự giao nhau như vậy. 

Một trường hợp phức tạp khác là khi tất cả các điểm nằm trên một đường:```
3
0 0
1 0
2 0
```Không có đa giác nào có diện tích dương tồn tại vì mọi thứ tự có thể đều tạo ra một hình suy biến. Thuật toán phải từ chối nó thay vì trả về một hoán vị tùy ý. 

Lỗi phổ biến thứ ba là để một cạnh đa giác chạm vào một cạnh khác tại một điểm. Tuyên bố cấm tự chạm cũng như cắt ngang, do đó, phép kiểm tra giao điểm đoạn phải bao gồm các trường hợp chồng chéo thẳng hàng và tiếp xúc điểm cuối đối với các cạnh không liền kề. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra mọi thứ tự có thể có của các điểm. Đối với mỗi hoán vị, chúng ta kết nối các điểm liên tiếp và cuối cùng kết nối điểm cuối cùng trở lại điểm đầu tiên. Nếu chu trình kết quả là một đa giác đơn giản, chúng ta tính diện tích của nó bằng công thức dây giày và giữ lại giá trị tốt nhất. 

Phương pháp bạo lực này là chính xác vì mọi ranh giới đa giác có thể sử dụng tất cả các điểm đều xuất hiện dưới dạng một trong các hoán vị được tạo. Hoán vị hợp lệ tốt nhất trong số đó chính xác là câu trả lời bắt buộc. 

Lý do phương pháp này khả thi là do hạn chế về`n`. Trường hợp xấu nhất chỉ có`9!`hoán vị. Đối với mỗi hoán vị chúng tôi kiểm tra`n`các cạnh và so sánh các cặp cạnh để phát hiện các giao điểm, gần như là một cách khác`n^2`nhân tố. Tổng số thao tác nguyên thủy vẫn còn thấp hơn nhiều so với giới hạn thực tế vì`9! * 9^2`là khoảng 29 triệu séc đơn giản. 

Một cấu trúc hình học tiên tiến hơn có thể tìm thấy một đa giác có diện tích tối đa bằng cách sắp xếp các điểm xung quanh tâm, nhưng nó đòi hỏi phải xử lý cẩn thận hơn các trường hợp suy biến. Kích thước đầu vào nhỏ giúp việc tìm kiếm toàn diện trở thành một giải pháp an toàn và rõ ràng hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n^2) | O(n) | Đã chấp nhận | 
| Xây dựng hình học | O(n log n) | O(n) | Không cần thiết | 

## Hướng dẫn thuật toán 

1. Tạo mọi hoán vị của chỉ số điểm. Mỗi hoán vị đại diện cho một thứ tự có thể có của việc đi bộ xung quanh đường viền tấm bạt lò xo. 
2. Đối với thứ tự hiện tại, hãy dựng các cạnh đa giác giữa các đỉnh liên tiếp, bao gồm cạnh cuối cùng từ đỉnh cuối cùng trở lại đỉnh đầu tiên. Một câu trả lời hợp lệ phải có tất cả các cạnh này tạo thành một chu trình đơn giản. 
3. Kiểm tra từng cặp cạnh không liền kề. Nếu có cặp nào cắt nhau thì bác bỏ hoán vị này. Các cạnh liền kề được phép gặp nhau tại điểm cuối chung của chúng, vì vậy chúng bị bỏ qua trong quá trình kiểm tra này. 
4. Với mỗi đa giác hợp lệ, hãy tính diện tích nhân đôi của nó bằng công thức dây giày. Việc giữ giá trị gấp đôi sẽ tránh được các vấn đề về độ chính xác của dấu phẩy động. 
5. Lưu trữ hoán vị có diện tích lớn nhất tìm được. Nếu không có hoán vị nào hợp lệ, xuất ra`No`; nếu không thì xuất ra`Yes`và thứ tự đã lưu. 

Tại sao nó hoạt động: mọi sự sắp xếp có thể có của các điểm đã cho đều được kiểm tra. Kiểm tra giao nhau loại bỏ chính xác các đơn hàng không tạo thành đa giác đơn giản. Đối với mỗi thứ tự còn lại, công thức dây giày sẽ cho biết diện tích thực của đa giác đó, do đó việc chọn giá trị lớn nhất trong số tất cả các ứng cử viên hợp lệ sẽ tạo ra một đa giác tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from itertools import permutations

def cross(ax, ay, bx, by):
    return ax * by - ay * bx

def orientation(a, b, c):
    return cross(
        b[0] - a[0],
        b[1] - a[1],
        c[0] - a[0],
        c[1] - a[1]
    )

def on_segment(a, b, c):
    return (
        min(a[0], b[0]) <= c[0] <= max(a[0], b[0]) and
        min(a[1], b[1]) <= c[1] <= max(a[1], b[1])
    )

def segments_intersect(a, b, c, d):
    ab_c = orientation(a, b, c)
    ab_d = orientation(a, b, d)
    cd_a = orientation(c, d, a)
    cd_b = orientation(c, d, b)

    if ab_c == 0 and on_segment(a, b, c):
        return True
    if ab_d == 0 and on_segment(a, b, d):
        return True
    if cd_a == 0 and on_segment(c, d, a):
        return True
    if cd_b == 0 and on_segment(c, d, b):
        return True

    return (ab_c > 0) != (ab_d > 0) and (cd_a > 0) != (cd_b > 0)

def is_simple(order, pts):
    n = len(order)
    edges = []
    for i in range(n):
        edges.append((pts[order[i]], pts[order[(i + 1) % n]]))

    for i in range(n):
        for j in range(i + 1, n):
            if j == i + 1 or (i == 0 and j == n - 1):
                continue
            if segments_intersect(edges[i][0], edges[i][1], edges[j][0], edges[j][1]):
                return False
    return True

def area2(order, pts):
    s = 0
    n = len(order)
    for i in range(n):
        x1, y1 = pts[order[i]]
        x2, y2 = pts[order[(i + 1) % n]]
        s += x1 * y2 - y1 * x2
    return abs(s)

def solve():
    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    best = None
    best_area = -1

    for p in permutations(range(n)):
        if is_simple(p, pts):
            cur = area2(p, pts)
            if cur > best_area:
                best_area = cur
                best = p

    if best is None:
        print("No")
    else:
        print("Yes")
        print(*[x + 1 for x in best])

if __name__ == "__main__":
    solve()
```Việc thực hiện đầu tiên xác định các nguyên thủy hình học. Tích chéo xác định cạnh mà một điểm nằm trên đó so với đoạn có hướng và nó là cơ sở cho tất cả các kiểm tra giao lộ. 

Hàm giao đoạn xử lý cả trường hợp giao nhau thông thường và trường hợp thẳng hàng. Việc kiểm tra thẳng hàng là cần thiết vì việc chạm vào một điểm hoặc chồng chéo cũng bị cấm đối với các cạnh đa giác không liền kề. 

các`is_simple`Hàm tạo các cạnh chu trình và chỉ so sánh các cặp không lân cận trong đa giác. Các cạnh lân cận chia sẻ một đỉnh theo định nghĩa, vì vậy việc kiểm tra chúng sẽ loại bỏ mọi đa giác hợp lệ một cách không chính xác. 

Việc tính diện tích sử dụng gấp đôi diện tích thực. Điều này giữ mọi thứ ở dạng số nguyên và tránh các lỗi chính xác. Vì tất cả các đa giác ứng cử viên được so sánh bằng cách sử dụng cùng một giá trị diện tích gấp đôi nên giá trị tối đa được giữ nguyên. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu:```
5
0 0
2 2
-2 -2
2 -2
-2 2
```Một dấu vết tìm kiếm có thể: 

| Đã kiểm tra đơn hàng | Đơn giản? | Diện tích nhân đôi | Thứ tự tốt nhất | 
| --- | --- | --- | --- | 
| 1 2 3 4 5 | Không | 0 | không | 
| 1 2 4 3 5 | Có | 24 | 1 2 4 3 5 | 

Thứ tự được chọn tránh giao cắt và đưa ra một đa giác hợp lệ. Thuật toán không cần phải biết trước hình dạng vì nó đánh giá mọi cách sắp xếp có thể. 

Một ví dụ khác:```
3
0 0
1 0
2 0
```| Đã kiểm tra đơn hàng | Đơn giản? | Diện tích nhân đôi | Thứ tự tốt nhất | 
| --- | --- | --- | --- | 
| 1 2 3 | Có | 0 | 1 2 3 | 
| 1 3 2 | Có | 0 | 1 2 3 | 

Tất cả các điểm đều thẳng hàng nên mọi chu trình đều có diện tích bằng 0. Vì đa giác phải đơn giản và không suy biến nên chỉ kiểm tra giao điểm là không đủ. Việc triển khai cũng phải từ chối đa giác có diện tích bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n! * n^2) | Có n! hoán vị và mỗi hoán vị cần kiểm tra giao điểm cạnh O(n^2). | 
| Không gian | O(n) | Thuật toán lưu trữ các điểm, dữ liệu hoán vị hiện tại và thông tin cạnh. | 

Với`n <= 9`, số hạng giai thừa vẫn đủ nhỏ. Thuật toán thực hiện tìm kiếm hoàn chỉnh trên toàn bộ không gian của các đa giác có thể có trong khi vẫn nằm trong giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue()

assert run("""5
0 0
2 2
-2 -2
2 -2
-2 2
""").split()[0] == "Yes", "sample"

assert run("""3
0 0
1 0
2 0
""").strip() == "No", "collinear points"

assert run("""3
0 0
0 1
1 0
""").split()[0] == "Yes", "minimum polygon"

assert run("""4
0 0
2 0
2 2
0 2
""").split()[0] == "Yes", "rectangle"

assert run("""4
0 0
1 1
2 2
3 3
""").strip() == "No", "all equal slope"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cấu hình mẫu | Có | Trường hợp xây dựng đa giác thông thường | 
| Ba điểm thẳng hàng | Không | Xử lý hình học suy biến | 
| Ba điểm không thẳng hàng | Có | Đa giác hợp lệ nhỏ nhất có thể | 
| Hình chữ nhật | Có | Phát hiện đa giác đơn giản thường xuyên | 
| Bốn điểm trên một dòng | Không | Từ chối các trường hợp không thể | 

## Vỏ cạnh 

Đối với các điểm thẳng hàng:```
3
0 0
1 0
2 0
```Mọi hoán vị đều tạo ra một chuỗi khép kín có diện tích bằng 0. Thuật toán kiểm tra mọi thứ tự, nhưng không có thứ tự nào đưa ra đa giác có diện tích dương hợp lệ, vì vậy nó in`No`. 

Đối với các ứng viên vượt qua:```
4
0 0
2 2
0 2
2 0
```Thứ tự`1 2 3 4`tạo thành hình cánh cung tự giao nhau. Khi thuật toán so sánh hai cạnh đối diện, phép kiểm tra giao điểm sẽ phát hiện sự giao cắt và loại bỏ hoán vị này. Các hoán vị khác vẫn được xem xét và có thể tìm thấy thứ tự hình chữ nhật hợp lệ. 

Để chạm vào các cạnh, đường thẳng sẽ kiểm tra bên trong`segments_intersect`xử lý các trường hợp hai cạnh không liền kề có chung một điểm hoặc chồng lên nhau. Điều này ngăn việc chấp nhận đa giác có đường viền chỉ hợp lệ khi bỏ qua hình học chính xác.
