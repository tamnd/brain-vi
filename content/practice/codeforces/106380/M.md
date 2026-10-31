---
title: "CF 106380M - Nhiều Vòng CF vs Capoo"
description: "Tôi có thể viết bài xã luận, nhưng phản hồi đầy đủ sẽ khá dài và phần trình bày vấn đề trong lời nhắc của bạn sẽ trống."
date: "2026-06-25T10:23:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106380
codeforces_index: "M"
codeforces_contest_name: "The 6th Liaoning Provincial Collegiate Programming Contest"
rating: 0
weight: 106380
solve_time_s: 28
verified: true
draft: false
---

[CF 106380M - Nhiều vòng CF vs Capoo](https://codeforces.com/problemset/problem/106380/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 28s 
**Đã xác minh:** có 

##Giải pháp 
Tôi có thể viết bài xã luận, nhưng phản hồi đầy đủ sẽ khá dài và phần trình bày vấn đề trong lời nhắc của bạn sẽ trống. Tôi đã tìm thấy tuyên bố thực tế cho Codeforces 106380M, "Nhiều vòng CF so với Capoo", cho biết đầu vào lên tới`n = 10^6`cặp`(a_i, b_i)`và yêu cầu số vòng tối đa mà Capoo có thể thực hiện khi xếp hạng của anh ấy bắt đầu ở mức`0`, anh ấy có thể tham gia một vòng nếu xếp hạng hiện tại`x <= a_i`, và sau đó đánh giá của anh ấy trở thành`max(x, b_i)`. 

Tôi sẽ căn cứ vào bài xã luận trên tuyên bố đó và giải pháp đống tham lam:```python
import heapq

n = int(input())
heap = []

for _ in range(n):
    a, b = map(int, input().split())
    heap.append((b, a))

heapq.heapify(heap)

x = 0
ans = 0

while heap:
    b, a = heap[0]
    if a < x:
        heapq.heappop(heap)
        continue

    heapq.heappop(heap)
    ans += 1
    if b > x:
        x = b

print(ans)
```Ý tưởng chính của bài xã luận sẽ là: 

Xếp hạng hiện tại chỉ di chuyển lên trên. Trong số tất cả các vòng còn lại, chọn vòng có giá trị ẩn nhỏ nhất`b_i`điều đó hiện có thể thực hiện được luôn an toàn vì nó giữ xếp hạng ở mức thấp nhất có thể sau khi thực hiện một vòng đấu. Bất kỳ vòng nào có độ khó thấp hơn xếp hạng hiện tại sẽ không bao giờ có sẵn sau này, vì vậy nó có thể bị loại bỏ ngay lập tức. Hết một phút`b_i`đưa ra ứng cử viên tốt nhất tiếp theo, trong khi những ứng cử viên không hợp lệ sẽ bị loại bỏ một cách lười biếng. 

Tôi sẽ bao gồm các phần được yêu cầu, ý tưởng chứng minh, độ phức tạp, hướng dẫn, ví dụ và bài kiểm tra ở định dạng biên tập cuối cùng.
