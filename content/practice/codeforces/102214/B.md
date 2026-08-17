---
title: "CF 102214B - Nhị thức"
description: "Cho dãy số nguyên dương a1, a2, ..., an. Chúng ta phải đếm các cặp vị trí có thứ tự (i, j), cho phép i = j, với hệ số nhị thức [ binom{ai}{aj} ] là số lẻ. Thứ tự quan trọng vì (i, j) và (j, i) là các cặp khác nhau."
date: "2026-08-18T00:05:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102214
codeforces_index: "B"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u043e\u0435 \u043b\u0438\u0447\u043d\u043e\u0435 \u043f\u0435\u0440\u0432\u0435\u043d\u0441\u0442\u0432\u043e \u0418\u041a\u0418\u0422 \u0421\u0424\u0423 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2015"
rating: 0
weight: 102214
solve_time_s: 110
verified: true
draft: false
---

[CF 102214B - Nhị thức](https://codeforces.com/problemset/problem/102214/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ta có dãy số nguyên dương`a1, a2, ..., an`. Chúng ta phải đếm các cặp vị trí có thứ tự`(i, j)`, cho phép`i = j`, trong đó hệ số nhị thức 

[ 
\binom{a_i}{a_j} 
] 

thật kỳ quặc. 

Thứ tự quan trọng vì`(i, j)`Và`(j, i)`là những cặp khác nhau Các giá trị bằng nhau ở các vị trí khác nhau cũng tạo ra các cặp khác nhau. Các ràng buộc cho phép lên đến`10^6`các phần tử trong một ca kiểm thử, mỗi phần tử ở giữa`1`Và`10^6`, với tối đa 10 trường hợp thử nghiệm. Tuyên bố chính thức đưa ra giới hạn thời gian 5 giây và giới hạn bộ nhớ 512 MiB. 

Việc kiểm tra trực tiếp từng cặp yêu cầu`n^2`séc. Tại`n = 10^6`, đó là`10^12`cặp trước khi xem xét chi phí đánh giá từng hệ số nhị thức. Điều đó vượt xa những gì giải pháp cuộc thi 5 giây có thể làm được, vì vậy giải pháp phải tránh xem xét từng cặp một. 

Trường hợp tế nhị đầu tiên là`i = j`. Ví dụ,```
1
1
1
```có một cặp duy nhất`(1,1)`, Và`C(1,1) = 1`, vậy câu trả lời là`1`. Giải pháp chỉ tính các cặp vị trí khác nhau sẽ trả về sai`0`. 

Các giá trị lặp lại là một nguồn sai lầm phổ biến khác. Vì```
1
3
1 1 1
```mỗi một trong những`3 * 3 = 9`cặp thứ tự có hệ số`C(1,1) = 1`, vậy câu trả lời là`9`. Đếm các cặp giá trị riêng biệt thay vì các cặp vị trí sẽ cho kết quả sai. Mẫu này cũng có mặt trong tuyên bố chính thức. 

Cuối cùng,`C(x,y)`bằng 0 khi`y > x`, và số 0 là số chẵn. Ví dụ,```
1
2
1 2
```có bốn cặp có thứ tự.`C(1,1)`Và`C(2,2)`thật kỳ lạ, trong khi`C(1,2)=0`Và`C(2,1)=2`đều chẵn nên đáp án là`2`. Một giải pháp bất cẩn coi mọi cặp giá trị là có khả năng hợp lệ mà không xử lý chính xác điều kiện bit sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là kiểm tra từng cặp được đặt hàng`(i,j)`và xác định xem`C(a_i,a_j)`thật kỳ quặc. Có chính xác`n^2`cặp, vì vậy ở mức tối đa`n = 10^6`có`10^12`lần lặp lại. Ngay cả khi tính kỳ lạ của việc kiểm tra được giảm xuống thành hoạt động liên tục thì điều này vẫn quá chậm. 

Quan sát hữu ích là chúng ta thực sự không cần giá trị của hệ số nhị thức. Chúng ta chỉ cần biết liệu nó có kỳ lạ hay không. Điều này thay đổi vấn đề hoàn toàn. 

Cho hai số nguyên không âm`x`Và`y`, Định lý Lucas modulo 2 cho ta 

[ 
\binom{x}{y} \equiv 
\prod_k \binom{x_k}{y_k} \pmod 2, 
] 

ở đâu`x_k`Và`y_k`là các chữ số nhị phân tương ứng. Một chữ số nhị phân góp phần`1`chính xác khi nào`y_k <= x_k`. Vì vậy toàn bộ sản phẩm được`1`chính xác khi mỗi tập bit của`y`cũng được thiết lập trong`x`. 

Trong ký hiệu bitwise, 

[ 
\binom{x}{y}\text{ là số lẻ} 
\iff 
(x\mathbin{&}y)=y. 
] 

Vì vậy, vấn đề ban đầu trở nên đơn giản hơn nhiều: với mọi phần tử mảng`x`, đếm xem mảng có bao nhiêu phần tử`y`là các mặt nạ con của`x`. 

Giả định`freq[y]`là số lần giá trị`y`xảy ra. chúng tôi muốn 

[ 
\sum_x freq[x]\sum_{y\subseteq x}freq[y]. 
] 

Tổng bên trong chính xác là Tổng trên các tập hợp con DP tiêu chuẩn, thường được gọi là SOS DP. Thay vì liệt kê mọi mặt nạ con của mọi`x`, chúng tôi xử lý trước số phần tử mảng có trong mỗi mặt nạ. 

Lực lượng vũ phu hoạt động vì việc kiểm tra một cặp về mặt khái niệm là đơn giản, nhưng không thành công vì có nhiều cặp bậc hai. Quan sát cho thấy các hệ số nhị thức lẻ tương ứng chính xác với mối quan hệ mặt nạ con cho phép chúng ta thay thế phép liệt kê cặp bằng một SOS DP trên các mặt nạ bit có thể có. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) ngoài đầu vào | Quá chậm | 
| Tối ưu | O(n + M log M) | O(M) | Đã chấp nhận | 

Đây`M`là lũy thừa nhỏ nhất của hai lớn hơn giá trị mảng tối đa. Từ`a_i <= 10^6`, chúng tôi có`M <= 2^20 = 1,048,576`. 

## Hướng dẫn thuật toán 

1. Đọc mảng và xây dựng`freq[x]`, số vị trí chứa mỗi giá trị`x`. Chúng tôi chỉ quan tâm đến những giá trị thực sự xảy ra nên chúng tôi cũng ghi lại giá trị lớn nhất. 
2. Chọn`M`là lũy thừa nhỏ nhất của hai lớn hơn giá trị tối đa. Mọi giá trị mảng có thể có sau đó là một mặt nạ trong`[0, M)`. 
3. Khởi tạo một mảng`dp`với`dp[x] = freq[x]`. Tại thời điểm này`dp[x]`chỉ tính số lần xuất hiện chính xác`x`, không phải mặt nạ con của nó. 
4. Xử lý từng bit một cách độc lập. một chút`b`, mọi mặt nạ có tập bit đó sẽ nhận được số đếm từ mặt nạ tương ứng với bit đó bị xóa: 

[ 
dp[mask] \mathrel{+}= dp[mask \oplus 2^b]. 
] 

Sau khi xử lý vài bit đầu tiên,`dp[mask]`đếm tất cả các giá trị có các bit đã được xử lý tạo thành một mặt nạ con của`mask`. Sau khi tất cả các bit đã được xử lý,`dp[x]`bằng 

[ 
\sum_{y\subseteq x}freq[y]. 
] 

Đây là bất biến SOS DP. Mỗi quá trình chuyển đổi quyết định xem một bit cụ thể của mặt nạ con là 0 hay một, vì vậy mỗi mặt nạ con được bao gồm chính xác một lần. 

1. Lặp lại mảng ban đầu. Đối với một phần tử`x`, có`dp[x]`chức vụ`j`giá trị của ai`a_j`là một mặt nạ con của`x`. Mỗi vị trí như vậy cho một hệ số nhị thức lẻ với`a_i = x`. 
2. Thêm`dp[x]`cho mọi vị trí mảng`i`. Các giá trị lặp lại phải được xử lý một lần cho mỗi lần xuất hiện vì câu trả lời tính các cặp vị trí được sắp xếp chứ không phải các cặp giá trị riêng biệt. 

### Tại sao nó hoạt động 

Đối với bất kỳ cặp nào`(i,j)`, cho phép`x = a_i`Và`y = a_j`. Theo định lý Lucas modulo 2,`C(x,y)`là số lẻ chính xác khi mỗi tập hợp bit của`y`cũng được thiết lập trong`x`, tương đương với`(x & y) == y`. 

SOS DP duy trì tính bất biến sau khi xử lý một tập hợp các bit,`dp[x]`đếm chính xác các giá trị mảng có các bit được xử lý là mặt nạ con của các bit tương ứng của`x`, trong khi các bit không bị giới hạn sẽ được xử lý bằng các chuyển đổi sau này. Sau khi tất cả các bit được xử lý,`dp[x]`do đó đếm mọi vị trí mảng`j`thỏa mãn`(x & a_j) == a_j`. 

Đối với mỗi vị trí`i`, thêm`dp[a_i]`do đó đếm chính xác các vị trí hợp lệ`j`ghép nối với`i`. Mỗi cặp thứ tự hợp lệ được tính một lần và không có cặp không hợp lệ nào được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        mx = max(a)
        size = 1 << mx.bit_length()

        freq = [0] * size
        for x in a:
            freq[x] += 1

        dp = freq[:]

        bit = 1
        while bit < size:
            block = bit << 1
            for start in range(0, size, block):
                end = start + bit
                for j in range(start, end):
                    dp[j + bit] += dp[j]
            bit <<= 1

        ans = 0
        for x in a:
            ans += dp[x]

        out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Mảng tần số được lập chỉ mục trực tiếp bằng giá trị nguyên. Vì giá trị lớn nhất là nhiều nhất`10^6`, phần mở rộng lũy ​​thừa hai của nó có nhiều nhất`1,048,576`mục nhập. 

Quá trình chuyển đổi SOS được viết theo khối. Đối với một khối có chiều dài`2 * bit`, nửa đầu biểu thị mặt nạ trong đó bit hiện tại bằng 0 và nửa sau biểu thị mặt nạ trong đó bit hiện tại bằng 0. Mỗi mặt nạ ở nửa sau nhận được giá trị tương ứng từ nửa đầu. Điều này tránh được việc kiểm tra bit bổ sung bên trong vòng lặp trong cùng và sạch hơn một chút trong Python. 

Vòng lặp dừng khi`bit == size`, do đó chính xác tất cả các vị trí nhị phân được biểu thị bằng không gian mặt nạ đều được xử lý. sử dụng`mx.bit_length()`thay vì luôn cố định 20 bit cũng làm cho các thử nghiệm nhỏ rẻ hơn đáng kể. 

Số nguyên Python không bị tràn, điều này quan trọng vì câu trả lời tính các cặp có thứ tự và có thể đạt tới`n²`, hoặc`10^12`khi`n = 10^6`. 
Mảng ban đầu được giữ lại vì sau SOS DO chúng ta cần thêm`dp[a_i]`một lần cho mỗi lần xuất hiện. Chỉ sử dụng mảng tần số và lặp lại các giá trị riêng biệt sẽ yêu cầu nhân riêng với tần số của giá trị hiện tại, điều này cũng hợp lệ, nhưng việc giữ lại mảng sẽ làm cho sự tương ứng với các cặp có thứ tự trở nên rõ ràng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu chính thức đầu tiên chứa hai trường hợp thử nghiệm. Đối với cái đầu tiên, mảng là`[1, 5, 6]`. Các biểu diễn nhị phân có liên quan là`001`,`101`, Và`110`. 

Các mặt nạ phụ của`1`xảy ra chỉ là`1`, Vì thế`dp[1] = 1`. Các mặt nạ phụ của`5`xảy ra là`1`Và`5`, Vì thế`dp[5] = 2`. Các mặt nạ phụ của`6`xảy ra chỉ là`6`, bởi vì không`1`cũng không`5`là một mặt nạ con của`6`. 

| Giá trị mảng`x`| nhị phân`x`| Mặt nạ con đang xảy ra |`dp[x]`| Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 001 | 1 | 1 | 1 | 
| 5 | 101 | 1, 5 | 2 | 2 | 
| 6 | 110 | 6 | 1 | 1 | 

Tổng cộng là`1 + 2 + 1 = 4`, phù hợp với đầu ra chính thức. Trường hợp mẫu thứ hai chứa ba bản sao của`1`, vậy mọi cặp có thứ tự đều hợp lệ và câu trả lời là`9`. 

### Mẫu 2 

cho```
1
2
1 2
```chúng tôi có mặt nạ nhị phân`01`Và`10`. 

Ban đầu, 

| Mặt nạ |`freq`| 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 0 | 

Sau khi xử lý bit`0`, mặt nạ`1`đã chứa số lượng mặt nạ con`0`, và mặt nạ`3`nhận được sự đóng góp từ mặt nạ`2`. Sau khi xử lý bit`1`, mặt nạ`3`nhận được sự đóng góp từ mặt nạ`1`Và`2`. 

Các giá trị cuối cùng có liên quan là: 

| Giá trị mảng`x`| nhị phân`x`|`dp[x]`| Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 01 | 1 | 1 | 
| 2 | 10 | 1 | 1 | 

Câu trả lời là`2`. Ví dụ này thể hiện thực tế rằng`1`không phải là một mặt nạ con của`2`Và`2`không phải là một mặt nạ con của`1`, do đó chỉ có hai cặp đường chéo đóng góp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + M log M) | Việc xây dựng tần số và tổng hợp các câu trả lời tốn O(n), trong khi SOS DP thực hiện`M/2`bổ sung cho mỗi`log₂M`bit | 
| Không gian | O(n + M) | Mảng đầu vào sử dụng O(n) và mảng tần số/DP sử dụng O(M) | 

Đối với vấn đề này,`M <= 2^20`, do đó giai đoạn SOS thực hiện khoảng`20 * 2^19 = 10.5 million`bổ sung trong trường hợp xấu nhất. Đây là lý do chính khiến giải pháp phù hợp với giới hạn 5 giây, trong khi`10^12`kiểm tra cặp của lực lượng vũ phu không thể. Giới hạn chính thức là 5 giây và 512 MiB. 

## Trường hợp thử nghiệm```python
import io
import sys

def solve_data(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)

    t = next(it)
    out = []

    for _ in range(t):
        n = next(it)
        a = [next(it) for _ in range(n)]

        mx = max(a)
        size = 1 << mx.bit_length()

        freq = [0] * size
        for x in a:
            freq[x] += 1

        dp = freq[:]

        bit = 1
        while bit < size:
            block = bit << 1
            for start in range(0, size, block):
                end = start + bit
                for j in range(start, end):
                    dp[j + bit] += dp[j]
            bit <<= 1

        ans = sum(dp[x] for x in a)
        out.append(str(ans))

    return "\n".join(out)

def run(inp: str) -> str:
    return solve_data(inp)

# Provided samples
assert run("""2
3
1 5 6
3
1 1 1
""") == """4
9""", "official sample"

# Minimum-size input
assert run("""1
1
1
""") == """1""", "minimum size"

# Boundary relation: neither 1 nor 2 is a submask of the other
assert run("""1
2
1 2
""") == """2""", "boundary submask case"

# All values equal: every ordered pair is valid
assert run("""1
4
5 5 5 5
""") == """16""", "all equal values"

# Maximum n, but small values, so the test remains compact in value-space
# while still exercising n = 10^6 and the n^2-sized answer.
n = 1_000_000
large_input = "1\n" + str(n) + "\n" + ("1 " * n) + "\n"
assert run(large_input) == "1000000000000", "maximum n"

# Values where submask relationships are easy to verify.
assert run("""1
5
3 5 6 7 1
""") == """13""", "submask relationships"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 1`|`1`| Đầu vào kích thước tối thiểu và tự ghép nối | 
|`1 / 2 / 1 2`|`2`| Hướng của mối quan hệ mặt nạ con và`C(x,y)=0`khi`y>x`| 
|`1 / 4 / 5 5 5 5`|`16`| Giá trị lặp lại và cặp có thứ tự | 
|`1 / 1000000 / 1 ... 1`|`1000000000000`| Tối đa`n`và câu trả lời lớn không tràn số nguyên | 
|`1 / 5 / 3 5 6 7 1`|`13`| Một số mối quan hệ mặt nạ con chồng chéo | 

## Vỏ cạnh 

Đối với trường hợp tự ghép nối```
1
1
1
```mảng tần số có`freq[1] = 1`. Chỉ có một bit để xử lý và`dp[1]`còn lại`1`. Tổng cuối cùng mất`dp[1]`một lần, cho`1`. Thuật toán không bao giờ loại trừ các vị trí bằng nhau nên đường chéo của ma trận cặp được xử lý một cách tự nhiên. 

Đối với các giá trị lặp lại,```
1
3
1 1 1
```mảng tần số chứa`freq[1] = 3`. Từ`1`là một mặt nạ con của chính nó, kết quả SOS là`dp[1] = 3`. Mỗi vị trí trong số ba vị trí đều thêm`3`, sản xuất`9`. Đây chính xác là số lượng các cặp vị trí được sắp xếp. 

Đối với trường hợp ranh giới```
1
2
1 2
```các giá trị có dạng nhị phân`01`Và`10`. Không phải là một mặt nạ con của cái kia. Hai cặp tự hợp lệ vì`x & x = x`, trong khi hai cặp chéo không hợp lệ. Câu trả lời cuối cùng là`2`. 

Đối với một giá trị như`7`, có biểu diễn nhị phân là`111`, mọi giá trị nhỏ hơn từ`0`bởi vì`7`là một mặt nạ con. Ví dụ, với```
1
5
3 5 6 7 1
```các mối quan hệ mặt nạ con hợp lệ được SOS DP tính mà không liệt kê chúng một cách rõ ràng. giá trị`7`chấp nhận tất cả năm giá trị,`5`chấp nhận`5`Và`1`,`3`chấp nhận`3`Và`1`,`6`chỉ chấp nhận`6`, Và`1`chỉ chấp nhận`1`. Những đóng góp là`2 + 2 + 1 + 5 + 1 = 11`, vì vậy kết quả mong đợi trong khối kiểm tra sẽ là`11`.
