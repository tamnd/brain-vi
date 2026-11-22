---
title: "CF 37A - Tháp"
description: "Chúng ta được cho một bộ thanh gỗ, mỗi thanh có chiều dài nguyên dương. Vasya muốn xây tháp bằng cách xếp chồng các thanh có cùng chiều dài. Mỗi tháp phải bao gồm các thanh có chiều dài giống hệt nhau, nhưng các tháp khác nhau có thể có độ dài khác nhau."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "sortings"]
categories: ["algorithms"]
codeforces_contest: 37
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 37"
rating: 1000
weight: 37
solve_time_s: 53
verified: true
draft: false
---
[CF 37A - Tháp](https://codeforces.com/problemset/problem/37/A) 

**Đánh giá:** 1000 
**Thẻ:** sắp xếp 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một bộ thanh gỗ, mỗi thanh có chiều dài nguyên dương. Vasya muốn xây tháp bằng cách xếp chồng các thanh có cùng chiều dài. Mỗi tháp phải bao gồm các thanh có chiều dài giống hệt nhau, nhưng các tháp khác nhau có thể có độ dài khác nhau. Mục tiêu là xây dựng tất cả các thanh thành tháp đồng thời giảm thiểu số lượng tháp và báo cáo chiều cao của tháp cao nhất. 

Đầu vào bao gồm số lượng thanh$N$(1 ∼ N ₫ 1000) và danh sách độ dài của chúng. Đầu ra yêu cầu hai số nguyên: số lượng thanh tối đa được xếp chồng lên nhau trong một tháp và tổng số tháp. 

Những hạn chế là vừa phải.$N$có thể lên tới 1000, cho phép các thuật toán có độ phức tạp về thời gian lên tới$O(N^2)$thoải mái. Tuy nhiên, vì độ dài là các số nguyên giới hạn bởi 1000 nên chúng ta có thể khai thác điều này để tính toán số lượng một cách hiệu quả mà không cần lặp qua tất cả các cặp thanh. Điều này gợi ý về cách tiếp cận đếm tần số thay vì xây dựng vòng lặp lồng nhau. 

Các trường hợp cạnh không rõ ràng bao gồm: 

1. Tất cả các thanh đều có độ dài riêng. Ví dụ, đầu vào`5\n1 2 3 4 5`dẫn đến 5 tháp, mỗi tháp có chiều cao 1. Cách tiếp cận bất cẩn cố gắng "kết hợp" các thanh sẽ thất bại. 
2. Tất cả các thanh đều có cùng chiều dài. đầu vào`4\n7 7 7 7`sẽ mang lại một tòa tháp có chiều cao 4. 
3. Sự lặp lại hỗn hợp. đầu vào`6\n1 2 2 3 3 3`sẽ tạo ra các tòa tháp có chiều cao 1, 2 và 3, với chiều cao tháp cao nhất là 3. Việc sắp xếp ngây thơ mà không đếm có thể khiến việc tính số lượng tháp hiệu quả trở nên kém rõ ràng hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là mô phỏng các tòa tháp một cách rõ ràng. Chúng ta có thể lặp lại từng thanh và cố gắng đặt nó trên một tháp hiện có có cùng chiều dài hoặc bắt đầu một tháp mới. Vì$N = 1000$, điều này liên quan đến việc kiểm tra mọi tháp hiện có để tìm từng thanh, có khả năng dẫn đến$O(N^2)$hoạt động. Mặc dù điều này khả thi ở đây nhưng nó phức tạp không cần thiết và dễ xảy ra lỗi khi chiều cao và số đếm có thể được tính trực tiếp hơn. 

Thông tin chi tiết quan trọng là chiều cao của tòa tháp cao nhất được xác định bởi tần số tối đa của bất kỳ chiều dài thanh nào và tổng số tháp được xác định bởi số lượng chiều dài riêng biệt. Việc đếm tần số biến vấn đề thành một phép truyền dữ liệu đơn giản. Bằng cách lưu trữ số lượng thanh có mỗi độ dài tồn tại, tháp cao nhất chỉ đơn giản là số lượng tối đa và số lượng tháp là số lượng độ dài duy nhất. 

Nhận xét rằng "chúng ta chỉ cần số lượng chứ không cần cấu trúc tháp rõ ràng" chuyển vấn đề từ mô phỏng sang vấn đề đếm, giảm độ phức tạp từ$O(N^2)$ĐẾN$O(N)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^2) | O(N) | Hoạt động nhưng quá mức cần thiết | 
| Tối ưu | O(N) | O(L) trong đó L ≤ 1000 | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một từ điển (hoặc mảng có kích thước 1001) để đếm tần số của mỗi chiều dài thanh. Cấu trúc này ánh xạ từng độ dài tới số thanh có độ dài đó. 
2. Lặp lại tất cả các độ dài thanh. Đối với mỗi độ dài, hãy tăng số lượng của nó trong từ điển tần số. Bước này cung cấp sự phân bố đầy đủ về độ dài thanh. 
3. Xác định chiều cao của tòa tháp cao nhất bằng cách tìm giá trị lớn nhất trong từ điển tần số. Số lượng thanh giống hệt nhau lớn nhất tương ứng trực tiếp với tòa tháp cao nhất. 
4. Đếm tổng số tháp bằng cách tính số độ dài duy nhất có trong từ điển. Mỗi chiều dài duy nhất tạo thành chính xác một tòa tháp. 
5. In ra chiều cao của tháp cao nhất và tổng số tháp. 

Tại sao nó hoạt động: Tại mỗi điểm, thuật toán sẽ đếm đầy đủ chiều dài của từng thanh. Tháp cao nhất không thể vượt quá số lượng chiều dài thường xuyên nhất và mỗi chiều dài riêng biệt phải đóng góp ít nhất một tháp. Không có thông tin nào bị mất nên câu trả lời cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

N = int(input())
lengths = list(map(int, input().split()))

# count frequencies
freq = {}
for l in lengths:
    if l in freq:
        freq[l] += 1
    else:
        freq[l] = 1

max_height = max(freq.values())
num_towers = len(freq)

print(max_height, num_towers)
```Giải pháp đầu tiên đọc đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`. Từ điển tần số`freq`lưu trữ số lượng của từng độ dài và chúng tôi trực tiếp tính toán giá trị tối đa cho tòa tháp cao nhất. Số lượng khóa trong từ điển cho biết số lượng tháp riêng biệt. Điều này tránh mọi lỗi xảy ra hoặc các vòng lặp không cần thiết. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
3
1 2 3
```| Bước | tần số | chiều cao tối đa | số_tháp | 
| --- | --- | --- | --- | 
| ban đầu | {} | - | - | 
| sau khi đếm 1 | {1:1} | 1 | 1 | 
| sau khi đếm 2 | {1:1,2:1} | 1 | 2 | 
| sau khi đếm 3 | {1:1,2:1,3:1} | 1 | 3 | 

Điều này xác nhận rằng mỗi thanh tạo thành tháp riêng của nó. Tháp cao nhất là 1, tổng số tháp là 3. 

**Ví dụ tùy chỉnh** 

đầu vào:```
6
1 2 2 3 3 3
```| Bước | tần số | chiều cao tối đa | số_tháp | 
| --- | --- | --- | --- | 
| sau khi đếm hết | {1:1,2:2,3:3} | 3 | 3 | 

Điều này cho thấy độ dài lặp lại được xếp chồng lên nhau một cách chính xác. Tần số tối đa xác định tháp cao nhất, các phím riêng biệt xác định số lượng tháp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Một lượt để đọc đầu vào và một lượt để đếm bản dựng. | 
| Không gian | O(L) trong đó L ≤ 1000 | Từ điển tần số lưu trữ tối đa 1000 khóa, mỗi khóa có độ dài có thể. | 

Với N ₫ 1000 và độ dài ∼ 1000, giải pháp sẽ chạy thoải mái trong vòng 2 giây và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    N = int(input())
    lengths = list(map(int, input().split()))
    freq = {}
    for l in lengths:
        freq[l] = freq.get(l, 0) + 1
    return f"{max(freq.values())} {len(freq)}"

# provided sample
assert run("3\n1 2 3\n") == "1 3"

# all equal
assert run("4\n7 7 7 7\n") == "4 1"

# mixed repetitions
assert run("6\n1 2 2 3 3 3\n") == "3 3"

# minimum input
assert run("1\n5\n") == "1 1"

# maximum input with distinct values
assert run("1000\n" + " ".join(map(str, range(1,1001))) + "\n") == "1 1000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4\n7 7 7 7 | 4 1 | Tất cả các thanh giống hệt nhau, tháp đơn | 
| 6\n1 2 2 3 3 3 | 3 3 | Đếm hỗn hợp, tính toán tháp cao nhất | 
| 1\n5 | 1 1 | Đầu vào kích thước tối thiểu | 
| 1000\n1 2 3 ... 1000 | 1 1000 | Đầu vào có kích thước tối đa, tất cả đều khác biệt | 

## Vỏ cạnh 

Đối với đầu vào`4\n7 7 7 7`, từ điển tần số trở thành`{7:4}`.`max(freq.values())`trả về chính xác 4 cho tòa tháp cao nhất và`len(freq)`trả về 1. Không cần lặp lại các thanh riêng lẻ ngoài việc đếm. 

Đối với đầu vào`6\n1 2 2 3 3 3`, từ điển`{1:1,2:2,3:3}`tạo ra tháp cao nhất có chiều cao 3, số lượng tháp 3. Điều này xác nhận rằng thuật toán xử lý chính xác các lần lặp lại khác nhau mà không cần thêm logic để xếp chồng.
