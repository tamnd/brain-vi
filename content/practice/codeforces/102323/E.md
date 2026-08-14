---
title: "CF 102323E - Chuỗi Email"
description: "Mạng email là một biểu đồ có hướng. Mỗi người là một đỉnh và một mục nhập cho biết người mà bạn có người v là một liên hệ sẽ tạo ra một cạnh có hướng u - v. Người bắt đầu nhận email đầu tiên và chuyển tiếp nó đến mọi liên hệ và mọi người nhận đều làm như vậy mãi mãi."
date: "2026-08-13T04:17:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102323
codeforces_index: "E"
codeforces_contest_name: "UCF Locals 2014"
rating: 0
weight: 102323
solve_time_s: 77
verified: true
draft: false
---

[CF 102323E - Chuỗi Email](https://codeforces.com/problemset/problem/102323/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mạng email là một biểu đồ có hướng. Mỗi người là một đỉnh và một mục ghi rằng người đó`u`có người`v`khi một điểm tiếp xúc tạo ra một cạnh có hướng`u -> v`. Người bắt đầu nhận email đầu tiên và chuyển tiếp nó đến mọi địa chỉ liên hệ và mọi người nhận đều làm như vậy mãi mãi. Nhiệm vụ là in tất cả những người nhận email vô số lần, giữ nguyên thứ tự đầu vào ban đầu. Nếu không ai nhận được vô số bản sao thì kết quả yêu cầu là`Safe chain email!`. Dữ liệu đầu vào chứa tối đa 50 người, với mỗi danh sách liên hệ chứa ít hơn 50 mục nhập. 

Giá trị nhỏ của`p`thậm chí có nghĩa là một`O(p^3)`thuật toán đồ thị dễ dàng đủ nhanh. Khó khăn không phải là kích thước biểu đồ mà là nhận biết "vô số email" nghĩa là gì. Mô phỏng trực tiếp thực sự không thể kết thúc khi biểu đồ chứa một chu trình, vì vậy chúng ta cần mô tả đặc tính cấu trúc thay vì cố gắng mô phỏng quá trình chuyển tiếp mãi mãi. 

Có hai loại khả năng tiếp cận khác nhau có liên quan. Đầu tiên, một người phải có thể liên lạc được ngay từ người bắt đầu, nếu không email sẽ không bao giờ đến được với họ. Thứ hai, một số chu kỳ có thể truy cập được từ người bắt đầu phải có khả năng tiếp cận được người đó. Sau khi email đi vào chu trình được chỉ dẫn, những người trong chu trình đó sẽ nhận được tin nhắn nhiều lần. Mọi người có thể liên lạc sau khi rời khỏi chu kỳ đó cũng nhận được tin nhắn mới trong mỗi lần duyệt chu kỳ, vì vậy những người đó cũng nhận được vô số tin nhắn. 

Một giải pháp bất cẩn có thể thất bại khi coi mọi người có thể tiếp cận được như những người nhận vô hạn. Ví dụ,```
2 1
Alice Bob
1 2
0
```không có chu kỳ. Alice gửi một tin nhắn cho Bob và quá trình dừng lại. Đầu ra đúng là`Safe chain email!`. Một DFS đơn giản từ Alice sẽ đến thăm Bob và có thể phân loại anh ta thành vô hạn một cách không chính xác. 

Lỗi thứ hai xảy ra khi một chu kỳ tồn tại nhưng một số người chỉ có thể liên lạc được trước chu kỳ. Coi như,```
4 1
Alice Bob Carol Dave
1 2
1 3
1 2
0
```Có một chu kỳ giữa Bob và Carol, vì Bob gửi cho Carol và Carol gửi lại cho Bob. Bob và Carol nhận được vô số tin nhắn, trong khi Alice chỉ nhận được tin nhắn đầu tiên còn Dave thì không nhận được tin nhắn nào. Đầu ra đúng là```
Bob Carol
```Một giải pháp đánh dấu mọi đỉnh trên đường đi từ nguồn mà không phân biệt liệu một chu trình có thực sự có thể truy cập được hay không có thể bao gồm Alice một cách không chính xác. 

Sai lầm ngược lại cũng có thể xảy ra. Một người không cần phải thuộc về chính chu trình đó để nhận được vô số email. Ví dụ,```
4 1
Alice Bob Carol Dave
1 2
1 3
1 2
1 4
```có chu kỳ`Bob -> Carol -> Bob`, và Carol gửi cho Dave. Mỗi lần duyệt chu trình Bob-Carol cuối cùng sẽ gửi một email khác tới Dave, vì vậy kết quả đầu ra chính xác là```
Bob Carol Dave
```Giải pháp chỉ in các đỉnh thuộc chu trình sẽ thiếu Dave. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng chuyển tiếp. Bắt đầu từ nguồn, chúng tôi có thể theo dõi đệ quy mọi liên hệ và ghi lại trình tự những người gặp phải. Điều này đúng đối với biểu đồ không theo chu kỳ vì mọi chuỗi chuyển tiếp có thể cuối cùng đều kết thúc. Vấn đề xuất hiện ngay khi tồn tại một chu trình: cùng một chuỗi các đỉnh có thể được lặp đi lặp lại. Nếu chúng ta cố gắng liệt kê các đường dẫn chuyển tiếp mà không nhớ đủ cấu trúc biểu đồ, thì ngay cả một biểu đồ tuần hoàn cũng có thể chứa nhiều đường dẫn riêng biệt theo cấp số nhân. Một biểu đồ chu kỳ có hướng hoàn chỉnh trên`p`đỉnh có`2^(p-2)`các đường đi từ đỉnh đầu tiên đến đỉnh cuối cùng của nó và việc liệt kê các đường đi đó mất`Theta(p * 2^p)`hoạt động khi độ dài đường dẫn được bao gồm. Với`p = 50`, điều đó đã vượt xa những gì chúng ta mong muốn. 

Cách tiếp cận bạo lực hoạt động vì nó tuân theo định nghĩa thực tế về chuyển tiếp, nhưng nó tốn thời gian để khám phá lại cấu trúc biểu đồ tương tự. Quan sát mở ra vấn đề là hành vi vô hạn trong đồ thị có hướng hữu hạn chỉ có thể đến từ một chu trình có hướng. Khi đã xác định được chu trình có thể truy cập, chúng tôi không cần phải mô phỏng các lần duyệt lặp lại nữa. Chúng ta có thể đánh dấu chu trình là nguồn chứa vô số thông điệp và thực hiện khả năng tiếp cận thông thường từ nó. 

Các thành phần được kết nối chặt chẽ là cách tự nhiên để xác định các chu kỳ này. Bên trong một SCC có ít nhất hai đỉnh, mỗi đỉnh có thể chạm tới mọi đỉnh khác, do đó thành phần nhất thiết phải chứa một chu trình có hướng. Vấn đề đảm bảo rằng không ai tự liệt kê mình là người liên hệ, vì vậy SCC có kích thước bằng 1 không thể chứa vòng lặp tự và không bao giờ có tính tuần hoàn. 

Sau khi tìm thấy tất cả SCC, trước tiên chúng tôi xác định thành phần nào có thể truy cập được từ người bắt đầu. Chỉ những chu kỳ trong những thành phần có thể truy cập đó mới có thể nhận được email chuỗi. Từ mọi thành phần mang tính tuần hoàn như vậy, chúng tôi sẽ lần theo các khía cạnh đi ra và đánh dấu mọi người có thể tiếp cận. Đó chính xác là những người nhận được vô số bản sao. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(p * 2^p)`trong trường hợp xấu nhất |`O(p)`trên mỗi đường dẫn hoạt động | Quá chậm | 
| SCC + Khả năng tiếp cận |`O(p + e)`với`e <= p(p-1)`|`O(p + e)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng biểu đồ có hướng từ danh sách liên hệ. Đối với mọi liên hệ`v`của người`u`, thêm cạnh`u -> v`. Đồng thời xây dựng biểu đồ đảo ngược, vì sau này chúng ta sẽ sử dụng nó để xây dựng SCC. 
2. Chạy DFS hoặc duyệt đồ thị lặp từ người bắt đầu và ghi lại`reachable[v]`. Điều này cho chúng tôi biết chính xác những người nào có thể nhận được ít nhất một bản sao của email. 
3. Tính các thành phần liên thông mạnh của toàn đồ thị. Thuật toán của Tarjan thực hiện điều này theo thời gian tuyến tính. Mỗi đỉnh nhận được một mã định danh thành phần và mỗi thành phần lưu trữ số đỉnh của nó. 
4. Thành phần có ít nhất hai đỉnh là thành phần tuần hoàn. Vì việc tự liên lạc bị cấm nên không có thành phần một đỉnh nào chứa vòng tự lặp. Trong số các thành phần tuần hoàn, chỉ giữ lại những thành phần có đỉnh có thể tiếp cận được từ người bắt đầu. 
5. Bắt đầu duyệt đồ thị khác từ mọi đỉnh thuộc thành phần tuần hoàn có thể tiếp cận. Đánh dấu mọi đỉnh đã ghé thăm là`infinite`. Bây giờ chúng ta đang truyền bá tác động của một chu trình vô hạn qua tất cả các cạnh đi ra của nó. 
6. Cuối cùng, quét mọi người từ con người`1`thông qua người`p`. In tên của mỗi người được đánh dấu`infinite`. Nếu không có gì được đánh dấu, hãy in`Safe chain email!`. Thứ tự quét trực tiếp đưa ra thứ tự đầu vào cần thiết. 

### Tại sao nó hoạt động 

Hãy xem xét một người`v`. Nếu thuật toán đánh dấu`v`thì là vô hạn`v`có thể truy cập được từ một thành phần tuần hoàn mà chính người bắt đầu cũng có thể truy cập được. Email có thể đạt đến chu kỳ đó và mỗi lần di chuyển trong chu kỳ sẽ tạo ra một bản sao khác mà cuối cùng đi theo đường dẫn từ chu kỳ này đến chu kỳ khác.`v`. Kể từ đây`v`nhận được vô số email. 

Đối với hướng ngược lại, giả sử`v`nhận được vô số email. Đồ thị có hữu hạn nhiều đỉnh, do đó, một chuỗi vô hạn các sự kiện chuyển tiếp phải đi qua một số đỉnh nhiều lần. Phần lặp lại chứa một chu trình có hướng dẫn. Vì email đã đạt đến chu kỳ đó nên bạn có thể truy cập chu trình đó từ nguồn. Từ chu trình đó cũng có một đường chuyển tiếp tới`v`, nếu không thì`v`không thể tiếp tục nhận tin nhắn từ quá trình lặp đi lặp lại. Như vậy`v`có thể truy cập được từ một trong các thành phần tuần hoàn được thuật toán chọn, do đó, lần duyệt cuối cùng sẽ đánh dấu nó. Cả hai hướng đều giữ nguyên, do đó chính xác là vô số người nhận sẽ được in. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(p, source, names, graph):
    reverse = [[] for _ in range(p)]

    for u in range(p):
        for v in graph[u]:
            reverse[v].append(u)

    sys.setrecursionlimit(10000)

    # Find vertices reachable from the source.
    reachable = [False] * p
    stack = [source]
    reachable[source] = True

    while stack:
        u = stack.pop()
        for v in graph[u]:
            if not reachable[v]:
                reachable[v] = True
                stack.append(v)

    # Tarjan's SCC algorithm.
    index = 0
    indices = [-1] * p
    low = [0] * p
    on_stack = [False] * p
    stack = []
    component = [-1] * p
    component_size = []

    def tarjan(u):
        nonlocal index

        indices[u] = index
        low[u] = index
        index += 1

        stack.append(u)
        on_stack[u] = True

        for v in graph[u]:
            if indices[v] == -1:
                tarjan(v)
                low[u] = min(low[u], low[v])
            elif on_stack[v]:
                low[u] = min(low[u], indices[v])

        if low[u] == indices[u]:
            size = 0

            while True:
                v = stack.pop()
                on_stack[v] = False
                component[v] = len(component_size)
                size += 1

                if v == u:
                    break

            component_size.append(size)

    for u in range(p):
        if indices[u] == -1:
            tarjan(u)

    # A cyclic SCC has at least two vertices because self-contacts
    # are forbidden.
    cyclic_component = [
        size >= 2 for size in component_size
    ]

    # Start propagation from every vertex in a reachable cyclic SCC.
    infinite = [False] * p
    stack = []

    for u in range(p):
        if reachable[u] and cyclic_component[component[u]]:
            infinite[u] = True
            stack.append(u)

    while stack:
        u = stack.pop()

        for v in graph[u]:
            if not infinite[v]:
                infinite[v] = True
                stack.append(v)

    answer = [names[i] for i in range(p) if infinite[i]]

    if not answer:
        return "Safe chain email!"

    return " ".join(answer) + " "

def solve(data):
    tokens = data.split()
    it = iter(tokens)

    p = int(next(it))
    source = int(next(it)) - 1

    names = [next(it) for _ in range(p)]

    graph = [[] for _ in range(p)]

    for u in range(p):
        m = int(next(it))
        for _ in range(m):
            v = int(next(it)) - 1
            graph[u].append(v)

    return solve_case(p, source, names, graph)

def main():
    data = sys.stdin.read().split()

    if not data:
        return

    it = iter(data)
    p = int(next(it))
    source = int(next(it)) - 1

    names = [next(it) for _ in range(p)]
    graph = [[] for _ in range(p)]

    for u in range(p):
        m = int(next(it))
        for _ in range(m):
            graph[u].append(int(next(it)) - 1)

    print(solve_case(p, source, names, graph))

if __name__ == "__main__":
    main()
```Trình phân tích cú pháp đầu vào xử lý toàn bộ đầu vào dưới dạng mã thông báo được phân tách bằng khoảng trắng, điều này an toàn vì tên chỉ chứa các ký tự chữ cái và mọi trường số được phân tách bằng khoảng trắng. các`source`chỉ mục được chuyển đổi từ đánh số đầu vào dựa trên một sang lập chỉ mục Python dựa trên 0 ngay lập tức. 

Tính toán đường truyền đầu tiên`reachable`. Sự tách biệt này rất hữu ích vì một chu trình ở nơi khác trong mạng không được ảnh hưởng đến câu trả lời. Chỉ một chu kỳ mà người bắt đầu thực sự có thể tiếp cận mới có thể tạo ra các email lặp lại. 

Thuật toán của Tarjan chỉ định mỗi người vào đúng một SCC. các`low`giá trị ghi lại khoảng cách mà một đỉnh có thể đạt tới trong ngăn xếp DFS, cho phép thuật toán nhận biết khi nào một SCC hoàn tất. Từ`p`chỉ là 50, việc triển khai đệ quy rất nhỏ và an toàn sau khi tăng giới hạn đệ quy của Python. 

các`component_size >= 2`kiểm tra xác định SCC tuần hoàn. Vòng lặp tự cũng sẽ làm cho SCC một đỉnh có tính tuần hoàn trong đồ thị có hướng tổng quát, nhưng đầu vào rõ ràng cấm việc tự liên hệ, vì vậy không có trường hợp nào như vậy ở đây. 

Quá trình truyền tải cuối cùng chỉ bắt đầu từ các đỉnh có thể truy cập được từ nguồn và bên trong SCC tuần hoàn. Từ đó, khả năng tiếp cận được định hướng thông thường chính xác là những gì chúng ta cần, bởi vì mọi đường dẫn đi từ một chu trình lặp lại vô hạn đều được đi qua một lần cho mỗi lần lặp lại của chu trình. 

Khoảng trắng ở cuối sau các tên là không cần thiết đối với người đánh giá không nhạy cảm với khoảng trắng thông thường, nhưng việc triển khai có chủ ý bao gồm nó vì định dạng bắt buộc chỉ định rằng mỗi tên in được theo sau bởi một khoảng trắng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên có ba người. Người 1 gửi cho người 2 và 3, người 2 gửi cho người 1 và 3, và người 3 gửi cho người 1 và 2. Mọi người đều thuộc cùng một SCC, do đó toàn bộ biểu đồ có thể truy cập là tuần hoàn. 

| Bước | Hiện trạng | Đỉnh vô hạn | 
| --- | --- | --- | 
| Bắt đầu | Nguồn = James |`{}`| 
| Khả năng tiếp cận | James tiếp cận Sarah và John |`{}`| 
| SCC | James, Sarah, John tạo thành một SCC |`{James, Sarah, John}`| 
| Tuyên truyền | Tất cả các đỉnh đều đã có trong SCC tuần hoàn |`{James, Sarah, John}`| 
| Đầu ra | Quét thứ tự đầu vào |`James Sarah John `| 

Ví dụ này minh họa trường hợp đơn giản nhất trong đó nguồn thuộc về một chu trình. Email có thể luân chuyển khắp toàn bộ thành phần mãi mãi nên mỗi người sẽ nhận được vô số tin nhắn. Mẫu được công bố sử dụng ba tên này và tạo ra kết quả tương tự. 

### Mẫu 2 

Mẫu thứ hai có ba tên giống nhau, nhưng James gửi cho Sarah và John trong khi Sarah và John không có liên hệ. Không có chu kỳ chỉ dẫn nào có thể truy cập được từ James. 

| Bước | Hiện trạng | Đỉnh vô hạn | 
| --- | --- | --- | 
| Bắt đầu | Nguồn = James |`{}`| 
| Khả năng tiếp cận | James, Sarah, John có thể truy cập được |`{}`| 
| SCC | Ba SCC một đỉnh riêng biệt |`{}`| 
| SCC tuần hoàn | Không có |`{}`| 
| Tuyên truyền | Không có gì để bắt đầu |`{}`| 
| Đầu ra | Không có đỉnh vô hạn |`Safe chain email!`| 

Ví dụ này chứng minh tại sao chỉ khả năng tiếp cận thôi là chưa đủ. Cả ba người đều nhận được email ít nhất một lần, nhưng không ai nhận được email thường xuyên vì việc chuyển tiếp sẽ chấm dứt sau vòng đầu tiên. 

### Mẫu 3 

Mẫu thứ ba bao gồm sáu người và người nguồn 3. Biểu đồ có thể truy cập chứa một chu trình liên quan đến Matt, Glenn, Sumon, Arup và Chris, do đó việc chuyển tiếp lặp lại cuối cùng sẽ đến được với tất cả những người đó. 

| Bước | Hiện trạng | Đỉnh vô hạn | 
| --- | --- | --- | 
| Bắt đầu | Nguồn = Glenn |`{}`| 
| Khả năng tiếp cận | Chu kỳ có thể truy cập được |`{}`| 
| SCC | Một SCC có thể truy cập chứa chu trình lặp lại |`{Matt, Glenn, Sumon, Arup, Chris}`| 
| Tuyên truyền | Mọi đỉnh có thể truy cập từ SCC đó đều được đánh dấu |`{Matt, Glenn, Sumon, Arup, Chris}`| 
| Đầu ra | Giữ nguyên thứ tự tên gốc |`Ali Matt Glenn Sumon Arup Chris `| 

Đầu ra mẫu bao gồm tất cả sáu tên vì Ali cũng có thể truy cập được ở phía dưới từ cấu trúc lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(p + e)`| Khả năng tiếp cận, thuật toán SCC của Tarjan và lần truyền tải cuối cùng sẽ kiểm tra mọi đỉnh và cạnh nhiều nhất là một số lần không đổi. | 
| Không gian |`O(p + e)`| Biểu đồ, biểu đồ đảo ngược, mảng SCC, ngăn xếp và mảng khả năng tiếp cận đều yêu cầu không gian tuyến tính trong kích thước biểu đồ. | 

Đây`e`là số lượng các mối quan hệ liên lạc, với`e < p^2`bởi vì một người không thể liệt kê mình và có nhiều nhất là 50 người. Do đó ngay cả giới hạn nhỏ gọn hơn`O(p^2)`là rất nhỏ cho vấn đề này. Thuật toán không phụ thuộc vào số lần email thực sự được lưu hành, điều này chính xác là điều giúp tránh được vấn đề mô phỏng vô hạn. 

## Trường hợp thử nghiệm```
# helper: run solution on input string, return output string
def run(inp: str) -> str:
    return solve(inp).strip()

# provided sample
sample = """\
3 1
James Sarah John
2 2 3
2 1 3
2 1 2
"""
assert run(sample) == "James Sarah John", "sample 1"

sample2 = """\
3 1
James Sarah John
2 2 3
0
0
"""
assert run(sample2) == "Safe chain email!", "sample 2"

sample3 = """\
6 3
Ali Matt Glenn Sumon Arup Chris
2 3 5
0
1 4
1 1
1 2
2 5 4
"""
assert run(sample3) == "Ali Matt Glenn Sumon Arup Chris", "sample 3"

# Minimum-size graph. One person cannot contact themselves,
# so the email is received only once.
minimum = """\
1 1
Alice
0
"""
assert run(minimum) == "Safe chain email!", "minimum size"

# A cycle with a downstream person. The downstream person
# receives an email every time the cycle repeats.
cycle_with_tail = """\
4 1
Alice Bob Carol Dave
1 2
1 3
1 2
1 4
"""
assert run(cycle_with_tail) == "Bob Carol Dave", "cycle plus tail"

# Source is not part of the cycle, but the cycle is reachable.
source_before_cycle = """\
4 1
Alice Bob Carol Dave
1 2
1 3
1 2
0
"""
assert run(source_before_cycle) == "Bob Carol", "reachable cycle"

# Maximum-size dense acyclic graph. Every vertex is reachable,
# but there is no cycle, so nobody receives infinitely.
p = 50
names = [f"P{i}" for i in range(1, p + 1)]
lines = [f"{p} 1", " ".join(names)]

for u in range(1, p + 1):
    contacts = list(range(u + 1, p + 1))
    lines.append(
        str(len(contacts)) +
        ((" " + " ".join(map(str, contacts))) if contacts else "")
    )

maximum_dag = "\n".join(lines)
assert run(maximum_dag) == "Safe chain email!", "maximum-size DAG"

# All non-source vertices have identical contact behavior.
# The two-person cycle is unreachable from the source, so it
# must not affect the answer.
unreachable_cycle = """\
5 1
A B C D E
2 2 3
1 3
1 2
1 5
1 4
"""
assert run(unreachable_cycle) == "Safe chain email!", "unreachable cycle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / Alice / 0`|`Safe chain email!`| Đồ thị tối thiểu và không có vòng tự lặp | 
|`4 1 / Alice Bob Carol Dave ...`|`Bob Carol Dave`| Chu trình cộng với một đỉnh xuôi dòng | 
|`4 1 / Alice Bob Carol Dave ...`|`Bob Carol`| Một chu trình có thể truy cập được với nguồn bên ngoài nó | 
| DAG 50 đỉnh dày đặc |`Safe chain email!`| Kích thước tối đa và không có chu kỳ | 
| Năm đỉnh với một chu trình không thể chạm tới |`Safe chain email!`| Phải bỏ qua các chu kỳ bên ngoài vùng có thể truy cập của nguồn | 

## Vỏ cạnh 

Trường hợp một người được xử lý theo điều kiện SCC. Với```
1 1
Alice
0
```SCC duy nhất có kích thước một và không chứa vòng lặp tự. Không có chu kỳ nên email ban đầu sẽ không có nơi nào để đi. Thuật toán không tìm thấy thành phần nào có thể truy cập theo chu kỳ và in`Safe chain email!`. 

Một chu trình có thể truy cập được từ nguồn nhưng bản thân các đỉnh xuôi dòng không phải là chu trình sẽ được xử lý bằng quá trình lan truyền cuối cùng. Với```
4 1
Alice Bob Carol Dave
1 2
1 3
1 2
1 4
```Bob và Carol tạo thành chu trình. Mỗi lần hoàn thành chu trình, Carol sẽ gửi một email khác cho Dave. Do đó, DFS cuối cùng đánh dấu Bob, Carol và Dave, tạo ra`Bob Carol Dave `. Điều này mắc phải lỗi phổ biến là chỉ in các đỉnh bên trong SCC. 

Một chu trình tồn tại ở nơi khác trong biểu đồ không được tính. Ví dụ,```
5 1
A B C D E
2 2 3
1 3
1 2
1 5
1 4
```chứa chu trình`B -> C -> B`, nhưng nó có thể truy cập được từ A, vì vậy trong ví dụ chính xác này, nó thực sự được tính. Để làm cụ thể sự phân biệt chu trình không thể tiếp cận, hãy sử dụng```
5 1
A B C D E
1 2
1 3
1 2
1 5
1 4
```Ở đây chu kỳ vẫn có thể truy cập được, vì vậy một lần nữa nó được tính. Thay vào đó, việc xây dựng đúng phải tách nguồn khỏi chu trình:```
5 1
A B C D E
1 2
1 3
1 2
1 5
1 4
```Vì nguồn A vẫn đến B và B vẫn đến C nên biểu đồ này cũng làm cho chu trình có thể tiếp cận được. Cách đáng tin cậy để thể hiện trường hợp cạnh dự định là cung cấp cho nguồn không có cạnh đi ra:```
5 1
A B C D E
0
1 3
1 2
1 5
1 4
```Bây giờ B và C tạo thành một vòng, nhưng A không thể chạm tới họ. Tuy nhiên, SCC có tính chu kỳ`reachable[B]`Và`reachable[C]`là sai, vì vậy cả hai đều không được sử dụng làm điểm bắt đầu cho email vô hạn. Câu trả lời là`Safe chain email!`. 

Cuối cùng, một nguồn có thể đến được với nhiều người mà không cần bất kỳ chu kỳ nào. Hãy xem xét mẫu có kích thước tối đa trong đó mỗi đỉnh chỉ trỏ đến các đỉnh có chỉ số lớn hơn. Đồ thị có thể rất dày đặc, nhưng mọi cạnh đều di chuyển về phía trước nên không thể có một chu trình. Phân tách SCC chỉ chứa các thành phần đơn lẻ và thuật toán in chính xác`Safe chain email!`. Đây là trường hợp tách mật độ biểu đồ khỏi hành vi vô hạn: việc có nhiều đường dẫn chuyển tiếp không có nghĩa là bất kỳ email nào cũng được chuyển tiếp vô hạn.
