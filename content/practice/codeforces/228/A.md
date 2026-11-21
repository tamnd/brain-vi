---
title: "CF 228A - Móng ngựa của bạn có ở móng bên kia không?"
description: "Valera có đúng bốn móng ngựa. Mỗi móng ngựa có một màu được biểu thị bằng một số nguyên. Đối với bữa tiệc, anh ấy muốn cả bốn chiếc móng ngựa đều có màu sắc khác nhau. Nếu một số móng ngựa của anh ta có cùng màu, anh ta phải mua những chiếc móng ngựa có màu mới để thay thế."
date: "2026-06-04T09:08:59+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 228
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 141 (Div. 2)"
rating: 800
weight: 228
solve_time_s: 67
verified: true
draft: false
---

[CF 228A - Móng ngựa của bạn có ở móng bên kia không?](https://codeforces.com/problemset/problem/228/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Valera có đúng bốn móng ngựa. Mỗi móng ngựa có một màu được biểu thị bằng một số nguyên. Đối với bữa tiệc, anh ấy muốn cả bốn chiếc móng ngựa đều có màu sắc khác nhau. 

Nếu một số móng ngựa của anh ta có cùng màu, anh ta phải mua những chiếc móng ngựa có màu mới để thay thế. Vì cửa hàng bán móng ngựa với bất kỳ màu nào nên câu hỏi duy nhất là anh ta cần mua thêm bao nhiêu móng ngựa để có được bốn màu riêng biệt. 

Dữ liệu đầu vào bao gồm bốn số nguyên biểu thị màu sắc của những chiếc móng ngựa mà anh ta hiện đang sở hữu. Đầu ra là một số nguyên duy nhất, số lượng móng ngựa tối thiểu phải mua. 

Những hạn chế là rất nhỏ. Chỉ có bốn giá trị và mỗi màu có thể lớn bằng$10^9$. Độ lớn của các giá trị màu không quan trọng vì chúng ta không bao giờ thực hiện phép tính trên chúng. Chúng ta chỉ cần xác định xem hai màu có bằng nhau hay không. Chỉ với bốn con số, bất kỳ thuật toán hợp lý nào cũng dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

Nguyên nhân chính của sai sót là việc đếm các bản sao không chính xác. 

Hãy xem xét đầu vào:```
1 1 1 1
```Câu trả lời đúng là:```
3
```Chỉ có một màu sắc riêng biệt nên phải thay ba chiếc móng ngựa. Việc thực hiện bất cẩn khi đếm các cặp trùng lặp có thể tạo ra số lượng lớn hơn. 

Coi như:```
1 2 3 4
```Câu trả lời đúng là:```
0
```Tất cả các màu đều đã khác nhau nên không cần phải mua gì cả. 

Coi như:```
5 5 7 8
```Câu trả lời đúng là:```
1
```Có ba màu sắc riêng biệt. Chỉ cần một màu bổ sung. 

Quan sát quan trọng là câu trả lời chỉ phụ thuộc vào số lượng màu sắc riêng biệt đã tồn tại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ so sánh từng cặp móng ngựa và cố gắng đếm các màu lặp lại theo cách thủ công. Vì chỉ có bốn chiếc móng ngựa nên điều này thực sự khả thi. Chỉ có sáu cặp nên công việc không đổi. 

Khó khăn với cách tiếp cận này không phải là hiệu suất mà là tính chính xác. Khi một màu xuất hiện ba hoặc bốn lần, việc đếm cặp trở nên khó khăn vì nhiều cặp trùng lặp chỉ tương ứng với một vài lần thay thế. Ví dụ, với màu sắc`1 1 1 1`, có sáu cặp bằng nhau nhưng đáp án chỉ có ba. 

Quan sát đơn giản hơn là mọi màu sắc riêng biệt đều có thể được giữ lại và mọi móng ngựa thừa có cùng màu hiện có phải được thay thế. Nếu có`k`màu sắc riêng biệt giữa bốn móng ngựa, thì chính xác`4 - k`móng ngựa cần thay thế. 

Ở đây, một tập hợp là một công cụ tự nhiên vì một tập hợp chỉ tự động lưu trữ các giá trị riêng biệt. Bằng cách chèn tất cả bốn màu vào một bộ, kích thước bộ ngay lập tức cho chúng ta biết có bao nhiêu màu duy nhất tồn tại. 

Câu trả lời trở thành:```
4 - number_of_distinct_colors
```| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(4²) | O(1) | Đã chấp nhận | 
| Tối ưu | O(4) | O(4) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bốn giá trị màu. 
2. Chèn tất cả bốn giá trị vào một bộ. 

Bộ chỉ giữ lại các màu duy nhất, tự động loại bỏ các màu trùng lặp. 
3. Tính số lượng các màu khác nhau theo kích thước của tập hợp. 
4. Trừ giá trị đó từ 4. 

Mọi màu sắc riêng biệt có thể vẫn còn, trong khi mọi móng ngựa còn lại phải được thay thế. 
5. In kết quả. 

### Tại sao nó hoạt động 

Bộ này chứa chính xác một bản sao của mỗi màu xuất hiện trong bốn chiếc móng ngựa. Nếu kích thước được đặt là`k`, thì có`k`màu sắc đã có thể được sử dụng mà không cần sửa đổi. Vì Valera cần tổng cộng bốn màu khác nhau nên số màu còn lại`4 - k`móng ngựa phải được thay thế bằng màu mới. Không thể mua số lượng nhỏ hơn vì mỗi móng ngựa trùng lặp sẽ góp phần thiếu một màu riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

colors = list(map(int, input().split()))
print(4 - len(set(colors)))
```Giải pháp đọc bốn số nguyên và lưu trữ chúng trong một danh sách. Việc chuyển đổi danh sách thành một bộ sẽ loại bỏ tất cả các màu trùng lặp. Kích thước của bộ đó là số lượng màu riêng biệt đã có sẵn. 

biểu hiện`4 - len(set(colors))`trực tiếp tính toán có bao nhiêu móng ngựa bị trùng lặp và do đó phải được thay thế. 

Không có điều kiện biên phức tạp. Câu trả lời nhỏ nhất có thể là`0`khi cả bốn màu đều khác nhau. Câu trả lời lớn nhất có thể là`3`khi cả bốn màu đều giống nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 7 3 3
```| Màu sắc | Đặt nội dung | Số lượng riêng biệt | Trả lời | 
| --- | --- | --- | --- | 
| 1 7 3 3 | {1, 7, 3} | 3 | 1 | 

Bộ sản phẩm có ba màu riêng biệt. Một chiếc móng ngựa trùng màu`3`, vì vậy cần có một sự thay thế. 

### Ví dụ 2 

đầu vào:```
5 5 5 5
```| Màu sắc | Đặt nội dung | Số lượng riêng biệt | Trả lời | 
| --- | --- | --- | --- | 
| 5 5 5 5 | {5} | 1 | 3 | 

Chỉ có một màu sắc riêng biệt tồn tại. Phải có thêm ba màu nữa, vì vậy câu trả lời là`3`. 

Ví dụ này chứng minh rằng thuật toán xử lý chính xác số lượng bản sao tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(4) | Bốn giá trị được chèn vào một tập hợp | 
| Không gian | O(4) | Bộ lưu trữ tối đa bốn màu riêng biệt | 

Vì kích thước đầu vào được cố định ở bốn giá trị nên thời gian chạy và mức sử dụng bộ nhớ thực tế là không đổi. Giải pháp dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    colors = list(map(int, input().split()))
    print(4 - len(set(colors)))

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
assert run("1 7 3 3\n") == "1\n", "sample 1"

# custom cases
assert run("1 2 3 4\n") == "0\n", "all distinct"
assert run("1 1 1 1\n") == "3\n", "all equal"
assert run("5 5 7 8\n") == "1\n", "one duplicate"
assert run("1000000000 1 1000000000 2\n") == "1\n", "large values"
assert run("9 9 8 8\n") == "2\n", "two duplicated colors"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 3 4`|`0`| Không cần mua hàng | 
|`1 1 1 1`|`3`| Số lần thay thế tối đa | 
|`5 5 7 8`|`1`| Màu trùng lặp đơn | 
|`1000000000 1 1000000000 2`|`1`| Giá trị màu lớn | 
|`9 9 8 8`|`2`| Nhiều nhóm trùng lặp | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1 1 1 1
```Tập hợp trở thành`{1}`. Kích thước của nó là`1`, do đó thuật toán tính toán`4 - 1 = 3`. Ba chiếc móng ngựa phải được thay thế vì chỉ tồn tại một màu sắc riêng biệt. 

Hãy xem xét đầu vào:```
1 2 3 4
```Tập hợp trở thành`{1, 2, 3, 4}`. Kích thước của nó là`4`, vậy câu trả lời là`4 - 4 = 0`. Mỗi móng ngựa đã có một màu sắc độc đáo. 

Hãy xem xét đầu vào:```
5 5 7 8
```Tập hợp trở thành`{5, 7, 8}`. Kích thước của nó là`3`, sản xuất`4 - 3 = 1`. Chỉ có một móng ngựa trùng lặp với màu hiện có. 

Hãy xem xét đầu vào:```
9 9 8 8
```Tập hợp trở thành`{8, 9}`. Kích thước của nó là`2`, sản xuất`4 - 2 = 2`. Cần có thêm hai màu riêng biệt. Thuật toán xử lý nhiều nhóm màu trùng lặp mà không cần bất kỳ logic đặc biệt nào vì kích thước đã đặt sẽ nắm bắt tất cả thông tin cần thiết.
