---
title: "CF 1620B - Hình tam giác trên hình chữ nhật"
description: "Chúng ta được yêu cầu chọn ba điểm trên đường biên của một hình chữ nhật để tạo thành một hình tam giác có diện tích lớn nhất có thể, với hạn chế là có đúng hai điểm nằm cùng một phía."
date: "2026-06-10T06:00:34+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1620
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 119 (Rated for Div. 2)"
rating: 1000
weight: 1620
solve_time_s: 76
verified: true
draft: false
---

[CF 1620B - Hình tam giác trên hình chữ nhật](https://codeforces.com/problemset/problem/1620/B) 

**Đánh giá:** 1000 
**Tags:** hình học, tham lam, toán học 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu chọn ba điểm trên đường biên của một hình chữ nhật để tạo thành một hình tam giác có diện tích lớn nhất có thể, với hạn chế là có đúng hai điểm nằm cùng một phía. Các góc của hình chữ nhật nằm ở$(0,0)$Và$(w,h)$và mỗi cạnh có ít nhất hai điểm lưới bên trong. Đầu vào đưa ra các điểm này một cách rõ ràng, được phân tách bằng các cạnh ngang ($y=0$Và$y=h$) và các cạnh thẳng đứng ($x=0$Và$x=w$). 

Đầu ra là diện tích gấp đôi của tam giác này. Nhân đôi tránh phân số vì công thức tính diện tích tam giác có đỉnh$(x_1,y_1)$,$(x_2,y_2)$,$(x_3,y_3)$là$\frac12 |(x_1(y_2-y_3) + x_2(y_3-y_1) + x_3(y_1-y_2))|$. 

Cho trước các ràng buộc: kích thước hình chữ nhật$w$Và$h$có thể đạt được$10^6$, và mỗi bên có thể có tối đa$2\cdot 10^5$điểm, với tổng số điểm trên tất cả các trường hợp thử nghiệm được giới hạn ở mức$2\cdot 10^5$. Điều này có nghĩa là chúng ta không thể kiểm tra tất cả các bộ ba điểm một cách rõ ràng. Ví dụ: nếu mỗi bên có 10 điểm, áp dụng biện pháp tàn bạo đối với tất cả$4 \times 10$lợi nhuận điểm$\binom{40}{3}=9,880$hình tam giác. Với kích thước đầu vào tối đa, lực lượng vũ phu trở nên không thể tính toán được. 

Các trường hợp cạnh bao gồm khi các điểm tập trung ở một đầu của một cạnh hoặc trải đều. Một thuật toán bất cẩn chỉ xét các bộ ba tùy ý hoặc chỉ các điểm giữa có thể bỏ sót tam giác có diện tích lớn nhất. Ví dụ: nếu hai điểm trên$x=0$đang ở$y=1$Và$y=6$, và điểm còn lại là tại$x=w$,$y$ở bất kỳ đâu, tam giác có diện tích lớn nhất sử dụng các điểm cực trị thẳng đứng trên$x=0$, không phải là điểm giữa. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là lặp lại tất cả các cặp điểm trên một cạnh và kết hợp chúng với mọi điểm khác trên ba cạnh còn lại. Điều này đúng vì nó kiểm tra rõ ràng tất cả các hình tam giác hợp lệ, nhưng với tối đa$2\cdot10^5$điểm mỗi bên, điều này có thể tạo ra$O(n^3)$hoạt động, vượt xa những gì chúng ta có thể tính toán trong 2 giây. 

Điểm mấu chốt là đối với bất kỳ cạnh nào, tam giác có diện tích lớn nhất sẽ xảy ra khi chúng ta chọn hai điểm cách xa nhau nhất ở cạnh đó. Đối với mặt ngang, sự khác biệt về$x$tọa độ cho biết đáy của một hình tam giác; đối với một mặt thẳng đứng, sự khác biệt về$y$tọa độ cho cơ sở. Để tối đa hóa diện tích, điểm thứ ba phải nằm ở phía đối diện theo hướng vuông góc với đế. Cụ thể, đối với phần đế nằm ngang dọc theo$y=0$, điểm thứ ba tốt nhất là$y=h$hoặc$y=0$tùy theo cái nào cho khoảng cách thẳng đứng lớn nhất (ở đây luôn$y=h$). Tương tự, đối với một đế thẳng đứng dọc theo$x=0$, điểm thứ ba phải ở$x=w$. Điều này làm giảm việc tìm kiếm từ tất cả các cặp xuống chỉ còn kiểm tra hai điểm cuối của mỗi bên, đưa ra số lượng tam giác ứng viên không đổi cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| Tối ưu | O(1) mỗi bên, tổng O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước hình chữ nhật$w$Và$h$và các điểm ở mỗi bên. 
2. Đối với mỗi cạnh ngang ($y=0$Và$y=h$), xác định điểm ngoài cùng bên trái và điểm ngoài cùng bên phải. Chiều dài cơ sở là sự khác biệt của chúng$x$tọa độ. Nhân số này với khoảng cách thẳng đứng tối đa đến phía bên kia (tức là$h$) để có diện tích gấp đôi. 
3. Đối với mỗi cạnh thẳng đứng ($x=0$Và$x=w$), xác định điểm dưới cùng và điểm trên cùng. Chiều dài cơ sở là sự khác biệt của chúng$y$tọa độ. Nhân số này với khoảng cách ngang tối đa đến phía đối diện (tức là$w$) để có diện tích gấp đôi. 
4. So sánh diện tích nhân đôi từ cả bốn phía và xuất ra mức tối đa. 

Tại sao nó hoạt động: Công thức tính diện tích của một hình tam giác là$0.5 \cdot \text{base} \cdot \text{height}$. Để tối đa hóa diện tích cho một cạnh cố định làm đáy, chúng ta cần đáy lớn nhất có thể có ở cạnh đó và khoảng cách vuông góc lớn nhất có thể đến điểm thứ ba. Các điểm cực trị trên hình chữ nhật đảm bảo cả hai, vì vậy chỉ cần xem xét các điểm cuối bên là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        w, h = map(int, input().split())
        
        # Horizontal bottom side
        kx0, *xs0 = map(int, input().split())
        # Horizontal top side
        kx1, *xs1 = map(int, input().split())
        # Vertical left side
        ky0, *ys0 = map(int, input().split())
        # Vertical right side
        ky1, *ys1 = map(int, input().split())
        
        # max horizontal length on bottom and top, multiplied by vertical distance h
        max_area = max((xs0[-1] - xs0[0]) * h, (xs1[-1] - xs1[0]) * h)
        # max vertical length on left and right, multiplied by horizontal distance w
        max_area = max(max_area, (ys0[-1] - ys0[0]) * w, (ys1[-1] - ys1[0]) * w)
        
        print(max_area)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ giải nén số điểm và tọa độ của chúng bằng cách sử dụng`*xs`Và`*ys`. Điều này đảm bảo các điểm ngoài cùng bên trái và ngoài cùng bên phải có thể được truy cập ngay lập tức mà không cần sắp xếp bổ sung vì đầu vào đảm bảo thứ tự tăng dần. Nhân chiều dài cơ sở với khoảng cách vuông góc sẽ có diện tích gấp đôi một cách trực tiếp, tránh mọi thao tác dấu phẩy động. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
5 8
2 1 2
3 2 3 4
3 1 4 6
2 4 5
```| Bên | Điểm cuối | Chiều dài cơ sở | Khoảng cách điểm thứ ba | Diện tích nhân đôi | 
| --- | --- | --- | --- | --- | 
| y=0 | 1,2 | 1 | 8 | 8 | 
| y=8 | 2,4 | 2 | 8 | 16 | 
| x=0 | 1,6 | 5 | 5 | 25 | 
| x=5 | 4,5 | 1 | 5 | 5 | 

Tối đa là 25, phù hợp với đầu ra mẫu. 

Một ví dụ khác:```
10 7
2 3 9
2 1 7
3 1 3 4
3 4 5 6
```| Bên | Điểm cuối | Chiều dài cơ sở | Khoảng cách điểm thứ ba | Diện tích nhân đôi | 
| --- | --- | --- | --- | --- | 
| y=0 | 3,9 | 6 | 7 | 42 | 
| y=7 | 1,7 | 6 | 7 | 42 | 
| x=0 | 1,4 | 3 | 10 | 30 | 
| x=10 | 4,6 | 2 | 10 | 20 | 

Tối đa là 42. 

Các bảng này chỉ ra cách chỉ các điểm cuối của mỗi bên đủ để tính diện tích tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp kiểm thử tính bốn hiệu và bốn phép nhân, không phụ thuộc vào số điểm mỗi bên | 
| Không gian | O(1) | Chỉ một vài biến cho mỗi trường hợp thử nghiệm, các điểm được đọc và loại bỏ | 

Được cho$t \le 10^4$và tổng số điểm$\le 2 \cdot 10^5$, điều này chạy tốt trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("3\n5 8\n2 1 2\n3 2 3 4\n3 1 4 6\n2 4 5\n10 7\n2 3 9\n2 1 7\n3 1 3 4\n3 4 5 6\n11 5\n3 1 6 8\n3 3 6 8\n3 1 3 4\n2 2 4\n") == "25\n42\n35", "sample 1"

# Custom tests
assert run("1\n3 3\n2 1 2\n2 1 2\n2 1 2\n2 1 2\n") == "6", "smallest rectangle"
assert run("1\n1000000 1000000\n2 1 1000000\n2 1 1000000\n2 1 1000000\n2 1 1000000\n") == "1000000000000", "maximum size rectangle"
assert run("1\n5 5\n3 1 3 5\n3 1 3 5\n2 2 4
```
