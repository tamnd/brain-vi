---
title: "CF 2063A - Đồng nguyên tố tối thiểu"
description: "Chúng ta có một phân đoạn nguyên $[l,r]$. Phân đoạn $[a,b]$ được gọi là cùng nguyên tố khi hai điểm cuối $a$ và $b$ có ước số chung lớn nhất bằng $1$. Trong số tất cả các phân khúc coprime, chúng tôi chỉ quan tâm đến những phân khúc tối thiểu."
date: "2026-06-08T07:28:07+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2063
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1000 (Div. 2)"
rating: 800
weight: 2063
solve_time_s: 110
verified: true
draft: false
---

[CF 2063A - Coprime tối thiểu](https://codeforces.com/problemset/problem/2063/A) 

**Đánh giá:** 800 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một phân đoạn số nguyên$[l,r]$. Một đoạn$[a,b]$được gọi là nguyên tố cùng nhau khi hai điểm cuối$a$Và$b$có ước chung lớn nhất bằng$1$. 

Trong số tất cả các phân khúc coprime, chúng tôi chỉ quan tâm đến những phân khúc tối thiểu. Một phân đoạn đồng nguyên tố là tối thiểu nếu không có phân đoạn đồng nguyên tố nhỏ hơn nào được chứa hoàn toàn bên trong nó. 

Đối với mỗi trường hợp thử nghiệm, chúng ta phải đếm xem có bao nhiêu phân đoạn nguyên tố tối thiểu được chứa trong phạm vi đã cho$[l,r]$. 

Những hạn chế rất rõ ràng. Các điểm cuối có thể lớn bằng$10^9$, có nghĩa là bất kỳ giải pháp nào lặp qua tất cả các phân đoạn đều không thể thực hiện được. Ngay cả một khoảng thời gian duy nhất$10^9$chứa khoảng$10^{18}$các phân đoạn phụ. Vì có tới 100 trường hợp thử nghiệm nên giải pháp dự định phải chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Phần khó khăn là hiểu được các phân khúc đồng nguyên tố tối thiểu thực sự trông như thế nào. 

Hãy xem xét phân khúc$[2,3]$. Từ$\gcd(2,3)=1$, nó là nguyên tố cùng nhau. Các phân đoạn chứa thích hợp duy nhất của nó là$[2,2]$Và$[3,3]$, cả hai đều không nguyên tố cùng nhau vì$\gcd(2,2)=2$Và$\gcd(3,3)=3$. Như vậy$[2,3]$là tối thiểu. 

Bây giờ hãy xem xét$[2,5]$. Nó nguyên tố cùng nhau vì$\gcd(2,5)=1$, nhưng nó chứa$[2,3]$, vốn đã là phân khúc đồng nguyên. Kể từ đây$[2,5]$không thể là tối thiểu. 

Trường hợp đặc biệt dễ bỏ sót là số 1. Đoạn$[1,1]$là nguyên tố cùng nhau vì$\gcd(1,1)=1$. Vì nó không chứa phân đoạn nào nhỏ hơn nên nó là tối thiểu. 

Ví dụ: 

đầu vào:```
1 1
```Đầu ra:```
1
```Một giải pháp bất cẩn giả định rằng mọi phân đoạn có độ dài 1 đều không nguyên tố cùng nhau sẽ trả về 0 không chính xác. 

Một trường hợp quan trọng khác là: 

đầu vào:```
2 2
```Đầu ra:```
0
```Đây$\gcd(2,2)=2$, do đó phân khúc không nguyên tố cùng nhau. 

Một ví dụ nữa: 

đầu vào:```
2 4
```Các phân khúc coprime tối thiểu chỉ$[2,3]$Và$[3,4]$, vậy đáp án là 2 

Một nỗ lực ngây thơ có thể đếm tất cả các cặp nguyên tố cùng nhau, bao gồm cả$[2,4]$, nhưng đoạn đó chứa$[2,3]$và không phải là tối thiểu. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Liệt kê mọi phân đoạn chứa$[a,b]$, kiểm tra xem$\gcd(a,b)=1$, sau đó xác minh xem nó có chứa phân đoạn nguyên tố cùng nhau khác hay không. 

Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Thật không may, một phạm vi chiều dài$n$chứa$O(n^2)$các phân đoạn phụ. Kể cả nếu$n$chỉ là$10^5$, điều này có nghĩa là về$5 \times 10^9$phân đoạn. Với điểm cuối lên đến$10^9$, vũ lực là hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là các phân đoạn đồng nguyên tố tối thiểu có cấu trúc cực kỳ cứng nhắc. 

Giả định$[a,b]$là tối thiểu và$a<b$. 

Nếu như$b-a\ge 2$, thì đoạn chứa$[a,a+1]$nằm bên trong nó. Các số nguyên liên tiếp luôn nguyên tố cùng nhau:$$\gcd(a,a+1)=1$$Vì thế$[a,a+1]$là một đoạn nguyên tố cùng nhau chứa bên trong$[a,b]$. Điều đó mâu thuẫn với sự tối giản. 

Do đó, mọi phân đoạn đồng nguyên tố tối thiểu phải có độ dài 0 hoặc 1. 

Độ dài 0 có nghĩa là$[x,x]$. Đoạn như vậy chỉ nguyên tố cùng nhau khi$x=1$, bởi vì$$\gcd(x,x)=x.$$Do đó, đoạn tối thiểu duy nhất có độ dài 0 là$[1,1]$. 

Độ dài 1 có nghĩa là$[x,x+1]$. Các số nguyên liên tiếp luôn nguyên tố cùng nhau và các phân đoạn được chứa duy nhất là$[x,x]$Và$[x+1,x+1]$. Vì$x\ge2$, cả hai đều không nguyên tố cùng nhau, vì vậy$[x,x+1]$là tối thiểu. 

Vụ án$x=1$là đặc biệt. Phân khúc$[1,2]$chứa$[1,1]$, đó là nguyên tố cùng nhau, vì vậy$[1,2]$không phải là tối thiểu. 

Chúng tôi đã mô tả đầy đủ tất cả các phân khúc coprime tối thiểu: 

1.$[1,1]$2.$[x,x+1]$cho mọi$x\ge2$Bây giờ việc đếm trở nên dễ dàng. 

Mỗi cặp liền kề$[x,x+1]$bên trong$[l,r]$đóng góp một phân đoạn tối thiểu. Những cặp như vậy tồn tại với mọi$$x \in [l,r-1].$$có$r-l$của họ. 

Điều chỉnh duy nhất là khi phạm vi chứa phân đoạn đặc biệt$[1,1]$. Điều đó xảy ra chính xác khi$l=1$. 

Nếu như$l=r=1$, câu trả lời là 1. 

Ngược lại, khi$l=1$, cặp$[1,2]$được bao gồm trong số lượng$r-l$, nhưng nó không phải là tối thiểu. Chúng ta phải thay thế nó bằng$[1,1]$. Tổng số còn lại$r-l$. 

Điều này dẫn đến một công thức cực kỳ đơn giản: 

Nếu$(l,r)=(1,1)$, trả lời 1. 

Còn không thì trả lời$r-l$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((r-l+1)^2)$hoặc tệ hơn |$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$l$Và$r$. 
2. Kiểm tra xem$l=r=1$. 

Đây là phạm vi duy nhất mà câu trả lời không được bao hàm trong công thức sai phân đơn giản. 
3. Nếu$l=r=1$, in 1. 
4. Nếu không, hãy in$r-l$. 

Lý do bước 4 có hiệu quả là vì mọi phân đoạn nguyên tố cùng cực tối thiểu trong khoảng tương ứng với chính xác một cặp liền kề$[x,x+1]$với$x\ge2$, cộng với singleton đặc biệt$[1,1]$. Việc đếm luôn đơn giản hóa thành$r-l$cho mọi khoảng thời gian ngoại trừ$[1,1]$. 

### Tại sao nó hoạt động 

Bất kỳ đoạn nào có độ dài ít nhất bằng 2 đều chứa một cặp liên tiếp$[x,x+1]$, bản thân nó là nguyên tố cùng nhau. Một phân khúc như vậy không thể là tối thiểu. 

Do đó mọi đoạn cùng nguyên tố tối thiểu đều có độ dài 0 hoặc 1. 

Trong số các đoạn có độ dài 0, chỉ$[1,1]$là nguyên tố cùng nhau. 

Trong số các đoạn có độ dài 1, mọi$[x,x+1]$là nguyên tố cùng nhau. Nó là tối thiểu chính xác khi$x\ge2$, bởi vì$[1,2]$chứa đoạn nguyên tố cùng nhau$[1,1]$. 

Đây đều là những phân đoạn đồng nguyên tố tối thiểu có thể có, vì vậy việc đếm chúng bên trong$[l,r]$thu được công thức trên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    l, r = map(int, input().split())

    if l == 1 and r == 1:
        print(1)
    else:
        print(r - l)
```Việc thực hiện trực tiếp tuân theo đặc tính đã được chứng minh ở trên. 

Điều kiện đầu tiên xử lý khoảng duy nhất chỉ bao gồm số 1. Câu trả lời của nó là 1 vì phân đoạn$[1,1]$là nguyên tố cùng nhau tối thiểu. 

Mỗi khoảng khác sử dụng công thức$r-l$. Từ$l$Và$r$nhiều nhất là$10^9$, số học số nguyên thông thường là đủ. Số nguyên Python dễ dàng xử lý các giá trị này. 

Lỗi phổ biến nhất là việc đếm quá phức tạp và cố gắng suy luận về các giá trị gcd trong suốt khoảng thời gian. Khi đã hiểu được cấu trúc của các phân đoạn tối thiểu, chỉ có trường hợp đặc biệt$[1,1]$vẫn còn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 10
```| Biến | Giá trị | 
| --- | --- | 
| tôi | 1 | 
| r | 10 | 
| l == 1 và r == 1 | Sai | 
| Trả lời | 10 - 1 = 9 | 

Đầu ra:```
9
```Các phân khúc coprime tối thiểu là:$$[1,1], [2,3], [3,4], \dots, [9,10]$$Có chính xác 9 người trong số họ. 

### Ví dụ 2 

đầu vào:```
49 49
```| Biến | Giá trị | 
| --- | --- | 
| tôi | 49 | 
| r | 49 | 
| l == 1 và r == 1 | Sai | 
| Trả lời | 49 - 49 = 0 | 

Đầu ra:```
0
```Phân đoạn chứa duy nhất là$[49,49]$. Từ$\gcd(49,49)=49$, nó không phải là phân đoạn cùng nguyên tố, vì vậy không tồn tại phân đoạn đồng nguyên tố tối thiểu. 

Ví dụ này chứng minh rằng hầu hết các phân đoạn đơn lẻ không đóng góp gì cả. Chỉ một$[1,1]$là đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$mỗi trường hợp thử nghiệm | Chỉ một vài so sánh và một phép trừ | 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu phụ trợ | 

Với tối đa 100 trường hợp thử nghiệm, tổng thời gian chạy thực sự là tức thời và dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        l, r = map(int, input().split())

        if l == 1 and r == 1:
            ans.append("1")
        else:
            ans.append(str(r - l))

    print("\n".join(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run(
"""6
1 2
1 10
49 49
69 420
1 1
9982 44353
"""
) == """1
9
0
351
1
34371
"""

# minimum interval containing 1
assert run(
"""1
1 1
"""
) == """1
"""

# singleton interval not equal to 1
assert run(
"""1
2 2
"""
) == """0
"""

# smallest adjacent pair after 1
assert run(
"""1
2 3
"""
) == """1
"""

# large boundary values
assert run(
"""1
1 1000000000
"""
) == """999999999
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Đoạn đơn đặc biệt | 
|`2 2`|`0`| Singleton không đồng nguyên | 
|`2 3`|`1`| Một cặp liền kề tối thiểu | 
|`1 1000000000`|`999999999`| Số học phạm vi tối đa | 
| Đầu vào mẫu | Đầu ra mẫu | Phù hợp với các ví dụ chính thức | 

## Vỏ cạnh 

### Trường hợp cạnh 1: Phân khúc đặc biệt [1,1] 

đầu vào:```
1
1 1
```Thực thi: 

| Bước | Giá trị | 
| --- | --- | 
| Kiểm tra`l == 1 and r == 1`| Đúng | 
| Đầu ra | 1 | 

Phân khúc$[1,1]$là nguyên tố cùng nhau vì$\gcd(1,1)=1$. Vì nó không chứa phân đoạn nào nhỏ hơn nên nó là tối thiểu. 

### Trường hợp Edge 2: Khoảng đơn lớn hơn 1 

đầu vào:```
1
7 7
```Thực thi: 

| Bước | Giá trị | 
| --- | --- | 
| Kiểm tra trường hợp đặc biệt | Sai | 
| Tính toán`r - l`| 0 | 
| Đầu ra | 0 | 

Phân đoạn chứa duy nhất là$[7,7]$, Và$\gcd(7,7)=7$, do đó không có phân đoạn nguyên tố cùng cực tối thiểu. 

### Trường hợp cạnh 3: Khoảng thời gian bắt đầu từ 1 

đầu vào:```
1
1 2
```Thực thi: 

| Bước | Giá trị | 
| --- | --- | 
| Kiểm tra trường hợp đặc biệt | Sai | 
| Tính toán`r - l`| 1 | 
| Đầu ra | 1 | 

Phân khúc coprime tối thiểu duy nhất là$[1,1]$. Phân khúc$[1,2]$là nguyên tố cùng nhau nhưng không tối thiểu vì nó chứa$[1,1]$. 

### Trường hợp Edge 4: Khoảng cách dài 

đầu vào:```
1
5 100
```Thực thi: 

| Bước | Giá trị | 
| --- | --- | 
| Kiểm tra trường hợp đặc biệt | Sai | 
| Tính toán`r - l`| 95 | 
| Đầu ra | 95 | 

Mỗi cặp liền kề$[x,x+1]$vì$x=5,6,\dots,99$đóng góp một phân đoạn coprime tối thiểu, mang lại chính xác$100-5=95$phân đoạn. Không còn phân đoạn nào có thể là tối thiểu vì nó chứa một trong các cặp nguyên tố cùng nhau liền kề này.
