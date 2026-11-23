---
title: "CF 451E - Devu và Hoa"
description: "Chúng tôi có n hộp hoa. Từ hộp i, chúng ta có thể lấy bất kỳ số lượng hoa nào từ 0 đến f[i]. Màu sắc khác biệt nên việc lựa chọn được mô tả hoàn toàn bằng số lượng hoa chúng tôi lấy từ mỗi hộp."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "combinatorics", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 451
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 258 (Div. 2)"
rating: 2300
weight: 451
solve_time_s: 97
verified: true
draft: false
---

[CF 451E - Devu và Hoa](https://codeforces.com/problemset/problem/451/E) 

**Đánh giá:** 2300 
**Tags:** bitmasks, tổ hợp, lý thuyết số 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`n`hộp hoa. Từ hộp`i`, chúng ta có thể lấy bất kỳ số lượng hoa nào giữa`0`Và`f[i]`, bao gồm. Màu sắc khác biệt nên việc lựa chọn được mô tả hoàn toàn bằng số lượng hoa chúng tôi lấy từ mỗi hộp. 

Nhiệm vụ là đếm số vectơ nguyên$$x_1+x_2+\cdots+x_n=s$$như vậy$$0 \le x_i \le f_i$$cho mỗi hộp. Câu trả lời phải được báo cáo modulo$10^9+7$. 

Điều đầu tiên nổi bật là kích thước của các con số. Số lượng hộp nhiều nhất là$20$, rất nhỏ. Mặt khác, cả hai$s$và dung lượng hộp có thể rất lớn, đạt tới$10^{14}$Và$10^{12}$tương ứng. 

Những ràng buộc này ngay lập tức loại trừ việc lập trình động trên tổng. Bất kỳ giải pháp nào có độ phức tạp phụ thuộc vào$s$là vô vọng. Thậm chí$O(ns)$sẽ yêu cầu xử lý lên tới$10^{14}$tiểu bang. 

Giá trị nhỏ của$n$là manh mối thực sự. Bất cứ khi nào$n \le 20$, các kỹ thuật liên quan đến tập hợp con thường trở nên khả thi vì$2^{20}\approx 10^6$, có thể quản lý được. 

Có một số trường hợp khó bỏ sót. 

Coi như```
1 0
0
```Sự lựa chọn hợp lệ duy nhất là không lấy hoa. Câu trả lời là`1`. Một công thức vô tình coi số 0 là trường hợp không hợp lệ đặc biệt sẽ thất bại ở đây. 

Coi như```
1 5
3
```Không có lựa chọn hợp lệ nào tồn tại vì hộp chỉ chứa ba bông hoa. Câu trả lời là`0`. Bất kỳ công thức tính không hạn chế nào cũng sẽ tính sai một nghiệm. 

Coi như```
2 1
0 0
```Một lần nữa câu trả lời là`0`. Giới hạn trên loại bỏ hoàn toàn mọi giải pháp ứng cử viên. 

Một trường hợp tinh vi khác xuất hiện trong quá trình bao gồm-loại trừ. Một số đóng góp của tập hợp con tương ứng với việc đếm các nghiệm của$$x_1+\cdots+x_n = T$$với$T<0$. Những điều khoản như vậy phải đóng góp bằng không. Việc quên kiểm tra này sẽ khiến các hệ số nhị thức không hợp lệ được đưa vào phép tính. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ sẽ thử mọi lựa chọn hoa có thể có từ mỗi hộp. 

Cho mỗi hộp$i$, có$f_i+1$khả năng. Tổng không gian tìm kiếm là$$\prod_{i=1}^{n}(f_i+1).$$Từ$f_i$có thể đạt được$10^{12}$, cái này lớn về mặt thiên văn. Ngay cả với các giá trị nhỏ như$f_i=100$, không gian tìm kiếm trở thành$101^{20}$, điều đó đã là không thể rồi. 

Phiên bản tự nhiên không hạn chế của vấn đề dễ dàng hơn nhiều. Nếu chúng ta bỏ qua giới hạn trên và chỉ yêu cầu$$x_1+\cdots+x_n=s,\qquad x_i\ge 0,$$thì công thức sao và vạch cổ điển sẽ cho$$\binom{s+n-1}{n-1}.$$Trở ngại duy nhất là giới hạn trên$x_i\le f_i$. 

Bất cứ khi nào chúng ta có các giới hạn dưới đơn giản cùng với các giới hạn trên, thì việc loại trừ bao gồm sẽ trở thành một ứng cử viên nặng ký. Số lượng không hạn chế là dễ dàng. Các giải pháp vi phạm một hoặc nhiều giới hạn trên có thể được tính một cách có hệ thống. 

Giả sử một hộp cụ thể$i$vi phạm giới hạn của nó. Sau đó$$x_i\ge f_i+1.$$Cho phép$$y_i=x_i-(f_i+1).$$Sau sự thay đổi này,$y_i\ge 0$và tổng số tiền giảm đi$f_i+1$. 

Nếu một tập hợp con$S$của các hộp buộc phải vi phạm giới hạn của nó, sau đó sau khi dịch chuyển mọi biến trong$S$,$$\sum x_i=s$$trở thành$$\sum y_i=s-\sum_{i\in S}(f_i+1).$$Số nghiệm không âm lại được tính theo số sao và vạch. 

Từ$n\le 20$, chúng ta có thể liệt kê tất cả các tập hợp con$S$. chỉ có$2^n$của họ. 

Thách thức còn lại là tính toán các hệ số nhị thức trong đó giá trị cao nhất có thể lớn bằng$10^{14}$. May mắn thay, giá trị đáy luôn lớn nhất$$n-1\le 19.$$Giá trị nhỏ này cho phép tính toán nhân trực tiếp theo modulo$10^9+7$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O\!\left(\prod (f_i+1)\right)$|$O(n)$| Quá chậm | 
| Bao gồm-Loại trừ tối ưu |$O(n2^n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`s`, và năng lực`f[i]`. 
2. Liệt kê mọi tập hợp con của các hộp bằng cách sử dụng mặt nạ bit từ`0`ĐẾN`(1<<n)-1`. 
3. Đối với tập hợp con hiện tại`mask`, tính toán$$T=s-\sum_{i\in S}(f_i+1),$$Ở đâu$S$là tập hợp các bit được chọn. 

Điều này tương ứng với việc buộc mọi hộp trong$S$vượt quá giới hạn của nó. 

1. Nếu$T<0$, không có nghiệm không âm sau phép dịch, vì vậy tập con này đóng góp bằng không. 
2. Ngược lại tính các giải pháp không hạn chế của$$y_1+\cdots+y_n=T.$$Ngôi sao và quán bar mang lại$$\binom{T+n-1}{n-1}.$$1. Nếu kích thước tập hợp con là số chẵn, hãy thêm đại lượng này vào câu trả lời. 
2. Nếu kích thước tập hợp con là số lẻ, hãy trừ số lượng này khỏi câu trả lời. 

Đây chính xác là nguyên tắc bao gồm-loại trừ. 
3. Sau khi xử lý tất cả các tập con, chuẩn hóa câu trả lời theo modulo$10^9+7$. 

### Tại sao nó hoạt động 

Đối với mọi tập hợp con$S$, bao gồm-loại trừ tính các giải pháp trong đó tất cả các biến trong$S$vi phạm giới hạn trên của họ. Thay thế từng biến vi phạm bằng$$y_i=x_i-(f_i+1)$$tạo ra sự song ánh giữa các nghiệm đó và nghiệm không âm của một phương trình tổng rút gọn. Các ngôi sao và thanh đếm chính xác các giải pháp này. 

Loại trừ bao gồm đảm bảo rằng mọi giải pháp hợp lệ được tính một lần, trong khi mọi giải pháp không hợp lệ sẽ bị hủy bỏ. Vì mọi tập hợp con đều được xử lý nên tổng cuối cùng bằng số nghiệm thỏa mãn tất cả các giới hạn trên cùng một lúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

def modinv(x):
    return pow(x, MOD - 2, MOD)

def comb_large_n_small_k(n, k):
    if n < 0 or k < 0 or k > n:
        return 0

    res = 1
    for i in range(1, k + 1):
        res = res * ((n - k + i) % MOD) % MOD
        res = res * modinv(i) % MOD
    return res

def solve():
    n, s = map(int, input().split())
    f = list(map(int, input().split()))

    ans = 0

    for mask in range(1 << n):
        t = s
        bits = 0

        for i in range(n):
            if mask & (1 << i):
                t -= f[i] + 1
                bits += 1

        if t < 0:
            continue

        ways = comb_large_n_small_k(t + n - 1, n - 1)

        if bits & 1:
            ans -= ways
        else:
            ans += ways

    print(ans % MOD)

solve()
```Thói quen trung tâm là`comb_large_n_small_k`. Đối số trên có thể lớn bằng$10^{14}$, nhưng đối số thấp hơn nhiều nhất là$19$. Chúng tôi tính toán$$\binom{N}{K} = \prod_{i=1}^{K} \frac{N-K+i}{i} \pmod{MOD}$$trực tiếp. 

Bởi vì$K<MOD$, mọi mẫu số đều có một nghịch đảo mô đun. 

Vòng lặp tập hợp con thực hiện chính xác việc loại trừ bao gồm. Đối với mỗi hộp được chọn, chúng tôi trừ đi`f[i] + 1`, tương ứng với việc buộc hộp đó vượt quá giới hạn của nó. Nếu tổng kết quả trở thành số âm thì không có nghiệm nào và tập hợp con không đóng góp gì. 

Tính chẵn lẻ của kích thước tập hợp con xác định xem phần đóng góp được cộng hay trừ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 3
1 3
```Chúng tôi cần$$x_1+x_2=3, \quad 0\le x_1\le1, \quad 0\le x_2\le3.$$| Mặt nạ | Tập hợp con |$T$| Đóng góp | Ký tên | 
| --- | --- | --- | --- | --- | 
| 00 | {} | 3 |$\binom{4}{1}=4$| + | 
| 01 | {1} | 1 |$\binom{2}{1}=2$| - | 
| 10 | {2} | -1 | 0 | - | 
| 11 | {1,2} | -3 | 0 | + | 

Giá trị cuối cùng:$$4-2=2.$$Đầu ra:```
2
```Dấu vết này cho thấy việc loại trừ bao gồm loại bỏ hai giải pháp không hạn chế trong đó hộp đầu tiên vượt quá dung lượng của nó. 

### Mẫu 2 

đầu vào:```
2 4
2 2
```| Mặt nạ | Tập hợp con |$T$| Đóng góp | Ký tên | 
| --- | --- | --- | --- | --- | 
| 00 | {} | 4 |$\binom{5}{1}=5$| + | 
| 01 | {1} | 1 |$\binom{2}{1}=2$| - | 
| 10 | {2} | 1 |$\binom{2}{1}=2$| - | 
| 11 | {1,2} | -2 | 0 | + | 

Giá trị cuối cùng:$$5-2-2=1.$$Đầu ra:```
1
```Giải pháp duy nhất còn sót lại là$(2,2)$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n2^n)$| Mỗi tập hợp con kiểm tra tối đa$n$hộp | 
| Không gian |$O(1)$| Chỉ có một vài biến ngoài mảng đầu vào | 

Từ$n\le 20$, thuật toán xử lý nhiều nhất$$20\cdot 2^{20}\approx 2\times10^7$$thao tác đơn giản, phù hợp thoải mái trong giới hạn. Những giá trị to lớn của$s$Và$f_i$không bao giờ xuất hiện trong vòng lặp. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

MOD = 1000000007

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def modinv(x):
        return pow(x, MOD - 2, MOD)

    def comb_large_n_small_k(n, k):
        if n < 0 or k < 0 or k > n:
            return 0

        res = 1
        for i in range(1, k + 1):
            res = res * ((n - k + i) % MOD) % MOD
            res = res * modinv(i) % MOD
        return res

    n, s = map(int, input().split())
    f = list(map(int, input().split())

    ans = 0

    for mask in range(1 << n):
        t = s
        bits = 0

        for i in range(n):
            if mask & (1 << i):
                t -= f[i] + 1
                bits += 1

        if t >= 0:
            ways = comb_large_n_small_k(t + n - 1, n - 1)
            if bits & 1:
                ans -= ways
            else:
                ans += ways

    return str(ans % MOD)

# provided sample
assert run("2 3\n1 3\n") == "2", "sample 1"

# custom cases
assert run("1 0\n0\n") == "1", "minimum input"
assert run("1 5\n3\n") == "0", "sum exceeds capacity"
assert run("2 4\n2 2\n") == "1", "unique solution"
assert run("3 0\n0 0 0\n") == "1", "all zeros"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 / 0`|`1`| Lựa chọn trống được tính | 
|`1 5 / 3`|`0`| Tổng mục tiêu không thể | 
|`2 4 / 2 2`|`1`| Giải pháp ranh giới chính xác | 
|`3 0 / 0 0 0`|`1`| Nhiều hộp không dung lượng | 

## Vỏ cạnh 

Hãy xem xét```
1 0
0
```Tập hợp con trống cho$$\binom{0}{0}=1.$$Tập hợp con vi phạm tạo ra tổng giảm âm và đóng góp bằng 0. Câu trả lời cuối cùng là`1`, đại diện cho sự lựa chọn độc đáo là không lấy hoa. 

Coi như```
1 5
3
```Tập hợp con trống đóng góp$$\binom{5}{0}=1.$$Tập hợp con vi phạm góp phần$$\binom{1}{0}=1.$$Sản lượng bao gồm-loại trừ$$1-1=0.$$Mọi giải pháp không hạn chế đều vi phạm giới hạn trên nên việc hủy bỏ hoàn tất. 

Coi như```
2 1
0 0
```Tập hợp con trống đóng góp$$\binom{2}{1}=2.$$Mỗi tập hợp con vi phạm hộp đơn đóng góp$1$và tập hợp con hộp đôi đóng góp$0$. Kết quả là$$2-1-1=0.$$Không có lựa chọn hợp lệ nào tồn tại vì không có hộp nào chứa bất kỳ bông hoa nào. 

Trường hợp tổng âm được xử lý tự động. Bất cứ khi nào$$s-\sum_{i\in S}(f_i+1)<0,$$không thể có nghiệm không âm sau phép dịch chuyển biến, vì vậy đóng góp bằng không. Điều này ngăn cản việc đánh giá nhị thức không hợp lệ và giữ cho việc loại trừ bao gồm chính xác về mặt toán học.
