---
title: "CF 104968F - Pizza Stack"
description: "Chúng ta đang sắp xếp một hoán vị của những chiếc pizza được dán nhãn từ 1 đến $n$, trong đó mỗi nhãn là bán kính của chiếc pizza đó. Sau khi chọn một đơn hàng, chúng tôi sẽ xem xét tất cả các cặp vị trí trong ngăn xếp: nếu một chiếc bánh pizza lớn hơn xuất hiện bên dưới một chiếc bánh nhỏ hơn, chúng tôi gọi cặp đó là "thích hợp"."
date: "2026-06-28T06:47:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104968
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 02-09-24 Div. 2 (Beginner)"
rating: 0
weight: 104968
solve_time_s: 24
verified: true
draft: false
---

[CF 104968F - Ngăn xếp Pizza](https://codeforces.com/problemset/problem/104968/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang sắp xếp hoán vị các loại pizza được dán nhãn từ 1 đến$n$, trong đó mỗi nhãn là bán kính của chiếc bánh pizza đó. Sau khi chọn một đơn hàng, chúng tôi sẽ xem xét tất cả các cặp vị trí trong ngăn xếp: nếu một chiếc bánh pizza lớn hơn xuất hiện bên dưới một chiếc bánh nhỏ hơn, chúng tôi gọi cặp đó là "thích hợp". 

Điều này hoàn toàn giống với việc đếm các nghịch đảo trong một hoán vị, ngoại trừ việc định nghĩa được diễn đạt theo thuật ngữ “dưới” và “trên” trong một ngăn xếp. Nếu chúng ta viết ngăn xếp từ trên xuống dưới dưới dạng hoán vị$p$, thì một cặp thích hợp là bất kỳ cặp nào$i < j$như vậy$p_i > p_j$. 

Vậy nhiệm vụ là: đếm xem có bao nhiêu hoán vị của$\{1,2,\dots,n\}$có chính xác$k$sự đảo ngược. 

Những hạn chế$n \le 1000$,$k \le 1000$ngay lập tức gợi ý rằng chúng tôi đang tính các hoán vị có số lượng đảo ngược giới hạn, không liệt kê các hoán vị. Việc liệt kê đầy đủ là không thể vì$n!$phát triển cực kỳ nhanh chóng, ngay cả đối với$n = 20$. Ràng buộc trên$k$là tín hiệu chính: mặc dù hoán vị lớn nhưng số lần đảo ngược mà chúng ta quan tâm lại nhỏ, do đó lập trình động trên$k$là hợp lý. 

Một trường hợp khó nhận thấy là khi$k = 0$hoặc$k = \frac{n(n-1)}{2}$. Vì$k = 0$, chỉ có hoán vị tăng dần mới có tác dụng, nên câu trả lời là 1. Đối với số lớn$k$, chúng ta phải nhớ rằng mặc dù$k \le 1000$, số lần đảo ngược tối đa có thể có cho$n = 1000$lớn hơn nhiều, vì vậy nhiều trạng thái tương ứng với các hoán vị hợp lệ bằng 0 và phải được xử lý một cách tự nhiên bằng giới hạn DP thay vì giả định. 

Một lỗi phổ biến khác là coi đây là một vấn đề chèn tổ hợp đơn giản mà không kiểm soát việc đếm quá mức. Mỗi bước chèn ảnh hưởng đến số lần đảo ngược theo cách phụ thuộc vào phạm vi, do đó, vị trí tham lam ngây thơ không đảm bảo tính chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ tạo ra tất cả các hoán vị về kích thước$n$và đếm số lần đảo ngược cho mỗi lần, tăng bộ đếm khi nó bằng$k$. Điều này đúng nhưng không khả thi. Thậm chí$n = 12$đã sản xuất rồi$479001600$hoán vị và tính toán nghịch đảo trên mỗi hoán vị sẽ nhân chi phí đó lên gấp bội, dẫn đến sự bùng nổ vượt xa mọi giới hạn thời gian. 

Quan sát chính là xây dựng các hoán vị tăng dần. Giả sử chúng ta đã biết có bao nhiêu cách sắp xếp$i-1$các phần tử có số lượng đảo ngược nhất định. Khi chúng ta chèn phần tử$i$, chúng ta chọn một vị trí trong dãy hiện tại. Đặt$i$ở vị trí$j$đóng góp chính xác$i - 1 - j$sự đảo ngược mới, bởi vì tất cả các phần tử sau vị trí$j$nhỏ hơn$i$tạo ra sự đảo ngược. Từ$i$là phần tử lớn nhất trong số phần tử đầu tiên$i$, mọi phần tử bên phải của nó đều nhỏ hơn, do đó mỗi phần tử bên phải đóng góp một phép nghịch đảo. 

Điều này làm giảm vấn đề thành công thức lập trình động tiêu chuẩn về kích thước tiền tố và số lượng đảo ngược. Cấu trúc này giống hệt với việc đếm các hoán vị bằng số nghịch đảo, còn được gọi là số Mahonian. 

Chúng tôi duy trì một DP nơi$dp[i][j]$là số hoán vị của$\{1..i\}$với chính xác$j$sự đảo ngược. Chuyển đổi đến từ việc chèn phần tử$i$vào tất cả các vị trí có thể có trong một hoán vị có kích thước$i-1$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| DP tối ưu |$O(nk)$|$O(nk)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định bảng DP trong đó mỗi trạng thái ghi lại số lượng hoán vị của độ dài tiền tố đạt được số lượng đảo ngược nhất định. 

### bước 

1. Khởi tạo bảng kích thước DP$(n+1) \times (k+1)$với số không. Bộ$dp[0][0] = 1$. 

Điều này thể hiện rằng có chính xác một cách để sắp xếp các phần tử bằng 0 với độ đảo bằng 0: hoán vị trống. 
2. Lặp lại kích thước tiền tố$i$từ 1 đến$n$. 

Ở mỗi bước, chúng tôi kết hợp phần tử$i$là giá trị lớn nhất trong tiền tố hiện tại. 
3. Đối với mỗi$i$, tính toán chuyển tiếp sang$dp[i]$từ$dp[i-1]$. 

Chúng tôi xem xét việc chèn$i$vào mọi vị trí có thể trong một hoán vị độ dài$i-1$. Nếu chúng ta chèn nó$pos$vị trí từ bên phải, nó tạo ra chính xác$pos$những nghịch đảo mới. 
4. Đối với mỗi lần đảo ngược$j$từ 0 đến$k$, tích lũy đóng góp từ các lần chèn hợp lệ:$$dp[i][j] = \sum_{x=0}^{\min(j, i-1)} dp[i-1][j-x]$$Đây$x$đại diện cho số lượng đảo ngược được đóng góp bằng cách đặt$i$và nó nằm trong khoảng từ 0 (đặt ở cuối) đến$i-1$(đặt ở phía trước). 
5. Để tính toán điều này một cách hiệu quả, hãy sử dụng tổng tiền tố trên$dp[i-1]$. 

Điều này tránh việc tính toán lại tổng cho mỗi$j$, giảm độ phức tạp từ$O(n^2k)$ĐẾN$O(nk)$. 
6. Trở về$dp[n][k]$modulo$10^9 + 7$. 

### Tại sao nó hoạt động 

Ở bước$i$, mọi hoán vị hợp lệ của kích thước$i$được hình thành duy nhất bằng cách lấy hoán vị kích thước$i-1$và chèn$i$tại đúng một vị trí. Việc chèn đó đóng góp một số lượng đảo ngược mới xác định bằng với số phần tử được dịch chuyển sang trái của$i$. Điều này đưa ra sự phân biệt giữa các trạng thái kích thước$i-1$và chuyển sang trạng thái có kích thước$i$. DP đếm mỗi công trình chính xác một lần, do đó không xảy ra việc đếm quá mức hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

n, k = map(int, input().split())

# dp[j] = number of permutations for current i with j inversions
dp = [0] * (k + 1)
dp[0] = 1

for i in range(1, n + 1):
    new = [0] * (k + 1)
    window_sum = 0

    for j in range(0, k + 1):
        window_sum += dp[j]
        if window_sum >= MOD:
            window_su_ -= MOD

        if j >= i:
            window_sum -= dp[j - i]
            if window_sum < 0:
                window_sum += MOD

        new[j] = window_sum

    dp = new

print(dp[k] % MOD)
```DP là com
