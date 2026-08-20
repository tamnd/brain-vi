---
title: "CF 102203K - \u041f\u0435\u0440\u0435\u0445\u0432\u0430\u0442"
description: "Có (n) tác nhân riêng biệt và (m) sân bay vũ trụ riêng biệt. Mỗi tác nhân phải được chỉ định cho chính xác một sân bay vũ trụ và mỗi sân bay vũ trụ phải nhận ít nhất một tác nhân."
date: "2026-08-18T00:53:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102203
codeforces_index: "K"
codeforces_contest_name: "\u0427\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u041b\u0438\u043f\u0435\u0446\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e (8-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 102203
solve_time_s: 72
verified: true
draft: false
---

[CF 102203K - \u041f\u0435\u0440\u0435\u0445\u0432\u0430\u0442](https://codeforces.com/problemset/problem/102203/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Có (n) tác nhân riêng biệt và (m) sân bay vũ trụ riêng biệt. Mỗi tác nhân phải được chỉ định cho chính xác một sân bay vũ trụ và mỗi sân bay vũ trụ phải nhận ít nhất một tác nhân. Các tác nhân được gán cho cùng một sân bay vũ trụ được phép, vì vậy nhiệm vụ là đếm tất cả các hàm tính từ từ tập hợp các tác nhân đến tập hợp các cổng không gian. 

Các đại lý là khác biệt. Ví dụ: với (n=2) và (m=2), hai phép gán hợp lệ là tác nhân 1 đến cổng 1 và tác nhân 2 đến cổng 2 hoặc ngược lại. Câu trả lời là như vậy 2. 

Chúng tôi cần câu trả lời modulo (998244353). Cả (n) và (m) đều có thể đạt tới 250000, do đó, giải pháp lập trình động (O(nm)) sẽ yêu cầu khoảng (6,25\cdot10^{10}) thao tác và hoàn toàn không khả thi. Thậm chí (O(m^2)) là quá lớn. Với giới hạn một giây, giải pháp dự định về cơ bản cần phải tuyến tính ngoại trừ hệ số logarit nhỏ. 

Có một số trường hợp ranh giới mà việc triển khai bất cẩn có thể xử lý sai. Ví dụ: nếu có nhiều cổng không gian hơn tác nhân (n=2,m=3), thì không có phép gán nào có thể bao gồm cả ba cổng, vì vậy câu trả lời là 0. Một công thức liên quan đến giai thừa hoặc loại trừ bao gồm phải xử lý vấn đề này trước khi thực hiện bất kỳ phép chia nào. 

Khi có chính xác một sân bay vũ trụ, mỗi đặc vụ chỉ có một điểm đến khả thi. Do đó (n=5,m=1) có câu trả lời 1. Việc triển khai vô tình bắt đầu vòng lặp loại trừ bao gồm từ điểm cuối sai có thể tạo ra 0 tại đây. 

Khi (n=m), mỗi sân bay vũ trụ phải chứa chính xác một tác nhân, vì vậy câu trả lời là (n!). Với (n=2,m=2), kết quả này là 2. Đối với (n=1,m=1), nó cho kết quả 1. Trường hợp này cũng hữu ích để kiểm tra xem dấu bao gồm-loại trừ cuối cùng và các hệ số nhị thức có được xử lý chính xác hay không. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp có thể xem xét mọi điểm đến có thể có của mọi tác nhân. Mỗi trong số (n) tác nhân có (m) lựa chọn, do đó có (m^n) nhiệm vụ cần kiểm tra. Đối với mỗi bài tập, chúng tôi sẽ kiểm tra xem tất cả (m) sân bay vũ trụ đã được sử dụng hay chưa. Số lượng bài tập được tạo đã là (250000^{250000}) trong trường hợp lớn nhất, do đó phương pháp này không thành công trước khi chi phí kiểm tra trở nên quan trọng. 

Brute-force hoạt động vì nó liệt kê rõ ràng chính xác các đối tượng mà chúng ta muốn đếm. Vấn đề là hầu hết những đối tượng đó đều không hợp lệ và không có lý do gì để kiểm tra chúng một cách riêng lẻ. 

Quan sát quan trọng là điều kiện "mọi sân bay vũ trụ đều được sử dụng" có thể được xử lý bằng cách loại trừ bao gồm. Bắt đầu với tất cả (m^n) phép gán mà không yêu cầu chiếm bất kỳ cổng nào. Sau đó trừ đi các phép gán trong đó có ít nhất một cổng cụ thể trống. Nếu hai cổng cụ thể bị buộc phải trống, thì chỉ có (m-2) đích đến có thể có cho mỗi tác nhân, đưa ra các nhiệm vụ ((m-2)^n). Tiếp tục quá trình này sẽ cho một số tiền xen kẽ. 

Nếu chính xác (k) sân bay vũ trụ được chọn là bị cấm, thì có (\binom{m}{k}) cách để chọn chúng và có ((m-k)^n) nhiệm vụ tránh chúng. Do đó, loại trừ bao gồm mang lại 

[ 
\sum_{k=0}^{m}(-1)^k\binom{m}{k}(m-k)^n. 
] 

Khi (m>n), câu trả lời ngay lập tức là 0. Ngược lại, chúng ta chỉ cần (m+1) số hạng. Mỗi lũy thừa có thể được tính bằng lũy ​​thừa mô-đun theo (O(\log n)), trong khi tất cả các hệ số nhị thức có thể được tạo ra trong thời gian tuyến tính bằng cách sử dụng giai thừa và giai thừa nghịch đảo. 

Vì môđun là số nguyên tố và (m<998244353), mọi số nguyên từ 1 đến (m) đều có môđun nghịch đảo. Chúng ta tính toán trước các giai thừa và giai thừa nghịch đảo, sau đó thu được 

\frac{m!}{k!(m-k)!} 
\pmod {998244353}. 
] 

Việc triển khai thậm chí còn đơn giản hơn sẽ tạo ra các hệ số nhị thức liên tiếp từ 

\binom{m}{k}\frac{m-k}{k+1}. 
] 

Đối với sự lặp lại đó, chúng tôi tính toán trước các nghịch đảo mô-đun của (1,2,\ldots,m). Điều này giữ cho toàn bộ quá trình triển khai tuyến tính trong bộ nhớ và tránh việc lưu trữ các mảng giai thừa. 

Sự so sánh là:

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(m^n)) | (O(n+m)) | Quá chậm | 
| Bao gồm-loại trừ | (O(m\log n)) | (O(m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (n) và (m). Nếu (m>n), xuất ra 0 ngay lập tức. Không có đủ đặc vụ để đặt ít nhất một đặc vụ vào mỗi sân bay vũ trụ. 
2. Tính toán trước nghịch đảo mô đun cho tất cả các số nguyên từ 1 đến (m). Vì môđun là số nguyên tố nên có thể thu được nghịch đảo của (i) bằng cách sử dụng 

MOD-\left\lfloor\frac{MOD}{i}\right\rfloor 
\operatorname{inv}(MOD\bmod i) 
\pmod {MOD}. 
] 

Toàn bộ mảng được tính trong thời gian (O(m)). 

1. Bắt đầu với hệ số nhị thức (\binom{m}{0}=1). Đối với hiện tại (k), hãy tính 

[ 
\binom{m}{k}(m-k)^n 
] 

sử dụng lũy thừa mô-đun. 

1. Thêm số hạng khi (k) chẵn và trừ nó khi (k) lẻ. Đây chính xác là dấu hiệu bao gồm-loại trừ, bởi vì việc chọn (k) các sân bay vũ trụ bị cấm có nghĩa là đếm các bài tập tránh được tất cả chúng. 
2. Cập nhật hệ số nhị thức bằng cách sử dụng 

\binom{m}{k}(m-k)\operatorname{inv}(k+1) 
\pmod {MOD}. 
] 

Điều này tránh việc tính toán giai thừa riêng biệt cho mỗi thuật ngữ. 

1. Tiếp tục đến (k=m), duy trì câu trả lời theo modulo (998244353). Vì (n\ge1), số hạng (k=m) chứa (0^n=0), nên nó không đóng góp gì cả. Dù sao thì việc bao gồm nó cũng làm cho công thức trở nên thống nhất. 

### Tại sao nó hoạt động 

Đối với mỗi tập con (S) của các sân bay vũ trụ, hãy xem xét các phép gán trong đó mọi cổng trong (S) đều trống. Nếu (|S|=k), mọi tác nhân đều có chính xác (m-k) đích đến, do đó có ((m-k)^n) các nhiệm vụ như vậy. Có (\binom{m}{k}) lựa chọn cho (S). 

Loại trừ bao gồm thêm các phép gán tránh không có cổng được chỉ định, trừ các phép gán trong đó một cổng trống, thêm lại các phép gán trong đó hai cổng trống, v.v. Phép gán sử dụng mọi cổng không gian chỉ thuộc về số hạng (k=0). Bất kỳ phép gán nào thiếu chính xác (r>0) cổng không gian sẽ xuất hiện với tổng hệ số 

# (1-1)^r 

1. 

] 

Do đó, mọi phép gán không hợp lệ sẽ bị hủy hoàn toàn, trong khi mọi phép gán hợp lệ vẫn duy trì chính xác một lần. Tổng kết quả chính xác là số lượng phân phối cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n, m = map(int, input().split())

    if m > n:
        print(0)
        return

    # Modular inverses of 1..m.
    inv = [0] * (m + 1)
    if m >= 1:
        inv[1] = 1

    for i in range(2, m + 1):
        inv[i] = MOD - (MOD // i) * inv[MOD % i] % MOD

    # C(m, 0)
    comb = 1
    ans = 0

    for k in range(m):
        ways = pow(m - k, n, MOD)
        term = comb * ways % MOD

        if k & 1:
            ans -= term
        else:
            ans += term

        if ans >= MOD:
            ans -= MOD
        elif ans < 0:
            ans += MOD

        # C(m, k + 1) from C(m, k)
        comb = comb * (m - k) % MOD
        comb = comb * inv[k + 1] % MOD

    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Nhánh đầu tiên xử lý sự bất khả thi về cấu trúc (m>n). Việc trả về ngay lập tức cũng hữu ích vì phần còn lại của công thức sẽ chỉ yêu cầu nghịch đảo mô-đun tối đa (m), trong khi không có phân phối hợp lệ nào tồn tại. 

Mảng nghịch đảo sử dụng phép truy hồi mô đun nguyên tố tiêu chuẩn. biểu hiện`MOD % i`luôn nhỏ hơn`i`, do đó nghịch đảo của nó đã được tính toán khi xử lý`i`. 

Biến`comb`lưu trữ giá trị hiện tại của (\binom{m}{k}). Nó bắt đầu từ 1, tương ứng với (k=0) và chỉ được cập nhật sau khi xử lý thuật ngữ hiện tại. Thứ tự quan trọng vì thuật ngữ hiện tại phải sử dụng (\binom{m}{k}), chứ không phải (\binom{m}{k+1}). 

Vòng lặp dừng ở`range(m)`, xử lý (k=0,\ldots,m-1). Số hạng (k=m) bị bỏ qua là (0^n=0), bởi vì (n\ge1), nên việc bỏ qua nó không thay đổi gì cả. Điều này cũng tránh việc phụ thuộc vào cách triển khai nguồn xử lý biểu thức đặc biệt (0^n). 

của Python`pow(base, exponent, MOD)`thực hiện phép lũy thừa mô-đun một cách trực tiếp, do đó các giá trị trung gian không bao giờ lớn bằng số nguyên thông thường (cơ số^n). Câu trả lời và tất cả các hệ số nhị thức đều được giảm modulo`MOD`sau mỗi lần nhân. 

## Ví dụ đã hoạt động 

### Mẫu 1: (n=2,m=2) 

Công thức tính các nhiệm vụ của hai tác nhân cho hai cổng trong khi loại trừ các nhiệm vụ khiến một cổng trống. 

| (k) | (\binom{2}{k}) | (2-k) | ((2-k)^2) | Ký kết thời hạn | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 4 | +4 | 4 | 
| 1 | 2 | 1 | 1 | -2 | 2 | 

Số hạng (k=2) là (0^2=0). Đáp án cuối cùng là 2, tương ứng với hai hoán vị của các tác nhân giữa hai cổng. Việc hủy bỏ loại trừ bao gồm sẽ loại bỏ hai nhiệm vụ trong đó cả hai tác nhân đều đi đến cùng một cổng. 

### Mẫu 2: (n=3,m=7) 

Có bảy cổng nhưng chỉ có ba đại lý. 

| Sân khấu | (n) | (m) | Quyết định | Trả lời | 
| --- | --- | --- | --- | --- | 
| Đầu vào | 3 | 7 | (m>n) | 0 | 

Thuật toán trả về ngay lập tức. Cần ít nhất bảy tác nhân để đưa một tác nhân vào mỗi cổng trong số bảy cổng riêng biệt, do đó không có sự phân công nào có thể thỏa mãn điều kiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(m\log n)) | Có (O(m)) thuật ngữ loại trừ bao gồm, mỗi thuật ngữ sử dụng lũy ​​thừa mô-đun trong quá trình tiền xử lý nghịch đảo (O(\log n)), cộng với (O(m)) | 
| Không gian | (O(m)) | Mảng nghịch đảo mô-đun chứa (m+1) số nguyên | 

Với (m\le250000), quá trình tiền xử lý tuyến tính nhỏ và phép lũy thừa mô-đun chỉ sử dụng nhiều bước nhân theo logarit cho mỗi số hạng. Công việc này nhỏ hơn đáng kể so với công việc (O(m^2)) hoặc (O(mn)) mà các ràng buộc loại trừ. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 998244353

def solve():
    n, m = map(int, sys.stdin.readline().split())

    if m > n:
        print(0)
        return

    inv = [0] * (m + 1)
    inv[1] = 1

    for i in range(2, m + 1):
        inv[i] = MOD - (MOD // i) * inv[MOD % i] % MOD

    comb = 1
    ans = 0

    for k in range(m):
        term = comb * pow(m - k, n, MOD) % MOD

        if k & 1:
            ans -= term
        else:
            ans += term

        ans %= MOD

        comb = comb * (m - k) % MOD
        comb = comb * inv[k + 1] % MOD

    print(ans)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("2 2\n") == "2", "sample 1"
assert run("3 7\n") == "0", "sample 2"

# Minimum-size input
assert run("1 1\n") == "1", "single agent and single port"

# More agents than ports
# Number of onto functions from 4 agents to 2 ports:
# 2^4 - C(2,1)*1^4 = 16 - 2 = 14.
assert run("4 2\n") == "14", "four agents, two ports"

# Exactly as many agents as ports: every agent must occupy
# a different port, so the answer is 5!.
assert run("5 5\n") == "120", "equal numbers"

# Maximum-size input.
# When n == m, the answer is n! modulo MOD.
expected = 1
for x in range(1, 250001):
    expected = expected * x % MOD

assert run("250000 250000\n") == str(expected), "maximum-size equal case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Đầu vào tối thiểu và ranh giới sân bay vũ trụ đơn | 
|`4 2`|`14`| Loại trừ bao gồm không cần thiết với các bài tập lặp đi lặp lại được phép | 
|`5 5`|`120`| Ranh giới (n=m), trong đó câu trả lời trở thành (n!) | 
|`250000 250000`| (250000!\bmod 998244353) | Kích thước đầu vào tối đa và số học mô-đun lớn | 

## Vỏ cạnh 

Với (n=2,m=3), thuật toán đi vào điều kiện đầu tiên vì có nhiều cổng hơn tác nhân. Nó in`0`mà không cần xây dựng mảng nghịch đảo. Đây là kết quả đúng vì việc bao phủ ba cổng riêng biệt cần ít nhất ba tác nhân. 

Với (n=5,m=1), mảng nghịch đảo chỉ chứa`inv[1]=1`. Vòng lặp có một lần lặp, tương ứng với (k=0). Nó cộng (1^5=1), đưa ra câu trả lời đúng`1`. Mỗi đại lý phải đi đến cổng duy nhất có sẵn. 

Với (n=1,m=1), phép tính tương tự sẽ cho kết quả (1^1=1). Điều này xác nhận rằng đầu vào nhỏ nhất có thể không yêu cầu công thức đặc biệt ngoài thuật toán chung. 

Với (n=m=2), phép tính bao gồm-loại trừ là 

# 4-2 

1. 

] 

Bốn nhiệm vụ không hạn chế bao gồm hai nhiệm vụ hợp lệ và hai nhiệm vụ trong đó cả hai tác nhân sử dụng cùng một cổng. Thuật ngữ thứ hai loại bỏ chính xác hai phép gán không hợp lệ đó. 

Với (n=m) nói chung, mọi phân phối hợp lệ đều có chính xác một tác nhân tại mỗi cổng. Các tác nhân có thể được hoán vị tùy ý, đưa ra (n!) phép gán hợp lệ. Công thức bao gồm-loại trừ tạo ra chính xác giá trị này, do đó ranh giới (n=m) cũng cung cấp khả năng kiểm tra chặt chẽ về dấu xen kẽ và phép truy hồi nhị thức. 

Việc triển khai cũng tránh được vấn đề riêng lẻ tại (k=m). Số hạng cuối cùng đó sẽ chứa (0^n), bằng 0 vì (n\ge1), do đó việc xử lý chỉ (k=0) đến (m-1) là hoàn tất về mặt toán học.
