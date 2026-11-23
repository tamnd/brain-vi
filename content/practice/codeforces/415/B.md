---
title: "CF 415B - Mashmokh và Token"
description: "Mỗi ngày Mashmokh nhận được rất nhiều token và vào cuối ngày hôm đó, anh ấy có thể đổi một số token đó lấy tiền bằng cách sử dụng quy tắc chuyển đổi cố định. Nếu anh ta trả lại token $w$, số tiền anh ta nhận được chỉ phụ thuộc vào số lượng “khối” đầy đủ có kích thước $a$ được chứa trong hàm tuyến tính của $w$."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 415
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 240 (Div. 2)"
rating: 1500
weight: 415
solve_time_s: 67
verified: true
draft: false
---

[CF 415B - Mashmokh và Token](https://codeforces.com/problemset/problem/415/B) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi ngày Mashmokh nhận được rất nhiều token và vào cuối ngày hôm đó, anh ấy có thể đổi một số token đó lấy tiền bằng cách sử dụng quy tắc chuyển đổi cố định. Nếu anh ấy quay lại$w$token, số tiền anh ta nhận được chỉ phụ thuộc vào số lượng “khối” đầy đủ$a$được chứa trong một hàm tuyến tính của$w$. Ánh xạ chính xác là tuyến tính và sau đó được thả nổi, vì vậy tiến trình từng phần hướng tới đơn vị tiền tiếp theo là vô giá trị. 

Mục tiêu chính không phải là tối đa hóa mã thông báo được chuyển đổi trực tiếp mà là tối đa hóa số tiền trong khi tiết kiệm càng nhiều mã thông báo càng tốt. Vì các mã thông báo không được sử dụng sẽ không được chuyển sang các ngày trong tương lai nên mỗi ngày là độc lập và chúng tôi chỉ cần quyết định số lượng mã thông báo sẽ trao đổi vào ngày đó để đạt được giá trị tiền tệ tối đa có thể. 

Đầu vào mang lại$n$ngày, thông số$a$Và$b$và một mảng$x_i$mô tả số lượng token có sẵn mỗi ngày. Đầu ra cho mỗi ngày là số lượng token có thể được lưu sau khi chọn số tiền trao đổi tối ưu. 

Ràng buộc$n \le 10^5$buộc một$O(n)$hoặc$O(n \log n)$giải pháp. Bất cứ điều gì liên quan đến mô phỏng mỗi ngày đối với tất cả các lựa chọn mã thông báo có thể có sẽ quá chậm vì$x_i$có thể lớn như$10^9$, làm cho vũ lực không thể thực hiện được. 

Trường hợp cạnh tinh tế xuất hiện khi$x_i$là rất nhỏ so với$a$. Ví dụ, nếu$x_i < a$, tùy thuộc vào công thức, không thể thực hiện trao đổi nào và giả định tham lam ngây thơ như “sử dụng tất cả các mã thông báo” sẽ thất bại. Một trường hợp cạnh khác là khi$x_i$nằm chính xác trên nhiều ranh giới trong đó việc tăng mã thông báo trao đổi không làm tăng tiền, nghĩa là tiết kiệm thêm một mã thông báo luôn là tối ưu. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi số lượng mã thông báo có thể$w$từ$0$ĐẾN$x_i$, tính toán số tiền thu được và chọn số tiền tốt nhất trong khi tối đa hóa số tiền còn lại giữa các mối quan hệ. Điều này đúng nhưng cực kỳ chậm. Đối với mỗi ngày chúng tôi sẽ làm$O(x_i)$làm việc, trong trường hợp xấu nhất sẽ trở thành$O(n \cdot 10^9)$, hoàn toàn không thể. 

Cấu trúc của hàm tiền là quan sát quan trọng. Tiền chỉ phụ thuộc vào$\left\lfloor \frac{a \cdot w}{b} \right\rfloor$, do đó giá trị chỉ tăng ở các ngưỡng rời rạc của$w$. Giữa hai ngưỡng liên tiếp, tăng dần$w$không đổi tiền. Điều này có nghĩa là chúng ta không bao giờ cần xem xét tất cả các giá trị của$w$, chỉ nhỏ nhất$w$đạt được giá trị nguyên tối đa có thể theo ràng buộc$w \le x_i$. 

Đối với một cố định$x_i$, chiến lược tối ưu là tính toán số tiền tối đa có thể đạt được bằng cách sử dụng tất cả các mã thông báo, sau đó tìm số lượng mã thông báo tối thiểu cần thiết để đạt được cùng mức tiền đó. Sự khác biệt giữa$x_i$và yêu cầu tối thiểu đó chính xác là số lượng token có thể được lưu. 

Điều này biến vấn đề thành một phép biến đổi số học đơn giản mỗi ngày. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\sum x_i)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tập trung vào một ngày duy nhất với$x$mã thông báo. 

1. Tính số tiền tối đa có thể đạt được nếu sử dụng tất cả token. Đây là$k = \left\lfloor \frac{a \cdot x}{b} \right\rfloor$. Giá trị này thể hiện mức thưởng số nguyên tốt nhất có thể trong ngày. 
2. Phiên dịch$k$ngược: xác định số lượng mã thông báo nhỏ nhất$w$như vậy$\left\lfloor \frac{a \cdot w}{b} \right\rfloor = k$. Điều này đảm bảo chúng tôi đạt được mức phần thưởng như nhau mà không lãng phí token. 
3. Giải bất đẳng thức$$k \le \frac{a \cdot w}{b} < k+1$$sắp xếp lại để$$\frac{k \cdot b}{a} \le w < \frac{(k+1)\cdot b}{a}$$1. Số nguyên nhỏ nhất$w$thỏa mãn điều này là$$w_{\min} = \left\lceil \frac{k \cdot b}{a} \right\rceil$$1. Số lượng token đã lưu chỉ đơn giản là$x - w_{\min}$. 

Mỗi ngày là độc lập nên chúng tôi lặp lại tính toán này cho tất cả đầu vào. 

### Tại sao nó hoạt động 

Bất biến chính là hàm tiền chia dòng số nguyên thành các khoảng liền kề trong đó mọi giá trị trong khoảng đều tạo ra phần thưởng giống nhau. Trong mỗi khoảng thời gian, việc chi tiêu nhiều token hơn không cải thiện được kết quả. Do đó, chiến lược tối ưu luôn là chọn ranh giới bên trái của khoảng tương ứng với phần thưởng tối đa có thể đạt được theo ràng buộc$w \le x$. Điều này đảm bảo tiết kiệm tối đa vì bất kỳ chi phí lớn hơn$w$sẽ không tăng phần thưởng, và bất kỳ phần thưởng nhỏ hơn$w$sẽ làm giảm nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, a, b = map(int, input().split())
    xs = list(map(int, input().split()))
    
    for x in xs:
        k = (a * x) // b
        w_min = (k * b + a - 1) // a
        print(x - w_min, end=' ')
        
solve()
```Giải pháp tính toán mức thưởng tốt nhất có thể đạt được$k$sử dụng tất cả các token. Sau đó, nó sẽ xây dựng lại số lượng mã thông báo nhỏ nhất cần thiết để đạt được phần thưởng tương tự bằng cách sử dụng thủ thuật chia trần:$(k \cdot b + a - 1) // a$. Trừ cái này khỏi$x$cung cấp số lượng mã thông báo tối đa có thể được lưu một cách an toàn mà không làm giảm tiền. 

Phải cẩn thận với số học số nguyên. phép nhân$a \cdot x$có thể đạt được$10^{18}$, nhưng Python xử lý nó một cách an toàn. Việc phân chia trần nhà phải được thực hiện cẩn thận để tránh sai sót. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 1 4
12 6 11 9 1
```Chúng tôi tính toán mỗi ngày. 

| x | k = tầng(a·x/b) | w_min = ceil(k·b/a) | đã lưu | 
| --- | --- | --- | --- | 
| 12 | 3 | 12 | 0 | 
| 6 | 1 | 4 | 2 | 
| 11 | 2 | 8 | 3 | 
| 9 | 2 | 8 | 1 | 
| 1 | 0 | 0 | 1 | 

Đầu ra:```
0 2 3 1 1
```Điều này xác nhận rằng khoản tiết kiệm đến từ việc cắt giảm từng giá trị xuống số tiền nhỏ nhất để duy trì mức thưởng như cũ. 

### Ví dụ 2 

đầu vào:```
3 2 3
5 4 1
```| x | k = tầng(a·x/b) | w_min | đã lưu | 
| --- | --- | --- | --- | 
| 5 | 3 | 5 | 0 | 
| 4 | 2 | 3 | 1 | 
| 1 | 0 | 0 | 1 | 

Đầu ra:```
0 1 1
```Ví dụ này cho thấy các mức thưởng khác nhau tạo ra độ rộng khoảng khác nhau như thế nào nhưng vẫn áp dụng cùng một nguyên tắc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| mỗi ngày sử dụng các phép tính số học liên tục | 
| Không gian |$O(1)$| chỉ lưu trữ đầu vào và một vài số nguyên | 

Độ phức tạp tuyến tính là đủ cho$n \le 10^5$và tất cả các phép toán đều là số học số nguyên đơn giản, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    n, a, b = map(int, input().split())
    xs = list(map(int, input().split()))
    
    out = []
    for x in xs:
        k = (a * x) // b
        w_min = (k * b + a - 1) // a
        out.append(str(x - w_min))
    return " ".join(out)

# provided sample
assert run("5 1 4\n12 6 11 9 1\n") == "0 2 3 1 1"

# minimum input
assert run("1 1 1\n1\n") == "0"

# small boundary
assert run("3 2 3\n5 4 1\n") == "0 1 1"

# all equal
assert run("4 3 5\n10 10 10 10\n") == "0 0 0 0"

# large values
assert run("2 1000000000 1000000000\n1000000000 999999999\n") == "0 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1/1 | 0 | trường hợp nhỏ nhất | 
| 3 2 3 / 5 4 1 | 0 1 1 | số học biên | 
| tất cả đều bình đẳng | 0 0 0 0 | tính nhất quán | 
| giá trị lớn | 0 0 | an toàn tràn | 

## Vỏ cạnh 

Khi nào$x_i$rất nhỏ, chẳng hạn như$x_i = 1$, mức thưởng được tính toán$k$trở thành số không. Trong trường hợp đó, mức tối thiểu$w$đạt được phần thưởng tương tự cũng bằng 0, vì vậy tất cả các mã thông báo đều có thể được lưu. Thuật toán xử lý chính xác điều này bởi vì$k = 0$dẫn đến$w_{\min} = 0$, tạo ra khoản tiết kiệm đầy đủ. 

Khi$x_i$lớn và gần với ranh giới nơi tăng$w$không thay đổi$\left\lfloor \frac{a w}{b} \right\rfloor$, tính toán$w_{\min}$sụp đổ đến ngưỡng gần nhất. Ví dụ, nếu$a=1, b=4, x=6$, sau đó$k=1$, Và$w_{\min}=4$. Thuật toán lưu chính xác$2$token, phù hợp với thực tế là chi tiêu vượt quá$4$không mang lại lợi ích bổ sung. 

Khi$a$Và$b$bằng nhau thì hàm trở thành$k = x$, Và$w_{\min} = x$, vì vậy không có mã thông báo nào được lưu. Thuật toán tự nhiên giảm xuống mức tiết kiệm bằng 0 vì mỗi mã thông báo đóng góp chính xác một đơn vị phần thưởng.
