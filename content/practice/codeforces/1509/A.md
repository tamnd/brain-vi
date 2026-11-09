---
title: "CF 1509A - Chiều cao trung bình"
description: "Chúng ta được yêu cầu sắp xếp một nhóm học sinh thành một hàng sao cho số lượng cặp liên tiếp ăn ảnh là lớn nhất. Một cặp được coi là ăn ảnh nếu chiều cao trung bình của chúng là số nguyên. Đầu vào bao gồm nhiều trường hợp thử nghiệm."
date: "2026-06-10T19:47:05+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1509
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 715 (Div. 2)"
rating: 800
weight: 1509
solve_time_s: 181
verified: false
draft: false
---

[CF 1509A - Chiều cao trung bình](https://codeforces.com/problemset/problem/1509/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng 
**Thời gian giải:** 3m 1s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu sắp xếp một nhóm học sinh thành một hàng sao cho số lượng cặp liên tiếp ăn ảnh là lớn nhất. Một cặp được coi là ăn ảnh nếu chiều cao trung bình của chúng là số nguyên. Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi test case đưa ra một danh sách các số nguyên biểu thị chiều cao của học sinh. Đầu ra phải là hoán vị của các độ cao này cho mỗi trường hợp thử nghiệm để tối đa hóa số lượng cặp ăn ảnh. 

Quan sát chính là một cặp số có giá trị trung bình là số nguyên nếu cả hai số đều là số chẵn hoặc số lẻ. Điều này ngay lập tức biến vấn đề thành một phân loại đơn giản: chúng ta muốn các số liên tiếp có cùng tính chẵn lẻ. Tổng độ cao trên tất cả các trường hợp thử nghiệm được giới hạn bởi 2000, do đó, một thuật toán chạy theo thời gian tuyến tính đối với số độ cao trên mỗi trường hợp thử nghiệm là đủ. 

Các trường hợp đặc biệt có thể phức tạp bao gồm khi tất cả các số đều là số chẵn, tất cả các số đều là số lẻ hoặc chỉ có một số duy nhất của một số chẵn lẻ. Ví dụ, nếu chiều cao là`[2, 4, 6]`, bất kỳ thứ tự nào cũng tạo ra tất cả các cặp ăn ảnh. Nếu độ cao là`[1, 2, 3]`, cần phải sắp xếp cẩn thận để tối đa hóa các cặp liên tiếp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là tạo ra tất cả các hoán vị của danh sách độ cao, đếm số cặp liên tiếp ăn ảnh cho mỗi hoán vị và chọn cặp có số lượng tối đa. Vì`n`cho đến năm 2000, điều này rõ ràng là không khả thi vì nó có độ phức tạp về thời gian theo giai thừa. Ngay cả đối với`n=10`, có 3,6 triệu hoán vị. 

Cái nhìn sâu sắc quan trọng là tính chất ăn ảnh chỉ phụ thuộc vào tính chẵn lẻ. Bằng cách tách các số thành hai nhóm lẻ và chẵn, chúng ta có thể đảm bảo rằng mọi cặp số liên tiếp trong cùng một nhóm sẽ ăn ảnh. Điều này làm giảm vấn đề thành một nhiệm vụ sắp xếp hoặc nhóm đơn giản. Cách tiếp cận đơn giản nhất là xuất ra tất cả các số chẵn theo sau là tất cả các số lẻ (hoặc ngược lại). Bất kỳ hai số liên tiếp nào trong cùng một nhóm chẵn lẻ sẽ có tính ăn ảnh và không có cách nào để tạo thêm các cặp ăn ảnh giữa các số có chẵn lẻ đối diện. Điều này đảm bảo một sự sắp xếp tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Nhóm chẵn lẻ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc số học sinh`n`và danh sách chiều cao của họ. 
2. Khởi tạo hai danh sách trống: một danh sách có chiều cao lẻ và một danh sách có chiều cao chẵn. 
3. Lặp lại danh sách các độ cao. Nếu chiều cao là số lẻ, hãy thêm nó vào danh sách số lẻ. Nếu nó chẵn thì thêm nó vào danh sách chẵn. 
4. Nối danh sách chẵn và danh sách lẻ. Thứ tự này đảm bảo rằng tất cả các số liên tiếp trong mỗi nhóm có cùng tính chẵn lẻ, tối đa hóa số lượng cặp ăn ảnh. 
5. Xuất danh sách nối. 

Tại sao nó hoạt động: Các cặp liên tiếp có cùng số chẵn lẻ luôn tạo ra giá trị trung bình là số nguyên. Bằng cách nhóm tất cả các số chẵn lại với nhau và tất cả các số lẻ lại với nhau, chúng tôi đảm bảo số lượng tối đa các cặp như vậy. Bất kỳ nỗ lực nào để xen kẽ các số chẵn lẻ khác nhau đều không thể làm tăng số lượng các cặp ăn ảnh, vì vậy chiến lược này là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    heights = list(map(int, input().split()))
    odd = []
    even = []
    for h in heights:
        if h % 2 == 0:
            even.append(h)
        else:
            odd.append(h)
    result = even + odd
    print(" ".join(map(str, result)))
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Nó phân tách độ cao theo tính chẵn lẻ và nối các danh sách. Thứ tự này đảm bảo số lượng cặp liên tiếp ăn ảnh tối ưu. Các điều kiện biên, chẳng hạn như tất cả các số chẵn hoặc lẻ, được xử lý một cách tự nhiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[1, 1, 2]`| Bước | Danh sách chẵn | Danh sách lẻ | Kết quả | 
| --- | --- | --- | --- | 
| Ban đầu | [] | [] | [] | 
| 1 | [] | [1] | [] | 
| 2 | [] | [1, 1] | [] | 
| 3 | [2] | [1, 1] | [2, 1, 1] | 

Đầu ra:`[2, 1, 1]`(hoặc`[1, 1, 2]`). Cặp đôi ăn ảnh là`(1,1)`. 

### Ví dụ 2 

đầu vào:`[10, 9, 13, 15, 3, 16, 9, 13]`| Bước | Danh sách chẵn | Danh sách lẻ | Kết quả | 
| --- | --- | --- | --- | 
| Ban đầu | [] | [] | [] | 
| 10 | [10] | [] | [] | 
| 9 | [10] | [9] | [] | 
| 13 | [10] | [9,13] | [] | 
| 15 | [10] | [9,13,15] | [] | 
| 3 | [10] | [9,13,15,3] | [] | 
| 16 | [10,16] | [9,13,15,3] | [] | 
| 9 | [10,16] | [9,13,15,3,9] | [] | 
| 13 | [10,16] | [9,13,15,3,9,13] | [] | 
| Nối | [10,16] | [9,13,15,3,9,13] | [10,16,9,13,15,3,9,13] | 

Đầu ra tối đa hóa các cặp ăn ảnh trong mỗi nhóm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Một lượt để phân tách độ cao theo tính chẵn lẻ, một lượt để nối và in | 
| Không gian | O(n) | Hai danh sách bổ sung cho chiều cao lẻ và chẵn | 

Tổng của`n`trên tất cả các trường hợp thử nghiệm là 2000, do đó tổng số thao tác nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        heights = list(map(int, input().split()))
        odd = []
        even = []
        for h in heights:
            if h % 2 == 0:
                even.append(h)
            else:
                odd.append(h)
        result = even + odd
        print(" ".join(map(str, result)))
    return output.getvalue().strip()

# Provided samples
assert run("4\n3\n1 1 2\n3\n1 1 1\n8\n10 9 13 15 3 16 9 13\n2\n18 9\n") == "2 1 1\n1 1 1\n10 16 9 13 15 3 9 13\n18 9"

# Custom test cases
assert run("1\n2\n2 4\n") == "2 4", "all even numbers"
assert run("1\n3\n1 3 5\n") == "1 3 5", "all odd numbers"
assert run("1\n4\n1 2 3 4\n") == "2 4 1 3", "mix of odd and even"
assert run("1\n5\n5 4 3 2 1\n") == "4 2 5 3 1", "descending mix"
assert run("1\n2\n1 2\n") == "2 1", "minimum size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 4`|`2 4`| tất cả các số chẵn đều tạo ra tất cả các cặp ăn ảnh | 
|`1 3 5`|`1 3 5`| mọi số lẻ đều sinh ra những cặp ăn ảnh | 
|`1 2 3 4`|`2 4 1 3`| sắp xếp chẵn lẻ hỗn hợp tối đa hóa các cặp ăn ảnh | 
|`5 4 3 2 1`|`4 2 5 3 1`| đầu vào giảm dần với tính chẵn lẻ hỗn hợp | 
|`1 2`|`2 1`| trường hợp cạnh kích thước tối thiểu | 

## Vỏ cạnh 

Nếu tất cả các chiều cao có cùng độ cao, thuật toán sẽ đặt chúng vào nhóm ban đầu, tự động tạo ra số lượng cặp ăn ảnh tối đa. Ví dụ, đầu vào`[2, 4, 6]`kết quả là`[2,4,6]`, với hai cặp ăn ảnh`(2,4)`Và`(4,6)`. 

Nếu chỉ tồn tại một chiều cao của một mức chẵn lẻ nhất định, nó sẽ được đặt ở cuối phép nối, điều này là tối ưu. Vì`[1,2,4]`, các số chẵn`[2,4]`được đặt đầu tiên, theo sau là số lẻ duy nhất`1`. Những cặp đôi ăn ảnh`(2,4)`được tính và không thể có thêm cặp nào với số lẻ.
