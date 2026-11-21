---
title: "CF 231D - Chiếc Hộp Thần Kỳ"
description: "Chúng ta được yêu cầu tính tổng các số nhìn thấy được trên một hộp từ một góc nhìn nhất định trong không gian ba chiều. Hộp được căn chỉnh theo trục, nghĩa là tất cả các cạnh chạy dọc theo trục X, Y và Z. Góc tối thiểu của nó nằm ở gốc tọa độ và góc đối diện nằm ở tọa độ $(x1, y1, z1)$."
date: "2026-06-04T09:11:58+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "geometry"]
categories: ["algorithms"]
codeforces_contest: 231
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 143 (Div. 2)"
rating: 1600
weight: 231
solve_time_s: 89
verified: true
draft: false
---

[CF 231D - Hộp ma thuật](https://codeforces.com/problemset/problem/231/D) 

**Đánh giá:** 1600 
**Tags:** lực lượng vũ phu, hình học 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính tổng các số nhìn thấy được trên một hộp từ một góc nhìn nhất định trong không gian ba chiều. Hộp được căn chỉnh theo trục, nghĩa là tất cả các cạnh chạy dọc theo trục X, Y và Z. Góc nhỏ nhất của nó ở gốc tọa độ, góc đối diện ở tọa độ$(x_1, y_1, z_1)$. Mỗi mặt trong số sáu mặt đều có một số được viết ở giữa. Vị trí của Vasya nằm ngay ngoài vòng cấm$(x, y, z)$. Nhiệm vụ là xác định xem anh ta có thể nhìn thấy số nào trong sáu số và tính tổng chúng. 

Mỗi mặt được liên kết với một mặt phẳng. Đối với hướng X, một mặt nằm ở$x=0$và ngược lại tại$x=x_1$. Tương tự, các mặt hướng Y nằm ở$y=0$Và$y=y_1$và các mặt theo hướng Z nằm ở$z=0$Và$z=z_1$. Một con số chỉ hiển thị nếu góc nhìn của Vasya ở phía bên ngoài khuôn mặt đó. Nếu tọa độ của anh ta dọc theo một chiều bằng tọa độ của khuôn mặt thì anh ta không thể nhìn thấy nó. 

Các ràng buộc cho chúng ta biết rằng tất cả tọa độ và số đều nằm trong$10^6$về giá trị tuyệt đối. Vì chỉ cần sáu lần kiểm tra cho mỗi đầu vào nên giải pháp là thời gian không đổi. Không cần phải lo lắng về hiệu quả thuật toán cho các giới hạn này. 

Trường hợp cạnh không rõ ràng phát sinh khi Vasya được căn chỉnh chính xác với mặt phẳng của hộp nhưng ở bên ngoài. Ví dụ: nếu hộp kéo dài từ 0 đến 1 dọc theo tất cả các trục và Vasya ở (2,0,0), anh ấy ở bên ngoài nhưng tọa độ y của anh ấy bằng mặt y=0. Anh ta sẽ không nhìn thấy mặt tại y=0 nhưng sẽ nhìn thấy các mặt khác dọc theo hướng x và z. Việc triển khai bất cẩn có thể cho rằng anh ta nhìn thấy một khuôn mặt một cách không chính xác bất cứ khi nào anh ta ở bên ngoài trục tương ứng mà không xử lý bình đẳng một cách chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ về mặt khái niệm sẽ mô phỏng góc nhìn ở dạng 3D, dò tia về phía tâm hộp hoặc kiểm tra độ che phủ trên mỗi mặt. Điều này sẽ liên quan đến rất nhiều tính toán hình học, số học dấu phẩy động và có khả năng$O(n)$trên mỗi tia nếu rời rạc. Đối với một hộp duy nhất, điều này là quá mức cần thiết nhưng nó sẽ có tỷ lệ kém nếu được mở rộng sang nhiều hộp hoặc kiểm tra mức độ hiển thị chi tiết hơn. 

Thông tin chi tiết quan trọng là hộp được căn chỉnh theo trục và hình chữ nhật. Điều này cho phép chúng tôi xử lý khả năng hiển thị dọc theo từng trục một cách độc lập. Dọc theo X, nếu tọa độ x của Vasya lớn hơn x1, anh ta sẽ nhìn thấy khuôn mặt tại x=x1; nếu nhỏ hơn 0 thì thấy x=0. Anh ta không thể nhìn thấy cả hai khuôn mặt dọc theo cùng một trục. Việc áp dụng điều này cho mỗi trục sẽ cho chúng ta một quy tắc đơn giản: kiểm tra từng chiều riêng biệt và thêm số lượng khuôn mặt nhìn thấy được nếu Vasya ở phía bên ngoài. Điều này làm giảm vấn đề xuống còn nhiều nhất là ba phép so sánh và ba phép cộng. 

Cách tiếp cận vũ phu là đúng về mặt khái niệm nhưng phức tạp không cần thiết. Việc quan sát theo trục làm giảm nó thành tính toán theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng tia 3D Brute Force | O(1) cho hộp đơn, cao hơn cho trường hợp chung | O(1) | Quá mức cần thiết | 
| So sánh theo trục | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tọa độ của Vasya$(x, y, z)$, góc xa của hộp$(x_1, y_1, z_1)$, và số mặt$a_1$bởi vì$a_6$. Điều này thiết lập các đầu vào. 
2. Khởi tạo một biến`total`về không. Điều này sẽ tích lũy tổng số nhìn thấy được. 
3. Xác định tầm nhìn dọc theo trục X. Nếu như$x > x_1$, Vasya ở bên phải hộp và có thể nhìn thấy mặt tại x=x1 (số a2). Nếu như$x < 0$, anh ta ở bên trái và có thể nhìn thấy mặt tại x=0 (số a1). Sự bình đẳng bị bỏ qua vì anh ta không thể nhìn thấy chính xác các khuôn mặt trên máy bay của mình. 
4. Áp dụng logic tương tự dọc theo trục Y. Nếu như$y > y_1$, anh ta nhìn thấy mặt xa y=y1 (số a4). Nếu như$y < 0$, anh ta thấy y=0 (số a3). 
5. Áp dụng logic tương tự dọc theo trục Z. Nếu như$z > z_1$, anh ta nhìn thấy mặt trên z=z1 (số a6). Nếu như$z < 0$, anh ta nhìn thấy mặt dưới z=0 (số a5). 
6. Tính tổng các số được xác định là hiển thị và in kết quả. 

Tại sao nó hoạt động: Mỗi trục có chính xác hai mặt. Vasya chỉ có thể ở một bên của hộp dọc theo mỗi trục, vì vậy anh ta nhìn thấy nhiều nhất một mặt trên mỗi trục. Việc kiểm tra từng trục một cách độc lập đảm bảo rằng tất cả các mặt nhìn thấy được đều được tính và không có mặt nào bị tính hai lần hoặc bị bỏ sót. Điều bất biến là phép so sánh tọa độ ánh xạ chính xác tới khả năng hiển thị và đẳng thức đó được coi là không nhìn thấy được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x, y, z = map(int, input().split())
x1, y1, z1 = map(int, input().split())
a1, a2, a3, a4, a5, a6 = map(int, input().split())

total = 0

# X-axis visibility
if x < 0:
    total += a1
elif x > x1:
    total += a2

# Y-axis visibility
if y < 0:
    total += a3
elif y > y1:
    total += a4

# Z-axis visibility
if z < 0:
    total += a5
elif z > z1:
    total += a6

print(total)
```Giải pháp trực tiếp thực hiện chiến lược so sánh theo trục. Phần trục X xác định chính xác mặt trái và mặt phải, phần trục Y xử lý mặt trước và mặt sau và phần trục Z xử lý mặt dưới và mặt trên. Có thể tránh được các lỗi từng cái một bằng cách sử dụng nghiêm ngặt`<`Và`>`so sánh, khớp yêu cầu bài toán rằng bình đẳng nghĩa là không nhìn thấy được khuôn mặt. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
2 2 2
1 1 1
1 2 3 4 5 6
```| Biến | Giá trị | Lý luận | 
| --- | --- | --- | 
| x > x1 | 2>1 | Đúng, thêm a2=2 | 
| y > y1 | 2>1 | Đúng, thêm a4=4 | 
| z > z1 | 2>1 | Đúng, thêm a6=6 | 
| tổng cộng | 12 | tổng số khuôn mặt nhìn thấy được | 

Điều này cho thấy rằng khi Vasya ở bên ngoài trong tất cả các trục dương, anh ta nhìn thấy các mặt xa dọc theo mỗi trục. 

**Mẫu 2:** 

đầu vào:```
-1 0 2
1 1 1
1 2 3 4 5 6
```| Biến | Giá trị | Lý luận | 
| --- | --- | --- | 
| x < 0 | -1<0 | Đúng, thêm a1=1 | 
| y < 0 | 0<0 | Sai, không thêm gì | 
| z > z1 | 2>1 | Đúng, thêm a6=6 | 
| tổng cộng | 7 | tổng số khuôn mặt nhìn thấy được | 

Điều này thể hiện việc xử lý đẳng thức: y=0 nằm trên mặt phẳng của a3, vì vậy nó không nhìn thấy được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có sáu so sánh và nhiều nhất là ba bổ sung | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Với những hạn chế của vấn đề, giải pháp này hoàn thành gần như ngay lập tức đối với bất kỳ đầu vào nào được phép. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    x, y, z = map(int, input().split())
    x1, y1, z1 = map(int, input().split())
    a1, a2, a3, a4, a5, a6 = map(int, input().split())
    total = 0
    if x < 0:
        total += a1
    elif x > x1:
        total += a2
    if y < 0:
        total += a3
    elif y > y1:
        total += a4
    if z < 0:
        total += a5
    elif z > z1:
        total += a6
    return str(total)

# provided samples
assert run("2 2 2\n1 1 1\n1 2 3 4 5 6\n") == "12", "sample 1"
assert run("-1 0 2\n1 1 1\n1 2 3 4 5 6\n") == "7", "custom sample"

# minimum size input, all numbers 1
assert run("-1 -1 -1\n1 1 1\n1 1 1 1 1 1\n") == "3", "min size all 1"

# maximum coordinates
assert run("1000000 1000000 1000000\n1 1 1\n1 2 3 4 5 6\n") == "12", "max positive"

# all coordinates negative
assert run("-1000000 -1000000 -1000000\n1 1 1\n1 2 3 4 5 6\n") == "9", "max negative"

# equality with plane
assert run("0 0 2\n1
```
