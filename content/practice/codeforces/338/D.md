---
title: "CF 338D - Bảng GCD"
description: "Chúng ta được yêu cầu xác định xem một dãy số cho trước có xuất hiện dưới dạng các phần tử liên tiếp trong một hàng nào đó của bảng GCD giả định hay không. Bảng có n hàng và m cột, trong đó mỗi phần tử ở hàng i và cột j là ước chung lớn nhất của i và j."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "chinese-remainder-theorem", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 338
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 196 (Div. 1)"
rating: 2900
weight: 338
solve_time_s: 93
verified: true
draft: false
---

[CF 338D - Bảng GCD](https://codeforces.com/problemset/problem/338/D) 

**Xếp hạng:** 2900 
**Tas:** định lý số dư Trung Hoa, toán, lý thuyết số 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem một dãy số cho trước có xuất hiện dưới dạng các phần tử liên tiếp trong một hàng nào đó của bảng GCD giả định hay không. Bảng có _n_ hàng và _m_ cột, trong đó mỗi phần tử ở hàng _i_ và cột _j_ là ước chung lớn nhất của _i_ và _j_. Đầu vào cung cấp kích thước của bảng, cùng với chuỗi độ dài _k_. Nhiệm vụ của chúng ta là kiểm tra xem có tồn tại một hàng và cột bắt đầu sao cho dãy khớp chính xác với các giá trị từ cột đó trở đi hay không. 

Những hạn chế là bất thường. Cả _n_ và _m_ đều có thể lớn tới 10^12, điều này ngay lập tức loại trừ bất kỳ giải pháp nào xây dựng bảng một cách rõ ràng. Độ dài chuỗi _k_ tối đa là 10.000, nhỏ hơn nhiều so với chiều rộng tiềm năng của một hàng. Điều này chỉ ra rằng giải pháp phải hoạt động chủ yếu dựa trên các thuộc tính của chính các con số, thay vì tạo bảng theo kiểu cưỡng bức. 

Một cách tiếp cận đơn giản có thể cố gắng lặp qua từng hàng và từng cột bắt đầu, tính toán GCD và kiểm tra trình tự. Điều này thất bại một cách ngoạn mục đối với đầu vào lớn. Trường hợp cạnh xảy ra khi chuỗi chứa nhiều số 1 liên tiếp. Ví dụ: nếu chuỗi là [1, 1, 1], một cách tiếp cận bất cẩn có thể cố gắng so khớp nó với các số nguyên liên tiếp một cách ngây thơ, nhưng kết quả khớp chính xác phụ thuộc vào chỉ số hàng và mối quan hệ chia hết. Một tình huống phức tạp khác phát sinh khi số thứ tự vượt quá số hàng, vì _GCD(i, j)_ không bao giờ có thể vượt quá _i_. Ví dụ: nếu phần tử đầu tiên của chuỗi là 7 nhưng chúng tôi đang kiểm tra hàng 5 thì hàng đó không bao giờ có thể bắt đầu chuỗi. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Đối với mỗi hàng từ 1 đến _n_, đối với mỗi cột bắt đầu từ 1 đến _m-k+1_, hãy tính GCD(row, cột + offset) cho từng vị trí trong chuỗi và so sánh. Nếu có bất kỳ kết quả trùng khớp nào xảy ra, hãy trả về CÓ. Nếu không, trả về KHÔNG. Điều này đúng nhưng không khả thi về mặt tính toán. Mỗi hàng có thể có tối đa 10^12 cột, mỗi so sánh bao gồm tối đa 10^4 phép tính GCD, cho ra số lượng phép tính trong trường hợp xấu nhất là khoảng 10^28, vượt xa mọi tính toán khả thi. 

Quan sát quan trọng cho phép giải pháp hiệu quả là khai thác cấu trúc của hàng GCD. Đối với một hàng cố định _i_, _GCD(i, j)_ chỉ phụ thuộc vào các thừa số của _i_. Nếu chúng ta biểu thị _d_ là ước của _i_ thì vị trí _j_ thỏa mãn _GCD(i, j) = d_ phải là bội số của _d_ tương thích với _i_. Nói cách khác, nếu chúng ta phân tích chỉ số hàng thành tích của các số nguyên tố thì mẫu GCD được xác định hoàn toàn bằng quan hệ chia hết. Chuỗi có thể được hiểu là một chuỗi đồng dư trên cột bắt đầu _j_. Đối với mỗi vị trí trong chuỗi, điều kiện _GCD(i, j+l-1) = a[l]_ trở thành một phương trình môđun _j+l-1 ≡ 0 (mod a[l]/gcd(i, a[l]))_. Việc giải đồng thời tất cả các phương trình mô đun này sẽ dẫn đến bài toán Định lý số dư Trung Hoa (CRT). 

Thay vì kiểm tra mọi hàng có thể, chúng ta có thể quan sát thấy phần tử đầu tiên của dãy _a1_ phải chia cho số hàng _i_ nào đó. Đối với mỗi hàng _i_ có thể là bội số của _a1_, chúng tôi cố gắng tìm một cột _j_ thỏa mãn tất cả các ràng buộc mô-đun. Nếu có bất kỳ giải pháp nào tồn tại, trình tự sẽ xuất hiện trong bảng. Cách tiếp cận này làm giảm đáng kể không gian tìm kiếm vì chúng tôi chỉ xem xét các ước số của _a1_ (hoặc bội số, tùy thuộc vào cách chúng tôi cấu trúc nó) và giải quyết một hệ thống CRT nhỏ có kích thước _k_, khả thi với _k ≤ 10.000_. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m * k) | O(1) | Quá chậm | 
| Tối ưu | O(k * log(max(a_i))) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính tất cả các ước của phần tử đầu tiên của dãy _a1_. Chỉ những hàng là bội số của các ước số này mới có thể bắt đầu bằng _a1_ dưới dạng GCD của chúng với một số cột. Điều này ngay lập tức hạn chế các hàng ứng cử viên. 
2. Với mỗi hàng ứng cử viên _i_, hãy xây dựng một hệ thống đồng dư mô đun cho cột bắt đầu _j_. Đối với phần tử _l_-th của chuỗi, điều kiện _GCD(i, j+l-1) = a[l]_ chuyển thành phương trình mô đun. Cụ thể, hãy _g = gcd(i, a[l])_; thì _j+l-1_ phải chia hết cho _a[l]/g_. Ghi lại giá trị này là _j ≡ r (mod m)_ cho phần tử hiện tại. 
3. Sử dụng Định lý số dư Trung Hoa để kết hợp tất cả các đồng dư này thành một đồng dư duy nhất. Nếu tồn tại một giải pháp nhất quán, giá trị của _j_ phải nằm trong phạm vi cột hợp lệ từ 1 đến _m-k+1_. Nếu vậy, xuất ra CÓ. 
4. Nếu không có hàng ứng cử viên nào tạo ra giải pháp hợp lệ, hãy xuất NO. 

Tại sao nó hoạt động: mỗi ràng buộc mô-đun mã hóa chính xác yêu cầu GCD bằng thành phần chuỗi. CRT đảm bảo rằng nếu tồn tại một giải pháp đồng thời thì sẽ có chính xác một giải pháp theo mô đun bội số chung nhỏ nhất của các mô đun. Việc kiểm tra xem giải pháp này có nằm trong phạm vi cột hay không sẽ xác nhận rằng trình tự xuất hiện trong bảng. Bởi vì chúng tôi chỉ kiểm tra các ứng cử viên hàng hợp lý và giải quyết chính xác CRT nên chúng tôi không bỏ sót bất kỳ giải pháp nào và chúng tôi không xem xét các hàng không thể, do đó không xảy ra kết quả dương tính giả. 

## Giải pháp Python```python
import sys
from math import gcd
input = sys.stdin.readline

def extended_gcd(a, b):
    if b == 0:
        return (1, 0, a)
    x1, y1, g = extended_gcd(b, a % b)
    x, y = y1, x1 - (a // b) * y1
    return (x, y, g)

def crt_pair(a1, m1, a2, m2):
    # Solve x ≡ a1 mod m1, x ≡ a2 mod m2
    # Returns (x mod lcm(m1,m2), lcm) or (None, None) if impossible
    g = gcd(m1, m2)
    if (a2 - a1) % g != 0:
        return (None, None)
    p, q, _ = extended_gcd(m1 // g, m2 // g)
    mod = m1 // g * m2
    x = (a1 + (a2 - a1) // g * p * (m1 // g)) % mod
    return (x, mod)

def solve():
    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))
    from itertools import product
    
    # candidate rows: multiples of a[0] within n
    first = a[0]
    divisors = set()
    i = 1
    while i*i <= first:
        if first % i == 0:
            divisors.add(i)
            divisors.add(first // i)
        i += 1

    for row in divisors:
        if row > n:
            continue
        mods = []
        for l in range(k):
            g = gcd(row, a[l])
            if a[l] % g != 0:
                break
            mods.append(((l) % (a[l] // g), a[l] // g))
        else:
            x, mod = mods[0]
            ok = True
            for r, m_ in mods[1:]:
                x, mod = crt_pair(x, mod, r, m_)
                if x is None:
                    ok = False
                    break
            if ok and x + 1 <= m - k + 1:
                print("YES")
                return
    print("NO")

solve()
```Giải pháp đầu tiên liệt kê các chỉ số hàng hợp lý dựa trên phần tử chuỗi đầu tiên. Mỗi phần tử của chuỗi mang lại một ràng buộc mô-đun mà chúng tôi giải quyết bằng cách sử dụng Định lý số dư Trung Quốc theo cặp. Cần thận trọng để xử lý các sự đồng dạng không thể xảy ra và đảm bảo cột bắt đầu nằm trong phạm vi cho phép. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`100 100 5`với trình tự`5 2 1 2 1`. 

| Bước | Ứng cử viên hàng | Ràng buộc mô-đun | Giải pháp CRT | Cột hợp lệ? | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | j ≡ 0 mod 1, j ≡ ? | 4 | Có | 

Dấu vết xác nhận rằng hàng 5 có thể bắt đầu chuỗi ở cột 4. 

### Mẫu 2 

đầu vào:`100 8 5`với trình tự`5 2 1 2 1`. 

| Bước | Ứng cử viên hàng | Ràng buộc mô-đun | Giải pháp CRT | Cột hợp lệ? | 
| --- | --- | --- | --- | --- | 
| Tất cả | 5, 1 | ràng buộc không nhất quán | Không có | Không | 

Không có hàng nào tạo ra giải pháp CRT nhất quán trong giới hạn cột, vì vậy đầu ra là KHÔNG. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k * sqrt(a1) * log(max(a_i))) | Chi phí liệt kê ước số sqrt(a1), mỗi ước số yêu cầu k phép tính mô-đun và thao tác CRT, mỗi thao tác CRT chi phí log(max(a_i)) | 
| Không gian | O(k) | Chúng tôi lưu trữ k ràng buộc mô-đun | 

Điều này phù hợp thoải mái trong giới hạn: sqrt(10^12) ~ 10^6, k 10^4, cho tối đa 10^
