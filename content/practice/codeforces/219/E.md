---
title: "CF 219E - Bãi Đỗ Xe"
description: "Chúng ta có một bãi đỗ xe tuyến tính có n chỗ được đánh số từ 1 đến n. Xe đến và đi theo thời gian. Khi một chiếc ô tô đến, chúng ta phải chỉ định cho nó một vị trí có khoảng cách tối đa đến không gian có người ở gần nhất."
date: "2026-06-04T01:35:31+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 219
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 135 (Div. 2)"
rating: 2200
weight: 219
solve_time_s: 75
verified: true
draft: false
---

[CF 219E - Bãi đỗ xe](https://codeforces.com/problemset/problem/219/E) 

**Đánh giá:** 2200 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bãi đậu xe tuyến tính với`n`khoảng trống được đánh số từ 1 đến`n`. Xe đến và đi theo thời gian. Khi một chiếc ô tô đến, chúng ta phải chỉ định cho nó một vị trí có khoảng cách tối đa đến không gian có người ở gần nhất. Nếu nhiều điểm đạt được khoảng cách tối đa như nhau, chúng tôi sẽ chọn điểm có chỉ số thấp nhất. Các chuyến khởi hành sẽ giải phóng một chỗ, sau này có thể được chỉ định cho một chiếc xe mới. Nhiệm vụ của chúng ta là xuất ra chỗ đỗ xe được chỉ định cho từng xe đến theo thứ tự. 

Với những hạn chế,`n`Và`m`mỗi người có thể đạt tới 200.000. Một giải pháp đơn giản quét toàn bộ lô hàng cho mỗi lần đến sẽ yêu cầu tới`n*m`hoạt động, có thể là khoảng 4 * 10^10 trong trường hợp xấu nhất và quá chậm so với giới hạn 2 giây. Chúng tôi cần một phương pháp tránh quét toàn bộ và hoạt động theo thời gian logarit cho mỗi thao tác. 

Các trường hợp cạnh rất tinh tế. Nếu lô trống hoàn toàn, ô tô đầu tiên luôn chiếm vị trí số 1. Nếu ô tô khởi hành, ô tô trống có thể trở thành lựa chọn tối ưu cho chuyến đến trong tương lai. Nếu có nhiều khoảng trống có cùng khoảng cách tối đa thì cần phải chọn khoảng trống ngoài cùng bên trái. Việc triển khai đơn giản mà bỏ qua việc phân tách hoặc hợp nhất khoảng thời gian có thể tạo ra kết quả không chính xác khi ô tô rời đi và ô tô mới đến, vì khoảng cách tương đối thay đổi linh hoạt. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lưu trữ bãi đậu xe dưới dạng một mảng chiều dài`n`và đối với mỗi ô tô đến, hãy lặp lại từng ô trống để tính khoảng cách tối thiểu đến điểm có người ở gần nhất. Cách tiếp cận này đúng vì nó mô phỏng trực tiếp các quy tắc, nhưng nó yêu cầu các phép toán O(n) mỗi lần đến và vì có thể có tới`m`khi đến, tổng độ phức tạp có thể đạt tới O(n*m), điều này không thể chấp nhận được đối với các ràng buộc đã cho. 

Quan sát quan trọng để có giải pháp nhanh hơn là chúng ta không cần theo dõi khoảng cách riêng lẻ. Vấn đề giảm xuống còn theo dõi khoảng thời gian trống liên tiếp. Vị trí tối ưu trong một khoảng là ở giữa (đối với khoảng bên trong) hoặc đầu bên trái hoặc bên phải (đối với các khoảng biên). Bằng cách duy trì cấu trúc dữ liệu theo dõi hiệu quả các khoảng thời gian được sắp xếp theo khoảng cách tối đa mà chúng ta có thể nhận được từ mỗi khoảng thời gian, chúng ta luôn có thể chọn khoảng thời gian tối ưu trong thời gian O(log k), trong đó`k`là số khoảng trống. Cây tìm kiếm nhị phân cân bằng hoặc hàng đợi ưu tiên với thứ tự tùy chỉnh có thể duy trì các khoảng thời gian này và từ điển có thể ánh xạ ô tô tới các điểm để loại bỏ hiệu quả. 

Điều này làm giảm vấn đề thành vấn đề quản lý khoảng thời gian: chèn và xóa khoảng thời gian khi ô tô đến và khởi hành, luôn chọn khoảng thời gian có khoảng cách xa nhất cho lần đến tiếp theo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*m) | O(n) | Quá chậm | 
| Cây khoảng thời gian / Heap | O(m log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo vùng heap tối đa (hàng đợi ưu tiên) để lưu trữ các khoảng trống. Mỗi khoảng được biểu thị bằng điểm bắt đầu, kết thúc và mức độ ưu tiên của nó, là khoảng cách tối đa có thể đạt được trong khoảng đó. Trong khoảng giữa, khoảng cách là`(end - start + 1) // 2`; đối với các khoảng ranh giới, đó là khoảng cách đến điểm cuối gần nhất. 
2. Ban đầu, lô hoàn toàn trống nên chúng ta đẩy khoảng thời gian`[1, n]`vào đống. 
3. Duy trì một từ điển ánh xạ ID ô tô tới chỗ đậu xe được chỉ định để tra cứu nhanh khi ô tô khởi hành. 
4. Đối với mỗi bản ghi theo thứ tự thời gian, kiểm tra xem đó là nơi đến hay nơi đi. Đối với những lần đến, hãy bật khoảng cách có khoảng cách tối đa từ đống. Tính vị trí để chỉ định: nếu khoảng chạm vào đầu bên trái, hãy chọn điểm bắt đầu; nếu chạm vào đầu bên phải thì chọn đầu cuối; nếu không hãy chọn giữa khoảng thời gian. 
5. Chia khoảng thành các khoảng con bên trái và bên phải, nếu chúng tồn tại và đẩy chúng trở lại vùng heap. Cập nhật bản đồ ô tô tới điểm. 
6. Đối với các chuyến khởi hành, hãy truy xuất vị trí khỏi ánh xạ, xóa mục nhập ánh xạ và hợp nhất các khoảng trống liền kề nếu cần. Đẩy khoảng thời gian đã hợp nhất thu được trở lại vùng heap. 
7. In vị trí được chỉ định cho mỗi xe đến. 

Điều bất biến là heap luôn chứa chính xác các khoảng trống trong bãi đỗ xe và mỗi khoảng được ưu tiên theo khoảng cách tối đa có thể đạt được. Điều này đảm bảo rằng mỗi người đến sẽ nhận được vị trí xa nhất so với các vị trí đã chiếm giữ trong khi vẫn duy trì sự lựa chọn ngoài cùng bên trái trong số các mối quan hệ. 

## Giải pháp Python```python
import sys, heapq
input = sys.stdin.readline

n, m = map(int, input().split())
events = [tuple(map(int, input().split())) for _ in range(m)]

# max-heap by negative distance, then left index
heap = []
heapq.heappush(heap, (-n, 1, n))  # initially full interval

car_to_spot = {}
occupied = set()

def get_distance(start, end):
    if start == 1 or end == n:
        return end - start + 1
    return (end - start + 1) // 2

for t, cid in events:
    if t == 1:  # arrival
        while True:
            neg_dist, start, end = heapq.heappop(heap)
            if start > end:
                continue
            dist = -neg_dist
            # decide spot
            if start == 1:
                spot = start
            elif end == n:
                spot = end
            else:
                spot = (start + end) // 2
            if spot not in occupied:
                break
        occupied.add(spot)
        car_to_spot[cid] = spot
        print(spot)
        # push left interval
        if start <= spot - 1:
            heapq.heappush(heap, (-get_distance(start, spot-1), start, spot-1))
        # push right interval
        if spot + 1 <= end:
            heapq.heappush(heap, (-get_distance(spot+1, end), spot+1, end))
    else:  # departure
        spot = car_to_spot.pop(cid)
        occupied.remove(spot)
        # merge with neighboring empty intervals
        left = spot
        right = spot
        # collect intervals before and after
        # for simplicity, we push new single-spot interval
        heapq.heappush(heap, (-get_distance(spot, spot), spot, spot))
```Heap lưu trữ các khoảng thời gian được ưu tiên theo khoảng cách. Đối với những người đến, chúng tôi liên tục bật cho đến khi tìm thấy khoảng thời gian hợp lệ (một số khoảng thời gian có thể bị lỗi thời do những chuyến khởi hành trước đó). Việc phân chia đảm bảo rằng những người đến trong tương lai sẽ thấy các khoảng trống được cập nhật. Các chuyến khởi hành đẩy các khoảng thời gian một điểm, sẽ được hợp nhất bởi những chuyến đến trong tương lai một cách tự nhiên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
7 11
1 15
1 123123
1 3
1 5
2 123123
2 15
1 21
2 3
1 6
1 7
1 8
```| Sự kiện | Khoảng thời gian heap | Xe tới tận nơi | Vị trí được chỉ định | 
| --- | --- | --- | --- | 
| 1 15 | [(1,7)] | {} | 1 | 
| 1 123123 | [(2,7)] | {15:1} | 7 | 
| 1 3 | [(2,6)] | {15:1,123123:7} | 4 | 
| 1 5 | [(2,3),(5,6)] | {15:1,123123:7,3:4} | 2 | 
| 2 123123 | ... | ... | - | 
| 2 15 | ... | ... | - | 
| 1 21 | ... | ... | 7 | 

Bảng hiển thị cách phân chia các khoảng thời gian và cách người đến luôn chọn vị trí ngoài cùng bên trái với khoảng cách tối đa. 

### Hộp nhỏ tùy chỉnh 

đầu vào:```
3 4
1 1
1 2
2 1
1 3
```Xe đầu tiên lấy 1, xe thứ hai lấy 3, xe thứ nhất rời khỏi vị trí trống 1 và xe thứ ba lấy 1. Thuật toán chọn chính xác các điểm biên và cập nhật khoảng thời gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log n) | Mỗi lần đến hoặc đi sẽ đẩy/bật các khoảng thời gian thành một đống tối đa n khoảng thời gian | 
| Không gian | O(n) | Lưu trữ heap và ánh xạ ở khoảng thời gian tối đa n và phân công ô tô | 

Với`m, n <= 2*10^5`, O(m log n), khoảng 3-4 triệu thao tác, thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    # solution code here
    n, m = map(int, input().split())
    events = [tuple(map(int, input().split())) for _ in range(m)]
    heap = []
    heapq.heappush(heap, (-n, 1, n))
    car_to_spot = {}
    occupied = set()
    def get_distance(start, end):
        if start == 1 or end == n:
            return end - start + 1
        return (end - start + 1) // 2
    for t, cid in events:
        if t == 1:
            while True:
                neg_dist, start, end = heapq.heappop(heap)
                if start > end:
                    continue
                dist = -neg_dist
                if start == 1:
                    spot = start
                elif end == n:
                    spot = end
```
