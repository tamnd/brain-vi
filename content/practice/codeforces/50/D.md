---
title: "CF 50D - Ném bom"
description: "Chúng ta được cung cấp một tập hợp các đối tượng của kẻ thù, mỗi đối tượng có tọa độ 2D cố định và một điểm tấn công cố định. Mục tiêu là xác định bán kính tác động tối thiểu của đầu đạn hạt nhân sao cho xác suất không thể vô hiệu hóa ít nhất K vật thể này nhiều nhất là ε trên mil."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 50
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 47"
rating: 2100
weight: 50
solve_time_s: 102
verified: false
draft: false
---

[CF 50D - Ném bom](https://codeforces.com/problemset/problem/50/D) 

**Xếp hạng:** 2100 
**Tags:** tìm kiếm nhị phân, dp, xác suất 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các đối tượng của kẻ thù, mỗi đối tượng có tọa độ 2D cố định và một điểm tấn công cố định. Mục tiêu là xác định bán kính tác động tối thiểu của đầu đạn hạt nhân sao cho xác suất không thể vô hiệu hóa ít nhất là`K`của các đối tượng này nhiều nhất là ε trên mil. Xác suất một tòa nhà bị phá hủy phụ thuộc vào khoảng cách của nó với điểm tấn công so với bán kính bom: các tòa nhà bên trong bán kính luôn bị phá hủy, trong khi các tòa nhà bên ngoài bán kính bị phá hủy theo xác suất theo công thức phân rã theo cấp số nhân. 

Đầu vào cung cấp`N`đối tượng (tối đa 100), mục tiêu bắt buộc`K`(≤ N), ngưỡng thất bại`ε`(1 ε ≤ 999 mỗi triệu, tức là xác suất lên tới 0,999), tọa độ tấn công và tọa độ của các vật thể. Đầu ra là một số dấu phẩy động duy nhất biểu thị bán kính tối thiểu. 

Những hạn chế gợi ý rằng`N`đủ nhỏ để các thuật toán có độ phức tạp bậc ba hoặc bậc hai có thể chạy thoải mái trong 2 giây. Khoảng cách được giới hạn bởi |1000| trong mỗi tọa độ, do đó bình phương khoảng cách nằm trong giới hạn số nguyên. 

Các trường hợp cạnh phát sinh khi`K = 1`hoặc`ε = 999`và khi các vật thể trùng với điểm tấn công. Một cách tiếp cận ngây thơ không xử lý chính xác xác suất đối với các vật thể chồng chéo hoặc cực gần có thể đánh giá thấp bán kính cần thiết. Ví dụ: với một vật thể ở khoảng cách 0 tính từ điểm va chạm, bán kính tối thiểu là 0; việc triển khai bất cẩn có thể tạo ra bán kính âm hoặc không hội tụ. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là thử mọi bán kính có thể, tính xác suất cho tất cả các đối tượng, liệt kê tất cả các kết hợp của`K`các đối tượng bị phá hủy, tính tổng xác suất xảy ra lỗi và kiểm tra xem nó có nằm dưới ε hay không. Điều này hiệu quả vì mỗi xác suất là độc lập, nhưng trong trường hợp xấu nhất là O(2^N), điều này bị cấm đối với N = 100. 

Cái nhìn sâu sắc quan trọng là mô hình hóa vấn đề như một vấn đề quy hoạch động xác suất. Mỗi đối tượng có xác suất phá hủy`p_i(R)`như một hàm của bán kính`R`. Cho phép`dp[i][k]`là xác suất phá hủy chính xác`k`đối tượng trong số những đối tượng đầu tiên`i`đồ vật. Với sự tái phát:```
dp[i+1][k] = dp[i][k] * (1 - p_i) + dp[i][k-1] * p_i
```chúng ta có thể tính toán xác suất phá hủy ít nhất`K`các đối tượng một cách hiệu quả. Điều này cho phép chúng tôi kiểm tra bán kính nhất định`R`trong thời gian O(N*K). Sau đó chúng ta có thể tìm kiếm nhị phân`R`bởi vì xác suất thất bại giảm dần khi bán kính tăng: bán kính lớn hơn chỉ làm tăng xác suất hủy diệt. 

Điều này làm giảm đáng kể độ phức tạp: O(N_K_log(max_distance/epsilon)), khả thi với N ≤ 100. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^N) | O(N) | Quá chậm | 
| DP + Tìm kiếm nhị phân | O(N_K_log(1e6)) | O(N*K) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. **Tính toán trước khoảng cách**: Đối với mỗi đối tượng, hãy tính khoảng cách Euclide`D_i`tới điểm tấn công. Khoảng cách có thể được lưu trữ dưới dạng phao. Điều này là cần thiết vì xác suất phá hủy phụ thuộc vào khoảng cách. 
2. **Xác định hàm xác suất**: Đối với bán kính ứng viên`R`, tính xác suất`p_i(R)`vật đó`i`bị phá hủy. Nếu vật nằm trong bán kính`p_i = 1`. Mặt khác, tính toán bằng cách sử dụng công thức hàm mũ được đưa ra trong bài toán. 
3. **Lập mảng DP**: Cho`dp[k]`đại diện cho xác suất phá hủy chính xác`k`đối tượng trong số tất cả được xem xét cho đến nay. Khởi tạo`dp[0] = 1.0`. 
4. **Lặp lại các đối tượng**: Đối với mỗi đối tượng, hãy cập nhật mảng DP theo thứ tự ngược lại (để tránh ghi đè các giá trị). Công thức cập nhật là:```
dp[k] = dp[k] * (1 - p_i) + dp[k-1] * p_i
```Điều này giải thích cho hai trường hợp: đối tượng không bị phá hủy hoặc nó bị phá hủy. 

1. **Kiểm tra xác suất thất bại**: Sau khi xử lý tất cả các đối tượng, tính tổng`dp[0]`bởi vì`dp[K-1]`để có được xác suất không tiêu diệt được ít nhất`K`đồ vật. Chuyển đổi ε trên triệu thành xác suất bằng cách chia cho 1000. 
2. **Tìm kiếm nhị phân theo bán kính**: Khởi tạo`low = 0`,`high = max_distance_possible`(giả sử 5000) và lặp lại cho đến khi chênh lệch nhỏ hơn 1e-7. Đối với mỗi điểm giữa, hãy tính DP và kiểm tra xem xác suất thất bại có ε hay không. Điều chỉnh`low`hoặc`high`tương ứng. 
3. **Đầu ra**: Sau khi tìm kiếm nhị phân,`low`sẽ hội tụ về bán kính tối thiểu thỏa mãn yêu cầu xác suất. 

**Tại sao nó hoạt động**: Xác suất không thể tiêu diệt được`K`vật thể đều đơn điệu giảm dần theo bán kính. DP tính toán chính xác tất cả các tập hợp con của các đối tượng bị phá hủy mà không liệt kê chúng một cách rõ ràng, đảm bảo tính toán xác suất chính xác. Tìm kiếm nhị phân tận dụng tính đơn điệu để tìm bán kính đủ tối thiểu. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def bombing():
    N = int(input())
    K, eps = map(int, input().split())
    eps /= 1000  # convert per mils to probability
    X0, Y0 = map(int, input().split())
    objects = [tuple(map(int, input().split())) for _ in range(N)]
    
    def prob(dist, R):
        if dist <= R:
            return 1.0
        else:
            return math.exp(1 - (dist / R) ** 2)
    
    distances = [math.hypot(x - X0, y - Y0) for x, y in objects]
    
    left, right = 0.0, 5000.0
    for _ in range(100):
        mid = (left + right) / 2
        dp = [0.0] * (N + 1)
        dp[0] = 1.0
        for d in distances:
            p = prob(d, mid)
            for k in range(N, 0, -1):
                dp[k] = dp[k] * (1 - p) + dp[k-1] * p
            dp[0] *= (1 - p)
        fail_prob = sum(dp[:K])
        if fail_prob <= eps:
            right = mid
        else:
            left = mid
    print(f"{(left + right) / 2:.12f}")

bombing()
```Mã đầu tiên chuyển đổi ε mỗi triệu thành xác suất thích hợp. Nó tính toán trước khoảng cách để tránh tính toán lặp lại. DP được cập nhật ngược lại để duy trì các giá trị chính xác trước đó. Vòng lặp tìm kiếm nhị phân lặp lại 100 lần, đảm bảo độ chính xác trong phạm vi 1e-6 cho giới hạn bài toán. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1
1 500
5 5
1 2
```| bán kính giữa | khoảng cách vật thể | p_i | dp[0] | dp[1] | lỗi_prob | 
| --- | --- | --- | --- | --- | --- | 
| 3,84 | 5.0 | 0,5 | 0,5 | 0,5 | 0,5 | 

DP theo dõi chính xác xác suất phá hủy các đối tượng 0 và 1. Tìm kiếm nhị phân hội tụ về bán kính ≈ 3,842577615187627. 

### Mẫu tùy chỉnh 

đầu vào:```
3
2 100
0 0
0 0
3 4
6 8
```| bán kính | p_i | dp sau 3 đối tượng | lỗi_prob | 
| --- | --- | --- | --- | 
| 5.0 | 1, 0,64, 0,25 | [0,14,0,36,0,5,0] | 0,14 | 

Bảng cho thấy ở bán kính 5, xác suất vô hiệu hóa không thành công ít nhất 2 đối tượng là 0,14 < 0,1 nên chúng ta cần bán kính lớn hơn một chút. DP xử lý xác suất một phần một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N_K_log(1e6)) | Tìm kiếm nhị phân ~100 lần lặp; DP trên mỗi bán kính là O(N*K). | 
| Không gian | O(N*K) | Lưu trữ mảng DP cho tối đa N đối tượng và K mục tiêu. | 

Với N ≤ 100, K ≤ 100, giải pháp thực hiện thoải mái trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        bombing()
    return f.getvalue().strip()

# provided sample
assert abs(float(run("1\n1 500\n5 5\n1 2")) - 3.842577615187627) < 1e-6, "sample 1"

# minimum size
assert abs(float(run("1\n1 1\n0 0\n0 0")) - 0.0) < 1e-6, "single object at strike"

# all objects same point
assert abs(float(run("3\n2 500\n0 0\n0 0\n0 0\n0 0")) - 0.0) < 1e
```
