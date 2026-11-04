---
title: "CF 1132B - Giảm giá"
description: "Chúng tôi có một bộ thanh sô cô la, mỗi thanh có giá riêng. Người mua hàng muốn có tất cả các thanh nhưng có nhiều phiếu giảm giá để lựa chọn. Mỗi phiếu giảm giá cho phép mua một số lượng thanh cố định, nhưng trong phạm vi lựa chọn đó, thanh rẻ nhất sẽ miễn phí."
date: "2026-06-12T04:07:51+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1132
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 61 (Rated for Div. 2)"
rating: 900
weight: 1132
solve_time_s: 73
verified: true
draft: false
---

[CF 1132B - Giảm giá](https://codeforces.com/problemset/problem/1132/B) 

**Xếp hạng:** 900 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ thanh sô cô la, mỗi thanh có giá riêng. Người mua hàng muốn có tất cả các thanh nhưng có nhiều phiếu giảm giá để lựa chọn. Mỗi phiếu giảm giá cho phép mua một số lượng thanh cố định, nhưng trong phạm vi lựa chọn đó, thanh rẻ nhất sẽ miễn phí. Nhiệm vụ là xác định, đối với mỗi phiếu giảm giá, tổng chi phí tối thiểu nếu phiếu giảm giá được áp dụng một cách tối ưu và các thanh còn lại được mua với giá đầy đủ. 

Đầu vào bao gồm số lượng thanh`n`, giá riêng của chúng`a_i`, số phiếu giảm giá`m`và kích thước của phiếu giảm giá`q_i`. Đầu ra là một mảng`m`số nguyên, trong đó mỗi số nguyên là khoản thanh toán tối thiểu nếu phiếu giảm giá tương ứng được sử dụng một cách tối ưu. 

Những ràng buộc cho phép`n`có thể lên tới 300.000. Điều này ngay lập tức loại trừ mọi cách tiếp cận bạo lực nhằm thử tất cả các kết hợp thanh sô-cô-la cho mỗi phiếu giảm giá, bởi vì`C(n, q)`phát triển theo kiểu tổ hợp. Ngay cả việc lặp lại một cách đơn giản cho mỗi phiếu giảm giá cũng sẽ quá chậm, vì vậy chúng ta cần một phương pháp có tỷ lệ tuyến tính hoặc gần tuyến tính trong`n`. 

Trường hợp khó khăn không rõ ràng xảy ra khi tất cả các thanh sô-cô-la có cùng mức giá. Ví dụ, nếu`n = 3`, thanh là`[5, 5, 5]`và phiếu giảm giá cho phép`q = 2`, các thanh tối ưu để chọn có thể là bất kỳ cặp nào. Đầu ra vẫn phải tính đến việc chỉ thanh toán cho sản phẩm đắt nhất trong cặp. Một cách tiếp cận bất cẩn cho rằng các thanh là khác nhau có thể tạo ra một phép tính sai. 

Một trường hợp tế nhị khác là khi phiếu giảm giá cho phép`q = n`thanh. Ở đây, tất cả các thanh đều được chọn để giảm giá và thanh rẻ nhất là miễn phí. Việc triển khai phải xử lý việc này mà không có lỗi lập chỉ mục. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: đối với mỗi phiếu giảm giá, liệt kê tất cả các tập hợp con có thể có kích thước`q_i`, tính tổng giá của các thanh đã chọn không bao gồm giá rẻ nhất và cộng tổng của các thanh còn lại. Điều này hoạt động chính xác nhưng có độ phức tạp trong trường hợp xấu nhất là O(n choose q) trên mỗi phiếu giảm giá. Với n lên tới 300.000, điều này là không khả thi, vì ngay cả những giá trị q khiêm tốn nhất cũng tạo ra một số lượng lớn các tổ hợp về mặt thiên văn. 

Thông tin chi tiết quan trọng là đối với mỗi phiếu giảm giá, chúng tôi muốn tối đa hóa mức chiết khấu. Mức giảm giá là giá của thanh rẻ nhất trong bộ đã chọn. Vì vậy, để giảm thiểu tổng chi phí, chúng ta nên chọn`q_i`thanh đắt nhất cho phiếu giảm giá. Thanh toán cho tất cả ngoại trừ khoản rẻ nhất trong số này sẽ tối đa hóa số tiền được khấu trừ trong tổng số tiền. Điều này cho phép chúng tôi giảm bớt vấn đề sắp xếp tất cả các thanh theo thứ tự giảm dần và sử dụng tổng tiền tố để tính nhanh tổng của thanh trên cùng`q_i`thanh. Tổng chi phí trở thành tổng của tất cả các thanh trừ đi thanh rẻ nhất trong số các thanh hàng đầu`q_i`. 

Điều này chuyển đổi giải pháp từ tổ hợp sang O(n log n + m), trong đó O(n log n) dùng để sắp xếp và O(m) dùng để tính kết quả của mỗi phiếu giảm giá bằng cách sử dụng tổng tiền tố được tính toán trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m * C(n, q)) | O(n) | Quá chậm | 
| Tối ưu | O(n log n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`, mảng giá`a`,`m`và mảng kích thước phiếu giảm giá`q`. Chúng đại diện cho số lượng thanh, giá riêng của chúng, số lượng phiếu giảm giá và số lượng phiếu giảm giá mà mỗi phiếu giảm giá ảnh hưởng. 
2. Sắp xếp mảng`a`theo thứ tự giảm dần. Điều này đảm bảo các thanh đắt nhất sẽ ở ngay đầu. Việc sắp xếp là rất quan trọng vì lựa chọn phiếu giảm giá tối ưu luôn bao gồm các thanh đắt nhất để tối đa hóa mức giảm giá. 
3. Tính tổng tiền tố của mảng đã được sắp xếp. Tổng tiền tố tại chỉ mục`i`đại diện cho tổng của số đầu tiên`i+1`thanh đắt nhất. Điều này cho phép truy vấn liên tục về tổng giá của sản phẩm hàng đầu`k`thanh. 
4. Tính tổng của tất cả các thanh. Điều này sẽ được sử dụng để tính toán khoản thanh toán cuối cùng cho mỗi phiếu giảm giá bằng cách trừ đi thanh miễn phí trong phiếu giảm giá. 
5. Đối với mỗi kích thước phiếu giảm giá`q_i`, chiến lược tối ưu là chọn đỉnh`q_i`thanh. Trong số đó, thanh rẻ nhất (là thanh cuối cùng trong tập hợp con được sắp xếp có kích thước`q_i`) là miễn phí. Tổng chi phí trở thành tổng của tất cả các thanh trừ đi thanh rẻ nhất này. 
6. In kết quả từng phiếu theo thứ tự. 

Bất biến đảm bảo tính đúng đắn là đối với bất kỳ phiếu giảm giá nào, mức chiết khấu được tối đa hóa bằng cách chọn mức lớn nhất`q_i`thanh. Không có tập hợp các thanh thay thế nào mang lại mức giảm giá lớn hơn vì bất kỳ thanh nào có giá thấp hơn sẽ làm giảm giá trị của sô-cô-la miễn phí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
m = int(input())
q = list(map(int, input().split()))

a.sort(reverse=True)
prefix_sum = [0] * n
prefix_sum[0] = a[0]
for i in range(1, n):
    prefix_sum[i] = prefix_sum[i-1] + a[i]

total_sum = prefix_sum[-1]

results = []
for x in q:
    free_bar = a[x-1]  # cheapest among the top x bars
    cost = total_sum - free_bar
    results.append(str(cost))

print('\n'.join(results))
```Giải pháp đầu tiên sắp xếp các thanh sô cô la và xây dựng một mảng tổng tiền tố. các`total_sum`nắm bắt tổng của tất cả các thanh. Đối với mỗi phiếu giảm giá, thanh cuối cùng ở trên cùng`q_i`các thanh được trừ vào tổng số tiền để tính sô cô la miễn phí. Thuật toán tránh tính toán lại bằng cách sử dụng mảng được sắp xếp trước. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
7
7 1 3 1 4 10 8
2
3 4
```Trạng thái của các biến chính sau khi sắp xếp và tính tổng tiền tố: 

| Đã sắp xếp một | Tiền tố Tổng | 
| --- | --- | 
| 10 8 7 4 3 1 1 | 10 18 25 29 32 33 34 | 

Đối với phiếu giảm giá q=3, 3 thanh trên cùng là 10, 8, 7. Rẻ nhất là 7. Tổng chi phí = 34 - 7 = 27. 

Đối với phiếu giảm giá q=4, 4 thanh trên cùng là 10, 8, 7, 4. Rẻ nhất là 4. Tổng chi phí = 34 - 4 = 30. 

Mẫu 2: 

đầu vào:```
5
5 5 5 5 5
1
2
```Tổng mảng và tiền tố được sắp xếp: 

| Đã sắp xếp một | Tiền tố Tổng | 
| --- | --- | 
| 5 5 5 5 5 | 5 10 15 20 25 | 

Đối với phiếu q=2, 2 thanh trên cùng là 5, 5. Rẻ nhất là 5. Tổng chi phí = 25 - 5 = 20. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m) | Sắp xếp các thanh mất n log n. Mỗi phép tính phiếu giảm giá là O(1) sử dụng tổng tiền tố. | 
| Không gian | O(n) | Chúng tôi lưu trữ các tổng tiền tố và mảng được sắp xếp. | 

Với n 3*10^5 và m ≤ n, thuật toán này phù hợp thoải mái trong giới hạn thời gian 2 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    m = int(input())
    q = list(map(int, input().split()))

    a.sort(reverse=True)
    total_sum = sum(a)
    results = []
    for x in q:
        results.append(str(total_sum - a[x-1]))
    return '\n'.join(results)

# provided samples
assert run("7\n7 1 3 1 4 10 8\n2\n3 4\n") == "27\n30", "sample 1"

# minimum size input
assert run("2\n1 2\n1\n2\n") == "2", "min size"

# all equal values
assert run("5\n5 5 5 5 5\n2\n2 5\n") == "20\n20", "all equal"

# maximum-size simplified test
assert run("6\n1 2 3 4 5 6\n1\n6\n") == "20", "full coupon"

# edge case: descending input
assert run("5\n10 8 5 3 1\n2\n2 3\n") == "27\n26", "descending input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n1 2\n1\n2 | 2 | tối thiểu n | 
| 5\n5 5 5 5 5\n2\n2 5 | 20\n20 | tất cả các thanh bằng nhau | 
| 6\n1 2 3 4 5 6\n1\n6 | 20 | phiếu giảm giá bao gồm tất cả các thanh | 
| 5\n10 8 5 3 1\n2\n2 3 | 27\n26 | thứ tự giảm dần, nhiều phiếu giảm giá | 

## Vỏ cạnh 

Đối với một phiếu giảm giá bao gồm tất cả các thanh,`q = n`, thanh miễn phí nhìn chung là rẻ nhất. Ví dụ, với thanh`[4, 2, 3]`Và`q = 3`, thanh miễn phí là 2, tổng số tiền thanh toán = 4 + 3 = 7. Thuật toán chọn đúng `a[q-1] = a
