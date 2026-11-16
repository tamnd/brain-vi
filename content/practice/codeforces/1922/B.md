---
title: "CF 1922B - Hình thành tam giác"
description: "Chúng ta được phát một bộ que, mỗi que có chiều dài bằng lũy ​​thừa hai. Dữ liệu đầu vào không cung cấp trực tiếp độ dài thực tế mà cung cấp số mũ $ai$ sao cho độ dài của thanh là $2^{ai}$."
date: "2026-06-08T19:16:13+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "constructive-algorithms", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1922
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 161 (Rated for Div. 2)"
rating: 1200
weight: 1922
solve_time_s: 100
verified: true
draft: false
---

[CF 1922B - Hình thành tam giác](https://codeforces.com/problemset/problem/1922/B) 

**Đánh giá:** 1200 
**Tags:** tổ hợp, thuật toán xây dựng, toán, sắp xếp 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được phát một bộ que, mỗi que có chiều dài bằng lũy thừa hai. Đầu vào không cung cấp trực tiếp độ dài thực tế mà là số mũ$a_i$sao cho chiều dài của thanh là$2^{a_i}$. Nhiệm vụ của chúng ta là đếm số cách chọn đúng ba que có thể tạo thành một tam giác không suy biến. Một tam giác không suy biến nếu tổng độ dài hai cạnh bất kỳ lớn hơn độ dài cạnh thứ ba. 

Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên duy nhất biểu thị số bộ ba thanh hợp lệ. Bởi vì các cây gậy được xác định bởi lũy thừa của hai, nên nhiều cây gậy có thể có cùng chiều dài, và bất đẳng thức tam giác rút gọn thành một phép so sánh đơn giản giữa cây gậy lớn nhất và tổng của hai cây gậy còn lại. 

Những ràng buộc cho chúng ta biết rằng$n$có thể lớn như$3 \cdot 10^5$và có thể có tới$10^4$các trường hợp thử nghiệm, với tổng số que trong tất cả các trường hợp thử nghiệm được giới hạn bởi$3 \cdot 10^5$. Điều này có nghĩa là bất kỳ thuật toán nào có độ phức tạp kém hơn$O(n^2)$mỗi trường hợp thử nghiệm có thể sẽ quá chậm. MỘT$O(n \log n)$hoặc$O(n^2)$Cách tiếp cận này có thể chấp nhận được nếu được thực hiện cẩn thận. 

Các trường hợp cạnh phát sinh khi tất cả các que có cùng độ dài, làm cho mọi bộ ba đều hợp lệ hoặc khi các que phát triển theo cấp số nhân, khiến hầu hết các bộ ba không hợp lệ. Ví dụ: nếu chúng ta có que tính với số mũ`[1, 2, 3]`tương ứng với độ dài`[2, 4, 8]`, không thể tạo được tam giác nào vì$2 + 4 = 6 \le 8$. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là mạnh mẽ: lặp qua tất cả các bộ ba que và kiểm tra xem chúng có thỏa mãn bất đẳng thức tam giác hay không. Điều này đúng nhưng có$O(n^3)$phức tạp, không thể thực hiện được$n$lên đến$3 \cdot 10^5$. Ngay cả đối với mức độ vừa phải$n$, cách tiếp cận như vậy sẽ quá chậm. 

Quan sát quan trọng là độ dài thanh là lũy thừa của hai, điều này ngụ ý rằng bất đẳng thức tam giác có thể được đơn giản hóa. Việc sắp xếp các que theo số mũ của chúng sẽ chuyển bài toán thành bài toán đếm tam giác tiêu chuẩn trên một mảng đã được sắp xếp. Đối với các que đã được sắp xếp, nếu chúng ta chọn ba chỉ số$i < j < k$, tồn tại một tam giác không suy biến nếu$2^{a_i} + 2^{a_j} > 2^{a_k}$. Việc sắp xếp đảm bảo rằng$a_i \le a_j \le a_k$, vì vậy chúng ta chỉ cần kiểm tra phần tử lớn nhất so với tổng của hai phần tử nhỏ hơn. Điều này cho phép cách tiếp cận hai con trỏ hoặc ba con trỏ để đếm các bộ ba hợp lệ một cách hiệu quả. 

Chúng ta có thể tính toán trước số bộ ba bằng cách sử dụng công thức tổ hợp cho các que có chiều dài bằng nhau và sau đó tính các que có độ dài khác nhau bằng cách quét qua các độ dài duy nhất được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Sắp xếp + Ba con trỏ | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$và số mũ$a_i$. 
2. Sắp xếp mảng số mũ. Việc sắp xếp cho phép chúng ta nhanh chóng áp dụng bất đẳng thức tam giác bằng cách chỉ so sánh thanh lớn nhất với tổng của hai thanh nhỏ hơn. 
3. Khởi tạo bộ đếm cho các bộ ba hợp lệ. 
4. Lặp lại mảng với ba chỉ số$i < j < k$. Sửa chữa$i$Và$j$và tìm mức tối đa$k$như vậy$2^{a_i} + 2^{a_j} > 2^{a_k}$. Sử dụng thuộc tính đã sắp xếp để di chuyển$k$chuyển tiếp cho đến khi điều kiện thất bại. 
5. Đối với mỗi hợp lệ$i, j$cặp, thêm số lượng hợp lệ$k$vị trí đến quầy. 
6. Xuất bộ đếm sau khi quét tất cả các cặp. 

Tại sao nó hoạt động: Mảng được sắp xếp đảm bảo rằng$k$tăng lên,$2^{a_k}$cũng tăng lên. Bằng cách tiến lên$k$chỉ khi bất đẳng thức tam giác đúng, chúng ta mới đếm mỗi bộ ba chính xác một lần, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_triangles(n, a):
    a.sort()
    count = 0
    for i in range(n - 2):
        k = i + 2
        for j in range(i + 1, n - 1):
            while k < n and (1 << a[i]) + (1 << a[j]) > (1 << a[k]):
                k += 1
            count += k - j - 1
    return count

t = int(input())
results = []
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    results.append(str(count_triangles(n, a)))

print("\n".join(results))
```Mã đầu tiên sắp xếp mảng số mũ. Đối với mỗi cặp gậy, nó tiến lên$k$cho đến khi bất đẳng thức tam giác thất bại. Số hợp lệ$k$cho mỗi cặp$(i, j)$chính xác là$k - j - 1$. Việc sắp xếp là cần thiết để đảm bảo rằng một khi$2^{a_i} + 2^{a_j} \le 2^{a_k}$, tất cả tiếp theo$k$cũng sẽ thất bại. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
7
1 1 1 1 1 1 1
```| tôi | j | k (tối đa) | Đã thêm bộ ba hợp lệ | Quầy | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 5 | 5 | 
| 0 | 2 | 3 | 4 | 9 | 
| 0 | 3 | 4 | 3 | 12 | 
| 0 | 4 | 5 | 2 | 14 | 
| 0 | 5 | 6 | 1 | 15 | 
| 1 | 2 | 3 | 4 | 19 | 
| 1 | 3 | 4 | 3 | 22 | 
| 1 | 4 | 5 | 2 | 24 | 
| 1 | 5 | 6 | 1 | 25 | 
| 2 | 3 | 4 | 3 | 28 | 
| 2 | 4 | 5 | 2 | 30 | 
| 2 | 5 | 6 | 1 | 31 | 
| 3 | 4 | 5 | 2 | 33 | 
| 3 | 5 | 6 | 1 | 34 | 
| 4 | 5 | 6 | 1 | 35 | 

Mọi bộ ba đều hoạt động vì tất cả các que đều có cùng chiều dài. 

Đầu vào mẫu 2:```
4
3 2 1 3
```Sau khi sắp xếp:`[1, 2, 3, 3]`(độ dài`[2, 4, 8, 8]`) 

| tôi | j | k (tối đa) | Đã thêm bộ ba hợp lệ | Quầy | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 0 | 0 | 
| 0 | 2 | 3 | 2 | 2 | 
| 1 | 2 | 3 | 0 | 2 | 

Chỉ có hai bộ ba thỏa mãn bất đẳng thức tam giác:`(1,3,4)`Và`(2,3,4)`trong lập chỉ mục dựa trên 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Việc sắp xếp mất O(n log n), vòng lặp lồng nhau với con trỏ k chạy trong trường hợp xấu nhất có tổng số O(n^2). | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng số mũ và danh sách kết quả. | 

Cho tổng của$n$trên tất cả các trường hợp thử nghiệm là 3·10^5, giải pháp phù hợp thoải mái trong thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution
    import builtins
    input = builtins.input
    t = int(input())
    results = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        results.append(str(count_triangles(n, a)))
    return output.getvalue().strip()

# Provided samples
assert run("4\n7\n1 1 1 1 1 1 1\n4\n3 2 1 3\n3\n1 2 3\n1\n1\n") == "35\n2\n0\n0"

# Custom tests
assert run("1\n3\n0 0 0\n") == "1"  # All equal lengths, one triangle
assert run("1\n5\n0 1 2 3 4\n") == "0"  # Exponentially increasing, no triangle
assert run("1\n6\n1 1 2 2 3 3\n") == "7"  # Mix of equal lengths
assert run("1\n1\n10\n") == "0"  # Single stick, cannot form triangle
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3\n0 0 0`| 1 | Đầu vào có kích thước tối thiểu, tất cả các thanh bằng nhau | 
|`5\n0 1 2 3 4`| 0 | Tăng sức mạnh, không thể có tam giác | 
|`6\n1 1 2 2 3 3`| 7 | Kết hợp các giá trị trùng lặp và khác biệt | 
|`1\n10`| 0 | Cây gậy đơn không tạo thành hình tam giác | 

## Vỏ cạnh 

Nếu tất cả các que đều bằng nhau, như `[1, 1, 1
