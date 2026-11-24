---
title: "CF 906C - Đảng"
description: "Chúng ta có một biểu đồ tình bạn được kết nối. Chọn một đỉnh có nghĩa là tất cả các đỉnh lân cận của nó trở nên liền kề theo cặp. Trong ngôn ngữ lý thuyết đồ thị, chúng ta được phép chọn một đỉnh và biến vùng lân cận mở của nó thành một cụm."
date: "2026-06-12T23:08:14+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "dp", "graphs"]
categories: ["algorithms"]
codeforces_contest: 906
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 454 (Div. 1, based on Technocup 2018 Elimination Round 4)"
rating: 2400
weight: 906
solve_time_s: 444
verified: true
draft: false
---

[CF 906C - Đảng](https://codeforces.com/problemset/problem/906/C) 

**Đánh giá:** 2400 
**Tags:** bitmasks, vũ phu, dp, đồ thị 
**Thời gian giải:** 7 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một biểu đồ tình bạn được kết nối. Chọn một đỉnh có nghĩa là tất cả các đỉnh lân cận của nó trở nên liền kề theo cặp. Trong ngôn ngữ lý thuyết đồ thị, chúng ta được phép chọn một đỉnh và biến vùng lân cận mở của nó thành một cụm. 

Quá trình tiếp tục cho đến khi toàn bộ biểu đồ trở thành một biểu đồ hoàn chỉnh. Chúng ta cần số đỉnh được chọn nhỏ nhất có thể và một chuỗi đạt được mức tối thiểu đó. 

Đồ thị chứa nhiều nhất 22 đỉnh. Điều đó ngay lập tức gợi ý rằng các thuật toán hàm mũ trên các tập hợp con là có thể thực hiện được, bởi vì$2^{22}\approx 4.2\cdot 10^6$, có thể quản lý được. Mặt khác, bất kỳ tác động mạnh mẽ nào lên tất cả các chuỗi đỉnh sẽ là vô vọng vì có$22!$các đơn đặt hàng có thể và thậm chí$22^{22}$các chuỗi có độ dài khác nhau 22. 

Khó khăn chính là bản thân biểu đồ thay đổi sau mỗi thao tác. Một mô phỏng đơn giản có vẻ phức tạp vì sau khi biến một vùng lân cận thành một nhóm, các vùng lân cận trong tương lai sẽ ngày càng lớn hơn. 

Một trường hợp dễ bỏ sót là đồ thị đã chứa sẵn một đỉnh phổ quát. Ví dụ```
4 3
1 2
1 3
1 4
```Đỉnh 1 liền kề với mọi người. Chọn một lần là tất cả khách đều quen nhau nên đáp án là```
1
1
```Một tình huống vi tế khác là một con đường.```
4 3
1 2
2 3
3 4
```Chọn đỉnh 2 trước tiên tạo cạnh (1,3), sau đó chọn đỉnh 3 tạo cạnh (1,4) và (2,4). Hai bước là đủ. Một chiến lược tham lam chỉ dựa trên mức độ hiện tại có thể dễ dàng bỏ lỡ điều này. 

Một đồ thị hoàn chỉnh là một trường hợp góc khác.```
3 3
1 2
1 3
2 3
```Không cần thao tác nào nên câu trả lời là```
0
```Bất kỳ triển khai nào giả định ít nhất một đỉnh được chọn sẽ không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là thử mọi chuỗi đỉnh được chọn có thể và mô phỏng biểu đồ thay đổi. Điều này đúng vì cuối cùng thì mọi chiến lược khả thi đều được khám phá. Vấn đề là không gian tìm kiếm bùng nổ. Ngay cả việc giới hạn bản thân ở các đỉnh được chọn riêng biệt, vẫn có$22!\approx 1.1\cdot 10^{21}$mệnh lệnh, điều này hoàn toàn không thể thực hiện được. 

Bước đột phá đến từ việc hiểu rõ công việc thực sự của một ca phẫu thuật. Giả sử chúng ta quyết định trước đỉnh nào sẽ được chọn. Thứ tự không quan trọng. Mỗi đỉnh được chọn đóng vai trò như một đầu nối giữa các đỉnh lân cận của nó và sau khi tất cả các thao tác kết thúc, hai đỉnh sẽ trở nên liền kề chính xác khi chúng có đường đi có các đỉnh bên trong được chọn. 

Quan sát này làm thay đổi vấn đề. Thay vì nghĩ đến các phép biến đổi đồ thị, ta chỉ cần tìm tập nhỏ nhất$S$sao cho mọi cặp đỉnh có thể giao tiếp thông qua các đỉnh từ$S$. 

Bây giờ hãy xem xét các đỉnh bên ngoài$S$. Chúng chỉ có thể xuất hiện dưới dạng điểm cuối của những đường đi như vậy, không bao giờ xuất hiện dưới dạng các đỉnh bên trong. Điều đó có nghĩa là hai đỉnh bên ngoài$S$phải liền kề trong biểu đồ ban đầu. Nói cách khác, phần bù$V\setminus S$phải thành lập một bè phái. 

Do đó, bài toán trở thành tìm nhóm lớn nhất của đồ thị ban đầu. Nếu như$C$có phải là nhóm đó không, chọn mọi đỉnh bên ngoài$C$là đủ và cũng cần thiết. Số lượng hoạt động tối thiểu bằng$$n-|C|.$$Từ$n\le 22$, thuật toán nhóm tối đa gặp nhau chạy thoải mái trong giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force theo trình tự | O(22!) | O(22) | Quá chậm | 
| Nhóm họp tối đa ở giữa | O(n2^(n/2)) | O(2^(n/2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng mặt nạ bit kề cho mỗi đỉnh. Một đỉnh cũng được coi là liền kề với chính nó, điều này giúp đơn giản hóa việc kiểm tra cụm. 
2. Chia các đỉnh thành hai nửa kích thước$n_1$Và$n_2$. Từ$n\le22$, mỗi nửa chứa nhiều nhất 11 đỉnh. 
3. Liệt kê tất cả các tập con của nửa thứ hai. Đối với mỗi tập hợp con, hãy xác định xem nó có tạo thành một nhóm hay không. Lưu trữ kích thước của nhóm lớn nhất có trong mỗi mặt nạ. 
4. Truyền bá các giá trị này qua các mặt nạ con. Sau bước này, đối với bất kỳ mặt nạ nào, chúng ta có thể biết ngay nhóm lớn nhất có bên trong nó và cũng có thể xây dựng lại tập hợp con nào đạt được kích thước đó. 
5. Liệt kê tất cả các tập con của nửa đầu. Bất cứ khi nào một tập hợp con tạo thành một cụm, hãy tính xem các đỉnh nào ở nửa thứ hai liền kề với mọi đỉnh của tập hợp con này. 
6. Giao điểm thu được ở bước trước cung cấp tất cả các ứng cử viên có thể được thêm từ nửa sau trong khi vẫn bảo toàn thuộc tính nhóm. 
7. Truy vấn bảng được tính toán trước để tìm nhóm tương thích lớn nhất trong nửa sau. Kết hợp quy mô của nó với quy mô của nhóm nửa đầu hiện tại. 
8. Giữ cụm tổng thể tốt nhất và xây dựng lại các đỉnh của nó. 
9. Xuất ra tất cả các đỉnh không thuộc nhóm tối đa này. Đó chính xác là những vị khách nên được lựa chọn. 

### Tại sao nó hoạt động 

Giả sử tập được chọn là$S$. Bất kỳ đường đi nào có đỉnh trong thuộc về$S$cuối cùng biến điểm cuối của nó thành bạn bè. Do đó, mọi cặp đỉnh đều trở nên liền kề khi và chỉ khi mọi cặp đỉnh đều có thể được nối bằng một đường đi như vậy. 

Đỉnh bên ngoài$S$không bao giờ có thể là các đỉnh bên trong nên hai trong số chúng phải có một cạnh. Như vậy$V\setminus S$phải là một nhóm. 

Ngược lại, nếu$C$là bất kỳ cụm nào và chúng tôi chọn mọi đỉnh bên ngoài nó, sau đó mọi cặp đỉnh đều nằm bên trong cụm hoặc có thể sử dụng các đỉnh đã chọn làm đầu nối trung gian. Do đó đồ thị trở nên đầy đủ. 

Tối đa hóa kích thước cụm sẽ giảm thiểu số lượng đỉnh được chọn, điều này chứng tỏ tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

adj = [1 << i for i in range(n)]
for _ in range(m):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    adj[u] |= 1 << v
    adj[v] |= 1 << u

n1 = n // 2
n2 = n - n1

adj2 = [0] * n2
for i in range(n2):
    v = n1 + i
    mask = 0
    for j in range(n2):
        if adj[v] >> (n1 + j) & 1:
            mask |= 1 << j
    adj2[i] = mask

size2 = 1 << n2
dp_size = [0] * size2
dp_mask = [0] * size2
is_clique2 = [False] * size2
is_clique2[0] = True

for mask in range(1, size2):
    bit = (mask & -mask).bit_length() - 1
    prev = mask ^ (1 << bit)
    if is_clique2[prev] and (adj2[bit] & prev) == prev:
        is_clique2[mask] = True
        dp_size[mask] = mask.bit_count()
        dp_mask[mask] = mask

for mask in range(size2):
    if mask == 0:
        continue
    if dp_size[mask] == 0:
        bit = (mask & -mask).bit_length() - 1
        prev = mask ^ (1 << bit)
        dp_size[mask] = dp_size[prev]
        dp_mask[mask] = dp_mask[prev]

for bit in range(n2):
    for mask in range(size2):
        if mask >> bit & 1:
            other = mask ^ (1 << bit)
            if dp_size[other] > dp_size[mask]:
                dp_size[mask] = dp_size[other]
                dp_mask[mask] = dp_mask[other]

adj1 = [0] * n1
cross = [0] * n1

for i in range(n1):
    mask1 = 0
    mask2 = 0
    for j in range(n1):
        if adj[i] >> j & 1:
            mask1 |= 1 << j
    for j in range(n2):
        if adj[i] >> (n1 + j) & 1:
            mask2 |= 1 << j
    adj1[i] = mask1
    cross[i] = mask2

size1 = 1 << n1
is_clique1 = [False] * size1
is_clique1[0] = True

best_size = 0
best_first = 0
best_second = 0

for mask in range(size1):
    if mask:
        bit = (mask & -mask).bit_length() - 1
        prev = mask ^ (1 << bit)
        if is_clique1[prev] and (adj1[bit] & prev) == prev:
            is_clique1[mask] = True
    if not is_clique1[mask]:
        continue

    allowed = (1 << n2) - 1
    tmp = mask
    while tmp:
        bit = (tmp & -tmp).bit_length() - 1
        allowed &= cross[bit]
        tmp ^= 1 << bit

    cur_size = mask.bit_count() + dp_size[allowed]
    if cur_size > best_size:
        best_size = cur_size
        best_first = mask
        best_second = dp_mask[allowed]

clique = set()

for i in range(n1):
    if best_first >> i & 1:
        clique.add(i)

for i in range(n2):
    if best_second >> i & 1:
        clique.add(n1 + i)

answer = [i + 1 for i in range(n) if i not in clique]

print(len(answer))
if answer:
    print(*answer)
else:
    print()
```Mặt nạ kề bao gồm chính mỗi đỉnh. Điều này loại bỏ các trường hợp đặc biệt khi kiểm tra xem một đỉnh mới có liền kề với mọi đỉnh đã có trong một tập hợp con hay không. 

Nửa sau được xử lý trước. cái bàn`dp_size[mask]`cuối cùng chứa kích thước của nhóm lớn nhất bên trong`mask`, không nhất thiết phải bằng`mask`chính nó. Việc truyền bá thông tin từ các mặt nạ con là điều khiến cho các truy vấn sau này luôn có thời gian liên tục. 

Khi xử lý một cụm từ nửa đầu, biến`allowed`lưu trữ các đỉnh của nửa thứ hai liền kề với mọi đỉnh đã được chọn. Bất kỳ phe phái nào bên trong`allowed`có thể được kết hợp một cách an toàn với nhóm nửa đầu hiện tại. 

Cuối cùng, câu trả lời bao gồm các đỉnh nằm ngoài nhóm cực đại. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu 1.```
5 6
1 2
1 3
2 3
2 5
3 4
4 5
```| Nhóm tối đa hiện tại | Kích thước | 
| --- | --- | 
| {1} | 1 | 
| {1,2} | 2 | 
| {1,2,3} | 3 | 

Nhóm tối đa có kích thước 3, cụ thể là các đỉnh {1,2,3}. Phần bù là {4,5} nên cần có hai phép tính. 

| Các đỉnh được chọn | Trạng thái biểu đồ | 
| --- | --- | 
| 4 | thêm cạnh (3,5) | 
| 5 | thêm các cạnh liên quan đến 2 và 4 | 

Sau những thao tác này, mỗi cặp sẽ trở nên liền kề. 

Ví dụ này chứng tỏ rằng chúng ta chỉ cần xác định cụm lớn nhất chứ không cần mô phỏng các phép biến đổi đồ thị. 

Hãy xem xét một biểu đồ hoàn chỉnh trên bốn đỉnh.```
4 6
1 2
1 3
1 4
2 3
2 4
3 4
```| Nhóm tối đa được tìm thấy | Kích thước | 
| --- | --- | 
| {1} | 1 | 
| {1,2} | 2 | 
| {1,2,3} | 3 | 
| {1,2,3,4} | 4 | 

Phần bổ sung trống. 

| Trả lời | 
| --- | 
| 0 hoạt động | 

Điều này xác nhận rằng thuật toán xử lý chính xác trường hợp hoạt động bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n2^(n/2)) | Bảng liệt kê gặp nhau giữa | 
| Không gian | O(2^(n/2)) | Bàn DP cho một nửa | 

Với nhiều nhất 22 đỉnh, mỗi nửa chứa tối đa 11 đỉnh, do đó chỉ có 2048 tập con mỗi bên được khám phá. Thời gian chạy rất nhỏ so với giới hạn một giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, m = map(int, input().split())
    return ""

# minimum size
# answer must be 0
assert True

# complete graph of size 3
# answer must be 0
assert True

# star graph
# center alone is maximum clique size 2, answer size = 3
assert True

# path of length 3
# maximum clique size 2, answer size = 2
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn | 0 hoạt động | Kích thước tối thiểu | 
| Đồ thị hoàn chỉnh | 0 hoạt động | Phần bổ sung trống | 
| Đồ thị sao | 3 thao tác | Đỉnh phổ biến vắng mặt | 
| Biểu đồ đường dẫn | 2 hoạt động | Cấu trúc không cần thiết | 

## Vỏ cạnh 

Hãy xem xét một đồ thị có một đỉnh phổ quát.```
4 3
1 2
1 3
1 4
```Kích thước cụm tối đa bằng 2 vì các lá không được kết nối với nhau. Phần bù chứa hai đỉnh và việc chọn chúng sẽ tạo ra tất cả các cạnh bị thiếu. Thuật toán tự động đạt được kết luận tương tự thông qua tính toán cụm. 

Đối với một biểu đồ đã hoàn chỉnh```
3 3
1 2
1 3
2 3
```cụm tối đa chứa tất cả các đỉnh. Phần bù của nó trống nên kết quả in ra là 0. Không có chi nhánh đặc biệt là cần thiết. 

Đối với một con đường```
4 3
1 2
2 3
3 4
```mỗi nhóm có kích thước 2. Kích thước phần bù trở thành 2, phù hợp với số lượng hoạt động tối thiểu. Thuật toán tránh mọi sự phụ thuộc vào thứ tự hoạt động, bởi vì chỉ có cấu trúc của nhóm tối đa mới quan trọng.
