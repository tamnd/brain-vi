---
title: "CF 129B - Học sinh và Dây giày"
description: "Chúng ta có thể mô hình học sinh và dây giày dưới dạng đồ thị vô hướng. Mỗi học sinh là một đỉnh và mỗi sợi dây giày giữa hai học sinh là một cạnh. Trong một vòng, mọi sinh viên có bằng chính xác là 1 sẽ bị loại cùng một lúc."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 129
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 94 (Div. 2 Only)"
rating: 1200
weight: 129
solve_time_s: 135
verified: true
draft: false
---

[CF 129B - Học sinh và Dây giày](https://codeforces.com/problemset/problem/129/B) 

**Đánh giá:** 1200 
**Tags:** bạo lực, dfs và tương tự, đồ thị, triển khai 
**Thời gian giải:** 2m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có thể mô hình học sinh và dây giày dưới dạng đồ thị vô hướng. Mỗi học sinh là một đỉnh và mỗi sợi dây giày giữa hai học sinh là một cạnh. 

Trong một vòng, mọi sinh viên có bằng chính xác là 1 sẽ bị loại cùng một lúc. Khi những học sinh đó rời đi, mọi khía cạnh sự việc cũng biến mất. Điều này có thể tạo ra các đỉnh cấp 1 mới cho vòng tiếp theo. Quá trình tiếp tục cho đến khi không còn đỉnh nào có bậc 1. 

Nhiệm vụ là đếm xem có bao nhiêu vòng loại bỏ xảy ra. 

Các ràng buộc đủ nhỏ để chúng ta không cần đến máy móc lý thuyết đồ thị phức tạp, nhưng chúng đủ lớn để việc tính toán lại bất cẩn lặp đi lặp lại có thể trở nên tốn kém. Nếu chúng ta tính lại bậc của mỗi đỉnh sau mỗi vòng, thì trong đồ thị dày đặc, chúng ta có thể quét hầu hết các cạnh nhiều lần. Một giải pháp xung quanh$O(n + m)$hoặc$O(n^2)$ở đây hoàn toàn an toàn, trong khi có thứ gì đó gần hơn$O(nm)$bắt đầu trở nên nguy hiểm. 

Chi tiết triển khai nguy hiểm nhất là việc loại bỏ đồng thời. Giả sử chúng ta loại bỏ từng học sinh một thay vì loại bỏ cả nhóm cùng nhau. Điều đó làm thay đổi biểu đồ trong vòng thi và có thể loại bỏ học sinh thừa một cách không chính xác. 

Hãy xem xét biểu đồ này:```
1 - 2 - 3
```đầu vào:```
3 2
1 2
2 3
```Quá trình đúng là: 

Vòng đầu tiên loại bỏ 1 và 3 cùng nhau. 

Vòng thứ hai loại bỏ 2. 

Trả lời = 2. 

Nếu chúng ta tham lam loại bỏ từng cái một, việc loại bỏ 1 trước sẽ khiến 2 trở thành cấp 1 ngay lập tức và chúng ta có thể loại bỏ nó một cách không chính xác trong cùng một vòng. 

Một lỗi dễ mắc phải khác là xử lý sai các đỉnh bị cô lập. Học sinh có bậc 0 không bao giờ bị loại bỏ vì quy tắc chỉ loại bỏ các đỉnh bậc 1. 

Ví dụ:```
3 0
```Tất cả học sinh đều bị cô lập ngay từ đầu. Không ai rời đi, nên câu trả lời là:```
0
```Việc thực hiện bất cẩn loại bỏ tất cả các đỉnh có bậc nhiều nhất là 1 sẽ tạo ra 1 không chính xác. 

Chu kỳ là một trường hợp cạnh quan trọng khác. Trong một chu trình, mọi đỉnh đều có bậc 2 nên không có gì xảy ra. 

Ví dụ:```
4 4
1 2
2 3
3 4
4 1
```Đầu ra đúng:```
0
```Bất kỳ giải pháp nào giả định việc cắt tỉa lặp đi lặp lại cuối cùng sẽ xóa mọi thành phần được kết nối sẽ không thành công ở đây. 

## Phương pháp tiếp cận 

Việc mô phỏng trực tiếp rất đơn giản. Chúng tôi duy trì biểu đồ hiện tại, tìm tất cả các đỉnh có bậc 1, loại bỏ chúng đồng thời, cập nhật biểu đồ và lặp lại cho đến khi không còn đỉnh nào như vậy. 

Phiên bản brute-force tính lại mọi cấp độ từ đầu sau mỗi vòng. Trong trường hợp xấu nhất, một chuỗi dài chỉ loại bỏ được hai đỉnh mỗi vòng. Nếu có$n$các đỉnh, chúng ta có thể thực hiện khoảng$n/2$vòng. Tính toán lại tất cả các mức độ mỗi lần chi phí$O(m)$, dẫn đến đại khái$O(nm)$. 

Điều đó vẫn phù hợp với các ràng buộc nhỏ, nhưng nó thực hiện công việc không cần thiết vì hầu hết các cạnh không thay đổi giữa các vòng. 

Quan sát quan trọng là chỉ những lân cận của các đỉnh bị loại bỏ mới có thể thay đổi độ của chúng. Khi một sinh viên rời đi, chúng tôi chỉ cần cập nhật bằng cấp của những sinh viên lân cận thay vì xây dựng lại mọi thứ. 

Điều này biến vấn đề thành một quá trình loại bỏ lá theo lớp. Chúng tôi duy trì bậc hiện tại và liên tục loại bỏ tất cả các đỉnh có bậc bằng 1. Mỗi cạnh chỉ được xử lý khi một trong các điểm cuối của nó biến mất, do đó tổng công việc trở thành tuyến tính theo kích thước của biểu đồ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$|$O(n + m)$| Về mặt khái niệm quá chậm | 
| Tối ưu |$O(n + m)$|$O(n + m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề cho đồ thị và tính bậc của mỗi đỉnh. 
2. Tìm tất cả các đỉnh có bậc chính xác bằng 1. Đây là những học sinh bị loại ở vòng đầu tiên. 
3. Trong khi vòng hiện tại không trống, hãy xử lý toàn bộ vòng một lần. 
4. Tăng câu trả lời lên 1 vì có một nhóm học sinh bị đuổi học. 
5. Đánh dấu mọi đỉnh trong vòng hiện tại là đã bị loại bỏ. Chúng tôi xử lý toàn bộ lô cùng nhau vì việc xóa diễn ra đồng thời. 
6. Đối với mỗi đỉnh bị loại bỏ, hãy lặp lại các đỉnh lân cận của nó. 
7. Nếu lân cận chưa bị loại bỏ, hãy giảm bậc của nó đi 1 vì một cạnh tới đã biến mất. 
8. Nếu cấp độ của người hàng xóm trở thành chính xác 1 sau khi cập nhật, hãy đặt nó vào vòng tiếp theo. 
9. Thay thế vòng hiện tại bằng vòng tiếp theo và tiếp tục. 
10. Khi không còn đỉnh bậc 1, dừng lại và in số vòng. 

### Tại sao nó hoạt động 

Tại mọi thời điểm, mảng độ khớp với biểu đồ hiện tại sau khi tất cả các vòng trước đó đã bị xóa. Một đỉnh bước vào vòng tiếp theo chính xác khi bậc của nó trở thành 1 sau khi vòng hiện tại biến mất. Bởi vì chúng tôi xử lý việc xóa theo đợt nên thuật toán hoàn toàn phù hợp với quy tắc xóa đồng thời của câu lệnh. Không có đỉnh nào bị loại bỏ quá sớm hoặc quá muộn. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    graph = [[] for _ in range(n)]
    degree = [0] * n

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1

        graph[u].append(v)
        graph[v].append(u)

        degree[u] += 1
        degree[v] += 1

    removed = [False] * n

    current = deque()

    for i in range(n):
        if degree[i] == 1:
            current.append(i)

    rounds = 0

    while current:
        rounds += 1

        next_round = deque()

        current_nodes = list(current)
        current.clear()

        for node in current_nodes:
            removed[node] = True

        for node in current_nodes:
            for nei in graph[node]:
                if removed[nei]:
                    continue

                degree[nei] -= 1

                if degree[nei] == 1:
                    next_round.append(nei)

        current = next_round

    print(rounds)

solve()
```Danh sách kề lưu trữ biểu đồ một cách hiệu quả và mảng độ theo dõi số lượng hàng xóm còn lại hiện tại của mỗi học sinh. 

Vòng lặp đầu tiên thu thập tất cả các lá ban đầu, nghĩa là các đỉnh có bậc chính xác là 1. Chúng tạo thành vòng đầu tiên. 

Phần tinh tế là xử lý việc loại bỏ đồng thời một cách chính xác. Đầu tiên chúng ta sao chép tất cả các đỉnh từ hàng đợi hiện tại vào`current_nodes`và đánh dấu tất cả chúng là đã bị xóa trước khi cập nhật hàng xóm. Điều này ngăn việc loại bỏ một bên trong vòng khỏi ảnh hưởng đến việc liệu một đỉnh khác trong cùng vòng có còn tồn tại hay không. 

các`removed`mảng ngăn chặn nhiều cập nhật trên các đỉnh đã bị xóa. Nếu không có nó, độ có thể trở thành âm hoặc các đỉnh có thể được chèn liên tục vào các vòng trong tương lai. 

Một lỗi phổ biến là thêm các đỉnh vào vòng tiếp theo bất cứ khi nào độ trở nên nhỏ hơn hoặc bằng 1. Điều đó loại bỏ không chính xác các đỉnh bị cô lập. Điều kiện phải chính xác`degree[nei] == 1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
1 2
2 3
3 1
```Đồ thị ban đầu là một hình tam giác. 

| Vòng | Mảng độ hiện tại | Đỉnh bậc 1 | Đã xóa vòng này | 
| --- | --- | --- | --- | 
| Bắt đầu | [2, 2, 2] | không | không | 

Không có đỉnh nào có bậc 1 nên quá trình dừng ngay lập tức. 

Trả lời:```
0
```Ví dụ này cho thấy chu kỳ tồn tại mãi mãi vì việc loại bỏ lá không bao giờ bắt đầu. 

### Ví dụ 2 

đầu vào:```
4 3
1 2
2 3
3 4
```Đây là một chuỗi đơn giản. 

| Vòng | Mảng độ hiện tại | Đỉnh bậc 1 | Đã xóa vòng này | 
| --- | --- | --- | --- | 
| 1 | [1, 2, 2, 1] | 1, 4 | 1, 4 | 
| 2 | [0, 1, 1, 0] | 2, 3 | 2, 3 | 
| Kết thúc | [0, 0, 0, 0] | không | không | 

Câu trả lời là:```
2
```Dấu vết này cho thấy tại sao việc loại bỏ phải diễn ra theo từng lớp. Các đỉnh ở giữa chỉ có thể tháo rời được sau khi các điểm cuối biến mất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Mỗi đỉnh và cạnh chỉ được xử lý một số lần không đổi | 
| Không gian |$O(n + m)$| Danh sách kề, mảng độ, hàng đợi và mảng bị loại bỏ | 

Điều này dễ dàng phù hợp với các giới hạn. Ngay cả đối với đồ thị hợp lệ lớn nhất, quá trình xử lý tuyến tính trên các đỉnh và cạnh vẫn diễn ra nhanh chóng trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n, m = map(int, input().split())

        graph = [[] for _ in range(n)]
        degree = [0] * n

        for _ in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1

            graph[u].append(v)
            graph[v].append(u)

            degree[u] += 1
            degree[v] += 1

        removed = [False] * n

        current = deque()

        for i in range(n):
            if degree[i] == 1:
                current.append(i)

        rounds = 0

        while current:
            rounds += 1

            current_nodes = list(current)
            current.clear()

            next_round = deque()

            for node in current_nodes:
                removed[node] = True

            for node in current_nodes:
                for nei in graph[node]:
                    if removed[nei]:
                        continue

                    degree[nei] -= 1

                    if degree[nei] == 1:
                        next_round.append(nei)

            current = next_round

        return str(rounds)

    return solve()

# provided sample
assert run(
"""3 3
1 2
2 3
3 1
"""
) == "0", "sample 1"

# chain of length 4
assert run(
"""4 3
1 2
2 3
3 4
"""
) == "2", "simple chain"

# isolated vertices only
assert run(
"""5 0
"""
) == "0", "isolated vertices are never removed"

# star graph
assert run(
"""5 4
1 2
1 3
1 4
1 5
"""
) == "1", "center becomes isolated, not removable"

# single edge
assert run(
"""2 1
1 2
"""
) == "1", "both endpoints removed together"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chu kỳ tam giác | 0 | Chu kỳ không bao giờ bắt đầu loại bỏ | 
| Chuỗi dài 4 | 2 | Loại bỏ nhiều lớp | 
| Không có cạnh | 0 | Đỉnh độ 0 tồn tại mãi mãi | 
| Đồ thị sao | 1 | Trung tâm trở nên biệt lập, không thể tháo rời | 
| Cạnh đơn | 1 | Loại bỏ điểm cuối đồng thời | 

## Vỏ cạnh 

Xét các đỉnh cô lập:```
3 0
```Bằng cấp ban đầu là`[0, 0, 0]`. Không có đỉnh nào có bậc 1 nên hàng đợi bắt đầu trống. Vòng lặp không bao giờ chạy và câu trả lời vẫn là 0. Điều này phù hợp với quy tắc vì học sinh bị cô lập không bị khiển trách. 

Bây giờ hãy xem xét một biểu đồ hình sao:```
5 4
1 2
1 3
1 4
1 5
```Bằng cấp ban đầu là`[4, 1, 1, 1, 1]`. 

Ở vòng đầu tiên, các đỉnh 2, 3, 4 và 5 bị loại bỏ đồng thời. Đỉnh 1 mất bốn cạnh và bậc của nó trở thành 0. Vì chỉ có các đỉnh bậc 1 có thể tháo rời được nên quá trình sẽ dừng sau một vòng. Câu trả lời là 1. 

Việc thực hiện bất cẩn bằng cách sử dụng`degree <= 1`sẽ loại bỏ sai đỉnh 1 trong vòng thứ hai. 

Cuối cùng, hãy xem xét một chuỗi:```
5 4
1 2
2 3
3 4
4 5
```Vòng 1 loại bỏ đỉnh 1 và 5. 

Bằng cấp trở thành`[0, 1, 2, 1, 0]`. 

Vòng 2 loại bỏ đỉnh 2 và 4. 

Bằng cấp trở thành`[0, 0, 0, 0, 0]`. 

Đỉnh 3 không bao giờ trở thành cấp 1 vì cả hai đỉnh lân cận đều biến mất trong cùng một vòng. Quá trình dừng lại với câu trả lời 2. 

Điều này khẳng định tầm quan trọng của việc cập nhật đồng thời.
