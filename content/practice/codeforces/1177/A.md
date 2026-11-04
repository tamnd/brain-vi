---
title: "CF 1177A - Chuỗi chữ số (Phiên bản dễ dàng)"
description: "Chúng ta có một số nguyên dương $k$ và chúng ta tưởng tượng việc viết tất cả các số nguyên dương theo thứ tự, bắt đầu từ 1, ngay cạnh nhau mà không có dấu cách hoặc dấu phân cách. Điều này tạo ra một luồng chữ số liên tục như 12345678910111213..., v.v."
date: "2026-06-13T10:23:58+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1177
codeforces_index: "A"
codeforces_contest_name: "Testing Round 15 (Unrated)"
rating: 1000
weight: 1177
solve_time_s: 303
verified: true
draft: false
---

[CF 1177A - Chuỗi chữ số (Phiên bản dễ dàng)](https://codeforces.com/problemset/problem/1177/A) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 5 phút 3 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương duy nhất$k$, và chúng ta tưởng tượng viết tất cả các số nguyên dương theo thứ tự, bắt đầu từ 1, ngay cạnh nhau mà không có dấu cách hoặc dấu phân cách. Điều này tạo ra một luồng chữ số liên tục như 12345678910111213..., v.v. Nhiệm vụ là xác định chữ số nào xuất hiện ở vị trí$k$trong luồng vô hạn này khi đếm từ bên trái bắt đầu từ 1. 

Thách thức chính là trình tự phát triển rất nhanh về độ dài nhưng theo cách có cấu trúc. Các số có một chữ số đóng góp 9 chữ số, các số có hai chữ số đóng góp 90 số nhân mỗi số 2 chữ số, các số có ba chữ số đóng góp 900 số nhân 3 chữ số, v.v. Cấu trúc này có nghĩa là chúng ta thực sự không bao giờ cần phải xây dựng chuỗi đầy đủ. 

Ràng buộc$k \le 10000$đủ nhỏ để ngay cả một mô phỏng đơn giản lên tới nhiều chữ số đó cũng có thể vượt qua, nhưng lý do dự định sẽ mở rộng đến các đầu vào lớn hơn nhiều và tránh hoàn toàn việc xây dựng các chuỗi. Bất kỳ cách tiếp cận nào xây dựng chuỗi nối một cách rõ ràng đều có nguy cơ gây ra chi phí không cần thiết và gây nhầm lẫn xung quanh việc lập chỉ mục. 

Trường hợp cạnh tinh vi phát sinh xung quanh ranh giới giữa các khối có độ dài chữ số. Ví dụ, khi$k = 9$, đáp án là 9, nhưng khi$k = 10$, chúng ta chuyển sang số 10 và phải trích đúng chữ số đầu tiên của số có nhiều chữ số. Trình tạo chuỗi gia tăng đơn giản có thể dễ dàng sắp xếp sai các chỉ mục ở những lần chuyển đổi này. 

## Phương pháp tiếp cận 

Một cách tiếp cận brute-force trực tiếp xây dựng chuỗi bằng cách liên tục chuyển đổi các số nguyên thành chuỗi và nối chúng cho đến khi vượt quá độ dài$k$. Khi chuỗi đầy đủ được tạo, chúng tôi chỉ cần lập chỉ mục cho chuỗi đó. Điều này có hiệu quả vì nối chuỗi Python và chuyển đổi số nguyên thành chuỗi là các thao tác chính xác cho vấn đề này. Tuy nhiên, chi phí đến từ việc phát triển chuỗi liên tục, dẫn đến tổng công việc tỷ lệ thuận với chiều dài được xây dựng cuối cùng. Trong khi$k \le 10000$giữ điều này có thể quản lý được ở đây, phương pháp sẽ trở nên kém hiệu quả nếu$k$lớn lên. 

Cách tiếp cận có nguyên tắc hơn sẽ tránh việc xây dựng chuỗi hoàn toàn. Thay vào đó, chúng ta suy luận theo khối số có cùng độ dài chữ số. Đầu tiên chúng tôi xác định liệu$k$-Chữ số thứ nằm trong khối số có 1 chữ số, số có 2 chữ số, số có 3 chữ số, v.v. Mỗi khối đóng góp một số chữ số đã biết, vì vậy chúng tôi trừ kích thước khối cho đến khi tìm được phạm vi chính xác. Khi chúng tôi biết khối chính xác, chúng tôi xác định số chính xác và sau đó trích xuất chữ số được yêu cầu bằng cách sử dụng số học thay vì nối chuỗi. 

Điều này hiệu quả vì trình tự được cấu trúc thành các phân đoạn thống nhất, liền kề trong đó số lượng chữ số có thể dự đoán được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(k)$|$O(k)$| Được chấp nhận cho ràng buộc này | 
| Tối ưu |$O(\log k)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp bằng cách dần dần thu hẹp phạm vi$k$-chữ số thứ nói dối. 

1. Bắt đầu với độ dài chữ số$d = 1$, đại diện cho các số từ 1 đến 9. Chúng tôi tính xem khối này đóng góp bao nhiêu chữ số$9 \cdot d$. Nếu như$k$lớn hơn khối này, chúng ta trừ nó đi và chuyển sang khối tiếp theo. 
2. Tăng$d$đến 2, sau đó xét các số từ 10 đến 99. Khối này góp phần$90 \cdot 2$chữ số. Nếu như$k$vẫn lớn hơn khối này, hãy trừ lại và tiếp tục. 
3. Lặp lại quá trình này để tăng độ dài chữ số. Ở mỗi bước, chúng tôi đang bỏ qua toàn bộ lớp thống nhất của hệ thống số một cách hiệu quả, điều này tránh việc kiểm tra các số riêng lẻ. 
4. Khi chúng ta tìm thấy khối đầu tiên ở đó$k$nằm trong tổng đóng góp chữ số của nó, chúng tôi xác định số chính xác bên trong khối đó. Chúng tôi tính toán có bao nhiêu số đầy đủ trong khối mà chúng tôi cần sử dụng phép chia số nguyên, sau đó xác định chữ số chính xác bên trong số đó bằng cách sử dụng modulo. 
5. Chuyển số đã xác định thành chuỗi và trích xuất ký tự chính xác. 

Lý do chính khiến tính năng này hoạt động là vì mỗi khối liền kề nhau và có chiều rộng chữ số không đổi, vì vậy chúng ta có thể ánh xạ các vị trí chữ số chung vào tọa độ cục bộ bên trong một phạm vi số nguyên cụ thể. 

### Tại sao nó hoạt động 

Ở mỗi bước, chúng tôi duy trì bất biến rằng$k$biểu thị phần bù thành hậu tố của chuỗi chữ số bắt đầu tại một số khối có độ dài chữ số. Mỗi phép trừ sẽ loại bỏ toàn bộ tiền tố được tính toán đầy đủ và chính xác. Vì các khối không chồng lên nhau và phân chia đầy đủ trình tự nên khi chúng ta dừng lại,$k$phải nằm trong một vùng có độ dài một chữ số. Trong vùng đó, mỗi số đóng góp chính xác$d$các chữ số, do đó ánh xạ số học từ chỉ số sang số là chính xác và không mất dữ liệu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

k = int(input().strip())

digit_len = 1
count = 9
start = 1

while k > digit_len * count:
    k -= digit_len * count
    digit_len += 1
    count *= 10
    start *= 10

num = start + (k - 1) // digit_len
idx = (k - 1) % digit_len

print(str(num)[idx])
```Việc thực hiện phản ánh lý luận từng khối. Các biến`digit_len`,`count`, Và`start`theo dõi phân đoạn số hiện tại mà chúng tôi đang phân tích.`digit_len * count`đưa ra tổng số chữ số trong khối hiện tại. Chúng tôi trừ toàn bộ khối cho đến khi tìm thấy phạm vi chính xác. 

Khi đã vào đúng khối,`(k - 1) // digit_len`đưa ra phần bù về số lượng đầy đủ và`(k - 1) % digit_len`đưa ra vị trí bên trong số đó. Trừ 1 trước khi chia và modulo là điều cần thiết để xử lý chính xác việc lập chỉ mục dựa trên 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1: k = 7 

| Bước | chữ số_len | đếm | khối_size | còn lại k | 
| --- | --- | --- | --- | --- | 
| ban đầu | 1 | 9 | 9 | 7 | 

Chúng ta không trừ bất cứ thứ gì vì 7 nằm ở khối đầu tiên của các số có 1 chữ số. Bản thân số đó là 7 và chữ số là 7. Điều này xác nhận rằng các vị trí đầu ánh xạ trực tiếp tới các số nguyên có một chữ số mà không cần chuyển đổi. 

### Ví dụ 2: k = 12 

| Bước | chữ số_len | đếm | khối_size | còn lại k | 
| --- | --- | --- | --- | --- | 
| 1 chữ số | 1 | 9 | 9 | 3 | 
| 2 chữ số | 2 | 90 | 180 | 3 | 

Sau khi loại bỏ 9 chữ số khỏi khối 1 chữ số, ta có$k = 3$bên trong khối 2 chữ số bắt đầu từ 10. Chữ số thứ ba trong dãy 101112... tương ứng với số 11 và trong đó chúng ta lấy chữ số đầu tiên là 1. 

Dấu vết này cho thấy cách thuật toán chuyển đổi rõ ràng qua các ranh giới có độ dài chữ số mà không cần xây dựng chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log k)$| Chúng tôi lặp lại theo độ dài chữ số và mỗi bước bỏ qua một khối số tăng theo cấp số nhân | 
| Không gian |$O(1)$| Chúng tôi chỉ lưu trữ một số lượng bộ đếm không đổi và tính toán các chữ số theo yêu cầu | 

Các ràng buộc cho phép lên đến$k = 10000$và thuật toán chạy trong thời gian không đổi so với phạm vi đó vì số lượng khối có độ dài chữ số nhiều nhất là 5. Con số này thấp hơn nhiều so với bất kỳ mối lo ngại nào về giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solution()

def solution():
    import sys
    input = sys.stdin.readline
    k = int(input().strip())

    digit_len = 1
    count = 9
    start = 1

    while k > digit_len * count:
        k -= digit_len * count
        digit_len += 1
        count *= 10
        start *= 10

    num = start + (k - 1) // digit_len
    idx = (k - 1) % digit_len
    return str(num)[idx]

# provided sample
assert run("7\n") == "7"

# boundary: end of first block
assert run("9\n") == "9"

# transition into two-digit numbers
assert run("10\n") == "1"

# within two-digit block
assert run("12\n") == "1"

# larger position
assert run("15\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 9 | 9 | ranh giới khối 1 chữ số | 
| 10 | 1 | chữ số đầu tiên của 10 | 
| 12 | 1 | lập chỉ mục chính xác bên trong khối 2 chữ số | 
| 15 | 2 | độ chính xác giữa khối | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra chính xác ở các chuyển tiếp khối, chẳng hạn như$k = 9$hoặc$k = 10$. Vì$k = 9$, thuật toán vẫn ở khối 1 chữ số và trả về trực tiếp 9. Vì$k = 10$, chúng ta trừ khối 1 chữ số đầy đủ, để lại$k = 1$trong khối 2 chữ số. Số được tính toán trở thành 10 và chỉ số 0 được trích xuất, tạo ra kết quả chính xác là 1. 

Một trường hợp tế nhị khác là khi$k$nằm ở chữ số cuối cùng của một số có nhiều chữ số, chẳng hạn như$k = 11$, tương ứng với chữ số thứ hai của 10, mang lại 0. Việc lập chỉ mục số học đảm bảo rằng tính toán modulo chọn chính xác ký tự cuối cùng của biểu diễn chuỗi mà không có bất kỳ lỗi nào.
