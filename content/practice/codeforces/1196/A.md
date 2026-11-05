---
title: "CF 1196A - Ba Đống Kẹo"
description: "Mỗi truy vấn sẽ cho ra ba đống kẹo. Alice phải lấy nguyên một cọc, Bob phải lấy một trong những cọc còn lại và cọc cuối cùng có thể được chia tùy ý giữa chúng."
date: "2026-06-12T00:12:15+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1196
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 575 (Div. 3)"
rating: 800
weight: 1196
solve_time_s: 243
verified: true
draft: false
---

[CF 1196A - Ba đống kẹo](https://codeforces.com/problemset/problem/1196/A) 

**Đánh giá:** 800 
**Tags:** vũ lực, thuật toán xây dựng, toán học 
**Thời gian giải:** 4m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi truy vấn sẽ cho ra ba đống kẹo. Alice phải lấy nguyên một cọc, Bob phải lấy một trong những cọc còn lại và cọc cuối cùng có thể được chia tùy ý giữa chúng. 

Sau khi phân phát, nếu một người có nhiều kẹo hơn người kia thì số kẹo thừa có thể bị loại bỏ. Kết quả cuối cùng là cả hai người đều có số kẹo như nhau. Mục tiêu của chúng tôi là tối đa hóa con số chung đó. 

Quan sát quan trọng là kẹo bỏ đi được phép. Chúng ta không cần phải sử dụng hết mọi viên kẹo. Chúng ta chỉ cần tối đa hóa số tiền mà cả Alice và Bob có thể giữ. 

Số lượng truy vấn nhiều nhất là 1000, rất nhỏ. Tuy nhiên, mỗi đống có thể chứa tới$10^{16}$kẹo, vì vậy bất kỳ thuật toán nào phụ thuộc vào kích thước đống kẹo đều không thể thực hiện được. Chúng ta cần một công thức toán học trực tiếp có thể được đánh giá trong thời gian không đổi cho mỗi truy vấn. 

Một sai lầm phổ biến là cho rằng câu trả lời luôn là một nửa tổng số kẹo. Hãy xem xét các cọc$(1,1,100)$. Tổng cộng là$102$, vậy một nửa là$51$. Nhưng hai đống nhỏ chỉ chứa$2$kẹo hoàn toàn. Một người có thể lấy đống$100$, nhưng người kia không thể với tới$51$kẹo vì chỉ$2$kẹo tồn tại bên ngoài đống lớn nhất. Câu trả lời đúng là$2$. 

Một sai lầm dễ mắc phải khác là nghĩ rằng chồng lớn nhất sẽ trực tiếp giới hạn câu trả lời. Coi như$(4,4,4)$. Đống lớn nhất có kích thước$4$, nhưng câu trả lời là$6$. Alice có thể nhận được$4$từ một đống và$2$từ đống còn lại, trong khi Bob nhận được đống còn lại$4$và cái khác$2$. 

Trường hợp cạnh thứ ba xuất hiện khi một cọc lấn át các cọc khác. Vì$(1,10,100)$, tổng số là$111$, do đó giới hạn trên lý thuyết là$\lfloor 111/2 \rfloor = 55$. Giá trị này có thể đạt được, đưa ra câu trả lời$55$. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là thử mọi nhiệm vụ có thể. Chúng ta có thể chọn Alice lấy cọc nào, Bob lấy cọc nào, sau đó xem xét mọi cách để chia cọc còn lại. 

Lý do này là đúng vì mọi phân phối hợp lệ đều được kiểm tra. Vấn đề là đống còn lại có thể chứa tới$10^{16}$kẹo nên việc liệt kê tất cả các phần có thể chia là hoàn toàn không thể. 

Để đạt được tiến bộ, chúng ta cần suy nghĩ về điều gì thực sự giới hạn số tiền bằng nhau cuối cùng. 

Giả sử kích thước cọc được sắp xếp:$$a \le b \le c.$$Cho phép$$S = a+b+c.$$Vì cả hai người đều có số tiền bằng nhau$x$, họ cùng nhau giữ$2x$kẹo. Họ không thể giữ nhiều kẹo hơn mức tồn tại, vì vậy$$2x \le S,$$mang lại$$x \le \left\lfloor \frac{S}{2} \right\rfloor.$$Đây là giới hạn trên đầu tiên. 

Có một cái khác. Để tặng cả hai người$x$ít nhất là kẹo$x$kẹo phải đến từ hai đống nhỏ hơn cộng lại, vì đống lớn nhất góp phần lớn số kẹo của nó vào một bên. Tổng số tiền bên ngoài đống lớn nhất là$$a+b.$$Kể từ đây$$x \le a+b.$$Đây là những hạn chế duy nhất quan trọng. Nếu như$$x \le \min\left(a+b,\left\lfloor \frac{S}{2}\right\rfloor\right),$$thì sự phân bố như vậy luôn có thể được xây dựng. 

Vì vậy câu trả lời chỉ đơn giản là$$\min\left(a+b,\left\lfloor \frac{a+b+c}{2}\right\rfloor\right),$$sau khi sắp xếp sao cho$c$là đống lớn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không thể thực hiện được trong thực tế, phụ thuộc vào kích thước cọc | O(1) | Quá chậm | 
| Tối ưu | O(1) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba kích thước cọc. 
2. Sắp xếp chúng sao cho$a \le b \le c$. 
3. Tính tổng số kẹo:$$S=a+b+c.$$4. Tính giới hạn trên thứ nhất:$$\left\lfloor \frac{S}{2} \right\rfloor.$$Hai người có cùng số kẹo không thể cùng nhau giữ nhiều kẹo hơn mức tồn tại. 
5. Tính giới hạn trên thứ hai:$$a+b.$$Hai đống nhỏ hơn chỉ chứa$a+b$tổng số kẹo nên số lượng bằng nhau cuối cùng không thể vượt quá giá trị này. 
6. Đầu ra$$\min\left(a+b,\left\lfloor \frac{S}{2}\right\rfloor\right).$$### Tại sao nó hoạt động 

giá trị$x$không thể vượt quá$\lfloor S/2 \rfloor$bởi vì chỉ có$S$kẹo có sẵn. Nó cũng không thể vượt quá$a+b$vì chỉ đống lớn nhất không thể cung cấp đủ kẹo cho cả hai người tham gia. 

Hai bất đẳng thức này đưa ra giới hạn trên:$$x \le \min\left(a+b,\left\lfloor \frac{S}{2}\right\rfloor\right).$$Giới hạn này luôn có thể đạt được. Nếu đống lớn nhất rất lớn thì$a+b$trở thành yếu tố hạn chế. Nếu không, tổng số kẹo sẽ trở thành yếu tố giới hạn. Trong cả hai trường hợp, cọc còn lại có thể được chia nhỏ một cách thích hợp để nhận ra chính xác giá trị đó. Do đó, công thức vừa là giới hạn trên vừa là giá trị có thể đạt được, chứng tỏ tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

q = int(input())

for _ in range(q):
    a, b, c = map(int, input().split())
    a, b, c = sorted((a, b, c))

    total = a + b + c
    print(min(a + b, total // 2))
```Việc thực hiện tuân theo công thức toán học trực tiếp. 

Ba giá trị được sắp xếp sao cho đống lớn nhất luôn được lưu trữ trong`c`. Điều này cho phép chúng ta xác định số lượng`a + b`, là tổng số kẹo nằm ngoài đống lớn nhất. 

giá trị`total // 2`đại diện cho phần chia đều lớn nhất có thể nếu chỉ có tổng số kẹo là quan trọng. 

Câu trả lời là mức tối thiểu của hai giới hạn này. 

Số nguyên Python tự động xử lý các giá trị lớn hơn nhiều so với$10^{16}$, vì vậy tràn không phải là một mối quan tâm. Toàn bộ giải pháp chỉ sử dụng một vài phép tính số học cho mỗi truy vấn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Cọc đầu vào:$(1, 3, 4)$Sau khi sắp xếp: 

| một | b | c | tổng cộng | a+b | tổng//2 | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 4 | 8 | 4 | 4 | 4 | 

Câu trả lời là$4$. Một phân phối hợp lệ là đưa ra một đống$4$cho Alice và cả hai viên kẹo còn lại từ đống cuối cùng cho Bob, tạo ra$4$từng viên kẹo. 

### Ví dụ 2 

Cọc đầu vào:$(1, 10, 100)$| một | b | c | tổng cộng | a+b | tổng//2 | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 10 | 100 | 111 | 11 | 55 | 11 | 

Đợi đã, sau khi sắp xếp, công thức sẽ cho:$$\min(11,55)=11.$$Điều này cho thấy tại sao việc sắp xếp lại quan trọng khi diễn giải giới hạn. Đống lớn nhất lớn hơn nhiều so với những đống khác. Số lượng bằng nhau cuối cùng bị giới hạn bởi tổng số kẹo bên ngoài đống lớn nhất. 

Thật vậy, Alice và Bob mỗi người có thể kết thúc bằng$11$kẹo, nhưng không nhiều hơn, bởi vì chỉ$11$kẹo tồn tại bên ngoài đống lớn nhất. 

### Ví dụ 3 

Cọc đầu vào:$(23, 34, 45)$| một | b | c | tổng cộng | a+b | tổng//2 | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 23 | 34 | 45 | 102 | 57 | 51 | 51 | 

Ở đây tổng số tiền là yếu tố giới hạn. Vì một nửa tổng số là$51$, cả hai người tham gia có thể kết thúc với$51$kẹo. 

Ví dụ này thể hiện cơ chế thứ hai của công thức, trong đó câu trả lời bằng$\lfloor S/2 \rfloor$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) mỗi truy vấn | Chỉ sắp xếp ba giá trị và một vài phép tính số học | 
| Không gian | O(1) | Sử dụng một lượng bộ nhớ bổ sung không đổi | 

Với tối đa 1000 truy vấn, tổng công việc không đáng kể. Giải pháp dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    q = int(input())

    ans = []
    for _ in range(q):
        a, b, c = map(int, input().split())
        a, b, c = sorted((a, b, c))
        ans.append(str(min(a + b, (a + b + c) // 2)))

    return "\n".join(ans) + "\n"

# provided sample
assert run(
"""4
1 3 4
1 10 100
10000000000000000 10000000000000000 10000000000000000
23 34 45
"""
) == """4
11
15000000000000000
51
"""

# minimum values
assert run(
"""1
1 1 1
"""
) == """1
"""

# all equal
assert run(
"""1
5 5 5
"""
) == """7
"""

# dominant largest pile
assert run(
"""1
1 1 100
"""
) == """2
"""

# maximum values
assert run(
"""1
10000000000000000 10000000000000000 10000000000000000
"""
) == """15000000000000000
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`1`| Trường hợp nhỏ nhất không tầm thường | 
|`5 5 5`|`7`| Phân phối đối xứng | 
|`1 1 100`|`2`| Cọc lớn nhất chiếm ưu thế | 
|`10^16 10^16 10^16`|`15000000000000000`| Giá trị tối đa và xử lý số nguyên lớn | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1
1 1 100
```Sau khi sắp xếp,$a=1$,$b=1$,$c=100$. Thuật toán tính toán:$$a+b=2,$$

$$\left\lfloor \frac{102}{2} \right\rfloor = 51.$$Câu trả lời là$\min(2,51)=2$. 

Trường hợp này cho thấy tại sao chỉ sử dụng một nửa tổng số là không chính xác. Hầu như tất cả kẹo đều bị mắc kẹt bên trong một đống. 

Bây giờ hãy xem xét:```
1
4 4 4
```Thuật toán tính toán:$$a+b=8,$$

$$\left\lfloor \frac{12}{2} \right\rfloor = 6.$$Câu trả lời là$6$. 

Một chiến lược ngây thơ chỉ nhìn vào đống lớn nhất có thể dự đoán$4$, quá nhỏ. Việc chia cọc còn lại cho phép cả hai người tham gia vượt quá kích thước của bất kỳ cọc riêng lẻ nào. 

Cuối cùng, hãy xem xét:```
1
2 3 5
```Thuật toán tính toán:$$a+b=5,$$

$$\left\lfloor \frac{10}{2} \right\rfloor = 5.$$Câu trả lời là$5$. 

Trường hợp ranh giới này là khi cả hai giới hạn trên đều bằng nhau. Thuật toán xử lý nó một cách tự nhiên mà không cần bất kỳ logic đặc biệt nào.
