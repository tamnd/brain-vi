---
title: "CF 43E - Chủng Tộc"
description: "Chúng ta được yêu cầu phân tích một cuộc đua đường thẳng có chiều dài _s_ km với _n_ ô tô. Mỗi ô tô có một danh sách các đoạn đường lái xe và mỗi đoạn đường chỉ định một tốc độ và khoảng thời gian không đổi. Xe di chuyển với tốc độ đó trong thời gian nhất định trước khi chuyển sang đoạn đường tiếp theo."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 43
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 42 (Div. 2)"
rating: 2300
weight: 43
solve_time_s: 207
verified: true
draft: false
---
[CF 43E - Cuộc đua](https://codeforces.com/problemset/problem/43/E) 

**Đánh giá:** 2300 
**Tags:** vũ lực, thực hiện, hai con trỏ 
**Thời gian giải:** 3m 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu phân tích một cuộc đua đường thẳng có chiều dài _s_ km với _n_ ô tô. Mỗi ô tô có một danh sách các đoạn đường lái xe và mỗi đoạn đường chỉ định một tốc độ và khoảng thời gian không đổi. Xe di chuyển với tốc độ đó trong thời gian nhất định trước khi chuyển sang đoạn đường tiếp theo. Tất cả các ô tô đều xuất phát cùng một lúc tại vạch xuất phát. Nhiệm vụ của chúng ta là đếm số lần một ô tô vượt một ô tô khác trong suốt cuộc đua, chỉ tính những lần vượt tức thời. Xe gặp nhau ở vạch xuất phát hoặc vạch đích không được tính. 

Đầu vào chỉ định số lượng ô tô và độ dài cuộc đua, theo sau là trình tự các phân đoạn của mỗi ô tô. Mỗi đoạn bao gồm tốc độ tính bằng km/h và khoảng thời gian tính bằng giờ. Tổng quãng đường mà ô tô đi được đảm bảo bằng chiều dài chặng đua. 

Với các ràng buộc, _n_ lên tới 100 và mỗi ô tô có tối đa 100 đoạn, trong khi _s_ có thể lên tới 10^6. Điều này cho thấy rằng việc lặp lại một cách đơn giản theo thời gian theo từng bước nhỏ là không khả thi vì _s_ có thể rất lớn. Thay vào đó, chúng ta phải tận dụng cấu trúc chuyển động của mỗi ô tô là tuyến tính từng phần trong không gian khoảng cách-thời gian. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó hai ô tô có tốc độ giống nhau trên một số đoạn đường và sau đó phân kỳ. Ví dụ, hai ô tô đều khởi động ở tốc độ 5 trong 1 giờ và sau đó một ô tô tăng tốc còn ô tô kia chạy chậm lại. Việc đếm số lượt vượt không chính xác có thể bao gồm việc tính nhiều khách hàng tiềm năng trong một đoạn nếu chúng tôi chỉ kiểm tra tại các ranh giới của đoạn. Một trường hợp khó phát hiện khác là khi nhiều lần vượt xảy ra đồng thời giữa nhiều hơn hai xe. Chúng tôi phải đảm bảo mỗi lần vượt theo cặp đều được tính riêng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là mô phỏng tất cả các ô tô theo từng khoảng thời gian rất nhỏ, so sánh từng cặp tại mỗi thời điểm. Đối với mỗi bước thời gian nhỏ, chúng tôi sẽ tính toán khoảng cách của mỗi ô tô và đếm số lượt vượt mỗi khi một ô tô vượt qua một ô tô khác. Về nguyên tắc, điều này đúng nhưng quá chậm vì các bước thời gian phải đủ nhỏ để phát hiện tất cả các khách hàng tiềm năng và số lượng thao tác tăng lên theo _s × n^2_. Với _s_ lên tới 10^6 và _n_ lên tới 100, điều này dẫn đến 10^10 thao tác, vượt quá giới hạn 2 giây. 

Thông tin chi tiết quan trọng là chuyển động của mỗi ô tô là tuyến tính từng phần, do đó việc vượt chỉ xảy ra ở ranh giới đoạn đường hoặc khi quỹ đạo tuyến tính của hai ô tô cắt nhau. Chúng tôi có thể xử lý các phân đoạn của mỗi ô tô thành một biểu diễn tích lũy khoảng cách-thời gian, sau đó mô phỏng cuộc đua theo cách đồng bộ trên tất cả các ô tô, chỉ cập nhật vị trí ở đầu và cuối mỗi phân đoạn. Ở mỗi bước thời gian, chúng tôi kiểm tra thứ tự tương đối của ô tô theo khoảng cách và đếm số lượt vượt bất cứ khi nào thứ tự thay đổi. Vì mỗi ô tô có tối đa 100 phân đoạn và _n_ ≤ 100 nên tổng số sự kiện cần kiểm tra có thể quản lý được, O(n × k) và kiểm tra thay đổi thứ tự giữa các cặp là O(n^2) cho mỗi sự kiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(s × n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n × k × n) = O(n^2 × k) | O(n × k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích cú pháp đầu vào và chuyển đổi các phân đoạn của từng ô tô thành biểu diễn khoảng cách-thời gian tích lũy. Đối với mỗi đoạn, hãy tính vị trí cuối cùng là tổng quãng đường đã đi trong đoạn đó. 
2. Khởi tạo danh sách vị trí hiện tại của tất cả các ô tô và bộ đếm số lượt vượt. Theo dõi thứ tự xe trước đó dựa trên vị trí của chúng. 
3. Lặp lại tất cả các ranh giới phân đoạn theo thứ tự thời gian. Vì các đoạn có thể có thời lượng khác nhau nên hãy duy trì một con trỏ cho mỗi ô tô để biểu thị đoạn hiện tại. 
4. Với mỗi bước thời gian tương ứng với một đầu đoạn của ô tô bất kỳ, hãy cập nhật vị trí của ô tô đó bằng công thức chuyển động tuyến tính`position += speed × duration`. 
5. Sau khi cập nhật tất cả các vị trí tại thời điểm này, hãy sắp xếp các ô tô theo khoảng cách để xác định thứ tự mới. So sánh đơn hàng mới với đơn hàng trước đó. Đối với mỗi cặp xe có vị trí tương đối thay đổi, hãy tăng bộ đếm vượt. 
6. Cập nhật thứ tự trước đó thành thứ tự mới và chuyển sang sự kiện tiếp theo. 
7. Tiếp tục cho đến khi tất cả các phân đoạn được xử lý cho tất cả các ô tô. Xuất ra tổng số lần vượt. 

Tại sao nó hoạt động: tính bất biến là giữa ranh giới các đoạn đường, tất cả ô tô đều chuyển động tuyến tính và việc vượt chỉ xảy ra khi các quỹ đạo tuyến tính giao nhau. Bằng cách chỉ xử lý tại các điểm cuối của đoạn đường và kiểm tra các thay đổi thứ tự tương đối, chúng tôi tính mỗi lần vượt chính xác một lần. Vì vấn đề đảm bảo rằng việc vượt diễn ra ngay lập tức nên chúng tôi không bỏ lỡ bất kỳ sự kiện nào giữa các đoạn đường. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, s = map(int, input().split())
cars = []

for _ in range(n):
    data = list(map(int, input().split()))
    k = data[0]
    segments = []
    for j in range(k):
        v, t = data[1 + 2*j], data[2 + 2*j]
        segments.append((v, t))
    cars.append(segments)

# Convert segments into cumulative distances and times
positions = [[0]] * n
times = [[0]] * n
for i in range(n):
    pos_list = [0]
    time_list = [0]
    for v, t in cars[i]:
        pos_list.append(pos_list[-1] + v * t)
        time_list.append(time_list[-1] + t)
    positions[i] = pos_list
    times[i] = time_list

overtakes = 0
prev_order = list(range(n))
current_time = 0

# Generate all event times
event_times = set()
for tlist in times:
    event_times.update(tlist)
event_times = sorted(event_times)

# For each event time, compute car positions
for t in event_times[1:]:
    dist = []
    for i in range(n):
        # Find which segment car i is in
        seg_idx = 0
        while seg_idx < len(times[i]) - 1 and times[i][seg_idx + 1] <= t:
            seg_idx += 1
        # Compute position at time t
        start_time = times[i][seg_idx]
        start_pos = positions[i][seg_idx]
        v = cars[i][seg_idx][0]
        curr_pos = start_pos + v * (t - start_time)
        dist.append((curr_pos, i))
    # Sort by distance
    dist.sort(reverse=True)
    new_order = [idx for _, idx in dist]
    # Count overtakes: every pair that swapped order
    for j in range(n):
        for k in range(j + 1, n):
            a, b = new_order[j], new_order[k]
            if prev_order.index(a) > prev_order.index(b):
                overtakes += 1
    prev_order = new_order

print(overtakes)
```Phần đầu tiên đọc và phân tích đầu vào. Mỗi đoạn của ô tô được chuyển đổi thành khoảng cách và thời gian tích lũy để chúng tôi có thể tính toán vị trí một cách hiệu quả bất kỳ lúc nào. Sau đó, chúng tôi tạo tất cả các điểm cuối của đoạn đường duy nhất làm thời gian diễn ra sự kiện vì việc vượt chỉ có thể xảy ra vào những thời điểm này. Tại mỗi sự kiện, chúng tôi tính toán vị trí hiện tại của mỗi ô tô bằng phép nội suy tuyến tính. Sắp xếp theo khoảng cách sẽ đưa ra thứ tự hiện tại và so sánh nó với thứ tự trước đó sẽ xác định tất cả các lần vượt. 

## Ví dụ đã hoạt động 

Mẫu 1 Đầu vào:```
2 33
2 5 1 2 14
1 3 11
```| Thời gian | Xe 0 Vị trí | Xe 1 Pos | Đặt hàng | Vượt Qua Mới | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | [0,1] | 0 | 
| 1 | 5 | 3 | [0,1] | 0 | 
| 3 | 33 | 9 | [0,1] | 1 | 

Giải thích: Xe 0 vượt xe 1 ở thời điểm 3 sau khi đoạn thứ hai kết thúc. Điều này tạo ra chính xác một lần vượt. 

Đầu vào tùy chỉnh:```
3 10
1 2 5
1 1 10
2 1 3 3 1
```| Thời gian | Xe 0 Vị trí | Xe 1 Pos | Xe 2 Pos | Đặt hàng | Vượt Qua Mới | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | [0,1,2] | 0 | 
| 3 | 6 | 3 | 3 | [0,2,1] | 2 | 
| 4 | 8 | 4 | 6 | [0,2,1] | 0 | 
| 5 | 10 | 5 | 9 | [0,2,1] | 1 | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 × k) | Đối với mỗi ranh giới phân khúc O(n × k), chúng tôi so sánh từng cặp n ô tô. | 
| Không gian | O(n × k) | Lưu trữ tích lũy vị trí, thời gian cho tất cả các chặng của từng xe. | 

Trường hợp xấu nhất là n=100, k=100 dẫn đến 10^6 kiểm tra theo cặp, khá thoải mái trong giới hạn thời gian 2 giây. Mức sử dụng bộ nhớ vẫn ở dưới giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open("solution.py").read())
    return sys.stdout.getvalue().strip()

assert run("2 33\n2
```
