---
title: "CF 2146A - Số lần xuất hiện bằng nhau"
description: "Chúng ta được cho một dãy số nguyên không giảm và chúng ta muốn tìm một dãy con có các phần tử xuất hiện cùng số lần. Một dãy con có thể bỏ qua các phần tử nhưng không thể thay đổi thứ tự của chúng."
date: "2026-06-08T01:25:39+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2146
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1052 (Div. 2)"
rating: 800
weight: 2146
solve_time_s: 72
verified: true
draft: false
---

[CF 2146A - Số lần xuất hiện bằng nhau](https://codeforces.com/problemset/problem/2146/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam, thực hiện 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên không giảm và chúng ta muốn tìm một dãy con có các phần tử xuất hiện cùng số lần. Một dãy con có thể bỏ qua các phần tử nhưng không thể thay đổi thứ tự của chúng. Ví dụ, từ`[1, 1, 4, 4, 4]`, dãy số sau`[1, 1, 4, 4]`được cân bằng vì`1`Và`4`xuất hiện đúng hai lần mỗi cái. Nhiệm vụ là xác định độ dài của chuỗi con cân bằng dài nhất như vậy. 

Các ràng buộc ở mức vừa phải: mỗi mảng có tối đa 100 phần tử và có tới 500 trường hợp thử nghiệm. Điều này cho thấy rằng một thuật toán có khoảng O(n²) hoạt động cho mỗi trường hợp thử nghiệm sẽ chạy thoải mái trong giới hạn thời gian. Kích thước nhỏ cũng cho phép chúng tôi xem xét các giải pháp dựa trên việc đếm tần số mà không phải lo lắng về việc tối ưu hóa độ phức tạp cao. 

Trường hợp cạnh tinh vi phát sinh khi tất cả các phần tử đều giống nhau. Vì`[3, 3, 3, 3, 3]`, toàn bộ mảng đã được cân bằng. Một trường hợp khác là khi mọi phần tử đều khác biệt, chẳng hạn như`[1, 2, 3]`; dãy con cân bằng ở đây là bất kỳ phần tử đơn lẻ nào hoặc tất cả các phần tử nếu mỗi phần tử xuất hiện một lần, điều này hiệu quả vì mỗi phần tử xuất hiện chính xác một lần. Một cách tiếp cận ngây thơ cố gắng hình thành các chuỗi con mà không nhìn vào tần số có thể tạo ra kết quả không chính xác trong những tình huống này. 

## Phương pháp tiếp cận 

Phương pháp brute-force là tạo ra tất cả các chuỗi con và kiểm tra xem chúng có cân bằng hay không. Đối với mỗi dãy con, hãy đếm số lần xuất hiện của từng phần tử và so sánh chúng. Điều này đúng vì nó xem xét thấu đáo mọi khả năng. Tuy nhiên, số lượng các chuỗi con tăng theo cấp số nhân (2ⁿ), khiến phương pháp này không khả thi ngay cả với n=20. 

Một cách tiếp cận tốt hơn tận dụng thực tế là các phần tử được sắp xếp và sử dụng số lượng tần số. Đầu tiên, hãy đếm xem mỗi số riêng biệt xuất hiện bao nhiêu lần. Sau đó, chuỗi con cân bằng dài nhất thu được bằng cách chọn một số k lần xuất hiện mà tất cả các phần tử có thể hỗ trợ. Ví dụ, nếu tần số là`[3, 5, 2]`, chúng ta có thể chọn k=2 vì tất cả các số xuất hiện ít nhất hai lần, tạo thành một dãy con có độ dài`2 * 3 = 6`. Việc lặp qua tất cả k có thể có từ 1 đến tần số tối đa của bất kỳ phần tử nào cho phép chúng ta tìm ra chuỗi con cân bằng lớn nhất một cách hiệu quả. 

Thông tin chi tiết quan trọng là độ dài của chuỗi con cân bằng là số lần xuất hiện được chọn nhân với số lần xuất hiện riêng biệt có thể cung cấp ít nhất số lần xuất hiện đó. Việc sắp xếp số đếm tần số giúp đơn giản hóa việc tìm độ dài tối đa, vì chúng ta chỉ cần kiểm tra các tần số tồn tại trong mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2ⁿ * n) | O(n) | Quá chậm | 
| Tối ưu | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case t. Đối với mỗi trường hợp thử nghiệm, hãy đọc n và mảng a. 
2. Đếm số lần xuất hiện của mỗi số riêng biệt. Vì mảng đã được sắp xếp nên chúng ta có thể thực hiện việc này chỉ trong một lần. Lưu số đếm vào danh sách`freq`. 
3. Khởi tạo`max_len`về 0. Điều này sẽ giữ độ dài của chuỗi con cân bằng dài nhất. 
4. Đối với mỗi số k có thể từ 1 đến tần số tối đa: 

một. Đếm xem có bao nhiêu phần tử trong`freq`lớn hơn hoặc bằng k. Gọi đây`num_elements`. 

b. Độ dài dãy con ứng cử viên là`k * num_elements`. Cập nhật`max_len`nếu cái này lớn hơn hiện tại`max_len`. 
5. Sau khi duyệt qua hết k, xuất ra`max_len`cho trường hợp thử nghiệm này. 

Điều bất biến là ở mỗi bước, chúng ta chỉ xem xét số lượng k có thể được cung cấp bởi ít nhất một phần tử. Bằng cách lặp lại tất cả k có thể, chúng tôi đảm bảo không bỏ lỡ độ dài chuỗi con tối ưu. Bởi vì chúng tôi tính toán`num_elements`đối với mỗi k, chúng tôi chỉ tính toán chính xác các phần tử có thể tham gia vào chuỗi con cân bằng có kích thước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    freq = []
    count = 1
    for i in range(1, n):
        if a[i] == a[i-1]:
            count += 1
        else:
            freq.append(count)
            count = 1
    freq.append(count)
    
    max_len = 0
    max_freq = max(freq)
    
    for k in range(1, max_freq + 1):
        num_elements = sum(1 for f in freq if f >= k)
        max_len = max(max_len, k * num_elements)
    
    print(max_len)
```Phần đầu tiên đọc đầu vào một cách hiệu quả và đếm tần số bằng cách quét tuyến tính đơn giản. các`freq`danh sách chứa số lần mỗi số duy nhất xuất hiện. Việc lặp lại các giá trị k có thể đảm bảo rằng chúng tôi xem xét tất cả các chuỗi con trong đó mỗi số xuất hiện cùng số lần. Việc đếm các phần tử có ít nhất k lần xuất hiện đảm bảo chuỗi con vẫn cân bằng. 

## Ví dụ đã hoạt động 

**Mẫu 1:**`[1, 1, 4, 4, 4]`| Bước | tần số | max_freq | k | số_elements | chiều dài ứng viên | max_len | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | [2,3] | 3 | 1 | 2 | 2 | 2 | 
| | | | 2 | 2 | 4 | 4 | 
| | | | 3 | 1 | 3 | 4 | 

Thuật toán chọn chính xác k=2 và cả hai số để tạo thành một dãy con cân bằng có độ dài 4. 

**Mẫu 2:**`[1,2]`| Bước | tần số | max_freq | k | số_elements | chiều dài ứng viên | max_len | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | [1,1] | 1 | 1 | 2 | 2 | 2 | 

Cả hai số đều xuất hiện một lần, do đó toàn bộ mảng được cân bằng, cho độ dài tối đa là 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Tần số đếm là O(n). Đối với mỗi k có tần số tối đa (< n), chúng tôi quét tần số (< n) để tính num_elements. | 
| Không gian | O(n) | Lưu trữ tần số của từng số riêng biệt. | 

Với n 100, thao tác O(n²) trên mỗi trường hợp thử nghiệm là khoảng 10.000 thao tác, nằm trong giới hạn 1 giây. Giải pháp chỉ sử dụng một mảng tuyến tính để lưu trữ tần số. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        freq = []
        count = 1
        for i in range(1, n):
            if a[i] == a[i-1]:
                count += 1
            else:
                freq.append(count)
                count = 1
        freq.append(count)
        max_len = 0
        max_freq = max(freq)
        for k in range(1, max_freq + 1):
            num_elements = sum(1 for f in freq if f >= k)
            max_len = max(max_len, k * num_elements)
        print(max_len)
    return output.getvalue().strip()

# Provided samples
assert run("4\n5\n1 1 4 4 4\n2\n1 2\n15\n1 1 1 1 1 2 2 2 2 3 3 3 4 4 5\n5\n3 3 3 3 3") == "4\n2\n9\n5"

# Custom cases
assert run("2\n1\n1\n3\n2 2 2") == "1\n3"  # minimum-size, all equal
assert run("1\n5\n1 2 3 4 5") == "5"  # all distinct
assert run("1\n6\n1 1 2 2 3 3") == "6"  # perfect balance
assert run("1\n7\n1 1 1 2 2 3 3") == "6"  # uneven counts
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1`|`1`| Mảng một phần tử | 
|`2 2 2`|`3`| Các phần tử hoàn toàn bằng nhau, cân bằng toàn bộ mảng | 
|`1 2 3 4 5`|`5`| Tất cả đều khác biệt, mỗi tính 1 | 
|`1 1 2 2 3 3`|`6`| Đã cân bằng hoàn hảo | 
|`1 1 1 2 2 3 3`|`6`| Phải loại trừ một lần xuất hiện của`1`để cân bằng | 

## Vỏ cạnh 

Đối với một mảng có tất cả các phần tử riêng biệt, chẳng hạn như`[1, 2, 3]`,`freq = [1,1,1]`. Lặp lại k=1, num_elements=3, độ dài ứng cử viên=3, khớp với độ dài mảng. Thuật toán trả về đúng 3. 

Đối với một mảng có tất cả các phần tử giống hệt nhau`[5,5,5,5]`, `tần số=[4
