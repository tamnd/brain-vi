---
title: "CF 233A - Hoán vị hoàn hảo"
description: "Chúng ta cần xây dựng một hoán vị các số từ 1 đến n với hai điều kiện. Điều kiện đầu tiên là p[p[i]] = i với mọi vị trí i. Áp dụng hoán vị hai lần phải đưa chúng ta trở lại chỉ mục ban đầu. Điều này có nghĩa là mọi phần tử đều trỏ về đối tác của nó."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 233
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 144 (Div. 2)"
rating: 800
weight: 233
solve_time_s: 86
verified: true
draft: false
---

[CF 233A - Hoán vị hoàn hảo](https://codeforces.com/problemset/problem/233/A) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một hoán vị các số từ`1`ĐẾN`n`với hai điều kiện. 

Điều kiện đầu tiên là`p[p[i]] = i`cho mọi vị trí`i`. Áp dụng hoán vị hai lần phải đưa chúng ta trở lại chỉ mục ban đầu. Điều này có nghĩa là mọi phần tử đều trỏ về đối tác của nó. 

Điều kiện thứ hai là`p[i] != i`cho mọi`i`. Không phần tử nào được phép giữ nguyên vị trí của nó. 

Cùng với nhau, những điều kiện này buộc hoán vị phải bao gồm toàn bộ sự hoán đổi giữa các cặp chỉ số. Nếu như`i`bản đồ tới`j`, sau đó`j`phải ánh xạ lại`i`và cả hai đều không thể ánh xạ tới chính chúng. 

Đầu vào chứa một số nguyên duy nhất`n`, độ lớn của hoán vị. Đầu ra là một hoán vị hoàn hảo hợp lệ hoặc`-1`nếu không có hoán vị như vậy tồn tại. 

Hạn chế là rất nhỏ,`n ≤ 100`, vì vậy hiệu suất không phải là vấn đề đáng lo ngại. Ngay cả vũ lực cũng sẽ phù hợp về mặt kỹ thuật. Thách thức thực sự là nhận ra cấu trúc toán học ẩn bên trong các điều kiện. 

Các trường hợp phức tạp đến từ các giá trị lẻ của`n`. 

Ví dụ: 

đầu vào:```
1
```Đầu ra:```
-1
```Chỉ có một hoán vị có thể xảy ra,`[1]`, nhưng nó vi phạm`p[i] != i`. 

Một trường hợp quan trọng khác: 

đầu vào:```
3
```Một cách tiếp cận bất cẩn có thể thử:```
2 1 3
```Hai vị trí đầu tiên tạo thành một hoán đổi hợp lệ, nhưng vị trí`3`ánh xạ tới chính nó, điều này bị cấm. 

Trường hợp hợp lệ nhỏ nhất là: 

đầu vào:```
2
```Đầu ra:```
2 1
```Đây:`p[1] = 2`

`p[2] = 1`Áp dụng hoán vị hai lần sẽ trả về chỉ mục ban đầu và không có vị trí nào cố định. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ tạo ra mọi hoán vị về kích thước`n`và kiểm tra xem nó có thỏa mãn cả hai điều kiện hay không. Việc kiểm tra chính nó là dễ dàng. Đối với mỗi chỉ số`i`, xác minh rằng`p[p[i]] == i`Và`p[i] != i`. 

Điều này hiệu quả vì những hạn chế là rất nhỏ, nhưng tốc độ tăng trưởng giai thừa sẽ trở nên không hợp lý rất nhanh. Thậm chí`10!`đã có nghĩa là hơn 3 triệu hoán vị. Việc tìm kiếm toàn diện là không cần thiết một khi chúng ta hiểu được các điều kiện ngụ ý gì. 

Quan sát quan trọng là hoán vị phải bao gồm hoàn toàn các cặp rời rạc. 

Giả định`p[i] = j`. Từ`p[p[i]] = i`, ta có ngay:```
p[j] = i
```Vì vậy, mọi phần tử phải thuộc về 2 chu kỳ:```
i <-> j
```Chu kỳ đơn phần tử bị cấm vì`p[i] != i`. 

Điều đó có nghĩa là mảng phải được phân chia thành từng cặp. Điều này chỉ có thể thực hiện được khi`n`là chẵn. 

Một khi chúng ta thấy được điều đó thì việc xây dựng trở nên tầm thường. Chúng ta chỉ cần hoán đổi các số liền kề:```
1 <-> 2
3 <-> 4
5 <-> 6
...
```Ví dụ, khi`n = 6`:```
2 1 4 3 6 5
```Áp dụng hoán vị hai lần sẽ trả về mọi chỉ mục cho chính nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! × n) | O(n) | Về mặt khái niệm quá chậm | 
| Tối ưu | O(n) | O(1) không bao gồm đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`. 
2. Kiểm tra xem`n`thật kỳ quặc. 

Nếu như`n`thật kỳ lạ, hãy in`-1`và dừng lại. Một hoán vị hoàn hảo bao gồm toàn bộ các cặp, do đó, số phần tử lẻ luôn để lại một phần tử không ghép đôi. 
3. Ngược lại, lặp qua các số từ`1`ĐẾN`n`trong các bước của`2`. 
4. Cho mỗi cặp`(i, i + 1)`, đầu ra:```
i + 1, i
```Điều này tạo ra 2 chu kỳ:```
i -> i + 1
i + 1 -> i
```5. In hoán vị đã xây dựng. 

### Tại sao nó hoạt động 

Mỗi cặp`(i, i + 1)`hình thành một sự hoán đổi. Đối với bất kỳ phần tử nào bên trong cặp, việc áp dụng hoán vị một khi sẽ chuyển sang đối tác và áp dụng lại nó sẽ trở về vị trí ban đầu. 

Ví dụ:```
p[i] = i + 1
p[i + 1] = i
```Sau đó:```
p[p[i]] = p[i + 1] = i
```Không có phần tử nào ánh xạ tới chính nó vì mọi phần tử đều hoán đổi với phần tử khác. 

Vì toàn bộ hoán vị được thực hiện từ 2 chu kỳ độc lập nên các điều kiện đúng cho tất cả các chỉ số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    if n % 2 == 1:
        print(-1)
        return

    ans = []

    for i in range(1, n + 1, 2):
        ans.append(str(i + 1))
        ans.append(str(i))

    print(" ".join(ans))

solve()
```Điều kiện đầu tiên được xử lý trong mã là liệu`n`thật kỳ quặc. Một tập hợp có kích thước lẻ không thể được phân chia hoàn toàn thành từng cặp, do đó không tồn tại hoán vị hoàn hảo. 

Vòng lặp tiến lên bằng cách`2`mỗi lần vì chúng tôi xử lý các chỉ số theo cặp. Đối với mỗi cặp`(i, i + 1)`, các số được nối theo thứ tự đảo ngược. Điều này trực tiếp tạo ra cấu trúc 2 chu kỳ cần thiết. 

Việc triển khai sử dụng các chuỗi bên trong danh sách để tránh lặp lại các chuyển đổi số nguyên thành chuỗi trong quá trình xây dựng đầu ra. 

Giới hạn rất nhỏ nhưng giải pháp vẫn chạy theo thời gian tuyến tính và sử dụng kiểu nhập liệu nhanh phù hợp với các quy ước lập trình cạnh tranh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
```Dấu vết thực hiện: 

| Bước | n | Tình trạng | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | n là lẻ | In`-1`| 

Đầu ra:```
-1
```Điều này chứng tỏ không thể ghép nối tất cả các phần tử khi kích thước là số lẻ. 

### Ví dụ 2 

đầu vào:```
4
```Dấu vết thực hiện: 

| Lặp lại | tôi | Giá trị gia tăng | Hoán vị hiện tại | 
| --- | --- | --- | --- | 
| 1 | 1 | 2, 1 | 2 1 | 
| 2 | 3 | 4, 3 | 2 1 4 3 | 

Đầu ra:```
2 1 4 3
```Xác minh: 

| Vị trí | p[i] | p[p[i]] | 
| --- | --- | --- | 
| 1 | 2 | 1 | 
| 2 | 1 | 2 | 
| 3 | 4 | 3 | 
| 4 | 3 | 4 | 

Mỗi phần tử sẽ trở về chính nó sau hai ứng dụng và không có phần tử nào cố định sau một ứng dụng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi số được xử lý đúng một lần | 
| Không gian | O(n) | Mảng đầu ra lưu trữ hoán vị | 

Giá trị tối đa của`n`chỉ là`100`, vì vậy giải pháp này dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. Thậm chí các giải pháp chậm hơn nhiều cũng sẽ vượt qua, nhưng việc xây dựng tuyến tính là quan sát toán học dự định. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())

    if n % 2 == 1:
        print(-1)
        return

    ans = []

    for i in range(1, n + 1, 2):
        ans.append(str(i + 1))
        ans.append(str(i))

    print(" ".join(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided sample
assert run("1\n") == "-1\n", "sample 1"

# smallest valid case
assert run("2\n") == "2 1\n", "n = 2"

# small even case
assert run("4\n") == "2 1 4 3\n", "n = 4"

# odd size greater than 1
assert run("5\n") == "-1\n", "odd n"

# maximum boundary
expected = "2 1 4 3 6 5 8 7 10 9 12 11 14 13 16 15 18 17 20 19 22 21 24 23 26 25 28 27 30 29 32 31 34 33 36 35 38 37 40 39 42 41 44 43 46 45 48 47 50 49 52 51 54 53 56 55 58 57 60 59 62 61 64 63 66 65 68 67 70 69 72 71 74 73 76 75 78 77 80 79 82 81 84 83 86 85 88 87 90 89 92 91 94 93 96 95 98 97 100 99\n"
assert run("100\n") == expected, "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`-1`| Trường hợp không thể tối thiểu | 
|`2`|`2 1`| Hoán vị hợp lệ nhỏ nhất | 
|`4`|`2 1 4 3`| Nhiều giao dịch hoán đổi độc lập | 
|`5`|`-1`| Từ chối kích thước lẻ | 
|`100`| hoán đổi xen kẽ | Trường hợp ranh giới tối đa | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn đầu tiên là khi`n = 1`. 

đầu vào:```
1
```Thuật toán ngay lập tức kiểm tra tính chẵn lẻ:```
1 % 2 == 1
```Vì giá trị là số lẻ nên nó in:```
-1
```Điều này đúng vì hoán vị duy nhất có thể là:```
1
```vi phạm yêu cầu`p[i] != i`. 

Một trường hợp tinh tế khác là bất kỳ giá trị lẻ nào lớn hơn, chẳng hạn như: 

đầu vào:```
3
```Thuật toán lại phát hiện tính chẵn lẻ lẻ và in:```
-1
```Điều này tránh được một lỗi phổ biến khi ai đó chỉ hoán đổi hai yếu tố đầu tiên:```
2 1 3
```Ở đây, vị trí thứ ba ánh xạ tới chính nó nên hoán vị không hợp lệ. 

Trường hợp biên có giá trị chẵn lớn nhất được phép là: 

đầu vào:```
100
```Thuật toán tạo ra:```
2 1 4 3 6 5 ...
```Mỗi cặp liền kề tạo thành 2 chu kỳ hợp lệ. Không có chỉ số nào được so sánh và tất cả các điều kiện vẫn được thỏa mãn trong phạm vi đầy đủ từ`1`ĐẾN`100`.
