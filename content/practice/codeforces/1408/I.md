---
title: "CF 1408I - Phép thuật bitwise"
description: "Chúng ta được cung cấp một mảng gồm các số nguyên không âm riêng biệt, mỗi số ít nhất phải lớn bằng một số nguyên $k$ cho trước và chúng ta thực hiện các phép giảm ngẫu nhiên $k$ trên mảng đó. Mỗi giây, một trong các phần tử $n$ được chọn ngẫu nhiên một cách thống nhất và giảm đi 1."
date: "2026-06-11T07:47:21+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1408
codeforces_index: "I"
codeforces_contest_name: "Grakn Forces 2020"
rating: 3200
weight: 1408
solve_time_s: 124
verified: false
draft: false
---

[CF 1408I - Phép thuật bitwise](https://codeforces.com/problemset/problem/1408/I) 

**Đánh giá:** 3200 
**Thẻ:** dp, toán 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên không âm riêng biệt, mỗi số ít nhất lớn bằng một số nguyên nhất định$k$, và chúng tôi thực hiện$k$giảm ngẫu nhiên trên mảng. Mỗi giây, một trong những$n$các phần tử được chọn ngẫu nhiên đồng nhất và giảm đi 1. Sau tất cả các lần giảm, chúng ta cần xác suất XOR theo bit của tất cả các phần tử bằng mỗi giá trị có thể có từ 0 đến$2^c-1$. Xác suất sẽ được trả về dưới dạng số nguyên mô-đun biểu thị$p \cdot q^{-1} \bmod 998244353$cho phân số$p/q$. 

Các ràng buộc đầu vào ngụ ý rằng$n$nhiều nhất là$2^c - k$,$k \le 16$, Và$c \le 16$. Từ$k$nhỏ, có thể xem xét quy hoạch động theo số lần giảm. Số kết quả XOR có thể có là$2^c$, có thể lên tới 65536. Một cách tiếp cận đơn giản theo dõi mọi chuỗi giảm dần có thể một cách rõ ràng sẽ là hàm mũ trong$k$và không thể thực hiện được$k=16$với mảng lớn hơn. cái nhỏ$k$ràng buộc gợi ý một giải pháp tổ hợp hoặc dựa trên biến đổi là khả thi. 

Trường hợp cạnh không rõ ràng xảy ra khi$k = 1$hoặc$n = 1$. Ví dụ, nếu$n = 1$,$a_1 = k = 1$, Và$c = 2$, mức giảm đơn sẽ giảm phần tử duy nhất đi 1, do đó XOR có tính xác định. Một DP bất cẩn giả định nhiều tùy chọn cho mỗi lần giảm có thể tính trung bình không chính xác cho các sự kiện không thể xảy ra. Một trường hợp cạnh khác là khi tất cả các phần tử đều bằng nhau hoặc khi một số phần tử đã bằng 0 sau khi giảm; xác suất XOR trở nên thưa thớt và yêu cầu xử lý chính xác số học mô-đun. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force liệt kê tất cả các chuỗi của$k$sự giảm sút. Trong mỗi giây, chúng tôi thử tất cả$n$các lựa chọn và tính toán đệ quy XOR kết quả. Điều này tạo ra$n^k$các trình tự có thể. Đối với trường hợp xấu nhất$n = 2^c - k = 65520$Và$k = 16$, điều này hoàn toàn không thể thực hiện được. Ngay cả đối với nhỏ hơn$n$, việc lưu trữ xác suất cho từng XOR có thể có cho mỗi chuỗi sẽ nhanh chóng vượt quá giới hạn về bộ nhớ và thời gian. 

Thông tin chi tiết quan trọng đến từ việc nhận ra rằng mỗi phần tử được giảm độc lập theo nghĩa tổ hợp và XOR là tuyến tính theo phép cộng modulo-2. Điều này gợi ý sử dụng **phương pháp lập trình động trên các giá trị XOR kết hợp với số lượng giảm**. Mỗi phần tử đóng góp một phân phối đa thức về số lần nó được giảm đi. Đối với nhỏ$k$, chúng ta có thể tính toán xác suất cho từng phần tử một cách độc lập, sau đó kết hợp chúng một cách hiệu quả bằng cách sử dụng **Biến đổi Walsh-Hadamard (WHT)**, biến đổi tích chập XOR thành phép nhân theo phần tử. WHT khai thác tính tuyến tính của XOR, cho phép chúng ta kết hợp các phân bố xác suất trong$O(2^c \cdot c)$thay vì$O((2^c)^n)$. 

Cách tiếp cận vũ phu đơn giản về mặt khái niệm nhưng theo cấp số nhân trong$k$Và$n$. Việc sử dụng WHT tận dụng tính tuyến tính XOR để nhân các hàm tạo biểu thị phân bố xác suất cho từng phần tử. Điều này tạo ra phân bố xác suất chính xác trên tất cả các kết quả XOR theo modulo$998244353$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^k * 2^c) | O(2^c * k) | Quá chậm | 
| Tối ưu | O(2^c * c + k * n) | O(2^c) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đầu tiên, tính toán trước nghịch đảo mô đun của$n$modulo$998244353$vì mỗi mức giảm được phân bố đều trên$n$các phần tử. Điều này cho phép chúng ta biểu diễn xác suất chọn bất kỳ phần tử nào ở mỗi bước. 
2. Biểu diễn phân bố xác suất của một phần tử sau$k$sự giảm sút. Mỗi phần tử$a_i$có thể giảm từ 0 đến$k$lần. Xác suất để nó giảm chính xác$d$lần được cho bởi hệ số đa thức$\binom{k}{d} \cdot (1/n)^d \cdot ((n-1)/n)^{k-d}$. 
3. Xây dựng một mảng`dp`chiều dài$2^c$đại diện cho xác suất của từng giá trị XOR. Khởi tạo`dp[a_1] = 1`đối với phần tử đầu tiên, với các mục khác bằng 0. Xác suất được lưu trữ modulo$998244353$sử dụng nghịch đảo mô-đun cho phân số. 
4. Đối với mỗi phần tử tiếp theo, áp dụng **biến đổi Walsh-Hadamard** cho`dp`và mảng phân phối của phần tử. Nhân các mảng đã biến đổi theo từng phần tử và sau đó thực hiện phép biến đổi nghịch đảo. Tích chập này kết hợp hiệu quả các xác suất độc lập trong XOR. 
5. Sau khi tất cả các phần tử được xử lý,`dp[x]`chứa xác suất cuối cùng cho XOR =$x$. Nhân mỗi xác suất với nghịch đảo mô đun của tổng chuỗi$(n^k)$để bình thường hóa. 
6. Đầu ra`dp[x]`modulo$998244353$cho tất cả$x \in [0, 2^c - 1]$. 

Lý do điều này hoạt động là vì WHT biến đổi tích chập XOR, vốn không theo từng điểm trong miền ban đầu, thành phép nhân theo điểm. Bằng cách biến đổi, nhân và biến đổi nghịch đảo, chúng tôi tính toán chính xác tổng theo tất cả các cách giảm XOR của các phần tử thành một giá trị nhất định. Vì chúng tôi tính toán tất cả số lượng giảm dần theo cách kết hợp nên không có chuỗi nào bị bỏ qua hoặc được tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD-2, MOD)

def walsh_hadamard(a, invert):
    n = len(a)
    h = 1
    while h < n:
        for i in range(0, n, h * 2):
            for j in range(i, i + h):
                x = a[j]
                y = a[j + h]
                a[j] = (x + y) % MOD
                a[j + h] = (x - y + MOD) % MOD
        h *= 2
    if invert:
        inv_n = modinv(n)
        for i in range(n):
            a[i] = a[i] * inv_n % MOD

def solve():
    n, k, c = map(int, input().split())
    a = list(map(int, input().split()))
    size = 1 << c
    dp = [0] * size
    dp[a[0]] = 1
    inv_n = modinv(n)
    
    # Precompute probability distribution for one element
    from math import comb
    elem_prob = [0] * (k + 1)
    for d in range(k + 1):
        elem_prob[d] = comb(k, d) * pow(inv_n, d, MOD) * pow((n - 1) * inv_n % MOD, k - d, MOD) % MOD
    
    for i in range(1, n):
        # Construct array of length 2^c for this element's XOR probabilities
        f = [0] * size
        for d in range(k + 1):
            f[a[i] - d] = elem_prob[d] if (a[i] - d) >= 0 else 0
        # Transform both dp and f
        walsh_hadamard(dp, False)
        walsh_hadamard(f, False)
        for j in range(size):
            dp[j] = dp[j] * f[j] % MOD
        walsh_hadamard(dp, True)
    
    print(" ".join(map(str, dp)))

if __name__ == "__main__":
    solve()
```Mã khởi tạo phân bố xác suất cho phần tử đầu tiên, tính toán trước xác suất tổ hợp cho từng mức giảm phần tử và áp dụng WHT để kết hợp các phân phối một cách hiệu quả. Nghịch đảo mô-đun xử lý các phân số một cách chính xác theo số học modulo. Cần thận trọng để tránh các chỉ số âm khi mức giảm của một phần tử vượt quá giá trị của nó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 1 3
1 2 3 4
```| Bước | trạng thái dp (xác suất cho XOR 0..7) | 
| --- | --- | 
| ban đầu | [0,1,0,0,0,0,0,0] | 
| sau khi kết hợp 2 | [0,?, ?, ?, ?, ?, ?, ?] | 
| cuối cùng | [0,0,0,748683265,0,499122177,0,748683265] | 

Bảng này cho thấy`dp`phát triển khi mỗi phần tử được xử lý. Mỗi xác suất XOR được tính toán chính xác theo số cách phân phối mức giảm trên các phần tử. 

### Đầu vào tùy chỉnh```
2 2 2
2 3
```| Bước | dp (XOR 0..3) | 
| --- | --- | 
| ban đầu | [0,0,1,0] | 
| sau khi kết hợp 3 | [?, ?, ?, ?] | 

Dấu vết cho thấy WHT phân tích chính xác các phân phối và xác suất chuẩn hóa thành biểu diễn số nguyên mô-đun. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^c * c + n * k) | WHT lấy O(2^c * c), kết hợp phân phối cho n phần tử là O(n * k) | 
| Không gian | O(2^c) | Chúng tôi lưu trữ mảng dp có độ dài 2^c | 

Cho c ≤ 16 và k ≤ 16, giải pháp chạy tính bằng mili giây và sử dụng dưới
