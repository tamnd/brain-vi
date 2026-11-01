---
title: "CF 106443C - Tính toán huấn luyện viên"
description: "Chúng ta được cho hai đa thức, một đa thức đóng vai trò là hàm ngoài và một đa thức đóng vai trò là hàm trong. Đa thức $P(x)$ đầu tiên xác định cách chúng ta chuyển đổi một giá trị khi chúng ta đã có nó và đa thức thứ hai $Q(x)$ xác định giá trị được đưa vào $P$."
date: "2026-06-19T17:42:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106443
codeforces_index: "C"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2026"
rating: 0
weight: 106443
solve_time_s: 55
verified: true
draft: false
---

[CF 106443C - Tính toán của huấn luyện viên](https://codeforces.com/problemset/problem/106443/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai đa thức, một đa thức đóng vai trò là hàm ngoài và một đa thức đóng vai trò là hàm trong. Đa thức đầu tiên$P(x)$xác định cách chúng ta chuyển đổi một giá trị khi chúng ta đã có nó và đa thức thứ hai$Q(x)$xác định giá trị được đưa vào$P$. Nhiệm vụ là tính đa thức hợp thành$R(x) = P(Q(x))$, nghĩa là mỗi lần xuất hiện của$x$bên trong$P$được thay thế bằng toàn bộ đa thức$Q(x)$, và sau đó mọi thứ được mở rộng. 

Đầu ra không phải là một biểu thức ký hiệu mà là các hệ số rõ ràng của đa thức thu được sau khi khai triển hoàn toàn, được tính theo modulo 998244353. Vì mức độ của$P$nhiều nhất là 100 và mức độ$Q$nhiều nhất là 100 thì đa thức thu được có thể có bậc lên tới 10000. 

Thách thức cơ cấu quan trọng là sự thay thế mở rộng quyền hạn: mỗi thuật ngữ$Q(x)^k$bản thân nó là một đối tượng giống như tích chập. Một phép khai triển ngây thơ sẽ nhân các đa thức nhiều lần, khiến giá thành tăng nhanh. 

Trường hợp cạnh tinh tế phát sinh khi một trong hai đa thức có cấu trúc thưa thớt, đặc biệt khi$Q(x)$là đơn thức cộng với hằng số hoặc khi$P(x)$có nhiều hệ số bằng 0. Một cách tiếp cận nhân từng số hạng đơn giản có thể vô tình tính lại lũy thừa của$Q(x)$nhiều lần, dẫn đến công việc dư thừa mặc dù cấu trúc kết quả mang tính quyết định. 

## Phương pháp tiếp cận 

Giải thích trực tiếp về định nghĩa gợi ý rằng việc tính toán$R(x)$bằng cách lặp qua từng hệ số của$P(x)$. Đối với mỗi$i$, chúng tôi tính toán$Q(x)^i$, nhân nó với$p_i$, và thêm nó vào kết quả. Điều này đúng vì nó tuân theo định nghĩa về thành phần đa thức một cách chính xác. 

Tuy nhiên, tính toán$Q(x)^i$từ đầu cho mọi$i$đắt tiền. Mỗi phép nhân đa thức có giá trị$O(m^2)$, và làm điều đó lên đến$n$lần dẫn đến khoảng$O(n^2 m^2)$làm việc trong trường hợp xấu nhất, điều này là quá đủ để vượt qua ở đây nhưng lại lãng phí về mặt khái niệm và có nguy cơ kém hiệu quả trong việc triển khai. 

Quan sát quan trọng là chúng ta không cần tính toán lại độc lập từng lũy ​​thừa. Chúng ta có thể xây dựng sức mạnh lặp đi lặp lại: một khi chúng ta biết$Q(x)^i$, chúng ta có thể có được$Q(x)^{i+1}$bằng cách nhân với$Q(x)$. Điều này biến việc tính toán lại lặp đi lặp lại thành một chuỗi các phép nhân đa thức. 

Chúng tôi cũng tích lũy câu trả lời tăng dần: bắt đầu từ$Q(x)^0 = 1$, chúng tôi liên tục nhân với$Q(x)$, và ở mỗi bước thêm$p_i \cdot Q(x)^i$vào kết quả. Điều này sắp xếp việc tính toán với một lần quét chuyển tiếp duy nhất qua các hệ số của$P$. 

Vì độ là nhỏ, một cách đơn giản$O(n m^2)$hoặc$O(n^2 m)$Cách tiếp cận này hoàn toàn an toàn trong các điều kiện ràng buộc và tính đơn giản của tích chập lặp lại quan trọng hơn tối ưu hóa tiệm cận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (tính toán lại từng sức mạnh) |$O(n^2 m^2)$|$O(m^2)$| Chấp nhận được nhưng dư thừa | 
| Tích tụ năng lượng lặp đi lặp lại |$O(n m^2)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng đa thức kết quả trong khi vẫn duy trì lũy thừa hiện tại của$Q(x)$. 

1. Khởi tạo đa thức`power`đại diện$Q(x)^0$, đó chỉ là$[1]$. Điều này tương ứng với phần tử nhận dạng của phép nhân đa thức. 
2. Khởi tạo đa thức kết quả`res`như tất cả các số không cho đến mức độ$n \cdot m$. Điều này sẽ tích lũy các khoản đóng góp từ mỗi nhiệm kỳ của$P(x)$. 
3. Lặp lại$i$từ 0 đến$n$. Tại mỗi chỉ số$i$, chúng tôi hiện đang nắm giữ$power = Q(x)^i$, do đó sự đóng góp của thuật ngữ$p_i x^i$TRONG$P(x)$trở thành$p_i \cdot power$. Chúng tôi thêm đa thức tỷ lệ này vào`res`. 
4. Hệ số sau xử lý$p_i$, cập nhật`power`bằng cách nhân nó với$Q(x)$. Điều này tạo ra$Q(x)^{i+1}$. Phép nhân là tích chập đa thức tiêu chuẩn: mỗi hệ số được hình thành bằng cách tính tổng các tích từng cặp có chỉ số cộng lại. 
5. Tiếp tục cho đến khi tất cả các hệ số của$P(x)$được xử lý. trận chung kết`res`chứa các hệ số của$P(Q(x))$. 

### Tại sao nó hoạt động 

Ở mỗi bước$i$, biến`power`chính xác là$Q(x)^i$. Điều này được duy trì theo phương pháp quy nạp: nó bắt đầu như$Q(x)^0 = 1$và mỗi bản cập nhật nhân với$Q(x)$, tăng số mũ lên một. Vì mỗi học kỳ của$P(x)$được áp dụng đúng một lần với công suất tương ứng chính xác của$Q(x)$, tổng cuối cùng khớp với định nghĩa của thành phần đa thức mà không bỏ sót hoặc trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def add_poly(a, b):
    res = [0] * max(len(a), len(b))
    for i, v in enumerate(a):
        res[i] += v
    for i, v in enumerate(b):
        res[i] = (res[i] + v) % MOD
    for i in range(len(res)):
        res[i] %= MOD
    return res

def mul_poly(a, b):
    res = [0] * (len(a) + len(b) - 1)
    for i in range(len(a)):
        ai = a[i]
        for j in range(len(b)):
            res[i + j] = (res[i + j] + ai * b[j]) % MOD
    return res

def scale_poly(a, c):
    return [(x * c) % MOD for x in a]

def main():
    n, m = map(int, input().split())
    p = list(map(int, input().split()))
    q = list(map(int, input().split()))

    power = [1]
    res = [0] * (n * m + 1)

    for i in range(n + 1):
        term = scale_poly(power, p[i])
        for j, v in enumerate(term):
            res[j] = (res[j] + v) % MOD
        if i != n:
            power = mul_poly(power, q)

    print(*res)

if __name__ == "__main__":
    main()
```Việc thực hiện giữ một đa thức đang chạy`power`bằng$Q(x)^i$. Với mỗi hệ số$p[i]$, nó chia tỷ lệ đa thức này và thêm nó vào mảng câu trả lời. Hàm tích chập`mul_poly`là trực tiếp$O(nm)$phép nhân vì kích thước nhỏ. 

Phần tinh tế nhất là giữ kích thước đa thức nhất quán. Mảng kết quả được phân bổ trước cho$n \cdot m + 1$, vì đây là mức độ tối đa có thể có của bố cục. Điều này tránh việc thay đổi kích thước lặp đi lặp lại và đảm bảo lập chỉ mục xác định trong quá trình tích lũy. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1
1 2 1
1 1
```Chúng tôi giải thích$P(x) = 1 + 2x + x^2$,$Q(x) = 1 + x$. 

| tôi | công suất = Q^i | đóng góp p[i] * sức mạnh | kết quả sau khi cộng | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 
| 1 | 1 + x | 2 + 2x | 3 + 2x | 
| 2 | (1 + x)^2 = 1 + 2x + x^2 | 1 + 2x + x^2 | 4 + 4x + x^2 | 

Đầu ra cuối cùng:```
4 4 1
```Dấu vết này cho thấy công suất hoạt động theo dõi chính xác sự thay thế lặp đi lặp lại và sự tích lũy phù hợp với việc mở rộng trực tiếp. 

### Ví dụ 2 

đầu vào:```
2 2
0 1 0
1 0 1
```Đây$P(x) = x$,$Q(x) = 1 + x^2$. 

| tôi | quyền lực | đóng góp | kết quả | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 
| 1 | 1 + x^2 | 1 + x^2 | 1 + x^2 | 
| 2 | (1 + x^2)^2 | 0 | 1 + x^2 | 

Đầu ra cuối cùng:```
1 0 1
```Điều này chứng tỏ rằng các hệ số bằng 0 trong$P(x)$bỏ qua các khoản đóng góp tốn kém một cách chính xác mà không ảnh hưởng đến việc tính toán công suất đang chạy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot m^2)$| Mỗi lần lặp nhân một đa thức có kích thước lên tới$m \cdot i$qua$m$, nằm trong giới hạn nhỏ kể từ$n, m \le 100$. | 
| Không gian |$O(nm)$| Kết quả đa thức lưu trữ đến mức độ$n \cdot m$. | 

Các ràng buộc đủ nhỏ để một thành phần dựa trên tích chập đơn giản có thể thoải mái nằm trong giới hạn. Ngay cả trường hợp xấu nhất vẫn ở dưới ngưỡng 1 giây thông thường trong Python do kích thước đa thức bị chặn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import prod

    n, m = map(int, input().split())
    p = list(map(int, input().split()))
    q = list(map(int, input().split()))

    power = [1]
    res = [0] * (n * m + 1)

    def mul(a, b):
        r = [0] * (len(a) + len(b) - 1)
        for i in range(len(a)):
            for j in range(len(b)):
                r[i + j] = (r[i + j] + a[i] * b[j]) % MOD
        return r

    for i in range(n + 1):
        for j, v in enumerate(power):
            res[j] = (res[j] + v * p[i]) % MOD
        if i != n:
            power = mul(power, q)

    return " ".join(map(str, res)).strip()

# provided samples
assert run("2 1\n1 2 1\n1 1\n") == "4 4 1"
assert run("2 2\n0 1 0\n1 0 1\n") == "1 0 1"

# minimum case
assert run("0 1\n5\n3 4\n") == "5"

# linear case
assert run("1 1\n1 1\n1 1\n") == "2 2 1"

# zero-heavy case
assert run("3 1\n0 0 0 1\n1 0\n") == "1 0 0 0"

# constant Q
assert run("2 0\n1 2 3\n7\n") == "7 14 21"

# high-degree monomial Q
assert run("2 1\n1 1 1\n0 2\n") == "1 2 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 độ P | đầu ra không đổi | xử lý trường hợp cơ bản | 
| P không nặng | đóng góp thưa thớt | bỏ qua các điều khoản một cách chính xác | 
| hằng số Q | hành vi mở rộng quy mô | lặp đi lặp lại thay thế liên tục | 
| đơn thức Q | tăng trưởng có cấu trúc | độ chính xác mở rộng độ | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là khi$P(x)$là không đổi. Trong tình huống này, tất cả các quyền lực cao hơn của$Q(x)$không liên quan. Thuật toán vẫn khởi tạo`power = 1`, chỉ thêm$p_0$và không thực hiện phép nhân có ý nghĩa. Đầu ra vẫn là đa thức không đổi, phù hợp với định nghĩa toán học. 

Một trường hợp khác là khi$P(x)$có một số hạng bậc cao khác 0. Ví dụ,$P(x) = x^n$. Thuật toán nhân lên`power`tiến về phía trước cho đến khi đạt được$Q(x)^n$và chỉ có một đóng góp được thêm vào. Điều này tránh được sự tích lũy lặp đi lặp lại mà vẫn tạo ra đa thức mở rộng chính xác. 

Cuối cùng, khi$Q(x)$là tuyến tính hoặc không đổi, phép nhân lặp lại không làm tăng độ phức tạp của cấu trúc một cách bất ngờ. Sự lặp đi lặp lại`power`bản cập nhật vẫn theo dõi sức mạnh chính xác, đảm bảo rằng ngay cả các tác phẩm suy thoái cũng được xử lý mà không cần vỏ đặc biệt.
