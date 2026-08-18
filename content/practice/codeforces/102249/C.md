---
title: "CF 102249C - Ông X"
description: "Chúng ta được cung cấp một biểu thức Boolean hợp lệ chứa một biến x, phủ định của nó X, các hằng số 0 và 1, và các toán tử nhị phân &, Một ký tự đơn có thể được thay thế bằng một ký tự khác, nhưng các ký tự không thể được chèn hoặc xóa."
date: "2026-08-17T21:51:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102249
codeforces_index: "C"
codeforces_contest_name: "2019 Facebook Hacker Cup, Qualification Round"
rating: 0
weight: 102249
solve_time_s: 124
verified: true
draft: false
---

[CF 102249C - Ông X](https://codeforces.com/problemset/problem/102249/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu thức Boolean hợp lệ chứa một biến,`x`, sự phủ định của nó`X`, các hằng số`0`Và`1`và các toán tử nhị phân`&`,`|`, Và`^`. Dấu ngoặc đơn xác định cây biểu thức. 

Một ký tự đơn có thể được thay thế bằng một ký tự khác nhưng không thể chèn hoặc xóa các ký tự. Sau tất cả các sửa đổi, kết quả vẫn phải là một biểu thức hợp lệ. Mục đích là làm cho giá trị của toàn bộ biểu thức độc lập với`x`, sử dụng càng ít sửa đổi càng tốt. 

Vì chỉ có một biến Boolean nên mỗi biểu thức chỉ biểu thị một trong bốn hàm Boolean có thể có:`0`, điều này luôn luôn sai.`1`, điều này luôn đúng.`x`, điều này sai đối với`x = 0`và đúng cho`x = 1`.`X`, điều này đúng với`x = 0`và sai cho`x = 1`. 

Điều này ngay lập tức gợi ý việc đánh giá biểu thức hai lần, một lần với`x = 0`và một lần với`x = 1`. Nếu hai kết quả bằng nhau thì biểu thức đã độc lập với`x`, vì vậy câu trả lời là không. 

Phần thú vị là chứng minh rằng nếu hai kết quả khác nhau thì chỉ cần sửa đổi một lần là đủ. Việc quan sát đó loại bỏ sự cần thiết của một chương trình động lớn hoặc tìm kiếm các sửa đổi có thể có. 

Biểu thức có độ dài tối đa là 300 và có tối đa 500 trường hợp thử nghiệm. Việc quét tuyến tính cho mỗi trường hợp thử nghiệm có thể dễ dàng thực hiện được. Ngay cả một thuật toán bậc hai thường đủ nhỏ cho các giới hạn này, nhưng cấu trúc của bài toán cho phép chúng ta làm tốt hơn nhiều. Quan trọng hơn, việc tìm kiếm theo cấp số nhân trên các biểu thức đã sửa đổi là hoàn toàn không thể. Bảng chữ cái biểu thức chứa chín ký tự có thể có, do đó, việc liệt kê tất cả các chuỗi có độ dài 300 sẽ yêu cầu xem xét tối đa (9^{300}), khoảng (3,4 \times 10^{286}), các ký tự. 

Có một số trường hợp đặc biệt có thể đánh lừa quá trình triển khai chuyển quá nhanh sang quan sát biểu thức nhị phân. 

Đối với biểu thức một ký tự`x`, giá trị thay đổi với`x`, vậy câu trả lời là`1`. Việc triển khai bất cẩn giả định mọi biểu thức đều có toán tử gốc sẽ thất bại vì không có toán tử nào để sửa đổi. 

Đối với biểu thức một ký tự`X`, lý do tương tự được áp dụng. Thay đổi nó thành`0`hoặc`1`cần chính xác một sửa đổi, vì vậy câu trả lời là`1`. 

Vì`0`, biểu thức đã không đổi nên câu trả lời là`0`. Một triển khai trả về một cách mù quáng`1`bất cứ khi nào biểu thức có độ dài thì sẽ sai. 

Vì`(x&X)`, biểu thức luôn luôn sai, bởi vì`x`Và`X`cả hai đều không thể đúng. Hai đánh giá của nó đều bằng 0, vì vậy câu trả lời là`0`. 

Vì`(x^X)`, biểu thức luôn đúng vì chính xác một trong`x`Và`X`là đúng. Một lần nữa câu trả lời là`0`. 

Những trường hợp này cho thấy tại sao trước tiên chúng ta nên xác định xem biểu thức ban đầu đã độc lập với`x`, thay vì ngay lập tức cho rằng cần phải sửa đổi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử mọi chuỗi có thể có được bằng cách thay đổi ký tự, kiểm tra xem chuỗi kết quả có phải là biểu thức hợp lệ hay không, đánh giá nó cho cả hai giá trị của`x`và giữ số lượng vị trí thay đổi tối thiểu. Điều này đúng vì mọi chuỗi sửa đổi được phép đều tương ứng với một chuỗi ứng cử viên, do đó việc tìm kiếm toàn diện cuối cùng sẽ xem xét một câu trả lời tối ưu. 

Vấn đề là số lượng ứng viên. Có chín ký tự có sẵn trong bảng chữ cái biểu thức:`x`,`X`,`0`,`1`,`|`,`&`,`^`,`(`, Và`)`. Nếu mọi vị trí được phép chứa bất kỳ vị trí nào trong số chúng thì sẽ có (9^n) chuỗi có độ dài (n). Ở độ dài tối đa (n=300), tức là khoảng (3,4 \times 10^{286}) chuỗi. Ngay cả việc kiểm tra một ứng cử viên trong thời gian không đổi cũng sẽ vô vọng và thực sự việc xác thực và đánh giá một ứng cử viên có chi phí (O(n)), khiến tổng công việc xấp xỉ (O(n9^n)). 

Lực lượng vũ phu hoạt động vì câu trả lời được xác định bởi khoảng cách Hamming tối thiểu đến một biểu thức hằng số hợp lệ, nhưng nó không thành công vì không gian của các chuỗi có thể là rất lớn. Quan sát quan trọng là chúng ta thực sự không cần phải xây dựng một biểu thức đã sửa đổi. 

Giả sử biểu thức ban đầu đã không đổi. Vậy thì câu trả lời rõ ràng là bằng không. 

Bây giờ giả sử giá trị của nó phụ thuộc vào`x`. Chỉ có hai khả năng cấu trúc. 

Nếu biểu thức bao gồm một thuật ngữ duy nhất thì nó phải là`x`hoặc`X`, bởi vì`0`Và`1`đã là hằng số rồi. Thay đổi một ký tự đó thành`0`hoặc`1`tạo ra một biểu thức không đổi. Vì vậy, một sửa đổi là đủ. 

Nếu biểu thức chứa phép toán nhị phân, hãy xem xét gốc của nó. Gốc có dạng`(A op B)`, Ở đâu`A`Và`B`là những biểu thức hợp lệ. Mỗi hàm con đại diện cho một trong bốn hàm Boolean được liệt kê trước đó. 

Đối với hai hàm Boolean bất kỳ của một biến, ít nhất một trong`&`,`|`, Và`^`làm cho sự kết hợp của chúng không đổi. Chúng ta có thể thấy điều này trực tiếp từ các hình thức có thể. 

Nếu một trong hai toán hạng là hằng số`0`, đang chọn`&`làm cho kết quả`0`. 

Nếu một trong hai toán hạng là hằng số`1`, đang chọn`|`làm cho kết quả`1`. 

Nếu hai toán hạng là cùng một hàm không cố định, việc chọn`^`khiến họ hủy bỏ. Ví dụ,`x^x = 0`Và`X^X = 0`. 

Nếu các toán hạng là`x`Và`X`, đang chọn`&`cho`0`, trong khi chọn`|`hoặc`^`cho`1`. 

Vì vậy, bất cứ khi nào một biểu thức nhị phân chưa phải là hằng số, chỉ cần thay đổi toán tử gốc của nó là đủ để làm cho nó không đổi. Chúng tôi thậm chí không cần phải tìm toán tử nào nên được sử dụng vì bài toán chỉ yêu cầu số lượng sửa đổi tối thiểu. 

Do đó, toàn bộ vấn đề rút gọn thành một câu hỏi: biểu thức ban đầu có đánh giá cùng một giá trị Boolean cho`x = 0`Và`x = 1`? Nếu có, hãy trả lời`0`. Còn không thì trả lời`1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n9^n)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích biểu thức và đánh giá mọi biểu thức con cho cả hai giá trị có thể có của`x`. Biểu diễn kết quả của một biểu thức con dưới dạng một cặp`(value_when_x_is_0, value_when_x_is_1)`. Ví dụ,`x`là`(0, 1)`,`X`là`(1, 0)`,`0`là`(0, 0)`, Và`1`là`(1, 1)`. 
2. Khi trình phân tích cú pháp gặp biểu thức nhị phân`(A op B)`, đệ quy thu được hai cặp kết quả cho`A`Và`B`. Áp dụng cùng một toán tử Boolean một cách độc lập cho hai tọa độ. Điều này hoạt động vì biểu thức được đánh giá độc lập cho`x = 0`Và`x = 1`. 
3. Sau khi phân tích cú pháp biểu thức hoàn chỉnh, hãy kiểm tra cặp kết quả. Nếu cả hai tọa độ đều bằng nhau thì biểu thức có cùng giá trị cho cả hai giá trị có thể có của`x`, vì vậy nó đã không đổi và câu trả lời là`0`. 
4. Nếu tọa độ khác nhau thì biểu thức không cố định. Nếu biểu thức là một ký tự đơn thì nó phải là`x`hoặc`X`, và thay đổi nó thành`0`hoặc`1`làm cho nó không đổi trong một lần chỉnh sửa. 
5. Nếu biểu thức là nhị phân, chỉ thay đổi toán tử gốc của nó. Đối với hai hàm con, ít nhất một trong`&`,`|`, Và`^`tạo ra một hàm không đổi, vì vậy chỉ cần chỉnh sửa một lần là đủ. 
6. Vì một biểu thức không cố định không thể yêu cầu chỉnh sửa bằng 0 và chỉ cần chỉnh sửa một lần là đủ, nên xuất ra`1`trong trường hợp không cố định. 

### Tại sao nó hoạt động 

Trình phân tích cú pháp tính toán hàm Boolean chính xác được biểu thị bằng mọi biểu thức con, được mã hóa bởi các giá trị của nó tại`x = 0`Và`x = 1`. Do đó, cặp gốc bằng nhau chính xác khi biểu thức ban đầu độc lập với`x`. 

Nếu cặp bằng nhau thì việc sửa đổi bằng 0 là đủ và tối ưu. Nếu nó khác, thì không thể sửa đổi bằng 0, do đó cần có ít nhất một sửa đổi. Giải pháp một ký tự luôn tồn tại: đối với một chiếc lá, hãy thay thế`x`hoặc`X`với một hằng số; đối với biểu thức nhị phân, hãy thay thế toán tử gốc bằng toán tử làm cho hai hàm con kết hợp thành một hằng số. Do đó mọi biểu thức không cố định đều có đáp án tối thiểu chính xác là một. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def evaluate(expr):
    n = len(expr)
    pos = 0

    def parse():
        nonlocal pos

        c = expr[pos]

        if c == '0':
            pos += 1
            return (0, 0)

        if c == '1':
            pos += 1
            return (1, 1)

        if c == 'x':
            pos += 1
            return (0, 1)

        if c == 'X':
            pos += 1
            return (1, 0)

        # c == '('
        pos += 1

        left = parse()
        op = expr[pos]
        pos += 1
        right = parse()

        pos += 1  # ')'

        a0, a1 = left
        b0, b1 = right

        if op == '&':
            return (a0 & b0, a1 & b1)

        if op == '|':
            return (a0 | b0, a1 | b1)

        return (a0 ^ b0, a1 ^ b1)

    return parse()

def solve():
    t = int(input())
    out = []

    for case in range(1, t + 1):
        expr = input().strip()
        v0, v1 = evaluate(expr)

        if v0 == v1:
            ans = 0
        else:
            ans = 1

        out.append(f"Case #{case}: {ans}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`evaluate`hàm là một trình phân tích cú pháp gốc đệ quy. Vì đầu vào được đảm bảo là một biểu thức hợp lệ nên trình phân tích cú pháp không bao giờ cần khôi phục từ cú pháp không đúng định dạng. 

Đối với ký tự đầu cuối, hàm sẽ nâng vị trí chung lên một đơn vị và trả về hai giá trị Boolean của nó. Việc biểu diễn cặp làm cho`x`Và`X`đặc biệt tiện lợi:`x`trở thành`(0, 1)`Và`X`trở thành`(1, 0)`. 

Khi trình phân tích cú pháp nhìn thấy`(`, đầu tiên nó phân tích biểu thức bên trái, sau đó đọc chính xác một toán tử, sau đó phân tích biểu thức bên phải và cuối cùng sử dụng phần đóng`)`. Thứ tự rất quan trọng vì ngữ pháp diễn đạt chính xác`(left operator right)`. 

Hai con được kết hợp tọa độ theo tọa độ. Ví dụ, nếu đứa trẻ bên trái là`(0, 1)`và đứa trẻ bên phải là`(1, 0)`, thì họ`&`kết quả là`(0 & 1, 1 & 0) = (0, 0)`. 

Không cần phải xây dựng biểu thức một cách rõ ràng sau khi sửa đổi giả thuyết. Bằng chứng ở trên đảm bảo rằng một biểu thức không cố định luôn có thể được giữ nguyên chỉ bằng một lần chỉnh sửa. Do đó, mã chỉ cần cặp cuối cùng và không cần xác định ký tự nào thực sự sẽ được thay đổi. 

Độ sâu lồng tối đa được giới hạn bởi độ dài biểu thức, tối đa là 300, do đó giới hạn đệ quy của Python là đủ thoải mái. Không có phép tính số nguyên lớn nên việc tràn số nguyên là không liên quan. 

## Ví dụ đã hoạt động 

Hãy xem xét biểu thức mẫu được cung cấp đầu tiên,`X`. 

| Vị trí phân tích cú pháp | Nhân vật | Cặp trả lại | 
| --- | --- | --- | 
| 0 |`X`|`(1, 0)`| 
| kết thúc | biểu thức hoàn chỉnh |`(1, 0)`| 

Hai giá trị này khác nhau nên biểu thức phụ thuộc vào`x`. Vì đây là một thuật ngữ duy nhất nên nó có thể được thay đổi trực tiếp thành`0`hoặc`1`. Không có giải pháp nào có thể sử dụng sửa đổi bằng 0 vì biểu thức ban đầu không phải là hằng số, vì vậy câu trả lời là`1`. 

Hãy xem xét biểu thức mẫu được cung cấp thứ hai,`(x|1)`. 

| Vị trí phân tích cú pháp | Nhân vật hoặc biểu hiện phụ | Cặp trả lại | 
| --- | --- | --- | 
| 1 |`x`|`(0, 1)`| 
| 3 |`1`|`(1, 1)`| 
| 0 |`(x | 1)`|`(0 | 1, 1 | 1)`| 
| kết thúc | biểu thức hoàn chỉnh |`(1, 1)`| 

Cả hai đánh giá đều`1`. Biểu thức đã không đổi vì OR với`1`luôn luôn cho`1`, vì vậy không cần thay đổi ký tự nào. Câu trả lời là`0`. 

Đối với một dấu vết hữu ích khác, hãy xem xét`(x&X)`. 

| Vị trí phân tích cú pháp | Nhân vật hoặc biểu hiện phụ | Cặp trả lại | 
| --- | --- | --- | 
| 1 |`x`|`(0, 1)`| 
| 3 |`X`|`(1, 0)`| 
| 0 |`(x&X)`|`(0 & 1, 1 & 0)`| 
| kết thúc | biểu thức hoàn chỉnh |`(0, 0)`| 

Biểu thức không đổi mặc dù cả hai toán hạng đều phụ thuộc vào`x`. Điều này chứng tỏ tại sao chỉ kiểm tra xem các lá có chứa biến hay không là không đủ. Điều quan trọng là hàm Boolean được tạo ra bởi toàn bộ cây biểu thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Mỗi ký tự được phân tích cú pháp chính xác một lần và mỗi toán tử nhị phân được xử lý một lần. | 
| Không gian | (O(n)) | Ngăn xếp cuộc gọi đệ quy chứa tối đa một khung hình cho mỗi cấp độ lồng biểu thức. | 

Với (n \le 300), quá trình quét tuyến tính rất nhỏ ngay cả đối với 500 trường hợp thử nghiệm. Tổng lượng đầu vào cũng đủ nhỏ để trình phân tích cú pháp đệ quy đơn giản có thể thoải mái đáp ứng các giới hạn cuộc thi điển hình. 

Cải thiện hiệu quả quan trọng không phải là cấu trúc dữ liệu phức tạp. Nó xuất phát từ việc chứng minh rằng câu trả lời chỉ có thể là`0`hoặc`1`. Khi điều đó được thiết lập, việc đánh giá biểu thức ban đầu là tất cả các tính toán cần thiết. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    data = inp.strip().splitlines()
    t = int(data[0])
    out = []

    def evaluate(expr):
        pos = 0

        def parse():
            nonlocal pos

            c = expr[pos]

            if c == '0':
                pos += 1
                return (0, 0)

            if c == '1':
                pos += 1
                return (1, 1)

            if c == 'x':
                pos += 1
                return (0, 1)

            if c == 'X':
                pos += 1
                return (1, 0)

            pos += 1
            left = parse()
            op = expr[pos]
            pos += 1
            right = parse()
            pos += 1

            a0, a1 = left
            b0, b1 = right

            if op == '&':
                return (a0 & b0, a1 & b1)
            if op == '|':
                return (a0 | b0, a1 | b1)
            return (a0 ^ b0, a1 ^ b1)

        return parse()

    for case in range(1, t + 1):
        expr = data[case].strip()
        a, b = evaluate(expr)
        out.append(f"Case #{case}: {0 if a == b else 1}")

    return "\n".join(out)

# Provided samples appearing in the statement.
assert solve_data(
    """3
X
(x|1)
((1^(X&X))|x)
"""
) == """Case #1: 1
Case #2: 0
Case #3: 0""", "provided samples"

# Minimum-size expressions.
assert solve_data(
    """4
0
1
x
X
"""
) == """Case #1: 0
Case #2: 0
Case #3: 1
Case #4: 1""", "single-character expressions"

# Already constant despite containing variables.
assert solve_data(
    """4
(x&X)
(x^X)
((x&X)|x)
((x|X)&0)
"""
) == """Case #1: 0
Case #2: 0
Case #3: 1
Case #4: 0""", "constant and nonconstant compound expressions"

# Deep expression near the maximum allowed length.
expr = "x"
for _ in range(74):
    expr = "(" + expr + "&x)"

assert len(expr) == 297

assert solve_data(
    "1\n" + expr + "\n"
) == "Case #1: 1", "maximum-size valid nonconstant expression"

# Constant expression with many nested operations.
expr = "1"
for _ in range(74):
    expr = "(" + expr + "|0)"

assert len(expr) == 297

assert solve_data(
    "1\n" + expr + "\n"
) == "Case #1: 0", "maximum-size valid constant expression"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`,`1`,`x`,`X`|`0`,`0`,`1`,`1`| Biểu thức kích thước tối thiểu và xử lý lá | 
|`(x&X)`,`(x^X)`|`0`,`0`| Các lá không cố định khác nhau có thể kết hợp thành một hằng số | 
|`((x&X) | x)`|`1`| Một biểu thức ghép vẫn có thể phụ thuộc vào`x`sau một biểu thức con hằng số nội bộ | 
| Lồng nhau sâu`&`biểu thức độ dài 297 |`1`| Kích thước đầu vào lớn và phân tích đệ quy | 
| Lồng nhau sâu` | 0`biểu thức độ dài 297 |`0`| Kích thước đầu vào lớn với kết quả không đổi | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là một biến duy nhất,`x`. Trình phân tích cú pháp trả về`(0, 1)`, do đó hai tọa độ khác nhau. Vì biểu thức không chứa toán tử nhị phân nên không có toán tử gốc để sửa đổi. Thay đổi`x`ĐẾN`0`đưa ra một biểu thức không đổi trong một lần chỉnh sửa, do đó thuật toán trả về`1`. 

Trường hợp cạnh thứ hai là một hằng số duy nhất,`0`. Trình phân tích cú pháp trả về`(0, 0)`. Hai tọa độ bằng nhau nên biểu thức đã bị bỏ qua`x`và câu trả lời là`0`. Lý do tương tự áp dụng cho`1`. 

Trường hợp cạnh thứ ba là`(x&X)`. Bộ phân tích cú pháp đầu tiên thu được`(0, 1)`vì`x`Và`(1, 0)`vì`X`. Áp dụng`&`cho`(0, 0)`. Mặc dù cả hai lá đều phụ thuộc vào`x`, sự kết hợp của họ thì không. Thuật toán trả về`0`, tránh một cách chính xác một sửa đổi không cần thiết. 

Trường hợp cạnh thứ tư là`(x^X)`. Hai cặp con của nó lại là`(0, 1)`Và`(1, 0)`, nhưng XOR tạo ra`(1, 1)`. Do đó toàn bộ biểu thức luôn đúng và đáp án là`0`. Trường hợp này rất hữu ích vì một quy tắc đơn giản chẳng hạn như "hai toán hạng chứa biến hàm ý một kết quả phụ thuộc vào biến" sẽ không thành công. 

Trường hợp cạnh thứ năm là một biểu thức lồng nhau lớn chẳng hạn như liên tục gói biểu thức trước đó bằng`&x`. Mọi cấp độ vẫn đánh giá`x`, vậy cặp cuối cùng là`(0, 1)`. Biểu thức có độ dài 297, gần với độ dài biểu thức nhị phân hợp lệ tối đa có thể dưới giới hạn 300 ký tự. Trình phân tích cú pháp xử lý mọi cấp độ một lần, phát hiện các giá trị cuối cùng khác nhau và trả về`1`. 

Trường hợp cạnh cuối cùng là một biểu thức lớn được bao bọc nhiều lần bằng`|0`, bắt đầu từ`1`. Mọi cấp độ vẫn còn`1`, vậy cặp cuối cùng là`(1, 1)`bất kể độ sâu của nó. Thuật toán trả về`0`. Điều này xác nhận rằng câu trả lời phụ thuộc vào giá trị ngữ nghĩa của toàn bộ biểu thức chứ không phụ thuộc vào kích thước của nó hoặc số lượng ký tự biến mà nó chứa.
