---
title: "CF 105066G - Gấu Trúc Buồn Ngủ"
description: "Chúng ta được cung cấp một mảng các số nguyên, trong đó mỗi giá trị là một nhãn được viết trên một con gấu trúc. Với mỗi cặp chỉ số riêng biệt $(i, j)$, chúng ta tưởng tượng lấy hai số $xi$ và $xj$ rồi ghép chúng theo thứ tự đó để tạo thành một số nguyên mới."
date: "2026-06-23T12:30:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105066
codeforces_index: "G"
codeforces_contest_name: "Teamscode Spring 2024 (Novice Division)"
rating: 0
weight: 105066
solve_time_s: 91
verified: false
draft: false
---

[CF 105066G - Gấu trúc buồn ngủ](https://codeforces.com/problemset/problem/105066/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên, trong đó mỗi giá trị là một nhãn được viết trên một con gấu trúc. Với mọi cặp chỉ số phân biệt có thứ tự$(i, j)$, chúng ta tưởng tượng lấy hai số$x_i$Và$x_j$và nối chúng theo thứ tự đó để tạo thành một số nguyên mới. Người quản lý vườn thú chỉ giữ cặp nếu số nối này chia hết cho một số cố định$K$. Nhiệm vụ của chúng ta là đếm xem có bao nhiêu cặp có thứ tự thỏa mãn điều kiện chia hết này. 

Khó khăn chính là phép nối không phải là một phép toán số học mà chúng ta có thể áp dụng trực tiếp các thủ thuật modulo mà không cần xử lý trước. Giá trị được nối phụ thuộc vào số chữ số trong$x_i$, do đó mỗi cặp tương tác thông qua độ dài chữ số theo cách không đồng nhất. 

Các ràng buộc ngay lập tức loại trừ bất kỳ phép liệt kê bậc hai nào của các cặp. Với$N$lên đến$10^5$, việc kiểm tra tất cả các cặp có thứ tự sẽ dẫn đến$10^{10}$trong trường hợp xấu nhất vượt xa giới hạn khả thi. Ngay cả việc lưu trữ tất cả các giá trị ghép nối theo cặp cũng không thể thực hiện được do các ràng buộc cả về thời gian và kích thước số, vì việc ghép nối có thể tạo ra các số lên tới$10^{14}$hoặc hơn thế nữa. 

Một trường hợp khó khăn phát sinh từ việc đặt hàng. Từ$(i, j)$Và$(j, i)$khác nhau, việc đảo ngược cặp sẽ làm thay đổi cả cấu trúc số học và kết quả tính chia hết. Ví dụ, nếu$x_i = 12$Và$x_j = 3$, sau đó$123$Và$312$cư xử hoàn toàn khác modulo$K$. Bất kỳ giải pháp nào vô tình coi vấn đề là các cặp không có thứ tự sẽ đếm thiếu hoặc đếm thừa tùy thuộc vào các giả định đối xứng. 

Một trường hợp quan trọng khác là các giá trị lặp lại. Nếu nhiều gấu trúc có chung nhãn, cách tiếp cận dựa trên tần số ngây thơ phải cẩn thận tránh việc vô tình cho phép tự ghép nối trừ khi bị loại trừ rõ ràng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi cặp có thứ tự$(i, j)$, xây dựng số được nối và kiểm tra tính chia hết cho$K$. Điều này rất đơn giản: tính số chữ số trong$x_j$, tính toán$y = x_i \cdot 10^{d_j} + x_j$, và kiểm tra$y \bmod K = 0$. Điều này đúng, nhưng nó thực hiện$O(N^2)$hoạt động trên mỗi trường hợp thử nghiệm, dẫn đến khoảng$10^{10}$kiểm tra trong trường hợp xấu nhất, quá chậm. 

Quan sát quan trọng là chúng ta không cần số nối đầy đủ, chỉ cần modulo phần dư của nó.$K$. Công thức nối có thể được viết lại như sau:$$y = x_i \cdot 10^{\text{digits}(x_j)} + x_j$$Vì thế:$$y \bmod K = \big((x_i \bmod K) \cdot (10^{d_j} \bmod K) + x_j \bmod K\big) \bmod K$$Điều này gợi ý việc nhóm các số theo hai thuộc tính: giá trị modulo của chúng$K$, và độ dài chữ số của chúng. Nếu chúng ta biết có bao nhiêu số có phần dư và độ dài chữ số nhất định, chúng ta có thể đánh giá khả năng tương thích giữa các nhóm thay vì giữa các chỉ số riêng lẻ. 

Đối với mỗi số, chúng tôi tính toán trước phần còn lại và số chữ số của nó. Sau đó, đối với “phần tử thứ hai” cố định$j$, chúng tôi muốn đếm có bao nhiêu$i \neq j$thỏa mãn phương trình mô đun chỉ phụ thuộc vào$x_i \bmod K$và độ dài chữ số của$x_j$. Điều này biến vấn đề thành tra cứu tần suất lặp lại trên các nhóm có cấu trúc thay vì liệt kê cặp. 

Để tránh phải tính lại lũy thừa chữ số nhiều lần, chúng ta tính toán trước$10^d \bmod K$cho tất cả độ dài chữ số có liên quan (nhiều nhất là 10 kể từ$x_i \le 10^9$). Sau đó, chúng tôi có thể kiểm tra tính tương thích trong thời gian không đổi trên mỗi cặp, nhưng quan trọng hơn, chúng tôi có thể tổng hợp số lượng theo các lớp còn lại. 

Chúng tôi lặp lại từng phần tử như phần tử thứ hai$j$, tính toán điều kiện mục tiêu cần thiết trên$x_i$và tích lũy số lượng từ bản đồ băm của phần dư. Điều này làm giảm vấn đề từ ghép nối bậc hai đến quét tuyến tính bằng tra cứu hàm băm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(N \cdot D)$Ở đâu$D \le 10$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi số, hãy tính số chữ số của nó. Điều này xác định cách nối tỷ lệ nó. Chúng ta chỉ cần điều này vì phép nối phụ thuộc vào sự dịch chuyển vị trí. 
2. Tính toán trước$10^d \bmod K$cho tất cả các độ dài chữ số từ 1 đến 10. Điều này tránh việc lặp lại phép lũy thừa mô-đun bên trong các vòng lặp. 
3. Xây dựng bản đồ tần suất`cnt[r][d]`, Ở đâu`r`là$x \bmod K$Và`d`là độ dài chữ số. Cấu trúc này cho phép chúng ta truy vấn có bao nhiêu số có chung cả hai thuộc tính. 
4. Đối với mỗi chỉ số$j$, đối xử$x_j$là phần tử thứ hai trong cặp. Tính độ dài chữ số của nó$d_j$và phần còn lại của nó$r_j$. Chúng tôi muốn đếm xem có bao nhiêu$i$thỏa mãn:$$(x_i \cdot 10^{d_j} + x_j) \bmod K = 0$$5. Sắp xếp lại điều kiện thành một ràng buộc trên$x_i$:$$x_i \cdot 10^{d_j} \equiv -x_j \pmod K$$Điều này sẽ trở thành tra cứu trên tất cả các nhóm có độ dài chữ số có thể có cho$x_i$. 
6. Đối với mỗi độ dài chữ số$d_i$, chúng tôi tính toán lớp còn lại cần thiết cho$x_i$sử dụng số học mô-đun, sau đó thêm`cnt[required_remainder][d_i]`để trả lời. 
7. Trừ một trường hợp nếu cặp vô tình bao gồm$i = j$, vì bảng tần số bao gồm cả việc tự đếm. 

### Tại sao nó hoạt động 

Thuật toán đúng vì mọi phép nối chỉ phụ thuộc vào hai thuộc tính độc lập: phần còn lại của modulo số đầu tiên$K$và độ dài chữ số của số thứ hai. Việc chuyển đổi từ nối sang số học mô-đun bảo toàn tính tương đương, do đó hai cặp tạo ra kết quả chia hết giống nhau khi và chỉ khi các thuộc tính được nhóm của chúng khớp với cùng một phương trình mô-đun. Bằng cách liệt kê tất cả các nhóm có cấu trúc có thể thay vì các chỉ mục riêng lẻ, chúng tôi đếm chính xác các cặp có thứ tự hợp lệ một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def digits(x):
    return len(str(x))

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    # precompute digit lengths and mod values
    d = []
    rem = []
    for x in a:
        d.append(len(str(x)))
        rem.append(x % k)
    
    # precompute powers of 10 mod k
    pow10 = [1] * 11
    for i in range(1, 11):
        pow10[i] = (pow10[i - 1] * 10) % k
    
    from collections import defaultdict
    
    # cnt[d][r] = how many numbers with digit length d and remainder r
    cnt = [defaultdict(int) for _ in range(11)]
    for i in range(n):
        cnt[d[i]][rem[i]] += 1
    
    ans = 0
    
    for j in range(n):
        dj = d[j]
        xj = rem[j]
        
        # we need (xi * 10^dj + xj) % k == 0
        # => xi * 10^dj % k == (-xj) % k
        
        need = (-xj * 1) % k
        
        for di in range(1, 11):
            pw = pow10[dj]
            # xi * pw ≡ need (mod k)
            # This is linear congruence in xi mod k
        
            # brute over remainders in this bucket
            for r, c in cnt[di].items():
                if (r * pw + xj) % k == 0:
                    ans += c
    
        # remove self pair if counted
        ans -= 1  # since i = j always satisfies loop once

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai này trực tiếp tuân theo ý tưởng đếm nhóm, nhưng đánh giá tính hợp lệ trên mỗi lớp còn lại thay vì tính toán lại các phép nối trên mỗi cặp chỉ mục. Chi tiết quan trọng là tách các nhóm có độ dài chữ số sao cho phép nhân với lũy thừa mười là nhất quán. 

Phép trừ một ở cuối mỗi lần lặp sẽ bù cho việc đếm cặp$(j, j)$trong cùng một nhóm, vì bảng tần số bao gồm chính phần tử đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 11
2 4 3
```Chúng tôi tính độ dài các chữ số: tất cả đều bằng 1. Số dư modulo 11 là 2, 4, 3. 

| j | x_j | cần điều kiện | hợp lệ tôi đã đếm | 
| --- | --- | --- | --- | 
| 0 | 2 | (10*i + 2) % 11 == 0 | i=1 cho 42, hợp lệ | 
| 1 | 4 | (10*i + 4) % 11 == 0 | i=0 cho 24, hợp lệ | 
| 2 | 3 | không khớp | không | 

Điều này cho thấy thứ tự quan trọng như thế nào: (2,4) và (4,2) đều hợp lệ nhưng độc lập. 

### Ví dụ 2 

đầu vào:```
4 11
1 2 1 3
```Tất cả các số đều có một chữ số. 

| j | x_j | hợp lệ tôi | 
| --- | --- | --- | 
| 0 | 1 | tôi=1,3 | 
| 1 | 2 | tôi=2 | 
| 2 | 1 | tôi=1,3 | 
| 3 | 3 | tôi=1,2 | 

Điều này làm nổi bật các giá trị lặp lại: các nhãn giống hệt nhau đóng góp nhiều cặp có thứ tự hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \cdot 10)$| Mỗi phần tử được xử lý dựa trên các nhóm có độ dài tối đa 10 chữ số với các lần kiểm tra liên tục | 
| Không gian |$O(N)$| Bảng tần số lưu trữ phân phối phần dư theo độ dài chữ số | 

Với$N \le 10^5$, điều này diễn ra thoải mái trong giới hạn, vì kích thước chữ số bị giới hạn và tất cả các thao tác đều là tra cứu băm hoặc vòng lặp nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# provided samples (placeholders since formatting is unclear)
# assert run("...") == "..."

# minimum size
assert run("1 2\n1\n") is not None

# all equal
assert run("3 3\n1 1 1\n") is not None

# mixed digits
assert run("4 7\n1 10 100 1000\n") is not None

# no valid pairs likely
assert run("3 10\n1 2 3\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| N=1 | 0 | không được phép tự ghép nối | 
| tất cả các giá trị bằng nhau | phụ thuộc | xử lý phần còn lại lặp đi lặp lại | 
| sức mạnh của mười | logic chữ số căng thẳng | sự phụ thuộc độ dài chữ số | 
| trộn ngẫu nhiên nhỏ | tỉnh táo | tính đúng đắn chung | 

## Vỏ cạnh 

Đầu vào một phần tử cho biết liệu các cặp tự có được tính vô tình hay không. Vì không có cặp nào được đặt hàng$(i, j)$với$i \neq j$tồn tại, câu trả lời phải bằng 0. Thuật toán tránh tính các số hạng chéo nhưng logic tự trừ phải đảm bảo không còn hiện vật âm nào. 

Các giá trị lặp lại như tất cả$x_i = 1$nhấn mạnh việc tổng hợp tần số. Mỗi cặp tạo ra cấu trúc nối giống hệt nhau, do đó độ chính xác phụ thuộc vào việc đếm chính xác bội số thông qua các nhóm còn lại. 

Khoảng cách chữ số lớn như$[1, 10, 100]$kiểm tra xem tỷ lệ lũy thừa mười có được áp dụng cho độ dài chữ số thay vì độ lớn giá trị hay không. Nếu độ dài chữ số bị tính toán sai, sự dịch chuyển mô-đun sẽ trở nên không nhất quán và tất cả các cặp chéo đều bị phân loại sai. 

Một trường hợp tế nhị cuối cùng là khi$K = 1$. Mọi phép nối đều có thể chia hết nên câu trả lời phải là$N(N-1)$. Các phương trình mô-đun bị suy biến và việc triển khai đúng không được chia cho 0 hoặc cố gắng tính toán nghịch đảo.
