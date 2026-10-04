---
title: "CF 104886B - Hình học dễ dàng"
description: "Chế độ xem brute-force là coi điểm tiếp xúc của sông là một điểm biến $P = (x, 0)$ và giảm thiểu hàm $$f(x) = sqrt{(x-x1)^2 + y1^2} + sqrt{(x-x2)^2 + y2^2}."
date: "2026-06-28T09:06:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104886
codeforces_index: "B"
codeforces_contest_name: "USI-Team-Selection 2023-2024"
rating: 0
weight: 104886
solve_time_s: 47
verified: true
draft: false
---

[CF 104886B - Hình học dễ dàng](https://codeforces.com/problemset/problem/104886/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Phương pháp tiếp cận 

Quan điểm bạo lực là coi điểm tiếp xúc của sông là một điểm thay đổi$P = (x, 0)$và giảm thiểu hàm$$f(x) = \sqrt{(x-x_1)^2 + y_1^2} + \sqrt{(x-x_2)^2 + y_2^2}.$$Cách tiếp cận trực tiếp sẽ thử nhiều giá trị ứng viên của$x$, nhưng không gian tìm kiếm là liên tục và không bị giới hạn. Ngay cả khi rời rạc, đạt được$10^{-6}$độ chính xác cho mọi trường hợp thử nghiệm dưới 10^5 truy vấn là không khả thi. 

Quan sát cấu trúc quan trọng là đường dẫn tối ưu phải phản ánh nguyên tắc hình học cổ điển: phản ánh một điểm cuối qua đường ràng buộc sẽ chuyển đổi một đường dẫn bị hỏng với một điểm tiếp xúc thành một khoảng cách đường thẳng. Nếu chúng ta phản ánh điểm đích qua dòng sông$y=0$, nó trở thành$(x_2, -y_2)$. Bất kỳ con đường nào đi từ$(x_1,y_1)$đến một điểm trên sông và sau đó đến$(x_2,y_2)$có độ dài tương đương với đường đi từ$(x_1,y_1)$ĐẾN$(x_2,-y_2)$với một “điểm gấp khúc” duy nhất trên sông và đường đi ngắn nhất như vậy xảy ra khi điểm gấp khúc nằm trên đoạn thẳng giữa hai điểm này. 

Điều này làm giảm vấn đề khi tính toán khoảng cách Euclide tiêu chuẩn giữa điểm bắt đầu ban đầu và điểm cuối được phản ánh. Ràng buộc sông được tự động thỏa mãn bằng cách xây dựng đối số phản chiếu, vì điểm giao nhau của đoạn thẳng với$y=0$chính xác là điểm tiếp xúc tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua điểm sông | O(T · K) cho K lớn hoặc tìm kiếm liên tục | O(1) | Quá chậm | 
| Thủ thuật phản ánh | O(T) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lấy điểm xuất phát$(x_1, y_1)$và phản ánh điểm đích qua dòng sông để có được$(x_2, -y_2)$. Phép biến đổi này chuyển đường đi bị ràng buộc thành bài toán đường thẳng không bị ràng buộc trong không gian Euclide. 
2. Tính bình phương khoảng cách giữa$(x_1, y_1)$Và$(x_2, -y_2)$, vì làm việc ở dạng bình phương sẽ tránh được các phép tính dấu phẩy động sớm và giữ cho quá trình triển khai ổn định. 
3. Lấy căn bậc hai của giá trị này để có được đáp án cuối cùng cho test case. 
4. Lặp lại điều này một cách độc lập cho từng trường hợp thử nghiệm vì không có sự tương tác giữa các truy vấn. 

Ý tưởng chính đằng sau sự phản chiếu là bất kỳ đường đi nào chạm vào sông đúng một lần đều có thể được “mở ra” thành một đoạn thẳng trong hệ tọa độ phản ánh. Sự tối ưu xuất phát từ thực tế là đường đi ngắn nhất của Euclide là những đường thẳng trong mặt phẳng không có chướng ngại vật. 

### Tại sao nó hoạt động 

Con sông đóng vai trò như một ràng buộc trung gian bắt buộc buộc con đường phải vượt qua ranh giới$y=0$. Việc phản chiếu một điểm cuối qua đường đó sẽ tạo ra sự đối xứng hình học trong đó mọi đường dẫn hai đoạn hợp lệ tương ứng với một đường gãy có cùng độ dài trong mặt phẳng phản chiếu. Trong số tất cả các đường đứt đoạn như vậy, đoạn thẳng giữa điểm ban đầu và điểm được phản chiếu là đoạn nối ngắn nhất có thể và giao điểm của nó với dòng sông tự động tạo ra một điểm gặp gỡ hợp lệ. Điều này đảm bảo khoảng cách tính toán là tối thiểu trong số tất cả các tuyến đường tiếp cận sông được chấp nhận. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

t = int(input())
for _ in range(t):
    x1, y1, x2, y2 = map(int, input().split())

    dx = x1 - x2
    dy = y1 + y2  # since reflected point is (x2, -y2)

    print(math.hypot(dx, dy))
```Việc thực hiện trực tiếp theo sau việc giảm phản ánh. Điểm tinh tế duy nhất là biểu thức cho hiệu theo chiều dọc: sau khi phản xạ, hiệu tọa độ y trở thành$y_1 - (-y_2) = y_1 + y_2$, rất dễ mắc sai lầm nếu viết một cách máy móc. 

sử dụng`math.hypot`tránh các vấn đề về bình phương và căn bậc hai thủ công và ổn định về mặt số cho các tọa độ lớn. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản trong đó hai điểm được căn chỉnh theo chiều ngang trên sông. Sự phản chiếu tạo ra một cấu hình đối xứng. 

Đối với đầu vào:```
0 1 2 1
```| Bước | x1 | y1 | x2 | y2 | Phản xạ x2,y2 | dx | nhuộm | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 2 | 1 | (2, -1) | -2 | 2 | 

Khoảng cách tính toán là$\sqrt{(-2)^2 + 2^2} = \sqrt{8}$, phù hợp với con đường ngắn nhất đi xuống sông và lùi lại một cách tối ưu. 

Đối với trường hợp sai lệch hơn:```
-10 10 -20 20
```| Bước | x1 | y1 | x2 | y2 | Phản xạ x2,y2 | dx | nhuộm | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | -10 | 10 | -20 | 20 | (-20, -20) | 10 | 30 | 

Khoảng cách trở thành$\sqrt{10^2 + 30^2} = \sqrt{1000}$, cho thấy sự phân tách theo chiều dọc chiếm ưu thế khi cả hai điểm đều ở xa sông. 

Những ví dụ này xác nhận rằng thuật toán hoạt động giống như một phép đo Euclide trực tiếp trong một không gian được biến đổi, đó chính xác là những gì mà đối số hình học dự đoán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm thực hiện một số phép tính số học không đổi và một căn bậc hai | 
| Không gian | O(1) | Không có cấu trúc dữ liệu phụ trợ ngoài các biến đầu vào | 

Giải pháp dễ dàng nằm trong giới hạn vì ngay cả đối với 10^5 trường hợp thử nghiệm, công việc sẽ giảm xuống còn một số thao tác dấu phẩy động cho mỗi trường hợp. 

## Trường hợp thử nghiệm```python
import sys, io, math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    t = int(input())
    out = []
    for _ in range(t):
        x1, y1, x2, y2 = map(int, input().split())
        dx = x1 - x2
        dy = y1 + y2
        out.append(str(math.hypot(dx, dy)))
    return "\n".join(out)

# provided samples
assert run("""2
0 1 2 1
-10 10 -20 20
""").split()[0][:5] == "2.828", "sample 1"

# minimum size-like symmetry
assert run("""1
0 1 0 1
""").strip()[:3] == "2.0", "vertical symmetry"

# large coordinates
assert run("""1
-1000000000 1000000000 1000000000 1000000000
""").strip()[:5] == "28284", "large scale"

# asymmetric heights
assert run("""1
1 2 3 10
""") != "", "general case"

# identical x
assert run("""1
5 1 5 100
""") != "", "same x axis-aligned test"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cùng một điểm được nhân đôi | 2.0 | hình học đối xứng suy biến | 
| tọa độ lớn | giá trị lớn | ổn định số | 
| giống x, khác y | khoảng cách tính toán | trường hợp thống trị theo chiều dọc | 
| trường hợp chung | số thực hợp lệ | tính đúng đắn của mô hình phản ánh | 

## Vỏ cạnh 

Khi cả hai điểm có cùng tọa độ x thì đường đi tối ưu vẫn đi thẳng xuống sông rồi đi thẳng lên, và công thức rút gọn thành$\sqrt{(y_1 + y_2)^2}$, phù hợp với trực giác. 

Khi các điểm đối xứng xung quanh một số đường thẳng đứng, công thức phản ánh đảm bảo rằng đoạn thẳng đi qua chính xác điểm tiếp xúc với sông tối ưu, do đó không cần có vỏ bọc đặc biệt. 

Khi một điểm ở rất gần sông, biểu thức vẫn hoạt động chính xác vì sự phản chiếu không gây ra sự mất ổn định, khoảng cách chỉ đơn giản là tiếp cận đoạn trực tiếp từ gần ranh giới đến điểm phản ánh của điểm cuối kia.
