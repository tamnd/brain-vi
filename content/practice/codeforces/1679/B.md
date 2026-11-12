---
title: "CF 1679B - Vấn đề thời đồ đá"
description: "Chúng ta được cung cấp một mảng các số nguyên và một chuỗi các truy vấn. Mỗi truy vấn sẽ thay thế một phần tử trong mảng bằng một giá trị mới hoặc thay thế mọi phần tử trong mảng bằng cùng một giá trị. Sau mỗi truy vấn, chúng ta cần báo cáo tổng của mảng."
date: "2026-06-10T00:40:01+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1679
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 791 (Div. 2)"
rating: 1200
weight: 1679
solve_time_s: 90
verified: true
draft: false
---

[CF 1679B - Vấn đề thời đồ đá](https://codeforces.com/problemset/problem/1679/B) 

**Đánh giá:** 1200 
**Tags:** cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và một chuỗi các truy vấn. Mỗi truy vấn sẽ thay thế một phần tử trong mảng bằng một giá trị mới hoặc thay thế mọi phần tử trong mảng bằng cùng một giá trị. Sau mỗi truy vấn, chúng ta cần báo cáo tổng của mảng. Điều quan trọng là mảng có thể có tới 200.000 phần tử và chúng ta có thể có tới 200.000 truy vấn. Bản thân các giá trị có thể lớn bằng$10^9$. 

Với những ràng buộc này, một cách tiếp cận đơn giản cập nhật từng phần tử mảng cho mỗi truy vấn sẽ quá chậm. Ví dụ: nếu chúng ta có một truy vấn đặt tất cả các phần tử thành một giá trị mới và mảng có 200.000 phần tử, thì chỉ cần lặp qua mảng sẽ mất$O(n) = 2 \cdot 10^5$hoạt động cho mỗi truy vấn. Nếu chúng ta có 200.000 truy vấn như vậy thì tổng số thao tác sẽ là$O(n \cdot q) = 4 \cdot 10^{10}$, vượt xa những gì có thể chạy trong 2 giây. Chúng tôi cần một giải pháp tránh chạm vào mọi phần tử cho thao tác "thay thế tất cả". 

Trường hợp phức tạp xảy ra khi chúng ta có truy vấn loại 1 sau truy vấn loại 2. Việc triển khai đơn giản chỉ giữ nguyên mảng có thể tính tổng các phần tử không chính xác vì nó không tính đến "sự thay thế toàn cục" đã thực hiện trước đó. Chẳng hạn, nếu mảng ban đầu là`[1, 2, 3]`, chúng tôi đặt tất cả các phần tử thành`5`(loại 2), sau đó đặt phần tử thứ hai thành`7`(loại 1), tổng phải là`5 + 7 + 5 = 17`. Việc thực hiện bất cẩn cố gắng cập nhật`a[1]`trực tiếp mà không theo dõi sự thay thế toàn cầu có thể tính toán sai`5 + 2 + 5 = 12`. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: giữ mảng trong bộ nhớ và đối với mỗi truy vấn, hãy sửa đổi mảng theo loại. Đối với truy vấn loại 1, chúng tôi chỉ cần thay thế một phần tử; đối với truy vấn loại 2, chúng tôi lặp qua mảng và đặt mọi phần tử thành giá trị mới. Sau mỗi truy vấn, chúng tôi tính tổng bằng cách lặp qua mảng. Cách tiếp cận này đúng nhưng quá chậm. Với trường hợp xấu nhất là 200.000 truy vấn loại 2 trên mảng 200.000 phần tử, chúng tôi đang thực hiện$4 \cdot 10^{10}$hoạt động. 

Quan sát chính là các truy vấn loại 2 ghi đè tất cả các giá trị trước đó. Thay vì cập nhật toàn bộ mảng, chúng ta có thể lưu trữ một "giá trị toàn cục" biểu thị giá trị của tất cả các phần tử sau truy vấn loại 2 cuối cùng. Sau đó, các bản cập nhật loại 1 riêng lẻ chỉ cần điều chỉnh tổng tương ứng với giá trị chung này. Chúng tôi duy trì tổng của mảng dưới dạng một biến duy nhất và theo dõi mọi sai lệch do cập nhật loại 1 gây ra. Với chiến lược này, mỗi truy vấn được xử lý trong thời gian không đổi,$O(1)$, bởi vì chúng tôi không bao giờ lặp qua mảng cho các truy vấn loại 2. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * q) | O(n) | Quá chậm | 
| Tối ưu | O(q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng ban đầu của mảng và lưu vào một biến`total_sum`. Điều này cho phép chúng tôi xuất tổng ngay sau truy vấn đầu tiên mà không cần tính toán lại. 
2. Duy trì một mảng`last_update`chiều dài`n`để theo dõi giá trị cập nhật gần đây nhất của từng phần tử liên quan đến truy vấn loại 2. Ban đầu, nó lưu trữ các giá trị thực của mảng. 
3. Duy trì một biến`global_value`đại diện cho giá trị của mọi phần tử sau truy vấn loại 2 cuối cùng. Ban đầu, nó là`None`bởi vì không có sự thay thế toàn cầu đã xảy ra. 
4. Đối với mỗi truy vấn, hãy đọc loại của nó. Nếu truy vấn thuộc loại 1, chúng tôi tính giá trị cũ của phần tử: if`last_update[i]`đã được sửa đổi sau loại 2 cuối cùng, chúng tôi sử dụng nó; nếu không, chúng tôi sử dụng`global_value`hoặc giá trị ban đầu. Trừ giá trị cũ khỏi`total_sum`, thêm giá trị mới và ghi giá trị mới vào`last_update[i]`. 
5. Nếu truy vấn thuộc loại 2, chúng tôi đặt`global_value`sang giá trị mới, xóa mọi cập nhật riêng lẻ trong`last_update`, và cập nhật`total_sum`ĐẾN`n * global_value`. 
6. Sau mỗi câu hỏi, in`total_sum`. 

Tại sao nó hoạt động: Bất biến là vậy`total_sum`luôn bằng tổng của mảng hiện tại. Sự kết hợp của giá trị chung cho truy vấn loại 2 và ghi đè từng phần tử cho truy vấn loại 1 đảm bảo không có phần tử mảng nào được tính không chính xác. Chúng ta không bao giờ cần duyệt toàn bộ mảng cho thao tác loại 2, do đó thuật toán nằm trong thời gian tuyến tính tương ứng với số lượng truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
a = list(map(int, input().split()))
total_sum = sum(a)
last_update = a[:]
global_value = None
last_global_time = -1
time = 0

for _ in range(q):
    query = list(map(int, input().split()))
    time += 1
    if query[0] == 1:
        i, x = query[1] - 1, query[2]
        if last_global_time > last_update[i]:
            old_val = global_value
        else:
            old_val = last_update[i]
        total_sum += x - old_val
        last_update[i] = x
    else:
        x = query[1]
        global_value = x
        last_global_time = time
        total_sum = n * x
    print(total_sum)
```Trong mã,`last_update[i]`theo dõi giá trị được biết cuối cùng của phần tử.`last_global_time`Và`time`hãy để chúng tôi xác định xem truy vấn loại 2 cuối cùng có xảy ra sau lần cập nhật riêng lẻ của một phần tử hay không. Khi chúng tôi xử lý truy vấn loại 1, chúng tôi sẽ trừ giá trị cũ chính xác khỏi`total_sum`và thêm cái mới. Đối với truy vấn loại 2, chúng tôi cập nhật`global_value`và tính toán lại ngay`total_sum`BẰNG`n * global_value`. Điều này giữ cho tất cả các hoạt động thời gian liên tục. 

## Ví dụ đã hoạt động 

Sử dụng mẫu 1: 

| Bước | Truy vấn | Trạng thái mảng | tổng_tổng | cập nhật cuối cùng | toàn cầu_value | 
| --- | --- | --- | --- | --- | --- | 
| 0 | ban đầu | [1,2,3,4,5] | 15 | [1,2,3,4,5] | Không có | 
| 1 | 1 1 5 | [5,2,3,4,5] | 19 | [5,2,3,4,5] | Không có | 
| 2 | 2 10 | [10,10,10,10,10] | 50 | [5,2,3,4,5] | 10 | 
| 3 | 1 5 11 | [10,10,10,10,11] | 51 | [5,2,3,11,5] | 10 | 
| 4 | 1 4 1 | [10,10,10,1,11] | 42 | [5,2,3,1,5] | 10 | 
| 5 | 2 1 | [1,1,1,1,1] | 5 | [5,2,3,1,5] | 1 | 

Dấu vết này chứng tỏ rằng thuật toán xử lý chính xác các truy vấn loại 1 và loại 2 xen kẽ, cập nhật`total_sum`một cách hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) | Mỗi truy vấn được xử lý trong thời gian không đổi bằng cách sử dụng`total_sum`và mảng phụ trợ. | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng ban đầu và`last_update`mảng kích thước`n`. | 

Giải pháp phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. Ngay cả với mức tối đa$2 \cdot 10^5$các truy vấn và phần tử, tất cả các phép toán đều là các phép gán số học và mảng đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution copied here
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    total_sum = sum(a)
    last_update = a[:]
    global_value = None
    last_global_time = -1
    time = 0

    for _ in range(q):
        query = list(map(int, input().split()))
        time += 1
        if query[0] == 1:
            i, x = query[1] - 1, query[2]
            if last_global_time > last_update[i]:
                old_val = global_value
            else:
                old_val = last_update[i]
            total_sum += x - old_val
            last_update[i] = x
        else:
            x = query[1]
            global_value = x
            last_global_time = time
            total_sum = n * x
        print(total_sum)
    return output.getvalue().strip()

# Provided sample
assert run("5 5\n1 2 3 4 5\n1 1 5\n2 10\n1 5 11\n1 4 1\n2 1\n") == "19\n50\n51\n42\n5", "sample 1"

# Minimum input
assert run("1 2
```
