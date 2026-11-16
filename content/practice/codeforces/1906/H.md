---
title: "CF 1906H - Đôi Bạn Sinh Đôi"
description: "Chúng ta có hai chuỗi, $A$ có độ dài $N$ và $B$ có độ dài $M$ với $N le M$, đại diện cho tên của hai cặp song sinh. Chúng tôi muốn tạo biệt danh $A'$ và $B'$ cho họ."
date: "2026-06-09T01:23:27+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp"]
categories: ["algorithms"]
codeforces_contest: 1906
codeforces_index: "H"
codeforces_contest_name: "2023-2024 ICPC, Asia Jakarta Regional Contest (Online Mirror, Unrated, ICPC Rules, Teams Preferred)"
rating: 2200
weight: 1906
solve_time_s: 71
verified: true
draft: false
---

[CF 1906H - Đôi bạn song sinh](https://codeforces.com/problemset/problem/1906/H) 

**Xếp hạng:** 2200 
**Tags:** tổ hợp, dp 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai chuỗi,$A$chiều dài$N$Và$B$chiều dài$M$với$N \le M$, đại diện cho tên của hai cặp song sinh. Chúng tôi muốn tạo biệt danh$A'$Và$B'$cho họ.$A'$có thể là bất kỳ hoán vị nào của$A$, trong khi$B'$thu được bằng cách thực hiện bất kỳ hoán vị nào của$B$và xóa chính xác$M-N$các ký tự, sao cho nó có cùng độ dài$N$BẰNG$A'$. 

Biệt hiệu phải đáp ứng điều kiện cho mỗi ký tự: cho mỗi vị trí$i$từ$1$ĐẾN$N$, cái$i$-ký tự thứ của$B'$phải bằng hoặc bằng$i$-ký tự thứ của$A'$hoặc chữ cái tiếp theo theo thứ tự bảng chữ cái. Mục tiêu là đếm tất cả những gì có thể hợp lệ$(A', B')$cặp modulo$998244353$. 

Các kích thước đầu vào$N, M \le 200,000$có nghĩa là một$O(NM)$giải pháp là không khả thi. Chúng ta phải hướng tới$O(N + M)$hoặc$O(M \log M)$sự phức tạp. Các cách tiếp cận ngây thơ tạo ra tất cả các hoán vị hoặc tập hợp con sẽ bùng nổ về mặt tổ hợp, như$N!$hoặc$M \choose N$trở nên lớn về mặt thiên văn rất nhanh. 

Các trường hợp cạnh không rõ ràng bao gồm: nếu tất cả các ký tự của$A$giống nhau thì mọi giá trị đều hợp lệ$B'$vẫn có thể có nhiều khả năng nếu$B$chứa chữ cái tiếp theo trong bảng chữ cái. Một trường hợp cạnh khác là khi$B$chứa các chữ cái không có trong$A$chứ không phải chữ cái tiếp theo - những chữ cái này phải được loại trừ cẩn thận khỏi$B'$các lựa chọn, nếu không chúng sẽ làm mất hiệu lực của cặp. Ví dụ,$A = "AAA"$,$B = "AAB"$, có hiệu lực$B'$bao gồm "AAA" và "AAB", nhưng lệnh bỏ qua việc đếm đơn giản có thể đếm quá mức không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xem xét tất cả các hoán vị của$A$và tất cả$N$-các dãy con có độ dài của hoán vị của$B$, kiểm tra điều kiện mỗi ký tự cho mỗi cặp. Về mặt hình thức, đối với$A'$có$N! / \prod_{c} f_A(c)!$hoán vị riêng biệt (hoán vị nhiều tập hợp) và cho mỗi hoán vị$A'$chúng tôi sẽ xem xét tất cả$M \choose N$hậu quả của$B$và của họ$N!$hoán vị. Ngay cả đối với nhỏ$N = 10$, điều này kết thúc$10^7$khả năng. Điều này đúng về nguyên tắc nhưng không thực tế. 

Quan sát quan trọng là vấn đề chỉ phụ thuộc vào **nhiều bộ** chữ cái chứ không phụ thuộc vào thứ tự của chúng, bởi vì hoán vị của$A$Và$B$có thể được tính bằng cách sử dụng số giai thừa và số tổ hợp. Vì$B'$, chúng ta chỉ cần đếm xem có bao nhiêu cách chọn được các chữ cái phù hợp$A'$hoặc chữ cái tiếp theo trong bảng chữ cái. Điều này biến vấn đề thành một DP tổ hợp về số lượng chữ cái thay vì vị trí. 

Cụ thể, chúng ta có thể đại diện$A$Và$B$như các mảng tần số trên bảng chữ cái. Với mỗi chữ cái từ 'A' đến 'Z', chúng ta biết có bao nhiêu lần xuất hiện trong$A'$và có bao nhiêu lựa chọn tồn tại trong$B$để khớp hoặc tăng từng chữ cái. Sử dụng giai thừa và nghịch đảo mô-đun, chúng ta có thể tính toán số lượng hoán vị hợp lệ một cách hiệu quả. DP lựa chọn hiệu quả số lượng chữ cái cho$B'$phù hợp với$A'$, xem xét "khớp hoặc tiếp theo" ở mỗi chỉ mục bảng chữ cái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N! \cdot M! / (M-N)!)$|$O(N + M)$| Quá chậm | 
| DP kết hợp / Đếm tần số |$O(N + M + 26^2)$|$O(26)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi chuỗi$A$Và$B$vào mảng tần số`freqA`Và`freqB`có độ dài 26 đại diện cho số lượng mỗi chữ cái viết hoa. 
2. Tính toán trước các giai thừa và nghịch đảo mô-đun lên đến$M$để xử lý hoán vị nhiều tập hợp một cách hiệu quả. 
3. Khởi tạo`result = 1`. Điều này sẽ tích lũy số cách để gán chữ cái cho$B'$đưa ra một tập hợp cụ thể của$A'$. 
4. Lặp lại từng chữ cái`c`từ 'A' đến 'Z'. Đối với mỗi`c`, cho phép`countA = freqA[c]`. Chúng ta cần gán`countA`chữ cái trong$B'$đó là một trong hai`c`hoặc`c+1`. 
5. Hãy để`available_c = freqB[c]`Và`available_next = freqB[c+1]`(nếu như`c+1`tồn tại). Số cách chọn chính xác`countA`các chữ cái từ hai nhóm này là tổng kết`k`từ`max(0, countA - available_next)`ĐẾN`min(countA, available_c)`của`C(available_c, k) * C(available_next, countA - k)`. 
6. Nhân`result`theo số cách viết chữ cái này modulo $998244353`. 
7. Sau khi xử lý xong tất cả các chữ cái, hãy nhân`result`bằng số lượng hoán vị nhiều tập cho$A$:$N! / \prod_{c} freqA[c]!$modulo$998244353$. Điều này giải thích cho tất cả sự khác biệt$A'$. 
8. Đầu ra`result`. 

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi đảm bảo rằng mọi chữ cái trong$A'$có một chữ cái tương ứng trong$B'$phù hợp với nó hoặc là chữ cái tiếp theo. Bằng cách tính toán số lượng thay vì hoán vị, chúng tôi bao gồm tất cả các khả năng sắp xếp mà không cần tính hai lần. Việc điều chỉnh hoán vị nhiều tập ở cuối tính đến tất cả sự sắp xếp lại của$A'$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
MAX = 200_000 + 10

# Precompute factorials and inverses
fact = [1] * MAX
inv_fact = [1] * MAX

for i in range(1, MAX):
    fact[i] = fact[i-1] * i % MOD

inv_fact[MAX-1] = pow(fact[MAX-1], MOD-2, MOD)
for i in range(MAX-2, -1, -1):
    inv_fact[i] = inv_fact[i+1] * (i+1) % MOD

def comb(n, k):
    if k < 0 or k > n:
        return 0
    return fact[n] * inv_fact[k] % MOD * inv_fact[n-k] % MOD

def solve():
    N, M = map(int, input().split())
    A = input().strip()
    B = input().strip()

    freqA = [0]*26
    freqB = [0]*26

    for c in A:
        freqA[ord(c)-65] += 1
    for c in B:
        freqB[ord(c)-65] += 1

    result = 1
    for i in range(26):
        countA = freqA[i]
        if countA == 0:
            continue
        available_c = freqB[i]
        available_next = freqB[i+1] if i+1 < 26 else 0
        ways = 0
        for k in range(max(0, countA - available_next), min(countA, available_c)+1):
            ways = (ways + comb(available_c, k) * comb(available_next, countA - k)) % MOD
        result = result * ways % MOD

    # multiply by number of distinct permutations of A
    denom = 1
    for cnt in freqA:
        denom = denom * inv_fact[cnt] % MOD
    result = result * fact[N] % MOD * denom % MOD
    print(result)

solve()
```Đầu tiên, mã này thiết lập các giai thừa và nghịch đảo mô-đun để xử lý số lượng tổ hợp một cách hiệu quả. Mảng tần số đơn giản hóa logic khớp và cho phép tích lũy giống như DP trên các chữ cái. Vòng lặp trên các chữ cái tính toán các cách hợp lệ để chọn các chữ cái cho$B'$độc lập, điều này hợp lệ vì không có hai chữ cái nào cạnh tranh cho cùng một vị trí trong$B'$. Cuối cùng, chúng tôi điều chỉnh các hoán vị của$A$chính nó. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
3 4
AMA
ANAB
```| Thư | tần số | tần sốB | Lựa chọn | Giải thích | 
| --- | --- | --- | --- | --- | 
| A | 2 | 2 | C(2,1)*C(1,1) + C(2,2)*C(1,0) = 3 | 2 chữ A ở A, các tùy chọn ở B: hai chữ A, một chữ B (chữ tiếp theo) | 
| M | 1 | 1 | C(1,1)*C(0,0) = 1 | Một M, một M ở B | 
| Khác | 0 | ... | 1 | Không có chữ cái để chọn | 

Hoán vị của A:`AMA`có 3! / (2! _1!) = 3. Nhân với các lựa chọn = 3_3 = 9. 

**Mẫu 2** 

đầu vào:```
5 5
BINUS
BINUS
```Tất cả các chữ cái đều khớp chính xác, chỉ có một cách chọn từng chữ cái trong B'. Hoán vị của A: 5! = 120. Kết quả = 120. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + M + 26^2) | Tần số đếm là O(N+M), lặp qua các chữ cái và tính tổng |
