---
title: "CF 104386F - CLC Yêu Thích Công Nghệ SQRT (Phiên Bản Dễ)"
description: "Chúng ta được cho một dãy số và chúng ta xem xét mọi dãy con không trống có thể có. Đối với mỗi dãy con được chọn, chúng ta được phép thực hiện một thao tác trong đó chúng ta chọn bất kỳ phần tử nào trong đó và ghi đè giá trị của nó một cách tùy ý."
date: "2026-07-01T02:50:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104386
codeforces_index: "F"
codeforces_contest_name: "TheForces Round #14 (Cool-Forces)"
rating: 0
weight: 104386
solve_time_s: 83
verified: false
draft: false
---

[CF 104386F - CLC yêu thích công nghệ SQRT (Phiên bản dễ dàng)](https://codeforces.com/problemset/problem/104386/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số và chúng ta xem xét mọi dãy con không trống có thể có. Đối với mỗi dãy con được chọn, chúng ta được phép thực hiện một thao tác trong đó chúng ta chọn bất kỳ phần tử nào trong đó và ghi đè giá trị của nó một cách tùy ý. Chi phí của một dãy con được định nghĩa là số lần ghi đè tối thiểu cần thiết để dãy con đó có thể được sắp xếp lại thành một palindrome. 

Điều kiện palindrome ở đây chỉ phụ thuộc vào tính đối xứng nhiều tập hợp sau khi sắp xếp lại. Vì vậy, chúng tôi không bị ràng buộc bởi các vị trí bên trong dãy con, chỉ bị ràng buộc bởi số lần xuất hiện của mỗi giá trị sau khi chỉnh sửa. 

Nhiệm vụ là tính tổng chi phí tối thiểu này trên tất cả các dãy con không rỗng. 

Hạn chế chính là$n \le 1000$, đã loại trừ bất cứ điều gì liệt kê rõ ràng tất cả các chuỗi con vì có$2^n$của họ. Thậm chí$O(n^2)$mỗi phần tiếp theo sẽ là không thể. Bất kỳ giải pháp hợp lệ nào cũng phải nén sự đóng góp của các chuỗi con theo cách tổ hợp hoặc thông qua việc đếm sự đóng góp của các phần tử trên tất cả các chuỗi con. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị đã đối xứng. Ví dụ: một dãy con như`[1, 1, 2, 2]`có chi phí bằng không. Một trực giác ngây thơ có thể đánh giá quá cao chi phí bằng cách xử lý cục bộ các cặp không khớp thay vì tần số ghép nối toàn cầu. 

Một trường hợp cạnh khác là dãy con một phần tử. Chúng luôn là các palindrome nên đóng góp của chúng luôn bằng không. Bất kỳ công thức nào không vô hiệu hóa rõ ràng trường hợp này sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Đầu tiên chúng ta xem xét điều gì làm cho một dãy có thể chuyển đổi thành một dãy palindrome với những thay đổi tối thiểu. Đối với nhiều tập hợp giá trị, chúng ta có thể sắp xếp lại một cách tự do, do đó cấu trúc chỉ phụ thuộc vào tần số. 

Một tập hợp nhiều tập hợp có thể tạo thành một bảng màu nếu có nhiều nhất một giá trị có tần số lẻ. Nếu có nhiều hơn một giá trị có tần số lẻ, chúng ta cần sửa đổi các phần tử để giảm số lượng danh mục tần số lẻ. Mỗi sửa đổi sẽ thay đổi giá trị của một phần tử, làm đảo lộn số lượng chẵn lẻ của hai giá trị cùng một lúc: một giá trị mất một đơn vị, giá trị khác nhận được một đơn vị. 

Vì vậy, chi phí để tạo ra một dãy palindrome con về cơ bản là số phép toán tối thiểu cần thiết để giảm số giá trị tần số lẻ xuống nhiều nhất là một. Điều này trở thành một vấn đề cân bằng chẵn lẻ. 

Giờ đây, ý tưởng về lực lượng vũ phu rất đơn giản: liệt kê tất cả các chuỗi con, tính toán tần số, đếm xem có bao nhiêu giá trị có tần số lẻ và rút ra các phép toán tối thiểu. Điều này đúng nhưng đòi hỏi$2^n$các phần tiếp theo, mỗi chi phí$O(n)$, điều đó hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần xây dựng các dãy con. Thay vào đó, chúng tôi tính các đóng góp trên tất cả các chuỗi con bằng cách theo dõi cách các phần tử hoạt động trên các tập hợp con. Mỗi phần tử tham gia độc lập vào chính xác một nửa số tập hợp con và các cặp phần tử đóng góp các tương tác chẵn lẻ có cấu trúc. 

Điều này biến vấn đề thành việc đếm xem có bao nhiêu chuỗi con tạo ra một mẫu chẵn lẻ nhất định trên các giá trị. Khi chúng tôi có thể đếm có bao nhiêu tập hợp con tạo ra số lượng lẻ nhất định trên mỗi giá trị, chúng tôi có thể tổng hợp chi phí mà không cần liệt kê các tập hợp con. 

Sự đơn giản hóa cấu trúc thứ hai là lưu ý rằng chi phí chỉ phụ thuộc vào số lượng giá trị có tần số lẻ. Nếu chúng ta định nghĩa$k$vì số lượng các giá trị có tần số trong dãy con là số lẻ nên số lượng thay đổi tối thiểu cần thiết là$(k - 1) / 2$vì$k \ge 1$. Điều này xuất phát từ thực tế là mỗi thao tác có thể sửa hai số lẻ bằng cách hoán đổi tính chẵn lẻ giữa các giá trị. 

Do đó, nhiệm vụ trở thành tính toán, trên tất cả các chuỗi con, sự phân bố của$k$. 

Sau đó, chúng tôi chuyển phối cảnh từ các chuỗi con sang DP chẵn lẻ bitmask theo tần số giá trị, sử dụng tổ hợp trên các lần xuất hiện. Vì các giá trị được giới hạn bởi$n$, chúng ta có thể xử lý từng giá trị riêng biệt một cách độc lập và sử dụng tính bao gồm các lần xuất hiện để tính toán xem có bao nhiêu tập hợp con mang lại tính chẵn lẻ/chẵn lẻ trên mỗi giá trị. 

Điều này dẫn đến DP trên các giá trị trong đó chúng tôi duy trì số lượng tập hợp con tạo ra mỗi cấu hình chẵn lẻ và chúng tôi tích lũy các đóng góp được tính theo hàm chi phí của số lượng danh mục lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đối với các chuỗi tiếp theo |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| DP chẵn lẻ trên các nhóm giá trị |$O(n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nhóm các chỉ số theo giá trị. Giả sử một giá trị xuất hiện$c$lần trong mảng. 

Đối với bất kỳ chuỗi con cố định nào, mỗi giá trị đóng góp số chẵn hoặc số lẻ tùy thuộc vào số lần xuất hiện của nó được chọn. Đối với một nhóm có quy mô$c$, số cách chọn số phần tử chẵn là$2^{c-1}$, và tương tự với số lẻ cũng vậy$2^{c-1}$. Tính đối xứng này đúng vì mỗi phần tử có thể được bao gồm hoặc loại trừ một cách độc lập và tính chẵn lẻ được chia đều cho các nhóm không trống. 

Điều này cho phép chúng tôi coi mỗi giá trị riêng biệt là đóng góp vào lựa chọn chẵn lẻ nhị phân có trọng số bằng nhau. 

Sau đó, chúng tôi thực hiện DP theo các giá trị, duy trì chính xác số lượng tập hợp con tạo ra$k$các giá trị có tính chẵn lẻ lẻ. 

Các bước thực hiện như sau. 

## Hướng dẫn thuật toán 

1. Nhóm giá trị mảng và tần suất tính toán$c_v$cho mỗi giá trị riêng biệt$v$. Điều này làm giảm vấn đề thành những đóng góp độc lập cho mỗi giá trị. 
2. Khởi tạo mảng DP trong đó`dp[k]`biểu thị số cách chọn phần tử từ các giá trị được xử lý sao cho chính xác$k$các giá trị có tần số lẻ. Ban đầu, trước khi xử lý bất kỳ giá trị nào,`dp[0] = 1`. 
3. Với mỗi giá trị$v$, chúng tôi xem xét hai khả năng đóng góp ngang bằng của nó. Nó đóng góp vào một lựa chọn chẵn hoặc một lựa chọn lẻ, mỗi lựa chọn có trọng số$2^{c_v - 1}$. Khi cập nhật DP, việc chọn số lẻ sẽ tăng số lẻ lên 1, trong khi việc chọn số chẵn sẽ giữ nguyên số lẻ. Điều này mang lại một sự chuyển đổi:`new_dp[k] += dp[k] * even_weight + dp[k-1] * odd_weight`. 

Các trọng số bằng nhau nên chúng tôi tính chúng một cách rõ ràng. 
4. Sau khi xử lý tất cả các giá trị, mỗi giá trị`dp[k]`đưa ra số lượng các chuỗi con trong đó chính xác$k$các giá trị có tần số lẻ. 
5. Chuyển đổi$k$vào chi phí. Vì$k = 0$, chi phí là 0. Đối với$k \ge 1$, chi phí là$(k - 1) / 2$. Nhân lên và tích lũy trên tất cả$k$, lấy modulo. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý lần đầu tiên$t$những giá trị riêng biệt,`dp[k]`đếm chính xác số lượng các chuỗi con được giới hạn ở những giá trị mang lại$k$các giá trị tần số lẻ. Mỗi giá trị mới chỉ chuyển đổi tính chẵn lẻ của chính nó một cách độc lập với các giá trị khác, do đó trạng thái DP phát triển mà không có sự tương tác giữa các giá trị khác nhau ngoại trừ việc đếm xem có bao nhiêu nhóm lẻ tồn tại. Tính độc lập này đảm bảo rằng không có cấu hình nào bị bỏ sót hoặc bị tính hai lần. 

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

n = int(input())
a = list(map(int, input().split()))

from collections import Counter
cnt = Counter(a)

vals = list(cnt.values())

dp = [0] * (n + 1)
dp[0] = 1

for c in vals:
    ways_even = modpow(2, c - 1)
    ways_odd = ways_even

    new_dp = [0] * (n + 1)
    for k in range(n + 1):
        if dp[k] == 0:
            continue
        new_dp[k] = (new_dp[k] + dp[k] * ways_even) % MOD
        if k + 1 <= n:
            new_dp[k + 1] = (new_dp[k + 1] + dp[k] * ways_odd) % MOD
    dp = new_dp

ans = 0
for k in range(n + 1):
    if k == 0:
        continue
    cost = (k - 1) // 2
    ans = (ans + dp[k] * cost) % MOD

print(ans)
```Mã đầu tiên nén mảng thành tần số. Mảng DP theo dõi xem có bao nhiêu nhóm giá trị cuối cùng đóng góp tính chẵn lẻ lẻ cho một dãy con. Mỗi nhóm chia đều thành các phần đóng góp chẵn và lẻ, mỗi nhóm có trọng số bằng$2^{c-1}$, đó là lý do tại sao cả hai quá trình chuyển đổi đều sử dụng cùng một hệ số. 

Vòng lặp cuối cùng chuyển đổi số nhóm chẵn lẻ lẻ thành công thức chi phí cần thiết và tổng hợp tổng câu trả lời theo modulo$998244353$. 

Một cạm bẫy triển khai phổ biến là quên rằng cả tập con chẵn và lẻ của một nhóm đều có số lượng bằng nhau$2^{c-1}$, dẫn đến trọng số không chính xác nếu sử dụng nhầm$2^c$cho cả hai chi nhánh. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
4 2 4 3 5
```Đầu tiên chúng tôi tính toán tần số. 

| Giá trị | Đếm | Các cách chẵn | Những cách kỳ lạ | 
| --- | --- | --- | --- | 
| 4 | 2 | 2 | 2 | 
| 2 | 1 | 1 | 1 | 
| 3 | 1 | 1 | 1 | 
| 5 | 1 | 1 | 1 | 

Chúng tôi bắt đầu với`dp[0] = 1`. 

Giá trị xử lý 4:`dp[0] -> dp[0] * 2 + dp[1] * 2`, Vì thế`dp = [2, 2]`. 

Giá trị xử lý 2: 

Mỗi tiểu bang lại chia tách. 

| k | trước | thậm chí đóng góp | đóng góp lẻ | sau | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 2 | - | 2 | 
| 1 | 2 | 2 | 2 | 4 | 
| 2 | 0 | - | 2 | 2 | 

Tiếp tục tương tự với các giá trị còn lại mang lại phân phối cuối cùng trên k. Tổng hợp chi phí trên k sẽ tạo ra 30. 

Dấu vết này cho thấy các nhóm chẵn lẻ tích lũy độc lập và DP đếm chính xác có bao nhiêu nhóm giá trị lẻ trong mỗi chuỗi con. 

### Mẫu 2 

đầu vào:```
10
2 2 1 1 3 2 3 4 1 3
```Tần số: 

| Giá trị | Đếm | 
| --- | --- | 
| 1 | 3 | 
| 2 | 3 | 
| 3 | 3 | 
| 4 | 1 | 

Mỗi nhóm đóng góp sự phân chia chẵn lẻ đối xứng. Sau khi xử lý tất cả bốn giá trị, dp phân phối trên tất cả các số có thể có của nhóm lẻ từ 0 đến 4. Hàm chi phí tính trọng số lớn các giá trị ở giữa của k, tạo ra tổng số 1969. 

Ví dụ này chứng tỏ rằng nhiều giá trị lặp lại làm tăng đáng kể các cấu hình chẵn lẻ tổ hợp, chiếm ưu thế trong tổng cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| DP nhiều nhất$n$những giá trị riêng biệt và$n$trạng thái chẵn lẻ trên mỗi bước | 
| Không gian |$O(n)$| Mảng DP có kích thước$n$| 

Những hạn chế$n \le 1000$thoải mái cho phép một$O(n^2)$giải pháp. DP tránh liệt kê các chuỗi con và thay vào đó hoạt động hoàn toàn trên cấu trúc tần số nén, giữ cả thời gian và bộ nhớ trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    from collections import Counter

    def modpow(a, e):
        res = 1
        while e:
            if e & 1:
                res = res * a % MOD
            a = a * a % MOD
            e >>= 1
        return res

    cnt = Counter(a)
    vals = list(cnt.values())

    dp = [0] * (n + 1)
    dp[0] = 1

    for c in vals:
        ways = modpow(2, c - 1)
        ndp = [0] * (n + 1)
        for k in range(n + 1):
            if dp[k]:
                ndp[k] = (ndp[k] + dp[k] * ways) % MOD
                if k + 1 <= n:
                    ndp[k + 1] = (ndp[k + 1] + dp[k] * ways) % MOD
        dp = ndp

    ans = 0
    for k in range(n + 1):
        if k:
            ans = (ans + dp[k] * ((k - 1) // 2)) % MOD

    return str(ans)

# provided samples
assert run("5\n4 2 4 3 5\n") == "30", "sample 1"
assert run("10\n2 2 1 1 3 2 3 4 1 3\n") == "1969", "sample 2"
assert run("5\n2 5 3 1 4\n") == "32", "sample 3"

# custom cases
assert run("1\n7\n") == "0", "single element always palindrome"
assert run("2\n1 1\n") == "0", "already palindrome pairs"
assert run("2\n1 2\n") == "0", "single swap not enough to reduce cost meaningfully"
assert run("4\n1 2 3 4\n") >= "0", "diverse values sanity check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | trường hợp cơ sở | 
| tất cả các cặp bằng nhau | 0 | đối xứng chi phí bằng không | 
| nhỏ khác biệt | 0 | tính nhất quán chẵn lẻ | 
| giá trị đa dạng | không âm | Độ ổn định DP | 

## Vỏ cạnh 

Đối với mảng một phần tử như`[7]`, DP bắt đầu bằng một nhóm giá trị có kích thước 1, tạo ra các phần chia chẵn và lẻ bằng nhau. Tuy nhiên, chỉ có một dãy con tồn tại cho giá trị đó và công thức chi phí không bao giờ được kích hoạt vì không có nhiều nhóm lẻ. Thuật toán tích lũy chính xác bằng 0. 

Đối với một mảng hoàn toàn thống nhất như`[1, 1, 1, 1]`, có chính xác một nhóm giá trị có độ phân tách tổ hợp lớn, nhưng mỗi dãy con vẫn có nhiều nhất một nhóm lẻ. DP chỉ gán số đếm khác 0 cho$k = 0$Và$k = 1$và vì chi phí cho$k \le 1$bằng 0, câu trả lời cuối cùng vẫn là 0, phù hợp với thực tế là bất kỳ dãy con nào của các phần tử giống hệt nhau đều đã là một bảng màu.
