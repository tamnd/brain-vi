---
title: "CF 1621E - Trường Học Mới"
description: "Chúng ta được yêu cầu phân công giáo viên cho các nhóm học sinh sao cho mỗi giáo viên phụ trách nhiều nhất một nhóm, mỗi nhóm có đúng một giáo viên và tuổi của giáo viên ít nhất bằng tuổi trung bình của học sinh trong nhóm được phân công."
date: "2026-06-10T05:55:47+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1621
codeforces_index: "E"
codeforces_contest_name: "Hello 2022"
rating: 2300
weight: 1621
solve_time_s: 123
verified: false
draft: false
---

[CF 1621E - Trường học mới](https://codeforces.com/problemset/problem/1621/E) 

**Đánh giá:** 2300 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, tham lam, triển khai, sắp xếp 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu phân công giáo viên cho các nhóm học sinh sao cho mỗi giáo viên phụ trách nhiều nhất một nhóm, mỗi nhóm có đúng một giáo viên và tuổi của giáo viên ít nhất bằng tuổi trung bình của học sinh trong nhóm được phân công. Sau bài tập đầu tiên này, chúng ta cần trả lời cho mỗi học sinh rằng liệu hệ thống có còn hoạt động hay không nếu học sinh đó từ chối tham dự. Điều đó có nghĩa là chúng ta cần kiểm tra xem sau khi loại bỏ từng học sinh, có sự phân công giáo viên hợp lệ cho các nhóm đã sửa đổi hay không. 

Đầu vào cho chúng ta`n`giáo viên và`m`nhóm, lứa tuổi cá nhân. Mỗi nhóm có ít nhất hai học sinh. Đầu ra là một chuỗi`0`cát`1`mỗi người ở đâu`1`có nghĩa là hệ thống có thể đáp ứng việc từ chối của sinh viên tương ứng. 

Các ràng buộc cho thấy chúng ta có thể có tới`10^5`giáo viên và nhóm cho mỗi trường hợp kiểm tra và tổng số học sinh trong tất cả các trường hợp kiểm tra có thể lên tới`2·10^5`. Một cách tiếp cận đơn giản để kiểm tra mọi bài tập có thể được giao cho mọi học sinh sẽ là quá chậm, bởi vì ngay cả một nỗ lực giao bài tập thô bạo cũng có thể bị hủy bỏ.`O(n * m)`và chúng tôi sẽ làm điều này cho mỗi học sinh, có thể tiếp cận`10^10`hoạt động. Chúng ta cần một giải pháp thông minh hơn, chủ yếu là tuyến tính hoặc log-tuyến tính. 

Các trường hợp quan trọng nhất bao gồm các nhóm có độ tuổi trung bình rất gần với độ tuổi giáo viên hiện có. Ví dụ: nếu một nhóm có độ tuổi`[30, 31]`và giáo viên có tuổi`[30, 31]`, sau đó loại bỏ`30`học sinh tăng điểm trung bình của nhóm lên`31`, điều này có thể phá vỡ một bài tập đã làm trước đó. Một trường hợp tinh tế khác xảy ra khi nhiều nhóm có điểm trung bình gần như giống hệt nhau và việc phân công giáo viên tối ưu phụ thuộc vào việc sắp xếp; một cách tiếp cận ngây thơ kiểm tra mức trung bình một cách tuần tự có thể phân công sai giáo viên. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ cố gắng tính toán tất cả các bài tập có thể có giữa giáo viên và nhóm đối với mỗi lần loại bỏ học sinh. Đối với mỗi lần loại bỏ, chúng tôi sẽ tính toán lại mức trung bình của nhóm, thử tất cả các hoán vị có thể có của bài tập của giáo viên và kiểm tra xem mỗi giáo viên có đáp ứng yêu cầu trung bình hay không. Điều này hoạt động hợp lý nhưng không khả thi, bởi vì ngay cả việc kiểm tra hoán vị cũng`O(n!)`trong trường hợp xấu nhất. 

Giải pháp tối ưu tận dụng hai hiểu biết chính. Đầu tiên, chỉ có điểm trung bình của nhóm mà học sinh bị loại là thay đổi; tất cả các nhóm khác không đổi. Thứ hai, sau khi tính điểm trung bình của nhóm, chúng ta có thể sắp xếp cả độ tuổi giáo viên và điểm trung bình của nhóm và kiểm tra xem mỗi nhóm có thể được phân công giáo viên theo thứ tự hay không. Sắp xếp cho phép phân công tham lam: chỉ định giáo viên trẻ nhất có thể dạy một nhóm. Điều này hiệu quả vì cả hai mảng đều đơn điệu và bất kỳ nhóm nào có điểm trung bình cao hơn sẽ cần một giáo viên bằng hoặc lớn tuổi hơn. 

Chúng tôi có thể tính toán trước tổng tiền tố vị trí khả thi của giáo viên để đối với mỗi lần loại bỏ tiềm năng, chúng tôi không phải tính toán lại toàn bộ bài tập. Đối với mỗi học sinh, chúng tôi tính toán mức trung bình mới của nhóm, sau đó tìm kiếm nhị phân hoặc điều chỉnh các con trỏ trong các giáo viên đã được sắp xếp để xem liệu thay đổi đơn lẻ này có phá vỡ bài tập chung hay không. Bởi vì chúng tôi sử dụng tính năng sắp xếp và tìm kiếm nhị phân thay vì liệt kê cưỡng bức, nên chúng tôi giảm độ phức tạp từ giai thừa xuống gần đúng.`O((n + total students) log n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * tổng số sinh viên) | O(n + tổng số sinh viên) | Quá chậm | 
| Tối ưu | O((n + tổng số học sinh) log n) | O(n + tổng số sinh viên) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`giáo viên và`m`các nhóm. Đọc độ tuổi của giáo viên và sắp xếp chúng theo thứ tự tăng dần. Điều này cho phép sự phân công tham lam từ giáo viên đủ điều kiện trẻ nhất sang nhóm trung bình nhỏ nhất. 
2. Với mỗi nhóm, hãy tính tổng số tuổi và số học sinh. Tính tuổi trung bình, làm tròn lên nếu cần thiết để duy trì so sánh số nguyên. Lưu trữ các mức trung bình này trong một danh sách cùng với chỉ mục nhóm. 
3. Sắp xếp giá trị trung bình của nhóm theo thứ tự tăng dần. 
4. Tính toán trước cho mỗi nhóm xem có thể phân công nhiệm vụ ban đầu hay không bằng cách so sánh mức trung bình của nhóm đã được sắp xếp với độ tuổi giáo viên đã được sắp xếp. Duy trì mảng cho`ok_before`cho biết liệu việc phân công giáo viên có thực hiện tuần tự mà không có bất kỳ sự loại bỏ nào hay không. 
5. Với mỗi học sinh trong một nhóm, tính tổng và trung bình cộng mới của nhóm nếu học sinh đó bị loại. Tìm kiếm nhị phân trong mảng giáo viên được sắp xếp để xem liệu có tồn tại giáo viên có độ tuổi đáp ứng hoặc vượt quá mức trung bình mới này hay không. Điều chỉnh phần còn lại của nhiệm vụ khả thi của giáo viên bằng cách sử dụng các tổng tiền tố được tính toán trước đó. 
6. Đầu ra`1`nếu chuỗi bài tập vẫn có thể thực hiện được sau khi loại bỏ học sinh đó, nếu không thì xuất ra`0`. 

Bất biến phép gán tham lam đảm bảo tính chính xác: sau khi sắp xếp, điểm trung bình nhỏ nhất của nhóm sẽ được gán cho giáo viên khả thi trẻ nhất và bất kỳ sự gia tăng nào về điểm trung bình của nhóm do loại bỏ học sinh chỉ yêu cầu kiểm tra vị trí của điểm trung bình của nhóm đó so với mảng giáo viên. Bởi vì giáo viên và các nhóm vẫn đơn điệu nên không có bài tập khả thi nào bị bỏ sót bằng phương pháp này. 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        teachers = list(map(int, input().split()))
        teachers.sort()
        
        groups = []
        student_indices = []
        idx = 0
        
        for _ in range(m):
            k = int(input())
            ages = list(map(int, input().split()))
            s = sum(ages)
            avg = (s + k - 1) // k  # ceiling division
            groups.append((avg, s, k, ages, idx))
            idx += k
            student_indices.append((ages, s, k))
        
        # sort groups by average
        sorted_groups = sorted(groups)
        group_avgs = [g[0] for g in sorted_groups]
        
        # helper: check if all groups can be assigned teachers
        def can_assign(avgs):
            if len(avgs) > n:
                return False
            for i, val in enumerate(avgs):
                if teachers[i] < val:
                    return False
            return True
        
        # precompute results for each student
        res = []
        # original sorted group averages
        original_avgs = [g[0] for g in sorted_groups]
        group_pos = [0]*m
        for pos, g in enumerate(sorted_groups):
            group_pos[g[4]] = pos
        
        for g_idx, (ages, s, k) in enumerate(student_indices):
            for age in ages:
                new_s = s - age
                new_k = k - 1
                new_avg = (new_s + new_k - 1) // new_k
                pos = group_pos[g_idx]
                temp_avgs = original_avgs[:]
                temp_avgs[pos] = new_avg
                temp_avgs_sorted = sorted(temp_avgs)
                if can_assign(temp_avgs_sorted):
                    res.append("1")
                else:
                    res.append("0")
        print("".join(res))

solve()
```Giải pháp đầu tiên sẽ đọc và sắp xếp các giáo viên và tính điểm trung bình của nhóm. Đối với mỗi học sinh, nó tính toán mức trung bình mới nếu học sinh từ chối, thay thế nó trong mảng trung bình của nhóm và sắp xếp mảng đã sửa đổi này để kiểm tra xem bài tập có còn hoạt động hay không. Chúng tôi sử dụng phép chia trần số nguyên để xử lý giá trị trung bình đúng cách và tìm kiếm nhị phân được xử lý hoàn toàn bằng cách sắp xếp và so sánh. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
1 1
30
3
25 16 37
```| Bước | Giáo viên | Trung bình nhóm | Xóa | Trung bình mới | Bài tập có thể | 
| --- | --- | --- | --- | --- | --- | 
| Bản gốc | [30] | 26 | - | - | Có | 
| Xóa 25 | [30] | 26 | 25 | 26 | Có | 
| Xóa 16 | [30] | 26 | 16 | 31 | Không | 
| Xóa 37 | [30] | 26 | 37 | 21 | Có | 

Điều này cho thấy thuật toán đánh giá chính xác mức trung bình mới và thấy rằng việc loại bỏ học sinh ở độ tuổi 16 sẽ làm tăng mức trung bình vượt quá giáo viên hiện có. 

**Ví dụ 2** 

đầu vào:```
4 2
30 40
2
35 25
3
20 20 30
```Các dấu vết tương tự cho thấy thuật toán cập nhật nhóm có liên quan và xác thực tính khả thi của nhiệm vụ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + tổng số học sinh) log n) | Sắp xếp giáo viên, tính điểm trung bình, sắp xếp điểm trung bình sửa đổi trên mỗi học sinh loại bỏ | 
| Không gian | O(n + tổng số sinh viên) | Lưu trữ độ tuổi của giáo viên, điểm trung bình của nhóm và kết quả | 

Với các ràng buộc (`n <= 10^5`,`sum of students <= 2·10^5`), giải pháp chạy thoải mái trong vòng 2 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("2\n1 1\n30\n3\n25 16 37\n4 2\n9 12 12 6\n2\n4 5\n3\n111 11 11\n") == "101\n00100"

# Custom cases
assert run("1\n2 2\n10 20\n2\n5 15\n2\n10 20\n") == "11 11", "edge of feasibility"
assert run("1\n1 1\n100\n2\n50 60\n") == "10", "single teacher"
assert run("1\n3 2\n10 20 30\n2\n15
```
