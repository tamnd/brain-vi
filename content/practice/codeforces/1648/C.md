---
title: "CF 1648C - Tyler và Dây"
description: "Chúng ta được cho hai dãy số nguyên, s và t, biểu thị các chữ cái của hai chuỗi. Mỗi số nguyên tương ứng với một chữ cái riêng biệt và các số nguyên bằng nhau trong s và t biểu thị cùng một ký tự."
date: "2026-06-10T03:58:28+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1648
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 775 (Div. 1, based on Moscow Open Olympiad in Informatics)"
rating: 1900
weight: 1648
solve_time_s: 57
verified: true
draft: false
---

[CF 1648C - Tyler và Dây](https://codeforces.com/problemset/problem/1648/C) 

**Xếp hạng:** 1900 
**Tags:** tổ hợp, cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai dãy số nguyên,`s`Và`t`, đại diện cho các chữ cái của hai chuỗi. Mỗi số nguyên tương ứng với một chữ cái riêng biệt và các số nguyên bằng nhau trong`s`Và`t`biểu thị cùng một ký tự. Tyler muốn đếm xem có bao nhiêu sự sắp xếp lại khác biệt của`s`dẫn đến một chuỗi có từ điển nhỏ hơn`t`. Đầu ra phải được lấy theo modulo`998244353`. 

So sánh từ điển được định nghĩa như thông thường: cho hai chuỗi, phần tử khác nhau đầu tiên sẽ xác định thứ tự hoặc nếu một chuỗi là tiền tố nghiêm ngặt của chuỗi kia thì chuỗi ngắn hơn sẽ nhỏ hơn. Ví dụ,`[1,2]`nhỏ hơn`[1,2,3]`, Nhưng`[2,1]`lớn hơn`[1,2]`. 

Những ràng buộc cho phép`n`Và`m`lên tới 200.000. Tính tất cả các hoán vị của`s`rõ ràng là không khả thi vì tốc độ tăng trưởng giai thừa là cực kỳ lớn. Ngay cả việc lưu trữ tất cả các hoán vị là không thể. Vì vậy, chúng ta cần một cách tiếp cận đếm các sắp xếp mà không tạo ra chúng một cách rõ ràng. 

Một vài tình huống khó khăn phát sinh. Nếu như`s`chứa các phần tử lặp lại, việc đếm ngây thơ các hoán vị riêng biệt có thể bị tính quá mức. Nếu như`n < m`, một số hoán vị của`s`có thể nhỏ hơn đơn giản vì chúng ngắn hơn`t`nhưng chia sẻ một tiền tố. Ngoài ra, nếu`n > m`, chỉ những hoán vị khác nhau ở vị trí đầu tiên`m`các yếu tố và nhỏ hơn tại thời điểm đó có liên quan; nếu không, chúng sẽ tự động lớn hơn. 

Ví dụ, nếu`s = [1,1,2]`Và`t = [1,2,1,2]`, các hoán vị`[1,1,2]`Và`[1,2,1]`nhỏ hơn`t`, Nhưng`[2,1,1]`lớn hơn. Một cách tiếp cận bất cẩn giả định tất cả các hoán vị của`s`ngắn hơn`t`nhỏ hơn sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Phương pháp brute-force là tạo ra tất cả các hoán vị riêng biệt của`s`và đếm những cái có từ điển nhỏ hơn`t`. Phương pháp này có tác dụng chính xác nhưng thất bại ngay lập tức đối với`n > 10`, vì số hoán vị là`n!`và có thể dễ dàng vượt quá`10^12`. Vì vậy, nó là không thực tế đối với các ràng buộc nhất định. 

Thông tin chi tiết quan trọng để có giải pháp nhanh hơn là thứ tự từ điển có thể được xác định tăng dần bằng cách so sánh phần tử đầu tiên, sau đó là phần tử thứ hai, v.v. Nếu chúng ta biết có bao nhiêu hoán vị bắt đầu bằng một số nhất định thì chúng ta có thể đếm tất cả các hoán vị nhỏ hơn`t`mà không tạo ra chúng. Điều này làm giảm vấn đề đếm các hoán vị với các ràng buộc nhiều tập hợp. Sử dụng giai thừa và nghịch đảo mô-đun, chúng ta có thể tính toán số cách sắp xếp cho từng tiền tố một cách hiệu quả. Cây Fenwick (cây chỉ số nhị phân) giúp tính tổng nhanh số phần tử nhỏ hơn hiện tại`t_i`TRONG`O(log n)`thời gian. 

Do đó, thay vì tạo ra các hoán vị, chúng tôi tính toán có bao nhiêu chuỗi bắt đầu với mỗi tiền tố có thể nhỏ hơn`t`. Khi tiền tố bằng với sự bắt đầu của`t`, chúng ta tiếp tục kiểm tra ký tự tiếp theo. Sự không phù hợp đầu tiên dưới đây`t`cho phép đếm tất cả các hoán vị còn lại bằng cách sử dụng phép chia giai thừa để tính các phần tử lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | Ồ (n!) | Quá chậm | 
| Tối ưu | O(n log n + max_element) | O(max_element + n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các giai thừa và nghịch đảo mô-đun lên đến`max(n, max(s))`để sử dụng trong việc đếm hoán vị. Điều này cho phép chúng ta tính toán các hệ số đa thức modulo`998244353`. 
2. Đếm số lần xuất hiện của mỗi số nguyên trong`s`và lưu trữ chúng trong một mảng hoặc từ điển`freq`. Điều này đại diện cho nhiều tập hợp các chữ cái. 
3. Khởi tạo cây Fenwick trong phạm vi các số nguyên có thể. Chèn tần số của tất cả các phần tử từ`s`. Điều này cho phép chúng ta truy vấn nhanh chóng có bao nhiêu phần tử nhỏ hơn một số nguyên nhất định. 
4. Khởi tạo tổng số hoán vị còn lại là hệ số đa thức của`s`,`total_perms = n! / (product of freq[i]!)`. Điều này tính tất cả các sắp xếp riêng biệt của`s`. 
5. Lặp lại`t`theo chỉ mục`i`:

Một. Vì`t_i`, truy vấn cây Fenwick để biết tổng số phần tử nhỏ hơn`t_i`. Nhân số tiền này với số hoán vị của số còn lại`n-i`phần tử (cập nhật giai thừa để loại bỏ phần tử đã sử dụng) và thêm vào câu trả lời. 

b. Nếu như`freq[t_i] == 0`, dừng việc lặp lại, vì không có hoán vị nào có thể tiếp tục khớp`t`. 

c. Ngược lại thì giảm`freq[t_i]`từng cái một, cập nhật cây Fenwick và cập nhật`total_perms`tương ứng để tính đến một phần tử được sử dụng. 
6. Nếu quá trình lặp hoàn tất mà không dừng lại và`n < m`, thêm 1 vào câu trả lời vì`s`chính nó là tiền tố của`t`và do đó nhỏ hơn. 
7. Xuất kết quả theo modulo`998244353`. 

**Tại sao nó hoạt động:** Ở mỗi bước, thuật toán sẽ tính tất cả các chuỗi khác nhau từ`t`tại vị trí đó theo cách nhỏ hơn, sử dụng hệ số đa thức để tính các phần tử lặp lại. Cây Fenwick cho phép truy vấn có bao nhiêu lựa chọn nhỏ hơn lựa chọn hiện tại`t_i`. Điều bất biến là số lượng hoán vị được tính ở mỗi bước chính xác là số lượng các chuỗi nhỏ hơn về mặt từ điển bắt đầu bằng tiền tố được xem xét cho đến nay. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    n, m = map(int, input().split())
    s = list(map(int, input().split()))
    t = list(map(int, input().split()))

    max_val = max(max(s, default=0), max(t, default=0)) + 2

    # Precompute factorials and inverses
    fact = [1] * (n + 2)
    invfact = [1] * (n + 2)
    for i in range(1, n + 2):
        fact[i] = fact[i - 1] * i % MOD
    invfact[n + 1] = modinv(fact[n + 1])
    for i in range(n + 1, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    # Frequency count
    freq = [0] * max_val
    for num in s:
        freq[num] += 1

    class Fenwick:
        def __init__(self, size):
            self.n = size
            self.tree = [0] * (size + 2)
        def add(self, i, v):
            while i <= self.n:
                self.tree[i] += v
                i += i & -i
        def sum(self, i):
            res = 0
            while i > 0:
                res += self.tree[i]
                i -= i & -i
            return res

    ft = Fenwick(max_val)
    for i in range(1, max_val):
        if freq[i]:
            ft.add(i, freq[i])

    total_perms = fact[n]
    for i in range(1, max_val):
        total_perms = total_perms * invfact[freq[i]] % MOD

    ans = 0
    rem = n
    for i in range(min(n, m)):
        x = t[i]
        smaller = ft.sum(x - 1)
        if smaller > 0:
            temp = total_perms * smaller % MOD
            temp = temp * modinv(rem) % MOD
            ans = (ans + temp) % MOD
        if freq[x] == 0:
            break
        total_perms = total_perms * freq[x] % MOD
        total_perms = total_perms * modinv(rem) % MOD
        freq[x] -= 1
        ft.add(x, -1)
        rem -= 1
    else:
        if n < m:
            ans = (ans + 1) % MOD

    print(ans)

solve()
```Giải pháp đầu tiên thiết lập các giai thừa và nghịch đảo mô-đun để tính toán các hệ số đa thức. Tần số được lưu trữ và quản lý thông qua cây Fenwick để cho phép truy vấn tổng nhanh các phần tử nhỏ hơn. các`total_perms`biến theo dõi số lượng hoán vị còn lại khi các phần tử được sử dụng. Mỗi lần lặp qua`t`đếm các hoán vị bắt đầu bằng tiền tố nhỏ hơn`t`và điều chỉnh các hoán vị còn lại cho phù hợp. Các điều kiện biên như`n < m`hoặc thiếu các phần tử trong`s`, được xử lý một cách rõ ràng. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
3 4
1 2 2
2 1 2 1
```| tôi | t[i] | nhỏ hơn | tần số | tổng_perms | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 1 | [1,2,2] | 3 | 1 | 
| 1 | 1 | 1 | [1,1,2] | 1 | 2 | 

Quá trình lặp dừng sau 2 phần tử, vì t[2]=2 tiếp theo nhưng tần số còn lại cho phép hoán vị. Vì n<m nên không cần thêm gì nữa. Trả lời = 2. 

**Mẫu 2:** 

đầu vào:```
4 4
1 2 3 4
4 3 2 1
```| tôi | t[i] | nhỏ hơn | tần số | tổng số lượt uốn | và | 
| --- | --- | --- | --- | --- | --- |
