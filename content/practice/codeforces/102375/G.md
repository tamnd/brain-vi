---
title: "CF 102375G - \u0415\u0441\u0442\u044c \u043b\u0438 \u0434\u0435\u043b\u0438\u0442\u0435\u043b\u044c?"
description: "Chúng ta được cấp một chuỗi thập phân khác rỗng, không có số 0 đứng đầu. Chuỗi này không nhất thiết phải được diễn giải theo cơ số 10. Chúng ta có thể chọn cơ số (B), miễn là mọi chữ số xuất hiện trong chuỗi đều là chữ số hợp lệ trong cơ số đó."
date: "2026-08-15T18:00:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102375
codeforces_index: "G"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u0421\u0435\u0432\u0435\u0440\u043e-\u0417\u0430\u043f\u0430\u0434\u0430 \u0420\u043e\u0441\u0441\u0438\u0438 \u0438 \u041c\u043e\u0441\u043a\u0432\u044b ICPC 2019"
rating: 0
weight: 102375
solve_time_s: 306
verified: false
draft: false
---

[CF 102375G - \u0415\u0441\u0442\u044c \u043b\u0438 \u0434\u0435\u043b\u0438\u0442\u0435\u043b\u044c?](https://codeforces.com/problemset/problem/102375/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 6s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi thập phân khác rỗng, không có số 0 đứng đầu. Chuỗi này không nhất thiết phải được diễn giải theo cơ số 10. Chúng ta có thể chọn cơ số (B), miễn là mọi chữ số xuất hiện trong chuỗi đều là chữ số hợp lệ trong cơ số đó. Giải thích cùng một dãy chữ số trong cơ số (B) sẽ tạo ra một số nguyên (D). 

Nhiệm vụ của chúng ta là chọn (B) và ước số thích hợp (X) của (D), với (1 < X < D), sao cho cả (B) và (X) lớn nhất là (10^9). Nếu không có cặp nào như vậy tồn tại, chúng ta in ra (-1). 

Độ dài có thể đạt tới (3\cdot10^6), do đó, bản thân dữ liệu đầu vào đã dài hàng triệu ký tự. Bất kỳ giải pháp nào liên tục quét toàn bộ chuỗi, thử nhiều cơ số hoặc thực hiện phân tích số nguyên trên số lượng lớn thu được đều không phù hợp. Về cơ bản, chúng tôi muốn một đường truyền tuyến tính qua các chữ số. Thực tế hữu ích là tổng của tất cả các chữ số tối đa là (9\cdot3\cdot10^6=27\cdot10^6), thấp hơn nhiều (10^9). Điều này mang lại cho chúng ta một ứng cử viên đủ nhỏ cho cả cơ số và số chia. 

Có một số trường hợp nhỏ áp dụng mù quáng công trình chính không thành công. Đối với đầu vào`1`, giá trị được biểu thị luôn là (1), bất kể cơ số, do đó không thể có ước số thích hợp và câu trả lời đúng là`-1`. Đối với đầu vào`4`, tổng các chữ số là (4), nhưng việc chọn tổng làm ước số sẽ cho (X=D=4), đây không phải là ước số thực sự. Câu trả lời đúng có thể là`10 2`. Đối với đầu vào`10`, tổng các chữ số là (1), do đó việc xây dựng dựa trên tổng các chữ số không thể sử dụng (X=1). Tuy nhiên, cơ số (10) cho (D=10) và (X=2) đúng. Những trường hợp này giải thích tại sao thuật toán cuối cùng tách các chuỗi có độ dài bằng một và trường hợp tổng các chữ số là một. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử cơ số (B), xây dựng số tương ứng (D) và tìm ước số (X). Ngay cả việc chỉ chọn trong số tất cả (10^9) cơ sở có thể cũng đã mang lại ít nhất (10^9) ứng cử viên. Việc đánh giá một cơ số từ một chuỗi có độ dài (n) yêu cầu xử lý các chữ số của nó, vì vậy chỉ cần kiểm tra mọi cơ số có thể sẽ tốn ít nhất (10^9n) phép toán chữ số. Ở độ dài tối đa, đây là ít nhất (3\cdot10^{15}) thao tác, trước khi thực hiện bất kỳ phép phân tích nhân tử nào. Phân tích nhân tử (D) thậm chí còn tệ hơn vì (D) có thể có hàng triệu chữ số thập phân. 

Lực lượng vũ phu hoạt động vì việc kiểm tra một cơ sở ứng cử viên và ước số là đơn giản, nhưng nó thất bại vì không gian của các cơ sở có thể là rất lớn và số lượng đại diện là rất lớn. Quan sát quan trọng là chúng ta không cần biết (D) một cách rõ ràng. 

Đối với một số được viết dưới dạng cơ số (B), việc rút gọn nó theo modulo (B-1) sẽ thay thế mọi lũy thừa của (B) bằng (1), bởi vì (B\equiv1\pmod{B-1}). Vì vậy, nếu các chữ số là (a_0,a_1,\ldots,a_{n-1}), thì 

[ 
D=\sum a_iB^i\equiv\sum a_i\pmod{B-1}. 
] 

Gọi tổng các chữ số là (S). Nếu chúng ta chọn 

[ 
B=S+1, 
] 

thì (B-1=S), do đó (D\equiv S\equiv0\pmod S). Do đó (X=S) tự động là ước số của (D). 

Đây là công trình trung tâm Vì (S) ít nhất là mọi chữ số riêng lẻ nên (B=S+1) là một cơ số hợp lệ. Vì (S\le27\cdot10^6), cả (B) và (X) đều ở dưới (10^9). 

Đối với một chuỗi có độ dài ít nhất là hai và (S\ge2), (X=S) cũng nhỏ hơn (D). Chữ số đứng đầu ít nhất là một, vì vậy (D\ge B=S+1>S=X). 

Trường hợp duy nhất còn lại có ít nhất hai chữ số là (S=1). Vì chữ số đầu tiên khác 0 nên chuỗi phải là`100...0`. Chọn (B=10) sẽ có (D=10^{n-1}) và (X=2) là một ước số thích hợp. Cuối cùng, chuỗi một chữ số biểu thị chính xác chữ số đó, độc lập với cơ số. Một số như vậy chỉ có ước số thực sự khi bản thân chữ số đó là hợp số, trong số các chữ số được phép có nghĩa là (4,6,8,9). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ít nhất (O(10^9n)), cộng với hệ số hóa | Có khả năng rất lớn | Quá chậm | 
| Tối ưu | (O(n)) | (O(1)) bên cạnh chuỗi đầu vào | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi chữ số và tính độ dài và tổng các chữ số (S) của nó. Chỉ cần số tiền cho việc xây dựng chính, vì vậy không có lý do gì để xây dựng con số khổng lồ (D). 
2. Nếu chuỗi có một chữ số, hãy xử lý nó một cách riêng biệt. Biểu diễn một chữ số luôn có giá trị bằng chữ số đó, bất kể cơ số. Nếu chữ số là (4,6,) hoặc (8), hãy chọn (B=10) và (X=2). Nếu chữ số là (9), hãy chọn (B=10) và (X=3). Với (1,2,3,5,7), số đó là số nguyên tố nên in ra (-1). 
3. Nếu chuỗi có ít nhất hai chữ số và (S=1) thì chuỗi đó nhất thiết phải là`100...0`. Chọn (B=10) và (X=2). Giá trị được biểu thị là (10^{n-1}), chia hết cho (2) và lớn hơn (2). 
4. Ngược lại (n\ge2) và (S\ge2). Đặt (X=S) và (B=S+1). Cơ số hợp lệ vì mọi chữ số nhiều nhất là (S), do đó nhỏ hơn (B). 
5. Đầu ra (B) và (X). Vì (S\le27\cdot10^6), cả hai giá trị đều thỏa mãn giới hạn (10^9). Ngoài ra, (D>X), bởi vì (D\ge B=S+1). 

### Tại sao nó hoạt động 

Bất biến đằng sau cấu trúc chính là (B\equiv1\pmod{B-1}). Với (B=S+1), giá trị này trở thành (B\equiv1\pmod S). Do đó, mọi lũy thừa vị trí (B^k) đều đồng dạng với (1) modulo (S), do đó toàn bộ số được biểu thị thỏa mãn (D\equiv S\equiv0\pmod S). Do đó (X=S) luôn chia hết (D). 

Việc xây dựng cũng đáp ứng mọi giới hạn. Tổng các chữ số nhiều nhất là (27\cdot10^6), vì vậy (B=S+1\le27,000,001) và (X=S\le27,000,000). Vì (S) ít nhất là chữ số lớn nhất nên mọi chữ số đều hợp lệ trong cơ số (B). Đối với các chuỗi có độ dài ít nhất là hai, (D\ge B>X), do đó số chia là thích hợp. Các trường hợp đặc biệt bao gồm chính xác các tình huống trong đó (S) không thể đóng vai trò là ước số thực hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(s: str) -> str:
    n = len(s)
    digit_sum = sum(ord(c) - 48 for c in s)

    if n == 1:
        d = ord(s[0]) - 48

        if d in (4, 6, 8):
            return "10 2"
        if d == 9:
            return "10 3"
        return "-1"

    if digit_sum == 1:
        return "10 2"

    return f"{digit_sum + 1} {digit_sum}"

s = input().strip()
print(solve(s))
```Lần đầu tiên tính tổng các chữ số mà không chuyển đổi toàn bộ chuỗi thành số nguyên. Việc chuyển đổi một chuỗi có tối đa ba triệu chữ số thành một số nguyên sẽ không cần thiết và trong nhiều môi trường Python, phải tuân theo các giới hạn chuyển đổi bổ sung. 

Nhánh một chữ số tách biệt vì cơ sở vị trí không có tác dụng khi chỉ có một chữ số. Ví dụ,`4`đại diện cho (4) trong mọi cơ số hợp lệ, vì vậy chúng ta thực sự phải hỏi liệu bản thân (4) có ước số thực sự hay không. 

Đối với chuỗi nhiều chữ số có tổng chữ số là một, chữ số đầu tiên phải là một và mọi chữ số khác phải bằng 0. Cấu trúc cố định (B=10,X=2) hoạt động vì số được biểu thị là lũy thừa dương của mười. 

Ở ngành tổng hợp,`digit_sum + 1`là cơ sở và`digit_sum`là số chia. Mã không bao giờ tính toán (D), đây là lợi thế triển khai chính. Do đó, số nguyên Python chỉ được sử dụng cho các giá trị nhỏ được giới hạn bởi (27\cdot10^6+1), bất kể độ dài đầu vào. 

Không có vấn đề tràn trong Python và ngay cả trong ngôn ngữ có số nguyên có chiều rộng cố định, (B) và (X) được xây dựng dễ dàng nằm trong phạm vi số nguyên có dấu 32 bit. Chuỗi được quét một lần, do đó việc triển khai cũng tránh được mọi hành vi ẩn bậc hai. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với đầu vào`1`, giá trị đại diện duy nhất là (1). 

| Đầu vào | Chiều dài | Tổng chữ số | Cơ sở được chọn | Số chia được chọn | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
|`1`| 1 | 1 | không | không |`-1`| 

Trường hợp một chữ số được xử lý trước khi xây dựng chung. Vì (1) không có ước số lớn hơn một nên không có đáp án nào tồn tại. 

### Mẫu 2 

Đối với đầu vào`4`, giá trị là (4) trong mọi cơ sở hợp lệ. 

| Đầu vào | Chiều dài | Tổng chữ số | Cơ sở được chọn | Số chia được chọn | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
|`4`| 1 | 4 | 10 | 2 |`10 2`| 

Đầu ra có nghĩa là chuỗi chữ số`4`được hiểu là (D=4) trong cơ số (10) và (2) là ước số thực sự của (4). Việc xây dựng tổng chữ số được cố tình không sử dụng ở đây vì nó sẽ chọn (X=4=D). 

### Mẫu 3 

Đối với đầu vào`19`, tổng các chữ số là (10), do đó công thức tổng quát cho (B=11) và (X=10). 

Trong cơ sở (11), 

[ 
D=1\cdot11+9=20, 
] 

và (20) chia hết cho (10). 

| Đầu vào | Chiều dài | Tổng chữ số | Cơ sở (B) | Số chia (X) | Giá trị (D) | 
| --- | --- | --- | --- | --- | --- | 
|`19`| 2 | 10 | 11 | 10 | 20 | 

Sự đồng đẳng cho (D\equiv10\equiv0\pmod{10}), chính xác như dự đoán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Chuỗi được quét một lần để tính tổng các chữ số của nó. | 
| Không gian | (O(n)) cho đầu vào, (O(1)) phụ trợ | Không có biểu diễn nào của (D) hoặc các cấu trúc lớn khác được tạo ra. | 

Với (n\le3\cdot10^6), một đường truyền tuyến tính duy nhất là phù hợp. Thuật toán chỉ thực hiện số học đơn giản trên mỗi ký tự và không bao giờ phân tích hoặc xây dựng giá trị có thể dài hàng triệu chữ số (D), do đó, các ràng buộc dễ dàng nằm trong phạm vi dự kiến. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve(s: str) -> str:
    n = len(s)
    digit_sum = sum(ord(c) - 48 for c in s)

    if n == 1:
        d = ord(s[0]) - 48

        if d in (4, 6, 8):
            return "10 2"
        if d == 9:
            return "10 3"
        return "-1"

    if digit_sum == 1:
        return "10 2"

    return f"{digit_sum + 1} {digit_sum}"

def run(inp: str) -> str:
    s = inp.strip()
    return solve(s)

# Provided samples
assert run("1\n") == "-1", "sample 1"
assert run("4\n") == "10 2", "sample 2"
assert run("19\n") == "11 10", "sample 3, another valid answer"

# Minimum-size prime digit
assert run("2\n") == "-1", "single-digit prime"

# Single-digit composite at the upper boundary
assert run("9\n") == "10 3", "single-digit composite"

# All digits equal
assert run("2222\n") == "9 8", "all-equal digits"

# Digit sum exactly 1, including the two-digit boundary
assert run("10\n") == "10 2", "sum-one boundary"

# Maximum possible length
s = "1" + "0" * (3_000_000 - 1)
assert run(s) == "10 2", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`|`-1`| Trường hợp nguyên tố một chữ số | 
|`9`|`10 3`| Tổng hợp một chữ số ở chữ số lớn nhất | 
|`2222`|`9 8`| Các chữ số lặp lại và cấu trúc chính (B=S+1) | 
|`10`|`10 2`| Ranh giới chính xác trong đó tổng chữ số là một | 
|`1`theo sau là (2.999.999) số không |`10 2`| Độ dài đầu vào tối đa và xử lý tuyến tính | 

## Vỏ cạnh 

Đối với đầu vào`1`, thuật toán sẽ ngay lập tức đi vào nhánh một chữ số. Giá trị của nó là (1) trong mọi cơ số có thể, nên không có (X>1) với (X<D). Đầu ra là chính xác`-1`. 

Đối với đầu vào`4`, nhánh một chữ số nhận ra (4) là hợp số và chọn (X=2). Cơ số (10) hợp lệ vì chữ số (4) nhỏ hơn (10) và giá trị được biểu thị vẫn giữ nguyên (4). Như vậy`10 2`thỏa mãn (2<4) và (2\mid4). 

Đối với đầu vào`9`, thuật toán sử dụng (X=3) thay vì (2), vì (3\mid9). Đầu ra`10 3`đại diện cho (D=9), do đó số chia là đúng. 

Đối với đầu vào`10`, tổng các chữ số là (1), khiến cho việc xây dựng chính không thể thực hiện được vì nó yêu cầu (X=1). Nhánh đặc biệt chọn (B=10), cho (D=10) và (X=2) là ước số thích hợp. Lý do tương tự áp dụng cho mọi đầu vào của biểu mẫu`100...0`có ít nhất hai chữ số. 

Đối với một đầu vào như`2222`, tổng các chữ số là (8) nên thuật toán chọn (B=9) và (X=8). Giá trị đại diện là 

[ 
2\cdot9^3+2\cdot9^2+2\cdot9+2=1622, 
] 

và (1622) chia hết cho (8). Đối số đồng đẳng đưa ra điều này mà không bao giờ xây dựng (D). 

Đối với đầu vào kích thước tối đa bao gồm`1`theo sau là (2.999.999) số 0, tổng các chữ số vẫn là (1). Do đó, thuật toán không cố gắng xử lý số lượng lớn được biểu diễn. Nó trở lại`10 2`ngay sau lần vượt qua tổng chữ số tuyến tính và giá trị được biểu thị là (10^{2,999,999}), giá trị này chắc chắn chia hết cho (2) và lớn hơn (2).
