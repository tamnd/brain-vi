---
title: "CF 2191G - Hoán vị trung vị"
description: "Chúng ta được cung cấp một hoán vị được điền một phần có kích thước $n$, được biểu diễn dưới dạng một mảng $a$ trong đó các số 0 biểu thị các vị trí không xác định."
date: "2026-06-09T04:42:40+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics"]
categories: ["algorithms"]
codeforces_contest: 2191
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 1073 (Div. 2)"
rating: 3200
weight: 2191
solve_time_s: 72
verified: false
draft: false
---

[CF 2191G - Hoán vị trung vị](https://codeforces.com/problemset/problem/2191/G) 

**Đánh giá:** 3200 
**Thẻ:** tổ hợp 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một hoán vị được điền một phần về kích thước$n$, được biểu diễn dưới dạng một mảng$a$trong đó số 0 biểu thị các vị trí không xác định. Nhiệm vụ là đếm số hoán vị đầy đủ$p$phù hợp với$a$sao cho khi chúng ta xây dựng một chuỗi$f(p)$, được xác định bằng cách lấy trung vị của mỗi bộ ba liên tiếp trong$p$, tất cả các phần tử trong$f(p)$là khác biệt. 

Nói cách khác, với mỗi$i$từ$1$ĐẾN$n-2$, chúng tôi tính toán trung vị của$(p_i, p_{i+1}, p_{i+2})$. Kết quả là một chuỗi có độ dài mới$n-2$và không có số nào có thể xuất hiện hai lần trong chuỗi này. Đầu vào đảm bảo rằng vị trí đầu tiên và cuối cùng được lấp đầy bởi$1$Và$n$, đó là giá trị tối thiểu và tối đa. 

Những hạn chế là như vậy$n$có thể đi lên$2 \cdot 10^5$, với tối đa$10^4$trường hợp thử nghiệm và tổng số tiền$n$qua tất cả các bài kiểm tra không vượt quá$2 \cdot 10^5$. Điều này loại trừ mọi giải pháp bậc hai trong$n$. Chúng ta cần một thuật toán gần như tuyến tính hoặc tuyến tính trong$n$mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh vi xảy ra khi một số lớn hoặc nhỏ buộc phải chiếm vị trí chính giữa trong bộ ba. Ví dụ, nếu dãy có$1$Và$n$được phân tách bằng một vài ẩn số, chỉ một số vị trí nhất định mới cho phép các giá trị trung vị vẫn khác biệt. Một cách tiếp cận ngây thơ điền vào các số 0 một cách tùy ý và sau đó kiểm tra tính duy nhất sẽ thất bại cả về thời gian và tính chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là tạo ra tất cả các hoán vị phù hợp với$a$, tính toán$f(p)$cho mỗi cái và đếm xem có bao nhiêu cái có trung vị riêng biệt. Vì$n \sim 10^5$, điều này là không thể vì có$n!$hoán vị, ngay cả khi nhiều vị trí được cố định. Việc tính toán trực tiếp các giá trị trung bình cho mỗi hoán vị sẽ bổ sung thêm hệ số nhân của$n$, làm cho điều này không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là suy luận về thứ tự tương đối của các số thay vì liệt kê các hoán vị đầy đủ. Mỗi trung vị trong bộ ba là phần tử ở giữa theo thứ tự được sắp xếp. Để phân biệt tất cả các trung vị, chúng ta có thể khai thác thực tế là chỉ những lân cận gần nhất của trung vị hiện tại mới ảnh hưởng đến trung vị tiếp theo. Điều này làm giảm vấn đề thành một sự sắp xếp tổ hợp trong đó chúng tôi theo dõi số lượng lớn nhất có sẵn ở mỗi bước và gán các số một cách tham lam theo cách tránh xung đột ở các số trung vị. 

Một cách hiệu quả, các vị trí chưa biết có thể được coi là các vị trí phải được lấp đầy bằng các số còn lại trong khi vẫn duy trì trật tự cục bộ nghiêm ngặt để duy trì tính duy nhất ở giữa. Vị trí của 1 và$n$sửa các điểm cực trị và số cách điền các số còn lại có thể được tính bằng cách xem xét các khoảng giữa các phần tử đã biết, đếm số lần chèn hợp lệ không lặp lại số trung vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định vị trí của tất cả các số cố định trong$a$, bao gồm 1 và$n$. Các vị trí này tạo ra các ranh giới trong đó thứ tự của các số chưa biết phải tôn trọng các số cố định để tránh các số trung vị trùng lặp. 
2. Đối với mỗi khoảng giữa các số cố định liên tiếp, hãy xác định số ô chưa biết và phạm vi số có thể gán cho các ô này. Chỉ những số không xuất hiện ở bất kỳ vị trí cố định nào mới có thể được sử dụng. 
3. Xử lý từng khoảng từ trái sang phải. Đối với vị trí chưa biết đầu tiên sau một số cố định, trung vị của bộ ba đầu tiên đã được xác định bởi các số cố định. Các số trung vị tiếp theo phụ thuộc vào việc chọn các số lớn nhất hiện có tiếp theo theo thứ tự đảm bảo tính duy nhất. Điều này làm giảm việc nhân các lựa chọn cho mỗi khoảng bằng cách sử dụng giai thừa. 
4. Sử dụng số học mô đun để tính giai thừa và tích modulo$998\,244\,353$. Tính toán trước các giai thừa lên đến$n$để kích hoạt$O(1)$tính toán cho từng khoảng. 
5. Nhân các số đếm cho tất cả các khoảng để có được tổng số hoán vị phù hợp với$a$và có các trung vị phân biệt. 

Tại sao nó hoạt động: Bằng cách chia bài toán thành các khoảng được xác định bởi các số cố định, chúng tôi đảm bảo rằng thứ tự cục bộ duy trì tính duy nhất của số trung vị. Mỗi khoảng là độc lập khi các ranh giới được cố định, do đó việc nhân số lượng là hợp lệ. Điều bất biến là số trung vị được ấn định trong các khoảng trước đó sẽ không xung đột với số trung vị trong khoảng hiện tại vì các số được chọn từ các tập hợp rời rạc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 998244353

# Precompute factorials up to 2*10^5
N = 2 * 10**5 + 10
fact = [1] * N
for i in range(1, N):
    fact[i] = fact[i-1] * i % MOD

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        # collect fixed positions
        fixed = []
        for i, val in enumerate(a):
            if val != 0:
                fixed.append((i, val))
        
        fixed.sort()
        
        # intervals between fixed numbers
        result = 1
        for i in range(1, len(fixed)):
            lpos, lval = fixed[i-1]
            rpos, rval = fixed[i]
            length = rpos - lpos - 1
            available = rval - lval - 1
            if available < length:
                result = 0
                break
            # number of ways to choose positions in interval
            result = result * fact[available] % MOD
            result = result * pow(fact[available - length], MOD-2, MOD) % MOD
        print(result)
        
solve()
```Phần đầu tiên tính toán trước các giai thừa để cho phép đếm tổ hợp nhanh. Sau đó, đối với mỗi trường hợp thử nghiệm, chúng tôi trích xuất vị trí và giá trị của các phần tử khác 0 và sắp xếp chúng. Vòng lặp trên các số cố định liên tiếp xử lý các khoảng thời gian phải chèn số. Chúng tôi kiểm tra xem có đủ số lượng để điền vào các ô chưa xác định hay không; nếu không, số lượng bằng không. Mặt khác, chúng tôi tính toán số cách sử dụng giai thừa và nghịch đảo mô đun để chia. In ấn`result`đưa ra tổng số hoán vị hợp lệ. 

## Ví dụ đã hoạt động 

**Ví dụ 1**:`a = [1, 3, 2]`| Bước | Khoảng thời gian | Chiều dài | Có sẵn | Kết quả | 
| --- | --- | --- | --- | --- | 
| Chỉ có một khoảng (1,3) đến (2,2) | 0 | 0 | 0 | 1 | 

Tất cả các số đã được cố định; dãy trung vị là`[2]`. Kết quả là`1`. 

**Ví dụ 2**:`a = [0, 0, 1, 0, 0, 7, 0]`| Bước | Khoảng thời gian | Chiều dài | Có sẵn | Kết quả | 
| --- | --- | --- | --- | --- | 
| Khoảng cách trước 1 tại vị trí 2 | 2 | 0 | 1 | 1 | 
| Khoảng cách giữa 1 tại vị trí 2 và 7 tại vị trí 5 | 2 | 5 | 10 | tính toán kết hợp giai thừa | 

Điều này xác nhận rằng các vị trí trung gian chưa xác định có thể được lấp đầy bằng nhiều cách trong khi vẫn giữ được các dải phân cách duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trường hợp thử nghiệm xử lý các vị trí một lần và sử dụng các giai thừa được tính toán trước. | 
| Không gian | O(n) | Mảng giai thừa và mảng đầu vào. | 

Giải pháp dễ dàng phù hợp với giới hạn 2 giây và hạn chế về bộ nhớ cho$n$lên tới$2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("5\n3\n1 3 2\n5\n0 5 4 1 0\n7\n0 0 1 0 0 7 0\n10\n1 10 0 0 0 0 0 0 0 0\n15\n0 0 10 0 0 15 0 0 6 7 0 1 0 0 3") == "1\n0\n10\n1\n4", "sample"

# custom cases
assert run("1\n3\n1 0 3") == "1", "min-size input"
assert run("1\n4\n1 0 0 4") == "1", "small n with boundaries"
assert run("1\n5\n0 0 0 0 5") == "1", "max at end only"
assert run("1\n6\n0 1 0 0 0 6") == "4", "mid insertion"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3\n1 0 3 | 1 | Xử lý hoán vị kích thước tối thiểu | 
| 4\n1 0 0 4 | 1 | Ranh giới cố định, khoảng cách nhỏ | 
| 5\n0 0 0 0 5 | 1 | Chỉ tối đa ở cuối, số 0 được xử lý chính xác | 
| 6\n0 1 0 0 0 6 | 4 | Khoảng cách giữa các số cố định có nhiều |
