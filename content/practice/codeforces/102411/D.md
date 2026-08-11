---
title: "CF 102411D - Bảng màu đôi"
description: "Chúng ta cần đếm các chuỗi trên một bảng chữ cái có kích thước (k), xem xét mọi độ dài không trống từ (1) đến (n). Một chuỗi là hợp lệ nếu bản thân nó là một palindrome hoặc có thể được chia thành hai palindrome. Hai mảnh có thể có độ dài khác nhau và thậm chí có thể giống hệt nhau."
date: "2026-08-11T07:30:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102411
codeforces_index: "D"
codeforces_contest_name: "ICPC 2019-2020 North-Western Russia Regional Contest"
rating: 0
weight: 102411
solve_time_s: 449
verified: true
draft: false
---

[CF 102411D - Bảng màu kép](https://codeforces.com/problemset/problem/102411/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần đếm các chuỗi trên một bảng chữ cái có kích thước (k), xem xét mọi độ dài không trống từ (1) đến (n). Một chuỗi là hợp lệ nếu bản thân nó là một palindrome hoặc có thể được chia thành hai palindrome. Hai mảnh có thể có độ dài khác nhau và thậm chí có thể giống hệt nhau. 

Cách giải thích hữu ích là một chuỗi hợp lệ có thể được tạo ra bằng cách chọn vị trí phân tách và tạo các bảng màu đối xứng cho cả hai bên. Khó khăn là cùng một chuỗi có thể có một số vị trí phân chia hợp lệ, vì vậy chỉ cần thêm số khả năng cho mỗi lần đếm quá mức phân chia. 

Bảng chữ cái có nhiều nhất là 26 chữ cái nhưng độ dài tối đa là (10^5). Bất kỳ phương pháp nào kiểm tra tất cả các chuỗi (k^n) đều không thể thực hiện được, ngay cả trước khi thực hiện bất kỳ kiểm tra bảng màu nào. Với (n=10^5), thuật toán (O(n^2)) cũng vượt xa giới hạn hai giây. Chúng ta cần khoảng (O(n\log n)) hoặc cao hơn, với các phép tính số học đơn giản. 

Có một số trường hợp khó xử lý. Đối với đầu vào`1 5`, mỗi chuỗi một chữ cái là một bảng màu, vì vậy câu trả lời chính xác là (5). Một giải pháp chỉ xem xét hai phần không trống sẽ trả về 0, bởi vì một chuỗi có độ dài một không có sự phân chia như vậy. 

Đối với đầu vào`2 3`, mỗi một trong số (3^2=9) chuỗi là một chuỗi palindrome kép. Dây đàn`ab`Và`ba`, ví dụ, bản thân chúng không phải là các palindrome, mà mỗi palindrome là sự kết hợp của hai palindrome một chữ cái. Một giải pháp chỉ tính các palindrome sẽ trả về (3+3=6) thay vì (9). 

Đối với đầu vào`5 1`, chỉ có một chuỗi có độ dài dương, đó là một chuỗi bao gồm toàn bộ`a`. Mỗi chuỗi như vậy là một chuỗi palindrome nên câu trả lời là (5). Trường hợp này rất hữu ích vì nhiều công thức liên quan đến lũy thừa của (k) trở thành (1), khiến cho các lỗi sai sót trong phép tính tổng độ dài trở nên đặc biệt rõ ràng. 

Một vấn đề tinh tế hơn là nhiều cách biểu diễn. Chuỗi`abacabacabac`có nhiều sự phân chia khác nhau thành hai palindromes. Do đó, việc đếm mọi phân chia hợp lệ một cách độc lập không tính các chuỗi mà tính các biểu diễn của chuỗi. Hướng dẫn chính thức của cuộc thi sử dụng chính xác hiện tượng này để thúc đẩy lập luận trong khoảng thời gian tối thiểu. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ liệt kê mọi chuỗi có độ dài lên tới (n). Đối với mỗi chuỗi, chúng ta có thể kiểm tra xem đó có phải là một chuỗi palindrome hay không, sau đó thử từng lần tách và kiểm tra xem hai phần kết quả có phải là palindrome hay không. Điều này đúng vì nó tuân theo định nghĩa trực tiếp nhưng lại cực kỳ tốn kém. có 

[ 
\sum_{m=1}^{n} k^m 
] 

các chuỗi, đã có (\Theta(k^n)) và kiểm tra tất cả các phần tách bằng kiểm tra palindrome đơn giản sẽ thêm một hệ số bậc hai khác trong trường hợp xấu nhất. Do đó, tổng công việc là (\Theta(n^2k^n)) khi triển khai đơn giản. Đối với (n=10^5) và (k=26), ngay cả việc tạo chuỗi cũng hoàn toàn không khả thi. 

Cách giảm hữu ích đầu tiên là đếm các biểu diễn thay vì các chuỗi riêng biệt. Cố định độ dài (l) của palindrome đầu tiên, cho phép (l=0) sao cho toàn bộ palindrome được biểu thị bằng phần đầu tiên trống. Một palindrome có độ dài (l) được xác định bởi các ký tự đầu tiên (\lceil l/2\rceil) của nó, do đó có 

[ 
k^{\lceil l/2\rceil} 
] 

những palindrome như vậy. Phần thứ hai có độ dài (n-l), cho 

[ 
k^{\lceil(n-l)/2\rceil} 
] 

khả năng. Do đó, nếu (R(n,k)) biểu thị số cách biểu diễn các chuỗi có độ dài-(n) dưới dạng hai palindrome, thì 

[ 
R(n,k)= 
\sum_{l=0}^{n-1} 
k^{\lceil l/2\rceil} 
k^{\lceil(n-l)/2\rceil}. 
] 

Tổng này có dạng đóng rất đơn giản. Nếu (n=2m), thì các giá trị (m) chẵn của (l) đóng góp (k^m) mỗi giá trị, trong khi các giá trị (m) lẻ đóng góp (k^{m+1}) mỗi giá trị. Do đó 

[ 
R(2m,k)=m(k+1)k^m. 
] 

Nếu (n=2m+1), mỗi một trong số (2m+1) vị trí phân chia có thể đóng góp (k^{m+1}), do đó 

[ 
R(2m+1,k)=(2m+1)k^{m+1}. 
] 

Vấn đề còn lại là loại bỏ các biểu diễn trùng lặp. Quan sát cấu trúc quan trọng là nếu một chuỗi có hai phần phân tách palindrome khác nhau thì khoảng cách giữa các vị trí phân tách đó là một khoảng thời gian của chuỗi. Trong thực tế, chuỗi có thể được xem như một sự dịch chuyển theo chu kỳ của sự đảo ngược của nó và hai vị trí phân chia hợp lệ khác nhau tạo ra hai sự dịch chuyển như vậy. Sự khác biệt của họ cho một khoảng thời gian. 

Lấy một palindrome kép có độ dài (n) và gọi (p) là khoảng thời gian nhỏ nhất của nó. Chuỗi cơ sở length-(p) tự nó là một chuỗi palindrome kép. Quan trọng hơn, chuỗi cơ sở này có chính xác một biểu diễn là hai palindromes. Khi nó được lặp lại (n/p) lần, chuỗi có độ dài-(n) kết quả sẽ xuất hiện chính xác (n/p) lần trong (R(n,k)), một lần cho mỗi vị trí phân chia tương thích. 

Gọi (D(n,k)) là số lượng palindrome kép có độ dài-(n) có sự phân chia duy nhất thành hai palindrome. Mọi biểu diễn được tính bởi (R(n,k)) đều thuộc về lớp duy nhất này hoặc xuất phát từ việc lặp lại một đối tượng duy nhất ngắn hơn. Như vậy 

##R(n,k) 

\sum_{\substack{l\mid n\l<n}} 
\frac{n}{l}D(l,k). 
] 

Khi đã biết (D(l,k)), mỗi lần lặp lại của một palindrome kép nguyên thủy có độ dài-(l) sẽ cho một chuỗi riêng biệt với mỗi độ dài là bội số của (l). Do đó số lượng các palindrome kép riêng biệt có độ dài (n) là 

[ 
T(n,k)=\sum_{l\mid n}D(l,k). 
] 

Câu trả lời bắt buộc bao gồm mọi độ dài từ (1) đến (n), vì vậy chúng ta có thể đảo ngược phép tính tổng: 

\sum_{l=1}^{n}D(l,k) 
\left\lfloor\frac nl\right\rfloor. 
] 

Sự truy hồi của số chia có thể được đánh giá bằng sàng. Sau khi tính toán (D(l,k)), chúng tôi thêm ((m/l)D(l,k)) vào bộ tích lũy cho mỗi bội số (m) của (l). Mỗi cặp số và một trong các bội số của nó được xử lý một lần, tạo ra tổng công (O(n\log n)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2k^n)) | (O(n)) mỗi chuỗi | Quá chậm | 
| Tối ưu | (O(n\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính toán trước (k^i\bmod 998244353) cho tất cả (0\le i\le\lceil n/2\rceil). Dạng đóng của (R(n,k)) chỉ cần lũy thừa tối đa số mũ này. 
2. Với mỗi độ dài (m), hãy tính số đại diện (R(m,k)). Đối với số chẵn (m=2q), hãy sử dụng 
[ 
R(m,k)=q(k+1)k^q. 
] 
Đối với số lẻ (m=2q+1), hãy sử dụng 
[ 
R(m,k)=m k^{q+1}. 
] 
Điều này tránh việc lặp lại tất cả các vị trí phân chia có thể có cho mọi độ dài. 
3. Duy trì một mảng`sub[m]`. Khi (D(l,k)) đã được tính toán, hãy thêm 
[ 
\frac{m}{l}D(l,k) 
] 
đến`sub[m]`với mọi bội số (m=2l,3l,\ldots). Khi chúng tôi sau này đạt được (m),`sub[m]`chứa chính xác phần đóng góp của tất cả các ước thực sự của (m). 
4. Tính toán 
[ 
D(m,k)=R(m,k)-sub[m]. 
] 
Độ dài xử lý theo thứ tự tăng dần đảm bảo rằng mọi (D(l,k)) cần thiết ở đây đều đã được tính toán. 
5. Thêm 
[ 
D(m,k)\left\lfloor\frac nm\right\rfloor 
] 
đến câu trả lời cuối cùng. Một đối tượng nguyên thủy có độ dài (m) có thể được lặp lại (1,2,\ldots,\lfloor n/m\rfloor) lần và mỗi lần lặp lại có độ dài khác nhau. 
6. Giảm từng modulo giá trị tích lũy (998244353). Số nguyên Python không bị tràn, nhưng việc giảm mô-đun giữ cho các giá trị trung gian ở mức nhỏ và khớp với đầu ra được yêu cầu. 

### Tại sao nó hoạt động 

Bất biến đằng sau sự tái phát là`D[m]`đếm chính xác các palindrome kép có chiều dài-(m) có sự phân chia palindrome là duy nhất.`R[m]`đếm mọi phân chia hợp lệ, bao gồm cả các biểu diễn lặp lại. Nếu một chuỗi có cách biểu diễn không duy nhất, thì sự khác biệt giữa hai vị trí phân tách sẽ tạo ra một khoảng thời gian không cần thiết, do đó chuỗi đó là sự lặp lại của một bảng màu kép ngắn hơn. Chu kỳ ngắn nhất của nó có một biểu diễn duy nhất và một đối tượng nguyên thủy có chiều dài (l) đóng góp chính xác (m/l) biểu diễn cho (R[m]). Do đó, trừ đi những đóng góp đó sẽ để lại chính xác những biểu diễn duy nhất. Mỗi palindrome kép có một cơ sở nguyên thủy có chu kỳ ngắn nhất, do đó tính tổng`D[l]`trên các ước số cho mỗi chuỗi riêng biệt chính xác một lần. 

Đây chính là cách phân rã khoảng thời gian tối thiểu được sử dụng trong hướng dẫn cuộc thi chính thức, trong đó phép lặp cho các biểu diễn duy nhất được đưa ra là (D(n,k)=R(n,k)-\sum_{l\mid n,l<n}(n/l)D(l,k)). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n, k = map(int, input().split())

    # We only need powers up to ceil(n / 2).
    half = (n + 1) // 2
    pw = [1] * (half + 1)
    for i in range(1, half + 1):
        pw[i] = pw[i - 1] * k % MOD

    # sub[m] will contain
    # sum_{l | m, l < m} (m / l) * D[l].
    sub = [0] * (n + 1)
    d = [0] * (n + 1)

    ans = 0

    for m in range(1, n + 1):
        if m & 1:
            q = m // 2
            r = m * pw[q + 1] % MOD
        else:
            q = m // 2
            r = q * (k + 1) % MOD
            r = r * pw[q] % MOD

        d[m] = (r - sub[m]) % MOD

        # Every primitive object of length m contributes one
        # distinct string for each repetition count up to n / m.
        ans = (ans + d[m] * (n // m)) % MOD

        # Make d[m] available to all larger multiples.
        for multiple in range(2 * m, n + 1, m):
            sub[multiple] = (
                sub[multiple] + (multiple // m) * d[m]
            ) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Mảng nguồn lưu trữ (k^i) modulo mô đun cần thiết. Số mũ lớn nhất là (\lceil n/2\rceil), bởi vì một palindrome có chiều dài (m) chỉ được xác định bởi một nửa vị trí của nó. 

Đối với mỗi chiều dài`m`, mã tính toán`r`, là dạng đóng của (R(m,k)). Các công thức chẵn và lẻ được cố tình giữ riêng biệt vì số lượng tổ hợp của chúng khác nhau. 

các`sub`mảng là cấu trúc sàng chính. Khi`d[m]`được biết đến, vòng lặp trên các bội số của nó sẽ cộng chính xác số lượng mà một nguyên hàm có độ dài-(m) đóng góp vào số lượng biểu diễn của mỗi bội số lớn hơn. Do đó, khi chiều dài`multiple`đã đạt được,`sub[multiple]`đã bằng toàn bộ số hạng trừ trong phép truy hồi. 

biểu thức`n // m`trong câu trả lời là một số chia khác, nhưng nó có ý nghĩa khác. Nó đếm xem có thể đạt được bao nhiêu độ dài cho phép bằng cách lặp lại một khối độ dài nguyên thủy`m`. Đây là lý do tại sao câu trả lời được tích lũy vào lúc này`d[m]`được tính toán thay vì xây dựng riêng từng (T(m,k)). 

Tất cả các phép nhân được theo sau bởi việc giảm mô-đun. yếu tố`multiple // m`nhiều nhất là (10^5), vì vậy Python xử lý nó một cách thoải mái. Các vòng lặp sử dụng`range(2 * m, n + 1, m)`bởi vì`m`bản thân nó không được đưa vào`sub[m]`: phép truy toán chỉ trừ các ước số thích hợp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho`n = 3`Và`k = 3`, các lũy thừa liên quan là (3^0=1) và (3^1=3). 

| Chiều dài (m) | (R(m,3)) |`sub[m]`| (D(m,3)) | Đóng góp (D(m)\lfloor3/m\rfloor) | Câu trả lời tích lũy | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 0 | 3 | 9 | 9 | 
| 2 | 12 | 6 | 6 | 6 | 15 | 
| 3 | 27 | 9 | 18 | 18 | 33 | 

Đối với độ dài 1, có ba chuỗi nguyên thủy. Mỗi chuỗi đóng góp vào cả ba số lần lặp lại được phép, tạo ra 9 chuỗi trên phạm vi độ dài. Đối với độ dài 2, số lượng biểu diễn là 12, nhưng sáu biểu diễn thuộc về sự lặp lại của các đối tượng có độ dài một, để lại sáu đối tượng nguyên thủy duy nhất. Ở độ dài 3, trừ chín biểu diễn được tạo bởi các nguyên hàm có độ dài một sẽ là 18. Tổng cuối cùng là (33), khớp với mẫu. 

Đóng góp đầu tiên là (9) không có nghĩa là có chín chuỗi một chữ cái. Nó có nghĩa là mỗi chuỗi trong số ba chuỗi nguyên thủy một chữ cái đóng góp một lần ở độ dài 1, hai lần ở độ dài 2 và ba lần ở độ dài 3. 

### Mẫu 2 

cho`n = 6`Và`k = 2`, các lũy thừa cần thiết là (2,4,8). 

| Chiều dài (m) | (R(m,2)) |`sub[m]`| (D(m,2)) | (\lfloor6/m\rfloor) | Đóng góp | Tích lũy | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 2 | 6 | 12 | 12 | 
| 2 | 6 | 4 | 2 | 3 | 6 | 18 | 
| 3 | 12 | 6 | 6 | 2 | 12 | 30 | 
| 4 | 24 | 12 | 12 | 1 | 12 | 42 | 
| 5 | 40 | 10 | 30 | 1 | 30 | 72 | 
| 6 | 72 | 30 | 42 | 1 | 42 | 114 | 

Kết quả là (114). Đối với độ dài từ 1 đến 5, số tích lũy bằng tổng số chuỗi nhị phân có độ dài đó, nhưng ở độ dài 6 chỉ có 52 trong số 64 chuỗi nhị phân là các chuỗi nhị phân kép. Do đó tổng số là 

[ 
2+4+8+16+32+52=114. 
] 

Bảng này cũng cho thấy tại sao việc đếm`R[m]`trực tiếp sẽ sai. Ở độ dài 6,`R[6]=72`, lớn hơn 52 chuỗi hợp lệ riêng biệt vì một số chuỗi có một số phân tách palindrome. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | Mỗi độ dài thực hiện một lượng công việc không đổi và mỗi độ dài cập nhật tất cả các bội số của nó | 
| Không gian | (O(n)) | Mỗi mảng lũy ​​thừa, phân số và số nguyên đều có kích thước (n+1) | 

Giới hạn chuỗi hài cho 

[ 
\sum_{m=1}^{n}\frac{n}{m}=O(n\log n), 
] 

vì vậy tổng số nhiều bản cập nhật có thể quản lý được một cách thoải mái trong (n=10^5). Thuật toán chỉ sử dụng một số mảng có độ dài (10^5+1), nằm trong giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 998244353

def solve_case(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    n, k = map(int, sys.stdin.readline().split())

    half = (n + 1) // 2
    pw = [1] * (half + 1)
    for i in range(1, half + 1):
        pw[i] = pw[i - 1] * k % MOD

    sub = [0] * (n + 1)
    d = [0] * (n + 1)

    ans = 0

    for m in range(1, n + 1):
        if m & 1:
            q = m // 2
            r = m * pw[q + 1] % MOD
        else:
            q = m // 2
            r = q * (k + 1) % MOD
            r = r * pw[q] % MOD

        d[m] = (r - sub[m]) % MOD
        ans = (ans + d[m] * (n // m)) % MOD

        for multiple in range(2 * m, n + 1, m):
            sub[multiple] = (
                sub[multiple] + (multiple // m) * d[m]
            ) % MOD

    sys.stdin = old_stdin
    return str(ans)

# Provided samples
assert solve_case("3 3\n") == "33", "sample 1"
assert solve_case("6 2\n") == "114", "sample 2"
assert solve_case("42 7\n") == "83419789", "sample 3"

# Minimum length
assert solve_case("1 5\n") == "5", "one-letter strings"

# Every string is valid when the alphabet has one character
assert solve_case("5 1\n") == "5", "single-letter alphabet"

# Maximum n, exercising the full sieve with the simplest alphabet
assert solve_case("100000 1\n") == "100000", "maximum n"

# Boundary where length 2 already contains every possible string
assert solve_case("2 3\n") == "12", "all strings of lengths 1 and 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 5`|`5`| Độ dài tối thiểu và cách biểu diễn phần đầu tiên trống | 
|`5 1`|`5`| Các chuỗi bằng nhau và bảng chữ cái một chữ cái | 
|`100000 1`|`100000`| Độ dài tối đa được phép và hành vi (O(n\log n)) | 
|`2 3`|`12`| Cả chuỗi một chữ cái và tất cả các chuỗi có độ dài hai chữ cái | 

Vì`2 3`, kết quả mong đợi là (3+9=12). Mỗi chuỗi có độ dài hai là sự nối của hai chuỗi palindrome một chữ cái, vì vậy trường hợp này kiểm tra xem thuật toán không vô tình yêu cầu bản thân chuỗi hoàn chỉnh phải là chuỗi palindromic. 

## Vỏ cạnh 

Đối với đầu vào`1 5`, mảng công suất chứa (5). Công thức cho (R(1,5)=1\cdot5=5),`sub[1]`bằng 0, và do đó (D(1,5)=5). Vì (1//1=1), câu trả lời là 5. Việc cho phép bảng màu đầu tiên có độ dài bằng 0 là điều cho phép biểu diễn toàn bộ bảng màu một chữ cái. 

Đối với đầu vào`2 3`, thuật toán tính (D(1)=3). Ở độ dài 2, (R(2)=1(3+1)3=12), trong khi`sub[2]=2D(1)=6`, do đó (D(2)=6). Câu trả lời cuối cùng là (3\cdot2+6=12). Sáu chuỗi có độ dài hai nguyên thủy kết hợp với sáu sự đóng góp từ các chuỗi nguyên thủy có độ dài một để tạo ra tất cả chín chuỗi có độ dài hai riêng biệt cùng với ba chuỗi có độ dài một. 

Đối với đầu vào`5 1`, mọi lũy thừa của (k) là 1. Các giá trị thu được là (D(1)=1,D(2)=0,D(3)=2,D(4)=0,D(5)=4) và tổng có trọng số là (5). Mặc dù bản thân sự phân rã nguyên thủy là không tầm thường, nhưng mọi chuỗi kết quả đều chỉ là`a`,`aa`,`aaa`,`aaaa`, hoặc`aaaaa`, do đó có chính xác một chuỗi hợp lệ cho mỗi độ dài. 

Đối với đầu vào`3 3`, các biểu diễn trùng lặp đã xuất hiện ở độ dài 2 và độ dài 3. Ở độ dài 2,`aaa`vẫn chưa liên quan, nhưng các chuỗi như`ab`có sự chia rẽ`a|b`; ở độ dài 3, các chuỗi như`aba`là các palindrome trong khi các palindrome khác có sự phân chia palindrome không tầm thường. Sự lặp lại không cố gắng xác định các chuỗi này một cách riêng lẻ. Thay vào đó, nó trừ chính xác các biểu diễn được tạo ra bởi các nguyên hàm tuần hoàn ngắn hơn, đó là lý do tại sao số đếm cuối cùng là 33 thay vì số lượng biểu diễn lớn hơn.
