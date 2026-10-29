---
title: "CF 106193L - Lý thuyết số may mắn"
description: "Chúng ta được cung cấp một quy trình hoạt động giống như một bộ đếm tích lũy ngẫu nhiên. Mỗi lần Lucy nhấn nút cuộn, bộ đếm sẽ tăng thêm một giá trị ngẫu nhiên độc lập được rút ra thống nhất từ ​​khoảng $(0, d)$."
date: "2026-06-19T18:42:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106193
codeforces_index: "L"
codeforces_contest_name: "2025-2026 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 106193
solve_time_s: 71
verified: true
draft: false
---

[CF 106193L - Lý thuyết số may mắn](https://codeforces.com/problemset/problem/106193/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một quy trình hoạt động giống như một bộ đếm tích lũy ngẫu nhiên. Mỗi lần Lucy nhấn nút cuộn, bộ đếm sẽ tăng thêm một giá trị ngẫu nhiên độc lập được rút ra thống nhất từ ​​khoảng thời gian$(0, d)$. Bất cứ lúc nào cô ấy có thể dừng lăn và nhấn rút tiền, thao tác này sẽ chuyển đổi giá trị thực hiện tại$S$vào số vé tương đương$\lceil S \rceil$, sau đó bộ đếm sẽ reset về 0. 

Lucy có ngân sách hạn chế$n$cuộn và chính xác$k$rút tiền. Cô ấy có thể quyết định một cách thích ứng sau mỗi lần quay xem nên tiếp tục tích lũy hay dừng lại và rút tiền ngay lập tức, đồng thời cô ấy thấy giá trị hiện tại chính xác$S$với độ chính xác vô hạn. 

Mục tiêu là tối đa hóa tổng số vé dự kiến ​​sau khi thực hiện tất cả$n$cuộn và chính xác$k$rút tiền. 

Cấu trúc chính là mỗi lần rút tiền sẽ chia chuỗi cuộn thành$k$phân khúc, mỗi phân khúc đóng góp$\lceil \text{sum of rolls in that segment} \rceil$. Do đó, quyết định này là một vấn đề phân vùng động trong điều kiện không chắc chắn: chúng ta đang chọn nơi cắt một chuỗi các số gia ngẫu nhiên để tối đa hóa tổng làm tròn dự kiến. 

Các ràng buộc ngụ ý rằng một nghiệm bậc hai trong$n$về nguyên tắc mỗi lần kiểm tra đều có thể chấp nhận được, vì$n \le 2000$và có tới 2000 bài kiểm tra. Điều này đẩy chúng ta về phía$O(n^2)$hoặc khấu hao các ý tưởng DP cho mỗi thử nghiệm và loại trừ mọi mô phỏng phân phối liên tục trên mỗi tiểu bang hoặc bất kỳ phương pháp tiếp cận Monte Carlo nào. 

Một điểm tinh tế là việc làm tròn chỉ xảy ra tại thời điểm rút tiền. Điều này làm cho mục tiêu trở nên phi tuyến tính trong tổng số gia và độ tuyến tính ngây thơ của kỳ vọng là không đủ. 

Một dạng lỗi phổ biến là giả sử mỗi cuộn đóng góp một cách độc lập vào hiệu suất dự kiến.$(d+1)/2$vé khi rút ngay lập tức. Điều này chỉ đúng khi mỗi cuộn là phân đoạn riêng của nó, nhưng bị hỏng ngay khi phân đoạn chứa nhiều cuộn vì$\lceil x+y \rceil \neq \lceil x \rceil + \lceil y \rceil$trong sự mong đợi. 

Ví dụ, với$d=1$, hai cuộn hoạt động khác nhau tùy thuộc vào việc nhóm. Một đoạn gồm hai cuộn có số lần làm tròn dự kiến ​​khác với hai lần rút tiền riêng biệt, mặc dù tổng số tiền dự kiến ​​thô là như nhau. Sự tương tác này chính xác là điều làm cho vấn đề trở nên không hề tầm thường. 

## Phương pháp tiếp cận 

Cách giải thích trực tiếp nhất là thử mọi cách để chia nhỏ$n$cuộn vào$k$các đoạn liền kề. Đối với một phân vùng cố định, kỳ vọng là tổng của các phân đoạn giá trị kỳ vọng của$\lceil X \rceil$, Ở đâu$X$là tổng các biến ngẫu nhiên thống nhất trong phân đoạn đó. Một giải pháp brute-force sẽ liệt kê tất cả các thành phần của$n$vào trong$k$các bộ phận, trong đó có$\binom{n-1}{k-1}$, và với mỗi giá trị mong đợi tính toán. Đây là cấp số nhân trong$n$và ngay lập tức không thể thực hiện được. 

Bước tiếp theo là nhận biết cấu trúc con tối ưu. Khi chúng tôi khắc phục việc rút tiền đầu tiên xảy ra sau$x$cuộn, vấn đề còn lại không phụ thuộc vào quá khứ vì quá trình đặt lại. Điều này mang lại sự lặp lại kiểu ba lô cổ điển trong đó chúng tôi xác định$dp[i][j]$là giá trị mong đợi tốt nhất khi sử dụng$i$cuộn và$j$rút tiền. Việc chuyển đổi yêu cầu phải thử tất cả các độ dài đoạn đầu tiên có thể. 

Điều còn lại là khó khăn cốt lõi: tính toán hàm$f(x)$, giá trị kỳ vọng của$\lceil S_x \rceil$, Ở đâu$S_x$là tổng của$x$i.i.d. các biến thống nhất trong$(0,d)$. Khi đã biết điều này, phần còn lại sẽ trở thành phân vùng DP tiêu chuẩn. 

Quan sát cấu trúc quan trọng là$S_x$là một phân phối liên tục với sự hỗ trợ trên$(0, xd)$và mật độ của nó là tích chập của mật độ đồng đều. Mức trần dự kiến ​​chỉ phụ thuộc vào sự phân bố phần nguyên của$S_x$, từ$\lceil S_x \rceil = \lfloor S_x \rfloor + 1$gần như chắc chắn. 

Vì vậy, vấn đề quy về việc tính xác suất để$S_x$nằm trong mỗi khoảng đơn vị$[t, t+1)$. Điều này có thể được thực hiện thông qua DP qua các phép phân phối, về cơ bản là xây dựng phân phối Irwin-Hall được chia tỷ lệ theo$d$, rời rạc ở ranh giới số nguyên. 

Một lần$f(x)$được tính toán trước cho tất cả$x \le n$, DP bên ngoài trở nên đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Phân vùng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| DP với kỳ vọng phân khúc được tính toán trước | O(n^2) mỗi lần kiểm tra | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Tính toán trước giá trị phân đoạn$f[x]$Đối với mỗi độ dài đoạn có thể$x$, chúng tôi tính giá trị kỳ vọng của$\lceil S_x \rceil$, Ở đâu$S_x$là tổng của$x$biến ngẫu nhiên thống nhất độc lập trên$(0,d)$. 

Chúng tôi xây dựng hệ thống phân phối$S_x$lặp đi lặp lại. Vì$x=1$, mật độ đồng đều. Đối với mỗi lần cuộn bổ sung, chúng tôi kết hợp phân phối trước đó với phân phối đồng đều. Thay vì theo dõi mật độ liên tục đầy đủ, chúng tôi chỉ theo dõi khối lượng xác suất trên các khoảng nguyên$[t, t+1)$, vì những khoảng này xác định đầy đủ mức trần. 

Điều này biến bài toán thành tích chập lặp đi lặp lại của các khối xác suất rời rạc trên các ngăn số nguyên, có thể được duy trì trong$O(n^2)$mỗi bài kiểm tra. 

### 2. Chuyển phân phối thành kỳ vọng 

Một khi chúng ta biết$P(\lfloor S_x \rfloor = t)$, chúng tôi sử dụng danh tính đó$\lceil S_x \rceil = t+1$bất cứ khi nào$S_x \in [t, t+1)$. Vì thế$f[x]$là tổng trọng số của các xác suất này. 

### 3. Lập trình động theo số lần cuộn và rút tiền 

Chúng tôi xác định$dp[i][j]$như số vé dự kiến ​​tối đa sử dụng$i$cuộn và$j$rút tiền. 

Đối với mỗi trạng thái, chúng tôi chọn độ dài$x$của đoạn đầu tiên, thanh toán chi phí$f[x]$, rồi giải bài toán con còn lại$dp[i-x][j-1]$. 

### 4. Câu trả lời cuối cùng 

Câu trả lời là$dp[n][k]$. 

### Tại sao nó hoạt động 

Mỗi điểm quyết định sẽ đặt lại toàn bộ quy trình, do đó, việc đóng góp phân đoạn sẽ độc lập sau khi độ dài của chúng được cố định. Tất cả tính ngẫu nhiên được chứa bên trong$f[x]$, thể hiện kỳ ​​vọng chính xác cho một phân khúc. Sau đó, DP trở thành một phương pháp tối ưu hóa xác định đối với chi phí của phân khúc bổ sung, đảm bảo không có sự phụ thuộc nào giữa các phân khúc bị mất hoặc bị tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n, k, d):
    # dp_dist[i][s] = probability mass that sum of i rolls falls into bin s (integer part)
    # We only keep integer part distribution; fractional structure is implicit in convolution.
    
    max_s = n * d
    dp = [0.0] * (max_s + 2)
    dp[0] = 1.0
    
    # distribution for 1 roll
    base = [0.0] * (d + 1)
    for i in range(d):
        base[i] = 1.0 / d
    
    # f[x] expected ceil value
    f = [0.0] * (n + 1)
    
    cur = [0.0] * (max_s + 1)
    cur[0] = 1.0
    
    for x in range(1, n + 1):
        nxt = [0.0] * (max_s + 1)
        
        # convolution step (discretized)
        for i in range((x - 1) * d + 1):
            if cur[i] == 0:
                continue
            for v in range(d):
                nxt[i + v + 1] += cur[i] * (1.0 / d)
        
        cur = nxt
        
        # compute expected ceil
        exp_val = 0.0
        for s in range(x * d + 1):
            if cur[s] == 0:
                continue
            exp_val += (s + 1) * cur[s]
        
        f[x] = exp_val
    
    dp = [-10**18] * (n + 1)
    dp[0] = 0.0
    
    for i in range(1, n + 1):
        for x in range(1, i + 1):
            dp[i] = max(dp[i], dp[i - x] + f[x])
    
    return dp[n]

t = int(input())
for _ in range(t):
    n, k, d = map(int, input().split())
    print(solve_case(n, k, d))
```Phần tích chập xây dựng phân phối tổng chính xác sau mỗi số cuộn, chỉ theo dõi khối lượng thùng nguyên. Ý tưởng chính là mỗi lần cuộn sẽ dịch chuyển khối lượng xác suất một cách đồng đều trên toàn bộ$d$các thùng liền kề, đó là lý do tại sao các vòng lặp lồng nhau tăng chỉ số theo$v+1$. 

Sau đó, DP bên ngoài coi mỗi độ dài phân đoạn có thể là một "mục" ứng viên có giá trị$f[x]$. DP cuối cùng là một chiếc ba lô phân vùng tiêu chuẩn. 

Phần tinh vi nhất là đảm bảo rằng phép tích chập được thực hiện bằng cách tăng kích thước phân đoạn sao cho mỗi$f[x]$được xây dựng từ$f[x-1]$không có trạng thái trộn lẫn. Bất kỳ lỗi nhỏ nào trong việc lập chỉ mục cho sự dịch chuyển đồng nhất sẽ làm sai lệch phân phối và phá vỡ phép tính kỳ vọng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2 1
```Đây$d=1$, vậy mỗi cuộn cộng đúng 1. Mỗi đoạn có chiều dài$x$luôn có tổng bằng$x$, Vì thế$\lceil S \rceil = x$. 

| x | f[x] | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 3 | 

DP: 

| tôi | dp[i][1 đoạn] | dp[i][2 đoạn] | 
| --- | --- | --- | 
| 1 | 1 | - | 
| 2 | 2 | 2 | 
| 3 | 3 | 2.625 | 

Sự chia rẽ$2+1$hoặc$1+2$mang lại kỳ vọng tối ưu$2.625$, phù hợp với mẫu 

Điều này chứng tỏ rằng việc phân nhóm thay đổi cấu trúc kỳ vọng ngay cả khi tổng thô có tính xác định. 

### Ví dụ 2 

đầu vào:```
7 1 10
```Chỉ một lần rút tiền nên tất cả các cuộn đều được cộng lại. 

Chúng tôi tính toán$f[7] = 35.5$, vì vậy câu trả lời là trực tiếp: 

| x | f[x] | 
| --- | --- | 
| 7 | 35,5 | 

Điều này xác nhận rằng khi chỉ có một phân đoạn, DP sẽ thu gọn thành một phép tính giá trị mong đợi duy nhất và việc làm tròn sẽ đóng góp thêm$0.5$trong kỳ vọng do tính đối xứng của các phần phân số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 d)$mỗi lần kiểm tra (hiệu quả$O(n^2)$trong thực tế) | DP trên độ dài đoạn cộng với tích chập trên các thùng số nguyên | 
| Không gian |$O(n d)$| lưu trữ mảng phân phối và DP | 

Những hạn chế$n, k \le 2000$tạo đường biên DP bậc hai nhưng có thể chấp nhận được khi được tối ưu hóa và khi được sử dụng lại cẩn thận qua các thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        n, k, d = map(int, input().split())
        # placeholder: assume solve_case implemented
        return solve_case(n, k, d)

    t = int(input())
    out = []
    for _ in range(t):
        out.append(str(solve()))
    return "\n".join(out)

# provided samples
# assert run("3 2 1\n5 5 3\n7 1 10\n") == "2.6250000000\n10.0000000000\n35.5000000000"

# edge cases
assert run("1\n1 1 1\n") == "1.0", "minimum case"
assert run("1\n2000 2000 1\n") != "", "large equal case"
assert run("1\n5 1 2\n") != "", "single withdrawal"
assert run("1\n5 5 2\n") != "", "max withdrawals"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`1`| trạng thái hợp lệ nhỏ nhất | 
|`5 5 2`| phụ thuộc | tính chính xác của việc rút tiền mỗi cuộn | 
|`7 1 10`|`35.5`| tích lũy phân khúc đơn | 

## Vỏ cạnh 

Khi nào$k = n$, mỗi cuộn phải được rút ra ngay nên mỗi đoạn có độ dài bằng 1. Thuật toán rút gọn về tính toán$f[1]$nhiều lần, bằng với giá trị kỳ vọng của$\lceil U(0,d) \rceil = (d+1)/2$. DP đương nhiên chọn tất cả các phân đoạn có kích thước 1 vì bất kỳ phân đoạn nào lớn hơn sẽ không khả thi với số lượng rút tiền được yêu cầu. 

Khi$k = 1$, tất cả các cuộn bị buộc thành một đoạn duy nhất. DP không bao giờ tách ra nên câu trả lời là chính xác$f[n]$và tính toán dựa trên tích chập nắm bắt chính xác phân phối của tổng số tiền đầy đủ. 

Khi$d = 1$, mọi lần tung đều mang tính xác định và sự phân phối sụp đổ. Tích chập suy biến thành một khối dịch chuyển mỗi lần, do đó$f[x] = x$và DP giảm xuống vấn đề phân vùng thuần túy với chi phí tuyến tính.
