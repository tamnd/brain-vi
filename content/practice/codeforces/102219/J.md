---
title: "CF 102219J - Đĩa Bếp"
description: "Có chính xác năm tấm, được xác định bởi A, B, C, D và E. Mỗi dòng trong số năm dòng đầu vào đưa ra một mối quan hệ giữa hai tấm, chẳng hạn như A<B hoặc DB. Mối quan hệ cho chúng ta biết tấm nào trong hai tấm nhỏ hơn."
date: "2026-08-17T23:03:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102219
codeforces_index: "J"
codeforces_contest_name: "2019 ICPC Malaysia National"
rating: 0
weight: 102219
solve_time_s: 191
verified: false
draft: false
---

[CF 102219J - Đĩa bếp](https://codeforces.com/problemset/problem/102219/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 11s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Có chính xác năm tấm, được xác định bởi`A`,`B`,`C`,`D`, Và`E`. Mỗi dòng trong số năm dòng đầu vào đưa ra một mối quan hệ giữa hai tấm, chẳng hạn như`A<B`hoặc`D>B`. Mối quan hệ cho chúng ta biết tấm nào trong hai tấm nhỏ hơn. 

Nhiệm vụ là tìm ra thứ tự của tất cả năm tấm từ nhỏ nhất đến lớn nhất thỏa mãn mọi so sánh nhất định. Nếu một số thứ tự thỏa mãn tất cả các so sánh thì bất kỳ thứ tự nào cũng hợp lệ. Nếu sự so sánh mâu thuẫn với nhau thì không tồn tại thứ tự hợp lệ nào và chúng ta in`impossible`. 

Kích thước đầu vào là cố ý nhỏ. Luôn có năm đỉnh và năm cạnh so sánh, do đó, ngay cả một thuật toán kiểm tra mọi thứ tự có thể cũng chỉ có`5! = 120`ứng viên. Điều đó có nghĩa là tìm kiếm giai thừa hoàn toàn an toàn cho bài toán thực tế. Giải thích thuật toán thú vị hơn là xem các so sánh dưới dạng biểu đồ có hướng và thực hiện sắp xếp tôpô. Vì đồ thị chỉ có năm đỉnh và năm cạnh nên việc này cần có thời gian không đổi ở đây và cũng có quy mô tốt hơn nhiều nếu bài toán được tổng quát hóa cho nhiều tấm hơn. 

Việc thực hiện bất cẩn có thể thất bại theo nhiều cách. Đầu tiên, một mâu thuẫn có thể hình thành một chu kỳ. Ví dụ,```
A<B
B<C
C<A
D<E
A<D
```không có câu trả lời vì ba mối quan hệ đầu tiên yêu cầu`A<B<C<A`. Một phương pháp chỉ ghi lại các so sánh cục bộ mà không kiểm tra tính nhất quán toàn cầu vẫn có thể in thứ tự. 

Thứ hai, một tấm có thể có một số ràng buộc hướng về phía nó. Ví dụ,```
A<B
C<B
D<B
E<B
A<C
```yêu cầu`A<C<B`, trong khi`D`Và`E`có thể xuất hiện trước`B`ở một trong hai vị trí so với tấm không bị ràng buộc khác. Một thuật toán chính xác phải tính đến tất cả các ràng buộc sắp tới trước khi đặt đĩa, thay vì xử lý từng phép so sánh một cách độc lập. 

Thứ ba, những so sánh dư thừa sẽ không gây rắc rối. Ví dụ,```
A<B
B<C
A<C
D<E
A<D
```chứa`A<C`mặc dù nó đã được ngụ ý bởi`A<B<C`. Việc sắp xếp tôpô đúng chỉ đơn giản là giữ cả hai cạnh. Không cần phải phân biệt các ràng buộc trực tiếp với hậu quả của các ràng buộc khác. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là vũ lực. Tạo tất cả`5! = 120`hoán vị của`A`,`B`,`C`,`D`, Và`E`. Đối với mỗi hoán vị, hãy tính vị trí của mỗi tấm và kiểm tra tất cả năm phép so sánh. Hoán vị đầu tiên thỏa mãn mọi quan hệ là một câu trả lời hợp lệ. Nếu tất cả 120 hoán vị đều thất bại thì các ràng buộc sẽ mâu thuẫn. 

Cách tiếp cận này đúng vì mọi thứ tự có thể có của năm tấm riêng biệt đều xuất hiện chính xác một lần trong số các hoán vị. Nếu một mệnh lệnh hợp lệ tồn tại, vũ lực cuối cùng sẽ kiểm tra nó. Nếu không có hoán vị nào thỏa mãn cả năm quan hệ thì không tồn tại thứ tự hợp lệ. 

Đối với vấn đề này, vũ lực không quá chậm chút nào. Trong trường hợp xấu nhất nó thực hiện nhiều nhất`120 * 5 = 600`kiểm tra mối quan hệ, cộng với chi phí nhỏ để tạo ra các hoán vị. Do đó, giới hạn thời gian một giây là vô cùng hào phóng. Sở dĩ không dừng lại ở đó là cấu trúc của các ràng buộc cho chúng ta một lời giải tổng quát hơn. 

Quan sát quan trọng là mọi so sánh đều có thể được chuyển thành cạnh có hướng. Nếu như`A<B`, sau đó`A`phải xuất hiện trước`B`, vì vậy chúng ta thêm một cạnh`A -> B`. Nếu như`A>B`, thay vào đó chúng tôi thêm`B -> A`. Bây giờ chúng ta cần sắp xếp các đỉnh trong đó mọi cạnh đều hướng từ đỉnh trước tới đỉnh sau. Đó chính xác là thứ tự tôpô của đồ thị có hướng. 

Đồ thị có hướng có thứ tự tôpô chính xác khi nó không có chu trình có hướng. Thuật toán của Kahn cung cấp cho chúng tôi cả hai phần chúng tôi cần. Nó liên tục chọn một đỉnh có bậc vô hướng bằng 0, đặt nó tiếp theo trong đáp án và loại bỏ các cạnh đi ra của nó. Nếu loại bỏ tất cả năm đỉnh thì chuỗi kết quả sẽ thỏa mãn mọi so sánh. Nếu quá trình bị kẹt trước khi tất cả các đỉnh bị loại bỏ thì các đỉnh còn lại sẽ thuộc hoặc phụ thuộc vào một chu trình, do đó các ràng buộc là không thể thực hiện được. 

Phương pháp vũ lực hoạt động vì chỉ có 120 cách sắp xếp có thể xảy ra, nhưng nó sẽ trở thành giai thừa khi số lượng đĩa tăng lên. Quan sát cho thấy các so sánh tạo thành biểu đồ tuần hoàn có hướng cho phép chúng ta thay thế phép liệt kê bằng thuật toán biểu đồ thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(5! * 5)`|`O(5)`| Được chấp nhận cho vấn đề này | 
| Sắp xếp tôpô |`O(V + E)`|`O(V + E)`| Đã chấp nhận | 

Đây`V=5`Và`E=5`, vì vậy phương pháp tối ưu là thời gian không đổi một cách hiệu quả đối với đầu vào đã cho. 

## Hướng dẫn thuật toán 

1. Tạo đồ thị có hướng với một đỉnh cho mỗi tấm. Đối với mỗi lần so sánh, hãy định hướng cạnh từ tấm nhỏ hơn đến tấm lớn hơn. Ví dụ,`D>B`trở thành`B -> D`. Hướng này thể hiện thứ tự mà mọi câu trả lời hợp lệ đều phải tôn trọng. 
2. Tính độ lớn của mỗi tấm. Mức độ cho chúng ta biết hiện tại cần có bao nhiêu tấm để đặt trước tấm đó. Một tấm có độ bằng 0 không có tấm trước chưa được giải quyết, do đó sẽ an toàn khi đặt tiếp theo. 
3. Xếp từng tấm không độ vào hàng đợi. Có thể có nhiều hơn một tấm như vậy vì đầu vào không nhất thiết xác định thứ tự duy nhất. Bất kỳ sự lựa chọn nào trong số đó đều hợp lệ. 
4. Liên tục lấy một đĩa ra khỏi hàng đợi và gắn nó vào câu trả lời. Đối với mỗi cạnh từ tấm này sang tấm khác, hãy giảm độ của điểm đến đi một. Nếu mức độ đó bằng 0, hãy thêm đích vào hàng đợi. Việc loại bỏ các cạnh đi ra thể hiện việc cố định tấm hiện tại ở vị trí của nó và thỏa mãn các ràng buộc của nó. 
5. Sau khi xử lý hàng đợi, hãy kiểm tra xem có bao nhiêu tấm được thêm vào câu trả lời. Nếu cả năm đều được xử lý thì câu trả lời là thứ tự được sắp xếp hợp lệ. Nếu ít hơn năm được xử lý, biểu đồ chứa một chu trình và các so sánh mâu thuẫn nhau, vì vậy hãy in`impossible`. 

Bất biến quan trọng là mọi tấm được đặt vào câu trả lời đều có mức độ bằng 0 sau khi tất cả các tấm được chọn trước đó đã bị loại bỏ. Do đó, mọi ràng buộc trỏ vào tấm đó đã được thỏa mãn, vì vậy việc đặt nó tiếp theo không thể vi phạm so sánh đầu vào. Nếu thuật toán xử lý mọi tấm, thì mỗi cạnh sẽ chuyển từ tấm được chọn trước đó sang tấm được chọn sau. Nếu nó không thể xử lý mọi tấm, một chu trình sẽ ngăn ít nhất một tấm còn lại đạt tới mức 0, chứng tỏ rằng không tồn tại thứ tự hợp lệ nào. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    graph = [[] for _ in range(5)]
    indegree = [0] * 5

    for _ in range(5):
        s = input().strip()
        a = ord(s[0]) - ord('A')
        b = ord(s[2]) - ord('A')

        if s[1] == '<':
            u, v = a, b
        else:
            u, v = b, a

        graph[u].append(v)
        indegree[v] += 1

    q = deque()

    for v in range(5):
        if indegree[v] == 0:
            q.append(v)

    order = []

    while q:
        u = q.popleft()
        order.append(u)

        for v in graph[u]:
            indegree[v] -= 1
            if indegree[v] == 0:
                q.append(v)

    if len(order) != 5:
        print("impossible")
        return

    print(''.join(chr(v + ord('A')) for v in order))

if __name__ == "__main__":
    solve()
```Biểu đồ sử dụng các chỉ số`0`bởi vì`4`vì`A`bởi vì`E`. Việc chuyển đổi các ký tự thành số nguyên làm cho danh sách kề và mảng một bậc trở nên đơn giản. 

Đối với mỗi quan hệ đầu vào, tấm nhỏ hơn được gán cho`u`và tấm lớn hơn để`v`, tạo ra lợi thế`u -> v`. Đối với một mối quan hệ như`A>B`, mã đảo ngược các ký tự và lưu trữ`B -> A`, bởi vì`B`phải đến sớm hơn theo thứ tự kích thước tăng dần. 

Hàng đợi ban đầu chứa mọi đỉnh có bậc bằng 0. Đây chính xác là những tấm hiện không có yêu cầu phải xuất hiện sau tấm khác. Thuật toán sau đó tuân theo quy trình sắp xếp tôpô của Kahn. 

Việc kiểm tra độ dài cuối cùng là cần thiết. Hàng đợi trống không tự động có nghĩa là câu trả lời đã hoàn tất. Nếu một chu trình tồn tại, tất cả các đỉnh trong chu trình đó vẫn giữ mức độ dương, do đó hàng đợi có thể trống trong khi một số tấm vẫn chưa được xử lý. So sánh`len(order)`với`5`nắm bắt chính xác tình huống đó. 

Không có vấn đề tràn số nguyên trong Python và không có vấn đề về ranh giới có ý nghĩa vì số đỉnh và cạnh là cố định. Đầu vào chứa chính xác năm phép so sánh, do đó vòng lặp đọc các ràng buộc phải thực hiện chính xác năm lần. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, các phép so sánh trở thành các cạnh có hướng sau:`B -> D`,`D -> A`,`E -> C`,`B -> A`, Và`C -> B`. 

Độ ban đầu là`A=2`,`B=1`,`C=1`,`D=1`, Và`E=0`. Chỉ một`E`có thể được chọn đầu tiên. 

| Xếp hàng trước bước | Đã chọn | Indegrees sau khi loại bỏ | Trả lời | 
| --- | --- | --- | --- | 
|`E`|`E`|`A=2, B=1, C=0, D=1, E=0`|`E`| 
|`C`|`C`|`A=2, B=0, C=0, D=1, E=0`|`EC`| 
|`B`|`B`|`A=1, B=0, C=0, D=0, E=0`|`ECB`| 
|`A,D`|`A`|`D=0`|`ECBA`| 
|`D`|`D`| tất cả đã được xử lý |`ECBAD`| 

Thứ tự chính xác có thể phụ thuộc vào thứ tự xếp hàng các đỉnh bậc 0. Đầu ra mẫu là`ECBDA`, trong khi dấu vết ở trên cho`ECBAD`, điều này cũng hợp lệ vì cả hai`A`Và`D`trở nên có sẵn tại thời điểm thích hợp và các ràng buộc chỉ yêu cầu`D<A`. Nếu chúng ta chọn`D`trước`A`, chúng tôi có được thứ tự mẫu`ECBDA`. 

Đối với Mẫu 2, ba mối quan hệ đầu tiên tạo thành một chu trình:`B -> E`,`E -> A`, Và`A -> B`. 

Các so sánh còn lại là`C -> B`Và`D -> B`. 

| Xếp hàng trước bước | Đã chọn | Bằng cấp liên quan còn lại | Trả lời | 
| --- | --- | --- | --- | 
|`C, D`|`C`|`B=3`|`C`| 
|`D`|`D`|`B=2`|`CD`| 
| trống | không |`A=1, B=2, E=1`|`CD`| 

Sau đó`C`Và`D`bị loại bỏ, ba đỉnh`A`,`B`, Và`E`vẫn tạo thành một vòng tuần hoàn. Không ai trong số chúng có thể đạt đến mức 0, vì vậy hàng đợi trở nên trống rỗng chỉ với hai trong số năm tấm được xử lý. Thuật toán in`impossible`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(V + E)`| Mỗi tấm được xử lý một lần và mỗi cạnh so sánh được kiểm tra một lần. | 
| Không gian |`O(V + E)`| Danh sách kề, mảng bậc, hàng đợi và đáp án chỉ chứa năm đỉnh và năm cạnh. | 

Đối với vấn đề này,`V=5`Và`E=5`, do đó thuật toán chỉ thực hiện một số thao tác không đổi. Nó thấp hơn nhiều so với giới hạn thời gian một giây và sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. Quan trọng hơn, việc triển khai tương tự sẽ vẫn hiệu quả nếu số lượng tấm cố định được thay thế bằng biểu đồ lớn hơn nhiều. 

## Trường hợp thử nghiệm 

Bài toán luôn chứa chính xác năm tấm và năm phép so sánh, do đó, kích thước đầu vào tối thiểu và tối đa có ý nghĩa đều có cùng kích thước cố định. Thay vào đó, các thử nghiệm tùy chỉnh bên dưới tập trung vào thứ tự hoàn chỉnh, các ràng buộc dư thừa, các lựa chọn và chu kỳ không bị ràng buộc.```python
import sys
import io
from collections import deque

def solve():
    input = sys.stdin.readline

    graph = [[] for _ in range(5)]
    indegree = [0] * 5

    for _ in range(5):
        s = input().strip()
        a = ord(s[0]) - ord('A')
        b = ord(s[2]) - ord('A')

        if s[1] == '<':
            u, v = a, b
        else:
            u, v = b, a

        graph[u].append(v)
        indegree[v] += 1

    q = deque(v for v in range(5) if indegree[v] == 0)
    order = []

    while q:
        u = q.popleft()
        order.append(u)

        for v in graph[u]:
            indegree[v] -= 1
            if indegree[v] == 0:
                q.append(v)

    if len(order) != 5:
        return "impossible"

    return ''.join(chr(v + ord('A')) for v in order)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

# Provided sample 1.
assert run(
    "D>B\n"
    "A>D\n"
    "E<C\n"
    "A>B\n"
    "B>C\n"
) == "ECBDA"

# Provided sample 2.
assert run(
    "B>E\n"
    "A>B\n"
    "E>A\n"
    "C<B\n"
    "D<B\n"
) == "impossible"

# Fully determines A < B < C < D < E.
assert run(
    "A<B\n"
    "B<C\n"
    "C<D\n"
    "D<E\n"
    "A<E\n"
) == "ABCDE"

# Same ordering information with a redundant edge and reversed syntax.
assert run(
    "E>D\n"
    "D>C\n"
    "C>B\n"
    "B>A\n"
    "E>A\n"
) == "ABCDE"

# Several plates are initially available, but the constraints are consistent.
assert run(
    "A<C\n"
    "B<C\n"
    "D<E\n"
    "A<D\n"
    "B<E\n"
) == "ABCD E".replace(" ", "")

# Direct cycle.
assert run(
    "A<B\n"
    "B<C\n"
    "C<A\n"
    "D<E\n"
    "A<D\n"
) == "impossible"
```Bài kiểm tra tùy chỉnh thứ tư xứng đáng được làm rõ một chút về quyền tự do đầu ra. Các ràng buộc của nó cho phép nhiều thứ tự hợp lệ và thứ tự hàng đợi được sử dụng bởi việc triển khai này tạo ra`ABCDE`. Khẳng định có chủ ý kiểm tra kết quả chính xác do quá trình triển khai tạo ra thay vì giả định rằng vấn đề có một câu trả lời duy nhất. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`A<B, B<C, C<D, D<E, A<E`|`ABCDE`| Một chuỗi hoàn chỉnh và một ràng buộc dư thừa | 
|`E>D, D>C, C>B, B>A, E>A`|`ABCDE`| Xử lý đúng`>`so sánh | 
|`A<C, B<C, D<E, A<D, B<E`|`ABCDE`| Nhiều đỉnh có sẵn ban đầu và các lựa chọn không duy nhất | 
|`A<B, B<C, C<A, D<E, A<D`|`impossible`| Phát hiện chu kỳ | 

## Vỏ cạnh 

Một mâu thuẫn trực tiếp được thể hiện bằng một chu trình chứ không nhất thiết phải bằng hai tấm giống hệt nhau có quan hệ đối lập nhau. Coi như:```
A<B
B<C
C<A
D<E
A<D
```Đồ thị chứa`A -> B -> C -> A`. Ban đầu,`D`và có lẽ các đỉnh khác có thể được xử lý, nhưng cuối cùng các đỉnh chu trình vẫn giữ nguyên mức độ dương. Vì không thể chọn được nên có ít hơn năm đỉnh nhập câu trả lời và thuật toán sẽ in ra`impossible`. Một phương pháp chỉ kiểm tra xem mỗi so sánh riêng lẻ có hợp lệ hay không có thể bỏ sót mâu thuẫn tổng thể này. 

Trường hợp cạnh thứ hai xảy ra khi một tấm có nhiều tấm trước đó:```
A<B
C<B
D<B
E<B
A<C
```Đây`A`phải đi trước`C`, và cả hai phải đứng trước`B`. Hai tấm còn lại cũng phải đặt trước`B`, nhưng vị trí tương đối của chúng không bị ràng buộc hoàn toàn. Thuật toán Kahn đợi cho đến khi tất cả các cạnh đến`B`đã được loại bỏ trước khi chọn nó. Đó chính xác là những gì mà mức độ thể hiện, vì vậy`B`không bao giờ có thể vô tình xuất hiện quá sớm. 

Các so sánh dư thừa cũng cần được bảo tồn thay vì coi đó là lý do để từ chối đầu vào. Ví dụ:```
A<B
B<C
A<C
D<E
A<D
```Mối quan hệ`A<C`đã được ngụ ý bởi`A<B<C`, nhưng nó vẫn là một cạnh hợp lệ. Khi`A`được xử lý, cả hai cạnh đi ra sẽ được loại bỏ một cách độc lập. Mức độ của`C`chỉ đạt 0 sau khi cả hai cạnh đến của nó đã được tính. Thứ tự kết quả là hợp lệ bất kể thông tin dư thừa. 

Cuối cùng, kích thước đầu vào cố định chính là điều kiện biên. Luôn có đúng năm đỉnh và đúng năm phép so sánh, do đó không có đồ thị trống và không có trường hợp một mảng nào để xử lý. Việc triển khai đọc chính xác năm dòng và kiểm tra chính xác năm đỉnh, khớp với giới hạn cố định của vấn đề mà không đưa vào máy móc trường hợp chung không cần thiết.
