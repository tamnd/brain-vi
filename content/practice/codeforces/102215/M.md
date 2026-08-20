---
title: "CF 102215M - Shlakoblock đã hoạt động!"
description: "Có (n) trò chơi. Trò chơi (i) hiện có (vi) phiếu bầu và việc xem trò chơi đó mang lại niềm vui (pi). Chúng tôi có thể bỏ phiếu cho bất kỳ trò chơi nào nhiều nhất một lần."
date: "2026-08-20T03:04:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102215
codeforces_index: "M"
codeforces_contest_name: "2019, XII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102215
solve_time_s: 451
verified: false
draft: false
---

[CF 102215M - Shlakoblock đã hoạt động!](https://codeforces.com/problemset/problem/102215/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 31 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Có (n) trò chơi. Trò chơi (i) hiện có (v_i) phiếu bầu và xem trò chơi đó mang lại cảm giác thích thú (p_i). Chúng tôi có thể bỏ phiếu cho bất kỳ trò chơi nào nhiều nhất một lần. Sau cuộc bỏ phiếu của chúng tôi, một phiếu bầu được chọn thống nhất từ ​​tất cả các phiếu bầu, do đó, một trò chơi có (x) phiếu bầu cuối cùng sẽ được chọn với xác suất (x) chia cho tổng số phiếu bầu. 

Giả sử chúng ta chọn một bộ (S) trò chơi. hãy để 

[ 
V=\sum_{i=1}^{n}v_i,\qquad 
A=\sum_{i=1}^{n}v_i p_i. 
] 

Trước cuộc bỏ phiếu của chúng ta, mức độ hài lòng mong đợi là (A/V). Nếu chúng ta bỏ phiếu cho mọi trò chơi trong (S), tổng số phiếu bầu sẽ trở thành (V+|S|), trong khi tổng số phiếu bầu theo mức độ hài lòng sẽ trở thành 

[ 
A+\sum_{i\in S}p_i. 
] 

Do đó, niềm vui mong đợi đối với (S) là 

[ 
\frac{A+\sum_{i\in S}p_i}{V+|S|}. 
] 

Đầu ra phải chứa giá trị kỳ vọng tối đa này dưới dạng phân số tối giản, theo sau là số lượng trò chơi chúng tôi đã chọn và chỉ số của chúng. 

Các ràng buộc đủ nhỏ để sắp xếp nhưng quá lớn để liệt kê các tập hợp con. Với (n\le 1000), giải pháp (O(n^2)) dễ dàng thực tế và giải pháp (O(n\log n)) có nhiều chỗ trong giới hạn 2 giây. Các trường hợp thử nghiệm (500) không thay đổi kết luận này vì tổng kích thước đầu vào vẫn bị giới hạn bởi tổng tương ứng của (n) trong dữ liệu thử nghiệm thực tế và thuật toán chỉ cần xử lý mỗi trò chơi một số lần nhỏ. 

Có một số trường hợp việc thực hiện bất cẩn có thể thất bại. Đầu tiên, không được phép chọn trò chơi nào. Vì```
1
1
5 10
```niềm vui được mong đợi đã là (5) và việc bỏ phiếu cho trò chơi duy nhất khiến kỳ vọng không thay đổi. Một đầu ra tối ưu có thể```
5/1
0
```Việc triển khai giả định ít nhất một trò chơi phải được chọn sẽ hạn chế câu trả lời một cách không cần thiết. 

Vấn đề thứ hai là một trò chơi hiện tại không có phiếu bầu nào vẫn có thể là trò chơi hay nhất để thêm vào. Vì```
1
2
0 0
10 1
```kỳ vọng ban đầu là (10). Việc chọn trò chơi (1) sẽ thay đổi kỳ vọng thành (5), trong khi chọn trò chơi (2) sẽ thay đổi kỳ vọng thành (10). Cả hai lựa chọn đều tối ưu, bao gồm cả việc không chọn gì. Một giải pháp chỉ xem xét các trò chơi có (v_i>0) có thể bỏ lỡ lựa chọn tối ưu hợp lệ khi trò chơi không có phiếu bầu có cùng mức độ hài lòng như mong đợi hiện tại. 

Trường hợp cạnh quan trọng nhất liên quan đến mẫu số. Vì```
1
2
10 1
0 1
```niềm vui mong đợi ban đầu là (5). Thêm trò chơi (1) được (20/3), còn thêm trò chơi (2) được (10/3). Đáp án đúng là (20/3). Một phương pháp chỉ so sánh (p_i/v_i), thay vì tác dụng thực tế của việc thêm một phiếu bầu, đang giải quyết một vấn đề khác. 

Cuối cùng, câu trả lời phải được giảm bớt. Vì```
1
2
6 1
2 1
```kỳ vọng ban đầu là (4) và việc chọn một trong hai trò chơi sẽ giữ kỳ vọng ở (4). Câu trả lời phải được in dưới dạng`4/1`, không`8/2`hoặc một phân số tương đương khác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi tập hợp con của trò chơi. Đối với một tập hợp con được chọn (S), chúng ta có thể tính toán mức độ hài lòng mong đợi của nó bằng cách sử dụng 

[ 
\frac{A+\sum_{i\in S}p_i}{V+|S|}. 
] 

Điều này đúng vì mọi quyết định biểu quyết có thể có đều được thể hiện bằng chính xác một tập hợp con. Vấn đề là số lượng tập hợp con. Có (2^n) trong số chúng và ngay cả khi mỗi tập hợp con được đánh giá trong thời gian (O(1)) bằng cách sử dụng tiền xử lý phù hợp, thì trường hợp xấu nhất với (n=1000) sẽ yêu cầu (2^{1000}) thao tác, điều này hoàn toàn không khả thi. 

Nhận xét hữu ích là mẫu số chỉ phụ thuộc vào số lượng trò chơi được chọn chứ không phụ thuộc vào danh tính của chúng. Cố định số lượng trò chơi đã chọn là (k). Khi đó mọi ứng cử viên đều có cùng mẫu số (V+k) và giá trị ban đầu (A) cũng cố định. Phần duy nhất chúng tôi có thể cải thiện là 

[ 
\sum_{i\in S}p_i. 
] 

Đối với chính xác (k) trò chơi được chọn, tổng này được tối đa hóa bằng cách lấy (k) giá trị khoái cảm lớn nhất. Không có lý do gì để chọn một giá trị hài lòng nhỏ hơn trong khi loại trừ một giá trị lớn hơn, bởi vì cả hai lựa chọn đều thêm chính xác một phiếu bầu và ảnh hưởng đến mẫu số như nhau. 

Điều này biến vấn đề thành tìm kiếm một chiều. Sắp xếp các trò chơi theo mức độ giảm dần (p_i), tính tổng tiền tố về mức độ hài lòng của chúng và đánh giá 

[ 
\frac{A+P_k}{V+k} 
] 

với mọi (k) từ (0) đến (n), trong đó (P_k) là tổng của (k) niềm vui đầu tiên. Chúng tôi chỉ đơn giản giữ lại phần tốt nhất. 

Lực lượng vũ phu hoạt động vì nó kiểm tra mọi tập hợp con có thể, nhưng không thành công vì có nhiều tập hợp con theo cấp số nhân. Nhận xét rằng chỉ số lượng trò chơi được chọn mới quan trọng đối với mẫu số cho phép chúng ta thay thế tất cả các tập hợp con có cùng kích thước bằng một đại diện, cụ thể là tập hợp chứa (k) niềm vui lớn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^n n)) hoặc (O(2^n)) với quá trình xử lý trước tập hợp con | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các trò chơi và tính tổng số phiếu bầu (V) hiện tại và mức độ hài lòng có trọng số hiện tại (A=\sum v_i p_i). Những giá trị này mô tả kỳ vọng trước khi chúng tôi thêm bất kỳ phiếu bầu nào. 
2. Sắp xếp trò chơi theo thứ tự giảm dần (p_i). Chỉ có thứ tự của niềm vui mới quan trọng để quyết định nên chọn trò chơi nào. Số phiếu bầu hiện có (v_i) đã được tính đầy đủ ở (A) và (V). 
3. Bắt đầu với (k=0). Ứng cử viên tương ứng là quyết định bỏ phiếu không có trò chơi, có giá trị (A/V). Việc thêm (k=0) là cần thiết vì việc thêm phiếu bầu có thể làm giảm kỳ vọng. 
4. Quét các trò chơi được sắp xếp từ mức độ hài lòng lớn nhất đến nhỏ nhất và duy trì tổng mức độ hài lòng tiền tố (P_k). Sau khi xử lý (k) trò chơi đầu tiên, câu trả lời tốt nhất có thể có trong số tất cả các lựa chọn chứa chính xác (k) trò chơi là 

[ 
\frac{A+P_k}{V+k}. 
] 

1. So sánh ứng viên này với giá trị tốt nhất được tìm thấy cho đến nay bằng cách sử dụng phép nhân chéo. Đối với hai phân số (a/b) và (c/d), hãy so sánh (a d) với (c b). Điều này tránh các vấn đề về độ chính xác của dấu phẩy động và đưa ra so sánh chính xác. 
2. Khi có thí sinh giỏi hơn, hãy lưu (k) của nó. Các trò chơi được chọn chính xác là (k) trò chơi đầu tiên theo thứ tự được sắp xếp, do đó không cần phải xây dựng lại tập hợp con riêng biệt. 
3. Tính lại tử số và mẫu số cho (k) đã lưu, chia cả hai cho ước số chung lớn nhất của chúng và in phân số rút gọn. Sau đó in (k) đã lưu và các chỉ số gốc tương ứng. 

Tại sao nó hoạt động: với mỗi (k) cố định, mẫu số (V+k) là cố định, do đó việc tối đa hóa giá trị mong đợi tương đương với việc tối đa hóa niềm vui gia tăng. Các giá trị (k) lớn nhất (p_i) mang lại sự hài lòng gia tăng tối đa có thể có trong số tất cả các tập hợp con phần tử (k). Do đó, quá trình quét sẽ xem xét tập hợp con tốt nhất có thể cho mọi lực lượng có thể có (k). Vì mọi tập hợp con hợp lệ đều có một số lượng số nằm trong khoảng từ (0) đến (n), nên một trong các tập hợp con này là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())

        games = []
        total_votes = 0
        weighted_pleasure = 0

        for idx in range(1, n + 1):
            p, v = map(int, input().split())
            games.append((p, idx))
            total_votes += v
            weighted_pleasure += p * v

        # For a fixed number k of new votes, choose the k largest pleasures.
        games.sort(reverse=True)

        best_k = 0
        best_num = weighted_pleasure
        best_den = total_votes

        prefix = 0

        for k, (p, idx) in enumerate(games, 1):
            prefix += p

            num = weighted_pleasure + prefix
            den = total_votes + k

            # num / den > best_num / best_den
            if num * best_den > best_num * den:
                best_num = num
                best_den = den
                best_k = k

        selected = [games[i][1] for i in range(best_k)]

        g = math.gcd(best_num, best_den)
        best_num //= g
        best_den //= g

        out.append(f"{best_num}/{best_den}")
        out.append(str(best_k))
        out.append(" ".join(map(str, selected)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên tính toán (V) và (A) trong khi lưu trữ niềm vui và chỉ số ban đầu của mỗi trò chơi. Chỉ mục gốc được giữ lại vì việc sắp xếp thay đổi thứ tự, nhưng đầu ra phải tham chiếu đến vị trí đầu vào. 

Sắp xếp theo thứ tự ngược lại đặt những niềm vui lớn nhất lên hàng đầu. Python sắp xếp các bộ dữ liệu theo từ điển, vì vậy`(p, idx)`với`reverse=True`cũng đảo ngược chỉ số khi niềm vui bằng nhau. Điều đó không ảnh hưởng đến tính đúng đắn vì những niềm vui bình đẳng có thể hoán đổi cho nhau. 

Quá trình quét bắt đầu với ứng cử viên (k=0). Đối với mỗi trò chơi mới được đưa vào,`prefix`trở thành (P_k), do đó tử số ứng cử viên là`weighted_pleasure + prefix`và mẫu số của nó là`total_votes + k`. 

Việc so sánh sử dụng phép nhân chứ không phải`/`. Số nguyên Python có độ chính xác tùy ý, do đó, ngay cả các sản phẩm như`num * best_den`đều được xử lý chính xác. Điều này tránh được cả mối lo ngại về làm tròn dấu phẩy động và tràn. 

Các chỉ số được chọn là chỉ số đầu tiên`best_k`trò chơi sau khi sắp xếp. Cuối cùng,`math.gcd`làm giảm phân số chính xác. Khi`best_k`bằng 0, danh sách đã chọn trống và dòng đầu ra cuối cùng đơn giản là trống, điều này hợp lệ vì (k=0). 

## Ví dụ đã hoạt động 

Trường hợp thử nghiệm đầu tiên có năm trò chơi. Ban đầu, 

[ 
V=5+7+3+2+4=21 
] 

và 

[ 
A=10\cdot5+4\cdot7+6\cdot3+8\cdot2+2\cdot4=132. 
] 

Sau khi sắp xếp theo sở thích, các trò chơi sẽ xuất hiện dưới dạng (10,8,6,4,2). 

| (k) | Thêm niềm vui trò chơi | Tiền tố (P_k) | Tử số | Mẫu số | Giá trị | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 132 | 21 | (132/21=44/7) | 
| 1 | 10 | 10 | 142 | 22 | (71/11) | 
| 2 | 8 | 18 | 150 | 23 | (150/23) | 
| 3 | 6 | 24 | 156 | 24 | (6) | 
| 4 | 4 | 28 | 160 | 25 | (32/5) | 
| 5 | 2 | 30 | 162 | 26 | (13/8) | 

Ứng cử viên tốt nhất là (k=3), có giá trị (6). Tuy nhiên, đầu ra mẫu cũng chọn trò chơi (1) và (4), cho ra (150/25=6). Điều này minh họa tại sao có thể tồn tại nhiều tập hợp con tối ưu. Trong quá trình triển khai ở trên, ứng cử viên đầu tiên tốt hơn sẽ được giữ lại, do đó kết quả đầu ra cũng hợp lệ ngay cả khi các chỉ số được chọn của nó khác với mẫu. 

Đối với trường hợp thử nghiệm thứ hai, 

[ 
V=1000+100+10+1=1111 
] 

và 

[ 
A=1\cdot1000+10\cdot100+100\cdot10+1000\cdot1=4000. 
] 

Những niềm vui đã được sắp xếp theo thứ tự tăng dần, do đó việc sắp xếp sẽ tạo ra (1000,100,10,1). 

| (k) | Thêm niềm vui trò chơi | Tiền tố (P_k) | Tử số | Mẫu số | Giá trị | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 4000 | 1111 | (4000/1111) | 
| 1 | 1000 | 1000 | 5000 | 1112 | (625/139) | 
| 2 | 100 | 1100 | 5100 | 1113 | (1700/371) | 
| 3 | 10 | 1110 | 5110 | 1114 | (2555/557) | 
| 4 | 1 | 1111 | 5111 | 1115 | (5111/1115) | 

Mức tối đa xảy ra ở (k=3), tương ứng với các trò chơi ban đầu có thú vui (10.100.1000), cụ thể là trò chơi (2,3,4). Phân số thu được là 

[ 
\frac{5110}{1114}=\frac{2555}{557}. 
] 

Dấu vết cũng cho thấy tại sao việc tham gia tất cả các trò chơi không tự động tối ưu. Trò chơi cuối cùng có niềm vui (1), quá thấp để bù đắp cho mẫu số bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | Việc sắp xếp chiếm ưu thế trong quá trình quét tuyến tính | 
| Không gian | (O(n)) | Các trò chơi và chỉ số đã chọn được lưu trữ | 

Đối với (n\le1000), việc sắp xếp tại (O(n\log n)) dễ dàng nằm trong giới hạn 2 giây. Thuật toán chỉ thực hiện một số phép toán số nguyên không đổi cho mỗi trò chơi sau khi sắp xếp và các số nguyên có độ chính xác tùy ý của Python làm cho phép so sánh phân số trở nên chính xác mà không gây ra mối lo ngại về bộ nhớ thực tế cho các giới hạn này. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm bên dưới kiểm tra giải pháp về mặt ngữ nghĩa thay vì yêu cầu một tập hợp con tối ưu cụ thể. Điều này là cần thiết vì bài toán rõ ràng cho phép bất kỳ câu trả lời tối ưu nào. Nó xác minh rằng phần được báo cáo đã giảm đi, các chỉ số khác biệt và hợp lệ, đồng thời giá trị kỳ vọng được báo cáo là tối ưu toàn cầu.```python
import sys
import io
import math
from fractions import Fraction

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())

        games = []
        total_votes = 0
        weighted_pleasure = 0

        for idx in range(1, n + 1):
            p, v = map(int, input().split())
            games.append((p, idx))
            total_votes += v
            weighted_pleasure += p * v

        games.sort(reverse=True)

        best_k = 0
        best_num = weighted_pleasure
        best_den = total_votes

        prefix = 0

        for k, (p, idx) in enumerate(games, 1):
            prefix += p
            num = weighted_pleasure + prefix
            den = total_votes + k

            if num * best_den > best_num * den:
                best_num = num
                best_den = den
                best_k = k

        selected = [games[i][1] for i in range(best_k)]

        g = math.gcd(best_num, best_den)
        best_num //= g
        best_den //= g

        out.append(f"{best_num}/{best_den}")
        out.append(str(best_k))
        out.append(" ".join(map(str, selected)))

    sys.stdout.write("\n".join(out))

def validate(inp: str):
    output = solve_data(inp).strip("\n")
    lines = output.splitlines()

    data = inp.split()
    pos = 0
    t = int(data[pos])
    pos += 1

    line_pos = 0

    for case in range(t):
        n = int(data[pos])
        pos += 1

        games = []
        total_votes = 0
        weighted = 0

        for idx in range(1, n + 1):
            p = int(data[pos])
            v = int(data[pos + 1])
            pos += 2
            games.append((p, v))
            total_votes += v
            weighted += p * v

        frac = lines[line_pos]
        line_pos += 1

        num_s, den_s = frac.split("/")
        num = int(num_s)
        den = int(den_s)

        assert den > 0
        assert math.gcd(num, den) == 1

        k = int(lines[line_pos])
        line_pos += 1

        indices = []
        if line_pos < len(lines):
            current = lines[line_pos].strip()
            if current:
                indices = list(map(int, current.split()))
        line_pos += 1

        assert len(indices) == k
        assert len(set(indices)) == k
        assert all(1 <= x <= n for x in indices)

        actual_num = weighted + sum(games[i - 1][0] for i in indices)
        actual_den = total_votes + k

        assert Fraction(num, den) == Fraction(actual_num, actual_den)

        best = Fraction(weighted, total_votes)
        for mask in range(1 << n) if n <= 10 else []:
            s = 0
            cnt = 0
            for i in range(n):
                if mask >> i & 1:
                    s += games[i][0]
                    cnt += 1
            best = max(best, Fraction(weighted + s, total_votes + cnt))

        if n <= 10:
            assert Fraction(num, den) == best

sample = """\
2
5
10 5
4 7
6 3
8 2
2 4
4
1 1000
10 100
100 10
1000 1
"""

validate(sample)

validate("""\
1
1
5 10
""")

validate("""\
1
2
0 0
10 1
""")

validate("""\
1
2
10 1
0 1
""")

validate("""\
1
3
7 1000
7 0
7 1
""")

# Maximum-size case. All games have the same pleasure, so k = 0 is optimal.
maximum_case = "1\n1000\n" + "\n".join(["500 1"] * 1000) + "\n"
validate(maximum_case)

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 5 10`|`5/1`, (k=0) | Kích thước tối thiểu và khả năng không chọn gì | 
|`2 / (0,0),(10,1)`|`10/1`với (k=0) hoặc ván thứ hai | Không có phiếu bầu hiện tại và mức tối ưu không thay đổi | 
|`2 / (10,1),(0,1)`|`20/3`với trò chơi 1 | Sắp xếp đúng thứ tự theo niềm vui và so sánh phân số | 
|`3 / (7,1000),(7,0),(7,1)`|`7/1`| Niềm vui bình đẳng và trò chơi không bầu cử | 
| 1000 trò chơi với`(500,1)`|`500/1`với (k=0) | Giá trị tối đa (n), lặp lại và ranh giới quét tuyến tính | 

Mẫu được cung cấp sẽ được người xác thực kiểm tra mà không yêu cầu chỉ số mẫu chính xác vì cho phép một tập hợp con tối ưu khác. Trường hợp kích thước tối đa xác nhận rằng việc triển khai xử lý tất cả (1000) trò chơi mà không cần dựa vào kích thước đầu vào nhỏ. 

## Vỏ cạnh 

Khi chọn không có trò chơi nào là tối ưu, thuật toán sẽ xử lý nó bằng cách khởi tạo`best_k = 0`trước khi quét bất kỳ trò chơi nào. Đối với đầu vào```
1
1
5 10
```chúng ta có (A=50) và (V=10), vì vậy giá trị ban đầu là (50/10=5). Thêm trò chơi duy nhất sẽ cho kết quả (55/11=5), bằng nhau chứ không phải tốt hơn. Bởi vì mã chỉ cập nhật trên một cải tiến nghiêm ngặt nên nó giữ (k=0) và in`5/1`. 

Trò chơi không có phiếu bầu không yêu cầu xử lý đặc biệt. Coi như```
1
2
0 0
10 1
```Giá trị ban đầu là (10/1=10). Sau khi sắp xếp, chuỗi khoái cảm là (10,0). Ứng cử viên đầu tiên là (20/2=10), vì vậy nó liên kết giá trị ban đầu và không thay thế nó. Ứng cử viên thứ hai là (20/3), tệ hơn. Câu trả lời vẫn còn`10/1`không có trò chơi nào được chọn. Một trò chơi không có phiếu bầu vẫn xuất hiện trong quá trình quét, nhưng sự thú vị của nó được đánh giá giống hệt như mọi trò chơi khác. 

Sự lựa chọn phải dựa trên niềm vui chứ không phải dựa trên số phiếu bầu hiện tại. Vì```
1
2
10 1
0 1
```chúng ta có (A=10) và (V=2), đưa ra kỳ vọng ban đầu là (5). Chọn trò chơi một cách hài lòng (10) sẽ tạo ra (20/3), trong khi chọn trò chơi một cách hài lòng (0) sẽ tạo ra (10/3). Việc sắp xếp theo sở thích sẽ đặt đúng trò chơi lên hàng đầu và quá trình quét sẽ chọn trò chơi đó. 

Giá trị bằng nhau cũng yêu cầu so sánh nghiêm ngặt. Vì```
1
3
7 1000
7 0
7 1
```chúng ta có (A=7007) và (V=1001), vì vậy kỳ vọng ban đầu chính xác là (7). Mỗi phiếu được thêm vào cũng có niềm vui (7), vì vậy với mọi (k), 

[ 
\frac{7007+7k}{1001+k}=7. 
] 

Thuật toán giữ nguyên (k=0), mặc dù việc chọn số lượng trò chơi bất kỳ cũng sẽ là tối ưu. Đây là lý do tại sao sử dụng`>`còn hơn là`>=`thuận tiện: nó đưa ra một ưu tiên xác định cho lựa chọn trống khi tất cả các ứng cử viên hòa nhau. 

Cuối cùng, việc giảm phân số được thực hiện sau khi tìm được (k) tối ưu. Đối với giá trị tối ưu của mẫu đầu tiên (156/24), ước số chung lớn nhất là (24), do đó kết quả in ra là`6/1`. Giữ tất cả số học dưới dạng số nguyên cho đến lần giảm cuối cùng này sẽ tránh được các lỗi chính xác và làm cho mọi so sánh trở nên chính xác.
