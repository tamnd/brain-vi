---
title: "CF 1774D - Cùng Đếm Một"
description: "Chúng ta có n hàng nhị phân, mỗi hàng có độ dài m. Một thao tác đơn lẻ sẽ chọn hai hàng và một cột, sau đó hoán đổi các giá trị trong cột đó giữa hai hàng đó. Vì việc hoán đổi xảy ra trong cùng một cột nên tổng số số 1 trong toàn bộ ma trận không bao giờ thay đổi."
date: "2026-06-09T12:00:44+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1774
codeforces_index: "D"
codeforces_contest_name: "Polynomial Round 2022 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 1600
weight: 1774
solve_time_s: 129
verified: true
draft: false
---

[CF 1774D - Cùng Đếm Một](https://codeforces.com/problemset/problem/1774/D) 

**Đánh giá:** 1600 
**Tags:** vũ lực, thuật toán xây dựng, tham lam, thực hiện, hai con trỏ 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

chúng tôi có`n`hàng nhị phân, mỗi hàng có độ dài`m`. Một thao tác đơn lẻ sẽ chọn hai hàng và một cột, sau đó hoán đổi các giá trị trong cột đó giữa hai hàng đó. 

Bởi vì việc hoán đổi xảy ra trong cùng một cột nên tổng số`1`s trong toàn bộ ma trận không bao giờ thay đổi. Những thay đổi đó là cách chúng`1`s được phân phối giữa các hàng. 

Mục đích là làm cho mỗi hàng chứa cùng một số lượng`1`S. Nếu điều đó là không thể, chúng tôi xuất ra`-1`. Nếu không, chúng ta phải xuất ra số lượng thao tác tối thiểu và một chuỗi hoán đổi hợp lệ để đạt được mục tiêu. 

Quan sát đầu tiên là tổng số`1`s là bất biến. Nếu tổng số`1`s không chia hết cho`n`, thì việc cân bằng là không thể vì mỗi hàng sẽ cần cùng một số nguyên`1`là. 
Kích thước đầu vào lớn. Mặc dù`n`Và`m`có thể tiếp cận riêng lẻ`10^5`, tổng của`n·m`trên tất cả các trường hợp thử nghiệm là nhiều nhất`10^6`. Điều này gợi ý rõ ràng rằng cần phải có một thuật toán tuyến tính hoặc gần tuyến tính trong kích thước ma trận. Bất kỳ phương pháp nào liên tục quét tất cả các hàng cho mọi thao tác sẽ trở nên quá chậm. 

Phần khó khăn không phải là quyết định liệu một giải pháp có tồn tại hay không. Thách thức là xây dựng số lượng giao dịch hoán đổi tối thiểu. 

Một sai lầm phổ biến là nghĩ rằng bất cứ khi nào một hàng có quá nhiều`1`s và cái khác có quá ít, chúng ta luôn có thể chuyển một`1`trực tiếp. Điều đó không đúng. Việc chuyển khoản chỉ có thể thực hiện được trong một cột nơi hàng nhà tài trợ có`1`và hàng người nhận có`0`. 

Coi như:```
2 2
1 1
1 1
```Cả hai hàng đều đã chứa hai`1`s, vì vậy câu trả lời là không có phép toán nào. 

Bây giờ hãy xem xét:```
2 2
1 0
1 0
```Tổng số`1`s là hai, vì vậy mục tiêu là một trên mỗi hàng. Cả hai hàng đều đã có một`1`, vì vậy một lần nữa câu trả lời là không. 

Việc triển khai bất cẩn cố gắng "cân bằng cột" thay vì đếm hàng có thể thực hiện các hoán đổi không cần thiết. 

Một trường hợp tế nhị khác là:```
2 2
1 1
0 0
```Mục tiêu là một`1`mỗi hàng. Chỉ cần một lần trao đổi. Chúng ta hoán đổi cột 1 hoặc cột 2 giữa các hàng. Bất kỳ giải pháp nào sử dụng hai lần hoán đổi đều đúng ở trạng thái cuối cùng nhưng không tối thiểu. 

Yêu cầu hoạt động tối thiểu rất quan trọng. 

## Phương pháp tiếp cận 

Chiến lược brute-force là liên tục chọn một hàng đầy đủ và một hàng thiếu đầy đủ, sau đó tìm kiếm tất cả các cột cho đến khi tìm thấy vị trí có thể chuyển nhượng. Sau mỗi lần chuyển, hãy tính toán lại mọi thứ và tiếp tục. 

Điều này có tác dụng vì mỗi lần hoán đổi thành công sẽ làm giảm sự mất cân bằng đi một. Cuối cùng tất cả các hàng đều đạt được số lượng mục tiêu. 

Vấn đề là hiệu quả. Giả sử có`O(nm)`chuyển khoản cần thiết. Nếu mỗi lần chuyển quét lại toàn bộ ma trận thì độ phức tạp có thể đạt tới`O(nm·min(n,m))`hoặc tệ hơn là vượt xa giới hạn cho phép. 

Quan sát quan trọng là một giao dịch hoán đổi hữu ích luôn trông giống nhau. 

Một hàng có số lượng lớn hơn mục tiêu phải đóng góp một số`1`S. 

Một hàng có số lượng nhỏ hơn mục tiêu phải nhận được một số`1`S. 

Đối với một cột cố định, nếu một hàng có`1`và cần quyên góp trong khi một hàng khác có`0`và cần nhận thì cột đó có thể thực hiện đúng một lần chuyển được yêu cầu. 

Điều này gợi ý việc xử lý các cột một cách độc lập. Đối với mỗi cột, chúng tôi thu thập: 

Đầu tiên, các hàng hiện có`1`trong cột này và vẫn còn thừa`1`tổng thể. 

Thứ hai, các hàng hiện có`0`trong cột này và vẫn cần bổ sung`1`tổng thể. 

Việc khớp một hàng của nhóm đầu tiên với một hàng của nhóm thứ hai sẽ tạo ra một hoán đổi hợp lệ. Mỗi lần hoán đổi như vậy sẽ đồng thời làm giảm một khoản thặng dư và một khoản thâm hụt. 

Tại sao điều này là tối ưu? 

Mỗi lần hoán đổi sẽ thay đổi số lượng chính xác hai hàng một đơn vị theo hướng ngược nhau. Nếu một hàng vượt quá mục tiêu`d`, ít nhất phải tham gia với tư cách là nhà tài trợ`d`lần. Tính tổng tất cả các hàng dư thừa sẽ cho ra số lần chuyển tối thiểu cần thiết. Công trình của chúng tôi thực hiện chính xác một lần hoán đổi cho mỗi lần chuyển`1`, do đó nó đạt tới giới hạn dưới này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²m) hoặc tệ hơn | O(1) | Quá chậm | 
| Tối ưu | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ma trận và tính số`1`s ở mỗi hàng. 
2. Tính tổng số`1`s trong ma trận. 
3. Nếu tổng không chia hết cho`n`, đầu ra`-1`. 
4. Hãy để`target = total // n`. 
5. Đối với mỗi cột, hãy xây dựng hai danh sách tạm thời. 

Danh sách đầu tiên chứa các hàng có số lượng lớn hơn`target`và giá trị của nó trong cột này là`1`. 

Danh sách thứ hai chứa các hàng có số lượng nhỏ hơn`target`và giá trị của nó trong cột này là`0`. 
6. Ghép các hàng từ hai danh sách theo thứ tự. 

Mỗi cặp khớp đại diện cho một lần chuyển hợp lệ. Hoán đổi giá trị cột giữa người cho và người nhận. 
7. Sau khi ghi lại thao tác, hãy giảm số hàng của nhà tài trợ xuống một và tăng số hàng của người nhận lên một. 

Bản cập nhật này rất quan trọng vì một hàng có thể ngừng đóng vai trò là người cho hoặc người nhận sau khi chuyển đủ số lượng. 
8. Tiếp tục qua tất cả các cột. 
9. Xuất ra tất cả các hoạt động được ghi lại. 

### Tại sao nó hoạt động 

hãy để`target`là số lượng mong muốn`1`s ở mỗi hàng. 

Một hàng có thặng dư`s = count - target`phải quyên góp chính xác`s`những cái đó. Một hàng có thâm hụt`d = target - count`phải nhận chính xác`d`những cái đó. Tổng thặng dư bằng tổng thâm hụt. 

Bất cứ khi nào chúng tôi thực hiện hoán đổi, chúng tôi sẽ chọn một cột mà nhà tài trợ có`1`và người nhận có`0`. Sau khi hoán đổi, nhà tài trợ mất một`1`và người nhận nhận được một`1`. Không có số hàng khác thay đổi. 

Mỗi hoạt động làm giảm sự mất cân bằng tổng cộng hai đơn vị, một thặng dư và một thâm hụt. Vì mỗi thao tác đều chuyển chính xác một yêu cầu`1`, số lượng thao tác bằng tổng số dư, là số lần hoán đổi nhỏ nhất có thể. Như vậy việc xây dựng vừa chính xác vừa tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, m = map(int, input().split())

        a = []
        cnt = []

        total = 0

        for _ in range(n):
            row = list(map(int, input().split()))
            a.append(row)

            s = sum(row)
            cnt.append(s)
            total += s

        if total % n != 0:
            print(-1)
            continue

        target = total // n
        ops = []

        for col in range(m):
            donors = []
            receivers = []

            for row in range(n):
                if cnt[row] > target and a[row][col] == 1:
                    donors.append(row)
                elif cnt[row] < target and a[row][col] == 0:
                    receivers.append(row)

            k = min(len(donors), len(receivers))

            for i in range(k):
                x = donors[i]
                y = receivers[i]

                cnt[x] -= 1
                cnt[y] += 1

                a[x][col] = 0
                a[y][col] = 1

                ops.append((x + 1, y + 1, col + 1))

        print(len(ops))
        for x, y, z in ops:
            print(x, y, z)

solve()
```Giải pháp bắt đầu bằng cách đếm số lượng`1`s ở mỗi hàng. Số lượng đó là số lượng duy nhất quan trọng để quyết định ai phải quyên góp và ai phải nhận. 

Việc kiểm tra tính chia hết được thực hiện trước tiên vì không có chuỗi hoán đổi nào có thể thay đổi tổng số`1`s trong ma trận. 

Đối với mỗi cột, chúng tôi xác định người cho và người nhận hiện tại bằng cách sử dụng số lượng hàng cập nhật. Số lượng được cập nhật ngay sau mỗi lần hoán đổi. Chi tiết này là cần thiết. Một hàng bắt đầu với số dư ba sẽ ngừng quyên góp sau đúng ba lần chuyển. 

Bản thân ma trận cũng được cập nhật sau mỗi thao tác. Nếu không cập nhật các giá trị cột, quá trình xử lý sau này có thể sử dụng lại giá trị đó một cách không chính xác`1`nhiều lần. 

Hoạt động được ghi lại`(x+1, y+1, col+1)`tuân theo chỉ mục dựa trên 1 theo yêu cầu của câu lệnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4
1 1 1 0
0 0 1 0
1 0 0 1
```Số hàng là: 

| Hàng | Đếm | 
| --- | --- | 
| 1 | 3 | 
| 2 | 1 | 
| 3 | 2 | 

Tổng cộng = 6, mục tiêu = 2. 

Xử lý các cột: 

| Cột | Nhà tài trợ | Người nhận | Hoạt động | 
| --- | --- | --- | --- | 
| 1 | {1} | {2} | (1,2,1) | 
| 2 | {} | {} | không | 
| 3 | {} | {} | không | 
| 4 | {} | {} | không | 

Sau khi hoán đổi: 

| Hàng | Đếm Mới | 
| --- | --- | 
| 1 | 2 | 
| 2 | 2 | 
| 3 | 2 | 

Tất cả các hàng đều đạt được mục tiêu chỉ bằng một thao tác. 

Ví dụ này cho thấy cách một cột có thể giải quyết đồng thời cả một khoản thặng dư và một khoản thâm hụt. 

### Ví dụ 2 

đầu vào:```
4 3
1 0 0
0 1 1
0 0 1
0 0 0
```Đếm: 

| Hàng | Đếm | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 1 | 
| 4 | 0 | 

Tổng cộng = 4, mục tiêu = 1. 

Xử lý: 

| Cột | Nhà tài trợ | Người nhận | Hoạt động | 
| --- | --- | --- | --- | 
| 1 | {} | {} | không | 
| 2 | {2} | {4} | (2,4,2) | 
| 3 | {} | {} | không | 

Số đếm cuối cùng: 

| Hàng | Đếm | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 

Dấu vết này cho thấy rằng chúng ta chỉ cần xử lý các cột mà nhà tài trợ có`1`và một người nhận có`0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được kiểm tra một số lần không đổi | 
| Không gian | O(nm) | Lưu trữ ma trận cộng với danh sách hoạt động | 

Tổng giá trị của`n·m`trên tất cả các trường hợp thử nghiệm là nhiều nhất`10^6`. Quét tuyến tính tất cả các ô dễ dàng đủ nhanh trong giới hạn 2 giây trong Python. Việc sử dụng bộ nhớ cũng vừa vặn thoải mái trong giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out

    def solve():
        input = sys.stdin.readline

        t = int(input())

        for _ in range(t):
            n, m = map(int, input().split())

            a = []
            cnt = []
            total = 0

            for _ in range(n):
                row = list(map(int, input().split()))
                a.append(row)
                s = sum(row)
                cnt.append(s)
                total += s

            if total % n:
                print(-1)
                continue

            target = total // n
            ops = []

            for col in range(m):
                donors = []
                receivers = []

                for row in range(n):
                    if cnt[row] > target and a[row][col] == 1:
                        donors.append(row)
                    elif cnt[row] < target and a[row][col] == 0:
                        receivers.append(row)

                k = min(len(donors), len(receivers))

                for i in range(k):
                    x = donors[i]
                    y = receivers[i]

                    cnt[x] -= 1
                    cnt[y] += 1

                    a[x][col] = 0
                    a[y][col] = 1

                    ops.append((x + 1, y + 1, col + 1))

            print(len(ops))
            for op in ops:
                print(*op)

    solve()

    sys.stdout = old_stdout
    return out.getvalue().strip()

# impossible case
assert run(
"""1
2 2
0 0
0 1
"""
) == "-1"

# already balanced
assert run(
"""1
2 2
1 0
0 1
"""
).splitlines()[0] == "0"

# one transfer needed
assert run(
"""1
2 2
1 1
0 0
"""
).splitlines()[0] == "1"

# all zeros
assert run(
"""1
3 3
0 0 0
0 0 0
0 0 0
"""
).splitlines()[0] == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[[0,0],[0,1]]`|`-1`| Tổng số không chia hết cho`n`| 
|`[[1,0],[0,1]]`|`0`hoạt động | Đã cân bằng | 
|`[[1,1],[0,0]]`|`1`hoạt động | Chuyển khoản tối thiểu không tầm thường | 
| Ma trận hoàn toàn bằng không |`0`hoạt động | Số mục tiêu bằng 0 | 

## Vỏ cạnh 

Hãy xem xét:```
2 2
0 0
0 1
```Tổng số`1`s là một. Từ`1 % 2 != 0`, không tồn tại sự phân bố bằng nhau. Thuật toán phát hiện điều này ngay lập tức và đưa ra`-1`. 

Coi như:```
2 2
1 0
0 1
```Mỗi hàng đã chứa một`1`. Mục tiêu cũng là một. Trong quá trình xử lý cột, không có hàng nào trở thành hàng cho hoặc hàng nhận. Không có thao tác nào được ghi lại và câu trả lời là 0. 

Coi như:```
2 2
1 1
0 0
```Số lượng là`(2,0)`và mục tiêu là một. Trong cột đầu tiên, hàng đầu tiên là nhà tài trợ và hàng thứ hai là người nhận. Một lần hoán đổi sẽ cân bằng cả hai hàng ngay lập tức. Thuật toán đưa ra chính xác một thao tác, đó là mức tối thiểu. 

Coi như:```
3 3
1 1 1
0 0 0
0 0 0
```Tổng số`1`s là ba, vậy mục tiêu là một. Hàng đầu tiên có thừa hai. Các hàng khác đều có một hàng bị thiếu. Các cột xử lý độc lập tìm thấy hai vị trí có thể chuyển nhượng và thực hiện chính xác hai lần hoán đổi. Sau hai lần chuyển đó, mỗi hàng chứa một`1`, và số thao tác bằng tổng số dư, là mức tối thiểu có thể.
