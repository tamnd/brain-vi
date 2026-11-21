---
title: "CF 237A - Tiền mặt miễn phí"
description: "Chúng ta được yêu cầu xác định xem Valera cần bao nhiêu máy tính tiền trong quán cà phê thức ăn nhanh của mình để mỗi khách có thể được phục vụ ngay lập tức, giả sử mỗi khách đến vào một thời điểm cụ thể trong một ngày và mỗi lần phục vụ mất ít hơn một phút."
date: "2026-06-04T16:42:18+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 237
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 147 (Div. 2)"
rating: 1000
weight: 237
solve_time_s: 207
verified: true
draft: false
---

[CF 237A - Tiền mặt miễn phí](https://codeforces.com/problemset/problem/237/A) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 3m 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem Valera cần bao nhiêu máy tính tiền trong quán cà phê thức ăn nhanh của mình để mỗi khách có thể được phục vụ ngay lập tức, giả sử mỗi khách đến vào một thời điểm cụ thể trong một ngày và mỗi lần phục vụ mất ít hơn một phút. Mỗi dòng đầu vào sau dòng đầu tiên sẽ cho biết giờ và phút khách hàng đến. Đầu ra là một số nguyên duy nhất: số lượng người tối đa đến vào cùng một thời điểm, vì đó là số lượng tiền mặt cần thiết để tránh bất kỳ khách hàng nào rời đi. 

Những ràng buộc cho phép`n`lên tới 100.000 và thời gian được cung cấp theo thứ tự thời gian. Điều này ngụ ý rằng bất kỳ giải pháp nào lặp lại trên tất cả khách hàng một lần hoặc sử dụng bản đồ từ lần này đến lần khác đều có thể chấp nhận được. Một giải pháp đơn giản so sánh từng cặp khách hàng trong những khoảng thời gian trùng nhau sẽ yêu cầu khoảng`n^2`các hoạt động không thể thực hiện được ở giới hạn trên của`n`. 

Các trường hợp chính cần cẩn thận là nhiều khách hàng đến cùng một lúc, khách hàng đến vào những thời điểm tuần tự mà không bị trùng lặp và thông tin đầu vào nhỏ nhất có thể có của một khách hàng. Ví dụ: nếu ba khách hàng đến lúc 8:00 và thêm hai khách hàng nữa lúc 9:00 thì kết quả đầu ra đúng là 3, mặc dù tổng cộng chỉ có năm khách hàng. Việc triển khai bất cẩn có thể tính tổng số lượt đến thay vì lấy số lượt đến đồng thời tối đa. 

## Phương pháp tiếp cận 

Phương pháp brute-force là so sánh mọi khách hàng với mọi khách hàng khác và đếm xem có bao nhiêu khách hàng có cùng dấu thời gian. Điều này sẽ đúng vì vấn đề giảm xuống còn việc đếm số lượt đến đồng thời, nhưng nó sẽ thực hiện gần như`n^2`so sánh trong trường hợp xấu nhất, quá chậm để`n = 10^5`. 

Cách tiếp cận tối ưu tận dụng thực tế là dữ liệu đầu vào đã được sắp xếp theo thứ tự thời gian. Chúng ta có thể lặp lại một lần, theo dõi số lượng khách hàng liên tiếp có chung giờ và phút. Bất cứ khi nào chúng tôi gặp thời gian mới, chúng tôi đặt lại bộ đếm. Giá trị lớn nhất mà bộ đếm đạt được trong lần lặp này là số lượng tiền mặt tối thiểu cần có. Phương pháp này vừa đơn giản vừa tuyến tính về mặt thời gian,`O(n)`, bởi vì chúng tôi xử lý mỗi khách hàng chính xác một lần và chỉ thực hiện các thao tác trong thời gian không đổi cho mỗi khách hàng. 

Thông tin chi tiết quan trọng là vì thời gian được sắp xếp nên tất cả khách hàng đến cùng một lúc đều liền kề nhau. Điều này có nghĩa là chúng tôi không cần bản đồ băm hoặc bất kỳ cấu trúc dữ liệu phức tạp nào; một bộ đếm đơn giản là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm`current_count`đến 1, đại diện cho khách hàng đầu tiên và`max_count`đến 1, sẽ lưu trữ số lượng đến đồng thời tối đa. 
2. Lưu trữ thời gian đến của khách hàng đầu tiên vào`last_time`như một bộ dữ liệu`(hour, minute)`. 
3. Lặp lại phần còn lại`n-1`khách hàng: 

một. Đối với mỗi khách hàng, hãy đọc thời gian đến dưới dạng một bộ dữ liệu`(hour, minute)`. 

b. Nếu thời gian này phù hợp`last_time`, tăng`current_count`bởi vì một khách hàng khác đã đến vào cùng phút đó. 

c. Nếu không khớp thì đặt`current_count`lên 1 vì chúng ta có thời gian mới. 

d. Cập nhật`last_time`đến thời điểm hiện tại. 

đ. Cập nhật`max_count`nếu như`current_count`vượt quá nó. 
4. Sau khi lặp lại tất cả khách hàng,`max_count`là số lượng tiền mặt tối thiểu cần thiết. In nó. 

Điều bất biến ở đây là`current_count`luôn phản ánh số lượng khách hàng đến vào thời điểm hiện tại và`max_count`ghi lại giá trị lớn nhất được thấy cho đến nay. Vì thời gian được sắp xếp nên không có khách hàng nào có cùng dấu thời gian xuất hiện sau trong dữ liệu nhập mà không được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
h, m = map(int, input().split())
last_time = (h, m)
current_count = 1
max_count = 1

for _ in range(n - 1):
    h, m = map(int, input().split())
    current_time = (h, m)
    if current_time == last_time:
        current_count += 1
    else:
        current_count = 1
        last_time = current_time
    if current_count > max_count:
        max_count = current_count

print(max_count)
```Giải pháp đọc khách hàng đầu tiên bên ngoài vòng lặp để khởi tạo`last_time`Và`current_count`. Đối với mỗi khách hàng tiếp theo, nó so sánh thời gian và cập nhật bộ đếm. Điều này đảm bảo không có sai sót nào khi đếm lần xuất hiện đầu tiên và tránh những lần kiểm tra không cần thiết. Sử dụng bộ dữ liệu`(hour, minute)`giữ cho sự so sánh đơn giản và rõ ràng. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
4
8 0
8 10
8 10
8 45
```| Bước | lần cuối cùng | hiện tại_thời gian | hiện tại_count | max_count | 
| --- | --- | --- | --- | --- | 
| ban đầu | (8,0) | (8,0) | 1 | 1 | 
| 1 | (8,0) | (8,10) | 1 | 1 | 
| 2 | (8,10) | (8,10) | 2 | 2 | 
| 3 | (8,10) | (8,45) | 1 | 2 | 

Bảng cho thấy số lần đến đồng thời tối đa là 2, vì vậy chúng tôi cần 2 tiền mặt. 

**Đầu vào mẫu 2**```
3
9 0
9 15
9 30
```| Bước | lần cuối cùng | hiện tại_thời gian | hiện tại_count | max_count | 
| --- | --- | --- | --- | --- | 
| ban đầu | (9,0) | (9,0) | 1 | 1 | 
| 1 | (9,0) | (9,15) | 1 | 1 | 
| 2 | (9,15) | (9,30) | 1 | 1 | 

Mỗi khách hàng đến vào một thời điểm khác nhau nên chỉ cần một khoản tiền mặt là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi lặp lại mỗi khách hàng chính xác một lần và thực hiện các hoạt động liên tục theo thời gian cho mỗi khách hàng. | 
| Không gian | O(1) | Chúng tôi chỉ lưu trữ một vài bộ đếm và bộ dữ liệu; không cần thêm mảng hoặc bản đồ. | 

Được cho`n ≤ 100,000`và lặp tuyến tính, giải pháp thực thi tốt trong giới hạn thời gian 2 giây, với mức sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    h, m = map(int, input().split())
    last_time = (h, m)
    current_count = 1
    max_count = 1
    for _ in range(n - 1):
        h, m = map(int, input().split())
        current_time = (h, m)
        if current_time == last_time:
            current_count += 1
        else:
            current_count = 1
            last_time = current_time
        if current_count > max_count:
            max_count = current_count
    return str(max_count)

# provided samples
assert run("4\n8 0\n8 10\n8 10\n8 45\n") == "2", "sample 1"
assert run("3\n9 0\n9 15\n9 30\n") == "1", "sample 2"

# custom cases
assert run("1\n0 0\n") == "1", "single customer"
assert run("5\n10 0\n10 0\n10 0\n10 0\n10 0\n") == "5", "all same time"
assert run("6\n1 1\n1 2\n1 2\n1 2\n1 3\n1 4\n") == "3", "three at same minute"
assert run("4\n23 59\n23 59\n0 0\n0 0\n") == "2", "boundary wrap-around"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 khách hàng | 1 | Kích thước đầu vào tối thiểu | 
| tất cả cùng một lúc | 5 | Số lượt đến đồng thời tối đa | 
| trùng lặp hỗn hợp | 3 | Nhiều nhóm có bản sao | 
| bọc ranh giới | 2 | Thời gian cuối và đầu ngày đảm bảo không tính chéo ngày | 

## Vỏ cạnh 

Đối với một khách hàng, hãy nhập`1\n12 30\n`,`current_count`Và`max_count`được khởi tạo là 1 và vòng lặp không chạy, tạo ra 1, điều này đúng. Đối với tất cả khách hàng đến cùng một lúc, bộ đếm sẽ tăng chính xác qua vòng lặp, mang lại giá trị tối đa chính xác. Khi khách đến vào cuối ngày và nửa đêm hôm sau, việc so sánh`(hour, minute)`đảm bảo không có sự tổng hợp sai trong nhiều ngày. Việc xử lý cụ thể này sẽ ngăn ngừa bất kỳ sai sót nào xảy ra ngoài ranh giới hoặc sai sót xuyên biên giới.
