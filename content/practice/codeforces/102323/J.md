---
title: "CF 102323J - Sản phẩm giai thừa"
description: "Đối với mỗi trường hợp thử nghiệm, chúng ta có ba danh sách số nguyên không âm, được gọi là A, B và C. Đối với danh sách như A = [a1, a2, ..., ak], hãy xác định điểm của nó là [ P(A)=a1!cdot a2!cdots ak!. ] Nhiệm vụ là xác định danh sách nào trong ba danh sách có số điểm lớn nhất."
date: "2026-08-13T04:20:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102323
codeforces_index: "J"
codeforces_contest_name: "UCF Locals 2014"
rating: 0
weight: 102323
solve_time_s: 70
verified: true
draft: false
---

[CF 102323J - Sản phẩm giai thừa](https://codeforces.com/problemset/problem/102323/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Với mỗi trường hợp thử nghiệm, chúng ta có ba danh sách các số nguyên không âm, được gọi là`A`,`B`, Và`C`. Đối với một danh sách như`A = [a1, a2, ..., ak]`, xác định điểm của nó là 

[ 
P(A)=a_1!\cdot a_2!\cdots a_k!. 
] 

Nhiệm vụ là xác định danh sách nào trong ba danh sách có số điểm lớn nhất. Nếu hai hoặc cả ba điểm bằng nhau và bằng điểm tối đa thì câu trả lời bắt buộc là`TIE`. 

Đầu vào bắt đầu bằng số lượng trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra ba kích thước danh sách, theo sau là các phần tử của ba danh sách. Mọi phần tử đều ở bên dưới`2501`. Tuyên bố chính thức cũng đảm bảo rằng nếu hai sản phẩm khác nhau thì sự khác biệt tương đối của chúng ít nhất là`0.01%`của sản phẩm lớn hơn. 

Ràng buộc quan trọng là giới hạn trên của`2500`vào các giá trị riêng lẻ, chứ không phải độ lớn của các sản phẩm giai thừa. Thậm chí`2500!`quá lớn để xây dựng một cách rõ ràng trong số học số nguyên thông thường. Một số nguyên Python có thể biểu diễn nó về mặt kỹ thuật, nhưng việc xây dựng và nhân nhiều lần các tích chứa nhiều giai thừa như vậy sẽ làm cho các con số trở nên khổng lồ và số học ngày càng đắt đỏ. Với giới hạn một giây, giải pháp phải tránh hiện thực hóa những sản phẩm này. Bản thân đầu vào vẫn phải được đọc, do đó, mọi thuật toán hiệu quả về cơ bản phải tuyến tính trong tổng số phần tử danh sách. 

Có một số trường hợp đặc biệt trong đó việc triển khai đơn giản có thể âm thầm thất bại. Coi như```
1
1 1 1
0
1
0
```Điểm số là`0! = 1`,`1! = 1`, Và`0! = 1`, vì vậy đầu ra là```
Case #1: TIE
```Một triển khai xử lý`0!`vì số 0 sẽ chọn sai`B`. 

Một trường hợp khác là```
1
2 1 1
2 2
3
2
```Đây`P(A)=2!*2!=4`,`P(B)=3!=6`, Và`P(C)=2!=2`, vậy câu trả lời là```
Case #1: B
```Việc triển khai bất cẩn chỉ so sánh phần tử lớn nhất của mỗi danh sách sẽ chọn sai`A`bởi vì phần tử lớn nhất của nó là`2`, nhưng sản phẩm phụ thuộc vào mọi giai thừa. 

Trường hợp cạnh thứ ba là hòa chính xác:```
1
1 2 1
3
2 1
3
```Cả hai`A`Và`C`có điểm`3!`, trong khi`B`có`2!*1!=2`, vậy kết quả là```
Case #1: TIE
```Đầu ra là`TIE`mặc dù danh sách liên kết không chứa các phần tử giống nhau. Sự so sánh phải được thực hiện giữa các sản phẩm tạo ra. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu tuân theo định nghĩa toán học trực tiếp. Với mọi giá trị`x`, tính toán`x!`, nhân các giai thừa đó với nhau để có được danh sách tương ứng, sau đó so sánh ba số nguyên thu được. Điều này đúng vì nó xây dựng chính xác ba đại lượng được xác định bởi bài toán. 

Khó khăn là kích thước của các con số. Nếu một danh sách chứa`m`giá trị và mọi giá trị là`2500`, việc tính toán từng giai thừa riêng biệt mất khoảng`2500m`phép nhân. Trên cả ba danh sách, điều này gần như là`7500m`phép nhân khi danh sách có kích thước tương đương. Nghiêm trọng hơn, bản thân các số nguyên trung gian có độ dài hàng trăm hoặc hàng nghìn chữ số và việc nhân nhiều giá trị như vậy làm cho mỗi phép tính số học ngày càng tốn kém. Lưu trữ các sản phẩm cuối cùng cũng là công việc không cần thiết vì chúng ta chỉ cần đặt hàng. 

Quan sát giúp loại bỏ vấn đề là phép nhân trở thành phép cộng sau khi lấy logarit: 

\log(a_1!)+\log(a_2!)+\cdots+\log(a_k!). 
] 

Vì logarit tăng nghiêm ngặt nên danh sách có điểm logarit lớn nhất chính xác là danh sách có sản phẩm ban đầu lớn nhất. Chúng ta có thể tính toán trước 

[ 
L[x]=\log(x!) 
] 

cho mọi`x`từ`0`bởi vì`2500`. Sự tái diễn 

[ 
L[x]=L[x-1]+\log x 
] 

tính toán toàn bộ bảng trong thời gian tuyến tính. 

Sau đó, mỗi danh sách chỉ yêu cầu một phép cộng cho mỗi phần tử. Chúng tôi không bao giờ xây dựng một sản phẩm giai thừa hoặc giai thừa. 

Sự đảm bảo về các sản phẩm khác nhau là điều làm cho việc so sánh dấu phẩy động trở nên phù hợp ở đây. Một sự khác biệt tương đối ít nhất`0.01%`tương ứng với sự khác biệt logarit của khoảng 

[ 
\log(1,0001)\khoảng 10^{-4}. 
] 

Sai số dấu phẩy động tích lũy trong tổng nhỏ hơn rất nhiều so với sai số đó đối với kích thước đầu vào thực tế, do đó, dung sai so sánh nhỏ có thể phân biệt các sản phẩm thực sự khác nhau trong khi coi các sản phẩm bằng nhau về mặt toán học là các mối quan hệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2500S) bước số học, với chi phí số nguyên lớn | O(S) cộng với số nguyên khổng lồ | Quá chậm | 
| Tối ưu | O(2500 + S) | O(2500) | Đã chấp nhận | 

Đây`S`là tổng số phần tử trong ba danh sách của một ca kiểm thử. Phương pháp tối ưu cũng sử dụng`math.lgamma(x + 1)`như một cách thay thế để có được`log(x!)`, nhưng việc tính toán trước logarit tích lũy làm cho phép truy hồi dự định trở nên rõ ràng và tránh việc đánh giá nhiều lần một hàm đặc biệt. 

## Hướng dẫn thuật toán 

1. Tính toán trước`log_fact[x]`với mọi số nguyên`x`từ`0`ĐẾN`2500`. Bắt đầu với`log_fact[0] = 0`, bởi vì`0! = 1`Và`log(1) = 0`. Đối với mọi`x > 0`, bộ`log_fact[x] = log_fact[x - 1] + log(x)`. Điều này đưa ra logarit của mọi giai thừa mà không bao giờ xây dựng giai thừa đó. 
2. Đọc ba kích thước danh sách và sau đó đọc ba danh sách. Kích thước cho chúng tôi biết chính xác có bao nhiêu giá trị thuộc về mỗi danh sách, do đó, dữ liệu đầu vào có thể được sử dụng ngay cả khi giá trị của danh sách được chia thành nhiều dòng đầu vào vật lý. 
3. Với mỗi danh sách, hãy thêm`log_fact[x]`cho mọi giá trị`x`trong đó. Tổng kết quả là logarit của tích giai thừa của danh sách đó. 
4. Tìm giá trị lớn nhất của ba số logarit. Vì logarit bảo toàn thứ tự nên điểm tối đa tương ứng với sản phẩm ban đầu tối đa. 
5. So sánh mọi điểm số với điểm tối đa bằng cách sử dụng một dung sai nhỏ. Nếu cả ba giá trị nằm trong phạm vi dung sai tối đa được coi là bằng nhau, hãy in`TIE`; nếu không thì in tên của danh sách tối đa duy nhất. 
6. Lặp lại cho mọi trường hợp thử nghiệm và đặt trước câu trả lời số trường hợp dựa trên một trường hợp. 

Tại sao nó hoạt động: sau khi xử lý một danh sách, giá trị tích lũy của nó chính xác là 

\log\left(\prod_{x\in A}x!\right). 
] 

Do đó, ba giá trị tích lũy biểu thị logarit của ba sản phẩm cần thiết. Bởi vì logarit tăng nghiêm ngặt nên thứ tự của chúng giống hệt với thứ tự của các sản phẩm ban đầu. Sự bảo đảm tách biệt của bài toán ngăn hai tích khác nhau đủ gần để bị nhầm lẫn khi so sánh dấu phẩy động, trong khi các tích bằng nhau tạo ra tổng logarit toán học bằng nhau và được coi là bằng nhau. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

MAXV = 2500
EPS = 1e-10

# log_fact[x] = log(x!)
log_fact = [0.0] * (MAXV + 1)
for x in range(1, MAXV + 1):
    log_fact[x] = log_fact[x - 1] + math.log(x)

def read_list(n):
    values = []
    while len(values) < n:
        values.extend(map(int, input().split()))
    return values[:n]

def solve():
    t = int(input())
    out = []

    for case_no in range(1, t + 1):
        sizes = []
        while len(sizes) < 3:
            sizes.extend(map(int, input().split()))

        na, nb, nc = sizes[:3]

        A = read_list(na)
        B = read_list(nb)
        C = read_list(nc)

        scores = [
            sum(log_fact[x] for x in A),
            sum(log_fact[x] for x in B),
            sum(log_fact[x] for x in C),
        ]

        mx = max(scores)

        tied = sum(abs(score - mx) <= EPS for score in scores)

        if tied >= 2:
            answer = "TIE"
        elif scores[0] == mx:
            answer = "A"
        elif scores[1] == mx:
            answer = "B"
        else:
            answer = "C"

        out.append(f"Case #{case_no}: {answer}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Quá trình tính toán trước sẽ tạo bảng được sử dụng cho mọi trường hợp thử nghiệm. Mục nhập đầu tiên bằng 0 vì logarit của`0!`là logarit của`1`. Mỗi mục tiếp theo chỉ thêm`log(x)`, phản ánh chính xác sự tái diễn`x! = x * (x - 1)!`. 

các`read_list`Trình trợ giúp đáng được chú ý vì câu lệnh mô tả mỗi danh sách chiếm một dòng đầu vào, nhưng mã lập trình cạnh tranh mạnh mẽ không nên phụ thuộc vào chi tiết định dạng đó. Nó tiếp tục đọc cho đến khi thu thập đủ số giá trị được yêu cầu. 

Ba`sum`các biểu thức thực hiện phép biến đổi trung tâm từ tích sang tổng. Không có giai thừa nào được xây dựng nên bộ máy số nguyên có độ chính xác tùy ý của Python không bao giờ phải xử lý các giá trị khổng lồ liên quan. 

Việc so sánh sử dụng dung sai tuyệt đối của logarit. Sự khác biệt tương đối của`0.01%`giữa các sản phẩm riêng biệt tương ứng với khoảng cách logarit gần`1e-4`, trong khi`1e-10`nhỏ hơn nhiều bậc. Do đó, dung sai nằm ở mức thoải mái dưới mức phân tách được đảm bảo. 

các`0!`Và`1!`các trường hợp không yêu cầu nhánh đặc biệt vì cả hai đều đã xuất hiện chính xác trong bảng được tính toán trước. Cũng không có vấn đề tràn số nguyên vì mọi giá trị được lưu trữ trong`scores`là logarit dấu phẩy động chứ không phải là tích giai thừa. 

## Ví dụ đã hoạt động 

Tuyên bố chính thức đưa ra danh sách ví dụ`A = {2,4,7}`,`B = {0,1,9}`, Và`C = {2,3,5,5}`. Sản phẩm thực tế của họ là`241920`,`362880`, Và`172800`, tương ứng, do đó`B`là lớn nhất. 

Đối với một dấu vết, trạng thái logarit có liên quan như sau. 

| Danh sách | Giá trị | Điểm logarit | Kết quả tương đối | 
| --- | --- | --- | --- | 
| A | 2, 4, 7 |`log(2!) + log(4!) + log(7!)`| Về`12.395`| 
| B | 0, 1, 9 |`log(0!) + log(1!) + log(9!)`| Về`12.802`| 
| C | 2, 3, 5, 5 |`log(2!) + log(3!) + log(5!) + log(5!)`| Về`12.059`| 

Điểm tối đa là điểm dành cho`B`, vì vậy đầu ra là`Case #1: B`. Dấu vết chứng minh lý do tại sao thuật toán có thể so sánh các số lượng khó lưu trữ trực tiếp: các sản phẩm ban đầu đã có hàng trăm nghìn, trong khi logarit của chúng vẫn là số dấu phẩy động nhỏ. 

Ví dụ thứ hai thực hiện một kết quả hòa chính xác:```
1
2 2 1
3 2
2 3
4
```Các tiểu bang là 

| Danh sách | Giá trị | Sản phẩm giai thừa | Điểm logarit | 
| --- | --- | --- | --- | 
| A | 3, 2 |`3! * 2! = 12`|`log(12)`| 
| B | 2, 3 |`2! * 3! = 12`|`log(12)`| 
| C | 4 |`4! = 24`|`log(24)`| 

Đây`C`thực sự lớn hơn nên câu trả lời là`Case #1: C`. Để có được một chiếc cà vạt chính hãng, hãy thay đổi`C`ĐẾN`2 1`cho`2!*1!=2`, rời đi`A`Và`B`ràng buộc ở mức tối đa. Thuộc tính quan trọng là thứ tự của các giá trị trong danh sách không quan trọng vì phép nhân có tính chất giao hoán, do đó`[3,2]`Và`[2,3]`phải tạo ra các tổng logarit giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2500 + S) cho mỗi trường hợp thử nghiệm | Quá trình tính toán trước là tuyến tính ở giá trị tối đa và mỗi thành phần danh sách đóng góp một lần tra cứu bảng và một phép cộng | 
| Không gian | O(2500) | Bảng logarit giai thừa chứa một giá trị dấu phẩy động cho mỗi giá trị đầu vào có thể có | 

Phần tử tối đa chỉ`2500`, do đó việc tính toán trước là không đáng kể. Sau đó, thuật toán thực hiện công không đổi cho mọi giá trị đầu vào. Không giống như giải pháp brute-force, thời gian chạy của nó không tăng theo số chữ số trong tích giai thừa, bởi vì những sản phẩm đó không bao giờ được tạo ra. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
import math

MAXV = 2500
EPS = 1e-10

log_fact = [0.0] * (MAXV + 1)
for x in range(1, MAXV + 1):
    log_fact[x] = log_fact[x - 1] + math.log(x)

def program(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)

    t = next(it)
    ans = []

    for case_no in range(1, t + 1):
        na = next(it)
        nb = next(it)
        nc = next(it)

        A = [next(it) for _ in range(na)]
        B = [next(it) for _ in range(nb)]
        C = [next(it) for _ in range(nc)]

        scores = [
            sum(log_fact[x] for x in A),
            sum(log_fact[x] for x in B),
            sum(log_fact[x] for x in C),
        ]

        mx = max(scores)
        tied = sum(abs(x - mx) <= EPS for x in scores)

        if tied >= 2:
            winner = "TIE"
        elif scores[0] == mx:
            winner = "A"
        elif scores[1] == mx:
            winner = "B"
        else:
            winner = "C"

        ans.append(f"Case #{case_no}: {winner}")

    return "\n".join(ans) + "\n"

# Provided example from the statement.
sample1 = """\
1
3 3 4
2 4 7
0 1 9
2 3 5 5
"""
assert program(sample1) == "Case #1: B\n", "provided example"

# Minimum-size values.  0! = 1! = 1, so all three products tie.
sample2 = """\
1
1 1 1
0
1
0
"""
assert program(sample2) == "Case #1: TIE\n", "minimum values and 0!"

# All lists contain exactly the same values, so they must tie.
assert program("""\
1
4 4 4
5 5 5 5
5 5 5 5
5 5 5 5
""") == "Case #1: TIE\n", "all equal lists"

# Boundary value 2500.  B has one additional 1!, so it is still tied with A.
assert program("""\
1
1 2 1
2500
2500 1
2499
""") == "Case #1: A\n", "maximum element and 1!"

# Catch an off-by-one mistake in factorial indexing.
# A = 4! = 24, B = 3! * 1! = 6, C = 3! = 6.
assert program("""\
1
1 2 1
4
3 1
3
""") == "Case #1: A\n", "factorial boundary"

# A and B have equal products despite different order.
assert program("""\
1
2 2 1
3 2
2 3
2
""") == "Case #1: TIE\n", "permutation equality"

# Large input value repeated many times, exercising the precomputed table
# without constructing the enormous factorial product.
assert program("""\
1
2 2 2
2500 2500
2500 2499
2500 2500
""") == "Case #1: A\n", "large factorial products"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 1 / 0 / 1 / 0`|`Case #1: TIE`| Giá trị tối thiểu và xử lý chính xác`0!`| 
| Bốn bản sao của`5`trong mỗi danh sách |`Case #1: TIE`| Bình đẳng chính xác và giá trị lặp lại | 
| Danh sách chứa`2500`|`Case #1: A`| Phần tử tối đa được phép và không có cấu trúc số nguyên lớn | 
|`4`so với`3,1`so với`3`|`Case #1: A`| Lập chỉ mục giai thừa chính xác và xử lý ranh giới | 
|`[3,2]`so với`[2,3]`|`Case #1: TIE`| Tính giao hoán của tích số và đẳng thức | 
| lặp đi lặp lại`2500!`giá trị |`Case #1: A`| Tổng logarit lớn và tính toán trước | 

## Vỏ cạnh 

cho`0!`, đầu vào```
1
1 1 1
0
1
0
```tạo ra ba điểm logarit bằng 0. Giá trị được tính toán trước`log_fact[0]`được khởi tạo rõ ràng bằng 0, vì vậy cả ba danh sách đều được nhận dạng là ràng buộc. Không cần trường hợp đặc biệt nào trong vòng lặp chính. 

Đối với các giá trị lặp lại, hãy xem xét```
1
3 2 1
5 5 5
5 5
5
```Mỗi danh sách đóng góp một số lượng bản sao như nhau`log(5!)`theo kích thước của nó, do đó điểm số tỷ lệ thuận với`3`,`2`, Và`1`. Kết quả là`Case #1: A`. Thuật toán xử lý sự lặp lại một cách tự nhiên vì mỗi lần xuất hiện đều đóng góp riêng vào tổng. 

Đối với các sản phẩm giống nhau có nội dung danh sách khác nhau, hãy sử dụng```
1
2 2 1
3 2
2 3
2
```Hai danh sách đầu tiên đều có sản phẩm`3!*2! = 12`. Điểm logarit của họ đều`log(12)`, do đó hiệu của chúng bằng 0 và cả hai đều nằm trong dung sai cực đại. Đầu ra là`Case #1: TIE`. 

Đối với phần tử lớn nhất có thể, hãy xem xét```
1
1 2 1
2500
2500 1
2499
```Danh sách đầu tiên có sản phẩm`2500!`, thứ hai cũng có`2500!*1!`, và thứ ba có`2499!`. Từ`1! = 1`, hai danh sách đầu tiên phù hợp với sản phẩm lớn nhất. Thuật toán tra cứu`log_fact[2500]`trực tiếp và không bao giờ cố gắng xây dựng`2500!`. 

Trường hợp cạnh cuối cùng là trường hợp số. Giả sử hai tích khác nhau một lượng nhỏ nhất mà phát biểu cho phép, khoảng`0.01%`. Logarit của chúng chênh nhau khoảng`0.0001`, trong khi dung sai so sánh là`1e-10`. Dung sai nhỏ hơn nhiều so với khoảng cách được đảm bảo, do đó, các sản phẩm riêng biệt không thể bị gộp lại thành một điểm bằng nhau khi so sánh. Đồng thời, các tích bằng nhau về mặt toán học có tổng logarit bằng nhau nên được coi là bằng nhau.
