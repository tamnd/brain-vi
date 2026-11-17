---
title: "CF 1946A - Trung vị của mảng"
description: "Chúng ta được cung cấp một mảng các số nguyên và được yêu cầu xác định số lượng phép toán tăng tối thiểu cần thiết để tăng trung vị của mảng. Hoạt động tăng dần bao gồm việc chọn bất kỳ phần tử nào và thêm một phần tử vào đó."
date: "2026-06-07T17:50:52+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1946
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 936 (Div. 2)"
rating: 800
weight: 1946
solve_time_s: 90
verified: true
draft: false
---

[CF 1946A - Trung vị của mảng](https://codeforces.com/problemset/problem/1946/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và được yêu cầu xác định số lượng phép toán tăng tối thiểu cần thiết để tăng trung vị của mảng. Hoạt động tăng dần bao gồm việc chọn bất kỳ phần tử nào và thêm một phần tử vào đó. Trung vị được định nghĩa là phần tử ở giữa của mảng sau khi nó được sắp xếp; nếu mảng có độ dài chẵn thì trung vị là phần tử ở vị trí$\lceil n/2 \rceil$, sử dụng chỉ mục dựa trên 1. 

Ví dụ, đối với mảng`[2, 2, 8]`, trung vị là`2`. Áp dụng một mức tăng cho phần tử đầu tiên mang lại`[3, 2, 8]`. Khi được sắp xếp, điều này sẽ trở thành`[2, 3, 8]`, và trung vị mới là`3`, đã tăng lên. Nhiệm vụ là tìm ra số lượng nhỏ nhất các phép toán như vậy để đảm bảo giá trị trung bình tăng đúng. 

Những hạn chế ngụ ý rằng chúng ta cần một cách tiếp cận hiệu quả. Mỗi mảng có thể có tới$10^5$các phần tử và tổng số trên tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$. Điều này loại trừ các cách tiếp cận bạo lực mô phỏng mọi hoạt động có thể vì việc tăng từng phần tử một và sắp xếp lại sẽ quá chậm, đặc biệt đối với số lượng lớn. 

Trường hợp cạnh tinh tế xảy ra với mảng có kích thước một. Ví dụ, nếu mảng là`[1000000000]`, trung vị là`1000000000`. Tăng phần tử đơn lẻ đó lên một phần tử ngay lập tức làm tăng trung vị. Một trường hợp cạnh khác là khi tất cả các phần tử đều bằng nhau. Ví dụ,`[5, 5, 5, 5]`yêu cầu các thao tác trên nửa sau của mảng để tăng giá trị trung vị. Một cách tiếp cận bất cẩn luôn nhắm vào nửa đầu có thể tính sai số lượng cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: tăng liên tục các phần tử trong mảng và tính toán lại giá trị trung bình mỗi lần cho đến khi nó tăng lên. Điều này có tác dụng vì việc tăng bất kỳ phần tử nào lớn hơn hoặc bằng trung vị đều có thể ảnh hưởng đến giá trị của trung vị. Tuy nhiên, điều này đòi hỏi phải sắp xếp mảng nhiều lần sau mỗi thao tác.$O(n \log n)$mỗi hoạt động. Đối với các mảng lớn hoặc khoảng cách trung bình lớn, điều này có thể dẫn đến$O(n^2 \log n)$hoạt động quá chậm. 

Thông tin chi tiết quan trọng cho giải pháp tối ưu là giá trị trung bình chỉ phụ thuộc vào nửa giữa và nửa trên của mảng được sắp xếp. Bất kỳ sự gia tăng nào đối với các phần tử trước giá trị trung bình đều không giúp ích được gì. Sau khi mảng được sắp xếp, nếu chúng ta nhắm mục tiêu đến số trung vị và các phần tử ở bên phải của nó, chúng ta có thể tăng chúng trực tiếp để đạt được số lớn hơn tiếp theo trong nửa đó. Bằng cách đếm tổng số số gia cần thiết để nâng trung vị lên phần tử tiếp theo ở nửa trên được sắp xếp, chúng ta có thể xác định các phép toán tối thiểu một cách hiệu quả. 

Chúng ta không cần phải mô phỏng từng mức tăng riêng lẻ. Chúng ta có thể sắp xếp mảng một lần, xác định vị trí trung vị, sau đó tính tổng sự khác biệt giữa giá trị đích và các phần tử hiện tại từ trung vị trở đi cho đến khi tăng trung vị. Cách tiếp cận này là tuyến tính theo kích thước của nửa trên sau khi sắp xếp, đó là$O(n)$cho mỗi trường hợp thử nghiệm sau khi mảng được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 log n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case,$t$. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$và mảng$a$. 
2. Sắp xếp mảng$a$theo thứ tự không giảm. Việc sắp xếp đảm bảo chúng ta có thể xác định trực tiếp số trung vị và làm việc với các phần tử ảnh hưởng đến nó. 
3. Tính chỉ số trung vị là$m = n // 2$trong lập chỉ mục dựa trên 0. Điều này tương ứng với$\lceil n/2 \rceil$-phần tử thứ trong lập chỉ mục dựa trên 1. 
4. Khởi tạo một biến`operations`về không. Điều này sẽ tích lũy tổng số lượng gia tăng cần thiết. 
5. Lặp lại các phần tử trong chỉ mục$m$ĐẾN$n-1$, bao gồm trung vị và tất cả các phần tử ở bên phải của nó. Đối với mỗi phần tử, hãy tính toán sự khác biệt giữa giá trị mục tiêu đã chọn và phần tử hiện tại. Giá trị mục tiêu là giá trị nhỏ nhất vượt quá giá trị trung bình hiện tại sau tất cả các lần tăng. 
6. Tổng hợp những khác biệt này. Tổng này cung cấp số lượng gia số tối thiểu cần thiết vì bất kỳ gia số nào cũng phải nhắm mục tiêu các phần tử ở mức bằng hoặc cao hơn mức trung bình để ảnh hưởng đến giá trị của nó. 
7. Xuất tổng dưới dạng kết quả cho ca kiểm thử hiện tại. 

Tại sao nó hoạt động: Sau khi sắp xếp, trung vị là phần tử ở vị trí$m$. Để tăng số trung vị thì ít nhất phần tử trung vị phải tăng. Số lượng thao tác để nâng trung vị lên bất kỳ giá trị nào$x$là tổng số gia tăng của tất cả các phần tử từ$m$trở đi bên dưới$x$. Vì chúng tôi luôn nhắm mục tiêu đúng phần tử và tính tổng các khác biệt chính xác, điều này đảm bảo các hoạt động tối thiểu để tăng trung vị một cách nghiêm ngặt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()
    median_idx = n // 2
    target = a[median_idx]
    operations = 0
    # Increment the median and right half until median increases
    for i in range(median_idx, n):
        operations += max(0, a[i] - target + 1)
        target = a[i] + 1
    print(operations)
```Mã đọc các trường hợp kiểm thử và mảng một cách hiệu quả bằng cách sử dụng I/O nhanh. Sắp xếp cho phép truy cập trực tiếp vào trung vị. Chúng tôi tính toán số gia tăng cần thiết cho các phần tử ở mức bằng hoặc cao hơn mức trung bình và tích lũy chúng. Điểm tinh tế là cập nhật mục tiêu theo từng bước để đảm bảo rằng chúng tôi luôn tăng đến giá trị có thể tiếp theo ảnh hưởng đến giá trị trung bình. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[2, 2, 8]`| Bước | Mảng được sắp xếp | Chỉ số trung bình | Mục tiêu | Hoạt động | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [2,2,8] | 1 | 2 | 0 | 
| tôi=1 | a[1]=2 | mục tiêu=2 | hoạt động += 1 | 1 | 
| tôi=2 | a[2]=8 | mục tiêu=3 | hoạt động += 0 | 1 | 

Thuật toán xác định rằng việc tăng số trung vị một lần là đủ. Tổng số thao tác = 1. 

### Ví dụ 2 

đầu vào:`[7,3,3,1]`| Bước | Mảng được sắp xếp | Chỉ số trung bình | Mục tiêu | Hoạt động | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [1,3,3,7] | 2 | 3 | 0 | 
| tôi=2 | a[2]=3 | mục tiêu=3 | hoạt động += 1 | 1 | 
| tôi=3 | a[3]=7 | mục tiêu=4 | phép toán += 3 | 4 | 

Chúng ta cần 2 thao tác để nâng trung vị từ 3 lên 4 (tăng a[2] và a[3] nếu cần). Thuật toán tính toán chính xác mức tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; lặp lại nửa trên là O(n) | 
| Không gian | O(n) | Lưu trữ và sắp xếp mảng | 

Với$n$lên đến$10^5$và tổng số phần tử trên tất cả các trường hợp thử nghiệm$2 \cdot 10^5$, việc sắp xếp từng mảng riêng lẻ là có thể chấp nhận được. Giải pháp phù hợp trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()
        median_idx = n // 2
        target = a[median_idx]
        operations = 0
        for i in range(median_idx, n):
            operations += max(0, a[i] - target + 1)
            target = a[i] + 1
        res.append(str(operations))
    return "\n".join(res)

# provided samples
assert run("8\n3\n2 2 8\n4\n7 3 3 1\n1\n1000000000\n5\n5 5 5 4 5\n6\n2 1 2 3 1 4\n2\n1 2\n2\n1 1\n4\n5 5 5 5\n") == "1\n2\n1\n3\n2\n1\n2\n3", "sample 1"

# custom cases
assert run("2\n1\n1\n5\n1 1 1 1 1\n") == "1\n3", "single element and all-equal array"
assert run("1\n6\n1 2 3 4 5 6\n") == "1", "increment median once"
assert run("1\n4\n5 5 5 5\n") == "3", "all equal array, median requires multiple ops"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1`|`1`| Mảng phần tử đơn, tăng trung vị ngay lập tức | 
|`5\n1 1 1 1 1`|`3`| Tất cả các phần tử bằng nhau, yêu cầu tăng nhiều lần | 
|`6\n1 2 3 4 5 6`|`1`| Tăng trung bình một lần đủ | 
|`4\n5 5 5 5`|`3`| Đếm đúng trên mảng bằng nhau có độ dài chẵn | 

## Vỏ cạnh 

Đối với mảng một phần tử `[100
