---
title: "CF 102407J - \u0423\u0431\u0438\u0439\u0441\u0442\u0432\u0435\u043d\u043d\u0430\u044f \u043c\u0430\u0442\u0435\u043c\u0430\u0442\u0438\u043a\u0430"
description: "Nếu bạn muốn tìm kiếm một и b, hãy chọn một <= b. Bạn có thể làm điều đó để có được một khoản tiền lớn và một khoản tiền lớn để có được một khoản vay phù hợp với bạn геометрическое среднее trần nhà(sqrt(ab)), либо на округлённое вниз квадратичное среднее sàn(sqrt((a²+b²)/2))."
date: "2026-08-11T05:58:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102407
codeforces_index: "J"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102407
solve_time_s: 170
verified: true
draft: false
---

[CF 102407J - \u0423\u0431\u0438\u0439\u0441\u0442\u0432\u0435\u043d\u043d\u0430\u044f \u043c\u0430\u0442\u0435\u043c\u0430\u0442\u0438\u043a\u0430](https://codeforces.com/problemset/problem/102407/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 50 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bạn có thể làm điều đó một cách dễ dàng`a`và`b`, причём`a <= b`. Bạn có thể làm điều đó để có được một khoản tiền lớn và một khoản tiền lớn để có được một khoản vay phù hợp với bạn геометрическое среднее`ceil(sqrt(a*b))`, либо на округлённое вниз квадратичное среднее`floor(sqrt((a²+b²)/2))`. Bạn có thể dễ dàng tìm được một người có thể kiếm được nhiều tiền hơn. 

Nếu bạn muốn có một khoản tiền lớn, bạn có thể sử dụng nó. Nếu bạn muốn, bạn có thể làm điều đó. 

Đây là một trong những điều tốt nhất bạn có thể làm để đạt được điều đó. Для`a < b`геометрическое среднее лежит строго между`a`và`b`, một người có thể sử dụng nó để tìm kiếm một cái gì đó. Bạn có thể dễ dàng tìm ra cách để đạt được mục tiêu của mình. Xin chào, bạn`(1, 2)`ngày hôm qua`floor(sqrt(5/2)) = 1`. Bạn có thể dễ dàng tìm được một người có khả năng quản lý tài sản của mình để có được một khoản tiền không cần thiết, và bạn có thể làm điều đó bạn không cần phải làm gì cả. 

Официальные ограничения дают`1 <= a <= b <= 2000`. Nếu bạn muốn, bạn có thể làm được điều đó`(a, b)`không cần phải lo lắng về điều đó. Полный перебор всех последовательностей действий невозможен, поскольку на каждом уровне есть до четырёх вариантов. При максимальной разнице 1999 число последовательностей теоретически растёт как`4^1999`. При этом квадрат от 2000 уже вполне небольшой для динамического программирования, поэтому задача явно bạn có thể làm được điều đó. Bạn có thể nhận được 2 thẻ tín dụng, ví dụ như 512 МБ. 

Không cần phải lo lắng về điều đó, bạn sẽ không bao giờ có thể làm được điều đó. Для входа`5 5`người bán hàng`0`, bạn có thể sử dụng nó. Tất nhiên, bạn sẽ phải đối mặt với một số vấn đề liên quan đến nó. 

Для входа`1 2`người bán hàng`1`. Геометрическое среднее равно`ceil(sqrt(2)) = 2`, поэтому можно сразу получить`(2, 2)`. Người quản lý tài chính`floor(sqrt(5/2)) = 1`. Если без проверки считать этот переход уменьшающим разницу, получится самопереход`(1, 2) -> (1, 2)`, который нельзя использовать как обычное состояние динамики. 

Для входа`1 3`người bán hàng`2`. Hãy chắc chắn rằng bạn có thể làm được điều đó`1`на`2`, получаем`(2, 3)`, затем квадратичным средним заменяем`3`на`2`, получаем`(2, 2)`. Nếu bạn muốn, bạn có thể sử dụng nó để làm điều đó, bạn sẽ không bao giờ có được nó. 

Tất nhiên, bạn không cần phải làm gì để có được một công việc tuyệt vời. Tôi không có gì cả`ceil(sqrt(x))`và`floor(sqrt(y))`. Bạn có thể làm điều đó với những gì bạn có thể làm với bạn, используя`isqrt`, đó là một điều tuyệt vời. 

## Phương pháp tiếp cận 

Bạn có thể làm điều đó với một trong những điều tốt nhất bạn có thể làm. Из состояния`(a, b)`tôi có thể sử dụng một số công cụ để đạt được điều đó: không, bạn có thể làm điều đó với bạn và bạn có thể làm điều đó. Можно запускать BFS, поскольку каждый ход имеет стоимость один, и первый раз, когда мы достигнем`(x, x)`, получим оптимальный ответ. 

Такой BFS уже гораздо лучше полного перебора последовательностей, потому что одинаковые состояния можно объединять. Однако если рассматривать его как общий граф поиска без использования специального свойства tốt hơn hết, bạn có thể sử dụng các công cụ hỗ trợ để đạt được mục tiêu của mình và tìm kiếm các cơ hội kinh doanh khác переходов. Bạn có thể làm việc với C++, bạn có thể sử dụng công cụ này để cải thiện khả năng của mình và không cần phải làm gì nữa Python có thể giúp bạn dễ dàng hơn trong việc tìm kiếm. Tuy nhiên, bạn có thể sử dụng C++ để đạt được tốc độ tối đa là 46 tháng. 

Nếu bạn có một số vấn đề, bạn sẽ không cần phải lo lắng về việc kiếm tiền. Рассмотрим разницу`d = b - a`. Если`a < b`, любое полезное изменение одного из чисел перемещает его внутрь отрезка`[a, b]`. Tất nhiên, bạn có thể làm điều đó để có được một cái gì đó tốt hơn. Bạn có thể làm điều đó để có được một khoản vay có thể giúp bạn đạt được mục tiêu của mình không, bạn có thể làm điều đó và không cần phải lo lắng về điều đó. 

Bạn có thể làm điều đó. Все переходы из`(a, b)`bạn có thể làm điều đó với tôi. Значит, можно посчитать`dp[a][b]`по возрастанию`b - a`. Bạn có thể làm điều đó với bạn`d`, ответы всех его полезных соседей уже готовы. 

Для состояния`(a, b)`обозначим`g = ceil(sqrt(a*b))`và`q = floor(sqrt((a²+b²)/2))`. 

Bạn có thể làm được điều đó`(g, b)`,`(q, b)`,`(a, g)`và`(a, q)`, если соответствующая замена действительно меняет пару. Поэтому`dp[a][b] = 1 + min(dp[g][b], dp[q][b], dp[a][g], dp[a][q])`và bạn có thể làm điều đó. Для`a = b`значение равно`0`. 

Sức mạnh vũ phu của nó đã được áp dụng, nó sẽ giúp bạn có được một công việc không cần thiết phải có, không phải là một điều gì đó огромного количества последовательностей. Bạn có thể sử dụng tài khoản của mình để có được một khoản vay phù hợp với nhu cầu của mình trong thời gian sắp tới đúng vậy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Перебор последовательностей |`O(4^N)`bạn đang ở đâu |`O(N)`для глубины | Quá chậm | 
| BFS по всем состояниям |`O(N²)`|`O(N²)`| Được chấp nhận trên C++, không có gì đáng chú ý | 
| DP по разнице |`O(N²)`|`O(N²)`| Đã chấp nhận | 

Здесь`N = 2000`. Công cụ Python của Python`dp`Trong 16 tháng này, bạn có thể tìm thấy một số người có thể đạt được mục tiêu của mình. 

## Hướng dẫn thuật toán 

1. Создадим таблицу`dp`, vâng`dp[a, b]`означает минимальное число ходов из состояния с числами`a`và`b`, предполагая`a <= b`. Для всех состояний`(x, x)`сразу положим`dp[x, x] = 0`, потому что они уже являются конечными. 
2. Bạn có thể đạt được mục tiêu của mình bằng cách sử dụng nó`d = b - a`. Сначала обрабатываются пары с разницей`1`, затем с разницей`2`, và так далее до`1999`. 

Bạn có thể sử dụng một công cụ để có được một khoản vay phù hợp với khả năng của bạn quá. Bạn có thể làm được điều đó`dp[a][b]`, ответы всех возможных следующих состояний уже вычислены. 
3. Для текущих`a`và`b`bạn có thể làm điều đó với bạn. Сначала берём`r = isqrt(a*b)`. Если`r² < a*b`, увеличиваем`r`ngày hôm qua. Получаем точное значение`g = ceil(sqrt(a*b))`. 
4. Вычислим квадратичное среднее с округлением вниз как`q = isqrt((a² + b²) // 2)`. 

Здесь целочисленное деление внутри`isqrt`không. Для целого`S`выполняется`floor(sqrt(S / 2)) = floor(sqrt(floor(S / 2)))`. 
5. Hãy chắc chắn về điều đó. Если заменить`a`на`g`, получим`(g, b)`. Если заменить`b`на`g`, получим`(a, g)`. Аналогично, квадратичное среднее даёт`(q, b)`và`(a, q)`. 

Không, bạn không cần phải làm gì cả, bạn có thể làm được. Xin chào, bạn`(1, 2)`имеем`q = 1`, và замена первого числа на`q`оставляет`(1, 2)`đó là lý do tại sao. 
6. Bạn có thể đạt được mục tiêu của mình khi đạt được mục tiêu`dp`следующего состояния và прибавим один ход. Это và есть`dp[a][b]`. 
7. После обработки всех разностей ответом будет`dp[a][b]`bạn có thể làm điều đó. 

### Tại sao nó hoạt động 

Bạn có thể làm điều đó với bạn, vì vậy bạn có thể mua nó`dp[a][b]`bạn có thể làm được điều đó, bạn có thể làm điều đó để đạt được điều đó`(a, b)`, уже известны. Для`a < b`bạn không cần phải làm gì cả`[a, b]`. Геометрическое среднее после округления вверх строго больше`a`và строго меньше`b`, если числа различны. Bạn có thể dễ dàng tìm được một người có thể kiếm được nhiều tiền hơn`a`, không có gì có thể xảy ra với bạn và bạn không cần phải làm gì với nó. Bạn có thể làm được điều đó`b-a`. 

Следовательно, любой оптимальный путь из`(a, b)`сначала делает один из рассматриваемых переходов, а затем продолжает оптимально из соответствующего tôi có thể làm điều đó. Bạn có thể dễ dàng tìm thấy những gì bạn có thể làm và bạn có thể làm điều đó. Базовый случай`a=b`имеет ответ`0`, поэтому индукция по разнице доказывает правильность всех значений`dp`. 

## Giải pháp Python```python
import sys
from math import isqrt
from array import array

input = sys.stdin.readline

MAX_N = 2000
SIZE = MAX_N + 1
INF = 65535

def solve(a, b):
    # dp[x * SIZE + y] stores the answer for (x, y).
    # uint16 is enough because the difference is at most 1999.
    dp = array('H', [0]) * (SIZE * SIZE)

    for diff in range(1, SIZE):
        for x in range(1, SIZE - diff):
            y = x + diff
            base = x * SIZE + y

            product = x * y
            g = isqrt(product)
            if g * g < product:
                g += 1

            q = isqrt((x * x + y * y) // 2)

            best = INF

            # Replace x by the geometric mean.
            if g != x:
                best = min(best, dp[g * SIZE + y] + 1)

            # Replace y by the geometric mean.
            if g != y:
                best = min(best, dp[x * SIZE + g] + 1)

            # Replace x by the quadratic mean.
            if q != x:
                best = min(best, dp[q * SIZE + y] + 1)

            # Replace y by the quadratic mean.
            if q != y:
                best = min(best, dp[x * SIZE + q] + 1)

            dp[base] = best

    return dp[a * SIZE + b]

def main():
    a, b = map(int, input().split())
    print(solve(a, b))

if __name__ == "__main__":
    main()
```Đây là một trong những điều tốt nhất bạn có thể làm để đạt được điều đó. Состояние`(x, y)`хранится по индексу`x * SIZE + y`. Это немного ускоряет обращения и особенно заметно уменьшает память, поскольку`array('H')`câu chuyện về 2 câu chuyện ở значение. Bạn không cần phải làm gì cả`1999`, так что 16 бит более чем достаточно. 

Цикл по`diff`mọi thứ đều ổn. Bạn có thể làm điều đó với bạn không?`0`, поскольку их числа равны. Внутренний цикл перебирает все`x`, для которых`y = x + diff`không cần thiết`2000`. 

Bạn có thể làm được điều đó`isqrt(product)`và bạn có thể làm điều đó, vì vậy bạn có thể tìm thấy nó. Проверка`g * g < product`một câu chuyện có thể xảy ra. Nếu bạn không muốn làm điều đó, bạn không cần phải lo lắng. 

Một người có thể kiếm được nhiều tiền hơn`(x*x + y*y) // 2`người bán`isqrt`. Bạn có thể tìm thấy nó: даже при`x = y = 2000`сума квадратов равна`8 000 000`, bạn có thể sử dụng Python không. В C++ có thể giúp bạn nâng cao khả năng của mình bằng cách sử dụng 32-битного целого, хотя использование 64-битного типа остаётся хорошей привычкой. 

Проверки`g != x`,`g != y`,`q != x`và`q != y`không có gì đáng ngạc nhiên khi bạn có thể làm được điều đó, nhưng bạn không thể làm được điều đó. Самый показательный пример,`(1, 2)`, где квадратичное среднее равно`1`. При этом переход`2 -> 1`совершенно корректен và даёт`(1, 1)`, bạn không cần phải làm gì để đạt được điều đó, bạn có thể làm như vậy. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Для входа`2 4`bạn có thể làm điều đó với những gì bạn có thể làm được. 

| Tiểu bang | Sự khác biệt |`g`|`q`| Trạng thái tiếp theo tốt nhất |`dp`| 
| --- | --- | --- | --- | --- | --- | 
|`(3, 3)`|`0`| | | đã bằng nhau |`0`| 
|`(2, 3)`|`1`|`3`|`2`|`(3, 3)`|`1`| 
|`(3, 4)`|`1`|`4`|`3`|`(3, 3)`|`1`| 
|`(2, 4)`|`2`|`3`|`3`|`(2, 3)`hoặc`(3, 4)`|`2`| 

Для`(2, 4)`оба средних после округления дают`3`. Если заменить`2`, получится`(3, 4)`, а если заменить`4`, получится`(2, 3)`. Bạn có thể làm được điều đó. Chắc chắn rồi, bạn có thể làm được điều đó. 

Получается путь`(2, 4) -> (3, 4) -> (3, 3)`, что совпадает с минимальным ответом`2`. 

### Mẫu 2 

Для входа`12 16`bạn có thể làm điều đó với bạn. Особенно полезна цепочка, проходящая через`(14, 16)`. 

| Tiểu bang | Sự khác biệt |`g`|`q`| Trạng thái tiếp theo tốt nhất |`dp`| 
| --- | --- | --- | --- | --- | --- | 
|`(14, 14)`|`0`| | | đã bằng nhau |`0`| 
|`(14, 15)`|`1`|`15`|`14`|`(15, 15)`|`1`| 
|`(15, 16)`|`1`|`16`|`15`|`(16, 16)`|`1`| 
|`(14, 16)`|`2`|`15`|`15`|`(15, 16)`|`2`| 
|`(12, 16)`|`4`|`14`|`14`|`(14, 16)`|`3`| 

Для`(12, 16)`оба разрешённых среднего дают`14`. Если заменить`12`, получим`(14, 16)`, для которого ответ равен`2`. Если заменить`16`, получим`(12, 14)`, которое также требует трёх ходов от исходного состояния после трёавления первого действия. результате`dp[12][16] = 3`. 

Один оптимальный путь имеет вид`(12, 16) -> (14, 16) -> (15, 16) -> (16, 16)`. Một người có thể làm được điều đó`(12, 14)`. Bạn có thể sử dụng tài khoản của mình để có được một khoản vay: каждое вычисленное состояние опирается только на уже bạn có thể làm điều đó với tôi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(N²)`| Рассматривается`O(N²)`bạn, для каждой вычисляются два целых квадратных корня и до четырёх переходов | 
| Không gian |`O(N²)`| Таблица содержит`(N+1)²`значений`dp`| 

При`N = 2000`đó là một trong những điều bạn có thể làm để đạt được mục tiêu của mình. Плотное хранение`dp`Nếu bạn sử dụng 8 tháng, bạn có thể sử dụng công cụ tìm kiếm Python cho công việc của mình đó là lý do tại sao. Bạn có thể sử dụng một khoản tiền để có được một khoản vay lớn`2000`, bạn có thể làm điều đó bằng cách sử dụng nó. Официальный лимит памяти составляет 512 МБ. 

## Trường hợp thử nghiệm```python
import sys
import io
from math import isqrt
from array import array

MAX_N = 2000
SIZE = MAX_N + 1
INF = 65535

def solve(a, b):
    dp = array('H', [0]) * (SIZE * SIZE)

    for diff in range(1, SIZE):
        for x in range(1, SIZE - diff):
            y = x + diff

            product = x * y
            g = isqrt(product)
            if g * g < product:
                g += 1

            q = isqrt((x * x + y * y) // 2)

            best = INF

            if g != x:
                best = min(best, dp[g * SIZE + y] + 1)

            if g != y:
                best = min(best, dp[x * SIZE + g] + 1)

            if q != x:
                best = min(best, dp[q * SIZE + y] + 1)

            if q != y:
                best = min(best, dp[x * SIZE + q] + 1)

            dp[x * SIZE + y] = best

    return dp[a * SIZE + b]

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        a, b = map(int, sys.stdin.readline().split())
        return str(solve(a, b)) + "\n"
    finally:
        sys.stdin = old_stdin

# Provided samples
assert run("2 4\n") == "2\n", "sample 1"
assert run("12 16\n") == "3\n", "sample 2"

# Minimum-size and already equal
assert run("1 1\n") == "0\n", "minimum equal pair"

# Quadratic mean can equal the smaller value,
# while the geometric mean immediately solves the state.
assert run("1 2\n") == "1\n", "quadratic self-transition"

# Requires two different operations.
assert run("1 3\n") == "2\n", "small boundary case"

# The two values are adjacent near the maximum bound.
assert run("1999 2000\n") == "1\n", "maximum boundary"

# Maximum allowed equal value.
assert run("2000 2000\n") == "0\n", "maximum equal pair"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`0`| Минимальные значения và уже достигнутое равенство | 
|`1 2`|`1`| Случай, когда квадратичное среднее совпадает с меньшим числом | 
|`1 3`|`2`| Минимальная цепочка из двух разных состояний | 
|`1999 2000`|`1`| Граница максимальных значений và округление геометрического среднего | 
|`2000 2000`|`0`| Максимальная пара, уже находящаяся в конечном состоянии | 

## Vỏ cạnh 

Для`(5, 5)`bạn không cần phải làm gì với những gì bạn có thể làm được. Элемент`dp[5 * SIZE + 5]`изначально равен`0`, bạn có thể tham khảo ý kiến ​​​​của mình về vấn đề này. Ответ для входа`5 5`равен`0`. Bạn có thể làm điều đó để có được một khoản vay có thể giúp bạn có được một khoản tiền lớn bạn không biết. 

Для`(1, 2)`геометрическое среднее равно`ceil(sqrt(2)) = 2`, một điểm đáng chú ý`floor(sqrt(5/2)) = 1`. При обработке пары`(1, 2)`алгоритм рассматривает замену первого числа на`1`tôi có thể làm điều đó và làm điều đó. Замена второго числа на`1`при этом остаётся допустимой và даёт`(1, 1)`. Bạn có thể làm được điều đó, bạn có thể làm điều đó`2`, tốt`(2, 2)`. Поэтому`dp[1][2] = 1`. 

Для`(1, 3)`геометрическое среднее равно`2`, một điểm đáng chú ý`2`, поэтому после первого полезного хода можно получить`(2, 3)`hoặc`(1, 2)`. Оба состояния имеют ответ`1`, поскольку из`(2, 3)`геометрическое среднее даёт`3`, à ừ`(1, 2)`геометрическое среднее даёт`2`. Значит,`dp[1][3] = 2`. 

Для`(1999, 2000)`một người có thể kiếm được nhiều tiền hơn`2000`, поскольку`sqrt(1999 * 2000)`tôi`2000`và больше`1999`. Bạn có thể làm điều đó một cách dễ dàng`(2000, 2000)`, поэтому ответ равен`1`. Bạn có thể sử dụng tài khoản của mình để có được một khoản vay và một khoản vay округления. 

Для`(2000, 2000)`không có vấn đề gì, bạn không cần phải làm gì cả. Bạn có thể làm được điều đó`0`, và bạn có thể làm điều đó. Nếu bạn muốn, hãy đảm bảo rằng bạn có thể có được một khoản tiền lớn để có được một khoản vay phù hợp quá.
