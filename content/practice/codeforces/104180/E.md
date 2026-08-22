---
title: "CF 104180E - Sau giờ học"
description: "Chúng ta có một lưới $n nhân n$ trong đó giá trị trong ô $(i, j)$ được xác định là phép chia số nguyên $leftlfloor frac{j}{i} rightrfloor$. Chỉ số hàng $i$ và chỉ mục cột $j$ đều bắt đầu từ 1. Nhiệm vụ là đếm xem có bao nhiêu ô trong toàn bộ lưới có giá trị là một số nguyên cố định $k$."
date: "2026-07-02T00:43:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104180
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 02-10-23 Div. 2 (Beginner)"
rating: 0
weight: 104180
solve_time_s: 52
verified: true
draft: false
---

[CF 104180E - Sau giờ học](https://codeforces.com/problemset/problem/104180/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$lưới nơi giá trị trong ô$(i, j)$được định nghĩa là phép chia số nguyên$\left\lfloor \frac{j}{i} \right\rfloor$. chỉ mục hàng$i$và chỉ số cột$j$cả hai đều bắt đầu từ 1. Nhiệm vụ là đếm xem có bao nhiêu ô trong toàn bộ lưới có giá trị là một số nguyên cố định$k$. 

Đây không phải là vấn đề về việc xây dựng lưới điện, vì việc xây dựng lưới điện một cách rõ ràng sẽ quá lớn khi$n$tùy thuộc vào$10^5$. Thay vào đó, chúng tôi đang đếm xem có bao nhiêu cặp$(i, j)$thỏa mãn$\left\lfloor \frac{j}{i} \right\rfloor = k$. 

Các ràng buộc ngay lập tức loại trừ mọi nghiệm bậc hai. Một mô phỏng lưới đầy đủ là$O(n^2)$, đó là$10^{10}$hoạt động trong trường hợp xấu nhất và sẽ không chạy trong thời gian giới hạn. Thậm chí lặp lại trên tất cả các cặp$(i, j)$là không thể. 

Trường hợp cạnh khóa nằm ở các giá trị nhỏ của$i$. Đối với một hàng cố định$i$, giá trị của$j$tạo ra một hàm hằng số từng phần của$\left\lfloor \frac{j}{i} \right\rfloor$và việc triển khai đơn giản có thể giả định các thay đổi thường xuyên trên mỗi cột, nhưng trên thực tế, hàm này không đổi trong khoảng thời gian dài. 

Một trường hợp cạnh tinh tế khác là$k = 0$. Điều này tương ứng với tất cả các cặp trong đó$j < i$và vùng này tạo thành một hình tam giác trong lưới. Một cách tiếp cận ngây thơ chỉ xem xét bội số dương của$i$sẽ bỏ lỡ toàn bộ khu vực này. 

## Phương pháp tiếp cận 

Phương pháp brute-force đánh giá trực tiếp từng tế bào, tính toán$\left\lfloor \frac{j}{i} \right\rfloor$, và tăng bộ đếm khi nó bằng$k$. Điều này đúng vì nó tuân theo định nghĩa chính xác. Tuy nhiên, nó đòi hỏi phải lặp lại tất cả$n^2$cặp. Với$n = 100000$, điều này trở thành$10^{10}$đánh giá vượt quá giới hạn cho phép. 

Cấu trúc của hàm$\left\lfloor \frac{j}{i} \right\rfloor$gợi ý một cái nhìn khác. Thay vì quét từng ô, hãy sửa một hàng$i$và hỏi: trong phạm vi nào$j$giá trị có bằng không$k$? Sự bất bình đẳng$$\left\lfloor \frac{j}{i} \right\rfloor = k$$tương đương với$$k \le \frac{j}{i} < k+1$$biến thành$$k \cdot i \le j < (k+1)\cdot i.$$Vì vậy với mỗi hàng$i$, tất cả đều hợp lệ$j$nằm trong một khoảng liền kề. Điều này làm giảm vấn đề đếm các giao điểm số nguyên giữa hai khoảng:$[k i, (k+1)i - 1]$Và$[1, n]$. Mỗi hàng đóng góp 0 hoặc toàn bộ độ dài phân đoạn và điều này có thể được tính toán theo thời gian không đổi trên mỗi hàng. 

Điều này biến vấn đề thành quét tuyến tính trên các hàng thay vì quét bậc hai trên các ô. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Khoảng thời gian mỗi hàng |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sửa chỉ mục hàng$i$và giải thích điều kiện$\left\lfloor \frac{j}{i} \right\rfloor = k$như một hạn chế đối với các chỉ số cột hợp lệ$j$. Điều này chuyển vấn đề từ các ô riêng lẻ sang các phạm vi liền kề. 
2. Viết lại điều kiện sàn dưới dạng bất đẳng thức$k i \le j < (k+1)i$. Điều này xác định chính xác tập hợp các cột trong hàng$i$tạo ra giá trị$k$. 
3. Giao khoảng này với phạm vi lưới hợp lệ$[1, n]$. Phân khúc có thể sử dụng thực tế sẽ trở thành$$L = \max(1, k i), \quad R = \min(n, (k+1)i - 1).$$4. Nếu$L \le R$, thêm vào$R - L + 1$để trả lời. Ngược lại, hàng$i$đóng góp bằng không. Điều này là cần thiết vì các khoảng có thể nằm một phần hoặc toàn bộ bên ngoài giới hạn lưới. 
5. Lặp lại điều này cho tất cả$i$từ 1 đến$n$, tích lũy tổng số. 

### Tại sao nó hoạt động 

Đối với mỗi cố định$i$, hàm$\left\lfloor \frac{j}{i} \right\rfloor$chỉ tăng khi$j$vượt qua bội số của$i$. Trong bất kỳ phạm vi nào mà thương số bằng$k$, mọi số nguyên$j$thỏa mãn các ràng buộc bất đẳng thức tương tự. Vì vậy, tính hợp lệ$j$mỗi hàng phân chia chính xác lưới thành các đoạn rời rạc và mỗi ô được tính chính xác một lần khi và chỉ khi nó thỏa mãn điều kiện sàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    ans = 0

    for i in range(1, n + 1):
        L = k * i
        R = (k + 1) * i - 1

        if R < 1 or L > n:
            continue

        if L < 1:
            L = 1
        if R > n:
            R = n

        if L <= R:
            ans += R - L + 1

    print(ans)

if __name__ == "__main__":
    solve()
```Vòng lặp lõi xử lý từng hàng một cách độc lập. Giới hạn$L = k i$Và$R = (k+1)i - 1$đến trực tiếp từ việc sắp xếp lại tình trạng sàn. Việc cắt chống lại$[1, n]$là cần thiết vì các cột hợp lệ không thể âm hoặc vượt quá$n$. Nếu không có sự điều chỉnh này, các hàng ở đó$k i > n$sẽ đóng góp sai số lượng tích cực. 

điều kiện$R < 1$lọc ra các trường hợp ngay cả giới hạn trên của khoảng nằm ngoài lưới. Điều này đặc biệt có liên quan đối với$k = 0$, Ở đâu$L = 0$và phạm vi hợp lệ bắt đầu từ$j = 1$. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 4, k = 2$Chúng tôi đánh giá từng hàng. 

| tôi | L = 2i | R = 3i-1 | Đã cắt bớt L | Đã cắt bớt R | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 2 | 2 | 1 | 
| 2 | 4 | 5 | 4 | 4 | 1 | 
| 3 | 6 | 8 | - | - | 0 | 
| 4 | 8 | 11 | - | - | 0 | 

Tổng cộng là$1 + 1 = 2$. 

Điều này phù hợp với đầu ra mẫu. Dấu vết xác nhận rằng các ô hợp lệ chỉ xuất hiện ở nơi khoảng giao với phạm vi lưới. 

### Ví dụ 2:$n = 5, k = 1$| tôi | L = tôi | R = 2i-1 | Đã cắt bớt L | Đã cắt bớt R | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 1 | 
| 2 | 2 | 3 | 2 | 3 | 2 | 
| 3 | 3 | 5 | 3 | 5 | 3 | 
| 4 | 4 | 7 | 4 | 5 | 2 | 
| 5 | 5 | 9 | 5 | 5 | 1 | 

Tổng cộng là$1 + 2 + 3 + 2 + 1 = 9$. 

Ví dụ này cho thấy các khoảng chồng lấp tăng tuyến tính như thế nào với$i$, nhưng bị cắt bớt$n$ngăn chặn sự tăng trưởng không giới hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi hàng đóng góp một lượng công việc không đổi thông qua tính toán khoảng thời gian | 
| Không gian |$O(1)$| Chỉ có một vài biến được sử dụng | 

Thuật toán phù hợp thoải mái trong giới hạn vì$n \le 10^5$ngụ ý về$10^5$các lần lặp lại, điều này không quan trọng trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("4 2\n") == "2"

# k = 0 triangular region
assert run("4 0\n") == "6"

# small full grid check
assert run("3 1\n") == "4"

# maximum n, edge sanity (just structure, not exact brute)
assert run("1 0\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 0 | 6 | vùng tam giác với k = 0 | 
| 3 1 | 4 | đếm khoảng thời gian chính xác | 
| 1 0 | 0 | trường hợp biên nhỏ nhất | 

## Vỏ cạnh 

cho$k = 0$, công thức cho$L = 0$Và$R = i - 1$. Sau khi cắt thành$[1, n]$, sự đóng góp trở thành$1$ĐẾN$i-1$, chính xác là đếm tất cả$j < i$. Ví dụ, với$n = 4$, hàng 3 cho$j \in \{1,2\}$, phù hợp với định nghĩa lưới. 

Đối với lớn$k$, cụ thể là khi$k i > n$, khoảng thời gian bắt đầu ngoài lưới và không đóng góp gì. Ví dụ, nếu$n = 5$,$k = 3$, Và$i = 2$, sau đó$L = 6$, nằm ngoài lưới, vì vậy hàng đóng góp bằng không. Điều này ngăn chặn việc đếm quá mức các vùng thương số lớn không thực sự tồn tại trong lưới giới hạn.
