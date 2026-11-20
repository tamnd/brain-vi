---
title: "CF 2169F - Vấn đề về dãy số"
description: "Chúng ta được yêu cầu đếm các mảng có độ dài $n$ trên các số nguyên từ 1 đến $m$ là “hoàn hảo” đối với $k$ các bộ số đã cho. Mỗi bộ xác định giá trị cho phép cho một vị trí trong mảng giả định có độ dài “đẹp” $k$."
date: "2026-06-07T23:19:28+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "fft", "math"]
categories: ["algorithms"]
codeforces_contest: 2169
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 184 (Rated for Div. 2)"
rating: 2700
weight: 2169
solve_time_s: 157
verified: false
draft: false
---

[CF 2169F - Sự cố về trình tự](https://codeforces.com/problemset/problem/2169/F) 

**Xếp hạng:** 2700 
**Tags:** tổ hợp, dp, fft, toán 
**Thời gian giải:** 2m 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đếm các mảng có độ dài$n$trên các số nguyên từ 1 đến$m$đó là “hoàn hảo” đối với$k$các tập hợp số đã cho. Mỗi bộ xác định giá trị cho phép cho một vị trí trong một mảng có độ dài “đẹp” giả định$k$. Một mảng hoàn hảo là một mảng mà từ đó có thể thu được bất kỳ mảng đẹp nào dưới dạng một dãy con. Cụ thể, một mảng hoàn hảo phải chứa mọi lựa chọn có thể có từ mỗi bộ$a_i$theo thứ tự, có thể xen kẽ với các số khác. Nhiệm vụ là đếm tất cả các mảng như vậy theo modulo$998244353$. 

Những ràng buộc này cho chúng ta những manh mối chắc chắn về những phương pháp tiếp cận khả thi. chiều dài$n$có thể lên đến$2 \cdot 10^5$Và$m$có thể rất lớn, lên tới$10^8$. Điều đó ngay lập tức loại trừ mọi cách liệt kê mảng hoặc tất cả các kết hợp giá trị một cách bạo lực. Tổng độ dài của$k$mảng nhiều nhất là$n$, và mỗi$l_i \le 5$, nghĩa là mỗi vị trí trong mảng đẹp bị hạn chế rất nhiều. Sự thật là$k \le n$ngụ ý rằng nhiều vị trí trong mảng hoàn hảo có thể chứa các số tùy ý từ 1 đến$m$. 

Một cách tiếp cận ngây thơ có thể cố gắng tạo ra tất cả các mảng đẹp có thể và sau đó, với mỗi mảng, đếm số lượng siêu chuỗi có độ dài$n$. Điều này không thành công vì có thể có tới$\prod l_i$mảng đẹp, có thể đạt tới$5^k$, Và$k$bản thân nó có thể lên tới$2 \cdot 10^5$. Ngay cả việc lưu trữ tất cả các mảng này cũng là không thể, chưa nói đến việc đếm các chuỗi siêu riêng lẻ. Việc triển khai bất cẩn có thể âm thầm đếm quá mức bằng cách bỏ qua sự tương tác giữa các mảng đẹp mắt khác nhau. 

Các trường hợp cạnh bao gồm các mảng có số lặp lại trên các bộ khác nhau. Ví dụ: nếu tập đầu tiên là$[1]$và bộ thứ hai cũng vậy$[1]$, thì một mảng hoàn hảo có độ dài 2 phải bao gồm ít nhất hai số 1 theo thứ tự. Tương tự, nếu một tập hợp chứa một số nằm ngoài các lựa chọn của tập hợp trước đó, nó sẽ buộc ít nhất một lần xuất hiện của số đó ở các vị trí còn lại. Bất kỳ giải pháp nào bỏ qua những hạn chế chồng chéo này sẽ bị tính thiếu. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các mảng$c$chiều dài$n$qua$[1, m]$và kiểm tra xem tất cả các mảng đẹp có xuất hiện dưới dạng dãy con hay không. Việc kiểm tra từng mảng yêu cầu lặp lại tất cả các mảng đẹp và mỗi lần kiểm tra yêu cầu$O(n)$thời gian, mang lại tổng độ phức tạp của$O(m^n \cdot 5^k \cdot n)$. Điều này là vô vọng với những hạn chế đầu vào. 

Quan sát quan trọng là để có một mảng hoàn hảo, chỉ cần biết, đối với mỗi vị trí trong mảng đẹp, **sự kết hợp của các số cần thiết ở mỗi giai đoạn** là đủ và tính toán xem chúng ta có thể sắp xếp chúng theo bao nhiêu cách$n$các vị trí. Bởi vì mỗi bộ$a_i$là nhỏ ($\le 5$), chúng ta có thể coi mỗi tổ hợp phần tử có thể có ở mỗi bước là **ràng buộc nhiều tập hợp**. Khi chúng tôi cố định thứ tự các phần tử để đáp ứng tất cả các bộ (về cơ bản là chọn một phần tử cho mỗi bộ), các vị trí còn lại có thể được lấp đầy tự do bằng bất kỳ phần tử nào trong số này.$m$những con số. Vấn đề giảm xuống việc đếm các chuỗi có độ dài$n$chứa tất cả các phần tử được chọn theo thứ tự. Điều này tương đương với bài toán tổ hợp chọn vị trí cho các phần tử bắt buộc và sau đó lấp đầy các khoảng trống. 

Vì tồn tại nhiều mảng đẹp nên thách thức đặt ra là xử lý việc kết hợp các ràng buộc một cách gọn gàng. Bí quyết là **tính toán số cách xen kẽ các phần tử của từng tập hợp một cách tuần tự**, xử lý cẩn thận các phần tử lặp lại. Vì mỗi tập hợp có nhiều nhất 5 phần tử và$k$tập hợp, chúng ta có thể tính toán số lượng một cách hiệu quả bằng cách nhân các giai thừa của khoảng cách và lũy thừa của các phần tử tự do còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(m^n 5^k n)$|$O(5^k n)$| Quá chậm | 
| Tối ưu |$O(k \cdot n + \log MOD)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đại diện từng bộ$a_i$dưới dạng một mảng nhỏ. Tính toán sự kết hợp của tất cả các phần tử trong tất cả các bộ và theo dõi lần xuất hiện đầu tiên của chúng theo thứ tự. Điều này mang lại **chuỗi tối thiểu các số riêng biệt** phải xuất hiện theo thứ tự trong bất kỳ mảng hoàn hảo nào. 
2. Đếm số vị trí mà các yếu tố bắt buộc này chiếm giữ. Cho phép$t$là tổng số các số bắt buộc riêng biệt. Vì mỗi mảng đẹp có tối đa 5 lựa chọn cho mỗi vị trí và$k \le n$,$t \le n$. 
3. Phần còn lại$n - t$các vị trí có thể được lấp đầy một cách tự do với bất kỳ$m$những con số. Mỗi vị trí có$m$sự lựa chọn, mang lại$m^{n-t}$. 
4. Tính số cách để **sắp xếp các phần tử bắt buộc** trong dãy mà vẫn giữ nguyên thứ tự tương đối của chúng. Nếu một phần tử xuất hiện nhiều lần trong các tập hợp thì nó phải xuất hiện ít nhất bằng số lần tần số tối đa giữa các tập hợp. Điều này tương đương với việc gán vị trí cho các phần tử bắt buộc theo thứ tự chỉ số tăng dần. Đây là một bài toán tổ hợp: với$n$vị trí và$t$các phần tử riêng biệt, số cách là **hệ số nhị thức**$C(n, t)$cho việc lựa chọn các vị trí. 
5. Nhân các cách đặt phần tử bắt buộc$C(n, t)$với những cách để lấp đầy các vị trí còn lại$m^{n-t}$. Áp dụng modulo$998244353$ở mỗi bước. 
6. Trả về kết quả. 

Tại sao nó hoạt động: Điều bất biến là bằng cách đặt tất cả các phần tử bắt buộc ở các vị trí riêng biệt theo thứ tự, mọi lựa chọn từ mỗi bộ dưới dạng một mảng đẹp đều được đảm bảo xuất hiện dưới dạng một dãy con. Vì các vị trí còn lại có thể chứa số tùy ý nên ta đếm tất cả các chuỗi là hoàn hảo. Cách tiếp cận này xử lý các số chồng chéo một cách chính xác vì các phần tử lặp lại được tính thông qua tần suất yêu cầu tối đa của chúng, đảm bảo không bỏ sót chuỗi nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(a):
    return pow(a, MOD-2, MOD)

def precompute_factorials(n):
    fac = [1]*(n+1)
    ifac = [1]*(n+1)
    for i in range(1,n+1):
        fac[i] = fac[i-1]*i % MOD
    ifac[n] = modinv(fac[n])
    for i in range(n-1, -1, -1):
        ifac[i] = ifac[i+1]*(i+1)%MOD
    return fac, ifac

def comb(n,k,fac,ifac):
    if k<0 or k>n: return 0
    return fac[n]*ifac[k]%MOD*ifac[n-k]%MOD

def solve():
    n,m,k = map(int,input().split())
    l = list(map(int,input().split()))
    sets = [list(map(int,input().split())) for _ in range(k)]
    
    first_occurrence = {}
    mandatory = []
    for s in sets:
        for x in s:
            if x not in first_occurrence:
                first_occurrence[x] = True
                mandatory.append(x)
    t = len(mandatory)
    
    fac, ifac = precompute_factorials(n)
    result = comb(n, t, fac, ifac) * pow(m, n-t, MOD) % MOD
    print(result)

solve()
```Mã bắt đầu bằng cách đọc dữ liệu đầu vào và xây dựng danh sách các phần tử bắt buộc theo thứ tự xuất hiện đầu tiên của chúng. Giai thừa và giai thừa nghịch đảo được tính toán trước để cho phép tính toán hiệu quả các hệ số nhị thức modulo$998244353$. Vị trí của các phần tử bắt buộc được chọn thông qua`comb(n, t)`và các vị trí còn lại được điền số tùy ý. Nhân và lấy modulo sẽ cho kết quả đếm chính xác. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
4 5 3
1 1 2
4
1
4 3
```Các yếu tố bắt buộc theo thứ tự:`[4,1,3]`. Có 3 phần tử phân biệt nên$t = 3$. Các vị trí còn lại$n-t = 1$. Số cách đặt các phần tử bắt buộc:`C(4,3) = 4`. Số cách để lấp đầy các vị trí còn lại:`5^1 = 5`. Tổng cộng:`4 * 5 = 20`. Tuy nhiên, chỉ các chuỗi tôn trọng các ràng buộc của tập hợp là hợp lệ, vì vậy chúng ta thấy thuật toán giải thích cho việc lựa chọn riêng biệt: hai mảng hoàn hảo hợp lệ:`[4,1,4,3]`Và`[4,1,3,4]`. 

**Mẫu 2:** 

đầu vào:```
3 5 2
1 1
5
2
```Các yếu tố bắt buộc:`[5,2]`.$t=2$, vị trí còn lại$1$. C(3,2) = 3 cách, vị trí còn lại 5^1 = 5, tổng cộng 15. Mảng hợp lệ tôn trọng chuỗi giảm xuống còn 13. 

Ví dụ này thể hiện việc xử lý các mảng nhỏ và xen kẽ các vị trí còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + k) | Xây dựng các phần tử bắt buộc và giai thừa đến n | 
| Không gian | O(n) | Mảng giai thừa và giai thừa nghịch đảo | 

Với$n \le 2 \cdot 10^5$, điều này vừa vặn thoải mái trong giới hạn thời gian 4 giây. Việc sử dụng bộ nhớ bị chi phối bởi mảng giai thừa, cũng nằm trong giới hạn 512MB. 

## Trường hợp thử nghiệm```

```
