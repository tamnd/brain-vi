---
title: "CF 2020E - Công suất kỳ vọng"
description: "Chúng ta được cung cấp một mảng các số nguyên, mỗi số có xác suất độc lập được đưa vào một tập hợp ngẫu nhiên. Nhiệm vụ là tính giá trị mong đợi của bình phương XOR của tất cả các phần tử được chọn."
date: "2026-06-08T12:49:35+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 2020
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 976 (Div. 2) and Divide By Zero 9.0"
rating: 2000
weight: 2020
solve_time_s: 152
verified: false
draft: false
---

[CF 2020E - Sức mạnh kỳ vọng](https://codeforces.com/problemset/problem/2020/E) 

**Xếp hạng:** 2000 
**Tags:** bitmasks, dp, toán học, xác suất 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên, mỗi số có xác suất độc lập được đưa vào một tập hợp ngẫu nhiên. Nhiệm vụ là tính giá trị mong đợi của bình phương XOR của tất cả các phần tử được chọn. Đầu vào cung cấp xác suất dưới dạng số nguyên trong số 10.000 và chính các phần tử mảng. Chúng tôi phải xử lý nhiều trường hợp thử nghiệm một cách hiệu quả, với tổng của tất cả các độ dài mảng không vượt quá 200.000 và tất cả các số tối đa là 1023. Đầu ra là modulo XOR bình phương dự kiến$10^9+7$. 

Một cách tiếp cận ngây thơ để liệt kê tất cả các tập con có thể là không khả thi vì có$2^n$các tập hợp con, có kích thước lớn về mặt thiên văn đối với$n$lên tới 200.000. Các trường hợp biên bao gồm mảng một phần tử, các số lặp lại với xác suất khác nhau và tình huống trong đó xác suất là 0 hoặc 100%, có thể đơn giản hóa một cách tầm thường giá trị mong đợi nhưng vẫn phải tính toán chính xác theo modulo$10^9+7$. Ví dụ, nếu mảng là`[1,1]`với xác suất`[10000,0]`, chỉ có phần tử đầu tiên đóng góp và$(f(S))^2$đơn giản là$1^2$. Việc triển khai bất cẩn có thể quên tỷ lệ nghịch đảo mô-đun hoặc số nguyên cần thiết cho xác suất. 

## Phương pháp tiếp cận 

Giải pháp brute-force tính toán XOR cho từng tập hợp con, nhân với xác suất, bình phương và tổng của nó trên tất cả$2^n$tập hợp con. Điều này đúng nhưng rõ ràng là không thể khi$n > 20$. 

Cái nhìn sâu sắc quan trọng cho một giải pháp nhanh hơn là tính tuyến tính của kỳ vọng và khả năng phân tách hoạt động XOR theo bit. Vì XOR là phép cộng bitwise modulo 2 nên mỗi bit hoạt động độc lập. Cho phép$X_i$biểu thị biến ngẫu nhiên chỉ báo cho bit$i$trong XOR của nhiều tập hợp đã chọn. Sau đó$\Pr(X_i=1)$là xác suất để một số lẻ phần tử mảng có bit$i$bao gồm. 

Máy tính$\Pr(X_i=1)$có thể được thực hiện trực tiếp bằng cách duy trì hàm tạo xác suất trên các tập hợp con số. Cho phép$q_i = 1 - p_i / 10^4$. Đối với một chút nhất định$b$, định nghĩa$s_b = \sum$về xác suất của các số có bit đó. Sử dụng lập trình động hoặc tích chập lặp trên 2 trạng thái (có tập bit hoặc không), chúng ta có thể tính xác suất bit đó$b$là 1 trong XOR cuối cùng. 

Một khi chúng tôi có$\Pr(X_i=1)$cho tất cả 10 bit (vì$a_i \le 1023$), giá trị kỳ vọng của$(f(S))^2$có thể được thể hiện như$$\mathbb{E}[(f(S))^2] = \sum_{i} 2^{2i} \Pr(X_i=1) + \sum_{i\neq j} 2^{i+j} \Pr(X_i=1 \text{ XOR } X_j=1)$$Ở đây, các thuật ngữ chéo được đơn giản hóa bằng cách sử dụng tính độc lập của các bit, đưa ra$$\Pr(X_i \oplus X_j = 1) = \Pr(X_i=1)\cdot(1-\Pr(X_j=1)) + (1-\Pr(X_i=1))\cdot\Pr(X_j=1)$$Sự phân rã này làm giảm vấn đề hàm mũ thành một vấn đề tuyến tính về số bit và kích thước mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n2^n)$|$O(n)$| Quá chậm | 
| DP theo bit |$O(nB^2)$,$B=10$|$O(B)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$, mảng$a$và mảng$p$. Chuyển đổi xác suất thành phân số mô-đun$p_i/10000 \bmod 10^9+7$. 
2. Khởi tạo một mảng`prob_bit[10]`để lưu trữ xác suất mỗi bit được đặt trong XOR của một tập hợp con ngẫu nhiên. 
3. Đối với mỗi bit$b$từ 0 đến 9, tính`prob_bit[b]`bằng cách lặp qua mảng và cập nhật xác suất để một số lẻ các số được chọn có tập hợp bit đó. Điều này có thể được thực hiện lặp đi lặp lại: đối với mỗi số có tập bit, hãy nhân xác suất hiện tại của tính chẵn lẻ chẵn và lẻ với xác suất bao gồm của nó. 
4. Tính bình phương dự kiến ​​bằng công thức:$$\mathbb{E}[(f(S))^2] = \sum_{b=0}^{9} 2^{2b} \text{prob\_bit}[b] + \sum_{0 \le b_1 < b_2 < 10} 2^{b_1+b_2} ( \text{prob\_bit}[b_1] + \text{prob\_bit}[b_2] - 2 \text{prob\_bit}[b_1]\text{prob\_bit}[b_2] )$$Sử dụng số học mô-đun với nghịch đảo của 10000 khi tính xác suất. 

1. Xuất kết quả theo modulo$10^9+7$. 

Tại sao nó hoạt động: Hoạt động XOR phân rã theo các bit và tính độc lập của việc lựa chọn cho phép xác suất mỗi bit kết hợp theo cấp số nhân. Các thuật ngữ chéo bit được xử lý bằng tính tuyến tính của kỳ vọng và công thức XOR. Thuật toán không bao giờ liệt kê các tập hợp con nhưng nó tính toán chính xác từng đóng góp có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7
INV10000 = pow(10000, MOD-2, MOD)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        p = list(map(int, input().split()))
        
        prob_bit = [0]*10
        for b in range(10):
            even, odd = 1, 0
            for i in range(n):
                if (a[i] >> b) & 1:
                    pi = p[i]*INV10000 % MOD
                    new_even = (even*(1-pi) + odd*pi) % MOD
                    new_odd  = (even*pi + odd*(1-pi)) % MOD
                    even, odd = new_even, new_odd
            prob_bit[b] = odd
        
        result = 0
        for b in range(10):
            result = (result + prob_bit[b]*(1 << (2*b))) % MOD
        for b1 in range(10):
            for b2 in range(b1+1, 10):
                cross = (prob_bit[b1] + prob_bit[b2] - 2*prob_bit[b1]*prob_bit[b2]%MOD) % MOD
                result = (result + cross*(1 << (b1+b2))) % MOD
        print(result)

if __name__ == "__main__":
    solve()
```Giải pháp chuyển đổi xác suất thành phân số mô-đun và tính toán lặp lại xác suất mà mỗi bit được đặt trong XOR cuối cùng. Hình vuông dự kiến ​​sau đó được tập hợp bằng cách sử dụng cả đóng góp bit đơn và đóng góp bit chéo theo cặp. Việc sử dụng phép nhân mô-đun sẽ ngăn ngừa tràn và thứ tự các phép toán tôn trọng quyền ưu tiên của toán tử Python. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
2
2
1 2
5000 5000
2
1 1
1000 2000
```Dấu vết cho trường hợp thử nghiệm đầu tiên: 

| Bước | thậm chí | lẻ | Số lượng đóng góp? | pi | 
| --- | --- | --- | --- | --- | 
| b=0 | 1 | 0 | 1 có bit0=1 | 0,5 | 
| sau 1 | 0,5 | 0,5 | | | 
| sau 2 | 0,5 | 0,5 | 2 có bit0=0 | | 
| b=1 | 1 | 0 | 1 có bit1=0 | | 
| sau 2 | 0,5 | 0,5 | 2 có bit1=1 | 0,5 | 

Cuối cùng`prob_bit`đưa ra xác suất dự kiến ​​​​cho các bit XOR. Kết hợp sản lượng giá trị bình phương dự kiến$7/2 \equiv 500000007$. 

Trường hợp thử nghiệm thứ hai tiến hành tương tự, với xác suất 0,1 và 0,2, mang lại 0,26 như mong đợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t_n_B^2) | B=10 bit, vòng lặp bên trong xử lý các cập nhật xác suất và tổng bit chéo | 
| Không gian | O(B) | Lưu trữ xác suất cho mỗi bit | 

Tổng của$n$trên tất cả các trường hợp thử nghiệm là 200.000, B không đổi, do đó tổng số phép tính ≈ 2×10^6, có thể chấp nhận được dưới giới hạn 4 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("4\n2\n1 2\n5000 5000\n2\n1 1\n1000 2000\n6\n343 624 675 451 902 820\n6536 5326 7648 2165 9430 5428\n1\n1\n10000\n") == "500000007\n820000006\n280120
```
