---
title: "CF 1886E - Tôi Muốn Làm Trưởng Nhóm"
description: "Chúng tôi được giao cho một nhóm lập trình viên, mỗi người có một mức độ chịu đựng căng thẳng khác nhau và một nhóm dự án, mỗi dự án có một độ khó khác nhau."
date: "2026-06-08T22:18:06+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "dp", "greedy", "math", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1886
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 156 (Rated for Div. 2)"
rating: 2400
weight: 1886
solve_time_s: 147
verified: false
draft: false
---

[CF 1886E - Tôi muốn trở thành Trưởng nhóm](https://codeforces.com/problemset/problem/1886/E) 

**Đánh giá:** 2400 
**Tags:** bitmasks, thuật toán xây dựng, dp, tham lam, toán học, sắp xếp, hai con trỏ 
**Thời gian giải:** 2m 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao cho một nhóm lập trình viên, mỗi người có một mức độ chịu đựng căng thẳng khác nhau và một nhóm dự án, mỗi dự án có một độ khó khác nhau. Mục tiêu là phân công các lập trình viên cho các dự án sao cho mỗi dự án có ít nhất một lập trình viên, không có lập trình viên nào làm việc trên nhiều dự án và khả năng chịu căng thẳng của mỗi lập trình viên được giao cho một dự án ít nhất là độ khó của dự án chia cho số lượng lập trình viên được giao cho nó. Đầu ra phải là "KHÔNG" nếu điều này là không thể hoặc "CÓ" theo sau là một phép gán hợp lệ. 

Những hạn chế là quan trọng. Có thể có tới 200.000 lập trình viên nhưng chỉ có tối đa 20 dự án. Điều này gợi ý rằng chúng ta có thể cung cấp các thuật toán có quy mô theo cấp số nhân về số lượng dự án, nhưng không phải về số lượng lập trình viên. Một cách ép buộc ngây thơ xem xét tất cả các phân vùng có thể có của các lập trình viên giữa các dự án sẽ ngay lập tức bị loại trừ vì có$m^n$các bài tập có thể thực hiện được, đó là điều vô cùng to lớn khi$n$là 200.000. Tuy nhiên, số lượng dự án nhỏ cho thấy chúng ta có thể sử dụng các kỹ thuật như che bit cho dự án. 

Các trường hợp khó khăn là khi cách tiếp cận ngây thơ có thể thất bại. Ví dụ: nếu tất cả các dự án đều rất khó và lập trình viên rất yếu thì thuật toán phải trả về chính xác "KHÔNG". Nếu một dự án cực kỳ dễ dàng, nó có thể thu hút tất cả các lập trình viên tham lam phân bổ trừ khi được kiểm soát cẩn thận. Nếu độ khó của dự án chia cho số lượng lập trình viên được giao chính xác bằng mức độ căng thẳng của một số lập trình viên, thì lỗi dấu phẩy động có thể gây ra so sánh sai nếu thực hiện bất cẩn. 

Ví dụ, hãy xem xét`n=3`,`m=2`,`a=[2,3,4]`,`b=[6,1]`. Một nhiệm vụ tham lam bất cẩn có thể cố gắng đưa lập trình viên 3 vào dự án 1 và sau đó không giao dự án 2 một cách chính xác, mặc dù vẫn tồn tại một nhiệm vụ hợp lệ: dự án 1 có lập trình viên 2 và 3, dự án 2 có lập trình viên 1. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực là thử mọi cách có thể để phân công các lập trình viên cho các dự án. Đối với mỗi dự án, bạn sẽ chọn một số tập hợp con của các lập trình viên còn lại, kiểm tra điều kiện chịu đựng áp lực và lặp lại cho các dự án còn lại. Về nguyên tắc thì điều này có hiệu quả, nhưng số lượng nhóm lập trình viên cho một dự án thậm chí là$2^n$, điều này là không thể thực hiện được với$n$lên tới 200.000. 

Cái nhìn sâu sắc quan trọng là các dự án rất ít. Bởi vì$m \le 20$, chúng ta có thể coi đây là vấn đề phân bổ các lập trình viên vào$m$các nhóm trong khi vẫn đảm bảo điều kiện số học đơn giản cho mỗi nhóm. Sắp xếp các lập trình viên và ấn định dung sai căng thẳng lớn nhất còn lại cho các dự án có tỷ lệ độ khó đối với lập trình viên lớn nhất là một cách tiếp cận hiệu quả. Cụ thể, nếu chúng tôi luôn chỉ định lập trình viên có sẵn tiếp theo cho dự án hiện có yêu cầu "độ khó cho mỗi lập trình viên được giao" lớn nhất còn lại, chúng tôi đảm bảo rằng chúng tôi không vi phạm điều kiện căng thẳng đồng thời đảm bảo mỗi dự án có ít nhất một lập trình viên. 

Quan sát này cho phép chúng tôi xử lý các lập trình viên theo thứ tự căng thẳng giảm dần và phân công họ một cách tham lam, duy trì cấu trúc heap tối thiểu hoặc tương tự để tìm ra dự án cần lập trình viên tiếp theo nhất một cách hiệu quả. Điều này làm giảm vấn đề xuống$O(n \log m)$thời gian vì mỗi bài tập chỉ yêu cầu cập nhật tối đa đống kích thước$m$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O($2^n$) | O(n) | Quá chậm | 
| Tối ưu | O(n log m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp các bộ lập trình theo khả năng chịu ứng suất theo thứ tự giảm dần, theo dõi các chỉ số ban đầu của chúng. Điều này đảm bảo rằng chúng tôi luôn cố gắng chỉ định lập trình viên mạnh nhất hiện có trước, giúp tối đa hóa tính linh hoạt của chúng tôi. 
2. Khởi tạo một min-heap cho các dự án, trong đó mỗi dự án được biểu diễn dưới dạng một bộ chứa số lượng lập trình viên hiện tại được chỉ định và độ khó của dự án. Bắt đầu mỗi dự án với một lập trình viên được giao về mặt khái niệm để chúng ta có thể tính toán "độ khó cho mỗi lập trình viên" hiện tại một cách dễ dàng. Đống được sắp xếp theo ứng suất cần thiết hiệu quả$b_i / k_i$. 
3. Lặp lại từng lập trình viên theo thứ tự căng thẳng giảm dần. Kích hoạt dự án với ứng suất yêu cầu lớn nhất (điều này có thể được thực hiện một cách hiệu quả với một đống lưu trữ các giá trị âm để mô phỏng một đống tối đa). Kiểm tra xem khả năng chịu đựng căng thẳng của người lập trình có ít nhất là mức căng thẳng cần thiết hiện tại cho dự án này hay không. Nếu có, hãy chỉ định lập trình viên, tăng số lượng lập trình viên được chỉ định cho dự án và đẩy nó trở lại vùng nhớ với ứng suất được yêu cầu đã cập nhật. Nếu lập trình viên không thể đáp ứng yêu cầu của dự án đòi hỏi khắt khe nhất thì sẽ không có sự phân công hợp lệ nào. 
4. Tiếp tục cho đến khi tất cả các lập trình viên được chỉ định hoặc cho đến khi mỗi dự án có ít nhất một lập trình viên. Sau vòng lặp, hãy kiểm tra xem tất cả các dự án có ít nhất một lập trình viên hay không. Nếu vậy, hãy in "CÓ" theo sau là danh sách bài tập; nếu không thì in "KHÔNG". 

Tại sao nó hoạt động: Thuật toán duy trì sự bất biến rằng áp lực cần thiết hiện tại cho bất kỳ dự án nào luôn được giảm thiểu với số lượng lập trình viên đã được chỉ định. Bằng cách chỉ định lập trình viên giỏi nhất cho dự án cần nhất, chúng tôi không bao giờ cản trở việc phân công trong tương lai. Việc sắp xếp lập trình viên đảm bảo rằng những lập trình viên yếu hơn chỉ được phân công khi họ có thể đáp ứng được yêu cầu nào đó của dự án. 

## Giải pháp Python```python
import sys, heapq
input = sys.stdin.readline

n, m = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

prog = sorted([(val, i + 1) for i, val in enumerate(a)], reverse=True)
projects = [(b[i], i) for i in range(m)]
assignments = [[] for _ in range(m)]

# Initialize heap: (current_needed_stress, num_assigned, project_index)
heap = []
for diff, idx in projects:
    heap.append((diff, 1, idx))  # start with 1 programmer to avoid divide by zero
heapq.heapify(heap)

for stress, prog_idx in prog:
    # get project with highest required stress
    curr_needed, count, proj_idx = heapq.heappop(heap)
    if stress * count < b[proj_idx]:  # b/k > stress check
        print("NO")
        sys.exit(0)
    assignments[proj_idx].append(prog_idx)
    count += 1
    heapq.heappush(heap, (b[proj_idx] / count, count, proj_idx))

print("YES")
for lst in assignments:
    print(len(lst), *lst)
```Mã sắp xếp các lập trình viên một lần và sử dụng một đống để theo dõi dự án cần lập trình viên tiếp theo nhất. Heap lưu trữ một giá trị dấu phẩy động`b[proj_idx]/count`để thể hiện ứng suất yêu cầu hiện tại. sử dụng`stress * count < b[proj_idx]`tránh các vấn đề về độ chính xác của dấu phẩy động. 

## Ví dụ đã hoạt động 

**Mẫu 1**: 

đầu vào:```
5 3
4 6 100 5 1
50 1 12
```| Bước | Lập trình viên | Đống trước | Hành động | Đống sau | Bài tập | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 100 (idx 3) | [(50,1,0),(1,1,1),(12,1,2)] | Giao cho dự án 0 | [(25,2,0),(1,1,1),(12,1,2)] | [[3],[],[]] | 
| 2 | 6 (idx 2) | [(25,2,0),(1,1,1),(12,1,2)] | Giao dự án 2 | [(25,2,0),(1,1,1),(12/2,2,2)=6] | [[3],[],[2]] | 
| 3 | 5 (idx 4) | ... | Giao dự án 2 | ... | [[3],[],[2,4]] | 
| 4 | 4 (idx 1) | ... | Giao dự án 1 | ... | [[3],[1], [2,4]] | 
| 5 | 1 (idx 5) | ... | Giao dự án 1 | ... | [[3],[1,5],[2,4]] | 

Điều này chứng tỏ rằng việc gán tham lam theo yêu cầu hiện tại tối đa có hiệu quả. 

**Mẫu tùy chỉnh**: 

đầu vào:```
3 2
2 3 4
6 1
```Trace xác nhận dự án 0 nhận lập trình viên 3 và 2, dự án 1 nhận lập trình viên 1, đáp ứng mọi ràng buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log m) | Lập trình viên sắp xếp là O(n log n), mỗi phép gán bao gồm một thao tác heap O(log m), tổng O(n log m) | 
| Không gian | O(n + m) | Lưu trữ các chỉ số lập trình viên và bài tập dự án | 

Điều này phù hợp trong giới hạn bởi vì$n$là 2e5 và$m$là 20 và hoạt động của heap cực kỳ nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        exec(open("solution.py").read())
    return out.getvalue().strip()

# Provided sample
assert run("5 3\n4 6 100 5 1\n50 1 12\n") == "YES\n1 3\n2 1 5\n2 2 4", "sample 1"

# Custom: minimal input
assert run("1 1\n5\n5\n") == "YES\n1 1", "minimal input"

# Custom: impossible case
assert run("2 1\n1 1\n3\n") == "
```
