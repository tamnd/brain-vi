---
title: "CF 102168M - \u0412\u044b\u043f\u0443\u043a\u043b\u0430\u044f \u043e\u0431\u043e\u043b\u043e\u0447\u043a\u0430"
description: "Chúng ta có chính xác bốn điểm phân biệt trên mặt phẳng tọa độ. Không có ba số nào thẳng hàng. Chúng ta cần xác định có bao nhiêu trong bốn điểm này thuộc về bao lồi của cả bốn điểm."
date: "2026-08-19T07:31:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102168
codeforces_index: "M"
codeforces_contest_name: "\u041b\u0438\u0447\u043d\u044b\u0439 \u0447\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0421\u0430\u043c\u0430\u0440\u0441\u043a\u043e\u0433\u043e \u0443\u043d\u0438\u0432\u0435\u0440\u0441\u0438\u0442\u0435\u0442\u0430 \u0441\u0440\u0435\u0434\u0438 \u043d\u043e\u0432\u0438\u0447\u043a\u043e\u0432 2018-2019"
rating: 0
weight: 102168
solve_time_s: 72
verified: true
draft: false
---

[CF 102168M - \u0412\u044b\u043f\u0443\u043a\u043b\u0430\u044f \u043e\u0431\u043e\u043b\u043e\u0447\u043a\u0430](https://codeforces.com/problemset/problem/102168/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có chính xác bốn điểm phân biệt trên mặt phẳng tọa độ. Không có ba số nào thẳng hàng. Chúng ta cần xác định có bao nhiêu trong bốn điểm này thuộc về bao lồi của cả bốn điểm. 

Vì không có ba điểm nào nằm trên một đường thẳng nên chỉ có thể có hai cấu hình hình học. Cả bốn điểm đều là đỉnh của một tứ giác lồi, trong trường hợp đó câu trả lời là 4, hoặc một điểm nằm hoàn toàn bên trong tam giác tạo bởi ba điểm còn lại, trong trường hợp đó chỉ có ba điểm bên ngoài thuộc về bao lồi và câu trả lời là 3. 

Tọa độ nằm trong khoảng từ -100 đến 100 và luôn có chính xác bốn điểm. Điều này làm cho kích thước đầu vào không đổi, do đó, ngay cả việc kiểm tra trực tiếp mọi cấu hình có thể cũng sẽ đủ nhanh. Không cần phải triển khai bao lồi O(n log n) dành cho một tập hợp lớn các điểm. Phạm vi tọa độ nhỏ cũng có nghĩa là mọi tích chéo đều phù hợp một cách thoải mái trong số học số nguyên thông thường. Dù sao thì số nguyên Python cũng có độ chính xác tùy ý. 

Trường hợp cạnh chính là bốn điểm có thể tạo thành một tứ giác lồi ngay cả khi thứ tự đầu vào của chúng hoàn toàn tùy ý. Ví dụ,```
0 0
3 3
0 3
3 0
```có câu trả lời`4`. Một giải pháp giả định các điểm đầu vào đã được đưa ra xung quanh đường biên có thể tạo ra đa giác sai. 

Trường hợp quan trọng khác là một điểm nằm hoàn toàn bên trong tam giác được tạo bởi ba điểm còn lại. Ví dụ,```
-1 -1
2 -1
2 2
1 0
```có câu trả lời`3`, bởi vì`(1, 0)`nằm trong tam giác tạo bởi ba điểm đầu tiên. Một giải pháp bất cẩn chỉ đơn giản là đếm các điểm riêng biệt hoặc coi mọi điểm đầu vào là một đỉnh của thân tàu sẽ in không chính xác`4`. 

Sự đảm bảo loại trừ ba điểm thẳng hàng, vì vậy chúng ta không bao giờ phải quyết định liệu một điểm nằm ở giữa mép thân tàu có thuộc ranh giới hay không. Ví dụ: một đầu vào như```
0 0
1 0
2 0
0 1
```không thể xảy ra. Do đó, giải pháp đúng có thể sử dụng các thử nghiệm định hướng nghiêm ngặt mà không cần xử lý tích số chéo bằng 0. 

Một đầu vào trong đó cả bốn tọa độ đều bằng nhau cũng không thể thực hiện được, vì câu lệnh đảm bảo rằng không có hai điểm nào trùng nhau. Vì vậy, trường hợp như vậy không nên được thêm vào như một thử nghiệm hợp lệ của thuật toán. 

## Phương pháp tiếp cận 

Cách tiếp cận hình học trực tiếp đã là đủ vì chỉ có bốn điểm. Chúng ta có thể lần lượt chọn từng điểm và hỏi xem nó có nằm trong tam giác tạo bởi ba điểm còn lại hay không. Có bốn lựa chọn cho điểm ứng viên và việc kiểm tra một điểm so với ba điểm còn lại yêu cầu ba phép tính định hướng. Do đó, toàn bộ quá trình kiểm tra brute-force thực hiện tối đa 12 đánh giá sản phẩm chéo. Độ phức tạp về thời gian của nó là O(1), không phải là thứ trở nên quá chậm đối với vấn đề này. 

Quan sát quan trọng là hình học của bốn điểm không có ba điểm thẳng hàng là cực kỳ hạn chế. Nếu một điểm nằm trong tam giác của ba điểm còn lại thì điểm đó không thể là đỉnh của bao lồi. Ngược lại, nếu không có điểm nào nằm trong tam giác do ba điểm còn lại tạo thành thì cả bốn điểm đó đều phải là điểm cực trị nên chúng tạo thành một tứ giác lồi. 

Điều này cho phép chúng ta tránh được việc xây dựng toàn bộ thân tàu. Đối với điểm P ứng cử viên và tam giác ABC được tạo bởi ba điểm còn lại, P hoàn toàn nằm bên trong tam giác khi hướng của P đối với cả ba cạnh có hướng có cùng dấu. Chúng ta có thể tính tích chéo của AB với AP, BC với BP ​​và CA với CP. Nếu cả ba giá trị đều dương hoặc cả ba đều âm thì P nằm trong. 

Các phương pháp tiếp cận vũ phu và tối ưu đều là thời gian không đổi ở đây. Sự khác biệt là cách tiếp cận thân tàu trực tiếp giải quyết được một vấn đề tổng quát hơn mức cần thiết, trong khi quan sát tam giác ngăn chặn sử dụng thực tế đặc biệt là có chính xác bốn điểm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bốn điểm và lưu chúng dưới dạng cặp tọa độ. 
2. Với mỗi điểm P, coi ba điểm còn lại là các đỉnh A, B, C của một tam giác. Chúng ta kiểm tra xem P có nằm hoàn toàn bên trong tam giác này hay không vì điểm trong chính xác là loại điểm không xuất hiện trên bao lồi. 
3. Tính hướng của P so với mỗi cạnh của tam giác. Cho hai vectơ`(x1, y1)`Và`(x2, y2)`, tích chéo của chúng là`x1 * y2 - y1 * x2`. Dấu của nó cho chúng ta biết một vectơ ở bên trái hay bên phải của vectơ kia. 
4. Nếu ba giá trị định hướng đều dương hoặc đều âm thì P nằm hoàn toàn bên trong ABC. Sự vắng mặt của các bộ ba thẳng hàng đảm bảo rằng không có giá trị nào trong số này bằng 0. 
5. Nếu tìm thấy bất kỳ điểm nào bên trong tam giác tạo bởi ba điểm còn lại, hãy in`3`. Chính xác điểm đó được loại trừ khỏi bao lồi. 
6. Nếu không có điểm nào nằm trong một tam giác như vậy, hãy in`4`. Mọi điểm khi đó đều là điểm cực trị nên bao lồi có 4 đỉnh. 

### Tại sao nó hoạt động 

Xét bốn điểm bất kỳ không có ba điểm nào thẳng hàng. Nếu một điểm P nằm bên trong tam giác của ba điểm còn lại thì đa giác lồi nhỏ nhất chứa cả bốn điểm chính xác là tam giác đó, do đó ba điểm đầu vào nằm trên ranh giới thân tàu. 

Nếu không có điểm nào nằm bên trong tam giác được tạo bởi ba điểm còn lại, giả sử mâu thuẫn rằng thân tàu có ít hơn bốn đỉnh. Vì có bốn điểm phân biệt nên một trong số chúng sẽ phải nằm bên trong tam giác được tạo bởi ba điểm còn lại. Điều đó mâu thuẫn với bài kiểm tra. Do đó cả bốn điểm đều là đỉnh của thân và đáp án là 4. 

Bài kiểm tra hướng xác định chính xác một điểm bên trong một tam giác vì một điểm nằm hoàn toàn bên trong một tam giác nằm trên cùng một cạnh của mỗi cạnh trong số ba cạnh định hướng của tam giác. Vì các bộ ba cộng tuyến bị cấm nên các trường hợp bên trong và biên nghiêm ngặt được phân tách mà không có sự mơ hồ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(a, b, c):
    """Cross product of AB and AC."""
    return (b[0] - a[0]) * (c[1] - a[1]) - \
           (b[1] - a[1]) * (c[0] - a[0])

def inside_triangle(p, a, b, c):
    x1 = cross(a, b, p)
    x2 = cross(b, c, p)
    x3 = cross(c, a, p)

    return (x1 > 0 and x2 > 0 and x3 > 0) or \
           (x1 < 0 and x2 < 0 and x3 < 0)

def solve():
    points = [tuple(map(int, input().split())) for _ in range(4)]

    for i in range(4):
        p = points[i]
        others = [points[j] for j in range(4) if j != i]

        if inside_triangle(p, others[0], others[1], others[2]):
            print(3)
            return

    print(4)

if __name__ == "__main__":
    solve()
```các`cross`hàm tính biểu thức diện tích có dấu cho tam giác được hình thành bởi`a`,`b`, Và`c`. Chỉ có dấu hiệu của nó là quan trọng. Giá trị dương có nghĩa là C nằm về một phía của đường thẳng AB, trong khi giá trị âm có nghĩa là nó nằm ở phía bên kia.`inside_triangle`đánh giá điểm ứng cử viên so với cả ba bên được chỉ đạo. Các hướng được chọn theo chu kỳ như`AB`,`BC`, Và`CA`, do đó một điểm bên trong tam giác sẽ cho ba dấu hiệu phù hợp. Hai hướng có thể có của tam giác được xử lý bằng cách kiểm tra ba giá trị dương hoặc ba giá trị âm. 

Vòng lặp chính thử tất cả bốn điểm làm điểm bên trong có thể có. Việc hiểu danh sách chọn chính xác ba điểm còn lại, điều này tránh dựa vào bất kỳ thứ tự đầu vào nào. 

Không có số học dấu phẩy động. Việc sử dụng tích chéo số nguyên vừa đơn giản vừa đáng tin cậy hơn so với tính toán góc hoặc giao điểm của đường thẳng. Với tọa độ từ -100 đến 100, các giá trị trung gian rất nhỏ và phép tính số nguyên của Python sẽ loại bỏ mọi lo ngại về tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Điểm đầu vào là bốn góc của hình vuông.```
0 0
3 0
3 3
0 3
```Thuật toán kiểm tra từng điểm so với tam giác được tạo bởi ba điểm còn lại. 

| Ứng viên P | Tam giác các điểm khác | Dấu hiệu sản phẩm chéo | Bên trong? | 
| --- | --- | --- | --- | 
|`(0, 0)`|`(3,0), (3,3), (0,3)`|`+, +, +`không thu được cho ứng cử viên này theo các cạnh được định hướng đã chọn | Không | 
|`(3, 0)`|`(0,0), (3,3), (0,3)`| dấu hiệu hỗn hợp | Không | 
|`(3, 3)`|`(0,0), (3,0), (0,3)`| dấu hiệu hỗn hợp | Không | 
|`(0, 3)`|`(0,0), (3,0), (3,3)`| dấu hiệu hỗn hợp | Không | 

Không có ứng cử viên nào nằm trong tam giác của ba điểm còn lại nên cả bốn điểm đều là điểm cực trị. Đầu ra là`4`. 

Dấu vết cho thấy tại sao thứ tự đầu vào không liên quan. Các điểm tạo thành một tứ giác lồi bất kể thứ tự chúng được cung cấp. 

### Mẫu 2 

Đầu vào là```
-1 -1
2 -1
2 2
1 0
```điểm`(1, 0)`nằm bên trong tam giác tạo bởi ba điểm đầu tiên. 

| Ứng viên P | Tam giác các điểm khác | Dấu hiệu sản phẩm chéo | Bên trong? | 
| --- | --- | --- | --- | 
|`(-1,-1)`|`(2,-1), (2,2), (1,0)`| hỗn hợp | Không | 
|`(2,-1)`|`(-1,-1), (2,2), (1,0)`| hỗn hợp | Không | 
|`(2,2)`|`(-1,-1), (2,-1), (1,0)`| hỗn hợp | Không | 
|`(1,0)`|`(-1,-1), (2,-1), (2,2)`|`+, +, +`| Có | 

Điểm thứ tư nằm hoàn toàn bên trong tam giác của ba điểm còn lại nên nó không thuộc bao lồi. Thuật toán in ngay lập tức`3`. 

Ví dụ này thể hiện sự khác biệt trung tâm giữa một tập điểm tùy ý và bao lồi của nó. Một điểm có thể là một trong những điểm cho trước mà không phải là đỉnh thân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Có chính xác bốn điểm ứng viên và tối đa ba tích chéo cho mỗi ứng viên. | 
| Không gian | O(1) | Chỉ có bốn điểm và một số giá trị trung gian không đổi được lưu trữ. | 

Kích thước đầu vào được cố định ở bốn điểm, do đó thuật toán thoải mái trong giới hạn thời gian 2 giây và sử dụng bộ nhớ không đáng kể. Ngay cả việc triển khai bao lồi thông thường cũng sẽ vượt qua, nhưng nó sẽ đưa ra sự phân loại không cần thiết và nhiều mã hơn. 

## Trường hợp thử nghiệm 

Bài toán có đúng bốn điểm nên không có đầu vào hợp lệ nào nhỏ hơn. Tương tự như vậy, bài kiểm tra tọa độ hoàn toàn bằng nhau không hợp lệ vì các điểm trùng khớp bị cấm. Thay vào đó, các thử nghiệm tùy chỉnh bên dưới bao gồm trường hợp hợp lệ nhỏ nhất, cấu hình lồi ở giới hạn tọa độ, điểm bên trong gần cạnh tam giác và cấu hình có thứ tự đầu vào không tuân theo thứ tự thân tàu.```python
import sys
import io

def cross(a, b, c):
    return (b[0] - a[0]) * (c[1] - a[1]) - \
           (b[1] - a[1]) * (c[0] - a[0])

def inside_triangle(p, a, b, c):
    x1 = cross(a, b, p)
    x2 = cross(b, c, p)
    x3 = cross(c, a, p)

    return (x1 > 0 and x2 > 0 and x3 > 0) or \
           (x1 < 0 and x2 < 0 and x3 < 0)

def solve():
    points = [tuple(map(int, input().split())) for _ in range(4)]

    for i in range(4):
        p = points[i]
        others = [points[j] for j in range(4) if j != i]

        if inside_triangle(p, others[0], others[1], others[2]):
            print(3)
            return

    print(4)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()
    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# Provided sample 1
assert run("""\
0 0
3 0
3 3
0 3
""") == "4\n", "sample 1"

# Provided sample 2
assert run("""\
-1 -1
2 -1
2 2
1 0
""") == "3\n", "sample 2"

# Custom 1: smallest valid input, four corners in arbitrary order
assert run("""\
0 0
1 1
0 1
1 0
""") == "4\n", "convex quadrilateral with arbitrary input order"

# Custom 2: coordinates at the allowed boundaries
assert run("""\
-100 -100
100 -100
100 100
-100 100
""") == "4\n", "maximum coordinate magnitude"

# Custom 3: one point is strictly inside, close to a triangle edge
assert run("""\
0 0
10 0
0 10
1 1
""") == "3\n", "one interior point"

# Custom 4: interior point with negative coordinates
assert run("""\
-5 -5
5 -5
0 5
0 0
""") == "3\n", "interior point in a triangle spanning negative coordinates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`,`1 1`,`0 1`,`1 0`|`4`| Đầu vào hợp lệ nhỏ nhất và thứ tự đầu vào tùy ý | 
|`-100 -100`,`100 -100`,`100 100`,`-100 100`|`4`| Tọa độ các giá trị biên | 
|`0 0`,`10 0`,`0 10`,`1 1`|`3`| Biển báo định hướng và phát hiện bên trong nghiêm ngặt | 
|`-5 -5`,`5 -5`,`0 5`,`0 0`|`3`| Phát hiện bên trong với tọa độ âm | 

## Vỏ cạnh 

Trường hợp thứ tự đầu vào tùy ý được xử lý vì thuật toán không bao giờ giả định rằng các điểm đầu vào liên tiếp tạo thành các cạnh thân. Vì```
0 0
1 1
0 1
1 0
```mỗi điểm được kiểm tra với ba điểm còn lại. Không có điểm nào nằm trong tam giác được tạo bởi các điểm khác, do đó thuật toán in ra`4`. Một phương pháp kết nối các điểm theo thứ tự đầu vào và coi thứ tự đó là ranh giới đa giác có thể đưa ra giả định không cần thiết mà đầu vào không cung cấp. 

Trường hợp điểm trong được xử lý trực tiếp bằng phép thử tam giác. Vì```
0 0
10 0
0 10
1 1
```điểm`(1,1)`nằm bên trong tam giác có các đỉnh`(0,0)`,`(10,0)`, Và`(0,10)`. Ba tích chéo đều cùng dấu nên`inside_triangle`trả về true và câu trả lời là`3`. Sự thật là`(1,1)`gần với cạnh chéo không gây ra vấn đề về độ chính xác vì tất cả các phép tính đều sử dụng số nguyên chính xác. 

Trường hợp ranh giới tọa độ```
-100 -100
100 -100
100 100
-100 100
```tạo thành một hình vuông có bốn góc là các đỉnh của thân. Các tích chéo vẫn là số nguyên chính xác, không liên quan đến so sánh dấu phẩy động và kết quả là`4`. 

Cuối cùng, các trường hợp biên thẳng hàng không cần nhánh đặc biệt vì đầu vào đảm bảo rằng không có ba điểm nào thẳng hàng. Một điểm không thể nằm chính xác trên một cạnh tam giác trong một bài kiểm tra hợp lệ. Do đó, sự so sánh chặt chẽ`> 0`Và`< 0`mô tả chính xác các khả năng liên quan duy nhất và thuật toán không bao giờ phải quyết định liệu một điểm thẳng hàng có được tính là một đỉnh của thân hay không.
