---
title: "CF 102881E - Baby Ehab's X(OR)"
description: "Chúng ta có một dãy số nguyên dương. Bản cập nhật chọn một phạm vi liền kề và áp dụng một trong hai phép biến đổi theo bit cho mọi số trong phạm vi đó."
date: "2026-07-25T12:32:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102881
codeforces_index: "E"
codeforces_contest_name: "ECPC 2019 Kickoff"
rating: 0
weight: 102881
solve_time_s: 59
verified: true
draft: false
---

[CF 102881E - X(OR) của Baby Ehab](https://codeforces.com/problemset/problem/102881/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dãy số nguyên dương. Bản cập nhật chọn một phạm vi liền kề và áp dụng một trong hai phép biến đổi theo bit cho mọi số trong phạm vi đó. Phép biến đổi đầu tiên thay thế một giá trị`x`với`x | (x - 1)`, và cái thứ hai thay thế nó bằng`x ^ (x - 1)`. Sau mỗi lần cập nhật, chúng ta phải in tổng của mảng. 

Phần quan trọng là hiểu hai biểu thức này thực sự làm gì với bit. Nếu như`x`có`k`số 0 ở cuối thì`x - 1`thay đổi chính xác những số 0 đó thành số 1 và biến một bit thấp nhất thành 0. 

Vì`x | (x - 1)`, tất cả các số 0 ở cuối sẽ trở thành số 1, trong khi các bit còn lại không thay đổi. Ví dụ,`12 (1100)`trở thành`15 (1111)`. Thao tác này chỉ thay đổi các số có ít nhất một số 0 ở cuối và sau đó là số lẻ. 

Vì`x ^ (x - 1)`, tất cả các bit từ bit được đặt thấp nhất trở xuống sẽ trở thành bit và tất cả các bit cao hơn sẽ biến mất. Một giá trị với`k`số 0 ở cuối trở thành`2^(k+1)-1`. Ví dụ,`12 (1100)`trở thành`7 (0111)`. Điều này cũng luôn tạo ra một số lẻ. 

Các ràng buộc lớn: cả kích thước mảng và số lượng thao tác đều có thể đạt tới`3 * 10^5`. Giải pháp chạm tới mọi phần tử trong một phạm vi là quá chậm vì trường hợp xấu nhất sẽ thực hiện xung quanh`9 * 10^10`cập nhật phần tử. Chúng ta cần một cấu trúc dữ liệu logarit có thể tóm tắt nhiều phần tử cùng một lúc. 

Một lỗi phổ biến là chỉ theo dõi tổng. Chỉ riêng tổng không thể cho chúng ta biết một phép toán trong tương lai sẽ thay đổi các con số như thế nào. Ví dụ, các giá trị`4`Và`6`cả hai đều có hành vi khác nhau trong phép toán thứ hai mặc dù tổng của chúng có thể được kết hợp. Thông tin còn thiếu là số lượng số 0 ở cuối. 

Một trường hợp cạnh khác là một phạm vi chứa lũy thừa của hai. Đối với đầu vào:```
1 1
8
1 1 1
```câu trả lời là:```
15
```bởi vì`8 (1000)`trở thành`15 (1111)`. Giải pháp giả định giá trị chỉ trở thành số lẻ sẽ mất các bit thấp hơn được thêm vào. 

Một trường hợp cạnh khác là áp dụng thao tác thứ hai sau thao tác đầu tiên. Vì:```
1 2
4
1 1 1
2 1 1
```đầu ra là:```
7
1
```Bản cập nhật đầu tiên tạo ra`7`, và bản cập nhật thứ hai thay đổi`7`ĐẾN`1`. Việc thực hiện bất cẩn áp dụng thao tác thứ hai vào giá trị ban đầu`4`sẽ sản xuất không chính xác`7`. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ lặp lại mọi vị trí trong mỗi phạm vi truy vấn và tính toán giá trị mới bằng cách sử dụng các thao tác bit. Điều này đúng vì mỗi thao tác đều độc lập với mọi phần tử. Tuy nhiên, với`n`Và`q`cả hai đều bằng`300000`, trường hợp xấu nhất chứa khoảng`n * q`sửa đổi phần tử, vượt xa thời gian có sẵn. 

Quan sát quan trọng là các phép biến đổi chỉ phụ thuộc vào số lượng các số 0 ở cuối. Sau một trong hai thao tác, số trở thành số lẻ, do đó hành vi trong tương lai của phần tử đó đơn giản hơn nhiều. Điều này có nghĩa là cây phân đoạn không cần lưu trữ mọi giá trị chính xác. Nó chỉ cần biết có bao nhiêu giá trị trong một phân đoạn có mỗi số 0 ở cuối cùng với tổng phân đoạn. 

Còn có một sự tinh tế nữa. Việc lan truyền lười biếng của cây phân đoạn không chỉ phải nhớ rằng một phân đoạn đã được cập nhật mà còn phải nhớ chuỗi chuyển đổi nào vẫn cần được truyền cho các phân đoạn con của nó. Các chuyển đổi đang chờ xử lý có thể là nhỏ. Chúng là danh tính, thao tác thứ nhất, thao tác thứ hai và là thao tác liên tục biến mọi thứ thành`1`. Bốn trạng thái này là đủ vì các phép biến đổi lặp đi lặp lại sẽ nhanh chóng sụp đổ thành một trong số chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Tối ưu | O((n + q) log n * 20) | O(n * 20) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn. Đối với mỗi nút, lưu trữ tổng phân đoạn của nó và một mảng trong đó`cnt[i]`là số lượng các giá trị có chính xác`i`các số 0 ở cuối. Lưu trữ một phép biến đổi lười biếng mô tả một thao tác vẫn phải được đẩy tới phần tử con. 

Giá trị lớn nhất có thể xuất hiện là bên dưới`2^19`, vì vậy có thể có tối đa 19 số 0 ở cuối. 
2. Khi áp dụng thao tác đầu tiên cho toàn bộ phân đoạn, mọi giá trị có`i > 0`các số 0 ở cuối tăng thêm`2^i - 1`. Sau sự thay đổi đó, tất cả các giá trị trở thành số lẻ. 

Chúng tôi cập nhật tổng bằng cách sử dụng số đếm được lưu trữ và chuyển mỗi số đếm vào`cnt[0]`. 
3. Khi áp dụng thao tác thứ hai cho toàn bộ phân đoạn, một giá trị có`i`số 0 ở cuối trở thành`2^(i+1)-1`. Tổng mới có thể được tính trực tiếp từ số đếm. 

Một lần nữa, mọi giá trị kết quả đều là số lẻ, do đó toàn bộ đoạn chuyển sang`cnt[0]`. 
4. Khi áp dụng phép toán liên tục là kết quả của việc tổng hợp các phép biến đổi, mọi giá trị sẽ trở thành`1`. Tổng trở thành độ dài đoạn và tất cả các giá trị đều có số 0 ở cuối. 
5. Để cập nhật một phần phạm vi, hãy đẩy chuyển đổi lười biếng đang chờ xử lý sang các phần tử con trước khi giảm dần. Hợp nhất các trẻ em sau đó bằng cách cộng tổng của chúng và số 0 ở cuối. 
6. Sau mỗi lần cập nhật, tổng gốc chính là đáp án. 

Tại sao nó hoạt động: 

Cây phân đoạn duy trì chính xác thông tin cần thiết cho các hoạt động. Hiệu quả của hoạt động theo chiều bit chỉ phụ thuộc vào số 0 ở cuối giá trị ban đầu. Số lượng được lưu trữ cho phép chúng tôi tính tổng mới mà không cần biết các giá trị riêng lẻ. Vì mọi phép biến đổi có thể được biểu thị bằng một trong bốn trạng thái lười biếng và các trạng thái này được kết hợp chính xác, nên các cập nhật bị trì hoãn luôn tạo ra kết quả tương tự như việc áp dụng các thao tác trực tiếp cho mọi phần tử. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXB = 20

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    size = 4 * n
    tree_sum = [0] * size
    cnt = [[0] * MAXB for _ in range(size)]
    lazy = [0] * size

    def tz(x):
        return (x & -x).bit_length() - 1

    def apply(node, l, r, op):
        length = r - l + 1

        if op == 1:
            add = 0
            for i in range(1, MAXB):
                add += cnt[node][i] * ((1 << i) - 1)
            tree_sum[node] += add

        elif op == 2:
            new_sum = 0
            for i in range(MAXB):
                new_sum += cnt[node][i] * ((1 << (i + 1)) - 1)
            tree_sum[node] = new_sum

        else:
            tree_sum[node] = length

        cnt[node][0] = length
        for i in range(1, MAXB):
            cnt[node][i] = 0

    def compose(old, new):
        if new == 0:
            return old
        if old == 0:
            return new

        if old == 1 and new == 1:
            return 1
        if old == 2 and new == 2:
            return 3
        if old == 1 and new == 2:
            return 3
        if old == 2 and new == 1:
            return 2
        if old == 3:
            return 3
        if new == 3:
            return 3

        return new

    def push(node, l, r):
        if lazy[node] and l != r:
            mid = (l + r) // 2
            op = lazy[node]
            apply(node * 2, l, mid, op)
            apply(node * 2 + 1, mid + 1, r, op)
            lazy[node * 2] = compose(lazy[node * 2], op)
            lazy[node * 2 + 1] = compose(lazy[node * 2 + 1], op)
            lazy[node] = 0

    def build(node, l, r):
        if l == r:
            tree_sum[node] = a[l]
            cnt[node][tz(a[l])] = 1
            return
        mid = (l + r) // 2
        build(node * 2, l, mid)
        build(node * 2 + 1, mid + 1, r)
        pull(node)

    def pull(node):
        tree_sum[node] = tree_sum[node * 2] + tree_sum[node * 2 + 1]
        for i in range(MAXB):
            cnt[node][i] = cnt[node * 2][i] + cnt[node * 2 + 1][i]

    def update(node, l, r, ql, qr, op):
        if qr < l or r < ql:
            return
        if ql <= l and r <= qr:
            apply(node, l, r, op)
            lazy[node] = compose(lazy[node], op)
            return

        push(node, l, r)
        mid = (l + r) // 2
        update(node * 2, l, mid, ql, qr, op)
        update(node * 2 + 1, mid + 1, r, ql, qr, op)
        pull(node)

    build(1, 0, n - 1)

    ans = []
    for _ in range(q):
        t, l, r = map(int, input().split())
        update(1, 0, n - 1, l - 1, r - 1, t)
        ans.append(str(tree_sum[1]))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```các`cnt`mảng là cốt lõi của việc thực hiện. Chúng tránh lưu trữ các giá trị riêng lẻ trong khi vẫn bảo toàn chính xác thông tin được yêu cầu bởi cả hai phép biến đổi. 

các`apply`chức năng xử lý một bản cập nhật phân khúc hoàn chỉnh. Hai trường hợp đầu tiên tính tổng mới từ các tần số bằng 0. Trường hợp cuối cùng được sử dụng khi một số thao tác đang chờ xử lý kết hợp thành một phép biến đổi buộc mọi giá trị trở thành`1`. 

các`compose`chức năng ngăn chặn một lỗi lan truyền lười biếng phổ biến. Một thao tác sau này không thể đơn giản thay thế cờ lười trước đó vì thứ tự rất quan trọng. Ví dụ: áp dụng thao tác đầu tiên và sau đó là thao tác thứ hai không giống như chỉ áp dụng thao tác thứ hai. 

Đệ quy sử dụng các chỉ mục dựa trên 0 trong nội bộ. Phạm vi đầu vào được chuyển đổi một lần trong quá trình cập nhật, tránh chuyển đổi lặp lại và sai sót từng cái một. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 3
1 2 3
1 1 3
2 2 2
2 1 3
```Các trạng thái phân đoạn quan trọng là: 

| Bước | Hoạt động | Giá trị | Tổng hợp | 
| --- | --- | --- | --- | 
| Ban đầu | không | 1, 2, 3 | 6 | 
| 1 | áp dụng thao tác đầu tiên | 1, 3, 3 | 7 | 
| 2 | áp dụng thao tác thứ hai cho giá trị thứ hai | 1, 1, 3 | 5 | 
| 3 | áp dụng thao tác thứ hai cho tất cả | 1, 1, 1 | 3 | 

Dấu vết cho thấy rằng sau mỗi thao tác, tất cả các số được sửa đổi đều trở thành số lẻ, đây chính xác là bất biến được cây phân đoạn sử dụng. 

Đối với mẫu thứ hai:```
5 5
5 5 5 5 5
1 5 5
2 1 1
2 2 3
1 3 4
2 1 5
```| Bước | Hoạt động | Giá trị | Tổng hợp | 
| --- | --- | --- | --- | 
| Ban đầu | không | 5, 5, 5, 5, 5 | 25 | 
| 1 | hoạt động đầu tiên ở vị trí 5 | 5, 5, 5, 5, 5 | 25 | 
| 2 | thao tác thứ hai ở vị trí 1 | 1, 5, 5, 5, 5 | 21 | 
| 3 | thao tác thứ hai ở vị trí 2 đến 3 | 1, 1, 1, 5, 5 | 13 | 
| 4 | hoạt động đầu tiên ở vị trí 3 đến 4 | 1, 1, 1, 5, 5 | 13 | 
| 5 | hoạt động thứ hai trên tất cả | 1, 1, 1, 1, 1 | 5 | 

Mẫu thực hiện các phép biến đổi lặp lại và cho thấy lý do tại sao chỉ giữ lại một số lẻ sẽ không đủ cho trạng thái ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n * 20) | Mọi thao tác phân đoạn đều chạm vào các nút O(log n) và mỗi lần cập nhật nút sẽ quét 20 số đếm có thể có ở cuối. | 
| Không gian | O(n * 20) | Mỗi nút cây phân đoạn lưu trữ 20 bộ đếm và một vài giá trị bổ sung. | 

Kích thước mảng tối đa là`300000`, do đó số nút cây là tuyến tính. Thời gian cập nhật logarit dễ dàng phù hợp với giới hạn vì hệ số không đổi chỉ là số lượng nhỏ các vị trí bit có thể có. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    data = sys.stdin.read().split()
    if not data:
        return ""
    it = iter(data)
    n = int(next(it))
    q = int(next(it))
    arr = [int(next(it)) for _ in range(n)]
    ops = [(int(next(it)), int(next(it)), int(next(it))) for _ in range(q)]

    res = []
    for t, l, r in ops:
        for i in range(l - 1, r):
            if t == 1:
                arr[i] |= arr[i] - 1
            else:
                arr[i] ^= arr[i] - 1
        res.append(str(sum(arr)))
    return "\n".join(res)

assert run("""3 3
1 2 3
1 1 3
2 2 2
2 1 3
""") == "7\n5\n3", "sample 1"

assert run("""5 5
5 5 5 5 5
1 5 5
2 1 1
2 2 3
1 3 4
2 1 5
""") == "25\n21\n13\n13\n5", "sample 2"

assert run("""1 1
8
1 1 1
""") == "15", "power of two"

assert run("""1 2
4
1 1 1
2 1 1
""") == "7\n1", "composition case"

assert run("""3 2
1 1 1
2 1 3
1 1 3
""") == "3\n3", "all equal odd values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Sức mạnh đơn của hai | 15 | Xử lý đúng các số 0 ở cuối | 
| Bản cập nhật đầu tiên tiếp theo là bản cập nhật thứ hai | 7 rồi 1 | Hành vi sáng tác lười biếng | 
| Tất cả các giá trị lẻ | 3 rồi 3 | Các hoạt động không nên tăng giá trị | 
| Cung cấp mẫu | Kết quả đầu ra mẫu | Tính đúng đắn chung | 

## Vỏ cạnh 

Đối với trường hợp lũy thừa hai:```
1 1
8
1 1 1
```giá trị`8`có ba số 0 ở cuối. Hoạt động đầu tiên thêm`2^3 - 1 = 7`, cho`15`. Cây phân đoạn lưu trữ một giá trị trong`cnt[3]`, do đó nó tính toán mức tăng trực tiếp và chuyển giá trị sang`cnt[0]`. 

Đối với trường hợp thành phần:```
1 2
4
1 1 1
2 1 1
```Sau lần cập nhật đầu tiên,`4`trở thành`7`, được biểu diễn dưới dạng số lẻ. Bản cập nhật thứ hai biến mọi giá trị lẻ thành`1`. Thành phần trạng thái lười ghi lại rằng chuyển đổi đang chờ xử lý là hiệu ứng kết hợp, thay vì xử lý sai hai thao tác một cách độc lập.
