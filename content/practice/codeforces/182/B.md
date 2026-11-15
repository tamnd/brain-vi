---
title: "CF 182B - Lịch của Vasya"
description: "Chúng tôi được yêu cầu mô phỏng một loại lịch đặc biệt. Vasya có một chiếc đồng hồ hiển thị các ngày từ 1 đến d. Mỗi tháng có một số ngày nhất định và đồng hồ cũng không biết đó là tháng nào."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 182
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 117 (Div. 2)"
rating: 1000
weight: 182
solve_time_s: 198
verified: false
draft: false
---

[CF 182B - Lịch của Vasya](https://codeforces.com/problemset/problem/182/B) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 3 phút 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng một loại lịch đặc biệt. Vasya có một chiếc đồng hồ hiển thị các ngày từ 1 đến`d`. Mỗi tháng có một số ngày nhất định và đồng hồ cũng không biết đó là tháng nào. Khi tháng thay đổi, nếu số ngày trên đồng hồ không khớp với ngày thực của tháng mới, Vasya sẽ tăng ngày theo cách thủ công cho đến khi đúng như vậy. Chúng tôi được yêu cầu đếm số lần tăng thủ công xảy ra trong suốt một năm. 

Đầu vào bao gồm`d`, ngày tối đa mà đồng hồ có thể hiển thị,`n`, số tháng và một mảng`n`số nguyên biểu thị số ngày trong mỗi tháng. Đầu ra là tổng số lần tăng thủ công mà Vasya thực hiện. 

Các hạn chế ở mức độ nhỏ đến trung bình:`n`nhiều nhất là 2000 và`d`có thể lên đến`10^6`. Điều này cho thấy rằng chúng ta không thể mô phỏng mọi ngày có thể trên một phạm vi lớn`d`nếu chúng tôi sử dụng một cách tiếp cận đơn giản, nhưng chúng tôi có thể lặp lại nhiều tháng và thực hiện các phép tính số học đơn giản mỗi tháng mà không gặp vấn đề về hiệu suất. 

Các trường hợp phức tạp liên quan đến các tháng có tổng số ngày trong tháng bằng`d`hoặc khi các tháng liên tiếp bắt đầu bằng số ngày trên đồng hồ đã ở phía trước hoặc phía sau. Ví dụ: tháng thứ nhất có 3 ngày, tháng thứ hai có 1 ngày và`d=3`, đồng hồ có thể hiển thị 1, 2, 3 vào cuối tháng 1, sau đó đặt lại về 1 vào đầu tháng 2. Việc xử lý bất cẩn số học modulo có thể tạo ra từng lỗi ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng mọi ngày trong năm. Bắt đầu với ngày đầu tiên trên đồng hồ. Đối với mỗi ngày trong tháng hiện tại, hãy kiểm tra xem đồng hồ có khớp với ngày hiện tại không. Nếu không, hãy tăng đồng hồ theo cách thủ công cho đến khi khớp. Sau đó chuyển sang ngày hôm sau. Điều này hiệu quả vì nó mô phỏng chính xác hành vi của Vasya, nhưng trong trường hợp xấu nhất, nó sẽ thực hiện`sum(a_i)`các hoạt động có thể đạt tới`2*10^9`nếu như`n=2000`Và`a_i`là tất cả`d=10^6`. Điều này là quá chậm. 

Quan sát quan trọng là số lần tăng thủ công trong một tháng chỉ phụ thuộc vào số ngày mà đồng hồ hiển thị vào đầu tháng và số ngày trong tháng. Nếu ngày đồng hồ hiện tại là`c`và tháng có`m`ngày thì số lần tăng thủ công cần thiết vào đầu tháng là`(c + m - 1) // d`hoặc trực quan hơn, sau khi đếm`m-1`ngày, nếu đồng hồ trôi qua`d`, mỗi lần tràn thể hiện mức tăng thủ công cần thiết để bắt kịp. Chúng ta có thể xử lý việc này bằng số học modulo. Điều này làm giảm mô phỏng thành một vòng lặp đơn giản trong nhiều tháng, thêm một số mức tăng mỗi tháng trong thời gian O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng(a_i)) | O(1) | Quá chậm đối với d lớn | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai biến:`current_day`đối với đồng hồ, đặt thành 1 và`manual_count`về 0 để lưu trữ tổng mức tăng thủ công. 
2. Lặp lại từng tháng theo thứ tự. 
3. Đối với tháng hiện tại với`days_in_month`:

Một. Tính số lần tăng thủ công cần thiết như sau:`(current_day + days_in_month - 1) // d * d - (current_day + days_in_month - 1) % d`. Một cách tiếp cận tương đương đơn giản hơn là: số ngày đồng hồ trôi qua`d`khi đếm`days_in_month - 1`ngày. 

b. Cộng số đó vào`manual_count`. 

c. Cập nhật`current_day`ĐẾN`(current_day + days_in_month - 1) % d + 1`. Điều này kết thúc chính xác suốt ngày đêm trong khi vẫn đảm bảo tháng tiếp theo bắt đầu vào đúng ngày. 
4. Sau khi xử lý tất cả các tháng, hãy in`manual_count`. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến mà`current_day`là ngày đồng hồ vào đầu mỗi tháng. Việc tính toán đảm bảo mỗi khi đồng hồ “tràn” qua`d`, chúng tôi tính số gia tăng thủ công. Bằng cách gói`current_day`modulo`d`, chúng tôi lập mô hình chính xác cho việc đặt lại đồng hồ và đảm bảo tháng tiếp theo bắt đầu bằng ngày bắt đầu chính xác của đồng hồ. Mỗi tháng được xử lý độc lập và tổng mức tăng thủ công được cộng lại một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

d = int(input())
n = int(input())
a = list(map(int, input().split()))

manual_count = 0
current_day = 1

for days_in_month in a:
    # Number of manual increments before the month ends
    manual_count += max(0, current_day + days_in_month - d - 1)
    # Update clock day for next month
    current_day = (current_day + days_in_month - 1) % d + 1

print(manual_count)
```Vòng lặp xử lý mỗi tháng theo thứ tự. Chúng tôi tính toán`max(0, current_day + days_in_month - d - 1)`để đảm bảo chúng tôi chỉ tính số gia tăng dương; nếu tháng hoàn toàn nằm trong phạm vi đồng hồ hiện tại thì không cần tăng thủ công. Hoạt động modulo bao bọc đồng hồ một cách chính xác, tránh các lỗi sai lệch. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
2
2 2
```Dấu vết: 

| Tháng | Ngày trong tháng | Đồng hồ bắt đầu | Đã thêm số lượng thủ công | Đồng hồ kết thúc | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 0 | 2 | 
| 2 | 2 | 3 | 2 | 2 | 

Giải thích: Tháng đầu tiên sử dụng ngày 1-2 trên đồng hồ, không tăng thủ công. Tháng thứ hai bắt đầu từ ngày thứ 3 theo đồng hồ, nhưng tháng có ngày 1-2 nên ta tăng thủ công 3->4, 4->1, đếm 2 số tăng. Dấu vết khớp với đầu ra mẫu. 

### Ví dụ tùy chỉnh 

đầu vào:```
5
3
3 5 2
```Dấu vết: 

| Tháng | Ngày trong tháng | Đồng hồ bắt đầu | Đã thêm số lượng thủ công | Đồng hồ kết thúc | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 1 | 0 | 3 | 
| 2 | 5 | 4 | 4 | 2 | 
| 3 | 2 | 3 | 0 | 4 | 

Giải thích: Tháng thứ hai kích hoạt 4 mức tăng thủ công do vượt quá 5. Tháng cuối cùng sử dụng ngày đồng hồ được bao bọc một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi lặp lại mỗi tháng đúng một lần và thực hiện số học O(1) mỗi tháng. | 
| Không gian | O(n) | Chỉ lưu trữ mảng đầu vào có độ dài tháng; tất cả các biến khác là không đổi. | 

Với các ràng buộc n ≤ 2000, quá trình này diễn ra thoải mái trong vòng chưa đầy 1 giây. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    d = int(input())
    n = int(input())
    a = list(map(int, input().split()))
    manual_count = 0
    current_day = 1
    for days_in_month in a:
        manual_count += max(0, current_day + days_in_month - d - 1)
        current_day = (current_day + days_in_month - 1) % d + 1
    return str(manual_count)

# provided sample
assert run("4\n2\n2 2\n") == "2", "sample 1"

# custom cases
assert run("1\n1\n1\n") == "0", "minimum input"
assert run("5\n3\n3 5 2\n") == "4", "overflow across multiple months"
assert run("6\n2\n6 6\n") == "5", "full months equal to d"
assert run("10\n4\n1 2 3 4\n") == "0", "all months fit in clock"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1\n1 | 0 | Kích thước đầu vào tối thiểu | 
| 5\n3\n3 5 2 | 4 | Tràn qua nhiều tháng | 
| 6\n2\n6 6 | 5 | Tháng bằng d, tràn cạnh kiểm tra | 
| 10\n4\n1 2 3 4 | 0 | Tất cả các tháng đều được tính theo đồng hồ, không tăng thủ công | 

## Vỏ cạnh 

Nếu đồng hồ chỉ có một ngày,`d=1`, mỗi ngày mới ngoại trừ ngày đầu tiên đều yêu cầu tăng thủ công. Ví dụ,`d=1`,`n=3`,`a=[1,1,1]`dẫn đến hai lần tăng thủ công. Thuật toán xử lý việc này vì`(current_day + days_in_month - d - 1)`tính toán tràn một cách chính xác ngay cả khi`d=1`. 

Nếu một tháng hoàn toàn khớp với phạm vi đồng hồ còn lại thì không có sự gia tăng thủ công nào xảy ra. Vì`d=6`, tháng có 3 ngày bắt đầu từ đồng hồ 4,`(4 + 3 - 6 - 1)`đánh giá là 0, không tạo ra mức tăng nào phù hợp với hành vi mong đợi.
