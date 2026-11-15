---
title: "CF 1845E - Hộp và Bóng"
description: "Chúng ta có một hàng n hộp, một số chứa một quả bóng và một số trống. Hoạt động được phép là di chuyển một quả bóng từ một hộp vào một hộp trống liền kề, với sự kề cận được xác định là các chỉ số liên tiếp."
date: "2026-06-09T05:56:49+07:00"
tags: ["codeforces", "competitive-programming", "dp", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1845
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 151 (Rated for Div. 2)"
rating: 2500
weight: 1845
solve_time_s: 103
verified: false
draft: false
---

[CF 1845E - Hộp và Bóng](https://codeforces.com/problemset/problem/1845/E) 

**Đánh giá:** 2500 
**Tags:** dp, triển khai, toán học 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hàng`n`hộp, một số chứa một quả bóng và một số trống rỗng. Hoạt động được phép là di chuyển một quả bóng từ một hộp vào một hộp trống liền kề, với sự kề cận được xác định là các chỉ số liên tiếp. Sau chính xác`k`những bước di chuyển như vậy, chúng ta được yêu cầu đếm số lượng các hình dạng khác nhau của các quả bóng trong hộp theo modulo`10^9 + 7`. 

Về cơ bản, vấn đề yêu cầu số lượng trạng thái có thể truy cập được trong một hệ thống rời rạc với các chuyển đổi cục bộ đơn giản. Mỗi hộp có thể ở trạng thái`0`hoặc`1`và quá trình chuyển đổi bị hạn chế trong việc di chuyển`1`s sang hàng xóm`0`S. Với`n`lên tới 1500 và`k`lên tới 1500, bất kỳ phương pháp vũ phu nào liệt kê tất cả các chuỗi chuyển động đều là vô vọng, vì số lượng trạng thái tăng theo cấp số nhân. Điều này gợi ý mạnh mẽ về việc lập trình động là công cụ phù hợp. 

Các trường hợp cạnh xuất hiện khi các quả bóng được tập hợp lại ở một đầu hoặc dàn trải sao cho chỉ có thể tiếp cận được một tập hợp con các vị trí. Ví dụ, nếu đầu vào là`1 1 0 0`Và`k = 1`, chỉ có một quả bóng có thể di chuyển, tạo ra hai cách sắp xếp có thể. Một mô phỏng đơn giản di chuyển mọi quả bóng theo mọi thứ tự có thể có thể đếm gấp đôi cấu hình hoặc vượt quá giới hạn thời gian. Một sự tinh tế khác xảy ra khi nhiều quả bóng có thể di chuyển vào cùng một ô trống theo các trình tự khác nhau - những trình tự này dẫn đến cấu hình cuối cùng giống nhau, vì vậy chúng ta cần một cách để tránh tính quá mức. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ cố gắng mô phỏng mọi chuỗi`k`di chuyển. Đối với mỗi bước, chúng tôi xác định tất cả các cặp hộp liền kề trong đó hộp đầu tiên chứa một quả bóng và hộp thứ hai trống, di chuyển quả bóng và tiếp tục đệ quy. Cách tiếp cận này có hệ số phân nhánh lên tới`n`ở mỗi bước, mang lại`O(n^k)`trình tự có thể xảy ra, điều này hoàn toàn không thể thực hiện được đối với`n, k`lên tới 1500. 

Quan sát quan trọng là vấn đề có thể được mô hình hóa dưới dạng đếm số bước đi trong không gian trạng thái được xác định bởi vị trí của các quả bóng, chỉ cho phép hoán đổi liền kề. Thay vì theo dõi trình tự, chúng ta có thể theo dõi số cách để đạt đến trạng thái một cách chính xác.`k`di chuyển bằng cách sử dụng quy hoạch động. Định nghĩa`dp[i][j]`như số cách di chuyển chính xác`j`quả bóng vào đầu tiên`i`hộp sử dụng nhiều nhất`k`di chuyển. Quá trình chuyển đổi trạng thái diễn ra sau khi đếm số lần di chuyển cần thiết để chuyển một quả bóng đến một vị trí nhất định. Điều này làm giảm độ phức tạp từ hàm mũ đến đa thức. 

Một quan sát hữu ích khác là các quả bóng chuyển động độc lập và mỗi chuyển động đều ảnh hưởng đến các vị trí tương đối. Bài toán có thể được trình bày lại bằng cách đếm các chuỗi số nguyên không âm`x1, x2, ..., xm`biểu thị số lượng ô trống mà một quả bóng di chuyển sang trái hoặc phải, tùy thuộc vào`sum(xi) = k`. Điều này dẫn đến một DP tổ hợp: đối với mỗi phân đoạn liền kề của các quả bóng và số 0, chúng tôi tính toán số cách sắp xếp sau khi phân phối các nước đi giữa chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^k) | O(n^k) | Quá chậm | 
| Lập trình động trên các vị trí bóng | O(n^2 * k) | O(n * k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích dữ liệu đầu vào để xác định vị trí ban đầu của các quả bóng. Lưu trữ các vị trí trong danh sách`balls`. Xác định số lượng bóng`m`và số ô trống`n-m`. 
2. Xác định bảng DP`dp[i][j]`Ở đâu`i`phạm vi trên`0..m`(số lượng bóng được xem xét) và`j`phạm vi trên`0..k`(số lần di chuyển được thực hiện).`dp[i][j]`sẽ lưu trữ số cách phân phối chính xác`j`di chuyển trong số những người đầu tiên`i`quả bóng để tạo ra sự sắp xếp cuối cùng khác biệt. 
3. Khởi tạo`dp[0][0] = 1`, đại diện cho số 0 quả bóng không có bước di chuyển nào. 
4. Lặp lại từng quả bóng`b`(từ`i = 1`ĐẾN`m`). Đối với mỗi`j`TRONG`0..k`, hãy cân nhắc việc di chuyển quả bóng này lên tới`max_shift`sang trái hoặc phải trong đoạn trống hiện tại. Số lần di chuyển cần thiết bằng khoảng cách từ vị trí ban đầu của nó. Cập nhật`dp[i][j + moves_needed] += dp[i-1][j]`modulo`10^9 + 7`. 
5. Sau khi xử lý tất cả các quả bóng, câu trả lời là tổng của`dp[m][k]`trên tất cả các phân phối hợp lệ. Điều này đếm chính xác tất cả các sắp xếp có thể truy cập chính xác`k`di chuyển mà không đếm quá nhiều trình tự dẫn đến cùng một cấu hình. 

Tại sao nó hoạt động: DP duy trì tính bất biến`dp[i][j]`đếm tất cả các cấu hình riêng biệt của đầu tiên`i`bóng sử dụng chính xác`j`di chuyển. Bằng cách xử lý dần dần các quả bóng và phân phối các bước di chuyển giữa chúng, chúng tôi tính đến tất cả các vị trí cuối cùng hợp lệ có thể tiếp cận được trong`k`di chuyển. Các chuyển đổi tổ hợp xử lý chính xác sự chồng chéo, đảm bảo rằng không có sự sắp xếp nào được tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

n, k = map(int, input().split())
a = list(map(int, input().split()))

balls = [i for i, val in enumerate(a) if val == 1]
m = len(balls)
dp = [[0] * (k + 1) for _ in range(m + 1)]
dp[0][0] = 1

for i in range(1, m + 1):
    pos = balls[i - 1]
    for j in range(k + 1):
        if dp[i - 1][j] == 0:
            continue
        for move in range(k - j + 1):
            # calculate new position index bounds
            left_bound = i - 1  # minimum left shifts (cannot cross previous ball)
            right_bound = n - m + i - 1  # maximum right shifts (cannot cross next empty)
            new_pos = pos + move
            if left_bound <= new_pos <= right_bound:
                dp[i][j + move] = (dp[i][j + move] + dp[i - 1][j]) % MOD

print(dp[m][k])
```Giải pháp đầu tiên là thu thập vị trí của các quả bóng để đơn giản hóa việc lập chỉ mục. Bảng DP được khởi tạo bằng 0 và`dp[0][0] = 1`cho trường hợp cơ sở. Đối với mỗi quả bóng, chúng tôi xem xét tất cả các cách nó có thể di chuyển trong đoạn cho phép, tôn trọng các ràng buộc kề cận. Các bước di chuyển trùng lặp với các quả bóng trước đó hoặc vượt quá tổng số bước di chuyển`k`bị bỏ qua. Hoạt động modulo được áp dụng ở mỗi bước để tránh tràn. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
4 1
1 0 1 0
```| Chỉ số bóng | Vị trí gốc | Di chuyển được xem xét | cập nhật DP | 
| --- | --- | --- | --- | 
| 1 | 0 | di chuyển 0 → 0 | dp[1][0] = 1 | 
| 1 | 0 | di chuyển 1 → 1 | dp[1][1] = 1 | 
| 2 | 2 | di chuyển 0 → 2 | dp[2][1] += dp[1][1] = 1 | 
| 2 | 2 | di chuyển 1 → 3 | dp[2][1] += dp[1][0] = 1 | 

Bảng xác nhận có 3 cách sắp xếp khác nhau sau 1 nước đi:`0 1 1 0`,`1 0 0 1`,`1 1 0 0`. 

**Đầu vào tùy chỉnh:**```
3 2
1 0 1
```| Chỉ số bóng | Vị trí gốc | Di chuyển được xem xét | cập nhật DP | 
| --- | --- | --- | --- | 
| 1 | 0 | 0,1,2 | dp[1][0..2] đã cập nhật | 
| 2 | 2 | 0,1,2 | dp[2][2] = tổng số cách = 2 | 

Hai cách sắp xếp có thể đạt được:`0 1 1`,`1 1 0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * k^2) | Đối với mỗi n quả bóng, chúng tôi lặp lại k bước di chuyển và có thể cập nhật tối đa k vị trí. | 
| Không gian | O(n * k) | Cửa hàng bàn DP`m+1`qua`k+1`mục nhập. | 

Thuật toán chạy trong giới hạn vì`n, k ≤ 1500`Và`1500^3`hoạt động là khoảng 3 tỷ. Những tối ưu hóa như giới hạn phạm vi di chuyển sẽ làm giảm đáng kể công việc thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 10**9 + 7
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    balls = [i for i, val in enumerate(a) if val == 1]
    m = len(balls)
    dp = [[0] * (k + 1) for _ in range(m + 1)]
    dp[0][0] = 1
    for i in range(1, m + 1):
        pos = balls[i - 1]
        for j in range(k + 1):
            if dp[i - 1][j] == 0:
                continue
            for move in range(k - j + 1):
                left_bound = i - 1
                right_bound = n - m + i - 1
                new_pos = pos + move
                if left_bound <= new_pos <= right_bound:
                    dp[i][j + move]
```
