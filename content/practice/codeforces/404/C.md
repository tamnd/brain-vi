---
title: "CF 404C - Khôi phục đồ thị"
description: "Chúng ta được yêu cầu xây dựng lại một đồ thị liên thông vô hướng từ danh sách các khoảng cách ngắn nhất đến một đỉnh, với điều kiện là mỗi đỉnh có thể có tối đa k cạnh."
date: "2026-06-07T01:29:51+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "sortings"]
categories: ["algorithms"]
codeforces_contest: 404
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 237 (Div. 2)"
rating: 1800
weight: 404
solve_time_s: 373
verified: false
draft: false
---

[CF 404C - Khôi phục đồ thị](https://codeforces.com/problemset/problem/404/C) 

**Đánh giá:** 1800 
**Thẻ:** dfs và tương tự, đồ thị, sắp xếp 
**Thời gian giải:** 6 phút 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng lại một đồ thị liên thông vô hướng từ danh sách các khoảng cách ngắn nhất từ một đỉnh, với điều kiện là mỗi đỉnh có thể có tối đa _k_ cạnh. Đầu vào cho số đỉnh`n`, mức độ tối đa`k`và một mảng`d`chiều dài`n`Ở đâu`d[i]`biểu thị khoảng cách từ đỉnh bắt đầu được chọn đến đỉnh`i`. Biểu đồ có thể có bất kỳ cấu trúc nào miễn là nó phù hợp với các giới hạn về khoảng cách và độ, đồng thời không chứa các vòng tự lặp hoặc nhiều cạnh. 

Các hạn chế chính là:`n`có thể lên tới 10^5 và mỗi đỉnh có nhiều nhất là bậc`k`. Điều này có nghĩa là mọi giải pháp đều phải gần như tuyến tính hoặc O(n log n). Các thuật toán cố gắng kiểm tra tất cả các kết hợp cạnh có thể có sẽ yêu cầu các thao tác O(n^2), quá chậm. Bộ nhớ đủ để lưu trữ danh sách kề hoặc các cấu trúc tương tự, vì 10^5 đỉnh có tối đa 10^5 cạnh được chấp nhận trong giới hạn 256 MB. 

Các trường hợp cạnh không rõ ràng bao gồm: một biểu đồ trong đó tất cả các đỉnh có khoảng cách bằng 0 ngoại trừ điểm bắt đầu, điều này không đáng kể; một biểu đồ trong đó thiếu một số giá trị khoảng cách, khiến việc xây dựng không thể thực hiện được; và những trường hợp`k`quá nhỏ để kết nối các đỉnh ở cùng một khoảng cách, dẫn đến đồ thị bị ngắt kết nối. Ví dụ, nếu`n = 3, k = 1`Và`d = [0, 1, 2]`, chúng ta không thể xây dựng một biểu đồ hợp lệ vì đỉnh ở khoảng cách 2 không thể kết nối với nhiều hơn một đỉnh và nó không có lựa chọn nào khác để đạt đến khoảng cách 1 mà không vượt quá mức. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ cố gắng kết nối các đỉnh một cách tham lam dựa trên khoảng cách: đối với mỗi đỉnh, hãy cố gắng kết nối nó với bất kỳ đỉnh nào có khoảng cách ít hơn một cho đến khi tất cả các khoảng cách đều được thỏa mãn. Điều này đúng về nguyên tắc nhưng chậm: với mỗi đỉnh, chúng ta có thể lặp lại tới`n`các ứng cử viên, cho độ phức tạp O(n^2), điều này không khả thi với n = 10^5. 

Cái nhìn sâu sắc quan trọng là tận dụng các mức khoảng cách. Các đỉnh có thể được nhóm theo khoảng cách từ đầu. Tất cả các đỉnh ở khoảng cách`d`phải được kết nối với một số đỉnh ở khoảng cách`d-1`. Trong một cấp độ, các đỉnh không kết nối với nhau (vì điều đó sẽ tạo ra lối tắt, vi phạm khoảng cách). Điều này cho phép chúng tôi xây dựng cấu trúc cây giống BFS theo cấp độ. Hạn chế về mức độ`k`được xử lý bằng cách theo dõi xem mỗi đỉnh đã sử dụng bao nhiêu cạnh và đảm bảo rằng không có đỉnh nào vượt quá`k`. 

Brute-force hoạt động vì nó xem xét mọi kết nối có thể có, nhưng không thành công khi n lớn. Cách tiếp cận BFS dựa trên cấp độ hoạt động vì khoảng cách áp đặt một hệ thống phân cấp chặt chẽ: một đỉnh ở khoảng cách`d`chỉ có thể được kết nối với các đỉnh ở khoảng cách`d-1`(hoặc`d+1`nếu nhìn về phía sau), và điều này ngăn cản các chu kỳ mâu thuẫn với khoảng cách. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n^2) | Quá chậm | 
| Cấp độ BFS / Nhóm khoảng cách | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định đỉnh có khoảng cách 0. Đây là đỉnh bắt đầu, gốc của cây BFS của chúng ta. Nếu không có đỉnh nào có khoảng cách 0 hoặc bội số, xuất -1 vì đầu vào không nhất quán. 
2. Nhóm tất cả các đỉnh theo khoảng cách từ điểm bắt đầu. Sử dụng từ điển hoặc danh sách các danh sách`level[d]`chứa tất cả các đỉnh ở khoảng cách`d`. Nếu bất kỳ mức nào trống trong khoảng từ 0 đến khoảng cách tối đa, xuất -1 vì khoảng cách có nghĩa là không thể đạt được một số khoảng cách. 
3. Khởi tạo bộ đếm độ cho mỗi đỉnh, bắt đầu từ 0. Thao tác này sẽ theo dõi xem mỗi đỉnh đã có bao nhiêu cạnh. 
4. Kết nối các đỉnh theo cấp độ. Đối với khoảng cách`d = 1`đến max_distance, cho mỗi đỉnh`v`ở cấp độ`d`, chọn đỉnh cha`u`từ cấp độ`d-1`có năng lực mức độ còn lại. Nếu không có cha mẹ như vậy tồn tại thì không thể tạo được biểu đồ, xuất ra -1. 
5. Đối với mỗi kết nối, hãy tăng bộ đếm độ cho cả hai đỉnh. Dừng việc giao con cho cha/mẹ nếu điều đó đạt đến`k`. 
6. Thu thập tất cả các cạnh và in tổng số theo sau mỗi cạnh. Nhiều giải pháp có thể được chấp nhận vì mọi phép gán cha mẹ hợp lệ đều có tác dụng. 

Tại sao nó hoạt động: Mỗi đỉnh ở khoảng cách`d`được đảm bảo kết nối với một đỉnh ở khoảng cách`d-1`. Không có đỉnh nào vượt quá giới hạn độ vì chúng tôi kiểm tra dung lượng trước khi kết nối. Khoảng cách được giữ nguyên vì các cạnh chỉ được hình thành giữa các cấp độ liên tiếp. Khả năng kết nối được đảm bảo vì cây BFS trải rộng trên tất cả các đỉnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
d = list(map(int, input().split()))

from collections import defaultdict, deque

# Step 1: check for valid root
roots = [i for i, dist in enumerate(d) if dist == 0]
if len(roots) != 1:
    print(-1)
    sys.exit()

root = roots[0]

# Step 2: group by distance
levels = defaultdict(list)
max_dist = 0
for i, dist in enumerate(d):
    levels[dist].append(i)
    max_dist = max(max_dist, dist)

# Step 3: check for missing levels
for dist in range(max_dist + 1):
    if not levels[dist]:
        print(-1)
        sys.exit()

# Step 4: assign edges
edges = []
degree = [0] * n

for dist in range(1, max_dist + 1):
    parents = deque(levels[dist - 1])
    if not parents:
        print(-1)
        sys.exit()
    for v in levels[dist]:
        # find parent with free degree
        while parents and degree[parents[0]] >= k:
            parents.popleft()
        if not parents:
            print(-1)
            sys.exit()
        u = parents[0]
        edges.append((u + 1, v + 1))
        degree[u] += 1
        degree[v] += 1
        # keep parent in deque if it can take more
        if degree[u] >= k:
            parents.popleft()

print(len(edges))
for u, v in edges:
    print(u, v)
```Giải pháp này trước tiên tìm gốc, nhóm các đỉnh theo khoảng cách, sau đó gán các cạnh theo cấp độ. các`degree`mảng đảm bảo chúng tôi không bao giờ vượt quá`k`kết nối trên mỗi đỉnh. sử dụng`deque`cho phép cha mẹ bật lên một cách hiệu quả khi họ đạt đến khả năng của mình. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
3 2
0 1 1
```| quận | cấp độ | bằng cấp | cạnh | 
| --- | --- | --- | --- | 
| 0 | [0] | [0,0,0] | [] | 
| 1 | [1,2] | [0,0,0] | [] | 

- kết nối 1 → 0 | - | [1,1,0] | [(1,2)] | 
- kết nối 2 → 0 | - | [2,1,1] | [(1,2),(1,3)] | 

Đầu ra:```
2
1 2
1 3
```Điều này chứng tỏ rằng các đỉnh ở khoảng cách 1 kết nối với gốc. Giới hạn mức độ được tôn trọng. 

**Ví dụ tùy chỉnh** 

đầu vào:```
4 2
0 1 2 2
```| quận | cấp độ | bằng cấp | cạnh | 
| --- | --- | --- | --- | 
| 0 | [0] | [0,0,0,0] | [] | 
| 1 | [1] | [0,0,0,0] | [] | 

- kết nối 1 → 0 | - | [1,1,0,0] | [(1,2)] | 

| 2 | [2,3] | [1,1,0,0] | [] | 
- kết nối 2 → 1 | - | [1,2,1,0] | [(1,2),(2,3)] | 
- kết nối 3 → 1 | - | [1,2,1,1] | [(1,2),(2,3),(2,4)] | 

Đầu ra:```
3
1 2
2 3
2 4
```Cho thấy các đỉnh ở khoảng cách 2 kết nối với bất kỳ đỉnh cha mẹ nào có sẵn ở khoảng cách 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đỉnh được xử lý một lần và các cạnh được hình thành trong O(1) bằng cách sử dụng deque. | 
| Không gian | O(n) | Việc lưu trữ các mức, độ và cạnh yêu cầu bộ nhớ tuyến tính. | 

Điều này phù hợp tốt trong các hạn chế`n <= 10^5`Và`m <= 10^6`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    exec(open("solution.py").read())
    return out.getvalue().strip()

# provided sample
assert run("3 2\n0 1 1\n") == "2\n1 2\n1 3"

# custom cases
assert run("4 2\n0 1 2 2\n") == "3\n1 2\n2 3\n2 4", "level 2 vertices connect correctly"
assert run("3 1\n0 1 2\n") == "-1", "degree too small to connect vertex at distance 2"
assert run("1 1\n0\n") == "0
```
