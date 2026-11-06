---
title: "CF 126B - Mật khẩu"
description: "Chúng ta được cho một chuỗi chữ thường duy nhất và cần tìm chuỗi dài nhất thỏa mãn cùng lúc ba điều kiện. Chuỗi được chọn phải là tiền tố của chuỗi gốc, hậu tố của chuỗi gốc và cũng xuất hiện ở đâu đó bên trong chuỗi."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "hashing", "string-suffix-structures", "strings"]
categories: ["algorithms"]
codeforces_contest: 126
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 93 (Div. 1 Only)"
rating: 1700
weight: 126
solve_time_s: 124
verified: true
draft: false
---

[CF 126B - Mật khẩu](https://codeforces.com/problemset/problem/126/B) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, dp, băm, cấu trúc hậu tố chuỗi, chuỗi 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi chữ thường duy nhất và cần tìm chuỗi dài nhất thỏa mãn cùng lúc ba điều kiện. 

Chuỗi được chọn phải là tiền tố của chuỗi gốc, hậu tố của chuỗi gốc và cũng xuất hiện ở đâu đó bên trong chuỗi. Sự xuất hiện bên trong không thể sử dụng toàn bộ tiền tố hoặc toàn bộ vị trí hậu tố. 

Ví dụ, trong`fixprefixsuffix`, chuỗi`fix`xuất hiện ở đầu, cuối và cả trong từ`prefix`, vì vậy đó là một câu trả lời hợp lệ. 

Độ dài chuỗi có thể đạt tới một triệu ký tự. Điều đó ngay lập tức loại trừ bất cứ điều gì bậc hai. Thậm chí một`O(n^2)`thuật toán sẽ yêu cầu khoảng`10^12`hoạt động trong trường hợp xấu nhất, vượt xa những gì phù hợp trong hai giây. Chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. 

Phần khó khăn là không kiểm tra xem một chuỗi có vừa là tiền tố vừa là hậu tố hay không. Điều đó có thể được thực hiện với các kỹ thuật chuỗi tiêu chuẩn. Khó khăn thực sự là việc xác minh rằng chuỗi tương tự cũng xuất hiện ở đâu đó ở giữa. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét đầu vào:```
aaaa
```Câu trả lời đúng là:```
aa
```Tiền tố`aaa`cũng là một hậu tố nhưng nó không xuất hiện hoàn toàn bên trong chuỗi. Hai lần xuất hiện của nó chính xác là vị trí tiền tố và hậu tố. Việc triển khai bất cẩn có thể trả về sai`aaa`. 

Một trường hợp nguy hiểm khác là:```
abcdabc
```Tiền tố`abc`cũng là hậu tố nhưng không có sự xuất hiện ở giữa. Đầu ra đúng là:```
Just a legend
```Việc triển khai chỉ kiểm tra tính bằng nhau của tiền tố-hậu tố sẽ thất bại ở đây. 

Một trường hợp tối thiểu cũng có vấn đề:```
a
```Không có chuỗi con không trống nào có thể đồng thời là tiền tố thích hợp, hậu tố thích hợp và chuỗi con ở giữa. Câu trả lời phải là:```
Just a legend
```Các chuỗi ký tự lặp lại tạo ra một tình huống tế nhị khác:```
aaaaa
```Câu trả lời đúng là:```
aaa
```Chuỗi`aaaa`vừa là tiền tố vừa là hậu tố, nhưng một lần nữa nó không xảy ra hoàn toàn bên trong. Chúng ta phải phân biệt giữa các lần xuất hiện chồng chéo và các lần xuất hiện nội bộ hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Liệt kê mọi tiền tố có thể có của chuỗi. Đối với mỗi tiền tố, hãy kiểm tra xem nó có phải là hậu tố hay không và liệu nó có xuất hiện ở đâu đó ở giữa hay không. 

Giả sử độ dài chuỗi là`n`. có`n`tiền tố có thể. So sánh tiền tố với hậu tố có giá lên tới`O(n)`và việc tìm kiếm một lần xuất hiện khác trong chuỗi cũng có giá lên tới`O(n)`. Tổng độ phức tạp trở thành`O(n^2)`hoặc tệ hơn tùy thuộc vào phương pháp tìm kiếm. 

Với`n = 10^6`, thậm chí`O(n^2)`là hoàn toàn không khả thi. 

Quan sát chính là vấn đề chỉ quan tâm đến đường viền của chuỗi. Đường viền là một chuỗi con vừa là tiền tố vừa là hậu tố. Thay vì kiểm tra tất cả các chuỗi con, chúng ta chỉ cần kiểm tra đường viền. 

Điều này đương nhiên dẫn đến hàm tiền tố từ thuật toán Knuth-Morris-Pratt. Chức năng tiền tố`pi[i]`lưu trữ độ dài của tiền tố thích hợp dài nhất của chuỗi cũng là hậu tố kết thúc tại vị trí`i`. 

Đối với chuỗi đầy đủ,`pi[n - 1]`cho chiều dài đường viền dài nhất. Nếu đường viền đó cũng xuất hiện ở đâu đó trước đó trong mảng hàm tiền tố thì nó xuất hiện ở giữa và là một câu trả lời hợp lệ. 

Nếu không, chúng ta có thể chuyển sang đường viền nhỏ hơn tiếp theo bằng cách sử dụng chính các liên kết hàm tiền tố. Đây là phần hay ho của KMP: các đường viền tạo thành một chuỗi. Thay vì tính toán lại bất cứ thứ gì, chúng tôi liên tục di chuyển từ đường viền này sang đường viền nhỏ hơn tiếp theo trong thời gian không đổi. 

Điều này biến vấn đề thành quét tuyến tính cộng với một vài bước nhảy biên, mang lại`O(n)`sự phức tạp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính mảng hàm tiền tố`pi`cho toàn bộ chuỗi. 

Hàm tiền tố cho chúng ta biết, đối với mọi vị trí, độ dài của đường viền dài nhất kết thúc ở đó. 
2. Nhìn vào`pi[n - 1]`. 

Giá trị này là độ dài của tiền tố dài nhất cũng là hậu tố của toàn bộ chuỗi. 
3. Ghi lại mọi giá trị tiền tố-hàm xuất hiện trước vị trí cuối cùng. 

Chúng tôi chỉ quan tâm đến vị trí`0`bởi vì`n - 2`bởi vì vị trí cuối cùng tương ứng với toàn bộ hậu tố chuỗi chứ không phải vị trí xuất hiện ở giữa. 
4. Hãy để`x = pi[n - 1]`. 

Đây là độ dài câu trả lời ứng viên hiện tại của chúng tôi. 
5. Trong khi`x > 0`, kiểm tra xem`x`xuất hiện ở đâu đó trước đó trong mảng hàm tiền tố. 

Nếu đúng như vậy thì tiền tố của độ dài`x`xảy ra ở giữa, vì vậy chúng tôi đã tìm được câu trả lời hợp lệ dài nhất. 
6. Nếu`x`không xuất hiện trước đó, hãy di chuyển đến đường viền nhỏ hơn tiếp theo bằng cách sử dụng:```
x = pi[x - 1]
```Điều này hoạt động vì đường viền của đường viền cũng là đường viền. 
7. Nếu vòng lặp kết thúc bằng`x = 0`, không tồn tại chuỗi con hợp lệ. 

### Tại sao nó hoạt động 

Hàm tiền tố nắm bắt tất cả các mối quan hệ đường viền trong chuỗi. 

Mỗi câu trả lời hợp lệ phải là một đường viền của chuỗi đầy đủ, vì vậy bắt đầu từ`pi[n - 1]`đảm bảo chúng tôi kiểm tra các ứng viên từ dài nhất đến ngắn nhất. 

Nếu độ dài đường viền xuất hiện ở đâu đó trước đó trong mảng hàm tiền tố, điều đó có nghĩa là tiền tố tương tự kết thúc ở một vị trí bên trong nào đó, điều này chứng tỏ chuỗi con xuất hiện ở giữa. 

Sự chuyển tiếp`x = pi[x - 1]`di chuyển chính xác đến đường viền có thể tiếp theo vì mọi đường viền hợp lệ nhỏ hơn của chuỗi đầy đủ cũng phải là đường viền của đường viền hiện tại. 

Vì chúng ta kiểm tra các đường viền theo thứ tự giảm dần nên đường viền hợp lệ đầu tiên sẽ tự động dài nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    pi = [0] * n

    j = 0
    for i in range(1, n):
        while j > 0 and s[i] != s[j]:
            j = pi[j - 1]

        if s[i] == s[j]:
            j += 1

        pi[i] = j

    seen = [False] * (n + 1)

    for i in range(n - 1):
        seen[pi[i]] = True

    x = pi[n - 1]

    while x > 0:
        if seen[x]:
            print(s[:x])
            return
        x = pi[x - 1]

    print("Just a legend")

solve()
```Phần đầu tiên tính toán hàm tiền tố KMP theo thời gian tuyến tính. 

Biến`j`lưu trữ độ dài tiền tố phù hợp hiện tại. Bất cứ khi nào các ký tự không khớp, chúng tôi sẽ nhảy lùi bằng cách sử dụng các giá trị hàm tiền tố đã tính toán trước đó thay vì bắt đầu lại từ số 0. Đây là lý do KMP vẫn tuyến tính. 

các`seen`mảng ghi lại độ dài đường viền xuất hiện trước vị trí cuối cùng. Việc sử dụng mảng boolean thay vì một tập hợp sẽ tránh được chi phí băm đối với các đầu vào rất lớn. 

Vòng lặp tìm kiếm bắt đầu từ đường viền dài nhất của toàn bộ chuỗi. Nếu đường viền đó xuất hiện bên trong, chúng tôi sẽ in nó ngay lập tức vì chúng tôi đang kiểm tra các ứng cử viên từ lớn nhất đến nhỏ nhất. 

Dòng:```
x = pi[x - 1]
```là bước chuyển tiếp quan trọng nhất. Nó đi qua chuỗi biên giới mà không cần tính toán lại bất cứ thứ gì. 

Một lỗi phổ biến là kiểm tra tất cả các vị trí bao gồm`n - 1`khi đổ đầy`seen`. Điều đó sẽ chấp nhận không chính xác các đường viền chỉ xuất hiện dưới dạng hậu tố đầy đủ. 

Một lỗi thường gặp khác xảy ra với`pi[x - 1]`. Hàm tiền tố lưu trữ thông tin theo vị trí kết thúc, do đó đường viền của tiền tố có độ dài`x`được tìm thấy tại chỉ mục`x - 1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
fixprefixsuffix
```Mảng hàm tiền tố trở thành: 

| Chỉ mục | Nhân vật | pi[i] | 
| --- | --- | --- | 
| 0 | f | 0 | 
| 1 | tôi | 0 | 
| 2 | x | 0 | 
| 3 | p | 0 | 
| 4 | r | 0 | 
| 5 | e | 0 | 
| 6 | f | 1 | 
| 7 | tôi | 2 | 
| 8 | x | 3 | 
| 9 | s | 0 | 
| 10 | bạn | 0 | 
| 11 | f | 1 | 
| 12 | f | 1 | 
| 13 | tôi | 2 | 
| 14 | x | 3 | 

Bây giờ: 

| Bước | x | đã thấy[x] | Hành động | 
| --- | --- | --- | --- | 
| Ban đầu | 3 | Đúng | Độ dài tiền tố đầu ra 3 | 

Câu trả lời là:```
fix
```Dấu vết này cho thấy tình huống lý tưởng khi đường viền dài nhất đã xuất hiện ở giữa. 

### Ví dụ 2 

đầu vào:```
abcdabc
```Mảng chức năng tiền tố: 

| Chỉ mục | Nhân vật | pi[i] | 
| --- | --- | --- | 
| 0 | một | 0 | 
| 1 | b | 0 | 
| 2 | c | 0 | 
| 3 | d | 0 | 
| 4 | một | 1 | 
| 5 | b | 2 | 
| 6 | c | 3 | 

Bây giờ: 

| Bước | x | đã thấy[x] | Tiếp theo x | 
| --- | --- | --- | --- | 
| Ban đầu | 3 | Sai | pi[2] = 0 | 

Không còn ứng cử viên nào, vì vậy đầu ra là:```
Just a legend
```Điều này chứng tỏ tại sao chỉ kiểm tra sự bằng nhau giữa tiền tố và hậu tố là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Tính toán hàm tiền tố và truyền tải đường viền là tuyến tính | 
| Không gian | O(n) | Mảng`pi`Và`seen`mỗi cửa hàng`n`số nguyên/boolean | 

Với`n`lên tới một triệu, độ phức tạp tuyến tính là cần thiết. Giải pháp chỉ thực hiện một vài lần truyền qua chuỗi và vừa vặn thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    s = input().strip()
    n = len(s)

    pi = [0] * n

    j = 0
    for i in range(1, n):
        while j > 0 and s[i] != s[j]:
            j = pi[j - 1]

        if s[i] == s[j]:
            j += 1

        pi[i] = j

    seen = [False] * (n + 1)

    for i in range(n - 1):
        seen[pi[i]] = True

    x = pi[n - 1]

    while x > 0:
        if seen[x]:
            print(s[:x])
            return

        x = pi[x - 1]

    print("Just a legend")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run("fixprefixsuffix\n") == "fix", "sample 1"

# minimum size
assert run("a\n") == "Just a legend", "single character"

# repeated characters
assert run("aaaaa\n") == "aaa", "overlapping borders"

# border exists but no middle occurrence
assert run("abcdabc\n") == "Just a legend", "no internal occurrence"

# multiple nested borders
assert run("ababa\n") == "a", "fallback to smaller border"

# all equal large-style pattern
assert run("aaaaaaaa\n") == "aaaaaa", "largest valid middle border"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`Just a legend`| Đầu vào kích thước tối thiểu | 
|`aaaaa`|`aaa`| Các sự cố chồng chéo được xử lý chính xác | 
|`abcdabc`|`Just a legend`| Chỉ tiền tố-hậu tố là không đủ | 
|`ababa`|`a`| Dự phòng chính xác thông qua chuỗi biên giới | 
|`aaaaaaaa`|`aaaaaa`| Phân cấp biên giới sâu | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
aaaa
```Mảng hàm tiền tố là: 

| Chỉ mục | Nhân vật | pi[i] | 
| --- | --- | --- | 
| 0 | một | 0 | 
| 1 | một | 1 | 
| 2 | một | 2 | 
| 3 | một | 3 | 

Đường viền dài nhất có chiều dài`3`, tương ứng với`aaa`. 

Chúng tôi kiểm tra xem`3`xuất hiện trước vị trí cuối cùng. Nó không. Thuật toán quay trở lại:```
x = pi[2] = 2
```Hiện nay`2`không xuất hiện sớm hơn, vì vậy câu trả lời trở thành`aa`. 

Điều này loại bỏ chính xác các đường viền chỉ xuất hiện dưới dạng tiền tố và hậu tố. 

Bây giờ hãy xem xét:```
abcdabc
```Chiều dài đường viền dài nhất là`3`, tương ứng với`abc`. 

giá trị`3`không bao giờ xuất hiện trước vị trí cuối cùng trong mảng hàm tiền tố, do đó không có sự xuất hiện ở giữa. Thuật toán quay trở lại`0`và in:```
Just a legend
```Điều này ngăn chặn việc chấp nhận sai một chuỗi con chỉ xuất hiện hai lần. 

Cuối cùng, hãy xem xét:```
ababa
```Biên giới dài nhất là`aba`, chiều dài`3`. 

Biên giới đó không xảy ra trong nội bộ. Thuật toán quay trở lại bằng cách sử dụng:```
x = pi[2] = 1
```Bây giờ biên giới`a`hợp lệ vì nó xuất hiện nhiều lần trong chuỗi. 

Đầu ra trở thành:```
a
```Ví dụ này cho thấy tại sao việc vượt qua chuỗi biên giới là cần thiết. Đường viền dài nhất có thể thất bại, trong khi đường viền nhỏ hơn vẫn thành công.
