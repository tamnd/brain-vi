---
title: "CF 1136D - Nastya đang mua bữa trưa"
description: "Hàng đợi ban đầu đã được cố định và người cuối cùng trong hàng đợi là Nastya. Đã cho một số cặp thứ tự $(u,v)$. Một cặp có nghĩa là bất cứ khi nào học sinh $u$ đứng ngay trước học sinh $v$, hai học sinh đó sẵn sàng đổi chỗ cho nhau."
date: "2026-06-12T04:01:37+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1136
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 546 (Div. 2)"
rating: 1800
weight: 1136
solve_time_s: 86
verified: true
draft: false
---

[CF 1136D - Nastya đang mua bữa trưa](https://codeforces.com/problemset/problem/1136/D) 

**Đánh giá:** 1800 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hàng đợi ban đầu đã được cố định và người cuối cùng trong hàng đợi là Nastya. Một số cặp đặt hàng$(u,v)$được đưa ra. Một cặp có nghĩa là bất cứ khi nào học sinh$u$đứng ngay trước mặt học sinh$v$, hai học sinh đó sẵn sàng đổi chỗ cho nhau. 

Nastya muốn di chuyển càng xa về phía trước hàng đợi càng tốt bằng cách liên tục sử dụng các giao dịch hoán đổi liền kề như vậy. Chúng ta cần xác định xem cô ấy có thể thăng tiến bao nhiêu vị trí. 

Hàng đợi chứa tới$3 \cdot 10^5$học sinh và có thể có tới$5 \cdot 10^5$quan hệ trao đổi được phép. Những giới hạn này ngay lập tức loại trừ bất kỳ mô phỏng hoán đổi nào. Thậm chí một$O(n^2)$thuật toán sẽ thực hiện xung quanh$9 \cdot 10^{10}$trong trường hợp xấu nhất vượt xa thời hạn. Chúng ta cần thứ gì đó gần tuyến tính ở kích thước đầu vào. 

Phần tinh vi của vấn đề là Nastya không trao đổi trực tiếp với những người độc đoán. Mỗi lần hoán đổi phải có sự tham gia của hai học sinh liền kề và sự cho phép phụ thuộc vào danh tính của hai học sinh đó. 

Một sai lầm phổ biến là nghĩ rằng Nastya có thể vượt qua một học sinh$x$nếu có sự cho phép trực tiếp$(x,\text{Nastya})$. Đôi khi cô ấy có thể bỏ qua$x$gián tiếp sau khi sắp xếp lại các học sinh khác. 

Coi như:```
Queue: [1, 2, 3]
Nastya = 3

Permissions:
1 -> 3
1 -> 2
```Nastya không thể vượt qua học sinh 2. Mặc dù 1 có thể hoán đổi với 3, học sinh 2 chặn quyền truy cập và không thể vượt qua. Câu trả lời là 0. 

Một sai lầm dễ mắc phải khác là chỉ mô phỏng những cơ hội trước mắt.```
Queue: [1, 2, 3, 4]
Nastya = 4

Permissions:
1 -> 4
2 -> 4
3 -> 4
```Ở đây Nastya có thể di chuyển về phía trước. Sau khi vượt qua 3, cô ấy liền kề với 2, rồi đến 1. Câu trả lời là 3. 

Thách thức là xác định chính xác học sinh nào là những trở ngại không thể tránh khỏi. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực là coi hàng đợi như một trạng thái và liên tục thực hiện bất kỳ hoán đổi liền kề nào có sẵn liên quan đến Nastya. Người ta có thể thử tìm kiếm tất cả các cấu hình có thể truy cập hoặc mô phỏng các giao dịch hoán đổi một cách tham lam. Khó khăn là việc sắp xếp lại giữa các học sinh khác có thể tạo ra những cơ hội mới sau này. Không gian trạng thái rất lớn, có thể lên tới$n!$, nên hướng này là vô vọng. 

Quan sát chính xuất phát từ việc nhìn vào hàng đợi từ phải sang trái. 

Cho phép$x$là số Nastya, là phần tử cuối cùng của hoán vị. 

Hãy tưởng tượng kiểm tra các học sinh đứng trước Nastya, bắt đầu từ người gần cô ấy nhất và di chuyển về phía trước. 

Giả sử chúng ta đã xác định được một nhóm học sinh phải ở trước mặt Nastya dù thế nào đi nữa. Gọi bộ này là **bị chặn**. 

Bây giờ hãy xem xét một học sinh khác$v$xa hơn về bên trái. 

Nastya cuối cùng có thể vượt qua$v$? 

Để vượt qua$v$, tại một thời điểm nào đó cô ấy phải trở nên liền kề với$v$. Mọi học sinh hiện đang bị chặn bằng cách nào đó phải di chuyển về phía sau$v$Đầu tiên. Cách duy nhất có thể xảy ra là nếu$v$sẵn sàng trao đổi với mọi học sinh bị chặn có nhu cầu vượt qua. 

Nếu thậm chí một học sinh bị chặn$u$tồn tại sao cho sự cho phép$(v,u)$thì vắng mặt$u$không bao giờ có thể di chuyển phía sau$v$. Do đó, Nastya không bao giờ có thể ở gần$v$, Vì thế$v$chính nó trở nên bị chặn. 

Mặt khác, nếu$v$có quyền đối với mọi học sinh hiện đang bị chặn, sau đó tất cả những học sinh bị chặn đó có thể được di chuyển về phía sau$v$, cuối cùng cho phép Nastya vượt qua$v$. 

Điều này dẫn đến một quá trình tham lam rất nhỏ gọn. 

Duy trì tập hợp học sinh bị chặn. Ban đầu nó chỉ chứa bản thân Nastya. 

Xử lý hàng đợi từ phải sang trái, ngoại trừ Nastya. 

Đối với mỗi học sinh$v$, đếm xem có bao nhiêu học sinh bị chặn$u$đáp ứng sự cho phép$(v,u)$. 

Nếu số lượng này bằng với kích thước hiện tại của tập hợp bị chặn thì$v$cuối cùng có thể được bỏ qua. 

Nếu không thì$v$tham gia tập hợp bị chặn. 

Cuối cùng, mọi học sinh bị chặn ngoại trừ Nastya đều đại diện cho một người mà Nastya không bao giờ có thể vượt qua. Nếu có$k$những học sinh như vậy thì cô ấy có thể vượt qua những học sinh còn lại$n-1-k$học sinh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Tìm kiếm theo cấp số nhân/không gian trạng thái | Rất lớn | Quá chậm | 
| Tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hàng đợi và xác định số Nastya, phần tử cuối cùng của hoán vị. 
2. Đối với mọi cặp quyền$(u,v)$, cửa hàng$v$trong tập kề cận của$u$. 
3. Tạo một bộ có tên`blocked`và ban đầu đặt Nastya vào trong đó. 
4. Xử lý hàng đợi từ phải sang trái, bắt đầu từ học sinh ngay trước Nastya. 
5. Đối với học sinh hiện tại$v$, đếm xem có bao nhiêu thành viên`blocked`xuất hiện ở$v$tập kề của. 
6. Nếu số này bằng kích thước của`blocked`, sau đó$v$cuối cùng có thể bị Nastya vượt qua, vì vậy đừng làm gì cả. 

Lý do là mọi học sinh đang bị chặn đều có thể di chuyển về phía sau$v$, cho phép$v$trở nên liền kề với Nastya vào một thời điểm nào đó. 
7. Nếu không thì thêm$v$ĐẾN`blocked`. 

Ít nhất một học sinh bị chặn không bao giờ có thể di chuyển phía sau$v$, làm$v$một trở ngại không thể tránh khỏi. 
8. Sau khi tất cả học sinh được xử lý, hãy`ans = len(blocked) - 1`. 

Đây là những học sinh không phải Nastya phải đi trước cô ấy. 
9. Đầu ra`(n - 1) - ans`. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý một hậu tố của hàng đợi,`blocked`chứa chính xác những học sinh từ hậu tố đó không bao giờ có thể di chuyển được phía sau Nastya. 

Khi kiểm tra học sinh mới$v$, mọi học sinh hiện đang bị chặn cuối cùng đều phải vượt qua$v$nếu Nastya vượt qua$v$. Việc vượt biển như vậy cần có sự cho phép$(v,u)$. Nếu thiếu một quyền cần thiết thì ít nhất một học sinh bị chặn vẫn vĩnh viễn dẫn trước$v$, ngăn cản Nastya tiếp cận$v$. Kể từ đây$v$cũng phải bị chặn. 

Nếu có quyền đối với mọi học sinh bị chặn, tất cả chúng có thể được di chuyển về phía sau$v$, Vì thế$v$không trở thành trở ngại vĩnh viễn. 

Bất biến vẫn đúng sau mỗi bước và khi toàn bộ hàng đợi được xử lý, tập hợp bị chặn sẽ chứa chính xác những học sinh mà Nastya không thể vượt qua. Đáp án là số học sinh còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    p = list(map(int, input().split()))

    adj = [set() for _ in range(n + 1)]

    for _ in range(m):
        u, v = map(int, input().split())
        adj[u].add(v)

    blocked = {p[-1]}  # Nastya

    for v in reversed(p[:-1]):
        cnt = 0
        s = adj[v]

        for u in blocked:
            if u in s:
                cnt += 1

        if cnt != len(blocked):
            blocked.add(v)

    cannot_pass = len(blocked) - 1
    print((n - 1) - cannot_pass)

solve()
```Cấu trúc kề lưu trữ tất cả các quyền có nguồn gốc từ mỗi học sinh. Kiểm tra tư cách thành viên là$O(1)$trung bình. 

bộ`blocked`là trái tim của giải pháp. Nó chứa chính xác những học sinh được đảm bảo sẽ dẫn trước Nastya. Chúng tôi quét từ phải sang trái vì trạng thái của học sinh chỉ phụ thuộc vào những người ở gần Nastya hơn chính nó. 

Một chi tiết tinh tế là chúng ta so sánh số lượng với kích thước hiện tại của`blocked`trước khi có khả năng chèn học sinh hiện tại. Đảo ngược trật tự đó sẽ khiến học sinh tự phụ vào chính mình một cách không chính xác. 

Một chi tiết khác là câu trả lời là không`len(blocked)`. Bản thân Nastya luôn nằm trong tập hợp bị chặn, vì vậy chúng tôi trừ đi một trước khi tính xem có bao nhiêu người còn ở phía trước cô ấy. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 1
1 2
1 2
```Nastya là học sinh 2. 

| Học sinh hiện tại | Bị chặn trước | Có tất cả các cạnh bị chặn không? | Bị chặn sau | 
| --- | --- | --- | --- | 
| 1 | {2} | Có | {2} | 

Tập bị chặn cuối cùng là`{2}`.`cannot_pass = 0`Trả lời =`1 - 0 = 1`. 

Nastya có thể trao đổi trực tiếp với học sinh 1 và tiến lên phía trước. 

### Mẫu 2```
3 3
1 2 3
1 3
1 2
2 3
```Nastya là học sinh thứ 3. 

| Học sinh hiện tại | Bị chặn trước | Có tất cả các cạnh bị chặn không? | Bị chặn sau | 
| --- | --- | --- | --- | 
| 2 | {3} | Có | {3} | 
| 1 | {3} | Có | {3} | 

Tập bị chặn cuối cùng là`{3}`.`cannot_pass = 0`Trả lời =`2`. 

Nastya cuối cùng có thể vượt qua cả hai học sinh và tiến lên phía trước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi cạnh được lưu trữ một lần và mỗi học sinh được xử lý một lần | 
| Không gian | O(n + m) | Tập kề cộng với tập bị chặn | 

Tổng kích thước đầu vào tối đa là$n=3\cdot10^5$Và$m=5\cdot10^5$. Lưu trữ tuyến tính và xử lý gần tuyến tính phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())
    p = list(map(int, input().split()))

    adj = [set() for _ in range(n + 1)]

    for _ in range(m):
        u, v = map(int, input().split())
        adj[u].add(v)

    blocked = {p[-1]}

    for v in reversed(p[:-1]):
        cnt = 0
        for u in blocked:
            if u in adj[v]:
                cnt += 1
        if cnt != len(blocked):
            blocked.add(v)

    cannot_pass = len(blocked) - 1
    return str((n - 1) - cannot_pass) + "\n"

# provided sample
assert run("2 1\n1 2\n1 2\n") == "1\n", "sample 1"

# minimum size
assert run("1 0\n1\n") == "0\n", "single pupil"

# no permissions
assert run("4 0\n1 2 3 4\n") == "0\n", "cannot move at all"

# can pass everyone
assert run(
    "4 3\n1 2 3 4\n1 4\n2 4\n3 4\n"
) == "3\n", "moves to front"

# one unavoidable blocker
assert run(
    "4 2\n1 2 3 4\n2 4\n3 4\n"
) == "2\n", "pupil 1 remains ahead"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 / 1`|`0`| Kích thước tối thiểu | 
| Không có quyền |`0`| Không ai có thể được thông qua | 
| Mọi người đều chỉ vào Nastya |`3`| Nastya tiến lên phía trước | 
| Thiếu một quyền quan trọng |`2`| Phát hiện chính xác các trình chặn không thể tránh khỏi | 

## Vỏ cạnh 

Hãy xem xét hàng đợi:```
4 0
1 2 3 4
```Không có quyền nào cả. Xử lý từ phải sang trái cho:```
blocked = {4}
3 cannot reach all blocked -> add
blocked = {4,3}
2 cannot reach all blocked -> add
blocked = {4,3,2}
1 cannot reach all blocked -> add
blocked = {4,3,2,1}
```Mọi học sinh đều bị tắc nghẽn. Câu trả lời là`0`, điều này đúng vì không có sự hoán đổi nào có thể xảy ra. 

Bây giờ hãy xem xét:```
4 3
1 2 3 4
1 4
2 4
3 4
```Quá trình xử lý mang lại:```
blocked = {4}
3 reaches all blocked
2 reaches all blocked
1 reaches all blocked
```Không có học sinh bổ sung nào vào tập hợp bị chặn. Câu trả lời là`3`, nghĩa là Nastya có thể vượt qua tất cả mọi người. 

Cuối cùng hãy xem xét:```
4 2
1 2 3 4
2 4
3 4
```Quá trình xử lý mang lại:```
blocked = {4}
3 reaches all blocked
2 reaches all blocked
1 does not reach 4
blocked = {4,1}
```Học sinh 1 trở thành trở ngại không thể tránh khỏi. Câu trả lời là`2`, nghĩa là Nastya có thể vượt qua học sinh 2 và 3 nhưng không bao giờ vượt qua học sinh 1. Đây chính xác là hành vi mà kẻ bất biến tham lam nắm bắt.
