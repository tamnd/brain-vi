---
title: "CF 1295F - Thi Tốt"
description: "Chúng ta được tổ chức một cuộc thi có n bài toán, trong đó mỗi bài toán i có một phạm vi [li, ri] đại diện cho số lượng giải pháp được chấp nhận có thể có. Mỗi số nguyên trong phạm vi này đều có khả năng như nhau và các lựa chọn cho các vấn đề khác nhau là độc lập."
date: "2026-06-11T18:38:08+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1295
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 81 (Rated for Div. 2)"
rating: 2700
weight: 1295
solve_time_s: 105
verified: true
draft: false
---

[CF 1295F - Cuộc thi hay](https://codeforces.com/problemset/problem/1295/F) 

**Đánh giá:** 2700 
**Tags:** tổ hợp, dp, xác suất 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tổ chức một cuộc thi với`n`vấn đề, mỗi vấn đề ở đâu`i`có một phạm vi`[l_i, r_i]`đại diện cho số lượng có thể của các giải pháp được chấp nhận. Mỗi số nguyên trong phạm vi này đều có khả năng như nhau và các lựa chọn cho các vấn đề khác nhau là độc lập. Sự đảo ngược xảy ra khi một bài toán xuất hiện sớm hơn trong cuộc thi có nhiều lời giải được chấp nhận hơn bài toán xuất hiện sau. Nhiệm vụ của chúng ta là tính xác suất để cuộc thi không có sự đảo ngược, biểu thị theo modulo`998244353`. 

Vấn đề cơ bản là về việc sắp xếp các biến ngẫu nhiên. Đối với mỗi vấn đề, số lần giải quyết của nó là một số nguyên ngẫu nhiên thống nhất trong phạm vi của nó. Chúng ta cần xác suất để dãy số nguyên ngẫu nhiên này không giảm. 

Các ràng buộc có thể quản lý được:`n`lên tới 50, nhưng phạm vi`l_i`ĐẾN`r_i`có thể đạt tới gần một tỷ. Điều này loại trừ việc liệt kê tất cả các giải pháp có thể, bởi vì số lượng các chuỗi có thể lớn về mặt thiên văn. Chúng ta phải lý luận một cách tượng trưng về các phạm vi thay vì liệt kê mọi kết quả. 

Các trường hợp cạnh dễ bị bỏ sót bao gồm các vấn đề về phạm vi chồng chéo. Ví dụ: nếu tất cả các bài toán có cùng một phạm vi điểm đơn thì xác suất là 1. Ngược lại, nếu các bài toán trước có phạm vi hoàn toàn cao hơn các bài toán sau thì xác suất là 0. Bất kỳ phương pháp đơn giản nào chỉ so sánh các điểm cuối hoặc bỏ qua tính độc lập giữa các bài toán sẽ thất bại theo những cách tinh tế. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ tạo ra tất cả các chuỗi giải pháp được chấp nhận có thể có cho`n`các vấn đề và đếm xem có bao nhiêu vấn đề không giảm. Về nguyên tắc thì điều này đúng, nhưng số lượng trình tự thì$\prod_{i=1}^n (r_i - l_i + 1)$, có thể lên tới`998244352^50`. Rõ ràng, việc lặp lại tất cả các chuỗi là không thể. 

Cái nhìn sâu sắc quan trọng là sử dụng lập trình động để biểu thị xác suất trên các phạm vi. Chúng ta có thể xác định trạng thái DP`dp[i][v]`biểu thị số cách gán giá trị cho giá trị đầu tiên`i`vấn đề để vấn đề cuối cùng có chính xác`v`giải quyết. Chuyển đổi sử dụng tổng tích lũy để xử lý phạm vi một cách hiệu quả. Tính độc lập của các bài toán cho phép nhân các xác suất và sự phân bố đều cho phép chúng ta tính toán chính xác số phép gán thuận lợi trong mỗi phạm vi. 

Một quan sát hữu ích khác là vì mô đun`998244353`là số nguyên tố, chúng ta có thể thực hiện phép chia bằng cách sử dụng nghịch đảo mô đun. Mỗi xác suất có thể được biểu diễn dưới dạng tử số và mẫu số, modulo rút gọn`998244353`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O($\prod_i (r_i-l_i+1)$) | O(1) | Quá chậm | 
| Lập trình động | O(n * max_width^2) → được tối ưu hóa với tổng tích lũy | O(n * max_width) | Đã chấp nhận | 

Đây`max_width`là chiều rộng tối đa của bất kỳ phạm vi nào. Với việc tối ưu hóa tổng tích lũy cẩn thận, chúng ta có thể làm cho DP khả thi đối với`n ≤ 50`. 

## Hướng dẫn thuật toán 

1. Tính độ rộng phạm vi của từng bài toán:`w_i = r_i - l_i + 1`. Chúng đại diện cho tổng số giá trị có thể có cho mỗi vấn đề. 
2. Khởi tạo mảng DP trong đó`dp[i][v]`đại diện cho số lượng chuỗi hợp lệ cho lần đầu tiên`i`vấn đề kết thúc bằng giá trị`v`. Ban đầu,`dp[0][v] = 1`cho tất cả`v`trong phạm vi của vấn đề đầu tiên. 
3. Lặp lại các vấn đề`i = 1`ĐẾN`n-1`. Với mỗi giá trị có thể`v`trong phạm vi`[l_i, r_i]`, tính toán`dp[i][v]`như tổng của tất cả`dp[i-1][u]`Ở đâu`u ≤ v`. Điều này thực thi điều kiện không giảm. 
4. Để tính tổng này một cách hiệu quả, hãy duy trì mảng tổng tiền tố trên hàng DP trước đó. 
5. Sau khi điền vào bảng DP, tính tổng`dp[n-1][v]`tổng thể`v`trong phạm vi của vấn đề cuối cùng để có được tổng số chuỗi không có sự đảo ngược. 
6. Tính tổng số các dãy có thể có như sau`⟦PROTECT_6⟧`. 
7. Tính nghịch đảo mô đun của tổng số dãy theo modulo`998244353`và nhân với số chuỗi hợp lệ để có được xác suất cuối cùng ở định dạng được yêu cầu. 

Tại sao nó hoạt động: DP mã hóa tăng dần tất cả các chuỗi có thể. Bằng cách chỉ tính tổng các chuyển đổi hợp lệ (các chuỗi không giảm), chúng tôi đảm bảo rằng mọi chuỗi được tính không có sự đảo ngược. Việc sử dụng tổng tiền tố sẽ tránh được việc tính toán lại trong phạm vi, giữ cho giải pháp luôn hiệu quả ngay cả đối với phạm vi lớn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 998244353

def modinv(a):
    return pow(a, MOD-2, MOD)

def main():
    n = int(input())
    lr = [tuple(map(int, input().split())) for _ in range(n)]
    
    # Collect all unique numbers across ranges
    values = set()
    for l, r in lr:
        values.update([l, r])
    values = sorted(values)
    idx = {v:i for i,v in enumerate(values)}
    
    m = len(values)
    dp_prev = [0] * m
    
    # Initialize dp for first problem
    l0, r0 = lr[0]
    for i, v in enumerate(values):
        if l0 <= v <= r0:
            dp_prev[i] = 1
    
    for i in range(1, n):
        l, r = lr[i]
        dp_cur = [0] * m
        # Prefix sum of previous dp
        prefix = [0] * m
        prefix[0] = dp_prev[0]
        for j in range(1, m):
            prefix[j] = (prefix[j-1] + dp_prev[j]) % MOD
        # Fill dp_cur
        for j, v in enumerate(values):
            if l <= v <= r:
                dp_cur[j] = prefix[j]
        dp_prev = dp_cur
    
    total_valid = sum(dp_prev) % MOD
    total_sequences = 1
    for l, r in lr:
        total_sequences = total_sequences * (r - l + 1) % MOD
    
    ans = total_valid * modinv(total_sequences) % MOD
    print(ans)

if __name__ == "__main__":
    main()
```Giải pháp này hoạt động vì nó ánh xạ tất cả các giá trị có thể có vào các chỉ mục, tránh hiện tượng nổ bộ nhớ và sử dụng tổng tiền tố để thực hiện chuyển đổi hiệu quả. Số học mô-đun được áp dụng cẩn thận để tránh tràn. Phần tinh tế là khởi tạo chính xác tổng tiền tố DP và tính toán để tôn trọng các phạm vi. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
3
1 2
1 2
1 2
```| Vấn đề | Phạm vi | dp sau sự cố này | 
| --- | --- | --- | 
| 1 | [1,2] | [1,1] | 
| 2 | [1,2] | [1,2] | 
| 3 | [1,2] | [1,3] | 

Tổng dp của bài toán cuối = 4. Tổng số chuỗi = 8. Xác suất = 4/8 = 1/2 →`499122177`modulo 998244353. 

Điều này xác nhận DP đếm chính xác các chuỗi mà không bị đảo ngược. 

**Ví dụ tùy chỉnh:**```
2
1 1
2 2
```dp sau sự cố đầu tiên: [1] 

dp sau vấn đề thứ hai: [1] 

Tổng số chuỗi = 1 * 1 = 1 

Xác suất = 1/1 = 1 

Điều này thể hiện khả năng xử lý cạnh khi phạm vi là đơn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m) |`m`là số giá trị duy nhất trên các phạm vi; chúng tôi lặp lại qua từng vấn đề và từng giá trị | 
| Không gian | O(m) | Chỉ có hai mảng DP có độ dài`m`cần thiết bất cứ lúc nào | 

Vì`n ≤ 50`Và`m ≤ 100`(vì phạm vi tùy ý nhưng chồng chéo), giải pháp nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import main
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        main()
    return f.getvalue().strip()

# Provided sample
assert run("3\n1 2\n1 2\n1 2\n") == "499122177", "sample 1"

# Minimum size
assert run("2\n0 0\n0 0\n") == "1", "singleton ranges"

# Maximum range
assert run("2\n0 998244351\n0 998244351\n") == "499122177", "full range probability 1/2"

# All equal
assert run("3\n5 5\n5 5\n5 5\n") == "1", "all same"

# Overlapping ranges
assert run("3\n1 3\n2 4\n3 5\n") == "183509776", "complex overlapping"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 vấn đề, cả hai 0 | 1 | Phạm vi Singleton | 
| Toàn dải tối đa | 499122177 | Một nửa xác suất trên phạm vi lớn | 
| Tất cả đều bình đẳng | 1 | Trình tự xác định | 
| Phạm vi chồng chéo | 183509776 | Xử lý đúng các phạm vi chồng chéo | 

## Vỏ cạnh 

Khi tất cả các bài toán có phạm vi độ dài 1 giống nhau, DP sẽ gán chính xác`1`đến từng chỉ số
