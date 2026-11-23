---
title: "CF 60A - Bột của tôi ở đâu?"
description: "Có n hộp xếp thành một đường thẳng. Chính xác một trong số chúng có thể chứa các mảnh ngũ cốc. Bạn cùng phòng để lại lời khai có hai dạng. Nếu gợi ý nói \"Ở bên trái của i\", thì các vảy phải ở đâu đó ngay trước hộp i. Bản thân hộp i cũng không thể được."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 60
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 56"
rating: 1300
weight: 60
solve_time_s: 100
verified: true
draft: false
---

[CF 60A - Mảnh vụn của tôi ở đâu?](https://codeforces.com/problemset/problem/60/A) 

**Đánh giá:** 1300 
**Thẻ:** triển khai, hai con trỏ 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

có`n`hộp xếp thành một đường thẳng. Chính xác một trong số chúng có thể chứa các mảnh ngũ cốc. Bạn cùng phòng để lại lời khai có hai dạng. 

Nếu gợi ý nói`"To the left of i"`, thì các vảy phải ở đâu đó nghiêm ngặt trước hộp`i`. Hộp`i`bản thân nó cũng là không thể được. 

Nếu gợi ý nói`"To the right of i"`, thì các vảy phải ở đâu đó sau hộp`i`. Một lần nữa, hộp`i`bản thân nó bị loại trừ. 

Nhiệm vụ là xác định còn lại bao nhiêu ô sau khi áp dụng mọi gợi ý. Nếu không có ô nào thỏa mãn đồng thời tất cả các gợi ý thì các gợi ý đó sẽ mâu thuẫn với nhau và câu trả lời là`-1`. 

Những hạn chế là rất nhỏ. Cả hai`n`Và`m`nhiều nhất là`1000`, vì vậy ngay cả một`O(nm)`giải pháp chỉ thực hiện khoảng một triệu thao tác trong trường hợp xấu nhất, dễ dàng phù hợp với thời hạn. Điều đó có nghĩa là chúng tôi không cần cấu trúc dữ liệu nâng cao hoặc tối ưu hóa nhiều. Một mô phỏng trực tiếp đã đủ nhanh. 

Phần khó khăn là xử lý các bất đẳng thức một cách chính xác. Mỗi câu lệnh loại trừ chính hộp được tham chiếu, vì vậy`"To the left of 3"`chỉ cho phép hộp`1`Và`2`, không`3`. 

Một trường hợp tinh tế khác xuất hiện khi nhiều gợi ý siết chặt khoảng thời gian hợp lệ cho đến khi không còn gì. 

Coi như:```
5 2
To the left of 3
To the right of 3
```Gợi ý đầu tiên chỉ cho phép các hộp`1`Và`2`. 

Thứ hai chỉ cho phép các hộp`4`Và`5`. 

Không có hộp nào thỏa mãn cả hai, nên đáp án đúng là:```
-1
```Việc triển khai bất cẩn coi các gợi ý là những bất đẳng thức không nghiêm ngặt có thể khiến bạn suy nghĩ sai`3`vẫn còn hiệu lực. 

Một sai lầm dễ mắc phải khác là bỏ qua những gợi ý trùng lặp.```
4 3
To the left of 3
To the left of 3
To the left of 3
```Câu trả lời vẫn là`2`, vì các ràng buộc lặp đi lặp lại không làm thay đổi phạm vi hợp lệ. 

Gợi ý ranh giới cũng có vấn đề.```
5 1
To the left of 1
```Không có hộp nghiêm ngặt trước hộp`1`, vậy câu trả lời là`-1`. 

Việc triển khai ngây thơ mà quên đi sự nghiêm ngặt có thể trả về không chính xác`1`. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là vũ lực. Đối với mỗi hộp từ`1`ĐẾN`n`, chúng tôi kiểm tra xem nó có thỏa mãn mọi gợi ý hay không. Nếu một hộp vi phạm dù chỉ một điều kiện, chúng tôi sẽ loại bỏ nó. Cuối cùng, chúng tôi đếm xem có bao nhiêu hộp còn sót lại. 

Điều này hoạt động vì các ràng buộc rất nhỏ. Với nhiều nhất`1000`hộp và`1000`gợi ý, trường hợp xấu nhất là:```
1000 × 1000 = 1,000,000 checks
```Điều đó hoàn toàn an toàn trong Python. 

Tuy nhiên, vấn đề có cấu trúc rõ ràng hơn. Mọi gợi ý chỉ giới hạn khoảng thời gian hợp lệ từ một phía. 

Một tuyên bố`"To the left of i"`có nghĩa:```
position < i
```Một tuyên bố`"To the right of i"`có nghĩa:```
position > i
```Thay vì kiểm tra từng ô theo mọi điều kiện, chúng ta có thể duy trì khoảng thời gian hợp lệ hiện tại. 

Chúng tôi giữ hai giá trị:```
L = smallest possible position
R = largest possible position
```Ban đầu:```
L = 1
R = n
```Đối với mỗi`"To the left of i"`gợi ý, chúng tôi cập nhật:```
R = min(R, i - 1)
```Đối với mỗi`"To the right of i"`gợi ý, chúng tôi cập nhật:```
L = max(L, i + 1)
```Sau khi xử lý tất cả các gợi ý: 

Nếu`L > R`, không có hộp hợp lệ tồn tại và chúng tôi in`-1`. 

Ngược lại, mỗi hộp trong`[L, R]`là có thể, vì vậy câu trả lời là:```
R - L + 1
```Cách tiếp cận vũ phu có hiệu quả vì các điều kiện độc lập và dễ dàng xác minh từng điều kiện một. Việc quan sát khoảng thời gian có hiệu quả vì mọi ràng buộc chỉ ảnh hưởng đến một ranh giới, vì vậy tất cả các hộp hợp lệ luôn tạo thành một phân đoạn liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Đã chấp nhận | 
| Tối ưu | O(m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`m`. 
2. Khởi tạo khoảng thời gian hợp lệ dưới dạng toàn bộ bộ hộp.```
L = 1
R = n
```Tại thời điểm này, mọi hộp đều có thể chứa mảnh vụn. 

1. Xử lý từng gợi ý một. 
2. Nếu gợi ý nói`"left of i"`, thì mọi hộp hợp lệ phải hoàn toàn nhỏ hơn`i`. 

Cập nhật:```
R = min(R, i - 1)
```Chúng tôi thu nhỏ ranh giới bên phải vì các hộp`i`và hơn thế nữa là không thể. 

1. Nếu gợi ý nói`"right of i"`, thì mọi hộp hợp lệ phải lớn hơn`i`. 

Cập nhật:```
L = max(L, i + 1)
```Chúng tôi thu nhỏ ranh giới bên trái vì các hộp`i`và trước đó là không thể. 

1. Sau khi tất cả các gợi ý được xử lý, hãy kiểm tra xem khoảng thời gian đó có còn hợp lệ hay không. 

Nếu như:```
L > R
```thì các ràng buộc mâu thuẫn với nhau, vì vậy hãy in`-1`. 

1. Nếu không, mỗi hộp từ`L`ĐẾN`R`là có thể. 

Số hộp như vậy là:```
R - L + 1
```### Tại sao nó hoạt động 

Mỗi gợi ý đều loại bỏ tiền tố hoặc hậu tố của dòng hộp. Sau khi áp dụng một số thao tác như vậy, các vị trí hợp lệ còn lại vẫn phải tạo thành một khoảng liên tục.`L`luôn lưu trữ vị trí nhỏ nhất thỏa mãn mọi gợi ý được xử lý.`R`luôn lưu trữ vị trí lớn nhất thỏa mãn mọi gợi ý được xử lý. 

Bất cứ khi nào có một ràng buộc mới xuất hiện, chúng ta sẽ giao khoảng thời gian hiện tại với khoảng thời gian được gợi ý đó cho phép. Vì giao điểm khoảng duy trì tính chính xác nên kết quả cuối cùng`[L, R]`đoạn chứa chính xác các hộp thỏa mãn tất cả các gợi ý cùng một lúc. 

Nếu khoảng trống thì không có hộp hợp lệ nào tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

left = 1
right = n

for _ in range(m):
    parts = input().split()
    
    direction = parts[2]
    x = int(parts[4])
    
    if direction == "left":
        right = min(right, x - 1)
    else:
        left = max(left, x + 1)

if left > right:
    print(-1)
else:
    print(right - left + 1)
```Chương trình duy trì khoảng thời gian hợp lệ hiện tại trong suốt quá trình xử lý đầu vào.`left`đại diện cho chỉ mục hộp nhỏ nhất vẫn có thể có, trong khi`right`đại diện cho lớn nhất. 

Các dòng đầu vào luôn theo cùng một cấu trúc:```
To the left of i
To the right of i
```Sau khi tách dòng:```
parts = ["To", "the", "left", "of", "i"]
```hướng là ở chỉ số`2`và con số ở chỉ mục`4`. 

Vì`"left"`gợi ý, chỉ số hợp lệ lớn nhất sẽ trở thành`x - 1`. 

Vì`"right"`gợi ý, chỉ số hợp lệ nhỏ nhất sẽ trở thành`x + 1`. 

Sự bất bình đẳng nghiêm ngặt là chi tiết thực hiện quan trọng nhất. sử dụng`x`thay vì`x - 1`hoặc`x + 1`sẽ tạo ra câu trả lời sai trong các trường hợp biên. 

Cuối cùng, nếu khoảng trở nên không hợp lệ, nghĩa là`left > right`, không có hộp khả thi nào và câu trả lời là`-1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1
To the left of 2
```| Bước | Gợi ý | trái | đúng | 
| --- | --- | --- | --- | 
| Ban đầu | không | 1 | 2 | 
| 1 | trái 2 | 1 | 1 | 

Khoảng thời gian cuối cùng là`[1, 1]`, vậy vẫn còn chính xác một hộp. 

Đầu ra:```
1
```Ví dụ này minh họa cách một ràng buộc đơn lẻ cắt bớt một bên của khoảng. 

### Ví dụ 2 

đầu vào:```
5 3
To the left of 5
To the right of 1
To the left of 4
```| Bước | Gợi ý | trái | đúng | 
| --- | --- | --- | --- | 
| Ban đầu | không | 1 | 5 | 
| 1 | trái 5 | 1 | 4 | 
| 2 | bên phải 1 | 2 | 4 | 
| 3 | trái 4 | 2 | 3 | 

Các ô hợp lệ còn lại là`2`Và`3`. 

Đầu ra:```
2
```Dấu vết này cho thấy nhiều gợi ý kết hợp như thế nào thông qua giao điểm khoảng thời gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Mỗi gợi ý được xử lý một lần | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Với nhiều nhất`1000`gợi ý, thuật toán về cơ bản chạy ngay lập tức. Việc sử dụng bộ nhớ là không đổi vì không cần mảng phụ trợ hoặc cấu trúc dữ liệu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())

    left = 1
    right = n

    for _ in range(m):
        parts = input().split()

        direction = parts[2]
        x = int(parts[4])

        if direction == "left":
            right = min(right, x - 1)
        else:
            left = max(left, x + 1)

    if left > right:
        print(-1)
    else:
        print(right - left + 1)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run(
"""2 1
To the left of 2
"""
) == "1", "sample 1"

# minimum input, no hints
assert run(
"""1 0
"""
) == "1", "single box remains valid"

# contradiction from boundaries
assert run(
"""5 2
To the left of 1
To the right of 5
"""
) == "-1", "empty interval"

# duplicate hints
assert run(
"""4 3
To the left of 3
To the left of 3
To the left of 3
"""
) == "2", "duplicates should not matter"

# interval squeezed to one point
assert run(
"""10 2
To the right of 4
To the left of 6
"""
) == "1", "only box 5 remains"

# no restriction after wide hints
assert run(
"""7 2
To the left of 7
To the right of 1
"""
) == "5", "middle segment survives"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`|`1`| Đầu vào kích thước tối thiểu | 
|`left of 1`Và`right of 5`|`-1`| Những hạn chế mâu thuẫn | 
| Gợi ý giống hệt nhau lặp đi lặp lại |`2`| Xử lý trùng lặp | 
|`right of 4`Và`left of 6`|`1`| Vị trí còn lại duy nhất | 
| Hạn chế về ranh giới rộng |`5`| Đếm khoảng thời gian thích hợp | 

## Vỏ cạnh 

Xét trường hợp mâu thuẫn:```
5 2
To the left of 3
To the right of 3
```Xử lý`"left of 3"`mang lại:```
left = 1
right = 2
```Xử lý`"right of 3"`mang lại:```
left = 4
right = 2
```Hiện nay`left > right`, do đó khoảng trống và thuật toán in`-1`. 

Điều này xác nhận rằng các gợi ý loại trừ lẫn nhau được phát hiện chính xác. 

Bây giờ hãy xem xét trường hợp biên:```
5 1
To the left of 1
```Bản cập nhật trở thành:```
right = min(5, 0) = 0
```Khoảng thời gian bây giờ là:```
left = 1
right = 0
```Lại`left > right`, vì vậy không tồn tại hộp hợp lệ. 

Điều này xác minh rằng việc triển khai xử lý chính xác các bất đẳng thức nghiêm ngặt ở gần cuối mảng. 

Cuối cùng, hãy xem xét các ràng buộc trùng lặp:```
4 3
To the left of 3
To the left of 3
To the left of 3
```Sau gợi ý đầu tiên:```
right = 2
```Hai gợi ý tiếp theo không thay đổi gì cả vì:```
min(2, 2) = 2
```Khoảng thời gian cuối cùng là`[1, 2]`, vậy câu trả lời là`2`. 

Điều này xác nhận rằng các gợi ý lặp đi lặp lại không vô tình làm khoảng cách bị thu hẹp quá mức.
