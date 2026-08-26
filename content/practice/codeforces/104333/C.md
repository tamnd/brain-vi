---
title: "CF 104333C - Chơi với Palindrome"
description: "Chúng ta được cung cấp một chuỗi các chữ cái viết thường và với mỗi vị trí, chúng ta muốn biết “chúng ta có thể ngồi bên trong một bảng màu lớn đến mức nào” trong khi buộc vị trí đó là một phần của nó."
date: "2026-07-01T18:54:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104333
codeforces_index: "C"
codeforces_contest_name: "Replay of BU - PSTU Programming club collaborative contest"
rating: 0
weight: 104333
solve_time_s: 96
verified: false
draft: false
---

[CF 104333C - Chơi với Palindrome](https://codeforces.com/problemset/problem/104333/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các chữ cái viết thường và với mỗi vị trí, chúng ta muốn biết “chúng ta có thể ngồi bên trong một bảng màu lớn đến mức nào” trong khi buộc vị trí đó là một phần của nó. Chính xác hơn, với mỗi chỉ số$i$, chúng tôi xem xét tất cả các chuỗi con bao gồm$i$và là các palindrome và chúng ta muốn độ dài tối đa có thể có của chúng. 

Vì vậy, thay vì yêu cầu tất cả các chuỗi con palindromic trên toàn cầu, trọng tâm là cục bộ: mỗi vị trí hoạt động giống như một mỏ neo phải được bao phủ bởi palindrome đã chọn và chúng tôi muốn đoạn đối xứng dài nhất như vậy. 

Các ràng buộc rất chặt chẽ: tổng chiều dài của các trường hợp thử nghiệm lên tới$2 \cdot 10^5$, và có thể có tới$10^5$các bài kiểm tra. Điều này ngay lập tức buộc hành vi tuyến tính hoặc gần tuyến tính trên mỗi lần kiểm tra tổng thể. Bất cứ điều gì cố gắng mở rộng các palindromes một cách độc lập cho mỗi vị trí một cách ngây thơ đều có nguy cơ$O(n^2)$trong một chuỗi dài duy nhất, điều này không được chấp nhận. 

Một nỗ lực bạo lực trực tiếp sẽ thử mọi trung tâm hoặc mọi chuỗi con chứa$i$, mở rộng ra bên ngoài và kiểm tra tính chất palindromicity. Ngay cả với việc mở rộng hai con trỏ, việc thực hiện việc này một cách độc lập cho từng$i$dẫn đến công việc lặp đi lặp lại. Đối với một chuỗi như$aaaaa\ldots$, mọi vị trí sẽ cố gắng mở rộng đến toàn bộ chuỗi, dẫn đến sự lặp lại so sánh bậc hai. 

Một vài trường hợp tế nhị bộc lộ những cạm bẫy: 

Nếu chuỗi là`aaaaa`, mọi vị trí sẽ xuất ra`5`. Một cách tiếp cận ngây thơ chỉ xem xét các palindromes tập trung chính xác tại$i$sẽ thất bại đối với các bảng màu có độ dài chẵn, thiếu các phân đoạn như`aa`tập trung giữa các vị trí. 

Nếu chuỗi là`ababa`, câu trả lời là`5`đối với chỉ số ở giữa và giảm đối xứng, nhưng phép tính chỉ dựa vào trung tâm có thể bỏ sót chỉ số che phủ bảng màu tối đa$2$vẫn là chuỗi đầy đủ mặc dù tâm của nó không chính xác ở$2$. 

Những trường hợp này buộc chúng ta phải suy nghĩ về cấu trúc palindromic toàn cầu hơn là mở rộng trên mỗi chỉ số. 

## Phương pháp tiếp cận 

Chiến lược brute-force bắt đầu bằng cách sửa từng chỉ mục$i$, sau đó liệt kê tất cả các chuỗi con$[l, r]$như vậy$l \le i \le r$, và kiểm tra xem$s[l..r]$là một palindrome. Mỗi chi phí kiểm tra$O(r-l)$, và có$O(n^2)$chuỗi con cho mỗi thử nghiệm trong trường hợp xấu nhất. Điều này dẫn đến$O(n^3)$tổng thể trong trường hợp xấu nhất, điều này rõ ràng là không thể thực hiện được. 

Ngay cả việc cải thiện việc kiểm tra palindrome bằng hàm băm cũng làm giảm mỗi lần kiểm tra xuống$O(1)$, nhưng vẫn rời đi$O(n^2)$chuỗi con cho mỗi chỉ mục, do đó độ phức tạp nhìn chung vẫn là bậc hai. 

Quan sát quan trọng là mỗi palindrome có thể được biểu diễn bằng tâm của nó, và quan trọng hơn, mỗi palindrome đóng góp toàn bộ khoảng của nó cho tất cả các chỉ số bên trong nó. Thay vì tính toán lại các đóng góp cho mỗi chỉ mục, chúng tôi muốn truyền bá từng khoảng palindrome tới tất cả các vị trí mà nó bao trùm. 

Điều này gợi ý một ý tưởng về kiểu mảng khác biệt: nếu chúng ta biết rằng một palindrome trải dài$[l, r]$, thì mọi chỉ mục trong khoảng đó có khả năng cập nhật câu trả lời của nó lên ít nhất$r-l+1$. Vì vậy, vấn đề giảm xuống còn việc tìm tất cả các khoảng palindrome tối đa một cách hiệu quả. 

Thuật toán của Manacher đưa ra chính xác điều đó: nó tính toán, cho mọi tâm (bao gồm cả giữa các ký tự), bán kính palindrome lớn nhất trong thời gian tuyến tính. Mỗi palindrome được tìm thấy tương ứng với một khoảng và từ đó chúng ta có thể cập nhật câu trả lời tốt nhất cho tất cả các chỉ số mà nó bao gồm. Thách thức là thực hiện các cập nhật phạm vi này một cách hiệu quả. 

Chúng ta có thể coi mỗi palindrome đóng góp một giá trị cho một phân đoạn và chúng ta muốn ở mỗi chỉ mục độ dài phân đoạn tối đa bao phủ nó. Điều này trở thành một vấn đề cổ điển về “cập nhật phạm vi tối đa, truy vấn điểm”, có thể giải quyết được bằng cây phân đoạn hoặc bằng cách xử lý điểm cuối bằng cách quét bằng cách sử dụng mảng sai phân và cực đại tiền tố. 

Một giải pháp sạch sử dụng quá trình quét theo các khoảng thời gian bắt nguồn từ Manacher, lưu trữ cho mỗi điểm cuối bên trái phần mở rộng bên phải tốt nhất rồi truyền bá. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Kiểm tra tất cả các chuỗi con bằng hàm băm |$O(n^2)$|$O(n)$| Quá chậm | 
| Manacher + lan truyền theo khoảng thời gian |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi tính toán tất cả các bán kính palindromic bằng thuật toán Manacher trên một chuỗi được biến đổi để xử lý các tâm chẵn và lẻ một cách thống nhất. 

1. Chuyển đổi chuỗi bằng cách chèn dấu phân cách (ví dụ:`#`) sao cho mọi palindrome đều có độ dài lẻ. Điều này tránh việc xử lý riêng biệt các palindrome chẵn và lẻ và đơn giản hóa việc giải thích bán kính. 
2. Chạy Manacher để tính một mảng`rad[i]`mỗi vị trí ở đâu`i`trong chuỗi được chuyển đổi sẽ cho bán kính palindrome tối đa tập trung ở đó. Bước này là tuyến tính vì thuật toán sử dụng lại thông tin đối xứng đã tính toán trước đó. 
3. Đối với mọi trung tâm`i`trong chuỗi đã biến đổi, chuyển đổi palindrome của nó trở lại thành một khoảng thực$[l, r]$trong chuỗi gốc. Mỗi palindrome như vậy đại diện cho một giá trị đóng góp của chuỗi con ứng cử viên$r - l + 1$. 
4. Thay vì gán giá trị này cho mọi chỉ mục trong$[l, r]$, chúng tôi thực hiện cập nhật phạm vi cho biết bảng màu này đóng góp ít nhất độ dài đó cho tất cả các chỉ số trong khoảng. 
5. Chúng tôi xử lý tất cả các khoảng như vậy bằng cách duy trì, đối với mỗi vị trí, ranh giới bên phải tối đa của một bảng màu bắt đầu hoặc bao phủ nó, sau đó tính toán các câu trả lời cuối cùng bằng cách quét qua chuỗi. 

Ý tưởng chính là chúng tôi không bao giờ quan tâm đến bảng màu nào tạo ra câu trả lời, chỉ có độ dài tốt nhất bao trùm từng vị trí. Vì vậy, các palindrome chồng chéo sẽ thu gọn thành một ràng buộc tối đa duy nhất cho mỗi chỉ mục. 

### Tại sao nó hoạt động 

Mỗi palindrome trong chuỗi tương ứng với chính xác một tâm trong biểu diễn Manacher được chuyển đổi và Manacher đảm bảo chúng ta tìm thấy bán kính tối đa có thể có cho tâm đó. Do đó, mọi chuỗi con palindromic được coi là một phần của khoảng tối đa nào đó. Bất kỳ bảng màu nhỏ hơn nào cũng không liên quan vì nó được chứa trong một bảng màu lớn hơn hoặc bằng nhau có tâm ở cùng một vị trí. 

Vì câu trả lời cho mỗi chỉ số chỉ phụ thuộc vào khoảng thời gian tốt nhất bao trùm nó và mọi khoảng thời gian hợp lệ đều được tạo ra, việc lấy giá trị tối đa trên tất cả các khoảng sẽ mang lại kết quả chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def manacher(s):
    t = ['#']
    for ch in s:
        t.append(ch)
        t.append('#')
    n = len(t)
    rad = [0] * n
    center = 0
    right = 0

    for i in range(n):
        if i < right:
            rad[i] = min(right - i, rad[2 * center - i])

        while i - rad[i] - 1 >= 0 and i + rad[i] + 1 < n and t[i - rad[i] - 1] == t[i + rad[i] + 1]:
            rad[i] += 1

        if i + rad[i] > right:
            center = i
            right = i + rad[i]

    return t, rad

def solve():
    n = int(input())
    s = input().strip()

    t, rad = manacher(s)

    m = len(t)

    diff = [0] * (n + 2)

    for i in range(m):
        if rad[i] == 0:
            continue

        l = (i - rad[i]) // 2
        r = (i + rad[i]) // 2 - 1

        length = r - l + 1
        diff[l] = max(diff[l], length)

    ans = [0] * n
    best = 0
    for i in range(n):
        best = max(best, diff[i])
        ans[i] = best

    print(*ans)

t = int(input())
for _ in range(t):
    solve()
```Giải pháp bắt đầu bằng thuật toán Manacher, thuật toán này xây dựng chuỗi biến đổi và tính toán bán kính palindrome theo thời gian tuyến tính. Sự trở lại`rad`mảng mã hóa mọi bảng màu tối đa tập trung ở mỗi vị trí. 

Việc chuyển đổi từ các chỉ số đã chuyển đổi trở lại các chỉ số ban đầu là một phần tế nhị. Mỗi trung tâm trải dài một phân đoạn trong mảng được chuyển đổi và chia cho hai bản đồ để trở lại tọa độ chuỗi ban đầu. Tính toán$[l, r]$cung cấp phạm vi bao phủ đầy đủ của palindrome đó. 

các`diff`mảng được sử dụng để lưu trữ, đối với mỗi chỉ mục bắt đầu, độ dài palindrome tốt nhất bắt đầu ở đó hoặc được neo ở đó với tư cách là người đóng góp ứng cử viên. Chúng tôi chỉ lưu trữ tối đa vì các ứng cử viên yếu hơn không liên quan. 

Cuối cùng, việc quét tối đa tiền tố sẽ truyền bá những độ dài tốt nhất có thể này để mọi vị trí đều kế thừa bảng màu mạnh nhất bao phủ hoặc chạm tới nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`ababa`Sau khi chuyển đổi, Manacher tìm thấy bán kính tối đa có tâm ở giữa kéo dài toàn bộ chuỗi. 

| Trung tâm | Bán kính | Khoảng (l, r) | Chiều dài | 
| --- | --- | --- | --- | 
| giữa | đầy đủ | (0, 4) | 5 | 

Bộ quét`diff[0] = 5`và hiệu suất lan truyền tiền tố`5 5 5 5 5`. 

Điều này xác nhận rằng mọi chỉ mục bên trong một palindrome toàn cục phải kế thừa độ dài của palindrome đó. 

### Ví dụ 2:`abca`Palindrome là:`a`,`b`,`c`,`a`và không còn tập trung vào các palindromes nữa. 

| Trung tâm | Khoảng thời gian | Chiều dài | 
| --- | --- | --- | 
| mỗi ký tự | (tôi, tôi) | 1 | 

Vì thế`diff[i] = 1`cho tất cả`i`và việc truyền tiền tố giữ tất cả các giá trị tại`1`. 

Điều này chứng tỏ rằng khi không tồn tại bảng màu lớn hơn, thuật toán sẽ quay lại câu trả lời một ký tự một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi bài kiểm tra | Manacher xử lý mỗi ký tự một lần và quá trình quét là tuyến tính | 
| Không gian |$O(n)$| mảng cho chuỗi chuyển đổi, bán kính và mảng chênh lệch | 

Tổng của$n$trên tất cả các bài kiểm tra là$2 \cdot 10^5$, do đó, một giải pháp tuyến tính phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ cũng tuyến tính và bị giới hạn bởi cùng một ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def manacher(s):
        t = ['#']
        for ch in s:
            t.append(ch)
            t.append('#')
        n = len(t)
        rad = [0] * n
        center = 0
        right = 0

        for i in range(n):
            if i < right:
                rad[i] = min(right - i, rad[2 * center - i])

            while i - rad[i] - 1 >= 0 and i + rad[i] + 1 < n and t[i - rad[i] - 1] == t[i + rad[i] + 1]:
                rad[i] += 1

            if i + rad[i] > right:
                center = i
                right = i + rad[i]

        return t, rad

    def solve():
        n = int(input())
        s = input().strip()

        t, rad = manacher(s)
        m = len(t)

        diff = [0] * (n + 2)

        for i in range(m):
            if rad[i] == 0:
                continue
            l = (i - rad[i]) // 2
            r = (i + rad[i]) // 2 - 1
            length = r - l + 1
            diff[l] = max(diff[l], length)

        ans = [0] * n
        best = 0
        for i in range(n):
            best = max(best, diff[i])
            ans[i] = best

        return " ".join(map(str, ans))

    t = int(input())
    out = []
    for _ in range(t):
        out.append(solve())
    return "\n".join(out)

# custom tests
assert run("1\n1\na\n") == "1"
assert run("1\n5\nababa\n") == "5 5 5 5 5"
assert run("1\n4\nabca\n") == "1 1 1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`1`| xử lý độ dài tối thiểu | 
|`ababa`|`5 5 5 5 5`| palindrome bảo hiểm đầy đủ | 
|`abca`|`1 1 1 1`| không có palindromes chồng chéo | 

## Vỏ cạnh 

Một chuỗi ký tự đơn như`a`được Manacher xử lý một cách tầm thường, nó trả về bán kính bằng 0 trong chuỗi được chuyển đổi và ánh xạ tới khoảng$(0, 0)$. Bộ thuật toán`diff[0] = 1`và đầu ra quét tiền tố`1`. 

Một chuỗi thống nhất như`aaaaaa`tạo ra một tâm duy nhất có bán kính tối đa bao trùm toàn bộ phạm vi. Khoảng đó được chuyển đổi thành$[0, n-1]$và quá trình quét đảm bảo mọi vị trí đều nhận được giá trị`n`, phù hợp với thực tế là mọi chỉ mục đều nằm trong bảng màu tổng thể. 

Một chuỗi không có ký tự lặp lại như`abcdef`chỉ tạo ra các palindrome có độ dài 1. Mỗi trung tâm chỉ đóng góp vị trí riêng của nó, và không có khoảng trống nào vượt ra ngoài nó. Việc quét không bao giờ tăng vượt quá 1, vì vậy mọi chỉ số đều xuất ra 1. 

Mỗi trường hợp xác nhận rằng thuật toán suy biến chính xác ở cả hai thái cực: đối xứng cực đại và không có đối xứng nào cả.
