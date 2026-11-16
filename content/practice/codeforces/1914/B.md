---
title: "CF 1914B - Chuẩn bị cho cuộc thi"
description: "Chúng tôi gặp vấn đề với các khó khăn từ 1 đến n. Chúng ta phải sắp xếp những con số này thành một hoán vị thể hiện thứ tự Monocarp giải chúng. Bất cứ khi nào một bài toán khó hơn bài toán trước đó theo thứ tự đã chọn, Monocarp sẽ trở nên hào hứng."
date: "2026-06-08T20:01:49+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1914
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 916 (Div. 3)"
rating: 800
weight: 1914
solve_time_s: 146
verified: false
draft: false
---

[CF 1914B - Chuẩn bị cho cuộc thi](https://codeforces.com/problemset/problem/1914/B) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 2 phút 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi gặp vấn đề với những khó khăn`1`bởi vì`n`. Chúng ta phải sắp xếp những con số này thành một hoán vị thể hiện thứ tự Monocarp giải chúng. 

Bất cứ khi nào một bài toán khó hơn bài toán trước đó theo thứ tự đã chọn, Monocarp sẽ trở nên hào hứng. Vấn đề được giải quyết đầu tiên không bao giờ được tính vì không có vấn đề nào trước đó để so sánh. 

Đối với một hoán vị`p`, một sự phấn khích xảy ra ở vị trí`i > 1`khi`p[i] > p[i-1]`. Nhiệm vụ là xây dựng bất kỳ hoán vị nào của`1...n`điều đó tạo ra chính xác`k`những vị trí như vậy. 

Những hạn chế là rất nhỏ. Có thể có tới 1000 trường hợp thử nghiệm, nhưng`n`nhiều nhất là 50. Ngay cả một`O(n²)`việc xây dựng sẽ là chuyện nhỏ. Thách thức không phải là hiệu quả, mà là tìm ra một cách xây dựng đơn giản luôn tạo ra chính xác số lượng các cặp liền kề tăng dần theo yêu cầu. 

Một sai lầm phổ biến là nghĩ rằng số lượng hứng thú phụ thuộc vào số lượng phần tử lớn hơn xuất hiện sau trong hoán vị. Chỉ những so sánh liền kề mới quan trọng. 

Ví dụ, với```
5 4 3 2 1
```không có sự phấn khích nào vì mọi so sánh liền kề đều giảm đi, mặc dù có nhiều số lớn hơn tồn tại trước đó trong chuỗi. 

Một cạm bẫy dễ dàng khác xuất hiện khi`k = n - 1`. Trong trường hợp đó, mọi so sánh liền kề đều phải tăng, do đó hoán vị hợp lệ duy nhất là thứ tự tăng dần:```
1 2 3 4 5
```Một công trình vô tình đưa vào dù chỉ một mức giảm sẽ chỉ tạo ra`n - 2`sự phấn khích. 

Cực đoan ngược lại là`k = 0`. Sau đó, mọi so sánh liền kề phải giảm, do đó hoán vị giảm nghiêm ngặt sẽ hoạt động:```
5 4 3 2 1
```Bất kỳ công trình nào đặt số nhỏ hơn trước số lớn hơn sẽ ngay lập tức tạo ra sự phấn khích không mong muốn. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Tạo hoán vị của`1...n`, đếm xem có bao nhiêu vị trí thỏa mãn`p[i] > p[i-1]`, và dừng khi số đếm bằng`k`. 

Việc đếm chính nó mất`O(n)`thời gian cho mỗi hoán vị. Vấn đề là số lượng hoán vị. có`n!`khả năng. Ngay cả đối với`n = 10`, con số này đã lên tới hơn 3,6 triệu hoán vị, khiến cho việc tìm kiếm toàn diện trở nên không thực tế. 

Quan sát quan trọng là chúng ta không cần phải tìm kiếm. Chúng ta chỉ cần một hoán vị với số lượng các cặp liền kề tăng dần theo quy định. 

Giả sử chúng ta đặt các số```
1 2 3 ... k+1
```lúc đầu. Mọi so sánh liền kề bên trong tiền tố này đều tăng lên, vì vậy nó đóng góp chính xác`k`sự phấn khích. 

Bây giờ hãy xem xét các số còn lại:```
k+2, k+3, ..., n
```Nếu chúng ta nối chúng theo thứ tự ngược lại:```
n, n-1, ..., k+2
```thì mọi so sánh bên trong hậu tố đó sẽ giảm dần. 

Còn ranh giới giữa hai phần thì sao? Phần tử cuối cùng của tiền tố tăng dần là`k+1`và phần tử đầu tiên của hậu tố đảo ngược là`n`. Từ`n > k+1`, ranh giới này đóng góp thêm một sự phấn khích. 

Điều đó có nghĩa là việc xây dựng trên tạo ra`k+1`sự phấn khích, không`k`. 

Để khắc phục điều này, thay vào đó chúng tôi làm cho phần tăng dần chứa chính xác`k`số:```
1 2 3 ... k
```và đảo ngược phần còn lại:```
n, n-1, ..., k+1
```Sự so sánh giữa`k`Và`n`đang tăng lên, tạo ra một sự phấn khích. Bản thân tiền tố góp phần`k-1`sự phấn khích. Cùng nhau:```
(k - 1) + 1 = k
```Chính xác những gì chúng ta cần. 

### So sánh độ phức tạp 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo danh sách chứa các số`1, 2, ..., k`. 

Bên trong tiền tố này có chính xác`k - 1`tăng so sánh liền kề. 
2. Lấy các số còn lại`k + 1, k + 2, ..., n`. 

Đây là tất cả các giá trị chưa được sử dụng. 
3. Nối các số còn lại theo thứ tự ngược lại:```
n, n-1, ..., k+1
```Mọi so sánh liền kề bên trong hậu tố này đều giảm dần. 
4. Xuất ra hoán vị kết quả. 

Ranh giới giữa hai phần là`k`theo sau là`n`, điều này đóng góp chính xác thêm một sự phấn khích. 

### Tại sao nó hoạt động 

Tiền tố`1, 2, ..., k`chứa`k-1`tăng các cặp liền kề vì mọi cặp lân cận đều tăng. 

Hậu tố`n, n-1, ..., k+1`chứa các cặp liền kề tăng dần bằng 0 vì mọi cặp lân cận đều giảm. 

Sự so sánh duy nhất vượt qua ranh giới là giữa`k`Và`n`, và kể từ đó`n > k`, nó đóng góp chính xác một cặp liền kề tăng dần. 

Do đó tổng số lần phấn khích là```
(k - 1) + 1 + 0 = k.
```Khi`k = 0`, tiền tố trống và toàn bộ hoán vị trở thành```
n, n-1, ..., 1
```không có sự phấn khích nào. Do đó việc xây dựng hoạt động với mọi giá trị hợp lệ của`k`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n, k = map(int, input().split())

    ans = list(range(1, k + 1))
    ans.extend(range(n, k, -1))

    print(*ans)
```Dòng đầu tiên của cấu trúc tạo tiền tố tăng dần`1...k`. 

Dòng thứ hai nối tất cả các số còn lại theo thứ tự giảm dần. cuộc gọi```
range(n, k, -1)
```tạo ra```
n, n-1, ..., k+1
```đó chính xác là hậu tố được yêu cầu bởi bằng chứng. 

Vụ án`k = 0`được xử lý tự động. Sau đó```
range(1, 1)
```tạo ra một tiền tố trống và hậu tố trở thành```
n, n-1, ..., 1.
```Không cần phân nhánh đặc biệt. 

Vụ án`k = n - 1`cũng được xử lý một cách tự nhiên. Tiền tố trở thành```
1, 2, ..., n-1
```và hậu tố chỉ chứa`n`, tạo ra hoán vị tăng đầy đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 6, k = 2
```| Bước | Tiền tố | Hậu tố | Kết quả | 
| --- | --- | --- | --- | 
| Xây dựng tiền tố | 1 2 | - | 1 2 | 
| Xây dựng hậu tố | 1 2 | 6 5 4 3 | 1 2 6 5 4 3 | 

So sánh liền kề: 

| Cặp | Tăng dần? | 
| --- | --- | 
| 1 → 2 | Có | 
| 2 → 6 | Có | 
| 6 → 5 | Không | 
| 5 → 4 | Không | 
| 4 → 3 | Không | 

Tổng số hứng thú = 2. 

Dấu vết này cho thấy tiền tố đóng góp một sự phấn khích và ranh giới đóng góp thêm một sự phấn khích như thế nào. 

### Ví dụ 2 

đầu vào:```
n = 5, k = 0
```| Bước | Tiền tố | Hậu tố | Kết quả | 
| --- | --- | --- | --- | 
| Xây dựng tiền tố | trống | - | trống | 
| Xây dựng hậu tố | trống | 5 4 3 2 1 | 5 4 3 2 1 | 

So sánh liền kề: 

| Cặp | Tăng dần? | 
| --- | --- | 
| 5 → 4 | Không | 
| 4 → 3 | Không | 
| 3 → 2 | Không | 
| 2 → 1 | Không | 

Tổng số hứng thú = 0. 

Ví dụ này thực hiện số lần kích thích nhỏ nhất có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi số từ 1 đến n được tạo một lần | 
| Không gian | O(n) | Hoán vị câu trả lời được lưu trữ | 

Từ`n ≤ 50`, thời gian chạy rất nhỏ. Ngay cả với 1000 trường hợp thử nghiệm, chương trình vẫn xử lý tối đa 50.000 con số, thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, k = map(int, input().split())
        ans = list(range(1, k + 1))
        ans.extend(range(n, k, -1))
        out.append(" ".join(map(str, ans)))

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# provided sample properties
assert run("1\n6 2\n") == "1 2 6 5 4 3"

# minimum size, k = 0
assert run("1\n2 0\n") == "2 1"

# minimum size, k = 1
assert run("1\n2 1\n") == "1 2"

# fully increasing
assert run("1\n5 4\n") == "1 2 3 4 5"

# fully decreasing
assert run("1\n5 0\n") == "5 4 3 2 1"

# larger boundary case
assert run("1\n7 3\n") == "1 2 3 7 6 5 4"
```### Tóm tắt phạm vi kiểm tra 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 0`|`2 1`| Kích thước tối thiểu, không có hứng thú | 
|`2 1`|`1 2`| Kích thước tối thiểu, hứng thú tối đa | 
|`5 4`|`1 2 3 4 5`| Hoán vị tăng đầy đủ | 
|`5 0`|`5 4 3 2 1`| Hoán vị giảm hoàn toàn | 
|`7 3`|`1 2 3 7 6 5 4`| Đóng góp ranh giới đúng | 

## Vỏ cạnh 

###Trường hợp 1: Không có hứng thú 

đầu vào:```
1
5 0
```Tiền tố trống. Hậu tố trở thành:```
5 4 3 2 1
```Mọi so sánh liền kề đều giảm, do đó số lượng phấn khích bằng không. Việc xây dựng tạo ra chính xác giá trị yêu cầu. 

### Trường hợp 2: Sự phấn khích tối đa có thể 

đầu vào:```
1
5 4
```Tiền tố là:```
1 2 3 4
```Hậu tố chỉ chứa:```
5
```Hoán vị cuối cùng là:```
1 2 3 4 5
```Cả bốn phép so sánh liền kề đều tăng, cho kết quả chính xác`n - 1 = 4`sự phấn khích. 

### Trường hợp 3: Đóng góp biên 

đầu vào:```
1
6 2
```Việc xây dựng mang lại:```
1 2 6 5 4 3
```Tiền tố đóng góp một sự phấn khích (`1 → 2`). Ranh giới đóng góp một sự phấn khích (`2 → 6`). Hậu tố không đóng góp gì. Tổng số chính xác là hai. 

Trường hợp này thể hiện ý tưởng trung tâm đằng sau việc xây dựng: tiền tố cung cấp`k - 1`sự phấn khích, và ranh giới cung cấp cái cuối cùng.
