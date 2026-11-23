---
title: "CF 486B - HOẶC trong Ma trận"
description: "Chúng ta có một ma trận B có kích thước m × n, trong đó mỗi phần tử là 0 hoặc 1. Ma trận này được cho là được tạo từ một số ma trận A gốc có cùng kích thước bằng cách sử dụng một quy tắc cụ thể: mỗi phần tử B[i][j] bằng logic OR của tất cả các phần tử trong hàng thứ i của A kết hợp với tất cả…"
date: "2026-06-07T17:26:52+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "hashing", "implementation"]
categories: ["algorithms"]
codeforces_contest: 486
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 277 (Div. 2)"
rating: 1300
weight: 486
solve_time_s: 92
verified: true
draft: false
---

[CF 486B - HOẶC trong Ma trận](https://codeforces.com/problemset/problem/486/B) 

**Đánh giá:** 1300 
**Tags:** tham lam, băm, thực hiện 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một ma trận`B`kích thước`m × n`, trong đó mỗi phần tử là 0 hoặc 1. Ma trận này được cho là được tạo từ một số ma trận gốc`A`có cùng kích thước bằng cách sử dụng một quy tắc cụ thể: mỗi phần tử`B[i][j]`bằng logic OR của tất cả các phần tử trong`i`-hàng thứ của`A`kết hợp với tất cả các phần tử trong`j`-cột thứ của`A`. chính thức,`B[i][j] = (OR of row i in A) OR (OR of column j in A)`. 

Nhiệm vụ là xây dựng lại bất kỳ ma trận nào`A`điều đó có thể đã tạo ra`B`hoặc xác định rằng không tồn tại ma trận như vậy bởi vì`B`không phù hợp với quy tắc OR. Giới hạn kích thước đầu vào rất khiêm tốn (`m, n ≤ 100`), cho phép các giải pháp chạy trong khoảng`O(m*n)`thời gian. 

Trường hợp khó phát hiện xảy ra khi số 0 xuất hiện trong`B`. Nếu như`B[i][j]`bằng 0, nó buộc tất cả các phần tử phải xếp hàng`i`và cột`j`của`A`bằng không. Bất kỳ sự thất bại nào trong việc thực thi điều này đều tạo ra sự mâu thuẫn. Một cách tiếp cận ngây thơ cố gắng lấp đầy mọi nơi một cách tham lam`B[i][j]`là người ta có thể thất bại nếu số 0 ở một nơi khác xung đột. 

Ví dụ, hãy xem xét:```
2 2
1 0
0 0
```Đây`B[1][2] = 0`lực lượng`A[1][2] = A[2][2] = 0`. Tương tự,`B[2][1] = 0`lực lượng`A[2][1] = A[2][2] = 0`. Nhưng`B[1][1] = 1`yêu cầu ít nhất một số 1 ở hàng 1 hoặc cột 1. Bất kỳ phép gán số 0 nào thỏa mãn các số 0 sẽ ngăn cản việc đặt số 1 cho`B[1][1]`, nên không có giải pháp nào tồn tại. Phát hiện những mâu thuẫn này là chìa khóa. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử tất cả`2^(m*n)`nhiệm vụ có thể cho`A`và kiểm tra xem kết quả`B`phù hợp với cái đã cho. Điều này đúng về mặt lý thuyết nhưng hoàn toàn không khả thi đối với`m = n = 100`, vì nó sẽ liên quan đến`2^10000`sự kết hợp. 

Cái nhìn sâu sắc quan trọng là số không trong`B`ràng buộc chặt chẽ các yếu tố trong`A`: nếu như`B[i][j] = 0`, tất cả hàng`i`và cột`j`phải bằng không. Những người trong`B`linh hoạt hơn, yêu cầu ít nhất một số 1 trong liên kết hàng`i`và cột`j`. Therefore, we can start by initializing`A`với những cái ở khắp mọi nơi, sau đó loại bỏ rõ ràng các hàng và cột tương ứng với số 0 trong`B`. Cuối cùng, chúng tôi xác minh xem tất cả những cái trong`B`vẫn hài lòng. Điều này làm giảm vấn đề thành một cách tiếp cận hai bước đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(m*n)) | O(m*n) | Quá chậm | 
| Không lan truyền + xác minh | O(m*n) | O(m*n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo ma trận`A`kích thước`m × n`chứa đầy những cái đó. Đây là một giả định lạc quan rằng người ta có thể thỏa mãn tất cả`B[i][j] = 1`hạn chế. 
2. Đối với mỗi phần tử`B[i][j]`bằng 0, đặt mọi phần tử trong hàng`i`và cột`j`của`A`về không. Điều này thực thi yêu cầu nghiêm ngặt rằng số 0 trong`B`không thể phát sinh nếu bất kỳ phần tử nào trong hàng hoặc cột tương ứng của`A`là một. 
3. Xây dựng ma trận mới`C`bằng cách tính toán`(OR of row i in A) OR (OR of column j in A)`cho mỗi phần tử. Điều này mô phỏng việc tạo ra`B`từ ứng viên`A`. 
4. So sánh`C`về bản gốc`B`. Nếu chúng khớp chính xác, hãy in "CÓ" và ma trận`A`. Ngược lại in ra "KHÔNG". 

Tại sao nó hoạt động: bước 2 đảm bảo rằng tất cả các mục bằng 0 trong`B`được tôn trọng ở`A`. Bước 3 kiểm tra xem tất cả những cái trong`B`có thể đạt được với các vị trí không được chọn. Nếu xung đột phát sinh sẽ được phát hiện trong quá trình xác minh, đảm bảo tính chính xác. Sự lan truyền bằng 0 nắm bắt được tất cả các ràng buộc cứng; bất kỳ cái nào còn lại có thể được đặt tùy ý để đáp ứng`B`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

m, n = map(int, input().split())
B = [list(map(int, input().split())) for _ in range(m)]

# Step 1: Initialize A with ones
A = [[1]*n for _ in range(m)]

# Step 2: Zero out rows and columns based on zeros in B
for i in range(m):
    for j in range(n):
        if B[i][j] == 0:
            for k in range(n):
                A[i][k] = 0
            for k in range(m):
                A[k][j] = 0

# Step 3: Verify by reconstructing B
C = [[0]*n for _ in range(m)]
row_or = [0]*m
col_or = [0]*n

for i in range(m):
    row_or[i] = 0
    for j in range(n):
        row_or[i] |= A[i][j]

for j in range(n):
    col_or[j] = 0
    for i in range(m):
        col_or[j] |= A[i][j]

valid = True
for i in range(m):
    for j in range(n):
        C[i][j] = row_or[i] | col_or[j]
        if C[i][j] != B[i][j]:
            valid = False
            break
    if not valid:
        break

if valid:
    print("YES")
    for row in A:
        print(' '.join(map(str, row)))
else:
    print("NO")
```Mã đầu tiên xây dựng một ứng cử viên`A`bằng cách loại bỏ các hàng và cột được biểu thị bằng số 0 trong`B`. Sau đó nó tính toán trước OR của từng hàng và cột để xây dựng lại`B`. Nếu ma trận được xây dựng lại khớp thì ứng viên hợp lệ; nếu không, nó bị từ chối. Việc sử dụng OR hàng và cột được tính toán trước sẽ tránh được việc tính toán lặp lại cho mọi phần tử. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 2
1 0
0 0
```| tôi | j | B[i][j] | Hành động trên A | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | ban đầu không có | 
| 0 | 1 | 0 | hàng không 0 và col 1 | 
| 1 | 0 | 0 | hàng 0 1 và col 0 | 
| 1 | 1 | 0 | đã bằng không | 

Ma trận được xây dựng lại`C`:```
0 0
0 0
```Không khớp`B`, vậy đầu ra là`NO`. Điều này cho thấy việc phát hiện các ràng buộc xung đột. 

### Mẫu 2 

đầu vào:```
3 3
1 1 1
1 0 1
1 1 1
```Bước 2 về 0: 

-`B[1][1] = 0`→ không hàng 1 và cột 1. 

ứng viên`A`:```
1 0 1
0 0 0
1 0 1
```Tái thiết`C`:```
1 1 1
1 0 1
1 1 1
```Trận đấu`B`, vậy đầu ra là`YES`với ứng viên`A`. Xác nhận rằng những cái có thể cùng tồn tại với các số không được thi hành. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m*n) | Mỗi số 0 kích hoạt cập nhật O(m+n); việc xây dựng lại B yêu cầu các thao tác O(m*n). | 
| Không gian | O(m*n) | Lưu trữ ma trận A, B, C, cộng với mảng row_or và col_or. | 

Với`m, n ≤ 100`,`m*n ≤ 10000`, điều này phù hợp thoải mái trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    m, n = map(int, input().split())
    B = [list(map(int, input().split())) for _ in range(m)]
    A = [[1]*n for _ in range(m)]
    for i in range(m):
        for j in range(n):
            if B[i][j] == 0:
                for k in range(n):
                    A[i][k] = 0
                for k in range(m):
                    A[k][j] = 0
    row_or = [0]*m
    col_or = [0]*n
    for i in range(m):
        for j in range(n):
            row_or[i] |= A[i][j]
    for j in range(n):
        for i in range(m):
            col_or[j] |= A[i][j]
    valid = True
    for i in range(m):
        for j in range(n):
            if (row_or[i]|col_or[j]) != B[i][j]:
                valid = False
                break
        if not valid:
            break
    if valid:
        print("YES")
        for row in A:
            print(' '.join(map(str, row)))
    else:
        print
```
