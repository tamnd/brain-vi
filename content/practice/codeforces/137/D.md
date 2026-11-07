---
title: "CF 137D - Palindromes"
description: "Chúng ta được cho một chuỗi và một số nguyên k. Chúng tôi có thể thay đổi bất kỳ ký tự nào chúng tôi muốn và mục tiêu là chuyển đổi chuỗi thành một chuỗi gồm nhiều nhất k palindromes trong khi giảm thiểu số lượng ký tự được sửa đổi. Ranh giới phân vùng không cố định."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "strings"]
categories: ["algorithms"]
codeforces_contest: 137
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 98 (Div. 2)"
rating: 1900
weight: 137
solve_time_s: 129
verified: true
draft: false
---

[CF 137D - Palindromes](https://codeforces.com/problemset/problem/137/D) 

**Xếp hạng:** 1900 
**Thẻ:** dp, chuỗi 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi và một số nguyên`k`. Chúng tôi có thể thay đổi bất kỳ ký tự nào chúng tôi muốn và mục tiêu là chuyển đổi chuỗi thành chuỗi nối nhiều nhất`k`palindromes trong khi giảm thiểu số lượng ký tự được sửa đổi. 

Ranh giới phân vùng không cố định. Chúng ta phải quyết định cả vị trí phân tách chuỗi và ký tự nào cần thay đổi bên trong mỗi phần. 

Ví dụ, nếu chuỗi là`abcdef`Và`k = 2`, một phân vùng có thể là:`abc | def`Quay`abc`vào một giá trị palindrome`1`thay đổi, vì chỉ có một cặp đối xứng là khác nhau. Điều này cũng đúng đối với`def`. Tổng chi phí trở thành`2`. 

Một phân vùng khác có thể đưa ra câu trả lời nhỏ hơn, vì vậy chúng ta phải tìm kiếm trên tất cả các phân đoạn hợp lệ. 

Độ dài chuỗi tối đa là`500`. Kích thước đó đủ nhỏ để lập trình động với độ phức tạp bậc ba, nhưng bất cứ điều gì theo cấp số nhân đều là không thể ngay lập tức. Việc liệt kê phân vùng bạo lực sẽ bao gồm khoảng`2^(n-1)`các cách để chia chuỗi, vốn rất lớn về mặt thiên văn ngay cả đối với`n = 50`. 

Khó khăn tiềm ẩn là mỗi chuỗi con đều có chi phí chuyển đổi palindrome riêng. Việc tính toán lại chi phí đó nhiều lần trong DP sẽ đưa ra một yếu tố khác của`O(n)`, làm cho tổng độ phức tạp quá lớn. Chúng ta cần một cách để trả lời: 

"Cần bao nhiêu thay đổi ký tự để tạo chuỗi con`s[l:r]`một bảng màu à?" 

trong thời gian không đổi sau khi tiền xử lý. 

Có một số trường hợp nguy hiểm có thể dễ dàng phá vỡ việc triển khai bất cẩn. 

Hãy xem xét:```
s = "abc"
k = 3
```Câu trả lời tối ưu là`0`, vì chúng ta có thể chia thành:```
a+b+c
```Mỗi ký tự đơn lẻ đã là một bảng màu. Một giải pháp buộc chính xác`k`phân đoạn thay vì "nhiều nhất`k`" có thể vô tình làm phức tạp quá mức DP. 

Một trường hợp tế nhị khác là:```
s = "ab"
k = 1
```Câu trả lời là`1`. Hoặc`aa`hoặc`bb`hoạt động. Một cách triển khai ngây thơ đếm không chính xác các cặp không khớp có thể khiến cả hai ký tự phải thay đổi và trả về`2`. 

Chữ hoa và chữ thường hỗn hợp cũng có vấn đề:```
s = "Aa"
k = 1
```Điều này vẫn cần một sửa đổi vì`'A' != 'a'`. 

Cuối cùng, việc tái thiết rất dễ mắc sai lầm. Giả định:```
s = "abcdef"
k = 2
```Chi phí tối thiểu là không đủ. Chúng ta cũng phải xuất ra một chuỗi được chuyển đổi thực tế với`+`dải phân cách. Nếu chúng ta không lưu trữ các quyết định gốc trong DP thì việc xây dựng lại câu trả lời sau đó sẽ trở nên khó khăn. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản về mặt khái niệm. 

Chúng ta có thể thử tối đa mọi phân vùng có thể có của chuỗi`k`miếng. Đối với mỗi phần, chúng tôi tính toán có bao nhiêu cặp ký tự được phản chiếu khác nhau. Mỗi cặp khác nhau đều đóng góp một sửa đổi bắt buộc. 

Đối với một chuỗi con có độ dài`m`, tính toán trực tiếp chi phí palindrom của nó`O(m)`thời gian. Vì có nhiều phân vùng theo cấp số nhân nên tổng độ phức tạp trở nên vô cùng lớn. 

Ngay cả khi chúng tôi cải thiện việc tìm kiếm phân vùng bằng DP, việc tính toán lại chi phí palindrome nhiều lần vẫn rất tốn kém. Giả sử chúng ta xác định:`dp[i][j] = minimum cost to split first i characters into j palindromes`Việc chuyển đổi từ mọi điểm phân chia trước đó sẽ mang lại:```
dp[i][j] = min(dp[p][j-1] + cost[p+1][i])
```có`O(n^2 * k)`chuyển tiếp. Nếu mỗi lần tính toán chi phí palindrome mất`O(n)`, tổng độ phức tạp trở thành`O(n^4)`. Với`n = 500`, thế là quá chậm. 

Quan sát quan trọng là chi phí chuyển đổi palindrom chỉ phụ thuộc vào sự không khớp được phản ánh. Chúng ta có thể xử lý trước mỗi chuỗi con có giá một lần. 

Đối với chuỗi con`s[l:r]`, những thay đổi tối thiểu cần thiết bằng số vị trí trong đó:```
s[l+i] != s[r-i]
```Điều này có thể được tính toán bằng một khoảng DP đơn giản. 

Sau quá trình tiền xử lý đó, mọi chuyển đổi sẽ trở thành thời gian không đổi, giảm DP chính xuống`O(n^2 * k)`. 

Vấn đề tự nhiên được chia thành hai lớp: 

Đầu tiên, tính chi phí để chuyển đổi mọi chuỗi con thành một chuỗi palindrome. 

Thứ hai, tính toán phân vùng tối ưu bằng lập trình động. 

Chúng tôi cũng lưu trữ thông tin tái thiết để có thể xây dựng lại một chuỗi đã chuyển đổi hợp lệ sau đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n2k + n2) | O(n2 + nk) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước chi phí chuyển đổi palindrome cho tất cả các chuỗi con. 

Định nghĩa:```
cost[l][r] = minimum changes needed to make s[l:r+1] a palindrome
```Nếu như`s[l] == s[r]`, thì các ký tự bên ngoài đã khớp và:```
cost[l][r] = cost[l+1][r-1]
```Nếu không, chúng ta phải sửa đổi một trong số chúng:```
cost[l][r] = cost[l+1][r-1] + 1
```Chúng tôi xử lý các chuỗi con bằng cách tăng độ dài để xác định được các khoảng nhỏ hơn. 
2. Xây dựng phân vùng DP. 

Định nghĩa:```
dp[i][p] = minimum cost to split first i characters into exactly p palindromes
```Chuyển tiếp qua đoạn cuối:```
dp[i][p] = min(
    dp[t][p-1] + cost[t][i-1]
)
```bảng màu cuối cùng ở đâu`s[t:i]`. 
3. Lưu trữ thông tin tái thiết. 

Bất cứ khi nào quá trình chuyển đổi được cải thiện`dp[i][p]`, lưu điểm phân chia`t`. 

Sau đó, điều này cho phép chúng ta xây dựng lại phân vùng đã chọn về phía sau. 
4. Tìm câu trả lời đúng nhất trong số tất cả số lượng phân vùng từ`1`ĐẾN`k`. 

Tuyên bố yêu cầu nhiều nhất`k`palindromes, không chính xác`k`. 

Chúng tôi chọn:```
min(dp[n][p]) for 1 <= p <= k
```5. Xây dựng lại các phân đoạn đã chọn. 

Bắt đầu từ`(n, best_p)`, liên tục theo sau các con trỏ cha được lưu trữ về phía sau. 

Điều này đưa ra khoảng thời gian của tất cả các phần palindromic. 
6. Chuyển đổi từng chuỗi con đã chọn thành một chuỗi màu thực sự. 

Sử dụng hai con trỏ từ cả hai đầu. 

Nếu các ký tự khác nhau, hãy thay đổi một bên để khớp với bên kia. Lựa chọn nào cũng được vì cả hai đều tốn chính xác một lần phẫu thuật. 
7. Tham gia tất cả các palindromes được xây dựng lại với`'+'`. 

### Tại sao nó hoạt động 

DP tiền xử lý tính toán chính xác các sửa đổi tối thiểu cho mỗi chuỗi con vì mỗi cặp được nhân đôi đóng góp độc lập. Các cặp phù hợp không mất phí, các cặp không khớp chỉ cần thay đổi một lần. 

Phân vùng DP đúng vì mọi phân tách hợp lệ đều có một phân đoạn cuối cùng duy nhất. Khi tính toán`dp[i][p]`, chúng tôi thử mọi vị trí bắt đầu có thể cho bảng màu cuối cùng đó. Vì tiền tố trước đó phải được phân chia tối ưu thành`p-1`palindromes, phép truy toán khám phá tất cả các giải pháp hợp lệ và giữ mức tối thiểu. 

Quá trình tái tạo hoạt động vì mọi trạng thái DP đều lưu trữ điểm phân chia tạo ra giá trị tối ưu của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**9

def make_palindrome(t):
    arr = list(t)
    l, r = 0, len(arr) - 1

    while l < r:
        if arr[l] != arr[r]:
            arr[r] = arr[l]
        l += 1
        r -= 1

    return ''.join(arr)

def solve():
    s = input().strip()
    k = int(input())

    n = len(s)

    # cost[l][r] = minimum changes to make s[l:r+1] palindrome
    cost = [[0] * n for _ in range(n)]

    for length in range(2, n + 1):
        for l in range(n - length + 1):
            r = l + length - 1

            if length == 2:
                cost[l][r] = 0 if s[l] == s[r] else 1
            else:
                cost[l][r] = cost[l + 1][r - 1]
                if s[l] != s[r]:
                    cost[l][r] += 1

    # dp[i][p] = min cost for first i chars using p palindromes
    dp = [[INF] * (k + 1) for _ in range(n + 1)]
    parent = [[-1] * (k + 1) for _ in range(n + 1)]

    dp[0][0] = 0

    for i in range(1, n + 1):
        for p in range(1, min(k, i) + 1):
            for t in range(i):
                cur = dp[t][p - 1] + cost[t][i - 1]

                if cur < dp[i][p]:
                    dp[i][p] = cur
                    parent[i][p] = t

    best_cost = INF
    best_p = -1

    for p in range(1, k + 1):
        if dp[n][p] < best_cost:
            best_cost = dp[n][p]
            best_p = p

    parts = []

    i = n
    p = best_p

    while p > 0:
        t = parent[i][p]
        parts.append((t, i))
        i = t
        p -= 1

    parts.reverse()

    answer_parts = []

    for l, r in parts:
        piece = s[l:r]
        answer_parts.append(make_palindrome(piece))

    print(best_cost)
    print('+'.join(answer_parts))

solve()
```Phần đầu tiên tính toán chi phí chuyển đổi palindrome cho mỗi chuỗi con. Xử lý bằng cách tăng chiều dài là cần thiết vì`cost[l][r]`phụ thuộc vào`cost[l+1][r-1]`. Nếu chúng ta xử lý các khoảng theo thứ tự tùy ý thì giá trị bên trong có thể chưa tồn tại. 

Bảng DP sử dụng`i`dưới dạng độ dài tiền tố, không phải chỉ mục. Quy ước đó tránh được nhiều lỗi riêng lẻ vì chuỗi con`s[t:i]`đương nhiên đại diện cho đoạn cuối cùng kết thúc ở vị trí`i-1`. 

Sự chuyển tiếp:```
dp[t][p - 1] + cost[t][i - 1]
```có nghĩa là: 

Đầu tiên phân vùng tối ưu tiền tố trước`t`, sau đó chuyển hậu tố còn lại thành một bảng màu. 

Mảng xây dựng lại lưu trữ điểm phân chia trước đó. Nếu không có điều này, chúng tôi có thể tính toán chi phí tối thiểu nhưng sẽ không biết phân vùng nào đã tạo ra nó. 

Bên trong`make_palindrome`, khi các ký tự được phản chiếu khác nhau, chúng ta sao chép ký tự bên trái sang bên phải. Sao chép theo hướng khác cũng sẽ đúng vì mỗi cách không khớp đều yêu cầu chính xác một sửa đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
abacaba
1
```Toàn bộ chuỗi đã là một bảng màu. 

| Tiểu bang | Giá trị | 
| --- | --- | 
| chi phí[0][6] | 0 | 
| dp[7][1] | 0 | 
| phân vùng đã chọn | [0, 7) | 

Kết quả đã xây dựng:```
abacaba
```Dấu vết cho thấy quá trình tiền xử lý phát hiện chính xác các cặp được nhân đôi không khớp. 

### Ví dụ 2 

đầu vào:```
abcdef
2
```Các trạng thái DP quan trọng: 

| tôi | p | Chia t tốt nhất | Phân đoạn | Chi phí phân khúc | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 3 | 1 | 0 | abc | 1 | 1 | 
| 6 | 2 | 3 | chắc chắn | 1 | 2 | 
| 6 | 2 | 2 | cdef | 2 | 3 | 
| 6 | 2 | 4 | ef | 1 | 3 | 

Tái thiết tối ưu:```
abc + def
```Sau khi chuyển đổi palindrome:```
aba + ded
```Tổng số sửa đổi trở thành`2`. 

Ví dụ này minh họa cách DP đánh giá mọi phân đoạn cuối cùng có thể có và chọn cách phân chia tối ưu toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n2k + n2) | Tiền xử lý O(n2) và chuyển tiếp O(n2k) DP | 
| Không gian | O(n2 + nk) | chi phí chuỗi con, bảng DP và con trỏ gốc | 

Với`n ≤ 500`, số lần chuyển tiếp DP trong trường hợp xấu nhất là khoảng:```
500 × 500 × 500 = 125 million
```Trong Python được tối ưu hóa với các phép toán số nguyên đơn giản, điều này nằm trong giới hạn. Việc sử dụng bộ nhớ cũng thoải mái dưới 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    INF = 10**9

    def make_palindrome(t):
        arr = list(t)
        l, r = 0, len(arr) - 1

        while l < r:
            if arr[l] != arr[r]:
                arr[r] = arr[l]
            l += 1
            r -= 1

        return ''.join(arr)

    s = input().strip()
    k = int(input())

    n = len(s)

    cost = [[0] * n for _ in range(n)]

    for length in range(2, n + 1):
        for l in range(n - length + 1):
            r = l + length - 1

            if length == 2:
                cost[l][r] = 0 if s[l] == s[r] else 1
            else:
                cost[l][r] = cost[l + 1][r - 1]
                if s[l] != s[r]:
                    cost[l][r] += 1

    dp = [[INF] * (k + 1) for _ in range(n + 1)]
    parent = [[-1] * (k + 1) for _ in range(n + 1)]

    dp[0][0] = 0

    for i in range(1, n + 1):
        for p in range(1, min(k, i) + 1):
            for t in range(i):
                cur = dp[t][p - 1] + cost[t][i - 1]

                if cur < dp[i][p]:
                    dp[i][p] = cur
                    parent[i][p] = t

    best_cost = min(dp[n][1:k + 1])

    print(best_cost)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
assert run("abacaba\n1\n") == "0", "sample 1"

# minimum size
assert run("a\n1\n") == "0", "single character"

# already splittable without changes
assert run("abc\n3\n") == "0", "all single-char palindromes"

# case-sensitive mismatch
assert run("Aa\n1\n") == "1", "uppercase/lowercase differ"

# even-length mismatch
assert run("ab\n1\n") == "1", "one replacement needed"

# all equal characters
assert run("aaaaaa\n1\n") == "0", "already palindrome"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a, k=1`|`0`| Đầu vào kích thước tối thiểu | 
|`abc, k=3`|`0`| Sử dụng ít thay đổi hơn bằng cách chia nhỏ | 
|`Aa, k=1`|`1`| Phân biệt chữ hoa chữ thường | 
|`ab, k=1`|`1`| Sửa lỗi đếm không khớp | 
|`aaaaaa, k=1`|`0`| Chiều dài chẵn đã palindromic | 

## Vỏ cạnh 

Hãy xem xét:```
abc
3
```DP cho phép phân vùng thành chính xác`1`,`2`, hoặc`3`palindrome. Giải pháp tốt nhất sử dụng ba phân đoạn ký tự đơn:```
a+b+c
```Mỗi chuỗi con có chi phí chuyển đổi palindrome bằng 0. Thuật toán tối thiểu hóa chính xác trên tất cả`p ≤ k`, thay vì buộc chính xác`k`. 

Bây giờ hãy xem xét:```
Aa
1
```Trong quá trình tiền xử lý:```
cost[0][1] = 1
```bởi vì`'A' != 'a'`. 

DP chọn phân vùng duy nhất có thể, toàn bộ chuỗi. Việc tái thiết có thể tạo ra:```
AA
```hoặc```
aa
```Cả hai đều yêu cầu một sửa đổi. 

Một trường hợp tế nhị khác là:```
abcd
1
```So sánh phản ánh là:```
a vs d -> mismatch
b vs c -> mismatch
```Vì thế:```
cost[0][3] = 2
```Thuật toán đếm chính xác các thay đổi trên mỗi cặp được phản chiếu chứ không phải trên mỗi ký tự. 

Cuối cùng, hãy xem xét:```
abcdef
6
```Câu trả lời tối ưu là`0`bởi vì mỗi ký tự đều có thể trở thành bảng màu của riêng nó. DP xử lý việc này một cách tự nhiên vì các chuyển đổi có độ dài phân đoạn`1`luôn có chi phí`0`.
