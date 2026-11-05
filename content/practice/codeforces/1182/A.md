---
title: "CF 1182A - Điền hình"
description: "Chúng ta có một bảng có 3 hàng và $n$ cột. Các mảnh duy nhất có sẵn là domino có kích thước $2 nhân 1$, có thể được đặt theo chiều dọc hoặc chiều ngang."
date: "2026-06-12T01:26:16+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1182
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 566 (Div. 2)"
rating: 1000
weight: 1182
solve_time_s: 83
verified: true
draft: false
---

[CF 1182A - Điền hình](https://codeforces.com/problemset/problem/1182/A) 

**Đánh giá:** 1000 
**Tags:** dp, toán 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bảng có 3 hàng và$n$cột. Các mảnh duy nhất có sẵn là domino có kích thước$2 \times 1$, có thể được đặt theo chiều dọc hoặc chiều ngang. Nhiệm vụ là đếm xem có bao nhiêu cách khác nhau để toàn bộ$3 \times n$hình chữ nhật có thể được che phủ mà không bị chồng chéo và không để lại các ô trống. 

Đầu vào chứa một số nguyên duy nhất biểu thị số cột. Đầu ra là số lượng ô hoàn chỉnh của bảng đó. 

Sự ràng buộc$n \le 60$là rất nhỏ. Thậm chí một$O(n^2)$hoặc$O(n^3)$giải pháp sẽ dễ dàng đủ nhanh. Điều quan trọng là tìm ra sự tái diễn phù hợp. Việc cố gắng liệt kê trực tiếp tất cả các ô một cách nhanh chóng trở nên không khả thi vì số lượng khả năng tăng theo cấp số nhân. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Coi như```
1
```Bảng có ba ô. Mỗi domino bao gồm hai ô, vì vậy việc bao phủ một số ô lẻ là không thể. Câu trả lời đúng là```
0
```Việc triển khai bất cẩn có thể cố gắng áp dụng phép truy toán một cách mù quáng và tạo ra giá trị dương. 

Một ví dụ khác là```
2
```Câu trả lời là```
1
```Có chính xác một mẫu ốp lát cho một$3\times2$hình chữ nhật. Thiếu trường hợp cơ sở sẽ làm cho tất cả các giá trị sau này không chính xác. 

Một ví dụ thậm chí lớn hơn là```
4
```câu trả lời của ai là```
4
```Vì chỉ có thể có chiều rộng chẵn nên việc quên xử lý riêng các giá trị lẻ sẽ dẫn đến câu trả lời sai cho một nửa số đầu vào. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ đặt đệ quy các quân domino vào ô đầu tiên không được che chắn và tiếp tục cho đến khi bảng đầy. Vì mỗi vị trí đều tạo ra một số nhánh nên số lượng trạng thái tăng theo cấp số nhân. Vì$n=60$, số lượng cấu hình rất lớn, vượt xa những gì có thể khám phá trong vòng một giây. 

Lý do vũ phu hoạt động về mặt khái niệm là vì mỗi ô xếp hoàn chỉnh được tạo chính xác một lần. Điểm yếu của nó là liên tục tính toán lại các bảng một phần tương đương. 

Nhìn vào các giá trị nhỏ sẽ thấy một mô hình mạnh mẽ. Độ rộng lẻ luôn có cách bằng 0. Đối với chiều rộng chẵn, trình tự bắt đầu$$1,1,4,16,64,\dots$$cho chiều rộng$$0,2,4,6,8,\dots$$Mỗi lần chúng ta thêm hai cột, sẽ có chính xác hai lựa chọn độc lập để mở rộng mọi ô xếp trước đó. Do đó,$$f(n)=2f(n-2)$$thậm chí$n$. 

Quan sát này biến vấn đề thành một vấn đề quy hoạch động nhỏ. Bắt đầu từ$f(0)=1$, chúng tôi liên tục nhân với hai mỗi lần thêm hai cột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị của$n$. 
2. Nếu$n$là số lẻ, in ngay 0 vì a$3\times n$bảng chứa$3n$tế bào, thật kỳ lạ. Domino bao phủ hai ô, vì vậy không thể tồn tại một ô xếp hoàn hảo. 
3. Khởi tạo đáp án là 1, tương ứng với bảng trống$f(0)=1$. 
4. Liên tục tăng chiều rộng lên hai cột và nhân câu trả lời với 2 mỗi lần. 

Sự tái diễn xuất phát từ thực tế là mỗi cặp cột bổ sung đều giới thiệu chính xác hai mẫu mở rộng độc lập. 

1. Xuất ra giá trị kết quả. 

### Tại sao nó hoạt động 

Bất biến là sau khi xử lý chiều rộng$i$, biến`ans`bằng số ô của một$3\times i$hình chữ nhật. Ban đầu điều này đúng với chiều rộng bằng 0 vì có một ô trống. Mỗi lần tăng hai cột sẽ nhân đôi số khả năng, do đó bất biến vẫn hợp lệ. Khi chiều rộng$n$đã đạt được,`ans`bằng số lượng gạch mong muốn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

if n % 2 == 1:
    print(0)
else:
    ans = 1
    for _ in range(n // 2):
        ans *= 2
    print(ans)
```Bước đầu tiên kiểm tra xem chiều rộng có phải là số lẻ hay không. Vì tổng số ô là số lẻ nên không thể che phủ hoàn toàn nên câu trả lời ngay lập tức là 0. 

Đối với chiều rộng chẵn, biến`ans`lưu trữ số lượng ô cho chiều rộng được xử lý hiện tại. Nó bắt đầu từ trường hợp cơ bản$f(0)=1$. Mỗi lần lặp tương ứng với việc thêm hai cột và mỗi phần mở rộng như vậy sẽ nhân đôi số lượng ô hợp lệ. 

Số nguyên Python tự động tăng đến kích thước tùy ý, do đó, ngay cả câu trả lời cho$n=60$phù hợp một cách an toàn mà không có mối lo ngại tràn. 

Vòng lặp chạy chính xác`n // 2`lần. Việc sử dụng phép chia số nguyên sẽ tránh được những lỗi sai sót khi$n=0$hoặc$n=2$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào```
4
```| Bước | Chiều rộng hiện tại | trả lời | 
| --- | --- | --- | 
| Ban đầu | 0 | 1 | 
| Sau lần lặp đầu tiên | 2 | 2 | 
| Sau lần lặp thứ hai | 4 | 4 | 

đầu ra```
4
```Dấu vết này cho thấy mỗi cặp cột nhân đôi số lượng cấu hình như thế nào. 

### Ví dụ 2 

đầu vào```
1
```| Bước | Chiều rộng hiện tại | và | 
| --- | --- | --- | 
| Đọc n | 1 | - | 
| Kiểm tra lẻ | - | - | 
đầu ra```
0
```Ví dụ này thực hiện điều kiện chẵn lẻ. Vì số ô là số lẻ nên không tồn tại ô domino. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Vòng lặp thực thi$n/2$lần | 
| Không gian |$O(1)$| Chỉ có một vài biến được lưu trữ | 

Với$n\le60$, ngay cả thời gian tuyến tính cũng là tầm thường. Chương trình thực hiện tối đa ba mươi lần lặp và sử dụng bộ nhớ không đổi, đáp ứng các giới hạn một cách thoải mái. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())

    if n % 2 == 1:
        return "0\n"

    ans = 1
    for _ in range(n // 2):
        ans *= 2
    return str(ans) + "\n"

# provided sample
assert run("4\n") == "4\n", "sample 1"

# custom cases
assert run("1\n") == "0\n", "minimum odd width"
assert run("2\n") == "2\n", "smallest even width"
assert run("3\n") == "0\n", "another odd width"
assert run("60\n") == "1073741824\n", "maximum input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`0`| Chiều rộng lẻ không có giải pháp | 
|`2`|`2`| Chiều rộng chẵn nhỏ nhất | 
|`3`|`0`| Xử lý chẵn lẻ | 
|`60`|`1073741824`| Kích thước đầu vào tối đa | 

## Vỏ cạnh 

Đối với đầu vào```
1
```thuật toán ngay lập tức phát hiện ra điều đó$n$là số lẻ và in```
0
```Không có vòng lặp nào được thực hiện. Điều này ngăn cản việc áp dụng phép truy hồi cho một bảng không thể thực hiện được. 

Đối với đầu vào```
2
```việc tính toán tiến hành như sau. 
| Chiều rộng | và | 
| --- | --- | 
| 0 | 1 | 
| 2 | 2 | 
Câu trả lời cuối cùng là```
2
```Điều này xác nhận rằng trường hợp cơ bản$f(0)=1$được truyền bá một cách chính xác. 

Đối với đầu vào```
5
```việc kiểm tra độ rộng lẻ sẽ được kích hoạt lại ngay lập tức.```
0
```Nếu không có kiểm tra này, phép truy toán được thiết kế cho độ rộng chẵn có thể tạo ra câu trả lời tích cực không chính xác. 

Để có đầu vào tối đa```
60
```vòng lặp thực hiện ba mươi lần, liên tục nhân đôi số lượng. Các số nguyên có độ chính xác tùy ý của Python đảm bảo rằng giá trị```
1073741824
```được sản xuất chính xác mà không bị tràn.
