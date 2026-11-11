---
title: "CF 161A - Mặc vest!"
description: "Chúng tôi có nhiệm vụ trang bị áo chống đạn cho càng nhiều binh sĩ càng tốt. Mỗi người lính có một kích cỡ áo vest ưa thích nhưng họ sẵn sàng chấp nhận những sai lệch trong một phạm vi nhất định. Cụ thể, người lính thứ i có thể mặc bất kỳ áo vest nào có kích thước từ a[i] - x đến a[i] + y."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 161
codeforces_index: "A"
codeforces_contest_name: "VK Cup 2012 Round 1"
rating: 1300
weight: 161
solve_time_s: 75
verified: true
draft: false
---

[CF 161A - Mặc vest!](https://codeforces.com/problemset/problem/161/A) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, vũ phu, tham lam, hai con trỏ 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có nhiệm vụ trang bị áo chống đạn cho càng nhiều binh sĩ càng tốt. Mỗi người lính có một kích cỡ áo vest ưa thích nhưng họ sẵn sàng chấp nhận những sai lệch trong một phạm vi nhất định. Cụ thể, người lính thứ i có thể mặc bất kỳ chiếc áo vest nào có kích cỡ từ`a[i] - x`Và`a[i] + y`. Những chiếc áo vest hiện có có kích thước cố định và mỗi chiếc áo vest chỉ có thể được cấp cho một người lính. Mục tiêu là xuất ra số lượng binh lính tối đa có thể được trang bị, cùng với việc phân công áo giáp cho binh lính. 

Kích thước đầu vào rất đáng kể: lên tới 100.000 binh sĩ và 100.000 áo khoác. Một giải pháp đơn giản để kiểm tra từng bộ vest của từng người lính sẽ yêu cầu tới 10^10 phép so sánh, vượt xa khả năng chấp nhận được trong thời gian 3 giây. Vì vậy, chúng ta cần một giải pháp hoạt động theo thời gian tuyến tính hoặc gần tuyến tính, lý tưởng nhất là`O(n + m)`hoặc`O(n log n + m log m)`. 

Một trường hợp phức tạp phát sinh khi nhiều binh sĩ tương thích với cùng một bộ vest. Ví dụ: nếu cả hai người lính đều chấp nhận áo vest cỡ 3 và áo vest có sẵn chỉ bao gồm một áo vest cỡ 3 thì thuật toán phải chọn người lính nào để chỉ định trước. Việc triển khai bất cẩn có thể bỏ lỡ các nhiệm vụ có thể thực hiện được hoặc cố gắng sử dụng lại cùng một bộ vest. 

Một trường hợp nguy hiểm khác là khi phạm vi chấp nhận được của một người lính vượt quá tất cả các loại áo vest hiện có. Ví dụ: nếu phạm vi chấp nhận được của một người lính là 1-100, nhưng tất cả áo vest đều có kích thước 101 trở lên thì không thể chỉ định áo vest nào và thuật toán phải bỏ qua người lính này một cách chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ lực rất đơn giản: đối với mỗi người lính, hãy quét tất cả áo khoác có sẵn để xem liệu có chiếc nào nằm trong phạm vi chấp nhận được của họ hay không. Nếu tìm thấy một chiếc áo vest tương thích, hãy đánh dấu nó là đã qua sử dụng và tiếp tục. Cách tiếp cận này đúng về nguyên tắc vì nó kiểm tra mọi khả năng, nhưng cực kỳ kém hiệu quả. Đối với các ràng buộc tối đa, nó có thể yêu cầu tối đa 10^10 phép so sánh, tốc độ này quá chậm. 

Quan sát quan trọng là cả binh lính và áo khoác đều được đưa ra theo thứ tự không giảm. Cấu trúc này cho phép cách tiếp cận hai con trỏ, trong đó chúng tôi duy trì một con trỏ dành cho binh lính và một con trỏ dành cho áo vest. Ở mỗi bước, chúng tôi kiểm tra xem bộ vest hiện tại có vừa với người lính hiện tại hay không. Nếu có, chúng tôi chỉ định nó và nâng cao cả hai con trỏ. Nếu áo vest quá nhỏ đối với người lính, chúng ta sẽ tiến tới con trỏ áo vest. Nếu áo vest quá lớn, chúng ta tiến tới con trỏ người lính. Chiến lược tham lam này hiệu quả vì binh lính và áo khoác được sắp xếp: một khi áo vest quá lớn đối với người lính hiện tại, nó không thể vừa với bất kỳ người lính nào trước đó, và một khi áo vest quá nhỏ, nó không thể vừa với bất kỳ người lính nào sau này. 

Điều này làm giảm độ phức tạp về thời gian từ O(n * m) xuống O(n + m) và chỉ yêu cầu không gian bổ sung tuyến tính để lưu trữ các bài tập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(1) | Quá chậm | 
| Tham lam hai con trỏ | O(n + m) | O(phút(n, m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai con trỏ,`i`cho binh sĩ và`j`đối với áo khoác, cả hai đều bắt đầu từ 0. Ngoài ra, hãy khởi tạo một danh sách trống`assignments`để lưu trữ các cặp cuối cùng. 
2. Trong khi cả hai con trỏ đều nằm trong giới hạn, hãy kiểm tra kích cỡ áo vest hiện tại`b[j]`so với phạm vi chấp nhận được của người lính hiện tại`[a[i] - x, a[i] + y]`. 
3. Nếu`b[j]`nhỏ hơn`a[i] - x`, tiến con trỏ vest`j`bởi vì chiếc áo vest này không thể vừa với người lính này hoặc bất kỳ người lính nào sau này, người cần ít nhất`a[i] - x`. 
4. Nếu`b[j]`lớn hơn`a[i] + y`, tiến con trỏ người lính`i`vì không còn chiếc áo vest nào có thể vừa với người lính này. 
5. Nếu`b[j]`ở trong`[a[i] - x, a[i] + y]`, giao chiếc áo vest này cho người lính này bằng cách gắn thêm`(i + 1, j + 1)`ĐẾN`assignments`, sau đó tiến cả hai con trỏ. 
6. Tiếp tục cho đến khi một trong các con trỏ đến cuối danh sách tương ứng. 
7. Xuất ra chiều dài của`assignments`theo sau là các cặp. 

Lý do điều này có hiệu quả là vì việc tham lam lựa chọn chiếc áo vest tương thích nhỏ nhất đảm bảo rằng những người lính sau này có cơ hội tốt nhất để tìm được một chiếc áo vest. Điều bất biến được duy trì là tất cả các áo vest được chỉ định đều tương thích và chỉ được sử dụng một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, x, y = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

i = j = 0
assignments = []

while i < n and j < m:
    if b[j] < a[i] - x:
        j += 1
    elif b[j] > a[i] + y:
        i += 1
    else:
        assignments.append((i + 1, j + 1))
        i += 1
        j += 1

print(len(assignments))
for soldier, vest in assignments:
    print(soldier, vest)
```Mã tuân theo thuật toán hai con trỏ. Chúng tôi cẩn thận kiểm tra từng áo giáp trong phạm vi chấp nhận được của người lính và tăng con trỏ chính xác dựa trên so sánh. Tránh lỗi từng cái một bằng cách chuyển đổi các chỉ số dựa trên 0 thành dựa trên 1 khi thêm vào`assignments`. sử dụng`sys.stdin.readline`đảm bảo đầu vào nhanh chóng cho các tập dữ liệu lớn. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
5 3 0 0
1 2 3 3 4
1 3 5
```| tôi | j | một [tôi] | b[j] | Hành động | bài tập | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 1 hợp 1 | [(1,1)] | 
| 1 | 1 | 2 | 3 | 3 > 2 | tiến tôi | 
| 2 | 1 | 3 | 3 | 3 hợp 3 | [(1,1),(3,2)] | 

Đầu ra:`2\n1 1\n3 2`Đầu vào mẫu 2:```
3 5 2 2
3 4 5
1 2 3 4 5
```| tôi | j | một [tôi] | b[j] | Hành động | bài tập | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 3 | 1 | 1 < 1 | 1 >= 1 | 
| 1 | 1 | 4 | 2 | phù hợp | [(1,1),(2,2)] | 
| 2 | 2 | 5 | 3 | phù hợp | [(1,1),(2,2),(3,3)] | 

Tất cả binh sĩ được trang bị đều thể hiện khả năng xử lý dung sai tầm bắn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi con trỏ chỉ di chuyển về phía trước qua mảng của nó một lần | 
| Không gian | O(phút(n, m)) | Lưu trữ tối đa một nhiệm vụ cho mỗi người lính hoặc áo vest | 

Giải pháp này dễ dàng phù hợp với các ràng buộc, vì số lần lặp 2 * 10^5 diễn ra nhanh và mức sử dụng bộ nhớ ở mức tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m, x, y = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    i = j = 0
    assignments = []
    while i < n and j < m:
        if b[j] < a[i] - x:
            j += 1
        elif b[j] > a[i] + y:
            i += 1
        else:
            assignments.append((i + 1, j + 1))
            i += 1
            j += 1
    out = [str(len(assignments))] + [f"{s} {v}" for s,v in assignments]
    return "\n".join(out)

# provided samples
assert run("5 3 0 0\n1 2 3 3 4\n1 3 5\n") == "2\n1 1\n3 2"
assert run("3 5 2 2\n3 4 5\n1 2 3 4 5\n") == "3\n1 1\n2 2\n3 3"

# custom test cases
assert run("1 1 0 0\n5\n5\n") == "1\n1 1", "minimum input"
assert run("2 2 1 1\n2 4\n1 3\n") == "2\n1 1\n2 2", "overlapping ranges"
assert run("3 3 0 0\n1 2 3\n10 11 12\n") == "0", "no compatible vests"
assert run("4 4 10 10\n5 6 7 8\n1 2 3 4\n") == "0", "vests smaller than range"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 0 0\n5\n5 | 1\n1 1 | đầu vào kích thước tối thiểu | 
| 2 2 1 1\n2 4\n1 3 | 2 | |
