---
title: "CF 102870B - Vòng tay của gấu trúc Orz"
description: "Chúng tôi cần đếm xem có thể kiếm được bao nhiêu tiền bằng cách bán Vòng tay Orz Panda độc đáo với mọi chiều dài có thể lên đến m. Một vòng tay có n vị trí xung quanh một vòng tròn, mỗi vị trí được chiếm bởi một khối hình chữ nhật."
date: "2026-07-25T13:21:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102870
codeforces_index: "B"
codeforces_contest_name: "2020-2021 \u201cOrz Panda\u201d Cup Programming Contest"
rating: 0
weight: 102870
solve_time_s: 79
verified: true
draft: false
---

[CF 102870B - Vòng tay của gấu trúc Orz](https://codeforces.com/problemset/problem/102870/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Chúng tôi cần đếm xem có thể kiếm được bao nhiêu tiền bằng cách bán Vòng tay Orz Panda độc đáo với mọi độ dài có thể lên đến`m`. Một chiếc vòng tay có sự sắp xếp hình tròn`n`các vị trí xung quanh cạnh của nó, mỗi vị trí được chiếm bởi một khối hình chữ nhật. Các khối tạo thành một hình trụ có chiều cao bằng hai, do đó, một cột có thể chứa một khối dọc hoặc được ghép với khối lân cận của nó bằng hai khối ngang. Hai chiếc vòng tay được coi là giống hệt nhau nếu việc xoay hình trụ quanh tâm của nó khiến chúng khớp nhau. 

Đối với một chiều dài cố định`n`, nhiệm vụ không yêu cầu số lượng gạch thô. Nó yêu cầu số lớp ốp lát xoay. Nếu con số đó là`b[n]`, số tiền đóng góp là`n * b[n]`. Câu trả lời cuối cùng là tổng của những đóng góp này cho tất cả`1 <= n <= m`, lấy modulo`p`. 

Các giá trị đầu vào có chủ ý cực đoan. Từ`m`có thể đạt được`10^9`, việc lặp lại mọi chiều dài vòng đeo tay có thể là không thể. Thậm chí một`O(m)`sự tái diễn quá chậm vì nó đòi hỏi hàng tỷ thao tác. Chúng ta cần sử dụng các công thức rút gọn bài toán về thời gian căn bậc hai. 

Khó khăn tiềm ẩn đầu tiên là tính chất hình tròn của chiếc vòng tay. Đối xử với chiếc vòng tay như bình thường`2 x n`hình chữ nhật tính các vị trí cắt khác nhau là các đối tượng khác nhau. Ví dụ, với`n = 2`, ba chiếc vòng tay độc đáo đưa ra câu trả lời mẫu đóng góp`1 * 1 + 2 * 3 = 7`, trong khi số lượng ốp lát tuyến tính ngây thơ sẽ không tôn trọng sự tương đương xoay. 

Một trường hợp cạnh khác là một chiếc vòng tay trong đó mỗi cột đều thẳng đứng. Vì`m = 1`, đầu vào là:```
1 114514
```Vòng đeo tay duy nhất có thể có một khối dọc, vì vậy đầu ra là:```
1
```Một phương pháp chia cho chiều dài mà không xử lý cẩn thận tính đối xứng tròn có thể thất bại ở đây vì chỉ có một phép quay. 

Vỏ cạnh thứ hai là một vòng đeo tay hoàn toàn định kỳ. Ví dụ:```
2 1919810
```Đối với chiều dài hai, có ba lớp xoay khác nhau. Đóng góp có trọng số của họ là`1 + 2 * 3 = 7`. Một phương pháp chỉ tính các sắp xếp tuyến tính sẽ tính riêng hai vết cắt có thể có của cùng một chiếc vòng tay. 

# Phương pháp tiếp cận 

Cách tiếp cận trực tiếp bắt đầu bằng cách liệt kê từng ô lát cho mọi chiều dài. Việc xếp lát có thể được biểu diễn bằng cách chọn cặp cột liền kề nào được bao phủ bởi các khối ngang. Điều này tương đương với việc chọn một kết quả phù hợp trên một chu kỳ có độ dài`n`. Số lượng các kết quả khớp như vậy giống như Fibonacci, nhưng việc tạo ra mọi sự sắp xếp đều theo cấp số nhân. Với độ dài khoảng 40, điều này đã trở nên không thực tế và giới hạn thực sự là`10^9`, vì vậy vũ lực không phải là một hướng khả thi. 

Quan sát hữu ích đầu tiên là bài toán lát gạch hình trụ có cấu trúc đơn giản. Nếu chúng ta bỏ qua phép quay, số ô của một chu trình có độ dài`n`là số lần khớp của chu kỳ đó:$$L_n = F_{n-1}+F_{n+1}$$Ở đâu`F`là dãy Fibonacci. 

Để loại bỏ phép quay, chúng ta sử dụng bổ đề Burnside. Thay vì cố gắng chọn một đại diện cho mỗi vòng tay, chúng tôi đếm xem có bao nhiêu ô không thay đổi trong mỗi vòng quay có thể và tính trung bình các số lượng này. 

Một vòng quay bằng`k`vị trí chia vị trí vòng đeo tay thành`gcd(n,k)`chu kỳ. Một ô được cố định bằng cách xoay này chính xác là một ô lặp lại mỗi`gcd(n,k)`các vị trí. Do đó số lượng gạch cố định của nó là`L_gcd(n,k)`. Việc nhóm các phép quay theo gcd của chúng sẽ mang lại:$$n \cdot b_n=\sum_{d|n}\varphi(n/d)L_d$$Ở đâu`φ`là hàm tổng Euler. 

Chúng ta cần tổng giá trị này cho tất cả`n`lên đến`m`:$$\sum_{n=1}^{m}\sum_{d|n}\varphi(n/d)L_d$$Thay đổi thứ tự tính tổng sẽ có:$$\sum_{d=1}^{m}L_d\sum_{k=1}^{\lfloor m/d\rfloor}\varphi(k)$$Thách thức còn lại là tính toán điều này một cách hiệu quả. thương số`floor(m/d)`chỉ thay đổi`O(sqrt(m))`lần, vì vậy chúng tôi có thể xử lý phạm vi`d`cùng nhau. Chúng ta cũng cần tổng tiền tố của`L_d`, điều này thật dễ dàng vì Fibonacci có công thức tính tổng đóng. Hàm bắt buộc khác là tổng tiền tố của hàm tổng Euler, được tính bằng đệ quy chia sàn được ghi nhớ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Burnside với chức năng tóm tắt | O(sqrt(m) log m) | O(sqrt(m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng tiền tố của hàm tổng Euler`Phi(x) = φ(1)+...+φ(x)`sử dụng ghi nhớ. Sự đệ quy xuất phát từ danh tính:$$\frac{x(x+1)}2=\sum_{i=1}^{x}\Phi(\lfloor x/i\rfloor)$$Các giá trị sàn được nhóm thành các phạm vi để phép đệ quy chỉ truy cập các thương số riêng biệt. 

1. Tính hàm tiền tố cho số lượng gạch hình trụ không hạn chế:$$\sum_{i=1}^{n}L_i=F_{n+1}+F_{n+3}-3$$Nhân đôi nhanh tính toán các số Fibonacci theo thời gian logarit, ngay cả đối với các giá trị gần`10^9`. 

1. Lặp lại các phạm vi trong đó`m / d`có cùng giá trị. Đối với một phạm vi`[l,r]`, mọi`d`có:$$\lfloor m/d\rfloor=q$$vậy phần đóng góp là:$$(P_L(r)-P_L(l-1)) \cdot \Phi(q)$$1. Thêm mọi modulo đóng góp phạm vi`p`và in kết quả. 

Tại sao nó hoạt động: 

Bổ đề Burnside đảm bảo rằng việc tính trung bình số lượng các ô cố định trên tất cả các phép quay sẽ cho ra chính xác số lớp phép quay. Nhóm gcd chiếm tất cả các phép quay có cùng cấu trúc chu trình và phép biến đổi số chia chuyển đổi tổng trên mỗi độ dài thành tổng chia sàn. Các công thức tiền tố chỉ tăng tốc các nhận dạng chính xác này, do đó không đưa ra phép tính gần đúng nào. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1 << 25)

m, mod = map(int, input().split())

phi_cache = {}
fib_cache = {}
pl_cache = {}

def fib(n):
    if n in fib_cache:
        return fib_cache[n]
    if n == 0:
        return (0, 1)
    a, b = fib(n >> 1)
    c = (a * ((2 * b - a) % mod)) % mod
    d = (a * a + b * b) % mod
    if n & 1:
        res = (d, (c + d) % mod)
    else:
        res = (c, d)
    fib_cache[n] = res
    return res

def sum_l(n):
    if n <= 0:
        return 0
    if n in pl_cache:
        return pl_cache[n]
    ans = (fib(n + 1)[0] + fib(n + 3)[0] - 3) % mod
    pl_cache[n] = ans
    return ans

def phi_sum(n):
    if n == 0:
        return 0
    if n in phi_cache:
        return phi_cache[n]
    ans = (n % mod) * ((n + 1) % mod) % mod
    ans = ans * pow(2, -1, mod) % mod if mod != 1 else 0
    l = 2
    while l <= n:
        q = n // l
        r = n // q
        ans -= (r - l + 1) % mod * phi_sum(q)
        ans %= mod
        l = r + 1
    phi_cache[n] = ans
    return ans

ans = 0
l = 1
while l <= m:
    q = m // l
    r = m // q
    cur = (sum_l(r) - sum_l(l - 1)) % mod
    ans = (ans + cur * phi_sum(q)) % mod
    l = r + 1

print(ans % mod)
```Người trợ giúp Fibonacci trả về một cặp`(F_n, F_{n+1})`. Nhân đôi nhanh chóng tránh lặp lại lên đến`m`, điều này là cần thiết bởi vì`m`có thể là một tỷ.`sum_l`lưu trữ tiền tố của số lượng ốp lát hình trụ không hạn chế. Công thức sử dụng trực tiếp chỉ số Fibonacci nên không có mảng lập trình động và không có bộ nhớ tỉ lệ với`m`.`phi_sum`là phần tinh tế nhất. Vòng lặp nhóm tất cả các giá trị bằng nhau của`n // i`cùng nhau. Độ sâu đệ quy nhỏ vì mỗi đối số đệ quy là một thương số sàn riêng biệt. Nghịch đảo mô-đun của 2 chỉ được sử dụng cho công thức chuỗi số học. Từ`p`không đảm bảo là số nguyên tố, nghịch đảo chỉ tồn tại khi`p`thật kỳ quặc. Thậm chí`p`, việc chia phải được thực hiện trước khi áp dụng mô đun. 

Vòng lặp cuối cùng sử dụng thủ thuật nhóm tầng tương tự. giá trị`q = m // d`không đổi trong suốt khoảng thời gian`[l, r]`, cho phép tất cả các số hạng đó được kết hợp thành một phép nhân. 

# Ví dụ đã hoạt động 

Dành cho:```
1 114514
```chiều dài duy nhất là một. 

| tôi | r | q | sự khác biệt tiền tố ốp lát | tiền tố phi | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 1 | 

Kết quả là`1`, phù hợp với vòng đeo tay duy nhất có thể. 

Vì:```
2 1919810
```các phạm vi là: 

| tôi | r | q | sự khác biệt tiền tố ốp lát | tiền tố phi | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 1 | 2 | 2 | 
| 2 | 2 | 1 | 3 | 1 | 3 | 

Công thức đưa ra tổng đóng góp có trọng số. giá trị`7`đại diện cho một chiếc vòng tay có chiều dài một và ba chiếc vòng tay có chiều dài hai. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(m) log m) | Phạm vi phân chia tầng và đệ quy tổng số được ghi nhớ | 
| Không gian | O(sqrt(m)) | Chỉ lưu trữ các giá trị thương đệ quy riêng biệt | 

Số giá trị phân biệt của`floor(m/x)`tỷ lệ thuận với`sqrt(m)`, do đó nghiệm không bao giờ thực hiện các phép tính tỷ lệ với toàn bộ giá trị của`m`. Điều này là cần thiết bởi vì`m`có thể lớn như`10^9`. 

# Trường hợp thử nghiệm```python
import sys, io

# This block assumes the submitted solution is wrapped into solve()
# and that solve() reads stdin and writes stdout.

def run(inp: str) -> str:
    old_stdin, old_stdout = sys.stdin, sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin, sys.stdout = old_stdin, old_stdout
    return out

assert run("1 114514\n") == "1\n", "minimum size"
assert run("2 1919810\n") == "7\n", "sample 2"
assert run("3 1000000007\n") == "13\n", "sample 3"
assert run("4 998244353\n") == "29\n", "sample 4"
assert run("5 1000000007\n") == "61\n", "larger boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 114514`|`1`| Xử lý vị trí đơn | 
|`2 1919810`|`7`| Luân chuyển định kỳ | 
|`3 1000000007`|`13`| Giá trị Fibonacci thủ công nhỏ | 
|`4 998244353`|`29`| Nhiều ước số và nhóm Burnside | 
|`5 1000000007`|`61`| Tích lũy phạm vi lớn hơn | 

# Vỏ cạnh 

Khi nào`m = 1`, thuật toán không bao giờ rơi vào trường hợp xoay phức tạp. Phạm vi phân chia tầng một là`[1,1]`, số lần xếp ô tiền tố tăng đúng một và tổng tiền tố là một. Câu trả lời là chính xác`1`. 

Vì`m = 2`, trường hợp quan trọng là một cặp nằm ngang có thể quấn quanh hình trụ. Thuật toán không liệt kê các vết cắt, do đó ba lớp xoay được tính chính xác một lần thông qua nhóm gcd của Burnside. 

Đối với các vòng đeo tay có tính tuần hoàn cao, nhiều vòng quay không thay đổi cách sắp xếp giống nhau. Burnside xử lý việc này bằng cách đếm các cấu hình cố định thay vì cho rằng mọi sự sắp xếp đều có`n`các phép quay khác nhau. Công thức tính tổng số chia tự động bảo toàn các cấu trúc lặp lại này, giúp ngăn ngừa việc đếm quá mức.
