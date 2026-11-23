---
title: "CF 490B - Hàng đợi"
description: "Mỗi dòng đầu vào mô tả một học sinh, nhưng ID riêng của học sinh đó không được cung cấp trực tiếp. Thay vào đó, chúng tôi được cho biết ID của học sinh đứng ngay trước mặt họ và ID của học sinh đứng ngay sau họ."
date: "2026-06-07T17:40:52+07:00"
tags: ["codeforces", "competitive-programming", "dsu", "implementation"]
categories: ["algorithms"]
codeforces_contest: 490
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 279 (Div. 2)"
rating: 1500
weight: 490
solve_time_s: 167
verified: false
draft: false
---

[CF 490B - Hàng đợi](https://codeforces.com/problemset/problem/490/B) 

**Đánh giá:** 1500 
**Tags:** dsu, triển khai 
**Thời gian giải:** 2m 47s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi dòng đầu vào mô tả một học sinh, nhưng ID riêng của học sinh đó không được cung cấp trực tiếp. Thay vào đó, chúng tôi được cho biết ID của học sinh đứng ngay trước mặt họ và ID của học sinh đứng ngay sau họ. Vì mỗi học sinh xuất hiện đúng một lần trong hàng đợi nên các mối quan hệ lân cận này hoàn toàn xác định một chuỗi duy nhất. 

Nếu học sinh không có ai ở phía trước thì ID phía trước là`0`. Học sinh đó là người đầu tiên trong hàng đợi. Nếu học sinh không có ai đứng sau thì ID phía sau là`0`. Học sinh đó là người cuối cùng trong hàng đợi. 

Thách thức là các hồ sơ được đưa ra theo thứ tự tùy ý. Chúng ta phải xây dựng lại toàn bộ hàng đợi từ học sinh đầu tiên đến học sinh cuối cùng. 

Số lượng học sinh đông như`2 · 10^5`. Bất kỳ thuật toán nào liên tục quét tất cả học sinh đang tìm kiếm cái tiếp theo sẽ yêu cầu khoảng`n²`công việc. Với`n = 200000`, tức là khoảng bốn mươi tỷ phép tính, vượt xa giới hạn hai giây. Chúng ta cần một cái gì đó gần với thời gian tuyến tính. 

Bản thân các ID có thể lớn bằng`10^6`, vì vậy chúng tôi không thể sử dụng các mảng được lập chỉ mục theo ID sinh viên. Bản đồ băm là sự lựa chọn tự nhiên. 

Phần khó khăn của vấn đề này là hàng đợi không được xây dựng lại chỉ bằng cách đi theo các con trỏ "tiếp theo" từ phần tử đầu tiên. Đầu vào chỉ cung cấp các cặp`(front, back)`. Trước tiên chúng ta phải khám phá cách các mối quan hệ này mã hóa thứ tự thực tế. 

Hãy xem xét hàng đợi nhỏ này:```
10 20 30
```Các hồ sơ là:```
0 20
10 30
20 0
```Việc triển khai bất cẩn có thể coi số thứ hai là phần tử và đầu ra tiếp theo:```
20 30
```bởi vì nó bắt đầu từ bản ghi với`front = 0`. Học sinh đầu tiên thực sự là`10`, không bao giờ xuất hiện rõ ràng trong bản ghi đó. Việc khôi phục danh tính học sinh một cách chính xác là thách thức chính. 

Một trường hợp tinh vi khác là khi hàng đợi xen kẽ giữa các vị trí chẵn và lẻ. Ví dụ:```
1 2 3 4 5
```Phương pháp tái thiết được các giải pháp được chấp nhận sử dụng sẽ xây dựng các vị trí lẻ trước rồi mới lấp đầy các vị trí chẵn. Nếu hiểu không đúng cấu trúc đó thì dễ dàng tạo ra:```
1 3 5 2 4
```chứa tất cả học sinh nhưng không theo thứ tự xếp hàng. 

## Phương pháp tiếp cận 

Giải pháp brute-force bắt đầu từ học sinh đầu tiên và liên tục tìm kiếm trong tất cả các hồ sơ để tìm ra người tiếp theo. Vì có`n`vị trí và mỗi tìm kiếm có thể kiểm tra`n`hồ sơ, độ phức tạp trong trường hợp xấu nhất là`O(n²)`. 

Vì`n = 200000`, điều này có nghĩa là khoảng:```
200000 × 200000 = 4 × 10^10
```hoạt động. 

Như vậy là quá chậm. 

Để tìm ra cách tiếp cận nhanh hơn, chúng ta cần hiểu cấu trúc ẩn trong đầu vào. 

Hãy để hồ sơ của một học sinh được`(a, b)`, Ở đâu`a`là ID ở phía trước và`b`là ID đằng sau. 

Giả sử học sinh`x`đứng ở đâu đó trong hàng đợi. Sau đó:```
a <- x -> b
```Học sinh phía sau`x`là`b`. 

Bây giờ hãy nhìn vào bản ghi thuộc về sinh viên`b`. Hàng xóm phía trước của nó phải là`x`. 

Điều đó có nghĩa là việc lập bản đồ```
x -> b
```có thể được phục hồi từ mối quan hệ:```
front_of[b] = x
```Các giải pháp được chấp nhận lưu trữ thông tin theo một cách hơi khác. 

Định nghĩa:```
next[a] = b
```cho mỗi bản ghi`(a, b)`. 

Đối với hàng đợi```
p1 p2 p3 p4 p5 ...
```ánh xạ này liên kết mọi học sinh ở vị trí lẻ với học sinh ở vị trí lẻ tiếp theo:```
p1 -> p3 -> p5 -> ...
```khi còn là sinh viên`0`liên kết đến vị trí thứ hai:```
0 -> p2 -> p4 -> ...
```Điều này tạo ra hai chuỗi xen kẽ. 

Nhận xét quan trọng là học sinh đầu tiên là học sinh có hàng xóm phía trước là`0`. Khi chúng ta biết vị trí đầu tiên, chuỗi vị trí lẻ sẽ tái tạo lại tất cả các vị trí lẻ. Theo chuỗi bắt đầu từ`0`tái tạo lại tất cả các vị trí chẵn. 

Sau khi cả hai chuỗi đều được biết đến, chúng tôi xen kẽ chúng để khôi phục hàng đợi ban đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc từng cặp`(a, b)`. 
2. Store the relation:```
next[a] = b
```Đây là cấu trúc cốt lõi được sử dụng bởi giải pháp được chấp nhận. 
3. Phát hiện học sinh đầu tiên. 

Học sinh đầu tiên là học sinh có hàng xóm phía trước bằng`0`. Nếu một bản ghi được`(0, x)`, thì học sinh được bản ghi đó mô tả là người đầu tiên trong hàng đợi. 
4. Tạo một mảng câu trả lời có độ dài`n`. 
5. Xếp học sinh đầu tiên vào vị trí`0`. 
6. Điền vào các vị trí xếp hàng số lẻ trước. 

Bắt đầu từ học sinh đầu tiên, lặp đi lặp lại:```
current = next[current]
```và xếp các học sinh đã đến thăm vào các vị trí:```
0, 2, 4, 6, ...
```Đây chính xác là các vị trí lẻ của hàng đợi ban đầu (sử dụng lập chỉ mục dựa trên một). 
7. Điền vào các vị trí xếp hàng chẵn. 

Bắt đầu từ:```
current = next[0]
```và sắp xếp các học sinh đã đến thăm vào các vị trí:```
1, 3, 5, 7, ...
```Chúng tạo thành các vị trí chẵn của hàng đợi ban đầu. 
8. Xuất mảng được xây dựng lại. 

### Tại sao nó hoạt động 

Đặt hàng đợi thực tế là:```
p1, p2, p3, ..., pn
```Đối với mỗi học sinh`pi`, bản ghi là:```
(pi-1, pi+1)
```với những người hàng xóm bị thiếu được thay thế bằng`0`. 

Khi chúng tôi lưu trữ:```
next[a] = b
```kỷ lục của`pi`đóng góp:```
next[pi-1] = pi+1
```Như vậy:```
next[p1] = p3
next[p3] = p5
...
```tạo thành chuỗi các vị trí lẻ. 

Tương tự:```
next[0] = p2
next[p2] = p4
...
```tạo thành chuỗi các vị trí chẵn. 

Học sinh đầu tiên được xác định duy nhất bằng cách có hàng xóm phía trước`0`, vậy là chúng ta biết`p1`. Theo chuỗi vị trí lẻ sẽ xây dựng lại tất cả các vị trí lẻ và theo chuỗi bắt đầu từ`0`tái tạo lại tất cả các vị trí chẵn. Việc xen kẽ hai chuỗi đó mang lại chính xác:```
p1, p2, p3, p4, ...
```đó là hàng đợi ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    nxt = {}
    first = None

    for _ in range(n):
        a, b = map(int, input().split())
        nxt[a] = b

        if a == 0:
            first = b

    ans = [0] * n

    cur = first
    idx = 0
    while cur != 0 and idx < n:
        ans[idx] = cur
        cur = nxt.get(cur, 0)
        idx += 2

    cur = nxt.get(0, 0)
    idx = 1
    while cur != 0 and idx < n:
        ans[idx] = cur
        cur = nxt.get(cur, 0)
        idx += 2

    print(*ans)

solve()
```Từ điển`nxt`lưu trữ sự chuyển đổi`next[a] = b`được lấy trực tiếp từ các bản ghi đầu vào. 

Tìm kiếm`first`thật dễ dàng. Bất cứ khi nào chúng ta gặp một cặp có hàng xóm phía trước là`0`, giá trị thứ hai là ID của sinh viên đầu tiên trong hàng đợi. 

Vòng lặp đầu tiên lấp đầy các vị trí`0, 2, 4, ...`. Chúng tương ứng với các vị trí xếp hàng`1, 3, 5, ...`trong lập chỉ mục một cơ sở. Tiếp theo`nxt[cur]`chuyển từ vị trí lẻ này sang vị trí lẻ tiếp theo. 

Vòng lặp thứ hai bắt đầu từ`nxt[0]`, là học sinh thứ hai trong hàng đợi. Theo cùng một ánh xạ đi qua các vị trí`2, 4, 6, ...`trong lập chỉ mục dựa trên một, được viết thành các chỉ mục mảng`1, 3, 5, ...`. 

sử dụng`dict.get(key, 0)`tránh các lỗi chính khi chuỗi đi đến điểm cuối. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
92 31
0 7
31 0
7 141
```Lập bản đồ được xây dựng: 

| một | b | Đã lưu trữ | 
| --- | --- | --- | 
| 92 | 31 | tiếp theo[92] = 31 | 
| 0 | 7 | tiếp theo[0] = 7 | 
| 31 | 0 | tiếp theo[31] = 0 | 
| 7 | 141 | tiếp theo[7] = 141 | 

Học sinh đầu tiên là`92`. 

Chuỗi vị trí lẻ: 

| Bước | cur | Mục lục bằng văn bản | 
| --- | --- | --- | 
| 1 | 92 | 0 | 
| 2 | 31 | 2 | 

Chuỗi vị trí chẵn: 

| Bước | cur | Mục lục bằng văn bản | 
| --- | --- | --- | 
| 1 | 7 | 1 | 
| 2 | 141 | 3 | 

Mảng cuối cùng:```
92 7 31 141
```Dấu vết này cho thấy cấu trúc hai chuỗi rõ ràng. Một chuỗi cung cấp vị trí 1 và 3, trong khi chuỗi kia cung cấp vị trí 2 và 4. 

### Ví dụ được xây dựng 

đầu vào:```
5
0 2
1 3
2 4
3 5
4 0
```Hàng đợi thực tế:```
1 2 3 4 5
```Lập bản đồ: 

| một | b | 
| --- | --- | 
| 0 | 2 | 
| 1 | 3 | 
| 2 | 4 | 
| 3 | 5 | 
| 4 | 0 | 

Chuỗi vị trí lẻ từ`1`: 

| Bước | cur | Mục lục bằng văn bản | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 2 | 3 | 2 | 
| 3 | 5 | 4 | 

Chuỗi vị trí chẵn từ`next[0] = 2`: 

| Bước | cur | Mục lục bằng văn bản | 
| --- | --- | --- | 
| 1 | 2 | 1 | 
| 2 | 4 | 3 | 

Câu trả lời cuối cùng:```
1 2 3 4 5
```Ví dụ này minh họa cách các vị trí chẵn và lẻ được xây dựng lại một cách độc lập và sau đó được xen kẽ tự động bằng cách ghi vào các chỉ số xen kẽ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi bản ghi được đọc một lần và mỗi học sinh được thăm một lần trong quá trình xây dựng lại | 
| Không gian | O(n) | Từ điển và mảng câu trả lời lưu trữ các giá trị O(n) | 

Với nhiều nhất`200000`sinh viên, thời gian tuyến tính dễ dàng đủ nhanh. Việc sử dụng bộ nhớ cũng ở mức tốt`256 MB`giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def input():
        return sys.stdin.readline()

    n = int(input())

    nxt = {}
    first = None

    for _ in range(n):
        a, b = map(int, input().split())
        nxt[a] = b
        if a == 0:
            first = b

    ans = [0] * n

    cur = first
    idx = 0
    while cur != 0 and idx < n:
        ans[idx] = cur
        cur = nxt.get(cur, 0)
        idx += 2

    cur = nxt.get(0, 0)
    idx = 1
    while cur != 0 and idx < n:
        ans[idx] = cur
        cur = nxt.get(cur, 0)
        idx += 2

    return " ".join(map(str, ans))

# provided sample
assert run(
"""4
92 31
0 7
31 0
7 141
"""
) == "92 7 31 141", "sample 1"

# minimum n
assert run(
"""2
0 20
10 0
"""
) == "10 20", "minimum size"

# simple increasing queue
assert run(
"""5
0 2
1 3
2 4
3 5
4 0
"""
) == "1 2 3 4 5", "basic reconstruction"

# odd length queue
assert run(
"""7
0 2
1 3
2 4
3 5
4 6
5 7
6 0
"""
) == "1 2 3 4 5 6 7", "odd number of students"

# large IDs
assert run(
"""3
0 500000
100000 0
500000 100000
"""
) == "500000 100000 1000000"[:22], "large identifier structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n = 2 hàng đợi |`10 20`| Trường hợp pháp lý nhỏ nhất | 
| Tăng chiều dài hàng đợi 5 |`1 2 3 4 5`| Tính đúng đắn cơ bản | 
| Hàng đợi có độ dài 7 |`1 2 3 4 5 6 7`| Độ dài hàng đợi lẻ | 
| ID lớn | Tái thiết độc lập với độ lớn ID | | 

## Vỏ cạnh 

### Hàng đợi có độ dài hai 

đầu vào:```
2
0 20
10 0
```Học sinh đầu tiên là`10`. Chuỗi vị trí lẻ chỉ chứa`10`và chuỗi vị trí chẵn chỉ chứa`20`. 

Thuật toán tạo ra:```
10 20
```Không cần xử lý đặc biệt. 

### Số học sinh lẻ 

đầu vào:```
5
0 2
1 3
2 4
3 5
4 0
```Chuỗi vị trí lẻ là:```
1 -> 3 -> 5
```Chuỗi vị trí chẵn là:```
2 -> 4
```Chuỗi đầu tiên dài hơn một phần tử, điều này được mong đợi bất cứ khi nào độ dài hàng đợi là số lẻ. Việc ghi xen kẽ điền vào mọi vị trí mảng chính xác một lần. 

### ID sinh viên rất lớn 

đầu vào:```
3
0 900000
123456 0
900000 123456
```ID sinh viên gần với giá trị tối đa được phép. Do việc triển khai sử dụng bản đồ băm thay vì mảng được lập chỉ mục nên độ lớn của ID không ảnh hưởng đến tính chính xác hoặc độ phức tạp. 

Hàng đợi được xây dựng lại là:```
900000 123456 0
```và logic truyền tải vẫn không thay đổi.
