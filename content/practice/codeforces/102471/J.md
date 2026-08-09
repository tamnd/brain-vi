---
title: "CF 102471J - Hoán vị"
description: "Chúng ta có hoán vị của các số từ 1 đến n và số nguyên c. Một thao tác xem xét chính xác c+1 vị trí liên tiếp. Nếu giá trị nhỏ nhất trong khoảng đó nằm ở một điểm cuối thì điểm cuối đó được giữ cố định và các giá trị c khác có thể được sắp xếp lại tùy ý."
date: "2026-08-09T18:42:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102471
codeforces_index: "J"
codeforces_contest_name: "2019 ICPC Asia-East Continent Final"
rating: 0
weight: 102471
solve_time_s: 450
verified: true
draft: false
---

[CF 102471J - Hoán vị](https://codeforces.com/problemset/problem/102471/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hoán vị của các số từ`1`ĐẾN`n`, và một số nguyên`c`. Một hoạt động xem xét chính xác`c+1`các vị trí liên tiếp. Nếu giá trị nhỏ nhất trong khoảng đó nằm ở một điểm cuối thì điểm cuối đó được giữ cố định và điểm cuối kia`c`các giá trị có thể được sắp xếp lại tùy ý. 

Nhiệm vụ không phải là tìm một hoán vị có thể tiếp cận được. Chúng ta cần tổng số hoán vị riêng biệt có thể đạt được sau khi áp dụng các phép toán đó bao nhiêu lần, với kết quả được lấy modulo`998244353`. Tuyên bố chính thức sử dụng hai trường hợp điểm cuối giống nhau, một trường hợp có mức tối thiểu ở điểm cuối bên trái và một trường hợp có mức tối thiểu ở điểm cuối bên phải. 

Vai trò quyết định thuộc về vị trí của`1`. Từ`1`là nhỏ nhất trên toàn cầu, nó không bao giờ có thể được di chuyển bằng một phép toán. Bất kỳ hoạt động nào có chứa`1`phải có`1`làm điểm cuối của nó. Do đó, các hoạt động sử dụng`1`chỉ có thể sắp xếp lại`c`vị trí ngay bên trái của nó hoặc`c`vị trí ngay bên phải của nó. 

Hãy để có`L`các phần tử bên trái của`1`Và`R`các phần tử ở bên phải của nó. Các phép toán ở hai bên không bao giờ phải tương tác, vì vậy tổng số hoán vị có thể tiếp cận là tích của số có thể đạt được từ phía bên trái và số có thể đạt được từ phía bên phải. 

Các ràng buộc làm cho thuật toán hàm mũ hoặc giai thừa không thể thực hiện được. Mặc dù tuyên bố cho phép`n`lên đến`500000`, tổng của`n`trên tất cả các trường hợp thử nghiệm cũng là nhiều nhất`500000`. Điều này có nghĩa là giải pháp dự định về cơ bản phải tuyến tính trong tổng kích thước đầu vào. Thậm chí`O(n log n)`ở đây là không cần thiết, trong khi bất cứ điều gì liên quan đến`n!`hoán vị có thể ngay lập tức là không thể. 

Có một số trường hợp ranh giới dễ dàng mà việc triển khai bất cẩn có thể xử lý sai. Ví dụ, hãy xem xét```
1
2 2
1 2
```Không có khoảng thời gian`3`, nên không thể thực hiện được thao tác nào. Câu trả lời là`1`. Một giải pháp xử lý các`c`vị trí bên cạnh`1`có thể hoán vị tự do ngay cả khi ít hơn`c`vị trí tồn tại sẽ trở lại không chính xác`2`. 

Một trường hợp quan trọng khác là```
1
3 2
2 1 3
```giá trị`1`nằm ở giữa nên chỉ có một phần tử ở hai bên. Không bên nào chứa yêu cầu`c=2`các phần tử. Một lần nữa câu trả lời là`1`. Sự thật là`1`có các yếu tố lân cận là không đủ, toàn bộ nhóm`c`yếu tố di chuyển phải tồn tại. 

Mặt khác,```
1
3 2
1 2 3
```có hai phần tử ở bên phải`1`. Toàn bộ cặp có thể được hoán vị, vì vậy câu trả lời là`2`. Đây là ví dụ nhỏ nhất trong đó một hoạt động thực sự tồn tại. 

Đầu vào được đảm bảo là một hoán vị, do đó phép thử "tất cả các giá trị bằng nhau" không phải là phép thử hợp lệ cho vấn đề này. Đầu vào như vậy sẽ vi phạm định nghĩa về hoán vị. Việc triển khai phải dựa vào các giá trị riêng biệt và không nên cố gắng hỗ trợ các bản sao trong trường hợp đặc biệt. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thực hiện tìm kiếm theo chiều rộng trên các hoán vị có thể tiếp cận. Đối với mỗi hoán vị hiện tại, hãy kiểm tra từng khoảng thời gian`c+1`. Bất cứ khi nào mức tối thiểu ở điểm cuối, hãy tạo mọi hoán vị của điểm khác`c`các phần tử và chèn các mảng kết quả vào một tập hợp đã truy cập. 

Điều này đúng vì mọi hoạt động hợp pháp đều được liệt kê rõ ràng và BFS tiếp tục cho đến khi không còn hoán vị không nhìn thấy được. Vấn đề là kích thước của không gian trạng thái. có thể có`n!`hoán vị khác nhau và thậm chí kiểm tra tất cả các khoảng cho mọi trạng thái đã có giá`O(n · n!)`. Nếu mọi sự sắp xếp lại có thể có bên trong một thao tác được tạo ra một cách rõ ràng thì công việc chuyển đổi sẽ chứa một hệ số khác lên tới`c!`. Với`n`đạt tới`500000`, điều này không khả thi chút nào. 

Cấu trúc xung quanh`1`đưa ra một mô tả nhỏ hơn nhiều. Từ`1`không bao giờ di chuyển, chỉ xem xét một mặt của nó. Giả sử cạnh này chứa`m`các phần tử được sắp xếp từ vị trí gần nhất`1`về phía bên ngoài. 

đầu tiên`c`vị trí có thể được hoán vị bất cứ khi nào chúng tồn tại. Trong số này`c`vị trí, một phần tử được phân biệt: phần tử nhỏ nhất. Nó có thể di chuyển bên trong những`c`vị trí, nhưng nó không thể thoát khỏi chúng. Khi một nhóm hoàn chỉnh khác của`c`vị trí trở nên có sẵn, lý do tương tự sẽ tạo ra một yếu tố phân biệt khác có vị trí bị giới hạn ở vị trí đầu tiên`2c`các vị trí của bên. Tiếp tục hướng ra ngoài tạo ra một phần tử bị hạn chế cho mỗi nhóm hoàn chỉnh`c`các vị trí. 

Như vậy, nếu 

[ 
k=\left\lfloor\frac{m}{c}\right\rfloor, 
] 

có chính xác`k`hạn chế lồng nhau. các`j`-phần tử bị hạn chế thứ phải chiếm một trong những phần tử đầu tiên`jc`các vị trí. Khi chúng tôi đếm các phần tử bị hạn chế này từ tiền tố nhỏ nhất được phép đến lớn nhất,`j-1`các vị trí đã bị chiếm giữ bởi các phần tử bị hạn chế trước đó. Do đó`j`- phần tử thứ có 

[ 
jc-(j-1)=j(c-1)+1 
] 

sự lựa chọn. 

Rốt cuộc`k`các yếu tố hạn chế đã được đặt, phần còn lại`m-k`các phần tử không còn hạn chế nào và có thể được sắp xếp tùy ý. Đóng góp của họ là`(m-k)!`. 

Vì vậy sự đóng góp của một bên chứa`m`phần tử là 

[ 
F(m,c)= 
(m-k)!\prod_{j=1}^{k}\left(jc-j+1\right), 
\qquad 
k=\left\lfloor\frac{m}{c}\right\rfloor. 
]

Nếu như`m<c`, sau đó`k=0`, cho`F(m,c)=m!`. Tuy nhiên, biểu thức này sẽ sai đối với các quy tắc phép toán thực tế vì không có phép toán nào tồn tại khi ít hơn`c`các phần tử có mặt. Trong trường hợp đó sự đóng góp chính xác là`1`. Vì vậy chúng tôi sử dụng 

[ 
F(m,c)= 
\bắt đầu{trường hợp} 
1,&m<c,\ 
(m-k)!\displaystyle\prod_{j=1}^{k}(jc-j+1),&m\ge c. 
\end{trường hợp} 
] 

Câu trả lời cuối cùng chỉ đơn giản là 

[ 
F(L,c)\cdot F(R,c). 
] 

Lực lượng vũ phu hoạt động vì nó khám phá rõ ràng cùng một không gian trạng thái có thể tiếp cận mà đối số tổ hợp mô tả. Sự quan sát về`1`chúng ta hãy thay thế không gian trạng thái khổng lồ đó bằng hai cạnh độc lập và một số hạn chế về vị trí lồng nhau. Đây là quan sát cấu trúc tương tự được nhấn mạnh trong cuộc thảo luận về giải pháp được công bố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n · n! · c!)`trong phiên bản chuyển tiếp rõ ràng |`O(n!)`| Quá chậm | 
| Tối ưu |`O(n)`mỗi trường hợp thử nghiệm |`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tìm vị trí của`1`. Nếu nó ở vị trí gốc 0`pos`, thì có`L=pos`các phần tử ở bên trái của nó và`R=n-1-pos`các phần tử ở bên phải của nó. Từ`1`không bao giờ có thể di chuyển, hai bên này có thể được tính độc lập. 
2. Tính toán trước giai thừa modulo`998244353`lên đến lớn nhất`n`. Công thức bên cuối cùng chứa`(m-k)!`, vì vậy cần có các giá trị giai thừa để truy cập theo thời gian liên tục. 
3. Xác định phần đóng góp của độ dài một cạnh`m`. Nếu như`m<c`, trở lại`1`, vì không có đủ phần tử để tạo thành một phép toán chứa`1`Và`c`các yếu tố di chuyển được. 
4. Nếu không thì đặt`k=m//c`. có`k`các phần tử bị hạn chế lồng nhau, bởi vì mọi nhóm hoàn chỉnh của`c`vị trí tạo thêm một tiền tố trong đó phần tử phân biệt được phép di chuyển. 
5. Bắt đầu đóng góp phụ với`(m-k)!`. Đây là những phần tử vẫn hoàn toàn không bị hạn chế sau khi tất cả các phần tử bị hạn chế đã được tính đến. 
6. Đối với mọi`j`từ`1`bởi vì`k`, nhân với`jc-j+1`. các`j`- phần tử bị hạn chế thứ có`jc`các vị trí có thể có trong tiền tố được phép của nó, nhưng`j-1`trong số những vị trí đó đã bị chiếm giữ bởi các phần tử bị hạn chế đã được xử lý trước đó. 
7. Tính phần đóng góp của vế trái và vế phải một cách độc lập rồi nhân chúng theo modulo`998244353`. Hai bên không tranh giành vị trí vì`1`ngăn cách họ vĩnh viễn. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi phơi bày`j`các nhóm hoàn chỉnh`c`vị trí ở một bên, chính xác`j`các yếu tố phân biệt có vị trí hạn chế, và`j`-thứ được phép ở bất cứ đâu trong cái đầu tiên`jc`các vị trí. Các phần tử được phân biệt trước đó vẫn nằm trong các tiền tố đó, trong khi mọi phần tử khác trong vùng được hiển thị có thể được sắp xếp lại một cách tự do. 

Bởi vì các tiền tố được phép này được lồng nhau nên các phần tử bị hạn chế có thể được đặt lần lượt. Ở sân khấu`j`, chính xác`j-1`vị trí bên trong đầu tiên`jc`các vị trí đã bị chiếm giữ bởi các phần tử bị hạn chế trước đó, để lại`jc-j+1`sự lựa chọn. Một lần tất cả`k`các phần tử bị hạn chế được đặt, mọi phần tử khác không bị hạn chế, mang lại`(m-k)!`sắp xếp. 

Mọi cách sắp xếp có thể truy cập đều thỏa mãn những hạn chế này vì một thao tác giữ cho điểm cuối tối thiểu cố định. Ngược lại, các hoạt động có thể thực hiện mọi sự sắp xếp thỏa mãn các hạn chế lồng nhau bằng cách lần lượt đưa ra nhóm tiếp theo của`c`vị trí và hoán vị tất cả các vị trí hiện đang miễn phí. Do đó, công thức tính mọi hoán vị có thể truy cập chính xác một lần. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

MOD = 998244353

def side_ways(m, c, fact):
    if m < c:
        return 1

    k = m // c
    ans = fact[m - k]

    for j in range(1, k + 1):
        ans = ans * (j * c - j + 1) % MOD

    return ans

def solve():
    t = int(input())

    tests = []
    max_n = 0

    for _ in range(t):
        n, c = map(int, input().split())
        p = list(map(int, input().split()))
        tests.append((n, c, p))
        max_n = max(max_n, n)

    fact = [1] * (max_n + 1)
    for i in range(1, max_n + 1):
        fact[i] = fact[i - 1] * i % MOD

    out = []

    for n, c, p in tests:
        pos = p.index(1)

        left = pos
        right = n - 1 - pos

        ans_left = side_ways(left, c, fact)
        ans_right = side_ways(right, c, fact)

        ans = ans_left * ans_right % MOD
        out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc tính toán trước giai thừa được thực hiện một lần cho tất cả các trường hợp thử nghiệm. Điều này quan trọng vì có thể có tới`100000`các trường hợp thử nghiệm, mặc dù tổng chiều dài của chúng chỉ`500000`. 

Vị trí của`1`được tìm thấy với`p.index(1)`. Với lập chỉ mục dựa trên số 0, vị trí đó chính xác là số phần tử ở bên trái của nó, trong khi`n-1-pos`là số ở bên phải của nó 

các`m<c`kiểm tra là cần thiết. Công thức với`k=0`sẽ cho`m!`, nhưng khi nhỏ hơn`c`các yếu tố tồn tại, không có khoảng thời gian pháp lý liên quan đến`1`, do đó cạnh hoàn toàn không thay đổi và đóng góp chính xác`1`. 

Vì`m>=c`,`k=m//c`đếm các nhóm hoàn chỉnh của`c`các vị trí. Vòng lặp nhân`k`yếu tố`jc-j+1`. Tổng số lần lặp của nó trên cả hai mặt là`O(n/c)`đối với một trường hợp thử nghiệm, nhiều nhất là`O(n)`. 

Số nguyên Python không bị tràn, nhưng mỗi phép nhân đều được giảm modulo`998244353`. Mảng giai thừa cũng lưu trữ các giá trị theo mô đun tương tự. 

Đầu vào được đọc bằng cách sử dụng`sys.stdin.readline`, theo yêu cầu đối với tổng kích thước đầu vào lớn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Trường hợp thử nghiệm đầu tiên là```
5 3
3 4 2 1 5
```giá trị`1`đang ở vị trí`4`trong việc lập chỉ mục một cơ sở. Như vậy có`3`các phần tử ở bên trái của nó và`1`ở bên phải của nó. 

Đối với phía bên trái,`m=3`Và`c=3`, Vì thế`k=1`. 

| Bên |`m`|`c`|`k`| Phần giai thừa | Các yếu tố hạn chế | Đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| Trái | 3 | 3 | 1 |`2! = 2`|`3-1+1 = 3`|`6`| 
| Đúng | 1 | 3 | 0 | không được sử dụng | không |`1`| 

Câu trả lời cuối cùng là`6*1=6`. 

Phía bên trái bao gồm chính xác`c`các phần tử, do đó hoạt động xung quanh`1`có thể tùy ý hoán vị ba phần tử đó. có`3!=6`khả năng. 

### Mẫu 2 

Trường hợp thử nghiệm thứ hai là```
5 4
4 2 1 3 5
```Đây`1`đang ở vị trí`3`. Cả hai vế đều chứa đúng hai phần tử, nhưng`c=4`. 

| Bên |`m`|`c`|`m<c`| Đóng góp | 
| --- | --- | --- | --- | --- | 
| Trái | 2 | 4 | vâng |`1`| 
| Đúng | 2 | 4 | vâng |`1`| 

Không có khoảng thời gian`5`chứa đựng`1`tồn tại, vì vậy không có hoạt động nào có thể liên quan đến`1`. Độ dài có thể khác`5`khoảng là toàn bộ mảng, có mức tối thiểu là`1`ở giữa nên nó cũng không hợp lệ. Câu trả lời là`1`. 

Hai mẫu này thể hiện cả hai mặt của ranh giới chính: có chính xác`c`các yếu tố mang lại sự tự do, trong khi có ít hơn`c`không cho gì cả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n)`mỗi trường hợp thử nghiệm,`O(sum n)`tổng thể | Tìm kiếm`1`là tuyến tính và các vòng bên cùng nhau sử dụng hầu hết công việc tuyến tính | 
| Không gian |`O(n)`| Mảng hoán vị và giai thừa sử dụng bộ nhớ tuyến tính | 

Tổng cộng`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`500000`, do đó, việc tính toán trước giai thừa và tất cả các trường hợp kiểm thử hoạt động cùng nhau là`O(500000)`. Việc sử dụng bộ nhớ cũng tuyến tính và phù hợp thoải mái với`256 MB`giới hạn. Vấn đề chính thức chỉ định tương tự`500000`tổng hợp ràng buộc và`998244353`mô đun. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 998244353

def reference(inp: str) -> str:
    data = inp.split()
    it = iter(data)

    t = int(next(it))
    tests = []
    max_n = 0

    for _ in range(t):
        n = int(next(it))
        c = int(next(it))
        p = [int(next(it)) for _ in range(n)]
        tests.append((n, c, p))
        max_n = max(max_n, n)

    fact = [1] * (max_n + 1)
    for i in range(1, max_n + 1):
        fact[i] = fact[i - 1] * i % MOD

    def side(m, c):
        if m < c:
            return 1

        k = m // c
        ans = fact[m - k]

        for j in range(1, k + 1):
            ans = ans * (j * c - j + 1) % MOD

        return ans

    ans = []

    for n, c, p in tests:
        pos = p.index(1)
        left = pos
        right = n - pos - 1

        ans.append(str(side(left, c) * side(right, c) % MOD))

    return "\n".join(ans) + "\n"

# Provided samples.
sample = """\
5
5 3
3 4 2 1 5
5 4
4 2 1 3 5
5 2
4 5 3 1 2
5 3
4 3 2 1 5
5 2
2 3 1 5 4
"""

assert reference(sample) == "6\n1\n4\n6\n4\n", "provided samples"

# Minimum-size case. No interval of length c+1 exists.
assert reference("""\
1
2 2
1 2
""") == "1\n", "minimum n"

# 1 in the middle, so neither side contains c elements.
assert reference("""\
1
3 2
2 1 3
""") == "1\n", "insufficient elements on both sides"

# Exactly c elements on one side can be permuted.
assert reference("""\
1
3 2
1 2 3
""") == "2\n", "exactly one active side"

# Both sides contain exactly c elements.
assert reference("""\
1
5 2
2 3 1 5 4
""") == "4\n", "two independent sides"

# Four elements on one side with c = 2.
# The side contribution is:
# (4 - 2)! * 2 * 3 = 12.
assert reference("""\
1
5 2
1 2 3 4 5
""") == "12\n", "multiple nested restrictions"

# Maximum-size n with c = n.
# No side can contain c elements, so the answer is 1.
n = 500000
p = list(range(1, n + 1))
large_input = "1\n{} {}\n{}\n".format(n, n, " ".join(map(str, p)))
assert reference(large_input) == "1\n", "maximum-size boundary"

# Duplicate values are deliberately not tested:
# [1, 1, 2] is not a valid permutation for this problem.
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 / 1 2`|`1`| Kích thước tối thiểu và không có bất kỳ khoảng thời gian pháp lý nào | 
|`3 2 / 2 1 3`|`1`|`1`ở giữa có cả hai bên quá ngắn | 
|`3 2 / 1 2 3`|`2`| Chính xác`c`các yếu tố ở một bên | 
|`5 2 / 2 3 1 5 4`|`4`| Hai bên hoạt động độc lập | 
|`5 2 / 1 2 3 4 5`|`12`| Nhiều hạn chế lồng nhau | 
|`500000 500000 / 1 2 ... 500000`|`1`| Tối đa`n`và`c=n`ranh giới | 

Không thể bao gồm kiểm tra hoàn toàn bằng nhau được yêu cầu vì đầu vào được đảm bảo là một hoán vị, do đó, các giá trị lặp lại sẽ làm cho kiểm tra không hợp lệ thay vì thực hiện một trường hợp đặc biệt của thuật toán. 

## Vỏ cạnh 

Khi một bên chứa ít hơn`c`các yếu tố, không có hoạt động liên quan đến`1`có thể sử dụng bên đó. Ví dụ,```
1
2 2
1 2
```có một phần tử ở bên phải, nhỏ hơn`c=2`. Phần đóng góp bên cạnh là`1`, và không có khoảng hợp lệ nào khác. Thuật toán đi vào`m<c`chi nhánh và lợi nhuận`1`. 

Khi`1`hoàn toàn nằm trong hoán vị và cả hai bên đều ngắn hơn`c`, không bên nào có thể tham gia. Vì```
1
3 2
2 1 3
```cả hai bên đều có chiều dài`1`. Thuật toán tính toán`F(1,2)=1`hai lần, sản xuất`1`. 

Khi một bên có chính xác`c`các phần tử thì các phần tử đó có thể được hoán vị tùy ý. Vì```
1
3 2
1 2 3
```phía bên phải có chiều dài`2`. Đây`k=1`, vậy phần đóng góp là 

[ 
(2-1)!\cdot(2-1+1)=1\cdot2=2. 
] 

Phía bên trái đóng góp`1`, đưa ra câu trả lời cuối cùng`2`. 

Khi một bên dài hơn`c`, các hạn chế lồng nhau bổ sung sẽ xuất hiện. Vì`m=4,c=2`, chúng tôi có`k=2`. Sự đóng góp là 

[ 
(4-2)!\cdot2\cdot3 
=2\cdot2\cdot3 
=12. 
] 

Phần tử bị hạn chế đầu tiên có`2`vị trí có thể, trong khi vị trí thứ hai có`4-1=3`các lựa chọn sau khi phần tử bị hạn chế đầu tiên được đặt. Hai yếu tố còn lại không bị hạn chế và góp phần`2!`. 

Khi`c=n`, không bên nào có thể chứa được`c`các phần tử vì kích thước kết hợp của chúng chỉ`n-1`. Vì vậy mỗi trường hợp thử nghiệm với`c=n`có câu trả lời`1`. Thử nghiệm kích thước tối đa với`n=c=500000`thực hiện chính xác ranh giới này và được xử lý bởi`m<c`nhánh ở cả hai bên.
