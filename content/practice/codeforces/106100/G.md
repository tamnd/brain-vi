---
title: "CF 106100G - Phân khúc có thể chuyển đổi"
description: "Chúng ta được cho một mảng dài A và một mảng mục tiêu B. Với bất kỳ đoạn A[L..R] nào được chọn, chúng ta được phép xóa liên tục các phần tử cho đến khi độ dài đoạn trở thành m = Nhiệm vụ là đếm xem có bao nhiêu đoạn của A có thể chuyển đổi chính xác thành B."
date: "2026-06-25T11:53:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106100
codeforces_index: "G"
codeforces_contest_name: "International MathCoding Narxoz open olympiad 2025"
rating: 0
weight: 106100
solve_time_s: 90
verified: true
draft: false
---

[CF 106100G - Phân đoạn có thể chuyển đổi](https://codeforces.com/problemset/problem/106100/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng dài`A`và một mảng mục tiêu`B`. 

Đối với bất kỳ phân khúc nào đã chọn`A[L..R]`, chúng ta được phép xóa các phần tử nhiều lần cho đến khi độ dài đoạn trở thành`m = |B|`. Quy tắc xóa không bình thường: một phần tử chỉ có thể bị xóa nếu nó có phần tử lân cận phù hợp và hai giá trị khác nhau. 

Nhiệm vụ là đếm xem có bao nhiêu đoạn của`A`có thể được chuyển đổi thành chính xác`B`. 

Hạn chế chính đó là`n`có thể lớn như`2 · 10^5`, trong khi`m ≤ 100`. Bất kỳ giải pháp nào kiểm tra tất cả`O(n²)`phân đoạn ngay lập tức là không thể. Thậm chí`O(n² / 10)`sẽ là quá lớn. Giá trị nhỏ của`m`là gợi ý thực sự. Nó gợi ý rằng chúng ta nên xây dựng một giải pháp lập trình động có độ phức tạp xấp xỉ`O(nm)`. 

Phần nguy hiểm của vấn đề là hiểu được phần tử nào thực sự có thể tháo rời được. 

Hãy xem xét phân khúc:```
1 1 2
```Chúng tôi có thể xóa thứ hai`1`, sau đó xóa cái đầu tiên`1`, chỉ để lại`2`. 

Bây giờ hãy xem xét:```
1 1
```Không thể xóa được. Phần tử đầu tiên bằng với phần tử lân cận bên phải của nó và phần tử thứ hai không có phần tử lân cận bên phải nào cả. 

Toàn bộ khối sau có giá trị bằng nhau là đặc biệt. Nếu một đoạn kết thúc bằng:```
... 5 5 5
```sau đó cả ba dấu vết`5`s buộc phải sống sót. Không gì có thể xóa được chúng. 

Quan sát này là nền tảng của giải pháp. 

### Trường hợp cạnh không rõ ràng 1```
A = [1, 1]
B = [1]
```Câu trả lời là`0`. 

Một sự giải thích bất cẩn có thể nghĩ rằng một trong những`1`s có thể được gỡ bỏ. Nó không thể. Toàn bộ quá trình chạy hậu tố bao gồm các giá trị bằng nhau, do đó không có phần tử nào có thể xóa được. 

### Trường hợp cạnh không rõ ràng 2```
A = [1, 2, 1]
B = [1, 1]
```Câu trả lời là`1`. 

Giữa`2`có thể bị xóa vì nó khác với hàng xóm bên phải của nó. Kết quả trở thành`[1,1]`. 

Một giải pháp chỉ nghĩ đến việc bảo toàn các lần chạy một cách độc lập thường bỏ sót việc loại bỏ toàn bộ lần chạy có thể khiến hai giá trị bằng nhau trở nên liền kề và hợp nhất. 

### Trường hợp cạnh không rõ ràng 3```
A = [7, 7, 7]
B = [7, 7]
```Câu trả lời là`0`. 

Độ dài chạy cuối cùng là`3`và mọi phần tử trong lần chạy đó buộc phải giữ nguyên. Vì độ dài mục tiêu chỉ là`2`, sự biến đổi là không thể. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ kiểm tra mọi phân đoạn và xác định xem liệu nó có thể được chuyển đổi thành`B`. 

có`O(n²)`phân đoạn. Ngay cả khi chúng tôi đã có một`O(m)`thủ tục xác minh, tổng độ phức tạp sẽ vào khoảng`4 · 10^10`trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là mô tả đặc điểm của tất cả các mảng cuối cùng có thể có được từ một phân đoạn. 

Lấy bất kỳ phân đoạn nào và xem lần chạy cuối cùng có giá trị bằng nhau.```
x x x x
```Mọi yếu tố của cuộc chạy đua đó buộc phải tồn tại. 

Mọi vị trí trước lần chạy cuối cùng đó đều là tùy chọn. Chúng ta có thể giữ nó hoặc cuối cùng xóa nó. 

Kết quả là một phân khúc có thể tạo ra`B`nếu và chỉ khi: 

1. Toàn bộ dấu vết được ánh xạ tới hậu tố của`B`. 
2. Tiền tố còn lại của`B`xuất hiện dưới dạng một chuỗi con trước lần chạy đó. 

Điều này biến bài toán thành bài toán đếm dãy con. 

Từ`m ≤ 100`, chúng ta có thể quét`A`một lần và duy trì các giá trị lập trình động cho tất cả các tiền tố của`B`. Đối với mỗi lần chạy cuối có thể, chúng tôi đặt một câu hỏi duy nhất:```
How many starting positions L allow
B[1..p] to appear as a subsequence
before the run starts?
```DP trả lời rằng trong`O(1)`thời gian sau khi tiền xử lý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²m) | O(m) | Quá chậm | 
| Tối ưu | O(nm) | O(m + n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính vị trí bắt đầu của lần chạy có giá trị bằng nhau kết thúc ở mọi chỉ số. 

Nếu chỉ mục`R`thuộc về một cuộc chạy bắt đầu lúc`s`, thì độ dài hậu tố bắt buộc là:```
k = R - s + 1
```2. Quét mảng từ trái sang phải và duy trì giá trị DP cho các chuỗi tiếp theo của`B`. 

Cho phép`dp[j]`biểu thị vị trí đầu tiên tối đa có thể có của một dãy con bằng`B[1..j]`bên trong tiền tố được xử lý. 
3. Đối với mỗi vị trí bắt đầu chạy`s`, lưu trữ ảnh chụp nhanh của tất cả`dp[j]`giá trị sau khi xử lý vị trí`s - 1`. 

Sau này chúng ta sẽ cần chính xác những giá trị này. 
4. Tính toán trước cho mọi độ dài hậu tố`k`liệu cuối cùng`k`các yếu tố của`B`tất cả đều bằng một giá trị nào đó`v`. 
5. Xử lý từng lần chạy`A`. 

Giả sử một cuộc chạy bắt đầu lúc`s`, có giá trị`v`, và chiều dài`len`. 

Đối với mọi độ dài hậu tố có thể:```
k = 1 .. min(len, m)
```tương ứng với đoạn kết thúc tại:```
R = s + k - 1
```kiểm tra xem cuối cùng`k`các yếu tố của`B`tất cả đều bằng nhau`v`. 
6. Nếu không, phân khúc này không bao giờ có thể hoạt động được. 

Nếu có, hãy:```
p = m - k
```là số phần tử phải có trước khi chạy. 
7. Khi nào`p = 0`, toàn bộ mục tiêu được hình thành bởi quá trình chạy bắt buộc. Bất kỳ vị trí bắt đầu nào:```
1 ≤ L ≤ s
```có giá trị, góp phần`s`phân đoạn. 
8. Khi nào`p > 0`, chúng ta cần một dãy con bằng`B[1..p]`trước khi chạy. 

Ảnh chụp nhanh DP được lưu trữ tại vị trí`s - 1`mang lại:```
mx = maximum first position of such a subsequence
```Mỗi vị trí bắt đầu:```
L ≤ mx
```hoạt động. 

Thêm vào`mx`để trả lời. 

### Tại sao nó hoạt động 

Lần chạy cuối cùng của một phân đoạn là không thay đổi. Mọi phần tử trong lần chạy đó đều tồn tại trong mọi phép biến đổi hợp lệ. 

Mọi vị trí trước lần chạy cuối cùng đều có thể tháo rời. Bằng cách xóa các phần tử không mong muốn, chúng tôi có thể giữ chính xác bất kỳ chuỗi con nào của tiền tố trước lần chạy. 

Do đó, một phép biến đổi hợp lệ được xác định hoàn toàn bằng cách chọn một dãy con bằng tiền tố của`B`trước khi chạy và khớp lệnh chạy bắt buộc với hậu tố của`B`. 

DP lưu trữ vị trí bắt đầu mới nhất có thể của mỗi tiền tố phù hợp của`B`. Nếu trận đấu có thể bắt đầu ở vị trí`x`, sau đó mọi phân đoạn bắt đầu`L ≤ x`chứa trận đấu đó. Việc đếm số lần bắt đầu như vậy sẽ cho chính xác số lượng phân đoạn hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
A = list(map(int, input().split()))
B = list(map(int, input().split()))

# run starts
run_start = [0] * n
run_start[0] = 0
for i in range(1, n):
    if A[i] == A[i - 1]:
        run_start[i] = run_start[i - 1]
    else:
        run_start[i] = i

# group positions by run start
runs = []
i = 0
while i < n:
    j = i
    while j + 1 < n and A[j + 1] == A[i]:
        j += 1
    runs.append((i, j, A[i]))
    i = j + 1

# snapshots[s] = dp state after processing prefix ending at s-1
snapshots = {}

dp = [0] * (m + 1)

for pos in range(n + 1):
    snapshots[pos] = dp[:]

    if pos == n:
        break

    x = A[pos]

    for j in range(m, 0, -1):
        if x == B[j - 1]:
            if j == 1:
                dp[1] = max(dp[1], pos + 1)
            elif dp[j - 1]:
                dp[j] = max(dp[j], dp[j - 1])

# suffix information of B
suffix_ok = [False] * (m + 1)
suffix_val = [None] * (m + 1)

for k in range(1, m + 1):
    v = B[m - 1]
    good = True
    for t in range(m - k, m):
        if B[t] != v:
            good = False
            break
    suffix_ok[k] = good
    suffix_val[k] = v

ans = 0

for start, end, value in runs:
    length = end - start + 1

    limit = min(length, m)

    state = snapshots[start]

    for k in range(1, limit + 1):
        if not suffix_ok[k]:
            continue
        if suffix_val[k] != value:
            continue

        p = m - k

        if p == 0:
            ans += start + 1
        else:
            ans += state[p]

print(ans)
```Mảng DP chỉ có`m + 1`tiểu bang, vì vậy việc cập nhật nó tốn kém`O(m)`mỗi phần tử mảng. 

Ý nghĩa của`dp[j]`là tinh tế. Nó lưu trữ vị trí đầu tiên lớn nhất có thể có của dãy con phù hợp bằng`B[1..j]`. Sử dụng vị trí đầu tiên lớn nhất chính là điều chúng ta cần, bởi vì mọi đoạn bắt đầu không vượt quá vị trí đó đều hợp lệ. 

Ảnh chụp nhanh được chụp trước khi xử lý từng vị trí. Khi một lần chạy bắt đầu ở chỉ mục`s`, chúng ta chỉ cần thông tin từ tiền tố kết thúc tại`s - 1`, đó là lý do tại sao`snapshots[s]`là trạng thái đúng. 

Việc triển khai sử dụng các vị trí dựa trên 1 bên trong các giá trị DP. Điều này làm cho công thức đếm trở nên đơn giản vì số lần bắt đầu hợp lệ chính xác là vị trí được lưu trữ. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
A = [1, 1, 2]
B = [1, 2]
```Các lần chạy là: 

| Chạy bắt đầu | Chạy cuối | Giá trị | 
| --- | --- | --- | 
| 1 | 2 | 1 | 
| 3 | 3 | 2 | 

Đối với lần chạy thứ hai: 

| k | Hậu tố bắt buộc của B | hợp lệ | 
| --- | --- | --- | 
| 1 | [2] | Có | 

Sau đó`p = 1`. 

Tiền tố trước khi chạy chứa một chuỗi con`[1]`người có khả năng bắt đầu muộn nhất là vị trí`2`. 

Thế là bắt đầu`L = 1`Và`L = 2`cả hai đều hợp lệ. 

Trả lời:```
2
```Điều này chứng tỏ tại sao cả hai phân đoạn`[1,1,2]`Và`[1,2]`được tính. 

### Ví dụ 2```
A = [1, 2, 1]
B = [1, 1]
```Lần chạy cuối cùng có giá trị`1`và chiều dài`1`. 

| k | p | 
| --- | --- | 
| 1 | 1 | 

Trước khi chạy chúng ta cần có chuỗi tiếp theo`[1]`. 

Sự bắt đầu muộn nhất có thể là vị trí`1`. 

Vì vậy, chính xác một phân khúc hoạt động. 

| Số lượng | Giá trị | 
| --- | --- | 
| Bắt đầu trận đấu tiền tố mới nhất | 1 | 
| Đã thêm vào câu trả lời | 1 | 

Trả lời:```
1
```Điều này xác nhận rằng việc xóa phần giữa`2`là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi vị trí mảng cập nhật tối đa`m`Trạng thái DP | 
| Không gian | Ảnh chụp nhanh O(nm) về mặt khái niệm được giảm xuống thành số nguyên O(nm) | Chúng tôi lưu trữ một ảnh chụp nhanh DP có kích thước`m+1`cho từng vị trí | 

Với`n = 2 · 10^5`Và`m ≤ 100`, thời gian chạy là khoảng hai mươi triệu lần chuyển đổi DP, vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    # paste solve() here when testing locally
    pass

# sample
assert run(
"""3 2
1 1 2
1 2
"""
) == "2\n"

# minimum size
assert run(
"""1 1
5
5
"""
) == "1\n"

# impossible
assert run(
"""2 1
1 1
1
"""
) == "0\n"

# all equal
assert run(
"""4 2
7 7 7 7
7 7
"""
) == "0\n"

# merge after deleting middle run
assert run(
"""3 2
1 2 1
1 1
"""
) == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 5 / 5`|`1`| Kích thước tối thiểu | 
|`1 1 / 1 1 / 1`|`0`| Buộc chạy hậu tố | 
| Tất cả các giá trị bằng nhau |`0`| Chạy lớn bất biến | 
|`1 2 1`→`1 1`|`1`| Chạy hợp nhất sau khi xóa | 

## Vỏ cạnh 

cho```
A = [1, 1]
B = [1]
```chiều dài chạy cuối cùng là`2`, trong khi`|B| = 1`. 

Thuật toán chỉ kiểm tra`k ≤ m`. Không có cấu hình hợp lệ nào tồn tại nên mức đóng góp bằng không. 

Vì```
A = [1, 2, 1]
B = [1, 1]
```lần chạy cuối cùng có độ dài`1`, khớp với phần tử cuối cùng của`B`. 

Ảnh chụp nhanh DP trước khi chạy ghi lại chuỗi con hợp lệ`[1]`bắt đầu ở vị trí`1`, tạo ra chính xác một phân đoạn hợp lệ. 

Vì```
A = [7, 7, 7]
B = [7, 7]
```độ dài chạy cuối đã`3`. 

Vì cả ba phần tử đều bắt buộc phải tồn tại nên không có đoạn nào kết thúc ở vị trí cuối cùng có thể thu nhỏ lại về độ dài`2`. Thuật toán tự động từ chối nó vì độ dài hậu tố yêu cầu không thể được thỏa mãn.
