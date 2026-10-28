---
title: "CF 106170E - Tính khách VIP"
description: "Chúng ta được cung cấp nhiều truy vấn, mỗi truy vấn mô tả một đoạn liên tục các số nhà từ $A$ đến $B$. Mỗi số nhà có một giá trị liên quan gọi là khóa của nó, được định nghĩa là ước số lẻ lớn nhất của số đó."
date: "2026-06-19T18:56:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106170
codeforces_index: "E"
codeforces_contest_name: "Swiss Subregional 2025-2026"
rating: 0
weight: 106170
solve_time_s: 40
verified: true
draft: false
---

[CF 106170E - Tính khách VIP](https://codeforces.com/problemset/problem/106170/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta nhận được nhiều truy vấn, mỗi truy vấn mô tả một đoạn liên tục các số nhà từ$A$ĐẾN$B$. Mỗi số nhà có một giá trị liên quan gọi là khóa của nó, được định nghĩa là ước số lẻ lớn nhất của số đó. Một ngôi nhà được coi là VIP nếu khóa này bằng 1 và nhiệm vụ là đếm xem có bao nhiêu ngôi nhà VIP như vậy tồn tại trong mỗi khoảng thời gian. 

Nhận xét quan trọng về định nghĩa này là việc liên tục loại bỏ các thừa số 2 khỏi một số sẽ để lại ước số lẻ lớn nhất của nó. Do đó, một số có khóa bằng 1 chính xác khi nó không chứa thừa số lẻ nào lớn hơn 1, nghĩa là số đó phải là lũy thừa của hai. Mọi số nguyên khác cuối cùng đều giảm xuống một số lẻ lớn hơn 1 sau khi chia lũy thừa của 2, do đó nó không thể đủ điều kiện. 

Vì vậy, mỗi truy vấn giảm xuống việc đếm xem có bao nhiêu lũy thừa của hai nằm trong phạm vi$[A, B]$. 

Các ràng buộc là rất lớn: lên tới$2 \cdot 10^5$truy vấn và giá trị lên đến$10^{18}$. Điều này ngay lập tức loại trừ việc lặp lại trong khoảng thời gian cho mỗi truy vấn. Ngay cả việc quét tuyến tính trên một phạm vi cũng không thể thực hiện được vì bản thân độ dài khoảng đó có thể$10^{18}$trong trường hợp xấu nhất. 

Các trường hợp cạnh xuất hiện khi ranh giới khoảng rất nhỏ hoặc cực kỳ lớn. Ví dụ, nếu$A = 1, B = 1$, câu trả lời là 1 vì 1 là$2^0$. Nếu như$A = 3, B = 7$, lũy thừa duy nhất của hai là 4, nên đáp án là 1. Một vòng lặp đơn giản từ$A$ĐẾN$B$sẽ hết thời gian ngay cả trên phạm vi nhỏ khi lặp lại trên nhiều truy vấn. 

## Phương pháp tiếp cận 

Một cách tiếp cận vũ phu tuân theo định nghĩa. Đối với mỗi truy vấn, lặp lại từ$A$ĐẾN$B$, tính ước số lẻ lớn nhất của mỗi số bằng cách chia cho 2 cho đến khi nó trở thành số lẻ và kiểm tra xem kết quả có phải là 1 hay không. Cách này hoạt động chính xác nhưng chi phí của nó tỷ lệ thuận với kích thước của khoảng. Trong trường hợp xấu nhất, một truy vấn có thể kéo dài$10^{18}$số lượng, khiến điều này hoàn toàn không thể thực hiện được. 

Cấu trúc của bài toán được đơn giản hóa khi chúng ta nhận ra rằng “ước số lẻ lớn nhất bằng 1” mô tả chính xác lũy thừa của 2. Thay vì kiểm tra từng số, chúng ta chỉ cần đếm xem có bao nhiêu lũy thừa của 2 nằm trong một phạm vi. Quyền hạn của hai rất thưa thớt và tuân theo cấu trúc hàm mũ nghiêm ngặt:$1, 2, 4, 8, 16, \dots$. Điều này cho phép chúng ta thay thế việc quét phạm vi bằng bài toán đếm trên một chuỗi đã được sắp xếp. 

Đối với bất kỳ giá trị$x$, chúng ta có thể đếm được có bao nhiêu lũy thừa của hai$\le x$bằng cách lấy$\lfloor \log_2 x \rfloor + 1$. Mỗi câu trả lời truy vấn sẽ trở thành một sự khác biệt giữa số lượng tiền tố lên tới$B$và lên đến$A-1$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((B-A+1)\log B)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(t)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Chiến lược đếm tối ưu 

1. Quan sát rằng một số có ước số lẻ lớn nhất bằng 1 chỉ khi nó trở thành 1 sau khi chia liên tục cho 2. Điều này có nghĩa là số đó không chứa thừa số lẻ nào lớn hơn 1. 
2. Kết luận rằng các số hợp lệ chính xác là lũy thừa của hai. 
3. Đối với giới hạn trên nhất định$x$, tính xem 2 có bao nhiêu lũy thừa$\le x$. Điều này tương đương với việc tìm số nguyên lớn nhất$k$như vậy$2^k \le x$, sau đó đếm tất cả số mũ từ 0 đến$k$. 
4. Tính toán số đếm này một cách hiệu quả bằng cách sử dụng các phép toán bit hoặc logarit: số mũ là$k = \lfloor \log_2 x \rfloor$. 
5. Đối với mỗi truy vấn$[A, B]$, tính:$$\text{answer} = F(B) - F(A-1)$$Ở đâu$F(x)$là số lũy thừa của hai lên đến$x$. 
6. Xử lý ranh giới cẩn thận khi$A = 1$, từ$A-1 = 0$đóng góp số không hợp lệ. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên sự phân biệt giữa các ngôi nhà hợp lệ và quyền hạn của hai ngôi nhà. Mọi lũy thừa của hai giảm xuống 1 sau khi chia cho 2 và mọi lũy thừa của hai đều giữ lại một thừa số lẻ lớn hơn 1. Vì lũy thừa của hai tăng nghiêm ngặt và tạo thành một chuỗi được xác định rõ ràng được lập chỉ mục theo số mũ, nên việc đếm chúng trong một phạm vi sẽ giảm xuống việc đếm số mũ nguyên trong một khoảng giới hạn. Sự khác biệt về tiền tố sẽ tách biệt chính xác những số mũ có lũy thừa tương ứng nằm bên trong$[A, B]$, không có sự chồng chéo hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_pow2(x):
    if x <= 0:
        return 0
    return x.bit_length()

t = int(input())
out = []

for _ in range(t):
    a, b = map(int, input().split())
    ans = count_pow2(b) - count_pow2(a - 1)
    out.append(str(ans))

print("\n".join(out))
```Chức năng trợ giúp dựa vào thực tế là`x.bit_length()`trả lại$\lfloor \log_2 x \rfloor + 1$, khớp chính xác với số lũy thừa của hai từ$2^0$lên tới$x$. Phép trừ xử lý phạm vi một cách rõ ràng và sử dụng`a - 1`loại trừ chính xác các giá trị bên dưới khoảng. 

Một điểm tinh tế là xử lý$x \le 0$, điều này chỉ cần thiết khi$A = 1$, đảm bảo chúng tôi không tính sai bất kỳ giá trị nào cho đầu vào không dương. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
A = 5, B = 35
```Chúng tôi đánh giá sức mạnh của hai lên đến 35:$1, 2, 4, 8, 16, 32$. 

| Bước | Giá trị | count_pow2(giá trị) | 
| --- | --- | --- | 
| B | 35 | 6 | 
| A-1 | 4 | 3 | 
| Kết quả | | 3 | 

Vậy câu trả lời là$6 - 3 = 3$, tương ứng với$8, 16, 32$. 

Điều này xác nhận rằng sự khác biệt tiền tố chỉ cách ly các lũy thừa trong khoảng. 

### Ví dụ 2 

đầu vào:```
A = 1, B = 1000000
```Sức mạnh của hai lên đến$10^6$đi lên$2^{19} = 524288$. 

| Bước | Giá trị | count_pow2(giá trị) | 
| --- | --- | --- | 
| B | 1000000 | 20 | 
| A-1 | 0 | 0 | 
| Kết quả | | 20 | 

Kết quả đếm tất cả các quyền hạn hợp lệ từ$2^0$bởi vì$2^{19}$. Điều này cho thấy trường hợp ranh giới trong đó giới hạn dưới bao gồm chính 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi truy vấn sử dụng tính toán độ dài bit theo thời gian không đổi | 
| Không gian |$O(1)$| Chỉ một số biến được duy trì bất kể kích thước đầu vào | 

Giải pháp phù hợp thoải mái trong giới hạn vì ngay cả ở$2 \cdot 10^5$các truy vấn, mỗi phép toán là một phép toán số học hoặc bit đơn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    def count_pow2(x):
        if x <= 0:
            return 0
        return x.bit_length()

    t = int(input())
    res = []
    for _ in range(t):
        a, b = map(int, input().split())
        res.append(str(count_pow2(b) - count_pow2(a - 1)))
    return "\n".join(res)

# sample-like tests
assert run("1\n5 35\n") == "3"

# minimum range
assert run("1\n1 1\n") == "1"

# small interval
assert run("1\n3 7\n") == "1"

# full power-of-two chain
assert run("1\n1 16\n") == "5"

# large range edge
assert run("1\n1 1000000000000000000\n") == str(len(bin(10**18)) - 3)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 / 1 1 | 1 | ranh giới phần tử đơn | 
| 3 7 | 1 | tầm trung thưa thớt sức mạnh của hai | 
| 1 16 | 5 | quyền hạn chính xác đến ranh giới | 
| 1 đến lớn | tính toán | căng thẳng về việc xử lý 64-bit | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi$A = 1$. Trong trường hợp này,$A - 1 = 0$và hàm tiền tố phải trả về 0 cho các giá trị không dương. Việc triển khai xử lý việc này một cách rõ ràng bằng cách trả về 0 khi$x \le 0$, đảm bảo không có đóng góp không hợp lệ nào được tính. 

Ví dụ:```
A = 1, B = 2
```Sức mạnh của hai là$1, 2$. Việc tính toán trở thành:$F(2) = 2$,$F(0) = 0$, vậy đáp án là 2 

Trường hợp cạnh thứ hai là khi khoảng không chứa lũy thừa của 2, chẳng hạn như:```
A = 6, B = 7
```Cả hai giá trị đều có ước số lẻ lớn nhất lớn hơn 1, do đó không tồn tại số hợp lệ. Số tiền tố được đánh giá lần lượt là 2 và 2, mang lại kết quả bằng 0 chính xác. 

Những trường hợp này xác nhận rằng việc xử lý vấn đề như đếm tiền tố trên lũy thừa của hai vẫn nhất quán ngay cả ở các ranh giới và giao điểm trống.
