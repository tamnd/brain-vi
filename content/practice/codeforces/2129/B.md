---
title: "CF 2129B - Ở hoặc Gương"
description: "Chúng ta được cho một hoán vị các số nguyên từ 1 đến n. Đối với mỗi phần tử trong hoán vị này, chúng ta được phép giữ nguyên hoặc thay thế nó bằng “gương” của nó đối với 2n, được định nghĩa là $2n - pi$."
date: "2026-06-08T03:02:05+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dp", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2129
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1040 (Div. 1)"
rating: 1600
weight: 2129
solve_time_s: 98
verified: false
draft: false
---

[CF 2129B - Ở lại hoặc Gương](https://codeforces.com/problemset/problem/2129/B) 

**Đánh giá:** 1600 
**Tags:** bạo lực, cấu trúc dữ liệu, dp, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số nguyên từ 1 đến n. Đối với mỗi phần tử trong hoán vị này, chúng ta được phép giữ nguyên hoặc thay thế nó bằng “gương” của nó đối với 2n, được định nghĩa là$2n - p_i$. Nhiệm vụ của chúng ta là xây dựng một mảng mới sao cho số lần đảo ngược được giảm thiểu. Đảo ngược là một cặp chỉ số$i < j$trong đó phần tử đầu tiên lớn hơn phần tử thứ hai. Đầu ra là một số nguyên duy nhất, số lần đảo ngược tối thiểu có thể đạt được đối với hoán vị đã cho. 

Các ràng buộc cho chúng ta biết rằng n có thể lên tới 5000 và tổng của tất cả n trong các trường hợp thử nghiệm cũng nhiều nhất là 5000. Điều này có nghĩa là một giải pháp gần như bậc hai theo n là có thể chấp nhận được, vì 5000² là 25 triệu, có thể chạy trong vòng 2 giây. Các thuật toán có độ phức tạp O(n³) sẽ quá chậm. 

Một trường hợp tế nhị phát sinh khi các phần tử nằm ở ranh giới. Ví dụ, hãy xem xét hoán vị`[1, n]`. Việc chọn phản chiếu một trong hai phần tử có thể tạo ra hoặc không tạo ra sự đảo ngược tùy thuộc vào lựa chọn. Một kịch bản khó khăn khác là khi hoán vị đã giảm đi hoặc tăng lên một cách nghiêm ngặt. Một cách tiếp cận đơn giản luôn phản chiếu các phần tử lớn nhất có thể tạo ra sự đảo ngược thay vì loại bỏ chúng, do đó thuật toán phải xem xét vị trí tương đối của từng phần tử và tác động tích lũy của các lựa chọn trước đó. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là tạo ra tất cả 2ⁿ mảng có thể bằng cách giữ từng phần tử hoặc phản chiếu nó. Đối với mỗi mảng, đếm số lần đảo ngược và chọn mức tối thiểu. Việc đếm các nghịch đảo có thể được thực hiện trong O(n log n) bằng cách sử dụng cây Fenwick, nhưng 2ⁿ mảng có thể khiến phương pháp này không khả thi ngay cả với n=20, thấp hơn nhiều so với giới hạn của chúng tôi. 

Thông tin chi tiết quan trọng là đối với mỗi phần tử, chúng ta chỉ cần quyết định xem nó nằm ở “nửa dưới” (giá trị ban đầu của nó) hay “nửa trên” (giá trị phản chiếu của nó). Chúng ta có thể mô hình hóa điều này như một bài toán quy hoạch động: đặt dp[i][j] là số lần đảo ngược tối thiểu nếu chúng ta đặt i phần tử đầu tiên và có j phần tử ở nửa dưới. Vì giá trị phản ánh của mỗi phần tử có thể dự đoán được nên chúng ta có thể sắp xếp các giá trị ứng cử viên và sử dụng tìm kiếm nhị phân hoặc cây Fenwick để tính toán hiệu quả số lần đảo ngược khi thêm một phần tử mới. 

Cấu trúc của bài toán cho phép một cách tiếp cận tham lam cùng với việc tính toán cẩn thận các nghịch đảo bằng cách xem xét tiền tố của mảng được đặt cho đến nay. Đối với mỗi vị trí, chúng tôi quyết định xem việc lấy giá trị ban đầu hay giá trị phản chiếu sẽ làm tăng số lần đảo ngược ít hơn và chúng tôi có thể tích lũy số lần đảo ngược bằng cách sử dụng cây Fenwick. Quan sát quan trọng là chúng ta chỉ cần so sánh phần tử hiện tại với các phần tử đã được đặt, vì vậy chúng ta không cần liệt kê tất cả các hoán vị một cách rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2ⁿ·n log n) | O(n) | Quá chậm | 
| Tối ưu | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biến đổi từng phần tử$p_i$thành hai ứng cử viên:$p_i$Và$2n - p_i$. Lưu trữ chúng trong một loạt các cặp`(min_value, max_value)`sao cho cái nhỏ hơn được coi là “ở lại” và cái lớn hơn được coi là “tấm gương”. 
2. Khởi tạo cây Fenwick hoặc cấu trúc dữ liệu tương tự để theo dõi số phần tử đã được đặt nhỏ hơn một giá trị nhất định. Điều này cho phép đếm đảo ngược nhanh chóng khi chèn một phần tử mới. 
3. Lặp lại các phần tử trong hoán vị ban đầu. Đối với mỗi phần tử, hãy kiểm tra số lần đảo ngược sẽ xảy ra nếu chúng ta đặt giá trị “ở lại” so với giá trị “gương”. Số lượng đảo ngược là số phần tử đã được đặt lớn hơn giá trị ứng cử viên. 
4. Chọn tùy chọn (ở yên hoặc phản chiếu) tạo ra ít sự đảo ngược hơn. Chèn giá trị đã chọn này vào cây Fenwick để cập nhật số lượng các phần tử tiếp theo. 
5. Sau khi xử lý tất cả các phần tử, số lượng đảo ngược tích lũy là mức tối thiểu có thể đạt được. 

Tại sao nó hoạt động: tính bất biến là ở mỗi bước, thuật toán sẽ đặt phần tử một cách tham lam vào vị trí giúp giảm thiểu sự đóng góp của nó vào tổng số nghịch đảo cho tiền tố hiện tại. Vì chỉ có hai tùy chọn cho mỗi phần tử được biết và mang tính xác định, điều này đảm bảo rằng không có lựa chọn vị trí nào có thể được cải thiện cục bộ. Việc tích lũy các cực tiểu cục bộ này tạo ra mức tối thiểu toàn cầu vì các nghịch đảo có tính chất cộng gộp và chỉ phụ thuộc vào thứ tự tương đối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 2)
    def add(self, i, x):
        while i <= self.n + 1:
            self.bit[i] += x
            i += i & -i
    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

def min_inversions(n, p):
    vals = sorted([p[i] for i in range(n)] + [2*n - p[i] for i in range(n)])
    idx = {v:i+1 for i,v in enumerate(vals)}
    bit = Fenwick(2*n+2)
    ans = 0
    a = []
    for x in p:
        low, high = x, 2*n - x
        inv_low = bit.sum(2*n+1) - bit.sum(idx[low])
        inv_high = bit.sum(2*n+1) - bit.sum(idx[high])
        if inv_low <= inv_high:
            a.append(low)
            bit.add(idx[low], 1)
            ans += inv_low
        else:
            a.append(high)
            bit.add(idx[high], 1)
            ans += inv_high
    return ans 

t = int(input())
for _ in range(t):
    n = int(input())
    p = list(map(int, input().split()))
    print(min_inversions(n, p), end=" ")
```Cây Fenwick theo dõi xem có bao nhiêu số ít hơn một ứng cử viên nhất định đã được xếp vào. Chúng tôi ánh xạ tất cả các giá trị tiềm năng vào một chỉ mục nén vì các giá trị được phản chiếu có thể lên tới 2n. Ở mỗi bước, phần đóng góp đảo ngược được tính bằng số phần tử đã được đặt lớn hơn ứng cử viên. Mã chọn tùy chọn giảm thiểu sự đóng góp này và cập nhật cây. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hoán vị`[2, 1]`với n=2. Các cặp ứng cử viên là`(2, 2)`Và`(1, 3)`. 

| tôi | Ứng viên | Được chọn | Bang BIT | Đảo ngược | 
| --- | --- | --- | --- | --- | 
| 1 | 2 hoặc 2 | 2 | [0,0,1,0,0] | 0 | 
| 2 | 1 hoặc 3 | 3 | [0,0,1,0,1] | 0 | 

Số lần đảo ngược cuối cùng là 0, phù hợp với kết quả đầu ra dự kiến. 

### Mẫu 2 

Hoán vị`[2,1,3]`với n=3. Cặp ứng viên`(2,4)`,`(1,5)`,`(3,3)`. 

| tôi | Ứng viên | Được chọn | Bang BIT | Đảo ngược | 
| --- | --- | --- | --- | --- | 
| 1 | 2 hoặc 4 | 2 | [0,...,1,...] | 0 | 
| 2 | 1 hoặc 5 | 5 | [...1,...1...] | 1 | 
| 3 | 3 hoặc 3 | 3 | [...] | 1 | 

Số lần đảo ngược cuối cùng là 1, việc xác nhận lựa chọn tham lam sẽ tạo ra số lần đảo ngược tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi phần tử so sánh hai tùy chọn, đếm các nghịch đảo trong O(n) bằng cách sử dụng tổng Fenwick | 
| Không gian | O(n) | Lưu trữ mảng và cây Fenwick cho giá trị 2n | 

Giải pháp có tỷ lệ trong giới hạn của bài toán vì tổng n trên tất cả các trường hợp thử nghiệm là 5000. Các phép toán bậc hai trên tổng kích thước đầu vào là khoảng 25 triệu, khả thi trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        output.append(str(min_inversions(n, p)))
    return " ".join(output) + " "

assert run("5\n2\n2 1\n3\n2 1 3\n4\n4 3 2 1\n5\n2 3 1 5 4\n6\n2 3 4 1 5 6\n") == "0 1 0 2 2 ", "sample tests"
assert run("1\n2\n1 2\n") == "0 ", "already sorted"
assert run("1\n2\n2 1\n") == "0 ", "reverse order small"
assert run("1\n4\n1 3 2 4\n") == "1 ", "one inversion"
assert run("1\n3\n3 1 2\n") == "1 ", "middle inversion"
assert run("1\n6\n6 5 4 3 2 1\n") == "0 ", "mirror all"

| Test input | Expected output | What it validates |
|---|---|---|
|2 1|0|Correctly handles small reverse permutation|
|1 2|0|No inversions needed|
|1 3 2 4|
```
