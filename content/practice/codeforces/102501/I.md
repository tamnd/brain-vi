---
title: "CF 102501I - Chuột"
description: "Douglas thực hiện một thí nghiệm bắt và bắt lại cổ điển để ước tính quy mô của quần thể chuột. Ngày đầu tiên anh ta bắt được n1 con chuột, đánh dấu tất cả rồi thả chúng ra. Vào ngày thứ hai, anh ta bắt được n2 con chuột, trong đó n12 con đã được đánh dấu."
date: "2026-08-05T17:46:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102501
codeforces_index: "I"
codeforces_contest_name: "2019-2020 ICPC Southwestern European Regional Programming Contest (SWERC 2019-20)"
rating: 0
weight: 102501
solve_time_s: 189
verified: true
draft: false
---

[CF 102501I - Chuột](https://codeforces.com/problemset/problem/102501/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Douglas thực hiện một thí nghiệm bắt và bắt lại cổ điển để ước tính quy mô của quần thể chuột. 

Ngày đầu tiên anh ta bắt được`n1`chuột, đánh dấu tất cả và thả chúng ra. Vào ngày thứ hai, anh ta bắt được`n2`chuột, trong đó`n12`đã được đánh dấu rồi. Thay vì tính toán ước tính theo cách thủ công, nhiệm vụ của chúng ta chỉ đơn giản là đánh giá công cụ ước tính Chapman$$\left\lfloor \frac{(n_1+1)(n_2+1)}{n_{12}+1}\right\rfloor-1$$và in số nguyên kết quả. 

Đầu vào bao gồm chính xác ba số nguyên, do đó không cần tìm kiếm, mô phỏng hoặc tối ưu hóa. Mỗi giá trị tối đa là 10000, tạo ra sản phẩm lớn nhất$$(10000+1)^2 = 100020001,$$dễ dàng vừa với số nguyên có chữ ký 32 bit. Dù sao thì số nguyên Python cũng có độ chính xác tùy ý nên việc tràn không bao giờ là vấn đề đáng lo ngại. 

Công việc duy nhất cần thực hiện là tính một phép nhân, một phép chia số nguyên và một phép trừ. Bất kỳ thuật toán nào vượt quá thời gian không đổi sẽ không cần thiết. 

Một lỗi triển khai tinh vi là sử dụng số học dấu phẩy động thay vì số học số nguyên. Ví dụ, với đầu vào```
10000 10000 3
```kết quả đúng là```
25004999
```Sử dụng phép chia dấu phẩy động sau đó chuyển đổi thành số nguyên có thể vẫn có tác dụng đối với các giới hạn này, nhưng việc dựa vào dấu phẩy động là không cần thiết và có thể gây ra lỗi làm tròn trong các vấn đề tương tự. Phép chia sàn số nguyên khớp trực tiếp với định nghĩa toán học. 

Một sai lầm dễ mắc phải khác là quên phép trừ cuối cùng cho một. Đối với đầu vào```
1 1 1
```công thức trở thành$$\left\lfloor\frac{2\cdot2}{2}\right\rfloor-1
=2-1
=1,$$vì vậy đầu ra đúng là```
1
```Chỉ in thương số sẽ tạo ra kết quả không chính xác`2`. 

Lỗi phổ biến thứ ba là bỏ sót những phần được thêm vào trong công thức. Đối với đầu vào```
0 0 0
```phép tính đúng là$$\left\lfloor\frac{1\cdot1}{1}\right\rfloor-1=0,$$vậy câu trả lời là```
0
```sử dụng`n1 * n2 / n12`thậm chí sẽ chia cho số 0. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ cố gắng ước tính quần thể bằng cách xem xét mọi quy mô quần thể có thể có, kiểm tra giá trị nào tương thích với số lần bắt được quan sát hoặc thậm chí mô phỏng các thí nghiệm lặp đi lặp lại. Cách tiếp cận như vậy là không cần thiết về mặt toán học và có thể yêu cầu kiểm tra hàng triệu giá trị ứng viên nếu phạm vi tìm kiếm được chọn rộng rãi. 

Báo cáo vấn đề đã cung cấp công cụ ước tính Chapman làm ước tính mong muốn. Quy mô dân số không phải là thứ chúng ta phải suy luận bằng thuật toán. Chúng ta chỉ cần thay thế ba giá trị đầu vào vào biểu thức đã cho và tính kết quả. 

Quan sát quan trọng là công cụ ước tính đã bao gồm hoạt động sàn cần thiết. Phép chia sàn số nguyên tính toán chính xác cùng một giá trị mà không sử dụng số học dấu phẩy động. Sau đó trừ đi một và in kết quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(M), trong đó M là phạm vi quần thể được tìm kiếm | O(1) | Quá chậm và không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba số nguyên`n1`,`n2`, Và`n12`. 
2. Tính tử số`(n1 + 1) * (n2 + 1)`. Công thức thêm một cách rõ ràng vào cả hai số lần chụp trước khi nhân. 
3. Chia tử số cho`n12 + 1`sử dụng phép chia tầng số nguyên. Điều này khớp chính xác với hoạt động sàn trong công cụ ước tính. 
4. Trừ một từ thương số để có được ước tính Chapman. 
5. In số nguyên thu được. 

### Tại sao nó hoạt động 

Thuật toán là sự thực hiện trực tiếp định nghĩa toán học được đưa ra trong bài toán. Tính toán chia tầng số nguyên$$\left\lfloor\frac{(n_1+1)(n_2+1)}{n_{12}+1}\right\rfloor,$$đó chính xác là phần đầu tiên của công cụ ước tính. Trừ đi một sau đó sẽ hoàn thành công thức một cách chính xác, do đó giá trị được tính toán giống với ước tính được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n1, n2, n12 = map(int, input().split())

answer = ((n1 + 1) * (n2 + 1)) // (n12 + 1) - 1
print(answer)
```Chương trình bắt đầu bằng cách đọc ba số nguyên từ đầu vào tiêu chuẩn. 

Dòng tiếp theo đánh giá công thức một cách trực tiếp. Phép nhân được thực hiện trước phép chia, bảo toàn số học số nguyên chính xác. sử dụng`//`là cần thiết vì biểu thức toán học yêu cầu giá trị sàn của thương. 

Cuối cùng, chương trình trừ một và in kết quả. Không cần trường hợp đặc biệt nào vì`n12 + 1`luôn có ít nhất một nên phép chia cho 0 không thể xảy ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
15 18 11
```| Bước | n1 | n2 | n12 | Tử số | Thương số | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| Đọc đầu vào | 15 | 18 | 11 | - | - | - | 
| Tính toán tử số | 15 | 18 | 11 | 304 | - | - | 
| Phân tầng | 15 | 18 | 11 | 304 | 25 | - | 
| Trừ một | 15 | 18 | 11 | 304 | 25 | 24 | 

Ước tính được tạo ra bởi công thức Chapman là`24`. 

### Ví dụ 2 

đầu vào:```
0 0 0
```| Bước | n1 | n2 | n12 | Tử số | Thương số | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| Đọc đầu vào | 0 | 0 | 0 | - | - | - | 
| Tính toán tử số | 0 | 0 | 0 | 1 | - | - | 
| Phân tầng | 0 | 0 | 0 | 1 | 1 | - | 
| Trừ một | 0 | 0 | 0 | 1 | 1 | 0 | 

Ví dụ này cho thấy tại sao những phần được thêm vào bên trong công thức lại quan trọng. Ngay cả khi mọi đầu vào đều bằng 0, phép tính vẫn hợp lệ và tạo ra`0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số phép tính số học cố định được thực hiện. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ. | 

Thời gian chạy và mức sử dụng bộ nhớ không phụ thuộc vào giá trị đầu vào. Giải pháp thoải mái thỏa mãn các giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    n1, n2, n12 = map(int, input().split())
    print(((n1 + 1) * (n2 + 1)) // (n12 + 1) - 1)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    return out.getvalue()

# provided sample
assert run("15 18 11\n") == "24\n", "sample 1"

# minimum values
assert run("0 0 0\n") == "0\n", "minimum input"

# all values equal
assert run("5 5 5\n") == "5\n", "all equal"

# no recaptured rats
assert run("10 20 0\n") == "230\n", "zero recaptures"

# maximum values
assert run("10000 10000 10000\n") == "10000\n", "maximum values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 0`|`0`| Đầu vào pháp lý nhỏ nhất và xử lý chính xác những cái được thêm vào | 
|`5 5 5`|`5`| Phép trừ đúng sau khi chia | 
|`10 20 0`|`230`| Mẫu số trở thành một khi không có con chuột được đánh dấu nào bị bắt lại | 
|`10000 10000 10000`|`10000`| Giá trị đầu vào và số học lớn nhất gần giới hạn trên | 

## Vỏ cạnh 

Xem xét đầu vào```
0 0 0
```Thuật toán tính toán`(0 + 1) * (0 + 1) = 1`, chia cho`0 + 1 = 1`, thu được`1`, và trừ đi một để tạo ra`0`. Mọi hoạt động đều được xác định rõ ràng vì mẫu số không bao giờ bằng 0. 

Xem xét đầu vào```
1 1 1
```Tử số là`(1 + 1) * (1 + 1) = 4`. Chia cho`1 + 1 = 2`cho`2`, và trừ đi một sẽ tạo ra`1`. Điều này xác nhận rằng phép trừ cuối cùng là một phần của công cụ ước tính và không thể bỏ qua. 

Xem xét đầu vào```
10000 10000 3
```Tử số là`10001 × 10001 = 100020001`. Chia tầng số nguyên cho`4`sản lượng`25005000`, và trừ đi một sẽ cho`25004999`. Phép tính hoàn toàn nằm trong số học số nguyên, tránh mọi vấn đề làm tròn trong khi xử lý phép nhân lớn nhất được phép.
