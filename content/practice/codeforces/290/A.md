---
title: "CF 290A - Những sợi dây bí ẩn"
description: "Toàn bộ nhiệm vụ là một bài toán đùa trong cuộc thi Cá tháng Tư. Đầu vào là một số nguyên a từ 1 đến 40. Thay vì tính toán bất cứ thứ gì, chúng ta phải xuất ra một chuỗi cụ thể liên quan đến vị trí đó."
date: "2026-06-05T16:49:16+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 290
codeforces_index: "A"
codeforces_contest_name: "April Fools Day Contest 2013"
rating: 1400
weight: 290
solve_time_s: 83
verified: true
draft: false
---

[CF 290A - Những sợi dây bí ẩn](https://codeforces.com/problemset/problem/290/A) 

**Đánh giá:** 1400 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Toàn bộ nhiệm vụ là một bài toán đùa trong cuộc thi Cá tháng Tư. 

Đầu vào là một số nguyên duy nhất`a`trong khoảng từ 1 đến 40. Thay vì tính toán bất cứ thứ gì, chúng ta phải xuất ra một chuỗi cụ thể liên quan đến vị trí đó. Các giá trị mẫu cho thấy rằng`2`bản đồ tới`"Adams"`,`8`bản đồ tới`"Van Buren"`, Và`29`bản đồ tới`"Harding"`. Điều quan sát ẩn giấu là các con số tương ứng với bốn mươi tổng thống đầu tiên của Hoa Kỳ, theo thứ tự thời gian. Đầu ra được yêu cầu chỉ đơn giản là họ của`a`-Chủ tịch thứ 

Hạn chế là rất nhỏ. Từ`a`không bao giờ vượt quá 40, bất kỳ giải pháp nào lưu trữ bốn mươi chuỗi và thực hiện một lần tra cứu đều có hiệu quả ngay lập tức. 

Nguồn sai lầm thực sự duy nhất là việc lập chỉ mục. 

Ví dụ, với đầu vào```
1
```đầu ra đúng là```
Washington
```vì danh sách được đánh số bắt đầu từ 1. Việc triển khai bất cẩn sử dụng đầu vào trực tiếp làm chỉ mục mảng dựa trên số 0 sẽ trả về mục nhập thứ hai thay thế. 

Một lỗi phổ biến khác xuất hiện với đầu vào```
40
```Đầu ra đúng là```
Reagan
```Mảng chứa chính xác bốn mươi phần tử, vì vậy chỉ mục dựa trên số 0 hợp lệ cuối cùng là`39`. Truy cập chỉ mục`40`sẽ vượt quá giới hạn. 

## Phương pháp tiếp cận 

Quan điểm bạo lực sẽ là kiểm tra thủ công tất cả các giá trị có thể có của`a`và in câu trả lời tương ứng bằng cách sử dụng một chuỗi dài các`if`các tuyên bố. Vì chỉ có bốn mươi khả năng nên điều này hoàn toàn đủ nhanh. 

Một cách tiếp cận rõ ràng hơn sẽ lưu trữ tất cả bốn mươi họ trong một mảng. Sau đó, vấn đề trở thành tra cứu bảng trực tiếp. Giá trị đầu vào dựa trên 1, trong khi mảng thường dựa trên 0, do đó câu trả lời được lưu ở vị trí`a - 1`. 

Phiên bản brute-force đã chạy trong thời gian không đổi vì phạm vi đầu vào được cố định. Giải pháp mảng được ưa chuộng hơn vì nó ngắn hơn, dễ xác minh hơn và ít xảy ra lỗi hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu (`if`chuỗi) | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu (tra cứu mảng) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng chứa họ của 40 tổng thống đầu tiên của Hoa Kỳ theo thứ tự thời gian. 
2. Đọc số nguyên`a`. 
3. Chuyển đổi vị trí dựa trên 1 thành chỉ số mảng dựa trên 0 bằng cách tính toán`a - 1`. 
4. Xuất chuỗi được lưu trữ tại chỉ mục đó. 

Tại sao nó hoạt động: 

Mảng được xây dựng sao cho vị trí đó`i`chứa họ tương ứng với số chủ tịch`i + 1`. Giá trị đầu vào`a`yêu cầu`a`- mục thứ trong chuỗi đó. Truy cập chỉ mục`a - 1`truy xuất chính xác họ đó, do đó kết quả đầu ra được tạo ra khớp với ánh xạ cần thiết cho mọi đầu vào hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

presidents = [
    "Washington", "Adams", "Jefferson", "Madison", "Monroe",
    "Adams", "Jackson", "Van Buren", "Harrison", "Tyler",
    "Polk", "Taylor", "Fillmore", "Pierce", "Buchanan",
    "Lincoln", "Johnson", "Grant", "Hayes", "Garfield",
    "Arthur", "Cleveland", "Harrison", "Cleveland",
    "McKinley", "Roosevelt", "Taft", "Wilson", "Harding",
    "Coolidge", "Hoover", "Roosevelt", "Truman",
    "Eisenhower", "Kennedy", "Johnson", "Nixon",
    "Ford", "Carter", "Reagan"
]

a = int(input())
print(presidents[a - 1])
```Mảng chứa toàn bộ ánh xạ mà bài toán yêu cầu. Việc đọc đầu vào sẽ cho giá trị từ 1 đến 40. Vì danh sách Python được lập chỉ mục từ 0 nên phần tử đúng là`presidents[a - 1]`. 

Điểm tinh tế duy nhất là chuyển đổi chỉ mục. quên đi`- 1`dịch chuyển mọi câu trả lời theo một vị trí và gây ra câu trả lời sai hoặc quyền truy cập ngoài phạm vi khi`a = 40`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
```| Bước | một | Chỉ mục được sử dụng | Đầu ra | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 2 | - | - | 
| Tính chỉ số | 2 | 1 | - | 
| Tra cứu | 2 | 1 | Adams | 

Đầu ra:```
Adams
```Dấu vết này hiển thị chuyển đổi dựa trên 1 thành 0. Chủ tịch thứ hai được lưu trữ tại chỉ số mảng 1. 

### Ví dụ 2 

đầu vào:```
29
```| Bước | một | Chỉ mục được sử dụng | Đầu ra | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 29 | - | - | 
| Tính chỉ số | 29 | 28 | - | 
| Tra cứu | 29 | 28 | Làm cứng | 

Đầu ra:```
Harding
```Ví dụ này xác nhận rằng giải pháp này hoạt động tốt như nhau đối với các vị trí nằm sâu trong danh sách. Không cần lặp lại hoặc tìm kiếm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Truy cập một mảng | 
| Không gian | O(1) | Mảng có kích thước cố định gồm 40 chuỗi | 

Thời gian chạy không phụ thuộc vào giá trị đầu vào. Một thao tác tra cứu và in dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

PRESIDENTS = [
    "Washington", "Adams", "Jefferson", "Madison", "Monroe",
    "Adams", "Jackson", "Van Buren", "Harrison", "Tyler",
    "Polk", "Taylor", "Fillmore", "Pierce", "Buchanan",
    "Lincoln", "Johnson", "Grant", "Hayes", "Garfield",
    "Arthur", "Cleveland", "Harrison", "Cleveland",
    "McKinley", "Roosevelt", "Taft", "Wilson", "Harding",
    "Coolidge", "Hoover", "Roosevelt", "Truman",
    "Eisenhower", "Kennedy", "Johnson", "Nixon",
    "Ford", "Carter", "Reagan"
]

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    a = int(sys.stdin.readline())
    return PRESIDENTS[a - 1] + "\n"

# provided sample
assert run("2\n") == "Adams\n", "sample 1"

# custom cases
assert run("1\n") == "Washington\n", "first element"
assert run("40\n") == "Reagan\n", "last element"
assert run("8\n") == "Van Buren\n", "middle value"
assert run("29\n") == "Harding\n", "another known mapping"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`Washington`| Chỉ số hợp lệ tối thiểu | 
|`40`|`Reagan`| Chỉ số hợp lệ tối đa | 
|`8`|`Van Buren`| Tra cứu chính xác ở giữa | 
|`29`|`Harding`| Một vị trí không hề tầm thường khác | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất:```
1
```Thuật toán tính toán`1 - 1 = 0`và truy cập phần tử mảng đầu tiên. Đầu ra là:```
Washington
```Điều này xác nhận rằng vị trí đầu tiên được xử lý chính xác và không xảy ra việc lập chỉ mục tiêu cực. 

Hãy xem xét đầu vào lớn nhất:```
40
```Thuật toán tính toán`40 - 1 = 39`và truy cập vị trí mảng hợp lệ cuối cùng. Đầu ra là:```
Reagan
```Điều này xác minh rằng việc triển khai nằm trong giới hạn mảng. 

Hãy xem xét một giá trị gần đầu:```
2
```Chỉ số được tính toán là`1`, sản xuất:```
Adams
```Trường hợp này bắt gặp lỗi từng cái một cổ điển. Nếu phép trừ bị bỏ qua, chương trình sẽ trả về sai`"Jefferson"`thay vì.
