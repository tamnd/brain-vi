---
title: "CF 1612D - Cặp X-Magic"
description: "Chúng ta được cho một cặp số nguyên $a$ và $b$, và một số nguyên $x$ khác. Chúng ta được phép thay thế nhiều lần $a$ hoặc $b$ bằng chênh lệch tuyệt đối $ Các ràng buộc cho phép $a, b, x$ tăng lên $10^{18}$ và có thể có tới $10^4$ trường hợp thử nghiệm."
date: "2026-06-10T06:58:23+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1612
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 117 (Rated for Div. 2)"
rating: 1600
weight: 1612
solve_time_s: 67
verified: true
draft: false
---

[CF 1612D - Cặp X-Magic](https://codeforces.com/problemset/problem/1612/D) 

**Đánh giá:** 1600 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cặp số nguyên$a$Và$b$, và một số nguyên khác$x$. Chúng tôi được phép thay thế nhiều lần$a$hoặc$b$với sự khác biệt tuyệt đối$|a - b|$. Nhiệm vụ là xác định xem sau một số thao tác này, liệu có thể thực hiện được một trong hai$a$hoặc$b$bằng nhau$x$. Chúng tôi có nhiều trường hợp thử nghiệm độc lập, mỗi trường hợp có trường hợp riêng$a, b, x$. 

Những ràng buộc cho phép$a, b, x$đi lên$10^{18}$, và có thể có tới$10^4$trường hợp thử nghiệm. Điều này làm cho bất kỳ cách tiếp cận nào mô phỏng tất cả các hoạt động một cách rõ ràng đều không khả thi vì số bước có thể cực kỳ lớn. Chúng ta cần một cách tiếp cận có thể làm việc trực tiếp với các thuộc tính số của$a$Và$b$, thay vì tạo ra tất cả các chuỗi hoạt động có thể có. 

Một trường hợp quan trọng không rõ ràng phát sinh khi$a = b$. Đây,$|a-b| = 0$, vì vậy mọi thao tác tiếp theo sẽ không bao giờ tạo ra một số khác ngoài cặp hiện tại và số 0. Ví dụ, nếu$a = b = 30$Và$x = 30$, câu trả lời là CÓ, nhưng nếu$x = 15$, nó sẽ là KHÔNG, vì cặp này chỉ có thể đạt tới 0 chứ không phải 15. 

Một trường hợp tế nhị khác là khi$x$lớn hơn cả hai$a$hoặc$b$. Ví dụ,$a = 40, b = 50, x = 90$. Các thao tác chỉ có thể giảm số lượng, vì vậy nếu$x > \max(a, b)$, câu trả lời ngay là KHÔNG. Việc nhận ra sớm những hạn chế này sẽ ngăn chặn việc tính toán không cần thiết. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản: áp dụng liên tục cả hai thao tác theo mọi cách có thể, theo dõi mọi cặp mới được tạo cho đến khi một trong hai$x$được tìm thấy hoặc trình tự ổn định. Điều này đúng vì bài toán cho phép trình tự tùy ý của hai phép toán. Vấn đề với cách tiếp cận này là đối với số lượng lớn$a, b$số lượng các cặp riêng biệt có thể được tạo ra là rất lớn, có khả năng lên tới$10^{18}$, điều đó hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng cho một giải pháp tối ưu xuất phát từ việc quan sát rằng các phép toán bắt chước thuật toán Euclide để tính ước số chung lớn nhất. Mỗi lần chúng ta thay thế một số bằng$|a-b|$, chúng ta đang hướng tới một cặp nhỏ hơn có hiệu là tổ hợp tuyến tính của cặp ban đầu$a$Và$b$. Cụ thể, bất kỳ số nào có thể truy cập được thông qua các hoạt động này đều phải đáp ứng$x \le \max(a, b)$Và$x \equiv a \pmod{\gcd(a, b)}$. Điều này là do thuật toán Euclide đảm bảo rằng chuỗi số được tạo ra bằng cách áp dụng lặp đi lặp lại$|a-b|$cuối cùng sẽ tạo ra tất cả bội số của$\gcd(a, b)$nhỏ hơn hoặc bằng$\max(a, b)$. 

Vì vậy, thay vì mô phỏng các thao tác, chúng ta có thể kiểm tra trực tiếp hai điều kiện:$x \le \max(a, b)$Và$x \% \gcd(a, b) == a \% \gcd(a, b)$. Nếu cả hai đều giữ, cặp đó là$x$-ảo thuật. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(lớn, có khả năng theo cấp số nhân) | O(lớn) | Quá chậm | 
| Tối ưu | O(log(max(a,b))) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test, đọc số nguyên$a, b, x$. 
3. Tính toán$g = \gcd(a, b)$. Điều này ghi lại kích thước bước của tất cả các số có thể truy cập được thông qua các thao tác. Mọi số có thể truy cập đều phải phù hợp với$a$modulo$g$. 
4. Kiểm tra xem$x \le \max(a, b)$. Nếu không, các thao tác không thể đạt số lớn hơn, vì vậy hãy in NO và tiếp tục. 
5. Kiểm tra xem$x \equiv a \pmod{g}$. Nếu đúng,$x$có thể truy cập được; nếu không thì không. In CÓ hoặc KHÔNG tương ứng. 

Tại sao nó hoạt động: Các phép toán giống như Euclide bảo toàn ước số chung lớn nhất của cặp và giá trị tối đa không bao giờ tăng. Vì vậy, bất kỳ số nào$x$có thể truy cập được phải nhỏ hơn hoặc bằng mức tối đa hiện tại và phải phù hợp với các số ban đầu theo mô đun GCD. Hai bước kiểm tra đơn giản này nắm bắt hoàn toàn tập hợp các số có thể truy cập. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b, x = map(int, input().split())
    g = math.gcd(a, b)
    if x <= max(a, b) and (x % g == a % g):
        print("YES")
    else:
        print("NO")
```Mã đầu tiên đọc tất cả các trường hợp thử nghiệm. Tính toán chính là`math.gcd(a, b)`, cho phép chúng tôi kiểm tra trực tiếp tình trạng mô-đun thay vì mô phỏng các hoạt động. Kiểm tra`x <= max(a, b)`đảm bảo chúng tôi không bao giờ cố gắng đạt được số lớn hơn cặp hiện tại và`x % g == a % g`đảm bảo khả năng tiếp cận thông qua mô hình Euclide. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**$a=6, b=9, x=3$| một | b | g | tối đa(a,b) | x <= tối đa | x % g | một % g | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 6 | 9 | 3 | 9 | Đúng | 0 | 0 | CÓ | 

GCD là 3. 3 ≤ 9 và 3 ≡ 6 mod 3, vì vậy CÓ. 

**Ví dụ 2:**$a=40, b=50, x=90$| một | b | g | tối đa(a,b) | x <= tối đa | x % g | một % g | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 40 | 50 | 10 | 50 | Sai | 0 | 0 | KHÔNG | 

x > max(a,b), nên cặp không thể đạt đến x. 

Những ví dụ này chứng minh rằng thuật toán xác định chính xác các số có thể truy cập theo hai điều kiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t log(max(a,b))) | Mỗi phép tính GCD mất O(log(max(a,b))), lặp lại cho t trường hợp kiểm thử. | 
| Không gian | O(1) | Chỉ một vài biến được lưu trữ cho mỗi trường hợp thử nghiệm. | 

Được cho$t \le 10^4$Và$a, b \le 10^{18}$, cách tiếp cận này phù hợp trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io
from math import gcd

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        a, b, x = map(int, input().split())
        g = gcd(a, b)
        if x <= max(a, b) and (x % g == a % g):
            print("YES")
        else:
            print("NO")
    return output.getvalue().strip()

# provided samples
assert run("8\n6 9 3\n15 38 7\n18 8 8\n30 30 30\n40 50 90\n24 28 20\n365 216 52\n537037812705867558 338887693834423551 3199921013340") == "YES\nYES\nYES\nYES\nNO\nYES\nYES\nYES"

# custom cases
assert run("3\n1 1 1\n1 1 2\n1000000000000000000 1000000000000000000 0") == "YES\nNO\nYES", "edge cases"
assert run("2\n10 6 4\n10 6 5") == "YES\nNO", "mod gcd test"
assert run("1\n7 14 7") == "YES", "simple gcd multiple"
assert run("1\n7 14 8") == "NO", "simple gcd failure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1; 1 1 2; 10^18 10^18 0 | ĐÚNG; KHÔNG; CÓ | kích thước tối thiểu, x không thể truy cập, cạnh tối đa | 
| 10 6 4; 10 6 5 | ĐÚNG; KHÔNG | có thể truy cập bằng mẫu gcd, không kiểm tra modulo | 
| 7 14 7 | CÓ | trường hợp chia hết gcd đơn giản | 
| 7 14 8 | KHÔNG | trường hợp gcd đơn giản không chia hết | 

## Vỏ cạnh 

Nếu$a = b = x$, thuật toán trả về đúng CÓ vì`x <= max(a,b)`Và`x % g == a % g`nắm giữ một cách tầm thường. Vì$x > \max(a, b)$, nó trả về NO ngay lập tức một cách chính xác. Khi các con số cực kỳ lớn, các phép tính GCD sẽ xử lý chúng một cách hiệu quả và số học mô-đun đảm bảo tính chính xác mà không bị tràn.
