---
title: "CF 105909K - UNO\uff01"
description: "Chúng ta có một bàn tròn có n người chơi. Người chơi i bắt đầu bằng thẻ a[i]. Trò chơi bắt đầu với người chơi 1 và hướng ban đầu là theo chiều kim đồng hồ. Một chuỗi chính xác m thẻ đã chơi được đưa ra."
date: "2026-06-25T14:08:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105909
codeforces_index: "K"
codeforces_contest_name: "The 9th Hebei Collegiate Programming Contest"
rating: 0
weight: 105909
solve_time_s: 62
verified: true
draft: false
---

[CF 105909K - UNO\uff01](https://codeforces.com/problemset/problem/105909/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cái bàn tròn với`n`người chơi. Người chơi`i`bắt đầu bằng`a[i]`thẻ. Trò chơi bắt đầu với người chơi 1 và hướng ban đầu là theo chiều kim đồng hồ. 

Một trình tự chính xác`m`thẻ chơi được đưa ra. Mỗi nhân vật mô tả lá bài được chơi ở lượt đó:`C`là một lá bài bình thường và không có tác dụng gì.`S`bỏ qua người chơi tiếp theo theo hướng hiện tại.`R`đảo ngược hướng chơi.`D`đưa cho người chơi tiếp theo hai lá bài bổ sung và cũng bỏ qua người chơi đó. 

Bất cứ khi nào người chơi chơi bài, kích thước bài của họ sẽ giảm đi một. Nếu ván bài của họ đạt đến số 0 ngay sau khi chơi, họ sẽ rời khỏi trò chơi ngay lập tức và bị loại khỏi vòng kết nối. 

Đầu vào đảm bảo rằng trình tự chơi hợp lệ và ít nhất hai người chơi vẫn còn bài sau lần chơi cuối cùng. Chúng ta phải xuất ra số lượng quân bài cuối cùng mà mỗi người chơi ban đầu nắm giữ. 

Điều thú vị là người chơi biến mất trong quá trình mô phỏng. Sau khi ai đó rời đi, thứ tự lượt trong tương lai, bỏ qua, đảo ngược và rút hai phải hoạt động trên danh sách vòng tròn còn lại của những người chơi đang hoạt động. 

Giới hạn lên đến`2 × 10^5`người chơi và`2 × 10^5`chơi bài. Một mô phỏng liên tục quét vòng tròn để tìm người chơi đang hoạt động tiếp theo có thể giảm xuống thời gian bậc hai. Với 200.000 phép tính, chúng ta cần mỗi lượt được xử lý theo thời gian không đổi hoặc logarit. 

Trường hợp khó khăn tinh tế xuất hiện khi người chơi rời đi ngay sau khi chơi một lá bài đặc biệt. 

Ví dụ:```
3 players
cards = [1, 5, 5]
play = "S"
```Người chơi 1 chơi`S`, số thẻ của họ trở thành 0 và họ rời đi. Người chơi bị bỏ qua vẫn là người chơi 2, vì "người chơi tiếp theo" được xác định từ vị trí của người chơi 1 trước khi họ biến mất. Việc thực hiện bất cẩn loại bỏ người chơi 1 trước rồi tìm kiếm người chơi tiếp theo có thể nhắm mục tiêu sai người. 

Một trường hợp phức tạp khác là quân bài đảo ngược được chơi bởi một người chơi rời đi.```
3 players
cards = [1, 5, 5]
play = "R"
```Người chơi 1 rời đi ngay sau khi chơi. Hướng thay đổi và lượt tiếp theo thuộc về người chơi ở phía đối diện của người chơi 1 theo hướng mới. Nếu chúng ta cố gắng tiếp tục từ trình phát đã bị loại bỏ mà không bảo toàn các trình phát lân cận của nó thì mô phỏng sẽ bị hỏng. 

Nguồn lỗi cuối cùng là việc kết hợp loại bỏ với bỏ qua.```
3 players
cards = [1, 5, 5]
play = "D"
```Người chơi 1 đưa cho người chơi 2 hai lá bài, người chơi 2 thua lượt, người chơi 1 bỏ đi và lượt tiếp theo trở thành người chơi 3. Thứ tự của những cập nhật này rất quan trọng. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp nhất lưu trữ tất cả những người chơi đang hoạt động trong một vòng tròn và sau mỗi thẻ, sẽ duyệt qua những người chơi để tìm người hành động tiếp theo. Điều này đúng vì luật chơi mang tính cục bộ, mỗi lượt chỉ ảnh hưởng đến những người chơi ở gần theo hướng hiện tại. 

Vấn đề là hiệu quả. Một người chơi có thể rời khỏi trò chơi, điều đó có nghĩa là chúng tôi cần xóa họ khỏi vòng kết nối. Nếu chúng tôi đại diện cho những người chơi đang hoạt động bằng một mảng hoặc danh sách bình thường, việc loại bỏ một người nào đó có thể tốn kém`O(n)`. Làm điều đó lên đến`m = 2 × 10^5`lần dẫn đến khoảng`O(nm)`làm việc, vượt xa giới hạn. 

Điều quan trọng là trò chơi chỉ cần ba thao tác đối với những người chơi đang hoạt động: 

1. Tìm người chơi hoạt động tiếp theo. 
2. Tìm người chơi hoạt động trước đó. 
3. Xóa người chơi khỏi vòng kết nối. 

Đây chính xác là các hoạt động được hỗ trợ bởi danh sách liên kết đôi vòng tròn trong`O(1)`thời gian. 

Chúng tôi lưu trữ cho mọi người chơi hàng xóm hiện tại theo chiều kim đồng hồ và hàng xóm ngược chiều kim đồng hồ. Việc xóa một trình phát sẽ trở thành một mối nối danh sách liên kết tiêu chuẩn. Sau đó, tất cả các hiệu ứng có thể được xử lý chỉ bằng các con trỏ lân cận. 

Sau đó, mỗi lá bài được chơi chỉ yêu cầu một số lượng cập nhật con trỏ và cập nhật số tay không đổi, tạo ra độ phức tạp tổng thể tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Tối ưu | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ số thẻ của mỗi người chơi trong một mảng. 
2. Xây dựng danh sách liên kết đôi vòng bằng cách sử dụng hai mảng:`nxt[i]`là hàng xóm theo chiều kim đồng hồ của người chơi`i`,`prv[i]`là hàng xóm ngược chiều kim đồng hồ của người chơi`i`. 
3. Hãy để`cur = 1`, vì người chơi 1 đi trước. 
4. Hãy để`dir = 1`đại diện theo chiều kim đồng hồ và`dir = -1`biểu thị ngược chiều kim đồng hồ. 
5. Đối với mỗi ký tự trong chuỗi phát: 

1. Ghi lại hai người hàng xóm của người chơi hiện tại trước khi loại bỏ:`left = prv[cur]`,`right = nxt[cur]`. 
2. Nếu thẻ bị`S`hoặc`D`, xác định người chơi bị ảnh hưởng:`target = right`khi`dir = 1`,`target = left`khi`dir = -1`. 
3. Nếu thẻ bị`D`, thêm hai thẻ vào`target`. 
4. Giảm`cards[cur]`bởi một vì thẻ được chơi. 
5. Nếu`cards[cur]`trở thành số 0, loại bỏ`cur`từ danh sách liên kết. 
6. Xử lý hiệu ứng thẻ. 

Vì`C`, người chơi tiếp theo chỉ đơn giản là`target`. 

Vì`S`Và`D`, người chơi tiếp theo là người chơi sau`target`theo hướng hiện tại. 

Vì`R`, đảo ngược hướng. Người chơi tiếp theo trở thành hàng xóm của vị trí hiện tại theo hướng mới. 
6. Tiếp tục cho đến hết`m`thẻ đã được xử lý. 
7. Xuất ra số lá bài cuối cùng của mỗi người chơi. 

### Tại sao nó hoạt động 

Danh sách liên kết luôn thể hiện chính xác những người chơi còn bài. Khi số lượng người chơi đạt đến 0, họ sẽ bị loại bỏ ngay lập tức, phù hợp với quy tắc. 

Đối với mỗi lượt, thuật toán xác định những người chơi bị ảnh hưởng bằng cách sử dụng vòng tròn hiện tại và hướng hiện tại trước khi chọn bất kỳ lượt nào trong tương lai. Điều này phù hợp với định nghĩa trò chơi của "người chơi tiếp theo". 

Điều bất biến là trước khi xử lý từng thẻ,`cur`chính xác là người chơi đến lượt và danh sách liên kết chứa chính xác những người chơi đang hoạt động theo đúng thứ tự vòng tròn. Mọi bản cập nhật đều giữ nguyên tính bất biến này, vì vậy sau lá bài cuối cùng, số lượng lá bài được lưu trữ là kích thước ván bài cuối cùng thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
a = list(map(int, input().split()))
s = input().strip()

nxt = [0] * n
prv = [0] * n

for i in range(n):
    nxt[i] = (i + 1) % n
    prv[i] = (i - 1 + n) % n

cur = 0
dirv = 1

for ch in s:
    left = prv[cur]
    right = nxt[cur]

    target = right if dirv == 1 else left

    if ch == 'D':
        a[target] += 2

    a[cur] -= 1

    dead = (a[cur] == 0)

    if dead:
        l = prv[cur]
        r = nxt[cur]
        nxt[l] = r
        prv[r] = l

    if ch == 'C':
        cur = target

    elif ch == 'S' or ch == 'D':
        if dirv == 1:
            cur = nxt[target]
        else:
            cur = prv[target]

    else:  # 'R'
        dirv *= -1

        if dirv == 1:
            cur = right
        else:
            cur = left

for x in a:
    print(x)
```Các mảng`nxt`Và`prv`tạo thành một danh sách liên kết đôi vòng tròn trên những người chơi đang hoạt động. Việc xóa một trình phát chỉ yêu cầu kết nối lại hai người chơi lân cận của nó, tức là thời gian không đổi. 

Biến`target`được tính toán trước khi loại bỏ. Điều này rất cần thiết vì bỏ qua và rút thăm hai đề cập đến người chơi tiếp theo so với người chơi vừa hành động, không liên quan đến vòng tròn sau khi cập nhật sau. 

Đối với thẻ ngược, chúng tôi lưu trữ`left`Và`right`trước khi loại bỏ. Nếu người chơi hiện tại rời đi, chúng tôi vẫn biết người chơi nào sẽ hành động tiếp theo sau khi chuyển hướng. 

Số lượng thẻ không bao giờ vượt quá khoảng`2 × 10^5 + 2m`, dễ dàng khớp với các số nguyên Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 6
3 2 3
SRDCCD
```Dấu vết: 

| Xoay | Hiện tại | Thẻ | Hướng | Thẻ Sau Lượt | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | S | CW | [2,2,3] | 
| 2 | 3 | R | CCW | [2,2,2] | 
| 3 | 2 | D | CCW | [4,1,2] | 
| 4 | 3 | C | CCW | [4,1,1] | 
| 5 | 2 | C | CCW | [4,0,1] | 
| 6 | 1 | D | CCW | [3,0,3] | 

Đầu ra:```
3
0
3
```Ví dụ này thể hiện cả ba cơ chế đặc biệt: bỏ qua, đảo ngược và đưa thêm thẻ. 

### Ví dụ 2 

đầu vào:```
4 3
1 2 2 2
RSC
```Dấu vết: 

| Xoay | Hiện tại | Thẻ | Hướng Sau Khi Rẽ | Người chơi tích cực | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | R | CCW | 2,3,4 | 
| 2 | 4 | S | CCW | 2,3 | 
| 3 | 2 | C | CCW | 2,3 | 

Tính toán cuối cùng: 

| Người chơi | Thẻ | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 2 | 
| 4 | 1 | 

Ví dụ này thực hiện trường hợp người chơi bỏ đi ngay sau khi đánh một lá bài ngược. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Xây dựng danh sách liên kết là O(n), mỗi lá bài đã chơi được xử lý trong O(1) | 
| Không gian | O(n) | Số lượng thẻ và mảng danh sách liên kết | 

Giới hạn cho phép lên tới 200.000 người chơi và 200.000 thẻ chơi. Mô phỏng tuyến tính dễ dàng phù hợp với giới hạn thời gian và chỉ sử dụng một vài mảng kích thước`n`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    s = input().strip()

    nxt = [0] * n
    prv = [0] * n

    for i in range(n):
        nxt[i] = (i + 1) % n
        prv[i] = (i - 1 + n) % n

    cur = 0
    dirv = 1

    for ch in s:
        left = prv[cur]
        right = nxt[cur]

        target = right if dirv == 1 else left

        if ch == 'D':
            a[target] += 2

        a[cur] -= 1

        if a[cur] == 0:
            l = prv[cur]
            r = nxt[cur]
            nxt[l] = r
            prv[r] = l

        if ch == 'C':
            cur = target
        elif ch in "SD":
            cur = nxt[target] if dirv == 1 else prv[target]
        else:
            dirv *= -1
            cur = right if dirv == 1 else left

    return "\n".join(map(str, a)) + "\n"

# sample
assert run("3 6\n3 2 3\nSRDCCD\n") == "3\n0\n3\n"

# minimum style case
assert run("2 2\n1 2\nCC\n") == "0\n1\n"

# reverse with elimination
assert run("3 1\n1 5 5\nR\n") == "0\n5\n5\n"

# draw-two with elimination
assert run("3 1\n1 5 5\nD\n") == "0\n7\n5\n"

# all normal cards
assert run("4 4\n3 3 3 3\nCCCC\n") == "2\n2\n2\n2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 / 1 2 / CC`|`0 1`| Vòng tròn hoạt động nhỏ nhất | 
|`3 1 / 1 5 5 / R`|`0 5 5`| Đảo ngược khi người chơi hiện tại rời đi | 
|`3 1 / 1 5 5 / D`|`0 7 5`| Hòa hai và bỏ qua bằng cách loại trừ | 
|`4 4 / 3 3 3 3 / CCCC`|`2 2 2 2`| Xoay vòng thuần túy | 

## Vỏ cạnh 

Hãy xem xét:```
3 1
1 5 5
S
```Người chơi 1 chơi bài bỏ qua và ngay lập tức đạt 0 quân bài. Người chơi bị bỏ qua vẫn là người chơi 2, vì người chơi 2 là người chơi hoạt động tiếp theo tại thời điểm lá bài được đánh. Thuật toán tính toán`target`trước khi loại bỏ, do đó người chơi chính xác sẽ bị bỏ qua. Số lượng cuối cùng trở thành:```
0
5
5
```Bây giờ hãy xem xét:```
3 1
1 5 5
R
```Người chơi 1 rời đi sau khi chơi ngược lại. Hướng thay đổi từ theo chiều kim đồng hồ sang ngược chiều kim đồng hồ. Người chơi tiếp theo phải là hàng xóm bên trái cũ của người chơi 1. Thuật toán lưu trữ`left`Và`right`trước khi loại bỏ, do đó nó có thể chọn lượt tiếp theo một cách chính xác ngay cả sau khi người chơi 1 biến mất. 

Cuối cùng:```
3 1
1 5 5
D
```Người chơi 1 đưa cho người chơi 2 hai lá bài, bỏ qua người chơi 2 rồi bỏ đi. Người chơi 3 trở thành người chơi tiếp theo. Bởi vì thuật toán áp dụng hiệu ứng hòa hai trước khi loại bỏ và tính toán người chơi bị bỏ qua trực tiếp từ vị trí hiện tại nên kết quả chính xác là:```
0
7
5
```Những trường hợp này là khi hầu hết các mô phỏng không chính xác đều thất bại và chúng được xử lý một cách tự nhiên bằng cách biểu diễn danh sách liên kết.
