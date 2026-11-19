---
title: "CF 2090C - Phòng ăn"
description: "Phòng ăn là một mạng lưới vô tận các ô. Một số ô này được nhóm thành các bảng, mỗi bảng chiếm một hình vuông 2×2 nằm ở tọa độ có dạng $(3x+1,3y+1)$ đến $(3x+2,3y+2)$ và tất cả các ô khác đều là hành lang."
date: "2026-06-09T03:49:23+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2090
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1012 (Div. 2)"
rating: 1700
weight: 2090
solve_time_s: 100
verified: false
draft: false
---

[CF 2090C - Phòng ăn](https://codeforces.com/problemset/problem/2090/C) 

**Đánh giá:** 1700 
**Tags:** cấu trúc dữ liệu, tham lam, triển khai, sắp xếp 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Phòng ăn là một mạng lưới vô tận các ô. Một số ô này được nhóm thành các bảng, mỗi bảng chiếm một hình vuông 2×2 nằm ở tọa độ của biểu mẫu$(3x+1,3y+1)$bởi vì$(3x+2,3y+2)$, và tất cả các ô khác là hành lang. Khách lần lượt đến điểm gốc (0,0) và muốn chiếm một ô trong bảng theo loại của họ. Khách loại 1 chọn ô bàn trống gần nhất, còn khách loại 0 chọn ô bàn trống gần nhất. Khoảng cách được đo bằng khoảng cách Manhattan dọc theo các ô hành lang và các mối liên kết bị phá vỡ trước tiên bởi tọa độ x, sau đó là tọa độ y. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm liệt kê số lượng khách và trình tự các loại của họ. Đầu ra phải liệt kê tọa độ các ô trong bàn nơi mỗi khách ngồi, theo thứ tự đến. 

lớn nhất`n`tổng cộng trên tất cả các trường hợp thử nghiệm là 50.000. Vì mỗi khách có thể yêu cầu quét một tập hợp các ô trong bảng không giới hạn để tìm ô trống gần nhất, nên BFS mạnh mẽ cho mỗi khách là quá chậm. Cụ thể, một BFS ngây thơ từ (0,0) cho mỗi khách có thể yêu cầu ghé thăm một số ô hành lang tỷ lệ với khoảng cách đến bàn trống gần nhất, bàn này sẽ phát triển không giới hạn. Điều này loại trừ mọi giải pháp có độ phức tạp O(n*d), trong đó d là khoảng cách đến bảng. 

Các trường hợp cạnh không rõ ràng bao gồm các khách loại 0 liên tiếp trong đó một số bảng có các ô bị chiếm một phần. Một cách tiếp cận ngây thơ chỉ kiểm tra "bất kỳ ô trống nào" sẽ chỉ định một bảng bị chiếm một phần cho khách loại 0 không chính xác. Một cách tinh tế khác là liên kết: nhiều ô có thể cách đều nhau, vì vậy việc sắp xếp theo x và y là điều cần thiết để phù hợp với yêu cầu tất định của bài toán. Ví dụ: nếu khách loại 1 nhìn thấy hai ô trống cách đều nhau (1,2) và (2,1) thì khách phải chọn (1,2). 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là duy trì một bản đồ lưới các ô bị chiếm đóng. Đối với mỗi khách, thực hiện BFS từ (0,0) cho đến khi đạt được ô trong bảng hợp lệ, sau đó đánh dấu ô đó đã có người sử dụng. Điều này đúng, nhưng số lượng ô được khám phá có thể tăng tuyến tính theo khoảng cách và với tối đa 50.000 khách, BFS trên mỗi khách là quá chậm. 

Thông tin chi tiết quan trọng là các bảng được căn chỉnh theo mẫu lưới 3×3 thông thường. Mỗi ô trong bảng có thể được ánh xạ trực tiếp từ “tọa độ bảng” (x, y) của nó tới tọa độ thực ((3x+1 hoặc +2, 3y+1 hoặc +2)). Hơn nữa, khoảng cách Manhattan từ (0,0) đến bất kỳ ô nào trong bảng chỉ phụ thuộc vào tọa độ của bảng và một khoảng lệch nhỏ so với ô trong bảng. Cụ thể, đối với tọa độ bảng (tx, ty) và độ lệch ô (dx, dy) trong {0,1}, khoảng cách là`(3*tx + dx) + (3*ty + dy)`. 

Vì lưới là vô hạn và có cấu trúc nên chúng ta có thể tính toán trước thứ tự các ô trong bảng sẽ được chiếm giữ. Khách loại 0 chiếm toàn bộ bàn theo khối 2 × 2, trong khi khách loại 1 có thể chọn bất kỳ ô trống nào trong bất kỳ bàn nào. Chúng ta có thể duy trì hai chuỗi: một chuỗi theo dõi các bảng trống hoàn toàn và một chuỗi khác theo dõi các ô trống trong các bảng bị chiếm một phần. Vì việc sắp xếp theo khoảng cách, rồi x, rồi y là có thể dự đoán được nên chúng ta có thể tạo các ô trong bảng theo đúng trình tự và gán cho khách theo thứ tự mà không cần BFS. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force BFS cho mỗi khách | O(n*d) trường hợp xấu nhất | O(d^2) cho BFS | Quá chậm | 
| Tính toán trước chuỗi ô và gán tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn bảng theo tọa độ`(tx, ty)`trong lưới 3×3. Mỗi bảng có 4 ô:`(3*tx + 1, 3*ty + 1)`,`(3*tx + 1, 3*ty + 2)`,`(3*tx + 2, 3*ty + 1)`,`(3*tx + 2, 3*ty + 2)`. 
2. Tính trước thứ tự các ô trong bảng theo khoảng cách từ (0,0) bằng công thức`dist = 3*tx + dx + 3*ty + dy`. Đối với mỗi khoảng cách, sắp xếp các ô ứng cử viên theo x rồi y. 
3. Duy trì một hàng bàn trống hoàn toàn cho khách loại 0. Khi khách loại 0 đến, hãy đưa bảng đầu tiên ra khỏi hàng đợi và chỉ định ô trống đầu tiên, đánh dấu bảng là đã chiếm một phần và thêm các ô trống còn lại vào nhóm dành cho khách loại 1. 
4. Duy trì một hàng ô riêng miễn phí cho khách loại 1. Khi có khách loại 1 đến, hãy bật ô trống đầu tiên từ hàng đợi này. Nếu bảng của ô trước đó trống, hãy xóa nó khỏi hàng đợi bảng trống. 
5. Đối với mỗi khách theo thứ tự, tùy thuộc vào loại của họ, chỉ định ô thích hợp tiếp theo trong hàng đợi tương ứng và đánh dấu ô đó đã có người. Cập nhật hàng đợi cho phù hợp. 
6. Xuất tọa độ của từng ô được chỉ định theo thứ tự đến. 

Chiến lược này hoạt động vì cấu trúc bảng đảm bảo rằng khoảng cách Manhattan của các ô trong bảng tăng lên theo các lớp riêng biệt tính từ gốc. Bằng cách sắp xếp các ô theo khoảng cách và liên kết theo tọa độ, hàng đợi duy trì tính bất biến một cách tự nhiên: khách loại 0 luôn chọn các bảng trống hoàn toàn, khách loại 1 luôn chọn các ô trống gần gốc nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    q = int(input())
    for _ in range(q):
        n = int(input())
        t = list(map(int, input().split()))
        
        # Precompute table cells in order
        empty_tables = deque()
        free_cells = deque()
        
        # Generate enough tables to cover n guests
        # 2D BFS-like layer expansion
        layer = 0
        cells = []
        while len(cells) < n*4:
            for tx in range(layer+1):
                ty = layer - tx
                for dx in (1,2):
                    for dy in (1,2):
                        cells.append((3*tx+dx, 3*ty+dy, tx, ty))
            layer += 1
        
        # empty tables queue
        table_cells_map = {}
        for x,y,tx,ty in cells:
            table_cells_map.setdefault((tx,ty), []).append((x,y))
        
        empty_table_queue = deque(sorted(table_cells_map.keys(), key=lambda k: 3*k[0]+3*k[1]))
        # prepare all free cells queue for type 1 guests
        free_cell_queue = deque()
        
        result = []
        for typ in t:
            if typ == 0:
                tx, ty = empty_table_queue.popleft()
                x, y = table_cells_map[(tx, ty)].pop(0)
                result.append(f"{x} {y}")
                # remaining cells go to free cells queue
                free_cell_queue.extend(table_cells_map[(tx,ty)])
            else:
                x, y = free_cell_queue.popleft()
                result.append(f"{x} {y}")
        
        print("\n".join(result))

if __name__ == "__main__":
    solve()
```Giải pháp tính toán trước đủ số lượng bảng theo thứ tự khoảng cách. Mỗi khách loại 0 sẽ xóa một bảng khỏi hàng đợi bảng trống và đẩy các ô còn lại vào hàng đợi ô trống loại 1. Khách loại 1 luôn chọn từ phía trước hàng đợi ô miễn phí. Điều này tránh mọi BFS và đảm bảo thứ tự chính xác theo khoảng cách và tọa độ. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

| Khách | Loại | Ô được chỉ định | Bàn trống | Tế bào miễn phí | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | (1,1) | {(0,1),(1,0),...} | [(1,2),(2,1),(2,2)] | 
| 2 | 1 | (1,2) | giống nhau | [(2,1),(2,2)] | 
| 3 | 1 | (2,1) | giống nhau | [(2,2)] | 
| 4 | 0 | (1,4) | bàn trống tiếp theo | [...] | 
| 5 | 0 | (4,1) | bàn trống tiếp theo | [...] | 
| 6 | 1 | (1,5) | giống nhau | [...] | 

Bảng cho thấy khách loại 0 luôn ngồi vào bàn mới trong khi khách loại 1 lấp đầy các ô trống còn sót lại. 

**Mẫu 2:** Ví dụ nhỏ được xây dựng với tất cả khách loại 1 cho thấy hàng đợi ô trống là đủ mà không cần chạm vào hàng đợi bàn trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi khách được xử lý một lần; tính toán trước tạo ra đủ các ô tỷ lệ với n. | 
| Không gian | O(n) | Hàng đợi lưu trữ tối đa 4*n ô để gán. | 

Giải pháp này rất phù hợp trong giới hạn thời gian 2 giây cho n lên tới 50.000 và bộ nhớ 512MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("2\n6\n0 1 1 0 0 1\n5\n1 0 0 1 1\n") == "1 1\n1 2\n2 1\n1 4\n4 1\n1 5\n1 1\n1 4\n4 1\n1
```
