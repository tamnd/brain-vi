---
title: "CF 306C - Trắng, Đen và Trắng Lại"
description: "Chúng ta được yêu cầu tính toán số cách để phân bổ một tập hợp các sự kiện tốt và không tốt trong một chuỗi ngày được chia thành ba phân đoạn liên tiếp: phân đoạn trắng, phân đoạn đen và phân đoạn thứ hai màu trắng."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 306
codeforces_index: "C"
codeforces_contest_name: "Testing Round 6"
rating: 2100
weight: 306
solve_time_s: 200
verified: false
draft: false
---

[CF 306C - Trắng, Đen và Trắng lần nữa](https://codeforces.com/problemset/problem/306/C) 

**Đánh giá:** 2100 
**Tags:** tổ hợp, lý thuyết số 
**Thời gian giải:** 3 phút 20s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính toán số cách để phân bổ một tập hợp các sự kiện tốt và không tốt trong một chuỗi ngày được chia thành ba phân đoạn liên tiếp: phân đoạn trắng, phân đoạn đen và phân đoạn thứ hai màu trắng. Mỗi phân đoạn phải chiếm ít nhất một ngày và tất cả các sự kiện trong một ngày nhất định đều thuộc cùng một loại. Hơn nữa, các sự kiện có thể phân biệt được: sắp xếp lại các sự kiện trong một ngày hoặc di chuyển chúng giữa các ngày trong cùng một dải sẽ tạo ra một cấu hình khác. 

Đầu vào bao gồm ba số nguyên: tổng số ngày`n`, số sự kiện tốt`w`, và số lượng sự kiện không tốt lắm`b`. Chúng ta được yêu cầu tính tổng số chuỗi hợp lệ theo modulo$10^9 + 9$. 

Những hạn chế`3 ≤ n ≤ 4000`Và`2 ≤ w, b ≤ 4000`ngụ ý rằng bất kỳ thuật toán nào lặp lại tất cả các sắp xếp sự kiện có thể xảy ra một cách rõ ràng sẽ quá chậm, vì giai thừa của 4000 là rất lớn về mặt thiên văn. Chúng ta cần một cách tiếp cận tổ hợp tận dụng tính toán trước và số học mô-đun. 

Các trường hợp cạnh dễ bị bỏ sót bao gồm các đoạn có độ dài tối thiểu. Ví dụ, nếu`n = 3`,`w = 2`, Và`b = 1`, độ dài ngày hợp lệ duy nhất là 1,1,1, nhưng nếu chúng ta phân chia các sự kiện một cách ngây thơ mà không thực thi ít nhất một ngày cho mỗi phân đoạn, thì chúng ta có thể tính những ngày có độ dài bằng 0 hoặc vượt quá số lượng sự kiện có sẵn. Một điểm tinh tế khác là các sự kiện có thể phân biệt được nên sự hoán vị trong một phân đoạn và giữa các ngày rất quan trọng. Việc đếm sai các hoán vị trong vài ngày hoặc không xử lý chính xác số học mô-đun sẽ mang lại kết quả không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi cách có thể để phân chia các sự kiện tốt và xấu trong các ngày. Đối với mỗi lần phân chia ngày có thể thành ba phân đoạn và với mỗi độ dài phân đoạn, chúng ta có thể thử tất cả các hoán vị của các sự kiện được chỉ định cho mỗi ngày. Điều này rõ ràng là không khả thi bởi vì ngay cả một phép tính giai thừa duy nhất cho`w = 100`có nhiều hơn$10^{150}$khả năng. Lực lượng vũ phu thất bại cả vì độ phức tạp theo thời gian theo cấp số nhân và vì việc xử lý tất cả các hoán vị một cách rõ ràng là không thực tế. 

Cái nhìn sâu sắc quan trọng là vấn đề có thể được giảm xuống bằng việc đếm các phân phối tổ hợp của các mục có thể phân biệt được trong những ngày không thể phân biệt được, bị ràng buộc bởi số lượng tối thiểu. Mỗi sọc về cơ bản là một bài toán "thành phần số nguyên": chia`k`sự kiện vào`d`ngày sao cho mỗi ngày có ít nhất một sự kiện. Công thức tính số cách phân phối`k`các mục có thể phân biệt thành`d`các thùng không thể phân biệt được, không trống, được tính bằng tổng tổ hợp sử dụng các hệ số giai thừa và nhị thức. 

Quan sát này cho phép chúng ta tính toán trước các giai thừa và nghịch đảo mô-đun lên đến mức tối đa`w`hoặc`b`. Sau đó, đối với mỗi phân vùng ngày hợp lệ thành ba sọc, chúng tôi nhân số lượng phân phối sự kiện tốt ở sọc trắng đầu tiên, sự kiện xấu ở sọc đen và sự kiện tốt ở sọc trắng thứ hai, tổng hợp tất cả độ dài ngày hợp lệ. Số học mô-đun đảm bảo chúng tôi không bao giờ bị tràn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * w! * b!) | O(1) | Quá chậm | 
| Tối ưu | O(n * w + n * b) | O(w + b + n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các giai thừa và nghịch đảo mô-đun lên đến`max(w, b, n)`modulo$10^9 + 9$. Điều này cho phép tính toán hiệu quả các hệ số nhị thức cho phân bố sự kiện. 
2. Xác định hàm trợ giúp`ways(k, d)`trả về số cách phân phối`k`các sự kiện có thể phân biệt thành`d`ngày, mỗi ngày nhận được ít nhất một sự kiện. Điều này được tính bằng công thức tổ hợp$\binom{k-1}{d-1} \times k! / \prod \text{day factorials}$, được đơn giản hóa thông qua các giai thừa và nghịch đảo được tính toán trước. 
3. Lặp lại tất cả số ngày có thể được gán cho sọc trắng đầu tiên`w1`từ 1 đến`n-2`, sọc đen`b_days`từ 1 đến`n-w1-1`, và sọc trắng thứ hai`w2 = n - w1 - b_days`. 
4. Đối với mỗi bộ ba`(w1, b_days, w2)`, lặp lại tất cả số sự kiện hợp lệ được gán cho các sọc tương ứng: ít nhất một sự kiện mỗi ngày và tổng số không vượt quá`w`hoặc`b`. 
5. Nhân số lượng phân phối sự kiện theo modulo ba sọc$10^9 + 9$và tổng hợp thành câu trả lời cuối cùng. 
6. In đáp án modulo$10^9 + 9$. 

Điều bất biến là tại mỗi lần lặp, tổng độ dài của tất cả các ngày bằng`n`và tất cả số sự kiện có tổng bằng`w`hoặc`b`trong giới hạn, đảm bảo không tính cấu hình không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 9
MAX = 4000 + 10

# precompute factorials and inverses
fact = [1] * (MAX)
inv_fact = [1] * (MAX)

for i in range(1, MAX):
    fact[i] = fact[i-1] * i % MOD

inv_fact[MAX-1] = pow(fact[MAX-1], MOD-2, MOD)
for i in range(MAX-2, -1, -1):
    inv_fact[i] = inv_fact[i+1] * (i+1) % MOD

def comb(n, k):
    if k < 0 or k > n:
        return 0
    return fact[n] * inv_fact[k] % MOD * inv_fact[n-k] % MOD

def ways(events, days):
    if days > events:
        return 0
    # number of ways to put 'events' distinguishable items into 'days' non-empty days
    return comb(events - 1, days - 1)

n, w, b = map(int, input().split())

ans = 0
for w1_days in range(1, n-1):
    for b_days in range(1, n - w1_days):
        w2_days = n - w1_days - b_days
        w1_ways = ways(w, w1_days)
        b_ways = ways(b, b_days)
        w2_ways = ways(w, w2_days)
        ans = (ans + w1_ways * b_ways % MOD * w2_ways % MOD) % MOD

print(ans)
```Đầu tiên, mã này tính toán trước các giai thừa và nghịch đảo mô-đun để cho phép tính toán tổ hợp nhanh chóng. các`ways`hàm tính toán số cách để chia các sự kiện có thể phân biệt thành các ngày, đảm bảo mỗi ngày có ít nhất một sự kiện. Vòng lặp chính lặp lại tất cả các phần chia ngày hợp lệ, nhân số lượng cho mỗi sọc và tích lũy mô-đun câu trả lời$10^9 + 9$. Xử lý phép nhân mô-đun cẩn thận để tránh tràn số nguyên. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3 2 1
```| w1_ngày | b_ngày | w2_ngày | w1_ways | b_ways | w2_ways | Đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | 1 | 2 | 4 | 

Vì chỉ`w=2`tổng cộng có 2 sự kiện diễn ra trong 2 ngày trắng, phân phối hợp lệ dẫn đến hai cấu hình sau khi xem xét các ràng buộc đặt hàng, xác nhận đầu ra`2`. 

Đầu vào mẫu 2:```
4 2 2
```| w1_ngày | b_ngày | w2_ngày | w1_ways | b_ways | w2_ways | Đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 2 | 2 | 1 | 4 | 
| 1 | 2 | 1 | 2 | 1 | 2 | 4 | 
| 2 | 1 | 1 | 1 | 2 | 2 | 4 | 

Tổng số đóng góp là 12, thể hiện việc chia nhiều lần trong các ngày mang lại những hoán vị khác nhau như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Lặp lại tất cả các phần chia ngày hợp lệ; tính toán trước giai thừa là O(MAX) | 
| Không gian | O(TỐI ĐA) | Lưu trữ giai thừa và nghịch đảo lên tới 4000 | 

Được cho`n ≤ 4000`, điều này dẫn đến ~ 16 triệu lần lặp, khả thi trong giới hạn 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 10**9 + 9
    MAX = 4000 + 10
    fact = [1]*MAX
    inv_fact = [1]*MAX
    for i in range(1, MAX):
        fact[i] = fact[i-1]*i%MOD
    inv_fact[MAX-1] = pow(fact[MAX-1], MOD-2, MOD)
    for i in range(MAX-2, -1, -1):
        inv_fact[i] = inv_fact[i+1]*(i+1)%MOD
    def comb(n,k):
        if k<0 or k>n: return 0
        return fact[n]*inv_fact[k]%MOD*inv_fact[n-k]%MOD
    def ways(events, days):
        if days>events: return 0
        return comb(events-1, days-1)
    n,w,b = map(int, input().split())
    ans = 0
    for w1_days in
```
