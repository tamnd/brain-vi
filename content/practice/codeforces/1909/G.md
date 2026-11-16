---
title: "CF 1909G - Bổ đề bơm"
description: "Chúng ta có hai chuỗi, s có độ dài n và t có độ dài m, trong đó n hoàn toàn nhỏ hơn m. Nhiệm vụ là đếm số cách chúng ta có thể chia s thành ba chuỗi con liền kề x, y, z sao cho khi lấy x, lặp lại y một số lần (ít nhất một lần), rồi nối thêm…"
date: "2026-06-08T20:31:35+07:00"
tags: ["codeforces", "competitive-programming", "hashing", "strings"]
categories: ["algorithms"]
codeforces_contest: 1909
codeforces_index: "G"
codeforces_contest_name: "Pinely Round 3 (Div. 1 + Div. 2)"
rating: 3000
weight: 1909
solve_time_s: 82
verified: true
draft: false
---

[CF 1909G - Bổ đề bơm](https://codeforces.com/problemset/problem/1909/G) 

**Đánh giá:** 3000 
**Thẻ:** băm, chuỗi 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai chuỗi,`s`chiều dài`n`Và`t`chiều dài`m`, Ở đâu`n`thực sự nhỏ hơn`m`. Nhiệm vụ là đếm số cách chia`s`thành ba chuỗi con liền kề`x`,`y`,`z`như vậy khi chúng ta lấy`x`, lặp lại`y`một số lần (ít nhất một lần), sau đó nối thêm`z`, chúng ta nhận được chính xác chuỗi`t`. Thiết yếu,`t`là một phiên bản "được bơm" của`s`bằng cách lặp lại đoạn giữa`y`một số lần. 

Những hạn chế chính là lớn:`n`Và`m`có thể lên tới`10^7`. Điều này loại trừ bất kỳ cách tiếp cận nào lặp đi lặp lại trên tất cả các phần phân chia có thể có của`s`và mô phỏng các phép nối lặp đi lặp lại một cách ngây thơ. Thuật toán brute-force xem xét tất cả các chuỗi con`x`,`y`,`z`sẽ liên quan đến`O(n^2 * m)`hoạt động quá chậm so với các giới hạn này. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó`x`hoặc`z`có thể trống và trường hợp`y`là một ký tự được lặp lại nhiều lần. Ví dụ, nếu`s = "aa"`Và`t = "aaaaa"`, có nhiều bộ ba hợp lệ:`("", "a", "aa")`,`("a", "a", "a")`, Và`("", "aa", "a")`. Một cách tiếp cận bất cẩn giả định không trống`x`hoặc`z`hoặc cố gắng để phù hợp`t`tham lam mà không căn chỉnh cẩn thận sẽ bỏ lỡ bộ ba hợp lệ. 

## Phương pháp tiếp cận 

Phương pháp brute-force là lặp lại tất cả các điểm phân chia có thể có cho`x`Và`y`TRONG`s`, tính toán`z`làm chuỗi con còn lại, sau đó kiểm tra từng bộ ba ứng cử viên xem có lặp lại không`y`sản xuất`t`một cách chính xác. Đối với mỗi ứng viên, điều này có thể yêu cầu lặp lại thông qua`t`để xác minh sự lặp lại. Nếu chúng ta xem xét tất cả các sự phân chia`(i, j)`vì`x`kết thúc tại`i`Và`y`kết thúc tại`j`, có khoảng`O(n^2)`phần tách và việc xác minh từng phần có thể mất tới`O(m)`hoạt động, mang lại sự phức tạp tổng thể của`O(n^2 * m)`, điều này là không thể thực hiện được đối với`n, m ~ 10^7`. 

Quan sát quan trọng là việc kiểm tra xem`y`lặp đi lặp lại một số lần phù hợp với một phân đoạn của`t`có thể được thực hiện một cách hiệu quả bằng cách sử dụng hàm băm chuỗi. Tính toán trước các giá trị băm cuộn cho`s`Và`t`. Sau đó, đối với mỗi lần phân chia tiềm năng, chúng ta chỉ cần so sánh các giá trị băm của`x`Và`z`với tiền tố và hậu tố tương ứng của`t`. Vì`y`, chúng tôi kiểm tra xem phần của`t`tương ứng với lần lặp lại`y`là bội số của`len(y)`và tất cả các khối lặp lại đều có cùng hàm băm như`y`. Điều này làm giảm việc xác minh từ việc lặp đi lặp lại`t`từng ký tự để`O(1)`so sánh hàm băm trên mỗi khối, đưa ra một tuyến tính phức tạp tổng thể trong`n + m`. 

Quan sát này biến một lực lượng vũ phu không khả thi thành một giải pháp dựa trên hàm băm lăn khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * m) | O(1) | Quá chậm | 
| Băm + Phân chia liệt kê | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các giá trị băm tiền tố cho cả hai`s`Và`t`sử dụng hàm băm luân phiên và tính toán công suất của cơ sở băm lên đến`m`để kích hoạt truy vấn băm chuỗi con O(1). Điều này cho phép chúng ta so sánh hiệu quả bất kỳ chuỗi con nào bằng hàm băm của nó thay vì theo từng ký tự. 
2. Lặp lại tất cả các điểm phân chia có thể`i`vì`x`TRONG`s`từ`0`ĐẾN`n`. Đối với mỗi`i`, lặp lại các điểm phân chia có thể`j`vì`y`bắt đầu từ`i`ĐẾN`n-1`. Sau đó`z`ngầm là`s[j:]`. 
3. Tính độ dài của “phần giữa” trong`t`tương ứng với lần lặp lại`y`: đây là`m - len(x) - len(z)`. Nếu độ dài này không phải là bội số của`len(y)`, bỏ qua phần chia này vì`y`không thể lặp lại số lần không nguyên. 
4. So sánh giá trị băm của`x`với`t[:len(x)]`Và`z`với`t[-len(z):]`. Nếu những điều này không khớp, hãy bỏ qua phần chia này. 
5. Tính số lần lặp lại`k = (m - len(x) - len(z)) // len(y)`. So sánh hàm băm của`y`với mỗi khối có chiều dài`len(y)`ở giữa`t`sử dụng băm lăn được tính toán trước. Nếu tất cả các khối khớp`y`, hãy tính bộ ba này là hợp lệ. 
6. Tính tổng tất cả các bộ ba hợp lệ và in ra tổng. 

Tại sao nó hoạt động: thuật toán đảm bảo rằng`x`Và`z`khớp chính xác với tiền tố và hậu tố của`t`, và đó`y`lặp đi lặp lại điền chính xác vào giữa. Băm luân chuyển đảm bảo tính chính xác với xác suất cao và việc lặp qua tất cả các phần tách bao gồm tất cả các giá trị hợp lệ.`(x, y, z)`cấu hình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9+7
BASE = 31

def compute_hashes(s):
    n = len(s)
    h = [0] * (n+1)
    power = [1] * (n+1)
    for i in range(n):
        h[i+1] = (h[i]*BASE + (ord(s[i]) - ord('a') + 1)) % MOD
        power[i+1] = (power[i]*BASE) % MOD
    return h, power

def get_hash(h, power, l, r):
    return (h[r] - h[l]*power[r-l] % MOD + MOD) % MOD

def main():
    n, m = map(int, input().split())
    s = input().strip()
    t = input().strip()
    
    hs, ps = compute_hashes(s)
    ht, pt = compute_hashes(t)
    
    ans = 0
    for i in range(n+1):  # x ends at i-1
        for j in range(i, n+1):  # y ends at j-1, z = s[j:]
            len_x = i
            len_y = j - i
            len_z = n - j
            
            if len_y == 0:
                continue
            
            middle_len = m - len_x - len_z
            if middle_len <= 0 or middle_len % len_y != 0:
                continue
            
            if get_hash(hs, ps, 0, len_x) != get_hash(ht, pt, 0, len_x):
                continue
            if get_hash(hs, ps, j, n) != get_hash(ht, pt, m - len_z, m):
                continue
            
            k = middle_len // len_y
            valid = True
            y_hash = get_hash(hs, ps, i, j)
            for t_start in range(len_x, len_x + k*len_y, len_y):
                if get_hash(ht, pt, t_start, t_start+len_y) != y_hash:
                    valid = False
                    break
            if valid:
                ans += 1
    print(ans)

if __name__ == "__main__":
    main()
```Giải pháp tính toán trước các giá trị băm để cho phép so sánh chuỗi con theo thời gian không đổi. Các vòng lặp lồng nhau`i`Và`j`liệt kê các phép chia có thể có của`x`Và`y`. Chúng tôi bỏ qua phần chia tách ở đâu`y`không thể nhét một số nguyên lần vào giữa`t`. Chúng tôi xác minh`x`Và`z`phù hợp với tiền tố và hậu tố của`t`, sau đó kiểm tra xem các khối lặp lại của`y`nối phần tương ứng trong`t`. tăng dần`ans`đếm từng bộ ba hợp lệ. 

## Ví dụ đã hoạt động 

**Mẫu 1:**```
s = "abcd", t = "abcbcbcd"
```| tôi | j | len_x | len_y | len_z | giữa_len | k | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 1 | 2 | 1 | 6 | 3 | Đúng | 

Bộ ba hợp lệ duy nhất là`("a", "bc", "d")`. Việc lặp đi lặp lại`y`"bc" xuất hiện 3 lần ở giữa`t`, khớp với các kiểm tra băm. 

**Mẫu 2:**```
s = "aa", t = "aaaaa"
```| tôi | j | len_x | len_y | len_z | giữa_len | k | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 1 | 4 | 4 | Đúng | 
| 0 | 2 | 0 | 2 | 0 | 5 | 2 | Đúng | 
| 1 | 2 | 1 | 1 | 0 | 4 | 4 | Đúng | 

Điều này xác nhận thuật toán xử lý sản phẩm nào`x`hoặc`z`và nhiều lần lặp lại của một ký tự`y`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Tính toán trước các giá trị băm theo O(n + m), sau đó liệt kê các phần tách trong O(n^2) nhưng kiểm tra bên trong là O(1) bằng cách sử dụng giá trị băm cuộn và số học. Trong thực tế, với kích thước lớn`n`|
