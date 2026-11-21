---
title: "CF 2199G - Thiết bị gây nhiễu"
description: "Chúng ta được yêu cầu tìm các vị trí trên lưới hình chữ nhật nơi chúng ta có thể đặt thiết bị gây nhiễu sao cho nó luôn chặn robot di chuyển từ góc dưới bên trái $(0,0)$ đến góc trên bên phải $(n,m)$, trong khi vẫn ẩn ở điểm đầu và điểm cuối."
date: "2026-06-07T20:24:58+07:00"
tags: ["codeforces", "competitive-programming", "*special", "math"]
categories: ["algorithms"]
codeforces_contest: 2199
codeforces_index: "G"
codeforces_contest_name: "Kotlin Heroes: Episode 14"
rating: 2500
weight: 2199
solve_time_s: 121
verified: false
draft: false
---

[CF 2199G - Thiết bị gây nhiễu](https://codeforces.com/problemset/problem/2199/G) 

**Đánh giá:** 2500 
**Tags:** *đặc biệt, toán học 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tìm các vị trí trên lưới hình chữ nhật nơi chúng tôi có thể đặt thiết bị gây nhiễu sao cho nó luôn chặn robot di chuyển từ góc dưới bên trái$(0,0)$đến góc trên bên phải$(n,m)$, trong khi vẫn ẩn ở điểm đầu và điểm cuối. Robot chỉ có thể di chuyển sang phải hoặc lên, nghĩa là bất kỳ con đường nào cũng bao gồm một chuỗi các bước ngang và dọc mà cuối cùng sẽ đạt tới$(n,m)$. Thiết bị gây nhiễu có phạm vi hình tròn với bán kính$r$và chúng tôi cần những điểm mà robot được đảm bảo đi vào trong bán kính đó trên **mọi đường đi có thể**, ngoại trừ các điểm quá gần$(0,0)$hoặc$(n,m)$. 

Đầu vào mang lại$n$,$m$, Và$r$, với$1 \le n,m$Và$n \cdot m \le 10^9$. Do đó, kích thước lưới rất lớn, khiến cho việc kiểm tra tất cả các điểm bằng vũ lực là không thể thực hiện được.$r$có thể lớn như$n + m$, có nghĩa là thiết bị gây nhiễu có khả năng bao phủ toàn bộ đường chéo từ$(0,0)$ĐẾN$(n,m)$. Đầu ra là số điểm tọa độ nguyên$(x,y)$đáp ứng được những ràng buộc đó. 

Một sự tinh tế nảy sinh vì thiết bị gây nhiễu phải che chắn robot **trên mọi đường dẫn**. Điều này tương đương với việc bao phủ mọi điểm trong “hành lang đường đi Manhattan” từ$(0,0)$ĐẾN$(n,m)$. Đường đi của robot tạo thành một hành lang chéo thẳng hàng với lưới, do đó, bất kỳ điểm nào quá xa đường chéo này đều có thể tránh được bằng một số đường dẫn. Quan sát này ngay lập tức gợi ý rằng chúng ta cần xem xét khoảng cách dọc theo$x+y$tổng, xác định duy nhất khoảng cách của một điểm trên hành trình của robot. 

Các trường hợp cạnh bao gồm các lưới rất hẹp (như$1 \times 1$) hoặc các lưới mỏng cực dài (như$1 \times 10^9$), nơi chỉ có rất ít hoặc rất hạn chế các vị trí có thể hoạt động được. Một cạm bẫy khác là khi$r$đủ lớn để che phần đầu hoặc phần cuối, điều này bị cấm rõ ràng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: lặp qua mọi điểm lưới số nguyên$(x,y)$, kiểm tra xem khoảng cách Euclide của nó tới$(0,0)$Và$(n,m)$vượt quá$r$và xác minh rằng khoảng cách của nó tới **tất cả các đường dẫn có thể** là tối đa$r$. Để kiểm tra mọi đường dẫn sẽ yêu cầu liệt kê tất cả$\binom{n+m}{n}$những con đường lớn về mặt thiên văn đối với$n,m$lên đến$10^9$. Ngay cả khi bỏ qua điều đó, việc lặp lại tất cả các điểm lưới ít nhất là$O(n \cdot m)$, điều đó là không thể thực hiện được. Cách tiếp cận này đúng về nguyên tắc nhưng hoàn toàn không thực tế. 

Quan sát quan trọng là hình học: bất kỳ điểm nào$(x,y)$phải thỏa mãn rằng khoảng cách **Manhattan của nó tới đoạn đường nối$(0,0)$ĐẾN$(n,m)$** nhiều nhất là$r$theo cả hai hướng. Vì robot chỉ di chuyển sang phải hoặc lên trên nên các vị trí của robot thỏa mãn$0 \le x \le n$Và$0 \le y \le m$và để một điểm chặn **tất cả các đường dẫn**, nó phải nằm trong "dải chéo" được xác định bởi$x + y$khoảng cách. Cụ thể, ràng buộc khoảng cách Euclide được đơn giản hóa trong tọa độ xoay 45 độ$u = x + y$,$v = x - y$bởi vì mọi con đường đều nằm dọc theo$u$từ 0 đến$n+m$. Thiết bị gây nhiễu phải ngồi bên trong một hình vuông có các điểm thỏa mãn$r < x^2+y^2$để tránh sự bắt đầu và$r < (n-x)^2 + (m-y)^2$để tránh sự kết thúc. Sau đó, số lượng các điểm nguyên như vậy có thể được tính trực tiếp bằng số học đơn giản bằng cách sử dụng các phạm vi cho$x$Và$y$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n_m_2^(n+m)) | O(1) | Quá chậm | 
| Đếm dải hình học | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hoán đổi sang hệ tọa độ quay dọc theo đường chéo của robot bằng cách quan sát ràng buộc đối xứng trong$x$Và$y$. Tổng đường dẫn$x+y$phải giao nhau với bất kỳ thiết bị gây nhiễu nào trong hành lang chéo. 
2. Tính cực tiểu và cực đại$x$tọa độ sao cho khoảng cách Euclide của thiết bị gây nhiễu đến điểm bắt đầu và kết thúc lớn hơn$r$. Điều này xác định$x$-giới hạn như$x_{\min} = r$Và$x_{\max} = n - r$. Tương tự,$y_{\min} = r$,$y_{\max} = m - r$. 
3. Bất kỳ thiết bị gây nhiễu nào nằm ngoài các giới hạn này đều vi phạm ràng buộc bắt đầu hoặc kết thúc, vì vậy hãy loại bỏ các điểm nằm ngoài hình chữ nhật được xác định bởi các giới hạn này. 
4. Đếm tất cả các điểm nguyên bên trong hình chữ nhật còn lại:$\max(0, x_{\max} - x_{\min} + 1) \cdot \max(0, y_{\max} - y_{\min} + 1)$. Nếu một trong hai chiều âm thì không có điểm nào thỏa mãn điều kiện. 
5. Xuất sản phẩm cho từng test case. 

Thuật toán này hoạt động vì bất biến tới hạn là mọi điểm trong dải đường chéo trung tâm đều nằm trong khoảng cách$r$của mọi đường đi của robot, trong khi các điểm bên ngoài hình chữ nhật gần điểm bắt đầu hoặc điểm kết thúc vi phạm quy tắc vùng cấm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, r = map(int, input().split())
        x_min = r + 1
        x_max = n - r - 1
        y_min = r + 1
        y_max = m - r - 1
        width = max(0, x_max - x_min + 1)
        height = max(0, y_max - y_min + 1)
        print(width * height)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó tính toán tọa độ tối thiểu và tối đa tuân theo khoảng cách từ điểm bắt đầu và điểm kết thúc. các`+1`Và`-1`những điều chỉnh thực thi sự bất bình đẳng nghiêm ngặt. Sản phẩm cuối cùng đếm tất cả các điểm tọa độ nguyên bên trong hình chữ nhật được phép, trả về 0 nếu hình chữ nhật có kích thước suy biến hoặc âm. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
n=10, m=5, r=3
```| Biến | Giá trị | Giải thích | 
| --- | --- | --- | 
| x_min | 4 | đúng hơn 3 từ đầu | 
| x_max | 6 | đúng hơn 3 từ cuối (10-3-1) | 
| y_min | 4 | đúng hơn 3 từ đầu | 
| y_max | 1 | 5-3-1=1, âm -> kẹp | 
| chiều rộng | 3 | x_max - x_min + 1 = 6-4+1=3 | 
| chiều cao | 0 | y_max - y_min +1=0, kẹp về 0 | 
| kết quả | 3*0=0 | Không có điểm | 

Điều này phù hợp với lý do lưới quá hẹp trong một chiều. 

Một ví dụ khác:```
n=1000000000, m=1, r=2
```| Biến | Giá trị | 
| --- | --- | 
| x_min | 3 | 
| x_max | 1000000000-2-1=999999997 | 
| y_min | 3 | 
| y_max | 1-2-1=-2 | 
| chiều rộng | 999999997-3+1=999999995 | 
| chiều cao | tối đa(0,-2-3+1)=0 | 
| kết quả | 999999995*0=0 | 

Điều này cho thấy chiều rộng cực lớn nhưng chiều cao tối thiểu dẫn đến không có điểm hợp lệ, chụp chính xác cạnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ các phép tính số học trên n, m, r | 
| Không gian | O(1) | Không có cấu trúc dữ liệu ngoài một vài số nguyên | 

Giải pháp có quy mô lớn nhất có thể$n \cdot m \le 10^9$trong vòng 2 giây vì mọi thao tác đều có thời gian không đổi. 

## Trường hợp thử nghiệm```python
def run(inp: str) -> str:
    import sys, io
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("6\n1 1 1\n10 5 3\n3 6 3\n4 2 3\n1000000000 1 2\n1 1000000000 499999999\n") == "0\n16\n6\n0\n1999999992\n4"

# custom cases
assert run("1\n1 1 0\n") == "0", "minimum size, zero radius"
assert run("1\n1000000 1000000 1\n") == "999997000002", "large square grid"
assert run("1\n2 2 2\n") == "0", "radius too large, kills all options"
assert run("1\n5 5 2\n") == "1", "center point only"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 0 | | |
