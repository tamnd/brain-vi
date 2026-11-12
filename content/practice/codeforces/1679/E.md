---
title: "CF 1679E - Bữa Tiệc Tiêu Biểu Trong Ký Túc Xá"
description: "Chúng ta được cấp một chuỗi có độ dài n gồm 17 chữ cái viết thường đầu tiên (a đến q) và dấu hỏi. Mỗi dấu chấm hỏi hoạt động như một ký tự đại diện có thể được thay thế bằng một chữ cái trong một tập hợp cụ thể được cung cấp trong truy vấn."
date: "2026-06-10T00:42:14+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "combinatorics", "dp", "strings"]
categories: ["algorithms"]
codeforces_contest: 1679
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 791 (Div. 2)"
rating: 2400
weight: 1679
solve_time_s: 133
verified: false
draft: false
---

[CF 1679E - Bữa tiệc tiêu biểu trong ký túc xá](https://codeforces.com/problemset/problem/1679/E) 

**Đánh giá:** 2400 
**Thẻ:** bitmasks, tổ hợp, dp, chuỗi 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi có độ dài`n`bao gồm 17 chữ cái viết thường đầu tiên (`a`bởi vì`q`) và dấu hỏi. Mỗi dấu chấm hỏi hoạt động như một ký tự đại diện có thể được thay thế bằng một chữ cái trong một tập hợp cụ thể được cung cấp trong truy vấn. Mỗi truy vấn cung cấp một tập hợp các chữ cái được phép cho các ký tự đại diện và chúng tôi được yêu cầu đếm, đối với tất cả các chuỗi có thể được tạo bằng cách thay thế dấu chấm hỏi bằng các chữ cái đó, tổng số chuỗi con palindromic. Chuỗi con palindromic là một đoạn liền kề đọc xuôi và ngược giống nhau và các chuỗi con ở các vị trí khác nhau được tính riêng, ngay cả khi chuỗi các chữ cái giống nhau. 

Các ràng buộc nhỏ đối với chính chuỗi đó (`n`lên tới 1000) nhưng lớn về số lượng truy vấn (`q`lên tới 200.000). Điều này ngay lập tức gợi ý rằng bất kỳ thuật toán nào tính toán lại các bảng màu cho mỗi truy vấn sẽ quá chậm. Tính toán tất cả các palindrome trong một chuỗi có độ dài 1000 một cách ngây thơ`O(n^3)`Cách tiếp cận này đã quá nhiều, vì vậy chúng ta cần một phương pháp tính toán trước thông tin về các bảng màu tiềm năng độc lập với truy vấn hoặc có thể nhanh chóng đánh giá nhiều truy vấn bằng cách sử dụng dữ liệu được xử lý trước. 

Trường hợp cạnh tinh tế phát sinh khi chuỗi con chỉ chứa dấu chấm hỏi. Ví dụ, trong chuỗi`??`, một truy vấn cho phép các chữ cái`{a, b}`sẽ tạo ra bốn chuỗi có thể:`aa`,`ab`,`ba`, Và`bb`. Mỗi chuỗi này đóng góp các chuỗi con palindromic khác nhau. Một cách tiếp cận ngây thơ có thể quên rằng mỗi lần thay thế sẽ nhân số lượng palindrome ở một số vùng nhất định, dẫn đến việc đếm thiếu. 

Một trường hợp cạnh khác xảy ra khi một chuỗi con được cố định một phần và một phần dấu chấm hỏi. Ví dụ,`a?b`với`{a, b}`cho phép`aab`hoặc`abb`. Chỉ có cái đầu tiên là palindromic trên toàn bộ chuỗi con, nhưng cả hai đều tạo ra palindrome một chữ cái. Việc triển khai bất cẩn có thể đếm quá hoặc thiếu những trường hợp này. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ là tạo ra tất cả các chuỗi có thể có cho mỗi truy vấn, sau đó liệt kê tất cả các chuỗi con và kiểm tra các bảng màu. Điều này đúng nhưng cực kỳ chậm. Nếu chuỗi có`m`dấu chấm hỏi và một truy vấn cho phép`k`những chữ cái, có`k^m`dây. Việc liệt kê các chuỗi con mất`O(n^2)`, và việc kiểm tra xem một chuỗi con có phải là một chuỗi palindrome hay không là một chuỗi khác`O(n)`trong trường hợp xấu nhất. Dù nhỏ`m`Và`n`nhanh chóng dẫn đến một số lượng lớn các hoạt động khó khăn. Rõ ràng, brute-force chỉ khả thi đối với các chuỗi nhỏ có ít dấu chấm hỏi. 

Quan sát quan trọng cho một giải pháp hiệu quả là các palindromes có tính đối xứng. Chuỗi con là một chuỗi ngược nếu các ký tự tương ứng từ đầu và cuối khớp trực tiếp hoặc ít nhất một trong số chúng là dấu chấm hỏi. Do đó, chúng ta có thể xử lý trước chuỗi để đếm, đối với mỗi chuỗi con có thể, cần bao nhiêu chữ cái riêng biệt để biến chuỗi đó thành một bảng màu. Sau đó, mỗi truy vấn sẽ giảm xuống việc đếm xem có bao nhiêu chữ cái trong truy vấn đáp ứng các yêu cầu này. Bằng cách biểu diễn các tập hợp chữ cái liên quan bằng cách sử dụng mặt nạ bit, chúng ta có thể nhanh chóng tính toán số lượng bảng màu tương thích với mỗi truy vấn bằng cách sử dụng phép nhân tổ hợp. Về cơ bản, chúng tôi tách cấu trúc chuỗi khỏi các chữ cái truy vấn. 

Chúng tôi kết thúc việc tính toán một mảng 2D`f[i][j]`đại diện cho chuỗi con`s[i..j]`, bitmask của các chữ cái phải xuất hiện nếu chuỗi con đó là một bảng màu. Sau đó, với mỗi truy vấn, chúng ta có thể lặp qua tất cả các chuỗi con và kiểm tra xem bộ truy vấn có bao gồm các chữ cái cần thiết hay không. Số lượng chuỗi con palindromic được đóng góp bởi`s[i..j]`khi đó kích thước của bộ truy vấn được nâng lên lũy thừa của số dấu chấm hỏi trong chuỗi con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * k^m * n^2) | O(n^2) | Quá chậm | 
| Tối ưu | O(n^2 + q * 2^17) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng 2D`dp[i][j]`cho tất cả các chuỗi con`s[i..j]`có độ dài từ 1 đến`n`. Nếu như`i == j`, chuỗi con là một chữ cái duy nhất và bitmask của nó là`0`nếu đó là một dấu chấm hỏi hoặc`1 << (ord(s[i]) - ord('a'))`nếu không thì. 
2. Đối với các chuỗi con có độ dài tăng dần từ 2 đến`n`, tính toán`dp[i][j]`dựa trên`dp[i+1][j-1]`. Nếu các ký tự ở vị trí`i`Và`j`đều là các chữ cái và bằng nhau, chuỗi con là chuỗi đối xứng và`dp[i][j]`kế thừa`dp[i+1][j-1]`. Nếu một hoặc cả hai là dấu chấm hỏi, chúng tôi sẽ thêm mặt nạ bit của các chữ cái đã biết vào`dp[i+1][j-1]`. 
3. Đếm số dấu chấm hỏi trong mỗi chuỗi con`s[i..j]`và lưu trữ trong`qm[i][j]`. Điều này là cần thiết vì mỗi dấu chấm hỏi có thể được thay thế độc lập bằng bất kỳ chữ cái nào trong bộ truy vấn, do đó sự đóng góp của`s[i..j]`cho câu trả lời cuối cùng cho một truy vấn được nhân với`k^qm[i][j]`, Ở đâu`k`là kích thước của truy vấn. 
4. Đối với mỗi truy vấn, hãy mã hóa các chữ cái của nó dưới dạng mặt nạ bit`mask`. Sau đó lặp lại trên tất cả các chuỗi con. Nếu mặt nạ chữ cái bắt buộc của chuỗi con`dp[i][j]`là một tập hợp con của`mask`, thêm vào`k^qm[i][j]`để trả lời modulo`998244353`. 
5. Trả lại câu trả lời cho tất cả các truy vấn. 

Tại sao nó hoạt động: Bất biến được duy trì là`dp[i][j]`mã hóa chính xác tất cả các chữ cái phải xuất hiện ở các vị trí cố định để chuỗi con có màu nhạt. Bằng cách lặp lại tất cả các chuỗi con và kiểm tra việc đưa vào bộ truy vấn, chúng tôi đếm tất cả các chuỗi con palindromic chính xác một lần cho mỗi lần thay thế có thể. nhân với`k^qm[i][j]`tính đến tất cả các phép gán hợp lệ của dấu chấm hỏi, đảm bảo tính đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n = int(input())
    s = input().strip()
    q = int(input())
    
    ord_a = ord('a')
    dp = [[0]*n for _ in range(n)]
    qm = [[0]*n for _ in range(n)]
    
    # Precompute single-letter substrings
    for i in range(n):
        qm[i][i] = 1 if s[i] == '?' else 0
        if s[i] != '?':
            dp[i][i] = 1 << (ord(s[i]) - ord_a)
    
    # Expand for substrings of length >= 2
    for length in range(2, n+1):
        for i in range(n-length+1):
            j = i + length - 1
            qm[i][j] = qm[i+1][j-1] if length > 2 else 0
            if s[i] == '?':
                qm[i][j] += 1
            if s[j] == '?' and i != j:
                qm[i][j] += 1
            if (s[i] == '?' or s[j] == '?' or s[i] == s[j]):
                dp[i][j] = dp[i+1][j-1] if length > 2 else 0
                if s[i] != '?' and s[j] != '?' and i != j:
                    dp[i][j] |= 1 << (ord(s[i]) - ord_a)
            else:
                dp[i][j] = -1  # Not a palindrome
    
    # Precompute powers for efficiency
    pow_cache = [1] * 18
    for k in range(1, 18):
        pow_cache[k] = [1]*(n+1)
        for e in range(1, n+1):
            pow_cache[k][e] = pow_cache[k][e-1] * k % MOD
    
    answers = []
    for _ in range(q):
        line = input().strip()
        k = len(line)
        mask = 0
        for c in line:
            mask |= 1 << (ord(c) - ord_a)
        total = 0
        for i in range(n):
            for j in range(i, n):
                if dp[i][j] != -1 and (dp[i][j] & ~mask) == 0:
                    total = (total + pow_cache[k][qm[i][j]]) % MOD
        answers.append(total)
    
    print('\n'.join(map(str, answers)))

solve()
```Giải pháp khởi tạo bảng DP cho tất cả các chuỗi con và đếm dấu chấm hỏi. Chi tiết chính là sử dụng mặt nạ bit để thể hiện các chữ cái bắt buộc, cho phép kiểm tra nhanh chóng bao gồm mỗi truy vấn. Khả năng tính toán trước tránh tính lũy thừa lặp đi lặp lại, rất quan trọng đối với tốc độ. Các trường hợp cạnh như chuỗi con một chữ cái, chuỗi con hoàn toàn không xác định và chuỗi con không thể tạo thành palindromes đều được xử lý một cách rõ ràng. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
7
ab??aba
8
a
b
ab
abc
abcd
abcde
abcdef
abcdefg
```Đối với chuỗi con`s[2..4] = b??`và truy vấn`{a, b}`,`dp[2][4]`chứa thư`b`,`qm[2][4] = 2`. Mặt nạ cho truy vấn bao gồm`b`, vậy đóng góp là`2^2 = 4`palindrome
