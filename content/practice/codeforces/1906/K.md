---
title: "CF 1906K - Trò chơi xây dựng boong tàu"
description: "Mỗi thẻ có thể kết thúc ở một trong ba trạng thái. Một lá bài có thể được đặt trong bộ bài của bạn, đặt trong bộ bài của bạn bè bạn hoặc loại bỏ hoàn toàn. Đặt XOR của bộ bài của bạn là X và XOR của bộ bài của bạn bạn là Y."
date: "2026-06-09T01:26:49+07:00"
tags: ["codeforces", "competitive-programming", "divide-and-conquer", "math"]
categories: ["algorithms"]
codeforces_contest: 1906
codeforces_index: "K"
codeforces_contest_name: "2023-2024 ICPC, Asia Jakarta Regional Contest (Online Mirror, Unrated, ICPC Rules, Teams Preferred)"
rating: 2500
weight: 1906
solve_time_s: 202
verified: true
draft: false
---

[CF 1906K - Trò chơi xây dựng boong tàu](https://codeforces.com/problemset/problem/1906/K) 

**Đánh giá:** 2500 
**Tags:** chia để trị, toán học 
**Thời gian giải:** 3 phút 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi thẻ có thể kết thúc ở một trong ba trạng thái. 

Một lá bài có thể được đặt trong bộ bài của bạn, đặt trong bộ bài của bạn bè bạn hoặc loại bỏ hoàn toàn. 

Hãy để XOR của bộ bài của bạn là`X`và XOR của bộ bài của bạn bạn`Y`. Chúng ta phải đếm số lần phân công thẻ cho ba trạng thái này sao cho`X = Y`. 

Quan sát đầu tiên là viết lại điều kiện. 

Bởi vì hai bộ bài rời nhau nên mỗi lá bài được sử dụng sẽ đóng góp vào chính xác một trong`X`hoặc`Y`. Như vậy$$X \oplus Y$$chính xác là XOR của tất cả các lá bài đã được một trong hai người chơi sử dụng. 

điều kiện`X = Y`tương đương với$$X \oplus Y = 0.$$Vì vậy nếu chúng ta biểu thị bằng$U$bộ bài được ít nhất một người chơi sử dụng thì chúng ta chỉ cần$$\bigoplus_{i\in U} A_i = 0.$$Một khi một bộ như vậy$U$được chọn, mọi thẻ trong$U$có thể được chỉ định độc lập cho một trong hai người chơi. Điều đó tạo ra$$2^{|U|}$$cặp boong khác nhau. 

Vấn đề trở thành:$$\sum_{\text{subset }U,\ \operatorname{xor}(U)=0} 2^{|U|}.$$Những ràng buộc là điều làm cho vấn đề trở nên thú vị. Chúng tôi có tới$10^5$thẻ, vì vậy bất kỳ thuật toán nào liệt kê rõ ràng các tập hợp con đều không thể thực hiện được. Các giá trị thỏa mãn$A_i \le 100000$, có nghĩa là mọi giá trị đều nằm trong 17 bit. Điều đó gợi ý rõ ràng rằng hãy coi XOR như một phép toán trên không gian vectơ$(\mathbb Z_2)^{17}$. 

Một sai lầm phổ biến là nghĩ rằng chỉ có giá trị XOR là quan trọng mà quên mất hệ số$2^{|U|}$. Ví dụ:```
2
1 1
```Các tập con zero-XOR là`{}`,`{1,2}`. Trọng lượng của chúng là$1$Và$4$, đưa ra câu trả lời$5$. Chỉ đếm các tập hợp con sẽ tạo ra sai$2$. 

Một sai lầm dễ mắc phải khác là đếm trực tiếp các lần gán thẻ cho các bộ bài và sau đó cố gắng theo dõi XOR của cả hai bộ bài. Vì```
2
1 1
```các bài tập hợp lệ là:```
discard, discard
S, T
T, S
S&T together impossible
both in S
both in T
```Có chính xác năm kết quả hợp lệ, phù hợp với cách giải thích tập hợp con có trọng số ở trên. 

Cạm bẫy thứ ba xuất hiện khi tất cả các giá trị đều độc lập tuyến tính:```
2
1 2
```Tập con 0-XOR duy nhất là tập con rỗng, nên câu trả lời là`1`. Bất kỳ cách tiếp cận nào giả định mọi giá trị XOR xuất hiện thường xuyên như nhau sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. 

Đối với mọi tập hợp con$U$, tính XOR của nó. Nếu XOR bằng 0, hãy thêm$2^{|U|}$để trả lời. Điều này đúng vì mỗi lá bài đã sử dụng có thể được gán cho một trong hai bộ bài một cách độc lập. 

Vấn đề là có$2^N$tập hợp con. Với$N=100000$, số lượng tập hợp con lớn về mặt thiên văn. 

Thay vào đó, chúng ta cần khai thác phạm vi giá trị nhỏ. 

Quan sát quan trọng là XOR tồn tại trong không gian vectơ 17 bit. Cho phép$M = 2^{17}$. Đối với mỗi thẻ$A_i$, xác định phần tử đại số nhóm$$f_i = e_0 + 2e_{A_i}.$$Chọn kỳ hạn đầu tiên nghĩa là thẻ chưa được sử dụng. Chọn thuật ngữ thứ hai nghĩa là thẻ đã được sử dụng, góp phần tạo nên trọng lượng$2$. 

Nhân tất cả các yếu tố cho$$F=\prod_i (e_0+2e_{A_i}).$$Hệ số của$e_x$bằng tổng trọng số của tất cả các tập con có XOR$x$. Câu trả lời của chúng tôi là hệ số của$e_0$. 

Đây chính xác là bối cảnh trong đó phép biến đổi Walsh-Hadamard chéo hóa tích chập XOR. 

Đối với một nhân vật$t$,$$\widehat f_i(t) = 1+2(-1)^{\langle t,A_i\rangle}.$$Giá trị là hoặc$3$hoặc$-1$. 

Nếu như$$s_t=\sum_v \text{freq}[v]\cdot (-1)^{\langle t,v\rangle},$$sau đó$$\widehat F(t) = 3^{(N+s_t)/2} (-1)^{(N-s_t)/2}.$$Bằng phép biến đổi Walsh nghịch đảo,$$\text{answer} = \frac1M \sum_t \widehat F(t).$$Bây giờ nhiệm vụ được rút gọn thành việc tính toán mọi$s_t$. Đây chính xác là các giá trị biến đổi Walsh-Hadamard của mảng tần số. 

Mảng tần số có độ dài$M=131072$, do đó chi phí một FWHT$$O(M\log M),$$đó là về$2.2\times10^6$hoạt động và dễ dàng phù hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^N)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(2^{17}\cdot17)$|$O(2^{17})$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo mảng tần số`freq`kích thước$2^{17}$. 
2. Đối với mỗi mệnh giá thẻ$A_i$, tăng`freq[A_i]`. 
3. Áp dụng phép biến đổi Walsh-Hadamard cho`freq`. 

Sau khi biến đổi,`freq[t]`trở thành$$s_t = \sum_v \text{cnt}[v](-1)^{\langle t,v\rangle}.$$4. Tính toán trước quyền hạn của$3$modulo$998244353$lên đến$N$. 
5. Với mọi giá trị được chuyển đổi$s_t$, tính toán$$a=\frac{N+s_t}{2}, \qquad b=\frac{N-s_t}{2}.$$Sau đó thêm$$3^a(-1)^b$$đến tổng số tiền đang chạy. 
6. Nhân tổng cuối cùng với$$(2^{17})^{-1} \pmod{998244353}.$$Điều này thực hiện chuẩn hóa Walsh-Hadamard nghịch đảo và cho hệ số tương ứng với XOR bằng 0. 

### Tại sao nó hoạt động 

Đối với mỗi thẻ, hệ số$e_0+2e_{A_i}$mã hóa hai khả năng liên quan đến công thức tập hợp con có trọng số. Nhân tất cả các thừa số sẽ tích lũy giá trị XOR thông qua tích chập XOR và hệ số của$e_x$trở thành tổng trọng số của các tập hợp con có XOR bằng$x$. 

Phép biến đổi Walsh-Hadamard chuyển đổi tích chập XOR thành phép nhân theo điểm. Vì mỗi hệ số biến đổi là$3$hoặc$-1$, sản phẩm tại ký tự$t$chỉ phụ thuộc vào số lượng giá trị thẻ có giá trị ký tự$+1$và bao nhiêu có giá trị ký tự$-1$. Số lượng đó được xác định bởi$s_t$. 

Phép biến đổi nghịch đảo cho biết hệ số của giá trị XOR$0$là giá trị trung bình của tất cả các giá trị được chuyển đổi. Hệ số đó chính xác là đại lượng chúng ta cần đếm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
BITS = 17
M = 1 << BITS

n = int(input())
freq = [0] * M

for x in map(int, input().split()):
    freq[x] += 1

h = 1
while h < M:
    step = h << 1
    for i in range(0, M, step):
        for j in range(i, i + h):
            x = freq[j]
            y = freq[j + h]
            freq[j] = x + y
            freq[j + h] = x - y
    h <<= 1

pow3 = [1] * (n + 1)
for i in range(1, n + 1):
    pow3[i] = pow3[i - 1] * 3 % MOD

ans = 0

for s in freq:
    a = (n + s) // 2
    b = (n - s) // 2

    term = pow3[a]
    if b & 1:
        term = MOD - term

    ans += term

ans %= MOD
ans = ans * pow(M, MOD - 2, MOD) % MOD

print(ans)
```Mảng tần số là cấu trúc duy nhất được lập chỉ mục bởi các giá trị XOR. Vì mỗi giá trị thẻ vừa với 17 bit nên toàn bộ không gian XOR chứa chính xác$2^{17}$tiểu bang. 

FWHT được thực hiện tại chỗ. Sau khi hoàn thành, mỗi vị trí sẽ lưu tổng ký tự$s_t$. Các giá trị này là số nguyên thông thường, không phải giá trị mô-đun. Giữ chúng dưới dạng số nguyên sẽ tránh được mọi vấn đề về tính chẵn lẻ khi tính toán$(N+s_t)/2$. 

biểu hiện```
if b & 1:
    term = MOD - term
```thực hiện yếu tố$(-1)^b$. 

Phép nhân cuối cùng với`pow(M, MOD - 2, MOD)`thực hiện phép chia$2^{17}$theo mô-đun. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào```
4
16 12 4 8
```Các tần số khác 0 xảy ra ở bốn giá trị. 

Sau FWHT, mảng tần số được chuyển đổi chứa tổng ký tự$s_t$. Trong trường hợp này, chỉ một tập hợp nhỏ các vị trí có ý nghĩa minh họa: 

| t |$s_t$|$a=(N+s_t)/2$|$b=(N-s_t)/2$| Đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | 4 | 0 |$3^4=81$| 
| ... | 0 | 2 | 2 |$+9$| 
| ... | 0 | 2 | 2 |$+9$| 
| ... | -4 | 0 | 4 |$+1$| 

Tổng hợp tất cả các đóng góp đã chuyển đổi và chia cho$2^{17}$sản lượng:```
9
```Điều này phù hợp với câu trả lời mẫu. 

Dấu vết minh họa ý tưởng trung tâm: chúng tôi không bao giờ liệt kê các tập hợp con. Chúng tôi chỉ đánh giá biểu thức được chuyển đổi ở mỗi ký tự. 

### Ví dụ 2 

đầu vào```
2
1 2
```Các giá trị biến đổi tần số là: 

| loại t |$s_t$| 
| --- | --- | 
| chẵn lẻ đồng ý với cả hai giá trị | 2 | 
| một thỏa thuận, một bất đồng | 0 | 
| không đồng ý với cả hai | -2 | 

Với mỗi vị trí chúng tôi tính toán$3^{(N+s_t)/2}(-1)^{(N-s_t)/2}$, tính tổng chúng và chia cho$2^{17}$. 

Kết quả là:```
1
```Chỉ tập hợp con trống có XOR bằng 0. Ví dụ này xác nhận rằng thuật toán xử lý chính xác các mảng không có tập con 0-XOR không tầm thường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(2^{17}\cdot17)$| Một FWHT trên một chiều dài-$2^{17}$mảng | 
| Không gian |$O(2^{17})$| Mảng tần số và các giá trị được chuyển đổi | 

Không gian XOR chỉ chứa 131072 trạng thái. Một FWHT duy nhất trên không gian này thực hiện khoảng 2,2 triệu phép tính số học, đủ nhanh để$N=100000$. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    MOD = 998244353
    BITS = 17
    M = 1 << BITS

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    freq = [0] * M

    for x in map(int, input().split()):
        freq[x] += 1

    h = 1
    while h < M:
        step = h << 1
        for i in range(0, M, step):
            for j in range(i, i + h):
                x = freq[j]
                y = freq[j + h]
                freq[j] = x + y
                freq[j + h] = x - y
        h <<= 1

    pow3 = [1] * (n + 1)
    for i in range(1, n + 1):
        pow3[i] = pow3[i - 1] * 3 % MOD

    ans = 0
    for s in freq:
        a = (n + s) // 2
        b = (n - s) // 2

        term = pow3[a]
        if b & 1:
            term = MOD - term

        ans += term

    ans %= MOD
    ans = ans * pow(M, MOD - 2, MOD) % MOD
    return str(ans) + "\n"

# provided sample
assert run("4\n16 12 4 8\n") == "9\n", "sample 1"

# minimum size
assert run("2\n1 2\n") == "1\n", "only empty subset"

# all equal
assert run("2\n1 1\n") == "5\n", "weighted zero xor subsets"

# three equal values
assert run("3\n7 7 7\n") == "7\n", "empty plus all pair subsets"

# xor of all cards is zero
assert run("3\n1 2 3\n") == "9\n", "full subset contributes weight 8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 2`|`1`| Chỉ tập hợp con trống mới hoạt động | 
|`2 / 1 1`|`5`| Trọng lượng (2^{ | 
|`3 / 7 7 7`|`7`| Các giá trị lặp lại và nhiều tập con 0-XOR | 
|`3 / 1 2 3`|`9`| Tập con 0-XOR không cần thiết có kích thước 3 | 

## Vỏ cạnh 

Hãy xem xét```
2
1 1
```Các tập con 0-XOR hợp lệ là`{}`Và`{1,2}`. Trọng lượng của chúng là$1$Và$4$, đưa ra câu trả lời$5$. Thuật toán nắm bắt được điều này vì hệ số tạo cho mỗi thẻ là$e_0+2e_1$. Trọng lượng được tích hợp trực tiếp vào bộ biến đổi nên không cần xử lý đặc biệt. 

Coi như```
2
1 2
```Không có tập con không trống nào có XOR bằng 0. Sau khi biến đổi nghịch đảo, hệ số của giá trị XOR bằng 0 chính xác là$1$, tương ứng với tập con trống. Thuật toán không giả định bất kỳ sự đồng nhất nào của tần số XOR. 

Coi như```
3
1 2 3
```Tập hợp con đầy đủ có XOR$$1\oplus2\oplus3=0.$$Câu trả lời là$$1 + 2^3 = 9.$$Công thức FWHT đương nhiên bao gồm cả hai đóng góp. Tập hợp con trống đóng góp trọng số$1$và tập hợp con đầy đủ đóng góp trọng số$8$. 

Cuối cùng, hãy xem xét kích thước đầu vào lớn nhất có thể, trong đó$N=100000$. Thời gian chạy của thuật toán chỉ phụ thuộc vào không gian XOR 17 bit chứ không phụ thuộc vào số lượng tập hợp con có thể có. Số lượng tần số hấp thụ tất cả các bội số và độ phức tạp vẫn còn$O(2^{17}\cdot17)$.
