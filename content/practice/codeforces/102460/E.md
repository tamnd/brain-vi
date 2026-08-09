---
title: "CF 102460E - Liên đoàn các nhà thiết kế trình tự"
description: "Chúng ta cần xây dựng một mảng số nguyên chứ không phải phân tích một mảng đã cho sẵn. Đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được chênh lệch mục tiêu (k) và độ dài tối thiểu cho phép (L)."
date: "2026-08-08T10:06:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102460
codeforces_index: "E"
codeforces_contest_name: "2019-2020 ICPC Asia Taipei-Hsinchu Regional Contest"
rating: 0
weight: 102460
solve_time_s: 256
verified: true
draft: false
---

[CF 102460E - Liên đoàn các nhà thiết kế trình tự](https://codeforces.com/problemset/problem/102460/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một mảng số nguyên chứ không phải phân tích một mảng đã cho sẵn. Đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được chênh lệch mục tiêu (k) và độ dài tối thiểu cho phép (L). Chúng ta phải xuất ra một mảng có độ dài ít nhất là (L), có độ dài dưới 2000 và các phần tử có giá trị tuyệt đối nhiều nhất là (10^6). Giá trị đúng của mảng là giá trị tối đa trên mỗi đoạn liền kề của độ dài nhân với tổng phần tử của nó. Chương trình của Natasha tính toán một giá trị khác vì nó đặt lại số tiền hiện tại bất cứ khi nào số tiền đó trở thành số âm. Hai giá trị phải khác nhau chính xác (k). 

Việc hạn chế độ dài ở đây cực kỳ hữu ích. Vì mọi mảng hợp lệ có độ dài tối đa là 1999, nên chúng ta có thể chọn cùng độ dài 1999 cho mọi trường hợp thử nghiệm khả thi. Tình huống bất khả thi duy nhất là (L \ge 2000), vì không có mảng hợp lệ nào có thể đủ dài. Các giá trị của (k) đạt tới (10^9), do đó việc xây dựng phải hoạt động theo đại số thay vì tìm kiếm trong các mảng ứng cử viên. May mắn thay, (k+1999) nhiều nhất là (1.000.001), đủ nhỏ để phân phối cho ít hơn 2000 vị trí mảng trong khi vẫn giữ mọi phần tử ở mức thoải mái dưới (10^6). 

Có một số trường hợp nguy hiểm có thể khiến việc xây dựng bất cẩn bị thất bại. Nếu (L=1999), chúng ta thực sự phải sử dụng độ dài 1999, vì độ dài 2000 bị cấm. Ví dụ, với (k=1,L=1999), việc xây dựng có độ dài 1999 là có thể, trong khi việc xây dựng thêm một phần tử bổ sung một cách mù quáng sẽ trở nên không hợp lệ. Nếu (L=2000), thì kết quả đầu ra đúng là (-1), vì mọi chuỗi pháp lý đều có độ dài tối đa là 1999. Việc triển khai bất cẩn khi sử dụng`if L > 2000`sẽ tuyên bố không chính xác rằng trường hợp này là khả thi. 

Trường hợp khó phát hiện thứ hai là (k=1), chênh lệch nhỏ nhất có thể có. Việc xây dựng không được dựa vào số dư lớn hoặc vào (k) chia hết cho một số nào đó. Đối với (k=1), chúng tôi sử dụng phần tử đầu tiên của (-1), theo sau là các phần tử không âm có tổng là (2000). Sự khác biệt giữa hai câu trả lời vẫn chính xác là một. 

Dấu hiệu của phần tử đầu tiên cũng rất cần thiết. Hãy xem xét mảng (6,-8,7,-42). Quy trình của Natasha loại bỏ tiền tố kết thúc bằng (-8), do đó, nó chỉ xem phân đoạn cuối cùng (7) là phân đoạn dương và trả về 7. Phân đoạn thực tế (6,-8,7) có tổng 5 và độ dài 3, cho kết quả 15. Một cấu trúc chỉ chứa các giá trị không âm không thể tạo ra hành vi này vì việc đặt lại của Natasha sẽ không bao giờ xảy ra. 

## Phương pháp tiếp cận 

Một cách trực tiếp để hiểu vấn đề là bắt đầu từ vũ lực. Nếu một mảng đã được cung cấp sẵn, chúng ta có thể liệt kê từng cặp điểm cuối và tính tổng từng phân đoạn bằng tổng tiền tố. Có (n(n+1)/2) phân đoạn, do đó, đối với độ dài pháp lý tối đa (n=1999), điều này có nghĩa là (1999\cdot2000/2=1.999.000) đánh giá phân đoạn. Điều đó hoàn toàn có thể quản lý được đối với quy mô vấn đề này và rất hữu ích cho việc kiểm tra cách xây dựng ứng viên. Khó khăn là đầu vào không cung cấp cho chúng ta một mảng. Việc tìm kiếm trực tiếp một mảng sẽ yêu cầu thử các giá trị cho tối đa 1999 vị trí, với mỗi vị trí có (2\cdot10^6+1) lựa chọn hợp lệ. Ngay cả trước khi kiểm tra xem sự khác biệt kết quả có phải là (k) hay không, việc tìm kiếm theo độ dài (n) sẽ có khả năng xấp xỉ ((2.000.001)^n), do đó việc xây dựng bằng vũ lực là hoàn toàn không khả thi. 

Quan sát hữu ích là sai lầm của Natasha xảy ra chính xác khi tiền tố phủ định bị loại bỏ mặc dù việc giữ tiền tố âm đó có thể tạo ra giá trị tốt hơn sau khi nhân với độ dài đoạn dài hơn. Chúng ta có thể buộc hành vi này theo cách đơn giản nhất có thể bằng cách đặt phần tử đầu tiên (-1), sau đó làm cho mọi phần tử còn lại không âm. 

Giả sử 1998 phần tử còn lại có tổng (x). Natasha nhìn thấy phần tử đầu tiên (-1), làm cho tổng hiện tại của nó bằng 0 và bắt đầu hậu tố dương từ phần tử thứ hai. Vì mọi giá trị còn lại đều không âm nên phân đoạn tốt nhất của cô ấy là toàn bộ hậu tố. Độ dài của nó là 1998 và tổng của nó là (x), vì vậy câu trả lời của cô ấy là 

[ 
1998x. 
] 

Câu trả lời thực sự có thể sử dụng toàn bộ mảng. Độ dài của nó là 1999 và tổng của nó là (x-1), vì vậy giá trị của nó là 

[ 
1999(x-1). 
] 

Sự khác biệt là 

[ 
1999(x-1)-1998x 
=1999x-1999-1998x 
=x-1999. 
] 

Chúng ta muốn hiệu này bằng (k), do đó toàn bộ vấn đề quy về việc chọn 

[ 
x=k+1999. 
] 

Đó là công trình then chốt. Chúng ta không cần phải kiểm soát từng phân đoạn riêng lẻ vì tất cả các phần tử sau số đầu tiên (-1) đều không âm. Hậu tố dương đầy đủ có tổng lớn nhất có thể và việc mở rộng một đoạn không âm chỉ làm tăng độ dài của nó chứ không bao giờ giảm tổng của nó. 

Chúng ta vẫn phải phân phối (x) giữa các vị trí hậu tố 1998. Một lựa chọn thuận tiện là đặt 

[ 
q=\left\lfloor\frac{x}{1998}\right\rfloor 
] 

ở vị trí hậu tố 1997 đầu tiên và đặt số tiền còn lại vào vị trí cuối cùng. Vì (x\le1.000.001), mọi phần tử kết quả đều thấp hơn nhiều (10^6). Do đó, việc xây dựng thỏa mãn giá trị bị ràng buộc một cách tự động. 

Do đó, việc so sánh là đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng lực lượng vũ phu | (O((2\cdot10^6+1)^n)) ứng viên | (O(n)) | Không thể | 
| Xây dựng đại số | (O(1)) cho mỗi trường hợp thử nghiệm ngoài đầu ra | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Nếu (L\ge2000), in (-1). Mọi mảng hợp lệ đều có độ dài tối đa là 1999, do đó không có độ dài hợp pháp nào có thể thỏa mãn giới hạn dưới. 
2. Ngược lại chọn (n=1999). Điều này tự động thỏa mãn (n\ge L), bao gồm cả trường hợp biên (L=1999). 
3. Đặt phần tử đầu tiên thành (-1). Thuật toán của Natasha sẽ xử lý giá trị này, lấy tổng hiện tại âm, đặt lại tổng về 0 và đặt vị trí bắt đầu của nó sau phần tử này. Điều này tạo ra sự khác biệt chính xác về độ dài của một phần tử mà chúng ta cần. 
4. Tính (x=k+1999). Chúng ta muốn tổng của các phần tử còn lại của 1998 là (x), vì câu trả lời đúng khi đó sẽ là (1999(x-1)), trong khi câu trả lời của Natasha sẽ là (1998x). 
5. Chia (x) cho 1998. Đặt (q=x//1998) và (r=x\bmod1998). Đặt (q) vào mỗi vị trí trong số 1997 vị trí đầu tiên sau (-1) và đặt (q+r) ở vị trí cuối cùng. Tổng số của họ là 

[ 
1997q+(q+r)=1998q+r=x. 
] 

Tất cả các giá trị này đều dương vì (x\ge2000). Do đó, toàn bộ hậu tố là hậu tố tốt nhất cho cả mục tiêu chính xác và mục tiêu của Natasha. 

1. Xuất mảng kết quả. Độ dài của nó là 1999, phần tử đầu tiên của nó là (-1) và tất cả các phần tử khác đều không âm và nhiều nhất là (q+r), nhỏ hơn (10^6). 

### Tại sao nó hoạt động 

Bất biến trung tâm là sau phần tử đầu tiên, mọi giá trị mảng đều dương. Do đó, Natasha đặt lại chính xác một lần, ở vị trí đầu tiên, và sau đó tổng hiện tại của nó tăng lên một cách đơn điệu thông qua toàn bộ hậu tố. Ứng cử viên cuối cùng của cô ấy là (1998x), và không có hậu tố nào sớm hơn có thể tốt hơn vì cả độ dài và tổng của nó đều nhỏ hơn. 

Để có mục tiêu chính xác, toàn bộ mảng có tổng (x-1>0). Bất kỳ phân đoạn nào bắt đầu sau phần tử đầu tiên có độ dài tối đa là 1998 và tổng tối đa là (x), trong khi toàn bộ mảng có độ dài tối đa là 1999 và tổng (x-1). Vì các giá trị hậu tố là dương nên việc thêm chúng lần lượt sẽ làm tăng tích của hậu tố liên quan. Do đó, toàn bộ mảng sẽ cho ra (1999(x-1)) và bất kỳ đoạn nào ngắn hơn sẽ nhỏ hơn. Sự khác biệt chính xác là 

[ 
1999(x-1)-1998x=x-1999=k. 
] 

Do đó, mọi trường hợp kiểm thử khả thi đều nhận được một cấu trúc hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        k, L = map(int, input().split())

        if L >= 2000:
            out.append("-1")
            continue

        n = 1999
        total = k + n

        q, r = divmod(total, 1998)

        a = [-1]
        a.extend([q] * 1997)
        a.append(q + r)

        out.append(str(n))
        out.append(" ".join(map(str, a)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Nhánh đầu tiên xử lý điều kiện không thể duy nhất. sử dụng`L >= 2000`còn hơn là`L > 2000`quan trọng vì hạn chế ban đầu là nghiêm ngặt (n<2000), vì vậy bản thân năm 2000 đã là bất hợp pháp. 

Đối với những trường hợp khả thi,`n`được cố định ở 1999. Biểu thức`total = k + n`chính xác là giá trị được gọi là (x) trong đạo hàm. sử dụng`divmod(total, 1998)`đưa ra cả giá trị chung (q) và phần dư (r) cần thiết để phân phối tổng mà không cần bất kỳ số học dấu phẩy động nào. 

Danh sách bắt đầu bằng`-1`, sau đó chứa 1997 bản sao của`q`, và cuối cùng`q + r`. Do đó có các phần tử (1+1997+1=1999). Giá trị cuối cùng không phải là một yêu cầu toán học đặc biệt. Nó chỉ đơn giản là một cách thuận tiện để hấp thụ phần còn lại trong khi vẫn giữ mọi phần tử hậu tố đều dương. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn. Ngay cả trong ngôn ngữ có chiều rộng cố định, các sản phẩm có liên quan vẫn dễ dàng khớp với số nguyên 64 bit. Bản thân việc xây dựng yêu cầu (O(1999)) hoạt động đầu ra cho mỗi trường hợp thử nghiệm khả thi, điều này là không thể tránh khỏi vì trình tự phải được in. 

## Ví dụ đã hoạt động 

### Mẫu 1: (k=8,\ L=3) 

Ở đây (n=1999) và 

[ 
x=8+1999=2007. 
] 

Chia 2007 cho 1998 được (q=1) và (r=9). Do đó, mảng bắt đầu bằng (-1), có 1997 bản sao là 1 và kết thúc bằng 10. 

Mảng hoàn chỉnh có kích thước lớn nên dấu vết bên dưới nhóm các lần lặp ở giữa giống hệt nhau thay vì in 1999 hàng giống hệt nhau. 

| Bước | Vị trí | Giá trị |`curMax`sau khi cập nhật |`left`| Ứng viên | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | -1 | 0 | 1 | 0 | 
| 2 | 2 | 1 | 1 | 1 | 1 | 
| 3 | 3 | 1 | 2 | 1 | 4 | 
| 4 | 4 | 1 | 3 | 1 | 9 | 
| 5 | 5 đến 1997 | 1 | tăng từ 4 lên 1996 | 1 | tăng tương ứng | 
| 6 | 1998 | 1 | 1997 | 1 | (1997^2) | 
| 7 | 1999 | 10 | 2007 | 1 | (1998\cdot2007) | 

Câu trả lời cuối cùng của Natasha là 

[ 
1998\cdot2007=4,017,? 
] 

Chính xác hơn, 

[ 
2007\cdot1998=4,009,986. 
] 

Câu trả lời đúng sử dụng mảng hoàn chỉnh, có tổng là (2006), cho 

[ 
1999\cdot2006=4.009.994. 
] 

Sự khác biệt của họ là (8), chính xác là yêu cầu (k). Giá trị âm đầu tiên khiến Natasha bỏ sót một phần tử độ dài trong khi chỉ mất một đơn vị tổng, đây chính là điều tạo ra sự khác biệt. 

### Mẫu 2: (k=612,\ L=7) 

bây giờ 

[ 
x=612+1999=2611. 
] 

Chúng ta có (2611=1998\cdot1+613), do đó kết quả là (-1), tiếp theo là kết quả năm 1997, tiếp theo là 614. 

| Bước | Vị trí | Giá trị |`curMax`sau khi cập nhật |`left`| Ứng viên | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | -1 | 0 | 1 | 0 | 
| 2 | 2 | 1 | 1 | 1 | 1 | 
| 3 | 3 | 1 | 2 | 1 | 4 | 
| 4 | 4 | 1 | 3 | 1 | 9 | 
| 5 | 5 đến 1998 | 1 | tăng đến năm 1997 | 1 | tăng lên (1997^2) | 
| 6 | 1999 | 614 | 2611 | 1 | (1998\cdot2611) | 

Natasha có được 

[ 
1998\cdot2611=5,215,? 
] 

và rõ ràng, 

[ 
2611\cdot1998=5,215,778. 
] 

Mảng đầy đủ có tổng (2610), nên đáp án đúng là 

[ 
1999\cdot2610=5,216,? 
] 

đó là 

[ 
5.216,? 
] 

Trực tiếp hơn, sự khác biệt có thể được tính toán mà không cần nhân lớn: 

[ 
1999\cdot2610-1998\cdot2611 
=2611-1999 
=612. 
] 

Dấu vết xác nhận tính bất biến của cấu trúc: Natasha bắt đầu đếm đoạn hữu ích sau đó một vị trí, trong khi đáp án đúng có thể bao gồm phần đầu tiên (-1). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) cho mỗi trường hợp thử nghiệm | Việc xây dựng tạo và in chính xác các giá trị 1999 cho mọi trường hợp khả thi. | 
| Không gian | (O(n)) | Mảng đầu ra chứa 1999 số nguyên. | 

Vì (n) được cố định ở 1999 và (T\le5) nên tổng lượng dữ liệu được xây dựng nhỏ hơn 10.000 số nguyên. Bản thân số học là thời gian không đổi cho mỗi trường hợp thử nghiệm và đầu ra chi phối thời gian chạy. Việc xây dựng vẫn thoải mái trong giới hạn thời gian và bộ nhớ đã nêu. 

## Trường hợp thử nghiệm 

Đầu ra của một vấn đề mang tính xây dựng không phải là duy nhất, vì vậy việc so sánh đầu ra của giải pháp với một chuỗi mẫu theo nghĩa đen sẽ không chính xác. Thay vào đó, khai thác kiểm tra bên dưới sẽ kiểm tra mọi thuộc tính bắt buộc của chuỗi được trả về và tính toán độc lập cả hai mục tiêu.```python
# helper: run solution on input string, return output string
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        t = int(sys.stdin.readline())
        out = []

        for _ in range(t):
            k, L = map(int, sys.stdin.readline().split())

            if L >= 2000:
                out.append("-1")
                continue

            n = 1999
            total = k + n
            q, r = divmod(total, 1998)

            a = [-1]
            a.extend([q] * 1997)
            a.append(q + r)

            out.append(str(n))
            out.append(" ".join(map(str, a)))

        return sys.stdout.getvalue() if False else "\n".join(out)
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def natasha_answer(a):
    result = 0
    cur = 0
    left = 0

    for i, x in enumerate(a, start=1):
        cur += x
        if cur < 0:
            cur = 0
            left = i
        result = max(result, (i - left) * cur)

    return result

def correct_answer(a):
    best = 0

    for l in range(len(a)):
        s = 0
        for r in range(l, len(a)):
            s += a[r]
            best = max(best, (r - l + 1) * s)

    return best

def check_output(inp: str, output: str):
    lines = output.strip().splitlines()
    tokens = inp.strip().split()
    t = int(tokens[0])

    cases = []
    p = 1
    for _ in range(t):
        k = int(tokens[p])
        L = int(tokens[p + 1])
        p += 2
        cases.append((k, L))

    pos = 0

    for k, L in cases:
        if L >= 2000:
            assert pos < len(lines)
            assert lines[pos].strip() == "-1"
            pos += 1
            continue

        assert pos + 1 < len(lines)

        n = int(lines[pos])
        a = list(map(int, lines[pos + 1].split()))
        pos += 2

        assert n == len(a)
        assert 1 <= n < 2000
        assert n >= L
        assert all(abs(x) <= 10**6 for x in a)

        good = correct_answer(a)
        bad = natasha_answer(a)

        assert good - bad == k

def run(inp: str) -> str:
    return solve_data(inp)

# Provided samples
sample_in = """3
8 3
612 7
4 2019
"""
sample_out = run(sample_in)
check_output(sample_in, sample_out)

# Minimum lower bound and minimum k.
inp = """1
1 0
"""
out = run(inp)
check_output(inp, out)

# Maximum feasible length.
inp = """1
1 1999
"""
out = run(inp)
check_output(inp, out)

# Maximum k.
inp = """1
1000000000 1999
"""
out = run(inp)
check_output(inp, out)

# Impossible boundary, L = 2000.
inp = """1
4 2000
"""
out = run(inp)
check_output(inp, out)

# Independent all-equal-value sanity check for the objective functions.
a = [5, 5, 5]
assert correct_answer(a) == 45
assert natasha_answer(a) == 45
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`8 3`,`612 7`,`4 2019`| Hai công trình hợp lệ và một`-1`| Trường hợp mẫu chính thức và độ dài không thể | 
|`1 0`| Một chuỗi có độ dài hợp lệ 1999 | Tối thiểu (L) và tối thiểu (k) | 
|`1 1999`| Một chuỗi có độ dài hợp lệ 1999 | Độ dài pháp lý tối đa chính xác | 
|`1000000000 1999`| Một chuỗi hợp lệ với các phần tử bị chặn | Phạm vi tối đa (k) và số học | 
|`4 2000`|`-1`| Ranh giới giữa khả thi và không thể (L) | 
|`[5,5,5]`trong máy kiểm tra | Cả hai câu trả lời đều bằng 45 | Giá trị hoàn toàn bình đẳng và mục tiêu thực hiện | 

## Vỏ cạnh 

Khi (L=2000), thuật toán sẽ in ngay (-1). Đối với đầu vào`1 2000`, chuỗi được yêu cầu sẽ cần độ dài ít nhất là 2000, nhưng các hạn chế ban đầu chỉ cho phép độ dài từ 1 đến 1999. Không có cấu trúc nào để tìm kiếm, vì vậy việc loại bỏ sớm là cần thiết và đủ. 

Khi (L=1999), thuật toán chọn chính xác (n=1999). Ví dụ, với`1 1999`, nó xây dựng một chuỗi bắt đầu bằng (-1) và có tổng hậu tố (2000). Natasha nhận được (1998\cdot2000), trong khi giá trị đúng là (1999\cdot1999). Sự khác biệt của chúng là (1999\cdot1999-1998\cdot2000=1). Ranh giới hoạt động vì độ dài được chọn chính xác là độ dài hợp pháp lớn nhất. 

Khi (k=1), không có giả định về tính chia hết trong cách xây dựng. Chúng tôi sử dụng (x=2000) và`divmod(2000, 1998)`cho (q=1,r=2). Do đó, chuỗi bao gồm (-1), số 1997 và số 3 cuối cùng. Tổng hậu tố của nó là 2000, do đó hai câu trả lời khác nhau bởi (2000-1999=1). Điều này bắt các công trình vô tình cho rằng (k) có phần dư thuận tiện. 

Trường hợp (k=10^9) kiểm tra ranh giới số. Ở đây (x=1.000.001). Chia cho 1998 sẽ cho một thương số và số dư nhỏ, do đó mọi phần tử được xây dựng vẫn ở dưới mức (10^6). Việc xây dựng không cần đặt toàn bộ (x) vào một phần tử, đó là lý do tại sao phần tử bị ràng buộc không bao giờ trở nên hạn chế. 

Cuối cùng, hãy xem tại sao tất cả các giá trị hậu tố phải dương thay vì chỉ có tổng không âm. Nếu các số 0 được chèn một cách bất cẩn, phân đoạn tốt nhất của Natasha vẫn có thể đúng, nhưng bằng chứng cho thấy hậu tố cuối cùng là tối ưu duy nhất sẽ cần xử lý thêm. Cấu trúc của chúng ta có (x\ge2000) và thương (q) ít nhất là 1, vì vậy mọi phần tử hậu tố đều hoàn toàn dương. Do đó, tổng hiện tại tăng lên ở mỗi lần lặp và cả mục tiêu đúng và mục tiêu của Natasha đều có cực đại liên quan ở vị trí cuối cùng. Điều này loại bỏ sự mơ hồ phổ biến nhất trong quá trình xây dựng.
