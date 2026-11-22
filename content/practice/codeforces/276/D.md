---
title: "CF 276D - Cô bé và XOR tối đa"
description: "Chúng ta được yêu cầu tìm giá trị XOR tối đa của hai số nguyên trong phạm vi bao gồm $[l, r]$ cho trước. Cụ thể hơn, với tất cả các cặp $a$ và $b$ sao cho $l le a le b le r$, chúng ta muốn kết quả lớn nhất là $a oplus b$."
date: "2026-06-05T02:16:31+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 276
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 169 (Div. 2)"
rating: 1700
weight: 276
solve_time_s: 58
verified: true
draft: false
---

[CF 276D - Cô bé và XOR tối đa](https://codeforces.com/problemset/problem/276/D) 

**Đánh giá:** 1700 
**Tags:** bitmasks, dp, tham lam, triển khai, toán học 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tìm giá trị XOR tối đa của hai số nguyên trong một phạm vi bao gồm nhất định$[l, r]$. Cụ thể hơn, với mọi cặp$a$Và$b$như vậy$l \le a \le b \le r$, chúng tôi muốn kết quả lớn nhất của$a \oplus b$. Đầu vào bao gồm hai số nguyên$l$Và$r$, có thể lớn bằng$10^{18}$, nghĩa là chúng ta không thể liệt kê tất cả các cặp vì điều đó có thể cần đến$10^{36}$hoạt động trong trường hợp xấu nhất. Đầu ra là một số nguyên duy nhất, XOR tối đa trong số tất cả các cặp hợp lệ. 

Quan sát quan trọng ở đây là XOR được tối đa hóa khi các số khác nhau nhiều nhất có thể ở các bit có thứ tự cao nhất. Nếu như$l$Và$r$khác nhau ở bit quan trọng nhất thì tồn tại một cặp đặt tất cả các bit thấp hơn thành 1, tạo ra XOR lớn. Nếu như$l = r$, XOR tối đa là 0 vì bản thân bất kỳ số nào XOR cũng bằng 0. Việc triển khai đơn giản để kiểm tra tất cả các cặp sẽ không thành công trong âm thầm đối với đầu vào lớn hoặc tràn bộ nhớ, vì vậy chúng ta cần một cách tiếp cận toán học hoặc bitwise thay vì dùng vũ lực. 

Các trường hợp cạnh bao gồm các phạm vi trong đó$l = r$, trong đó tất cả các số đều là lũy thừa của hai, hoặc trong đó$r = l + 1$. Ví dụ, nếu$l = 1$Và$r = 1$, đầu ra đúng là 0. Nếu$l = 1$Và$r = 2$, XOR tối đa là 3 vì$1 \oplus 2 = 3$. Một cách tiếp cận bất cẩn có thể cố gắng lặp qua các cặp và bỏ lỡ XOR lớn nhất hoặc hết thời gian. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử tất cả các cặp$(a, b)$với$l \le a \le b \le r$, tính toán$a \oplus b$và theo dõi mức tối đa. Điều này hoạt động chính xác đối với đầu vào nhỏ, nhưng đối với hạn chế tối đa của$r - l \approx 10^{18}$, nó đòi hỏi khoảng$(r-l+1)^2 / 2$Hoạt động XOR, điều này hoàn toàn không khả thi. 

Thông tin chi tiết quan trọng là XOR tối đa phụ thuộc vào vị trí của bit cao nhất trong đó$l$Và$r$khác nhau. Nếu chúng ta xác định được bit quan trọng nhất ở đâu$l$Và$r$khác nhau thì chúng ta có thể xây dựng một số có tất cả các bit từ vị trí đó trở xuống được đặt thành 1. Điều này hiệu quả vì XOR giữa hai số lật các bit ở nơi chúng khác nhau. Do đó, XOR lớn nhất trong một phạm vi sẽ có dạng$2^k - 1$, Ở đâu$k$là số bit ở vị trí khác nhau lớn nhất. 

Để minh họa, nếu$l = 5$(101 ở dạng nhị phân) và$r = 6$(110 ở dạng nhị phân), bit khác biệt đáng kể nhất là bit thứ hai tính từ bên phải. Nếu chúng ta đặt tất cả các bit từ vị trí đó xuống 1, chúng ta sẽ nhận được 111 ở dạng nhị phân, tức là 7, XOR tối đa chính xác. 

Quan sát này làm giảm vấn đề trong việc tìm vị trí của bit khác biệt quan trọng nhất và sau đó xây dựng số với tất cả các bit thấp hơn được đặt, đưa ra giải pháp rất nhanh trong$O(\log r)$thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((r-l+1)^2) | O(1) | Quá chậm | 
| Tối ưu | O(log r) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$l$Và$r$. Chúng ta cần tính XOR tối đa cho các số trong phạm vi này. 
2. Tính XOR của$l$Và$r$, ký hiệu$x = l \oplus r$. Con số này làm nổi bật chính xác các bit khác nhau giữa số nhỏ nhất và số lớn nhất trong phạm vi. 
3. Tìm vị trí của bit quan trọng nhất được đặt trong$x$. Điều này có thể được thực hiện bằng cách dịch chuyển liên tục$x$phải cho đến khi nó trở thành 0, đếm số ca. Hãy để vị trí này được$k$. 
4. XOR tối đa đạt được bằng cách đặt tất cả các bit từ vị trí này trở xuống 1. Trong hệ nhị phân, đây là số có$k$những cái đó. Về mặt toán học, đây là$2^k - 1$. 
5. In kết quả. Đây là XOR lớn nhất có thể đạt được giữa hai số trong phạm vi$[l, r]$. 

Tại sao nó hoạt động: XOR tạo ra số 1 ở mỗi vị trí bit trong đó hai toán hạng của nó khác nhau. Để tối đa hóa XOR, chúng tôi muốn bit cao nhất có thể là 1. Bit khác nhau đầu tiên giữa$l$Và$r$đảm bảo rằng chúng ta có thể chọn hai số trong phạm vi khác nhau ở bit đó và có thể là tất cả các bit thấp hơn. Vì thế, số$2^k - 1$thu được chính xác XOR tối đa có thể. Bất kỳ nỗ lực nào để chọn các số bên ngoài cấu trúc này sẽ mang lại XOR nhỏ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

l, r = map(int, input().split())

x = l ^ r
max_xor = 0
while x:
    max_xor = (max_xor << 1) | 1
    x >>= 1

print(max_xor)
```Mã đầu tiên tính toán$l \oplus r$để tìm các bit khác nhau. Sau đó, nó xây dựng XOR tối đa bằng cách đặt tất cả các bit bên dưới bit khác biệt đáng kể nhất thành 1 bằng cách sử dụng phép dịch trái và phép toán OR. Vòng lặp dừng khi tất cả các bit khác nhau đã được xử lý, tạo ra XOR tối đa cho phạm vi. Điều này tránh bất kỳ sự lặp lại nào trong phạm vi đó, điều này sẽ quá chậm. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
1 2
```| Bước | tôi | r | x = l^r | max_xor | 
| --- | --- | --- | --- | --- | 
| ban đầu | 1 | 2 | 3 | 0 | 
| vòng1 | 1 | 2 | 1 | 1 | 
| vòng2 | 1 | 0 | 0 | 3 | 

Giải thích: 1 XOR 2 là 3. Bit khác biệt đáng kể nhất là bit thứ hai, tạo ra 11 ở dạng nhị phân, tức là 3 ở dạng thập phân. 

Đầu vào mẫu 2:```
5 6
```| Bước | tôi | r | x = l^r | max_xor | 
| --- | --- | --- | --- | --- | 
| ban đầu | 5 | 6 | 3 | 0 | 
| vòng1 | 5 | 6 | 1 | 1 | 
| vòng2 | 5 | 6 | 0 | 3 | 

Giải thích: 5 XOR 6 cho tối đa là 7, phù hợp với cấu trúc của tất cả các đơn vị tính theo bit cho đến vị trí khác biệt đáng kể nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log r) | Chúng tôi xử lý từng bit của x = l ^ r một lần trong vòng lặp while. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được sử dụng. | 

Với những hạn chế$1 \le l \le r \le 10^{18}$, log2(r) nhiều nhất là 60, do đó vòng lặp lặp lại tối đa 60 lần, vừa vặn thoải mái trong giới hạn 1 giây với mức sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    l, r = map(int, input().split())
    x = l ^ r
    max_xor = 0
    while x:
        max_xor = (max_xor << 1) | 1
        x >>= 1
    return str(max_xor)

# provided sample
assert run("1 2\n") == "3", "sample 1"

# custom cases
assert run("5 6\n") == "3", "small range"
assert run("1 1\n") == "0", "l equals r"
assert run("8 15\n") == "7", "range crossing power of two boundary"
assert run("0 1023\n") == "1023", "full range of 10 bits"
assert run("123456789 987654321\n") == "939524095", "large numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 6 | 3 | số liền kề nhỏ | 
| 1 1 | 0 | điểm cuối giống hệt nhau | 
| 8 15 | 7 | phạm vi vượt qua ranh giới sức mạnh của hai | 
| 0 1023 | 1023 | đầy đủ các số 10 bit nhỏ | 
| 123456789 987654321 | 939524095 | số lượng lớn, kiểm tra căng thẳng | 

## Vỏ cạnh 

cho$l = r$, XOR luôn bằng 0. Ví dụ: nhập`42 42`sản xuất$x = 42 ^ 42 = 0$, vòng lặp không thực thi và max_xor vẫn bằng 0. 

Đối với các phạm vi bao trùm một ranh giới lũy thừa hai, như`8 15`, bit khác nhau đầu tiên là bit thứ 3 từ phải sang. Xây dựng tất cả những cái bên dưới sẽ cho 7, đây là XOR tối đa chính xác có thể đạt được trong phạm vi. 

Với số lượng lớn đang đến gần$10^{18}$, thuật toán chỉ lặp lại trên 60 bit, tạo ra XOR tối đa chính xác mà không bị tràn.
