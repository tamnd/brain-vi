---
title: "CF 102361J - MUV LUV EXTRA"
description: "Chỉ các chữ số sau dấu thập phân mới quan trọng. Đặt phần phân số đó có độ dài (n). Phần lặp lại ứng cử viên phải là hậu tố của các chữ số phân số được quan sát, bởi vì sự lặp lại phải tiếp tục ở cuối phần mà Sumika đo được."
date: "2026-08-13T00:15:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102361
codeforces_index: "J"
codeforces_contest_name: "2019 China Collegiate Programming Contest Qinhuangdao Onsite"
rating: 0
weight: 102361
solve_time_s: 95
verified: true
draft: false
---

[CF 102361J - MUV LUV EXTRA](https://codeforces.com/problemset/problem/102361/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chỉ các chữ số sau dấu thập phân mới quan trọng. Đặt phần phân số đó có độ dài (n). Phần lặp lại ứng cử viên phải là hậu tố của các chữ số phân số được quan sát, bởi vì sự lặp lại phải tiếp tục ở cuối phần mà Sumika đo được. 

Giả sử một hậu tố ứng cử viên đã xuất hiện với tổng số (p) chữ số, bao gồm bản sao cuối cùng có thể không đầy đủ của khối lặp và khối lặp hợp lệ ngắn nhất của nó có độ dài (l). Độ tin cậy của nó là 

[ 
một p-b l. 
] 

Đối với (p) cố định, số hạng đầu tiên (ap) đã được xác định. Vì (b>0), ứng viên tốt nhất kết thúc ở vị trí đó luôn là ứng viên có thời gian (l) nhỏ nhất có thể. Do đó, vấn đề là tìm, với mỗi độ dài hậu tố (p), khoảng thời gian ngắn nhất của hậu tố đó. 

Phần phân số có thể chứa tối đa (10^7) chữ số. Một thuật toán kiểm tra tất cả các cặp độ dài hậu tố và độ dài chu kỳ đã quá lớn ở quy mô này. Công việc thậm chí (O(n^2)) có nghĩa là khoảng (10^{14}) lần lặp ở kích thước tối đa, trong khi việc triển khai so sánh ký tự (O(n^3)) có thể đạt được khoảng 

[ 
\sum_{p=1}^{n}\frac{p(p-1)}2 
=\frac{n(n+1)(n-1)}6 
] 

so sánh ký tự, khoảng (1,67\time10^{20}) khi (n=10^7). Chúng ta cần một thuật toán chuỗi thời gian tuyến tính và chúng ta cũng chỉ cần bộ nhớ tuyến tính. 

Có một số trường hợp ranh giới có thể khiến việc triển khai dường như đúng không thành công. Câu trả lời có thể là tiêu cực. Ví dụ, với```
1 2
0.5
```phần lặp lại duy nhất có thể có (p=1,l=1), vì vậy câu trả lời là (-1). Khởi tạo câu trả lời bằng 0 sẽ tạo ra số 0 không chính xác. 

Phần nguyên phải được bỏ qua hoàn toàn. Ví dụ,```
5 3
123.1020
```có câu trả lời giống hệt như mẫu với`1.1020`, cụ thể là`9`. Việc triển khai xử lý toàn bộ chuỗi sẽ tạo ra các giai đoạn sai. 

Sự lặp lại cuối cùng được phép không đầy đủ. Vì```
2 1
0.102
```chuỗi phân số đảo ngược là`201`. Tiền tố có độ dài ba của nó có chu kỳ hai ngắn nhất, do đó (p=3,l=2), cho (6-2=4). Việc hạn chế các ứng cử viên trong các khoảng thời gian chia theo độ dài quan sát được sẽ bỏ lỡ loại ứng cử viên này. 

Một phần phân số của độ dài một cũng hợp lệ. Vì```
7 10
42.8
```chỉ có (p=l=1), nên đáp án là (-3). Điều này nắm bắt cả ranh giới một ký tự và câu trả lời phủ định. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ liệt kê mọi độ dài hậu tố có thể có (p). Đối với mỗi hậu tố, nó sẽ thử mọi độ dài lặp lại có thể (l), kiểm tra xem các ký tự ở khoảng cách (l) có thống nhất trong toàn bộ hậu tố hay không. Nếu kiểm tra thành công, ứng viên có độ tin cậy (ap-bl) và vì (b) dương nên chúng tôi có thể giữ (l) hợp lệ nhỏ nhất cho hậu tố đó. 

Điều này đúng vì mỗi phần lặp lại hợp pháp là một khoảng thời gian của một hậu tố nào đó và việc kiểm tra tất cả các khả năng (l) sẽ kiểm tra mọi lựa chọn hợp pháp. Vấn đề là số lượng so sánh chuỗi lặp đi lặp lại. Đối với hậu tố có độ dài (p), việc kiểm tra tất cả độ dài ứng viên có thể yêu cầu so sánh ký tự (\Theta(p^2)), đưa ra (\Theta(n^3)) tổng thể trong trường hợp xấu nhất. Ngay cả khi các phép so sánh chuỗi con được tối ưu hóa đủ để khiến mỗi lần kiểm tra có thời gian không đổi, thì việc chỉ kiểm tra từng cặp ((p,l)) vẫn sẽ là (O(n^2)), quá lớn đối với (10^7) ký tự. 

Quan sát hữu ích là mọi ứng cử viên đều là hậu tố của chuỗi phân số ban đầu. Đảo ngược chuỗi phân số. Hậu tố có độ dài (p) trong chuỗi gốc trở thành tiền tố có độ dài (p) trong chuỗi đảo ngược. Do đó, chúng ta đã chuyển bài toán sang tìm khoảng thời gian ngắn nhất của mọi tiền tố. 

Đây chính xác là thông tin được cung cấp bởi chức năng tiền tố KMP. Đối với tiền tố có độ dài (p), hãy đặt giá trị hàm tiền tố của nó là (k), độ dài của tiền tố thích hợp dài nhất cũng là hậu tố. Đường viền có độ dài (k) có nghĩa là ký tự đầu tiên (k) và ký tự (k) cuối cùng trùng nhau. Khoảng cách giữa hai vùng bằng nhau đó là (p-k) và khoảng cách đó là một khoảng thời gian của tiền tố. Vì (k) là đường biên dài nhất có thể nên (p-k) là khoảng thời gian ngắn nhất có thể. 

Do đó, nếu chuỗi phân số đảo ngược là (t) và hàm tiền tố của nó là (\pi), thì với mỗi độ dài tiền tố (p), 

[ 
l=p-\pi[p-1]. 
] 

Độ dài xuất hiện đơn giản là (p), vì vậy độ tin cậy tốt nhất của nó là 

[ 
một p-b(p-\pi[p-1]). 
] 

Chúng tôi tính toán điều này cho mọi tiền tố trong khi xây dựng hàm tiền tố KMP. 

Cách tiếp cận bạo lực hoạt động vì nó kiểm tra rõ ràng mọi giai đoạn có thể, nhưng không thành công vì các ký tự giống nhau được so sánh nhiều lần cho các hậu tố có liên quan chặt chẽ. Đảo ngược chuyển đổi tất cả các hậu tố có liên quan thành tiền tố và KMP sử dụng lại thông tin đường viền giữa các tiền tố liên tiếp, giảm toàn bộ vấn đề về thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^3)) với kiểm tra định kỳ trực tiếp | (O(n)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (a), (b) và chuỗi thập phân, sau đó loại bỏ mọi thứ cho đến dấu thập phân. Phần nguyên không có vai trò gì trong việc tính toán độ tin cậy, vì vậy việc giữ lại nó sẽ chỉ lãng phí bộ nhớ và làm phức tạp quá trình xử lý chuỗi. 
2. Đảo ngược các chữ số phân số. Phần lặp lại ứng cử viên phải đến cuối chuỗi được quan sát ban đầu, vì vậy mọi ứng cử viên đều tương ứng với một hậu tố. Sau khi đảo ngược, hậu tố đó trở thành tiền tố, chính xác là dạng được xử lý bởi hàm tiền tố. 
3. Phân bổ mảng hàm tiền tố KMP cho chuỗi phân số đảo ngược. Đối với vị trí (i), đặt (\pi[i]) là tiền tố thích hợp dài nhất của`t[:i+1]`đó cũng là hậu tố của nó. 
4. Xây dựng hàm tiền tố bằng quy tắc dự phòng KMP tiêu chuẩn. Khi ký tự tiếp theo không khớp với đường viền hiện tại, hãy liên tục thay thế độ dài đường viền (j) bằng (\pi[j-1]). Những dự phòng này bỏ qua toàn bộ nhóm ký tự mà việc so sánh được cho là không thành công. 
5. Với mọi tiền tố kết thúc ở vị trí (i), đặt (p=i+1). Đường viền dài nhất của nó có chiều dài (\pi[i]), vì vậy khoảng thời gian ngắn nhất của nó là 

[ 
l=p-\pi[i]. 
] 

Lý do điều này mang lại phần lặp lại hợp lệ ngắn nhất là mối quan hệ giữa thời kỳ biên giới. Đường viền có độ dài (k) làm cho chuỗi lặp lại sau các vị trí (p-k) và đường viền dài nhất tạo ra khoảng cách nhỏ nhất như vậy. 

1. Tính toán 

[ 
a p-b l 
] 

cho tiền tố này và cập nhật tối đa. Vì (p) tăng từ một đến độ dài phân số đầy đủ nên mọi độ dài có thể xuất hiện được coi là chính xác một lần. 

1. Xuất giá trị lớn nhất. Câu trả lời phải được khởi tạo bằng cách sử dụng ứng cử viên đầu tiên, (a-b), thay vì 0, vì tất cả các giá trị độ tin cậy có thể âm. 

### Tại sao nó hoạt động 

Xem xét bất kỳ phần lặp lại hợp pháp nào trong chuỗi phân số ban đầu. Bởi vì nó vẫn lặp lại ở cuối nên tất cả các chữ số liên quan đến sự xuất hiện của nó tạo thành một hậu tố nào đó của chuỗi phân số. Hãy để hậu tố đó chứa (p) chữ số. Sau khi đảo ngược chuỗi phân số, các ký tự giống nhau sẽ tạo thành tiền tố có độ dài (p). 

Đối với tiền tố này, mỗi độ dài khối lặp lại hợp lệ là một khoảng thời gian của tiền tố. Nếu đường viền dài nhất của nó có chiều dài (k) thì (p-k) là một dấu chấm. Vì không có đường viền nào có thể dài hơn (k) nên không có khoảng thời gian nào có thể ngắn hơn (p-k). Do đó, giá trị KMP đưa ra chính xác giá trị nhỏ nhất có thể có (l) cho (p) cố định này. 

Đối với (p cố định), độ tin cậy là (ap-bl) và (b) là dương. (l) nhỏ hơn luôn cho độ tin cậy lớn hơn. Do đó, chỉ xem xét (l=p-\pi[p-1]) không mất ứng cử viên tối ưu nào. Vì mọi (p) có thể đều được kiểm tra nên giá trị tối đa mà thuật toán tìm thấy là độ tin cậy tối đa trên tất cả các phần lặp lại hợp lệ. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    a, b = map(int, input().split())
    s = input().strip()

    # Only the fractional part matters.
    frac = s.split('.', 1)[1]
    n = len(frac)

    # A signed 32-bit integer is enough for every prefix-function value.
    # Using array instead of a Python list keeps memory close to O(n) bytes
    # rather than O(n) Python objects.
    pi = array('i', [0]) * n

    if n == 0:
        print(a - b)
        return

    # Prefix of length 1: p = 1, l = 1.
    ans = a - b

    # We need suffixes of frac, so process the reversed string.
    t = frac[::-1]

    j = 0
    for i in range(1, n):
        while j > 0 and t[i] != t[j]:
            j = pi[j - 1]

        if t[i] == t[j]:
            j += 1

        pi[i] = j

        p = i + 1
        l = p - j
        value = a * p - b * l

        if value > ans:
            ans = value

    print(ans)

if __name__ == "__main__":
    solve()
```Dòng đầu vào đầu tiên cung cấp (a) và (b), trong khi dòng thứ hai chứa số thập phân được quan sát đầy đủ.`split('.', 1)[1]`trích xuất chính xác phần phân số, vì vậy các chữ số trước dấu thập phân không bao giờ tham gia KMP. 

Chuỗi đảo ngược`t`được tạo vì các ứng cử viên có liên quan là hậu tố của các chữ số phân số ban đầu. Giá trị tiền tố-hàm tại chỉ mục`i`mô tả tiền tố có độ dài`i+1`, tương ứng với hậu tố có độ dài chính xác theo hướng ban đầu. 

các`pi`mảng sử dụng số nguyên có dấu bốn byte. Mỗi giá trị hàm tiền tố tối đa là (n) và (n\le10^7), do đó, số nguyên có dấu 32 bit là đủ. Lựa chọn này quan trọng trong Python vì một danh sách bình thường gồm mười triệu số nguyên Python có thể tiêu tốn vài trăm megabyte, trong khi mảng được gõ sẽ lưu trữ các giá trị tương tự một cách gọn gàng. 

Vòng lặp KMP bắt đầu lúc`i = 1`bởi vì ký tự đầu tiên luôn có giá trị tiền tố bằng 0. Biến`j`là chiều dài đường viền hiện tại. Trên một sự không phù hợp,`j = pi[j - 1]`nhảy tới đường viền nhỏ hơn tiếp theo thay vì bắt đầu lại quá trình so sánh từ đầu. 

Sau khi tính toán`pi[i]`, độ dài tiền tố là`p = i + 1`. Khoảng thời gian ngắn nhất là`p - pi[i]`, do đó độ tin cậy được tính trực tiếp từ hai giá trị đó. Thứ tự trừ quan trọng vì (a p-b l) có thể âm. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn trong quá trình triển khai. Tuy nhiên, trong giải pháp C++, các sản phẩm có thể đạt khoảng (10^{16}), vì vậy`long long`được yêu cầu. 

Vòng lặp dự phòng KMP trông có vẻ được lồng bên trong vòng lặp chính nhưng tổng công việc của nó vẫn tuyến tính. Mỗi trận đấu nhân vật thành công đều tăng lên`j`, trong khi mọi dự phòng đều giảm nghiêm ngặt`j`. Trong toàn bộ quá trình tính toán, số lượng thao tác dự phòng bị giới hạn bởi số lần tăng, tạo ra tổng công việc là (O(n)). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Phần phân số là`1020`. Đảo ngược nó mang lại`0201`. 

| Độ dài tiền tố (p) | Tiền tố | (\pi[p-1]) | Dấu chấm (l=p-\pi[p-1]) | Độ tin cậy (5p-3l) | Tốt nhất cho đến nay | 
| --- | --- | --- | --- | --- | --- | 
| 1 |`0`| 0 | 1 | 2 | 2 | 
| 2 |`02`| 0 | 2 | 4 | 4 | 
| 3 |`020`| 1 | 2 | 9 | 9 | 
| 4 |`0201`| 0 | 4 | 8 | 9 | 

Ở tiền tố có độ dài ba,`020`có biên giới`0`có chiều dài một. Xóa đường viền đó khỏi độ dài tiền tố sẽ có dấu chấm (3-1=2). Đảo ngược lại tương ứng với hậu tố ban đầu`020`, khối lặp lại của nó là`02`, xuất hiện với ba chữ số. Độ tin cậy của nó là (5\cdot3-3\cdot2=9), đây là mức tối ưu. 

Điểm mấu chốt là ứng cử viên tốt nhất không nhất thiết phải có đủ số lần lặp lại. Tiền tố`020`bao gồm`02`theo sau là ký tự đầu tiên`0`của một bản sao khác. Bản sao cuối cùng một phần đó được cho phép. 

### Mẫu 2 

Phần phân số là`1212`, vậy chuỗi đảo ngược là`2121`. 

| Độ dài tiền tố (p) | Tiền tố | (\pi[p-1]) | Dấu chấm (l=p-\pi[p-1]) | Độ tin cậy (2p-l) | Tốt nhất cho đến nay | 
| --- | --- | --- | --- | --- | --- | 
| 1 |`2`| 0 | 1 | 1 | 1 | 
| 2 |`21`| 0 | 2 | 2 | 2 | 
| 3 |`212`| 1 | 2 | 4 | 4 | 
| 4 |`2121`| 2 | 2 | 6 | 6 | 

Đối với tiền tố hoàn chỉnh`2121`, biên giới dài nhất là`21`, có độ dài bằng hai. Do đó khoảng thời gian ngắn nhất là (4-2=2). Theo hướng ban đầu, phần này tương ứng với phần lặp lại`12`, xuất hiện cho cả bốn chữ số được quan sát. Độ tin cậy của nó là (2\cdot4-1\cdot2=6). 

Dấu vết cũng cho thấy tại sao chỉ kiểm tra các tiền tố không lặp lại một phần là không đủ. Tiền tố có độ dài ba có chu kỳ hai mặc dù ba không chia hết cho hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Trích xuất, đảo ngược, xây dựng KMP và đánh giá tất cả các tiền tố đều là tuyến tính | 
| Không gian | (O(n)) | Cả chuỗi phân số và mảng hàm tiền tố đều có kích thước tuyến tính | 

Ở đây (n) là số chữ số sau dấu thập phân, với (n\le10^7). Xử lý tuyến tính là thang đo cần thiết cho kích thước đầu vào. Mảng hàm tiền tố được gõ cũng giữ tỷ lệ bộ nhớ với đầu vào thay vì lưu trữ hàng triệu đối tượng số nguyên Python riêng biệt. 

## Trường hợp thử nghiệm```python
import sys
import io
from array import array

def solve_text(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        a, b = map(int, sys.stdin.readline().split())
        s = sys.stdin.readline().strip()

        frac = s.split('.', 1)[1]
        n = len(frac)

        if n == 0:
            return str(a - b)

        t = frac[::-1]
        pi = array('i', [0]) * n

        ans = a - b
        j = 0

        for i in range(1, n):
            while j > 0 and t[i] != t[j]:
                j = pi[j - 1]

            if t[i] == t[j]:
                j += 1

            pi[i] = j

            p = i + 1
            l = p - j
            value = a * p - b * l

            if value > ans:
                ans = value

        return str(ans)
    finally:
        sys.stdin = old_stdin

def run(inp: str) -> str:
    return solve_text(inp)

# Provided samples
assert run("5 3\n1.1020\n") == "9", "sample 1"
assert run("2 1\n12.1212\n") == "6", "sample 2"

# Minimum-size fractional part
assert run("7 10\n42.8\n") == "-3", "single fractional digit"

# Negative answer
assert run("1 2\n0.5\n") == "-1", "answer may be negative"

# All equal digits
assert run("2 1\n0.1111\n") == "7", "shortest period remains 1"

# Partial final repetition and off-by-one boundary
assert run("2 1\n0.121\n") == "4", "partial final repetition"

# Integer part must be ignored
assert run("5 3\n987654321.1020\n") == "9", "integer part is irrelevant"

# Maximum-size stress case.
# Ten million equal fractional digits have period 1 for every prefix.
# The final prefix gives p = 10,000,000 and l = 1.
n = 10_000_000
maximum_case = "1 1\n0." + ("7" * n) + "\n"
assert run(maximum_case) == "9999999", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`7 10 / 42.8`|`-3`| Độ dài phân số tối thiểu và kết quả âm tính | 
|`1 2 / 0.5`|`-1`| Ngăn chặn việc khởi tạo sai câu trả lời về 0 | 
|`2 1 / 0.1111`|`7`| Các chữ số hoàn toàn bằng nhau và dấu chấm là một | 
|`2 1 / 0.121`|`4`| Sự lặp lại một phần cuối cùng và ranh giới hàm tiền tố | 
|`5 3 / 987654321.1020`|`9`| Xác nhận rằng phần nguyên bị bỏ qua | 
|`1 1 / 0.`theo sau là (10^7) bản sao của`7`|`9999999`| Kích thước đầu vào tối đa và xử lý bộ nhớ tuyến tính | 

Bài kiểm tra kích thước tối đa có chủ ý sử dụng các chữ số bằng nhau vì nó nhấn mạnh toàn bộ cấu trúc hàm tiền tố trong khi vẫn giữ cho câu trả lời mong đợi dễ dàng rút ra. Đối với (n=10^7), mọi tiền tố đều có chu kỳ một ngắn nhất, do đó ứng cử viên cuối cùng có (p=10^7), (l=1) và độ tin cậy (10^7-1=9,999,999). 

## Vỏ cạnh 

Đối với câu trả lời phủ định, hãy xem xét```
1 2
0.5
```Chuỗi phân số có độ dài bằng 1 nên chuỗi đảo ngược cũng có độ dài`5`. Giá trị tiền tố-hàm của nó bằng 0, cho (p=1) và (l=1). Độ tin cậy duy nhất là (1\cdot1-2\cdot1=-1). Thuật toán bắt đầu với`ans = a - b`, do đó nó giữ nguyên kết quả âm này thay vì vô tình thay thế nó bằng 0. 

Đối với một chữ số phân số, hãy xem xét```
7 10
42.8
```Thuật toán trích xuất`8`, đảo ngược nó thành`8`và không có lần lặp KMP nào ngoài tiền tố ban đầu. Khoảng thời gian là một và độ tin cậy là (7-10=-3). Không có trường hợp mẫu chuỗi đặc biệt nào ẩn ở đây, vì vậy quá trình khởi tạo thông thường đã xử lý đầu vào hợp lệ nhỏ nhất. 

Đối với sự lặp lại cuối cùng không đầy đủ, hãy xem xét```
2 1
0.121
```Chuỗi phân số là`121`, và đảo ngược mang lại`121`. Các giá trị tiền tố-hàm là (0,0,1). Tại (p=3), đường viền dài nhất có chiều dài bằng 1, do đó (l=3-1=2). Ứng cử viên là hậu tố ban đầu`121`, được hiểu là khoảng thời gian`12`theo sau là chữ số đầu tiên của nó`1`. Độ tin cậy của nó là (2\cdot3-1\cdot2=4). Một giải pháp yêu cầu (p) là bội số của (l) sẽ loại bỏ ứng viên này một cách không chính xác. 

Đối với ranh giới phần nguyên, hãy xem xét```
5 3
987654321.1020
```Sau dấu thập phân, thuật toán thấy chính xác`1020`, tạo ra chuỗi đảo ngược tương tự`0201`và bốn ứng cử viên giống như mẫu đầu tiên. Còn lại tối đa`9`. Điều này xác nhận rằng phần nguyên không thể ảnh hưởng đến (p), (l) hoặc độ tin cậy. 

Đối với các chữ số lặp lại, hãy xem xét```
2 1
0.1111
```Chuỗi đảo ngược vẫn là`1111`. Các giá trị hàm tiền tố của nó là (0,1,2,3), vì vậy đối với tiền tố đầy đủ (p=4), khoảng thời gian ngắn nhất là (4-3=1). Độ tin cậy là (2\cdot4-1=7), vượt trội hơn tất cả các tiền tố ngắn hơn. Trường hợp này thực hiện chuỗi dài các đường viền KMP và xác nhận rằng cấu trúc dự phòng xử lý sự lặp lại tối đa mà không suy biến thành công việc bậc hai.
