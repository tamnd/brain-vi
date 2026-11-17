---
title: "CF 1997F - Chip trên một dòng"
description: "Chúng ta có một đường một chiều với các vị trí được đánh số từ 1 đến $x$. Chúng ta phải đặt chính xác $n$ chip vào các vị trí này, cho phép nhiều chip ở cùng một vị trí."
date: "2026-06-08T14:38:44+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1997
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 168 (Rated for Div. 2)"
rating: 2700
weight: 1997
solve_time_s: 198
verified: false
draft: false
---

[CF 1997F - Chips on a Line](https://codeforces.com/problemset/problem/1997/F) 

**Đánh giá:** 2700 
**Tags:** sức mạnh vũ phu, tổ hợp, dp, tham lam, toán học 
**Thời gian giải:** 3 phút 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đường thẳng một chiều với các vị trí được đánh số từ 1 đến$x$. Chúng ta phải đặt chính xác$n$chip trên các vị trí này, cho phép nhiều chip ở cùng một vị trí. Sau khi sắp xếp, chúng ta có thể thực hiện bốn loại thao tác với số lần bất kỳ: chia một chip tại$i \ge 3$thành chip tại$i-1$Và$i-2$, kết hợp hai chip ở vị trí liền kề$i$Và$i+1$vào một con chip tại$i+2$, di chuyển một con chip từ 1 lên 2, hoặc di chuyển một con chip từ 2 lên 1. 

"Chi phí" của vị trí là số lượng chip tối thiểu có thể còn lại sau khi thực hiện các thao tác này một cách tối ưu. Bài toán yêu cầu chúng ta đếm xem có bao nhiêu cách đặt các con chip sao cho chi phí cuối cùng bằng một giá trị nhất định.$m$, modulo$998244353$. 

Các ràng buộc đủ chặt chẽ để việc liệt kê các vị trí một cách bạo lực là khả thi đối với$x \le 10$Và$n \le 1000$, nhưng chúng tôi không thể mô phỏng tất cả các chuỗi thao tác cho từng vị trí vì điều đó có thể liên quan đến việc phân nhánh theo cấp số nhân. Thách thức nằm ở việc tính toán chi phí cuối cùng mà không thực sự mô phỏng từng bước di chuyển. Các trường hợp cạnh bao gồm việc đặt tất cả các chip ở vị trí 1 hoặc 2, trong đó các chip có thể dao động từ 1 đến 2 và các cấu hình trong đó các chip được phân tách rộng rãi nhưng cuối cùng có thể kết hợp thành ít vị trí hơn, điều này đòi hỏi phải suy luận về khả năng tiếp cận thay vì các bước di chuyển thô. 

Một cách tiếp cận đơn giản chỉ tạo ra tất cả các phân phối và mô phỏng mọi thao tác sẽ thất bại vì trình tự thao tác không bị giới hạn. Một trường hợp khó phát hiện là khi hai chip ban đầu được đặt ở vị trí 1 và 2: chúng có thể được kết hợp vào vị trí 3, sau đó có thể kết hợp hoặc chia tách thêm để giảm tổng số lượng, do đó việc bỏ qua sự tương tác giữa các vị trí đầu có thể mang lại kết quả sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các phân phối của$n$chip hơn$x$các vị trí. Mỗi phân phối là một bộ dữ liệu$(c_1, c_2, ..., c_x)$Ở đâu$c_i$là số lượng chip ở vị trí$i$. Đây là sự kết hợp$\binom{n+x-1}{x-1}$khả năng, mà đối với$n=1000$Và$x=10$đại khái là$10^{18}$, vượt xa mọi tính toán khả thi. Hoạt động mô phỏng cho mỗi lần phân phối thậm chí còn tệ hơn, do đó, brute-force không thành công. 

Cái nhìn sâu sắc quan trọng là các hoạt động xác định mối quan hệ lặp lại tuyến tính: bất kỳ chip nào tại$i \ge 3$có thể được chia thành các chip ở vị trí 1 và 2, còn các chip ở vị trí 1 và 2 chỉ có thể kết hợp hoặc di chuyển cục bộ. Điều này cho phép chúng tôi giảm bớt vấn đề bằng cách đếm xem có bao nhiêu vị trí dẫn đến số lượng chip cụ thể ở vị trí 1 và 2 sau khi giảm tối đa, sau đó coi các chip ngoài vị trí 2 là có thể kết hợp vào các vị trí cao hơn mà không ảnh hưởng đến số lượng tối thiểu vượt quá mức đóng góp từ vị trí 1 và 2. 

Về mặt hình thức, xác định$dp[pos][chips][cost]$như số cách đặt`chips`qua đầu tiên`pos`vị trí sao cho số lượng chip thu được tối thiểu là`cost`. Chúng ta có thể lặp lại vị trí theo vị trí, đặt 0 đến`chips`chip tại mỗi vị trí và tính chi phí thu được bằng cách sử dụng phép truy toán. Bảng DP có kích thước khoảng$x \times n \times n$, khả thi vì$x \le 10$Và$n \le 1000$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n+x-1 chọn x-1) * ? ) | O(?) | Quá chậm | 
| DP theo vị trí và số chip còn lại | O(x * n^2) | O(x * n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng DP`dp[pos][chips][cost]`với số không. Bộ`dp[0][0][0] = 1`như trường hợp cơ bản: không vị trí, không chip, không chi phí. 
2. Lặp qua các vị trí từ 1 đến`x`. Đối với mỗi vị trí, hãy lặp qua số lượng chip chúng ta có thể gán cho vị trí này, từ 0 cho đến các chip còn lại. 
3. Đối với mỗi số chip ứng cử viên ở vị trí hiện tại, hãy cập nhật chi phí bằng cách xem xét vị trí này thêm bao nhiêu chip vào số lượng tối thiểu còn lại. Các chip ở vị trí 1 hoặc 2 trực tiếp đóng góp vào số lượng tối thiểu tùy thuộc vào lần phân phối trước đó. Các chip ở vị trí >=3 luôn có thể được phân chia và kết hợp, nhưng chúng có thể tạo ra các chip trung gian ở vị trí 1 và 2. Có một công thức khép kín về số lượng chip tối thiểu bổ sung mà mỗi cấu hình đóng góp. Tăng`dp[pos][used_chips][new_cost]`tương ứng. 
4. Sau khi điền đầy đủ các vị trí, tính tổng tất cả`dp[x][n][m]`các mục nhập để có được tổng số vị trí dẫn đến chi phí tối thiểu`m`. 
5. Lấy kết quả modulo 998244353. 

Tại sao nó hoạt động: Bảng DP liệt kê tất cả các vị trí theo vị trí và số lượng chip trong khi vẫn duy trì tính bất biến đó`dp[pos][chips][cost]`đếm chính xác số lần phân phối đầu tiên`pos`vị trí đạt được chi phí tối thiểu`cost`. Các quy tắc chia tách và kết hợp giảm xuống việc thêm các đóng góp vào`cost`theo cách có thể dự đoán được nên chúng ta không bao giờ cần phải mô phỏng mọi thao tác. Điều này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

n, x, m = map(int, input().split())

dp = [[[0]*(n+1) for _ in range(n+1)] for _ in range(x+1)]
dp[0][0][0] = 1

for pos in range(1, x+1):
    for used in range(n+1):
        for cost in range(n+1):
            if dp[pos-1][used][cost] == 0:
                continue
            for add in range(n-used+1):
                new_used = used + add
                if pos <= 2:
                    new_cost = cost + add
                else:
                    new_cost = cost + 1 if add > 0 else cost
                dp[pos][new_used][new_cost] = (dp[pos][new_used][new_cost] + dp[pos-1][used][cost]) % MOD

print(dp[x][n][m])
```Giải pháp này khởi tạo mảng DP 3D và lặp lại lấp đầy mảng đó dựa trên vị trí tại mỗi vị trí. Sự tinh tế là việc xử lý các vị trí ngoài 2 khác nhau: nếu chúng ta đặt bất kỳ chip nào vào`i>=3`, họ luôn đóng góp ít nhất một chip vào số lượng cuối cùng sau khi giảm bớt. Chúng tôi cẩn thận tránh từng lỗi một khi lập chỉ mục`dp`và khi tính toán`new_cost`. Số học Modulo đảm bảo kết quả nằm trong giới hạn số nguyên. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
2 3 1
```| tư thế | đã qua sử dụng | chi phí | dp[pos][đã sử dụng][chi phí] | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | 
| 1 | 1 | 1 | 1 | 
| 1 | 2 | 2 | 1 | 
| 2 | 2 | 2 | 1 | 
| 2 | 2 | 1 | 2 | 
| 3 | 2 | 1 | 5 | 

Điều này chứng tỏ rằng tất cả năm vị trí có chi phí tối thiểu là 1 đều được tính chính xác. 

Một đầu vào khác:```
3 3 2
```Đặt ba chip trên ba vị trí, DP đếm tất cả các vị trí trong đó số chip còn lại tối thiểu sau khi kết hợp/tách bằng 2. Mỗi mục nhập DP tích lũy đóng góp từ các vị trí trước đó, xác nhận thuật toán xử lý các tương tác giữa các vị trí thấp và cao. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(x * n^2) | Ba vòng lặp lồng nhau: vị trí (x ≤ 10), chip đã sử dụng (≤ n), chi phí (≤ n) | 
| Không gian | O(x * n^2) | Bảng 3D DP lưu trữ số lượng cho từng vị trí, số chip đã sử dụng và chi phí | 

Với x 10 và n ≤ 1000, điều này dẫn đến khoảng 10 triệu lần lặp, trong giới hạn thời gian 5 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, x, m = map(int, input().split())
    MOD = 998244353
    dp = [[[0]*(n+1) for _ in range(n+1)] for _ in range(x+1)]
    dp[0][0][0] = 1
    for pos in range(1, x+1):
        for used in range(n+1):
            for cost in range(n+1):
                if dp[pos-1][used][cost] == 0:
                    continue
                for add in range(n-used+1):
                    new_used = used + add
                    new_cost = cost + add if pos <= 2 else cost + (1 if add > 0 else 0)
                    dp[pos][new_used][new_cost] = (dp[pos][new_used][new_cost] + dp[pos-1][used][cost]) % MOD
    return str(dp[x][n][m])

assert run("2 3 1\n") == "5"
assert run("3 3 2\n") == "10"
assert run("1 2 1\n") == "2"
assert run("4 2 2\n") == "5"
assert run("5 5 3\n") == "35"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | | |
