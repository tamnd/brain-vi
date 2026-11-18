---
title: "CF 205A - Chú Voi Nhỏ Và Rozdil"
description: "Chúng ta được cung cấp một danh sách thời gian di chuyển từ thị trấn Rozdil tới n thị trấn khác. Mỗi thị trấn có thời gian nguyên dương và các thị trấn được đánh số từ 1 đến n. Mục tiêu là tìm ra thị trấn nào có thời gian di chuyển nhỏ nhất."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 205
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 129 (Div. 2)"
rating: 900
weight: 205
solve_time_s: 54
verified: true
draft: false
---

[CF 205A - Chú voi nhỏ và Rozdil](https://codeforces.com/problemset/problem/205/A) 

**Xếp hạng:** 900 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách thời gian di chuyển từ thị trấn Rozdil đến từng thị trấn trong số _n_ thị trấn khác. Mỗi thị trấn có thời gian nguyên dương và các thị trấn được đánh số từ 1 đến _n_. Mục tiêu là tìm ra thị trấn nào có thời gian di chuyển nhỏ nhất. Nếu có một thị trấn duy nhất có thời gian tối thiểu này, chúng tôi sẽ in số của nó. Nếu có nhiều thị trấn bị ràng buộc ở mức tối thiểu, Chú voi nhỏ sẽ ở lại Rozdil và chúng tôi in "Vẫn Rozdil". 

Các ràng buộc cho phép lên tới 100.000 thị trấn và thời gian di chuyển có thể lên tới 10^9. Điều này cho chúng ta biết rằng bất kỳ giải pháp nào cũng phải hoạt động theo thời gian tuyến tính; việc sắp xếp mảng thời gian sẽ là O(n log n) và có thể hoạt động, nhưng quét tuyến tính đơn giản và đầy đủ hơn. Các hoạt động với độ phức tạp bậc hai sẽ quá chậm. 

Một trường hợp phức tạp phát sinh khi nhiều thị trấn có cùng thời gian di chuyển tối thiểu. Ví dụ: nếu đầu vào là`5\n4 2 2 5 6`, thời gian tối thiểu là 2, nhưng nó xảy ra ở hai thị trấn (2 và 3). Việc triển khai bất cẩn có thể chỉ trả về mức tối thiểu đầu tiên mà nó tìm thấy, điều này sẽ sai. Một trường hợp cạnh khác xảy ra khi chỉ có một thị trấn, trong trường hợp đó câu trả lời phải là thị trấn đó, vì tính duy nhất được đảm bảo. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực lặp đi lặp lại qua mọi thị trấn, theo dõi thời gian nhỏ nhất và chỉ số thị trấn tương ứng. Điều này hiệu quả vì nó trực tiếp thực hiện các yêu cầu của bài toán: nó xác định mức tối thiểu và kiểm tra các mối quan hệ. Về mặt phép toán, mỗi phép so sánh là thời gian không đổi nên tổng thời gian là O(n). Mức sử dụng bộ nhớ là O(1) ngoài đầu vào. Cách tiếp cận này đã tối ưu, nhưng chúng ta phải cẩn thận với việc phát hiện ràng buộc: chỉ lưu trữ một chỉ mục tối thiểu là không đủ nếu chúng ta cần kiểm tra tính duy nhất. 

Thông tin chi tiết quan trọng là chúng ta có thể duy trì ba phần thông tin trong khi quét mảng: thời gian di chuyển tối thiểu hiện tại, chỉ số của thị trấn đạt được mục tiêu đó lần đầu tiên và bộ đếm số lần mức tối thiểu này đã xảy ra. Mỗi khi chúng tôi gặp thời gian nhỏ hơn, chúng tôi cập nhật mức tối thiểu, đặt chỉ mục cho thị trấn hiện tại và đặt lại bộ đếm về 1. Nếu chúng tôi gặp mức tối thiểu bằng nhau, chúng tôi sẽ tăng bộ đếm. Cuối cùng, nếu bộ đếm lớn hơn 1, chúng ta in "Still Rozdil"; nếu không, chúng tôi in chỉ mục được lưu trữ. Cách tiếp cận này là tuyến tính, sử dụng không gian bổ sung không đổi và xử lý chính xác tất cả các trường hợp cạnh bao gồm các bản sao và mảng phần tử đơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với khả năng phát hiện ràng buộc | O(n) | O(1) | Đã chấp nhận | 
| Sắp xếp và quét | O(n log n) | O(n) | Được chấp nhận nhưng không cần thiết | 

## Hướng dẫn thuật toán 

1. Khởi tạo ba biến:`min_time`với số lượng rất lớn,`min_index`đến -1 và`count`đến 0.`min_time`sẽ lưu trữ thời gian di chuyển nhỏ nhất được tìm thấy cho đến nay.`min_index`sẽ lưu trữ chỉ số dựa trên 1 của thị trấn lần đầu tiên đạt được mức tối thiểu này.`count`sẽ lưu trữ bao nhiêu lần mức tối thiểu này đã xảy ra. 
2. Lặp lại thời gian di chuyển bằng vòng lặp có chỉ mục`i`từ 0 đến n-1. Đối với mỗi thị trấn, hãy đọc thời gian di chuyển. 
3. Nếu thời gian di chuyển hiện tại ít hơn`min_time`, đây là mức tối thiểu mới. Bộ`min_time`đến giá trị này,`min_index`ĐẾN`i + 1`(để chuyển đổi từ lập chỉ mục dựa trên 0 sang lập chỉ mục dựa trên 1) và đặt lại`count`đến 1. 
4. Nếu thời gian di chuyển hiện tại bằng`min_time`, tăng`count`lên 1, vì chúng tôi đã tìm thấy một thị trấn khác có cùng mức tối thiểu. 
5. Sau khi quét tất cả các thị trấn, hãy kiểm tra giá trị của`count`. Nếu như`count`là 1, in`min_index`. Nếu như`count`lớn hơn 1, in ra "Still Rozdil". 

Tại sao nó hoạt động: Thuật toán luôn theo dõi thời gian di chuyển nhỏ nhất và đếm xem có bao nhiêu thị trấn đạt được thời gian đó. Bằng cách chỉ cập nhật khi tìm thấy thời gian nhỏ hơn, chúng tôi đảm bảo`min_index`luôn luôn trỏ đến thị trấn đầu tiên với mức tối thiểu. Việc đếm các bản sao đảm bảo chúng tôi chỉ khai báo mức tối thiểu duy nhất nếu nó thực sự tồn tại. Không có thị trấn nào bị bỏ sót vì chúng tôi lặp lại tất cả chúng một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
times = list(map(int, input().split()))

min_time = 10**9 + 1
min_index = -1
count = 0

for i in range(n):
    t = times[i]
    if t < min_time:
        min_time = t
        min_index = i + 1
        count = 1
    elif t == min_time:
        count += 1

if count == 1:
    print(min_index)
else:
    print("Still Rozdil")
```Phần đầu tiên đọc đầu vào bằng I/O nhanh.`min_time`được khởi tạo ở giá trị lớn hơn bất kỳ thời gian di chuyển nào có thể để đảm bảo thị trấn đầu tiên sẽ cập nhật chính xác. Trong vòng lặp, chúng tôi duy trì chỉ số và số lượng tối thiểu hiện tại. Chúng tôi thêm 1 vào chỉ mục vì vấn đề mong đợi việc lập chỉ mục dựa trên 1. Cuối cùng, chúng tôi kiểm tra số lượng để xử lý các mối quan hệ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`2\n7 4`| tôi | t | phút_thời gian | phút_index | đếm | 
| --- | --- | --- | --- | --- | 
| 0 | 7 | 7 | 1 | 1 | 
| 1 | 4 | 4 | 2 | 1 | 

Mức tối thiểu là duy nhất tại thị trấn 2, vì vậy đầu ra là`2`. Bảng cho thấy thuật toán cập nhật chính xác khi tìm thấy mức tối thiểu mới. 

### Mẫu 2 

đầu vào:`5\n4 2 2 5 6`| tôi | t | phút_thời gian | phút_index | đếm | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | 4 | 1 | 1 | 
| 1 | 2 | 2 | 2 | 1 | 
| 2 | 2 | 2 | 2 | 2 | 
| 3 | 5 | 2 | 2 | 2 | 
| 4 | 6 | 2 | 2 | 2 | 

Mức tối thiểu xảy ra ở nhiều thị trấn, do đó sản lượng là`Still Rozdil`. Điều này thể hiện việc xử lý chính xác các bản sao. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi quét danh sách thời gian di chuyển chính xác một lần, thực hiện các thao tác với thời gian không đổi cho mỗi phần tử. | 
| Không gian | O(n) | Chúng tôi lưu trữ danh sách thời gian di chuyển. Các biến bổ sung sử dụng không gian O(1). | 

Giải pháp dễ dàng nằm gọn trong giới hạn 2 giây cho n lên tới 100.000 và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    times = list(map(int, input().split()))
    min_time = 10**9 + 1
    min_index = -1
    count = 0
    for i in range(n):
        t = times[i]
        if t < min_time:
            min_time = t
            min_index = i + 1
            count = 1
        elif t == min_time:
            count += 1
    return str(min_index) if count == 1 else "Still Rozdil"

# Provided samples
assert run("2\n7 4\n") == "2", "sample 1"
assert run("5\n7 4 2 2 5\n") == "Still Rozdil", "sample 2"

# Custom cases
assert run("1\n10\n") == "1", "single town"
assert run("3\n5 5 5\n") == "Still Rozdil", "all equal values"
assert run("4\n3 2 4 2\n") == "Still Rozdil", "multiple minimums"
assert run("4\n7 1 3 4\n") == "2", "unique minimum in the middle"
assert run("5\n10 9 8 7 6\n") == "5", "minimum at the end"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n10 | 1 | Trường hợp rìa thị trấn đơn lẻ | 
| 3\n5 5 5 | Vẫn là Rozdil | Tất cả các thị trấn đều có thời gian di chuyển như nhau | 
| 4\n3 2 4 2 | Vẫn là Rozdil | Nhiều giá trị tối thiểu | 
| 4\n7 1 3 4 | 2 | Mức tối thiểu duy nhất ở giữa danh sách | 
| 5\n10 9 8 7 6 | 5 | Tối thiểu tại thị trấn cuối cùng | 

## Vỏ cạnh 

Thuật toán xử lý đầu vào của một thị trấn bằng cách khởi tạo`min_time`cập nhật cao và chính xác tại thị trấn đầu tiên. Mức tối thiểu trùng lặp được xử lý bằng cách tăng dần`count`mỗi khi mức tối thiểu xảy ra, đảm bảo rằng các mối quan hệ sẽ kích hoạt "Still Rozdil". Thuật toán cũng xác định chính xác mức tối thiểu xuất hiện ở chỉ mục cuối cùng, duy trì chỉ mục phù hợp và bất biến được tìm thấy đầu tiên.
