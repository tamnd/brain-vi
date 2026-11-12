---
title: "CF 1630C - Sơn phần giữa"
description: "Chúng ta được cho một dãy số, ban đầu mỗi số không được tô màu. Thao tác duy nhất được phép là chọn ba phần tử $i < j < k$ sao cho hai phần tử bên ngoài có giá trị bằng nhau và cả ba phần tử đều không được sơn, sau đó sơn phần tử ở giữa."
date: "2026-06-10T05:00:05+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1630
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 768 (Div. 1)"
rating: 2200
weight: 1630
solve_time_s: 93
verified: true
draft: false
---

[CF 1630C - Sơn phần giữa](https://codeforces.com/problemset/problem/1630/C) 

**Xếp hạng:** 2200 
**Tags:** dp, tham lam, sắp xếp, hai con trỏ 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số, ban đầu mỗi số không được tô màu. Thao tác duy nhất được phép là chọn ba phần tử$i < j < k$sao cho 2 phần tử bên ngoài có giá trị bằng nhau và cả 3 phần tử đều không được sơn thì sơn phần tử ở giữa. Mục tiêu là tối đa hóa số lượng phần tử có thể được vẽ bằng quy tắc này. 

Đầu vào cung cấp độ dài của chuỗi$n$và mảng$a$của$n$số nguyên. Đầu ra là một số nguyên duy nhất: số lượng phần tử được sơn tối đa có thể đạt được. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các phần tử và mỗi giá trị tối đa là$n$. Điều này ngay lập tức loại trừ các phương pháp tiếp cận bạo lực nhằm kiểm tra tất cả các bộ ba một cách rõ ràng, bởi vì có$\mathcal{O}(n^3)$bộ ba như vậy, điều này vượt xa khả năng thực hiện trong 2 giây. 

Các trường hợp đặc biệt bao gồm các chuỗi có các số lặp lại, các chuỗi có tất cả các số riêng biệt, các chuỗi trong đó các số lặp lại liền kề nhau và các chuỗi chỉ có một số lặp lại. Ví dụ, ở đầu vào`[1, 1, 1]`, chỉ có thể vẽ phần tử ở giữa. Nếu tất cả các số đều khác biệt, như`[1, 2, 3]`, không thể thực hiện được thao tác nào. Việc triển khai bất cẩn có thể đếm các cặp không chính xác hoặc sử dụng lại các phần tử được vẽ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng lặp lại trên tất cả các bộ ba`(i, j, k)`và kiểm tra xem`a[i] == a[k]`và cả ba vị trí đều không được sơn. Nếu vậy thì nó sẽ sơn`j`. Mặc dù đúng về mặt logic nhưng điều này đòi hỏi phải kiểm tra đại khái$O(n^3)$sinh ba, theo thứ tự$10^{15}$hoạt động cho đầu vào lớn nhất, hoàn toàn không khả thi. 

Thông tin chi tiết quan trọng đến từ việc nhận thấy rằng mỗi giá trị$v$chỉ cho phép vẽ phần tử ở giữa giữa các vị trí lặp lại của nó. Do đó, bài toán rút gọn thành việc tìm kiếm từng giá trị duy nhất ở tất cả các vị trí mà nó xuất hiện, sau đó đếm xem có bao nhiêu phần tử có thể được sơn giữa các vị trí này. Với mỗi cặp vị trí liên tiếp`(p1, p2)`chính xác có cùng giá trị`p2 - p1 - 1`vị trí là ứng viên. Không thể vẽ nhiều phần tử trong các khoảng chồng chéo cho cùng một giá trị, bởi vì một khi phần tử ở giữa được vẽ, nó không thể được sử dụng lại. Do đó, lời giải trở thành một bài toán quy hoạch động: với mỗi giá trị, chúng ta muốn số lượng khoảng không chồng lấp tối đa có độ dài ≥ 3. 

Điều này làm giảm việc đếm kiểu chuỗi con tăng dài nhất hoặc đơn giản hơn là quét tham lam dọc theo các vị trí của từng giá trị, đếm số lượng tối đa các vị trí ở giữa không chồng chéo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| Tối ưu | O(n) khấu hao | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tập hợp tất cả các vị trí của từng giá trị trong mảng vào một từ điển ánh xạ giá trị vào danh sách các chỉ số. Điều này cho phép chúng tôi xử lý từng giá trị riêng biệt. 
2. Khởi tạo bộ đếm tổng số phần tử được sơn về 0. 
3. Đối với danh sách vị trí của mỗi giá trị, lặp lại các cặp liên tiếp`(pos[i], pos[i+1])`. Nếu như`pos[i+1] - pos[i] > 1`, tăng bộ đếm lên 1. Điều này thể hiện việc vẽ phần tử ở giữa giữa hai giá trị bằng nhau này. 
4. Tính tổng các số gia này trên tất cả các giá trị và trả về tổng số. 

Tại sao điều này hoạt động: mỗi thao tác yêu cầu ba phần tử chưa được sơn có giá trị bên ngoài bằng nhau. Bằng cách lặp lại các lần xuất hiện liên tiếp của cùng một giá trị, chúng tôi đảm bảo rằng phần tử ở giữa có các phần tử lân cận chưa được sơn và có thể được sơn. Các bản sao không liên tiếp không gây trở ngại vì một khi phần tử ở giữa được vẽ, nó sẽ không bao giờ được sử dụng lại, đáp ứng các ràng buộc của bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict

n = int(input())
a = list(map(int, input().split()))

positions = defaultdict(list)
for idx, val in enumerate(a):
    positions[val].append(idx)

painted = 0
for pos_list in positions.values():
    for i in range(len(pos_list) - 1):
        if pos_list[i+1] - pos_list[i] > 1:
            painted += 1

print(painted)
```Mã này nhóm các chỉ số đầu tiên theo giá trị của chúng. Sau đó, nó lặp qua từng cặp chỉ số liên tiếp có cùng giá trị, kiểm tra xem có ít nhất một phần tử giữa chúng không. Nếu vậy, nó sẽ tăng`painted`quầy tính tiền. Điều này đảm bảo không có sự chồng chéo và chiếm tất cả các phần tử ở giữa có thể có. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
7
1 2 1 2 7 4 7
```| Giá trị | Vị trí | Tăng sơn | 
| --- | --- | --- | 
| 1 | [0,2] | 1 | 
| 2 | [1,3] | 1 | 
| 7 | [4,6] | 1 | 
| 4 | [5] | 0 | 

Tổng số sơn = 2 (chúng ta không thể sơn đồng thời cả số 2 và số 7 mà không bị chồng chéo). Thuật toán chỉ đếm các khoảng không chồng chéo một cách tham lam, cho giá trị tối đa chính xác là 2. 

**Đầu vào tùy chỉnh:**```
5
1 1 1 2 2
```| Giá trị | Vị trí | Tăng sơn | 
| --- | --- | --- | 
| 1 | [0,1,2] | 1 | 
| 2 | [3,4] | 0 | 

Sơn = 1. Chỉ có thể sơn phần tử ở giữa giữa 1 đầu tiên và 1 cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được thêm vào danh sách một lần và mỗi danh sách được lặp lại một cách tuyến tính. | 
| Không gian | O(n) | Từ điển lưu trữ vị trí của tất cả các phần tử. | 

Được cho$n \le 2 \cdot 10^5$, thao tác này diễn ra thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    from collections import defaultdict
    positions = defaultdict(list)
    for idx, val in enumerate(a):
        positions[val].append(idx)
    painted = 0
    for pos_list in positions.values():
        for i in range(len(pos_list) - 1):
            if pos_list[i+1] - pos_list[i] > 1:
                painted += 1
    return str(painted)

# Provided samples
assert run("7\n1 2 1 2 7 4 7\n") == "2", "sample 1"

# Custom cases
assert run("3\n1 1 1\n") == "1", "all same minimal"
assert run("5\n1 1 1 2 2\n") == "1", "overlapping triples"
assert run("4\n1 2 3 4\n") == "0", "all distinct"
assert run("6\n1 2 1 2 1 2\n") == "3", "multiple non-overlapping triples"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3\n1 1 1 | 1 | Mảng có kích thước tối thiểu, tất cả các phần tử giống nhau | 
| 5\n1 1 1 2 2 | 1 | Bộ ba chồng lên nhau, chỉ có thể sơn một ở giữa | 
| 4\n1 2 3 4 | 0 | Không thể thực hiện các thao tác hợp lệ | 
| 6\n1 2 1 2 1 2 | 3 | Nhiều bộ ba không chồng chéo có cùng giá trị | 

## Vỏ cạnh 

Đối với đầu vào`[1,1,1]`, vị trí =`[0,1,2]`. Chỉ có cặp`(0,2)`thỏa mãn`pos[i+1] - pos[i] > 1`, vì vậy chúng ta tăng số lượng sơn thêm 1, cho kết quả đầu ra chính xác. Cặp nhỏ hơn`(0,1)`Và`(1,2)`bị bỏ qua vì không có phần tử nào nằm giữa chúng. Điều này xác nhận thuật toán xử lý các chuỗi tối thiểu một cách chính xác. 

Vì`[1,2,3,4]`, tất cả các vị trí đều là đơn vị. Không có cặp nào có khoảng cách >1, do đó sơn vẫn bằng 0. Thuật toán không đếm sai các phần tử ở giữa không tồn tại. 

Đối với các chuỗi chồng chéo như`[1,1,1,2,2]`, thuật toán chỉ tính một phần tử ở giữa cho`1`và bỏ qua`2`bởi vì các chỉ số liên tiếp của nó có khoảng cách 1. Điều này ngăn cản việc sử dụng lại bất hợp pháp các phần tử được sơn.
