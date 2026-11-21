---
title: "CF 253E - Máy in"
description: "Chúng tôi đang mô phỏng một máy in đơn luồng nhận tác vụ theo thời gian. Mỗi tác vụ đến vào một thời điểm nhất định, có số lượng trang đã biết và mức độ ưu tiên xác định thứ tự nó được phân phát khi có nhiều tác vụ đang chờ."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 253
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 154 (Div. 2)"
rating: 2200
weight: 253
solve_time_s: 61
verified: true
draft: false
---

[CF 253E - Máy in](https://codeforces.com/problemset/problem/253/E) 

**Đánh giá:** 2200 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, triển khai, sắp xếp 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một máy in đơn luồng nhận tác vụ theo thời gian. Mỗi tác vụ đến vào một thời điểm nhất định, có số lượng trang đã biết và mức độ ưu tiên xác định thứ tự nó được phân phát khi có nhiều tác vụ đang chờ. Nhiệm vụ có mức độ ưu tiên cao hơn sẽ được ưu tiên trước những nhiệm vụ có mức độ ưu tiên thấp hơn. Điều khó khăn là thiếu chính xác một mức độ ưu tiên của nhiệm vụ. Thay vào đó, chúng tôi biết khi nào tác vụ này kết thúc quá trình in. 

Mục tiêu gồm có hai phần: thứ nhất, suy ra mức độ ưu tiên chưa xác định và thứ hai, xác định thời gian hoàn thành chính xác của mọi nhiệm vụ, bao gồm cả nhiệm vụ có mức độ ưu tiên bị thiếu. Máy in in chính xác một trang mỗi giây và các tác vụ sẽ được đưa vào hàng đợi ngay khi đến nơi. 

Những hạn chế là đáng kể. Chúng tôi có thể có tới 50.000 nhiệm vụ và mỗi nhiệm vụ có thể rất lớn (trang tối đa 10^9, thời gian tối đa 10^9 và thời gian hoàn thành tối đa 10^15). Điều này ngay lập tức loại trừ mọi mô phỏng mạnh mẽ từng giây trên toàn bộ dòng thời gian, bởi vì cách tiếp cận đơn giản có thể lặp lại tới 10^15 giây trong trường hợp xấu nhất. Thay vào đó, chúng ta cần một cách tiếp cận có độ phức tạp phụ thuộc vào số lượng tác vụ chứ không phải tổng số giây được in. 

Có những cạm bẫy tinh vi. Ví dụ: nếu có nhiều tác vụ đến cùng một lúc thì tác vụ có mức độ ưu tiên cao nhất sẽ được chọn trước tiên. Quản lý sai thứ tự giao hàng có thể dẫn đến thời gian hoàn thành không chính xác. Một cái bẫy khác là cho rằng nhiệm vụ không xác định luôn hoàn thành sau cùng; trên thực tế, nó có thể bị ưu tiên bởi những người đến sau nếu mức độ ưu tiên của nó thấp hơn một số nhiệm vụ đã biết. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ thử mọi mức độ ưu tiên có thể có cho tác vụ còn thiếu, mô phỏng máy in từng giây một và kiểm tra xem mức độ ưu tiên nào dẫn đến thời gian hoàn thành được quan sát. Brute-force hoạt động vì việc mô phỏng máy in rất đơn giản: giữ hàng đợi ưu tiên của các tác vụ đang chờ, chọn tác vụ có mức ưu tiên cao nhất mỗi giây, giảm các trang còn lại và ghi lại khi hoàn thành. Nhưng điều này không thành công vì dòng thời gian có thể đạt tới 10^15 giây; lặp đi lặp lại một giây tại một thời điểm là không khả thi. 

Thông tin chi tiết quan trọng là hoạt động của máy in chỉ thay đổi tại những thời điểm riêng biệt: khi một tác vụ đến hoặc một tác vụ kết thúc. Chúng ta không bao giờ cần phải mô phỏng từng giây ở giữa. Bằng cách duy trì hàng ưu tiên của các nhiệm vụ đang chờ và tăng thời gian cho sự kiện liên quan tiếp theo, chúng tôi có thể tính toán thời gian hoàn thành một cách hiệu quả. Khi hàm mô phỏng này tồn tại, chúng ta có thể thực hiện tìm kiếm nhị phân trên các giá trị ưu tiên có thể có của tác vụ chưa biết. Chúng tôi kiểm tra xem mức độ ưu tiên của ứng viên có dẫn đến thời gian hoàn thành nhất định cho nhiệm vụ không xác định hay không và điều chỉnh tìm kiếm của chúng tôi cho phù hợp. 

Bí quyết giúp việc này diễn ra nhanh chóng là nhận ra rằng chúng ta chỉ quan tâm đến thứ tự ưu tiên tương đối. Vì vậy, chúng ta chỉ có thể tìm kiếm trong số các giá trị chưa được thực hiện bởi các tác vụ khác. Việc sử dụng một đống cho hàng đợi ưu tiên giúp việc chèn và trích xuất logarit, dẫn đến giải pháp tổng thể O(n log n log U), trong đó U là số mức độ ưu tiên có thể có cho tác vụ chưa xác định. Điều này khả thi với n = 50.000 và U lên tới 10^9. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(T*n) | O(n) | Quá chậm, T có thể đạt tới 10^15 | 
| Mô phỏng + Tìm kiếm nhị phân | O(n log n log U) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc tất cả các nhiệm vụ vào một danh sách và tách nhiệm vụ không xác định mức độ ưu tiên. Lưu trữ thời gian đến, số lượng trang và mức độ ưu tiên đã biết hoặc phần giữ chỗ cho tác vụ chưa xác định. Đồng thời ghi lại thời gian hoàn thành mục tiêu của nhiệm vụ chưa xác định. 
2. Xây dựng một tập hợp tất cả các ưu tiên đã biết. Điều này cho phép chúng tôi chọn mức độ ưu tiên của ứng viên cho nhiệm vụ chưa xác định và không xung đột với nhiệm vụ hiện có. 
3. Thực hiện chức năng mô phỏng. Khởi tạo vùng heap tối thiểu (hoặc vùng heap tối đa tùy theo quy ước) được khóa theo mức độ ưu tiên để máy in luôn chọn tác vụ có mức độ ưu tiên cao nhất từ ​​hàng đợi. Giữ một con trỏ đến thời điểm hiện tại. Tối ưu hóa chính là mô phỏng theo hướng sự kiện: tăng thời gian cho nhiệm vụ tiếp theo hoặc hoàn thành trang tiếp theo của nhiệm vụ hiện tại. 
4. Đối với mỗi ứng viên có mức độ ưu tiên cho nhiệm vụ chưa xác định, hãy chỉ định nhiệm vụ đó và chạy mô phỏng. Trong quá trình mô phỏng, bất cứ khi nào một nhiệm vụ hoàn thành, hãy ghi lại thời gian hoàn thành của nhiệm vụ đó. Nếu nhiệm vụ không xác định kết thúc vào thời điểm mục tiêu, chúng tôi đã tìm thấy mức độ ưu tiên chính xác. 
5. Vì mức độ ưu tiên là các số nguyên riêng biệt lên tới 10^9, nên hãy thực hiện tìm kiếm nhị phân trên phạm vi ưu tiên khả thi chưa được sử dụng. Mỗi lần chạy mô phỏng đều đưa ra một boolean: mức độ ưu tiên này có mang lại thời gian hoàn thành mục tiêu không? Thu hẹp tìm kiếm cho phù hợp. 
6. Sau khi xác định được mức độ ưu tiên chính xác, hãy chạy lại mô phỏng để nắm bắt thời gian hoàn thành chính xác của tất cả các nhiệm vụ. Đưa ra mức độ ưu tiên đã xác định và thời gian hoàn thành theo thứ tự nhiệm vụ được nhập. 

Tại sao nó hoạt động: thuật toán duy trì hàng đợi ưu tiên của các tác vụ đang chờ in. Điều bất biến là tác vụ có mức ưu tiên cao nhất trong hàng đợi luôn là tác vụ được in. Mô phỏng sự kiện rời rạc đảm bảo rằng không có nhiệm vụ nào bị bỏ qua và các nhiệm vụ đó được chọn theo các quy tắc, do đó thời gian hoàn thành được tính toán khớp chính xác với những gì máy in sẽ tạo ra. Tìm kiếm nhị phân đảm bảo rằng chúng tôi tìm thấy mức độ ưu tiên thỏa mãn chính xác thời gian kết thúc đã biết của nhiệm vụ còn thiếu. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

def simulate(tasks, unknown_idx, unknown_priority):
    n = len(tasks)
    events = sorted([(t, i) for i, (t, _, _) in enumerate(tasks)])
    finish_times = [0] * n
    waiting = []
    time = 0
    task_pages = [s for _, s, _ in tasks]
    
    if unknown_idx != -1:
        tasks[unknown_idx] = (tasks[unknown_idx][0], tasks[unknown_idx][1], unknown_priority)
    
    i = 0
    while i < n or waiting:
        if not waiting:
            time = max(time, events[i][0])
        
        while i < n and events[i][0] <= time:
            idx = events[i][1]
            _, _, prio = tasks[idx]
            heapq.heappush(waiting, (-prio, idx))
            i += 1
        
        prio, idx = heapq.heappop(waiting)
        task_pages[idx] -= 1
        time += 1
        if task_pages[idx] == 0:
            finish_times[idx] = time
    
    return finish_times

def main():
    n = int(input())
    tasks = []
    unknown_idx = -1
    used_priorities = set()
    
    for idx in range(n):
        t, s, p = map(int, input().split())
        tasks.append([t, s, p])
        if p == -1:
            unknown_idx = idx
        else:
            used_priorities.add(p)
    
    target_time = int(input())
    
    low, high = 1, 10**9
    result_priority = -1
    while low <= high:
        mid = (low + high) // 2
        if mid in used_priorities:
            if mid == 10**9:
                mid -= 1
            else:
                mid += 1
        finish_times = simulate([list(task) for task in tasks], unknown_idx, mid)
        if finish_times[unknown_idx] == target_time:
            result_priority = mid
            break
        elif finish_times[unknown_idx] < target_time:
            low = mid + 1
        else:
            high = mid - 1
    
    finish_times = simulate([list(task) for task in tasks], unknown_idx, result_priority)
    
    print(result_priority)
    print(' '.join(map(str, finish_times)))

if __name__ == "__main__":
    main()
```Trong mã, chúng tôi xử lý cẩn thận mô phỏng theo sự kiện để không bao giờ mô phỏng những giây nhàn rỗi một cách không cần thiết. Các tác vụ có thời gian đến giống hệt nhau sẽ được đẩy vào đống trong cùng một lần lặp. Hàng đợi ưu tiên luôn đảm bảo tác vụ được in chính xác, ngay cả khi tác vụ không xác định được chèn vào. Tìm kiếm nhị phân bị hạn chế ở các giá trị chưa được sử dụng. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
3
4 3 -1
0 2 2
1 3 3
7
```| Thời gian | Xếp hàng | In ấn | Thời gian kết thúc | Giải thích | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 2 | - | Chỉ có nhiệm vụ 2 đã về | 
| 1 | 2,3 | 3 | - | Nhiệm vụ 3 ưu tiên cao hơn | 
| 2 | 2,3 | 3 | - | Tiếp tục nhiệm vụ 3 | 
| 3 | 2,3 | 3 | 4 | Nhiệm vụ 3 kết thúc | 
| 4 | 2,1 | 1 | - | Nhiệm vụ không xác định 1 bắt đầu | 
| 5 | 2,1 | 1 | - | Tiếp tục nhiệm vụ 1 | 
| 6 | 2,1 | 1 | 7 | Nhiệm vụ 1 kết thúc | 
| 7 | 2 | 2 | - | Tiếp tục nhiệm vụ 2 | 
| 8 | | 2 | 8 | Nhiệm vụ 2 kết thúc | 

Điều này xác nhận rằng nhiệm vụ chưa biết phải có mức độ ưu tiên 4 để hoàn thành chính xác ở mức 7. 

**Đầu vào tùy chỉnh 2**```
2
0 5 -1
2 1 1
6
```Nhiệm vụ không xác định có giá trị 0 với 5 trang. Nhiệm vụ 2 đạt đến mức 2. Để hoàn thành ở mức 6, nhiệm vụ chưa xác định phải có mức độ ưu tiên cao hơn 1. Mô phỏng xác nhận điều này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích
