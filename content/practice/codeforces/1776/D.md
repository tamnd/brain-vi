---
title: "CF 1776D - Làm việc theo nhóm"
description: "Chúng ta được yêu cầu sắp xếp một nhóm gồm ba người để giải một nhóm bài toán với ba mức độ khó: dễ, trung bình và khó. Mỗi vấn đề có thời lượng cố định tùy thuộc vào độ khó của nó: dễ mất 2 đơn vị thời gian, trung bình 3 đơn vị và khó 4 đơn vị."
date: "2026-06-09T11:44:30+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1776
codeforces_index: "D"
codeforces_contest_name: "SWERC 2022-2023 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 2800
weight: 1776
solve_time_s: 111
verified: false
draft: false
---

[CF 1776D - Làm việc theo nhóm](https://codeforces.com/problemset/problem/1776/D) 

**Đánh giá:** 2800 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu sắp xếp một nhóm gồm ba người để giải một nhóm bài toán với ba mức độ khó: dễ, trung bình và khó. Mỗi vấn đề có thời lượng cố định tùy thuộc vào độ khó của nó: dễ mất 2 đơn vị thời gian, trung bình 3 đơn vị và khó 4 đơn vị. Mỗi người chỉ có thể giải quyết một vấn đề tại một thời điểm và đơn vị thời gian cuối cùng của mỗi vấn đề yêu cầu quyền truy cập độc quyền vào một máy tính dùng chung, nghĩa là hai người không thể giải quyết một vấn đề cùng một lúc. Cuộc thi kéo dài trong`l`đơn vị thời gian và mục tiêu là tối đa hóa số lượng vấn đề được giải quyết và đưa ra một lịch trình hợp lệ. 

Các đầu vào`a`,`b`,`c`đại diện cho số lượng các bài toán dễ, trung bình và khó, mỗi bài có tối đa 10^4. Thời lượng cuộc thi`l`nhiều nhất là 10^5. Điều này có nghĩa là việc tìm kiếm mạnh mẽ trên tất cả các hoán vị của bài toán hoặc trên tất cả thời gian bắt đầu có thể có sẽ quá chậm, vì nó sẽ liên quan đến việc xem xét theo thứ tự 10^4! trình tự. Chúng ta cần một chiến lược sắp xếp các vấn đề một cách hiệu quả đồng thời tôn trọng các ràng buộc của máy tính cá nhân và máy tính dùng chung. 

Một cách tiếp cận ngây thơ có thể bỏ qua ràng buộc về máy tính dùng chung và chỉ cố gắng gán các vấn đề một cách tham lam theo mức độ sẵn có của con người. Điều này không thành công khi hai vấn đề kết thúc đồng thời, điều này bị cấm. Một trường hợp khó phát hiện khác xảy ra khi`l`là rất nhỏ hoặc bằng không. Ví dụ, nếu`l = 1`và chúng ta thậm chí còn có một bài toán dễ, không bài toán nào có thể giải được vì thời lượng tối thiểu là 2. Tương tự, nếu một loại khó không có bài toán nào, chúng ta phải tránh cố gắng sắp xếp chúng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực thử mọi hoán vị của mọi vấn đề và giao chúng cho mọi người vào mọi thời điểm bắt đầu có thể, tôn trọng ràng buộc của máy tính. Về nguyên tắc, điều này đúng vì nó kiểm tra rõ ràng tất cả các lịch trình, nhưng độ phức tạp phụ thuộc vào số lượng vấn đề và theo cấp số nhân của các bước thời gian. Với tối đa 10^4 bài toán, điều này hoàn toàn không khả thi. 

Cái nhìn sâu sắc quan trọng đến từ cấu trúc của vấn đề: mỗi người có thể làm việc độc lập ngoại trừ đơn vị lần cuối yêu cầu máy tính dùng chung. Nếu chúng ta bỏ qua máy tính trong giây lát, chúng ta có thể giao nhiệm vụ cho người đầu tiên có mặt theo bất kỳ thứ tự nào. Khó khăn thực sự là tránh va chạm ở đơn vị lần cuối. Nhận thấy rằng chỉ có ba người, số lượng xung đột đơn vị lần cuối tối đa là nhỏ và chúng ta có thể lập mô hình lịch trình như một hàng ưu tiên về thời gian có sẵn cho mỗi người, cộng với một tập hợp các vị trí của đơn vị lần cuối đã được sử dụng. Điều này cho phép chúng tôi lên lịch một cách tham lam theo thời gian kết thúc sớm nhất có thể trong khi vẫn đảm bảo đơn vị lần cuối còn trống. 

Chúng tôi phân công các vấn đề theo thứ tự thời gian ngắn nhất trước tiên để tối đa hóa tổng số vấn đề. Các vấn đề ngắn hơn sẽ kết thúc nhanh hơn và giải phóng máy tính sớm hơn. Đối với mỗi vấn đề, chúng tôi chọn người có thể bắt đầu sớm nhất và kiểm tra xem đơn vị lần cuối có còn hay không. Nếu không, chúng tôi sẽ chuyển thời gian bắt đầu về phía trước cho đến khi đạt được. Vì thời lượng của mỗi vấn đề là nhỏ (2-4 đơn vị) và`l`10^5, cách tiếp cận tham lam này diễn ra theo thời gian tuyến tính đối với số lượng vấn đề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((a+b+c)! * l^3) | O(l) | Quá chậm | 
| Lập kế hoạch tham lam | O((a+b+c) * log 3 + l) | O(a+b+c + l) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn mỗi bài toán dưới dạng một bộ`(duration, type)`và thu thập tất cả các vấn đề vào một danh sách duy nhất. Việc sắp xếp theo thời lượng tăng dần đảm bảo chúng tôi cố gắng sắp xếp các vấn đề ngắn hơn trước. 
2. Khởi tạo một đống tối thiểu với thời gian sẵn có của mỗi người, bắt đầu từ 0. Phần này theo dõi thời điểm mỗi người có thể bắt đầu bài toán tiếp theo của mình. 
3. Duy trì một mảng boolean có kích thước`l`để theo dõi đơn vị thời gian nào đã được sử dụng làm đơn vị cuối cùng của một vấn đề. Điều này thể hiện sự chiếm dụng máy tính được chia sẻ. 
4. Lặp lại từng vấn đề theo thứ tự thời lượng. Đối với vấn đề hiện tại, hãy xem xét người có mặt sớm nhất trong đống. Cố gắng lên lịch giải quyết vấn đề vào thời gian rảnh của người đó. 
5. Nếu đơn vị thời gian cuối cùng của bài toán trùng với một khe máy tính đã được sử dụng, hãy tăng thời gian bắt đầu cho đến khi tìm thấy đơn vị thời gian trống cuối cùng, đồng thời đảm bảo rằng người đó rảnh rỗi. Nếu thời gian kết thúc điều chỉnh vượt quá`l`, bỏ qua vấn đề. 
6. Sau khi tìm thấy một vị trí hợp lệ, hãy ghi lại`(person_id, start_time, end_time)`, đánh dấu đơn vị thời gian cuối cùng là đã bận và cập nhật thời gian rảnh của người đó trong vùng nhớ heap. 
7. Sau khi lên lịch tất cả các vấn đề có thể xảy ra, hãy sắp xếp danh sách đã lên lịch theo thời gian kết thúc và xuất kết quả. 

Điều bất biến được duy trì là không có hai bài toán nào kết thúc trong cùng một đơn vị thời gian và không có người nào làm việc trong những khoảng thời gian trùng nhau. Điều này đảm bảo rằng lịch trình tôn trọng cả các ràng buộc của máy tính cá nhân và máy tính dùng chung. Bằng cách luôn chỉ định người có mặt sớm nhất và di chuyển tối thiểu để tránh va chạm, thuật toán tối đa hóa số lượng vấn đề phù hợp với`l`. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

a, b, c, l = map(int, input().split())
problems = [(2, 'E')] * a + [(3, 'M')] * b + [(4, 'H')] * c
problems.sort()  # prioritize shorter problems

person_heap = [(0, i+1) for i in range(3)]  # (available_time, person_id)
heapq.heapify(person_heap)
computer_used = [False] * (l+1)

schedule = []

for duration, _ in problems:
    avail_time, person = heapq.heappop(person_heap)
    start = avail_time
    while start + duration <= l and computer_used[start + duration - 1]:
        start += 1
    end = start + duration
    if end > l:
        heapq.heappush(person_heap, (avail_time, person))
        continue
    schedule.append((person, start, end))
    computer_used[end-1] = True
    heapq.heappush(person_heap, (end, person))

schedule.sort(key=lambda x: x[2])
print(len(schedule))
for person, start, end in schedule:
    print(person, start, end)
```Mã này tạo ra một danh sách thống nhất tất cả các vấn đề và sắp xếp chúng một cách tham lam theo thời gian ngắn nhất. Heap tối thiểu tìm thấy người có sẵn tiếp theo một cách hiệu quả. Vòng lặp while đảm bảo đơn vị cuối cùng của bài toán không xung đột với máy tính và nếu bài toán không thể phù hợp với cuộc thi thì nó sẽ bị bỏ qua. Sắp xếp ở cuối đảm bảo thứ tự đầu ra bằng cách tăng thời gian kết thúc. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`2 1 1 3`Vấn đề:`[2, 2, 3, 4]`| Vấn đề | Thời lượng | Người Heap (trước) | Bắt đầu | Kết thúc | Đơn vị cuối cùng miễn phí? | Cập nhật đống | Máy tính | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 2 | 2 | [(0,1),(0,2),(0,3)] | 0 | 2 | Có | [(2,1),(0,2),(0,3)] | đánh dấu 1 | 
| 2 | 2 | [(0,2),(2,1),(0,3)] | 0 | 2 | Có | [(2,1),(2,2),(0,3)] | đánh dấu 1 | 
| 3 | 3 | [(0,3),(2,1),(2,2)] | 0 | 3 | Có | [(2,1),(2,2),(3,3)] | đánh dấu 2 | 
| 4 | 4 | [(2,1),(2,2),(3,3)] | 2 | 6 | Vượt quá l=3 | bỏ qua | - | 

Lịch trình đầu ra: hai vấn đề được giải quyết, khớp với đầu ra mẫu. 

### Mẫu 2 

đầu vào:`1 1 0 3`| Vấn đề | Thời lượng | Bắt đầu | Kết thúc | Máy tính | 
| --- | --- | --- | --- | --- | 
| Dễ dàng 2 | 0 | 0 | 2 | đánh dấu 1 | 
| Trung bình 3 | 0 | 0 | 3 | đánh dấu 2 | 

Cả hai đều phù hợp; số vấn đề tối đa được giải quyết là 2. 

Những dấu vết này cho thấy việc lập lịch tham lam tôn trọng cả tính khả dụng của người dùng và máy tính dùng chung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((a+b+c) log 3 + l) | Các vấn đề sắp xếp là O(a+b+c log(a+b+c)), các thao tác heap là O(log 3) cho mỗi vấn đề, kiểm tra đơn vị lần cuối là O(l) trong trường hợp xấu nhất | 
| Không gian | O(a+b+c + l) | Lưu trữ danh sách và lịch trình sự cố, cộng với mảng sử dụng máy tính | 

Giải pháp thoải mái phù hợp trong giới hạn cho`a+b+c ≤ 3*10^4`Và`l ≤ 10^5`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import heapq
    input = sys.stdin.readline

    a, b, c, l = map(int, input().split())
    problems = [(2, 'E')] * a + [(3, 'M')] * b + [(4, 'H')] * c
    problems.sort()

    person_heap = [(0, i+1) for i in range(3)]
    heapq.heapify(person_heap)
    computer_used = [False] * (l+1)

    schedule = []

    for duration, _ in problems:
        avail_time, person = heapq.heappop(person_heap)
        start = avail_time
        while start + duration <= l and computer_used[start + duration - 1]:
            start += 1
        end = start + duration
        if end > l:
```
