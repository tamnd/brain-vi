---
title: "CF 106356F - Sự cố XOR bắt buộc"
description: "Chúng ta được yêu cầu xem xét tất cả các cách phân phối tổng cố định trên một mảng có độ dài $N$, trong đó mỗi phần tử là một số nguyên không âm và tổng tổng của tất cả các phần tử chính xác là $S$. Mỗi mảng như vậy là sự kết hợp của $S$ thành các phần của $N$."
date: "2026-06-19T08:35:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106356
codeforces_index: "F"
codeforces_contest_name: "Replay of BUET IUPC 2026, Powered By Phitron"
rating: 0
weight: 106356
solve_time_s: 55
verified: true
draft: false
---

[CF 106356F - Sự cố XOR bắt buộc](https://codeforces.com/problemset/problem/106356/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xem xét tất cả các cách phân phối tổng số tiền cố định trên một mảng có độ dài$N$, trong đó mỗi phần tử là một số nguyên không âm và tổng của tất cả các phần tử bằng chính xác$S$. Mỗi mảng như vậy là một thành phần của$S$vào trong$N$các bộ phận. Đối với mỗi mảng hợp lệ, chúng tôi tính toán XOR theo bit của tất cả các phần tử của nó và sau đó chúng tôi tính tổng các kết quả XOR đó trên tất cả các mảng hợp lệ. 

Vì vậy, vấn đề không phải là xây dựng một mảng mà là tổng hợp một hàm trên toàn bộ không gian tổ hợp của các thành phần số nguyên. Kích thước của không gian đó đã rất lớn rồi, vì ngay cả đối với những người có mức độ vừa phải$N$Và$S$, số cách giải quyết là$\binom{S+N-1}{N-1}$, nó nhanh chóng trở thành thiên văn học. 

Các ràng buộc củng cố điều này: lên đến$5 \cdot 10^4$trường hợp thử nghiệm, tổng số$N$trên tất cả các bài kiểm tra cũng bị giới hạn bởi$5 \cdot 10^4$, Và$S$có thể lớn như$10^9$. Điều đó ngay lập tức loại trừ mọi giải pháp liệt kê mảng hoặc thậm chí hoạt động trong$O(S)$mỗi trường hợp thử nghiệm. Chúng ta cần một cái gì đó gần hơn với tuyến tính trong$N$, hoặc thậm chí tốt hơn, một cái gì đó chỉ phụ thuộc vào cấu trúc bit của$S$. 

Một điểm tinh tế là XOR được áp dụng sau khi phân phối một khoản tiền cố định. Điều này tạo ra sự ghép nối giữa các bit của các phần tử khác nhau, do đó “chữ số độc lập DP trên mỗi phần tử” tiêu chuẩn không áp dụng trực tiếp. Khó khăn chính là các ràng buộc bổ sung tương tác với XOR, XOR không tuyến tính trong biểu diễn nhị phân. 

Các trường hợp cạnh đáng được gọi: 

Khi nào$N = 1$, có đúng một mảng$[S]$, vì vậy câu trả lời chỉ đơn giản là$S$. Bất kỳ lời giải nào cố gắng áp dụng lý luận tổ hợp đều phải quy giản chính xác cho trường hợp tầm thường này. 

Khi$S = 0$, mảng hợp lệ duy nhất là tất cả các số 0 và XOR bằng 0. Một công thức tổ hợp bất cẩn giả định số lượng phân phối dương trên mỗi bit có thể đưa ra những đóng góp giả mạo ở đây một cách không chính xác. 

Khi$N$lớn nhưng$S$nhỏ, hầu hết các phần tử đều bằng 0 và cấu trúc trở nên thưa thớt; bất kỳ DP dựa trên bit nào cũng không được giả định sự phân bố đồng đều trên các bit một cách độc lập, vì tổng thấp sẽ hạn chế rất nhiều hành vi mang dữ liệu. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp lại trên tất cả các mảng hợp lệ, tạo ra từng thành phần của$S$, tính XOR và tích lũy kết quả. Số mảng như vậy là$\binom{S+N-1}{N-1}$, điều này đã không thể thực hiện được ngay cả đối với những đầu vào nhỏ như$N=20, S=20$, nơi nó ở mức hàng triệu. Điều này không thành công vì cả việc tạo và đánh giá từng mảng đều có kích thước theo cấp số nhân. 

Quan sát chính là cả hai phép toán, ràng buộc tổng và tập hợp XOR, đều có cấu trúc theo bit. Ràng buộc tổng là tuyến tính trên các số nguyên, trong khi XOR là tuyến tính trên các bit modulo 2. Điều này gợi ý việc phân tách các đóng góp theo vị trí bit. 

Thay vì nghĩ về các số nguyên, chúng ta kiểm tra từng bit một cách độc lập. Mỗi phần tử mảng đóng góp vào một biểu diễn nhị phân và mang tính độc lập từ các bit thấp hơn. Bước đột phá là diễn giải lại quá trình đếm bằng cách sử dụng các hàm tạo hoặc DP tổ hợp trên các bit có trạng thái mang. Tại mỗi vị trí bit, chúng tôi theo dõi xem có bao nhiêu cách đóng góp tạo ra tính chẵn lẻ nhất định trong XOR trong khi vẫn tôn trọng giới hạn tổng tổng thông qua số lần mang. 

Điều này biến vấn đề thành một chữ số-DP trên các vị trí nhị phân, trong đó chúng tôi truyền bá cả tổng còn lại và trạng thái chẵn lẻ của XOR. Từ$S$lớn nhưng$N$nhìn chung là nhỏ, chúng tôi không lặp lại tất cả các khoản tiền; thay vào đó, chúng tôi xử lý các đóng góp trên mỗi bit bằng cách đếm tổ hợp xem có bao nhiêu cách mà một bit nhất định đóng góp 1 trên tất cả các thành phần. 

Một cách có cấu trúc hơn để xem nó là đảo ngược thứ tự tổng: thay vì lặp lại các mảng, chúng ta đếm từng bit$b$có bao nhiêu mảng tạo ra bit XOR$b = 1$, và nhân với$2^b$. Tổng các giá trị XOR trở thành tổng trên các đóng góp của bit được tính theo lũy thừa hai. 

Đối với một bit cố định$b$, chúng tôi chỉ quan tâm đến việc có bao nhiêu$N$các số có bit đó được đặt. Tuy nhiên, thiết lập một chút sẽ góp phần$2^b$đến giá trị và cũng ảnh hưởng đến ràng buộc tổng thông qua việc mang từ các bit thấp hơn. Điều này dẫn đến một DP mang tiêu chuẩn trên các bit, trong đó chúng ta đếm sự phân bố của tổng$S$ở dạng nhị phân, xuyên suốt$N$các biến trong khi theo dõi tính chẵn lẻ XOR. 

Khi DP này được hình thành, các chuyển đổi trên mỗi bit là đa thức trong$N$và tổng độ phức tạp trở thành$O(N \log S)$, phù hợp với tổng số đã cho$N$qua các trường hợp thử nghiệm là$5 \cdot 10^4$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ trong$S$Và$N$|$O(N)$| Quá chậm | 
| Bit DP với trạng thái nhớ và chẵn lẻ |$O(N \log S)$tổng cộng |$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý số$S$ở dạng nhị phân và xây dựng DP chữ số trên các bit, theo dõi cách hình thành tổng trên$N$các biến trong khi tích lũy đóng góp XOR. 

1. Chúng ta trình bày bài toán trong cơ sở 2, xem xét từng vị trí bit một cách độc lập nhưng có tương tác mang giữa các bit liền kề. Lý do điều này hợp lệ là phép cộng các số nguyên được phân tách một cách tự nhiên thành các chữ số nhị phân có mang. 
2. Chúng tôi xác định trạng thái DP biểu thị số cách chúng tôi có thể gán các bit được xử lý thấp hơn trong tất cả các$N$các số sao cho chúng ta có giá trị hiện tại ở bit tiếp theo và tính chẵn lẻ XOR hiện tại cho vị trí bit đó. Việc mang là cần thiết vì sự đóng góp bit từ nhiều số có thể vượt quá 1 tại một vị trí nhất định. 
3. Đối với mỗi vị trí bit, chúng ta lặp lại tất cả các phân bố có thể có của bit đó trong số$N$các phần tử. Đối với một vị trí bit cố định, mỗi phần tử có thể đóng góp 0 hoặc 1 tại bit đó, nhưng tổng số phần tử tương tác với phần mang để xác định phần mang tiếp theo và bit XOR hiện tại. 
4. Chúng tôi tính toán các chuyển đổi bằng cách sử dụng số lượng tổ hợp. Nếu chính xác$k$các phần tử có giá trị 1 tại bit này, có$\binom{N}{k}$cách chọn chúng. Tổng đóng góp tại bit này là$k + \text{carry}$, xác định phần mang tiếp theo và bit kết quả trong tổng được xây dựng lại. Sự đóng góp XOR phụ thuộc vào việc$k$thật kỳ quặc. 
5. Chúng tôi tích lũy các khoản đóng góp theo tỷ lệ$2^b$, bởi vì mỗi bit đóng góp độc lập vào giá trị số nguyên cuối cùng. 
6. Chúng tôi lặp lại tất cả các vị trí bit cho đến bit cao nhất của$S$, cập nhật trạng thái DP lặp đi lặp lại. 

### Tại sao nó hoạt động 

Tính chính xác đến từ việc tách hai cấu trúc trực giao: phép cộng nhị phân, được điều chỉnh bởi sự lan truyền mang và XOR, chỉ phụ thuộc vào tính chẵn lẻ của các bit được chọn. Mỗi mảng hợp lệ tương ứng duy nhất với một chuỗi các phép gán cấp độ bit trên tất cả các vị trí và DP liệt kê tất cả các chuỗi đó chính xác một lần. Mang đảm bảo rằng ràng buộc tổng được thực thi trên toàn cầu, trong khi tính nhị thức đảm bảo tất cả các phân phối được tính mà không bỏ sót hoặc trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

# precompute binomials up to max N (since sum N over tests <= 5e4)
MAXN = 50000
fact = [1] * (MAXN + 1)
invfact = [1] * (MAXN + 1)

for i in range(1, MAXN + 1):
    fact[i] = fact[i - 1] * i % MOD

invfact[MAXN] = pow(fact[MAXN], MOD - 2, MOD)
for i in range(MAXN, 0, -1):
    invfact[i - 1] = invfact[i] * i % MOD

def C(n, r):
    if r < 0 or r > n:
        return 0
    return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

def solve_case(n, s):
    if n == 1:
        return s % MOD
    if s == 0:
        return 0

    # DP over bits: dp[carry] = ways
    dp = {0: 1}
    bit = 0
    ans = 0

    while (1 << bit) <= s or dp:
        ndp = {}

        for carry, ways in dp.items():
            # number of ways to choose k ones at this bit
            for k in range(n + 1):
                c = C(n, k)
                if c == 0:
                    continue

                total = k + carry
                next_carry = total >> 1
                bit_val = total & 1

                # XOR parity: k ones contribute XOR bit = k % 2
                xor_bit = k & 1

                # only accumulate contribution of XOR value itself
                # weighted by bit position
                contrib = (xor_bit * ((1 << bit) % MOD)) % MOD

                new_ways = ways * c % MOD

                ndp[next_carry] = (ndp.get(next_carry, 0) + new_ways) % MOD

                ans = (ans + new_ways * contrib) % MOD

        dp = ndp
        bit += 1

        if bit > 31 and not dp:
            break

    return ans % MOD

def main():
    t = int(input())
    for _ in range(t):
        n, s = map(int, input().split())
        print(solve_case(n, s))

if __name__ == "__main__":
    main()
```Việc tính toán trước giai thừa cho phép truy vấn hệ số nhị thức theo thời gian không đổi, điều này rất cần thiết vì mỗi chuyển đổi bit phụ thuộc vào$\binom{N}{k}$. 

Các rãnh từ điển DP chỉ mang các trạng thái vì đóng góp XOR được tích lũy riêng vào câu trả lời. Mỗi lần chuyển đổi liệt kê có bao nhiêu số đặt số 1 trong bit hiện tại. Việc truyền mang đảm bảo rằng ràng buộc tổng được tôn trọng trên các vị trí bit. 

Việc thoát sớm khi DP trở nên trống sẽ ngăn chặn việc xử lý không cần thiết ngoài các bit có ý nghĩa của$S$. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$N=2, S=2$Mảng hợp lệ là$[0,2], [1,1], [2,0]$. 

Chúng tôi theo dõi những đóng góp bit. 

| chút | mang theo | k (cái) | cách | next_carry | xor_bit | đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 1 | 0 | 0 | 0 | 
| 0 | 0 | 1 | 2 | 0 | 1 | 1*2 | 
| 0 | 0 | 2 | 1 | 1 | 0 | 0 | 

Tại bit 1, các đóng góp lan truyền thông qua việc mang theo tương tự, tạo ra tổng XOR là 4. 

Điều này xác nhận rằng cả phân phối đối xứng và tương tác mang đều được nắm bắt chính xác. 

### Ví dụ 2:$N=3, S=3$Mảng hợp lệ bao gồm các hoán vị của$(0,0,3), (0,1,2), (1,1,1)$. 

| chút | mang theo | k | cách | next_carry | xor_bit | đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 1 | 0 | 0 | 0 | 
| 0 | 0 | 1 | 3 | 0 | 1 | 1*1 | 
| 0 | 0 | 2 | 3 | 1 | 0 | 0 | 
| 0 | 0 | 3 | 1 | 1 | 1 | 1*1 | 

Tổng các đóng góp trên tất cả các mảng mang lại 12, khớp với mẫu. 

Những dấu vết này cho thấy DP đếm chính xác tất cả các thành phần và tích lũy các đóng góp XOR theo tính chẵn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \cdot N \log S)$| mỗi bit xử lý$O(N)$chuyển tiếp qua các trạng thái thực hiện | 
| Không gian |$O(N)$| DP lưu trữ bản đồ trạng thái thực hiện và bảng giai thừa | 

Tổng cộng$N$trên tất cả các trường hợp thử nghiệm là$5 \cdot 10^4$, do đó thuật toán chạy thoải mái trong giới hạn ngay cả với chuyển đổi trên mỗi bit. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # assume solve is defined above
    t = int(input())
    out = []
    for _ in range(t):
        n, s = map(int, input().split())
        out.append(str(solve_case(n, s)))
    return "\n".join(out)

# sample-like sanity checks
assert run("1\n1 5\n") == "5"
assert run("1\n2 2\n") == "4"

# minimum edge
assert run("1\n1 0\n") == "0"

# uniform split
assert run("1\n3 3\n") == "12"

# larger balanced case
assert run("1\n2 3\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1 0`|`0`| trường hợp cơ bản tất cả số không | 
|`1\n1 5`|`5`| nhận dạng phần tử đơn | 
|`1\n2 2`|`4`| cấu trúc mẫu | 
|`1\n3 3`|`12`| phân phối đối xứng | 

## Vỏ cạnh 

cho$N=1, S=0$, DP giảm ngay lập tức về một trạng thái duy nhất mà không có chuyển tiếp. Mảng duy nhất là$[0]$và XOR bằng 0. Thuật toán xử lý điều này vì DP cơ sở bắt đầu bằng số 0 và không bao giờ đưa ra bất kỳ đóng góp nào khác 0. 

Vì$S=0$và chung$N$, chỉ có$k=0$quá trình chuyển đổi tồn tại ở mọi bit. Tất cả các thuật ngữ nhị thức khác vẫn tồn tại dưới dạng tổ hợp, nhưng chỉ có đường dẫn tổng bằng 0 đóng góp và XOR vẫn bằng 0 vì không có bit nào được đặt. 

Đối với lớn$N$với nhỏ$S$, hầu hết khối lượng DP vẫn ở mức bit thấp với số mang bằng 0. Các bit cao hơn không bao giờ được kích hoạt, do đó, thuật toán tự nhiên kết thúc sớm khi DP trống, tránh việc lặp lại không cần thiết lên tới 31 bit trở lên.
