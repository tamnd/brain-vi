---
title: "CF 102437E - \u041f\u043e\u0445\u043e\u0436\u0438\u0435 \u0437\u0430\u043a\u0430\u0437\u044b"
description: "Chúng ta có hai chuỗi có độ dài (n). (Các) chuỗi mô tả ngăn xếp hộp hiện tại, trong khi (t) mô tả ngăn xếp trước đó. Chúng ta có thể xoay (các) theo chu kỳ sang trái một số (k), sau đó áp dụng cùng một phép dịch chuyển Caesar cho mọi ký tự."
date: "2026-08-15T09:19:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102437
codeforces_index: "E"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0427\u0435\u0442\u0432\u0451\u0440\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102437
solve_time_s: 486
verified: false
draft: false
---

[CF 102437E - \u041f\u043e\u0445\u043e\u0436\u0438\u0435 \u0437\u0430\u043a\u0430\u0437\u044b](https://codeforces.com/problemset/problem/102437/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8m 6s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi có độ dài (n). (Các) chuỗi mô tả ngăn xếp hộp hiện tại, trong khi (t) mô tả ngăn xếp trước đó. Chúng ta có thể xoay (các) theo chu kỳ sang trái một số (k), sau đó áp dụng cùng một phép dịch chuyển Caesar cho mọi ký tự. Nhiệm vụ là tìm bất kỳ cặp ((k,d)) nào biến đổi (s) thành (t) hoặc báo cáo rằng không có cặp nào như vậy tồn tại. 

Đối với phép quay theo (k), ký tự kết quả ở vị trí (i) là (s[(i+k)\bmod n]). Nếu phép dịch Caesar di chuyển mọi chữ cái lùi lại một khoảng (d), thì đẳng thức cần có là 

[ 
t_i \equiv s_{(i+k)\bmod n}-d \pmod{26}. 
] 

Độ dài có thể đạt tới (200.000) và giới hạn chính thức là 2 giây và 512 MB. Thuật toán (O(n^2)) có thể thực hiện khoảng (4\cdot10^{10}) so sánh ký tự trong trường hợp xấu nhất, vượt xa những gì phù hợp trong giới hạn thời gian. Chúng ta cần một giải pháp (O(n)) hoặc (O(n\log n)) và thuật toán khớp chuỗi tuyến tính là đủ. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai trực tiếp bị sai lệch. Đối với (n=1), phép quay duy nhất có thể là (k=0), nhưng bất kỳ hai chữ cái nào cũng có thể được chuyển đổi thành nhau bằng phép dịch chuyển Caesar. Ví dụ,```
1
z
a
```có một câu trả lời hợp lệ như`Success`theo sau là`0 25`. Phương pháp so sánh các ký tự liền kề sẽ dường như không có thông tin nào cả, bởi vì chuỗi một ký tự không có cặp liền kề thông thường. 

Trường hợp cạnh thứ hai là phép quay đi qua đầu chuỗi. Ví dụ,```
5
abcde
cdeab
```có câu trả lời`Success`với`3 0`. Xoay đúng là ba ký tự đầu tiên được chuyển xuống dưới cùng. Việc triển khai chỉ kiểm tra các chuỗi con thông thường của (s), thay vì xử lý chuỗi theo chu kỳ, sẽ bỏ lỡ câu trả lời này. 

Các ký tự lặp đi lặp lại tạo ra một trường hợp tinh tế khác. Vì```
4
aaaa
zzzz
```mọi vòng quay đều hợp lệ và chỉ cần một ca Caesar là đủ. Một giải pháp không được cho rằng phép quay phù hợp là duy nhất. 

Cuối cùng, các chuỗi có thể có cùng tần số ký tự trong khi vẫn không thể chuyển đổi. Ví dụ,```
3
abc
aba
```là không thể. Cả hai chuỗi đều chứa ba chữ cái viết thường, nhưng không có vòng quay tuần hoàn của`aba`có thể trở thành`abc`sau một ca làm việc thống nhất. Chỉ so sánh số lượng ký tự sẽ chấp nhận điều này không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi vòng quay (k). Đối với mỗi vòng quay, xây dựng hoặc kiểm tra khái niệm 

[ 
s[k],s[k+1],\ldots,s[n-1],s[0],\ldots,s[k-1]. 
] 

Ký tự đầu tiên xác định sự thay đổi Caesar duy nhất có thể. Khi đã biết sự thay đổi đó, chúng tôi so sánh mọi ký tự còn lại với ký tự tương ứng của (t). Điều này đúng vì đối với một phép quay cố định thì có nhiều nhất một phép dịch chuyển Caesar có thể làm cho các ký tự đầu tiên bằng nhau. 

Vấn đề là số lượng so sánh. Trong trường hợp xấu nhất, có (n) vòng quay và (n) kiểm tra ký tự cho mỗi vòng quay, cho ra thời gian (O(n^2)). Đối với (n=200.000), đó là khoảng (40) tỷ so sánh. 

Quan sát hữu ích là phép dịch chuyển Caesar thay đổi mọi ký tự với cùng một lượng, do đó nó không làm thay đổi sự khác biệt giữa các ký tự liên tiếp. Mã hóa mọi sai phân liền kề theo chu kỳ bằng cách 

[ 
D_i=(x_{i+1}-x_i)\bmod 26, 
] 

ở đâu (x_n=x_0). Ví dụ, sự khác biệt của`abc`là 

[ 
[1,1,24], 
] 

bởi vì`c`ĐẾN`a`là (0-2\equiv24\pmod{26}). 

Giả sử một phiên bản xoay của (s) trở thành (t) sau khi dịch chuyển Caesar. Mọi chênh lệch liền kề trong (các) góc quay khi đó phải bằng chênh lệch liền kề tương ứng trong (t). Một phép quay của (các) chỉ đơn giản là xoay mảng sai phân của nó với cùng một lượng. Do đó, bài toán ban đầu trở thành bài toán so khớp chuỗi tuần hoàn tiêu chuẩn: tìm mảng sai phân của (t) bên trong hai bản sao liên tiếp của mảng sai phân của (s). 

Quan sát này cũng hoạt động theo hướng khác. Nếu các mảng khác biệt khớp nhau dưới một số phép quay thì mỗi cặp liên tiếp sẽ khác nhau một lượng như nhau trong hai chuỗi. Bắt đầu từ một ký tự, độ lệch không đổi đó sẽ lan truyền trong toàn bộ chuỗi, do đó tồn tại một sự thay đổi Caesar. 

Chúng ta có thể tìm góc quay cần thiết bằng thuật toán Knuth-Morris-Pratt. KMP tìm mẫu (D_t) trong (D_s+D_s) theo thời gian tuyến tính mà không cần kiểm tra từng vòng quay riêng biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Mảng chênh lệch + KMP | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mọi ký tự của (s) và (t) thành số nguyên từ (0) đến (25). Xây dựng mảng khác biệt theo chu kỳ của họ. Đối với một chuỗi (x), vị trí (i) lưu trữ ((x[(i+1)\bmod n]-x[i])\bmod26). Sự khác biệt theo chu kỳ từ cuối đến đầu là cần thiết vì các phép quay cũng bảo toàn cạnh giữa vị trí cuối cùng và đầu tiên. 
2. Gọi (A) là mảng sai phân của (s) và (B) là mảng sai phân của (t). Một phép quay trái của (s) theo (k) sẽ quay (A) sang trái chính xác (k) vị trí. Do đó, chúng ta cần tìm (B) dưới dạng đoạn có độ dài-(n) bắt đầu tại vị trí (k) nào đó trong (A+A). 
3. Xây dựng hàm tiền tố KMP cho (B). Hàm tiền tố cho chúng ta biết lượng mẫu vẫn có thể sử dụng được sau khi không khớp, cho phép tìm kiếm bỏ qua so sánh thay vì bắt đầu lại từ đầu. 
4. Chạy KMP trên hai bản sao của (A). Bất cứ khi nào sự xuất hiện đầy đủ của (B) bắt đầu ở vị trí (k<n), chúng ta đã tìm thấy một phép quay bảo toàn tất cả các sai phân tuần hoàn. Chúng ta có thể dừng lại ở lần xuất hiện đầu tiên như vậy. 
5. Khi đã biết (k), hãy so sánh ký tự đầu tiên của (s) được xoay, là (s[k]), với (t[0]). Vì phép biến đổi Caesar di chuyển các ký tự lùi lại (d), 

[ 
t_0\equiv s_k-d\pmod{26}, 
] 

vậy 

[ 
d\equiv s_k-t_0\pmod{26}. 
] 

Việc chọn đại diện từ (0) đến (25) luôn thỏa mãn khoảng yêu cầu (-26<d<26). 

1. Nếu KMP không tìm thấy sự xuất hiện nào bắt đầu ở (n) vị trí đầu tiên, thì không có phép quay nào có độ lệch chu kỳ cần thiết, do đó không tồn tại phép biến đổi hợp lệ. 

### Tại sao nó hoạt động 

Tính bất biến là một phép dịch chuyển Caesar thống nhất sẽ làm cho mọi sai phân liền kề theo chu kỳ không thay đổi. Do đó, một phép biến đổi hợp lệ ngụ ý rằng mảng sai phân của (t) là một phép quay của mảng sai phân của (các) nên KMP phải tìm ra nó. 

Ngược lại, giả sử KMP tìm thấy một phép quay (k) mà hai mảng khác biệt giống hệt nhau. Sau đó, với mỗi cặp liên tiếp, hiệu giữa (s) và (t) được quay là cùng một modulo (26). Do đó tất cả các ký tự tương ứng khác nhau một giá trị không đổi. Hằng số đó chính xác là phép dịch chuyển Caesar (d) được tính từ ký tự đầu tiên, do đó việc áp dụng phép quay (k) và phép dịch chuyển đó biến đổi (s) thành (t). 

Trường hợp (n=1) cũng có lý do tương tự. Cả hai mảng khác biệt đều chứa một giá trị duy nhất (0), do đó KMP tìm thấy phép xoay duy nhất có thể và phép tính ký tự đầu tiên cung cấp độ dịch chuyển Caesar được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def differences(s):
    n = len(s)
    a = [ord(c) - 97 for c in s]
    return [(a[(i + 1) % n] - a[i]) % 26 for i in range(n)], a

def prefix_function(p):
    pi = [0] * len(p)
    for i in range(1, len(p)):
        j = pi[i - 1]
        while j > 0 and p[i] != p[j]:
            j = pi[j - 1]
        if p[i] == p[j]:
            j += 1
        pi[i] = j
    return pi

def solve():
    n = int(input())
    t = input().strip()
    s = input().strip()

    dt, tv = differences(t)
    ds, sv = differences(s)

    pi = prefix_function(dt)

    j = 0
    rotation = -1

    # We only need starts from 0 through n - 1.
    # Two copies of ds contain every cyclic rotation.
    for i in range(2 * n):
        value = ds[i % n]

        while j > 0 and value != dt[j]:
            j = pi[j - 1]

        if value == dt[j]:
            j += 1

        if j == n:
            start = i - n + 1
            if start < n:
                rotation = start
                break
            j = pi[j - 1]

    if rotation == -1:
        print("Impossible")
        return

    # t[0] = s[rotation] - d (mod 26)
    d = (sv[rotation] - tv[0]) % 26

    print("Success")
    print(rotation, d)

if __name__ == "__main__":
    solve()
```các`differences`hàm chuyển đổi các ký tự thành giá trị từ (0) đến (25) và tính toán tất cả các khác biệt theo chu kỳ. biểu thức`(i + 1) % n`xử lý cạnh cuối cùng trở lại ký tự đầu tiên, bao gồm cả trường hợp (n=1). 

Chức năng tiền tố là tiền xử lý KMP tiêu chuẩn. Các chỉ số của nó luôn nằm trong mẫu và`while`vòng lặp liên tục quay trở lại độ dài tiền tố được tính toán trước đó. Vì mọi chuyển động dự phòng`j`đến một giá trị nhỏ hơn thì tổng công vẫn tuyến tính. 

Việc tìm kiếm lặp đi lặp lại`2 * n`vị trí và quyền truy cập`ds[i % n]`, đại diện cho hai bản sao của mảng sai phân tuần hoàn mà không phân bổ một danh sách khác. Trận đấu bắt đầu lúc`start`tương ứng chính xác với việc xoay (các) trái bởi`start`. các`start < n`điều kiện loại bỏ các lần xuất hiện trùng lặp bắt đầu sau (n) vị trí đầu tiên. 

Sự dịch chuyển Caesar chỉ được tính sau khi tìm thấy một phép quay hợp lệ. Chúng tôi sử dụng`(sv[rotation] - tv[0]) % 26`, bởi vì phép biến đổi là một phép dịch ngược. Giá trị kết quả nằm trong (0,\ldots,25), nằm trong phạm vi đầu ra được phép. Số nguyên Python không bị tràn nên không cần xử lý số học đặc biệt. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
3
abc
fde
```Vì`t = abc`, sự khác biệt theo chu kỳ là`1, 1, 24`. Vì`s = fde`, họ là`24, 1, 1`. 

| Chỉ số mẫu |`dt`| Tìm kiếm giá trị từ`ds + ds`| Bang KMP | 
| --- | --- | --- | --- | 
| 0 | 1 | 24 | 0 | 
| 1 | 1 | 1 | 1 | 
| 2 | 24 | 1 | 2 | 
| 3 | 1 | 24 | 3 | 

Mẫu hoàn chỉnh bắt đầu ở vị trí tìm kiếm (1), do đó góc xoay yêu cầu là (k=1). Sau khi quay`fde`còn lại một vị trí, chúng tôi nhận được`def`. Ký tự đầu tiên thay đổi từ`d`ĐẾN`a`, đòi hỏi phải dịch chuyển ngược về (3). 

|`k`| Đã xoay`s`|`t[0]`|`s[k] - t[0]`| Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 |`def`|`a`| (3-0=3) |`Success 1 3`| 

Ví dụ này chứng minh rằng sự khác biệt so khớp sẽ xác định phép quay mà không so sánh tất cả các ký tự của mọi phép quay có thể. 

### Mẫu 2 

Đầu vào là```
3
abc
aba
```Sự khác biệt mang tính chu kỳ của`abc`là`1, 1, 24`. Sự khác biệt mang tính chu kỳ của`aba`là`25, 25, 0`. 

| Chỉ số mẫu |`dt`| Tìm kiếm giá trị từ`ds + ds`| Bang KMP | 
| --- | --- | --- | --- | 
| 0 | 1 | 25 | 0 | 
| 1 | 1 | 25 | 0 | 
| 2 | 24 | 0 | 0 | 
| 3 | 1 | 25 | 0 | 
| 4 | 1 | 25 | 0 | 
| 5 | 24 | 0 | 0 | 

Không có sự xuất hiện đầy đủ của mẫu nên không có phép xoay hợp lệ. Vì phép dịch chuyển Caesar không thể thay đổi các hiệu liền kề nên không có giá trị nào của (d) có thể sửa chữa được sự không khớp này. 

Do đó, đầu ra là```
Impossible
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Việc xây dựng sự khác biệt, tiền xử lý KMP và tìm kiếm đều mất thời gian tuyến tính | 
| Không gian | (O(n)) | Hai mảng sai phân và mảng tiền tố KMP chứa số nguyên (O(n)) | 

Với (n\le200.000), thuật toán chỉ thực hiện một số lần tuyến tính không đổi qua đầu vào, phù hợp với giới hạn 2 giây chính thức. Mức tiêu thụ bộ nhớ cũng ở mức thoải mái dưới giới hạn 512 MB chính thức. 

## Trường hợp thử nghiệm 

Khai thác kiểm tra bên dưới không so sánh các câu trả lời thành công với một cặp cố định ((k,d)), vì vấn đề rõ ràng cho phép bất kỳ chuyển đổi hợp lệ nào. Thay vào đó, nó kiểm tra xem cặp được báo cáo có nằm trong phạm vi hay không và thực sự chuyển đổi (các) thành (t). Những trường hợp bất khả thi được so sánh chính xác.```python
import sys
import io

def solve_case(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    t = sys.stdin.readline().strip()
    s = sys.stdin.readline().strip()

    def differences(x):
        values = [ord(c) - 97 for c in x]
        return [
            (values[(i + 1) % n] - values[i]) % 26
            for i in range(n)
        ], values

    def prefix_function(p):
        pi = [0] * len(p)
        for i in range(1, len(p)):
            j = pi[i - 1]
            while j > 0 and p[i] != p[j]:
                j = pi[j - 1]
            if p[i] == p[j]:
                j += 1
            pi[i] = j
        return pi

    dt, tv = differences(t)
    ds, sv = differences(s)

    pi = prefix_function(dt)

    j = 0
    rotation = -1

    for i in range(2 * n):
        value = ds[i % n]

        while j > 0 and value != dt[j]:
            j = pi[j - 1]

        if value == dt[j]:
            j += 1

        if j == n:
            start = i - n + 1
            if start < n:
                rotation = start
                break
            j = pi[j - 1]

    if rotation == -1:
        result = "Impossible\n"
    else:
        d = (sv[rotation] - tv[0]) % 26
        result = f"Success\n{rotation} {d}\n"

    sys.stdin = old_stdin
    return result

def is_valid(inp: str, out: str) -> bool:
    lines = inp.strip().splitlines()
    n = int(lines[0])
    t = lines[1]
    s = lines[2]

    out_lines = out.strip().splitlines()

    if out_lines[0] == "Impossible":
        return False

    assert out_lines[0] == "Success"
    k, d = map(int, out_lines[1].split())

    assert 0 <= k < n
    assert -26 < d < 26

    for i in range(n):
        source = ord(s[(i + k) % n]) - 97
        target = (source - d) % 26
        if target != ord(t[i]) - 97:
            return False

    return True

# Provided samples.
sample1 = """3
abc
fde
"""
assert is_valid(sample1, solve_case(sample1)), "sample 1"

sample2 = """3
abc
aba
"""
assert solve_case(sample2).strip() == "Impossible", "sample 2"

sample3 = """1
z
a
"""
assert is_valid(sample3, solve_case(sample3)), "sample 3"

# Minimum size, where the difference arrays contain only zero.
case1 = """1
a
z
"""
assert is_valid(case1, solve_case(case1)), "minimum size"

# Rotation crosses the end of the string.
case2 = """5
abcde
cdeab
"""
assert is_valid(case2, solve_case(case2)), "wrap-around rotation"

# All characters are equal, and n is at the maximum allowed size.
n = 200000
case3 = f"{n}\n" + "a" * n + "\n" + "z" * n + "\n"
assert is_valid(case3, solve_case(case3)), "maximum size and all equal"

# Almost matching strings, designed to reject a wrong rotation.
case4 = """4
abca
caab
"""
assert solve_case(case4).strip() == "Impossible", "invalid rotation"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / a / z`| Bất kỳ hợp lệ`Success`| Kích thước tối thiểu và thực tế là một ký tự luôn có thể được dịch chuyển | 
|`5 / abcde / cdeab`| Bất kỳ hợp lệ`Success`, với (k=3,d=0) là một câu trả lời | Xoay quanh và hướng quay chính xác | 
| (n=200000), cả hai chuỗi đều không đổi | Bất kỳ hợp lệ`Success`| Kích thước đầu vào tối đa, ký tự lặp lại và hiệu suất tuyến tính | 
|`4 / abca / caab`|`Impossible`| Từ chối một vòng quay có sự khác biệt cục bộ không khớp | 

## Vỏ cạnh 

Với (n=1), hãy xem xét```
1
z
a
```Mảng khác biệt của cả hai chuỗi là`[0]`, bởi vì vị trí duy nhất cũng là vị trí kế thừa theo chu kỳ của chính nó. KMP ngay lập tức tìm thấy kết quả phù hợp khi xoay vòng (k=0). Sự thay đổi là 

[ 
d=(25-0)\bmod26=25, 
] 

để chương trình có thể in```
Success
0 25
```Mẫu của`0 -25`là một cách biểu diễn khác được chấp nhận bởi quy ước dịch chuyển được phép của bài toán. Điều kiện thiết yếu là cặp được báo cáo tạo ra ký tự đích. 

Đối với một vòng quay qua điểm cuối, hãy xem xét```
5
abcde
cdeab
```Mảng khác biệt của`abcde`là`[1,1,1,1,22]`, trong khi mảng khác biệt của`cdeab`là`[1,1,22,1,1]`. Mảng thứ hai bắt đầu ở vị trí (3) trong chuỗi tuần hoàn của mảng thứ nhất, do đó KMP tìm thấy (k=3). Nguồn quay là`cdeab`, đã bằng mục tiêu, cho (d=0). 

Đối với các ký tự lặp lại, hãy xem xét```
4
aaaa
zzzz
```Cả hai mảng sai phân theo chu kỳ đều`[0,0,0,0]`. KMP tìm phép quay (0) và các ký tự đầu tiên cho 

[ 
d=(25-0)\bmod26=25. 
] 

Mỗi nhân vật của`zzzz`chuyển ngược lại bởi (25) trở thành`aaaa`. Thực tế là tất cả các phép quay đều hợp lệ không gây ra vấn đề gì vì câu lệnh cho phép bất kỳ câu trả lời hợp lệ nào. 

Đối với một cặp không thể, hãy xem xét```
3
abc
aba
```Sự khác biệt mục tiêu là`[1,1,24]`, trong khi sự khác biệt về nguồn là`[25,25,0]`. Không có vòng quay theo chu kỳ nào có thể biến chuỗi này thành chuỗi khác, vì vậy KMP không bao giờ đạt được kết quả khớp mẫu đầy đủ. Thuật toán in`Impossible`mà không cố gắng đoán sự dịch chuyển của Caesar. Đây chính xác là lý do tại sao chỉ kiểm tra số lượng ký tự sẽ không đủ.
