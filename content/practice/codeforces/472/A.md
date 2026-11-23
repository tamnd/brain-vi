---
title: "CF 472A - Hướng dẫn thiết kế: Học từ Toán"
description: "Chúng ta được cho một số nguyên n, trong đó n ≥ 12. Nhiệm vụ là tìm bất kỳ hai hợp số nào có tổng chính xác là n. Hợp số là số nguyên lớn hơn 1, có ít nhất một ước số khác 1 và chính nó."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 472
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 270"
rating: 800
weight: 472
solve_time_s: 82
verified: true
draft: false
---

[CF 472A - Hướng dẫn thiết kế: Học từ toán học](https://codeforces.com/problemset/problem/472/A) 

**Đánh giá:** 800 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên duy nhất`n`, Ở đâu`n ≥ 12`. Nhiệm vụ là tìm hai hợp số bất kỳ có tổng bằng chính xác`n`. 

Hợp số là số nguyên lớn hơn 1, có ít nhất một ước số khác 1 và chính nó. Ví dụ: 4, 6, 8 và 9 là hợp số, trong khi 2, 3, 5 và 7 là số nguyên tố. 

Đầu vào chỉ chứa một giá trị nên không cần xử lý nhiều trường hợp kiểm thử. Giới hạn trên là`10^6`, đủ nhỏ để ngay cả các thuật toán liên quan đến vài triệu thao tác cũng có thể dễ dàng đủ nhanh trong giới hạn thời gian một giây. 

Phần thú vị của vấn đề không phải là hiệu quả mà là tìm ra một công trình đảm bảo. Vì câu lệnh hứa hẹn rằng mọi số nguyên ít nhất 12 có thể được biểu diễn dưới dạng tổng của hai số tổng hợp, nên chúng ta chỉ cần một phương thức luôn tạo ra một cặp hợp lệ. 

Một sai lầm phổ biến là tập trung vào việc kiểm tra tính nguyên tố và tìm kiếm một cách thấu đáo một phân tách hợp lệ. Trong khi điều đó hoạt động, nó là không cần thiết. 

Trường hợp một cạnh là giá trị nhỏ nhất được phép. 

Đối với đầu vào:```
12
```một câu trả lời hợp lệ là:```
4 8
```Cả hai số đều là hợp số. Bất kỳ giải pháp nào cũng phải xử lý ranh giới dưới này một cách chính xác. 

Một trường hợp tinh tế khác là một giá trị lẻ như:```
15
```Một sự phân hủy như`4 + 11`không hợp lệ vì 11 là số nguyên tố. Đầu ra đúng có thể là:```
6 9
```Cả hai số đều là hợp số. 

Một công trình bất cẩn như luôn luôn in ấn`4`Và`n - 4`thất bại đối với số lẻ. Ví dụ, với`n = 15`, nó sẽ tạo ra`4 + 11`, điều này không được phép. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là vũ lực. Chúng ta có thể thử mọi số đầu tiên có thể`x`từ 4 đến`n - 4`, cho phép`y = n - x`và kiểm tra xem cả hai số có phải là hợp số hay không. Một con số lên tới`10^6`có thể được kiểm tra tính tổng hợp trong khoảng`O(√n)`thời gian. 

Cách tiếp cận này đúng vì nó tìm kiếm rõ ràng tất cả các phân tách có thể có và trả về phân tách hợp lệ đầu tiên. Vấn đề là trong trường hợp xấu nhất chúng ta có thể kiểm tra gần một triệu ứng viên và mỗi ứng viên có thể yêu cầu kiểm tra tính nguyên tố căn bậc hai. Điều đó đẩy sự phức tạp về phía`O(n√n)`, việc đó tốn nhiều công sức hơn mức cần thiết. 

Quan sát quan trọng đến từ tính chẵn lẻ. 

Nếu như`n`là số chẵn thì:```
4 + (n - 4)
```luôn luôn hoạt động. 

Số 4 là hợp số. Từ`n`là chẵn,`n - 4`cũng là chẵn. Bởi vì`n ≥ 12`, chúng tôi có`n - 4 ≥ 8`. Mọi số chẵn lớn hơn 2 đều là hợp số hay ít nhất không phải là số nguyên tố? Cụ thể hơn, mọi số chẵn ít nhất 4 đều là hợp số. Vậy cả hai số đều là hợp số. 

Nếu như`n`thì kỳ quặc thì:```
9 + (n - 9)
```luôn luôn hoạt động. 

Số 9 là hợp số. Từ`n`thật kỳ lạ,`n - 9`là chẵn. Bởi vì`n ≥ 13`đối với bất kỳ đầu vào hợp lệ lẻ nào, chúng tôi có`n - 9 ≥ 4`. Mọi số chẵn ít nhất 4 là hợp số nên số thứ hai cũng là hợp số. 

Quan sát này loại bỏ tất cả tìm kiếm. Chúng ta có thể xây dựng câu trả lời ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n√n) | O(1) | Quá chậm và không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`. 
2. Kiểm tra xem`n`là chẵn. 
3. Nếu`n`chẵn, đầu ra`4`Và`n - 4`. 

Số đầu tiên là hợp số. Số thứ hai là số nguyên chẵn ít nhất là 8, cũng là số nguyên hợp số. 
4. Nếu không,`n`thật kỳ quặc. đầu ra`9`Và`n - 9`. 

Số đầu tiên là hợp số. Số thứ hai là số nguyên chẵn ít nhất là 4, là số tổng hợp. 

### Tại sao nó hoạt động 

Thuật toán dựa hoàn toàn vào tính chẵn lẻ. 

Thậm chí`n`, sự phân hủy`4 + (n - 4)`hợp lệ vì 4 là hợp số và`n - 4`là một số chẵn ít nhất là 8. Bất kỳ số nguyên chẵn nào lớn hơn 2 đều có ước số là 2, vì vậy nó là hợp số. 

Đối với số lẻ`n`, sự phân hủy`9 + (n - 9)`hợp lệ vì 9 là hợp số và`n - 9`là một số chẵn ít nhất là 4. Một lần nữa, mọi số nguyên chẵn lớn hơn 2 đều là hợp số. 

Một trong hai trường hợp này luôn được áp dụng nên thuật toán luôn đưa ra hai hợp số có tổng chính xác là`n`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

if n % 2 == 0:
    print(4, n - 4)
else:
    print(9, n - 9)
```Chương trình bắt đầu bằng cách đọc giá trị đầu vào duy nhất. 

Sự ngang bằng của`n`xác định công trình nào sẽ được sử dụng. Đối với các số chẵn, cặp`(4, n - 4)`được sản xuất. Đối với số lẻ, cặp`(9, n - 9)`được sản xuất. 

Điều kiện biên duy nhất là đảm bảo số thứ hai vẫn là hợp số. Các ràng buộc đảm bảo điều này một cách tự động. Đối với đầu vào chẵn nhỏ nhất,`n = 12`, giá trị thứ hai là`8`. Đối với đầu vào lẻ ​​nhỏ nhất,`n = 13`, giá trị thứ hai là`4`. Cả hai đều là tổng hợp. 

Không cần vòng lặp, kiểm tra tính nguyên thủy hoặc cấu trúc dữ liệu bổ sung. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
12
```| n | n % 2 | Cặp đầu ra | 
| --- | --- | --- | 
| 12 | 0 | (4, 8) | 

Vì 12 là số chẵn nên thuật toán chọn`4`Và`12 - 4 = 8`. Cả hai số đều là hợp số và tổng của chúng là 12. 

### Ví dụ 2 

đầu vào:```
15
```| n | n % 2 | Cặp đầu ra | 
| --- | --- | --- | 
| 15 | 1 | (9, 6) | 

Vì 15 là số lẻ nên thuật toán chọn`9`Và`15 - 9 = 6`. Cả hai số đều là hợp số và tổng của chúng là 15. 

Ví dụ này chứng minh tại sao sử dụng`4`vì tất cả đầu vào sẽ thất bại. Sự phân hủy`4 + 11`sẽ chứa một số nguyên tố, trong khi`9 + 6`luôn luôn hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ kiểm tra tính chẵn lẻ và các phép tính số học | 
| Không gian | O(1) | Sử dụng một lượng bộ nhớ không đổi | 

Thời gian chạy không phụ thuộc vào kích thước của`n`. Ngay cả đối với giá trị tối đa`10^6`, thuật toán thực hiện cùng một số thao tác cố định, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    n = int(input())

    if n % 2 == 0:
        print(4, n - 4)
    else:
        print(9, n - 9)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
assert run("12\n") == "4 8", "sample"

# custom cases
assert run("13\n") == "9 4", "smallest odd valid input"
assert run("14\n") == "4 10", "small even input"
assert run("15\n") == "9 6", "odd input"
assert run("1000000\n") == "4 999996", "maximum constraint"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 13 | 9 4 | Giá trị hợp lệ lẻ nhỏ nhất | 
| 14 | 4 10 | Giá trị chẵn nhỏ | 
| 15 | 9 6 | Xây dựng số lẻ | 
| 1000000 | 4 999996 | Hạn chế tối đa | 

## Vỏ cạnh 

Đối với đầu vào tối thiểu:```
12
```thuật toán đi vào nhánh chẵn và xuất ra:```
4 8
```Số thứ hai chính xác là 8, là hợp số. Điều này xác nhận rằng ranh giới dưới hoạt động mà không cần xử lý đặc biệt. 

Đối với đầu vào hợp lệ lẻ nhỏ nhất:```
13
```thuật toán đi vào nhánh lẻ và đưa ra:```
9 4
```Cả hai số đều là hợp số. Đây là trường hợp nhỏ nhất mà`9 + (n - 9)`xây dựng được sử dụng. 

Đối với một giá trị lẻ như:```
15
```đầu ra của thuật toán:```
9 6
```Giá trị thứ hai là số chẵn lớn hơn 2, vì vậy nó là hợp số. Điều này chứng tỏ tại sao việc xây dựng số lẻ tránh được cạm bẫy khi tạo ra số hạng nguyên tố thứ hai. 

Đối với đầu vào tối đa:```
1000000
```đầu ra của thuật toán:```
4 999996
```Số thứ hai là số chẵn và lớn hơn 2 nên là hợp số. Thuật toán thực hiện cùng một lượng công việc không đổi như đối với bất kỳ đầu vào nào khác.
