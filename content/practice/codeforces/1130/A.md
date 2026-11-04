---
title: "CF 1130A - Hãy tích cực"
description: "Chúng ta được cung cấp một mảng chứa số dương, số âm và có thể cả số 0. Chúng ta cần chọn một số nguyên khác 0 $d$ sao cho sau khi chia mọi phần tử cho $d$, ít nhất một nửa số phần tử mảng là dương."
date: "2026-06-12T04:16:47+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1130
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 542 [Alex Lopashev Thanks-Round] (Div. 2)"
rating: 800
weight: 1130
solve_time_s: 112
verified: false
draft: false
---

[CF 1130A - Hãy tích cực](https://codeforces.com/problemset/problem/1130/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng chứa số dương, số âm và có thể cả số 0. Chúng ta cần chọn một số nguyên khác 0$d$sao cho sau khi chia mọi phần tử cho$d$, ít nhất một nửa số phần tử mảng là dương. Chính xác hơn, số giá trị dương sau khi chia ít nhất phải bằng$\lceil n/2 \rceil$. 

Độ lớn thực tế sau khi chia không quan trọng. Chỉ có dấu hiệu quan trọng. Một giá trị như$2.5$vẫn tích cực, trong khi$0$vẫn không tích cực cũng không tiêu cực. 

Những hạn chế là rất nhỏ. Mảng chứa tối đa 100 phần tử và mọi giá trị nằm trong khoảng$-1000$Và$1000$. Chỉ với 100 con số, ngay cả việc kiểm tra mảng một cách thô bạo cũng là chuyện nhỏ. 

Quan sát quan trọng xuất phát từ cách phân chia ảnh hưởng đến các dấu hiệu. Chia cho một số dương giữ cho mọi dấu không đổi. Chia cho một số âm sẽ chuyển mọi số dương thành số âm và mọi số âm thành số dương. Số không vẫn là số không trong cả hai trường hợp. 

Một số trường hợp khó khăn có thể dễ dàng gây ra sai lầm. 

Coi như:```
3
0 0 5
```Nếu chia cho số dương thì mảng vẫn giữ nguyên dấu$(0,0,+)$, do đó chỉ có một giá trị dương. Từ$\lceil 3/2 \rceil = 2$, điều này là không đủ. Một giải pháp bất cẩn bỏ qua số 0 có thể kết luận không chính xác rằng mặt tích cực chiếm đa số. 

Coi như:```
5
-1 -2 -3 0 0
```Việc sử dụng ước số âm sẽ chuyển ba số âm thành số dương, cho ra ba giá trị dương. Câu trả lời đúng là$-1$. Giải pháp chỉ kiểm tra ước số dương sẽ in sai 0. 

Hãy xem xét:```
4
1 -1 0 0
```Có một tích cực và một tiêu cực. Một số chia dương mang lại một số dương. Một số chia âm cũng mang lại một số dương. Chúng ta cần ít nhất hai kết quả dương tính nên câu trả lời đúng là 0. 

Sự hiện diện của nhiều số 0 là điểm tinh tế chính vì các số 0 không bao giờ trở thành số dương bất kể số chia. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi ước số hợp lệ từ$-1000$ĐẾN$1000$, không bao gồm số không. Đối với mỗi ước số, chúng tôi sẽ chia mọi phần tử mảng và đếm xem có bao nhiêu kết quả dương. 

Điều này hoạt động vì không gian tìm kiếm rất nhỏ. Chỉ có 2000 ước số có thể và tối đa 100 phần tử mảng. Công việc trong trường hợp xấu nhất là về$2000 \times 100 = 200{,}000$hoạt động, dễ dàng phù hợp trong giới hạn. 

Phần thú vị là nhận thấy rằng giá trị chính xác của số chia là không liên quan. Chỉ có dấu hiệu của nó là quan trọng. 

Nếu như$d > 0$, mọi phần tử dương vẫn dương và mọi phần tử âm vẫn âm. 

Nếu như$d < 0$, mọi phần tử dương đều trở thành âm và mọi phần tử âm đều trở thành dương. 

Điều đó có nghĩa là chỉ có hai tình huống riêng biệt cần kiểm tra: 

Đầu tiên, sử dụng ước số dương. Số lượng giá trị dương bằng số phần tử dương trong mảng ban đầu. 

Thứ hai, sử dụng ước số âm. Số lượng giá trị dương bằng số phần tử âm trong mảng ban đầu. 

Cho phép:

-$pos$là số phần tử dương. 
-$neg$là số phần tử âm. 
-$need = \lceil n/2 \rceil$. 

Nếu như$pos \ge need$, chúng ta có thể in ra bất kỳ ước số dương nào, ví dụ 1. 

Bằng không, nếu$neg \ge need$, chúng ta có thể in bất kỳ ước số âm nào, ví dụ -1. 

Ngược lại, không lựa chọn dấu nào có thể tạo ra đủ giá trị dương, vì vậy chúng ta in ra 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2000·n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và đếm xem có bao nhiêu phần tử dương. Lưu số đếm này vào`pos`. 
2. Đếm xem có bao nhiêu phần tử âm. Lưu số đếm này vào`neg`. 
3. Tính số giá trị dương cần tìm:$$need = \left\lceil \frac{n}{2} \right\rceil$$Trong số học số nguyên đây là`(n + 1) // 2`. 
4. Nếu`pos >= need`, in`1`. 

Số chia dương bảo toàn mọi dấu nên số giá trị dương sau khi chia chính xác là`pos`. 
5. Ngược lại, nếu`neg >= need`, in`-1`. 

Ước số âm đổi dấu nên mọi phần tử âm đều trở thành dương. Số lượng giá trị dương trở nên chính xác`neg`. 
6. Nếu không thì in`0`. 

Cả hai lựa chọn dấu hiệu đều không thể tạo ra đủ giá trị dương. 

### Tại sao nó hoạt động 

Dấu của mọi thương khác 0 chỉ phụ thuộc vào dấu của số bị chia và dấu của số chia. Độ lớn của số chia không bao giờ ảnh hưởng đến việc kết quả có dương hay không. 

Việc sử dụng ước số dương sẽ bảo toàn phân bố dấu ban đầu, tạo ra kết quả chính xác`pos`những giá trị tích cực. Sử dụng ước số âm hoán đổi số dương và số âm, tạo ra kết quả chính xác`neg`những giá trị tích cực. Vì đây là hai kết quả duy nhất có thể xảy ra nên việc kiểm tra xem một trong hai kết quả có đạt đến$\lceil n/2 \rceil$hoàn toàn đặc trưng cho tất cả các câu trả lời hợp lệ. Nếu không có số nào đủ lớn thì không có ước số nào có thể đáp ứng yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

pos = sum(1 for x in a if x > 0)
neg = sum(1 for x in a if x < 0)

need = (n + 1) // 2

if pos >= need:
    print(1)
elif neg >= need:
    print(-1)
else:
    print(0)
```Phần đầu tiên tính riêng các yếu tố tích cực và tiêu cực. Các số 0 bị bỏ qua vì chúng không bao giờ đóng góp vào số giá trị dương sau khi chia. 

Biến`need`cửa hàng$\lceil n/2 \rceil$. sử dụng`(n + 1) // 2`xử lý chính xác cả giá trị chẵn và lẻ của`n`. 

Thứ tự kiểm tra chỉ quan trọng đối với việc chọn câu trả lời hợp lệ nào để in khi cả hai số đếm đều đáp ứng yêu cầu. Vấn đề cho phép bất kỳ ước số hợp lệ nào, vì vậy việc in`1`đầu tiên là hoàn toàn có thể chấp nhận được. 

Không có phép chia thực tế nào được thực hiện. Giải pháp dựa hoàn toàn vào hành vi ký hiệu, đây là sự đơn giản hóa chính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
10 0 -7 2 6
```Đây`need = (5 + 1) // 2 = 3`. 

| Yếu tố | Đếm dương | Đếm âm | 
| --- | --- | --- | 
| 10 | 1 | 0 | 
| 0 | 1 | 0 | 
| -7 | 1 | 1 | 
| 2 | 2 | 1 | 
| 6 | 3 | 1 | 

Giá trị cuối cùng: 

| Biến | Giá trị | 
| --- | --- | 
| tư thế | 3 | 
| phủ định | 1 | 
| cần | 3 | 

Từ`pos >= need`, chúng tôi in:```
1
```Điều này chứng tỏ rằng mọi ước số dương đều hoạt động vì mảng ban đầu đã chứa đủ phần tử dương. 

### Ví dụ 2 

đầu vào:```
5
-1 -2 -3 0 0
```Đây`need = 3`. 

| Yếu tố | Đếm dương | Đếm âm | 
| --- | --- | --- | 
| -1 | 0 | 1 | 
| -2 | 0 | 2 | 
| -3 | 0 | 3 | 
| 0 | 0 | 3 | 
| 0 | 0 | 3 | 

Giá trị cuối cùng: 

| Biến | Giá trị | 
| --- | --- | 
| tư thế | 0 | 
| phủ định | 3 | 
| cần | 3 |`pos`là không đủ, nhưng`neg >= need`, vì vậy chúng tôi in:```
-1
```Một ước số âm sẽ chuyển ba số âm thành số dương, đạt được số lượng cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần đi qua mảng | 
| Không gian | O(1) | Chỉ có một vài quầy được lưu trữ | 

Với tối đa 100 phần tử, thuật toán sẽ chạy ngay lập tức. Việc sử dụng bộ nhớ là không đổi và thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    pos = sum(1 for x in a if x > 0)
    neg = sum(1 for x in a if x < 0)

    need = (n + 1) // 2

    if pos >= need:
        print(1)
    elif neg >= need:
        print(-1)
    else:
        print(0)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    result = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return result

# sample-style case
assert run("5\n10 0 -7 2 6\n") == "1"

# minimum size, positive
assert run("1\n5\n") == "1"

# minimum size, negative
assert run("1\n-5\n") == "-1"

# impossible because zeros dominate
assert run("4\n1 -1 0 0\n") == "0"

# all negatives
assert run("5\n-1 -2 -3 -4 -5\n") == "-1"

# all positives
assert run("6\n1 1 1 1 1 1\n") == "1"

# boundary count exactly equal to need
assert run("5\n1 2 3 0 -1\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 5`|`1`| Mảng dương kích thước tối thiểu | 
|`1 / -5`|`-1`| Mảng âm có kích thước tối thiểu | 
|`4 / 1 -1 0 0`|`0`| Số không ngăn cản đa số | 
|`5 / all negatives`|`-1`| Lật dấu qua ước số âm | 
|`6 / all positives`|`1`| Trường hợp ước số dương | 
|`5 / 1 2 3 0 -1`|`1`| Bình đẳng ngưỡng chính xác | 

## Vỏ cạnh 

Hãy xem xét:```
4
1 -1 0 0
```Chúng tôi có:```
pos = 1
neg = 1
need = 2
```Một ước số dương mang lại một giá trị dương. Một ước số âm cũng mang lại một giá trị dương. Không đạt đến hai, do đó thuật toán in:```
0
```Điều này xử lý chính xác các mảng trong đó số 0 chiếm quá nhiều mảng. 

Coi như:```
5
-1 -2 -3 0 0
```Số lượng là:```
pos = 0
neg = 3
need = 3
```Từ`neg`đạt đến ngưỡng, thuật toán in`-1`. Chia cho một số âm biến cả ba số âm thành số dương, thỏa mãn yêu cầu. 

Coi như:```
3
0 0 5
```Số lượng là:```
pos = 1
neg = 0
need = 2
```Cả số đếm đều không đạt đến ngưỡng, do đó thuật toán sẽ in:```
0
```Điều này xác nhận rằng số 0 không bao giờ giúp tăng số lượng giá trị dương, bất kể ước số được chọn là gì.
