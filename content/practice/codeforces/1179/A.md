---
title: "CF 1179A - Valeriy và Deque"
description: "Chúng ta được cho một deque chứa n số nguyên. Valeriy liên tục thực hiện một thao tác trong đó anh loại bỏ hai phần tử đầu tiên, so sánh chúng và sau đó lắp lại chúng: phần tử lớn hơn sẽ ở phía trước và phần tử nhỏ hơn sẽ ở phía sau."
date: "2026-06-12T01:35:59+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1179
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 569 (Div. 1)"
rating: 1500
weight: 1179
solve_time_s: 184
verified: true
draft: false
---

[CF 1179A - Valeriy và Deque](https://codeforces.com/problemset/problem/1179/A) 

**Đánh giá:** 1500 
**Tags:** cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 3m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một deque chứa`n`số nguyên. Valeriy liên tục thực hiện một thao tác trong đó anh loại bỏ hai phần tử đầu tiên, so sánh chúng và sau đó lắp lại chúng: phần tử lớn hơn sẽ ở phía trước và phần tử nhỏ hơn sẽ ở phía sau. Chúng tôi được yêu cầu, vì`q`truy vấn, để xác định hai phần tử nào bị loại bỏ trên`m_j`-thao tác thứ cho mỗi truy vấn. 

Những hạn chế chính là`n`có thể lên tới 10^5 và số lượng truy vấn`q`có thể lên tới 3×10^5. Mỗi truy vấn có thể hỏi về`m_j`-hoạt động thứ, ở đâu`m_j`có thể lớn tới 10^18. Điều này ngay lập tức loại trừ mọi giải pháp mô phỏng từng thao tác một cho tất cả các truy vấn, bởi vì ngay cả`O(n)`mỗi truy vấn sẽ quá chậm và`O(m_j)`là không thể cho lớn`m_j`. 

Các trường hợp biên có thể khiến quá trình triển khai đơn giản không thành công bao gồm: tất cả các phần tử bằng nhau, mảng đã được sắp xếp hoặc truy vấn các thao tác sau khi phần tử lớn nhất đã tiến lên phía trước. Ví dụ: nếu deque là`[5, 5, 5]`và chúng tôi yêu cầu thực hiện thao tác thứ 10, một sự mô phỏng bất cẩn có thể dẫn đến một sự thay đổi khiến điều đó không bao giờ xảy ra. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Bắt đầu với deque như đã cho và đối với mỗi thao tác, hãy bật hai phần tử đầu tiên, so sánh chúng và chèn lại tương ứng. Lặp lại điều này cho đến khi đạt được số lượng thao tác mong muốn. Điều này làm việc cho nhỏ`m_j`, nhưng nếu`m_j`ở mức 10^18, điều đó là không thể thực hiện được. Ngay cả đối với`n = 10^5`Và`q = 10^5`, việc mô phỏng từng thao tác một sẽ yêu cầu hàng nghìn tỷ thao tác trong trường hợp xấu nhất. 

Cái nhìn sâu sắc quan trọng là quá trình này có hai giai đoạn. Đầu tiên, phần tử tối đa trong deque cuối cùng sẽ tiến tới phía trước. Trong giai đoạn này, mỗi thao tác so sánh hai số, di chuyển số lớn hơn về phía trước và số nhỏ hơn về phía sau. Khi mức tối đa ở phía trước, phần tử đầu tiên sẽ không bao giờ thay đổi. Sau đó, mọi thao tác chỉ ghép phần tử phía trước (tối đa) với phần tử tiếp theo, gửi phần tử tiếp theo về phía sau. Giai đoạn thứ hai này hoàn toàn tuần hoàn: nó quay phần còn lại`n-1`các phần tử đứng sau mức tối đa trong một chu kỳ cố định. 

Bằng cách quan sát điều này, chúng ta có thể tính toán trước`n`hoạt động cho đến khi tối đa đạt đến phía trước. Đối với các truy vấn trong phạm vi này, chúng tôi trả về kết quả được tính toán trước. Đối với các truy vấn ngoài điều này, câu trả lời có thể được tính toán bằng cách sử dụng số học mô-đun trên chuỗi tuần hoàn của các phần tử còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m_j) | O(n) | Quá chậm | 
| Tối ưu | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc đầu vào, bao gồm các phần tử deque và các truy vấn. Xác định phần tử tối đa trong deque. 
2. Khởi tạo cấu trúc deque để mô phỏng các hoạt động cho đến khi đạt mức tối đa phía trước. Giữ một danh sách`first_phase`để lưu trữ các cặp phần tử bị loại bỏ trong mỗi thao tác. 
3. Trong khi mặt trước của deque không phải là mức tối đa, hãy bật hai phần tử đầu tiên`A`Và`B`. Nối thêm`(A, B)`ĐẾN`first_phase`. So sánh`A`Và`B`và đẩy cái lớn hơn về phía trước và cái nhỏ hơn về phía sau. Điều này đảm bảo rằng cuối cùng mức tối đa sẽ chiếm lĩnh phía trước. 
4. Khi mức tối đa ở phía trước, hãy ghi lại các phần tử còn lại phía sau nó theo thứ tự như sau:`cycle`. Các phần tử này quay về phía sau tối đa sau mỗi thao tác. 
5. Đối với mỗi truy vấn`m_j`, kiểm tra xem nó có nằm trong phạm vi được tính toán trước không`first_phase`. Nếu vậy, chỉ cần xuất cặp được lưu trữ. Ngược lại, hãy trừ số thao tác trong`first_phase`để có được chỉ số trong giai đoạn tuần hoàn. Phần tử ghép với giá trị lớn nhất là`cycle[(m_j - len(first_phase) - 1) % len(cycle)]`và bản thân giá trị lớn nhất luôn là phần tử còn lại. 
6. In câu trả lời cho tất cả các câu hỏi theo thứ tự được đưa ra. 

Tại sao nó hoạt động: Điều bất biến là khi mức tối đa ở phía trước, nó sẽ không bao giờ di chuyển. Các phần tử còn lại tạo thành một chuỗi cố định phía sau nó và mỗi thao tác sẽ di chuyển phần tử tiếp theo về phía sau. Điều này đảm bảo rằng mọi truy vấn đều có thể được trả lời bằng cách sử dụng danh sách giai đoạn đầu được tính toán trước hoặc tra cứu mô-đun đơn giản trong giai đoạn tuần hoàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

n, q = map(int, input().split())
a = list(map(int, input().split()))
queries = [int(input()) for _ in range(q)]

d = deque(a)
first_phase = []

max_val = max(a)

while d[0] != max_val:
    A = d.popleft()
    B = d.popleft()
    first_phase.append((A, B))
    if A > B:
        d.appendleft(A)
        d.append(B)
    else:
        d.appendleft(B)
        d.append(A)

# Now the maximum is at the front
d.popleft()  # remove max for cycle
cycle = list(d)

answers = []
for m in queries:
    if m <= len(first_phase):
        answers.append(first_phase[m - 1])
    else:
        idx = (m - len(first_phase) - 1) % len(cycle)
        answers.append((max_val, cycle[idx]))

for a, b in answers:
    print(a, b)
```Giải pháp đầu tiên chỉ mô phỏng các hoạt động cho đến khi đạt mức tối đa về phía trước, lưu trữ kết quả của từng hoạt động. Sau đó, deque được chia thành phần tử lớn nhất và chu kỳ của các phần tử còn lại. Đối với các truy vấn sau giai đoạn đầu tiên, số học mô-đun được sử dụng để xác định phần tử nào được ghép với giá trị tối đa. Điều này xử lý số lượng truy vấn cực lớn một cách hiệu quả mà không cần mô phỏng tất cả các hoạt động. 

## Ví dụ đã hoạt động 

Dấu vết cho đầu vào mẫu:```
5 3
1 2 3 4 5
1
2
10
```### Mô phỏng giai đoạn đầu 

| Hoạt động | Deque Mặt Trận Hai | Deque Sau Hoạt Động | 
| --- | --- | --- | 
| 1 | 1, 2 | 2, 3, 4, 5, 1 | 
| 2 | 2, 3 | 3, 4, 5, 1, 2 | 
| 3 | 3, 4 | 4, 5, 1, 2, 3 | 
| 4 | 4, 5 | 5, 1, 2, 3, 4 | 

Tối đa`5`đạt phía trước, chu kỳ là`[1,2,3,4]`. 

### Xử lý truy vấn 

Truy vấn 1 → Thao tác 1 →`(1,2)`từ giai đoạn đầu tiên. 

Truy vấn 2 → Thao tác 2 →`(2,3)`từ giai đoạn đầu tiên. 

Truy vấn 10 → Thao tác 10 → 10 - 4 = 6 → chỉ số 6 % 4 = 2 → chu kỳ[2] = 3 → trả lời`(5,3)`. 

Dấu vết này cho thấy sự tách biệt giữa pha đầu tiên và pha tuần hoàn, đồng thời số học mô-đun đó mang lại sự ghép nối chính xác với mức tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Mô phỏng nhiều nhất`n`các hoạt động để đưa giá trị tối đa lên phía trước, sau đó O(1) cho mỗi truy vấn bằng cách sử dụng số học mô-đun | 
| Không gian | O(n) | Lưu trữ các phép toán deque và pha đầu tiên cũng như chu kỳ có độ dài tối đa n-1 | 

Điều này đủ hiệu quả cho đầu vào lớn nhất vì`n`10^5 và`q`3×10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline
    
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    queries = [int(input()) for _ in range(q)]
    
    d = deque(a)
    first_phase = []

    max_val = max(a)
    while d[0] != max_val:
        A = d.popleft()
        B = d.popleft()
        first_phase.append((A, B))
        if A > B:
            d.appendleft(A)
            d.append(B)
        else:
            d.appendleft(B)
            d.append(A)
    
    d.popleft()
    cycle = list(d)
    
    answers = []
    for m in queries:
        if m <= len(first_phase):
            answers.append(first_phase[m - 1])
        else:
            idx = (m - len(first_phase) - 1) % len(cycle)
            answers.append((max_val, cycle[idx]))
    
    return "\n".join(f"{x[0]} {x[1]}" for x in answers)

# Provided samples
assert run("5 3\n1 2 3 4 5\n1\n2\n10\n") == "1 2\n2 3\n5 2"

# Minimum input
assert run("2 1\n0 0\n1\n") == "0 0"

# Maximum element at front initially
assert run("3 2\n9 1 2\n1\n4\n") == "9 1\n9 2"

# All equal elements
assert run("4 3\n7 7 7 7\n1\n5\n10\n") == "7 7\n7 7\n7 7"

# Large query beyond first phase
assert run("5 2\n1 2
```
