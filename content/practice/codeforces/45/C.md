---
title: "CF 45C - Bài học khiêu vũ"
description: "Chúng tôi được xếp một hàng người, mỗi người được xác định là nam hay nữ và mỗi người đều có kỹ năng nhảy số. Đường này phát triển theo thời gian khi các cặp đôi gồm một trai và một gái ở cạnh nhau và có sự khác biệt nhỏ nhất về kỹ năng sẽ nhảy."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 45
codeforces_index: "C"
codeforces_contest_name: "School Team Contest 3 (Winter Computer School 2010/11)"
rating: 1900
weight: 45
solve_time_s: 84
verified: true
draft: false
---

[CF 45C - Bài học khiêu vũ](https://codeforces.com/problemset/problem/45/C) 

**Xếp hạng:** 1900 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được xếp một hàng người, mỗi người được xác định là nam hay nữ và mỗi người đều có kỹ năng nhảy số. Đường này phát triển theo thời gian khi các cặp đôi gồm một trai và một gái ở cạnh nhau và có sự khác biệt nhỏ nhất về kỹ năng sẽ nhảy. Khi một cặp đôi rời đi, hàng còn lại sẽ tự động "đóng lại", vì vậy mọi người di chuyển lại gần nhau hơn và quá trình này lặp lại cho đến khi không còn cặp trai-gái liền kề hợp lệ. Nhiệm vụ là xác định thứ tự các cặp đôi rời đi và những cá nhân nào được ghép đôi. Mọi người giữ lại vị trí ban đầu của họ cho mục đích đánh số. 

Những ràng buộc cho chúng ta biết rằng`n`có thể lên tới 200.000. Một cách tiếp cận đơn giản để kiểm tra tất cả các cặp trong dòng ở mỗi bước có thể yêu cầu kiểm tra theo thứ tự$n^2$các cặp liền kề lặp đi lặp lại, có thể lên tới 4·10^10 thao tác. Điều này vượt xa giới hạn 2 giây, vì vậy chúng ta cần một cái gì đó gần hơn với O(n log n) hoặc O(n). Giá trị kỹ năng có thể lên tới 10^7, điều này không ảnh hưởng đến việc lựa chọn thuật toán nhưng xác nhận rằng số nguyên là đủ. 

Các trường hợp phức tạp phát sinh khi nhiều cặp liền kề có cùng sự khác biệt về kỹ năng tối thiểu. Trong trường hợp đó, người hòa là cặp ngoài cùng bên trái. Một cách tiếp cận đơn giản có thể vô tình loại bỏ một cặp ở giữa vì nó chỉ tìm kiếm sự khác biệt tối thiểu đầu tiên mà không xem xét những thay đổi kề sau khi loại bỏ. Một trường hợp đặc biệt khác là khi có một nhóm bé trai hoặc bé gái không có người khác giới liền kề - những người này đơn giản là không bao giờ tạo thành một cặp. 

Một ví dụ cụ thể minh họa điều này. Hãy xem xét đầu vào:```
3
BBG
1 2 3
```Cặp hợp lệ duy nhất là chàng trai và cô gái cuối cùng. Một cách tiếp cận ngây thơ có thể cố gắng ghép đôi hai người đầu tiên nhưng không nhận ra rằng họ cùng giới tính. Đầu ra đúng chỉ nên liệt kê cặp (2,3). 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu hoạt động bằng cách quét liên tục dòng từ trái sang phải, tính toán sự khác biệt tuyệt đối về kỹ năng cho từng cặp nam-gái liền kề, chọn sự khác biệt tối thiểu và loại bỏ cặp đó. Điều này đúng vì nó tuân theo chính xác các quy tắc được mô tả, nhưng nó trở nên quá chậm đối với n = 200.000. Mỗi lần quét có thể là O(n) và chúng tôi có thể thực hiện quét O(n) nếu mỗi cặp rời đi riêng lẻ, dẫn đến độ phức tạp O(n^2). 

Quan sát quan trọng cho một giải pháp tối ưu là tại bất kỳ thời điểm nào, chỉ những cặp trai-gái liền kề mới có thể trở thành ứng cử viên. Khi một cặp bị loại bỏ, chỉ các cặp lân cận với các phần tử bị loại bỏ mới có thể thay đổi sự khác biệt tối thiểu của chúng. Điều này tự nhiên gợi ý sử dụng **hàng ưu tiên** để theo dõi tất cả các cặp nam-nữ liền kề hiện tại dựa trên sự khác biệt về kỹ năng của họ. Mỗi mục trong hàng ưu tiên lưu trữ sự khác biệt về kỹ năng và chỉ số của cặp. Khi chúng tôi xóa một cặp, chúng tôi sẽ xóa các cặp bị ảnh hưởng khỏi hàng đợi và thêm các cặp mới được tạo bởi hàng xóm của chúng. Điều này làm giảm việc kiểm tra dư thừa và cho phép chúng tôi luôn chọn chênh lệch tối thiểu một cách hiệu quả. 

Lực lượng vũ phu hoạt động vì nó tuân theo mô tả vấn đề theo đúng nghĩa đen. Nó thất bại khi n lớn vì việc tính toán lại nhiều lần tất cả các sai phân liền kề là quá tốn kém. Việc quan sát thấy rằng chỉ các cặp lân cận thay đổi khi ai đó rời đi cho phép chúng tôi giảm bớt vấn đề để duy trì hiệu quả các cặp liền kề và sự khác biệt của chúng, nhanh hơn nhiều. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu (hàng ưu tiên + danh sách liên kết) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích dữ liệu đầu vào để có được danh sách mọi người, giới tính và kỹ năng của họ. Chỉ định cho mỗi người một chỉ mục gốc cho mục đích đầu ra. 
2. Tạo danh sách liên kết đôi đại diện cho dòng. Điều này cho phép loại bỏ các phần tử theo thời gian liên tục và truy cập nhanh vào các phần tử lân cận. 
3. Xác định tất cả các cặp trai-gái liền kề và tính toán sự khác biệt về kỹ năng của chúng. Chèn từng cặp vào hàng đợi ưu tiên heap tối thiểu được khóa bằng sự khác biệt. Lưu trữ không chỉ các chỉ mục mà còn cả các con trỏ tới các nút danh sách liên kết. 
4. Trong khi hàng đợi ưu tiên không trống: 

1. Lấy cặp sai biệt tối thiểu ra khỏi đống. Kiểm tra xem cả hai nút vẫn còn trong danh sách. Nếu một trong hai đã bị xóa, hãy loại bỏ cặp này. 
2. Ghi lại các chỉ số đầu ra của cặp, sắp xếp theo thứ tự tăng dần. 
3. Xóa cả hai nút khỏi danh sách liên kết. 
4. Kiểm tra các hàng xóm mới được tạo bằng cách xóa. Nếu tồn tại một cặp trai-gái mới liền kề, hãy tính hiệu của nó và thêm nó vào heap. 
5. In ra tổng số cặp được hình thành và danh sách các cặp theo thứ tự. 

Thuật toán hoạt động vì chúng tôi duy trì một bất biến: hàng đợi ưu tiên luôn chứa tất cả các cặp trai-gái liền kề hợp lệ tồn tại tại thời điểm hiện tại trong dòng. Bằng cách chỉ thêm các cặp mới được hình thành bởi các nút lân cận sau khi loại bỏ, chúng tôi đảm bảo rằng vùng nhớ luôn phản ánh sự khác biệt tối thiểu tiếp theo. Việc kiểm tra xem các nút có còn tồn tại hay không sẽ ngăn việc sử dụng các cặp lỗi thời. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

class Node:
    def __init__(self, idx, gender, skill):
        self.idx = idx
        self.gender = gender
        self.skill = skill
        self.prev = None
        self.next = None
        self.alive = True

n = int(input())
genders = input().strip()
skills = list(map(int, input().split()))

nodes = [Node(i+1, genders[i], skills[i]) for i in range(n)]
for i in range(n-1):
    nodes[i].next = nodes[i+1]
    nodes[i+1].prev = nodes[i]

heap = []

def push_pair(left, right):
    if left.alive and right.alive and left.gender != right.gender:
        diff = abs(left.skill - right.skill)
        heapq.heappush(heap, (diff, left.idx, right.idx, left, right))

for i in range(n-1):
    push_pair(nodes[i], nodes[i+1])

result = []

while heap:
    diff, idx1, idx2, left, right = heapq.heappop(heap)
    if not left.alive or not right.alive:
        continue
    result.append((min(left.idx, right.idx), max(left.idx, right.idx)))
    left.alive = right.alive = False
    # Link neighbors
    if left.prev:
        left.prev.next = right.next
    if right.next:
        right.next.prev = left.prev
    if left.prev and right.next:
        push_pair(left.prev, right.next)

print(len(result))
for a,b in result:
    print(a,b)
```Mã này xây dựng một danh sách liên kết các nút đại diện cho mọi người. Mỗi nút lưu trữ một con trỏ tới các nút lân cận và một cờ để cho biết liệu nó đã bị xóa hay chưa. Chỉ các cặp trai-gái liền kề hợp lệ mới được thêm vào đống. Sau khi loại bỏ một cặp, chúng tôi kiểm tra vùng kề mới được tạo bởi việc loại bỏ và đẩy nó vào heap. sử dụng`alive`ngăn chặn việc sử dụng các cặp cũ đã bị xóa khỏi dòng. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
4
BGBG
4 2 4 3
```| Bước | Dòng | Đỉnh đống | Cặp đã chọn | Dòng mới | 
| --- | --- | --- | --- | --- | 
| 0 | B(4) G(2) B(4) G(3) | (2, 2,3) | 3,4 | B(4) G(2) | 
| 1 | B(4) G(2) | (2, 1,2) | 1,2 | trống | 

Bảng cho thấy ở mỗi bước, chúng tôi chọn cặp có sự khác biệt tối thiểu giữa các cặp B-G liền kề, loại bỏ chúng và cập nhật các cặp lân cận. 

**Ví dụ tùy chỉnh** 

đầu vào:```
5
BGBGB
1 5 2 6 3
```| Bước | Dòng | Đỉnh đống | Cặp đã chọn | Dòng mới | 
| --- | --- | --- | --- | --- | 
| 0 | B(1) G(5) B(2) G(6) B(3) | (1, 3,4) | 3,4 | B(1) G(5) B(3) | 
| 1 | B(1) G(5) B(3) | (2, 1,2) | 1,2 | B(3) | 

Trình bày cách xử lý nhiều lần xóa và sửa lỗi liên kết từ trái sang phải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi cặp liền kề được thêm vào heap nhiều nhất một lần. Các thao tác trên heap là log n. | 
| Không gian | O(n) | Các nút danh sách liên kết và các mục nhập heap có tỷ lệ tuyến tính với n. | 

Với n 2·10^5, các thao tác O(n log n) diễn ra tốt đẹp trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        exec(open("solution.py").read())
    return out.getvalue().strip()

# Provided sample
assert run("4\nBGBG\n4 2 4 3\n") == "2\n3 4\n1 2"

# Minimum size
assert run("1\nB\n1\n") == "0"

# All boys
assert run("3\nBBB\n1 2 3\n") == "0"

# All equal values
assert run("4\nBGBG\n5 5 5 5\n") == "2\n1
```
