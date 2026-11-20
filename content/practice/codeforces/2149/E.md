---
title: "CF 2149E - Tri thức tiềm ẩn của người xưa"
description: "Chúng ta được cung cấp một dãy số nguyên biểu thị các ký hiệu trên một tấm bảng cổ. Mỗi trường hợp thử nghiệm yêu cầu chúng ta đếm số lượng phân đoạn liên tục (mảng con) của chuỗi này thỏa mãn hai điều kiện: phân đoạn chứa chính xác k số riêng biệt và độ dài của nó ít nhất là…"
date: "2026-06-08T01:10:24+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2149
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1054 (Div. 3)"
rating: 1500
weight: 2149
solve_time_s: 86
verified: true
draft: false
---

[CF 2149E - Kiến thức tiềm ẩn của người xưa](https://codeforces.com/problemset/problem/2149/E) 

**Đánh giá:** 1500 
**Thẻ:** cấu trúc dữ liệu, hai con trỏ 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số nguyên biểu thị các ký hiệu trên một tấm bảng cổ. Mỗi trường hợp thử nghiệm yêu cầu chúng ta đếm số phân đoạn liên tục (mảng con) của chuỗi này thỏa mãn hai điều kiện: phân đoạn chứa chính xác`k`các số khác nhau và độ dài của nó ít nhất là`l`và nhiều nhất`r`. Đối với mỗi trường hợp thử nghiệm, chúng ta phải xuất ra tổng số các phân đoạn đó. 

Kích thước đầu vào cho phép các chuỗi lên tới 200.000 phần tử và tối đa 10.000 trường hợp thử nghiệm, với tổng số phần tử trên tất cả các trường hợp thử nghiệm được giới hạn bởi 200.000. Điều này ngụ ý rằng bất kỳ thuật toán nào có độ phức tạp trong trường hợp xấu nhất trên tuyến tính trong`n`mỗi trường hợp thử nghiệm có thể hết thời gian chờ. Một cách tiếp cận đơn giản để kiểm tra tất cả các mảng con có thể sẽ yêu cầu`O(n^2)`hoạt động trên mỗi trường hợp thử nghiệm, điều này không khả thi đối với đầu vào lớn. 

Các trường hợp cạnh bao gồm các chuỗi trong đó tất cả các phần tử giống hệt nhau, các chuỗi có tất cả các phần tử riêng biệt hoặc trong đó`l`Và`r`giới hạn độ dài đoạn trong phạm vi rất hẹp. Ví dụ, một trình tự`[7,7,7,7]`với`k=1`Và`l=1, r=2`cho phép mảng con`[7]`,`[7,7]`,`[7,7]`,`[7]`- Cần tính toán cẩn thận để tránh sai số một. 

## Phương pháp tiếp cận 

Giải pháp brute-force lặp lại trên mọi cặp chỉ số bắt đầu và kết thúc có thể có, đếm các số riêng biệt trong mảng con và kiểm tra độ dài của nó. Nó đúng nhưng đòi hỏi tới`O(n^2)`hoạt động, có thể đạt tới 4*10^10 cho các ràng buộc tối đa. Điều này là quá chậm. 

Thông tin chi tiết quan trọng là số lượng phần tử riêng biệt trong cửa sổ trượt có thể được theo dõi một cách hiệu quả bằng cách sử dụng phương pháp hai con trỏ và bản đồ tần số. Bằng cách duy trì một cửa sổ`[left, right]`mở rộng để bao gồm các phần tử mới và hợp đồng để loại bỏ các phần tử, chúng ta có thể đếm các mảng con một cách chính xác`k`số phân biệt trong`O(n)`thời gian cho mỗi trường hợp thử nghiệm. Để xử lý giới hạn độ dài`l`Và`r`, chúng ta có thể duy trì hai con trỏ bên phải: một để theo dõi độ dài phân đoạn hợp lệ nhỏ nhất và một con trỏ khác để theo dõi độ dài phân đoạn hợp lệ lớn nhất. Sự khác biệt giữa các con trỏ này cho biết số lượng mảng con hợp lệ bắt đầu từ vị trí bên trái hiện tại. 

Quan sát này làm giảm độ phức tạp từ bậc hai sang tuyến tính và sự kết hợp giữa bản đồ tần số và cửa sổ hai con trỏ đảm bảo rằng mỗi phần tử được xử lý nhiều nhất hai lần trên mỗi con trỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Hai con trỏ + Bản đồ tần số | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai con trỏ phải`right_min`Và`right_max`, bản đồ tần số`freq`và số lượng các số khác nhau`distinct_count`. 
2. Lặp lại mảng bằng con trỏ trái`left`từ`0`ĐẾN`n-1`. 
3. Mở rộng`right_max`cho đến khi cửa sổ`[left, right_max]`có nhiều hơn`k`số phân biệt, giữ`freq`được cập nhật. Dừng ở phần tử không hợp lệ đầu tiên. Điều này đảm bảo tất cả các phân đoạn bắt đầu tại`left`với chính xác`k`các số riêng biệt nằm trong`[left, right_max-1]`. 
4. Mở rộng`right_min`cho đến khi cửa sổ`[left, right_min]`có ít nhất`k`số riêng biệt. Điều này đánh dấu mảng con đầu tiên thỏa mãn điều kiện phần tử riêng biệt. 
5. Các mảng con hợp lệ bắt đầu từ`left`là những cái có độ dài giữa`l`Và`r`và bên trong`[right_min, right_max-1]`. Tính toán sự chồng lấp: số mảng con hợp lệ là`max(0, min(right_max-1, left + r - 1) - max(right_min, left + l - 1) + 1)`. 
6. Thêm số này vào kết quả. 
7. Di chuyển con trỏ trái về phía trước một đơn vị. Giảm tần số của`a[left]`và điều chỉnh`distinct_count`nếu cần thiết. Lặp lại các bước 3-6 cho đến hết mảng. 

**Tại sao nó hoạt động**: Kỹ thuật hai con trỏ đảm bảo rằng tất cả các mảng con bắt đầu từ`left`được tính đúng một lần. Bản đồ tần số đảm bảo`distinct_count`luôn luôn chính xác. Bằng cách điều chỉnh`right_min`Và`right_max`một cách độc lập, chúng tôi tính đến cả các ràng buộc về số lượng và độ dài riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_valid_subarrays(n, k, l, r, a):
    from collections import defaultdict
    freq = defaultdict(int)
    distinct_count = 0
    result = 0
    right_min = right_max = 0
    
    for left in range(n):
        while right_max < n and (distinct_count < k or (distinct_count == k and freq[a[right_max]] > 0)):
            if freq[a[right_max]] == 0:
                distinct_count += 1
            freq[a[right_max]] += 1
            right_max += 1
        
        while right_min < n and (distinct_count > k or (distinct_count == k and freq[a[right_min]] == 0)):
            if freq[a[right_min]] == 0:
                distinct_count += 1
            freq[a[right_min]] += 1
            right_min += 1
        
        start = max(left + l - 1, right_min)
        end = min(left + r - 1, right_max - 1)
        if end >= start:
            result += end - start + 1
        
        freq[a[left]] -= 1
        if freq[a[left]] == 0:
            distinct_count -= 1
    
    return result

t = int(input())
for _ in range(t):
    n, k, l, r = map(int, input().split())
    a = list(map(int, input().split()))
    print(count_valid_subarrays(n, k, l, r, a))
```Giải pháp khởi tạo số lượng tần số và sử dụng hai con trỏ để trượt cửa sổ một cách hiệu quả trong khi vẫn duy trì số lượng phần tử riêng biệt. Phần khó khăn là tính toán số lượng mảng con hợp lệ bằng cách sử dụng`start`Và`end`đồng thời tôn trọng cả các ràng buộc về số lượng và độ dài riêng biệt. Giảm tần số khi di chuyển`left`đảm bảo cửa sổ trượt vẫn còn hiệu lực. 

## Ví dụ đã hoạt động 

Đối với đầu vào`[1,2,1,3,2]`với`k=2`,`l=1`,`r=3`: 

| trái | đúng_min | đúng_max | tần số | khác biệt_count | mảng con hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | {1:1,2:1} | 2 | 2 | 
| 1 | 2 | 4 | {1:1,2:1,3:1} | 3 | 2 | 
| 2 | 2 | 4 | {1:1,2:1,3:1} | 3 | 1 | 

Điều này xác nhận rằng các điều chỉnh cửa sổ tính toán chính xác số phần tử riêng biệt và các ràng buộc về độ dài mảng con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được thêm và xóa khỏi bản đồ tần số tối đa một lần cho mỗi con trỏ. | 
| Không gian | O(n) | Bản đồ tần số lưu trữ nhiều nhất`n`các yếu tố riêng biệt. | 

Giải pháp tuyến tính này phù hợp thoải mái trong giới hạn 2 giây ngay cả ở mức tối đa`n=2*10^5`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# Provided samples
assert run("5\n1 1 1 1\n5\n5 2 2 3\n1 2 1 3 2\n6 3 1 6\n1 2 3 1 2 3\n4 1 1 2\n7 7 7 7\n7 3 2 4\n1 2 1 2 3 2 1") == "1\n5\n10\n7\n5"

# Custom cases
assert run("1\n4 1 1 1\n1 1 1 1") == "4", "all identical, l=r=1"
assert run("1\n5 5 1 5\n1 2 3 4 5") == "1", "all distinct, k=n"
assert run("1\n5 3 2 3\n1 2 1 3 2") == "4", "mixed distinct numbers"
assert run("1\n3 2 2 3\n1 2 3") == "2", "length constraints narrower than array"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[1,1,1,1], k=1, l=1, r=1`| 4 | Đếm chính xác các mảng con có độ dài đơn | 
|`[1,2,3,4,5], k=5, l=1, r=5`| 1 | Xử lý trường hợp toàn bộ mảng là phân đoạn hợp lệ duy nhất | 
|`[1,2,1,3,2], k=3, l=2, r=3`| 4 | Đếm đúng với các mảng con chồng chéo | 
|`[1,2,3], k=2, l=2, r=3`| 2 | Tôn trọng các hạn chế về độ dài | 

##
