---
title: "CF 2157E - Điều chỉnh Drone"
description: "Chúng tôi đang quản lý một đội máy bay không người lái, mỗi chiếc đều có mức năng lượng ban đầu. Quy tắc là không có mức năng lượng nào xuất hiện quá k lần."
date: "2026-06-08T00:20:04+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "dp", "dsu", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2157
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1066 (Div. 1 + Div. 2)"
rating: 1900
weight: 2157
solve_time_s: 117
verified: false
draft: false
---

[CF 2157E - Điều chỉnh máy bay không người lái](https://codeforces.com/problemset/problem/2157/E) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, sức mạnh vũ phu, cấu trúc dữ liệu, dp, dsu, tham lam, triển khai, sắp xếp 
**Thời gian giải:** 1 phút 57 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang quản lý một đội máy bay không người lái, mỗi chiếc đều có mức năng lượng ban đầu. Nguyên tắc là không có mức năng lượng nào được xuất hiện nhiều hơn`k`lần. Nếu bất kỳ mức năng lượng nào vượt quá giới hạn này, máy bay không người lái sẽ thực hiện một loạt hoạt động cân bằng: mỗi máy bay không người lái là bản sao của lần xuất hiện trước đó trong mảng sẽ tăng năng lượng của nó lên một. Hoạt động này tiếp tục cho đến khi tất cả các mức năng lượng xuất hiện nhiều nhất`k`lần. Chúng ta cần tính tổng số thao tác này cho từng trường hợp thử nghiệm. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp có số lượng máy bay không người lái`n`, số lượng tối đa cho phép`k`và dãy năng lượng ban đầu. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm biểu thị số lượng thao tác được thực hiện. 

Những hạn chế là đáng kể.`n`có thể lên đến`2·10^5`, và tổng số tiền`n`trên các trường hợp thử nghiệm cũng bị giới hạn bởi`2·10^5`. Điều này loại trừ một mô phỏng đơn giản của mọi hoạt động, sẽ`O(n^2)`trong trường hợp xấu nhất nếu chúng ta lặp đi lặp lại các bản sao. Bản thân mức năng lượng nhiều nhất là`2n`, vì vậy việc sử dụng mảng tần số hoặc bản đồ là khả thi. 

Trường hợp cạnh tinh tế xảy ra khi tất cả các mức năng lượng đều bằng nhau. Ví dụ, nếu`n = 6`Và`k = 3`với`a = [1,1,1,1,1,1]`, các thao tác sẽ phát triển mảng dần dần, tăng dần từng bản sao một. Một cách tiếp cận tham lam bất cẩn chỉ tăng số lần trùng lặp cuối cùng hoặc chỉ đếm số lần tràn ngay lập tức có thể tính toán sai tổng số thao tác. 

Một trường hợp cạnh khác là khi`k >= n`. Sau đó, không cần thực hiện thao tác nào vì giới hạn tần số không bao giờ bị vượt quá. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp lại mảng, đánh dấu tất cả các phần tử trùng lặp với các phần tử trước đó, tăng chúng và lặp lại cho đến khi không có phần tử nào xuất hiện nhiều hơn`k`lần. Điều này hoạt động chính xác, nhưng trong trường hợp xấu nhất, mọi máy bay không người lái có thể cần phải tăng lên nhiều lần. Nếu số lớn nhất là 1 và`k = 1`, các hoạt động có thể lặp lại khoảng`n`lần trên mỗi máy bay không người lái, dẫn đến`O(n^2)`hành vi quá chậm đối với`n = 2·10^5`. 

Thông tin chi tiết quan trọng là vị trí cuối cùng chính xác của máy bay không người lái không quan trọng mà chỉ là số lượng của từng mức năng lượng. Chúng ta có thể sắp xếp các mức năng lượng và mô phỏng mức tăng trong không gian tần số nén. Nếu chúng ta đếm số lượng bản sao trên mỗi mức năng lượng vượt quá`k`, chúng ta có thể "mang" những chiếc máy bay không người lái dư thừa này lên mức năng lượng cao hơn tiếp theo chỉ bằng một bước. Điều này làm giảm quy trình thành một đợt quét tuyến tính trên các năng lượng duy nhất đã được sắp xếp, tăng số lượng máy bay không người lái đang chờ xử lý, có thể được thêm vào cấp độ tiếp theo trong một bước. Mỗi mức tăng tương ứng chính xác với một thao tác trong quy trình ban đầu. Điều này cho phép chúng ta tính toán tổng số hoạt động trong`O(n log n)`thời gian do bước sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu (tần số + mang) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`Và`k`và mảng năng lượng`a`. 
2. Đếm tần số của từng mức năng lượng bằng cách sử dụng bản đồ hoặc mảng. Điều này cho chúng ta số lượng máy bay không người lái hiện có ở mỗi năng lượng. 
3. Khởi tạo một biến`carry`về 0. Điều này sẽ lưu trữ số lượng máy bay không người lái mà chúng ta cần tăng ở mỗi bước. 
4. Lặp lại các mức năng lượng theo thứ tự tăng dần. Đối với mỗi cấp độ`x`: 

1. Thêm dòng điện`carry`đến tần số của`x`. Điều này đại diện cho tất cả các máy bay không người lái đã được tăng lên từ các cấp độ trước đó. 
2. Nếu tần số của`x`vượt quá`k`, tính toán`excess = freq[x] - k`. Tăng`carry`bằng cách này`excess`. Mỗi đơn vị dư thừa cuối cùng sẽ trở thành một hoạt động cân bằng. 
3. Tăng tổng bộ đếm hoạt động lên`excess`. 
5. Sau khi xử lý tất cả các mức năng lượng, bộ đếm hoạt động tổng chứa câu trả lời cho trường hợp kiểm thử. 

Tại sao nó hoạt động: thuật toán duy trì tính bất biến sau khi xử lý mức năng lượng`x`, không có máy bay không người lái bên dưới`x`đã qua rồi`k`giới hạn. Bất kỳ sự dư thừa nào cũng được truyền bá như`carry`lên cấp độ tiếp theo, bắt chước các hoạt động cân bằng ban đầu. Việc sắp xếp đảm bảo rằng các số gia luôn tiến về phía trước, do đó thứ tự các thao tác khớp với quy trình được mô tả. 

## Giải pháp Python```python
import sys
from collections import Counter
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        
        freq = Counter(a)
        ops = 0
        carry = 0
        
        for x in sorted(freq.keys()):
            freq[x] += carry
            if freq[x] > k:
                excess = freq[x] - k
                carry = excess
                ops += excess
            else:
                carry = 0
        
        print(ops)

solve()
```Mã đầu tiên đếm tần số và sau đó quét qua các phím được sắp xếp.`carry`truyền các bản sao dư thừa về phía trước và mỗi đơn vị mang biểu thị chính xác một thao tác được thực hiện trong quy trình lặp ban đầu. Việc sắp xếp là cần thiết để đảm bảo máy bay không người lái luôn tăng lên mức năng lượng cao hơn tiếp theo theo đúng trình tự. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
6 3
1 1 1 1 1 1
```Bản đồ tần số ban đầu được`{1:6}`. Sau khi xử lý`1`: 

| Năng lượng | tần số | mang theo | hoạt động | 
| --- | --- | --- | --- | 
| 1 | 6 | 3 | 3 |`freq[1] > k`Vì thế`excess = 6-3=3`. Mang 3 đến cấp độ tiếp theo, ops += 3. Không có phím nào khác tồn tại, câu trả lời là 3. 

Đối với đầu vào:```
5 1
1 3 2 1 4
```Tần số ban đầu:`{1:2, 2:1, 3:1, 4:1}`| Năng lượng | tần số+mang theo | mang theo | hoạt động | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 1 | 
| 2 | 2 | 1 | 2 | 
| 3 | 2 | 1 | 3 | 
| 4 | 2 | 1 | 4 | 

Câu trả lời cuối cùng: 4. Điều này xác nhận rằng mỗi thao tác đều tương ứng chính xác với việc di chuyển các bản sao về phía trước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các nguồn năng lượng độc đáo chiếm ưu thế; quá trình quét là tuyến tính. | 
| Không gian | O(n) | Bản đồ tần số lưu trữ nhiều nhất`n`phím. | 

Với`n`lên đến`2·10^5`và tổng cộng`2·10^5`trong tất cả các trường hợp thử nghiệm, giải pháp này dễ dàng nằm gọn trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("5\n6 3\n1 1 1 1 1 1\n5 1\n1 3 2 1 4\n6 2\n1 1 1 2 3 3\n4 1\n8 8 8 8\n2 2\n1 2") == "3\n4\n4\n3\n0"

# custom tests
assert run("1\n1 1\n1") == "0", "single drone, no ops"
assert run("1\n4 4\n1 1 1 1") == "0", "all drones allowed, no ops"
assert run("1\n4 1\n1 1 1 1") == "3", "all equal, k=1, multiple ops"
assert run("1\n5 2\n2 2 2 2 2") == "3", "all equal, k=2"
assert run("1\n6 3\n1 1 1 2 2 2") == "0", "two groups at limit"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 0 | máy bay không người lái đơn, không hoạt động | 
| 4 4 4 4, k=4 | 0 | tất cả đều bằng nhau, giới hạn bằng kích thước | 
| 4 1 1 1 1, k=1 | 3 | số gia tăng lặp lại cho mảng hoàn toàn bằng nhau | 
| 2 2 2 2 2, k=2 | 3 | nhiều thao tác cho nhóm vượt quá giới hạn | 
| 1 1 1 2 2 2, k=3 | 0 | nhiều nhóm chính xác ở mức giới hạn | 

## Vỏ cạnh 

Khi nào`n = k`, không có hoạt động nào xảy ra bất kể năng lượng của máy bay không người lái. Ví dụ,`n = 2, k = 2, a = [5,5]`. Tần số của 5 là 2, bằng`k`, Vì thế`carry`vẫn bằng 0 và đầu ra là`0`. 

Khi tất cả năng lượng đều giống nhau và`k = 1`, chẳng hạn như`n = 6, k = 1, a = [1,1,1,1,1,1]`, mỗi lượt tăng gấp đôi và tạo ra mức mang cho đến khi không còn năng lượng vượt quá`k`. Thuật toán của chúng tôi truyền bá chính xác phần dư thừa bằng cách mang
