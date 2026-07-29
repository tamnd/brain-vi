---
title: "CF 102801E - Vectơ lót"
description: "Bài toán hỏi liệu có thể thu được vectơ B từ vectơ A ban đầu hay không bằng cách áp dụng liên tục hai chuyển động được phép: xoay vectơ hiện tại 90 độ theo chiều kim đồng hồ hoặc thêm vectơ C cố định vào vectơ hiện tại."
date: "2026-07-28T22:56:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102801
codeforces_index: "E"
codeforces_contest_name: "The 14th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 102801
solve_time_s: 54
verified: true
draft: false
---

[CF 102801E - Vectơ lót](https://codeforces.com/problemset/problem/102801/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán hỏi liệu một vectơ có`B`có thể thu được từ một vectơ ban đầu`A`bằng cách áp dụng liên tục hai động tác được phép: xoay vectơ hiện tại 90 độ theo chiều kim đồng hồ hoặc thêm một vectơ cố định`C`tới vectơ hiện tại. Các phép quay và phép cộng có thể được trộn theo bất kỳ thứ tự nào và được sử dụng với số lần bất kỳ. Nhiệm vụ là quyết định xem chuỗi hoạt động như vậy có tồn tại hay không. 

Tọa độ của tất cả các vectơ có thể lớn bằng`10^8`về giá trị tuyệt đối. Điều này loại trừ mọi mô phỏng dựa trên số lượng thao tác, vì số lượng phép cộng cần thiết có thể cực kỳ lớn và không có giới hạn trên về độ dài chuỗi. Lời giải phải sử dụng cấu trúc đại số của các phép tính và kết thúc trong thời gian không đổi. 

Khó khăn tiềm ẩn chính là thao tác xoay sẽ thay đổi hướng bổ sung trong tương lai. Một cách tiếp cận đơn giản chỉ kiểm tra xem`B - A`là bội số của`C`bỏ lỡ các trường hợp phép quay cho phép các hướng chuyển động mới. 

Trường hợp cạnh đầu tiên là khi`C`là vectơ số không. Ví dụ:```
Input
0 0
1 1
0 0
```Câu trả lời là:```
NO
```Cho dù có thực hiện bao nhiêu phép quay thì vectơ 0 vẫn bằng 0, do đó, các vectơ duy nhất có thể tiếp cận là bốn phép quay của`A`. Phương pháp chia theo chiều dài`C`sẽ thất bại ở đây vì không có ước số hợp lệ. 

Một trường hợp khác là mục tiêu có thể yêu cầu quay vectơ bắt đầu trước khi thêm bất kỳ thứ gì. Ví dụ:```
Input
1 0
0 -1
0 0
```Câu trả lời là:```
YES
```Xoay`(1, 0)`theo chiều kim đồng hồ cho`(0, -1)`. Một phương pháp chỉ kiểm tra các phần bổ sung từ vị trí ban đầu sẽ từ chối điều này một cách không chính xác. 

Trường hợp cạnh thứ ba là khi chuyển động được yêu cầu chỉ có thể thực hiện được vì các bản sao được xoay của`C`hủy bỏ hoặc kết hợp. Ví dụ:```
Input
0 0
1 0
0 1
```Câu trả lời là:```
YES
```Vectơ`(1,0)`có thể thu được bằng cách thêm`(0,1)`sau ba lần quay theo chiều kim đồng hồ của hiệu ứng hệ tọa độ, bởi vì các phép cộng được xoay sẽ tạo ra tất cả các hướng vuông góc. Chỉ kiểm tra bội số của bản gốc`C`sẽ thất bại. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ thử mọi chuỗi phép quay và phép cộng có thể có. Vấn đề với ý tưởng này là không có điểm dừng có ý nghĩa. Một chuỗi có thể chứa một số lượng bổ sung tùy ý và việc tìm kiếm tất cả các thứ tự hoạt động có thể tăng lên theo cấp số nhân. Ngay cả việc hạn chế tìm kiếm ở một số thao tác cố định cũng không thể biện minh được. 

Quan sát hữu ích là các phép quay có tính tuần hoàn. Bốn phép quay theo chiều kim đồng hồ sẽ trả lại một vectơ về hướng ban đầu của nó. Sau khi tất cả các thao tác kết thúc, vectơ ban đầu`A`chỉ có bốn hình thức cuối cùng có thể:```
A
R(A)
R^2(A)
R^3(A)
```Ở đâu`R`là một vòng quay 90 độ theo chiều kim đồng hồ. 

Việc bổ sung cũng dễ hiểu hơn sau khi xem xét các phép quay. Mỗi lần thêm`C`sau này có thể được luân chuyển, do đó tổng đóng góp của tất cả các phép cộng là tổ hợp số nguyên của:```
C
R(C)
R^2(C)
R^3(C)
```Hai cái cuối cùng chỉ là số âm của hai cái đầu tiên, vì vậy cái này giống với mạng được tạo bởi`C`Và`R(C)`. 

Đối với mỗi định hướng cuối cùng có thể có của`A`, chúng ta chỉ cần kiểm tra xem sự khác biệt giữa`B`và hướng đó thuộc về mạng này. 

Cho phép:```
C = (x, y)
R(C) = (y, -x)
```Một vectơ`(u, v)`nằm trong mạng này nếu tồn tại số nguyên`p`Và`q`như vậy:```
p * (x, y) + q * (y, -x) = (u, v)
```Ma trận của hệ này đặc biệt vì ma trận bình phương của nó là ma trận vô hướng:```
[x  y] [x  y]   [x²+y²  0]
[y -x] [y -x] = [0      x²+y²]
```Điều này đưa ra các hệ số:```
p = (x*u + y*v) / (x² + y²)
q = (y*u - x*v) / (x² + y²)
```Việc duy nhất còn lại là kiểm tra xem cả hai tử số có chia hết cho`x² + y²`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Tìm kiếm không giới hạn, theo cấp số nhân | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba vectơ`A`,`B`, Và`C`. Lưu trữ tọa độ dưới dạng số nguyên vì tất cả các phép tính có thể được thực hiện chính xác mà không cần số học dấu phẩy động. 
2. Nếu`C`là`(0, 0)`, kiểm tra bốn phép quay có thể có của`A`. Chỉ có thể xoay vì phép cộng không làm gì cả. 
3. Tính toán`x² + y²`vì`C = (x, y)`. Giá trị này là độ lớn xác định của hai vectơ cơ sở mạng. Nó cũng là mẫu số được sử dụng để khôi phục các hệ số nguyên. 
4. Hãy thử tất cả bốn phép quay cuối cùng có thể có của`A`. Với mỗi cái, hãy tính vectơ sai phân`D = B - rotated_A`. 
5. Kiểm tra xem`D`có thể được viết dưới dạng tổ hợp số nguyên của`C`và sự quay theo chiều kim đồng hồ của nó. Hai kiểm tra chia hết bắt buộc là:```
x*D.x + y*D.y
y*D.x - x*D.y
```Cả hai đều phải chia hết cho`x² + y²`. 

1. Nếu bất kỳ phép quay nào vượt qua bài kiểm tra mạng, hãy in`YES`. Nếu cả bốn đều thất bại, hãy in`NO`. 

Tại sao nó hoạt động: mọi chuỗi thao tác hợp lệ đều có thể được sắp xếp lại về mặt khái niệm thành một vòng quay cuối cùng của`A`cộng với một số phiên bản luân phiên của`C`. Các phép quay chỉ có bốn trạng thái có thể có và tổng các phép quay có thể có`C`vectơ tạo thành chính xác mạng số nguyên được tạo bởi`C`Và`R(C)`. Thuật toán kiểm tra mọi trạng thái xoay có thể có và sử dụng điều kiện thành viên chính xác cho mạng đó, do đó, nó chấp nhận mọi vectơ có thể truy cập và từ chối mọi vectơ không thể truy cập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def rotate(v):
    x, y = v
    return (y, -x)

def possible(a, b, c):
    cx, cy = c

    if cx == 0 and cy == 0:
        cur = a
        for _ in range(4):
            if cur == b:
                return True
            cur = rotate(cur)
        return False

    den = cx * cx + cy * cy

    cur = a
    for _ in range(4):
        dx = b[0] - cur[0]
        dy = b[1] - cur[1]

        p_num = cx * dx + cy * dy
        q_num = cy * dx - cx * dy

        if p_num % den == 0 and q_num % den == 0:
            return True

        cur = rotate(cur)

    return False

def solve():
    a = tuple(map(int, input().split()))
    b = tuple(map(int, input().split()))
    c = tuple(map(int, input().split()))

    print("YES" if possible(a, b, c) else "NO")

if __name__ == "__main__":
    solve()
```các`rotate`chức năng trực tiếp thực hiện một phần tư chiều kim đồng hồ. Áp dụng nó bốn lần sẽ trả về vectơ ban đầu, đó là lý do tại sao vòng lặp chính chỉ cần bốn lần lặp. 

Trường hợp vectơ 0 được xử lý riêng vì mẫu số mạng trở thành 0. Khi`C`bằng 0, thao tác duy nhất có sẵn là xoay, vì vậy việc kiểm tra bốn hướng là đủ. 

Đối với khác không`C`, biến`den`luôn luôn tích cực. Việc kiểm tra tính chia hết tránh các vấn đề về độ chính xác của dấu phẩy động và hoạt động chính xác với tọa độ lớn vì số nguyên Python có độ chính xác tùy ý. 

Hai biểu thức`p_num`Và`q_num`đến từ việc giải hệ hai nhân hai tuyến tính. Cả hai đều phải chia đều vì số lần mỗi hướng mạng được sử dụng phải là số nguyên. 

## Ví dụ đã hoạt động 

Ví dụ 1:```
Input
0 0
1 1
0 1
```Dấu vết là: 

| Bước | Vòng quay hiện tại của A | Sự khác biệt B - A | Tử số đầu tiên | Tử số thứ hai | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 0 | (0, 0) | (1, 1) | 1 | -1 | Đã chấp nhận | 

Mẫu số là`1`, vì vậy cả hai hệ số đều là số nguyên. Có thể truy cập mục tiêu bằng cách thêm vectơ`(0,1)`và sử dụng các phép quay để tạo ra hướng cần thiết. 

Ví dụ 2:```
Input
0 0
1 1
2 2
```Dấu vết là: 

| Bước | Vòng quay hiện tại của A | Sự khác biệt B - A | Mẫu số | Tính chia hết | 
| --- | --- | --- | --- | --- | 
| 0 | (0,0) | (1,1) | 8 | Thất bại | 
| 1 | (0,0) | (1,1) | 8 | Thất bại | 
| 2 | (0,0) | (1,1) | 8 | Thất bại | 
| 3 | (0,0) | (1,1) | 8 | Thất bại | 

Mạng được tạo ra bởi`(2,2)`Và`(2,-2)`chỉ chứa các vectơ có hệ số biến đổi chia hết cho`8`.`(1,1)`không có bên trong nó, nên câu trả lời là`NO`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có bốn phép quay và một số phép tính số học không đổi được thực hiện. | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ một vài cặp tọa độ. | 

Giá trị đầu vào lớn nhưng số lượng thao tác được thực hiện không phụ thuộc vào độ lớn của chúng. Giải pháp dễ dàng phù hợp trong giới hạn yêu cầu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue()

assert run("""0 0
1 1
0 1
""") == "YES\n", "sample 1"

assert run("""0 0
1 1
2 2
""") == "NO\n", "sample 2"

assert run("""0 0
0 0
0 0
""") == "YES\n", "zero vector"

assert run("""1 0
0 -1
0 0
""") == "YES\n", "rotation only"

assert run("""100000000 100000000
-100000000 100000000
100000000 -100000000
""") == "YES\n", "large coordinates"

assert run("""0 0
1 1
3 3
""") == "NO\n", "same direction but impossible scale"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 / 1 1 / 0 1`|`YES`| Trường hợp cơ bản có thể tiếp cận bằng cách sử dụng phép cộng và phép quay | 
|`0 0 / 1 1 / 2 2`|`NO`| Lưới phân chia từ chối | 
|`0 0 / 0 0 / 0 0`|`YES`| Xử lý véc tơ bằng không | 
|`1 0 / 0 -1 / 0 0`|`YES`| Xoay mà không cần bổ sung | 
| Trường hợp tọa độ lớn |`YES`| Số học số nguyên có giá trị lớn | 
|`0 0 / 1 1 / 3 3`|`NO`| Ngăn chặn việc kiểm tra nhiều lần không chính xác | 

## Vỏ cạnh 

Khi nào`C`bằng 0, thuật toán không bao giờ đưa vào tính toán mạng. Vì:```
0 0
1 1
0 0
```bốn phép quay của`(0,0)`là tất cả`(0,0)`, Vì thế`(1,1)`không thể đạt được và thuật toán trả về`NO`. 

Khi một vòng quay của`A`là bắt buộc, bốn bước kiểm tra trạng thái sẽ xử lý trực tiếp. Vì:```
1 0
0 -1
0 0
```lần kiểm tra đầu tiên không thành công vì các vectơ khác nhau, nhưng lần quay thứ hai tạo ra`(0,-1)`, phù hợp`B`. 

Khi câu trả lời phụ thuộc vào việc kết hợp các phép cộng xoay, bài kiểm tra mạng sẽ nắm bắt được nó. Vì:```
0 0
1 0
0 1
```các hướng được tạo ra là`(0,1)`Và`(1,0)`với các âm bản của chúng có sẵn thông qua các lần quay bổ sung. Chuyển động cần thiết nằm bên trong mạng nên thuật toán trả về`YES`.
