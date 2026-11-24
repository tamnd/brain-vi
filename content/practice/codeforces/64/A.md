---
title: "CF 64A - Giai thừa"
description: "Chúng ta được yêu cầu tính giai thừa của một số nguyên $n$. Giai thừa là tích của tất cả các số nguyên dương đến $n$, vì vậy với $n = 3$, giai thừa là $1 nhân 2 nhân 3 = 6$. Đầu vào là một số nguyên duy nhất và đầu ra là kết quả số nguyên duy nhất của sản phẩm này."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 64
codeforces_index: "A"
codeforces_contest_name: "Unknown Language Round 1"
rating: 1300
weight: 64
solve_time_s: 73
verified: true
draft: false
---

[CF 64A - Giai thừa](https://codeforces.com/problemset/problem/64/A) 

**Đánh giá:** 1300 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính giai thừa của một số nguyên cho trước$n$. Giai thừa là tích của tất cả các số nguyên dương đến$n$, vì vậy đối với$n = 3$, giai thừa là$1 \times 2 \times 3 = 6$. Đầu vào là một số nguyên duy nhất và đầu ra là kết quả số nguyên duy nhất của sản phẩm này. 

Các ràng buộc rất chặt chẽ:$n$được đảm bảo nằm trong khoảng từ 1 đến 10. Giá trị này cực kỳ nhỏ, giúp đơn giản hóa vấn đề một cách đáng kể. Ngay cả một cách triển khai đơn giản để nhân các số trong một vòng lặp cũng nhanh một cách tầm thường, vì tính toán$10!$chỉ liên quan đến 10 phép nhân, thấp hơn nhiều so với bất kỳ mối quan tâm nào về hiệu suất. Việc sử dụng bộ nhớ là không đáng kể vì giai thừa của 10 dễ dàng khớp với số nguyên 32 bit tiêu chuẩn. 

Mặc dù đơn giản nhưng chúng ta nên cẩn thận với các trường hợp biên. Đầu vào nhỏ nhất là$n = 1$, sẽ trả về 1. Bất kỳ cách triển khai nào khởi tạo biến sản phẩm không chính xác hoặc bắt đầu vòng lặp ở 2 thay vì 1 đều có thể tính toán sai giá trị này. Tương tự, đầu vào lớn nhất là$n = 10$, đó là$3628800$. Giá trị này đủ nhỏ để chứa các số nguyên tiêu chuẩn, nhưng nếu vấn đề được mở rộng, việc triển khai đơn giản có thể bị tràn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực cũng là cách tiếp cận tối ưu ở đây do phạm vi đầu vào nhỏ. Nó bao gồm việc khởi tạo một biến để giữ tích, sau đó nhân lặp lại nó với mỗi số nguyên từ 1 đến$n$. Điều này hiệu quả vì phép nhân có tính kết hợp và giao hoán, vì vậy chúng ta có thể xây dựng sản phẩm theo bất kỳ thứ tự nào một cách an toàn. 

Ở đây không cần ghi nhớ hoặc đệ quy vì số lượng quá nhỏ nên độ sâu ngăn xếp cũng như tính toán lặp lại đều không thành vấn đề. Người ta cũng có thể thực hiện đệ quy giai thừa, xác định$factorial(n) = n \times factorial(n-1)$với trường hợp cơ sở$factorial(1) = 1$. Cả hai phương pháp lặp và đệ quy đều tính toán kết quả giống hệt nhau, nhưng phương pháp lặp tránh được chi phí nhỏ của các lệnh gọi đệ quy, điều này không đáng kể đối với những số lượng nhỏ như vậy nhưng vẫn là một cách thực hành tốt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lặp đi lặp lại | O(n) | O(1) | Đã chấp nhận | 
| Đệ quy | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên đầu vào$n$. Đây là số mà chúng ta cần tính giai thừa. 
2. Khởi tạo một biến`fact`đến 1. Điều này sẽ giữ sản phẩm tích lũy. Bắt đầu từ 1 đảm bảo xử lý chính xác cho$n = 1$. 
3. Lặp lại số nguyên$i$từ 2 đến$n$bao gồm. nhân`fact`qua$i$ở mỗi lần lặp. Bắt đầu từ 2 là một sự tối ưu hóa vì nhân với 1 không làm thay đổi kết quả. 
4. Sau khi vòng lặp kết thúc,`fact`chứa giai thừa của$n$. In giá trị. 

Tại sao nó hoạt động: Mỗi lần lặp lại nhân lên`fact`bởi số nguyên tiếp theo trong dãy. Đến cuối vòng lặp,`fact`chính xác là$1 \times 2 \times \dots \times n$. Bất biến là sau khi xử lý số nguyên$i$,`fact`bằng giai thừa của$i$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
fact = 1
for i in range(2, n + 1):
    fact *= i
print(fact)
```Chúng tôi đọc số nguyên bằng cách sử dụng I/O nhanh để phù hợp với các quy ước lập trình cạnh tranh, mặc dù đối với đầu vào nhỏ như vậy, tiêu chuẩn`input()`cũng ổn. Chúng ta bắt đầu vòng lặp ở số 2 để tránh phép nhân không cần thiết với 1. Bản in cuối cùng sẽ xuất ra giai thừa. Không cần xử lý đặc biệt vì số nguyên Python có thể tự động tăng vượt quá giới hạn 32 bit. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3
```| tôi | sự thật trước đây | sự thật sau | 
| --- | --- | --- | 
| 2 | 1 | 2 | 
| 3 | 2 | 6 | 

Đầu ra:`6`Điều này chứng tỏ rằng tích lũy tính toán giai thừa một cách chính xác, bắt đầu bằng 1 và nhân lên đến 3. 

Đầu vào mẫu 2:```
5
```| tôi | sự thật trước đây | sự thật sau | 
| --- | --- | --- | 
| 2 | 1 | 2 | 
| 3 | 2 | 6 | 
| 4 | 6 | 24 | 
| 5 | 24 | 120 | 

Đầu ra:`120`Điều này cho thấy rằng khi chúng ta tăng$n$, thuật toán chia tỷ lệ tuyến tính và duy trì tính bất biến:`fact`luôn bằng giai thừa của dòng điện`i`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi thực hiện một phép nhân cho mỗi số nguyên từ 2 đến n. Vì$n = 10$, đây là 9 phép nhân. | 
| Không gian | O(1) | Chúng tôi chỉ sử dụng một biến số nguyên duy nhất để lưu trữ tích lũy. | 

Được cho$n \le 10$, thuật toán chạy trong thời gian không đáng kể và yêu cầu bộ nhớ tầm thường nên dễ dàng nằm gọn trong các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())
    fact = 1
    for i in range(2, n + 1):
        fact *= i
    return str(fact)

# provided sample
assert run("3\n") == "6", "sample 1"

# minimum input
assert run("1\n") == "1", "minimum n"

# maximum input
assert run("10\n") == "3628800", "maximum n"

# medium input
assert run("4\n") == "24", "4 factorial"

# another input
assert run("7\n") == "5040", "7 factorial"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | đầu vào tối thiểu | 
| 10 | 3628800 | đầu vào tối đa | 
| 4 | 24 | tính toán bình thường | 
| 7 | 5040 | đầu vào tầm trung lớn hơn | 

## Vỏ cạnh 

cho$n = 1$, vòng lặp từ 2 đến$n$không thực hiện, rời đi`fact = 1`. Điều này phù hợp với định nghĩa của$1!$. Vì$n = 10$, vòng lặp thực hiện chính xác 9 phép nhân, dẫn đến`fact = 3628800`, điều này đúng và nằm trong giới hạn số nguyên. Mọi đầu vào khác từ 1 đến 10 đều hoạt động tương tự và không có chế độ lỗi ẩn nào. 

Bài xã luận này đưa ra một lộ trình rõ ràng từ việc hiểu giai thừa, nhận biết các ràng buộc, chọn cách tiếp cận lặp lại và xử lý cẩn thận các trường hợp ranh giới.
