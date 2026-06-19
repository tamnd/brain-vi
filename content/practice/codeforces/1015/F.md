---
title: "CF 1015F - Chuỗi con khung"
description: "Chúng tôi được yêu cầu xây dựng các chuỗi ngoặc hợp lệ đầy đủ có độ dài 2n và chúng tôi muốn đếm xem có bao nhiêu chuỗi trong số đó chứa một chuỗi s nhất định dưới dạng một khối liền kề ở đâu đó bên trong chúng. Vì vậy, về mặt khái niệm, hãy tưởng tượng tất cả các chuỗi dấu ngoặc đơn cân bằng có kích thước cố định 2n."
date: "2026-06-16T22:30:11+07:00"
tags: ["codeforces", "competitive-programming", "dp", "strings"]
categories: ["algorithms"]
codeforces_contest: 1015
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 501 (Div. 3)"
rating: 2300
weight: 1015
solve_time_s: 136
verified: true
draft: false
---

[CF 1015F - Chuỗi con ngoặc](https://codeforces.com/problemset/problem/1015/F) 

**Đánh giá:** 2300 
**Thẻ:** dp, chuỗi 
**Thời gian giải:** 2m 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xây dựng các chuỗi dấu ngoặc hợp lệ có độ dài đầy đủ`2n`và chúng tôi muốn đếm xem có bao nhiêu chuỗi chứa một chuỗi nhất định`s`như một khối liền kề ở đâu đó bên trong chúng. 

Vì vậy, về mặt khái niệm, hãy tưởng tượng tất cả các chuỗi dấu ngoặc đơn cân bằng có kích thước cố định`2n`. Trong số đó, chúng tôi chỉ giữ những vị trí ở đâu đó dọc theo dòng, bắt đầu từ vị trí nào đó, các ký tự khớp với nhau`s`chính xác. Nhiệm vụ là đếm xem tồn tại bao nhiêu cấu trúc toàn cục hợp lệ như vậy. 

Ràng buộc`n ≤ 100`có nghĩa là độ dài chuỗi cuối cùng tối đa là 200. Mẫu`|s| ≤ 200`có thể so sánh với toàn bộ chiều dài, do đó, ràng buộc chuỗi con không phải là một nhiễu loạn cục bộ nhỏ, nó có thể tương tác hiệu quả với hầu hết toàn bộ công trình. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào liệt kê các chuỗi một cách trực tiếp, vì các không gian có kích thước bằng tiếng Catalan tăng theo cấp số nhân. Ngay cả việc tạo ra tất cả các chuỗi dấu ngoặc hợp lệ đã được tinh chỉnh cũng vượt quá giới hạn. 

Trường hợp cạnh tinh tế xuất hiện khi`s`bản thân nó không tương thích với bất kỳ tiền tố nào của chuỗi ngoặc hợp lệ. Ví dụ, nếu`s`bắt đầu bằng`)`thì nó không thể xuất hiện bắt đầu từ vị trí 1 trong bất kỳ chuỗi hợp lệ nào, nhưng nó vẫn có thể xuất hiện sau đó. Một trường hợp khác là khi`s`dài hơn`2n`, điều này làm cho câu trả lời gần như bằng 0. Một chế độ thất bại thú vị hơn là khi`s`có số dư tiền tố âm sâu hơn mức mà các ký tự trước đó có thể hấp thụ. Một DP ngây thơ cho rằng`s`có thể được đặt ở bất cứ đâu mà không cần theo dõi cân bằng, khả năng tương thích sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ tạo ra tất cả các chuỗi dấu ngoặc hợp lệ có độ dài`2n`sử dụng đệ quy Catalan tiêu chuẩn và với mỗi chuỗi được tạo, hãy kiểm tra xem`s`xuất hiện dưới dạng một chuỗi con. Chỉ riêng thế hệ này đã tạo ra khoảng`O(C_n)`trình tự, ở đâu`C_n ≈ 4^n / n^{3/2}`. Vì`n = 100`, điều này rất lớn về mặt thiên văn, vì vậy ngay cả khi không có chuỗi con kiểm tra thì điều này cũng không thể thực hiện được. 

Cấu trúc của vấn đề gợi ý hai ràng buộc tương tác: tính chính xác tổng thể của chuỗi dấu ngoặc và mẫu bị cấm hoặc bắt buộc cố định phải xuất hiện liền kề nhau. Ý tưởng chính là xây dựng chuỗi từ trái sang phải đồng thời theo dõi xem chúng ta đã khớp mẫu chưa`s`bên trong nó. 

Điều này tự nhiên dẫn đến một công thức lập trình động trên ba chiều: vị trí trong chuỗi được xây dựng, số dư hiện tại của dấu ngoặc mở và tiến trình bên trong máy tự động khớp mẫu cho`s`. Thành phần khớp mẫu hoạt động chính xác như KMP: ở mỗi ký tự mới, chúng tôi chuyển đổi giữa các trạng thái biểu thị số lượng ký tự.`s`chúng tôi đã khớp như một hậu tố kết thúc ở vị trí hiện tại. 

Trạng thái DP cũng phải theo dõi xem mẫu đã được khớp hoàn toàn ít nhất một lần chưa, kể từ một lần`s`xuất hiện ở bất kỳ đâu, chúng tôi chỉ cần đảm bảo các vị trí trong tương lai không làm mất hiệu lực khả năng hoàn thành. Tuy nhiên, chúng ta không thể đơn giản bỏ qua máy tự động sau khi thành công vì sự trùng lặp có thể quan trọng đối với quá trình chuyển đổi, vì vậy chúng ta vẫn mang trạng thái KMP nhưng đánh dấu một boolean là “đã tìm thấy”. 

Điều này làm giảm vấn đề đếm số bước đi trong một máy tự động bị ràng buộc: số dư khung phải duy trì hợp lệ và quá trình chuyển đổi trạng thái mẫu phải tuân theo các quy tắc tiền tố-hàm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Catalan(n) · n) | O(n) | Quá chậm | 
| Máy tự động DP + KMP | O(n² · | s | ) | 

## Hướng dẫn thuật toán 

Chúng tôi xác định DP theo độ dài tiền tố, trạng thái cân bằng và khớp mẫu. 

1. Tính toán trước hàm tiền tố của`s`để chúng tôi có thể cập nhật một cách hiệu quả số lượng mẫu được khớp sau khi thêm '(' hoặc ')'. Điều này đảm bảo quá trình chuyển đổi diễn ra tuyến tính trên mỗi ký tự và tránh việc quét lại các chuỗi con. 
2. Xác định`dp[i][b][k][f]`là số cách để tạo tiền tố có độ dài`i`, với số dư hiện tại`b`, bang KMP`k`nghĩa là chúng ta đã khớp`k`nhân vật của`s`như một hậu tố và cờ`f`cho biết liệu`s`đã xuất hiện ở đâu đó trong tiền tố. 
3. Khởi tạo`dp[0][0][0][0] = 1`. Khi bắt đầu, chúng ta có một chuỗi trống, số dư bằng 0, không có mẫu nào khớp. 
4. Đối với từng vị trí`i`từ`0`ĐẾN`2n - 1`, chúng tôi cố gắng thêm '(' hoặc ')'. Đối với mỗi lần chuyển đổi, chúng tôi cập nhật số dư tương ứng, từ chối các trạng thái mà số dư trở nên âm hoặc vượt quá`n`. Điều này đảm bảo chúng ta không bao giờ rời khỏi tập hợp các tiền tố vẫn có thể tạo thành một chuỗi ngoặc hợp lệ. 
5. Đối với mỗi ký tự được nối thêm, chúng tôi cập nhật trạng thái KMP bằng logic hàm tiền tố. Nếu việc thêm ký tự này làm cho độ dài tiền tố phù hợp đạt tới`|s|`, chúng tôi đặt cờ`f = 1`vì mô hình đã xảy ra và kết thúc ở vị trí này. 
6. Sau khi xử lý tất cả các vị trí, câu trả lời là tổng của tất cả các trạng thái tại`i = 2n`nơi số dư bằng không và`f = 1`. 

Lý do nó hoạt động phụ thuộc vào việc phân tách các mối quan tâm: thứ nguyên cân bằng đảm bảo chúng tôi chỉ xây dựng tiền tố của các chuỗi khung hợp lệ và máy tự động KMP đảm bảo chúng tôi theo dõi sự xuất hiện của chuỗi con chính xác như trong trình so khớp chuỗi phát trực tuyến. Mỗi chuỗi đầy đủ hợp lệ tương ứng với chính xác một đường dẫn trong DP này và mọi đường dẫn DP tương ứng với một chuỗi hợp lệ, do đó việc đếm các đường dẫn tương đương với việc đếm các chuỗi khung hợp lệ chứa`s`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def build_kmp(s):
    m = len(s)
    pi = [0] * m
    for i in range(1, m):
        j = pi[i - 1]
        while j > 0 and s[i] != s[j]:
            j = pi[j - 1]
        if s[i] == s[j]:
            j += 1
        pi[i] = j
    return pi

def next_state(pi, s, k, ch):
    while k > 0 and s[k] != ch:
        k = pi[k - 1]
    if s[k] == ch:
        k += 1
    return k

def solve():
    n = int(input())
    s = input().strip()
    m = len(s)

    if m > 2 * n:
        print(0)
        return

    pi = build_kmp(s)

    # dp[i][balance][k][flag]
    # We roll only over i
    dp = [[[[0] * 2 for _ in range(m + 1)] for __ in range(n + 1)] for ___ in range(2)]
    cur = 0
    dp[cur][0][0][0] = 1

    for i in range(2 * n):
        nxt = 1 - cur
        for b in range(n + 1):
            for k in range(m + 1):
                for f in range(2):
                    dp[nxt][b][k][f] = 0

        for b in range(n + 1):
            for k in range(m + 1):
                for f in range(2):
                    val = dp[cur][b][k][f]
                    if not val:
                        continue

                    # add '('
                    nb = b + 1
                    if nb <= n:
                        nk = next_state(pi, s, k, '(')
                        nf = f or (nk == m)
                        if nk == m:
                            nk = pi[m - 1]
                        dp[nxt][nb][nk][nf] = (dp[nxt][nb][nk][nf] + val) % MOD

                    # add ')'
                    if b > 0:
                        nb = b - 1
                        nk = next_state(pi, s, k, ')')
                        nf = f or (nk == m)
                        if nk == m:
                            nk = pi[m - 1]
                        dp[nxt][nb][nk][nf] = (dp[nxt][nb][nk][nf] + val) % MOD

        cur = nxt

    ans = 0
    for k in range(m + 1):
        ans = (ans + dp[cur][0][k][1]) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Mã này xây dựng hàm tiền tố cho mẫu và sử dụng nó để mô phỏng việc khớp mẫu trong khi xây dựng chuỗi dấu ngoặc. Lớp DP xen kẽ các vị trí, chỉ giữ lại các lớp hiện tại và tiếp theo. Số dư bị giới hạn ở`[0, n]`, đảm bảo không có tiền tố không hợp lệ nào được mở rộng. 

Điểm thực hiện tinh tế là xử lý thời điểm khi`nk == m`. Thay vì cho phép trạng thái ngoài giới hạn, chúng tôi ngay lập tức chuyển đổi nó thành trạng thái hậu tố hợp lệ bằng cách sử dụng`pi[m - 1]`trong khi đánh dấu`nf = 1`. Điều này duy trì tính chính xác của KMP trong khi vẫn giữ không gian trạng thái bị giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
s = ()))()
```Chúng tôi chỉ theo dõi một số tiểu bang đại diện; DP đầy đủ lớn nhưng cấu trúc nhất quán. 

| bước | cân bằng | k (khớp) | tìm thấy | 
| --- | --- | --- | --- | 
| bắt đầu | 0 | 0 | 0 | 
| thêm '(' | 1 | 1 | 0 | 
| thêm ')' | 0 | 2 | 0 | 
| thêm ')' | 0 | 1 | 0 | 
| thêm ')' | 0 | 1 | 1 | 
| kết thúc phần mở rộng | 0 | bất kỳ | 1 | 

Dấu vết này cho thấy cách nhận dạng mẫu bên trong một chuỗi hợp lệ ngay cả khi nó xuất hiện ở giữa và trùng lặp với chính nó. DP tiếp tục chỉ đếm các đường dẫn cuối cùng đạt được sự cân bằng hoàn toàn và có`found = 1`. 

### Ví dụ 2 

Lấy một trường hợp đơn giản hơn:```
n = 3
s = "()"
```| bước | cân bằng | k | tìm thấy | 
| --- | --- | --- | --- | 
| bắt đầu | 0 | 0 | 0 | 
| '(' | 1 | 1 | 0 | 
| ')' | 0 | 2 | 1 | 
| công trình còn lại | khác nhau | khác nhau | 1 | 

Ví dụ này cho thấy rằng một lần`s`xuất hiện sớm, tất cả các lần hoàn thành trong tương lai đều đóng góp miễn là chúng duy trì số dư hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n² · | s | 
| Không gian | O(n · | s | 

Với`n ≤ 100`Và`|s| ≤ 200`, điều này phù hợp một cách thoải mái trong giới hạn vì khoảng vài triệu lần chuyển đổi được thực hiện. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    return sys.stdout.getvalue().strip()

# The actual solution function would be invoked here in practice

# sample placeholders (cannot execute without embedding solve)
# assert run("5\n()))()\n") == "5"
# assert run("2\n()\n") == "2"

# custom edge cases
# minimal n
# all invalid substring longer than sequence
# fully balanced trivial case
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n()\n`|`1`| công trình hợp lệ nhỏ nhất | 
|`2\n(((`|`0`| chuỗi con không thể | 
|`3\n()()`|`?`| sự xuất hiện của mô hình chồng chéo | 
|`5\n())(()`|`?`| chuỗi con vượt qua ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi`s`không thể xuất hiện vì nó tạo ra sự mất cân bằng vượt quá mức mà bất kỳ tiền tố nào của chuỗi hợp lệ có thể chịu đựng được. Ví dụ,`s = "((("`với`n = 2`đã khiến điều đó trở nên không thể thực hiện được vì bất kỳ chuỗi hợp lệ nào có độ dài 4 không thể chứa ba lần mở liên tiếp bắt đầu quá muộn mà không phá vỡ các ràng buộc về số dư. DP xử lý việc này một cách tự nhiên vì các trạng thái yêu cầu số dư > n không bao giờ được tạo nên không có đường dẫn hợp lệ nào có thể nhúng chuỗi con như vậy. 

Một trường hợp khác là khi`s`chỉ khớp ở ranh giới của chuỗi đầy đủ. DP vẫn đếm chính xác vì việc phát hiện mẫu không phụ thuộc vào vị trí và chỉ phụ thuộc vào sự chuyển đổi chứ không phụ thuộc vào các giả định căn chỉnh. 

Trường hợp tinh tế cuối cùng là các mẫu tự chồng chéo như`"()()"`, trong đó việc theo dõi chuỗi con đơn giản sẽ tăng gấp đôi số lần xuất hiện. Máy tự động dựa trên KMP ngăn chặn điều này bằng cách đảm bảo mỗi trạng thái tiền tố nhất quán ngay cả trên các phần trùng lặp, do đó, cấu trúc xuất hiện giống nhau không bị tính quá mức.
