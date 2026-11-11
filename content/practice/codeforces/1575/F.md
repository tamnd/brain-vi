---
title: "CF 1575F - Tìm giá trị kỳ vọng"
description: "Chúng ta bắt đầu với một mảng có giá trị nằm trong khoảng [0, k - 1]. Miễn là không phải tất cả các vị trí đều chứa cùng một giá trị, chúng ta liên tục chọn một vị trí ngẫu nhiên và một giá trị ngẫu nhiên, sau đó ghi đè lên vị trí đó."
date: "2026-06-10T10:55:13+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1575
codeforces_index: "F"
codeforces_contest_name: "COMPFEST 13 - Finals Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 2900
weight: 1575
solve_time_s: 76
verified: true
draft: false
---

[CF 1575F - Tìm giá trị mong đợi](https://codeforces.com/problemset/problem/1575/F) 

**Xếp hạng:** 2900 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một mảng có giá trị nằm trong phạm vi`[0, k - 1]`. Miễn là không phải tất cả các vị trí đều chứa cùng một giá trị, chúng ta liên tục chọn một vị trí ngẫu nhiên và một giá trị ngẫu nhiên, sau đó ghi đè lên vị trí đó. 

Đối với một mảng được chỉ định đầy đủ`b`, cho phép`f(b)`là số phép toán dự kiến ​​cho đến khi mọi phần tử trở nên bằng nhau. 

Mảng đầu vào chỉ được xác định một phần. Một số vị trí có chứa`-1`. Mỗi vị trí như vậy được thay thế độc lập bằng một giá trị ngẫu nhiên thống nhất từ`[0, k - 1]`. Sau khi thay thế tất cả, chúng ta thu được một mảng hoàn chỉnh ngẫu nhiên. Nhiệm vụ là tính giá trị kỳ vọng của`f`trên tất cả các hoàn thành như vậy. 

Phần nổi bật nhất của tuyên bố là`k`có thể lớn như`10^9`. Bất kỳ giải pháp nào phụ thuộc vào việc lặp lại màu sắc đều là không thể ngay lập tức. Đồng thời,`n`chỉ là`10^5`, điều này gợi ý rõ ràng rằng công thức cuối cùng nên phụ thuộc vào số lượng tần số thay vì vào từng màu riêng lẻ. 

Quan sát thứ hai là bản thân quá trình ngẫu nhiên có một không gian trạng thái rất lớn. Ngay cả đối với mức độ vừa phải`n`, việc lưu trữ tất cả các mảng có thể là vô vọng. Chúng ta cần một cách để tính thời gian đánh dự kiến ​​mà không cần xây dựng chuỗi Markov. 

Những trường hợp phức tạp là những trường hợp liên quan đến màu sắc hiện chưa xuất hiện. 

Coi như:```
n = 3, k = 10
a = [0, 0, 1]
```Có tám màu có tần số bằng 0. Không thể đơn giản bỏ qua sự đóng góp của họ, vì sau khi lấp đầy những vị trí chưa biết, họ vẫn tham gia vào công thức kỳ vọng. 

Một sai lầm dễ mắc phải khác là chỉ tính toán kỳ vọng đối với các màu hiện có.```
n = 3, k = 3
a = [-1, -1, -1]
```Mỗi màu sắc đều đối xứng. Câu trả lời mong đợi phụ thuộc vào số lượng vị trí cuối cùng nhận được mỗi màu, chứ không phụ thuộc vào màu nào sẽ xuất hiện trong đầu vào. 

Cạm bẫy thứ ba là trạng thái cuối. Khi tất cả các phần tử đều bằng nhau, quá trình sẽ dừng ngay lập tức. Bất kỳ đối số hàm tiềm năng nào cũng phải gán cùng một giá trị cho mọi cấu hình đầu cuối, nếu không công thức thời gian dừng sẽ không chính xác. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là coi mọi mảng như một trạng thái của chuỗi Markov và giải một hệ phương trình tuyến tính về thời gian chạm dự kiến. 

Số lượng trạng thái là`k^n`. Ngay cả đối với`n = 10`Và`k = 10`, cái này đã rồi`10^10`tiểu bang. Cách tiếp cận này đúng về mặt toán học nhưng hoàn toàn không thể sử dụng được. 

Quan sát quan trọng là quá trình này chỉ phụ thuộc vào tần số màu. 

Cho phép`cnt(x)`biểu thị số lần xuất hiện của màu sắc`x`. Thay vì nghiên cứu trực tiếp mảng, chúng ta tìm kiếm hàm thế$$F = \sum_x g(cnt(x))$$mà sự thay đổi dự kiến ​​​​trong một hoạt động luôn luôn là`-1`. 

Nếu hàm như vậy tồn tại thì định lý dừng tùy chọn sẽ ngay lập tức cho$$\mathbb E[\text{time to finish}] = F(\text{initial}) - F(\text{terminal}).$$Điều này hoàn toàn tránh được việc giải được chuỗi Markov. 

Sau khi tìm ra sự tái phát cho`g`, bài toán trở thành tổ hợp thuần túy. Chúng ta chỉ cần giá trị kỳ vọng của$$\sum_x g(cnt(x))$$sau khi thay thế ngẫu nhiên tất cả`-1`các vị trí. 

Mỗi vị trí chưa biết độc lập chọn một trong các`k`màu sắc. Do đó, đối với mỗi màu, số lần xuất hiện bổ sung tuân theo phân bố nhị thức. Tổng hợp những đóng góp này một cách cẩn thận mang lại một`O(n)`giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chuỗi Markov Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Hàm thế + Đếm nhị thức |`O(n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Khai thác tiềm năng 

hãy để$$F=\sum_x g(cnt(x)).$$Giả sử một màu hiện có tần số`i`. 

Nếu chúng ta chọn sự xuất hiện của màu đó và tô lại thành màu khác, tần số của nó sẽ giảm từ`i`ĐẾN`i-1`. 

Nếu chúng ta tô lại một số vị trí khác thành màu đó, tần số của nó sẽ tăng từ`i`ĐẾN`i+1`. 

Định nghĩa$$d_i=g(i)-g(i-1).$$Tính toán sự thay đổi dự kiến ​​của`F`và yêu cầu nó phải chính xác`-1`cho mọi trạng thái đưa ra điều kiện$$(k-1)i\,d_i-(n-i)d_{i+1}=n.$$Tương đương,$$(k-1)i(g(i)-g(i-1)) + (n-i)(g(i)-g(i+1)) = n.$$Chúng tôi thiết lập$$g(0)=g(1)=0,$$và tính toán tất cả các giá trị còn lại bằng cách lặp lại:$$g(i+1) = g(i) + \frac{(k-1)i(g(i)-g(i-1))-n}{n-i}.$$Tất cả số học được thực hiện modulo`10^9+7`. 

### Đóng góp dự kiến sau khi điền vào các vị trí chưa xác định 

hãy để`c`là số lượng`-1`mục nhập. 

Đối với mỗi màu, giả sử tần số hiện tại đã biết của nó bằng`j`. 

Sau khi điền vào các vị trí chưa biết, số lần xuất hiện bổ sung là$$X \sim \text{Binomial}(c,\tfrac1k).$$Do đó tần số cuối cùng bằng`j + X`. 

Cho phép`b[j]`biểu thị số lượng màu có tần số đã biết chính xác là`j`. 

Khi đó số màu dự kiến ​​có tần số cuối cùng bằng`i`là$$\sum_j b[j] \Pr(X=i-j).$$Xác suất nhị thức góp phần$$\binom{c}{i-j} \left(\frac1k\right)^{i-j} \left(\frac{k-1}{k}\right)^{c-(i-j)}.$$Thay vì chia theo lũy thừa của`k`nhiều lần, chúng ta tích lũy tử số$$\binom{c}{i-j}(k-1)^{c-(i-j)}$$và chia cho`k^c`một lần vào cuối. 

###Câu trả lời cuối cùng 

Giá trị kỳ vọng của điện thế trước khi quá trình bắt đầu là$$\mathbb E[F] = \sum_i g(i)\cdot \mathbb E[\#\text{colors with final frequency }i].$$Mỗi trạng thái cuối bao gồm một màu xuất hiện`n`lần và tất cả những lần khác xuất hiện`0`lần, nên tiềm năng của nó bằng$$g(n).$$Như vậy$$\mathbb E[f(a)] = \mathbb E[F]-g(n).$$Đây chính xác là câu trả lời cần thiết. 

### Tại sao nó hoạt động 

Sự tái phát cho`g`được rút ra sao cho mọi thao tác đều giảm giá trị kỳ vọng của`F`bởi chính xác một, bất kể cấu hình hiện tại. Do đó,$$F_t+t$$là một martingale. Cho phép`T`là lần đầu tiên tất cả các phần tử trở nên bằng nhau. Áp dụng nhận dạng thời gian dừng mang lại$$\mathbb E[T] = F(\text{start})-F(\text{finish}).$$Phần còn lại của giải pháp tính toán tiềm năng ban đầu dự kiến ​​sau khi thay thế ngẫu nhiên các mục chưa biết. Vì kỳ vọng là tuyến tính nên tổng các đóng góp nhị thức của mỗi màu sẽ mang lại giá trị chính xác của`\mathbb E[F]`. Kết hợp cả hai phần sẽ tạo ra thời gian dừng mong muốn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

def modpow(a, e):
    res = 1
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    freq = {}
    unknown = 0

    for x in a:
        if x == -1:
            unknown += 1
        else:
            freq[x] = freq.get(x, 0) + 1

    inv = [0] * (n + 1)
    inv[1] = 1
    for i in range(2, n + 1):
        inv[i] = (MOD - MOD // i) * inv[MOD % i] % MOD

    g = [0] * (n + 1)

    for i in range(1, n):
        term = ((k - 1) % MOD) * i % MOD
        term = term * ((g[i] - g[i - 1]) % MOD) % MOD
        term = (term + (n - i) * g[i] - n) % MOD
        g[i + 1] = term * inv[n - i] % MOD

    fac = [1] * (n + 1)
    ifac = [1] * (n + 1)

    for i in range(1, n + 1):
        fac[i] = fac[i - 1] * i % MOD

    ifac[n] = modpow(fac[n], MOD - 2)
    for i in range(n, 0, -1):
        ifac[i - 1] = ifac[i] * i % MOD

    def C(N, R):
        if R < 0 or R > N:
            return 0
        return fac[N] * ifac[R] % MOD * ifac[N - R] % MOD

    b = [0] * (n + 1)

    b[0] = k - len(freq)
    for v in freq.values():
        b[v] += 1

    pw = [1] * (unknown + 1)
    for i in range(1, unknown + 1):
        pw[i] = pw[i - 1] * ((k - 1) % MOD) % MOD

    nonzero = [i for i in range(n + 1) if b[i]]

    ans = 0

    for final_cnt in range(1, n + 1):
        ways = 0

        for base in nonzero:
            add = final_cnt - base
            if add < 0 or add > unknown:
                continue

            ways = (
                ways
                + b[base] % MOD
                * C(unknown, add)
                % MOD
                * pw[unknown - add]
            ) % MOD

        ans = (ans + ways * g[final_cnt]) % MOD

    ans = ans * modpow(modpow(k % MOD, unknown), MOD - 2) % MOD
    ans = (ans - g[n]) % MOD

    print(ans)

solve()
```Khối đầu tiên tính toán các giá trị tiềm năng`g(i)`từ sự tái phát bắt nguồn. Chỉ có tần số lên tới`n`là cần thiết. 

Khối tiếp theo chuẩn bị các giai thừa và giai thừa nghịch đảo để mọi hệ số nhị thức có thể được đánh giá trong thời gian không đổi. 

Mảng`b`lưu trữ bao nhiêu màu hiện có mỗi tần số đã biết. Điều này nén tất cả các màu thành chỉ`n + 1`các lớp tần số. 

Đối với mọi tần số cuối cùng có thể`i`, chúng tôi tính toán số lượng màu dự kiến ​​kết thúc bằng tần số đó. Hệ số nhị thức chọn số lượng vị trí chưa biết tham gia vào một màu nhất định, trong khi`(k - 1)^(...)`chiếm các vị trí chưa biết còn lại bằng cách chọn các màu khác. 

Giá trị tích lũy vẫn có mẫu số chung`k^c`, Ở đâu`c`là số vị trí chưa biết. Cuối cùng chúng ta chia một lần bằng cách nhân với nghịch đảo môđun. 

Cuối cùng chúng tôi trừ`g(n)`, tiềm năng của mọi trạng thái cuối. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 2
0 1
```Không có vị trí nào chưa biết. 

| Biến | Giá trị | 
| --- | --- | 
|`unknown`| 0 | 
| Tần số | `{0:1, 1: |
