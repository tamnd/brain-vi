---
title: "CF 18C - Sọc"
description: "Chúng ta có một dải ô và mỗi ô chứa một số nguyên. Chúng ta chỉ có thể cắt dải giữa các ô liền kề để chia mảng thành phần bên trái và phần bên phải. Nhiệm vụ là đếm xem có bao nhiêu vị trí cắt tạo ra hai phần không trống có tổng bằng nhau."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 18
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 18 (Div. 2 Only)"
rating: 1200
weight: 18
solve_time_s: 103
verified: true
draft: false
---
[CF 18C - Sọc](https://codeforces.com/problemset/problem/18/C) 

**Đánh giá:** 1200 
**Tags:** cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 1m 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dải ô và mỗi ô chứa một số nguyên. Chúng ta chỉ có thể cắt dải giữa các ô liền kề để chia mảng thành phần bên trái và phần bên phải. Nhiệm vụ là đếm xem có bao nhiêu vị trí cắt tạo ra hai phần không trống có tổng bằng nhau. 

Ví dụ: nếu mảng là:```
[1, 2, 3, 3]
```cắt sau phần tử thứ hai cho:```
[1, 2] | [3, 3]
```Tổng bên trái là`3`, tổng đúng là`6`, vì vậy việc cắt này không hợp lệ. 

Kích thước đầu vào đạt`10^5`, điều này ngay lập tức loại trừ các thuật toán liên tục tính lại tổng cho mỗi lần cắt. Một giải pháp bậc hai sẽ thực hiện gần như`10^10`trong trường hợp xấu nhất, vượt xa giới hạn 2 giây. Độ phức tạp mục tiêu là thời gian tuyến tính hoặc thứ gì đó rất gần với nó. 

Các con số cũng có thể âm nên chúng ta không thể dựa vào hành vi đơn điệu. Tổng tiền tố có thể tăng hoặc giảm tùy ý. Bất kỳ giải pháp nào dựa trên hai con trỏ hoặc khai triển tham lam sẽ thất bại vì tổng không di chuyển theo hướng có thể dự đoán được. 

Có một số trường hợp khó bỏ sót. 

Hãy xem xét một yếu tố duy nhất:```
1
5
```Không có phần cắt hợp lệ vì cả hai phần phải chứa ít nhất một phần tử. Câu trả lời đúng là:```
0
```Việc triển khai bất cẩn để kiểm tra tất cả các chỉ số từ`0`ĐẾN`n`có thể vô tình đếm các phân vùng trống. 

Một trường hợp phức tạp khác xuất hiện khi tổng số tiền là số lẻ:```
4
1 2 3 5
```Tổng cộng là`11`. Không thể có hai nửa số nguyên bằng nhau nên đáp án phải là:```
0
```Một số triển khai vẫn quét tất cả các vết cắt mặc dù không có giải pháp nào có thể tồn tại. 

Giá trị âm là một nguồn sai lầm khác:```
5
2 -2 2 -2 0
```Tổng số tiền là`0`, và có một số vết cắt hợp lệ. Bất kỳ cách tiếp cận nào giả định tổng tiền tố chỉ tăng lên sẽ bị hỏng ở đây. 

Cuối cùng, bản thân vị trí cắt cũng quan trọng. Chúng ta không thể cắt sau phần tử cuối cùng vì vế phải sẽ trống. Ví dụ:```
2
1 -1
```Tổng số tiền là`0`, nhưng chỉ có thể cắt một lần, sau phần tử đầu tiên:```
[1] | [-1]
```Tổng không bằng nhau nên đáp án là`0`. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là thử mọi cách cắt có thể và tính toán tổng của phần bên trái và bên phải một cách độc lập. 

Giả sử chúng ta cắt sau chỉ mục`i`. Chúng ta có thể tính toán:```
left_sum  = sum(a[0..i])
right_sum = sum(a[i+1..n-1])
```Nếu tổng khớp nhau, chúng ta sẽ tăng câu trả lời. 

Cách tiếp cận này đúng vì nó kiểm tra rõ ràng mọi phân vùng hợp pháp. Vấn đề là hiệu quả. Tính tổng trên một phạm vi mất`O(n)`thời gian nếu được thực hiện một cách ngây thơ, và có`n - 1`những vết cắt có thể xảy ra. Tổng độ phức tạp trở thành`O(n^2)`. 

Với`n = 100000`, điều này có nghĩa đại khái là:```
100000 * 100000 = 10^10
```hoạt động trong trường hợp xấu nhất là quá chậm. 

Quan sát quan trọng là các vết cắt lân cận chồng lên nhau rất nhiều. Khi chúng ta di chuyển hình cắt sang bên phải một bước, hầu hết tất cả các phần tử vẫn nằm trong cùng một phân vùng. Việc tính lại số tiền từ đầu thật lãng phí. 

Điều này tự nhiên gợi ý tổng tiền tố. 

Nếu chúng ta biết tổng của toàn bộ mảng thì để cắt sau chỉ mục`i`:```
left_sum  = prefix[i]
right_sum = total_sum - prefix[i]
```Điều kiện trở thành:```
prefix[i] == total_sum - prefix[i]
```mà đơn giản hóa thành:```
2 * prefix[i] == total_sum
```Bây giờ mỗi lần cắt có thể được kiểm tra trong thời gian không đổi. Chúng tôi quét mảng một lần, duy trì tổng tiền tố đang chạy và đếm xem có bao nhiêu vị trí thỏa mãn đẳng thức. 

Điều này làm giảm độ phức tạp từ bậc hai sang tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và tính tổng của nó. 

Chúng ta cần tổng vì mọi tổng vế phải đều có thể viết là:```
total_sum - left_sum
```2. Khởi tạo tổng tiền tố đang chạy như`0`và câu trả lời như`0`. 

Tổng tiền tố biểu thị tổng của phân vùng bên trái cho phần cắt hiện tại. 
3. Lặp lại mảng từ chỉ mục`0`ĐẾN`n - 2`. 

Chúng tôi dừng lại ở`n - 2`vì vết cắt phải để lại ít nhất một phần tử ở phía bên phải. 
4. Thêm phần tử hiện tại vào tổng tiền tố. 

Sau khi thêm`a[i]`, tổng tiền tố bằng tổng của phân vùng bên trái nếu chúng ta cắt sau chỉ mục`i`. 
5. Kiểm tra xem:```
prefix_sum * 2 == total_sum
```Nếu điều kiện này đúng thì phân vùng bên trái và bên phải có tổng bằng nhau. 
6. Nếu điều kiện đúng, hãy tăng câu trả lời. 
7. Sau khi xử lý tất cả các vị trí cắt hợp lệ, hãy in câu trả lời. 

### Tại sao nó hoạt động 

Ở mỗi bước, tổng tiền tố đang chạy bằng tổng của phân vùng bên trái được tạo bằng cách cắt sau chỉ mục hiện tại. Phân vùng bên phải phải có tổng:```
total_sum - prefix_sum
```Việc cắt giảm có hiệu lực chính xác khi:```
prefix_sum == total_sum - prefix_sum
```Thuật toán kiểm tra điều kiện này cho mọi vị trí cắt hợp lệ đúng một lần. Vì mọi lần cắt có thể đều được kiểm tra và kiểm tra tính bằng nhau về mặt toán học tương đương với yêu cầu ban đầu nên thuật toán không thể bỏ sót một lần cắt hợp lệ hoặc đếm một lần cắt không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

total_sum = sum(a)

prefix_sum = 0
answer = 0

for i in range(n - 1):
    prefix_sum += a[i]

    if prefix_sum * 2 == total_sum:
        answer += 1

print(answer)
```Phần đầu tiên đọc đầu vào và tính tổng của mảng. Điều này cho phép chúng ta truy cập ngay vào tổng bên phải cho mỗi lần cắt. 

Biến`prefix_sum`lưu trữ tổng hoạt động của phân vùng bên trái. Khi chúng ta lặp qua mảng, mỗi phần tử mới sẽ trở thành một phần của vế trái. 

Vòng lặp chỉ chạy cho đến khi`n - 2`. Chi tiết này quan trọng. Nếu chúng tôi cho phép`i = n - 1`, phân vùng bên phải sẽ trống, điều này vi phạm báo cáo vấn đề. 

Điều kiện:```
prefix_sum * 2 == total_sum
```tránh hoàn toàn sự phân chia. Điều này sạch hơn và cũng tránh được các vấn đề với tổng số lẻ. Nếu tổng là số lẻ thì không có tổng tiền tố số nguyên nào có thể thỏa mãn đẳng thức, do đó điều kiện đương nhiên không thành công. 

Số nguyên Python tự động xử lý các giá trị lớn một cách an toàn, do đó việc tràn không phải là vấn đề đáng lo ngại ở đây. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, vẫn nên sử dụng loại 64 bit. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9
1 5 -6 7 9 -16 0 -2 2
```Tổng số tiền là`0`. 

| tôi | một [tôi] | tiền tố_sum | đúng_sum | Có hiệu lực? | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | -1 | Không | 0 | 
| 1 | 5 | 6 | -6 | Không | 0 | 
| 2 | -6 | 0 | 0 | Có | 1 | 
| 3 | 7 | 7 | -7 | Không | 1 | 
| 4 | 9 | 16 | -16 | Không | 1 | 
| 5 | -16 | 0 | 0 | Có | 2 | 
| 6 | 0 | 0 | 0 | Có | 3 | 
| 7 | -2 | -2 | 2 | Không | 3 | 

Câu trả lời cuối cùng:```
3
```Dấu vết này cho thấy tại sao tổng tiền tố là đủ. Tại mỗi lần cắt, toàn bộ quyết định chỉ phụ thuộc vào tổng số tiền đang chạy cho đến thời điểm đó. 

### Ví dụ 2 

đầu vào:```
5
2 1 1 2 2
```Tổng số tiền là`8`. 

| tôi | một [tôi] | tiền tố_sum | đúng_sum | Có hiệu lực? | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 2 | 6 | Không | 0 | 
| 1 | 1 | 3 | 5 | Không | 0 | 
| 2 | 1 | 4 | 4 | Có | 1 | 
| 3 | 2 | 6 | 2 | Không | 1 | 

Câu trả lời cuối cùng:```
1
```Ví dụ này minh họa trường hợp số dương tiêu chuẩn. Chỉ một lần cắt chia mảng thành hai nửa bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mảng được quét một lần | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Quét tuyến tính`100000`các yếu tố dễ dàng phù hợp trong thời hạn. Việc sử dụng bộ nhớ không đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    total_sum = sum(a)

    prefix_sum = 0
    answer = 0

    for i in range(n - 1):
        prefix_sum += a[i]

        if prefix_sum * 2 == total_sum:
            answer += 1

    print(answer)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output.strip()

# provided sample
assert run(
"""9
1 5 -6 7 9 -16 0 -2 2
"""
) == "3", "sample 1"

# minimum size
assert run(
"""1
5
"""
) == "0", "single element"

# all zeros
assert run(
"""5
0 0 0 0 0
"""
) == "4", "every cut works"

# odd total sum
assert run(
"""4
1 2 3 5
"""
) == "0", "odd total"

# negative numbers
assert run(
"""5
2 -2 2 -2 0
"""
) == "3", "negative values"

# off-by-one boundary
assert run(
"""2
1 -1
"""
) == "0", "cannot cut after last element"

# balanced split in middle
assert run(
"""6
1 1 1 1 1 1
"""
) == "1", "single valid split"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 5`|`0`| Không thể cắt | 
|`0 0 0 0 0`|`4`| Mỗi lần cắt đều hợp lệ | 
|`1 2 3 5`|`0`| Tổng số lẻ | 
|`2 -2 2 -2 0`|`3`| Giá trị âm và tổng bằng 0 | 
|`1 -1`|`0`| Ngăn chặn phân vùng trống bên phải | 
|`1 1 1 1 1 1`|`1`| Chia cân đối chuẩn | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
5
```Vòng lặp chạy qua:```
range(n - 1) = range(0)
```vì vậy không có sự lặp lại xảy ra. Câu trả lời vẫn ở lại`0`, điều này đúng vì chúng ta không thể chia một phần tử thành hai phần không trống. 

Bây giờ hãy xem xét tổng số lẻ:```
4
1 2 3 5
```Tổng số tiền là`11`. 

Trong quá trình quét: 

| tôi | tiền tố_sum | tiền tố_sum * 2 | 
| --- | --- | --- | 
| 0 | 1 | 2 | 
| 1 | 3 | 6 | 
| 2 | 6 | 12 | 

Không bằng`11`, vậy đáp án vẫn là`0`. Thuật toán sẽ loại bỏ một cách tự nhiên các trường hợp không thể thực hiện được nếu không có biện pháp xử lý đặc biệt. 

Tiếp theo, hãy xem xét các giá trị âm:```
5
2 -2 2 -2 0
```Tổng số tiền là`0`. 

Tổng tiền tố đang chạy trở thành:```
2, 0, 2, 0
```Bất cứ khi nào tổng tiền tố là`0`, hậu tố còn lại cũng có tổng là`0`, do đó những vết cắt đó được tính chính xác. Thuật toán không bao giờ giả định tổng tăng một cách đơn điệu, vì vậy số âm không gây ra vấn đề gì. 

Cuối cùng, xét điều kiện biên:```
2
1 -1
```Sự cắt giảm hợp pháp duy nhất là sau chỉ số`0`. 

Tổng tiền tố trở thành`1`, trong khi vế phải là`-1`, vì vậy điều kiện không thành công và câu trả lời là`0`. 

Thuật toán không bao giờ kiểm tra phần cắt sau phần tử cuối cùng vì vòng lặp dừng ở`n - 2`. Điều này tránh việc vô tình đếm một phân vùng trống bên phải.
