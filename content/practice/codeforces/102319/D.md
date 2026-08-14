---
title: "CF 102319D - David vs David"
description: "Chúng ta có một trò chơi khách quan hai cọc. Một vị trí được biểu thị bằng hai kích thước cọc, chẳng hạn như (x, y). Trong một lượt, người chơi có thể loại bỏ bất kỳ số lượng quân dương nào từ chính xác một đống."
date: "2026-08-13T04:49:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102319
codeforces_index: "D"
codeforces_contest_name: "UBC Summer Contest 2018"
rating: 0
weight: 102319
solve_time_s: 625
verified: true
draft: false
---

[CF 102319D - David vs David](https://codeforces.com/problemset/problem/102319/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 10 phút 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một trò chơi khách quan hai cọc. Một vị trí được thể hiện bằng hai kích thước cọc, chẳng hạn`(x, y)`. Trong một lượt, người chơi có thể loại bỏ bất kỳ số lượng quân dương nào từ chính xác một đống. Họ cũng có thể loại bỏ số tiền dương từ cả hai cọc, nhưng hai số tiền bị loại bỏ tối đa phải khác nhau.`k`. Người chơi không có nước đi hợp lệ sẽ thua, vì vậy`(0, 0)`là một vị trí thua cuộc. 

Đối với mỗi lên đến`10^5`trò chơi độc lập, chúng tôi nhận được hai kích thước cọc ban đầu và dung sai`k`. Chúng ta cần in`2`chính xác khi vị trí xuất phát bị thua đối với người chơi đầu tiên và`1`nếu không thì. Tuyên bố chính thức xác nhận những giới hạn này và dữ liệu mẫu. 

Giới hạn tọa độ lớn,`x,y <= 10^9`, loại trừ bất kỳ chương trình động nào được lập chỉ mục theo kích thước cọc. Thậm chí một trò chơi có thể chứa khoảng`10^18`các vị trí khác nhau. Với`10^5`trò chơi và giới hạn 2 giây, giải pháp dự định phải sử dụng số học không đổi hoặc logarit cho mỗi trò chơi. Sự thật là`k <= 12`cũng là một gợi ý rõ ràng rằng dung sai sẽ thay đổi cấu trúc theo cách được kiểm soát thay vì yêu cầu tìm kiếm rộng rãi về dung sai có thể có. 

Có một số trường hợp khó xử lý. 

Vị trí đầu cuối là đặc biệt. Đối với đầu vào```
1
0 0 0
```đầu ra đúng là```
2
```bởi vì người chơi đầu tiên không được di chuyển. Một công thức chỉ xử lý các chỉ số dương và vô tình coi chỉ số 0 là vị trí P thông thường có thể mắc lỗi này. 

Các cọc có thể hoán đổi cho nhau nên thứ tự của tọa độ đầu vào không quan trọng. Vì```
1
2 1 0
```đầu ra đúng là`2`, bởi vì`(1,2)`là vị trí Wythoff đang bị mất. Việc triển khai giả định tọa độ đầu tiên luôn nhỏ hơn mà không thực sự hoán đổi các giá trị có thể từ chối vị trí P hợp lệ. 

Dung sai thay đổi khoảng cách đường chéo có liên quan. Vì```
1
1 3 1
```đầu ra đúng là`2`. Vị trí đang thua vì đây là một trong những vị trí Wythoff tổng quát cho`k=1`. Điều trị tình trạng như`|a-b| < k`thay vì`|a-b| <= k`thay đổi trò chơi và đưa ra phân loại sai. 

Trường hợp chênh lệch cọc không chia hết cho`k+1`cũng rất đáng kể. Vì```
1
1 2 1
```đầu ra đúng là`1`. Sự khác biệt ở đây là`1`, trong khi`k+1=2`, do đó bản thân vị trí không thể là một trong các vị trí P được mô tả bởi lời giải. Việc triển khai bất cẩn chỉ kiểm tra xem sự khác biệt có "nhỏ" hay không chứ không phải liệu nó có khớp với cấp số cộng cần thiết hay không sẽ phân loại sai nó. 

Cuối cùng, số cọc bằng nhau không có nghĩa là vị thế đang thua. Vì```
1
5 5 3
```đầu ra đúng là`1`. Vị trí P bằng nhau duy nhất trong trò chơi này là`(0,0)`, bởi vì các vị trí P tổng quát có chênh lệch dương bất cứ khi nào chỉ số của chúng dương. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là giải quyết trò chơi theo cách đệ quy. Một thế cờ sẽ thua nếu mọi nước đi hợp lệ đều đi đến thế thắng và sẽ thắng nếu có ít nhất một nước đi đến thế thua. Với tính năng ghi nhớ, điều này mang lại một chương trình động chính xác. 

Vấn đề là kích thước của không gian trạng thái. Nếu các cọc ban đầu là`X`Và`Y`, việc ghi nhớ có thể vẫn yêu cầu mọi vị trí`(x,y)`với`0 <= x <= X`Và`0 <= y <= Y`. Đại khái là thế`XY`tiểu bang. Ngay cả khi chúng ta chỉ đếm các bước di chuyển của một cọc đơn lẻ, tổng số lần chuyển đổi được kiểm tra trên tất cả các trạng thái là 

\frac{XY(X+Y+2)}2. 
]

Vì`X=Y=10^9`, đây là về`10^27`chuyển tiếp trước khi xem xét các bước di chuyển của hai cọc. Một phép đệ quy không ghi nhớ thậm chí còn tệ hơn vì nó liên tục khám phá các trò chơi con giống nhau. 

Bước đột phá là ngừng suy nghĩ về từng vị trí một cách riêng lẻ và thay vào đó mô tả chính xác vị trí nào đang bị mất. Trò chơi này là một dạng tổng quát của Wythoff Nim. Đối với số nguyên dương`r = k+1`, vị trí P của nó được tạo bởi 

[ 
a_n=\left\lfloor n\alpha\right\rfloor, 
\qquad 
b_n=a_n+rn, 
] 

ở đâu 

[ 
\alpha=\frac{2-r+\sqrt{r^2+4}}2. 
] 

Đây là cách xây dựng Wythoff tổng quát tiêu chuẩn, trong đó hai chuỗi Beatty bổ sung khác nhau bởi`rn`. Đặc tính vị trí P tương ứng còn được gọi là`k`-Wythoff Nim mô tả đặc điểm. 

Lý do`r=k+1`có vẻ đặc biệt sạch sẽ. Các vị trí P liên tiếp có sự khác biệt 

[ 
b_n-a_n=rn. 
] 

Nếu chúng ta cố gắng di chuyển từ vị trí P`n`đến vị trí P trước đó`m`, hai lượng bị loại bỏ khác nhau bởi 

[ 
rn-rm=r(n-m). 
]

Từ`r=k+1`, sự khác biệt này ít nhất là`k+1`, do đó việc di chuyển như vậy đã vi phạm giới hạn cho phép`k`. Không thể di chuyển một cọc giữa các vị trí P vì các chuỗi tọa độ bổ sung cho nhau. 

Thuộc tính bổ sung là nửa còn lại của giải pháp. Trình tự`(a_n)`Và`(b_n)`chứa mọi số nguyên dương đúng một lần. Đây là hệ quả trực tiếp của định lý Beatty vì hai hệ số góc thỏa mãn 

[ 
\frac1\alpha+\frac1{\alpha+r}=1. 
] 

Điều đó cho chúng ta một cách để chứng minh rằng mọi vị trí không phải P đều có bước chuyển sang vị trí P. 

Việc phân loại cuối cùng đặc biệt đơn giản. Sắp xếp hai cọc sao cho`x <= y`, cho phép`d=y-x`, và đặt`r=k+1`. Nếu như`d`chia hết cho`r`, hãy để 

[ 
n=\frac d r. 
] 

Vị trí đang mất chính xác khi 

[ 
x=a_n. 
] 

Nếu không thì nó đang thắng. 

Còn một vấn đề về số. Máy tính`floor(n*alpha)`trực tiếp với dấu phẩy động là rủi ro vì`n`có thể lớn như`10^9`. Chúng ta có thể loại bỏ hoàn toàn dấu phẩy động. Kể từ khi 

\left\làn 
\frac{n(2-r)+n\sqrt{r^2+4}}2 
\right\rsàn, 
] 

hãy để 

[ 
q=\left\lfloor n\sqrt{r^2+4}\right\rfloor. 
] 

Sau đó 

[ 
q=\left\lfloor\sqrt{n^2(r^2+4)}\right\rfloor, 
] 

có thể được tính toán chính xác bằng Python`math.isqrt`. Do đó 

[ 
a_n= 
\frac{n(2-r)+q}{2} 
] 

với phép chia tầng số nguyên. Không cần xấp xỉ dấu phẩy động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(XY(X+Y))`mỗi trò chơi có ghi nhớ |`O(XY)`| Quá chậm | 
| Tối ưu |`O(log(x+y))`số học mỗi trò chơi |`O(1)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trò chơi, trước tiên hãy sắp xếp lại các cọc sao cho`x <= y`. Trò chơi có tính đối xứng nên điều này không làm thay đổi kết quả của nó. 
2. Đặt`r = k + 1`. Vị trí P Wythoff tổng quát có dạng`(a_n,b_n)`với 

[ 
b_n-a_n=rn. 
] 

Do đó, một vị trí chỉ có thể là vị trí P nếu`y-x`chia hết cho`r`. 
3. Nếu`d = y-x`không chia hết cho`r`, ngay lập tức phân loại vị trí là chiến thắng. Vị trí P có sự khác biệt chính xác`rn`đối với một số nguyên`n`. 
4. Ngược lại tính toán`n = d // r`. Vị trí P duy nhất có thể có sự khác biệt này là`(a_n,b_n)`, vậy câu hỏi còn lại là liệu`x`bằng`a_n`. 
5. Tính toán 

[ 
a_n= 
\left\làn 
n\frac{2-r+\sqrt{r^2+4}}2 
\right\rsàn 
] 

không có dấu phẩy động. Tính toán 

[ 
q=\tên toán tử{isqrt}(n^2(r^2+4)), 
] 

sau đó sử dụng 

[ 
a_n=(n(2-r)+q)//2. 
] 

các`isqrt`phép toán đưa ra số nguyên chính xác của căn bậc hai. 
6. Nếu`x == a_n`, vị trí là vị trí P nên người chơi đầu tiên thua và chúng tôi in`2`. Nếu không thì đó là vị trí N, vì vậy người chơi đầu tiên thắng và chúng tôi in`1`. 

### Tại sao nó hoạt động 

Hai trình tự 

[ 
a_n=\lfloor n\alpha\rfloor, 
\qquad 
b_n=\lfloor n(\alpha+r)\rfloor 
] 

bổ sung cho nhau vì độ dốc của chúng thỏa mãn 

[ 
\frac1\alpha+\frac1{\alpha+r}=1. 
] 

Do đó, mọi số nguyên dương đều xuất hiện đúng một trong hai dãy. 

Xem xét hai vị trí P riêng biệt với các chỉ số`m < n`. Sự khác biệt về tọa độ của chúng đều dương và sự khác biệt giữa hai lượng bị loại bỏ là 

k+1. 
] 

Vì vậy, không có nước đi hai cọc hợp pháp nào có thể kết nối hai vị trí P. Nước đi một cọc cũng không thể kết nối chúng, bởi vì tính bổ sung có nghĩa là không có tọa độ nào được chia sẻ giữa các tọa độ vị trí P khác nhau. 

Bây giờ hãy ở vị trí không phải P`(x,y)`với`x <= y`. Nếu một tọa độ thuộc dãy trên`b_n`, tính bổ sung cho phép chúng ta giảm tọa độ khác để thu được vị trí P tương ứng bằng cách di chuyển một cọc. 

Trường hợp còn lại là`x=a_m`Và`y=a_n`. Nếu như`y>b_m`, chúng ta có thể giảm`y`ĐẾN`b_m`và đạt được`(a_m,b_m)`. Nếu không, 

[ 
d=y-x=a_n-a_m<b_m-a_m=rm. 
] 

chọn 

[ 
j=\left\lfloor\frac d r\right\rfloor. 
]

Sau đó`j<m`và 

[ 
0\le d-rj\le r-1=k. 
]

Từ`a_j < a_m=x`Và`b_j=a_j+rj <= y`, chúng ta có thể giảm cả hai cọc xuống`(a_j,b_j)`. Hai số tiền bị loại bỏ khác nhau một cách chính xác`d-rj`, nhiều nhất là`k`. Do đó, mọi vị trí không phải P đều chuyển sang vị trí P. 

Hai thuộc tính này chính xác là đặc tính xác định của việc mất vị trí: không có nước đi nào rời khỏi vị trí P, trong khi mọi vị trí khác đều có nước đi vào một vị trí. Do đó, việc phân loại là chính xác. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        x, y, k = map(int, input().split())

        if x > y:
            x, y = y, x

        r = k + 1
        d = y - x

        if d % r != 0:
            out.append("1")
            continue

        n = d // r

        # a_n = floor(n * (2 - r + sqrt(r^2 + 4)) / 2)
        # floor(n * sqrt(D)) = isqrt(n^2 * D)
        D = r * r + 4
        q = math.isqrt(n * n * D)

        a = (n * (2 - r) + q) // 2

        if x == a:
            out.append("2")
        else:
            out.append("1")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của vòng lặp sắp xếp hai cọc. Điều này cho phép phần còn lại của mã sử dụng quy ước`x <= y`và tránh kiểm tra cả hai hướng của mọi vị trí P. 

Biến`r`là`k+1`, không`k`. Chi tiết riêng biệt này là chi tiết trung tâm của trò chơi. Sự khác biệt về vị trí P là bội số của`k+1`, vì hai vị trí P phải cách nhau nhiều hơn mức chênh lệch cho phép`k`giữa số tiền bị loại bỏ. 

Việc kiểm tra tính chia hết được thực hiện trước bất kỳ phép tính căn bậc hai nào. Nếu như`d`không chia hết cho`r`, không thể có vị trí P với sự chênh lệch tọa độ đó nên câu trả lời là ngay lập tức`1`. 

Khi`d`có thể chia được,`n=d/r`xác định vị trí P ứng cử viên duy nhất. Mã sau đó tính toán tọa độ nhỏ hơn của nó`a_n`. 

Biểu thức liên quan đến`isqrt`là chính xác. Cho phép`D=r*r+4`. Kể từ khi 

[ 
\operatorname{isqrt}(n^2D)=\lfloor n\sqrt D\rfloor, 
] 

chúng ta có thể thay thế phần vô tỷ của công thức Beatty bằng một số nguyên. Số nguyên Python có độ chính xác tùy ý, do đó giá trị trung gian lớn nhất, khoảng`10^20`, không thể tràn. 

các`// 2`hoạt động cũng an toàn mặc dù thời hạn không hợp lý. Nếu 

[ 
n\sqrt D=q+f,\qquad 0\le f<1, 
] 

thì số lượng được thả nổi là`(integer + f)/2`. Cho dù phần nguyên là chẵn hay lẻ thì phần phân số không bao giờ có thể đẩy kết quả qua số nguyên tiếp theo. Do đó, sàn được lấy chính xác từ tử số nguyên bằng cách sử dụng`// 2`. 

Mã sử ​​dụng`sys.stdin.readline`như yêu cầu. Chỉ với một vài phép toán số nguyên và một căn bậc hai số nguyên cho mỗi trò chơi, việc triển khai dễ dàng tránh được việc xây dựng bất kỳ bảng trạng thái trò chơi lớn nào. 

## Ví dụ đã hoạt động 

Hãy xem xét trò chơi mẫu đầu tiên,`(0,0,k=0)`. Đây`r=1`và các cọc đã được đặt hàng. 

|`x`|`y`|`k`|`r`|`d=y-x`|`d % r`|`n`|`a_n`| Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 2 | 

Sự khác biệt được chia cho`r`, cho`n=0`. Công thức cho`a_0=0`, bằng`x`. Kể từ đây`(0,0)`là vị trí P và người chơi đầu tiên thua. Điều này xác nhận rằng trường hợp chỉ số 0 được xử lý một cách tự nhiên theo cùng một công thức như tất cả các vị trí P khác. 

Bây giờ hãy xem xét trò chơi mẫu`(23,9,k=1)`. Sắp xếp lại mang lại`(9,23)`. Từ`r=k+1=2`, sự khác biệt là`14`, vì vậy chỉ số ứng cử viên là`n=7`. 

Vì`r=2`, công thức trở thành 
[ 
a n=\floor n\sqrt 2\rfloor. 
]
Tại`n=7`, 
[ 
a_7=\tầng 7\sqrt 2\tầng=9. 
] 
|`x`|`y`|`k`|`r`|`d`|`n=d/r`|`a_n`|`x == a_n`| Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 9 | 23 | 1 | 2 | 14 | 7 | 9 | Có | 2 | 

Vì thế`(9,23)`là vị trí P, khớp với đầu ra mẫu`2`. Dấu vết cũng chứng minh tại sao việc sắp xếp tọa độ là cần thiết: đầu vào ban đầu có cọc lớn hơn trước. 

Đối với vị trí không phải P gần ranh giới dung sai, hãy xem xét`(1,13,k=12)`. Đây`r=13`, và sự khác biệt chính xác là`12`, nhỏ hơn`r`. 

|`x`|`y`|`k`|`r`|`d`|`d % r`|`n`|`a_n`| Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 13 | 12 | 13 | 12 | 12 | không được sử dụng | không được sử dụng | 1 | 

Sự khác biệt không chia hết cho`13`, vì vậy đây không thể là vị trí P. Trên thực tế, người chơi đầu tiên có thể loại bỏ`1`đá từ đống đầu tiên và`13`từ thứ hai, đạt`(0,0)`. Hai số tiền này khác nhau một cách chính xác`12`, vì vậy việc di chuyển là hợp pháp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log max(x,y))`| Mỗi trò chơi thực hiện một số phép tính số nguyên không đổi và một`isqrt`trên một số nguyên với`O(log max(x,y))`bit. | 
| Không gian |`O(1)`không gian phụ trợ | Ngoài bộ đệm đầu ra, thuật toán chỉ lưu trữ một số nguyên không đổi cho mỗi trò chơi. | 

Với`n <= 10^5`và tọa độ nhiều nhất`10^9`, toán hạng căn bậc hai chỉ chứa khoảng 67 bit. Do đó, thuật toán thực hiện một lượng nhỏ số học số nguyên lớn cố định cho mỗi truy vấn thay vì lặp qua các kích thước cọc. Điều này tương thích thoải mái với giới hạn 2 giây và giới hạn bộ nhớ 256 MB đã nêu. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        x, y, k = map(int, input().split())

        if x > y:
            x, y = y, x

        r = k + 1
        d = y - x

        if d % r != 0:
            out.append("1")
            continue

        n = d // r
        D = r * r + 4
        q = math.isqrt(n * n * D)
        a = (n * (2 - r) + q) // 2

        out.append("2" if x == a else "1")

    sys.stdout.write("\n".join(out))

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

sample = """15
0 0 0
23 9 1
97 99 2
1984 6 3
277 348 4
2384 19 5
138 19 6
123 372 7
112 1021 8
99328 9702 9
3172 283401 10
1937 23405 11
421443 503539 12
508320368 822479633 0
924717293 228947159 1
"""

sample_output = """2
2
1
1
1
1
2
1
2
1
1
2
1
2
1"""

assert run(sample) == sample_output, "official sample"

assert run("""1
0 0 0
""") == "2\n", "terminal position"

assert run("""4
1 1 0
1 2 0
2 1 0
1 3 1
""") == """1
2
2
2
""", "classical Wythoff and coordinate symmetry"

assert run("""3
2 4 1
1 13 12
1 14 12
""") == """1
1
2
""", "tolerance boundaries and generalized P-position"

assert run("""3
5 5 3
1000000000 1000000000 12
0 1000000000 0
""") == """1
1
1
""", "equal piles and maximum coordinates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 0`|`2`| Vị trí và chỉ mục đầu cuối duy nhất`n=0`. | 
|`1 1 0`,`1 2 0`,`2 1 0`,`1 3 1`|`1, 2, 2, 2`| Vị trí Wythoff cổ điển, tọa độ đối xứng và`k=1`trường hợp. | 
|`2 4 1`,`1 13 12`,`1 14 12`|`1, 1, 2`| Sự khác biệt giữa sự khác biệt chia hết cho`k+1`và kiểm tra vị trí P chính xác. | 
|`5 5 3`,`10^9 10^9 12`,`0 10^9 0`|`1, 1, 1`| Các cọc bằng nhau, tọa độ tối đa và các vị trí mà một cọc trống. | 

## Vỏ cạnh 

Đối với vị trí đầu cuối```
1
0 0 0
```thuật toán được`r=1`,`d=0`, Và`n=0`. Biểu thức căn bậc hai chính xác bằng 0, vì vậy`a_0=0`. Từ`x=0`, vị trí được coi là thua và đầu ra là`2`. Không có trường hợp đặc biệt nào thực sự cần thiết cho`(0,0)`. 

Đối với tọa độ đảo ngược, hãy xem xét```
1
2 1 0
```Thuật toán đầu tiên hoán đổi các cọc và thu được`(1,2)`. Với`r=1`, sự khác biệt là`1`, cho`n=1`. Công thức cho`a_1=1`, vậy vị trí là P và câu trả lời là`2`. Nếu không có sự hoán đổi ban đầu, vị trí P chính xác có thể bị từ chối đơn giản vì tọa độ của nó được cung cấp theo thứ tự ngược lại. 

Đối với ranh giới dung sai, hãy xem xét```
1
1 13 12
```Đây`r=13`Và`d=12`. Từ`12 % 13 != 0`, thuật toán trả về ngay`1`. Điều này đúng vì các vị trí P tổng quát có sự khác biệt`0,13,26,39,...`. Vị trí thực sự đang giành chiến thắng bằng cách di chuyển trực tiếp từ`(1,13)`ĐẾN`(0,0)`, loại bỏ`1`Và`13`đá, có sự khác biệt chính xác là dung sai cho phép`12`. 

Đối với vị trí P thực tế có cùng dung sai, hãy xem xét```
1
1 14 12
```Hiện nay`d=13`, Vì thế`n=1`. Với`r=13`, 

[ 
a_1= 
\left\làn 
\frac{2-13+\sqrt{173}}2 
\right\rsàn 
=1. 
] 

Như vậy`(1,14)`là vị trí P và đầu ra là`2`. Cặp này thể hiện chính xác lý do tại sao khoảng cách có liên quan là`k+1`, còn hơn là`k`. 

Đối với các cọc bằng nhau, hãy xem xét```
1
5 5 3
```Sự khác biệt là bằng không, vì vậy`n=0`Và`a_0=0`. Vì đống nhỏ hơn là`5`, không`0`, vị trí không phải là P và câu trả lời là`1`. Người chơi đầu tiên có thể lấy năm viên đá khỏi một đống và lấy`(0,5)`, sau đó đối thủ có nước đi. 

Để có tọa độ tối đa, hãy xem xét```
1
1000000000 1000000000 12
```Lại`d=0`, vậy chỉ`n=0`cần phải được xem xét. Thuật toán tính toán`a_0=0`, thấy rằng kích thước cọc không bằng 0 và in`1`. Tọa độ lớn không bao giờ gây ra vụ nổ không gian trạng thái vì giải pháp không liệt kê bất kỳ vị trí nào. 

Trường hợp tinh vi cuối cùng là ranh giới dấu phẩy động chính xác. Công thức chứa căn bậc hai vô tỉ nhân với chỉ số lớn bằng`10^9`. sử dụng`float`và gọi`int(n*alpha)`về mặt lý thuyết có thể đưa ra câu trả lời sai nếu làm tròn số ở phía sai của một số nguyên. Việc triển khai sẽ tránh được toàn bộ loại lỗi đó bằng cách thay thế phép tính vô tỷ bằng`math.isqrt(n*n*(r*r+4))`, vì vậy mọi so sánh đều được thực hiện với các số nguyên chính xác.
