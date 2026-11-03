---
title: "CF 1101A - Số nguyên tối thiểu"
description: "Mỗi truy vấn mô tả một khoảng cấm trên trục số và kích thước bước cố định. Chúng ta được yêu cầu tìm số nguyên dương nhỏ nhất là bội số của một số $d$ cho trước, nhưng nằm hoàn toàn bên ngoài khoảng $[l, r]$."
date: "2026-06-13T07:19:17+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1101
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 58 (Rated for Div. 2)"
rating: 1000
weight: 1101
solve_time_s: 304
verified: true
draft: false
---

[CF 1101A - Số nguyên tối thiểu](https://codeforces.com/problemset/problem/1101/A) 

**Đánh giá:** 1000 
**Thẻ:** toán 
**Thời gian giải:** 5 phút 4 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi truy vấn mô tả một khoảng cấm trên trục số và kích thước bước cố định. Chúng ta được yêu cầu tìm số nguyên dương nhỏ nhất là bội số của một số cho trước$d$, nhưng nằm hoàn toàn bên ngoài khoảng$[l, r]$. 

Một cách hữu ích để nghĩ về điều này là tất cả các ứng viên hợp lệ đều được tính điểm$d, 2d, 3d, \dots$, và trong số này, chúng tôi muốn cái đầu tiên không nằm trong phân đoạn bị chặn. Đoạn bị chặn có thể cắt bỏ một hoặc nhiều bội số của$d$, nhưng câu trả lời phải là bội số sớm nhất tồn tại sau loại trừ này. 

Các ràng buộc nhỏ về số lượng truy vấn, nhưng các giá trị bên trong mỗi truy vấn có thể lớn tới$10^9$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng lặp lại tất cả các bội số cho đến$r$cho mỗi truy vấn. Ngay cả khi mỗi truy vấn chỉ tốn$O(r/d)$, trong trường hợp xấu nhất điều này trở thành$10^9$hoạt động cho mỗi truy vấn, điều này là không thể trong giới hạn 1 giây. 

Các trường hợp cạnh chính xuất phát từ cách bội số tương tác với các ranh giới khoảng. Đầu tiên, nếu$d > r$, thì mọi bội số ngoại trừ$d$bản thân nó có thể đã nằm ngoài phân khúc, vì vậy câu trả lời rất tầm thường$d$. Thứ hai, nếu$d < l$, sau đó$d$đã hợp lệ và là câu trả lời. Trường hợp thú vị duy nhất là khi bội số của$d$rơi vào trong khoảng và chúng ta phải "nhảy" qua nó. 

Một trường hợp thất bại tinh vi đối với lý luận ngây thơ là giả sử chúng ta chỉ cần kiểm tra$r$. Ví dụ, nếu$l = 5$,$r = 10$,$d = 4$, bội số là$4, 8, 12, \dots$. Số 8 nằm trong khoảng nên chỉ bỏ qua bằng cách chọn$r$sẽ chấp nhận sai 8 nếu chúng ta chỉ kiểm tra$r$giới hạn dựa trên mà không cần xác minh tư cách thành viên trong$[l, r]$. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực tạo ra bội số$d$: bắt đầu từ$d$, tiếp tục thêm$d$và dừng lại khi tìm thấy giá trị bên ngoài$[l, r]$. Điều này đúng vì nó kiểm tra rõ ràng định nghĩa của câu trả lời. Tuy nhiên, trong trường hợp xấu nhất khi$d = 1$Và$l = 1, r = 10^9$, chúng tôi sẽ quét tới$10^9$giá trị vượt xa các hoạt động cho phép. 

Quan sát quan trọng là chúng ta chỉ cần suy luận về một ứng cử viên duy nhất liên quan đến khoảng thời gian. Hoặc là bội số đầu tiên$d$đã ở bên ngoài phân khúc hoặc nó ở bên trong. Nếu nó nằm bên trong thì tất cả các bội số nhỏ hơn đều không liên quan và chúng ta chỉ cần "nhảy" tới bội số đầu tiên lớn hơn chính xác$r$. Giá trị đó chính xác là bội số tiếp theo của$d$sau đó$r$, có thể được tính trực tiếp bằng phép tính số học thay vì phép lặp. 

Điều này làm giảm vấn đề thành tính toán theo thời gian không đổi cho mỗi truy vấn: quyết định xem$d$nằm trong khoảng và nếu vậy hãy tính bội số tiếp theo ở trên$r$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(r/d) mỗi truy vấn | O(1) | Quá chậm | 
| Tối ưu | O(1) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$l, r, d$. Mục đích là tìm bội số đầu tiên của$d$điều đó tránh được$[l, r]$. 
2. Kiểm tra xem$d$nằm trong khoảng$[l, r]$. Nếu như$d < l$, sau đó$d$đã hợp lệ và là câu trả lời nhỏ nhất có thể vì nó là bội số dương nhỏ nhất của$d$. 
3. Nếu$d \ge l$, chúng tôi kiểm tra xem$d \le r$. Nếu điều này là sai thì một lần nữa$d$nằm ngoài khoảng và là đáp án. 
4. Nếu$l \le d \le r$, sau đó$d$bị cấm, vì vậy chúng ta phải bỏ qua tất cả các bội số vẫn có thể nằm trong hoặc chồng lên khoảng. 
5. Ứng viên tiếp theo bội số sau$r$được tính như$\left\lfloor \frac{r}{d} \right\rfloor d + d$. Điều này cho bội số nhỏ nhất hoàn toàn lớn hơn$r$. 
6. Xuất giá trị tính toán đó. 

Bước quan trọng là nhận ra rằng khi một bội số nằm trong đoạn bị cấm thì câu trả lời tối ưu không thể là bất kỳ bội số nhỏ hơn nào, bởi vì tất cả các bội số nhỏ hơn đã được kiểm tra và từ chối. Cấu trúc bội số đảm bảo rằng ứng cử viên hợp lệ tiếp theo là bội số đầu tiên nằm ngoài điểm cuối bên phải. 

### Tại sao nó hoạt động 

Tất cả các số hợp lệ tạo thành một chuỗi bội số tăng dần của$d$. Vùng cấm loại bỏ một đoạn liền kề của dãy số. Nếu bội số đầu tiên$d$không bị loại bỏ, nó phải tối ưu vì nó là tối thiểu. Mặt khác, khi bội số đi vào phân đoạn bị cấm, mọi bội số tiếp theo cho đến một điểm nào đó vẫn ở bên trong hoặc phía trên phân đoạn đó theo thứ tự. Bội số tiếp theo sau$r$là điểm đầu tiên đảm bảo nằm ngoài$[l, r]$, và không có bội số nhỏ hơn ngoài$d$có thể tồn tại mà không vi phạm tính chia hết hoặc ràng buộc khoảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

q = int(input())
for _ in range(q):
    l, r, d = map(int, input().split())

    if d < l or d > r:
        print(d)
    else:
        print((r // d + 1) * d)
```Mã trực tiếp triển khai logic bắt nguồn từ hướng dẫn này. điều kiện`d < l or d > r`nắm bắt trường hợp bội số nhỏ nhất đã tránh được khoảng, do đó không cần điều chỉnh. Mặt khác, chúng tôi tính toán bội số đầu tiên lớn hơn`r`dùng phép chia số nguyên. 

biểu thức`(r // d + 1) * d`là một cách tiêu chuẩn để chuyển sang bội số tiếp theo. Nó tránh hoàn toàn các vòng lặp và dựa vào phép chia tầng để tìm bội số cuối cùng không vượt quá`r`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
l = 2, r = 4, d = 2
```| Bước | d trong [l, r]? | Hành động | Kết quả | 
| --- | --- | --- | --- | 
| 1 | Có | tính bội số tiếp theo sau r | 6 | 

Bội số 2 đầu tiên nằm trong khoảng, 4 cũng nằm trong nên chúng ta nhảy lên 6. 

Điều này xác nhận tính bất biến rằng khi bội số hợp lệ nhỏ nhất bị chặn, chúng ta phải bỏ qua toàn bộ phân đoạn và chuyển sang khối bội số tiếp theo. 

### Ví dụ 2 

đầu vào:```
l = 5, r = 10, d = 4
```| Bước | d trong [l, r]? | Hành động | Kết quả | 
| --- | --- | --- | --- | 
| 1 | Không | trả lại d trực tiếp | 4 | 

Ở đây bội số đầu tiên đã hợp lệ nên không cần lý do gì thêm. Điều này thể hiện trường hợp rút gọn trong đó không cần điều chỉnh khoảng thời gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) | Mỗi truy vấn được xử lý bằng một số phép toán số học không đổi | 
| Không gian | O(1) | Không có cấu trúc bổ sung ngoài các biến đầu vào | 

Lời giải dễ dàng nằm trong giới hạn vì$q \le 500$và mỗi truy vấn có thời gian không đổi, dẫn đến tổng cộng tối đa vài nghìn thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    q = int(input())
    out = []
    for _ in range(q):
        l, r, d = map(int, input().split())
        if d < l or d > r:
            out.append(str(d))
        else:
            out.append(str((r // d + 1) * d))
    return "\n".join(out)

# provided samples
assert run("5\n2 4 2\n5 10 4\n3 10 1\n1 2 3\n4 6 5\n") == "6\n4\n1\n3\n10"

# custom cases
assert run("1\n1 100 101\n") == "101", "d just above range"
assert run("1\n10 20 3\n") == "3", "small multiple already valid"
assert run("1\n6 6 2\n") == "8", "tight interval containing multiple"
assert run("1\n1 1 1\n") == "2", "minimal boundary case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 100 101 | 101 | d lớn hơn khoảng | 
| 10 20 3 | 3 | d đã tối ưu | 
| 6 6 2 | 8 | bội số chặn khoảng thời gian một điểm | 
| 1 1 1 | 2 | trường hợp khoảng cách cạnh nhỏ nhất | 

## Vỏ cạnh 

Khi nào$d > r$, khoảng đó hoàn toàn không ảnh hưởng đến bội số nhỏ nhất. Ví dụ, với đầu vào$l=1, r=5, d=10$, thuật toán trả về ngay 10 vì nó đã nằm ngoài phân đoạn. 

Khi$d \in [l, r]$, bội số đầu tiên bị chặn và chúng ta phải nhảy. Vì$l=2, r=10, d=3$, bội số là 3, 6, 9, 12. Ba số đầu tiên nằm trong hoặc gần khoảng, nhưng câu trả lời đúng là 12, chính xác là bội số đầu tiên ở trên$r$. Sự tính toán`(r // d + 1) * d`trực tiếp tạo ra 12, khớp với hành vi bỏ qua dự kiến ​​mà không cần lặp lại.
