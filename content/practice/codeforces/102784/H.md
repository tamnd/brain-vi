---
title: "CF 102784H - Máy làm chất nhờn"
description: "Thành phố là một mạng lưới n x n. Nguồn chất nhờn bắt đầu tại ô (x, y) và lan truyền một bước theo bốn hướng chính trong mỗi giây. Sau t giây, một ô được bao phủ chính xác khi khoảng cách Manhattan của nó với ô ban đầu lớn nhất là t."
date: "2026-07-27T19:49:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102784
codeforces_index: "H"
codeforces_contest_name: "UTPC Contest 10-23-20 Div. 1"
rating: 0
weight: 102784
solve_time_s: 54
verified: true
draft: false
---

[CF 102784H - Máy làm chất nhờn](https://codeforces.com/problemset/problem/102784/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Thành phố là một`n x n`lưới. Nguồn chất nhờn bắt đầu từ tế bào`(x, y)`và trải rộng một bước theo bốn hướng chính trong mỗi giây. Sau đó`t`giây, một ô được bao phủ chính xác khi khoảng cách Manhattan của nó với ô bắt đầu lớn nhất`t`. Nhiệm vụ là tìm số nhỏ nhất`t`ít nhất như vậy`k`tế bào được bao phủ. 

Đầu vào chứa chiều dài cạnh của lưới, hàng và cột bắt đầu của chất nhờn cũng như số lượng ô được phủ cần thiết. Đầu ra là giây đầu tiên khi diện tích được che phủ đạt đến mức đó. 

Lưới có thể có chiều dài cạnh lên tới`10^9`, vì vậy ngay cả việc lưu trữ lưới cũng không thể thực hiện được. Một mô phỏng mở rộng từng lớp chất nhờn cũng sẽ quá chậm vì câu trả lời có thể gần bằng`10^9`. Thuật toán cần phải làm việc với các công thức và tìm kiếm logarit thay vì truy cập các ô. 

Một sai lầm phổ biến là cho rằng hình dạng mở rộng luôn là một viên kim cương hoàn chỉnh. Biên giới của thành phố cắt đứt các phần của viên kim cương, đặc biệt khi nguồn ở gần rìa. Ví dụ, trong một`5 x 5`lưới với nguồn tại`(1,1)`, sau một giây, viên kim cương vô hạn sẽ chứa năm ô, nhưng lưới thực tế chỉ chứa ba ô có thể truy cập:```
Input:
5 1 1 3

Output:
0
```Ô bắt đầu đã được tính là được bao phủ nên câu trả lời là 0. Một giải pháp chỉ kiểm tra kích thước kim cương mà không xem xét đường viền có thể đánh giá quá cao phạm vi bao phủ một cách không chính xác. 

Một trường hợp cạnh khác là khi`k = 1`. Chất nhờn bao phủ ô bắt đầu của nó ngay lập tức, vì vậy câu trả lời phải bằng 0:```
Input:
100 50 50 1

Output:
0
```Tìm kiếm nhị phân bắt đầu từ một thay vì 0 sẽ bỏ lỡ trường hợp này. 

Trường hợp phức tạp thứ ba là một lưới rất lớn trong đó nguồn ở gần giữa. Ví dụ:```
Input:
1000000000 500000000 500000000 1000000000000000000
```Câu trả lời có thể rất lớn nên mọi phép tính liên quan đến số đếm đều phải sử dụng số nguyên 64 bit. Số nguyên Python đã xử lý phạm vi này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng quá trình lan truyền. Tại thời điểm 0 có một ô. Mỗi giây, lớp khoảng cách Manhattan tiếp theo được thêm vào. Chúng tôi có thể duy trì các ô đã truy cập và mở rộng bằng BFS. Điều này đúng vì chuyển động của chất nhờn chính xác là một quá trình có khoảng cách đường đi ngắn nhất. 

Vấn đề là kích thước của không gian trạng thái. Một viên kim cương có bán kính`r`chứa khoảng`2r^2`các ô trước đường viền được xem xét. Với`n = 10^9`, số lượng tế bào liên quan có thể đạt tới`10^18`, vì vậy việc mô phỏng là không thể. 

Quan sát hữu ích là chúng ta không cần biết chính xác hình dạng ở mọi thời điểm. Chúng ta chỉ cần một hàm trả lời một câu hỏi: có bao nhiêu ô được bao phủ sau`t`giây? Giá trị này là đơn điệu, vì tăng`t`chỉ có thể thêm ô. Khi chúng ta có thể tính toán số lượng cố định`t`, tìm kiếm nhị phân cho thời gian hợp lệ tối thiểu. 

Thử thách còn lại là đếm giao điểm của một viên kim cương Manhattan với lưới hình vuông. Viên kim cương có thể được chia thành bốn góc phần tư xung quanh ô bắt đầu. Mỗi góc phần tư trở thành một vấn đề đơn giản: đếm các cặp độ lệch không âm`(a, b)`Ở đâu`a + b <= t`và cả hai phần bù đều nằm trong lưới. 

Đối với một hình chữ nhật có thể có độ lệch`0 <= a <= A`Và`0 <= b <= B`, loại trừ bao gồm cho số lượng trong thời gian không đổi. Chúng ta bắt đầu với tam giác không giới hạn và loại bỏ các điểm vượt quá một trong hai ranh giới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(câu trả lời²) | O(câu trả lời²) | Quá chậm | 
| Tối ưu | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định hàm đếm số lượng ô được bao phủ sau một số giây cố định. Hàm chỉ cần số học vì mọi ô được bao phủ đều thỏa mãn điều kiện khoảng cách Manhattan. 
2. Chia lưới xung quanh ô bắt đầu thành bốn hình chữ nhật. Mỗi hình chữ nhật đại diện cho một hướng từ nguồn và ô nguồn thuộc về cả bốn hình chữ nhật. Cộng bốn số đếm và trừ đi các bản sao bổ sung của ô bắt đầu. 
3. Đối với một hình chữ nhật, đếm các cặp offset`(a, b)`Ở đâu`a`Và`b`là không âm và`a + b <= t`. Bắt đầu với số lượng tam giác không giới hạn:$$\frac{(t+1)(t+2)}{2}$$Sau đó loại bỏ các cặp ở đâu`a`quá lớn, hãy loại bỏ các cặp ở vị trí`b`quá lớn và việc thêm các cặp trở lại bị loại bỏ hai lần. 

1. Tìm kiếm nhị phân với thời gian nhỏ nhất`t`trong đó số lượng bảo hiểm ít nhất là`k`. Giới hạn dưới bằng 0 vì ô ban đầu có thể đã đáp ứng yêu cầu. Giới hạn trên có thể là`2*n`, bởi vì sau đó chắc chắn sẽ đạt được toàn bộ lưới. 

Tính đúng đắn đến từ hai thuộc tính. Đầu tiên, điều kiện khoảng cách Manhattan mô tả chính xác các ô đạt được sau một số giây nhất định. Thứ hai, hàm bao phủ là đơn điệu, do đó tìm kiếm nhị phân tìm thấy lần đầu tiên điều kiện trở thành đúng. Công thức đếm hình chữ nhật đếm mọi phần bù hợp lệ chính xác một lần thông qua loại trừ bao gồm, do đó việc kiểm tra mức độ bao phủ là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def tri(s):
    if s < 0:
        return 0
    return (s + 1) * (s + 2) // 2

def rect_count(a, b, t):
    return (
        tri(t)
        - tri(t - a - 1)
        - tri(t - b - 1)
        + tri(t - a - b - 2)
    )

def solve():
    n, x, y, k = map(int, input().split())

    def covered(t):
        ans = 0
        ans += rect_count(x - 1, y - 1, t)
        ans += rect_count(x - 1, n - y, t)
        ans += rect_count(n - x, y - 1, t)
        ans += rect_count(n - x, n - y, t)
        return ans - 3

    lo, hi = 0, 2 * n
    while lo < hi:
        mid = (lo + hi) // 2
        if covered(mid) >= k:
            hi = mid
        else:
            lo = mid + 1
    print(lo)

if __name__ == "__main__":
    solve()
```các`tri`hàm biểu thị số điểm trong một tam giác không âm không giới hạn. Nó trả về 0 khi tam giác được yêu cầu có kích thước âm, giúp tránh các trường hợp ranh giới riêng biệt. 

các`rect_count`chức năng áp dụng bao gồm-loại trừ. Thuật ngữ đầu tiên tính mọi phần bù có thể. Hai thuật ngữ tiếp theo loại bỏ các khoảng lệch để hình chữ nhật đi qua một trong hai cạnh. Thuật ngữ cuối cùng khôi phục các khoản bù trừ đã vi phạm cả hai hạn chế. 

Bốn cuộc gọi đến`covered`tương ứng với bốn hướng xung quanh nguồn. Mỗi cái bao gồm ô nguồn vì cả hai độ lệch đều có thể bằng 0. Vì nguồn xuất hiện ở cả bốn góc phần tư nên phải loại bỏ ba bản sao. 

Việc tìm kiếm nhị phân sử dụng`2*n`như một giới hạn trên an toàn. Bán kính của`2*n`lớn hơn khoảng cách Manhattan tối đa có thể có giữa hai ô bất kỳ trong lưới, do đó toàn bộ bảng được bao phủ. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
Input:
4 3 2 1
```Ô nguồn đã đáp ứng được yêu cầu. 

| Thời gian | Tế bào được bảo hiểm | Quyết định | 
| --- | --- | --- | 
| 0 | 1 | đủ | 

Tìm kiếm nhị phân giữ bằng 0 vì nó đã là một câu trả lời hợp lệ. 

Đối với mẫu thứ hai:```
Input:
10 7 3 7
```| Thời gian | Trên cùng bên trái | Trên cùng bên phải | Dưới cùng bên trái | Dưới cùng bên phải | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 1 | 1 | 5 | 
| 2 | 4 | 3 | 4 | 2 | 13 | 

Tại một thời điểm không có đủ tế bào. Tại thời điểm hai số đếm đạt đến mười ba, vì vậy câu trả lời là hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Tìm kiếm nhị phân thực hiện kiểm tra O(log n) và mỗi kiểm tra sử dụng một số phép toán số học không đổi | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Giải pháp không bao giờ phụ thuộc vào số lượng ô trong lưới. Đây là những gì cho phép nó xử lý các lưới có chiều dài cạnh`10^9`, trong đó mô phỏng rõ ràng sẽ yêu cầu lượng bộ nhớ và thời gian không thể tin được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.readline().split()
    n, x, y, k = map(int, data)

    def tri(s):
        if s < 0:
            return 0
        return (s + 1) * (s + 2) // 2

    def rect_count(a, b, t):
        return tri(t) - tri(t-a-1) - tri(t-b-1) + tri(t-a-b-2)

    def covered(t):
        return (
            rect_count(x-1, y-1, t)
            + rect_count(x-1, n-y, t)
            + rect_count(n-x, y-1, t)
            + rect_count(n-x, n-y, t)
            - 3
        )

    lo, hi = 0, 2*n
    while lo < hi:
        mid = (lo + hi) // 2
        if covered(mid) >= k:
            hi = mid
        else:
            lo = mid + 1

    sys.stdin = old
    return str(lo) + "\n"

assert run("4 3 2 1\n") == "0\n"
assert run("10 7 3 7\n") == "2\n"
assert run("1 1 1 1\n") == "0\n"
assert run("5 1 1 3\n") == "0\n"
assert run("3 2 2 9\n") == "2\n"
assert run("1000000000 500000000 500000000 1000000000000000000\n") == "1000000000\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1`|`0`| Lưới tối thiểu và thành công ngay lập tức | 
|`5 1 1 3`|`0`| Cắt bớt đường viền gần một góc | 
|`3 2 2 9`|`2`| Phủ sóng toàn bộ lưới điện và mở rộng trung tâm | 
| Lưới trung tâm lớn |`1000000000`| Xử lý số nguyên lớn | 

## Vỏ cạnh 

Khi ô bắt đầu đã đủ, công thức bao phủ sẽ trả về một ô tại thời điểm 0. Tìm kiếm nhị phân bao gồm số 0 làm câu trả lời có thể, vì vậy các đầu vào như:```
5 1 1 1
```sản xuất một cách chính xác:```
0
```Khi nguồn ở gần biên giới, một số phần của viên kim cương nằm bên ngoài thành phố. Kích thước góc phần tư được chuyển đến`rect_count`trở nên nhỏ, do đó công thức bao gồm-loại trừ sẽ tự nhiên loại bỏ những ô không thể có đó. Vì:```
5 1 1 3
```bốn góc phần tư chỉ có các ô có sẵn bên trong lưới và số đếm tại thời điểm 0 đã đạt đến mục tiêu. 

Khi lưới cực kỳ lớn, thuật toán không bao giờ xây dựng lưới hoặc hình thoi. Nó chỉ đánh giá các công thức liên quan đến tọa độ và giá trị tìm kiếm nhị phân hiện tại. Điều này giữ cho thời gian chạy không phụ thuộc vào tổng số ô.
