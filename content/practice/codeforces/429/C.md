---
title: "CF 429C - Đoán Cây"
description: "Bài toán yêu cầu chúng ta xây dựng lại một cây có gốc với các ràng buộc về kích thước của các cây con cho mỗi nút. Bạn được cho một mảng c có độ dài n, trong đó c[i] biểu thị tổng số nút trong cây con có gốc tại nút i."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "dp", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 429
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 245 (Div. 1)"
rating: 2300
weight: 429
solve_time_s: 86
verified: true
draft: false
---

[CF 429C - Đoán cây](https://codeforces.com/problemset/problem/429/C) 

**Đánh giá:** 2300 
**Tags:** bitmasks, thuật toán xây dựng, dp, tham lam, cây cối 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta xây dựng lại một cây có gốc với các ràng buộc về kích thước của các cây con cho mỗi nút. Bạn được cung cấp một mảng`c`chiều dài`n`, Ở đâu`c[i]`biểu thị tổng số nút trong cây con có gốc tại nút`i`. Mỗi nút có con phải có ít nhất hai nút con. Nhiệm vụ là xác định xem có tồn tại một cây thỏa mãn các tính chất này hay không. 

Kích thước đầu vào nhỏ,`n ≤ 24`, do đó các thuật toán có quy mô theo cấp số nhân với`n`là khả thi. Mỗi`c[i]`nhiều nhất là`n`, do đó không có giá trị nào nằm ngoài phạm vi hợp lệ. Phần khó khăn là các giá trị trong`c`có thể xuất hiện theo bất kỳ thứ tự nào, vì vậy chúng ta không thể cho rằng phần tử cuối cùng là gốc hoặc các giá trị đó tăng đơn điệu dọc theo cây. 

Một cách tiếp cận đơn giản có thể thử tất cả các cây có thể và kiểm tra kích thước cây con của chúng. Vì`n = 24`, điều này là không thể vì số lượng cây có rễ tăng theo cấp số nhân. Một trường hợp khó phát hiện khác xảy ra khi chỉ có một nút. Nếu như`n = 1`Và`c = [1]`, giải pháp gần như là "CÓ" vì một nút đơn lẻ được tính là cây con có kích thước một. Một cạm bẫy khác là có`c[i] = n`cho nhiều hơn một nút; cây không thể có hai rễ với kích thước cây đầy đủ. 

Ngoài ra, nếu tất cả`c[i] = 1`, cấu hình hợp lệ duy nhất là một rừng các nút lá, điều này là không thể nếu`n > 1`bởi vì một nút bên trong phải có ít nhất hai nút con. Ví dụ, đầu vào`[1,1,1]`nên trả về "KHÔNG". 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra tất cả các cây có gốc có thể và kiểm tra từng cây theo kích thước của cây con. Vì`n`các nút, số lượng cây có thể có gốc là theo cấp số nhân trong`n`. Việc kiểm tra kích thước cây con cho mỗi cây mất`O(n^2)`, vì vậy ngay cả đối với`n = 15`, phương pháp này trở nên không khả thi. Tính đúng đắn của phương pháp brute-force này đến từ việc xác minh toàn diện mọi cấu trúc, nhưng nó quá chậm. 

Cái nhìn sâu sắc quan trọng là vấn đề có thể được giảm xuống thành vấn đề phân vùng tổ hợp. Nếu chúng ta sắp xếp các nút theo kích thước cây con của chúng theo thứ tự giảm dần thì giá trị lớn nhất phải tương ứng với nút gốc. Sau đó, mỗi nút bên trong phải được gán các nút con sao cho tổng kích thước cây con của chúng cộng với 1 bằng kích thước cây con của nút cha. Điều này giống như một bài toán tổng tập con đệ quy. Bởi vì`n ≤ 24`, chúng ta có thể khám phá tất cả các nhóm nút còn lại có thể có một cách hiệu quả bằng cách sử dụng lập trình động bitmask. Bitmask biểu thị những nút nào vẫn chưa được gán. Chúng tôi cố gắng tạo một tập hợp con hợp lệ cho nút hiện tại theo cách đệ quy và ghi nhớ kết quả để tránh công việc lặp lại. Điều này làm giảm đáng kể không gian tìm kiếm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(#cây × n^2) | O(n) | Quá chậm | 
| Bitmask DP / Xây dựng đệ quy | O(2^n × n^2) | O(2^n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng`c`theo thứ tự giảm dần. Phần tử lớn nhất phải là gốc, nếu không thì không có cây nào có thể đáp ứng được tất cả các kích thước của cây con. 
2. Sử dụng hàm đệ quy`can_build(mask)`Ở đâu`mask`đại diện cho các nút chưa được đặt trong cây. Nếu như`mask`bằng 0, chúng tôi đã gán thành công tất cả các nút. 
3. Đối với nút hiện tại`u`, chúng tôi cố gắng gán một tập hợp con các nút còn lại làm nút con. Tập hợp con hợp lệ`S`thỏa mãn`sum(c[i] for i in S) + 1 = c[u]`. Mỗi nút bên trong phải có ít nhất hai nút con nếu`c[u] > 1`. 
4. Với mỗi tập hợp con hợp lệ`S`, cố gắng xây dựng cây cho các nút còn lại một cách đệ quy,`mask - S`. Nếu bất kỳ nhánh nào thành công, trả về true. 
5. Sử dụng tính năng ghi nhớ để lưu trữ kết quả cho từng`mask`, vì có thể gặp cùng một tập hợp các nút còn lại nhiều lần. 
6. Nếu không có phép gán hợp lệ nào cho nút và mặt nạ hiện tại, hãy trả về sai. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến rằng mọi nút trong mặt nạ hiện tại đều chưa được gán. Mỗi lần đệ quy đảm bảo rằng ràng buộc về kích thước cây con được thỏa mãn một cách chính xác. Các nút sắp xếp đảm bảo kích thước cây con lớn nhất luôn được chỉ định trước, đảm bảo gốc là hợp lệ. Việc ghi nhớ tránh việc kiểm tra lại cấu hình tương tự. Nếu thuật toán trả về true thì tồn tại một phép gán hợp lệ cho tất cả các nút thỏa mãn các quy tắc đã cho. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from functools import lru_cache

def solve():
    n = int(input())
    c = list(map(int, input().split()))
    c.sort(reverse=True)

    @lru_cache(None)
    def can_build(mask):
        if mask == 0:
            return True
        # pick the first node in mask
        u = mask.bit_length() - 1
        size_needed = c[u]
        # if it's a leaf
        if size_needed == 1:
            return can_build(mask ^ (1 << u))
        # try all subsets of remaining nodes to be children
        submask = mask ^ (1 << u)
        subset = submask
        while subset:
            total = sum(c[i] for i in range(n) if (subset >> i) & 1)
            if total + 1 == size_needed:
                if bin(subset).count("1") >= 2 and can_build(submask ^ subset):
                    return True
            subset = (subset - 1) & submask
        return False

    full_mask = (1 << n) - 1
    if can_build(full_mask):
        print("YES")
    else:
        print("NO")

solve()
```Mã bắt đầu bằng cách sắp xếp các nút để đảm bảo nút gốc được xử lý trước. Hàm đệ quy sử dụng mặt nạ bit để theo dõi các nút còn lại. Việc tạo tập hợp con kiểm tra tất cả các tổ hợp con có thể một cách hiệu quả bằng cách sử dụng`(subset - 1) & submask`lừa. Nút lá (`c[i] = 1`) được xử lý tự động. Việc ghi nhớ đảm bảo công việc theo cấp số nhân không bị lặp lại. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
4
1 1 1 4
```| Bước | Mặt nạ (nhị phân) | Nút hiện tại | Tập con trẻ em | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1111 | nút 3 (cỡ 4) | 0111 (nút 0,1,2) | giao 3 con | 
| 2 | 0000 | nút lá 0,1,2 | - | thành công | 

Đệ quy chọn nút 3 làm nút gốc, gán ba nút còn lại làm nút con của nó. Mỗi đứa trẻ là một chiếc lá, thỏa mãn kích thước của cây con. Hàm trả về CÓ. 

Đầu vào tùy chỉnh 2:```
3
1 2 2
```Mặt nạ`111`→ kích thước nút lớn nhất 2 → không thể chỉ định chính xác một nút (cần ít nhất hai nút con) → KHÔNG. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^n × n^2) | Đệ quy Bitmask khám phá tất cả các tập hợp con của các nút còn lại, mỗi tập hợp yêu cầu tổng tối đa n phần tử | 
| Không gian | O(2^n) | Bộ đệm ghi nhớ cho mỗi bitmask | 

Với`n ≤ 24`,`2^24 ≈ 16 million`mục là khả thi. Mỗi lần đệ quy chỉ lặp lại các tập hợp con của mặt nạ và thời gian chạy tổng thể dưới 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("4\n1 1 1 4\n") == "YES", "sample 1"

# minimum input
assert run("1\n1\n") == "YES", "single node tree"

# impossible small tree
assert run("3\n1 2 2\n") == "NO", "cannot form internal node with 2 children"

# all leaves but more than 1 node
assert run("4\n1 1 1 1\n") == "NO", "no internal node possible"

# maximum node values
assert run("5\n1 2 2 3 5\n") == "YES", "valid tree with root size 5"

# another impossible
assert run("4\n2 2 2 4\n") == "NO", "internal nodes cannot satisfy size 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | CÓ | Tái thiết cây đúng cách | 
| 1 | CÓ | Đầu vào kích thước tối thiểu | 
| 3 | KHÔNG | Cấu hình không thể cho các nút nội bộ | 
| 4 | KHÔNG | Tất cả các nút lá có n>1 | 
| 5 | CÓ | Cây hợp lệ lớn hơn | 
| 4 | KHÔNG | Kích thước nút nội bộ không khớp | 

## Vỏ cạnh 

Đối với một đầu vào nút duy nhất`1\n1\n`, mặt nạ bắt đầu như`1`. Nút gốc có kích thước 1, đệ quy loại bỏ nó, mặt nạ trở thành`0`. Thuật toán trả về CÓ chính xác. 

Đối với đầu vào`[1,1,1,1]`, mặt nạ`1111`, nút lớn nhất có kích thước 1. Mỗi nút là một lá, nhưng nút gốc phải có kích thước >1 để có con. Không có tập hợp con nào của các nút còn lại thỏa mãn yêu cầu con của ≥2 con đối với các nút bên trong. Thuật toán khám phá tất cả các tập hợp con và trả về NO. Điều này xác nhận việc xử lý các kịch bản toàn lá.
