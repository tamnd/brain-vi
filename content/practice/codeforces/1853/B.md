---
title: "CF 1853B - Fibonaccharsis"
description: "Cho ta hai số nguyên n và k. Chúng ta muốn đếm xem có bao nhiêu chuỗi giống Fibonacci có độ dài k kết thúc bằng giá trị n."
date: "2026-06-09T05:16:55+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 1853
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 887 (Div. 2)"
rating: 1200
weight: 1853
solve_time_s: 116
verified: true
draft: false
---

[CF 1853B - Fibonaccharsis](https://codeforces.com/problemset/problem/1853/B) 

**Đánh giá:** 1200 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, toán học 
**Thời gian giải:** 1 phút 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho ta hai số nguyên`n`Và`k`. Chúng tôi muốn đếm xem có bao nhiêu chuỗi có độ dài giống Fibonacci`k`kết thúc bằng giá trị`n`. 

Chuỗi giống Fibonacci được xây dựng giống hệt như chuỗi Fibonacci, ngoại trừ hai giá trị đầu tiên là các số nguyên không âm tùy ý và chuỗi phải không giảm. Đối với mọi vị trí`i > 2`,$$f_i = f_{i-1} + f_{i-2}.$$Phần tử cuối cùng được cố định:$$f_k = n.$$Nhiệm vụ của chúng ta là đếm xem có bao nhiêu lựa chọn trong hai phần tử đầu tiên tạo ra một chuỗi hợp lệ thỏa mãn mọi yêu cầu. 

Những ràng buộc là manh mối đầu tiên. Số lượng ca kiểm thử có thể lớn tới mức`2 · 10^5`, và tổng của tất cả`n`giá trị cũng nhiều nhất`2 · 10^5`. Một giải pháp thực hiện công việc tỷ lệ thuận với`n`mỗi trường hợp thử nghiệm đều có thể chấp nhận được, nhưng bất cứ điều gì bậc hai trong`n`ngay lập tức là không thể. 

Hạn chế bất thường đó là`k`có thể lớn như`10^9`. Bất kỳ thuật toán nào xây dựng rõ ràng một chuỗi có độ dài`k`là vô vọng. Lời giải phải khai thác thực tế là các số Fibonacci tăng cực kỳ nhanh. 

Một vài trường hợp đáng chú ý. 

Khi`k`rất lớn và`n`nhỏ thì có thể không có giải pháp nào cả. Ví dụ:```
n = 3, k = 9
```Các hệ số liên quan`f_k`to the first two elements become large long before position 9, making it impossible to represent 3 using non-negative starting values.

 Khi`k = 3`, sự tái phát mang lại$$f_3 = f_1 + f_2.$$Mỗi cặp`(f1, f2)`với tổng`n`hoạt động, cung cấp`f1 ≤ f2`. Vì`n = 1`, chỉ một`(0,1)`là hợp lệ, vì vậy câu trả lời là`1`. 

Một trường hợp tế nhị khác là thực thi điều kiện không giảm. Một cặp có thể thỏa mãn sự truy hồi và tạo ra`f_k = n`, nhưng lại thất bại vì`f_1 > f_2`. Ví dụ, nếu một giải pháp dẫn xuất cho`(3,1)`, nó phải bị từ chối mặc dù tất cả các giá trị sau đó đã được xác định. 

Quan sát quan trọng là một lần`f_1`Và`f_2`không âm và thỏa mãn`f_1 ≤ f_2`, mọi số hạng sau đó sẽ tự động không giảm. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là thử mọi cặp có thể`(f_1, f_2)`. 

Vì tất cả các số hạng đều không âm và giá trị cuối cùng là`n`, cả hai giá trị bắt đầu nhiều nhất là`n`. Chúng ta có thể liệt kê tất cả các cặp và tạo ra chuỗi cho đến vị trí`k`, kiểm tra xem giá trị cuối cùng có bằng không`n`. 

Điều này đúng nhưng quá chậm. có`O(n²)`các cặp ứng cử viên và mỗi cặp có thể yêu cầu nhiều bước lặp lại. Với`n`lên đến`2 · 10^5`, phương án này hoàn toàn không khả thi. 

Quan sát tiếp theo là mọi dãy giống Fibonacci đều tuyến tính ở hai giá trị đầu tiên của nó. 

Cho phép$$f_1=a,\qquad f_2=b.$$Sau đó:$$f_3=a+b,$$

$$f_4=a+2b,$$

$$f_5=2a+3b,$$và vân vân. 

Bản thân các hệ số đều tuân theo số Fibonacci. Nói chung,$$f_k = F_{k-2}a + F_{k-1}b,$$Ở đâu$F_1=1, F_2=1$. 

Toàn bộ bài toán trở thành việc đếm các nghiệm nguyên không âm của$$F_{k-2}a + F_{k-1}b = n,$$tùy thuộc vào$$a \le b.$$Đây bây giờ là một phương trình Diophantine tuyến tính. 

Một quan sát quan trọng khác đến từ sự hạn chế về`n`. Số Fibonacci vượt quá`200000`chỉ sau vài chục học kỳ. Từ`n ≤ 200000`, khi một trong hai hệ số trở nên lớn hơn`n`, có thể có nhiều nhất một khả năng tầm thường và thường là không có. 

Trên thực tế, chỉ có khoảng 30 số Fibonacci đầu tiên là quan trọng. Đối với lớn hơn`k`, câu trả lời tự động là 0. 

Đối với mỗi trường hợp thử nghiệm, chúng ta có thể tính hai hệ số Fibonacci có liên quan và đếm tất cả các cặp số nguyên`(a,b)`thỏa mãn phương trình và`a ≤ b`. 

Từ`n`nhỏ, lặp đi lặp lại tất cả những gì có thể`b`giá trị đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²·k) | O(1) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các số Fibonacci cho đến khi chúng vượt quá`200000`. 

Vì tất cả`n`giá trị nhiều nhất là`200000`, các số Fibonacci lớn hơn không bao giờ có thể tham gia vào một biểu diễn hợp lệ. 
2. Đối với trường hợp thử nghiệm`(n, k)`, xác định các hệ số. 

sử dụng$$f_k = F_{k-2}a + F_{k-1}b,$$chúng ta cần hệ số`A = F_{k-2}`Và`B = F_{k-1}`. 
3. Nếu`k-1`vượt quá phạm vi số Fibonacci được tính toán trước, ngay lập tức quay trở lại`0`. 

Tại thời điểm đó`B > 200000 > n`, làm cho bất kỳ biểu diễn không tầm thường nào là không thể. 
4. Liệt kê tất cả các giá trị có thể có của`b`. 

Từ$$A a + B b = n,$$chúng ta chỉ cần xem xét$$0 \le b \le \left\lfloor \frac n B \right\rfloor .$$5. Đối với mỗi ứng viên`b`, tính giá trị còn lại:$$rem = n - B b.$$Nếu như`rem`không chia hết cho`A`, không có số nguyên`a`tồn tại. 
6. Ngược lại tính toán$$a = \frac{rem}{A}.$$Đếm nghiệm khi và chỉ khi`a ≤ b`. 
7. Xuất ra tổng số. 

### Tại sao nó hoạt động 

Mỗi dãy giống Fibonacci được xác định duy nhất bởi hai phần tử đầu tiên của nó. Mở rộng phép truy hồi cho thấy rằng`k`-số hạng thứ chính xác$$F_{k-2}a + F_{k-1}b.$$Do đó, mọi dãy hợp lệ đều tương ứng với một nghiệm số nguyên`(a,b)`của phương trình đó. 

Ngược lại, mọi nghiệm số nguyên không âm với`a ≤ b`tạo ra một chuỗi giống Fibonacci không giảm hợp lệ. Sự tái diễn bảo toàn tính không tiêu cực, và bởi vì$$f_3=a+b \ge b,$$tất cả các số hạng tiếp theo ít nhất cũng lớn bằng số hạng trước đó. 

Thuật toán đếm chính xác những cặp đó và không có gì khác, vì vậy nó chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# Fibonacci numbers with F1 = 1, F2 = 1
fib = [0, 1, 1]
while fib[-1] <= 200000:
    fib.append(fib[-1] + fib[-2])

t = int(input())

for _ in range(t):
    n, k = map(int, input().split())

    if k - 1 >= len(fib):
        print(0)
        continue

    A = fib[k - 2]
    B = fib[k - 1]

    ans = 0

    for b in range(n // B + 1):
        rem = n - B * b

        if rem % A != 0:
            continue

        a = rem // A

        if a <= b:
            ans += 1

    print(ans)
```Phần đầu tiên chỉ tính toán trước các số Fibonacci cho đến khi chúng vượt quá giá trị lớn nhất có thể`n`. Điều này là đủ vì các hệ số lớn hơn không bao giờ có thể đóng góp vào một biểu diễn hợp lệ. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi thu được các hệ số`A = F_{k-2}`Và`B = F_{k-1}`. Nếu như`k`lớn đến mức các hệ số này nằm ngoài phạm vi được tính toán trước, câu trả lời ngay lập tức là 0. 

Vòng lặp kết thúc`b`kiểm tra mọi giá trị không âm có thể xuất hiện trong lời giải. Một lần`b`là cố định, phương trình xác định`a`độc đáo. Tính chia hết kiểm tra xem giá trị đó có phải là số nguyên hay không. 

điều kiện`a <= b`chính xác là yêu cầu không giảm. Không cần phải xác thực trình tự bổ sung vì sự lặp lại sẽ tự động tạo ra các số hạng tăng dần sau đó. 

Số nguyên Python có độ chính xác tùy ý nên việc tràn không bao giờ là vấn đề. Những giá trị số học lớn nhất vẫn chỉ quanh quẩn`200000`Dẫu sao thì. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
22 4
```Vì`k = 4`:$$f_4 = F_2 a + F_3 b = a + 2b.$$Vì vậy, chúng tôi giải quyết:$$a + 2b = 22.$$| b | rem = 22 - 2b | một | a ≤ b? | Đếm | 
| --- | --- | --- | --- | --- | 
| 0 | 22 | 22 | Không | 0 | 
| 1 | 20 | 20 | Không | 0 | 
| 2 | 18 | 18 | Không | 0 | 
| 3 | 16 | 16 | Không | 0 | 
| 4 | 14 | 14 | Không | 0 | 
| 5 | 12 | 12 | Không | 0 | 
| 6 | 10 | 10 | Không | 0 | 
| 7 | 8 | 8 | Không | 0 | 
| 8 | 6 | 6 | Có | 1 | 
| 9 | 4 | 4 | Có | 2 | 
| 10 | 2 | 2 | Có | 3 | 
| 11 | 0 | 0 | Có | 4 | 

Trả lời:`4`. 

Chúng tương ứng chính xác với bốn trình tự được hiển thị trong tuyên bố. 

### Ví dụ 2 

đầu vào:```
55 11
```Đây:$$F_9 = 34,\qquad F_{10}=55.$$Chúng tôi giải quyết:$$34a + 55b = 55.$$| b | rem | chia hết cho 34? | một | có hiệu lực? | 
| --- | --- | --- | --- | --- | 
| 0 | 55 | Không | - | Không | 
| 1 | 0 | Có | 0 | Có | 

Chỉ có một giải pháp tồn tại:$$(a,b)=(0,1).$$Trả lời:`1`. 

Điều này tạo ra chuỗi Fibonacci cổ điển kết thúc ở mức 55. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Lặp lại nhiều nhất`n / F_{k-1}`giá trị của`b`, giới hạn bởi`n`| 
| Không gian | O(1) | Chỉ một vài biến cộng với một bảng Fibonacci nhỏ | 

Tổng của tất cả`n`giá trị trên toàn bộ đầu vào nhiều nhất là`200000`. Vì mỗi ca kiểm thử thực hiện công việc tương ứng với công việc của chính nó`n`, tổng công việc trên tất cả các trường hợp thử nghiệm cũng là`O(200000)`, dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    fib = [0, 1, 1]
    while fib[-1] <= 200000:
        fib.append(fib[-1] + fib[-2])

    t = int(input())
    ans = []

    for _ in range(t):
        n, k = map(int, input().split())

        if k - 1 >= len(fib):
            ans.append("0")
            continue

        A = fib[k - 2]
        B = fib[k - 1]

        cur = 0
        for b in range(n // B + 1):
            rem = n - B * b
            if rem % A == 0 and rem // A <= b:
                cur += 1

        ans.append(str(cur))

    sys.stdout.write("\n".join(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue().strip()

# provided sample
assert run(
"""8
22 4
3 9
55 11
42069 6
69420 4
69 1434
1 3
1 4
"""
) == "\n".join([
    "4",
    "0",
    "1",
    "1052",
    "11571",
    "0",
    "1",
    "0"
]), "sample 1"

# minimum values
assert run(
"""1
1 3
"""
) == "1"

# no valid representation
assert run(
"""1
1 4
"""
) == "0"

# large k immediately impossible
assert run(
"""1
69 1434
"""
) == "0"

# simple hand-checkable case
assert run(
"""1
22 4
"""
) == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 3`|`1`| Độ dài chuỗi có ý nghĩa nhỏ nhất | 
|`1 4`|`0`| thực thi`a ≤ b`chính xác | 
|`69 1434`|`0`| To lớn`k`, phím tắt tăng trưởng hệ số | 
|`22 4`|`4`| Nhiều giải pháp hợp lệ | 

## Vỏ cạnh 

Hãy xem xét:```
1
1 3
```Phương trình là$$a+b=1.$$Các cặp không âm có thể có là`(0,1)`Và`(1,0)`. Chỉ một`(0,1)`thỏa mãn`a ≤ b`, vậy câu trả lời là`1`. Thuật toán kiểm tra cả hai khả năng thông qua việc liệt kê`b`và đếm chính xác một. 

Bây giờ hãy xem xét:```
1
1 4
```Phương trình trở thành$$a+2b=1.$$Giải pháp số nguyên duy nhất là`(1,0)`. Từ`a>b`, chuỗi bắt đầu giảm dần và không hợp lệ. Thuật toán tìm ra giải pháp nhưng từ chối nó thông qua`a <= b`điều kiện, sản xuất`0`. 

Cuối cùng hãy xem xét:```
1
69 1434
```Hệ số Fibonacci`F_{1433}`lớn hơn rất nhiều so với`69`. Không thể tồn tại nghiệm không âm. Thuật toán phát hiện ra rằng`k`vượt quá phạm vi Fibonacci được tính toán trước và ngay lập tức trả về`0`, tránh mọi công việc không cần thiết.
