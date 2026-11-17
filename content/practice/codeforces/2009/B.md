---
title: "CF 2009B - osu!mania"
description: "Mỗi trường hợp thử nghiệm mô tả một bản đồ nhịp osu!mania nhỏ có 4 cột. Mỗi hàng chứa chính xác một ghi chú, được đánh dấu bằng , và các ô còn lại chứa .."
date: "2026-06-08T13:17:30+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2009
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 971 (Div. 4)"
rating: 800
weight: 2009
solve_time_s: 120
verified: false
draft: false
---

[CF 2009B - osu!mania](https://codeforces.com/problemset/problem/2009/B) 

**Đánh giá:** 800 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 2 phút 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm mô tả một bản đồ nhịp osu!mania nhỏ có 4 cột. Mỗi hàng chứa chính xác một ghi chú, được đánh dấu bằng`#`, và các ô còn lại chứa`.`. 

Các hàng được đưa ra từ trên xuống dưới, nhưng trò chơi xử lý các nốt từ dưới lên trên vì các nốt gần cuối được đánh trước. Đối với mỗi ghi chú trong thứ tự xử lý, chúng ta phải xuất ra số cột nơi ghi chú đó được ghi chú.`#`xuất hiện. 

Một hàng như:```
..#.
```chứa ghi chú của nó trong cột 3. 

Chi tiết quan trọng là thứ tự đầu vào và thứ tự đầu ra cần thiết trái ngược nhau. Hàng đầu tiên trong dữ liệu nhập là hàng trên cùng nhưng câu trả lời đầu tiên chúng ta in ra phải tương ứng với hàng dưới cùng. 

Những hạn chế là cực kỳ nhỏ. Có tối đa 500 hàng trong tất cả các trường hợp thử nghiệm và mỗi hàng có chính xác 4 ký tự. Ngay cả một thuật toán quét mọi ký tự của mỗi hàng cũng chỉ thực hiện tổng cộng khoảng 2000 ký tự. Thời gian chạy không phải là thách thức ở đây. Nhiệm vụ hoàn toàn là thực hiện cẩn thận. 

Một lỗi dễ mắc phải là quên đảo ngược các hàng. 

Coi như:```
1
3
#...
.#..
..#.
```Các hàng từ trên xuống dưới chứa các cột`1, 2, 3`. 

Trò chơi xử lý từ dưới lên trên nên kết quả đúng là:```
3 2 1
```Một giải pháp bất cẩn in các hàng theo thứ tự đầu vào sẽ tạo ra:```
1 2 3
```điều đó là sai. 

Một lỗi phổ biến khác là sử dụng lập chỉ mục dựa trên 0. 

Ví dụ:```
1
1
...#
```Ghi chú nằm ở cột thứ tư. 

Đầu ra đúng là:```
4
```Một giải pháp xuất trực tiếp vị trí chuỗi sẽ in`3`. 

Trường hợp khó phát hiện thứ ba là khi nhiều hàng có ghi chú trong cùng một cột.```
1
2
.#..
.#..
```Đầu ra đúng là:```
2 2
```Mỗi hàng phải được xử lý độc lập. Không có hạn chế rằng các cột phải khác biệt. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là kiểm tra từng hàng, tìm vị trí của`#`, lưu trữ số cột đó và cuối cùng xuất ra các câu trả lời đã lưu theo thứ tự hàng ngược lại. 

Phương pháp brute-force này đã đủ nhanh vì mỗi hàng chỉ chứa bốn ký tự. Việc tìm vị trí ghi chú yêu cầu kiểm tra tối đa bốn ô. Với tổng số tối đa 500 hàng, công việc trong trường hợp xấu nhất chỉ kiểm tra khoảng 2000 ký tự. 

Vì các ràng buộc rất nhỏ nên không cần phải tối ưu hóa phức tạp. Sự quan sát thực sự chỉ đơn giản là hiểu thứ tự xử lý các ghi chú. 

Dữ liệu nhập đưa ra các hàng từ trên xuống dưới, nhưng câu trả lời bắt buộc là từ dưới lên trên. Khi chúng tôi nhận ra điều đó, vấn đề sẽ trở thành quá trình quét đơn giản từng hàng. Đối với mỗi hàng, chúng tôi xác định cột chứa`#`, lưu trữ và sau đó in các cột được lưu trữ theo thứ tự ngược lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

Vì mỗi hàng có độ dài cố định là 4 nên việc quét một hàng thực sự là thời gian không đổi. Cả hai cách tiếp cận đều giống nhau cho vấn đề này. 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, hãy đọc`n`, số hàng. 
3. Tạo danh sách trống`cols`nó sẽ lưu trữ số cột của ghi chú trong mỗi hàng. 
4. Đọc từng hàng. 
5. Tìm vị trí của`#`trong hàng và chuyển đổi nó thành số cột dựa trên 1. 
6. Nối số cột đó vào`cols`. 
7. Sau khi tất cả các hàng được xử lý, đảo ngược thứ tự bằng cách lặp qua`cols`từ phần tử cuối cùng đến phần tử đầu tiên. 
8. In trình tự đảo ngược cách nhau bằng dấu cách. 

Việc đảo ngược là cần thiết vì đầu vào được đưa ra từ trên xuống dưới trong khi các ghi chú được xử lý từ dưới lên trên. 

### Tại sao nó hoạt động 

Với mỗi hàng có đúng một`#`, do đó cột của nốt đó được xác định duy nhất. Thuật toán ghi lại cột đó cho mỗi hàng theo thứ tự đầu vào. 

Nếu các hàng được đánh số từ trên xuống dưới như`1...n`, thì thứ tự xử lý là`n...1`. Đảo ngược danh sách đã ghi sẽ tạo ra chính xác thứ tự đó. Mỗi vị trí đầu ra đều tương ứng với nốt được xử lý chính xác nên thuật toán luôn trả về trình tự được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    cols = []

    for _ in range(n):
        row = input().strip()
        cols.append(row.index('#') + 1)

    print(*reversed(cols))
```Giải pháp tuân theo thuật toán trực tiếp. 

Danh sách`cols`lưu trữ số cột cho mỗi hàng. của Python`index('#')`tìm vị trí của ghi chú và việc thêm một ghi chú sẽ chuyển đổi kết quả từ việc lập chỉ mục dựa trên 0 thành đánh số cột dựa trên 1 được yêu cầu. 

Sau khi tất cả các hàng được đọc,`reversed(cols)`tạo ra thứ tự xử lý từ dưới lên trên. sử dụng`print(*...)`tự động đặt khoảng trắng giữa các số. 

Lỗi triển khai phổ biến nhất là quên`+ 1`. Một cái khác là in ấn`cols`trực tiếp thay vì đảo ngược nó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
#...
.#..
..#.
...#
```| Hàng đọc | Cột Ghi chú | cols Sau khi xử lý | 
| --- | --- | --- | 
| #... | 1 | [1] | 
| .#.. | 2 | [1, 2] | 
| ..#. | 3 | [1, 2, 3] | 
| ...# | 4 | [1, 2, 3, 4] | 

Sau khi đảo chiều: 

| Thứ tự đảo ngược | 
| --- | 
| 4 | 
| 3 | 
| 2 | 
| 1 | 

Đầu ra:```
4 3 2 1
```Ví dụ này thể hiện ý tưởng trung tâm của vấn đề. Các cột được tìm thấy theo thứ tự đầu vào nhưng được in theo thứ tự ngược lại. 

### Mẫu 2 

đầu vào:```
2
.#..
.#..
```| Hàng đọc | Cột Ghi chú | cols Sau khi xử lý | 
| --- | --- | --- | 
| .#.. | 2 | [2] | 
| .#.. | 2 | [2, 2] | 

Sau khi đảo chiều: 

| Thứ tự đảo ngược | 
| --- | 
| 2 | 
| 2 | 

Đầu ra:```
2 2
```Dấu vết này cho thấy nhiều ghi chú có thể chiếm cùng một cột. Mỗi hàng được xử lý độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi hàng được quét một lần | 
| Không gian | O(n) | Cột của mỗi hàng được lưu trữ | 

Đây,`n`là số hàng trong một trường hợp thử nghiệm. Vì mỗi hàng có chính xác bốn ký tự nên hằng số ẩn cực kỳ nhỏ. Với tối đa 500 hàng trên tất cả các trường hợp thử nghiệm, giải pháp dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        cols = []

        for _ in range(n):
            row = input().strip()
            cols.append(row.index('#') + 1)

        out.append(" ".join(map(str, reversed(cols))))

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return result

# provided sample
assert run(
"""3
4
#...
.#..
..#.
...#
2
.#..
.#..
1
...#
"""
) == "4 3 2 1\n2 2\n4"

# minimum size
assert run(
"""1
1
#...
"""
) == "1"

# last column
assert run(
"""1
1
...#
"""
) == "4"

# repeated columns
assert run(
"""1
3
.#..
.#..
.#..
"""
) == "2 2 2"

# catches missing reversal
assert run(
"""1
4
#...
.#..
..#.
...#
"""
) == "4 3 2 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hàng đơn với`#...`|`1`| Kích thước đầu vào tối thiểu | 
| Hàng đơn với`...#`|`4`| Lập chỉ mục dựa trên 1 đúng | 
| Ba hàng giống nhau ở cột 2 |`2 2 2`| Các cột lặp lại được xử lý chính xác | 
| Tăng cột từ trên xuống dưới |`4 3 2 1`| Đảo ngược trình tự xử lý | 

## Vỏ cạnh 

### Thứ tự từ trên xuống so với thứ tự từ dưới lên trên 

đầu vào:```
1
3
#...
.#..
..#.
```Thuật toán ghi lại:```
[1, 2, 3]
```Sau đó nó đảo ngược danh sách:```
[3, 2, 1]
```và kết quả đầu ra:```
3 2 1
```Điều này phù hợp với thứ tự xử lý cần thiết. 

### Ghi chú ở cột cuối cùng 

đầu vào:```
1
1
...#
```Vị trí của`#`là chỉ số`3`. Thêm một cột cho cột`4`. 

Đầu ra:```
4
```Điều này xác nhận rằng thuật toán chuyển đổi chính xác từ đánh số cột dựa trên 0 của Python sang đánh số cột dựa trên 1 của vấn đề. 

### Nhiều hàng sử dụng cùng một cột 

đầu vào:```
1
2
.#..
.#..
```Thuật toán lưu trữ:```
[2, 2]
```Đảo ngược không làm thay đổi trình tự:```
[2, 2]
```Đầu ra:```
2 2
```Giải pháp không bao giờ giả định các cột là duy nhất, do đó các cột lặp lại được xử lý một cách tự nhiên.
