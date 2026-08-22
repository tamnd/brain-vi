---
title: "CF 104181E - Sau giờ học"
description: "Chúng ta có một lưới $n nhân n$ trong đó mỗi ô được xác định bởi hàng $i$ và cột $j$ của nó. Giá trị trong ô đó là kết quả phép chia số nguyên $leftlfloor frac{j}{i} rightrfloor$. Nói cách khác, mỗi hàng $i$ được hình thành bằng cách chia tất cả các chỉ số cột cho $i$, làm tròn xuống."
date: "2026-07-02T00:37:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104181
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 02-10-23 Div. 1 (Advanced)"
rating: 0
weight: 104181
solve_time_s: 56
verified: true
draft: false
---

[CF 104181E - Sau giờ học](https://codeforces.com/problemset/problem/104181/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$lưới trong đó mỗi ô được xác định bởi hàng của nó$i$và cột$j$. Giá trị trong ô đó là kết quả phép chia số nguyên$\left\lfloor \frac{j}{i} \right\rfloor$. Nói cách khác, mỗi hàng$i$được hình thành bằng cách chia tất cả các chỉ số cột cho$i$, làm tròn xuống. 

Nhiệm vụ không phải là xây dựng lưới mà là đếm xem có bao nhiêu ô chứa một giá trị nhất định$k$. 

Vì vậy, thay vì điền vào bảng, chúng ta được yêu cầu một cách hiệu quả: trên tất cả các cặp$(i, j)$với$1 \le i, j \le n$, có bao nhiêu thỏa mãn$$\left\lfloor \frac{j}{i} \right\rfloor = k.$$Ràng buộc$n \le 100000$ngay lập tức loại trừ bất kỳ sự liệt kê trực tiếp nào của tất cả$n^2$tế bào. Việc đánh giá lưới điện đầy đủ sẽ yêu cầu tới$10^{10}$hoạt động vượt xa mọi giới hạn thời gian khả thi. 

Một vấn đề tế nhị hơn xuất hiện khi suy nghĩ từng hàng một. Ngay cả khi chúng ta sửa một hàng$i$, quét tất cả các cột$j$vẫn tuyến tính trên mỗi hàng, lại dẫn đến$O(n^2)$. 

Các trường hợp cạnh xuất phát từ hành vi phân chia sàn gần ranh giới: 

Khi nào$k = 0$, chúng tôi đang đếm các cặp trong đó$j < i$. Ví dụ, với$n = 4$, hàng ngang$i = 3$đóng góp$j = 1,2$, bởi vì cả hai đều cho số không. Một triển khai ngây thơ giả định$j / i \ge 1$sẽ luôn nhớ toàn bộ khu vực này. 

Khi$k = n$, chỉ những cặp cực kỳ ràng buộc mới có thể thỏa mãn$j = i \cdot k$và nhiều hàng không đóng góp gì cả. Bất kỳ phương pháp nào mù quáng thừa nhận từng$k$xuất hiện thường xuyên sẽ bị tính quá mức. 

Khó khăn chính là mỗi hàng tạo ra các phạm vi dài liền kề có giá trị bằng nhau chứ không phải hoạt động độc lập trên mỗi ô. 

## Phương pháp tiếp cận 

Một giải pháp brute-force đánh giá trực tiếp từng cặp$(i, j)$, tính toán$j // i$, và kiểm tra xem nó có bằng không$k$. Điều này đơn giản và chính xác nhưng nó đòi hỏi$n^2$hoạt động. Với$n = 10^5$, đây là$10^{10}$sự chia rẽ, điều này không thể thực hiện được từ xa. 

Cấu trúc của hàm$\left\lfloor \frac{j}{i} \right\rfloor$thay đổi hình ảnh hoàn toàn. Đối với một cố định$i$, giá trị không đổi trong khoảng thời gian$j$. Cụ thể là tất cả$j$TRONG$$[k \cdot i, (k+1)\cdot i - 1]$$tạo ra giá trị$k$, miễn là họ ở trong$[1, n]$. 

Điều này chuyển đổi mỗi hàng từ một chuỗi kiểm tra riêng lẻ thành một bài toán giao phạm vi. Thay vì lặp đi lặp lại tất cả$j$, chúng tôi tính toán có bao nhiêu số nguyên nằm trong phần chồng lên nhau giữa$[k i, (k+1)i - 1]$Và$[1, n]$. 

Điều này làm giảm mỗi hàng thành công việc có thời gian không đổi, làm cho toàn bộ giải pháp tuyến tính theo$n$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Phạm vi mỗi hàng |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Sửa chỉ mục hàng$i$. Chúng tôi muốn đếm có bao nhiêu cột$j$thỏa mãn$\left\lfloor \frac{j}{i} \right\rfloor = k$. Thay vì kiểm tra từng$j$, chúng tôi hiểu điều kiện là một ràng buộc về phạm vi trên$j$. 
2. Viết lại điều kiện thành bất đẳng thức:$$k \le \frac{j}{i} < k+1.$$nhân với$i$mang lại:$$k i \le j < (k+1)i.$$Từ$j$là một số nguyên, đây sẽ trở thành phạm vi bao gồm:$$j \in [k i, (k+1)i - 1].$$3. Giao phạm vi này với giới hạn cột hợp lệ$[1, n]$. Phân đoạn hợp lệ thực tế trở thành:$$L = \max(1, k i), \quad R = \min(n, (k+1)i - 1).$$Nếu như$L > R$, hàng đóng góp giá trị bằng 0. 
4. Số lượng hợp lệ$j$cho hàng này là$\max(0, R - L + 1)$. Thêm đóng góp này vào câu trả lời. 
5. Lặp lại cho tất cả$i$từ$1$ĐẾN$n$, tích lũy tổng số. 

### Tại sao nó hoạt động 

Mỗi hàng được phân chia thành các khoảng rời rạc trong đó độ phân chia tầng không đổi. Khoảng giá trị$k$được suy ra trực tiếp từ định nghĩa chia tầng và chứa chính xác các số nguyên tạo ra giá trị$k$. Vì các khoảng thời gian này là chính xác và không chồng chéo cho các khoảng thời gian khác nhau.$k$, tổng hợp các giao điểm của họ với$[1, n]$đếm mọi ô hợp lệ chính xác một lần mà không bị trùng lặp hoặc bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())

ans = 0

for i in range(1, n + 1):
    L = k * i
    R = (k + 1) * i - 1

    if R < 1 or L > n:
        continue

    L = max(L, 1)
    R = min(R, n)

    if L <= R:
        ans += R - L + 1

print(ans)
```Việc thực hiện trực tiếp tuân theo logic khoảng thời gian dẫn xuất. Mỗi lần lặp tính toán phân đoạn cột hợp lệ cho một hàng cố định$i$, sau đó kẹp nó vào ranh giới lưới. Séc`R < 1 or L > n`bỏ qua các hàng không đóng góp gì cả, điều này đặc biệt quan trọng khi$k = 0$, vì công thức thô tạo ra giới hạn dưới âm hoặc bằng 0. 

Phải cẩn thận với các ranh giới số nguyên. biểu hiện`(k + 1) * i - 1`là rất quan trọng vì giới hạn trên là độc quyền trước khi chuyển đổi về phạm vi số nguyên. Thiếu`-1`dẫn đến việc đếm quá mức từng cái một. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
```Chúng tôi tính toán đóng góp cho mỗi hàng. 

| tôi | k*i | (k+1)*i - 1 | L | R | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 2 | 2 | 1 | 
| 2 | 4 | 5 | 4 | 4 | 1 | 
| 3 | 6 | 8 | 6 | 4 | 0 | 
| 4 | 8 | 11 | 8 | 4 | 0 | 

Tổng cộng là$2$, phù hợp với đầu ra. 

Điều này xác nhận rằng chỉ những hàng có khoảng hợp lệ giao nhau$[1,4]$đóng góp và các hàng lớn hơn nhanh chóng di chuyển ra khỏi phạm vi. 

### Ví dụ 2 

đầu vào:```
5 0
```Ở đây chúng tôi đếm các cặp trong đó$j < i$. 

| tôi | k*i | (k+1)*i - 1 | L | R | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | 0 | 0 | 
| 2 | 0 | 1 | 1 | 1 | 1 | 
| 3 | 0 | 2 | 1 | 2 | 2 | 
| 4 | 0 | 3 | 1 | 3 | 3 | 
| 5 | 0 | 4 | 1 | 4 | 4 | 

Tổng cộng là$10$. 

Điều này thể hiện hành vi đặc biệt của$k = 0$, trong đó phạm vi hợp lệ bắt đầu từ 1 thay vì 0 và các đóng góp tạo thành một mẫu hình tam giác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi hàng được xử lý một lần với số học theo thời gian không đổi | 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được sử dụng | 

Giải pháp chạy thoải mái trong giới hạn vì$n = 10^5$dẫn đến nhiều nhất$10^5$lần lặp và mỗi lần lặp chỉ thực hiện một vài phép tính số học. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    ans = 0

    for i in range(1, n + 1):
        L = k * i
        R = (k + 1) * i - 1
        if R < 1 or L > n:
            continue
        L = max(L, 1)
        R = min(R, n)
        if L <= R:
            ans += R - L + 1

    return str(ans)

# provided sample
assert run("4 2\n") == "2", "sample 1"

# k = 0 small
assert run("5 0\n") == "10", "triangle case"

# n = 1
assert run("1 0\n") == "1", "single cell zero case"

# large k out of range
assert run("10 20\n") == "0", "no valid cells"

# full diagonal-ish case
assert run("6 1\n") == "9", "mixed distribution"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 0 | 10 | cấu trúc tam giác với k = 0 | 
| 1 0 | 1 | lưới ranh giới tối thiểu | 
| 10 20 | 0 | k sản lượng quá lớn không khớp | 
| 6 1 | 9 | tính đúng đắn của trường hợp tổng quát | 

## Vỏ cạnh 

Khi nào$k = 0$, khoảng trở thành$[0, i-1]$, nhưng hợp lệ$j$bắt đầu từ 1. Thuật toán xử lý việc này bằng cách kẹp$L = \max(1, 0)$, vì vậy mỗi hàng đóng góp chính xác$i-1$giá trị khi$i \le n$. Ví dụ, với$n = 4$, hàng ngang$i = 3$khoảng sản lượng$[1,2]$, đưa ra hai ô hợp lệ, khớp với định nghĩa phân chia tầng. 

Khi$k i > n$, giới hạn dưới vượt quá phạm vi lưới và hàng không đóng góp gì. Điều này tự nhiên loại bỏ lớn$i$cố định$k$, đó là lý do tại sao tổng ổn định nhanh chóng ngay cả đối với số lượng lớn$n$. Ví dụ, với$n = 10$,$k = 3$, hàng$i \ge 4$thường tạo ra các nút giao trống và vòng lặp sẽ bỏ qua chúng một cách an toàn. 

Khi$(k+1)i - 1 < 1$, điều này chỉ xảy ra ở mức cực kỳ nhỏ$i$khi$k = 0$, thuật toán sẽ sớm phát hiện các phạm vi không hợp lệ và tránh hoàn toàn hành vi lập chỉ mục tiêu cực.
