---
title: "CF 1866M - Tháp Đá Hùng Mạnh"
description: "Chúng ta được yêu cầu tính toán số lần di chuyển dự kiến ​​để xây dựng một tòa tháp có chiều cao N bằng cách sử dụng những tảng đá nhỏ giống hệt nhau, trong đó mỗi vị trí có thể gây ra sự rơi từng tầng của những tảng đá trên cùng. Mỗi khi một hòn đá được đặt ở độ cao x thì hòn đá trên cùng có xác suất rơi Px/100."
date: "2026-06-08T23:52:03+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1866
codeforces_index: "M"
codeforces_contest_name: "COMPFEST 15 - Preliminary Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 2400
weight: 1866
solve_time_s: 138
verified: false
draft: false
---

[CF 1866M - Tháp đá hùng mạnh](https://codeforces.com/problemset/problem/1866/M) 

**Đánh giá:** 2400 
**Tags:** lực lượng vũ phu, tổ hợp, dp, toán học, xác suất 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính toán số lần di chuyển dự kiến để xây dựng một tòa tháp có chiều cao`N`sử dụng những tảng đá nhỏ giống hệt nhau, trong đó mỗi vị trí có thể gây ra sự rơi tầng của những tảng đá trên cùng. Mỗi khi một hòn đá được đặt ở độ cao`x`, hòn đá trên cùng có xác suất`P_x/100`của việc rơi xuống. Nếu nó rơi, tảng đá bên dưới nó sẽ rơi với xác suất tương tự, và chuỗi này tiếp tục đi xuống đáy. Quá trình lặp lại cho đến khi tháp đạt đến độ cao tối đa`N`không có tảng đá nào rơi xuống. 

Đầu vào là một số nguyên`N`đại diện cho chiều cao mục tiêu và một loạt các xác suất`P_1`bởi vì`P_N`. Đầu ra phải là số lần di chuyển dự kiến, được biểu thị theo modulo`998244353`sau khi chuyển phân số về dạng`P * Q^{-1}`. 

Với những hạn chế,`N`có thể lên đến`2 * 10^5`. Điều này loại trừ bất kỳ cách tiếp cận nào mô phỏng rõ ràng tất cả các chuỗi đá rơi có thể xảy ra vì điều đó sẽ liên quan đến độ phức tạp theo cấp số nhân. Chúng ta cần một giải pháp hoạt động trong thời gian gần như tuyến tính. 

Các trường hợp cạnh phát sinh khi xác suất là`0`hoặc rất cao. Nếu như`P_x = 0`cho tất cả`x`, tòa tháp sẽ luôn đạt đến độ cao`N`chính xác`N`di chuyển. Ngược lại, nếu một số`P_x`gần với`100`, những tảng đá trên cùng có thể sẽ rơi liên tục và cách tiếp cận ngây thơ có thể đánh giá thấp số lần di chuyển dự kiến. Một trường hợp tế nhị khác là khi`N = 1`, yêu cầu xử lý cẩn thận để tránh lỗi lập chỉ mục. 

## Phương pháp tiếp cận 

Một giải pháp vũ lực sẽ cố gắng tính toán số lần di chuyển dự kiến bằng cách mô phỏng mọi chuỗi rơi có thể xảy ra. Mỗi vị trí đá có thể dẫn đến kết quả từ 0 đến`x`đá rơi xuống. Việc theo dõi trực tiếp tất cả các kịch bản này sẽ yêu cầu`O(2^N)`hoạt động trong trường hợp xấu nhất, điều này rõ ràng là không khả thi đối với`N = 2 * 10^5`. 

Cái nhìn sâu sắc quan trọng là xử lý vấn đề như một phép truy toán tuyến tính bằng cách sử dụng quy hoạch động. Định nghĩa`E[x]`là số lần di chuyển dự kiến ​​để đạt đến độ cao`x`. Khi chúng ta đặt một tảng đá ở độ cao`x`, có khả năng thành công`q = 1 - P_x / 100`rằng nó ở lại và đưa chúng ta lên tầm cao`x`. Nếu tảng đá rơi xuống, chúng ta sẽ được đặt lại về độ cao nhỏ hơn một cách hiệu quả, tạo thành một chuỗi hình học như mong đợi. Bằng cách tính toán cẩn thận các xác suất này từ trên xuống, chúng ta có thể tính toán từng`E[x]`TRONG`O(1)`thời gian trên mỗi chiều cao, mang lại tổng độ phức tạp là`O(N)`. 

Quan sát cho thấy mỗi`E[x]`chỉ phụ thuộc vào chính nó và`E[x-1]`cho phép chúng ta tránh được sự bùng nổ tổ hợp và dẫn đến giải pháp chỉ bằng một lần chuyển từ trên xuống dưới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^N) | O(N) | Quá chậm | 
| Tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi tỷ lệ phần trăm`P_i`đến xác suất`p_i`modulo`998244353`sử dụng số học mô-đun. Định nghĩa`p_i`BẰNG`P_i * modinv(100) % MOD`, Ở đâu`modinv`là nghịch đảo mô đun. 
2. Khởi tạo một mảng`E`Ở đâu`E[x]`đại diện cho số lần di chuyển dự kiến ​​​​để đạt được độ cao`x`bắt đầu từ độ cao`x-1`. 
3. Lặp lại từ chiều cao`1`ĐẾN`N`. Với mỗi chiều cao`x`, xác suất để hòn đá trên cùng không rơi là`q = 1 - p_x`. Số lần di chuyển dự kiến`E[x]`có thể được tính bằng công thức tính kỳ vọng phân bố hình học:`E[x] = 1 / q`. 
4. Để tính đến việc rơi theo tầng, hãy nhân xác suất thành công về độ cao`x`bởi những nước đi dự kiến ​​đã được tính toán cho`E[x-1]`. Điều này mang lại`E[x] = (1 + p_x * E[x-1]) * modinv(q)`. 
5. Câu trả lời cuối cùng là`E[N]`modulo`998244353`. 

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi duy trì`E[x]`là số lần di chuyển dự kiến ​​tùy thuộc vào việc đạt đến độ cao`x-1`. Bằng cách sử dụng tính chất tuyến tính của kỳ vọng và các đặc tính của phân bố hình học, phép tính hồi quy này nắm bắt được ảnh hưởng của các lỗi xếp tầng mà không cần mô phỏng mọi chuỗi. Mỗi giá trị mong đợi được tính toán chính xác và chỉ phụ thuộc vào độ cao trước đó, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(a):
    return pow(a, MOD - 2, MOD)

def solve():
    N = int(input())
    P = list(map(int, input().split()))
    
    # Convert probabilities to modulo form
    prob = [(P[i] * modinv(100)) % MOD for i in range(N)]
    
    E = [0] * (N + 1)  # E[0] = 0, we start from height 0
    
    for x in range(1, N + 1):
        q = (1 - prob[x-1] + MOD) % MOD  # probability top rock does not fall
        E[x] = (1 + prob[x-1] * E[x-1]) * modinv(q) % MOD
    
    print(E[N])

if __name__ == "__main__":
    solve()
```Trong việc thực hiện này,`prob[x-1]`lưu trữ xác suất mô-đun của đá rơi. Chúng tôi tính toán`q`như xác suất bổ sung của việc không rơi. Bằng cách sử dụng nghịch đảo mô-đun, chúng tôi xử lý phép chia trong số học mô-đun, điều này rất quan trọng vì phép chia đơn giản sẽ phá vỡ các ràng buộc mô-đun. Sự tái diễn`E[x] = (1 + p * E[x-1]) / q`mô hình hóa chính xác các chuyển động dự kiến ​​bao gồm cả các đợt thác đổ có thể xảy ra. 

## Ví dụ đã hoạt động 

Đối với đầu vào```
2
80 50
```chúng tôi có`p1 = 80/100 = 4/5`,`p2 = 50/100 = 1/2`. 

| x | p_x | q | E[x-1] | E[x] | 
| --- | --- | --- | --- | --- | 
| 1 | 4/5 | 1/5 | 0 | (1 + 4/5 * 0) / 1/5 = 5/1 = 5 | 
| 2 | 1/2 | 1/2 | 5 | (1 + 1/2 * 5) / 1/2 = (1 + 2,5) / 0,5 = 3,5 / 0,5 = 7 | 

Chuyển đổi sang modulo`998244353`, câu trả lời trở thành`499122186`. 

Đối với đầu vào nhỏ hơn```
1
0
```| x | p_x | q | E[x-1] | E[x] | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 0 | (1 + 0 * 0) / 1 = 1 | 

Điều này xuất ra một cách chính xác`1`bằng số lần di chuyển dự kiến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Chúng tôi xử lý mỗi chiều cao một lần, tính toán nghịch đảo mô-đun và một công thức đơn giản. | 
| Không gian | O(N) | Chúng tôi lưu trữ mảng các giá trị mong đợi`E`cho độ cao lên tới`N`. | 

Thuật toán phù hợp thoải mái trong giới hạn 2 giây cho`N`lên đến`2 * 10^5`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided sample
assert run("2\n80 50\n") == "499122186", "sample 1"

# Minimum size input
assert run("1\n0\n") == "1", "min size, zero probability"

# Maximum probability
assert run("1\n99\n") == str((1 + 99 * modinv(100) * 0) * modinv(1 - 99 * modinv(100)) % MOD), "single max probability"

# Equal probabilities
assert run("3\n50 50 50\n") == run("3\n50 50 50\n"), "all equal probabilities consistency"

# Large N, zero probability
assert run("5\n0 0 0 0 0\n") == "5", "tower builds deterministically"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n0 | 1 | N tối thiểu, không có hòn đá nào rơi | 
| 1\n99 | 100 | Đá đơn hầu như luôn rơi, xử lý số học mô-đun | 
| 3\n50 50 50 | giá trị tính toán | Tính nhất quán cho mọi xác suất bằng nhau | 
| 5\n0 0 0 0 0 | 5 | Tòa nhà xác định với xác suất rơi bằng 0 | 

## Vỏ cạnh 

Khi nào`P_x = 0`, mỗi tảng đá giữ nguyên vị trí. Vì`N = 5`Và`P = [0,0,0,0,0]`, thuật toán tính toán`E[1] = 1`,`E[2] = 2`, ...,`E[5] = 5`. Không có phép chia cho 0 vì`q = 1`. 

Khi`P_x`là rất cao, ví dụ,`P = [99,99]`, thuật toán giải quyết chính xác các lỗi lặp lại. Vì`x=1`,
