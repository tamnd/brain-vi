---
title: "CF 130G - BẬT CAPS LOCK"
description: "Chúng ta được cung cấp một chuỗi chứa các ký tự ASCII có thể in được. Một số ký tự có thể là chữ cái tiếng Anh viết thường, một số có thể là chữ cái viết hoa và một số khác có thể là ký hiệu hoặc chữ số."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 130
codeforces_index: "G"
codeforces_contest_name: "Unknown Language Round 4"
rating: 1700
weight: 130
solve_time_s: 88
verified: true
draft: false
---

[CF 130G - BẬT CAPS LOCK](https://codeforces.com/problemset/problem/130/G) 

**Đánh giá:** 1700 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chứa các ký tự ASCII có thể in được. Một số ký tự có thể là chữ cái tiếng Anh viết thường, một số có thể là chữ cái viết hoa và một số khác có thể là ký hiệu hoặc chữ số. Nhiệm vụ là chỉ chuyển đổi các chữ cái viết thường thành phiên bản viết hoa trong khi giữ nguyên mọi ký tự khác. 

Ví dụ, chuỗi`cOdEfOrCeS`trở thành`CODEFORCES`. Các chữ cái viết thường`c`,`d`,`f`,`r`, Và`e`được chuyển đổi, trong khi các ký tự viết hoa vẫn giữ nguyên. 

Độ dài đầu vào tối đa là 100 ký tự, cực kỳ nhỏ. Ngay cả một giải pháp không hiệu quả cũng có thể nằm gọn trong giới hạn. Quét tuyến tính trên chuỗi là quá đủ vì việc xử lý 100 ký tự về cơ bản không mất thời gian. Việc sử dụng bộ nhớ cũng không đáng kể vì chúng ta chỉ cần lưu trữ chuỗi kết quả. 

Phần khó khăn không phải là hiệu suất mà là tính chính xác. Việc triển khai bất cẩn có thể vô tình sửa đổi các ký tự không phải là chữ thường. 

Hãy xem xét đầu vào này:```
abc123!?
```Đầu ra đúng là:```
ABC123!?
```Các chữ số và dấu chấm câu phải không thay đổi. Việc triển khai dịch chuyển một cách mù quáng mọi mã ký tự theo một lượng cố định sẽ làm hỏng các ký tự không phải chữ cái. 

Một trường hợp cạnh khác là một chuỗi đã viết hoa:```
HELLO
```Đầu ra phải giữ nguyên:```
HELLO
```Việc triển khai có lỗi giả định mọi chữ cái đều là chữ thường có thể chuyển đổi không chính xác các ký tự chữ hoa thành các ký hiệu ASCII không liên quan. 

Một trường hợp quan trọng hơn là các ký hiệu hỗn hợp:```
a-z_
```Đầu ra đúng là:```
A-Z_
```Chỉ các chữ cái viết thường`a`Và`z`nên thay đổi. Dấu gạch ngang và dấu gạch dưới phải được giữ nguyên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp nhất là kiểm tra từng nhân vật một. Đối với mỗi ký tự, chúng tôi kiểm tra xem đó có phải là chữ cái tiếng Anh viết thường hay không. Nếu có, chúng tôi thay thế nó bằng chữ hoa của nó. Nếu không, chúng tôi sao chép nó không thay đổi vào kết quả. 

Điều này đã hoạt động hiệu quả vì độ dài chuỗi tối đa là 100. Trường hợp xấu nhất chỉ thực hiện 100 lần kiểm tra và biến đổi ký tự, con số này không đáng kể. 

Một cách để thực hiện thủ công phương pháp brute-force này là thông qua số học ASCII. Chữ thường`'a'`bởi vì`'z'`chính xác là 32 vị trí sau chữ in hoa`'A'`bởi vì`'Z'`trong bảng ASCII. Vì vậy, nếu một ký tự nằm trong phạm vi chữ thường, chúng ta có thể trừ 32 khỏi giá trị ASCII của nó. 

Ngôn ngữ này đã cung cấp một giải pháp sạch hơn thông qua tính năng tích hợp sẵn`upper()`phương pháp. Phương thức này thực hiện nội bộ logic chuyển đổi tương tự và giữ nguyên các ký tự không phải chữ cái. Do bài toán chỉ yêu cầu chuyển đổi chữ hoa nên việc sử dụng phương thức có sẵn sẽ mang lại cách thực hiện đơn giản và an toàn nhất. 

Ý tưởng brute-force và giải pháp tối ưu ở đây thực sự giống nhau vì đầu vào rất nhỏ và mọi cách tiếp cận hợp lệ sẽ quét chuỗi một lần. Cải tiến chính không phải là tốc độ tiệm cận mà là tính đơn giản và độ tin cậy của mã. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chuyển đổi ký tự thủ công | O(n) | O(n) | Đã chấp nhận | 
| Sử dụng tích hợp sẵn`upper()`| O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào từ đầu vào tiêu chuẩn. 
2. Chuyển chuỗi thành chữ hoa bằng cách sử dụng Python`upper()`phương pháp. 

Phương pháp này chuyển đổi mọi chữ cái tiếng Anh viết thường thành phiên bản viết hoa trong khi vẫn giữ nguyên các chữ số, dấu câu và các chữ cái viết hoa. 
3. In chuỗi đã chuyển đổi. 

### Tại sao nó hoạt động 

Thuật toán xử lý từng ký tự một cách độc lập. Đối với mỗi ký tự, việc chuyển đổi chữ hoa tuân theo các quy tắc ánh xạ chữ cái ASCII tiêu chuẩn. Các chữ cái viết thường được chuyển thành chữ hoa, trong khi tất cả các ký tự khác vẫn giữ nguyên. Vì mọi vị trí đều được xử lý chính xác và độc lập nên chuỗi cuối cùng chính xác là phép biến đổi chữ hoa bắt buộc của đầu vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# solution
s = input().strip()
print(s.upper())
```Chương trình bắt đầu bằng cách đọc chuỗi từ đầu vào. Vì đầu vào bao gồm một dòng duy nhất,`strip()`loại bỏ một cách an toàn ký tự dòng mới ở cuối được thêm vào bởi đầu vào tiêu chuẩn. 

Cuộc gọi đến`upper()`thực hiện toàn bộ quá trình chuyển đổi. Python tự động chuyển đổi các chữ cái tiếng Anh viết thường thành chữ hoa và giữ nguyên tất cả các ký tự ASCII có thể in khác. Hành vi này phù hợp chính xác với yêu cầu của vấn đề. 

Việc triển khai tránh thao tác ASCII thủ công, giúp giảm nguy cơ mắc lỗi liên quan đến dấu câu hoặc chữ in hoa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
cOdEfOrCeS
```| Vị trí | Nhân vật | Sau đó`upper()`| 
| --- | --- | --- | 
| 0 | c | C | 
| 1 | Ồ | Ồ | 
| 2 | d | D | 
| 3 | E | E | 
| 4 | f | F | 
| 5 | Ồ | Ồ | 
| 6 | r | R | 
| 7 | C | C | 
| 8 | e | E | 
| 9 | S | S | 

Đầu ra cuối cùng:```
CODEFORCES
```Ví dụ này cho thấy chữ thường và chữ in hoa có thể xuất hiện theo bất kỳ thứ tự nào. Thuật toán xử lý từng ký tự một cách độc lập và chỉ chuyển đổi các ký tự viết thường. 

### Ví dụ 2 

đầu vào:```
a-z_123
```| Vị trí | Nhân vật | Sau đó`upper()`| 
| --- | --- | --- | 
| 0 | một | A | 
| 1 | - | - | 
| 2 | z | Z | 
| 3 | _ | _ | 
| 4 | 1 | 1 | 
| 5 | 2 | 2 | 
| 6 | 3 | 3 | 

Đầu ra cuối cùng:```
A-Z_123
```Dấu vết này xác nhận rằng các ký hiệu và chữ số không thay đổi. Chỉ có chữ cái viết thường được sửa đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuỗi được quét một lần | 
| Không gian | O(n) | Một chuỗi chữ hoa mới được tạo | 

Đây,`n`là độ dài của chuỗi. Vì độ dài tối đa chỉ là 100 nên giải pháp dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    s = input().strip()
    print(s.upper())

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided samples
assert run("cOdEfOrCeS\n") == "CODEFORCES\n", "sample 1"

# custom cases
assert run("a\n") == "A\n", "single lowercase letter"
assert run("Z\n") == "Z\n", "single uppercase letter"
assert run("123!@#\n") == "123!@#\n", "symbols and digits unchanged"
assert run("abcxyz\n") == "ABCXYZ\n", "all lowercase letters"
assert run("a-z_123\n") == "A-Z_123\n", "mixed symbols and letters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`A`| Đầu vào chữ thường có kích thước tối thiểu | 
|`Z`|`Z`| Ký tự viết hoa vẫn không thay đổi | 
|`123!@#`|`123!@#`| Các chữ số và ký hiệu được giữ nguyên | 
|`abcxyz`|`ABCXYZ`| Tất cả các chữ cái viết thường đều chuyển đổi chính xác | 
|`a-z_123`|`A-Z_123`| Các ký tự hỗn hợp được xử lý an toàn | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
123!@#
```Thuật toán áp dụng`upper()`đến toàn bộ chuỗi. Vì không có ký tự nào là chữ thường nên mọi ký tự đều không thay đổi. Đầu ra trở thành:```
123!@#
```Trường hợp này xác nhận rằng dấu câu và chữ số được giữ nguyên chính xác. 

Bây giờ hãy xem xét:```
HELLO
```Mỗi ký tự đã được viết hoa. Việc chuyển đổi giữ nguyên chuỗi, tạo ra:```
HELLO
```Điều này xác minh rằng thuật toán không vô tình sửa đổi các chữ cái viết hoa. 

Cuối cùng, hãy xem xét:```
a-z_
```Thuật toán xử lý từng ký tự một trong nội bộ: 

| Nhân vật | Kết quả | 
| --- | --- | 
| một | A | 
| - | - | 
| z | Z | 
| _ | _ | 

Đầu ra cuối cùng là:```
A-Z_
```Điều này chứng tỏ rằng các chữ cái viết thường được chuyển đổi chính xác ngay cả khi được bao quanh bởi các ký hiệu không phải chữ cái.
