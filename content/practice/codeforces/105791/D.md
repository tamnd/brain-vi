---
title: "CF 105791D - Phi Tiêu"
description: "Mỗi bài kiểm tra mô tả một trận chiến có cấu trúc đơn giản: có n đợt kẻ thù và đợt thứ i chứa đúng i quả bóng bay. Mỗi quả bóng bay trong một làn sóng có cùng mức sức mạnh k, và chi phí để phá hủy một quả bóng bay sẽ được nâng lên lũy thừa k."
date: "2026-06-21T14:24:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105791
codeforces_index: "D"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2025"
rating: 0
weight: 105791
solve_time_s: 58
verified: true
draft: false
---

[CF 105791D - Phi tiêu](https://codeforces.com/problemset/problem/105791/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi bài kiểm tra mô tả một trận chiến có cấu trúc đơn giản: có n đợt kẻ thù và đợt thứ i chứa đúng i quả bóng bay. Mỗi quả bóng bay trong một làn sóng có cùng mức sức mạnh k, và chi phí để phá hủy một quả bóng bay sẽ được nâng lên lũy thừa k. Vì vậy, tổng số phi tiêu cần thiết cho cả cấp độ là tổng của i^k trên tất cả i từ 1 đến n. 

Nói cách khác, nhiệm vụ là tính tổng lũy ​​thừa cổ điển có dạng S_k(n) = 1^k + 2^k + … + n^k theo một mô đun lớn. 

Khó khăn không phải là định nghĩa mà là quy mô. Số số hạng n có thể lớn tới 10^9, vì vậy việc lặp lại tất cả i là không thể. Số mũ k nhiều nhất là 1000, điều này gợi ý cấu trúc đa thức theo k chứ không phải theo n. 

Việc triển khai đơn giản sẽ cố gắng lặp từ 1 đến n cho mỗi trường hợp thử nghiệm và tích lũy i^k. Điều này ngay lập tức thất bại ngay cả đối với một thử nghiệm duy nhất với n = 10^9 vì nó sẽ yêu cầu một tỷ lũy thừa mô-đun. 

Một lỗi phổ biến khác là khả năng tính toán trước cho tất cả i cho đến n. Điều này cũng phá vỡ các hạn chế về bộ nhớ và thời gian vì n quá lớn để thực hiện. 

Một trường hợp phức tạp hơn là khi k = 0 hoặc k = 1. Với k = 0, mọi số hạng là 1 và câu trả lời là n, và với k = 1, câu trả lời là n(n+1)/2. Bất kỳ công thức tổng quát nào cũng phải suy biến chính xác cho những trường hợp này mà không mất ổn định về số. 

## Phương pháp tiếp cận 

Phương pháp vũ phu đánh giá trực tiếp từng số hạng i^k và tính tổng chúng. Điều này đúng về mặt toán học nhưng không khả thi về mặt tính toán. Đối với mỗi trường hợp thử nghiệm, nó thực hiện phép tính lũy thừa O(n) và mỗi phép tính lũy thừa có giá O(log k), dẫn đến khoảng 10^9 phép tính cho mỗi thử nghiệm trong trường hợp xấu nhất, vượt xa mọi giới hạn thời gian. 

Quan sát quan trọng là S_k(n) không phải là hàm tùy ý của n. Nó là một đa thức ở n bậc k+1. Đây là một thực tế nổi tiếng về tổng lũy ​​thừa và có thể được suy ra bằng cách sử dụng sai phân hữu hạn hoặc khai triển nhị thức liên quan đến số Stirling. 

Một khi chúng ta chấp nhận cấu trúc đó, bài toán sẽ trở thành bài toán đánh giá một đa thức một cách hiệu quả tại một điểm lớn n. Thay vì lặp lại i, chúng ta viết lại i^k bằng cách sử dụng số Stirling loại hai: 

i^k = tổng trên j từ 0 đến k của S(k, j) * j! * C(i,j) 

Bây giờ tính tổng i từ 1 đến n, chúng ta có thể hoán đổi phép tính tổng: 

S_k(n) = tổng trên j của S(k, j) * j! * tổng trên i của C(i, j) 

Tổng bên trong có dạng đóng rõ ràng: 

tổng_{i=1..n} C(i, j) = C(n+1, j+1) 

Điều này làm giảm toàn bộ vấn đề về việc tính số Stirling cho k cố định và đánh giá một vài hệ số nhị thức tại n. 

Vì vậy, thay vì lặp trên n, chúng ta chỉ lặp trên k, tối đa là 1000. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · log k) | O(1) | Quá chậm | 
| Stirling + tổ hợp | O(k^2) mỗi lần kiểm tra | O(k^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc n và k cho một trường hợp thử nghiệm. Mục tiêu là tính S_k(n), tổng của i^k trên i từ 1 đến n. 
2. Tính trước số Stirling loại thứ hai S(k, j) cho k đã cho bằng cách sử dụng phép truy toán S(k, j) = j * S(k-1, j) + S(k-1, j-1). Điều này xây dựng các hệ số chuyển đổi sức mạnh thành sự kết hợp. 
3. Tính trước các giai thừa lên tới k vì mỗi số hạng cũng bao gồm một số nhân j!. Điều này cho phép truy cập liên tục trong quá trình đánh giá. 
4. Với mỗi j từ 0 đến k, hãy tính hệ số nhị thức C(n+1, j+1). Vì n lớn, hãy tính nó dưới dạng tích giảm dần (n+1)(n)…(n-j+1) chia cho (j+1)! theo số học modulo. 
5. Với mỗi j, nhân S(k, j), j!, và C(n+1, j+1), rồi tích lũy kết quả vào câu trả lời. 
6. In tổng cuối cùng theo modulo 10^9 + 7.

Tại sao nó hoạt động: phép biến đổi thay thế từng đơn thức i^k bằng tổ hợp tuyến tính của các hàm cơ sở nhị thức C(i, j). Tổng trên i của các hàm cơ sở này quy về một số hạng nhị thức C(n+1, j+1), do đó toàn bộ tổng trở thành tổ hợp tuyến tính hữu hạn của các dạng đóng chứ không phải là tổng tiền tố dài. Tính đúng đắn xuất phát từ sự đồng nhất thức rằng cả hai cách biểu diễn đều đồng ý với mọi số nguyên i, do đó tổng của chúng trên bất kỳ tiền tố nào cũng phải bằng nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def build_stirling(k):
    S = [[0] * (k + 1) for _ in range(k + 1)]
    S[0][0] = 1
    for i in range(1, k + 1):
        for j in range(1, i + 1):
            S[i][j] = (S[i - 1][j - 1] + j * S[i - 1][j]) % MOD
    return S

def solve():
    t = int(input())
    tests = []
    max_k = 0
    for _ in range(t):
        n, k = map(int, input().split())
        tests.append((n, k))
        max_k = max(max_k, k)

    fact = [1] * (max_k + 2)
    invfact = [1] * (max_k + 2)
    for i in range(1, max_k + 2):
        fact[i] = fact[i - 1] * i % MOD

    invfact[max_k + 1] = pow(fact[max_k + 1], MOD - 2, MOD)
    for i in range(max_k, -1, -1):
        invfact[i] = invfact[i + 1] * (i + 1) % MOD

    stir = build_stirling(max_k)

    for n, k in tests:
        if k == 0:
            print(n % MOD)
            continue

        ans = 0
        # compute C(n+1, j+1) using falling product
        for j in range(0, k + 1):
            # compute numerator (n+1)P(j+1)
            num = 1
            x = n + 1
            for t2 in range(j + 1):
                num = num * (x - t2) % MOD

            comb = num * invfact[j + 1] % MOD

            ans = (ans + stir[k][j] * fact[j] % MOD * comb) % MOD

        print(ans % MOD)

if __name__ == "__main__":
    solve()
```Giải pháp được cấu trúc xung quanh việc tách tiền xử lý khỏi tính toán trên mỗi lần kiểm tra. Số Stirling được tính một lần cho đến k tối đa trong tất cả các thử nghiệm, giúp tránh phải tính toán lại. Giai thừa và giai thừa nghịch đảo cũng được tính toán trước một lần để đánh giá nhị thức nhanh. 

Bên trong mỗi thử nghiệm, số hạng nhị thức C(n+1, j+1) được tính bằng cách sử dụng tích giảm dần thay vì giai thừa của n, vì n quá lớn đối với các kết hợp dựa trên giai thừa trực tiếp. Điều này tránh tràn và giữ tất cả các mô-đun số học. 

Một sai lầm phổ biến là quên rằng tổng bắt đầu ở i = 1, đó là lý do tại sao đẳng thức tạo ra C(n+1, j+1) thay vì C(n, j+1). 

## Ví dụ đã hoạt động 

Xét n = 3, k = 2. Đáp án đúng là 1^2 + 2^2 + 3^2 = 14. 

Chúng ta tính số Stirling cho k = 2: S(2,1) = 1, S(2,2) = 1. 

| j | S(k,j) | j! | C(n+1,j+1) | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | C(4,1)=4 | 0 | 
| 1 | 1 | 1 | C(4,2)=6 | 6 | 
| 2 | 1 | 2 | C(4,3)=4 | 8 | 

Tổng hợp cho 14. 

Điều này xác nhận rằng phép biến đổi phù hợp với đánh giá trực tiếp và cấu trúc bậc cao hơn sẽ sụp đổ một cách chính xác thành các thuật ngữ nhị thức. 

Bây giờ hãy xem xét n = 5, k = 1. Kết quả mong đợi là 1 + 2 + 3 + 4 + 5 = 15. 

| j | S(1,j) | j! | C(6,j+1) | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 6 | 0 | 
| 1 | 1 | 1 | 15 | 15 | 

Điều này xác nhận rằng công thức rút gọn chính xác về dạng số tam giác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k^2) mỗi lần kiểm tra | Stirling DP chiếm ưu thế, đánh giá nhị thức là O(k) | 
| Không gian | O(k^2) | bảo quản bàn Stirling | 

Các ràng buộc giữ k tối đa là 1000, do đó, ngay cả quá trình tiền xử lý bậc hai cũng phù hợp một cách thoải mái. Số lượng thử nghiệm đủ nhỏ để công việc O(k^2) lặp lại vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    def build_stirling(k):
        S = [[0] * (k + 1) for _ in range(k + 1)]
        S[0][0] = 1
        for i in range(1, k + 1):
            for j in range(1, i + 1):
                S[i][j] = (S[i - 1][j - 1] + j * S[i - 1][j]) % MOD
        return S

    t = int(input())
    tests = []
    max_k = 0
    for _ in range(t):
        n, k = map(int, input().split())
        tests.append((n, k))
        max_k = max(max_k, k)

    fact = [1] * (max_k + 2)
    invfact = [1] * (max_k + 2)
    for i in range(1, max_k + 2):
        fact[i] = fact[i - 1] * i % MOD
    invfact[max_k + 1] = pow(fact[max_k + 1], MOD - 2, MOD)
    for i in range(max_k, -1, -1):
        invfact[i] = invfact[i + 1] * (i + 1) % MOD

    stir = build_stirling(max_k)

    out = []
    for n, k in tests:
        if k == 0:
            out.append(str(n % MOD))
            continue
        ans = 0
        for j in range(k + 1):
            num = 1
            x = n + 1
            for t2 in range(j + 1):
                num = num * (x - t2) % MOD
            comb = num * invfact[j + 1] % MOD
            ans = (ans + stir[k][j] * fact[j] % MOD * comb) % MOD
        out.append(str(ans % MOD))

    return "\n".join(out)

# provided samples (placeholders since original sample output not fully visible)
# basic sanity checks
assert run("1\n3 2\n") == "14"
assert run("1\n5 1\n") == "15"
assert run("1\n10 0\n") == "10"

# custom cases
assert run("1\n1 100\n") == "1", "n=1 edge"
assert run("1\n10 2\n") == str((1*1 + 2*2 + 3*3 + 4*4 + 5*5 + 6*6 + 7*7 + 8*8 + 9*9 + 10*10) % MOD), "square sum"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1 100 | 1 | k lớn, n tối thiểu | 
| 1\n10 2 | 385 | tính đúng đắn của tổng bậc hai | 
| 1\n5 1 | 15 | trường hợp tuyến tính | 

## Vỏ cạnh 

Khi k = 0, mọi số hạng i^0 bằng 1, do đó tổng phải bằng n. Trong thuật toán, điều này bỏ qua hoàn toàn việc khai triển Stirling và trực tiếp trả về n modulo MOD, khớp với danh tính C(n+1,1) = n+1 nhưng được dịch chuyển bằng cách lập chỉ mục chính xác của tổng bắt đầu từ 1. 

Khi n = 1, tất cả cấu trúc cao hơn sẽ sụp đổ vì tổng chứa một số hạng duy nhất. Đánh giá nhị thức tạo ra C(2, j+1), khác 0 chỉ với j = 0, đảm bảo kết quả luôn là 1^k = 1 bất kể k. 

Khi k lớn so với n, nhiều số hạng Stirling cao hơn vẫn xuất hiện trong tính toán, nhưng hệ số nhị thức C(n+1, j+1) biến mất một cách tự nhiên đối với j > n, do đó công thức vẫn ổn định và tránh sự đóng góp không cần thiết từ các chỉ số lớn.
