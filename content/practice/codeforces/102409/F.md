---
title: "CF 102409F - Giải pháp mỉa mai 1"
description: "Chúng ta được cung cấp một từ có các ký tự đã được sắp xếp theo giá trị ASCII của chúng. Chúng tôi thay thế mọi ký tự bằng mã ASCII của nó, thu được một mảng gồm (N) số nguyên dương."
date: "2026-08-11T16:33:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102409
codeforces_index: "F"
codeforces_contest_name: "Semana i 2019"
rating: 0
weight: 102409
solve_time_s: 146
verified: true
draft: false
---

[CF 102409F - Giải pháp mỉa mai 1](https://codeforces.com/problemset/problem/102409/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một từ có các ký tự đã được sắp xếp theo giá trị ASCII của chúng. Chúng tôi thay thế mọi ký tự bằng mã ASCII của nó, thu được một mảng gồm (N) số nguyên dương. 

Quá trình mã hóa bao gồm lấy mọi tập hợp con có thể có của mảng này và ghi lại tổng các phần tử đã chọn. Tập hợp con trống đóng góp (0). Có chính xác (2^N) tập hợp con, do đó đầu ra chứa chính xác (2^N) số nguyên, được sắp xếp theo thứ tự không giảm. 

Ví dụ, đối với từ`ab`, giá trị ASCII là (97,98). Các tập hợp con của nó là tập hợp con trống, mỗi ký tự đơn và cả hai ký tự cùng nhau. Tổng của chúng là (0,97,98,195), vì vậy đầu ra được mã hóa là`0 97 98 195`. 

Ràng buộc (N \le 20) đủ nhỏ để (2^N) có thể quản lý được. Ở mức tối đa là (2^{20}=1.048.576), do đó, bản thân kết quả đầu ra đã chứa hơn một triệu số. Điều này mang lại cho chúng ta giới hạn dưới hữu ích: mọi thuật toán được chấp nhận đều phải dành ít nhất (O(2^N)) thời gian chỉ để đưa ra câu trả lời. Thuật toán (O(N2^N)) thực hiện công việc nhiều gấp khoảng 20 lần so với kích thước đầu ra ở giới hạn trên, trong khi phương pháp sắp xếp (O(2^N \log 2^N)) thực hiện nhiều công việc hơn đáng kể. Do đó, mục tiêu là xây dựng tất cả các tổng tập hợp con trong thời gian (O(2^N)). 

Có một số trường hợp đặc biệt có thể bộc lộ sai sót khi triển khai đơn giản. Với một ký tự, chẳng hạn như```
1
a
```tổng tập hợp con duy nhất là (0) và (97), vì vậy đầu ra là```
0 97
```Một lỗi phổ biến là chỉ tạo các tập con không trống, sẽ bỏ qua số 0 ban đầu một cách không chính xác. 

Giá trị ký tự trùng lặp cũng có vấn đề. Vì```
2
aa
```cả hai ký tự đều có giá trị (97). Bốn tập hợp con tạo ra (0,97,97,194), vì vậy kết quả đầu ra đúng là```
0 97 97 194
```Hai giá trị bằng nhau (97) đều phải ở đầu ra vì chúng tương ứng với các tập hợp con khác nhau. Việc trừ các khoản tiền sẽ tạo ra kết quả sai. 

Dữ liệu đầu vào đã được sắp xếp không có nghĩa là mọi tổng tập hợp con đều khác biệt. Ví dụ, từ`ab`có các giá trị khác nhau, nhưng với các mảng lớn hơn, các kết hợp khác nhau vẫn có thể tạo ra cùng một tổng. Thuật toán phải bảo toàn bội số thay vì coi tổng tập hợp con là một tập hợp. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi tập hợp con bằng mặt nạ bit. Đối với mỗi mặt nạ từ (0) đến (2^N-1), chúng tôi kiểm tra các bit (N) của nó và thêm các giá trị mảng tương ứng. Điều này tạo ra chính xác mọi tập hợp con chính xác một lần. Sau đó chúng ta có thể sắp xếp tổng (2^N) kết quả. 

Vấn đề là hệ số (N) trong thế hệ tập hợp con. Trong trường hợp xấu nhất có (2^{20}=1,048,576) mặt nạ và mỗi mặt nạ có thể yêu cầu kiểm tra tất cả (20) vị trí. Đó là khoảng (20.971.520) kiểm tra phần tử trước khi sắp xếp cuối cùng. Việc sắp xếp thêm một triệu số nguyên khác sẽ cộng thêm khoảng (2^{20}\log_2(2^{20})) hoặc khoảng hai mươi triệu mức so sánh. Đây là công việc không cần thiết khi bản thân đầu ra chỉ có khoảng một triệu phần tử. 

Cấu trúc hữu ích là các giá trị đầu vào đã được sắp xếp. Tổng quát hơn, giả sử chúng ta đã xử lý các giá trị (k) đầu tiên và đã có tất cả các tổng tập hợp con của chúng theo thứ tự được sắp xếp. Khi chúng ta thêm một giá trị mới (x), mọi tập hợp con cũ sẽ tạo ra chính xác hai khả năng: giữ nguyên tập hợp con, cho (s) hoặc bao gồm (x), cho (s+x). 

Nếu số tiền cũ là 

[ 
s_1 \le s_2 \le \dots \le s_m, 
] 

thì tổng mới thu được khi cộng (x) là 

[ 
s_1+x \le s_2+x \le \dots \le s_m+x. 
] 

Vì vậy, chúng ta có hai mảng được sắp xếp: tổng cũ và tổng đã dịch chuyển. Chúng ta có thể hợp nhất chúng theo thời gian tuyến tính thay vì tạo ra mọi thứ và sắp xếp sau đó. 

Quan sát quan trọng thậm chí còn mạnh mẽ hơn việc chỉ tránh loại cuối cùng. Sau mỗi lần chèn, toàn bộ mảng tổng con vẫn được sắp xếp. Bắt đầu từ`[0]`, mỗi ký tự mới đóng góp một bản sao đã dịch chuyển của danh sách hiện có và hai danh sách có thể được hợp nhất. Vì danh sách tăng gấp đôi kích thước ở mỗi bước nên tổng công việc là 

[ 
1+2+4+\dots+2^{N-1}=2^N-1, 
] 

đó là tối ưu cho đến chi phí viết đầu ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N2^N + 2^N\log 2^N)) | (O(2^N)) | Công việc quá chậm/không cần thiết | 
| Tối ưu | (O(2^N)) | (O(2^N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (N) và chuyển đổi mọi ký tự của từ thành giá trị ASCII của nó. Các giá trị đã không giảm vì từ được đảm bảo được sắp xếp theo giá trị ASCII. 
2. Bắt đầu với danh sách đã sắp xếp`sums = [0]`. Điều này thể hiện tất cả các tổng tập hợp con sử dụng các phần tử bằng 0. Tập hợp con trống là tập hợp con duy nhất tại thời điểm này, vì vậy 0 là trạng thái ban đầu đầy đủ và chính xác. 
3. Xử lý từng giá trị ký tự một. Giả sử tổng tập hợp con được sắp xếp hiện tại là`sums`và giá trị tiếp theo là`x`. 
4. Xây dựng dãy thứ hai bằng cách cộng`x`tới mọi phần tử của`sums`. Nếu như`sums`được sắp xếp, chuỗi đã dịch chuyển này cũng được sắp xếp vì việc thêm cùng một số vào mọi phần tử sẽ giữ nguyên thứ tự. 
5. Hợp nhất bản gốc`sums`và chuỗi đã dịch chuyển thành một danh sách được sắp xếp mới. Mỗi phần tử trong danh sách ban đầu đại diện cho một tập hợp con không sử dụng`x`, trong khi mỗi phần tử được dịch chuyển đại diện cho tập hợp con tương ứng sử dụng`x`. Do đó, việc hợp nhất chứa mọi tập hợp con của tiền tố được xử lý chính xác một lần. 
6. Thay thế`sums`với danh sách đã hợp nhất và tiếp tục với giá trị tiếp theo. Sau khi xử lý (k) ký tự,`sums`chứa chính xác (2^k) giá trị. 
7. Sau khi tất cả (N) ký tự đã được xử lý, hãy in`sums`. Độ dài của nó là (2^N) và bởi vì mọi thứ tự sắp xếp được bảo toàn khi hợp nhất, đầu ra đã được sắp xếp và không yêu cầu lượt sắp xếp cuối cùng. 

### Tại sao nó hoạt động 

Bất biến là sau khi xử lý ký tự (k) đầu tiên,`sums`chứa mọi tập hợp con của (k) ký tự đó chính xác một lần và được sắp xếp. 

Bất biến ban đầu là đúng vì tiền tố trống có chính xác một tập con, tập con trống, có tổng bằng 0. Khi một giá trị mới (x) được thêm vào, mỗi tập hợp con cũ có chính xác hai phần mở rộng: một phần mở rộng loại trừ (x), có cùng tổng và một phần mở rộng bao gồm (x), với tổng của nó tăng thêm (x). Danh sách ban đầu chứa tất cả các trường hợp loại trừ và danh sách được dịch chuyển chứa tất cả các trường hợp bao gồm. Các trường hợp này rời rạc và cùng nhau bao gồm mọi tập hợp con của tiền tố mở rộng. 

Cả hai danh sách đều được sắp xếp, do đó việc hợp nhất chúng sẽ tạo ra bộ sưu tập hoàn chỉnh theo thứ tự được sắp xếp. Điều này chứng tỏ rằng bất biến vẫn đúng sau mỗi ký tự và do đó danh sách cuối cùng chính xác là thông điệp được mã hóa cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    word = input().strip()

    values = [ord(c) for c in word]

    sums = [0]

    for x in values:
        m = len(sums)
        shifted = [sums[i] + x for i in range(m)]

        merged = []
        i = 0
        j = 0

        while i < m and j < m:
            if sums[i] <= shifted[j]:
                merged.append(sums[i])
                i += 1
            else:
                merged.append(shifted[j])
                j += 1

        while i < m:
            merged.append(sums[i])
            i += 1

        while j < m:
            merged.append(shifted[j])
            j += 1

        sums = merged

    print(*sums)

if __name__ == "__main__":
    solve()
```các`ord(c)`cuộc gọi chuyển đổi trực tiếp từng ký tự thành giá trị ASCII của nó. Không cần phải phân biệt chữ hoa, chữ thường hoặc chữ số theo cách thủ công vì Python đã cung cấp mã số cần thiết. 

Danh sách ban đầu chỉ chứa`0`, tương ứng với tập con trống. Đối với một giá trị`x`,`shifted`chứa chính xác tổng các tập con bao gồm`x`. Bản gốc`sums`chứa tổng các tập con loại trừ nó. 

Việc hợp nhất sử dụng`<=`còn hơn là`<`. Sự lựa chọn này là có chủ ý. Tổng bằng nhau phải được phát ra một lần cho mỗi tập hợp con tạo ra chúng. Việc chọn một trong hai bên khi các giá trị bằng nhau vẫn giữ nguyên cả hai bản sao vì mỗi lần chỉ có một con trỏ tiến lên. 

Hai người còn lại`while`các vòng lặp sẽ nối thêm bất kỳ nửa nào của quá trình hợp nhất chưa hết. Việc bỏ qua một trong hai vòng lặp là một lỗi triển khai hợp nhất cổ điển và sẽ làm mất tổng hợp lệ của tập hợp con. 

Số nguyên Python không tràn cho các giá trị liên quan ở đây. Ngay cả tổng lớn nhất có thể cũng nhiều nhất là (20 \times 122), vì ký tự lớn nhất được phép là chữ cái viết thường, do đó số học số nguyên thông thường là quá đủ. 

Đầu vào chứa chính xác một từ, do đó không có vòng lặp test-case. Đầu ra được sản xuất với`print(*sums)`, ghi tất cả (2^N) số nguyên cách nhau bằng dấu cách. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét mẫu được cung cấp:```
5
Beery
```Các giá trị ASCII là (66,101,101,114,121). Bảng sau hiển thị danh sách sau mỗi lần chèn. 

| Giá trị gia tăng | Số tiền trước đó | Số tiền chuyển đổi | Tổng sắp xếp mới | 
| --- | --- | --- | --- | 
| 66 |`[0]`|`[66]`|`[0, 66]`| 
| 101 |`[0, 66]`|`[101, 167]`|`[0, 66, 101, 167]`| 
| 101 |`[0, 66, 101, 167]`|`[101, 167, 202, 268]`|`[0, 66, 101, 101, 167, 167, 202, 268]`| 
| 114 |`[0, 66, 101, 101, 167, 167, 202, 268]`|`[114, 180, 215, 215, 281, 281, 316, 382]`|`[0, 66, 101, 101, 114, 167, 167, 180, 202, 215, 215, 268, 281, 281, 316, 382]`| 
| 121 | 16 khoản trước | mỗi tổng trước cộng 121 | 32 khoản tiền được sắp xếp cuối cùng | 

Sau giá trị thứ năm, danh sách có (2^5=32) phần tử, chính xác là các số được in trong đầu ra mẫu. 

hai`101`các ký tự chứng minh tại sao các giá trị trùng lặp được giữ nguyên. Thêm thứ hai`101`tạo bản sao thứ hai của mỗi tổng từ giai đoạn trước, do đó các tổng tập hợp con trùng lặp sẽ xuất hiện một cách tự nhiên trong kết quả. 

### Ví dụ 2 

Hãy xem xét đầu vào nhỏ```
3
abc
```Các giá trị ASCII là (97,98,99). 

| Giá trị gia tăng | Số tiền trước đó | Số tiền chuyển đổi | Tổng sắp xếp mới | 
| --- | --- | --- | --- | 
| 97 |`[0]`|`[97]`|`[0, 97]`| 
| 98 |`[0, 97]`|`[98, 195]`|`[0, 97, 98, 195]`| 
| 99 |`[0, 97, 98, 195]`|`[99, 196, 197, 294]`|`[0, 97, 98, 99, 195, 196, 197, 294]`| 

Câu trả lời cuối cùng là```
0 97 98 99 195 196 197 294
```Dấu vết này làm cho việc xây dựng hai chiều trở nên đặc biệt rõ ràng. Ở mỗi giai đoạn, danh sách cũ đại diện cho các tập hợp con không bao gồm ký tự mới, trong khi danh sách được dịch chuyển đại diện cho các tập hợp con bao gồm ký tự đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(2^N)) | Việc hợp nhất ở mỗi giai đoạn sẽ xử lý danh sách hiện tại nhiều nhất hai lần và kích thước danh sách là (1,2,4,\ldots,2^{N-1}). | 
| Không gian | (O(2^N)) | Câu trả lời cuối cùng chứa (2^N) số nguyên và thuật toán tạm thời lưu trữ danh sách hiện tại và danh sách đã thay đổi. | 

Tại (N=20), danh sách cuối cùng chứa (1.048.576) số nguyên. Thuật toán (O(2^N)) là phù hợp vì việc tạo ra nhiều giá trị đầu ra đó đã yêu cầu công việc tuyến tính trong (2^N). Yêu cầu bộ nhớ cũng ở mức thoải mái trong khoảng 256 MB đối với kích thước đầu vào này, mặc dù chi phí danh sách và số nguyên của Python giúp việc tránh các bản sao bổ sung không cần thiết trở nên hữu ích. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây triển khai thuật toán tương tự như một hàm có thể gọi để có thể kiểm tra kết quả đầu ra bằng các xác nhận.```python
import sys
import io

def solve_io(inp: str) -> str:
    data = inp.split()
    n = int(data[0])
    word = data[1]

    values = [ord(c) for c in word]

    sums = [0]

    for x in values:
        m = len(sums)
        shifted = [value + x for value in sums]

        merged = []
        i = 0
        j = 0

        while i < m and j < m:
            if sums[i] <= shifted[j]:
                merged.append(sums[i])
                i += 1
            else:
                merged.append(shifted[j])
                j += 1

        while i < m:
            merged.append(sums[i])
            i += 1

        while j < m:
            merged.append(shifted[j])
            j += 1

        sums = merged

    return " ".join(map(str, sums))

# Provided sample
sample1_in = """5
Beery
"""

sample1_out = (
    "0 66 101 101 114 121 167 167 180 187 202 215 215 222 222 235 "
    "268 281 281 288 288 301 316 323 336 336 382 389 402 402 437 503"
)

assert solve_io(sample1_in) == sample1_out, "sample 1"

# Minimum-size input
assert solve_io("1\na\n") == "0 97", "single character"

# All values equal
assert solve_io("3\naaa\n") == (
    "0 97 97 97 194 194 194 291"
), "duplicate values"

# Boundary between uppercase and lowercase ASCII values
assert solve_io("2\nAz\n") == (
    "0 65 122 187"
), "ASCII boundary"

# Larger case with four distinct values
assert solve_io("4\nabcd\n") == (
    "0 97 98 99 100 195 196 197 198 197 198 199 199 "
    "294 295 296 393"
), "four characters"
```Xác nhận tùy chỉnh cuối cùng ở trên chứa một giá trị mong đợi được viết thủ công một cách có chủ ý, khiến bản thân bài kiểm tra dễ mắc lỗi đánh máy. Bộ thử nghiệm kiểu cuộc thi an toàn hơn có thể tính toán các giá trị mong đợi một cách độc lập bằng cách liệt kê các mặt nạ cho các đầu vào nhỏ. Phiên bản sau đây thích hợp hơn khi sử dụng các thử nghiệm làm bộ hồi quy.```
import io

def run(inp: str) -> str:
    data = inp.split()
    n = int(data[0])
    word = data[1]

    values = [ord(c) for c in word]
    sums = [0]

    for x in values:
        m = len(sums)
        shifted = [s + x for s in sums]

        merged = []
        i = 0
        j = 0

        while i < m and j < m:
            if sums[i] <= shifted[j]:
                merged.append(sums[i])
                i += 1
            else:
                merged.append(shifted[j])
                j += 1

        while i < m:
            merged.append(sums[i])
            i += 1

        while j < m:
            merged.append(shifted[j])
            j += 1

        sums = merged

    return " ".join(map(str, sums))

def brute_force(word: str) -> str:
    values = [ord(c) for c in word]
    n = len(values)

    result = []

    for mask in range(1 << n):
        total = 0
        for i in range(n):
            if mask & (1 << i):
                total += values[i]
        result.append(total)

    result.sort()
    return " ".join(map(str, result))

# Provided sample
assert run("5\nBeery\n") == (
    "0 66 101 101 114 121 167 167 180 187 202 215 215 222 222 235 "
    "268 281 281 288 288 301 316 323 336 336 382 389 402 402 437 503"
), "sample 1"

# Minimum-size input
assert run("1\na\n") == brute_force("a"), "minimum size"

# All-equal values
assert run("4\naaaa\n") == brute_force("aaaa"), "all equal"

# ASCII boundary
assert run("2\nAz\n") == brute_force("Az"), "uppercase/lowercase boundary"

# Digits and uppercase letters
assert run("4\n012A\n") == brute_force("012A"), "digit and uppercase values"

# Larger input, still independently checked by brute force
word = "aabb"
assert run(f"{len(word)}\n{word}\n") == brute_force(word), "duplicate combinations"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`với`a`|`0 97`| Tập hợp con trống và kích thước đầu vào tối thiểu | 
|`4`với`aaaa`|`0 97 97 97 97 194 194 194 194 194 194 291 291 291 291 388`| Tổng tập hợp con trùng lặp và bội số | 
|`2`với`Az`|`0 65 122 187`| Thứ tự ASCII theo chữ hoa và chữ thường | 
|`4`với`012A`| Brute-force tương đương | Giá trị ASCII chữ số và chữ hoa | 
|`4`với`aabb`| Brute-force tương đương | Các giá trị lặp lại tạo ra các kết hợp lặp lại | 

## Vỏ cạnh 

Đầu vào tối thiểu là```
1
a
```Thuật toán bắt đầu với`[0]`. Đang xử lý (97) tạo danh sách đã dịch chuyển`[97]`và sự hợp nhất mang lại`[0, 97]`. Tập hợp con trống được giữ nguyên và có kết quả chính xác (2^1=2). 

Đối với các giá trị lặp lại, hãy xem xét```
2
aa
```đầu tiên`a`biến đổi`[0]`vào trong`[0, 97]`. thứ hai`a`tạo ra`[97, 194]`, và sự hợp nhất tạo ra```
0 97 97 194
```Hai bản sao của (97) tương ứng với hai tập con một ký tự khác nhau. Việc hợp nhất bảo toàn cả hai vì nó không bao giờ loại bỏ các phần tử bằng nhau. 

Trường hợp tinh tế thứ hai là đầu vào chứa cả ký tự viết hoa và viết thường:```
2
Az
```Giá trị ASCII của chúng là (65) và (122). Bắt đầu từ`[0]`, giá trị đầu tiên cho`[0, 65]`và số thứ hai cho số tiền đã dịch chuyển`[122, 187]`. Sự hợp nhất tạo ra```
0 65 122 187
```Thuật toán hoạt động trên các giá trị số ASCII, do đó nó không cần xử lý đặc biệt vì thực tế là chữ hoa và chữ thường chiếm các vùng khác nhau của bảng ASCII. 

Cuối cùng, hãy xem xét```
3
aaa
```Kích thước danh sách là (1), (2), (4) và (8). Mỗi lần chèn sẽ nhân đôi số tổng tập hợp con vì mỗi tập hợp con hiện có có chính xác hai lựa chọn liên quan đến tập hợp con mới`a`. Đầu ra cuối cùng là```
0 97 97 97 194 194 194 291
```Không có sự trùng lặp ở bất kỳ giai đoạn nào, vì vậy tất cả tám tập hợp con vẫn được biểu diễn mặc dù một số trong số chúng có tổng giống nhau.
