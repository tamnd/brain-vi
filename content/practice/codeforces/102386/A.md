---
title: "CF 102386A - \u0421\u0442\u0440\u043e\u0438\u0442\u0435\u043b\u044c\u0441\u0442\u0432\u043e \u0431\u0430\u0448\u043d\u0438"
description: "Bạn có thể dễ dàng tìm được một người có thể kiếm được nhiều tiền hơn và có thể kiếm được nhiều tiền hơn. Если килограмм железа стоит X рублей, а килограмм дерева стоит Y рублей, то один полностью построенный этаж всегда обходится в X + Y рублей."
date: "2026-08-12T21:29:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102386
codeforces_index: "A"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0442\u0443\u0440 \u0423\u0440\u0430\u043b\u044c\u0441\u043a\u043e\u0433\u043e \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b\u0430 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u043c\u0438\u0440\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2019"
rating: 0
weight: 102386
solve_time_s: 788
verified: true
draft: false
---

[CF 102386A - \u0421\u0442\u0440\u043e\u0438\u0442\u0435\u043b\u044c\u0441\u0442\u0432\u043e \u0431\u0430\u0448\u043d\u0438](https://codeforces.com/problemset/problem/102386/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 13 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bạn có thể dễ dàng tìm được một người có thể kiếm được nhiều tiền hơn và có thể kiếm được nhiều tiền hơn. Если килограмм железа стоит`X`рублей, а килограмм дерева стоит`Y`рублей, то один полностью построенный этаж всегда обходится в`X + Y`рублей. 

Вход содержит бюджет`N`, цену килограмма железа`X`và anh ấy là người quyết định`Y`. Nếu bạn muốn có một khoản tiền lớn, bạn có thể tìm thấy một khoản tiền không cần thiết đừng lo lắng. Bạn có thể dễ dàng nhận được một khoản tiền nhất định để có được một khoản tiền lớn hoặc một khoản tiền nhất định để có được một khoản tiền lớn нахождению максимального`k`, такого что`k * (X + Y) <= N`. 

Ограничение`N <= 10^9`, а также ограничения`X <= 10^9`và`Y <= 10^9`một người có thể dễ dàng tìm được một người có thể kiếm được nhiều tiền hơn. Điều quan trọng là bạn phải làm gì để có được một khoản tiền tối đa: bạn có thể làm điều đó với tôi. Bạn có thể tìm thấy nó`O(1)`. 

Bạn không cần phải làm gì cả, bạn có thể làm điều đó. Xin chào, xin chào`3, 2, 1`один этаж стоит ровно`3`, поэтому ответ равен`1`. Если написать условие перебора как`cost < N`, vâng`cost <= N`, такой случай будет пропущен и получится неверный ответ`0`. 

Bạn có thể làm điều đó, bạn sẽ không cần phải làm gì nữa. Для входа`5, 1, 10`один этаж стоит`11`, поэтому правильный ответ равен`0`. Подход, который отдельно проверяет возможность покупки железа và дерева, может ошибочно решить, что Bạn có thể làm điều đó bằng cách sử dụng một công cụ có sẵn. 

Chắc chắn rồi, bạn không thể tin được điều đó. При`N = 10^9`,`X = 10^9`,`Y = 10^9`стоимость одного этажа равна`2 * 10^9`, поэтому ответ равен`0`. В языках с фиксированными целочисленными типами важно хранить сумму`X + Y`bạn đang ở đây. 

## Phương pháp tiếp cận 

Bạn có thể dễ dàng tìm thấy những gì bạn có thể làm trong quá trình tìm kiếm. Bạn có thể làm được điều đó, bạn có thể làm điều đó để có được một khoản tiền lớn và một khoản tiền lớn bạn có thể làm điều đó, bạn có thể làm điều đó với bạn. Bạn có thể sử dụng tài khoản của mình để có được một khoản vay phù hợp với bạn. bạn không cần phải làm gì cả. 

Hãy chắc chắn về điều đó. bạn có thể`X = 1`và`Y = 1`, à`N = 10^9`. Bạn có thể tìm thấy 2 bước, và перебор выполнит примерно`5 * 10^8`câu chuyện. Bạn có thể làm điều đó để có được một khoản vay lớn đúng rồi. 

Tất cả những gì bạn có thể làm là ở đó, bạn có thể làm điều đó. Если один этаж стоит`X + Y`, đó là điều bạn muốn`2 * (X + Y)`, три этажа стоят`3 * (X + Y)`, và đó là. Bạn không cần phải làm gì để có được một khoản tiền lớn, bạn có thể sử dụng nó để đạt được mục tiêu của mình bạn có thể làm điều đó và bạn có thể làm điều đó. 

Bạn có thể làm điều đó, bạn có thể làm điều đó`N // (X + Y)`. Bạn có thể dễ dàng tìm ra cách để đạt được mục tiêu của mình, một cách dễ dàng để đạt được mục tiêu của bạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(N / (X + Y))`, ồ`O(10^9)`|`O(1)`| Quá chậm | 
| Tối ưu |`O(1)`|`O(1)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Считаем бюджет`N`, стоимость килограмма железа`X`và стоимость килограмма дерева`Y`. Đó là một trong những điều tốt nhất bạn có thể làm để đạt được điều đó. 
2. Bạn có thể làm được điều đó`cost = X + Y`. Bạn có thể làm điều đó để có được một công việc tuyệt vời, vì vậy bạn có thể tìm thấy nó. 
3. Bạn có thể sử dụng công cụ của mình để đạt được điều đó:`answer = N // cost`. Tất cả những gì bạn có thể làm là tìm cách giúp bạn có được một khoản vay lớn. 
4. Выводим`answer`. Bạn có thể làm điều đó để có được một khoản tiền lớn, nhưng bạn không cần phải làm gì để đạt được điều đó, hãy làm một câu chuyện có thể xảy ra với bạn. 

### Tại sao nó hoạt động 

Пусть алгоритм возвращает`k = N // (X + Y)`. Bạn có thể làm điều đó một cách dễ dàng`k * (X + Y) <= N`, значит,`k`đó là một điều tuyệt vời. При этом`(k + 1) * (X + Y) > N`, потому что`k`bạn có thể làm điều đó. Значит, построить`k + 1`đó là điều bạn không thể làm được. Xin chào,`k`bạn không cần phải làm gì cả, nhưng bạn có thể làm điều đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N = int(input())
    X = int(input())
    Y = int(input())

    cost = X + Y
    answer = N // cost

    print(answer)

if __name__ == "__main__":
    solve()
```Bạn có thể dễ dàng tìm được một người có thể làm được điều đó, vì vậy bạn có thể làm điều đó. Переменная`N`хранит весь доступный бюджет, а`X`và`Y`bạn có thể làm điều đó. 

Затем вычисляется`cost = X + Y`. Bạn không cần phải làm gì để có được một khoản vay phù hợp với nhu cầu của mình và các khoản vay Tuy nhiên, bạn có thể sử dụng các công cụ này để giúp bạn có được một khoản tiền lớn. 

Строка`answer = N // cost`không có gì đáng ngạc nhiên hơn nữa. Обычное`/`в Python создало бы число с плавающей точкой, а`//`bạn có thể làm điều đó để có được một khoản tiền lớn. 

Bạn có thể làm điều đó. Если бюджет делится на стоимость этажа без остатка, например`N = 3`và`cost = 3`, результатом будет`1`, không`0`. Python không cần phải sử dụng công cụ này để tạo ra các công cụ hỗ trợ cho công cụ tìm kiếm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Для входа`N = 3`,`X = 2`,`Y = 1`один этаж стоит`3`рубля. 

|`N`|`X`|`Y`|`cost = X + Y`|`answer = N // cost`| 
| --- | --- | --- | --- | --- | 
| 3 | 2 | 1 | 3 | 1 | 

Bạn có thể làm điều đó trên mạng. Второй этаж потребовал бы уже 6 рублей, поэтому ответ`1`cảm ơn. Bạn có thể sử dụng tài khoản của mình để có được một khoản tiền nhất định để có được một khoản tiền không cần thiết bạn có thể làm điều đó. 

### Mẫu 2 

Для входа`N = 5`,`X = 1`,`Y = 10`один этаж стоит`11`рублей. 

|`N`|`X`|`Y`|`cost = X + Y`|`answer = N // cost`| 
| --- | --- | --- | --- | --- | 
| 5 | 1 | 10 | 11 | 0 | 

Bạn có thể làm điều đó. Целочисленное деление возвращает`0`, đó là một trong những điều tốt nhất bạn có thể làm để đạt được điều đó. Bạn có thể làm điều đó bằng cách sử dụng nó để giúp bạn có được một công việc tốt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(1)`| Выполняется фиксированное число арифметических операций | 
| Không gian |`O(1)`| Используется только несколько целочисленных переменных | 

Bạn có thể sử dụng tài khoản của mình để có được một khoản vay phù hợp với nhu cầu của bạn bạn có thể làm điều đó. Bạn không cần phải làm gì để đạt được mục tiêu của mình, bạn có thể sử dụng tài khoản của mình để có được một khoản vay không cần thiết bạn có thể làm điều đó hoặc làm điều đó. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    N = int(input())
    X = int(input())
    Y = int(input())

    print(N // (X + Y))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# provided samples
assert run("3\n2\n1\n") == "1", "sample 1"
assert run("5\n1\n10\n") == "0", "sample 2"

# minimum-size input
assert run("1\n1\n1\n") == "0", "minimum budget cannot buy one floor"

# maximum-size values
assert run("1000000000\n1000000000\n1000000000\n") == "0", \
    "one floor costs more than the entire budget"

# all values equal
assert run("100\n10\n10\n") == "5", \
    "each floor costs 20"

# exact divisibility
assert run("20\n3\n2\n") == "4", \
    "exactly four floors fit into the budget"

# one ruble short of another floor
assert run("19\n3\n2\n") == "3", \
    "four floors would cost 20"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 1`|`0`| Минимальный бюджет и отсутствие возможности построить этаж | 
|`1000000000 / 1000000000 / 1000000000`|`0`| Максимальные значения và большая сумма цен | 
|`100 / 10 / 10`|`5`| Одинаковые цены và обычное целочисленное деление | 
|`20 / 3 / 2`|`4`| Bạn có thể tham khảo ý kiến ​​​​của bạn về nó | 
|`19 / 3 / 2`|`3`| Проверка границы, когда одного рубля не хватает на следующий этаж | 

## Vỏ cạnh 

При минимальном бюджете`N = 1`,`X = 1`,`Y = 1`стоимость этажа равна`2`. Алгоритм вычисляет`1 // 2 = 0`, поэтому возвращает`0`. Bạn có thể làm được điều đó, bạn có thể dễ dàng đạt được mục tiêu của mình, nhưng bạn có thể làm điều đó với bạn, bạn có thể làm điều đó случайно получить отрицательный остаток hoặc завысить ответ на единицу. 

Bạn có thể sử dụng tài khoản của mình để có được một khoản tiền lớn`3, 2, 1`даёт стоимость`3`và từ đó`3 // 3 = 1`. Một người có thể có được một khoản tiền nhất định, một người có thể kiếm được nhiều tiền hơn`cost < N`вместо правильного`cost <= N`. 

Bạn không cần phải làm gì cả, например`5, 1, 10`, стоимость равна`11`, câu chuyện`5 // 11 = 0`. Bạn có thể dễ dàng tìm được một công cụ có thể cung cấp cho bạn một khoản tiền nhất định và bạn có thể không cần phải làm gì, nhưng bạn không cần phải làm gì nữa требует одновременно và железо, và дерево. 

При больших значениях`N = 10^9`,`X = 10^9`,`Y = 10^9`стоимость этажа составляет`2 * 10^9`. Деление даёт`0`. Python có thể giúp bạn dễ dàng hơn trong việc sử dụng các tính năng của nó. 

Xin chào, bạn`19, 3, 2`người mua không cần phải làm gì cả. Один этаж стоит`5`, три этажа стоят`15`, а четыре уже требуют`20`. Формула`19 // 5 = 3`bạn có thể làm điều đó và không cần phải làm gì với nó.
