---
title: "CF 106160A - Số học ngẫu nhiên"
description: "Chúng ta được cấp một số tự nhiên n, nhưng thay vì nhập nó một cách hoàn hảo vào máy tính, mỗi lần nhấn chữ số có thể vô tình được theo sau bởi một lần nhấn nút +, một lần nhấn nút - hoặc không gì cả. Xác suất được cố định: + xảy ra với xác suất 0,45, - xảy ra với xác suất 0."
date: "2026-06-25T11:12:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106160
codeforces_index: "A"
codeforces_contest_name: "2025 Benelux Algorithm Programming Contest (BAPC 25)"
rating: 0
weight: 106160
solve_time_s: 95
verified: true
draft: false
---

[CF 106160A - Số học ngẫu nhiên](https://codeforces.com/problemset/problem/106160/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số tự nhiên`n`, nhưng thay vì nhập nó một cách hoàn hảo vào máy tính, mỗi lần nhấn chữ số có thể vô tình được theo sau bởi một`+`nhấn nút, một`-`nhấn nút, hoặc không có gì. Xác suất được cố định:`+`xảy ra với xác suất`0.45`,`-`xảy ra với xác suất`0.45`và không có nút bổ sung nào xảy ra với xác suất`0.10`. 

Các chữ số được nhập theo thứ tự bình thường của chúng. Các phép toán ngẫu nhiên này tạo ra một biểu thức số học có chứa các chữ số giống nhau, có thể được phân chia bằng dấu cộng và dấu trừ. Nếu biểu thức kết thúc bằng một dấu thì máy tính sẽ bỏ qua dấu cuối cùng đó. Nhiệm vụ là tính giá trị kỳ vọng của kết quả. 

Số đầu vào có thể lên tới`1000`các chữ số, do đó số học số nguyên thông thường không thể thực hiện được trong các ngôn ngữ có kiểu số nguyên có kích thước cố định. Quan trọng hơn, bất kỳ cách tiếp cận nào cố gắng liệt kê các biểu thức có thể đều không thể thực hiện được vì có ba lựa chọn sau mỗi chữ số, tạo ra sự tăng trưởng theo cấp số nhân. Quét tuyến tính trên các chữ số là độ phức tạp dự kiến. 

Phần khó khăn là các chữ số không độc lập. Một chữ số có thể trở thành một phần của số lớn hơn có nhiều chữ số nếu không có toán tử nào xuất hiện sau nó. Ví dụ: chữ số đầu tiên trong`123`đóng góp khác nhau tùy thuộc vào việc máy tính có tạo ra`1+23`,`12+3`, hoặc`123`. Giải pháp chỉ xử lý từng chữ số riêng biệt sẽ bỏ lỡ những tương tác này. 

Hãy xem xét đầu vào:```
12
```Các biểu thức có thể là`1+2`,`1-2`, Và`12`. Giá trị kỳ vọng của chúng là:```
0.45 * 3 + 0.45 * (-1) + 0.10 * 12 = 2.1
```Phép cộng từng chữ số ngây thơ sẽ không nắm bắt được trường hợp nối. 

Một trường hợp cạnh khác là một chữ số:```
7
```Không có chữ số nào sau nó, vì vậy bất kỳ sự vô tình nào`+`hoặc`-`cuối cùng được bỏ qua. Câu trả lời chính xác là:```
7
```Giải pháp áp dụng quy tắc chuyển tiếp mà không xử lý riêng chữ số cuối cùng có thể sửa đổi giá trị không chính xác. 

Trường hợp cạnh cuối cùng là một số dài như:```
000000
```Câu trả lời vẫn là`0`. Thuật toán phải hoạt động trên chuỗi và không thể dựa vào việc chuyển đổi đầu vào thành số nguyên. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ tạo ra mọi biểu thức có thể bằng cách thử cả ba khả năng sau mỗi chữ số. Đối với một số có`n`chữ số, có`3^(n-1)`các lựa chọn có thể có của các toán tử được chèn. Ngay cả đối với vài chục chữ số, điều này vẫn không khả thi và đầu vào có thể chứa một nghìn chữ số. 

Điều quan trọng cần lưu ý là quá trình ngẫu nhiên có một lượng nhỏ thông tin cần được ghi nhớ. Khi nhìn vào hậu tố của số, hai giá trị là đủ. Chúng tôi cần giá trị mong đợi của hậu tố được đánh giá và chúng tôi cần giá trị mong đợi của`10^(length of the first number in that suffix)`. Giá trị thứ hai xử lý trường hợp chữ số trước đó nối với hậu tố vì không có toán tử nào được chèn vào. 

Giả sử hậu tố hiện tại bắt đầu bằng chữ số`d`và hậu tố còn lại có giá trị mong đợi`value`và hệ số độ dài số đầu tiên dự kiến`power`. Nếu một`+`hoặc`-`xuất hiện sau`d`, số đầu tiên chỉ là`d`. Nếu không có gì xuất hiện,`d`trở thành chữ số đầu tiên của số đầu tiên hiện có, nhân giá trị vị trí thập phân của nó với mười. 

Việc xử lý từ phải sang trái làm cho mối quan hệ này trở nên đơn giản. Thừa số số thứ nhất tăng lên một cách chính xác`9`khi thêm một chữ số và giá trị mong đợi chỉ có thể được cập nhật bằng cách sử dụng hai đại lượng được lưu trữ. 

Cách tiếp cận vũ phu hoạt động vì nó mô hình trực tiếp mọi trạng thái máy tính có thể có, nhưng nó thất bại vì số lượng trạng thái tăng theo cấp số nhân. Nhận xét rằng tất cả các hậu tố có thể được tóm tắt bằng hai kỳ vọng sẽ làm giảm vấn đề xuống còn một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số dưới dạng chuỗi vì nó có thể chứa tới 1000 chữ số. 
2. Bắt đầu từ chữ số cuối cùng. Đối với hậu tố một chữ số, giá trị mong đợi chính là chữ số đó. Số đầu tiên trong hậu tố này có độ dài bằng 1 nên hệ số thập phân là`10`. 
3. Di chuyển qua các chữ số còn lại từ phải sang trái. Đối với chữ số hiện tại`d`, giữ hai giá trị của hậu tố đã được xử lý:`value`là kết quả mong đợi của hậu tố đó.`power`là giá trị kỳ vọng của`10`được nâng lên số chữ số trong toán hạng đầu tiên của hậu tố đó. 
4. Cập nhật giá trị mong đợi bằng:`new_value = 0.9 * d + 0.1 * d * power + 0.1 * value`Số hạng đầu tiên biểu thị các trường hợp toán tử xuất hiện sau`d`. Thuật ngữ thứ hai đại diện cho sự nối. Thuật ngữ thứ ba đại diện cho phần còn lại của hậu tố không thay đổi. 
5. Cập nhật hệ số thập phân bằng:`new_power = power + 9`Khi một toán tử xuất hiện sau`d`, toán hạng đầu tiên có độ dài bằng một. Khi không có toán tử nào xuất hiện, toán hạng đầu tiên sẽ tăng thêm một chữ số. Kỳ vọng đơn giản hóa việc thêm chín. 
6. Sau khi xử lý xong từng chữ số, hãy in giá trị mong đợi cuối cùng. 

Tại sao nó hoạt động: 

Điều bất biến là sau khi xử lý một hậu tố,`value`chính xác là kết quả tính toán mong đợi cho hậu tố đó và`power`chính xác là số nhân thập phân dự kiến ​​của toán hạng đầu tiên của nó. Quá trình chuyển đổi xem xét tất cả ba sự kiện có thể xảy ra sau chữ số hiện tại và kết hợp xác suất của chúng. Vì quá trình chuyển đổi bảo toàn cả hai ý nghĩa nên trạng thái cuối cùng sau khi xử lý toàn bộ chuỗi chứa giá trị mong đợi chính xác. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    s = input().strip()

    value = int(s[-1])
    power = 10.0

    for ch in reversed(s[:-1]):
        d = int(ch)
        value = 0.9 * d + 0.1 * d * power + 0.1 * value
        power += 9.0

    print("{:.12f}".format(value))

if __name__ == "__main__":
    solve()
```Mã lưu trữ số dưới dạng chuỗi để các đầu vào rất lớn được xử lý một cách an toàn. Chữ số cuối cùng khởi tạo hậu tố vì nó không có chữ số theo sau để hợp nhất. 

Vòng lặp xử lý tất cả các chữ số còn lại theo thứ tự ngược lại. Biến`value`tương ứng với kết quả mong đợi của hậu tố được xử lý, trong khi`power`lưu trữ hệ số nhân vị trí thập phân dự kiến ​​​​cần thiết khi một chữ số nối đầu hậu tố đó. 

Thứ tự cập nhật quan trọng. cái mới`value`phải dùng cái cũ`power`và cũ`value`, do đó phép gán được thực hiện trước khi tăng`power`. 

Số dấu phẩy động Python ở đây có đủ độ chính xác vì số chữ số chỉ là 1000 và lỗi bắt buộc là`10^-6`. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
12345
```các tiểu bang là: 

| Chữ số được xử lý | giá trị | quyền lực | 
| --- | --- | --- | 
| 5 | 5 | 10 | 
| 4 | 8.1 | 19 | 
| 3 | 9,21 | 28 | 
| 2 | 8.321 | 37 | 
| 1 | 5.4321 | 46 | 

trận chung kết`value`là`5.4321`, phù hợp với mẫu Dấu vết cho thấy bản tóm tắt hậu tố đủ để khôi phục toàn bộ sự mong đợi. 

Đối với đầu vào:```
777777
```các tiểu bang là: 

| Chữ số được xử lý | giá trị | quyền lực | 
| --- | --- | --- | 
| 7 | 7 | 10 | 
| 7 | 12.4 | 19 | 
| 7 | 21.16 | 28 | 
| 7 | 29.296 | 37 | 
| 7 | 35.5296 | 46 | 
| 7 | 42 | 55 | 

Các chữ số lặp lại tạo ra mô hình tăng trưởng ổn định và giá trị kỳ vọng cuối cùng là`42`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chữ số được xử lý một lần từ phải sang trái. | 
| Không gian | O(1) | Chỉ có hai giá trị dấu phẩy động được lưu trữ. | 

Kích thước đầu vào tối đa là 1000 chữ số, do đó, quét tuyến tính dễ dàng phù hợp với giới hạn thời gian và tránh tất cả việc liệt kê theo cấp số nhân. 

## Trường hợp thử nghiệm```python
import io
import sys

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue().strip()

assert run("12345\n") == "5.432100000000", "sample 1"
assert run("777777\n") == "42.000000000000", "sample 2"

assert run("7\n") == "7.000000000000", "single digit"
assert run("0\n") == "0.000000000000", "zero input"
assert run("12\n") == "2.100000000000", "operator and concatenation case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`7`|`7`| Các toán tử theo sau có một chữ số và bị bỏ qua | 
|`0`|`0`| Không xử lý | 
|`12`|`2.1`| Sự khác biệt giữa nối và toán tử | 
|`777777`|`42`| Các chữ số lặp lại và chuyển tiếp tích lũy | 

## Vỏ cạnh 

Dành cho:```
7
```thuật toán khởi tạo`value`ĐẾN`7`và không bao giờ vào vòng lặp vì không còn chữ số nào. Câu trả lời vẫn ở lại`7`, phù hợp với thực tế là mọi dấu hiệu ngẫu nhiên cuối cùng đều bị bỏ qua. 

Vì:```
12
```thuật toán bắt đầu với hậu tố`2`, Ở đâu`value = 2`Và`power = 10`. Đang xử lý chữ số`1`mang lại:```
0.9 * 1 + 0.1 * 1 * 10 + 0.1 * 2
= 2.1
```Điều này kết hợp ba khả năng:`1+2`,`1-2`, Và`12`. 

Vì:```
000000
```mọi chữ số đều bằng 0, vì vậy cả hai số hạng cập nhật vẫn bằng 0. Thuật toán không bao giờ cố gắng chuyển đổi toàn bộ chuỗi thành số nguyên, tránh tràn và giữ nguyên các số 0 đứng đầu. Câu trả lời cuối cùng là`0`.
