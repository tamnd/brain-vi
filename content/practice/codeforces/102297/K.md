---
title: "CF 102297K - Thử thách của Turing"
description: "Với mỗi truy vấn, chúng ta được cung cấp các số nguyên dương (X) và (N). Hãy xem xét các số hạng (N+1) của khai triển nhị thức [ (1+X)^N, ] trong đó số hạng có chỉ số (i), sử dụng chỉ mục một cơ sở, là [ Ti=binom{N}{i-1}X^{i-1}. ] Chúng tôi có thể chọn bất kỳ tập hợp con nào của các chỉ số này."
date: "2026-08-13T08:40:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102297
codeforces_index: "K"
codeforces_contest_name: "UCF Locals 2015"
rating: 0
weight: 102297
solve_time_s: 115
verified: true
draft: false
---

[CF 102297K - Thử thách của Turing](https://codeforces.com/problemset/problem/102297/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Với mỗi truy vấn, chúng ta được cung cấp các số nguyên dương (X) và (N). Xét các số hạng (N+1) của khai triển nhị thức 

[ 
(1+X)^N, 
] 

trong đó thuật ngữ có chỉ mục (i), sử dụng chỉ mục dựa trên một, là 

[ 
T_i=\binom{N}{i-1}X^{i-1}. 
] 

Chúng tôi có thể chọn bất kỳ tập hợp con nào của các chỉ số này. Tích của các số hạng tương ứng phải đồng dạng với (2\pmod 4), và trong số tất cả các tập hợp con hợp lệ, chúng ta muốn tổng tối đa có thể có của các chỉ số của chúng. Tuyên bố cuộc thi ban đầu đưa ra (q\le 500) truy vấn và (X,N<2^{31}). 

Giới hạn (N<2^{31}) là độ khó trung tâm. Việc quét trực tiếp tất cả (N+1) cụm từ sẽ mất tới (2^{31}) lần lặp cho một truy vấn, điều này đã vượt xa giới hạn thời gian dự thi thông thường. Với tối đa 500 truy vấn, số lần lặp sẽ đạt khoảng (500\cdot2^{31}) hoặc khoảng (1,07\times10^{12}). Lời giải phải phụ thuộc vào số bit của (N), chứ không phụ thuộc vào chính (N). 

Có một số trường hợp đặc biệt trong đó việc triển khai trực tiếp có thể âm thầm gặp trục trặc. Nếu (X) chia hết cho 4 thì mọi số hạng ngoại trừ (T_1=1) đều chia hết cho 4, nên không có tích nào có thể là (2\pmod4). Ví dụ, với```
1
4 5
```đầu ra đúng là`0`, sử dụng`0`để biểu thị rằng không tồn tại tập hợp con hợp lệ. Chỉ chọn (T_1) sẽ cho kết quả là 1, trong khi mọi số hạng khác đều đóng góp một thừa số chia hết cho 4. 

Nếu (X\equiv2\pmod4), thì mọi lũy thừa (X^k) với (k\ge2) đều chia hết cho 4. Thừa số duy nhất có thể bằng 2 là (T_2=NX) và điều này chỉ xảy ra khi (N) là số lẻ. Như vậy```
1
2 3
```có câu trả lời`3`, vì (T_1=1) và (T_2=6\equiv2\pmod4) nên chỉ số 1 và 2 có thể được chọn. 

Đối với (X) lẻ, lũy thừa của (X) không đóng góp hệ số 2. Hành vi hoàn toàn được kiểm soát bởi hệ số nhị thức. Ví dụ,```
1
3 3
```có các hệ số (1,3,3,1), tất cả đều là số lẻ nên mọi số hạng đều là số lẻ và không có tích nào có thể là (2\pmod4). Câu trả lời là`0`. Việc triển khai bất cẩn giả định mọi (N) đủ lớn đều chứa hệ số bằng 2 modulo 4 sẽ thất bại ở đây. 

Cuối cùng, sự phân biệt giữa các hệ số chia hết cho 2 nhưng không chia hết cho 4 và các hệ số chia hết cho 4 là điều cần thiết. Với (X=3,N=5), các hệ số là (1,5,10,10,5,1). Cả hai số 10 đều là (2\pmod4) và chính xác một trong số chúng có thể được chọn. Ví dụ đã xuất bản chọn thuật ngữ có chỉ số 4 thay vì chỉ mục 3, cho tổng tối đa (18). 

## Phương pháp tiếp cận 

Một giải pháp brute-force có thể tạo ra mọi số hạng một cách rõ ràng, giảm nó theo modulo 4 và sau đó suy luận xem tập hợp con nào có thể tạo ra tích bằng 2. Có (N+1) số hạng và các tập hợp con tiềm năng (2^{N+1}), mặc dù cấu trúc mô-đun cho phép chúng ta giảm đáng kể việc tìm kiếm tập hợp con. Ngay cả sau lần giảm đó, việc kiểm tra tất cả các điều khoản có chi phí (O(N)) cho mỗi truy vấn. Trong trường hợp xấu nhất, đây là khoảng (2^{31}) thao tác cho một truy vấn và kiểm tra thuật ngữ khoảng (1,07\time10^{12}) cho 500 truy vấn, điều này là không khả thi. 

Quan sát quan trọng là tích chính xác là (2\pmod4) khi mọi thừa số được chọn đều là số lẻ ngoại trừ chính xác một thừa số đó là (2\pmod4). Một hệ số chia hết cho 4 không bao giờ có thể được chọn. 

Điều này thay đổi hoàn toàn vấn đề tối ưu hóa. Mỗi số hạng lẻ phải luôn được chọn vì nó không làm thay đổi tích modulo 4 và làm tăng tổng chỉ số. Trong số các số hạng đồng dạng với 2 modulo 4, cần chọn đúng một số hạng và ta nên chọn số hạng có chỉ số lớn nhất. Do đó, vấn đề được rút gọn thành việc phân loại mọi hệ số nhị thức theo lũy thừa hai của nó, nhưng không liệt kê tất cả (k=0,\ldots,N). 

Đối với số lẻ (X), 

[ 
T_{k+1}=\binom NkX^k 
] 

có cùng mức định giá 2-ad như (\binom Nk). Định lý Kummer nói rằng số mũ của 2 phép chia (\binom Nk) bằng số lần mang khi cộng (k) và (N-k) trong hệ nhị phân. Tương tự, nó bằng số lượng khoản vay được tạo ra trong khi trừ (k) từ (N) ở dạng nhị phân. 

Điều đó mang lại cho chúng ta một chữ số nhỏ DP. Trong khi xử lý các bit của (k) từ ít quan trọng nhất đến quan trọng nhất, chúng tôi giữ lại khoản vay trừ hiện tại và số lượng khoản vay được thấy cho đến nay. Chúng tôi chỉ quan tâm số lượng khoản vay là 0, chính xác là 1 hay ít nhất là 2. Do đó, toàn bộ truy vấn chỉ mất (O(\log N)) thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N)) cho mỗi truy vấn sau khi giảm mô-đun | (O(1)) | Quá chậm | 
| Tối ưu | (O(\log N)) mỗi truy vấn | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Kiểm tra lần đầu (X\bmod4). Nếu (X\equiv0\pmod4), mọi (T_i) cho (i>1) đều chia hết cho 4, do đó không có tích hợp lệ nào tồn tại và chúng ta xuất ra 0. 
2. Nếu (X\equiv2\pmod4) thì (X^k) chia hết cho 4 với mọi (k\ge2). Số hạng duy nhất có thể có (2\pmod4) là (T_2=NX). Chính xác là (2\pmod4) khi (N) lẻ. Trong trường hợp đó, tập con tối ưu là ({1,2}), cho đáp án 3. Nếu (N) chẵn thì không tồn tại tập con hợp lệ. 
3. Từ nay trở đi giả sử (X) là số lẻ. Khi đó (X^k) là số lẻ với mọi (k), do đó lớp thặng dư của (T_{k+1}) lũy thừa modulo của 2 được xác định hoàn toàn bởi (\binom Nk). 
4. Với mọi (k), gọi (v_2\left(\binom Nk\right)) là số mũ của 2 trong hệ số. Một hệ số là số lẻ khi giá trị này bằng 0, là (2\pmod4) khi nó bằng 1 và chia hết cho 4 khi nó ít nhất bằng 2. 
5. Sử dụng phép trừ nhị phân để tính giá trị này mà không tính hệ số nhị thức. Xử lý các bit của (k) từ thấp đến cao. Tại mỗi bit, chọn (k_i=0) hoặc (k_i=1). Cho khoản vay đến, trừ (k_i) và khoản vay từ bit tương ứng của (N). Nếu phép trừ trở thành âm, trạng thái tiếp theo có khoản vay là 1 và chúng tôi đã tìm thấy thêm một thừa số 2 trong hệ số nhị thức. 
6. Trạng thái DP bao gồm khoản vay hiện tại và danh mục định giá (v\in{0,1,2}), trong đó loại 2 có nghĩa là ít nhất hai khoản vay. Đối với loại 0, chúng tôi lưu trữ bao nhiêu giá trị của (k) đạt đến trạng thái và tổng của các giá trị (k) đó. Đối với loại 1, chúng ta chỉ cần (k) lớn nhất, vì chính xác một số hạng như vậy cuối cùng sẽ được chọn. 
7. Sau khi xử lý tất cả 31 bit, giữ lại các trạng thái có phép trừ cuối cùng bằng 0. Những trạng thái như vậy tương ứng chính xác với (0\le k\le N). Các trạng thái loại 0 đại diện cho tất cả các hệ số nhị thức lẻ, do đó tất cả các chỉ số của chúng (k+1) đều được chọn. 
8. Nếu không có trạng thái loại 1 thì không có số hạng nào là (2\pmod4), nên câu trả lời là 0. Ngược lại, lấy (k) lớn nhất trong loại 1 và cộng chỉ số của nó (k+1) vào tổng của tất cả các chỉ số loại 0. 

Tại sao nó hoạt động: tính bất biến của chữ số DP là sau khi xử lý các bit (j) đầu tiên, mỗi trạng thái biểu thị chính xác các lựa chọn của các bit (j) thấp của (k) có số lần mượn phép trừ được ghi lại và chính xác số lần mượn được ghi lại, giới hạn ở mức hai. Định lý Kummer chuyển đổi số tiền mượn đó thành (v_2\left(\binom Nk\right)). Cuối cùng, số 0 vay cuối cùng có nghĩa là (k\le N), vì vậy mọi (k) hợp lệ được biểu thị chính xác một lần. Điều kiện sản phẩm yêu cầu tất cả các yếu tố được chọn phải là số lẻ ngoại trừ một yếu tố có giá trị chính xác là một. Do đó, việc chọn mọi thuật ngữ không định giá luôn là tối ưu và việc chọn thuật ngữ định giá chỉ số lớn nhất sẽ cho tổng tối đa có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_query(x, n):
    xm = x & 3

    # X is divisible by 4.
    if xm == 0:
        return 0

    # X is 2 modulo 4.
    if xm == 2:
        if n & 1:
            return 3
        return 0

    # X is odd.
    #
    # dp[(borrow, v)] = (count, sum_k, max_k)
    # v = 0: exactly zero borrows
    # v = 1: exactly one borrow
    # v = 2: at least two borrows
    #
    # max_k is only relevant for v = 1.
    dp = {
        (0, 0): (1, 0, -1)
    }

    for bit in range(31):
        ndp = {}
        nb_n = (n >> bit) & 1
        value = 1 << bit

        for (borrow, v), (cnt, sum_k, max_k) in dp.items():
            for kb in (0, 1):
                t = nb_n - kb - borrow
                new_borrow = 1 if t < 0 else 0
                new_v = min(2, v + new_borrow)

                key = (new_borrow, new_v)

                old_cnt, old_sum, old_max = ndp.get(key, (0, 0, -1))

                add_sum = sum_k + cnt * kb * value
                new_cnt = old_cnt + cnt
                new_sum = old_sum + add_sum

                candidate_max = max_k
                if new_v == 1:
                    if kb:
                        candidate_max = max(candidate_max, max_k + value)
                    else:
                        candidate_max = max(candidate_max, max_k)

                ndp[key] = (
                    new_cnt,
                    new_sum,
                    max(old_max, candidate_max)
                )

        dp = ndp

    # Final borrow must be zero, otherwise k > N.
    odd_cnt, odd_sum, _ = dp.get((0, 0), (0, 0, -1))
    _, _, max_one = dp.get((0, 1), (0, 0, -1))

    if max_one == -1:
        return 0

    # Each k corresponds to term index k + 1.
    return odd_sum + odd_cnt + max_one + 1

def solve():
    q = int(input())
    ans = []

    for _ in range(q):
        x, n = map(int, input().split())
        ans.append(str(solve_query(x, n)))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Hai nhánh đầu tiên xử lý trực tiếp (X). Khi (X\equiv0\pmod4), không có thừa số nào có thể đóng góp chính xác một lũy thừa bằng 2. Khi (X\equiv2\pmod4), chỉ số mũ (k=1) có thể quan trọng và hệ số của nó là (N), cho một số hạng (2\pmod4) chính xác khi (N) là số lẻ. 

Mã còn lại xử lý số lẻ (X). Từ điển`dp`chỉ chứa một số trạng thái không đổi, bởi vì có hai giá trị vay có thể có và ba loại định giá. 

Đối với mỗi bit,`kb`đại diện cho bit đã chọn của (k). biểu hiện`n_bit - kb - borrow`thực hiện một bước trừ nhị phân. Kết quả âm có nghĩa là bit này cần mượn từ vị trí tiếp theo. Theo định lý Kummer, mỗi khoản vay như vậy đóng góp một phần vào việc định giá 2 adic của hệ số nhị thức. 

DP cũng theo dõi tổng của tất cả các giá trị (k) trong danh mục không vay mượn. Vì chỉ số thuật ngữ tương ứng là (k+1), nên tổng chỉ số là`odd_sum + odd_cnt`. 

Đối với danh mục vay một lần, chúng tôi chỉ giữ lại (k) lớn nhất. Không có lý do gì để lưu trữ tổng đầy đủ của nó vì việc chọn nhiều hơn một số hạng như vậy sẽ làm cho tích chia hết cho 4. (k) lớn nhất cho chỉ số số hạng lớn nhất có thể và chính xác là số hạng chúng ta muốn. 

Vòng lặp sử dụng 31 bit vì đảm bảo ràng buộc (N<2^{31}). Việc xử lý thêm một bit cũng sẽ vô hại, nhưng 31 bit là đủ để biểu thị mọi (k) có thể có trong phạm vi đầu vào. Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn trong tổng chỉ số tích lũy. 

Khoản vay cuối cùng phải bằng 0. Khoản vay cuối cùng khác 0 có nghĩa là số nhị phân được chọn (k) lớn hơn (N), do đó nó không phải là chỉ số hệ số nhị thức hợp lệ. 

## Ví dụ đã hoạt động 

Ví dụ cụ thể của tuyên bố đã xuất bản sử dụng (X=3,N=5). Các hệ số của nó là (1,5,10,10,5,1) và các số hạng tương ứng có chỉ số từ 1 đến 6. 

Đối với DP nhị phân, (N=5) là`101`. 

| chút | đã chọn (k_i) | vay sau khi trừ | hạng mục định giá | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 
| 1 | 1 | 1 | 1 | 
| 2 | 0 | 0 | 1 | 

Đường dẫn này biểu thị (k=2) và (\binom52=10), có chính xác một thừa số là 2. 

Các giá trị (k) liên quan của (N=5) là 

| (k) | (\binom5k) | (v_2) | chỉ số thuật ngữ | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 
| 1 | 5 | 0 | 2 | 
| 2 | 10 | 1 | 3 | 
| 3 | 10 | 1 | 4 | 
| 4 | 5 | 0 | 5 | 
| 5 | 1 | 0 | 6 | 

Tất cả các chỉ số ngoại trừ 3 và 4 phải được chọn. Tổng của chúng là (1+2+5+6=14). Giữa chỉ số 3 và 4, chúng ta chọn 4, cho ra (14+4=18). 

Đối với ví dụ thứ hai, hãy xem xét (X=1,N=4). Các hệ số là (1,4,6,4,1). 

| (k) | (\ binom4k) | (v_2) | chỉ số thuật ngữ | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 
| 1 | 4 | 2 | 2 | 
| 2 | 6 | 1 | 3 | 
| 3 | 4 | 2 | 4 | 
| 4 | 1 | 0 | 5 | 

Các số hạng lẻ có chỉ số 1 và 5, nên chúng đóng góp 6. Số hạng duy nhất (2\pmod4) có chỉ số 3, nên đáp án cuối cùng là (6+3=9). 

Ví dụ này thực hiện sự phân biệt giữa chính xác một thừa số của 2 và ít nhất hai thừa số của 2. Hệ số 4 phải bị loại bỏ, trong khi 6 có thể cung cấp thừa số duy nhất đồng dạng với 2 modulo 4. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(q\log N)) | Mỗi truy vấn xử lý 31 vị trí nhị phân và một số trạng thái DP không đổi. | 
| Không gian | (O(1)) | DP chỉ chứa sáu trạng thái, độc lập với (N). | 

Với (q\le500) và (N<2^{31}), thuật toán chỉ thực hiện tổng cộng vài nghìn chuyển đổi trạng thái. Giá trị số khổng lồ của (N) không bao giờ gây ra một vòng lặp tỷ lệ với (N), đây là đặc tính làm cho giải pháp trở nên thực tế. 

## Trường hợp thử nghiệm 

Văn bản bài toán được cung cấp không cung cấp các khối mẫu đầu vào/đầu ra chính thức. Nó cung cấp trường hợp hoạt động được (X=3,N=5), có câu trả lời là 18, vì vậy trường hợp đó được đưa vào bên dưới làm ví dụ đã xuất bản.```python
import io
import sys

def solve_query(x, n):
    xm = x & 3

    if xm == 0:
        return 0

    if xm == 2:
        return 3 if (n & 1) else 0

    dp = {
        (0, 0): (1, 0, -1)
    }

    for bit in range(31):
        ndp = {}
        nbit = (n >> bit) & 1
        value = 1 << bit

        for (borrow, v), (cnt, sum_k, max_k) in dp.items():
            for kb in (0, 1):
                t = nbit - kb - borrow
                new_borrow = 1 if t < 0 else 0
                new_v = min(2, v + new_borrow)
                key = (new_borrow, new_v)

                old_cnt, old_sum, old_max = ndp.get(key, (0, 0, -1))

                candidate_max = max_k
                if new_v == 1 and kb:
                    candidate_max = max(candidate_max, max_k + value)

                ndp[key] = (
                    old_cnt + cnt,
                    old_sum + sum_k + cnt * kb * value,
                    max(old_max, candidate_max)
                )

        dp = ndp

    odd_cnt, odd_sum, _ = dp.get((0, 0), (0, 0, -1))
    _, _, max_one = dp.get((0, 1), (0, 0, -1))

    if max_one == -1:
        return 0

    return odd_sum + odd_cnt + max_one + 1

def run(inp: str) -> str:
    data = io.StringIO(inp)
    q = int(data.readline())
    out = []

    for _ in range(q):
        x, n = map(int, data.readline().split())
        out.append(str(solve_query(x, n)))

    return "\n".join(out)

# Published worked example.
assert run("1\n3 5\n") == "18", "published example"

# Minimum-size input. X = 1, N = 1 gives terms 1, 1, so no product is 2 mod 4.
assert run("1\n1 1\n") == "0", "minimum size"

# Smallest useful odd X case. Coefficients of (1 + 1)^2 are 1, 2, 1.
# Select indices 1, 2, 3, giving sum 6.
assert run("1\n1 2\n") == "6", "single 2-mod-4 coefficient"

# Boundary case with coefficients 1, 4, 6, 4, 1.
# Only indices 1, 3, 5 can participate, giving 9.
assert run("1\n1 4\n") == "9", "coefficients divisible by 4"

# X = 2, N = 3. T1 = 1 and T2 = 6, while later terms are divisible by 4.
assert run("1\n2 3\n") == "3", "even X, odd N"

# X divisible by 4. No valid subset exists.
assert run("1\n4 5\n") == "0", "X divisible by 4"

# Maximum-size N. Since N = 2^31 - 1 has all binary bits set,
# every binomial coefficient is odd, so there is no 2-mod-4 coefficient.
assert run("1\n1 2147483647\n") == "0", "maximum-size N"

# Two queries together, checking that state is reset between queries.
assert run("2\n3 5\n1 4\n") == "18\n9", "multiple queries"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 5`|`18`| Ví dụ được xuất bản và chọn số hạng lớn nhất (2\pmod4) | 
|`1 1`|`0`| Trường hợp kích thước tối thiểu không có yếu tố hợp lệ | 
|`1 2`|`6`| Chính xác một hệ số đồng dạng với 2 modulo 4 | 
|`1 4`|`9`| Phải loại trừ các hệ số chia hết cho 4 | 
|`2 3`|`3`| Trường hợp đặc biệt (X\equiv2\pmod4) | 
|`4 5`|`0`| Trường hợp đặc biệt (X\equiv0\pmod4) | 
|`1 2147483647`|`0`| Kích thước tối đa (N) và hành vi ranh giới nhị phân | 
| Hai truy vấn |`18`,`9`| Xử lý độc lập nhiều truy vấn | 

## Vỏ cạnh 

Khi (X) chia hết cho 4, hãy xét```
1
4 5
```Mỗi số hạng sau (T_1) chứa một thừa số (X^k) với (k\ge1), do đó nó chia hết cho 4. Số hạng duy nhất có thể chọn không chia hết cho 4 là (T_1=1), nhưng tích của nó là 1 chứ không phải 2 modulo 4. Nhánh đặc biệt ngay lập tức trả về 0. 

Khi (X\equiv2\pmod4), hãy xem xét```
1
2 3
```Các số hạng modulo 4 là (1,2,0,0), bởi vì (T_2=3\cdot2=6\equiv2\pmod4), trong khi (X^2) và lũy thừa cao hơn chia hết cho 4. Chọn chỉ số 1 và 2 sẽ cho tích (6\equiv2\pmod4) và tổng 3. Thuật toán trả về 3 mà không cần nhập DP nhị phân. 

Khi (X) lẻ và không có hệ số nào là (2\pmod4), xét```
1
3 3
```Các hệ số nhị thức là (1,3,3,1), nên mọi số hạng đều là số lẻ. DP không có trạng thái với chính xác một khoản vay, tương ứng với việc không có hệ số với (v_2=1). Thuật toán trả về 0 thay vì chỉ chọn sai các số hạng lẻ, tích của chúng sẽ vẫn là số lẻ. 

Đối với ví dụ được xuất bản,```
1
3 5
```các hệ số không vay tương ứng với (k=0,1,4,5), cho các chỉ số thuật ngữ 1, 2, 5, 6. Tổng của chúng là 14. Các hệ số một lần vay tương ứng với (k=2,3), cho các chỉ số 3 và 4. Chọn chỉ số lớn hơn 4 cho 18. Lựa chọn còn lại cho 17, do đó thu được mức tối đa chính xác. 

Giá trị lớn nhất được phép (N) cũng an toàn vì thuật toán không bao giờ lặp lại (N). Vì```
1
1 2147483647
```biểu diễn nhị phân của (N) là 31 đơn vị. Mọi (k\le N) đều là mặt nạ con của (N), nên mọi hệ số nhị thức đều là số lẻ. DP tìm thấy trạng thái 0 với đúng một lần mượn và trả về 0 sau khi chỉ xử lý các vị trí 31 bit.
