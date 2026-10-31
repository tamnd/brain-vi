---
title: "CF 106368A - Shustrik hay quên và chiếc điều khiển từ xa"
description: "Chúng ta có một máy tính có màn hình bắt đầu bằng giá trị A. Máy tính có một số M cố định và mỗi lần nhấn nút sẽ áp dụng một trong bốn phép biến đổi: cộng M, trừ M, nhân với M hoặc thay thế giá trị hiện tại bằng số dư của nó sau khi chia cho M."
date: "2026-06-25T08:14:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106368
codeforces_index: "A"
codeforces_contest_name: "Innopolis Open 2025-2026. Final round"
rating: 0
weight: 106368
solve_time_s: 50
verified: true
draft: false
---

[CF 106368A - Shustrik hay quên và chiếc điều khiển từ xa](https://codeforces.com/problemset/problem/106368/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một máy tính có màn hình bắt đầu bằng một giá trị`A`. Máy tính có một số cố định`M`và mỗi lần nhấn nút sẽ áp dụng một trong bốn phép biến đổi: thêm`M`, trừ`M`, nhân với`M`hoặc thay thế giá trị hiện tại bằng số dư của nó sau khi chia cho`M`. Mục tiêu không phải là đạt được một giá trị chính xác. Chúng tôi chỉ cần giá trị màn hình cuối cùng có cùng số dư như`B`khi chia cho`N`. Nhiệm vụ là xuất ra chuỗi lần nhấn nút ngắn nhất để đạt được điều này hoặc báo cáo rằng điều đó là không thể. 

Các giá trị của`A`Và`M`có thể lớn như`10^9`, Nhưng`N`nhiều nhất là`2 * 10^6`. Điều này ngay lập tức loại trừ việc lưu trữ các giá trị máy tính thực tế vì chúng có thể tăng cực kỳ lớn sau khi nhân. Chúng ta cần một đại diện trạng thái dựa trên số dư và số lượng trạng thái phải gần bằng`N`để phù hợp với giới hạn. Một giải pháp thực hiện một số việc như thử tất cả các giá trị có thể lên tới`M`là không thể bởi vì`M`không có giới hạn nhỏ hữu ích. 

Các trường hợp khó khăn đến từ`%`hoạt động. Nó không hoạt động giống như các phép toán khác khi chúng ta chỉ biết giá trị modulo`N`, bởi vì`% M`phụ thuộc vào giá trị modulo`M`. 

Ví dụ:```
A = 5, M = 6, B = 1, N = 6
```Câu trả lời là`-1`. Việc triển khai bất cẩn chỉ theo dõi các giá trị theo modulo`N`có thể nghĩ rằng`%`hoạt động luôn có thể tạo ra dư lượng`5 mod 6`, nhưng giá trị máy tính sau`%`phải nằm trong phạm vi`[0, M - 1]`và ở đây phần dư có thể truy cập không bao giờ bao gồm`1`. 

Một trường hợp quan trọng khác là khi phép nhân đã xảy ra trước đó`%`:```
A = 5, M = 9, B = 1, N = 8
```Một trình tự tối ưu là:```
*%+
```Sau đó`*`, giá trị trở thành`45`, chia hết cho`M`, Vì thế`%`sản xuất`0`, không`A mod M`. Nếu việc triển khai giả định`%`luôn cho giá trị ban đầu modulo`M`, nó sẽ bỏ lỡ các đường dẫn hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ mô phỏng mọi trình tự nhấn nút có thể xảy ra. Vì có bốn lựa chọn ở mỗi bước nên độ sâu`d`tìm kiếm khám phá lên đến`4^d`trình tự. Mặc dù câu trả lời được đảm bảo là nhiều nhất`N`, với`N`đạt được hai triệu, cách tiếp cận này vượt xa những gì có thể. 

Quan sát hữu ích là chúng ta không bao giờ cần giá trị máy tính đầy đủ. Chúng ta chỉ cần giá trị modulo`N`, cộng với đủ thông tin để dự đoán điều gì`%`sẽ làm được. 

Đặt giá trị hiện tại là`X`. Chúng tôi theo dõi hai điều: 

Đầu tiên là`X mod N`, bởi vì điều này cho chúng ta biết liệu chúng ta đã đạt được điều kiện yêu cầu hay chưa. 

Thứ hai là`X mod M`, bởi vì`%`thay thế`X`với đúng số dư này. 

Phần đáng ngạc nhiên là`X mod M`chỉ có hai giá trị có thể có trong toàn bộ quá trình. Ban đầu nó là`A mod M`. Thêm hoặc bớt`M`không thay đổi nó. nhân với`M`làm cho nó`0`. các`%`hoạt động giữ nguyên giá trị modulo`M`, vì số mới đã bằng số dư cũ. 

Vì vậy, mọi trạng thái có thể được biểu diễn dưới dạng:```
(current value modulo N, current value modulo M is A mod M or 0)
```Điều này mang lại nhiều nhất`2N`tiểu bang. Mọi thao tác đều trở thành một cạnh đồ thị bình thường và chuỗi ngắn nhất được tìm thấy bằng tìm kiếm theo chiều rộng. 

Tìm kiếm brute-force không thành công vì nó khám phá các chuỗi. Quan sát trên chuyển bài toán thành tìm đường đi ngắn nhất trong một đồ thị nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(4^N) | O(4^N) | Quá chậm | 
| Tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một biểu đồ ngầm trong đó mỗi trạng thái là một cặp`(r, t)`. Đây`r`là giá trị hiện tại modulo`N`, Và`t`cho biết giá trị hiện tại modulo`M`là giá trị ban đầu`A mod M`hoặc bằng không. 
2. Bắt đầu BFS từ`(A mod N, 0)`. Cờ ban đầu bằng 0 vì trước khi bất kỳ phép nhân nào xảy ra, giá trị modulo`M`vẫn còn`A mod M`. 
3. Đối với mỗi trạng thái bật lên, hãy tạo bốn thao tác có thể thực hiện được. 

các`+`hoạt động chỉ thay đổi modulo còn lại`N`:```
r -> (r + M) mod N
```Cờ vẫn giữ nguyên vì thêm`M`không thay đổi modulo dư`M`. 
4. Áp dụng`-`hoạt động tương tự:```
r -> (r - M) mod N
```Một lần nữa, modulo`M`phần còn lại không thay đổi. 
5. Áp dụng phép nhân:```
r -> (r * M) mod N
```Cờ mới trở thành một vì giá trị bây giờ chia hết cho`M`. 
6. Áp dụng`%`. 

Nếu cờ bằng 0 thì giá trị mới là`A mod M`. Nếu cờ là một thì giá trị mới bằng 0. Bản thân lá cờ không thay đổi. 
7. Dừng BFS khi trạng thái còn lại`B`đã đạt được. Bởi vì BFS khám phá các trạng thái bằng cách tăng khoảng cách, trạng thái đầu tiên như vậy sẽ cho chuỗi ngắn nhất. 
8. Lưu trữ trạng thái trước đó và thao tác được sử dụng để đạt mọi trạng thái mới. Đi theo các liên kết ngược này từ trạng thái trả lời để xây dựng lại trình tự thao tác. 

Tại sao nó hoạt động: 

Biểu đồ BFS chứa mọi tình huống máy tính có thể được nén thành trạng thái tương đương. Hai giá trị máy tính có cùng modulo`N`phần còn lại và cùng một modulo`M`phần còn lại luôn có hành vi tương lai giống hệt nhau cho cả bốn hoạt động. Vì thành phần thứ hai chỉ có thể`A mod M`hoặc`0`, biểu đồ chứa tất cả các khả năng có liên quan. BFS tìm đường đi ngắn nhất trong biểu đồ trạng thái hoàn chỉnh này, do đó trình tự được xây dựng lại là tối thiểu. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    A, M, B, N = map(int, input().split())

    base = A % M

    total = 2 * N
    parent = [-1] * total
    parent_op = [''] * total

    def get_id(flag, rem):
        return flag * N + rem

    start = get_id(0, A % N)

    q = deque([start])
    parent[start] = start

    answer = -1

    while q:
        cur = q.popleft()
        flag = cur // N
        rem = cur % N

        if rem == B:
            answer = cur
            break

        nxt = [
            (flag, (rem + M) % N, '+'),
            (flag, (rem - M) % N, '-'),
            (1, (rem * M) % N, '*'),
            (flag, base % N if flag == 0 else 0, '%')
        ]

        for nf, nr, op in nxt:
            nid = get_id(nf, nr)
            if parent[nid] == -1:
                parent[nid] = cur
                parent_op[nid] = op
                q.append(nid)

    if answer == -1:
        print(-1)
        return

    path = []
    while answer != start:
        path.append(parent_op[answer])
        answer = parent[answer]

    path.reverse()

    print(len(path))
    if path:
        print(''.join(path))

if __name__ == "__main__":
    solve()
```Mã lưu trữ mỗi trạng thái dưới dạng số nguyên. Các bang có cờ`0`chiếm chỉ số`0`bởi vì`N-1`và các trạng thái có cờ`1`chiếm chỉ số`N`bởi vì`2N-1`. Điều này giữ cho mảng BFS nhỏ gọn và tránh các bộ dữ liệu trong vòng lặp nóng. 

Danh sách chuyển tiếp trực tiếp theo sau bốn thao tác máy tính. các`%`transition là cách duy nhất yêu cầu thêm cờ. Một lỗi phổ biến là thay đổi cờ sau`%`, nhưng modulo còn lại`M`vẫn giữ nguyên vì kết quả của`%`đã là phần còn lại rồi. 

Mảng cha cho phép xây dựng lại mà không cần lưu trữ các chuỗi hoàn chỉnh trong hàng đợi BFS. Điều này quan trọng vì biểu đồ có thể chứa tới bốn triệu trạng thái trong trường hợp lớn nhất. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
1 3 2 5
```Con đường ngắn nhất là`+*`. 

| Bước | Cờ tiểu bang | Giá trị mod N | Hoạt động | 
| --- | --- | --- | --- | 
| 0 | bản gốc | 1 | bắt đầu | 
| 1 | bản gốc | 4 | + | 
| 2 | không | 2 | * | 

Sau phép nhân, cờ thay đổi vì giá trị máy tính chia hết cho`M`. Số dư cuối cùng là`2`, phù hợp với mục tiêu. 

Đối với mẫu thứ hai:```
6 7 2 4
```| Bước | Cờ tiểu bang | Giá trị mod N | Hoạt động | 
| --- | --- | --- | --- | 
| 0 | bản gốc | 2 | bắt đầu | 

Giá trị ban đầu đã có phần dư mong muốn nên BFS ngay lập tức trả về một chuỗi trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Có nhiều nhất`2N`trạng thái và bốn chuyển đổi cho mỗi trạng thái. | 
| Không gian | O(N) | Hàng đợi BFS và mảng gốc lưu trữ một lượng dữ liệu không đổi trên mỗi trạng thái. | 

Giá trị tối đa của`N`là hai triệu, vậy đồ thị có nhiều nhất là bốn triệu trạng thái. Thuật toán tránh sự phụ thuộc vào kích thước của`A`hoặc`M`, đó là điều làm cho nó phù hợp với các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    A, M, B, N = map(int, sys.stdin.readline().split())

    base = A % M
    total = 2 * N
    parent = [-1] * total
    parent_op = [''] * total

    def gid(flag, rem):
        return flag * N + rem

    start = gid(0, A % N)
    q = deque([start])
    parent[start] = start
    ans = -1

    while q:
        cur = q.popleft()
        flag = cur // N
        rem = cur % N

        if rem == B:
            ans = cur
            break

        for nf, nr, op in [
            (flag, (rem + M) % N, '+'),
            (flag, (rem - M) % N, '-'),
            (1, (rem * M) % N, '*'),
            (flag, base % N if flag == 0 else 0, '%')
        ]:
            nxt = gid(nf, nr)
            if parent[nxt] == -1:
                parent[nxt] = cur
                parent_op[nxt] = op
                q.append(nxt)

    if ans == -1:
        out = "-1"
    else:
        res = []
        while ans != start:
            res.append(parent_op[ans])
            ans = parent[ans]
        res.reverse()
        out = str(len(res)) + ("\n" + ''.join(res) if res else "")

    sys.stdin = old
    return out

assert run("1 3 2 5\n") == "2\n+*", "sample 1"
assert run("6 7 2 4\n") == "0", "sample 2"

assert run("5 6 1 6\n") == "-1", "impossible case"
assert run("0 6 3 7\n") == "3\n---", "negative moves are useful"
assert run("5 9 1 8\n") == "3\n*%+", "multiplication changes modulo M state"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 3 2 5`|`2`hoạt động | Tái thiết đường đi ngắn nhất cơ bản | 
|`6 7 2 4`|`0`hoạt động | Đã thỏa mãn điều kiện | 
|`5 6 1 6`|`-1`| Phát hiện trạng thái không thể truy cập | 
|`0 6 3 7`|`3`hoạt động | Phép trừ lặp đi lặp lại và dư lượng biên | 
|`5 9 1 8`|`3`hoạt động | Xử lý đúng`%`sau đó`*`| 

## Vỏ cạnh 

Khi giá trị ban đầu đã thỏa mãn điều kiện thì BFS phải trả về ngay lập tức. Đối với đầu vào:```
6 7 2 4
```số dư ban đầu là`6 mod 4 = 2`, bằng`B`, vì vậy chuỗi trống là câu trả lời ngắn nhất có thể. 

Khi`%`được sử dụng sau phép nhân, modulo`M`trạng thái bằng không. Vì:```
5 9 1 8
```trình tự`*%+`hoạt động. Phép nhân thay đổi modulo ẩn`M`giá trị về 0,`%`giữ màn hình ở mức 0 và`+`di chuyển phần còn lại đến mục tiêu. Chỉ theo dõi`A mod M`sẽ bỏ lỡ con đường này. 

Khi câu trả lời không tồn tại, BFS sẽ khám phá mọi trạng thái nén có thể truy cập và không bao giờ đạt đến phần còn lại của`B`. Vì:```
5 6 1 6
```tất cả các trạng thái có thể đã cạn kiệt mà không tìm thấy dư lượng`1`, do đó thuật toán in chính xác`-1`.
