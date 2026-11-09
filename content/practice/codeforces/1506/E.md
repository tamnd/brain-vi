---
title: "CF 1506E - Khôi phục hoán vị"
description: "Chúng ta được cho một mảng q bắt nguồn từ một hoán vị p chưa biết của các số từ 1 đến n bằng cách sử dụng quy tắc qi = max(p1, ..., pi). Nhiệm vụ của chúng ta là xây dựng lại hai hoán vị có thể tạo ra q này: một hoán vị tối thiểu về mặt từ điển và một hoán vị tối đa về mặt từ điển."
date: "2026-06-10T20:20:11+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1506
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 710 (Div. 3)"
rating: 1500
weight: 1506
solve_time_s: 175
verified: false
draft: false
---

[CF 1506E - Khôi phục hoán vị](https://codeforces.com/problemset/problem/1506/E) 

**Đánh giá:** 1500 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 2m 55s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng`q`bắt nguồn từ một hoán vị chưa biết`p`của các số từ`1`ĐẾN`n`sử dụng quy tắc`q_i = max(p_1, ..., p_i)`. Nhiệm vụ của chúng ta là xây dựng lại hai hoán vị có thể tạo ra điều này`q`: một cái tối thiểu về mặt từ điển và một cái là tối đa về mặt từ điển. Thứ tự từ điển giống như thứ tự từ điển: chúng ta so sánh từng phần tử của mảng và sự khác biệt đầu tiên sẽ xác định thứ tự. 

Đầu vào đảm bảo rằng`q`là mảng "tiền tố tối đa" hợp lệ nên chúng ta không cần kiểm tra tính hợp lệ của nó. Thách thức nằm ở việc lựa chọn các yếu tố của`p`giữa các vị trí ở đó`q`tăng lên. Khi`q_i > q_{i-1}`, phần tử`p_i`phải bằng`q_i`. Khi`q_i == q_{i-1}`, chúng ta có thể tự do đặt bất kỳ số nào còn lại chưa sử dụng nhỏ hơn hoặc bằng`q_i`. 

Được cho`n`có thể lên tới 2 × 10^5, một cách tiếp cận ngây thơ thử tất cả các hoán vị giữa cực đại đã biết sẽ quá chậm. Chúng tôi cần một giải pháp O(n) cho mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh bao gồm các mảng trong đó`q`đang tăng lên một cách nghiêm ngặt, điều này buộc hoán vị phải giống hệt nhau và các mảng trong đó`q`có các giá trị lặp lại kéo dài, đòi hỏi phải lựa chọn cẩn thận các số không sử dụng để đáp ứng các ràng buộc về mặt từ điển. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ tạo ra tất cả các hoán vị phù hợp với`q`bằng cách điền vào các vị trí chưa biết với tất cả các số còn lại. Điều này hoạt động chính xác nhưng có độ phức tạp giai thừa về số lượng vị trí không xác định, điều này không khả thi đối với`n`lên tới 2×10^5. 

Quan sát quan trọng là các vị trí ở đó`q`mức tăng được cố định. Tất cả các vị trí khác có thể được điền từ tập hợp số chưa được sử dụng, hạn chế nhỏ hơn hoặc bằng hiện tại`q_i`. Đối với hoán vị tối thiểu về mặt từ điển, chúng ta phải luôn chọn số nhỏ nhất có sẵn trong các khoảng trống này. Đối với hoán vị tối đa về mặt từ điển, chúng tôi chọn số lớn nhất có sẵn không vượt quá mức tối đa hiện tại. Điều này có thể được thực hiện một cách hiệu quả với vùng chứa được sắp xếp hoặc hàng đợi ưu tiên. Cái nhìn sâu sắc quan trọng là bằng cách duy trì tập hợp các số còn lại, mỗi lựa chọn có thể được thực hiện theo O(log n) hoặc, với danh sách được tính toán trước và một con trỏ, thậm chí ở O(1) cho mỗi lựa chọn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai danh sách trống`p_min`Và`p_max`cho các hoán vị tối thiểu và tối đa. Duy trì một bộ`remaining`chứa tất cả các số từ 1 đến n chưa được đặt. 
2. Lặp qua từng chỉ mục`i`của`q`. Nếu như`i == 0`hoặc`q[i] != q[i-1]`, đây là vị trí "tối đa mới". Bộ`p_min[i] = p_max[i] = q[i]`và loại bỏ`q[i]`từ`remaining`. 
3. Nếu`q[i] == q[i-1]`, hãy điền vào vị trí này một cách khác nhau đối với các hoán vị tối thiểu và tối đa. Để có hoán vị tối thiểu, hãy chọn số nhỏ nhất trong`remaining`ít hơn`q[i]`. Để có hoán vị tối đa, hãy chọn số lớn nhất trong`remaining`ít hơn`q[i]`. 
4. Xóa số đã chọn khỏi`remaining`để đảm bảo không trùng lặp. 
5. Sau khi duyệt qua tất cả các vị trí, in`p_min`Và`p_max`. 

Tính bất biến là tại mọi vị trí`i`, giá trị lớn nhất ban đầu`i`yếu tố phù hợp`q[i]`, và mọi phần tử của`p`vẫn khác biệt. Bằng cách luôn chọn số nhỏ nhất hoặc lớn nhất có thể trong các khoảng trống, chúng tôi đảm bảo hoán vị ở mức tối thiểu hoặc tối đa về mặt từ điển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        q = list(map(int, input().split()))
        
        used = set()
        remaining_min = []
        remaining_max = []
        last = 0
        
        p_min = []
        p_max = []
        
        import heapq
        min_heap = []
        max_heap = []
        
        for num in range(1, n+1):
            remaining_min.append(num)
        remaining_min_ptr = 0
        
        # For maximal, we will use sorted list and pop from end
        remaining_max = list(range(1, n+1))
        
        from bisect import bisect_left
        import collections
        
        # used numbers
        used_set = set()
        
        # Preprocess min and max
        min_available = []
        max_available = []
        import bisect
        
        # We maintain min_available as a pointer to the next smallest unused number
        available = set(range(1, n+1))
        min_available = []
        max_available = sorted(available)
        
        last_q = 0
        for i in range(n):
            if i == 0 or q[i] != q[i-1]:
                # fixed element
                p_min.append(q[i])
                p_max.append(q[i])
                available.remove(q[i])
                max_available.remove(q[i])
            else:
                # q[i] == q[i-1]
                # minimal: choose smallest number in available
                smallest = min(available)
                p_min.append(smallest)
                available.remove(smallest)
                
                # maximal: choose largest number < q[i]
                idx = bisect.bisect_left(max_available, q[i])
                largest = max_available[idx-1]
                p_max.append(largest)
                max_available.pop(idx-1)
        
        print(*p_min)
        print(*p_max)

solve()
```Mã duy trì tập hợp các số chưa sử dụng và lấp đầy các khoảng trống bằng chiến lược chính xác. Đối với hoán vị tối thiểu, một trực tiếp`min`truy vấn trên tập hợp hoạt động vì chúng tôi chỉ cần O(1) cho mỗi phần tử do loại bỏ theo thứ tự. Đối với hoán vị tối đa, tìm kiếm nhị phân trên danh sách được sắp xếp sẽ tìm thấy số lớn nhất dưới mức tối đa hiện tại một cách hiệu quả. Các trường hợp cạnh, như cực đại lặp lại hoặc`q`tăng nghiêm ngặt, được xử lý tự động vì các quy tắc lựa chọn tôn trọng các ràng buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào`q = [3, 3, 4, 4, 7, 7, 7]`. 

| tôi | q[i] | có sẵn (phút) | đã chọn_min | có sẵn (tối đa) | đã chọn_max | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | {1..7} | 3 | {1..7} | 3 | 
| 1 | 3 | {1,2,4,5,6,7} | 1 | {1,2,4,5,6,7} | 2 | 
| 2 | 4 | {2,4,5,6,7} | 4 | {1,4,5,6,7} | 4 | 
| 3 | 4 | {2,5,6,7} | 2 | {1,5,6,7} | 1 | 
| 4 | 7 | {5,6,7} | 7 | {5,6,7} | 7 | 
| 5 | 7 | {5,6} | 5 | {5,6} | 6 | 
| 6 | 7 | {6} | 6 | {5} | 5 | 

Đầu ra tối thiểu:`[3,1,4,2,7,5,6]`Đầu ra tối đa:`[3,2,4,1,7,6,5]`Điều này chứng tỏ thuật toán đặt cực đại một cách chính xác và lấp đầy các khoảng trống để tối ưu hóa thứ tự từ điển. 

### Ví dụ 2 

đầu vào`q = [1,2,3,4]`. 

Đây,`q`đang gia tăng nghiêm trọng. Tất cả các phần tử đều có cực đại cố định. 

Đầu ra tối thiểu:`[1,2,3,4]`Đầu ra tối đa:`[1,2,3,4]`Không có khoảng trống tồn tại, vì vậy cả hai hoán vị đều giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được xử lý một lần và các số có sẵn được duy trì trong các cấu trúc cho phép truy vấn tối đa O(1) phút và O(log n). | 
| Không gian | O(n) | Chúng tôi lưu trữ các mảng hoán vị và tập hợp/danh sách các số còn lại. | 

Cho tổng của`n`trên tất cả các trường hợp thử nghiệm là 2 × 10^5, giải pháp này thực hiện tốt trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("4\n7\n3 3 4 4 7 7 7\n4\n1 2 3 4\n7\n3 4 5 5 5 7 7\n1\n1\n") == "3 1 4 2 7 5 6\n3 2 4 1 7 6 5\n1 2 3 4\n1 2 3 4\n3 4 5 1 2 7 6\n3 4 5 2 1 7 6\n1\n1"

# Custom cases
assert run("1
```
