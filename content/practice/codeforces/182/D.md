---
title: "CF 182D - Ước chung"
description: "Chúng tôi được cung cấp hai chuỗi chữ thường. Chuỗi d được gọi là ước số của chuỗi s khác nếu s có thể được hình thành bằng cách nối d nhiều lần liên tiếp. Ví dụ: \"ab\" chia \"ababab\" vì lặp lại \"ab\" ba lần sẽ cho ra chuỗi đầy đủ."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "hashing", "implementation", "math", "strings"]
categories: ["algorithms"]
codeforces_contest: 182
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 117 (Div. 2)"
rating: 1400
weight: 182
solve_time_s: 96
verified: true
draft: false
---

[CF 182D - Ước chung](https://codeforces.com/problemset/problem/182/D) 

**Đánh giá:** 1400 
**Tags:** brute Force, băm, triển khai, toán học, chuỗi 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai chuỗi chữ thường. một chuỗi`d`được gọi là ước số của một chuỗi khác`s`nếu như`s`có thể được hình thành bằng cách nối`d`nhiều lần liên tiếp. 

Ví dụ,`"ab"`chia rẽ`"ababab"`bởi vì lặp đi lặp lại`"ab"`ba lần cho ra chuỗi đầy đủ. Mặt khác,`"aba"`không chia`"ababab"`bởi vì việc lặp lại nó không bao giờ tạo ra mục tiêu chính xác. 

Nhiệm vụ là đếm xem có bao nhiêu chuỗi chia cả hai chuỗi đầu vào cùng một lúc. 

Độ dài chuỗi tối đa là`10^5`, điều này ngay lập tức loại trừ việc tạo chuỗi con đắt tiền kết hợp với việc tái thiết lặp đi lặp lại. Bất cứ điều gì gần gũi`O(n^2)`công việc của ký tự có thể trở nên quá chậm khi cả hai chuỗi đều lớn. Một giải pháp thực hiện hàng chục tỷ thao tác sẽ không thể hoàn thành trong thời hạn. Chúng ta cần một cái gì đó gần tuyến tính hoặc`O(n log n)`. 

Phần khó khăn là tiền tố phù hợp là không đủ. Một ước số ứng viên phải lặp lại một cách hoàn hảo trên toàn bộ chuỗi. 

Hãy xem xét đầu vào này:```
ababab
abab
```Ước chung là`"ab"`, không`"abab"`. Việc triển khai bất cẩn chỉ có thể kiểm tra xem cả hai chuỗi có bắt đầu bằng cùng một tiền tố hay không và vô tình đếm`"abab"`mặc dù nó không thể xếp gạch`"ababab"`hoàn toàn. 

Một lỗi dễ xảy ra khác khi độ dài ứng cử viên không chia độ dài chuỗi đầy đủ.```
aaa
aa
```Ước chung duy nhất là`"a"`. 

A naive approach might treat`"aa"`hợp lệ vì cả hai chuỗi đều bắt đầu bằng`"aa"`, Nhưng`"aa"`không thể tạo ra`"aaa"`thông qua sự lặp lại. 

Chuỗi ký tự đơn cũng rất quan trọng:```
a
a
```Câu trả lời là`1`, bởi vì`"a"`chia cả hai chuỗi. 

Các mẫu khác nhau có cùng số lượng ký tự cũng có thể phá vỡ các giải pháp không chính xác:```
abab
abba
```Câu trả lời là`0`. 

Cả hai chuỗi đều chứa các chữ cái giống nhau, nhưng không có chuỗi cơ sở lặp lại nào tạo ra cả hai một cách chính xác. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là thử mọi chuỗi con có thể làm ước số ứng viên. Đối với mỗi ứng cử viên, chúng tôi kiểm tra xem việc lặp lại nó có tái tạo lại chính xác cả hai chuỗi hay không. 

Giả sử chuỗi đầu tiên có độ dài`n`và cái thứ hai có chiều dài`m`. Trong trường hợp xấu nhất, có`O(min(n, m))`độ dài của ứng cử viên. Đối với mỗi ứng cử viên, việc xây dựng lại và so sánh các chuỗi có chi phí lên tới`O(n + m)`. 

Điều đó mang lại sự phức tạp hoàn toàn xung quanh`O(min(n, m) * (n + m))`. 

Với chiều dài gần`10^5`, điều này trở nên quá đắt. Đầu vào trong trường hợp xấu nhất có thể yêu cầu khoảng`10^10`các thao tác của nhân vật. 

Quan sát quan trọng là độ dài ước số hợp lệ phải chia cả hai độ dài chuỗi. 

Nếu một chuỗi có độ dài`k`tạo ra một chuỗi có độ dài`n`, sau đó`k`phải chia`n`chính xác. Vì vậy, thay vì kiểm tra mọi độ dài có thể, chúng ta chỉ cần kiểm tra ước chung của hai độ dài. 

Ví dụ: nếu chuỗi có độ dài`8`Và`16`, độ dài ước số duy nhất có thể là ước số chung của`8`Và`16`, cụ thể là`1`,`2`,`4`, Và`8`. 

Điều đó làm giảm đáng kể số lượng ứng viên. Số lượng ước của một số nguyên nhỏ, thường nhiều nhất là vài trăm đối với các giá trị xung quanh`10^5`. 

Đối với mỗi độ dài hợp lệ`k`, chúng tôi lấy tiền tố có độ dài`k`từ chuỗi đầu tiên và xác minh rằng việc lặp lại nó sẽ tái tạo lại cả hai chuỗi. 

Bản thân việc xác minh có thể được thực hiện một cách hiệu quả bằng cách sử dụng sự lặp lại và so sánh bằng nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(min(n,m) × (n+m)) | O(n+m) | Quá chậm | 
| Tối ưu | O((n+m) × d) | O(n+m) | Đã chấp nhận | 

Đây`d`là số ước chung của độ dài chuỗi, rất nhỏ trong thực tế. 

## Hướng dẫn thuật toán 

1. Đọc hai chuỗi đầu vào. 
2. Tính độ dài của chúng`n`Và`m`. 
3. Lặp qua tất cả các số nguyên`k`từ`1`ĐẾN`min(n, m)`. 
4. Bỏ qua bất kỳ`k`điều đó không chia cả hai`n`Và`m`. 

Một khối có độ dài lặp lại`k`chỉ có thể tạo ra các chuỗi có độ dài là bội số của`k`. 
5. Lấy cái đầu tiên`k`các ký tự của chuỗi đầu tiên làm ước số ứng viên. 
6. Xây dựng lại chuỗi đầu tiên bằng cách lặp lại ứng viên`n // k`lần. 
7. Xây dựng lại chuỗi thứ hai bằng cách lặp lại ứng cử viên`m // k`lần. 
8. Nếu cả hai chuỗi được xây dựng lại khớp chính xác với chuỗi gốc, hãy tăng câu trả lời. 
9. In số đếm cuối cùng. 

### Tại sao nó hoạt động 

Mọi chuỗi ước số hợp lệ phải xuất hiện dưới dạng tiền tố của cả hai chuỗi, vì sự lặp lại luôn bắt đầu từ đầu. Độ dài của nó cũng phải chia chính xác cả độ dài chuỗi. 

Thuật toán kiểm tra mọi độ dài ước chung có thể có và xác minh xem tiền tố tương ứng có thực sự tạo ra cả hai chuỗi thông qua sự lặp lại hay không. Không thể bỏ qua ước số hợp lệ vì mọi ước số đều phải thỏa mãn các tính chất này. Không thể tính ước số không hợp lệ vì việc xây dựng lại đảm bảo sự bằng nhau chính xác với cả hai chuỗi gốc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s1 = input().strip()
    s2 = input().strip()

    n = len(s1)
    m = len(s2)

    ans = 0

    for k in range(1, min(n, m) + 1):
        if n % k != 0 or m % k != 0:
            continue

        candidate = s1[:k]

        if candidate * (n // k) == s1 and candidate * (m // k) == s2:
            ans += 1

    print(ans)

solve()
```Vòng lặp kiểm tra mọi độ dài ước số có thể. Điều kiện chia hết là cần thiết vì khối lặp không thể tạo ra chuỗi có độ dài không phải là bội số của độ dài khối. 

Ước số ứng cử viên luôn được lấy từ tiền tố của`s1`. Mọi ước số hợp lệ đều phải bắt đầu ở đầu chuỗi, vì vậy việc kiểm tra các chuỗi con tùy ý là không cần thiết. 

Bước tái thiết là nơi tính chính xác được thực thi. Ngay cả khi hai chuỗi có chung tiền tố, mẫu lặp lại có thể bị lỗi sau này trong chuỗi. Xây dựng chuỗi lặp lại và so sánh với chuỗi ban đầu sẽ nắm bắt rõ ràng các trường hợp này. 

Một chi tiết tinh tế đang sử dụng`.strip()`khi đọc đầu vào. Không có nó, ký tự dòng mới sẽ trở thành một phần của chuỗi và phá vỡ mọi so sánh. 

Một lỗi dễ mắc phải khác là quên kiểm tra cả hai chuỗi riêng biệt. Một ứng viên có thể chia`s1`đúng nhưng không thành công`s2`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
abcdabcd
abcdabcdabcdabcd
```| k | Chia cả hai chiều dài? | Ứng viên | Xây dựng lại s1 | Xây dựng lại s2 | Có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Có |`"a"`|`"aaaaaaaa"`|`"aaaaaaaaaaaaaaaa"`| Không | 
| 2 | Có |`"ab"`|`"abababab"`|`"abababababababab"`| Không | 
| 4 | Có |`"abcd"`|`"abcdabcd"`|`"abcdabcdabcdabcd"`| Có | 
| 8 | Có |`"abcdabcd"`|`"abcdabcd"`|`"abcdabcdabcdabcd"`| Có | 

Trả lời:`2`Dấu vết này cho thấy tại sao chỉ các tiền tố phù hợp là không đủ.`"ab"`xuất hiện ở đầu cả hai chuỗi nhưng không xếp chúng một cách chính xác. 

### Ví dụ 2 

đầu vào:```
aaa
aa
```| k | Chia cả hai chiều dài? | Ứng viên | Xây dựng lại s1 | Xây dựng lại s2 | Có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Có |`"a"`|`"aaa"`|`"aa"`| Có | 
| 2 | Không | - | - | - | Đã bỏ qua | 

Trả lời:`1`Ví dụ này chứng minh tại sao độ dài ước số lại quan trọng.`"aa"`không thể được xem xét vì chiều dài`2`không chia`3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+m) × d) |`d`là số độ dài ước chung được kiểm tra | 
| Không gian | O(n+m) | Chuỗi lặp lại tạm thời trong quá trình so sánh | 

Số lượng ước số cho các giá trị lên tới`10^5`rất nhỏ nên lời giải dễ dàng phù hợp trong thời gian giới hạn. Ngay cả với kích thước đầu vào tối đa, số lượng công việc lặp đi lặp lại vẫn có thể quản lý được. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        s1 = input().strip()
        s2 = input().strip()

        n = len(s1)
        m = len(s2)

        ans = 0

        for k in range(1, min(n, m) + 1):
            if n % k != 0 or m % k != 0:
                continue

            candidate = s1[:k]

            if candidate * (n // k) == s1 and candidate * (m // k) == s2:
                ans += 1

        return str(ans)

    return solve()

# provided sample
assert run("abcdabcd\nabcdabcdabcdabcd\n") == "2", "sample 1"

# minimum-size input
assert run("a\na\n") == "1", "single character strings"

# no common divisors
assert run("abab\nabba\n") == "0", "same letters, different structure"

# repeated same character
assert run("aaaaaa\naaa\n") == "2", "divisors are 'a' and 'aaa'"

# boundary length divisibility
assert run("aaa\naa\n") == "1", "length mismatch prevents 'aa'"

# identical strings
assert run("xyzxyz\nxyzxyz\n") == "2", "divisors are 'xyz' and full string"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a / a`|`1`| Đầu vào hợp lệ tối thiểu | 
|`abab / abba`|`0`| Tiền tố được chia sẻ là không đủ | 
|`aaaaaa / aaa`|`2`| Nhiều độ dài ước số hợp lệ | 
|`aaa / aa`|`1`| Điều kiện chia hết chiều dài | 
|`xyzxyz / xyzxyz`|`2`| Các chuỗi giống nhau có nhiều ước số | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
aaa
aa
```Thuật toán kiểm tra`k = 1`Và`k = 2`. 

Vì`k = 1`, ứng cử viên là`"a"`. Việc lặp lại nó sẽ tạo ra cả hai chuỗi gốc một cách chính xác nên nó được tính. 

Vì`k = 2`, thuật toán bỏ qua ngay lập tức vì`2`không chia`3`. 

Điều này ngăn chặn việc đưa vào không chính xác`"aa"`. 

Bây giờ hãy xem xét:```
abab
abba
```Độ dài ước chung có thể có là`1`,`2`, Và`4`. 

Vì`k = 1`, ứng cử viên là`"a"`. Lặp đi lặp lại nó mang lại`"aaaa"`Và`"aaaa"`, thất bại. 

Vì`k = 2`, ứng cử viên là`"ab"`. Lặp đi lặp lại nó mang lại`"abab"`Và`"abab"`. Việc tái thiết thứ hai không khớp`"abba"`. 

Vì`k = 4`, ứng cử viên là`"abab"`. Lặp lại nó một lần mang lại`"abab"`cho chuỗi đầu tiên nhưng`"abab"`không khớp`"abba"`. 

Câu trả lời đúng sẽ trở thành`0`. 

Cuối cùng, hãy xem xét các chuỗi giống hệt nhau:```
abcabc
abcabc
```Thuật toán kiểm tra độ dài`1`,`2`,`3`, Và`6`. 

Chỉ một`"abc"`Và`"abcabc"`xây dựng lại cả hai chuỗi một cách chính xác. 

Điều này xác nhận rằng thuật toán đếm cả ước số nguyên thủy và chính chuỗi đầy đủ khi hợp lệ.
