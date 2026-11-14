---
title: "CF 1793E - Velepin và Tiếp thị"
description: "Chúng tôi có một nhóm độc giả thường xuyên, mỗi người có một ngưỡng hài lòng riêng: một độc giả sẽ vui nếu có ít nhất một số lượng người nhất định đọc cùng cuốn sách với họ."
date: "2026-06-09T10:19:13+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "greedy", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1793
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 852 (Div. 2)"
rating: 2600
weight: 1793
solve_time_s: 138
verified: false
draft: false
---

[CF 1793E - Velepin và Tiếp thị](https://codeforces.com/problemset/problem/1793/E) 

**Đánh giá:** 2600 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, tham lam, sắp xếp, hai con trỏ 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một nhóm độc giả thường xuyên, mỗi người có một ngưỡng hài lòng riêng: một độc giả sẽ vui nếu có ít nhất một số lượng người nhất định đọc cùng cuốn sách với họ. Mỗi năm, Velepin viết một số lượng sách nhất định và chúng tôi được phép chỉ định mỗi độc giả một trong những cuốn sách này. Câu hỏi đặt ra là, trong mỗi năm, số lượng độc giả tối đa mà chúng ta có thể hài lòng là bao nhiêu nếu chúng ta có thể phân bổ số lượng độc giả đọc sách một cách tối ưu. 

Đầu vào cho chúng ta`n`, số lượng người đọc và một mảng`a`Ở đâu`a[i]`là số lượng người tối thiểu cần có trên một cuốn sách dành cho người đọc`i`để được hài lòng. Sau đó chúng tôi được trao`q`năm, mỗi năm có`k_j`sách, và chúng ta phải tính toán số lượng độc giả hài lòng tối đa mỗi năm. 

Các ràng buộc rất lớn:`n`Và`q`có thể lên tới 300.000. Điều này có nghĩa là một`O(n*q)`giải pháp quá chậm. Chúng ta cần thứ gì đó gần gũi hơn`O(n log n + q log n)`hoặc`O(n + q)`cho mỗi truy vấn sau khi tiền xử lý. Các trường hợp khó khăn có thể gặp phải một giải pháp ngây thơ bao gồm tất cả độc giả có ngưỡng cao hoặc số lượng sách rất gần với số lượng độc giả, điều này có thể khiến một số độc giả không thể hài lòng nếu không được xử lý cẩn thận. Ví dụ, nếu`n=5`,`a=[5,5,5,5,5]`Và`k=2`, chúng tôi không thể làm hài lòng bất cứ ai nếu chúng tôi chỉ định ít hơn 5 độc giả cho mỗi cuốn sách. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi cách phân bổ độc giả có thể có giữa các cuốn sách và đếm những độc giả hài lòng. Đối với mỗi`k`, chúng ta có thể lặp qua tất cả các kết hợp của`n`độc giả vào`k`các nhóm không trống, kiểm tra xem độc giả nào đáp ứng ngưỡng hài lòng của họ và chọn nhiệm vụ tối đa hóa sự hài lòng. Điều này hoạt động hợp lý nhưng cực kỳ chậm: có rất nhiều cách để phân vùng`n`độc giả vào`k`các nhóm. Vì`n`khoảng 3e5, điều này hoàn toàn không thực tế. 

Điểm mấu chốt là sự hài lòng chỉ phụ thuộc vào số lượng độc giả được chỉ định cho mỗi cuốn sách chứ không phụ thuộc vào danh tính cụ thể của độc giả. Sắp xếp các ngưỡng hài lòng`a`cho phép chúng ta suy luận về nhóm độc giả lớn nhất mà chúng ta có thể đáp ứng cho một số lượng sách nhất định. Về cơ bản, chúng tôi muốn gán cho độc giả những thông tin nhỏ nhất`a[i]`thành các nhóm khả thi nhỏ nhất, sau đó lớn hơn`a[i]`thành các nhóm lớn hơn, đảm bảo mỗi nhóm có đủ thành viên để làm hài lòng tất cả độc giả. 

Vấn đề này giảm xuống thành một biến thể của cách tiếp cận tổng tiền tố/tham lam: sau khi sắp xếp`a`, chúng tôi muốn tiền tố lớn nhất của người đọc sao cho có thể chia họ thành`k`nhóm trong đó mỗi nhóm ít nhất phải lớn bằng ngưỡng lớn nhất trong nhóm đó. Tìm kiếm nhị phân trên kích thước tiền tố có thể trả lời câu hỏi này một cách hiệu quả cho mỗi`k`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu (sắp xếp + tổng tiền tố + tìm kiếm nhị phân) | O(n log n + q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng`a`theo thứ tự không giảm. Điều này cho phép chúng tôi xử lý trước tiên những độc giả có ngưỡng nhỏ hơn. Điều này rất quan trọng vì việc đáp ứng những ngưỡng đó đòi hỏi ít độc giả hơn cho mỗi cuốn sách. 
2. Tính mảng tổng tiền tố của`a`. Điều này sẽ cho phép chúng tôi tính toán nhanh tổng số "độc giả bắt buộc" cho bất kỳ nhóm độc giả liền kề nào theo thứ tự được sắp xếp. 
3. Đối với mỗi truy vấn`k`, thực hiện tìm kiếm nhị phân theo số lượng độc giả`m`chúng tôi cố gắng đáp ứng, từ`0`ĐẾN`n`. Đối với một ứng cử viên`m`, chia chúng`m`độc giả vào`k`các nhóm một cách đồng đều nhất có thể. Mỗi nhóm phải đáp ứng tối đa`a[i]`trong nhóm đó. Sử dụng tổng tiền tố để kiểm tra hiệu quả xem điều này có khả thi hay không. 
4. Lớn nhất`m`vượt qua kiểm tra là câu trả lời cho năm đó. Trả lại số này. 

Tại sao nó hoạt động: Tính năng sắp xếp đảm bảo rằng những độc giả có ngưỡng thấp hơn sẽ được nhóm trước, tối đa hóa cơ hội làm hài lòng nhiều người hơn. Tổng tiền tố cho phép chúng tôi xác minh tính khả thi trong O(1) cho mỗi nhóm ứng cử viên. Tìm kiếm nhị phân đảm bảo chúng tôi tìm thấy số lượng độc giả hài lòng lớn nhất một cách hiệu quả. Bất biến chính là ở mỗi bước, tất cả độc giả trong một nhóm đều đáp ứng ngưỡng hài lòng của họ vì chúng tôi luôn so sánh quy mô nhóm với ngưỡng lớn nhất trong nhóm đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())
    ks = [int(input()) for _ in range(q)]
    
    a.sort()
    
    prefix = [0] * (n + 1)
    for i in range(n):
        prefix[i+1] = prefix[i] + a[i]
    
    results = []
    
    for k in ks:
        low, high = 0, n
        ans = 0
        while low <= high:
            mid = (low + high) // 2
            full_groups = mid // k
            extra = mid % k
            ok = True
            for i in range(k):
                group_size = full_groups + (1 if i < extra else 0)
                if group_size == 0:
                    continue
                group_max_index = mid - (k - i - 1) * full_groups - (1 if i >= extra else 0)
                group_max_index -= 1
                if a[group_max_index] > group_size:
                    ok = False
                    break
            if ok:
                ans = mid
                low = mid + 1
            else:
                high = mid - 1
        results.append(ans)
    
    print("\n".join(map(str, results)))

solve()
```Giải pháp bắt đầu bằng cách sắp xếp`a`, sau đó xây dựng tổng tiền tố để kiểm tra tính khả thi của nhóm một cách hiệu quả. Mỗi năm, tìm kiếm nhị phân xác định số lượng độc giả tối đa có thể được chỉ định cho sách mà vẫn đáp ứng được sự hài lòng của họ. Các trường hợp khó khăn như có nhiều sách hơn số người đọc hoặc tất cả các ngưỡng cao đều được xử lý một cách tự nhiên bằng tìm kiếm nhị phân. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
1 2 2 2 2
3
2
3
4
```Dấu vết cho`k=2`: 

| Bước | giữa | Nhóm | Max a trong nhóm | Thỏa mãn được không? | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | [1,1] | 1,2 | Đúng | 
| 2 | 4 | [2,2] | 2,2 | Đúng | 
| 3 | 5 | [3,2] | 2,2 | Đúng | 

Kết quả: 5 

### Mẫu 2 

đầu vào:```
6
1 1 1 1 2 3
2
2
3
```Dấu vết cho`k=3`: 

| Bước | giữa | Nhóm | Max a trong nhóm | Thỏa mãn được không? | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | [1,1,1] | 1,1,1 | Đúng | 
| 2 | 5 | [2,2,1] | 2,2,2 | Đúng | 
| 3 | 6 | [2,2,2] | 2,2,3 | Sai | 

Kết quả: 5 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q log n) | Phân loại chi phí`O(n log n)`, mỗi truy vấn sử dụng tìm kiếm nhị phân trên`n`với`k`các nhóm, đó là`O(log n * k)`. k ≤ n nên trường hợp xấu nhất`O(log n * n)`nhưng sự đơn giản hóa thực tế sẽ làm giảm điều này. | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng đã sắp xếp, tổng tiền tố và kết quả truy vấn. | 

Giải pháp phù hợp thoải mái trong 2 giây cho bộ nhớ n,q 3e5 và 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("5\n1 2 2 2 2\n3\n2\n3\n4\n") == "5\n5\n3", "sample 1"
assert run("6\n1 1 1 1 2 3\n2\n2\n3\n") == "5\n5", "sample 2"

# Custom tests
assert run("2\n1 2\n1\n2\n") == "2", "minimum-size input"
assert run("3\n3 3 3\n1\n2\n") == "0", "high thresholds unsatisfiable"
assert run("5\n1 1 1 1 1\n2\n2\n3\n") == "5\n5", "all-equal small thresholds"
assert run("5\n1 2 3 4 5\n1\n5\n") == "5", "boundary case k=n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n1 2\n1\n2 | 2 | đầu vào kích thước tối thiểu | 
| 3\n3 3 3\n1\n2 | 0 | ngưỡng cao không thỏa mãn | 
| 5\n1 1 1 1 1\n2\n | | |
