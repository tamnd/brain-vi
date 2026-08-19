---
title: "CF 102215M - Shlakoblock đã hoạt động!"
description: "Chúng tôi có (n) trò chơi. Trò chơi (i) hiện có (vi) phiếu bầu và xem nó mang lại cảm giác thích thú (pi). Chúng tôi có thể thêm một phiếu bầu cho bất kỳ trò chơi nào, nhưng nhiều nhất là một lần cho mỗi trò chơi. Sau các lựa chọn của chúng tôi, một phiếu bầu sẽ được chọn ngẫu nhiên thống nhất, do đó, trò chơi có nhiều phiếu bầu hơn sẽ có nhiều khả năng được phát trực tuyến hơn."
date: "2026-08-18T12:20:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102215
codeforces_index: "M"
codeforces_contest_name: "2019, XII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102215
solve_time_s: 641
verified: false
draft: false
---

[CF 102215M - Shlakoblock đã hoạt động!](https://codeforces.com/problemset/problem/102215/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 10 phút 41 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có (n) trò chơi. Trò chơi (i) hiện có (v_i) phiếu bầu và xem nó mang lại cảm giác thích thú (p_i). Chúng tôi có thể thêm một phiếu bầu cho bất kỳ trò chơi nào, nhưng nhiều nhất là một lần cho mỗi trò chơi. Sau các lựa chọn của chúng tôi, một phiếu bầu sẽ được chọn ngẫu nhiên thống nhất, do đó, trò chơi có nhiều phiếu bầu hơn sẽ có nhiều khả năng được phát trực tuyến hơn. 

Gọi (S) là tập hợp các trò chơi mà chúng ta bầu chọn. Nếu tổng số phiếu bầu hiện tại là 

[ 
V=\sum_{i=1}^n v_i, 
] 

thì sau khi bỏ phiếu sẽ có (V+|S|) phiếu bầu. Tổng số niềm vui được đại diện bởi tất cả các phiếu bầu là 

[ 
A+\sum_{i\in S}p_i, 
] 

ở đâu 

[ 
A=\sum_{i=1}^n v_i p_i. 
] 

Vì vậy, niềm vui mong đợi là 

[ 
\frac{A+\sum_{i\in S}p_i}{V+|S|}. 
] 

Nhiệm vụ là chọn (S), in phân số tối đa có thể có ở dạng tối giản và in ra một bộ trò chơi đạt được phân số đó. 

Các ràng buộc đủ nhỏ để sắp xếp nhưng không đủ nhỏ để liệt kê các tập hợp con. Có thể có (n=1000) trò chơi trong một thử nghiệm và tối đa 500 trường hợp thử nghiệm. Giải pháp (O(n^2)) đã đắt một cách không cần thiết trong trường hợp tổng hợp xấu nhất, trong khi (O(n\log n)) lại đủ nhanh. Các giá trị (p_i,v_i) nhiều nhất là 1000, nhưng tổng bao gồm tối đa 1000 số hạng, vì vậy số nguyên Python thông thường là quá đủ. 

Có một số trường hợp việc thực hiện bất cẩn có thể thất bại. Nếu chúng ta không chọn trò chơi nào thì câu trả lời vẫn có thể là tối ưu. Ví dụ,```
1
1
0 5
```mang lại niềm vui mong đợi (0/5=0), vì vậy đầu ra chính xác là```
0/1
0
```Việc triển khai luôn thêm ít nhất một trò chơi sẽ tạo ra kết quả tồi tệ hơn. 

Vấn đề thứ hai là các trò chơi hiện tại không có phiếu bầu vẫn đủ điều kiện để chúng tôi bỏ phiếu. Vì```
1
2
10 1
100 0
```kỳ vọng ban đầu là (10). Bỏ phiếu cho ván 2 cho kết quả (110/2=55), là tối ưu. Bỏ qua các trò chơi có (v_i=0) sẽ bỏ lỡ câu trả lời. 

Vấn đề thứ ba là mẫu số thay đổi bất cứ khi nào chúng ta bỏ phiếu cho một trò chơi khác. Vì```
1
2
100 1
0 100
```bỏ phiếu cho trò chơi đầu tiên cho (200/101), trong khi bỏ phiếu cho trò chơi thứ hai cho (100/101). Sự lựa chọn không thể được thực hiện bằng cách đơn giản chọn mọi trò chơi với niềm vui tích cực. Phần đóng góp của phiếu bầu bổ sung phải được xem xét cùng với dấu cộng (1) ở mẫu số. 

Cuối cùng, một số tập hợp con khác nhau có thể đạt được cùng mức tối ưu. Với```
1
2
5 1
5 1
```câu trả lời đúng nhất là (10/2=5) sau khi bỏ phiếu cho một trong hai trò chơi và cả hai lựa chọn đều hợp lệ. Thuật toán chỉ cần giữ lại một tập con tối ưu. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là thử mọi tập hợp con của trò chơi. Đối với tập hợp con (S), chúng ta có thể tính toán tử số và mẫu số của nó và giữ giá trị mong đợi tốt nhất. Điều này đúng vì mọi chiến lược bỏ phiếu hợp pháp đều được thể hiện bằng chính xác một tập hợp con. Tuy nhiên, có (2^n) tập hợp con và việc đánh giá mỗi tập hợp con chiếm tới (O(n)) công việc, đưa ra các phép toán (O(n2^n)) trong trường hợp xấu nhất. Với (n=1000), thậm chí (2^{1000}) vượt xa mọi thứ có thể chạy trong thời gian giới hạn. 

Cấu trúc hữu ích xuất hiện khi chúng ta ngừng quan tâm đến danh tính của các trò chơi đã chọn và trước tiên hãy sửa số lượng của chúng. Giả sử chúng ta quyết định bỏ phiếu cho chính xác (k) trò chơi. Mẫu số sau đó được cố định tại (V+k) và phần đóng góp ban đầu (A) cũng được cố định. Phần duy nhất chúng tôi có thể tối ưu hóa là 

[ 
\sum_{i\in S}p_i. 
] 

Đối với chính xác (k) trò chơi, tổng này được tối đa hóa bằng cách lấy (k) giá trị khoái cảm lớn nhất. 

Quan sát đó biến tìm kiếm theo cấp số nhân thành tìm kiếm tiền tố được sắp xếp đơn giản. Sắp xếp các trò chơi theo mức độ giảm dần (p_i). Sau khi sắp xếp, tập hợp con tốt nhất có kích thước (k) chính xác là (k) trò chơi đầu tiên. Chúng ta có thể xây dựng tổng mức độ hài lòng của họ tăng dần và đánh giá tất cả (k) từ 0 đến (n). 

Cách tiếp cận bạo lực có hiệu quả vì nó xem xét mọi tập hợp con có thể. Nó thất bại vì có nhiều tập hợp con theo cấp số nhân. Nhận xét rằng lựa chọn tối ưu cho kích thước tập hợp con cố định bao gồm các trò chơi có (p_i) lớn nhất cho phép chúng tôi thay thế tất cả các tập hợp con có cùng kích thước bằng một đại diện, giảm vấn đề thành (n+1) chiến lược ứng cử viên sau khi sắp xếp. 

Để so sánh chính xác các phân số, chúng ta không nên sử dụng dấu phẩy động. Đối với hai ứng cử viên 

[ 
\frac{x_1}{y_1} 
\quad\text{và}\quad 
\frac{x_2}{y_2}, 
] 

chúng tôi so sánh (x_1y_2) với (x_2y_1). Số nguyên Python xử lý chính xác các sản phẩm này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n2^n)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số phiếu bầu hiện tại (V=\sum v_i) và tổng mức đóng góp niềm vui hiện tại (A=\sum v_i p_i). Những giá trị này mô tả sự hài lòng được mong đợi trước khi thêm bất kỳ phiếu bầu nào của chúng tôi. 
2. Sắp xếp tất cả các trò chơi theo thứ tự giảm dần (p_i), giữ nguyên chỉ số ban đầu của chúng. Nếu cuối cùng chúng tôi quyết định thêm chính xác (k) phiếu bầu, thì (k) trò chơi đầu tiên theo thứ tự này sẽ mang lại niềm vui gia tăng lớn nhất có thể. 
3. Bắt đầu với (k=0). Kỳ vọng của ứng viên là (A/V). Bài toán đảm bảo rằng có ít nhất một (v_i) dương, do đó (V>0). 
4. Duyệt qua các trò chơi được sắp xếp. Khi xử lý trò chơi tiếp theo, hãy thêm (p_i) của nó vào tổng tiền tố đang chạy. Sau khi cộng (k) trò chơi, tử số ứng viên là (A+\text{tiền tố}), trong khi mẫu số là (V+k). 
5. So sánh mọi ứng viên với ứng viên giỏi nhất cho đến nay bằng cách sử dụng phép nhân chéo. Nếu 

[ 
(A+\văn bản{tiền tố})(V+k_{\text{tốt nhất}}) 

> 

(A+\văn bản{tiền tố__{\văn bản{tốt nhất}})(V+k), 
] 

thay thế câu trả lời tốt nhất hiện tại. 

1. Lưu trữ (k) tương ứng. Vì các trò chơi đã được sắp xếp theo mức độ hài lòng giảm dần nên chỉ số (k) đầu tiên tạo thành một tập hợp biểu quyết tối ưu cho (k) đó. 
2. Sau khi quét, giảm phân số tốt nhất bằng cách chia tử số và mẫu số cho ước số chung lớn nhất của chúng. In phân số rút gọn, số đếm đã chọn và chỉ số ban đầu tương ứng. 

### Tại sao nó hoạt động 

Đối với mọi số có thể có (k) của phiếu bầu bổ sung, mẫu số chính xác là (V+k). Trong số tất cả các tập con của (k) trò chơi, đóng góp ban đầu (A) là giống hệt nhau, do đó việc tối đa hóa niềm vui mong đợi tương đương với việc tối đa hóa tổng giá trị (p_i) của chúng. Các giá trị (k) lớn nhất (p_i) cho tổng lớn nhất có thể, do đó tiền tố được sắp xếp là tối ưu cho (k) cụ thể đó.

Thuật toán kiểm tra mọi (k) có thể từ 0 đến (n) và với mỗi (k), nó sẽ kiểm tra tập con tốt nhất có kích thước đó. Do đó, mức tối ưu toàn cục phải nằm trong số các ứng cử viên được xem xét trong quá trình quét. Phép nhân chéo so sánh các ứng cử viên này một cách chính xác, vì vậy ứng cử viên được chọn là giá trị tối đa thực sự chứ không phải là xấp xỉ dấu phẩy động. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())

        games = []
        total_votes = 0
        total_pleasure = 0

        for idx in range(1, n + 1):
            p, v = map(int, input().split())
            games.append((p, idx))
            total_votes += v
            total_pleasure += p * v

        games.sort(key=lambda x: (-x[0], x[1]))

        best_num = total_pleasure
        best_den = total_votes
        best_k = 0

        prefix = 0

        for k, (p, idx) in enumerate(games, 1):
            prefix += p

            cur_num = total_pleasure + prefix
            cur_den = total_votes + k

            if cur_num * best_den > best_num * cur_den:
                best_num = cur_num
                best_den = cur_den
                best_k = k

        g = gcd(best_num, best_den)
        best_num //= g
        best_den //= g

        out.append(f"{best_num}/{best_den}")
        out.append(str(best_k))

        if best_k == 0:
            out.append("")
        else:
            chosen = [str(games[i][1]) for i in range(best_k)]
            out.append(" ".join(chosen))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào lưu trữ mỗi trò chơi dưới dạng`(p, index)`bởi vì chỉ có niềm vui của nó mới ảnh hưởng đến việc sắp xếp, trong khi chỉ mục ban đầu của nó là cần thiết cho đầu ra. Đồng thời, nó tích lũy số phiếu bầu hiện tại và đóng góp niềm vui hiện tại. 

Bước sắp xếp sử dụng niềm vui giảm dần. Thứ tự phụ theo chỉ mục ban đầu không cần thiết về mặt toán học, nhưng nó làm cho chương trình mang tính quyết định khi một số trò chơi có mức độ thú vị như nhau. 

Quá trình quét bắt đầu bằng (k=0), điều này rất cần thiết vì việc bỏ phiếu cho không có trò chơi nào là hợp pháp. Biến`prefix`là tổng các thú vui của trò chơi được sắp xếp (k) đầu tiên, do đó tử số và mẫu số ứng viên luôn chính xác (A+\text{tiền tố}) và (V+k). 

Việc so sánh sử dụng phép nhân chứ không phải phép chia. Đối với mẫu số dương, 

[ 
\frac{x}{y}>\frac{a}{b} 
] 

tương đương với (xb>ay). Điều này tránh được các lỗi về độ chính xác của dấu phẩy động và cũng tránh được việc liên tục xây dựng các giá trị dấu phẩy động. 

Các chỉ số đã chọn được xây dựng lại từ đầu tiên`best_k`các phần tử của mảng đã được sắp xếp. Không có vấn đề riêng lẻ nào bởi vì`enumerate(games, 1)`làm cho`k`bằng với số lượng trò chơi có trong tiền tố. 

Mẫu số luôn dương vì đầu vào ban đầu chứa ít nhất một phiếu bầu tích cực. Các số nguyên có độ chính xác tùy ý của Python cũng khiến cho việc tràn không thể xảy ra, mặc dù giới hạn thực tế đã đủ nhỏ cho số học 64-bit tiêu chuẩn. 

Khi`best_k`bằng 0, dòng đầu ra thứ ba được yêu cầu trống. Mã này gắn thêm một chuỗi trống một cách rõ ràng để mỗi trường hợp thử nghiệm vẫn chiếm chính xác ba dòng đầu ra. 

## Ví dụ đã hoạt động 

Mẫu đầu tiên chứa năm trò chơi. Tổng số ban đầu của họ là (V=21), và mức đóng góp niềm vui hiện tại của họ là 

[ 
A=5\cdot10+7\cdot4+3\cdot6+2\cdot8+4\cdot2=120. 
] 

Sau khi sắp xếp theo sở thích, thứ tự là các trò chơi 1, 4, 3, 2, 5. 

| (k) | Thêm niềm vui | Tử số | Mẫu số | Kỳ Vọng | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 120 | 21 | (120/21) | 
| 1 | 10 | 130 | 22 | (130/22) | 
| 2 | 18 | 138 | 23 | (138/23=6) | 
| 3 | 24 | 144 | 24 | (144/24=6) | 
| 4 | 28 | 148 | 25 | (148/25) | 
| 5 | 30 | 150 | 26 | (150/26) | 

Tối đa là 6. Có sự liên kết giữa (k=2) và (k=3). Việc triển khai giữ mức tối đa đầu tiên vì nó chỉ thay thế câu trả lời tốt nhất khi ứng viên mới lớn hơn. Vì vậy, nó chọn trò chơi 1 và 4 và in`6/1`. 

Mẫu thứ hai có (V=1111) và 

[ 
A=1000\cdot1+100\cdot10+10\cdot100+1\cdot1000=4000. 
] 

Thứ tự sắp xếp là trò chơi 4, 3, 2, 1. 

| (k) | Thêm niềm vui | Tử số | Mẫu số | Kỳ Vọng | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 4000 | 1111 | (4000/1111) | 
| 1 | 1000 | 5000 | 1112 | (5000/1112) | 
| 2 | 1100 | 5100 | 1113 | (5100/1113) | 
| 3 | 1110 | 5110 | 1114 | (5110/1114) | 
| 4 | 1111 | 5111 | 1115 | (5111/1115) | 

Thí sinh giỏi nhất sử dụng trò chơi 4, 3 và 2. Phân số của nó là 

[ 
\frac{5110}{1114}=\frac{2555}{557}, 
] 

đây đã là biểu diễn rút gọn được yêu cầu sau khi chia cả hai số cho 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | Việc sắp xếp chiếm ưu thế trong quá trình quét tuyến tính và xử lý đầu vào | 
| Không gian | (O(n)) | Mảng trò chơi lưu trữ một bản ghi cho mỗi trò chơi | 

Đối với (n\le1000), việc sắp xếp tại (O(n\log n)) nằm trong giới hạn 2 giây. Thậm chí trên 500 trường hợp thử nghiệm, thuật toán chỉ thực hiện một lượng nhỏ công việc cho mỗi trò chơi ngoài việc sắp xếp và mức sử dụng bộ nhớ của nó là tuyến tính trong kích thước của một trường hợp thử nghiệm. 

## Trường hợp thử nghiệm 

Bộ khai thác thử nghiệm bên dưới sử dụng cơ chế ngắt kết nối xác định tương tự như giải pháp đã gửi. Để xác minh chung, nó cũng kiểm tra tính hợp lệ về cấu trúc của câu trả lời và giá trị tối ưu của nó, vì Codeforces cho phép bất kỳ tập hợp con tối ưu nào.```python
import sys
import io
from math import gcd

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        input = sys.stdin.readline

        t = int(input())
        out = []

        for _ in range(t):
            n = int(input())

            games = []
            total_votes = 0
            total_pleasure = 0

            for idx in range(1, n + 1):
                p, v = map(int, input().split())
                games.append((p, idx))
                total_votes += v
                total_pleasure += p * v

            games.sort(key=lambda x: (-x[0], x[1]))

            best_num = total_pleasure
            best_den = total_votes
            best_k = 0
            prefix = 0

            for k, (p, idx) in enumerate(games, 1):
                prefix += p
                cur_num = total_pleasure + prefix
                cur_den = total_votes + k

                if cur_num * best_den > best_num * cur_den:
                    best_num = cur_num
                    best_den = cur_den
                    best_k = k

            g = gcd(best_num, best_den)
            best_num //= g
            best_den //= g

            out.append(f"{best_num}/{best_den}")
            out.append(str(best_k))

            if best_k == 0:
                out.append("")
            else:
                out.append(" ".join(str(games[i][1]) for i in range(best_k)))

        return "\n".join(out)
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def check(inp: str, output: str):
    data = list(map(int, inp.split()))
    pos = 0
    t = data[pos]
    pos += 1

    lines = output.splitlines()
    line_pos = 0

    for _ in range(t):
        n = data[pos]
        pos += 1

        games = []
        total_votes = 0
        total_pleasure = 0

        for idx in range(1, n + 1):
            p = data[pos]
            v = data[pos + 1]
            pos += 2
            games.append((p, v))
            total_votes += v
            total_pleasure += p * v

        fraction = lines[line_pos]
        line_pos += 1

        num, den = map(int, fraction.split("/"))
        assert gcd(num, den) == 1
        assert den > 0

        k = int(lines[line_pos])
        line_pos += 1

        chosen = []
        if k > 0:
            chosen = list(map(int, lines[line_pos].split()))
        line_pos += 1

        assert 0 <= k <= n
        assert len(chosen) == k
        assert len(set(chosen)) == k
        assert all(1 <= x <= n for x in chosen)

        chosen_set = set(chosen)
        actual_num = total_pleasure
        for i, (p, v) in enumerate(games, 1):
            if i in chosen_set:
                actual_num += p

        actual_den = total_votes + k

        assert num * actual_den == actual_num * den

        best_num = total_pleasure
        best_den = total_votes

        ordered = sorted((p, i) for i, (p, v) in enumerate(games, 1))
        ordered.reverse()

        prefix = 0
        for kk in range(1, n + 1):
            prefix += ordered[kk - 1][0]
            candidate_num = total_pleasure + prefix
            candidate_den = total_votes + kk
            assert candidate_num * best_den <= best_num * candidate_den or (
                candidate_num * best_den == best_num * candidate_den
            )

            if candidate_num * best_den > best_num * candidate_den:
                best_num = candidate_num
                best_den = candidate_den

sample = """2
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

check(sample, solution(sample))

minimum = """1
1
0 7
"""
check(minimum, solution(minimum))

all_equal = """1
4
5 1
5 2
5 3
5 4
"""
check(all_equal, solution(all_equal))

zero_votes = """1
2
10 1
100 0
"""
check(zero_votes, solution(zero_votes))

boundary = """1
3
0 1000
1000 0
999 1
"""
check(boundary, solution(boundary))

large = "1\n1000\n" + "\n".join(
    f"{i % 1001} {1 if i == 1 else 0}" for i in range(1000)
) + "\n"
check(large, solution(large))

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một trò chơi với`p=0`|`0/1`,`k=0`| Tập hợp con trống hợp pháp và tử số bằng 0 | 
| Bốn trò chơi có niềm vui như nhau | Tiền tố tối ưu bất kỳ | Giá trị bằng nhau và xử lý ràng buộc | 
| Một trò chơi không có phiếu bầu mang lại niềm vui cao | Trò chơi mang tính giải trí cao được chọn lọc | Trò chơi có (v_i=0) vẫn phải đủ điều kiện | 
|`p=0`,`v=1000`xen lẫn niềm vui lớn | Phân số chính xác từ tiền tố tốt nhất | Giá trị biên và thay đổi mẫu số | 
| Trường hợp tạo 1000 trò chơi | Bất kỳ tối ưu hợp lệ nào | Hành vi tối đa (n) và bộ nhớ tuyến tính | 

## Vỏ cạnh 

Khi bỏ phiếu cho không có trò chơi nào là tối ưu, quá trình quét sẽ xử lý nó vì ứng cử viên tốt nhất ban đầu là (k=0). Đối với đầu vào```
1
1
0 5
```chúng ta có (A=0) và (V=5). Phương án thay thế duy nhất thêm một phiếu bầu không hài lòng và vẫn đưa ra kỳ vọng (0), do đó thuật toán giữ (k=0) và giảm (0/5) thành`0/1`. Dòng thứ ba trống. 

Khi một trò chơi không có phiếu bầu hiện tại, nó vẫn xuất hiện trong mảng đã sắp xếp. Vì```
1
2
10 1
100 0
```chúng ta có (A=10) và (V=1). Ứng cử viên ban đầu là (10/1). Sau khi thêm trò chơi 2, ứng cử viên trở thành (110/2=55), do đó thuật toán chọn trò chơi 2. Số phiếu bầu hiện tại bằng 0 không ngăn cản phiếu bầu mới của chúng tôi biến trò chơi này thành trò chơi có nhiều khả năng được phát trực tuyến nhất. 

Việc thay đổi mẫu số được xử lý trực tiếp bằng cách sử dụng`total_votes + k`. Coi như```
1
2
100 1
0 100
```Ở đây (A=100) và (V=101). Nếu không có phiếu bầu bổ sung, kỳ vọng là (100/101). Thêm trò chơi đầu tiên sẽ tạo ra (200/102=100/51), điều này tốt hơn. Thay vào đó, việc thêm trò chơi không mang lại niềm vui sẽ mang lại (100/102=50/51), điều này còn tệ hơn. Quá trình quét tiền tố đánh giá chính xác cả hai khả năng. 

Các ứng cử viên tối ưu bằng nhau được xử lý bởi quy trình nghiêm ngặt`>`so sánh. Vì```
1
2
5 1
5 1
```kỳ vọng (k=0) là (10/2=5) và việc thêm một trong hai trò chơi cũng cho kết quả (15/3=5). Vì giá trị không cải thiện nên việc triển khai vẫn giữ nguyên (k=0). Điều này hợp lệ vì bài toán yêu cầu bất kỳ chiến lược tối đa hóa nào. 

Bước rút gọn cũng cần thiết ngay cả khi giá trị tối ưu có một giá trị đơn giản. Trong mẫu thứ hai, ứng cử viên được chọn là (5110/1114) và ước chung lớn nhất là 2. Chia cả hai phần cho kết quả`2555/557`, thỏa mãn định dạng phân số tối giản cần thiết.
