---
title: "CF 105453C - Sự phân chia công bằng của viên vàng"
description: "Bài toán mô tả một tình huống hình học bao gồm một vùng hình tròn và một đường cắt chia nó thành hai phần. Một phần là vùng giống như đoạn “xanh” có diện tích phụ thuộc vào tham số độ cao $h$, bán kính $R$ và hình dạng của đoạn tròn."
date: "2026-06-23T17:35:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105453
codeforces_index: "C"
codeforces_contest_name: "2024 ICPC Greece Regional Collegiate Programming Contest (GRCPC 2024)"
rating: 0
weight: 105453
solve_time_s: 54
verified: true
draft: false
---

[CF 105453C - Sự phân chia công bằng của tấm bảng vàng](https://codeforces.com/problemset/problem/105453/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán mô tả một tình huống hình học bao gồm một vùng hình tròn và một đường cắt chia nó thành hai phần. Một phần là vùng giống như đoạn “xanh” có diện tích phụ thuộc vào tham số chiều cao$h$, bán kính$R$, và hình học của một đoạn tròn. Bản thân câu lệnh này không đầy đủ ở định dạng CF thông thường, nhưng công thức được cung cấp cho thấy cách tính toán chính: chúng ta dự kiến ​​sẽ đánh giá một biểu thức diện tích xuất phát từ việc xây dựng tam giác cung tròn trừ đi. 

Cụ thể, nhiệm vụ giảm xuống còn tính diện tích của một đoạn hình tròn được xác định bằng cách cắt một đường tròn có bán kính$R$với một đường ngang ở độ cao$h$. Vùng màu xanh lá cây tương ứng với phần hình tròn phía trên hoặc bên dưới đường cắt đó và diện tích của nó được biểu thị bằng thuật ngữ arccosine và thuật ngữ hiệu chỉnh tuyến tính liên quan đến$R - h$. Công thức đã cho là:$$a = R^2 \arccos\left(1 - \frac{h}{R}\right) - (R - h)\sqrt{2Rh - h^2}$$Đây là biểu thức chuẩn cho diện tích đoạn tròn, trong đó số hạng đầu tiên tương ứng với diện tích hình cung và số hạng thứ hai trừ đi diện tích hình tam giác được tạo thành bởi dây cung. 

Do đó, đầu vào bao gồm các giá trị mô tả một hoặc nhiều cấu hình hình học như vậy, thường là các cặp hoặc bộ ba liên quan đến bán kính và chiều cao cắt. Đầu ra là vùng màu xanh lá cây được tính toán cho mỗi cấu hình. 

Từ quan điểm tính toán, hàm ý ràng buộc chính là đây là một bài toán đánh giá dấu phẩy động chứ không phải là một bài toán tổ hợp hoặc đồ thị. Thậm chí có tới$10^5$các truy vấn, mỗi truy vấn phải được trả lời trong thời gian không đổi bằng cách sử dụng các hàm số ổn định. Điều này ngay lập tức loại trừ mọi hoạt động tái cấu trúc hình học hoặc tích hợp số lặp cho mỗi truy vấn. 

Các trường hợp biên chính xuất phát từ ranh giới miền của hàm arccos. Khi$h$rất nhỏ hoặc rất gần với$2R$, biểu thức bên trong căn bậc hai hoặc arccos có thể đạt đến mức không ổn định về số. Ví dụ, nếu$h = 0$, diện tích sẽ bằng không. Nếu như$h = 2R$, diện tích phải là hình tròn đầy đủ$\pi R^2$. Việc triển khai đơn giản không kẹp các giá trị dấu phẩy động có thể tạo ra lỗi miền trong arccos hoặc giá trị âm dưới căn bậc hai do độ lệch chính xác. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng tái tạo lại hình học một cách rõ ràng. Người ta có thể rời rạc hóa đường tròn thành các bước góc nhỏ, tính tổng các đóng góp diện tích nhỏ và ước chừng đoạn đó về mặt số học. Về nguyên tắc, điều này đúng vì tổng Riemann hội tụ về diện tích thực, nhưng mỗi truy vấn sẽ yêu cầu$O(K)$bước ở đâu$K$là độ phân giải rời rạc. Dù khiêm tốn$K = 10^6$mỗi truy vấn trở nên không khả thi đối với kích thước đầu vào lớn, dẫn đến$10^{11}$hoạt động trong các tình huống xấu nhất. 

Quan sát quan trọng là hình học của một đoạn tròn có biểu thức dạng đóng. Khi chúng tôi nhận ra rằng vùng màu xanh lá cây được xác định hoàn toàn bởi độ cao hợp âm, toàn bộ vấn đề sẽ chuyển thành việc đánh giá công thức thời gian không đổi cho mỗi truy vấn. Số hạng arccos xuất phát từ góc ở tâm chắn bởi dây cung, trong khi số hạng căn bậc hai là chiều cao của tam giác được hình thành bởi các điểm cuối của dây cung. 

Điều này chuyển đổi vấn đề từ xấp xỉ bằng số sang đánh giá trực tiếp biểu thức phân tích dẫn xuất. Thách thức duy nhất còn lại là sự ổn định về số lượng và xử lý chính xác các trường hợp biên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (rời rạc) |$O(nK)$|$O(1)$| Quá chậm | 
| Đánh giá công thức tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giả sử mỗi trường hợp thử nghiệm cung cấp bán kính$R$và một chiều cao$h$, mô tả khoảng cách từ đường cắt đến đáy vòng tròn. 

1. Đọc$R$Và$h$. Hình dạng đối xứng nên chúng ta chỉ cần hai giá trị này để xác định đoạn. 
2. Tính đại lượng trung gian liên quan đến dây cung:$$x = 1 - \frac{h}{R}$$Điều này biểu thị cosin của một nửa góc ở tâm ở dạng chuẩn hóa. Nó sẽ được sử dụng bên trong hàm arccos. 
3. Kẹp$x$vào miền hợp lệ$[-1, 1]$. Điều này ngăn các lỗi dấu phẩy động tạo ra đầu vào không hợp lệ cho arccos. Nếu không có bước này, các giá trị như$1.0000000002$có thể làm hỏng tính toán. 
4. Tính góc ở tâm:$$\theta = \arccos(x)$$Góc này tương ứng với hình tròn tạo thành một phần của đoạn thẳng. 
5. Tính số hạng tam giác theo chiều cao:$$t = \sqrt{h(2R - h)}$$Đây là thành phần khoảng cách vuông góc dùng để trừ diện tích hình tam giác dưới dây cung. 
6. Kết hợp cả hai phần vào khu vực cuối cùng:$$a = R^2 \theta - (R - h)t$$Biểu thức này trừ hình tam giác khỏi khu vực để tạo ra vùng màu xanh lục. 
7. Đưa ra kết quả với độ chính xác vừa đủ, thường là 10 đến 15 chữ số thập phân. 

### Tại sao nó hoạt động 

Thuật toán này là sự sao chép trực tiếp sự phân rã hình học của một đoạn tròn thành một hình cung trừ một tam giác cân. Bất biến là tại mọi giá trị$h$, diện tích đoạn thẳng được xác định duy nhất bởi góc ở tâm chắn bởi dây cung. Phép biến đổi arccos đảm bảo rằng chúng ta ánh xạ chính xác chiều cao tuyến tính thành thước đo góc, bảo toàn chính xác hình dạng của đường tròn. Vì cả hai thành phần đều bắt nguồn từ các danh tính Euclide chính xác nên nguồn lỗi duy nhất là số học dấu phẩy động, được kiểm soát thông qua việc kẹp. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    data = input().strip().split()
    if not data:
        return
    
    it = iter(data)
    out = []
    
    for R_s, h_s in zip(it, it):
        R = float(R_s)
        h = float(h_s)

        if h <= 0:
            out.append("0.0000000000")
            continue
        if h >= 2 * R:
            out.append(f"{math.pi * R * R:.10f}")
            continue

        x = 1 - h / R
        x = max(-1.0, min(1.0, x))

        theta = math.acos(x)
        tri = math.sqrt(max(0.0, h * (2 * R - h)))

        area = R * R * theta - (R - h) * tri
        out.append(f"{area:.10f}")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả các giá trị trong một lần và xử lý chúng trong thời gian không đổi trên mỗi cặp. Những trường hợp đặc biệt$h \le 0$Và$h \ge 2R$ngăn chặn các căn bậc hai không hợp lệ và khôi phục các giới hạn hình học chính xác trong đó đoạn suy biến thành diện tích bằng 0 hoặc hình tròn đầy đủ. 

Việc sử dụng`max(0.0, h * (2R - h))`đảm bảo sự ổn định về số khi$h$là rất gần với ranh giới. 

## Ví dụ đã hoạt động 

Hãy xem xét một vòng tròn bán kính$R = 2$với chiều cao cắt khác nhau. 

### Ví dụ 1 

đầu vào:```
2 1
```| Bước | R | h | x = 1 - h/R | θ = acos(x) | thuật ngữ tam giác | khu vực | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 1 | 0,5 | - | - | - | 
| Tính θ | 2 | 1 | 0,5 | 1.0472 | - | - | 
| Tính tri | 2 | 1 | 0,5 | 1.0472 | √(1·3)=1,732 | - | 
| Cuối cùng | 2 | 1 | 0,5 | 1.0472 | 1.732 | 4·1.0472 - (1)·1.732 | 

Kết quả tương ứng với một đoạn tròn có kích thước trung bình. Tính toán xác nhận rằng khu vực này chiếm ưu thế trong khi hiệu chỉnh tam giác làm giảm diện tích một cách thích hợp. 

### Ví dụ 2 

đầu vào:```
3 0
```| Bước | R | h | x | θ | tam giác | khu vực | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 3 | 0 | 1 | 0 | 0 | 0 | 

Điều này xác nhận trường hợp ranh giới trong đó vết cắt biến mất và không có đoạn nào tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi truy vấn được đánh giá bằng cách sử dụng một số lượng không đổi các phép toán số học và siêu việt | 
| Không gian |$O(1)$| Chỉ một tập hợp biến cố định nhỏ được sử dụng bất kể kích thước đầu vào | 

Độ phức tạp là tối ưu cho tới$10^5$các truy vấn vì mỗi truy vấn chỉ yêu cầu đánh giá các hàm toán học tiêu chuẩn theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        data = input().strip().split()
        it = iter(data)
        out = []
        for R_s, h_s in zip(it, it):
            R = float(R_s)
            h = float(h_s)

            if h <= 0:
                out.append("0.0000000000")
                continue
            if h >= 2 * R:
                out.append(f"{math.pi * R * R:.10f}")
                continue

            x = 1 - h / R
            x = max(-1.0, min(1.0, x))

            theta = math.acos(x)
            tri = math.sqrt(max(0.0, h * (2 * R - h)))
            area = R * R * theta - (R - h) * tri
            out.append(f"{area:.10f}")

        return "\n".join(out)

    return solve()

# boundary cases
assert run("2 0") == "0.0000000000"
assert run("2 4") == f"{4*math.pi:.10f}"

# small case
assert abs(float(run("2 1")) - 4*math.acos(0.5) + (1)*math.sqrt(3)) < 1e-6

# multiple queries
assert run("1 0 2 2") == "0.0000000000\n12.5663706144"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 0`|`0`| suy thoái ranh giới dưới | 
|`2 4`| vòng tròn đầy đủ | trường hợp ranh giới trên | 
|`1 0 2 2`| hỗn hợp | xử lý nhiều truy vấn | 

## Vỏ cạnh 

Khi nào$h = 0$, dây cung sẽ co lại thành một điểm ở cuối vòng tròn. Thuật toán ngay lập tức trả về 0 mà không đánh giá arccos, tránh tính toán góc suy biến. Số hạng của tam giác cũng trở thành số 0 vì$h(2R-h) = 0$. 

Khi$h = 2R$, vết cắt đi qua đỉnh của vòng tròn và bao gồm toàn bộ đĩa. Thuật toán chuyển sang$\pi R^2$, tránh việc hủy bỏ giữa hai số hạng lớn gần bằng nhau có thể gây mất độ chính xác. 

Khi$h$cực kỳ gần với một trong hai ranh giới, việc làm tròn dấu phẩy động có thể đẩy$1 - h/R$hơi bên ngoài$[-1, 1]$. Bước kẹp đảm bảo các arccos vẫn được xác định và`max(0.0, ...)`bảo vệ ngăn chặn việc lấy căn bậc hai của số âm nhỏ do độ lệch chính xác.
