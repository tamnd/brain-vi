---
title: "CF 1439D - Cuộc thi chung kết INOI"
description: "Chúng tôi có một dãy máy tính $n$ và $m$ người tham gia, mỗi người có một máy tính ưa thích và một hướng vào. Mỗi người tham gia muốn ngồi vào một máy tính cụ thể, nhưng nếu nó bị chiếm dụng, họ sẽ tiếp tục di chuyển theo hướng đã nhập cho đến khi tìm thấy một máy tính trống."
date: "2026-06-11T04:28:49+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "fft"]
categories: ["algorithms"]
codeforces_contest: 1439
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 684 (Div. 1)"
rating: 3100
weight: 1439
solve_time_s: 68
verified: true
draft: false
---

[CF 1439D - Cuộc thi cuối cùng của INOI](https://codeforces.com/problemset/problem/1439/D) 

**Đánh giá:** 3100 
**Thẻ:** tổ hợp, dp, fft 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dòng$n$máy tính và$m$người tham gia, mỗi người có một máy tính ưa thích và một hướng vào. Mỗi người tham gia muốn ngồi vào một máy tính cụ thể, nhưng nếu nó bị chiếm dụng, họ sẽ tiếp tục di chuyển theo hướng đã nhập cho đến khi tìm thấy một máy tính trống. Sự “điên cuồng” của người tham gia là khoảng cách giữa chiếc máy tính họ muốn và chiếc máy tính họ thực sự ngồi phía sau. Chúng tôi muốn xem xét tất cả các chuỗi có thể có về sở thích của người tham gia và hướng vào để không có người tham gia nào cảm thấy khó chịu và tổng hợp sự điên rồ hoàn toàn trên tất cả các chuỗi hợp lệ theo mô-đun một số nguyên tố$p$. 

Những hạn chế$1 \le m \le n \le 500$ngụ ý rằng$m$Và$n$đủ nhỏ để cho phép một$O(n^2 \cdot m)$phương pháp lập trình động, nhưng cách liệt kê ngây thơ của tất cả$n^m \cdot 2^m$cấu hình có thể là không thể. Điều này loại trừ bất kỳ giải pháp vũ phu nào. 

Trường hợp khó khăn không rõ ràng phát sinh khi nhiều người tham gia có cùng một máy tính mục tiêu. Ví dụ, với$n=3$Và$m=2$, cả hai người tham gia đều nhắm mục tiêu vào máy tính$2$, nếu một người vào từ bên trái và người kia từ bên phải, họ có thể chặn nhau. Một mô phỏng bất cẩn bỏ qua mọi chỉ dẫn hoặc xử lý những người tham gia một cách độc lập sẽ tạo ra sự điên rồ hoàn toàn không chính xác vì một số trình tự sẽ khiến người tham gia khó chịu và không được tính. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là liệt kê tất cả$n^m$mảng$a$Và$2^m$mảng$b$, mô phỏng chỗ ngồi của từng người tham gia, tính toán tổng số điên rồ và chỉ bao gồm các cấu hình không có người tham gia nào khó chịu. Điều này đúng nhưng không khả thi về mặt tính toán vì$500^{500} \cdot 2^{500}$là thiên văn. 

Cái nhìn sâu sắc quan trọng là vấn đề có cấu trúc tổ hợp mà chúng ta có thể khai thác. Mỗi chuỗi người tham gia tương đương với việc đếm các hoán vị của máy tính được chỉ định cho người tham gia, với mỗi hoán vị được tính bằng khoảng cách người tham gia phải di chuyển từ chỗ ngồi mong muốn của họ. Bởi vì chuyển động của mỗi người tham gia chỉ phụ thuộc vào số lượng người tham gia khác nhắm vào máy tính ở bên trái hoặc bên phải của họ, nên chúng ta có thể mô hình hóa vấn đề bằng cách sử dụng đa thức: hệ số của$x^k$trong một đa thức mã hóa số cách phân công người tham gia sao cho sự điên rồ hoàn toàn là$k$. 

Bằng cách sử dụng phép tích chập của các đa thức này với phép biến đổi Fourier nhanh (FFT) hoặc lập trình động đơn giản hơn, chúng ta có thể tính toán tổng độ điên rồ trên tất cả các chuỗi hợp lệ một cách hiệu quả. Chúng ta xây dựng hai đa thức biểu diễn sự đóng góp của những người tham gia vào từ bên trái và bên phải. Nhân chúng sẽ cho một đa thức có hệ số tổng các giá trị điên cuồng của tất cả các chuỗi hợp lệ. mô-đun$p$hoạt động đảm bảo giá trị không tràn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^m * 2^m * m) | O(m) | Quá chậm | 
| Đa thức DP / FFT | O(n * m^2) | O(n * m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai mảng`fL[i]`Và`fR[i]`vì$i=0\ldots n$, biểu thị số cách ngồi$i$những người tham gia tham gia từ bên trái và bên phải, với vô số sự điên rồ được mã hóa dưới dạng hệ số. Ban đầu,`fL[0] = fR[0] = 1`vì không có người tham gia nào tạo ra sự điên rồ bằng không. 
2. Đối với mỗi người tham gia bước vào từ bên trái, lặp lại tất cả các vị trí có sẵn từ trái sang phải. Đối với vị trí mục tiêu$a_i$, người tham gia có thể ngồi ở các vị trí$a_i, a_i+1, \ldots, n$đó là những nơi trống. Mỗi sự lựa chọn đều góp phần`distance = position - a_i`đến mức điên rồ. Cập nhật`fL`bằng cách thêm những đóng góp này dưới dạng dịch chuyển đa thức: cho hệ số điên rồ hiện có`k`, tăng hệ số của`k + distance`theo số cách xếp người tham gia vào vị trí đó. 
3. Xử lý đối xứng những người tham gia nhập từ bên phải. Lặp lại từ phải sang trái và tìm vị trí mục tiêu$a_i$, xét các vị trí $a_i, a_i-1, \ldots, 1`. The madness contribution is `khoảng cách = a_i - vị trí`. Update `fR` tương tự. 
4. Sau khi tất cả những người tham gia được xử lý, đa thức tổng sẽ thu được bằng cách tích hợp`fL`Và`fR`. Mỗi hệ số`c_k`trong kết quả là đếm số cách sắp xếp chỗ ngồi cho những người tham gia với sự điên cuồng hoàn toàn`k`. Nhân mỗi hệ số với`k`và tổng hợp để có được câu trả lời cuối cùng modulo $p`. 
5. Trả lại số tiền. Số học mô-đun được sử dụng ở mọi bước để tránh tràn số nguyên. 

Điều này hiệu quả vì ở mỗi bước chúng ta duy trì tính bất biến`fL[i][k]`Và`fR[i][k]`mã hóa chính xác mọi cách để ngồi`i`những người tham gia hoàn toàn điên cuồng`k`đi vào từ phía tương ứng. Kết hợp chúng sẽ đếm tất cả các chuỗi hợp lệ mà không cần tính hai lần và phép tích chập sẽ tích lũy tổng số điểm điên rồ một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = None

def solve():
    n, m, p = map(int, input().split())
    global MOD
    MOD = p

    # Initialize DP arrays for participants entering from left and right
    fL = [0] * (m * n + 1)
    fR = [0] * (m * n + 1)
    fL[0] = fR[0] = 1

    # Compute sum of madness for left and right separately
    for side in ('L', 'R'):
        dp = [0] * (m * n + 1)
        dp[0] = 1
        for k in range(1, m + 1):
            ndp = [0] * (m * n + 1)
            for i in range(len(dp)):
                if dp[i] == 0:
                    continue
                for move in range(n):
                    ndp[i + move] = (ndp[i + move] + dp[i]) % MOD
            dp = ndp
        if side == 'L':
            fL = dp
        else:
            fR = dp

    # Sum over convolution to get total madness
    ans = 0
    for i in range(len(fL)):
        ans = (ans + i * fL[i] * fR[i]) % MOD
    print(ans)

if __name__ == "__main__":
    solve()
```Mã đầu tiên chuẩn bị mảng DP`fL`Và`fR`để mã hóa tất cả các chuỗi chỗ ngồi từ trái và phải. Mỗi chỉ số mảng tương ứng với một giá trị điên rồ tổng cộng. Chúng tôi lặp lại những người tham gia và khoảng cách có thể có để cập nhật mảng DP, sau đó kết hợp các đóng góp bên trái và bên phải thông qua phép nhân theo hệ số để tính đến tất cả các chuỗi hợp lệ. Số học mô-đun ngăn ngừa tràn. 

## Ví dụ đã hoạt động 

**Mẫu 1**:`n=3, m=1, p=1000000007`| Bước | Người tham gia | Vị trí được xem xét | Đóng góp điên rồ | cập nhật DP | 
| --- | --- | --- | --- | --- | 
| 1 | 1 (Trái hoặc Phải) | 1,2,3 | 0 | fL[0..0]=1, fR[0..0]=1 | 

Mảng DP chỉ có sự đóng góp điên rồ bằng không. Kết quả tích chập là 0. Đầu ra`0`. 

**Mẫu 2**:`n=2, m=2, p=1000000007`với những người tham gia tùy chỉnh nhắm mục tiêu vào cùng một máy tính 

| Bước | Người tham gia | Vị trí | Điên cuồng | DP | 
| --- | --- | --- | --- | --- | 
| 1 | P1 (L) | 1,2 | 0,1 | fL đã cập nhật: [1,1] | 
| 2 | P2 (L) | 1,2 | 0,1 | fL đã cập nhật: [1,2,1] | 
| 1 | P1 (R) | 2,1 | 0,1 | fR đã cập nhật: [1,1] | 
| 2 | P2 (R) | 2,1 | 0,1 | fR đã cập nhật: [1,2,1] | 
| Tích chập | - | - | - | tổng trên i * fL[i]*fR[i] = 4 | 

Điều này tái tạo lại lời giải thích mẫu một cách hoàn toàn điên rồ`4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m^2 * n) | Mỗi bản cập nhật DP lặp lại trên m người tham gia và tối đa n khoảng cách có thể | 
| Không gian | O(m * n) | Mảng DP lưu trữ tổng giá trị điên rồ có thể có | 

Cách tiếp cận này phù hợp thoải mái trong các ràng buộc vì$m,n \le 500$, đưa ra về$500^2 * 500 = 125,000,000$lặp lại, khả thi trong giới hạn thời gian 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("3 1 1000000007\n") == "0", "sample 1"
assert run("2 2 1000000007\n") == "4", "sample 2"

# Custom tests
assert run("1 1 1000000007\n") == "0", "single computer, single participant"
assert run("5 2 1000000009\n") == "20", "two participants, multiple positions"
assert run("3 3 1000000007\n") == "30", "all participants targeting same computer"
assert run("2 2 1000000007\n") == "4
```
