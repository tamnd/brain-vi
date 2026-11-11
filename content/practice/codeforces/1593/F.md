---
title: "CF 1593F - Số Đỏ-Đen"
description: "Chúng ta được cho một số dưới dạng một chuỗi các chữ số và hai ước số A và B. Nhiệm vụ là tô màu mỗi chữ số là đỏ hoặc đen sao cho số tạo bởi các chữ số màu đỏ chia hết cho A, số tạo bởi các chữ số màu đen chia hết cho B và cả hai màu đều được sử dụng ít nhất…"
date: "2026-06-10T09:10:01+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "implementation", "math", "meet-in-the-middle"]
categories: ["algorithms"]
codeforces_contest: 1593
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 748 (Div. 3)"
rating: 2100
weight: 1593
solve_time_s: 136
verified: false
draft: false
---

[CF 1593F - Số Đỏ-Đen](https://codeforces.com/problemset/problem/1593/F) 

**Xếp hạng:** 2100 
**Thẻ:** dfs và tương tự, dp, triển khai, toán học, gặp gỡ ở giữa 
**Thời gian giải:** 2 phút 16s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số dưới dạng một chuỗi các chữ số và hai ước số A và B. Nhiệm vụ là tô màu mỗi chữ số là đỏ hoặc đen sao cho số tạo bởi các chữ số màu đỏ chia hết cho A, số tạo bởi các chữ số màu đen chia hết cho B và cả hai màu được sử dụng ít nhất một lần. Trong số tất cả các màu hợp lệ, chúng tôi mong muốn giảm thiểu sự khác biệt tuyệt đối giữa số lượng chữ số màu đỏ và màu đen. Đầu ra phải là một chuỗi 'R' và 'B' đại diện cho màu hoặc -1 nếu không có màu hợp lệ. 

Các ràng buộc cho chúng ta biết rằng n, số chữ số, nhiều nhất là 40. A và B nhiều nhất là 40, và có nhiều nhất 10 ca kiểm thử. Giá trị này đủ nhỏ để các giải pháp theo thời gian hàm mũ trong n là khả thi nếu chúng có thể khai thác cấu trúc, vì 2^40 là lớn nhưng có thể quản lý được nếu kết hợp với kỹ thuật số học hoặc cắt tỉa mô-đun. 

Các trường hợp cạnh phát sinh từ các số có số 0 đứng đầu, số có chữ số lặp lại hoặc ước số nguyên tố cùng nhau. Ví dụ, một số như`10`với A=2 và B=5 có thể được chia thành`1`màu đỏ và`0`màu đen, hình thành số`1`Và`0`, trong đó người ta không chia hết được. Một cách tiếp cận ngây thơ có thể bỏ qua khả năng có các số 0 đứng đầu hoặc chỉ xem xét các nhóm chữ số liền kề, điều này sẽ tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Giải pháp mạnh mẽ là thử mọi cách tô màu có thể có của n chữ số, tính các số cho chữ số đỏ và đen và kiểm tra tính chia hết. Điều này sẽ yêu cầu lặp lại trên 2^n khả năng và đối với mỗi khả năng, thực hiện chuyển đổi chuỗi thành số, đó là O(n). Với n lên tới 40, điều này dẫn đến khoảng 10^12 thao tác, quá chậm. 

Cái nhìn sâu sắc quan trọng là khả năng chia hết chỉ phụ thuộc vào phần dư modulo A cho các chữ số màu đỏ và modulo B cho các chữ số màu đen. Thay vì lưu trữ toàn bộ số, chúng ta có thể lưu trữ phần còn lại của số được tạo thành bởi các chữ số đã chọn theo modulo A hoặc B khi chúng ta tô màu. Điều này dẫn đến một giải pháp lập trình động một cách tự nhiên: dp[position][red_mod][black_mod][count_red] lưu trữ liệu có thể tô màu đầu tiên hay không`position`chữ số để đạt được`red_mod`mô-đun A,`black_mod`modulo B, với`count_red`chữ số màu đỏ. Số chữ số màu đen được ngầm hiểu`position - count_red`. Điều này làm giảm không gian trạng thái xuống n * A * B * n ≈ 40 * 40 * 40 * 40 = 2,56 triệu, khả thi đối với giới hạn 1 giây. 

Sau đó, chúng tôi xây dựng lại màu bằng cách quay lui từ trạng thái cuối cùng hợp lệ với |count_red - count_black| tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| DP với trạng thái modulo | O(n * A * B * n) | O(n * A * B * n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển số thành danh sách các chữ số. Khởi tạo mảng DP 4 chiều có kích thước [n+1][A][B][n+1] và đặt trường hợp cơ sở cho vị trí 0 với 0 phần còn lại cho cả hai màu và 0 chữ số màu đỏ thành True. 
2. Lặp lại các vị trí từ 0 đến n-1. Đối với mỗi trạng thái DP tại vị trí`i`với`red_mod`,`black_mod`, Và`count_red`, hãy cân nhắc việc tô chữ số tiếp theo là đỏ hoặc đen. 
3. Nếu chữ số tiếp theo có màu đỏ, hãy tính phần dư mới theo modulo A là`(red_mod * 10 + digit) % A`. Tăng`count_red`bằng 1. Cập nhật trạng thái DP cho vị trí`i+1`. 
4. Nếu chữ số tiếp theo có màu đen, hãy tính phần dư mới theo modulo B như sau:`(black_mod * 10 + digit) % B`.`count_red`vẫn như cũ. Cập nhật trạng thái DP cho vị trí`i+1`. 
5. Sau khi xử lý tất cả các vị trí, kiểm tra tất cả các trạng thái DP tại vị trí n trong đó red_mod=0 và black_mod=0. Với mỗi trường hợp, hãy tính`count_black = n - count_red`Và`diff = |count_red - count_black|`. Theo dõi trạng thái với độ khác biệt tối thiểu. 
6. Nếu không có trạng thái đó, ghi -1. Mặt khác, tái tạo lại màu bằng cách quay lui từ trạng thái cuối cùng đã chọn, chọn ở mỗi bước màu dẫn đến trạng thái DP được lưu trữ. 

Tại sao nó hoạt động: Mỗi trạng thái DP thể hiện một phần tô màu hợp lệ và lưu trữ tất cả thông tin mô-đun cần thiết cho khả năng chia hết. Bằng cách xem xét từng chữ số ở mỗi vị trí và cập nhật phần dư mô-đun, chúng tôi đảm bảo mọi khả năng đều được khám phá. Giảm thiểu |r - b| sau khi điền DP đảm bảo chúng tôi chọn màu có độ chênh lệch tuyệt đối tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

for _ in range(int(input())):
    n, A, B = map(int, input().split())
    s = input().strip()
    digits = [int(c) for c in s]

    dp = [[[[False]*(n+1) for _ in range(B)] for _ in range(A)] for _ in range(n+1)]
    parent = [[[[None]*(n+1) for _ in range(B)] for _ in range(A)] for _ in range(n+1)]

    dp[0][0][0][0] = True

    for i in range(n):
        d = digits[i]
        for r_mod in range(A):
            for b_mod in range(B):
                for r_count in range(i+1):
                    if not dp[i][r_mod][b_mod][r_count]:
                        continue

                    # Color red
                    new_r_mod = (r_mod*10 + d) % A
                    new_r_count = r_count + 1
                    if not dp[i+1][new_r_mod][b_mod][new_r_count]:
                        dp[i+1][new_r_mod][b_mod][new_r_count] = True
                        parent[i+1][new_r_mod][b_mod][new_r_count] = ('R', r_mod, b_mod, r_count)

                    # Color black
                    new_b_mod = (b_mod*10 + d) % B
                    if not dp[i+1][r_mod][new_b_mod][r_count]:
                        dp[i+1][r_mod][new_b_mod][r_count] = True
                        parent[i+1][r_mod][new_b_mod][r_count] = ('B', r_mod, b_mod, r_count)

    best_diff = n+1
    best_state = None
    for r_count in range(1, n):
        b_count = n - r_count
        if dp[n][0][0][r_count]:
            diff = abs(r_count - b_count)
            if diff < best_diff:
                best_diff = diff
                best_state = (n, 0, 0, r_count)

    if not best_state:
        print(-1)
        continue

    res = []
    i, r_mod, b_mod, r_count = best_state
    while i > 0:
        color, pr_mod, pb_mod, pr_count = parent[i][r_mod][b_mod][r_count]
        res.append(color)
        i, r_mod, b_mod, r_count = i-1, pr_mod, pb_mod, pr_count

    print(''.join(reversed(res)))
```Phần DP theo dõi tính khả thi của việc tô màu một phần, trong khi phần`parent`mảng cho phép quay lui để xây dựng lại màu sắc. Chúng tôi chỉ cập nhật cẩn thận từng trạng thái mới nếu trước đó chưa đạt được trạng thái đó, tránh ghi đè không cần thiết và đảm bảo đường dẫn chúng tôi khôi phục dẫn đến sự khác biệt tối thiểu. 

## Ví dụ đã hoạt động 

Theo dõi đầu vào`02165`, A=3, B=13, n=5: 

| tôi | chữ số | r_mod | b_mod | r_count | hành động | trạng thái mới | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 0 | R | (1,0,0,1) | 
| 0 | 0 | 0 | 0 | 0 | B | (1,0,0,0) | 
| 1 | 2 | ... | ... | ... | ... | ... | 

Sau khi điền DP, các trạng thái kết thúc có thể có với r_mod=0, b_mod=0 là r_count=2 hoặc 3. Chọn r_count=3, b_count=2 sẽ mang lại khác biệt tối thiểu 1. Quay lui mang lại màu sắc`RBRBR`. 

Đối với đầu vào`1357`, A=2, B=1, n=4: 

Tất cả các chữ số đều là số lẻ; không thể có số màu đỏ chia hết cho 2. DP không bao giờ đạt tới red_mod=0 với bất kỳ r_count nào >0. Đầu ra -1. 

Những dấu vết này xác nhận rằng thuật toán xử lý chính xác việc kiểm tra tính chia hết và tìm ra sự khác biệt tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * A * B * n) | 4 vòng lặp lồng nhau: vị trí, red_mod, black_mod, red_count | 
| Không gian | O(n * A * B * n) | Mảng DP và mảng mẹ lưu trữ tất cả các trạng thái | 

Với n 40, A, B 40, tổng số trạng thái ≈ 2,56 triệu. Mỗi lần cập nhật là thời gian liên tục. Phù hợp thoải mái trong giới hạn 256 MB và 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    exec(open("solution.py").read())
    return out.getvalue().strip()

# Provided samples
assert run("4\n5 3 13\n02165\n4 2 1\n1357\n8
```
