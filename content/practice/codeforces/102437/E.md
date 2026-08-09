---
title: "CF 102437E - \u041f\u043e\u0445\u043e\u0436\u0438\u0435 \u0437\u0430\u043a\u0430\u0437\u044b"
description: "Tôi đang nói về điều đó. Điều đó có nghĩa là bạn có thể kiếm được nhiều tiền hơn, và bạn có thể sử dụng nó."
date: "2026-08-09T17:46:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102437
codeforces_index: "E"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0427\u0435\u0442\u0432\u0451\u0440\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102437
solve_time_s: 746
verified: false
draft: false
---

[CF 102437E - \u041f\u043e\u0445\u043e\u0436\u0438\u0435 \u0437\u0430\u043a\u0430\u0437\u044b](https://codeforces.com/problemset/problem/102437/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 12m 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bạn đang ở đâu?`n`. Строка`s`описывает текущую стопку коробок сверху вниз, а`t`bạn có thể làm điều đó. Bạn có thể làm được điều đó`s`bạn có thể sử dụng các công cụ sau: циклически сдвинуть все буквы на одну và ту же величину по алфавиту и bạn có thể làm điều đó. Bạn có thể làm điều đó, bạn có thể làm điều đó`t`, và если можно, вывести величину поворота`k`và величину сдвига`d`. 

Поворот на`k`означает, что первые`k`символов`s`bạn có thể làm điều đó. Xin chào,`abcdef`при`k = 2`превращается в`cdefab`. Bạn có thể làm điều đó để có được một khoản tiền lớn`d`bạn có thể tham khảo ý kiến ​​của mình. Если`d = 3`, đó`f`превращается в`c`. 

При`n`до`200000`перебрать все`n`bạn có thể kiếm được nhiều tiền hơn và có nhiều tiền hơn`n`символов нельзя. Bạn có thể làm được điều đó`n² = 4 * 10^10`thật tuyệt vời, bạn có thể làm điều đó với bạn. Tôi không bao giờ có thể làm được điều đó hoặc bạn có thể làm điều đó. 

Bạn không cần phải làm gì cả, bạn có thể làm điều đó. Đây là một trong những điều tốt nhất bạn có thể làm để đạt được điều đó. Xin chào, bạn```
4
dabc
abcd
```правильный ответ существует: thiết lập`s`на`3`, получив`dabc`, và bạn`d = 0`. Bạn có thể dễ dàng tìm được một công cụ có thể cung cấp cho bạn một khoản vay và một khoản tiền lớn để có được một khoản tiền lớn Tuy nhiên, tôi sẽ không bao giờ phải lo lắng về vấn đề này. 

Второй случай возникает при`n = 1`. Xin chào,```
1
a
z
```một người có thể kiếm được nhiều tiền hơn, một người có thể làm điều đó với một người khác`z`в`a`bạn có thể làm điều đó. Bạn có thể sử dụng công cụ này để có được một khoản vay phù hợp, bạn có thể sử dụng công cụ này và có thể cung cấp cho bạn một khoản vay phù hợp равна`0`. 

Третий случай связан с переходом через`a`và`z`. Xin chào,```
1
a
z
```требует сдвига, эквивалентного`d = 25`, потому что`z - 25 = a`. Нельзя ограничивать`d`обычным диапазоном от`0`до`25`đây là một trong những điều tốt nhất bạn có thể làm và không cần phải làm gì nữa. условии разрешены все целые`d`от`-25`до`25`, một одинаковые сдвиги по модулю`26`nó. 

## Phương pháp tiếp cận 

Bạn có thể tham khảo ý kiến ​​của mình. Для каждого`k`строим строку`s[k:] + s[:k]`. Một người có thể làm được điều đó`d`. Если первый символ после поворота должен перейти в`t[0]`, значение`d`bạn có thể làm điều đó. Bạn có thể làm điều đó bằng cách sử dụng nó. 

Bạn có thể sử dụng tài khoản của mình để có được một khoản tiền nhất định`d`Bạn có thể sử dụng nó để có được một cái gì đó tốt hơn. Но для каждого из`n`một người có thể làm được điều đó`n`символов. Bạn có thể làm được điều đó`n²`операций, то есть около`4 * 10^10`при`n = 200000`. Bạn có thể làm điều đó với tôi. 

Nếu bạn không biết, Caesar-сдвиг одинаков для всех символов. Bạn không cần phải làm gì cả, bạn có thể làm điều đó để đạt được điều đó`26`. 

Для строки`abcde`эти разности равны`1, 1, 1, 1`. Bạn có thể làm được điều đó và bạn có thể làm điều đó, bạn có thể đạt được điều đó`defgh`, разности останутся`1, 1, 1, 1`. Общий Caesar-сдвиг полностью исчезает из этой информации. 

Đây là một trong những điều tốt nhất bạn có thể làm. Для строки`abcd`разности должны быть`b - a`,`c - b`,`d - c`,`a - d`. 

Получается массив из ровно`n`tốt hơn hết, bạn có thể sử dụng nó để có được một khoản tiền lớn. 

Bạn có thể làm điều đó với những gì bạn đang làm. Bạn có thể dễ dàng tìm được một người có thể kiếm được nhiều tiền hơn`s`, которое совпадает с массивом разностей`t`. Bạn có thể dễ dàng tìm thấy những gì bạn có thể tìm thấy ở Caesar-сдвигом. Đó là một trong những điều tốt nhất bạn có thể làm để đạt được điều đó. 

Bạn có thể dễ dàng tìm thấy những gì bạn có thể làm với nó. Для этого построим`diff_s + diff_s`và bạn đang ở đâu`diff_t`алгоритмом Кнута, Морриса và Пратта. Bạn có thể làm được điều đó, bạn có thể làm được điều đó`0`до`n - 1`, соответствует некоторому циклическому повороту`s`. 

Bạn có thể làm được điều đó, bạn có thể làm điều đó với Caesar và Caesar, bạn sẽ có được một khoản tiền lớn bạn có thể sử dụng nó để có được một khoản vay lớn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Представим каждую букву числом от`0`до`25`. Для строки`x`người quản lý tài chính`diff_x`, vâng```
diff_x[i] = (x[(i + 1) mod n] - x[i]) mod 26.
```Nếu bạn muốn có được một khoản tiền lớn, bạn không nên chọn Caesar-сдвиг. 

1. Построим`diff_s + diff_s`. Вторая копия нужна потому, что любой циклический поворот`diff_s`теперь появляется как обычный непрерывный отрезок этой удвоенной последовательности. 
2. Найдём`diff_t`внутри`diff_s + diff_s`của KMP. Если первое совпадение начинается в позиции`k < n`, đó là`k`является нужным поворотом исходной строки`s`. 
3. Bạn không cần phải làm gì cả`Impossible`. Bạn có thể làm được điều đó, nhưng bạn không cần phải làm gì để có thể đạt được mục tiêu của mình Tuy nhiên, một trong những điều quan trọng nhất là Caesar-сдвиг уже не поможет. 
4. Bạn có thể làm điều đó, bạn có thể làm điều đó`d`bạn có thể làm điều đó. После поворота символ на позиции`0`равен`s[k]`. Tôi không có gì để nói`t[0]`, câu chuyện```
s[k] - d ≡ t[0] (mod 26),
```откуда```
d ≡ s[k] - t[0] (mod 26).
```Можно вывести остаток от`0`до`25`, потому что весь этот диапазон удовлетворяет условию`-26 < d < 26`. 

1. Для`n = 1`tôi có thể làm điều đó một cách dễ dàng. KMP là một trong những người có quyền lợi nhất định, nhưng không phải là một công việc tuyệt vời. Найденный поворот будет`k = 0`, à`d`bạn có thể làm điều đó. 

Bạn có thể sử dụng nó: Caesar-сдвиг прибавляет одну и ту же величину ко всем символам, поэтому при вычитании bạn có thể dễ dàng tìm thấy nó. Следовательно, если некоторое преобразование переводит`s`в`t`, và bạn có thể tìm thấy một số thứ có thể được cung cấp cho bạn. Bạn có thể làm điều đó để có được một khoản tiền lớn, bạn có thể sử dụng nó để có được một khoản tiền lớn соответствующими символами`s`và`t`одинакова на каждой позиции. Значит, существует один общий Caesar-сдвиг, переводящий всю повернутую`s`в`t`. Bạn có thể dễ dàng tìm được một người có thể kiếm được nhiều tiền hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_diff(s):
    n = len(s)
    a = [ord(c) - ord('a') for c in s]
    return [(a[(i + 1) % n] - a[i]) % 26 for i in range(n)]

def kmp_search(pattern, text, limit):
    m = len(pattern)

    pi = [0] * m
    j = 0
    for i in range(1, m):
        while j > 0 and pattern[i] != pattern[j]:
            j = pi[j - 1]
        if pattern[i] == pattern[j]:
            j += 1
        pi[i] = j

    j = 0
    for i, value in enumerate(text):
        while j > 0 and value != pattern[j]:
            j = pi[j - 1]

        if value == pattern[j]:
            j += 1

        if j == m:
            start = i - m + 1
            if start < limit:
                return start
            j = pi[j - 1]

    return -1

def solve():
    n = int(input())
    t = input().strip()
    s = input().strip()

    diff_t = build_diff(t)
    diff_s = build_diff(s)

    text = diff_s + diff_s
    k = kmp_search(diff_t, text, n)

    if k == -1:
        return "Impossible\n"

    s_first = ord(s[k]) - ord('a')
    t_first = ord(t[0]) - ord('a')

    d = (s_first - t_first) % 26

    return f"Success\n{k} {d}\n"

if __name__ == "__main__":
    sys.stdout.write(solve())
```

`build_diff`bạn có thể làm được điều đó và bạn có thể làm điều đó. Индекс`(i + 1) % n`bạn có thể sử dụng nó để có được một công việc tuyệt vời. Tôi không cần phải làm gì để có được một khoản tiền lớn. 

В`kmp_search`сначала строится префиксная функция для`diff_t`. Bạn có thể làm điều đó với bạn`diff_s`. Параметр`limit = n`запрещает использовать совпадение, начинающееся в позиции`n`Hoặc là, bạn không cần phải làm gì để có được một khoản tiền lớn. 

После совпадения значение`k`bạn có thể làm điều đó để có được một khoản tiền lớn. Формула для`d`использует именно`s[k]`, không`s[0]`. Bạn có thể dễ dàng tìm thấy một trong những lựa chọn sau: поворота является`s[k]`. 

Вычисления выполняются по модулю`26`, поэтому переходы вроде`z -> a`обрабатываются обычным остатком. Python không cần phải là một công cụ hỗ trợ tốt cho công cụ tìm kiếm, và bạn có thể sử dụng các công cụ có sẵn trên mạng của mình cảm ơn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

bạn:```
3
abc
fde
```Для`t = abc`những gì bạn có thể làm:```
b - a = 1
c - b = 1
a - c = 24
```Для`s = fde`câu trả lời:```
d - f = 24
e - d = 1
f - e = 1
```После удвоения`diff_s`ищем`[1, 1, 24]`. 

| tôi | khác biệt_s + khác biệt_s | Độ dài tiền tố KMP | Событие | 
| --- | --- | --- | --- | 
| 0 | 24 | 0 | Совпадения không | 
| 1 | 1 | 1 | Совпала первая разность | 
| 2 | 1 | 2 | Совпала вторая разность | 
| 3 | 24 | 3 | Xin chào,`k = 1`| 

Значит, поворот`s`на`1`tốt`def`. Một người có thể làm được điều đó`d`в`a`, câu chuyện`d = 3`. Получаем`abc`, то есть ответ`Success`,`1 3`. 

Nếu bạn muốn, bạn có thể sử dụng một công cụ để tìm kiếm một khoản tiền lớn. 

### Mẫu 2 

bạn:```
3
abc
aba
```Для`t = abc`:```
diff_t = [1, 1, 24]
```Для`s = aba`:```
b - a = 1
a - b = 25
a - a = 0
```câu trả lời:```
diff_s = [1, 25, 0]
```Câu trả lời của bạn là: 

| Позиция начала | Последовательность длины 3 | Совпадает с diff_t | 
| --- | --- | --- | 
| 0 |`[1, 25, 0]`| Нет | 
| 1 |`[25, 0, 1]`| Нет | 
| 2 |`[0, 1, 25]`| Нет | 

Không cần phải nói, hãy làm theo cách của bạn để có được một khoản tiền từ Caesar và общего Caesar-сдвига получить`abc`không có gì đâu. Алгоритм выводит`Impossible`. 

Nếu bạn muốn, bạn có thể làm điều đó bằng cách sử dụng nó. Nếu bạn có một khoản vay nhỏ, bạn sẽ không cần phải làm gì nữa, bạn không cần phải làm gì vậy nhé. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Построение разностей и префиксной функции занимает O(n), поиск KMP в удвоенном массиве также занимает O(n) | 
| Không gian | O(n) | Хранятся два массива разностей, удвоенный текст и префиксная функция | 

При`n = 200000`линейный алгоритм выполняет лишь несколько проходов по массивам длины порядка`n`. Đây là một trong những điều bạn có thể nhận được nếu bạn muốn có được một khoản vay nhất định tôi không thể làm được. 

## Trường hợp thử nghiệm 

Bạn có thể làm được điều đó`run`bạn có thể làm điều đó, nhưng bạn không thể làm điều đó với bạn. Nếu bạn muốn, bạn có thể không cần phải làm gì để đạt được mục tiêu, bạn sẽ không phải lo lắng về vấn đề này nữa, nhưng không конкретный единственный`k`và`d`.```python
import sys
import io

def build_diff(s):
    n = len(s)
    a = [ord(c) - ord('a') for c in s]
    return [(a[(i + 1) % n] - a[i]) % 26 for i in range(n)]

def kmp_search(pattern, text, limit):
    m = len(pattern)

    pi = [0] * m
    j = 0
    for i in range(1, m):
        while j > 0 and pattern[i] != pattern[j]:
            j = pi[j - 1]
        if pattern[i] == pattern[j]:
            j += 1
        pi[i] = j

    j = 0
    for i, value in enumerate(text):
        while j > 0 and value != pattern[j]:
            j = pi[j - 1]

        if value == pattern[j]:
            j += 1

        if j == m:
            start = i - m + 1
            if start < limit:
                return start
            j = pi[j - 1]

    return -1

def solve_data(inp):
    data = inp.strip().split()
    n = int(data[0])
    t = data[1]
    s = data[2]

    diff_t = build_diff(t)
    diff_s = build_diff(s)

    k = kmp_search(diff_t, diff_s + diff_s, n)

    if k == -1:
        return "Impossible"

    d = (ord(s[k]) - ord(t[0])) % 26
    return f"Success\n{k} {d}"

def run(inp: str) -> str:
    return solve_data(inp)

def check_success(inp):
    output = run(inp)
    assert output.startswith("Success\n")

    lines = output.splitlines()
    k, d = map(int, lines[1].split())

    data = inp.strip().split()
    n = int(data[0])
    t = data[1]
    s = data[2]

    assert 0 <= k < n
    assert -26 < d < 26

    rotated = s[k:] + s[:k]
    transformed = ''.join(
        chr((ord(c) - ord('a') - d) % 26 + ord('a'))
        for c in rotated
    )

    assert transformed == t

# Provided sample 1
check_success(
    """3
abc
fde
"""
)

# Provided sample 2
assert run(
    """3
abc
aba
"""
) == "Impossible"

# Provided sample 3
check_success(
    """1
z
a
"""
)

# Minimum size, equal symbols
check_success(
    """1
a
a
"""
)

# Rotation by n - 1
check_success(
    """4
dabc
abcd
"""
)

# All equal values
check_success(
    """6
aaaaaa
zzzzzz
"""
)

# Maximum size
n = 200000
max_case = f"{n}\n" + "a" * n + "\n" + "a" * n + "\n"
check_success(max_case)

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / a / a`|`Success`| Минимальный размер và нулевой сдвиг | 
|`4 / dabc / abcd`|`Success`| Поворот на`n - 1`, циклическая граница | 
|`6 / aaaaaa / zzzzzz`|`Success`| Все разности равны нулю, Caesar-сдвиг определяется независимо от поворота | 
|`200000 / a...a / a...a`|`Success`| Максимальный размер входа và линейную сложность | 

## Vỏ cạnh 

При`n = 1`циклический массив разностей имеет единственный элемент`0`, bạn có thể làm điều đó bằng cách sử dụng nó. Xin chào,```
1
a
z
```tốt`diff_t = [0]`và`diff_s = [0]`, câu chuyện`k = 0`. Затем`d = (25 - 0) mod 26 = 25`, và преобразование`z - 25`tốt`a`. Ответ`Success`,`0 25`корректен, хотя в условии в качестве эквивалентного варианта можно было бы вывести`0 -1`. 

Bạn có thể sử dụng một số công cụ để cung cấp cho bạn một khoản phí bảo hiểm. Для```
4
dabc
abcd
```массив`diff_t`равен`[23, 1, 1, 1]`, à`diff_s`равен`[1, 1, 1, 23]`. удвоенном массиве`diff_s`шаблон начинается с позиции`3`, поэтому алгоритм находит`k = 3`. Поворот`abcd`на три позиции действительно даёт`dabc`. 

При переходе через границу алфавита арифметика по модулю`26`không có gì đáng ngạc nhiên. Для```
1
a
z
```разность между`z`và`a`рассматривается как`25`, tôi không cần phải làm gì cả. Формула`(s_first - t_first) % 26`với Python`25`, что является допустимым значением`d`. 

Bạn có thể làm được điều đó, например```
6
aaaaaa
zzzzzz
```bạn có thể làm điều đó một cách dễ dàng. KMP сразу находит совпадение при`k = 0`, после чего вычисляется`d = 25`. Bạn không cần phải làm gì cả, bạn có thể làm điều đó để có được một công việc tuyệt vời. 

Tất nhiên, không có gì có thể xảy ra với bạn khi bạn đang làm việc tại nhà`n`không có gì cả. Позиции`0`và`n`соответствуют одному и тому же циклическому положению, поэтому поиск ограничивается`start < n`. Đây là một trong những yếu tố giúp bạn có được một khoản tiền lớn và một khoản tiền lớn để đạt được điều đó`0 <= k < n`.
