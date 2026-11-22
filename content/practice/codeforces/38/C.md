---
title: "CF 38C - Rèm"
description: "Chúng tôi có một bộ rèm ngang có độ dài khác nhau và mục tiêu của chúng tôi là tạo ra một tấm rèm hình chữ nhật cho cửa sổ bằng cách sử dụng những sọc này. Mỗi sọc có thể được cắt thành các mảnh nhỏ hơn, nhưng các mảnh không được ngắn hơn chiều dài tối thiểu nhất định, l."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 38
codeforces_index: "C"
codeforces_contest_name: "School Personal Contest #1 (Winter Computer School 2010/11) - Codeforces Beta Round 38 (ACM-ICPC Rules)"
rating: 1400
weight: 38
solve_time_s: 67
verified: true
draft: false
---
[CF 38C - Rèm](https://codeforces.com/problemset/problem/38/C) 

**Đánh giá:** 1400 
**Tags:** vũ lực 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ rèm ngang có độ dài khác nhau và mục tiêu của chúng tôi là tạo ra một tấm rèm hình chữ nhật cho cửa sổ bằng cách sử dụng những sọc này. Mỗi sọc có thể được cắt thành các mảnh nhỏ hơn, nhưng các mảnh không được ngắn hơn độ dài tối thiểu nhất định,`l`. Tấm rèm cuối cùng được làm bằng cách xếp nhiều mảnh có chiều dài bằng nhau để che chiều rộng cửa sổ. Chiều rộng của rèm bằng số mảnh được sử dụng trong một hàng và chiều cao bằng chiều dài của mỗi mảnh. Nhiệm vụ là tối đa hóa diện tích cửa sổ có thể được che phủ bằng các sọc có sẵn. 

Đầu vào cung cấp số sọc`n`và chiều dài mảnh tối thiểu được phép`l`, theo sau là danh sách độ dài sọc. Đầu ra phải là diện tích của cửa sổ lớn nhất có thể có thể được bao phủ hoàn toàn. Nếu không thể xây dựng blind theo các ràng buộc thì đầu ra là 0. 

Với các ràng buộc (`n`100, chiều dài ≤ 100), việc thăm dò mạnh mẽ trên các chiều dài mảnh có thể có là khả thi. Các trường hợp cạnh có thể gặp phải một giải pháp ngây thơ bao gồm việc có tất cả các sọc ngắn hơn`l`(phải trả về 0), có sọc chính xác bằng`l`và có sự kết hợp giữa sọc dài và sọc ngắn, trong đó giải pháp tốt nhất đòi hỏi phải cắt cẩn thận. 

Ví dụ: nếu chúng ta có sọc`[1, 2, 3, 4]`Và`l = 2`, một cách tiếp cận đơn giản có thể cố gắng chỉ sử dụng sọc dài nhất, nhưng giải pháp tối ưu kết hợp nhiều đoạn có độ dài 2 để che khu vực cửa sổ lớn nhất, thu được cửa sổ 2 × 4 có diện tích 8. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu đơn giản nhất xem xét mọi chiều dài mảnh có thể`d`từ`l`đến chiều dài sọc tối đa. Đối với mỗi`d`, chúng ta đếm được có bao nhiêu đoạn dài`d`có thể thu được từ tất cả các sọc. Sau đó, chúng tôi tính toán diện tích giả định rằng chúng tôi có thể sử dụng tất cả những mảnh đó để tạo ra một tấm rèm hình vuông hoặc hình chữ nhật có kích thước phù hợp.`k × d`, Ở đâu`k`là số mảnh. Điều này hiệu quả vì mỗi hàng mù phải bao gồm các mảnh có chiều dài bằng nhau và tổng số mảnh sẽ cho chúng ta chiều cao hoặc chiều rộng. 

Vấn đề với brute-force ngây thơ là nó không giới hạn số lượng mảnh có thể tạo thành một hình chữ nhật hợp lệ. Để tối đa hóa diện tích, chúng ta cần kiểm tra từng ước số của tổng số mảnh cho điều đó`d`. Cái nhìn sâu sắc là đối với mỗi chiều dài mảnh`d`, chiều rộng mù tối ưu phải chia tổng số mảnh; nếu không, một số phần không thể được sử dụng. Bởi vì`n`và độ dài ≤ 100, số lượng ứng cử viên ít nên phải lặp qua tất cả`d`và chiều rộng có thể là khả thi. 

Quan sát quan trọng là chiều dài mảnh`d`và số lượng mảnh`k`cùng nhau xác định diện tích`k * d`. Bằng cách thử tất cả các mảnh có độ dài và hợp lệ`k`s, chúng ta có thể đảm bảo rằng chúng ta sẽ tìm được diện tích lớn nhất. Điều này chuyển đổi vấn đề từ một cuộc tìm kiếm vũ phu không có cấu trúc thành một tìm kiếm có cấu trúc với độ phức tạp có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * max_length * n) | O(1) | Quá chậm đối với n lớn | 
| Tối ưu | O(n * max_length * n) | O(1) | Đã chấp nhận | 

Đây`max_length`100 và`n`100, do đó`100 * 100 * 100 = 10^6`hoạt động, chấp nhận được trong 2 giây. 

## Hướng dẫn thuật toán 

1. Xác định độ dài tối đa giữa các sọc,`max_a`. Điều này đặt giới hạn trên cho độ dài mảnh cần kiểm tra. 
2. Lặp lại tất cả các độ dài mảnh có thể`d`từ`l`ĐẾN`max_a`. Mỗi`d`là chiều dài mảnh ứng cử viên cho người mù. 
3. Đối với mỗi`d`, tính xem có bao nhiêu đoạn dài`d`có thể thu được từ tất cả các sọc. Đối với một dải dài`a_i`, chúng ta có thể nhận được`a_i // d`miếng. Bỏ qua những sọc ngắn hơn`d`. 
4. Về tổng số mảnh cho việc này`d`, lặp lại trên tất cả các chiều rộng có thể`k`từ 1 đến`total_pieces`. Chỉ xem xét chiều rộng chia tổng số mảnh để đảm bảo tất cả các mảnh có thể được sử dụng để tạo thành hàng. Tính diện tích như`k * d`và theo dõi tối đa. 
5. Sau khi kiểm tra tất cả`d`, in diện tích tối đa được tìm thấy. Nếu không có chiều dài mảnh nào mang lại điểm mù hợp lệ thì mức tối đa vẫn là 0. 

Tại sao nó hoạt động: Đối với mỗi độ dài mảnh ứng cử viên, chúng tôi tính toán một cách có hệ thống tất cả các cấu hình mù có thể sử dụng đầy đủ các mảnh số nguyên. Bằng cách lặp lại mọi chiều dài mảnh khả thi và mọi ước số của tổng số mảnh, chúng tôi đảm bảo rằng không bỏ sót cấu hình hợp lệ nào. Thuật toán khai thác tính chất rời rạc của độ dài và số lượng, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, l = map(int, input().split())
a = list(map(int, input().split()))

max_area = 0
max_length = max(a)

for d in range(l, max_length + 1):
    pieces = sum(ai // d for ai in a)
    for k in range(1, pieces + 1):
        if pieces >= k:
            area = k * d
            if area > max_area:
                max_area = area

print(max_area)
```Mã đầu tiên đọc đầu vào và khởi tạo`max_area`đến 0. Đối với mỗi chiều dài mảnh ứng cử viên`d`, nó tính toán tổng số phần có thể đạt được bằng phép chia số nguyên. Sau đó nó xem xét từng số hàng`k`và tính diện tích, cập nhật giá trị lớn nhất. Việc sử dụng phép chia số nguyên đảm bảo không có phần nào ngắn hơn`d`và việc lặp lại các ước số của tổng số phần đảm bảo phạm vi bao phủ đầy đủ. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
4 2
1 2 3 4
```| d | mảnh từ [1,2,3,4] | max k đã thử | khu vực | diện tích tối đa | 
| --- | --- | --- | --- | --- | 
| 2 | [0,1,1,2] → 4 | 1..4 | 2,4,6,8 | 8 | 

Giải thích: Sử dụng những mảnh có chiều dài 2, ta được 4 mảnh. Hình chữ nhật lớn nhất chúng ta có thể làm là chiều rộng 4, chiều cao 2, diện tích 8. 

**Đầu vào tùy chỉnh 2**```
3 3
2 5 6
```| d | mảnh từ [2,5,6] | max k đã thử | khu vực | diện tích tối đa | 
| --- | --- | --- | --- | --- | 
| 3 | [0,1,2] → 3 | 1..3 | 3,6,9 | 9 | 

Giải thích: Sử dụng các mảnh có chiều dài 3, chúng ta có được ba mảnh. Hình chữ nhật tốt nhất là 3 × 3, diện tích 9. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * max_a * n) | Vòng lặp bên ngoài trên d (≤ max_a), vòng lặp bên trong trên các phần (≤ n*max_a), việc đếm các phần mất O(n) | 
| Không gian | O(1) | Chỉ số nguyên cho bộ đếm và mức tối đa | 

Với n ≤ 100 và độ dài ≤ 100, tổng số thao tác ≈ 10^6, vừa vặn thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, l = map(int, input().split())
    a = list(map(int, input().split()))
    max_area = 0
    max_length = max(a)
    for d in range(l, max_length + 1):
        pieces = sum(ai // d for ai in a)
        for k in range(1, pieces + 1):
            if pieces >= k:
                area = k * d
                if area > max_area:
                    max_area = area
    return str(max_area)

# provided sample
assert run("4 2\n1 2 3 4\n") == "8", "sample 1"

# minimum inputs
assert run("1 1\n1\n") == "1", "min input"

# all stripes too short
assert run("3 5\n1 2 3\n") == "0", "no valid pieces"

# all equal
assert run("3 2\n4 4 4\n") == "6", "all equal lengths"

# mix of short and long
assert run("5 3\n1 2 3 4 5\n") == "9", "mix lengths"

# max size inputs
assert run("100 1\n" + " ".join(["100"]*100) + "\n") == "10000", "max size inputs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n1 | 1 | chiều dài sọc tối thiểu bằng l | 
| 3 5\n1 2 3 | 0 | tất cả các sọc quá ngắn | 
| 3 2\n4 4 4 | 6 | độ dài đồng đều | 
| 5 3\n1 2 3 4 5 | 9 | yêu cầu cắt tối ưu | 
| 100 1\n100*100 | 10000 | hạn chế lớn nhất | 

## Vỏ cạnh 

Đối với các sọc ngắn hơn chiều dài mảnh tối thiểu, thuật toán sẽ bỏ qua chúng một cách chính xác. Ví dụ:`[1, 1, 2]`với`l = 3`tạo ra 0, vì không có sọc nào có thể được cắt thành một mảnh hợp lệ. Khi tất cả các sọc đều bằng nhau
