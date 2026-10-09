---
title: "CF 104974I - Quận Collin"
description: "Chúng ta có một tập hợp các điểm phân biệt trên mặt phẳng 2D. Nhiệm vụ là đếm xem có bao nhiêu cách chọn bốn điểm khác nhau sao cho cả bốn đều cùng nằm trên một đường thẳng."
date: "2026-06-28T06:13:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104974
codeforces_index: "I"
codeforces_contest_name: "Codentines Day"
rating: 0
weight: 104974
solve_time_s: 87
verified: false
draft: false
---

[CF 104974I - Collin-Count](https://codeforces.com/problemset/problem/104974/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các điểm phân biệt trên mặt phẳng 2D. Nhiệm vụ là đếm xem có bao nhiêu cách chọn bốn điểm khác nhau sao cho cả bốn đều cùng nằm trên một đường thẳng. 

Nói cách khác, chúng tôi muốn đếm gấp bốn lần chỉ số$i < j < k < l$trong đó các điểm tương ứng thẳng hàng. Sự cộng tuyến ở đây có nghĩa là cả bốn điểm đều có chung một đường thẳng, không chỉ mỗi cặp đều tạo thành một sự căn chỉnh tùy ý. 

Kích thước đầu vào đủ nhỏ để chúng ta có thể xem xét lập luận bậc hai hoặc bậc ba cho mỗi điểm. Với$n \le 400$, MỘT$O(n^3)$Cách tiếp cận này đã ở mức giới hạn nhưng đôi khi có thể chấp nhận được trong C++ và chặt chẽ trong Python. MỘT$O(n^2)$hoặc$O(n^2 \log n)$giải pháp là lý tưởng. 

Một hướng đi ngây thơ là thử trực tiếp tất cả các bộ bốn, điều này sẽ$O(n^4)$, Nhưng$400^4$đã quá lớn và sẽ hết thời gian chờ. Cấu trúc cộng tuyến gợi ý rằng thay vào đó chúng ta nên nhóm các điểm theo hướng hoặc đường đồng nhất thay vì liệt kê tất cả các bộ tứ. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều điểm nằm trên cùng một đường thẳng. Ví dụ: nếu tất cả các điểm nằm trên một dòng thì câu trả lời là$\binom{n}{4}$. Bất kỳ cách tiếp cận nào chỉ tính bộ ba hoặc cặp mà không tổng hợp chính xác tất cả các điểm trên mỗi dòng sẽ bị tính thiếu hoặc đếm quá trong các cấu hình đó. 

Một trường hợp phức tạp khác là các đường thẳng đứng, trong đó việc tính toán độ dốc có thể bị hỏng do chia cho 0. Ví dụ:```
4
0 0
0 1
0 2
0 3
```Câu trả lời đúng là 1, nhưng việc nhóm dấu phẩy động dựa trên độ dốc thường sẽ thất bại trừ khi được chuẩn hóa cẩn thận. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: liệt kê từng 4 bộ điểm và kiểm tra xem chúng có thẳng hàng hay không. Việc kiểm tra tính cộng tuyến có thể được thực hiện thông qua tích chéo. Đối với bốn điểm, chúng tôi có thể xác minh rằng điểm$A, B, C$là thẳng hàng và$A, B, D$đang thẳng hàng. Điều này mang lại một$O(1)$kiểm tra theo bốn lần, nhưng có$\binom{n}{4}$gấp bốn lần như vậy, tức là về$10^9$khi$n = 400$. Như vậy là quá chậm. 

Một quan điểm tốt hơn là sửa một điểm$i$và nhìn vào tất cả các điểm khác liên quan đến nó. Mỗi dòng đi qua$i$tương ứng với một vectơ chỉ phương. Nếu chúng ta nhóm tất cả các điểm khác theo hướng chuẩn hóa của chúng từ$i$, thì bất kỳ nhóm kích thước nào$k$tương ứng với tập hợp các điểm thẳng hàng với$i$. Tuy nhiên, bộ tứ không tập trung vào một trục duy nhất; một bộ bốn hợp lệ có thể không bao gồm điểm tham chiếu đã chọn. 

Điều này dẫn đến quan sát quan trọng: mỗi bộ$k$điểm thẳng hàng góp phần$\binom{k}{4}$bốn lần hợp lệ, bất kể dòng được tìm thấy như thế nào. Vì vậy, bài toán quy về việc xác định tất cả các nhóm cộng tuyến cực đại và tính tổng đóng góp của chúng. 

Để làm điều này một cách hiệu quả, chúng ta xem xét tất cả các cặp điểm. Mỗi cặp xác định một dòng duy nhất. Nếu chúng ta có thể đếm được có bao nhiêu điểm nằm trên đường thẳng đó thì chúng ta có thể tính được đường thẳng đó đóng góp bao nhiêu phần tư. Thách thức là tránh đếm cùng một dòng nhiều lần. 

Chúng tôi giải quyết vấn đề này bằng cách mã hóa từng dòng ở dạng chuẩn hóa. Một dòng có thể được biểu diễn dưới dạng$Ax + By + C = 0$, Ở đâu$A, B, C$là các số nguyên dẫn xuất từ ​​hai điểm. Chúng tôi chuẩn hóa cách biểu diễn này để tất cả các dòng tương đương ánh xạ tới cùng một khóa. Sau đó, chúng ta đếm xem có bao nhiêu điểm nằm trên mỗi đường biểu diễn bằng cách kiểm tra tất cả các cặp và tăng dần. 

Điều này cung cấp cho chúng ta một bản đồ từ đường thẳng → tập hợp các điểm, được xây dựng ngầm thông qua xử lý cặp. Cuối cùng, với mỗi dòng có$k$điểm hỗ trợ, chúng tôi thêm$\binom{k}{4}$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force tăng gấp bốn lần |$O(n^4)$|$O(1)$| Quá chậm | 
| Nhóm dòng theo cặp |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mọi cặp điểm không có thứ tự$(i, j)$, hãy tính đường đi qua chúng dưới dạng biểu diễn số nguyên đã chuẩn hóa. Điều này đảm bảo tất cả các cặp trên cùng một đường hình học tạo ra các khóa giống hệt nhau. 
2. Duy trì một từ điển từ biểu diễn dòng đến một tập hợp các điểm (hoặc cấu trúc bộ đếm tăng dần). Đối với mỗi cặp$(i, j)$, thêm cả hai điểm cuối vào tập hợp được liên kết với dòng đó. Điều này đảm bảo cuối cùng chúng tôi sẽ phục hồi được tất cả các điểm nằm trên mỗi dòng. 
3. Sau khi xử lý tất cả các cặp, lặp lại tất cả các dòng được lưu trữ và tính số điểm phân biệt trên mỗi dòng. Hãy để điều này được$k$. 
4. Với mỗi dòng, hãy thêm$\binom{k}{4} = \frac{k(k-1)(k-2)(k-3)}{24}$để trả lời. 
5. Xuất số tiền tích lũy. 

Lý do bước 2 sử dụng các bộ thay vì chỉ đếm các cặp là vì một dòng có nhiều điểm tạo ra nhiều cặp và chúng ta cần loại bỏ các điểm trùng lặp để khôi phục số lượng phần tử thực sự của dòng. 

### Tại sao nó hoạt động 

Mọi tứ giác thẳng hàng đều nằm trên đúng một đường hình học. Khi chúng ta nhóm các điểm theo cách biểu diễn đường duy nhất của chúng, mỗi bộ tứ hợp lệ sẽ được tính chính xác một lần vì nó thuộc về đúng một nhóm đường. Trong một dòng có chứa$k$điểm, tất cả các tập hợp con có kích thước 4 đều hợp lệ và không phụ thuộc vào thứ tự, do đó tính tổng$\binom{k}{4}$trên tất cả các dòng liệt kê chính xác tất cả các bộ tứ hợp lệ mà không bị trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import gcd
from collections import defaultdict

def norm_line(x1, y1, x2, y2):
    A = y2 - y1
    B = x1 - x2
    C = A * x1 + B * y1

    g = gcd(gcd(abs(A), abs(B)), abs(C))
    if g:
        A //= g
        B //= g
        C //= g

    if A < 0 or (A == 0 and B < 0):
        A, B, C = -A, -B, -C

    return (A, B, C)

def comb4(k):
    if k < 4:
        return 0
    return k * (k - 1) * (k - 2) * (k - 3) // 24

def solve():
    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    lines = defaultdict(set)

    for i in range(n):
        x1, y1 = pts[i]
        for j in range(i + 1, n):
            x2, y2 = pts[j]
            key = norm_line(x1, y1, x2, y2)
            lines[key].add(i)
            lines[key].add(j)

    ans = 0
    for s in lines.values():
        k = len(s)
        ans += comb4(k)

    print(ans)

if __name__ == "__main__":
    solve()
```Chi tiết triển khai cốt lõi là chuẩn hóa cách biểu diễn dòng. các hệ số$A, B, C$được giảm đi bởi ước số chung lớn nhất của chúng sao cho các đường hình học giống hệt nhau thu gọn vào cùng một khóa từ điển. Việc chuẩn hóa dấu hiệu đảm bảo rằng cùng một dòng không được biểu thị hai lần với các dấu hiệu trái ngược nhau. 

Chúng tôi sử dụng một bộ trên mỗi dòng để tránh các điểm đếm kép đến từ nhiều cặp trên cùng một dòng. Điều này rất cần thiết vì mỗi dòng có$k$điểm tạo ra$\binom{k}{2}$các cặp, nhưng chúng tôi chỉ muốn số điểm khác biệt cuối cùng chứ không phải bội số của cặp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 1
2 2
3 3
4 4
5 5
```Tất cả các điểm nằm trên một đường chéo. 

| Đã xử lý cặp | Phím dòng | Đặt cập nhật kích thước | 
| --- | --- | --- | 
| (1,2) | cùng dòng | {1,2} | 
| (1,3) | cùng dòng | {1,2,3} | 
| (1,4) | cùng dòng | {1,2,3,4} | 
| (1,5) | cùng dòng | {1,2,3,4,5} | 

Kích thước tập hợp cuối cùng là 5. 

Đóng góp là$\binom{5}{4} = 5$. 

Điều này xác nhận rằng một khi tất cả các điểm được tổng hợp dưới một dòng chuẩn hóa duy nhất, việc đếm sẽ giảm chính xác thành lựa chọn tổ hợp. 

### Ví dụ 2 

đầu vào:```
4
1 1
1 2
2 1
2 2
```Không có ba điểm nào thẳng hàng nên không có đường thẳng nào chứa 4 điểm. 

| Đã xử lý cặp | Phím dòng | Đặt kích thước | 
| --- | --- | --- | 
| (1,2) | dọc | 2 | 
| (1,3) | ngang-ish | 2 | 
| (2,4) | đường chéo | 2 | 

Tất cả các nhóm dòng đều có kích thước 2, vì vậy mọi$\binom{2}{4} = 0$. 

Câu trả lời cuối cùng là 0. 

Điều này cho thấy việc nhóm các cặp ngẫu nhiên không ảnh hưởng đến kết quả trừ khi một đường tích lũy ít nhất 4 điểm phân biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Mỗi cặp điểm được xử lý một lần và các phần chèn vào được khấu hao không đổi | 
| Không gian |$O(n^2)$| Trong trường hợp xấu nhất, mỗi cặp đóng góp vào một mục nhập dòng | 

Với$n \le 400$, tổng số cặp tối đa là 160.000, vừa vặn thoải mái trong giới hạn thời gian trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import gcd
    from collections import defaultdict

    def norm_line(x1, y1, x2, y2):
        A = y2 - y1
        B = x1 - x2
        C = A * x1 + B * y1
        g = gcd(gcd(abs(A), abs(B)), abs(C))
        if g:
            A //= g
            B //= g
            C //= g
        if A < 0 or (A == 0 and B < 0):
            A, B, C = -A, -B, -C
        return (A, B, C)

    def comb4(k):
        return k * (k - 1) * (k - 2) * (k - 3) // 24 if k >= 4 else 0

    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]
    lines = defaultdict(set)

    for i in range(n):
        x1, y1 = pts[i]
        for j in range(i + 1, n):
            x2, y2 = pts[j]
            key = norm_line(x1, y1, x2, y2)
            lines[key].add(i)
            lines[key].add(j)

    return str(sum(comb4(len(s)) for s in lines.values()))

assert run("5\n1 1\n2 2\n3 3\n4 4\n5 5\n") == "5", "all collinear"
assert run("4\n1 1\n1 2\n2 1\n2 2\n") == "0", "grid no collinearity"
assert run("4\n0 0\n1 1\n2 2\n3 3\n") == "1", "single quadruple"
assert run("5\n0 0\n1 0\n2 0\n3 0\n0 1\n") == "1", "one line of 4 points"
assert run("3\n0 0\n1 1\n2 3\n") == "0", "insufficient points"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các điểm trên một đường | 5 | vụ nổ tổ hợp đầy đủ | 
| điểm lưới | 0 | không có sự cộng tác ngẫu nhiên | 
| 4 điểm thẳng hàng | 1 | trường hợp hợp lệ tối thiểu | 
| 5 điểm với một dòng 4 | 1 | cấu trúc hỗn hợp | 
| 3 điểm | 0 | ranh giới dưới ngưỡng | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi có nhiều điểm nằm trên cùng một đường thẳng đứng. Ví dụ:```
4
0 0
0 1
0 2
0 3
```Mỗi cặp tạo ra một đường thẳng đứng có biểu diễn chuẩn hóa giống hệt nhau. Tập hợp tích lũy tất cả bốn điểm và thuật toán tính toán$\binom{4}{4} = 1$, phù hợp với câu trả lời đúng. Cách tiếp cận dựa trên độ dốc mà không chuẩn hóa sẽ thất bại ở đây do chia cho 0. 

Một trường hợp khác là khi các điểm tạo thành nhiều đường chồng chéo, chia sẻ các điểm nhưng không phải tất cả đều thẳng hàng với nhau. Việc phân nhóm dựa trên tập hợp đảm bảo mỗi đường hình học được xử lý độc lập và các đóng góp chồng chéo không gây trở ngại vì các bộ tứ chỉ được tính trong mỗi nhóm đường riêng biệt.
