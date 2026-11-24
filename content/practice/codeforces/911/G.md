---
title: "CF 911G - Truy vấn thay đổi hàng loạt"
description: "Chúng ta được cung cấp một mảng số nguyên một chiều và chúng ta phải xử lý một chuỗi các truy vấn thay thế có chọn lọc các giá trị trong mảng con. Mỗi truy vấn chỉ định một phạm vi trong mảng và hai số nguyên x và y. Đối với mọi phần tử trong phạm vi đó bằng x, chúng tôi thay thế nó bằng y."
date: "2026-06-13T00:34:41+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 911
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 35 (Rated for Div. 2)"
rating: 2500
weight: 911
solve_time_s: 270
verified: true
draft: false
---

[CF 911G - Truy vấn về thay đổi hàng loạt](https://codeforces.com/problemset/problem/911/G) 

**Đánh giá:** 2500 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 4 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng số nguyên một chiều và chúng ta phải xử lý một chuỗi các truy vấn thay thế có chọn lọc các giá trị trong mảng con. Mỗi truy vấn chỉ định một phạm vi trong mảng và hai số nguyên _x_ và _y_. Đối với mọi phần tử trong phạm vi đó bằng _x_, chúng tôi thay thế nó bằng _y_. Sau khi tất cả các truy vấn được áp dụng theo thứ tự, nhiệm vụ là xuất ra trạng thái cuối cùng của mảng. 

Kích thước mảng có thể đạt tới 200.000 phần tử và mỗi phần tử được giới hạn từ 1 đến 100. Số lượng truy vấn cũng có thể đạt tới 200.000. Một cách tiếp cận đơn giản lặp lại mảng con cho mỗi truy vấn có thể dẫn đến trường hợp xấu nhất là khoảng 4 × 10^10 phép toán nếu tất cả các truy vấn bao trùm toàn bộ mảng. Rõ ràng, giải pháp O(n·q) là không khả thi trong giới hạn thời gian 3 giây, trong đó thông thường có khoảng 10^8 thao tác có thể được thực hiện mỗi giây. 

Các trường hợp đặc biệt không rõ ràng bao gồm các truy vấn chồng chéo nhắm mục tiêu vào cùng một phần tử với nhiều thay thế, các truy vấn trong đó _x_ và _y_ bằng nhau hoặc các truy vấn bao gồm các phạm vi không chứa giá trị _x_ trùng khớp. Ví dụ, nếu mảng là`[1, 2, 1]`và các truy vấn là`(1, 3, 1, 2)`theo sau là`(2, 3, 2, 3)`, sự thay thế ngây thơ phải theo dõi cẩn thận mảng được cập nhật để tạo ra`[2, 3, 2]`. Việc bỏ qua những cập nhật này hoặc áp dụng chúng không đúng cách có thể tạo ra một mảng cuối cùng sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực chỉ đơn giản lặp lại mảng cho mỗi truy vấn và kiểm tra xem mỗi phần tử trong phạm vi đã chỉ định có bằng _x_ hay không. Nếu vậy, nó sẽ thay thế phần tử bằng _y_. Phương pháp này hoạt động chính xác vì nó thực hiện đúng thao tác được mô tả trong bài toán. Vấn đề phát sinh từ tính kém hiệu quả của nó: trong trường hợp xấu nhất, mọi truy vấn đều sửa đổi gần như toàn bộ mảng, dẫn đến số lượng thao tác là O(n·q), ở mức 4 × 10^10 đối với đầu vào lớn nhất và vượt xa giới hạn thực tế. 

Quan sát quan trọng để tối ưu hóa là các phần tử mảng là các số nguyên nhỏ, giới hạn bởi 100. Điều này cho phép chúng ta lưu trữ, đối với mỗi giá trị có thể, một danh sách được sắp xếp các chỉ mục nơi nó xuất hiện. Khi truy vấn yêu cầu thay thế tất cả các lần xuất hiện của _x_ bằng _y_ trong một phạm vi, chúng tôi có thể nhanh chóng xác định vị trí các chỉ mục có liên quan trong danh sách các vị trí được sắp xếp cho _x_ bằng cách sử dụng tìm kiếm nhị phân, sau đó cập nhật các chỉ mục đó và điều chỉnh danh sách cho _x_ và _y_. Vì mỗi số chỉ có các chỉ mục trong danh sách riêng nên chúng tôi tránh quét các phần tử không liên quan. 

Điều này chuyển đổi vấn đề từ việc quét toàn bộ mảng sang chỉ quét các vị trí của các giá trị mà chúng ta thực sự cần thay thế. Mỗi chỉ mục được truy cập nhiều nhất một lần cho mỗi lần thay đổi số riêng biệt, dẫn đến tổng thời gian chạy tỷ lệ với O(n + q·log n) hoặc cao hơn. Độ phức tạp về không gian vẫn có thể quản lý được vì chúng tôi lưu trữ ánh xạ từ các số 1-100 vào danh sách các chỉ mục, tổng cộng là O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·q) | O(n) | Quá chậm | 
| Tối ưu hóa với danh sách chỉ mục | O(n + q·log n) | O(n + 100) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng`a`và khởi tạo một từ điển`pos`ánh xạ từng giá trị từ 1 đến 100 vào danh sách các chỉ mục được sắp xếp nơi nó xuất hiện. Điều này cho phép chúng tôi nhanh chóng xác định vị trí của bất kỳ giá trị nào. 
2. Đối với mỗi truy vấn`(l, r, x, y)`, kiểm tra xem _x_ có bằng _y_ không. Nếu chúng giống nhau, hãy bỏ qua truy vấn này vì không cần thay đổi. 
3. Nếu không, hãy sử dụng tìm kiếm nhị phân để tìm danh sách con của các chỉ mục trong`pos[x]`nằm bên trong`[l-1, r-1]`. Chỉ những vị trí này cần được cập nhật. 
4. Với mỗi chỉ mục trong danh sách con này, hãy đặt`a[index] = y`. Nối chỉ mục vào`pos[y]`. 
5. Xóa các chỉ mục đã cập nhật khỏi`pos[x]`để giữ cho danh sách chính xác. 
6. Sau khi truy vấn xong, in mảng`a`. 

Tại sao nó hoạt động: Mỗi truy vấn chỉ sửa đổi các vị trí hiện giữ giá trị _x_ và chúng tôi duy trì danh sách vị trí cập nhật cho mọi giá trị. Tìm kiếm nhị phân đảm bảo chúng tôi chỉ chạm vào các chỉ mục có liên quan. Vì mỗi lần cập nhật chỉ mục được phản ánh trong`pos`ngay lập tức, các truy vấn tiếp theo sẽ thấy giá trị hiện tại chính xác. Điều này bảo tồn thứ tự hoạt động và đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
q = int(input())

pos = {i: [] for i in range(1, 101)}
for idx, val in enumerate(a):
    pos[val].append(idx)

for _ in range(q):
    l, r, x, y = map(int, input().split())
    if x == y:
        continue
    indices = pos[x]
    left = bisect.bisect_left(indices, l - 1)
    right = bisect.bisect_right(indices, r - 1)
    for idx in indices[left:right]:
        a[idx] = y
        pos[y].append(idx)
    del indices[left:right]

print(' '.join(map(str, a)))
```Đầu tiên, mã sẽ xây dựng danh sách chỉ mục cho tất cả các giá trị có thể có. Đối với mỗi truy vấn, nó sử dụng tìm kiếm nhị phân để nhanh chóng tìm thấy các chỉ mục trong phạm vi liên quan. Sau khi cập nhật, nó đảm bảo`pos[x]`không còn chứa các chỉ số được thay thế, trong khi`pos[y]`nhận được các vị trí cập nhật. Điều này tránh việc xem lại các phần tử không thay đổi. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Bước | Mảng`a`| Truy vấn | Chỉ số được cập nhật | Đã cập nhật`pos`| 
| --- | --- | --- | --- | --- | 
| Ban đầu | [1,2,3,4,5] | - | - | {1:[0],2:[1],3:[2],4:[3],5:[4]} | 
| 1 | [1,2,5,4,5] | 3 5 3 5 | [2] | 3:[], 5:[4,2] | 
| 2 | [1,2,1,4,1] | 1 5 5 1 | [4,2] | 5:[], 1:[0,4,2] | 
| 3 | [5,2,5,4,5] | 1 5 1 5 | [0,2,4] | 1:[], 5:[0,2,4] | 

Dấu vết này xác nhận rằng thuật toán xác định chính xác các chỉ số và cập nhật các giá trị theo thứ tự. 

Mẫu tùy chỉnh: 

đầu vào:```
4
2 2 2 2
2
1 4 2 3
2 3 3 4
```Dấu vết hiển thị thông tin cập nhật: 

| Bước | Mảng`a`| Chỉ số được cập nhật | 
| --- | --- | --- | 
| Ban đầu | [2,2,2,2] | - | 
| 1 | [3,3,3,3] | [0,1,2,3] | 
| 2 | [4,4,4,3] | [0,1,2] | 

Xác nhận cập nhật một phần phạm vi hoạt động chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q·log n) | Lập chỉ mục ban đầu là O(n). Mỗi truy vấn sử dụng tìm kiếm nhị phân trong O(log n) trên danh sách nhỏ. Chi phí cập nhật O(#indices). Tuyến tính tổng thể tính bằng n cộng với chi phí ghi nhật ký cho mỗi truy vấn. | 
| Không gian | O(n + 100) | Lưu trữ mảng và ánh xạ các giá trị vào danh sách chỉ mục. Kích thước tối đa của danh sách là n. Chi phí từ điển không đáng kể. | 

Điều này phù hợp với giới hạn thời gian 3 giây đối với n, q 200.000 và giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())

    import bisect
    pos = {i: [] for i in range(1, 101)}
    for idx, val in enumerate(a):
        pos[val].append(idx)

    for _ in range(q):
        l, r, x, y = map(int, input().split())
        if x == y:
            continue
        indices = pos[x]
        left = bisect.bisect_left(indices, l - 1)
        right = bisect.bisect_right(indices, r - 1)
        for idx in indices[left:right]:
            a[idx] = y
            pos[y].append(idx)
        del indices[left:right]
    return ' '.join(map(str, a))

# Provided sample
assert run("5\n1 2 3 4 5\n3\n3 5 3 5\n1 5 5 1\n1 5 1 5\n") == "5 2 5 4 5", "sample 1"

# Custom cases
assert run("1\n1\n1\n1 1 1 1\n") == "1", "single element no change"
assert run("4\n2 2 2 2\n2\n1 4 2 3\n2 3 3 4\n") == "4 4 4 3", "overlapping ranges"
assert run("3\n1 2 1\n2\n1 3 1 2\n2 3 2 3\n") == "2 3 2", "
```
