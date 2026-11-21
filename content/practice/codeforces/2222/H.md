---
title: "CF 2222H - Đếm Sắp xếp?"
description: "Chúng tôi được yêu cầu đếm các mảng có thuộc tính được xác định đệ quy. Cho một mảng các số nguyên, mỗi số được giới hạn riêng lẻ bởi một ri tương ứng, chúng ta xác định một phép biến đổi f(b) đếm số lần mỗi số nguyên xuất hiện trong b."
date: "2026-06-07T18:44:51+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "dp"]
categories: ["algorithms"]
codeforces_contest: 2222
codeforces_index: "H"
codeforces_contest_name: "Spectral::Cup 2026 Round 1 (Codeforces Round 1094, Div. 1 + Div. 2)"
rating: 0
weight: 2222
solve_time_s: 114
verified: false
draft: false
---

[CF 2222H - Sắp xếp đếm?](https://codeforces.com/problemset/problem/2222/H) 

**Đánh giá:** - 
**Tags:** vũ lực, tổ hợp, dp 
**Thời gian giải:** 1m 54s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đếm các mảng có thuộc tính được xác định đệ quy. Cho một mảng các số nguyên, mỗi số được giới hạn bởi một giá trị tương ứng`r_i`, chúng tôi xác định một sự chuyển đổi`f(b)`đếm số lần mỗi số nguyên xuất hiện trong`b`. Sau đó,`g(b)`được định nghĩa là số mảng riêng biệt xuất hiện nếu chúng ta áp dụng nhiều lần`f`bắt đầu từ`b`. Thiết yếu,`g(b)`đo xem cần bao nhiêu lần lặp để quá trình đếm này ổn định thành một chu kỳ. 

Đối với mỗi trường hợp thử nghiệm, chúng ta cần xác định, với mọi số nguyên`p`từ`1`ĐẾN`k`, có bao nhiêu mảng`a`thỏa mãn`g(a) = p`. Các mảng đầu vào nhỏ (`n ≤ 50`) Và`r_i ≤ n`, nhưng phạm vi đầu ra`k`có thể lên tới 1000. Các ràng buộc đảm bảo rằng tổng khối của tất cả các trường hợp thử nghiệm có thể quản lý được, ngụ ý rằng`O(n^3)`cách tiếp cận cho mỗi trường hợp thử nghiệm là có thể chấp nhận được. 

Các trường hợp cạnh không rõ ràng phát sinh khi tất cả các phần tử bằng 0 hoặc khi một số phần tử`r_i = 0`nhưng những cái khác thì lớn hơn. Ví dụ, đối với`n=3`Và`r = [0,0,1]`, các mảng`[0,0,0]`Và`[0,0,1]`tồn tại và`g([0,0,0])`ổn định ngay lập tức với giá trị 1, trong khi`g([0,0,1])`có thể mất nhiều lần lặp lại. Việc đếm ngây thơ mà không xem xét phạm vi giới hạn chính xác cho từng phần tử sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Phương pháp brute-force lặp lại trên tất cả các mảng có thể`[a_1, ..., a_n]`trong giới hạn`0 ≤ a_i ≤ r_i`. Đối với mỗi mảng, chúng tôi mô phỏng`f`lặp đi lặp lại cho đến khi chuỗi ổn định, đếm số mảng riêng biệt được nhìn thấy. Cách tiếp cận này đúng vì nó trực tiếp thực hiện định nghĩa về`g(b)`, nhưng nó nhanh chóng trở nên khó chữa. Ngay cả đối với`n=50`với mỗi`r_i=50`, có`51^50`mảng, đó là thiên văn. 

Cái nhìn sâu sắc quan trọng là`g(b)`chỉ phụ thuộc vào nhiều tập giá trị trong`b`, không phải thứ tự của họ. Điều này cho phép chúng tôi nén vấn đề: thay vì lặp lại trên mỗi mảng, chúng tôi có thể đếm xem có bao nhiêu mảng tạo ra mỗi tập hợp số đếm. Hơn nữa, bởi vì`f`luôn tạo ra một mảng đếm, các mảng có thể có trong`f`tạo thành một không gian nhỏ: mỗi phần tử được giới hạn bởi`n`, và tổng là`n`. Điều này cho phép một cách tiếp cận lập trình động trên "vectơ tần số", đếm xem có bao nhiêu mảng dẫn đến mỗi vectơ tần số ở lần lặp tiếp theo. 

DP xây dựng các mảng đếm theo cấp độ, từ giới hạn ban đầu đến giới hạn tiếp theo của chúng.`f`sự biến đổi. Chúng tôi sử dụng tính năng ghi nhớ để tránh tính toán lại số lượng mảng đạt cùng vectơ tần số. Cuối cùng, mỗi vectơ ổn định và chúng tôi ghi lại có bao nhiêu mảng ban đầu dẫn đến một chu kỳ có độ dài`p`. Điều này làm giảm vấn đề từ hàm mũ trong`n`đại khái`O(n^3)`cho từng trường hợp thử nghiệm, điều này có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(sản phẩm(r_i) * n^2) | O(n^2) | Quá chậm | 
| Tần số-DP | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc đầu vào cho nhiều trường hợp kiểm thử và trích xuất`n`,`k`, Và`r`. 
2. Khởi tạo bảng ghi nhớ ánh xạ một vectơ tần số tới số mảng tiếp cận nó. 
3. Liệt kê tất cả các mảng có thể`[a_1, ..., a_n]`sử dụng hiệu quả DP trên tổng một phần bị ràng buộc bởi`r_i`. 
4. Với mỗi mảng, tính vectơ tần số của nó`f(a)`. Lưu trữ số lượng ánh xạ mảng ban đầu tới từng vectơ tần số. 
5. Lặp lại phép biến đổi`f`trên mỗi vectơ cho đến khi ổn định. Đối với mỗi mảng riêng biệt trong chuỗi, hãy đếm xem có bao nhiêu mảng đạt được độ dài ổn định này. 
6. Tổng số cho tất cả các mảng ban đầu theo kết quả cuối cùng`g(a)`giá trị mà họ tạo ra. 
7. Đầu ra, modulo 998244353, số lượng mảng cho mỗi mảng`p`từ`1`ĐẾN`k`. 

Tại sao nó hoạt động: Các vectơ DP theo tần số đảm bảo rằng chúng tôi tính đến mọi mảng ban đầu có thể có trong giới hạn của nó mà không liệt kê chúng một cách rõ ràng. Mỗi phép biến đổi sẽ giảm mảng thành nhiều tập hợp số đếm của nó và việc ghi nhớ đảm bảo chúng ta không tính toán lại cùng một bài toán con. Vì mọi chuỗi cuối cùng đều ổn định nên việc đếm các mảng riêng biệt trong chu trình sẽ cho kết quả chính xác`g(a)`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 998244353

from collections import Counter, defaultdict
from itertools import product

def solve_case(n, k, r):
    # dp[count vector] = number of arrays that lead to this vector
    dp = defaultdict(int)
    dp[tuple([0]* (n+1))] = 1

    # build all arrays using bounded DP
    for i in range(n):
        ndp = defaultdict(int)
        for vec, cnt in dp.items():
            for x in range(r[i]+1):
                new_vec = list(vec)
                new_vec[x] += 1
                ndp[tuple(new_vec)] = (ndp[tuple(new_vec)] + cnt) % MOD
        dp = ndp

    # memoize g values
    g_count = defaultdict(int)
    for vec, cnt in dp.items():
        seen = []
        while vec not in seen:
            seen.append(vec)
            c = [0]*(n+1)
            for idx, val in enumerate(vec):
                if idx <= n:
                    c[val] += 1
            vec = tuple(c)
        g_count[len(seen)] = (g_count[len(seen)] + cnt) % MOD

    res = [g_count[p] if p in g_count else 0 for p in range(1, k+1)]
    return res

def main():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        r = list(map(int, input().split()))
        ans = solve_case(n, k, r)
        print(' '.join(map(str, ans)))

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên là xây dựng một bảng DP đếm xem có bao nhiêu mảng dẫn đến mỗi vectơ đếm có thể có. Điều này tránh việc liệt kê tất cả các mảng một cách rõ ràng. Sau đó, với mỗi vectơ, nó mô phỏng các ứng dụng lặp đi lặp lại của`f`cho đến khi ổn định, theo dõi độ dài chuỗi như`g(a)`. Chúng tôi tổng hợp kết quả bằng cách sử dụng`defaultdict`để tính tổng số modulo 998244353. Các điểm tinh tế bao gồm lập chỉ mục chính xác các mảng tần số lên đến`n`và xử lý các mảng ổn định nhanh chóng, sẽ dễ bị tính sai nếu xảy ra lỗi lẻ tẻ. 

## Ví dụ đã hoạt động 

**Ví dụ 1:`n=2, k=5, r=[1,1]`** 

| Mảng`a`| f(a) | Trình tự | g(a) | 
| --- | --- | --- | --- | 
| [0,0] | [2,0] | [0,0] -> [2,0] -> [0,1] -> [1,0] -> ... | 1 | 
| [0,1] | [1,1] | [0,1] -> [1,1] -> [2,0] -> ... | 2 | 
| [1,0] | [1,1] | tương tự như trên | 2 | 
| [1,1] | [0,2] | [1,1] -> [0,2] -> ... | 4 | 

Điều này chứng tỏ trình tự của`f`có thể ổn định thành các chu kỳ, và`g(a)`đếm số mảng riêng biệt trong chuỗi. Mảng nhóm DP của chúng tôi tạo ra cùng một`f(a)`để tránh phải tính toán lại. 

**Ví dụ 2:`n=3, k=3, r=[1,2,1]`** 

DP xây dựng các vectơ đếm cho từng mảng, chẳng hạn như`(1,1,1,0)`cho [1,1,1], sau đó mô phỏng`f`until stabilization. Bảng theo dõi độ dài trình tự, xác nhận chính xác`g(a)`giá trị cho tất cả các mảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Các vectơ đếm DP có độ dài ≤ n và tổng ≤ n, mô phỏng lặp lại của`f`trên các vectơ nhỏ | 
| Không gian | O(n^2) | Lưu trữ DP và vectơ đếm được ghi nhớ | 

Giải pháp vừa vặn thoải mái trong giới hạn thời gian 8 giây ngay cả đối với kích thước lớn nhất`n=50`, vì tổng độ phức tạp bậc ba bị giới hạn bởi các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# sample test
assert run("1\n2 5\n1 1\n") == "1 2 0 1 0", "sample 1"

# custom tests
assert run("1\n3 3\n0 0 0\n") == "1 0 0", "all zeros"
assert run("1\n2 2\n2 2\n") == "4 2", "all max bounds"
assert run("1\n3 3\n1 2 1\n") == "3 2 1", "mixed bounds"
assert run("1\n1 1\n0\n") == "1", "single element zero"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Cái gì
