---
title: "CF 102440C - A + B = C"
description: "Chúng ta có chính xác ba số nguyên và chúng ta có thể sắp xếp lại chúng. Mục tiêu là in thứ tự x y z sao cho x + y = z. Nếu không có hoán vị nào có thuộc tính này thì in ra -1 -1 -1. Bất kỳ đơn đặt hàng hợp lệ đều được chấp nhận. Các giá trị đầu vào thỏa mãn -2^63 < ai < 2^63."
date: "2026-08-09T13:19:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102440
codeforces_index: "C"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Junior"
rating: 0
weight: 102440
solve_time_s: 530
verified: true
draft: false
---

[CF 102440C - A + B = C](https://codeforces.com/problemset/problem/102440/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8 phút 50 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có chính xác ba số nguyên và chúng ta có thể sắp xếp lại chúng. Mục đích là in đơn đặt hàng`x y z`như vậy`x + y = z`. Nếu không có hoán vị nào có thuộc tính này thì ta in`-1 -1 -1`. Bất kỳ đơn đặt hàng hợp lệ đều được chấp nhận. 

Các giá trị đầu vào thỏa mãn`-2^63 < a_i < 2^63`. Do đó, các số có thể gần với giới hạn của số nguyên 64 bit có dấu, do đó việc triển khai nên tránh các giả định rằng các giá trị phù hợp với loại số nguyên nhỏ hơn. Số nguyên Python có độ chính xác tùy ý, vì vậy số học như`a + b`an toàn ngay cả khi tổng toán học nằm ngoài phạm vi đầu vào. 

Chỉ có ba con số. Điều đó làm cho không gian tìm kiếm không đổi: có nhiều nhất sáu hoán vị và việc kiểm tra từng hoán vị cần có thời gian không đổi. Ngay cả việc liệt kê trực tiếp tất cả các hoán vị cũng đủ nhanh. Phần thú vị của vấn đề không phải là hiệu suất, mà là nhận ra rằng chỉ có ba lựa chọn khả thi để số đóng vai trò là`C`. 

Một số trường hợp đặc biệt có thể khiến việc triển khai bất cẩn không thành công. Đầu tiên, các số có thể chứa số không. Đối với đầu vào`0 0 0`, đầu ra đúng là`0 0 0`, bởi vì`0 + 0 = 0`. Việc triển khai coi số 0 là giá trị lỗi đặc biệt sẽ sai. 

Các giá trị trùng lặp cũng hợp lệ. Đối với đầu vào`1 1 2`, đầu ra`1 1 2`hoạt động vì`1 + 1 = 2`. Một giải pháp giả sử cả ba số phải khác biệt sẽ từ chối một trường hợp hợp lệ. 

Giá trị âm không yêu cầu xử lý đặc biệt. Đối với đầu vào`-5 2 -3`, đầu ra đúng là`-5 2 -3`, từ`-5 + 2 = -3`. Chỉ tìm kiếm số tiền dương sẽ bác bỏ nó một cách không chính xác. 

Cuối cùng, các giá trị đầu vào có thể ở gần ranh giới 64 bit. Ví dụ,`9223372036854775807 -1 9223372036854775806`có đơn đặt hàng hợp lệ. Việc triển khai có chiều rộng cố định thực hiện phép cộng tràn trước khi so sánh có thể tạo ra kết quả không chính xác, trong khi Python xử lý phép tính một cách chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là thử mọi thứ tự có thể có của ba số. có`3! = 6`hoán vị. Đối với mỗi hoán vị, chúng tôi kiểm tra xem hai giá trị đầu tiên có cộng lại với giá trị thứ ba hay không. Nếu một hoán vị thỏa mãn phương trình thì ta in ra ngay. Nếu cả sáu đều thất bại, không có thứ tự hợp lệ nào tồn tại. 

Phương pháp vũ phu này đã tối ưu cho vấn đề thực tế. Trường hợp xấu nhất của nó thực hiện chính xác sáu lần kiểm tra, vì vậy thời gian chạy của nó là`O(6)`, đơn giản là`O(1)`. Không có kích thước đầu vào nào có thể khiến sáu lần kiểm tra trở nên quá chậm. 

Chúng ta có thể làm cho khối lượng công việc không đổi thậm chí còn nhỏ hơn bằng cách nhận thấy rằng lựa chọn duy nhất có ý nghĩa là số nào trong ba số trở thành`C`. Nếu như`a`là`C`, hai cái còn lại phải là`b`Và`c`, vì vậy chúng tôi kiểm tra`b + c = a`. Tương tự, chúng tôi kiểm tra`a + c = b`Và`a + b = c`. Đây chính xác là ba phương trình có thể xảy ra và mỗi phương trình thành công sẽ ngay lập tức đưa ra thứ tự cần thiết. 

Cách tiếp cận vũ phu có hiệu quả vì không gian hoán vị rất nhỏ. Việc quan sát cấu trúc cho phép chúng ta tránh hoàn toàn việc tạo ra các hoán vị, giảm giải pháp thành ba phép so sánh số học. Cả hai cách tiếp cận đều được chấp nhận, nhưng phiên bản ba kiểm tra trực tiếp thì đơn giản hơn và làm cho lý do trở nên đặc biệt minh bạch. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1), nhiều nhất là 6 hoán vị | O(1) | Đã chấp nhận | 
| Tối ưu | O(1), 3 phương trình | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba số nguyên dưới dạng`a`,`b`, Và`c`. Chúng ta giữ nguyên thứ tự ban đầu vì mỗi lựa chọn trong số ba lựa chọn có thể có ở vế phải sau đó có thể được kiểm tra trực tiếp. 
2. Kiểm tra xem`a + b = c`. Nếu đúng thì in`a b c`. Đây`c`là phía bên phải được chọn, trong khi`a`Và`b`là hai phần bổ sung. 
3. Nếu không, hãy kiểm tra xem`a + c = b`. Nếu đúng thì in`a c b`. Điều này sử dụng`b`ở phía bên phải và đặt hai giá trị còn lại lên đầu tiên. 
4. Nếu không, hãy kiểm tra xem`b + c = a`. Nếu đúng thì in`b c a`. Đây là sự lựa chọn cuối cùng có thể có cho phía bên tay phải. 
5. Nếu không có phương trình nào trong ba phương trình đúng, hãy in`-1 -1 -1`. Mọi thứ tự có thể phải chọn chính xác một trong ba giá trị đầu vào làm giá trị thứ ba, vì vậy tất cả các trường hợp có thể xảy ra hiện đã hết. 

### Tại sao nó hoạt động 

Bất biến chính là mọi thứ tự hợp lệ phải chỉ định chính xác một số đầu vào ở phía bên phải`C`. Nếu như`C`là`c`, phương trình cần tìm là`a + b = c`. Nếu như`C`là`b`, đó là`a + c = b`. Nếu như`C`là`a`, đó là`b + c = a`. Thuật toán kiểm tra chính xác ba khả năng này, vì vậy bất cứ khi nào nó in ra một thứ tự hợp lệ, thứ tự đó sẽ thỏa mãn phương trình được yêu cầu. Nếu cả ba lần kiểm tra đều thất bại, không có lựa chọn nào`C`có thể hoạt động, có nghĩa là không hoán vị nào có thể là giải pháp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b, c = map(int, input().split())

    if a + b == c:
        print(a, b, c)
    elif a + c == b:
        print(a, c, b)
    elif b + c == a:
        print(b, c, a)
    else:
        print(-1, -1, -1)

if __name__ == "__main__":
    solve()
```Dòng đầu tiên của`solve`đọc chính xác ba số nguyên đầu vào. Không có trường hợp thử nghiệm nào để lặp lại vì vấn đề cung cấp một bộ ba. 

Điều kiện đầu tiên tương ứng với bước thuật toán đầu tiên và sử dụng`c`như kết quả. Hoán đổi điều kiện thứ hai`b`Và`c`ở đầu ra vì`b`bây giờ là kết quả. Điều kiện thứ ba diễn ra tương tự`a`cuối cùng. 

Việc sử dụng séc`elif`, vì vậy một khi tìm thấy thứ tự hợp lệ thì không có trường hợp nào sau này có thể thay thế nó. Điều này cũng xử lý các giá trị trùng lặp một cách chính xác. Ví dụ, với`0 0 0`, điều kiện đầu tiên thành công và chương trình in ra thứ tự ban đầu. 

Các số nguyên có độ chính xác tùy ý của Python rất hữu ích ở các ranh giới số. Các giá trị đầu vào có thể gần`2^63`và tổng trung gian có thể vượt quá phạm vi 64 bit đã ký, nhưng Python đánh giá tổng mà không bị tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với đầu vào`2 1 3`, thuật toán đánh giá các vế phải có thể có theo thứ tự ban đầu. 

|`a`|`b`|`c`| Kiểm tra | Kết quả | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | 3 |`2 + 1 = 3`| Đúng | 

Phương trình đầu tiên thành công ngay lập tức nên chương trình in ra`2 1 3`. Đầu ra mẫu sử dụng`1 2 3`, điều này cũng hợp lệ vì`1 + 2 = 3`. Vấn đề cho phép bất kỳ thứ tự hợp lệ. 

### Mẫu 2 

cho`2 4 42`, không giá trị nào trong ba giá trị có thể là tổng của hai giá trị còn lại. 

|`a`|`b`|`c`| Kiểm tra | Kết quả | 
| --- | --- | --- | --- | --- | 
| 2 | 4 | 42 |`2 + 4 = 42`| Sai | 
| 2 | 4 | 42 |`2 + 42 = 4`| Sai | 
| 2 | 4 | 42 |`4 + 42 = 2`| Sai | 

Tất cả các lựa chọn có thể có cho vế phải đều không thành công, do đó chương trình sẽ in`-1 -1 -1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chính xác ba phép cộng và so sánh được thực hiện trong trường hợp xấu nhất. | 
| Không gian | O(1) | Chỉ có ba số nguyên đầu vào được lưu trữ. | 

Đầu vào chỉ chứa ba số, do đó giải pháp thực hiện một lượng công việc cố định bất kể độ lớn của chúng. Việc sử dụng bộ nhớ cũng không đổi. Sự vắng mặt của một lượng lớn`n`có nghĩa là không có áp lực hiệu suất đáng kể nào ở đây và giải pháp này phù hợp thoải mái với mọi giới hạn tiêu chuẩn của Codeforce cho vấn đề này. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    a, b, c = map(int, input().split())

    if a + b == c:
        print(a, b, c)
    elif a + c == b:
        print(a, c, b)
    elif b + c == a:
        print(b, c, a)
    else:
        print(-1, -1, -1)

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_input = input

    try:
        sys.stdin = io.StringIO(inp)
        input = sys.stdin.readline

        old_stdout = sys.stdout
        sys.stdout = io.StringIO()
        try:
            solve()
            return sys.stdout.getvalue().strip()
        finally:
            sys.stdout = old_stdout
    finally:
        sys.stdin = old_stdin
        input = old_input

# Provided samples
assert run("2 1 3\n") == "2 1 3", "sample 1"
assert run("2 4 42\n") == "-1 -1 -1", "sample 2"

# All equal values
assert run("0 0 0\n") == "0 0 0", "all equal values"

# Maximum positive boundary
assert run("9223372036854775807 -1 9223372036854775806") == \
       "9223372036854775807 -1 9223372036854775806", \
       "maximum positive boundary"

# Minimum negative boundary
assert run("-9223372036854775807 1 -9223372036854775806") == \
       "-9223372036854775807 1 -9223372036854775806", \
       "minimum negative boundary"

# Valid equation appears only when the second value is the result
assert run("5 7 12\n") == "5 7 12", "direct sum"

# Valid equation appears when the first value is the result
assert run("12 5 7\n") == "5 7 12", "first value is the sum"

# No valid permutation
assert run("-10 3 8\n") == "-1 -1 -1", "no solution"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 3`|`2 1 3`| Mẫu được cung cấp với số tiền hợp lệ | 
|`2 4 42`|`-1 -1 -1`| Cung cấp mẫu không có đơn đặt hàng hợp lệ | 
|`0 0 0`|`0 0 0`| Các giá trị bằng nhau và bằng 0 | 
|`9223372036854775807 -1 9223372036854775806`| Đặt hàng giống nhau | Ranh giới đầu vào 64-bit trên và các giá trị trung gian lớn | 
|`-9223372036854775807 1 -9223372036854775806`| Đặt hàng giống nhau | Ranh giới đầu vào 64-bit thấp hơn | 
|`12 5 7`|`5 7 12`| Giá trị đầu vào đầu tiên phải được đặt cuối cùng | 
|`-10 3 8`|`-1 -1 -1`| Giá trị âm không có nghiệm | 

## Vỏ cạnh 

Trường hợp hoàn toàn bằng không`0 0 0`đạt đến điều kiện đầu tiên bởi vì`0 + 0 = 0`. Thuật toán in`0 0 0`, vì vậy các giá trị trùng lặp và số 0 không yêu cầu nhánh đặc biệt. 

Đối với các giá trị khác 0 trùng lặp, hãy xem xét`1 1 2`. Việc kiểm tra đầu tiên cho`1 + 1 = 2`, do đó chương trình in`1 1 2`. Việc hai vị trí đầu vào chứa cùng một giá trị không làm thay đổi số học hoặc làm mất hiệu lực thứ tự. 

Đối với các giá trị âm, hãy xem xét`-5 2 -3`. Lần kiểm tra đầu tiên thành công vì`-5 + 2 = -3`, và chương trình in`-5 2 -3`. Thuật toán không bao giờ giả định rằng toán hạng hoặc kết quả là dương. 

Đối với trường hợp giá trị đầu vào đầu tiên là tổng, hãy xem xét`12 5 7`. Các bài kiểm tra kiểm tra đầu tiên`12 + 5 = 7`và thất bại. Các thử nghiệm thứ hai`12 + 7 = 5`và thất bại. Các thử nghiệm thứ ba`5 + 7 = 12`và thành công, vì vậy đầu ra là`5 7 12`. Điều này xác nhận rằng thuật toán sẽ kiểm tra cả ba lựa chọn có thể có cho vế phải. 

Tại ranh giới số,`9223372036854775807 -1 9223372036854775806`thỏa mãn`9223372036854775807 + (-1) = 9223372036854775806`. Python tính toán chính xác điều này, do đó giá trị biên không gây ra hiện tượng tràn hoặc cắt bớt. 

Cuối cùng,`2 4 42`thực hiện con đường thất bại. Ba phương trình có thể là`2 + 4 = 42`,`2 + 42 = 4`, Và`4 + 42 = 2`, tất cả đều sai. Vì mọi lựa chọn có thể có của giá trị thứ ba đều bị từ chối,`-1 -1 -1`là câu trả lời đúng duy nhất.
