---
title: "CF 102318J - Bội số"
description: "Với mỗi truy vấn, chúng ta có hai số nguyên a và b. Chúng ta xét mọi số nguyên từ 1 đến b và đếm nó nếu nó chia hết cho ít nhất một số nguyên từ 2 đến a. Câu trả lời là kích thước của tập hợp bội số đó."
date: "2026-08-14T00:06:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102318
codeforces_index: "J"
codeforces_contest_name: "UCF Locals 2017"
rating: 0
weight: 102318
solve_time_s: 210
verified: true
draft: false
---

[CF 102318J - Bội số](https://codeforces.com/problemset/problem/102318/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Với mỗi truy vấn, chúng ta có hai số nguyên`a`Và`b`. Chúng tôi xem xét mọi số nguyên từ`1`bởi vì`b`và đếm nó nếu nó chia hết cho ít nhất một số nguyên từ`2`bởi vì`a`. Câu trả lời là kích thước của tập hợp bội số đó. 

Ví dụ, với`a = 3`Và`b = 30`, các ước số liên quan là`2`Và`3`. bội số của chúng trùng nhau nên câu trả lời là`15 + 10 - 5 = 20`. Tuyên bố cuộc thi ban đầu đưa ra chính xác ví dụ này. 

Những hạn chế là nhỏ đối với`a`, với`a <= 130`, Nhưng`b`có thể lớn như`10^15`, và có thể có tới`100`truy vấn. Sự kết hợp đó loại trừ việc lặp đi lặp lại`1..b`, dù chỉ một lần. Một vòng lặp`10^15`số nguyên vượt xa sáu giây có sẵn. Sự hạn chế về`a`là phần hữu ích: chỉ có`31`nhiều nhất là số nguyên tố`130`, vì vậy vấn đề thực sự là xử lý một tập hợp nhỏ các điều kiện chia hết cố định một cách hiệu quả. 

Sự tinh tế đầu tiên là khả năng chia chồng chéo. Vì`a = 3, b = 6`, các số nguyên`2, 3, 4, 6`là hợp lệ, vì vậy câu trả lời là`4`. Đơn giản chỉ cần thêm`6//2 + 6//3 = 5`đếm`6`hai lần. Bao gồm-loại trừ là bắt buộc. 

Trường hợp cạnh thứ hai là ranh giới trên. Vì`a = 15, b = 15`, câu trả lời bao gồm`15`bản thân nó, bởi vì`15`chia hết cho`3`Và`5`. Một công thức chỉ dựa trên`b // d // 2`có thể vô tình làm mất giá trị này khi đếm bội số lẻ. Số bội số lẻ chính xác của ước số lẻ`d`lên tới`b`là`(b // d + 1) // 2`. 

Trường hợp cạnh thứ ba là`b = 1`. Ví dụ,`a = 130, b = 1`có câu trả lời`0`, bởi vì không có số nguyên dương nào trong phạm vi chia hết cho bất kỳ số nguyên nào từ`2`bởi vì`130`. Bất kỳ phương pháp nào bắt đầu bằng`b - 1`hoặc giả định rằng một số ước số phải xảy ra sẽ thất bại ở đây. 

Trường hợp cạnh thứ tư là các ước số tổng hợp không cần các tập hợp loại trừ của riêng chúng. Nếu một số chia hết cho`12`, nó đã chia hết cho`2`Và`3`. Thêm một bộ riêng biệt cho bội số của`12`sẽ chỉ trùng lặp thông tin. Cuộc đánh giá chính thức của cuộc thi cũng đưa ra nhận xét tương tự và quy các ước số liên quan về số nguyên tố. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là kiểm tra mọi số nguyên`x`từ`1`ĐẾN`b`, và với mỗi`x`kiểm tra xem một số giá trị trong`2..a`chia nó. Ngay cả khi khả năng chia hết được kiểm tra một cách thông minh hơn, việc truy cập tất cả`b`giá trị đã có giá`O(b)`, có nghĩa là lên đến`10^15`lặp lại cho một truy vấn. Một lực lượng toán học mạnh mẽ hơn áp dụng phép loại trừ bao gồm cho các ước số nguyên tố. có`31`nhiều nhất là số nguyên tố`130`, vì vậy phiên bản không hạn chế sẽ xem xét tất cả`2^31`, hoặc về`2.15 * 10^9`, tập hợp con. Điều đó cũng quá lớn. Phân tích chính thức xác định chính xác trở ngại theo cấp số nhân này. 

Quan sát hữu ích là câu trả lời dễ mô tả hơn thông qua phần bổ sung của nó. Một số không được tính chính xác khi nó không chia hết cho bất kỳ số nguyên tố nào`a`. Giá trị tổng hợp trong`2..a`không thêm điều kiện mới vì mọi hợp số đều có thừa số nguyên tố không lớn hơn chính nó. 

Để số nguyên tố không vượt quá`a`là`p1, p2, ..., pk`. Định nghĩa`phi(x, k)`là số nguyên từ`1`bởi vì`x`không chia hết cho cái nào trong số này`k`số nguyên tố. Sau đó, câu trả lời được yêu cầu chỉ đơn giản là`b - phi(b, k)`. 

Hàm này có một phép lặp tiêu chuẩn. Nếu chúng ta đã biết số đếm tránh số đầu tiên`k-1`số nguyên tố thì trong số đó ta loại bỏ những số chia hết cho`pk`. Sau khi chia các số đó cho`pk`, những gì còn lại chính xác là tập hợp được tính bởi`phi(b // pk, k-1)`. Kể từ đây`phi(x, k) = phi(x, k-1) - phi(x // pk, k-1)`. 

Sự truy hồi là đúng, nhưng việc mở rộng nó một cách mù quáng vẫn tạo ra một cây hàm mũ. Tối ưu hóa thứ hai là đánh giá trực tiếp các trạng thái nhỏ và ghi nhớ các trạng thái lớn hữu ích. chỉ có`31`cấp độ bởi vì`a <= 130`. Chúng tôi cũng dừng lại ngay khi`x < pk`, bởi vì sau khi loại trừ tất cả các số nguyên tố thông qua`pk`, số nguyên dương duy nhất còn sót lại là`1`. 

Đây là hàm sàng từng phần tương tự được sử dụng trong các thuật toán đếm nguyên tố cổ điển. Sự lặp lại và tầm quan trọng của việc ghi nhớ là đặc tính tiêu chuẩn của`phi(x,k)`. 

Việc triển khai đạt được sẽ tránh được việc tạo ra hàng chục triệu sản phẩm cơ bản một cách rõ ràng. Đánh giá ban đầu của UCF mô tả cách triển khai loại trừ bao gồm thay thế nhằm tính toán trước về`23.6`triệu sản phẩm liên quan. Đối với Python, việc đánh giá phép truy toán sàng từng phần tương đương thực tế hơn đáng kể vì phép đệ quy chỉ cụ thể hóa các trạng thái mà các truy vấn thực sự đạt được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Liệt kê trực tiếp |`O(b)`mỗi truy vấn |`O(1)`| Quá chậm | 
| Bao gồm-loại trừ hoàn toàn |`O(2^31)`mỗi truy vấn |`O(31)`| Quá chậm | 
| Sàng từng phần được ghi nhớ | Số trạng thái dưới cấp số nhân thực tế, chỉ có 31 cấp độ nguyên tố |`O(S)`trạng thái được lưu trong bộ nhớ đệm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các truy vấn và xác định danh sách các số nguyên tố lên đến`130`. Đối với một truy vấn`(a, b)`, chỉ có số nguyên tố`p <= a`quan trọng, vậy hãy để`k`hãy là người đếm của họ. Điều này chuyển đổi phạm vi ban đầu của các ước số có thể`2..a`vào nhiều nhất`31`điều kiện nguyên tố riêng biệt. 
2. Xác định`phi(x, k)`bằng số số nguyên trong`[1, x]`không chia hết cho bất kỳ số nào đầu tiên`k`số nguyên tố. Câu trả lời mong muốn là`x - phi(x, k)`, bởi vì mọi số nguyên dương đều không bị ảnh hưởng bởi tất cả các số nguyên tố đó hoặc chia hết cho ít nhất một trong số chúng. 
3. Sử dụng`phi(x, 0) = x`. Không có số nguyên tố bị cấm, mọi số nguyên từ`1`bởi vì`x`sống sót. 
4. Đối với`k > 0`, sử dụng`phi(x, k) = phi(x, k-1) - phi(x // p[k-1], k-1)`. 

Số hạng đầu tiên giữ lại mọi số tránh được số nguyên tố trước đó. Số hạng thứ hai loại bỏ chính xác những người sống sót cũng chia hết cho số nguyên tố mới được đưa vào. 
5. Nếu`x < p[k-1]`, trở lại`1`tích cực`x`. Không có số nguyên nào khác ngoài`1`ít nhất có thể có thừa số nguyên tố`p[k-1]`trong khi còn lại nhiều nhất`x`, vậy chỉ`1`sống sót. 
6. Đối với nhỏ`k`, đánh giá trực tiếp sự lặp lại thay vì tạo nhiều lệnh gọi đệ quy. Bảy số nguyên tố yêu cầu nhiều nhất`2^7 = 128`các điều khoản bao gồm-loại trừ, rất nhỏ. 
7. Tính toán trước`phi(x, k)`cho tất cả`x < 800`và tất cả`k <= 31`. Khi trạng thái đệ quy trở nên nhỏ, nó có thể được trả lời trong thời gian không đổi. Đây là cách tối ưu hóa tiêu chuẩn cho các phép tính sàng từng phần và ngăn chặn đệ quy liên tục xây dựng lại các trạng thái nhỏ giống nhau. 
8. Ghi nhớ các trạng thái lớn mà các truy vấn đầu vào đạt được. Các nhánh khác nhau thường tạo ra cùng một cặp`(x, k)`, đặc biệt là sau khi chia số nguyên. Việc sử dụng lại các kết quả đó là nguyên nhân ngăn cản đệ quy hoạt động như một`2^k`cây. 
9. Với mỗi truy vấn, hãy tính`b - phi(b, k)`và in kết quả. Số nguyên Python có độ chính xác tùy ý, vì vậy các giá trị lên tới`10^15`không yêu cầu xử lý tràn đặc biệt. 

Điều bất biến là mọi cuộc gọi`phi(x, k)`biểu diễn chính xác các số nguyên trong`[1, x]`có các thừa số nguyên tố tránh được số nguyên tố đầu tiên`k`số nguyên tố. Phép lặp phân chia các số nguyên đó thành các số nguyên không chia hết cho`pk`và những cái chia hết cho`pk`. Cái sau được đặt trong sự tương ứng một-một với các giá trị được tính bằng`phi(x // pk, k-1)`. Vì hai nhóm rời rạc và đầy đủ nên mọi kết quả đệ quy đều chính xác. 

## Giải pháp Python```python
import sys
from functools import lru_cache

input = sys.stdin.readline

MAX_A = 130
SMALL = 800

def sieve_primes(n):
    is_prime = bytearray(b'\x01') * (n + 1)
    is_prime[0:2] = b'\x00\x00'
    p = 2
    while p * p <= n:
        if is_prime[p]:
            is_prime[p * p:n + 1:p] = b'\x00' * (((n - p * p) // p) + 1)
        p += 1
    return [i for i in range(2, n + 1) if is_prime[i]]

primes = sieve_primes(MAX_A)
K = len(primes)

# small_phi[x][k] = numbers <= x not divisible by the first k primes.
small_phi = [[0] * (K + 1) for _ in range(SMALL)]

for x in range(SMALL):
    small_phi[x][0] = x

for k in range(1, K + 1):
    p = primes[k - 1]
    for x in range(SMALL):
        if x < p:
            small_phi[x][k] = 1 if x > 0 else 0
        else:
            small_phi[x][k] = (
                small_phi[x][k - 1]
                - small_phi[x // p][k - 1]
            )

@lru_cache(maxsize=250000)
def phi(x, k):
    if x < SMALL:
        return small_phi[x][k]

    if k == 0:
        return x

    p = primes[k - 1]

    if x < p:
        return 1

    # For small k, direct inclusion-exclusion is tiny.
    if k <= 7:
        res = x
        # Add/subtract all non-empty subsets.
        products = [1]
        for i in range(k):
            p = primes[i]
            old = products[:]
            for v in old:
                nv = v * p
                if nv <= x:
                    products.append(nv)

        # Recompute with signs from the number of prime factors.
        res = x
        products = [1]
        for i in range(k):
            p = primes[i]
            old = products[:]
            for v in old:
                nv = v * p
                if nv <= x:
                    products.append(nv)
                    if len([]) == -1:
                        pass

        # The compact recursive version is clearer and has only 2^7 states.
        def dfs(i, product, sign):
            if i == k:
                return 0
            total = 0
            np = product * primes[i]
            if np <= x:
                total += sign * (x // np)
                total += dfs(i + 1, np, -sign)
            total += dfs(i + 1, product, sign)
            return total

        # Inclusion-exclusion gives the number removed from [1, x].
        removed = dfs(0, 1, 1)
        return x - removed

    return phi(x, k - 1) - phi(x // p, k - 1)

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        a, b = map(int, input().split())

        k = 0
        while k < K and primes[k] <= a:
            k += 1

        out.append(str(b - phi(b, k)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Sàng tạo nên`31`số nguyên tố lên đến`130`. Bản thân truy vấn không bao giờ cần biết bất cứ điều gì về ước số tổng hợp vì khả năng chia hết cho số tổng hợp đã được ngụ ý bởi khả năng chia hết cho một trong các thừa số nguyên tố của nó. 

các`small_phi`bảng xử lý mọi trạng thái với`x < 800`. Sự tái diễn của nó hoàn toàn giống với định nghĩa toán học, vì vậy nó không phải là một phép tính gần đúng hay một phương pháp phỏng đoán. Bảng chỉ đơn giản thay thế đệ quy lặp đi lặp lại bằng tra cứu theo thời gian liên tục. 

Bộ nhớ đệm`phi`hàm xử lý các trạng thái lớn hơn. các`x < p`kiểm tra là một điều kiện biên hữu ích: trả về`1`chỉ đúng với ý nghĩa tích cực`x`, trong khi`x = 0`phải quay lại`0`. Cái nhỏ-`k`chi nhánh sử dụng nhiều nhất`128`các lựa chọn tập hợp con và không đáng kể so với phần đệ quy lớn. 

Hai cái tạm thời`products`các công trình ở quy mô nhỏ`k`nhánh là không cần thiết cho việc tính toán thực tế và có thể được loại bỏ. Phiên bản sạch hơn sau đây là phiên bản cần được gửi:```python
import sys
from functools import lru_cache

input = sys.stdin.readline

MAX_A = 130
SMALL = 800

def sieve_primes(n):
    is_prime = bytearray(b'\x01') * (n + 1)
    is_prime[0:2] = b'\x00\x00'
    p = 2
    while p * p <= n:
        if is_prime[p]:
            is_prime[p * p:n + 1:p] = b'\x00' * (
                (n - p * p) // p + 1
            )
        p += 1
    return [i for i in range(2, n + 1) if is_prime[i]]

primes = sieve_primes(MAX_A)
K = len(primes)

small_phi = [[0] * (K + 1) for _ in range(SMALL)]

for x in range(SMALL):
    small_phi[x][0] = x

for k in range(1, K + 1):
    p = primes[k - 1]
    for x in range(SMALL):
        if x < p:
            small_phi[x][k] = 1 if x else 0
        else:
            small_phi[x][k] = (
                small_phi[x][k - 1]
                - small_phi[x // p][k - 1]
            )

@lru_cache(maxsize=250000)
def phi(x, k):
    if x < SMALL:
        return small_phi[x][k]

    if k == 0:
        return x

    p = primes[k - 1]

    if x < p:
        return 1

    if k <= 7:
        def dfs(i, product, sign):
            if i == k:
                return 0

            total = dfs(i + 1, product, sign)

            np = product * primes[i]
            if np <= x:
                total += sign * (x // np)
                total += dfs(i + 1, np, -sign)

            return total

        return x - dfs(0, 1, 1)

    return phi(x, k - 1) - phi(x // p, k - 1)

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        a, b = map(int, input().split())

        k = 0
        while k < K and primes[k] <= a:
            k += 1

        ans.append(str(b - phi(b, k)))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Khối mã thứ hai là phiên bản gửi. Số nguyên tố chỉ là`31`, vì vậy việc tìm`k`bởi một vòng lặp ngắn là không đáng kể. Bộ nhớ đệm được giới hạn một cách có chủ ý nên một lượng lớn các truy vấn không liên quan không thể tăng bộ nhớ mà không có giới hạn. Lần truy cập bộ đệm sẽ trả về ngay lập tức, trong khi tính chính xác của thuật toán không phụ thuộc vào kích thước bộ đệm cụ thể. 

## Ví dụ đã hoạt động 

Trang Codeforces được cung cấp không hiển thị đầu vào và đầu ra mẫu trong kết xuất HTML hiện tại của nó, trong khi câu lệnh UCF ban đầu cung cấp ví dụ`a = 3, b = 30`. Các dấu vết sau đây sử dụng ví dụ đó và truy vấn nhỏ thứ hai. 

Vì`a = 3, b = 30`, các số nguyên tố liên quan là`2`Và`3`. 

| Bước |`x`|`k`| Thủ tướng giới thiệu |`phi(x,k)`| 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 30 | 2 | 2, 3 | ? | 
| Xóa bội số của 3 khỏi`phi(30,1)`| 30 | 2 | 3 |`phi(30,1) - phi(10,1)`| 
| Tránh 2 | 30 | 1 | 2 |`15`| 
| Tránh 2 trong số`1..10`| 10 | 1 | 2 |`5`| 
| Cuối cùng | 30 | 2 | 2, 3 |`15 - 5 = 10`| 

có`10`số nguyên từ`1`bởi vì`30`không chia hết cho cả hai`2`cũng không`3`. Họ là`1, 5, 7, 11, 13, 17, 19, 23, 25, 29`. Trừ chúng khỏi`30`cho`20`, phù hợp với ví dụ đã nêu. 

Vì`a = 5, b = 10`, các số nguyên tố liên quan là`2, 3, 5`. 

| Bước |`x`|`k`| Ý nghĩa | 
| --- | --- | --- | --- | 
| Bắt đầu | 10 | 3 | Tránh 2, 3, 5 | 
| Lần chia đầu tiên | 10 | 2 |`phi(10,2) - phi(2,2)`| 
| Tránh 2 và 3 | 10 | 2 |`3`, cụ thể là`1, 7, 5`| 
| Tránh 2 và 3 trong`1..2`| 2 | 2 |`1`| 
| Cuối cùng | 10 | 3 |`3 - 1 = 2`| 

Hai số không chia hết cho`2`,`3`, hoặc`5`là`1`Và`7`. Do đó câu trả lời là`10 - 2 = 8`. Các số hợp lệ là`2, 3, 4, 5, 6, 8, 9, 10`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Thực tế dưới cấp số nhân về số lượng số nguyên tố có liên quan | Chỉ có 31 cấp độ đệ quy, các trạng thái nhỏ là tra cứu bảng và các trạng thái lặp lại được lưu vào bộ đệm | 
| Không gian |`O(800 * 31 + S)`| Bảng nhỏ cố định sử dụng khoảng 25.000 số nguyên, trong khi`S`là số lượng bộ nhớ đệm lớn`(x,k)`tiểu bang | 

Ràng buộc chính không phải là`b`bản thân nó, kể từ khi`b`có thể đạt được`10^15`. Thuật toán không bao giờ lặp lại trong phạm vi đó. Công của nó được xác định bởi số lượng nhỏ các số nguyên tố dưới đây`130`và bởi các trạng thái sàng từng phần riêng biệt được tạo ra thông qua phép chia số nguyên. Giới hạn nguyên tố cố định là điều làm cho phương pháp này trở nên thực tế. 

Giải pháp cuộc thi ban đầu thực hiện một lộ trình loại trừ bao gồm khác nhưng tương đương, chỉ tạo ra các sản phẩm có các số nguyên tố riêng biệt nằm dưới`10^15`; phân tích của nó báo cáo về`23.6`triệu sản phẩm được tạo ra. Công thức sàng một phần tránh hiện thực hóa toàn bộ bộ sưu tập đó và đặc biệt phù hợp với Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from functools import lru_cache

def solve_io(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    # Same implementation used by the submission.
    MAX_A = 130
    SMALL = 800

    def sieve_primes(n):
        is_prime = bytearray(b'\x01') * (n + 1)
        is_prime[0:2] = b'\x00\x00'
        p = 2
        while p * p <= n:
            if is_prime[p]:
                is_prime[p * p:n + 1:p] = b'\x00' * (
                    (n - p * p) // p + 1
                )
            p += 1
        return [i for i in range(2, n + 1) if is_prime[i]]

    primes = sieve_primes(MAX_A)
    K = len(primes)

    small_phi = [[0] * (K + 1) for _ in range(SMALL)]

    for x in range(SMALL):
        small_phi[x][0] = x

    for k in range(1, K + 1):
        p = primes[k - 1]
        for x in range(SMALL):
            if x < p:
                small_phi[x][k] = 1 if x else 0
            else:
                small_phi[x][k] = (
                    small_phi[x][k - 1]
                    - small_phi[x // p][k - 1]
                )

    @lru_cache(maxsize=250000)
    def phi(x, k):
        if x < SMALL:
            return small_phi[x][k]

        if k == 0:
            return x

        p = primes[k - 1]

        if x < p:
            return 1

        if k <= 7:
            def dfs(i, product, sign):
                if i == k:
                    return 0

                total = dfs(i + 1, product, sign)

                np = product * primes[i]
                if np <= x:
                    total += sign * (x // np)
                    total += dfs(i + 1, np, -sign)

                return total

            return x - dfs(0, 1, 1)

        return phi(x, k - 1) - phi(x // p, k - 1)

    data = sys.stdin.readline
    t = int(data())
    out = []

    for _ in range(t):
        a, b = map(int, data().split())

        k = 0
        while k < K and primes[k] <= a:
            k += 1

        out.append(str(b - phi(b, k)))

    sys.stdout = old_stdout
    sys.stdin = old_stdin
    return "\n".join(out)

# Provided statement example.
assert solve_io("1\n3 30\n") == "20", "a=3, b=30"

# Minimum b: no positive integer can be a multiple of 2.
assert solve_io("1\n2 1\n") == "0", "minimum b"

# Minimum a and exact boundary.
assert solve_io("1\n2 2\n") == "1", "2 itself is a multiple of 2"

# All numbers except 1 are covered when a >= b.
assert solve_io("1\n130 100\n") == "99", "every integer 2..100 is itself an allowed divisor"

# Composite divisors must not create double counting.
assert solve_io("1\n4 20\n") == "13", "divisibility by 4 adds nothing beyond divisibility by 2"

# Maximum-size query, checked by range and complement properties.
out = solve_io("2\n130 1000000000000000\n130 1000000000000000\n").splitlines()
assert len(out) == 2
assert out[0] == out[1], "identical maximum-size queries must reuse the same exact answer"
assert 0 <= int(out[0]) <= 10**15, "answer must stay inside the queried range"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 2 1`|`0`| tối thiểu`b`và tập rỗng các bội số hợp lệ | 
|`1 / 2 2`|`1`| Chính xác ranh giới dưới nơi số chia bằng`b`| 
|`1 / 130 100`|`99`| Mọi giá trị từ`2`bởi vì`100`được bảo hiểm | 
|`1 / 4 20`|`13`| Các ước số tổng hợp không được tính là điều kiện độc lập | 
|`2 / 130 10^15`lặp đi lặp lại | Cùng một giá trị hai lần | Tái sử dụng bộ đệm và số học kích thước tối đa | 

## Vỏ cạnh 

cho`a = 2, b = 1`, thuật toán tìm một số nguyên tố có liên quan,`2`. Số phần bù là`phi(1,1) = 1`, bởi vì`1`không chia hết cho`2`. Câu trả lời là`1 - 1 = 0`. Ranh giới được xử lý trước bất kỳ phép chia nào cho số nguyên tố. 

Vì`a = 2, b = 2`,`phi(2,1)`chỉ tính`1`, vậy câu trả lời là`2 - 1 = 1`. giá trị`2`chính nó được bao gồm vì định nghĩa sử dụng phạm vi đóng`1..b`. 

Vì`a = 3, b = 30`, phép tính đệ quy`phi(30,2) = phi(30,1) - phi(10,1) = 15 - 5 = 10`. Mười người sống sót là những số nguyên tố cùng nhau`6`, và hai mươi số còn lại chia hết cho`2`hoặc`3`. Điều này xác nhận rằng phép lặp lại xử lý sự chồng chéo mà không liệt kê các giao điểm theo cách thủ công. 

Vì`a = 4, b = 20`, các ước số cho phép là`2, 3, 4`, nhưng danh sách nguyên tố chỉ chứa`2`Và`3`. Đây là cố ý. Mỗi bội số của`4`đã là bội số của`2`, do đó thêm`4`không thể thay đổi liên minh. Thuật toán thu được`20 - phi(20,2) = 20 - 7 = 13`. 

Vì`a = 130, b = 100`, mọi số nguyên từ`2`bởi vì`100`bản thân nó là một ước số được phép. Do đó chỉ`1`bị loại trừ, đưa ra`99`. Thuật toán bao gồm tất cả các số nguyên tố cho đến`127`, nhưng cách diễn giải sàng từng phần vẫn tạo ra chính xác một số nguyên còn sót lại. 

Vì`a = 130, b = 10^15`, thuật toán không bao giờ cố gắng xây dựng cái đầu tiên`10^15`số nguyên. Nó chia đệ quy giới hạn nhiều nhất cho các số nguyên tố`127`và một khi trạng thái giảm xuống dưới`800`nó trở thành một bảng tra cứu. Số học số nguyên của Python biểu diễn một cách an toàn mọi giá trị trung gian liên quan đến số đếm.
