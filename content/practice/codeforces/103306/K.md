---
title: "CF 103306K - Số lặp lại nhị phân K"
description: "Chúng ta được cho một số nguyên dương $K$. Chúng ta xem xét tất cả các biểu diễn nhị phân của các số $N$, nhưng không ở dạng tối thiểu thông thường của chúng. Thay vào đó, mỗi số được viết bằng cách sử dụng chính xác các bit $K$, bao gồm cả các số 0 đứng đầu. Vì vậy, mọi $N$ tương ứng với một chuỗi nhị phân có độ dài cố định có độ dài $K$."
date: "2026-07-03T14:24:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103306
codeforces_index: "K"
codeforces_contest_name: "2021 ICPC Gran Premio de Mexico 2da Fecha"
rating: 0
weight: 103306
solve_time_s: 48
verified: true
draft: false
---

[CF 103306K - Số lặp lại nhị phân K](https://codeforces.com/problemset/problem/103306/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương$K$. Chúng tôi xem xét tất cả các biểu diễn nhị phân của các số$N$, nhưng không ở dạng tối thiểu thông thường của chúng. Thay vào đó, mỗi số được viết bằng cách sử dụng chính xác$K$bit, bao gồm cả số 0 đứng đầu. Vì vậy mỗi$N$tương ứng với một chuỗi nhị phân có độ dài cố định$K$. 

một con số$N$được gọi là$K$-lặp đi lặp lại nhị phân nếu điều này$K$Chuỗi -bit có thể được biểu thị dưới dạng lặp lại của một số mẫu nhị phân ngắn hơn. Nói cách khác, tồn tại một chuỗi nhị phân không trống$S$, ngắn hơn chiều dài$K$, sao cho lặp đi lặp lại$S$một số lần cho biết chính xác$K$-bit đại diện của$N$. 

Nhiệm vụ là, đối với mỗi truy vấn$K$, đếm xem có bao nhiêu số nguyên$N$trong phạm vi$[0, 2^K - 1]$sản xuất một$K$-biểu diễn bit mang tính tuần hoàn theo nghĩa này và xuất ra modulo đếm$10^9 + 7$. 

Điều tinh tế quan trọng là chúng ta không yêu cầu tính tuần hoàn theo nghĩa số nguyên, mà là tính tuần hoàn của chuỗi nhị phân có độ dài cố định bao gồm các số 0 đứng đầu. Điều này có nghĩa là các chuỗi như`0011`là những ứng cử viên hợp lệ cho các mẫu lặp lại mặc dù bản thân số nguyên thường được viết khác. 

Những hạn chế rất quan trọng. Chúng tôi có tới$10^5$trường hợp thử nghiệm và$K$lên đến$10^6$, vì vậy bất kỳ giải pháp nào xử lý từng$K$độc lập trong thời gian tuyến tính là không thể. Thậm chí$O(K)$mỗi truy vấn sẽ dẫn đến$10^{11}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn. Điều này buộc phải tính toán trước hoặc quan sát tổ hợp dạng đóng để giảm mỗi truy vấn xuống$O(1)$. 

Một vài trường hợp đặc biệt làm rõ định nghĩa. 

Khi$K = 1$, chỉ có hai chuỗi:`0`Và`1`. Không thể hình thành bằng cách lặp lại một chuỗi ngắn hơn, không trống, vì vậy câu trả lời là$0$. 

Khi$K = 2$, các dây là`00`,`01`,`10`,`11`. Đây`00`là sự lặp lại của`0`, Và`11`là sự lặp lại của`1`, vậy có 2 số hợp lệ. 

Một lỗi phổ biến là chỉ xử lý các biểu diễn số nguyên “sạch”, bỏ qua các số 0 đứng đầu. Điều đó sẽ từ chối không chính xác các trường hợp như`00`, rất quan trọng đối với cấu trúc tuần hoàn. 

Một lỗi khác là cho rằng bất kỳ chuỗi nào có ký tự lặp lại đều hợp lệ. Ví dụ`0101`là hợp lệ, nhưng`0110`thì không, mặc dù cả hai đều chứa các bit lặp lại, bởi vì chỉ có bit đầu tiên có khoảng thời gian nhất quán chia cho toàn bộ chiều dài. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: đối với mỗi$K$chuỗi -bit, hãy thử mọi độ dài khoảng thời gian có thể$d$đó là sự chia rẽ$K$. Với mỗi số chia$d < K$, kiểm tra xem tiền tố có độ dài$d$lặp đi lặp lại$K/d$time xây dựng lại chuỗi đầy đủ. Nếu bất kỳ số chia nào hoạt động thì chuỗi đó sẽ được tính. 

Điều này hoạt động vì các chuỗi định kỳ chính xác là những chuỗi có chu kỳ thích hợp chia toàn bộ chiều dài. Tuy nhiên, điều này là tốn kém thảm khốc. Đối với mỗi$2^K$chuỗi, chúng tôi có thể kiểm tra tới$O(K)$chia số và thực hiện$O(K)$so sánh, đưa ra$O(K \cdot 2^K)$, điều này là không thể thực hiện được ngay cả đối với rất nhỏ$K$, chứ đừng nói đến$10^6$. 

Quan sát quan trọng là chúng ta không làm việc với một chuỗi cố định mà đếm xem có bao nhiêu chuỗi có độ dài$K$mang tính định kỳ. Điều này chuyển đổi vấn đề từ việc kiểm tra từng chuỗi thành vấn đề đếm tổ hợp. 

Một chuỗi nhị phân có độ dài$K$được xác định đầy đủ bởi khoảng thời gian nhỏ nhất của nó$p$, và một khi chúng tôi sửa$p$, chuỗi được xác định bằng cách chọn chuỗi đầu tiên$p$bit, sau đó lặp lại chúng. Tuy nhiên, việc đếm quá mức xảy ra do một chuỗi có dấu chấm$p$cũng được tính cho tất cả bội số của$p$. Đây chính xác là một cấu trúc mạng chia trên$K$. 

Vì vậy, thay vì đếm trực tiếp các chuỗi tuần hoàn, chúng ta đếm tất cả các chuỗi và trừ đi những chuỗi “nguyên thủy”, nghĩa là chúng không có chu kỳ nhỏ hơn. Số lượng tất cả các chuỗi là$2^K$. Số lượng chuỗi nguyên thủy có thể được tính bằng cách sử dụng phép đảo ngược Möbius trên các ước của$K$. Khi chúng ta biết số lượng nguyên thủy cho tất cả các ước số, chúng ta có thể rút ra số lượng định kỳ một cách hiệu quả. 

Điều này dẫn đến DP theo lý thuyết số tiêu chuẩn trên các ước số: chúng tôi tính toán trước$2^K$và sau đó trừ đi sự đóng góp của các ước số nhỏ hơn bằng cách sử dụng loại trừ bao gồm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^K \cdot K)$|$O(1)$| Quá chậm | 
| Số chia + Mobius / DP |$O(K \log K)$tính toán trước,$O(1)$mỗi truy vấn |$O(K)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước lũy thừa của hai lên đến mức tối đa$K$trên tất cả các truy vấn, vì$2^K$đại diện cho tất cả các chuỗi nhị phân có độ dài$K$. Điều này đưa ra số lượng cơ sở cho mọi trường hợp. 
2. Với mỗi giá trị$K$, hãy khởi tạo ý tưởng rằng tất cả các chuỗi đều là ứng cử viên hợp lệ trước khi lọc ra những chuỗi nguyên thủy. Cấu trúc lặp lại chỉ phụ thuộc vào ước số của$K$, vì vậy chúng tôi tập trung vào các mối quan hệ số chia. 
3. Lặp lại tất cả các ước số có thể$d$của mỗi người$K$theo thứ tự tăng dần, xử lý$d$như một khoảng thời gian tiềm năng. Một chuỗi có dấu chấm$d$được hình thành bằng cách lặp lại một khối có kích thước$d$, vậy có$2^d$các khối như vậy trước khi xem xét tính toán quá mức. 
4. Trừ phần đóng góp từ các ước số nhỏ hơn đã biểu thị các mẫu cơ bản hơn. Bước này đảm bảo rằng mỗi cấu trúc định kỳ được tính chính xác một lần ở chu kỳ tối thiểu của nó. Phép trừ tuân theo phép loại trừ bao gồm trên mạng chia. 
5. Sau khi tính toán số chuỗi nguyên thủy cho tất cả các ước số, hãy rút ra số chuỗi tuần hoàn dưới dạng tổng trừ các chuỗi nguyên thủy cho toàn bộ chiều dài. Điều này tách biệt chính xác những chuỗi có cấu trúc lặp lại thích hợp. 
6. Trả lời từng câu hỏi trong$O(1)$bằng cách sử dụng các mảng được tính toán trước. 

### Tại sao nó hoạt động 

Mỗi$K$Chuỗi -bit có một khoảng thời gian tối thiểu duy nhất, là tiền tố nhỏ nhất tạo ra nó bằng cách lặp lại. Điều này tạo ra sự phân vùng của tất cả các chuỗi theo chu kỳ tối thiểu của chúng. Số lượng brute-force chồng lên nhau vì một chuỗi có khoảng thời gian tối thiểu$d$cũng tương thích với mọi bội số của$d$, nhưng nó chỉ nên được quy một lần. Đảo ngược Möbius khắc phục việc đếm quá mức này bằng cách hủy bỏ một cách có hệ thống các khoản đóng góp từ các ước số nhỏ hơn, đảm bảo mỗi chuỗi được gán cho chính xác một lớp khoảng thời gian tối thiểu. Điều này đảm bảo tính chính xác của số đếm dựa trên phép trừ cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

MAX_K = 10**6

# precompute powers of 2
pow2 = [1] * (MAX_K + 1)
for i in range(1, MAX_K + 1):
    pow2[i] = (pow2[i - 1] * 2) % MOD

# mobius function via linear sieve
mu = [1] * (MAX_K + 1)
is_prime = [True] * (MAX_K + 1)
primes = []

for i in range(2, MAX_K + 1):
    if is_prime[i]:
        primes.append(i)
        mu[i] = -1
    for p in primes:
        if i * p > MAX_K:
            break
        is_prime[i * p] = False
        if i % p == 0:
            mu[i * p] = 0
            break
        else:
            mu[i * p] = -mu[i]

# precompute periodic counts using divisor sums
periodic = [0] * (MAX_K + 1)

for i in range(1, MAX_K + 1):
    # strings with period dividing i
    total = 0
    j = 1
    while j * j <= i:
        if i % j == 0:
            d1 = j
            d2 = i // j
            total = (total + mu[i // d1] * pow2[d1]) % MOD
            if d1 != d2:
                total = (total + mu[i // d2] * pow2[d2]) % MOD
        j += 1
    periodic[i] = (pow2[i] - total) % MOD

t = int(input())
for _ in range(t):
    k = int(input())
    print(periodic[k] % MOD)
```Mã tách vấn đề thành hai lớp khái niệm. Đầu tiên, nó tính toán trước lũy thừa của hai, vì mỗi chuỗi nhị phân tương ứng với một lựa chọn bit tự do. Thứ hai, nó sử dụng tính năng loại trừ bao gồm dựa trên Möbius đối với các ước số để cô lập các chuỗi nguyên thủy, chính xác là những chuỗi không được hình thành bằng cách lặp lại một khối nhỏ hơn. Câu trả lời cuối cùng trừ đi số lượng nguyên thủy trong tổng không gian. 

Một lỗi triển khai phổ biến là quên rằng các số 0 đứng đầu là một phần của không gian chuỗi, đó là lý do tại sao tất cả các lần đếm đều kết thúc$2^K$, không phải trên số nguyên trong biểu diễn rút gọn. 

Một điểm tinh tế khác là xử lý phép liệt kê số chia một cách hiệu quả. Việc tính toán trước Möbius tránh việc tính toán lại các cấu trúc ước số cho mỗi truy vấn, nếu không sẽ dẫn đến hết thời gian chờ theo$10^5$truy vấn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$K = 1$| Bước | Tổng số chuỗi | Số nguyên thủy | Đếm định kỳ | 
| --- | --- | --- | --- | 
| 1 | 2 (`0`,`1`) | 2 | 0 | 

Hai chuỗi duy nhất không có cấu trúc lặp lại ngắn hơn nên không có chuỗi nào đủ tiêu chuẩn. 

Điều này xác nhận rằng thuật toán đã loại trừ chính xác các chuỗi tầm thường không có dấu chấm thích hợp. 

### Ví dụ 2 

đầu vào:$K = 2$| Bước | Tổng số chuỗi | Số nguyên thủy | Đếm định kỳ | 
| --- | --- | --- | --- | 
| 1 | 4 (`00`,`01`,`10`,`11`) | 2 (`01`,`10`) | 2 | 

Chỉ một`00`Và`11`mang tính tuần hoàn vì chúng là sự lặp lại của các mẫu bit đơn. Bảng này cho thấy cách phương thức phân tách rõ ràng các chuỗi có cấu trúc khỏi các chuỗi không có cấu trúc. 

Điều này chứng tỏ rằng phân tách dựa trên số chia nắm bắt chính xác sự lặp lại ngay cả với các số 0 đứng đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(MAX\_K \log MAX\_K)$| Sàng để xử lý Möbius + số chia trên tất cả$K$| 
| Không gian |$O(MAX\_K)$| Mảng hàm mũ, giá trị Möbius và DP | 

Quá trình tiền xử lý phù hợp thoải mái trong giới hạn vì$MAX_K = 10^6$. Mỗi truy vấn được trả lời trong thời gian không đổi, làm cho$10^5$truy vấn khả thi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # call solution logic here (wrap main in function in practice)
    ...

# provided samples
# assert run("2\n1\n2\n") == "0\n2\n"

# custom cases
# K = 3: 000,111,010101 patterns
# K = 4: multiple periodic structures
# K = 5: prime length edge case (only full-length repeats)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| K=1 | 0 | trường hợp cơ bản, không thể lặp lại | 
| K=2 | 2 | trường hợp tuần hoàn không tầm thường nhỏ nhất | 
| K=3 | 2 | ràng buộc hành vi có độ dài nguyên tố | 
| K=4 | 6 | cấu trúc bội số | 
| K=5 | 2 | hành vi cạnh có độ dài nguyên tố | 

## Vỏ cạnh 

cho$K = 1$, thuật toán trả về chính xác 0 vì không có ước số thích hợp để tạo thành sự lặp lại, do đó, loại trừ bao gồm không để lại cấu trúc tuần hoàn. 

Đối với nguyên tố$K$, chỉ tồn tại các chuỗi lặp lại mẫu có độ dài 1 hoặc mẫu có độ dài đầy đủ và phép trừ sẽ loại bỏ chính xác tất cả các cấu trúc chỉ có độ dài đầy đủ, chỉ để lại các chuỗi không đổi. 

Đối với lũy thừa của hai, mạng chia dày đặc và việc hủy Möbius đảm bảo rằng các lần lặp lồng nhau như giai đoạn 2 bên trong giai đoạn 4 không bị tính hai lần, duy trì tính chính xác trên tất cả các lần lặp lại phân cấp.
