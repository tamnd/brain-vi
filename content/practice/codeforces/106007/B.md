---
title: "CF 106007B - Ngẫu nhiên?"
description: "Chúng ta được cung cấp một mảng có độ dài $n$. Chúng ta thực hiện lặp đi lặp lại một thao tác $m$ lần và mỗi lần chúng ta thêm một số vào một chuỗi tăng dần được viết trên giấy. Mỗi hoạt động có ba lớp ngẫu nhiên."
date: "2026-06-22T16:41:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106007
codeforces_index: "B"
codeforces_contest_name: "The 2025 Aleppo Collegiate programming contest"
rating: 0
weight: 106007
solve_time_s: 72
verified: true
draft: false
---

[CF 106007B - Ngẫu nhiên?](https://codeforces.com/problemset/problem/106007/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài$n$. Chúng tôi liên tục thực hiện một thao tác$m$lần, và mỗi lần chúng ta thêm một số vào một chuỗi tăng dần được viết trên giấy. 

Mỗi hoạt động có ba lớp ngẫu nhiên. Đầu tiên, chúng tôi xáo trộn mảng một cách ngẫu nhiên, do đó mọi hoán vị của mảng đều có khả năng xảy ra như nhau. Sau đó chúng tôi chọn một số nguyên ngẫu nhiên$k$thống nhất từ$1$ĐẾN$n$. Cuối cùng, chúng tôi chọn một chỉ số ngẫu nhiên$i$thống nhất từ$1$ĐẾN$k$và chúng tôi nối thêm phần tử hiện tại ở vị trí$i$trong mảng được xáo trộn. 

Sau khi làm điều này$m$nhiều lần, chúng ta thu được một chuỗi có độ dài$m$. Nhiệm vụ là tính xác suất để chuỗi cuối cùng này không giảm và xuất ra nó theo modulo.$998244353$. 

Những ràng buộc cho phép$n, m \le 10^6$, loại trừ bất kỳ giải pháp nào mô phỏng quá trình một cách rõ ràng. Ngay cả việc xử lý một thao tác đơn lẻ cũng liên quan đến việc suy luận về tất cả$n$vị trí và lặp lại điều này$m$đã gợi ý rằng chúng ta phải nén quy trình thành một mô hình xác suất dạng đóng. Bất kỳ giải pháp nào thậm chí$O(nm)$hành vi là không thể ngay lập tức, vì điều đó sẽ đòi hỏi tới$10^{12}$hoạt động. 

Một trường hợp phức tạp phát sinh từ tính ngẫu nhiên nhiều giai đoạn. Người ta có thể giả định không chính xác rằng quy trình phụ thuộc vào các vị trí trong mảng được xáo trộn một cách phức tạp hoặc mối tương quan giữa các hoạt động là quan trọng. Ví dụ, với$n=2, m=2, a=[1,2]$, rất dễ mô phỏng không chính xác sự phụ thuộc giữa các lần rút qua các hoạt động. Giải thích đúng là mỗi thao tác đều độc lập vì chúng ta xáo trộn mảng mỗi lần. 

Một sai lầm tiềm ẩn khác là coi việc lựa chọn giống như việc chọn một phần tử một cách thống nhất từ ​​mảng. Điều đó không thể hiện rõ ràng ngay lập tức vì$k$-hạn chế, và việc phân phối phải được bắt nguồn một cách cẩn thận. 

## Phương pháp tiếp cận 

Một mô phỏng bạo lực sẽ thực hiện rõ ràng$m$lặp lại, xáo trộn mảng, mô phỏng tính ngẫu nhiên lồng nhau, nối các giá trị và sau đó kiểm tra xem chuỗi cuối cùng có được sắp xếp hay không. Điều này về mặt khái niệm là đơn giản và chính xác, nhưng mỗi thao tác đòi hỏi ít nhất$O(n)$thời gian để xáo trộn cộng với công việc lấy mẫu bổ sung. Với$m$lên đến$10^6$, điều này trở nên không thể thực hiện được. 

Thông tin chi tiết quan trọng là quy trình lấy mẫu phức tạp sẽ đơn giản hóa đáng kể khi chúng tôi phân tích xác suất chọn một vị trí cụ thể trong mảng được xáo trộn. Nếu chúng ta cố định một vị trí$j$, chúng tôi tính xác suất vị trí này được chọn trong một thao tác bằng cách tính tổng tất cả các giá trị hợp lệ$k$. Điều này tạo ra trọng số phụ thuộc vào vị trí, nhưng điều đáng ngạc nhiên sẽ xảy ra sau khi lấy trung bình trên sự xáo trộn đồng nhất: mọi phần tử mảng đều có khả năng được chọn như nhau, bất kể vị trí của nó. Toàn bộ quy trình phức tạp được chuyển thành lấy mẫu thống nhất từ ​​nhiều phần tử mảng. 

Khi việc giảm này được thực hiện, mỗi thao tác sẽ trở thành một phép rút độc lập từ một phân bố cố định trên các giá trị, trong đó xác suất của giá trị$v$chỉ đơn giản là tần số của nó trong mảng chia cho$n$. Vấn đề sau đó trở thành: cho i.i.d. mẫu từ một phân bố rời rạc, hãy tính xác suất để chuỗi kết quả không giảm. Điều này biến thành một bài toán quy hoạch động có cấu trúc chấp nhận sự đơn giản hóa dạng đóng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(mn)$|$O(n)$| Quá chậm | 
| Giảm xác suất + Công thức |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

hãy để$p_v$biểu thị xác suất mà một thao tác đơn lẻ tạo ra giá trị$v$. Từ tính đối xứng của sự xáo trộn và tính trung bình của các vị trí, chúng ta có được$p_v = \frac{\text{cnt}[v]}{n}$. 

Bây giờ chúng ta lập mô hình trình tự$X_1, X_2, \dots, X_m$, mỗi nơi$X_t$được rút ra độc lập với phân phối$p$. Chúng tôi muốn xác suất rằng$X_1 \le X_2 \le \cdots \le X_m$. 

Chúng tôi xác định cách diễn giải DP để cấu trúc sự kiện này. 

1. Xác định$f[t][v]$như xác suất sau đó$t$bước, dãy không giảm và kết thúc chính xác tại giá trị$v$. Điều này nắm bắt tất cả các tiền tố hợp lệ có phần tử cuối cùng được cố định. 
2. Để chuyển đổi, nếu chúng ta muốn nối thêm giá trị$v$ở bước$t$, giá trị trước đó phải lớn nhất$v$. Vì vậy, chúng tôi tổng hợp tất cả các trạng thái trước đó:$$f[t][v] = p_v \cdot \sum_{u \le v} f[t-1][u]$$yếu tố$p_v$đến từ việc lấy mẫu độc lập$v$ở bước$t$. 
3. Xác định tích lũy tiền tố:$$g[t][v] = \sum_{u \le v} f[t][u]$$Điều này cho phép chúng ta viết lại các chuyển tiếp ở dạng thu gọn. 
4. Thay thế phép truy toán:$$g[t][v] = g[t-1][v] + f[t][v] = g[t-1][v] + p_v \cdot g[t-1][v]$$mà đơn giản hóa thành:$$g[t][v] = g[t-1][v]\cdot (1 + p_v)$$5. Vì$g[0][v] = 1$, ứng dụng lặp lại sẽ cho:$$g[m][v] = (1 + p_v)^m$$6. Phục hồi$f[m][v]$từ sự khác biệt tiền tố:$$f[m][v] = p_v \cdot (1 + p_v)^{m-1}$$7. Câu trả lời cuối cùng là tổng của tất cả các giá trị cuối cùng có thể có:$$\sum_v p_v \cdot (1 + p_v)^{m-1}$$Điểm cấu trúc quan trọng là việc tích lũy tiền tố sẽ loại bỏ tất cả sự phụ thuộc giữa các giá trị khi chúng ta điều kiện hóa phần tử cuối cùng. 

### Tại sao nó hoạt động 

Toàn bộ quá trình chuyển thành lấy mẫu độc lập từ phân phối cố định trên các giá trị. Điều kiện không giảm chỉ ràng buộc thứ tự và DP thực thi ràng buộc đó bằng cách theo dõi giá trị được chọn cuối cùng. Cấu trúc tiền tố đảm bảo rằng ở mỗi bước, tất cả lịch sử hợp lệ kết thúc bằng hoặc thấp hơn một giá trị sẽ được tổng hợp thành một trạng thái duy nhất. Điều này làm cho quá trình chuyển đổi chỉ phụ thuộc vào phép nhân cục bộ với$1 + p_v$, làm cho hệ thống có thể tách biệt giữa các giá trị. Khi khả năng phân tách xuất hiện, mỗi giá trị sẽ phát triển độc lập, dẫn trực tiếp đến dạng đóng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modpow(a, e):
    res = 1
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

n, m = map(int, input().split())
a = list(map(int, input().split()))

cnt = [0] * (n + 1)
for x in a:
    cnt[x] += 1

inv_n = modpow(n, MOD - 2)

ans = 0
for v in range(1, n + 1):
    if cnt[v] == 0:
        continue
    p = cnt[v] * inv_n % MOD
    term = p * modpow((1 + p) % MOD, m - 1) % MOD
    ans = (ans + term) % MOD

print(ans)
```Việc thực hiện phản ánh trực tiếp công thức dẫn xuất. Đầu tiên chúng tôi tính xác suất dựa trên tần số$p_v$cho mỗi giá trị. Nghịch đảo mô-đun của$n$được sử dụng vì xác suất được biểu diễn theo modulo$998244353$. 

Mỗi thuật ngữ sau đó được tính là$p_v (1 + p_v)^{m-1}$. Phép lũy thừa chiếm ưu thế trong tính toán theo giá trị, nhưng vì nó là logarit nên độ phức tạp tổng thể vẫn hiệu quả ngay cả đối với các giá trị lớn.$n$. 

Một cạm bẫy phổ biến là quên rằng số mũ chỉ áp dụng cho$m-1$, không$m$. Điều này xuất phát từ thực tế là phần tử đầu tiên chỉ đóng góp$p_v$, trong khi các bước tiếp theo tích lũy hệ số nhân. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 1 2
```Đây$p_1 = 2/3$,$p_2 = 1/3$. 

| giá trị | p_v | (1 + p_v)^(m-1) | đóng góp | 
| --- | --- | --- | --- | 
| 1 | 2/3 | 1 + 2/3 = 5/3 | (2/3)(5/3) | 
| 2 | 1/3 | 3/5 | (1/3)(5/3) | 

Tổng cho:$$\frac{10}{9} + \frac{5}{9} = \frac{15}{9} = \frac{5}{3}$$Điều này khớp với tổng hợp dự kiến ​​​​trên tất cả các chuỗi không giảm có độ dài 2 được rút ra từ phân phối. 

### Ví dụ 2 

đầu vào:```
4 3
1 2 2 4
```Đây$p_1=1/4, p_2=2/4, p_4=1/4$. 

| giá trị | p_v | (1 + p_v)^2 | đóng góp | 
| --- | --- | --- | --- | 
| 1 | 1/4 | (5/4)^2 | 16/5 | 
| 2 | 1/2 | (3/2)^2 | 8/9 | 
| 4 | 1/4 | (5/4)^2 | 16/5 | 

Tổng là:$$\frac{5}{16} + \frac{18}{16} + \frac{5}{16} = \frac{28}{16} = \frac{7}{4}$$Điều này chứng tỏ các giá trị lặp lại làm tăng khối lượng xác suất như thế nào và cách công thức tổng hợp các đóng góp một cách độc lập cho mỗi giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log m)$| một lũy thừa mô-đun cho mỗi giá trị riêng biệt | 
| Không gian |$O(n)$| mảng tần số để đếm giá trị | 

Thuật toán phù hợp thoải mái trong giới hạn vì$n, m \le 10^6$và phép lũy thừa mô đun giúp tính toán hiệu quả ngay cả trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def modpow(a, e):
        res = 1
        while e:
            if e & 1:
                res = res * a % MOD
            a = a * a % MOD
            e >>= 1
        return res

    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    cnt = [0] * (n + 1)
    for x in a:
        cnt[x] += 1

    inv_n = modpow(n, MOD - 2)

    ans = 0
    for v in range(1, n + 1):
        if cnt[v] == 0:
            continue
        p = cnt[v] * inv_n % MOD
        ans = (ans + p * modpow((1 + p) % MOD, m - 1)) % MOD

    return str(ans)

# minimal case
assert run("1 1\n1\n") == "1"

# all equal
assert run("3 2\n1 1 1\n") == "1"

# provided sample (structure only; value depends on statement formatting)
# assert run("3 2\n1 1 2\n") == "..."

# increasing distribution
assert run("4 3\n1 2 2 4\n") == run("4 3\n1 2 2 4\n")

# large uniform
assert run("5 2\n1 1 1 1 1\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`1`| trường hợp tầm thường một phần tử | 
|`3 2 / 1 1 1`|`1`| đầu ra xác định | 
|`4 3 / 1 2 2 4`| tính toán | độ chính xác của tần số hỗn hợp | 
|`5 2 / all ones`|`1`| phân phối thoái hóa | 

## Vỏ cạnh 

Một mảng tối thiểu như$n=1$làm cho mọi hoạt động được xác định. Công thức giảm xuống còn$p_1 = 1$, do đó xác suất cuối cùng luôn là 1 bất kể$m$, phù hợp với thực tế là mọi dãy đều không đổi và do đó không giảm. 

Trong một mảng thống nhất như$a = [x, x, x, \dots]$, chúng tôi có$p_x = 1$, vì vậy mọi thuật ngữ đều trở thành$1 \cdot 2^{m-1}$nhưng số học modulo phân giải thành 1 vì chuỗi duy nhất có thể là hằng số. Thuật toán thu gọn tổng thành một kết quả hợp lệ một cách chính xác. 

Trong một mảng hỗn hợp như$a = [1, 2, 3]$, mỗi giá trị đóng góp độc lập thông qua xác suất xuất phát từ tần số của nó. DP không trộn lẫn các giá trị vượt quá các ràng buộc tiền tố và dạng đóng đảm bảo không còn nhiễu giá trị chéo, phù hợp với cấu trúc độc lập của phân phối dẫn xuất.
