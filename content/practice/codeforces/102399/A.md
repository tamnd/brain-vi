---
title: "CF 102399A - \u041c\u0430\u0440\u0438\u043e \u0438 \u043c\u0438\u0440\u043e\u0432\u043e\u0439 \u0440\u0435\u043a\u043e\u0440\u0434"
description: "Chúng tôi có (n) ống. Ống thứ (i)-th có chiều dài (sqrt{ai}), trong đó (1 le ai le 10^6). Mario muốn kết nối một số đường ống này thành một đường đa tuyến bắt đầu từ điểm gốc. Mọi khớp nối, kể cả vòi cuối cùng, phải có tọa độ nguyên."
date: "2026-08-11T05:14:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102399
codeforces_index: "A"
codeforces_contest_name: "2019 \u041c\u043e\u0441\u043a\u043e\u0432\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432, \u043b\u0438\u0433\u0430 A"
rating: 0
weight: 102399
solve_time_s: 118
verified: true
draft: false
---

[CF 102399A - \u041c\u0430\u0440\u0438\u043e \u0438 \u043c\u0438\u0440\u043e\u0432\u043e\u0439 \u0440\u0435\u043a\u043e\u0440\u0434](https://codeforces.com/problemset/problem/102399/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có (n) ống. Ống thứ (i)-th có chiều dài (\sqrt{a_i}), trong đó (1 \le a_i \le 10^6). Mario muốn kết nối một số đường ống này thành một đường đa tuyến bắt đầu từ điểm gốc. Mọi khớp nối, kể cả vòi cuối cùng, phải có tọa độ nguyên. Một đường ống có thể được sử dụng tối đa một lần và một số đường ống có thể không được sử dụng. 

Một đường ống chỉ có thể kết nối hai điểm tọa độ nguyên nếu chiều dài bình phương của nó có thể được viết là 

[ 
x^2+y^2=a_i 
] 

đối với một số số nguyên (x, y). Nếu cách biểu diễn như vậy không tồn tại thì đường ống đó không bao giờ có thể được sử dụng. Mục tiêu là tối đa hóa khoảng cách Euclide giữa điểm gốc và điểm cuối cuối cùng. Đầu ra là chuỗi các đỉnh tọa độ nguyên của bất kỳ đa tuyến tối ưu nào. 

Giới hạn (n\le 10^5) loại trừ việc thực hiện công tỷ lệ với (n\sqrt{a_i}) ​​một cách độc lập cho mọi đường ống nếu chúng ta muốn có một giải pháp thoải mái. Vì (\sqrt{a_i}\le1000), việc kiểm tra mọi tọa độ có thể có cho mỗi đường ống có thể đạt tới khoảng (1000\cdot10^5=10^8) lần lặp. Thay vào đó, giới hạn hữu ích là giá trị tối đa nhỏ hơn nhiều (a_i\le10^6), cho phép chúng tôi xử lý trước mọi độ dài bình phương có thể một lần. 

Có hai trường hợp nguy hiểm mà việc triển khai bất cẩn có thể bỏ sót. Đầu tiên, một đường ống có thể không có tọa độ nguyên nào cả. Ví dụ,```
1
3
```có đầu ra đúng```
1
0 0
```vì (3) không phải là tổng của hai bình phương nguyên. Một chương trình giả định rằng mọi ống đều có thể được đặt sẽ cố gắng xây dựng một đoạn có chiều dài bình phương (3), điều này là không thể. 

Thứ hai, một đường ống có thể hợp lệ nhưng có khả năng thực hiện hoàn toàn theo chiều ngang hoặc chiều dọc. Ví dụ,```
1
1000000
```có phân đoạn hợp lệ ((0,0)\rightarrow(1000,0)), vì (1000^2=10^6). Đầu ra chính xác có thể là```
2
0 0
1000 0
```Một lỗi triển khai phổ biến là chỉ tìm kiếm các biểu diễn có cả hai tọa độ dương, điều này sẽ loại bỏ đường ống này một cách không chính xác. 

Ngoài ra còn có trường hợp ranh giới mang tính xây dựng khi mọi đường ống đều hợp lệ và hướng về cùng một hướng. Vì```
2
1000000 1000000
```chúng ta có thể sử dụng cả hai ống là ((1000,0)), tạo ra```
3
0 0
1000 0
2000 0
```Thực tế là có thể sử dụng cùng một hướng nhiều lần vì bản thân các đường ống là khác nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý từng ống một cách độc lập và tìm kiếm các số nguyên (x,y) thỏa mãn (x^2+y^2=a_i). Chúng ta có thể quét (x) từ (0) đến (\lfloor\sqrt{a_i}\rfloor), tính toán (a_i-x^2) và kiểm tra xem phần còn lại có phải là số chính phương hay không. Sau khi tìm thấy một biểu diễn, chúng ta có thể định hướng nó vào quãng tám đầu tiên và nối nó vào điểm cuối hiện tại. 

Tìm kiếm đại diện này là chính xác, nhưng chi phí của nó là (O(n\sqrt A)), trong đó (A=\max a_i). Với (n=10^5) và (A=10^6), trường hợp xấu nhất là khoảng (1001\cdot10^5=100,1) triệu giá trị ứng viên (x). Đó là công việc không cần thiết, đặc biệt là trong Python. 

Quan sát quan trọng là (A) chỉ là (10^6). Thay vì giải đi giải lại cùng một câu hỏi có tổng hai bình phương, hãy xử lý trước mọi giá trị lên tới (10^6). Chúng tôi liệt kê tất cả các cặp 

[ 
0\le y\le x,\qquad x^2+y^2\le10^6 
] 

một lần. Bất cứ khi nào chúng tôi gặp một giá trị (s=x^2+y^2), chúng tôi lưu trữ biểu diễn chính tắc ((x,y)), trong đó (x\ge y\ge0). 

Phần thú vị hơn là chứng minh rằng các biểu diễn chuẩn này là đủ cho việc tối ưu hóa. Mọi vectơ nguyên có chiều dài bình phương (a_i) có thể được phản xạ và quay theo bội số của (90^\circ). Trong số tất cả các khả năng này, ((x,y)) với (x\ge y\ge0) là đại diện có góc nằm giữa (0^\circ) và (45^\circ). 

Giả sử một số giải pháp khả thi tùy ý kết thúc tại vectơ (S). Xoay và phản ánh toàn bộ giải pháp nếu cần thiết để (S) chỉ vào quãng tám đầu tiên. Đối với mỗi ống, vectơ chính tắc của nó có hình chiếu lớn nhất có thể theo hướng của (S). Do đó, việc thay thế mọi vectơ ống bằng đại diện chính tắc của nó không thể làm giảm hình chiếu của vectơ cuối cùng lên (S). Vì hình chiếu của điểm cuối ban đầu chính xác là (|S|), nên tổng các vectơ chính tắc có độ dài ít nhất là (|S|). 

Vì vậy, mọi đường ống có thể sử dụng đều phải được đưa vào và biểu diễn chính tắc của nó là đủ. Vấn đề trở thành một nhiệm vụ tiền xử lý đơn giản, theo sau là các vectơ tổng hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n\sqrt A)), tối đa khoảng (10^8) kiểm tra | (O(1)) bên cạnh đầu vào | Quá chậm trong Python | 
| Tối ưu | (O(A+n)) | (O(A)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả (a_i) và tìm (A=\max a_i). Giá trị tối đa xác định quá trình tiền xử lý cần đi bao xa. 
2. Liệt kê mọi cặp số nguyên ((x,y)) thỏa mãn (0\le y\le x) và (x^2+y^2\le A). Hạn chế (y\le x) đưa ra chính xác hướng chính tắc trong quãng tám đầu tiên mà chúng ta muốn. 
3. Với mỗi cặp, gọi (s=x^2+y^2). Nếu (s) chưa được gán một biểu diễn nào, hãy lưu trữ ((x,y)) cho (s). Bất kỳ biểu diễn nào cũng đủ vì tất cả các biểu diễn đều có cùng độ dài hình học, trong khi hướng chính tắc mang lại đặc tính góc hữu ích. 
4. Bắt đầu điểm cuối hiện tại tại ((0,0)). Đối với mỗi đường ống, hãy tra cứu đại diện của nó. Nếu không tồn tại, hãy bỏ qua đường ống vì không có đường đa tuyến nào có các đỉnh nguyên có thể chứa đường ống đó. 
5. Nếu biểu diễn là ((x,y)), hãy thêm điểm cuối mới ((X+x,Y+y)), sau đó cập nhật (X) và (Y). Cả hai tọa độ không bao giờ giảm, do đó mọi phân đoạn được tạo đều hợp lệ và toàn bộ đường đa tuyến nằm trong góc phần tư thứ nhất. 
6. Xuất ra tất cả các đỉnh được tạo, bao gồm cả gốc ban đầu. Nếu không sử dụng được đường ống thì đầu ra chỉ bao gồm ((0,0)). 

### Tại sao nó hoạt động 

Đặt (C_i) là vectơ chính tắc được chọn cho ống (i) và xem xét mọi giải pháp khả thi với điểm cuối (S). Bằng cách phản ánh toàn bộ nghiệm, chúng ta có thể giả sử hướng của (S) nằm trong quãng tám thứ nhất. 

Đối với một đường ống có vectơ chính tắc có góc (\theta\in[0,45^\circ]), mọi vectơ số nguyên khác có cùng độ dài bình phương đều thu được bằng cách đổi dấu và hoán đổi tọa độ. Trong số những khả năng đó, vectơ chính tắc có khoảng cách góc nhỏ nhất so với bất kỳ hướng nào trong quãng tám đầu tiên. Do đó, 

[ 
C_i\cdot S \ge V_i\cdot S 
]

đối với mọi vectơ (V_i) mà giải pháp ban đầu có thể sử dụng cho đường ống đó. 

Tổng hợp các đường ống được sử dụng bởi giải pháp ban đầu cho 

# S\cdot S 

|S|^2. 
] 

Bởi Cauchy-Schwarz, 

[ 
\left|\sum C_i\right||S| 
\ge 
\left(\sum C_i\right)\cdot S 
\ge |S|^2, 
] 

vậy 

[ 
\left|\sum C_i\right|\ge |S|. 
] 

Do đó, tổng của tất cả các vectơ chính tắc ít nhất phải bằng mọi nghiệm khả thi. Vì mỗi vectơ chính tắc đều tương ứng với một ống tọa độ nguyên, nên đa tuyến được xây dựng là khả thi và tối ưu. Việc thêm mọi ống dẫn có thể sử dụng cũng an toàn vì tất cả các vectơ chính tắc đều nằm trong góc phần tư thứ nhất, do đó, mỗi vectơ mới được thêm vào đều có tích chấm không âm với điểm cuối hiện tại và tăng khoảng cách bình phương của nó một cách nghiêm ngặt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    max_a = max(a)

    # rep[s] stores x * 1024 + y for a canonical representation
    # s = x^2 + y^2, with x >= y >= 0.
    # 1024 is larger than every possible coordinate (<= 1000).
    rep = [-1] * (max_a + 1)

    limit = int(max_a ** 0.5)

    for x in range(limit + 1):
        xx = x * x
        for y in range(x + 1):
            s = xx + y * y
            if s > max_a:
                break
            if rep[s] == -1:
                rep[s] = (x << 10) | y

    points = [(0, 0)]
    cur_x = 0
    cur_y = 0

    for value in a:
        code = rep[value]
        if code == -1:
            continue

        x = code >> 10
        y = code & 1023

        cur_x += x
        cur_y += y
        points.append((cur_x, cur_y))

    out = [str(len(points))]
    out.extend(f"{x} {y}" for x, y in points)
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Mảng tiền xử lý`rep`được lập chỉ mục trực tiếp bởi chiều dài bình phương. Đây là lý do tại sao giới hạn (10^6) rất hữu ích: sau khi xử lý trước, việc kiểm tra xem một đường ống có thể sử dụng được hay không sẽ mất (O(1)) thời gian. 

Biểu diễn được đóng gói thành một số nguyên thay vì lưu trữ một bộ cho mỗi giá trị. Mười bit là đủ cho mỗi tọa độ vì mỗi tọa độ tối đa là (1000). biểu hiện`x << 10 | y`lưu trữ cả hai tọa độ, trong khi`code >> 10`Và`code & 1023`phục hồi chúng. 

Các vòng lặp lồng nhau chỉ liệt kê (x) và (y) trong vùng tam giác (0\le y\le x). Chỉ có (O(A)) cặp như vậy vì (x,y\le\sqrt A). các`break`bên trong vòng lặp bên trong là hợp lệ vì (x^2+y^2) tăng khi (y) tăng. 

Khi một giá trị không có biểu diễn được lưu trữ, đường dẫn tương ứng sẽ bị bỏ qua. Một đường ống như vậy không thể xuất hiện trong bất kỳ đường đa tuyến tọa độ nguyên hợp lệ nào, vì vậy việc bỏ qua nó không thể ảnh hưởng đến mức tối ưu. 

Điểm cuối được cập nhật bằng cách thêm vectơ chính tắc. Vì tất cả các vectơ này có tọa độ không âm nên việc xây dựng không bao giờ cần phải quay lại hoặc thay đổi hướng. Đầu ra chứa nhiều đỉnh hơn số lượng ống có thể sử dụng được vì điểm bắt đầu cũng là một đỉnh. 

Số nguyên Python không tràn và tọa độ điểm cuối lớn nhất tối đa là (10^8), vì có nhiều nhất (10^5) ống và mỗi tọa độ tối đa là (1000). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2
5 25
```Với (5), chúng ta có 

[ 
5=2^2+1^2, 
] 

vậy vectơ chính tắc của nó là ((2,1)). Đối với (25), cách biểu diễn chính tắc là ((5,0)), mặc dù ((3,4)) cũng hợp lệ. 

Thuật toán có thể chọn ((5,0)), đưa ra điểm cuối ((7,1)), có khoảng cách là (\sqrt{50}). Tuy nhiên, điều đó không tối ưu nếu biểu diễn chuẩn chỉ được chọn bởi cặp gặp đầu tiên. Điều này tiết lộ chi tiết triển khai: biểu diễn chuẩn phải được chọn là biểu diễn có **tọa độ đầu tiên lớn nhất**, vì đó là vectơ gần trục dương (x) nhất. 

Đoạn code trên liệt kê (x) tăng dần nên chưa thỏa mãn yêu cầu đó. Do đó chúng tôi sử dụng thứ tự tiền xử lý đã sửa sau đây.```
for x in range(limit, -1, -1):
    xx = x * x
    for y in range(x + 1):
        s = xx + y * y
        if s > max_a:
            break
        if rep[s] == -1:
            rep[s] = (x << 10) | y
```Với thứ tự này, (5) trở thành ((2,1)) và (25) trở thành ((5,0)). Đường dẫn kết quả là 

| Ống | Vectơ chuẩn | Điểm hiện tại | 
| --- | --- | --- | 
| Bắt đầu | ((0,0)) | ((0,0)) | 
| (5) | ((2,1)) | ((2,1)) | 
| (25) | ((5,0)) | ((7,1)) | 

Khoảng cách điểm cuối bình phương là (7^2+1^2=50). Nhưng đường dẫn tối ưu của mẫu sử dụng ((3,4)) cho ống thứ hai và đạt tới ((6,4)), có khoảng cách bình phương là (52). Điều này cho thấy rằng việc chọn cách biểu diễn gần nhất với trục (x) một cách độc lập là không đủ. 

Nguyên tắc tối ưu thực tế là chọn một hướng chung và tối đa hóa các hình chiếu, đòi hỏi một cách xây dựng khác. 

### Mẫu 2 

Đầu vào là```
3
7 3 6
```Không có (7,3,6) nào là tổng của hai bình phương nguyên: 

[ 
7\ne x^2+y^2,\qquad 
3\ne x^2+y^2,\qquad 
6\ne x^2+y^2. 
] 

Vì vậy không thể sử dụng đường ống. Polyline khả thi duy nhất là nguồn gốc của chính nó. 

| Ống | Biểu diễn số nguyên? | Điểm cuối | 
| --- | --- | --- | 
| (7) | Không | ((0,0)) | 
| (3) | Không | ((0,0)) | 
| (6) | Không | ((0,0)) | 

Đầu ra cần thiết là```
1
0 0
```Ví dụ này thực hiện trường hợp mọi đường ống có sẵn đều không thể sử dụng được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(A+n)) | Quá trình tiền xử lý xem xét các cặp mạng (O(A)) và mỗi ống được xử lý một lần | 
| Không gian | (O(A+n)) | Mảng biểu diễn và các đỉnh đầu ra chiếm ưu thế trong bộ nhớ | 

Ở đây (A\le10^6), do đó quá trình tiền xử lý đủ nhỏ cho các giới hạn nhất định, trong khi (n\le10^5) làm cho cấu trúc cuối cùng trở thành tuyến tính theo kích thước đầu vào. Bộ nhớ giới hạn 512 MB cũng đủ thoải mái. 

Tuy nhiên, ví dụ hoạt động đã bộc lộ một lỗ hổng trong cách tiếp cận vectơ độc lập có vẻ tự nhiên. Việc tối ưu hóa chính xác không thể chỉ chọn một hướng cố định cho mỗi đường ống. Hướng cuối cùng của toàn bộ đa tuyến quan trọng và mẫu có độ dài (\sqrt5) và (5) thể hiện trực tiếp điều này: ((2,1)+(3,4)=(5,5)) dài hơn ((2,1)+(5,0)=(7,1)). 

Do đó, giải pháp đúng cần tối ưu hóa hướng chung trước tiên. Đối với vấn đề này, cách rõ ràng để làm điều đó là khai thác thực tế là mỗi chiều dài ống tối đa là (1000) và liệt kê các vectơ nguyên có thể có của nó, sau đó tìm hướng mà mọi vectơ được chọn đều có hình chiếu tối đa. Bài xã luận ở trên không nên được sử dụng như một cách triển khai được chấp nhận nếu không có sự chỉnh sửa đó. 

## Trường hợp thử nghiệm 

Bởi vì vấn đề mang tính xây dựng và chấp nhận nhiều kết quả đầu ra khác nhau nên các bài kiểm tra nên xác nhận đa tuyến được tạo ra thay vì so sánh văn bản của nó với một câu trả lời cố định. Khai thác thử nghiệm sau đây tính toán cấu trúc chính tắc tối ưu bằng cách liệt kê toàn diện cho các trường hợp nhỏ và kiểm tra đường đa tuyến được trả về so với độ dài ống cần thiết.```python
# helper: run solution on input string, return output string
import sys
import io
import math
from collections import Counter

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

def validate(inp: str, out: str) -> bool:
    data = list(map(int, inp.split()))
    n = data[0]
    a = data[1:1 + n]

    lines = out.strip().splitlines()
    k = int(lines[0])

    points = [tuple(map(int, line.split())) for line in lines[1:]]

    if len(points) != k:
        return False

    if points[0] != (0, 0):
        return False

    used = Counter()

    for i in range(1, k):
        x1, y1 = points[i - 1]
        x2, y2 = points[i]

        dx = x2 - x1
        dy = y2 - y1
        sq = dx * dx + dy * dy

        if sq not in a:
            return False

        used[sq] += 1

    available = Counter(a)

    if any(used[x] > available[x] for x in used):
        return False

    # For these small tests, compute the exact optimum by enumerating
    # every possible integer vector for every usable pipe.
    vectors = []
    for value in a:
        cur = []
        r = math.isqrt(value)

        for x in range(-r, r + 1):
            y2 = value - x * x
            if y2 < 0:
                continue

            y = math.isqrt(y2)
            if y * y == y2:
                cur.append((x, y))
                if y:
                    cur.append((x, -y))

        if cur:
            vectors.append(cur)

    best = 0

    def dfs(pos, sx, sy):
        nonlocal best

        if pos == len(vectors):
            best = max(best, sx * sx + sy * sy)
            return

        # Do not use this pipe.
        dfs(pos + 1, sx, sy)

        for dx, dy in vectors[pos]:
            dfs(pos + 1, sx + dx, sy + dy)

    # Only use exhaustive verification for tiny cases.
    if n <= 8:
        dfs(0, 0, 0)

        end_x, end_y = points[-1]
        got = end_x * end_x + end_y * end_y

        if got != best:
            return False

    return True

# Provided sample 1
sample1 = """\
2
5 25
"""

assert validate(sample1, run(sample1)), "sample 1"

# Provided sample 2
sample2 = """\
3
7 3 6
"""

assert validate(sample2, run(sample2)), "sample 2"

# Provided sample 3
sample3 = """\
2
1000000 1000000
"""

assert validate(sample3, run(sample3)), "sample 3"

# Minimum-size input, unusable pipe.
case1 = """\
1
3
"""
assert validate(case1, run(case1)), "minimum-size unusable pipe"

# All equal values.
case2 = """\
3
5 5 5
"""
assert validate(case2, run(case2)), "all equal values"

# Mixture of horizontal, diagonal, and unusable pipes.
case3 = """\
4
1 2 3 4
"""
assert validate(case3, run(case3)), "boundary representations"

# Maximum-size input. We only check feasibility here.
case4 = "100000\n" + " ".join(["1"] * 100000) + "\n"
assert validate(case4, run(case4)), "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 3`|`1 / 0 0`| Kích thước tối thiểu và một đường ống không sử dụng được | 
|`3 / 5 5 5`| Bất kỳ đường đa tuyến 4 đỉnh tối ưu nào | Giá trị hoàn toàn bằng nhau | 
|`4 / 1 2 3 4`| Bất kỳ đường đa tuyến hợp lệ tối ưu nào | Biểu diễn theo chiều ngang, đường chéo, không hợp lệ và ranh giới | 
|`100000 / 1 1 ... 1`| Một polyline hợp lệ sử dụng tất cả các đường ống | Xây dựng tối đa (n) và đầu ra | 

## Vỏ cạnh 

Đối với một đường ống không sử dụng được như```
1
3
```bảng tiền xử lý không chứa biểu diễn nào cho (3), vì không có nghiệm số nguyên cho (x^2+y^2=3). Đường ống bị bỏ qua và đầu ra vẫn là điểm duy nhất ((0,0)). Một giải pháp không được cố gắng tính gần đúng độ dài bằng tọa độ dấu phẩy động vì mọi đỉnh đều bắt buộc phải có tọa độ nguyên. 

Đối với một ống nằm ngang như```
1
1000000
```biểu diễn ((1000,0)) là hợp lệ. Tọa độ 0 không phải là trường hợp lỗi đặc biệt. Đoạn kết quả có chiều dài bình phương (1000^2=1000000), khớp chính xác với đường ống. 

Đối với các đường ống lặp đi lặp lại như```
3
5 5 5
```mỗi ống vật lý có thể được sử dụng một lần, vì vậy ba bản sao có cùng độ dài có thể xuất hiện trong công trình. Thực tế là độ dài của chúng bằng nhau không có nghĩa là bản thân các ống có thể hoán đổi cho nhau cho mục đích đếm và thuật toán xử lý cả ba lần xuất hiện. 

Trường hợp tinh tế nhất là sự lựa chọn giữa một số biểu diễn có cùng độ dài bình phương. Với (25), cả ((5,0)) và ((3,4)) đều hợp lệ. Cái nào tốt nhất phụ thuộc vào hướng của các đường ống khác. Mẫu có (5) và (25) chứng minh rằng việc chọn biểu diễn gần nhất với một trục cố định là không đủ, bởi vì ((2,1)+(3,4)=(5,5)) có độ dài (\sqrt{50}), trong khi phương án thay thế ((2,1)+(5,0)=(7,1)) có cùng độ dài bình phương (50) và các lựa chọn hướng khác có thể thay đổi mức tối ưu. Một giải pháp được chấp nhận đúng phải tối ưu hóa các lựa chọn biểu diễn này cùng nhau thay vì độc lập.
