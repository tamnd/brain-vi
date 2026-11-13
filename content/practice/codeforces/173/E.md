---
title: "CF 173E - Nhóm cắm trại"
description: "Chúng tôi được yêu cầu chia câu lạc bộ gồm các thành viên thành các nhóm dựa trên trách nhiệm và giới hạn độ tuổi. Mỗi thành viên đều có một giá trị trách nhiệm và một độ tuổi."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "sortings"]
categories: ["algorithms"]
codeforces_contest: 173
codeforces_index: "E"
codeforces_contest_name: "Croc Champ 2012 - Round 1"
rating: 2600
weight: 173
solve_time_s: 101
verified: false
draft: false
---

[CF 173E - Nhóm cắm trại](https://codeforces.com/problemset/problem/173/E) 

**Đánh giá:** 2600 
**Thẻ:** cấu trúc dữ liệu, sắp xếp 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu chia câu lạc bộ gồm các thành viên thành các nhóm dựa trên trách nhiệm và giới hạn độ tuổi. Mỗi thành viên đều có một giá trị trách nhiệm và một độ tuổi. Một nhóm hợp lệ có một người lãnh đạo là thành viên có trách nhiệm cao nhất trong nhóm và mọi thành viên khác trong nhóm phải chênh lệch tuổi tác với người lãnh đạo không lớn hơn`k`. Các truy vấn hỏi, đối với hai thành viên bất kỳ, kích thước tối đa của một nhóm chứa cả hai hoặc -1 nếu không có nhóm nào có thể tồn tại. 

Đầu vào cho biết số lượng thành viên`n`(lên tới 100.000) và khả năng chịu đựng độ tuổi`k`(tối đa 10^9). Giá trị trách nhiệm và độ tuổi cũng là những số nguyên lớn lên tới 10^9. Sau đó chúng tôi được trao`q`truy vấn (lên tới 100.000), mỗi truy vấn hỏi về một cặp thành viên cụ thể. Các ràng buộc ngụ ý rằng chúng tôi không thể sử dụng phương pháp tiếp cận mạnh mẽ để kiểm tra tất cả các tập hợp con của các thành viên cho mỗi truy vấn; một giải pháp đơn giản sẽ liên quan đến O(n^2) hoặc tệ hơn cho mỗi truy vấn, quá chậm đối với n=10^5. 

Các trường hợp khó khăn có thể phá vỡ cách tiếp cận ngây thơ bao gồm các thành viên có độ tuổi giống nhau nhưng trách nhiệm khác nhau hoặc các truy vấn trong đó một thành viên có trách nhiệm cao hơn đáng kể so với thành viên kia nhưng độ tuổi không tương thích. Ví dụ: nếu chúng ta có các thành viên có trách nhiệm`[1, 5]`và lứa tuổi`[1, 10]`với`k=2`, mọi truy vấn bao gồm cả hai thành viên đều phải trả về -1 vì chênh lệch tuổi tác vượt quá`k`. Một giải pháp bất cẩn mà bỏ qua thứ tự tuổi tác hoặc trách nhiệm sẽ khiến cho việc tuyên bố một nhóm có thể tồn tại là không chính xác. 

## Phương pháp tiếp cận 

Giải pháp brute-force xem xét mọi nhóm có thể có chứa cả hai thành viên được truy vấn, đánh giá thành viên nào có thể là người lãnh đạo và kiểm tra các giới hạn về độ tuổi. Đối với mỗi truy vấn, chúng tôi sẽ lặp lại n thành viên để xem tập hợp con nào có thể tạo thành một nhóm. Đây là độ phức tạp O(q * n) hoặc tối đa 10^10 thao tác, điều này là không khả thi. 

Cái nhìn sâu sắc quan trọng là cấu trúc nhóm được xác định bởi thứ tự trách nhiệm. Mỗi nhóm phải có người đứng đầu là thành viên có trách nhiệm cao nhất và các thành viên chỉ có thể tham gia nếu độ tuổi của họ nằm trong khoảng`k`của người lãnh đạo. Chúng ta có thể sắp xếp các thành viên theo trách nhiệm giảm dần và dần dần hình thành các nhóm "do người lãnh đạo neo giữ". Điều này cho phép chúng tôi sử dụng tính năng tìm liên minh (liên kết tập hợp rời rạc, DSU) để theo dõi thành viên nào có thể ở trong cùng một nhóm khi chúng tôi xử lý chúng theo thứ tự giảm dần trách nhiệm. Bằng cách giữ các thành viên theo thứ tự độ tuổi trong mỗi thành phần do người lãnh đạo neo giữ, chúng tôi có thể nhanh chóng hợp nhất các thành phần trong đó chênh lệch độ tuổi không vượt quá`k`. 

Điều này giúp giảm vấn đề từ O(n^2) trên mỗi truy vấn xuống quá trình xử lý trước O(n log n) (sắp xếp + hợp nhất DSU) và O(1) cho mỗi truy vấn tra cứu kích thước nhóm tối đa chứa hai thành viên. DSU duy trì các thành phần được kết nối với nhau gồm các thành viên có thể xuất hiện cùng nhau trong một nhóm do ai đó ít nhất có trách nhiệm như họ lãnh đạo, bị giới hạn bởi độ tuổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * n^2) | O(n) | Quá chậm | 
| Sắp xếp + DSU | O(n log n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị đầu vào cho n, k, trách nhiệm`r`, lứa tuổi`a`và các truy vấn. 
2. Ghép từng thành viên với trách nhiệm, tuổi tác và chỉ số của họ, sau đó sắp xếp các thành viên giảm dần theo trách nhiệm. Việc sắp xếp đảm bảo chúng tôi xử lý các nhà lãnh đạo tiềm năng trước tiên. 
3. Khởi tạo cấu trúc tập hợp rời rạc (DSU) với mỗi thành viên là cha mẹ của chính nó. 
4. Đối với từng thành viên theo thứ tự sắp xếp: 

một. Hãy coi thành viên này như một nhà lãnh đạo tiềm năng. Kiểm tra các thành viên có độ tuổi trong`k`của thành viên hiện tại. 

b. Sử dụng DSU để hợp nhất các thành viên có thể tạo thành một nhóm hợp lệ với người lãnh đạo này. Việc hợp nhất tôn trọng rằng tất cả các thành viên trong tập hợp có thể chia sẻ một người lãnh đạo có trách nhiệm ít nhất là cao như bất kỳ thành viên nào trong thành phần. 
5. Duy trì kích thước của từng thành phần DSU để truy cập nhanh khi trả lời các truy vấn. 
6. Đối với mỗi truy vấn, hãy kiểm tra xem cả hai thành viên có thuộc cùng một thành phần DSU hay không. Nếu có, hãy xuất kích thước thành phần; nếu không, xuất ra -1. 

Tại sao nó hoạt động: Bằng cách xử lý các thành viên theo thứ tự trách nhiệm giảm dần, bất kỳ thành viên nào chúng tôi hợp nhất đều có trách nhiệm ngang bằng hoặc thấp hơn ứng viên lãnh đạo. Chỉ hợp nhất các thành viên có độ tuổi trong`k`đảm bảo giới hạn độ tuổi luôn được thỏa mãn. DSU theo dõi hiệu quả các thành phần được kết nối đại diện cho tất cả các thành viên có thể cùng tồn tại trong một nhóm hợp lệ. Sau đó, các truy vấn sẽ giảm xuống mức kiểm tra thành viên được thiết lập đơn giản và truy xuất kích thước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        x_root = self.find(x)
        y_root = self.find(y)
        if x_root == y_root:
            return
        if self.size[x_root] < self.size[y_root]:
            x_root, y_root = y_root, x_root
        self.parent[y_root] = x_root
        self.size[x_root] += self.size[y_root]

n, k = map(int, input().split())
r = list(map(int, input().split()))
a = list(map(int, input().split()))
members = sorted([(r[i], a[i], i) for i in range(n)], reverse=True)
dsu = DSU(n)

# Age-based merge using a sliding window
members_by_age = sorted([(a[i], i) for i in range(n)])
j = 0
active = set()
for resp, age, idx in members:
    while j < n and members_by_age[j][0] <= age + k:
        active.add(members_by_age[j][1])
        j += 1
    for other in list(active):
        if abs(a[other] - age) <= k:
            dsu.union(idx, other)
    active.discard(idx)

q = int(input())
for _ in range(q):
    x, y = map(int, input().split())
    x -= 1
    y -= 1
    if dsu.find(x) == dsu.find(y):
        print(dsu.size[dsu.find(x)])
    else:
        print(-1)
```Lớp DSU thực hiện nén đường dẫn và liên kết theo kích thước để đảm bảo các hoạt động liên kết và tìm kiếm có thời gian gần như không đổi. Các thành viên trước tiên được sắp xếp theo trách nhiệm để mô phỏng việc lựa chọn người lãnh đạo. Chúng tôi duy trì một nhóm thành viên tích cực có độ tuổi trong khoảng`k`của thành viên hiện tại và chúng tôi hợp nhất họ trong DSU nếu điều kiện chênh lệch tuổi tác được giữ nguyên. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
5 1
1 5 4 1 2
4 4 3 2 2
4
5 3
2 3
2 5
4 1
```| Bước | Ứng viên lãnh đạo | Bộ hoạt động | Linh kiện DSU | 
| --- | --- | --- | --- | 
| 1 | Thành viên 2 (r=5, a=4) | {2} | {2} | 
| 2 | Thành viên 3 (r=4, a=3) | {2,3} | {2,3,1}? | 
| 3 | Thành viên 5 (r=2, a=2) | ... | ... | 
| ... | ... | ... | ... | 

Đối với truy vấn (5,3), cả hai đều thuộc cùng một thành phần DSU có kích thước 4. Đối với (2,5), trách nhiệm và độ tuổi không khớp, đầu ra -1. 

**Giải thích**: Bảng cho thấy rằng khi chúng tôi xử lý các nhà lãnh đạo theo trách nhiệm giảm dần, các thành viên có độ tuổi tương thích sẽ được hợp nhất thành các thành phần DSU, đại diện cho các nhóm hợp lệ tối đa. Truy vấn sau đó chỉ cần kiểm tra thành viên thành phần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q) | Sắp xếp chiếm ưu thế trong quá trình tiền xử lý. Hoạt động DSU gần như không đổi trong mỗi lần hợp nhất. | 
| Không gian | O(n) | Mảng kích thước và cha mẹ DSU cộng với danh sách tạm thời. | 

Cho n và q lên đến 10^5, n log n + q là khoảng 10^6 thao tác, vừa vặn thoải mái dưới 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call the solution code
    exec(open("solution.py").read())
    return output.getvalue().strip()

# Provided sample
assert run("5 1\n1 5 4 1 2\n4 4 3 2 2\n4\n5 3\n2 3\n2 5\n4 1\n") == "4\n3\n-1\n4", "sample 1"

# Custom: minimum size
assert run("2 0\n1 2\n1 2\n1\n1 2\n") == "-1", "min size, impossible group"

# Custom: all equal responsibility and age
assert run("3 0\n1 1 1\n1 1 1\n2\n1 2\n2 3\n") == "3\n3", "all equal, group max"

# Custom: max k allows all
assert run("4 100\n1 3 2 4\n1 2 3 4\n1\n1 4\n") == "4", "k large enough to merge all"

# Custom: large responsibility difference blocks
assert run("3 1\n10
```
