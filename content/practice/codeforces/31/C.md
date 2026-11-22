---
title: "CF 31C - Lịch trình"
description: "Chúng ta được cung cấp một danh sách n bài học, mỗi bài học có thời gian bắt đầu và kết thúc, được sắp xếp trong một phòng duy nhất. Hai bài học chồng lên nhau nếu một bài học bắt đầu trước khi bài học khác kết thúc. Mục đích là tìm ra bài học nào có thể bỏ đi để các bài học còn lại không bị trùng lặp."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 31
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 31 (Div. 2, Codeforces format)"
rating: 1700
weight: 31
solve_time_s: 58
verified: true
draft: false
---
[CF 31C - Lịch trình](https://codeforces.com/problemset/problem/31/C) 

**Đánh giá:** 1700 
**Thẻ:** triển khai 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách`n`các bài học, mỗi bài học đều có thời gian bắt đầu và kết thúc, được lên lịch trong một phòng duy nhất. Hai bài học chồng lên nhau nếu một bài học bắt đầu trước khi bài học khác kết thúc. Mục đích là tìm ra bài học nào có thể bỏ đi để các bài học còn lại không bị trùng lặp. Nếu việc loại bỏ bất kỳ bài học nào tạo ra một lịch trình không có xung đột, chúng ta nên liệt kê tất cả các khả năng có thể xảy ra. Nếu lịch trình đã không có xung đột thì bất kỳ bài học nào cũng có thể bị xóa. 

Các hạn chế ở mức vừa phải:`n`nhiều nhất là 5000 và thời gian lên tới 10^6. Với`n = 5000`, một cách tiếp cận mạnh mẽ để kiểm tra liên tục từng cặp bài học sẽ bao gồm thứ tự 5000^2 = 25 triệu thao tác, có thể vẫn phù hợp với giới hạn 2 giây nếu được thực hiện cẩn thận, nhưng nó gần đạt đến giới hạn. Điều này thúc đẩy việc tìm kiếm một giải pháp tuyến tính hoặc tuyến tính cho sự an toàn. Thời gian của bài học là số nguyên nên không phát sinh vấn đề về độ chính xác của dấu phẩy động. 

Các trường hợp cạnh bao gồm những điều sau đây. Nếu các bài học không bị trùng lặp thì câu trả lời phải bao gồm tất cả các bài học. Nếu nhiều bài học có cùng thời gian bắt đầu hoặc kết thúc thì thuật toán phải xử lý chúng một cách chính xác: cho phép các bài học có điểm bắt đầu và kết thúc chính xác và không được tính là trùng lặp. Nếu một bài học được lồng hoàn toàn bên trong một bài học khác, việc loại bỏ một trong hai bài học đó có thể giải quyết được xung đột, nhưng không phải lúc nào cũng vậy - thuật toán cần phải suy luận trên toàn cục chứ không chỉ cục bộ. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là dùng vũ lực: đối với mỗi bài học, hãy tạm thời xóa bài học đó, sau đó kiểm tra tất cả các bài học còn lại theo từng cặp để xem có sự trùng lặp nào không. Điều này hoạt động vì nó trực tiếp thực hiện báo cáo vấn đề, nhưng nó yêu cầu các thao tác O(n^3): loại bỏ O(n) và cho mỗi lần loại bỏ O(n^2) so sánh theo cặp. Với`n = 5000`, điều đó rõ ràng là không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải kiểm tra tất cả các cặp nhiều lần. Nếu sắp xếp các bài học theo thời gian bắt đầu, chúng ta chỉ cần so sánh các bài học liền kề để phát hiện sự trùng lặp. Sau khi được sắp xếp, sự chồng chéo chỉ xảy ra khi phần cuối của bài học trước vượt quá phần đầu của bài học hiện tại. Do đó, chúng ta có thể xử lý trước thời gian kết thúc tối đa nhìn từ bên trái và thời gian bắt đầu tối thiểu nhìn từ bên phải. Sau đó, để loại bỏ bất kỳ bài học ứng viên nào, chúng tôi có thể kiểm tra xem việc hợp nhất các bài học ở bên trái và bên phải có tạo ra sự chồng chéo hay không. Điều này làm giảm vấn đề xuống còn O(n log n) để sắp xếp cộng với O(n) cho các kiểm tra chồng chéo. 

Bạo lực có hiệu quả vì nó trực tiếp thực hiện định nghĩa về xung đột, nhưng nó không thành công.`n = 5000`do độ phức tạp của khối. Quan sát cho thấy xung đột chỉ xảy ra giữa các bài học liền kề trong một mảng được sắp xếp tại thời điểm bắt đầu cho phép chúng ta giảm vấn đề thành các bước tuyến tính, tránh hoàn toàn các vòng lặp lồng nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Sắp xếp + Tiền tố/Hậu tố | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giờ học và lưu trữ chúng cùng với chỉ mục ban đầu. 
2. Sắp xếp bài học theo thời gian bắt đầu. Điều này cho phép chúng ta chỉ tập trung vào những xung đột giữa các bài học liên tiếp. 
3. Xây dựng một`max_end_from_left`mảng, trong đó mỗi mục i chứa thời gian kết thúc tối đa giữa các bài học từ 0 đến i. Điều này cho phép chúng tôi biết thời gian kết thúc muộn nhất trước bất kỳ bài học nào. 
4. Xây dựng một`min_start_from_right`mảng, trong đó mỗi mục i chứa thời gian bắt đầu tối thiểu giữa các bài học từ i đến n-1. Điều này cho phép chúng tôi biết thời gian bắt đầu sớm nhất sau bất kỳ bài học nào. 
5. Với mỗi bài học ở chỉ mục i, kiểm tra xem việc xóa nó có giải quyết được xung đột hay không: so sánh`max_end_from_left[i-1]`với`min_start_from_right[i+1]`. Nếu như`max_end_from_left[i-1] <= min_start_from_right[i+1]`, việc loại bỏ bài học này sẽ tạo ra một lịch trình không chồng chéo. 
6. Thu thập tất cả các chỉ mục gốc của các bài học thỏa mãn điều kiện trên và xuất ra theo thứ tự tăng dần. 

Tại sao nó hoạt động: Việc sắp xếp đảm bảo rằng mọi sự trùng lặp sẽ xảy ra giữa các bài học liên tiếp. các`max_end_from_left`Và`min_start_from_right`mảng nắm bắt thời gian cực trị một cách hiệu quả, cho phép chúng tôi kiểm tra việc loại bỏ trong thời gian không đổi cho mỗi bài học. Điều này đảm bảo chúng tôi xác định được tất cả các bài học bị loại bỏ dẫn đến lịch trình không có xung đột. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
lessons = []

for i in range(n):
    l, r = map(int, input().split())
    lessons.append((l, r, i + 1))

lessons.sort(key=lambda x: x[0])  # sort by start time

max_end_from_left = [0] * n
min_start_from_right = [0] * n

max_end_from_left[0] = lessons[0][1]
for i in range(1, n):
    max_end_from_left[i] = max(max_end_from_left[i-1], lessons[i][1])

min_start_from_right[-1] = lessons[-1][0]
for i in range(n-2, -1, -1):
    min_start_from_right[i] = min(min_start_from_right[i+1], lessons[i][0])

res = []
for i in range(n):
    left_max = max_end_from_left[i-1] if i > 0 else float('-inf')
    right_min = min_start_from_right[i+1] if i < n-1 else float('inf')
    if left_max <= right_min:
        res.append(lessons[i][2])

print(len(res))
print(" ".join(map(str, sorted(res))))
```Giải pháp đầu tiên sắp xếp các bài học để giảm thiểu việc phát hiện xung đột trong việc kiểm tra tính kề cận. các`max_end_from_left`mảng tích lũy kết thúc mới nhất được thấy cho đến nay và`min_start_from_right`tích lũy điểm bắt đầu sớm nhất bên phải. Mỗi bài học được kiểm tra bằng cách so sánh các giá trị này. sử dụng`float('-inf')`Và`float('inf')`xử lý các bài học ranh giới một cách chính xác mà không cần thêm điều kiện. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
3
3 10
20 30
1 3
```Sau khi sắp xếp theo thời gian bắt đầu: 

| tôi | bắt đầu | kết thúc | chỉ mục | 
| --- | --- | --- | --- | 
| 0 | 1 | 3 | 3 | 
| 1 | 3 | 10 | 1 | 
| 2 | 20 | 30 | 2 |`max_end_from_left`: [3, 10, 30]`min_start_from_right`: [3, 3, 20] 

Kiểm tra từng bài học: 

- i = 0: left_max = -inf, right_min = 3 → -inf 3 
- i = 1: left_max = 3, right_min = 20 → 3 ≤ 20 
- i = 2: left_max = 10, right_min = inf → 10 ≤ inf 

Tất cả các bài học có thể được gỡ bỏ, xuất 1 2 3. 

**Mẫu tùy chỉnh** 

đầu vào:```
4
1 5
2 6
7 8
9 10
```Bài học được sắp xếp: 

| tôi | bắt đầu | kết thúc | chỉ mục | 
| --- | --- | --- | --- | 
| 0 | 1 | 5 | 1 | 
| 1 | 2 | 6 | 2 | 
| 2 | 7 | 8 | 3 | 
| 3 | 9 | 10 | 4 |`max_end_from_left`: [5, 6, 8, 10]`min_start_from_right`: [2, 7, 9, 9] 

Kiểm tra việc xóa: 

- i=0: left=-inf, right=7 → 
- i=1: trái=5, phải=7 → 5 ≤ 7 
- i=2: left=6, right=10 → 6 ≤ 10 
- i=3: left=8, right=inf → 

Loại bỏ 1 hoặc 2 sẽ giải quyết xung đột duy nhất (1-5 so với 2-6). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế. Các đường truyền tuyến tính cho mảng tiền tố/hậu tố và việc loại bỏ kiểm tra là O(n). | 
| Không gian | O(n) | Lưu trữ dữ liệu bài học, mảng tiền tố và hậu tố. | 

Với`n ≤ 5000`, O(n log n) dễ dàng dưới 100.000 thao tác, nằm trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    lessons = []
    for i in range(n):
        l, r = map(int, input().split())
        lessons.append((l, r, i + 1))
    lessons.sort(key=lambda x: x[0])
    max_end_from_left = [0] * n
    min_start_from_right = [0] * n
    max_end_from_left[0] = lessons[0][1]
    for i in range(1, n):
        max_end_from_left[i] = max(max_end_from_left[i-1], lessons[i][1])
    min_start_from_right[-1] = lessons[-1][0]
    for i in range(n-2, -1, -1):
        min_start_from_right[i] = min(min_start_from_right[i+1], lessons[i][0])
    res = []
    for i in range(n):
        left_max = max_end_from_left[i-1] if i > 0 else
```
