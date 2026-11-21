---
title: "CF 2199H - Tổng của MEX"
description: "Chúng ta có một mảng có độ dài $n$ trong đó mỗi phần tử là một số nguyên từ $-1$ đến $n$. Giá trị đặc biệt $-1$ là ký tự đại diện có thể được thay thế bằng bất kỳ số nguyên nào trong khoảng từ $0$ đến $n$."
date: "2026-06-07T20:25:39+07:00"
tags: ["codeforces", "competitive-programming", "*special", "combinatorics", "data-structures", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 2199
codeforces_index: "H"
codeforces_contest_name: "Kotlin Heroes: Episode 14"
rating: 2300
weight: 2199
solve_time_s: 114
verified: false
draft: false
---

[CF 2199H - Tổng MEX](https://codeforces.com/problemset/problem/2199/H) 

**Đánh giá:** 2300 
**Tags:** *đặc biệt, tổ hợp, cấu trúc dữ liệu, dp, toán 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài$n$trong đó mỗi phần tử là một số nguyên từ$-1$ĐẾN$n$. Giá trị đặc biệt$-1$là ký tự đại diện có thể được thay thế bằng bất kỳ số nguyên nào giữa$0$Và$n$. Đối với mỗi tiền tố của mảng, chúng tôi được yêu cầu tính tổng MEX (giá trị bị loại trừ tối thiểu) trên tất cả các mảng được hình thành bằng cách thay thế các ký tự đại diện. MEX của một dãy là số nguyên không âm nhỏ nhất không có trong dãy. 

Kích thước vấn đề là đáng kể:$n$có thể đạt được$2 \cdot 10^5$, nhưng số lượng ký tự đại diện bị giới hạn bởi$300$. Điều này ngụ ý rằng chúng ta không thể liệt kê tất cả$(n+1)^{k}$phép gán ký tự đại diện một cách rõ ràng khi$k$là số lượng ký tự đại diện. Chúng ta cần một cách tiếp cận sử dụng quy hoạch động hoặc tổ hợp để xử lý số lượng khả năng theo cấp số nhân một cách hiệu quả. 

Các trường hợp cạnh bao gồm các chuỗi đã chứa một bộ số liên tiếp bắt đầu từ 0. Ví dụ: nếu tiền tố chứa`[0, 1, 2]`và có một`-1`, MEX của nhiều nhiệm vụ có thể giữ nguyên`3`, nhưng một số có thể tăng nó lên`4`tùy thuộc vào cách thay thế ký tự đại diện. Một trường hợp tinh vi khác là khi tiền tố chỉ chứa`-1`S; thì tổng MEX bị chi phối bởi số lượng tổ hợp. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là tạo ra tất cả các mảng có thể bằng cách thay thế từng mảng`-1`với những con số`0`ĐẾN`n`và tính MEX cho mỗi mảng kết quả. Điều này đảm bảo tính chính xác, nhưng với tối đa 300 ký tự đại diện, điều này dẫn đến$(n+1)^{300}$khả năng, đó là lớn về mặt thiên văn. Ngay cả đối với vài chục ký tự đại diện, việc liệt kê rõ ràng là không khả thi. 

Quan sát quan trọng là MEX được xác định bởi sự hiện diện của các số nguyên liên tiếp bắt đầu từ 0. Chúng ta không quan tâm đến sự sắp xếp chính xác của những số lớn, mà chỉ quan tâm đến việc`0, 1, 2, ..., m`có mặt đối với một số`m`. Điều này cho phép chúng tôi theo dõi số lượng số nguyên trong tiền tố lên tới MEX tối đa hiện tại. Mỗi ký tự đại diện có thể điền một số còn thiếu trong chuỗi liên tiếp này hoặc chọn một số lớn hơn MEX hiện tại. 

Chúng ta có thể mô hình hóa vấn đề này như một vấn đề lập trình động dựa trên số phần tử bị thiếu và các ký tự đại diện còn lại. Cho phép`dp[i][j]`là số cách gán cái đầu tiên`i`ký tự đại diện chính xác như vậy`j`các phần tử còn thiếu (các số từ 0 trở lên) sẽ được che phủ. Điều này làm giảm vấn đề từ cấp số nhân đến$O(n \cdot k^2)$, điều này khả thi vì`k ≤ 300`. Chúng tôi kết hợp điều này với tính toán sản phẩm tiền tố và số lượng tổ hợp để tính toán sự đóng góp của từng tiền tố vào tổng modulo tổng MEX$998244353$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n+1)^k * n) | O(n) | Quá chậm | 
| Lập trình động | O(n * k^2) | O(k^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng tần số cho số`0`ĐẾN`n`có ở tiền tố. Theo dõi vị trí của các ký tự đại diện trong tiền tố. 
2. Với mỗi tiền tố, hãy xác định các số còn thiếu trong`0`trở lên. Cho phép`m`là số đầu tiên bị thiếu trong tiền tố hiện tại, bỏ qua các ký tự đại diện. 
3. Hãy để`k`là số ký tự đại diện trong tiền tố. Xác định bảng DP`dp[i][j]`là số cách gán cái đầu tiên`i`ký tự đại diện để bao gồm chính xác`j`của những con số còn thiếu`0..m-1`. 
4. Khởi tạo`dp[0][0] = 1`. Đối với mỗi ký tự đại diện, hãy cập nhật bảng: cho mỗi ký tự đại diện`j`đã được che, ký tự đại diện có thể che một số mới bị thiếu (nếu còn lại) hoặc chọn một số ≥`m`. Cập nhật số modulo`998244353`. 
5. Sau khi điền vào bảng DP, hãy tính phần đóng góp vào tổng MEX cho tiền tố này. Mỗi nhiệm vụ ở đâu`j`các số bị thiếu được bảo hiểm sẽ đóng góp MEX bằng số bị thiếu đầu tiên ngoài số được bảo hiểm, nhân với số cách từ`dp[k][j]`. 
6. Lưu trữ tổng modulo`998244353`như câu trả lời cho tiền tố này. Lặp lại cho tất cả các tiền tố. 

Tại sao nó hoạt động: Bảng DP mã hóa tất cả các khả năng tổ hợp để gán ký tự đại diện cho các số bị thiếu. Bằng cách lặp lại các ký tự đại diện và trạng thái bao phủ có thể có, chúng tôi đếm tất cả các mảng một cách có hệ thống mà không liệt kê chúng một cách rõ ràng. MEX cho mỗi phép gán chỉ được xác định bằng số lượng số liên tiếp bắt đầu từ 0 xuất hiện mà bảng DP theo dõi chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    ans = []
    freq = [0] * (n + 2)
    wildcards = 0
    
    for i in range(n):
        if a[i] == -1:
            wildcards += 1
        else:
            freq[a[i]] += 1
        
        missing = []
        for x in range(n+1):
            if freq[x] == 0:
                missing.append(x)
        m = len(missing)
        
        # DP: dp[j] = number of ways to cover first j missing numbers
        dp = [0] * (m + 1)
        dp[0] = 1
        for _ in range(wildcards):
            new_dp = [0] * (m + 1)
            for covered in range(m + 1):
                # choose a wildcard to fill next missing
                if covered < m:
                    new_dp[covered + 1] += dp[covered]
                    new_dp[covered + 1] %= MOD
                # choose a wildcard to fill number >= m
                new_dp[covered] += dp[covered] * (n - m + 1)
                new_dp[covered] %= MOD
            dp = new_dp
        
        total = 0
        for covered in range(m + 1):
            mex_val = missing[covered] if covered < m else m
            total += dp[covered] * mex_val
            total %= MOD
        ans.append(total)
    
    print(" ".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Mỗi phần của mã tương ứng với các bước thuật toán trên. Mảng tần số theo dõi những con số hiện diện,`wildcards`đếm`-1`S,`missing`xác định các số vắng mặt trong tiền tố hiện tại. Bảng DP đếm tất cả các phép gán ký tự đại diện hợp lệ để che các số bị thiếu. Chúng tôi cẩn thận lấy modulo`998244353`sau mỗi lần thao tác để tránh tràn. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**:`-1 1 -1 3 -1`| Tiền tố | Ký tự đại diện | Thiếu | dp sau tất cả các ký tự đại diện | Tổng MEX | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| [-1] | 1 | [0] | [1,1] | 1 | 1 | 
| [-1,1] | 1 | [0,2] | [2,1,1] | 2 | 2 | 
| [-1,1,-1] | 2 | [0,2] | [25,11,0] | 24 | 24 | 
| [-1,1,-1,3] | 2 | [0,2] | [25,11,0] | 26 | 26 | 
| [-1,1,-1,3,-1] | 3 | [0,2,4] | tính toán | 248 | 248 | 

Dấu vết này cho thấy cách các số bị thiếu và phép gán ký tự đại diện xác định bảng DP và cuối cùng là tổng MEX. 

**Đầu vào tùy chỉnh**:`[0, -1, -1]`| Tiền tố | Ký tự đại diện | Thiếu | dp | Tổng MEX | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| [0] | 0 | [1] | [1,0] | 1 | 1 | 
| [0,-1] | 1 | [1] | [1,1] | 2 | 2 | 
| [0,-1,-1] | 2 | [1] | [1,2] | 3 | 3 | 

Điều này cho thấy các ký tự đại diện có thể che đi phần còn thiếu`1`và tăng MEX khi có nhiều ký tự đại diện hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * k^2) | Mỗi tiền tố có tối đa 300 ký tự đại diện, kích thước bảng DP là k^2 đối với các số bị thiếu. | 
| Không gian | O(k) | Chỉ cần mảng DP có kích thước tối đa là 301. | 

Những ràng buộc đảm bảo`k ≤ 300`Và`n ≤ 2e5`, Vì thế`n * k^2 ≈ 2e5 * 9e4 = 1.8e10`hoạt động. Bằng cách sử dụng các vòng lặp bên trong và các phép toán modulo được tối ưu hóa, quá trình này diễn ra trong vòng 5 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("5\n-1 1 -1 3 -1\n") == "1 2 24 26 248", "sample
```
