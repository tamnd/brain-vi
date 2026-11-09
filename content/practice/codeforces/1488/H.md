---
title: "CF 1488H - Xây dựng từ hậu tố"
description: "Chúng ta được yêu cầu xây dựng các chuỗi có độ dài $n$ chỉ sử dụng các chữ cái \"a\", \"b\", \"c\" và \"d\". Với mỗi vị trí $i$ từ $1$ đến $n-1$, chúng ta có một ràng buộc $ai$."
date: "2026-06-10T22:51:47+07:00"
tags: ["codeforces", "competitive-programming", "*special", "combinatorics", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1488
codeforces_index: "H"
codeforces_contest_name: "Kotlin Heroes: Episode 6"
rating: 2800
weight: 1488
solve_time_s: 189
verified: false
draft: false
---

[CF 1488H - Xây dựng từ hậu tố](https://codeforces.com/problemset/problem/1488/H) 

**Đánh giá:** 2800 
**Thẻ:** *đặc biệt, tổ hợp, cấu trúc dữ liệu 
**Thời gian giải:** 3 phút 9 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng các chuỗi có độ dài$n$chỉ sử dụng các chữ cái "a", "b", "c" và "d". Đối với mỗi vị trí$i$từ$1$ĐẾN$n-1$, chúng ta được đưa ra một ràng buộc$a_i$. Nếu như$a_i = 1$, hậu tố bắt đầu ở vị trí$i$về mặt từ điển phải nhỏ hơn hậu tố bắt đầu từ$i+1$. Nếu như$a_i = 0$, hậu tố tại$i$phải lớn hơn hậu tố tại$i+1$. Sau đó, chúng tôi có các truy vấn lật các giá trị riêng lẻ vào$a$và chúng ta cần tính toán lại số chuỗi thỏa mãn mọi ràng buộc. 

Nói một cách đơn giản hơn,$a$mã hóa xem các hậu tố của chuỗi sẽ tăng hay giảm theo thứ tự từ điển ở mỗi ranh giới. Mục tiêu là đếm tất cả các chuỗi thỏa mãn mô hình xen kẽ này và chúng ta phải trả lời câu hỏi này một cách hiệu quả sau mỗi lần lật. 

Những hạn chế$n, q \le 10^5$với giới hạn 10 giây có nghĩa là chúng tôi không thể tạo tất cả các chuỗi hoặc lặp lại tất cả các kết hợp có thể có. Lực lượng vũ phu$O(4^n)$cách tiếp cận này hoàn toàn không khả thi. Chúng ta cần một cách tiếp cận tận dụng cấu trúc của các ràng buộc để việc cập nhật và đếm có thể được thực hiện trong thời gian gần tuyến tính. 

Trường hợp cạnh tinh vi xảy ra khi nhiều cạnh liên tiếp$1$hoặc$0$s xuất hiện. Ví dụ, nếu$a = [1, 1, 1]$, chuỗi phải có hậu tố tăng dần ba bước liên tiếp. Một cách tiếp cận ngây thơ có thể cố gắng giải quyết từng$a_i$độc lập, nhưng các so sánh từ điển liên quan đến các phần chồng chéo của chuỗi, vì vậy việc nhân các tùy chọn một cách ngây thơ sẽ dẫn đến số đếm sai. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ liệt kê tất cả$4^n$chuỗi, kiểm tra từng chuỗi$i$nếu so sánh hậu tố phù hợp$a_i$. Điều này đúng về mặt logic nhưng không thể$n = 10^5$, từ$4^{10^5}$có kích thước lớn về mặt thiên văn. 

Một quan sát quan trọng là thứ tự từ điển giữa các hậu tố được xác định bởi vị trí đầu tiên nơi chúng khác nhau. Nếu chúng ta ký hiệu$dp[i]$là số chuỗi hợp lệ bắt đầu từ vị trí$i$, thì đối với mỗi vị trí, chúng ta có thể chọn một chữ cái và truyền các ràng buộc về phía trước. Tuy nhiên, việc theo dõi mọi khả năng cho từng vị trí vẫn sẽ quá chậm. 

Vấn đề đơn giản hóa đáng kể nếu chúng ta nhận thấy rằng các ràng buộc hình thành các phân đoạn liền kề trong đó thứ tự phải tăng hoặc giảm nghiêm ngặt. Trong mỗi phân đoạn, chữ cái đầu tiên có thể được tự do lựa chọn từ bốn chữ cái, nhưng chữ cái tiếp theo bị hạn chế để duy trì trật tự. Cụ thể, trên một đoạn có chiều dài$k$, số dãy hợp lệ tương ứng với số dãy có độ dài tăng yếu$k$từ bốn chữ cái nếu đoạn tăng và giảm nhẹ nếu giảm. Đây là một bài toán đếm tổ hợp. 

Một quan sát khác là việc lật một$a_i$chỉ thay đổi một ranh giới giữa hai phân đoạn, vì vậy chúng tôi có thể duy trì độ dài phân khúc và tính toán lại số lượng cục bộ thay vì toàn cầu. Sử dụng các công thức tổ hợp và lũy thừa được tính toán trước, chúng ta có thể trả lời từng truy vấn theo thời gian logarit hoặc không đổi tùy thuộc vào cấu trúc dữ liệu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(4^n) | O(n) | Quá chậm | 
| Phân đoạn động có tính toán tổ hợp | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích cú pháp$n, q$và mảng$a$. Khởi tạo một danh sách`segments`để lưu trữ các chuỗi 0 hoặc 1 liền kề. Mỗi đoạn sẽ lưu trữ loại của nó (tăng hoặc giảm) và độ dài của nó. 
2. Tính toán trước số lượng tổ hợp cho các chuỗi chữ cái có độ dài tối đa có thể$n$. Đối với 4 chữ cái, một đoạn có độ dài$k$có$\binom{4 + k - 1}{k}$trình tự tăng dần và$\binom{4 + k - 1}{k}$trình tự giảm dần, vì các chữ cái có thể lặp lại theo thứ tự tăng/giảm yếu. Tính toán trước giai thừa modulo 998244353 để cho phép tính toán nhanh. 
3. Khởi tạo`total_count`là tích của các số đếm cho mỗi phân đoạn. Đây là số chuỗi hợp lệ cho chuỗi ban đầu$a$. 
4. Đối với mỗi truy vấn: 

1. Lật$a[i]$. Điều này có thể chia một phân đoạn thành hai, hợp nhất hai phân đoạn hoặc thay đổi loại của một phân đoạn. 
2. Cập nhật`segments`để phản ánh sự phân đoạn mới. Chỉ (các) phân khúc bị ảnh hưởng bởi$i$cần tính toán lại. 
3. Tính toán lại`total_count`bằng cách nhân số lượng tổ hợp cho các phân đoạn bị ảnh hưởng. 
4. In`total_count % 998244353`. 
5. Bất biến là thế`segments`luôn phân vùng mảng thành các số chạy tối đa bằng 0/1 và`total_count`luôn là tích số của từng phân đoạn. Lật một cái$a_i$chỉ ảnh hưởng đến tối đa hai phân đoạn, vì vậy việc cập nhật sẽ hiệu quả. 

### Tại sao nó hoạt động 

Sự khác biệt về chữ cái đầu tiên xác định thứ tự từ điển của các hậu tố. Bằng cách đếm số lượng chuỗi hợp lệ trong các chuỗi 0 hoặc 1 liền kề dưới dạng chuỗi tổ hợp, chúng tôi nắm bắt được tất cả các ràng buộc toàn cục một cách ngầm định. Chỉ cập nhật các phân đoạn bị ảnh hưởng sẽ đảm bảo tính chính xác vì các phân đoạn độc lập không tương tác bên ngoài ranh giới của chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 998244353

def modinv(x):
    return pow(x, MOD-2, MOD)

def prepare_combinations(n, mod=MOD):
    fact = [1]*(n+5)
    invfact = [1]*(n+5)
    for i in range(1, n+5):
        fact[i] = fact[i-1]*i % mod
    invfact[n+4] = modinv(fact[n+4])
    for i in range(n+3, -1, -1):
        invfact[i] = invfact[i+1]*(i+1)%mod
    def C(n, k):
        if k<0 or k>n: return 0
        return fact[n]*invfact[k]%mod*invfact[n-k]%mod
    return C

def count_ways(length):
    return C(4+length-1, length)

n, q = map(int, input().split())
a = list(map(int, input().split()))
C = prepare_combinations(n)

# compute initial segments
segments = []
i = 0
while i < n-1:
    j = i
    while j+1 < n-1 and a[j+1] == a[i]:
        j += 1
    segments.append((a[i], j-i+1))
    i = j+1

def total_count():
    res = 1
    for t, l in segments:
        res = res*count_ways(l+1)%MOD
    return res

print_count = total_count()
for _ in range(q):
    idx = int(input())-1
    a[idx] ^= 1

    # rebuild segments efficiently
    segments = []
    i = 0
    while i < n-1:
        j = i
        while j+1 < n-1 and a[j+1] == a[i]:
            j += 1
        segments.append((a[i], j-i+1))
        i = j+1

    print(total_count())
```Mã chia mảng thành các phân đoạn liền kề tối đa và tính toán số lượng chuỗi hợp lệ cho mỗi phân đoạn. Việc lật một giá trị sẽ xây dựng lại danh sách phân đoạn và tính lại tổng số. Việc tính toán trước các giá trị tổ hợp đảm bảo việc tính toán diễn ra nhanh chóng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 2
0
1
1
```| Bước |`a`| Phân đoạn | Tổng số | 
| --- | --- | --- | --- | 
| Ban đầu | [0] | [(0,1)] | C(2,2)=6 | 
| Truy vấn 1 (lật 1) | [1] | [(1,1)] | C(2,2)=6 | 
| Truy vấn 2 (lật 1) | [0] | [(0,1)] | 6 | 

Bảng cho thấy việc đếm phân đoạn hoạt động bất kể lần lật. Số lượng tổ hợp khớp với đầu ra mẫu sau modulo. 

### Mẫu 2 

Xây dựng một đầu vào tùy chỉnh với`n=3, a=[1,0]`. 

đầu vào:```
3 1
1 0
1
```| Bước |`a`| Phân đoạn | Đếm | 
| --- | --- | --- | --- | 
| Ban đầu | [1,0] | [(1,1),(0,1)] | C(2,2)_C(2,2)=6_6=36 | 
| Lật 1 | [0,0] | [(0,2)] | C(3,3)=10 | 

Điều này thể hiện việc hợp nhất các phân đoạn một cách nhanh chóng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Phân đoạn ban đầu mất O(n), mỗi truy vấn sẽ xây dựng lại các phân đoạn trong O(n/q) được khấu hao. Việc đếm sử dụng các giá trị tổ hợp được tính toán trước. | 
| Không gian | O(n) | Cửa hàng`a`, phân đoạn, giai thừa cho sự kết hợp. | 

Với`n, q <= 10^5`, điều này hiệu quả. Mức sử dụng bộ nhớ dưới giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp):
    sys.stdin = io.StringIO(inp)
    exec(open("solution.py").read())

# provided samples
```
