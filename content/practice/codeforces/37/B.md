---
title: "CF 37B - Trò chơi máy tính"
description: "Sếp bắt đầu với lượng máu tối đa và hồi phục lượng máu quy định mỗi giây. Chúng tôi sở hữu một số cuộn giấy. Mỗi cuộn chỉ có thể được kích hoạt khi sức khỏe của ông chủ tối đa bằng một phần trăm lượng máu ban đầu."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 37
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 37"
rating: 1800
weight: 37
solve_time_s: 116
verified: true
draft: false
---
[CF 37B - Trò chơi trên máy tính](https://codeforces.com/problemset/problem/37/B) 

**Đánh giá:** 1800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ông chủ bắt đầu với`max`sức khỏe và tái sinh`reg`sức khỏe mỗi giây. Chúng tôi sở hữu một số cuộn giấy. Mỗi cuộn chỉ có thể được kích hoạt khi sức khỏe của ông chủ tối đa bằng một phần trăm lượng máu ban đầu. Sau khi được kích hoạt, cuộn giấy đó sẽ vĩnh viễn cộng sát thương mỗi giây vào tổng lượng sát thương gây ra của chúng tôi. 

Dòng thời gian của một giây là vô cùng quan trọng: 

1. Các phép thuật đang hoạt động hiện tại sẽ gây sát thương. 
2. Ông chủ tái sinh. 
3. Chúng tôi có thể kích hoạt tối đa một cuộn giấy mới. 
4. Nếu bây giờ sức khỏe của ông chủ là`<= 0`, cuộc chiến kết thúc. 

Điều kiện kích hoạt phụ thuộc vào sức khỏe của trùm sau khi gây sát thương và hồi phục trong giây đó. 

Nhiệm vụ là quyết định xem có thể đánh bại được tên trùm hay không. Nếu có, chúng tôi phải xuất ra một chuỗi kích hoạt cuộn hợp lệ để đạt được thời gian hoàn thành tối thiểu có thể. 

Các ràng buộc đủ nhỏ để cho phép thực hiện phép tính bậc hai hoặc bậc ba. Có nhiều nhất là 1000 cuộn giấy. Giá trị thiệt hại cũng nhỏ. Điều này ngay lập tức gợi ý rằng chúng ta nên tập trung nhiều hơn vào việc khám phá chiến lược tham lam chính xác hơn là các cấu trúc dữ liệu nâng cao. 

Một điểm tinh tế là cuộn giấy không gây sát thương một lần. Mỗi cuộn sẽ tăng vĩnh viễn tỷ lệ sát thương trong tương lai. Kích hoạt cuộn mạnh hơn sớm hơn có thể cho phép truy cập các ngưỡng trong tương lai nhanh hơn. 

Một lỗi dễ mắc phải khác đến từ thứ tự các hành động trong một giây. Giả sử sức khỏe của ông chủ trở nên đủ thấp sau khi tái sinh. Chúng tôi có thể kích hoạt một cuộn giấy mới trong giây đó, nhưng cuộn giấy mới được kích hoạt đó không gây sát thương ngay lập tức. Thiệt hại của nó bắt đầu vào giây tiếp theo. 

Hãy xem xét ví dụ này:```
1 10 0
50 100
```Ông chủ bắt đầu ở mức 10 HP. Cuộn giấy yêu cầu lượng máu tối đa là 5. Vì hiện tại không có nguồn sát thương nào nên lượng máu không bao giờ giảm xuống dưới 10. Câu trả lời đúng là`NO`. 

Việc thực hiện bất cẩn có thể cho rằng chúng ta có thể kích hoạt cuộn giấy ngay lập tức một cách sai lầm vì thiệt hại của nó rất lớn. 

Một trường hợp cạnh nguy hiểm khác xuất hiện khi DPS hiện tại không lớn hơn khả năng tái tạo. Giả định:```
2 100 10
100 5
90 100
```Chúng ta có thể kích hoạt cuộn đầu tiên ngay lập tức vì ngưỡng là 100%. Sau đó, tổng DPS chỉ là 5, nhỏ hơn mức hồi phục 10. Boss không bao giờ giảm xuống dưới 100 HP nên cuộn giấy thứ hai không bao giờ có thể sử dụng được. Câu trả lời đúng là`NO`. 

Một mô phỏng đơn giản chỉ kiểm tra xem tổng thiệt hại cuối cùng có vượt quá khả năng tái tạo hay không sẽ tuyên bố chiến thắng một cách không chính xác. 

Một trường hợp tinh tế hơn xuất phát từ đẳng thức ngưỡng chính xác.```
1 100 0
99 1
```Cuộn giấy chỉ có thể được sử dụng khi sức khỏe tối đa là 99. Vì ban đầu không có phép thuật nào được kích hoạt nên sức khỏe không bao giờ thay đổi từ 100. Sự bình đẳng rất quan trọng vì một khi ông chủ đạt đến chính xác ngưỡng, cuộn giấy sẽ trở thành hợp pháp ngay sau khi tái sinh. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi thứ tự kích hoạt cuộn có thể có. Đối với mỗi hoán vị, chúng tôi mô phỏng cuộc chiến từng giây một và kiểm tra xem liệu các ngưỡng có thể đạt được hay không. 

Điều này là đắt tiền một cách vô vọng. Với 1000 cuộn giấy, thậm chí việc khám phá một phần nhỏ của tất cả các đơn hàng là không thể. Khó khăn cốt lõi là quyết định cuộn nào sẽ kích hoạt tiếp theo. 

Quan sát quan trọng là chỉ có một điều quan trọng đối với các cuộn đã chọn trước đó: tổng DPS hiện tại. 

Giả sử tổng DPS hiện tại của chúng tôi là`D`. Trong mỗi giây, ông chủ sẽ thua một cách hiệu quả`D - reg`sức khỏe, ngoại trừ sức khỏe đó bị giới hạn ở trên`max`. 

Nếu như`D <= reg`, sức khỏe của ông chủ không bao giờ giảm. Không có ngưỡng tương lai nào nhỏ hơn sức khỏe hiện tại có thể đạt được. 

Nếu như`D > reg`, sức khỏe của ông chủ giảm dần. Cuối cùng, mọi ngưỡng đều có thể đạt được. 

Điều này biến vấn đề thành một câu hỏi lập kế hoạch. 

Đối với một cuộn có phần trăm ngưỡng`pow[i]`, xác định giới hạn sức khỏe kích hoạt của nó:```
limit[i] = floor(max * pow[i] / 100)
```Một khi sức khỏe hiện tại trở nên tối đa`limit[i]`, cuộn đó sẽ có sẵn vĩnh viễn. 

Bây giờ đến cái nhìn sâu sắc tham lam. 

Bất cứ khi nào đã có sẵn một số cuộn, việc kích hoạt cuộn có sát thương lớn nhất trước tiên luôn là tối ưu. Thiệt hại sớm hơn sẽ góp phần vào tất cả các giây trong tương lai, vì vậy việc trì hoãn DPS lớn hơn không bao giờ có thể giúp ích. 

Điều này mang lại một quá trình tự nhiên: 

Chúng tôi liên tục hạ thấp sức khỏe của ông chủ bằng cách sử dụng DPS hiện tại. Bất cứ khi nào có cuộn giấy mới, chúng tôi sẽ tham lam kích hoạt cuộn giấy mạnh nhất hiện có. 

Toàn bộ cuộc chiến trở nên quyết định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N!) | O(N) | Quá chậm | 
| Tối ưu | O(N2 log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi từng ngưỡng phần trăm thành giới hạn sức khỏe tuyệt đối. 

Để cuộn`i`, tính:```
limit[i] = floor(max_hp * pow[i] / 100)
```Đây là lượng máu tối đa của trùm mà cuộn giấy có thể được kích hoạt. 
2. Sắp xếp tất cả các cuộn theo giới hạn giảm dần. 

Khi sức khỏe của ông chủ giảm đi, sẽ có nhiều cuộn giấy hơn. Ngưỡng xử lý theo thứ tự giảm dần phù hợp với diễn biến thực tế của cuộc chiến. 
3. Duy trì ba điều trong quá trình mô phỏng:`cur_hp`, sức khỏe của ông chủ sau khi được phục hồi ở giây hiện tại.`dps`, tổng sát thương chủ động mỗi giây. 

Hàng đợi ưu tiên chứa tất cả các cuộn hiện có nhưng chưa được sử dụng, được sắp xếp theo mức độ hư hỏng. 
4. Ban đầu sức khỏe của boss chính xác là`max`. 

Mỗi cuộn với`limit >= max`có thể sử dụng ngay lập tức tại thời điểm 0. Đẩy chúng vào đống. 
5. Nếu heap trống khi bắt đầu, chúng ta không bao giờ có thể bắt đầu gây sát thương cho trùm. 

đầu ra`NO`. 
6. Kích hoạt liên tục cuộn giấy mạnh nhất hiện có. 

Mỗi kích hoạt xảy ra tại thời điểm hiện tại. 

Thêm thiệt hại của nó vào`dps`. 

Ghi lại thao tác kích hoạt. 
7. Sau khi cập nhật DPS, hãy kiểm tra xem`dps > reg`. 

Nếu không, sức khỏe của ông chủ không thể giảm thêm nữa. Cuộc chiến trở nên bế tắc mãi mãi. 

Tiếp tục kích hoạt các cuộn đã có sẵn nếu có. Nếu cuối cùng không còn cuộn nào có sẵn và`dps <= reg`, đầu ra`NO`. 
8. Khi nào`dps > reg`, ông chủ thua cuộc`dps - reg`sức khỏe hiệu quả mỗi giây. 

Chúng ta có thể tính toán trực tiếp cần bao nhiêu giây để đạt đến ngưỡng tiếp theo thay vì mô phỏng từng giây. 

Giả sử cuộn không có sẵn tiếp theo yêu cầu sức khỏe`next_limit`. 

Chúng ta cần cái nhỏ nhất`t`như vậy:```
cur_hp - t * (dps - reg) <= next_limit
```Giải quyết vấn đề này bằng cách chia trần. 
9. Nâng cao thời gian bằng cách`t`. 

Cập nhật sức khỏe của ông chủ cho phù hợp. 

Mỗi cuộn có giới hạn hiện có thể truy cập được sẽ được chèn vào vùng nhớ heap. 
10. Ở mọi giai đoạn, hãy kiểm tra xem trùm có thể bị giết trước khi đạt đến ngưỡng khác hay không. 

Nếu sức khoẻ hiện tại`cur_hp`và thiệt hại ròng mỗi giây là`delta = dps - reg`, thì ông chủ chết sau:```
ceil(cur_hp / delta)
```giây bổ sung. 

Nếu điều này xảy ra trước bất kỳ sự kiện ngưỡng nào trong tương lai, chúng tôi sẽ kết thúc ngay lập tức. 

### Tại sao nó hoạt động 

Bất biến quan trọng là bất cứ khi nào chúng ta chọn một cuộn, tất cả các cuộn hiện có đều có thể được chọn thay thế. Chọn kẻ có sát thương tối đa luôn là tối ưu vì sát thương sẽ tích lũy vĩnh viễn trong những giây sau. 

Giả sử hai cuộn có sẵn có thiệt hại`a < b`. Nếu chúng ta kích hoạt`a`đầu tiên và`b`sau đó, việc hoán đổi thứ tự của chúng sẽ làm tăng tổng sát thương tích lũy trong mỗi giây trung gian. Ngưỡng sức khỏe chỉ có thể đạt được sớm hơn, không bao giờ muộn hơn. 

Do đối số trao đổi này, chiến lược tối ưu được đặc trưng duy nhất bằng cách liên tục lấy cuộn giấy mạnh nhất hiện có. 

Sự tiến triển về sức khỏe giữa các sự kiện ngưỡng là tuyến tính, vì vậy chúng ta có thể chuyển trực tiếp giữa các thời điểm quan trọng thay vì mô phỏng từng giây. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, max_hp, reg = map(int, input().split())

    scrolls = []

    for i in range(n):
        p, d = map(int, input().split())
        limit = max_hp * p // 100
        scrolls.append((limit, d, i + 1))

    scrolls.sort(reverse=True)

    ptr = 0
    heap = []

    cur_hp = max_hp
    dps = 0
    time = 0

    ans = []

    while ptr < n and scrolls[ptr][0] >= cur_hp:
        limit, dmg, idx = scrolls[ptr]
        heapq.heappush(heap, (-dmg, idx, limit))
        ptr += 1

    while True:
        while heap:
            dmg_neg, idx, limit = heapq.heappop(heap)
            dmg = -dmg_neg

            dps += dmg
            ans.append((time, idx))

        if dps <= reg:
            print("NO")
            return

        delta = dps - reg

        kill_time = (cur_hp + delta - 1) // delta

        next_limit = -1
        if ptr < n:
            next_limit = scrolls[ptr][0]

        if next_limit == -1:
            time += kill_time
            print("YES")
            print(time, len(ans))
            for t, idx in ans:
                print(t, idx)
            return

        if cur_hp <= next_limit:
            need = 0
        else:
            need = (cur_hp - next_limit + delta - 1) // delta

        if kill_time <= need:
            time += kill_time
            print("YES")
            print(time, len(ans))
            for t, idx in ans:
                print(t, idx)
            return

        time += need
        cur_hp -= need * delta

        while ptr < n and scrolls[ptr][0] >= cur_hp:
            limit, dmg, idx = scrolls[ptr]
            heapq.heappush(heap, (-dmg, idx, limit))
            ptr += 1

solve()
```Phần đầu tiên chuyển đổi ngưỡng phần trăm thành giới hạn sức khỏe tuyệt đối. Sử dụng phép chia số nguyên là đúng vì điều kiện dựa trên các giá trị sức khỏe số nguyên thực tế. 

Các cuộn giấy được sắp xếp theo giới hạn giảm dần để chúng ta có thể mở khóa dần dần khi sức khỏe của trùm giảm dần. 

Heap lưu trữ các cuộn cuộn hiện có sẵn được sắp xếp theo mức độ hư hỏng. của Python`heapq`là một đống tối thiểu, do đó thiệt hại được chèn dưới dạng giá trị âm. 

Vòng lặp bên trong kích hoạt mọi cuộn hiện có ngay lập tức. Việc trì hoãn bất kỳ cuộn giấy nào có sẵn không bao giờ có lợi vì thiệt hại của nó sẽ bắt đầu gây ra sau đó. 

điều kiện`dps <= reg`là bộ phát hiện bế tắc. Khi đó sức khỏe của trùm không thể giảm được nữa. Nếu không có cuộn giấy nào mạnh hơn thì cuộc chiến sẽ không thể thắng được. 

Công thức:```
(cur_hp - next_limit + delta - 1) // delta
```là sự chia trần. Nó tính toán giây đầu tiên khi sức khỏe trở nên đủ thấp để đạt ngưỡng tiếp theo. 

Một chi tiết triển khai tinh tế là các cuộn mới được kích hoạt không gây sát thương trong cùng một giây. Mô phỏng tôn trọng điều này một cách tự nhiên vì thời gian trôi qua trước khi các cuộn mới được mở khóa được kích hoạt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 10 3
100 3
99 1
```Ngưỡng chuyển đổi: 

| Cuộn | Giới hạn | Thiệt hại | 
| --- | --- | --- | 
| 1 | 10 | 3 | 
| 2 | 9 | 1 | 

Trạng thái ban đầu: 

| Thời gian | HP | DPS | Có sẵn | 
| --- | --- | --- | --- | 
| 0 | 10 | 0 | {1} | 

Kích hoạt cuộn 1: 

| Thời gian | HP | DPS | 
| --- | --- | --- | 
| 0 | 10 | 3 | 

Hiện nay`dps = reg = 3`. 

Sát thương hiệu quả bằng 0, vì vậy máu của trùm không bao giờ giảm xuống dưới 10. Cuộn 2 không bao giờ có sẵn. 

Đầu ra đúng:```
NO
```Ví dụ này chứng minh tại sao chỉ kiểm tra tổng DPS cuối cùng là không đủ. Cuộc chiến có thể bị đình trệ vĩnh viễn trước khi đạt được các ngưỡng trong tương lai. 

### Ví dụ 2 

đầu vào:```
3 100 10
100 15
80 20
50 100
```Ngưỡng: 

| Cuộn | Giới hạn | Thiệt hại | 
| --- | --- | --- | 
| 1 | 100 | 15 | 
| 2 | 80 | 20 | 
| 3 | 50 | 100 | 

Mô phỏng: 

| Thời gian | HP | DPS | Hành động | 
| --- | --- | --- | --- | 
| 0 | 100 | 0 | Cuộn 1 có sẵn | 
| 0 | 100 | 15 | Kích hoạt 1 | 
| 4 | 80 | 15 | Cuộn 2 đã được mở khóa | 
| 4 | 80 | 35 | Kích hoạt 2 | 
| 5 | 55 | 35 | Tiếp tục | 
| 6 | 30 | 35 | Cuộn 3 đã được mở khóa | 
| 6 | 30 | 135 | Kích hoạt 3 | 
| 7 | -95 | 135 | Ông chủ chết | 

Dấu vết cho thấy cách thuật toán nhảy trực tiếp giữa các sự kiện ngưỡng thay vì mô phỏng từng giây riêng lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Sắp xếp cộng với việc chèn/xóa vùng heap | 
| Không gian | O(N) | Di chuyển lưu trữ và hàng đợi ưu tiên | 

Mỗi cuộn được chèn vào heap đúng một lần và bị xóa đúng một lần. Với`N <= 1000`, điều này dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from heapq import heappush, heappop

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out

    input = sys.stdin.readline

    n, max_hp, reg = map(int, input().split())

    scrolls = []

    for i in range(n):
        p, d = map(int, input().split())
        limit = max_hp * p // 100
        scrolls.append((limit, d, i + 1))

    scrolls.sort(reverse=True)

    ptr = 0
    heap = []

    cur_hp = max_hp
    dps = 0
    time = 0

    ans = []

    while ptr < n and scrolls[ptr][0] >= cur_hp:
        limit, dmg, idx = scrolls[ptr]
        heappush(heap, (-dmg, idx, limit))
        ptr += 1

    while True:
        while heap:
            dmg_neg, idx, limit = heappop(heap)
            dmg = -dmg_neg

            dps += dmg
            ans.append((time, idx))

        if dps <= reg:
            print("NO")
            break

        delta = dps - reg

        kill_time = (cur_hp + delta - 1) // delta

        next_limit = -1
        if ptr < n:
            next_limit = scrolls[ptr][0]

        if next_limit == -1:
            time += kill_time
            print("YES")
            print(time, len(ans))
            for t, idx in ans:
                print(t, idx)
            break

        if cur_hp <= next_limit:
            need = 0
        else:
            need = (cur_hp - next_limit + delta - 1) // delta

        if kill_time <= need:
            time += kill_time
            print("YES")
            print(time, len(ans))
            for t, idx in ans:
                print(t, idx)
            break

        time += need
        cur_hp -= need * delta

        while ptr < n and scrolls[ptr][0] >= cur_hp:
            limit, dmg, idx = scrolls[ptr]
            heappush(heap, (-dmg, idx, limit))
            ptr += 1

    sys.stdout = old_stdout
    return out.getvalue()

# provided sample
assert run(
"""2 10 3
100 3
99 1
"""
).strip() == "NO", "sample 1"

# minimum size winning case
assert run(
"""1 1 0
100 1
"""
).startswith("YES"), "single scroll immediate win"

# impossible because initial threshold unreachable
assert run(
"""1 100 0
99 100
"""
).strip() == "NO", "cannot start fight"

# exact equality threshold
assert run(
"""2 10 0
100 1
90 100
"""
).startswith("YES"), "unlock exactly at threshold"

# large regeneration deadlock
assert run(
"""2 100 50
100 40
0 100
"""
).strip() == "NO", "health never decreases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cuộn đơn với ngưỡng 100% | CÓ | Phiên bản hợp lệ tối thiểu | 
| Ngưỡng 99% không có thiệt hại ban đầu | KHÔNG | Không thể mở khóa cuộn đầu tiên | 
| Mở khóa bình đẳng chính xác | CÓ | Xử lý ranh giới cho các ngưỡng | 
| DPS dưới mức tái sinh | KHÔNG | Phát hiện bế tắc | 

## Vỏ cạnh 

Hãy xem xét trường hợp ban đầu không có cuộn giấy nào.```
1 100 0
99 100
```Ngưỡng là 99 HP, nhưng trùm bắt đầu ở mức 100 HP và không có nguồn sát thương nào tồn tại. Heap trống ngay lập tức. Thuật toán phát hiện điều này trước bất kỳ mô phỏng và đầu ra nào`NO`. 

Bây giờ hãy kiểm tra sự bế tắc tái sinh:```
2 100 10
100 5
90 100
```Tại thời điểm 0, chỉ có cuộn đầu tiên. Kích hoạt nó sẽ mang lại DPS 5. Vì`5 <= 10`, thiệt hại hiệu quả là không tích cực. Máu không bao giờ giảm xuống dưới 100, vì vậy cuộn thứ hai không bao giờ có thể mở khóa. Thuật toán kiểm tra`dps <= reg`và đầu ra chính xác`NO`. 

Cuối cùng, hãy xem xét chuyển đổi ngưỡng chính xác:```
2 10 0
100 1
90 100
```Sau khi kích hoạt cuộn giấy đầu tiên, sát thương hiệu dụng là 1. Sau một giây, trùm đạt chính xác 9 HP. Vì cuộn thứ hai có giới hạn là 9 nên nó sẽ có sẵn ngay lập tức. Thuật toán sử dụng mức phân chia trần một cách cẩn thận để sự bình đẳng được coi là có thể truy cập được thay vì bỏ qua một giây.
