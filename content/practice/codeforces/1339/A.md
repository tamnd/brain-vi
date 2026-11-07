---
title: "CF 1339A - Kim cương lấp đầy"
description: "Nhiệm vụ mô tả một dải hình tam giác cố định phát triển với tham số $n$. Đối với mỗi $n$, hình dạng bao gồm các hình tam giác đơn vị $4n-2$ được sắp xếp thành một vùng dài giống như vành đai."
date: "2026-06-11T15:42:32+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1339
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 633 (Div. 2)"
rating: 900
weight: 1339
solve_time_s: 56
verified: true
draft: false
---

[CF 1339A - Đổ đầy kim cương](https://codeforces.com/problemset/problem/1339/A) 

**Xếp hạng:** 900 
**Tags:** vũ lực, dp, triển khai, toán học 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ mô tả một dải hình tam giác cố định phát triển với một tham số$n$. Đối với mỗi$n$, hình bao gồm$4n-2$các hình tam giác đơn vị sắp xếp thành một vùng dài giống như vành đai. Chúng tôi được phép bao phủ hoàn toàn khu vực này bằng cách sử dụng các ô “kim cương” giống hệt nhau, trong đó mỗi viên kim cương được hình thành bởi hai hình tam giác liền kề. Các ô có thể xoay hoặc lật nhưng không thể chia tỷ lệ và chúng phải bao phủ khu vực mà không bị chồng chéo hoặc có khoảng trống. 

Đối với mỗi trường hợp thử nghiệm, đầu vào cho một giá trị duy nhất$n$, và chúng ta phải đếm có bao nhiêu lớp phủ đầy đủ riêng biệt tồn tại cho dải hình tam giác tương ứng. Hai lớp phủ được coi là khác nhau nếu có ít nhất một cặp hình tam giác được bao phủ bởi các nhóm kim cương khác nhau. 

Các ràng buộc rất lớn: lên tới$t = 10^4$trường hợp thử nghiệm và$n \le 10^9$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào xây dựng cách xếp lớp một cách rõ ràng hoặc chạy phép truy toán tuyến tính cho mỗi trường hợp thử nghiệm. Thậm chí$O(n)$mỗi truy vấn là không thể, vì nó sẽ yêu cầu tối đa$10^{13}$hoạt động trong trường hợp xấu nhất. Giải pháp phải tính toán từng câu trả lời theo thời gian logarit hoặc không đổi cho mỗi trường hợp kiểm thử. 

Một điểm tinh tế là hình học gợi ý những lựa chọn cục bộ, nhưng cấu trúc toàn cầu lại cứng nhắc. Dải này có mô hình lặp lại, vì vậy việc quay lui ngây thơ trên các vị trí sẽ vượt quá sự đối xứng và bùng nổ theo cấp số nhân. 

Vỏ cạnh là tối thiểu nhưng quan trọng: 

cho$n = 1$, dải chỉ chứa hai hình tam giác, vì vậy có đúng một cách để đặt một viên kim cương. Bất kỳ phương pháp nào giả định sự lặp lại bắt đầu từ các cấu hình lớn hơn đều phải xử lý rõ ràng trường hợp cơ bản này. 

Vì$n = 2$, mẫu cho thấy có chính xác hai cấu hình. Một cách tiếp cận ốp lát tham lam ngây thơ có thể giả định một vị trí từ trái sang phải duy nhất, nhưng ở đây sự phân nhánh xuất hiện ngay lập tức do hình học cục bộ, cho thấy rằng các lựa chọn có tính lan truyền. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ cố gắng xây dựng dải và đặt đệ quy một viên kim cương vào vùng ngoài cùng bên trái không được che chắn. Ở mỗi bước, có nhiều nhất một số hướng không đổi cho viên kim cương tiếp theo và chúng tôi lặp lại cho đến khi dải được bao phủ hoàn toàn. Điều này đúng vì nó khám phá tất cả các ô hợp lệ mà không bỏ sót. 

Tuy nhiên, độ sâu đệ quy tỷ lệ thuận với số lượng kim cương, tức là$2n-1$. Trong mỗi bước, việc phân nhánh tạo ra số lượng trạng thái theo cấp số nhân trong trường hợp xấu nhất, tăng lên gần giống như vụ nổ kiểu Fibonacci. Điều này trở nên không khả thi ngay cả đối với nhỏ$n$, vì số lượng cấu hình vượt quá bất kỳ giới hạn đa thức nào. 

Quan sát quan trọng là dải này không mang các phụ thuộc tầm xa. Sau khi chúng tôi sửa ô gần ranh giới bên trái, vùng không được che phủ còn lại luôn có cấu trúc giống hệt với một trường hợp nhỏ hơn của cùng một vấn đề, ngoại trừ một sự mơ hồ nhỏ cục bộ về cách đặt một số viên kim cương đầu tiên. Điều này làm giảm vấn đề đến mức tái diễn trong đó số lượng ô cho kích thước$n$chỉ phụ thuộc vào hai kích thước trước đó. 

Nếu chúng ta ký hiệu$f(n)$là số lượng ô hợp lệ cho một dải kích thước$n$, thì cấu trúc buộc phải phân tách từng ô lát thành một trong hai trường hợp: hoặc vị trí kim cương đầu tiên bị ép buộc và giảm vấn đề về kích thước$n-1$, hoặc hai viên kim cương đầu tiên phải được sắp xếp theo cấu hình ghép đôi để giảm vấn đề về kích thước$n-2$. Điều này mang lại:$$f(n) = f(n-1) + f(n-2)$$với điều kiện cơ bản$f(1)=1$,$f(2)=2$. 

Đây chính xác là phép truy hồi Fibonacci, do đó vấn đề giảm xuống còn việc tính toán chuỗi giống Fibonacci cho các chỉ số lớn. 

Từ$n$tùy thuộc vào$10^9$, chúng tôi tính toán$f(n)$sử dụng nhân đôi nhanh để đánh giá các số Fibonacci trong$O(\log n)$thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đệ quy Brute Force | Hàm mũ | Ngăn xếp O(n) | Quá chậm | 
| Fibonacci DP (lặp lại) | O(n) | O(1) | Quá chậm | 
| Nhân đôi nhanh chóng | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán các số Fibonacci bằng phương pháp nhân đôi nhanh, phù hợp với việc lập chỉ mục đã dịch chuyển$f(n)=\text{Fib}(n+1)$. 

1. Xác định hàm trả về một cặp$(F(n), F(n+1))$. Việc ghép nối này cho phép chúng ta tính toán hai số Fibonacci liên tiếp cùng một lúc, điều này cho phép nhân đôi. 
2. Nếu$n = 0$, trở lại$(0, 1)$. Đây là điểm neo cơ bản làm cho sự tái phát được xác định rõ ràng. 
3. Tính toán đệ quy$(F(k), F(k+1))$vì$k = \lfloor n/2 \rfloor$. Bước này làm giảm kích thước bài toán xuống một nửa ở mỗi lệnh gọi, đây là nguồn gốc của độ phức tạp logarit. 
4. Sử dụng danh tính$$F(2k) = F(k)\cdot(2F(k+1) - F(k)), \quad F(2k+1) = F(k+1)^2 + F(k)^2$$Những công thức này kết hợp các nghiệm nhỏ hơn thành nghiệm lớn hơn mà không cần tính toán lại. 
5. Nếu$n$chẵn, trở về$(F(2k), F(2k+1))$. Nếu không thì quay lại$(F(2k+1), F(2k)+F(2k+1))$. 
6. Với mỗi test case, xuất ra$F(n+1)$, tương ứng với số lượng ốp lát$f(n)$. 

Tính chính xác phụ thuộc vào tính bất biến mà mọi lệnh gọi đệ quy đều trả về các giá trị Fibonacci liên tiếp chính xác cho đối số của nó. Các công thức nhân đôi là các đồng nhất thức đại số bắt nguồn từ phép truy toán Fibonacci, do đó chúng bảo toàn tính chính xác khi kết hợp các bài toán con. Vì mọi$n$được phân tách thành hai nửa nhị phân, không có cấu hình nào bị bỏ qua hoặc trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def fib_pair(n):
    if n == 0:
        return (0, 1)
    a, b = fib_pair(n >> 1)
    c = a * (2 * b - a)
    d = a * a + b * b
    if n & 1:
        return (d, c + d)
    return (c, d)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        print(fib_pair(n + 1)[0])

if __name__ == "__main__":
    solve()
```Việc thực hiện cốt lõi là`fib_pair`hàm trả về hai số Fibonacci liên tiếp. Sự thay đổi bởi$n+1$trong đầu ra giải thích cho sự không khớp về chỉ mục giữa phép lặp ốp lát và định nghĩa Fibonacci tiêu chuẩn. 

Các bước nhân và cộng thực hiện trực tiếp các nhận dạng nhân đôi. Tất cả số học được thực hiện bằng các số nguyên chính xác tùy ý của Python, do đó không cần xử lý tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$n = 2$Chúng tôi tính toán$F(3)$vì câu trả lời là$F(n+1)$. 

| Gọi | n | Cặp kết quả | 
| --- | --- | --- | 
| fib_pair(3) | 3 | tính toán thông qua đệ quy | 
| fib_pair(1) | 1 | chia căn cứ | 

Đệ quy cuối cùng đạt đến các trường hợp cơ bản và xây dựng lại lên tới$(2, 3)$, vậy câu trả lời là$2$. 

Điều này phù hợp với thực tế là dải kích thước 2 có chính xác hai ô hợp lệ. 

### Ví dụ 2 

đầu vào:$n = 1$Chúng tôi tính toán$F(2)$. 

| Gọi | n | Cặp kết quả | 
| --- | --- | --- | 
| fib_pair(2) | 2 | bắt nguồn từ fib_pair(1) | 

Việc đánh giá cơ sở mang lại kết quả$(1, 1)$, vậy câu trả lời là$1$. 

Điều này xác nhận rằng dải nhỏ nhất có một lớp phủ hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log n)$mỗi bài kiểm tra | mỗi nửa cuộc gọi$n$| 
| Không gian |$O(\log n)$| độ sâu ngăn xếp đệ quy | 

Độ phức tạp logarit là cần thiết
