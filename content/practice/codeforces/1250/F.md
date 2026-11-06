---
title: "CF 1250F - Trung Tâm Dữ Liệu"
description: "Chúng ta được cho một vùng mục tiêu $n$ và chúng ta muốn xây dựng một hình chữ nhật có các cạnh là số nguyên và có diện tích chính xác là $n$. Mọi hình chữ nhật hợp lệ tương ứng với việc chọn hai số nguyên $a$ và $b$ sao cho $a cdot b = n$."
date: "2026-06-13T21:14:28+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1250
codeforces_index: "F"
codeforces_contest_name: "2019-2020 ICPC, NERC, Southern and Volga Russian Regional Contest (Online Mirror, ICPC Rules, Teams Preferred)"
rating: 800
weight: 1250
solve_time_s: 146
verified: true
draft: false
---

[CF 1250F - Trung tâm dữ liệu](https://codeforces.com/problemset/problem/1250/F) 

**Đánh giá:** 800 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 2m 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một khu vực mục tiêu$n$, và chúng ta muốn xây dựng một hình chữ nhật có các cạnh là số nguyên và có diện tích chính xác là$n$. Mọi hình chữ nhật hợp lệ tương ứng với việc chọn hai số nguyên$a$Và$b$như vậy$a \cdot b = n$. Trong số tất cả các cặp thừa số như vậy, chúng ta muốn hình chữ nhật có chu vi nhỏ nhất, đó là$2(a + b)$. 

Vì vậy, nhiệm vụ hoàn toàn là phân tích nhân tử: trong số tất cả các cặp số nguyên có tích cố định, chúng ta cần cặp có tổng của chúng cực tiểu hóa. Về mặt hình học, điều này tương ứng với việc làm cho hình chữ nhật càng gần hình vuông càng tốt, vì các cạnh cân bằng làm giảm chu vi cho một diện tích cố định. 

Ràng buộc$n \le 10^5$đủ nhỏ để lặp lại tới$\sqrt{n}$là khả thi trong thời gian không đổi cho mỗi bài kiểm tra. Một sức mạnh tàn bạo đối với tất cả các yếu tố có thể lên đến$n$vẫn sẽ vượt qua, nhưng nó là không cần thiết. 

Một trường hợp thất bại tinh vi đối với các giải pháp bất cẩn là giả định$a = \lfloor \sqrt{n} \rfloor$luôn đưa ra một hình chữ nhật hợp lệ. Điều đó chỉ có tác dụng khi$n$là một hình vuông hoàn hảo Ví dụ, nếu$n = 13$,$\lfloor \sqrt{13} \rfloor = 3$, Nhưng$13$không chia hết cho$3$, Vì thế$3 \times 3$không hợp lệ. Câu trả lời đúng đến từ$1 \times 13$. 

Một trường hợp cạnh khác là$n = 1$. Hình chữ nhật duy nhất là$1 \times 1$, vậy chu vi là$4$. Bất kỳ logic nào dựa vào tìm kiếm nhân tố vẫn phải xử lý việc này một cách rõ ràng. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là liệt kê mọi cặp có thể$(a, b)$như vậy$a \cdot b = n$, tính toán$2(a+b)$, và lấy mức tối thiểu. Tính chính xác là ngay lập tức vì nó kiểm tra tất cả các hình chữ nhật hợp lệ. Vấn đề là tính hiệu quả: quét tất cả các giá trị của$a$từ$1$ĐẾN$n$dẫn đến$O(n)$kiểm tra, điều này là không cần thiết và lãng phí mặc dù$n$chỉ là$10^5$. 

Quan sát quan trọng là các yếu tố xuất hiện theo từng cặp xung quanh$\sqrt{n}$. Nếu như$a \cdot b = n$, thì ít nhất một trong$a, b$nhỏ hơn hoặc bằng$\sqrt{n}$. Điều này có nghĩa là chúng ta chỉ cần lặp lại$a$lên đến$\sqrt{n}$, và bất cứ khi nào$a$chia rẽ$n$, chúng tôi đánh giá cặp tương ứng$(a, n/a)$. 

Điều này làm giảm không gian tìm kiếm từ kích thước tuyến tính sang kích thước căn bậc hai trong khi vẫn đảm bảo chúng tôi kiểm tra mọi cặp yếu tố hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$|$O(1)$| Quá chậm | 
| Tối ưu (quét hệ số) |$O(\sqrt{n})$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`best`với một giá trị rất lớn để lưu trữ chu vi tối thiểu được tìm thấy cho đến nay. Điều này đảm bảo bất kỳ hình chữ nhật hợp lệ nào cũng sẽ cải thiện nó. 
2. Lặp lại$a$từ$1$ĐẾN$\lfloor \sqrt{n} \rfloor$. Chúng tôi chỉ tìm kiếm đến căn bậc hai vì mỗi cặp nhân tố phải có ít nhất một phần tử trong phạm vi này. 
3. Đối với mỗi$a$, kiểm tra xem$n \bmod a = 0$. Nếu không, hãy bỏ qua vì nó không thể tạo thành một cạnh hình chữ nhật hợp lệ. 
4. Nếu chia đều thì tính$b = n / a$. Điều này mang lại một hình chữ nhật hợp lệ$a \times b$. 
5. Tính chu vi$p = 2(a + b)$và cập nhật`best = min(best, p)`. Mỗi cặp yếu tố hợp lệ là một hình dạng ứng cử viên. 
6. Sau khi kết thúc vòng lặp, xuất ra`best`. 

### Tại sao nó hoạt động 

Mỗi hình chữ nhật hợp lệ tương ứng với chính xác một cặp nhân tố$(a, b)$với$a \le \sqrt{n}$hoặc$b \le \sqrt{n}$. Vòng lặp đảm bảo chúng ta gặp tất cả các cặp như vậy chính xác một lần thông qua hệ số nhỏ hơn. Vì chu vi chỉ phụ thuộc vào tổng$a + b$, việc kiểm tra tất cả các cặp hợp lệ sẽ đảm bảo tìm thấy mức tối thiểu mà không bỏ sót bất kỳ ứng cử viên nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

best = 10**18
i = 1

while i * i <= n:
    if n % i == 0:
        j = n // i
        best = min(best, 2 * (i + j))
    i += 1

print(best)
```Giải pháp duy trì mức hoạt động tối thiểu trên tất cả các cặp yếu tố hợp lệ. Điều kiện vòng lặp`i * i <= n`tránh các căn bậc hai có dấu phẩy động và đảm bảo phép lặp an toàn với số nguyên. Mỗi khi chúng tôi tìm thấy một ước số, chúng tôi sẽ ngay lập tức tạo thành hệ số ghép đôi bằng cách sử dụng phép chia số nguyên, đảm bảo không phát sinh vấn đề về độ chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 36$| tôi | Chia? | j = 36/i | Chu vi$2(i+j)$| Tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | vâng | 36 | 74 | 74 | 
| 2 | vâng | 18 | 40 | 40 | 
| 3 | vâng | 12 | 30 | 30 | 
| 4 | vâng | 9 | 26 | 26 | 
| 5 | không | - | - | 26 | 
| 6 | vâng | 6 | 24 | 24 | 

Dấu vết cho thấy chu vi co lại khi cặp nhân tố trở nên cân bằng hơn, đạt mức tối thiểu tại bình phương$6 \times 6$. 

### Ví dụ 2:$n = 13$| tôi | Chia? | j = 13/i | Chu vi | Tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | vâng | 13 | 28 | 28 | 
| 2 | không | - | - | 28 | 
| 3 | không | - | - | 28 | 

Chỉ tồn tại một cặp nhân tố hợp lệ, do đó thuật toán sẽ quay trở lại hình chữ nhật một cách chính xác$1 \times 13$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{n})$| Chúng tôi kiểm tra từng số nguyên lên đến$\sqrt{n}$một lần | 
| Không gian |$O(1)$| Chỉ có một vài biến số nguyên được lưu trữ | 

Giới hạn trên$n \le 10^5$làm cho$\sqrt{n} \le 316$, do đó vòng lặp chạy số lần lặp không đáng kể trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline
    n = int(input())

    best = 10**18
    i = 1
    while i * i <= n:
        if n % i == 0:
            j = n // i
            best = min(best, 2 * (i + j))
        i += 1

    print(best)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    from io import StringIO
    out = StringIO()
    old_stdout = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old_stdout
    sys.stdin = old_stdin
    return out.getvalue().strip()

# provided samples
assert run("36\n") == "24"
assert run("13\n") == "28"

# custom cases
assert run("1\n") == "4"
assert run("2\n") == "6"
assert run("49\n") == "28"
assert run("100000\n") == str(min(2*(i + 100000//i) for i in range(1, 100000+1) if 100000 % i == 0))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 4 | trường hợp biên nhỏ nhất | 
| 2 | 6 | hành vi giống số nguyên tố không vuông nhỏ nhất | 
| 49 | 28 | trường hợp vuông hoàn hảo | 

## Vỏ cạnh 

cho$n = 1$, hình chữ nhật hợp lệ duy nhất là$1 \times 1$. Vòng lặp vẫn thực hiện một lần với$i = 1$, phát hiện khả năng chia hết và tính chu vi$2(1+1) = 4$, phù hợp với câu trả lời đúng. 

Đối với một số nguyên tố như$n = 13$, không có ước số nào tồn tại ngoại trừ$1$, do đó thuật toán tự nhiên quay trở lại$1 \times 13$, tạo chu vi$28$. Sự vắng mặt của các ước số bên trong không yêu cầu xử lý đặc biệt. 

Đối với các hình vuông hoàn hảo như$n = 36$, nhiều cặp yếu tố được phát hiện và cực tiểu xảy ra ở cặp trung tâm$6 \times 6$. Việc lặp lại đảm bảo cặp này được đánh giá rõ ràng, do đó cấu trúc tối ưu không bao giờ bị bỏ sót.
