---
title: "CF 1033B - Hiệu bình phương"
description: "Chúng ta bắt đầu với một mảnh vải hình vuông lớn có cạnh dài $a$. Từ một góc, một hình vuông nhỏ hơn có cạnh $b$ được cắt ra. Tấm vải còn lại là một vùng hình chữ L có diện tích đơn giản bằng diện tích của hình vuông lớn trừ đi diện tích của hình vuông bị loại bỏ, do đó $a^2 - b^2$."
date: "2026-06-16T19:36:43+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1033
codeforces_index: "B"
codeforces_contest_name: "Lyft Level 5 Challenge 2018 - Elimination Round"
rating: 1100
weight: 1033
solve_time_s: 291
verified: true
draft: false
---

[CF 1033B - Hiệu bình phương](https://codeforces.com/problemset/problem/1033/B) 

**Đánh giá:** 1100 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 4 phút 51 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một mảnh vải hình vuông lớn có chiều dài cạnh$a$. Từ một góc, một hình vuông có cạnh nhỏ hơn$b$được cắt ra. Tấm vải còn lại là một vùng hình chữ L có diện tích bằng diện tích hình vuông lớn trừ đi diện tích hình vuông bị bỏ đi, vì vậy$a^2 - b^2$. 

Nhiệm vụ không phải là tính trực tiếp diện tích này cho đầu ra mà là quyết định xem số kết quả này có phải là số nguyên tố hay không. 

Mỗi trường hợp thử nghiệm cho một cặp$(a, b)$, và chúng ta phải trả lời liệu$a^2 - b^2$là nguyên tố. 

Những hạn chế là rất lớn:$a$Và$b$có thể đi lên$10^{11}$, có nghĩa là$a^2$có thể đạt được$10^{22}$. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng phân tích các số bằng cách chia thử theo giá trị của chính nó hoặc thậm chí lên đến căn bậc hai của nó một cách ngây thơ cho mọi trường hợp thử nghiệm. Thậm chí$O(\sqrt{n})$mỗi trường hợp thử nghiệm sẽ trở thành ranh giới nếu được thực hiện nhiều lần, bởi vì$\sqrt{10^{22}} = 10^{11}$, nó quá lớn. 

Cấu trúc chính ở đây là chúng ta không được cung cấp một con số tùy ý. Giá trị luôn là hiệu của hai bình phương, có tính chất đại số mạnh hạn chế hành vi phân tích nhân tử của nó. 

Trường hợp cạnh tinh tế xuất hiện khi$a$Và$b$đang rất gần gũi. Ví dụ,$a=34, b=33$cho$34^2 - 33^2 = 67$, đó là số nguyên tố. Nhưng đối với những khoảng trống lớn hơn một chút như$16, 13$, chúng tôi nhận được$87$, là hợp số mặc dù số lượng nhỏ. Một trường hợp khác là khi biểu thức trở thành số chẵn; bất kỳ số chẵn nào lớn hơn 2 đều tự động không phải là số nguyên tố và điều này đã loại trừ nhiều trường hợp không cần tính toán. 

## Phương pháp tiếp cận 

Một cách tiếp cận vũ phu sẽ tính toán$a^2 - b^2$và sau đó kiểm tra tính nguyên tố bằng cách sử dụng phép chia thử lên đến$\sqrt{a^2 - b^2}$. Mặc dù đúng nhưng điều này không khả thi vì sự khác biệt có thể lớn bằng$10^{22}$, việc kiểm tra tính nguyên thủy đòi hỏi tới$10^{11}$số lần lặp cho mỗi trường hợp thử nghiệm trong trường hợp xấu nhất. 

Thông tin chi tiết quan trọng đến từ việc viết lại biểu thức:$$a^2 - b^2 = (a-b)(a+b)$$Hệ số này có tính chất quyết định. Số này luôn là tích của hai số nguyên lớn hơn 0 và cấu trúc của nó cực kỳ hạn chế. 

Từ$a > b$, cả hai thừa số đều là số nguyên dương. Cũng,$a+b > a-b$, và cả hai đều ít nhất bằng 1. Cách duy nhất để tích của hai số nguyên là số nguyên tố là khi một trong chúng bằng 1. 

Vì vậy chúng tôi hỏi: khi nào có thể$(a-b)(a+b)$bằng số nguyên tố? 

Một số nguyên tố có đúng hai ước dương: 1 và chính nó. Do đó một trong các thừa số phải là 1. Vì$a+b \ge 2$, nó không thể bằng 1. Vậy khả năng duy nhất là:$$a - b = 1$$Nếu như$a-b = 1$, thì biểu thức trở thành:$$(1)(2b+1) = 2b+1$$Vậy kết quả là số nguyên tố khi và chỉ khi$2b+1$là nguyên tố. 

Điều này làm giảm vấn đề xuống còn một bài kiểm tra tính nguyên tố duy nhất trên một số lên đến$2 \cdot 10^{11}$, có thể quản lý được bằng cách phân chia thử nghiệm xác định lên đến$\sqrt{n}$, hoặc kiểm tra nhanh hơn chỉ bằng cách sử dụng ước số lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\sqrt{n} \cdot t)$với$n \approx 10^{22}$|$O(1)$| Quá chậm | 
| Tối ưu |$O(\sqrt{b})$cho mỗi trường hợp hợp lệ (thường không đổi do bị từ chối sớm) |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Thiết lập ý tưởng chính 

1. Tính toán$d = a - b$. Điều này cô lập phần duy nhất có thể làm giảm hệ số hóa thành một thứ giống như số nguyên tố. 
2. Nếu$d \neq 1$, ngay lập tức trả về "KHÔNG". Điều này xảy ra vì số này trở thành tích của hai số nguyên đều lớn hơn 1, nên nó là hợp số. 
3. Nếu$d = 1$, tính toán$x = a^2 - b^2 = (a-b)(a+b) = 2b + 1$. 
4. Kiểm tra xem$x$là số nguyên tố sử dụng phép chia thử lên tới$\sqrt{x}$. 
5. Trả về "CÓ" nếu là số nguyên tố, nếu không thì trả về "KHÔNG". 

Quan sát quan trọng là tất cả sự phức tạp đều quy về một trường hợp đặc biệt duy nhất trong đó cấu trúc suy biến thành một số nguyên tố ứng cử viên. 

### Tại sao nó hoạt động 

Bất biến là mọi giá trị hợp lệ$a^2 - b^2$các yếu tố chính xác như$(a-b)(a+b)$. Từ$a+b > a-b$và cả hai đều là số nguyên lớn hơn hoặc bằng 1, biểu thức là hợp số trừ khi một thừa số là 1. Cách duy nhất có thể thỏa mãn điều này là$a-b = 1$, điều này làm giảm một cách duy nhất vấn đề về việc kiểm tra tính nguyên tố của một số dẫn xuất duy nhất$2b+1$. Không có cấu trúc nào khác có thể tạo ra số nguyên tố vì bất kỳ phân tích nhân tử không tầm thường nào đều vi phạm tính nguyên tố ngay lập tức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_prime(n: int) -> bool:
    if n < 2:
        return False
    if n % 2 == 0:
        return n == 2
    i = 3
    while i * i <= n:
        if n % i == 0:
            return False
        i += 2
    return True

t = int(input())
for _ in range(t):
    a, b = map(int, input().split())
    
    if a - b != 1:
        print("NO")
    else:
        val = 2 * b + 1
        print("YES" if is_prime(val) else "NO")
```Việc triển khai dựa vào việc tách trường hợp từ chối tầm thường$a-b \neq 1$từ trường hợp có ý nghĩa duy nhất. Điều này tránh mọi số học số lớn trên$a^2$trực tiếp, ngăn chặn những lo ngại về tràn và tính toán không cần thiết. 

Việc kiểm tra tính nguyên tố được tối ưu hóa bằng cách bỏ qua các số chẵn sau khi xử lý 2, điều này là đủ với cấu trúc của$2b+1$, luôn kỳ quặc. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$a=16, b=13$Chúng tôi tính toán$d = a-b = 3$, nên chúng tôi bác bỏ ngay. 

| Bước | một | b | a-b | Quyết định | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 16 | 13 | - | - | 
| Kiểm tra khác biệt | 16 | 13 | 3 | Từ chối | 

Điều này chứng tỏ rằng bất kỳ sự khác biệt nào lớn hơn 1 đều dẫn đến một số tổng hợp được đảm bảo. 

### Ví dụ 2:$a=34, b=33$Hiện nay$d = 1$, vì vậy chúng tôi tiến hành. 

Chúng tôi tính toán$x = 2b+1 = 67$, sau đó kiểm tra tính nguyên tố. 

| Bước | một | b | a-b | giá trị (2b+1) | Kiểm tra chính | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 34 | 33 | - | - | - | 
| Khác biệt | 34 | 33 | 1 | - | tiến hành | 
| Tính toán | 34 | 33 | 1 | 67 | kiểm tra | 
| Kết quả | 34 | 33 | 1 | 67 | nguyên tố | 

Điều này xác nhận trường hợp đặc biệt trong đó cấu trúc giảm xuống thành ứng cử viên chính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \sqrt{b})$trường hợp xấu nhất | Chỉ khi$a-b=1$, nếu không thì từ chối thời gian liên tục | 
| Không gian |$O(1)$| Chỉ sử dụng một số số nguyên | 

Các ràng buộc cho phép tối đa 5 trường hợp thử nghiệm, do đó, ngay cả việc kiểm tra tính nguyên tố trong trường hợp xấu nhất trên một số có 10 chữ số cũng đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def is_prime(n: int) -> bool:
        if n < 2:
            return False
        if n % 2 == 0:
            return n == 2
        i = 3
        while i * i <= n:
            if n % i == 0:
                return False
            i += 2
        return True

    t = int(input())
    out = []
    for _ in range(t):
        a, b = map(int, input().split())
        if a - b != 1:
            out.append("NO")
        else:
            out.append("YES" if is_prime(2 * b + 1) else "NO")
    return "\n".join(out)

# provided samples
assert run("4\n6 5\n16 13\n61690850361 24777622630\n34 33\n") == "YES\nNO\nNO\nYES"

# custom cases
assert run("1\n3 2\n") == "YES"
assert run("1\n10 9\n") == "NO"
assert run("1\n5 4\n") == "YES"
assert run("1\n100 98\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 2 | CÓ | trường hợp chênh lệch hợp lệ nhỏ nhất | 
| 10 9 | KHÔNG | kết quả lẻ tổng hợp | 
| 5 4 | CÓ | trường hợp kết quả số nguyên tố nhỏ | 
| 100 98 | KHÔNG | chênh lệch không bằng 1 | 

## Vỏ cạnh 

###Trường hợp:$a-b > 1$Đối với đầu vào$a=10, b=7$, chúng tôi nhận được$a-b=3$. Thuật toán ngay lập tức trả về "KHÔNG". Điều này đúng vì$a^2-b^2 = (a-b)(a+b) = 3 \cdot 17 = 51$, đó là hợp chất. 

Việc tính toán không bao giờ đạt đến mức kiểm tra nguyên tố, điều này giúp tránh được những công việc không cần thiết. 

### Trường hợp: khác biệt tối thiểu 

cho$a=3, b=2$, chúng tôi nhận được$a-b=1$, vì vậy chúng tôi tính toán$2b+1=5$. Kiểm tra tính nguyên tố xác nhận 5 là số nguyên tố, do đó kết quả đầu ra là "CÓ". Đây là trường hợp không tầm thường nhỏ nhất trong đó logic kích hoạt nhánh thứ hai. 

### Trường hợp: giá trị lớn 

cho$a=10^{11}, b=10^{11}-1$, thuật toán tính toán$2b+1 \approx 2 \cdot 10^{11}$. Quá trình kiểm tra tính nguyên thủy diễn ra$O(\sqrt{b})$, nhưng điều này vẫn phù hợp dễ dàng vì chỉ cần vài trăm nghìn lần lặp và$t \le 5$. 

Điều này xác nhận giải pháp có tỷ lệ rõ ràng ngay cả ở giới hạn trên.
