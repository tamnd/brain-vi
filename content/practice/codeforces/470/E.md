---
title: "CF 470E - Bàn Cờ"
description: "Chúng ta cần in một bàn cờ n×n sử dụng các ký tự. Hình vuông màu trắng được biểu thị bằng . và hình vuông màu đen bằng . Hình vuông ở góc trên bên trái có màu trắng và các màu sắc xen kẽ theo chiều ngang và chiều dọc giống như trên bàn cờ thật."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 470
codeforces_index: "E"
codeforces_contest_name: "Surprise Language Round 7"
rating: 1900
weight: 470
solve_time_s: 89
verified: true
draft: false
---

[CF 470E - Bàn cờ](https://codeforces.com/problemset/problem/470/E) 

**Xếp hạng:** 1900 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần in một`n × n`bàn cờ sử dụng ký tự. Hình vuông màu trắng được đại diện bởi`.`và hình vuông màu đen bởi`#`. Hình vuông ở góc trên bên trái có màu trắng và các màu sắc xen kẽ theo chiều ngang và chiều dọc giống như trên bàn cờ thật. 

Đầu vào chứa một số nguyên duy nhất`n`, cung cấp kích thước bảng. Đầu ra phải chứa`n`dòng, mỗi dòng có`n`các ký tự mô tả một hàng của bảng. 

Những hạn chế là cực kỳ nhỏ. Từ`1 ≤ n ≤ 9`, thậm chí việc tạo ra từng ô riêng lẻ cũng yêu cầu nhiều nhất`81`hoạt động. Bất kỳ giải pháp hợp lý nào cũng đủ nhanh. Nhiệm vụ không phải là tối ưu hóa mà là tái tạo chính xác mẫu màu xen kẽ. 

Một lỗi phổ biến là chọn sai màu bắt đầu. Ví dụ: 

đầu vào:```
1
```Đầu ra đúng:```
.
```Hình vuông phía trên bên trái có màu trắng nên ký tự đầu tiên phải là`.`. Bắt đầu với`#`sẽ đảo ngược toàn bộ bảng. 

Một lỗi dễ mắc phải khác là chỉ xen kẽ bên trong mỗi hàng mà quên rằng các hàng liên tiếp phải bắt đầu bằng màu đối lập. 

đầu vào:```
2
```Đầu ra đúng:```
.#
#.
```Việc thực hiện bất cẩn có thể tạo ra:```
.#
.#
```bởi vì nó khởi động lại mọi hàng có cùng màu. 

Nguồn lỗi thứ ba là việc lập chỉ mục theo từng lỗi một. Màu sắc phụ thuộc vào tổng vị trí của hàng và cột là chẵn hay lẻ. Sử dụng lập chỉ mục dựa trên một mà không điều chỉnh logic chẵn lẻ có thể lật toàn bộ bảng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là kiểm tra từng ô vuông và xác định màu sắc của nó. Đối với mỗi vị trí`(row, col)`, chúng tôi quyết định liệu nó có nên chứa`.`hoặc`#`, nối ký tự đó vào hàng hiện tại và in hàng đã hoàn thành. 

Cách tiếp cận bạo lực này đã đủ vì chỉ có`n²`tế bào. Với`n = 9`, trường hợp xấu nhất chỉ chứa`81`các vị trí. 

Quan sát quan trọng là màu sắc bàn cờ xen kẽ nhau. Nếu chúng ta lập chỉ mục các hàng và cột từ 0 thì các vị trí có số chẵn`row + col`có cùng màu với hình vuông trên cùng bên trái, trong khi các vị trí có số lẻ`row + col`có màu đối lập. 

Vì hình vuông trên cùng bên trái có màu trắng, được biểu thị bằng`.`, chúng tôi chỉ cần xuất ra:```
.
```khi`(row + col)`là chẵn, và```
#
```khi`(row + col)`thật kỳ quặc. 

Điều này cho phép chúng tôi xác định từng ô một cách độc lập bằng cách kiểm tra tính chẵn lẻ đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) không bao gồm đầu ra | Đã chấp nhận | 
| Tối ưu | O(n²) | O(1) không bao gồm đầu ra | Đã chấp nhận | 

Đối với vấn đề này, các phương pháp tiếp cận tối ưu và bạo lực thực sự giống nhau vì mọi ô đều phải xuất hiện ở đầu ra. 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`. 
2. Lặp lại qua từng chỉ mục hàng`i`từ`0`ĐẾN`n - 1`. 
3. Xây dựng hàng hiện tại theo từng ký tự. 
4. Đối với mỗi chỉ mục cột`j`từ`0`ĐẾN`n - 1`, tính toán`(i + j) % 2`. 
5. Nếu kết quả là`0`, nối thêm`.`vì hình vuông này có cùng màu với hình vuông trên cùng bên trái. 
6. Nếu không thì nối thêm`#`vì màu sắc phải xen kẽ. 
7. Sau khi xây dựng hàng, hãy in nó. 

### Tại sao nó hoạt động 

Bàn cờ xen kẽ màu sắc giữa các ô lân cận. Di chuyển một bước theo chiều ngang hoặc chiều dọc sẽ thay đổi tính chẵn lẻ của`row + col`, do đó, độ chẵn lẻ sẽ thay thế chính xác như màu sắc. Hình vuông trên cùng bên trái`(0,0)`có số chẵn chẵn và có màu trắng. Mỗi ô vuông chẵn lẻ sẽ nhận được`.`, mọi ô vuông có chẵn lẻ lẻ sẽ nhận được`#`, tạo ra chính xác mẫu bàn cờ được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

for i in range(n):
    row = []
    for j in range(n):
        if (i + j) % 2 == 0:
            row.append('.')
        else:
            row.append('#')
    print(''.join(row))
```Chương trình bắt đầu bằng cách đọc kích thước bảng. 

Vòng lặp bên ngoài xử lý các hàng, trong khi vòng lặp bên trong xử lý các cột. Đối với mỗi vị trí`(i, j)`, tính chẵn lẻ của`i + j`quyết định màu sắc. Màu chẵn lẻ tương ứng với màu giống như`(0, 0)`, có màu trắng và được biểu thị bằng`.`. Tính chẵn lẻ lẻ tương ứng với`#`. 

Mỗi hàng được tích lũy trong một danh sách và được chuyển đổi thành một chuỗi bằng cách sử dụng`''.join(row)`. Điều này tránh việc nối các chuỗi liên tục và giữ cho quá trình triển khai được rõ ràng. 

Lỗi triển khai phổ biến nhất là đảo ngược quy tắc chẵn lẻ. Từ`(0,0)`phải có màu trắng, chẵn lẻ phải ánh xạ tới`.`còn hơn là`#`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
```| Hàng ngang`i`| Col`j`|`i+j`| Chẵn lẻ | Nhân vật | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | Thậm chí | . | 
| 0 | 1 | 1 | lẻ | # | 
| 0 | 2 | 2 | Thậm chí | . | 
| 0 | 3 | 3 | lẻ | # | 

Hàng đầu tiên trở thành:```
.#.#
```Tiếp tục tương tự: 

| Hàng ngang`i`| Hàng đã tạo | 
| --- | --- | 
| 0 | .#.# | 
| 1 | #.#. | 
| 2 | .#.# | 
| 3 | #.#. | 

Đầu ra:```
.#.#
#.#.
.#.#
#.#.
```Điều này chứng tỏ rằng tính chẵn lẻ xen kẽ tự nhiên tạo ra các màu xen kẽ cần thiết theo cả chiều ngang và chiều dọc. 

### Ví dụ 2 

đầu vào:```
3
```| Hàng ngang`i`| Hàng đã tạo | 
| --- | --- | 
| 0 | .#. | 
| 1 | #.# | 
| 2 | .#. | 

Đầu ra:```
.#.
#.#
.#.
```Ví dụ này cho thấy kích thước bảng lẻ hoạt động theo cùng một cách. Mọi ô vuông liền kề đều có tính chẵn lẻ đối diện nên mẫu bàn cờ vẫn hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi ô của bảng được tạo một lần | 
| Không gian | O(1) | Chỉ một bộ đệm hàng tạm thời nhỏ được sử dụng, không bao gồm đầu ra | 

Với`n ≤ 9`, thuật toán thực hiện tối đa`81`tính toán tế bào. Giá trị này thấp hơn nhiều so với giới hạn sẵn có, vì vậy giải pháp này phù hợp một cách thoải mái trong cả giới hạn về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())
    out = []

    for i in range(n):
        row = []
        for j in range(n):
            row.append('.' if (i + j) % 2 == 0 else '#')
        out.append(''.join(row))

    return '\n'.join(out)

# provided sample
assert run("4\n") == ".#.#\n#.#.\n.#.#\n#.#.", "sample 1"

# minimum size
assert run("1\n") == ".", "single white square"

# small even board
assert run("2\n") == ".#\n#.", "alternation across rows"

# small odd board
assert run("3\n") == ".#.\n#.#\n.#.", "odd dimension board"

# maximum size
assert run("9\n").split("\n")[0] == ".#.#.#.#.", "maximum size first row"
assert len(run("9\n").split("\n")) == 9, "maximum size row count"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`.`| Kích thước bảng tối thiểu | 
|`2`|`.# / #.`| Luân phiên hàng này sang hàng đúng | 
|`3`|`.#. / #.# / .#.`| Xử lý bảng có kích thước lẻ | 
|`9`| 9 hàng bàn cờ hợp lệ | Hạn chế tối đa | 

## Vỏ cạnh 

Hãy xem xét bảng nhỏ nhất có thể. 

đầu vào:```
1
```Thuật toán chỉ truy cập`(0,0)`. Từ`0 + 0 = 0`là số chẵn, nó xuất ra`.`. 

Đầu ra:```
.
```Điều này xác nhận rằng hình vuông bắt đầu có màu trắng. 

Hãy xem xét một bảng nơi việc luân phiên hàng đóng vai trò quan trọng. 

đầu vào:```
2
```Dấu vết thực hiện: 

| Vị trí | Tổng hợp | Nhân vật | 
| --- | --- | --- | 
| (0,0) | 0 | . | 
| (0,1) | 1 | # | 
| (1,0) | 1 | # | 
| (1,1) | 2 | . | 

Đầu ra:```
.#
#.
```Ký tự đầu tiên của hàng thứ hai trở thành`#`tự động vì tính chẵn lẻ thay đổi. Điều này ngăn ngừa lỗi phổ biến khi khởi động lại mỗi hàng với`.`. 

Hãy xem xét một bảng có kích thước lẻ lớn hơn. 

đầu vào:```
5
```Hàng đầu tiên trở thành:```
.#.#.
```Hàng thứ hai trở thành:```
#.#.#
```và mô hình tiếp tục. Vì tính chẵn lẻ xác định màu sắc duy nhất nên không cần xử lý đặc biệt đối với các kích thước lẻ. Thuật toán tạo ra cấu trúc bàn cờ chính xác cho mọi`n`.
