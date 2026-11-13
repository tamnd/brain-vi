---
title: "CF 17E - Phân chia"
description: "Chúng ta được cho một chuỗi và chúng ta xem xét mọi chuỗi con palindromic bên trong chuỗi đó. Mỗi lần xuất hiện đều có ý nghĩa riêng biệt, ngay cả khi hai chuỗi con có cùng văn bản. Ví dụ: trong \"aaa\" có ba lần xuất hiện khác nhau của \"a\"."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "strings"]
categories: ["algorithms"]
codeforces_contest: 17
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 17"
rating: 2900
weight: 17
solve_time_s: 230
verified: true
draft: false
---
[CF 17E - Palisection](https://codeforces.com/problemset/problem/17/E) 

**Xếp hạng:** 2900 
**Thẻ:** chuỗi 
**Thời gian giải:** 3 phút 50 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi và chúng ta xem xét mọi chuỗi con palindromic bên trong chuỗi đó. Mỗi lần xuất hiện đều có ý nghĩa riêng biệt, ngay cả khi hai chuỗi con có cùng văn bản. Ví dụ, trong`"aaa"`có ba lần xuất hiện khác nhau của`"a"`. 

Mỗi palindrome tương ứng với một khoảng`[l, r]`. Chúng ta phải đếm xem có bao nhiêu cặp khoảng palindromic khác nhau không có thứ tự giao nhau. Hai khoảng giao nhau nếu chúng có chung ít nhất một vị trí. 

Giải thích trực tiếp là: 

1. Liệt kê mọi chuỗi con palindromic. 
2. Hãy xem xét từng cặp trong số chúng. 
3. Đếm các cặp có khoảng cách trùng nhau. 

Chiều dài chuỗi có thể đạt tới`2 * 10^6`, điều này thay đổi hoàn toàn những gì khả thi. Thậm chí một`O(n^2)`thuật toán là không thể ở đây. Quét bậc hai trên tất cả các cặp sẽ yêu cầu khoảng`4 * 10^12`hoạt động trong trường hợp xấu nhất. Chúng ta cần một cái gì đó gần với thời gian tuyến tính. 

Một khó khăn tiềm ẩn khác là số lượng chuỗi con palindromic bản thân nó đã có thể là bậc hai. Một chuỗi như`"aaaa....a"`chứa khoảng`n(n+1)/2`palindrome. Chúng tôi không thể lưu trữ rõ ràng tất cả các khoảng thời gian. 

mô-đun`51123987`cũng quan trọng. Số lượng trung gian có thể trở nên rất lớn, vì vậy mọi phép toán số học phải giữ nguyên giá trị này theo modulo. 

Có một số trường hợp khó bỏ sót. 

Hãy xem xét đầu vào nhỏ nhất:```
1
a
```Chỉ có một palindrome nên câu trả lời là`0`. Việc triển khai bất cẩn có thể vô tình đếm một palindrome được ghép nối với chính nó. 

Bây giờ hãy xem xét:```
2
aa
```Các palindrome là: 

-`[1,1]`-`[2,2]`-`[1,2]`Các cặp giao nhau là: 

-`[1,1]`với`[1,2]`-`[2,2]`với`[1,2]`Câu trả lời là`2`. Nếu chúng ta đếm các cặp không chồng lên nhau và trừ đi tổng số cặp, chúng ta phải cẩn thận để không đếm kép các cặp có thứ tự. 

Một ví dụ phức tạp khác là:```
4
abba
```Palindrome: 

- bốn chữ cái duy nhất 
-`"bb"`-`"abba"`bảng màu`"abba"`giao nhau với mọi palindrome khác. Các thuật toán chỉ nhìn vào các trung tâm cục bộ thường quên các palindrome dài bao gồm nhiều palindrome nhỏ hơn. 

Trường hợp xấu nhất là chuỗi ký tự lặp lại:```
5
aaaaa
```Mỗi chuỗi con là một palindrome. có`15`tổng số palindromes, vì vậy có`105`cặp không có thứ tự. Chỉ nên loại trừ các cặp khoảng cách rời nhau. Bất kỳ triển khai nào tạo ra tất cả các palindrome một cách rõ ràng sẽ hết cả thời gian và bộ nhớ đối với dung lượng lớn.`n`. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi liệt kê mọi chuỗi con palindromic, lưu trữ tất cả các khoảng, sau đó kiểm tra từng cặp xem có trùng nhau không. 

Việc kiểm tra xem một chuỗi con có phải là một chuỗi palindrome hay không có thể mất thời gian tuyến tính, nhưng ngay cả khi chúng ta tối ưu hóa chuỗi đó bằng cách mở rộng trung tâm hoặc thuật toán Manacher thì tổng số palindrome vẫn có thể là`O(n^2)`. Sau khi tạo chúng, việc kiểm tra tất cả các cặp sẽ tốn một hệ số bậc hai khác. 

Đối với một chuỗi có độ dài`2 * 10^6`, số lượng palindrome có thể vào khoảng`2 * 10^12`. So sánh theo cặp trở nên lớn về mặt thiên văn. 

Quan sát quan trọng là việc đếm các cặp giao nhau một cách trực tiếp thì khó khăn, nhưng việc đếm các cặp không giao nhau thì dễ dàng hơn nhiều. 

Giả sử chúng ta biết: 

-`T`= tổng số chuỗi con palindromic. 
-`D`= số cặp palindromes rời rạc không có thứ tự. 

Thì câu trả lời chỉ đơn giản là:```
T choose 2 - D
```Hai palindromes rời nhau chính xác khi một palindrome kết thúc trước khi palindrome kia bắt đầu. 

Điều đó thay đổi vấn đề thành một cái gì đó có cấu trúc hơn nhiều. Nếu chúng ta có thể tính toán: 

- có bao nhiêu palindromes kết thúc ở mỗi vị trí, 
- có bao nhiêu palindromes bắt đầu ở mỗi vị trí, 

thì chúng ta có thể đếm các cặp rời nhau bằng tổng tiền tố. 

Thử thách còn lại là đạt được số lượng này mà không liệt kê từng bảng màu riêng lẻ. 

Đây chính xác là lúc thuật toán của Manacher trở nên hữu ích. Manacher tính toán theo thời gian tuyến tính: 

- bán kính palindrome lẻ tối đa ở mọi trung tâm, 
- bán kính palindrome chẵn tối đa ở mọi trung tâm. 

Mỗi bán kính đại diện cho nhiều palindromes. 

Từ những bán kính này, chúng ta có thể xây dựng lại số lượng palindrome bắt đầu hoặc kết thúc ở mỗi chỉ mục bằng cách sử dụng các mảng khác nhau. Thay vì lặp lại từng palindrome riêng lẻ, chúng tôi thêm toàn bộ phạm vi cùng một lúc. 

Điều đó mang lại một`O(n)`giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^4) ngây thơ, O(n^2) với bảng liệt kê Manacher | O(n^2) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy thuật toán Manacher để tính bán kính palindrome lẻ. 

Đối với mỗi vị trí`i`,`d1[i]`lưu trữ số lượng palindromes lẻ tập trung tại`i`. 

Ví dụ: nếu`d1[i] = 3`, thì các palindromes có bán kính`1,2,3`. 
2. Chạy thuật toán Manacher để tính bán kính palindrome chẵn. 

Đối với mỗi vị trí`i`,`d2[i]`lưu trữ số lượng các palindromes chẵn tập trung giữa`i-1`Và`i`. 
3. Tính tổng số palindromes. 

Mỗi bán kính đóng góp chính xác số lượng palindromes đó.```
total = sum(d1) + sum(d2)
```4. Xây dựng mảng`start[]`Và`end[]`.`start[i]`sẽ chứa số lượng palindromes bắt đầu từ vị trí`i`.`end[i]`sẽ chứa số lượng palindromes kết thúc ở vị trí`i`. 
5. Điền vào các mảng này bằng cách sử dụng các cập nhật chênh lệch. 

Đối với các palindromes lẻ tập trung tại`i`với bán kính`k`: 

- bắt đầu từ`i-k+1`ĐẾN`i`- kết thúc nằm trong khoảng từ`i`ĐẾN`i+k-1`Thay vì thêm từng palindrome riêng biệt, chúng ta thêm`+1`qua các khoảng thời gian này bằng cách sử dụng các mảng khác nhau. 
6. Lặp lại quá trình tương tự cho các bảng màu chẵn. 

Nếu một bảng màu chẵn có tâm`i`và bán kính`k`: 

- bắt đầu từ`i-k`ĐẾN`i-1`- kết thúc nằm trong khoảng từ`i`ĐẾN`i+k-1`7. Chuyển đổi các mảng chênh lệch thành số đếm thực tế bằng cách sử dụng tổng tiền tố. 
8. Xây dựng tổng tiền tố`end[]`. 

Cho phép:```
pref[i] = number of palindromes ending at or before i
```9. Đếm các cặp rời rạc. 

Đối với mọi vị trí xuất phát`i`, tất cả các palindromes bắt đầu từ`i`rời rạc khỏi mọi palindrome kết thúc trước đó`i`. 

Vì vậy, chúng tôi thêm:```
start[i] * pref[i-1]
```10. Tính đáp án cuối cùng. 

Tổng số cặp không có thứ tự:```
total * (total - 1) / 2
```Trừ các cặp rời rạc và lấy modulo. 

### Tại sao nó hoạt động 

Mỗi cặp palindrome riêng biệt thuộc đúng một trong hai loại: 

- giao nhau, 
- rời rạc. 

Một cặp bị tách rời chính xác khi bảng màu bên trái kết thúc trước khi bảng màu bên phải bắt đầu. Bằng cách cố định vị trí bắt đầu của palindrome bên phải, chúng ta có thể đếm tất cả các palindrome bên trái tương thích bằng cách sử dụng tổng tiền tố trên các vị trí kết thúc. 

Thuật toán của Manacher đảm bảo rằng mỗi palindrome được biểu diễn chính xác một lần qua tâm và bán kính của nó. Việc tái cấu trúc mảng sai phân bảo toàn bội số một cách chính xác, bởi vì mỗi bán kính hợp lệ đóng góp một bảng màu có điểm bắt đầu và kết thúc duy nhất. 

Vì mỗi cặp rời rạc được tính chính xác một lần, nên trừ đi tổng số cặp không có thứ tự, chỉ còn lại chính xác các cặp giao nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 51123987

def solve():
    n = int(input())
    s = input().strip()

    # Manacher odd
    d1 = [0] * n
    l = 0
    r = -1

    for i in range(n):
        k = 1 if i > r else min(d1[l + r - i], r - i + 1)

        while i - k >= 0 and i + k < n and s[i - k] == s[i + k]:
            k += 1

        d1[i] = k

        if i + k - 1 > r:
            l = i - k + 1
            r = i + k - 1

    # Manacher even
    d2 = [0] * n
    l = 0
    r = -1

    for i in range(n):
        k = 0 if i > r else min(d2[l + r - i + 1], r - i + 1)

        while i - k - 1 >= 0 and i + k < n and s[i - k - 1] == s[i + k]:
            k += 1

        d2[i] = k

        if i + k - 1 > r:
            l = i - k
            r = i + k - 1

    total = (sum(d1) + sum(d2)) % MOD

    start_diff = [0] * (n + 1)
    end_diff = [0] * (n + 1)

    # Odd palindromes
    for i in range(n):
        k = d1[i]

        L = i - k + 1
        R = i

        start_diff[L] += 1
        if R + 1 < n:
            start_diff[R + 1] -= 1

        L = i
        R = i + k - 1

        end_diff[L] += 1
        if R + 1 < n:
            end_diff[R + 1] -= 1

    # Even palindromes
    for i in range(n):
        k = d2[i]

        if k == 0:
            continue

        L = i - k
        R = i - 1

        start_diff[L] += 1
        if R + 1 < n:
            start_diff[R + 1] -= 1

        L = i
        R = i + k - 1

        end_diff[L] += 1
        if R + 1 < n:
            end_diff[R + 1] -= 1

    start = [0] * n
    end = [0] * n

    cur = 0
    for i in range(n):
        cur += start_diff[i]
        start[i] = cur

    cur = 0
    for i in range(n):
        cur += end_diff[i]
        end[i] = cur

    pref = [0] * n
    pref[0] = end[0]

    for i in range(1, n):
        pref[i] = pref[i - 1] + end[i]

    disjoint = 0

    for i in range(1, n):
        disjoint += start[i] * pref[i - 1]
        disjoint %= MOD

    total_pairs = total * (total - 1) // 2
    total_pairs %= MOD

    ans = (total_pairs - disjoint) % MOD

    print(ans)

solve()
```Phần đầu tiên là thuật toán Manacher tiêu chuẩn. Phiên bản chẵn và lẻ khác nhau vì quy tắc đối xứng của chúng hơi khác nhau.`d1[i]`lưu trữ số lượng, không phải độ dài bán kính trong ký tự. Nếu như`d1[i] = 3`, độ dài palindrom thực tế là`1,3,5`. 

Phần mảng khác biệt là phần tinh tế. 

Giả định`d1[i] = k`. Thế thì có chính xác`k`các palindromes kỳ lạ tập trung ở`i`. Vị trí bắt đầu của họ tạo thành một khoảng thời gian liên tục. Thay vì lặp lại trên mọi bán kính, chúng tôi tăng toàn bộ khoảng đó theo`O(1)`thời gian. 

Logic tương tự áp dụng cho các vị trí kết thúc. 

Sau khi xây dựng lại`start[]`Và`end[]`, chúng tôi đếm các cặp rời rạc. Đối với mọi vị trí`i`, mọi palindrome bắt đầu từ đó có thể ghép nối với mọi palindrome kết thúc trước đó`i`. 

Thứ tự quan trọng ở đây. Chúng tôi chỉ đếm các cặp mà bảng màu bên trái kết thúc sớm hơn, vì vậy mỗi cặp rời rạc không có thứ tự được tính chính xác một lần. 

Một chi tiết quan trọng khác là tăng trưởng số nguyên. Tổng số palindromes có thể vào khoảng`2 * 10^12`, vì vậy các ngôn ngữ có số nguyên có chiều rộng cố định sẽ tràn. Số nguyên Python là an toàn, nhưng chúng tôi vẫn giảm modulo thường xuyên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
babb
```Kết quả của người quản lý: 

| Vị trí | Nhân vật | d1 | d2 | 
| --- | --- | --- | --- | 
| 0 | b | 1 | 0 | 
| 1 | một | 2 | 0 | 
| 2 | b | 1 | 0 | 
| 3 | b | 1 | 1 | 

Tổng số palindrome: 

| Palindrom | Khoảng thời gian | 
| --- | --- | 
| b | [1,1] | 
| em yêu | [1,3] | 
| một | [2,2] | 
| b | [3,3] | 
| bb | [3,4] | 
| b | [4,4] | 

Bây giờ tính toán bắt đầu và kết thúc. 

| Vị trí | bắt đầu | kết thúc | trước | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 1 | 
| 2 | 1 | 1 | 2 | 
| 3 | 2 | 2 | 4 | 
| 4 | 1 | 2 | 6 | 

Các cặp rời rạc: 

- bắt đầu lúc 2h:`1 * 1 = 1`- bắt đầu lúc 3h:`2 * 2 = 4`- bắt đầu lúc 4h:`1 * 4 = 4`Tổng số rời rạc =`9`. 

Tổng số palindrome =`6`. 

Tổng số cặp không có thứ tự =`15`. 

Trả lời =`15 - 9 = 6`. 

Dấu vết này cho thấy tại sao việc đếm các cặp rời rạc lại đơn giản hơn việc kiểm tra chồng chéo trực tiếp. 

### Ví dụ 2 

đầu vào:```
5
aaaaa
```Mỗi chuỗi con là một palindrome. 

| Chiều dài | Đếm | 
| --- | --- | 
| 1 | 5 | 
| 2 | 4 | 
| 3 | 3 | 
| 4 | 2 | 
| 5 | 1 | 

Tổng số palindrome =`15`. 

| Vị trí | bắt đầu | kết thúc | 
| --- | --- | --- | 
| 1 | 5 | 1 | 
| 2 | 4 | 2 | 
| 3 | 3 | 3 | 
| 4 | 2 | 4 | 
| 5 | 1 | 5 | 

Các cặp rời rạc: 

- vị trí 2:`4 * 1 = 4`- vị trí 3:`3 * 3 = 9`- vị trí 4:`2 * 6 = 12`- vị trí 5:`1 * 10 = 10`Tổng số rời rạc =`35`. 

Tổng số cặp =`105`. 

Cặp giao nhau =`70`. 

Ví dụ này nhấn mạnh mật độ trường hợp xấu nhất của palindromes. Thuật toán vẫn tuyến tính vì nó không bao giờ cụ thể hóa tất cả các khoảng một cách rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Trình quản lý, cập nhật khác biệt và quét tiền tố đều là tuyến tính | 
| Không gian | O(n) | Mảng cho bán kính, số đếm và tổng tiền tố | 

Với`n = 2 * 10^6`, độ phức tạp tuyến tính là cần thiết. Giải pháp chỉ thực hiện một số lần truyền không đổi trên chuỗi và lưu trữ một vài mảng số nguyên có kích thước`n`, vừa vặn thoải mái trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 51123987

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())
    s = input().strip()

    d1 = [0] * n
    l = 0
    r = -1

    for i in range(n):
        k = 1 if i > r else min(d1[l + r - i], r - i + 1)

        while i - k >= 0 and i + k < n and s[i - k] == s[i + k]:
            k += 1

        d1[i] = k

        if i + k - 1 > r:
            l = i - k + 1
            r = i + k - 1

    d2 = [0] * n
    l = 0
    r = -1

    for i in range(n):
        k = 0 if i > r else min(d2[l + r - i + 1], r - i + 1)

        while i - k - 1 >= 0 and i + k < n and s[i - k - 1] == s[i + k]:
            k += 1

        d2[i] = k

        if i + k - 1 > r:
            l = i - k
            r = i + k - 1

    total = sum(d1) + sum(d2)

    start_diff = [0] * (n + 1)
    end_diff = [0] * (n + 1)

    for i in range(n):
        k = d1[i]

        start_diff[i - k + 1] += 1
        if i + 1 < n:
            start_diff[i + 1] -= 1

        end_diff[i] += 1
        if i + k < n:
            end_diff[i + k] -= 1

    for i in range(n):
        k = d2[i]

        if k == 0:
            continue

        start_diff[i - k] += 1
        if i < n:
            start_diff[i] -= 1

        end_diff[i] += 1
        if i + k < n:
            end_diff[i + k] -= 1

    start = [0] * n
    end = [0] * n

    cur = 0
    for i in range(n):
        cur += start_diff[i]
        start[i] = cur

    cur = 0
    for i in range(n):
        cur += end_diff[i]
        end[i] = cur

    pref = [0] * n
    pref[0] = end[0]

    for i in range(1, n):
        pref[i] = pref[i - 1] + end[i]

    disjoint = 0

    for i in range(1, n):
        disjoint += start[i] * pref[i - 1]

    ans = total * (total - 1) // 2 - disjoint

    return str(ans % MOD) + "\n"

# provided sample
assert run("4\nbabb\n") == "6\n", "sample 1"

# minimum size
assert run("1\na\n") == "0\n", "single character"

# all equal
assert run("2\naa\n") == "2\n", "all substrings palindromic"

# odd and even palindromes together
assert run("4\nabba\n") == "7\n", "mixed palindrome types"

# no palindrome longer than 1
assert run("5\nabcde\n") == "0\n", "all palindromes disjoint"

# repeated characters
assert run("5\naaaaa\n") == "70\n", "dense palindrome structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / a`|`0`| Không tự đếm cặp | 
|`2 / aa`|`2`| Xử lý bảng màu chẵn cơ bản | 
|`4 / abba`|`7`| Tương tác dài thậm chí palindrome với người độc thân | 
|`5 / abcde`|`0`| Các bảng chữ cái đơn hoàn toàn rời rạc | 
|`5 / aaaaa`|`70`| Mật độ palindrome trong trường hợp xấu nhất | 

## Vỏ cạnh 

Hãy xem xét chuỗi nhỏ nhất có thể:```
1
a
```Manacher đưa ra: 

-`d1 = [1]`-`d2 = [0]`Có chính xác một palindrome. Tổng số cặp không có thứ tự bằng 0 vì:```
1 * 0 / 2 = 0
```Thuật toán không bao giờ cố gắng ghép một palindrome với chính nó vì tất cả việc đếm cặp đều dựa trên các khoảng riêng biệt. 

Bây giờ hãy xem xét:```
2
aa
```Palindrome: 

-`[1,1]`-`[2,2]`-`[1,2]`Cặp rời rạc duy nhất là không có cặp nào, bởi vì mọi cặp đều trùng nhau. Tổng số cặp là`3`. Sau khi trừ đi các cặp rời rạc bằng 0, câu trả lời trở thành`2`bởi vì các cặp palindrome riêng biệt không có thứ tự là: 

-`[1,1]`với`[1,2]`-`[2,2]`với`[1,2]`Thuật toán xử lý bảng màu chẵn một cách chính xác thông qua`d2`. 

Bây giờ hãy kiểm tra:```
5
abcde
```Chỉ tồn tại các palindrome một ký tự. 

Mỗi palindrome bắt đầu và kết thúc ở vị trí riêng của nó. Vì không có khoảng chồng lên nhau nên mọi cặp đều rời rạc. 

Thuật toán tính toán:```
total = 5
total pairs = 10
disjoint = 10
answer = 0
```Điều này xác nhận việc đếm rời rạc dựa trên tiền tố hoạt động chính xác khi tất cả các bảng màu được cách ly. 

Cuối cùng, trường hợp dày đặc:```
5
aaaaa
```Mỗi chuỗi con là một palindrome. Số lượng palindrome là bậc hai, nhưng Manacher vẫn lưu trữ mọi thứ một cách gọn gàng trong không gian tuyến tính: 

- bán kính lẻ mã hóa tất cả các bảng màu lẻ, 
- bán kính chẵn mã hóa tất cả các bảng màu chẵn. 

Các bản cập nhật mảng khác biệt tổng hợp số lượng lớn các khoảng mà không lặp lại chúng một cách riêng lẻ, đó chính xác là lý do tại sao giải pháp vẫn tuyến tính ngay cả trong trường hợp xấu nhất.
