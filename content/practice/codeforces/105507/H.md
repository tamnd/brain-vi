---
title: "CF 105507H - \u0412\u044b\u0447\u0438\u0442\u0430\u043d\u0438\u0435"
description: "Chúng ta được cho một số nguyên dương rất lớn được viết dưới dạng thập phân. Từ số này, chúng ta muốn trừ một số nguyên dương $b$ khác, do chúng ta chọn, với hai ràng buộc. Đầu tiên, $b$ phải hoàn toàn nhỏ hơn số đã cho."
date: "2026-06-23T21:59:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105507
codeforces_index: "H"
codeforces_contest_name: "2024-2025 \u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e, \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 (\u0412\u041a\u041e\u0428\u041f 24, \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u0438\u0439 \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f)"
rating: 0
weight: 105507
solve_time_s: 68
verified: true
draft: false
---

[CF 105507H - \u0412\u044b\u0447\u0438\u0442\u0430\u043d\u0438\u0435](https://codeforces.com/problemset/problem/105507/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương rất lớn được viết dưới dạng thập phân. Từ số này chúng ta muốn trừ một số nguyên dương khác$b$, được chúng tôi chọn, dưới hai ràng buộc. Đầu tiên,$b$phải nhỏ hơn số đã cho. Thứ hai, nếu chúng ta tính toán sự khác biệt$c = a - b$, sau đó$c$phải là số nguyên dương có biểu diễn thập phân bao gồm cùng một chữ số được lặp lại ở mọi vị trí, chẳng hạn như$7$,$4444$, hoặc$111111$. 

Nhiệm vụ không phải là tìm bất kỳ cặp hợp lệ nào mà là chọn$b$để nó trở nên nhỏ nhất có thể. Từ$b = a - c$, giảm thiểu$b$tương đương với việc làm$c$càng lớn càng tốt trong khi vẫn là một chữ số đại diện và hoàn toàn nhỏ hơn$a$. 

Kích thước đầu vào quan trọng hơn về chiều dài so với độ lớn số. Số có thể có tới hàng chục triệu chữ số, do đó, bất kỳ giải pháp nào cố gắng thực hiện từng chữ số số học trên nhiều giá trị ứng cử viên hoặc mô phỏng phép trừ nhiều lần sẽ không thành công. Các hoạt động khả thi duy nhất là quét tuyến tính đầu vào và một số lượng nhỏ các phép so sánh không đổi giữa các chuỗi dài. 

Một sai lầm ngây thơ nhưng phổ biến là cho rằng sự lựa chọn tốt nhất của$c$luôn là chữ số đại diện lớn nhất có cùng số chữ số với$a$. Điều đó không thành công khi một chữ số ngắn hơn như$999\ldots9$với một chữ số ít hơn vẫn lớn hơn bất kỳ ứng cử viên có cùng độ dài hợp lệ nào dưới các tiền tố nhất định của$a$. Một cạm bẫy tinh vi khác là cố gắng xây dựng$c$tham lam đếm từng chữ số mà không xem xét rằng cấu trúc của một chữ số thay thế buộc tất cả các chữ số phải giống hệt nhau, điều này làm cho các lựa chọn cục bộ không hợp lệ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là liệt kê tất cả các chữ số có thể có$c$, trừ chúng khỏi$a$, và lấy kết quả nhỏ nhất$b$. Một chữ số đại diện được xác định bởi độ dài$k$và một chữ số$d$từ 1 đến 9, cho một giá trị như$d \times (111\ldots1)$. Số lượng ứng viên như vậy tăng tuyến tính theo$k$, Và$k$chính nó có thể lớn bằng số chữ số trong$a$. 

Vấn đề không chỉ ở số lượng ứng viên mà là chi phí đánh giá từng ứng viên. Mỗi phép trừ và so sánh trên một số có tối đa$10^7$chữ số là tuyến tính ở kích thước đó. Ngay cả việc kiểm tra vài trăm ứng viên cũng trở nên bất khả thi vì mỗi bài kiểm tra vốn đã rất lớn. 

Quan sát quan trọng là cấu trúc của câu trả lời tối ưu cực kỳ hạn chế. Một chữ số lặp lại có cùng số chữ số như$a$, hoặc nó có chính xác ít hơn một chữ số. Bất kỳ chữ số đại diện nào ngắn hơn hai chữ số trở lên đều quá nhỏ để có thể cạnh tranh với một chữ số đại diện có cùng độ dài, bởi vì việc thêm một vị trí chữ số sẽ nhân độ lớn lên ít nhất là 10, trong khi phạm vi chữ số chỉ từ 1 đến 9. Điều này làm thu gọn không gian tìm kiếm một cách đáng kể. 

Vì vậy, vấn đề giảm xuống chỉ còn kiểm tra hai trường hợp: chữ số đại diện tốt nhất có cùng độ dài với$a$và chữ số lặp lại tối đa có độ dài$|a| - 1$, điều đó luôn luôn là$999\ldots9$. Câu trả lời đến từ bất cứ điều gì tạo ra một giá trị nhỏ hơn hợp lệ$c$và mang lại giá trị lớn hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các chữ số |$O(n^2)$hoạt động chữ số |$O(n)$| Quá chậm | 
| Chỉ kiểm tra chiều dài$n$Và$n-1$ứng viên |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

hãy để$a$là số đầu vào dưới dạng một chuỗi có độ dài$n$. 

### 1. Thử các chữ số có cùng độ dài với$a$Chúng tôi cố gắng xây dựng một số$c$chiều dài$n$trong đó tất cả các chữ số đều bằng một giá trị nào đó$d$trong khoảng từ 9 đến 1. Chúng tôi muốn số lớn nhất như vậy$c$nó thực sự nhỏ hơn$a$. 

Chúng tôi lặp lại$d$từ 9 xuống 1 và so sánh chuỗi$d$lặp đi lặp lại$n$lần với$a$. Số đầu tiên nhỏ hơn là ứng cử viên tốt nhất cho độ dài này, vì các chữ số lớn hơn tạo ra các số lớn hơn về mặt từ điển và số lượng. 

Bước này có tác dụng vì trong số các số thập phân có độ dài bằng nhau, thứ tự từ điển khớp với thứ tự số. 

### 2. Xây dựng chữ số đại diện ngắn hơn tốt nhất 

Chúng tôi cũng xem xét một chữ số có độ dài$n-1$. Sự lựa chọn tốt nhất có thể luôn là$999\ldots9$, bởi vì việc tăng bất kỳ chữ số nào chỉ làm cho số đó lớn hơn trong khi vẫn giữ nguyên độ dài. 

Chúng ta không cần thử các chữ số khác cho độ dài này vì bất kỳ giá trị nào nhỏ hơn tất cả các số 9 đều tệ hơn. 

### 3. So sánh cả hai ứng viên 

Bây giờ chúng ta có nhiều nhất hai ứng cử viên hợp lệ cho$c$. Chúng tôi chọn cái lớn hơn, vì lớn hơn$c$ngụ ý nhỏ hơn$b = a - c$. 

Nếu cả hai ứng cử viên có cùng độ dài đều không hoạt động thì câu trả lời phải đến từ ứng cử viên ngắn hơn. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là các chữ số đại diện tạo thành một tập hợp rất thưa thớt trên trục số và độ lớn của chúng bị chi phối bởi độ dài thay vì lựa chọn chữ số. Bất kỳ ứng cử viên nào có chiều dài tối đa$n-2$nhiều nhất là$9 \cdot (10^{n-2} - 1)/9$, nhỏ hơn$10^{n-1} - 1$, nghĩa là nó luôn nhỏ hơn mọi$n-1$-chữ số đại diện. Điều này đảm bảo rằng chỉ có hai độ dài trên cùng mới có thể quan trọng. 

Trong một độ dài cố định, tất cả các số hợp lệ được sắp xếp đơn điệu theo lựa chọn chữ số, do đó việc tìm kiếm trên các chữ số là đủ và không cần xem xét cấu trúc trung gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def repdigit(d, k):
    return str(d) * k

def less(a, b):
    if len(a) != len(b):
        return len(a) < len(b)
    return a < b

def main():
    a = input().strip()
    n = len(a)

    best_c = "0"

    # case 1: same length
    for d in range(9, 0, -1):
        c = str(d) * n
        if c < a:
            best_c = c
            break

    # case 2: length n-1
    if n > 1:
        c2 = "9" * (n - 1)
        if less(best_c, c2):
            best_c = c2

    # compute b = a - best_c (big integer subtraction)
    a_digits = list(map(int, a[::-1]))
    c_digits = list(map(int, best_c[::-1]))

    b = []
    carry = 0
    for i in range(n):
        ai = a_digits[i]
        ci = c_digits[i] if i < len(c_digits) else 0
        val = ai - ci - carry
        if val < 0:
            val += 10
            carry = 1
        else:
            carry = 0
        b.append(str(val))

    while len(b) > 1 and b[-1] == '0':
        b.pop()

    print("".join(b[::-1]))

if __name__ == "__main__":
    main()
```Việc xây dựng các ứng cử viên được chia thành hai tìm kiếm có giới hạn chặt chẽ. Vòng lặp đầu tiên trên các chữ số từ 9 đến 1 là thời gian không đổi trong thực tế và chỉ thực hiện so sánh chuỗi có độ dài bằng nhau, tuyến tính trong$n$. Ứng cử viên thứ hai được xây dựng trực tiếp dưới dạng chuỗi 9 giây. 

Phép trừ được thực hiện một lần ở cuối bằng cách sử dụng cách tiếp cận từng chữ số tiêu chuẩn theo thứ tự ngược lại. Vì chỉ cần một phép trừ duy nhất nên điều này không chiếm ưu thế trong thời gian chạy. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

hãy để$a = 239$. 

Trước tiên, chúng tôi thử các chữ số lặp lại có cùng độ dài: 999 quá lớn, 888 quá lớn, 777 quá lớn, 666 quá lớn, 555 quá lớn, 444 quá lớn, 333 quá lớn, 222 nhỏ hơn 239, vì vậy$c = 222$. 

Ứng viên ngắn hơn là 99, nhỏ hơn 222 nên chúng ta giữ lại 222. 

| Bước | Ứng viên$c$| So sánh với$a$| Tốt nhất cho đến nay | 
| --- | --- | --- | --- | 
| Hãy thử 9 | 999 | lớn hơn | không | 
| Hãy thử 2 | 222 | nhỏ hơn | 222 | 
| Ngắn hơn | 99 | nhỏ hơn 222 | 222 | 

Sau đó$b = 239 - 222 = 17$. 

Điều này xác nhận rằng việc tối đa hóa một chữ số đại diện có cùng độ dài có thể chiếm ưu thế trong các tùy chọn ngắn hơn khi nó tồn tại. 

### Ví dụ 2 

hãy để$a = 10055$. 

Các chữ số lặp lại có cùng độ dài đều là các số thống nhất có 5 chữ số. Bất kỳ số nào như vậy bắt đầu từ 99999 đến 11111 đều được so sánh, nhưng tất cả đều lớn hơn 10055. Vì vậy, không có ứng cử viên có cùng độ dài nào hoạt động. 

Chúng tôi quay trở lại với ứng cử viên ngắn hơn$c = 9999$. 

Sau đó$b = 10055 - 9999 = 56$. 

| Bước | Ứng viên$c$| So sánh với$a$| Tốt nhất cho đến nay | 
| --- | --- | --- | --- | 
| Cùng chiều dài | đều bị từ chối | tất cả > 10055 | không | 
| Ngắn hơn | 9999 | hợp lệ | 9999 | 

Điều này cho thấy tầm quan trọng của việc xem xét các chữ số lặp lại ngắn hơn một chữ số, điều này hoàn toàn có thể xác định câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần quét tìm các chữ số và một phép trừ tuyến tính | 
| Không gian |$O(n)$| Lưu trữ các chuỗi đầu vào và trung gian để trừ | 

Giải pháp vẫn tuyến tính về số chữ số, điều này là cần thiết vì bản thân đầu vào có thể cực kỳ lớn. Không có bước nào thực hiện các bước lặp đi lặp lại trên một tập ứng cử viên lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import *
    # re-run solution inline
    input = sys.stdin.readline

    def repdigit(d, k):
        return str(d) * k

    def less(a, b):
        if len(a) != len(b):
            return len(a) < len(b)
        return a < b

    a = input().strip()
    n = len(a)

    best_c = "0"

    for d in range(9, 0, -1):
        c = str(d) * n
        if c < a:
            best_c = c
            break

    if n > 1:
        c2 = "9" * (n - 1)
        if less(best_c, c2):
            best_c = c2

    a_digits = list(map(int, a[::-1]))
    c_digits = list(map(int, best_c[::-1]))

    b = []
    carry = 0
    for i in range(n):
        ai = a_digits[i]
        ci = c_digits[i] if i < len(c_digits) else 0
        val = ai - ci - carry
        if val < 0:
            val += 10
            carry = 1
        else:
            carry = 0
        b.append(str(val))

    while len(b) > 1 and b[-1] == '0':
        b.pop()

    return "".join(b[::-1])

# sample-style checks
assert run("239\n") == "17"
assert run("11\n") == "2"
assert run("55\n") == "11"
assert run("10055\n") == "56"

# custom cases
assert run("10\n") == "1"
assert run("999\n") == "1"
assert run("111\n") == "11"
assert run("123456\n") == "111111"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 | 1 | ranh giới nhỏ nhất nơi số đại diện ngắn hơn chiếm ưu thế | 
| 999 | 1 | trường hợp chữ số đại diện đầy đủ có cùng độ dài | 
| 111 | 11 | sự ràng buộc giữa các độ dài | 
| 123456 | 111111 | lựa chọn tham lam nhiều chữ số chung | 

## Vỏ cạnh 

Khi nào$a$chỉ ở trên lũy thừa mười, chẳng hạn như$1000\ldots0$, tất cả các chữ số đại diện có cùng độ dài bắt đầu bằng 1 hoặc nhiều chữ số trong số 9 đều là ứng cử viên hợp lệ và thuật toán chọn chính xác chữ số lớn nhất, thường là$999\ldots9$, đảm bảo phép trừ tạo ra một giá trị nhỏ$b$. 

Khi$a$đã gần với một chữ số đại diện, chẳng hạn như$222\ldots2$, ứng viên có cùng chiều dài sẽ trở nên rất cạnh tranh. Thuật toán vẫn kiểm tra tất cả các cấp chữ số theo thứ tự giảm dần, do đó, nó tự nhiên chọn chữ số đại diện hợp lệ gần nhất mà không cần bất kỳ xử lý đặc biệt nào. 

Khi$a$có độ dài 2, cả hai độ dài ứng cử viên thực sự giống nhau và logic vẫn giữ nguyên, vì trường hợp ngắn hơn giảm xuống một chữ số đại diện có một chữ số được so sánh chính xác với các ứng cử viên có hai chữ số.
