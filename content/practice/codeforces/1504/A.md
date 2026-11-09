---
title: "CF 1504A - D\u00e9j\u00e0 Vũ"
description: "Chúng tôi được cung cấp một số chuỗi. Với mỗi chuỗi, chúng ta phải chèn chính xác một ký tự 'a' vào một vị trí nào đó. Chuỗi mới phải có độ dài Đầu vào chứa tối đa 10^4 trường hợp thử nghiệm và tổng của tất cả độ dài chuỗi tối đa là 3·10^5."
date: "2026-06-10T20:40:31+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "strings"]
categories: ["algorithms"]
codeforces_contest: 1504
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 712 (Div. 2)"
rating: 800
weight: 1504
solve_time_s: 358
verified: false
draft: false
---

[CF 1504A - D\u00e9j\u00e0 Vu](https://codeforces.com/problemset/problem/1504/A) 

**Đánh giá:** 800 
**Tas:** thuật toán xây dựng, chuỗi 
**Thời gian giải:** 5 phút 58 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số chuỗi. Với mỗi chuỗi, chúng ta phải chèn đúng một ký tự`'a'`ở một vị trí nào đó. Chuỗi mới phải có độ dài`|s|+1`, và nó không được là một bảng màu. Nếu mọi phép chèn có thể đều tạo ra một palindrome thì chúng ta phải báo cáo rằng không có nghiệm nào tồn tại. 

Đầu vào chứa tối đa`10^4`các trường hợp thử nghiệm và tổng độ dài của tất cả các chuỗi nhiều nhất là`3·10^5`. Vì tổng lượng dữ liệu vừa phải nên một thuật toán có thời gian chạy tuyến tính trên tổng chiều dài dễ dàng đủ nhanh. Công việc bậc hai trên mỗi chuỗi vẫn sẽ được áp dụng đối với các chuỗi ngắn, nhưng cách tiếp cận bậc ba sẽ trở thành chi phí không cần thiết. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Đầu tiên là một chuỗi bao gồm toàn bộ`'a'`. 

đầu vào:```
a
```Việc chèn duy nhất có thể tạo ra`"aa"`, vẫn là một palindrome. Câu trả lời đúng là:```
NO
```Một giải pháp bất cẩn luôn đặt ra một`'a'`ở phía trước sẽ xuất ra không chính xác`"aa"`. 

Một trường hợp khác là khi một vị trí chèn tạo ra một bảng màu nhưng vị trí khác lại hoạt động. 

đầu vào:```
ab
```Chèn vào cuối sẽ cho`"aba"`, là một bảng màu, nhưng việc chèn vào đầu sẽ cho`"aab"`, không phải vậy. Câu trả lời đúng là:```
YES
aab
```Giải pháp chỉ kiểm tra một vị trí có thể bỏ lỡ các công trình xây dựng hợp lệ. 

Cũng có những chuỗi đã là palindromes nhưng vẫn chấp nhận một câu trả lời hợp lệ. 

đầu vào:```
aba
```Thêm`'a'`ở phía trước cho`"aaba"`, không phải là một palindrome. Trở thành một palindrome ban đầu không có nghĩa là không thể. 

## Phương pháp tiếp cận 

Một phương pháp bạo lực đơn giản sẽ thử tất cả`n+1`các vị trí chèn. Đối với mỗi vị trí, nó xây dựng chuỗi kết quả và kiểm tra xem chuỗi đó có phải là một bảng màu hay không. Phương pháp này đúng vì mọi khả năng chèn đều được kiểm tra. 

Vấn đề là một chuỗi có độ dài`n`có`n+1`vị trí ứng cử viên và kiểm tra xem ứng cử viên có phải là một palindrome hay không`O(n)`thời gian. Việc xây dựng chuỗi cũng tốn kém`O(n)`. Tổng độ phức tạp trở thành`O(n²)`. 

Với những ràng buộc đã cho, ngay cả thời gian bậc hai cũng được chấp nhận trong thực tế vì tổng độ dài chỉ`3·10^5`. Tuy nhiên, vấn đề có cấu trúc đơn giản hơn nhiều. 

Quan sát quan trọng là nếu chuỗi gốc chứa một số ký tự không phải là`'a'`, thì một trong hai công trình phải hoạt động. 

Chúng ta có thể trả trước`'a'`, sản xuất```
a + s
```hoặc nối thêm`'a'`, sản xuất```
s + a
```Giả sử cả hai đều là palindromes. Từ```
a+s
```là một palindrome, việc loại bỏ các ký tự đầu tiên và cuối cùng của nó cho thấy phần còn lại của chuỗi phải đối xứng. Làm tương tự với```
s+a
```dẫn đến cùng một kết luận. Sự kết hợp những điều kiện này buộc mọi đặc tính của`s`được`'a'`. 

Vì vậy, bất cứ khi nào chuỗi chứa ít nhất một chuỗi không phải`'a'`ít nhất một trong hai cách xây dựng này được đảm bảo là không phải palindromic. Chúng ta chỉ cần kiểm tra hai khả năng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc một chuỗi`s`. 
2. Xây dựng chuỗi thu được bằng cách thêm`'a'`ở phía trước. 
3. Kiểm tra xem chuỗi mới này có phải là chuỗi palindrome hay không. Nếu không thì xuất ra`"YES"`và chuỗi này. 
4. Mặt khác, xây dựng chuỗi thu được bằng cách thêm`'a'`ở cuối. 
5. Kiểm tra xem chuỗi thứ hai này có phải là chuỗi palindrome hay không. Nếu không thì xuất ra`"YES"`và chuỗi này. 
6. Nếu cả hai ứng cử viên đều là palindromes, xuất ra`"NO"`. 

Lý do chỉ có hai vị trí được kiểm tra là vì bất kỳ chuỗi nào chứa ký tự khác với`'a'`phải làm cho ít nhất một trong số các ứng cử viên này không phải là palindromic. 

### Tại sao nó hoạt động 

Thuật toán kiểm tra rõ ràng hai chuỗi ứng viên`a+s`Và`s+a`. Nếu một trong hai câu trả lời không phải là palindrome thì đó là câu trả lời hợp lệ. 

Giả sử cả hai ứng cử viên đều là palindromes. TRONG`a+s`, ký tự đầu tiên và ký tự cuối cùng bằng nhau nên ký tự cuối cùng của`s`phải là`'a'`. Việc loại bỏ các ký tự trùng khớp này sẽ để lại một bảng màu. Việc lặp lại lập luận này cho thấy rằng mọi ký tự của`s`là`'a'`. Đối với một chuỗi như vậy, mỗi lần chèn`'a'`tạo ra một thứ khác-`'a'`chuỗi luôn luôn là một chuỗi palindrome. Do đó không có giải pháp tồn tại. 

Vì thuật toán đưa ra một chuỗi hợp lệ bất cứ khi nào chuỗi đó tồn tại và báo cáo`"NO"`chính xác khi không có giải pháp tồn tại, nó là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_palindrome(s):
    return s == s[::-1]

t = int(input())
ans = []

for _ in range(t):
    s = input().strip()

    front = 'a' + s
    if not is_palindrome(front):
        ans.append("YES")
        ans.append(front)
        continue

    back = s + 'a'
    if not is_palindrome(back):
        ans.append("YES")
        ans.append(back)
    else:
        ans.append("NO")

print("\n".join(ans))
```Hàm trợ giúp kiểm tra xem một chuỗi có bằng chuỗi đảo ngược của nó hay không. Việc cắt Python làm cho thao tác này trở nên ngắn gọn và chạy theo thời gian tuyến tính. 

Đối với mỗi trường hợp thử nghiệm, trước tiên mã sẽ thử đặt`'a'`ở phía trước. Nếu chuỗi đó không phải là chuỗi palindrome, nó sẽ ngay lập tức ghi lại câu trả lời và chuyển sang trường hợp kiểm thử tiếp theo. 

Nếu lần thử đầu tiên không thành công, mã sẽ thử đặt`'a'`ở cuối. Chỉ khi cả hai ứng cử viên đều là palindromes thì nó mới in`"NO"`. 

Thứ tự quan trọng vì một khi tìm thấy một công trình hợp lệ, không có lý do gì để kiểm tra bất cứ điều gì khác. 

Điều kiện biên rất đơn giản. Các chuỗi có độ dài một được xử lý một cách tự nhiên. Ví dụ,`"a"`dẫn đến`"aa"`trong cả hai lần thử, vì vậy câu trả lời trở thành`"NO"`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chuỗi đầu vào:```
ab
```| Bước | Ứng viên | Xuôi ngược đều giống nhau? | Hành động | 
| --- | --- | --- | --- | 
| 1 |`"aab"`| Không | Câu trả lời đầu ra | 

Thuật toán dừng ngay lập tức và in:```
YES
aab
```Ví dụ này cho thấy ứng viên đầu tiên có thể đã làm việc, mặc dù việc thêm`'a'`nơi khác có thể tạo ra một bảng màu. 

### Ví dụ 2 

Chuỗi đầu vào:```
zza
```| Bước | Ứng viên | Xuôi ngược đều giống nhau? | Hành động | 
| --- | --- | --- | --- | 
| 1 |`"azza"`| Có | Tiếp tục | 
| 2 |`"zzaa"`| Không | Câu trả lời đầu ra | 

Đầu ra trở thành:```
YES
zzaa
```Ví dụ này chứng minh tại sao cả hai đầu phải được kiểm tra. Một vị trí chèn có thể thất bại trong khi vị trí chèn kia thành công. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Hai kiểm tra palindrome, mỗi kiểm tra tuyến tính | 
| Không gian | O(n) | Chuỗi tạm thời và bản sao đảo ngược | 

Đây`n`biểu thị độ dài của một chuỗi. Vì tổng của tất cả các độ dài lớn nhất`3·10^5`, tổng khối lượng công việc là tuyến tính theo kích thước đầu vào và vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def is_palindrome(s):
        return s == s[::-1]

    t = int(input())
    ans = []

    for _ in range(t):
        s = input().strip()

        front = 'a' + s
        if not is_palindrome(front):
            ans.append("YES")
            ans.append(front)
            continue

        back = s + 'a'
        if not is_palindrome(back):
            ans.append("YES")
            ans.append(back)
        else:
            ans.append("NO")

    return "\n".join(ans)

# provided sample
assert run(
"""6
cbabc
ab
zza
ba
a
nutforajaroftuna
"""
) == (
"""YES
acbabc
YES
aab
YES
zzaa
YES
aba
NO
YES
anutforajaroftuna"""
)

# minimum size
assert run(
"""1
a
"""
) == "NO", "single a"

# single non-a character
assert run(
"""1
b
"""
) == "YES\nab", "length one, solvable"

# all equal characters
assert run(
"""1
aaaa
"""
) == "NO", "all characters are a"

# off-by-one case
assert run(
"""1
zza
"""
) == "YES\nzzaa", "front insertion fails, back works"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`NO`| Kích thước tối thiểu và trường hợp không thể | 
|`b`|`YES ab`| Chiều dài một với một câu trả lời hợp lệ | 
|`aaaa`|`NO`| Tất cả các ký tự bằng`'a'`| 
|`zza`|`YES zzaa`| Một đầu thất bại, đầu kia thành công | 

## Vỏ cạnh 

Xem xét đầu vào```
1
a
```Các dạng thuật toán`"aa"`bằng cách chuẩn bị trước và`"aa"`bằng cách thêm vào. Cả hai đều là palindromes, vì vậy nó xuất ra:```
NO
```Điều này phù hợp với thực tế là mỗi lần chèn đều tạo ra cùng một chuỗi. 

Bây giờ hãy xem xét```
1
ab
```Ứng cử viên đầu tiên là`"aab"`.```
aab ≠ baa
```vì vậy nó không phải là một palindrome. Thuật toán xuất ra ngay lập tức```
YES
aab
```Không có công việc bổ sung được yêu cầu. 

Cuối cùng, hãy xem xét```
1
aba
```Các ứng cử viên là:```
aaba
abaa
```Cả hai chuỗi đều không bằng chuỗi đảo ngược của nó, vì vậy chuỗi đầu tiên được chấp nhận:```
YES
aaba
```Điều này xác nhận rằng một palindrome ban đầu vẫn có thể có cấu trúc hợp lệ.
