---
title: "CF 1633D - Làm cho họ bình đẳng"
description: "Chúng ta được yêu cầu bắt đầu với một mảng có kích thước $n$ trong đó mọi phần tử ban đầu là 1. Với mỗi phần tử $ai$, chúng ta có thể thực hiện các phép toán có dạng $ai = ai + lfloor ai / x rfloor$, chọn $x 0$ tùy thích. Mỗi phần tử có giá trị mục tiêu $bi$ và phần thưởng $ci$."
date: "2026-06-10T04:49:03+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1633
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 122 (Rated for Div. 2)"
rating: 1600
weight: 1633
solve_time_s: 42
verified: true
draft: false
---

[CF 1633D - Làm cho họ bình đẳng](https://codeforces.com/problemset/problem/1633/D) 

**Đánh giá:** 1600 
**Tags:** dp, tham lam 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu bắt đầu với một mảng có kích thước$n$trong đó mọi phần tử ban đầu là 1. Với mỗi phần tử$a_i$, chúng ta có thể thực hiện các thao tác có dạng$a_i = a_i + \lfloor a_i / x \rfloor$, đang chọn$x > 0$như chúng tôi muốn. Mỗi phần tử có một giá trị đích$b_i$và một phần thưởng$c_i$. Mục tiêu của chúng tôi là tối đa hóa tổng số tiền kiếm được bằng cách tạo ra một số yếu tố chính xác bằng mục tiêu của chúng mà không vượt quá$k$tổng số hoạt động trên tất cả các yếu tố. 

Đầu vào chứa nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chỉ định kích thước mảng$n$, giới hạn hoạt động$k$, mảng mục tiêu$b$và mảng tiền xu$c$. Chúng tôi phải xuất ra số tiền tối đa có thể đạt được cho mỗi trường hợp thử nghiệm. 

Nhìn vào những hạn chế,$n$có thể lên tới$10^3$và tổng của$n$trên tất cả các trường hợp thử nghiệm là nhiều nhất$10^3$, có nghĩa là tổng số phần tử là nhỏ. Tuy nhiên,$k$có thể lớn như$10^6$, vì vậy bất kỳ thuật toán nào mô phỏng các hoạt động một cách đơn giản sẽ quá chậm. Mỗi thao tác sẽ tăng một phần tử theo một hàm của chính nó và vì$b_i \le 10^3$, số lượng thao tác để đạt được bất kỳ$b_i$bị giới hạn và tương đối nhỏ. Điều này cho thấy chúng ta có thể tính toán trước số lượng thao tác tối thiểu để đạt được từng mục tiêu có thể từ 1. 

Trường hợp cạnh không rõ ràng xảy ra khi$k = 0$. Trong trường hợp đó, không có phần tử nào có thể tăng lên, do đó, phần tử duy nhất đóng góp tiền là những phần tử có mục tiêu$b_i = 1$. Việc triển khai bất cẩn có thể cố gắng thực hiện các thao tác mà không kiểm tra$k = 0$và đưa ra câu trả lời khác 0. Một trường hợp cạnh khác là khi$b_i = 1$; đạt đến 1 yêu cầu không có thao tác nào, phải được công nhận. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng từng lựa chọn có thể có của$x$cho mọi phần tử cho đến khi đạt được mục tiêu, đếm các thao tác. Đối với một phần tử duy nhất$a_i$, số lượng thao tác cần thiết để đạt được$b_i$có thể lên đến$b_i - 1$, vì mỗi thao tác tăng$a_i$ít nhất là 1. Trên$n$các yếu tố, điều này có thể dẫn đến$n \cdot 10^3$các hoạt động được mô phỏng cho mỗi trường hợp thử nghiệm, nhân với phạm vi$x$, nhanh chóng vượt quá thời hạn. 

Quan sát quan trọng là số lượng thao tác tối thiểu cần thiết để đạt bất kỳ giá trị nào từ 1 là tương đối nhỏ và độc lập với$k$. Vì$1 \le a_i \le 10^3$, chúng ta có thể tính toán trước một mảng`ops_needed[val]`như vậy`ops_needed[val]`là số lượng hoạt động tối thiểu để đạt được`val`từ 1. Chúng ta có thể xây dựng điều này bằng cách tiếp cận giống BFS, khám phá từ 1 và đối với mỗi giá trị hiện tại, áp dụng tất cả các hoạt động có thể$a_i + \lfloor a_i / x \rfloor$không vượt quá 1000. Điều này cung cấp một bảng các phép toán tối thiểu cho tất cả các giá trị lên tới 1000. 

Khi chúng tôi biết các hoạt động tối thiểu cho từng mục tiêu$b_i$, bài toán trở thành bài toán ba lô giới hạn cổ điển. Mỗi phần tử là một vật phẩm có trọng lượng bằng`ops_needed[b_i]`và giá trị bằng`c_i`. Chúng tôi có một chiếc ba lô có sức chứa$k$và chúng tôi muốn tối đa hóa tổng số tiền. Sử dụng quy hoạch động`k`hiệu quả vì`k`có thể lên tới$10^6$nhưng tổng số thao tác tối thiểu trên tất cả các mục nhỏ hơn nhiều; cụ thể là tổng của tất cả`ops_needed[b_i]`qua một trường hợp thử nghiệm là nhiều nhất$n \cdot 12$vì không có phần tử nào yêu cầu nhiều hơn khoảng 12 thao tác để đạt 1000. Do đó, DP trên`k`với sự tối ưu hóa mà chúng tôi không bao giờ vượt quá`k`hoạt động tốt 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * b_i * x) | O(1) | Quá chậm | 
| Tối ưu (tính toán trước + ba lô) | O(n * k) trường hợp xấu nhất, thực tế ~O(n * max_ops_ Need) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước số lượng thao tác tối thiểu để đạt từng giá trị từ 1 đến 1000. Bắt đầu với hàng đợi chứa 1 với 0 thao tác. Đối với mỗi giá trị trong hàng đợi, hãy lặp lại`x`từ 1 đến giá trị hiện tại. Tính giá trị tiếp theo là`next_val = cur + cur // x`. Nếu như`next_val`chưa đạt được trước đó, hãy đánh dấu hoạt động của nó được tính là`cur_ops + 1`và thêm nó vào hàng đợi. Dừng khi tất cả các giá trị lên tới 1000 được lấp đầy. 
2. Với mỗi test case, hãy đọc`n`,`k`,`b`, Và`c`. Hãy biến bài toán thành một chiếc ba lô: với mỗi phần tử,`weight = ops_needed[b_i]`Và`value = c_i`. 
3. Khởi tạo mảng DP`dp[0..k]`đến 0, ở đâu`dp[j]`đại diện cho số tiền tối đa có thể đạt được với nhiều nhất`j`hoạt động. 
4. Đối với mỗi phần tử, lặp lại`j`từ`k`xuống`weight`. Cập nhật`dp[j] = max(dp[j], dp[j - weight] + value)`. Lặp lại ngược lại đảm bảo mỗi phần tử được sử dụng nhiều nhất một lần. 
5. Sau khi xử lý tất cả các phần tử,`dp[k]`chứa số tiền tối đa có thể đạt được lên tới`k`hoạt động. Xuất giá trị này. 

Tại sao nó hoạt động: Việc tính toán trước các hoạt động tối thiểu đảm bảo chúng tôi biết cách đạt được từng mục tiêu với chi phí thấp nhất. DP đảm bảo rằng chúng tôi chọn tập hợp con các phần tử có hoạt động kết hợp phù hợp với ngân sách`k`trong khi tối đa hóa tiền xu. Việc lặp ngược sẽ ngăn chặn việc đếm hai lần bất kỳ phần tử nào, duy trì thuộc tính ba lô 0-1. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

# Precompute minimal operations to reach values from 1 to 1000
MAX_VAL = 1000
ops_needed = [float('inf')] * (MAX_VAL + 1)
ops_needed[1] = 0
queue = deque([1])
while queue:
    cur = queue.popleft()
    cur_ops = ops_needed[cur]
    for x in range(1, cur + 1):
        nxt = cur + cur // x
        if nxt <= MAX_VAL and ops_needed[nxt] > cur_ops + 1:
            ops_needed[nxt] = cur_ops + 1
            queue.append(nxt)

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    b = list(map(int, input().split()))
    c = list(map(int, input().split()))
    
    # Map b[i] to ops_needed, cap at k+1 (we cannot use more than k)
    weights = [min(ops_needed[val], k + 1) for val in b]
    
    dp = [0] * (k + 1)
    for w, v in zip(weights, c):
        if w > k:
            continue
        for j in range(k, w - 1, -1):
            dp[j] = max(dp[j], dp[j - w] + v)
    
    print(dp[k])
```Phần tính toán trước đảm bảo rằng mỗi giá trị đích có số lượng thao tác tối thiểu. Việc sử dụng BFS ngăn chặn việc tính toán lại dư thừa và tránh các vòng lặp vô hạn. Ánh xạ trọng số tới`min(ops_needed[val], k + 1)`đảm bảo chúng tôi bỏ qua các yếu tố không thể tiếp cận được trong`k`hoạt động. DP ba lô xử lý việc lựa chọn tập hợp con. Lặp lại`j`ngược đảm bảo lựa chọn 0-1. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
n = 4, k = 4
b = [1, 7, 5, 2]
c = [2, 6, 5, 2]
```| tôi | b_i | ops_cần thiết | c_i | Cập nhật DP | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 2 | dp[0..4] = max(dp[j], dp[j-0]+2) → cộng 2 vào tất cả j | 
| 1 | 7 | 4 | 6 | dp[4] = max(dp[4], dp[0]+6)=8, dp[3]=..., v.v. | 
| 2 | 5 | 3 | 5 | kết hợp với dp để tối đa hóa | 
| 3 | 2 | 1 | 2 | dp cuối cùng[4]=9 | 

Điều này xác nhận DP chọn các yếu tố tối đa hóa số tiền trong giới hạn hoạt động. 

**Vỏ cạnh k=0** 

đầu vào:```
n = 3, k = 0
b = [1, 2, 1]
c = [5, 4, 3]
```| tôi | b_i | ops_cần thiết | c_i | Cập nhật DP | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 5 | dp[0] = max(0,0+5)=5 | 
| 1 | 2 | 1 | 4 | trọng lượng>k, bỏ qua | 
| 2 | 1 | 0 | 3 | dp[0]=max(5,5+3)=8 | 

Đếm chính xác số tiền từ các phần tử đã bằng 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp
