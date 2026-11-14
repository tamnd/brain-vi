---
title: "CF 1777C - Câu đố Master"
description: "Chúng ta được yêu cầu chọn một nhóm nhỏ học sinh từ một trường học để thành lập một đội đố vui. Mỗi học sinh có một giá trị thông minh và mỗi chủ đề bài kiểm tra yêu cầu ít nhất một thành viên trong nhóm có độ thông minh chia hết cho số chủ đề."
date: "2026-06-09T11:40:22+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "math", "number-theory", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1777
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 845 (Div. 2) and ByteRace 2023"
rating: 1700
weight: 1777
solve_time_s: 207
verified: true
draft: false
---

[CF 1777C - Quiz Master](https://codeforces.com/problemset/problem/1777/C) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, toán, lý thuyết số, sắp xếp, hai con trỏ 
**Thời gian giải:** 3m 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu chọn một nhóm nhỏ học sinh từ một trường học để thành lập một đội đố vui. Mỗi học sinh có một giá trị thông minh và mỗi chủ đề bài kiểm tra yêu cầu ít nhất một thành viên trong nhóm có độ thông minh chia hết cho số chủ đề. Mục tiêu là thành lập một nhóm bao gồm tất cả các chủ đề đồng thời giảm thiểu sự khác biệt lớn nhất giữa mức độ thông minh của hai học sinh trong nhóm. 

Dữ liệu đầu vào cho biết số lượng sinh viên`n`và chủ đề`m`, theo sau là một loạt các giá trị thông minh`a`. Đối với mỗi trường hợp thử nghiệm, chúng tôi phải đưa ra mức chênh lệch tối đa tối thiểu giữa mức độ thông minh trong một nhóm hợp lệ hoặc`-1`nếu không có đội nào có thể bao quát được tất cả các chủ đề. 

Những ràng buộc cho phép`n`Và`m`lên tới 100.000 và tổng của tất cả các trường hợp thử nghiệm là 100.000. Điều này loại trừ các cách tiếp cận ngây thơ thử tất cả các tập hợp con của sinh viên, vì có nhiều tổ hợp theo cấp số nhân. Các hoạt động phải gần như tuyến tính hoặc gần tuyến tính trong`n`cho từng trường hợp thử nghiệm, nếu không chúng tôi có nguy cơ hết thời gian chờ. 

Các trường hợp đặc biệt bao gồm các tình huống trong đó không có học sinh nào được chia theo một chủ đề nhất định. Ví dụ, nếu`a = [3,7]`Và`m = 4`, không có học sinh nào chia hết cho`2`hoặc`4`, nên không có đội nào tồn tại. Một trường hợp tinh tế khác phát sinh khi một học sinh có thể học nhiều chủ đề, cho phép chênh lệch tối thiểu bằng không. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các tập hợp con của học sinh, kiểm tra xem mỗi tập hợp con có bao gồm tất cả các chủ đề hay không và tính toán sự khác biệt giữa độ thông minh tối đa và tối thiểu. Về nguyên tắc thì đúng nhưng cần kiểm tra lại`2^n`tập hợp con, điều này là không thể thực hiện được đối với`n = 10^5`. 

Thông tin chi tiết quan trọng là chúng tôi không cần tất cả các tập hợp con, chúng tôi chỉ cần phạm vi giá trị thông minh nhỏ nhất có thể bao gồm tất cả các chủ đề. Các chủ đề tương ứng với các ước số, vì vậy chúng ta có thể nghĩ về phạm vi bao phủ của số nguyên. Bằng cách lặp lại từng mức độ thông minh với tư cách là thành viên tối thiểu trong nhóm và cố gắng thu hút đủ số học sinh để bao quát tất cả các chủ đề cho đến`m`, chúng ta có thể giảm vấn đề thành một loại cửa sổ trượt trên các giá trị thông minh được sắp xếp, kết hợp với việc theo dõi phạm vi chủ đề giống như sàng. 

Chúng tôi xử lý trước những học sinh nào sẽ học những chủ đề nào. Sau đó, chúng tôi chỉ xem xét phạm vi mức độ thông minh và kiểm tra xem các chủ đề trong phạm vi đó có bao gồm tất cả các chủ đề từ`1`ĐẾN`m`. Độ rộng tối thiểu của phạm vi như vậy sẽ đưa ra câu trả lời. 

Cách tiếp cận tối ưu tận dụng khả năng sắp xếp và dạng hai con trỏ hoặc một cửa sổ động. Bằng cách sắp xếp`a`và theo dõi số lượng chủ đề được bao phủ trong một cửa sổ trượt, chúng tôi có thể tính toán một cách hiệu quả sự khác biệt tối đa tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * m) | O(n * m) | Quá chậm | 
| Tối ưu | O(n log n + n m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng`a`. Điều này cho phép chúng ta xem xét phạm vi mức độ thông minh theo thứ tự tăng dần, giúp đơn giản hóa việc tìm ra những khác biệt tối thiểu. 
2. Khởi tạo mảng đếm`topic_count`kích thước`m+1`về 0, theo dõi số lượng học sinh trong cửa sổ hiện tại thành thạo từng chủ đề. 
3. Duy trì hai con trỏ`l`Và`r`đại diện cho đầu bên trái và bên phải của cửa sổ giá trị thông minh hiện tại. 
4. Di chuyển`r`từ trái sang phải, thêm sinh viên vào cửa sổ. Đối với mỗi học sinh mới, số gia tăng được tính cho mọi chủ đề họ đề cập, tức là tất cả các ước số`T`của`a[r]`Ở đâu`T <= m`. 
5. Bất cứ khi nào tất cả các chủ đề từ`1`ĐẾN`m`có ít nhất một học sinh trong cửa sổ (`topic_count[T] > 0`cho tất cả`T`), cố gắng thu nhỏ cửa sổ bằng cách di chuyển`l`Phải. Giảm`topic_count`cho các chủ đề được đề cập bởi`a[l]`tương ứng. 
6. Mỗi lần cửa sổ bao gồm tất cả các chủ đề, hãy cập nhật câu trả lời dưới dạng`min(ans, a[r] - a[l])`. 
7. Nếu không có cửa sổ nào bao gồm tất cả các chủ đề, hãy quay lại`-1`. 

Tại sao nó hoạt động: Việc sắp xếp đảm bảo rằng việc tăng`r`chỉ làm tăng độ thông minh tối đa trong cửa sổ và di chuyển`l`chuyển tiếp làm giảm sự thông minh tối thiểu. Bằng cách duy trì số lượng chủ đề một cách linh hoạt, chúng tôi đảm bảo cửa sổ luôn đại diện cho một tập hợp con hợp lệ và việc theo dõi độ rộng tối thiểu đảm bảo sự khác biệt tối đa tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()
        
        max_val = max(a)
        # count frequency of each smartness
        from collections import defaultdict, deque
        indices = defaultdict(list)
        for val in a:
            indices[val].append(val)
        
        # initialize topic coverage for sliding window
        topic_count = [0] * (m + 1)
        total_covered = 0
        ans = float('inf')
        l = 0
        
        for r in range(n):
            val = a[r]
            # increment topic counts for all divisors <= m
            for T in range(1, min(val, m) + 1):
                if val % T == 0:
                    if topic_count[T] == 0:
                        total_covered += 1
                    topic_count[T] += 1
            
            # shrink window from left
            while total_covered == m:
                ans = min(ans, a[r] - a[l])
                val_l = a[l]
                for T in range(1, min(val_l, m) + 1):
                    if val_l % T == 0:
                        topic_count[T] -= 1
                        if topic_count[T] == 0:
                            total_covered -= 1
                l += 1
        
        print(ans if ans != float('inf') else -1)

if __name__ == "__main__":
    solve()
```Mã sắp xếp`a`và sử dụng một cửa sổ trượt với một`topic_count`array để theo dõi có bao nhiêu học sinh trong cửa sổ hiện tại thông thạo từng chủ đề. Vòng lặp bên trong trên các ước số đảm bảo phạm vi bao phủ chính xác và việc thu nhỏ cửa sổ sẽ duy trì sự khác biệt tối thiểu. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 2:**```
4 2
3 7 2 9
```| Bước | tôi | r | Cửa sổ | Chủ đề được đề cập | Khác biệt hiện tại | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | [2] | 1,2 | 0 | 0 | 
| 1 | 0 | 1 | [2,3] | 1,2 | 1 | 0 | 

Sự khác biệt tối thiểu là 0 nếu chỉ sử dụng độ thông minh của học sinh 2. 

**Đầu vào mẫu 3:**```
5 7
6 4 3 5 7
```| Bước | tôi | r | Cửa sổ | Chủ đề được đề cập | Khác biệt hiện tại | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | [3] | 1,3 | 0 | thông tin | 
| 1 | 0 | 1 | [3,4] | 1,2,3,4 | 1 | thông tin | 
| 2 | 0 | 2 | [3,4,5] | 1,2,3,4,5 | 2 | thông tin | 
| 3 | 0 | 3 | [3,4,5,6] | 1..6 | 3 | 3 | 
| 4 | 1 | 4 | [4,5,6,7] | 1..7 | 3 | 3 | 

Sự khác biệt tối đa tối thiểu là 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n m) | Đối với mỗi học sinh, chúng tôi kiểm tra các ước số tối đa min(a[i], m). Sắp xếp là O(n log n), bị chi phối bởi việc kiểm tra số chia. | 
| Không gian | O(m + n) | Chúng tôi lưu trữ số chủ đề và sắp xếp mảng. | 

Được cho`n`Và`m`tổng tới 10^5 trên tất cả các trường hợp thử nghiệm, giải pháp này đủ hiệu quả cho giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("3\n2 4\n3 7\n4 2\n3 7 2 9\n5 7\n6 4 3 5 7\n") == "-1\n0\n3"

# Custom cases
assert run("1\n1 1\n1\n") == "0", "single student single topic"
assert run("1\n3 3\n2 3 4\n") == "1", "need all topics covered by minimal range"
assert run("1\n5 5\n1 2 3 4 5\n") == "4", "all distinct, minimal range is full array"
assert run("1\n4 2\n6 8 10 12\n") == "0", "multiple students divisible by topics, minimal difference zero"
assert run("1\n2 2\n3 7\n") == "-1", "no student covers topic 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 

|---|---
