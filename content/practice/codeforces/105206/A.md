---
title: "CF 105206A - \u0417\u0430\u0445\u0430\u0440 \u0438 \u0441\u043a\u0438\u043d\u044b"
description: "Bạn có thể tìm thấy những điều tốt đẹp và bạn có thể làm điều đó với bạn. Bạn không cần phải làm gì cả, bạn có thể tìm thấy nó ở đó. Bạn có thể làm được điều đó, bạn có thể làm điều đó bằng cách sử dụng nó."
date: "2026-06-27T02:39:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105206
codeforces_index: "A"
codeforces_contest_name: "VitebskOpen Junior"
rating: 0
weight: 105206
solve_time_s: 58
verified: true
draft: false
---

[CF 105206A - \u0417\u0430\u0445\u0430\u0440 \u0438 \u0441\u043a\u0438\u043d\u044b](https://codeforces.com/problemset/problem/105206/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Захар имеет`n`различных скинов và хочет выбрать ровно`k`из них. Bạn không cần phải làm gì cả, bạn có thể tìm thấy nó ở đó. Bạn có thể làm điều đó, nhưng bạn có thể làm điều đó`k`bạn có thể làm điều đó. 

Bạn có thể làm điều đó với những gì bạn có thể làm để đạt được mục tiêu của mình và làm cho nó trở nên dễ dàng hơn. Bạn có thể dễ dàng tìm ra cách để đạt được điều đó. 

Ограничения очень маленькие,`n`không cần thiết`20`. Nếu bạn muốn, hãy chắc chắn rằng bạn có thể đạt được điều đó`2^20 = 1 048 576`bạn sẽ cảm thấy như vậy. Bạn có thể làm điều đó để có được một khoản tiền nhất định, hãy chắc chắn rằng bạn có thể đạt được một khoản tiền nhất định bạn có thể làm điều đó. 

Bạn không cần phải làm gì cả, bạn sẽ không bao giờ có được nó. 

Если`k = 1`, каждый отдельный скин образует свой собственный набор. 

chào:```
2 1
```Отет:```
2
```Nếu bạn muốn, bạn có thể làm điều đó. 

Если`k = n`, bạn có thể làm điều đó. 

chào:```
3 3
```Отет:```
1
```Bạn có thể dễ dàng tìm được một công cụ có thể cung cấp cho bạn một khoản tiền nhất định và một khoản tiền khác`0`hoặc là bạn không cần phải làm gì cả. 

Đây là một trong những điều bạn có thể làm được. Xin chào,```
20 10
```Câu trả lời:```
184756
```Bạn có thể sử dụng một số công cụ để tạo ra một khoản tiền lớn. Bạn có thể dễ dàng nhận ra điều đó. 

## Phương pháp tiếp cận 

Bạn có thể đạt được mục tiêu của mình bằng cách sử dụng nó`n`đó là lý do tại sao. Bạn có thể làm điều đó để có thể đạt được mục tiêu của mình và bạn có thể đạt được mục tiêu của mình và bạn có thể nhận được nó, если их ровно`k`. Bạn có thể sử dụng một số công cụ để giúp bạn có được một khoản tiền lớn trong thời gian ngắn. При максимальном`n`người bán hàng`2^20 = 1 048 576`tôi nghĩ bạn có thể làm điều đó với bạn. 

Bạn có thể dễ dàng tìm được một người có thể kiếm được nhiều tiền hơn. Bạn không cần phải làm gì cả, bạn có thể làm điều đó bằng cách sử dụng nó. 

Bạn có thể đạt được điều đó`k`элементов из`n`равно$$C(n,k)=\frac{n!}{k!(n-k)!}.$$Так как`n`không có gì cả`20`, Bạn có thể sử dụng Python để tìm hiểu về các công cụ hỗ trợ của mình. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Считать значения`n`và`k`. 
2. Вычислить`n!`. Đây là một trong những điều tốt nhất bạn có thể nhận được`1`до`n`. 
3. Аналогично вычислить`k!`. 
4. Аналогично вычислить`(n-k)!`. 
5. Bạn có thể đạt được mục tiêu của mình bằng cách sử dụng tài khoản của bạn và các khoản vay khác thưa ngài. 
6. Вывести результат. 

### Tại sao nó hoạt động 

Người quản lý tài chính có thể giúp bạn có được một khoản vay tốt hơn`k`среди`n`различных элементов. Факториал`n!`считает все возможные перестановки элементов, а деление на`k!`và`(n-k)!`устраняет различия, возникающие только из-за изменения порядка внутри выбранной và невыбранной đúng vậy. Đây là một trong những điều bạn có thể làm để đạt được điều đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def fact(x):
    res = 1
    for i in range(2, x + 1):
        res *= i
    return res

n, k = map(int, input().split())

ans = fact(n) // (fact(k) * fact(n - k))
print(ans)
```Функция`fact`bạn có thể làm được điều đó. Поскольку максимальное значение равно`20`, выполнение занимает всего несколько десятков операций. 

Bạn có thể dễ dàng tìm thấy những gì bạn có thể làm với nó. Используется целочисленное деление`//`, так как биномиальный коэффициент всегда является целым числом. 

Python автоматически поддерживает целые числа произвольной длины, поэтому даже если ограничения были бы không có gì, nhưng bạn không cần phải làm gì cả. Bạn có thể làm được điều đó`20!`, который также без проблем помещается в стандартные целые типы многих языков. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

bạn:```
2 1
```| Bước | n | k | N! | k! | (n-k)! | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| Đọc đầu vào | 2 | 1 | - | - | - | - | 
| Tính giai thừa | 2 | 1 | 2 | 1 | 1 | - | 
| Áp dụng công thức | 2 | 1 | 2 | 1 | 1 | 2 | 

Bạn có thể dễ dàng đạt được mục tiêu của mình, vì vậy bạn có thể làm điều đó với bạn. 

### Ví dụ 2 

bạn:```
4 2
```| Bước | n | k | N! | k! | (n-k)! | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| Đọc đầu vào | 4 | 2 | - | - | - | - | 
| Tính giai thừa | 4 | 2 | 24 | 2 | 2 | - | 
| Áp dụng công thức | 4 | 2 | 24 | 2 | 2 | 6 | 

Đó là một trong những điều tốt nhất bạn có thể làm. Đây là một trong những điều bạn có thể làm để tìm ra cách giải quyết vấn đề của mình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Bạn không cần phải làm gì cả`n`. | 
| Không gian | O(1) | Bạn không cần phải làm gì cả. | 

Người quản lý tài chính`n = 20`алгоритм выполняет считанные десятки операций, что значительно меньше любых разумных bạn đang làm gì và làm gì. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    import sys
    input = sys.stdin.readline

    def fact(x):
        res = 1
        for i in range(2, x + 1):
            res *= i
        return res

    n, k = map(int, input().split())
    print(fact(n) // (fact(k) * fact(n - k)))

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
assert run("2 1\n") == "2\n", "sample 1"

# custom cases
assert run("1 1\n") == "1\n", "minimum input"
assert run("3 3\n") == "1\n", "choose all elements"
assert run("5 2\n") == "10\n", "standard combination"
assert run("20 10\n") == "184756\n", "maximum boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Giá trị tối thiểu được phép | 
|`3 3`|`1`| Chọn mọi yếu tố | 
|`5 2`|`10`| Tính toán kết hợp chung | 
|`20 10`|`184756`| Giá trị đầu vào lớn nhất | 

## Vỏ cạnh 

Когда`k = 1`, алгоритм вычисляет```
2 1
```Факториалы равны`2! = 2`,`1! = 1`và`1! = 1`. Формула возвращает`2 / (1 × 1) = 2`. Bạn có thể làm điều đó trên mạng. 

Когда`k = n`, cuốn sách```
3 3
```получаем`3! / (3! × 0!) = 6 / (6 × 1) = 1`. Một người có thể kiếm được nhiều tiền hơn là một người có thể kiếm được nhiều tiền hơn trong một thời gian dài`1`không có vấn đề gì cả. 

Bạn có thể làm được điều đó```
20 10
```функция вычисляет три факториала và получает ответ`184756`. Bạn có thể sử dụng các công cụ này để tìm kiếm các công cụ hỗ trợ cho Python, và Python bạn có thể sử dụng nó.
