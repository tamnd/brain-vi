---
title: "CF 417A - Loại bỏ"
description: "Chúng tôi đang tổ chức một cuộc thi lập trình với mục tiêu là chọn ra ít nhất $n cdot m$ những người vào chung kết. Người tham gia có thể đủ điều kiện theo ba cách: thứ nhất, bằng cách chiến thắng một trong các vòng loại trừ chính, có vấn đề về $c$ và tạo ra người chiến thắng $n$; thứ hai, bằng cách giành được một trong…"
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 417
codeforces_index: "A"
codeforces_contest_name: "RCC 2014 Warmup (Div. 2)"
rating: 1500
weight: 417
solve_time_s: 99
verified: true
draft: false
---

[CF 417A - Loại bỏ](https://codeforces.com/problemset/problem/417/A) 

**Đánh giá:** 1500 
**Tags:** dp, triển khai, toán học 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang tổ chức một cuộc thi lập trình với mục tiêu là chọn ra ít nhất$n \cdot m$lọt vào vòng chung kết. Người tham gia có thể đủ điều kiện theo ba cách: thứ nhất, bằng cách chiến thắng một trong các vòng loại trừ chính, vòng này có$c$vấn đề và tạo ra$n$người chiến thắng; thứ hai, bằng cách chiến thắng một trong các vòng loại trừ bổ sung, có$d$vấn đề và tạo ra một người chiến thắng; thứ ba, bằng cách nằm trong số$k$những người chiến thắng được chọn trước sẽ tự động đủ điều kiện. Nhiệm vụ là sắp xếp các vòng thi sao cho tổng số bài toán được sử dụng trong tất cả các vòng được giảm thiểu trong khi vẫn đảm bảo đủ số lượng thí sinh lọt vào vòng chung kết theo yêu cầu. 

Đầu vào đưa ra quy mô của mỗi vòng trong các bài toán, số người chiến thắng trong mỗi vòng chính, số nhóm yêu cầu$m$và số người chiến thắng được chọn trước$k$. Đầu ra là tổng số vấn đề tối thiểu cần thiết để đạt được ít nhất$n \cdot m$lọt vào vòng chung kết. 

Bởi vì tất cả các giá trị đều ≤ 100 nên việc kiểm tra mạnh mẽ tất cả các kết hợp hợp lý giữa các vòng chính và vòng bổ sung là khả thi. Các ràng buộc ngăn cản chúng ta cần đến các cấu trúc dữ liệu nâng cao hoặc các thuật toán phức tạp. 

Phần khó khăn là những người chiến thắng được chọn trước sẽ giảm số vòng bổ sung cần thiết. Một cách tiếp cận ngây thơ mà bỏ qua$k$có thể giải quyết được toàn bộ vấn đề. Tương tự, các vấn đề làm tròn trong phép chia số nguyên rất quan trọng: nếu chúng ta cần thêm 15 người chiến thắng và vòng chính tạo ra 7, thì chúng ta thực sự cần ba vòng chính (hai vòng tạo ra 14 người chiến thắng, một vòng nữa tạo ra 7, vượt quá một chút). Việc thực hiện bất cẩn có thể mất hai hiệp và trượt mục tiêu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực xem xét tất cả sự kết hợp của các vòng chính và vòng bổ sung. Đối với mỗi số vòng chính có thể có, chúng tôi tính toán số người chiến thắng thu được. Nếu con số vẫn thấp hơn tổng số yêu cầu sau khi tính đến những người chiến thắng được chọn trước, chúng tôi sẽ thêm số vòng bổ sung tối thiểu để đạt đến ngưỡng. Chúng tôi tính toán tổng số vấn đề cho mỗi sự kết hợp và chọn mức tối thiểu. Điều này hiệu quả vì số vòng cần thiết bị giới hạn (tối đa 100 người chiến thắng mỗi vòng chính, tổng số người chiến thắng bắt buộc tối đa là 100), nhưng việc liệt kê tất cả các kết hợp vẫn liên quan đến các vòng lặp lồng nhau có thể tránh được bằng tính toán cẩn thận. 

Điểm mấu chốt là vấn đề rút gọn thành hai phép tính độc lập: số vòng chính cần thiết và số vòng bổ sung cần thiết sau khi tính đến người chiến thắng trong vòng chính và người chiến thắng được chọn trước. Có một mối quan hệ đơn điệu: việc thêm một vòng chính luôn làm tăng tổng số người chiến thắng và số vòng bổ sung cần thiết chỉ giảm đi. Cấu trúc này cho phép chúng ta lặp lại số vòng chính, tính toán chính xác các vòng bổ sung cần thiết về mặt toán học và tránh việc liệt kê bạo lực không cần thiết đối với các vòng bổ sung. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(T^2) cho tất cả các kết hợp vòng | O(1) | Hoạt động nhưng vòng lặp lồng nhau không cần thiết | 
| Tối ưu | O(T) chỉ để lặp lại các vòng chính | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số thí sinh lọt vào vòng chung kết theo yêu cầu$\text{total\_needed} = n \cdot m$. 
2. Khởi tạo các bài toán tổng nhỏ nhất thành một số rất lớn. 
3. Lặp lại số vòng chính$x$từ 0 đến số lượng tối đa có thể bao gồm những người chiến thắng được yêu cầu: về mặt kỹ thuật$\text{ceil}(\text{total\_needed}/n)$, nhưng chúng ta có thể lên tới 100 một cách an toàn do có những hạn chế. 
4. Đối với mỗi$x$, tính tổng số người chiến thắng từ các vòng chính:$\text{winners\_main} = x \cdot n$. 
5. Tính số người trúng thưởng còn lại cần thiết:$\text{remaining} = \text{total\_needed} - k - \text{winners\_main}$. Nếu giá trị này ≤ ​​0 thì không cần làm vòng bổ sung. 
6. Mặt khác, tính số vòng cần thiết:$y = \text{ceil}(\text{remaining}/1) = \text{remaining}$vì mỗi vòng bổ sung sẽ thêm một người chiến thắng. 
7. Tính tổng số bài toán đã sử dụng:$\text{total\_problems} = x \cdot c + y \cdot d$. Cập nhật mức tối thiểu nếu giá trị này nhỏ hơn. 
8. Sau khi kết thúc quá trình lặp, hãy in ra tổng số bài toán tối thiểu. 

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi xem xét tất cả số vòng chính hợp lý và tính toán số vòng bổ sung tối thiểu chính xác theo toán học. Bởi vì người chiến thắng chỉ tăng theo các vòng và các vòng bổ sung được tính toán tối ưu nên thuật toán đảm bảo tìm thấy tổng số vấn đề tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

c, d = map(int, input().split())
n, m = map(int, input().split())
k = int(input())

total_needed = n * m
min_problems = float('inf')

# iterate over possible numbers of main rounds
for main_rounds in range(0, 101):
    winners_from_main = main_rounds * n
    remaining_needed = total_needed - k - winners_from_main
    if remaining_needed <= 0:
        additional_rounds = 0
    else:
        additional_rounds = remaining_needed  # each additional round gives 1 winner
    total_problems = main_rounds * c + additional_rounds * d
    min_problems = min(min_problems, total_problems)

print(min_problems)
```Chúng tôi lặp lại các vòng chính có thể lên tới 100 do các hạn chế. Chúng tôi xử lý cẩn thận trường hợp còn lại 0 cần thiết để tránh các vòng bổ sung không cần thiết. Công thức làm tròn bổ sung chính xác, tránh sai số làm tròn. Tổng số tối thiểu được cập nhật trong mỗi lần lặp. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
1 10
7 2
1
```Dấu vết: 

| main_rounds | người chiến thắng_from_main | còn lại_cần thiết | bổ sung_rounds | tổng_vấn đề | vấn đề tối thiểu | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 13 | 13 | 0_1 + 13_10=130 | 130 | 
| 1 | 7 | 6 | 6 | 1 + 60=61 | 61 | 
| 2 | 14 | -1 | 0 | 2*1 + 0=2 | 2 | 

Thuật toán nhận thấy chính xác rằng sử dụng 2 vòng chính mà không có vòng bổ sung là tối ưu, với tổng số 2 bài toán. 

**Đầu vào tùy chỉnh**```
2 3
3 2
4
```Dấu vết: 

| main_rounds | người chiến thắng_from_main | còn lại_cần thiết | bổ sung_rounds | tổng_vấn đề | vấn đề tối thiểu | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 2 | 2 | 0 + 6=6 | 6 | 
| 1 | 3 | -1 | 0 | 2 + 0=2 | 2 | 

Giải pháp tối ưu sử dụng 1 vòng chính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(100) | Chúng tôi lặp lại main_rounds từ 0 đến 100. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ. | 

Độ phức tạp thấp hơn nhiều so với giới hạn 1 giây ngay cả đối với trường hợp xấu nhất. Thuật toán phù hợp thoải mái trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    old_input = builtins.input
    builtins.input = lambda: sys.stdin.readline()
    c, d = map(int, input().split())
    n, m = map(int, input().split())
    k = int(input())
    total_needed = n * m
    min_problems = float('inf')
    for main_rounds in range(0, 101):
        winners_from_main = main_rounds * n
        remaining_needed = total_needed - k - winners_from_main
        if remaining_needed <= 0:
            additional_rounds = 0
        else:
            additional_rounds = remaining_needed
        total_problems = main_rounds * c + additional_rounds * d
        min_problems = min(min_problems, total_problems)
    builtins.input = old_input
    return str(min_problems)

# provided sample
assert run("1 10\n7 2\n1\n") == "2", "sample 1"

# custom cases
assert run("2 3\n3 2\n4\n") == "2", "minimum additional round needed"
assert run("5 5\n10 10\n0\n") == "50", "large total, no pre-selected winners"
assert run("1 100\n1 1\n1\n") == "0", "pre-selected winner covers requirement"
assert run("10 1\n2 3\n0\n") == "6", "main round cheaper than additional rounds"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 10/7 2/1 | 2 | Cung cấp mẫu chính xác | 
| 2 3 / 3 2 / 4 | 2 | Chọn đúng vòng chính thay vì nhiều vòng bổ sung | 
| 5 5/10 10/0 | 50 | Cần xử lý tổng số tiền lớn hơn, không có người chiến thắng được chọn trước | 
| 1 100/1 1/1 | 0 | |
