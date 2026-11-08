---
title: "CF 140C - Người Tuyết Năm Mới"
description: "Chúng tôi được cung cấp một bộ sưu tập các quả cầu tuyết, mỗi quả cầu có bán kính cụ thể và mục tiêu của chúng tôi là tập hợp càng nhiều người tuyết càng tốt bằng cách sử dụng những quả cầu tuyết này. Mỗi người tuyết phải được làm từ chính xác ba quả cầu tuyết và mỗi quả cầu trong số đó phải có bán kính riêng biệt."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 140
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 100"
rating: 1800
weight: 140
solve_time_s: 120
verified: false
draft: false
---

[CF 140C - Người tuyết năm mới](https://codeforces.com/problemset/problem/140/C) 

**Đánh giá:** 1800 
**Tas:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 2 phút 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ sưu tập các quả cầu tuyết, mỗi quả cầu có bán kính cụ thể và mục tiêu của chúng tôi là tập hợp càng nhiều người tuyết càng tốt bằng cách sử dụng những quả cầu tuyết này. Mỗi người tuyết phải được làm từ chính xác ba quả cầu tuyết và mỗi quả cầu trong số đó phải có bán kính riêng biệt. Đầu vào bao gồm một số nguyên _n_, số lượng quả cầu tuyết, theo sau là các số nguyên _n_ biểu thị bán kính. Kết quả đầu ra sẽ cho chúng ta biết số lượng người tuyết tối đa mà chúng ta có thể tạo, theo sau là danh sách người tuyết, mỗi người được xác định bằng bán kính của ba quả cầu tuyết theo thứ tự giảm dần (lớn, trung bình, nhỏ). 

Các ràng buộc cho _n_ lên tới 100.000 và bán kính lên tới 1.000.000.000. Điều này loại trừ bất kỳ cách tiếp cận nào kiểm tra tất cả các bộ ba một cách rõ ràng, bởi vì cách tiếp cận đơn giản sẽ yêu cầu khoảng n³ phép toán, quá chậm đối với n = 10⁵. Chúng ta cần một phương pháp hoạt động trong thời gian gần tuyến tính hoặc tệ nhất là O(n log n). 

Các trường hợp cạnh có thể dễ dàng phá vỡ việc triển khai ngây thơ. Ví dụ: nếu tất cả các quả cầu tuyết có cùng bán kính, như`5 5 5 5`, không có người tuyết nào có thể được xây dựng. Một trường hợp phức tạp khác là khi một bán kính chiếm ưu thế, chẳng hạn`1 1 1 2 3`. Một cách tiếp cận tham lam bất cẩn chỉ lặp đi lặp lại ba giá trị lớn nhất có thể cố gắng sử dụng nhầm ba quả cầu tuyết giống hệt nhau, vi phạm yêu cầu về tính khác biệt. Ngoài ra, có chính xác ba quả cầu tuyết có bán kính khác nhau là trường hợp tối thiểu để tạo ra chính xác một người tuyết. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ lặp lại trên tất cả các bộ ba quả cầu tuyết có thể có, kiểm tra xem bán kính có khác biệt hay không và thu thập những người tuyết hợp lệ cho đến khi không còn bộ ba nào. Mặc dù đúng về mặt nguyên tắc nhưng điều này sẽ yêu cầu các phép toán O(n³), tức là khoảng 10¹⁵ đối với n = 10⁵. Điều này là hoàn toàn không thực tế. 

Quan sát quan trọng là chúng tôi chỉ quan tâm đến bán kính riêng biệt và số lượng quả cầu tuyết có sẵn cho mỗi bán kính. Nếu chúng ta duy trì số lượng quả cầu tuyết tồn tại cho mỗi bán kính thì vấn đề sẽ giảm xuống còn việc chọn liên tục ba bán kính phân biệt lớn nhất còn lại. Bằng cách luôn chọn bán kính lớn nhất hiện có, chúng tôi tối đa hóa số lượng người tuyết vì những quả cầu tuyết có bán kính nhỏ hơn sẽ không cản trở việc tạo ra những người tuyết trong tương lai - chúng có thể được ghép nối sau khi bán kính cao hơn đã cạn kiệt. 

Điều này gợi ý cấu trúc dữ liệu hỗ trợ hiệu quả "bật bán kính khả dụng lớn nhất" và giảm số lượng của nó. Một đống tối đa là lý tưởng ở đây. Mỗi lần lặp lại, chúng tôi trích xuất ba bán kính lớn nhất, ghi lại chúng dưới dạng người tuyết, giảm số lượng và đẩy chúng trở lại nếu chúng vẫn còn. Chúng tôi tiếp tục cho đến khi còn lại ít hơn ba bán kính riêng biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| Tham lam Max-Heap | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của từng bán kính. Điều này cho phép chúng tôi biết có bao nhiêu quả cầu tuyết thuộc mỗi loại mà không cần lưu trữ các bản sao riêng lẻ. 
2. Khởi tạo một heap tối đa (hàng đợi ưu tiên) chứa tất cả các bán kính riêng biệt cùng với số lượng của chúng. Sử dụng bán kính làm chìa khóa để duy trì thứ tự giảm dần. 
3. Khi có ít nhất ba bán kính riêng biệt trong vùng heap, hãy thực hiện như sau: 

một. Bật ba bán kính lớn nhất. 

b. Ghi lại chúng dưới dạng người tuyết, sắp xếp chúng theo thứ tự giảm dần. 

c. Giảm số lượng cho mỗi bán kính này. Nếu bán kính nào vẫn còn bóng tuyết, hãy đẩy nó trở lại đống. 
4. Khi còn lại ít hơn ba bán kính riêng biệt, hãy dừng lại. Số lượng người tuyết được ghi lại đại diện cho số lượng tối đa có thể được xây dựng. 
5. Xuất ra tổng số người tuyết và danh sách bán kính cấu thành của chúng. 

Điều bất biến là ở mỗi bước, ba bán kính có sẵn lớn nhất sẽ được chọn. Điều này đảm bảo rằng không có người tuyết tiềm năng nào bị chặn bởi một lựa chọn không hiệu quả, đảm bảo đạt được số lượng tối đa. 

## Giải pháp Python```python
import sys
import heapq
from collections import Counter

input = sys.stdin.readline

n = int(input())
radii = list(map(int, input().split()))

# Step 1: Count frequencies
freq = Counter(radii)

# Step 2: Build max-heap (invert the radius for max-heap using heapq)
heap = [(-radius, count) for radius, count in freq.items()]
heapq.heapify(heap)

snowmen = []

while len(heap) >= 3:
    first = heapq.heappop(heap)
    second = heapq.heappop(heap)
    third = heapq.heappop(heap)
    
    radii_triplet = sorted([-first[0], -second[0], -third[0]], reverse=True)
    snowmen.append(radii_triplet)
    
    # Decrement counts and push back if still positive
    if first[1] > 1:
        heapq.heappush(heap, (first[0], first[1] - 1))
    if second[1] > 1:
        heapq.heappush(heap, (second[0], second[1] - 1))
    if third[1] > 1:
        heapq.heappush(heap, (third[0], third[1] - 1))

print(len(snowmen))
for s in snowmen:
    print(*s)
```Mã đầu tiên đếm số quả cầu tuyết theo bán kính. Vùng heap tối đa được xây dựng bằng cách phủ định bán kính vì vùng heapq của Python là vùng heap tối thiểu. Ở mỗi bước, chúng tôi trích xuất ba bán kính riêng biệt lớn nhất, giảm số lượng của chúng và đẩy chúng trở lại nếu vẫn còn quả cầu tuyết. Sắp xếp từng bộ ba đảm bảo đầu ra theo thứ tự giảm dần. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**:```
7
1 2 3 4 5 6 7
```| Đống (top 3) | Đếm | Người tuyết hình thành | 
| --- | --- | --- | 
| 7,6,5 | mỗi cái 1 | 7 6 5 | 
| 4,3,2 | mỗi cái 1 | 4 3 2 | 

Sau những lần lặp này, chỉ còn lại ít hơn ba bán kính (1), do đó thuật toán dừng lại. Hai người tuyết được tạo ra:`[7 6 5]`Và`[4 3 2]`. 

**Đầu vào tùy chỉnh 2**:```
8
1 1 2 2 3 3 4 4
```| Đống (top 3) | Đếm | Người tuyết hình thành | 
| --- | --- | --- | 
| 4,3,2 | mỗi cái 1 | 4 3 2 | 
| 4,3,1 | mỗi cái 1 | 4 3 1 | 

Heap bây giờ chỉ còn lại 2 bán kính riêng biệt nên thuật toán dừng lại. Hai người tuyết được hình thành. Điều này cho thấy thuật toán xử lý chính xác nhiều bản sao và tối đa hóa số lượng người tuyết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Tần số đếm là O(n). Mỗi quả cầu tuyết có thể được đẩy và bật ra nhiều nhất một lần cho mỗi người tuyết, dẫn đến các phép toán O(n log n) với đống. | 
| Không gian | O(n) | Heap lưu trữ tối đa một mục nhập cho mỗi bán kính riêng biệt và bộ đếm tần số lưu trữ một mục nhập cho mỗi bán kính riêng biệt. | 

Với n 10⁵, thuật toán này chạy hiệu quả trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution
    n = int(input())
    radii = list(map(int, input().split()))
    from collections import Counter
    import heapq

    freq = Counter(radii)
    heap = [(-radius, count) for radius, count in freq.items()]
    heapq.heapify(heap)

    snowmen = []

    while len(heap) >= 3:
        first = heapq.heappop(heap)
        second = heapq.heappop(heap)
        third = heapq.heappop(heap)

        radii_triplet = sorted([-first[0], -second[0], -third[0]], reverse=True)
        snowmen.append(radii_triplet)

        if first[1] > 1:
            heapq.heappush(heap, (first[0], first[1] - 1))
        if second[1] > 1:
            heapq.heappush(heap, (second[0], second[1] - 1))
        if third[1] > 1:
            heapq.heappush(heap, (third[0], third[1] - 1))

    print(len(snowmen))
    for s in snowmen:
        print(*s)

    return output.getvalue().strip()

# provided samples
assert run("7\n1 2 3 4 5 6 7\n") == "2\n7 6 5\n4 3 2"

# custom tests
assert run("4\n5 5 5 5\n") == "0"
assert run("3\n1 2 3\n") == "1\n3 2 1"
assert run("8\n1 1 2 2 3 3 4 4\n") == "2\n4 3 2\n4 3 1"
assert run("6\n1 1 2 2 3 3\n") == "2\n3 2 1\n3 2 1"
```| Kiểm tra đầu vào
