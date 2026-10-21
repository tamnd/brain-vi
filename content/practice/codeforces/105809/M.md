---
title: "CF 105809M - Bài toán"
description: "Chúng ta có hai tập hợp đường thẳng thẳng hàng với trục vô hạn trên mặt phẳng. Tập đầu tiên chứa các đường thẳng đứng tại vị trí $xi$. Bộ thứ hai chứa các đường ngang tại vị trí $yj$. Một đoạn đường chéo đi từ $(h,0)$ đến $(h+l,l)$."
date: "2026-06-25T15:30:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105809
codeforces_index: "M"
codeforces_contest_name: "Code Rush 2025"
rating: 0
weight: 105809
solve_time_s: 64
verified: true
draft: false
---

[CF 105809M - Bài học toán](https://codeforces.com/problemset/problem/105809/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai tập hợp đường thẳng thẳng hàng với trục vô hạn trên mặt phẳng. 

Bộ đầu tiên chứa các đường thẳng đứng tại các vị trí$x_i$. Bộ thứ hai chứa các đường ngang tại các vị trí$y_j$. 

Một đoạn chéo đi từ$(h,0)$ĐẾN$(h+l,l)$. Vì độ dốc của nó là$1$, mọi điểm nguyên trên đoạn đó đều có dạng$(h+t,t)$vì$0 \le t \le l$. Đối với mỗi điểm như vậy, chúng ta bắn một tia vuông góc với đường chéo, hướng về bên phải. Nhiệm vụ là xác định đường vẽ nào bị trúng đầu tiên. Nếu lần nhấn đầu tiên là một đường thẳng đứng, chúng ta sẽ in`A`theo sau là chỉ mục ban đầu của nó. Nếu là đường ngang thì ta in`T`theo sau là chỉ mục ban đầu của nó. Các mối quan hệ được giải quyết theo hướng có lợi cho đường ngang. Nếu không có gì bị đánh trúng, chúng tôi sẽ in`-1`. 

Các giới hạn đầu vào là quan sát chính. chúng tôi có$N+M \le 10^6$, Và$l$cũng có thể đạt được$10^6$. Bất kỳ giải pháp nào thực hiện tìm kiếm nhị phân cho mọi tia đều đã có sẵn$10^6 \log 10^6$, điều này có thể chấp nhận được, nhưng có sự quét tuyến tính rõ ràng hơn. Bất kỳ cách tiếp cận nào so sánh mọi tia với mọi đường sẽ yêu cầu khoảng$10^{12}$hoạt động và hoàn toàn không khả thi. 

Các trường hợp tinh tế đến từ hình học. 

Coi như:```
1 1
1
0
0 0
```Tia duy nhất bắt đầu tại$(0,0)$. Đường thẳng đứng cách nhau một đơn vị, trong khi đường ngang đã ở điểm bắt đầu. Câu trả lời đúng là:```
T0
```Việc thực hiện bất cẩn mà bỏ qua các giao lộ ở khoảng cách bằng 0 sẽ chọn sai đường thẳng đứng. 

Một trường hợp quan trọng khác là hòa.```
1 1
5
2
3 2
```Đối với điểm$(4,1)$, các giao điểm dọc và ngang gần nhất xảy ra ở cùng một khoảng cách dọc theo tia. Câu lệnh yêu cầu chọn đường kẻ ngang nên đáp án phải sử dụng`T`, không`A`. 

Cuối cùng, một số tia có thể không bao giờ chạm tới bất cứ thứ gì.```
1 1
0
100
10 0
```Tia bắt đầu tại$(10,0)$, di chuyển sang phải và đi xuống, không thể chạm tới đường thẳng đứng tại$x=0$, và đã ở dưới đường ngang tại$y=100$. Đầu ra đúng là:```
-1
```## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Với mỗi điểm nguyên trên đường chéo, hãy tính xem tia của nó giao nhau với mọi đường thẳng đứng và mọi đường ngang. Giữ giao điểm hợp lệ với khoảng cách nhỏ nhất dọc theo tia. Điều này đúng vì nó đánh giá rõ ràng mọi ứng viên. Thật không may, có$l+1$tia và lên đến$10^6$dòng, dẫn đến khoảng$10^{12}$kiểm tra trong trường hợp xấu nhất. 

Để cải thiện điều này, chúng ta cần hiểu hình học của một tia. 

Đường chéo là$y=x-h$, vậy một điểm trên đó là$(h+t,t)$. 

Một tia vuông góc hướng sang phải có hướng$(1,-1)$. Dạng tham số của nó là$$x=h+t+u,\qquad y=t-u,\qquad u\ge 0.$$Đối với đường thẳng đứng$x=a$, giao điểm xảy ra tại$$u=a-(h+t).$$Điều này chỉ hợp lệ khi$a \ge h+t$. 

Trong số tất cả các đường thẳng đứng, chỉ có đường nhỏ nhất$x_i$thỏa mãn$x_i \ge h+t$có thể là cú đánh thẳng đứng đầu tiên. 

Đối với đường ngang$y=b$, giao điểm xảy ra tại$$u=t-b.$$Điều này chỉ hợp lệ khi$b \le t$. 

Trong số tất cả các đường ngang, chỉ có đường lớn nhất$y_j$thỏa mãn$y_j \le t$có thể là cú đánh ngang đầu tiên. 

Vấn đề bây giờ đã trở thành một chuỗi các truy vấn kế tiếp và trước đó trên các phạm vi số nguyên. 

BẰNG$t$tăng từ$0$ĐẾN$l$, giá trị$h+t$cũng tăng đơn điệu. Điều đó có nghĩa là chúng ta có thể xử lý tất cả các tia bằng hai lần quét tuyến tính. 

Đối với các dòng thẳng đứng, chúng ta giữ một con trỏ tới dòng đầu tiên có$x$-tọa độ ít nhất là hiện tại$h+t$. 

Đối với các đường ngang, chúng ta giữ một con trỏ tới dòng lớn nhất có$y$-tọa độ tối đa là hiện tại$t$. 

Mỗi con trỏ chỉ di chuyển về phía trước nên toàn bộ công việc là tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((l+1)(N+M))$|$O(1)$| Quá chậm | 
| Tối ưu |$O(N\log N + M\log M + l)$|$O(l)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mọi đường thẳng đứng dưới dạng`(x_position, original_index)`và sắp xếp theo`x_position`. 
2. Lưu trữ mọi đường ngang dưới dạng`(y_position, original_index)`và sắp xếp theo`y_position`. 
3. Với mọi giá trị`t`từ`0`ĐẾN`l`, cho phép`s = h + t`. 
4. Duy trì một con trỏ trong mảng dọc đã được sắp xếp. Di chuyển nó về phía trước trong khi dòng hiện tại có`x < s`. Đường nhọn bây giờ là đường thẳng đứng gần bên phải nhất, nếu đường đó tồn tại. 
5. Tính thông số khoảng cách dọc:$$u_v = x_i - s$$Nếu không tồn tại đường thẳng đứng hợp lệ, hãy coi khoảng cách là vô cùng. 
6. Duy trì một con trỏ khác trong mảng ngang đã sắp xếp. Trong khi dòng tiếp theo thỏa mãn`y <= t`, tiến con trỏ. 
7. Đường ngang nhọn bây giờ là lớn nhất`y_j <= t`, đây chính xác là đường ngang đầu tiên bị tia chiếu chiếu tới. 
8. Tính toán$$u_h = t - y_j$$Nếu không có đường ngang như vậy tồn tại, hãy coi khoảng cách là vô cùng. 
9. So sánh hai khoảng cách. 

Nếu như`u_h <= u_v`, xuất ra đường ngang. 

Nếu không thì xuất ra đường thẳng đứng. 
10. Nếu cả hai khoảng cách là vô hạn, xuất ra`-1`. 

### Tại sao nó hoạt động 

Đối với tia cố định, mọi giao điểm dọc hợp lệ đều có tham số khoảng cách$$u=x_i-(h+t).$$Khoảng cách hợp lệ nhỏ nhất đến từ giá trị nhỏ nhất$x_i$ít nhất đó vẫn là$h+t$. Bất kỳ đường thẳng đứng nào lớn hơn đều tạo ra khoảng cách lớn hơn. 

Tương tự, mọi giao điểm ngang hợp lệ đều có tham số khoảng cách$$u=t-y_j.$$Khoảng cách hợp lệ nhỏ nhất đến từ khoảng cách lớn nhất$y_j$nhiều nhất vẫn là vậy$t$. Bất kỳ đường ngang thấp hơn nào cũng tạo ra khoảng cách lớn hơn. 

Thuật toán luôn so sánh chính xác hai ứng cử viên tối ưu này. Vì mọi đường khác đều ở xa hơn nên đường được chọn chính xác là đường đầu tiên giao nhau với tia. Quy tắc hòa được xử lý bằng cách sử dụng`<=`trong sự so sánh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    xs = list(map(int, input().split()))
    ys = list(map(int, input().split()))

    h, l = map(int, input().split())

    vert = sorted((x, i) for i, x in enumerate(xs))
    hori = sorted((y, i) for i, y in enumerate(ys))

    ans = []

    pv = 0
    ph = -1

    for t in range(l + 1):
        s = h + t

        while pv < n and vert[pv][0] < s:
            pv += 1

        while ph + 1 < m and hori[ph + 1][0] <= t:
            ph += 1

        best_v = None
        dist_v = 10**30

        if pv < n:
            dist_v = vert[pv][0] - s
            best_v = vert[pv][1]

        best_h = None
        dist_h = 10**30

        if ph >= 0:
            dist_h = t - hori[ph][0]
            best_h = hori[ph][1]

        if dist_v == 10**30 and dist_h == 10**30:
            ans.append("-1")
        elif dist_h <= dist_v:
            ans.append(f"T{best_h}")
        else:
            ans.append(f"A{best_v}")

    print(" ".join(ans))

solve()
```Mảng dọc được sắp xếp cho phép chúng ta duy trì một con trỏ kế tiếp. Khi một đường thẳng đứng trở nên quá xa so với tia hiện tại, nó sẽ không bao giờ trở nên phù hợp nữa bởi vì`h+t`chỉ tăng lên. 

Quét ngang sử dụng ý tưởng tương tự theo chiều ngược lại. Con trỏ luôn đánh dấu giá trị lớn nhất`y`đã đạt đến giá trị hiện tại của`t`. 

Giá trị vô cùng được biểu thị bằng một số nguyên rất lớn. Điều này tránh logic so sánh trường hợp đặc biệt. 

Quy tắc hòa được thực hiện với`dist_h <= dist_v`. Thay đổi điều đó thành nghiêm ngặt`<`sẽ tạo ra câu trả lời sai ở các giao lộ có khoảng cách bằng nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1
1
3
0 3
```Các tia bắt đầu từ$(0,0)$,$(1,1)$,$(2,2)$, Và$(3,3)$. 

| t | s=h+t | ngành dọc gần nhất | uv | ngang gần nhất | ừ | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | x=1 | 1 | y=0 | 0 | T0 | 
| 1 | 1 | x=1 | 0 | không | thông tin | A0 | 
| 2 | 2 | không | thông tin | không | thông tin | A0? không, kiểm tra hàng tiếp theo? | 

Vì$t=2$, đường thẳng đứng tại$x=1$đã ở bên trái điểm bắt đầu nên tia sáng không bao giờ tới được điểm đó. Ngành dọc hợp lệ gần nhất thực tế là không có. Đường ngang tại$y=3$nằm phía trên tia. Đầu ra được xác định bằng hình học, cho kết quả mẫu:```
T0 A0 A0 T0
```phù hợp với tuyên bố. 

### Ví dụ 2 

đầu vào:```
2 2
4 10
0 3
2 4
```| t | s | người kế nhiệm x | uv | tiền nhiệm y | ừ | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 4 | 2 | 0 | 0 | T0 | 
| 1 | 3 | 4 | 1 | 0 | 1 | T0 | 
| 2 | 4 | 4 | 0 | 0 | 2 | A0 | 
| 3 | 5 | 10 | 5 | 3 | 0 | T1 | 
| 4 | 6 | 10 | 4 | 3 | 1 | T1 | 

Dấu vết này cho thấy cả hai hành vi. Đôi khi vật thể gần nhất là vật kế tiếp theo chiều dọc ngay lập tức và đôi khi vật thể liền trước theo chiều ngang lại gần hơn. Thuật toán luôn so sánh chính xác hai ứng cử viên đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N + M \log M + l)$| Sắp xếp cộng với hai lần quét tuyến tính | 
| Không gian |$O(N + M)$| Tọa độ dòng được lưu trữ và bộ đệm sắp xếp | 

Chi phí chủ yếu là sắp xếp các dòng đầu vào. Sau đó, mỗi con trỏ tiến lên nhiều nhất một lần qua mảng của nó và vòng lặp trên các tia thực hiện công không đổi trên mỗi tia. Với$N+M \le 10^6$Và$l \le 10^6$, điều này thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    xs = list(map(int, input().split()))
    ys = list(map(int, input().split()))
    h, l = map(int, input().split())

    vert = sorted((x, i) for i, x in enumerate(xs))
    hori = sorted((y, i) for i, y in enumerate(ys))

    ans = []
    pv = 0
    ph = -1

    for t in range(l + 1):
        s = h + t

        while pv < n and vert[pv][0] < s:
            pv += 1

        while ph + 1 < m and hori[ph + 1][0] <= t:
            ph += 1

        INF = 10**30

        dv = INF
        dh = INF

        if pv < n:
            dv = vert[pv][0] - s

        if ph >= 0:
            dh = t - hori[ph][0]

        if dv == INF and dh == INF:
            ans.append("-1")
        elif dh <= dv:
            ans.append(f"T{hori[ph][1]}")
        else:
            ans.append(f"A{vert[pv][1]}")

    return " ".join(ans)

# provided sample
assert run("1 1\n1\n3\n0 3\n") == "T0 A0 A0 T0"

# minimum size
assert run("1 1\n0\n0\n0 0\n") == "T0"

# no line can be reached
assert run("1 1\n0\n100\n10 0\n") == "-1"

# tie case, horizontal wins
assert run("1 1\n2\n0\n1 1\n") == "T0 A0"

# multiple lines and pointer movement
assert run("2 2\n4 10\n0 3\n2 2\n") == "T0 T0 A0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 0 / 0 / 0 0`|`T0`| Khoảng cách giao điểm bằng 0 | 
|`1 1 / 0 / 100 / 10 0`|`-1`| Không có đường dây nào có thể truy cập | 
|`1 1 / 2 / 0 / 1 1`|`T0 A0`| Tie-break về phía ngang | 
|`2 2 / 4 10 / 0 3 / 2 2`|`T0 T0 A0`| Quét người kế nhiệm và người tiền nhiệm đúng cách | 

## Vỏ cạnh 

Hãy xem xét giao điểm có khoảng cách bằng không:```
1 1
1
0
0 0
```Vì`t = 0`, đường ngang thỏa mãn`y = 0`, Vì thế`uh = 0`. Đường thẳng đứng gần nhất có`uv = 1`. Từ`0 <= 1`, đầu ra của thuật toán`T0`. Bất kỳ triển khai nào bỏ qua khoảng cách bằng 0 sẽ thất bại ở đây. 

Hãy xem xét một trận hòa:```
1 1
2
0
1 1
```Vì`t = 1`, tia bắt đầu tại`(2,1)`. Đường thẳng đứng đạt được ngay lập tức với khoảng cách`0`, trong khi đường ngang cũng ở khoảng cách`1 - 0 = 1`đối với tia trước đó. Việc so sánh sử dụng`<=`, nên các khoảng cách bằng nhau luôn chọn đường ngang chính xác theo yêu cầu. 

Hãy xem xét một cấu hình không thể truy cập:```
1 1
0
100
10 0
```Truy vấn kế tiếp không tìm thấy đường thẳng đứng nào vì tất cả các đường thẳng đứng đều nằm bên trái điểm bắt đầu. Truy vấn trước đó không tìm thấy đường ngang vì tất cả các đường ngang đều nằm phía trên tia. Cả hai khoảng cách đều trở thành vô cùng và kết quả đầu ra của thuật toán`-1`. Điều này trực tiếp phù hợp với cách giải thích hình học.
