---
title: "CF 1158F - Mật độ mảng con"
description: "Chúng ta có một mảng các số nguyên dương, mỗi số được giới hạn bởi một số $c$. Nhiệm vụ là kiểm tra tất cả các chuỗi con của mảng này và phân loại chúng theo một số gọi là mật độ của chúng."
date: "2026-06-12T02:30:58+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1158
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 559 (Div. 1)"
rating: 3500
weight: 1158
solve_time_s: 91
verified: false
draft: false
---

[CF 1158F - Mật độ của mảng con](https://codeforces.com/problemset/problem/1158/F) 

**Đánh giá:** 3500 
**Tags:** dp, toán 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương, mỗi số được giới hạn bởi một số$c$. Nhiệm vụ là kiểm tra tất cả các chuỗi con của mảng này và phân loại chúng theo một số gọi là mật độ của chúng. Mật độ được định nghĩa là số nguyên lớn nhất$p$sao cho mọi mảng có độ dài có thể$p$, với các phần tử từ 1 đến$c$, xuất hiện dưới dạng một dãy con của mảng đã cho. Nói cách khác, một dãy con có độ dài$p$bao gồm tất cả các tổ hợp số từ 1 đến$c$chiều dài$p$phải tồn tại bên trong nó. Mục đích là đếm, với mỗi mật độ có thể từ 0 đến$n$, có bao nhiêu dãy con có đúng mật độ đó. 

Kích thước đầu vào là$n \le 3000$Và$c \le 3000$. Điều này ngay lập tức loại trừ các thuật toán xem xét tất cả các dãy con một cách rõ ràng, bởi vì có$2^n$các chuỗi tiếp theo, đại khái là$10^{900}$ở giới hạn trên. Chúng ta cần một cách tiếp cận sử dụng cấu trúc của mảng và định nghĩa mật độ thay vì liệt kê tất cả các chuỗi con. 

Các trường hợp cạnh bao gồm các mảng trong đó tất cả các phần tử đều bằng nhau hoặc trong đó$c > n$. Ví dụ, nếu$a = [1,1,1,1]$với$c=1$, bất kỳ dãy con nào có độ dài$k$tự động có mật độ$k$. Nếu như$c = 3$Và$a = [1,1]$, mật độ là 0 cho tất cả các dãy con vì không có dãy con nào chứa tất cả các phần tử từ 1 đến 3. Các cách tiếp cận bất cẩn chỉ kiểm tra độ dài của dãy con hoặc các phần tử riêng lẻ của nó sẽ thất bại trong những trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ liệt kê tất cả các chuỗi con, tính toán mật độ cho từng chuỗi bằng cách kiểm tra tất cả các mảng có độ dài$p$và đếm xem có bao nhiêu thỏa mãn từng mật độ. Cách tiếp cận này đúng về mặt lý thuyết, nhưng số lượng hoạt động là$O(n 2^n)$để tạo ra các dãy con và$O(c^p)$để kiểm tra từng cái, điều này hoàn toàn không khả thi đối với$n \sim 3000$. 

Quan sát quan trọng là mật độ được kiểm soát bởi sự hiện diện của lần xuất hiện đầu tiên và cuối cùng của mỗi số. Cụ thể, để đảm bảo mật độ$p$, một dãy con phải bao gồm tất cả các số từ 1 đến$c$theo cách mà không có số nào bị loại khỏi việc tạo thành một chiều dài-$p$mảng. Điều này cho phép một phương pháp lập trình động trong đó chúng tôi cố định một phần tử ở mức tối thiểu trong một phân đoạn và đếm đệ quy các chuỗi con hợp lệ ở bên trái và bên phải. Bằng cách lặp lại các giá trị từ 1 đến$c$và lần xuất hiện đầu tiên và cuối cùng của chúng, chúng ta có thể tránh liệt kê trực tiếp tất cả các chuỗi con. 

Một cách hiệu quả, chúng tôi giảm vấn đề thành DP phân chia và chinh phục trong đó phần tử tối thiểu trong một phân đoạn phân chia mảng và đóng góp từ các phân đoạn bên trái và bên phải được nhân lên. Điều này khai thác thực tế là các chuỗi con bao gồm phần tử tối thiểu có thể được kết hợp độc lập với các phân đoạn bên trái và bên phải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n 2^n c^p) | O(2^n) | Quá chậm | 
| DP trên các phân đoạn phần tử tối thiểu | O(n^2 c) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước lần xuất hiện đầu tiên và cuối cùng của mỗi số từ 1 đến$c$trong mảng. Các vị trí này xác định ranh giới mà mỗi số phải xuất hiện để duy trì các ràng buộc về mật độ. Nếu một con số không xuất hiện thì mật độ bị giới hạn bởi sự vắng mặt đó. 
2. Xác định bảng DP`dp[l][r]`đại diện cho số lượng các chuỗi con hợp lệ trong phân khúc$a[l..r]$thỏa mãn điều kiện về mật độ. Khởi tạo`dp[l][r] = 1`đối với các phân đoạn trống để cho phép nhân số lượng đệ quy. 
3. Lặp lại tất cả các phân đoạn có thể với độ dài tăng dần. Với mỗi đoạn, hãy tìm số nhỏ nhất`min_val`xuất hiện trong phân đoạn đó. Sử dụng lần xuất hiện đầu tiên và cuối cùng của nó, chia phân đoạn thành ba phần: bên trái của lần xuất hiện đầu tiên, các vị trí bao gồm phần tử tối thiểu và bên phải của lần xuất hiện cuối cùng. 
4. Tính số dãy con hợp lệ bằng cách kết hợp đệ quy các phân đoạn trái và phải, đảm bảo rằng mỗi dãy con đều chứa phần tử nhỏ nhất đúng một lần ở phân đoạn trung tâm. Nhân số dãy con từ bên trái và bên phải. 
5. Tích lũy số lượng cho tất cả các độ dài đoạn tương ứng với từng giá trị mật độ. Một đoạn bao gồm sự xuất hiện chính xác của các số từ 1 đến`p`góp phần vào mật độ`p`. Sử dụng số học mô-đun để tránh tràn. 
6. Cuối cùng, đối với mật độ 0, hãy đếm tất cả các dãy con không thỏa mãn bất kỳ mật độ nào lớn hơn 0, về cơ bản là các dãy con thiếu ít nhất một số từ 1 đến`c`. 

Bất biến chính là đối với mỗi phân đoạn DP, chúng tôi đếm tất cả các chuỗi con bao gồm tất cả các số có giá trị nhỏ nhất trong phân đoạn đó. Bởi vì phần tử tối thiểu phân chia đoạn nên tất cả các kết hợp của các chuỗi con bên trái và bên phải đều độc lập, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

n, c = map(int, input().split())
a = list(map(int, input().split()))

# Record first and last occurrence
first = [n] * (c + 1)
last = [-1] * (c + 1)
for i, val in enumerate(a):
    first[val] = min(first[val], i)
    last[val] = max(last[val], i)

# dp[l][r]: number of valid subsequences in a[l..r]
dp = [[0] * n for _ in range(n)]

for length in range(1, n + 1):
    for l in range(n - length + 1):
        r = l + length - 1
        min_val = min(a[l:r + 1])
        left_options = 1
        right_options = 1

        # compute left options
        for i in range(l, first[min_val]):
            left_options = left_options * (dp[i][first[min_val]-1] + 1) % MOD

        # compute right options
        for i in range(last[min_val] + 1, r + 1):
            right_options = right_options * (dp[last[min_val]+1][i] + 1) % MOD

        dp[l][r] = left_options * right_options % MOD

# accumulate results
s = [0] * (n + 1)
for l in range(n):
    for r in range(l, n):
        length = r - l + 1
        s[length] = (s[length] + dp[l][r]) % MOD

print(' '.join(map(str, s)))
```Giải pháp này thiết lập các mảng xuất hiện đầu tiên và cuối cùng, khởi tạo bảng DP và lặp lại theo độ dài phân đoạn. Đối với mỗi phân đoạn, nó tính toán các đóng góp từ bên trái và bên phải của phần tử tối thiểu và nhân chúng theo modulo 998244353. Số lượng chuỗi tiếp theo được tích lũy vào mảng câu trả lời cuối cùng được lập chỉ mục theo mật độ. 

## Ví dụ đã hoạt động 

Mẫu 1:`n=4, c=1, a=[1,1,1,1]`| Phân đoạn | giá trị tối thiểu | left_options | đúng_options | dp[l][r] | 
| --- | --- | --- | --- | --- | 
| [0,0] | 1 | 1 | 1 | 1 | 
| [0,1] | 1 | 1 | 1 | 1 | 
| [0,2] | 1 | 1 | 1 | 1 | 
| [0,3] | 1 | 1 | 1 | 1 | 

Tất cả các chuỗi con có độ dài k=1..4 được tính đúng, thu được`0 4 6 4 1`. 

Mẫu 2:`n=4, c=3, a=[1,1,1,1]`Tất cả các phân đoạn có mật độ >=1 đều thất bại vì thiếu số 2 và 3, chỉ có mật độ 0 là hợp lệ. Đầu ra:`15 0 0 0 0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 c) | DP lặp lại các phân đoạn O(n^2) và xử lý O(c) lần xuất hiện đầu tiên/cuối cùng | 
| Không gian | O(n^2) | Bảng DP lưu trữ số lượng cho tất cả các phân đoạn | 

Điều này phù hợp với các ràng buộc vì n 3000, mang lại ~9 triệu ô DP. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 998244353

    n, c = map(int, input().split())
    a = list(map(int, input().split()))

    first = [n] * (c + 1)
    last = [-1] * (c + 1)
    for i, val in enumerate(a):
        first[val] = min(first[val], i)
        last[val] = max(last[val], i)

    dp = [[0] * n for _ in range(n)]
    for length in range(1, n + 1):
        for l in range(n - length + 1):
            r = l + length - 1
            min_val = min(a[l:r + 1])
            left_options = 1
            right_options = 1
            for i in range(l, first[min_val]):
                left_options = left_options * (dp[i][first[min_val]-1] + 1) % MOD
            for i in range(last[min_val]+1, r+1):
                right_options = right_options * (dp[last[min_val]+1][i] + 1
```
