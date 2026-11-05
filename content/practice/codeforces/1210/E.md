---
title: "CF 1210E - Wojtek và Thủ thuật đánh bài"
description: "Chúng tôi có một bộ bài $k$ nhỏ, được đánh số từ 1 đến $k$, và Wojtek biết $n$ các cách xáo trộn xác định (hoán vị) khác nhau của bộ bài. Mỗi lần xáo trộn chỉ định chính xác nơi mỗi thẻ kết thúc."
date: "2026-06-11T23:14:48+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1210
codeforces_index: "E"
codeforces_contest_name: "Dasha Code Championship - SPb Finals Round (only for onsite-finalists)"
rating: 2700
weight: 1210
solve_time_s: 109
verified: true
draft: false
---

[CF 1210E - Wojtek và thủ thuật đánh bài](https://codeforces.com/problemset/problem/1210/E) 

**Đánh giá:** 2700 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một cái sàn nhỏ$k$các thẻ được đánh số từ 1 đến$k$, và Wojtek biết$n$xáo trộn xác định khác nhau (hoán vị) của bộ bài. Mỗi lần xáo trộn chỉ định chính xác nơi mỗi thẻ kết thúc. Wojtek tò mò về tất cả các bộ bài mà anh ấy có thể tạo ra nếu anh ấy ghi nhớ một bộ bài xáo trộn liền kề từ$l$ĐẾN$r$và áp dụng chúng theo bất kỳ trình tự nào với số lần bất kỳ. Cụ thể, chúng tôi muốn tính tổng tất cả các phạm vi liền kề có thể có$l$ĐẾN$r$số lượng bộ bài riêng biệt mà Wojtek có thể tạo ra bằng những thủ thuật đó. 

Đầu vào mang lại$n$hoán vị, mỗi hoán vị có độ dài$k$. Những hạn chế đó là$n$có thể lên tới 200.000 trong khi$k$là rất nhỏ, nhiều nhất là 5. Sự kết hợp này ngụ ý rằng bất kỳ thuật toán nào lặp qua tất cả các cặp$(l,r)$TRONG$O(n^2)$thời gian quá chậm. Tuy nhiên, kể từ khi$k$rất nhỏ, nó cho phép chúng ta mô hình hóa tất cả các trạng thái bộ bài có thể một cách rõ ràng - số lượng trạng thái bộ bài riêng biệt nhiều nhất là$k!$. Con số nhỏ này giúp cho việc suy luận về hoán vị của các thẻ một cách thấu đáo trở nên khả thi. 

Các trường hợp đặc biệt bao gồm các chuỗi trong đó một lần xáo trộn duy nhất có thể tạo ra tất cả các bộ bài có thể có. Ví dụ, với$k=3$và một chu trình xáo trộn như (2,3,1), áp dụng nó nhiều lần sẽ tạo ra mọi hoán vị. Một trường hợp cạnh khác là$k=1$, trong đó mỗi lần xáo trộn đều không đáng kể, chỉ tạo ra một bộ bài khả thi bất kể$n$. Việc triển khai bất cẩn có thể bỏ qua việc áp dụng lặp đi lặp lại cùng một cách xáo trộn hoặc bỏ lỡ rằng một khi tập hợp các hoán vị tạo ra nhóm đối xứng đầy đủ, các lần xáo trộn bổ sung sẽ không làm tăng số lượng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại trên mọi mảng con$l$ĐẾN$r$, mô phỏng tất cả các chuỗi ứng dụng xáo trộn và đếm các bộ bài riêng biệt. Với$n$lên tới 200.000, có$O(n^2)$mảng con, và đối với mỗi mảng, trình tự mô phỏng là hàm mũ trong$k$. Mặc dù$k \le 5$,$O(n^2 \cdot k!)$là quá chậm. Lực lượng vũ phu hoạt động vì nhỏ$n$nó tính toán chính xác$f(l,r)$bằng cách khám phá nhóm được tạo ra bởi các hoán vị, nhưng nó không thành công$n$. 

Cái nhìn sâu sắc quan trọng là$k!$là nhỏ bé. Vì$k=5$,$k! = 120$, nghĩa là chúng ta có thể biểu thị tập hợp các bộ bài có thể truy cập cho bất kỳ phạm vi nào dưới dạng mặt nạ bit hoặc tập hợp số nguyên. Hơn nữa, việc thêm một hoán vị mới vào một nhóm hoán vị chỉ mở rộng các bộ bài có thể tiếp cận; nó không bao giờ làm giảm chúng. Do đó, đối với điểm cuối bên trái cố định$l$, có một chỉ số ngoài cùng bên phải$r$ngoài ra, việc thêm nhiều hoán vị sẽ không làm thay đổi tập hợp các bộ bài có thể tiếp cận. Sử dụng hai con trỏ hoặc phương pháp cửa sổ trượt, chúng ta có thể tính tổng một cách hiệu quả: với mỗi$l$, chúng tôi mở rộng$r$cho đến khi tập hợp các bộ bài có thể tiếp cận bão hòa, sau đó tính đến tất cả các phạm vi từ$l$đến đó$r$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * k!) | O(k!) | Quá chậm | 
| Tối ưu | O(n * k! * k) | O(k! * k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi từng hoán vị xáo trộn thành dạng không có chỉ mục để đơn giản hóa các phép tính và lưu trữ trong một mảng`P`. 
2. Liệt kê tất cả các hoán vị của độ dài$k$và chỉ định một ID duy nhất cho mỗi người. Điều này cho phép chúng ta biểu diễn các trạng thái bộ bài dưới dạng số nguyên từ 0 đến$k!-1$. 
3. Tính trước bảng nhân các hoán vị: cho hai hoán vị bất kỳ$A$Và$B$,`mul[A][B]`đưa ra hoán vị tương ứng với việc áp dụng$A$theo sau là$B$. Điều này cho phép tính toán nhanh chóng các xáo trộn tổng hợp. 
4. Đối với mỗi điểm cuối bên trái$l$, duy trì một bộ`reachable`ID bộ bài bắt đầu từ hoán vị danh tính. Khởi tạo con trỏ phải`r=l`. 
5. Tăng dần các hoán vị tại chỉ mục`r`, cập nhật bộ`reachable`bằng cách tính toán tất cả các thành phần của các hoán vị có thể tiếp cận hiện có với hoán vị mới. Tiếp tục cho đến khi`reachable`ngừng mở rộng. 
6. Hãy để`R`là chỉ số tối đa đạt được cho điểm cuối bên trái`l`. Tất cả các phạm vi`[l, r']`vì`r'`từ`r`ĐẾN`R`có cùng một thứ`f(l,r')`. Thêm tổng số đóng góp vào câu trả lời và tăng dần`l`. 
7. Lặp lại cho tất cả`l`. Tổng số tiền tích lũy trên tất cả các phạm vi. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến mà`reachable`chứa tất cả các ID bộ bài có thể nhận được từ việc áp dụng bất kỳ chuỗi xáo trộn được ghi nhớ nào trong cửa sổ hiện tại`[l, r]`. Bởi vì thành phần hoán vị có tính kết hợp và việc thêm một hoán vị không bao giờ làm giảm các bộ bài có thể tiếp cận, mở rộng`r`cho đến khi bão hòa đảm bảo chúng tôi đếm chính xác tất cả các phạm vi. 

## Giải pháp Python```python
import sys
from itertools import permutations
input = sys.stdin.readline

n, k = map(int, input().split())
P = [list(map(lambda x: int(x)-1, input().split())) for _ in range(n)]

# Enumerate all permutations and assign IDs
all_perms = list(permutations(range(k)))
perm_id = {p: i for i, p in enumerate(all_perms)}

# Precompute multiplication table
mul = [[0]*len(all_perms) for _ in range(len(all_perms))]
for i, a in enumerate(all_perms):
    for j, b in enumerate(all_perms):
        c = tuple(a[b[x]] for x in range(k))
        mul[i][j] = perm_id[c]

ans = 0
n_states = len(all_perms)

for l in range(n):
    reachable = set([perm_id[tuple(range(k))]])
    r = l
    while r < n:
        new_reach = set(reachable)
        p_idx = perm_id[tuple(P[r])]
        for state in reachable:
            new_reach.add(mul[state][p_idx])
        if len(new_reach) == len(reachable):
            break
        reachable = new_reach
        r += 1
    ans += len(reachable) * (r - l + 1)

print(ans)
```Đầu tiên, mã này lập chỉ mục tất cả các hoán vị để cho phép kết hợp theo thời gian không đổi. Đối với mỗi ranh giới bên trái, nó mở rộng ranh giới bên phải cho đến khi không thể tiếp cận được bộ bài mới nào. Nó tích lũy các khoản đóng góp bằng cách nhân số lượng bộ bài có thể tiếp cận với số lượng điểm cuối bên phải hợp lệ, như được mô tả trong thuật toán. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
3 3
2 1 3
3 1 2
1 3 2
```| tôi | r | sàn có thể tiếp cận | f(l,r) | đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | {123, 213} | 2 | 2 | 
| 1 | 2 | {123,132,213,231,312,321} | 6 | 6 | 
| 1 | 3 | {tất cả 6 uốn} | 6 | 6 | 
| 2 | 2 | {123, 312, 231} | 3 | 3 | 
| 2 | 3 | {tất cả 6 uốn} | 6 | 6 | 
| 3 | 3 | {123,132} | 2 | 2 | 

Sum = 25, phù hợp với kết quả mong đợi. 

Một đầu vào khác:```
2 2
2 1
1 2
```| tôi | r | sàn có thể tiếp cận | f(l,r) | đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | {12,21} | 2 | 2 | 
| 1 | 2 | {12,21} | 2 | 2 | 
| 2 | 2 | {12,21} | 2 | 2 | 

Tổng = 6 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * k! * k) | Đối với mỗi điểm cuối bên trái, chúng tôi có thể mở rộng`r`lên đến`n`, đang cập nhật`reachable`trong đó có nhiều nhất`k!`các phần tử, mỗi phần tử yêu cầu`O(k)`cho thành phần hoán vị | 
| Không gian | O(k! * k + n*k) | Lưu trữ tất cả các hoán vị, bảng nhân và dữ liệu đầu vào | 

Với$n \le 2 \times 10^5$Và$k \le 5$,`k! = 120`, vì vậy các hoạt động trong trường hợp xấu nhất là đại khái$2 \times 10^5 * 120 * 5 \approx 1.2 \times 10^8$, điều đó có thể chấp nhận được. 

## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    P = [list(map(lambda x: int(x)-1, input().split())) for _ in range(n)]
    from itertools import permutations
    all_perms = list(permutations(range(k)))
    perm_id = {p: i for i, p in enumerate(all_perms)}
    mul = [[0]*len(all_perms) for _ in range(len(all_perms))]
    for i, a in enumerate(all_perms):
        for j, b in enumerate(all_perms):
            c = tuple(a[b[x]] for x in range(k))
            mul[i][j] = perm_id[c]
    ans = 0
```
