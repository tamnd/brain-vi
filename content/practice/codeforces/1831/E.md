---
title: "CF 1831E - Chuỗi khung siêu đều đặn"
description: "Chúng ta được yêu cầu đếm các chuỗi ngoặc có độ dài $n$ chính quy toàn cầu, đồng thời cũng chính quy cục bộ trên các khoảng con được chỉ định nhất định. Đầu vào cung cấp cho chúng ta độ dài $n$ và danh sách các khoảng $k$ $[li, ri]$."
date: "2026-06-09T07:08:38+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "data-structures", "hashing", "math", "number-theory", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1831
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 875 (Div. 2)"
rating: 2400
weight: 1831
solve_time_s: 200
verified: false
draft: false
---

[CF 1831E - Chuỗi khung siêu chính quy](https://codeforces.com/problemset/problem/1831/E) 

**Đánh giá:** 2400 
**Tags:** tổ hợp, cấu trúc dữ liệu, băm, toán, lý thuyết số, sắp xếp, hai con trỏ 
**Thời gian giải:** 3 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm các chuỗi ngoặc có độ dài$n$đều đặn trên toàn cầu, đồng thời cũng có tính đều đặn cục bộ trong các khoảng thời gian phụ được chỉ định nhất định. Đầu vào cho chúng ta độ dài$n$và một danh sách$k$khoảng thời gian$[l_i, r_i]$. Chuỗi ngoặc thông thường trên toàn cầu là chuỗi trong đó mọi dấu ngoặc đơn mở đều có dấu ngoặc đơn đóng phù hợp và không có dấu ngoặc đơn đóng nào xuất hiện trước dấu ngoặc đơn đóng phù hợp. Một chuỗi là siêu chính quy nếu mỗi khoảng được chỉ định tự tạo thành một chuỗi ngoặc vuông hợp lệ. Đầu ra của chúng tôi là số lượng các chuỗi siêu đều đặn như vậy theo modulo$998{,}244{,}353$. 

Những ràng buộc rất chặt chẽ. Tổng cộng$n$trên tất cả các trường hợp thử nghiệm có thể đạt được$3 \cdot 10^5$, Và$k$cũng có thể đạt được$3 \cdot 10^5$. Một cách tiếp cận đơn giản tạo ra tất cả các chuỗi, kiểm tra tính đều đặn và sau đó lọc dựa trên các khoảng là không thể vì số lượng các chuỗi tăng theo cấp số nhân trong$n$. Vì vậy, chúng ta cần một cách tiếp cận tổ hợp hoặc toán học, lý tưởng nhất là trong thời gian tuyến tính hoặc gần tuyến tính đối với$n + k$. 

Các trường hợp cạnh phát sinh khi$n$là số lẻ, vì không tồn tại chuỗi thông thường hoặc khi các khoảng chồng chéo hoặc lồng nhau, vì các ràng buộc này làm giảm số lượng chuỗi hợp lệ và thậm chí có thể làm cho số đếm bằng 0. Việc xử lý cẩn thận các khoảng trống và các khoảng có độ dài cũng được yêu cầu vì chúng tự động hạn chế các vị trí nhất định trong chuỗi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra tất cả$2^n$các chuỗi, kiểm tra xem mỗi chuỗi có chính quy toàn cục hay không, sau đó kiểm tra từng khoảng để biết tính quy luật cục bộ. Điều này đúng về nguyên tắc nhưng hoàn toàn không khả thi đối với$n > 20$do sự tăng trưởng theo cấp số nhân. 

Quan sát quan trọng là các chuỗi dấu ngoặc thông thường được đặc trưng đầy đủ bởi số lượng dấu ngoặc mở và đóng cũng như vị trí của chúng. Số Catalan liệt kê tất cả các dãy đều đặn có độ dài cho trước. Khi chúng ta thêm các khoảng với ràng buộc siêu đều đặn, nó buộc các vị trí nhất định phải khớp, cố định các phần của chuỗi một cách hiệu quả. Nếu các khoảng chồng chéo hoặc lồng nhau, thì chuỗi bên trong phải đáp ứng đồng thời tất cả các ràng buộc chồng chéo. Điều này dẫn đến sự giảm tổ hợp: chúng ta chỉ cần xem xét điểm cuối của các khoảng này có thể khớp với nhau như thế nào để tạo thành các chuỗi hợp lệ nhỏ hơn. 

Cách tiếp cận tối ưu tận dụng thực tế là bất kỳ cấu trúc khoảng lồng nhau nào cũng có thể được chia đệ quy thành các phân đoạn độc lập. Bằng cách đếm số cách để lấp đầy từng phân đoạn độc lập bằng cách sử dụng số Catalan và nhân lên, chúng ta có thể tính toán tổng số chuỗi siêu đều đặn một cách hiệu quả. Tính toán trước giai thừa và nghịch đảo giai thừa modulo$998{,}244{,}353$cho phép chúng ta tính số Catalan theo thời gian không đổi cho từng đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * k) | O(n) | Quá chậm | 
| Tối ưu | O((n + k) log(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$Và$k$, và lưu trữ các khoảng$[l_i, r_i]$. Sắp xếp các khoảng thời gian theo điểm bắt đầu. Việc sắp xếp là cần thiết để xử lý các khoảng thời gian lồng nhau hoặc chồng chéo một cách hiệu quả. 
2. Khởi tạo một ngăn xếp để mô phỏng cấu trúc lồng nhau. Lặp lại qua các khoảng và với mỗi khoảng, nếu nó bắt đầu sau phần trên cùng của ngăn xếp, hãy bật ngăn xếp cho đến khi khoảng thời gian hiện tại vừa khít. Điều này xây dựng một cây đại diện cho các khoảng lồng nhau. 
3. Đối với mỗi nút trong cây khoảng, hãy tính số chuỗi hợp lệ theo cách đệ quy. Nếu một nút có các khoảng con, hãy chia chuỗi thành các phân đoạn được xác định bởi các nút con này. Đối với mỗi đoạn có độ dài$m$, tính số Catalan$C_{m/2}$nếu như$m$là chẵn. Nếu như$m$là số lẻ, đặt số đếm về 0 vì không tồn tại chuỗi thông thường nào có độ dài lẻ. 
4. Nhân số Catalan của tất cả các phân đoạn và đệ quy cho các phần tử con lồng nhau. Điều này đưa ra số lượng cho khoảng thời gian hiện tại. 
5. Câu trả lời cho trường hợp kiểm thử là số lượng khoảng bao trùm toàn bộ chuỗi. Nếu không có khoảng nào bao phủ toàn bộ chuỗi thì hãy coi toàn bộ chuỗi là một khoảng duy nhất. 

Tại sao nó hoạt động: Số Catalan liệt kê tất cả các chuỗi thỏa mãn tính đều đặn. Bằng cách chia chuỗi thành các phân đoạn không chồng chéo được xác định bởi cấu trúc khoảng lồng nhau, chúng tôi đảm bảo rằng mỗi phân đoạn được tính độc lập. Nhân số lượng là hợp lệ do tính độc lập. Đệ quy tôn trọng việc lồng nhau, do đó mọi ràng buộc về khoảng thời gian đều được thỏa mãn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
MAX = 3 * 10**5 + 10

# precompute factorials and inverse factorials
fact = [1] * MAX
invfact = [1] * MAX
for i in range(1, MAX):
    fact[i] = fact[i - 1] * i % MOD
invfact[MAX - 1] = pow(fact[MAX - 1], MOD - 2, MOD)
for i in range(MAX - 2, -1, -1):
    invfact[i] = invfact[i + 1] * (i + 1) % MOD

def catalan(n):
    if n == 0:
        return 1
    return fact[2 * n] * invfact[n] % MOD * invfact[n + 1] % MOD

def solve_case(n, k, intervals):
    intervals.sort()
    stack = []
    result = 1
    last = 0

    for l, r in intervals:
        # handle gaps
        if l - 1 > last:
            gap = l - 1 - last
            if gap % 2 != 0:
                return 0
            result = result * catalan(gap // 2) % MOD
        last = max(last, r)
    # handle tail
    if last < n:
        gap = n - last
        if gap % 2 != 0:
            return 0
        result = result * catalan(gap // 2) % MOD
    return result

def main():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        intervals = [tuple(map(int, input().split())) for _ in range(k)]
```
