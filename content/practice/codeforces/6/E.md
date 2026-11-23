---
title: "CF 6E - Triển lãm"
description: "Berlbury đưa cho chúng ta một dãy sách, mỗi cuốn đều có độ cao đã biết, được sắp xếp theo thứ tự thời gian. Thư viện muốn tổ chức một cuộc trưng bày bằng cách chọn những cuốn sách liên tiếp sao cho sự khác biệt giữa cuốn sách cao nhất và ngắn nhất trong bộ sưu tập không vượt quá một giá trị nhất định…"
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dsu", "trees", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 6
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 6 (Div. 2 Only)"
rating: 1900
weight: 6
solve_time_s: 58
verified: true
draft: false
---
[CF 6E - Triển lãm](https://codeforces.com/problemset/problem/6/E) 

**Xếp hạng:** 1900 
**Thẻ:** tìm kiếm nhị phân, cấu trúc dữ liệu, dsu, cây, hai con trỏ 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Berlbury đưa cho chúng ta một dãy sách, mỗi cuốn đều có độ cao đã biết, được sắp xếp theo thứ tự thời gian. Thư viện muốn tổ chức một cuộc trình bày bằng cách chọn những cuốn sách liên tiếp sao cho sự khác biệt giữa cuốn sách cao nhất và ngắn nhất trong bộ sưu tập không vượt quá một giới hạn nhất định,`k`. Nhiệm vụ của chúng tôi gồm có hai phần: thứ nhất, tối đa hóa số lượng sách trong một lựa chọn như vậy và thứ hai, liệt kê tất cả các khoảng liên tiếp đạt được số lượng tối đa này trong khi vẫn đáp ứng giới hạn về chiều cao. 

Đầu vào cung cấp`n`, số lượng sách`k`, chênh lệch chiều cao tối đa được phép và một mảng`h`về chiều cao của cuốn sách. Đầu ra trước tiên phải cung cấp số lượng sách tối đa`a`và số kỳ hợp lệ`b`, theo sau là`b`dòng chỉ số dựa trên 1 cho biết thời điểm bắt đầu và kết thúc của mỗi giai đoạn. 

Với`n`lên tới 10^5 và giới hạn thời gian 2 giây, bất kỳ cách tiếp cận ngây thơ nào kiểm tra tất cả các mảng con có thể sẽ quá chậm. Việc kiểm tra mạnh mẽ tất cả các khoảng thời gian có thể sẽ yêu cầu các hoạt động O(n^2), có thể đạt tới 10^10 so sánh trong trường hợp xấu nhất. Điều này vượt xa khả năng khả thi, vì vậy chúng ta phải tìm một phương pháp hiệu quả hơn. 

Một trường hợp cạnh quan trọng phát sinh khi`k`bằng 0, nghĩa là chỉ có thể chọn những cuốn sách liên tiếp có chiều cao chính xác bằng nhau. Một trường hợp khác là khi tất cả các cuốn sách đều tăng hoặc giảm chiều cao một cách nghiêm ngặt, điều này có thể khiến cuốn sách đầu tiên hoặc cuối cùng bị loại trừ trong mọi khoảng thời gian tối đa nếu không cẩn thận. Cách tiếp cận bất cẩn có thể bỏ sót các khoảng ở ranh giới hoặc tính sai độ dài tối đa. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực lặp đi lặp lại trên mọi chỉ số bắt đầu có thể`i`và với mỗi cái, kéo dài khoảng sang phải cho đến khi chênh lệch chiều cao vượt quá`k`. Đối với mỗi khoảng thời gian, chúng tôi theo dõi độ dài và so sánh nó với mức tối đa hiện tại. Phương pháp này đúng vì nó xem xét mọi dãy con liên tiếp, nhưng nó yêu cầu so sánh O(n^2) trong trường hợp xấu nhất. Với`n = 10^5`, điều này dẫn đến 10^10 thao tác, quá chậm so với giới hạn thời gian 2 giây. 

Quan sát quan trọng giúp mở ra một giải pháp hiệu quả là chúng ta chỉ quan tâm đến các mảng con liên tiếp mà chúng ta cần duy trì các giá trị tối thiểu và tối đa một cách hiệu quả. Cấu trúc này cho phép chúng ta sử dụng kỹ thuật hai con trỏ kết hợp với cấu trúc dữ liệu có khả năng truy xuất giá trị tối thiểu và tối đa trong O(1) cho cửa sổ trượt. Cụ thể, việc sử dụng hai deque để theo dõi các chỉ số về độ cao tối thiểu và tối đa cho phép chúng tôi mở rộng ranh giới bên phải trong khi vẫn giữ khoảng thời gian hợp lệ. Khi khoảng không hợp lệ, chúng ta di chuyển con trỏ trái về phía trước, duy trì bất biến rằng khoảng cách giữa hai con trỏ luôn thỏa mãn ràng buộc chênh lệch độ cao. Điều này làm giảm độ phức tạp xuống O(n), vì mỗi phần tử được đẩy và bật ra khỏi deque nhiều nhất một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Hai con trỏ + Deques | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai deques: một để theo dõi độ cao tối đa (`max_deque`) và một cho chiều cao tối thiểu (`min_deque`). Khởi tạo hai con trỏ,`l = 0`Và`r = 0`, đại diện cho khoảng thời gian hiện tại`[l, r]`. Khởi tạo các biến để lưu trữ độ dài tối đa hiện tại`max_len`và một danh sách`intervals`cho những khoảng thời gian hợp lệ. 
2. Mở rộng con trỏ bên phải`r`mỗi lần một cuốn sách Đối với mỗi cuốn sách mới`h[r]`, xóa các chỉ số ở mặt sau của`max_deque`trong khi`h[r]`lớn hơn độ cao mà chúng đại diện. Điều này đảm bảo`max_deque[0]`luôn hướng tới giá trị lớn nhất trong khoảng. Tương tự, loại bỏ các chỉ số ở mặt sau của`min_deque`trong khi`h[r]`nhỏ hơn độ cao mà chúng đại diện. 
3. Sau khi thêm`r`cho cả hai deques, hãy kiểm tra xem khoảng thời gian hiện tại`[l, r]`vi phạm ràng buộc chênh lệch chiều cao:`h[max_deque[0]] - h[min_deque[0]] > k`. Nếu có thì tăng`l`thu hẹp khoảng cách cho đến khi chênh lệch lớn nhất`k`. Xóa các chỉ mục khỏi phía trước của deque hiện nằm ngoài khoảng. 
4. Khi khoảng thời gian`[l, r]`là hợp lệ, so sánh độ dài của nó với`max_len`. Nếu nó vượt quá`max_len`, cập nhật`max_len`và thiết lập lại`intervals`list với khoảng thời gian hiện tại. Nếu nó bằng`max_len`, nối thêm khoảng hiện tại. 
5. Tăng`r`cho đến hết mảng. Sau vòng lặp, xuất ra`max_len`, số khoảng và mỗi khoảng trong lập chỉ mục dựa trên 1. 

**Tại sao nó hoạt động**: Thuật toán duy trì tính bất biến mà`[l, r]`luôn là khoảng liên tiếp tối đa kết thúc tại`r`với sự khác biệt chiều cao hợp lệ. Việc sử dụng deque đảm bảo tối thiểu và tối đa luôn được theo dõi hiệu quả, do đó, độ dài của mỗi khoảng tối đa được tính toán chính xác mà không bỏ sót bất kỳ chuỗi con hợp lệ nào. 

## Giải pháp Python```python
import sys
from collections import deque
input = sys.stdin.readline

n, k = map(int, input().split())
h = list(map(int, input().split()))

l = 0
max_len = 0
intervals = []

min_deque = deque()
max_deque = deque()

for r in range(n):
    while max_deque and h[r] > h[max_deque[-1]]:
        max_deque.pop()
    max_deque.append(r)
    
    while min_deque and h[r] < h[min_deque[-1]]:
        min_deque.pop()
    min_deque.append(r)
    
    while h[max_deque[0]] - h[min_deque[0]] > k:
        l += 1
        if max_deque[0] < l:
            max_deque.popleft()
        if min_deque[0] < l:
            min_deque.popleft()
    
    curr_len = r - l + 1
    if curr_len > max_len:
        max_len = curr_len
        intervals = [(l + 1, r + 1)]
    elif curr_len == max_len:
        intervals.append((l + 1, r + 1))

print(max_len, len(intervals))
for a, b in intervals:
    print(a, b)
```Các deque đảm bảo rằng mỗi thao tác trên chúng được khấu hao O(1). Các điều kiện biên được xử lý cẩn thận bằng cách kiểm tra các chỉ số đối với`l`khi con trỏ bên trái di chuyển. Lập chỉ mục dựa trên 1 được áp dụng khi lưu trữ các khoảng thời gian. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3 3
14 12 10
```Dấu vết của các biến chính: 

| r | tôi | max_deque | min_deque | curr_len | max_len | khoảng thời gian | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | [0] | [0] | 1 | 1 | [(1,1)] | 
| 1 | 0 | [0] | [1] | 2 | 2 | [(1,2)] | 
| 2 | 1 | [1,2] | [2] | 2 | 2 | [(1,2),(2,3)] | 

Điều này chứng tỏ rằng phương thức hai con trỏ và deque theo dõi chính xác tất cả các khoảng thời gian tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cuốn sách được thêm và xóa khỏi mỗi deque nhiều nhất một lần. Truyền tải hai con trỏ là tuyến tính. | 
| Không gian | O(n) | Hai deque mỗi cái có thể lưu trữ tối đa`n`chỉ số trong trường hợp xấu nhất. | 

Với n tối đa 10^5, các thao tác O(n) dễ dàng nằm trong giới hạn 2 giây và mức sử dụng bộ nhớ thấp hơn nhiều so với 64 MB. 

## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sysio
    f = sysio.StringIO()
    with redirect_stdout(f):
        exec(open('solution.py').read())
    return f.getvalue().strip()

# Provided sample
assert run("3 3\n14 12 10\n") == "2 2\n1 2\n2 3", "sample 1"

# Minimum size
assert run("1 0\n5\n") == "1 1\n1 1", "single book"

# All equal values
assert run("4 0\n7 7 7 7\n") == "4 1\n1 4", "all equal"

# Maximum k
assert run("5 1000000\n1 1000 500 999 1001\n") == "5 1\n1 5", "huge k allows all"

# Boundary condition
assert run("5 2\n1 2 3 4 5\n") == "3 3\n1 3\n2 4\n3 5", "sliding windows correct"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 cuốn sách | 1 1\n1 1 | Đầu vào tối thiểu được xử lý chính xác | 
| 4 cuốn bằng nhau | 4 1\n1 4 | k=0 với tất cả các độ cao bằng nhau | 
| Tối đa k | 5 1\n1 5 | K lớn bao gồm tất cả |
