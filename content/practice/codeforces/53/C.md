---
title: "CF 53C - Ếch Nhỏ"
description: "Ta có n gò đất xếp thẳng hàng tại các vị trí 1, 2, ..., n. Con ếch muốn ghé thăm mỗi gò đất đúng một lần, vì vậy chúng ta phải đưa ra một hoán vị của các vị trí này."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 53
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 49 (Div. 2)"
rating: 1200
weight: 53
solve_time_s: 95
verified: true
draft: false
---
[CF 53C - Chú ếch nhỏ](https://codeforces.com/problemset/problem/53/C) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`n`gò đặt trên một đường thẳng tại các vị trí`1, 2, ..., n`. Con ếch muốn ghé thăm mỗi gò đất đúng một lần, vì vậy chúng ta phải đưa ra một hoán vị của các vị trí này. 

Hạn chế thú vị là ở bước nhảy. Nếu con ếch đến thăm các gò đất theo thứ tự`p1, p2, ..., pn`, thì độ dài bước nhảy là:$$|p_1 - p_2|,\ |p_2 - p_3|,\ \dots,\ |p_{n-1} - p_n|$$Tất cả những khoảng cách này phải khác nhau. 

Đầu vào chỉ chứa một số nguyên,`n`, và chúng ta cần xây dựng bất kỳ thứ tự truy cập hợp lệ nào. 

Những hạn chế là rất nhỏ. Thậm chí một`O(n^2)`bước xác minh sẽ dễ dàng nằm trong giới hạn vì`n ≤ 10^4`. Điều quan trọng là tìm ra một khuôn mẫu mang tính xây dựng luôn hiệu quả. Việc tìm kiếm toàn diện các hoán vị là hoàn toàn không thể bởi vì`n!`phát triển bùng nổ. Ngay cả đối với`n = 12`, đã có hàng trăm triệu hoán vị rồi. 

Phần khó khăn là một tuyến đường có thể trông hợp lệ cục bộ trong khi thất bại trên toàn cầu vì một số bước nhảy lặp lại sau đó. 

Ví dụ, hãy xem xét:```
1 3 2 4
```Độ dài bước nhảy là:```
2, 1, 2
```Khoảng cách`2`xuất hiện hai lần, vì vậy tuyến đường này không hợp lệ. 

Một sai lầm dễ mắc phải khác là cho rằng khoảng cách tăng dần liên tiếp sẽ tự động có tác dụng. Vì`n = 5`, trình tự:```
1 5 2 4 3
```tạo ra bước nhảy:```
4, 3, 2, 1
```Điều này là hợp lệ, nhưng những thay đổi nhỏ sẽ phá vỡ nó ngay lập tức. Ví dụ:```
1 4 2 5 3
```mang lại:```
3, 2, 3, 2
```Hai khoảng cách lặp đi lặp lại xuất hiện. 

Các giá trị nhỏ nhất cũng đáng được chú ý. 

Vì`n = 1`, không có bước nhảy nào cả, vì vậy bất kỳ gò đất nào cũng hoạt động. 

Vì`n = 2`, có chính xác một lần nhảy, nó tự động là duy nhất. 

Một thuật toán mang tính xây dựng phải xử lý các trường hợp ranh giới này một cách tự nhiên mà không cần sử dụng các thủ thuật đặc biệt. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ tạo ra các hoán vị của`1..n`và kiểm tra xem tất cả các độ dài bước nhảy có khác biệt hay không. 

Sự đúng đắn là hiển nhiên. Nếu cuối cùng chúng ta tìm được một hoán vị mà các hiệu tuyệt đối liên tiếp đều khác nhau thì nó thỏa mãn trực tiếp bài toán. 

Vấn đề là không gian tìm kiếm. có`n!`hoán vị. Ngay cả khi việc kiểm tra một hoán vị chỉ mất`O(n)`, tổng độ phức tạp trở thành:$$O(n \cdot n!)$$Điều này gần như không thể sử dụng được ngay lập tức. Vì`n = 15`, số lượng hoán vị đã vượt quá một nghìn tỷ. 

Quan sát quan trọng là độ dài bước nhảy phải khác nhau và có chính xác`n - 1`nhảy. Độ dài bước nhảy khác biệt duy nhất có thể là:$$1, 2, 3, \dots, n-1$$Vì vậy, một công trình hợp lệ phải sử dụng mọi khoảng cách có thể đúng một lần. 

Điều đó gợi ý mạnh mẽ việc xây dựng trình tự để số lần nhảy giảm xuống một cách có kiểm soát:$$n-1,\ n-2,\ n-3,\ \dots,\ 1$$Một cách rất tự nhiên để đạt được điều này là luân phiên giữa vị trí không sử dụng nhỏ nhất và vị trí không sử dụng lớn nhất. 

Ví dụ, với`n = 7`:```
1 7 2 6 3 5 4
```Độ dài bước nhảy trở thành:```
6, 5, 4, 3, 2, 1
```Mỗi khoảng cách xuất hiện đúng một lần. 

Tại sao mô hình xen kẽ này hoạt động? Bởi vì mỗi lần chúng ta di chuyển từ đầu này của khoảng còn lại sang đầu kia, khoảng đó sẽ co lại một phần. Lần nhảy đầu tiên kéo dài gần như toàn bộ mảng, lần nhảy tiếp theo kéo dài ít hơn một mảng, v.v. 

Điều này biến vấn đề từ một tìm kiếm hoán vị không thể thành một giải pháp mang tính xây dựng trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot n!)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với hai con trỏ:`left = 1`Và`right = n`. 

Chúng đại diện cho các vị trí gò nhỏ nhất và lớn nhất chưa được sử dụng. 
2. Tạo một mảng câu trả lời trống. 
3. Trong khi`left <= right`, nối các vị trí theo thứ tự xen kẽ. 

Đầu tiên nối thêm`left`, sau đó tăng`left`. 

Sau đó, nếu vẫn còn vị trí chưa sử dụng, hãy nối thêm`right`, sau đó giảm`right`. 
4. Tiếp tục cho đến khi tất cả các vị trí được sử dụng đúng một lần. 

Ví dụ, khi`n = 6`: 

1. Thêm`1`2. Thêm`6`3. Thêm`2`4. Thêm`5`5. Thêm`3`6. Thêm`4`Trình tự trở thành:```
1 6 2 5 3 4
```Độ dài bước nhảy là:```
5, 4, 3, 2, 1
```Mỗi lần nhảy nhỏ hơn lần trước một lần. 

### Tại sao nó hoạt động 

Ở mỗi bước, trình tự sẽ nhảy giữa các đầu đối diện của khoảng thời gian chưa sử dụng còn lại. 

Giả sử khoảng còn lại là`[L, R]`. 

Khi chúng ta di chuyển từ bên này sang bên kia, độ dài bước nhảy là:$$R - L$$Sau khi sử dụng một điểm cuối, khoảng thời gian sẽ giảm đi một, do đó bước nhảy tiếp theo sẽ trở thành:$$(R - L) - 1$$Điều này tiếp tục cho đến khi khoảng thời gian sụp đổ. 

Độ dài bước nhảy được tạo ra chính xác là:$$n-1,\ n-2,\ \dots,\ 1$$Tất cả đều khác biệt và mỗi gò được sử dụng đúng một lần vì mỗi vị trí sẽ bị xóa khỏi việc xem xét ngay sau khi được thêm vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

left = 1
right = n

ans = []

while left <= right:
    ans.append(left)
    left += 1

    if left <= right:
        ans.append(right)
        right -= 1

print(*ans)
```Việc thực hiện theo sau việc xây dựng trực tiếp.`left`theo dõi gò đất nhỏ nhất chưa được sử dụng và`right`theo dõi gò đất chưa sử dụng lớn nhất. Mỗi lần lặp lại thêm một giá trị từ phía bên trái và một giá trị từ phía bên phải. 

điều kiện`if left <= right`là quan trọng khi`n`thật kỳ quặc. Không có nó, phần tử ở giữa sẽ được thêm hai lần. 

Ví dụ, với`n = 5`, sau khi chèn:```
1 5 2 4
```cả hai con trỏ đều trở thành`3`. Chúng ta nên nối thêm`3`đúng một lần. 

Thuật toán không bao giờ cần tính toán độ dài bước nhảy một cách rõ ràng vì việc xây dựng đảm bảo tính chính xác theo thiết kế. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
```| Bước | trái | đúng | và | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 | 2 | [] | 
| Thêm trái | 2 | 2 | [1] | 
| Thêm đúng | 2 | 1 | [1, 2] | 
Trình tự cuối cùng:```
1 2
```Độ dài bước nhảy:```
1
```Chỉ có một bước nhảy nên tính duy nhất là tự động. 

### Ví dụ 2 

đầu vào:```
7
```| Bước | trái | đúng | trả lời | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 | 7 | [] | 
| Thêm trái | 2 | 7 | [1] | 
| Thêm đúng | 2 | 6 | [1, 7] | 
| Thêm trái | 3 | 6 | [1, 7, 2] | 
| Thêm đúng | 3 | 5 | [1, 7, 2, 6] | 
| Thêm trái | 4 | 5 | [1, 7, 2, 6, 3] | 
| Thêm đúng | 4 | 4 | [1, 7, 2, 6, 3, 5] | 
| Thêm trái | 5 | 4 | [1, 7, 2, 6, 3, 5, 4] | 

Trình tự cuối cùng:```
1 7 2 6 3 5 4
```Độ dài bước nhảy:```
6, 5, 4, 3, 2, 1
```Dấu vết này cho thấy sự bất biến cốt lõi: mỗi lần nhảy chính xác nhỏ hơn một lần so với lần nhảy trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi gò được nối đúng một lần | 
| Không gian |$O(n)$| Mảng câu trả lời lưu trữ tất cả các vị trí | 

Giải pháp dễ dàng phù hợp trong giới hạn. Ngay cả đối với`n = 10^4`, thuật toán chỉ thực hiện một vài thao tác đơn giản cho mỗi phần tử. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())

    left = 1
    right = n

    ans = []

    while left <= right:
        ans.append(left)
        left += 1

        if left <= right:
            ans.append(right)
            right -= 1

    print(*ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("2\n") == "1 2\n", "sample 1"

# minimum size
assert run("1\n") == "1\n", "n = 1"

# odd n
assert run("5\n") == "1 5 2 4 3\n", "odd length construction"

# even n
assert run("6\n") == "1 6 2 5 3 4\n", "even length construction"

# larger case
out = run("10\n").strip().split()
arr = list(map(int, out))

assert sorted(arr) == list(range(1, 11)), "must be a permutation"

diffs = [abs(arr[i] - arr[i + 1]) for i in range(9)]
assert len(set(diffs)) == 9, "all jump lengths must be unique"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Xử lý bước nhảy trống được đặt chính xác | 
|`5`|`1 5 2 4 3`| Công trình có kích thước lẻ | 
|`6`|`1 6 2 5 3 4`| Xây dựng quy mô chẵn | 
|`10`| Bất kỳ hoán vị hợp lệ nào | Độ chính xác chung và khoảng cách duy nhất | 

## Vỏ cạnh 

cho`n = 1`, đầu vào là:```
1
```Thuật toán bắt đầu với`left = right = 1`. 

Nó nối thêm`1`, số gia`left`ĐẾN`2`, và dừng lại vì`left > right`. 

Đầu ra là:```
1
```Không có bước nhảy nào nên điều kiện duy nhất được thỏa mãn một cách trống rỗng. 

Vì`n = 2`, thuật toán tạo ra:```
1 2
```Độ dài bước nhảy duy nhất là:```
|1 - 2| = 1
```Không thể sao chép vì chỉ có một bước nhảy. 

Đối với các giá trị lẻ như`n = 5`, vị trí ở giữa là phần nguy hiểm. 

Việc xây dựng mang lại:```
1 5 2 4 3
```Các bước nhảy là:```
4, 3, 2, 1
```Giá trị trung tâm`3`xuất hiện đúng một lần vì điều kiện:```
if left <= right:
```Nếu không có điều kiện đó, phần tử ở giữa sẽ được chèn hai lần, vi phạm yêu cầu hoán vị. 

Đối với các giá trị chẵn lớn hơn như`n = 8`, thuật toán đầu ra:```
1 8 2 7 3 6 4 5
```Các bước nhảy là:```
7, 6, 5, 4, 3, 2, 1
```Điều này xác nhận tính bất biến rằng mỗi bước nhảy mới sẽ giảm đi đúng một.
