---
title: "CF 1883D - Đang Yêu"
description: "Chúng ta được yêu cầu duy trì một tập hợp nhiều đoạn động trên trục số, xử lý các phép cộng và xóa từng đoạn một. Sau mỗi thao tác, chúng ta phải xác định xem tập hợp nhiều tập hợp có chứa ít nhất hai phân đoạn không trùng nhau hay không."
date: "2026-06-08T22:29:43+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1883
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 905 (Div. 3)"
rating: 1500
weight: 1883
solve_time_s: 135
verified: true
draft: false
---

[CF 1883D - Đang yêu](https://codeforces.com/problemset/problem/1883/D) 

**Đánh giá:** 1500 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 2m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu duy trì một tập hợp nhiều đoạn động trên trục số, xử lý các phép cộng và xóa từng đoạn một. Sau mỗi thao tác, chúng ta phải xác định xem tập hợp nhiều tập hợp có chứa ít nhất hai phân đoạn không trùng nhau hay không. Một cặp đoạn thẳng không cắt nhau nếu không có điểm nào trên trục số thuộc cả hai đoạn thẳng đó. 

Đầu vào bao gồm tối đa$10^5$các hoạt động, mỗi hoạt động thêm hoặc xóa một phân đoạn được xác định bởi các điểm cuối của nó$l$Và$r$, Ở đâu$1 \le l \le r \le 10^9$. Sau mỗi thao tác, chúng ta phải xuất "CÓ" nếu tồn tại một cặp đoạn không giao nhau và "KHÔNG" nếu ngược lại. 

Bởi vì$q$có thể lớn như$10^5$, bất kỳ thuật toán nào kiểm tra tất cả các cặp phân đoạn sau mỗi thao tác sẽ yêu cầu$O(q^2)$thời gian trong trường hợp xấu nhất, đó là$10^{10}$hoạt động và quá chậm. Điều này buộc chúng tôi chỉ duy trì thông tin cần thiết để trả lời truy vấn một cách hiệu quả thay vì quét lại tất cả các phân đoạn nhiều lần. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống tồn tại nhiều phân đoạn giống hệt nhau. Ví dụ: nếu nhiều tập hợp chứa hai bản sao của phân đoạn$(2, 4)$và một đoạn$(5, 6)$, câu trả lời vẫn phải là “CÓ” vì$(2, 4)$Và$(5, 6)$không cắt nhau, mặc dù$(2, 4)$chính nó xuất hiện hai lần. Một trường hợp cạnh khác là khi nhiều tập hợp chỉ chứa các phân đoạn giống hệt nhau hoặc chồng chéo; bất kỳ thuật toán nào không xem xét điểm cuối bên trái tối đa và điểm cuối bên phải tối thiểu có thể kết luận sai rằng tồn tại các phân đoạn không giao nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: sau mỗi lần thêm hoặc bớt, hãy kiểm tra tất cả các cặp phân đoạn để xem liệu chúng có giao nhau hay không. Điều này đúng nhưng đòi hỏi$O(n^2)$mỗi thao tác trong trường hợp xấu nhất. Với tối đa$10^5$phân khúc, điều này dẫn đến tối đa$10^{10}$hoạt động, điều đó là không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải kiểm tra từng cặp phân đoạn. Để xác định xem một cặp đoạn có giao nhau hay không, chỉ cần biết đoạn có điểm cuối bên trái nhỏ nhất và đoạn có điểm cuối bên phải lớn nhất là đủ. Nếu đoạn có điểm cuối bên trái tối đa lớn hơn đoạn có điểm cuối bên phải tối thiểu thì hai đoạn này không giao nhau. 

Để triển khai điều này một cách hiệu quả, chúng tôi duy trì điểm cuối bên trái tối thiểu hiện tại, điểm cuối bên trái tối đa, điểm cuối bên phải tối thiểu và điểm cuối bên phải tối đa hiện tại trên tất cả các phân đoạn, cùng với số lượng của chúng để xử lý các phân đoạn trùng lặp. Với phương pháp này, mỗi lần thêm hoặc bớt sẽ cập nhật bốn giá trị này theo$O(\log n)$thời gian sử dụng cấu trúc dữ liệu cân bằng hoặc`SortedDict`/`SortedList`từ Python`sortedcontainers`. Sau đó, chúng ta có thể kiểm tra các đoạn không giao nhau trong thời gian không đổi bằng cách so sánh`max_left`Và`min_right`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * n) | O(n) | Quá chậm | 
| Duy trì thái cực | O(q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bốn cấu trúc dữ liệu: một multiset hoặc bộ đếm cho điểm cuối bên trái, một cho điểm cuối bên phải. Theo dõi`min_left`,`max_left`,`min_right`,`max_right`động với số lượng trùng lặp. 
2. Đối với từng thao tác: 

- Nếu là phần bổ sung, hãy chèn điểm cuối bên trái và bên phải của đoạn vào các tập hợp tương ứng. 
- Nếu đó là loại bỏ, hãy xóa một lần xuất hiện của điểm cuối của phân đoạn khỏi nhiều tập hợp. 
3. Sau mỗi thao tác, hãy tính dòng điện`min_left`,`max_left`,`min_right`, Và`max_right`. 
4. Nếu`max_left`>`min_right`, xuất ra "CÓ" vì tồn tại ít nhất một cặp đoạn thẳng không giao nhau. Nếu không thì xuất ra "NO". 
5. Lặp lại cho tất cả các thao tác. 

Tại sao nó hoạt động: Bằng cách theo dõi các điểm cuối cuối cùng, chúng tôi đảm bảo rằng chúng tôi coi các phân đoạn có nhiều khả năng không chồng chéo nhất. Tính bất biến đó là`min_right`là điểm ngoài cùng bên phải mà bất kỳ đoạn nào cũng bắt đầu ở bên trái và`max_left`là điểm ngoài cùng bên trái mà bất kỳ đoạn nào cũng kéo dài sang bên phải. Nếu như`max_left`vượt quá`min_right`, có khoảng cách giữa các đoạn, đảm bảo không giao nhau. 

## Giải pháp Python```python
import sys
from collections import Counter
import bisect

input = sys.stdin.readline

class Multiset:
    def __init__(self):
        self.counter = Counter()
        self.sorted = []

    def add(self, x):
        if self.counter[x] == 0:
            bisect.insort(self.sorted, x)
        self.counter[x] += 1

    def remove(self, x):
        self.counter[x] -= 1
        if self.counter[x] == 0:
            idx = bisect.bisect_left(self.sorted, x)
            self.sorted.pop(idx)

    def min(self):
        return self.sorted[0]

    def max(self):
        return self.sorted[-1]

q = int(input())
lefts = Multiset()
rights = Multiset()
segments = Counter()

for _ in range(q):
    op, l, r = input().split()
    l = int(l)
    r = int(r)
    if op == '+':
        segments[(l,r)] += 1
        lefts.add(l)
        rights.add(r)
    else:
        segments[(l,r)] -= 1
        lefts.remove(l)
        rights.remove(r)
    if len(segments) < 2:
        print("NO")
        continue
    if lefts.max() > rights.min():
        print("YES")
    else:
        print("NO")
```các`Multiset`lớp sử dụng một`Counter`đối với các bản sao và danh sách được sắp xếp để truy xuất nhanh mức tối thiểu và tối đa. Mỗi lần thêm và bớt giữ cho dữ liệu nhất quán. Điều này tránh việc quét tất cả các phân đoạn và`max_left`Và`min_right`so sánh trả lời truy vấn trong thời gian không đổi. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu:```
+ 1 2
+ 3 4
+ 2 3
```| Hoạt động | Phân đoạn | phút_right | max_left | Đầu ra | 
| --- | --- | --- | --- | --- | 
| + 1 2 | {(1,2)} | 2 | 1 | KHÔNG | 
| + 3 4 | {(1,2),(3,4)} | 2 | 3 | CÓ | 
| + 2 3 | {(1,2),(3,4),(2,3)} | 2 | 3 | CÓ | 

Dấu vết này xác nhận sự bất biến:`max_left`>`min_right`chỉ ra một khoảng cách. 

Ví dụ khác:```
+ 2 2
+ 3 4
- 2 2
```| Hoạt động | Phân đoạn | phút_right | max_left | Đầu ra | 
| --- | --- | --- | --- | --- | 
| + 2 2 | {(2,2)} | 2 | 2 | KHÔNG | 
| + 3 4 | {(2,2),(3,4)} | 2 | 3 | CÓ | 
| - 2 2 | {(3,4)} | 4 | 3 | KHÔNG | 

Nó thể hiện việc xử lý chính xác việc loại bỏ phân đoạn và cập nhật đến mức cực đoan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | Mỗi lần chèn/xóa trong`Multiset`là O(log n) do`bisect`. Tối đa q = 10^5 thực hiện các thao tác 10^5 * log 10^5 ≈ 2*10^6. | 
| Không gian | O(n) | Mỗi phân đoạn được lưu trữ trong Bộ đếm và hai tập hợp nhiều tập, tồn tại tối đa q phân đoạn. | 

Điều này phù hợp thoải mái trong giới hạn 2 giây, 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    exec(open('solution.py').read())
    return out.getvalue().strip()

# provided sample
assert run("""12
+ 1 2
+ 3 4
+ 2 3
+ 2 2
+ 3 4
- 3 4
- 3 4
- 1 2
+ 3 4
- 2 2
- 2 3
- 3 4
""") == """NO
YES
YES
YES
YES
YES
NO
NO
YES
NO
NO
NO"""

# minimal input
assert run("""1
+ 1 1
""") == "NO", "single segment cannot have non-intersecting pair"

# identical segments
assert run("""3
+ 1 2
+ 1 2
+ 1 2
""") == "NO", "all identical, no non-intersecting pair"

# non-intersecting multiple
assert run("""4
+ 1 2
+ 3 4
+ 5 6
- 1 2
""") == """NO
YES
YES
YES""", "after removal, non-intersecting still exists"

# edge large numbers
assert run("""2
+ 1 1000000000
+ 500000000 1000000000
""") == """NO
NO""", "overlapping
```
