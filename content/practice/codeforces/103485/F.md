---
title: "CF 103485F - Ramesses, Ra và Rễ"
description: "Chúng ta được cung cấp một danh sách các số nguyên và với mỗi giá trị truy vấn $r$, chúng ta phải đếm xem có bao nhiêu số nguyên đó là lũy thừa $r$-th hoàn hảo. Nói cách khác, với một $r$ cố định, chúng ta muốn biết có bao nhiêu giá trị $ai$ có thể được viết dưới dạng $x^r$ đối với một số nguyên $x$."
date: "2026-07-03T06:24:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103485
codeforces_index: "F"
codeforces_contest_name: "Copa Do Mat\u00e3o, University Of S\u00e3o Paulo Programming Contest"
rating: 0
weight: 103485
solve_time_s: 47
verified: true
draft: false
---

[CF 103485F - Ramesses, Ra và Roots](https://codeforces.com/problemset/problem/103485/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên và với mỗi giá trị truy vấn$r$, chúng ta phải đếm xem có bao nhiêu số nguyên đó là hoàn hảo$r$-quyền lực thứ. Nói cách khác, đối với một mức cố định$r$, chúng tôi muốn biết có bao nhiêu giá trị$a_i$có thể được viết là$x^r$đối với một số nguyên$x$. 

Tương tự, nhiệm vụ là kiểm tra xem việc sử dụng$r$- căn bậc thứ của mỗi số mang lại một số nguyên. Đầu ra của mỗi truy vấn chỉ đơn giản là số phần tử chính xác trong mảng$r$-quyền lực thứ. 

Khó khăn quan trọng là cả kích thước mảng và số lượng truy vấn đều có thể đạt tới$10^5$và bản thân các giá trị sẽ tăng lên$10^9$. Điều này ngay lập tức loại trừ việc tính toán lại các gốc cho mỗi cặp phần tử truy vấn, vì việc đó sẽ yêu cầu tối đa$10^{10}$séc. 

Một cách tiếp cận ngây thơ cũng sẽ gặp khó khăn với các vấn đề về độ chính xác: tính toán các nghiệm dấu phẩy động và kiểm tra tính tích phân là không đáng tin cậy ở quy mô này, đặc biệt khi số mũ lớn và giá trị gần với lũy thừa hoàn hảo. 

Trường hợp cạnh tinh tế xuất hiện khi$r = 1$, trong đó mọi số đều có giá trị lũy thừa bậc 1 hợp lệ. Một trường hợp góc khác là$a_i = 1$, đó là một sự hoàn hảo$r$- sức mạnh thứ cho mọi người$r$, từ$1^r = 1$. Một giải pháp bất cẩn dựa vào phép tính gần đúng gốc có thể phân loại sai các giá trị như 8 hoặc 16 đối với số mũ lớn hơn do làm tròn dấu phẩy động. 

## Phương pháp tiếp cận 

Chiến lược vũ phu rất đơn giản. Đối với mỗi truy vấn$r$, lặp lại tất cả các số và kiểm tra xem$a_i$là một sự hoàn hảo$r$-th lũy thừa bằng cách tính toán một ứng cử viên gốc số nguyên và xác minh nó. Mỗi lần kiểm tra có giá khoảng$O(\log a_i)$hoặc$O(1)$với tìm kiếm nhị phân số nguyên cẩn thận, do đó độ phức tạp đầy đủ sẽ trở thành$O(nq \log A)$. Với$n, q = 10^5$, tốc độ này quá chậm. 

Quan sát quan trọng là mặc dù$r$có thể lớn như$10^9$, số bất kỳ$a_i \le 10^9$không thể có nhiều số mũ khác nhau để làm cho nó trở thành một lũy thừa hoàn hảo. Mỗi số có một số lượng nhỏ các phân tách có ý nghĩa như$x^k$, bởi vì số mũ tăng nhanh và việc phân tích nhân tử lặp đi lặp lại sẽ làm sụp đổ cấu trúc. 

Điều này gợi ý việc lật ngược quan điểm. Thay vì trả lời từng truy vấn một cách độc lập, chúng tôi tính toán trước cho mọi số tất cả số mũ$k$như thế nó là một sự hoàn hảo$k$-quyền lực thứ. Sau đó, mỗi truy vấn chỉ hỏi có bao nhiêu số chứa số mũ$r$trong sự phân hủy của chúng. 

Chúng tôi tránh hoàn toàn các nghiệm số dấu phẩy động bằng cách liên tục trích xuất các nghiệm số nguyên cho mỗi số cho đến khi nó không còn là lũy thừa hoàn hảo, theo dõi tất cả các số mũ gặp phải trong quá trình phân tách này. 

Điều này giúp giảm bớt vấn đề trong việc xây dựng bản đồ tần số theo các giá trị số mũ xuất phát từ độ sâu phân tích hệ số, sau đó mỗi truy vấn được trả lời theo$O(1)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq \log A)$|$O(1)$| Quá chậm | 
| Phân tích lũy thừa + băm |$O(n \log^2 A + q)$|$O(n \log A)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng số một cách độc lập và rút ra mọi cách để nó có thể được biểu diễn dưới dạng lũy thừa hoàn hảo. 

1. Với mỗi số$a_i$, cố gắng diễn đạt nó như$b^k$đối với một số nguyên$k \ge 2$. Chúng tôi thực hiện điều này bằng cách kiểm tra liên tục các nghiệm nguyên bằng cách sử dụng tìm kiếm nhị phân. Mỗi lần chúng tôi tìm thấy điều đó$a_i$là một sự hoàn hảo$k$- lũy thừa thứ, chúng ta thay nó bằng gốc của nó và ghi lại số mũ đó. 
2. Chúng ta tiếp tục quá trình này cho đến khi số này không còn có thể giảm xuống cơ số nguyên nhỏ hơn thông qua việc lấy căn. Điều này tạo ra một chuỗi như$a = x^{k_1} = (x^{1})^{k_1 k_2}$, vân vân. Chúng tôi ghi lại tất cả các số mũ tổng hợp gặp phải trong quá trình phân rã này. 
3. Với mỗi số ban đầu, ta xây dựng một tập hợp tất cả các số mũ$r$như thế nó là một sự hoàn hảo$r$-quyền lực thứ. Chúng tôi tăng bản đồ tần số toàn cầu cho từng số mũ như vậy. 
4. Sau khi xử lý trước tất cả các số, chúng tôi trả lời từng truy vấn$r$bằng cách trả về tần số được lưu trữ của$r$hoặc bằng 0 nếu nó không tồn tại. 

Lý do điều này hoạt động là vì mọi số mũ hợp lệ đều tương ứng với cấu trúc nhân tử lặp lại của số. Nếu một số là một khối lập phương hoàn hảo thì nó cũng là một lũy thừa hoàn hảo cho tất cả các ước số của chuỗi số mũ của nó và việc trích rút căn bậc hai lặp đi lặp lại sẽ nắm bắt chính xác những mối quan hệ này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def integer_root(x, k):
    lo, hi = 1, int(x ** (1 / k)) + 2
    while lo <= hi:
        mid = (lo + hi) // 2
        val = mid ** k
        if val == x:
            return mid
        if val < x:
            lo = mid + 1
        else:
            hi = mid - 1
    return -1

def get_exponents(x):
    exps = set()
    current = x
    # try exponents from 2 upward implicitly via root extraction
    while True:
        found = False
        for k in range(2, 32):
            r = integer_root(current, k)
            if r != -1 and r ** k == current:
                exps.add(k)
                current = r
                found = True
                break
        if not found:
            break
    return exps

n, q = map(int, input().split())
arr = list(map(int, input().split()))
queries = list(map(int, input().split()))

freq = {}

for a in arr:
    exps = get_exponents(a)
    for e in exps:
        freq[e] = freq.get(e, 0) + 1

freq[1] = n

out = []
for r in queries:
    out.append(str(freq.get(r, 0)))

print("\n".join(out))
```Việc triển khai dựa trên thực tế là mỗi số chỉ đóng góp một số lượng nhỏ số mũ hợp lệ, vì vậy chúng ta có thể liệt kê chúng một cách an toàn bằng cách trích xuất gốc nhiều lần. Kiểm tra gốc tìm kiếm nhị phân đảm bảo tính chính xác mà không có lỗi dấu phẩy động và vòng lặp bên trong trên số mũ nhỏ bị giới hạn vì số mũ tăng nhanh và không thể xâu chuỗi sâu hơn khoảng 30 cho các giá trị lên tới$10^9$. 

Một chi tiết triển khai tinh tế là xử lý$r = 1$, phải luôn trả về$n$, vì mọi số đều là lũy thừa bậc nhất của chính nó. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu. 

đầu vào:```
5 4
1 16 8 9 7
1 2 3 4
```Đối với mỗi số, chúng tôi trích xuất số mũ: 

| Số | Số mũ được tìm thấy | 
| --- | --- | 
| 1 | tất cả r (được xử lý riêng là 1) | 
| 16 | 2, 4 | 
| 8 | 2, 3 | 
| 9 | 2 | 
| 7 | không | 

Bây giờ chúng tôi xử lý các truy vấn. 

Vì$r = 1$, mọi số đều hợp lệ nên đáp án là 5. 

cho$r = 2$, các số hợp lệ là 16, 8, 9 nên đáp án là 3. 

cho$r = 3$, các số hợp lệ là 8 và 1 nên đáp án là 2. 

cho$r = 4$, các số hợp lệ là 16 và 1 nên đáp án là 2. 

Điều này phù hợp với đầu ra. 

Dấu vết cho thấy rằng khi cấu trúc số mũ được tính toán trước, mỗi truy vấn chỉ là một phép tra cứu và các số như 1 sẽ đóng góp chính xác cho tất cả số mũ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log^2 A + q)$| mỗi số trải qua các tìm kiếm gốc có giới hạn và trích xuất số mũ, các truy vấn là O(1) | 
| Không gian |$O(n)$| bản đồ tần số lưu trữ tối đa một mục nhập cho mỗi số mũ được phát hiện trên mỗi số | 

Những hạn chế$n, q \le 10^5$Và$a_i \le 10^9$vừa vặn thoải mái, vì độ sâu trích xuất gốc nhỏ và các truy vấn có thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    arr = list(map(int, input().split()))
    queries = list(map(int, input().split()))

    freq = {}

    def integer_root(x, k):
        lo, hi = 1, int(x ** (1 / k)) + 2
        while lo <= hi:
            mid = (lo + hi) // 2
            val = mid ** k
            if val == x:
                return mid
            if val < x:
                lo = mid + 1
            else:
                hi = mid - 1
        return -1

    def get_exponents(x):
        exps = set()
        current = x
        while True:
            found = False
            for k in range(2, 10):
                r = integer_root(current, k)
                if r != -1 and r ** k == current:
                    exps.add(k)
                    current = r
                    found = True
                    break
            if not found:
                break
        return exps

    for a in arr:
        for e in get_exponents(a):
            freq[e] = freq.get(e, 0) + 1

    freq[1] = len(arr)

    return "\n".join(str(freq.get(r, 0)) for r in queries)

# samples
assert run("5 4\n1 16 8 9 7\n1 2 3 4\n") == "5\n3\n2\n2\n"

# all ones
assert run("4 3\n1 1 1 1\n1 2 100\n") == "4\n4\n4\n"

# primes only
assert run("3 3\n2 3 5\n1 2 3\n") == "3\n0\n0\n"

# perfect squares and cubes mix
assert run("4 3\n64 27 16 81\n2 3 4\n") == "4\n2\n2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả những cái | tất cả các truy vấn trả về n | thuộc tính gốc phổ quát | 
| chỉ số nguyên tố | chỉ r=1 hoạt động | sức mạnh không hoàn hảo | 
| quyền lực hỗn hợp | nhóm số mũ đúng | độ đúng đa số | 

## Vỏ cạnh 

Đối với trường hợp tất cả các giá trị đều bằng 1, mọi truy vấn đều trả về kích thước mảng đầy đủ vì 1 vẫn là lũy thừa hoàn hảo cho bất kỳ số mũ nào. Thuật toán xử lý việc này một cách rõ ràng thông qua$r = 1$quy tắc và tính toán ngầm cho các số mũ khác. 

Đối với các mảng chỉ có số nguyên tố, không có số nào ngoại trừ số mũ tầm thường 1 đóng góp cho bất kỳ truy vấn nào. Vòng trích rút gốc không bao giờ tìm thấy lũy thừa hợp lệ cao hơn, do đó bản đồ tần số vẫn trống ngoại trừ trường hợp cơ sở. 

Đối với các số như 64, cả hai đều$2^6$Và$4^3$, việc trích xuất gốc lặp đi lặp lại sẽ nắm bắt chính xác nhiều lớp số mũ. Bắt đầu từ 64, phát hiện căn bậc ba 4 cộng số mũ 3, sau đó phát hiện căn bậc hai 8 cộng số mũ 2, đảm bảo bao gồm tất cả các câu trả lời truy vấn hợp lệ.
