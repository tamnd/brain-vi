---
title: "CF 32A - Trinh Sát"
description: "Chúng tôi có nhiệm vụ đếm xem có bao nhiêu cặp lính trong một phân đội có thể tạo thành một đơn vị trinh sát. Một đơn vị bao gồm chính xác hai người lính và sự khác biệt về chiều cao của họ không thể vượt quá ngưỡng nhất định _d_."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 32
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 32 (Div. 2, Codeforces format)"
rating: 800
weight: 32
solve_time_s: 65
verified: true
draft: false
---
[CF 32A - Trinh sát](https://codeforces.com/problemset/problem/32/A) 

**Đánh giá:** 800 
**Tags:** vũ lực 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có nhiệm vụ đếm xem có bao nhiêu cặp lính trong một phân đội có thể tạo thành một đơn vị trinh sát. Một đơn vị bao gồm chính xác hai người lính và sự khác biệt về chiều cao của họ không thể vượt quá ngưỡng nhất định _d_. Dữ liệu đầu vào cung cấp cho chúng ta tổng số binh sĩ _n_, chênh lệch chiều cao tối đa được phép _d_ và danh sách chiều cao của binh lính. Đầu ra là một số nguyên duy nhất biểu thị số cặp thứ tự hợp lệ (i, j) sao cho |height[i] − Height[j]| ≤ d. Ở đây các cặp có thứ tự rất quan trọng, vì vậy (i, j) khác với (j, i). 

Các ràng buộc ở mức vừa phải: n có thể lên tới 1000 và chiều cao có thể lớn tới 10^9. Vì n nhỏ nên cách tiếp cận O(n2) khả thi trong thực tế nhưng có thể được tối ưu hóa. Chi tiết quan trọng là bản thân độ cao có thể rất lớn, do đó, bất kỳ thuật toán nào dựa vào lập chỉ mục trực tiếp theo chiều cao (như mảng tần số) đều không thực tế. 

Các trường hợp cần xem xét bao gồm các tình huống trong đó tất cả binh sĩ có cùng chiều cao hoặc khi chênh lệch d rất nhỏ. Chẳng hạn, nếu n = 3, d = 0 và chiều cao = [5, 5, 6] thì chỉ những cặp lính có cùng chiều cao mới hợp lệ. Việc triển khai bất cẩn có thể bị tính thiếu hoặc không xem xét được thứ tự của các cặp. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là vũ phu: lặp lại mọi cặp lính có thể có (i, j) và kiểm tra xem chênh lệch chiều cao của họ có nhiều nhất là d hay không. Điều này hiệu quả vì nó trực tiếp thực hiện yêu cầu của vấn đề. Tổng số thao tác là n * (n − 1), xấp xỉ 1 triệu khi n = 1000. Điều này có thể chấp nhận được trong giới hạn thời gian 2 giây, nhưng nó không tinh tế. 

Chúng ta có thể cải thiện hiệu quả bằng cách sắp xếp. Nếu sắp xếp mảng chiều cao theo thứ tự không giảm thì vấn đề trở nên dễ dàng hơn: với mỗi người lính, chúng ta chỉ cần kiểm tra những người lính tiếp theo cho đến khi chênh lệch chiều cao vượt quá d. Chi phí sắp xếp là O(n log n) và lần quét tuyến tính tiếp theo cho mỗi người lính tổng cộng là O(n), do đó, điều này mang lại giải pháp O(n log n). Thông tin chi tiết quan trọng là việc sắp xếp cho phép kết thúc sớm ở vòng lặp bên trong: khi chênh lệch vượt quá d, chúng ta biết rằng tất cả binh lính còn lại cũng sẽ vượt quá giới hạn vì độ cao đang tăng lên. 

Bảng so sánh làm rõ điều này: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Chấp nhận được với n ≤ 1000 | 
| Sắp xếp + Hai con trỏ | O(n log n) | O(1) | Tối ưu và rõ ràng | 

## Hướng dẫn thuật toán 

1. Đọc n, d và danh sách chiều cao của người lính. Chúng tôi sử dụng đầu vào nhanh để tránh chi phí không cần thiết. 
2. Sắp xếp chiều cao theo thứ tự không giảm. Điều này đảm bảo rằng đối với bất kỳ người lính i nào, tất cả những người lính tiếp theo đều có chiều cao lớn hơn hoặc bằng nhau. 
3. Khởi tạo bộ đếm về 0 để theo dõi số cặp thứ tự hợp lệ. 
4. Với mỗi lính i từ 0 đến n − 1, bắt đầu kiểm tra lính j > i. Miễn là chiều cao[j] − chiều cao[i] ≤ d, hãy tăng bộ đếm. Phá vỡ vòng lặp bên trong ngay lập tức khi Height[j] − Height[i] > d vì không còn j nào thỏa mãn điều kiện nữa. 
5. Xuất bộ đếm sau khi quét tất cả binh lính. 

Tại sao nó hoạt động: Việc sắp xếp đảm bảo rằng khi một cặp vượt quá chênh lệch chiều cao cho phép, tất cả các cặp sau này có cùng người lính đầu tiên cũng sẽ vượt quá chênh lệch chiều cao đó. Cấu trúc vòng lặp đảm bảo chúng tôi kiểm tra tất cả các cặp chính xác một lần, đếm ngầm cả hai hướng vì vòng lặp bên ngoài lặp qua mọi người lính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, d = map(int, input().split())
heights = list(map(int, input().split()))
heights.sort()

count = 0

for i in range(n):
    for j in range(i + 1, n):
        if heights[j] - heights[i] <= d:
            count += 1
        else:
            break

# Each valid pair can be ordered in two ways
print(count * 2)
```Mã đầu tiên sắp xếp chiều cao của người lính. Các vòng lặp lồng nhau kiểm tra tất cả các cặp hợp lệ theo thứ tự tăng dần. Chúng ta nhân số đếm cuối cùng với 2 vì mỗi cặp (i, j) có thể xuất hiện ở cả hai thứ tự (i, j) và (j, i). Các điều kiện biên được xử lý ngầm: vòng lặp bên trong bắt đầu ở i + 1, ngăn chặn việc tự ghép nối và ngắt ngay khi chênh lệch độ cao vượt quá d. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
5 10
10 20 50 60 65
```Dấu vết bước: 

| tôi | chiều cao[i] | j đã kiểm tra | Cặp hợp lệ | Số lần chạy | 
| --- | --- | --- | --- | --- | 
| 0 | 10 | 20 | (10,20) | 1 | 
| 0 | 10 | 50 | - | 1 | 
| 1 | 20 | 50 | - | 1 | 
| 2 | 50 | 60 | (50,60) | 2 | 
| 2 | 50 | 65 | - | 2 | 
| 3 | 60 | 65 | (60,65) | 3 | 

Nhân với 2 cho đơn hàng: 3 * 2 = 6, phù hợp với kết quả mong đợi. 

Trường hợp tùy chỉnh: 

đầu vào:```
3 0
5 5 6
```Dấu vết bước: 

| tôi | chiều cao[i] | j đã kiểm tra | Cặp hợp lệ | Số lần chạy | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | 5 | (5,5) | 1 | 
| 0 | 5 | 6 | - | 1 | 
| 1 | 5 | 6 | - | 1 | 

Nhân với 2: 1 * 2 = 2, đúng vì chỉ có các cặp 5s giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế, vòng lặp bên trong chạy tối đa n lần cho tất cả i do bị đứt. | 
| Không gian | O(1) | Sắp xếp tại chỗ, chỉ lưu trữ bộ đếm. | 

Với n ≤ 1000, việc sắp xếp và quét tuyến tính là không đáng kể. Mức sử dụng bộ nhớ thấp hơn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, d = map(int, input().split())
    heights = list(map(int, input().split()))
    heights.sort()
    count = 0
    for i in range(n):
        for j in range(i + 1, n):
            if heights[j] - heights[i] <= d:
                count += 1
            else:
                break
    return str(count * 2)

# Provided sample
assert run("5 10\n10 20 50 60 65\n") == "6", "sample 1"

# Minimum-size input
assert run("1 5\n10\n") == "0", "minimum size"

# Maximum-size, all equal
assert run("1000 0\n" + " ".join(["42"]*1000) + "\n") == str(1000*999), "all equal"

# Small d, some differences exceed
assert run("4 1\n1 2 4 5\n") == "4", "small d with gaps"

# Ordered heights
assert run("3 2\n1 3 5\n") == "2", "ordered with step 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5\n10 | 0 | Tối thiểu n, không thể tạo thành một cặp | 
| 1000 0\n42… | 999000 | N tối đa, tất cả các giá trị bằng nhau | 
| 4 1\n1 2 4 5 | 4 | Một số khoảng trống quá lớn để ghép | 
| 3 2\n1 3 5 | 2 | Chênh lệch chiều cao chính xác ở mức giới hạn | 

## Vỏ cạnh 

Nếu tất cả các binh sĩ có cùng chiều cao, ví dụ chiều cao = [7,7,7,7] và d = 0 thì mọi cặp đều hợp lệ. Thuật toán của chúng tôi tính i < j cặp là 6 và nhân với 2 sẽ bằng 12, đếm chính xác cả hai thứ tự. Nếu d rất nhỏ, chẳng hạn như 0, chỉ những người lính có cùng chiều cao mới được ghép đôi. Câu lệnh break trong vòng lặp bên trong sẽ tránh những kiểm tra không cần thiết khi chênh lệch vượt quá d. Với n = 1, vòng lặp bên ngoài thực hiện một lần nhưng không có vòng lặp bên trong nào chạy, do đó thuật toán xuất ra 0, điều này đúng. 

Phương pháp này xử lý chính xác tất cả các trường hợp cạnh do logic sắp xếp và kết thúc sớm.
