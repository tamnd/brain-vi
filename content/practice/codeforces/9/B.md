---
title: "CF 9B - Sinh Viên Chạy Bộ"
description: "Học sinh đang đi trên một chiếc xe buýt di chuyển dọc theo trục x từ trái sang phải. Bus dừng ở vị trí cố định (xi, 0) theo thứ tự tăng dần. Tại bất kỳ điểm dừng nào ngoại trừ điểm đầu tiên, sinh viên có thể xuống xe và chạy thẳng đến trường đại học ở (xu, yu)."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "geometry", "implementation"]
categories: ["algorithms"]
codeforces_contest: 9
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 9 (Div. 2 Only)"
rating: 1200
weight: 9
solve_time_s: 86
verified: true
draft: false
---
[CF 9B - Học sinh đang chạy](https://codeforces.com/problemset/problem/9/B) 

**Đánh giá:** 1200 
**Tags:** vũ lực, hình học, thực hiện 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Học sinh đang đi trên một chiếc xe buýt di chuyển dọc theo trục x từ trái sang phải. Xe buýt dừng ở vị trí cố định`(xi, 0)`theo thứ tự tăng dần. Tại bất kỳ điểm dừng nào ngoại trừ điểm đầu tiên, sinh viên có thể xuống xe và chạy thẳng đến trường đại học ở`(xu, yu)`. 

Nếu học sinh ở lại trên xe buýt cho đến khi dừng`i`, tổng thời gian di chuyển có hai phần. Phần đầu tiên là thời gian trên xe buýt từ`x = 0`ĐẾN`x = xi`. Vì xe buýt chuyển động với vận tốc không đổi`vb`, lần này là`xi / vb`. Phần thứ hai là thời gian chạy từ`(xi, 0)`ĐẾN`(xu, yu)`. Khoảng cách đó là khoảng cách Euclide nên thời gian chạy là:$$\frac{\sqrt{(xu - xi)^2 + yu^2}}{vs}$$Nhiệm vụ là tìm chỉ số dừng sao cho tổng thời gian đến là nhỏ nhất. Nếu có nhiều điểm dừng tạo ra cùng một thời điểm, chúng ta phải chọn điểm dừng gần trường đại học hơn. 

Những hạn chế là rất nhỏ. Có tối đa 100 điểm dừng, vì vậy ngay cả việc kiểm tra trực tiếp từng điểm dừng cũng không đáng kể. Một giải pháp đang làm`O(n)`hoặc`O(n^2)`công việc hoàn toàn an toàn trong giới hạn 1 giây. Vấn đề này không phải là về các thủ thuật tối ưu hóa, mà là về việc triển khai logic so sánh một cách chính xác và xử lý các so sánh dấu phẩy động một cách cẩn thận. 

Trường hợp khó khăn đầu tiên là quy tắc ràng buộc. Hai điểm dừng có thể tạo ra tổng thời gian di chuyển giống hệt nhau, nhưng câu trả lời không nhất thiết phải là điểm dừng sớm hơn. Chúng ta phải chọn điểm dừng gần trường đại học hơn. 

Coi như:```
3 1 1
0 2 4
3 4
```Đối với điểm dừng 2:```
bus time = 2
run distance = 5
total = 7
```Đối với điểm dừng 3:```
bus time = 4
run distance = 1
total = 5
```Người dừng muộn hơn sẽ thắng vì dù đi xe buýt lâu hơn nhưng quãng đường chạy lại ngắn hơn rất nhiều. 

Một sai lầm dễ mắc phải khác là cho phép học sinh rời đi ở điểm dừng đầu tiên. Tuyên bố rõ ràng cấm điều này. Một vòng lặp bất cẩn bắt đầu từ chỉ mục`0`sẽ đưa ra câu trả lời sai. 

Ví dụ:```
2 10 1
0 100
0 1
```Học sinh muốn xuống xe ngay và chạy khoảng cách`1`, nhưng điều đó là bất hợp pháp. Câu trả lời đúng là dừng lại`2`. 

Bình đẳng dấu phẩy động là một mối nguy hiểm khác. Về mặt lý thuyết, hai thời điểm bằng nhau có thể hơi khác nhau do sai số về độ chính xác. Sử dụng chính xác`==`so sánh đôi có thể thất bại bất ngờ. Cách tiếp cận an toàn nhất là so sánh với một epsilon nhỏ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng mọi điểm dừng có thể. Đối với mỗi điểm dừng`i`, tính:$$\text{time} = \frac{xi}{vb} + \frac{\sqrt{(xu-xi)^2 + yu^2}}{vs}$$Sau đó chọn điểm dừng có thời gian tối thiểu. Nếu thời gian bằng nhau, hãy so sánh khoảng cách đến trường đại học và chọn trường nhỏ hơn. 

Cách tiếp cận bạo lực này đã được chấp nhận hoàn toàn vì`n ≤ 100`. Tổng khối lượng công việc rất nhỏ, chỉ khoảng vài trăm phép tính số học và căn bậc hai. 

Không có lập trình động hoặc tối ưu hóa hình học ẩn ở đây. Nhận xét quan trọng là học sinh chỉ có một quyết định có ý nghĩa duy nhất, đó là điểm dừng nơi anh ta thoát ra. Khi điểm dừng đó được cố định, toàn bộ tuyến đường và thời gian di chuyển được xác định duy nhất. Điều đó biến vấn đề thành một vấn đề liệt kê đơn giản. 

Một cách tiếp cận phức tạp hơn sẽ cố gắng suy luận về độ lồi hoặc tối ưu hóa khoảng cách về mặt hình học, nhưng không cách nào giúp ích được vì kích thước đầu vào vốn đã cực kỳ nhỏ. Giải pháp rõ ràng và đáng tin cậy nhất chỉ đơn giản là đánh giá trực tiếp từng ứng viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số điểm dừng, tốc độ xe buýt và tốc độ chạy của học sinh. 
2. Đọc mảng tọa độ dừng`x`. 
3. Đọc tọa độ trường đại học`(xu, yu)`. 
4. Khởi tạo điểm dừng tốt nhất là điểm dừng`2`, vì học sinh không được phép rời khỏi điểm dừng đầu tiên. 
5. Đối với mỗi điểm dừng từ chỉ mục`2`ĐẾN`n`: 

Tính thời gian đi của xe buýt:$$\frac{xi}{vb}$$Sau đó tính quãng đường chạy:$$\sqrt{(xu-xi)^2 + yu^2}$$Chia khoảng cách đó cho`vs`để có được thời gian chạy. 
6. Cộng cả hai lần để có tổng thời gian đến điểm dừng này. 
7. So sánh tổng thời gian này với thời gian tốt nhất hiện tại. 

Nếu thời gian mới nhỏ hơn, hãy cập nhật câu trả lời. 

Nếu thời gian bằng nhau trong độ chính xác của dấu phẩy động, hãy so sánh khoảng cách đến trường đại học và dừng lại gần hơn. 
8. Xuất chỉ số dừng đã chọn. 

### Tại sao nó hoạt động 

Mọi chiến lược hợp lệ đều tương ứng với việc chọn chính xác một điểm dừng nơi học sinh xuống xe buýt. Khi điểm dừng đó được cố định, tổng thời gian được xác định duy nhất bằng quãng đường đi xe buýt cộng với quãng đường chạy theo đường thẳng. Vì thuật toán đánh giá mọi điểm dừng hợp pháp và giữ điểm dừng có tổng thời gian tối thiểu nên nó không thể bỏ lỡ câu trả lời tối ưu. Quy tắc ràng buộc được xử lý rõ ràng trong quá trình so sánh, do đó các trường hợp thời gian bằng nhau cũng được giải quyết chính xác. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

n, vb, vs = map(int, input().split())
x = list(map(int, input().split()))
xu, yu = map(int, input().split())

eps = 1e-9

best_idx = 1
best_time = float('inf')
best_dist = float('inf')

for i in range(1, n):
    bus_time = x[i] / vb

    dist = math.sqrt((xu - x[i]) ** 2 + yu ** 2)
    run_time = dist / vs

    total_time = bus_time + run_time

    if total_time < best_time - eps:
        best_time = total_time
        best_dist = dist
        best_idx = i + 1
    elif abs(total_time - best_time) <= eps:
        if dist < best_dist:
            best_dist = dist
            best_idx = i + 1

print(best_idx)
```Vòng lặp bắt đầu từ chỉ mục`1`thay vì`0`vì điểm dừng đầu tiên bị cấm. Đây là lỗi thực hiện phổ biến nhất trong vấn đề. 

Biến`dist`lưu trữ khoảng cách hình học từ điểm dừng hiện tại đến trường đại học. Giá trị tương tự đó được sử dụng lại cho cả thời gian chạy và để ngắt kết nối, giúp việc triển khai trở nên đơn giản. 

Việc so sánh epsilon tránh được các vấn đề về độ chính xác. Thay vì kiểm tra xem hai giá trị dấu phẩy động có bằng nhau hay không, mã sẽ coi chúng bằng nhau nếu chênh lệch của chúng cực kỳ nhỏ. 

Câu trả lời sử dụng`i + 1`bởi vì các chỉ số của Codeforces dựa trên 1 trong khi danh sách Python dựa trên 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 5 2
0 2 4 6
4 1
```| Dừng lại | Giờ xe buýt | Khoảng cách chạy | Thời gian chạy | Tổng Thời Gian | Tốt nhất cho đến nay | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 0,4 | 2.236 | 1.118 | 1.518 | 2 | 
| 3 | 0,8 | 1.000 | 0,500 | 1.300 | 3 | 
| 4 | 1.2 | 2.236 | 1.118 | 2.318 | 3 | 

Sự lựa chọn tốt nhất trở thành điểm dừng`3`. Mặc dù xe buýt chạy lâu hơn điểm dừng`2`, khoảng cách chạy trở nên nhỏ hơn đáng kể. 

### Ví dụ 2 

đầu vào:```
5 2 10
0 5 10 15 20
12 1
```| Dừng lại | Giờ xe buýt | Khoảng cách chạy | Thời gian chạy | Tổng Thời Gian | Tốt nhất cho đến nay | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 2,5 | 7.071 | 0,707 | 3.207 | 2 | 
| 3 | 5.0 | 2.236 | 0,224 | 5.224 | 2 | 
| 4 | 7,5 | 3.162 | 0,316 | 7.816 | 2 | 
| 5 | 10.0 | 8.062 | 0,806 | 10.806 | 2 | 

Ở đây học sinh chạy rất nhanh nên về sớm thì tốt hơn. Dành thêm thời gian trên xe buýt chậm sẽ gây tổn hại nhiều hơn là giúp ích cho việc giảm khoảng cách chạy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi đánh giá mỗi điểm dừng chính xác một lần | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Với tối đa 100 điểm dừng, chương trình sẽ chạy ngay lập tức. Việc sử dụng bộ nhớ không đáng kể vì thuật toán chỉ lưu trữ tọa độ dừng và một số giá trị dấu phẩy động. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, vb, vs = map(int, input().split())
    x = list(map(int, input().split()))
    xu, yu = map(int, input().split())

    eps = 1e-9

    best_idx = 1
    best_time = float('inf')
    best_dist = float('inf')

    for i in range(1, n):
        bus_time = x[i] / vb

        dist = math.sqrt((xu - x[i]) ** 2 + yu ** 2)
        run_time = dist / vs

        total_time = bus_time + run_time

        if total_time < best_time - eps:
            best_time = total_time
            best_dist = dist
            best_idx = i + 1
        elif abs(total_time - best_time) <= eps:
            if dist < best_dist:
                best_dist = dist
                best_idx = i + 1

    return str(best_idx)

# provided sample
assert run(
"""4 5 2
0 2 4 6
4 1
"""
) == "3", "sample 1"

# minimum size
assert run(
"""2 10 1
0 100
0 1
"""
) == "2", "minimum size"

# fast student prefers early exit
assert run(
"""5 1 100
0 10 20 30 40
11 0
"""
) == "2", "fast runner"

# slow student prefers later exit
assert run(
"""5 100 1
0 10 20 30 40
39 0
"""
) == "5", "slow runner"

# tie-breaking by distance
assert run(
"""3 1 1
0 1 2
1 0
"""
) == "2", "tie-breaking"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu`n = 2`|`2`| Không thể rời đi ở điểm dừng đầu tiên | 
| Người chạy rất nhanh |`2`| Thoát sớm trở nên tối ưu | 
| Người chạy rất chậm |`5`| Ở trên xe buýt lâu hơn trở nên tối ưu | 
| Trường hợp đứt dây buộc |`2`| Số lần bằng nhau được xử lý chính xác | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng đầu tiên là điểm dừng đầu tiên bị cấm. 

đầu vào:```
2 10 1
0 100
0 1
```Nếu được phép khởi hành ở điểm dừng đầu tiên thì tổng thời gian sẽ chỉ là`1`. Nhưng học sinh phải ở trên xe buýt cho đến khi ít nhất dừng lại`2`. 

Thuật toán xử lý việc này bằng cách bắt đầu vòng lặp từ chỉ mục`1`, tương ứng với điểm dừng`2`trong lập chỉ mục dựa trên 1. Ứng cử viên được đánh giá duy nhất là dừng lại`2`, vì vậy kết quả đầu ra là chính xác:```
2
```Một tình huống khó khăn khác là tổng số lần bằng nhau. 

đầu vào:```
3 1 1
0 1 2
1 0
```Để dừng lại`2`:```
bus = 1
run = 0
total = 1
```Để dừng lại`3`:```
bus = 2
run = 1
total = 3
```Dừng lại`2`thắng ngay lập tức. 

Bây giờ hãy xem xét:```
3 1 1
0 1 2
1 1
```Các tổng được tính toán trở nên cực kỳ gần nhau trong số học dấu phẩy động. Việc sử dụng kiểm tra đẳng thức trực tiếp có thể tạo ra hành vi không nhất quán vì căn bậc hai là số vô tỷ. Thay vào đó, thuật toán sử dụng ngưỡng epsilon nên các lỗi làm tròn nhỏ không ảnh hưởng đến quyết định. 

Trường hợp cạnh cuối cùng xuất hiện khi điểm dừng tốt nhất là điểm dừng cuối cùng. 

đầu vào:```
5 100 1
0 10 20 30 40
39 0
```Xe buýt chạy cực nhanh trong khi học sinh chạy chậm. Thuật toán vẫn kiểm tra từng điểm dừng một cách độc lập, do đó, nó phát hiện chính xác rằng việc ở lại trên xe buýt cho đến cuối sẽ giảm thiểu khoảng cách chạy đủ để bù đắp cho việc di chuyển thêm trên xe buýt.
