---
title: "CF 1531D - \u0420\u0435\u0434\u0430\u043a\u0442\u0438\u0440\u0443\u0435\u043c \u0417\u0438\u043d\u0433\u0435\u0440 | màu sắc"
description: "Chúng tôi có một danh sách theo thứ tự thời gian các tin nhắn của bot. Bot giữ hai phần trạng thái. Đầu tiên là liệu thay đổi màu sắc hiện có bị khóa hay không. Thứ hai là màu mái vòm hiện tại, một trong bảy màu cầu vồng. Ban đầu mái vòm có màu xanh lam và những thay đổi về màu sắc được mở khóa."
date: "2026-06-10T16:48:08+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 1531
codeforces_index: "D"
codeforces_contest_name: "VK Cup 2021 - \u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u044f (Engine)"
rating: 0
weight: 1531
solve_time_s: 127
verified: true
draft: false
---

[CF 1531D - \u0420\u0435\u0434\u0430\u043a\u0442\u0438\u0440\u0443\u0435\u043c \u0417\u0438\u043d\u0433\u0435\u0440 | màu](https://codeforces.com/problemset/problem/1531/D) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một danh sách theo thứ tự thời gian các tin nhắn của bot. Bot giữ hai phần trạng thái. 

Đầu tiên là liệu thay đổi màu sắc hiện có bị khóa hay không. 

Thứ hai là màu mái vòm hiện tại, một trong bảy màu cầu vồng. 

Ban đầu mái vòm có màu xanh lam và những thay đổi về màu sắc được mở khóa. 

Mỗi tin nhắn hoạt động theo trạng thái hiện tại. 

MỘT`lock`thông báo chuyển hệ thống sang trạng thái khóa nếu nó chưa bị khóa. 

MỘT`unlock`thông báo chuyển hệ thống sang trạng thái mở khóa nếu nó chưa được mở khóa. 

Thông báo màu chỉ thay đổi màu vòm khi hệ thống hiện được mở khóa. Nếu hệ thống bị khóa, thông báo màu sẽ bị bỏ qua. 

Sau lịch sử ban đầu, chúng tôi nhận được tới$10^5$chỉnh sửa. Mỗi chỉnh sửa sẽ thay thế một tin nhắn trong lịch sử. Sau mỗi lần chỉnh sửa, toàn bộ lịch sử phải được xử lý lại từ trạng thái ban đầu và chúng tôi phải xuất ra màu vòm thu được. 

Những hạn chế là thách thức thực sự. Cả số lượng tin nhắn và số lần chỉnh sửa đều có thể đạt tới$10^5$. Việc phát lại toàn bộ lịch sử sau mỗi lần chỉnh sửa sẽ yêu cầu khoảng$10^{10}$ứng dụng tin nhắn trong trường hợp xấu nhất, vượt xa những gì phù hợp với giới hạn thời gian của cuộc thi. 

Quan sát quan trọng là không gian trạng thái của bot rất nhỏ. Chỉ có bảy màu và hai trạng thái khóa, vì vậy chỉ có mười bốn trạng thái có thể tồn tại. 

Trường hợp cạnh tinh tế xuất hiện khi thông báo màu được thực thi trong khi bị khóa. 

Ví dụ:```
lock
red
unlock
```Màu cuối cùng vẫn là màu xanh. các`red`lệnh bị bỏ qua vì hệ thống đã bị khóa khi nó đến. 

Một sai lầm dễ mắc phải khác là điều trị`lock`Và`unlock`như những nhiệm vụ vô điều kiện. 

Ví dụ:```
lock
lock
unlock
```thứ hai`lock`không làm gì cả Hành vi này vẫn được xác định rõ ràng vì các tin nhắn hoạt động ở trạng thái hiện tại chứ không phải trên chính lịch sử tin nhắn. 

Trường hợp thứ ba là chỉnh sửa một tin nhắn cũ. 

Ví dụ:```
1: red
2: lock
3: green
```Kết quả ban đầu là`red`. 

Nếu tin nhắn 2 được sửa thành`unlock`, thì tin nhắn 3 đột nhiên có hiệu lực và câu trả lời trở thành`green`. 

Bất kỳ giải pháp nào cố gắng chỉ cập nhật màu hậu tố mà không xem xét lại cấu trúc chuyển đổi trạng thái sẽ không thành công. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Hãy coi danh sách tin nhắn như một chương trình. Bắt đầu từ trạng thái ban đầu, xử lý tất cả$n$tin nhắn và có được màu sắc cuối cùng. Sau mỗi lần chỉnh sửa, hãy sửa đổi thông báo và chạy lại toàn bộ mô phỏng. 

Điều này hoạt động vì các quy tắc của bot mang tính quyết định. Vấn đề là chi phí. Mỗi truy vấn cần$O(n)$làm việc, và có$10^5$truy vấn. Tổng độ phức tạp trở thành$O(nt)$, đạt tới$10^{10}$hoạt động. 

Quan sát quan trọng là mỗi thông điệp thực sự là một hàm trên tập hợp mười bốn trạng thái có thể có. 

Ví dụ,`lock`ánh xạ mọi trạng thái đã mở khóa sang trạng thái khóa tương ứng và giữ nguyên trạng thái bị khóa. 

Một lệnh màu như`red`ánh xạ mọi trạng thái đã mở khóa sang trạng thái màu đỏ đã mở khóa và giữ nguyên mọi trạng thái bị khóa. 

Vì mỗi thông báo là một chức năng chuyển trạng thái nên toàn bộ phân đoạn thông báo chỉ đơn giản là sự kết hợp các chức năng của chúng. 

Điều này ngay lập tức gợi ý một cây phân đoạn. 

Đối với mỗi nút, chúng tôi lưu trữ chức năng được đại diện bởi phân khúc đó. Vì không gian trạng thái chỉ chứa 14 trạng thái nên một hàm có thể được lưu trữ dưới dạng một mảng có độ dài 14 trong đó:`f[s]`là trạng thái kết quả sau khi áp dụng phân đoạn vào trạng thái`s`. 

Khi kết hợp hai phân đoạn con, chúng ta soạn các hàm của chúng. Nếu đoạn bên trái đại diện`L`và đoạn bên phải đại diện`R`, thì cha mẹ đại diện`R ∘ L`, vì các thông báo từ nửa bên trái sẽ thực thi trước các thông báo từ nửa bên phải. 

Một cập nhật điểm sẽ thay đổi một thông báo, vì vậy chỉ$O(\log n)$các nút cây phân đoạn phải được tính toán lại. Mỗi lần tính toán lại bao gồm hai hàm có kích thước mười bốn, là công việc không đổi. 

Câu trả lời có được bằng cách áp dụng hàm gốc cho trạng thái ban đầu`(unlocked, blue)`và trích xuất màu kết quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nt)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(14 \cdot (n + t \log n))$|$O(14n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Mã hóa mọi trạng thái bot có thể có dưới dạng số nguyên từ 0 đến 13. 
2. Biểu diễn một trạng thái dưới dạng`(locked, color)`. 
3. Đối với mọi loại thông báo có thể, hãy tính toán trước hàm chuyển đổi của nó trên tất cả 14 trạng thái. 
4. Xây dựng cây phân đoạn trong đó mỗi lá lưu trữ chức năng tương ứng với một thông báo. 
5. Đối với một nút bên trong, hãy soạn các chức năng của các nút con của nó. Nếu như`left`Và`right`là các hàm con thì:`parent[s] = right[left[s]]`bởi vì đoạn bên trái thực thi đầu tiên. 
6. Để có được câu trả lời hiện tại, hãy áp dụng hàm gốc cho trạng thái ban đầu`(unlocked, blue)`. 
7. Trích xuất thành phần màu của trạng thái thu được và in tên của nó. 
8. Đối với thao tác chỉnh sửa, hãy thay thế hàm lá tương ứng. 
9. Tính toán lại tất cả các tổ tiên đến gốc bằng cách sử dụng cùng một quy tắc hợp thành. 
10. Truy vấn lại gốc và xuất ra màu kết quả. 

### Tại sao nó hoạt động 

Mỗi thông báo xác định một sự biến đổi xác định của máy tự động mười bốn trạng thái. Thành phần chức năng khớp chính xác với việc thực hiện tuần tự các tin nhắn. Nút cây phân đoạn lưu trữ phép biến đổi được tạo ra bởi toàn bộ khoảng của nó. Bằng quy nạp trên cấu trúc cây, mỗi nút thể hiện chính xác tác động của việc xử lý tất cả các thông báo trong phân đoạn của nó. 

Khoảng thời gian gốc bao trùm toàn bộ lịch sử, vì vậy chức năng của nó chính xác là tác dụng phát lại chuỗi thông báo hoàn chỉnh từ trạng thái ban đầu. Sau khi cập nhật điểm, chỉ các phân đoạn chứa vị trí đó mới thay đổi và việc tính toán lại các thành phần sẽ khôi phục chuyển đổi chính xác cho mọi nút bị ảnh hưởng. Do đó, mọi màu được báo cáo đều khớp với kết quả của lần phát lại đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

colors = ["red", "orange", "yellow", "green", "blue", "indigo", "violet"]
color_id = {c: i for i, c in enumerate(colors)}

STATE_COUNT = 14

def state_id(locked, color):
    return locked * 7 + color

# Precompute transition function for every possible message.
funcs = {}

for msg in ["lock", "unlock"] + colors:
    f = [0] * STATE_COUNT

    for locked in range(2):
        for color in range(7):
            s = state_id(locked, color)

            if msg == "lock":
                ns = state_id(1, color) if locked == 0 else s
            elif msg == "unlock":
                ns = state_id(0, color) if locked == 1 else s
            else:
                if locked == 0:
                    ns = state_id(0, color_id[msg])
                else:
                    ns = s

            f[s] = ns

    funcs[msg] = f

def compose(left, right):
    # right ∘ left
    return [right[left[i]] for i in range(STATE_COUNT)]

n = int(input())
messages = [input().strip() for _ in range(n)]

size = 1
while size < n:
    size <<= 1

identity = list(range(STATE_COUNT))
seg = [identity[:] for _ in range(2 * size)]

for i in range(n):
    seg[size + i] = funcs[messages[i]]

for i in range(size - 1, 0, -1):
    seg[i] = compose(seg[i * 2], seg[i * 2 + 1])

initial_state = state_id(0, color_id["blue"])

def answer():
    final_state = seg[1][initial_state]
    color = final_state % 7
    return colors[color]

out = [answer()]

t = int(input())

for _ in range(t):
    pos, msg = input().split()
    pos = int(pos) - 1

    p = size + pos
    seg[p] = funcs[msg]

    p //= 2
    while p:
        seg[p] = compose(seg[p * 2], seg[p * 2 + 1])
        p //= 2

    out.append(answer())

sys.stdout.write("\n".join(out))
```Mã hóa trạng thái là chi tiết triển khai quan trọng nhất. Mỗi trạng thái phải thể hiện duy nhất cả cờ khóa và màu hiện tại. sử dụng`locked * 7 + color`đưa ra một phạm vi nhỏ gọn từ 0 đến 13. 

Mỗi tin nhắn được chuyển đổi thành bảng chuyển tiếp có độ dài 14. Điều này biến vấn đề từ mô phỏng tin nhắn thành chức năng soạn thảo. 

Cây phân đoạn lưu trữ các hàm hoàn chỉnh thay vì các giá trị vô hướng. Thứ tự thành phần quan trọng. Nếu đoạn bên trái thực thi trước đoạn bên phải thì đoạn gốc phải biểu diễn`right[left[s]]`. Đảo ngược thứ tự này sẽ tạo ra câu trả lời sai. 

Chức năng nhận dạng được sử dụng cho các lá không sử dụng. Vì việc soạn thảo với danh tính không thay đổi gì nên việc đệm cây trở nên an toàn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Lịch sử ban đầu: 

| Bước | Tin nhắn | Đã khóa | Màu sắc | 
| --- | --- | --- | --- | 
| Bắt đầu | - | Không | màu xanh | 
| 1 | đỏ | Không | đỏ | 
| 2 | màu tím | Không | màu tím | 
| 3 | mở khóa | Không | màu tím | 
| 4 | đỏ | Không | đỏ | 
| 5 | cam | Không | cam | 
| 6 | khóa | Có | cam | 
| 7 | chàm | Có | cam | 

Câu trả lời cuối cùng là`orange`. 

cuối cùng`indigo`bị bỏ qua vì hệ thống đã bị khóa. 

Sau khi chỉnh sửa tin nhắn 5 tới`green`, sự phát lại tương tự tạo ra`green`, phù hợp với đầu ra mẫu. 

### Ví dụ tùy chỉnh```
3
lock
red
unlock
```| Bước | Tin nhắn | Đã khóa | Màu sắc | 
| --- | --- | --- | --- | 
| Bắt đầu | - | Không | màu xanh | 
| 1 | khóa | Có | màu xanh | 
| 2 | đỏ | Có | màu xanh | 
| 3 | mở khóa | Không | màu xanh | 

Kết quả vẫn còn`blue`. 

Ví dụ này cho thấy tại sao các lệnh màu không thể được coi là phép gán vô điều kiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(14 \cdot (n + t \log n))$| Tòa nhà yêu cầu thành phần chức năng, mỗi bản cập nhật chạm vào$O(\log n)$nút | 
| Không gian |$O(14n)$| Mỗi nút cây phân đoạn lưu trữ một hàm có kích thước 14 | 

Vì 14 là một hằng số cố định nên độ phức tạp thực tế về cơ bản là$O(n + t \log n)$, có thể xử lý thoải mái$10^5$tin nhắn và$10^5$chỉnh sửa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    colors = ["red", "orange", "yellow", "green", "blue", "indigo", "violet"]
    color_id = {c: i for i, c in enumerate(colors)}

    STATE_COUNT = 14

    def state_id(locked, color):
        return locked * 7 + color

    funcs = {}

    for msg in ["lock", "unlock"] + colors:
        f = [0] * STATE_COUNT
        for locked in range(2):
            for color in range(7):
                s = state_id(locked, color)

                if msg == "lock":
                    ns = state_id(1, color) if locked == 0 else s
                elif msg == "unlock":
                    ns = state_id(0, color) if locked == 1 else s
                else:
                    ns = state_id(0, color_id[msg]) if locked == 0 else s

                f[s] = ns
        funcs[msg] = f

    def compose(a, b):
        return [b[a[i]] for i in range(STATE_COUNT)]

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    msgs = [input().strip() for _ in range(n)]

    size = 1
    while size < n:
        size <<= 1

    identity = list(range(STATE_COUNT))
    seg = [identity[:] for _ in range(2 * size)]

    for i in range(n):
        seg[size + i] = funcs[msgs[i]]

    for i in range(size - 1, 0, -1):
        seg[i] = compose(seg[i * 2], seg[i * 2 + 1])

    initial = state_id(0, color_id["blue"])

    def ans():
        s = seg[1][initial]
        return colors[s % 7]

    out = [ans()]

    t = int(input())
    for _ in range(t):
        p, msg = input().split()
        p = int(p) - 1

        idx = size + p
        seg[idx] = funcs[msg]

        idx //= 2
        while idx:
            seg[idx] = compose(seg[idx * 2], seg[idx * 2 + 1])
            idx //= 2

        out.append(ans())

    return "\n".join(out)

# provided sample
assert run(
"""7
red
violet
unlock
red
orange
lock
indigo
6
5 green
6 lock
6 yellow
4 lock
1 lock
5 unlock
"""
) == """orange
green
green
indigo
violet
blue
indigo"""

# minimum size
assert run(
"""1
blue
1
1 red
"""
) == """blue
red"""

# locked color ignored
assert run(
"""2
lock
red
1
2 green
"""
) == """blue
blue"""

# unlock enables later color
assert run(
"""3
lock
red
unlock
1
2 green
"""
) == """blue
blue"""

# edit old message changes future behavior
assert run(
"""3
red
lock
green
1
2 unlock
"""
) == """red
green"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một tin nhắn đã được chỉnh sửa | xanh → đỏ | Kích thước tối thiểu | 
|`lock`rồi tô màu | màu xanh | Bỏ qua màu khi bị khóa | 
|`lock`, màu sắc,`unlock`| màu xanh | Mở khóa không phát lại màu cũ | 
| Biên tập`lock`vào trong`unlock`| đỏ → xanh | Các chỉnh sửa lịch sử ảnh hưởng đến các tin nhắn sau này | 

## Vỏ cạnh 

Hãy xem xét:```
2
lock
red
```Hệ thống sẽ bị khóa trước khi lệnh màu đến. Hàm chuyển tiếp cho`red`giữ nguyên mọi trạng thái khóa, vì vậy màu cuối cùng vẫn là màu xanh lam. Thành phần cây phân đoạn bảo tồn hành vi này một cách tự nhiên. 

Coi như:```
3
lock
lock
unlock
```thứ hai`lock`ánh xạ trạng thái bị khóa vào chính nó. Hàm chuyển tiếp của nó là bình thường. Soạn nó với cái đầu tiên`lock`tạo ra hiệu ứng chính xác tương tự như một thao tác khóa đơn. 

Coi như:```
3
red
lock
green
```Câu trả lời là`red`. Nếu tin nhắn 2 được sửa thành`unlock`, thì tin nhắn thứ ba sẽ có hiệu lực và câu trả lời sẽ thay đổi thành`green`. Bản cập nhật chỉ thay đổi một lá, nhưng các bố cục được tính toán lại sẽ tự động truyền hiệu ứng trong toàn bộ lịch sử.
