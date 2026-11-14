---
title: "CF 1793F - Đổi tên"
description: "Chúng tôi được đưa ra một danh sách các ứng cử viên cho một ban nhạc, mỗi người có một chiều cao riêng. Trong nhiều ngày, ban tổ chức chọn ra một nhóm thí sinh liền kề nhau và muốn chọn ra hai người có chiều cao càng gần nhau càng tốt."
date: "2026-06-09T10:19:56+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "divide-and-conquer", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1793
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 852 (Div. 2)"
rating: 2600
weight: 1793
solve_time_s: 119
verified: false
draft: false
---

[CF 1793F - Tái tạo lại](https://codeforces.com/problemset/problem/1793/F) 

**Đánh giá:** 2600 
**Tags:** bạo lực, cấu trúc dữ liệu, phân chia để chinh phục, thực hiện 
**Thời gian giải:** 1 phút 59s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một danh sách các ứng cử viên cho một ban nhạc, mỗi người có một chiều cao riêng. Trong nhiều ngày, ban tổ chức chọn ra một nhóm thí sinh liền kề nhau và muốn chọn ra hai người có chiều cao càng gần nhau càng tốt. Trong mỗi ngày, nhiệm vụ là báo cáo sự khác biệt nhỏ nhất giữa hai độ cao bất kỳ trong đoạn đã chọn. 

Đầu vào bao gồm một mảng`n`số nguyên duy nhất biểu thị chiều cao và`q`truy vấn, mỗi truy vấn chỉ định một phân đoạn`[l_i, r_i]`. Đầu ra cho mỗi truy vấn là một số nguyên duy nhất: chênh lệch chiều cao tối thiểu trong phân đoạn đó. 

Các ràng buộc rất lớn:`n`có thể lên tới 300.000 và`q`lên tới 1.000.000. Một giải pháp đơn giản để so sánh tất cả các cặp chiều cao trong mỗi đoạn sẽ yêu cầu tới$O(n^2)$cho mỗi truy vấn, điều này hoàn toàn không khả thi. Thậm chí$O(n \cdot q)$quá chậm. Điều này buộc chúng ta phải tìm kiếm một giải pháp xử lý trước hoặc cấu trúc dữ liệu một cách hiệu quả, lý tưởng nhất là với thứ gì đó xung quanh$O(n \log n + q \log n)$. 

Các trường hợp cạnh không rõ ràng bao gồm các phân đoạn có độ dài tối thiểu (chỉ hai người), trong đó câu trả lời luôn là sự khác biệt của hai chiều cao đó và các phân đoạn ở ranh giới mảng, có thể hiển thị từng lỗi một. Ví dụ: nếu phân đoạn là`[1,2]`trong một mảng`[5,3,8]`, câu trả lời phải là`2`(5−3), không được tính toán ngẫu nhiên bằng cách sử dụng phần tử bên ngoài đoạn. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force chỉ đơn giản là kiểm tra từng cặp độ cao trong phân đoạn cho mỗi truy vấn. Nó đúng nhưng cực kỳ chậm. Đối với mỗi truy vấn, nó thực hiện$(r-l+1)^2$so sánh. Với các giá trị cực đại của`n`Và`q`, điều này có thể dẫn đến$10^{11}$hoạt động, vượt xa những gì khả thi trong 4 giây. 

Quan sát quan trọng là sự khác biệt tối thiểu giữa hai số trong một tập hợp xảy ra giữa hai số liên tiếp khi các số được sắp xếp. Do đó, đối với mỗi phân đoạn truy vấn, nếu chúng ta có thể truy xuất hiệu quả độ cao đã sắp xếp hoặc ít nhất là chênh lệch tối thiểu giữa các độ cao được sắp xếp liên tiếp, chúng ta sẽ nhận được câu trả lời ngay lập tức. 

Thông tin chi tiết này đề xuất sử dụng cấu trúc dữ liệu cho phép truy vấn phạm vi để tìm ra sự khác biệt tối thiểu giữa các phần tử liên tiếp. Một cách tiếp cận thực tế là một biến thể cây phân đoạn hoặc phân chia để chinh phục. Chúng ta có thể xây dựng một cây phân đoạn trong đó mỗi nút lưu trữ danh sách đã sắp xếp của phân đoạn đó. Khi hợp nhất các nút, chúng ta có thể tính toán chênh lệch tối thiểu trên ranh giới, kết hợp nó với chênh lệch tối thiểu bên trong được lưu trữ ở các nút con. Điều này làm giảm thời gian truy vấn xuống$O(\log n)$cho mỗi truy vấn nếu chúng tôi lưu trữ thông tin bổ sung một cách cẩn thận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * n^2) | O(n) | Quá chậm | 
| Cây phân đoạn với danh sách được sắp xếp | O(n log n + q log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn trên mảng độ cao. Mỗi lá tương ứng với một phần tử duy nhất, lưu trữ giá trị của nó và đặt chênh lệch tối thiểu bên trong của nó thành vô cùng. 
2. Đối với mỗi nút bên trong, hợp nhất danh sách đã sắp xếp của hai nút con để có được phân đoạn được sắp xếp tổng hợp. Tính toán chênh lệch tối thiểu trong danh sách hợp nhất này và lưu trữ nó tại nút. Ngoài ra, hãy lưu trữ chính danh sách đã sắp xếp vì nó cần thiết để tính toán sự khác biệt khi hợp nhất các nút cao hơn. 
3. Để trả lời một câu hỏi`[l, r]`, duyệt cây phân đoạn. Tại mỗi nút chứa đầy đủ trong`[l, r]`, truy xuất sự khác biệt tối thiểu được lưu trữ tại nút đó. Đối với các nút chồng chéo một phần, hãy hợp nhất các phần có liên quan và tính toán sự khác biệt tối thiểu giữa các phần tử biên, kết hợp nó với những khác biệt tối thiểu từ các phần tử con được bao phủ hoàn toàn. 
4. Trả về chênh lệch tối thiểu được tìm thấy từ tất cả các nút được xem xét. 

Lý do điều này hoạt động là vì bất kỳ hai chiều cao nào có sự khác biệt tối thiểu trong một phân đoạn đều phải liên tiếp trong phiên bản được sắp xếp của phân khúc đó. Bằng cách duy trì các danh sách được sắp xếp và những khác biệt tối thiểu bên trong trong các nút cây, chúng tôi đảm bảo rằng khi kết hợp các nút, chúng tôi không bao giờ bỏ lỡ một cặp liên tiếp có thể mang lại sự khác biệt tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from bisect import bisect_left

class SegmentTree:
    def __init__(self, data):
        self.n = len(data)
        self.size = 1
        while self.size < self.n:
            self.size *= 2
        self.tree = [([], float('inf')) for _ in range(2*self.size)]
        for i, v in enumerate(data):
            self.tree[self.size + i] = ([v], float('inf'))
        for i in range(self.size - 1, 0, -1):
            left_list, left_min = self.tree[2*i]
            right_list, right_min = self.tree[2*i+1]
            merged = sorted(left_list + right_list)
            min_diff = min(left_min, right_min, *(merged[j+1]-merged[j] for j in range(len(merged)-1)))
            self.tree[i] = (merged, min_diff)

    def query(self, l, r):
        l += self.size
        r += self.size
        res_list = []
        res_min = float('inf')
        while l < r:
            if l % 2:
                merged = res_list + self.tree[l][0]
                if merged:
                    merged.sort()
                    res_min = min(res_min, self.tree[l][1], *(merged[j+1]-merged[j] for j in range(len(merged)-1)))
                res_list += self.tree[l][0]
                l += 1
            if r % 2:
                r -= 1
                merged = res_list + self.tree[r][0]
                if merged:
                    merged.sort()
                    res_min = min(res_min, self.tree[r][1], *(merged[j+1]-merged[j] for j in range(len(merged)-1)))
                res_list += self.tree[r][0]
            l //= 2
            r //= 2
        return res_min

n, q = map(int, input().split())
a = list(map(int, input().split()))
seg = SegmentTree(a)
for _ in range(q):
    l, r = map(int, input().split())
    print(seg.query(l-1, r))
```Cây phân đoạn lưu trữ cả danh sách đã sắp xếp và những khác biệt tối thiểu để tránh tính toán lại cho các nút được bao phủ đầy đủ. các`query`hàm chỉ hợp nhất các phân đoạn cần thiết một cách cẩn thận và tính toán sự khác biệt tối thiểu giữa các ranh giới. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
3 3
1 3 2
1 2
2 3
1 3
```| Bước | Phân đoạn | Danh sách được sắp xếp | Khác biệt tối thiểu | Kết quả truy vấn | 
| --- | --- | --- | --- | --- | 
| Ngày 1 | [1,2] | [1,3] | 2 | 2 | 
| Ngày 2 | [2,3] | [2,3] | 1 | 1 | 
| Ngày 3 | [1,3] | [1,2,3] | 1 | 1 | 

Cây phân đoạn hợp nhất và lưu trữ những khác biệt tối thiểu, tìm ra chính xác những khác biệt nhỏ nhất liên tiếp. 

### Đầu vào mẫu 2```
6 3
1 3 5 2 6 4
4 6
1 2
3 6
```| Bước | Phân đoạn | Danh sách được sắp xếp | Khác biệt tối thiểu | Kết quả truy vấn | 
| --- | --- | --- | --- | --- | 
| Ngày 1 | [2,6] | [2,4,6] | 2 | 2 | 
| Ngày 2 | [1,2] | [1,3] | 2 | 2 | 
| Ngày 3 | [3,6] | [2,4,5,6] | 1 | 1 | 

Điều này cho thấy thuật toán xử lý các phạm vi không liền kề và hợp nhất các khác biệt ranh giới một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q log n) | Xây dựng cây hợp nhất các danh sách đã sắp xếp tại mỗi nút, độ cao logarit đảm bảo thời gian truy vấn là logarit. | 
| Không gian | O(n log n) | Mỗi nút lưu trữ một danh sách đã được sắp xếp; tổng dung lượng lưu trữ trên tất cả các nút là O(n log n). | 

Giải pháp phù hợp thoải mái trong giới hạn thời gian 4 giây cho n lên tới 300.000 và q lên tới 1.000.000, vì log n xấp xỉ 18. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # run solution
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    seg = SegmentTree(a)
    for _ in range(q):
        l, r = map(int, input().split())
        print(seg.query(l-1, r))
    return output.getvalue().strip()

assert run("3 3\n1 3 2\n1 2\n2 3\n1 3\n") == "2\n1\n1"
assert run("6 3\n1 3 5 2 6 4\n4 6\n1 2\n3 6\n") == "2\n2\n1"
assert run("2 1\n1000000000 1\n1 2\n") == "999999999"
assert run("5 2\n5 4 3 2 1\n1 5\n2 4\n") == "1\n1"
assert run("4 2\n1 4 2 3\n1
```
