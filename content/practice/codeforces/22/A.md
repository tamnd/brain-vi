---
title: "CF 22A - Thống kê bậc hai"
description: "Chúng ta được cho một mảng nhỏ các số nguyên và cần tìm giá trị nhỏ nhất lớn hơn phần tử tối thiểu trong mảng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 22
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 22 (Div. 2 Only)"
rating: 800
weight: 22
solve_time_s: 81
verified: true
draft: false
---
[CF 22A - Thống kê bậc hai](https://codeforces.com/problemset/problem/22/A) 

**Đánh giá:** 800 
**Tags:** vũ lực 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng nhỏ các số nguyên và cần tìm giá trị nhỏ nhất lớn hơn phần tử tối thiểu trong mảng. 

Một cách khác để suy nghĩ về điều này là: nếu chúng ta loại bỏ các giá trị trùng lặp, sắp xếp các số còn lại và xem phần tử thứ hai, thì giá trị đó chính là câu trả lời. Nếu giá trị đó không tồn tại, chúng tôi in`NO`. 

Ví dụ: trong mảng:```
1 2 2 -4
```giá trị tối thiểu là`-4`. Số nhỏ nhất lớn hơn`-4`là`1`, vậy câu trả lời là`1`. 

Những hạn chế là rất nhỏ. Độ dài mảng tối đa là 100, vì vậy ngay cả các giải pháp bậc hai cũng hoàn toàn an toàn. MỘT`O(n^2)`thuật toán sẽ thực hiện tối đa 10.000 phép so sánh, không đáng kể trong giới hạn thời gian 2 giây. Việc sử dụng bộ nhớ cũng không liên quan ở đây vì đầu vào rất nhỏ. 

Phần khó khăn không phải là hiệu suất, mà là xử lý các bản sao một cách chính xác. Vấn đề yêu cầu số nhỏ nhất lớn hơn số tối thiểu, không phải phần tử thứ hai sau khi sắp xếp toàn bộ mảng. 

Hãy xem xét đầu vào này:```
5
1 1 1 2 3
```Câu trả lời đúng là:```
2
```Nếu chúng ta chỉ cần sắp xếp mảng và lấy phần tử ở chỉ số 1, chúng ta sẽ nhận được`1`, điều này sai vì các bản sao tối thiểu không được tính. 

Một trường hợp cạnh quan trọng khác xảy ra khi tất cả các giá trị đều bằng nhau:```
4
7 7 7 7
```Không có số nào lớn hơn mức tối thiểu, vì vậy kết quả đúng là:```
NO
```Việc triển khai bất cẩn vẫn có thể cố gắng truy cập phần tử thứ hai sau khi loại bỏ các phần tử trùng lặp và gặp sự cố hoặc in sai`7`. 

Giá trị âm cũng quan trọng:```
5
-10 -5 -10 -3
```Tối thiểu là`-10`và giá trị nhỏ nhất lớn hơn nó`-5`. Logic sắp xếp cũng phải hoạt động chính xác đối với số âm. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ trực tiếp nhất là so sánh mọi phần tử với mọi phần tử khác. Đầu tiên chúng ta có thể tìm giá trị nhỏ nhất, sau đó quét lại mảng và tìm kiếm số nhỏ nhất lớn hơn giá trị nhỏ nhất đó. Điều này đã hoạt động trong thời gian tuyến tính. 

Một phiên bản mạnh mẽ hơn theo nghĩa đen sẽ tạo ra tất cả các giá trị riêng biệt, sắp xếp chúng và lấy giá trị thứ hai. Vì mảng chứa tối đa 100 phần tử nên việc sắp xếp cũng hoàn toàn ổn. Sắp xếp 100 số tốn khoảng`100 log 100`, nó rất nhỏ. 

Phần thú vị của vấn đề là hiểu "thống kê bậc hai" thực sự có ý nghĩa gì ở đây. Nó không có nghĩa là vị trí thứ hai trong mảng ban đầu được sắp xếp. Nó có nghĩa là giá trị khác biệt thứ hai sau khi sắp xếp. 

Quan sát đó ngay lập tức gợi ý một giải pháp rõ ràng: 

1. Xóa các bản sao. 
2. Sắp xếp các giá trị còn lại. 
3. Nếu tồn tại ít hơn hai giá trị phân biệt, hãy in`NO`. 
4. Nếu không thì in giá trị thứ hai. 

Brute-force hoạt động vì đầu vào rất nhỏ, nhưng việc quan sát giá trị riêng biệt giúp việc triển khai trở nên rõ ràng hơn và ít xảy ra lỗi hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`và các giá trị mảng. 
2. Chuyển đổi mảng thành một tập hợp để loại bỏ các bản sao. 

Bước này rất cần thiết vì việc lặp lại giá trị tối thiểu sẽ không ảnh hưởng đến câu trả lời. 
3. Chuyển tập hợp trở lại thành danh sách và sắp xếp theo thứ tự tăng dần. 

Sau khi sắp xếp, phần tử đầu tiên là giá trị khác biệt nhỏ nhất và phần tử thứ hai, nếu tồn tại, là câu trả lời chúng ta cần. 
4. Kiểm tra xem có bao nhiêu giá trị khác biệt tồn tại. 

Nếu kích thước nhỏ hơn 2 thì không có thống kê bậc hai hợp lệ. 
5. Nếu có ít nhất hai giá trị riêng biệt, hãy in phần tử tại chỉ mục`1`. 

Vì mảng đã được sắp xếp nên đây là giá trị nhỏ nhất lớn hơn giá trị tối thiểu. 

### Tại sao nó hoạt động 

Sau khi loại bỏ các số trùng lặp, mỗi số còn lại biểu thị một giá trị riêng biệt so với mảng ban đầu. Sắp xếp các giá trị này sẽ đặt chúng theo thứ tự tăng dần. Giá trị đầu tiên là phần tử riêng biệt tối thiểu và giá trị tiếp theo chính xác là phần tử riêng biệt nhỏ nhất lớn hơn nó. Vì định nghĩa bài toán khớp trực tiếp với thuộc tính này nên thuật toán luôn tạo ra câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
arr = list(map(int, input().split()))

distinct = sorted(set(arr))

if len(distinct) < 2:
    print("NO")
else:
    print(distinct[1])
```Giải pháp bắt đầu bằng cách đọc mảng bình thường bằng cách sử dụng đầu vào nhanh. I/O nhanh thực sự không cần thiết cho những hạn chế này, nhưng đó là thông lệ tiêu chuẩn trong lập trình cạnh tranh. 

Dòng chính là:```
distinct = sorted(set(arr))
```các`set`tự động loại bỏ các giá trị trùng lặp. Điều này ngăn chặn các giá trị tối thiểu lặp đi lặp lại trở thành câu trả lời không chính xác. Sau đó,`sorted`sắp xếp các số còn lại theo thứ tự tăng dần. 

Việc kiểm tra độ dài rất quan trọng vì các mảng như:```
5 5 5 5
```chỉ tạo ra một giá trị riêng biệt sau khi loại bỏ trùng lặp. Truy cập chỉ mục`1`không kiểm tra sẽ gây ra lỗi. 

Cuối cùng, nếu tồn tại ít nhất hai giá trị phân biệt thì số phân biệt nhỏ thứ hai chính xác là`distinct[1]`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2 2 -4
```| Bước | Giá trị | 
| --- | --- | 
| Mảng gốc |`[1, 2, 2, -4]`| 
| Sau khi loại bỏ trùng lặp |`{1, 2, -4}`| 
| Sau khi sắp xếp |`[-4, 1, 2]`| 
| Giá trị khác biệt thứ hai |`1`| 

Giá trị khác biệt tối thiểu là`-4`. Giá trị khác biệt lớn hơn tiếp theo là`1`, phù hợp với định nghĩa được yêu cầu. 

### Ví dụ 2 

đầu vào:```
5
7 7 7 7 7
```| Bước | Giá trị | 
| --- | --- | 
| Mảng gốc |`[7, 7, 7, 7, 7]`| 
| Sau khi loại bỏ trùng lặp |`{7}`| 
| Sau khi sắp xếp |`[7]`| 
| Số giá trị riêng biệt |`1`| 
| Đầu ra |`NO`| 

Ví dụ này minh họa trường hợp cạnh quan trọng trong đó không tồn tại giá trị phân biệt thứ hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các giá trị riêng biệt chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n) | Danh sách tập hợp và sắp xếp lưu trữ các phần tử riêng biệt | 

Với`n ≤ 100`, giải pháp này dễ dàng nằm trong giới hạn. Ngay cả những cách tiếp cận chậm hơn nhiều vẫn sẽ trôi qua một cách thoải mái. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    arr = list(map(int, input().split()))

    distinct = sorted(set(arr))

    if len(distinct) < 2:
        print("NO")
    else:
        print(distinct[1])

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
assert run("4\n1 2 2 -4\n") == "1", "sample 1"

# minimum size input
assert run("1\n5\n") == "NO", "single element"

# all values equal
assert run("5\n7 7 7 7 7\n") == "NO", "all equal"

# negative numbers
assert run("5\n-10 -5 -10 -3 -5\n") == "-5", "negative values"

# already sorted distinct values
assert run("4\n1 2 3 4\n") == "2", "simple increasing"

# duplicates of minimum
assert run("6\n1 1 1 2 3 4\n") == "2", "duplicate minimum"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 5`|`NO`| Kích thước mảng tối thiểu có thể | 
|`5 / 7 7 7 7 7`|`NO`| Tất cả các giá trị giống nhau | 
|`5 / -10 -5 -10 -3 -5`|`-5`| Xử lý đúng số âm | 
|`4 / 1 2 3 4`|`2`| Trường hợp tăng cơ bản khác biệt | 
|`6 / 1 1 1 2 3 4`|`2`| Các bản sao tối thiểu bị bỏ qua | 

## Vỏ cạnh 

Hãy xem xét trường hợp tất cả các giá trị đều bằng nhau:```
4
7 7 7 7
```Thuật toán chuyển đổi mảng thành một tập hợp:```
{7}
```Sau khi sắp xếp:```
[7]
```Số lượng giá trị riêng biệt chỉ là 1 nên thuật toán in ra:```
NO
```Điều này đúng vì không có giá trị nào lớn hơn mức tối thiểu. 

Bây giờ hãy xem xét các giá trị tối thiểu lặp lại:```
6
1 1 1 2 3 4
```Giá trị tối thiểu xuất hiện nhiều lần. Sau khi loại bỏ trùng lặp:```
{1, 2, 3, 4}
```Sắp xếp mang lại:```
[1, 2, 3, 4]
```Đầu ra của thuật toán`2`, đó là giá trị nhỏ nhất hoàn toàn lớn hơn`1`. Một cách tiếp cận ngây thơ chỉ đơn giản là sắp xếp mảng ban đầu và lấy phần tử thứ hai sẽ trả về không chính xác`1`. 

Cuối cùng, hãy xem xét số âm:```
5
-10 -5 -10 -3
```Sau khi loại bỏ các bản sao và sắp xếp:```
[-10, -5, -3]
```Giá trị khác biệt thứ hai là`-5`. Thuật toán hoạt động chính xác vì việc sắp xếp số nguyên xử lý các giá trị âm một cách tự nhiên.
