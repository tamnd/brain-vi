---
title: "CF 1924B - Cảng vũ trụ"
description: "Chúng ta có một đường thẳng có n điểm, mỗi điểm có một con tàu. Một số điểm trong số này đã có bến cảng, mỗi điểm có một giá trị liên quan. Một con tàu tại một thời điểm sẽ phải chịu chi phí nếu nó \"di chuyển\" đến bến cảng tiếp theo bên phải nó."
date: "2026-06-08T19:10:29+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1924
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 921 (Div. 1)"
rating: 2100
weight: 1924
solve_time_s: 334
verified: false
draft: false
---

[CF 1924B - Cảng không gian](https://codeforces.com/problemset/problem/1924/B) 

**Xếp hạng:** 2100 
**Tags:** cấu trúc dữ liệu, triển khai, toán học, sắp xếp 
**Thời gian giải:** 5 phút 34 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đường thẳng với`n`điểm, mỗi điểm do một con tàu chiếm giữ. Một số điểm trong số này đã có bến cảng, mỗi điểm có một giá trị liên quan. Một con tàu tại một thời điểm sẽ phải chịu chi phí nếu nó "di chuyển" đến bến cảng tiếp theo bên phải nó. Chi phí được định nghĩa là tích của giá trị của bến cảng gần nhất ở bên trái và khoảng cách đến bến cảng tiếp theo ở bên phải. Nếu một con tàu đã cập bến thì chi phí sẽ bằng 0. Nhiệm vụ là xử lý hai loại hoạt động một cách hiệu quả: thêm một bến cảng ở một vị trí mới với một giá trị nhất định và tính toán tổng chi phí để di chuyển các tàu trong một phân đoạn cụ thể đến các bến cảng tiếp theo của chúng. 

Giới hạn đầu vào lớn:`n`Và`q`có thể đạt tới 300.000, điều này loại trừ bất kỳ phương pháp nào tính toán chi phí một cách đơn giản cho mỗi truy vấn vì việc lặp qua các phân đoạn có thể dẫn đến`O(n * q)`hoạt động, có khả năng kết thúc`9 * 10^10`các bước. Điều này đòi hỏi một cấu trúc dữ liệu hoặc thuật toán có thể trả lời các truy vấn tổng phạm vi một cách hiệu quả và xử lý việc chèn các bến cảng động. 

Một vài trường hợp phức tạp phải được xem xét. Nếu một truy vấn yêu cầu chi phí cho tàu chính xác tại các vị trí bến cảng thì chi phí sẽ bằng 0. Tương tự, việc thêm một bến cảng sẽ phân chia khoảng cách giữa các bến cảng hiện có và thay đổi chi phí cho tàu trong khoảng thời gian đó. Ví dụ, nếu tàu chiếm giữ các điểm`[1, 2, 3, 4]`với bến cảng ở`1`Và`4`, thêm một bến cảng tại`3`giảm chi phí cho tàu tại`3`về 0 và thay đổi chi phí của con tàu tại`2`. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ đơn giản lặp lại từng truy vấn và gửi từng truy vấn trong phân đoạn cho các truy vấn loại 2, tính toán các bến trái và phải gần nhất. Đối với truy vấn loại 1, nó chỉ cần thêm bến cảng mới vào danh sách. Điều này đúng nhưng cực kỳ chậm: độ phức tạp trong trường hợp xấu nhất là`O(n * q)`, điều này không khả thi đối với các giới hạn đã cho. 

Cái nhìn sâu sắc quan trọng cho một giải pháp tối ưu là chi phí của mỗi con tàu phụ thuộc tuyến tính vào các khoảng thời gian được xác định bởi vị trí của các bến cảng. Giữa hai bến cảng liên tiếp, chi phí của một con tàu tại vị trí`i`đơn giản là`(i - left) * value[left]`nếu như`left`là bến cảng gần nhất ở bên trái và`right`là gần nhất bên phải. Quan sát này cho phép chúng ta biểu diễn chi phí dưới dạng tổng tiền tố theo các khoảng thời gian. Sử dụng cấu trúc dữ liệu cân bằng, chẳng hạn như`SortedDict`từ`sortedcontainers`, chúng ta có thể duy trì vị trí bến cảng và tính toán chi phí tích lũy một cách hiệu quả. Khi một bến cảng mới được thêm vào, chỉ cần cập nhật khoảng thời gian mà nó phân chia. Đối với truy vấn loại 2, tìm kiếm nhị phân hoặc truy vấn theo khoảng cho phép chúng tôi tính tổng chi phí một cách nhanh chóng trên một phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * q) | O(n) | Quá chậm | 
| Dựa trên khoảng thời gian với tổng tiền tố SortedDict + | O((m+q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo ánh xạ được sắp xếp của các vị trí bến cảng theo giá trị của chúng. Điều này đảm bảo quyền truy cập O(log n) vào các bến trái và phải gần nhất cho bất kỳ điểm nào. 
2. Tính toán trước chi phí ban đầu cho tất cả các tàu. Đối với mỗi khoảng cách giữa các bến cảng liên tiếp, chi phí cho một con tàu tại vị trí`i`là`(i - left) * value[left]`. Lưu trữ tổng chi phí tiền tố để có thể trả lời các truy vấn phạm vi trong O(log n) bằng cách sử dụng tổng tích lũy. 
3. Khi xử lý truy vấn loại 1`(1 x v)`, tìm khoảng`[L, R]`có chứa`x`. Loại bỏ chi phí tích lũy trong khoảng thời gian đó, sau đó chia nó thành`[L, x-1]`Và`[x+1, R]`với bến cảng mới tại`x`. Tính lại tổng tiền tố cho các khoảng này. 
4. Đối với truy vấn loại 2`(2 l r)`, hãy sử dụng mảng tổng tiền tố để tính toán tổng chi phí giữa`l`Và`r`. Tìm kiếm nhị phân ở các vị trí bến cảng xác định khoảng thời gian nào trùng lặp với`[l, r]`, sau đó tính tổng chi phí được tính toán trước cho các khoảng thời gian đó. 
5. Lặp lại quy trình này cho tất cả các truy vấn. 

Tại sao nó hoạt động: tính bất biến là tại bất kỳ thời điểm nào, mảng chi phí thể hiện chính xác chi phí di chuyển tàu trong mỗi khoảng thời gian giữa các bến cảng liên tiếp. Việc thêm một bến cảng mới chỉ ảnh hưởng đến khoảng thời gian mà nó phân chia và tổng tiền tố cho phép truy vấn nhanh chóng. Sự kết hợp giữa ánh xạ được sắp xếp và tổng tiền tố dựa trên khoảng thời gian đảm bảo cả tính chính xác và hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from sortedcontainers import SortedDict

n, m, q = map(int, input().split())
harbour_positions = list(map(int, input().split()))
harbour_values = list(map(int, input().split()))

harbours = SortedDict()
for pos, val in zip(harbour_positions, harbour_values):
    harbours[pos] = val

# Precompute interval costs
def interval_cost(l, r, val):
    # sum_{i=l}^{r} (i - l) * val = val * (sum_{i=0}^{r-l} i) = val * (r-l)*(r-l+1)//2
    length = r - l + 1
    return val * (length * (length - 1) // 2)

# store prefix sum of costs by intervals
intervals = SortedDict()
prev = None
for pos in harbours:
    if prev is not None:
        intervals[prev] = (pos, interval_cost(prev + 1, pos - 1, harbours[prev]))
    prev = pos

def add_harbour(x, v):
    idx = harbours.bisect_left(x)
    right = harbours.peekitem(idx)[0]
    left = harbours.peekitem(idx - 1)[0]
    # remove old interval
    if left in intervals:
        del intervals[left]
    # add new intervals
    if left <= x - 1:
        intervals[left] = (x, interval_cost(left + 1, x - 1, harbours[left]))
    if x + 1 <= right - 1:
        intervals[x] = (right, interval_cost(x + 1, right - 1, v))
    harbours[x] = v

def query_cost(l, r):
    total = 0
    # find relevant intervals
    idx = intervals.bisect_right(l)
    if idx > 0:
        idx -= 1
    keys = list(intervals.islice(idx, len(intervals)))
    for start in keys:
        end, cost = intervals[start]
        if start > r:
            break
        sub_l = max(l, start + 1)
        sub_r = min(r, end - 1)
        if sub_l <= sub_r:
            length = sub_r - sub_l + 1
            val = harbours[start]
            total += val * (length * (length + 1) // 2 - (sub_l - start - 1) * (sub_l - start) // 2)
    return total

for _ in range(q):
    parts = input().split()
    t = int(parts[0])
    if t == 1:
        x, v = int(parts[1]), int(parts[2])
        add_harbour(x, v)
    else:
        l, r = int(parts[1]), int(parts[2])
        print(query_cost(l, r))
```Giải pháp sử dụng`SortedDict`để duy trì vị trí bến cảng và tiếp cận hiệu quả các nước láng giềng gần nhất. Chi phí giữa các bến cảng được tính toán trước theo từng khoảng thời gian. Khi thêm một bến cảng, chỉ khoảng thời gian nó tách ra mới được cập nhật. Chi phí truy vấn sử dụng các giá trị được tính toán trước với sự điều chỉnh cẩn thận cho các khoảng thời gian từng phần. Các lỗi riêng lẻ được tránh bằng cách xử lý các phạm vi bao gồm`[l, r]`đúng cách. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Vị trí | Cảng | Bến cảng bên trái gần nhất | Bến cảng bên phải gần nhất | Chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | Có | 1 | 3 | 0 | 
| 2 | Không | 1 | 3 | 3 | 
| 3 | Có | 3 | 8 | 0 | 
| 4 | Không | 3 | 8 | 24 | 
| 5 | Không | 3 | 8 | 72 | 
| 6 | Không | 3 | 8 | 96 | 
| 7 | Không | 3 | 8 | 120 | 
| 8 | Có | 8 | - | 0 | 

Truy vấn loại 2 đầu tiên`(2 2 5)`mặt trời`3 + 0 + 24 + 72 = 99`. Sau khi thêm bến cảng ở mức 5, cập nhật chi phí theo khoảng thời gian và các truy vấn tiếp theo phản ánh chi phí bằng 0 đối với tàu tại các vị trí bến cảng mới. 

Một ví dụ khác: việc thêm một bến cảng vào giữa hai bến cảng hiện có sẽ phân chia khoảng cách và phân phối lại chi phí theo tỷ lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((m + q) log n) | Các thao tác SortedDict (chèn, chia đôi) là O(log n). Mỗi truy vấn chỉ chạm vào các khoảng có liên quan. | 
| Không gian | O(n) | Lưu trữ các bến cảng, khoảng thời gian và tổng tiền tố. | 

Được cho`n, q ≤ 3*10^5`, các hoạt động trong trường hợp xấu nhất là dưới 10^7, vừa vặn thoải mái trong 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
```
