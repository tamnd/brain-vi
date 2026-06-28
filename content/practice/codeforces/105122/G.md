---
title: "CF 105122G - Con số khiêm tốn"
description: "Chúng ta được yêu cầu đếm các số nguyên trong một phạm vi $[a, b]$ sao cho mỗi số nguyên có đúng bảy ước số dương. Nhiệm vụ không phải là phân tích các số tùy ý một cách hiệu quả trực tuyến mà là tìm hiểu cấu trúc của các số có hàm chia bằng 7."
date: "2026-06-27T19:39:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105122
codeforces_index: "G"
codeforces_contest_name: "XXVI Interregional Programming Olympiad, Vologda SU, 2024"
rating: 0
weight: 105122
solve_time_s: 72
verified: false
draft: false
---

[CF 105122G - Con số khiêm tốn](https://codeforces.com/problemset/problem/105122/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đếm số nguyên trong một phạm vi$[a, b]$sao cho mỗi số nguyên có đúng bảy ước số dương. Nhiệm vụ không phải là phân tích các số tùy ý một cách hiệu quả trực tuyến mà là tìm hiểu cấu trúc của các số có hàm chia bằng 7. 

Đầu vào bao gồm hai số nguyên rất lớn, lên tới$10^{18}$, xác định một khoảng đóng. Đầu ra là số số nguyên bên trong khoảng này có số ước chính xác là bảy. 

Ràng buộc ngay lập tức loại trừ mọi cách tiếp cận lặp lại trong phạm vi. Ngay cả một lần vượt qua một phạm vi kích thước$10^{18}$là không thể. Thay vào đó, bất kỳ giải pháp hợp lệ nào cũng phải mô tả tất cả các số có chính xác bảy ước số và chỉ tạo ra các ứng cử viên đó. 

Một cạm bẫy phổ biến là cố gắng phân tích nhân tử nhanh chóng cho mỗi số trong phạm vi. Ngay cả khi mỗi hệ số được tối ưu hóa để$O(\sqrt{n})$, kích thước phạm vi làm cho phương pháp này không thể sử dụng được. 

Một vấn đề tế nhị khác là giả sử “chính xác có bảy ước số” hoạt động tương tự như các trường hợp phổ biến như 2, 4 hoặc 6 ước số. Đối với bảy, cấu trúc bị ràng buộc rất nhiều và việc thiếu cấu trúc đó dẫn đến không gian giải pháp hoàn toàn sai. 

## Phương pháp tiếp cận 

Chúng ta bắt đầu từ ý tưởng trực tiếp nhất: với mỗi số$x \in [a, b]$, tính số ước của nó bằng cách lặp đến$\sqrt{x}$, đếm các cặp số chia. Điều này đúng nhưng hoàn toàn không khả thi. Trong trường hợp xấu nhất, thậm chí kiểm tra một số gần$10^{18}$yêu cầu lên tới$10^9$lặp đi lặp lại và chúng tôi sẽ cần phải làm điều này trong tối đa$10^{18}$những con số. 

Quan sát quan trọng là phân loại các số có đúng bảy ước số. Vì 7 là số nguyên tố nên công thức đếm số chia buộc phải có cấu trúc rất cứng nhắc trong việc phân tích thành thừa số nguyên tố. 

Nếu một số$n = p_1^{e_1} p_2^{e_2} \cdots$, thì số ước là$(e_1+1)(e_2+1)\cdots$. Để tích này bằng 7, một số nguyên tố, chỉ có thể tồn tại một thừa số. Điều đó có nghĩa là số phải có dạng$p^6$, Ở đâu$p$là nguyên tố. 

Vì vậy, mọi số hợp lệ đều chính xác là lũy thừa thứ sáu của số nguyên tố. Điều này làm giảm vấn đề thành: đếm số nguyên tố$p$như vậy$p^6 \in [a, b]$. Tương tự, chúng ta cần các số nguyên tố trong khoảng:$$\lceil a^{1/6} \rceil \le p \le \lfloor b^{1/6} \rfloor.$$Phạm vi của$p$nhiều nhất là$(10^{18})^{1/6} = 10^3$, vì vậy chúng ta chỉ cần các số nguyên tố đến 1000. Chúng ta có thể tính trước các số nguyên tố một lần bằng cách sử dụng sàng và sau đó đếm xem có bao nhiêu số thỏa mãn giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(b-a)$hoặc tệ hơn |$O(1)$| Quá chậm | 
| Tối ưu (sàng sơ cấp + lọc) |$O(\sqrt[6]{b} + \log \log \sqrt[6]{b})$|$O(\sqrt[6]{b})$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính giới hạn dưới và giới hạn trên của số nguyên tố. Chúng tôi tìm thấy tất cả các số nguyên$p$như vậy$p^6$nằm ở$[a, b]$. Điều này được thực hiện bằng cách tính số nguyên căn bậc sáu của$a$Và$b$, chú ý điều chỉnh các lỗi làm tròn. 
2. Tính toán trước tất cả các số nguyên tố đến$\lfloor b^{1/6} \rfloor$bằng sàng Eratosthenes. Điều này hoạt động hiệu quả vì giới hạn tối đa là 1000. 
3. Lặp lại tất cả các số nguyên tố$p$từ sàng. 
4. Đối với mỗi số nguyên tố$p$, kiểm tra xem nó có nằm trong phạm vi tính toán hay không. Nếu có, nó đóng góp chính xác một số hợp lệ$p^6$. 
5. Đếm tất cả các số nguyên tố đó và đưa ra kết quả. 

### Tại sao nó hoạt động 

Hàm đếm số chia có tính nhân và chỉ bằng 7 khi hệ số nguyên tố chứa chính xác một số nguyên tố được nâng lên lũy thừa thứ sáu. Không có phép phân tích nhân tử nào khác có thể tạo ra số nguyên tố của các ước số vì bất kỳ thừa số nguyên tố bổ sung nào cũng sẽ nhân số ước số đó với ít nhất 2. Hạn chế này làm giảm toàn bộ bài toán thành bài toán đếm số nguyên tố giới hạn trong một khoảng rất nhỏ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def iroot6(x: int) -> int:
    lo, hi = 1, 10**3 + 5
    while lo <= hi:
        mid = (lo + hi) // 2
        v = mid**6
        if v <= x:
            lo = mid + 1
        else:
            hi = mid - 1
    return hi

def sieve(n: int):
    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = False
    for i in range(2, int(n**0.5) + 1):
        if is_prime[i]:
            for j in range(i * i, n + 1, i):
                is_prime[j] = False
    return is_prime

def solve():
    a = int(input().strip())
    b = int(input().strip())

    hi = iroot6(b)
    lo = iroot6(a)

    if lo**6 < a:
        lo += 1

    is_prime = sieve(hi)

    ans = 0
    for p in range(lo, hi + 1):
        if is_prime[p]:
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Cốt lõi của giải pháp là tính toán căn bậc sáu. Chúng tôi tránh sự mất ổn định của dấu phẩy động bằng cách sử dụng tìm kiếm nhị phân số nguyên, đảm bảo tính chính xác ngay cả khi ở gần các ranh giới lớn. Sự điều chỉnh`if lo**6 < a: lo += 1`khắc phục trường hợp cạnh trong đó giới hạn dưới được tính toán làm tròn xuống giá trị có lũy thừa thứ sáu vẫn nằm ngoài khoảng. 

Máy sàng chỉ chạy tối đa khoảng 1000 nên cực kỳ nhanh. Sau đó, chúng tôi lọc các số nguyên tố trong phạm vi giới hạn, tương ứng trực tiếp với lũy thừa thứ sáu hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
50
100
```Chúng tôi tính toán căn bậc sáu:$$50^{1/6} \approx 1,\quad 100^{1/6} \approx 2$$Vì vậy, các số nguyên tố ứng cử viên nằm trong$[1, 2]$. Số nguyên tố duy nhất là 2. 

| Bước | lo | xin chào | ứng viên p | p nguyên tố? | đếm | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 1 | 2 | - | - | 0 | 
| p=1 | 1 | 2 | 1 | không | 0 | 
| p=2 | 1 | 2 | 2 | vâng | 1 | 

Chỉ một$2^6 = 64$nằm trong phạm vi, vì vậy câu trả lời là 1. 

Điều này xác nhận rằng chỉ có một lũy thừa thứ sáu hợp lệ tồn tại trong khoảng. 

### Ví dụ 2 

đầu vào:```
1
1000000000000000000
```Chúng tôi tính toán:$$b^{1/6} = 1000$$Vì vậy chúng ta đếm số nguyên tố$p \le 1000$. 

| Bước | phạm vi p | hành động | đếm | 
| --- | --- | --- | --- | 
| ban đầu | 2..1000 | sàng số nguyên tố | 0 | 
| quét | số nguyên tố ≤ 1000 | đếm tất cả | 168 | 

Điều này cho thấy giải pháp giảm truy vấn phạm vi lớn thành một nhiệm vụ đếm số nguyên tố cố định. 

Dấu vết xác nhận rằng thuật toán chỉ phụ thuộc vào việc giới hạn không gian số mũ chứ không phụ thuộc vào việc quét khoảng ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt[6]{b} \log \log \sqrt[6]{b})$| sàng lên tới 1000 cộng với quét tuyến tính các số nguyên tố | 
| Không gian |$O(\sqrt[6]{b})$| mảng boolean cho sàng | 

Sự ràng buộc$\sqrt[6]{10^{18}} \le 1000$làm cho giải pháp có hiệu quả liên tục trong thời gian thực tế. Cả bộ nhớ và thời gian chạy đều không đáng kể trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip() if False else ""

# direct inline solution for testing
def solve_test(inp: str) -> str:
    import sys
    from io import StringIO

    def iroot6(x: int) -> int:
        lo, hi = 1, 10**3 + 5
        while lo <= hi:
            mid = (lo + hi) // 2
            v = mid**6
            if v <= x:
                lo = mid + 1
            else:
                hi = mid - 1
        return hi

    def sieve(n: int):
        is_prime = [True] * (n + 1)
        is_prime[0] = is_prime[1] = False
        for i in range(2, int(n**0.5) + 1):
            if is_prime[i]:
                for j in range(i * i, n + 1, i):
                    is_prime[j] = False
        return is_prime

    old = sys.stdin
    sys.stdin = StringIO(inp)

    a = int(input().strip())
    b = int(input().strip())

    hi = iroot6(b)
    lo = iroot6(a)
    if lo**6 < a:
        lo += 1

    is_prime = sieve(hi)

    ans = sum(1 for p in range(lo, hi + 1) if is_prime[p])

    sys.stdin = old
    return str(ans)

# samples
assert solve_test("50\n100\n") == "1"

# custom cases
assert solve_test("1\n63\n") == "0"
assert solve_test("1\n64\n") == "1"
assert solve_test("64\n64\n") == "1"
assert solve_test("1\n1000000000000000000\n") == "168"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 63 | 0 | chưa có lũy thừa thứ sáu của số nguyên tố | 
| 1 64 | 1 | bao gồm ranh giới của 2^6 | 
| 64 64 | 1 | phạm vi điểm đơn | 
| 1 10^18 | 168 | nguyên tố đầy đủ được thiết lập lên tới 1000 | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi giới hạn dưới nằm ngay trên lũy thừa thứ sáu hợp lệ. Ví dụ, nếu$a = 65$, sau đó$a^{1/6}$làm tròn xuống 2, nhưng$2^6 = 64$không nằm trong khoảng. Bước điều chỉnh`if lo**6 < a: lo += 1`đảm bảo chúng tôi loại trừ ứng cử viên được đưa vào không chính xác này. 

Một trường hợp khác là khi$a$Và$b$rất nhỏ. Vì$a = b = 1$, phép tính căn bậc sáu tạo ra$lo = hi = 1$, nhưng 1 không phải là số nguyên tố nên sàng chính xác sẽ cho kết quả bằng 0. 

Trường hợp cuối cùng là khi phạm vi lớn nhưng chứa tất cả các số hợp lệ. Vì$a = 1$,$b = 10^{18}$, thuật toán đếm tất cả các số nguyên tố lên đến 1000. Sàng đảm bảo tính chính xác vì mỗi số hợp lệ tương ứng duy nhất với một cơ số nguyên tố và không tồn tại sự trùng lặp hoặc khoảng trống trong ánh xạ.
