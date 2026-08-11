---
title: "CF 102420A - \u0417\u0430 \u0433\u0440\u043e\u0431\u043e\u0446\u0432\u0435\u0442\u0430\u043c\u0438"
description: "Chúng ta có (n) thợ săn và mỗi thợ săn chiếm một điểm riêng biệt ((xi,yi)) trên mặt phẳng. Chúng ta cần chọn ba thợ săn khác nhau có vị trí không nằm trên một đường thẳng. Nếu bộ ba như vậy tồn tại, chúng ta in Có và chỉ số của chúng."
date: "2026-08-10T11:33:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102420
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102420
solve_time_s: 1141
verified: true
draft: false
---

[CF 102420A - \u0417\u0430 \u0433\u0440\u043e\u0431\u043e\u0446\u0432\u0435\u0442\u0430\u043c\u0438](https://codeforces.com/problemset/problem/102420/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 19 phút 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có (n) thợ săn và mỗi thợ săn chiếm một điểm riêng biệt ((x_i,y_i)) trên mặt phẳng. Chúng ta cần chọn ba thợ săn khác nhau có vị trí không nằm trên một đường thẳng. Nếu bộ ba như vậy tồn tại, chúng tôi in`Yes`và chỉ số của chúng. Nếu mọi thợ săn nằm trên cùng một đường thẳng, chúng ta sẽ in`No`. 

Sự khác biệt giữa hai trường hợp này là hình học nhưng có thể được kiểm tra bằng số học số nguyên. Đối với ba điểm (A), (B) và (C), chúng thẳng hàng khi các vectơ (B-A) và (C-A) có tích chéo bằng 0: 

[ 
(x_B-x_A)(y_C-y_A) - (y_B-y_A)(x_C-x_A)=0. 
] 

Giới hạn (n\le 100000) loại trừ việc kiểm tra mọi bộ ba. có 

[ 
\binom{100000}{3}\khoảng 1,67\cdot 10^{14} 
] 

gấp ba lần, vượt xa giới hạn thời gian lập trình cạnh tranh có thể xử lý được. Chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. Giới hạn tọa độ đạt đến (10^9), do đó các sản phẩm trong tích chéo có thể đạt tới khoảng (4\cdot 10^{18}) và chênh lệch có thể đạt tới (8\cdot 10^{18}). Số nguyên Python xử lý chính xác các giá trị này, trong khi số nguyên 32 bit có chiều rộng cố định sẽ bị tràn rất nhiều. 

Trường hợp không rõ ràng đầu tiên là khi ba thợ săn đầu tiên thẳng hàng mặc dù tồn tại một câu trả lời hợp lệ. Ví dụ,```
4
0 0
1 1
2 2
0 1
```Đầu ra đúng là`Yes`, ví dụ với các chỉ số`1 2 4`. Phương pháp chỉ kiểm tra ba điểm đầu tiên sẽ in sai`No`. 

Trường hợp ngược lại là khi tất cả các điểm thực sự nằm trên một dòng:```
4
1 1
2 2
3 3
4 4
```Đầu ra đúng là`No`. Bất kỳ phương pháp nào cũng phải có khả năng kiểm tra các điểm ngoài ba điểm đầu tiên trước khi tuyên bố là không thể thực hiện được. 

Trường hợp thứ ba liên quan đến một đường thẳng đứng:```
3
5 0
5 2
5 7
```Đầu ra đúng là`No`. Việc tính toán độ dốc chẳng hạn như ((y_2-y_1)/(x_2-x_1)) có thể vô tình đưa ra phép chia cho 0. Sản phẩm chéo không có trường hợp đặc biệt như vậy nên các đường dọc và ngang được xử lý giống hệt nhau. 

## Phương pháp tiếp cận 

Giải pháp brute-force trực tiếp xem xét từng bộ ba thợ săn và kiểm tra xem ba điểm của nó có thẳng hàng hay không. Điều này đúng vì mọi câu trả lời có thể đều được kiểm tra rõ ràng. Vấn đề là số lượng gấp ba. Đối với (n=100000), có khoảng (1,67\cdot10^{14}) trong số chúng, do đó, ngay cả một lượng công việc không đổi rất nhỏ trên mỗi bộ ba cũng là quá nhiều. 

Quan sát hữu ích là hai thợ săn đầu tiên rất khác biệt nên họ xác định được một đường duy nhất. Nếu có bất kỳ câu trả lời hợp lệ nào thì phải có thợ săn thứ ba ở ngoài ranh giới này. Ngược lại, nếu mọi thợ săn đều nằm trên đường này thì mọi bộ ba có thể đều thẳng hàng và câu trả lời là không thể. 

Điều đó có nghĩa là chúng ta không cần phải tìm kiếm theo bộ ba. Sửa thợ săn (1) và (2), sau đó quét mọi thợ săn còn lại và kiểm tra xem nó có nằm trên đường của họ hay không. Điểm đầu tiên không nằm trên đường thẳng sẽ ngay lập tức đưa ra câu trả lời hợp lệ. Nếu quá trình quét đến cuối mà không tìm thấy, tất cả các thợ săn đều ở trên cùng một hàng và câu trả lời là`No`. 

Phương pháp brute-force hoạt động vì việc kiểm tra bộ ba trực tiếp trả lời câu hỏi cho bộ ba đó, nhưng nó không thành công vì có quá nhiều bộ ba. Việc quan sát theo đường cố định làm giảm vấn đề xuống còn một phép thử cộng tuyến cho mỗi điểm còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^3)) | (O(1)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả (n) điểm cùng với chỉ số dựa trên 1 ban đầu của chúng. Chúng ta cần các chỉ số ở đầu ra, không chỉ tọa độ. 
2. Lấy hai điểm đầu tiên là (A=(x_1,y_1)) và (B=(x_2,y_2)). Bởi vì tuyên bố đảm bảo rằng không có hai thợ săn nào chiếm giữ cùng một điểm, nên hai điểm này xác định một đường duy nhất. 
3. Với mọi điểm (C=(x_i,y_i)) bắt đầu từ điểm thứ ba, hãy tính 

[ 
chéo=(x_2-x_1)(y_i-y_1)-(y_2-y_1)(x_i-x_1). 
]

Nếu như`cross != 0`, ba điểm (A,B,C) không thẳng hàng nên in ngay`Yes`và chỉ số của chúng. 

1. Nếu mọi điểm còn lại có`cross == 0`, mọi thợ săn đều nằm trên đường đi qua hai thợ săn đầu tiên. Trong trường hợp đó không có ba thợ săn nào có thể tạo thành một tam giác không suy biến, vì vậy in`No`. 

Quá trình quét hoạt động vì đường đi qua hai điểm đầu tiên được cố định trong toàn bộ thuật toán. Tích chéo khác 0 chứng tỏ rằng điểm hiện tại nằm ngoài đường đó, trong khi tích chéo bằng 0 chứng tỏ rằng điểm đó nằm trên đường đó. 

### Tại sao nó hoạt động 

Điều bất biến là hai điểm đầu tiên luôn xác định đường tham chiếu. Trong quá trình quét, nếu một điểm có tích chéo khác 0 với hai điểm này thì nằm ngoài đường thẳng đó nên ba điểm được chọn không thể thẳng hàng và thuật toán đã tìm được đáp án hợp lệ. 

Nếu thuật toán không bao giờ tìm thấy điểm như vậy thì mọi điểm đầu vào đều có tích chéo bằng 0 với hai điểm đầu tiên. Do đó mọi điểm đều nằm trên đường thẳng duy nhất của chúng. Vì tất cả các thợ săn đều ở trên một đường nên mọi lựa chọn có thể có của ba thợ săn đều thẳng hàng, vì vậy`No`là câu trả lời đúng duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    points = []

    for i in range(1, n + 1):
        x, y = map(int, input().split())
        points.append((x, y, i))

    x1, y1, i1 = points[0]
    x2, y2, i2 = points[1]

    dx = x2 - x1
    dy = y2 - y1

    for x, y, i in points[2:]:
        cross = dx * (y - y1) - dy * (x - x1)

        if cross != 0:
            print("Yes")
            print(i1, i2, i)
            return

    print("No")

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào lưu trữ từng tọa độ cùng với chỉ mục ban đầu của nó. Điều này tránh việc phải xây dựng lại các chỉ số sau này và giữ cho phép tính hình học tách biệt khỏi việc đánh số đầu ra. 

Hai sự khác biệt`dx`Và`dy`được tính một lần vì đường tham chiếu không bao giờ thay đổi. Đối với mỗi điểm sau đó, chỉ có vectơ từ điểm đầu tiên đến điểm hiện tại thay đổi. 

biểu thức```
dx * (y - y1) - dy * (x - x1)
```là tích chéo hai chiều. Dấu hiệu của nó cho biết điểm nằm ở phía nào của đường tham chiếu, nhưng đối với bài toán này chỉ có vấn đề 0 và khác 0. 

Không có sự phân chia nên đường thẳng đứng không gây ra trường hợp đặc biệt nào. Số nguyên Python cũng tránh tràn ngay cả đối với tọa độ lớn nhất được phép. Các chỉ số được lưu trữ trong`points`đã dựa trên 1, phù hợp với định dạng đầu ra được yêu cầu. 

Vòng lặp bắt đầu lúc`points[2:]`bởi vì hai thợ săn đầu tiên đã được sử dụng làm cặp cố định. Không có vấn đề gì xảy ra ở đầu ra vì chỉ mục được lưu trữ chính xác là vị trí đầu vào. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
3
1 1
2 2
2 3
```Hai điểm đầu tiên xác định đường thẳng (y=x). Điểm thứ ba nằm phía trên đường đó. 

| Bước | Tham khảo A | Tham khảo B | C hiện tại | Sản phẩm chéo | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | ((1,1)) | ((2,2)) | ((2,3)) | (1\cdot2-1\cdot1=1) | In`Yes 1 2 3`| 

Tích chéo khác 0 nên ba điểm không thẳng hàng. Thuật toán kết thúc ngay lập tức với bộ ba hợp lệ. 

### Mẫu 2 

Đầu vào là:```
5
1 2
0 0
3 6
4 8
4 4
```Hai điểm đầu tiên xác định đường thẳng (y=2x). Điểm 3 và 4 cũng nằm trên đường đó, còn điểm 5 thì không. 

| Bước | Tham khảo A | Tham khảo B | C hiện tại | Sản phẩm chéo | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | ((1,2)) | ((0,0)) | ((3,6)) | ((-1)\cdot4-(-2)\cdot2=0) | Tiếp tục | 
| 2 | ((1,2)) | ((0,0)) | ((4,8)) | ((-1)\cdot6-(-2)\cdot3=0) | Tiếp tục | 
| 3 | ((1,2)) | ((0,0)) | ((4,4)) | ((-1)\cdot2-(-2)\cdot3=4) | In`Yes 1 2 5`| 

Đầu ra mẫu sử dụng bộ ba hợp lệ khác,`3 2 5`. Bài toán chấp nhận ba thợ săn không thẳng hàng bất kỳ, vì vậy`1 2 5`cũng đúng như nhau. 

Hai điểm ứng cử viên đầu tiên sau cặp tham chiếu chứng minh tại sao chỉ kiểm tra ba thợ săn đầu tiên là không đủ. Một số điểm có thể nằm trên đường tham chiếu trước khi quá trình quét đạt đến điểm thứ ba hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Mỗi thợ săn được đọc một lần và mỗi điểm sau hai điểm đầu tiên nhận được một bài kiểm tra sản phẩm chéo. | 
| Không gian | (O(n)) | Tọa độ và chỉ số của tất cả các thợ săn được lưu trữ. | 

Với (n\le100000), quá trình quét tuyến tính chỉ thực hiện khoảng (100000) phép thử hình học, điều này rất dễ thực hiện. Việc sử dụng bộ nhớ cũng tuyến tính và thoải mái trong giới hạn lập trình cạnh tranh thông thường. 

## Trường hợp thử nghiệm 

Câu lệnh đảm bảo rằng không có hai thợ săn nào chiếm giữ cùng một điểm, do đó, đầu vào theo nghĩa đen trong đó tất cả các cặp tọa độ giống hệt nhau sẽ nằm ngoài miền đầu vào hợp lệ. Bộ thử nghiệm bên dưới vẫn bao gồm đầu vào như một biện pháp kiểm tra độ chắc chắn. Không cần phải có một giải pháp hợp lệ để hỗ trợ nó, nhưng việc triển khai sẽ trả về một cách tự nhiên`No`. 

Trình trợ giúp xác nhận nắm bắt đầu ra tiêu chuẩn, trong khi trình kiểm tra xác thực thuộc tính hình học thực tế thay vì yêu cầu một bộ ba hợp lệ cụ thể. Điều này là cần thiết vì bài toán cho phép bất kỳ bộ ba chỉ số chính xác nào.```python
import sys
import io
import contextlib

def solve():
    input = sys.stdin.readline

    n = int(input())
    points = []

    for i in range(1, n + 1):
        x, y = map(int, input().split())
        points.append((x, y, i))

    x1, y1, i1 = points[0]
    x2, y2, i2 = points[1]

    dx = x2 - x1
    dy = y2 - y1

    for x, y, i in points[2:]:
        cross = dx * (y - y1) - dy * (x - x1)

        if cross != 0:
            print("Yes")
            print(i1, i2, i)
            return

    print("No")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    output = io.StringIO()
    try:
        with contextlib.redirect_stdout(output):
            solve()
    finally:
        sys.stdin = old_stdin

    return output.getvalue().strip()

def valid_answer(inp: str, out: str) -> bool:
    data = inp.strip().splitlines()
    n = int(data[0])
    points = [tuple(map(int, line.split())) for line in data[1:]]

    tokens = out.split()

    if tokens[0] == "No":
        x1, y1 = points[0]
        x2, y2 = points[1]

        for x, y in points[2:]:
            cross = (x2 - x1) * (y - y1) - (y2 - y1) * (x - x1)
            if cross != 0:
                return False

        return True

    assert tokens[0] == "Yes"
    a, b, c = map(int, tokens[1:4])
    assert 1 <= a <= n
    assert 1 <= b <= n
    assert 1 <= c <= n
    assert len({a, b, c}) == 3

    x1, y1 = points[a - 1]
    x2, y2 = points[b - 1]
    x3, y3 = points[c - 1]

    cross = (x2 - x1) * (y3 - y1) - (y2 - y1) * (x3 - x1)
    return cross != 0

# Sample 1
sample1 = """\
3
1 1
2 2
2 3
"""
assert run(sample1).startswith("Yes")
assert valid_answer(sample1, run(sample1)), "sample 1"

# Sample 2
sample2 = """\
5
1 2
0 0
3 6
4 8
4 4
"""
assert run(sample2).startswith("Yes")
assert valid_answer(sample2, run(sample2)), "sample 2"

# Sample 3
sample3 = """\
4
1 1
2 2
3 3
4 4
"""
assert run(sample3) == "No"
assert valid_answer(sample3, run(sample3)), "sample 3"

# Minimum-size input, non-collinear
case4 = """\
3
0 0
1 1
1 0
"""
assert valid_answer(case4, run(case4)), "minimum non-collinear case"

# Minimum-size input, all points collinear
case5 = """\
3
-5 7
0 7
10 7
"""
assert run(case5) == "No"
assert valid_answer(case5, run(case5)), "minimum collinear case"

# Boundary coordinates and large cross product
case6 = """\
4
-1000000000 -1000000000
1000000000 1000000000
1000000000 -1000000000
0 0
"""
assert valid_answer(case6, run(case6)), "coordinate boundary case"

# First three points are collinear, fourth is not
case7 = """\
4
0 0
1 1
2 2
0 1
"""
assert valid_answer(case7, run(case7)), "late non-collinear point"

# Robustness only: duplicate coordinates are forbidden by the statement.
case8 = """\
3
5 5
5 5
5 5
"""
assert run(case8) == "No"
assert valid_answer(case8, run(case8)), "duplicate-coordinate robustness"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 0 0 / 1 1 / 1 0`|`Yes`| Đầu vào hợp lệ tối thiểu với bộ ba không thẳng hàng | 
|`3 / -5 7 / 0 7 / 10 7`|`No`| Đầu vào tối thiểu trong đó mỗi điểm nằm trên một đường ngang | 
|`4 / -10^9 -10^9 / 10^9 10^9 / 10^9 -10^9 / 0 0`|`Yes`| Ranh giới tọa độ và tích chéo số nguyên lớn | 
|`4 / 0 0 / 1 1 / 2 2 / 0 1`|`Yes`| Ngăn chặn chiến lược sai lầm khi chỉ kiểm tra ba điểm đầu tiên | 
|`3 / 5 5 / 5 5 / 5 5`|`No`| Mạnh mẽ chống lại tọa độ trùng lặp, mặc dù điều này vi phạm đảm bảo đầu vào | 

## Vỏ cạnh 

Khi ba điểm đầu tiên thẳng hàng, thuật toán không kết luận rằng câu trả lời là không thể. Vì```
4
0 0
1 1
2 2
0 1
```đường tham chiếu là (y=x). Điểm thứ ba cho tích số chéo (0), do đó quá trình quét vẫn tiếp tục. Điểm thứ tư cho 

[ 
1\cdot1-1\cdot0=1, 
] 

vì vậy thuật toán in`Yes`với chỉ số`1 2 4`. Đây chính xác là trường hợp phá vỡ giải pháp ba điểm đầu tiên ngây thơ. 

Khi tất cả các điểm thẳng hàng, chẳng hạn như```
4
1 1
2 2
3 3
4 4
```hai điểm đầu tiên xác định (y=x). Mọi tích chéo tiếp theo đều bằng 0. Vòng lặp kết thúc mà không tìm thấy điểm bên ngoài, do đó thuật toán sẽ in`No`. Vì mọi điểm đều thuộc cùng một đường thẳng nên không có bộ ba thay thế nào có thể hoạt động được. 

Đối với đường thẳng đứng,```
3
5 0
5 2
5 7
```sự khác biệt giữa hai tọa độ (x) đầu tiên bằng không. Sản phẩm chéo vẫn hoạt động trực tiếp: 

[ 
0\cdot(y-0)-2\cdot(5-5)=0. 
] 

Thuật toán in`No`không có bất kỳ sự phân chia hay xử lý đặc biệt nào. Điều này an toàn hơn việc triển khai dựa trên độ dốc, trong đó biểu thức ((y_2-y_1)/(x_2-x_1)) sẽ chia cho 0. 

Cuối cùng, hãy xem xét phạm vi tọa độ tối đa:```
4
-1000000000 -1000000000
1000000000 1000000000
1000000000 -1000000000
0 0
```Sự khác biệt giữa hai tọa độ (x) đầu tiên là (2\cdot10^9) và sai phân (y) tương ứng có cùng độ lớn. Đối với điểm thứ ba, tích chéo có thứ tự là (10^{18}), nhưng các số nguyên có độ chính xác tùy ý của Python đánh giá chính xác nó. Do đó, thuật toán đưa ra quyết định hình học chính xác mà không bị tràn.
