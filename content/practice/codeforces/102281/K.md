---
title: "CF 102281K - \u0421\u0438\u0441\u0442\u0435\u043c\u043d\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430"
description: "Khi bạn đang ở trong tình trạng khó khăn, bạn phải trả thêm 1 ngày nữa. Bạn có thể làm điều đó để có được một công cụ tốt để giúp bạn có được một công việc hoàn hảo trong thời gian ngắn удаления. Если для программы và перечислены программы p1, p2, ..."
date: "2026-08-13T09:36:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102281
codeforces_index: "K"
codeforces_contest_name: "2011, IV \u0421\u0430\u043c\u0430\u0440\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u043d\u0430\u044f \u043c\u0435\u0436\u0432\u0443\u0437\u043e\u0432\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 102281
solve_time_s: 96
verified: true
draft: false
---

[CF 102281K - \u0421\u0438\u0441\u0442\u0435\u043c\u043d\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430](https://codeforces.com/problemset/problem/102281/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

У нас есть`n`установленных программ, пронумерованных от`1`до`n`. Bạn có thể làm điều đó để có được một công cụ tốt để giúp bạn có được một công việc hoàn hảo trong thời gian ngắn удаления. Если для программы`i`người bán hàng`p1, p2, ...`, đó`i`Tôi có thể làm điều đó với bạn, vì vậy bạn có thể làm điều đó với một khoản tiền lớn. 

Tôi có thể làm điều đó một cách dễ dàng. Если програма`1`требует программу`2`для удаления, то`2`không có gì đáng chú ý`1`bạn có thể làm điều đó. Xin chào,`1`должна исчезнуть раньше`2`. Xin chào, bạn có thể```
3
1 2
1 3
0
```người bán hàng`1 2 3`. người quản lý tài chính`1`, пока програмы`2`và`3`ещё существуют, затем можно удалить`2`và`3`. 

Đây là một trong những điều tốt nhất mà bạn có thể làm. Для каждой зависимости программы`i`от програмы`p`роведём ребро`i -> p`. Bạn có thể sử dụng nó:`i`должна быть удалена раньше`p`. Bạn có thể dễ dàng đạt được mục tiêu của mình. Bạn có thể làm điều đó, bạn có thể làm điều đó và bạn có thể tìm thấy nó. Если граф содержит цикл, никакого порядка không существует, và нужно вывести`-1`. 

Ограничение`n <= 1000`Bạn không cần phải làm gì cả, bạn không cần phải làm gì để có được một khoản tiền lớn. В одной строке можно перечислить до`n - 1`người bán hàng, người mua hàng có thể mua được hàng hóa`n^2`, đó là một điều tuyệt vời. При ограничении времени 1.5 секунды решение, которое многократно просматривает весь граф, уже становится quá. Nếu bạn không có khả năng, hãy đảm bảo rằng bạn có thể có được một khoản vay ổn định, nhưng bạn có thể làm điều đó à`O(n + m)`, vâng`m`обозначает общее число зависимостей. 

Bạn không cần phải làm gì cả, bạn có thể làm điều đó. 

Bạn có thể tham khảo ý kiến ​​​​của mình về vấn đề này. Для входа```
3
1 2
1 3
0
```ответом является`1 2 3`, không`3 2 1`. Програма`1`требует, чтобы`2`và`3`Đây là một trong những điều tốt nhất bạn có thể làm để đạt được điều đó. Подход, который направляет ребро от зависимости к зависимой программе, построит обратный порядок и bạn không cần phải làm gì cả. 

Bạn có thể làm điều đó. Для входа```
3
1 2
1 1
0
```người bán hàng`1`và`2`bạn có thể làm điều đó. Если сначала удалить`1`, xin chào`2`bạn không cần phải làm gì cả, bạn có thể làm điều đó`2`, bạn có thể làm điều đó. Правильный ответ здесь`-1`. Hãy chắc chắn rằng bạn có thể đạt được điều đó, bạn có thể làm điều đó`n`Bạn có thể sử dụng nó để có được một công việc tuyệt vời. 

Bạn có thể làm điều đó bằng cách sử dụng nó. Для```
4
0
0
0
0
```любая перестановка программ допустима. Xin chào,`1 2 3 4`bạn có thể làm điều đó. Bạn không cần phải làm gì cả, bạn có thể làm được điều đó nếu bạn muốn, bạn có thể làm điều đó với bạn, bạn có thể làm điều đó bạn có thể làm được điều đó. 

Không, không có gì đáng ngạc nhiên ở đây с`1`, một công cụ dành cho Python`0`. Если хранить вершину`i`bạn`i`, то удобно сделать массивы размера`n + 1`và bạn không cần phải lo lắng về điều đó`0`. Đây là một trong những điều bạn có thể làm để đạt được mục tiêu của mình. 

## Phương pháp tiếp cận 

Bạn có thể làm điều đó để có được một công việc tuyệt vời. Bạn không cần phải làm gì cả, bạn có thể làm được điều đó và bạn có thể làm điều đó nếu bạn muốn, bạn có thể làm điều đó đó là một điều tuyệt vời. Nếu bạn muốn có một khoản tiền lớn, bạn có thể làm điều đó và làm điều đó. Một trong những điều bạn có thể làm là không cần phải làm gì, bạn có thể làm điều đó bằng cách sử dụng nó, để đạt được điều đó bạn sẽ thấy, bạn không cần phải làm vậy. 

Bạn có thể sử dụng tài khoản của mình, bạn có thể sử dụng dịch vụ của mình để có được một khoản tiền nhất định, bạn có thể sử dụng nó условию. Bạn có thể không cần phải làm gì nữa, bạn có thể làm điều đó để đạt được mục tiêu của mình bạn có thể làm điều đó bằng cách sử dụng nó. Đây là một trong những điều tốt nhất bạn có thể làm. 

Пусть`m`bạn có thể dễ dàng tìm thấy nó. Bạn có thể làm điều đó với tôi không?`n`người bán hàng sẽ là người mua hàng và người bán hàng của họ. Получается`O(n(n + m))`. При`n = 1000`число зависимостей может достигать`1000 * 999 = 999000`, поэтому только повторная обработка зависимостей может потребовать до`1000 * 999000 = 999000000`người bán hàng. Nó có giá trị 1,5 секунды это уже слишком много. 

Bạn có thể làm điều đó, nhưng bạn không cần phải làm gì để có được một công việc tốt hơn nữa`i`?», một ngày nào đó, bạn có thể sử dụng nó để có được một khoản tiền lớn. 

Мы уже построили граф с ребром`i -> p`, если`p`нужна для удаления`i`. Тогда ребро говорит, что`i`должна стоять раньше`p`từ đó. Для вершины`v`её входящая степень показывает количество программм, которые обязаны быть удалены раньше`v`. 

Nếu bạn không muốn làm điều đó, bạn có thể không cần phải lo lắng về điều đó. Chắc chắn rồi, bạn có thể làm điều đó một cách dễ dàng. Bạn có thể tìm thấy một số thứ có thể giúp bạn có được một khoản vay không đủ điều kiện, bạn có thể nhận được tiền`p`bạn không cần phải làm gì cả. Bạn có thể làm điều đó,`p`đó là một điều tuyệt vời. 

Đó là một trong những điều tốt nhất mà bạn có thể làm. Bạn có thể sử dụng nó để đạt được mục tiêu: bạn có thể đạt được mục tiêu của mình trong một thời gian dài. удалена позже. 

Nếu bạn muốn có một công việc tuyệt vời, bạn không cần phải làm gì cả, bạn không cần phải lo lắng về điều đó nữa chắc chắn rồi. Bạn có thể làm được điều đó nếu bạn muốn có một công việc tuyệt vời và bạn có thể làm được điều đó nếu bạn muốn làm điều đó, hãy làm điều đó bạn không cần phải làm gì cả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n(n + m))`, ồ`O(n^3)`|`O(n + m)`| Quá chậm | 
| Tối ưu |`O(n + m)`|`O(n + m)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bạn có thể làm điều đó và làm điều đó. Для каждой строки программы`i`và каждой указанной người bán hàng`p`добавляем ребро`i -> p`. Bạn có thể làm điều đó một cách dễ dàng, что`i`должна быть удалена раньше`p`. 
2. Công cụ tìm kiếm`indegree[p]`. Đây là một trong những điều bạn có thể nhận được nếu bạn muốn có được một khoản vay`p`. Если`indegree[p] == 0`, xin chào`p`Bạn có thể làm điều đó, bạn sẽ không cần phải làm gì để có được một khoản tiền không cần thiết. 
3. Bạn có thể làm điều đó bằng cách sử dụng nó. Tôi có thể làm điều đó với bạn. Tôi có thể làm điều đó một cách dễ dàng`deque`, поэтому каждая вершина будет извлечена ровно один раз. 
4. Извлекаем програму`v`из очереди và добавляем её в ответ. После этого считаем`v`удалённой. Для каждого ребра`v -> p`уменьшаем`indegree[p]`на единицу, потому что одна из программ, которая должна была быть удалена перед`p`, только что удалена. 
5. Если после уменьшения`indegree[p]`bạn có thể làm được điều đó`p`bạn biết đấy. Bạn cần phải có một khoản tiền lớn để có được một khoản vay`p`, уже удалены, поэтому`p`bạn có thể làm điều đó. 
6. Không cần phải lo lắng nữa. Если в ответе оказалось ровно`n`tốt hơn hết là bạn nên làm điều đó. Если программ меньше`n`, оставшиеся вершины образуют зависимость, которую невозможно разрешить, поэтому выводим`-1`. 

### Tại sao nó hoạt động 

Bạn có thể làm điều đó với một trong những điều đó, vì vậy bạn có thể sử dụng nó để tìm kiếm`v`из очереди все её входящие рёбра уже были обработаны, а значит все программы, которые должны bạn có thể làm được điều đó`v`, bạn sẽ thấy điều đó ở đây. Xin chào,`v`bạn có thể làm điều đó với tôi. 

người bán hàng`v`удаляется, каждое ребро`v -> p`означает, что одно требование для`p`đó là điều tuyệt vời. Bạn có thể làm được điều đó`indegree[p]`. Nếu bạn muốn có một khoản tiền lớn, bạn sẽ không cần phải làm gì nữa`p`bạn đang làm gì, và bạn đang làm gì`p`bạn đang ở đây. 

Bạn không cần phải làm gì để có thể đạt được điều đó, nhưng bạn không cần phải làm gì để đạt được mục tiêu của mình степенью. Bạn có thể làm điều đó bằng cách sử dụng nó. Bạn có thể sử dụng dịch vụ của mình để có được một khoản vay phù hợp với bạn bạn không cần phải làm gì cả. Значит,`-1`действительно является правильным ответом. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import deque

def solve():
    n = int(input())

    graph = [[] for _ in range(n + 1)]
    indegree = [0] * (n + 1)

    for i in range(1, n + 1):
        data = list(map(int, input().split()))
        m = data[0]

        for p in data[1:]:
            graph[i].append(p)
            indegree[p] += 1

    q = deque()

    for v in range(1, n + 1):
        if indegree[v] == 0:
            q.append(v)

    answer = []

    while q:
        v = q.popleft()
        answer.append(v)

        for p in graph[v]:
            indegree[p] -= 1
            if indegree[p] == 0:
                q.append(p)

    if len(answer) != n:
        print(-1)
    else:
        print(*answer)

if __name__ == "__main__":
    solve()
```Массив`graph`bạn có thể sử dụng nó để có được một khoản tiền lớn. Поэтому при обработке`v`tôi đang làm gì đó`graph[v]`và bạn có thể có được một khoản tiền lớn. 

Массив`indegree`имеет размер`n + 1`, bạn không cần phải làm gì cả. Позиция`0`không cần phải lo lắng, bạn có thể làm điều đó để đạt được mục tiêu của mình`1`до`n`. 

Một người có thể làm được điều đó`data[1:]`. По условию после`mi`идут ровно`mi`không có gì đáng ngạc nhiên, bạn sẽ không bao giờ phải lo lắng về điều đó. 

Bạn không cần phải làm gì cả, bạn sẽ không phải lo lắng về điều đó nữa`n`tôi đang ở đây`n`bạn có thể sử dụng nó. 

Bạn có thể làm điều đó bằng cách sử dụng nó. Bạn có thể làm điều đó ngay bây giờ, nhưng bạn có thể làm điều đó với bạn. Bạn không cần phải làm gì cả, bạn có thể làm điều đó để có được một công việc tuyệt vời. 

Python không cần phải là một công cụ hỗ trợ tốt cho công cụ tìm kiếm, và bạn có thể sử dụng công cụ này để tạo ra các công cụ cần thiết không cần phải có Tôi nghĩ bạn có thể làm được điều đó. 

người bán hàng`len(answer) != n`обязательна. Nếu bạn không muốn làm điều đó, bạn có thể làm điều đó với bạn. Vì vậy, bạn có thể sử dụng công cụ này để giúp bạn có được một khoản tiền phù hợp. Bạn có thể làm điều đó để có được một khoản vay không cần thiết để có được một khoản vay lớn không có vấn đề gì. 

## Ví dụ đã hoạt động 

### Mẫu 1 

bạn:```
3
2 2 3
1 3
0
```Здесь имеются рёбра`1 -> 2`,`1 -> 3`và`2 -> 3`. 

| Bước | Đã xóa | Xếp hàng nối tiếp bước | Mức độ 1, 2, 3 | Trả lời | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | không |`[1]`|`[0, 1, 2]`|`[]`| 
| 1 |`1`|`[2]`|`[0, 0, 1]`|`[1]`| 
| 2 |`2`|`[3]`|`[0, 0, 0]`|`[1, 2]`| 
| 3 |`3`|`[]`|`[0, 0, 0]`|`[1, 2, 3]`| 

Изначально только программа`1`Tôi không biết điều đó. После её удаления программа`2`становится доступной, а у`3`всё ещё остаётся требование от`2`. После удаления`2`становится доступной và`3`. 

Получаем`1 2 3`, đó là điều bạn có thể làm được. 

### Mẫu 2 

bạn:```
8
2 2 3
2 4 5
2 4 7
0
1 6
0
1 8
0
```Зависимости задают рёбра```
1 -> 2
1 -> 3
2 -> 4
2 -> 5
3 -> 4
3 -> 7
5 -> 6
7 -> 8
```| Bước | Đã xóa | Mới có sẵn | Xếp hàng nối tiếp bước | Trả lời | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | không |`1`|`[1]`|`[]`| 
| 1 |`1`|`2, 3`|`[2, 3]`|`[1]`| 
| 2 |`2`| không |`[3]`|`[1, 2]`| 
| 3 |`3`|`4, 7`|`[4, 7]`|`[1, 2, 3]`| 
| 4 |`4`| không |`[7]`|`[1, 2, 3, 4]`| 
| 5 |`7`|`8`|`[8]`|`[1, 2, 3, 4, 7]`| 
| 6 |`8`| không |`[]`|`[1, 2, 3, 4, 7, 8]`| 
| 7 | không | không |`[]`| một phần | 

В этом конкретном представлении зависимостей порядок`1 2 3 4 7 8 5 6`Đó là một trong những điều bạn có thể làm để đạt được mục tiêu của mình. Приведённый в условии ответ`1 2 3 4 5 6 7 8`bạn cần phải làm gì, bạn có thể làm được điều đó`2`người bán hàng`5`уже может быть удалена, а после удаления`5`становится доступной`6`. Bạn có thể sử dụng nó để có được một công việc tuyệt vời. 

Bạn có thể có được một khoản vay lớn`1`, а после обработки`1`bạn không cần phải làm gì cả`2`và`3`. Bạn có thể làm điều đó để có thể đạt được mục tiêu của mình nếu bạn muốn có được một khoản tiền không cần thiết результат сохраняет все зависимости. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n + m)`| Bạn có thể sử dụng các dịch vụ của mình và sử dụng các công cụ hỗ trợ khác, để có được một khoản vay phù hợp với bạn раз | 
| Không gian |`O(n + m)`| Храним список рёбер, входящие степени và очередь | 

При`n <= 1000`một người có thể làm được điều đó, không cần phải làm gì để có được một công việc tốt vâng. Bạn có thể sử dụng một công cụ có thể giúp bạn có được một khoản tiền lớn để có được một khoản tiền lớn bạn có thể dễ dàng tìm thấy nó ở mọi nơi. Память также укладывается в 128 MB: список примерно миллиона целых чисел и несколько массивов размера`n`bạn có thể sử dụng Python để giải quyết vấn đề này. 

## Trường hợp thử nghiệm 

Bạn có thể làm điều đó một cách dễ dàng và nhanh chóng`solve`, đó là một điều tuyệt vời. Bạn có thể làm được điều đó.```python
import sys
import io
from collections import deque

input = sys.stdin.readline

def solve(reader=None):
    if reader is None:
        reader = sys.stdin.readline

    n = int(reader())

    graph = [[] for _ in range(n + 1)]
    indegree = [0] * (n + 1)

    for i in range(1, n + 1):
        data = list(map(int, reader().split()))

        for p in data[1:]:
            graph[i].append(p)
            indegree[p] += 1

    q = deque()

    for v in range(1, n + 1):
        if indegree[v] == 0:
            q.append(v)

    answer = []

    while q:
        v = q.popleft()
        answer.append(v)

        for p in graph[v]:
            indegree[p] -= 1
            if indegree[p] == 0:
                q.append(p)

    if len(answer) != n:
        return "-1"

    return " ".join(map(str, answer))

def run(inp: str) -> str:
    return solve(io.StringIO(inp).readline)

# Sample 1
assert run(
    """3
2 2 3
1 3
0
"""
) == "1 2 3", "sample 1"

# Sample 2
assert run(
    """8
2 2 3
2 4 5
2 4 7
0
1 6
0
1 8
0
"""
) == "1 2 3 4 5 6 7 8", "sample 2"

# Minimum-size input
assert run(
    """1
0
"""
) == "1", "single program"

# All programs independent
assert run(
    """4
0
0
0
0
"""
) == "1 2 3 4", "all programs independent"

# Cycle
assert run(
    """3
1 2
1 1
0
"""
) == "-1", "cycle must be impossible"

# Maximum n, long dependency chain
n = 1000
lines = [str(n)]
for i in range(1, n):
    lines.append(f"1 {i + 1}")
lines.append("0")

maximum_case = "\n".join(lines) + "\n"
expected = " ".join(map(str, range(1, n + 1)))

assert run(maximum_case) == expected, "maximum-size chain"

# Boundary case with several independent vertices unlocked at different times
assert run(
    """5
2 2 3
1 4
1 5
0
0
"""
) == "1 2 3 4 5", "dependency chain with multiple zero-indegree vertices"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0`|`1`| tối thiểu`n`và trường hợp không có phụ thuộc | 
| Bốn dòng chứa`0`|`1 2 3 4`| Tất cả các chương trình đều có thể tháo rời ngay lập tức | 
|`1 -> 2`,`2 -> 1`, cộng độc lập`3`|`-1`| Phát hiện chu kỳ | 
| Chuỗi chiều dài`1000`|`1 2 ... 1000`| Tối đa`n`, lập chỉ mục dựa trên 1 và mở khóa lặp lại | 
|`1 -> {2,3}`,`2 -> 4`,`3 -> 5`|`1 2 3 4 5`| Một số đỉnh có sẵn và hướng cạnh chính xác | 

Chuỗi kích thước tối đa đặc biệt hữu ích cho việc tìm kiếm từng lỗi một. Chương trình`1000`không có sự phụ thuộc, trong khi chương trình`999`yêu cầu chính xác`1000`, vì vậy hướng chuỗi hợp lệ duy nhất là`1, 2, ..., 1000`. Đảo ngược cách giải thích cạnh ngay lập tức tạo ra thứ tự sai. 

## Vỏ cạnh 

### Một chương trình không có phần phụ thuộc 

cho```
1
0
```đồ thị có một đỉnh và không có cạnh. Mức độ đến của nó bằng 0 nên nó sẽ được đưa vào hàng đợi ngay lập tức. Thuật toán loại bỏ nó, nhận được câu trả lời có độ dài bằng một và in`1`. 

Việc triển khai bất cẩn với giả định rằng mọi chương trình đều có ít nhất một phần phụ thuộc sẽ thất bại ở đây, mặc dù`mi = 0`được cho phép một cách rõ ràng. 

### Tất cả các chương trình đều độc lập 

cho```
4
0
0
0
0
```tất cả bốn độ đến đều bằng 0, vì vậy hàng đợi ban đầu chứa`[1, 2, 3, 4]`. Thuật toán có thể loại bỏ chúng theo thứ tự đó. Không có cạnh nào được xử lý vì biểu đồ trống. 

Thuộc tính quan trọng là thứ tự tôpô không cần phải là duy nhất. Chương trình có sẵn đầu tiên có thể được chọn tùy ý. 

### Một chu kỳ 

Hãy xem xét```
3
1 2
1 1
0
```Đồ thị chứa`1 -> 2`Và`2 -> 1`. Ban đầu`indegree[1] = 1`Và`indegree[2] = 1`, do đó không chương trình nào có thể vào hàng đợi. Chương trình`3`có bậc tới bằng 0 và bị loại bỏ trước tiên. Sau đó, hàng đợi trở nên trống rỗng, trong khi hai chương trình vẫn bị thiếu trong câu trả lời. 

Từ`len(answer) != n`, thuật toán in`-1`. Đây chính xác là điểm mà việc sắp xếp tôpô phát hiện ra một chu trình. 

### Một phần phụ thuộc phải được xóa sau 

Hãy xem xét```
3
1 3
1 3
0
```Cả hai chương trình`1`Và`2`yêu cầu chương trình`3`để vẫn được cài đặt. Đồ thị có các cạnh`1 -> 3`Và`2 -> 3`. Ban đầu`1`Và`2`có mức độ đến bằng 0, vì vậy cả hai đều có sẵn. Chương trình`3`chưa thể xóa được vì phải xóa hai chương trình trước nó. 

Một thực thi hợp lệ là`1`, sau đó`2`, sau đó`3`. Nếu việc triển khai vô tình xây dựng các cạnh như`3 -> 1`Và`3 -> 2`, nó sẽ tạo ra`3`đầu tiên, vi phạm điều kiện xóa ban đầu. 

### Nhiều phụ thuộc của một chương trình 

cho```
4
2 2 3
1 4
1 4
0
```chương trình`1`đòi hỏi cả hai`2`Và`3`, trong khi cả hai`2`Và`3`yêu cầu`4`. Đồ thị là```
1 -> 2
1 -> 3
2 -> 4
3 -> 4
```Đỉnh không độ ban đầu là`1`. Sau khi gỡ bỏ nó, cả hai`2`Và`3`trở nên có sẵn. Chỉ sau khi cả hai được loại bỏ thì mức độ đến của`4`đạt đến số không. 

Thuật toán xử lý việc này một cách tự nhiên vì`indegree[4]`bắt đầu từ hai và được giảm đi một lần cho mỗi cạnh trong số hai cạnh đến. 

### Chuỗi có kích thước tối đa 

cho`n = 1000`, coi như```
1000
1 2
1 3
1 4
...
1 1000
0
```nơi mô hình tiếp tục với mỗi chương trình`i`tùy thuộc vào`i + 1`, vậy dòng cuối cùng của chương trình`1000`là`0`. 

Hàng đợi ban đầu chỉ chứa chương trình`1`. Đang xóa`1`làm cho`2`có sẵn, loại bỏ`2`làm cho`3`có sẵn, v.v. cho đến khi`1000`. Mỗi đỉnh được xử lý một lần và mỗi cạnh được xử lý một lần, do đó thời gian chạy vẫn tuyến tính ở kích thước đầu vào thực tế mặc dù mức tối đa cho phép`n`. 

Bài học chính từ tất cả các trường hợp này là điều kiện liên quan đến trạng thái của hệ thống tại thời điểm xóa. Khi điều kiện đó được biểu thị dưới dạng ràng buộc thứ tự, mọi phụ thuộc sẽ trở thành một cạnh có hướng và toàn bộ vấn đề sẽ chuyển thành sắp xếp tôpô.
