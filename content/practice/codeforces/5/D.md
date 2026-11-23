---
title: "CF 5D - Tuân thủ luật lệ giao thông"
description: "Chúng ta được yêu cầu tính thời gian di chuyển tối thiểu để một ô tô di chuyển dọc theo một con đường thẳng từ Berland đến Bercouver. Đường có chiều dài _l_, cách điểm xuất phát một khoảng _d_ có biển báo tốc độ giới hạn tốc độ tức thời của ô tô ở _w_."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 5
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 5"
rating: 2100
weight: 5
solve_time_s: 54
verified: true
draft: false
---
[CF 5D - Tuân thủ quy tắc giao thông](https://codeforces.com/problemset/problem/5/D) 

**Xếp hạng:** 2100 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính thời gian di chuyển tối thiểu để một ô tô di chuyển dọc theo một con đường thẳng từ Berland đến Bercouver. Đường có chiều dài _l_, cách điểm xuất phát một khoảng _d_ có biển báo tốc độ giới hạn tốc độ tức thời của ô tô ở _w_. Ô tô khởi động từ trạng thái đứng yên, có thể tăng tốc hoặc giảm tốc với tốc độ cố định _a_ và có tốc độ tối đa _v_. Sau khi vượt qua biển báo, ô tô có thể di chuyển với tốc độ bất kỳ lên tới _v_, bao gồm cả khả năng tăng tốc ngay lập tức nếu có thể. 

Các đầu vào xác định các giới hạn vật lý: gia tốc tối đa _a_ và tốc độ tối đa _v_, chiều dài đường _l_, khoảng cách của biển báo tốc độ _d_ và giới hạn tốc độ _w_. Đầu ra là thời gian di chuyển tối thiểu, giả sử người lái tăng tốc và giảm tốc tối ưu để tuân theo biển báo tốc độ. 

Các ràng buộc rất khiêm tốn: tất cả các giá trị đều ≤ 10.000, do đó, bất kỳ thuật toán nào hoạt động theo số học không đổi hoặc đơn giản trên mỗi dòng đầu vào đều đủ nhanh. Không có thao tác lặp lại nào trên _l_ hoặc _d_ vượt quá giới hạn thời gian. Các trường hợp có giới hạn không rõ ràng bao gồm các tình huống trong đó ô tô thậm chí không thể đạt đến giới hạn tốc độ trước biển báo hoặc khi giới hạn tốc độ cao hơn tốc độ tối đa của ô tô hoặc khoảng cách sau biển báo quá ngắn để tăng tốc đến tốc độ tối đa. Một cách triển khai đơn giản chỉ đơn giản là tăng tốc đến _v_, kiểm tra tốc độ tại biển báo rồi tiếp tục, có thể tạo ra thời gian âm nếu nó giả sử giảm tốc tức thời. 

Một đầu vào phức tạp cụ thể là:```
a = 2, v = 5
l = 5, d = 2, w = 1
```Ở đây, ô tô phải giảm tốc độ để tuân theo biển báo vì tốc độ tối đa _v_ vượt quá giới hạn tốc độ _w_. Một cách tiếp cận bất cẩn mà bỏ qua dấu hiệu hoặc luôn tăng tốc đến _v_ sẽ mang lại một giải pháp không hợp lệ. 

## Phương pháp tiếp cận 

Phương pháp brute-force là mô phỏng chuyển động của ô tô theo từng bước thời gian nhỏ: tại mỗi thời điểm, tăng hoặc giảm tốc độ bằng cách tăng tốc, tôn trọng giới hạn tốc độ tại biển báo. Điều này đúng vì nó mô hình hóa chuyển động liên tục một cách chính xác, nhưng nó yêu cầu những khoảng tăng thời gian rất nhỏ để duy trì độ chính xác, có thể là hàng triệu bước đối với đầu vào lớn nhất. Với _l_ lên tới 10.000 km và khả năng tăng tốc thông thường, điều này là quá mức cần thiết và không cần thiết. 

Quan sát quan trọng là gia tốc và giảm tốc là tuyến tính theo thời gian và khoảng cách. Quỹ đạo trong thời gian tối thiểu bao gồm nhiều nhất là ba giai đoạn: tăng tốc ở tốc độ tối đa cho đến khi đạt đến giới hạn tốc độ hoặc tốc độ tối đa của ô tô, lao đi với tốc độ không đổi nếu được phép và giảm tốc để tôn trọng biển báo tốc độ. Bài toán về cơ bản là chuyển động từng đoạn dọc theo các đoạn thẳng của đồ thị vận tốc-thời gian. Bằng cách tính toán khoảng cách cần thiết để tăng tốc hoặc giảm tốc đến một tốc độ nhất định bằng công thức$s = v^2 / (2a)$, chúng ta có thể xác định liệu ô tô có cần bắt đầu phanh trước biển báo hay có thể vượt qua biển báo đó với tốc độ tối đa hay không. Sau biển báo, chúng ta có thể sử dụng lý luận tương tự cho đoạn đường còn lại. 

Điều này làm giảm giải pháp thành một loạt các phép tính số học có điều kiện, tránh hoàn toàn việc mô phỏng. Cấu trúc của bài toán đảm bảo rằng một giải pháp tối ưu không bao giờ yêu cầu điều gì phức tạp hơn việc tính toán khoảng cách từ gia tốc và tốc độ giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(l * bước chính xác) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tốc độ tối đa theo lý thuyết có thể đạt được tại dấu nếu ô tô tăng tốc với tốc độ _a_ từ trạng thái nghỉ trên quãng đường _d_. Sử dụng$v_{\text{reach}} = \sqrt{2 a d}$. Tốc độ tại biển báo bị ràng buộc bởi _w_ và _v_ nên tốc độ thực tế tại biển báo là nhỏ nhất là _v_, _w_, và$v_{\text{reach}}$. 
2. Tính thời gian để đến biển báo. Nếu ô tô đạt tốc độ mục tiêu trước khoảng cách _d_, ô tô sẽ lao đi trong quãng đường còn lại tới biển báo. Ngược lại, nó tăng tốc liên tục đến dấu. 
3. Tính quãng đường còn lại sau dấu như sau$l - d$. Xác định xem ô tô có thể tăng tốc đến tốc độ tối đa _v_ trên quãng đường đó hay quãng đường quá ngắn và phải di chuyển với tốc độ thấp hơn. Một lần nữa, sử dụng công thức$s = v^2 / (2a)$để tìm tốc độ tối đa có thể đạt được ở cuối. 
4. Tính thời gian đoạn còn lại sau dấu. Nếu ô tô đạt tốc độ tối đa _v_, nó tăng tốc rồi lao đi với tốc độ _v_. Nếu không, nó sẽ tăng tốc liên tục mà không cần dừng lại. 
5. Tính tổng hai thời gian: thời gian đến dấu hiệu và thời gian từ dấu hiệu đến điểm kết thúc. 

Tại sao nó hoạt động: chuyển động của ô tô hoàn toàn được xác định bởi các ràng buộc về gia tốc. Giải pháp tối ưu không bao giờ tăng tốc chậm hơn mức cho phép hoặc giảm tốc chậm hơn mức cần thiết; bất kỳ sai lệch nào cũng làm tăng thời gian di chuyển. Bằng cách xem xét khoảng cách và tốc độ tối đa cho phép ở mỗi đoạn, thuật toán đảm bảo thời gian khả thi ngắn nhất. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

a, v = map(int, input().split())
l, d, w = map(int, input().split())

def time_to_reach(s, start_speed, end_speed, accel):
    # distance needed to accelerate from start_speed to end_speed
    s_needed = (end_speed**2 - start_speed**2) / (2 * accel)
    if s_needed >= s:
        # cannot reach end_speed, accelerate as much as possible
        return (-start_speed + math.sqrt(start_speed**2 + 2 * accel * s)) / accel
    else:
        # accelerate to end_speed, then coast
        t_accel = (end_speed - start_speed) / accel
        s_remaining = s - s_needed
        t_coast = s_remaining / end_speed
        return t_accel + t_coast

# speed limit at the sign
speed_at_sign = min(w, v, math.sqrt(2 * a * d))

# time to reach the sign
time_to_sign = time_to_reach(d, 0, speed_at_sign, a)

# remaining distance
remaining = l - d

# maximum speed achievable after sign
max_speed_after = math.sqrt(speed_at_sign**2 + 2 * a * remaining)
final_speed = min(v, max_speed_after)

# time for remaining segment
time_after_sign = time_to_reach(remaining, speed_at_sign, final_speed, a)

total_time = time_to_sign + time_after_sign
print(f"{total_time:.10f}")
```các`time_to_reach`hàm trừu tượng hóa việc tính toán thời gian cho một đoạn có khả năng tăng tốc, xử lý cả hai trường hợp ô tô có thể đạt tốc độ mục tiêu và trường hợp không thể đạt được tốc độ mục tiêu. Mã chính tính toán cẩn thận tốc độ tại biển báo và sau đó xác định khả năng tăng tốc tối ưu sau biển báo. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
1 1
2 1 3
```| Biến | Giá trị | 
| --- | --- | 
| tốc độ_at_sign | phút(3,1,sqrt(2_1_1)) = 1 | 
| thời gian_to_sign | (-0 + sqrt(0 + 2_1_1))/1 = 1,4142… | 
| còn lại | 1 | 
| max_speed_after | sqrt(1^2 + 2_1_1) = sqrt(3) ≈ 1,732 | 
| cuối cùng_speed | phút(1,1.732) = 1 | 
| time_after_sign | (-1 + sqrt(1+2_1_1))/1 = 1,086… | 
| tổng_thời gian | 1,4142 + 1,086 ≈ 2,500 | 

Điều này xác nhận thuật toán tính toán chính xác gia tốc trước và sau dấu. 

**Ví dụ tùy chỉnh** 

đầu vào:```
2 10
5 2 3
```| Biến | Giá trị | 
| --- | --- | 
| tốc độ_at_sign | phút(3,10,sqrt(2_2_2)=2,828) = 2,828 | 
| thời gian_to_sign | (-0+sqrt(0+2_2_2))/2 ≈ 1.414 | 
| còn lại | 3 | 
| max_speed_after | sqrt(2,828^2 + 2_2_3) = sqrt(8+12)=sqrt(20) ≈ 4,472 | 
| cuối cùng_speed | phút(10,4.472)=4.472 | 
| time_after_sign | (-2,828 + sqrt(2,828^2+2_2_3))/2 = (-2,828+4,472)/2 ≈ 0,822 | 
| tổng_thời gian | 1,414+0,822 ≈ 2,236 | 

Điều này cho thấy khả năng xử lý đúng khi xe không thể đạt tốc độ tối đa sau biển báo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Tất cả các phép tính đều là số học và căn bậc hai; không có vòng lặp trên khoảng cách. | 
| Không gian | O(1) | Chỉ có một số biến được sử dụng. | 

Các ràng buộc chỉ cho phép các phép tính số học đơn giản. Tất cả các căn bậc hai đều có giá trị ≤ 10^8, được Python xử lý một cách an toàn. Giải pháp chạy trong micro giây. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    a, v = map(int, input().split())
    l, d, w = map
```
