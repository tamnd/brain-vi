---
title: "CF 36A - Trí tuệ ngoài Trái đất"
description: "Chúng tôi được cung cấp một chuỗi nhị phân thể hiện những quan sát của Vasya trong vài ngày. Ký tự '1' có nghĩa là tín hiệu đã được nhận vào ngày hôm đó, trong khi '0' có nghĩa là không có tín hiệu nào xuất hiện."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 36
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 36"
rating: 1300
weight: 36
solve_time_s: 81
verified: true
draft: false
---
[CF 36A - Trí thông minh ngoài Trái đất](https://codeforces.com/problemset/problem/36/A) 

**Đánh giá:** 1300 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi nhị phân thể hiện những quan sát của Vasya trong vài ngày. Một nhân vật`'1'`có nghĩa là một tín hiệu đã được nhận vào ngày hôm đó, trong khi`'0'`có nghĩa là không có tín hiệu xuất hiện. 

Nhiệm vụ là xác định xem các tín hiệu nhận được có tuân theo một mẫu hoàn toàn đều đặn hay không. Chính xác hơn, nếu chúng ta nhìn vào vị trí của tất cả`'1'`các ký tự, khoảng cách giữa các vị trí liên tiếp phải giống nhau. Nếu mọi khoảng đều giống nhau, chúng ta in`"YES"`. Ngược lại, chúng tôi in`"NO"`. 

Ví dụ, trong chuỗi`001001001`, tín hiệu xuất hiện ở vị trí`2`,`5`, Và`8`. Những khoảng trống là`3`Và`3`, do đó mô hình là đều đặn. 

Những hạn chế là rất nhỏ. Độ dài chuỗi tối đa là`100`, có nghĩa là ngay cả những giải pháp kém hiệu quả cũng sẽ dễ dàng chạy trong giới hạn. Một giải pháp bậc hai với xung quanh`10^4`hoạt động là tầm thường trong giới hạn thời gian 2 giây. Tuy nhiên, vấn đề này chủ yếu liên quan đến việc triển khai rõ ràng và xử lý chính xác các vị trí của tín hiệu. 

Phần khó khăn không phải là hiệu suất mà là diễn giải chính xác khoảng cách giữa các tín hiệu. 

Hãy xem xét đầu vào:```
5
10101
```Các vị trí tín hiệu được`0`,`2`, Và`4`. Những khoảng trống là`2`Và`2`, vậy câu trả lời là`"YES"`. 

Thay vào đó, việc triển khai bất cẩn có thể đếm số 0 giữa các tín hiệu và vô tình so sánh`1`Và`1`, điều này có tác dụng ở đây nhưng có thể trở nên khó hiểu khi kết hợp với logic lập chỉ mục. 

Một trường hợp tinh vi khác là khi các tín hiệu liên tiếp xuất hiện mà không có số 0 ở giữa chúng:```
6
111000
```Các vị trí là`0`,`1`, Và`2`. Những khoảng trống là`1`Và`1`, vậy câu trả lời là`"YES"`. 

Ở đây, một cách tiếp cận không chính xác giả định mọi tín hiệu phải có ít nhất một số 0 giữa chúng. 

Trường hợp cạnh nguy hiểm hơn là khi chỉ có một số khoảng khớp:```
7
1001001
```Các vị trí tín hiệu được`0`,`3`, Và`6`. Những khoảng trống là`3`Và`3`, vậy câu trả lời là`"YES"`. 

Nhưng:```
7
1001010
```Các vị trí là`0`,`3`, Và`5`. Những khoảng trống là`3`Và`2`, vậy đáp án đúng là`"NO"`. 

Nếu chỉ so sánh tín hiệu đầu tiên và tín hiệu cuối cùng, chúng ta có thể kết luận sai rằng mẫu này là đều đặn. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực trực tiếp nhất là thu thập mọi vị trí có chứa`'1'`, sau đó tính toán rõ ràng tất cả các khác biệt liên tiếp và so sánh chúng. 

Giả sử vị trí tín hiệu được lưu trữ trong một mảng:```
pos = [2, 5, 8, 11]
```Chúng tôi tính toán:```
5 - 2 = 3
8 - 5 = 3
11 - 8 = 3
```Nếu mọi sự khác biệt đều bằng cái đầu tiên thì mẫu đó là đều đặn. 

Cách tiếp cận này đã đủ nhanh. Với nhiều nhất`100`các ký tự, thậm chí việc quét chuỗi liên tục sẽ hoạt động thoải mái trong giới hạn. 

Một biến thể mạnh mẽ hơn có thể thử mọi độ dài khoảng thời gian có thể và xác minh xem tất cả các tín hiệu có tuân theo nó hay không. Điều đó vẫn sẽ vượt qua vì các ràng buộc rất nhỏ nhưng nó thực hiện những công việc không cần thiết và làm phức tạp việc triển khai. 

Nhận xét quan trọng là vấn đề chỉ phụ thuộc vào khoảng cách giữa các`'1'`các vị trí. Một khi chúng ta biết khoảng đầu tiên thì các khoảng sau đó phải khớp chính xác với nó. Không có gì khác quan trọng. 

Sự quan sát đó làm giảm nhiệm vụ thành một lần quét tuyến tính đơn giản trên các vị trí tín hiệu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`và chuỗi nhị phân. 
2. Duyệt chuỗi và lưu trữ các chỉ số nơi ký tự đó`'1'`. 

Các chỉ số này đại diện cho những ngày nhận được tín hiệu. 
3. Tính khoảng cách giữa hai vị trí tín hiệu đầu tiên. 

Điều này trở thành khoảng thời gian dự kiến ​​mà mọi cặp sau này phải tuân theo. 
4. Lặp lại các cặp vị trí tín hiệu liên tiếp còn lại. 

Với mỗi cặp, hãy tính hiệu giữa các chỉ số của chúng. 
5. So sánh sự khác biệt hiện tại với khoảng thời gian dự kiến. 

Nếu có khoảng cách khác nhau, hãy in`"NO"`ngay lập tức vì mẫu không đều đặn. 
6. Nếu tất cả các khoảng đều khớp nhau, hãy in`"YES"`. 

### Tại sao nó hoạt động 

Điều kiện duy nhất mà bài toán yêu cầu là sự bằng nhau của tất cả các khoảng giữa các tín hiệu liên tiếp. Thuật toán trực tiếp kiểm tra chính xác thuộc tính đó. 

Đặt vị trí tín hiệu là:```
p₀, p₁, p₂, ..., pₖ
```Dãy số đúng khi và chỉ khi:```
p₁ - p₀ = p₂ - p₁ = ... = pₖ - pₖ₋₁
```Thuật toán tính toán sự khác biệt đầu tiên và xác minh mọi sự khác biệt sau đó đối với nó. Vì mỗi khoảng thời gian bắt buộc được kiểm tra chính xác một lần nên thuật toán không thể chấp nhận hoặc từ chối một chuỗi không chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
s = input().strip()

positions = []

for i in range(n):
    if s[i] == '1':
        positions.append(i)

gap = positions[1] - positions[0]

for i in range(2, len(positions)):
    if positions[i] - positions[i - 1] != gap:
        print("NO")
        break
else:
    print("YES")
```Phần đầu tiên của mã đọc đầu vào và tập hợp mọi chỉ mục chứa`'1'`. Việc sử dụng trực tiếp các chỉ số giúp logic đơn giản và tránh nhầm lẫn về việc đếm số 0 giữa các tín hiệu. 

Biến`gap`lưu trữ khoảng thời gian dự kiến ​​giữa các tín hiệu. Vì tuyên bố đảm bảo ít nhất ba`'1'`ký tự, truy cập`positions[1]`luôn được an toàn. 

Vòng lặp bắt đầu từ chỉ mục`2`vì khoảng đầu tiên đã được sử dụng làm tham chiếu. Đối với mỗi tín hiệu sau, chúng tôi so sánh khoảng cách của nó với tín hiệu trước đó với`gap`. 

các`for-else`cấu trúc ở đây thuận tiện. Nếu vòng lặp kết thúc mà không nhấn`break`, mọi khoảng thời gian đều khớp và chúng tôi in`"YES"`. 

Một lỗi thường gặp là trộn lẫn số 0 với chênh lệch chỉ số. Ví dụ, các vị trí`0`Và`3`cách nhau một khoảng là`3`, mặc dù giữa chúng chỉ có hai số 0. Sử dụng các chỉ số trực tiếp sẽ tránh được sự mơ hồ đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8
00111000
```Các vị trí tín hiệu được`2`,`3`, Và`4`. 

| Bước | Vị trí hiện tại | Vị trí trước đây | Sự khác biệt | Khoảng cách dự kiến ​​| 
| --- | --- | --- | --- | --- | 
| Ban đầu | 3 | 2 | 1 | 1 | 
| Kiểm tra | 4 | 3 | 1 | 1 | 

Mọi sự khác biệt đều bình đẳng`1`, do đó đầu ra là:```
YES
```Ví dụ này cho thấy các tín hiệu liên tiếp được cho phép. Khoảng thời gian có thể là`1`. 

### Ví dụ 2 

đầu vào:```
7
1001010
```Các vị trí tín hiệu được`0`,`3`, Và`5`. 

| Bước | Vị trí hiện tại | Vị trí trước đây | Sự khác biệt | Khoảng cách dự kiến ​​| 
| --- | --- | --- | --- | --- | 
| Ban đầu | 3 | 0 | 3 | 3 | 
| Kiểm tra | 5 | 3 | 2 | 3 | 

Khoảng thứ hai khác với khoảng thứ nhất, vì vậy đầu ra là:```
NO
```Dấu vết này chứng minh tại sao chỉ kiểm tra tín hiệu đầu tiên và cuối cùng là không đủ. Mỗi khoảng thời gian liên tiếp đều quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuỗi được quét một lần và vị trí tín hiệu được kiểm tra một lần | 
| Không gian | O(n) | Trong trường hợp xấu nhất, mọi nhân vật đều`'1'`| 

Với`n ≤ 100`, giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. Ngay cả những cách tiếp cận chậm hơn nhiều cũng có thể vượt qua, nhưng giải pháp tuyến tính rõ ràng và phù hợp trực tiếp với cấu trúc vấn đề. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    s = input().strip()

    positions = []

    for i in range(n):
        if s[i] == '1':
            positions.append(i)

    gap = positions[1] - positions[0]

    for i in range(2, len(positions)):
        if positions[i] - positions[i - 1] != gap:
            print("NO")
            return

    print("YES")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
assert run("8\n00111000\n") == "YES", "sample 1"

# custom cases
assert run("5\n10101\n") == "YES", "equal spacing"
assert run("7\n1001010\n") == "NO", "unequal spacing"
assert run("6\n111000\n") == "YES", "consecutive signals"
assert run("3\n111\n") == "YES", "minimum size"
assert run("10\n1001001001\n") == "YES", "large equal intervals"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`10101`|`YES`| Khoảng cách đều đặn với số 0 giữa các tín hiệu | 
|`1001010`|`NO`| Khoảng cách không bằng nhau | 
|`111000`|`YES`| Tín hiệu liên tiếp có khoảng 1 | 
|`111`|`YES`| Kích thước đầu vào hợp lệ tối thiểu | 
|`1001001001`|`YES`| Khoảng thời gian lớn hơn lặp đi lặp lại | 

## Vỏ cạnh 

Xét trường hợp có tín hiệu liên tiếp:```
6
111000
```Mảng vị trí trở thành:```
[0, 1, 2]
```Khoảng cách dự kiến ​​là:```
1 - 0 = 1
```Kiểm tra còn lại là:```
2 - 1 = 1
```Tất cả các khoảng đều khớp nhau, do đó thuật toán sẽ in`"YES"`. 

Trường hợp này xác nhận rằng các khoảng độ dài`1`là hoàn toàn hợp lệ. 

Bây giờ hãy xem xét một mô hình bất thường:```
7
1001010
```Các vị trí là:```
[0, 3, 5]
```Khoảng cách dự kiến ​​là`3`. 

Khoảng tiếp theo là:```
5 - 3 = 2
```Từ`2 != 3`, thuật toán in ngay lập tức`"NO"`. 

Điều này chứng tỏ rằng mọi khoảng thời gian liên tiếp đều được kiểm tra độc lập. 

Cuối cùng, hãy xem xét một mẫu thưa thớt hoàn toàn đều đặn:```
9
100100100
```Các vị trí là:```
[0, 3, 6]
```Khoảng cách dự kiến ​​là:```
3
```Khoảng thứ hai cũng là`3`, do đó thuật toán in`"YES"`. 

Điều này xác nhận rằng số lượng số 0 thực tế không quan trọng. Chỉ có sự bình đẳng của khoảng cách liên tiếp mới quan trọng.
