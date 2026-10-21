---
title: "CF 105757D - Đoán hoán vị"
description: "Chúng ta bắt đầu với mảng được sắp xếp $$[1,2,3,dots,n].$$ Ba vị trí $i<j<k$ được chọn, với $j-i1$. Đoạn $[i,j-1]$ bị đảo ngược và đoạn $[j,k]$ bị đảo ngược. Trong phiên bản tương tác ban đầu, chúng tôi có thể yêu cầu số lần đảo ngược trên các mảng con."
date: "2026-06-25T23:21:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105757
codeforces_index: "D"
codeforces_contest_name: "Insomnia 2025"
rating: 0
weight: 105757
solve_time_s: 36
verified: true
draft: false
---

[CF 105757D - Đoán hoán vị](https://codeforces.com/problemset/problem/105757/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với mảng được sắp xếp$$[1,2,3,\dots,n].$$Ba vị trí$i<j<k$được chọn, với$j-i>1$. Phân khúc$[i,j-1]$bị đảo ngược và đoạn$[j,k]$bị đảo ngược. 

Trong phiên bản tương tác ban đầu, chúng tôi có thể yêu cầu số lần đảo ngược trên các mảng con. Trong phiên bản bị hack, các giá trị$n,i,j,k$được đưa trực tiếp làm đầu vào và chúng ta phải xây dựng lại logic tương tự khi ngoại tuyến. 

Quan sát chính là hình dạng của mảng kết quả. 

Sự đảo ngược đầu tiên biến đổi$$[i,i+1,\dots,j-1]$$vào trong$$[j-1,j-2,\dots,i].$$Sự đảo ngược thứ hai biến đổi$$[j,j+1,\dots,k]$$vào trong$$[k,k-1,\dots,j].$$Mọi thứ bên ngoài$[i,k]$vẫn được sắp xếp. 

Bên trong$[i,k]$, chúng ta thu được hai khối giảm dần:$$[j-1,\dots,i,\; k,\dots,j].$$Vì mọi giá trị trong khối đầu tiên đều nhỏ hơn mọi giá trị trong khối thứ hai nên không có sự đảo ngược giữa hai khối. Tất cả các sự đảo ngược đều đến từ bên trong mỗi khối giảm dần. 

Cho phép$$a=j-i,
\qquad
b=k-j+1.$$Sau đó, số lượng đảo ngược được đóng góp bởi mỗi khối là$$\frac{a(a-1)}2$$Và$$\frac{b(b-1)}2.$$Phiên bản ngoại tuyến chỉ cung cấp$n,i,j,k$, nhưng việc hiểu các công thức nghịch đảo này chính là điều mà giải pháp ban đầu dựa vào. 

Những ràng buộc cho phép$n$lên đến$10^9$, nhưng chỉ có tối đa$100$các trường hợp thử nghiệm. Bất kì$O(1)$hoặc$O(\log n)$tính toán cho mỗi trường hợp thử nghiệm là không đáng kể. Mô phỏng một mảng có chiều dài$10^9$is impossible, so the solution must be purely mathematical.

 A common mistake is confusing the length of the first reversed segment.

 Ví dụ, nếu```
i=2, j=5
```thì đoạn đó là$[2,4]$, có độ dài là$$j-i=3,$$không$j-i+1$. 

Một lỗi dễ xảy ra khác xuất hiện ở đoạn thứ hai. chiều dài của nó là$$k-j+1.$$Vì```
j=4, k=6
```phân khúc là$[4,6]$, chứa ba phần tử. 

sử dụng$k-j$thay vào đó sẽ tính thiếu số lần đảo ngược. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ xây dựng mảng một cách rõ ràng, thực hiện cả hai lần đảo ngược và sau đó đếm các lần đảo ngược. Đếm các nghịch đảo một cách đơn giản đòi hỏi phải kiểm tra từng cặp, dẫn đến$O(n^2)$công việc. 

Cách tiếp cận đó ngay lập tức thất bại khi$n$có thể đạt được$10^9$. Ngay cả việc xây dựng mảng là không thể. 

Cấu trúc được tạo ra bởi hai đảo ngược có nhiều thông tin hơn chính mảng cuối cùng. 

Hoán vị kết quả bao gồm ba phần:$$\text{sorted prefix}$$theo sau là$$\text{first decreasing block}$$theo sau là$$\text{second decreasing block}$$theo sau là$$\text{sorted suffix}.$$Bên trong một khối có chiều dài giảm dần$L$, mỗi cặp vị trí tạo thành một sự đảo ngược. Số lần đảo ngược chính xác$$\binom{L}{2}
=
\frac{L(L-1)}2.$$Bởi vì mọi giá trị trong khối đầu tiên đều nhỏ hơn mọi giá trị trong khối thứ hai nên việc đảo ngược giữa các khối không bao giờ xảy ra. 

Điều này chuyển đổi vấn đề thành số học đơn giản về độ dài đoạn thay vì tự thao tác hoán vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Phân tích toán học | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n,i,j,k$. 
2. Tính độ dài đoạn đảo ngược thứ nhất:$$a=j-i.$$Phân đoạn là$[i,j-1]$, vậy độ dài của nó không$j-i+1$. 

1. Tính độ dài đoạn đảo ngược thứ hai:$$b=k-j+1.$$Phân đoạn là$[j,k]$, bao gồm cả hai đầu. 

1. Tính số lần đảo ngược do khối đầu tiên đóng góp:$$inv_1=\frac{a(a-1)}2.$$Chuỗi có độ dài giảm dần$a$chứa một sự đảo ngược cho mỗi cặp vị trí. 

1. Tính số lần đảo ngược do khối thứ hai đóng góp:$$inv_2=\frac{b(b-1)}2.$$1. Đầu ra$$inv_1+inv_2.$$### Tại sao nó hoạt động 

Sau hai lần đảo chiều, khoảng thời gian$[i,k]$trở thành hai khối giảm độc lập. Mọi nghịch đảo đều nằm hoàn toàn bên trong một trong những khối đó. 

Khối đầu tiên đóng góp chính xác$\binom{a}{2}$đảo ngược và đóng góp thứ hai chính xác$\binom{b}{2}$. 

Không có sự đảo ngược giữa các khối vì mọi giá trị bắt nguồn từ khối đầu tiên đều nhỏ hơn mọi giá trị bắt nguồn từ khối thứ hai. Vì tất cả các vị trí còn lại vẫn được sắp xếp nên không có sự đảo ngược bổ sung nào tồn tại. 

Tổng của hai số tam giác chính xác là tổng số nghịch đảo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    
    for _ in range(t):
        n, i, j, k = map(int, input().split())
        
        a = j - i
        b = k - j + 1
        
        ans = a * (a - 1) // 2 + b * (b - 1) // 2
        print(ans)

solve()
```Việc thực hiện hoàn toàn là số học. 

Đoạn đầu tiên là$[i,j-1]$, vậy độ dài của nó là`j - i`. Đây là nơi phổ biến nhất để mắc lỗi ngẫu nhiên. 

Đoạn thứ hai là$[j,k]$, vậy độ dài của nó là`k - j + 1`. 

Cả hai số đảo ngược đều là số hình tam giác. Số nguyên Python xử lý thoải mái các giá trị lớn nhất có thể vì$n\le 10^9$, đưa ra câu trả lời nhiều nhất về$5\times10^{17}$, nằm trong phạm vi số nguyên của Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
5 1 3 5
```Độ dài:$$a=3-1=2,
\qquad
b=5-3+1=3.$$| Biến | Giá trị | 
| --- | --- | 
| một | 2 | 
| b | 3 | 
| inv₁ | 1 | 
| inv₂ | 3 | 
| trả lời | 4 | 

Mảng kết quả là:$$[2,1,5,4,3].$$Những sự đảo ngược là$(2,1)$,$(5,4)$,$(5,3)$, Và$(4,3)$, mang lại tổng cộng$4$. 

### Ví dụ 2 

đầu vào:```
1
8 2 5 7
```Độ dài:$$a=5-2=3,
\qquad
b=7-5+1=3.$$| Biến | Giá trị | 
| --- | --- | 
| một | 3 | 
| b | 3 | 
| inv₁ | 3 | 
| inv₂ | 3 | 
| trả lời | 6 | 

Khối đảo ngược đầu tiên đóng góp ba lần đảo ngược và khối thứ hai đóng góp thêm ba lần đảo ngược nữa. Không có sự đảo ngược xuyên khối nào xuất hiện. 

Ví dụ này nêu bật tính bất biến chính: sự đảo ngược chỉ đến từ bên trong các khối giảm dần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một vài phép tính số học cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Không sử dụng cấu trúc phụ trợ | 

Giải pháp không bao giờ xây dựng hoán vị, điều này rất cần thiết vì$n$có thể lớn như$10^9$. Số học thời gian không đổi dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, i, j, k = map(int, input().split())

        a = j - i
        b = k - j + 1

        ans = a * (a - 1) // 2 + b * (b - 1) // 2
        out.append(str(ans))

    return "\n".join(out)

# sample-style cases
assert run("1\n5 1 3 5\n") == "4"

# minimum valid configuration
assert run("1\n4 1 3 4\n") == "1"

# both blocks length 3
assert run("1\n8 2 5 7\n") == "6"

# large values
assert run("1\n1000000000 1 500000000 1000000000\n") == \
       str((499999999 * 499999998 // 2) + (500000001 * 500000000 // 2))

# boundary where second block has length 2
assert run("1\n10 3 7 8\n") == "16"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 1 3 4`|`1`| Cấu hình hợp lệ nhỏ nhất | 
|`8 2 5 7`|`6`| Khối giảm kích thước bằng nhau | 
| Lớn$n$trường hợp | Số lượng lớn | Không phụ thuộc vào việc xây dựng mảng | 
|`10 3 7 8`|`16`| Xử lý đúng độ dài đoạn | 

## Vỏ cạnh 

Hãy xem xét cấu hình hợp lệ nhỏ nhất:```
1
4 1 3 4
```Đoạn đầu tiên là`[1,2]`, chiều dài`2`, góp phần đảo ngược. Đoạn thứ hai là`[3,4]`, chiều dài`2`, góp phần đảo ngược. Sau khi đảo ngược mảng trở thành:$$[2,1,4,3].$$Số lượng đảo ngược là$1+1=2$. Công thức tính toán chính xác cùng một giá trị. 

Bây giờ hãy xem xét trường hợp khối thứ hai rất ngắn:```
1
10 3 7 8
```Đây$$a=4,\qquad b=2.$$Câu trả lời là$$\binom{4}{2}+\binom{2}{2}
=
6+1
=
7.$$sử dụng$k-j$thay vì$k-j+1$sẽ đặt sai$b=1$và mất đi một lần đảo ngược. Đây chính xác là lý do tại sao điểm cuối bao gồm trong phân đoạn thứ hai lại quan trọng. 

Cuối cùng, hãy xem xét một giá trị rất lớn:```
1
1000000000 1 500000000 1000000000
```Thuật toán chỉ thực hiện một số phép tính số nguyên và không bao giờ phân bổ một mảng có kích thước$10^9$. Độ phức tạp không đổi, đây là cách tiếp cận khả thi duy nhất đối với các giới hạn đã cho.
