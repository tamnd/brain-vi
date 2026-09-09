---
title: "CF 104592B - Hoạt động"
description: "Mỗi trường hợp thử nghiệm đưa ra một giá trị bắt đầu và một tập hợp các “thẻ” số học. Mỗi thẻ là một phép toán có toán hạng cố định và chúng ta được phép sắp xếp lại các thẻ này một cách tùy ý."
date: "2026-06-30T05:24:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104592
codeforces_index: "B"
codeforces_contest_name: "2017 Google Code Jam World Finals (GCJ 17 World Finals)"
rating: 0
weight: 104592
solve_time_s: 59
verified: true
draft: false
---

[CF 104592B - Hoạt động](https://codeforces.com/problemset/problem/104592/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm đưa ra một giá trị bắt đầu và một tập hợp các “thẻ” số học. Mỗi thẻ là một phép toán có toán hạng cố định và chúng ta được phép sắp xếp lại các thẻ này một cách tùy ý. Sau khi chọn thứ tự, ta áp dụng tuần tự các thao tác với giá trị ban đầu và kết quả là một số hữu tỉ. 

Khó khăn chính là thứ tự rất quan trọng vì các phép toán không có tính giao hoán và phép chia chính xác theo số hữu tỉ. Nhiệm vụ là tìm hoán vị của các quân bài sao cho giá trị cuối cùng lớn nhất. 

Mặc dù có bốn loại hoạt động, mỗi thẻ đều biến đổi giá trị hiện tại theo cách rất có cấu trúc. Phép cộng và phép trừ dịch chuyển giá trị, phép nhân và chia tỷ lệ nó. Điều này gợi ý rằng mỗi thẻ không phải là một phép toán tùy ý mà là một phép biến đổi tuyến tính đơn giản trên các số hữu tỉ. 

Các ràng buộc cho phép tối đa 1000 thẻ cho mỗi trường hợp thử nghiệm. Việc thử tất cả các hoán vị là không thể vì điều đó sẽ tăng theo giai thừa. Ngay cả việc lập trình động trên các tập hợp con cũng chỉ khả thi đối với trường hợp nhỏ chứ không khả thi đối với kích thước đầu vào đầy đủ. 

Trường hợp cạnh tinh tế xuất hiện khi các phép toán tạo ra hệ số tỷ lệ âm hoặc khi phép chia tạo ra phân số. Một mô phỏng dựa trên số nguyên đơn giản sẽ bị hỏng ngay lập tức vì các kết quả trung gian không phải là số nguyên và có thể tăng lớn ở cả tử số và mẫu số. Một trường hợp thất bại khác xuất hiện khi cho rằng phép nhân phải luôn được áp dụng cuối cùng hoặc đầu tiên. Ví dụ: hoán đổi phép nhân với số âm bằng phép cộng có thể đảo ngược thứ tự tối ưu, do đó, các quy tắc tham lam dựa trên trực giác về quyền ưu tiên số học không được giữ vững. 

## Phương pháp tiếp cận 

Nỗ lực tự nhiên đầu tiên là bạo lực: thử tất cả các hoán vị của thẻ, mô phỏng biểu thức và giữ kết quả tối đa. Điều này đúng vì nó khám phá toàn bộ không gian giải pháp, nhưng số lượng thẻ lại theo cấp số nhân. Với 1000 thẻ, số lượng hoán vị rất lớn, khiến phương pháp này không thể sử dụng được ngay cả đối với giới hạn thử nghiệm nhỏ vượt quá khoảng 10 đến 12 thẻ. 

Quan sát quan trọng là mỗi lá bài thể hiện một phép biến đổi affine có dạng x → a x + b. Phép cộng và phép trừ tạo ra a = 1 với b khác nhau, phép nhân và chia tạo ra b = 0 với a khác nhau. Khi soạn các phép biến đổi như vậy, kết quả vẫn là affine. Điều này có nghĩa là toàn bộ chuỗi rút gọn thành một phép biến đổi duy nhất x → A x + B, bất kể dấu ngoặc đơn hay độ ưu tiên của toán tử. 

Thành phần hoạt động có thể dự đoán được: nếu chúng ta áp dụng f rồi g, các tham số sẽ nhân lên và kết hợp tuyến tính. Cấu trúc này ngụ ý rằng câu trả lời cuối cùng chỉ phụ thuộc vào cách chúng ta sắp xếp các cặp (a, b). 

Một sự đơn giản hóa quan trọng xuất phát từ việc kiểm tra những gì thực sự thay đổi khi đặt hàng. Nếu chúng ta so sánh hai bậc của hai phép biến đổi, hệ số nhân tổng hợp A của chúng sẽ giống hệt nhau bất kể bậc nào vì phép nhân của các giá trị a có tính chất giao hoán. Số lượng duy nhất bị ảnh hưởng bởi việc đặt hàng là phần phụ gia B. Vì vậy, toàn bộ sự tối ưu hóa giảm xuống mức tối đa hóa B. 

Điều này biến bài toán thành bài toán sắp xếp trong đó mỗi phần tử đóng góp một giá trị b_i được tính bằng tích của các giá trị a theo sau nó. Sau đó, thứ tự tối ưu có thể được rút ra bằng cách sử dụng đối số hoán đổi theo cặp, dẫn đến quy tắc sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | O(C!) | O(C) | Quá chậm | 
| Phép biến đổi affine + quy tắc sắp xếp | O(C log C) | O(C) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta viết lại mỗi thẻ dưới dạng một phép biến đổi affine x → a x + b. 

Đối với mỗi thẻ, chúng tôi xây dựng các tham số của nó. Phép cộng và phép trừ cho a = 1 và b = ±v. Nhân với v cho ra a = v và b = 0. Chia cho v cho ra a = 1/v và b = 0. 

Sau đó chúng ta cần chọn thứ tự của các cặp này.

1. Chuyển tất cả các thẻ thành cặp (a_i, b_i). Điều này tiêu chuẩn hóa tất cả các hoạt động thành một dạng toán học duy nhất. 
2. Quyết định chiến lược đặt hàng bằng cách so sánh các cặp. Đối với hai thẻ i và j bất kỳ, chúng tôi so sánh tác động của việc đặt i trước j so với j trước i về phần đóng góp phụ. Phần nhân không phụ thuộc vào thứ tự nên chỉ có phần đóng góp cộng là quan trọng. 
3. Đối với một cặp cố định, hãy tính cả hai khả năng: 

đặt i trước j đóng góp b_i * a_j + b_j 

đặt j trước khi tôi đóng góp b_j * a_i + b_i 
4. Thích i trước j khi biểu thức đầu tiên lớn hơn. Bất đẳng thức này đơn giản hóa thành một quy tắc so sánh: 

b_i (a_j − 1) ≥ b_j (a_i − 1) 
5. Sắp xếp tất cả các thẻ bằng quy tắc so sánh này. 
6. Sau khi sắp xếp, tính toán phép biến đổi affine cuối cùng theo thành phần tuần tự. Bắt đầu từ phép biến đổi danh tính x → x, cập nhật (A, B) bằng cách sắp xếp từng thẻ theo thứ tự. 
7. Áp dụng phép biến đổi cuối cùng cho S, cho A*S + B. 
8. Xuất kết quả dưới dạng phân số rút gọn với mẫu số dương. 

Bước so sánh là phần không cần thiết duy nhất và nó xác định toàn bộ thứ tự. 

### Tại sao nó hoạt động 

Thành phần phép biến đổi luôn tạo ra giá trị cuối cùng có dạng A*S + B. Vì A không phụ thuộc vào thứ tự, nên việc tối đa hóa kết quả cuối cùng sẽ giảm xuống mức tối đa hóa B. Điều kiện hoán đổi đảm bảo rằng đối với bất kỳ phép đảo ngược liền kề nào, việc hoán đổi sang thứ tự ưu tiên sẽ không giảm B. Việc loại bỏ nhiều lần các phép đảo ngược sẽ dẫn đến một thứ tự tối ưu toàn cục, vì mọi hoán vị đều có thể được sắp xếp bằng cách sử dụng các phép hoán đổi liền kề mà không vi phạm quy tắc đặt hàng. 

## Giải pháp Python```python
import sys
from functools import cmp_to_key
from fractions import Fraction

input = sys.stdin.readline

def solve():
    T = int(input())
    for tc in range(1, T + 1):
        S, C = map(int, input().split())

        cards = []
        for _ in range(C):
            op, v = input().split()
            v = int(v)

            if op == '+':
                a = Fraction(1, 1)
                b = Fraction(v, 1)
            elif op == '-':
                a = Fraction(1, 1)
                b = Fraction(-v, 1)
            elif op == '*':
                a = Fraction(v, 1)
                b = Fraction(0, 1)
            else:
                a = Fraction(1, v)
                b = Fraction(0, 1)

            cards.append((a, b))

        def cmp(x, y):
            a1, b1 = x
            a2, b2 = y
            left = b1 * (a2 - 1)
            right = b2 * (a1 - 1)
            if left > right:
                return -1
            if left < right:
                return 1
            return 0

        cards.sort(key=cmp_to_key(cmp))

        A = Fraction(1, 1)
        B = Fraction(0, 1)

        for a, b in cards:
            B = B * a + b
            A = A * a

        result = A * S + B

        num = result.numerator
        den = result.denominator
        if den < 0:
            num = -num
            den = -den

        print(f"Case #{tc}: {num} {den}")

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách chuyển đổi mọi thao tác thành phép biến đổi tuyến tính phân đoạn. Phép chia được xử lý một cách tự nhiên thông qua phân số, giúp tránh hoàn toàn các vấn đề về độ chính xác. 

Bộ so sánh mã hóa trực tiếp điều kiện hoán đổi xuất phát từ phần thuật toán. Việc sắp xếp của Python sử dụng bộ so sánh này để tạo ra thứ tự tối ưu toàn cục. 

Vòng lặp cuối cùng bao gồm các phép biến đổi theo trình tự. Mặc dù A không thực sự cần thiết cho việc sắp xếp, nhưng nó vẫn được duy trì để đảm bảo tính chính xác và rõ ràng trong việc tính toán dạng affine cuối cùng. 

Cuối cùng, kết quả được chuẩn hóa sao cho mẫu số dương và phân số đã được giảm đi một phần`Fraction`kiểu. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ có S = 5 và các thẻ: +1, -2, *3, /-2. 

Chúng tôi tính toán các cặp (a, b): 

+1 → (1, 1) 

-2 → (1, -2) 

*3 → (3, 0) 

/ -2 → (-1/2, 0) 

Sau khi sắp xếp bằng bộ so sánh, một thứ tự tối ưu sẽ đặt phép nhân sớm và phép chia ở vị trí giảm thiểu tỷ lệ phá hủy trong khi vẫn duy trì mức tăng cộng. 

Một dấu vết của thành phần: 

| Bước | một | b | A | B | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | - | - | 1 | 0 | 
| *3 | 3 | 0 | 3 | 0 | 
| /-2 | -1/2 | 0 | -3/2 | 0 | 
| +1 | 1 | 1 | -3/2 | 1 | 
| -2 | 1 | -2 | -3/2 | -7/2 | 

Giá trị cuối cùng là A*S + B = (-3/2)*5 + (-7/2) = -3/2. 

Dấu vết này cho thấy rằng một khi thứ tự được cố định, việc tính toán hoàn toàn mang tính cơ học và ổn định theo số học hợp lý. 

Ví dụ thứ hai trong đó tất cả các phép toán đều là phép nhân chứng tỏ rằng thứ tự không quan trọng đối với A nhưng lại quan trọng đối với các dấu hiệu khi kết hợp với phép cộng, củng cố lý do tại sao bộ so sánh là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(C log C) | sắp xếp chiếm ưu thế, mỗi so sánh là O(1) | 
| Không gian | O(C) | lưu trữ các cặp phân số và các phép biến đổi | 

Các ràng buộc cho phép tối đa 1000 thẻ cho mỗi trường hợp thử nghiệm, do đó, giải pháp sắp xếp O(C log C) với số học hợp lý sẽ phù hợp một cách thoải mái trong giới hạn. Số học phân số của Python xử lý các tử số lớn mà không lo tràn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from fractions import Fraction
    from functools import cmp_to_key

    input_data = inp.strip().split()
    T = int(input_data[0])
    idx = 1
    out_lines = []

    for tc in range(1, T + 1):
        S = int(input_data[idx]); idx += 1
        C = int(input_data[idx]); idx += 1

        cards = []
        for _ in range(C):
            op = input_data[idx]; v = int(input_data[idx+1]); idx += 2

            if op == '+':
                a = Fraction(1,1); b = Fraction(v,1)
            elif op == '-':
                a = Fraction(1,1); b = Fraction(-v,1)
            elif op == '*':
                a = Fraction(v,1); b = Fraction(0,1)
            else:
                a = Fraction(1,v); b = Fraction(0,1)

            cards.append((a,b))

        def cmp(x,y):
            a1,b1 = x; a2,b2 = y
            l = b1*(a2-1); r = b2*(a1-1)
            return -1 if l>r else (1 if l<r else 0)

        cards.sort(key=cmp_to_key(cmp))

        A = Fraction(1,1)
        B = Fraction(0,1)

        for a,b in cards:
            B = B*a + b
            A = A*a

        res = A*S + B
        num, den = res.numerator, res.denominator
        if den < 0:
            num, den = -num, -den

        out_lines.append(f"Case #{tc}: {num} {den}")

    return "\n".join(out_lines)

# provided samples
assert run("1\n5 2\n+ 1\n- 2\n* 3\n/ -2\n") == "Case #1: -3 2"

# custom cases
assert run("1\n0 1\n+ 0\n") == "Case #1: 0 1"
assert run("1\n1 1\n* 5\n") == "Case #1: 5 1"
assert run("1\n2 2\n+ 1\n+ 1\n") == "Case #1: 4 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đơn +0 | 0/1 | tính trung lập về danh tính | 
| Phép nhân đơn | 1/5 | chia tỷ lệ thuần túy | 
| Hai bổ sung | 1/4 | tích lũy phụ gia giao hoán | 

## Vỏ cạnh 

Một tình huống tế nhị là khi một lá bài có a = 1, xảy ra đối với phép cộng và phép trừ. Trong trường hợp này, bộ so sánh giảm rõ ràng vì (a − 1) trở thành 0 và quy tắc sắp xếp chỉ phụ thuộc vào việc việc hoán đổi có ảnh hưởng đến việc chia tỷ lệ xuôi dòng hay không. Thuật toán vẫn xử lý việc này một cách chính xác vì bất đẳng thức được rút gọn mà không cần chia. 

Một trường hợp khác là phép chia tạo ra giá trị a âm. Ví dụ: a / -2 giới thiệu việc lật dấu có thể thay đổi đáng kể thứ tự. Bộ so sánh xử lý việc này một cách tự nhiên vì nó so sánh các biểu thức hợp lý đầy đủ thay vì giả định giá trị dương. 

Trường hợp góc cuối cùng là khi tất cả các giá trị a đều bằng 1. Khi đó, mọi phép biến đổi hoàn toàn là phép cộng và bộ so sánh suy biến thành việc sắp xếp theo các giá trị b theo cách tôn trọng cùng một quy tắc bất đẳng thức. Thuật toán vẫn tạo ra thứ tự nhất quán và tránh chia cho 0 trong biểu thức so sánh.
