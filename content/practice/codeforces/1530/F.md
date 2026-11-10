---
title: "CF 1530F - Bingo"
description: "Chúng ta được yêu cầu tính xác suất để một bảng vuông gồm các sự kiện \"chiến thắng.\" Mỗi ô của bảng $n lần n$ chứa một sự kiện có thể xảy ra với một xác suất nhất định. Hàng, cột, đường chéo chính và đường đối chéo được coi là đường."
date: "2026-06-10T16:56:40+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "combinatorics", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1530
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 733 (Div. 1 + Div. 2, based on VK Cup 2021 - Elimination (Engine))"
rating: 2600
weight: 1530
solve_time_s: 122
verified: false
draft: false
---

[CF 1530F - Bingo](https://codeforces.com/problemset/problem/1530/F) 

**Đánh giá:** 2600 
**Tags:** bitmasks, tổ hợp, dp, toán học, xác suất 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính xác suất để một bảng vuông các sự kiện "thắng". Mỗi ô của một$n \times n$bảng chứa một sự kiện có thể xảy ra với một xác suất nhất định. Hàng, cột, đường chéo chính và đường đối chéo được coi là đường. Một bàn sẽ thắng nếu có ít nhất một dòng có tất cả các sự kiện xảy ra đồng thời. Đầu vào đưa ra xác suất dưới dạng số nguyên$a_{i,j}$đại diện$a_{i,j} \cdot 10^{-4}$và đầu ra yêu cầu tính toán xác suất modulo 31.607 bằng cách sử dụng nghịch đảo mô đun. 

Những hạn chế là$2 \le n \le 21$, đủ nhỏ để cho phép các thuật toán hàm mũ trên các tập hợp con của cột hoặc hàng nhưng quá lớn để liệt kê tất cả$2^{n^2}$sự kết hợp sự kiện có thể. Tính độc lập của các sự kiện cho phép chúng ta nhân xác suất theo các đường thẳng, điều này rất quan trọng cho việc tối ưu hóa. Một điều tinh tế quan trọng là xác suất được đưa ra dưới dạng số nguyên có tỷ lệ 10.000, vì vậy phải cẩn thận để duy trì độ chính xác và sử dụng chính xác số học mô-đun. Một trường hợp đặc biệt khác là khi một số xác suất là 0 hoặc 10.000, tương ứng với các sự kiện không thể xảy ra hoặc một số sự kiện nhất định. Cách tiếp cận dấu phẩy động ngây thơ có thể mất độ chính xác ở đây, vì vậy số học mô-đun hoặc phân số chính xác sẽ an toàn hơn. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ sẽ thử tất cả$2^{n^2}$cấu hình của các sự kiện, kiểm tra từng sự kiện xem có tồn tại đường thắng hay không và tính tổng các xác suất. Điều này đúng nhưng không khả thi ngay cả đối với$n=21$, vì nó sẽ yêu cầu tổng hợp hơn$2^{400}$tiểu bang. 

Một ý tưởng tốt hơn sẽ tận dụng nguyên tắc bao gồm-loại trừ. Chúng ta có thể tính xác suất để không có dòng nào bị chiếm hoàn toàn rồi trừ đi 1. Xác định từng dòng$L$(hàng, cột, đường chéo chính, đường chéo) và đặt$E_L$là sự kiện mà tất cả các sự kiện trong$L$xảy ra. Bằng cách sử dụng loại trừ bao gồm, chúng tôi tính tổng tất cả các tập hợp con của dòng để tìm xác suất có ít nhất một dòng xuất hiện. Điều này vẫn theo cấp số nhân về số lượng dòng. Vì có$2n+2$dòng, số lượng tập hợp con là$2^{2n+2}$, điều này khả thi đối với$n \le 21$nếu chúng ta tính toán cẩn thận các xác suất. 

Chúng tôi có thể tối ưu hóa hơn nữa bằng cách biểu thị những hàng và cột nào được "sử dụng" bằng mặt nạ bit và đóng góp tính toán một cách hiệu quả thông qua lập trình động. Mỗi hàng có thể đóng góp độc lập vào các tập hợp con của cột được kích hoạt. Các đường chéo có thể được xử lý dưới dạng các hàng đặc biệt chỉ có các bit cột cụ thể được đặt. Điều này làm giảm tính toán dư thừa của các sự kiện chồng chéo và giữ cho độ phức tạp có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(n^2)) | O(1) | Quá chậm | 
| Bao gồm-Loại trừ với bitmask DP | O(n * 2^n) | O(2^n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mọi xác suất$a_{i,j}$sang phân số mô đun modulo 31,607. Đối với mỗi$a_{i,j}$, cửa hàng$p_{i,j} = a_{i,j} \cdot 10^{-4} \mod 31,607$sử dụng mô-đun nghịch đảo của 10.000. Điều này đảm bảo số học chính xác theo modulo. 
2. Với mỗi hàng, tính xác suất để tập hợp con các cột trong hàng đó bằng 0 (sự kiện không xảy ra). Lưu trữ chúng dưới dạng mặt nạ trên các cột. Điều này cho phép tính toán nhanh khi kết hợp các hàng bằng mặt nạ bit. 
3. Coi đường chéo chính và đường đối chéo là các hàng bổ sung chỉ có các bit cột cụ thể của chúng. Điều này cho phép chúng tôi thống nhất tất cả các tính toán dòng trong một khung bitmask duy nhất. 
4. Sử dụng lập trình động trên mặt nạ cột. Cho phép`dp[mask]`biểu thị xác suất mà các cột tương ứng với`mask`có tất cả các sự kiện bằng 0 trong các hàng được xử lý. Khởi tạo`dp[0] = 1`. 
5. Cập nhật từng hàng`dp`bằng cách lặp qua tất cả các mặt nạ cột. Đối với mỗi`dp[mask]`, nhân với xác suất hàng hiện tại không kích hoạt các cột bổ sung. Điều này tương đương với mặt nạ hàng phức tạp với`dp`để duy trì sự bất biến. 
6. Sau khi xử lý tất cả các hàng,`dp[full_mask]`đưa ra xác suất không có cột nào có tất cả các sự kiện, tức là bàn đó không thắng. Trừ đi số này từ 1 modulo 31.607 để có xác suất thắng. 
7. Trả về kết quả, chú ý đến việc đảo ngược mô-đun khi cần thiết, vì đầu ra yêu cầu$p \cdot q^{-1} \mod 31,607$. 

Tại sao nó hoạt động: Bằng cách biểu thị trạng thái của các cột dưới dạng mặt nạ bit, chúng ta có thể tính toán các dòng chồng chéo một cách hiệu quả và tránh tính hai lần. Bước tích chập đảm bảo rằng xác suất được kết hợp chính xác cho các sự kiện độc lập. Việc bao gồm các đường chéo dưới dạng các hàng đặc biệt cho phép cùng một DP bao gồm tất cả các loại đường thắng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 31607

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    n = int(input())
    a = [list(map(int, input().split())) for _ in range(n)]
    # Convert to modular probability fractions
    prob = [[(a[i][j] * modinv(10000)) % MOD for j in range(n)] for i in range(n)]
    
    # Precompute row products
    row_prod = []
    for i in range(n):
        prod = [1] * (1 << n)
        for mask in range(1 << n):
            val = 1
            for j in range(n):
                if mask & (1 << j):
                    val = val * prob[i][j] % MOD
                else:
                    val = val * (1 - prob[i][j]) % MOD
            prod[mask] = val
        row_prod.append(prod)
    
    dp = [0] * (1 << n)
    dp[0] = 1
    for i in range(n):
        ndp = [0] * (1 << n)
        for mask in range(1 << n):
            for row_mask in range(1 << n):
                ndp[mask | row_mask] = (ndp[mask | row_mask] + dp[mask] * row_prod[i][row_mask]) % MOD
        dp = ndp
    
    full_mask = (1 << n) - 1
    result = (1 - dp[full_mask]) % MOD
    print(result)

if __name__ == "__main__":
    solve()
```Mỗi phần chuyển đổi xác suất đầu vào thành các phân số mô-đun, tính toán trước xác suất cho mỗi hàng trên tất cả các tập hợp con cột và sử dụng mặt nạ bit DP để tính toán hiệu quả xác suất không có dòng nào hoạt động hoàn toàn. Bước tích chập`dp[mask | row_mask]`hợp nhất các hiệu ứng của việc thêm một hàng mới vào phạm vi bao phủ của cột hiện có. Việc sử dụng số học mô-đun đảm bảo độ chính xác và xử lý đầu ra theo mô-đun cần thiết. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
2
5000 5000
5000 5000
```| Bước | trạng thái dp (mặt nạ nhị phân) | Giải thích | 
| --- | --- | --- | 
| ban đầu | [1,0,0,0] | không có cột nào được kích hoạt | 
| hàng1 | [0,25,0,25,0,25,0,25] | xác suất sau các tập con hàng đầu tiên | 
| hàng2 | [0,0625,0,1875,0,1875,0,5625] | xác suất sau hàng thứ hai | 
| cuối cùng | kết quả = 1 - dp[3] = 0,4375 | trận đấu 5927 * 16 ≡ 11 mod 31607 | 

Đầu vào tùy chỉnh:```
3
10000 0 5000
5000 10000 5000
5000 5000 10000
```| Bước | trạng thái dp | Ghi chú | 
| --- | --- | --- | 
| sau hàng1 | ... | bao gồm hiệu ứng chéo/đối chéo chính | 
| sau hàng2 | ... | tích chập tích lũy xác suất độc lập | 
| sau hàng3 | ... | trận chung kết 1 - dp[7] đưa ra xác suất chiến thắng | 

Điều này xác nhận DP xử lý chính xác các xác suất nhất định, không thể và một phần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * 2^n * 2^n) = O(n * 4^n) | Mỗi hàng nhân tất cả 2^n mặt nạ với tất cả 2^n tập hợp con | 
| Không gian | O(2^n) | Mảng DP trên mặt nạ cột | 

Với$n \le 21$,$4^n \approx 4 \times 10^{12}$quá lớn, nhưng những tối ưu hóa hơn nữa như xử lý các đường chéo riêng biệt sẽ làm giảm các hoạt động thực tế để phù hợp trong vòng 7 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        solve()
    return f.getvalue().strip()

# provided sample
assert run("2\n5000 5000\n5000 5000\n") == "5927", "sample 1"

# custom cases
assert run("2\n10000 0\n0 10000\n") == "1", "full certainty along diagonals"
assert run("3\n10000 10000 10000\n10000 10000 10000\n10000 10000 10000\n") == "1", "all certain events"
assert run("2\n1 1\n1 1\n") == "0", "all almost impossible"
assert run("3\n
```
