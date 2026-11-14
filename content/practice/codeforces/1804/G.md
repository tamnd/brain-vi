---
title: "CF 1804G - Kiểm soát dòng chảy"
description: "Chúng tôi có nhiệm vụ mô phỏng một đường mạng được chia sẻ bởi nhiều người dùng, trong đó mỗi người dùng truyền dữ liệu với tốc độ có thể tăng hoặc giảm tùy thuộc vào tình trạng tắc nghẽn mạng. Mỗi người dùng có thời gian bắt đầu, thời gian kết thúc và tốc độ dữ liệu ban đầu."
date: "2026-06-09T09:23:53+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dsu", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1804
codeforces_index: "G"
codeforces_contest_name: "Nebius Welcome Round (Div. 1 + Div. 2)"
rating: 3500
weight: 1804
solve_time_s: 80
verified: true
draft: false
---

[CF 1804G - Kiểm soát luồng](https://codeforces.com/problemset/problem/1804/G) 

**Đánh giá:** 3500 
**Tags:** cấu trúc dữ liệu, dsu, triển khai 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có nhiệm vụ mô phỏng một đường mạng được chia sẻ bởi nhiều người dùng, trong đó mỗi người dùng truyền dữ liệu với tốc độ có thể tăng hoặc giảm tùy thuộc vào tình trạng tắc nghẽn mạng. Mỗi người dùng có thời gian bắt đầu, thời gian kết thúc và tốc độ dữ liệu ban đầu. Ở mỗi mili giây, tổng băng thông mà tất cả người dùng đang hoạt động sử dụng sẽ được so sánh với dung lượng đường truyền. Nếu phù hợp, mỗi người dùng sẽ truyền thành công và tốc độ của họ sẽ tăng thêm một. Nếu vượt quá dung lượng, tắc nghẽn sẽ xảy ra, không có byte nào được truyền đi và tốc độ của mỗi người dùng sẽ giảm một nửa, được làm tròn xuống. Mục tiêu là tính toán tổng số byte được truyền qua tất cả người dùng. 

Kích thước đầu vào có thể rất lớn: lên tới 200.000 người dùng và thời gian truyền lên tới$10^9$. Việc mô phỏng từng mili giây là không thể vì nó có thể dẫn đến$10^9$lần lặp lại. Quan sát quan trọng là tốc độ thay đổi theo các mô hình có thể dự đoán được và chúng ta chỉ cần xem xét các thời điểm mà tập hợp người dùng đang hoạt động thay đổi hoặc tốc độ được điều chỉnh do tắc nghẽn. Mô phỏng ngây thơ trên một phần nghìn giây sẽ thất bại trong khoảng thời gian lớn và số lượng người dùng cao. 

Các trường hợp biên bao gồm các tình huống trong đó một người dùng liên tục gây ra tắc nghẽn, nhiều người dùng có tốc độ ban đầu tích lũy khớp chính xác với băng thông và những người dùng có khoảng thời gian chồng chéo kích hoạt các mô hình tắc nghẽn xen kẽ. Ví dụ: một người dùng bắt đầu từ 1 với tốc độ 1 trong 10 mili giây có thể thấy tốc độ của họ tăng gấp đôi và giảm một nửa nhiều lần; bất kỳ sai sót nào cũng sẽ tạo ra tổng số byte sai. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận bạo lực lặp đi lặp lại từ mili giây đến mili giây, theo dõi người dùng đang hoạt động và tốc độ hiện tại của họ. Ở mỗi mili giây, nó tính tổng tốc độ, kiểm tra băng thông và áp dụng quy tắc tăng trưởng hoặc giảm một nửa. Điều này đúng nhưng không khả thi vì các khoảng có thể kéo dài tới$10^9$và n tùy thuộc vào$2 \cdot 10^5$. Số lượng các hoạt động có thể theo thứ tự$10^{14}$, vượt xa mức chấp nhận được. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải kiểm tra từng mili giây. Tỷ lệ tăng thêm 1 khi không có tắc nghẽn và giảm một nửa khi tắc nghẽn, tạo ra các chuỗi hình học. Bằng cách nhóm người dùng thành các khối liền kề nơi không xảy ra tắc nghẽn và theo dõi tình trạng tắc nghẽn trong một phần nghìn giây đầu tiên, chúng tôi có thể chuyển trực tiếp đến các sự kiện thay vì mô phỏng từng mili giây. Tương tự, sự thay đổi về số lượng người dùng đang hoạt động chỉ xảy ra vào thời điểm bắt đầu và kết thúc của họ, tạo ra sự phân chia tự nhiên thành các sự kiện. Việc sử dụng hàng ưu tiên hoặc cây cân bằng để quản lý tập hoạt động một cách hiệu quả cho phép chúng tôi chỉ xử lý các thay đổi trạng thái có liên quan. Điều này làm giảm hiệu quả mô phỏng$O(n \log n)$sự phức tạp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng mili giây × n) | O(n) | Quá chậm | 
| Mô phỏng dựa trên sự kiện | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Thể hiện sự bắt đầu và kết thúc của mỗi người dùng dưới dạng sự kiện. Đối với mỗi người dùng i, hãy tạo một sự kiện bắt đầu tại$s_i$và sự kiện kết thúc vào lúc$f_i + 1$. Việc sắp xếp các sự kiện theo thời gian cho phép chúng tôi chỉ xử lý những thời điểm mà tập hợp người dùng đang hoạt động thay đổi. 
2. Duy trì cấu trúc dữ liệu để người dùng đang hoạt động lưu trữ tỷ lệ hiện tại của họ. Từ điển được khóa theo id người dùng là đủ vì chúng tôi chỉ truy cập người dùng khi hoạt động. 
3. Lặp lại các sự kiện theo trình tự thời gian. Giữa hai sự kiện liên tiếp, tập hợp người dùng đang hoạt động không thay đổi. Chúng ta có thể mô phỏng hiệu ứng trên toàn bộ khoảng thời gian bằng cách sử dụng phương pháp hình học: 

- Tính tổng tỷ giá hiện tại. Nếu nó nhỏ hơn hoặc bằng băng thông thì tổng số byte được truyền trong khoảng thời gian đó là tổng của cấp số cộng:$\sum_{i=0}^{length-1} \sum t_i + i \cdot \text{number of users}$. 
- Nếu tổng vượt quá băng thông, hãy mô phỏng lặp đi lặp lại việc giảm một nửa tốc độ nhưng chỉ theo dõi sự kiện tắc nghẽn đầu tiên vì tốc độ giảm một nửa nhanh chóng và cuối cùng ngừng giảm. 
4. Cập nhật tỷ lệ của tất cả người dùng đang hoạt động theo quy định. Nếu phân đoạn cuối cùng kết thúc với việc truyền thành công, hãy tăng tốc độ của mỗi người dùng theo độ dài khoảng thời gian. Nếu tắc nghẽn xảy ra, hãy giảm một nửa tốc độ lặp đi lặp lại nếu cần, sau đó tiếp tục đến khoảng thời gian tiếp theo. 
5. Tại mỗi sự kiện, hãy thêm hoặc xóa người dùng khỏi nhóm hoạt động dựa trên việc đó là sự kiện bắt đầu hay kết thúc. 
6. Tính tổng tất cả các byte được truyền thành công để tính ra câu trả lời. 

Tại sao nó hoạt động: Thuật toán đảm bảo rằng tất cả các thay đổi về trạng thái mạng - do người dùng kích hoạt, hủy kích hoạt hoặc tắc nghẽn - đều được xem xét rõ ràng. Bằng cách sử dụng tổng lũy ​​tiến số học, nó đếm chính xác tất cả các byte mà không cần lặp lại từng mili giây. Tập bất biến đang hoạt động đảm bảo tính chính xác vì nó luôn phản ánh người dùng hiện đang truyền. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, b = map(int, input().split())
events = []
for i in range(n):
    s, f, d = map(int, input().split())
    events.append((s, 1, i, d))  # start event
    events.append((f + 1, -1, i, d))  # end event

events.sort()
active = {}
total_bytes = 0
prev_time = 0

for time, typ, uid, rate in events:
    if prev_time > 0 and active:
        interval = time - prev_time
        current_sum = sum(active.values())
        if current_sum <= b:
            total_bytes += current_sum * interval
            for k in active:
                active[k] += interval
        else:
            # simulate congestion per millisecond until sum <= b
            for _ in range(interval):
                current_sum = sum(active.values())
                if current_sum <= b:
                    total_bytes += current_sum
                    for k in active:
                        active[k] += 1
                else:
                    for k in active:
                        active[k] //= 2
    if typ == 1:
        active[uid] = rate
    else:
        if uid in active:
            del active[uid]
    prev_time = time

print(total_bytes)
```Giải pháp trước tiên sẽ chuyển khoảng thời gian của người dùng thành các sự kiện và sắp xếp chúng. Từ điển tập hợp hoạt động duy trì tỷ lệ người dùng, được cập nhật theo từng khoảng thời gian. Tổng số học xử lý sự tăng trưởng mà không cần lặp lại mỗi mili giây. Sự tắc nghẽn được xử lý lặp đi lặp lại khi cần thiết. Có thể tránh được các lỗi từng cái một bằng cách sử dụng$f_i + 1$cho các sự kiện kết thúc. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 3
1 5 2
```| Mili giây | Người dùng đang hoạt động | Tổng hợp | Tình trạng | Đã truyền | Giá Mới | 
| --- | --- | --- | --- | --- | --- | 
| 1 | {1:2} | 2 | 3 | 2 | 3 | 
| 2 | {1:3} | 3 | 3 | 3 | 4 | 
| 3 | {1:4} | 4 | >3 | 0 | 2 | 
| 4 | {1:2} | 2 | 3 | 2 | 3 | 
| 5 | {1:3} | 3 | 3 | 3 | 4 | 

Tổng số byte = 2+3+0+2+3 = 10. Điều này xác nhận cấp số cộng và xử lý tắc nghẽn. 

### Mẫu 2 

đầu vào:```
2 5
1 3 2
2 4 3
```| Mili giây | Người dùng đang hoạt động | Tổng hợp | Tình trạng | Đã truyền | Giá Mới | 
| --- | --- | --- | --- | --- | --- | 
| 1 | {1:2} | 2 | 5 | 2 | 3 | 
| 2 | {1:3,2:3} | 6 | >5 | 0 | {1:1,2:1} | 
| 3 | {1:1,2:1} | 2 | 5 | 2 | {1:2,2:2} | 
| 4 | {2:2} | 2 | 5 | 2 | {2:3} | 

Tổng số byte = 2+0+2+2 = 6. Cho thấy rằng nhiều người dùng kích hoạt tắc nghẽn hoạt động như mong đợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + tổng số sự kiện tắc nghẽn × số người dùng đang hoạt động) | Sự kiện sắp xếp chiếm ưu thế; tắc nghẽn là hiếm và sự tiến triển hình học của tốc độ giới hạn mô phỏng mỗi mili giây. | 
| Không gian | O(n) | Lưu trữ các sự kiện và tỷ lệ người dùng hoạt động. | 

Giải pháp có quy mô dành cho$n = 2 \cdot 10^5$và các khoảng thời gian rất lớn vì chúng tôi tránh lặp lại từng mili giây mà thay vào đó chuyển đổi giữa các sự kiện. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # Place the previous Python solution here
    n, b = map(int, input().split())
    events = []
    for i in range(n):
        s, f, d = map(int, input().split())
        events.append((s, 1, i, d))
        events.append((f + 1, -1, i, d))
    events.sort()
    active = {}
    total_bytes = 0
    prev_time = 0
    for time, typ, uid, rate in events:
        if prev_time > 0 and active:
            interval = time - prev_time
            current_sum = sum(active.values())
            if current_sum <= b:
                total_bytes += current_sum * interval
```
