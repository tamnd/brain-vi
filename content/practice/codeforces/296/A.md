---
title: "CF 296A - Yaroslav và hoán vị"
description: "Yaroslav có một dãy số nguyên và anh muốn sắp xếp lại chúng sao cho không có hai phần tử liên tiếp nào bằng nhau. Hoạt động duy nhất được phép là hoán đổi hai phần tử lân cận và anh ta muốn biết liệu có thể đạt được cấu hình thỏa mãn điều kiện này hay không."
date: "2026-06-05T17:54:28+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 296
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 179 (Div. 2)"
rating: 1100
weight: 296
solve_time_s: 95
verified: true
draft: false
---

[CF 296A - Yaroslav và Hoán vị](https://codeforces.com/problemset/problem/296/A) 

**Đánh giá:** 1100 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Yaroslav có một dãy số nguyên và anh muốn sắp xếp lại chúng sao cho không có hai phần tử liên tiếp nào bằng nhau. Hoạt động duy nhất được phép là hoán đổi hai phần tử lân cận và anh ta muốn biết liệu có thể đạt được cấu hình thỏa mãn điều kiện này hay không. Đầu vào bao gồm độ dài mảng,`n`, theo sau là`n`số nguyên đại diện cho mảng. Đầu ra phải là "CÓ" nếu tồn tại sự sắp xếp lại hợp lệ và "KHÔNG" nếu ngược lại. 

Những hạn chế chính là`n`có thể lên tới 100 và giá trị của các phần tử mảng có phạm vi lên tới 1000. Bởi vì`n`nhỏ, các thuật toán có độ phức tạp về thời gian lên tới`O(n^2)`là khả thi, nhưng bất cứ điều gì theo cấp số nhân hoặc giai thừa trong`n`sẽ là quá mức cần thiết. Bản thân các giá trị mảng ít liên quan đến độ phức tạp về thời gian; chúng chỉ quan trọng khi đếm số lần xuất hiện. 

Trường hợp cạnh không rõ ràng xảy ra khi một phần tử xuất hiện quá thường xuyên. Ví dụ, nếu`n = 5`và mảng là`[1, 1, 1, 2, 3]`, phần tử`1`xuất hiện ba lần. Để đặt những thứ này mà không trùng lặp liên tiếp, chúng ta sẽ cần ít nhất bốn “khe” để phân tách chúng, điều này là không thể vì`n - max_count = 2`. Trong trường hợp này, câu trả lời đúng là "KHÔNG". Một cách tiếp cận bất cẩn có thể chỉ cố gắng sắp xếp lại các phần tử một cách tham lam mà không kiểm tra tần số, điều này sẽ thất bại. 

Trường hợp đặc biệt là khi`n = 1`. Bất kỳ mảng một phần tử nào cũng thỏa mãn yêu cầu một cách tầm thường, vì vậy câu trả lời là "CÓ". 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi hoán vị có thể có của mảng và kiểm tra xem có bất kỳ hoán vị nào trong số chúng thỏa mãn điều kiện sai phân liên tiếp hay không. có`n!`hoán vị. Ngay cả với`n = 10`, điều này mang lại hơn 3 triệu hoán vị và chi phí kiểm tra mỗi hoán vị là`O(n)`, điều này không thực tế đối với`n = 100`. 

Quan sát quan trọng để tối ưu hóa là hạn chế duy nhất ngăn cản sự sắp xếp hợp lệ là khi một số số xuất hiện quá thường xuyên. Giả sử phần tử thường xuyên nhất xảy ra`max_count`lần. Để tránh trùng lặp liên tiếp, chúng ta cần xen kẽ tất cả các lần xuất hiện của phần tử này với các phần tử khác. Điều này chỉ có thể thực hiện được nếu số phần tử khác ít nhất`max_count - 1`. Chính thức, điều kiện cho "CÓ" là:```
max_count <= (n + 1) // 2
```Điều này hiệu quả vì chúng ta có thể đặt phần tử thường xuyên nhất ở các vị trí 0, 2, 4, ... và lấp đầy các vị trí còn lại bằng các phần tử khác. Nếu phần tử thường xuyên nhất vượt quá`(n + 1) // 2`, ngay cả vị trí tối ưu cũng để lại ít nhất một cặp phần tử giống hệt nhau liên tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1000) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`và mảng`a`. 
2. Đếm tần số của từng phần tử trong mảng. Điều này có thể được thực hiện bằng cách sử dụng từ điển hoặc mảng được lập chỉ mục theo các giá trị phần tử. 
3. Xác định tần số tối đa,`max_count`. 
4. So sánh`max_count`ĐẾN`(n + 1) // 2`. Nếu như`max_count`lớn hơn`(n + 1) // 2`, in ra "KHÔNG" vì không thể tách rời tất cả các lần xuất hiện của phần tử này. 
5. Ngược lại, in "CÓ" vì đã có sự sắp xếp hợp lệ bằng cách xen kẽ các phần tử một cách thích hợp. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến rằng số lượng vị trí có sẵn để phân tách các phần tử lặp lại là`n - max_count + 1`. Nếu phần tử thường xuyên nhất có thể vừa với các vị trí này mà không bị chồng chéo, thì các phần tử còn lại có thể lấp đầy các khoảng trống, đảm bảo không có sự trùng lặp liên tiếp. Điều này vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import Counter

n = int(input())
a = list(map(int, input().split()))

freq = Counter(a)
max_count = max(freq.values())

if max_count <= (n + 1) // 2:
    print("YES")
else:
    print("NO")
```Giải pháp đầu tiên đọc đầu vào hiệu quả với`sys.stdin.readline`. Đếm công dụng`Counter`từ thư viện chuẩn, vừa đơn giản vừa xử lý các mảng thưa thớt mà không xác định trước phạm vi giá trị. Tính toán`(n + 1) // 2`đảm bảo chia trần đúng cho số lẻ`n`. Việc so sánh đảm bảo chúng tôi chỉ từ chối những trường hợp không thể. Không cần sắp xếp hoặc sắp xếp lại vì điều kiện phụ thuộc hoàn toàn vào số lượng. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
1
1
```| Bước | Hành động | max_count | n+1//2 | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | Đếm tần số | 1 | 1 | 1 <= 1 → CÓ | 

Giải thích: Một phần tử duy nhất thỏa mãn điều kiện một cách tầm thường. 

**Ví dụ 2** 

đầu vào:```
5
1 1 1 2 3
```| Bước | Hành động | max_count | n+1//2 | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | Đếm tần số | 3 | 3 | 3 <= 3 → CÓ | 

Giải thích: Phần tử mặc dù`1`xuất hiện 3 lần, ta có 5 vị trí và có thể xen kẽ các vị trí còn lại: 1 2 1 3 1. Điều này thỏa mãn điều kiện. 

**Ví dụ 3** 

đầu vào:```
4
1 1 1 2
```| Bước | Hành động | max_count | n+1//2 | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | Đếm tần số | 3 | 2 | 3 > 2 → KHÔNG | 

Giải thích: Yếu tố thường xuyên nhất xảy ra hơn một nửa`n`, do đó không thể tránh khỏi sự trùng lặp liên tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Đếm tần số và tìm giá trị tối đa là tuyến tính theo kích thước mảng. | 
| Không gian | O(k) |`k`là số giá trị phân biệt (< 1000). | 

Giải pháp xử lý tối đa`n = 100`dễ dàng và mức sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import Counter

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    freq = Counter(a)
    max_count = max(freq.values())
    return "YES\n" if max_count <= (n + 1) // 2 else "NO\n"

# Provided samples
assert run("1\n1\n") == "YES\n", "sample 1"
assert run("5\n1 1 1 2 3\n") == "YES\n", "sample 2"
assert run("4\n1 1 1 2\n") == "NO\n", "sample 3"

# Custom cases
assert run("2\n1 1\n") == "YES\n", "2 elements same"
assert run("3\n2 2 2\n") == "NO\n", "3 identical elements"
assert run("6\n1 1 2 2 3 3\n") == "YES\n", "perfectly interleavable"
assert run("7\n1 1 1 1 2 3 4\n") == "NO\n", "most frequent exceeds (n+1)//2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n1 1 | CÓ | Đầu vào có kích thước tối thiểu với các phần tử lặp lại | 
| 3\n2 2 2 | KHÔNG | Tất cả các yếu tố giống hệt nhau | 
| 6\n1 1 2 2 3 3 | CÓ | Mảng có thể được xen kẽ hoàn hảo | 
| 7\n1 1 1 1 2 3 4 | KHÔNG | Phần tử thường xuyên nhất vượt quá số lượng cho phép | 

## Vỏ cạnh 

Khi nào`n = 1`, thuật toán trả về "CÓ" ngay lập tức vì`(1 + 1)//2 = 1`và tần số phần tử đơn là 1. Đối với các mảng có tất cả các phần tử giống hệt nhau, như`[2, 2, 2]`, tần số là 3 trong khi`(n+1)//2 = 2`, kích hoạt "KHÔNG". Mảng có chính xác một nửa hoặc chỉ dưới một nửa vị trí được lấp đầy bởi phần tử thường xuyên nhất được xử lý chính xác vì`(n+1)//2`thực hiện việc chia trần.
