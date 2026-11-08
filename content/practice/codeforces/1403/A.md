---
title: "CF 1403A - Liều thuốc sức mạnh vĩ đại"
description: "Chúng tôi có một mạng lưới tình bạn năng động giữa N pháp sư, mỗi người sống ở độ cao cụ thể H[i]. Ban đầu, không có pháp sư nào tin tưởng bất cứ ai, và mỗi ngày một tình bạn duy nhất hình thành hoặc tan vỡ. Mỗi pháp sư có thể tin tưởng tối đa D người khác vào bất kỳ lúc nào."
date: "2026-06-11T08:20:48+07:00"
tags: ["codeforces", "competitive-programming", "*special", "2-sat", "binary-search", "data-structures", "graphs", "interactive", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1403
codeforces_index: "A"
codeforces_contest_name: "Central-European Olympiad in Informatics, CEOI 2020, Day 2 (IOI, Unofficial Mirror Contest, Unrated)"
rating: 2400
weight: 1403
solve_time_s: 123
verified: false
draft: false
---

[CF 1403A - Liều thuốc sức mạnh vĩ đại](https://codeforces.com/problemset/problem/1403/A) 

**Đánh giá:** 2400 
**Thẻ:** *đặc biệt, 2-sat, tìm kiếm nhị phân, cấu trúc dữ liệu, đồ thị, tương tác, sắp xếp, hai con trỏ 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mạng lưới tình bạn năng động giữa`N`pháp sư, mỗi người sống ở một độ cao cụ thể`H[i]`. Ban đầu, không có pháp sư nào tin tưởng bất cứ ai, và mỗi ngày một tình bạn duy nhất hình thành hoặc tan vỡ. Mỗi pháp sư có thể tin tưởng nhiều nhất`D`những người khác bất cứ lúc nào. 

Nhiệm vụ là trả lời các câu hỏi có dạng sau: đưa ra một Kẻ trộm bị nghi ngờ`x`, một pháp sư tà ác bị nghi ngờ`y`, và một ngày`v`, tìm khoảng cách tối thiểu giữa độ cao của một người bạn đáng tin cậy của`x`và một người bạn đáng tin cậy của`y`vào cuối ngày`v`. Nếu như`x`Và`y`chia sẻ một người bạn, khoảng cách là`0`. Nếu một trong hai không có bạn bè thì khoảng cách được xác định là`10^9`. 

Các ràng buộc rất chặt chẽ:`N`có thể đạt tới 100.000 và số lượng cập nhật`U`có thể đạt tới 200.000, với tối đa 50.000 truy vấn. Cách tiếp cận mạnh mẽ để tính toán lại nhóm bạn bè hoặc khoảng cách cho mỗi truy vấn sẽ quá chậm. Mức độ tối đa`D`là nhỏ, lên tới 500, cho phép chúng tôi khai thác sự thưa thớt của mạng lưới bạn bè của mỗi pháp sư. 

Các trường hợp Edge bao gồm những ngày trước khi có bất kỳ bản cập nhật nào (`v = 0`), pháp sư không có bạn bè, và tình huống mà cùng một người bạn được tính cho cả hai`x`Và`y`. Một cách tiếp cận ngây thơ có thể bỏ lỡ`v = 0`hoặc xử lý không chính xác việc chuyển đổi tình bạn lặp đi lặp lại trên cùng một cặp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ duy trì biểu đồ tình bạn mỗi ngày bằng cách phát lại tất cả các bản cập nhật và sau đó, đối với mỗi truy vấn, hãy lặp qua tất cả các cặp bạn bè để tính toán khoảng cách. Điều này đúng nhưng không thực tế vì mỗi truy vấn có thể tốn`O(D^2)`hoạt động và cập nhật biểu đồ cho tất cả`U`ngày sẽ là`O(U*D)`. Đối với giới hạn trên, điều này trở thành hàng tỷ thao tác, vượt quá giới hạn hợp lý. 

Cái nhìn sâu sắc quan trọng là mỗi pháp sư có nhiều nhất`D`bạn bè, điều đó thật nhỏ bé so với`N`. Điều này cho phép chúng tôi duy trì nhóm bạn hiện tại một cách linh hoạt và trả lời các truy vấn một cách hiệu quả. Vì số ngày tăng dần nên chúng tôi có thể xử lý các cập nhật theo thứ tự và chỉ chụp nhanh các nhóm tình bạn khi cần. Trong một truy vấn trong ngày`v`, chúng ta chỉ cần nhóm bạn của`x`Và`y`vào ngày đó. Chúng ta có thể duy trì nhật ký tình bạn theo thời gian hoặc chỉ cần áp dụng các bản cập nhật tăng dần khi có truy vấn. 

Để tính toán khoảng cách tối thiểu một cách hiệu quả, chúng ta có thể sắp xếp độ cao của những người bạn của`x`Và`y`và sử dụng thao tác quét hai con trỏ để tìm sự khác biệt tuyệt đối tối thiểu. Điều này hiệu quả vì mỗi nhóm bạn có nhiều nhất`D`các phần tử, thực hiện sắp xếp và quét`O(D log D + D)`cho mỗi truy vấn, có thể chấp nhận được`D <= 500`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tính toán lại tất cả các bộ cho mỗi truy vấn) | O(U_D + Q_D^2) | O(N*D) | Quá chậm | 
| Cập nhật gia tăng + hai con trỏ cho mỗi truy vấn | O(U + Q*D log D) | O(N*D) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số pháp sư`N`, số bạn bè tối đa`D`, số lượng cập nhật`U`, và số lượng truy vấn`Q`. Đọc độ cao`H[i]`. 
2. Khởi tạo danh sách các bộ`friends[i]`để theo dõi những người bạn hiện tại của mỗi pháp sư. Vì mỗi pháp sư có nhiều nhất`D`các bạn ơi, những bộ này vẫn còn nhỏ. 
3. Đọc`U`cập nhật tình bạn`(A_i, B_i)`. Lưu trữ chúng trong một danh sách để áp dụng tuần tự. 
4. Khởi tạo một`current_day = 0`. Đối với mỗi truy vấn`(x, y, v)`, áp dụng tất cả các bản cập nhật từ`current_day`tính đến ngày`v`bằng cách chuyển đổi tình bạn trong`friends[A_i]`Và`friends[B_i]`. Cập nhật`current_day = v`. 
5. Trích xuất bạn bè của`x`Và`y`. Nếu một trong hai trống, hãy quay lại`10^9`. Nếu giao lộ không trống, hãy quay lại`0`. 
6. Sắp xếp độ cao của bạn bè`x`Và`y`. Thực hiện quét hai con trỏ: giữ một con trỏ trong mỗi danh sách đã sắp xếp và di chuyển con trỏ tương ứng với độ cao nhỏ hơn. Cập nhật sự khác biệt tuyệt đối tối thiểu ở mỗi bước. 
7. In khoảng cách tối thiểu. 

Tại sao nó hoạt động: Tại mỗi truy vấn, nhóm bạn bè phản ánh trạng thái chính xác trong ngày`v`do việc áp dụng tuần tự các bản cập nhật. Từ`D`nhỏ, việc sắp xếp và so sánh hai con trỏ sẽ tìm ra khoảng cách tối thiểu thực sự một cách hiệu quả. Các trường hợp cạnh dành cho nhóm bạn bè trống hoặc bạn bè được chia sẻ được kiểm tra rõ ràng, do đó không có truy vấn nào trả về giá trị không chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from bisect import bisect_left

N, D, U, Q = map(int, input().split())
H = list(map(int, input().split()))

updates = [tuple(map(int, input().split())) for _ in range(U)]
friends = [set() for _ in range(N)]

current_day = 0

def apply_update(a, b):
    if b in friends[a]:
        friends[a].remove(b)
        friends[b].remove(a)
    else:
        if len(friends[a]) < D and len(friends[b]) < D:
            friends[a].add(b)
            friends[b].add(a)

for _ in range(Q):
    x, y, v = map(int, input().split())
    # apply all updates until day v
    while current_day < v:
        a, b = updates[current_day]
        apply_update(a, b)
        current_day += 1
    
    fx = friends[x]
    fy = friends[y]
    
    if not fx or not fy:
        print(10**9)
        continue
    if fx & fy:
        print(0)
        continue
    
    # two-pointer minimal distance
    Ax = sorted(H[i] for i in fx)
    Ay = sorted(H[i] for i in fy)
    
    i = j = 0
    best = 10**9
    while i < len(Ax) and j < len(Ay):
        best = min(best, abs(Ax[i] - Ay[j]))
        if Ax[i] < Ay[j]:
            i += 1
        else:
            j += 1
    print(best)
```Giải pháp duy trì nhóm bạn bè một cách hiệu quả, chuyển đổi dần dần tình bạn. Việc sắp xếp các tập hợp nhỏ cho mỗi truy vấn đảm bảo tính toán khoảng cách tối thiểu được nhanh chóng. Quét hai con trỏ khai thác thứ tự độ cao, đảm bảo chúng ta không bỏ lỡ cặp tối thiểu. 

## Ví dụ đã hoạt động 

Đối với mẫu 1: 

| Ngày | Đã áp dụng cập nhật | Bạn bè của 0 | Bạn của 3 | Truy vấn (0,3,v) | Khoảng cách tối thiểu | 
| --- | --- | --- | --- | --- | --- | 
| 0 | - | {} | {} | 0,3,4 | 26 | 
| 1 | 0 1 | {1} | {} | - | - | 
| 2 | 2 0 | {1,2} | {0} | - | - | 
| 3 | 3 4 | {1,2} | {4} | - | - | 
| 4 | 3 5 | {1,2} | {4,5} | 0,3,4 | khoảng cách = phút( | 

Bảng này chứng tỏ rằng ứng dụng cập nhật tuần tự duy trì các nhóm bạn bè chính xác cho mỗi truy vấn. Sắp xếp và hai con trỏ cho khoảng cách tối thiểu chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(U + Q*D log D) | Mỗi bản cập nhật chuyển đổi tình bạn trong O(1) và mỗi truy vấn sắp xếp hai nhóm bạn bè có kích thước tối đa là D và thực hiện quét hai con trỏ tuyến tính. | 
| Không gian | O(N*D + U) | Mỗi pháp sư lưu trữ tối đa D bạn bè và tất cả các bản cập nhật đều được lưu trữ. | 

Điều này phù hợp thoải mái trong các ràng buộc:`U`lên tới 2e5,`D`lên tới 500,`Q`lên đến 5e4, cung cấp ít nhất 5e4_500_log500 ≈ 7e6 cho mỗi giai đoạn truy vấn, cộng với xử lý cập nhật tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        exec(open("solution.py").read())
    return out.getvalue().strip()

# provided sample
assert run("""6 5 11 4
2 42 1000 54 68 234
0 1
2 0
3 4
3 5
3 5
1 3
5 3
0 5
3 0
1 3
3 5
0 3 4
3 0 8
0 5 5
3 0 11""") == "26\n0\n1000000000\n14"

# custom cases
assert run("""2 1 1 2
0 10
0 1
0 1 0
0 1 1""") == "1000000000\n10"

assert run("""3 2 2 1
1 2 3
0 1
1 2
0 2 2""") == "1
```
