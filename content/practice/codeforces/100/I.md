---
title: "CF 100I - Xoay"
description: "Chúng ta có một điểm $(x, y)$ trên mặt phẳng 2D và một góc $k$ tính bằng độ. Nhiệm vụ là xoay điểm ngược chiều kim đồng hồ xung quanh gốc tọa độ chính xác $k$ độ và in tọa độ của điểm mới."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 100
codeforces_index: "I"
codeforces_contest_name: "Unknown Language Round 3"
rating: 1500
weight: 100
solve_time_s: 261
verified: true
draft: false
---

[CF 100I - Xoay](https://codeforces.com/problemset/problem/100/I) 

**Đánh giá:** 1500 
**Tags:** *đặc biệt, hình học, toán học 
**Thời gian giải:** 4 phút 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một điểm$(x, y)$trên mặt phẳng 2D và một góc$k$tính bằng độ. Nhiệm vụ là xoay điểm ngược chiều kim đồng hồ quanh điểm gốc một cách chính xác.$k$độ và in tọa độ của điểm mới. 

Một phép quay quanh điểm gốc bảo toàn khoảng cách từ điểm gốc trong khi làm thay đổi hướng của vectơ. Về mặt hình học, đây là phép biến đổi xoay 2D tiêu chuẩn. 

Những hạn chế là cực kỳ nhỏ. Các tọa độ chỉ được giới hạn bởi 1390 về giá trị tuyệt đối và chỉ có một điểm để xử lý. Sự phức tạp về thời gian thực sự không liên quan ở đây bởi vì ngay cả một giải pháp nặng về mặt toán học cũng có thể chạy ngay lập tức. Thách thức thực sự là triển khai công thức hình học một cách chính xác và xử lý độ chính xác của dấu phẩy động đủ cẩn thận để đáp ứng giới hạn lỗi. 

Sai số tương đối bắt buộc nhỏ hơn$10^{-1}$, điều đó rất dễ tha thứ. Số học dấu phẩy động có độ chính xác kép tiêu chuẩn là quá đủ. 

Một số trường hợp nguy hiểm có thể âm thầm phá vỡ việc triển khai bất cẩn. 

Xoay 0 độ sẽ trả về điểm ban đầu không thay đổi. 

đầu vào:```
0
5 -3
```Đầu ra đúng:```
5.00000000 -3.00000000
```Việc triển khai có lỗi có thể vô tình chuyển đổi độ không chính xác hoặc áp dụng xoay theo chiều kim đồng hồ thay vì xoay ngược chiều kim đồng hồ. 

Xoay 90 độ là một cái bẫy cổ điển khác vì tọa độ hoán đổi vị trí và một dấu hiệu thay đổi. 

đầu vào:```
90
1 1
```Đầu ra đúng:```
-1.00000000 1.00000000
```Nếu ma trận xoay được viết không chính xác, kết quả đầu ra có thể trở thành`(1, -1)`hoặc`(-1, -1)`. 

Tọa độ âm cũng rất quan trọng vì lỗi ký hiệu trở nên rõ ràng ở đó. 

đầu vào:```
180
-2 7
```Đầu ra đúng:```
2.00000000 -7.00000000
```Chuyển đổi góc sai hoặc vị trí sin/cosine không chính xác thường tạo ra kết quả phản chiếu thay vì xoay thực. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để suy nghĩ về vấn đề này là mô phỏng hình học. Người ta có thể liên tục thực hiện các phép quay nhỏ cho đến khi tổng góc đạt tới$k$độ. Vì một phép quay chỉ là một phép biến đổi tọa độ nên việc nhân liên tục với các ma trận góc nhỏ cuối cùng sẽ đạt đến hướng đích. 

Ý tưởng này đúng về mặt toán học vì các phép quay có bố cục rõ ràng. Áp dụng xoay 1 độ chín mươi lần sẽ tạo ra kết quả tương tự như một lần xoay 90 độ. 

Vấn đề là các phép tính dấu phẩy động lặp đi lặp lại sẽ tích lũy lỗi số một cách không cần thiết. Mặc dù các ràng buộc đủ nhỏ để nó vẫn chạy nhanh, nhưng nó không hiệu quả và kém chính xác hơn nhiều so với mức cần thiết. Nếu chúng ta mô phỏng từng độ một, trường hợp xấu nhất sẽ liên quan đến 359 phép nhân ma trận mà không mang lại lợi ích thực sự nào. 

Quan sát quan trọng là phép quay trong 2D đã có công thức dạng đóng trực tiếp. Nếu một điểm$(x, y)$được quay ngược chiều kim đồng hồ một góc$\theta$, tọa độ mới là:$\begin{aligned}x' &= x\cos\theta - y\sin\theta \\ y' &= x\sin\theta + y\cos\theta\end{aligned}$Điều này xuất phát trực tiếp từ hình học của vòng tròn đơn vị và ma trận quay. Vì các hàm lượng giác của Python hoạt động theo radian nên trước tiên chúng ta chuyển đổi độ sang radian. 

Giải pháp tối ưu chỉ đơn giản là đánh giá hai công thức này một lần. Điều đó mang lại độ phức tạp về thời gian không đổi và tránh sự trôi nổi tích lũy của dấu phẩy động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) | O(1) | Không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc góc quay`k`và tọa độ điểm`x`Và`y`. 
2. Chuyển đổi góc từ độ sang radian vì Python`math.sin`Và`math.cos`chức năng mong đợi radian. 

Công thức chuyển đổi là:$\theta = k \cdot \frac{\pi}{180}$1. Tính toán`cos(theta)`Và`sin(theta)`một lần. 

Việc tính toán chúng một lần sẽ tránh được công việc trùng lặp và giữ cho mã sạch hơn. 

1. Áp dụng công thức xoay 2D:$$x' = x \cos\theta - y \sin\theta$$

$$y' = x \sin\theta + y \cos\theta$$Những công thức này đến từ việc nhân vectơ điểm với ma trận xoay tiêu chuẩn. 

1. In tọa độ thu được bằng cách sử dụng định dạng dấu phẩy động. 

Thẩm phán cho phép sai số dấu phẩy động nhỏ nên định dạng thập phân tiêu chuẩn là đủ. 

### Tại sao nó hoạt động 

Xoay ngược chiều kim đồng hồ theo góc$\theta$được biểu diễn bằng ma trận:$\begin{bmatrix}\cos\theta & -\sin\theta \\ \sin\theta & \cos\theta\end{bmatrix}$Nhân ma trận này với vectơ$(x, y)$tạo ra tọa độ chính xác của điểm quay. Vì phép quay ma trận bảo toàn khoảng cách và góc, nên điểm được biến đổi chính xác là điểm ban đầu được quay quanh gốc tọa độ. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

# solution

k = int(input())
x, y = map(int, input().split())

theta = math.radians(k)

c = math.cos(theta)
s = math.sin(theta)

nx = x * c - y * s
ny = x * s + y * c

print(f"{nx:.8f} {ny:.8f}")
```Chương trình bắt đầu bằng cách đọc góc và tọa độ. Góc được chuyển đổi thành radian bằng cách sử dụng`math.radians`, giúp tránh các lỗi chuyển đổi thủ công. 

Các biến`c`Và`s`lưu trữ các giá trị cosin và sin của góc. Việc tính toán chúng một lần sẽ sạch hơn và hiệu quả hơn một chút. 

Các công thức cho`nx`Và`ny`trực tiếp thực hiện phép nhân ma trận quay. Thứ tự quan trọng. Một lỗi phổ biến là trộn các dấu hoặc hoán đổi các số hạng sin và cos. 

Đầu ra sử dụng định dạng thập phân cố định với 8 chữ số sau dấu thập phân. Bài toán chỉ cần một sai số tương đối nhỏ nên điều này khá chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
90
1 1
```| Bước | Giá trị | 
| --- | --- | 
| k | 90 | 
| theta |$\pi / 2$| 
| cos(theta) | 0 | 
| tội lỗi(theta) | 1 | 
| nx |$1 \cdot 0 - 1 \cdot 1 = -1$| 
| ny |$1 \cdot 1 + 1 \cdot 0 = 1$| 

Đầu ra:```
-1.00000000 1.00000000
```Dấu vết này thể hiện góc quay 90 độ cổ điển. điểm`(1,1)`di chuyển đến`(-1,1)`chính xác như mong đợi đối với chuyển động ngược chiều kim đồng hồ. 

### Ví dụ 2 

đầu vào:```
180
2 -3
```| Bước | Giá trị | 
| --- | --- | 
| k | 180 | 
| theta |$\pi$| 
| cos(theta) | -1 | 
| tội lỗi(theta) | 0 | 
| nx |$2 \cdot (-1) - (-3) \cdot 0 = -2$| 
| ny |$2 \cdot 0 + (-3) \cdot (-1) = 3$| 

Đầu ra:```
-2.00000000 3.00000000
```Xoay 180 độ sẽ lật điểm sang phía đối diện của điểm gốc. Ví dụ này xác nhận rằng các dấu hiệu được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học và lượng giác được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu bổ sung | 

Giải pháp dễ dàng phù hợp trong giới hạn. Chỉ có một điểm được xử lý và tất cả các hoạt động đều có thời gian không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
import math

def solve():
    input = sys.stdin.readline

    k = int(input())
    x, y = map(int, input().split())

    theta = math.radians(k)

    c = math.cos(theta)
    s = math.sin(theta)

    nx = x * c - y * s
    ny = x * s + y * c

    print(f"{nx:.8f} {ny:.8f}")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
assert run("90\n1 1\n") == "-1.00000000 1.00000000", "sample 1"

# zero rotation
assert run("0\n5 -3\n") == "5.00000000 -3.00000000", "zero rotation"

# 180 degree rotation
assert run("180\n2 -3\n") == "-2.00000000 3.00000000", "180 degrees"

# point at origin
assert run("270\n0 0\n") == "0.00000000 -0.00000000", "origin"

# maximum coordinate magnitude
out = run("90\n1390 -1390\n")
assert out.startswith("1390.00000000"), "large coordinates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 / 5 -3`|`5.00000000 -3.00000000`| Luân chuyển danh tính | 
|`180 / 2 -3`|`-2.00000000 3.00000000`| Ký hiệu đúng | 
|`270 / 0 0`|`0.00000000 -0.00000000`| Nguồn gốc vẫn cố định | 
|`90 / 1390 -1390`| khoảng`1390 1390`| Xử lý tọa độ lớn | 

## Vỏ cạnh 

Xoay 0 độ sẽ giữ nguyên điểm. 

đầu vào:```
0
5 -3
```Thuật toán chuyển đổi 0 độ thành 0 radian. Từ:$$\cos(0)=1$$Và$$\sin(0)=0$$các công thức trở thành:$$x' = 5 \cdot 1 - (-3)\cdot 0 = 5$$

$$y' = 5 \cdot 0 + (-3)\cdot 1 = -3$$Đầu ra vẫn giống hệt với điểm đầu vào. 

Xoay 90 độ rất nhạy cảm với lỗi dấu hiệu. 

đầu vào:```
90
1 1
```Thuật toán tính toán:$$\cos(90^\circ)=0$$

$$\sin(90^\circ)=1$$Sau đó:$$x' = 1\cdot0 - 1\cdot1 = -1$$

$$y' = 1\cdot1 + 1\cdot0 = 1$$Điều này xác nhận vòng quay ngược chiều kim đồng hồ chứ không phải theo chiều kim đồng hồ. 

Nguồn gốc là một trường hợp ranh giới hữu ích khác. 

đầu vào:```
270
0 0
```Bất kể góc độ:$$x' = 0$$

$$y' = 0$$Thuật toán bảo toàn chính xác gốc tọa độ vì mọi thuật ngữ trong công thức đều bằng 0.
