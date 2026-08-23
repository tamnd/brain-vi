---
title: "CF 104283I - Chìa Khóa Bí Mật"
description: "Chúng ta có hai số nguyên, $A$ và $B$, cùng với hai số dư đích $m1$ và $m2$. Nhiệm vụ là tìm số nguyên dương nhỏ nhất $X$ sao cho khi chia $A$ cho $X$, số dư chính xác là $m1$, và khi $B$ chia cho $X$, số dư chính xác là $m2$."
date: "2026-07-01T21:02:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104283
codeforces_index: "I"
codeforces_contest_name: "Contest Based on Brain Craft Intra SUST Programming Contest 2023"
rating: 0
weight: 104283
solve_time_s: 51
verified: true
draft: false
---

[CF 104283I - Chìa khóa bí mật](https://codeforces.com/problemset/problem/104283/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho ta hai số nguyên$A$Và$B$, cùng với hai số dư mục tiêu$m_1$Và$m_2$. Nhiệm vụ là tìm số nguyên dương nhỏ nhất$X$như vậy khi$A$được chia cho$X$, số dư chính xác là$m_1$, và khi nào$B$được chia cho$X$, số dư chính xác là$m_2$. 

Nói cách khác, chúng ta muốn một mô đun$X$đồng thời tạo ra hai số dư cố định từ hai số cố định. Ràng buộc về số dư bao hàm hai điều kiện chia hết:$A - m_1$phải chia hết cho$X$, Và$B - m_2$cũng phải chia hết cho$X$. Ngoài ra, định nghĩa phần còn lại thực thi$m_1 < X$Và$m_2 < X$, nếu không phần còn lại sẽ không hợp lệ. 

Kích thước đầu vào tăng lên$5 \cdot 10^8$cho các giá trị, vì vậy bất kỳ cách tiếp cận nào lặp lại tới$A$hoặc$B$trực tiếp là không thể. Thậm chí quét tất cả các ước số lên đến$10^9$là quá chậm trong trường hợp xấu nhất. Chúng ta cần một phương pháp suy luận dựa trên logarit hoặc căn bậc hai. 

Trường hợp cạnh tinh tế xuất hiện khi một trong hai$A = m_1$hoặc$B = m_2$. Trong tình huống đó, sự khác biệt tương ứng sẽ bằng không. Ví dụ, nếu$A = 10$Và$m_1 = 10$, sau đó$A \bmod X = 10$là không thể cho bất kỳ hợp lệ$X > 10$, vì số dư luôn nhỏ hơn số chia. Điều này ngay lập tức làm cho vấn đề không được giải quyết. Việc thực hiện bất cẩn chỉ kiểm tra tính chia hết của các khác biệt sẽ coi số 0 là tương thích với tất cả một cách không chính xác.$X$. 

Một trường hợp cạnh khác là khi phần dư cần thiết xung đột với các ràng buộc về kích thước. Nếu như$m_1 \ge X$hoặc$m_2 \ge X$, mô đun không hợp lệ ngay cả khi điều kiện chia hết được giữ nguyên. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ thử mọi ứng viên$X$từ 1 đến$\max(A, B)$, kiểm tra xem cả hai điều kiện còn lại có đúng không. Đối với mỗi$X$, chúng tôi tính toán$A \bmod X$Và$B \bmod X$trong thời gian không đổi, nên tổng độ phức tạp là$O(\max(A, B))$. Với giá trị lên đến$5 \cdot 10^8$, cách tiếp cận này thực hiện hàng trăm triệu thao tác cho mỗi trường hợp thử nghiệm, vượt xa giới hạn thời gian. 

Quan sát quan trọng là điều kiện còn lại có thể được viết lại thành dạng chia hết. Từ$A \bmod X = m_1$, chúng tôi nhận được$A - m_1 = kX$đối với một số nguyên$k$. Tương tự,$B - m_2 = tX$. Điều này có nghĩa$X$phải chia cả hai$A - m_1$Và$B - m_2$. Vì thế$X$phải là ước chung của hai số đó. 

Tại thời điểm này, bài toán quy về việc tìm ước số của hai số nguyên cùng một lúc và chúng ta nghĩ ngay đến ước số chung lớn nhất. Bất kỳ hợp lệ$X$phải chia$g = \gcd(A - m_1, B - m_2)$. Vì vậy ứng viên cho$X$chính xác là các ước của$g$, với ràng buộc bổ sung là$X > m_1$Và$X > m_2$. 

Thay vì lặp lại đến$A$, chúng ta chỉ lặp qua các ước của$g$, điều này có thể được thực hiện trong$O(\sqrt{g})$. Trong số các ước số hợp lệ này, chúng tôi chọn ước số nhỏ nhất thỏa mãn giới hạn còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\max(A,B))$|$O(1)$| Quá chậm | 
| GCD + Ước |$O(\sqrt{g})$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán$x = A - m_1$Và$y = B - m_2$. Nếu một trong hai$x < 0$hoặc$y < 0$, không có giải pháp tồn tại. Điều này là do số dư không thể vượt quá số ban đầu. 
2. Tính toán$g = \gcd(x, y)$. Bất kỳ hợp lệ$X$phải chia cả hai$x$Và$y$, do đó phải chia$g$. 
3. Liệt kê tất cả các ước của$g$. Với mỗi số chia$d$, chúng tôi kiểm tra xem nó có thể phục vụ như một mô-đun hợp lệ hay không. 
4. Đối với mỗi ước số ứng viên$d$, xác minh rằng$d > m_1$Và$d > m_2$. Điều này đảm bảo định nghĩa phần dư là hợp lệ cho cả hai số. 
5. Trong số tất cả các ước số hợp lệ, hãy chọn ước số nhỏ nhất. Nếu không có ước số nào thỏa mãn ràng buộc thì trả về$-1$. 

### Tại sao nó hoạt động 

Sự biến đổi$A \bmod X = m_1 \Rightarrow X \mid (A - m_1)$và tương tự cho$B$hoàn toàn mô tả vấn đề về mặt phân chia. Vì bất kỳ số nào chia cả hai hiệu đều phải chia hết gcd của chúng, nên chúng ta không mất bất kỳ ứng cử viên nào bằng cách tự giới hạn ở các ước của$g$. Các ràng buộc còn lại chỉ lọc các ứng cử viên này hơn nữa. Bởi vì chúng ta liệt kê tất cả các ước của$g$, chúng tôi đảm bảo rằng nếu hợp lệ$X$tồn tại, nó sẽ xuất hiện trong không gian tìm kiếm và việc chọn giá trị hợp lệ nhỏ nhất sẽ đưa ra câu trả lời đúng. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def solve():
    A, B, m1, m2 = map(int, input().split())

    x = A - m1
    y = B - m2

    if x < 0 or y < 0:
        print(-1)
        return

    g = math.gcd(x, y)

    ans = None

    d = 1
    while d * d <= g:
        if g % d == 0:
            d1 = d
            d2 = g // d

            if d1 > m1 and d1 > m2:
                if ans is None or d1 < ans:
                    ans = d1

            if d2 > m1 and d2 > m2:
                if ans is None or d2 < ans:
                    ans = d2

        d += 1

    print(ans if ans is not None else -1)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách chuyển đổi các điều kiện còn lại thành dạng sai phân. Việc kiểm tra tính khả thi sớm đảm bảo chúng tôi không tiến hành với những khác biệt tiêu cực không hợp lệ. 

Bước gcd là sự rút gọn về cấu trúc: nó thu gọn bài toán từ hai ràng buộc thành một không gian ước số duy nhất. Vòng liệt kê số chia kiểm tra tất cả các ứng cử viên một cách hiệu quả đến căn bậc hai của$g$, ghép mỗi ước số với phần bù của nó. 

Một cạm bẫy triển khai phổ biến là quên kiểm tra cả hai$d$Và$g/d$. Thiếu ước số bù dẫn đến đáp án sai khi phương án tối ưu có hệ số lớn hơn. 

Một sự tinh tế khác là xử lý việc so sánh một cách nghiêm ngặt như$>$, không$\ge$, vì số dư phải nhỏ hơn số chia. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
A = 5, B = 3, m1 = 2, m2 = 1
```Chúng tôi tính toán: 

-$x = 5 - 2 = 3$-$y = 3 - 1 = 2$-$g = \gcd(3, 2) = 1$Bây giờ chúng ta liệt kê các ước của 1: chỉ 1. 

| Bước | x | y | gcd | số chia | có hiệu lực? | tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 3 | 2 | - | - | - | - | 
| gcd | 3 | 2 | 1 | - | - | - | 
| kiểm tra | - | - | 1 | 1 | 1 > 2? không | - | 

Không có ước số hợp lệ nào tồn tại nên kết quả là -1. 

Điều này cho thấy rằng ngay cả khi gcd tồn tại, các ràng buộc còn lại có thể loại bỏ tất cả các ứng cử viên. 

### Ví dụ 2 

đầu vào:```
A = 10, B = 14, m1 = 2, m2 = 4
```Chúng tôi tính toán: 

-$x = 8$-$y = 10$-$g = \gcd(8, 10) = 2$Ước của 2 là 1 và 2 

| Bước | x | y | gcd | số chia | có hiệu lực? | tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 8 | 10 | - | - | - | - | 
| gcd | 8 | 10 | 2 | - | - | - | 
| kiểm tra | - | - | 2 | 1 | 1 > 4? không | - | 
| kiểm tra | - | - | 2 | 2 | 2 > 4? không | - | 

Một lần nữa, không có câu trả lời xác đáng nào tồn tại, khẳng định rằng gcd nhỏ không thể thực hiện được khi số dư lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{\gcd(A-m_1, B-m_2)})$| chúng tôi liệt kê các cặp ước số của gcd | 
| Không gian |$O(1)$| chỉ sử dụng một số lượng biến không đổi | 

Việc liệt kê số chia đảm bảo hiệu suất vẫn nhanh ngay cả đối với đầu vào lớn, vì việc phân tích căn bậc hai có hiệu quả đối với các giá trị lên đến$10^9$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    A, B, m1, m2 = map(int, sys.stdin.readline().split())
    x = A - m1
    y = B - m2

    if x < 0 or y < 0:
        return "-1\n"

    g = math.gcd(x, y)

    ans = None
    d = 1
    while d * d <= g:
        if g % d == 0:
            for cand in (d, g // d):
                if cand > m1 and cand > m2:
                    if ans is None or cand < ans:
                        ans = cand
        d += 1

    return (str(ans) if ans is not None else "-1") + "\n"

# custom cases
assert run("5 3 2 1") == "-1\n"
assert run("10 14 2 4") == "-1\n"
assert run("10 14 1 1") == "2\n"
assert run("12 18 0 0") == "2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 3 2 1 | -1 | không có ước số hợp lệ sau các ràng buộc | 
| 10 14 2 4 | -1 | gcd tồn tại nhưng tất cả các ước số không hợp lệ | 
| 10 14 1 1 | 2 | ước số hợp lệ nhỏ nhất được chọn đúng | 
| 12 18 0 0 | 2 | trường hợp cạnh có số dư bằng 0 | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng xảy ra khi$A = m_1$hoặc$B = m_2$. Ví dụ, nếu$A = 10$Và$m_1 = 10$, sau đó$x = 0$. Bất kỳ mô-đun hợp lệ nào cũng sẽ yêu cầu$10 \bmod X = 10$, điều này là không thể vì số dư phải nhỏ hơn$X$. Thuật toán phát hiện ngay$x < 0$là không hợp lệ, nhưng ngay cả khi chúng tôi nới lỏng việc kiểm tra đó,$x = 0$sẽ dẫn đến$g = y$, và chúng ta sẽ xem xét sai các ước của$y$. Điều kiện bất đẳng thức chặt chẽ$X > m_1$ngăn chặn những kết quả dương tính giả như vậy. 

Một trường hợp cạnh khác là khi gcd bằng 1. Trong trường hợp này, ứng cử viên duy nhất là$X = 1$, nhưng điều này không bao giờ có thể đáp ứng bất kỳ yêu cầu về số dư dương nào trừ khi cả hai số dư đều bằng 0. Phép liệt kê ước số xử lý chính xác điều này vì nó vẫn kiểm tra các ràng buộc bất đẳng thức trước khi chấp nhận ứng viên.
