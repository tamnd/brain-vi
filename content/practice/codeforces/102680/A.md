---
title: "CF 102680A - Chuyển hóa đơn"
description: "Bài toán được cố ý gói gọn trong một câu chuyện dài nhưng khả năng tính toán thực tế lại cực kỳ nhỏ. Ủy ban xem xét một phiên họp có n dự luật. Mọi dự luật đạt được số phiếu bầu đều nhận được sự chấp thuận nhất trí, vì vậy mọi dự luật được bỏ phiếu đều được thông qua."
date: "2026-08-03T03:53:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102680
codeforces_index: "A"
codeforces_contest_name: "Brookfield Computer Programming Challenge 1"
rating: 0
weight: 102680
solve_time_s: 62
verified: true
draft: false
---

[CF 102680A - Chuyển hóa đơn](https://codeforces.com/problemset/problem/102680/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Bài toán được cố ý gói gọn trong một câu chuyện dài nhưng khả năng tính toán thực tế lại cực kỳ nhỏ. Ủy ban xem xét một phiên họp bao gồm`n`hóa đơn. Mọi dự luật đạt được số phiếu bầu đều nhận được sự chấp thuận nhất trí, vì vậy mọi dự luật được bỏ phiếu đều được thông qua. Giá trị đầu vào thứ hai,`f`, thể hiện lượng khoai tây chiên giả định được tiêu thụ và không ảnh hưởng đến kết quả. Nhiệm vụ chỉ đơn giản là xuất ra số lượng dự luật sẽ được thông qua, tức là số dự luật được biểu quyết. 

Đầu vào chứa hai số nguyên. Cái đầu tiên,`n`, là số lượng hóa đơn được xem xét trong phiên họp. Cái thứ hai,`f`, là dữ liệu không liên quan được đưa vào như một phần của trò đùa trong tuyên bố. Đầu ra phải là số lượng hóa đơn được chuyển. 

Những hạn chế là rất nhỏ, với`n`từ`0`ĐẾN`99`Và`f`từ`0`ĐẾN`20`. Điều này có nghĩa là bất kỳ cách tiếp cận thông thường nào cũng có thể dễ dàng phù hợp, nhưng mục tiêu thực sự là nhận ra rằng không cần mô phỏng hoặc thuật toán. Vì câu trả lời chỉ phụ thuộc vào`n`, thời gian chạy phải không đổi. 

Các trường hợp chính xuất phát từ các giá trị có thể khiến ai đó vô tình đưa ra logic không cần thiết. Khi không có dự luật, câu trả lời là 0 vì không có gì để bỏ phiếu. 

Ví dụ:```
Input:
0 15

Output:
0
```Một giải pháp bất cẩn có thể cho rằng có ít nhất một tờ tiền tồn tại và in ra một giá trị dương. 

Trường hợp thứ hai là khi số lượng cá bột chiên không bình thường. Giá trị của`f`không bao giờ nên thay đổi câu trả lời. 

Ví dụ:```
Input:
7 0

Output:
7
```Một giải pháp cố gắng rút ra mối quan hệ nào đó giữa khoai tây chiên và hóa đơn sẽ thất bại ở đây vì con số thứ hai hoàn toàn không liên quan. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ lặp lại mọi hóa đơn, kiểm tra xem nó có được phê duyệt hay không và đếm nó. Điều này đúng về mặt logic vì mọi dự luật đều được đảm bảo thông qua. Tuy nhiên, việc mô phỏng là không cần thiết vì tuyên bố đã đưa ra kết quả của mỗi cuộc bỏ phiếu. Ngay cả khi số lượng tờ tiền cực kỳ lớn, việc lặp lại cùng một thao tác cho mỗi tờ tiền sẽ chỉ tìm lại được thông tin mà chúng ta đã biết. 

Quan sát quan trọng là đầu ra chính xác là giá trị đầu vào đầu tiên. Quá trình bỏ phiếu không thay đổi số lượng hóa đơn và giá trị đầu vào bổ sung không ảnh hưởng đến bất cứ điều gì. Toàn bộ vấn đề giảm xuống việc đọc số nguyên đầu tiên và in nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Được chấp nhận, nhưng không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai số nguyên từ đầu vào. Chỉ có giá trị đầu tiên quan trọng vì nó đại diện cho số lượng dự luật sẽ được biểu quyết. 
2. Đầu ra`n`trực tiếp. Mọi dự luật đều đảm bảo nhận được sự nhất trí thông qua nên số lượng dự luật được thông qua không thay đổi. 

Tại sao nó hoạt động: 

Bất biến đằng sau giải pháp là mỗi dự luật được xem xét trong phiên họp đóng góp chính xác một dự luật được thông qua. Vì có`n`hóa đơn và không có hóa đơn nào có thể thất bại, số hóa đơn cuối cùng được thông qua vẫn còn`n`. Giá trị đầu vào thứ hai không bao giờ xuất hiện trong lý do này, vì vậy việc bỏ qua nó không thể ảnh hưởng đến tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, f = map(int, input().split())
print(n)
```Chương trình đọc cả hai giá trị vì chúng có ở định dạng đầu vào nhưng chỉ lưu trữ giá trị đầu tiên để tính toán. Biến`f`được đọc và cố ý không sử dụng vì báo cáo vấn đề xác định nó là thông tin không liên quan. 

Không có vòng lặp, mảng hoặc phép biến đổi số học. Điều này tránh được những công việc không cần thiết và cũng tránh được những sai sót có thể xảy ra như cố gắng mô hình hóa quy trình bỏ phiếu theo cách thủ công. 

Giới hạn đầu vào nhỏ nên việc tràn số nguyên không phải là vấn đề đáng lo ngại trong Python. Cũng không có điều kiện biên liên quan đến việc lập chỉ mục vì giải pháp không truy cập bất kỳ cấu trúc dữ liệu nào. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
61 19
```Thuật toán chỉ cần số lượng hóa đơn. 

| Bước | n | f | Đầu ra | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 61 | 19 | | 
| In n | 61 | 19 | 61 | 

Dấu vết cho thấy số lượng cá bột không ảnh hưởng đến kết quả. Tất cả 61 dự luật đều được thông qua. 

### Mẫu 2 

đầu vào:```
36 10
```| Bước | n | f | Đầu ra | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 36 | 10 | | 
| In n | 36 | 10 | 36 | 

Ví dụ này xác nhận rằng một giá trị khác của`f`vẫn để lại câu trả lời bằng số hóa đơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chương trình chỉ thực hiện phân tích cú pháp đầu vào và một thao tác đầu ra. | 
| Không gian | O(1) | Chỉ có hai biến số nguyên được lưu trữ. | 

Các ràng buộc dễ dàng được thỏa mãn vì giải pháp không phụ thuộc vào kích thước của các giá trị đầu vào. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(data: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(data)
    input = sys.stdin.readline
    n, f = map(int, input().split())
    ans = str(n)
    sys.stdin = old_stdin
    return ans

# provided samples
assert solve("61 19\n") == "61", "sample 1"
assert solve("36 10\n") == "36", "sample 2"

# custom cases
assert solve("0 20\n") == "0", "no bills"
assert solve("1 0\n") == "1", "single bill"
assert solve("99 20\n") == "99", "maximum number of bills"
assert solve("50 5\n") == "50", "different fries value ignored"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 20`|`0`| Xử lý số lượng hóa đơn tối thiểu. | 
|`1 0`|`1`| Xử lý phiên tích cực nhỏ nhất. | 
|`99 20`|`99`| Xử lý số lượng hóa đơn tối đa được phép. | 
|`50 5`|`50`| Xác nhận giá trị đầu vào thứ hai không có hiệu lực. | 

## Vỏ cạnh 

Khi nào`n = 0`, thuật toán đọc số 0 và in số 0 ngay lập tức. Không có hóa đơn nào được thông qua nên kết quả là chính xác.```python
Input:
0 15

Execution:
n = 0
print(n)

Output:
0
```Khi`f`thay đổi trong khi`n`giữ nguyên thì kết quả đầu ra không đổi vì kết quả biểu quyết chỉ phụ thuộc vào số tờ tiền.```python
Input:
7 0

Execution:
n = 7
f = 0
print(n)

Output:
7
```Thuật toán xử lý việc này vì nó không bao giờ sử dụng`f`trong tính toán. Giá trị duy nhất quyết định câu trả lời là số lượng dự luật tham gia phiên biểu quyết.
