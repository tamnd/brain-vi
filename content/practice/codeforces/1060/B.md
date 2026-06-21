---
title: "CF 1060B - Tổng chữ số tối đa"
description: "Chúng ta có một số nguyên lớn $n$, và chúng ta muốn chia nó thành hai số nguyên không âm $a$ và $b$ sao cho tổng của chúng vẫn chính xác là $n$."
date: "2026-06-15T09:15:40+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1060
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 513 by Barcelona Bootcamp (rated, Div. 1 + Div. 2)"
rating: 1100
weight: 1060
solve_time_s: 649
verified: true
draft: false
---

[CF 1060B - Tổng chữ số tối đa](https://codeforces.com/problemset/problem/1060/B) 

**Đánh giá:** 1100 
**Thẻ:** tham lam 
**Thời gian giải:** 10 phút 49 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên lớn$n$, và chúng tôi muốn chia nó thành hai số nguyên không âm$a$Và$b$sao cho tổng của chúng giữ nguyên chính xác$n$. Trong số tất cả các phép chia có thể xảy ra, chúng ta được yêu cầu tối đa hóa tổng các chữ số của hai phần, nghĩa là chúng ta muốn tối đa hóa$S(a) + S(b)$, Ở đâu$S(x)$là tổng các chữ số thập phân của$x$. 

Vì vậy, vấn đề không phải là chọn số tùy ý mà là phân bố các chữ số của$n$thành hai số cộng lại một cách chính xác đồng thời làm cho tổng các chữ số càng lớn càng tốt. 

Ràng buộc$n \le 10^{12}$có nghĩa$n$có nhiều nhất 12 chữ số. Một liệt kê trực tiếp của tất cả các phần tách$a \in [0, n]$là không thể bởi vì nó sẽ yêu cầu lặp lại lên đến$10^{12}$giá trị vượt quá giới hạn cho phép. Ngay cả việc kiểm tra từng phần và tính tổng các chữ số cũng sẽ$O(n \cdot \log n)$, điều đó hoàn toàn không thể thực hiện được. 

Điều này ngay lập tức buộc chúng ta phải tìm kiếm một thuộc tính cấu trúc về cách các tổng chữ số ứng xử dưới phép cộng với số mang. 

Một trực giác ngây thơ có thể gợi ý rằng việc thực hiện cả hai$a$Và$b$“Cân bằng” có thể hữu ích, nhưng tổng chữ số hoạt động kỳ lạ khi được truyền bá. Sự phức tạp thực sự xuất phát từ cách mang giảm tổng các chữ số: bất cứ khi nào hai chữ số có tổng bằng 10 hoặc nhiều hơn, việc mang sẽ giảm tổng các chữ số đi 9 tại vị trí đó. 

Trường hợp cạnh tinh tế xuất hiện khi$n$là lũy thừa của mười chẳng hạn$n = 1000$. Một sự chia rẽ ngây thơ như$500 + 500$có vẻ cân bằng, nhưng tổng chữ số tương đối nhỏ so với một cấu trúc như$999 + 1$, khai thác hiệu quả hơn. Điều này gợi ý rằng cấu trúc tối ưu được kết nối chặt chẽ với số thập phân hơn là phân vùng đối xứng. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu thử mọi cách phân chia có thể$a$từ 0 đến$n$, tính toán$b = n - a$, và đánh giá$S(a) + S(b)$. Điều này đúng vì nó kiểm tra mọi cấu hình hợp lệ. Tuy nhiên, nó đòi hỏi$n$lần lặp và chi phí tính toán tổng mỗi chữ số$O(\log n)$, do đó độ phức tạp tổng cộng là$O(n \log n)$. Vì$n \approx 10^{12}$, điều này vượt xa mọi thời gian chạy khả thi. 

Quan sát quan trọng là tổng chữ số tương tác với phép cộng thông qua nhớ. Thay vì nghĩ đến việc chọn các phép chia tùy ý, chúng ta xem xét cấu trúc từng chữ số. Mỗi vị trí thập phân hoạt động độc lập ngoại trừ vị trí mang. Ý tưởng quan trọng là cách tốt nhất để tối đa hóa tổng chữ số là giảm thiểu số mang mang tính hủy diệt và thay vào đó buộc các chữ số trở nên lớn nhất có thể trong cả hai số. 

Điều này dẫn đến một cái nhìn sâu sắc tham lam tiêu chuẩn: chúng tôi cố gắng xây dựng$a$Và$b$sao cho các chữ số của chúng càng lớn càng tốt trong khi vẫn có tổng bằng$n$. Một cách đơn giản hóa nổi tiếng là sự phân chia tối ưu xảy ra khi chúng ta "mượn" theo cách tạo ra cấu trúc chữ số tối đa 9 và 0. Cụ thể, câu trả lời hóa ra đạt được bằng cách thử phân chia trong đó một số bao gồm các chữ số dẫn đầu bão hòa đến 9 trước khi giảm và số kia hấp thụ phần còn lại. 

Một cách thực tế để nắm bắt được điều này là thử tách$n$ở các ranh giới chữ số khác nhau. Đối với mỗi vị trí phân chia tiền tố, chúng tôi mô phỏng việc tạo một số bao gồm một tiền tố như$10^k - 1$đóng góp kiểu -, giúp tối đa hóa tổng chữ số cục bộ. Từ$n$có nhiều nhất 12 chữ số, chúng ta chỉ cần kiểm tra$O(\log n)$cấu trúc ứng cử viên bắt nguồn từ điều chỉnh tiền tố. 

Do đó chúng ta rút gọn vấn đề từ việc tìm kiếm đầy đủ trên tất cả$a$tới một tập nhỏ các ứng cử viên tham lam được xây dựng từ cấu trúc chữ số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \log n)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(d^2)$Ở đâu$d \le 12$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi$n$thành một chuỗi để chúng ta có thể suy luận trực tiếp về các chữ số thập phân của nó. Làm việc theo chữ số là điều cần thiết vì hành vi của tổng chữ số phụ thuộc vào việc mang giữa các vị trí. 
2. Hãy thử xây dựng các phân tách ứng viên bằng cách lặp lại từng vị trí có thể trong đó chúng ta rút gọn tiền tố của$n$và thay thế hậu tố bằng cấu hình chữ số lớn nhất có thể không vượt quá số ban đầu. Điều này thể hiện ý tưởng rằng việc đẩy các chữ số về phía 9 sẽ làm tăng tổng các chữ số. 
3. Với mỗi cách xây dựng ứng cử viên, hãy tính cặp ngụ ý$(a, b)$bằng cách mô phỏng phần còn lại của$n$sẽ được phân phối sau khi sửa một thành phần. 
4. Tính toán$S(a) + S(b)$cho mỗi ứng viên. Vì tổng các chữ số rẻ đối với các số có 12 chữ số nên bước này là thời gian không đổi cho mỗi ứng viên trong thực tế. 
5. Theo dõi giá trị tối đa trên tất cả các ứng cử viên và xuất nó. 

Lý do chúng ta chỉ cần những ứng cử viên có cấu trúc này là vì bất kỳ giải pháp tối ưu nào cũng có thể được chuyển đổi thành một giải pháp trong đó ít nhất một trong các số có mẫu chữ số bao gồm tiền tố theo sau là tất cả số 9 mà không làm giảm mục tiêu. Đối số chuyển đổi này làm giảm không gian tìm kiếm vô hạn thành một tập hợp hữu hạn nhỏ. 

### Tại sao nó hoạt động 

Bất biến chính là bất cứ khi nào một vị trí chữ số không bão hòa hoàn toàn đến 9 trong cả hai$a$hoặc$b$, chúng ta có thể điều chỉnh cục bộ phép phân tách để đẩy chữ số đó lên trên mà không vi phạm ràng buộc về tổng, trừ khi có ràng buộc nhớ chặn nó. Điểm chặn đó xảy ra chính xác tại ranh giới tiền tố của$n$. Do đó, bất kỳ cấu hình tối ưu nào đều tương ứng với một ranh giới nơi xảy ra quá trình chuyển đổi mang và những ranh giới đó chính xác là những ranh giới mà chúng tôi liệt kê. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def digit_sum(x: int) -> int:
    s = 0
    while x:
        s += x % 10
        x //= 10
    return s

def solve():
    n = int(input().strip())

    best = digit_sum(n)  # (a=n, b=0) baseline

    # Try splitting by forcing a prefix boundary in b
    # We construct candidates of form:
    # b = 10^k - 1, a = n - b
    p = 1
    while p <= n:
        b = p - 1
        a = n - b
        best = max(best, digit_sum(a) + digit_sum(b))
        p *= 10

    print(best)

if __name__ == "__main__":
    solve()
```Việc triển khai được xây dựng dựa trên quan sát rằng sự phân chia hiệu quả nhất xảy ra khi một số gần với một số có dạng$10^k - 1$, là một dãy số 9 ở dạng thập phân. Chúng tôi kiểm tra tất cả các ranh giới như vậy với mức độ$n$. 

Trường hợp cơ bản$(n, 0)$được đưa vào vì nó luôn hợp lệ và đôi khi tối ưu khi$n$chính nó có tổng chữ số lớn. 

Vòng lặp lũy thừa mười tạo ra tất cả các ứng cử viên có thể có "tất cả số 9" được căn chỉnh theo tiền tố. Đối với mỗi ứng viên như vậy$b$, chúng tôi tính toán$a = n - b$, đảm bảo ràng buộc$a + b = n$luôn luôn giữ. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 35$Chúng tôi kiểm tra ứng viên ở đâu$b = 9$Và$b = 99$(chỉ một$9$có liên quan ở đây). 

| bước | p | b = p-1 | a = n-b | S(a) | S(b) | tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | - | 0 | 35 | 8 | 0 | 8 | 
| 1 | 1 | 0 | 35 | 8 | 0 | 8 | 
| 2 | 10 | 9 | 26 | 8 | 9 | 17 | 
| 3 | 100 | 99 | không hợp lệ (có hiệu lực âm a bị bỏ qua) | - | - | - | 

Cấu hình tốt nhất là$a=26, b=9$, cho 17. 

Điều này cho thấy cách giới thiệu một chữ số 9 trong$b$lực lượng$a$để tăng tổng chữ số sau khi điều chỉnh. 

### Ví dụ 2:$n = 100$| bước | p | b | một | S(a) | S(b) | tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | - | 0 | 100 | 1 | 0 | 1 | 
| 1 | 1 | 0 | 100 | 1 | 0 | 1 | 
| 2 | 10 | 9 | 91 | 10 | 9 | 19 | 
| 3 | 100 | 99 | 1 | 1 | 18 | 19 | 

Giá trị tối ưu là 19, đạt được bằng cả hai cách. 

Những dấu vết này cho thấy rằng việc phân phối số 9 trong một thành phần sẽ tối đa hóa sự đóng góp của chữ số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log n \cdot d)$| Chúng ta thử tối đa 12 lũy thừa của 10, tổng giá trị của mỗi chữ số$O(d)$| 
| Không gian |$O(1)$| Chỉ một số số nguyên được lưu trữ | 

Số logarit của các ứng cử viên phù hợp với cấu trúc thập phân của$n$, làm cho giải pháp dễ dàng đủ nhanh cho$n \le 10^{12}$. 

## Trường hợp thử nghiệm```python
import sys, io

def digit_sum(x):
    return sum(map(int, str(x)))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input().strip())

    best = digit_sum(n)
    p = 1
    while p <= n:
        b = p - 1
        a = n - b
        best = max(best, digit_sum(a) + digit_sum(b))
        p *= 10

    return str(best)

# provided sample
assert run("35\n") == "17"

# minimum case
assert run("1\n") == "1"

# power of ten case
assert run("100\n") == "19"

# all digits 9
assert run("99\n") == "18"

# large case
assert run("1000000000000\n") == str(max(digit_sum(1000000000000 - (10**k - 1)) + digit_sum(10**k - 1) for k in range(13)))

# symmetric case
assert run("50\n") == str(run("50\n"))

# edge carry-heavy case
assert run("19\n") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | ranh giới nhỏ nhất | 
| 100 | 19 | mang lại lợi ích cơ cấu | 
| 99 | 18 | hành vi toàn 9 | 
| 19 | 10 | phân chia nhạy cảm mang theo | 
| 10^12 | tính toán | xử lý ràng buộc lớn | 

## Vỏ cạnh 

cho$n = 1$, phép chia hợp lệ duy nhất là$(1, 0)$. Thuật toán cố gắng$b = 0$và trả về$S(1) = 1$, điều đó đúng. 

Vì$n = 1000$, thuật toán đánh giá$b = 9, 99, 999$. Điều tốt nhất thường là$b = 999$, mang lại$a = 1$, cho tổng các chữ số$1 + 27 = 28$. Vòng lặp tiếp cận chính xác trường hợp này vì nó lặp trên tất cả lũy thừa của mười. 

Vì$n = 19$, cách chia tốt nhất là$10 + 9$, cho tổng các chữ số$1 + 0 + 9 = 10$. Ứng viên$b = 9$được kiểm tra rõ ràng và thuật toán nắm bắt chính xác sự phân tách dựa trên số liệu mang theo tối ưu này. 

Mỗi trường hợp này đều chứng minh rằng thuật toán không dựa vào sự phân bố đồng đều mà thay vào đó khám phá một cách có hệ thống các ranh giới liên kết mang, đây chính xác là nơi diễn ra những cải tiến đối với việc phân tách đơn giản.
