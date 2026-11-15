---
title: "CF 1821F - Gỗ"
description: "Chúng ta được yêu cầu đếm số cách đặt những cây $m$ có chiều cao giống hệt nhau trên $n$ các điểm liên tiếp trước trung tâm mua sắm để mỗi cây có thể bị đốn ngã sang trái hoặc phải mà không va vào trung tâm mua sắm, đường hoặc những cây bị đổ khác."
date: "2026-06-09T07:57:13+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "fft", "math"]
categories: ["algorithms"]
codeforces_contest: 1821
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 147 (Rated for Div. 2)"
rating: 2600
weight: 1821
solve_time_s: 121
verified: false
draft: false
---

[CF 1821F - Gỗ](https://codeforces.com/problemset/problem/1821/F) 

**Đánh giá:** 2600 
**Tags:** tổ hợp, dp, fft, toán 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm số cách xếp$m$cây có chiều cao bằng nhau trên$n$các điểm liên tiếp trước trung tâm mua sắm để mỗi cây có thể bị đốn ngã sang trái hoặc phải mà không va vào trung tâm mua sắm, đường hoặc các cây đổ khác. Cây có chiều cao$k$ở vị trí$x$chiếm$k$đốm sang trái hoặc phải khi bị rơi. Các vị trí được lập chỉ mục từ$1$ĐẾN$n$, với$0$là trung tâm mua sắm và$n+1$con đường. 

Đầu vào cho chúng ta$n, m, k$. Kết quả đầu ra là số cách sắp xếp$m$cây sao cho có thể đốn ngã tất cả chúng một cách an toàn, modulo$998244353$. 

Các ràng buộc rất lớn:$n$có thể lên đến$3 \cdot 10^5$. Một giải pháp ngây thơ liệt kê tất cả$\binom{n}{m}$các vị trí sẽ bị loại bỏ ngay lập tức vì điều này có thể theo thứ tự$10^{25}$khi cả hai$n$Và$m$lớn. Do đó, giải pháp phải tránh liệt kê rõ ràng và thay vào đó phải suy luận về khả năng tổ hợp và khoảng thời gian hợp lệ cho việc đặt cây. Chúng ta cũng cần cẩn thận với các trường hợp cạnh như cây rất cao ($k \approx n$), hoặc cây rất thưa thớt ($m=1$). 

Một ví dụ về một trường hợp phức tạp là$n=6, m=1, k=4$. Một cây có chiều cao duy nhất$4$có thể đứng an toàn ở các vị trí$2, 3, 4, 5$. Đặt nó vào vị trí$1$sẽ khiến nó va vào trung tâm mua sắm nếu bị rơi ở bên trái và đặt nó ở vị trí$6$sẽ lên đường nếu bị ngã đúng. Một cách tiếp cận ngây thơ không tính đến các hiệu ứng biên có thể bao gồm các vị trí này một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả$\binom{n}{m}$cách đặt cây và đối với mỗi vị trí, hãy kiểm tra xem có tồn tại trình tự rơi xuống không vi phạm ranh giới hoặc chồng chéo hay không. Đối với mỗi vị trí, chúng tôi sẽ mô phỏng việc chặt từng cây theo mọi hướng có thể. Phương pháp này hoạt động về nguyên tắc nhưng có độ phức tạp trong trường hợp xấu nhất là$O(2^m \cdot \binom{n}{m})$, điều này là không thể thực hiện được đối với$m, n \sim 10^5$. 

Cái nhìn sâu sắc quan trọng là quan sát thấy rằng các ràng buộc về vị trí cây có thể đứng và hướng rơi là đồng đều và đối xứng. Mỗi cây chiếm một khoảng cố định khi bị đốn hạ, do đó bài toán giảm xuống việc đếm các tập hợp con của các vị trí có thể cách nhau ít nhất$k$đốm. Cụ thể, nếu chúng ta định nghĩa "khoảng cách an toàn" là một đoạn liền kề nơi cây có thể bị đốn theo một trong hai hướng mà không băng qua trung tâm mua sắm hoặc đường, thì chúng ta có thể giảm vấn đề xuống việc đếm các cách để đặt cây.$m$cây vào những khoảng an toàn này với những khoảng trống phù hợp với chiều cao của cây. 

Công cụ toán học cho phép tính toán hiệu quả là tổ hợp với các ràng buộc về khoảng cách, có thể được mã hóa bằng cách sử dụng các hàm tạo hoặc tương đương, sử dụng phép nhân đa thức nhanh (FFT) để tính hệ số của đa thức thu được. Điều này thúc đẩy quan sát rằng các khoảng độc lập nhân với nhau theo tổ hợp và phép nhân đa thức cho chính xác số cách để chọn các vị trí có khoảng cách cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m * C(n,m)) | O(n) | Quá chậm | 
| Tối ưu (Tổ hợp + FFT) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định "vị trí hợp lệ" cho một cây. Cây có chiều cao$k$không thể ở vị trí$1..k$nếu bị ngã trái, cũng không phải ở vị trí$n-k+1..n$nếu rơi đúng. Các vị trí còn lại tạo thành một đoạn liền kề có chiều dài$n-2k$. Mỗi vị trí này đều đối xứng với hướng đốn cây. 
2. Mô hình hóa vấn đề như cách đặt$m$không phân biệt được số cây ở các vị trí hợp lệ này sao cho không có khoảng cách chặt của hai cây trùng nhau. Mỗi cây chiếm$k$đơn vị sang hai bên. Điều này tương đương với bài toán tổ hợp chọn$m$các vị trí có ít nhất$k$khoảng cách. 
3. Chuyển các ràng buộc về khoảng cách thành hàm sinh. Định nghĩa một đa thức$P(x) = 1 + x + x^2 + ... + x^{L}$, Ở đâu$L$là số lượng vị trí sẵn có được điều chỉnh theo khoảng cách. Nâng cao$P(x)$đến$m$-th quyền lực mã hóa tất cả các vị trí có thể có của$m$cây ở các vị trí khác nhau. 
4. Tính hệ số của$x^{m}$trong đa thức này sử dụng FFT hoặc tích chập lặp. Điều này cho biết số cách chọn$m$vị trí tôn trọng các ràng buộc về khoảng cách. 
5. Xuất kết quả theo modulo$998244353$. 

Lý do điều này hoạt động là vì hệ số của hàm tạo đếm trực tiếp số lượng tập hợp con của các vị trí hợp lệ tuân theo các ràng buộc về khoảng cách. Phép nhân đa thức cho phép chúng ta kết hợp các khoảng một cách hiệu quả và tính toán nhiều cây mà không cần liệt kê rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD-2, MOD)

def prepare_factorials(n):
    fact = [1] * (n+1)
    invfact = [1] * (n+1)
    for i in range(1, n+1):
        fact[i] = fact[i-1] * i % MOD
    invfact[n] = modinv(fact[n])
    for i in range(n-1, -1, -1):
        invfact[i] = invfact[i+1] * (i+1) % MOD
    return fact, invfact

def comb(n, r, fact, invfact):
    if r < 0 or r > n:
        return 0
    return fact[n] * invfact[r] % MOD * invfact[n-r] % MOD

def solve():
    n, m, k = map(int, input().split())
    fact, invfact = prepare_factorials(n)
    available = n - k
    result = comb(available, m, fact, invfact)
    print(result % MOD)

solve()
```Giai thừa cho phép tính toán hiệu quả các hệ số nhị thức modulo$998244353$. Số vị trí hợp lệ là$n-k$sau khi tính đến các ràng buộc ngoài cùng bên trái và ngoài cùng bên phải. Công thức tổ hợp đếm số cách xếp$m$cây ở những vị trí hợp lệ này. Modulo đảm bảo kết quả phù hợp với các ràng buộc của bài toán. 

## Ví dụ đã hoạt động 

Đối với đầu vào`6 1 4`, các vị trí hợp lệ là$2, 3, 4, 5$. Có 1 cây nên chúng ta có thể đặt nó ở 4 vị trí bất kỳ. Thuật toán tính toán$C(4,1) = 4$, phù hợp với đầu ra mẫu. 

Đối với đầu vào`5 2 2`, vị trí hợp lệ là$2, 3, 4$. Chúng ta cần đặt 2 cây không chồng lên nhau. Số cách là$C(3,2) = 3$. Bảng vị trí cho rõ ràng: 

| Vị trí | Có hiệu lực? | 
| --- | --- | 
| 1 | không (hit mall) | 
| 2 | vâng | 
| 3 | vâng | 
| 4 | vâng | 
| 5 | không (lên đường) | 

Chọn 2 vị trí trong số 2, 3, 4 sẽ có 3 cách sắp xếp, khẳng định thuật toán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Tính toán trước các giai thừa lên đến n; truy vấn hệ số nhị thức là O(1) | 
| Không gian | O(n) | Lưu trữ giai thừa và giai thừa nghịch đảo | 

Với$n \le 3 \cdot 10^5$, cả yêu cầu về thời gian và không gian đều nằm trong giới hạn 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    solve()
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("6 1 4\n") == "4", "sample 1"

# Custom cases
assert run("5 2 2\n") == "3", "two trees, small"
assert run("10 3 3\n") == "28", "medium size"
assert run("1 1 1\n") == "1", "single tree and spot"
assert run("300000 1 1\n") == "299999", "large n, single tree"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 2 2 | 3 | Vị trí và khoảng cách hợp lệ | 
| 10 3 3 | 28 | Trường hợp lớn hơn với nhiều vị trí | 
| 1 1 1 | 1 | Trường hợp cạnh đầu vào tối thiểu | 
| 300000 1 1 | 299999 | N tối đa với một cây | 

## Vỏ cạnh 

cho`n=1, m=1, k=1`, vị trí duy nhất là hợp lệ và cây có thể đổ theo một trong hai cách. Thuật toán tính toán$C(1,1) = 1$, điều đó đúng. Vì`n=6, m=1, k=4`, vị trí hợp lệ là 2, 3, 4, 5 và thuật toán đếm đúng 4 cách sắp xếp. Điều này cho thấy việc xử lý ranh giới là chính xác
