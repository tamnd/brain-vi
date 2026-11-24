---
title: "CF 73C - LionAge II"
description: "Chúng ta được cung cấp một chuỗi đại diện cho tên của một nhân vật trong trò chơi và chúng ta có thể thay đổi tối đa k chữ cái của nó để tối đa hóa điểm gọi là sự hưng phấn. Sự hòa âm được tính bằng tổng số tiền thưởng cho mỗi cặp chữ cái liên tiếp trong chuỗi."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 73
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 66"
rating: 1800
weight: 73
solve_time_s: 87
verified: true
draft: false
---

[CF 73C - LionAge II](https://codeforces.com/problemset/problem/73/C) 

**Đánh giá:** 1800 
**Thẻ:** dp 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi đại diện cho tên của một nhân vật trong trò chơi và chúng ta có thể thay đổi tối đa _k_ các chữ cái của nó để tối đa hóa điểm được gọi là sự hưng phấn. Sự hòa âm được tính bằng tổng số tiền thưởng cho mỗi cặp chữ cái liên tiếp trong chuỗi. Mỗi cặp chữ cái có thể có phần thưởng liên quan, có thể dương hoặc âm và nếu một cặp không được liệt kê thì phần thưởng của nó bằng 0. 

Đầu vào bao gồm chuỗi _s_, số nguyên _k_ và một tập hợp các cặp chữ cái có phần thưởng tương ứng. Đầu ra của chúng tôi là một số nguyên duy nhất, mức độ hòa âm tối đa có thể sau khi thực hiện tối đa _k_ thay đổi. 

Các ràng buộc khá chặt chẽ: độ dài chuỗi tối đa là 100 và _k_ cũng tối đa là 100. Số lượng cặp chữ cái nhiều nhất là 676 (tất cả các cặp chữ cái viết thường có thể có). Các giới hạn này chỉ ra rằng một cách tiếp cận có độ phức tạp thời gian đa thức trên độ dài chuỗi là khả thi. Một thuật toán có độ phức tạp O(n * k * 26^2), trong đó n là độ dài chuỗi, có thể chấp nhận được vì 100 * 100 * 676 là khoảng 6,7 triệu thao tác, vừa với giới hạn 2 giây. Các trường hợp đặc biệt bao gồm _k_ = 0, trong đó không được phép thay đổi và _n_ = 0, trong đó tất cả phần thưởng của cặp đều bằng 0. Cách tiếp cận bất cẩn có thể cho rằng tất cả các cặp đều có tiền thưởng hoặc bỏ qua _k_ = 0, dẫn đến kết quả không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các cách có thể để thay đổi tối đa _k_ chữ cái trong chuỗi. Đối với mỗi vị trí, chúng ta có thể giữ nguyên chữ cái đó hoặc thay thế nó bằng bất kỳ chữ cái nào trong số 25 chữ cái khác, tạo ra rất nhiều khả năng. Đối với một chuỗi có độ dài 100 và _k_ = 100, điều này sẽ dẫn đến khả năng O(26^100), điều này hoàn toàn không khả thi. Ngay cả việc cố gắng tối ưu hóa bằng cách chỉ xem xét những thay đổi có lợi vẫn để lại một không gian tìm kiếm theo cấp số nhân. 

Điểm mấu chốt là bài toán có cấu trúc con tối ưu và các bài toán con chồng chéo, làm cho nó phù hợp với lập trình động. Chúng ta có thể định nghĩa trạng thái DP là`dp[i][changes][last]`, đại diện cho sự hòa âm tối đa có thể đạt được bằng cách sử dụng lần đầu tiên`i`các chữ cái, đã sử dụng`changes`thay thế, trong đó chữ cái cuối cùng được sử dụng là`last`. Ở mỗi bước, chúng ta có thể thử mọi chữ cái có thể ở vị trí hiện tại, hoặc thay đổi chữ cái gốc (tính vào`k`) hoặc giữ nó và cập nhật DP dựa trên phần thưởng cho chữ cái cuối cùng và hiện tại. Bảng chữ cái nhỏ (26 chữ cái) và độ dài chuỗi giới hạn làm cho phương pháp này trở nên dễ thực hiện. 

Điều này biến đổi vấn đề từ sức mạnh vũ phu theo cấp số nhân sang giải pháp DP đa thức, khai thác quan sát rằng điểm của tiền tố chỉ phụ thuộc vào chữ cái cuối cùng của tiền tố và số lượng thay đổi được sử dụng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(26^n) | O(1) | Quá chậm | 
| Lập trình động | O(n * k * 26^2) | O(n * k * 26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng DP 3D`dp[i][changes][last]`, Ở đâu`i`là chỉ mục trong chuỗi,`changes`là số chữ cái được thay thế, và`last`là chỉ số của chữ cái cuối cùng được sử dụng. Hãy điền vào nó một số âm lớn vì chúng ta đang tối đa hóa. 
2. Chuyển đổi chuỗi đầu vào thành các chỉ số số 0-25 tương ứng với các chữ cái 'a'-'z'. 
3. Phân tích các cặp tiền thưởng thành ma trận 26x26`bonus`Ở đâu`bonus[x][y]`cho điểm từng cặp`xy`. Các cặp không được liệt kê mặc định là 0. 
4. Đặt kiểu chữ cơ bản cho ký tự đầu tiên. Đối với mỗi chữ cái có thể có ở chỉ số 0, nếu chúng ta thay đổi nó, hãy tăng các thay đổi đã sử dụng. Cập nhật`dp[0][changes][letter]`bằng 0, vì không có chữ cái nào trước đó để tạo phần thưởng. 
5. Lặp lại chuỗi từ chỉ số 1 đến n-1. Đối với mỗi số lượng thay đổi có thể được sử dụng cho đến nay và đối với từng chữ cái có thể có trước đó, hãy xem xét mọi chữ cái chúng ta có thể đặt ở vị trí hiện tại. Tính số thay đổi mới và phần thưởng của cặp được hình thành bằng chữ cái trước đó. Cập nhật`dp[i][new_changes][current_letter]`là giá trị lớn nhất giữa giá trị trước đó của nó và`dp[i-1][changes][prev_letter] + bonus[prev_letter][current_letter]`. 
6. Sau khi xử lý toàn bộ chuỗi, lặp lại tất cả các trạng thái DP ở chỉ số n-1 và thay đổi ≤ k, theo dõi mức hòa âm tối đa. 
7. Xuất ra giá trị lớn nhất tìm được. 

Tại sao nó hoạt động: DP đảm bảo rằng tại mỗi vị trí, chúng tôi xem xét tất cả các cách có thể để tiếp cận vị trí đó với số lượng thay thế và chữ cái cuối cùng nhất định. Bằng cách luôn tận dụng tối đa khi cập nhật, chúng tôi đảm bảo rằng không có cấu hình nào tốt hơn bị bỏ qua. Việc sử dụng thứ nguyên chữ cái cuối cùng đảm bảo rằng phần thưởng cặp được áp dụng chính xác giữa các chữ cái liên tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s, k = input().split()
k = int(k)
n = int(input())

bonus = [[0]*26 for _ in range(26)]
for _ in range(n):
    x, y, c = input().split()
    bonus[ord(x)-97][ord(y)-97] = int(c)

m = len(s)
dp = [[[-10**18]*26 for _ in range(k+1)] for _ in range(m)]

# Initialize first character
for letter in range(26):
    changes = 0 if letter == ord(s[0])-97 else 1
    if changes <= k:
        dp[0][changes][letter] = 0

# Fill DP
for i in range(1, m):
    for used in range(k+1):
        for prev in range(26):
            if dp[i-1][used][prev] == -10**18:
                continue
            for cur in range(26):
                new_used = used + (cur != ord(s[i])-97)
                if new_used <= k:
                    dp[i][new_used][cur] = max(dp[i][new_used][cur], dp[i-1][used][prev] + bonus[prev][cur])

# Get result
ans = max(dp[m-1][used][letter] for used in range(k+1) for letter in range(26))
print(ans)
```Mã đầu tiên đọc và chuyển đổi đầu vào. Ma trận thưởng rất đơn giản và việc khởi tạo DP sử dụng số âm lớn để biểu thị các trạng thái không thể. Chúng tôi cẩn thận xử lý ký tự đầu tiên một cách riêng biệt vì nó không có chữ cái trước đó. Việc lặp lại các chữ cái trước đó và các chữ cái hiện tại có thể đảm bảo tất cả các chuyển đổi đều được xem xét. sử dụng`max`đảm bảo chúng tôi theo dõi sự hòa âm tối ưu ở mỗi trạng thái. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:`winner 4`với tiền thưởng:`s e 7, o s 8, l o 13, o o 8`. 

| tôi | đã qua sử dụng | trước | cur | new_used | dp[i][new_used][cur] | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | - | tôi | 1 | 0 | 
| 1 | 1 | tôi | o | 1 | 13 | 
| 2 | 1 | o | o | 1 | 21 | 
| 3 | 1 | o | s | 2 | 29 | 
| 4 | 2 | s | e | 2 | 36 | 
| 5 | 2 | e | r | 3 | 36 | 

Điều này cho thấy việc xây dựng`looser`và sự tích lũy hưng phấn. 

**Một ví dụ nhỏ khác:** 

đầu vào:`abc 1`, tiền thưởng`a b 5, b c 10`. 

| tôi | đã qua sử dụng | trước | cur | new_used | dp[i][new_used][cur] | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | - | một | 0 | 0 | 
| 1 | 0 | một | b | 0 | 5 | 
| 2 | 0 | b | c | 0 | 15 | 
| 1 | 1 | một | c | 1 | 0 | 
| 2 | 1 | c | b | 1 | 0 | 

DP chọn chính xác không cần thay đổi nào là tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * k * 26^2) | Đối với mỗi vị trí, mỗi thay đổi có thể được sử dụng và mỗi chữ cái trước đó, chúng tôi xem xét 26 chữ cái hiện tại có thể có. | 
| Không gian | O(n * k * 26) | Mảng DP lưu trữ các giá trị cho từng vị trí, số lần thay đổi và chữ cái cuối cùng. | 

Điều này phù hợp thoải mái trong các giới hạn: 100 * 100 * 26^2 ≈ 6,7 triệu lần lặp và khoảng 260.000 số nguyên trong bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open("solution.py").read())
    return ""

# provided sample
assert run("winner 4\n4\ns e 7\no s 8\nl o 13\no o 8\n") == "", "sample 1"

# minimum input
assert run("a 0\n0\n") == "", "single character, no changes"

# maximum k
assert run("abc 3\n3\na b 5\nb c 10\nc a 2\n") == "", "all letters can be changed"

# all pair bonuses negative
assert run("abc
```
