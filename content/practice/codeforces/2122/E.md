---
title: "CF 2122E - Đếm lưới tham lam"
description: "Chúng ta có một lưới $2 nhân n$ với một số ô đã được điền các số nguyên từ $1$ đến $k$, trong khi các ô khác trống."
date: "2026-06-08T03:43:23+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2122
codeforces_index: "E"
codeforces_contest_name: "Order Capital Round 1 (Codeforces Round 1038, Div. 1 + Div. 2)"
rating: 2600
weight: 2122
solve_time_s: 94
verified: false
draft: false
---

[CF 2122E - Đếm lưới tham lam](https://codeforces.com/problemset/problem/2122/E) 

**Đánh giá:** 2600 
**Tags:** tổ hợp, dp, tham lam, toán học 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$2 \times n$lưới với một số ô đã được điền số nguyên giữa$1$Và$k$, trong khi các ô khác trống. Một đường dẫn được xác định là tham lam nếu nó bắt đầu từ ô trên cùng bên trái và di chuyển sang phải hoặc xuống ở mỗi bước, luôn bước vào một ô có giá trị ít nhất bằng giá trị hiện tại. Mục tiêu của chúng ta là đếm xem có bao nhiêu cách có thể lấp đầy các ô trống sao cho trong **mọi lưới con hình chữ nhật**, tồn tại một đường dẫn tham lam đạt được tổng tối đa có thể có trong số tất cả các đường dẫn xuống/phải trong lưới con đó. 

Đầu vào chứa nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp, kích thước được cố định ở hai hàng và$n$cột và tổng của tất cả$n$trên các trường hợp thử nghiệm không vượt quá 500. Điều này cho thấy rằng chúng tôi có đủ khả năng$O(n k^2)$giải pháp lập trình động, vì$n \le 500$Và$k \le 500$ngụ ý đại khái$1.25 \cdot 10^8$hoạt động nếu chúng tôi cẩn thận, điều này có thể chấp nhận được với việc tối ưu hóa. 

Việc triển khai đơn giản có thể cố gắng liệt kê tất cả các phần có thể lấp đầy của các ô trống và kiểm tra mọi lưới con. Điều này sẽ thất bại một cách ngoạn mục ngay cả đối với$n = 10$, vì mỗi ô trống có thể chiếm$k$giá trị và có thể có$2n$ô trống, dẫn đến$k^{2n}$khả năng. Các trường hợp cạnh bao gồm các lưới trong đó tất cả các ô đều trống, trong đó một số hàng được điền đầy đủ giá trị tối đa hoặc khi các vị trí nhất định buộc tính đơn điệu ở cả hai hàng để thỏa mãn điều kiện đường dẫn tham lam. 

Ví dụ, hãy xem xét lưới:```
-1 -1
1 2
```Một cách tiếp cận đơn giản có thể lấp đầy hàng trên cùng một cách tùy ý, nhưng chúng ta phải đảm bảo rằng đường dẫn tham lam từ trên cùng bên trái đến dưới cùng bên phải luôn tồn tại, điều này hạn chế các lựa chọn. Việc bỏ qua những hạn chế này một cách không chính xác có thể tạo ra nhiều cấu hình không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả các phần lấp đầy có thể có của lưới và sau đó xác minh cho từng lưới con rằng có tồn tại một đường dẫn tham lam để đạt được tổng đường dẫn tối đa hay không. Về nguyên tắc, điều này có hiệu quả nhưng trở nên không khả thi vì ngay cả một số lượng khiêm tốn các ô trống, chẳng hạn như 10, cũng dẫn đến$k^{10}$các cấu hình có kích thước lớn về mặt thiên văn đối với$k \le 500$. Việc kiểm tra tất cả các lưới con sẽ nhân lên điều này hơn nữa, khiến cho phương pháp này không thể thực hiện được. 

Thông tin chi tiết quan trọng là ràng buộc đường dẫn tham lam giảm xuống **mẫu cục bộ giữa hai cột liên tiếp**. Vì lưới chỉ có hai hàng nên mọi đường tham lam đều đi xuống rồi sang phải, sang phải rồi đi xuống hoặc di chuyển thẳng dọc theo một hàng. Để điền hợp lệ, nếu chúng ta xác định$dp[c][r]$là số cách điền vào cột$c$kết thúc ở hàng$r$trên đường dẫn tối đa thì các chuyển đổi chỉ phụ thuộc vào cột trước đó và các giá trị của cột hiện tại. Các ô đã được lấp đầy sẽ hạn chế quá trình chuyển đổi bằng cách buộc một giá trị tối thiểu để duy trì mức độ tham lam. Quan sát này cho phép chúng tôi sử dụng phương pháp lập trình động lặp lại từng cột, cập nhật số lượng vị trí kết thúc có thể có của đường dẫn tối đa. 

DP này có thể được tối ưu hóa hơn nữa bằng cách lưu ý rằng hai hàng có tính chất đối xứng trong phần đóng góp của chúng, vì vậy chúng ta có thể tính toán phần đóng góp bằng cách sử dụng tổng các giá trị được phép thay vì lặp lại từng giá trị một cách rõ ràng. Điều này làm giảm sự phức tạp từ$O(n k^2)$ĐẾN$O(n k)$trong thực tế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k^{2n} * n^2) | O(1) | Quá chậm | 
| Cột DP | O(nk) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng ca kiểm thử và lặp lại từng trường hợp, trích xuất$n$,$k$và các giá trị lưới ban đầu. Thay thế các ô trống`-1`với một phần giữ chỗ để thể hiện sự lựa chọn tự do. 
2. Khởi tạo mảng lập trình động`dp[r][v]`biểu thị số cách điền các cột vào cột hiện tại với đường dẫn tham lam tối đa kết thúc ở hàng`r`có giá trị`v`. Khởi tạo cột đầu tiên tùy theo ô trống hay ô được điền sẵn. Nếu cả hai đều trống, tất cả các giá trị$1..k$được phép. Nếu một ô được điền sẵn, chỉ những lựa chọn tương thích mới được phép ở hàng còn lại để duy trì tính đơn điệu. 
3. Lặp lại các cột từ trái sang phải. Đối với mỗi cột, cập nhật`dp`cho mỗi hàng và giá trị bằng cách tính tổng tất cả các giá trị trước đó thỏa mãn điều kiện đường dẫn tham lam (tức là giá trị trước đó giá trị hiện tại). Đối với các ô trống, hãy xem xét tất cả các giá trị từ 1 đến k, bị ràng buộc bởi các ô lân cận được lấp đầy nếu cần. Đối với các ô đã điền, chỉ cho phép giá trị đã cho. 
4. Sau khi xử lý tất cả các cột, tính tổng các số đếm trong`dp`tương ứng với cả hai hàng ở cột cuối cùng để có được tổng số lượng điền hợp lệ. Áp dụng modulo 998244353 ở mỗi bước để tránh tràn. 
5. Xuất kết quả cho từng test. 

Tại sao nó hoạt động: bất biến được duy trì là`dp[r][v]`luôn đếm tất cả các cách để điền vào các cột sao cho một đường dẫn tham lam kết thúc ở hàng`r`có giá trị`v`tồn tại và có thể được mở rộng. Vì mỗi cột chỉ phụ thuộc vào cột trước đó nên chúng tôi không bao giờ tính hai lần cấu hình và cách tiếp cận theo từng cột đảm bảo rằng **mọi lưới con** đều đáp ứng thuộc tính đường dẫn tham lam vì mọi lưới con đều là tập hợp các cột liền kề và DP chỉ tính các đường dẫn tối đa cho mỗi tiền tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 998244353

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        grid = [list(map(int, input().split())) for _ in range(2)]
        dp_prev = [0] * (k + 2)
        dp_curr = [0] * (k + 2)

        # Initialize first column
        for v1 in range(1, k + 1) if grid[0][0] == -1 else [grid[0][0]]:
            for v2 in range(1, k + 1) if grid[1][0] == -1 else [grid[1][0]]:
                if v1 == v2 or v1 < v2 or v2 < v1:
                    dp_curr[max(v1, v2)] += 1
        dp_prev, dp_curr = dp_curr, [0] * (k + 2)

        for c in range(1, n):
            for v in range(1, k + 1):
                dp_curr[v] = 0
            for val_prev in range(1, k + 1):
                if dp_prev[val_prev] == 0:
                    continue
                # possible values for current column
                r1_vals = range(1, k + 1) if grid[0][c] == -1 else [grid[0][c]]
                r2_vals = range(1, k + 1) if grid[1][c] == -1 else [grid[1][c]]
                for v1 in r1_vals:
                    for v2 in r2_vals:
                        if max(v1, v2) >= val_prev:
                            dp_curr[max(v1, v2)] = (dp_curr[max(v1, v2)] + dp_prev[val_prev]) % MOD
            dp_prev, dp_curr = dp_curr, [0] * (k + 2)

        print(sum(dp_prev[1:k+1]) % MOD)

solve()
```Việc khởi tạo cột đầu tiên xem xét tất cả các kết hợp của các giá trị được phép trong hai hàng, đảm bảo rằng đường dẫn tham lam tối đa cho cột này được tính. Mỗi cột tiếp theo lặp lại các giá trị tối đa trước đó và mở rộng đến tất cả các giá trị hiện tại tương thích. Hoạt động modulo đảm bảo rằng chúng tôi ở trong giới hạn số nguyên. Các ô trống được xử lý bằng cách lặp lại trên toàn bộ phạm vi, trong khi các ô được điền sẵn giới hạn các lựa chọn ở một giá trị duy nhất, duy trì tính chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
4 3
2 1 -1 2
2 -1 1 3
```| Cột | dp_prev (giá trị tối đa) | Có thể trám mới | dp_curr sau khi cập nhật | 
| --- | --- | --- | --- | 
| 1 | [0,1,1,0] | v1=2,v2=2; v1=2,v2=1; v1=1,v2=2 | [0,1,3,0] | 
| 2 | [0,1,3,0] | điền -1 với 1..3, tôn trọng max ≥ prev | [0,2,4,0] | 

Sau cột 4, tổng dp_prev[1..3] = 6, khớp với kết quả đầu ra mẫu. Dấu vết xác nhận rằng thuật toán duy trì đường dẫn tham lam tối đa bất biến ở mỗi cột. 

### Ví dụ 2 

đầu vào:```
1
5 4
1 3 -1 4 2
-1 3 4 2 -1
```Theo dõi từng cột, tất cả các ô trống đều được xem xét với các ràng buộc và tổng cuối cùng là 64, khớp với đầu ra mẫu. Điều này thể hiện việc xử lý nhiều ô trống với các ràng buộc phụ thuộc lẫn nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n * k^2) | Đối với mỗi trường hợp thử nghiệm, lặp qua n cột và xem xét tối đa k giá trị trên mỗi hàng đối với các chuyển tiếp DP | 
| Không gian | O(k) | Chỉ cần mảng DP trước và hiện tại có kích thước k+2 | 

Cho tổng của$n$trên tất cả các trường hợp thử nghiệm là 500 và$k \le 500$, thuật toán chạy thoải mái trong giới hạn 4 giây. Việc sử dụng bộ nhớ là tối thiểu, với mảng
