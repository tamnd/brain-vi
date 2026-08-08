---
title: "CF 102565B - Python bị hỏng"
description: "Nhiệm vụ là đánh giá một biểu thức toán học được viết dưới dạng một chuỗi. Biểu thức chứa số nguyên, toán tử số học và dấu ngoặc đơn."
date: "2026-08-07T06:18:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102565
codeforces_index: "B"
codeforces_contest_name: "AGM 2020, Final Round, Day 2"
rating: 0
weight: 102565
solve_time_s: 406
verified: true
draft: false
---

[CF 102565B - Python bị hỏng](https://codeforces.com/problemset/problem/102565/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 46 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là đánh giá một biểu thức toán học được viết dưới dạng một chuỗi. Biểu thức chứa số nguyên, toán tử số học và dấu ngoặc đơn. Kết quả được yêu cầu tuân theo thứ tự ưu tiên toán học thông thường: đầu tiên là dấu ngoặc đơn, sau đó là lũy thừa, sau đó là phép nhân và chia, sau đó là phép cộng và phép trừ. Các toán tử có cùng mức độ ưu tiên sẽ được xử lý từ trái sang phải. 

Khó khăn không phải ở bản thân số học. Triển khai trực tiếp bằng cách sử dụng Python`eval`hoặc trình phân tích cú pháp đệ quy có thể không thành công vì độ dài biểu thức có thể đạt tới 100000 ký tự. Kích thước đó ngay lập tức loại trừ các phương pháp sao chép liên tục các chuỗi con lớn hoặc xây dựng cây phân tích đệ quy sâu. Giải pháp O(n) hoặc O(n log n) là mục tiêu thực tế vì mọi ký tự có thể cần được kiểm tra. 

Biểu thức cũng cho phép các giá trị trung gian là số hữu tỉ mặc dù đáp án cuối cùng là số nguyên. Một giải pháp thực hiện phép chia số nguyên sẽ âm thầm đưa ra đáp án sai. Ví dụ,`(1/2)*2`nên đánh giá để`1`, trong khi phép chia số nguyên sẽ biến`1/2`vào trong`0`. 

Một trường hợp nguy hiểm khác là lũy thừa. Đối với đầu vào`2^3^2`, nhiều ngôn ngữ lập trình không đồng ý về tính kết hợp, nhưng vấn đề này đảm bảo rằng phần mũ là số nguyên dương chứ không phải là biểu thức. Trình phân tích cú pháp xử lý các lũy thừa như phép nhân và phép chia mà không tôn trọng cấu trúc đã cho có thể đọc sai các biểu thức như`35^66*0/35`. 

Toán tử một ngôi cũng có thể là nguyên nhân gây ra lỗi. Câu lệnh đảm bảo chúng luôn được bao quanh bởi dấu ngoặc đơn, vì vậy một biểu thức như`(-5)`xuất hiện thay vì đứng tự do`-5`ở giữa quá trình phân tích cú pháp. Việc bỏ qua chi tiết này có thể dẫn đến việc xử lý sai số âm. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là liên tục tìm thao tác có mức độ ưu tiên cao nhất, tính toán, thay thế bằng kết quả của nó và tiếp tục cho đến khi chỉ còn lại một giá trị. Điều này phản ánh cách con người đánh giá các biểu thức và dễ dàng xác minh. Vấn đề là việc tìm kiếm và xây dựng lại biểu thức lặp đi lặp lại. Nếu mọi thao tác đều yêu cầu quét hầu hết chuỗi 100000 ký tự thì trường hợp xấu nhất sẽ trở thành phép tính bậc hai, khoảng 10^10 ký tự, vượt xa giới hạn. 

Quan sát quan trọng là biểu thức đã có ngữ pháp nghiêm ngặt. Thay vì liên tục tìm kiếm thao tác tiếp theo, chúng ta có thể quét biểu thức một lần trong khi vẫn duy trì trạng thái đánh giá hiện tại. Cách tiêu chuẩn để thực hiện việc này là sử dụng trình phân tích cú pháp dựa trên ngăn xếp. 

Trình phân tích cú pháp tách biểu thức thành các giá trị và toán tử. Bất cứ khi nào nhìn thấy một toán tử, độ ưu tiên của nó sẽ được so sánh với các toán tử đang chờ trên ngăn xếp. Các hoạt động có mức độ ưu tiên cao hơn sẽ được giải quyết ngay lập tức, trong khi các hoạt động có mức độ ưu tiên thấp hơn sẽ đợi cho đến khi có sẵn các giá trị cần thiết. Dấu ngoặc đơn đóng vai trò là điểm đánh dấu tạm thời chặn đánh giá cho đến khi dấu ngoặc đơn đóng phù hợp xuất hiện. 

Vì lũy thừa luôn có dạng số mũ đơn giản trong bài toán này nên chúng có thể được xử lý khi gặp phải mà không cần đến trình phân tích cú pháp đệ quy phức tạp hơn. Các giá trị trung gian hợp lý được lưu trữ chính xác bằng phân số, tránh lỗi dấu phẩy động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét biểu thức từ trái sang phải và mã hóa các số, toán tử và dấu ngoặc đơn. Khi tìm thấy một số nguyên đầy đủ, hãy chuyển đổi nó thành một giá trị phân số chính xác. Việc giữ các phân số chính xác ngay từ đầu sẽ ngăn ngừa các vấn đề về độ chính xác sau này. 
2. Duy trì hai ngăn xếp. Một ngăn xếp lưu trữ số và ngăn xếp khác lưu trữ các toán tử. Khi một số xuất hiện, hãy đẩy nó vào ngăn xếp số vì nó không thể được kết hợp cho đến khi toán tử kết nối nó với một giá trị khác. 
3. Khi một toán tử xuất hiện, hãy giải quyết các toán tử đã chờ trên ngăn xếp toán tử trong khi chúng có mức độ ưu tiên cao hơn hoặc bằng nhau. Quyền ưu tiên bằng nhau được giải quyết ngay lập tức vì phép nhân, chia, cộng và trừ được kết hợp. 
4. Khi dấu ngoặc đơn mở xuất hiện, hãy ấn nó làm điểm đánh dấu đặc biệt. Nó tách biểu thức bên trong khỏi các thao tác bên ngoài dấu ngoặc đơn. 
5. Khi dấu ngoặc đơn đóng xuất hiện, hãy áp dụng lặp lại các toán tử cho đến khi loại bỏ dấu ngoặc đơn mở phù hợp. Các giá trị bên trong dấu ngoặc đơn giờ đã trở thành một kết quả biểu thức hoàn chỉnh. 
6. Sau khi quá trình quét hoàn tất kết thúc, hãy áp dụng tất cả các toán tử còn lại trong ngăn xếp toán tử. Số duy nhất còn lại là giá trị cuối cùng. 
7. Chuyển phân số cuối cùng thành số nguyên và in ra. Vấn đề đảm bảo rằng chuyển đổi này là chính xác. 

Tại sao nó hoạt động: 

Tại mọi thời điểm trong quá trình quét, các ngăn xếp thể hiện chính xác phần biểu thức đã được đọc nhưng chưa thể hoàn thiện. Toán tử chỉ ở trên ngăn xếp toán tử trong khi mã thông báo trong tương lai vẫn có thể thay đổi thứ tự đánh giá. Bất cứ khi nào điều đó không thể thực hiện được nữa, toán tử sẽ được áp dụng ngay lập tức. Vì các toán tử được loại bỏ theo các quy tắc ưu tiên và kết hợp bắt buộc nên mọi thao tác được thực hiện theo cùng thứ tự như đánh giá toán học. 

## Giải pháp Python```python
import sys
from fractions import Fraction

input = sys.stdin.readline

def solve():
    s = input().strip()

    def precedence(op):
        if op == '^':
            return 3
        if op == '*' or op == '/':
            return 2
        if op == '+' or op == '-':
            return 1
        return 0

    def apply_op(nums, op):
        b = nums.pop()
        a = nums.pop()

        if op == '+':
            nums.append(a + b)
        elif op == '-':
            nums.append(a - b)
        elif op == '*':
            nums.append(a * b)
        elif op == '/':
            nums.append(a / b)
        else:
            nums.append(b ** a)

    nums = []
    ops = []

    i = 0
    n = len(s)

    while i < n:
        c = s[i]

        if c.isdigit():
            value = 0
            while i < n and s[i].isdigit():
                value = value * 10 + ord(s[i]) - ord('0')
                i += 1
            nums.append(Fraction(value))
            continue

        if c == '(':
            ops.append(c)

        elif c == ')':
            while ops[-1] != '(':
                apply_op(nums, ops.pop())
            ops.pop()

        else:
            while ops and ops[-1] != '(' and precedence(ops[-1]) >= precedence(c):
                apply_op(nums, ops.pop())
            ops.append(c)

        i += 1

    while ops:
        apply_op(nums, ops.pop())

    print(nums[0].numerator)

if __name__ == "__main__":
    solve()
```Trình phân tích cú pháp sử dụng`Fraction`vì các phép chia trung gian có thể tạo ra các giá trị không nguyên. Ví dụ, sau khi đọc`(1/2)`, ngăn xếp chứa giá trị hữu tỷ chính xác thay vì xấp xỉ dấu phẩy động được làm tròn. 

Trình phân tích cú pháp số đọc các chữ số liên tiếp theo cách thủ công. Điều này tránh tạo ra nhiều chuỗi con tạm thời và giữ cho quá trình quét tuyến tính. 

Hàm ưu tiên xác định khi nào một toán tử đang chờ trên ngăn xếp sẽ được thực thi. Việc so sánh sử dụng`>=`bởi vì các hoạt động có mức độ ưu tiên như nhau được đánh giá từ trái sang phải. 

Toán tử số mũ hơi khác so với các toán tử khác. Bài toán đảm bảo rằng số mũ đã là một giá trị nguyên dương, do đó việc áp dụng nó thông qua cùng một cơ chế ngăn xếp là an toàn. 

Số nguyên Python không bị tràn, điều này rất hữu ích vì các giá trị trung gian có thể lớn hơn nhiều so với câu trả lời cuối cùng. 

## Ví dụ đã hoạt động 

cho`1+1-1`: 

| Mã thông báo hiện tại | Ngăn xếp số | Ngăn xếp toán tử | Hành động | 
| --- | --- | --- | --- | 
|`1`| 1 | | Đẩy số | 
|`+`| 1 | + | Toán tử đẩy | 
|`1`| 1, 1 | + | Đẩy số | 
|`-`| 2 | - | Giải quyết`+`| 
|`1`| 2, 1 | - | Đẩy số | 
| kết thúc | 1 | | Giải quyết`-`| 

Ví dụ này cho thấy cách xử lý phép cộng và phép trừ từ trái sang phải. các`+`hoạt động được hoàn thành trước sau này`-`hoạt động được xem xét. 

Vì`35^66*0/35`: 

| Mã thông báo hiện tại | Ngăn xếp số | Ngăn xếp toán tử | Hành động | 
| --- | --- | --- | --- | 
|`35`| 35 | | Đẩy số | 
|`^`| 35 | ^ | Toán tử đẩy | 
|`66`| 35, 66 | ^ | Đẩy số | 
|`*`| giá trị lớn | * | Giải quyết sức mạnh | 
|`0`| giá trị lớn, 0 | * | Đẩy số | 
|`/`| 0 | / | Giải quyết phép nhân | 
|`35`| 0, 35 | / | Đẩy số | 
| kết thúc | 0 | | Giải quyết phép chia | 

Dấu vết cho thấy tại sao việc áp dụng các thao tác theo thứ tự ưu tiên lại quan trọng. Sức mạnh to lớn được tính toán trước tiên, nhưng sau đó nó được nhân với 0, tạo ra kết quả cuối cùng có thể quản lý được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được quét một lần và mỗi toán tử được đẩy và bật một lần. | 
| Không gian | O(n) | Các ngăn xếp có thể chứa tất cả các mã thông báo trong trường hợp xấu nhất. | 

Với độ dài biểu thức 100000 ký tự, trình phân tích cú pháp tuyến tính dễ dàng phù hợp với giới hạn thời gian. Việc sử dụng bộ nhớ cũng có thể chấp nhận được vì ngăn xếp chỉ lưu trữ mã thông báo từ đầu vào. 

## Trường hợp thử nghiệm```
from fractions import Fraction

def evaluate(inp):
    s = inp.strip()

    def precedence(op):
        if op == '^':
            return 3
        if op in '*/':
            return 2
        if op in '+-':
            return 1
        return 0

    def apply(nums, op):
        b = nums.pop()
        a = nums.pop()
        if op == '+':
            nums.append(a + b)
        elif op == '-':
            nums.append(a - b)
        elif op == '*':
            nums.append(a * b)
        elif op == '/':
            nums.append(a / b)
        else:
            nums.append(b ** a)

    nums, ops = [], []
    i = 0

    while i < len(s):
        if s[i].isdigit():
            x = 0
            while i < len(s) and s[i].isdigit():
                x = x * 10 + int(s[i])
                i += 1
            nums.append(Fraction(x))
            continue

        if s[i] == '(':
            ops.append(s[i])
        elif s[i] == ')':
            while ops[-1] != '(':
                apply(nums, ops.pop())
            ops.pop()
        else:
            while ops and ops[-1] != '(' and precedence(ops[-1]) >= precedence(s[i]):
                apply(nums, ops.pop())
            ops.append(s[i])
        i += 1

    while ops:
        apply(nums, ops.pop())

    return str(nums[0].numerator)

assert evaluate("1+1-1") == "1"
assert evaluate("35^66*0/35") == "0"
assert evaluate("(1/2)*2") == "1"

assert evaluate("1") == "1"
assert evaluate("5*5*5") == "125"
assert evaluate("10/(2+3)") == "2"
assert evaluate("(7-3)*(2+1)") == "12"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Xử lý số đơn | 
|`5*5*5`|`125`| Phép nhân từ trái sang phải | 
|`10/(2+3)`|`2`| Dấu ngoặc đơn và phép chia | 
|`(7-3)*(2+1)`|`12`| Đánh giá biểu thức lồng nhau | 

## Vỏ cạnh 

cho`(1/2)*2`, trước tiên trình phân tích cú pháp sẽ đánh giá phép chia bên trong dấu ngoặc đơn. Ngăn xếp số lưu trữ`1/2`chính xác thì phép nhân kết hợp nó với`2`để sản xuất`1`. Một giải pháp sử dụng phép chia số nguyên sẽ xuất ra không chính xác`0`. 

Vì`35^66*0/35`, trình phân tích cú pháp áp dụng phép toán lũy thừa trước phép nhân vì lũy thừa có mức độ ưu tiên cao hơn. Giá trị trung gian khổng lồ không gây ra vấn đề vì số nguyên Python hỗ trợ độ chính xác tùy ý và phép nhân sau đó với 0 sẽ làm giảm kết quả. 

Vì`(5)`, dấu ngoặc đơn mở được lưu dưới dạng rào cản và bị xóa khi dấu ngoặc đơn đóng xuất hiện. Thuật toán giữ nguyên số chứa trong đó, tạo ra`5`. 

Vì`8/4/2`, phép chia thứ nhất được áp dụng trước phép chia thứ hai vì các toán tử có mức độ ưu tiên bằng nhau được giải quyết ngay lập tức khi toán tử tiếp theo xuất hiện. Việc đánh giá trở nên`(8/4)/2`, cho`1`còn hơn là`4`. 

Tôi cũng có thể chuyển đổi định dạng này thành định dạng biên tập ngắn hơn theo phong cách Codeforces nếu bạn cần thứ gì đó gần giống với bài viết chính thức về cuộc thi.
