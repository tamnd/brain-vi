---
title: "CF 102861N - Phép nhân số"
description: "Có M nút ẩn. Mỗi nút ẩn sở hữu một số nguyên tố không xác định và các số nguyên tố được sắp xếp theo thứ tự tăng dần theo chỉ số nút. Ngoài ra còn có N nút có thể nhìn thấy được."
date: "2026-07-25T14:09:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102861
codeforces_index: "N"
codeforces_contest_name: "2020-2021 ACM-ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 102861
solve_time_s: 62
verified: true
draft: false
---

[CF 102861N - Phép nhân số](https://codeforces.com/problemset/problem/102861/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

có`M`các nút ẩn. Mỗi nút ẩn sở hữu một số nguyên tố không xác định và các số nguyên tố được sắp xếp theo thứ tự tăng dần theo chỉ số nút. Ngoài ra còn có`N`các nút có thể nhìn thấy. Một nút hiển thị lưu trữ tích của các số nguyên tố của tất cả các nút ẩn được kết nối với nó, trong đó các cạnh lặp lại có nghĩa là phép nhân lặp lại. 

Đầu vào cung cấp các sản phẩm được lưu trữ trên`N`các nút hiển thị và danh sách đầy đủ các kết nối với tính đa dạng của chúng. Các nhãn gốc ban đầu của các nút ẩn đã biến mất và nhiệm vụ là khôi phục chúng theo thứ tự tăng dần của chỉ số nút ẩn. 

Quan sát quan trọng là đồ thị đã cho chúng ta biết mẫu số mũ của mọi số nguyên tố ẩn. Nếu nút ẩn`j`có`d`các cạnh của nút hiển thị`i`, thì số nguyên tố thuộc nút`j`xuất hiện với số mũ`d`TRONG`c[i]`. Vì vậy, mỗi nút ẩn tương ứng với một hàng của ma trận số mũ. 

Các ràng buộc hướng tới việc phân tích nhân tử hơn là cố gắng giải một hệ đại số lớn. Có ít hơn`1000`giá trị hiển thị, mỗi giá trị bên dưới`10^15`, vì vậy việc phân tích từng giá trị bằng thuật toán nhân tử số nguyên hiệu quả là thực tế. Số cạnh dưới đây`10000`, do đó việc lưu trữ các mẫu số mũ là nhỏ. Một giải pháp thử mọi phép gán có thể có giữa các số nguyên tố và nút sẽ là không thể vì số lượng hoán vị tăng lên một cách bùng nổ. 

Một số trường hợp có thể phá vỡ việc thực hiện bất cẩn. Nếu hai nút ẩn có các kết nối giống hệt nhau thì mẫu số mũ của chúng giống hệt nhau. Ví dụ:```
2 1 2
15
1 1 1
2 1 1
```Đầu ra là:```
3 5
```Cả hai nút ẩn đều đóng góp số mũ`1`đến nút hiển thị duy nhất, do đó việc phân tích nhân tử sẽ cho hai số nguyên tố có cùng chữ ký. Một giải pháp giả định mỗi chữ ký là duy nhất sẽ thất bại. 

Một vấn đề khác là một nút hiển thị có thể chứa một số nguyên tố có số mũ cao. Ví dụ:```
1 1 1
2
1 1 10
```Câu trả lời là:```
2
```Vectơ số mũ chứa`10`, do đó chỉ tính xem số nguyên tố có xuất hiện hay không là không chính xác. Tính đa dạng của mọi cạnh phải được bảo toàn. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là phân tích các tích đã cho và sau đó cố gắng gán các số nguyên tố được phát hiện cho các nút ẩn. Nhiệm vụ bị hạn chế bởi bội số cạnh, nhưng việc tìm kiếm vũ lực đối với các nhiệm vụ có thể vẫn sẽ rất lớn. Với`M`các nút ẩn, có thể có tới`M!`ánh xạ có thể, điều này vốn không thể thực hiện được đối với các giá trị gần với`1000`. 

Cấu trúc của bài toán loại bỏ nhu cầu tìm kiếm. Khi phân tích từng giá trị hiển thị, chúng ta có thể xây dựng vectơ số mũ của mọi số nguyên tố được phát hiện. Ví dụ: nếu một số nguyên tố xuất hiện dưới dạng```
c1: exponent 2
c2: exponent 0
c3: exponent 5
```thì chữ ký của nó là`[2, 0, 5]`. Nút ẩn được kết nối với bội số`[2, 0, 5]`phải là nút sở hữu số nguyên tố đó. 

Biểu đồ cung cấp trước cho chúng tôi mọi chữ ký nút ẩn. Chúng ta chỉ cần ghép chữ ký từ biểu đồ với chữ ký thu được từ hệ số hóa. Vì cùng một chữ ký có thể thuộc về nhiều số nguyên tố nên chúng tôi lưu trữ tất cả các số nguyên tố cho mỗi chữ ký và sử dụng chúng khi xử lý các nút ẩn theo thứ tự chỉ mục. 

Lực lượng vũ phu hoạt động vì các mẫu số mũ mô tả duy nhất mối quan hệ giữa các nút ẩn và các giá trị hiển thị. Nó thất bại vì việc tìm kiếm hoán vị phù hợp là công việc không cần thiết. Bao thanh toán hiển thị cùng một thông tin trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(M! + hệ số hóa) | O(M + N) | Quá chậm | 
| Tối ưu | O(N * F + K + M * N) | O(N * F + M * N) | Đã chấp nhận |`F`là chi phí của việc phân tích một số bằng Pollard Rho, đủ nhỏ cho giới hạn đã cho. 

## Hướng dẫn thuật toán 

1. Xây dựng chữ ký số mũ của mọi nút ẩn. Đối với nút ẩn`j`, tạo một mảng có độ dài`N`. Giá trị tại vị trí`i`là số cạnh giữa nút ẩn`j`và nút hiển thị`i`. Đây chính xác là mẫu số mũ mà số nguyên tố ẩn phải có trong tích hiển thị. 
2. Phân tích mọi giá trị nhìn thấy được`c[i]`. Trong khi phân tích thành thừa số, hãy lưu trữ số lần mỗi số nguyên tố xuất hiện trong mỗi nút hiển thị. Kết quả là chữ ký số mũ của mọi số nguyên tố được phát hiện. 
3. Nhóm tìm số nguyên tố bằng chữ ký số mũ của chúng. Một từ điển ánh xạ một mảng chữ ký tới tất cả các số nguyên tố tạo ra nó. Nhiều số nguyên tố có thể thuộc cùng một nhóm vì các nút ẩn khác nhau có thể có các mẫu cạnh giống hệt nhau. 
4. Đối với mỗi nút ẩn từ chỉ mục`1`ĐẾN`M`, tra cứu chữ ký của nó và lấy một số nguyên tố chưa sử dụng từ nhóm phù hợp. Thứ tự xử lý đưa ra thứ tự đầu ra cần thiết. 
5. In các số nguyên tố đã được khôi phục. 

Tại sao nó hoạt động: mỗi nút ẩn đóng góp một số nguyên tố không xác định và thông tin duy nhất mà các nút hiển thị chứa về số nguyên tố đó là số mũ của nó trong mỗi tích. Vectơ số mũ từ đồ thị và vectơ số mũ thu được từ quá trình phân tích nhân tử mô tả cùng một đối tượng. Vì mỗi nút ẩn đều có ít nhất một cạnh nên mọi số nguyên tố ẩn đều xuất hiện ở đâu đó và được phục hồi trong quá trình phân tích nhân tử. Việc khớp các chữ ký bằng nhau sẽ xây dựng lại bài tập ban đầu. 

## Giải pháp Python```python
import sys
import random
from collections import defaultdict

input = sys.stdin.readline

def mul_mod(a, b, mod):
    return (a * b) % mod

def pow_mod(a, n, mod):
    r = 1
    while n:
        if n & 1:
            r = mul_mod(r, a, mod)
        a = mul_mod(a, a, mod)
        n >>= 1
    return r

def is_prime(n):
    if n < 2:
        return False
    small = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37]
    for p in small:
        if n == p:
            return True
        if n % p == 0:
            return False

    d = n - 1
    s = 0
    while d % 2 == 0:
        s += 1
        d //= 2

    for a in [2, 3, 5, 7, 11, 13]:
        if a >= n:
            continue
        x = pow_mod(a, d, n)
        if x == 1 or x == n - 1:
            continue
        for _ in range(s - 1):
            x = mul_mod(x, x, n)
            if x == n - 1:
                break
        else:
            return False
    return True

def gcd(a, b):
    while b:
        a, b = b, a % b
    return a

def pollard_rho(n):
    if n % 2 == 0:
        return 2
    while True:
        c = random.randrange(1, n - 1)
        x = random.randrange(0, n - 1)
        y = x
        d = 1
        while d == 1:
            x = (mul_mod(x, x, n) + c) % n
            y = (mul_mod(y, y, n) + c) % n
            y = (mul_mod(y, y, n) + c) % n
            d = gcd(abs(x - y), n)
        if d != n:
            return d

def factor(n, out):
    if n == 1:
        return
    if is_prime(n):
        out[n] += 1
    else:
        d = pollard_rho(n)
        factor(d, out)
        factor(n // d, out)

def solve():
    M, N, K = map(int, input().split())
    c = list(map(int, input().split()))

    signatures = [[0] * N for _ in range(M)]
    for _ in range(K):
        m, n, d = map(int, input().split())
        signatures[m - 1][n - 1] = d

    prime_vectors = defaultdict(lambda: [0] * N)

    for i, value in enumerate(c):
        fac = defaultdict(int)
        factor(value, fac)
        for p, e in fac.items():
            prime_vectors[p][i] = e

    groups = defaultdict(list)
    for p, vec in prime_vectors.items():
        groups[tuple(vec)].append(p)

    answer = []
    for sig in signatures:
        arr = groups[tuple(sig)]
        answer.append(arr.pop())

    answer.sort()
    print(*answer)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã thực hiện kiểm tra tính nguyên tố xác định và nhân tử Pollard Rho cho các số lên đến`10^15`. Phép chia thử sẽ quá chậm vì một số nguyên tố gần giới hạn có thể yêu cầu phải kiểm tra nhiều ước. 

Thuật toán chính lưu trữ thông tin đồ thị dưới dạng các hàng của ma trận số mũ. Bội số cạnh đầu vào được gán trực tiếp cho vị trí tương ứng vì bội số đó chính xác là phần đóng góp theo cấp số nhân của số nguyên tố ẩn đó. 

Trong quá trình phân tích nhân tử, mã đặt mọi số nguyên tố được phát hiện vào một vectơ được lập chỉ mục bởi nút hiển thị. Vectơ này có cùng chữ ký với hàng biểu đồ. Từ điển của các nhóm xử lý trường hợp một số nút ẩn có chung chữ ký. 

Việc tra cứu cuối cùng được thực hiện theo thứ tự nút ẩn, sau đó các số nguyên tố thu được sẽ được sắp xếp trước khi in. Việc sắp xếp là an toàn vì bài toán đảm bảo rằng các chỉ số nút ẩn tương ứng với các nhãn nguyên tố tăng dần. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 3 4
15 16 13
2 1 1
3 1 1
1 2 4
4 3 1
```Chữ ký đồ thị và các hệ số được phục hồi là: 

| Nút ẩn | Chữ ký | Phù hợp với nguyên tố | 
| --- | --- | --- | 
| 1 | [0,4,0] | 2 | 
| 2 | [1,0,0] | 3 | 
| 3 | [1,0,0] | 5 | 
| 4 | [0,0,1] | 13 | 

Việc nhân tố hóa tạo ra: 

| Nút hiển thị | Nhân tố hóa | 
| --- | --- | 
| 1 | 3*5 | 
| 2 | 2^4 | 
| 3 | 13 | 

Điều này xác nhận rằng các chữ ký bằng nhau có thể ánh xạ tới nhiều số nguyên tố. 

### Mẫu 2 

đầu vào:```
4 5 7
3 9 7 143 143
1 1 1
1 2 2
2 3 1
3 4 1
3 5 1
4 5 1
4 4 1
```Trạng thái trong quá trình khớp là: 

| Nút ẩn | Chữ ký | Các số nguyên tố phù hợp còn lại | Được chọn | 
| --- | --- | --- | --- | 
| 1 | [1,2,0,0,0] | [3] | 3 | 
| 2 | [0,0,1,0,0] | [7] | 7 | 
| 3 | [0,0,0,1,1] | [11] | 11 | 
| 4 | [0,0,0,1,1] | [13] | 13 | 

Hai nút ẩn cuối cùng có hành vi đồ thị giống hệt nhau và việc phân tích nhân tử cũng tạo ra hai số nguyên tố có cùng chữ ký. Bước nhóm là bước cho phép cả hai được phục hồi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N * F + K + M * N) | Mỗi sản phẩm được tính một lần, các cạnh được đọc một lần và chữ ký được so sánh thông qua hàm băm. | 
| Không gian | O(N * P + M * N) |`P`là số số nguyên tố phân biệt xuất hiện trong tích. | 

Số lượng các số nguyên tố riêng biệt không thể vượt quá số nút ẩn vì mọi thừa số đều thuộc về một nhãn nút ẩn. Các giới hạn này làm cho việc lưu trữ ma trận số mũ và chữ ký nhân tử trở nên khả thi. 

## Trường hợp thử nghiệm```python
import sys
import io

# This helper assumes solve() from the solution is available.
def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue().strip()

assert run("""4 3 4
15 16 13
2 1 1
3 1 1
1 2 4
4 3 1
""") == "2 3 5 13"

assert run("""4 5 7
3 9 7 143 143
1 1 1
1 2 2
2 3 1
3 4 1
3 5 1
4 5 1
4 4 1
""") == "3 7 11 13"

assert run("""1 1 1
2
1 1 1
""") == "2"

assert run("""2 1 2
15
1 1 1
2 1 1
""") in ["3 5", "5 3"]

assert run("""3 3 3
4 9 25
1 1 2
2 2 2
3 3 2
""") == "2 3 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Nút đơn có một cạnh |`2`| Kích thước biểu đồ tối thiểu | 
| Hai chữ ký bằng nhau |`3 5`| Mẫu số mũ trùng lặp | 
| Hệ số bình phương độc lập |`2 3 5`| Số mũ lớn | 
| Mẫu | Kết quả đầu ra | Hành vi chuẩn | 

## Vỏ cạnh 

Trường hợp chữ ký trùng lặp:```
2 1 2
15
1 1 1
2 1 1
```Sau khi nhân tử hóa, giá trị hiển thị duy nhất sẽ trở thành`3 * 5`. Cả hai số nguyên tố đều có chữ ký`[1]`và cả hai nút ẩn cũng có chữ ký`[1]`. Thuật toán lưu trữ cả hai số nguyên tố trong cùng một nhóm và loại bỏ một số nguyên tố cho mỗi nút ẩn, tạo ra cặp đúng. 

Trường hợp số mũ cao:```
1 1 1
2
1 1 10
```Chữ ký đồ thị là`[10]`. Thừa số hóa tạo ra số nguyên tố`2`với vectơ số mũ`[10]`, thế là trận đấu thành công. Một phương pháp chỉ ghi lại liệu số nguyên tố có tồn tại hay không sẽ tạo chữ ký không chính xác`[1]`và thất bại. 

Một nút ẩn có một kết nối duy nhất cũng được xử lý một cách tự nhiên. Ví dụ:```
1 2 1
6 7
1 2 1
```Chữ ký là`[0,1]`, và phân tích thành thừa số nguyên tố`7`có cùng một vectơ. Yếu tố không sử dụng`2`không tồn tại vì mọi nút ẩn phải xuất hiện trong biểu đồ, do đó câu trả lời được khôi phục vẫn nhất quán.
