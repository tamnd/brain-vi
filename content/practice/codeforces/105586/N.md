---
title: "CF 105586N - \u54e5\u4f26\u6bd4\u4e9a\u5927\u9009"
description: "Chúng ta được cung cấp một danh sách nhỏ các ứng cử viên trong một cuộc bầu cử tổng thống, trong đó mỗi ứng cử viên có một tên và một con số đại diện cho số lượng người ủng hộ họ. Nhiệm vụ là xác định ứng cử viên nào có số lượng người ủng hộ lớn nhất và ghi tên ứng cử viên đó."
date: "2026-06-22T14:46:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105586
codeforces_index: "N"
codeforces_contest_name: "\u201c\u534e\u4e3a\u676f\u201d 2024 \u5e74\u5e7f\u4e1c\u5de5\u4e1a\u5927\u5b66 ACM \u65b0\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\uff08\u51b3\u8d5b\uff09"
rating: 0
weight: 105586
solve_time_s: 39
verified: true
draft: false
---

[CF 105586N - \u54e5\u4f26\u6bd4\u4e9a\u5927\u9009](https://codeforces.com/problemset/problem/105586/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách nhỏ các ứng cử viên trong một cuộc bầu cử tổng thống, trong đó mỗi ứng cử viên có một tên và một con số đại diện cho số lượng người ủng hộ họ. Nhiệm vụ là xác định ứng cử viên nào có số lượng người ủng hộ lớn nhất và ghi tên ứng cử viên đó. 

Đầu vào bao gồm một số nguyên duy nhất$n$, theo sau là$n$dòng. Mỗi dòng mô tả một ứng cử viên bằng cách sử dụng một chuỗi cho tên và một số nguyên cho số lượng người ủng hộ họ. Việc đảm bảo rằng tất cả các tên đều khác biệt và tất cả số lượng người ủng hộ đều khác biệt sẽ loại bỏ mọi sự mơ hồ về tỷ số hòa, do đó luôn có chính xác một người chiến thắng duy nhất. 

Dưới góc độ tính toán,$n \le 10$, cực kỳ nhỏ. Kể cả ngây thơ$O(n^2)$hoặc tệ hơn là cách tiếp cận sẽ nhanh một cách tầm thường, nhưng cấu trúc của bài toán cho thấy quét tuyến tính trực tiếp là đủ. Ràng buộc có ý nghĩa duy nhất là việc phân tích cú pháp đầu vào phải chính xác và chúng tôi duy trì mức tối đa một cách chính xác. 

Không có trường hợp cạnh cấu trúc phức tạp nào liên quan đến trùng lặp vì cả tên và giá trị đều được đảm bảo là duy nhất. Chế độ lỗi nhỏ duy nhất là khởi tạo không chính xác trình theo dõi tối đa hoặc xử lý sai phần tử đầu tiên, điều này có thể dẫn đến thiếu giá trị tối đa thực sự khi tất cả các giá trị đều âm hoặc nhỏ, mặc dù ở đây các giá trị là dương. 

Một trường hợp cạnh đại diện là: 

đầu vào:```
3
A 10
B 20
C 15
```Đầu ra đúng là`B`. Cách tiếp cận bất cẩn có thể khởi tạo không chính xác số lượng hỗ trợ tối đa thành 0 và không thành công nếu tất cả các giá trị đều âm trong một vấn đề biến thể, nhưng ở đây các giá trị ít nhất là 1, do đó lỗi cụ thể đó vô hại nhưng vẫn là vấn đề phổ biến trong các vấn đề tương tự. 

## Phương pháp tiếp cận 

Chiến lược trực tiếp nhất là đọc từng ứng viên một, theo dõi ứng viên tốt nhất được thấy cho đến nay. Đối với mỗi mục, chúng tôi so sánh số lượng người ủng hộ mục đó với số lượng tối đa hiện tại. Nếu vượt quá mức tốt nhất hiện tại, chúng tôi sẽ cập nhật cả giá trị tối đa và tên được lưu trữ. 

Một cách giải thích thô bạo sẽ là, đối với mỗi ứng cử viên, hãy quét tất cả những người khác để kiểm tra xem có ai có nhiều người ủng hộ hơn không. Điều đó sẽ liên quan đến$O(n^2)$so sánh, điều này vẫn hoàn toàn tốt cho$n \le 10$, nhưng nó tạo ra những công việc lặp đi lặp lại không cần thiết và mã phức tạp hơn mà không mang lại bất kỳ lợi ích nào. 

Quan sát quan trọng là vấn đề là lựa chọn tối đa một lần truyền thống. Vì chúng ta chỉ cần mức tối đa toàn cục của một tập hợp nhỏ các cặp nên chúng ta không cần bất kỳ sự sắp xếp hoặc so sánh lặp lại nào. Một lần duyệt là đủ để duy trì tính chính xác vì giá trị tối đa của tiền tố luôn có thể được cập nhật tăng dần khi giá trị lớn hơn xuất hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các ứng viên một cách tuần tự và duy trì kết quả tốt nhất cho đến nay. 

1. Đọc số lượng thí sinh$n$. Điều này cho chúng tôi biết chúng tôi sẽ thực hiện bao nhiêu cập nhật trong quá trình quét của mình. 
2. Đọc ứng cử viên đầu tiên và khởi tạo cả tên tốt nhất hiện tại và số người ủng hộ tốt nhất với giá trị này. Điều này tránh việc phải phát minh ra một giá trị trọng điểm và đảm bảo tính chính xác ngay cả khi tất cả các giá trị đều nhỏ. 
3. Đối với mỗi ứng cử viên còn lại, hãy đọc tên và số lượng người ủng hộ. 
4. So sánh số lượng người ủng hộ với giá trị tốt nhất hiện tại. Nếu nó lớn hơn, hãy thay thế cả giá trị tốt nhất được lưu trữ và tên liên quan. Điều này đảm bảo rằng ứng cử viên tốt nhất luôn tương ứng với giá trị tối đa được thấy cho đến nay. 
5. Sau khi xử lý tất cả các ứng viên, xuất ra tên tốt nhất được lưu trữ. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình quét, thuật toán sẽ duy trì tính bất biến rằng cặp được lưu trữ là giá trị tối đa trong số tất cả các ứng cử viên được xử lý cho đến nay. Mỗi ứng cử viên mới sẽ bảo toàn bất biến này (nếu nó không lớn hơn) hoặc khôi phục nó bằng cách thay thế cặp được lưu trữ bằng số lượng người hỗ trợ lớn hơn. Vì mỗi ứng cử viên được xử lý chính xác một lần nên cuối cùng, bất biến sẽ mở rộng đến toàn bộ đầu vào, nghĩa là ứng cử viên được lưu trữ là mức tối đa toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())

best_name = ""
best_count = -1

for i in range(n):
    name, cnt = input().split()
    cnt = int(cnt)
    
    if i == 0 or cnt > best_count:
        best_count = cnt
        best_name = name

print(best_name)
```Việc thực hiện dựa vào việc quét tuyến tính đơn giản. Chúng tôi khởi tạo trình theo dõi tốt nhất bằng cách sử dụng phần tử đầu tiên hoặc cặp trọng điểm trong đó số lượng được đặt thành -1. Điều này đảm bảo rằng mọi giá trị đầu vào hợp lệ sẽ thay thế nó ngay lập tức nếu cần. 

điều kiện`i == 0`đảm bảo rằng chúng tôi luôn khởi tạo chính xác từ dữ liệu thực tế thay vì chỉ dựa vào logic trọng điểm. Sự so sánh`cnt > best_count`thực thi theo dõi tối đa nghiêm ngặt, an toàn vì tất cả số lượng đều khác biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
A 10
B 50
C 20
D 80
E 30
```| Bước | Tên | Đếm | Tên Hay Nhất | Số lượng tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | A | 10 | A | 10 | 
| 2 | B | 50 | B | 50 | 
| 3 | C | 20 | B | 50 | 
| 4 | D | 80 | D | 80 | 
| 5 | E | 30 | D | 80 | 

Số lượng tối đa được cập nhật bất cứ khi nào số lượng người ủng hộ lớn hơn xuất hiện. Ứng cử viên được lưu trữ cuối cùng là D, tương ứng chính xác với giá trị lớn nhất. 

### Ví dụ 2 

đầu vào:```
4
X 5
Y 3
Z 9
W 8
```| Bước | Tên | Đếm | Tên Hay Nhất | Số lượng tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | X | 5 | X | 5 | 
| 2 | Y | 3 | X | 5 | 
| 3 | Z | 9 | Z | 9 | 
| 4 | W | 8 | Z | 9 | 

Quá trình quét chứng tỏ rằng thuật toán chỉ cập nhật khi xuất hiện một giá trị lớn hơn nghiêm ngặt, đảm bảo theo dõi chính xác mức tối đa toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ứng cử viên được xử lý chính xác một lần với so sánh thời gian không đổi | 
| Không gian | O(1) | Chỉ một số biến cố định được lưu trữ | 

Được cho$n \le 10$, thời gian chạy thực sự là tức thời. Ngay cả khi ràng buộc lớn hơn nhiều, chiến lược quét tuyến tính tương tự sẽ vẫn tối ưu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n = int(input().strip())
    best_name = ""
    best_count = -1
    
    for i in range(n):
        name, cnt = input().split()
        cnt = int(cnt)
        if i == 0 or cnt > best_count:
            best_count = cnt
            best_name = name
    
    return best_name

# provided sample
assert run("""6
Keqing 1
LuoXuan 321
XianYa 333
XuFeichi 233
CZY 777
PangXiang 888
MarkMax 1000
""") == "MarkMax"

# minimum size
assert run("""1
Solo 42
""") == "Solo"

# increasing order
assert run("""3
A 1
B 2
C 3
""") == "C"

# decreasing order
assert run("""3
A 3
B 2
C 1
""") == "A"

# mixed values
assert run("""4
A 10
B 5
C 15
D 12
""") == "C"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | Độc tấu | trường hợp cạnh ứng cử viên duy nhất | 
| thứ tự tăng dần | C | cập nhật lặp đi lặp lại | 
| thứ tự giảm dần | A | không cập nhật sau lần đầu tiên | 
| giá trị hỗn hợp | C | theo dõi không đơn điệu | 

## Vỏ cạnh 

Vì các ràng buộc đảm bảo có ít nhất một ứng cử viên nên trường hợp cạnh cấu trúc duy nhất là xử lý$n = 1$. Trong trường hợp đó, việc khởi tạo từ phần tử đầu tiên ngay lập tức làm cho nó trở thành ứng cử viên đầu tiên và tốt nhất, đồng thời vòng lặp không sửa đổi nó. 

Ví dụ:```
1
Solo 42
```Bộ thuật toán`best_name = Solo`,`best_count = 42`và bỏ qua tất cả các bản cập nhật. Đầu ra vẫn còn`Solo`, điều này đúng theo định nghĩa. 

Không có trường hợp đặc biệt nào khác ảnh hưởng đến tính chính xác vì không có ràng buộc và không có giá trị bị thiếu, vì vậy mọi so sánh đều mang tính xác định nghiêm ngặt.
