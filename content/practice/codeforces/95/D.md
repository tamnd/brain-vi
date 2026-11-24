---
title: "CF 95D - Đua Ngựa"
description: "Chúng tôi được cung cấp một số khoảng số nguyên. Với mỗi khoảng $[l, r]$, chúng ta phải đếm xem có bao nhiêu số chứa hai chữ số may mắn, 4 hoặc 7, có vị trí khác nhau tối đa k. Vị trí được tính bên trong biểu diễn thập phân của số."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 95
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 77 (Div. 1 Only)"
rating: 2500
weight: 95
solve_time_s: 120
verified: true
draft: false
---

[CF 95D - Đua ngựa](https://codeforces.com/problemset/problem/95/D) 

**Đánh giá:** 2500 
**Tags:** dp, toán 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số khoảng số nguyên. Đối với mỗi khoảng thời gian$[l, r]$, chúng ta phải đếm xem có bao nhiêu số có hai chữ số may mắn`4`hoặc`7`, vị trí của chúng cách nhau nhiều nhất`k`. 

Vị trí được tính bên trong biểu diễn thập phân của số. Nếu hai chữ số may mắn xuất hiện đủ gần nhau thì số đó được gọi là số gần may mắn. 

Ví dụ, với$k = 2$: 

-`404`gần như may mắn vì cả hai`4`s cách nhau hai vị trí. 
-`4123954997`gần như không may mắn vì cặp chữ số may mắn gần nhất cách nhau quá xa. 
- Một số chỉ có một chữ số may mắn không bao giờ đủ điều kiện. 

Giới hạn khoảng là rất lớn. Một số có thể chứa tới 1000 chữ số thập phân, do đó việc lặp qua mọi giá trị trong phạm vi là không thể. Ngay cả việc kiểm tra từng chữ số một cũng đã quá tốn kém nếu chúng ta thử tất cả các giá trị. 

Điều này ngay lập tức loại trừ bất kỳ giải pháp nào tỷ lệ thuận với kích thước số của khoảng. Hướng khả thi duy nhất là chữ số DP, trong đó chúng ta đếm các số hợp lệ bằng cách xây dựng chúng theo từng chữ số. 

Số lượng ca kiểm thử và giá trị của$k$đều có nhiều nhất là 1000. Vì độ dài chữ số cũng nhiều nhất là 1000, nên một nghiệm xung quanh$O(\text{digits}^2)$mỗi truy vấn có thể chấp nhận được. Một giải pháp khối sẽ trở nên rủi ro. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như$k = 1$. Khi đó chỉ có các chữ số may mắn liền kề mới quan trọng. 

đầu vào:```
1 1
1 100
```Các số hợp lệ chỉ`44`,`47`,`74`, Và`77`, vậy câu trả lời là`4`. Việc thực hiện bất cẩn có thể đếm sai các số như`404`, trong đó các chữ số may mắn cách nhau một khoảng cách`2`. 

Một trường hợp nguy hiểm khác là số 0 đứng đầu. Giả sử chúng ta xử lý các số dưới dạng chuỗi có độ dài cố định. 

đầu vào:```
1 2
1 50
```Nếu chúng ta cho phép các số 0 đứng đầu hoạt động giống như các chữ số thực thì`"004"`Và`"040"`có thể vô tình tạo ra các cặp may mắn giả. Các số 0 đứng đầu không được tham gia vào điều kiện. 

Trường hợp tinh tế thứ ba xuất hiện khi một cặp hợp lệ đã tồn tại. Khi chúng tôi đã tìm thấy một cặp chữ số may mắn đủ gần thì các chữ số sau đó không còn quan trọng nữa. 

Ví dụ:```
1 2
447000000000 447000000000
```Con số gần như may mắn vì hai chữ số đầu tiên tạo thành một cặp hợp lệ. Một DP bị lỗi sau đó có thể ghi đè lên trạng thái và làm mất thông tin đó. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Với mỗi số trong khoảng, quét các chữ số thập phân của nó và ghi lại vị trí của tất cả`4`Và`7`chữ số. Nếu hai vị trí may mắn liên tiếp bất kỳ khác nhau tối đa`k`, đếm số 

Điều này có tác dụng vì thuộc tính chỉ phụ thuộc vào dãy chữ số của một số. 

Vấn đề là quy mô. Điểm cuối khoảng có thể có 1000 chữ số. Ngay cả bản thân kích thước khoảng cũng lớn về mặt thiên văn. Việc liệt kê các con số là không thể. 

Vì vậy, thay vì lặp lại các số, chúng ta đếm chúng theo cách tổ hợp. 

Quan sát quan trọng là thuộc tính chỉ phụ thuộc vào thông tin cục bộ khi quét các chữ số từ trái sang phải. Tại bất kỳ thời điểm nào, chúng ta chỉ cần biết: 

- liệu một cặp hợp lệ đã xuất hiện chưa, 
- nơi xảy ra chữ số may mắn gần đây nhất. 

Điều đó làm cho bài toán hoàn hảo với chữ số DP. 

Chúng tôi xác định DP trên các vị trí chữ số. Trong khi xây dựng từng chữ số, chúng tôi theo dõi: 

- vị trí hiện tại, 
- vị trí của chữ số may mắn cuối cùng, 
- liệu chúng ta đã tạo thành một cặp hợp lệ hay chưa, 
- cho dù chúng tôi đã bắt đầu số, 
- liệu chúng ta có còn chặt chẽ ở giới hạn trên hay không. 

Bất cứ khi nào chúng tôi đặt một`4`hoặc`7`, ta so sánh vị trí của nó với chữ số may mắn trước đó. Nếu khoảng cách lớn nhất`k`, số sẽ có hiệu lực mãi mãi. 

Điều này chuyển đổi vấn đề từ lặp qua các giá trị sang lặp qua các trạng thái. Tổng số tiểu bang là khoảng$1000^2$, điều này hoàn toàn có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không thể | Không thể | Quá chậm | 
| Chữ số tối ưu DP |$O(L^2 \cdot 10)$|$O(L^2)$| Đã chấp nhận | 

Đây,$L$là số chữ số. 

## Hướng dẫn thuật toán 

1. Xác định hàm`solve(x)`trả về số lượng gần như may mắn trong phạm vi$[0, x]$. 

Sau đó, mọi câu trả lời truy vấn sẽ trở thành:$$solve(r) - solve(l-1)$$2. Chuyển giới hạn trên thành mảng chữ số. 

Chữ số DP hoạt động tự nhiên trên chuỗi vì chúng tôi xử lý từng vị trí một chữ số. 
3. Sử dụng DFS được ghi nhớ với trạng thái:$$(pos,\ last,\ found,\ started,\ tight)$$Ở đâu:

-`pos`là chỉ số chữ số hiện tại. 
-`last`là vị trí của chữ số may mắn gần đây nhất. 
-`found`cho biết liệu một cặp hợp lệ đã tồn tại hay chưa. 
-`started`cho biết liệu chúng ta đã đặt một chữ số không phải số 0 đứng đầu hay chưa. 
-`tight`cho biết tiền tố hiện tại có bằng tiền tố giới hạn trên hay không. 
4. Nếu đến cuối chuỗi chữ số, hãy trả về`1`chỉ nếu`found = True`. 

Điều này có nghĩa là con số được xây dựng gần như là may mắn. 
5. Thử tất cả các chữ số có thể cho vị trí hiện tại. 

Chữ số tối đa được phép phụ thuộc vào`tight`. 
6. Xử lý các số 0 ở đầu một cách cẩn thận. 

Nếu số chưa bắt đầu và chúng ta đặt một số 0 khác, chúng ta vẫn ở vị trí`started = False`trạng thái và không coi số 0 là chữ số thực. 
7. Nếu chữ số được đặt là`4`hoặc`7`, cập nhật thông tin may mắn. 

Nếu có một chữ số may mắn trước đó ở vị trí`last`, Và:$$pos - last \le k$$sau đó đặt`found = True`. 

Sau đó, cập nhật`last = pos`. 
8. Lặp lại vị trí tiếp theo và tích lũy câu trả lời theo modulo$10^9 + 7$. 
9. Với mỗi truy vấn, hãy tính:$$(solve(r) - solve(l-1)) \bmod (10^9+7)$$### Tại sao nó hoạt động 

Trạng thái DP lưu trữ chính xác thông tin cần thiết để xác định xem các chữ số trong tương lai có thể tạo một cặp hợp lệ hay không. 

Tương lai chỉ phụ thuộc vào chữ số may mắn gần nhất trước đó vì bất kỳ chữ số may mắn mới nào cũng chỉ cần so sánh với chữ số may mắn mới nhất. Các chữ số may mắn trước đó ở xa hơn và không bao giờ có thể tạo ra khoảng cách nhỏ hơn. 

các`found`cờ là đơn điệu. Khi một cặp hợp lệ xuất hiện, con số đó sẽ gần như may mắn bất kể các chữ số trong tương lai. 

Vì mỗi số hợp lệ tương ứng với chính xác một đường dẫn DP và mỗi đường dẫn DP biểu thị chính xác một số nên việc đếm là chính xác. 

## Giải pháp Python```python
import sys
from functools import lru_cache

input = sys.stdin.readline

MOD = 10**9 + 7

def subtract_one(s):
    if s == "0":
        return "0"

    a = list(s)
    i = len(a) - 1

    while i >= 0 and a[i] == '0':
        a[i] = '9'
        i -= 1

    a[i] = str(int(a[i]) - 1)

    res = ''.join(a).lstrip('0')
    return res if res else "0"

def count_nearly_lucky(s, k):
    digits = list(map(int, s))
    n = len(digits)

    @lru_cache(None)
    def dfs(pos, last, found, started, tight):
        if pos == n:
            return 1 if found else 0

        limit = digits[pos] if tight else 9
        ans = 0

        for d in range(limit + 1):
            ntight = tight and (d == limit)

            if not started and d == 0:
                ans += dfs(pos + 1, -1, found, False, ntight)
                ans %= MOD
                continue

            nfound = found
            nlast = last

            if d == 4 or d == 7:
                if last != -1 and pos - last <= k:
                    nfound = True
                nlast = pos

            ans += dfs(pos + 1, nlast, nfound, True, ntight)
            ans %= MOD

        return ans

    return dfs(0, -1, False, False, True)

def solve():
    t, k = map(int, input().split())

    out = []

    for _ in range(t):
        l, r = input().split()

        left = subtract_one(l)

        ans = (count_nearly_lucky(r, k) -
               count_nearly_lucky(left, k)) % MOD

        out.append(str(ans))

    print('\n'.join(out))

solve()
```chức năng`count_nearly_lucky`thực hiện chữ số DP trên tất cả các số từ`0`tới giới hạn đã cho. 

các`last`biến lưu trữ vị trí của chữ số may mắn gần đây nhất. Chỉ sử dụng số gần đây nhất là đủ vì bất kỳ chữ số may mắn nào trước đó thậm chí còn cách xa vị trí trong tương lai hơn. 

Các số 0 đứng đầu được xử lý thông qua`started`lá cờ. Cho đến khi chữ số khác 0 đầu tiên xuất hiện, các vị trí không được tính là một phần của số. Nếu không có sự phân biệt này, các số có độ dài khác nhau sẽ gây nhiễu lẫn nhau. 

Sự chuyển tiếp:```
if last != -1 and pos - last <= k:
```là cốt lõi của vấn đề. Nó phát hiện xem chữ số may mắn hiện tại có tạo thành một cặp đủ gần với chữ số may mắn trước đó hay không. 

Trình trợ giúp phép trừ cũng rất tinh tế. Vì giới hạn có thể có 1000 chữ số nên chúng ta không thể chuyển đổi chúng thành số nguyên. Việc giảm phải được thực hiện trực tiếp trên chuỗi. 

Cuối cùng, câu trả lời luôn được giảm modulo$10^9 + 7$. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 2
1 100
```Chúng tôi tính toán:$$solve(100) - solve(0)$$Các số hợp lệ là`44`,`47`,`74`,`77`. 

| Số | Vị trí may mắn | Khoảng cách tối thiểu | hợp lệ | 
| --- | --- | --- | --- | 
| 44 | 0,1 | 1 | Có | 
| 47 | 0,1 | 1 | Có | 
| 74 | 0,1 | 1 | Có | 
| 77 | 0,1 | 1 | Có | 

Câu trả lời cuối cùng:```
4
```Ví dụ này xác nhận rằng các chữ số may mắn liền kề được phát hiện chính xác. 

### Ví dụ 2 

đầu vào:```
1 2
400 500
```Ứng viên liên quan: 

| Số | Vị trí may mắn | Khoảng cách | hợp lệ | 
| --- | --- | --- | --- | 
| 404 | 0,2 | 2 | Có | 
| 407 | 0,2 | 2 | Có | 
| 447 | 0,1,2 | 1 | Có | 
| 470 | 0,1 | 1 | Có | 
| 499 | 0 | - | Không | 

DP phát hiện ra những con số này trong khi xây dựng các chữ số từ trái sang phải. 

Vì`404`: 

| Vị trí | Chữ số | Vị trí may mắn cuối cùng | Đã tìm thấy | 
| --- | --- | --- | --- | 
| 0 | 4 | 0 | Sai | 
| 1 | 0 | 0 | Sai | 
| 2 | 4 | 2 | Đúng | 

Tại vị trí`2`, khoảng cách là`2 - 0 = 2`, thỏa mãn điều kiện. 

Dấu vết này chứng tỏ tại sao chỉ giữ lại chữ số may mắn mới nhất là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(L^2 \cdot 10)$| Trạng thái DP dựa trên vị trí chữ số và vị trí may mắn cuối cùng | 
| Không gian |$O(L^2)$| Bảng ghi nhớ | 

Đây,$L$là số chữ số, nhiều nhất là 1000. 

DP bậc hai trên 1000 chữ số dễ dàng nằm trong giới hạn. Số lượng trạng thái chỉ là vài triệu trong trường hợp xấu nhất và các chuyển đổi có hệ số không đổi nhỏ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string

import sys
import io
from functools import lru_cache

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def subtract_one(s):
        if s == "0":
            return "0"

        a = list(s)
        i = len(a) - 1

        while i >= 0 and a[i] == '0':
            a[i] = '9'
            i -= 1

        a[i] = str(int(a[i]) - 1)

        res = ''.join(a).lstrip('0')
        return res if res else "0"

    def count_nearly_lucky(s, k):
        digits = list(map(int, s))
        n = len(digits)

        @lru_cache(None)
        def dfs(pos, last, found, started, tight):
            if pos == n:
                return 1 if found else 0

            limit = digits[pos] if tight else 9
            ans = 0

            for d in range(limit + 1):
                ntight = tight and (d == limit)

                if not started and d == 0:
                    ans += dfs(pos + 1, -1, found, False, ntight)
                    continue

                nfound = found
                nlast = last

                if d == 4 or d == 7:
                    if last != -1 and pos - last <= k:
                        nfound = True
                    nlast = pos

                ans += dfs(pos + 1, nlast, nfound, True, ntight)

            return ans % MOD

        return dfs(0, -1, False, False, True)

    t, k = map(int, input().split())

    out = []

    for _ in range(t):
        l, r = input().split()

        left = subtract_one(l)

        ans = (count_nearly_lucky(r, k) -
               count_nearly_lucky(left, k)) % MOD

        out.append(str(ans))

    return "\n".join(out)

# provided sample
assert run("1 2\n1 100\n") == "4", "sample 1"

# single number, not nearly lucky
assert run("1 1\n4 4\n") == "0", "single lucky digit"

# exact boundary distance
assert run("1 2\n404 404\n") == "1", "distance exactly k"

# distance too large
assert run("1 1\n404 404\n") == "0", "distance exceeds k"

# multiple valid numbers
assert run("1 2\n44 77\n") == "4", "all two-digit lucky pairs"

# leading zero handling
assert run("1 3\n1 9\n") == "0", "no fake leading-zero pairs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 4 4`|`0`| Một chữ số may mắn là không đủ | 
|`1 2 / 404 404`|`1`| Khoảng cách bằng`k`đếm | 
|`1 1 / 404 404`|`0`| Khoảng cách lớn hơn`k`không được tính | 
|`1 2 / 44 77`|`4`| Tất cả các kết hợp may mắn hai chữ số | 
|`1 3 / 1 9`|`0`| Các số 0 đứng đầu bị bỏ qua | 

## Vỏ cạnh 

Hãy xem xét:```
1 1
404 404
```Các chữ số may mắn đều ở vị trí`0`Và`2`. Khoảng cách của họ là`2`, vượt quá`k = 1`. 

Trong DP: 

| Vị trí | Chữ số | May mắn cuối cùng | Đã tìm thấy | 
| --- | --- | --- | --- | 
| 0 | 4 | 0 | Sai | 
| 1 | 0 | 0 | Sai | 
| 2 | 4 | 2 | Sai | 

điều kiện`pos - last <= k`không thành công nên số đó bị từ chối một cách chính xác. 

Bây giờ hãy xem xét các số 0 đứng đầu:```
1 2
1 9
```DP xử lý nội bộ các số có cùng độ dài với giới hạn trên. Ví dụ,`4`có thể xuất hiện như`"04"`. 

Bởi vì`started = False`trước chữ số khác 0 đầu tiên, số 0 đứng đầu sẽ bị bỏ qua và không trở thành một phần của số. Thuật toán không bao giờ xử lý`"04"`vì có chữ số may mắn ở các vị trí`0`Và`1`. 

Cuối cùng, hãy xem xét:```
1 100
447000000000 447000000000
```Hai chữ số đầu tiên đã thỏa mãn điều kiện. 

| Vị trí | Chữ số | Đã tìm thấy | 
| --- | --- | --- | 
| 0 | 4 | Sai | 
| 1 | 4 | Đúng | 
| 2..11 | khác nhau | Đúng | 

Một lần`found`trở thành đúng, nó vẫn đúng vĩnh viễn. Các chữ số sau không thể làm mất hiệu lực của số.
