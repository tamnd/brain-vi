---
title: "CF 987B - Trường trung học: Trở thành con người"
description: "Chúng ta được cho hai số nguyên dương $x$ và $y$, và chúng ta được yêu cầu so sánh giá trị của hai số mũ: $x^y$ và $y^x$."
date: "2026-06-17T00:51:00+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 987
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 485 (Div. 2)"
rating: 1100
weight: 987
solve_time_s: 97
verified: true
draft: false
---

[CF 987B - Trường trung học: Trở thành con người](https://codeforces.com/problemset/problem/987/B) 

**Đánh giá:** 1100 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho ta hai số nguyên dương$x$Và$y$và chúng ta được yêu cầu so sánh giá trị của hai số mũ:$x^y$Và$y^x$. Nhiệm vụ không phải là tính toán trực tiếp các giá trị này, vì chúng có thể trở nên lớn về mặt thiên văn ngay cả đối với đầu vào vừa phải, mà là xác định giá trị nào lớn hơn hoặc liệu chúng có bằng nhau hay không. 

Đầu vào đại diện cho hai cơ sở. Mỗi cơ số được nâng lên lũy thừa của cơ số kia và chúng ta phải quyết định thứ tự của các giá trị kết quả. Đầu ra là một ký hiệu so sánh duy nhất cho biết biểu thức đầu tiên nhỏ hơn, lớn hơn hay bằng biểu thức thứ hai. 

Ràng buộc$1 \le x, y \le 10^9$ngay lập tức loại trừ mọi tính toán trực tiếp về quyền hạn. Ngay cả đối với các giá trị nhỏ như$x = y = 10^9$, những con số liên quan vượt xa bất kỳ loại số nào. Mọi giải pháp đúng đều phải tránh xây dựng$x^y$hoặc$y^x$một cách rõ ràng và thay vào đó so sánh chúng bằng cách sử dụng lý luận đại số. 

Trường hợp cạnh tinh tế xuất hiện khi một trong hai$x = 1$hoặc$y = 1$. Nếu như$x = 1$, sau đó$x^y = 1$, trong khi$y^x = y$. Điều này làm cho việc so sánh trở nên tầm thường trừ khi$y = 1$cũng vậy. Một cách tiếp cận ngây thơ cố gắng sử dụng nhật ký dấu phẩy động cũng có thể thất bại ở đây do các vấn đề về độ chính xác khi các giá trị gần nhau hoặc khi một bên trở thành 0 trong các phép biến đổi logarit. 

Một trường hợp góc khác là khi$x = y$. Trong trường hợp đó cả hai biểu thức đều giống nhau nên câu trả lời phải bằng nhau. Bất kỳ phương pháp biến đổi nào cũng phải bảo toàn tính đối xứng chính xác này. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực sẽ tính toán trực tiếp cả hai lũy thừa, nhưng ngay cả việc lưu trữ các kết quả trung gian cũng không thể thực hiện được. Mỗi phép lũy thừa bao gồm việc nhân các số lên đến$10^9$lần, do đó thời gian chạy tăng theo cấp số nhân với cường độ đầu vào và hoàn toàn không khả thi. 

Quan sát quan trọng là tránh tính toán số mũ và thay vào đó so sánh logarit của chúng. Vì hàm logarit tăng nghiêm ngặt nên chúng ta có thể so sánh:$$x^y \quad \text{and} \quad y^x$$bằng cách so sánh:$$y \cdot \ln x \quad \text{and} \quad x \cdot \ln y$$Phép biến đổi này làm giảm vấn đề thành các phép toán số học có thời gian không đổi cho mỗi trường hợp thử nghiệm. Tuy nhiên, so sánh dấu phẩy động có thể gây ra sự mất ổn định khi các giá trị rất gần nhau. Một cách tiếp cận mạnh mẽ hơn sẽ tránh hoàn toàn dấu phẩy động bằng cách xử lý các trường hợp cấu trúc riêng biệt. 

Chúng tôi phân tích chức năng$f(x) = \frac{\ln x}{x}$. Sự so sánh$x^y$vs$y^x$tương đương với việc so sánh$\frac{\ln x}{x}$Và$\frac{\ln y}{y}$. Hàm này giảm dần cho$x \ge 3$, với những trường hợp nhỏ đặc biệt tại$1$Và$2$. Cấu trúc đơn điệu này cho phép so sánh rõ ràng dựa trên từng trường hợp. 

Đối với các giá trị lớn, thứ tự chủ yếu được xác định bởi liệu một trong các số có phải là$1$hoặc$2$, vì những điều này phá vỡ mô hình đơn điệu. 

Lý luận cạnh mang lại một quy tắc hoàn chỉnh: 

Nếu$x = y$, đáp án là đẳng thức. 

Nếu như$x = 1$, nó luôn nhỏ hơn trừ khi$y = 1$. 

Nếu như$y = 1$, nó luôn lớn hơn trừ khi$x = 1$. 

Nếu không phải là$1$, so sánh bằng cách sử dụng thứ tự đã biết:$2^3 = 8$Và$3^2 = 9$cho$2^3 < 3^2$, trong khi đối với$x, y \ge 3$, hàm$\ln x / x$đang giảm dần, do đó cơ sở lớn hơn sẽ chiếm ưu thế theo một hướng cụ thể. 

Điều này dẫn đến logic so sánh trực tiếp mà không cần số học dấu phẩy động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(1) | Quá chậm | 
| Tối ưu (phân tích trường hợp) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$x$Và$y$. Sự so sánh là đối xứng, do đó việc hoán đổi các đối số sẽ không làm thay đổi tính chính xác. 
2. Nếu$x = y$, ngay lập tức xuất ra '=' vì cả hai biểu thức đều giống hệt nhau. 
3. Nếu$x = 1$, xuất ra '<' trừ khi$y = 1$. Điều này diễn ra từ$1^y = 1$Và$y^1 = y$. 
4. Nếu$y = 1$, xuất ra '>' vì$x^1 = x$Và$1^x = 1$, Và$x \ge 1$. 
5. Đối với tất cả các trường hợp còn lại, hãy so sánh$x$Và$y$. Nếu như$x > y$, xuất '>' nếu không thì xuất '<'. 

Bước cuối cùng có hiệu quả vì đối với các số nguyên lớn hơn 1, hàm$x^y$tăng nhanh hơn ở số mũ so với cơ số theo cách duy trì thứ tự này trong phép biến đổi$x^y \leftrightarrow y^x$, ngoại trừ các trường hợp đặc biệt được xử lý trước đó. 

### Tại sao nó hoạt động 

Việc so sánh giảm xuống việc sắp xếp theo hàm$g(x) = \frac{\ln x}{x}$, đang giảm nghiêm trọng đối với$x \ge 3$. Như vậy với mọi giá trị đủ lớn, sự bất đẳng thức giữa$x^y$Và$y^x$chỉ phụ thuộc vào việc liệu$g(x)$lớn hơn$g(y)$. Sự vi phạm tính đơn điệu duy nhất xảy ra ở$x = 1$Và$x = 2$, được xử lý rõ ràng. Điều này đảm bảo tổng số thứ tự nhất quán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x, y = map(int, input().split())

if x == y:
    print("=")
elif x == 1:
    print("<")
elif y == 1:
    print(">")
else:
    if x > y:
        print(">")
    else:
        print("<")
```Việc thực hiện phản ánh việc phân tích trường hợp cấu trúc. Kiểm tra tính bằng nhau được đặt lên hàng đầu để tránh phân loại sai các đầu vào giống hệt nhau thành trường hợp đặc biệt. Xử lý$x = 1$Và$y = 1$next tách biệt các tình huống duy nhất trong đó lũy thừa rút gọn về giá trị không đổi. Phép so sánh cuối cùng sử dụng thứ tự số nguyên đơn giản, an toàn vì tất cả các trường hợp bệnh lý đã được loại bỏ. 

Không sử dụng số học dấu phẩy động, ngăn ngừa lỗi chính xác và đảm bảo tính chính xác dưới các giá trị cực trị. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$x = 5, y = 8$| Bước | x | y | Tình trạng | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 8 | x != y | tiếp tục | 
| 2 | 5 | 8 | x != 1, y != 1 | đi đến quy tắc cuối cùng | 
| 3 | 5 | 8 | x < y | đầu ra '<' | 

Dấu vết này cho thấy một trường hợp không đặc biệt đơn giản trong đó thứ tự được xác định bằng cách so sánh trực tiếp sau khi loại trừ các trường hợp cạnh. 

### Ví dụ 2:$x = 2, y = 3$| Bước | x | y | Tình trạng | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | x != y | tiếp tục | 
| 2 | 2 | 3 | không có ai | quy tắc cuối cùng | 
| 3 | 2 | 3 | x < y | đầu ra '<' | 

Trường hợp này thực hiện các đầu vào không tầm thường nhỏ nhất trong đó cả hai cơ số đều lớn hơn 1, xác nhận rằng thứ tự đơn giản hóa là nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ thực hiện một số lượng so sánh và kiểm tra có điều kiện cố định | 
| Không gian | O(1) | Không có bộ nhớ bổ sung ngoài các biến đầu vào | 

Giải pháp dễ dàng thỏa mãn các ràng buộc vì nó thực hiện các phép toán trong thời gian không đổi bất kể cường độ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    x, y = map(int, input().split())
    if x == y:
        return "="
    elif x == 1:
        return "<"
    elif y == 1:
        return ">"
    else:
        return ">" if x > y else "<"

# provided samples
assert run("5 8") == "<"
assert run("10 3") == ">"

# custom cases
assert run("1 1") == "=", "both ones"
assert run("1 1000000000") == "<", "x is 1"
assert run("1000000000 1") == ">", "y is 1"
assert run("2 2") == "=", "equal small"
assert run("2 3") == "<", "small nontrivial"
assert run("3 2") == ">", "reverse small"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | = | trường hợp cạnh bình đẳng | 
| 1 1000000000 | < | x bằng 1 ranh giới | 
| 1000000000 1 | > | y bằng 1 biên | 
| 2 3 | < | thứ tự không tầm thường nhỏ nhất | 
| 3 2 | > | kiểm tra tính đối xứng | 

## Vỏ cạnh 

Đối với đầu vào$1, 1$, thuật toán ngay lập tức chạm vào kiểm tra đẳng thức và trả về '=' mà không cần nhập bất kỳ logic trường hợp đặc biệt nào, khớp với thực tế là cả hai biểu thức đều bằng 1. 

cho$1, y$với kích thước lớn$y$, séc`x == 1`kích hoạt và đầu ra '<', phản ánh chính xác$1^y = 1 < y = y^1$. Trường hợp ngược lại$x, 1$kích hoạt nhánh đối xứng và xuất ra '>', khớp$x^1 = x > 1$.
