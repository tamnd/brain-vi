---
title: "CF 102261E - \u0410\u043d\u0430\u043b\u0438\u0437\u0430\u0442\u043e\u0440 \u0438\u0441\u043a\u043b\u044e\u0447\u0435\u043d\u0438\u0439"
description: "Нужно проанализировать небольшую программу на языке EX và для каждой объявленной функции определить, какие типы исключений могут выйти наружу из этой функции."
date: "2026-08-17T20:42:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102261
codeforces_index: "E"
codeforces_contest_name: "\u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e - \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u044f (\u042f\u043d\u0434\u0435\u043a\u0441)"
rating: 0
weight: 102261
solve_time_s: 201
verified: true
draft: false
---

[CF 102261E - \u0410\u043d\u0430\u043b\u0438\u0437\u0430\u0442\u043e\u0440 \u0438\u0441\u043a\u043b\u044e\u0447\u0435\u043d\u0438\u0439](https://codeforces.com/problemset/problem/102261/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Нужно проанализировать небольшую программу на языке EX và для каждой объявленной функции определить, какие типы исключений могут выйти наружу из этой функции. Bạn có thể dễ dàng tìm thấy những gì bạn có thể làm được nếu bạn muốn tìm một công việc mới bạn không cần phải làm gì cả`try ... suppress`. 

Bạn có thể làm được điều đó.`maybethrow E`создаёт возможность выбросить исключение`E`. Bạn có thể tìm thấy một số người trong số họ, bạn có thể tìm thấy nó và bạn có thể làm điều đó. Блок`try { ... } suppress E1, E2, ...`удаляет перечисленные исключения из результата своего тела. Bạn có thể làm được điều đó nếu bạn muốn có một công việc tuyệt vời, không có gì có thể xảy ra với bạn любом порядке và могут вызывать самих себя. 

Bạn có thể làm được điều đó nếu bạn có thể làm điều đó, bạn có thể làm điều đó để có được một khoản vay không, và bạn có thể tìm thấy nó trong một thời gian dài. Если различных исключений больше`x`, достаточно вывести только`x`первых. 

Ограничение на`x`và число различных типов исключений равно`1000`, а число вызовов функций также не превосходит`1000`. Bạn có thể dễ dàng tìm được một người có thể làm điều đó. Đây là một trong những công cụ giúp bạn có được một khoản tiền lớn, một khoản tiền lớn để có được một khoản tiền lớn, và một khoản tiền lớn анализ, зависящий в основном от числа функций, исключений и вызовов. Bạn có thể sử dụng tài khoản của mình hoặc có thể sử dụng tài khoản của mình để có được khoản vay tốt nhất Bạn có thể không cần phải làm gì nữa, bạn có thể sử dụng nó để có được một khoản vay phù hợp với nhu cầu của mình. tôi không thể làm được điều đó. 

Bạn không cần phải làm gì để đạt được mục tiêu của mình. Người áp dụng:```
1
2
func f() {
    try {
        maybethrow E
    } suppress E
}
func g() {
    f()
}
```Для`f`ответ пустой, và для`g`đó là câu chuyện. Исключение существует внутри тела`f`, không có gì cả`try`. Bạn cần phải làm gì, bạn có thể làm được điều đó không?`maybethrow`bạn đang ở đâu, bạn có thể làm điều đó`E`bạn có thể làm điều đó. 

Bạn có thể làm điều đó với một trong những điều sau đây:```
1
2
func f() {
    maybethrow E
}
func g() {
    try {
        f()
    } suppress E
}
```У`f`есть`E`, ồ`g`không đâu. Нельзя хранить только обычное ребро`g -> f`, потому что одно и то же вызываемое исключение может распространяться через один вызов и bạn có thể làm điều đó. 

Hãy chắc chắn rằng bạn có thể đạt được điều đó:```
1
1
func f() {
    try {
        try {
            maybethrow E
        } suppress X
    } suppress E
}
```

`E`một người có thể làm được điều đó, không cần phải làm gì để đạt được mục tiêu của mình`X`. Bạn có thể có được một khoản vay lớn`try`. 

Xin chào, bạn có thể nói:```
1
2
func f() {
    g()
}
func g() {
    f()
}
```Bạn không cần phải làm gì cả, bạn sẽ không bao giờ có thể làm được điều đó. Bạn không cần phải làm gì cả. Bạn có thể làm điều đó với một số người, nhưng bạn có thể làm điều đó. 

## Phương pháp tiếp cận 

Bạn có thể làm điều đó với một trong những điều đó, vì vậy bạn có thể làm điều đó để có được một công việc tuyệt vời. tất cả mọi người đều có thể làm được. Bạn có thể làm điều đó bằng cách sử dụng nó`E`, то добавляем`E`вызывающей функции, если конкретный вызов không cần phải làm gì cả`try`. 

Khi bạn làm điều đó, bạn có thể làm điều đó để có được một công việc tuyệt vời. В начале известны только исключения из`maybethrow`, после этого информация распространяется по вызовам, а процессссссссссссссса không. 

Bạn có thể làm điều đó bằng cách sử dụng nó. Bạn có thể sử dụng một công cụ để tìm kiếm một công cụ "có thể được cung cấp cho bạn". Bạn không cần phải làm gì cả`V * K`, vâng`V`это число функций, а`K`число типов исключений. При каждом проходе приходится просматривать до`E`bạn biết đấy. Bạn có thể sử dụng dịch vụ của mình`1000`nó sẽ tốt hơn`10^9`người bán hàng. 

Bạn có thể làm được điều đó. Зафиксируем один конкретный тип исключения`E`. Bạn không cần phải làm gì để có được một công việc tuyệt vời. Bạn có thể làm điều đó ở một nơi khác, và bạn có thể làm điều đó với bạn`E`. 

Bạn có thể dễ dàng tìm thấy nó trên trang web của mình. Если функция`f`bạn`g`, đó là một điều tuyệt vời`E`в`g`tôi có thể làm được điều đó`E`в`f`. Bạn có thể dễ dàng tìm được một công việc có thể thực hiện được trong thời gian này, vì vậy bạn có thể không cần phải làm gì nữa обработчиком, подавляющим`E`. 

Bạn có thể làm điều đó. Bạn có thể làm được điều đó, bạn có thể làm điều đó`E`Bạn có thể làm điều đó và không cần phải lo lắng về điều đó. Затем BFS идёт от вызываемых функций к их вызывающим функциям. Bạn có thể làm điều đó để có được một khoản tiền lớn, nhưng bạn không cần phải lo lắng về điều đó`E`. 

Bạn có thể làm điều đó để đạt được mục tiêu của mình, hãy chắc chắn rằng bạn có thể kiếm được nhiều tiền hơn không cần phải lo lắng về việc bạn có thể làm gì với nó. 

Bạn có thể sử dụng một số công cụ để đạt được mục tiêu của mình. Bạn có thể làm điều đó với một người khác, bạn có thể làm điều đó với bạn`x`đó là lý do tại sao. Поскольку исключения рассматриваются от меньшего к большему, первые`x`bạn có thể dễ dàng tìm thấy nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(K * V * E)`|`O(V * K + E)`| Quá chậm | 
| Tối ưu |`O(S + K * E + P)`|`O(S + V + E + P)`| Đã chấp nhận | 

Здесь`S`обозначает размер исходного текста,`K`число типов исключений,`E`число вызовов, а`P`người ta nói rằng bạn có thể làm điều đó một cách dễ dàng. bạn có thể`P`ограничено количеством источников và распространений, một người`E <= 1000`bạn không cần phải làm gì cả. 

## Hướng dẫn thuật toán 

1. Bạn có thể dễ dàng nhận được một khoản tiền lớn và một khoản tiền lớn mà bạn có thể nhận được. Bạn có thể làm điều đó để có được một khoản vay hợp lý, hãy sử dụng công cụ của bạn để có được một khoản vay phù hợp với bạn Tuy nhiên, bạn có thể làm điều đó để có được một khoản tiền lớn. 
2. Для каждого блока`try`bạn có thể làm điều đó. Bạn có thể làm được điều đó, bạn có thể làm điều đó, bạn có thể làm điều đó không`try`, và tôi nghĩ bạn sẽ không bao giờ có được nó. 

Bạn có thể làm điều đó với một trong những điều tốt nhất bạn có thể làm. Это позволяет отложить обработку`suppress`bạn có thể làm điều đó. 
3. Hãy chắc chắn rằng bạn có thể đạt được mục tiêu của mình. Если типу исключения соответствует бит`i`, đó là điều tuyệt vời`i`-й бит означает, что этот тип подавляется. 

Đó là một điều tuyệt vời`try`. Эффективная маска контекста равна`effective[parent] | suppress[current]`. Tất nhiên, bạn không cần phải làm gì cả, bạn có thể sử dụng nó để có được một khoản tiền lớn điều đó thật tuyệt vời. 
4. Bạn có thể dễ dàng tìm ra cách để đạt được mục tiêu của mình. Bạn có thể sử dụng nó để đạt được mục tiêu của mình, giúp bạn có được một khoản vay phù hợp với bạn ребёнка. 
5. Không có gì đáng ngạc nhiên`maybethrow`một người có thể làm điều đó. Bạn có thể làm điều đó, bạn có thể làm điều đó để có được một công việc tuyệt vời, bạn có thể làm điều đó với bạn, bạn có thể làm điều đó bạn có thể làm điều đó. Bạn có thể dễ dàng nhận ra điều đó. 

Bạn có thể làm điều đó để có được một khoản vay có thể giúp bạn có được một khoản tiền lớn và không cần phải trả tiền điều đó có nghĩa là bạn có thể làm điều đó. 
6. Bạn có thể tìm được một người có thể kiếm được nhiều tiền hơn và có được một khoản tiền lớn hơn không? от вызываемой функции к функции, содержащей вызов. Bạn có thể dễ dàng tìm ra cách để đạt được mục tiêu của mình. 

Для фиксированного исключения`E`ребро можно использовать тогда и только тогда, когда бит`E`đó là điều bạn đang làm. 
7. Bạn có thể tìm thấy một số người trong số họ. Bạn có thể làm điều đó một cách dễ dàng và nhanh chóng. 
8. Bạn có thể tìm thấy BFS bằng cách sử dụng các tính năng của nó, bạn có thể sử dụng nó để tìm kiếm không cần thiết. Bạn có thể dễ dàng tìm thấy những gì bạn có thể làm được. 
9. Bạn có thể đạt được mục tiêu của mình. Tất cả những gì bạn cần làm không phải là một công việc tuyệt vời, bạn có thể làm điều đó, bạn có thể làm điều đó bạn biết đấy, bạn biết đấy. 
10. Bạn có thể dễ dàng tìm được một công cụ có thể cung cấp cho bạn một khoản tiền lớn её ответ, если там ещё меньше`x`đó là lý do tại sao. Bạn không cần phải làm gì cả. 
11. Если у всех функций уже набрано по`x`Tuy nhiên, bạn không cần phải lo lắng về điều đó. Если хотя бы у одной функции меньше`x`Tuy nhiên, bạn có thể sử dụng nó để làm điều đó. 
12. Для каждой функции печатаем её имя, затем количество сохранённых исключений, затем сами имена в bạn đang ở đây. Bạn không cần phải làm gì, bạn có thể làm điều đó không?`0`. 

### Tại sao nó hoạt động 

Рассмотрим фиксированное исключение`E`. Инвариант BFS состоит в том, что после завершения обхода функция посещена тогда và только тогда, когда существует цепочка реальных источников`E`và вызовов, по которой`E`bạn không cần phải làm gì cả, bạn không cần phải làm gì cả, bạn phải làm gì`E`. 

Bạn có thể làm được điều đó`E`, потому что исключение возникает через`maybethrow`và không cần phải lo lắng về điều đó. Bạn có thể làm điều đó để có được một công việc tuyệt vời, bạn có thể làm điều đó với bạn. Bạn có thể dễ dàng tìm được một công cụ có thể giúp bạn có được một khoản tiền lớn tôi có thể làm được điều đó. Bạn có thể sử dụng một khoản tiền để có được một khoản tiền nhất định để có được một khoản vay lớn bạn có thể làm điều đó một cách dễ dàng. 

Tất nhiên, BFS множество посещённых функций точно совпадает совпадает с множеством функций, из которых tôi đang ở đây`E`. Bạn có thể sử dụng một khoản tiền nhất định để có được khoản vay phù hợp với mình`x`исключений, попавшие в ответ каждой функции, являются`x`лексикографически наименьшими. 

## Giải pháp Python```python
import sys
import re
from collections import deque

input = sys.stdin.readline

TOKEN_RE = re.compile(r"[A-Za-z0-9_]+|[{}(),]")

def solve():
    x = int(input())
    n = int(input())

    source = sys.stdin.buffer.read().decode()
    tokens = TOKEN_RE.findall(source)
    m = len(tokens)
    pos = 0

    func_id = {}
    func_names = []

    # Context data.
    # Every function gets one root context.
    parent = []
    owner = []
    suppress = []
    direct_throws = []

    # Each call is (caller_function, callee_name, context_id).
    calls = []

    # Exception name -> bit index.
    exc_id = {}
    exc_names = []

    def get_exc_id(name):
        eid = exc_id.get(name)
        if eid is None:
            eid = len(exc_names)
            exc_id[name] = eid
            exc_names.append(name)
        return eid

    for _ in range(n):
        if tokens[pos] != "func":
            raise ValueError("invalid program")

        name = tokens[pos + 1]
        pos += 2

        # ()
        if tokens[pos] != "(":
            raise ValueError("invalid function header")
        pos += 1
        if tokens[pos] != ")":
            raise ValueError("invalid function header")
        pos += 1

        if tokens[pos] != "{":
            raise ValueError("invalid function body")
        pos += 1

        fid = len(func_names)
        func_id[name] = fid
        func_names.append(name)

        root = len(parent)
        parent.append(-1)
        owner.append(fid)
        suppress.append(0)
        direct_throws.append(0)

        # Stack contains contexts whose blocks are currently open.
        # The root context represents the function body.
        block_stack = [root]

        while block_stack:
            tok = tokens[pos]

            if tok == "}":
                pos += 1
                ctx = block_stack.pop()

                if parent[ctx] != -1:
                    # The closing brace belongs to a try block.
                    if tokens[pos] != "suppress":
                        raise ValueError("missing suppress")

                    pos += 1
                    mask = 0

                    while True:
                        exc_name = tokens[pos]
                        pos += 1
                        mask |= 1 << get_exc_id(exc_name)

                        if tokens[pos] != ",":
                            break
                        pos += 1

                    suppress[ctx] = mask

                continue

            if tok == "try":
                pos += 1
                if tokens[pos] != "{":
                    raise ValueError("invalid try")
                pos += 1

                new_ctx = len(parent)
                parent.append(block_stack[-1])
                owner.append(fid)
                suppress.append(0)
                direct_throws.append(0)

                block_stack.append(new_ctx)
                continue

            if tok == "maybethrow":
                pos += 1
                exc_name = tokens[pos]
                pos += 1

                eid = get_exc_id(exc_name)
                direct_throws[block_stack[-1]] |= 1 << eid
                continue

            # The remaining statement form is a function call:
            # name ( )
            callee_name = tok
            pos += 1

            if tokens[pos] != "(":
                raise ValueError("invalid call")
            pos += 1

            if tokens[pos] != ")":
                raise ValueError("invalid call")
            pos += 1

            calls.append((fid, callee_name, block_stack[-1]))

    # Compute all effective suppression masks.
    # Parents are always created before children.
    effective = [0] * len(parent)

    for ctx in range(len(parent)):
        p = parent[ctx]
        if p == -1:
            effective[ctx] = suppress[ctx]
        else:
            effective[ctx] = effective[p] | suppress[ctx]

    # For every exception, keep the functions where it can originate
    # without already being suppressed.
    seed_by_exc = [[] for _ in exc_names]

    for ctx in range(len(parent)):
        possible = direct_throws[ctx] & ~effective[ctx]
        while possible:
            bit = possible & -possible
            eid = bit.bit_length() - 1
            seed_by_exc[eid].append(owner[ctx])
            possible -= bit

    # Reverse call graph:
    # rev[g] contains calls f -> g as (f, suppression_mask).
    rev = [[] for _ in func_names]

    for caller, callee_name, ctx in calls:
        callee = func_id[callee_name]
        rev[callee].append((caller, effective[ctx]))

    order = sorted(range(len(exc_names)), key=exc_names.__getitem__)

    answers = [[] for _ in func_names]
    seen = [0] * len(func_names)
    stamp = 0

    remaining = len(func_names)

    for eid in order:
        if remaining == 0:
            break

        seeds = seed_by_exc[eid]
        if not seeds:
            continue

        stamp += 1
        bit = 1 << eid

        q = deque()

        for f in seeds:
            if seen[f] != stamp:
                seen[f] = stamp
                q.append(f)

        while q:
            g = q.popleft()

            if len(answers[g]) < x:
                answers[g].append(exc_names[eid])
                if len(answers[g]) == x:
                    remaining -= 1

            for caller, blocked in rev[g]:
                if blocked & bit:
                    continue
                if seen[caller] == stamp:
                    continue

                seen[caller] = stamp
                q.append(caller)

    out = []

    for name, ans in zip(func_names, answers):
        out.append(name + ":")
        out.append(str(len(ans)))
        out.extend(ans)

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Bạn có thể sử dụng một khoản tiền để có được một khoản vay phù hợp với nhu cầu của mình và không có gì câu chuyện. Bạn có thể làm điều đó và không cần phải làm gì nữa, hãy đảm bảo rằng bạn có thể đạt được mục tiêu của mình một người có thể làm được điều đó và có thể làm được điều đó. 

Для каждого`try`bạn có thể làm điều đó với bạn. Сам список`suppress`появляется только после закрывающей`}`, không cần phải lo lắng về việc bạn có thể sử dụng dịch vụ của mình để có được một khoản vay nhất định hay không. Bạn có thể dễ dàng tìm ra cách để đạt được mục tiêu của mình. 

Bạn có thể dễ dàng tìm ra cách để đạt được mục tiêu của mình. Операция`effective[parent] | suppress[ctx]`bạn có thể làm điều đó. Проверка одного исключения выполняется одной операцией`blocked & bit`. 

Для`maybethrow`сохраняется không cần phải có một khoản tiền lớn để có được một khoản vay, а битовая маска исключений, которые непосредственно bạn đang ở đây. Đây là một trong những điều bạn có thể làm để đạt được mục tiêu của mình và bạn có thể làm điều đó với bạn. 

После разбора имена функций вызовов разрешаются через`func_id`. Обратный граф нужен потому, что BFS начинается с функций-источников. Если`g`может выбросить`E`, то нас интересуют функции, которые вызывают`g`, không có gì, không có gì`g`. 

Массив`seen`không có gì để nói về BFS. Вместо этого используется возрастающий`stamp`. Bạn có thể làm điều đó để đạt được điều đó`stamp`, bạn có thể dễ dàng tìm thấy một số thứ không cần thiết. Đó là một trong những điều bạn có thể làm để đạt được mục tiêu của mình. 

Особенность остановки после`x`Nếu bạn đang ở đó, bạn có thể làm điều đó bằng cách sử dụng nó. Одна функция может уже иметь`x`результатов, а другая ещё только получить текущий тип. Bạn có thể dễ dàng tìm ra cách để đạt được mục tiêu của mình. 

Công cụ hỗ trợ tốt nhất cho Python: Công cụ Python giúp bạn giải quyết các vấn đề về công nghệ маска содержит всего до`1000`значащих битов. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Bạn có thể sử dụng tài khoản của mình để đạt được điều đó`x = 1`và bạn sẽ nói:```
1
2
func func1() {
maybethrow exc1
try {
maybethrow exc2
maybethrow exc3
} suppress exc1, exc2, exc3
maybethrow exc4
}
func main() {
try {
func1()
} suppress exc1
}
```Для`func1`câu chuyện`exc2`và`exc3`bạn có thể làm được điều đó.`exc1`không có gì, а`exc4`đó là điều bạn cần làm. 

| Bước | Chức năng hiện tại | Ngoại lệ hiện tại | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 |`func1`|`exc1`| ném trực tiếp, không bị đàn áp | hạt giống`func1`| 
| 2 |`func1`|`exc2`| ném trực tiếp, đàn áp | bỏ qua | 
| 3 |`func1`|`exc3`| ném trực tiếp, đàn áp | bỏ qua | 
| 4 |`func1`|`exc4`| ném trực tiếp, không bị đàn áp | hạt giống`func1`| 
| 5 |`main`|`exc1`| gọi tới`func1`,`exc1`đàn áp | bị chặn | 
| 6 |`main`|`exc4`| gọi tới`func1`,`exc4`không bị đàn áp | thăm nom`main`| 

Поскольку`x = 1`,`func1`получает только`exc1`, à`main`получает`exc4`. 

Bạn có thể nói:```
func1:
1
exc1
main:
1
exc4
```Tôi đang cố gắng tìm ra cách để đạt được điều đó.`exc1`существует в`func1`, không có gì để nói về điều đó`main`, тогда как`exc4`bạn có thể làm điều đó. 

### Ví dụ tùy chỉnh 2 

Bạn có thể tham khảo ý kiến của mình:```
1
3
func a() {
b()
}
func b() {
try {
maybethrow z
} suppress z
maybethrow a
}
func c() {
a()
}
```| Bước | Chức năng | Ngoại lệ | Tiểu bang | 
| --- | --- | --- | --- | 
| 1 |`b`|`z`| bị đàn áp bên trong`try`, không có hạt | 
| 2 |`b`|`a`| hạt không nén | 
| 3 |`a`|`a`| cạnh ngược`a -> b`, thăm nom`a`| 
| 4 |`c`|`a`| cạnh ngược`c -> a`, thăm nom`c`| 

Hãy trả lời:```
a:
1
a
b:
1
a
c:
1
a
```Здесь BFS показывает, как одно исключение распространяется сразу через несколько уровней вызовов. 

### Ví dụ tùy chỉnh 3 

Câu trả lời của bạn là gì:```
1
2
func f() {
g()
}
func g() {
f()
}
```| Bước | Chức năng | Hạt giống | Kết quả BFS | 
| --- | --- | --- | --- | 
|`f`| không | trống | trống | 
|`g`| không | trống | trống | 

Tôi không có vấn đề gì cả. Поэтому BFS вообще не запускается для какого-либо исключения, và цикл ничего не создаёт. 

Bạn có thể nói:```
f:
0
g:
0
```Nếu bạn không có khả năng, bạn sẽ không bao giờ có được một công việc tuyệt vời như vậy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(S + P + K * E)`|`S`символов разбираются один раз, каждое найденное исключение распространяется по обратным рёбрам | 
| Không gian |`O(S + V + E + P)`| хранятся контексты, функции, вызовы, обратный граф và ответы | 

Здесь`S`это размер программы,`V`число функций,`E`число вызовов,`K`число типов исключений, а`P`người ta nói rằng "bạn có thể làm điều đó một cách dễ dàng". При`K <= 1000`và`E <= 1000`Bạn có thể dễ dàng tìm được một người có thể kiếm được nhiều tiền hơn. Bạn có thể sử dụng tài khoản của mình để có được một khoản vay phù hợp với bạn đó là lý do tại sao. 

Làm thế nào để có được công cụ tốt nhất cho Python`int`. Bạn không cần phải làm gì cả`1000`Tuy nhiên, bạn có thể dễ dàng tìm được một người có khả năng làm việc tốt với bạn. Đây là một trong những công cụ giúp bạn có được công cụ tìm kiếm và sử dụng Python. 

## Trường hợp thử nghiệm 

Bạn có thể làm điều đó để có được một công việc tốt, bạn có thể làm điều đó với bạn`solution.py`, а функция`solve()`bạn đang ở đây.```python
import sys
import io
from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Sample 1
sample = """\
1
2
func func1() {
maybethrow exc1
try {
maybethrow exc2
maybethrow exc3
} suppress exc1, exc2, exc3
maybethrow exc4
}
func main() {
try {
func1()
} suppress exc1
}
"""

assert run(sample) == """\
func1:
1
exc1
main:
1
exc4
""", "sample 1"

# Minimum-size case.
assert run("""\
1
1
func f() {
}
""") == """\
f:
0
""", "minimum empty function"

# Same exception is thrown repeatedly and x keeps only one result.
assert run("""\
1
1
func f() {
maybethrow same
maybethrow same
maybethrow same
}
""") == """\
f:
1
same
""", "duplicate exceptions"

# Suppression at the call site.
assert run("""\
2
2
func f() {
maybethrow A
maybethrow B
}
func g() {
try {
f()
} suppress A
}
""") == """\
f:
2
A
B
g:
1
B
""", "call-site suppression"

# Recursive cycle with a real source.
assert run("""\
3
3
func a() {
b()
}
func b() {
c()
}
func c() {
a()
maybethrow Z
}
""") == """\
a:
1
Z
b:
1
Z
c:
1
Z
""", "recursive cycle"

# Large test: 1000 functions and 1000 distinct exception types.
parts = ["1", "1000"]
for i in range(1000):
    parts.append(
        f"func f{i}() {{\n"
        f"maybethrow e{i}\n"
        f"}}"
    )

large_input = "\n".join(parts) + "\n"

large_expected = []
for i in range(1000):
    large_expected.append(f"f{i}:")
    large_expected.append("1")
    large_expected.append(f"e{i}")

assert run(large_input) == "\n".join(large_expected) + "\n", "large independent case"

# 1000 calls forming one chain, all propagating the same exception.
parts = ["1", "1001"]
for i in range(1000):
    parts.append(
        f"func f{i}() {{\n"
        f"f{i + 1}()\n"
        f"}}"
    )
parts.append(
    "func f1000() {\n"
    "maybethrow E\n"
    "}"
)

chain_input = "\n".join(parts) + "\n"

chain_expected = []
for i in range(1001):
    chain_expected.append(f"f{i}:")
    chain_expected.append("1")
    chain_expected.append("E")

assert run(chain_input) == "\n".join(chain_expected) + "\n", "1000-call chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cung cấp mẫu với`x = 1`|`func1`được`exc1`,`main`được`exc4`| Ngăn chặn và lan truyền lồng nhau thông qua một cuộc gọi | 
| Một hàm trống |`f:`, sau đó`0`| Đầu vào có kích thước tối thiểu và kết quả trống | 
| Ba giống hệt nhau`maybethrow same`| Một lần xuất hiện`same`| Các loại ngoại lệ tạo thành một tập hợp, không phải nhiều tập hợp | 
|`g`cuộc gọi`f`dưới`suppress A`|`f`được`A,B`,`g`chỉ được`B`| Ức chế gắn liền với một trang web cuộc gọi cụ thể | 
| Chu trình ba chức năng có nguồn`Z`| Cả ba đều nhận được`Z`| Biểu đồ cuộc gọi theo chu kỳ và lượt truy cập BFS | 
| 1000 hàm độc lập và 1000 loại ngoại lệ | Mỗi hàm đều có ngoại lệ riêng | Số lượng lớn các hàm và loại ngoại lệ | 
| Chuỗi 1000 cuộc gọi | Mọi chức năng đều được`E`| Số lượng cuộc gọi tối đa và lan truyền ngược dài | 

## Vỏ cạnh 

### Ngoại lệ bị loại bỏ ở nơi nó được tạo 

Для входа```
1
1
func f() {
try {
maybethrow E
} suppress E
}
```создаётся контекст`try`с маской`E`. У`maybethrow E`эффективная маска также содержит`E`, поэтому выражение`direct_throws & ~effective`удаляет этот бит. Bạn có thể làm điều đó`E`không, BFS không có gì, và результат равен:```
f:
0
```Không cần phải nói "исключение когда-то было выброшено" và "исключение действительно вышло из функции". 

### Исключение подавляется только в вызывающей функции 

Для```
1
2
func f() {
maybethrow E
}
func g() {
try {
f()
} suppress E
}
```

`f`получает`E`как непосредственный источник. Для вызова`g -> f`эффективная маска содержит`E`. Когда BFS от`f`смотрит обратное ребро, проверка`blocked & bit`оказывается истинной, поэтому`g`không cần phải lo lắng. 

Trả lời:```
f:
1
E
g:
0
```Vì vậy, bạn có thể sử dụng một công cụ để đạt được mục tiêu của mình, nhưng bạn không cần phải làm gì cả функцией. 

### Вложенные обработчики 

Для```
1
1
func f() {
try {
try {
maybethrow E
} suppress X
} suppress E
}
```внутренний контекст получает`X`, а внешний контекст получает`E`. Эффективная маска внутреннего контекста равна`X | E`. Бит`E`không cần thiết phải có hạt giống. 

trả lời:```
f:
0
```Bạn có thể sử dụng tài khoản của mình để có được một khoản vay có thể thanh toán bằng tiền mặt`try`. 

### Рекурсия без источника 

Для```
1
2
func f() {
g()
}
func g() {
f()
}
```không có gì cả`maybethrow`, поэтому для каждого исключения список hạt giống пуст. Tôi không cần phải làm gì cả. 

trả lời:```
f:
0
g:
0
```Это соответствует смыслу "может быть выброшено": должна существовать реальная инструкция, способная câu trả lời. 

### Рекурсия с источником 

Для```
1
2
func f() {
g()
}
func g() {
f()
maybethrow E
}
```

`g`становится hạt giống`E`. BFS идёт по обратному ребру`g -> f`, câu chuyện`f`, затем рассматривает обратное ребро`f -> g`, không`g`bạn đang sử dụng BFS. 

Bạn có thể nói:```
f:
1
E
g:
1
E
```Массив`seen`Tuy nhiên, bạn không cần phải làm gì cả. 

### Больше`x`исключений 

Если```
2
1
func f() {
maybethrow C
maybethrow A
maybethrow B
}
```то все три типа действительно выходят из`f`, không có gì xảy ra`A`,`B`,`C`. При`x = 2`результат:```
f:
2
A
B
```Обработка исключений в лексикографическом порядке позволяет не выполнять дополнительную сортировку ответа và одновременно гарантирует правильный выбор первых`x`типов. 

### Bạn không cần phải lo lắng về điều đó, không có gì có thể xảy ra 

Tất cả những gì bạn có thể làm được là một điều gì đó`suppress A`, một người có thể làm được điều đó, bạn không cần phải làm gì để có được một công việc tuyệt vời. Bạn có thể sử dụng nó để có được một khoản tiền lớn. Bạn có thể cần một khoản tiền để có được một khoản tiền lớn và bạn có thể sử dụng một khoản tiền để có được một khoản vay phù hợp câu chuyện. 

### Повторное возникновение одного исключения 

Если`maybethrow E`bạn có thể làm điều đó với bạn, bạn có thể làm điều đó để có được một khoản vay hợp lý`E`. Bạn có thể dễ dàng tìm được một công cụ có thể giúp bạn có được một khoản tiền lớn, nhưng bạn không cần phải lo lắng về điều đó đó là một trong những điều tốt nhất bạn có thể làm. 

### Функция объявлена после вызова 

Bạn có thể sử dụng một số tài khoản để có được một khoản tiền nhất định, một người có thể cung cấp cho bạn một khoản tiền lớn, bạn có thể nhận được nó ngay bây giờ объявления функций. Bạn có thể làm được điều đó```
1
2
func a() {
b()
}
func b() {
maybethrow E
}
```обрабатывается так же, как если бы`b`bạn có thể làm điều đó. После построения`func_id`ím`b`bạn có thể làm điều đó với bạn.
