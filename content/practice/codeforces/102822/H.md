---
title: "CF 102822H - Trốn Tìm"
description: "Chúng tôi được cung cấp ba khoảng cách Manhattan. Một người chơi ở tọa độ nguyên không xác định (x1, y1), người còn lại ở (x2, y2) và điểm gốc là điểm tham chiếu thứ ba."
date: "2026-07-26T15:55:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102822
codeforces_index: "H"
codeforces_contest_name: "2020 China Collegiate Programming Contest - Mianyang Site"
rating: 0
weight: 102822
solve_time_s: 51
verified: true
draft: false
---

[CF 102822H - Trốn tìm](https://codeforces.com/problemset/problem/102822/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp ba khoảng cách Manhattan. Một người chơi đang ở tọa độ nguyên không xác định`(x1, y1)`, cái còn lại là tại`(x2, y2)`và gốc tọa độ là điểm tham chiếu thứ ba. Khoảng cách đầu tiên cho chúng ta biết người chơi đầu tiên cách xa bao xa`(0,0)`, cái thứ hai cho chúng ta biết điều tương tự đối với người chơi thứ hai và cái thứ ba cho chúng ta biết khoảng cách giữa hai người chơi. Nhiệm vụ là đếm xem có bao nhiêu cặp vị trí nguyên theo thứ tự thỏa mãn cả ba phép đo. Việc hoán đổi hai người chơi sẽ tạo ra một tình huống khác. 

Đầu vào chứa tối đa`100000`các trường hợp thử nghiệm độc lập và mỗi khoảng cách có thể lớn bằng`10^9`. Việc liệt kê trực tiếp các tọa độ có thể có là không thể vì ngay cả một vòng tròn Manhattan cũng có thể chứa hàng tỷ điểm nguyên. Giải pháp phải xử lý mọi trường hợp thử nghiệm trong thời gian không đổi. 

Các trường hợp bẫy chính xảy ra do tính chẵn lẻ. Sau khi chuyển đổi tọa độ, không phải mọi điểm trong mặt phẳng được chuyển đổi đều tương ứng với một điểm nguyên trong mặt phẳng ban đầu. Một sai lầm phổ biến khác là quên rằng hai người chơi đã được ra lệnh. 

Ví dụ: nếu đầu vào là:```
1
1 1 1
```câu trả lời là`0`. Hai điểm ở khoảng cách Manhattan`1`từ điểm gốc cũng không thể ở khoảng cách Manhattan`1`với nhau theo cách này trong khi vẫn tôn trọng tính chẵn lẻ cần thiết của các tọa độ được chuyển đổi. Một giải pháp đếm tất cả các điểm ranh giới hình vuông được chuyển đổi mà không cần kiểm tra số lần đếm chẵn lẻ. 

Một trường hợp khác là cả hai người chơi đều ở điểm gốc:```
1
0 0 0
```Câu trả lời là`1`, bởi vì cả hai tọa độ phải là`(0,0)`. Một công thức chỉ dựa trên kích thước của các hình vuông có thể vô tình tính các hướng không tồn tại. 

Trường hợp quan trọng thứ ba là khi hai khoảng cách đo được từ điểm gốc khác nhau nhưng người chơi bị đổi chỗ cho nhau. Vì:```
1
2 4 2
```Câu trả lời tính tình huống với người chơi đầu tiên ở lớp khoảng cách-2 và người chơi thứ hai ở lớp khoảng cách-4 tách biệt với cách sắp xếp ngược lại. Việc coi cặp này là không có thứ tự sẽ làm mất đi các câu trả lời hợp lệ. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ tạo ra mọi điểm nguyên với khoảng cách Manhattan`d01`từ điểm gốc, tạo mọi điểm với khoảng cách Manhattan`d02`và kiểm tra từng cặp. Số điểm trên đường tròn bán kính Manhattan`r`tỷ lệ thuận với`r`, vậy trường hợp xấu nhất là về`10^9`điểm trên mỗi lớp. Việc ghép nối chúng mang lại vượt xa giới hạn khả thi. 

Quan sát chính là khoảng cách Manhattan trở nên đơn giản hơn nhiều sau khi xoay hệ tọa độ. Định nghĩa:```
u = x + y
v = x - y
```Đối với hai điểm, khoảng cách Manhattan trở thành:```
max(|u1-u2|, |v1-v2|)
```và khoảng cách từ điểm gốc trở thành:```
max(|u|, |v|)
```Vì vậy, bài toán thay đổi từ các viên kim cương trong mặt phẳng ban đầu thành các hình vuông thẳng hàng theo trục trong`(u,v)`máy bay. 

một điểm`(u,v)`đại diện cho số nguyên`(x,y)`chỉ khi`u`Và`v`có cùng độ ngang bằng. Thay vì liệt kê đường viền hình vuông, chúng tôi đếm các cặp bên trong hình vuông và sử dụng loại trừ bao gồm để khôi phục đường viền. 

Cho phép`G(a,b,r)`là số cặp điểm biến đổi trong đó điểm đầu tiên nằm trong bình phương bán kính`a`, giây nằm trong bình phương bán kính`b`và khoảng cách Chebyshev của chúng lớn nhất là`r`. Khi đó số cặp chính xác trên hai lớp được yêu cầu là:```
G(a,b,r) - G(a-1,b,r) - G(a,b-1,r) + G(a-1,b-1,r)
```Cuối cùng, câu trả lời cho khoảng cách chính xác`c`là số có khoảng cách lớn nhất`c`trừ đi số có khoảng cách nhiều nhất`c-1`. 

Để tính toán`G`, chúng tôi chia các điểm chuyển đổi theo tính chẵn lẻ. Một điểm có thể có`(u,v)`đều chẵn hoặc cả hai đều lẻ. Với mỗi lựa chọn tính chẵn lẻ của điểm thứ nhất và điểm thứ hai, hai tọa độ trở thành bài toán đếm một chiều độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(d01 * d02) | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển bài toán Manhattan thành bài toán Chebyshev bằng cách sử dụng`u=x+y`Và`v=x-y`. 

Phép biến đổi biến các viên kim cương thành các hình vuông, cho phép chúng ta suy luận về các phạm vi được căn chỉnh theo trục. 

1. Thực hiện hàm đếm các cặp điểm biến đổi bên trong hai hình vuông có khoảng cách lớn nhất`r`. 

Một hình vuông bán kính`a`chứa tất cả các điểm trong đó cả hai tọa độ được chuyển đổi đều nằm giữa`-a`Và`a`. Chúng tôi tính các vùng hình vuông này trước tiên vì đường viền khó xử lý trực tiếp hơn. 

1. Chia số đếm theo số chẵn lẻ. 

Đối với mọi khả năng chẵn lẻ của điểm đầu tiên và điểm thứ hai, hãy đếm hợp lệ`u`tọa độ và hợp lệ`v`phối hợp độc lập. Tích của hai số đó cho ra số cặp hai chiều cho sự kết hợp chẵn lẻ đó. 

1. Sử dụng tính năng đếm tiền tố một chiều. 

Đối với một tọa độ, sau khi chia cho hai, chúng ta cần đếm các cặp chỉ số có hiệu nằm trong một khoảng. Điều này có thể được thực hiện bằng cách tính toán số lượng tiền tố của các cặp có chênh lệch tối đa một giá trị. 

1. Chuyển đổi số lượng bình phương thành số lượng lớp bằng cách sử dụng loại trừ. 

Trừ các hình vuông bên trong khỏi các hình vuông bên ngoài để chỉ còn lại các điểm ở khoảng cách chính xác theo yêu cầu của Manhattan. 

1. Tính đáp án khoảng cách chính xác. 

Khoảng cách cần thiết là chính xác`d12`, vậy hãy tính số có khoảng cách lớn nhất`d12`và trừ số có khoảng cách nhiều nhất`d12-1`. 

Tại sao nó hoạt động: 

Phép biến đổi tọa độ bảo toàn khoảng cách cần thiết cho bài toán. Mỗi điểm ban đầu hợp lệ sẽ ánh xạ tới chính xác một điểm được chuyển đổi có tọa độ chẵn lẻ phù hợp và mọi điểm được chuyển đổi có tọa độ chẵn lẻ bằng nhau sẽ ánh xạ trở lại chính xác một điểm nguyên. Loại trừ bao gồm hình vuông sẽ loại bỏ mọi điểm không nằm trên lớp khoảng cách được yêu cầu đúng một lần. Phép trừ cuối cùng sẽ loại bỏ tất cả các cặp có khoảng cách nhỏ hơn khoảng cách thứ ba được yêu cầu, chỉ để lại chính xác các cặp có khoảng cách mong muốn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ceil_div(a, b):
    return -((-a) // b)

def sum_range(l, r):
    if l > r:
        return 0
    return (l + r) * (r - l + 1) // 2

def prefix_diff(n1, n2, s):
    # number of pairs (i, j), 0 <= i < n1, 0 <= j < n2, i-j <= s
    ans = 0

    l = 0
    r = min(n1 - 1, s)
    if l <= r:
        ans += (r - l + 1) * n2

    l = max(0, s + 1)
    r = min(n1 - 1, s + n2 - 1)
    if l <= r:
        ans += (r - l + 1) * (n2 + s) - sum_range(l, r)

    return ans

def count_difference(l1, r1, l2, r2, lo, hi):
    if l1 > r1 or l2 > r2:
        return 0
    n1 = r1 - l1 + 1
    n2 = r2 - l2 + 1
    shift = l1 - l2
    return prefix_diff(n1, n2, hi - shift) - prefix_diff(n1, n2, lo - 1 - shift)

def coordinate_count(a, b, r, p, q):
    if a < 0 or b < 0:
        return 0

    l1 = ceil_div(-a - p, 2)
    r1 = (a - p) // 2
    l2 = ceil_div(-b - q, 2)
    r2 = (b - q) // 2

    if l1 > r1 or l2 > r2:
        return 0

    diff = p - q
    lo = ceil_div(-r - diff, 2)
    hi = (r - diff) // 2

    return count_difference(l1, r1, l2, r2, lo, hi)

def square_count(a, b, r):
    if a < 0 or b < 0:
        return 0

    ans = 0
    for p in (0, 1):
        for q in (0, 1):
            c = coordinate_count(a, b, r, p, q)
            ans += c * c
    return ans

def inside_layers(a, b, r):
    return (
        square_count(a, b, r)
        - square_count(a - 1, b, r)
        - square_count(a, b - 1, r)
        + square_count(a - 1, b - 1, r)
    )

def solve_case(a, b, c):
    if c < 0:
        return 0
    return inside_layers(a, b, c) - inside_layers(a, b, c - 1)

t = int(input())
out = []
for case in range(1, t + 1):
    a, b, c = map(int, input().split())
    out.append(f"Case #{case}: {solve_case(a, b, c)}")

print("\n".join(out))
```các`ceil_div`cần có trình trợ giúp vì phép chia số nguyên của Python làm tròn xuống, trong khi phép chia trần toán học là cần thiết khi chuyển đổi phạm vi tọa độ. 

các`prefix_diff`chức năng là thói quen đếm một chiều cốt lõi. Nó chia các chỉ mục đầu tiên có thể có thành ba vùng: các chỉ mục trong đó mỗi tọa độ thứ hai hoạt động, các chỉ mục trong đó chỉ một phần của khoảng hoạt động và các chỉ mục không có tọa độ thứ hai hoạt động.`coordinate_count`chuyển đổi một phạm vi tọa độ được chuyển đổi thành một phạm vi chỉ mục bằng cách sửa giá trị chẵn lẻ. Điều kiện khoảng cách trở thành một phạm vi dựa trên sự khác biệt của hai chỉ số.`square_count`kết hợp bốn kết hợp chẵn lẻ. Phép nhân tự nó có giá trị vì`u`Và`v`kích thước là độc lập sau khi chuyển đổi.`inside_layers`áp dụng loại trừ bao gồm để loại bỏ các lớp hình vuông bên trong. Cuối cùng,`solve_case`thay đổi số lượng "khoảng cách tối đa" thành số lượng khoảng cách chính xác. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
2 4 2
```Các giá trị quan trọng trong quá trình tính toán là: 

| Bước | Bán kính đầu tiên | Bán kính thứ hai | Giới hạn khoảng cách | Kết quả | 
| --- | --- | --- | --- | --- | 
| Đếm nhiều nhất là 2 | 2 | 4 | 2 | 32 | 
| Đếm nhiều nhất là 1 | 2 | 4 | 1 | 0 | 
| Câu trả lời cuối cùng | | | | 32 | 

Ví dụ chứng minh rằng việc đếm khoảng cách chính xác thu được bằng cách trừ đi vùng khoảng cách nhỏ hơn. Mọi cặp hợp lệ sẽ xuất hiện trong lần đếm đầu tiên và chỉ biến mất khi khoảng cách của nó thấp hơn mục tiêu. 

Đối với mẫu thứ tư:```
1 1 1
```| Bước | Bán kính đầu tiên | Bán kính thứ hai | Giới hạn khoảng cách | Kết quả | 
| --- | --- | --- | --- | --- | 
| Đếm nhiều nhất là 1 | 1 | 1 | 1 | 0 | 
| Đếm nhiều nhất là 0 | 1 | 1 | 0 | 0 | 
| Câu trả lời cuối cùng | | | | 0 | 

Trường hợp này xác nhận rằng các hạn chế chẵn lẻ sẽ loại bỏ các điểm được chuyển đổi không tương ứng với tọa độ nguyên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Mỗi trường hợp thử nghiệm thực hiện một số phép tính số học và kiểm tra tính chẵn lẻ cố định | 
| Không gian | O(1) | Chỉ các biến vô hướng được lưu trữ | 

Kích thước đầu vào lớn vì có thể có`100000`các trường hợp thử nghiệm, do đó giải pháp tránh được các vòng lặp tùy thuộc vào khoảng cách. Tất cả các hoạt động là thời gian không đổi và phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```
# These tests are intended to be run with the submitted solution.

def brute(a, b, c):
    ans = 0
    for x1 in range(-10, 11):
        for y1 in range(-10, 11):
            if abs(x1) + abs(y1) != a:
                continue
            for x2 in range(-10, 11):
                for y2 in range(-10, 11):
                    if abs(x2) + abs(y2) == b and abs(x1-x2)+abs(y1-y2) == c:
                        ans += 1
    return ans

assert solve_case(0, 0, 0) == 1
assert solve_case(1, 1, 1) == 0
assert solve_case(2, 4, 2) == 32
assert solve_case(1, 3, 2) == 20
assert solve_case(3, 4, 5) == 48
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 0`|`1`| Cả hai người chơi ở gốc | 
|`1 1 1`|`0`| Hạn chế chẵn lẻ | 
|`2 4 2`|`32`| Bán kính và vị trí theo thứ tự khác nhau | 
|`1 3 2`|`20`| Các lớp nhỏ không tầm thường | 
|`3 4 5`|`48`| Tương tác hình vuông biến đổi lớn hơn | 

## Vỏ cạnh 

Dành cho:```
1
0 0 0
```cả hai hình vuông biến đổi đều có bán kính bằng không. Loại trừ bao gồm chỉ để lại điểm chuyển đổi duy nhất`(0,0)`, tương ứng với điểm ban đầu`(0,0)`. 

Vì:```
1
1 1 1
```hình vuông được biến đổi có một số điểm, nhưng chỉ có những điểm ở đó`u`Và`v`chia sẻ chẵn lẻ là hợp lệ. Tính chẵn lẻ được chia thành`square_count`loại bỏ các điểm chuyển đổi không hợp lệ trước khi đếm. 

Vì:```
1
2 4 2
```hai người chơi thuộc các tầng Manhattan khác nhau. Việc loại trừ bao gồm chỉ giữ lại đường viền của mỗi hình vuông và phép trừ khoảng cách cuối cùng chỉ giữ lại các cặp có khoảng cách Chebyshev chính xác là hai. 

Đối với những trường hợp có khoảng cách cực lớn như:```
1
1000000000 1000000000 1000000000
```thuật toán không bao giờ lặp qua phạm vi tọa độ. Tất cả các phép tính được thực hiện bằng số học số nguyên, do đó giới hạn lớn chỉ ảnh hưởng đến kích thước của các giá trị trung gian. Số nguyên Python xử lý các giá trị này một cách an toàn.
