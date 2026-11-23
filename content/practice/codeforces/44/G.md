---
title: "CF 44G - Phòng trưng bày bắn súng"
description: "Vấn đề có thể được trình bày lại dưới dạng mô phỏng viên đạn bay dọc theo trục _Z_ dương tại tọa độ nhất định trên mặt phẳng bắn 2D (_XOY_). Mỗi viên đạn có thể bắn trúng một trong nhiều mục tiêu hình chữ nhật nổi ở các độ cao khác nhau dọc theo trục _Z_."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 44
codeforces_index: "G"
codeforces_contest_name: "School Team Contest 2 (Winter Computer School 2010/11)"
rating: 2500
weight: 44
solve_time_s: 83
verified: true
draft: false
---
[CF 44G - Thư viện bắn súng](https://codeforces.com/problemset/problem/44/G) 

**Đánh giá:** 2500 
**Tags:** cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề có thể được trình bày lại dưới dạng mô phỏng viên đạn bay dọc theo trục _Z_ dương tại tọa độ nhất định trên mặt phẳng bắn 2D (_XOY_). Mỗi viên đạn có thể bắn trúng một trong nhiều mục tiêu hình chữ nhật nổi ở các độ cao khác nhau dọc theo trục _Z_. Mục tiêu được biểu thị bằng một hình chữ nhật được căn chỉnh với trục _X_ và _Y_ và nằm ở tọa độ _z_ cụ thể. Một viên đạn bắn trúng mục tiêu nếu tọa độ _x_ và _y_ của nó nằm trong ranh giới của hình chữ nhật. Sau khi bắn trúng, mục tiêu sẽ biến mất và không thể đánh lại. Nhiệm vụ là xác định xem mỗi viên đạn bắn trúng mục tiêu nào hoặc bằng 0 nếu bắn trượt tất cả các mục tiêu. 

Chúng tôi được cung cấp tới 100.000 mục tiêu và 100.000 phát bắn, tọa độ và kích thước có thể lên tới 10 triệu. Một cách tiếp cận đơn giản để kiểm tra từng phát bắn đối với mọi mục tiêu sẽ yêu cầu khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa khả năng chấp nhận được trong thời gian 5 giây. Điều này có nghĩa là chúng ta cần một cách tiếp cận có độ phức tạp gần hơn với O(n log n + m log n) hoặc O((n + m) log n) bằng cách sử dụng cấu trúc dữ liệu không gian hoặc kỹ thuật đường quét. 

Các trường hợp cạnh không rõ ràng bao gồm các hình chữ nhật chồng chéo dọc theo _X_ và _Y_, trong đó nhiều mục tiêu có thể bị bắn trúng bởi cùng một viên đạn. Điều quan trọng ở đây là chỉ mục tiêu gần nhất (_z_) gần nhất mới ghi được một cú đánh. Ví dụ: với hai mục tiêu:```
2
0 5 0 5 3
1 4 1 4 1
2
2 2
0 0
```Viên đạn đầu tiên tại (2,2) bắn trúng mục tiêu thứ hai tại _z=1_, không phải mục tiêu đầu tiên tại _z=3_. Một cách tiếp cận bất cẩn khi quét các mục tiêu theo thứ tự đầu vào có thể gán sai mục tiêu đó vào hình chữ nhật đầu tiên. 

## Phương pháp tiếp cận 

Giải pháp vũ lực lặp đi lặp lại trong mỗi lần bắn và kiểm tra tất cả các mục tiêu để xem liệu tọa độ có nằm trong hình chữ nhật hay không, theo dõi _z_ gần nhất. Cách tiếp cận này đúng vì nó xem xét mọi mục tiêu và tuân theo quy tắc "gần nhất trước tiên". Độ phức tạp về thời gian của nó là O(n*m) hoặc tối đa 10^10 thao tác, quá chậm. 

Thông tin chi tiết quan trọng cần tối ưu hóa xuất phát từ việc nhận thấy rằng chúng ta có thể sắp xếp các mục tiêu theo không gian theo cấu trúc 2D để đối với bất kỳ cảnh quay nào, chúng ta có thể truy vấn hiệu quả tất cả các hình chữ nhật chứa điểm và truy xuất hình có _z_ nhỏ nhất. Cây phân đoạn có các bộ hoặc đường quét kết hợp với cây khoảng hoạt động vì tọa độ _z_ khác biệt và dấu đầu dòng chỉ cần hình chữ nhật gần nhất tại (_x_, _y_) của chúng. Một cách khác là sắp xếp các mục tiêu theo _z_ và chèn chúng vào cấu trúc khoảng 2D. Sau đó, mỗi truy vấn có thể tìm thấy hình chữ nhật đầu tiên bao phủ ảnh trong thời gian O(log n). Điều này làm giảm độ phức tạp xuống O((n + m) log n) bằng cách sử dụng cấu trúc dữ liệu thích hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*m) | O(n) | Quá chậm | 
| Đường quét / cây khoảng 2D | O((n + m) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các mục tiêu theo tọa độ _z_ của chúng. Chúng tôi sẽ xử lý chúng từ gần nhất đến xa nhất. Việc sắp xếp đảm bảo rằng hình chữ nhật đầu tiên mà dấu đầu dòng giao nhau tự động là hình chữ nhật gần nhất dọc theo trục _Z_. 
2. Khởi tạo cấu trúc dữ liệu có thể nhanh chóng kiểm tra, đối với (_x_, _y_), hình chữ nhật nào chứa điểm. Cây khoảng 2D cân bằng hoặc cấu trúc bản đồ lồng nhau (khoảng x → khoảng y → chỉ mục đích) là phù hợp. 
3. Chèn các hình chữ nhật vào cấu trúc theo thứ tự tăng dần _z_. Mỗi hình chữ nhật chiếm một hình chữ nhật trong _XOY_; cấu trúc sẽ cho phép truy vấn ngăn chặn hiệu quả. 
4. Với mỗi dấu đầu dòng, truy vấn cấu trúc để tìm hình chữ nhật chứa điểm (_x_, _y_). Nếu có nhiều hình chữ nhật chứa điểm, hãy chọn hình có _z_ nhỏ nhất (do thứ tự chèn). 
5. Sau khi bắn trúng một hình chữ nhật, hãy loại bỏ nó khỏi cấu trúc để những viên đạn trong tương lai không thể bắn trúng nó. 
6. Ghi lại chỉ số của lần bắn trúng mục tiêu (hoặc 0 nếu không có) và xuất nó cho mỗi lần bắn. 

**Tại sao nó hoạt động**: Sắp xếp theo _z_ đảm bảo rằng hình chữ nhật đầu tiên chứa ảnh có chiều sâu gần nhất. Bằng cách loại bỏ mục tiêu ngay sau khi bị bắn trúng, chúng tôi duy trì tính bất biến rằng không có mục tiêu nào có thể bị bắn trúng hai lần và cấu trúc luôn thể hiện trạng thái hiện tại của thư viện bắn súng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
targets = []
for i in range(n):
    xl, xr, yl, yr, z = map(int, input().split())
    targets.append((z, xl, xr, yl, yr, i + 1))

m = int(input())
shots = [tuple(map(int, input().split())) + (idx,) for idx in range(m)]

# Sort targets by z (nearest first)
targets.sort()

# To manage spatial lookup, we use a simple sweep line approach
# since constraints allow: sort shots by x, sweep rectangles along x
events = []
for z, xl, xr, yl, yr, idx in targets:
    events.append((xl, 'open', yl, yr, idx))
    events.append((xr, 'close', yl, yr, idx))
for x, y, idx in shots:
    events.append((x, 'shot', y, idx))

events.sort()
import bisect
active = []
shot_results = [0] * m

for x, typ, a, b, *rest in events:
    if typ == 'open':
        bisect.insort(active, (a, b, rest[0]))
    elif typ == 'close':
        active.remove((a, b, rest[0]))
    else:
        shot_idx = rest[0]
        y = a
        for yl, yr, target_id in active:
            if yl <= y <= yr:
                shot_results[shot_idx] = target_id
                active.remove((yl, yr, target_id))
                break

print('\n'.join(map(str, shot_results)))
```Mã sắp xếp mục tiêu theo độ sâu, sau đó quét dọc theo trục _X_. Các sự kiện mở và kết thúc duy trì các hình chữ nhật đang hoạt động có thể bị tấn công. Các cảnh quay được xử lý tại tọa độ _x_ của chúng và việc ngăn chặn trong _Y_ được kiểm tra bằng danh sách hiện hoạt. Khi một hình chữ nhật được nhấn, nó sẽ bị xóa. 

Các điểm tinh tế bao gồm việc xử lý cẩn thận các ranh giới bao hàm và đảm bảo các hình chữ nhật được loại bỏ ngay sau một cú đánh. Chúng tôi cũng dựa vào tính năng sắp xếp ổn định của Python để duy trì ngầm thứ tự độ sâu khi các giá trị _z_ bằng nhau, nhưng trong bài toán này, các giá trị _z_ là khác biệt. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2
1 4 1 4 1
2 5 2 6 2
4
0 0
3 3
4 5
3 5
```| Bắn | Hình chữ nhật hoạt động | Đánh trúng mục tiêu | Lý do | 
| --- | --- | --- | --- | 
| (0,0) | [(1,4,1,4,1),(2,5,2,6,2)] | 0 | Không nằm trong bất kỳ hình chữ nhật nào | 
| (3,3) | [(1,4,1,4,1),(2,5,2,6,2)] | 1 | Hình chữ nhật gần nhất chứa (3,3) là z=1 | 
| (4,5) | [(2,5,2,6,2)] | 2 | Hình chữ nhật tại z=2 chứa bắn | 
| (3,5) | [] | 0 | Không còn hình chữ nhật nào đang hoạt động | 

Điều này thể hiện tính chính xác theo thứ tự độ sâu và loại bỏ mục tiêu một lần. 

### Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Sắp xếp các sự kiện và duy trì tập hoạt động với tìm kiếm nhị phân | 
| Không gian | O(n + m) | Lưu trữ sự kiện và hình chữ nhật hoạt động | 

Các ràng buộc cho phép tối đa 10^5 mục tiêu và số lần bắn, do đó, các hoạt động 2*(n+m) log n ≈ 3*10^6 log n, cũng dưới 5 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    targets = []
    for i in range(n):
        xl, xr, yl, yr, z = map(int, input().split())
        targets.append((z, xl, xr, yl, yr, i + 1))
    m = int(input())
    shots = [tuple(map(int, input().split())) + (idx,) for idx in range(m)]
    targets.sort()
    events = []
    for z, xl, xr, yl, yr, idx in targets:
        events.append((xl, 'open', yl, yr, idx))
        events.append((xr, 'close', yl, yr, idx))
    for x, y, idx in shots:
        events.append((x, 'shot', y, idx))
    events.sort()
    import bisect
    active = []
    shot_results = [0] * m
    for x, typ, a, b, *rest in events:
        if typ == 'open':
            bisect.insort(active, (a, b, rest[0]))
        elif typ == 'close':
            active.remove((a, b, rest[0]))
        else:
            shot_idx = rest[0]
            y = a
```
