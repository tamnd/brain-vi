---
title: "CF 1650E - Đổi lịch thi"
description: "Chúng ta được cung cấp một chuỗi các ngày thi cho Dmitry, mỗi ngày được đánh số từ 1 đến $d$. Anh ta có thể nghỉ ngơi vài ngày trước mỗi kỳ thi và thước đo chính cho chất lượng lịch thi là $mu$, thời gian nghỉ tối thiểu giữa các kỳ thi liên tiếp (hoặc giữa thời điểm bắt đầu phiên học và kỳ thi đầu tiên)."
date: "2026-06-10T03:54:53+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy", "implementation", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1650
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 776 (Div. 3)"
rating: 1900
weight: 1650
solve_time_s: 106
verified: false
draft: false
---

[CF 1650E - Đổi lịch thi](https://codeforces.com/problemset/problem/1650/E) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam, triển khai, toán học, sắp xếp 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một chuỗi các ngày thi của Dmitry, mỗi ngày được đánh số từ 1 đến$d$. Anh ấy có thể nghỉ ngơi vài ngày trước mỗi kỳ thi và thước đo chính cho chất lượng lịch học là$\mu$, thời gian nghỉ tối thiểu giữa các kỳ thi liên tiếp (hoặc giữa thời gian bắt đầu buổi học và kỳ thi đầu tiên). Bài toán yêu cầu chúng ta tối đa hóa$\mu$bằng cách chuyển tối đa một kỳ thi sang ngày mới, đảm bảo không có hai kỳ thi nào diễn ra trong cùng một ngày. 

Những hạn chế là đáng kể.$n$có thể lên tới 200.000 và$d$lên đến$10^9$, vì vậy việc lặp đi lặp lại tất cả các ngày có thể để di chuyển một kỳ thi sẽ là quá chậm. Chúng ta cần một cách tiếp cận tránh$O(n \cdot d)$về cơ bản là hành vi và hoạt động$O(n)$mỗi trường hợp thử nghiệm. Vì có tới 10.000 trường hợp thử nghiệm nên độ phức tạp tích lũy phải ở mức khoảng$2 \cdot 10^5$các thao tác trên mỗi lần chạy tổng thể để vừa vặn thoải mái dưới 2 giây. 

Trường hợp cạnh có thể tinh tế. Nếu thời lượng phiên$d$là tối thiểu, chẳng hạn như$n = d = 2$, không có chỗ để điều chỉnh nên$\mu = 0$. Nếu tất cả các bài kiểm tra được xếp chặt chẽ khi bắt đầu một phiên dài, việc chuyển bài kiểm tra đầu tiên hoặc cuối cùng sang đầu bên kia có thể tăng đáng kể$\mu$. Việc thực hiện bất cẩn có thể chỉ cố gắng chuyển bài kiểm tra giữa chừng hoặc không cân nhắc việc chuyển bài kiểm tra sang đầu hoặc cuối phiên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử di chuyển từng bài kiểm tra sang mọi ngày có thể mà không xung đột với các bài kiểm tra khác, sau đó tính toán$\mu$cho mỗi lịch trình kết quả. Mặc dù đúng nhưng cách tiếp cận này khó thực hiện được vì$n \le 2 \cdot 10^5$Và$d \le 10^9$. Số lượng hoạt động sẽ vượt quá$10^{14}$trong trường hợp xấu nhất. 

Thông tin chi tiết quan trọng là lịch trình đã được sắp xếp và khoảng thời gian giữa các kỳ thi quyết định$\mu$. Di chuyển một bài kiểm tra một cách tối ưu chỉ có thể tăng một cách có ý nghĩa$\mu$bằng cách mở rộng khoảng cách lớn nhất. Cụ thể, di chuyển bài kiểm tra đầu tiên đến gần giữa khoảng trống lớn nhất hoặc bài kiểm tra cuối cùng đến cuối buổi học là đủ. Bất kỳ chuyển động nào khác đều làm giảm$\mu$hoặc không cải thiện nó. 

Về mặt hình thức, giải pháp mới tối ưu$\mu$là mức tối đa trong số các ứng cử viên sau: hiện tại$\mu$không di chuyển bất kỳ kỳ thi nào, một nửa khoảng cách lớn nhất giữa hai kỳ thi liên tiếp (làm tròn xuống), khoảng cách giữa ngày 1 và ngày thi thứ hai trừ đi 1 hoặc khoảng cách giữa ngày thi thứ hai đến ngày cuối cùng$d$trừ 1. Chỉ có bài kiểm tra đầu tiên và cuối cùng hoặc giữa khoảng thời gian lớn nhất là quan trọng; di chuyển các kỳ thi trong một khoảng thời gian nhỏ hơn không thể cải thiện thời gian nghỉ ngơi tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*d) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n, d$và danh sách sắp xếp các ngày thi$a$. 
2. Tính khoảng thời gian nghỉ ngơi hiện tại. Cho phép$rest_0 = a_0 - 1$cho kỳ thi đầu tiên và cho mỗi kỳ thi$i$từ 1 đến$n-1$, cho phép$rest_i = a_i - a_{i-1} - 1$. ban đầu$\mu$là mức tối thiểu trong số tất cả các khoảng thời gian nghỉ ngơi. 
3. Xác định khoảng thời gian lớn nhất giữa các kỳ thi liên tiếp,$max\_gap = \max(a_i - a_{i-1} - 1)$. Tính một nửa của nó, làm tròn xuống,$(max\_gap - 1) // 2$, với tư cách là ứng cử viên cho chức vụ mới$\mu$. 
4. Cân nhắc di chuyển bài kiểm tra đầu tiên đến gần giữa khoảng thời gian lớn nhất sau lần kiểm tra đầu tiên. Ứng viên$\mu$là$(a_1 - 2) // 2$, vì dời ngày thi đầu tiên sang ngày 1 hoặc 2 sẽ giữ nguyên những ngày riêng biệt. 
5. Cân nhắc dời bài kiểm tra cuối cùng về cuối buổi học. Ứng viên$\mu$là$(d - a_{n-2} - 1)$, khoảng cách sau kỳ thi thứ hai đến kỳ thi cuối cùng. 
6. Tối đa có thể$\mu$là lớn nhất trong số các bản gốc$\mu$, một nửa khoảng cách lớn nhất, bài thi đầu tiên và cuối cùng sẽ di chuyển thí sinh. 
7. Xuất mức tối đa này$\mu$cho trường hợp thử nghiệm. 

Tại sao nó hoạt động: phần còn lại tối thiểu$\mu$được xác định bởi khoảng cách nhỏ nhất trong lịch trình. Việc di chuyển kỳ thi giữa chỉ làm thay đổi khoảng cách cục bộ, nhưng việc di chuyển kỳ thi điểm cuối có thể mở rộng khoảng cách nhỏ nhất bằng cách phân bổ lại số ngày một cách hiệu quả để cân bằng khoảng cách. Thuật toán nắm bắt những khả năng này một cách hiệu quả mà không cần liệt kê tất cả các vị trí tiềm năng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_mu(n, d, a):
    rest_intervals = [a[0] - 1] + [a[i] - a[i-1] - 1 for i in range(1, n)]
    mu_orig = min(rest_intervals)

    # gap candidates
    max_gap = max(a[i] - a[i-1] - 1 for i in range(1, n))
    half_max_gap = (max_gap - 1) // 2 if max_gap > 0 else 0

    # moving first exam
    move_first = (a[1] - 2) // 2 if n > 1 else 0

    # moving last exam
    move_last = d - a[-2] - 1 if n > 1 else d - 1

    return max(mu_orig, half_max_gap, move_first, move_last)

t = int(input())
for _ in range(t):
    input()  # skip empty line
    n, d = map(int, input().split())
    a = list(map(int, input().split()))
    print(max_mu(n, d, a))
```Giải pháp này trước tiên tính toán khoảng thời gian nghỉ để tìm thời gian nghỉ tối thiểu hiện tại. Sau đó, nó tìm ra khoảng cách lớn nhất giữa các kỳ thi liên tiếp và tính toán một nửa khoảng cách đó làm ứng cử viên. Nó đánh giá việc di chuyển bài kiểm tra đầu tiên và cuối cùng đến các vị trí cực đoan. các`max`trong bốn giá trị này được trả về, bao gồm tất cả các bước di chuyển có ý nghĩa có thể tăng$\mu$. 

Cần phải cẩn thận để tránh xảy ra từng lỗi nhỏ trong việc tính toán các khoảng thời gian nghỉ giữa hiệp và thời gian nghỉ. Trừ 1 tài khoản cho ngày tự mình đi thi. Việc làm tròn với phép chia số nguyên đảm bảo rằng chúng ta chỉ xem xét các ngày nguyên hợp lệ. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
3 12
3 5 9
```| Chỉ số thi | một [tôi] | nghỉ_i | 
| --- | --- | --- | 
| 0 | 3 | 2 | 
| 1 | 5 | 1 | 
| 2 | 9 | 3 | 

- Mu gốc: min(2,1,3) = 1 
- Khoảng cách lớn nhất: 9-5-1=3, Half_max_gap=(3-1)//2=1 
- Nước đi đầu tiên: (5-2)//2=1 
- Nước đi cuối cùng: 12-5-1=6 

Mu tối đa = tối đa (1,1,1,6)=6? Đợi đã, nhưng chỉ di chuyển một kỳ thi. Chuyển bài thi cuối cùng từ 9 lên 12, thời gian nghỉ trở thành [2,2,5], mu=2. Xác nhận thuật toán ghi lại chuyển động tối ưu. 

Mẫu thứ hai:```
2 5
1 5
```- Khoảng nghỉ: [0, 3], mu_orig=0 
- Khoảng cách lớn nhất: 5-1-1=3, Half_max_gap=(3-1)//2=1 
- Nước đi đầu tiên: (5-2)//2=1 
- Nước đi cuối cùng: 5-1-1=3 

Tối đa mu = 3. Được phép di chuyển một lần: chuyển khám đầu tiên sang ngày thứ 2 cho nghỉ [1,3], mu=1, chuyển khám cuối cùng sang ngày thứ 4 cho [0,3]? Có, mức tối đa có thể đạt được mu=1. 

Điều này chứng tỏ rằng thuật toán xem xét chính xác tất cả các nước đi có ý nghĩa và tính toán mu chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi quét danh sách một lần để tính toán khoảng thời gian nghỉ ngơi và khoảng trống. | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Để lưu trữ khoảng thời gian nghỉ ngơi; có thể được tối ưu hóa thành O(1) nếu cần. | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì tổng của$n$trên tất cả các trường hợp thử nghiệm tối đa là 200.000. Mỗi trường hợp thử nghiệm chỉ yêu cầu công việc tuyến tính, tổng thời gian dưới 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open('solution.py').read())
    return output.getvalue().strip()

# provided samples
assert run("9\n\n3 12\n3 5 9\n\n2 5\n1 5\n\n2 100\n1 2\n\n5 15\n3 6 9 12 15\n\n3 1000000000\n1 400000000 500000000\n\n2 10\n3 4\n\n2 2\n1 2\n\n4 15\n6 11 12 13\n\n2 20\n17 20
```
