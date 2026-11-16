---
title: "CF 1877C - Cần điều khiển"
description: "Chúng ta chọn giá trị $a{n+1}$ trong khoảng từ $0$ đến $m$. Sau đó, mọi vị trí trước đó được xác định duy nhất bởi $$ai = a{i+1} bmod i$$ cho $i=n,n-1,dots,1$. Toàn bộ mảng được tạo từ một giá trị bắt đầu duy nhất."
date: "2026-06-09T01:04:05+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1877
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 902 (Div. 2, based on COMPFEST 15 - Final Round)"
rating: 1200
weight: 1877
solve_time_s: 121
verified: true
draft: false
---

[CF 1877C - Joyboard](https://codeforces.com/problemset/problem/1877/C) 

**Đánh giá:** 1200 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi chọn một giá trị$a_{n+1}$giữa$0$Và$m$. Sau đó, mọi vị trí trước đó được xác định duy nhất bởi$$a_i = a_{i+1} \bmod i$$vì$i=n,n-1,\dots,1$. 

Toàn bộ mảng được tạo từ một giá trị bắt đầu duy nhất. Chúng ta phải đếm xem có bao nhiêu sự lựa chọn$a_{n+1}$sản xuất chính xác$k$những con số khác biệt giữa tất cả$n+1$các vị trí. 

Các ràng buộc ngay lập tức gợi ý rằng chúng ta không thể mô phỏng quá trình này. Cả hai$n$Và$m$có thể đạt được$10^9$, và có tới$2\cdot10^4$trường hợp thử nghiệm. Bất kỳ giải pháp nào lặp qua các giá trị của$a_{n+1}$, hoặc thậm chí thông qua tất cả các vị trí$1\ldots n$, là không thể. 

Thách thức chính là hiểu được cấu trúc của chuỗi được tạo ra bởi các phép toán modulo lặp đi lặp lại. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Coi như$n=2,m=0,k=1$. Sự lựa chọn duy nhất có thể là$a_3=0$. Mảng được tạo ra là$[0,0,0]$, vậy câu trả lời là$1$. Bất kỳ công thức nào giả định$m\ge1$sẽ thất bại ở đây. 

Coi như$n=4,m=6,k=3$. Các lựa chọn hợp lệ là$5$Và$6$, đưa ra câu trả lời$2$. Một phỏng đoán ngây thơ rằng số lượng các giá trị riêng biệt chỉ phụ thuộc vào$a_{n+1}$lớn hay nhỏ là không đủ. 

Coi như$n=265,m=265,k=265$. Câu trả lời là$0$. Vì chỉ có$266$vị trí, đạt được$265$các giá trị riêng biệt đã rất hạn chế. Hiểu chính xác giá trị nào của$k$có thể đạt được là cốt lõi của vấn đề. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi giá trị có thể$x=a_{n+1}$từ$0$ĐẾN$m$. Đối với mỗi người, chúng tôi sẽ tạo ra$$a_n=x\bmod n,\quad a_{n-1}=a_n\bmod(n-1),\dots$$và đếm các giá trị khác nhau. 

Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Thật không may, nó đòi hỏi$O(mn)$công việc. Với cả hai tham số đạt$10^9$, điều này hoàn toàn không thể thực hiện được. 

Bước đột phá đến từ việc kiểm tra xem các phép toán modulo lặp đi lặp lại thực sự làm gì. 

Cho phép$x=a_{n+1}$. 

Nếu như$x<n$, sau đó$$a_n=x\bmod n=x.$$Từ$x<n$, việc áp dụng modulo với số lượng nhỏ hơn sẽ giữ nguyên giá trị hoặc giảm giá trị đó. Mọi giá trị xuất hiện trong chuỗi nhiều nhất là$x$. 

Nếu như$x\ge n$, sau đó$$a_n=x\bmod n.$$Toàn bộ phần còn lại của chuỗi chỉ phụ thuộc vào$r=x\bmod n$. 

Quan trọng hơn, sau vị trí$n$, mọi giá trị lớn nhất là$n-1$. 

Hãy để chúng tôi hiểu có bao nhiêu giá trị khác biệt xuất hiện. 

Nếu như$x=0$, mọi phần tử đều trở thành$0$. Chúng tôi nhận được chính xác một giá trị riêng biệt. 

Nếu như$1\le x<n$, thì chuỗi bắt đầu từ giá trị$x$và bất cứ khi nào chúng tôi đạt được mô đun$x$,$$x\bmod x = 0.$$Trước thời điểm đó giá trị vẫn còn$x$. Sau thời điểm đó nó vẫn còn$0$. 

Vì vậy, mảng chứa chính xác hai giá trị riêng biệt:$x$Và$0$. 

Nếu như$x\ge n$, viết$$r=x\bmod n.$$giá trị$x$chính nó xuất hiện ở vị trí$n+1$, trong khi tất cả các vị trí trước đó chỉ phụ thuộc vào$r$. 

Nếu như$r=0$, sau đó vị trí$1\ldots n$là tất cả$0$. Các giá trị riêng biệt là$\{0,x\}$, cho chính xác hai giá trị phân biệt. 

Nếu như$1\le r<n$, sau đó vị trí$1\ldots n$chỉ chứa$r$Và$0$, trong khi vị trí$n+1$chứa$x$, lớn hơn$n$và khác với cả hai. Do đó, chúng tôi thu được chính xác ba giá trị riêng biệt:$$\{x,r,0\}.$$Điều này hoàn toàn mô tả vấn đề. 

Các số duy nhất có thể có giá trị riêng biệt là: 

cho$k=1$, chỉ một$x=0$. 

Vì$k=2$, hoặc$x\in[1,n-1]$hoặc$x$là bội số dương của$n$. 

Vì$k=3$,$x\ge n$Và$x\bmod n\neq0$. 

Vì$k\ge4$, câu trả lời luôn là$0$. 

Một khi đã biết cấu trúc này, việc đếm trở thành số học đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(mn)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi trường hợp thử nghiệm |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu$k=1$, sự lựa chọn hợp lệ duy nhất là$x=0$. Trở lại$1$bởi vì$0$luôn được cho phép. 
2. Nếu$k\ge4$, trở lại$0$. Chuỗi không bao giờ có thể chứa nhiều hơn ba giá trị riêng biệt. 
3. Nếu$k=2$, đếm hai nhóm giá trị. 

Đầu tiên, tất cả$x$như vậy$1\le x<n$. Mỗi người trong số họ tạo ra chính xác các giá trị$\{x,0\}$. 

Thứ hai, tất cả các bội số dương của$n$không vượt quá$m$. Những sản phẩm này$\{x,0\}$cũng vậy. 

Số lượng là$$\min(m,n-1)+\left\lfloor\frac{m}{n}\right\rfloor.$$4. Nếu$k=3$, đếm tất cả các giá trị$x\ge n$với modulo dư khác 0$n$. 

có$$m-n+1$$số nguyên trong khoảng$[n,m]$khi$m\ge n$. 

Trong số đó,$$\left\lfloor\frac{m}{n}\right\rfloor$$là bội số của$n$. 

Loại bỏ những thứ đó mang lại$$(m-n+1)-\left\lfloor\frac{m}{n}\right\rfloor.$$Nếu như$m<n$, câu trả lời là$0$. 

### Tại sao nó hoạt động 

Bất biến quan trọng là sau khi tính toán$a_n=x\bmod n$, mọi giá trị tiếp theo chỉ phụ thuộc vào số dư đó và không bao giờ vượt quá$n-1$. 

Khi$x<n$, giá trị$x$tồn tại không thay đổi cho đến khi mô đun$x$được áp dụng, tại thời điểm đó nó trở thành$0$. Không có giá trị thứ ba có thể xuất hiện. 

Khi$x\ge n$, giá trị$x$chính nó chỉ xuất hiện ở vị trí$n+1$. Các vị trí còn lại hoạt động giống hệt như chuỗi được tạo từ$r=x\bmod n$, chỉ có thể chứa$r$Và$0$. Do đó toàn bộ mảng chứa hai giá trị riêng biệt khi$r=0$hoặc ba giá trị riêng biệt khi$r\neq0$. 

Vì mọi điều có thể$x$rơi vào một trong các trường hợp này thì công thức tính là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, m, k = map(int, input().split())

        if k == 1:
            ans.append("1")

        elif k == 2:
            res = min(m, n - 1) + m // n
            ans.append(str(res))

        elif k == 3:
            if m < n:
                ans.append("0")
            else:
                res = (m - n + 1) - (m // n)
                ans.append(str(res))

        else:
            ans.append("0")

    sys.stdout.write("\n".join(ans))

solve()
```Nhánh đầu tiên xử lý giá trị duy nhất$x=0$, đó là cách duy nhất để có được một số riêng biệt. 

Nhánh thứ hai đếm hai lớp tạo ra chính xác hai giá trị riêng biệt. sử dụng`min(m, n - 1)`xử lý chính xác trường hợp khoảng$[1,n-1]$vượt quá phạm vi cho phép$[0,m]$. 

Nhánh thứ ba đếm số$[n,m]$và trừ bội số của$n$. Séc`m < n`tránh tạo ra kết quả âm tính khi khoảng trống. 

Tất cả số học phù hợp thoải mái với số nguyên 64 bit. Số nguyên Python không bị giới hạn nên không cần chăm sóc đặc biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 6 3
```| Biến | Giá trị | 
| --- | --- | 
| n | 4 | 
| m | 6 | 
| k | 3 | 

Từ$k=3$, chúng ta đếm các số trong$[4,6]$đó không phải là bội số của$4$. 

| x | bội số của 4? | Đóng góp? | 
| --- | --- | --- | 
| 4 | Có | Không | 
| 5 | Không | Có | 
| 6 | Không | Có | 

Công thức: 

| Biểu hiện | Giá trị | 
| --- | --- | 
|$m-n+1$| 3 | 
|$\lfloor m/n\rfloor$| 1 | 
| Trả lời |$3-1=2$| 

Các giá trị hợp lệ là$5$Và$6$, phù hợp với mẫu 

### Ví dụ 2 

đầu vào:```
2 0 1
```| Biến | Giá trị | 
| --- | --- | 
| n | 2 | 
| m | 0 | 
| k | 1 | 

Vì$k=1$, chỉ một$x=0$hoạt động. 

| Có thể x | Giá trị riêng biệt | 
| --- | --- | 
| 0 | {0} | 

Trả lời: 

| Kết quả | Giá trị | 
| --- | --- | 
| Đếm | 1 | 

Ví dụ này thể hiện phạm vi không tầm thường nhỏ nhất và xác nhận rằng việc xử lý đặc biệt của$k=1$là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$mỗi trường hợp thử nghiệm | Chỉ một số phép tính số học được thực hiện | 
| Không gian |$O(1)$| Không có cấu trúc phụ trợ nào được lưu trữ | 

Với nhiều nhất$2\cdot10^4$các trường hợp thử nghiệm, giải pháp chỉ thực hiện một lượng công việc không đổi cho từng trường hợp, dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    input_data = io.StringIO(inp)
    output_data = io.StringIO()

    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = input_data
    sys.stdout = output_data

    def solve():
        input = sys.stdin.readline
        t = int(input())
        ans = []

        for _ in range(t):
            n, m, k = map(int, input().split())

            if k == 1:
                ans.append("1")
            elif k == 2:
                ans.append(str(min(m, n - 1) + m // n))
            elif k == 3:
                if m < n:
                    ans.append("0")
                else:
                    ans.append(str((m - n + 1) - (m // n)))
            else:
                ans.append("0")

        print("\n".join(ans))

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return output_data.getvalue()

# provided sample
assert run(
"""4
4 6 3
2 0 1
265 265 265
3 10 2
"""
) == "2\n1\n0\n5\n"

# minimum size
assert run(
"""1
1 0 1
"""
) == "1\n"

# k = 2 boundary
assert run(
"""1
4 4 2
"""
) == "4\n"

# k = 3 with m < n
assert run(
"""1
10 5 3
"""
) == "0\n"

# large values
assert run(
"""1
1000000000 1000000000 4
"""
) == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 1`|`1`| Ví dụ nhỏ nhất có thể | 
|`4 4 2`|`4`| Ranh giới giữa các giá trị bên dưới`n`và bội số của`n`| 
|`10 5 3`|`0`| Khoảng trống cho`k=3`trường hợp | 
|`1000000000 1000000000 4`|`0`| Không có giải pháp nào cho`k≥4`| 

## Vỏ cạnh 

### Trường hợp 1: Chỉ có 0 

đầu vào:```
1
2 0 1
```Thuật toán đi vào`k == 1`chi nhánh và ngay lập tức trở lại`1`. 

Giá trị duy nhất có thể là$x=0$, sản xuất$[0,0,0]$. Chính xác một giá trị riêng biệt tồn tại. 

### Trường hợp 2: Yêu cầu 4 giá trị phân biệt 

đầu vào:```
1
5 100 4
```Thuật toán trả về`0`. 

Đối với bất kỳ người được chọn$x$, dãy chứa nhiều nhất các giá trị$x$,$x\bmod n$, Và$0$. Ba giá trị riêng biệt là giá trị tối đa có thể. Bốn không bao giờ có thể xảy ra. 

### Trường hợp 3:$k=3$Nhưng$m<n$đầu vào:```
1
10 7 3
```Thuật toán kiểm tra`m < n`và trả về`0`. 

Mỗi hợp lệ$x$thỏa mãn$x<10$. Những giá trị như vậy chỉ tạo ra$\{x,0\}$hoặc chỉ$\{0\}$, không bao giờ có ba giá trị phân biệt. 

### Trường hợp 4: bội số chính xác của$n$đầu vào:```
1
4 8 2
```Các giá trị có thể có là$1,2,3,4,8$. 

Vì$x=4$Và$x=8$,$$x\bmod4=0,$$nên dãy chỉ chứa$x$Và$0$. 

Công thức cho$$\min(8,3)+\left\lfloor\frac{8}{4}\right\rfloor =3+2=5,$$phù hợp với năm lựa chọn hợp lệ.
