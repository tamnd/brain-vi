---
title: "CF 138A - Bài Văn"
description: "Mỗi bài thơ được chia thành bốn câu, nhóm bốn dòng. Hai dòng có vần nếu hậu tố bắt đầu từ nguyên âm thứ k tính từ cuối giống hệt nhau ở cả hai dòng. Ví dụ: với k = 1, chúng ta so sánh các hậu tố bắt đầu từ nguyên âm cuối cùng."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 138
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 99 (Div. 1)"
rating: 1600
weight: 138
solve_time_s: 120
verified: true
draft: false
---

[CF 138A - Bài học Văn](https://codeforces.com/problemset/problem/138/A) 

**Đánh giá:** 1600 
**Thẻ:** triển khai 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi bài thơ được chia thành bốn câu, nhóm bốn dòng. Hai dòng có vần nếu hậu tố bắt đầu từ nguyên âm thứ k tính từ cuối giống hệt nhau ở cả hai dòng. Ví dụ, với`k = 1`, chúng ta so sánh các hậu tố bắt đầu từ nguyên âm cuối cùng. Với`k = 2`, chúng ta so sánh các hậu tố bắt đầu từ nguyên âm thứ hai cuối cùng. 

Đối với mỗi quatrain, chúng tôi chỉ quan tâm đến bốn cấu trúc vần có thể có: 

-`aabb`, nghĩa là dòng`(1,2)`vần điệu và`(3,4)`vần điệu 
-`abab`, nghĩa`(1,3)`vần điệu và`(2,4)`vần điệu 
-`abba`, nghĩa`(1,4)`vần điệu và`(2,3)`vần điệu 
-`aaaa`, nghĩa là cả bốn dòng đều có vần với nhau 

Toàn bộ bài thơ có một sơ đồ hợp lệ nếu mỗi câu thơ bốn câu đều tương thích với cùng một cấu trúc. Một câu thơ bốn câu có đủ bốn dòng vần rất linh hoạt vì nó có thể thể hiện bất kỳ sơ đồ nào. 

Đầu vào mang lại`n`quatrain và giá trị`k`. Sau đó đến`4n`dòng văn bản. Chúng ta phải xuất ra một trong những sơ đồ hợp lệ hoặc`"NO"`nếu không có sơ đồ chung nào tồn tại. 

Các ràng buộc đủ nhỏ để chúng ta không cần các thuật toán chuỗi phức tạp. Có nhiều nhất`2500`quatrain, nhiều nhất là`10000`dòng. Tổng chiều dài của tất cả các chuỗi chỉ là`10^4`, có nghĩa là thậm chí quét từng ký tự nhiều lần cũng rẻ. MỘT`O(total_length)`hoặc`O(total_length + n)`giải pháp dễ dàng đủ nhanh. 

Phần khó khăn không phải là hiệu suất, mà là xử lý chính xác các quy tắc vần điệu. 

Một sai lầm phổ biến là xử lý các dòng có ít hơn`k`các nguyên âm phù hợp với nhau. Chúng không bao giờ có vần với bất cứ thứ gì, kể cả một dòng ngắn khác. 

Ví dụ:```
1 2
sky
try
day
may
```Hai dòng đầu tiên chỉ chứa một nguyên âm, vì vậy chúng không thể vần với nhau.`k = 2`. Đầu ra đúng là:```
NO
```Một lỗi dễ dàng khác là quên điều đó`aaaa`quatrains tương thích với mọi chương trình. 

Ví dụ:```
2 1
day
may
sun
fun
code
mode
road
toad
```Quatrain đầu tiên là`aabb`. Thứ hai là`aaaa`. Toàn bộ bài thơ vẫn có giá trị như`aabb`. 

Trường hợp tinh tế thứ ba xuất hiện khi nhiều sơ đồ phù hợp riêng lẻ, nhưng không có sơ đồ nào hoạt động trên toàn cầu. 

Ví dụ:```
2 1
day
may
sun
fun
red
bed
red
bed
```Quatrain đầu tiên chỉ là`aabb`. Thứ hai là`abab`Và`abba`. Không có một sơ đồ chung nào cho tất cả các câu thơ bốn câu, nên câu trả lời là:```
NO
```## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ so sánh trực tiếp từng dòng ký tự bất cứ khi nào chúng ta cần kiểm tra xem hai dòng có vần với nhau hay không. Đối với mỗi so sánh, chúng tôi quét ngược để xác định vị trí nguyên âm thứ k, trích xuất hậu tố và so sánh nó với một hậu tố khác. 

Điều này hoạt động vì tổng kích thước đầu vào nhỏ. Ngay cả khi chúng tôi thực hiện một số phép so sánh trên mỗi câu thơ, tổng lượng văn bản đã xử lý vẫn có thể quản lý được. 

Điểm yếu của phiên bản brute-force là sự lặp lại. Cùng một dòng có thể tham gia vào một số quá trình kiểm tra vần, khiến chúng tôi phải tính toán lại hậu tố vần của nó nhiều lần. 

Quan sát quan trọng là một dòng có thể được rút gọn thành một cách biểu diễn chính tắc duy nhất: hậu tố bắt đầu từ nguyên âm thứ k tính từ cuối. Một khi chúng ta tính toán hậu tố này một lần, việc kiểm tra vần sẽ trở thành việc kiểm tra tính bằng nhau đơn giản. 

Ví dụ:```
commit -> ommit   (k = 2)
hermit -> ermit
```Bây giờ vần điệu giảm xuống:```
suffix1 == suffix2
```Nếu một dòng có ít hơn`k`nguyên âm, chúng tôi gán cho nó một điểm đánh dấu không hợp lệ và nó tự động thất bại trong mọi bài kiểm tra vần. 

Mỗi câu bốn câu chỉ có bốn dòng nên chỉ có ba mẫu ghép nối có ý nghĩa để kiểm tra. Sau khi xác định sơ đồ nào mà quatrain hỗ trợ, chúng tôi kết hợp các khả năng đó với tập hợp các sơ đồ hợp lệ toàn cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n × Average_line_length × lặp lại_checks) | O(1) | Đã chấp nhận | 
| Tối ưu | O(total_length + n) | O(tổng_chiều dài) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các dòng đầu vào. 
2. Với mỗi dòng, hãy tính hậu tố vần của nó. 

Quét chuỗi từ phải sang trái trong khi đếm nguyên âm. Khi đạt đến nguyên âm thứ k, hãy lưu chuỗi con bắt đầu từ đó. 

Nếu quá trình quét kết thúc trước khi tìm thấy`k`nguyên âm, đánh dấu dòng là không hợp lệ. 
3. Xử lý từng câu một cách độc lập. 

Đặt bốn hậu tố được xử lý là`a, b, c, d`. 
4. Xác định sơ đồ vần nào mà câu thơ này hỗ trợ. 

Vần giữa hai dòng chỉ tồn tại nếu cả hai hậu tố đều hợp lệ và bằng nhau. 
5. Kiểm tra vần bốn dòng trước. 

Nếu tất cả bốn hậu tố đều bằng nhau và hợp lệ thì quatrain này hỗ trợ`aaaa`. 
6. Kiểm tra các mẫu còn lại.`aabb`yêu cầu:```
a == b and c == d
```

`abab`yêu cầu:```
a == c and b == d
```

`abba`yêu cầu:```
a == d and b == c
```7. Duy trì một tập hợp toàn cầu các sơ đồ thơ vẫn có thể thực hiện được. 

Ban đầu:```
{aabb, abab, abba}
```Một quatrain hỗ trợ`aaaa`không hạn chế bất cứ điều gì. 

Ngược lại, hãy giao tập hợp toàn cục với các lược đồ được hỗ trợ bởi câu bốn câu này. 
8. Nếu tập hợp chung trở nên trống, hãy in`"NO"`. 
9. Nếu không thì in: 

-`"aaaa"`nếu mọi câu thơ đều có vần điệu 
- bất kỳ sơ đồ còn lại nào khác 

### Tại sao nó hoạt động 

Mỗi dòng được chuyển đổi thành chuỗi con chính xác xác định vần theo quy tắc của bài toán. Hai dòng vần khi và chỉ khi các hậu tố được xử lý này bằng nhau và hợp lệ. 

Đối với mỗi câu thơ, chúng tôi kiểm tra rõ ràng tất cả các cấu trúc vần hợp pháp. Một sơ đồ chỉ tồn tại trên toàn cầu nếu mọi câu thơ đều tương thích với nó. Vì thuật toán không bao giờ loại bỏ một sơ đồ hợp lệ thực sự và không bao giờ chấp nhận một sơ đồ không hợp lệ, nên tập hợp còn lại sau tất cả các giao điểm chính xác là tập hợp các sơ đồ thơ hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

VOWELS = set("aeiou")

def get_suffix(s, k):
    cnt = 0

    for i in range(len(s) - 1, -1, -1):
        if s[i] in VOWELS:
            cnt += 1

        if cnt == k:
            return s[i:]

    return None

def solve():
    n, k = map(int, input().split())

    schemes = {"aabb", "abab", "abba"}
    all_aaaa = True

    for _ in range(n):
        lines = [input().strip() for _ in range(4)]
        suf = [get_suffix(x, k) for x in lines]

        possible = set()

        if (
            suf[0] is not None and
            suf[0] == suf[1] == suf[2] == suf[3]
        ):
            possible.add("aaaa")

        if (
            suf[0] is not None and
            suf[0] == suf[1] and
            suf[2] is not None and
            suf[2] == suf[3]
        ):
            possible.add("aabb")

        if (
            suf[0] is not None and
            suf[0] == suf[2] and
            suf[1] is not None and
            suf[1] == suf[3]
        ):
            possible.add("abab")

        if (
            suf[0] is not None and
            suf[0] == suf[3] and
            suf[1] is not None and
            suf[1] == suf[2]
        ):
            possible.add("abba")

        if "aaaa" not in possible:
            all_aaaa = False
            schemes &= possible

    if all_aaaa:
        print("aaaa")
    elif schemes:
        print(sorted(schemes)[0])
    else:
        print("NO")

solve()
```Chức năng trợ giúp`get_suffix`thực hiện định nghĩa vần một cách trực tiếp. Nó quét ngược cho đến khi đạt nguyên âm thứ k. Trở về`None`đối với các dòng không hợp lệ sẽ an toàn hơn việc trả về một chuỗi trống vì các chuỗi trống có thể vô tình so sánh bằng nhau. 

Logic quatrain phản ánh định nghĩa toán học của từng sơ đồ vần. Mọi so sánh đều kiểm tra rõ ràng xem các hậu tố tham gia có hợp lệ hay không. 

các`all_aaaa`cờ xử lý quy tắc đặc biệt rằng nếu mỗi câu thơ bốn vần đều có vần điệu thì câu trả lời phải là`"aaaa"`chứ không phải là một sơ đồ tương thích tùy ý. 

Bước giao nhau toàn cầu là ý tưởng cốt lõi. Mỗi quatrain hạn chế tập hợp các sơ đồ bài thơ được phép. Một khi kế hoạch thất bại trong một câu thơ, nó sẽ không bao giờ có thể phục hồi sau này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1
day
may
sun
fun
```Hậu tố được xử lý: 

| Dòng | Hậu tố | 
| --- | --- | 
| ngày | vâng | 
| tháng năm | vâng | 
| mặt trời | un | 
| vui vẻ | un | 

Kiểm tra sơ đồ: 

| Đề án | hợp lệ | 
| --- | --- | 
| aaa | Không | 
| aabb | Có | 
| abab | Không | 
| abba | Không | 

Câu trả lời cuối cùng:```
aabb
```Dấu vết này cho thấy trường hợp không tầm thường đơn giản nhất. Chỉ có một kiểu ghép nối hoạt động nên sơ đồ bài thơ được xác định duy nhất. 

### Ví dụ 2 

đầu vào:```
2 1
code
mode
road
toad
red
bed
red
bed
```Quatrain đầu tiên: 

| Dòng | Hậu tố | 
| --- | --- | 
| mã | ca ngợi | 
| chế độ | ca ngợi | 
| đường | oa | 
| cóc | oa | 

Đề án hợp lệ: 

| Đề án | hợp lệ | 
| --- | --- | 
| aaa | Không | 
| aabb | Có | 
| abab | Không | 
| abba | Không | 

Tập hợp toàn cầu trở thành:```
{aabb}
```quatrain thứ hai: 

| Dòng | Hậu tố | 
| --- | --- | 
| đỏ | ed | 
| giường | ed | 
| đỏ | ed | 
| giường | ed | 

Đề án hợp lệ: 

| Đề án | hợp lệ | 
| --- | --- | 
| aaa | Có | 
| aabb | Có | 
| abab | Có | 
| abba | Có | 

Quatrain thứ hai không áp đặt hạn chế nào vì`aaaa`là hợp lệ. 

Câu trả lời cuối cùng:```
aabb
```Điều này chứng tỏ tại sao`aaaa`quatrain phải được xử lý đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(total_length + n) | Mỗi ký tự được quét một lần trong khi trích xuất hậu tố | 
| Không gian | O(tổng_chiều dài) | Chuỗi hậu tố được lưu trữ chiếm ưu thế trong việc sử dụng bộ nhớ | 

Tổng kích thước đầu vào chỉ`10^4`ký tự, vì vậy giải pháp này dễ dàng nằm trong giới hạn. Ngay cả việc cắt chuỗi Python cũng hoàn toàn an toàn ở quy mô này. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

VOWELS = set("aeiou")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def get_suffix(s, k):
        cnt = 0

        for i in range(len(s) - 1, -1, -1):
            if s[i] in VOWELS:
                cnt += 1

            if cnt == k:
                return s[i:]

        return None

    n, k = map(int, input().split())

    schemes = {"aabb", "abab", "abba"}
    all_aaaa = True

    for _ in range(n):
        lines = [input().strip() for _ in range(4)]
        suf = [get_suffix(x, k) for x in lines]

        possible = set()

        if (
            suf[0] is not None and
            suf[0] == suf[1] == suf[2] == suf[3]
        ):
            possible.add("aaaa")

        if (
            suf[0] is not None and
            suf[0] == suf[1] and
            suf[2] is not None and
            suf[2] == suf[3]
        ):
            possible.add("aabb")

        if (
            suf[0] is not None and
            suf[0] == suf[2] and
            suf[1] is not None and
            suf[1] == suf[3]
        ):
            possible.add("abab")

        if (
            suf[0] is not None and
            suf[0] == suf[3] and
            suf[1] is not None and
            suf[1] == suf[2]
        ):
            possible.add("abba")

        if "aaaa" not in possible:
            all_aaaa = False
            schemes &= possible

    if all_aaaa:
        return "aaaa"
    elif schemes:
        return sorted(schemes)[0]
    else:
        return "NO"

# provided sample
assert run(
"""1 1
day
may
sun
fun
"""
) == "aabb"

# minimum-size input with full rhyme
assert run(
"""1 1
cat
hat
mat
bat
"""
) == "aaaa"

# insufficient vowels
assert run(
"""1 2
sky
try
day
may
"""
) == "NO"

# conflicting schemes
assert run(
"""2 1
day
may
sun
fun
red
bed
red
bed
"""
) == "aabb"

# only abab works
assert run(
"""1 1
light
stone
bright
phone
"""
) == "abab"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Quatrain đơn với tất cả các vần | aaa | Xử lý đặc biệt vần phổ thông | 
| Dòng có quá ít nguyên âm | KHÔNG | Những dòng không hợp lệ không bao giờ có vần | 
| Quatrain tương thích hỗn hợp | aabb | Logic giao lộ toàn cầu | 
| Chỉ ghép nối chéo | abab | Phát hiện sơ đồ chính xác | 
| Đầu vào hợp lệ nhỏ nhất | aaa | Xử lý kích thước ranh giới | 

## Vỏ cạnh 

Hãy xem xét trường hợp các dòng không chứa đủ nguyên âm. 

đầu vào:```
1 2
sky
try
day
may
```Thuật toán tính toán: 

| Dòng | Hậu tố | 
| --- | --- | 
| bầu trời | Không có | 
| thử | Không có | 
| ngày | vâng | 
| tháng năm | vâng | 

Mỗi lược đồ yêu cầu ít nhất một so sánh liên quan đến các hậu tố không hợp lệ, do đó không có lược đồ nào được thêm vào`possible`. Câu trả lời cuối cùng trở thành`"NO"`. 

Bây giờ hãy xem xét một câu thơ bốn câu có vần điệu đầy đủ. 

đầu vào:```
1 1
red
bed
fed
led
```Tất cả các hậu tố trở thành`"ed"`. 

Thuật toán đánh dấu tất cả bốn lược đồ là hợp lệ, bao gồm`aaaa`. Vì mọi quatrain đều thỏa mãn`aaaa`, đầu ra cuối cùng chính xác là:```
aaaa
```Điều này quan trọng vì việc in ấn`"aabb"`về mặt kỹ thuật sẽ mô tả mẫu vần, nhưng vấn đề đòi hỏi một cách rõ ràng`"aaaa"`khi mỗi câu thơ đều có vần điệu. 

Cuối cùng, hãy xem xét những câu quatrain không tương thích. 

đầu vào:```
2 1
day
may
sun
fun
cat
dog
cat
dog
```Quatrain đầu tiên chỉ hỗ trợ`aabb`. 

Chỉ hỗ trợ thứ hai`abab`. 

Sau ngã tư:```
{aabb} ∩ {abab} = empty
```Thuật toán ngay lập tức phát hiện rằng không có sơ đồ toàn cục nào tồn tại và in:```
NO
```
