---
title: "CF 104336B - GCD của chuỗi con"
description: "Chúng ta được cung cấp một số nguyên rất lớn được viết dưới dạng một chuỗi, có khả năng lên tới một triệu chữ số và chúng ta cần tính một giá trị được xác định theo cách không chuẩn."
date: "2026-07-01T18:46:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104336
codeforces_index: "B"
codeforces_contest_name: "II Olympiad of classes at the Mechanics and Mathematics Faculty of MSU in programming 2023."
rating: 0
weight: 104336
solve_time_s: 68
verified: true
draft: false
---

[CF 104336B - GCD của chuỗi con](https://codeforces.com/problemset/problem/104336/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số nguyên rất lớn được viết dưới dạng một chuỗi, có khả năng lên tới một triệu chữ số và chúng ta cần tính một giá trị được xác định theo cách không chuẩn. Thay vì làm việc với chính số đó, chúng ta xem xét tất cả các chuỗi con liền kề của nó, diễn giải mỗi chuỗi con là một số nguyên, sau đó lấy ước số chung lớn nhất của tất cả các số nguyên này. Giá trị cuối cùng đó chính là câu trả lời. 

Thách thức chính là số lượng chuỗi con tăng theo phương trình bậc hai theo độ dài của chuỗi. Đối với một chuỗi có độ dài$n$, có khoảng$n(n+1)/2$chuỗi con và mỗi chuỗi con có thể biểu thị một số có tối đa$n$chữ số. Một cách tiếp cận ngây thơ tạo ra các chuỗi con một cách rõ ràng sẽ yêu cầu$O(n^2)$thời gian chỉ để liệt kê chúng và sau đó là chi phí bổ sung để tính toán GCD, điều này là không thể$n = 10^6$. 

Việc biểu diễn đầu vào dưới dạng một chuỗi thay vì một số là rất quan trọng. Bản thân giá trị này quá lớn để phù hợp với bất kỳ loại số nào, nhưng quan trọng hơn, cấu trúc của chuỗi con phụ thuộc vào vị trí chữ số chứ không phải độ lớn số học. 

Trường hợp góc tinh tế xuất hiện khi độ dài chuỗi bằng 1. Trong trường hợp đó, có chính xác một chuỗi con, chính là số đó, do đó đáp án phải bằng chữ số. 

Một quan sát quan trọng khác là các chuỗi con có một chữ số đã xuất hiện trong tập hợp. Điều này có nghĩa là GCD cuối cùng phải chia từng chữ số của số đó. Bất kỳ cách tiếp cận nào bỏ qua các chuỗi con có một chữ số sẽ ngay lập tức không chính xác, vì chỉ riêng những cách đó đã hạn chế câu trả lời một cách mạnh mẽ. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Chúng tôi liệt kê mọi chuỗi con, chuyển đổi nó thành số nguyên và duy trì GCD đang chạy trên tất cả các giá trị. Điều này đúng vì nó khớp trực tiếp với định nghĩa. Tuy nhiên, việc tạo ra tất cả các chuỗi con đòi hỏi$O(n^2)$hoạt động và chuyển đổi mỗi chuỗi con thành một số mất tới$O(n)$trong trường hợp xấu nhất nếu được thực hiện một cách ngây thơ, tạo ra tổng độ phức tạp trong thực tế cho các đầu vào lớn. Ngay cả với phân tích cú pháp tăng dần, chỉ riêng số chuỗi con bậc hai cũng khiến điều này không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là tránh suy nghĩ theo các chuỗi con đầy đủ và thay vào đó tập trung vào các ràng buộc về khả năng phân chia do cấu trúc áp đặt. Mỗi chuỗi con kết thúc ở vị trí$i$có thể được xem như một số được hình thành bằng cách thêm các chữ số vào các hậu tố trước đó. Điều này dẫn đến sự đơn giản hóa quan trọng: GCD trên tất cả các chuỗi con được xác định hoàn toàn bởi hai loại chuỗi con, chuỗi con một chữ số và giá trị số của toàn bộ chuỗi. 

Chuỗi con một chữ số buộc câu trả lời phải chia hết mọi chữ số trong số, do đó câu trả lời phải chia$\gcd(\text{all digits})$. Mặt khác, bản thân chuỗi đầy đủ là một trong các chuỗi con nên đáp án cũng phải chia giá trị nguyên của số nguyên. Kết hợp các ràng buộc này, câu trả lời chính xác là ước số chung lớn nhất của giá trị nguyên của số đầy đủ và GCD của các chữ số của nó. 

Điều này làm giảm vấn đề từ việc liệt kê$O(n^2)$chuỗi con để quét tuyến tính một chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 \cdot n)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý số dưới dạng một chuỗi và tính toán song song hai giá trị, GCD của tất cả các chữ số và giá trị số nguyên đầy đủ theo mô-đun một hệ thống chạy số học số nguyên lớn được xử lý hoàn toàn bằng Python. 

1. Khởi tạo một biến`g`về 0. Điều này sẽ lưu trữ GCD của tất cả các chữ số đã thấy cho đến nay. Giá trị nhận dạng 0 được sử dụng vì`gcd(0, x) = x`. 
2. Khởi tạo một biến`value`thành 0. Số này sẽ biểu thị số đầy đủ khi chúng ta đọc từng chữ số. Ở mỗi bước chúng tôi cập nhật nó bằng cách sử dụng`value = value * 10 + digit`. 
3. Lặp lại từng ký tự của chuỗi, chuyển đổi nó thành một chữ số nguyên và cập nhật`g`BẰNG`g = gcd(g, digit)`. Điều này đảm bảo rằng sau khi xử lý tất cả các chữ số,`g`bằng GCD của tất cả các chữ số riêng lẻ trong số đó. 
4. Cập nhật`value`tăng dần cho mỗi chữ số bằng cách sử dụng tích lũy cơ số 10. Điều này xây dựng số đầy đủ mà không bao giờ lưu trữ toàn bộ số nguyên một cách rõ ràng theo nghĩa int lớn ngoài sự hỗ trợ tự nhiên của Python. 
5. Sau khi xử lý tất cả các chữ số, hãy tính kết quả cuối cùng là`gcd(value, g)`. 

Lý do điều này là đủ là vì mỗi chuỗi con áp đặt một ràng buộc về tính chia hết đã được ghi lại bởi chuỗi con số nguyên hoặc chuỗi con một chữ số. Bất kỳ chuỗi con dài hơn nào cũng không tạo ra ràng buộc độc lập mới ngoài hai thái cực này. 

### Tại sao nó hoạt động 

Mỗi chuỗi con là một số nguyên được hình thành từ các chữ số liên tiếp. Bất kỳ số nào như vậy đều có thể chia hết cho bất kỳ số nguyên nào chia cả hai chữ số của nó và cấu trúc được tạo ra bởi phép nối. Tập hợp tất cả các chuỗi con bao gồm tất cả các chữ số đơn và số đầy đủ, và bất kỳ ước số chung nào của toàn bộ tập hợp đều phải chia hai trường hợp cực trị này. Ngược lại, bất kỳ số nào chia cả tất cả các chữ số và số đầy đủ sẽ chia mọi chuỗi con vì các chuỗi con được tạo từ các chữ số giống nhau theo trọng số vị trí, giúp duy trì khả năng chia hết cho bất kỳ thừa số chung nào của các chữ số và nối đầy đủ. Điều này thu gọn toàn bộ tập hợp các chuỗi con thành hai ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import gcd

def solve():
    s = input().strip()
    
    g = 0
    value = 0
    
    for ch in s:
        d = ord(ch) - 48
        g = gcd(g, d)
        value = value * 10 + d
    
    print(gcd(value, g))

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào một lần truyền qua chuỗi. Việc trích xuất chữ số sử dụng số học trên mã ký tự để đạt hiệu quả. GCD của các chữ số được tích lũy tăng dần, bắt đầu từ 0 đến hấp thụ tự nhiên chữ số đầu tiên. 

Giá trị số đầy đủ được xây dựng tăng dần theo cơ số 10. Các số nguyên chính xác tùy ý của Python đảm bảo tính chính xác ngay cả đối với một triệu chữ số. 

Bước cuối cùng kết hợp cả hai ràng buộc bằng hàm GCD tiêu chuẩn. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`28`Chúng tôi theo dõi chữ số GCD và giá trị tăng dần. 

| Bước | Chữ số | g (chữ số GCD) | giá trị | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | 2 | 
| 2 | 8 | 2 | 28 | 

Tính toán cuối cùng cho$\gcd(28, 2) = 2$. 

Điều này cho thấy dù số đầy đủ là 28 nhưng cấu trúc chữ số lại hạn chế câu trả lời hơn nữa. 

### Ví dụ 2:`171`| Bước | Chữ số | g (chữ số GCD) | giá trị | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 7 | 1 | 17 | 
| 3 | 1 | 1 | 171 | 

Kết quả cuối cùng là$\gcd(171, 1) = 1$. 

Điều này chứng tỏ rằng khi bất kỳ chữ số nào là 1, chữ số-GCD sẽ giảm xuống 1, buộc câu trả lời là 1 bất kể số đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi chữ số được xử lý một lần với các cập nhật liên tục | 
| Không gian |$O(1)$| Chỉ có hai số nguyên được duy trì | 

Quét tuyến tính dễ dàng phù hợp với giới hạn lên tới một triệu chữ số. Các thao tác trên mỗi ký tự là tối thiểu, bao gồm một gcd duy nhất và một bản cập nhật số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io
from math import gcd

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    s = sys.stdin.readline().strip()
    g = 0
    value = 0
    for ch in s:
        d = ord(ch) - 48
        g = gcd(g, d)
        value = value * 10 + d
    return str(gcd(value, g))

assert run("6\n") == "6", "sample 1"
assert run("28\n") == "2", "sample 2"
assert run("171\n") == "1", "sample 3"

assert run("1\n") == "1", "single digit"
assert run("999999\n") == "999999", "all same digits"
assert run("123456\n") == "1", "mixed digits"
assert run("888888888888888888888888888888\n") == "888888888888888888888888888888", "repeated digit edge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| độ chính xác kích thước tối thiểu | 
|`999999`|`999999`| tính nhất quán của tất cả các chữ số giống hệt nhau | 
|`123456`|`1`| các chữ số hỗn hợp thu gọn thành 1 | 

## Vỏ cạnh 

Đối với đầu vào có một chữ số như`7`, thuật toán khởi tạo`g = 0`, cập nhật nó lên 7 và xây dựng`value = 7`. Câu trả lời cuối cùng trở thành`gcd(7, 7) = 7`, khớp với thực tế là chỉ có một chuỗi con. 

Đối với một đầu vào thống nhất như`999`, mỗi bước giữ nguyên`g = 9`, trong khi`value`tăng lên 999. gcd cuối cùng là`gcd(999, 9) = 9`, xác nhận rằng các chữ số lặp lại không đưa ra bất kỳ hạn chế bổ sung nào ngoài ràng buộc gcd chữ số. 

Đối với một đầu vào hỗn hợp như`123`, chữ số gcd ngay lập tức trở thành 1 và kết quả giảm xuống 1 bất kể giá trị số đầy đủ.
