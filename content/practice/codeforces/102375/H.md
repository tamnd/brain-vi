---
title: "CF 102375H - ICPC"
description: "Đối với độ dài từ tối đa nhất định (N), từ điển chứa mọi chuỗi tiếng Anh viết thường có độ dài từ (1) đến (N). Các từ có cùng độ dài xuất hiện theo thứ tự từ điển, trong khi tất cả các từ ngắn hơn sẽ xuất hiện trước."
date: "2026-08-15T18:03:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102375
codeforces_index: "H"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u0421\u0435\u0432\u0435\u0440\u043e-\u0417\u0430\u043f\u0430\u0434\u0430 \u0420\u043e\u0441\u0441\u0438\u0438 \u0438 \u041c\u043e\u0441\u043a\u0432\u044b ICPC 2019"
rating: 0
weight: 102375
solve_time_s: 165
verified: false
draft: false
---

[CF 102375H - ICPC](https://codeforces.com/problemset/problem/102375/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 45s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Đối với độ dài từ tối đa nhất định (N), từ điển chứa mọi chuỗi tiếng Anh viết thường có độ dài từ (1) đến (N). Các từ có cùng độ dài xuất hiện theo thứ tự từ điển, trong khi tất cả các từ ngắn hơn sẽ xuất hiện trước. Việc nối toàn bộ từ điển sẽ tạo ra một chuỗi rất lớn. Chúng ta cần số lần xuất hiện của chuỗi con bốn ký tự`icpc`trong phép nối đó, modulo (10^9+7). 

Khó khăn là (N) có thể lớn bằng (10^9). Từ điển đã chứa (26^N) từ có độ dài (N), do đó, ngay cả việc biểu diễn lớp cuối cùng cũng là không thể. Vấn đề ban đầu có giới hạn 2 giây và giới hạn bộ nhớ 512 MiB, loại trừ mọi thứ tỷ lệ thuận với số lượng từ hoặc ký tự được tạo. Lời giải phải phụ thuộc logarit vào (N), sử dụng lũy ​​thừa mô-đun thay vì liệt kê từng độ dài một. 

Có hai loại lần xuất hiện cần đếm. Một sự xuất hiện có thể hoàn toàn nằm trong một từ trong từ điển hoặc nó có thể vượt qua ranh giới giữa hai từ liên tiếp. Bỏ qua loại thứ hai là một lỗi nhỏ vì phép nối sẽ loại bỏ ranh giới giữa các từ. 

Ví dụ, với (N=3), không có từ nào đủ dài để chứa`icpc`, và cũng không có ranh giới nào có thể tạo ra được nó. Câu trả lời đúng là`0`. Một giải pháp áp dụng một cách mù quáng công thức chứa (26^{N-4}) mà không xử lý (N<4) có thể vô tình diễn giải số mũ âm. 

Với (N=4), đáp án là`4`, không`1`. từ`icpc`chính nó đóng góp một sự xuất hiện nội bộ. Ba lần xuất hiện bổ sung được tạo ra qua ranh giới giữa các từ có bốn chữ cái liên tiếp, cụ thể là ranh giới sau`cpci`,`pcic`, Và`cicp`. Giải pháp chỉ tính số lần xuất hiện bên trong các từ riêng lẻ sẽ bỏ sót ba từ này. 

Ranh giới giữa từ cuối cùng của một độ dài và từ đầu tiên của độ dài tiếp theo cũng dễ bị xử lý sai. Ví dụ, ranh giới giữa`zzz`Và`aaaa`bao gồm hoàn toàn`z`ký tự theo sau là`a`ký tự, vì vậy nó không thể tạo`icpc`. Việc coi mọi ranh giới từ điển là tương đương sẽ vượt quá các chuyển đổi như vậy. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Tạo mọi từ theo thứ tự từ điển, nối chúng và quét tìm`icpc`. Điều này đúng vì mọi lần xuất hiện có thể xảy ra trong chuỗi cuối cùng đều được kiểm tra chính xác một lần. Tuy nhiên, có 

[ 
26+26^2+\cdots+26^N 
] 

từ và tổng số ký tự là 

[ 
\sum_{L=1}^{N} L26^L=\Theta(N26^N). 
] 

Đối với (N=10^9), thậm chí (26^N) không thể được biểu diễn, do đó, lực lượng vũ phu sẽ thất bại về mặt thiên văn trước khi giới hạn thời gian trở nên phù hợp. 

Quan sát hữu ích là mẫu có độ dài cố định là bốn. Đối với một từ có độ dài (L), các lần xuất hiện bên trong có thể được tính theo vị trí mà không cần tạo bất kỳ từ nào. Mỗi vị trí cố định yêu cầu bốn chữ cái được chỉ định, để lại (L-3) vị trí trống, do đó có (26^{L-4}(L-3)) lần xuất hiện trên tất cả các từ có độ dài đó. 

Vấn đề duy nhất còn lại là ranh giới nối. Sự xuất hiện bốn ký tự vượt qua ranh giới phải lấy (1), (2) hoặc (3) ký tự từ từ bên trái. Bởi vì mô hình là`icpc`, hậu tố bên trái và tiền tố bên phải được yêu cầu lần lượt là 

[ 
(i,\cpc),\qquad 
(ic,\pc),\qquad 
(icp,\c). 
] 

Đối với các từ liên tiếp có cùng độ dài, từ bên phải thu được bằng cách tăng dần từ bên trái theo cơ số (26). Mọi hậu tố bên trái bắt buộc đều kết thúc bằng một chữ cái khác với`z`, do đó không có phần mang nào đạt đến tiền tố. Đối với (L\ge4), mỗi loại trong số ba loại ranh giới cố định bốn vị trí ký tự và để lại (L-4) vị trí tùy ý. Do đó, mỗi loại xảy ra (26^{L-4}) lần, tạo ra (3\cdot26^{L-4}) lần xuất hiện giao nhau. 

Sự chuyển tiếp từ`z...z`ĐẾN`a...a`đóng góp bằng 0, vì các ký tự ranh giới của nó chỉ chứa`z`Và`a`. Đối với (L<4), ba mẫu ranh giới không thể khớp một cách nhất quán vào một từ có độ dài đó, do đó không có sự xuất hiện giao nhau có cùng độ dài. 

Kết hợp các lần xuất hiện bên trong và giao nhau cho một (L\ge4) cố định sẽ cho 

[ 
(L-3)26^{L-4}+3\cdot26^{L-4} 
=L26^{L-4}. 
] 

Vì vậy, toàn bộ vấn đề trở thành việc đánh giá 

[ 
\sum_{L=4}^{N}L26^{L-4} 
] 

modulo (10^9+7). 

Đây là một tiến trình hình học có trọng số. Chúng ta có thể đánh giá nó bằng cách sử dụng các công thức tiêu chuẩn cho tổng hình học và tổng hình học có trọng số, chỉ yêu cầu một lũy thừa mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N26^N)) | (O(N26^N)) | Quá chậm | 
| Tối ưu | (O(\log N)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu (N<4), trả về`0`. Không có từ nào chứa bốn ký tự và ranh giới từ ngắn không thể hình thành`icpc`. 
2. Với mỗi độ dài từ có thể có (L\ge4), hãy đếm số lần xuất hiện hoàn toàn bên trong các từ. Có (26^{L-4}) lựa chọn cho các ký tự còn lại cho mỗi (L-3) vị trí bắt đầu có thể, cho ra ((L-3)26^{L-4}). 
3. Đếm số lần vượt qua ranh giới giữa các từ liên tiếp có cùng độ dài. Tách`icpc`sau ký tự đầu tiên, thứ hai hoặc thứ ba của nó cho biết ba hình dạng ranh giới có thể có. Đối với mỗi hình dạng, bốn vị trí ký tự được cố định và các vị trí (L-4) khác là tùy ý, do đó mỗi hình dạng đóng góp (26^{L-4}). Do đó, đóng góp ranh giới là (3\cdot26^{L-4}). 
4. Thêm hai đóng góp. Tổng chiều dài (L) là 

[ 
(L-3+3)26^{L-4}=L26^{L-4}. 
] 

1. Đặt (k=L-4) và đặt (t=N-3). Khi đó (k) nằm trong khoảng từ (0) đến (t-1) và câu trả lời trở thành 

[ 
\sum_{k=0}^{t-1}(k+4)26^k. 
] 

Tách cái này thành 

[ 
\sum_{k=0}^{t-1}k26^k 
+ 
4\sum_{k=0}^{t-1}26^k. 
] 

1. Sử dụng công thức chuỗi hình học 

[ 
\sum_{k=0}^{t-1}r^k=\frac{r^t-1}{r-1} 
] 

với (r=26). 

1. Sử dụng công thức chuỗi hình học có trọng số 

\frac{r-tr^t+(t-1)r^{t+1}}{(r-1)^2}. 
] 

Tất cả các phép chia được thực hiện theo modulo (10^9+7). Vì (r-1=25) khác 0 modulo mô đun nguyên tố, nên tồn tại nghịch đảo mô đun của nó. 

1. Tính (26^t\bmod(10^9+7)) bằng cách sử dụng lũy ​​thừa nhị phân. Đây là phép toán duy nhất phụ thuộc logarit vào (N), vì vậy thuật toán hoàn chỉnh mất (O(\log N)) thời gian. 

### Tại sao nó hoạt động 

Với mỗi độ dài (L), mỗi lần xuất hiện thuộc về duy nhất hoặc bên trong một từ hoặc trên chính xác một ranh giới giữa các từ liên tiếp. Số đếm nội bộ xem xét mọi vị trí bắt đầu có thể có và mọi phép gán của các ký tự còn lại. Đối với sự xuất hiện của ranh giới, điểm phân chia phải là một trong ba đường cắt bên trong của`icpc`và cấu trúc kế thừa từ điển học đưa ra chính xác (26^{L-4}) ranh giới hợp lệ cho mỗi lần cắt khi (L\ge4). Sự chuyển đổi giữa các độ dài từ khác nhau không đóng góp gì. Do đó, đóng góp chính xác của mọi độ dài là (L26^{L-4}) và phép tính chuỗi hình học cuối cùng đánh giá chính xác tổng của các đóng góp đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7
R = 26
INV25 = 280000002

def solve():
    n = int(input())

    if n < 4:
        print(0)
        return

    # We need:
    # sum_{k=0}^{t-1} (k + 4) * 26^k
    t = n - 3

    p = pow(R, t, MOD)

    # sum_{k=0}^{t-1} r^k
    geometric = (p - 1) % MOD
    geometric = geometric * INV25 % MOD

    # sum_{k=0}^{t-1} k * r^k
    weighted_num = (
        R
        - (t % MOD) * p
        + ((t - 1) % MOD) * p % MOD * R
    ) % MOD

    weighted = weighted_num * INV25 % MOD * INV25 % MOD

    answer = (weighted + 4 * geometric) % MOD
    print(answer)

if __name__ == "__main__":
    solve()
```Lợi nhuận sớm xử lý toàn bộ phạm vi (N<4), tránh số mũ âm và các giả định ranh giới không hợp lệ. 

Biến`t = n - 3`là số số hạng sau khi thay thế (k=L-4). Với (N=4),`t`là một, vì vậy tổng chỉ chứa số hạng (4\cdot26^0=4), chính xác như yêu cầu. 

giá trị`p`lưu trữ (26^t) theo mô đun câu trả lời. Tích hợp sẵn của Python`pow(base, exponent, modulus)`thực hiện phép lũy thừa mô-đun một cách hiệu quả và không bao giờ tạo ra số nguyên khổng lồ (26^t). 

Nghịch đảo của (25) là`280000002`, bởi vì 

[ 
25\cdot280000002\equiv1\pmod{10^9+7}. 
] 

Tử số chuỗi có trọng số được giảm modulo`MOD`trước khi nhân. Các số nguyên trong Python không bị tràn, nhưng việc giảm các giá trị trung gian sẽ giữ cho số học ở mức nhỏ và làm cho cấu trúc mô-đun trở nên rõ ràng. 

Hai thành phần này được tính toán riêng biệt vì tổng ban đầu (k+4) tự nhiên được chia thành một chuỗi hình học có trọng số và bốn bản sao của một chuỗi hình học thông thường. 

## Ví dụ đã hoạt động 

Với (N=3), thuật toán dừng ngay lập tức. 

| (N) | (N<4) | Trả lời | 
| --- | --- | --- | 
| 3 | đúng | 0 | 

Không có từ có bốn ký tự nên không có sự xuất hiện bên trong. Sự chuyển tiếp từ`zzz`ĐẾN`aaaa`không thể sản xuất`icpc`và ranh giới ngắn hơn không thể chứa bốn ký tự bắt buộc. Điều này xác nhận việc xử lý giới hạn dưới. 

Với (N=5), có các đóng góp từ độ dài bốn và năm. 

| (L) | Nội bộ | Vượt qua | Tổng cộng | 
| --- | --- | --- | --- | 
| 4 | (1\cdot26^0=1) | (3\cdot26^0=3) | 4 | 
| 5 | (2\cdot26^1=52) | (3\cdot26^1=78) | 130 | 
| | | | **134** | 

Sau khi thay (k=L-4), ta có (t=2), vậy 

# 4+5\cdot26 

1. 

] 

Dấu vết xác nhận cả hai phần của đối số đếm. Độ dài bốn đóng góp bốn lần xuất hiện, trong khi độ dài năm đóng góp 130, mang lại đầu ra mẫu cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\log N)) | Một phép tính lũy thừa mô-đun (26^{N-3}) | 
| Không gian | (O(1)) | Chỉ một số lượng số nguyên mô-đun không đổi được lưu trữ | 

(N) lớn nhất có thể là (10^9), do đó việc lặp qua tất cả các độ dài sẽ quá chậm. Phép lũy thừa nhị phân chỉ cần khoảng 30 bước bình phương cho một số mũ như vậy, làm cho giải pháp phù hợp một cách thoải mái với các giới hạn đã nêu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 10**9 + 7
INV25 = 280000002

def solution():
    input = sys.stdin.readline
    n = int(input())

    if n < 4:
        print(0)
        return

    t = n - 3
    p = pow(26, t, MOD)

    geometric = (p - 1) % MOD
    geometric = geometric * INV25 % MOD

    weighted_num = (
        26
        - (t % MOD) * p
        + ((t - 1) % MOD) * p % MOD * 26
    ) % MOD

    weighted = weighted_num * INV25 % MOD * INV25 % MOD

    print((weighted + 4 * geometric) % MOD)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solution()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def brute(n: int) -> int:
    # Independent reference for small n.
    words = []

    for length in range(1, n + 1):
        total = 26 ** length
        for x in range(total):
            chars = []
            y = x
            for _ in range(length):
                chars.append(chr(ord('a') + y % 26))
                y //= 26
            words.append(''.join(reversed(chars)))

    s = ''.join(words)
    return sum(s[i:i + 4] == 'icpc' for i in range(len(s) - 3))

def fast_reference(n: int) -> int:
    if n < 4:
        return 0

    ans = 0
    power = 1

    for length in range(4, n + 1):
        ans = (ans + length * power) % MOD
        power = power * 26 % MOD

    return ans

# Provided samples
assert run("3\n") == "0", "sample 1"
assert run("5\n") == "134", "sample 2"

# Minimum size
assert run("1\n") == "0", "minimum N"

# Boundary where the first occurrences appear
assert run("4\n") == "4", "first non-zero case"

# Small case with an independent brute-force reference
assert run("6\n") == str(brute(6)), "small brute-force cross-check"

# Uniform boundary words such as zzz...z -> aaa...a must contribute nothing
assert run("3\n") == "0", "uniform word boundary"

# Maximum allowed N, checked against a direct O(N) modular reference.
# This reference is used only by the test harness, not by the submitted solution.
assert run("1000000000\n") == str(fast_reference(1000000000)), "maximum N"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3`|`0`| Phạm vi tối thiểu nơi mẫu không thể vừa | 
|`4`|`4`| Trường hợp khác 0 đầu tiên và cả ba lần phân chia ranh giới | 
|`5`|`134`| Đóng góp cả nội bộ và chéo | 
|`6`|`4190`| Kiểm tra chéo vũ phu độc lập | 
|`1000000000`| Tính toán theo tham chiếu mô-đun | Ràng buộc tối đa và lũy thừa logarit | 

các`N=4`trường hợp đặc biệt hữu ích vì giải pháp chỉ đếm số lần xuất hiện bên trong các từ trả về`1`, trong khi câu trả lời đúng là`4`. các`N=6`trường hợp so sánh với một từ điển được tạo thực tế, do đó, nó kiểm tra toàn bộ đạo hàm tổ hợp thay vì chỉ đơn thuần là một cách triển khai khác của công thức cuối cùng. Kiểm tra kích thước tối đa xác minh rằng việc triển khai không vô tình lặp lại (N) hoặc xây dựng bất kỳ từ điển nào. 

## Vỏ cạnh 

Với (N=3), đầu vào là```
3
```Thuật toán đi vào`n < 4`chi nhánh và lợi nhuận`0`. Không có từ điển nào có bốn ký tự. Quá trình chuyển đổi độ dài duy nhất có khả năng trông đáng ngờ là`zzz`theo sau là`aaaa`, nhưng ranh giới đó được tạo từ`z`Và`a`, nên nó không thể chứa`icpc`. 

Với (N=4), đầu vào là```
4
```Ở đây (t=1), (26^t=26) và tổng nhân là (1). Tổng hình học có trọng số là (0), vì số hạng duy nhất của nó là (0\cdot26^0). Biểu thức cuối cùng là (0+4\cdot1=4). Bốn lần xuất hiện này bao gồm một lần xuất hiện bên trong từ`icpc`và ba lần vượt qua sau`cpci`,`pcic`, Và`cicp`. 

Với (N=5), hai độ dài liên quan đóng góp (4) và (130). Ở độ dài bốn, tổng số là (4). Ở độ dài 5, có (2\cdot26=52) lần xuất hiện bên trong và (3\cdot26=78) lần xuất hiện giao nhau. Tổng của chúng là (134), khớp với mẫu. 

Để có đầu vào tối đa```
1000000000
```thuật toán không bao giờ cố gắng xây dựng một từ hoặc lặp qua (10^9) độ dài có thể. Nó đặt (t=999999997), tính toán (26^t) với lũy thừa mô-đun trong phép nhân (O(\log N)) và đánh giá hai tổng dạng đóng. Số mũ lớn nhưng biểu diễn nhị phân của nó chỉ có khoảng 30 bit, do đó tính toán vẫn có kích thước không đổi trong thực tế. 

Ranh giới mang theo từ điển được xử lý hoàn toàn bằng đối số tổ hợp. Đối với ba hình dạng ranh giới hữu ích, từ bên trái phải kết thúc bằng`i`,`c`, hoặc`p`, không cái nào trong số đó là`z`. Do đó, số gia của từ điển tiếp theo chỉ thay đổi ký tự cuối cùng và không thể thay đổi tiền tố được yêu cầu. Điều đặc biệt`z...z`ĐẾN`a...a`ranh giới được xử lý riêng biệt và đóng góp bằng không.
