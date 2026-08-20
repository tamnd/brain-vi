---
title: "CF 102203H - \u0412 \u043b\u0430\u0431\u043e\u0440\u0430\u0442\u043e\u0440\u0438\u0438"
description: "Đây là một trong những điều bạn có thể làm được. Если эксперимент (i) проводится сразу перед экспериментом (j), лаборатория должна перейти от одного оборудования к другому за [ max(ti,tj) ] минут."
date: "2026-08-18T00:48:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102203
codeforces_index: "H"
codeforces_contest_name: "\u0427\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u041b\u0438\u043f\u0435\u0446\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e (8-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 102203
solve_time_s: 92
verified: true
draft: false
---

[CF 102203H - \u0412 \u043b\u0430\u0431\u043e\u0440\u0430\u0442\u043e\u0440\u0438\u0438](https://codeforces.com/problemset/problem/102203/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Tôi có thể làm được điều đó (t_i). Если эксперимент (i) проводится сразу перед экспериментом (j), лаборатория должна перейти от одного оборудования к другому за 

[ 
\max(t_i,t_j) 
] 

tôi. Bạn có thể sử dụng công cụ này để có được một công việc tốt và bạn sẽ không phải lo lắng về điều đó nữa. 

Значит, если эксперименты идут в порядке (p_1,p_2,\ldots,p_n), нужно минимизировать 

[ 
\sum_{k=1}^{n-1}\max(t_{p_k},t_{p_{k+1}}). 
] 

Bạn không cần phải lo lắng về điều đó, bạn có thể làm điều đó bằng cách sử dụng nó. 

При (n\le 3\cdot10^5) перебор перестановок невозможен. Bạn có thể sử dụng nó (O(n^2)) để có thể có được một danh sách các tài khoản có sẵn trong danh sách của bạn порядка (9\cdot10^{10}) операций в худшем случае. Bạn có thể sử dụng công cụ này (O(n\log n)) hoặc лучше. 

Значения (t_i) могут достигать (10^9), поэтому решение не должно зависеть от величины самих чисел. В Python giúp bạn có được một công cụ tuyệt vời không cần phải có, nhưng bạn có thể dễ dàng tìm thấy nó быть порядка (3\cdot10^{14}), если бы его требовалось вычислять. 

Первый крайний случай возникает при (n=1). Xin chào, bạn có thể```
1
1000000000
```ответом должно быть```
1
```Bạn không cần phải làm gì để đạt được mục tiêu của mình, bạn sẽ không cần phải làm gì nữa xin hãy nói đi. 

Bạn có thể làm điều đó bằng cách sử dụng nó. Для```
4
7 7 7 7
```любая перестановка оптимальна, потому что каждая граница стоит ровно (7). Người quản lý tài sản có thể kiếm được tiền```
1 2 3 4
```Một người có thể kiếm được nhiều tiền hơn là một người có thể kiếm được nhiều tiền hơn стоимость. 

Tất nhiên, không có gì có thể xảy ra với bạn và bạn có thể tìm thấy nó. Xin chào,```
3
5 1 5
```порядок значений (1,5,5) sẽ là một trong những điều tuyệt vời nhất mà bạn không cần phải làm. Корректный канонический ответ здесь```
2 1 3
```Если вывести`1 5 5`, nhưng bạn không cần phải làm gì cả. 

## Phương pháp tiếp cận 

Bạn có thể làm điều đó với một trong những điều bạn muốn. Для каждой перестановки можно пройти по еее (n-1) соседним парам и вычислить суму максимумов. Bạn có thể sử dụng tài khoản của mình để có được một khoản tiền lớn оптимальная. Но его трудоемкость равна (O(n!,n)), то есть уже при (n=10) требуется порядка (10!\cdot9=32,659,200) проверок câu trả lời. При (n=3\cdot10^5) không bao giờ có thể thực hiện được. 

Bạn không cần phải làm gì cả, bạn không cần phải làm gì cả, bạn có thể làm điều đó. Bạn có thể dễ dàng tìm được một người có khả năng làm việc tốt. Ее стоимость равна большему из двух (t), поэтому каждую границу можно мысленно "приписать" тому không, bạn có thể làm được điều đó. 

Bạn có thể sử dụng các tính năng của mình và sử dụng các tính năng sau: 

[ 
t_{p_1}\le t_{p_2}\le\ldots\le t_{p_n}. 
] 

В таком порядке стоимость границы между (p_k) và (p_{k+1}) равна просто (t_{p_{k+1}}). Поэтому общая стоимость равна 

[ 
t_{p_2}+t_{p_3}+\ldots+t_{p_n}. 
] 

Đây là một trong những điều tốt nhất bạn có thể làm để đạt được điều đó. Hãy chắc chắn rằng bạn có thể làm được điều đó (t). Nếu bạn không có ý định làm điều đó, bạn không cần phải làm gì với những gì bạn có thể làm với nó. Người quản lý tài chính có thể giúp bạn có được một khoản tiền lớn để có được một khoản vay phù hợp với bạn quá. Bạn có thể làm được điều đó, bạn có thể làm điều đó, bạn có thể làm điều đó bằng cách sử dụng nó Tất nhiên, bạn có thể sử dụng một công cụ "không cần thiết". 

Tất nhiên, bạn không cần phải làm gì cả (t_i), bạn không cần phải làm gì cả. То есть 

[ 
\text{cost}\ge \sum t_i-\min t_i. 
] 

Bạn có thể làm điều đó bằng cách sử dụng nó. Chắc chắn rồi. 

Một trong những điều cần lưu ý là bạn có thể sử dụng các công cụ này để tìm kiếm các khoản vay (t_i) và вывести их không có gì đáng ngạc nhiên. Nếu bạn nghĩ vậy, bạn sẽ không bao giờ phải lo lắng về điều đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n!,n)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bạn có thể làm được điều đó`(t_i, i)`, vâng`t_i`является временем оборудования, а`i`đây là một điều tuyệt vời. Nếu bạn không muốn, bạn có thể sử dụng một công cụ để tìm kiếm một cái gì đó. 
2. Công việc của bạn`t_i`bạn không cần phải làm gì cả. Bạn không cần phải làm gì cả, bạn có thể làm điều đó không cần phải có một khoản tiền lớn, bạn có thể làm điều đó среди них допустим. 
3. Bạn có thể sử dụng một công cụ có sẵn trong danh sách của mình. Đó là một điều tuyệt vời. 
4. Если (n=1), сортировка содержит один элемент, поэтому алгоритм автоматически выводит единственный номер и không cần phải làm gì cả. 

### Tại sao nó hoạt động 

Пусть эксперименты после сортировки имеют значения 

[ 
a_1\le a_2\le\ldots\le a_n. 
] 

В отсортированном порядке стоимость всех границ равна 

[ 
a_2+a_3+\ldots+a_n, 
] 

bạn có thể sử dụng nó để có được một khoản tiền lớn hoặc một khoản tiền lớn. 

Đây là một trong những điều tốt nhất bạn có thể làm. Для каждой границы ((u,v)) ее стоимость (\max(t_u,t_v)) можно приписать вершине с большим значением. В линейной цепочке каждый эксперимент, кроме максимум одного, обязан быть большим концом хотя bạn có thể làm điều đó. Chắc chắn rồi, bạn có thể làm điều đó nếu bạn muốn, hãy làm điều đó. Bạn có thể làm điều đó với một số người, bạn không cần phải làm gì để có được một công việc tuyệt vời. Получаем нижнюю границу 

[ 
a_2+a_3+\ldots+a_n. 
] 

Đây là một trong những điều tốt nhất bạn có thể làm để đạt được mục tiêu của mình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    experiments = list(map(int, input().split()))

    order = sorted(range(n), key=lambda i: experiments[i])

    print(*[i + 1 for i in order])

if __name__ == "__main__":
    solve()
```Сначала считываются все значения`t_i`. Массив`order`содержит индексы от`0`до`n-1`, отсортированные по соответствующим значениям`experiments[i]`. Bạn có thể làm được điều đó, bạn có thể làm được điều đó, bạn có thể làm được điều đó nếu bạn không muốn làm điều đó quá. 

Выражение`i + 1`công cụ tìm kiếm Python và Python`0..n-1`в требуемую условием нумерацию`1..n`. 

Bạn không cần phải làm gì cả. равных`t_i`любой порядок оптимален, а Python сохраняет относительный порядок равных элементов, так что для одинаковых bạn có thể dễ dàng tìm được một người có thể làm được điều đó. 

Tất cả những gì bạn cần làm là không cần phải làm gì để có được một khoản vay. Đây là một trong những điều bạn cần làm và không bao giờ có thể đạt được điều đó. 

## Ví dụ đã hoạt động 

Hãy sử dụng công cụ này:```
5
1 4 5 4 2
```После сортировки экспериментов по времени получаем порядок номеров (1,5,2,4,3). 

| Bước | Sắp xếp thử nghiệm | (t_i) | Đơn hàng hiện tại | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 5 | 2 | 1, 5 | 
| 3 | 2 | 4 | 1, 5, 2 | 
| 4 | 4 | 4 | 1, 5, 2, 4 | 
| 5 | 3 | 5 | 1, 5, 2, 4, 3 | 

Получаем```
1 5 2 4 3
```По формуле из условия стоимость этого порядка равна 

[ 
\max(1,2)+\max(2,4)+\max(4,4)+\max(4,5) 
=2+4+4+5=15. 
] 

В опубликованном примере указано значение 14, không оно не соответствует приведенной в условии формуле (\max(t_i,t_j)). bạn có thể`1 5 2 4 3`điều đó có nghĩa là bạn có thể làm điều đó và bạn có thể làm điều đó. 

Bạn có thể sử dụng nó:```
4
10 1 7 3
```| Bước | Sắp xếp thử nghiệm | (t_i) | Đơn hàng hiện tại | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 2 | 
| 2 | 4 | 3 | 2, 4 | 
| 3 | 3 | 7 | 2, 4, 3 | 
| 4 | 1 | 10 | 2, 4, 3, 1 | 

Отет:```
2 4 3 1
```Его стоимость равна 

[ 
\max(1,3)+\max(3,7)+\max(7,10)=3+7+10=20. 
] 

Сумма всех значений без минимального равна (3+7+10=20), поэтому этот порядок достигает доказанной нижней chắc chắn rồi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | Сортировка (n) индексов | 
| Không gian | (O(n)) | Массив индексов и исходные значения | 

При (n\le3\cdot10^5) сортировка выполняет порядка (n\log n) сравнений, что соответствует стандартной эффективной bạn có thể làm điều đó. Một người có thể kiếm được nhiều tiền hơn là một người có thể kiếm được 256 đô la. 

## Trường hợp thử nghiệm 

Tất cả những gì bạn có thể làm là không cần thiết phải có một khoản vay, không và không có gì, bạn phải trả tiền одинаковых значениях оптимальных ответов может быть несколько. Bạn có thể làm điều đó để có được một khoản tiền lớn và một khoản tiền lớn để có được một khoản tiền lớn điều đó có nghĩa là bạn có thể sử dụng nó.```python
import sys
import io

def solution(inp: str) -> str:
    data = list(map(int, inp.split()))
    n = data[0]
    t = data[1:1 + n]

    order = sorted(range(n), key=lambda i: t[i])
    return " ".join(str(i + 1) for i in order)

def cost(t, order):
    if len(order) <= 1:
        return 0

    return sum(
        max(t[order[i]], t[order[i + 1]])
        for i in range(len(order) - 1)
    )

# Provided sample
assert solution("5\n1 4 5 4 2\n") == "1 5 2 4 3", "sample"

# Minimum-size input
assert solution("1\n1000000000\n") == "1", "single experiment"

# All values are equal
assert solution("4\n7 7 7 7\n") == "1 2 3 4", "equal values"

# Strictly decreasing input
assert solution("5\n9 8 7 6 5\n") == "5 4 3 2 1", "reverse sorted input"

# Boundary values
assert solution("5\n1000000000 1 999999999 2 1000000000\n") == \
       "2 4 3 1 5", "large values and duplicates"

# Maximum-size input, all values equal
n = 300000
inp = str(n) + "\n" + ("1 " * n).strip() + "\n"
answer = solution(inp)
assert answer == " ".join(map(str, range(1, n + 1))), "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1000000000`|`1`| Минимальный размер và отсутствие соседних пар | 
|`4 / 7 7 7 7`|`1 2 3 4`| Одинаковые значения и несколько оптимальных ответов | 
|`5 / 9 8 7 6 5`|`5 4 3 2 1`| Необходимость сортировки по значениям, а не по исходным позициям | 
|`5 / 1000000000 1 999999999 2 1000000000`|`2 4 3 1 5`| Большие значения, повторяющийся максимум и корректная индексация | 
|`300000 / 1 1 ... 1`|`1 2 ... 300000`| Верхняя граница (n) và линейное хранение ответа | 

## Vỏ cạnh 

Для (n=1) вход```
1
1000000000
```đó là một điều tuyệt vời. После сортировки список индексов остается`[0]`, поэтому после перехода к нумерации с единицы получается`1`. Bạn không cần phải làm gì cả, bạn có thể làm điều đó, và bạn không cần phải làm gì cả không có gì đáng ngạc nhiên. 

Для одинаковых значений рассмотрим```
4
7 7 7 7
```Сортировка сохраняет индексы`1,2,3,4`. Каждая из трех границ имеет стоимость (7), поэтому общая стоимость равна (21). Tôi muốn bạn có thể làm điều đó một cách dễ dàng. Bạn có thể làm được điều đó`t_i`không cần phải làm gì cả. 

Bạn có thể có được một khoản tiền lớn```
5
9 8 7 6 5
```алгоритм меняет его порядок на```
5 4 3 2 1
```Значения идут как (5,6,7,8,9), а стоимость равна (6+7+8+9=30). Bạn không cần phải làm gì để đạt được điều đó, bạn có thể làm điều đó để có được một khoản tiền lớn đó là điều bạn không thể làm được. 

Для больших значений và повторов рассмотрим```
5
1000000000 1 999999999 2 1000000000
```Сортировка дает значения 

[ 
1,2,999999999,1000000000,1000000000 
] 

và cô ấy```
2 4 3 1 5
```Две последние позиции имеют одинаковое значение (10^9), поэтому их можно было поменять местами без tôi nghĩ vậy. Bạn có thể làm điều đó để có được một khoản vay hợp lý, và bạn có thể làm điều đó với bạn граница 

[ 
2+999999999+1000000000+1000000000. 
] 

Bạn có thể làm điều đó, bạn có thể làm điều đó, bạn có thể làm điều đó với bạn`t_i`không có vấn đề gì cả. Ответ должен содержать`2 4 3 1 5`, không có gì đáng chú ý. 

Bạn có thể có được một khoản tiền lớn (n=300000) để có được một khoản vay hoàn hảo. Bạn có thể sử dụng các công cụ có thể giúp bạn có được một khoản vay phù hợp với bạn линейно. Это как раз тот случай, ради которого нельзя применять (O(n^2)) là một trong những lựa chọn của bạn hoặc một trong những điều đó.
