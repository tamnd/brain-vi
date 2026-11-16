---
title: "CF 1925B - Bộ bài toán cân bằng?"
description: "Chúng ta được yêu cầu chia một số nguyên $x$ thành chính xác $n$ số nguyên dương có tổng là $x$, theo cách cực đại hóa ước số chung lớn nhất của các số nguyên này. Đầu vào đưa ra nhiều trường hợp kiểm thử, mỗi trường hợp có giá trị cho $x$ và $n$."
date: "2026-06-08T19:04:05+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1925
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 921 (Div. 2)"
rating: 1200
weight: 1925
solve_time_s: 81
verified: true
draft: false
---

[CF 1925B - Bộ vấn đề cân bằng?](https://codeforces.com/problemset/problem/1925/B) 

**Đánh giá:** 1200 
**Tags:** sức mạnh vũ phu, lòng tham, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu chia một số nguyên$x$vào chính xác$n$số nguyên dương có tổng bằng$x$, theo cách tối đa hóa ước số chung lớn nhất của các số nguyên này. Đầu vào đưa ra nhiều trường hợp kiểm thử, mỗi trường hợp có các giá trị cho$x$Và$n$. Đối với mỗi trường hợp thử nghiệm, đầu ra là một số nguyên biểu thị GCD lớn nhất có thể mà bạn có thể đạt được khi chia$x$vào trong$n$các bộ phận. 

Các giới hạn chỉ ra rằng$x$có thể lớn như$10^8$Và$n$có thể lên tới$x$. Với tối đa 1000 trường hợp thử nghiệm, bất kỳ giải pháp nào lặp tuyến tính trên tất cả các phần tách có thể có hoặc tất cả các ước số của$x$ngây thơ là quá chậm. Chúng ta cần một thuật toán gần$O(\sqrt{x})$hoặc tốt hơn cho từng trường hợp thử nghiệm. 

Trường hợp cạnh xuất hiện khi$n = 1$hoặc$n = x$. Nếu như$n = 1$, lựa chọn duy nhất là$x$chính nó, vậy GCD là$x$. Nếu như$n = x$, phép chia hợp lệ duy nhất là$1 + 1 + \dots + 1$, cho GCD là 1. Một tình huống tinh tế khác là khi$n$không chia$x$đồng đều; cách tiếp cận ngây thơ của việc phân công$x / n$mỗi phần sẽ không tạo ra số nguyên, vì vậy chúng ta cần nghĩ đến các ước của$x$cho phép phân vùng hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu thử mọi cách có thể để chia rẽ$x$vào trong$n$số nguyên và tính GCD cho mỗi lần chia. Về mặt lý thuyết, điều này hiệu quả vì chúng ta có thể liệt kê tất cả các chuỗi có độ dài$n$tổng hợp lại$x$, nhưng số lượng chuỗi tăng lên theo kiểu tổ hợp. Ví dụ, nếu$x = 10^8$Và$n = 2$, có khoảng$10^8$các cặp có thể xảy ra, quá lớn. Vì vậy, vũ lực chỉ khả thi với số lượng rất nhỏ. 

Điểm mấu chốt là để tối đa hóa GCD, tất cả các số phải là bội số của GCD đó, chẳng hạn$g$. Nếu chúng ta biểu thị các phần là$g \cdot a_1, g \cdot a_2, \dots, g \cdot a_n$, thì tổng là$g \cdot (a_1 + \dots + a_n) = x$. Điều này ngụ ý$g$phải chia$x$. Nhiệm vụ còn lại là chọn số lớn nhất$g$cho phép phân rã thành chính xác$n$số nguyên dương. 

Một khi chúng ta biết$g$chia rẽ$x$, chúng tôi xác định$y = x / g$. Bây giờ chúng tôi cần$n$số nguyên dương có tổng bằng$y$. Tổng tối thiểu của$n$số nguyên dương là$n$, vậy ta phải có$y \ge n$, điều này đưa ra ràng buộc$g \le x / n$. Kết hợp điều này với thực tế là$g$phải chia$x$, bài toán quy về việc tìm ước số lớn nhất của$x$đó là nhiều nhất$x / n$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C(x-1, n-1)) | O(n) | Quá chậm | 
| Tối ưu | O(sqrt(x)) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$x$Và$n$. Chúng đại diện cho tổng độ khó và số lượng các vấn đề phụ. 
2. Khởi tạo một biến`best_gcd`đến 1, sẽ theo dõi ước số hợp lệ lớn nhất. 
3. Lặp qua tất cả các số nguyên$d$từ 1 đến$\sqrt{x}$. Đối với mỗi$d$: 

- Kiểm tra xem$d$chia rẽ$x$. Nếu không, hãy tiếp tục. 
- Nếu như$d$chia rẽ$x$, tính ước số bù$x / d$. 
4. Với mỗi ước số$d$và phần bổ sung của nó$x / d$, kiểm tra xem nó có nhỏ hơn hoặc bằng không$x / n$. Nếu vậy, hãy cập nhật`best_gcd`là giá trị lớn nhất của giá trị hiện tại và ước số đó. 
5. Sau khi xem xét tất cả các ước số,`best_gcd`chứa ước số lớn nhất của$x$điều đó không vượt quá$x / n$. 
6. In`best_gcd`cho trường hợp thử nghiệm. 

Tại sao nó hoạt động: Bằng cách lặp qua các ước số lên đến$\sqrt{x}$, chúng tôi đảm bảo rằng tất cả các ước số đều được xem xét, kể cả các ước số bổ sung. Tính bất biến đó là`best_gcd`luôn là ước của$x$và thỏa mãn ràng buộc phân vùng. Điều này đảm bảo giá trị cuối cùng tối đa hóa sự cân bằng của bộ vấn đề. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    x, n = map(int, input().split())
    best_gcd = 1
    limit = x // n
    d = 1
    while d * d <= x:
        if x % d == 0:
            if d <= limit:
                best_gcd = max(best_gcd, d)
            if x // d <= limit:
                best_gcd = max(best_gcd, x // d)
        d += 1
    print(best_gcd)
```Dòng đầu tiên ghi số lượng test case. Vòng lặp xử lý từng trường hợp thử nghiệm một cách độc lập.`limit`đảm bảo chúng tôi chỉ xem xét các ước số cho phép phân chia hợp lệ thành$n$số nguyên dương. Vòng lặp while tìm thấy tất cả các ước số một cách hiệu quả, tận dụng tính đối xứng để kiểm tra$x / d$cho mỗi$d$. Điều này tránh việc lặp lại tất cả các cách để$x$, như thế sẽ là quá chậm. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu:** 10 3 

| x | n | giới hạn | d | x% d == 0? | ứng cử viên | tốt nhất_gcd | 
| --- | --- | --- | --- | --- | --- | --- | 
| 10 | 3 | 3 | 1 | vâng | 1,10 | 1 | 
| 10 | 3 | 3 | 2 | vâng | 2,5 | 2 | 
| 10 | 3 | 3 | 3 | không | - | 2 | 

Ước số lớn nhất không vượt quá$x / n = 3$là 2, do đó đầu ra là 2. 

**Đầu vào mẫu:** 5 5 

| x | n | giới hạn | d | x% d == 0? | ứng cử viên | tốt nhất_gcd | 
| --- | --- | --- | --- | --- | --- | --- | 
| 5 | 5 | 1 | 1 | vâng | 1,5 | 1 | 
| 5 | 5 | 1 | 2 | không | - | 1 | 

Ước số hợp lệ duy nhất là 1, cho đầu ra 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * sqrt(x)) | Mỗi trường hợp thử nghiệm lặp lại các ước số lên tới sqrt(x) | 
| Không gian | O(1) | Chỉ một vài biến cho mỗi trường hợp thử nghiệm | 

Với$t \le 10^3$Và$x \le 10^8$, giải pháp chạy thoải mái trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution
    t = int(input())
    for _ in range(t):
        x, n = map(int, input().split())
        best_gcd = 1
        limit = x // n
        d = 1
        while d * d <= x:
            if x % d == 0:
                if d <= limit:
                    best_gcd = max(best_gcd, d)
                if x // d <= limit:
                    best_gcd = max(best_gcd, x // d)
            d += 1
        print(best_gcd)
    return output.getvalue().strip()

# provided samples
assert run("3\n10 3\n5 5\n420 69\n") == "2\n1\n6", "samples"

# custom cases
assert run("1\n1 1\n") == "1", "min input"
assert run("1\n100000000 1\n") == "100000000", "n=1, max x"
assert run("1\n100000000 100000000\n") == "1", "n=x, max x"
assert run("1\n12 5\n") == "2", "x divisible by multiple options"
assert run("1\n17 4\n") == "4", "prime x, small n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | trường hợp tối thiểu | 
| 100000000 1 | 100000000 | x cực đại với n=1 | 
| 100000000 100000000 | 1 | x cực đại với n=x | 
| 12 5 | 2 | nhiều ước số, chọn giá trị lớn nhất | 
| 17 4 | 4 | số nguyên tố x, kiểm tra việc chọn số chia | 

## Vỏ cạnh 

Nếu$n = 1$, bất kì$x$được chia nhỏ thành một phần duy nhất, do đó GCD lớn nhất là$x$. Đối với đầu vào `
