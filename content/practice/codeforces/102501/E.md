---
title: "CF 102501E - Điểm ảnh"
description: "Chúng ta có một lưới nhị phân hình chữ nhật. Một ô có màu đen hoặc trắng và chúng ta cần chọn một tập hợp các ô có công tắc được nhấn. Nhấn một công tắc sẽ chuyển đổi ô đó và bốn ô lân cận trực giao của nó."
date: "2026-08-06T18:55:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102501
codeforces_index: "E"
codeforces_contest_name: "2019-2020 ICPC Southwestern European Regional Programming Contest (SWERC 2019-20)"
rating: 0
weight: 102501
solve_time_s: 60
verified: true
draft: false
---

[CF 102501E - Pixel](https://codeforces.com/problemset/problem/102501/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới nhị phân hình chữ nhật. Một ô có màu đen hoặc trắng và chúng ta cần chọn một tập hợp các ô có công tắc được nhấn. Nhấn một công tắc sẽ chuyển đổi ô đó và bốn ô lân cận trực giao của nó. Nhiệm vụ là xuất ra một bộ công tắc nhấn hợp lệ hoặc chứng minh rằng không tồn tại bộ đó. 

Đầu vào đưa ra trạng thái cuối cùng mong muốn của mỗi ô. Đầu ra là một lưới khác có cùng kích thước, trong đó`P`có nghĩa là công tắc tương ứng được nhấn và`A`có nghĩa là nó không được nhấn. 

Tổng số ô nhiều nhất là 100000. Việc loại bỏ Gaussian chung trên tất cả các ô sẽ yêu cầu một ma trận có 100000 biến, vượt xa những gì có thể xử lý được. Giải pháp cần khai thác cấu trúc hình chữ nhật thay vì coi mọi ô là một biến không liên quan. Quan sát quan trọng là cạnh nhỏ hơn của hình chữ nhật nhiều nhất là khoảng 316 khi diện tích bị giới hạn bởi 100000, do đó hệ thống tuyến tính trên kích thước nhỏ hơn là khả thi. 

Có một số trường hợp nhỏ mà giải pháp bất cẩn lại thất bại. Đối với một hàng, việc nhấn một ô chỉ ảnh hưởng đến các ô lân cận trong hàng đó, do đó, lý do dọc được sử dụng cho các lưới lớn hơn vẫn phải hoạt động. Ví dụ:```
1 2
B W
```Hai ô luôn thay đổi cùng nhau khi nhấn một trong hai công tắc, vì vậy câu trả lời là`IMPOSSIBLE`. 

Cái bẫy thứ hai là quên rằng hàng đầu tiên hoặc hàng cuối cùng có ít hàng xóm hơn. Ví dụ:```
2 1
B
B
```Nhấn công tắc trên cùng sẽ chuyển đổi cả hai ô, do đó nhấn vào ô trên cùng sẽ tạo ra kết quả mong muốn. Một giải pháp giả định mỗi ô có bốn ô lân cận sẽ xây dựng các phương trình sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xem mỗi ô là một biến và xây dựng hệ thống tuyến tính trên GF(2). Mỗi biến cho biết liệu một công tắc có được nhấn hay không. Mỗi phương trình mô tả ô cuối cùng nên có màu đen hay trắng. Điều này đúng vì việc chuyển đổi tương đương với phép cộng modulo hai. Tuy nhiên, ma trận sẽ chứa tới 100000 biến, khiến việc loại bỏ thông thường trở nên quá chậm. Trường hợp xấu nhất sẽ cần khoảng 10 thao tác 15 bit. 

Cấu trúc lưới mang lại một tuyến đường tốt hơn. Nếu chúng ta quyết định công tắc nào được nhấn trong một hàng thì tất cả các hàng tiếp theo đều có thể bị ép. Đây là kỹ thuật đuổi bắt cổ điển được sử dụng trong các bài toán Tắt đèn. Phần chưa biết duy nhất sẽ trở thành hàng đầu tiên. Vì chúng ta có thể hoán vị lưới nên số lượng cột luôn có thể được điều chỉnh theo kích thước nhỏ hơn. Hàng đầu tiên chứa tối đa 316 biến. 

Vấn đề còn lại là một hệ thống tuyến tính nhỏ. Chúng tôi mô phỏng cuộc rượt đuổi một lần với mỗi vectơ cơ sở có thể có của hàng đầu tiên để tìm hiểu xem mọi biến của hàng đầu tiên ảnh hưởng đến hàng cuối cùng như thế nào. Sau đó phép loại bỏ Gaussian sẽ tìm ra số lần nhấn hàng đầu tiên cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2 phút(K,L) ) | O(KL) | Quá chậm | 
| Tối ưu | O(KL⋅min(K,L)) | O(KL) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu số hàng lớn hơn số cột, hãy hoán vị lưới. Điều này giữ cho hàng đầu tiên nhỏ, đây là phần duy nhất sẽ được giải quyết bằng cách loại bỏ Gaussian. 
2. Biểu diễn mỗi hàng dưới dạng mặt nạ bit. Lưới mục tiêu được chuyển đổi thành ma trận nhị phân trong đó`1`có nghĩa là màu đen. 
3. Xác định chức năng đuổi theo. Với hàng máy ép đầu tiên, mỗi hàng tiếp theo được xác định vì hàng`i`phải được sửa bằng cách chọn hàng`i + 1`. Công thức là: 

x i+1 ​ =d i ​ ⊕T(x i )⊕x i−1 ​ 

trong đó T chuyển đổi một hàng cùng với các hàng xóm bên trái và bên phải của nó. 

1. Chạy cuộc rượt đuổi với hàng đầu tiên trống. Hiệu còn lại ở hàng cuối cùng trở thành phần không đổi của phương trình cuối cùng. 
2. Chạy đuổi theo một lần cho mỗi bit ở hàng đầu tiên. Sự khác biệt ở hàng cuối cùng tạo thành các cột của một hệ thống tuyến tính. 
3. Giải hệ phương trình tuyến tính bằng phép khử Gauss trên GF(2). Nếu xuất hiện mâu thuẫn thì không thể tạo ra được bức tranh. 
4. Sử dụng hàng đầu tiên đã giải quyết, chạy cuộc rượt đuổi lần cuối và xuất máy ép. Nếu lưới đã được chuyển đổi, hãy chuyển đổi câu trả lời. 

Bất biến đằng sau việc đuổi bắt là sau khi xử lý hàng`i`, tất cả các hàng phía trên đều đúng và sẽ không bao giờ thay đổi nữa. Mọi giải pháp khả thi đều phải có một số lần nhấn đầu tiên và cuộc rượt đuổi sẽ tạo ra chính xác giải pháp được xác định bởi hàng đầu tiên đó. Hệ thống tuyến tính cuối cùng sẽ kiểm tra hàng đầu tiên nào làm cho hàng cuối cùng đúng, do đó giải pháp tìm được luôn hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def toggle_row(x, m):
    return x ^ ((x << 1) & ((1 << m) - 1)) ^ (x >> 1)

def chase(first, target, n, m):
    press = [0] * n
    press[0] = first
    for i in range(n - 1):
        cur = target[i] ^ press[i - 1] if i else target[i]
        press[i + 1] = cur ^ toggle_row(press[i], m)
    return press

def solve_system(cols, rhs, n):
    rows = []
    for i in range(n):
        mask = 0
        for j in range(n):
            if (cols[j] >> i) & 1:
                mask |= 1 << j
        if (rhs >> i) & 1:
            mask |= 1 << n
        rows.append(mask)

    pivot = 0
    where = [-1] * n
    for col in range(n):
        found = -1
        for r in range(pivot, n):
            if (rows[r] >> col) & 1:
                found = r
                break
        if found == -1:
            continue
        rows[pivot], rows[found] = rows[found], rows[pivot]
        where[col] = pivot
        for r in range(n):
            if r != pivot and ((rows[r] >> col) & 1):
                rows[r] ^= rows[pivot]
        pivot += 1

    for r in range(n):
        if rows[r] == (1 << n):
            return None

    ans = 0
    for i in range(n):
        if where[i] != -1 and ((rows[where[i]] >> n) & 1):
            ans |= 1 << i
    return ans

def main():
    k, l = map(int, input().split())
    a = [[1 if x == 'B' else 0 for x in input().split()] for _ in range(k)]

    transposed = False
    if k < l:
        a = [list(x) for x in zip(*a)]
        k, l = l, k
        transposed = True

    target = []
    for row in a:
        mask = 0
        for j, x in enumerate(row):
            if x:
                mask |= 1 << j
        target.append(mask)

    base = chase(0, target, k, l)
    rhs = target[-1] ^ (base[-2] if k > 1 else 0) ^ toggle_row(base[-1], l)

    cols = []
    for i in range(l):
        cur = chase(1 << i, target, k, l)
        cols.append(toggle_row(cur[-1], l) ^ (cur[-2] if k > 1 else 0) ^ rhs)

    first = solve_system(cols, rhs, l)
    if first is None:
        print("IMPOSSIBLE")
        return

    ans = chase(first, target, k, l)
    out = [['P' if (ans[i] >> j) & 1 else 'A' for j in range(l)] for i in range(k)]

    if transposed:
        out = [list(x) for x in zip(*out)]

    print('\n'.join(' '.join(row) for row in out))

if __name__ == "__main__":
    main()
```Việc triển khai lưu trữ mỗi hàng dưới dạng mặt nạ bit số nguyên. Điều này làm cho hoạt động lân cận theo chiều ngang trở thành một vài thao tác bit thay vì lặp qua các ô. Hàm đuổi theo tuân theo cùng một bất biến được mô tả ở trên: khi một hàng được chuyển qua, nó sẽ được cố định vĩnh viễn. 

Việc loại bỏ Gaussian sử dụng số nguyên làm bitset. Bit cao nhất trong mỗi hàng sẽ lưu trữ phía bên phải, do đó các hàng XOR thực hiện việc loại bỏ GF(2). Bước chuyển vị rất cần thiết vì nó giữ cho số lượng biến trong hệ thống cuối cùng ở mức nhỏ. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên: 

| Bước | Lựa chọn hàng đầu tiên | Yêu cầu hàng cuối cùng | Kết quả | 
| --- | --- | --- | --- | 
| Ban đầu | không có máy ép nào được chọn | Hai ô phải trở nên khác nhau | Không thể | 
| Loại bỏ | không tồn tại sự phân công nhất quán | Mâu thuẫn được tìm thấy |`IMPOSSIBLE`| 

Sự mâu thuẫn xuất hiện do cả hai ô luôn được chuyển đổi cùng nhau nên trạng thái mục tiêu không thể tách rời. 

Đối với một ví dụ hợp lệ nhỏ:```
2 1
B
B
```| Bước | Máy ép hàng hiện tại | Nhấn hàng tiếp theo | Tiểu bang | 
| --- | --- | --- | --- | 
| Bắt đầu |`0`|`1`| Hàng trên cùng đã được cố định | 
| Kết thúc |`1`| không | Cả hai ô đều trở thành màu đen | 

Cuộc rượt đuổi chọn đúng công tắc duy nhất có thể tạo ra cặp được yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(KL⋅min(K,L)) | Mỗi bit hàng đầu tiên có thể yêu cầu một lần đuổi theo và cuộc đuổi bắt chạm vào mọi ô | 
| Không gian | O(KL) | Lưới và câu trả lời được lưu trữ | 

Vì cạnh nhỏ hơn của lưới nhiều nhất là 316 nên hệ số nhân vẫn nhỏ ngay cả khi số lượng ô đạt tới 100000. 

## Trường hợp thử nghiệm```python
# The submitted program is read from stdin, so these examples are intended
# to be run manually with the solution above.

cases = [
    (
        "1 2\nB W\n",
        "IMPOSSIBLE"
    ),
    (
        "1 1\nB\n",
        "P"
    ),
    (
        "2 1\nB\nB\n",
        "P\nA"
    ),
    (
        "1 3\nB B B\n",
        "A P A"
    )
]

for inp, expected in cases:
    print("Input:")
    print(inp)
    print("Expected contains:")
    print(expected)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 / B W`|`IMPOSSIBLE`| Trường hợp một hàng không thỏa mãn | 
|`1 1 / B`| Bất kỳ lần nhấn nào | Lưới nhỏ nhất có thể | 
|`2 1 / B B`| Một lần nhấn dọc | Xử lý ranh giới | 
|`1 3 / B B B`| Báo chí giữa | Logic lân cận theo chiều ngang | 

## Vỏ cạnh 

Hàng hai ô không giải được sẽ được xử lý vì hệ thống tuyến tính được tạo không có phép gán hàng đầu tiên hợp lệ. Giai đoạn loại bỏ phát hiện sự mâu thuẫn thay vì tạo ra mẫu in không hợp lệ. 

Lưới một hàng và một cột được xử lý vì vẫn áp dụng phép lặp tương tự. Những người hàng xóm bị thiếu chỉ đơn giản là đóng góp số 0 vào các hoạt động bit. 

Chuyển vị tránh được vấn đề hiệu suất tiềm ẩn. Lưới có kích thước`1 x 100000`nếu không sẽ tạo ra một hệ thống tuyến tính với 100000 biến. Sau khi chuyển vị, nó trở thành`100000 x 1`, và hệ thống chỉ có một biến.
