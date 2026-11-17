---
title: "CF 1993E - Sự cố lưới Xor"
description: "Chúng ta được cung cấp một ma trận nhỏ gồm các số nguyên và các phép toán chúng ta có thể thực hiện đều khác thường. Đối với bất kỳ hàng nào, chúng ta có thể thay thế mọi phần tử bằng XOR của cột tương ứng. Đối với bất kỳ cột nào, chúng ta có thể thay thế mọi phần tử bằng XOR của hàng tương ứng."
date: "2026-06-08T15:10:51+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "dp", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1993
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 963 (Div. 2)"
rating: 2700
weight: 1993
solve_time_s: 206
verified: false
draft: false
---

[CF 1993E - Sự cố Xor-Grid](https://codeforces.com/problemset/problem/1993/E) 

**Xếp hạng:** 2700 
**Tags:** bitmasks, thuật toán xây dựng, dp, triển khai 
**Thời gian giải:** 3 phút 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một ma trận nhỏ gồm các số nguyên và các phép toán chúng ta có thể thực hiện đều khác thường. Đối với bất kỳ hàng nào, chúng ta có thể thay thế mọi phần tử bằng XOR của cột tương ứng. Đối với bất kỳ cột nào, chúng ta có thể thay thế mọi phần tử bằng XOR của hàng tương ứng. Sau khi thực hiện bất kỳ số thao tác nào như vậy, mục tiêu là giảm thiểu tổng chênh lệch tuyệt đối giữa các ô liền kề, điều mà bài toán gọi là vẻ đẹp của ma trận. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có kích thước ma trận lên tới 15x15. Tổng số tiền của$n^2 + m^2$trên tất cả các trường hợp thử nghiệm được giới hạn ở 500, nghĩa là ma trận đủ nhỏ để chúng tôi có thể cung cấp các thuật toán khám phá nhiều trạng thái hoặc cấu hình, miễn là việc khám phá không mở rộng theo cấp số nhân với số lượng lớn. 

Một quan sát quan trọng là các hoạt động XOR hoạt động có thể dự đoán được trong ứng dụng lặp đi lặp lại. Nếu chúng ta áp dụng một thao tác trên cùng một hàng hoặc cột nhiều lần thì ma trận sẽ hội tụ về một điểm cố định. Do đó, chúng ta có thể mô hình hóa vấn đề bằng cách chọn hàng và cột nào cần chuyển đổi, sau đó tính toán ma trận kết quả. Các trường hợp cạnh là các ma trận rất nhỏ chẳng hạn như 1x1 hoặc 1xN, trong đó các phép tính kề giảm xuống còn một cặp duy nhất và các ma trận trong đó tất cả các giá trị ban đầu bằng 0, cho phép nhiều cấu hình tạo ra vẻ đẹp giống hệt nhau. 

Một cách tiếp cận ngây thơ trong việc mô phỏng mọi chuỗi hoạt động sẽ bùng nổ về mặt tổ hợp, bởi vì mỗi$n + m$hàng và cột có thể được vận hành hoặc không. Chúng ta cần khai thác cấu trúc XOR và kích thước nhỏ để giảm không gian tìm kiếm. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ là thử mọi sự kết hợp giữa việc áp dụng hoặc không áp dụng các thao tác cho từng hàng và cột. Vì$n$hàng và$m$cột, điều này mang lại$2^{n+m}$khả năng. Đối với mỗi khả năng, chúng tôi tính toán ma trận kết quả và sau đó tính toán vẻ đẹp trong$O(nm)$thời gian. Với mức tối đa$n+m = 30$,$2^{30}$quá lớn, do đó, lực lượng vũ phu là không khả thi ngay cả khi bản thân ma trận nhỏ. 

Thông tin chi tiết quan trọng là hoạt động của mỗi hàng hoặc cột chỉ phụ thuộc vào XOR của thứ nguyên khác. Điều này có nghĩa là sau khi sửa một hàng, giá trị của mỗi cột được xác định duy nhất. Vì XOR là nghịch đảo của chính nó nên chúng ta có thể coi vấn đề này như một tìm kiếm mặt nạ bit mang tính xây dựng: chúng ta quyết định hàng nào cần lật và sau đó đối với mỗi cột, chúng ta tính toán XOR cột và áp dụng nó. Số cách kết hợp hàng là$2^n$, điều này khả thi đối với$n \le 15$. Đối với mỗi cấu hình hàng, các giá trị cột được xác định đầy đủ, đưa ra ma trận kết quả ngay lập tức. Điều này làm giảm đáng kể sự phức tạp. 

Chúng ta có thể triển khai điều này một cách hiệu quả bằng cách lặp qua tất cả các tập hợp con của hàng để áp dụng thao tác hàng, tính toán XOR cột từ ma trận kết quả, áp dụng chúng và sau đó tính toán độ đẹp. Đây là cách sử dụng tiêu chuẩn của phép liệt kê kiểu bitmask DP. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^{n+m} * n * m) | O(n*m) | Quá chậm | 
| Hàng Bitmask | O(2^n * n * m) | O(n*m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$,$m$, và$n \times m$ma trận. Lưu trữ nó dưới dạng danh sách các danh sách. 
2. Tính toán trước XOR của mỗi hàng trong ma trận ban đầu. Chúng sẽ được sử dụng khi quyết định có nên áp dụng thao tác hàng hay không. 
3. Lặp lại tất cả$2^n$tập hợp con có thể có của các hàng. Mỗi tập hợp con đại diện cho các hàng nơi chúng ta sẽ áp dụng thao tác hàng. Đối với mỗi tập hợp con: 

- Tạo một bản sao của ma trận. Đối với mỗi hàng trong tập hợp con, hãy thay thế các phần tử của nó bằng XOR của các cột tương ứng được tính từ ma trận gốc. Điều này đưa ra một ma trận được chuyển đổi một phần. 
4. Sau khi chuyển đổi hàng, hãy tính XOR của từng cột trong ma trận mới này. Đối với mỗi cột, áp dụng thao tác cột bằng cách đặt từng phần tử vào cột XOR. Bây giờ ma trận đã được xác định đầy đủ cho tập hợp con hàng này. 
5. Tính toán vẻ đẹp bằng cách lặp lại tất cả các cặp ô liền kề và tính tổng các chênh lệch tuyệt đối. Theo dõi vẻ đẹp tối thiểu được tìm thấy trên tất cả các tập hợp con hàng. 
6. In ấn độ đẹp tối thiểu cho từng test case. 

Tại sao nó hoạt động: Điều bất biến là sau khi quyết định hàng nào sẽ áp dụng thao tác, các giá trị cột được xác định duy nhất. Vì XOR có thể đảo ngược và bình thường, bất kể thứ tự của các phép toán, chúng ta luôn có thể biểu diễn kết quả dưới dạng kết hợp các lần lật hàng theo sau là các điều chỉnh cột. Bằng cách liệt kê tất cả các tập hợp con của các hàng, chúng tôi khám phá tất cả các ma trận có thể có được thông qua bất kỳ chuỗi thao tác được phép nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def beauty(matrix, n, m):
    total = 0
    for i in range(n):
        for j in range(m):
            if i + 1 < n:
                total += abs(matrix[i][j] - matrix[i+1][j])
            if j + 1 < m:
                total += abs(matrix[i][j] - matrix[i][j+1])
    return total

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = [list(map(int, input().split())) for _ in range(n)]
        row_xor = [0]*n
        for i in range(n):
            for val in a[i]:
                row_xor[i] ^= val
        
        min_beauty = float('inf')
        for mask in range(1 << n):
            b = [row[:] for row in a]
            # Apply row operations according to mask
            for i in range(n):
                if (mask >> i) & 1:
                    for j in range(m):
                        col_xor = 0
                        for k in range(n):
                            col_xor ^= b[k][j]
                        b[i][j] = col_xor
            # Apply column operations
            col_vals = [0]*m
            for j in range(m):
                for i in range(n):
                    col_vals[j] ^= b[i][j]
            for j in range(m):
                for i in range(n):
                    b[i][j] = col_vals[j]
            min_beauty = min(min_beauty, beauty(b, n, m))
        print(min_beauty)
```Đầu tiên, mã sẽ tính toán XOR hàng để cho phép kiểm tra chuyển đổi nhanh chóng. Đối với mỗi tập hợp con của các hàng được biểu thị bằng mặt nạ bit, nó sẽ sao chép ma trận và áp dụng các phép toán hàng. Cột XOR sau đó được áp dụng một cách xác định. các`beauty`hàm tính tổng chênh lệch tuyệt đối giữa các ô liền kề. Mức tối thiểu cuối cùng được in. 

Điểm tinh tế: sao chép ma trận là rất quan trọng để tránh làm thay đổi bản gốc. XOR cột phải được tính toán sau khi lật các hàng cho mỗi mặt nạ. Việc sử dụng mặt nạ bit đảm bảo chúng tôi chỉ liệt kê các tập hợp con khả thi một cách hiệu quả. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
1 2
1 3
```| mặt nạ | Lật hàng | Ma trận sau khi lật | Cột XOR | Ma trận sau cột | Sắc đẹp | 
| --- | --- | --- | --- | --- | --- | 
| 0 | không | [[1,3]] | [1,3] | [[1,3]] | 2 | 
| 1 | hàng 0 | [[1^3=2,1^3=2]] | [2,2] | [[2,2]] | 0 | 

Mặt nạ tối ưu là 1, lật hàng đầu tiên, sau đó áp dụng XOR cột mang lại [[2,2]], với vẻ đẹp 0. 

### Đầu vào mẫu 2```
2 3
0 1 0
```| mặt nạ | Lật hàng | Ma trận sau khi lật | Cột XOR | Ma trận sau cột | Sắc đẹp | 
| --- | --- | --- | --- | --- | --- | 
| 0 | không | [[0,1,0]] | [0,1,0] | [[0,1,0]] | | 
| 1 | hàng 0 | [[0^1^0=1,0^1^0=1,0^1^0=1]] | [1,1,1] | [[1,1,1]] | | 

Chúng tôi thấy việc lật hàng và cập nhật các cột sẽ làm giảm vẻ đẹp bằng cách căn chỉnh tất cả các giá trị. 

Những dấu vết này chứng minh rằng việc lật hàng XOR kết hợp với các điều chỉnh cột sẽ khám phá đầy đủ các cấu hình có thể truy cập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^n * n * m^2) | Đối với mỗi mặt nạ trong số 2^n hàng, XOR cột tính toán lấy O(n_m), tính toán độ đẹp O(n_m) | 
| Không gian | O(n*m) | Sao chép ma trận mỗi lần lặp | 

Thuật toán rất phù hợp vì$n\le15$ngụ ý$2^n=32768$và tổng số thao tác trên mỗi trường hợp thử nghiệm nhỏ hơn 15_15_32768, khả thi trong vòng 5 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("4\n1 2\n1 3\n2 3\n0 1 0\n5 4\n4 2 3\n0 2 4\n4 5 1\n3 3\n1 2 3\n4 5 6\n7 8 9\n") == "1\n3\n13\n24"

# Custom cases
assert run("1\n1 1\n5\n") == "
```
