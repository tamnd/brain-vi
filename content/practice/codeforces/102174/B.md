---
title: "CF 102174B - \u70bc\u91d1\u672f"
description: "Chúng ta có (m) tài liệu hiện có, mỗi tài liệu được biểu thị bằng một chuỗi chữ thường (si). Một vật liệu mới có thể được chấp nhận nếu chuỗi của nó có độ dài chính xác (n), nhưng nó không được xuất hiện dưới dạng chuỗi con liền kề của bất kỳ (si) hiện có nào."
date: "2026-08-19T15:18:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102174
codeforces_index: "B"
codeforces_contest_name: "The 14-th BIT Campus Programming Contest"
rating: 0
weight: 102174
solve_time_s: 138
verified: true
draft: false
---

[CF 102174B - \u70bc\u91d1\u672f](https://codeforces.com/problemset/problem/102174/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có (m) tài liệu hiện có, mỗi tài liệu được biểu thị bằng một chuỗi chữ thường (s_i). Một vật liệu mới có thể được chấp nhận nếu chuỗi của nó có độ dài chính xác (n), nhưng nó không được xuất hiện dưới dạng chuỗi con liền kề của bất kỳ (s_i) hiện có nào. Chúng ta chỉ cần xuất ra một chuỗi như vậy và câu lệnh đảm bảo rằng chuỗi đó tồn tại. 

Cách hữu ích để xem xét vấn đề là quên thuật ngữ hóa học và đặt một câu hỏi thuần túy về chuỗi: trong số tất cả (26^n) chuỗi chữ thường có độ dài (n), hãy tìm một chuỗi không có trong tập hợp tất cả các chuỗi con có độ dài-(n) của chuỗi đầu vào. 

hãy để 

[ 
S=\sum_i |s_i|. 
] 

Chúng ta có (S\le 3\times 10^5), trong khi (n) có thể lớn bằng (10^5). Điều này ngay lập tức loại trừ các thuật toán tùy thuộc vào (26^n), bởi vì thậm chí (26^{10}) đã vượt xa mọi thứ chúng ta có thể liệt kê. Nó cũng loại trừ việc kiểm tra trực tiếp mọi ứng cử viên đối với mọi chuỗi đầu vào. Bản thân tổng số đầu vào chỉ có vài trăm nghìn ký tự, vì vậy giải pháp dự định sẽ xử lý nó một cách gần như tuyến tính. 

Có một quan sát đếm đặc biệt hữu ích. Một chuỗi (s_i) chứa tối đa (|s_i|-n+1) vị trí bắt đầu khác nhau cho các chuỗi con có độ dài-(n) và do đó tổng số chuỗi bị cấm riêng biệt là nhiều nhất 

[ 
\sum_i \max(0, |s_i|-n+1)\le S. 
] 

Vì vậy, mặc dù có (26^n) câu trả lời khả thi, nhưng nhiều nhất (3\times10^5) trong số đó thực sự có thể bị cấm. Chúng ta chỉ cần một cách để liệt kê một số lượng nhỏ ứng viên và kiểm tra tư cách thành viên một cách nhanh chóng. 

Trường hợp một cạnh là khi mọi chuỗi hiện có đều ngắn hơn (n). Ví dụ,```
3 2
a
bc
```Không có chuỗi con có độ dài 3 ở bất cứ đâu, vì vậy`aaa`đã là một câu trả lời hợp lệ. Việc triển khai bất cẩn giả định rằng mỗi chuỗi đầu vào đóng góp ít nhất một cửa sổ có thể tạo ra giới hạn vòng lặp không chính xác hoặc một tập ứng cử viên trống. 

Một trường hợp cạnh khác xảy ra khi ứng viên đầu tiên có mặt nhưng ứng viên tiếp theo thì không. Ví dụ,```
3 1
aaaa
```Chuỗi`aaa`xảy ra nhưng`aab`không. Đầu ra chính xác có thể là`aab`. Việc triển khai chỉ kiểm tra các ứng cử viên có ký tự lặp lại, chẳng hạn như`aaa`,`bbb`, v.v. sẽ bỏ lỡ các câu trả lời hợp lệ. 

Trường hợp thứ ba là (n=1). Ví dụ,```
1 25
a
b
c
d
e
f
g
h
i
j
k
l
m
n
o
p
q
r
s
t
u
v
w
x
y
```Mỗi chuỗi một chữ cái ngoại trừ`z`bị cấm nên`z`là câu trả lời. Mã tạo ứng cử viên phải hoạt động khi chuỗi chỉ bao gồm một vị trí. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp rất đơn giản về mặt khái niệm. Liệt kê từng chuỗi trong số (26^n) chữ thường có độ dài (n) và đối với mỗi ứng cử viên, hãy kiểm tra xem nó có xuất hiện trong bất kỳ chuỗi đầu vào nào không. Nếu chúng ta sử dụng kết hợp chuỗi con đơn giản, việc kiểm tra một ứng cử viên có thể mất (O(Sn)) thời gian trong trường hợp xấu nhất, đưa ra 

[ 
O(26^nSn). 
] 

Ngay cả khi thành viên chuỗi con được giảm xuống còn (O(1)) bằng cách sử dụng cấu trúc tra cứu hoàn hảo, thì việc chỉ liệt kê (26^n) ứng viên cho (n) lớn là không thể. Lực lượng vũ phu là chính xác vì nó kiểm tra không gian câu trả lời hoàn chỉnh, nhưng không gian câu trả lời lớn hơn theo cấp số nhân so với đầu vào. 

Quan sát quan trọng là đầu vào chỉ có thể cấm một số chuỗi có độ dài-(n) tuyến tính. Trên tất cả các chuỗi đầu vào có tối đa (S) độ dài-(n) cửa sổ. Do đó, sau khi xem xét nhiều nhất (S+1) ứng viên khác biệt, ít nhất một ứng viên phải vắng mặt. 

Điều này thay đổi vấn đề hoàn toàn. Chúng ta không cần phải hiểu tất cả (26^n) chuỗi. Chúng ta có thể liệt kê các ứng cử viên theo thứ tự từ điển và dừng lại ngay khi chúng ta tìm thấy một dấu vân tay có dấu vân tay không thuộc tập dấu vân tay của chuỗi con bị cấm. 

Hàm băm đa thức cuộn cho phép chúng ta tính toán dấu vân tay của mọi cửa sổ có độ dài-(n) trong thời gian phân bổ (O(1)) trên mỗi cửa sổ. Chúng tôi lưu trữ tất cả những dấu vân tay đó trong một bộ băm. Sau đó chúng tôi liệt kê các chuỗi ứng cử viên bắt đầu bằng`aaa...a`, theo sau là`aaa...b`, vân vân. Ứng cử viên có thể được duy trì dưới dạng bộ đếm cơ sở 26 và hàm băm của nó có thể được cập nhật theo các vị trí thay đổi. Trên (K) ứng viên liên tiếp, tổng số vị trí cuối cùng được thay đổi là (O(K)), do đó việc tạo ứng viên cũng tuyến tính theo số lượng ứng viên. 

Băm giới thiệu khả năng va chạm thông thường. Chúng tôi sử dụng mô đun nguyên tố lớn (2^{61}-1) và chọn cơ sở một cách ngẫu nhiên. Một xung đột có thể làm cho một ứng cử viên bị cấm trông cũng bị cấm, vì vậy nó chỉ có thể trì hoãn việc tìm kiếm chứ không khiến chúng ta xuất ra một chuỗi thực sự có mặt. Với hàm băm đa thức 61 bit ngẫu nhiên, xác suất xảy ra đủ va chạm để ảnh hưởng đến độ phức tạp dự định là không đáng kể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(26^nSn)) | (O(n)) | Quá chậm | 
| Rolling Hash + liệt kê ứng cử viên | Dự kiến ​​(O(S+n)) | (O(S+n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (n) và tất cả các chuỗi hiện có và chọn cơ sở hàm băm đa thức ngẫu nhiên. Chúng tôi sử dụng mô đun nguyên tố (P=2^{61}-1), mang lại không gian băm rất lớn trong khi vẫn quản lý được số học số nguyên Python. 
2. Tính toán trước (B^n\bmod P). Đây là hệ số cần thiết để loại bỏ ký tự cũ nhất khi cửa sổ cuộn dài (n) di chuyển sang phải một vị trí. 
3. Quét mọi chuỗi đầu vào bằng cửa sổ cuộn có độ dài (n). Nếu chuỗi ngắn hơn (n), nó không đóng góp ứng cử viên bị cấm nào. Ngược lại, hãy tính hàm băm của mọi cửa sổ có độ dài-(n) và chèn nó vào một tập hợp. 
4. Khởi tạo ứng viên thành`a`lặp lại (n) lần. Hàm băm đa thức của nó được tính từ thực tế là mọi ký tự đều có giá trị (1). Đây là chuỗi có độ dài-(n) nhỏ nhất có thể về mặt từ điển. 
5. Kiểm tra xem hàm băm ứng viên có nằm trong tập hàm băm bị cấm hay không. Nếu vắng mặt, hãy xuất ứng viên ngay lập tức. Một ứng cử viên xuất hiện trong chuỗi đầu vào phải có hàm băm giống hệt như một trong các cửa sổ được lưu trữ, do đó, hàm băm bị thiếu chứng nhận rằng ứng cử viên đó vắng mặt. 
6. Nếu ứng viên bị cấm, hãy tăng nó thành số cơ sở 26. Bắt đầu từ vị trí cuối cùng, mỗi dấu vết`z`trở thành`a`, và cái đầu tiên không phải`z`ký tự được tăng thêm một. Đối với mỗi vị trí đã thay đổi, hãy cập nhật hàm băm ứng viên bằng cách thêm thay đổi nhân với lũy thừa thích hợp của cơ sở. 
7. Lặp lại việc kiểm tra tư cách thành viên và tăng dần cho đến khi tìm thấy hàm băm bị thiếu. Không thể có nhiều hơn (S) chuỗi bị cấm riêng biệt, vì vậy sau tối đa (S+1) ứng cử viên riêng biệt, một câu trả lời hợp lệ phải xuất hiện, bỏ qua xác suất va chạm băm không đáng kể. 

Tại sao nó hoạt động: tập hợp hàm băm bị cấm chứa dấu vân tay của mọi chuỗi con có độ dài-(n) thực tế của mọi tài liệu hiện có. Bảng liệt kê ứng cử viên truy cập các chuỗi có độ dài-(n) riêng biệt theo thứ tự từ điển. Bất cứ khi nào hàm băm của nó vắng mặt trong tập hợp, ứng cử viên không thể bằng bất kỳ chuỗi con có độ dài-(n) hiện có nào, vì vậy nó là một vật liệu mới hợp lệ. Vì đầu vào có thể chứa tổng cộng tối đa (S) độ dài-(n) cửa sổ, nên nó không thể cấm nhiều hơn (S) ứng cử viên riêng biệt. Do đó, một trong những ứng cử viên đầu tiên (S+1) phải hợp lệ. 

## Giải pháp Python```python
import sys
import random

input = sys.stdin.readline

MOD = (1 << 61) - 1

def solve():
    n, m = map(int, input().split())

    # Randomized base makes adversarial hash collisions extremely unlikely.
    base = random.randrange(256, MOD - 1)

    pow_n = pow(base, n, MOD)

    forbidden = set()

    for _ in range(m):
        s = input().strip()
        if len(s) < n:
            continue

        h = 0

        # Hash of the first window.
        for i in range(n):
            h = (h * base + (ord(s[i]) - 96)) % MOD
        forbidden.add(h)

        # Roll the window.
        for i in range(n, len(s)):
            old = ord(s[i - n]) - 96
            new = ord(s[i]) - 96
            h = (h * base + new - old * pow_n) % MOD
            forbidden.add(h)

    # Candidate is represented by values 0..25.
    # Its polynomial character values are values + 1.
    candidate = bytearray(b'a' * n)

    # Hash of a^n.
    h = (pow(base, n, MOD) - 1) * pow(base - 1, MOD - 2, MOD) % MOD

    # The formula above is the geometric sum:
    # 1 * base^(n-1) + ... + 1.
    #
    # Handle base == 1, which is astronomically unlikely but easy to avoid.
    if base == 1:
        h = n % MOD

    powers = [1] * n
    for i in range(1, n):
        powers[i] = powers[i - 1] * base % MOD

    while True:
        if h not in forbidden:
            sys.stdout.write(candidate.decode())
            return

        # Increment the base-26 number.
        pos = n - 1

        while pos >= 0 and candidate[pos] == ord('z'):
            # z -> a changes the character value from 26 to 1.
            h = (h - 25 * powers[n - 1 - pos]) % MOD
            candidate[pos] = ord('a')
            pos -= 1

        if pos < 0:
            # The statement guarantees that an answer exists.
            return

        # Increase the first non-z character by one.
        h = (h + powers[n - 1 - pos]) % MOD
        candidate[pos] += 1

if __name__ == "__main__":
    solve()
```Các chuỗi đầu vào được xử lý lần lượt, do đó không cần phải giữ tất cả chúng trong bộ nhớ. Đối với một chuỗi có độ dài ít nhất (n), cửa sổ đầu tiên được băm trực tiếp. Mỗi cửa sổ tiếp theo được lấy từ hàm băm trước đó bằng cách nhân với cơ số, thêm ký tự mới và trừ ký tự cũ nhân với (B^n). 

Ứng viên được lưu trữ dưới dạng`bytearray`, giúp tránh việc liên tục xây dựng chuỗi Python trong khi tìm kiếm. Các ký tự của nó được coi là các giá trị từ (1) đến (26), trong khi bản thân các giá trị byte là mã ASCII từ`a`ĐẾN`z`. 

Hàm băm ứng cử viên có thể được cập nhật cục bộ khi bộ đếm cơ sở 26 được tăng lên. Nếu một vị trí thay đổi từ`a`ĐẾN`b`, đóng góp băm của nó tăng thêm (B^k). Nếu nó thay đổi từ`z`ĐẾN`a`, phần đóng góp của nó giảm đi (25B^k). Số mũ (k=n-1-\text{pos}) chính xác là số vị trí bên phải của nó. 

Mảng quyền hạn được lập chỉ mục theo số mũ này. Vì ứng viên hoàn toàn bắt đầu`a`, hàm băm ban đầu của nó là tổng hình học (1+B+\cdots+B^{n-1}). Điều đặc biệt`base == 1`nhánh tránh chia cho 0 trong công thức tổng hình học, mặc dù lựa chọn ngẫu nhiên làm cho sự kiện đó thực sự không thể thực hiện được. 

Không có vấn đề tràn số nguyên trong Python vì số nguyên có độ chính xác tùy ý. Mô-đun này được áp dụng sau mỗi lần cập nhật số học, giữ cho hàm băm được lưu trữ giới hạn bởi (2^{61}). 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, đầu vào là```
3 1
a
```Chuỗi hiện có duy nhất có độ dài nhỏ hơn (n=3), do đó, nó không đóng góp chuỗi con có độ dài 3 bị cấm. 

| Ứng viên | Băm trong bộ bị cấm? | Hành động | 
| --- | --- | --- | 
|`aaa`| Không | đầu ra`aaa`| 

Đầu ra mẫu sử dụng`zzz`, nhưng mọi chuỗi hợp lệ đều được chấp nhận. Đây`aaa`hợp lệ vì tài liệu hiện có duy nhất là`a`, không có chuỗi con có độ dài bằng ba. 

Đối với mẫu thứ hai, đầu vào là```
3 2
ac
ak
```Một lần nữa, cả hai chuỗi hiện tại đều có độ dài bằng 2, do đó không chứa chuỗi con có độ dài 3. 

| Ứng viên | Băm trong bộ bị cấm? | Hành động | 
| --- | --- | --- | 
|`aaa`| Không | đầu ra`aaa`| 

Đầu ra được cung cấp`fun`là một câu trả lời hợp lệ khác. Dấu vết này cũng thực hiện ranh giới trong đó chuỗi đầu vào ngắn hơn chính xác một ký tự so với độ dài câu trả lời được yêu cầu. 

Đối với trường hợp ứng cử viên đầu tiên thực sự bị cấm, hãy xem xét```
3 1
aaaa
```Cả hai cửa sổ dài 3 đều`aaa`, do đó tập bị cấm chỉ chứa hàm băm của`aaa`. 

| Ứng viên | Băm trong bộ bị cấm? | Hành động | 
| --- | --- | --- | 
|`aaa`| Có | Ứng viên tăng trưởng | 
|`aab`| Không | đầu ra`aab`| 

Sự xuất hiện trùng lặp của`aaa`chỉ được lưu trữ một lần vì cấu trúc bị cấm là một tập hợp. Ứng viên`aab`vắng mặt nên việc tìm kiếm dừng lại ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Dự kiến ​​(O(S+n)) | Mỗi ký tự đầu vào tham gia vào một lần cập nhật băm luân phiên và tối đa (S+1) ứng cử viên được kiểm tra. Việc tăng ứng viên mất (O(1)) thời gian khấu hao. | 
| Không gian | (O(S+n)) | Tập hợp chứa tối đa (S) giá trị băm, trong khi mảng ứng cử viên và mảng lũy ​​thừa sử dụng bộ nhớ (O(n)). | 

Ở đây (S\le3\times10^5), do đó thuật toán chỉ xử lý vài trăm nghìn ký tự và duy trì bộ băm có kích thước tương tự. Điều này tương thích với giới hạn bộ nhớ 256 MB đã nêu và nó tránh được mọi hệ số mũ trong (n). 

## Trường hợp thử nghiệm```python
import io
import random

MOD = (1 << 61) - 1

def solve_data(inp: str) -> str:
    data = inp.split()
    it = iter(data)

    n = int(next(it))
    m = int(next(it))

    base = random.randrange(256, MOD - 1)
    pow_n = pow(base, n, MOD)

    forbidden = set()

    for _ in range(m):
        s = next(it)

        if len(s) < n:
            continue

        h = 0
        for i in range(n):
            h = (h * base + ord(s[i]) - 96) % MOD
        forbidden.add(h)

        for i in range(n, len(s)):
            old = ord(s[i - n]) - 96
            new = ord(s[i]) - 96
            h = (h * base + new - old * pow_n) % MOD
            forbidden.add(h)

    candidate = bytearray(b'a' * n)

    powers = [1] * n
    for i in range(1, n):
        powers[i] = powers[i - 1] * base % MOD

    if base == 1:
        h = n % MOD
    else:
        h = (pow(base, n, MOD) - 1) * pow(base - 1, MOD - 2, MOD) % MOD

    while True:
        if h not in forbidden:
            return candidate.decode()

        pos = n - 1

        while pos >= 0 and candidate[pos] == ord('z'):
            h = (h - 25 * powers[n - 1 - pos]) % MOD
            candidate[pos] = ord('a')
            pos -= 1

        assert pos >= 0, "The original problem guarantees an answer."

        h = (h + powers[n - 1 - pos]) % MOD
        candidate[pos] += 1

# Provided sample 1.
assert solve_data("""\
3 1
a
""") == "aaa", "sample 1"

# Provided sample 2.
assert solve_data("""\
3 2
ac
ak
""") == "aaa", "sample 2"

# Minimum n. Every character except z is forbidden.
case = "1 25\n" + "\n".join(chr(ord('a') + i) for i in range(25)) + "\n"
assert solve_data(case) == "z", "minimum n"

# Off-by-one case: aaa occurs in aaaa, but aab does not.
assert solve_data("""\
3 1
aaaa
""") == "aab", "window boundary"

# Multiple strings cover every length-2 string beginning with a.
case = "2 26\n" + "\n".join("a" + chr(ord('a') + i) for i in range(26)) + "\n"
assert solve_data(case) == "ba", "candidate increment"

# Maximum-size input. The only forbidden length-100000 string is a^100000.
# The next lexicographic candidate is a^(99999)b.
n = 100000
s = "a" * 300000
case = f"{n} 1\n{s}\n"
assert solve_data(case) == "a" * (n - 1) + "b", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 25`theo sau là`a`bởi vì`y`|`z`| Mức độ bao phủ tối thiểu (n) và đầy đủ của 25 ký hiệu bảng chữ cái | 
|`3 1 / aaaa`|`aab`| Ứng cử viên đầu tiên bị cấm và ranh giới cửa sổ có độ dài-(n) chính xác | 
|`2 26`theo sau là`aa`bởi vì`az`|`ba`| Sự gia tăng và chuyển đổi ứng cử viên Base-26 từ`az`ĐẾN`ba`| 
|`100000 1 / a`lặp đi lặp lại 300000 lần |`a...ab`| Tối đa (n), tổng kích thước đầu vào tối đa và xử lý ứng viên dài | 

## Vỏ cạnh 

Khi mọi chuỗi đầu vào đều ngắn hơn (n), tập hợp bị cấm vẫn trống. Vì```
3 2
a
bc
```không có cửa sổ dài-3 nào cả. Ứng viên bắt đầu như`aaa`, hàm băm của nó không có và thuật toán xuất ra`aaa`ngay lập tức. Ranh giới quan trọng là`len(s) < n`kiểm tra, điều này ngăn cản việc cố gắng xây dựng một cửa sổ không tồn tại. 

Khi có ứng cử viên đầu tiên, thuật toán không cho rằng một chuỗi ký tự lặp lại khác sẽ hoạt động. Vì```
3 1
aaaa
```chuỗi con có độ dài 3 riêng biệt duy nhất là`aaa`. Ứng viên đầu tiên bị từ chối, bộ đếm thay đổi vị trí cuối cùng của nó từ`a`ĐẾN`b`, sản xuất`aab`, và ứng cử viên thứ hai được chấp nhận. 

Khi (n=1), mảng lũy ​​thừa có chính xác một phần tử và ứng viên có chính xác một byte. Vì```
1 25
a
b
...
y
```thí sinh di chuyển qua bảng chữ cái từng ký tự một cho đến khi đạt được`z`. Không có trường hợp đặc biệt nào trong logic liệt kê cho chuỗi một ký tự. 

Khi mức tăng ứng viên vượt qua một loạt`z`ký tự, mỗi dấu`z`phải trở thành`a`trước khi ký tự đứng trước được tăng lên. Ví dụ, sau`azz`, ứng cử viên tiếp theo là`baa`, không`bzz`hoặc`aza`. Cập nhật băm trừ (25B^k) cho mỗi vị trí đặt lại, sau đó cộng phần đóng góp của vị trí tăng lên. Đây là phần có nhiều khả năng xảy ra lỗi lẻ tẻ nhất, vì vậy`az`ĐẾN`ba`test thực hiện nó một cách rõ ràng.
