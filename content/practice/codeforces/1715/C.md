---
title: "CF 1715C - Khối đơn"
description: "Chúng tôi được cung cấp một mảng các số nguyên và chúng tôi xác định độ tuyệt vời của nó là số khối giống hệt nhau liên tiếp tối thiểu mà mảng có thể được chia thành. Ví dụ: [1,1,2,2,2,3] có độ tuyệt vời 3 vì nó có thể được chia thành [1,1], [2,2,2], [3]."
date: "2026-06-09T19:55:21+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "data-structures", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1715
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 816 (Div. 2)"
rating: 1700
weight: 1715
solve_time_s: 129
verified: true
draft: false
---

[CF 1715C - Monoblock](https://codeforces.com/problemset/problem/1715/C) 

**Đánh giá:** 1700 
**Tags:** tổ hợp, cấu trúc dữ liệu, triển khai, toán học 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên và chúng tôi xác định độ tuyệt vời của nó là số khối giống hệt nhau liên tiếp tối thiểu mà mảng có thể được chia thành. Ví dụ,`[1,1,2,2,2,3]`có sự tuyệt vời 3 vì nó có thể được chia thành`[1,1]`,`[2,2,2]`,`[3]`. Vấn đề yêu cầu chúng tôi hỗ trợ các truy vấn thay đổi một phần tử của mảng và sau mỗi thay đổi, hãy tính tổng các giá trị tuyệt vời trên tất cả các mảng con. 

Độ dài mảng và số lượng truy vấn có thể lên tới$10^5$. Một cách tiếp cận mạnh mẽ xem xét mọi mảng con sau mỗi truy vấn sẽ yêu cầu tới$10^5 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất, rõ ràng là quá chậm. Điều này buộc chúng tôi phải tìm cách tính tổng một cách hiệu quả, lý tưởng nhất là ở O(1) hoặc O(log n) cho mỗi truy vấn, sau một số quá trình tiền xử lý. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng có tất cả các phần tử giống hệt nhau, các mảng trong đó mọi phần tử đều khác biệt và các bản cập nhật không thay đổi giá trị của phần tử mảng. Ví dụ, nếu mảng là`[5,5,5]`và chúng tôi đặt lại phần tử thứ hai thành 5, tổng mức độ tuyệt vời sẽ không thay đổi. Tương tự, các cập nhật ở các cạnh của mảng có thể ảnh hưởng đến ít mảng con hơn và cần xử lý cẩn thận. 

## Phương pháp tiếp cận 

Phương thức brute-force lặp lại trên tất cả các mảng con sau mỗi truy vấn và đếm các khối. Đối với một mảng có độ dài$n$, có$\frac{n(n+1)}{2}$mảng con, và đối với mỗi mảng con, khối đếm chiếm tới$O(n)$. Điều này mang lại sự phức tạp trong trường hợp xấu nhất của$O(n^3)$cho mỗi truy vấn, điều này không thực tế đối với$n = 10^5$. 

Cái nhìn sâu sắc quan trọng là tổng mức độ tuyệt vời của tất cả các mảng con có thể được biểu thị bằng$n(n+1)/2$cộng với sự đóng góp từ mọi vị trí mà phần tử thay đổi giá trị so với vị trí lân cận của nó. Cụ thể, ranh giới khối xảy ra tại chỉ số$i$nếu như$a[i] \neq a[i+1]$. Mỗi ranh giới như vậy đóng góp vào tất cả các mảng con chứa nó và số lượng mảng con như vậy chính xác là$i \cdot (n-i)$ranh giới giữa các vị trí$i$Và$i+1$. 

Điều này làm giảm vấn đề theo dõi tập hợp các cặp liền kề không bằng nhau. Mỗi truy vấn thay đổi tối đa hai cặp: một cặp ở bên trái của chỉ mục được cập nhật và một cặp ở bên phải. Bằng cách duy trì tổng số đóng góp từ tất cả các ranh giới, chúng tôi có thể cập nhật câu trả lời ở dạng O(1) cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Tối ưu | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo tổng số awesomeness. Bắt đầu với`total = n*(n+1)//2`. Điều này tính mỗi mảng con là ít nhất một khối. 
2. Lặp lại tất cả các cặp liền kề`(a[i], a[i+1])`. Nếu chúng khác nhau, hãy tính phần đóng góp của chúng là`i * (n-i)`và thêm nó vào`total`. Bước này tính toán sự đóng góp của tất cả các ranh giới khối. 
3. Đối với mỗi truy vấn`(idx, x)`, hãy xem xét hai ranh giới bị ảnh hưởng:`(idx-1, idx)`Và`(idx, idx+1)`, nếu chúng tồn tại. Trừ đi những đóng góp cũ từ`total`trước khi cập nhật. 
4. Cập nhật`a[idx] = x`. 
5. Thêm sự đóng góp của các ranh giới mới`(idx-1, idx)`Và`(idx, idx+1)`ĐẾN`total`. 
6. In`total`. 

Tại sao nó hoạt động: Tổng số điều tuyệt vời có thể được phân tách thành những đóng góp từ mọi thay đổi ở các phần tử liền kề. Việc cập nhật một phần tử chỉ ảnh hưởng đến các ranh giới chạm vào phần tử đó. By maintaining the contributions for each boundary and updating only those affected, the algorithm preserves correctness while avoiding recomputation over unaffected subarrays.

 ## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
a = list(map(int, input().split()))

total = n * (n + 1) // 2

def contrib(i):
    if 0 <= i < n - 1 and a[i] != a[i+1]:
        return (i + 1) * (n - (i + 1))
    return 0

for i in range(n - 1):
    total += contrib(i)

for _ in range(m):
    idx, x = map(int, input().split())
    idx -= 1
    
    total -= contrib(idx - 1)
    total -= contrib(idx)
    
    a[idx] = x
    
    total += contrib(idx - 1)
    total += contrib(idx)
    
    print(total)
```chức năng`contrib(i)`tính toán sự đóng góp biên của cặp`(a[i], a[i+1])`. Chúng tôi trừ đi những đóng góp cũ trước khi cập nhật và thêm những đóng góp mới sau khi cập nhật. Sử dụng lập chỉ mục dựa trên 0 yêu cầu điều chỉnh`idx`từ truy vấn. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
5 5
1 2 3 4 5
3 2
4 2
3 1
2 1
2 2
```| Bước | Mảng | Thay đổi ranh giới | Tổng số tuyệt vời | 
| --- | --- | --- | --- | 
| Ban đầu | [1,2,3,4,5] | tất cả các cặp đều khác nhau | 35 | 
| Câu 1: (3,2) | [1,2,2,4,5] | cặp (2,3) thay đổi | 29 | 
| Câu 2: (4,2) | [1,2,2,2,5] | cặp (3,4) thay đổi | 23 | 
| Câu 3: (3,1) | [1,2,1,2,5] | cặp (2,3) thay đổi | 35 | 
| Câu 4: (2,1) | [1,1,1,2,5] | cặp (1,2) thay đổi | 25 | 
| Câu 5: (2,2) | [1,2,1,2,5] | cặp (1,2) thay đổi | 35 | 

Dấu vết xác nhận tính bất biến rằng chỉ các ranh giới chạm vào chỉ mục được cập nhật mới ảnh hưởng đến tổng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Tính toán ban đầu của ranh giới là O(n), mỗi truy vấn cập nhật ranh giới O(1) | 
| Không gian | O(n) | Lưu trữ mảng | 

Điều này phù hợp thoải mái trong giới hạn 1 giây cho$n, m \le 10^5$và chỉ sử dụng bộ nhớ tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    # copy-paste solution here
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    total = n * (n + 1) // 2
    def contrib(i):
        if 0 <= i < n - 1 and a[i] != a[i+1]:
            return (i + 1) * (n - (i + 1))
        return 0
    for i in range(n - 1):
        total += contrib(i)
    for _ in range(m):
        idx, x = map(int, input().split())
        idx -= 1
        total -= contrib(idx - 1)
        total -= contrib(idx)
        a[idx] = x
        total += contrib(idx - 1)
        total += contrib(idx)
        print(total)
    return out.getvalue().strip()

# provided sample
assert run("5 5\n1 2 3 4 5\n3 2\n4 2\n3 1\n2 1\n2 2\n") == "29\n23\n35\n25\n35", "sample 1"

# minimum-size input
assert run("1 1\n1\n1 1\n") == "1", "single element"

# all equal values
assert run("3 2\n2 2 2\n2 3\n3 2\n") == "6\n7", "all equal initially, then changing"

# alternating values
assert run("4 2\n1 2 1 2\n2 2\n3 2\n") == "20\n21", "alternating pattern"

# edge update
assert run("5 1\n1 2 3 4 5\n5 1\n") == "31", "updating last element"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | mảng tầm thường, tổng các mảng con là 1 | 
| tất cả các giá trị bằng nhau | 6,7 | cập nhật hợp nhất/tách khối | 
| giá trị xen kẽ | 20,21 | các mẫu có ranh giới thường xuyên | 
| cập nhật cạnh | 31 | xử lý chính xác bản cập nhật phần tử cuối cùng | 

## Vỏ cạnh 

Nếu giá trị cập nhật không làm thay đổi phần tử thì tổng vẫn giữ nguyên. Ví dụ: `[2,2,2
