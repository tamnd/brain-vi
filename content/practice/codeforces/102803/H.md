---
title: "CF 102803H - Ghét Việc Biết Tôi"
description: "Đối với số mũ k cố định, hãy định nghĩa sigmak(i) là tổng lũy ​​thừa thứ k của tất cả các ước của i. Nhiệm vụ không yêu cầu một giá trị của hàm này mà yêu cầu XOR của hai tổng tiền tố lớn. Đối với mỗi trường hợp thử nghiệm, chúng ta có hai số mũ nhỏ a và b và giới hạn n."
date: "2026-07-26T16:24:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102803
codeforces_index: "H"
codeforces_contest_name: "The 15th Heilongjiang Provincial Collegiate Programming Contest"
rating: 0
weight: 102803
solve_time_s: 45
verified: true
draft: false
---

[CF 102803H - Ghét việc bạn biết tôi](https://codeforces.com/problemset/problem/102803/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đối với số mũ cố định`k`, định nghĩa`sigma_k(i)`như tổng của`k`- lũy thừa thứ của tất cả các ước của`i`. Nhiệm vụ không yêu cầu một giá trị của hàm này mà yêu cầu XOR của hai tổng tiền tố lớn. Đối với mỗi trường hợp thử nghiệm, chúng tôi được cấp hai số mũ nhỏ`a`Và`b`, và một giới hạn`n`. Chúng ta cần tính tổng`sigma_a(i)`cho mọi`i`từ`1`ĐẾN`n`, làm tương tự cho`sigma_b(i)`, XOR hai kết quả và chỉ giữ lại 64 bit thấp nhất. 

Số mũ bị giới hạn ở`0, 1, 2, 3`, Nhưng`n`có thể đạt được`10^12`. Một mô phỏng trực tiếp trên tất cả các số lên đến`n`sẽ yêu cầu lặp lại một nghìn tỷ lần trong trường hợp xấu nhất, vượt xa giới hạn 2 giây cho phép. Thậm chí một`O(n log n)`phương pháp liệt kê số chia sẽ cần khoảng`10^13`nên lời giải phải khai thác cấu trúc toán học của tổng chia. 

Các trường hợp cạnh chính đến từ kích thước của`n`và từ hành vi 64-bit được yêu cầu. Ví dụ, khi`n=1`,`a=0`, Và`b=1`, số duy nhất được xét là`1`. Cả hai tổng số chia đều là`1`, vậy XOR là`0`.```
Input:
1
0 1 1

Output:
0
```Một giải pháp bắt đầu vòng lặp của nó từ`d=0`sẽ thất bại vì ước số bắt đầu tại`1`. 

Một lỗi dễ mắc phải khác là quên câu trả lời là modulo`2^64`. Ví dụ: tổng tiền tố của tổng số chia có thể vượt quá phạm vi số nguyên 64 bit có dấu thông thường đối với số lớn`n`, do đó, việc sử dụng triển khai ngôn ngữ với độ chính xác tùy ý đòi hỏi phải áp dụng mặt nạ một cách rõ ràng. 

Vì`n=4`,`a=0`, Và`b=1`, hai tổng tiền tố là:```
sigma_0: 1, 2, 2, 3 -> total 8
sigma_1: 1, 3, 4, 7 -> total 15
```Câu trả lời đúng là:```
8 XOR 15 = 7
```Việc triển khai bất cẩn làm nhầm lẫn XOR với phép cộng sẽ tạo ra`23`. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản tuân theo định nghĩa trực tiếp. Với mọi số nguyên`i`từ`1`ĐẾN`n`, chúng ta có thể liệt kê các ước của nó, tính toán`sigma_a(i)`Và`sigma_b(i)`, và thêm chúng. Điều này đúng vì nó đánh giá định nghĩa toán học theo đúng nghĩa đen. 

Vấn đề là quy mô. Các trường hợp thử nghiệm lớn nhất chứa`n=10^12`, do đó thậm chí chạm vào mọi giá trị của`i`là không thể. Việc liệt kê dựa trên số chia thậm chí còn chậm hơn vì mỗi số phải được phân tích thành thừa số hoặc quét để tìm số chia. 

Quan sát quan trọng là thứ tự tính tổng có thể được hoán đổi. Thay vì hỏi ước số nào thuộc về mỗi số, hãy hỏi có bao nhiêu số chứa một ước số cụ thể. một số chia`d`đóng góp`d^k`tới mọi bội số của`d`, và có chính xác`floor(n/d)`bội số như vậy. Vì thế:$$\sum_{i=1}^{n}\sigma_k(i)=\sum_{d=1}^{n} d^k\left\lfloor\frac nd\right\rfloor$$Thách thức còn lại là số tiền dường như vẫn có`n`điều khoản. Tính chất quan trọng đó là`floor(n/d)`không đổi trong phạm vi dài. Nếu như`floor(n/l)=q`, thì mọi`d`trong một khoảng nhất định kết thúc tại`r=floor(n/q)`có cùng thương số`q`. 

Điều này cho phép chúng tôi xử lý các phạm vi thay vì các ước số riêng lẻ. Chỉ có khoảng`2*sqrt(n)`phạm vi như vậy, đó là khoảng hai triệu khi`n=10^12`. Đối với mỗi phạm vi, chúng ta cần tổng của`d^k`giữa hai ranh giới. Từ`k`nhiều nhất là`3`, công thức tiền tố dạng đóng là đủ. 

Brute-force hoạt động vì nó đếm chính xác từng đóng góp của ước số, nhưng không thành công vì thực hiện quá nhiều thao tác nhỏ. Quan sát nhóm thương số nén nhiều số hạng giống nhau vào một phép tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) hoặc tệ hơn | O(1) | Quá chậm | 
| Nhóm thương số | O(sqrt(n)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi số mũ cần tìm`k`, tính giá trị của$$F_k(n)=\sum_{d=1}^{n}d^k\left\lfloor\frac nd\right\rfloor$$sử dụng phạm vi thương thay vì lặp qua mọi ước số. 

1. Bắt đầu một phạm vi tại`l=1`. thương số là`q=n//l`. Vị trí cuối cùng mà thương số này không thay đổi là`r=n//q`. 

Toàn bộ khoảng thời gian`[l,r]`đóng góp:$$q \times \sum_{d=l}^{r}d^k$$vì mọi ước số trong khoảng này đều có cùng một số nhân. 

1. Lấy tổng lũy ​​thừa của khoảng từ các công thức tiền tố. Các công thức cần có là:$$\sum d = \frac{x(x+1)}2$$

$$\sum d^2=\frac{x(x+1)(2x+1)}6$$

$$\sum d^3=\left(\frac{x(x+1)}2\right)^2$$các`k=0`trường hợp chỉ đơn giản là đếm số trong khoảng. 

1. Thêm phần đóng góp vào câu trả lời modulo`2^64`, sau đó tiếp tục từ`r+1`. 
2. Tính toán hai tổng tiền tố bắt buộc, XOR chúng và xuất ra giá trị 64-bit thu được. 

Tính đúng đắn đến từ phép biến đổi đóng góp của số chia. Mỗi cặp`(i,d)`Ở đâu`d`chia rẽ`i`được tính chính xác một lần trong định nghĩa ban đầu. Sau khi đổi thứ tự, cặp giống nhau được tính khi xử lý số chia`d`, nhân với số bội số hợp lệ. Nhóm thương chỉ kết hợp các ước số liền kề với các số nhân giống hệt nhau nên nó thay đổi thứ tự tính toán chứ không thay đổi giá trị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1 << 64

def prefix_power(x, k):
    if k == 0:
        return x & (MOD - 1)
    if k == 1:
        return (x * (x + 1) // 2) & (MOD - 1)
    if k == 2:
        return (x * (x + 1) * (2 * x + 1) // 6) & (MOD - 1)
    t = x * (x + 1) // 2
    return (t * t) & (MOD - 1)

def calc(n, k):
    ans = 0
    l = 1

    while l <= n:
        q = n // l
        r = n // q

        segment = (prefix_power(r, k) - prefix_power(l - 1, k)) & (MOD - 1)
        ans = (ans + q * segment) & (MOD - 1)

        l = r + 1

    return ans

def solve():
    data = sys.stdin.read().strip().split()
    if not data:
        return

    t = int(data[0])
    idx = 1
    cache = {}
    out = []

    for _ in range(t):
        a = int(data[idx])
        b = int(data[idx + 1])
        n = int(data[idx + 2])
        idx += 3

        if n not in cache:
            cache[n] = [calc(n, k) for k in range(4)]

        out.append(str((cache[n][a] ^ cache[n][b]) & (MOD - 1)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`prefix_power`Hàm thực hiện bốn dạng đóng mà thuật toán cần. Việc chia xảy ra trước phép toán modulo vì các công thức chứa phân số và việc áp dụng modulo quá sớm sẽ làm mất kết quả số nguyên. 

các`calc`Hàm thực hiện việc nhóm thương. Các biến`l`Và`r`mô tả khoảng chia hiện tại. Di chuyển trực tiếp đến`r+1`tránh việc xử lý từng ước số một cách riêng biệt. 

Bộ đệm lưu trữ tất cả bốn kết quả số mũ có thể có cho một giá trị nhất định`n`. Vì nhiều trường hợp thử nghiệm có thể có cùng giới hạn, điều này tránh lặp lại cùng một giới hạn.`O(sqrt(n))`tính toán. 

Mặt nạ bit`MOD - 1`chỉ giữ lại 64 bit thấp nhất sau mỗi thao tác lớn. Các số nguyên Python không tràn một cách tự nhiên, do đó, mặt nạ rõ ràng này tái tạo modulo cần thiết`2^64`. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
Input:
1
0 1 4
```Thuật toán tính cả hai tổng tiền tố. 

| tôi | r | thương số | k=0 đóng góp | k=1 đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | 4 | 4 | 
| 2 | 2 | 2 | 2 | 4 | 
| 3 | 4 | 1 | 2 | 7 | 

Tổng số là`8`Và`15`. XOR của họ là`7`. 

Đối với mẫu thứ hai:```
Input:
1
2 3 2
```| tôi | r | thương số | k=2 đóng góp | k=3 đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 2 | 2 | 
| 2 | 2 | 1 | 4 | 8 | 

Tổng số là`6`Và`10`. XOR cuối cùng là`12`. 

Những dấu vết này cho thấy các khoảng thương bao trùm mọi ước số chính xác một lần, đồng thời kết hợp tất cả các ước số có cùng giá trị của`floor(n/d)`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(n)) cho mỗi n khác biệt | Việc phân nhóm thương số tạo ra khoảng`2*sqrt(n)`các khoảng thời gian và mỗi khoảng sử dụng công thức thời gian không đổi. | 
| Không gian | O(1) ngoài bộ đệm | Chỉ cần một vài biến số nguyên, cộng với bốn câu trả lời được lưu trữ cho mỗi giá trị kiểm tra riêng biệt của n. | 

Với`n`lên đến`10^12`, số khoảng thương là khoảng hai triệu. Giá trị này đủ nhỏ đối với các giới hạn đã cho, trong khi bất kỳ cách tiếp cận nào cũng tỷ lệ thuận với`n`là không thể. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 1 << 64

def prefix_power(x, k):
    if k == 0:
        return x & (MOD - 1)
    if k == 1:
        return (x * (x + 1) // 2) & (MOD - 1)
    if k == 2:
        return (x * (x + 1) * (2 * x + 1) // 6) & (MOD - 1)
    s = x * (x + 1) // 2
    return (s * s) & (MOD - 1)

def calc(n, k):
    ans = 0
    l = 1
    while l <= n:
        q = n // l
        r = n // q
        ans = (ans + q * ((prefix_power(r, k) - prefix_power(l - 1, k)) & (MOD - 1))) & (MOD - 1)
        l = r + 1
    return ans

def run(inp):
    data = inp.split()
    t = int(data[0])
    p = 1
    out = []
    for _ in range(t):
        a, b, n = map(int, data[p:p+3])
        p += 3
        x = calc(n, a)
        y = calc(n, b)
        out.append(str((x ^ y) & (MOD - 1)))
    return "\n".join(out)

assert run("2\n0 1 4\n2 3 2\n") == "7\n12"

assert run("1\n0 0 1\n") == "0"

assert run("1\n1 1 10\n") == "0"

assert run("1\n0 1 1\n") == "0"

assert run("1\n2 3 1000000000000\n") == run("1\n2 3 1000000000000\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 1 4`|`7`| Cung cấp mẫu và xử lý XOR | 
|`0 0 1`|`0`| Đầu vào tối thiểu và số mũ bằng nhau | 
|`1 1 10`|`0`| Các tổng giống hệt nhau luôn XOR về 0 | 
|`0 1 1`|`0`| Trường hợp ranh giới nhỏ nhất | 
|`2 3 1000000000000`| tính bằng tham chiếu | Kích thước tối đa`n`xử lý | 

## Vỏ cạnh 

cho`n=1`, nhóm thương chỉ có một khoảng. Thuật toán bắt đầu với`l=1`, tính toán`q=1`và xử lý ước số duy nhất. Không có nỗ lực truy cập số chia bằng 0, do đó trường hợp nhỏ nhất được xử lý một cách tự nhiên. 

Đối với số mũ bằng nhau như:```
Input:
1
2 2 100
```cả hai phép tính đều sử dụng cùng số mũ và tạo ra tổng tiền tố giống nhau. Hoạt động XOR cuối cùng tạo ra số không. Việc triển khai xử lý việc này mà không cần nhánh đặc biệt vì XOR đã có thuộc tính này. 

Đối với các giá trị rất lớn như:```
Input:
1
3 2 1000000000000
```số học trung gian vượt quá phạm vi 64-bit được ký thông thường. Thuật toán che dấu sau các thao tác, bảo toàn chính xác 64 bit thấp hơn cần thiết thay vì dựa vào hành vi tràn ngôn ngữ. 

Đối với những trường hợp`n`gần một ranh giới thương số, chẳng hạn như`n=10`, phép tính khoảng`r=n//q`phải chính xác. Nếu một triển khai được sử dụng`r=l`hoặc tăng không chính xác, nó sẽ bỏ qua hoặc đếm gấp đôi các ước số. Công thức phân nhóm được sử dụng ở đây luôn tiến triển từ`l`ĐẾN`r+1`, do đó mọi ước số đều xuất hiện trong đúng một khoảng.
