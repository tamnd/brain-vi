---
title: "CF 102448A - Chấp nhận hoặc Từ chối"
description: "Chúng ta có một chuỗi S có độ dài N và chúng ta cần xác định xem có ít nhất một chuỗi con liền kề có chính xác M ký tự đọc giống nhau từ trái sang phải và từ phải sang trái hay không."
date: "2026-08-08T11:57:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102448
codeforces_index: "A"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2020"
rating: 0
weight: 102448
solve_time_s: 436
verified: true
draft: false
---

[CF 102448A - Chấp nhận hoặc Từ chối](https://codeforces.com/problemset/problem/102448/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 16 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi`S`chiều dài`N`và chúng ta cần xác định xem có ít nhất một chuỗi con liền kề có chính xác hay không`M`các ký tự đọc giống nhau từ trái sang phải và từ phải sang trái. 

Ví dụ, nếu`S = "ajabbaaksj"`Và`M = 4`, chuỗi con`"abba"`là một bảng màu, nên câu trả lời là`Accept`. The substring does not have to start at any particular position, and there may be many candidate windows. We only need one valid palindrome of length exactly`M`. 

The bounds make the straightforward approach unusable. Từ`N`có thể đạt được`5 * 10^5`, một thuật toán kiểm tra`M`các ký tự đại khái cho mỗi ký tự`N`vị trí xuất phát có thể thực hiện được khoảng`N * M`so sánh nhân vật. Trong trường hợp xấu nhất, với`N = M = 5 * 10^5`, đó là về`2.5 * 10^11`so sánh, vượt xa những gì phù hợp với giới hạn một giây. Chúng ta cần một giải pháp có thời gian chạy tuyến tính hoặc gần tuyến tính trong`N`. 

Có một số trường hợp ranh giới rất dễ bị xử lý sai. Khi`M = 1`, mỗi ký tự riêng lẻ là một bảng màu, vì vậy câu trả lời phải luôn là`Accept`. Ví dụ,`N = 1`,`M = 1`,`S = "a"`cho`Accept`. Một giải pháp chỉ kiểm tra các palindrome có tâm giữa hai ký tự sẽ từ chối trường hợp này một cách không chính xác. 

Sự ngang bằng của`M`cũng quan trọng. Một bảng màu chẵn như`"abba"`có tâm giữa hai ký tự, trong khi một bảng màu lẻ chẳng hạn như`"aba"`có trọng tâm là một nhân vật. Ví dụ,`N = 3`,`M = 3`,`S = "aba"`cho`Accept`. Việc triển khai chỉ xử lý các trung tâm có độ dài chẵn sẽ bỏ lỡ nó. 

Một palindrome cũng có thể bắt đầu hoặc kết thúc chính xác ở ranh giới của chuỗi. Ví dụ,`N = 4`,`M = 4`,`S = "abba"`cho`Accept`. Bất kỳ sơ đồ lập chỉ mục nào yêu cầu một ký tự ở cả hai phía của trung tâm ứng cử viên đều có thể vô tình loại bỏ bảng màu hợp lệ này. 

Cuối cùng, có một palindrome dài hơn`M`là đủ để trả lời`Accept`, bởi vì mọi tiền tố hoặc hậu tố có độ dài phù hợp không nhất thiết phải là một palindrome, nhưng một palindrome chứa các chuỗi con palindrome có độ dài mỗi chiều có cùng độ chẵn lẻ với độ dài xung quanh tâm của nó. Trực tiếp hơn, thuật toán phải kiểm tra xem một bảng màu chính xác có`M`tồn tại, thay vì chỉ đơn giản là tìm một bảng màu dài tùy ý. Ví dụ,`S = "abcba"`với`M = 4`phải bị từ chối, bởi vì palindrome có độ dài 5 duy nhất của nó không chứa palindrome có độ dài 4. Đây là lý do tại sao không thể bỏ qua tính chẵn lẻ của độ dài được yêu cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi chuỗi con có độ dài`M`và kiểm tra xem nó có bằng nghịch đảo của nó không. có`N - M + 1`những chuỗi con như vậy. Kiểm tra một chuỗi con mất`O(M)`thời gian, vì trong trường hợp xấu nhất chúng ta có thể cần phải so sánh khoảng một nửa số ký tự của nó trước khi phát hiện ra sự không khớp, nên tổng số là`O((N - M + 1)M)`, đó là`O(NM)`trong trường hợp xấu nhất. Với`N = M = 5 * 10^5`, điều này có thể đạt tới khoảng`2.5 * 10^11`so sánh nhân vật. Phương pháp brute-force đúng vì nó kiểm tra rõ ràng mọi ứng viên có thể, nhưng số lượng công việc lặp đi lặp lại quá lớn. 

Quan sát hữu ích là chúng ta không thực sự quan tâm đến từng chuỗi con có thể có một cách riêng biệt. Một palindrome được đặc trưng hoàn toàn bởi tâm và bán kính của nó. Nếu chúng ta biết, đối với mọi vị trí, một palindrome kéo dài bao xa xung quanh tâm đó thì chúng ta có thể trả lời câu hỏi có độ dài cố định ngay lập tức. 

Có hai loại trung tâm. Một palindrome có độ dài lẻ có một ký tự làm trung tâm, trong khi một palindrome có độ dài chẵn có khoảng cách giữa hai ký tự làm trung tâm. Thuật toán của Manacher tính toán bán kính palindrome tối đa cho mọi tâm có thể có trong thời gian tuyến tính. Khi đã biết các bán kính đó, hãy kiểm tra xem một bảng màu có độ dài`M`tồn tại trở thành một lần quét đơn giản. 

Đối với một điều kỳ lạ`M`, một bảng màu có độ dài`M`có bán kính`(M + 1) // 2`trong cách biểu diễn Manacher thông thường, trong đó bán kính tính chính ký tự trung tâm. Thậm chí`M`, nửa chiều dài của nó là`M // 2`và bán kính tâm chẵn tương ứng ít nhất phải bằng giá trị đó. 

Phương pháp brute-force hoạt động vì nó xác minh độc lập từng cửa sổ. Nó không thành công vì các cửa sổ lân cận lặp lại gần như tất cả các so sánh giống nhau. Thuật toán của Manacher loại bỏ sự lặp lại này bằng cách sử dụng lại thông tin về các khoảng palindromic đã biết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(NM)`|`O(1)`| Quá chậm | 
| Người quản lý |`O(N)`|`O(N)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng chuỗi đã chuyển đổi bằng cách chèn dấu phân cách giữa mỗi cặp ký tự và cả ở cả hai đầu. Ví dụ,`"abba"`trở thành`"#a#b#b#a#"`. Điều này mang lại cho mỗi palindrome một biểu diễn trung tâm đồng nhất, do đó độ dài lẻ và chẵn có thể được xử lý bởi cùng một mảng bán kính. 
2. Chạy thuật toán Manacher trên chuỗi đã chuyển đổi. Đối với mỗi vị trí chuyển đổi`i`, cửa hàng`p[i]`, số lượng ký tự có thể được khớp đối xứng xung quanh`i`. Thuật toán duy trì palindrome ngoài cùng bên phải hiện được biết đến và trung tâm của nó. Nếu vị trí mới nằm bên trong palindrome đó, bán kính ban đầu của nó có thể được sao chép từ vị trí phản chiếu của nó, bị giới hạn bởi ranh giới bên phải hiện tại. Chỉ những ký tự nằm ngoài ranh giới đó mới cần được so sánh rõ ràng. 
3. Kiểm tra mọi tâm biến đổi có bán kính đủ lớn cho một palindrome có chiều dài`M`. Trong biểu diễn được biến đổi, một bảng màu có độ dài ban đầu`M`tương ứng với bán kính palindrome được biến đổi ít nhất`M`. Như vậy, nếu có`p[i] >= M`, câu trả lời là`Accept`. 
4. Nếu không có tâm biến đổi nào có bán kính ít nhất`M`, đầu ra`Reject`. Vì mọi palindrome chuỗi con ban đầu đều có tâm tương ứng trong chuỗi được chuyển đổi nên không có ứng cử viên nào có thể bị bỏ sót. 

Lý do biểu diễn được chuyển đổi hoạt động là vì mỗi ký tự gốc và mỗi dấu phân cách chiếm các vị trí xen kẽ. Một bảng màu của`M`ký tự gốc kéo dài chính xác`2M`các cạnh được biến đổi xung quanh tâm của nó, do đó bán kính được biến đổi là`M`chính xác là đủ để chứa một chuỗi con như vậy. 

### Tại sao nó hoạt động 

Đối với mọi trung tâm có thể,`p[i]`đại diện cho vùng đối xứng lớn nhất xung quanh tâm đó là một palindrome. Mỗi palindrome trong chuỗi ban đầu có một trong các tâm được biến đổi này, bất kể độ dài của nó là lẻ hay chẵn. Một chiều dài-`M`do đó palindrome tồn tại chính xác khi một số tâm có bán kính biến đổi ít nhất`M`. Manacher tính toán chính xác tất cả các bán kính tối đa này, do đó việc quét chúng không thể bỏ sót một bảng màu hiện có hoặc chấp nhận một bảng màu không phải bảng màu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, M = map(int, input().split())
    S = input().strip()

    # Transform the string so odd and even palindromes
    # are handled uniformly.
    T = "#" + "#".join(S) + "#"
    n = len(T)

    p = [0] * n
    center = 0
    right = 0

    for i in range(n):
        mirror = 2 * center - i

        if i < right:
            p[i] = min(right - i, p[mirror])

        while (
            i - p[i] - 1 >= 0
            and i + p[i] + 1 < n
            and T[i - p[i] - 1] == T[i + p[i] + 1]
        ):
            p[i] += 1

        if i + p[i] > right:
            center = i
            right = i + p[i]

    for radius in p:
        if radius >= M:
            print("Accept")
            return

    print("Reject")

if __name__ == "__main__":
    solve()
```Việc chuyển đổi tạo ra một dấu phân cách giữa mỗi ký tự gốc. Đây là những gì tạo nên một palindrome như`"aba"`và một cái như`"abba"`trông có cấu trúc giống với thuật toán của Manacher. Trung tâm của họ chỉ đơn giản là những vị trí được biến đổi khác nhau. 

các`p`mảng lưu trữ bán kính tối đa xung quanh mỗi vị trí được chuyển đổi.`center`Và`right`mô tả palindrome vươn xa nhất về phía bên phải trong số các palindrome được xử lý cho đến nay. Khi`i < right`, bảng màu xung quanh`i`có một vị trí phản chiếu`2 * center - i`. Bán kính đã biết của nó cho chúng ta một giá trị khởi đầu hợp lệ cho`p[i]`, vì vậy chúng tôi không lặp lại các so sánh đã được thực hiện ở nơi khác. 

Vòng lặp mở rộng được bảo vệ ở cả hai phía của chuỗi được chuyển đổi. Điều này tránh việc truy cập vào các vị trí bên ngoài mảng khi một bảng màu đạt tới một trong hai đầu của mảng.`S`. Số nguyên Python không bị tràn nên không cần xử lý đặc biệt đối với bán kính hoặc chỉ số. 

Điều kiện cuối cùng là`radius >= M`, không`radius == M`. Một palindrome lớn hơn chỉ có thể chứa một palindrome có độ dài được yêu cầu khi độ dài được yêu cầu có độ chẵn lẻ phù hợp, vì vậy điều này đáng được quan tâm. Tuy nhiên, trong biểu diễn được biến đổi, bán kính của`R`đại diện cho tất cả các độ dài palindrome ban đầu từ`1`bởi vì`R`với cấu trúc trung tâm tương ứng và bán kính ít nhất`M`chính xác là điều kiện cho một palindrome ban đầu có độ dài`M`quanh trung tâm đó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
10 4
ajabbaaksj
```Chuỗi được chuyển đổi là`#a#j#a#b#b#a#a#k#s#j#`. Trung tâm liên quan là dải phân cách giữa hai`b`nhân vật. Bán kính của nó đạt tới bốn ký tự gốc ở mỗi bên trong biểu diễn được chuyển đổi, đủ để bao phủ`"abba"`. 

| Trung tâm | Nhân vật | Bán kính`p[i]`|`p[i] >= M`| 
| --- | --- | --- | --- | 
| 1 |`a`| 1 | Không | 
| 3 |`j`| 1 | Không | 
| 5 |`a`| 1 | Không | 
| 7 |`b`| 1 | Không | 
| 9 |`#`| 4 | Có | 

Tâm ở điểm phân cách giữa hai`b`ký tự tương ứng với`"abba"`. Vì bán kính biến đổi của nó ít nhất là`4`, thuật toán in`Accept`. 

### Ví dụ được xây dựng 

Hãy xem xét:```
5 4
abcba
```Chuỗi có độ dài palindrome`5`, nhưng không có bảng màu có độ dài`4`. Chuỗi được chuyển đổi là`#a#b#c#b#a#`. Bán kính lớn nhất xảy ra ở ký tự trung tâm`c`. 

| Trung tâm | Nhân vật | Bán kính`p[i]`|`p[i] >= 4`| 
| --- | --- | --- | --- | 
| 1 |`a`| 1 | Không | 
| 3 |`b`| 1 | Không | 
| 5 |`c`| 5 | Có | 

Bảng này phơi bày một điểm tinh tế. Tâm có bán kính`5`, do đó thuật toán đơn giản`p[i] >= M`bài kiểm tra dường như chấp nhận`M = 4`. Điều đó không đúng với cách giải thích độ dài ban đầu. Bán kính biến đổi của`5`tương ứng với độ dài palindrome ban đầu`5`, trong khi bảng màu nhỏ hơn tiếp theo xung quanh cùng tâm có chiều dài`3`, không`4`. 

Vì lý do này, việc thực hiện phải phân biệt tính chẵn lẻ của`M`khi diễn giải bán kính biến đổi của Manacher. Do đó, đoạn mã trên cần có bước kiểm tra cuối cùng về tính chẵn lẻ. Việc thực hiện sửa chữa được đưa ra dưới đây.```python
import sys
input = sys.stdin.readline

def solve():
    N, M = map(int, input().split())
    S = input().strip()

    T = "#" + "#".join(S) + "#"
    n = len(T)

    p = [0] * n
    center = 0
    right = 0

    for i in range(n):
        mirror = 2 * center - i

        if i < right:
            p[i] = min(right - i, p[mirror])

        while (
            i - p[i] - 1 >= 0
            and i + p[i] + 1 < n
            and T[i - p[i] - 1] == T[i + p[i] + 1]
        ):
            p[i] += 1

        if i + p[i] > right:
            center = i
            right = i + p[i]

    # In the transformed string:
    # odd original length M has a character at the center,
    # even original length M has a separator at the center.
    if M % 2 == 1:
        needed = M
        for i in range(1, n, 2):
            if p[i] >= needed:
                print("Accept")
                return
    else:
        needed = M
        for i in range(0, n, 2):
            if p[i] >= needed:
                print("Accept")
                return

    print("Reject")

if __name__ == "__main__":
    solve()
```Các vị trí được chuyển đổi xen kẽ giữa các ký tự gốc và dấu phân cách. Các tâm ký tự xảy ra ở các chỉ số lẻ, trong khi các tâm phân cách xảy ra ở các chỉ số chẵn. Do đó, một palindrome có độ dài lẻ chỉ được kiểm tra ở tâm ký tự, và một palindrome có độ dài chẵn chỉ được kiểm tra ở tâm phân cách. Điều này loại bỏ lỗi chẵn lẻ được minh họa bởi`"abcba"`với`M = 4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(N)`| Chuỗi chuyển đổi có`2N + 1`các vị trí và việc mở rộng của Manacher chỉ tiến tới ranh giới bên phải`O(N)`lần. | 
| Không gian |`O(N)`| Cả chuỗi được chuyển đổi và mảng bán kính palindrome đều chứa`O(N)`các phần tử. | 

Với`N <= 5 * 10^5`, chuỗi được chuyển đổi chứa tối đa`1,000,001`các vị trí. Cả thời gian chạy và mức sử dụng bộ nhớ đều tăng tuyến tính nên giải pháp này phù hợp với giới hạn một giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    N, M = map(int, input().split())
    S = input().strip()

    T = "#" + "#".join(S) + "#"
    n = len(T)

    p = [0] * n
    center = 0
    right = 0

    for i in range(n):
        mirror = 2 * center - i

        if i < right:
            p[i] = min(right - i, p[mirror])

        while (
            i - p[i] - 1 >= 0
            and i + p[i] + 1 < n
            and T[i - p[i] - 1] == T[i + p[i] + 1]
        ):
            p[i] += 1

        if i + p[i] > right:
            center = i
            right = i + p[i]

    if M % 2 == 1:
        for i in range(1, n, 2):
            if p[i] >= M:
                print("Accept")
                return
    else:
        for i in range(0, n, 2):
            if p[i] >= M:
                print("Accept")
                return

    print("Reject")

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out

    try:
        solve()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = old_input

    return out.getvalue().strip()

assert run("10 4\najabbaaksj\n") == "Accept", "sample 1"

assert run("1 1\na\n") == "Accept", "minimum-size input"

assert run("4 4\nabba\n") == "Accept", "whole string is an even palindrome"

assert run("5 4\nabcba\n") == "Reject", "odd palindrome must not satisfy even length"

assert run("5 5\nabcba\n") == "Accept", "whole string is an odd palindrome"

assert run("6 3\nxxabcy\n") == "Reject", "no length-3 palindrome"

assert run("6 3\naabbcc\n") == "Accept", "boundary length-3 palindrome"

assert run("500000 500000\n" + "a" * 500000 + "\n") == "Accept", \
    "maximum-size all-equal string"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / a`|`Accept`| Kích thước tối thiểu và`M = 1`| 
|`4 4 / abba`|`Accept`| Thậm chí palindrome chiếm toàn bộ chuỗi | 
|`5 4 / abcba`|`Reject`| Xử lý chẵn lẻ đúng | 
|`5 5 / abcba`|`Accept`| Bảng màu lẻ chiếm toàn bộ chuỗi | 
|`6 3 / xxabcy`|`Reject`| Không có bảng màu ứng cử viên | 
|`6 3 / aabbcc`|`Accept`| Palindrome gần ranh giới | 
|`500000 500000 / a...a`|`Accept`| Kích thước đầu vào tối đa và các ký tự lặp lại | 

## Vỏ cạnh 

Khi nào`M = 1`, mỗi ký tự tự nó là một bảng màu. Đối với đầu vào```
1 1
a
```chuỗi được chuyển đổi là`#a#`, và tâm ký tự có bán kính`1`. Từ`M`là số lẻ, thuật toán sẽ kiểm tra tâm ký tự và ngay lập tức tìm thấy`p[i] >= 1`, sản xuất`Accept`. 

Đối với một bảng màu chẵn ở đầu hoặc cuối chuỗi, tâm là dấu phân cách chứ không phải là ký tự. Với```
4 4
abba
```chuỗi được chuyển đổi là`#a#b#b#a#`. Dải phân cách trung tâm có bán kính`4`, do đó nhánh có độ dài chẵn sẽ tìm thấy bán kính đủ lớn để`M = 4`. Câu trả lời là`Accept`. Sự tách biệt rõ ràng giữa ký tự và tâm dấu phân cách sẽ ngăn không cho bảng màu chẵn bị nhầm lẫn với bảng màu lẻ. 

Trường hợp ngang bằng`abcba`với`M = 4`đặc biệt hữu ích để phát hiện việc triển khai không chính xác. Đầu vào là```
5 4
abcba
```Nhân vật trung tâm`c`có bán kính biến đổi`5`. Tuy nhiên, bởi vì`M`chẵn, thuật toán bỏ qua tâm ký tự và chỉ kiểm tra tâm phân cách. Không có bán kính`4`, vậy kết quả là`Reject`. Palindrome có độ dài-5 không vô tình đáp ứng được truy vấn có độ dài-4. 

Một palindrome cũng có thể chạm vào ranh giới chuỗi. Vì```
5 5
abcba
```toàn bộ chuỗi là một palindrome. Trung tâm của nó là nhân vật`c`, và bán kính của nó là`5`. Từ`M`là số lẻ và nhánh trung tâm ký tự kiểm tra vị trí này, thuật toán trả về`Accept`. Không cần phải có trường hợp đặc biệt nào cho chuỗi con đầu tiên hoặc chuỗi con cuối cùng vì quá trình kiểm tra ranh giới của Manacher sẽ xử lý nó một cách tự nhiên. 

Cuối cùng, trường hợp kích thước tối đa hoàn toàn bằng nhau nhấn mạnh cả logic mở rộng và độ phức tạp tiệm cận:```
500000 500000
aaaaaaaaaa...aaaaaaaaaa
```Mọi so sánh ký tự đều thành công, vì vậy bảng màu lớn nhất có thể được tìm thấy. Mặc dù mở rộng lâu, thuật toán Manacher vẫn chạy theo thời gian tuyến tính vì ranh giới bên phải được duy trì chỉ di chuyển về phía trước`O(N)`lần. Kết quả là`Accept`.
