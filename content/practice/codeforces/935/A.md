---
title: "CF 935A - Fafa và Công ty của anh ấy"
description: "Fafa cần chia nhân viên của công ty mình thành các nhóm để quản lý dự án. Anh ta muốn chọn một số trưởng nhóm, ký hiệu là l, và phân công đều số nhân viên còn lại trong số họ."
date: "2026-06-13T03:24:34+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 935
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 465 (Div. 2)"
rating: 800
weight: 935
solve_time_s: 549
verified: true
draft: false
---

[CF 935A - Fafa và Công ty của anh ấy](https://codeforces.com/problemset/problem/935/A) 

**Đánh giá:** 800 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 9 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Fafa cần chia nhân viên của công ty mình thành các nhóm để quản lý dự án. Anh ấy muốn chọn một số trưởng nhóm, ký hiệu là _l_, và phân công đều số nhân viên còn lại trong số họ. Mỗi trưởng nhóm phải giám sát cùng một số lượng nhân viên và không có nhân viên nào không có trưởng nhóm. Đầu vào là một số nguyên _n_, tổng số nhân viên. Đầu ra là số giá trị có thể có của _l_ sao cho có thể thực hiện được phép chia hợp lý này. 

Trình bày lại, chúng ta đang tìm kiếm tất cả các số nguyên dương _l_ nhỏ hơn _n_ mà số số không đứng đầu, _n - l_, chia hết cho _l_. Điều này đảm bảo rằng số nhân viên còn lại có thể được chia đều cho các nhà lãnh đạo. Ví dụ: nếu có 6 nhân viên thì chọn 2 người lãnh đạo còn lại 4 người không phải là người lãnh đạo. Vì 4 chia hết cho 2 nên mỗi lãnh đạo có thể quản lý 2 nhân viên. 

Những hạn chế là khiêm tốn nhưng đáng kể. _n_ có thể lớn tới 100.000. Một giải pháp đơn giản thử tất cả các nhóm có thể có cho tất cả các giá trị lên tới _n_ sẽ yêu cầu tới 100.000 thao tác. Điều này có thể chấp nhận được trong giới hạn thời gian 1 giây nếu mỗi thao tác đơn giản, nhưng đáng để suy nghĩ về các ước số để tránh những lần kiểm tra không cần thiết. Các trường hợp biên bao gồm _n = 2_, số lượng nhân viên nhỏ nhất, trong đó số lượng lãnh đạo hợp lệ duy nhất là 1. Một trường hợp tinh vi khác là khi _n_ là số nguyên tố; thì chỉ có 1 người lãnh đạo làm việc vì mọi lựa chọn khác đều để lại phần còn lại. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu xem xét mọi số lượng người lãnh đạo có thể có _l_ từ 1 đến _n - 1_. Đối với mỗi _l_, chúng tôi kiểm tra xem số lượng nhân viên còn lại, _n - l_, có chia hết cho _l_ hay không. Điều này đúng, nhưng nó thực hiện các phép chia _n-1_. Trong trường hợp xấu nhất khi _n = 100.000_, tức là khoảng 100.000 lần kiểm tra, điều này khả thi nhưng hơi không phù hợp. 

Quan sát quan trọng là vấn đề giảm xuống việc đếm các ước số của _n - l_ nhỏ hơn _n_. Cụ thể hơn, chúng ta đang tìm kiếm tất cả _l_ trong đó _l_ chia _n - l_. Nếu chúng ta viết lại điều kiện này, nó sẽ trở thành _n - l = k * l_ với một số nguyên k ≥ 1. Sắp xếp lại sẽ có _n = l * (k + 1)_. Do đó, _l_ phải là ước số của _n_ nhỏ hơn _n_. Phép biến đổi này loại bỏ những kiểm tra không cần thiết và là một bài toán về ước số cổ điển. Chỉ lặp lại các ước số lên tới √n đảm bảo độ phức tạp O(√n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Hoạt động nhưng có thể được tối ưu hóa | 
| Tối ưu | O(√n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên đầu vào _n_. Con số này thể hiện tổng số nhân viên trong công ty của Fafa. 
2. Khởi tạo bộ đếm về 0. Điều này sẽ theo dõi số lượng lựa chọn hợp lệ của các trưởng nhóm. 
3. Lặp lại _l_ từ 1 đến √n. Với mỗi _l_, hãy kiểm tra xem nó có chia hết _n_ không. 
4. Nếu _l_ chia _n_ thì là ứng viên tiềm năng vì nó thỏa mãn _n = l * (k + 1)_. Nếu _l_ nhỏ hơn _n_, hãy tăng bộ đếm. 
5. Đồng thời kiểm tra ước số tương ứng _n // l_. Nếu nó khác _l_ và nhỏ hơn _n_, hãy tăng bộ đếm. 
6. Sau khi xử lý tất cả các ước số, hãy in bộ đếm. Đây là số cách hợp lệ để chọn trưởng nhóm. 

Tại sao nó hoạt động: Bất biến chính là số đầu _l_ hợp lệ khi và chỉ khi nó chia hết _n_ và hoàn toàn nhỏ hơn _n_. Bằng cách lặp lại tất cả các ước số của _n_, chúng tôi đảm bảo rằng tất cả _l_ hợp lệ tiềm năng đều được xem xét chính xác một lần. Không có giá trị nào khác có thể thỏa mãn điều kiện công bằng, do đó bộ đếm là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
count = 0
i = 1
while i * i <= n:
    if n % i == 0:
        if i < n:
            count += 1
        if i != n // i and n // i < n:
            count += 1
    i += 1
print(count)
```Giải pháp sử dụng phép chia số nguyên để tìm các ước số một cách hiệu quả. Chúng tôi kiểm tra cả _i_ và _n // i_ để tính đến các cặp ước số. Việc kiểm tra ranh giới đảm bảo rằng số lượng người lãnh đạo hoàn toàn nhỏ hơn _n_, tránh trường hợp không hợp lệ khi tất cả nhân viên đều là người lãnh đạo. Vòng lặp dừng ở √n để tránh việc kiểm tra dư thừa, giúp giải pháp trở nên hiệu quả. 

## Ví dụ đã hoạt động 

**Ví dụ 1**: n = 2 

| tôi | n% tôi == 0? | Đếm | 
| --- | --- | --- | 
| 1 | Có | 1 | 

Chúng ta lặp lại i=1, nó chia cho 2 và 1 < 2, do đó count = 1. Ước số tương ứng n // i = 2 không phải là < n, vì vậy chúng ta không tăng thêm. Kết quả là 1. 

**Ví dụ 2**: n = 6 

| tôi | n% tôi == 0? | Cặp chia số | Đếm | 
| --- | --- | --- | --- | 
| 1 | Có | 1 & 6 | 1 | 
| 2 | Có | 2 & 3 | 3 | 

Các ước số nhỏ hơn 6 là 1, 2, 3. Mỗi ước số đều hợp lệ. Đếm = 3. 

Điều này chứng tỏ rằng tất cả các lựa chọn hợp lệ đều được ghi lại và không có giá trị bổ sung nào được tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(√n) | Chúng tôi chỉ lặp lại tối đa √n để tìm tất cả các ước số. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được sử dụng. | 

Với n ≤ 100.000, √n ≈ 317, do đó cần ít hơn 400 lần lặp. Giải pháp dễ dàng phù hợp với giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    count = 0
    i = 1
    while i * i <= n:
        if n % i == 0:
            if i < n:
                count += 1
            if i != n // i and n // i < n:
                count += 1
        i += 1
    return str(count)

# Provided sample
assert run("2\n") == "1", "sample 1"

# Custom cases
assert run("6\n") == "3", "divisors 1, 2, 3"
assert run("12\n") == "5", "divisors 1, 2, 3, 4, 6"
assert run("17\n") == "1", "prime number, only 1 leader"
assert run("100000\n") == "35", "large n, many divisors"
assert run("3\n") == "1", "small n, minimal case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 6 | 3 | bội số chuẩn | 
| 12 | 5 | nhiều cặp ước số | 
| 17 | 1 | trường hợp cạnh số nguyên tố | 
| 100000 | 35 | hiệu suất đầu vào lớn | 
| 3 | 1 | đầu vào nhỏ tối thiểu | 

## Vỏ cạnh 

Với _n = 2_, chỉ có thể có một người dẫn đầu. Vòng lặp lặp i = 1. 1 < 2, do đó số đếm tăng lên 1. Ước số 2 tương ứng không được tính, do đó đầu ra là 1, xử lý chính xác đầu vào nhỏ nhất. 

Đối với số nguyên tố _n = 17_, chỉ i = 1 chia hết 17 với i < 17, do đó count = 1. Ước số 17 tương ứng bị bỏ qua. Thuật toán tránh tính toán quá mức và xử lý các số nguyên tố một cách chính xác. 

Đối với một hình vuông hoàn hảo như _n = 36_, ước số bao gồm 6, là căn bậc hai. Việc kiểm tra i != n // i ngăn chặn việc đếm hai lần 6, đảm bảo mỗi số lượng người lãnh đạo hợp lệ được tính chính xác một lần.
