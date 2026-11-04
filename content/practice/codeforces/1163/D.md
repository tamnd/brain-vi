---
title: "CF 1163D - Mật mã bí ẩn"
description: "Chúng ta được cấp một chuỗi c đại diện cho một mã không thể đọc được một phần. Một số vị trí trong c là các chữ cái viết thường có thể đọc được và các vị trí khác là dấu hoa thị đại diện cho các ký tự không xác định."
date: "2026-06-12T02:22:06+07:00"
tags: ["codeforces", "competitive-programming", "dp", "strings"]
categories: ["algorithms"]
codeforces_contest: 1163
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 558 (Div. 2)"
rating: 2100
weight: 1163
solve_time_s: 66
verified: true
draft: false
---

[CF 1163D - Mã bí ẩn](https://codeforces.com/problemset/problem/1163/D) 

**Xếp hạng:** 2100 
**Thẻ:** dp, chuỗi 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một chuỗi`c`đại diện cho một mã không thể đọc được một phần. Một số vị trí trong`c`là các chữ cái viết thường có thể đọc được và các chữ cái khác là dấu hoa thị đại diện cho các ký tự không xác định. Chúng tôi cũng được cung cấp hai chuỗi:`s`, mà chúng tôi muốn xuất hiện nhiều lần nhất có thể trong mã được khôi phục cuối cùng và`t`, mà chúng tôi muốn giảm thiểu trong cùng một chuỗi. Nhiệm vụ là thay thế các dấu hoa thị trong`c`bằng chữ cái viết thường để tối đa hóa sự khác biệt giữa số lần xuất hiện của`s`Và`t`. 

Chuỗi`c`có thể dài tới 1000 ký tự, trong khi`s`Và`t`dài tối đa 50 ký tự. Bởi vì`c`tương đối nhỏ nên có thể thực hiện quy hoạch động qua các vị trí của`c`. Thách thức chính là mỗi lần thay thế trong dấu hoa thị sẽ ảnh hưởng đến nhiều kết quả khớp chuỗi con có thể có của`s`Và`t`và các trường hợp trùng lặp phải được xử lý cẩn thận. Một cách tiếp cận ngây thơ thử tất cả các khả năng thay thế có thể sẽ yêu cầu kiểm tra`26^1000`những khả năng có thể xảy ra trong trường hợp xấu nhất, vốn rất lớn về mặt thiên văn và hoàn toàn không khả thi. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó`s`Và`t`trùng lặp một phần hoặc trong đó mọi ký tự trong`c`là một dấu hoa thị. Ví dụ, nếu`c = "***"`,`s = "aa"`, Và`t = "aaa"`, một cách tiếp cận tham lam bất cẩn nhằm tối đa hóa`s`cục bộ có thể vô tình tạo ra sự xuất hiện thêm của`t`, làm giảm tổng điểm. Một trường hợp cạnh khác là khi`s`Và`t`chia sẻ tiền tố chung; tối đa hóa`s`có thể đồng thời tạo ra sự xuất hiện của`t`nếu sự chồng chéo bị bỏ qua. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ tạo ra tất cả các khả năng thay thế dấu hoa thị và đếm số lần xuất hiện của`s`Và`t`cho mỗi chuỗi ứng cử viên. Điều này hoạt động với các chuỗi cực nhỏ, nhưng với`|c|`lên tới 1000, có thể có tới`26^1000`chuỗi ứng cử viên, điều này hoàn toàn không khả thi. 

Quan sát quan trọng là vấn đề có thể được giảm xuống thành lập trình động với automata khớp chuỗi. Chúng ta có thể duy trì một bảng DP`dp[pos][state_s][state_t]`Ở đâu`pos`là vị trí hiện tại trong`c`,`state_s`là độ dài tiền tố của`s`phù hợp cho đến nay, và`state_t`là độ dài tiền tố của`t`phù hợp cho đến nay. Với mỗi ký tự trong`c`(hoặc mỗi lần thay thế nếu đó là dấu hoa thị), chúng tôi cập nhật các trạng thái theo sự chuyển đổi của một máy tự động hữu hạn theo dõi mức độ`s`Và`t`đã được khớp. Khi đầy đủ`s`được khớp, chúng tôi tăng số điểm lên 1 và khi đầy đủ`t`trùng khớp thì chúng ta giảm điểm đi 1. 

Việc xây dựng máy tự động một cách hiệu quả dựa vào hàm lỗi Knuth-Morris-Pratt. Điều này cho phép chúng ta tính toán theo thời gian không đổi, đối với một trạng thái nhất định và ký tự tiếp theo, trạng thái tiếp theo trong máy tự động. Với cách tiếp cận này, chúng tôi giảm vấn đề cấp số nhân cho DP trong`n * |s| * |t|`tiểu bang với`O(26)`chuyển đổi trên mỗi trạng thái, điều này khả thi đối với các ràng buộc nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(26^n * n) | O(n) | Quá chậm | 
| Máy tự động DP | O(n * | s | * | 

## Hướng dẫn thuật toán 

1. Tính toán trước hàm lỗi cho`s`Và`t`để cho phép chuyển đổi nhanh chóng giữa các trận đấu một phần. Đối với một chuỗi`p`,`fail[i]`là độ dài của tiền tố dài nhất của`p`đó là hậu tố của`p[0:i]`. Điều này cho chúng tôi biết, với một phần khớp và ký tự tiếp theo, độ dài khớp mới là bao nhiêu. 
2. Xây dựng bảng chuyển tiếp`next_s[state][char]`Và`next_t[state][char]`vì`s`Và`t`tương ứng.`next_s[state][char]`cho chúng ta biết độ dài của tiền tố mới của`s`phù hợp sau khi nhìn thấy`char`cho rằng chúng tôi đã phù hợp`state`nhân vật cho đến nay. Điều tương tự cũng áp dụng cho`t`. 
3. Khởi tạo bảng DP`dp[pos][state_s][state_t]`chứa đầy âm vô cực ngoại trừ`dp[0][0][0] = 0`. Đây`pos`là chỉ số trong`c`,`state_s`có bao nhiêu ký tự của`s`chúng tôi đã kết thúc phù hợp tại`pos-1`, Và`state_t`có bao nhiêu ký tự của`t`chúng tôi đã kết thúc phù hợp tại`pos-1`. 
4. Lặp lại các vị trí`pos`từ 0 đến`len(c)-1`. Đối với mỗi`(state_s, state_t)`, hãy thử tất cả các ký tự tiếp theo có thể. Nếu như`c[pos]`là một chữ cái đã biết thì chỉ có chữ cái đó mới được xem xét; nếu không thì tất cả 26 chữ cái đều được xem xét. 
5. Đối với mỗi nhân vật`ch`, tính các trạng thái tiếp theo`new_s = next_s[state_s][ch]`Và`new_t = next_t[state_t][ch]`. Cập nhật DP:`dp[pos+1][new_s][new_t] = max(dp[pos+1][new_s][new_t], dp[pos][state_s][state_t] + score)`, Ở đâu`score`là 1 nếu`new_s`đạt đầy đủ`|s|`(trừ`|s|`từ`new_s`sau khi đếm) và -1 nếu`new_t`đạt đầy đủ`|t|`(trừ`|t|`từ`new_t`sau khi đếm). 
6. Sau khi điền vào bảng DP, câu trả lời là giá trị lớn nhất trên tất cả`dp[len(c)][*][*]`. 

Tại sao nó hoạt động: DP duy trì tính bất biến`dp[pos][state_s][state_t]`là mức tối đa có thể đạt được`f(c', s) - f(c', t)`sử dụng cái đầu tiên`pos`nhân vật của`c`và kết thúc bằng các trận đấu một phần`state_s`Và`state_t`. Quá trình chuyển đổi tự động đảm bảo rằng các lần xuất hiện chồng chéo được xử lý chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def compute_kmp_transition(pattern):
    m = len(pattern)
    fail = [0] * m
    for i in range(1, m):
        j = fail[i-1]
        while j > 0 and pattern[i] != pattern[j]:
            j = fail[j-1]
        if pattern[i] == pattern[j]:
            j += 1
        fail[i] = j
    nxt = [[0]*26 for _ in range(m+1)]
    for state in range(m+1):
        for c in range(26):
            if state < m and ord(pattern[state]) - ord('a') == c:
                nxt[state][c] = state + 1
            else:
                j = state
                while j > 0 and (j == m or ord(pattern[j]) - ord('a') != c):
                    j = fail[j-1]
                if j < m and ord(pattern[j]) - ord('a') == c:
                    nxt[state][c] = j + 1
                else:
                    nxt[state][c] = 0
    return nxt

def main():
    c = input().strip()
    s = input().strip()
    t = input().strip()
    n = len(c)
    ns = len(s)
    nt = len(t)
    nxt_s = compute_kmp_transition(s)
    nxt_t = compute_kmp_transition(t)
    INF = float('-inf')
    dp = [[ [INF]*(nt+1) for _ in range(ns+1)] for _ in range(n+1)]
    dp[0][0][0] = 0
    for i in range(n):
        for ps in range(ns+1):
            for pt in range(nt+1):
                if dp[i][ps][pt] == INF:
                    continue
                chars = [ord(c[i])-ord('a')] if c[i] != '*' else list(range(26))
                for ch in chars:
                    ns_state = nxt_s[ps][ch]
                    nt_state = nxt_t[pt][ch]
                    score = 0
                    if ns_state == ns:
                        score += 1
                        ns_state = nxt_s[ns_state-1][ch]
                    if nt_state == nt:
                        score -= 1
                        nt_state = nxt_t[nt_state-1][ch]
                    dp[i+1][ns_state][nt_state] = max(dp[i+1][ns_state][nt_state], dp[i][ps][pt] + score)
    result = max(dp[n][ps][pt] for ps in range(ns+1) for pt in range(nt+1))
    print(result)

if __name__ == "__main__":
    main()
```các`compute_kmp_transition`hàm tính toán trước các chuyển đổi trạng thái tiếp theo cho từng tiền tố của mẫu cho từng ký tự có thể. Bảng DP`dp[i][ps][pt]`theo dõi số điểm tốt nhất có thể đạt được cho mỗi sự kết hợp của các trận đấu từng phần. Khi xử lý dấu hoa thị, tất cả 26 chữ cái đều được thử, trong khi một ký tự đã biết buộc phải chuyển đổi một lần. Cập nhật DP với`max`đảm bảo chúng tôi luôn giữ được sự lựa chọn tối ưu. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
*****
katie
shiro
```| tôi | ps | pt | dp[i][ps][pt] | ch đã thử | ns_state | nt_state | dp mới | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | | | | | | | |
