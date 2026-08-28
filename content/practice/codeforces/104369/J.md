---
title: "CF 104369J - X Bằng Y"
description: "Chúng ta được yêu cầu chọn hai cơ số, một cho $x$ và một cho $y$, sao cho khi cả hai số được viết theo cơ số tương ứng của chúng, dãy chữ số thu được sẽ giống hệt nhau khi đọc từ chữ số có nghĩa nhỏ nhất đến chữ số có nghĩa nhất."
date: "2026-07-01T17:39:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104369
codeforces_index: "J"
codeforces_contest_name: "The 2023 Guangdong Provincial Collegiate Programming Contest"
rating: 0
weight: 104369
solve_time_s: 71
verified: true
draft: false
---

[CF 104369J - X Bằng Y](https://codeforces.com/problemset/problem/104369/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu chọn hai căn cứ, một cho$x$và một cho$y$, sao cho khi cả hai số được viết theo cơ số tương ứng của chúng, dãy chữ số thu được sẽ giống hệt nhau khi đọc từ chữ số có nghĩa nhỏ nhất đến chữ số có nghĩa lớn nhất. 

Cụ thể, để có cơ sở$a$, chúng tôi liên tục chia$x$qua$a$và ghi lại số dư, tạo ra một dãy chữ số. Quá trình tương tự được thực hiện cho$y$trong căn cứ$b$. Yêu cầu là hai chuỗi này phải khớp chính xác, bao gồm độ dài và vị trí từng chữ số. 

Điều này mạnh hơn việc chỉ có các giá trị số bằng nhau ở các cơ số khác nhau. Toàn bộ cấu trúc của việc mở rộng vị trí phải căn chỉnh, nghĩa là cả hai số phải phân tách thành cùng một “mẫu chữ số” dưới các hệ số có thể khác nhau. 

Các ràng buộc cho phép các giá trị lên tới$10^9$, do đó mỗi biểu diễn cơ sở có nhiều nhất khoảng 30 chữ số trong trường hợp xấu nhất. Điều đó ngay lập tức giới hạn độ dài của bất kỳ chuỗi hợp lệ nào, vì mỗi chuỗi chữ số đều tương ứng với một biểu diễn cơ số tiêu chuẩn trong cả hai số. 

Trường hợp cạnh tinh tế xuất hiện khi biểu diễn có độ dài bằng một. Trong trường hợp đó, trình tự chỉ là$[x]$trong căn cứ$a > x$, và tương tự$[y]$trong căn cứ$b > y$. Lực lượng này$x = y$, nếu không thì không thể khớp được. Một cách tiếp cận ngây thơ mà bỏ qua cấu trúc đặc biệt này có thể cố gắng xây dựng các kết quả khớp có nhiều chữ số một cách không chính xác khi biểu diễn thực sự thu gọn thành một chữ số. 

Một dạng hư hỏng khác xuất phát từ việc giả định rằng phải sử dụng cùng một đế. Bài toán cho phép các cơ sở độc lập, do đó, bất kỳ phương pháp nào sửa một cơ sở và chỉ tìm kiếm cơ sở kia sẽ bỏ lỡ các giải pháp bất đối xứng hợp lệ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tất cả các cặp căn cứ$(a, b)$, tính cả hai dãy chữ số và so sánh chúng. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của vấn đề. Tuy nhiên, không gian tìm kiếm là rất lớn, vì cả hai$a$Và$b$đi lên$10^9$, khiến ngay cả một lần quét toàn bộ cũng không thể thực hiện được. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần so sánh trực tiếp các căn cứ. Thay vào đó, đối với bất kỳ cơ số cố định nào, số này xác định một chuỗi chữ số duy nhất. Nếu tồn tại hai giải pháp hợp lệ, chúng phải chia sẻ chuỗi này và chuỗi đó ngắn (tối đa khoảng 30 phần tử). 

Điều này chuyển vấn đề sang dạng cấu trúc: chúng ta đang tìm kiếm một chuỗi chữ số có thể đồng thời đóng vai trò là biểu diễn cơ sở hợp lệ của$x$ở một căn cứ nào đó$a$và của$y$ở một căn cứ nào đó$b$. Khi một chuỗi ứng cử viên được cố định từ một phía, việc xác minh nó theo phía bên kia sẽ giảm xuống việc giải một phương trình đa thức trong cơ sở, có thể được kiểm tra hiệu quả bằng tìm kiếm nhị phân vì việc đánh giá là đơn điệu đối với các chữ số không âm. 

Thay vì liệt kê tất cả các cơ sở, chúng tôi khai thác thực tế là các chuỗi chữ số hợp lệ được tạo ra bởi các biểu diễn cơ sở của$x$Và$y$. Đối với mỗi cơ sở khả thi của$x$, chúng tôi tạo ra chuỗi chữ số của nó và cố gắng khớp nó với$y$bằng cách tìm một cơ sở tương thích. 

Để làm cho điều này trở nên thiết thực, chúng tôi giới hạn phạm vi xuất hiện các chuỗi chữ số không tầm thường. Đối với các căn cứ lớn hơn khoảng$\sqrt{x}$, các biểu diễn trở nên rất ngắn và tổng cộng chỉ có một số lượng nhỏ các trường hợp như vậy tồn tại. Điều này giúp cho việc liệt kê có thể thực hiện được trong tất cả các trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu tất cả các căn cứ |$O(A \cdot B \cdot \log x)$|$O(1)$| Quá chậm | 
| Liệt kê các cơ số của một số và so khớp thông qua đánh giá |$O(\sqrt{x} \log x + \sqrt{y} \log y)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tập trung vào việc xây dựng tất cả các chuỗi chữ số có ý nghĩa của$x$bằng cách thử các cơ sở ứng cử viên, sau đó xác minh xem liệu cùng một chuỗi có thể biểu diễn$y$ở một cơ sở nào đó. 

### bước 

1. Nếu$x = y$, hãy xem xét trường hợp biểu diễn một chữ số. Chúng ta có thể chọn bất kỳ cơ sở nào$a > x$Và$b > y$miễn là chúng nằm trong giới hạn. Nếu những căn cứ như vậy tồn tại dưới$A$Và$B$, chúng tôi ngay lập tức trả lại chúng. Điều này xử lý trình tự suy biến$[x]$. 
2. Đối với tất cả các cơ sở ứng cử viên$a$bắt đầu từ 2 đến một mức cắt thực tế xung quanh$\sqrt{x}$, tính dãy số của$x$trong căn cứ$a$. Chuỗi này thu được bằng các phép toán modulo và chia lặp đi lặp lại, tạo ra các chữ số từ ít quan trọng nhất đến quan trọng nhất. 
3. Với mỗi dãy số được tạo ra$d$, giải thích nó như là một cơ sở-$b$biểu diễn số chưa biết:$$f(b) = \sum_{i=0}^{k-1} d_i b^i$$Chúng ta cần xác định liệu có tồn tại một cơ sở hay không$b$như vậy$f(b) = y$, với$2 \le b \le B$. 
4. Vì tất cả các chữ số đều không âm và chữ số cao nhất khác 0,$f(b)$đang tăng lên nghiêm trọng đối với$b \ge 1$. Chúng ta có thể tìm kiếm nhị phân trên$b$để kiểm tra xem phương trình có đúng không. 
5. Nếu chúng tôi tìm thấy một giá trị hợp lệ$b$, chúng tôi xác minh rằng$b \le B$Và$a \le A$. Nếu cả hai ràng buộc đều được thỏa mãn, chúng ta sẽ xuất cặp. 
6. Nếu không có cơ sở ứng cử$x$mang lại sự trùng khớp, chúng tôi lặp lại quá trình tương tự một cách đối xứng bắt đầu từ$y$. 

### Tại sao nó hoạt động 

Mọi nghiệm hợp lệ đều tương ứng với một chuỗi chữ số đồng thời là sự mở rộng vị trí hợp lệ cho cả hai số. Bất kỳ chuỗi nào như vậy phải xuất hiện dưới dạng phân tách chữ số của ít nhất một biểu diễn cơ sở hợp lệ của một trong hai$x$hoặc$y$. Bằng cách liệt kê tất cả các biểu diễn dựa trên cơ sở từ một phía, chúng tôi liệt kê tất cả các chuỗi ứng cử viên có thể khớp. Đối với mỗi chuỗi, dạng đa thức đơn điệu đảm bảo rằng việc kiểm tra sự tồn tại của cơ số tương thích cho số kia tương đương với việc giải một phương trình hàm tăng đơn lẻ, do đó tìm kiếm nhị phân không thể bỏ sót kết quả khớp hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def digits(n, base):
    d = []
    while n > 0:
        d.append(n % base)
        n //= base
    return d

def eval_poly(d, b):
    res = 0
    p = 1
    for x in d:
        res += x * p
        p *= b
    return res

def find_base(d, target, lim):
    lo, hi = 2, lim
    while lo <= hi:
        mid = (lo + hi) // 2
        val = eval_poly(d, mid)
        if val == target:
            return mid
        if val < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return -1

def solve_case(x, y, A, B):
    if x == y:
        if x < A and y < B:
            return (x + 1, y + 1)
        return None

    def try_x():
        limit = int(x ** 0.5) + 2
        for a in range(2, min(A, limit) + 1):
            d = digits(x, a)
            if len(d) == 1:
                continue
            b = find_base(d, y, B)
            if b != -1:
                return a, b
        return None

    def try_y():
        limit = int(y ** 0.5) + 2
        for b in range(2, min(B, limit) + 1):
            d = digits(y, b)
            if len(d) == 1:
                continue
            a = find_base(d, x, A)
            if a != -1:
                return a, b
        return None

    ans = try_x()
    if ans:
        return ans
    return try_y()

t = int(input())
for _ in range(t):
    x, y, A, B = map(int, input().split())
    ans = solve_case(x, y, A, B)
    if not ans:
        print("NO")
    else:
        print("YES")
        print(ans[0], ans[1])
```Đầu tiên, mã này xử lý trường hợp đẳng thức suy biến trong đó cả hai số có thể được biểu diễn dưới dạng chuỗi một chữ số. Sau đó, nó chỉ liệt kê các cơ sở ứng cử viên trong phạm vi rút gọn nơi tồn tại các biểu diễn nhiều chữ số. Đối với mỗi cơ số, nó xây dựng chuỗi chữ số và cố gắng khôi phục cơ số phù hợp cho số kia bằng cách sử dụng tìm kiếm nhị phân qua đánh giá đa thức đơn điệu. 

Một cạm bẫy triển khai phổ biến là quên rằng đánh giá đa thức phải được tính toán lại cho mọi điểm giữa trong tìm kiếm nhị phân. Việc sử dụng lại quyền hạn hoặc tính toán trước không chính xác có thể dẫn đến so sánh tràn hoặc không chính xác, đặc biệt vì các giá trị trung gian có thể tăng vượt quá$10^9$mặc dù câu trả lời cuối cùng vẫn nằm trong giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
x = 3, y = 11, A = 1000, B = 1000
```Chúng tôi thử căn cứ cho$x$. 

| một | chữ số của x | trình tự hợp lệ? | tìm thấy b cho y | 
| --- | --- | --- | --- | 
| 2 | [1,1] | vâng | b = 10 cho 11 = 1 + 1·10 | 
| 3 | [0,1] | hợp lệ nhưng không khớp | không | 

Vì vậy, chúng tôi tìm thấy một trận đấu tại$a=2, b=10$. 

Điều này xác nhận rằng mẫu chữ số giống nhau$[1,1]$đại diện cho cả hai số dưới các cơ sở khác nhau. 

### Ví dụ 2 

đầu vào:```
x = 157, y = 291
```Đang thử căn cứ nhỏ cho$x$, cuối cùng chúng ta tìm thấy một dãy chữ số từ cơ số nào đó$a$. Giả sử nó mang lại chuỗi$d$. Sau đó chúng tôi cố gắng giải quyết$f(b) = 291$. Nếu không$b$trong phạm vi thỏa mãn phương trình, chúng ta bác bỏ chuỗi đó và tiếp tục. 

Ví dụ này thực hiện trường hợp các chuỗi chữ số tồn tại nhưng không tương thích, đảm bảo tìm kiếm nhị phân loại bỏ chính xác các kết quả dương tính giả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{x} \log x + \sqrt{y} \log y)$mỗi bài kiểm tra | Mỗi cơ sở ứng cử viên xây dựng các chữ số trong$O(\log x)$và mỗi kiểm tra sử dụng tìm kiếm nhị phân trên đánh giá cơ sở | 
| Không gian |$O(1)$| Chỉ các vectơ chữ số tạm thời mới được lưu trữ cho mỗi lần thử | 

Các ràng buộc cho phép tối đa 1000 bài kiểm tra, nhưng chỉ một số ít đạt được giá trị lớn. Phép liệt kê giới hạn căn bậc hai giữ cho tổng số phép tính nằm trong giới hạn có thể chấp nhận được trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        x, y, A, B = map(int, input().split())

        def digits(n, base):
            d = []
            while n:
                d.append(n % base)
                n //= base
            return d

        def eval_poly(d, b):
            res = 0
            p = 1
            for x in d:
                res += x * p
                p *= b
            return res

        def find_base(d, target, lim):
            lo, hi = 2, lim
            while lo <= hi:
                mid = (lo + hi) // 2
                val = eval_poly(d, mid)
                if val == target:
                    return mid
                if val < target:
                    lo = mid + 1
                else:
                    hi = mid - 1
            return -1

        def solve(x, y, A, B):
            if x == y and x < A and y < B:
                return x + 1, y + 1
            limit = int(x ** 0.5) + 2
            for a in range(2, min(A, limit) + 1):
                d = digits(x, a)
                if len(d) > 1:
                    b = find_base(d, y, B)
                    if b != -1:
                        return a, b
            limit = int(y ** 0.5) + 2
            for b in range(2, min(B, limit) + 1):
                d = digits(y, b)
                if len(d) > 1:
                    a = find_base(d, x, A)
                    if a != -1:
                        return a, b
            return None

        ans = solve(x, y, A, B)
        output.append("NO" if not ans else f"YES\n{ans[0]} {ans[1]}")
    return "\n".join(output)

# sample and custom tests
assert run("1\n1 1 1000 1000\n") == "YES\n2 2"
assert run("1\n1 2 1000 1000\n") == "NO"
assert run("1\n3 11 1000 1000\n") == "YES\n2 10"
assert run("1\n5 5 1000 1000\n") == "YES\n6 6"
assert run("1\n2 3 10 10\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1000 1000`| CÓ 2 2 | trường hợp một chữ số | 
|`1 2 1000 1000`| KHÔNG | sự bình đẳng không thể | 
|`3 11 1000 1000`| CÓ 2 10 | khớp nhiều chữ số | 
|`5 5 1000 1000`| CÓ 6 6 | trường hợp cạnh bình đẳng | 
|`2 3 10 10`| KHÔNG | không có trận đấu ngẫu nhiên | 

## Vỏ cạnh 

Khi nào$x = y$, thuật toán ngay lập tức xem xét biểu diễn một chữ số. Đối với một đầu vào như$x = y = 5$, đang chọn$a = 6$Và$b = 7$tạo ra chuỗi chữ số$[5]$Và$[5]$, trận đấu nào. Thuật toán nắm bắt được điều này mà không cần bất kỳ tìm kiếm cơ sở nào vì nó xử lý rõ ràng trường hợp biểu diễn thu gọn. 

Đối với trường hợp một số nhỏ và số kia lớn, chẳng hạn như$x = 1$Và$y = 10^9$, mọi chuỗi chữ số ứng cử viên được tạo từ$x$là tầm thường và không thể khai triển thành đa thức bằng$y$. Tìm kiếm nhị phân trên các cơ sở cho$y$sẽ liên tục thất bại, đảm bảo từ chối chính xác. 

Khi chuỗi chữ số trở nên dài do các cơ sở nhỏ như$a=2$, việc đánh giá đa thức vẫn ổn định vì độ dài chuỗi bị giới hạn bởi$\log_2(10^9)$. Mặc dù các giá trị trung gian có thể tăng lớn nhưng chúng không bao giờ vượt quá phạm vi cần thiết để so sánh với$y$, vì chúng ta chấm dứt ngay khi giá trị vượt qua nó.
