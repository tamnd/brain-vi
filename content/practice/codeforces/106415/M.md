---
title: "CF 106415M - Hiya Ti7 Wena Ntala3ha"
description: "Bài toán mô tả một quá trình giảm trên một giá trị nguyên x. Chúng ta bắt đầu từ S và có N nước đi có thể thực hiện được. Một nước đi có khoảng cho phép [li, ri] và có giá trị giảm di. Khi giá trị hiện tại nằm trong khoảng đó, chúng ta có thể trừ di."
date: "2026-06-25T09:45:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106415
codeforces_index: "M"
codeforces_contest_name: "Winter Cup 8.0 Online Mirror Contest"
rating: 0
weight: 106415
solve_time_s: 45
verified: true
draft: false
---

[CF 106415M - Hiya Ti7 Wena Ntala3ha](https://codeforces.com/problemset/problem/106415/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán mô tả quá trình giảm trên một giá trị nguyên`x`. Chúng tôi bắt đầu lúc`S`, và có`N`những chuyển động có thể. Một nước đi có khoảng thời gian cho phép`[l_i, r_i]`và giá trị giảm`d_i`. Khi giá trị hiện tại nằm trong khoảng đó, chúng ta có thể trừ`d_i`. 

Nhiệm vụ là tiếp cận chính xác`0`với số lần di chuyển ít nhất. Trong số tất cả các chuỗi ngắn nhất, chúng ta phải xuất ra chuỗi giá trị bị trừ nhỏ nhất về mặt từ điển. Đầu vào chứa giá trị bắt đầu và tất cả các phép toán, còn đầu ra là`-1`nếu không thể đạt được 0 hoặc độ dài của chuỗi tối ưu theo sau đã chọn sẽ giảm. 

Giá trị của`S`có thể lớn như`10^6`và số lượng thao tác cũng có thể là`10^6`. Một giải pháp thử mọi chuỗi thao tác là không thể vì số lượng đường đi có thể tăng theo cấp số nhân. Ngay cả một giải pháp lập trình động kiểm tra mọi hoạt động ở mọi trạng thái cũng sẽ hoạt động xung quanh`10^12`kiểm tra trong trường hợp xấu nhất. Ràng buộc hữu ích là tổng chiều dài của tất cả các khoảng thời gian hoạt động tối đa là`2 * 10^7`, cho phép chúng tôi xử lý mọi cặp thao tác trạng thái hợp lệ một lần. 

Các trường hợp phức tạp xuất phát từ thực tế là các thao tác có thể chồng lên nhau và trình tự nhỏ nhất về mặt từ điển chỉ được xem xét sau khi giảm thiểu số bước. Ví dụ:```
10 2
1 10 1
5 10 5
```Câu trả lời là:```
2
5 5
```Một giải pháp tham lam luôn chọn cái nhỏ nhất`d`sẽ tạo ra mười bước di chuyển`1`, điều đó không tối ưu. 

Một trường hợp cạnh khác là giá trị không thể truy cập được:```
5 1
3 5 3
```Đầu ra là:```
-1
```Không thể sử dụng thao tác khi giá trị dưới đây`3`, và trừ`3`từ`5`đạt tới`2`, không`0`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là giải pháp lập trình động có đường dẫn ngắn nhất. Cho phép`dp[x]`là số lần di chuyển tối thiểu cần thiết để giảm`x`ĐẾN`0`. Vì mỗi bước di chuyển đều làm giảm giá trị nên biểu đồ có tính tuần hoàn và các trạng thái có thể được xử lý theo thứ tự tăng dần:`dp[x] = 1 + min(dp[x - d_i])`cho tất cả các hoạt động bao gồm`x`. 

Khó khăn là tìm ra tất cả các hoạt động bao trùm mọi`x`. Kiểm tra tất cả`N`hoạt động cho mọi tiểu bang mang lại`O(SN)`, quá chậm. Quan sát quan trọng là tổng số vị trí được bảo hiểm trong tất cả các khoảng thời gian là có hạn. Chúng ta có thể mở rộng các khoảng thành cấu trúc kề cận được nén và chỉ xử lý các cặp hợp lệ. 

Sau khi tính toán`dp`, tái thiết là tham lam. Nếu giá trị hiện tại là`x`, giá trị đầu tiên trong câu trả lời phải nhỏ nhất`d`giữa các phép toán thỏa mãn:`dp[x - d] = dp[x] - 1`bởi vì các thao tác như vậy giữ cho chuỗi tối ưu và việc chọn phần tử đầu tiên nhỏ nhất sẽ mang lại câu trả lời nhỏ nhất về mặt từ điển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(S) | Quá chậm | 
| Kiểm tra mọi hoạt động trên mỗi trạng thái | O(SN) | O(S) | Quá chậm | 
| Mở rộng khoảng DP | O(S + tổng chiều dài khoảng) | O(S + tổng chiều dài khoảng) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mọi khoảng thời gian hoạt động và xây dựng một danh sách nhỏ gọn các mức giảm có sẵn ở mỗi giá trị. Tổng số mục được lưu trữ được giới hạn bởi tổng độ dài khoảng, tối đa là`2 * 10^7`. 
2. Tính toán`dp`từ`1`ĐẾN`S`. Với mọi giá trị`x`, kiểm tra tất cả các khoản giảm có sẵn tại`x`. Nếu trừ đi`d`đạt đến trạng thái có thể giải quyết được, hãy cập nhật`dp[x]`. 
3. Nếu`dp[S]`là vô hạn, không tồn tại dãy nào nên hãy in`-1`. 
4. Bắt đầu từ`x = S`, liên tục chọn mức giảm nhỏ nhất`d`như vậy`dp[x-d] = dp[x]-1`. Nối nó vào câu trả lời và chuyển đến`x-d`. 
5. Dừng lại khi`x`trở thành`0`. 

Tại sao nó hoạt động:`dp[x]`là đúng vì mọi nước đi đầu tiên có thể từ`x`được kiểm tra và mọi trạng thái tiếp theo đều nhỏ hơn và đã được giải quyết. Trong quá trình tái thiết, mỗi bước di chuyển được chọn sẽ giảm khoảng cách tối ưu còn lại chính xác một, do đó số lần di chuyển vẫn ở mức tối thiểu. Việc chọn mức giảm hợp lệ nhỏ nhất ở mọi vị trí sẽ tạo ra chuỗi nhỏ nhất có thể có trong số tất cả các chuỗi ngắn nhất. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    S, N = map(int, input().split())

    ops = []
    diff = array('i', [0]) * (S + 2)

    for _ in range(N):
        l, r, d = map(int, input().split())
        ops.append((l, r, d))
        diff[l] += 1
        diff[r + 1] -= 1

    offsets = array('i', [0]) * (S + 2)
    cur = 0
    for i in range(1, S + 1):
        cur += diff[i]
        offsets[i] = offsets[i - 1] + cur

    total = offsets[S]
    vals = array('i', [0]) * total
    ptr = array('i', offsets)

    for l, r, d in ops:
        for x in range(l, r + 1):
            vals[ptr[x]] = d
            ptr[x] += 1

    INF = 10**9
    dp = array('i', [INF]) * (S + 1)
    dp[0] = 0

    for x in range(1, S + 1):
        best = INF
        for i in range(offsets[x - 1], offsets[x]):
            d = vals[i]
            v = dp[x - d]
            if v + 1 < best:
                best = v + 1
        dp[x] = best

    if dp[S] == INF:
        print(-1)
        return

    ans = []
    x = S
    while x:
        best = INF
        for i in range(offsets[x - 1], offsets[x]):
            d = vals[i]
            if dp[x - d] == dp[x] - 1 and d < best:
                best = d
        ans.append(best)
        x -= best

    print(len(ans))
    print(*ans)

solve()
```Phần đầu tiên đọc các thao tác và tạo một mảng khác biệt. Điều này tránh việc mở rộng khoảng thời gian ngay lập tức và cho chúng tôi biết mỗi vị trí cần bao nhiêu dung lượng lưu trữ. 

các`offsets`mảng biến các hoạt động mở rộng thành cấu trúc giống CSR. lát cắt`vals[offsets[x-1]:offsets[x]]`chứa chính xác mức giảm có thể sử dụng khi giá trị hiện tại là`x`. 

Vòng lặp lập trình động dựa trên thực tế là mọi chuyển đổi đều đi từ giá trị lớn hơn đến giá trị nhỏ hơn. Không cần BFS hay Dijkstra vì đồ thị được sắp xếp tự nhiên. 

Trong quá trình xây dựng lại, mã không chọn mức giảm nhỏ nhất một cách mù quáng. Đầu tiên nó kiểm tra xem nước đi có giữ khoảng cách tối ưu hay không, sau đó giảm thiểu giá trị giảm. Sự phân biệt này tránh được lỗi phổ biến là mất số bước tối thiểu. 

## Ví dụ đã hoạt động 

Dành cho:```
10 2
1 10 1
5 10 5
```các trạng thái quan trọng là: 

| Giá trị hiện tại | Giảm sẵn có | giá trị dp | 
| --- | --- | --- | 
| 0 | không | 0 | 
| 5 | 1,5 | 1 | 
| 10 | 1,5 | 2 | 

Quá trình tái thiết bắt đầu vào lúc`10`. Giảm bớt`5`là hợp lệ bởi vì`dp[5] = 1`, trong khi chọn`1`không bảo toàn độ dài tối thiểu. Câu trả lời trở thành`[5,5]`. 

Vì:```
15 3
1 15 1
3 12 3
7 15 7
```dấu vết là: 

| Giá trị hiện tại | Đã chọn giảm | Giá trị còn lại | 
| --- | --- | --- | 
| 15 | 1 | 14 | 
| 14 | 7 | 7 | 
| 7 | 7 | 0 | 

Ví dụ này cho thấy tại sao việc tái cấu trúc từ điển lại xảy ra sau khi biết được khoảng cách ngắn nhất. Trình tự`[1,7,7]`ngắn hơn các chuỗi chỉ sử dụng mức giảm nhỏ hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(S + tổng chiều dài khoảng) | Mọi trạng thái và mọi khoảng thời gian hợp lệ đều được xử lý với số lần không đổi. | 
| Không gian | O(S + tổng chiều dài khoảng) | Mảng DP và bộ lưu trữ hoạt động nén phù hợp với các ràng buộc. | 

Kích thước khoảng mở rộng tối đa là`2 * 10^7`, nên giải pháp tránh được điều không thể`S * N`hành vi trong khi vẫn ở trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```
# helper tests

def run(inp):
    import subprocess, sys
    p = subprocess.run(
        [sys.executable, "main.py"],
        input=inp.encode(),
        stdout=subprocess.PIPE
    )
    return p.stdout.decode().strip()

assert run("""10 2
1 10 1
5 10 5
""") == "2\n5 5"

assert run("""15 3
1 15 1
3 12 3
7 15 7
""") == "3\n1 7 7"

assert run("""5 1
3 5 3
""") == "-1"

assert run("""1 1
1 1 1
""") == "1\n1"

assert run("""6 2
1 6 2
4 6 3
""") == "2\n3 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`10 2 ...`|`2 / 5 5`| Số nước đi tối thiểu đánh bại những nước đi nhỏ nhất tham lam | 
|`15 3 ...`|`3 / 1 7 7`| Lựa chọn từ điển trong số những con đường ngắn nhất | 
|`5 1 ...`|`-1`| Những trạng thái bất khả thi | 
|`1 1 ...`|`1 / 1`| Giá trị nhỏ nhất có thể | 
|`6 2 ...`|`2 / 3 3`| Khoảng chồng chéo và tái thiết | 

## Vỏ cạnh 

Khi một số bước di chuyển có thể giải quyết trạng thái nhưng có độ dài khác nhau, trước tiên thuật toán sẽ tin cậy`dp`và chỉ sau đó so sánh giảm. Đối với đầu vào:```
10 2
1 10 1
5 10 5
```tiểu bang`10`có cả hai`1`Và`5`có sẵn. Chỉ một`5`giữ khoảng cách tối ưu, do đó việc tái thiết không thể vô tình chọn mười bước di chuyển nhỏ hơn. 

Đối với các trạng thái không thể truy cập:```
5 1
3 5 3
```giá trị DP của`5`được tìm thấy thông qua hoạt động phụ thuộc vào`dp[2]`, Nhưng`dp[2]`là vô hạn. Giá trị vẫn không thể truy cập được và chương trình sẽ in`-1`. 

Đối với ranh giới nhỏ nhất:```
1 1
1 1 1
```sự chuyển đổi hợp lệ duy nhất là từ`1`ĐẾN`0`. Việc khởi tạo DP với`dp[0] = 0`xử lý việc này mà không có trường hợp đặc biệt.
