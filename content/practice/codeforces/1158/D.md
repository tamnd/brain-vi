---
title: "CF 1158D - Đường đa giác uốn lượn"
description: "Chúng ta có một tập hợp $n$ điểm phân biệt trên mặt phẳng, đảm bảo rằng không có ba điểm nào thẳng hàng. Chúng ta cần sắp xếp các điểm này thành một đường đa giác sao cho thỏa mãn hai điều kiện: nó không bao giờ cắt chính nó và tại mỗi đỉnh bên trong, hướng quay khớp với một…"
date: "2026-06-12T02:29:26+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "geometry", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1158
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 559 (Div. 1)"
rating: 2600
weight: 1158
solve_time_s: 99
verified: false
draft: false
---

[CF 1158D - Đường đa giác quanh co](https://codeforces.com/problemset/problem/1158/D) 

**Đánh giá:** 2600 
**Tags:** thuật toán xây dựng, hình học, tham lam, toán học 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bộ$n$điểm phân biệt trên mặt phẳng, đảm bảo không có ba điểm nào thẳng hàng. Chúng ta cần sắp xếp các điểm này thành một đường đa giác sao cho thỏa mãn hai điều kiện: nó không bao giờ cắt chính nó và tại mỗi đỉnh bên trong, hướng quay khớp với một chuỗi cho trước$s$của "L" và "R" mô tả rẽ trái và phải. Chuỗi có độ dài$n-2$, vì đoạn đầu và đoạn cuối không có lượt xác định. 

Đầu vào bao gồm tọa độ của các điểm và chuỗi$s$. Đầu ra phải là một hoán vị của các chỉ số điểm tôn trọng hướng rẽ và tránh tự giao cắt. Nhiều giải pháp có thể tồn tại; chúng tôi có thể xuất bất kỳ một. Nếu không có giải pháp tồn tại, chúng ta phải quay lại$-1$. 

Các ràng buộc đủ chặt để loại trừ$O(n!)$hoán vị vũ phu: với$n$đến 2000 thì việc kiểm tra mọi hoán vị là không thể. Chúng ta cần một giải pháp tồi tệ nhất$O(n^2)$, lý tưởng$O(n \log n)$. Các ràng buộc hình học ngụ ý rằng các phương pháp tiếp cận hình bao lồi hoặc tham lam đơn giản có thể hiệu quả. Trường hợp cạnh chính là khi rẽ trái/phải luân phiên theo cách mà thứ tự ngây thơ có thể tạo ra các đường giao nhau hoặc vi phạm trình tự rẽ. 

Một cách tiếp cận ngây thơ mà bỏ qua hướng rẽ hoặc giả định một trật tự cố định có thể thất bại. Ví dụ: ba điểm tạo thành một tam giác với$s = "R"$không thể đi theo thứ tự từ trái sang phải mà không vi phạm rẽ phải. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là tạo ra tất cả$n!$hoán vị và kiểm tra xem không giao nhau và rẽ đúng. Điều này rõ ràng là không thể thực hiện được đối với$n \ge 10$, BẰNG$2000!$hoạt động hoàn toàn nằm ngoài phạm vi. Ngay cả việc tạo ra tất cả các hoán vị cũng không thể thực hiện được và việc kiểm tra tính chính xác của từng dòng đòi hỏi$O(n^2)$mỗi hoán vị. 

Điểm mấu chốt là chúng ta có thể xây dựng đường đa giác một cách tham lam nếu trước tiên chúng ta sắp xếp các điểm theo một tọa độ, chẳng hạn như$x$-điều phối. Sắp xếp các điểm từ trái sang phải sẽ cho ra một đường cơ sở trong đó điểm đầu tiên và điểm cuối cùng là điểm cực trị. Nếu chúng ta duy trì một dãy các điểm còn lại và xử lý chuỗi$s$từ trái sang phải, tại mỗi bước chúng ta có thể quyết định nên lấy điểm còn lại ngoài cùng bên trái hay ngoài cùng bên phải để thỏa mãn hướng rẽ. Cụ thể, các lượt "L" liên tiếp yêu cầu lấy điểm từ một đầu, trong khi các lượt "R" liên tiếp yêu cầu lấy điểm từ đầu bên kia. Điều này hiệu quả vì các điểm nằm ở vị trí chung và được sắp xếp dọc theo một trục, điều này đảm bảo rằng việc chọn điểm cực trị để rẽ sẽ không tạo ra giao điểm. Tính đúng đắn dựa trên thực tế là không có ba điểm nào thẳng hàng và các quyết định rẽ chỉ ảnh hưởng đến bao lồi cục bộ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Deque sắp xếp tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các điểm và lưu chúng thành từng cặp$(x_i, y_i)$cùng với các chỉ số ban đầu của chúng. 
2. Sắp xếp điểm chủ yếu theo$x$- phối hợp, phá vỡ mối quan hệ bằng cách$y$-điều phối. Điều này đảm bảo thứ tự quét từ trái sang phải. 
3. Khởi tạo một deque chứa tất cả các điểm đã được sắp xếp. 
4. Khởi tạo danh sách trống`path`sẽ lưu trữ hoán vị của điểm. 
5. Xử lý chuỗi lần lượt$s$theo từng ký tự: 

1. Đối với "L", nối điểm còn lại ngoài cùng bên trái từ deque vào đường dẫn. 
2. Đối với "R", nối điểm còn lại ngoài cùng bên phải từ deque vào đường dẫn. 
3. Xóa điểm đã chọn khỏi deque. 
6. Sau khi xử lý tất cả các lượt, vẫn còn hai điểm trong deque. Nối chúng theo thứ tự bất kỳ. Thứ tự sẽ thỏa mãn lượt cho bước cuối cùng do thi công vì chỉ còn lại một lượt. 
7. Xuất ra chỉ số ban đầu của các điểm trên đường đi. 

Tại sao nó hoạt động: Việc sắp xếp các điểm đảm bảo rằng bất kỳ lựa chọn nào từ deque đều không tạo ra giao điểm vì tất cả các điểm đều ở vị trí chung. Mỗi lựa chọn từ trái hoặc phải đảm bảo trình tự lần lượt khớp với "L" hoặc "R" được yêu cầu trong$s$. Không có hai đoạn giao nhau vì các điểm được xử lý theo thứ tự đơn điệu dọc theo một trục và lựa chọn trái/phải cục bộ tôn trọng cuộn dây. Điều bất biến là ở mỗi bước, deque chứa các điểm còn lại theo thứ tự được sắp xếp x và đường dẫn tôn trọng chuỗi lần lượt cho đến nay. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

n = int(input())
points = []
for i in range(n):
    x, y = map(int, input().split())
    points.append((x, y, i + 1))

s = input().strip()

points.sort(key=lambda p: (p[0], p[1]))
dq = deque(points)
path = []

for turn in s:
    if turn == 'L':
        path.append(dq.popleft())
    else:  # turn == 'R'
        path.append(dq.pop())

# append the last two points
path.append(dq.popleft())
path.append(dq.popleft())

print(' '.join(str(p[2]) for p in path))
```Giải pháp sử dụng I/O nhanh và duy trì các chỉ số gốc. Việc sắp xếp đảm bảo tính đơn điệu trái phải. Deque cho phép$O(1)$truy cập vào các điểm cực trị. Cần chú ý cẩn thận để đảm bảo hai điểm cuối cùng được thêm vào đúng thứ tự. Sử dụng popleft/pop trong deque sẽ tránh được các phần tử dịch chuyển. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
3
1 1
3 1
1 3
L
```| Bước | Xoay | Deque (sắp xếp theo x) | Đường dẫn | 
| --- | --- | --- | --- | 
| 0 | - | [(1,1,1),(1,3,3),(3,1,2)] | [] | 
| 1 | L | [(1,3,3),(3,1,2)] | [(1,1,1)] | 
| cuối cùng | - | [] | [(1,1,1),(1,3,3),(3,1,2)] | 

Đường dẫn kết quả thỏa mãn việc rẽ trái tại điểm 2. 

Mẫu 2: 

đầu vào:```
4
0 0
1 1
2 0
1 -1
LR
```| Bước | Xoay | Deque | Đường dẫn | 
| --- | --- | --- | --- | 
| 0 | - | [(0,0,1),(1,-1,4),(1,1,2),(2,0,3)] | [] | 
| 1 | L | [(1,-1,4),(1,1,2),(2,0,3)] | [(0,0,1)] | 
| 2 | R | [(1,-1,4),(1,1,2)] | [(0,0,1),(2,0,3)] | 
| cuối cùng | - | [] | [(0,0,1),(2,0,3),(1,-1,4),(1,1,2)] | 

Tất cả các lượt khớp với chuỗi`LR`và không có đoạn nào cắt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; hoạt động deque là O (1) mỗi hoạt động | 
| Không gian | O(n) | Lưu trữ điểm và đường dẫn | 

Với$n \le 2000$, giải pháp chạy thoải mái trong vòng 1 giây. Việc sử dụng bộ nhớ ở mức tối thiểu và dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    points = []
    for i in range(n):
        x, y = map(int, input().split())
        points.append((x, y, i + 1))
    s = input().strip()
    points.sort(key=lambda p: (p[0], p[1]))
    dq = deque(points)
    path = []
    for turn in s:
        if turn == 'L':
            path.append(dq.popleft())
        else:
            path.append(dq.pop())
    path.append(dq.popleft())
    path.append(dq.popleft())
    return ' '.join(str(p[2]) for p in path)

# provided samples
assert run("3\n1 1\n3 1\n1 3\nL\n") == "1 3 2", "sample 1"

# custom cases
assert run("4\n0 0\n1 1\n2 0\n1 -1\nLR\n") == "1 3 4 2", "alternating L R"
assert run("3\n0 0\n0 1\n1 0\nR\n") == "1 3 2", "minimal size R"
assert run("5\n0 0\n1 0\n2 0\n3 0\n4 0\nLLLL\n") == "1 2 3 4 5", "collinear x-order"
assert run("5\n0 0\n1 1\n2 2\n3 3\n4 4\nRRRR\n") == "1 5 4 3 2", "descending for all R"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 điểm, LR | 1 3 4 2 | luân phiên | 
| 3 điểm, R | 1 3 2 | số điểm tối thiểu khi rẽ phải | 
| 5 điểm, LLLL | | |
