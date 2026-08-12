---
title: "CF 102437E - \u041f\u043e\u0445\u043e\u0436\u0438\u0435 \u0437\u0430\u043a\u0430\u0437\u044b"
description: "Chúng ta có hai đơn hàng, mỗi đơn hàng được biểu thị bằng một chuỗi có độ dài (n). Ký tự thứ (i)-th mô tả số bài viết của hộp thứ (i)-th trong ngăn xếp. Chúng ta cần xác định xem (các) đơn hàng hiện tại có thể được chuyển đổi thành đơn hàng (t) trước đó hay không."
date: "2026-08-12T07:59:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102437
codeforces_index: "E"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0427\u0435\u0442\u0432\u0451\u0440\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102437
solve_time_s: 836
verified: false
draft: false
---

[CF 102437E - \u041f\u043e\u0445\u043e\u0436\u0438\u0435 \u0437\u0430\u043a\u0430\u0437\u044b](https://codeforces.com/problemset/problem/102437/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 13 phút 56 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai đơn hàng, mỗi đơn hàng được biểu thị bằng một chuỗi có độ dài (n). Ký tự thứ (i)-th mô tả số bài viết của hộp thứ (i)-th trong ngăn xếp. Chúng ta cần xác định xem (các) đơn hàng hiện tại có thể được chuyển đổi thành đơn hàng (t) trước đó hay không. 

Phép biến đổi được phép có hai phần độc lập. Đầu tiên, mọi chữ cái đều được dịch chuyển bằng cùng một dịch chuyển Caesar (d), theo chu kỳ modulo 26. Thứ hai, ngăn xếp có thể được xoay, nghĩa là tiền tố của (s) được di chuyển từ trên xuống dưới. Nếu số lượng vòng quay là (k), thứ tự kết quả là 

[ 
s[k:] + s[]. 
] 

Sau cả hai thao tác, chuỗi kết quả phải bằng (t). Chúng ta phải xuất ra bất kỳ cặp hợp lệ nào ((k,d)) hoặc báo cáo`Impossible`. 

Độ dài có thể lớn tới (200.000). Một thuật toán kiểm tra mọi phép quay và so sánh tất cả (n) ký tự sẽ thực hiện so sánh tối đa (n^2 = 40.000.000.000) ký tự trong trường hợp xấu nhất, vượt xa những gì thực tế. Chúng ta cần một giải pháp mà công việc của nó về cơ bản là tuyến tính theo độ dài chuỗi. 

Có một số trường hợp khó khăn có thể phá vỡ quá trình triển khai đơn giản. Đầu tiên là (n=1). Không có sự xoay vòng có ý nghĩa nào để tìm kiếm, nhưng sự dịch chuyển Caesar có thể vẫn cần thiết. Ví dụ,```
1
z
a
```có thể giải được với (k=0), vì dịch chuyển`z`lùi lại 25 cho`a`. Việc triển khai giả định có các ký tự liền kề cần kiểm tra sẽ không thành công trong trường hợp này. 

Một trường hợp cạnh khác được bao bọc trong bảng chữ cái. Ví dụ,```
1
a
z
```cũng có thể giải quyết được. Sự dịch chuyển cần thiết có thể được biểu diễn dưới dạng (d=1), bởi vì sự dịch chuyển`z`lùi lại 1 cho`y`, trong khi dịch chuyển`a`lùi lại 1 cho`z`. Phép tính số học phải được thực hiện theo modulo 26 thay vì sử dụng sai số nguyên thông thường. 

Vấn đề thứ ba là xoay vòng ở cuối chuỗi. Coi như```
5
abcde
bcdea
```Xoay`bcdea`bởi (4) vị trí tạo ra`abcde`, do đó câu trả lời đúng tồn tại với (k=4) và (d=0). Một tìm kiếm chỉ kiểm tra các chuỗi con thông thường của`s`và quên ranh giới tuần hoàn sẽ bỏ lỡ giải pháp này. 

Cuối cùng, các ký tự lặp lại có thể làm cho một số phép quay hợp lệ. Ví dụ,```
4
aaaa
aaaa
```có mọi vòng quay là một vòng quay hợp lệ và (d=0) hoạt động với tất cả chúng. Thuật toán phải chấp nhận ứng viên hợp lệ đầu tiên thay vì dựa vào tính duy nhất. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là thử mọi phép quay có thể (k). Đối với mỗi phép quay, chúng ta sẽ so sánh mọi ký tự của (các) được xoay với ký tự tương ứng của (t). Cặp vị trí đầu tiên xác định độ dịch chuyển Caesar, sau đó mọi vị trí còn lại phải có cùng độ dịch chuyển theo modulo 26. Phương pháp này đúng vì nó kiểm tra rõ ràng mọi phép biến đổi có thể có. 

Vấn đề là số lượng công việc lặp đi lặp lại. Có (n) vòng quay và việc kiểm tra một vòng quay mất (O(n)) thời gian. Tại (n=200.000), trường hợp xấu nhất đạt tới (200.000^2=40.000.000.000) kiểm tra ký tự. Lực lượng vũ phu đơn giản về mặt khái niệm, nhưng hành vi bậc hai của nó loại trừ nó. 

Quan sát hữu ích là sự dịch chuyển Caesar không làm thay đổi sự khác biệt giữa các chữ cái lân cận. Nếu như`x`được đổi thành`x-d`Và`y`được đổi thành`y-d`, thì sự khác biệt của họ vẫn còn 

[ 
(y-d)-(x-d)=y-x \pmod {26}. 
] 

Vì vậy, thay vì so sánh các chữ cái gốc, chúng ta có thể so sánh trình tự khác nhau theo chu kỳ giữa các chữ cái liên tiếp. 

Đối với một chuỗi (x), hãy xác định 

(x[(i+1)\bmod n]-x[i])\bmod 26. 
] 

Chuỗi này có chính xác (n) phần tử vì nó cũng chứa sự khác biệt từ ký tự cuối cùng đến ký tự đầu tiên. 

Giả sử xoay (các) vị trí theo (k) sẽ có sự sắp xếp chính xác trước khi chuyển Caesar. Dãy sai phân tuần hoàn của nó đơn giản là dãy sai phân tuần hoàn của (s), bắt đầu từ vị trí (k). Sự dịch chuyển Caesar sau đó thay đổi không có sự khác biệt nào cả. Do đó, một phép quay hợp lệ tồn tại chính xác khi chuỗi sai phân tuần hoàn của (t) xảy ra dưới dạng phép quay tuần hoàn của chuỗi sai phân tuần hoàn của (s). 

Tìm một chuỗi tuần hoàn bên trong một chuỗi khác là một vấn đề so khớp chuỗi tiêu chuẩn. Chúng ta có thể ghép chuỗi sai phân của (các) với chính nó và sử dụng thuật toán Knuth-Morris-Pratt để tìm chuỗi sai phân của (t) trong thời gian (O(n)). Khi tìm thấy vị trí bắt đầu phù hợp (k), sự dịch chuyển Caesar được xác định bởi ký tự đầu tiên: 

[ 
d=(s[k]-t[0])\bmod 26. 
] 

Biểu diễn sự khác biệt giải quyết vấn đề xoay vòng, trong khi KMP làm cho tìm kiếm tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mọi ký tự của (s) và (t) thành giá trị số của nó từ 0 đến 25. Điều này cho phép chúng ta thực hiện tất cả số học dịch chuyển Caesar với số học mô-đun thông thường. 
2. Xây dựng mảng sai phân tuần hoàn`ds`cho (các) cái. Đối với mọi vị trí (i), lưu trữ chênh lệch từ (s[i]) đến (s[(i+1)\bmod n]), modulo 26. Xây dựng`dt`cho (t) theo cách hoàn toàn tương tự. 
3. Xây dựng hàm tiền tố KMP cho`dt`. Hàm tiền tố cho chúng ta biết bao nhiêu mẫu vẫn có thể sử dụng được sau khi không khớp, do đó việc tìm kiếm không bao giờ phải bắt đầu lại từ đầu. 
4. Tìm kiếm`dt`bên trong`ds + ds`. Một phép quay của mảng tuần hoàn tương ứng với một đoạn liền kề của phiên bản nhân đôi của nó. Chúng tôi chỉ chấp nhận kết quả khớp bắt đầu từ chỉ số nhỏ hơn (n), vì đó chính xác là (n) phép quay có thể có. 
5. Nếu không có sự trùng khớp như vậy, hãy in`Impossible`. Việc so khớp các khác biệt theo chu kỳ là cần thiết để một phép biến đổi hợp lệ, do đó, không có phép dịch chuyển Caesar nào có thể sửa chữa được một phép quay bị thiếu. 
6. Nếu trận đấu bắt đầu ở (k), hãy tính 

[ 
d=(s[k]-t[0])\bmod 26. 
] 

Chuỗi xoay bắt đầu bằng`s[k]`. Dịch chuyển ký tự đó lùi lại (d) phải tạo ra`t[0]`, do đó phương trình này cho chính xác độ dịch chuyển Caesar cần thiết. 

1. In`Success`, theo sau là (k) và (d). Giá trị được tạo bởi modulo 26 nằm trong khoảng từ 0 đến 25, thỏa mãn phạm vi yêu cầu (-26<d<26). 

### Tại sao nó hoạt động 

Bất biến trung tâm là hai chuỗi chỉ khác nhau bởi một phép dịch chuyển Caesar đều khi và chỉ khi hiệu chu kỳ tương ứng của chúng bằng nhau. Phép dịch Caesar hủy bỏ khi hai ký tự lân cận bị trừ đi, do đó nó không thể ảnh hưởng đến mảng chênh lệch. 

Một phép quay theo (k) chỉ đơn giản là thay đổi điểm bắt đầu của mảng sai phân theo chu kỳ. Tìm kiếm`dt`bên trong`ds + ds`do đó tìm thấy chính xác các phép quay có cấu trúc ký tự tương đối khớp với (t). Khi tìm thấy một phép quay như vậy, mọi sai phân liền kề đều đồng ý, do đó hiệu giữa (các) và (t) được quay là không đổi trong toàn bộ chu kỳ. Hằng số đó chính xác là phép dịch chuyển Caesar được tính từ ký tự đầu tiên. Do đó, mọi cặp được báo cáo ((k,d)) đều tạo ra (t) và nếu tồn tại một cặp hợp lệ thì phép quay của nó phải xuất hiện trong tìm kiếm KMP. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_diff(s):
    n = len(s)
    if n == 1:
        return []
    return [
        (ord(s[(i + 1) % n]) - ord(s[i])) % 26
        for i in range(n)
    ]

def prefix_function(pattern):
    m = len(pattern)
    pi = [0] * m

    for i in range(1, m):
        j = pi[i - 1]

        while j > 0 and pattern[i] != pattern[j]:
            j = pi[j - 1]

        if pattern[i] == pattern[j]:
            j += 1

        pi[i] = j

    return pi

def solve():
    n = int(input())
    t = input().strip()
    s = input().strip()

    if n == 1:
        d = (ord(s[0]) - ord(t[0])) % 26
        print("Success")
        print(0, d)
        return

    ds = build_diff(s)
    dt = build_diff(t)

    pi = prefix_function(dt)

    j = 0
    doubled = ds + ds

    for i, value in enumerate(doubled):
        while j > 0 and value != dt[j]:
            j = pi[j - 1]

        if value == dt[j]:
            j += 1

        if j == n:
            start = i - n + 1

            if start < n:
                d = (ord(s[start]) - ord(t[0])) % 26
                print("Success")
                print(start, d)
                return

            j = pi[j - 1]

    print("Impossible")

if __name__ == "__main__":
    solve()
```các`build_diff`hàm chuyển đổi một chuỗi thành chuỗi sai phân tuần hoàn của nó. biểu thức`(i + 1) % n`xử lý cạnh cuối cùng đến cạnh đầu tiên, điều này là cần thiết vì các phép quay là tuần hoàn chứ không phải là các hoạt động chuỗi con thông thường. 

Trường hợp (n=1) được xử lý riêng vì chuỗi sai phân của nó sẽ trống. Chỉ có một phép quay khả thi, (k=0) và sự dịch chuyển Caesar có thể được lấy trực tiếp từ hai ký tự. 

Hàm tiền tố chỉ được tính cho`dt`. Trong quá trình tìm kiếm KMP,`ds + ds`đại diện cho mọi vòng quay theo chu kỳ của`ds`như một đoạn liền kề bình thường. (n) vị trí bắt đầu đầu tiên có thể tương ứng chính xác với (k=0,\ldots,n-1). 

Khi KMP đạt`j == n`, toàn bộ chuỗi khác biệt mục tiêu đã khớp. biểu thức`i - n + 1`bắt đầu trận đấu đó. Chúng tôi từ chối bắt đầu bằng hoặc xa hơn (n), vì đó là những kết quả trùng lặp được tạo bằng cách nhân đôi mảng. 

Cuối cùng,`d = (ord(s[start]) - ord(t[0])) % 26`theo trực tiếp từ sự chỉ đạo của hoạt động Caesar. Nếu ký tự được xoay là`c`, dịch chuyển nó lùi lại bởi (d) cho`c-d`, vì vậy chúng ta cần`c-d = t[0] (mod 26)`. Sắp xếp lại sẽ đưa ra biểu thức được sử dụng trong mã. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn. Tất cả các chỉ số đều nằm trong (2n) và mọi chuyển đổi ký tự đều có thời gian không đổi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
3
abc
fde
```Sự khác biệt theo chu kỳ như sau. 

| Chuỗi | Trình tự khác biệt | 
| --- | --- | 
|`t = abc`|`[1, 1, 24]`| 
|`s = fde`|`[24, 1, 1]`| 

Nhân đôi trình tự khác biệt của`s`cho`[24, 1, 1, 24, 1, 1]`. Trình tự mục tiêu`[1, 1, 24]`đầu tiên xảy ra ở vị trí (1). 

| Bang KMP | Giá trị | Vị trí mẫu | Kết quả | 
| --- | --- | --- | --- | 
| bắt đầu | 24 | 0 | không khớp | 
| sau chỉ số 1 | 1 | 1 | trận đấu | 
| sau chỉ số 2 | 1 | 2 | trận đấu | 
| sau chỉ số 3 | 24 | 3 | trận đấu đầy đủ | 

Do đó (k=1). Xoay`fde`bởi một vị trí mang lại`def`. Ký tự đầu tiên của nó là`d`, trong khi mục tiêu bắt đầu bằng`a`, vậy 

[ 
d=(d-a)\bmod26=3. 
] 

Dịch chuyển`def`lùi lại 3 cho`abc`, do đó thuật toán in`Success`,`1 3`. 

### Mẫu 2 

Đầu vào là```
3
abc
aba
```Sự khác biệt mang tính chu kỳ là 

| Chuỗi | Trình tự khác biệt | 
| --- | --- | 
|`t = abc`|`[1, 1, 24]`| 
|`s = aba`|`[25, 25, 0]`| 

Trình tự nhân đôi của`s`là`[25, 25, 0, 25, 25, 0]`, không chứa sự xuất hiện của`[1, 1, 24]`. 

| Vị trí tìm kiếm | Sự khác biệt hiện tại | Tiến độ mục tiêu | 
| --- | --- | --- | 
| 0 | 25 | 0 | 
| 1 | 25 | 0 | 
| 2 | 0 | 0 | 
| 3 | 25 | 0 | 
| 4 | 25 | 0 | 
| 5 | 0 | 0 | 

Không có vòng quay nào có các thay đổi ký tự tương đối giống như`t`, do đó không có phép dịch chuyển Caesar nào có thể làm cho các dây bằng nhau. Câu trả lời là`Impossible`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Việc xây dựng cả hai mảng sai phân, xây dựng hàm tiền tố của KMP và tìm kiếm mảng nhân đôi đều mất thời gian tuyến tính. | 
| Không gian | (O(n)) | Các mảng sai phân, chuỗi nhân đôi và hàm tiền tố đều yêu cầu bộ nhớ tuyến tính. | 

Với (n\le200.000), thuật toán chỉ thực hiện một số lần truyền tuyến tính không đổi qua đầu vào. Việc sử dụng bộ nhớ của nó cũng tuyến tính nên nó phù hợp với các ràng buộc đã nêu. 

## Trường hợp thử nghiệm 

Kết quả đầu ra thành công không phải là duy nhất, do đó, một bộ khai thác thử nghiệm mạnh mẽ sẽ xác minh phép biến đổi được trả về thay vì so sánh chuỗi đầu ra hoàn chỉnh theo nghĩa đen. Mã kiểm tra sau đây thực hiện điều đó trong khi vẫn kiểm tra chính xác`Impossible`mẫu.```python
import sys
import io

def solve_data(inp: str) -> str:
    data = inp.strip().split()
    n = int(data[0])
    t = data[1]
    s = data[2]

    def build_diff(x):
        if n == 1:
            return []
        return [
            (ord(x[(i + 1) % n]) - ord(x[i])) % 26
            for i in range(n)
        ]

    if n == 1:
        d = (ord(s[0]) - ord(t[0])) % 26
        return f"Success\n0 {d}\n"

    ds = build_diff(s)
    dt = build_diff(t)

    pi = [0] * n
    for i in range(1, n):
        j = pi[i - 1]
        while j > 0 and dt[i] != dt[j]:
            j = pi[j - 1]
        if dt[i] == dt[j]:
            j += 1
        pi[i] = j

    j = 0
    for i, value in enumerate(ds + ds):
        while j > 0 and value != dt[j]:
            j = pi[j - 1]

        if value == dt[j]:
            j += 1

        if j == n:
            k = i - n + 1
            if k < n:
                d = (ord(s[k]) - ord(t[0])) % 26
                return f"Success\n{k} {d}\n"
            j = pi[j - 1]

    return "Impossible\n"

def run(inp: str) -> str:
    return solve_data(inp)

def valid_output(inp: str, out: str) -> bool:
    data = inp.strip().split()
    n = int(data[0])
    t = data[1]
    s = data[2]

    lines = out.strip().split()

    if lines[0] == "Impossible":
        return len(lines) == 1

    if lines[0] != "Success" or len(lines) != 3:
        return False

    k = int(lines[1])
    d = int(lines[2])

    if not (0 <= k < n and -26 < d < 26):
        return False

    rotated = s[k:] + s[:k]

    transformed = "".join(
        chr((ord(c) - ord('a') - d) % 26 + ord('a'))
        for c in rotated
    )

    return transformed == t

# Provided samples.
assert run("""3
abc
fde
""") == "Success\n1 3\n"

assert run("""3
abc
aba
""") == "Impossible\n"

assert valid_output(
    """1
z
a
""",
    run("""1
z
a
""")
)

# Minimum-size, no transformation needed.
assert valid_output(
    """1
a
a
""",
    run("""1
a
a
""")
)

# All characters equal, with a non-zero Caesar shift.
assert valid_output(
    """4
zzzz
aaaa
""",
    run("""4
zzzz
aaaa
""")
)

# Rotation by n - 1, exercising the cyclic boundary.
assert valid_output(
    """5
abcde
bcdea
""",
    run("""5
abcde
bcdea
""")
)

# Maximum-size input, all characters equal.
n = 200000
max_case = f"{n}\n" + "a" * n + "\n" + "a" * n + "\n"
assert valid_output(max_case, run(max_case))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / a / a`|`Success`, (k=0,d=0) | Kích thước tối thiểu và chuỗi chênh lệch trống | 
|`4 / zzzz / aaaa`|`Success`, bất kỳ phép quay nào và (d=1) | Chuỗi hoàn toàn bằng nhau và số học Caesar mô-đun | 
|`5 / abcde / bcdea`|`Success`, (k=4,d=0) | Xoay quanh cuối | 
| (n=200000), tất cả`a`|`Success`, (k=0,d=0) | Kích thước đầu vào tối đa và hiệu suất tuyến tính | 

## Vỏ cạnh 

Đối với (n=1), các mảng sai phân không chứa phần tử nào, vì vậy KMP không có ý nghĩa. Coi như```
1
z
a
```Chỉ có một khả năng quay duy nhất (k=0). Sự thay đổi cần thiết là 

[ 
(z-a)\bmod26=25. 
] 

Thuật toán trả về`Success`,`0 25`. Điều này tương đương với mẫu`0 -25`bởi vì sự dịch chuyển Caesar là tuần hoàn modulo 26 và cả hai giá trị đều biểu thị cùng một phép biến đổi. 

Đối với bảng chữ cái, hãy xem xét```
1
a
z
```Thuật toán tính toán 

[ 
(a-z)\bmod26=1. 
] 

Dịch chuyển`a`lùi lại 1 tạo ra`z`, Vì thế`Success 0 1`là hợp lệ. Hoạt động modulo ngăn không cho chênh lệch thô âm bị coi là một ca làm việc không hợp lệ. 

Đối với một phép quay đi qua phần cuối của chuỗi, hãy xem xét```
5
abcde
bcdea
```Trình tự sai phân theo chu kỳ của`s`được tìm kiếm trong`ds + ds`. Mục tiêu bắt đầu tại vị trí (4), tương ứng với vòng quay 

# \texttt{a}+\texttt{bcde} 

\texttt{abcde}. 
] 

KMP tìm thấy (k=4) và các ký tự đầu tiên đã đồng ý, vì vậy (d=0). 

Đối với các ký tự lặp lại, hãy xem xét```
4
aaaa
aaaa
```Mọi sai khác theo chu kỳ đều bằng 0, nên mọi phép quay đều khớp. KMP chấp nhận ký tự đầu tiên (k=0) và ký tự đầu tiên cho (d=0). Không cần phải phân biệt giữa nhiều câu trả lời hợp lệ vì bài toán chấp nhận bất kỳ câu trả lời nào trong số đó. 

Trường hợp đúng đắn nhất là khi các chuỗi khác nhau khớp nhau nhưng các chuỗi ban đầu không có cùng ký tự đầu tiên. Ví dụ,```
3
abc
def
```Chuỗi sai phân tuần hoàn của cả hai chuỗi là`[1, 1, 24]`, vì vậy (k=0) là một kết quả khớp cấu trúc hợp lệ. Sự khác biệt của ký tự đầu tiên mang lại 

[ 
d=(d-a)\bmod26=3. 
] 

Dịch chuyển`def`lùi lại 3 tạo ra`abc`. Điều này chứng tỏ tại sao việc so khớp những khác biệt không phải là bước cuối cùng mà nó làm giảm công việc còn lại trong việc xác định một sự dịch chuyển Caesar toàn cầu.
