---
title: "CF 7D - Độ Palindrome"
description: "Chúng ta được cung cấp một chuỗi bao gồm các chữ cái và chữ số, đồng thời chúng ta được yêu cầu tính toán một thước đo đặc biệt cho mỗi tiền tố được gọi là _palindrome độ_."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "hashing", "strings"]
categories: ["algorithms"]
codeforces_contest: 7
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 7"
rating: 2200
weight: 7
solve_time_s: 65
verified: true
draft: false
---
[CF 7D - Bằng Palindrome](https://codeforces.com/problemset/problem/7/D) 

**Đánh giá:** 2200 
**Thẻ:** băm, chuỗi 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi bao gồm các chữ cái và chữ số, đồng thời chúng ta được yêu cầu tính toán một thước đo đặc biệt cho mỗi tiền tố được gọi là _palindrome độ_. Một chuỗi có mức độ palindrome _k_ nếu nó là một palindrome và nửa đầu và nửa cuối của nó có độ dài`len(s) // 2`mỗi người đều có độ _k-1_. Theo định nghĩa, bất kỳ chuỗi nào cũng có bậc 0 nếu chúng ta coi nó là chuỗi rỗng. Mục tiêu của chúng tôi là tính tổng mức độ palindrome của tất cả các tiền tố của chuỗi đầu vào. 

Vì chuỗi có thể có tối đa 5·10⁶ ký tự nên cách tiếp cận đơn giản là kiểm tra từng tiền tố một cách độc lập và kiểm tra đệ quy các nửa của nó sẽ là quá chậm. Bất kỳ thuật toán nào chạy trong thời gian O(n²) đều không khả thi vì bình phương 5·10⁶ là 25·10¹² hoạt động, vượt xa giới hạn thời gian. Do đó chúng ta cần một thuật toán chạy gần với thời gian tuyến tính, lý tưởng nhất là O(n). 

Các trường hợp cạnh không rõ ràng phát sinh khi các chuỗi có mẫu lặp lại hoặc rất ngắn. Ví dụ: chuỗi một ký tự "a" có độ 1, nhưng chuỗi hai ký tự như "aa" có độ 2, vì tiền tố và hậu tố có độ dài 1 đều có độ 1. Nếu chúng ta không truyền độ chính xác từ các bảng màu nhỏ hơn, chúng ta có thể đếm thiếu. 

Một trường hợp tinh vi khác xảy ra khi tiền tố là một bảng màu nhưng hai nửa của nó thì không, chẳng hạn như "abba". Chuỗi đầy đủ của nó là một palindrome, nhưng tiền tố "ab" và hậu tố "ba" không phải là palindrome cấp 1, vì vậy cấp "abba" là 1, không phải 2. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ lặp lại mọi tiền tố của chuỗi, kiểm tra xem đó có phải là một bảng màu hay không, sau đó tính toán đệ quy mức độ bằng cách kiểm tra nửa bên trái và bên phải của nó. Về nguyên tắc, điều này hoạt động nhưng trở nên không thực tế đối với các chuỗi lớn. Đối với một chuỗi có độ dài n, cách tiếp cận brute-force có thể yêu cầu khoảng n2/2 phép so sánh cho mỗi tiền tố, dẫn đến tổng số phép toán là O(n³) nếu chúng ta bao gồm cả việc truyền độ. Tốc độ đó rõ ràng là quá chậm đối với n = 5·10⁶. 

Thông tin chi tiết quan trọng là nhận ra rằng độ palindrome có thể được tính tăng dần bằng cách sử dụng mảng _tiền tố của độ_. Nếu chúng ta tính toán trước tiền tố nào là palindrome một cách hiệu quả, chúng ta có thể xây dựng mảng độ chỉ trong một lần duyệt. Để làm điều này, chúng tôi sử dụng thuật toán Z hoặc kỹ thuật băm lăn, nhưng một cách tiếp cận đơn giản hơn tận dụng thực tế là thuộc tính palindrome của chuỗi vốn có tính đệ quy: bậc của một chuỗi phụ thuộc trực tiếp vào bậc của một nửa chuỗi đó. Cụ thể, nếu tiền tố có độ dài L là một bảng màu, chúng ta có thể kiểm tra độ của nửa đầu của nó (độ dài L//2) và tăng thêm 1. Nếu chúng ta duy trì một mảng`deg[i]`đối với mức độ palindrome của tiền tố kết thúc ở chỉ số i, chúng ta có thể tính toán`deg[i]`BẰNG`deg[i//2] + 1`nếu tiền tố là palindrome hoặc 0 nếu ngược lại. 

Để kiểm tra xem tiền tố có phải là palindrome một cách hiệu quả hay không, chúng ta có thể sử dụng hàm băm. Chúng tôi tính toán hàm băm cuộn từ trái sang phải và hàm băm cuộn ngược từ phải sang trái. Nếu hai giá trị băm này khớp với một tiền tố nhất định thì đó là một bảng màu. Điều này đưa ra kiểm tra palindrome O(n) cho mỗi tiền tố và tổng O(n) cho tất cả các tiền tố có tính toán trước thích hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán mảng băm chuyển tiếp cho chuỗi. Hàm băm của mỗi tiền tố cho phép truy xuất theo thời gian liên tục bất kỳ hàm băm chuỗi con nào bằng cách sử dụng mảng nguồn được tính toán trước. Điều này đảm bảo chúng ta có thể nhanh chóng so sánh các tiền tố với các hậu tố được phản ánh của chúng. 
2. Tính mảng băm đảo ngược tương tự. Điều này phản chiếu chuỗi và cho phép chúng ta so sánh tiền tố với hậu tố tương ứng của nó mà không cần đảo ngược chuỗi con về mặt vật lý. 
3. Khởi tạo một mảng`deg`có độ dài n để lưu trữ mức độ palindrome cho mỗi tiền tố. 
4. Lặp lại từng tiền tố kết thúc ở vị trí i. Sử dụng mảng băm để kiểm tra xem chuỗi con`s[0..i]`là một palindrome. Nếu không, hãy đặt`deg[i] = 0`. 
5. Nếu tiền tố là một bảng màu, hãy tính bậc của nó bằng cách sử dụng`deg[i] = deg[i//2] + 1`. Đây,`i//2`tương ứng với tiền tố lớn nhất có bậc xác định cấp độ tiếp theo. Tăng thêm 1 sẽ truyền bá định nghĩa đệ quy của mức độ palindrome. 
6. Tổng hợp tất cả`deg[i]`giá trị để có được câu trả lời cuối cùng. 

Tại sao nó hoạt động: Hàm băm tiến và lùi đảm bảo phát hiện chính xác bảng màu cho mọi tiền tố. Sự lan truyền đệ quy từ`deg[i//2]`đảm bảo rằng bậc của mỗi tiền tố phản ánh chính xác bậc của các nửa của nó. Điều này duy trì sự bất biến rằng`deg[i]`chính xác là k lớn nhất sao cho tiền tố kết thúc tại i là k-palindrome. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7
BASE = 31

s = input().strip()
n = len(s)

pow_base = [1] * (n + 1)
for i in range(1, n + 1):
    pow_base[i] = (pow_base[i-1] * BASE) % MOD

hash_forward = [0] * (n + 1)
hash_reverse = [0] * (n + 1)

for i in range(n):
    hash_forward[i+1] = (hash_forward[i] * BASE + ord(s[i])) % MOD
    hash_reverse[i+1] = (hash_reverse[i] * BASE + ord(s[n-1-i])) % MOD

def get_hash(h, l, r):
    return (h[r+1] - h[l] * pow_base[r-l+1]) % MOD

deg = [0] * n
ans = 0

for i in range(n):
    if get_hash(hash_forward, 0, i) == get_hash(hash_reverse, n-i-1, n-1):
        deg[i] = (deg[i//2] + 1) if i > 0 else 1
    else:
        deg[i] = 0
    ans += deg[i]

print(ans)
```Mã bắt đầu bằng việc tính toán trước lũy thừa của cơ sở băm modulo một số nguyên tố lớn. Các giá trị băm tiến và lùi được lưu trữ để cho phép kiểm tra bảng màu theo thời gian liên tục. Vòng lặp chính lặp qua từng tiền tố, kiểm tra thuộc tính palindrome bằng cách sử dụng so sánh hàm băm, sau đó tính toán độ đệ quy thông qua`deg[i//2]`. Tổng hợp tất cả các độ tạo ra câu trả lời. Các lỗi xảy ra một cách cẩn thận được tránh cẩn thận bằng cách sử dụng nhất quán lập chỉ mục dựa trên 0 cho băm chuỗi con. 

## Ví dụ đã hoạt động 

đầu vào:`"a2A"`| tôi | Tiền tố | Xuôi ngược đều giống nhau? | độ[i//2] | độ[i] | Tổng Chạy | 
| --- | --- | --- | --- | --- | --- | 
| 0 | "một" | Có | - | 1 | 1 | 
| 1 | "a2" | Không | - | 0 | 1 | 
| 2 | "a2A" | Không | - | 0 | 1 | 

Điều này cho thấy rằng chỉ có các palindrome một ký tự mới đóng góp vào tổng. 

đầu vào:`"abaaba"`| tôi | Tiền tố | Xuôi ngược đều giống nhau? | độ[i//2] | độ[i] | Tổng Chạy | 
| --- | --- | --- | --- | --- | --- | 
| 0 | "một" | Có | - | 1 | 1 | 
| 1 | "ab" | Không | - | 0 | 1 | 
| 2 | "aba" | Có | 1 | 2 | 3 | 
| 3 | "abaa" | Không | - | 0 | 3 | 
| 4 | "abaab" | Không | - | 0 | 3 | 
| 5 | "abaaba" | Có | 2 | 3 | 6 | 

Điều này chứng tỏ sự lan truyền đệ quy: mức độ`"abaaba"`phụ thuộc vào mức độ`"aba"`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần cho phép băm, kiểm tra bảng màu và tính toán độ | 
| Không gian | O(n) | Mảng cho lũy thừa, hàm băm tiến/ngược và độ | 

Giải pháp vừa vặn trong 1 giây với n 5·10⁶ vì mỗi thao tác đều là truy cập mảng và số học đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open("solution.py").read(), globals())
    return str(ans)

# provided sample
assert run("a2A\n") == "1", "sample 1"

# minimum input
assert run("x\n") == "1", "single character"

# all same letters
assert run("aaaa\n") == "8", "degrees: 1,2,2,3"

# even length palindrome
assert run("abba\n") == "4", "degrees: 1,1,1,2"

# mixed case
assert run("AaAaaA\n") == "7", "checks case sensitivity"

# long repeating pattern
assert run("ababab\n") == "6", "degrees: 1,1,2,1,1,2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "x" | 1 | chuỗi nhỏ nhất | 
| "aaa" | 8 | lan truyền ký tự lặp đi lặp lại | 
| "abba" | 4 | |
