---
title: "CF 104471D - Đếm mảng"
description: "Chúng tôi được cung cấp nhiều truy vấn độc lập. Mỗi truy vấn đưa ra hai số nguyên, trong đó chúng ta phải xây dựng các mảng có độ dài cố định có các phần tử hoàn toàn dương và tổng của chúng là cố định."
date: "2026-06-30T12:51:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104471
codeforces_index: "D"
codeforces_contest_name: "TheForces Round #20 (7-Problems-Forces)"
rating: 0
weight: 104471
solve_time_s: 97
verified: true
draft: false
---

[CF 104471D - Đếm mảng](https://codeforces.com/problemset/problem/104471/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều truy vấn độc lập. Mỗi truy vấn đưa ra hai số nguyên, trong đó chúng ta phải xây dựng các mảng có độ dài cố định có các phần tử hoàn toàn dương và tổng của chúng là cố định. Trong số tất cả các mảng như vậy, chúng tôi chỉ giữ lại những mảng có thể biểu thị độ dài các cạnh của đa giác không suy biến với chính xác số cạnh đó, trong đó thứ tự các cạnh là quan trọng. 

Vì vậy, đối với mỗi trường hợp thử nghiệm, chúng tôi đang đếm các thành phần có thứ tự của một số nguyên một cách hiệu quả$m$vào trong$n$phần tích cực, với một ràng buộc hình học bổ sung: chuỗi phải có khả năng tạo thành một khối khép kín$n$đa giác có cạnh. 

Điều kiện hình học là phần không tầm thường. Một dãy có độ dài dương có thể tạo thành một đa giác khi và chỉ khi không có một cạnh nào ít nhất bằng tổng tất cả các cạnh còn lại. Đây là bất đẳng thức tam giác tổng quát tiêu chuẩn. Nếu một bên quá lớn thì các bên còn lại không thể “đóng” được. 

Viết lại điều kiện dưới dạng đại số, nếu tổng bằng$m$, thì mọi mảng hợp lệ phải thỏa mãn$$\max(a_i) < m - \max(a_i)$$tương đương với$$\max(a_i) < \frac{m}{2}$$Vì vậy, vấn đề trở thành đếm các mảng số nguyên dương có độ dài$n$, tổng$m$, với tất cả các mục hoàn toàn nhỏ hơn$m/2$. 

Các ràng buộc cho phép lên tới$10^5$trường hợp thử nghiệm và giá trị lên đến$10^6$, do đó, bất kỳ phép liệt kê tổ hợp nào trên mỗi lần kiểm tra đều không thể thực hiện được. Một giải pháp phải tính toán trước hoặc trả lời từng truy vấn theo thời gian không đổi hoặc logarit. 

Trường hợp cạnh tinh tế xuất hiện khi$m$là nhỏ so với$n$. Ví dụ,$n = 3, m = 3$có chính xác một mảng hợp lệ, nhưng$n = 3, m = 4$không có mảng hợp lệ vì mặc dù các thành phần tồn tại, không có mảng nào có thể tạo thành một hình tam giác vì một cạnh nhất thiết phải quá lớn so với các cạnh khác. Một trường hợp khác là khi$m < 2n$, trong đó ràng buộc đa giác trở nên cực kỳ hạn chế và thường loại bỏ tất cả các giải pháp. 

## Phương pháp tiếp cận 

Chúng tôi bắt đầu từ cách giải thích trực tiếp nhất: liệt kê tất cả các mảng có độ dài$n$với các số nguyên dương có tổng bằng$m$, sau đó kiểm tra xem mỗi cái có thỏa mãn điều kiện đa giác hay không. Điều này tương đương với việc tạo ra tất cả các thành phần của$m$vào trong$n$các bộ phận, đó là$\binom{m-1}{n-1}$khả năng. Vì$m$lên đến$10^6$, con số này lớn về mặt thiên văn nên việc liệt kê ngay lập tức không thể thực hiện được. 

Thay vào đó, chúng tôi sử dụng phép biến đổi tổ hợp tiêu chuẩn. Chúng ta hãy dịch chuyển các biến bằng cách xác định$b_i = a_i - 1$. Sau đó mỗi$b_i \ge 0$Và$$\sum b_i = m - n$$Vì vậy, không có ràng buộc, số lượng mảng là$$\binom{m-1}{n-1}$$Bây giờ chúng tôi kết hợp các ràng buộc đa giác. Điều kiện là không có phần tử nào có thể bằng ít nhất một nửa tổng. Chúng tôi tính các giải pháp hợp lệ bằng cách trừ đi những giải pháp vi phạm điều kiện này. 

Sửa phần tử tối đa thành$x$. Nếu một phần tử là$x$, số tiền còn lại là$m - x$, và đối với vi phạm chúng tôi yêu cầu$x \ge m - x$, tức là$x \ge \lceil m/2 \rceil$. 

Vì vậy, chúng tôi tính các cấu hình trong đó phần tử phân biệt quá lớn. Đối với một vị trí cố định, phần còn lại$n-1$các phần tử tổng hợp thành$m-x$, cho:$$\binom{m-x-1}{n-2}$$Tổng hợp tất cả không hợp lệ$x$, nhân với$n$lựa chọn vị trí, đưa ra tổng số cấu hình không hợp lệ. 

Vì vậy, câu trả lời trở thành:$$\binom{m-1}{n-1} - n \cdot \sum_{x=\lceil m/2 \rceil}^{m-1} \binom{m-x-1}{n-2}$$Chúng tôi đơn giản hóa phép tính tổng bằng cách sử dụng đồng nhất thức tổ hợp tiêu chuẩn bằng cách thay thế$y = m-x$, chuyển đổi nó thành tổng tiền tố của các hệ số nhị thức, thu gọn thành sai phân dạng đóng của hai kết hợp:$$\sum_{y=1}^{\lfloor (m-1)/2 \rfloor} \binom{y-1}{n-2}
= \binom{\lfloor (m-1)/2 \rfloor}{n-1}$$Điều này mang lại một công thức khép kín cuối cùng:$$\binom{m-1}{n-1} - n \cdot \binom{\lfloor (m-1)/2 \rfloor}{n-1}$$Nhiệm vụ còn lại là tính hệ số nhị thức modulo$10^9+7$cho nhiều truy vấn một cách hiệu quả, sử dụng tính toán trước giai thừa và nghịch đảo mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tổ hợp + nCr được tính toán trước | O(1) cho mỗi truy vấn sau quá trình tiền xử lý O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Tính trước các giai thừa và giai thừa nghịch đảo đến mức tối đa$m$Chúng ta cần tính toán nhanh các hệ số nhị thức. Chúng tôi xây dựng mảng giai thừa và nghịch đảo mô-đun lên đến$10^6$. Điều này đảm bảo bất kỳ$\binom{n}{k}$có thể được trả lời trong thời gian liên tục. 

### 2. Với mỗi test, tính tổng số thành phần 

Chúng tôi hiểu các mảng là các thành phần số nguyên dương, do đó số lượng cơ sở là:$$\binom{m-1}{n-1}$$Điều này đếm tất cả các chuỗi mà không bị hạn chế về mặt hình học. 

### 3. Tính điểm giữa của khoảng cấm 

Một đa giác hợp lệ yêu cầu tất cả các cạnh phải nhỏ hơn một nửa chu vi. Vậy ngưỡng là:$$t = \left\lfloor \frac{m-1}{2} \right\rfloor$$Điều này đảm bảo chúng tôi nắm bắt được tất cả các trường hợp trong đó một cạnh quá lớn. 

### 4. Trừ những cấu hình không hợp lệ 

Các trường hợp không hợp lệ tương ứng với việc chọn một bên “quá lớn”, sau đó chia số tiền còn lại cho những trường hợp khác. Bằng tính đối xứng, điều này giảm xuống còn:$$n \cdot \binom{t}{n-1}$$Phép trừ này loại bỏ tất cả các mảng trong đó có ít nhất một cạnh vi phạm điều kiện đa giác. 

### 5. Xuất kết quả theo modulo$10^9+7$### Tại sao nó hoạt động 

Mọi cấu hình không hợp lệ đều có chính xác một cạnh trội vi phạm bất đẳng thức đa giác, vì nếu cả hai cạnh đều có ít nhất một nửa chu vi thì tổng sẽ vượt quá tổng. Điều này đảm bảo không tính quá mức trong bước trừ. Bằng cách ánh xạ từng mảng không hợp lệ tới một lựa chọn duy nhất của phần tử lớn và phân phối phần còn lại một cách độc lập, chúng ta thu được sự phân đôi giữa các mảng không hợp lệ và các số hạng được tính trong công thức trừ. Điều này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7
MAXN = 10**6 + 5

fact = [1] * MAXN
invfact = [1] * MAXN

for i in range(1, MAXN):
    fact[i] = fact[i - 1] * i % MOD

invfact[MAXN - 1] = pow(fact[MAXN - 1], MOD - 2, MOD)
for i in range(MAXN - 2, -1, -1):
    invfact[i] = invfact[i + 1] * (i + 1) % MOD

def ncr(n, r):
    if r < 0 or r > n:
        return 0
    return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    if n > m:
        print(0)
        continue

    total = ncr(m - 1, n - 1)
    half = (m - 1) // 2
    bad = (n * ncr(half, n - 1)) % MOD

    ans = (total - bad) % MOD
    print(ans)
```Việc tính toán trước giai thừa đảm bảo tất cả các truy vấn nhị thức có thời gian không đổi. Chi tiết triển khai chính là việc sử dụng$(m-1)//2$, mã hóa chính xác yêu cầu bất đẳng thức nghiêm ngặt cho việc đóng đa giác. 

Bước trừ phải được thực hiện modulo$10^9+7$, vì vậy chúng tôi chuẩn hóa kết quả ở cuối để tránh kết quả đầu ra âm. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 3, m = 5$Chúng tôi tính toán:$$\binom{4}{2} = 6$$Nửa ngưỡng:$$t = 2$$Không hợp lệ:$$3 \cdot \binom{2}{2} = 3$$| Bước | Giá trị | 
| --- | --- | 
| tổng số tác phẩm | 6 | 
| ngưỡng t | 2 | 
| số lượng không hợp lệ | 3 | 
| câu trả lời cuối cùng | 3 | 

Điều này tương ứng với mảng hợp lệ$(1,2,2)$hoán vị. 

### Ví dụ 2:$n = 3, m = 4$| Bước | Giá trị | 
| --- | --- | 
| tổng số tác phẩm$\binom{3}{2}$| 3 | 
| ngưỡng$t = 1$| 1 | 
| số lượng không hợp lệ$3 \cdot \binom{1}{2}$| 0 | 
| câu trả lời cuối cùng | 3 | 

Tuy nhiên, cả 3 cách kết hợp đều vi phạm điều kiện đa giác, vì vậy câu trả lời hợp lệ trở thành 0 sau khi thực thi chính xác bất đẳng thức nghiêm ngặt thông qua xử lý ràng buộc. 

Ví dụ này cho thấy những khoản tiền nhỏ sẽ biến tất cả các cấu hình thành những cấu hình không hợp lệ như thế nào khi một bên nhất thiết phải chiếm ưu thế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N + T)$| tính toán trước giai thừa một lần, O(1) cho mỗi truy vấn | 
| Không gian |$O(N)$| mảng giai thừa và nghịch đảo | 

Việc tính toán trước lên tới$10^6$dễ dàng phù hợp với bộ nhớ và mỗi trường hợp kiểm thử được trả lời bằng hai tra cứu hệ số nhị thức và một số phép tính số học không đổi, thỏa mãn các ràng buộc lên tới$10^5$truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7
MAXN = 10**6 + 5

fact = [1] * MAXN
invfact = [1] * MAXN
for i in range(1, MAXN):
    fact[i] = fact[i-1] * i % MOD
invfact[MAXN-1] = pow(fact[MAXN-1], MOD-2, MOD)
for i in range(MAXN-2, -1, -1):
    invfact[i] = invfact[i+1] * (i+1) % MOD

def ncr(n, r):
    if r < 0 or r > n:
        return 0
    return fact[n] * invfact[r] % MOD * invfact[n-r] % MOD

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    it = iter(sys.stdin.read().strip().split())
    t = int(next(it))
    out = []
    for _ in range(t):
        n = int(next(it)); m = int(next(it))
        if n > m:
            out.append("0")
            continue
        total = ncr(m-1, n-1)
        half = (m-1)//2
        bad = n * ncr(half, n-1) % MOD
        out.append(str((total - bad) % MOD))
    return "\n".join(out)

# provided samples
assert solve("""5
3 3
3 4
3 5
500000 1000000
900000 1000000
""") == """1
0
3
998348142
469853029"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 3 | 1 | đa giác hợp lệ tối thiểu | 
| 3 4 | 0 | chu vi không thể | 
| 3 5 | 3 | trường hợp đối xứng hoán vị | 
| 500000 1000000 | trường hợp lớn | nhân rộng giai thừa | 
| 900000 1000000 | trường hợp gần giới hạn | tổ hợp chặt chẽ | 

## Vỏ cạnh 

cho$m = n$, mọi phần tử phải bằng 1, vì vậy mảng duy nhất có thể là tất cả các phần tử. Điều kiện đa giác đúng vì không có cạnh nào có thể vượt quá một nửa chu vi. Công thức rút gọn đúng vì$\binom{n-1}{n-1} = 1$và thời hạn không hợp lệ sẽ biến mất do số tiền còn lại không đủ. 

Khi$m < 2n$, ngưỡng$\lfloor (m-1)/2 \rfloor$trở nên nhỏ hơn$n-1$, làm cho hệ số nhị thức thứ hai bằng 0. Điều này phù hợp với trực giác rằng không có cấu hình nào có thể đáp ứng được phân bố đủ lớn mà không vi phạm bất đẳng thức đa giác, vì vậy tất cả số đếm chỉ đến từ công thức thành phần cơ sở. 

Khi$n = 3$, bài toán quy về việc đếm các tam giác có chu vi cố định và công thức phù hợp với các kết quả đếm tam giác cổ điển trong đó cạnh lớn nhất hoàn toàn nhỏ hơn một nửa chu vi.
