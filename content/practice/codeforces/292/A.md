---
title: "CF 292A - SMSC"
description: "Chúng tôi được giao một chuỗi nhiệm vụ đến trung tâm SMS của công ty. Mỗi tác vụ bao gồm thời gian ti khi nó đến và một số tin nhắn văn bản cần gửi. Trung tâm SMS có thể gửi tối đa một tin nhắn mỗi giây và các tin nhắn được xếp hàng theo thứ tự chúng đến."
date: "2026-06-05T17:17:06+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 292
codeforces_index: "A"
codeforces_contest_name: "Croc Champ 2013 - Round 1"
rating: 1100
weight: 292
solve_time_s: 95
verified: true
draft: false
---

[CF 292A - SMSC](https://codeforces.com/problemset/problem/292/A) 

**Đánh giá:** 1100 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao một chuỗi nhiệm vụ đến trung tâm SMS của công ty. Mỗi nhiệm vụ bao gồm một thời gian`t_i`khi nó đến và một số`c_i`số tin nhắn văn bản cần gửi. Trung tâm SMS có thể gửi tối đa một tin nhắn mỗi giây và các tin nhắn được xếp hàng theo thứ tự chúng đến. Tại bất kỳ giây nào, trước tiên trung tâm sẽ gửi một tin nhắn nếu hàng đợi không trống, sau đó thêm bất kỳ tin nhắn mới nào đến vào giây đó vào cuối hàng đợi. 

Mục tiêu là tính toán hai đại lượng: thời điểm tin nhắn cuối cùng được gửi và số lượng tin nhắn tối đa trong hàng đợi tại bất kỳ thời điểm nào. 

Các hạn chế là nhỏ:`n`nhiều nhất là 1000 lần`t_i`và đếm`c_i`lên tới 10^6. Từ`n`nhỏ, chúng ta có thể lặp lại các nhiệm vụ một cách tuyến tính mà không cần lo lắng về thời gian, nhưng chúng ta phải xử lý cẩn thận hành vi của hàng đợi vì mô phỏng mỗi giây đơn giản lên đến`10^6`giây sẽ là quá chậm. 

Các trường hợp đặc biệt không rõ ràng bao gồm các tác vụ đến cách xa nhau về thời gian, tạo ra khoảng thời gian nhàn rỗi trong đó hàng đợi có thể trống hoàn toàn và các tác vụ đến với nhiều thư cùng lúc, điều này có thể tăng kích thước hàng đợi tối đa. Ví dụ: nếu một tác vụ đến giây thứ 1 với 5 tin nhắn và tác vụ tiếp theo đến giây thứ 10 với 1 tin nhắn thì hàng đợi sẽ trống vào giây thứ 6, sau đó ở giây thứ 10 sẽ có một tin nhắn mới đến, vì vậy tin nhắn cuối cùng sẽ được gửi vào giây thứ 10. Việc triển khai bất cẩn có thể quên tính đến những khoảng trống nhàn rỗi này. 

## Phương pháp tiếp cận 

Một cách tiếp cận brute-force là mô phỏng hàng đợi từng giây một kể từ lần đầu tiên đến cho đến khi tất cả tin nhắn được gửi đi. Tại mỗi giây, hãy gửi tin nhắn nếu hàng đợi không trống, sau đó thêm tin nhắn đến vào giây đó. Mặc dù đúng nhưng điều này sẽ yêu cầu lặp lại tối đa 10^6 giây trong trường hợp xấu nhất, điều này là không cần thiết và không hiệu quả. 

Quan sát quan trọng là trung tâm SMS luôn xử lý tối đa một tin nhắn mỗi giây và chúng tôi biết thời gian đến của tất cả các tác vụ. Do đó, chúng ta không cần phải lặp lại rõ ràng từng giây. Thay vào đó, chúng ta có thể theo dõi kích thước hàng đợi và thời gian hiện tại, tăng thời gian một cách thông minh cho tác vụ tiếp theo nếu hàng đợi trống hoặc gửi tin nhắn liên tục cho đến khi tác vụ tiếp theo đến. Điều này cho phép quét tuyến tính các tác vụ trong khi cập nhật hàng đợi và kích thước hàng đợi tối đa mà không cần mô phỏng từng giây nhàn rỗi. Thời gian tin nhắn cuối cùng cuối cùng chỉ đơn giản là thời gian sau khi tất cả tin nhắn đã được gửi đi, xét đến hàng đợi còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng mỗi giây) | O(max(t_i + sum(c_i))) | O(1) | Quá chậm so với khoảng cách lớn giữa các nhiệm vụ | 
| Tối ưu (mô phỏng dựa trên nhiệm vụ) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`queue = 0`để biểu thị số lượng tin nhắn đang chờ được gửi,`max_queue = 0`để theo dõi kích thước hàng đợi tối đa và`current_time = 0`để đại diện cho giây cuối cùng chúng tôi xử lý. 
2. Lặp lại các nhiệm vụ theo thứ tự thời gian. Đối với mỗi nhiệm vụ`(t, c)`, báo trước thời gian tới nơi`t`. Tính số giây đã trôi qua kể từ lần xử lý cuối cùng:`delta = t - current_time`. Trong khoảng thời gian này, trung tâm SMS sẽ gửi`sent = min(queue, delta)`tin nhắn. Giảm bớt`queue`qua`sent`và tăng dần`current_time`qua`delta`. 
3. Thêm tin nhắn mới`c`ĐẾN`queue`sau khi gửi tin nhắn trong khoảng thời gian đã trôi qua. Cập nhật`max_queue`BẰNG`max(max_queue, queue)`để giải quyết các tin nhắn mới. Bộ`current_time = t`bởi vì chúng tôi đã xử lý đến khi nhiệm vụ đến. 
4. Sau khi tất cả các tác vụ được xử lý, có thể vẫn còn tin nhắn trong hàng đợi. Tin nhắn cuối cùng sẽ được gửi sau`queue`giây. Tăng`current_time`qua`queue`để biết thời điểm cuối cùng của tin nhắn cuối cùng được gửi. 
5. Trở về`current_time`là thời điểm của tin nhắn cuối cùng và`max_queue`như kích thước hàng đợi tối đa được quan sát. 

Bất biến được duy trì trong suốt thuật toán là`queue`luôn thể hiện số lượng tin nhắn đang chờ gửi ngay sau khi xử lý thời điểm hiện tại. Điều này đảm bảo việc tính toán kích thước hàng đợi tối đa và thời gian gửi tin nhắn cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
tasks = [tuple(map(int, input().split())) for _ in range(n)]

queue = 0
max_queue = 0
current_time = 0

for t, c in tasks:
    # Time passed since last task
    delta = t - current_time
    # Send as many messages as possible in that interval
    sent = min(queue, delta)
    queue -= sent
    current_time += delta
    # Add new messages
    queue += c
    max_queue = max(max_queue, queue)

# Process remaining messages
current_time += queue

print(current_time, max_queue)
```Giải pháp này trước tiên đọc tất cả các tác vụ, sau đó mô phỏng hàng đợi một cách hiệu quả bằng cách chỉ xử lý các tác vụ đến và các tin nhắn còn sót lại. các`delta`tính toán đảm bảo rằng các giây nhàn rỗi được xem xét ngầm mà không cần lặp qua chúng. Đang cập nhật`max_queue`sau khi thêm tin nhắn, hãy theo dõi chính xác kích thước hàng đợi lớn nhất. 

## Ví dụ đã hoạt động 

Mẫu 1:```
Input:
2
1 1
2 1
```| Nhiệm vụ | hiện tại_thời gian | đồng bằng | xếp hàng trước | đã gửi | xếp hàng sau | max_queue | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 1 | 0 | 1 | 0 | 0 | 1 | 1 | 
| 2 1 | 1 | 1 | 1 | 1 | 1 | 1 | 
| Kết thúc | 2 | - | 1 | - | 0 | 1 | 

Đầu ra cuối cùng:`3 1`Trường hợp đã xây dựng:```
Input:
3
1 2
4 3
6 1
```| Nhiệm vụ | hiện tại_thời gian | đồng bằng | xếp hàng trước | đã gửi | xếp hàng sau | max_queue | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 2 | 0 | 1 | 0 | 0 | 2 | 2 | 
| 4 3 | 1 | 3 | 2 | 2 | 3 | 3 | 
| 6 1 | 4 | 2 | 3 | 2 | 2 | 3 | 
| Kết thúc | 6 | - | 2 | - | 0 | 3 | 

Đầu ra cuối cùng:`8 3`Dấu vết này cho thấy cách thuật toán xử lý các khoảng trống không hoạt động và việc gửi tin nhắn chồng chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi lặp lại từng nhiệm vụ một lần, chỉ thực hiện các hoạt động có thời gian không đổi cho mỗi nhiệm vụ | 
| Không gian | O(n) | Chúng tôi lưu trữ danh sách nhiệm vụ | 

Với`n ≤ 1000`, thuật toán này chạy tốt trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    queue = 0
    max_queue = 0
    current_time = 0
    n = int(input())
    tasks = [tuple(map(int, input().split())) for _ in range(n)]
    for t, c in tasks:
        delta = t - current_time
        sent = min(queue, delta)
        queue -= sent
        current_time += delta
        queue += c
        max_queue = max(max_queue, queue)
    current_time += queue
    return f"{current_time} {max_queue}"

# provided samples
assert run("2\n1 1\n2 1\n") == "3 1", "sample 1"

# minimum input
assert run("1\n1 1\n") == "2 1", "single task"

# multiple messages arriving before queue empties
assert run("3\n1 2\n2 2\n3 1\n") == "6 3", "messages overlap"

# large gap between tasks
assert run("2\n1 1\n10 2\n") == "12 2", "idle period"

# all tasks have 1 message, consecutive seconds
assert run("5\n1 1\n2 1\n3 1\n4 1\n5 1\n") == "6 1", "consecutive single messages"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 2 1 | Trường hợp cạnh nhiệm vụ đơn | 
| 3\n1 2\n2 2\n3 1 | 6 3 | Xếp hàng chồng chéo giữa các lượt đến | 
| 2\n1 1\n10 2 | 12 2 | Xử lý khoảng trống lớn | 
| 5\n1 1\n2 1\n3 1\n4 1\n5 1 | 6 1 | Đến liên tiếp, không tăng hàng đợi | 

## Vỏ cạnh 

Nếu một tác vụ đến sau khi các tin nhắn trước đó đã được gửi đi một thời gian dài,`delta`đảm bảo rằng hàng đợi trống hoàn toàn trước khi tác vụ mới được thêm vào. Đối với đầu vào`2\n1 1\n10 2\n`, hàng đợi là`1`sau nhiệm vụ đầu tiên. `delta = 10 - 0 = 10
