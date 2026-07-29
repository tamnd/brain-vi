---
title: "CF 102757D - Điều Tuyệt vời nhất Kể từ Bánh mì Cắt lát"
description: "Bài toán mô tả một tập hợp các phát minh mang tính lịch sử. Mỗi phát minh đều có tiêu đề, ngày phát minh và điểm số thể hiện mức độ tuyệt vời của nó đối với bánh mì cắt lát."
date: "2026-07-29T00:24:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102757
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 10-09-20 Div. 2"
rating: 0
weight: 102757
solve_time_s: 78
verified: true
draft: false
---

[CF 102757D - Điều tuyệt vời nhất kể từ khi cắt bánh mì](https://codeforces.com/problemset/problem/102757/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán mô tả một tập hợp các phát minh mang tính lịch sử. Mỗi phát minh đều có tiêu đề, ngày phát minh và điểm số thể hiện mức độ tuyệt vời của nó đối với bánh mì cắt lát. Mục đích là tìm ra phát minh vĩ đại nhất xảy ra sau khi bánh mì cắt lát được phát minh, nơi bánh mì cắt lát được cho là đã xuất hiện vào ngày 7 tháng 7 năm 1928. 

Đầu vào chứa một số hồ sơ phát minh. Mỗi bản ghi bao gồm một tên, một ngày được viết bằng chữ viết tắt gồm ba chữ cái của tháng, theo sau là ngày và năm và số thập phân từ 0 đến 1. Đầu ra chỉ đơn giản là tên của phát minh hợp lệ có số điểm lớn nhất. 

Số lượng phát minh có thể lên tới 10.000. Nó đủ nhỏ để chúng ta có thể kiểm tra mọi phát minh một lần. Bất kỳ cách tiếp cận nào so sánh từng cặp phát minh sẽ thực hiện khoảng 100 triệu so sánh trong trường hợp xấu nhất, điều này là không cần thiết đối với tìm kiếm tối đa đơn giản. 

Khó khăn chính trong việc triển khai không phải là việc tìm kiếm mà là việc diễn giải ngày tháng một cách chính xác. Một giải pháp bất cẩn có thể so sánh ngày tháng dưới dạng chuỗi hoặc quên rằng năm không phải lúc nào cũng có bốn chữ số. Ngày tháng phải được chuyển đổi thành cách trình bày trong đó trật tự thời gian được giữ nguyên. 

Ví dụ: một đầu vào như:```
3
Old Machine
Jan 1, 3000
0.9
New Machine
Jan 1, 2000
0.8
Ancient Tool
Jan 1, 100
0.99
```chỉ có một phát minh hợp lệ, Máy Cũ, vì hai phát minh còn lại được tạo ra trước bánh mì cắt lát. Đầu ra đúng là:```
Old Machine
```Giải pháp chỉ chọn điểm lớn nhất sẽ tạo ra Công cụ Cổ đại không chính xác. 

Một trường hợp khác là một phát minh được tạo ra chính xác vào ngày 7 tháng 7 năm 1928. Nó không được tính vì cách diễn đạt yêu cầu những phát minh được thực hiện sau bánh mì cắt lát. Ví dụ:```
2
Exact Date
Jul 7, 1928
1.0
Later
Jul 8, 1928
0.5
```Đầu ra đúng là:```
Later
```Việc so sánh sử dụng lớn hơn hoặc bằng sẽ chọn Ngày Chính xác không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là lưu trữ mọi phát minh và kiểm tra tất cả các lựa chọn có thể. Đối với mỗi phát minh, chúng tôi kiểm tra xem nó có được tạo ra sau ngày 7 tháng 7 năm 1928 hay không, sau đó so sánh điểm của nó với mọi phát minh hợp lệ khác. Điều này đúng vì số điểm cao nhất trong số tất cả các ứng viên hợp lệ cũng phải được tìm thấy trong số những so sánh đó. Tuy nhiên, với 10.000 phát minh, nó thực hiện khoảng 100 triệu so sánh, trong khi việc giải bài toán không yêu cầu bất kỳ sự tương tác nào giữa các phát minh. 

Quan sát hữu ích là điều này chỉ yêu cầu giá trị tối đa trong số các bản ghi được lọc. Chúng ta không cần biết bất cứ điều gì về những phát minh khác sau khi đã xử lý chúng. Chúng tôi có thể quét đầu vào một lần, loại bỏ các phát minh trước hoặc vào ngày bánh mì cắt lát và giữ lại điểm số cao nhất được thấy cho đến nay. 

Giải pháp vũ phu có hiệu quả vì cuối cùng nó sẽ xem xét mọi người có thể chiến thắng, nhưng nó thất bại vì lặp lại những so sánh không cần thiết. Việc quan sát thấy mức tối đa có thể được duy trì dần dần sẽ giảm vấn đề xuống còn một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(N) | Về mặt khái niệm quá chậm | 
| Tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số phát minh. Xử lý từng hồ sơ sáng chế một cách độc lập vì câu trả lời chỉ phụ thuộc vào ứng viên hợp lệ nhất được tìm thấy cho đến nay. 
2. Chuyển ngày phát minh sang dạng số có thể so sánh được. Một cách biểu diễn thuận tiện là năm, tháng và ngày được lưu trữ dưới dạng một bộ dữ liệu. Các bộ dữ liệu so sánh theo trình tự thời gian khi được sắp xếp từ đơn vị lớn nhất đến đơn vị nhỏ nhất. 
3. Kiểm tra xem ngày của sáng chế có đúng sau ngày 7 tháng 7 năm 1928 hay không. Ngày bằng nhau phải bị loại bỏ vì sáng chế phải mới hơn bánh mì cắt lát. 
4. Đối với mỗi phát minh hợp lệ, hãy so sánh điểm của nó với điểm cao nhất hiện tại. Thay thế câu trả lời được lưu trữ nếu phát minh này có số điểm lớn hơn. 
5. Sau khi tất cả các bản ghi được xử lý, hãy in tiêu đề đã lưu. 

Tại sao nó hoạt động: trong quá trình quét, phát minh được lưu trữ luôn là phát minh hợp lệ có điểm cao nhất trong số tất cả các hồ sơ được xử lý cho đến nay. Khi một phát minh hợp lệ khác được kiểm tra, nó có điểm nhỏ hơn và tính bất biến vẫn đúng hoặc nó có điểm lớn hơn và việc thay thế câu trả lời đã lưu sẽ khôi phục tính bất biến. Sau bản ghi cuối cùng, câu trả lời được lưu trữ phải là phát minh có giá trị lớn nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

months = {
    "Jan": 1,
    "Feb": 2,
    "Mar": 3,
    "Apr": 4,
    "May": 5,
    "Jun": 6,
    "Jul": 7,
    "Aug": 8,
    "Sep": 9,
    "Oct": 10,
    "Nov": 11,
    "Dec": 12,
}

def parse_date(s):
    month, day, year = s.replace(",", "").split()
    return (int(year), months[month], int(day))

def solve(data=None):
    global input
    if data is not None:
        import io
        input = io.StringIO(data).readline

    n = int(input())

    cutoff = (1928, 7, 7)
    best_score = -1.0
    answer = ""

    for _ in range(n):
        title = input().rstrip("\n")
        date = parse_date(input().rstrip("\n"))
        score = float(input().rstrip("\n"))

        if date > cutoff and score > best_score:
            best_score = score
            answer = title

    return answer

if __name__ == "__main__":
    print(solve())
```Từ điển tháng chuyển đổi tháng văn bản thành số để có thể so sánh trực tiếp ngày tháng. Trình phân tích cú pháp sẽ xóa dấu phẩy khỏi các đầu vào như`Jul 7, 1928`trước khi chia từng mảnh. 

Quá trình quét chỉ giữ lại hai phần trạng thái: điểm tốt nhất được tìm thấy cho đến nay và tiêu đề tương ứng. Việc so sánh sử dụng`>`để kiểm tra ngày tháng vì các phát minh về ngày bánh mì cắt lát không được bao gồm. 

Việc so sánh điểm sử dụng các giá trị dấu phẩy động vì điểm đầu vào là phân số thập phân. Vì bài toán đảm bảo rằng không có mối ràng buộc nào cho phát minh có giá trị lớn nhất nên việc xử lý mối ràng buộc chính xác là không cần thiết. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
5
FM Radio
Dec 26, 1933
0.65
Ballpoint Pen
Oct 30, 1888
0.7442
Penicillin
Sep 28, 1928
0.0036
The Hovercraft
Dec 12, 1955
0.896
Fireworks
Apr 20, 960
0.99999
```quá trình quét hoạt động như sau: 

| Phát minh | Ngày hợp lệ? | Điểm | Tốt nhất hiện nay | 
| --- | --- | --- | --- | 
| Đài FM | Có | 0,65 | Đài FM | 
| Bút bi | Không | 0,7442 | Đài FM | 
| Penicillin | Có | 0,0036 | Đài FM | 
| Thủy phi cơ | Có | 0,896 | Thủy phi cơ | 
| Pháo hoa | Không | 0,99999 | Thủy phi cơ | 

Dấu vết cho thấy tại sao những phát minh cũ với điểm số xuất sắc lại không thể chiến thắng. Điểm số chỉ quan trọng sau khi điều kiện ngày được thỏa mãn. 

Một ví dụ thứ hai:```
3
A
Jul 7, 1928
1.0
B
Jul 8, 1928
0.4
C
Aug 1, 1928
0.7
```| Phát minh | Ngày hợp lệ? | Điểm | Tốt nhất hiện nay | 
| --- | --- | --- | --- | 
| A | Không | 1.0 | Không có | 
| B | Có | 0,4 | B | 
| C | Có | 0,7 | C | 

Điều này xác nhận ranh giới ngày nghiêm ngặt. Sáng chế có số điểm cao nhất có thể sẽ bị bỏ qua nếu nó không thực sự là bánh mì cắt lát. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi phát minh được phân tích cú pháp và kiểm tra một lần. | 
| Không gian | O(1) | Chỉ có dữ liệu phân tích tạm thời và phát minh tốt nhất hiện tại mới được lưu trữ. | 

Kích thước đầu vào tối đa chỉ là 10.000 phát minh, do đó việc quét tuyến tính dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    return solve(inp)

assert run("""5
FM Radio
Dec 26, 1933
0.65
Ballpoint Pen
Oct 30, 1888
0.7442
Penicillin
Sep 28, 1928
0.0036
The Hovercraft
Dec 12, 1955
0.896
Fireworks
Apr 20, 960
0.99999
""") == "The Hovercraft", "sample 1"

assert run("""3
A
Jul 7, 1928
1.0
B
Jul 8, 1928
0.4
C
Aug 1, 1928
0.7
""") == "C", "boundary date"

assert run("""1
First
Jul 8, 1928
0.5
""") == "First", "minimum size"

assert run("""4
Low
Jan 1, 2000
0.1
Mid
Jan 2, 2000
0.5
High
Jan 3, 2000
0.9
Lower
Jan 4, 2000
0.2
""") == "High", "maximum score selection"

assert run("""3
Ancient
Jan 1, 100
0.99
Old
Jul 7, 1928
1.0
New
Jul 8, 1928
0.1
""") == "New", "ignore invalid scores"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đầu vào mẫu | Thủy phi cơ | Lọc cơ bản và tìm kiếm tối đa | 
| Ngày ranh giới chính xác | C | Nghiêm khắc sau ngày 7 tháng 7 năm 1928 | 
| Một phát minh | Đầu tiên | Kích thước đầu vào tối thiểu | 
| Một số phát minh hợp lệ | Cao | Logic cập nhật điểm tối đa | 
| Những phát minh cũ đạt điểm cao | Mới | Lọc ngày trước khi so sánh điểm | 

## Vỏ cạnh 

Đối với sáng chế đúng vào ngày khóa sổ:```
2
Exact
Jul 7, 1928
1.0
Later
Jul 8, 1928
0.5
```Thuật toán phân tích chính xác như`(1928, 7, 7)`. Vì giá trị này không lớn hơn bộ giới hạn nên nó bị bỏ qua. Sau này trở thành phát minh hợp lệ đầu tiên và được in. 

Đối với một phát minh cũ có điểm tuyệt đối:```
2
Ancient
Jan 1, 1000
1.0
Modern
Jan 1, 2000
0.2
```Ancient bị bỏ qua trước khi so sánh điểm số. Thuật toán không bao giờ cho phép một phát minh không hợp lệ trở thành câu trả lời tốt nhất hiện tại nên Modern đã được chọn chính xác.
