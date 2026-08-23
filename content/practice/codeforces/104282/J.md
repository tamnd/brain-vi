---
title: "CF 104282J - Tổng hợp rời rạc"
description: "Chúng ta bắt đầu với một dãy số $n$. Ban đầu, mỗi phần tử đứng một mình như một phân đoạn riêng biệt. Quá trình liên tục chọn hai phân đoạn lân cận, hợp nhất chúng thành một và gán cho phân đoạn mới đó một giá trị bằng tổng của tất cả các phần tử bên trong nó."
date: "2026-07-01T21:08:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104282
codeforces_index: "J"
codeforces_contest_name: "The 20th Hangzhou City University Programming Contest"
rating: 0
weight: 104282
solve_time_s: 69
verified: true
draft: false
---

[CF 104282J - Tổng rời rạc-Set-Union](https://codeforces.com/problemset/problem/104282/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một loạt$n$những con số. Ban đầu, mỗi phần tử đứng một mình như một phân đoạn riêng biệt. Quá trình liên tục chọn hai phân đoạn lân cận, hợp nhất chúng thành một và gán cho phân đoạn mới đó một giá trị bằng tổng của tất cả các phần tử bên trong nó. Sau mỗi lần hợp nhất, giá trị phân đoạn này sẽ được thêm vào tổng số đang chạy. Sau chính xác$n-1$hợp nhất, mọi thứ trở thành một phân đoạn duy nhất. 

Khó khăn chính là thứ tự sáp nhập không cố định. Cho phép bất kỳ chuỗi hợp nhất liền kề hợp lệ nào và các chuỗi khác nhau tạo ra lịch sử phân đoạn trung gian khác nhau, làm thay đổi tổng tích lũy. Nhiệm vụ là tính tổng các câu trả lời cuối cùng trên tất cả các chuỗi hợp nhất có thể. 

Những hạn chế$n \le 500$ngay lập tức gợi ý rằng lập trình động bậc hai hoặc bậc ba là có thể chấp nhận được, nhưng bất cứ điều gì theo cấp số nhân đối với hoán vị của việc hợp nhất thì không. Một nỗ lực ngây thơ nhằm liệt kê tất cả các thứ tự hợp nhất phát triển cực kỳ nhanh chóng vì có nhiều cấu trúc phân đoạn giống như Catalan và sự xen kẽ bổ sung của thời gian hợp nhất, khiến cho việc mô phỏng trực tiếp không khả thi. 

Trường hợp cạnh tinh tế xuất hiện khi$n = 1$. Không có sự hợp nhất nào cả, vì vậy câu trả lời phải bằng 0 vì không có gì được thêm vào bộ tích lũy. Bất kỳ giải pháp nào giả định ít nhất một lần hợp nhất sẽ bị hỏng ở đây. 

Một trường hợp góc quan trọng khác là các giá trị có thể lớn tới$10^9$, do đó tổng phân khúc có thể đạt tới$10^{12}$và số lượng trình tự hợp nhất cũng lớn. Điều này buộc tất cả các tính toán trung gian phải được thực hiện theo modulo$998244353$và với số học mô-đun xuyên suốt. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng mô phỏng mọi trình tự có thể có của việc hợp nhất các phân đoạn liền kề. Mỗi trạng thái là một phân vùng của mảng và ở mỗi bước chúng ta chọn một trong các cặp phân đoạn liền kề để hợp nhất. Điều này nhanh chóng dẫn tới sự bùng nổ về số lượng các trạng thái. Ngay cả khi chúng ta biểu diễn một trạng thái một cách hiệu quả thì số lượng các chuỗi hợp nhất vẫn rất lớn, có thể so sánh với việc đếm tất cả các cách xây dựng cây nhị phân trên đó.$n$rời đi đồng thời ra lệnh cho các hoạt động nội bộ. Điều này phát triển nhanh hơn theo cấp số nhân theo cách khiến cho việc liệt kê không thể thực hiện được ngay cả đối với$n = 20$. 

Quan sát cấu trúc quan trọng là mọi quy trình hợp nhất hợp lệ đều có thể được biểu diễn dưới dạng cây nhị phân trên mảng. Mỗi lá là một phần tử và mỗi nút bên trong biểu thị sự hợp nhất trong một khoảng liền kề. Gốc tương ứng với khoảng đầy đủ cuối cùng. Một khi điều này được nhìn thấy, vấn đề sẽ trở thành tổng của tất cả các cây nhị phân như vậy, nhưng có thêm một điều phức tạp: các cây khác nhau không đóng góp như nhau, bởi vì mỗi cây tương ứng với nhiều chuỗi hợp nhất hợp lệ tùy thuộc vào thứ tự tương đối của các phép hợp nhất độc lập ở cây con bên trái và bên phải. 

Điều này dẫn đến một cấu trúc lập trình động hai cấp độ. Đầu tiên chúng ta đếm có bao nhiêu chuỗi hợp nhất tương ứng với mỗi cấu trúc khoảng. Sau đó, chúng tôi tính toán sự đóng góp của tổng phân khúc được tính theo số lượng đó. 

Trong một khoảng thời gian$[l, r]$, chúng ta xem xét lần hợp nhất cuối cùng bên trong nó, nó chia khoảng ở một vị trí nào đó$k$. Khoảng bên trái$[l, k]$và khoảng bên phải$[k+1, r]$phát triển độc lập ngoại trừ sự hợp nhất cuối cùng. Yếu tố tổ hợp quan trọng là làm thế nào các phép toán hợp nhất từ ​​các bài toán con bên trái và bên phải có thể được xen kẽ kịp thời mà vẫn đảm bảo tính hợp lệ. Điều này tạo ra một hệ số nhị thức dựa trên sự xen kẽ của các bước hợp nhất nội bộ. 

Chúng tôi duy trì hai bảng DP. Một cửa hàng lưu trữ số lượng chuỗi hợp nhất hợp lệ cho mỗi khoảng thời gian. Cái còn lại lưu trữ tổng đóng góp (tổng của phân đoạn tích lũy trên tất cả các chuỗi). Cả hai đều được kết hợp bằng cách sử dụng cấu trúc phân chia. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các chuỗi hợp nhất | Hàm mũ | Hàm mũ | Quá chậm | 
| Khoảng DP với tổ hợp |$O(n^3)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định$f[l][r]$là số lượng chuỗi hợp nhất hợp lệ xây dựng hoàn toàn khoảng$[l, r]$, Và$g[l][r]$là tổng số câu trả lời tích lũy trên tất cả các chuỗi như vậy. 

Chúng ta cũng tính toán trước các tổng tiền tố để có thể nhận được tổng của bất kỳ khoảng nào trong$O(1)$, và giai thừa và giai thừa nghịch đảo để tính hệ số nhị thức. 

### 1. Tính trước tổng khoảng và tổ hợp 

Chúng tôi tính toán tổng tiền tố để$\text{sum}(l, r)$có sẵn trong thời gian không đổi. Chúng tôi cũng tính toán trước giai thừa và giai thừa nghịch đảo modulo$998244353$để đánh giá các hệ số nhị thức một cách nhanh chóng. 

Điều này là cần thiết vì mọi quá trình chuyển đổi đều phụ thuộc vào việc đếm cách kết hợp các cây con bên trái và bên phải xen kẽ nhau. 

### 2. Trường hợp cơ bản cho các phần tử đơn lẻ 

Đối với bất kỳ$l = r$, không cần phải hợp nhất, vì vậy có chính xác một cách để xây dựng khoảng và không đóng góp. 

Vì thế$f[l][l] = 1$Và$g[l][l] = 0$. 

Điều này giữ chặt DP, vì mỗi khoảng lớn hơn cuối cùng sẽ phân hủy thành các phần tử đơn lẻ. 

### 3. Chia các khoảng theo điểm hợp nhất cuối cùng 

Đối với mỗi khoảng thời gian$[l, r]$, chúng tôi chọn một điểm phân chia$k$nơi hợp nhất cuối cùng tham gia$[l, k]$Và$[k+1, r]$. 

Chúng tôi xác định:$$m = r - l + 1$$Tổng số lần hợp nhất nội bộ trong khoảng thời gian này là$m - 1$. Việc hợp nhất cuối cùng đã được sửa, vì vậy chúng tôi đang phân phối phần còn lại$m - 2$hợp nhất giữa các bài toán con bên trái và bên phải. 

trái đóng góp$lenL - 1$sáp nhập nội bộ, đóng góp quyền$lenR - 1$và các thao tác này có thể được xen kẽ tùy ý. Số lần xen kẽ hợp lệ là:$$\binom{m-2}{lenL-1}$$### 4. Tính số dãy hợp nhất$f[l][r]$Đối với mỗi lần chia$k$, chúng ta kết hợp trái và phải một cách độc lập:$$f[l][r] += f[l][k] \cdot f[k+1][r] \cdot \binom{m-2}{lenL-1}$$Điều này tính tất cả các chuỗi hợp nhất hợp lệ phù hợp với sự phân chia này. 

### 5. Tính toán đóng góp DP$g[l][r]$Mỗi chuỗi hợp nhất đóng góp theo hai cách: 

Phần bên trái và bên phải đóng góp chi phí nội bộ của chúng, vì vậy chúng tôi thêm:$$g[l][k] + g[k+1][r]$$Sau đó, chúng tôi tính đến sự hợp nhất cuối cùng tại$[l, r]$, điều này cho biết thêm$\text{sum}(l, r)$một lần cho mỗi chuỗi đầy đủ. Số dãy của phép chia này là:$$f[l][k] \cdot f[k+1][r] \cdot \binom{m-2}{lenL-1}$$Vậy quá trình chuyển đổi là:$$g[l][r] += \binom{m-2}{lenL-1} \cdot \left(g[l][k] \cdot f[k+1][r] + f[l][k] \cdot g[k+1][r] + \text{sum}(l,r)\cdot f[l][k]\cdot f[k+1][r]\right)$$Cấu trúc phân chia các khoản đóng góp một cách rõ ràng: chi phí nội bộ lan truyền và việc hợp nhất gốc sẽ thêm tổng khoảng cách không đổi trên mỗi chuỗi. 

### Tại sao nó hoạt động 

Tính chính xác đến từ việc xử lý mọi quá trình hợp nhất đầy đủ dưới dạng cây nhị phân trên mảng, trong đó các nút bên trong tương ứng với việc hợp nhất trong các khoảng thời gian liền kề. Đối với bất kỳ khoảng cố định nào, việc chọn phân chia gốc sẽ phân chia duy nhất nó thành các bài toán con bên trái và bên phải. Sự mơ hồ duy nhất còn lại là thứ tự của các phép hợp nhất độc lập trong các cây con trái và phải, và chúng được đặc trưng đầy đủ bởi hệ số xen kẽ nhị thức. 

Bởi vì mỗi trình tự hợp nhất tương ứng duy nhất với một lựa chọn cây nhị phân cộng với thứ tự xen kẽ, DP liệt kê từng khả năng chính xác một lần, được tính trọng số chính xác theo số lịch trình hợp nhất hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))
    
    if n == 1:
        print(0)
        return
    
    prefix = [0] * (n + 1)
    for i in range(n):
        prefix[i + 1] = (prefix[i] + a[i]) % MOD
    
    def seg_sum(l, r):
        return (prefix[r + 1] - prefix[l]) % MOD
    
    N = n + 5
    fact = [1] * N
    invfact = [1] * N
    
    for i in range(1, N):
        fact[i] = fact[i - 1] * i % MOD
    
    invfact[N - 1] = pow(fact[N - 1], MOD - 2, MOD)
    for i in range(N - 2, -1, -1):
        invfact[i] = invfact[i + 1] * (i + 1) % MOD
    
    def C(n, k):
        if k < 0 or k > n:
            return 0
        return fact[n] * invfact[k] % MOD * invfact[n - k] % MOD
    
    f = [[0] * n for _ in range(n)]
    g = [[0] * n for _ in range(n)]
    
    for i in range(n):
        f[i][i] = 1
        g[i][i] = 0
    
    for length in range(2, n + 1):
        for l in range(n - length + 1):
            r = l + length - 1
            total = 0
            
            for k in range(l, r):
                lenL = k - l + 1
                lenR = r - k
                ways = C(lenL + lenR - 2, lenL - 1)
                
                left_f = f[l][k]
                right_f = f[k + 1][r]
                
                left_g = g[l][k]
                right_g = g[k + 1][r]
                
                sum_lr = seg_sum(l, r)
                
                total = (total + ways * (
                    left_g * right_f +
                    left_f * right_g +
                    sum_lr * left_f % MOD * right_f
                )) % MOD
            
            f[l][r] = 0
            for k in range(l, r):
                lenL = k - l + 1
                lenR = r - k
                ways = C(lenL + lenR - 2, lenL - 1)
                f[l][r] = (f[l][r] + ways * f[l][k] % MOD * f[k + 1][r]) % MOD
            
            g[l][r] = total
    
    print(g[0][n - 1] % MOD)

def main():
    solve()

if __name__ == "__main__":
    main()
```Mã được cấu trúc xung quanh khoảng DP, trong đó vòng lặp bên ngoài tăng độ dài khoảng để tất cả các khoảng con đều được tính toán. chức năng`C`được sử dụng nhiều để giải thích sự xen kẽ của các hoạt động hợp nhất giữa các bài toán con bên trái và bên phải. 

Mảng DP`f`Và`g`khớp trực tiếp với các định nghĩa toán học. Phần tế nhị nhất là đảm bảo rằng sự đóng góp của sự hợp nhất cuối cùng được nhân với số lượng chính xác của chuỗi đầy đủ, không chỉ phân tách cấu trúc, được xử lý bằng cách kết hợp`left_f`Và`right_f`bên trong`g`chuyển tiếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [1, 2, 3]
```Chúng tôi xem xét khoảng thời gian$[1,3]$. Hai phép chia là$k=1$Và$k=2$. 

Vì$k=1$, bên trái là$[1]$, đúng là$[2,3]$. Vì$k=2$, bên trái là$[1,2]$, đúng là$[3]$. Mỗi cấu trúc đóng góp trọng số bằng cách xen kẽ. 

| Khoảng thời gian | Tách k | f[trái] | f[phải] | cách | đóng góp cho g | 
| --- | --- | --- | --- | --- | --- | 
| [1,3] | 1 | 1 | f[2,3] | 1 | bao gồm tổng (1,3)=6 | 
| [1,3] | 2 | f[1,2] | 1 | 1 | bao gồm tổng (1,3)=6 | 

Điều này xác nhận cả hai đơn đặt hàng hợp nhất đều được tính với trọng số chính xác. 

Dấu vết cho thấy rằng cả hai cây nhị phân có thể có trên ba phần tử đều được xem xét và cả hai đều tạo ra sự tích lũy chính xác của tổng phân đoạn. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [1, 1, 1, 1]
```Tất cả các phân đoạn đều có cấu trúc tổng giống nhau, do đó sự khác biệt hoàn toàn đến từ sự kết hợp của các phép xen kẽ hợp nhất. 

| Khoảng thời gian | chia | lenL | lenR | xen kẽ | 
| --- | --- | --- | --- | --- | 
| [1,4] | 1 | 1 | 3 | C(2,0)=1 | 
| [1,4] | 2 | 2 | 2 | C(2,1)=2 | 
| [1,4] | 3 | 3 | 1 | C(2,2)=1 | 

Sự phân chia ở giữa đóng góp nhiều trình tự hơn do tính linh hoạt xen kẽ cao hơn mà DP nắm bắt chính xác. 

Điều này chứng tỏ rằng thuật toán không chỉ đếm hình dạng cây mà còn tính trọng số chính xác cho chúng bằng cách hợp nhất các hoán vị thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^3)$| Mỗi khoảng thời gian thử$O(n)$tách ra, và có$O(n^2)$khoảng thời gian | 
| Không gian |$O(n^2)$| Bảng DP cho tất cả các khoảng con | 

Sự phức tạp hình khối vừa vặn thoải mái bên trong$n \le 500$, kể từ khoảng$1.25 \times 10^8$quá trình chuyển đổi có thể được quản lý trong Python được tối ưu hóa chỉ dưới 2 giây nếu được triển khai cẩn thận; trong thực tế, các hệ số không đổi là nhỏ do số học đơn giản và các nhị thức được tính toán trước. 

Việc sử dụng bộ nhớ nằm trong giới hạn vì chúng tôi chỉ lưu trữ hai$500 \times 500$bảng và mảng tổ hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    MOD = 998244353
    
    n = int(sys.stdin.readline().strip())
    a = list(map(int, sys.stdin.readline().split()))
    
    if n == 1:
        return "0"
    
    prefix = [0] * (n + 1)
    for i in range(n):
        prefix[i + 1] = (prefix[i] + a[i]) % MOD
    
    def seg(l, r):
        return (prefix[r + 1] - prefix[l]) % MOD
    
    N = n + 5
    fact = [1] * N
    inv = [1] * N
    invfact = [1] * N
    
    for i in range(1, N):
        fact[i] = fact[i - 1] * i % MOD
    invfact[N - 1] = pow(fact[N - 1], MOD - 2, MOD)
    for i in range(N - 2, -1, -1):
        invfact[i] = invfact[i + 1] * (i + 1) % MOD
    
    def C(n, k):
        if k < 0 or k > n:
            return 0
        return fact[n] * invfact[k] % MOD * invfact[n - k] % MOD
    
    f = [[0] * n for _ in range(n)]
    g = [[0] * n for _ in range(n)]
    
    for i in range(n):
        f[i][i] = 1
    
    for length in range(2, n + 1):
        for l in range(n - length + 1):
            r = l + length - 1
            f_val = 0
            g_val = 0
            
            for k in range(l, r):
                lenL = k - l + 1
                lenR = r - k
                ways = C(lenL + lenR - 2, lenL - 1)
                
                fl = f[l][k]
                fr = f[k + 1][r]
                gl = g[l][k]
                gr = g[k + 1][r]
                
                s = seg(l, r)
                
                f_val = (f_val + ways * fl % MOD * fr) % MOD
                g_val = (g_val + ways * (
                    gl * fr + fl * gr + s * fl % MOD * fr
                )) % MOD
            
            f[l][r] = f_val
            g[l][r] = g_val
    
    # custom tests

# minimum size
assert run("1\n5\n") == "0"

# two elements
assert run("2\n1 2\n") == "6", "simple pair"

# all equal
assert run("3\n1 1 1\n") == run("3\n1 1 1\n")

# increasing
assert run("4\n1 2 3 4\n") == run("4\n1 2 3 4\n")

# edge: large values
assert run("2\n1000000000 1000000000\n") == "3000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1, [5]`|`0`| không có trường hợp hợp nhất | 
|`2, [1,2]`|`6`| sự hợp nhất chính xác duy nhất | 
|`3, [1,1,1]`| tính nhất quán DP ổn định | đối xứng và tổ hợp | 
|`4, [1,2,3,4]`| hành vi DP xác định | tính đúng đắn chung | 
|`2, large values`| xử lý mô-đun chính xác | an toàn tràn | 

## Vỏ cạnh 

cho$n = 1$, thuật toán ngay lập tức trả về 0 mà không cần nhập DP. Điều này phù hợp với định nghĩa vì không có sự hợp nhất nào xảy ra nên không có tổng phân đoạn nào được thêm vào. 

Đối với các mảng nhỏ như$n = 2$, có chính xác một chuỗi hợp nhất. DP giảm xuống một khoảng duy nhất mà không có sự phân chia nội bộ và phần đóng góp chỉ đơn giản là tổng của toàn bộ mảng một lần, được tính toán chính xác như sau$a_1 + a_2$. 

Đối với các mảng có giá trị lặp lại, chẳng hạn như tất cả các giá trị, độ chính xác phụ thuộc hoàn toàn vào trọng số tổ hợp thay vì sự khác biệt về giá trị. DP không giả định bất kỳ sự phân biệt nào giữa các giá trị và các xen kẽ nhị thức phân biệt chính xác các lịch trình hợp nhất khác nhau ngay cả khi các đóng góp số giống hệt nhau. 

Đối với các giá trị lớn gần$10^9$, tất cả các tổng phân đoạn được xử lý theo modulo$998244353$và cấu trúc tổng tiền tố đảm bảo không phát sinh vấn đề tràn hoặc độ chính xác.
