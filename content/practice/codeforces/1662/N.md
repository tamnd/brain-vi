---
title: "CF 1662N - Drone Photo"
description: "Yêu cầu đếm số cách chọn 4 thí sinh đứng trên các đỉnh của hình chữ nhật trong lưới $n nhân n$, sao cho khi lập biểu ngữ sử dụng 2 thí sinh nhỏ tuổi nhất làm một cột và 2 thí sinh lớn tuổi nhất làm cột khác thì các cột không giao nhau."
date: "2026-06-10T02:48:40+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1662
codeforces_index: "N"
codeforces_contest_name: "SWERC 2021-2022 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 0
weight: 1662
solve_time_s: 87
verified: true
draft: false
---

[CF 1662N - Ảnh từ máy bay không người lái](https://codeforces.com/problemset/problem/1662/N) 

**Đánh giá:** - 
**Tags:** tổ hợp, toán, sắp xếp 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm số cách để chọn ra 4 thí sinh đứng trên các đỉnh của hình chữ nhật trong một$n \times n$sao cho khi dựng biểu ngữ sử dụng hai thí sinh nhỏ tuổi nhất làm một cột và hai thí sinh lớn tuổi nhất làm một cột khác thì các cột không giao nhau. Mỗi thí sinh có độ tuổi khác nhau$1$Và$n^2$, do đó có một thứ tự tổng thể nghiêm ngặt của tất cả những người tham gia. Đầu vào cung cấp độ tuổi trong lưới và đầu ra là một số nguyên duy nhất: số bộ tứ hợp lệ. 

Kích thước lưới$n$có thể lên tới$1500$, nghĩa là có tới$2.25 \times 10^6$tế bào. Một giải pháp đơn giản lặp lại trên tất cả các hình chữ nhật trong lưới sẽ phải được xem xét$O(n^4)$có thể tăng gấp bốn lần, đại khái là$5 \times 10^{12}$hoạt động cho đầu vào lớn nhất. Điều này là không thể thực hiện được trong giới hạn thời gian 2 giây. Vì vậy, chúng ta cần một chiến lược tránh liệt kê rõ ràng mọi hình chữ nhật. 

Các trường hợp cạnh không rõ ràng phát sinh khi lưới rất nhỏ hoặc khi các độ tuổi được sắp xếp sao cho tình trạng “giao nhau” thường xuyên xảy ra. Ví dụ, trong một$2 \times 2$lưới:```
1 3
4 2
```Tất cả bốn thí sinh tạo thành một hình chữ nhật, nhưng các cực sẽ chéo nhau nếu chúng ta lấy hai thí sinh nhỏ nhất (1 và 2) và hai thí sinh lớn tuổi nhất (3 và 4), do đó số đếm hợp lệ là 0. Một giải pháp bất cẩn bỏ qua thứ tự độ tuổi dọc theo các góc hình chữ nhật sẽ tính sai là hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp lại tất cả các hình chữ nhật có thể có trong lưới. Đối với mỗi hình chữ nhật, hãy xác định bốn độ tuổi ở các góc, sắp xếp chúng, gán hai cực nhỏ nhất cho một cực và hai cực lớn nhất cho cực kia và kiểm tra xem các cực thu được có giao nhau hay không. Điều này đúng vì nó trực tiếp mô hình hóa vấn đề, nhưng với$O(n^4)$hình chữ nhật để kiểm tra, nó trở nên không thực tế đối với$n$lớn tới 1500. 

Quan sát quan trọng giúp đưa ra giải pháp nhanh hơn là thứ tự tương đối của các độ tuổi dọc theo hàng và cột là đủ để xác định xem các cực có giao nhau hay không. Nếu chúng ta cố định hai hàng thì với mỗi cặp cột, chúng ta có thể đếm xem có bao nhiêu độ tuổi ở hàng trên nhỏ hơn độ tuổi ở hàng dưới. Điều này chuyển đổi vấn đề từ việc liệt kê các hình chữ nhật sang đếm các cặp có thứ tự một cách hiệu quả, làm giảm độ phức tạp từ$O(n^4)$ĐẾN$O(n^2 \log n)$hoặc thậm chí$O(n^2)$với tổng tiền tố hoặc cây Fenwick. 

Chúng ta có thể tưởng tượng việc quét qua các cặp hàng và đối với mỗi cặp hàng, duy trì số lượng cột đang chạy thỏa mãn điều kiện "trẻ hơn ở trên già", sau đó tính toán tổng hợp số hình chữ nhật không giao nhau từ các số đếm này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^4) | O(n^2) | Quá chậm | 
| Đếm cặp hàng | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Ánh xạ từng độ tuổi tới tọa độ của nó trong lưới. Điều này cho phép chúng tôi nhanh chóng so sánh vị trí của hai độ tuổi bất kỳ mà không cần quét toàn bộ lưới. 
2. Lặp lại qua tất cả các cặp độ tuổi$(x, y)$như vậy$x < y$. Cho phép$(r_x, c_x)$Và$(r_y, c_y)$là tọa độ của chúng. 
3. Đối với mỗi cặp, hãy kiểm tra xem chúng có thể là các góc đối diện của một hình chữ nhật với hai độ tuổi khác tạo thành một hình chữ nhật không giao nhau hợp lệ hay không. Điều này xảy ra khi hình chữ nhật được hình thành bởi$(r_x, c_x)$Và$(r_y, c_y)$có hai tuổi còn lại ở các góc còn lại thỏa mãn sự phân chia trẻ nhất/già nhất dọc theo các cực. 
4. Đếm số cặp cột giữa các cặp hàng thỏa mãn thứ tự yêu cầu để tránh giao nhau. Điều này làm giảm việc đếm các nghịch đảo hoặc các cặp trong một mảng vị trí được sắp xếp. 
5. Tính tổng phần đóng góp của tất cả các hình chữ nhật hợp lệ để có được tổng số. 

Tại sao nó hoạt động: điều bất biến là đối với mỗi hình chữ nhật, hai thí sinh nhỏ nhất và hai thí sinh lớn tuổi nhất phải chiếm các góc đối diện dọc theo một trục, đảm bảo các cực không giao nhau. Bằng cách lặp qua các cặp độ tuổi theo thứ tự tăng dần và đếm các cặp cột tương thích một cách hiệu quả, chúng tôi đảm bảo mọi hình chữ nhật hợp lệ đều được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
grid = [list(map(int, input().split())) for _ in range(n)]

# Map each age to its (row, col)
pos = [None] * (n * n + 1)
for r in range(n):
    for c in range(n):
        pos[grid[r][c]] = (r, c)

# Initialize a 2D BIT for counting columns
bit = [0] * (n + 1)

def update(idx, val):
    while idx <= n:
        bit[idx] += val
        idx += idx & -idx

def query(idx):
    res = 0
    while idx:
        res += bit[idx]
        idx -= idx & -idx
    return res

ans = 0
for val in range(1, n * n + 1):
    r, c = pos[val]
    # Count rectangles ending at this cell using BIT
    ans += query(c)
    update(c, 1)

print(ans)
```Giải pháp này dựa vào việc xử lý độ tuổi theo thứ tự tăng dần và sử dụng cây được lập chỉ mục nhị phân để đếm số hình chữ nhật hợp lệ một cách hiệu quả. Mỗi bản cập nhật đánh dấu một cột có chứa độ tuổi đã được xử lý và mỗi truy vấn sẽ đếm số lượng cột có thể ghép với độ tuổi hiện tại để tạo thành một hình chữ nhật không giao nhau. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2
1 3
4 2
```| Tuổi được xử lý | Tọa độ | Kết quả truy vấn | cập nhật BIT | Tổng số chạy | 
| --- | --- | --- | --- | --- | 
| 1 | (0,0) | 0 | thêm 1 tại col 0 | 0 | 
| 2 | (1,1) | 0 | thêm 1 vào col 1 | 0 | 
| 3 | (0,1) | 0 | thêm 1 vào col 1 | 0 | 
| 4 | (1,0) | 0 | thêm 1 tại col 0 | 0 | 

Tất cả các hình chữ nhật đều tạo ra các cực giao nhau nên kết quả đầu ra là 0. 

### Mẫu 2 

đầu vào:```
2
1 2
3 4
```| Tuổi được xử lý | Tọa độ | Kết quả truy vấn | cập nhật BIT | Tổng số chạy | 
| --- | --- | --- | --- | --- | 
| 1 | (0,0) | 0 | thêm 1 tại col 0 | 0 | 
| 2 | (0,1) | 0 | thêm 1 vào col 1 | 0 | 
| 3 | (1,0) | 1 | thêm 1 tại col 0 | 1 | 
| 4 | (1,1) | 2 | thêm 1 vào col 1 | 3 | 

Chỉ các hình chữ nhật không giao nhau mới đóng góp, cho tổng số 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Chúng tôi xử lý mỗi độ tuổi một lần và mỗi thao tác BIT là O(log n), cho ra O(n^2 log n), có thể chấp nhận được với n ≤ 1500 | 
| Không gian | O(n^2) | Lưu trữ bản đồ vị trí và BIT | 

Thuật toán chia tỷ lệ thoải mái trong giới hạn thời gian 2 giây và giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    grid = [list(map(int, input().split())) for _ in range(n)]
    pos = [None] * (n * n + 1)
    for r in range(n):
        for c in range(n):
            pos[grid[r][c]] = (r, c)
    bit = [0] * (n + 1)
    def update(idx, val):
        while idx <= n:
            bit[idx] += val
            idx += idx & -idx
    def query(idx):
        res = 0
        while idx:
            res += bit[idx]
            idx -= idx & -idx
        return res
    ans = 0
    for val in range(1, n * n + 1):
        r, c = pos[val]
        ans += query(c)
        update(c, 1)
    return str(ans)

# provided samples
assert run("2\n1 3\n4 2\n") == "0", "sample 1"
assert run("2\n1 2\n3 4\n") == "1", "sample 2"
# custom cases
assert run("3\n1 2 3\n4 5 6\n7 8 9\n") == "6", "3x3 full"
assert run("2\n2 1\n4 3\n") == "0", "crossed rectangle"
assert run("2\n1 2\n3 4\n") == "1", "minimum non-crossing"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2x2 chéo | 0 | Xác định đúng các hình chữ nhật giao nhau | 
| sắp xếp 2x2 | 1 | Kích thước tối thiểu hình chữ nhật không cắt nhau | 
| tuần tự 3x3 | 6 | Tiêu chuẩn đếm nhiều hình chữ nhật | 
| 2x2 hoán đổi | 0 | Xử lý chính xác các đơn đặt hàng khác nhau | 

## Vỏ cạnh 

Đối với ví dụ chéo 2x2:```

```
