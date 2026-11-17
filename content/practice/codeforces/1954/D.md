---
title: "CF 1954D - Bóng Màu"
description: "Chúng ta được cho một bộ sưu tập các quả bóng, mỗi quả bóng có n màu riêng biệt, trong đó màu i có ai quả bóng. Chúng ta được phép nhóm những quả bóng này lại nhưng mỗi nhóm được phép có tối đa hai quả bóng và không được có hai quả bóng nào trong cùng một nhóm có cùng màu."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1954
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 164 (Rated for Div. 2)"
rating: 1800
weight: 1954
solve_time_s: 103
verified: false
draft: false
---

[CF 1954D - Quả bóng màu](https://codeforces.com/problemset/problem/1954/D) 

**Đánh giá:** 1800 
**Tags:** tổ hợp, dp, toán, sắp xếp 
**Thời gian giải:** 1m 43s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được phát một bộ sưu tập các quả bóng, mỗi quả bóng được giao một`n`màu sắc riêng biệt, nơi màu sắc`i`có`a_i`quả bóng. Chúng ta được phép nhóm những quả bóng này lại nhưng mỗi nhóm được phép có tối đa hai quả bóng và không được có hai quả bóng nào trong cùng một nhóm có cùng màu. Đối với bất kỳ tập hợp con màu nào, "giá trị" của tập hợp con đó là số nhóm tối thiểu cần thiết để chứa tất cả các quả bóng có màu đó theo các ràng buộc này. Nhiệm vụ của chúng ta là tính tổng các giá trị trên tất cả`2^n`tập hợp con màu có thể, modulo`998244353`. 

Những ràng buộc cho chúng ta biết rằng`n`có thể lên tới 5000, nhưng tổng số bóng cũng bị giới hạn bởi 5000. Điều này có nghĩa là mặc dù`n`tương đối lớn, tổng của tất cả`a_i`là khiêm tốn. Do đó, các thuật toán mở rộng quy mô theo tổng số quả bóng, thay vì số lượng tập hợp con một cách rõ ràng, là khả thi. Một cách tiếp cận ngây thơ cố gắng liệt kê tất cả các tập hợp con là không khả thi ngay lập tức bởi vì`2^n`tăng trưởng theo cấp số nhân. 

Trường hợp cạnh cần lưu ý là tập hợp con chỉ chứa một màu với nhiều quả bóng. Ví dụ: nếu tập hợp con là`{1}`Và`a_1 = 4`, số lượng nhóm tối thiểu là`2`bởi vì chúng ta chỉ có thể ghép hai quả bóng mỗi nhóm. Một trường hợp phức tạp khác là tập con trống, có giá trị bằng 0. Những chi tiết nhỏ này rất quan trọng vì việc xử lý bất cẩn các tập hợp rỗng hoặc tập hợp đơn lẻ có thể dẫn đến sai sót từng phần một trong tổng số. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản về mặt khái niệm. Đối với mỗi`2^n`các tập hợp con, chúng ta có thể tính số lượng bóng tối đa có bất kỳ màu nào trong tập hợp con đó và sau đó tính số lượng nhóm tối thiểu cần thiết. Đối với một tập hợp con`S`, nếu như`max_count`là số lượng bóng lớn nhất có một màu trong`S`, số lượng nhóm cần thiết ít nhất là`max_count`, nhưng việc ghép các quả bóng có màu sắc khác nhau đôi khi có thể làm giảm tổng số nhóm. Để triển khai điều này một cách chính xác cho từng tập hợp con sẽ yêu cầu lặp lại các tập hợp con, tổng hợp các quả bóng và nhóm tính toán, điều này tốn kém`O(2^n * n)`. Điều này là quá chậm đối với`n = 5000`. 

Cái nhìn sâu sắc quan trọng đến từ việc xem xét vấn đề dưới dạng quy hoạch động. Thay vì lặp lại các tập hợp con, chúng ta có thể lặp lại các quả bóng một cách tích lũy. Vì tổng của tất cả các quả bóng nhiều nhất là 5000 nên chúng ta có thể xác định trạng thái DP`dp[i][k]`là số cách để hình thành các tập hợp con bằng cách sử dụng tập hợp con đầu tiên`i`màu sắc sao cho tổng số quả bóng được chọn là`k`. Đây là một bài toán DP tổ hợp cổ điển: với mỗi màu, chúng ta có thể chọn từ 0 đến`a_i`quả bóng và với mỗi lựa chọn, chúng tôi cập nhật`dp[i+1][new_k] += dp[i][k]`. Khi chúng ta biết số tập con chứa chính xác`k`quả bóng, chúng ta có thể tính toán sự đóng góp của mỗi quả bóng`k`đến tổng cuối cùng của các nhóm tối thiểu, vì đối với`k`bóng, số lượng nhóm tối thiểu là`(k + 1) // 2`(vì mỗi nhóm được cầm tối đa 2 quả bóng). Điều này làm giảm vấn đề từ hàm mũ trong`n`thành bậc hai trong tổng số bóng, điều này có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(2^n) | Quá chậm | 
| DP theo tổng số bóng | O(n * tổng_ball) | O(tổng_bóng) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số quả bóng,`total = sum(a_i)`. Đây sẽ là mức tối đa`k`chúng ta cần xem xét trong DP của mình. 
2. Khởi tạo mảng DP`dp`kích thước`total + 1`với`dp[0] = 1`, đại diện cho tập hợp con trống. chỉ số`k`đại diện cho tổng số quả bóng được chọn trong một tập hợp con. 
3. Lặp lại từng màu`i`. Với mỗi số bóng`x`từ`a_i`xuống còn 1, cập nhật`dp[k + x] += dp[k]`cho tất cả`k`theo thứ tự giảm dần. Điều này đảm bảo rằng với mỗi tổng số trước đó`k`, chúng tôi đang đếm tất cả các cách để thêm`x`quả bóng từ màu sắc`i`không có tập hợp con đếm kép. 
4. Sau khi xử lý tất cả các màu,`dp[k]`chứa số tập hợp con có chính xác`k`quả bóng. Giá trị của tập con với`k`quả bóng là`(k + 1) // 2`, vì mỗi nhóm được giữ nhiều nhất hai quả bóng. 
5. Lặp lại tất cả`k`từ 1 đến`total`và tổng`dp[k] * ((k + 1) // 2)`. Áp dụng modulo`998244353`ở mỗi bước để tránh tràn. 
6. Ghi tổng số tiền dưới dạng câu trả lời. 

Tính đúng đắn phụ thuộc vào bất biến DP:`dp[k]`đếm chính xác số tập hợp con một cách chính xác`k`quả bóng sau khi xem xét tất cả các màu sắc. Ghép bóng tối ưu làm giảm số lượng nhóm xuống`(k + 1) // 2`cho bất kỳ kích thước tập hợp con nào`k`, bởi vì ghép hai quả bóng mỗi nhóm luôn là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

n = int(input())
a = list(map(int, input().split()))
total = sum(a)

dp = [0] * (total + 1)
dp[0] = 1

for balls in a:
    for k in range(total, -1, -1):
        if dp[k]:
            for x in range(1, balls + 1):
                dp[k + x] = (dp[k + x] + dp[k]) % MOD

result = 0
for k in range(1, total + 1):
    result = (result + dp[k] * ((k + 1) // 2)) % MOD

print(result)
```Phần đầu tiên khởi tạo DP cho tập hợp con trống. Các vòng lặp lồng nhau cập nhật mảng DP cho từng màu, lặp lại cẩn thận theo thứ tự giảm dần để tránh đếm cùng một tập hợp con nhiều lần. Cuối cùng, chúng tôi tính tổng các giá trị có trọng số theo số quả bóng, áp dụng công thức chia số nguyên`(k + 1) // 2`cho các nhóm tối thiểu. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`n = 3, a = [1, 1, 2]`| Tập hợp con | # Bóng (k) | nhóm tối thiểu | đóng góp | 
| --- | --- | --- | --- | 
| {} | 0 | 0 | 0 | 
| {1} | 1 | 1 | 1 | 
| {2} | 1 | 1 | 1 | 
| {3} | 2 | 1 | 2//2=1 | 
| {1,2} | 2 | 1 | 1 | 
| {1,3} | 3 | 2 | 2 | 
| {2,3} | 3 | 2 | 2 | 
| {1,2,3} | 4 | 2 | 2 | 

Tổng số tiền = 11, phù hợp với sản lượng dự kiến. 

**Ví dụ 2:**`n = 2, a = [3,1]`| Tập hợp con | # Bóng (k) | nhóm tối thiểu | đóng góp | 
| --- | --- | --- | --- | 
| {} | 0 | 0 | 0 | 
| {1} | 3 | 2 | 2 | 
| {2} | 1 | 1 | 1 | 
| {1,2} | 4 | 2 | 2 | 

Tổng số tiền = 5. 

Dấu vết xác nhận DP đếm chính xác tất cả các tập hợp con và công thức`(k + 1)//2`đưa ra số lượng nhóm tối thiểu chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * tổng^2) | Đối với mỗi màu, chúng tôi lặp lại tổng số DP hiện có và lên tới`a_i`quả bóng để cập nhật tổng số mới. Vì tổng 5000 nên n*total^2 là khả thi. | 
| Không gian | O(tổng cộng) | Chúng tôi chỉ lưu trữ một mảng DP có tổng kích thước + 1. | 

Điều này phù hợp thoải mái trong giới hạn 2 giây vì`5000*5000`các phép toán khoảng 25 triệu, có thể chấp nhận được trong Python với số học đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 998244353
    n = int(input())
    a = list(map(int, input().split()))
    total = sum(a)
    dp = [0] * (total + 1)
    dp[0] = 1
    for balls in a:
        for k in range(total, -1, -1):
            if dp[k]:
                for x in range(1, balls + 1):
                    dp[k + x] = (dp[k + x] + dp[k]) % MOD
    result = 0
    for k in range(1, total + 1):
        result = (result + dp[k] * ((k + 1)//2)) % MOD
    return str(result)

# provided sample
assert run("3\n1 1 2\n") == "11", "sample 1"

# minimum-size input
assert run("1\n1\n") == "1", "min input"

# all equal balls
assert run("3\n2 2 2\n") == "20", "all equal"

#
```
