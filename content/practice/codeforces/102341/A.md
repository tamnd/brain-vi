---
title: "CF 102341A - Alakazam"
description: "Chúng ta có một mảng số lượng thìa, trong đó vị trí ban đầu của i chứa a[i]. Thao tác xáo trộn l r lấy mọi giá trị hiện nằm trong khoảng [l, r] và hoán vị ngẫu nhiên các giá trị đó vào cùng một vị trí."
date: "2026-08-13T03:02:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102341
codeforces_index: "A"
codeforces_contest_name: "Radewoosh+mnbvmar Contest (supported by AIM Tech)"
rating: 0
weight: 102341
solve_time_s: 643
verified: true
draft: false
---

[CF 102341A - Alakazam](https://codeforces.com/problemset/problem/102341/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 10 phút 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một loạt số lượng thìa, vị trí ở đâu`i`ban đầu chứa`a[i]`. MỘT`shuffle l r`hoạt động lấy mọi giá trị hiện nằm trong khoảng`[l, r]`và hoán vị ngẫu nhiên các giá trị đó vào cùng một vị trí. MỘT`get i`hoạt động yêu cầu giá trị mong đợi tại vị trí`i`sau tất cả những lần xáo trộn trước đó. 

Khó khăn chính là hoán vị thực tế là ngẫu nhiên, vì vậy chúng ta không thể duy trì một mảng cụ thể. Chúng ta cần duy trì giá trị mong đợi ở mọi vị trí. Việc xáo trộn không bảo toàn giá trị mong đợi của một vị trí riêng lẻ, nhưng nó bảo toàn tổng số tiền của toàn bộ khoảng thời gian được xáo trộn. 

Với`n, q <= 250000`, một cách tiếp cận quét toàn bộ khoảng thời gian cho mọi hoạt động có thể yêu cầu khoảng`nq`, đó là về`6.25 * 10^10`hoạt động phần tử trong trường hợp xấu nhất. Điều đó vượt xa những gì giới hạn lập trình cạnh tranh 2 giây có thể hỗ trợ. Chúng ta cần mọi thao tác chỉ chạm vào nhiều nút cấu trúc dữ liệu theo logarit. 

Có một số trường hợp ranh giới trong đó việc triển khai trực tiếp có thể âm thầm gặp trục trặc. Ví dụ: nếu khoảng xáo trộn chỉ chứa một vị trí```
1 2
7
shuffle 1 1
get 1
```câu trả lời là`7.000000000000`. Việc triển khai bất cẩn coi mọi xáo trộn là thay đổi giá trị có thể gây ra phép tính làm tròn hoặc lấy trung bình không cần thiết, mặc dù về mặt toán học, khoảng trung bình vẫn chính xác`7`. 

Một trường hợp rõ ràng hơn là sự xáo trộn toàn bộ mảng:```
3 2
1 2 6
shuffle 1 3
get 2
```Câu trả lời là`3.000000000000`, bởi vì mọi vị trí đều có khả năng nhận được một trong ba giá trị như nhau. Chỉ nhìn vào giá trị ban đầu tại vị trí`2`sẽ sản xuất không chính xác`2`. 

Một lỗi ranh giới phổ biến khác là quên rằng các điểm cuối đã bao gồm:```
3 2
1 2 9
shuffle 1 2
get 2
```Giá trị mong đợi là`1.500000000000`. Chức vụ`3`không bị ảnh hưởng, trong khi vị trí`1`Và`2`cả hai đều trở thành trung bình của`1`Và`2`. Việc triển khai vô tình sử dụng`[l, r)`sẽ rời khỏi vị trí`2`không thay đổi và trở lại`2`. 

Việc xáo trộn chồng chéo lặp đi lặp lại là một trường hợp khác khi chỉ lưu trữ mảng ban đầu là không đủ:```
3 4
1 2 3
shuffle 1 2
shuffle 2 3
get 1
get 3
```Các câu trả lời là`1.500000000000`Và`2.250000000000`. Lần xáo trộn thứ hai phải sử dụng các giá trị dự kiến ​​được tạo ra bởi lần xáo trộn đầu tiên, không phải các giá trị ban đầu`2`Và`3`. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Duy trì giá trị kỳ vọng hiện tại của mọi vị trí. Vì`shuffle l r`, tính tổng các giá trị kỳ vọng từ`l`bởi vì`r`, chia cho`r-l+1`, và viết giá trị trung bình đó vào mọi vị trí của khoảng. Vì`get i`, chỉ cần in giá trị hiện tại tại vị trí`i`. 

Điều này đúng vì sau một hoán vị ngẫu nhiên thống nhất của các giá trị trong`[l,r]`, mọi vị trí trong khoảng đó đều có cùng xác suất nhận được mọi giá trị. Nếu giá trị kỳ vọng hiện tại là`E_l, E_{l+1}, ..., E_r`, tính tuyến tính của kỳ vọng mang lại kỳ vọng mới ở mọi vị trí như 

[ 
\frac{E_l+E_{l+1}+\cdots+E_r}{r-l+1}. 
] 

Vấn đề là chi phí. Một shuffle có thể chứa`250000`vị trí, và trong trường hợp xấu nhất tất cả`250000`hoạt động có thể được xáo trộn của toàn bộ mảng. Cập nhật mọi vị trí thì mất khoảng 

[ 
250000 \times 250000 = 62.500.000.000 
] 

cập nhật vị trí, ngay cả trước khi tính toán số tiền. 

Quan sát giúp loại bỏ nút cổ chai này là sau khi xáo trộn, toàn bộ khoảng thời gian có một giá trị kỳ vọng chung. Chúng ta không cần phải nhớ các giá trị mong đợi riêng lẻ trong khoảng đó sau khi việc xáo trộn đã xảy ra. Chúng ta chỉ cần tổng của khoảng trước khi xáo trộn, sau đó chúng ta có thể đánh dấu toàn bộ khoảng đó một cách lười biếng là có một giá trị không đổi. 

Đây chính xác là phép gán phạm vi được kết hợp với truy vấn tổng phạm vi. Cây phân đoạn lười biếng có thể biểu diễn cả hai thao tác trong`O(log n)`. Mỗi nút cây lưu trữ tổng khoảng của nó và khi cần thiết, một phép gán lười biếng cho biết rằng mọi vị trí trong khoảng hiện tại đều có cùng giá trị mong đợi. 

Phương pháp brute-force hoạt động hiệu quả vì nó thực hiện rõ ràng cùng một phép gán phạm vi mà toán học yêu cầu, nhưng nó hiện thực hóa mọi phép gán riêng lẻ. Cây phân đoạn giữ nhiệm vụ đó được nén bên trong các nút cây. Toàn bộ khoảng có thể được biểu diễn bằng một số thay vì viết lại tất cả các vị trí của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(nq)`|`O(n)`| Quá chậm | 
| Tối ưu |`O((n+q) log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn trên mảng ban đầu. Mỗi nút lưu trữ tổng các giá trị dự kiến ​​trong khoảng của nó. Ban đầu, đây chỉ là số thìa ban đầu. 
2. Thêm giá trị gán lười cho mỗi nút cây phân đoạn. Nếu một nút có giá trị lười biếng`x`, điều đó có nghĩa là mọi vị trí được đại diện bởi nút đó hiện có giá trị mong đợi`x`. Chúng tôi sử dụng`None`có nghĩa là không có sự phân công đồng phục nào đang chờ xử lý. 
3. Đối với`shuffle l r`, đầu tiên truy vấn tổng của khoảng`[l,r]`. Hãy để số tiền này là`S`, và gọi độ dài khoảng là`len = r-l+1`. Giá trị mong đợi sau khi hoán vị ngẫu nhiên là`S / len`. 
4. Gán giá trị trung bình này cho toàn bộ khoảng thời gian`[l,r]`sử dụng sự lan truyền lười biếng. Đối với một nút cây được bao phủ đầy đủ đại diện`k`vị trí, tổng của nó trở thành`average * k`, và bài tập lười biếng của nó trở thành`average`. 
5. Đối với`get i`, đi xuống cây phân đoạn tới vị trí lá đại diện`i`. Bất cứ khi nào một nút có một bài tập lười biếng đang chờ xử lý, hãy truyền nó tới các nút con của nó trước khi đi xuống. Lá sau đó chứa giá trị mong đợi chính xác được yêu cầu. 
6. In câu trả lời có đủ chữ số thập phân. In mười hai chữ số sau dấu thập phân là quá đủ cho yêu cầu`1e-9`sai số tuyệt đối hoặc tương đối. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi nút cây phân đoạn đều lưu trữ tổng chính xác các giá trị dự kiến của tất cả các vị trí trong khoảng của nó, trong khi phép gán lười ghi lại rằng mọi vị trí trong khoảng đó đều có cùng giá trị kỳ vọng. 

Hãy xem xét một sự xáo trộn của`[l,r]`. Trước khi xáo trộn, hãy để giá trị mong đợi ở vị trí`j`là`E_j`. Mọi hoán vị đều mang lại cho mọi vị trí ban đầu trong khoảng đó cùng một xác suất xuất hiện ở bất kỳ vị trí đích nào. Do đó, giá trị kỳ vọng mới tại mỗi vị trí mục tiêu là 

[ 
\frac{\sum_{j=l}^{r} E_j}{r-l+1}. 
] 

Cây phân đoạn tính toán chính xác tổng này, sau đó gán chính xác mức trung bình này cho toàn bộ khoảng. Do đó, bất biến vẫn đúng sau mỗi lần xáo trộn. Một truy vấn điểm tuân theo các phép gán ảnh hưởng đến đường đi của nó và trả về giá trị mong đợi được biểu thị bởi lá, vì vậy mọi`get`câu trả lời là đúng 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(float, input().split()))

    size = 4 * n + 5
    tree = [0.0] * size
    lazy = [None] * size

    def build(node, left, right):
        if left == right:
            tree[node] = a[left]
            return

        mid = (left + right) >> 1
        lc = node << 1
        rc = lc | 1

        build(lc, left, mid)
        build(rc, mid + 1, right)
        tree[node] = tree[lc] + tree[rc]

    def apply(node, left, right, value):
        tree[node] = value * (right - left + 1)
        lazy[node] = value

    def push(node, left, right):
        value = lazy[node]
        if value is None or left == right:
            return

        mid = (left + right) >> 1
        lc = node << 1
        rc = lc | 1

        apply(lc, left, mid, value)
        apply(rc, mid + 1, right, value)
        lazy[node] = None

    def range_sum(node, left, right, ql, qr):
        if ql <= left and right <= qr:
            return tree[node]

        push(node, left, right)

        mid = (left + right) >> 1
        result = 0.0

        if ql <= mid:
            result += range_sum(node << 1, left, mid, ql, qr)

        if qr > mid:
            result += range_sum(node << 1 | 1, mid + 1, right, ql, qr)

        return result

    def range_assign(node, left, right, ql, qr, value):
        if ql <= left and right <= qr:
            apply(node, left, right, value)
            return

        push(node, left, right)

        mid = (left + right) >> 1

        if ql <= mid:
            range_assign(node << 1, left, mid, ql, qr, value)

        if qr > mid:
            range_assign(node << 1 | 1, mid + 1, right, ql, qr, value)

        tree[node] = tree[node << 1] + tree[node << 1 | 1]

    def point_query(node, left, right, pos):
        if left == right:
            return tree[node]

        push(node, left, right)

        mid = (left + right) >> 1

        if pos <= mid:
            return point_query(node << 1, left, mid, pos)

        return point_query(node << 1 | 1, mid + 1, right, pos)

    build(1, 0, n - 1)

    output = []

    for _ in range(q):
        parts = input().split()

        if parts[0] == "shuffle":
            l = int(parts[1]) - 1
            r = int(parts[2]) - 1

            total = range_sum(1, 0, n - 1, l, r)
            average = total / (r - l + 1)

            range_assign(1, 0, n - 1, l, r, average)

        else:
            pos = int(parts[1]) - 1
            answer = point_query(1, 0, n - 1, pos)
            output.append(f"{answer:.12f}")

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```các`tree`mảng chứa tổng khoảng. Đối với một chiếc lá, tổng đơn giản là giá trị mong đợi ở vị trí đó. Đối với một nút bên trong, tổng này là tổng của hai nút con của nó. 

các`lazy`mảng đại diện cho một phạm vi phân công đang chờ xử lý. Nếu như`lazy[node]`là`x`, mọi vị trí trong khoảng của nút đó đều có giá trị mong đợi`x`. Do đó tổng của nó là`x * interval_length`. Một giá trị gán có thể đại diện cho toàn bộ cây con, việc nén này giúp giải pháp nhanh chóng. 

các`range_sum`chức năng chỉ được sử dụng để lấy tổng trước khi xáo trộn. Về mặt khái niệm, nó không sửa đổi các giá trị, mặc dù nó có thể đẩy một phép gán lười biếng xuống dưới để quá trình duyệt đệ quy nhìn thấy tổng con chính xác. 

các`range_assign`chức năng thực hiện hiệu ứng thực tế của một sự xáo trộn. Trên một nút được che phủ hoàn toàn, nó hoàn toàn không ghé thăm nút con. Nó thay đổi tổng của nút và ghi lại bài tập một cách lười biếng. Chồng chéo một phần yêu cầu đẩy nhiệm vụ của cha mẹ lên trước, bởi vì con cái phải nhận được giá trị hiện tại của cha mẹ trước khi một trong số chúng được sửa đổi thêm. 

các`point_query`chức năng chỉ đi theo một đường dẫn từ gốc đến lá. Nó đẩy các bài tập lười biếng trên đường dẫn đó, do đó, chiếc lá luôn biểu thị giá trị mong đợi mới nhất cho vị trí được yêu cầu. 

Tất cả các chỉ mục đầu vào được chuyển đổi từ lập chỉ mục dựa trên một sang lập chỉ mục dựa trên 0 chính xác một lần khi truy vấn được đọc. Độ dài khoảng vẫn được tính là`r - l + 1`, vì cả hai điểm cuối đều được bao gồm. 

Số nguyên Python sẽ không gặp vấn đề tràn đối với tổng, nhưng cây lưu trữ các kỳ vọng về dấu phẩy động vì cần phải chia sau mỗi lần xáo trộn. Các giá trị được giới hạn bởi phạm vi giá trị ban đầu, vì mức trung bình không bao giờ rời khỏi mức tối thiểu và tối đa của các giá trị được tính trung bình. Việc in mười hai chữ số sau dấu thập phân sẽ mang lại độ chính xác đủ cho dung sai yêu cầu. 

## Ví dụ đã hoạt động 

Mẫu được cung cấp bắt đầu bằng`[1, 2, 3]`. 

| Hoạt động | Khoảng thời gian | Tính tổng trước khi xáo trộn | Trung bình được giao | Giá trị mong đợi sau vận hành | 
| --- | --- | --- | --- | --- | 
|`get 1`|`1`|`1`|`1`|`[1, 2, 3]`| 
|`get 3`|`3`|`3`|`3`|`[1, 2, 3]`| 
|`shuffle 1 2`|`[1,2]`|`3`|`1.5`|`[1.5, 1.5, 3]`| 
|`shuffle 2 3`|`[2,3]`|`4.5`|`2.25`|`[1.5, 2.25, 2.25]`| 
|`get 1`|`1`|`1.5`|`1.5`|`[1.5, 2.25, 2.25]`| 
|`get 3`|`3`|`2.25`|`2.25`|`[1.5, 2.25, 2.25]`| 

Lần xáo trộn đầu tiên thay thế kỳ vọng ở các vị trí`1`Và`2`qua`(1+2)/2 = 1.5`. Lần xáo trộn thứ hai phải sử dụng`1.5`như kỳ vọng ở vị trí`2`, vậy trung bình của nó là`(1.5+3)/2 = 2.25`. Điều này chứng tỏ tại sao các bản cập nhật phải hoạt động trên các giá trị mong đợi hiện tại thay vì mảng ban đầu. 

Hãy xem xét một ví dụ thứ hai:```
4 6
10 20 30 40
shuffle 1 4
get 1
get 4
shuffle 2 3
get 2
get 3
```| Hoạt động | Khoảng thời gian | Tính tổng trước khi xáo trộn | Trung bình được giao | Giá trị mong đợi sau vận hành | 
| --- | --- | --- | --- | --- | 
|`shuffle 1 4`|`[1,4]`|`100`|`25`|`[25,25,25,25]`| 
|`get 1`|`1`|`25`|`25`|`[25,25,25,25]`| 
|`get 4`|`4`|`25`|`25`|`[25,25,25,25]`| 
|`shuffle 2 3`|`[2,3]`|`50`|`25`|`[25,25,25,25]`| 
|`get 2`|`2`|`25`|`25`|`[25,25,25,25]`| 
|`get 3`|`3`|`25`|`25`|`[25,25,25,25]`| 

Lần xáo trộn đầu tiên chỉ định cùng một kỳ vọng cho toàn bộ mảng. Lần xáo trộn thứ hai không có hiệu ứng rõ ràng vì khoảng thời gian đã có giá trị mong đợi thống nhất. Đây là trường hợp các bài tập lười biếng lặp đi lặp lại nhiều lần trên các phần lớn của cây. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O((n+q) log n)`| Chi phí xây dựng cây`O(n)`và mỗi lần xáo trộn thực hiện một tổng phạm vi và một phép gán phạm vi, mỗi phạm vi`O(log n)`. Mọi`get`là một truy vấn điểm trong`O(log n)`. | 
| Không gian |`O(n)`| Cây phân đoạn lưu trữ một lượng thông tin không đổi cho mỗi nút cây, do đó kích thước của nó là tuyến tính theo`n`. | 

Với`n`Và`q`nhiều nhất là cả hai`250000`, thuật toán chỉ thực hiện logarit nhiều phép toán cây phân đoạn cho mỗi truy vấn. Chiều cao của cây khoảng 18, do đó tổng số nút đã truy cập vẫn có thể quản lý được, không giống như`O(nq)`mô phỏng vũ lực. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n, q = map(int, input().split())
    a = list(map(float, input().split()))

    size = 4 * n + 5
    tree = [0.0] * size
    lazy = [None] * size

    def build(node, left, right):
        if left == right:
            tree[node] = a[left]
            return

        mid = (left + right) >> 1
        lc = node << 1
        rc = lc | 1
        build(lc, left, mid)
        build(rc, mid + 1, right)
        tree[node] = tree[lc] + tree[rc]

    def apply(node, left, right, value):
        tree[node] = value * (right - left + 1)
        lazy[node] = value

    def push(node, left, right):
        value = lazy[node]
        if value is None or left == right:
            return

        mid = (left + right) >> 1
        lc = node << 1
        rc = lc | 1

        apply(lc, left, mid, value)
        apply(rc, mid + 1, right, value)
        lazy[node] = None

    def range_sum(node, left, right, ql, qr):
        if ql <= left and right <= qr:
            return tree[node]

        push(node, left, right)
        mid = (left + right) >> 1
        result = 0.0

        if ql <= mid:
            result += range_sum(node << 1, left, mid, ql, qr)

        if qr > mid:
            result += range_sum(node << 1 | 1, mid + 1, right, ql, qr)

        return result

    def range_assign(node, left, right, ql, qr, value):
        if ql <= left and right <= qr:
            apply(node, left, right, value)
            return

        push(node, left, right)
        mid = (left + right) >> 1

        if ql <= mid:
            range_assign(node << 1, left, mid, ql, qr, value)

        if qr > mid:
            range_assign(node << 1 | 1, mid + 1, right, ql, qr, value)

        tree[node] = tree[node << 1] + tree[node << 1 | 1]

    def point_query(node, left, right, pos):
        if left == right:
            return tree[node]

        push(node, left, right)
        mid = (left + right) >> 1

        if pos <= mid:
            return point_query(node << 1, left, mid, pos)

        return point_query(node << 1 | 1, mid + 1, right, pos)

    build(1, 0, n - 1)

    output = []

    for _ in range(q):
        parts = input().split()

        if parts[0] == "shuffle":
            l = int(parts[1]) - 1
            r = int(parts[2]) - 1
            total = range_sum(1, 0, n - 1, l, r)
            average = total / (r - l + 1)
            range_assign(1, 0, n - 1, l, r, average)
        else:
            pos = int(parts[1]) - 1
            output.append(f"{point_query(1, 0, n - 1, pos):.12f}")

    sys.stdout.write("\n".join(output))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def close_enough(actual: str, expected: str) -> bool:
    a = [float(x) for x in actual.split()]
    b = [float(x) for x in expected.split()]

    if len(a) != len(b):
        return False

    return all(abs(x - y) <= 1e-9 * max(1.0, abs(y)) for x, y in zip(a, b))

sample1 = """\
3 6
1 2 3
get 1
get 3
shuffle 1 2
shuffle 2 3
get 1
get 3
"""

assert close_enough(
    run(sample1),
    """\
1.000000000000
3.000000000000
1.500000000000
2.250000000000
"""
), "provided sample"

minimum = """\
1 3
7
get 1
shuffle 1 1
get 1
"""

assert close_enough(
    run(minimum),
    """\
7.000000000000
7.000000000000
"""
), "minimum size"

all_equal = """\
5 7
8 8 8 8 8
shuffle 1 5
shuffle 2 4
get 1
get 2
get 5
"""

assert close_enough(
    run(all_equal),
    """\
8.000000000000
8.000000000000
8.000000000000
"""
), "all equal values"

boundaries = """\
4 7
1 2 9 10
shuffle 1 2
get 1
get 2
shuffle 3 4
get 3
get 4
get 2
"""

assert close_enough(
    run(boundaries),
    """\
1.500000000000
1.500000000000
9.500000000000
9.500000000000
1.500000000000
"""
), "boundary intervals"

maximum_n = 250000
maximum_case = (
    f"{maximum_n} 3\n"
    + " ".join(["1000000"] * maximum_n)
    + "\n"
    + "shuffle 1 250000\n"
    + "get 125000\n"
    + "get 250000\n"
)

assert close_enough(
    run(maximum_case),
    """\
1000000.000000000000
1000000.000000000000
"""
), "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 6`với các hoạt động được cung cấp |`1`,`3`,`1.5`,`2.25`| Tuyên truyền chính xác qua các lần xáo trộn chồng chéo | 
|`1 3`có giá trị`7`|`7`,`7`| Khoảng cách vị trí đơn và mảng nhỏ nhất có thể | 
| Năm giá trị bằng nhau với sự xáo trộn toàn bộ mảng và mảng con | Ba câu trả lời bằng`8`| Kỳ vọng thống nhất và bài tập lười biếng lặp đi lặp lại | 
| Bốn giá trị có sự xáo trộn`[1,2]`Và`[3,4]`|`1.5`,`1.5`,`9.5`,`9.5`,`1.5`| Ranh giới bao gồm và khoảng cách rời rạc | 
|`n = 250000`, tất cả các giá trị`1000000`| Hai câu trả lời bằng nhau`1000000`| Kích thước mảng tối đa và phân công phạm vi lớn | 

## Vỏ cạnh 

Đối với xáo trộn một vị trí, cây phân đoạn tính tổng khoảng và chia cho độ dài khoảng là`1`. Với```
1 3
7
get 1
shuffle 1 1
get 1
```tổng số tiền là`7`, trung bình là`7 / 1 = 7`và việc gán phạm vi sẽ giữ nguyên giá trị. Cả hai truy vấn đều trả về`7.000000000000`. 

Đối với việc xáo trộn toàn mảng, mọi vị trí đều nhận được cùng một giá trị mong đợi vì mọi phần tử ban đầu đều có khả năng đạt đến mọi vị trí như nhau. Vì```
3 2
1 2 6
shuffle 1 3
get 2
```nút gốc đã lưu trữ tổng số tiền đầy đủ`9`. Tính toán xáo trộn`9 / 3 = 3`và phân công`3`đến tận gốc một cách lười biếng. Truy vấn điểm sau đó đẩy giá trị đó xuống đường dẫn của nó và trả về`3.000000000000`. 

Đối với ranh giới bao gồm, hãy xem xét```
3 2
1 2 9
shuffle 1 2
get 2
```khoảng thời gian được yêu cầu có tổng`3`và chiều dài`2`, vậy giá trị kỳ vọng mới của nó là`1.5`. Nhiệm vụ bao gồm chính xác các vị trí`1`Và`2`. Chức vụ`3`còn lại`9`, Và`get 2`trả lại`1.500000000000`. 

Đối với các lần xáo trộn chồng chéo, hãy xem xét trình tự được cung cấp```
3 4
1 2 3
shuffle 1 2
shuffle 2 3
get 1
get 3
```Sau thao tác đầu tiên, các vị trí`1`Và`2`cả hai đều có kỳ vọng`1.5`. Thao tác thứ hai truy vấn tổng vị trí hiện tại`2`Và`3`, đó là`1.5 + 3 = 4.5`, không`2 + 3 = 5`. Mức trung bình mới của nó là`2.25`. Kỳ vọng cuối cùng là`[1.5, 2.25, 2.25]`, vậy câu trả lời là`1.500000000000`Và`2.250000000000`. 

Để gán lặp lại một khoảng đã thống nhất, hãy xem xét```
4 3
5 5 5 5
shuffle 1 4
shuffle 2 3
get 2
```Phân công xáo trộn đầu tiên`5`tới mọi vị trí. Lần xáo trộn thứ hai tính toán khoảng trung bình của`(5+5)/2 = 5`, do đó trạng thái không thay đổi. Cây phân đoạn lười xử lý cả hai thao tác mà không mở rộng phép gán toàn bộ mảng đầu tiên thành các phần tử riêng lẻ. 

Đối với mảng lớn nhất được phép, mọi nút có thể được chỉ định cùng một kỳ vọng mà không cần thực hiện các thay đổi riêng lẻ. Với`250000`tất cả các vị trí đều chứa`1000000`, xáo trộn toàn dải vẫn có mức trung bình`1000000`. Truy vấn điểm sau này chỉ đi theo một đường dẫn cây, do đó kích thước của mảng không biến truy vấn thành quét tuyến tính.
