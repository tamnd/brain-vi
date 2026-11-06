---
title: "CF 130I - Sắp xếp mảng"
description: "Chúng ta được cung cấp một mảng có tối đa 100 số nguyên. Mọi giá trị nằm trong khoảng từ 1 đến 60. Nhiệm vụ đơn giản là in mảng theo thứ tự không giảm."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "sortings"]
categories: ["algorithms"]
codeforces_contest: 130
codeforces_index: "I"
codeforces_contest_name: "Unknown Language Round 4"
rating: 2300
weight: 130
solve_time_s: 107
verified: true
draft: false
---

[CF 130I - Sắp xếp mảng](https://codeforces.com/problemset/problem/130/I) 

**Đánh giá:** 2300 
**Thẻ:** *đặc biệt, phân loại 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng có tối đa 100 số nguyên. Mọi giá trị nằm trong khoảng từ 1 đến 60. Nhiệm vụ đơn giản là in mảng theo thứ tự không giảm. 

Thoạt nhìn, đây có vẻ giống như một vấn đề sắp xếp tiêu chuẩn, nhưng phạm vi giá trị nhỏ bất thường sẽ thay đổi hoàn toàn cấu trúc của vấn đề. Thay vì nghĩ đến việc sắp xếp lại các số tùy ý, chúng ta có thể nghĩ đến việc đếm xem mỗi giá trị xuất hiện bao nhiêu lần. 

Độ dài mảng rất nhỏ. Thậm chí một$O(n^2)$thuật toán sắp xếp như sắp xếp bong bóng hoặc sắp xếp chèn sẽ phù hợp thoải mái trong các giới hạn vì$100^2 = 10{,}000$hoạt động là không đáng kể. Phạm vi giá trị thậm chí còn nhỏ hơn kích thước mảng, chỉ có 60 giá trị riêng biệt có thể. Điều đó mở ra cơ hội cho một giải pháp dựa trên việc đếm chạy theo thời gian tuyến tính đối với kích thước đầu vào. 

Các trường hợp cạnh chính đến từ các giá trị trùng lặp và từ các giá trị xuất hiện ở ranh giới của phạm vi cho phép. 

Hãy xem xét đầu vào này:```
5
7
7
7
7
7
```Đầu ra đúng là:```
7 7 7 7 7
```Việc triển khai bất cẩn khi lưu trữ các giá trị trong một tập hợp sẽ loại bỏ các giá trị trùng lặp và chỉ in một giá trị`7`. 

Một sai lầm dễ mắc phải khác là quên xử lý chính xác giá trị nhỏ nhất hoặc lớn nhất.```
4
1
60
1
60
```Đầu ra đúng là:```
1 1 60 60
```Nếu mảng đếm được tạo với kích thước 60 thay vì 61, việc truy cập chỉ mục`60`sẽ thất bại hoặc âm thầm tạo ra kết quả sai tùy thuộc vào ngôn ngữ. 

Kích thước đầu vào tối thiểu cũng có vấn đề:```
1
42
```Đầu ra phải là:```
42
```Một số triển khai vô tình in thêm khoảng trắng hoặc quên in bất cứ thứ gì khi chỉ tồn tại một phần tử. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là sử dụng cách sắp xếp so sánh tiêu chuẩn. Chúng tôi liên tục so sánh các phần tử và di chuyển các giá trị nhỏ hơn về phía trước. Sắp xếp bong bóng, sắp xếp chèn hoặc sắp xếp lựa chọn đều hoạt động vì sắp xếp theo so sánh đảm bảo thứ tự mảng cuối cùng là chính xác. 

Vì$n = 100$, thậm chí$O(n^2)$sự phức tạp là tốt. Sắp xếp bong bóng hoạt động tối đa khoảng$100 \times 100 = 10{,}000$so sánh, điều này là tầm thường đối với giới hạn 2 giây. 

Phần thú vị của vấn đề này là phạm vi giá trị giới hạn. Mỗi số nằm trong khoảng từ 1 đến 60, nghĩa là chỉ có 60 giá trị riêng biệt có thể có bất kể kích thước đầu vào. Thay vì sắp xếp các phần tử một cách trực tiếp, chúng ta có thể đếm số lần mỗi giá trị xuất hiện. 

Quan sát là nếu chúng ta biết giá trị`x`xuất hiện`k`lần thì mảng được sắp xếp phải chứa chính xác`k`bản sao của`x`và tất cả chúng phải xuất hiện trước bất kỳ giá trị lớn hơn nào. 

Điều này biến bài toán thành tần số đếm và xây dựng lại chuỗi đã sắp xếp theo thứ tự tăng dần. Thuật toán trở thành: 

1. Đếm số lần xuất hiện của mỗi số. 
2. Lặp lại từ 1 đến 60. 
3. In mỗi số bao nhiêu lần khi nó xuất hiện. 

Đây là loại đếm chuyên biệt cho một phạm vi rất nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n + 60) | O(60) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`, số phần tử trong mảng. 
2. Tạo mảng tần số`cnt`có kích thước 61 được khởi tạo bằng số không. 

chỉ mục`i`sẽ lưu trữ giá trị bao nhiêu lần`i`xuất hiện. Chúng tôi sử dụng kích thước 61 để chỉ số`1`bởi vì`60`có thể sử dụng trực tiếp. 
3. Đọc từng phần tử mảng`x`. 
4. Tăng`cnt[x]`. 

Sau khi xử lý tất cả các số,`cnt[v]`bằng tần số của giá trị`v`trong mảng đầu vào. 
5. Tạo danh sách câu trả lời trống. 
6. Lặp lại các giá trị từ`1`ĐẾN`60`. 
7. Với mỗi giá trị`v`, thêm nó vào danh sách câu trả lời một cách chính xác`cnt[v]`lần. 

Vì chúng tôi xử lý các giá trị theo thứ tự tăng dần nên trình tự được xây dựng lại sẽ tự động được sắp xếp. 
8. In danh sách câu trả lời dưới dạng số nguyên được phân tách bằng dấu cách. 

### Tại sao nó hoạt động 

Thuật toán duy trì bất biến sau khi xử lý giá trị`v`, danh sách câu trả lời chứa mọi phần tử đầu vào nhỏ hơn hoặc bằng`v`, theo thứ tự được sắp xếp, với bội số chính xác. 

Vì mỗi lần xuất hiện của một giá trị đều được tính chính xác một lần và sau đó được sao chép chính xác một lần nên không có phần tử nào bị mất hoặc trùng lặp. Vì các giá trị được thêm vào theo thứ tự tăng dần nên không có số lớn hơn nào có thể xuất hiện trước số nhỏ hơn. Điều đó đảm bảo chuỗi cuối cùng là mảng được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

cnt = [0] * 61

for _ in range(n):
    x = int(input())
    cnt[x] += 1

ans = []

for value in range(1, 61):
    ans.extend([value] * cnt[value])

print(*ans)
```Phần đầu tiên đọc đầu vào và xây dựng mảng tần số. Vì các giá trị nằm trong phạm vi từ 1 đến 60 nên danh sách có kích thước 61 là đủ. Chỉ mục 0 không được sử dụng, giúp việc lập chỉ mục đơn giản và tránh bị trừ 1 từ mọi giá trị. 

Giai đoạn tái thiết lặp lại các giá trị theo thứ tự tăng dần. Đối với mỗi giá trị, chúng tôi thêm chính xác số lần xuất hiện trong đầu vào. Biểu thức:```
[value] * cnt[value]
```tạo một danh sách chứa các bản sao lặp lại của giá trị hiện tại. 

sử dụng`extend`thay vì lặp lại`append`các cuộc gọi giữ cho việc thực hiện ngắn gọn. 

trận chung kết`print(*ans)`tự động in các phần tử được phân tách bằng dấu cách, phù hợp với định dạng đầu ra được yêu cầu. Điều này cũng tránh được các vấn đề về dấu cách. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
7
1
9
7
3
```Xây dựng tần số: 

| Đọc giá trị | cnt[1] | cnt[3] | cnt[7] | cnt[9] | 
| --- | --- | --- | --- | --- | 
| 7 | 0 | 0 | 1 | 0 | 
| 1 | 1 | 0 | 1 | 0 | 
| 9 | 1 | 0 | 1 | 1 | 
| 7 | 1 | 0 | 2 | 1 | 
| 3 | 1 | 1 | 2 | 1 | 

Tái thiết: 

| Giá trị hiện tại | Tần số | Trả lời | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 0 | 1 | 
| 3 | 1 | 1 3 | 
| 4..6 | 0 | 1 3 | 
| 7 | 2 | 1 3 7 7 | 
| 8 | 0 | 1 3 7 7 | 
| 9 | 1 | 1 3 7 7 9 | 

Đầu ra cuối cùng:```
1 3 7 7 9
```Dấu vết này cho thấy các bản sao được lưu giữ chính xác như thế nào vì mảng tần số lưu trữ số lượng thay vì chỉ hiện diện. 

### Ví dụ 2 

đầu vào:```
6
60
1
60
2
1
2
```Xây dựng tần số: 

| Đọc giá trị | cnt[1] | cnt[2] | cnt[60] | 
| --- | --- | --- | --- | 
| 60 | 0 | 0 | 1 | 
| 1 | 1 | 0 | 1 | 
| 60 | 1 | 0 | 2 | 
| 2 | 1 | 1 | 2 | 
| 1 | 2 | 1 | 2 | 
| 2 | 2 | 2 | 2 | 

Tái thiết: 

| Giá trị hiện tại | Tần số | Trả lời | 
| --- | --- | --- | 
| 1 | 2 | 1 1 | 
| 2 | 2 | 1 1 2 2 | 
| 3..59 | 0 | 1 1 2 2 | 
| 60 | 2 | 1 1 2 2 60 60 | 

Đầu ra cuối cùng:```
1 1 2 2 60 60
```Ví dụ này thực hiện cả hai ranh giới của phạm vi giá trị được phép. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + 60) | Đầu vào đọc lấy O(n), việc tái tạo quét các giá trị từ 1 đến 60 | 
| Không gian | O(60) | Mảng tần số lưu trữ số đếm cho từng giá trị có thể | 

Với$n \le 100$, giải pháp này dễ dàng nằm trong giới hạn. Thời gian chạy thực tế là không đổi vì phạm vi giá trị cố định và rất nhỏ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    cnt = [0] * 61

    for _ in range(n):
        x = int(input())
        cnt[x] += 1

    ans = []

    for value in range(1, 61):
        ans.extend([value] * cnt[value])

    print(*ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided sample
assert run(
"""5
7
1
9
7
3
"""
) == "1 3 7 7 9", "sample 1"

# minimum size
assert run(
"""1
42
"""
) == "42", "minimum size"

# all equal values
assert run(
"""5
7
7
7
7
7
"""
) == "7 7 7 7 7", "all equal"

# boundary values
assert run(
"""4
1
60
1
60
"""
) == "1 1 60 60", "boundary values"

# reverse order input
assert run(
"""6
6
5
4
3
2
1
"""
) == "1 2 3 4 5 6", "reverse order"

# duplicates mixed with boundaries
assert run(
"""8
60
1
30
60
1
30
15
15
"""
) == "1 1 15 15 30 30 60 60", "mixed duplicates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Giá trị đơn`42`|`42`| Kích thước đầu vào tối thiểu | 
| Tất cả các giá trị bằng`7`|`7 7 7 7 7`| Xử lý trùng lặp | 
| Giá trị`1`Và`60`|`1 1 60 60`| Tính chính xác của việc lập chỉ mục ranh giới | 
| Trình tự đảo ngược |`1 2 3 4 5 6`| Hành vi sắp xếp hợp lý | 
| Hỗn hợp trùng lặp và cực đoan |`1 1 15 15 30 30 60 60`| Tái tạo tần số | 

## Vỏ cạnh 

Hãy xem xét trường hợp tất cả các yếu tố giống hệt nhau:```
5
7
7
7
7
7
```Mảng tần số kết thúc bằng`cnt[7] = 5`. Trong quá trình xây dựng lại, thuật toán sẽ thêm giá trị`7`đúng năm lần. Vì không có tần số nào khác khác 0 nên đầu ra cuối cùng sẽ trở thành:```
7 7 7 7 7
```Điều này xác nhận các bản sao được bảo tồn thay vì bị thu gọn. 

Bây giờ hãy xem xét các giá trị biên:```
4
1
60
1
60
```Sau khi đọc đầu vào, chúng ta có:```
cnt[1] = 2
cnt[60] = 2
```Vòng lặp tái thiết quét từ 1 đến 60. Khi nó đạt tới`1`, nó nối thêm hai bản sao. Khi nó đạt tới`60`, nó gắn thêm hai bản sao nữa. Kết quả cuối cùng là:```
1 1 60 60
```Sử dụng mảng tần số có kích thước 61 là điều tạo nên chỉ số`60`có hiệu lực. 

Cuối cùng, hãy xem xét đầu vào nhỏ nhất có thể:```
1
42
```Thuật toán tăng dần`cnt[42]`một lần. Tái thiết nối thêm một bản sao của`42`. Đầu ra là:```
42
```Không cần xử lý đặc biệt đối với mảng một phần tử vì phương pháp đếm hoạt động tự nhiên với mọi kích thước đầu vào hợp lệ.
