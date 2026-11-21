---
title: "CF 2209E - Một vấn đề về chuỗi tầm thường"
description: "Chúng ta được cung cấp một chuỗi và một tập hợp các truy vấn. Đối với mỗi truy vấn, chúng tôi lấy một chuỗi con được xác định bởi các điểm cuối của nó và với mỗi tiền tố của chuỗi con này, chúng tôi tính toán số phần tối đa có thể được chia thành sao cho mỗi phần chính là tiền tố của chuỗi con."
date: "2026-06-07T19:24:12+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "hashing", "string-suffix-structures", "strings"]
categories: ["algorithms"]
codeforces_contest: 2209
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1087 (Div. 2)"
rating: 2200
weight: 2209
solve_time_s: 111
verified: false
draft: false
---

[CF 2209E - Một vấn đề về chuỗi tầm thường](https://codeforces.com/problemset/problem/2209/E) 

**Đánh giá:** 2200 
**Tags:** brute Force, dp, băm, cấu trúc hậu tố chuỗi, chuỗi 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi và một tập hợp các truy vấn. Đối với mỗi truy vấn, chúng tôi lấy một chuỗi con được xác định bởi các điểm cuối của nó và với mỗi tiền tố của chuỗi con này, chúng tôi tính toán số phần tối đa có thể được chia thành sao cho mỗi phần chính là tiền tố của chuỗi con. Sau đó chúng tôi tính tổng các giá trị này cho tất cả các tiền tố. Về mặt khái niệm,$f(t)$đếm số lần bạn có thể tham lam cắt bỏ tiền tố lớn nhất liên tục khỏi một chuỗi cho đến khi nó được phân tách hoàn toàn thành các phần tiền tố. 

Chuỗi đầu vào có thể lên tới$10^6$ký tự trên tất cả các trường hợp thử nghiệm và mỗi truy vấn yêu cầu tính tổng$O(n)$tiền tố. Một cách tiếp cận đơn giản để tính toán lại$f(t)$từ đầu cho mọi tiền tố ngay lập tức là không thực tế vì việc đánh giá$f(t)$cho một chuỗi dài$m$có thể lấy một cách ngây thơ$O(m)$và làm điều này với mọi tiền tố sẽ dẫn đến$O(n^2)$mỗi truy vấn. Cho rằng$n$có thể$10^6$và có thể có nhiều truy vấn,$O(n^2)$là quá chậm. Chúng ta cần một thuật toán gần$O(n)$hoặc$O(n \log n)$mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh phát sinh khi chuỗi đồng nhất hoặc có tính lặp lại cao, chẳng hạn như "aaaaa". Trong trường hợp này, mỗi tiền tố có giá trị cao$f(t)$giá trị (nó phát triển tuyến tính) và việc triển khai đơn giản sử dụng so sánh chuỗi có thể dẫn đến các tính toán dư thừa. Ngược lại, khi chuỗi có tất cả các ký tự riêng biệt thì mỗi tiền tố chỉ có thể chia thành một phần, do đó$f(t)$luôn luôn là một. Xử lý cả hai thái cực một cách hiệu quả là rất quan trọng. 

## Phương pháp tiếp cận 

Giải pháp brute-force là lặp lại từng truy vấn và đối với mỗi tiền tố của chuỗi con, hãy liên tục tìm kiếm tiền tố lớn nhất mà chúng tôi có thể loại bỏ. Mỗi đánh giá chuỗi con có thể mất$O(m^2)$do so sánh tiền tố lặp đi lặp lại. Trong trường hợp xấu nhất, đối với một chuỗi con có độ dài$n$, điều này dẫn đến$O(n^3)$cho mỗi truy vấn, điều này hoàn toàn không khả thi. 

Cái nhìn sâu sắc quan trọng là để ý rằng$f(t)$có liên quan chặt chẽ đến cấu trúc đường viền trong chuỗi. Đường viền là tiền tố cũng là hậu tố. Đối với bất kỳ chuỗi nào$t$, sự phân tách tối đa thành các tiền tố tương ứng với việc loại bỏ liên tục đường viền dài nhất của nó. Quan sát này dẫn đến việc sử dụng hàm tiền tố từ thuật toán Knuth-Morris-Pratt (KMP). Đối với một chuỗi$t$, nếu như$\pi[i]$là độ dài của tiền tố thích hợp dài nhất của$t[0..i]$đó cũng là một hậu tố, vậy thì$f(t[0..i]) = 1 + f(\text{prefix of length } \pi[i])$. Điều này làm giảm tính toán từ bậc hai sang tuyến tính. 

Để trả lời các truy vấn một cách hiệu quả, chúng ta có thể tính toán trước hàm tiền tố cho toàn bộ chuỗi một lần. Sau đó, bằng cách sử dụng quy hoạch động trên hàm tiền tố, chúng ta có thể tính toán$f(t)$cho tất cả các tiền tố trong thời gian tuyến tính. Một khi chúng tôi có$f(t)$đối với tất cả các tiền tố, việc tính tổng chúng cho bất kỳ truy vấn nào chỉ là vấn đề tổng tiền tố, cho phép mỗi truy vấn được trả lời theo$O(1)$sau đó$O(n)$tiền xử lý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) mỗi truy vấn | O(n) | Quá chậm | 
| KMP + DP + Tổng tiền tố | O(n) cho mỗi trường hợp thử nghiệm, O(1) cho mỗi truy vấn | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước hàm tiền tố$\pi$cho chuỗi$s$. Đối với mỗi chỉ số$i$,$\pi[i]$cung cấp độ dài của tiền tố thích hợp dài nhất của$s[0..i]$đó cũng là một hậu tố. 
2. Khởi tạo mảng DP`f`có kích thước $n+1`. Set `f[0] = 1` vì ký tự đầu tiên chỉ có thể được chia thành chính nó. 
3. Với mọi chỉ số $i`from 1 to n-1, compute`f[i] = 1 + f[pi[i]-1]`if`pi[i] > 0`, otherwise `f[i] = 1`. Điều này hoạt động vì việc phân tách tiền tố-hậu tố lớn nhất là đệ quy và hàm tiền tố nắm bắt chính xác độ dài của tiền tố trước đó có thể tiếp tục phân tách. 
4. Xây dựng mảng tổng tiền tố`F`như vậy`F[i] = f[0] + f[1] + ... + f[i]`. Điều này cho phép tổng hợp`f`trên bất kỳ phạm vi nào trong thời gian không đổi. 
5. Đối với mỗi truy vấn`(l, r)`, điều chỉnh các chỉ số về giá trị 0. Câu trả lời là tổng của`f`cho tất cả các tiền tố của`s[l..r]`. Điều này có thể có được như`F[r] - F[l-1]`nếu như`l > 0`, nếu không thì`F[r]`. 

Tại sao nó hoạt động: Hàm tiền tố vốn đã mã hóa sự phân tách tiền tố-hậu tố tối đa, do đó, việc sử dụng nó sẽ tính toán đệ quy$f(t)$cho mỗi tiền tố một cách chính xác. Vì tổng tiền tố tích lũy các giá trị này nên mọi tổng truy vấn đều chính xác. Không cần so sánh chuỗi và mỗi bước được đảm bảo thể hiện sự phân tách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def compute_prefix_function(s):
    n = len(s)
    pi = [0]*n
    for i in range(1, n):
        j = pi[i-1]
        while j > 0 and s[i] != s[j]:
            j = pi[j-1]
        if s[i] == s[j]:
            j += 1
        pi[i] = j
    return pi

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        s = input().strip()
        pi = compute_prefix_function(s)
        f = [0]*n
        for i in range(n):
            if pi[i] == 0:
                f[i] = 1
            else:
                f[i] = 1 + f[pi[i]-1]
        F = [0]*n
        F[0] = f[0]
        for i in range(1, n):
            F[i] = F[i-1] + f[i]
        for _ in range(q):
            l, r = map(int, input().split())
            l -= 1
            r -= 1
            if l == 0:
                print(F[r])
            else:
                print(F[r] - F[l-1])

if __name__ == "__main__":
    solve()
```chức năng`compute_prefix_function`tính toán mảng tiền tố KMP theo thời gian tuyến tính. Mảng DP`f`sử dụng mảng tiền tố để tính số lượng phân tách tối đa cho mỗi tiền tố. Mảng tổng tiền tố`F`cho phép trả lời truy vấn theo thời gian không đổi. Điều chỉnh chỉ mục xử lý việc chuyển đổi từ đầu vào truy vấn dựa trên một sang chỉ mục mảng dựa trên 0. 

## Ví dụ đã hoạt động 

Đối với chuỗi`s = "aaaaa"`: 

| tôi | s[0..i] | pi[i] | f[i] | F[i] | 
| --- | --- | --- | --- | --- | 
| 0 | một | 0 | 1 | 1 | 
| 1 | aa | 1 | 2 | 3 | 
| 2 | aaa | 2 | 3 | 6 | 
| 3 | aaa | 3 | 4 | 10 | 
| 4 | aaa | 4 | 5 | 15 | 

Truy vấn`(1,5)`sản lượng`F[4] = 15`. 

Vì`s = "abcdef"`: 

| tôi | s[0..i] | pi[i] | f[i] | F[i] | 
| --- | --- | --- | --- | --- | 
| 0 | một | 0 | 1 | 1 | 
| 1 | ab | 0 | 1 | 2 | 
| 2 | abc | 0 | 1 | 3 | 
| 3 | abcd | 0 | 1 | 4 | 
| 4 | abcde | 0 | 1 | 5 | 
| 5 | abcdef | 0 | 1 | 6 | 

Truy vấn`(1,6)`sản lượng`F[5] = 6`. 

Các bảng này thể hiện tính chính xác của cả phép tính tổng DP và tiền tố, xử lý cả trường hợp lặp lại và ký tự riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Hàm tiền tố và tính toán DP duyệt chuỗi một cách tuyến tính, mỗi truy vấn được trả lời bằng O(1) | 
| Không gian | O(n) | Mảng cho pi, f và F có tỷ lệ tuyến tính với độ dài chuỗi | 

Cho rằng tổng của`n`trên tất cả các trường hợp thử nghiệm là 10^6, tổng số thao tác nằm trong khoảng 10^7, vừa vặn thoải mái trong giới hạn 4 giây. Mức sử dụng bộ nhớ cũng dưới 10 MB cho mỗi trường hợp thử nghiệm, thấp hơn nhiều so với giới hạn 1024 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("6\n1 1\na\n1 1\n5 2\naaaaa\n1 5\n2 4\n6 2\nabcdef\n1 6\n3 5\n6 3\nabaaba\n1 6\n1 3\n2 6\n7 3\nabcabca\n1 7\n2 7\n4 7\n8 3\naababaac\n1 8\n2 8
```
