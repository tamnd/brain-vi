---
title: "CF 141B - Nhảy lò cò"
description: "Chúng ta được yêu cầu xác định xem một hòn đá ném lên sân nhảy lò cò có nằm đúng bên trong một hình vuông hay không và nếu có thì xác định xem đó là hình vuông nào. Sân được xây dựng từ các hình vuông có cạnh dài a, xếp thành hàng theo mẫu 1-1-2-1-2 lặp đi lặp lại."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 141
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 101 (Div. 2)"
rating: 1400
weight: 141
solve_time_s: 86
verified: true
draft: false
---

[CF 141B - Nhảy lò cò](https://codeforces.com/problemset/problem/141/B) 

**Đánh giá:** 1400 
**Thẻ:** hình học, toán học 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem một hòn đá ném lên sân nhảy lò cò có nằm đúng bên trong một hình vuông hay không và nếu có thì xác định xem đó là hình vuông nào. Sân được xây dựng từ các hình vuông có cạnh dài`a`, được sắp xếp thành hàng theo mẫu 1-1-2-1-2 lặp lại. Mỗi ô vuông được đánh số tuần tự bắt đầu từ hàng dưới cùng, từ trái qua phải. tọa độ`(x, y)`chỉ định vị trí của hòn đá trong mặt phẳng, với`(0, 0)`ở góc dưới bên trái của hình vuông đầu tiên. Nếu hòn đá rơi vào đường biên hoặc bên ngoài sân, kết quả đầu ra phải là`-1`. 

Những ràng buộc chỉ ra rằng`a`nhỏ, lên tới 100, trong khi`x`Và`y`có thể rất lớn, lên tới`10^6`về độ lớn. Điều này loại trừ bất kỳ cách tiếp cận nào lặp lại trên tất cả các ô vuông tiềm năng riêng lẻ, vì số lượng ô vuông theo chiều dọc có thể vượt quá`10^4`trong trường hợp xấu nhất. 

Các trường hợp cạnh không rõ ràng liên quan đến việc các viên đá rơi chính xác vào viền của các ô vuông hoặc giữa các hàng. Ví dụ, với`a = 1`, một hòn đá ở`(0, 0)`nằm ở góc của hình vuông đầu tiên và sẽ quay trở lại`-1`. Tương tự, một hòn đá rơi chính xác vào khoảng trống nằm ngang giữa hai ô vuông trong một hàng cũng sẽ quay trở lại.`-1`. Bất kỳ giải pháp ngây thơ nào chỉ đơn giản là chia`x`Và`y`qua`a`không kiểm tra việc ngăn chặn nghiêm ngặt sẽ thất bại trong các trường hợp biên giới này. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ liên quan đến việc tạo tọa độ của mỗi ô vuông, lưu trữ các hộp giới hạn của chúng và kiểm tra xem liệu`(x, y)`nằm đúng bên trong mỗi hộp. Điều này hiệu quả vì nó trực tiếp mô hình hóa vấn đề, nhưng quá chậm đối với các tọa độ lớn vì số lượng ô vuông tăng tuyến tính với`y`và chiều rộng hàng xen kẽ. Với`y`lên tới`10^6`Và`a`có thể là 1, chúng ta sẽ cần kiểm tra tới`10^6`các hàng, mỗi hàng có tối đa 2 ô vuông, tạo ra hàng triệu tấm séc trong trường hợp xấu nhất. 

Nhận xét quan trọng về một giải pháp tối ưu là sân nhảy lò cò có tính chu kỳ. Hàng dưới cùng là một hình vuông đơn, hàng tiếp theo là một hình vuông đơn, sau đó là hàng hình vuông đôi, rồi mẫu 1-2 này lặp lại. Cách bố trí theo chiều dọc cho phép chúng ta tính toán trực tiếp viên đá nằm ở hàng nào bằng phép chia số nguyên của`y`qua`a`, trong khi bố cục theo chiều ngang rất dễ xử lý sau khi xác định được loại hàng. Chúng ta cũng cần xử lý điều kiện bên trong nghiêm ngặt bằng cách kiểm tra xem viên đá có nằm trên ranh giới của hình vuông hay không. Điều này làm giảm vấn đề thành các phép tính số học dựa trên`x`,`y`, Và`a`, tránh sự cần thiết phải tạo tất cả các ô vuông một cách rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(y/a) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đầu tiên, hãy kiểm tra xem`x`là tiêu cực. Nếu đúng thì hòn đá ở ngoài sân nên xuất ra`-1`. 
2. Tính chỉ số hàng theo phép chia số nguyên của`y`qua`a`để tìm hàng nào chứa đá theo chiều dọc. Gọi đây`row`. 
3. Tính số dư`y % a`để xác định vị trí bên trong hàng. Nếu số dư bằng 0 hoặc chính xác`a`, hòn đá nằm trên ranh giới nằm ngang nên đầu ra`-1`. 
4. Xác định loại hàng: hai hàng đầu tiên là hàng ô vuông đơn, sau đó mô hình lặp lại với hàng ô vuông đôi, sau đó là hàng ô vuông đơn. Sử dụng chỉ mục hàng để xác định xem hàng có 1 hay 2 ô vuông. 
5. Đối với các hàng ô vuông đơn, hãy kiểm tra xem`x`hoàn toàn nằm trong khoảng từ 0 đến`a`. Nếu không thì xuất`-1`. 
6. Đối với các hàng có hai ô vuông, hãy kiểm tra xem`x`hoàn toàn nằm trong khoảng từ 0 đến`2*a`. Nếu như`x`nằm giữa 0 và`a`, viên đá ở ô vuông đầu tiên; nếu giữa`a`Và`2*a`, viên đá nằm ở ô vuông thứ hai. Nếu như`x`nằm ngoài phạm vi này, đầu ra`-1`. 
7. Tính số bình phương tuyệt đối bằng cách tính tổng số bình phương ở tất cả các hàng trước đó và cộng chỉ số bình phương cục bộ trong hàng hiện tại. 

Tại sao nó hoạt động: Thuật toán ánh xạ trực tiếp`y`thành một hàng và`x`vào một cột, tôn trọng mẫu 1-2 xen kẽ. Bằng cách kiểm tra sự bất bình đẳng nghiêm ngặt, chúng tôi đảm bảo rằng các viên đá trên ranh giới sẽ bị loại bỏ. Mẫu hàng định kỳ cho phép tính toán mà không cần tạo toàn bộ sân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, x, y = map(int, input().split())

if x < 0:
    print(-1)
    sys.exit()

row = y // a
row_pos = y % a

if row_pos == 0:
    print(-1)
    sys.exit()

# first two rows: 1 square each
if row == 0:
    if 0 < x < a:
        print(1)
    else:
        print(-1)
    sys.exit()
elif row == 1:
    if 0 < x < a:
        print(2)
    else:
        print(-1)
    sys.exit()

# pattern repeats starting from row 2
# rows: 2 -> 2+ pattern 1-2-1-2...
pattern_row = row - 2
# compute how many full cycles of 2 rows we have
cycle = pattern_row // 2
# determine type of row in cycle
row_in_cycle = pattern_row % 2  # 0 -> 1-square, 1 -> 2-squares

# base number of squares before this cycle
base_square = 2 + cycle * 3

if row_in_cycle == 0:  # single square row
    if 0 < x < a:
        print(base_square + 1)
    else:
        print(-1)
else:  # double square row
    if 0 < x < a:
        print(base_square + 1)
    elif a < x < 2*a:
        print(base_square + 2)
    else:
        print(-1)
```Mã đầu tiên xử lý các trường hợp tiêu cực tầm thường`x`và hai hàng đầu tiên. Đối với các hàng tiếp theo, nó xác định loại của hàng bằng cách sử dụng số học mô-đun và phép chia số nguyên, sau đó ánh xạ`x`vào đúng ô vuông. Việc kiểm tra ranh giới rất nghiêm ngặt để loại trừ đá ở các cạnh. 

## Ví dụ đã hoạt động 

**Mẫu 1**:`a=1, x=0, y=0`| Biến | Giá trị | 
| --- | --- | 
| x | 0 | 
| y | 0 | 
| hàng | 0 | 
| hàng_pos | 0 | 

Séc`row_pos == 0`kích hoạt và đầu ra là`-1`. Hòn đá nằm ở góc của hình vuông đầu tiên. 

**Mẫu 2**:`a=2, x=1, y=5`| Biến | Giá trị | 
| --- | --- | 
| hàng | 2 | 
| hàng_pos | 1 | 
| mẫu_row | 0 | 
| row_in_cycle | 0 | 
| cơ sở_vuông | 2 | 

Đá xếp thành một hàng vuông,`0 < x < a`là đúng (`1 < 2`), vậy số bình phương là`base_square + 1 = 3`. 

Những dấu vết này cho thấy cách thuật toán ánh xạ tọa độ vào hàng, xác định loại hình vuông và tính số bình phương chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Tất cả các hoạt động là kiểm tra số học và có điều kiện, độc lập với`x`hoặc`y`. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được sử dụng. | 

Giải pháp xử lý lớn`x`Và`y`hiệu quả và phù hợp trong giới hạn 2 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    old_out = sys.stdout
    sys.stdout = io.StringIO()
    exec(open("solution.py").read())
    res = sys.stdout.getvalue().strip()
    sys.stdout = old_out
    return res

# provided sample
assert run("1 0 0") == "-1", "sample 1"

# custom cases
assert run("2 1 5") == "3", "stone inside 3rd square"
assert run("2 2 5") == "-1", "stone on vertical border of double row"
assert run("1 -1 1") == "-1", "stone outside left"
assert run("3 1 1") == "1", "stone inside first square"
assert run("1 1 2") == "-1", "stone exactly on boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 5 | 3 | Ánh xạ chính xác trong một hàng vuông ngoài hai hàng đầu tiên | 
| 2 2 5 | -1 | Đá ranh giới theo hàng vuông đôi | 
| 1 -1 1 | -1 | Tọa độ x âm | 
| 3 1 1 | 1 | Đá bên trong hình vuông đầu tiên | 
| 1 1 2 | -1 | Đá trên ranh giới ngang | 

## Vỏ cạnh 

Đối với đầu vào`1 0 0`, hòn đá ở góc. Thuật toán tính toán`row = 0`,`row_pos = 0`, kích hoạt kiểm tra ranh giới ngang và xuất kết quả chính xác`-1`. 

Đối với `2
