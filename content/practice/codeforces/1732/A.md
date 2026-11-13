---
title: "CF 1732A - Bestie"
description: "Chúng tôi được cung cấp một mảng các số nguyên. Chúng ta có thể liên tục chọn chỉ mục $i$ và thay thế $ai$ bằng $gcd(ai, i)$. Việc thực hiện thao tác này tốn $n-i+1$, do đó các thao tác trên các vị trí gần cuối mảng sẽ rẻ hơn."
date: "2026-06-09T18:34:07+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "constructive-algorithms", "implementation", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1732
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 830 (Div. 2)"
rating: 1000
weight: 1732
solve_time_s: 535
verified: true
draft: false
---

[CF 1732A - Bestie](https://codeforces.com/problemset/problem/1732/A) 

**Đánh giá:** 1000 
**Tags:** lực lượng vũ phu, tổ hợp, thuật toán xây dựng, thực hiện, toán học, lý thuyết số 
**Thời gian giải:** 8 phút 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên. Chúng tôi có thể liên tục chọn một chỉ mục$i$và thay thế$a_i$qua$\gcd(a_i, i)$. Chi phí thực hiện hoạt động này$n-i+1$, do đó các thao tác trên các vị trí gần cuối mảng sẽ rẻ hơn. 

Mục tiêu là làm cho gcd của toàn bộ mảng bằng$1$với tổng chi phí tối thiểu. 

Các hoạt động có một hình thức rất hạn chế. Chúng tôi không được tự do gán các giá trị tùy ý. Sau khi vận hành ở vị trí$i$, giá trị trở thành$\gcd(a_i,i)$, luôn là ước số của$i$. Từ$n \le 20$, mảng cực kỳ nhỏ. Điều này ngay lập tức gợi ý rằng một giải pháp liên quan đến việc thử một vài khả năng là khả thi. Số lượng lớn các ca kiểm thử không quan trọng lắm vì mỗi ca kiểm thử chứa tối đa 20 phần tử. 

Quan sát đầu tiên là khi gcd của toàn bộ mảng trở thành$1$, chúng ta đã xong. Các giá trị thực tế không quan trọng sau thời điểm đó. 

Trường hợp cạnh tinh vi xảy ra khi mảng đã có gcd$1$. 

Ví dụ:```
n = 4
a = [5, 10, 15, 20]
```Gcd đã có rồi$1$, vậy câu trả lời là$0$. Bất kỳ giải pháp nào luôn thực hiện ít nhất một thao tác đều sai. 

Một trường hợp thú vị khác là:```
n = 1
a = [2]
```Áp dụng thao tác tại chỉ mục$1$thay đổi giá trị thành$\gcd(2,1)=1$. Chi phí là$1$. Câu trả lời là không$0$. 

Trường hợp cạnh thứ ba là khi một thao tác là không đủ, nhưng sự kết hợp rất rẻ thì có.```
n = 5
a = [120, 60, 80, 40, 80]
```Thay đổi vị trí$5$cho$5$, bản thân điều đó là không đủ. Thay đổi vị trí$4$Và$5$đưa ra các giá trị chia hết cho$4$Và$5$, gcd của ai là$1$. Câu trả lời tối ưu là$3$, không phải chi phí của một số vị trí trước đó. 

Những ví dụ này gợi ý rằng câu hỏi chính không phải là bản thân các giá trị cuối cùng mà là gcd của toàn bộ mảng thay đổi như thế nào sau khi sửa đổi một vài vị trí. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi chuỗi hoạt động. Ngay cả khi chỉ có 20 vị trí, số lượng tập hợp con các phép toán có thể có là$2^{20}$và mỗi vị trí có thể được chọn nhiều lần. Việc tìm kiếm như vậy phát triển theo cấp số nhân và không cần thiết. 

Cấu trúc của hoạt động cung cấp một sự quan sát mạnh mẽ hơn nhiều. Giả sử gcd hiện tại của toàn bộ mảng là$g$. 

Nếu chúng ta sửa đổi vị trí$i$, gcd toàn cầu mới sẽ trở thành$$\gcd\bigl(g,\gcd(a_i,i)\bigr).$$Điều này xảy ra vì mọi phần tử khác vẫn đóng góp bội số của$g$và chỉ vị trí đã chọn thay đổi. 

Bây giờ hãy nhìn vào chi phí:$$\text{cost}(n)=1,\qquad \text{cost}(n-1)=2,\qquad \text{cost}(n-2)=3.$$Các vị trí rẻ nhất luôn ở cuối mảng. 

Quan sát quan trọng là câu trả lời không bao giờ lớn hơn$3$. 

Nếu chúng ta áp dụng các phép toán cho các vị trí$n$,$n-1$, Và$n-2$, thì gcd toàn cầu sẽ trở thành$$\gcd(g,n,n-1,n-2).$$Trong ba số nguyên liên tiếp thì gcd luôn là$1$. Vì thế$$\gcd(g,n,n-1,n-2)=1.$$Tổng chi phí là$$1+2+3=6.$$Nhưng các giải pháp biên tập của Codeforces sử dụng một thực tế thậm chí còn mạnh mẽ hơn: vì$n\le 20$, chúng ta chỉ cần kiểm tra xem sử dụng không, một, hai hay ba vị trí cuối cùng là đủ. Câu trả lời tối ưu luôn nằm trong số những khả năng đó. 

Chúng ta có thể kiểm tra trực tiếp: 

- Chi phí$0$: không làm gì cả 
- Trị giá$1$: chỉ áp dụng ở vị trí$n$. 
- Trị giá$2$: chỉ áp dụng ở vị trí$n-1$. 
- Trị giá$3$: chỉ áp dụng ở vị trí$n-2$hoặc sử dụng sự kết hợp rẻ hơn tương ứng một cách hiệu quả với các câu trả lời được tìm thấy bằng cách kiểm tra các lần giảm gcd liên tiếp. 

Giải pháp được chấp nhận chỉ đơn giản là kiểm tra những khả năng này theo thứ tự chi phí tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính gcd$g$của toàn bộ mảng. 
2. Nếu$g=1$, trở lại$0$. Mục tiêu đã đạt được rồi. 
3. Kiểm tra xem việc áp dụng thao tác tại vị trí$n$một mình là đủ. Sau hoạt động này, gcd toàn cầu mới sẽ trở thành$$\gcd(g,n).$$Nếu điều này bằng$1$, chi phí trả lại$1$. 

1. Kiểm tra xem việc áp dụng thao tác tại vị trí$n-1$một mình là đủ. Kết quả gcd trở thành$$\gcd(g,n-1).$$Nếu điều này bằng$1$, chi phí trả lại$2$. 

1. Kiểm tra xem việc áp dụng thao tác tại vị trí$n-2$một mình là đủ. Kết quả gcd trở thành$$\gcd(g,n-2).$$Nếu điều này bằng$1$, chi phí trả lại$3$. 

1. Nếu không có lần kiểm tra nào trước đó thành công thì câu trả lời là$3$. 

Bước cuối cùng tiếp theo từ thuộc tính đã biết được sử dụng trong giải pháp chính thức. Bằng cách sửa đổi một số vị trí cuối cùng, chúng ta luôn có thể buộc gcd toàn cầu trở thành$1$, và chi phí còn lại tối thiểu chính xác là$3$. 

### Tại sao nó hoạt động 

hãy để$g$là gcd của mảng ban đầu. 

Thực hiện thao tác tại vị trí$i$chỉ có thể ảnh hưởng đến gcd toàn cầu thông qua số lượng$i$, vì giá trị mới trở thành ước số của$i$. Kết quả gcd toàn cầu chính xác là$\gcd(g,i)$. 

Vì chi phí tăng khi chúng ta di chuyển sang trái nên trước tiên chúng ta nên thử vị trí rẻ nhất$n$, thì rẻ nhất tiếp theo$n-1$, sau đó$n-2$. 

Nếu không có thao tác riêng lẻ nào trong số này làm giảm gcd thành$1$, đối số chuẩn cho bài toán này cho thấy rằng việc sử dụng một vài vị trí cuối cùng luôn đạt được gcd$1$với tổng chi phí$3$. Do đó kiểm tra chi phí$0$,$1$,$2$, Và$3$cạn kiệt mọi khả năng. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    g = 0
    for x in a:
        g = gcd(g, x)

    if g == 1:
        print(0)
    elif gcd(g, n) == 1:
        print(1)
    elif gcd(g, n - 1) == 1:
        print(2)
    else:
        print(3)
```Mã bắt đầu bằng cách tính gcd của toàn bộ mảng. Đây là thông tin duy nhất chúng ta thực sự cần từ mảng. 

Điều kiện đầu tiên xử lý các mảng có gcd đã có sẵn$1$. 

Hai điều kiện tiếp theo kiểm tra các hoạt động rẻ nhất có thể. Chức vụ$n$chi phí$1$, và vị trí$n-1$chi phí$2$. 

Nếu cả hai đều không hoạt động, thuộc tính đã biết của vấn đề đảm bảo rằng chi phí$3$là đủ, vì vậy chúng tôi in$3$. 

Một lỗi phổ biến là cố gắng mô phỏng các sửa đổi mảng thực tế. Điều đó là không cần thiết. Chỉ có gcd toàn cầu mới quan trọng và sự thay đổi của nó có thể được tính toán trực tiếp bằng một thao tác gcd khác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [3, 6, 9]
```gcd ban đầu: 

| Bước | Giá trị | 
| --- | --- | 
| gcd(3,6) | 3 | 
| gcd(3,9) | 3 | 

Vì thế$g=3$. 

| Kiểm tra | Kết quả | 
| --- | --- | 
| gcd(3,3) | 3 | 
| gcd(3,2) | 1 | 

Lần kiểm tra thứ hai thành công nên câu trả lời là$2$. 

Điều này chứng tỏ rằng một thao tác đơn lẻ trên vị trí$n-1$đã có thể làm cho toàn bộ mảng gcd bằng$1$. 

### Ví dụ 2 

đầu vào:```
n = 5
a = [120, 60, 80, 40, 80]
```gcd ban đầu: 

| Bước | Giá trị | 
| --- | --- | 
| gcd(120,60) | 60 | 
| gcd(60,80) | 20 | 
| gcd(20,40) | 20 | 
| gcd(20,80) | 20 | 

Như vậy$g=20$. 

| Kiểm tra | Kết quả | 
| --- | --- | 
| gcd(20,5) | 5 | 
| gcd(20,4) | 4 | 

Cả hai séc đều không tạo ra$1$, vậy câu trả lời là$3$. 

Đây chính xác là mẫu cần có nhiều sửa đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần để tính mảng gcd | 
| Không gian |$O(1)$| Chỉ có một vài biến được lưu trữ | 

Từ$n \le 20$, giải pháp này thấp hơn nhiều so với giới hạn. Ngay cả với$5000$trường hợp thử nghiệm, tổng công việc là rất nhỏ. 

## Trường hợp thử nghiệm```python
import sys
import io
from math import gcd

def solve():
    input = sys.stdin.readline
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        g = 0
        for x in a:
            g = gcd(g, x)

        if g == 1:
            ans.append("0")
        elif gcd(g, n) == 1:
            ans.append("1")
        elif gcd(g, n - 1) == 1:
            ans.append("2")
        else:
            ans.append("3")

    return "\n".join(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

assert run("1\n1\n1\n") == "0", "already gcd 1"
assert run("1\n1\n2\n") == "1", "single element"
assert run("1\n2\n2 4\n") == "2", "sample case"
assert run("1\n3\n3 6 9\n") == "2", "sample case"
assert run("1\n6\n2 4 6 9 12 18\n") == "0", "gcd already 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$[1]$|$0$| Đã được giải quyết | 
|$[2]$|$1$| Hoạt động đơn lẻ tại vị trí 1 | 
|$[2,4]$|$2$| Trường hợp giá 2 | 
|$[3,6,9]$|$2$| Chức vụ$n-1$hoạt động | 
|$[2,4,6,9,12,18]$|$0$| Hiện tại gcd 1 | 

## Vỏ cạnh 

Hãy xem xét:```
n = 1
a = [1]
```Gcd toàn cầu đã có sẵn$1$. Thuật toán dừng ngay lập tức và quay trở lại$0$. 

Coi như:```
n = 1
a = [2]
```gcd là$2$. Chúng tôi kiểm tra$\gcd(2,1)=1$, vậy là chỉ cần một thao tác là đủ và câu trả lời là$1$. 

Coi như:```
n = 2
a = [2,4]
```Gcd toàn cầu là$2$. Vị trí kiểm tra$2$cho$\gcd(2,2)=2$. Vị trí kiểm tra$1$cho$\gcd(2,1)=1$. Câu trả lời trở thành$2$. 

Coi như:```
n = 5
a = [120,60,80,40,80]
```Gcd toàn cầu là$20$. Không$\gcd(20,5)$cũng không$\gcd(20,4)$bằng$1$. Thuật toán đi đến câu trả lời cuối cùng$3$, phù hợp với giải pháp tối ưu.
