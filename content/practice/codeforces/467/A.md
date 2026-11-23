---
title: "CF 467A - George và chỗ ở"
description: "Bài toán đưa ra một kịch bản đơn giản: George và Alex muốn cùng nhau tìm một phòng ký túc xá. Mỗi phòng trong ký túc xá đều có sức chứa hiện tại và sức chứa tối đa."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 467
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 267 (Div. 2)"
rating: 800
weight: 467
solve_time_s: 61
verified: true
draft: false
---

[CF 467A - George và Chỗ ở](https://codeforces.com/problemset/problem/467/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra một kịch bản đơn giản: George và Alex muốn cùng nhau tìm một phòng ký túc xá. Mỗi phòng trong ký túc xá đều có sức chứa hiện tại và sức chứa tối đa. Đầu vào cung cấp số lượng phòng và số lượng người ở hiện tại cũng như tổng sức chứa của mỗi phòng. Nhiệm vụ là đếm xem có bao nhiêu phòng có thể chứa cả George và Alex cùng một lúc. 

Các ràng buộc rất nhỏ: có tối đa 100 phòng và sức chứa của mỗi phòng không vượt quá 100. Điều này có nghĩa là bất kỳ thuật toán nào kiểm tra từng phòng riêng lẻ, ngay cả với một số kiểm tra lồng nhau, sẽ chạy thoải mái trong thời gian giới hạn. Vì các số đều là số nguyên nhỏ nên không có nguy cơ tràn số nguyên hoặc các vấn đề về độ chính xác. 

Một trường hợp khó phát hiện khi một căn phòng gần như đã kín chỗ. Ví dụ: một căn phòng có sức chứa 2 và 1 người chỉ có thể chứa thêm một người, vì vậy George và Alex đều không thể chuyển đến. Tương tự, nếu một căn phòng trống với sức chứa 2, nó có thể chứa chính xác cả hai. Không kiểm tra điều kiện "khoảng trống cho hai" sẽ cho kết quả không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi phòng, hãy tính không gian sẵn có bằng cách lấy sức chứa của phòng trừ đi số người hiện đang ở. Nếu không gian có sẵn ít nhất là 2, hãy tăng bộ đếm. Vì có tối đa 100 phòng nên việc này yêu cầu tối đa 100 phép kiểm tra số học đơn giản, điều này không quan trọng đối với các máy tính hiện đại. Không có cách tiếp cận tiệm cận nào nhanh hơn, vì chúng ta phải kiểm tra từng phòng ít nhất một lần. Cái nhìn sâu sắc quan trọng là nhận ra rằng "phòng có thể vừa cả hai" chỉ đơn giản là so sánh`q_i - p_i >= 2`. Sự đơn giản hóa nhỏ này giúp loại bỏ mọi công việc ghi sổ kế toán hoặc logic phức tạp không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

Vấn đề đủ đơn giản để giải pháp vũ phu cũng là tối ưu. 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`, số phòng 
2. Khởi tạo bộ đếm`count`về không. Điều này sẽ theo dõi số lượng phòng có thể chứa cả George và Alex. 
3. Đối với mỗi điều tiếp theo`n`dòng: 

1. Đọc số người ở hiện tại`p`và sức chứa của phòng`q`. 
2. Tính toán không gian có sẵn như`q - p`. 
3. Nếu dung lượng trống ít nhất là 2, hãy tăng`count`bằng 1. 
4. Sau khi xử lý tất cả các phòng, in giá trị của`count`. 

Tại sao nó hoạt động: Ở mỗi bước, thuật toán sẽ kiểm tra chính xác điều kiện cần thiết để George và Alex chuyển đến: phòng phải có ít nhất hai chỗ trống. Vì tất cả các phòng đều được xem xét và mỗi phòng được đánh giá độc lập nên số lượng cuối cùng phản ánh chính xác tất cả các phòng hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
count = 0

for _ in range(n):
    p, q = map(int, input().split())
    if q - p >= 2:
        count += 1

print(count)
```Giải pháp tuân theo thuật toán trực tiếp. Vòng lặp lặp lại trên mỗi phòng, trừ số lượng người sử dụng hiện tại khỏi sức chứa và tăng bộ đếm nếu có hai hoặc nhiều không gian. Sự lựa chọn của`q - p >= 2`trực tiếp thực hiện yêu cầu của bài toán. Tuy nhiên, việc sử dụng I/O nhanh là biện pháp phòng ngừa đối với kích thước đầu vào lớn hơn.`n ≤ 100`nó không thực sự cần thiết. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 1
2 2
3 3
```| Phòng | p | q | q - p | Có thể phù hợp với cả hai? | đếm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | Không | 0 | 
| 2 | 2 | 2 | 0 | Không | 0 | 
| 3 | 3 | 3 | 0 | Không | 0 | 

Đầu ra:`0`Dấu vết này cho thấy tất cả các phòng đều đã kín chỗ nên không có phòng nào có thể chứa được George và Alex. 

### Mẫu 2 (tùy chỉnh) 

đầu vào:```
4
0 2
1 3
2 5
3 5
```| Phòng | p | q | q - p | Có thể phù hợp với cả hai? | đếm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 2 | 2 | Có | 1 | 
| 2 | 1 | 3 | 2 | Có | 2 | 
| 3 | 2 | 5 | 3 | Có | 3 | 
| 4 | 3 | 5 | 2 | Có | 4 | 

Đầu ra:`4`Dấu vết này chứng tỏ rằng thuật toán đếm chính xác các phòng đã đầy một phần nhưng vẫn còn chỗ cho hai người. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phòng được kiểm tra đúng một lần và việc so sánh là thời gian không đổi. | 
| Không gian | O(1) | Chỉ có một bộ đếm duy nhất và các biến tạm thời được sử dụng; không cần cấu trúc dữ liệu bổ sung. | 

Với`n ≤ 100`, giải pháp là nhanh hơn mức cần thiết đối với giới hạn 1 giây. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    count = 0
    for _ in range(n):
        p, q = map(int, input().split())
        if q - p >= 2:
            count += 1
    return str(count)

# provided samples
assert run("3\n1 1\n2 2\n3 3\n") == "0", "sample 1"

# custom cases
assert run("4\n0 2\n1 3\n2 5\n3 5\n") == "4", "all fit"
assert run("1\n0 0\n") == "0", "room has zero capacity"
assert run("2\n0 1\n1 2\n") == "0", "rooms cannot fit both"
assert run("5\n0 2\n1 2\n0 3\n2 2\n1 4\n") == "3", "mixed case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3\n1 1\n2 2\n3 3\n`| 0 | Tất cả các phòng đều đầy đủ | 
|`4\n0 2\n1 3\n2 5\n3 5\n`| 4 | Nhiều phòng có không gian chính xác | 
|`1\n0 0\n`| 0 | Phòng không có sức chứa | 
|`2\n0 1\n1 2\n`| 0 | Phòng không thể chứa hai người | 
|`5\n0 2\n1 2\n0 3\n2 2\n1 4\n`| 3 | Phòng hỗn hợp đầy đủ và một phần | 

## Vỏ cạnh 

Nếu một phòng không có người ở hiện tại nhưng sức chứa nhỏ hơn 2 thì phòng đó không thể chứa cả hai, ví dụ:`p=0, q=1`. Thuật toán tính toán chính xác`q - p = 1 < 2`, Vì thế`count`không được tăng lên. Đối với một căn phòng còn đúng hai chỗ trống,`p=1, q=3`, thuật toán tính toán`q - p = 2`, tăng dần`count`một cách chính xác. Các phòng đã hết công suất sẽ tự động bị bỏ qua. Điều này khẳng định việc so sánh`q - p >= 2`xử lý chính xác tất cả các tình huống chiếm chỗ tinh tế.
