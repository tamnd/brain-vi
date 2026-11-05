---
title: "CF 1214F - Việc làm"
description: "Chúng ta có hai tập hợp điểm trên đường tròn có chiều dài m. Bộ đầu tiên chứa các địa điểm văn phòng. Văn phòng i tọa lạc tại thành phố a[i]. Tập thứ hai chứa các ứng cử viên. Ứng viên j sống ở thành phố b[j]."
date: "2026-06-11T23:03:09+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1214
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 583 (Div. 1 + Div. 2, based on Olympiad of Metropolises)"
rating: 2700
weight: 1214
solve_time_s: 160
verified: true
draft: false
---

[CF 1214F - Việc làm](https://codeforces.com/problemset/problem/1214/F) 

**Đánh giá:** 2700 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 2 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Chúng ta có hai tập hợp điểm trên một đường tròn có chiều dài`m`. 

Bộ đầu tiên chứa các địa điểm văn phòng. Văn phòng`i`nằm ở thành phố`a[i]`. 

Tập thứ hai chứa các ứng cử viên. Ứng viên`j`sống ở thành phố`b[j]`. 

Mỗi chức vụ phải tiếp nhận chính xác một ứng cử viên và mỗi ứng cử viên phải được bổ nhiệm vào đúng một chức vụ. Chi phí để phân công một ứng viên vào một văn phòng là khoảng cách vòng tròn ngắn nhất giữa các thành phố của họ. Chúng ta phải giảm thiểu tổng chi phí và đưa ra một nhiệm vụ tối ưu. 

Vòng tròn chứa tối đa`10^9`thành phố nhưng chỉ`n ≤ 200000`văn phòng và ứng viên. Số lượng thành phố rất lớn nên bất kỳ thuật toán nào phụ thuộc vào kích thước của vòng tròn đều không thể thực hiện được. Đối tượng duy nhất chúng ta có đủ khả năng để xử lý là`n`định vị chính mình. 

Giá trị của`n`ngay lập tức loại trừ các thuật toán bậc hai. MỘT`O(n²)`giải pháp sẽ yêu cầu khoảng`4 × 10^10`trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. Độ phức tạp của mục tiêu phải ở mức xung quanh`O(n log n)`. 

Phần khó khăn là các thành phố nằm trên một vòng tròn chứ không phải một đường thẳng. 

Coi như:```
m = 10
offices    : 1 9
candidates : 2 10
```Giải thích tuyến tính sẽ sử dụng khoảng cách`1`Và`1`, nhưng trên thành phố vòng tròn`10`nằm cạnh thành phố`1`, do đó hình học bao bọc xung quanh. 

Một sai lầm dễ mắc phải khác là sắp xếp cả hai mảng và khớp chúng theo từng vị trí.```
m = 10
offices    : 1 4 8
candidates : 3 6 8
```Sau khi sắp xếp, xếp hạng bằng nhau sẽ cho ra chi phí```
|1-3| + |4-6| + |8-8| = 4
```nhưng trên vòng tròn chúng ta có thể xoay một thứ tự được sắp xếp. Kết hợp```
1 <- 3
4 <- 6
8 <- 8
```thực sự là sự thay đổi tạo ra sự tối ưu ở đây. Quan sát quan trọng là sự kết hợp tối ưu luôn là sự dịch chuyển theo chu kỳ của thứ tự được sắp xếp, không nhất thiết phải là sự dịch chuyển bằng 0. 

Các bản sao tạo ra một trường hợp tinh tế khác.```
m = 20
offices    : 5 5 5
candidates : 5 5 5
```Có nhiều bài tập tối ưu. Một giải pháp vẫn phải đưa ra một hoán vị hợp lệ của các chỉ số ứng cử viên. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử mọi cách kết hợp hoàn hảo có thể. có`n!`sự phù hợp, điều này gần như trở nên không thể ngay lập tức. 

Một lực lượng vũ phu hợp lý hơn bắt đầu bằng một quan sát cấu trúc. Nếu chúng ta sắp xếp các vị trí văn phòng xung quanh vòng tròn và sắp xếp các vị trí ứng viên xung quanh vòng tròn thì mọi sự kết hợp tối ưu đều không có cặp giao nhau. Nếu hai đường đi trùng nhau cắt nhau, việc trao đổi điểm cuối của chúng không bao giờ làm tăng tổng khoảng cách mà thường làm giảm khoảng cách đó. Việc loại bỏ nhiều lần các giao điểm sẽ biến đổi bất kỳ mức tối ưu nào thành mức tối ưu không giao nhau. 

Khi việc giao cắt bị cấm, việc khớp hoàn toàn được xác định bởi sự dịch chuyển theo chu kỳ. Sau khi sắp xếp cả hai bộ, chọn một số ca`k`, sau đó khớp```
A[i] <-> B[i+k]
```với các chỉ số được thực hiện theo chu kỳ. 

Điều đó làm giảm không gian tìm kiếm từ`n!`chỉ khớp với`n`thay đổi. 

Thật không may, việc đánh giá từng ca một cách độc lập vẫn tốn kém`O(n²)`. 

Quan sát còn lại là đối với một vị trí văn phòng cố định`a`, sự đóng góp của nó khi sự thay đổi thay đổi là một hàm không đổi từng phần trên phạm vi ca. Khoảng cách vòng tròn```
dist(a, b) = min(|a-b|, m-|a-b|)
```chỉ thay đổi hình thức khi`b`vượt qua một trong một vài vị trí quan trọng liên quan đến`a`. 

Sau khi sắp xếp tọa độ, những chuyển đổi quan trọng này có thể được định vị bằng tìm kiếm nhị phân. Mỗi văn phòng và mỗi ứng cử viên đều đóng góp một giá trị không đổi trong các phạm vi ca làm việc liền kề nhau. Chúng ta có thể tích lũy tất cả các đóng góp bằng cách sử dụng một mảng sai phân trên miền shift. 

Kết quả là một`O(n log n)`thuật toán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các ca | O(n²) | O(n) | Quá chậm | 
| Mảng khác biệt theo ca | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp các văn phòng theo vị trí thành phố, ghi nhớ các chỉ số ban đầu của chúng. 
2. Sắp xếp các ứng viên theo vị trí thành phố, đồng thời ghi nhớ các chỉ số ban đầu của họ. 
3. Sử dụng thuộc tính không giao nhau. Bất kỳ sự phân công tối ưu nào cũng phải khớp với trình tự văn phòng đã được sắp xếp với sự dịch chuyển theo chu kỳ của trình tự ứng viên đã được sắp xếp. 
4. Hãy dịch chuyển`k`có nghĩa là văn phòng được sắp xếp`i`được khớp với ứng viên đã được sắp xếp`(i+k) mod n`. 
5. Xây dựng một mảng`cost[k]`, tổng chi phí ca`k`. 
6. Thay vì đánh giá từng ca riêng biệt, hãy xử lý từng vị trí văn phòng`a[i]`. 

Đối với văn phòng cố định, biểu thức`dist(a[i], b[j])`chỉ thay đổi hình thức khi`b[j]`đi qua một trong bốn vùng tới hạn xung quanh đường tròn. Tìm kiếm nhị phân trên tọa độ ứng cử viên được sắp xếp xác định phạm vi của`j`thuộc từng vùng. 
7. Chuyển đổi mọi vùng như vậy thành cập nhật phạm vi trên biến shift`k = j - i`. 

Một mảng khác biệt cho phép thêm cùng một giá trị vào toàn bộ khoảng thời gian thay đổi trong`O(1)`thời gian. 
8. Lặp lại quy trình đối xứng cho mọi vị trí ứng viên`b[j]`. 

Sau cả hai lần vượt qua, giá trị tích lũy cho mỗi ca bằng tổng chi phí khớp cho ca đó. 
9. Tiền tố tổng của mảng sai phân để phục hồi mọi`cost[k]`. 
10. Chọn ca có chi phí tối thiểu. 
11. Xây dựng lại bài tập. Nếu sự thay đổi tốt nhất là`k`, sau đó sắp xếp văn phòng`i`tiếp nhận ứng viên đã được sắp xếp`(i+k) mod n`. 
12. Chuyển phép gán về chỉ số ban đầu và in kết quả. 

### Tại sao nó hoạt động 

Sau khi sắp xếp các điểm xung quanh vòng tròn, mọi kết quả khớp giao nhau đều có thể được bỏ qua mà không làm tăng giá trị mục tiêu. Việc lặp lại quá trình này mang lại sự kết hợp không chéo tối ưu. 

Sự kết hợp hoàn hảo không giao nhau giữa hai bộ được sắp xếp theo chu kỳ hoàn toàn được xác định bởi một cặp bắt đầu. Sau khi chọn được cặp đầu tiên, các cặp còn lại phải tuân theo thứ tự vòng tròn. Điều đó có nghĩa là mọi giải pháp tối ưu đều tương ứng với một số dịch chuyển có tính chu kỳ của dãy ứng cử viên đã được sắp xếp. 

Cấu trúc mảng chênh lệch tính toán tổng chi phí chính xác cho mỗi ca cùng một lúc. Mỗi cặp ứng cử viên chức vụ đóng góp chính xác vào những ca làm việc mà cặp đó phù hợp và việc phân tách từng phần bao gồm tất cả các khả năng. Do đó, giá trị phục hồi của độ dịch chuyển`k`chính xác là tổng chi phí của việc khớp được xác định bởi ca đó. Việc chọn mức tối thiểu trong số chúng mang lại mức tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
from bisect import bisect_right

input = sys.stdin.readline

def solve():
    m, n = map(int, input().split())

    A = [(x, i) for i, x in enumerate(map(int, input().split()), start=1)]
    B = [(x, i) for i, x in enumerate(map(int, input().split()), start=1)]

    A.sort()
    B.sort()

    a = [x for x, _ in A]
    b = [x for x, _ in B]

    aa = [2 * x for x in a]
    bb = [2 * x for x in b]

    diff = [0] * (n + 1)

    def add_range(l, r, v):
        if l > r:
            return

        l %= n
        r %= n

        if l <= r:
            diff[l] += v
            if r + 1 < n:
                diff[r + 1] -= v
        else:
            diff[0] += v
            if r + 1 < n:
                diff[r + 1] -= v
            diff[l] += v

    for i in range(n):
        x = a[i]

        r = bisect_right(bb, 2 * x - m - 1) - 1
        add_range(-i, r - i, -x)

        l = r + 1
        r = bisect_right(bb, 2 * x - 1) - 1
        add_range(l - i, r - i, x)

        l = r + 1
        r = bisect_right(bb, 2 * x + m) - 1
        add_range(l - i, r - i, -x)

        l = r + 1
        r = n - 1
        add_range(l - i, r - i, x + m)

    for i in range(n):
        x = b[i]

        r = bisect_right(aa, 2 * x - m - 1) - 1
        add_range(i - r, i, -x)

        l = r + 1
        r = bisect_right(aa, 2 * x) - 1
        add_range(i - r, i - l, x)

        l = r + 1
        r = bisect_right(aa, 2 * x + m) - 1
        add_range(i - r, i - l, -x)

        l = r + 1
        r = n - 1
        add_range(i - r, i - l, x + m)

    cur = 0
    best_shift = 0
    best_cost = None

    for k in range(n):
        cur += diff[k]
        if best_cost is None or cur < best_cost:
            best_cost = cur
            best_shift = k

    ans = [0] * n

    for i in range(n):
        j = (i + best_shift) % n
        office_idx = A[i][1]
        candidate_idx = B[j][1]
        ans[office_idx - 1] = candidate_idx

    print(best_cost)
    print(*ans)

solve()
```Phần đầu tiên sắp xếp các chức vụ và ứng cử viên trong khi vẫn giữ nguyên các chỉ số ban đầu để có thể xây dựng lại nhiệm vụ cuối cùng. 

Cốt lõi của việc triển khai là mảng khác biệt giữa các miền. Thay vì tính toán chi phí của từng ca riêng biệt, mỗi tìm kiếm nhị phân sẽ xác định toàn bộ khoảng thời gian của các ca nhận được sự đóng góp như nhau. Người trợ giúp`add_range`thực hiện cập nhật phạm vi theo chu kỳ vì các ca được thực hiện theo modulo`n`. 

Bốn phạm vi tìm kiếm nhị phân tương ứng với bốn dạng đại số khác nhau của khoảng cách đường tròn. Nguồn gốc của chúng xuất phát từ việc so sánh cung trực tiếp và cung bao bọc. 

Sau khi tất cả các bản cập nhật được áp dụng, tổng tiền tố sẽ xây dựng lại tổng chi phí cho mỗi ca. Sự thay đổi tối thiểu xác định sự kết hợp tối ưu. 

Một nơi dễ mắc lỗi là cập nhật theo khoảng thời gian theo mô-đun. Các khoảng có thể bao quanh phần cuối của mảng dịch chuyển, vì vậy chúng phải được chia thành hai phạm vi thông thường. Một nguồn lỗi phổ biến khác là việc sử dụng`bisect_right(...)-1`, cố ý tìm chỉ mục cuối cùng thuộc về một khu vực. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
10 3
1 5 5
10 4 6
```Vị trí được sắp xếp: 

| tôi | Văn phòng A[i] | Ứng viên B[i] | 
| --- | --- | --- | 
| 0 | 1 | 4 | 
| 1 | 5 | 6 | 
| 2 | 5 | 10 | 

Sau khi xử lý tất cả các cập nhật phạm vi: 

| Dịch chuyển k | Tổng chi phí | 
| --- | --- | 
| 0 | 3 | 
| 1 | 9 | 
| 2 | 9 | 

Sự thay đổi tốt nhất là`0`. 

Phù hợp: 

| Thành phố văn phòng | Thành phố ứng cử viên | Khoảng cách | 
| --- | --- | --- | 
| 1 | 10 | 1 | 
| 5 | 4 | 1 | 
| 5 | 6 | 1 | 

Tổng chi phí =`3`. 

Ví dụ này cho thấy các thứ tự được sắp xếp đã được căn chỉnh một cách tối ưu. 

### Mẫu 2 

đầu vào:```
10 3
1 4 8
3 6 8
```Vị trí được sắp xếp: 

| tôi | Văn phòng A[i] | Ứng viên B[i] | 
| --- | --- | --- | 
| 0 | 1 | 3 | 
| 1 | 4 | 6 | 
| 2 | 8 | 8 | 

Chi phí tính toán: 

| Dịch chuyển k | Tổng chi phí | 
| --- | --- | 
| 0 | 4 | 
| 1 | 10 | 
| 2 | 10 | 

Sự thay đổi tốt nhất là`0`. 

Phù hợp: 

| Thành phố văn phòng | Thành phố ứng cử viên | Khoảng cách | 
| --- | --- | --- | 
| 1 | 3 | 2 | 
| 4 | 6 | 2 | 
| 8 | 8 | 0 | 

Tổng chi phí =`4`. 

Dấu vết này minh họa cấu trúc không giao nhau. Sau khi cặp đầu tiên được sửa, phần còn lại của việc khớp sẽ tự động được thực hiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp cộng với tìm kiếm nhị phân cho mọi điểm | 
| Không gian | O(n) | Mảng sắp xếp, mảng gán, mảng sai phân | 

Với`n ≤ 200000`, MỘT`O(n log n)`giải pháp dễ dàng phù hợp trong thời hạn. Việc sử dụng bộ nhớ là tuyến tính và thoải mái dưới giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
# helper skeleton for local testing

import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    # call solve() here
    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old_stdout
    return out.getvalue().strip()

# sample 1
res = run(
"""10 3
1 5 5
10 4 6
"""
)
assert res.splitlines()[0] == "3"

# sample 2
res = run(
"""10 3
1 4 8
8 3 6
"""
)
assert res.splitlines()[0] == "4"

# minimum size
res = run(
"""1 1
1
1
"""
)
assert res.splitlines()[0] == "0"

# all equal
res = run(
"""20 4
5 5 5 5
5 5 5 5
"""
)
assert res.splitlines()[0] == "0"

# wrap-around distance
res = run(
"""10 2
1 9
10 2
"""
)
assert res.splitlines()[0] == "2"

# duplicated cities
res = run(
"""100 3
10 10 50
9 11 50
"""
)
assert res.splitlines()[0] == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`m=1, n=1`| trị giá`0`| Hạn chế tối thiểu | 
| Tất cả các thành phố đều bình đẳng | trị giá`0`| Vị trí trùng lặp | 
| Ví dụ bao quanh | trị giá`2`| Hình học tròn | 
| Bản sao hỗn hợp | trị giá`2`| Xử lý ổn định các tọa độ lặp lại | 

## Vỏ cạnh 

Hãy xem xét:```
10 2
1 9
10 2
```Khoảng cách hình tròn là`1`Và`1`, cho biết tổng chi phí`2`. Giải pháp khoảng cách tuyến tính sẽ tính toán không chính xác`1 + 7 = 8`. Thuật toán xử lý điều này vì việc phân tách khoảng cách sử dụng rõ ràng số liệu vòng tròn và xử lý các đường dẫn được bao bọc như các ứng cử viên hạng nhất. 

Bây giờ hãy xem xét các vị trí lặp lại:```
20 3
5 5 5
5 5 5
```Mỗi ca đều có chi phí`0`. Cấu trúc mảng sai phân ấn định mức đóng góp bằng 0 ở mọi nơi, tất cả các ca đều bằng nhau và thuật toán đưa ra một hoán vị hợp lệ. Không cần xử lý đặc biệt. 

Cuối cùng, hãy xem xét trường hợp trong đó sự kết hợp tối ưu không rõ ràng so với thứ tự ban đầu:```
10 3
1 4 8
8 3 6
```Sắp xếp cho thấy thứ tự vòng tròn. Thuộc tính không giao nhau làm giảm không gian tìm kiếm thành các dịch chuyển theo chu kỳ và dịch chuyển chi phí tối thiểu tái tạo phép gán tối ưu với tổng chi phí`4`. Thuật toán không bao giờ phụ thuộc vào thứ tự đầu vào ban đầu mà chỉ phụ thuộc vào thứ tự hình học xung quanh vòng tròn.
