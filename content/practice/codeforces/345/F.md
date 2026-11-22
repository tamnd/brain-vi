---
title: "CF 345F - Kiểm tra mê tín"
description: "Đầu vào không được cung cấp ở định dạng có cấu trúc thông thường với số lượng. Thay vào đó, nó là một tệp văn bản nhỏ xen kẽ giữa tên quốc gia và danh sách những điều mê tín. Dòng không bắt đầu bằng tên quốc gia."
date: "2026-06-07T15:47:38+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 345
codeforces_index: "F"
codeforces_contest_name: "Friday the 13th, Programmers Day"
rating: 2700
weight: 345
solve_time_s: 114
verified: false
draft: false
---

[CF 345F - Kiểm tra mê tín](https://codeforces.com/problemset/problem/345/F) 

**Đánh giá:** 2700 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào không được cung cấp ở định dạng có cấu trúc thông thường với số lượng. Thay vào đó, nó là một tệp văn bản nhỏ xen kẽ giữa tên quốc gia và danh sách những điều mê tín. 

Một dòng **không** bắt đầu bằng`* `là tên một quốc gia Một dòng bắt đầu bằng`* `là một sự mê tín thuộc về quốc gia được nhìn thấy gần đây nhất. 

Tên quốc gia được đảm bảo là duy nhất. Một đất nước có thể không có sự mê tín. Nhiệm vụ là tìm ra những điều mê tín nào xuất hiện ở nhiều quốc gia nhất. 

Phần khó khăn là các tên được so sánh ở dạng chuẩn hóa. Phải bỏ qua trường hợp chữ cái và nhiều dấu cách giữa các từ không thành vấn đề. Hai tên biểu thị cùng một chuỗi nếu sau khi tách thành các từ, các chuỗi từ giống hệt nhau, bỏ qua chữ hoa chữ thường. 

Ví dụ:```
Friday the   13th
friday   THE 13TH
```đại diện cho sự mê tín tương tự và phải được coi như một đối tượng. 

Đầu ra phải chứa mọi mê tín có số lượng quốc gia là tối đa. Tên phải được in bằng chữ thường và sắp xếp theo thứ tự từ điển theo chuỗi từ. 

Dữ liệu nhập chứa tối đa 50 dòng và mỗi dòng chứa tối đa 50 ký tự. Ngay cả một giải pháp không hiệu quả cũng chỉ xử lý được vài nghìn ký tự. Thách thức không phải là hiệu suất mà là phân tích cú pháp và chuẩn hóa dữ liệu một cách chính xác. 

Một số trường hợp nguy hiểm có thể âm thầm phá vỡ việc triển khai bất cẩn. 

Xem xét khoảng cách không nhất quán:```
A
* black   cat
B
* black cat
```Đầu ra đúng:```
black cat
```Một giải pháp so sánh các chuỗi thô sẽ coi những chuỗi này là những mê tín khác nhau một cách không chính xác. 

Xem xét cách viết hoa khác nhau:```
A
* Friday The 13th
B
* friday the 13th
```Đầu ra đúng:```
friday the 13th
```Một so sánh phân biệt chữ hoa chữ thường sẽ tính hai mê tín riêng biệt. 

Hãy xem xét những quốc gia không có mê tín dị đoan:```
A
B
* lucky coin
```Đầu ra đúng:```
lucky coin
```Trình phân tích cú pháp phải nhận ra rằng`B`bắt đầu một đất nước mới mặc dù`A`không có mục mê tín dị đoan. 

Xem xét thứ tự từ điển của các chuỗi từ:```
A
* zebra
B
* apple
```Cả hai đều xảy ra một lần, vì vậy cả hai đều phải được xuất ra:```
apple
zebra
```Thứ tự được áp dụng sau khi chuẩn hóa. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trước tiên sẽ phân tích tất cả các quốc gia và lưu trữ, đối với mỗi quốc gia, tập hợp những điều mê tín của quốc gia đó. Sau đó, đối với từng mê tín riêng biệt, hãy rà soát từng quốc gia và kiểm tra xem liệu mê tín có xuất hiện ở đó hay không. Nếu có`S`những mê tín riêng biệt và`C`các nước, điều này đòi hỏi`O(SC)`kiểm tra thành viên sau khi phân tích cú pháp. 

Với các ràng buộc thực tế, điều này sẽ dễ dàng phù hợp vì toàn bộ đầu vào chứa tối đa 50 dòng. Tuy nhiên, nó vẫn thực hiện công việc không cần thiết. 

Nhận xét quan trọng là số lượng chúng ta cần chỉ đơn giản là số quốc gia chứa đựng từng điều mê tín. Khi một dòng mê tín được đọc và chuẩn hóa, chúng ta đã biết nó thuộc về quốc gia nào. Chúng ta có thể tăng cường một biện pháp toàn cầu chống lại sự mê tín đó ngay lập tức. 

Tuyên bố vấn đề đảm bảo rằng các mục trong một quốc gia là duy nhất, vì vậy mỗi mê tín đóng góp tối đa một số lượng cho mỗi quốc gia. Sau khi xử lý toàn bộ tập tin, mọi mê tín đều có tần suất quốc gia chính xác. 

Công việc còn lại rất đơn giản. Tìm tần số tối đa, thu thập mọi mê tín có tần số đó, sắp xếp chúng theo từ điển và in chúng. 

Mặc dù ở đây cả hai cách tiếp cận đều đủ nhanh, nhưng cách đếm đơn giản hơn và trực tiếp khớp với số lượng chúng ta được yêu cầu tính toán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(SC) | O(S + tổng số mục) | Đã chấp nhận | 
| Tối ưu | O(tổng số dòng · chiều dài dòng) | O(S) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các dòng đầu vào. 
2. Duy trì một cuốn từ điển`cnt`ánh xạ một tên mê tín được chuẩn hóa theo số quốc gia chứa nó. 
3. Xử lý từng dòng theo thứ tự. 
4. Nếu dòng bắt đầu bằng`* `, nó thể hiện sự mê tín. 

Bình thường hóa nó bằng cách loại bỏ phần đầu`* `, tách thành các từ, chuyển đổi mọi từ thành chữ thường và nối các từ bằng một khoảng trắng. 
5. Tăng`cnt[normalized_name]`. 

Điều này đúng vì mọi mục mê tín đều thuộc về chính xác một quốc gia và các mục trong một quốc gia được đảm bảo là duy nhất. 
6. Nếu dòng không bắt đầu bằng`* `, đó là tên một quốc gia. Không cần thực hiện hành động đếm nào. 
7. Sau khi tất cả các dòng được xử lý, hãy tính giá trị lớn nhất được lưu trong`cnt`. 
8. Thu thập mọi mê tín có số lượng bằng mức tối đa này. 
9. Sắp xếp các tên đã thu thập theo từ điển. 
10. In chúng từng dòng một. 

### Tại sao nó hoạt động 

Sau khi chuẩn hóa, mọi cách trình bày văn bản của cùng một sự mê tín sẽ trở thành một chuỗi giống hệt nhau. Mỗi mục mê tín đóng góp một lần xuất hiện cho chính xác một quốc gia và các mục trùng lặp trong một quốc gia đều bị cấm theo tuyên bố. Do đó, giá trị được lưu trữ trong`cnt[x]`Sau khi xử lý toàn bộ file chính xác là số quốc gia có chứa mê tín`x`. 

Thuật toán đưa ra chính xác những mê tín có số lượng bằng mức tối đa toàn cầu, đó là định nghĩa của câu trả lời bắt buộc. Sắp xếp các chuỗi chuẩn hóa sẽ tạo ra thứ tự từ điển cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def normalize(s: str) -> str:
    return " ".join(s.lower().split())

def solve():
    lines = [line.rstrip("\n") for line in sys.stdin]

    cnt = {}

    for line in lines:
        if line.startswith("* "):
            superstition = normalize(line[2:])
            cnt[superstition] = cnt.get(superstition, 0) + 1

    best = max(cnt.values())

    ans = sorted(
        name for name, value in cnt.items()
        if value == best
    )

    sys.stdout.write("\n".join(ans))

solve()
```các`normalize`chức năng là trái tim của giải pháp. Đang gọi`split()`không có đối số sẽ thu gọn bất kỳ số khoảng trắng liên tiếp nào thành một dấu phân cách duy nhất. Việc chuyển đổi sang chữ thường sẽ xử lý việc so sánh không phân biệt chữ hoa chữ thường. Việc kết hợp bằng một khoảng trắng sẽ tạo ra một biểu diễn chuẩn. 

Tên quốc gia bị bỏ qua sau khi phân tích cú pháp vì chỉ có tần số mê tín mới quan trọng. Tuyên bố đảm bảo rằng tên quốc gia là khác biệt và các mục mê tín trong một quốc gia là duy nhất, do đó không cần phải ghi sổ kế toán bổ sung. 

Một lỗi phổ biến là gọi`lower()`nhưng giữ nguyên khoảng cách ban đầu. Sau đó`"black cat"`Và`"black   cat"`vẫn sẽ được coi là các chuỗi khác nhau. sử dụng`split()`theo sau là`" ".join(...)`tránh được vấn đề này. 

Một lỗi dễ mắc phải khác là sắp xếp trước khi chuẩn hóa. Đầu ra phải chứa các dạng chuẩn chữ thường, vì vậy việc chuẩn hóa phải diễn ra trước tiên và các chuỗi chuẩn hóa phải được lưu trữ trong từ điển. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
Ukraine
* Friday the   13th
* black   cat
* knock the   wood
USA
* wishing well
* friday   the   13th
Holland
France
* Wishing Well
```Xử lý dấu vết: 

| Dòng | Sự mê tín được bình thường hóa | Đếm sau khi cập nhật | 
| --- | --- | --- | 
| * Thứ sáu ngày 13 | thứ sáu ngày 13 | 1 | 
| *mèo đen | mèo đen | 1 | 
| * gõ gỗ | gõ gỗ | 1 | 
| * chúc mọi điều tốt lành | chúc mọi điều tốt lành | 1 | 
| * thứ sáu ngày 13 | thứ sáu ngày 13 | 2 | 
| * Chúc Tốt | chúc mọi điều tốt lành | 2 | 

Tính toán cuối cùng: 

| Mê tín dị đoan | Đếm | 
| --- | --- | 
| thứ sáu ngày 13 | 2 | 
| chúc mọi điều tốt lành | 2 | 
| mèo đen | 1 | 
| gõ gỗ | 1 | 

Tần số tối đa là 2. 

Đầu ra:```
friday the 13th
wishing well
```Ví dụ này thể hiện cả hai quy tắc chuẩn hóa. Cách viết hoa khác nhau và khoảng cách khác nhau sẽ thu gọn thành cùng một tên chuẩn. 

### Ví dụ 2 

đầu vào:```
A
* Lucky Coin
B
* lucky   coin
C
* rabbit foot
```Xử lý dấu vết: 

| Dòng | Sự mê tín được bình thường hóa | Đếm sau khi cập nhật | 
| --- | --- | --- | 
| * Đồng xu may mắn | đồng xu may mắn | 1 | 
| * đồng xu may mắn | đồng xu may mắn | 2 | 
| *chân thỏ | chân thỏ | 1 | 

Tính toán cuối cùng: 

| Mê tín dị đoan | Đếm | 
| --- | --- | 
| đồng xu may mắn | 2 | 
| chân thỏ | 1 | 

Đầu ra:```
lucky coin
```Dấu vết này xác nhận rằng nhiều cách viết văn bản của cùng một sự mê tín đều góp phần tạo nên một bộ đếm tần số duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi ký tự của đầu vào được xử lý một số lần không đổi | 
| Không gian | O(S) | Một mục từ điển cho mỗi mê tín khác nhau | 

Đây`T`là tổng số ký tự trong đầu vào và`S`là số lượng mê tín được bình thường hóa riêng biệt. Với tối đa 50 dòng ngắn, thời gian chạy không đáng kể so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    lines = inp.splitlines()

    cnt = {}

    for line in lines:
        if line.startswith("* "):
            s = " ".join(line[2:].lower().split())
            cnt[s] = cnt.get(s, 0) + 1

    best = max(cnt.values())
    ans = sorted(k for k, v in cnt.items() if v == best)

    return "\n".join(ans)

# provided sample
assert run(
"""Ukraine
* Friday the   13th
* black   cat
* knock the   wood
USA
* wishing well
* friday   the   13th
Holland
France
* Wishing Well
"""
) == "friday the 13th\nwishing well"

# minimum valid input
assert run(
"""A
* X
"""
) == "x"

# spacing normalization
assert run(
"""A
* black   cat
B
* black cat
"""
) == "black cat"

# case normalization
assert run(
"""A
* Lucky Coin
B
* lucky coin
"""
) == "lucky coin"

# multiple winners requiring sorting
assert run(
"""A
* zebra
B
* apple
"""
) == "apple\nzebra"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một đất nước, một mê tín |`x`| Đầu vào hợp lệ tối thiểu | 
| Khoảng cách khác nhau |`black cat`| Chuẩn hóa không gian | 
| Viết hoa khác nhau |`lucky coin`| Khớp không phân biệt chữ hoa chữ thường | 
| Hai người chiến thắng khác biệt |`apple`,`zebra`| Thứ tự đầu ra từ điển | 

## Vỏ cạnh 

Xem xét khoảng cách không nhất quán:```
A
* black   cat
B
* black cat
```Mục đầu tiên bình thường hóa thành`black cat`. Mục thứ hai bình thường hóa chính xác thành cùng một chuỗi. Số lượng từ điển trở thành 2. Thuật toán đưa ra:```
black cat
```Xem xét cách viết hoa không nhất quán:```
A
* Friday The 13th
B
* friday the 13th
```Cả hai mục đều bình thường hóa thành`friday the 13th`. Số đếm trở thành 2 và đầu ra là:```
friday the 13th
```Hãy xem xét các quốc gia không có danh sách mê tín:```
A
B
* lucky coin
```Trình phân tích cú pháp xử lý cả hai`A`Và`B`làm tên quốc gia vì không dòng nào bắt đầu bằng`* `. Chỉ có một mục mê tín được xử lý, tạo ra:```
lucky coin
```Hãy xem xét một số mê tín được xếp ở vị trí đầu tiên:```
A
* zebra
B
* apple
```Cả hai tần số đều là 1. Thuật toán thu thập cả hai tên và sắp xếp chúng theo từ điển, tạo ra:```
apple
zebra
```Điều này xác nhận rằng các mối quan hệ được xử lý chính xác và việc sắp xếp đó xảy ra sau khi chuẩn hóa.
