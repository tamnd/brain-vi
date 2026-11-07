---
title: "CF 1371A - Cây gậy thần kỳ"
description: "Chúng ta có một bộ que có độ dài từ 1 đến $n$, mỗi que có chiều dài một que. Rocher có thể nối hai cây gậy bất kỳ để tạo thành một cây gậy mới có chiều dài bằng tổng của cả hai cây gậy, loại bỏ các cây gậy ban đầu trong quá trình này."
date: "2026-06-11T11:20:42+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1371
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 654 (Div. 2)"
rating: 800
weight: 1371
solve_time_s: 102
verified: true
draft: false
---

[CF 1371A - Cây gậy ma thuật](https://codeforces.com/problemset/problem/1371/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bộ gậy có độ dài từ 1 đến$n$, mỗi chiều dài một thanh. Rocher có thể nối hai cây gậy bất kỳ để tạo thành một cây gậy mới có chiều dài bằng tổng của cả hai cây gậy, loại bỏ các cây gậy ban đầu trong quá trình này. Mục tiêu là xác định số lượng que tối đa có thể có cùng chiều dài. Đầu vào cung cấp nhiều giá trị của$n$và với mỗi câu hỏi chúng ta phải tính một câu trả lời số nguyên duy nhất. 

Những ràng buộc cho phép$n$lớn như$10^9$với tối đa 1000 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ thuật toán nào mô phỏng các kết nối thanh một cách rõ ràng vì việc mô phỏng như vậy sẽ yêu cầu$O(n)$hoặc$O(n \log n)$hoạt động cho mỗi trường hợp thử nghiệm, có thể dễ dàng vượt quá$10^9$hoạt động tổng thể. 

Các trường hợp cạnh không rõ ràng xảy ra khi$n$là rất nhỏ, chẳng hạn như 1 hoặc 2. Đối với$n = 1$, chỉ có một cây gậy nên câu trả lời hiển nhiên là 1. Vì$n = 2$, các que là 1 và 2. Nối chúng lại sẽ tạo ra một que có chiều dài 3, do đó chỉ có một que có độ dài cho trước; câu trả lời vẫn là 1. Khi nào$n = 3$, các que là 1, 2 và 3. Chúng ta có thể nối 1 và 2 để tạo thành số 3, cho chúng ta hai que có chiều dài 3, do đó câu trả lời trở thành 2. Việc nhận ra mẫu này là rất quan trọng vì nó tiết lộ cấu trúc toán học cho phép chúng ta tính toán câu trả lời trực tiếp mà không cần mô phỏng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các chuỗi kết hợp que có thể có, theo dõi số lượng của từng độ dài que thu được và sau đó xác định tần số tối đa của bất kỳ độ dài nào. Mặc dù đúng nhưng điều này là không khả thi vì đối với$n = 10^9$có$2^{10^9}$các kết hợp có thể, vượt xa giới hạn tính toán. 

Thông tin chi tiết quan trọng xuất phát từ thực tế là chúng ta muốn có càng nhiều que có chiều dài bằng nhau càng tốt và việc kết hợp hai que luôn tạo ra một que lớn hơn. Hãy xem xét tổng số tiền của tất cả các gậy:$S = 1 + 2 + \dots + n = n(n+1)/2$. Để tối đa hóa số lượng que có chiều dài bằng nhau, chúng ta cần độ dài que mục tiêu$L$khoảng một nửa số tiền này hoặc ít hơn, bởi vì nếu$L$quá lớn, chúng ta chỉ có thể làm một cây gậy. Một quan sát khác là nếu chúng ta ghép các que đối xứng từ các đầu đối diện nhau, giống như kết hợp các que nhỏ nhất và lớn nhất liên tục, chúng ta sẽ tự nhiên tạo ra nhiều que có chiều dài gần bằng nhau. Mẫu này dẫn đến một công thức đơn giản: số que tối đa có chiều dài bằng nhau là$\lfloor (n+1)/2 \rfloor$. Những ví dụ nhỏ xác nhận điều này: for$n = 3$,$(3+1)//2 = 2$, phù hợp với câu trả lời được quan sát. Vì$n = 4$,$(4+1)//2 = 2$, cũng phù hợp với sản lượng dự kiến. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Chúng tôi sẽ lặp lại qua từng trường hợp thử nghiệm. 
2. Với mỗi test, hãy đọc số nguyên$n$, số gậy mà Rocher có. 
3. Tính số que tối đa có chiều dài bằng nhau bằng công thức$(n+1)//2$. Điều này hoạt động vì lần đầu tiên$(n+1)//2$Các que có thể được ghép với các que còn lại để tạo ra các bản sao có độ dài và đối với bất kỳ số que lẻ nào, một que có thể không được ghép đôi, điều này không làm giảm số lượng que trùng lặp. 
4. Xuất kết quả cho test case. 

Tại sao nó hoạt động: công thức$(n+1)//2$phát sinh từ việc ghép các que nhỏ nhất và lớn nhất rồi di chuyển vào trong. Mỗi cặp góp phần tạo thành một cây gậy có cùng chiều dài và điều này sẽ tối đa hóa số lượng. Hoạt động không yêu cầu mô phỏng từng kết hợp và công thức số học được rút ra từ bất biến rằng số lượng que bằng nhau tối đa luôn bằng khoảng một nửa số$n$làm tròn lên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    print((n + 1) // 2)
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi$n$, nó tính toán trực tiếp$(n+1)//2$và in kết quả. Việc sử dụng phép chia số nguyên đảm bảo chúng tôi tự động làm tròn xuống, xử lý chính xác cả số lẻ và số chẵn$n$. Không có mảng hoặc vòng lặp$n$là cần thiết, giúp giải pháp nhanh chóng và tiết kiệm bộ nhớ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:$n = 3$| Bước | Hành động | n | Tính toán | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | Đọc n | 3 | | | 
| 2 | Tính toán | 3 | (3+1)//2 | 2 | 
| 3 | Đầu ra | | | 2 | 

Vì$n = 3$, ghép 1+2 tạo ra 3, để lại hai que có chiều dài 3. Công thức dự đoán đúng 2. 

đầu vào:$n = 4$| Bước | Hành động | n | Tính toán | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | Đọc n | 4 | | | 
| 2 | Tính toán | 4 | (4+1)//2 | 2 | 
| 3 | Đầu ra | | | 2 | 

Vì$n = 4$, kết hợp 1+3 và 2+4 sẽ được các que có độ dài 4 và 6. Các que có độ dài tối đa bằng nhau là 2 (độ dài 4), xác nhận công thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ một phép toán số học và đầu ra cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Không có mảng nào được phân bổ; mỗi lần chỉ có một số nguyên được lưu trữ | 

Được cho$t \le 1000$và mỗi thao tác là O(1) thì tổng công không đáng kể so với thời hạn 1s. Việc sử dụng bộ nhớ cũng không đáng kể, chỉ dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        output.append(str((n + 1) // 2))
    return "\n".join(output)

# Provided samples
assert run("4\n1\n2\n3\n4\n") == "1\n1\n2\n2", "sample 1"

# Custom cases
assert run("1\n1\n") == "1", "minimum n"
assert run("1\n2\n") == "1", "small even n"
assert run("1\n10\n") == "5", "even n"
assert run("1\n11\n") == "6", "odd n"
assert run("2\n999999999\n1000000000\n") == "500000000\n500000000", "large n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | trường hợp n cạnh tối thiểu | 
| 2 | 1 | nhỏ thậm chí n | 
| 10 | 5 | thậm chí n lớn hơn 4 | 
| 11 | 6 | lẻ n lớn hơn 4 | 
| 999999999, 1000000000 | 500000000, 500000000 | độ chính xác lớn n | 

## Vỏ cạnh 

cho$n = 1$, kết quả đầu ra của thuật toán$(1+1)//2 = 1$, điều này đúng vì chỉ tồn tại một cây gậy. Vì$n = 2$, đầu ra là$(2+1)//2 = 1$, phản ánh rằng việc nối hai cây gậy chỉ còn lại một cây gậy có độ dài nhất định. Đối với số lẻ lớn$n$giống$999999999$,$(999999999+1)//2 = 500000000$, khớp với số lượng gậy có chiều dài bằng nhau tối đa theo lý thuyết có nguồn gốc từ mẫu ghép nối. Mỗi trường hợp cạnh được xử lý chính xác vì công thức khái quát cho cả trường hợp nhỏ và lớn$n$không phân nhánh.
