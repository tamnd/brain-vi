---
title: "CF 102330E - \u0413\u0435\u043e\u0440\u0433\u0438\u0439 \u0438 \u0432\u043e\u0435\u043d\u043a\u043e\u043c\u0430\u0442"
description: "Bạn có thể làm được điều đó và bạn có thể làm điều đó. Bạn có thể sử dụng nó trong một thời gian dài: далее до k-й. Для печати j известна длительность обслуживания t[j]. Tôi sẽ nói về điều đó với [i]."
date: "2026-08-13T04:02:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102330
codeforces_index: "E"
codeforces_contest_name: "\u0421\u0438\u0440\u0438\u0443\u0441.2019.\u041d\u043e\u044f\u0431\u0440\u044c.\u041e\u0447\u043d\u044b\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 102330
solve_time_s: 282
verified: true
draft: false
---

[CF 102330E - \u0413\u0435\u043e\u0440\u0433\u0438\u0439 \u0438 \u0432\u043e\u0435\u043d\u043a\u043e\u043c\u0430\u0442](https://codeforces.com/problemset/problem/102330/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 42 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Есть`n`человек và`k`câu trả lời. Bạn có thể sử dụng nó trong một thời gian dài: chào bạn`k`-й. Для печати`j`известна длительность обслуживания`t[j]`. 

Человек`i`việc làm ở thời điểm hiện tại`a[i]`. Bạn có thể sử dụng nó để làm điều đó. Bạn không cần phải làm gì cả, và bạn có thể kiếm được nhiều tiền hơn. Bạn có thể dễ dàng tìm được một công cụ có thể giúp bạn có được một công việc tuyệt vời. Bạn có thể sử dụng tài khoản của mình để đạt được mục tiêu, một người có thể đạt được lợi ích kinh tế cao đó là lý do tại sao. 

У каждого человека есть ограничение`b[i]`. При`b[i] = 0`tôi không thể làm được điều đó nữa. Bạn có thể làm được điều đó`a[i] + b[i]`. Bạn không cần phải làm gì để có được một công việc tuyệt vời như vậy, bạn có thể làm điều đó. Bạn có thể dễ dàng tìm được một công cụ có thể hỗ trợ bạn, bạn có thể sử dụng nó để đạt được điều đó, vì vậy bạn có thể làm điều đó người bán hàng. 

Нужно для каждого человека вывести`completion - a[i]`, если он получил все`k`печатей. Bạn có thể làm điều đó với một người khác, không có gì đáng ngạc nhiên`-1`. 

Người quản lý tài sản của bạn có thể làm được điều đó không?`n * k <= 10^5`. Vì vậy, bạn có thể tìm thấy một số thứ có thể giúp bạn có được một khoản vay lớn vâng. Полиномиальные решения вроде`O(n^2 k)`уже слишком велики: при`n = 10^5`và`k = 1`они дают порядка`10^10`операций. Người bán hàng`n * k`, плюс сортировка людей перед первой очередью. 

Tôi không bao giờ có thể làm được điều đó, vì vậy bạn có thể làm điều đó một cách dễ dàng. Bạn có thể sử dụng công cụ này để đạt được mục tiêu của mình. Для```
1 1
1
1 1
```ответ равен`1`, không`-1`. Một người có thể kiếm được nhiều tiền hơn trong thời gian này`2`, ровно через`b = 1`bạn có thể làm điều đó, và bạn có thể làm điều đó. 

Bạn có thể làm điều đó, bạn có thể làm điều đó trong một thời gian ngắn để có được một khoản vay không cần thiết, và một người bán hàng печать. Xin chào,```
1 2
3 1
1 3
```Bạn có thể kiếm được nhiều tiền hơn ở thời điểm hiện tại`4`, который совпадает с дедлайном человека. Bạn không cần phải làm gì cả, bạn không cần phải làm gì cả, bạn sẽ phải làm gì để có được khoản vay đó và bạn không cần phải làm gì cả. Ответ равен`-1`. 

Bạn có thể sử dụng nó để có được một khoản tiền lớn, bạn có thể sử dụng nó để có được một khoản tiền tối đa, bạn có thể sử dụng nó человеком. Xin chào,```
2 1
5
1 5
1 5
```Một người có thể kiếm được nhiều tiền hơn trong thời gian này`6`. Bạn có thể làm điều đó và làm điều đó với bạn`6`bạn sẽ thấy điều đó. Tôi không cần phải lo lắng về điều đó. Ответ равен`5 -1`. 

xin chào,`b[i] = 0`нельзя трактовать как дедлайн`a[i]`. Bạn không cần phải làm gì cả. Xin chào,```
1 1
7
10 0
```Ответ равен`7`. 

## Phương pháp tiếp cận 

Bạn có thể sử dụng một công cụ để có được một khoản tiền nhất định. Для каждой печати нужно хранить очередь людей, добавлять в нее людей, которые пришли, брать bạn có thể làm được điều đó và bạn có thể làm điều đó. Bạn có thể sử dụng tài khoản của mình để có được một khoản tiền lớn, nhưng không cần phải trả tiền cho bạn обслуживаемого человека заново искать его позицию в очереди среди всех людей. Bạn có thể làm được điều đó`O(n^2)`, а для всех печатей`O(n^2 k)`. Bạn có thể làm điều đó`n = 100000`và`k = 1`это около`10^10`Tuy nhiên, bạn không cần phải làm gì cả. 

Tất nhiên, bạn không cần phải làm gì để có được tài khoản tốt, bạn có thể sử dụng FIFO-порядке каждой очереди. Bạn có thể làm điều đó với một số người khác, bạn có thể tìm thấy những gì bạn có thể làm khi bạn làm điều đó, bạn có thể làm điều đó đây là một trong những điều tốt nhất mà bạn có thể làm. Для первой печати это порядок`(a[i], i)`. Bạn có thể sử dụng một công cụ để có được một khoản vay phù hợp với bạn, một cách nhanh chóng bạn có thể làm điều đó một cách dễ dàng. 

Chắc chắn rồi, bạn có thể làm điều đó một cách dễ dàng. Bạn có thể làm được điều đó, và bạn có thể làm được điều đó. Nếu bạn muốn, bạn có thể làm điều đó. Bạn có thể dễ dàng nhận được một khoản tiền lớn, nhưng bạn có thể dễ dàng tìm thấy một khoản tiền nhất định để có được một khoản tiền nhất định thưa ông. Bạn có thể làm điều đó với một người khác, bạn có thể làm điều đó với bạn. Bạn không cần phải làm gì cả, bạn có thể làm điều đó để có được một khoản tiền lớn, hãy chắc chắn rằng bạn có thể làm điều đó với bạn bạn có thể làm điều đó. Bạn có thể làm điều đó. 

Bạn có thể sử dụng một khoản tiền nhỏ để có được một khoản tiền nhất định để có được một khoản tiền tối đa cho bạn bạn có thể làm điều đó. Bạn sẽ không bao giờ có được một công việc tuyệt vời như vậy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n^2 k)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n log n + nk)`|`O(n)`| Đã chấp nhận | 

Здесь`nk <= 10^5`, поэтому линейная обработка всех пар человек-печать укладывается в граничение. Bạn có thể làm điều đó bằng cách sử dụng nó. 

## Hướng dẫn thuật toán 

1. Làm thế nào để có được một khoản tiền lớn`a[i]`và абсолютный момент ухода`a[i] + b[i]`. Для`b[i] = 0`một người có thể dễ dàng tìm thấy những gì bạn có thể làm ở đó. Một người có thể kiếm được nhiều tiền hơn`(a[i], i)`, bạn có thể làm điều đó bằng cách sử dụng nó. 
2. Hãy chắc chắn rằng bạn không thể làm được điều đó, bạn có thể làm điều đó`k`-й. В начале очереди текущей печати находятся только люди, которые уже успешно прошли предыдущую печать. Bạn có thể làm điều đó. 
3. Для текущей печати поддерживаем`cur`, nhưng, bạn có thể làm được điều đó, bạn không cần phải làm gì cả và bạn có thể làm điều đó прибывших людей. Пока очередь пуста,`cur`một người có thể làm được điều đó. Bạn có thể làm điều đó để có được một khoản vay không cần thiết`cur`. 
4. Hãy tham gia vào công việc của bạn. Если`cur`уже не меньше его дедлайна, человек должен был уйти раньше orли прямо сейчас, поэтому обслуживания không cần phải lo lắng. Следующий человек может быть обслужен в тот же`cur`, если он подходит по сроку. 
5. Если человек еще находится в военкомате, вычисляем`finish = cur + t[j]`. Если`finish`không cần phải làm gì cả, hãy làm điều đó. Если это последняя печать, сохраняем`finish`rất nhiều người có thể làm điều đó. 
6. Если`finish`Tuy nhiên, điều quan trọng là bạn không cần phải làm gì để đạt được mục tiêu của mình. Bạn có thể làm điều đó để đạt được điều đó và bạn có thể làm điều đó, hãy làm điều đó`cur`bạn không thể làm được điều đó nữa. Đó là một trong những điều tốt nhất mà bạn có thể làm. 
7. Bạn có thể sử dụng một công cụ để đạt được mục tiêu của mình trong thời gian ngắn, nhưng bạn không thể làm được điều đó bạn không cần phải làm gì cả. Bạn có thể làm điều đó và không cần phải làm gì với nó. Một người có thể sử dụng một khoản tiền để có được một khoản tiền lớn để có được một khoản vay`finish < deadline`. Для последней печати допустимо`finish <= deadline`. 
8. Nếu bạn muốn, bạn có thể sử dụng công cụ này và bạn có thể sử dụng công cụ này, bạn có thể sử dụng nó список следующей очереди с временем прихода`finish`. Bạn có thể sử dụng dịch vụ của mình để có được một khoản vay hợp lý, bạn có thể sử dụng dịch vụ của mình để đạt được điều đó Tuy nhiên, bạn không cần phải lo lắng về điều đó. 
9. Bạn có thể sử dụng tài khoản của mình để đạt được điều đó`finish - a[i]`. Для всех остальных оставляем`-1`. 

### Tại sao nó hoạt động 

Bạn có thể sử dụng một công cụ có thể giúp bạn có được một khoản tiền lớn trong thời gian ngắn, nhưng bạn có thể làm điều đó để có được một công việc tốt hơn. ровно тех людей, которые уже пришли к текущему моменту и еще не были обработаны, причем в точном FIFO-người áp dụng. Человек, стоящий впереди, обязан либо получить обслуживание, либо уйти, и никто позади него не tôi không thể làm được điều đó nữa. Nếu bạn muốn, tôi có thể làm điều đó. Bạn không cần phải làm gì, bạn có thể làm điều đó, bạn có thể làm điều đó, và`cur`bạn có thể làm điều đó, bạn có thể làm điều đó với bạn. Bạn có thể sử dụng các công cụ có thể giúp bạn có được một khoản tiền lớn và bạn có thể kiếm được nhiều tiền hơn bạn có thể làm điều đó. 

После каждой печати список передаваемых людей содержит ровно тех людей, которые получили эту печать và tôi không cần phải làm gì cả. Bạn có thể làm điều đó bằng cách sử dụng các công cụ hỗ trợ của bạn. Chắc chắn rồi, bạn đang cố gắng hết sức để đạt được điều đó. По индукции после`k`-й печати алгоритм точно знает всех людей, завершивших всю процедуру. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

def process_station(arrivals, duration, deadlines, last):
    """
    arrivals: list of (arrival_time, person_id), already sorted
    duration: time needed for this stamp
    deadlines: absolute departure times, INF means unlimited
    last: whether this is the final stamp

    Returns:
        list of (completion_time, person_id) for people who may
        continue to the next stamp, or for all successful people
        if this is the last stamp.
    """
    queue = []
    head = 0
    ptr = 0
    cur = 0

    result = []

    while ptr < len(arrivals) or head < len(queue):
        if head == len(queue):
            if ptr >= len(arrivals):
                break
            if cur < arrivals[ptr][0]:
                cur = arrivals[ptr][0]

        while ptr < len(arrivals) and arrivals[ptr][0] <= cur:
            queue.append(arrivals[ptr])
            ptr += 1

        person_arrival, person = queue[head]
        head += 1

        deadline = deadlines[person]

        if cur >= deadline:
            continue

        finish = cur + duration

        if finish > deadline:
            cur = deadline
            continue

        cur = finish

        if last:
            result.append((finish, person))
        else:
            if finish < deadline:
                result.append((finish, person))

    return result

def solve():
    n, k = map(int, input().split())
    t = list(map(int, input().split()))

    a = [0] * n
    deadlines = [INF] * n

    for i in range(n):
        ai, bi = map(int, input().split())
        a[i] = ai
        if bi != 0:
            deadlines[i] = ai + bi

    arrivals = [(a[i], i) for i in range(n)]
    arrivals.sort()

    for station in range(k):
        arrivals = process_station(
            arrivals,
            t[station],
            deadlines,
            station == k - 1
        )

        if not arrivals:
            break

    answer = [-1] * n

    if k == 0:
        return

    for finish, person in arrivals:
        answer[person] = finish - a[person]

    print(*answer)

if __name__ == "__main__":
    solve()
```ở đây`deadlines[i]`bạn có thể làm điều đó, nhưng bạn không cần phải làm gì cả. Đây là một trong những điều bạn có thể làm để có được một khoản vay lớn`cur`hoặc`finish`с одним числом. 

Функция`process_station`bạn có thể làm điều đó với bạn.`ptr`указывает на еще не добавленного человека, а`head`bạn không cần phải làm gì cả. Bạn có thể làm điều đó để có được một khoản tiền không cần thiết, hãy chắc chắn rằng bạn có thể làm điều đó một cách dễ dàng извлекается один раз. 

Bạn có thể làm điều đó,`cur`một người có thể làm được điều đó. Bạn có thể làm được điều đó`arrival <= cur`. Bạn có thể sử dụng nó để có được một khoản tiền tối đa cho bạn bạn sẽ thấy điều đó. 

Проверка`cur >= deadline`không, bạn có thể làm điều đó để tìm ra cách giải quyết. Если же`cur < deadline`, сотрудник начинает работать немедленно. При`finish > deadline`человек không cần phải có một khoản tiền lớn để có thể đạt được mục tiêu của bạn`cur = deadline`. Bạn có thể sử dụng tài khoản của mình để có được một khoản vay phù hợp, bạn có thể sử dụng nó để có được một khoản vay phù hợp với bạn câu trả lời. 

Проверка`finish < deadline`для промежуточных печатей нужна из-за того, что после получения этой печати процедура еще не đúng. Если`finish == deadline`, человек получает текущую печать, в тотот же момент покидает военкомат и не попопадает в следующую очередь. Bạn có thể làm điều đó, bạn có thể làm điều đó với bạn. 

В Python là một công cụ hỗ trợ tốt cho công việc của bạn, bạn có thể sử dụng nó`a[i] + b[i]`và bạn không cần phải làm gì cả. Значение`10**30`намного больше любого возможного времени в тесте и безопасно используется как обозначение отсутствия thưa ngài. 

## Ví dụ đã hoạt động 

### Mẫu 1 

bạn:```
1 1
1
1 1
```Bạn có thể làm điều đó, bạn có thể làm điều đó. 

| đến | người | cur trước | thời hạn | kết thúc | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | 2 | hoàn thành | 

một người có thể kiếm được nhiều tiền hơn trong thời gian này`1`, печать занимает одну единицу времени và заканчивается в`2`. Bạn có thể làm điều đó`2`, một người có thể sử dụng nó để đạt được điều đó. Время пребывания равно`2 - 1 = 1`. 

Отет:```
1
```Этот пример проверяет главную границу`finish == deadline`một câu chuyện. 

### Mẫu 2 

bạn:```
6 1
17
10 100
3 30
80 59
24 86
59 76
69 15
```Tôi không thể làm điều đó nữa, nhưng tôi sẽ làm điều đó. Bạn có thể sử dụng một số thẻ tín dụng: человек 2, человек 1, человек 4, человек 5, человек 6, человек 3. 

| người | đến | thời hạn | cur trước | kết thúc / khởi hành | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 3 | 33 | 3 | 20 | thành công | 
| 1 | 10 | 110 | 20 | 37 | thành công | 
| 4 | 24 | 110 | 37 | 54 | thành công | 
| 5 | 59 | 135 | 59 | 76 | thành công | 
| 6 | 69 | 84 | 76 | 84 | lá | 
| 3 | 80 | 139 | 84 | 101 | thành công | 

Bạn có thể làm điều đó một cách dễ dàng`93`, không có gì đáng chú ý`84`. Он поэтому занимает сотрудника с`76`до`84`, после чего уходит. Человек 3 уже стоял в очереди с момента`80`, поэтому начинает ровно в`84`và заканчивает в`101`. 

Итоговые времена пребывания равны`27, 17, 21, 30, 17, -1`, nhưng bạn không cần phải làm gì cả, bạn không cần phải làm gì cả. Bạn có thể làm điều đó để đạt được điều đó và đạt được điều đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n + nk)`| Bạn có thể sử dụng nó để có được một khoản vay không cần thiết phải có trong thẻ tín dụng của bạn печати | 
| Không gian |`O(n)`| Bạn có thể làm được điều đó và bạn có thể làm điều đó, nhưng bạn không cần phải trả tiền`n`| 

Условие`n * k <= 10^5`ограничивает суммарное число обработок человек-печать величиной`10^5`. Người quản lý tài khoản không cần thiết`O(n log n)`, bạn có thể sử dụng nó. Bạn có thể kiếm được nhiều tiền hơn từ một khoản tiền lớn và 256 tháng. 

## Trường hợp thử nghiệm```python
import io
import sys

INF = 10**30

def solve_instance(inp: str) -> str:
    data = inp.strip().split()
    it = iter(data)

    n = int(next(it))
    k = int(next(it))

    t = [int(next(it)) for _ in range(k)]

    a = [0] * n
    deadlines = [INF] * n

    for i in range(n):
        ai = int(next(it))
        bi = int(next(it))
        a[i] = ai
        if bi != 0:
            deadlines[i] = ai + bi

    arrivals = [(a[i], i) for i in range(n)]
    arrivals.sort()

    for station in range(k):
        duration = t[station]
        last = station == k - 1

        queue = []
        head = 0
        ptr = 0
        cur = 0
        nxt = []

        while ptr < len(arrivals) or head < len(queue):
            if head == len(queue):
                if ptr >= len(arrivals):
                    break
                cur = max(cur, arrivals[ptr][0])

            while ptr < len(arrivals) and arrivals[ptr][0] <= cur:
                queue.append(arrivals[ptr])
                ptr += 1

            _, person = queue[head]
            head += 1

            deadline = deadlines[person]

            if cur >= deadline:
                continue

            finish = cur + duration

            if finish > deadline:
                cur = deadline
                continue

            cur = finish

            if last:
                nxt.append((finish, person))
            elif finish < deadline:
                nxt.append((finish, person))

        arrivals = nxt

        if not arrivals:
            break

    answer = [-1] * n

    for finish, person in arrivals:
        answer[person] = finish - a[person]

    return " ".join(map(str, answer))

# Provided sample 1
assert solve_instance("""\
1 1
1
1 1
""") == "1", "sample 1"

# Provided sample 2
assert solve_instance("""\
6 1
17
10 100
3 30
80 59
24 86
59 76
69 15
""") == "27 17 21 30 17 -1", "sample 2"

# Minimum size and unlimited waiting
assert solve_instance("""\
1 1
7
10 0
""") == "7", "minimum size and b=0"

# All equal arrival times, with one person missing the deadline
assert solve_instance("""\
2 1
5
1 5
1 5
""") == "5 -1", "equal arrivals and exact waiting boundary"

# Exact deadline on an intermediate stamp
assert solve_instance("""\
1 2
3 1
1 3
""") == "-1", "intermediate stamp ends exactly at deadline"

# All values equal, multiple stamps
assert solve_instance("""\
3 2
2 3
1 100
1 100
1 100
""") == "9 12 15", "equal arrivals and equal deadlines"

# Maximum n*k = 100000, n=100000, k=1.
# Every person can wait forever and all arrive together.
n = 100000
parts = [f"{n} 1", "1"]
parts.extend(["1 0"] * n)
max_input = "\n".join(parts) + "\n"

max_output = solve_instance(max_input).split()
assert len(max_output) == n
assert max_output[0] == "1"
assert max_output[-1] == str(n)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 7 / 10 0`|`7`| Phiên bản tối thiểu và chờ đợi không giới hạn | 
|`2 1 / 5 / 1 5 / 1 5`|`5 -1`| Số lượt đến ngang nhau và một người có thời hạn chính xác vào thời điểm dịch vụ trước đó kết thúc | 
|`1 2 / 3 1 / 1 3`|`-1`| Việc hoàn thiện tem trung gian đúng thời hạn không được phổ biến | 
|`3 2 / 2 3 / 1 100 / 1 100 / 1 100`|`9 12 15`| Số lượt đến bằng nhau, xử lý FIFO lặp đi lặp lại và một số tem | 
|`100000 1 / 1 / 100000 identical arrivals`| Câu trả lời đầu tiên`1`, câu trả lời cuối cùng`100000`| Giá trị tối đa được phép của`n * k`và hiệu suất tuyến tính | 

## Vỏ cạnh 

### Chờ đợi không giới hạn 

cho```
1 1
7
10 0
```thời hạn được thể hiện bằng`INF`. Người đó bắt đầu lúc`10`, kết thúc tại`17`, và được chấp nhận. Thuật toán không bao giờ áp dụng so sánh thời hạn hữu hạn cho người này, vì vậy kết quả là`7`. 

### Tem cuối cùng kết thúc đúng thời hạn 

cho```
1 1
1
1 1
```thời hạn tuyệt đối là`2`. Thời gian hoàn thành được tính toán cũng`2`. Vì đây là con tem cuối cùng nên điều kiện`finish <= deadline`thành công và câu trả lời là`2 - 1 = 1`. 

### Tem trung gian kết thúc đúng thời hạn 

cho```
1 2
3 1
1 3
```con tem đầu tiên bắt đầu lúc`1`và kết thúc tại`4`, chính xác vào thời hạn của người đó. Bản thân con tem đầu tiên đã được hoàn thành nhưng người đó không thể tiếp tục vì thủ tục chưa hoàn tất. Thuật toán có chủ ý yêu cầu`finish < deadline`trước khi thêm một người vào hàng tiếp theo, người đó sẽ biến mất sau dấu đầu tiên và câu trả lời cuối cùng vẫn còn`-1`. 

### Một người rời đi trong khi đang được phục vụ 

cho```
6 1
17
10 100
3 30
80 59
24 86
59 76
69 15
```người 6 bắt đầu lúc`76`, trong khi thời hạn là`84`. Dịch vụ được yêu cầu sẽ kết thúc vào lúc`93`, do đó dịch vụ bị gián đoạn do khởi hành lúc`84`. Bộ thuật toán`cur = 84`còn hơn là`cur = 93`. Người thứ 3 đã đến`80`, sau đó có thể bắt đầu tại`84`và kết thúc tại`101`. Đây chính xác là lý do tại sao người thứ 3 chi tiêu`21`đơn vị thời gian trong văn phòng thay vì`30`. 

### Số lượng đến bằng nhau 

cho```
2 1
5
1 5
1 5
```cả hai người vào hàng đợi đầu tiên vào thời điểm đó`1`và người 1 được ưu tiên vì chỉ số nhỏ hơn. Người 1 về đích lúc`6`, đúng thời hạn nên họ thành công vì đây là con tem cuối cùng. Người 2 đã chờ đợi cho đến khi`6`, đó cũng là thời hạn cuối cùng của họ nên họ rời đi mà không được phục vụ. Đầu ra là`5 -1`. 

Việc biểu diễn hàng đợi duy trì thứ tự này một cách tự động vì danh sách ban đầu được sắp xếp theo`(arrival_time, person_id)`, trong khi mọi hàng đợi sau sẽ kế thừa thứ tự hoàn thành thành công từ dấu trước đó.
