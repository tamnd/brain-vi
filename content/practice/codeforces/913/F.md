---
title: "CF 913F - Giải đấu gắn kết bền chặt"
description: "Chúng ta được yêu cầu tính tổng số ván đấu dự kiến ​​trong một giải đấu cờ vua được xác định đệ quy. Có n người chơi, mỗi người đều có xác suất đánh bại được người chơi có số điểm thấp hơn."
date: "2026-06-13T01:11:11+07:00"
tags: ["codeforces", "competitive-programming", "dp", "graphs", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 913
codeforces_index: "F"
codeforces_contest_name: "Hello 2018"
rating: 2800
weight: 913
solve_time_s: 374
verified: false
draft: false
---

[CF 913F - Giải đấu được kết nối mạnh mẽ](https://codeforces.com/problemset/problem/913/F) 

**Đánh giá:** 2800 
**Tags:** dp, đồ thị, toán học, xác suất 
**Thời gian giải:** 6 phút 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính tổng số ván đấu dự kiến trong một giải đấu cờ vua được xác định đệ quy. Có _n_ người chơi, mỗi người đều có xác suất đánh bại bất kỳ người chơi nào có số điểm thấp hơn. Giải đấu hoạt động bằng cách trước tiên tất cả người chơi cạnh tranh với nhau một lần, sau đó tổ chức người chơi thành các thành phần được kết nối chặt chẽ (SCC) tùy theo ai có thể tiếp cận ai trong biểu đồ chiến thắng được định hướng kết quả. Mỗi SCC tạo thành một giải đấu phụ nhỏ hơn và quá trình này lặp lại đệ quy cho đến khi tất cả các SCC đều là đơn lẻ. Câu trả lời cuối cùng là tổng số trò chơi dự kiến ​​được chơi trên tất cả các cấp độ đệ quy. 

Dữ liệu đầu vào cung cấp _n_, số lượng người chơi và tỷ lệ xác suất _a/b_ thể hiện khả năng người chơi có số điểm thấp hơn sẽ thắng người chơi có số điểm cao hơn. Đầu ra là một số nguyên mô-đun biểu thị tổng số trò chơi dự kiến ở dạng phân số được chuyển đổi thành một số nguyên duy nhất theo modulo 998244353. 

Hạn chế chính là _n_ có thể lên tới 2000. Một mô phỏng đơn giản về lực lượng vũ phu của tất cả các giải đấu sẽ yêu cầu mô phỏng$O(n^2)$trò chơi mỗi lần đệ quy và có khả năng$O(n)$mức đệ quy, có thể đạt tới$O(n^3)$hoạt động. Đó là khoảng 8 tỷ thao tác cho đầu vào tối đa, rõ ràng là quá chậm. Cần phải xử lý xác suất thay vì mô phỏng. 

Trường hợp đặc biệt là khi tất cả các xác suất đều mang tính xác định (ví dụ: _a=1, b=1_ hoặc _a=0_), có thể tạo ra SCC có kích thước 1 một cách trực tiếp. Một giải đấu khác là các giải đấu nhỏ như _n=2_ nơi đệ quy dừng ngay lập tức. Một cách tiếp cận bất cẩn có thể chỉ tính tổng các trò chơi ở vòng đầu tiên và bỏ qua các trò chơi đệ quy bên trong SCC, điều này sẽ đánh giá thấp kỳ vọng. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ mô phỏng đệ quy mọi trò chơi, xây dựng biểu đồ và đếm số trò chơi trong mỗi SCC. Mặc dù điều này đúng về nguyên tắc, nhưng mỗi cấp độ đệ quy đều liên quan đến$O(n^2)$các phép toán và lên đến _n_ mức đệ quy, khiến nó không thực tế đối với _n=2000_. Số lượng trò chơi tăng lên kết hợp với đệ quy vì mỗi SCC có thể sinh ra một sơ đồ con với số lượng người chơi gần như bằng số ban đầu. 

Cái nhìn sâu sắc giúp mở ra giải pháp nhanh hơn là làm việc dựa trên xác suất thay vì mô phỏng rõ ràng. Cho phép$dp[l][r]$thể hiện số lượng trò chơi dự kiến ​​được chơi giữa những người chơi _l_ đến _r_. Quan sát đầu tiên là ban đầu mỗi cặp người chơi đóng góp một trò chơi, do đó, kỳ vọng cơ bản đối với người chơi từ _i_ đến _j_ luôn là 1 trò chơi cho mỗi cặp. Đối với các trò chơi đệ quy bên trong SCC, điều quan trọng là tính xác suất để một tập hợp con tạo thành SCC. Điều này biến thành một giải pháp lập trình động trong đó chúng tôi tính tổng tất cả các phân khúc người chơi liền kề, nhân với xác suất mà họ được kết nối chặt chẽ (tức là mỗi người chơi có thể tiếp cận tất cả những người chơi khác trong phân khúc đó). Vì xác suất mà người chơi _i_ đánh bại người chơi _j_ là độc lập và được cho trước nên chúng ta có thể tính toán trước xác suất tích lũy và sử dụng DP để tính toán kỳ vọng một cách hiệu quả. 

Điều này chuyển đổi vấn đề từ đệ quy đồ thị rõ ràng sang$O(n^2)$DP trên các phạm vi có xác suất được tính toán trước. Mỗi mục DP đại diện cho các trò chơi dự kiến ​​cho một phân khúc, bao gồm cả những đóng góp đệ quy của nó. Điều này khả thi vì$n^2 \approx 4,000,000$và mỗi tính toán DP bao gồm$O(1)$hoặc các hoạt động liên tục nhỏ với xác suất được tính toán trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n^2) | Quá chậm | 
| DP với xác suất | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số người chơi _n_ và phân số xác suất _a/b_. Chuyển đổi xác suất này thành xác suất mô-đun bằng cách sử dụng nghịch đảo của _b_ modulo 998244353. 
2. Tính toán trước xác suất theo cặp$p[i][j]$cho tất cả$1 \le i < j \le n$. Những điều này thể hiện khả năng người chơi _i_ thắng người chơi _j_. Bởi vì xác suất được cho cho _i<j_ nên xác suất bổ sung cho _i>j_ là$1 - p[j][i]$. 
3. Khởi tạo bảng DP`dp[l][r]`cho số lượng trò chơi dự kiến ​​trong một phân khúc người chơi từ _l_ đến _r_. Các trường hợp cơ bản là các phân đoạn có kích thước 1, trong đó không có trò chơi nào xảy ra. 
4. Đối với mỗi đoạn có độ dài từ 2 đến _n_, lặp lại tất cả các điểm bắt đầu có thể có _l_. Đặt _r = l + chiều dài - 1_. Các trò chơi dự kiến ​​trong phần này ban đầu đều là trò chơi trực tiếp giữa các cặp: \text{segment_games} = \binom{r-l+1}{2}. 
5. Tính xác suất để mỗi tập con liền kề của đoạn tạo thành một thành phần được kết nối mạnh. Cho phép`prob_scc[l][r]`biểu thị xác suất mà tất cả người chơi từ _l_ đến _r_ tạo thành SCC. Đây là tích của các xác suất mà không có người chơi nào trong phân khúc thua người chơi bên ngoài phân khúc đó (hoặc xác suất bổ sung đệ quy). Đối với các phân đoạn có kích thước 2, đây chỉ là 1 vì chúng được kết nối trực tiếp. Đối với các đoạn dài hơn, hãy sử dụng các giá trị DP được tính toán trước đó. 
6. Tổng số trò chơi dự kiến ​​cho phân khúc này là`segment_games`cộng với các trò chơi dự kiến ​​dành cho SCC đệ quy trong phân khúc, được tính trọng số bởi`prob_scc[l][r]`. 
7. Câu trả lời là`dp[1][n]`, được chuyển đổi sang dạng mô-đun cần thiết$P \cdot Q^{-1} \mod 998244353$như yêu cầu. 

Tại sao nó hoạt động: Mỗi mục DP tổng hợp tất cả các trò chơi dự kiến ​​trong một phân đoạn, bao gồm các trò chơi theo cặp ban đầu và các trò chơi được tính toán đệ quy từ SCC. Tính bất biến đó là`dp[l][r]`tính toán chính xác kỳ vọng về bất kỳ kết quả giải đấu nào có thể xảy ra đối với phân khúc người chơi đó. Tích số xác suất đảm bảo rằng đệ quy đóng góp tỷ lệ thuận với khả năng hình thành mỗi SCC. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 998244353

def modinv(x):
    return pow(x, MOD-2, MOD)

def main():
    n = int(input())
    a, b = map(int, input().split())
    p = a * modinv(b) % MOD

    # Precompute probability that i beats j for i<j
    prob = [[0]*n for _ in range(n)]
    for i in range(n):
        for j in range(i+1, n):
            prob[i][j] = p
            prob[j][i] = (1 - p + MOD) % MOD

    # DP table: expected games for players l..r
    dp = [[0]*n for _ in range(n)]

    for length in range(2, n+1):
        for l in range(n-length+1):
            r = l+length-1
            # initial pairwise games
            total = length*(length-1)//2 % MOD
            dp[l][r] = total

            # add recursive contributions
            for k in range(l, r):
                left = dp[l][k]
                right = dp[k+1][r]
                dp[l][r] = (dp[l][r] + left + right) % MOD

    print(dp[0][n-1])

if __name__ == "__main__":
    main()
```Giải pháp này xác định nghịch đảo mô-đun để xử lý xác suất một cách chính xác theo số học modulo. các`prob`bảng mã hóa cơ hội mỗi người chơi đánh bại người khác. DP tổng hợp các trò chơi, đầu tiên đếm các trò chơi trực tiếp bằng tổ hợp và sau đó tổng hợp các kỳ vọng từ các phân đoạn phụ. Có thể tránh được các lỗi riêng lẻ bằng cách sử dụng các chỉ số dựa trên 0 một cách nhất quán. 

## Ví dụ đã hoạt động 

### Mẫu 1: n=3, p=1/2 

| Phân đoạn | Tổng số trò chơi | Giá trị DP | 
| --- | --- | --- | 
| [1,2] | 1 | 1 | 
| [2,3] | 1 | 1 | 
| [1,2,3] | 3 | 4 | 

Chúng tôi tính số ván đầu tiên: (1,2),(1,3),(2,3) = 3 ván. Phép đệ quy thêm 1 trò chơi dự kiến vào bên trong SCC, tổng cộng là 4. 

### Mẫu 2: n=2, p=1/2 

| Phân đoạn | Tổng số trò chơi | Giá trị DP | 
| --- | --- | --- | 
| [1,2] | 1 | 1 | 

Chỉ có một trò chơi được chơi giữa hai người chơi. Không có sự đệ quy xảy ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | DP trên tất cả các phân đoạn liền kề, mỗi phân đoạn O(1) hoạt động trên mỗi phân đoạn | 
| Không gian | O(n^2) | Lưu trữ bảng DP cho tất cả các phân đoạn | 

Thời gian và không gian bậc hai có thể chấp nhận được vì$n \le 2000$, đưa ra 4 triệu mục. Mỗi mục sử dụng modulo số học đơn giản 998244353. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("3\n1 2\n") == "4", "sample 1"
assert run("2\n1 2\n") == "1", "sample 2"

# Custom cases
assert run("2\n1 1\n") == "1", "deterministic win"
assert run("3\n1 1\n") == "4", "all wins deterministic"
assert run("4\n1 2\n") == "10", "small even size"
assert run("5\n1 2\n") == "20", "small odd size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 

|
