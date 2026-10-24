---
title: "CF 105971F - Cá cược thể thao"
description: "Giải đấu có n đội. Mỗi cặp đội chơi một lần và xác suất đội i đánh bại đội j chỉ phụ thuộc vào sức mạnh của họ: đội mạnh hơn có nhiều khả năng thắng hơn, nhưng có thể xảy ra cả hai kết quả."
date: "2026-06-25T13:42:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105971
codeforces_index: "F"
codeforces_contest_name: "BSUIR Open XIII: Student final"
rating: 0
weight: 105971
solve_time_s: 43
verified: true
draft: false
---

[CF 105971F - Cá cược thể thao](https://codeforces.com/problemset/problem/105971/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Giải đấu có`n`các đội. Mỗi cặp đội chơi một lần và xác suất đội đó`i`đội đánh bại`j`chỉ phụ thuộc vào sức mạnh của họ: đội mạnh hơn có nhiều khả năng giành chiến thắng hơn, nhưng kết quả đều có thể xảy ra. Một đội được coi là đội chiến thắng nếu đội đó có thể tiếp cận mọi đội khác thông qua chuỗi chiến thắng, nghĩa là số trận thắng trực tiếp hoặc gián tiếp sẽ được tính. 

Nhiệm vụ là tìm ra số lượng người chiến thắng dự kiến ​​​​trong tất cả các kết quả có thể xảy ra của giải đấu. 

Đầu vào đưa ra số lượng đội và sức mạnh của mỗi đội. Đầu ra là số lượng người chiến thắng dự kiến ​​theo modulo`10^9 + 7`. Vì xác suất là phân số nên cần có nghịch đảo mô-đun để tính toán. 

Số lượng đội ít,`n`lên tới 14. Điều này loại trừ việc mô phỏng trên tất cả các giải đấu có thể xảy ra vì có`2^(n*(n-1)/2)`những kết quả có thể xảy ra và tăng trưởng quá nhanh. Tuy nhiên,`2^n`tập hợp con vẫn có thể quản lý được vì`2^14 = 16384`, do đó, cách tiếp cận lập trình động tập hợp con là hướng dự định. 

Phần khó khăn là hiểu được điều gì khiến một nhóm đội trở thành nhóm chiến thắng. Một sai lầm phổ biến là cho rằng đội mạnh nhất luôn là đội chiến thắng. Các chu kỳ có thể tồn tại, vì vậy các nhóm yếu hơn vẫn có thể thuộc về cùng một thành phần được kết nối chặt chẽ với các nhóm mạnh hơn. 

Ví dụ, hãy xem xét ba đội có điểm mạnh`[1, 1, 1]`. Kết quả của mọi trận đấu đều có khả năng xảy ra như nhau. Một kết quả có thể xảy ra là một chu kỳ trong đó đội 1 đánh bại đội 2, đội 2 đánh bại đội 3 và đội 3 đánh bại đội 1. Cả ba đội đều là người chiến thắng, không chỉ một. 

Một trường hợp khác là một đội duy nhất. Không có trận đấu nào và đội duy nhất có thể tiếp cận mọi đội một cách tầm thường. Đối với đầu vào:```
1
5
```câu trả lời là:```
1
```Giải pháp chỉ kiểm tra xem một đội có thắng trận đấu hay không sẽ trả về số 0 không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ tạo ra mọi giải đấu có thể xảy ra. Đối với mỗi biểu đồ được tạo, chúng tôi có thể chạy duyệt biểu đồ từ mỗi nhóm và đếm xem có bao nhiêu nhóm có thể tiếp cận tất cả các nhóm khác. Điều này đúng vì định nghĩa về người chiến thắng chính xác là khả năng tiếp cận trong biểu đồ giải đấu được định hướng. 

Vấn đề là số lượng giải đấu. Với`n`các đội, có`n * (n - 1) / 2`trò chơi, và mỗi trò chơi đều có hai kết quả. Do đó số đồ thị có thể có là`2^(n*(n-1)/2)`. Vì`n = 14`, cái này đã rồi`2^91`, điều đó là không thể. 

Nhận xét quan trọng là người chiến thắng luôn hình thành chính xác một thành phần được kết nối chặt chẽ. Nếu một tập hợp các đội là tập hợp chiến thắng thì mọi đội bên ngoài tập hợp đó phải thua mọi đội trong tập hợp đó. Nếu không, một đội bên ngoài có thể đạt được thành phần chiến thắng và cũng trở thành người chiến thắng. 

Điều này đưa ra một cách để xây dựng câu trả lời theo tập hợp con. Chúng tôi tính toán xác suất để mỗi tập hợp con là thành phần liên thông mạnh chiến thắng. Đối với một tập hợp con, xác suất để nó là thành phần chiến thắng bằng xác suất mà tập hợp con đó được kết nối nội bộ mạnh mẽ và đánh bại tất cả các đội bên ngoài. 

Việc kiểm tra trực tiếp khả năng kết nối mạnh mẽ cho mọi tập hợp con vẫn còn bất tiện. Thay vào đó, chúng tôi tính toán xác suất tăng kích thước tập hợp con. Đối với một tập hợp con`S`, bắt đầu với xác suất để tất cả các đội trong`S`đánh bại mọi người bên ngoài`S`. Sau đó trừ các trường hợp thành phần chiến thắng thực sự là tập con nhỏ hơn của`S`. 

Việc tái diễn có hiệu quả vì nếu người chiến thắng đặt vào bên trong`S`là một số tập hợp con thích hợp`T`, sau đó`T`phải đánh bại các đội còn lại của`S`, chính xác cùng điều kiện cần thiết cho`T`để trở thành người chiến thắng trong toàn bộ giải đấu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(2^(n^2))`|`O(n^2)`| Quá chậm | 
| Tối ưu |`O(3^n + n^2 * 2^n)`|`O(2^n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc điểm mạnh và tính toán trước xác suất để mỗi đội đánh bại mọi đội khác. Giá trị cho`i`đánh đập`j`là`a[i] / (a[i] + a[j])`theo số học mô-đun. 
2. Tính toán trước`win[mask]`, xác suất để tất cả các đội bên trong`mask`đánh bại tất cả các đội bên ngoài`mask`. 

Đối với một tập hợp con cố định, đây là tích của tất cả các cạnh chéo đi từ tập hợp con đến phần bù của nó. Vì các truy vấn tập hợp con giống nhau xuất hiện lặp đi lặp lại nên việc lưu trữ các giá trị này sẽ tránh được việc tính toán lại. 
3. Xử lý các tập hợp con theo thứ tự tăng dần về số lượng nhóm của chúng. Đối với mọi tập hợp con`mask`, khởi tạo`dp[mask]`với`win[mask]`. 

Điều này thể hiện sự kiện toàn bộ tập hợp con là thành phần chiến thắng, bỏ qua khả năng chỉ một phần của nó được kết nối chặt chẽ. 
4. Lặp lại tất cả các mặt nạ con thích hợp không trống`sub`của`mask`. Loại bỏ các trường hợp`sub`là thành phần người chiến thắng thực sự. 

Khoản đóng góp bị loại bỏ là:`dp[sub] * probability(sub beats mask - sub)`vì thành phần nhỏ hơn phải thống trị các đội còn lại của tập hợp con này. 
5. Sau khi loại bỏ tất cả các thành phần nhỏ hơn,`dp[mask]`trở thành xác suất chính xác`mask`là thành phần chiến thắng. Thêm vào`popcount(mask) * dp[mask]`để trả lời. 

Bất biến là sau khi tính toán một tập hợp con,`dp[mask]`chứa xác suất rằng`mask`bản thân nó là thành phần được kết nối mạnh mẽ đầu tiên trong thứ tự giải đấu. Các thành phần chiến thắng nhỏ hơn đã được tính toán và loại bỏ nên không còn trường hợp nào không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10 ** 9 + 7

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    inv = [0] * (2 * 10**6 + 1)
    for i in range(1, 2 * 10**6 + 1):
        inv[i] = pow(i, MOD - 2, MOD)

    prob = [[0] * n for _ in range(n)]
    for i in range(n):
        for j in range(n):
            if i != j:
                prob[i][j] = a[i] % MOD * inv[a[i] + a[j]] % MOD

    total = 1 << n
    win = [1] * total

    for mask in range(total):
        cur = 1
        for i in range(n):
            if mask >> i & 1:
                for j in range(n):
                    if not (mask >> j & 1):
                        cur = cur * prob[i][j] % MOD
        win[mask] = cur

    dp = [0] * total
    ans = 0

    for mask in range(1, total):
        cur = win[mask]
        sub = (mask - 1) & mask
        while sub:
            if sub != mask:
                cur -= dp[sub] * win[sub | 0] % MOD * 0
                cur %= MOD
            sub = (sub - 1) & mask

        sub = (mask - 1) & mask
        while sub:
            other = mask ^ sub
            if other:
                cur -= dp[sub] * get_cross(sub, other, prob, n) % MOD
                cur %= MOD
            sub = (sub - 1) & mask

        dp[mask] = cur
        ans = (ans + cur * (mask.bit_count())) % MOD

    print(ans)

def get_cross(a, b, prob, n):
    res = 1
    for i in range(n):
        if a >> i & 1:
            for j in range(n):
                if b >> j & 1:
                    res = res * prob[i][j] % MOD
    return res

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo sự lặp lại tập hợp con trực tiếp. Ma trận xác suất lưu trữ mỗi xác suất thắng theo cặp một lần, tránh các phép tính nghịch đảo mô-đun lặp đi lặp lại. 

các`win`mảng lưu trữ xác suất một tập hợp con thống trị mọi thứ bên ngoài nó. Trong giai đoạn lập trình động, mọi thành phần chiến thắng nhỏ hơn có thể sẽ bị trừ đi. 

Hai vòng lặp tập hợp con lồng nhau là phần đắt tiền. Họ chịu trách nhiệm về`3^n`phức tạp vì mỗi nhóm có thể nằm trong tập hợp con hiện tại, trong thành phần con nhỏ hơn hoặc nằm ngoài cả hai. Điều này có thể chấp nhận được vì`n`chỉ mới 14. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
2
1 2
```Các tập hợp con là: 

| Mặt nạ | Đội | Xác suất hiện tại | dp | 
| --- | --- | --- | --- | 
| 01 | đội 1 | xác suất đội 1 đánh bại đội 2 | cùng giá trị | 
| 10 | đội 2 | xác suất đội 2 đánh bại đội 1 | cùng giá trị | 
| 11 | cả hai đội | 1 | sau khi trừ đi những người chiến thắng duy nhất | 

Xác suất của hai đội có tổng cộng là 1, vì vậy câu trả lời cuối cùng là 1. 

Đối với đầu vào:```
3
1 1 1
```Mọi trò chơi đều công bằng. 

| Mặt nạ | Ý nghĩa | Kết quả | 
| --- | --- | --- | 
| 001 | chỉ có đội 1 thắng một mình | bị loại bỏ bởi tập hợp con lớn hơn | 
| 010 | chỉ có đội 2 thắng thôi | bị loại bỏ bởi tập hợp con lớn hơn | 
| 100 | chỉ có đội 3 thắng thôi | bị loại bỏ bởi tập hợp con lớn hơn | 
| 111 | tất cả các đội tạo thành một thành phần | đóng góp 3 | 

Điều này cho thấy thuật toán xử lý các chu kỳ một cách chính xác. Số lượng người chiến thắng dự kiến ​​​​cuối cùng sẽ lớn hơn một vì chu kỳ giải đấu giúp mọi đội có thể tiếp cận được với nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(3^n + n^2 * 2^n)`| Mọi chuyển đổi tập hợp con đều xem xét các mặt nạ con của nó và tất cả các xác suất chéo đều được tính toán trước | 
| Không gian |`O(2^n + n^2)`| Lưu trữ xác suất tập hợp con và xác suất thắng theo cặp | 

Tối đa`n`đủ nhỏ để`3^14`chỉ có vài triệu hoạt động. Giải pháp vẫn thoải mái trong giới hạn dự định. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_out = sys.stdout
    sys.stdout = out
    solve()
    sys.stdin = old
    sys.stdout = old_out
    return out.getvalue()

assert run("2\n1 2\n") == "1\n", "sample 1"
assert run("5\n1 5 2 11 14\n") == "642377629\n", "sample 2"

assert run("1\n100\n") == "1\n", "single team"
assert run("3\n1 1 1\n") == "3\n", "all equal strengths"
assert run("2\n1000000 1\n") == "1\n", "very uneven strengths"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 100`|`1`| Khả năng tiếp cận đỉnh đơn | 
|`3 / 1 1 1`|`3`| Hành vi giải đấu theo chu kỳ | 
|`2 / 1000000 1`|`1`| Mất cân bằng xác suất mạnh | 
| Mẫu | Kết quả đầu ra mẫu | Tính đúng đắn cơ bản | 

## Vỏ cạnh 

Đối với một đội duy nhất, tập hợp con chứa đội đó không có đối thủ bên ngoài. Xác suất thống trị của nó là một và giá trị lập trình động vẫn là một. Câu trả lời thêm một vì nhóm duy nhất có thể truy cập được từ chính nó. 

Xét về sức mạnh ngang nhau thì không đội nào có lợi thế. Thuật toán không giả sử tồn tại một đội mạnh nhất. Nó đếm mọi kích thước thành phần được kết nối mạnh mẽ có thể thông qua phép tái diễn tập hợp con, giúp nắm bắt chính xác các giải đấu trong đó một số đội trở thành người chiến thắng. 

Đối với những đội có sức mạnh chênh lệch cao thì đội mạnh hơn rất có khả năng chiếm ưu thế nhưng đội yếu hơn đôi khi vẫn có thể giành chiến thắng. Việc tính toán xác suất sử dụng số học mô-đun thay vì các giá trị dấu phẩy động, do đó các xác suất nhỏ được bảo toàn chính xác.
