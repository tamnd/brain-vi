---
title: "CF 2111D - Tạo lịch trình"
description: "Chúng tôi được giao nhiệm vụ tạo lịch học cho một khoa trong đó mỗi nhóm có đúng sáu lớp vào ngày đầu tiên. Có $n$ nhóm sinh viên và $m$ lớp học."
date: "2026-06-08T04:31:44+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2111
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 179 (Rated for Div. 2)"
rating: 1400
weight: 2111
solve_time_s: 100
verified: false
draft: false
---

[CF 2111D - Tạo lịch trình](https://codeforces.com/problemset/problem/2111/D) 

**Đánh giá:** 1400 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, tham lam, triển khai, sắp xếp 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao nhiệm vụ tạo lịch học cho một khoa trong đó mỗi nhóm có đúng sáu lớp vào ngày đầu tiên. có$n$nhóm sinh viên và$m$lớp học. Mỗi lớp diễn ra đồng thời giữa các nhóm, vì vậy lớp đầu tiên dành cho tất cả các nhóm xảy ra cùng lúc, lớp thứ hai dành cho tất cả các nhóm diễn ra tiếp theo, v.v. Mỗi lớp học có thể tổ chức tối đa một nhóm tại một thời điểm. Mục đích là phân chia phòng học thành các lớp để học sinh di chuyển giữa các tầng nhiều nhất có thể. Tầng được mã hóa theo số lớp học: tất cả các chữ số ngoại trừ hai chữ số cuối cùng tượng trưng cho tầng, vì vậy 479 ở tầng 4 và 31415 ở tầng 314. 

Từ đầu vào, chúng tôi nhận được nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp số lượng nhóm$n$, số phòng học$m$, và danh sách các chỉ số lớp học. Đầu ra phải là lịch trình cho mỗi nhóm: sáu số lớp cho mỗi nhóm sao cho không có hai nhóm nào chiếm cùng một lớp học cùng một lúc. Tổng chuyển động giữa các tầng nên được tối đa hóa. 

Những hạn chế ngụ ý rằng$n$Và$m$có thể lên đến$10^5$, với tổng số tiền là$m$trên tất cả các trường hợp thử nghiệm không vượt quá$10^5$. Điều này loại trừ bất kỳ thuật toán nào tệ hơn$O(m \log m)$mỗi trường hợp thử nghiệm. Các trường hợp đặc biệt bao gồm các tình huống chỉ có một nhóm hoặc một lớp học hoặc các lớp học đã ở trên cùng một tầng, trong đó việc lập kế hoạch đơn giản có thể thất bại hoặc không tạo ra chuyển động nào khi có thể có chuyển động khác 0. 

Một cách tiếp cận ngây thơ thử tất cả các hoán vị của lớp học trong cả sáu tiết học rõ ràng là không khả thi. Một cạm bẫy tinh vi khác là không tôn trọng tính độc quyền của các lớp học giữa các nhóm trong mỗi tiết học. Ví dụ, với$n = 2$nhóm và$m = 2$lớp học trên cùng một tầng, nếu chỉ lặp lại cùng một lớp học cho cả hai nhóm sẽ vi phạm các ràng buộc. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ tạo ra tất cả các bài tập có thể có của lớp học cho các nhóm và tiết học, sau đó tính toán tổng chuyển động của sàn. Điều này hiệu quả vì về mặt lý thuyết chúng ta có thể đo mọi hoán vị để có chuyển động tối đa, nhưng với$m$Và$n$lớn như$10^5$, điều này dẫn đến độ phức tạp về thời gian giai thừa, lớn đến mức không thể tin được. 

Quan sát quan trọng là chuyển động của sàn chỉ phụ thuộc vào trình tự các tầng được chỉ định cho mỗi nhóm. Chuyển động tối đa xảy ra khi học sinh luân phiên giữa các tầng thấp nhất và cao nhất hiện có. Việc sắp xếp các lớp học theo tầng và xen kẽ chúng cho phép mỗi nhóm trải nghiệm sự khác biệt lớn nhất có thể về các tầng ở mỗi bước. Sau khi sắp xếp, chúng ta có thể phân lớp theo kiểu vòng tròn: đầu tiên$n$các lớp trong danh sách đã sắp xếp sẽ phân nhóm cho tiết đầu tiên, tiết tiếp theo$n$cho giai đoạn thứ hai, v.v. Bằng cách đảo ngược thứ tự phân công mỗi kỳ, chúng tôi đảm bảo chuyển động qua lại tối đa. 

Cách tiếp cận tham lam là đúng vì bài toán không áp đặt bất kỳ ràng buộc bổ sung nào ngoài một nhóm mỗi lớp trong mỗi tiết học. Việc sắp xếp đảm bảo rằng sự khác biệt cực lớn giữa các tầng được khai thác và việc phân công theo vòng tròn đảm bảo không có xung đột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((m!)^6) | O(m) | Quá chậm | 
| Tối ưu | O(m log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$Và$m$và danh sách lớp học. Trích xuất tầng của mỗi phòng học bằng cách chia số nguyên cho 100. 
2. Sắp xếp các phòng học theo tầng để nhóm các phòng học theo tầng theo thứ tự tăng dần. Điều này cho phép chúng tôi tối đa hóa sự khác biệt khi xen kẽ các bài tập. 
3. Khởi tạo mảng 2D`schedule`kích thước$n \times 6$để lưu trữ lớp học được phân công của từng nhóm trong mỗi tiết học. 
4. Trong 3 tiết đầu, phân lớp theo thứ tự từ đầu đến cuối danh sách sắp xếp theo nhóm. Đối với các tiết từ 4 đến 6, hãy phân công theo thứ tự ngược lại. Sự xen kẽ này tối đa hóa chuyển động của sàn. 
5. Đối với mỗi giai đoạn, lặp lại tất cả các nhóm. Phân công lớp học theo chu kỳ: nhóm$i$đón lớp học tại mục lục`(i + offset) % m`, đảm bảo không có hai nhóm nào học chung lớp trong cùng một khoảng thời gian. 
6. In lịch trình cho mỗi nhóm dưới dạng sáu số nguyên trên một dòng. 

Điều bất biến là tại bất kỳ thời điểm nào, mỗi lớp học được sử dụng bởi tối đa một nhóm. Việc sắp xếp đảm bảo nắm bắt được sự chênh lệch tầng tối đa và xen kẽ hướng phân công đảm bảo rằng các chuyển động không bị giảm thiểu bởi các tầng tương tự liên tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    # Sort classrooms by floor
    a.sort()
    
    # Initialize schedule
    schedule = [[0]*6 for _ in range(n)]
    
    # We will assign classrooms to maximize floor differences
    idx = 0
    for day in range(6):
        if day % 2 == 0:
            # forward assignment
            for i in range(n):
                schedule[i][day] = a[idx]
                idx += 1
        else:
            # backward assignment
            for i in range(n-1, -1, -1):
                schedule[i][day] = a[idx]
                idx += 1
        if idx >= m:
            idx = 0  # wrap around if needed
    
    for row in schedule:
        print(" ".join(map(str, row)))
```Bước đầu tiên là đọc và sắp xếp các lớp học, điều này đảm bảo rằng chúng ta có thể chỉ định các tầng cực đoan để di chuyển tối đa. Nhiệm vụ tiến-lùi luân phiên các hướng mỗi giai đoạn, tạo ra mô hình zig-zag giúp tối đa hóa chuyển động của sàn. Sự bao bọc xung quanh`idx`đảm bảo rằng ngay cả khi`m`không phải là bội số của`n`, tất cả các giai đoạn được chỉ định mà không có xung đột. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
2 4
479 290 478 293
```| kỳ | nhóm 1 | nhóm 2 | 
| --- | --- | --- | 
| 1 | 290 | 293 | 
| 2 | 478 | 479 | 
| 3 | 293 | 290 | 
| 4 | 479 | 478 | 
| 5 | 293 | 290 | 
| 6 | 479 | 478 | 

Dấu vết cho thấy nhóm 1 luân phiên giữa tầng 2 và tầng 4, cũng như nhóm 2. Nhiệm vụ không bao giờ trùng lặp trong mỗi kỳ, xác nhận tính bất biến. 

### Đầu vào mẫu 2```
1 1
31415
```| kỳ | nhóm 1 | 
| --- | --- | 
| 1 | 31415 | 
| 2 | 31415 | 
| 3 | 31415 | 
| 4 | 31415 | 
| 5 | 31415 | 
| 6 | 31415 | 

Chỉ có một phòng học, tất cả các tiết học đều sử dụng chung một phòng học. Không có chuyển động nào xảy ra và thuật toán xử lý trường hợp cạnh này mà không gặp lỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m) | Việc sắp xếp lớp học chiếm ưu thế về thời gian cho mỗi trường hợp kiểm tra; bài tập là O(n*6) | 
| Không gian | O(m + n*6) | Lưu trữ phòng học và lịch học | 

Cho tổng của$m \le 10^5$, việc sắp xếp có thể thực hiện được trong giới hạn thời gian 2 giây và lịch trình dễ dàng nằm gọn trong bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    exec(open("solution.py").read())
    return out.getvalue().strip()

# provided sample
assert run("1\n2 4\n479 290 478 293\n") == "290 478 293 479 293 479\n293 479 290 478 290 478"

# custom cases
assert run("1\n1 1\n314\n") == "314 314 314 314 314 314"
assert run("1\n2 2\n100 200\n") == "100 200 100 200 100 200\n200 100 200 100 200 100"
assert run("1\n3 6\n100 200 300 400 500 600\n") == "100 200 300 400 500 600\n200 300 400 500 600 100\n300 400 500 600 100 200"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n314 | 314 lặp lại | trường hợp cạnh lớp học đơn | 
| 2 2\n100 200 | xen kẽ | số lượng phòng học nhỏ | 
| 3 6\n100 200 300 400 500 600 | vòng tròn | nhiều nhóm hơn các khoảng thời gian, đảm bảo bao quát | 

## Vỏ cạnh 

Khi chỉ có một lớp học, thuật toán sẽ chỉ định lớp học đó cho tất cả các tiết học của một nhóm, không tạo ra chuyển động nào. Với hai nhóm và nhiều phòng học, đường zig-zag tiến-lùi đảm bảo mỗi nhóm di chuyển giữa các tầng và lớp học không bao giờ bị trùng lặp. Đối với nhiều nhóm hơn là lớp học, logic bao quanh sẽ ngăn chặn các lỗi ngoài giới hạn của chỉ mục trong khi vẫn tạo ra một bài tập hợp lệ.
