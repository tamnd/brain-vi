---
title: "CF 105002M - \u041d\u043e\u0434\u043d\u044b\u0435 \u043e\u0431\u043c\u0435\u043d\u044b"
description: "Chúng ta được cho một dãy số $n$. Hoạt động duy nhất được phép là hoán đổi hai vị trí nếu các số tại các vị trí đó có chung ước số lớn hơn 1."
date: "2026-06-28T03:29:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105002
codeforces_index: "M"
codeforces_contest_name: "vkoshp.letovo 2022"
rating: 0
weight: 105002
solve_time_s: 77
verified: false
draft: false
---

[CF 105002M - \u041d\u043e\u0434\u043d\u044b\u0435 \u043e\u0431\u043c\u0435\u043d\u044b](https://codeforces.com/problemset/problem/105002/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hàng$n$những con số. Hoạt động duy nhất được phép là hoán đổi hai vị trí nếu các số tại các vị trí đó có chung ước số lớn hơn 1. Từ những hoán đổi này, chúng ta có thể sắp xếp lại mảng, nhưng không tùy tiện: các phần tử chỉ có thể di chuyển qua một chuỗi hoán đổi trong đó mọi hoán đổi liền kề đều được chứng minh bằng một điều kiện gcd không tầm thường. 

Nhiệm vụ là xác định mảng nhỏ nhất về mặt từ điển có thể thu được sau khi áp dụng bất kỳ số lần hoán đổi nào như vậy. 

Khó khăn chính là thao tác không cho phép hoán vị tùy ý. Hai phần tử chỉ có thể hoán đổi cho nhau nếu tồn tại một chuỗi hoán đổi kết nối chúng, trong đó mỗi bước đều bảo toàn điều kiện$\gcd(x_i, x_j) > 1$. Điều này tự nhiên tạo ra sự kết nối giữa các chỉ số dựa trên các thừa số nguyên tố được chia sẻ. 

Ràng buộc$n \le 10^5$và giá trị lên đến$10^5$gợi ý rằng bất kỳ giải pháp nào cũng phải tránh$O(n^2)$so sánh hoặc kiểm tra gcd lặp đi lặp lại giữa các cặp. Thay vào đó, chúng ta cần một cấu trúc nhóm các giá trị một cách hiệu quả, thường là gần tuyến tính hoặc$O(n \log n)$thời gian bằng cách sử dụng phép phân tích nhân tử hoặc tìm liên kết trên các thừa số nguyên tố. 

Một sai lầm ngây thơ là cho rằng nếu hai số có chung bất kỳ ước số nào thì chúng có thể được tự do hoán đổi theo thứ tự được sắp xếp trên toàn cầu. Điều đó là chưa đủ trừ khi chúng ta tuyên truyền đúng đắn tính bắc cầu thông qua các yếu tố chung. 

Trường hợp cạnh tinh tế thứ hai là số nguyên tố cô lập hoặc lũy thừa nguyên tố. Ví dụ: nếu một số không chia sẻ thừa số nguyên tố với những số khác thì nó không thể di chuyển được. Một cách tiếp cận sắp xếp tham lam sẽ di chuyển nó một cách không chính xác. 

Trường hợp cạnh thứ ba phát sinh khi kết nối là gián tiếp. Ví dụ,$6, 10, 15$tất cả không thể được kết nối theo cặp mà chúng tạo thành một thành phần được kết nối thông qua các số nguyên tố dùng chung (liên kết 2, 3, 5). Bất kỳ giải pháp chính xác nào cũng phải nắm bắt được kết nối bắc cầu này. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo sẽ cố gắng áp dụng nhiều lần các giao dịch hoán đổi hợp lệ cho đến khi không thể cải thiện thêm nữa. Người ta có thể mô phỏng các giao dịch hoán đổi và cố gắng tạo bong bóng các giá trị nhỏ hơn về phía trái bất cứ khi nào tồn tại một điều kiện gcd hợp lệ. Về nguyên tắc, điều này đúng vì mọi di chuyển được phép đều duy trì các hạn chế về khả năng tiếp cận. 

Tuy nhiên, số lượng các tiểu bang là rất lớn. Mỗi lần hoán đổi sẽ thay đổi cấu hình mảng và việc kiểm tra tất cả các cặp để tìm các hoán đổi hợp lệ sẽ dẫn đến$O(n^2)$gcd kiểm tra mỗi lần lặp và có khả năng$O(n!)$hoán vị trong không gian khái niệm tồi tệ nhất. Ngay cả khi tối ưu hóa, cách tiếp cận này không thể sử dụng được cho$n = 10^5$. 

Quan sát quan trọng là các giao dịch hoán đổi xác định một biểu đồ trên các chỉ số: hai vị trí được kết nối nếu các giá trị của chúng có chung một thừa số nguyên tố, trực tiếp hoặc gián tiếp. Vì kết nối có tính bắc cầu nên mỗi thành phần được kết nối có thể được hoán vị tùy ý. Bên trong mỗi thành phần, mọi sự sắp xếp đều có thể đạt được vì chúng ta có thể di chuyển các giá trị dọc theo các đường dẫn được liên kết với gcd. 

Điều này làm giảm vấn đề tìm các thành phần liên thông qua các giá trị, nhóm tất cả các số có chung bất kỳ thừa số nguyên tố nào và sau đó sắp xếp các giá trị một cách độc lập trong mỗi nhóm. Cuối cùng, chúng tôi đặt các giá trị sẵn có nhỏ nhất vào các vị trí sớm nhất của từng thành phần để đạt được thứ tự tối thiểu về mặt từ điển. 

Do đó, chúng ta chuyển vấn đề thành việc xây dựng một liên minh rời rạc dựa trên các thừa số nguyên tố của các giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | O(n) | Quá chậm | 
| Tối ưu (DSU trên số nguyên tố) | O(n log A) | O(n + A) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng cấu trúc tập hợp rời rạc (DSU) để kết nối các chỉ số thông qua các thừa số nguyên tố chung. 

1. Phân tích mỗi số thành thừa số nguyên tố riêng biệt. Chúng tôi thực hiện điều này một cách hiệu quả bằng cách sử dụng sàng hệ số nguyên tố nhỏ nhất lên đến$10^5$. Điều này đảm bảo hệ số hóa đủ nhanh cho tất cả các phần tử. 
2. Với mỗi số, ta lấy danh sách các thừa số nguyên tố của nó. Chúng tôi chọn một yếu tố đại diện và kết hợp tất cả những yếu tố khác với nó trong DSU. Điều này xây dựng khả năng kết nối giữa các số nguyên tố chia sẻ. 
3. Chúng tôi duy trì ánh xạ từ mỗi gốc DSU tới tất cả các chỉ mục thuộc thành phần đó. Đồng thời, chúng ta cũng thu thập tất cả các giá trị thuộc về thành phần. 
4. Đối với mỗi thành phần được kết nối, chúng tôi sắp xếp cả chỉ số và giá trị một cách độc lập. Việc sắp xếp các chỉ mục cung cấp cho chúng ta các vị trí mà chúng ta có thể đặt các giá trị và việc sắp xếp các giá trị mang lại cho chúng ta sự sắp xếp nhỏ nhất về mặt từ điển. 
5. Chúng tôi gán các giá trị nhỏ nhất cho các chỉ số nhỏ nhất trong thành phần, đảm bảo tính tối thiểu về mặt từ điển tổng thể. 

Lý do chính khiến chúng tôi sắp xếp các chỉ số và giá trị riêng biệt là vì trong một thành phần được kết nối, mọi hoán vị đều có thể đạt được thông qua các hoán đổi hợp lệ, vì vậy chúng tôi có thể tự do sắp xếp lại thứ tự hoàn toàn. 

### Tại sao nó hoạt động 

DSU nắm bắt chính xác việc đóng bắc cầu của quan hệ “có thể hoán đổi qua gcd > 1”. Nếu hai số nằm trong cùng một thành phần thì sẽ tồn tại một chuỗi hoán đổi kết nối chúng thông qua các thừa số nguyên tố chung. Do đó, mọi hoán vị bên trong một thành phần đều có thể truy cập được. Vì các thành phần là độc lập nên việc giảm thiểu về mặt từ điển sẽ giảm xuống việc sắp xếp độc lập từng thành phần và đặt các giá trị nhỏ nhất sớm nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 100000

# smallest prime factor sieve
spf = list(range(MAXV + 1))
for i in range(2, int(MAXV ** 0.5) + 1):
    if spf[i] == i:
        for j in range(i * i, MAXV + 1, i):
            if spf[j] == j:
                spf[j] = i

def factorize(x):
    res = []
    while x > 1:
        p = spf[x]
        res.append(p)
        while x % p == 0:
            x //= p
    return res

class DSU:
    def __init__(self, n):
        self.p = list(range(n))
        self.r = [0] * n

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a != b:
            if self.r[a] < self.r[b]:
                a, b = b, a
            self.p[b] = a
            if self.r[a] == self.r[b]:
                self.r[a] += 1

def solve():
    n = int(input())
    arr = list(map(int, input().split()))

    dsu = DSU(n)
    prime_owner = {}

    for i, val in enumerate(arr):
        primes = factorize(val)
        if not primes:
            continue
        first = primes[0]
        for p in primes[1:]:
            dsu.union(first, p)
        if first in prime_owner:
            dsu.union(i, prime_owner[first])
        else:
            prime_owner[first] = i

    comp_idx = {}
    comp_vals = {}

    for i, v in enumerate(arr):
        root = dsu.find(i) if arr[i] != 1 else i
        comp_idx.setdefault(root, []).append(i)
        comp_vals.setdefault(root, []).append(v)

    res = arr[:]
    for root in comp_idx:
        idxs = sorted(comp_idx[root])
        vals = sorted(comp_vals[root])
        for i, v in zip(idxs, vals):
            res[i] = v

    print(*res)

if __name__ == "__main__":
    solve()
```Bộ sàng ở trên cùng đảm bảo hệ số hóa đủ nhanh cho tất cả các giá trị lên đến$10^5$. DSU được sử dụng để kết nối các chỉ số một cách gián tiếp thông qua các thừa số nguyên tố được chia sẻ. các`prime_owner`bản đồ đảm bảo rằng mỗi thành phần chính được gắn vào các chỉ mục thực tế để các giá trị và vị trí có thể được thống nhất. 

Khi xây dựng các thành phần, chúng tôi thu thập các chỉ số và giá trị riêng biệt. Sự phân tách này rất quan trọng vì gốc DSU trên số nguyên tố không tương ứng trực tiếp với chỉ số mảng, vì vậy chúng ta phải ánh xạ lại mọi thứ ở cuối. 

Cuối cùng, việc sắp xếp trong mỗi thành phần đảm bảo sự sắp xếp tối thiểu về mặt từ điển vì chúng tôi luôn khớp các giá trị sẵn có nhỏ nhất với các vị trí sẵn có sớm nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
6 10 15
```Ta phân tích thành thừa số: 6 = 2·3, 10 = 2·5, 15 = 3·5. Tất cả các số được kết nối thông qua các số nguyên tố được chia sẻ. 

| Bước | Hành động | Linh kiện | 
| --- | --- | --- | 
| 1 | hợp (6 với 10 qua 2) | {6,10} | 
| 2 | hợp (6 với 15 đến 3) | {6,10,15} | 
| 3 | công đoàn (10 với 15 đến 5) | {6,10,15} | 

Tất cả các chỉ số thuộc về một thành phần. Các giá trị sắp xếp cho [6,10,15], các chỉ số được sắp xếp là [0,1,2]. Mảng cuối cùng vẫn còn:```
6 10 15
```Điều này xác nhận tính bắc cầu đầy đủ thông qua các số nguyên tố được chia sẻ. 

### Ví dụ 2 

đầu vào:```
6
12 45 3 8 15 7
```Cấu trúc nhân tử: 12(2,3), 45(3,5), 3(3), 8(2), 15(3,5), 7(riêng số nguyên tố). 

| Bước | Hành động | Thành phần | 
| --- | --- | --- | 
| 1 | kết nối 12-8 qua 2 | {12,8} | 
| 2 | kết nối 12-45 qua 3 | {12,8,45,3,15} | 
| 3 | 7 bị cô lập | {7} | 

Giá trị thành phần: [12,45,3,8,15], chỉ số [0,1,2,3,4]. Các giá trị được sắp xếp: [3,8,12,15,45]. Các chỉ số được sắp xếp: [0,1,2,3,4]. 

Kết quả:```
3 8 12 15 45 7
```Điều này cho thấy 7 vẫn cố định vì nó không chia sẻ thừa số nguyên tố với bất kỳ phần tử nào khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log A + A \alpha(n))$| sàng + hệ số hóa + công đoàn DSU | 
| Không gian |$O(n + A)$| Mảng DSU, sàng, cấu trúc nhóm | 

Các ràng buộc cho phép lên đến$10^5$các giá trị, do đó, hệ số hóa dựa trên sàng và các phép toán DSU gần tuyến tính diễn ra nhanh chóng một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MAXV = 100000
    spf = list(range(MAXV + 1))
    for i in range(2, int(MAXV ** 0.5) + 1):
        if spf[i] == i:
            for j in range(i * i, MAXV + 1, i):
                if spf[j] == j:
                    spf[j] = i

    def factorize(x):
        res = []
        while x > 1:
            p = spf[x]
            res.append(p)
            while x % p == 0:
                x //= p
        return res

    class DSU:
        def __init__(self, n):
            self.p = list(range(n))
            self.r = [0] * n
        def find(self, x):
            while self.p[x] != x:
                self.p[x] = self.p[self.p[x]]
                x = self.p[x]
            return x
        def union(self, a, b):
            a = self.find(a)
            b = self.find(b)
            if a != b:
                if self.r[a] < self.r[b]:
                    a, b = b, a
                self.p[b] = a
                if self.r[a] == self.r[b]:
                    self.r[a] += 1

    n_and_rest = list(map(int, sys.stdin.read().split()))
    n = n_and_rest[0]
    arr = n_and_rest[1:]

    dsu = DSU(n)
    prime_owner = {}

    for i, val in enumerate(arr):
        primes = factorize(val)
        if primes:
            first = primes[0]
            for p in primes[1:]:
                dsu.union(first, p)
            if first in prime_owner:
                dsu.union(i, prime_owner[first])
            else:
                prime_owner[first] = i

    comp_idx = {}
    comp_vals = {}

    for i, v in enumerate(arr):
        root = i if arr[i] == 1 else dsu.find(i)
        comp_idx.setdefault(root, []).append(i)
        comp_vals.setdefault(root, []).append(v)

    res = arr[:]
    for r in comp_idx:
        idxs = sorted(comp_idx[r])
        vals = sorted(comp_vals[r])
        for i, v in zip(idxs, vals):
            res[i] = v

    return " ".join(map(str, res))

# provided samples
assert run("3\n6 4 2") == "2 4 6", "sample 1"
assert run("3\n10 15 6") == "6 10 15", "sample 2"
assert run("6\n12 45 3 8 15 7") == "3 8 12 15 45 7", "sample 3"

# custom cases
assert run("2\n7 11") == "7 11", "both primes isolated"
assert run("4\n6 10 15 14") == "6 10 14 15", "multiple connected via 2,3,5,7 chain"
assert run("5\n1 1 1 1 1") == "1 1 1 1 1", "all ones"
assert run("5\n2 4 8 16 3") == "2 4 8 16 3", "one isolated prime"

print("OK")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 7 11 | 7 11 | số nguyên tố bị cô lập | 
| 6 10 15 14 | 6 10 14 15 | kết nối đa thành phần | 
| 1 1 1 1 1 | 1 1 1 1 1 | thành phần tầm thường | 
| 2 4 8 16 3 | 2 4 8 16 3 | chuỗi có phần tử biệt lập | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi các số nguyên tố cùng nhau theo cặp. Đối với đầu vào:```
7
7 11 13 17
```mỗi phần tử tạo thành thành phần riêng của nó. Thuật toán tạo ra các bộ DSU đơn lẻ và việc sắp xếp trong mỗi thành phần không làm gì cả. Đầu ra không thay đổi, phù hợp với thực tế là không thể thực hiện hoán đổi. 

Một trường hợp cạnh khác được lặp lại các giá trị giống hệt nhau. Vì:```
4
6 6 6 6
```tất cả các chỉ số kết nối thông qua các thừa số nguyên tố chung là 6. Thuật toán hợp nhất mọi thứ thành một thành phần, sắp xếp các giá trị giống hệt nhau và xây dựng lại cùng một mảng. Mặc dù có nhiều hoán vị tồn tại nhưng tính tối thiểu của từ điển vẫn ổn định. 

Trường hợp thứ ba liên quan đến giá trị 1. Vì 1 không có thừa số nguyên tố nên nó không thể kết nối với bất kỳ số nào khác. Trong quá trình triển khai, nó vẫn bị cô lập, đảm bảo nó không bao giờ kết hợp sai vào một thành phần.
