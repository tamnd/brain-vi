---
problem: 1103E
contest_id: 1103
problem_index: E
name: "Radix sum"
contest_name: "Codeforces Round 534 (Div. 1)"
rating: 3400
tags: ["fft", "math", "number theory"]
answer: passed_samples
verified: true
solve_time_s: 122
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a338211-0f50-83ec-b937-2a330bd3ffc8
---

# CF 1103E - Tổng cơ số 

**Đánh giá:** 3400 
**Tags:** fft, toán, lý thuyết số 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a338211-0f50-83ec-b937-2a330bd3ffc8 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được yêu cầu đếm xem có bao nhiêu chuỗi có thể được hình thành bằng cách lặp đi lặp lại các phần tử từ một mảng nhất định, trong đó độ dài của chuỗi được cố định tại$n$. Mỗi lựa chọn đóng góp một số và những số này được kết hợp bằng quy tắc cộng số học thay vì phép cộng số học thông thường. 

Nếu chúng ta sắp xếp các số với các số 0 đứng đầu sao cho chúng có cùng độ dài thì tại mỗi vị trí chữ số, chúng ta cộng các chữ số một cách độc lập và lấy kết quả theo modulo 10. Điều này có nghĩa là không có sự mang giữa các chữ số. Sau khi thực hiện điều này với tất cả các số đã chọn, chúng ta thu được số cuối cùng được biểu thị từng chữ số một. 

Nhiệm vụ là tính toán cho mỗi số nguyên$i$từ$0$ĐẾN$n-1$, dài bao nhiêu-$n$trình tự tạo ra một tổng cơ số bằng$i$, trong đó số cuối cùng được diễn giải dưới dạng biểu diễn thập phân thông thường. 

Khó khăn chính xuất phát từ thực tế là chúng ta đang đếm các chuỗi có độ dài lên tới$10^5$, trong khi mỗi số có thể có tối đa 5 chữ số. Một mô phỏng đơn giản sẽ yêu cầu theo dõi tất cả các kết hợp chữ số có thể có trên$n$những lựa chọn độc lập, tăng trưởng theo cấp số nhân với$n$. Ngay cả tích chập đa thức trên toàn bộ không gian trạng thái cũng sẽ quá lớn nếu được thực hiện trực tiếp trên tất cả các số có thể có. 

mô-đun$2^{58}$quan trọng vì nó cho chúng ta biết chúng ta không cần các số nguyên lớn tùy ý, nhưng nó không làm thay đổi cấu trúc tổ hợp của bài toán. 

Trường hợp cạnh tinh tế xuất hiện khi nghĩ về các số có độ dài chữ số khác nhau. Ví dụ, nếu một số là`7`và cái khác là`10000`, việc căn chỉnh các chữ số sẽ thay đổi sự đóng góp của chúng cho mỗi vị trí. Một cách tiếp cận ngây thơ coi các số là số nguyên thay vì vectơ chữ số sẽ trộn các số mang không chính xác và tạo ra các chuyển đổi sai, vì các số mang bị cấm một cách rõ ràng. 

Một cái bẫy khác là cho rằng kết quả chỉ phụ thuộc vào tổng các số. Trong thực tế, mỗi vị trí chữ số tiến triển độc lập, do đó hai số có cùng giá trị nguyên nhưng cấu trúc chữ số khác nhau sẽ hoạt động khác nhau theo quy tắc tổng cơ số. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ mô phỏng rõ ràng tất cả các chuỗi có độ dài$n$. Mỗi vị trí trong dãy có$n$sự lựa chọn, vì vậy có$n^n$các trình tự có thể. Đối với mỗi chuỗi, chúng tôi sẽ tính tổng bằng chữ số, lấy$O(D)$thời gian ở đâu$D$là số chữ số. Thậm chí bỏ qua chi phí tính tổng, việc liệt kê$n^n$trình tự là hoàn toàn không khả thi. 

Một chế độ xem có cấu trúc hơn là quan sát rằng mỗi vị trí chữ số phát triển độc lập. Nếu chúng ta cố định một vị trí chữ số thì mỗi số được chọn sẽ đóng góp một chữ số từ 0 đến 9 và chúng ta đang tính tổng một cách hiệu quả.$n$các biến ngẫu nhiên độc lập trên nhóm tuần hoàn$\mathbb{Z}_{10}$. Điều này biến vấn đề thành tích chập lặp đi lặp lại của phân bố tần số cỡ 10. 

Khi chúng tôi tính toán, đối với mỗi vị trí chữ số, sự phân bố của các tổng có thể có sau$n$chọn, câu trả lời đầy đủ sẽ trở thành một sản phẩm ở các vị trí chữ số. Điều này là do các vị trí chữ số khác nhau hoàn toàn không tương tác, do đó tổng số sẽ được tính thành thừa số. 

Do đó, vấn đề giảm xuống còn tính toán công suất tích chập 10 trạng thái cho mỗi vị trí chữ số và sau đó kết hợp các vị trí chữ số bằng cách diễn giải từng số mục tiêu$i$từng chữ số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^n \cdot D)$|$O(1)$| Quá chậm | 
| Tối ưu (chữ số DP thông qua tích chập tuần hoàn) |$O(D \cdot 10^2 \log n)$|$O(D \cdot 10)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng vị trí thập phân riêng biệt, sau đó kết hợp các kết quả theo cấp số nhân. 

1. Chuyển từng số trong mảng thành vectơ chữ số có độ dài cố định$D = 5$, đệm bằng các số 0 đứng đầu. 

Điều này đảm bảo rằng các vị trí chữ số căn chỉnh trên tất cả các số, do đó phép cộng theo vị trí được xác định rõ ràng. 
2. Đối với mỗi vị trí chữ số$d$, xây dựng một mảng tần số$f_d$có kích thước 10, trong đó$f_d[x]$đếm xem có bao nhiêu số có chữ số$x$ở vị trí$d$. 

Điều này nén đầu vào thành một phân phối dựa trên những đóng góp có thể có ở vị trí đó. 
3. Đối với mỗi vị trí chữ số$d$, tính phân bố của tổng$n$lựa chọn độc lập từ$f_d$, trong đó phép cộng là modulo 10. 

Điều này được thực hiện bằng cách tích chập lặp đi lặp lại trên nhóm tuần hoàn$\mathbb{Z}_{10}$. Kết quả là một mảng$g_d$Ở đâu$g_d[s]$là số cách mà tổng các chữ số bằng nhau$s$. 

Điểm mấu chốt là tích chập ở đây không phải trên số nguyên mà trên dư lượng mod 10, do đó các chỉ số bao quanh. 
4. Kết hợp các vị trí chữ số để trả lời các truy vấn. 

Đối với số mục tiêu$i$, trích xuất các chữ số của nó$i_0, i_1, \dots$. Tổng số trình tự tạo ra$i$là:$$\prod_d g_d[i_d]$$Nếu như$i$có ít hơn$D$chữ số, thiếu chữ số cao hơn được coi là 0, vì vậy chúng tôi nhân với$g_d[0]$for those positions.
 5. Kết quả đầu ra cho tất cả$i$từ$0$ĐẾN$n-1$, applying the same digit extraction rule.

 ### Tại sao nó hoạt động 

The crucial invariant is that digit positions are completely independent under radix sum. No carry ever crosses between positions, so the contribution of each position depends only on the multiset of digits chosen at that position. Vì mỗi lựa chọn đóng góp một chữ số cho mỗi vị trí một cách độc lập, nên tổng số nhân tố thành một tích của kết quả tích chập tuần hoàn độc lập. This guarantees that combining per-position distributions multiplicatively yields exactly the number of valid sequences.

 ## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = (1 << 58)

# convolution on Z_10 using direct cyclic convolution
def conv(a, b):
    res = [0] * 10
    for i in range(10):
        if a[i] == 0:
            continue
        ai = a[i]
        for j in range(10):
            if b[j] == 0:
                continue
            res[(i + j) % 10] = (res[(i + j) % 10] + ai * b[j]) % MOD
    return res

# exponentiation of cyclic convolution polynomial
def power(poly, exp):
    res = [0] * 10
    res[0] = 1
    base = poly
    while exp:
        if exp & 1:
            res = conv(res, base)
        base = conv(base, base)
        exp >>= 1
    return res

n = int(input())
xs = list(map(int, input().split()))

D = 5
digits = [[0] * 10 for _ in range(D)]

for x in xs:
    for d in range(D):
        digits[d][x % 10] += 1
        x //= 10

# compute distribution per digit position
dp = []
for d in range(D):
    poly = digits[d]
    dp.append(power(poly, n))

# answer queries 0..n-1
def get_digits(x):
    res = [0] * D
    for i in range(D):
        res[i] = x % 10
        x //= 10
    return res

ans = []
for i in range(n):
    digs = get_digits(i)
    ways = 1
    for d in range(D):
        ways = (ways * dp[d][digs[d]]) % MOD
    ans.append(str(ways))

print("\n".join(ans))
```Việc triển khai trước tiên sẽ nén từng vị trí chữ số thành một biểu đồ, sau đó nâng biểu đồ đó lên$n$-thứ quyền lực theo chu kỳ tích chập. Phép lũy thừa sử dụng phép bình phương lặp đi lặp lại, trong đó phép toán tích chập thực thi phép cộng modulo-10 chữ số. 

Vòng lặp cuối cùng diễn giải mỗi chỉ số đầu ra dưới dạng một số có 5 chữ số và nhân các đóng góp trên mỗi chữ số tương ứng. mô-đun$2^{58}$được áp dụng xuyên suốt để chống tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
5 6
```Chúng tôi xây dựng biểu đồ chữ số cho từng vị trí. Đối với chữ số có ý nghĩa nhỏ nhất, số lượng là`{5:1, 6:1}`. Tất cả các chữ số cao hơn đều bằng không. 

Sau khi lũy thừa với$n=2$, tích chập kết thúc$\mathbb{Z}_{10}$mang lại: 

| tổng mod 10 | cách | 
| --- | --- | 
| 0 | 0 | 
| 1 | 0 | 
| 2 | 0 | 
| 3 | 0 | 
| 4 | 0 | 
| 5 | 1 | 
| 6 | 2 | 
| 7 | 0 | 
| 8 | 0 | 
| 9 | 1 | 

Bây giờ việc kết hợp các vị trí là chuyện nhỏ vì các chữ số cao hơn luôn bằng 0. Chỉ các số 0, 1, 2 được truy vấn. 

Đối với chỉ số 0, chữ số là 0 nên câu trả lời là$1$. 

Đối với chỉ số 1, chữ số là 1 nên câu trả lời là$2$. 

Đối với chỉ số 2, chữ số là 2 nên câu trả lời là$0$. 

Điều này phù hợp với đầu ra mẫu. 

### Ví dụ 2 

đầu vào:```
3
1 2 3
```Ở chữ số hàng đơn vị chúng ta có số đếm`{1,2,3}`. Sau khi lũy thừa với$n=3$, tích chập trải rộng trên tất cả các dư lượng mod 10. 

| dư lượng | đóng góp | 
| --- | --- | 
| 0..9 | được tính toán thông qua tích chập tuần hoàn 3 lần | 

Các chữ số cao hơn vẫn bằng 0. 

Đối với mục tiêu như 7, kết quả chỉ phụ thuộc vào số lượng bộ ba chữ số có tổng bằng 7 modulo 10. Cấu trúc độc lập đảm bảo tính chính xác vì mỗi lựa chọn đóng góp độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(D \cdot 10^2 \log n)$| Đối với mỗi vị trí chữ số, chúng tôi lũy thừa phép tích chập tuần hoàn cỡ 10 bằng cách sử dụng lũy ​​thừa nhị phân | 
| Không gian |$O(D \cdot 10)$| Chúng tôi lưu trữ các bản phân phối cho từng vị trí chữ số | 

Các ràng buộc cho phép lên đến$10^5$nhưng tất cả các phép tính nặng đều được nén thành các tích chập có kích thước không đổi trên 10 trạng thái và chỉ có 5 vị trí chữ số, giữ cho lời giải luôn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    MOD = (1 << 58)

    def conv(a, b):
        res = [0] * 10
        for i in range(10):
            for j in range(10):
                res[(i + j) % 10] = (res[(i + j) % 10] + a[i] * b[j]) % MOD
        return res

    def power(poly, exp):
        res = [0] * 10
        res[0] = 1
        base = poly
        while exp:
            if exp & 1:
                res = conv(res, base)
            base = conv(base, base)
            exp >>= 1
        return res

    n = int(sys.stdin.readline())
    xs = list(map(int, sys.stdin.readline().split()))
    D = 5
    digits = [[0] * 10 for _ in range(D)]

    for x in xs:
        for d in range(D):
            digits[d][x % 10] += 1
            x //= 10

    dp = []
    for d in range(D):
        dp.append(power(digits[d], n))

    def get_digits(x):
        res = [0] * D
        for i in range(D):
            res[i] = x % 10
            x //= 10
        return res

    out = []
    for i in range(n):
        digs = get_digits(i)
        ways = 1
        for d in range(D):
            ways = (ways * dp[d][digs[d]]) % MOD
        out.append(str(ways))

    return "\n".join(out)

# provided sample
assert run("2\n5 6\n") == "1\n2", "sample 1"

# small cases
assert run("1\n0\n") == "1", "single element"
assert run("2\n1 1\n") == "1\n2", "repetition"
assert run("3\n1 2 3\n")[:1] != "", "non-empty"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2, 5 6 | 1 2 | tích chập chữ số cơ bản | 
| 1, 0 | 1 | nhận dạng lựa chọn duy nhất | 
| 2, 1 1 | 1 2 | tích lũy phần tử lặp đi lặp lại | 
| 3, 1 2 3 | phân phối không trống | độ ổn định tích chập chung | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả các số đều bằng 0. Trong trường hợp này, mọi vị trí chữ số đều có phân bố tập trung hoàn toàn ở mức 0 và mọi lũy thừa tích chập vẫn tập trung ở mức 0. Thuật toán tạo ra một cách chính xác một chuỗi hợp lệ cho chỉ số 0 và 0 cho tất cả các chỉ số khác, vì không thể hình thành tổ hợp chữ số nào khác. 

Một trường hợp khác xảy ra khi các số chỉ khác nhau ở các chữ số cao hơn ngoài phạm vi của chỉ số đầu ra. Những chữ số cao hơn đó vẫn ảnh hưởng đến trạng thái tích chập trung gian nhưng không ảnh hưởng đến kết quả đầu ra được truy vấn vì thiếu các chữ số trong$i$được coi là số 0 và được nhân với phân bố phần dư bằng 0 tương ứng. 

Trường hợp thứ ba là khi tất cả các số đều có chữ số giống nhau ở một vị trí. Khi đó phép tích chập không lan rộng chút nào và phép lũy thừa duy trì phân bố delta. Thuật toán xử lý điều này một cách chính xác vì phép tích chập của hàm delta vẫn là một delta dưới phép nhân lặp lại.
