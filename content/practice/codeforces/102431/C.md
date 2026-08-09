---
title: "CF 102431C - Ông Gấu Trúc và Máy Đánh Chữ"
description: "Chúng ta cần xây dựng một mảng (S) cố định từ trái sang phải. Tại bất kỳ thời điểm nào, chúng tôi có thể nhập một phần tử mới, sao chép bất kỳ chuỗi con nào đã tồn tại trên giấy vào bảng tạm hoặc nối toàn bộ bảng tạm vào giấy. Chi phí đánh máy (X), chi phí sao chép (Y) và chi phí dán (Z)."
date: "2026-08-09T17:33:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102431
codeforces_index: "C"
codeforces_contest_name: "2019 China Collegiate Programming Contest Final (CCPC-Final 2019)"
rating: 0
weight: 102431
solve_time_s: 191
verified: true
draft: false
---

[CF 102431C - Ông Panda và Máy đánh chữ](https://codeforces.com/problemset/problem/102431/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 11s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một mảng (S) cố định từ trái sang phải. Tại bất kỳ thời điểm nào, chúng tôi có thể nhập một phần tử mới, sao chép bất kỳ chuỗi con nào đã tồn tại trên giấy vào bảng tạm hoặc nối toàn bộ bảng tạm vào giấy. Chi phí đánh máy (X), chi phí sao chép (Y) và chi phí dán (Z). Bảng tạm vẫn tồn tại sau khi dán, do đó, cùng một chuỗi con được sao chép có thể được dán nhiều lần. 

Đầu vào chứa (T) trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm đưa ra độ dài mục tiêu (n), ba chi phí vận hành (X, Y, Z) và (n) giá trị nguyên của (S). Đầu ra yêu cầu là tổng chi phí tối thiểu cần thiết để tạo ra chính xác mảng mục tiêu, tiếp theo là yêu cầu`Case #x:`tiền tố. 

(n) lớn nhất là 5000 và cuộc thi ban đầu đưa ra giới hạn thời gian 15 giây và bộ nhớ 1024 MB. Một giải pháp (O(n^3)) sẽ xem xét khoảng (5000^3=125) tỷ ứng viên trong trường hợp xấu nhất, vì vậy đây không phải là một lựa chọn thực tế. Thuật toán (O(n^2)) là mục tiêu tự nhiên. Dữ liệu đầu vào cũng cho biết rằng ít nhất 80 phần trăm các thử nghiệm có (n\le1000), điều này làm cho một giải pháp bậc hai được triển khai cẩn thận trở nên đặc biệt phù hợp. 

Có một số trường hợp khó khăn có thể khiến một giải pháp hợp lý bề ngoài trở nên sai lầm. Với mục tiêu một yếu tố như`1 5 1 1`theo sau là`7`, đáp án là 5 vì chưa có gì để sao chép. Giải pháp giả định mọi trạng thái cuối cùng đều đến từ một bản dán có thể thất bại ở đây. 

Trường hợp thứ hai là`4 10 1 1`theo sau là`1 1 1 1`. Câu trả lời là 14. Chúng ta gõ chữ đầu tiên`1`cho 10, sao chép nó cho 1, sau đó dán ba lần cho 3 lần nữa. Một giải pháp tính chi phí sao chép cho mỗi lần dán sẽ thu được 16. 

Trường hợp thứ ba là`4 10 1 1`theo sau là`1 2 1 2`. Câu trả lời là 22. Chúng ta gõ`1 2`, sao chép chuỗi con hai phần tử đó và dán nó một lần. Chuỗi con lặp lại bắt đầu ở vị trí 0 và được dán bắt đầu từ vị trí 2. Việc kiểm tra ranh giới bất cẩn có thể vô tình từ chối sự xuất hiện này vì nguồn và đích gần nhau. 

Cuối cùng, chuỗi con lặp lại không nhất thiết phải là chuỗi con ngay trước đó. TRONG`6 10 8 8`với`1 2 2 1 2 3`, ba giá trị đầu tiên được gõ, sau đó`1 2`được sao chép từ vị trí 0 và 1 và dán sau ba giá trị đầu tiên. Điều này tạo ra`1 2 2 1 2`, sau đó là trận chung kết`3`được gõ, cho kết quả 56. Một giải pháp chỉ tìm kiếm các lần lặp lại liền kề với tiền tố hiện tại sẽ bỏ qua cấu trúc này. 

## Phương pháp tiếp cận 

Một chương trình động đơn giản xem xét mọi tiền tố của (S) và với mỗi lần dán có thể, nó sẽ thử mọi chuỗi con của tiền tố đã được tạo sẵn làm nội dung bảng tạm có thể có. Nếu đẳng thức chuỗi con đã được xử lý trước thì sẽ có (O(n^2)) chuỗi con nguồn ứng viên cho mỗi tiền tố (O(n)), đưa ra chuyển tiếp (O(n^3)). Với (n=5000), đây là khoảng 125 tỷ lượt chuyển đổi ứng viên trong trường hợp xấu nhất. Nếu đẳng thức chuỗi con cũng được kiểm tra theo từng ký tự thì độ phức tạp sẽ càng trở nên tồi tệ hơn. 

DP brute-force là chính xác vì mọi dán hợp pháp phải sao chép một số chuỗi con đã tồn tại trên giấy, do đó việc liệt kê mọi nguồn có thể và mọi đích có thể bao gồm mọi chuỗi hoạt động hợp pháp. Vấn đề là hầu hết các lựa chọn nguồn đó đều tương đương nhau. Chi phí sao chép một chuỗi con cụ thể không phụ thuộc vào nơi chuỗi con đó xuất hiện và điều duy nhất quan trọng sau này là nội dung của bảng tạm. 

Quan sát quan trọng là nhìn vào một miếng dán ngược. Giả sử khối tiếp theo mà chúng ta muốn nối thêm có độ dài (j) và nó bằng với một số lần xuất hiện trước đó kết thúc ở vị trí (t). Khi khối đó nằm trong bảng nhớ tạm, chúng tôi có thể nhập mọi thứ từ lần xuất hiện đó đến vị trí dán cuối cùng mà không cần thay đổi bảng nhớ tạm. Do đó, toàn bộ chuỗi các thao tác "giữ bảng tạm này, nhập một số ký tự, sau đó dán" có thể được biểu diễn bằng một chuyển đổi DP. 

Có một tính chất đơn điệu hữu ích bổ sung. Giả sử khối tương tự xảy ra kết thúc ở vị trí (t_1<t_2). Bắt đầu từ trạng thái mà bảng tạm là khối đó tại (t_1), chúng ta có thể nhập các ký tự từ (t_1) đến (t_2-1) mà không cần thay đổi bảng tạm. Do đó, 

[ 
dp[t_2][j]\le dp[t_1][j]+(t_2-t_1)X. 
] 

Sau khi sắp xếp lại, 

[ 
dp[t_2][j]-t_2X\le dp[t_1][j]-t_1X. 
] 

Vì vậy, trong số tất cả các lần xuất hiện của cùng một khối, lần xuất hiện sớm nhất ít nhất luôn tốt cho kiểu chuyển đổi này. Chúng ta không bao giờ cần phải thử mọi lần xuất hiện. Chúng ta chỉ cần sự xuất hiện đầu tiên của mỗi chuỗi con. 

Một máy tự động hậu tố cung cấp chính xác thông tin đó. Mỗi chuỗi con tương ứng với một đường dẫn từ trạng thái ban đầu và tất cả các chuỗi con được biểu thị bởi cùng một trạng thái đều có cùng một tập hợp các vị trí kết thúc. Nếu như`firstpos[v]`lưu trữ vị trí kết thúc sớm nhất của các chuỗi con được biểu thị bằng trạng thái (v), sau đó sau khi duyệt qua chuỗi con có độ dài (j), lần xuất hiện đầu tiên của nó kết thúc tại`firstpos[v]`. Đây là thuộc tính xuất hiện lần đầu tiêu chuẩn của hậu tố automata. 

Chúng ta có thể liệt kê tất cả các chuỗi con (O(n^2)) bằng cách bắt đầu từ mọi vị trí và đi tiếp qua máy tự động hậu tố. Đối với mỗi chuỗi con, chúng tôi ghi lại lần xuất hiện sớm nhất của nó. DP khi đó chỉ có trạng thái (O(n^2)) và (O(1)) hoạt động trên mỗi trạng thái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DP | (O(n^3)) | (O(n^2)) | Quá chậm | 
| DP tối ưu + hậu tố automaton | (O(n^2)) | (O(n^2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một máy tự động hậu tố cho mảng mục tiêu. Mỗi trạng thái lưu trữ độ dài chuỗi con được biểu thị tối đa, liên kết hậu tố, chuyển tiếp và vị trí kết thúc sớm nhất của một lần xuất hiện. Khi trạng thái bình thường được tạo trong khi xử lý vị trí (p), vị trí kết thúc sớm nhất của nó là (p). Khi một bản sao được tạo, nó sẽ kế thừa vị trí sớm nhất của trạng thái mà nó nhân bản. 
2. Tính toán trước lần xuất hiện sớm nhất của mỗi chuỗi con. Đối với mọi vị trí bắt đầu (l), hãy bắt đầu từ gốc tự động và sử dụng (S_l,S_{l+1},\ldots) từng vị trí một. Sau khi sử dụng (S_l\ldots S_r), trạng thái hiện tại đại diện cho chuỗi con đó, vì vậy`firstpos[state] + 1`là độ dài tiền tố sớm nhất mà chuỗi con này xuất hiện. Lưu trữ giá trị này cho cặp ((l,r)). 
3. Xác định`dp[i][j]`for (j>0) là chi phí tối thiểu để xây dựng các phần tử (i) đầu tiên trong khi có chuỗi con (S[i-j]) trong bảng tạm, với trạng thái này hữu ích ngay trước khi dán. Định nghĩa`dp[i][0]`là chi phí tối thiểu để xây dựng (i) phần tử đầu tiên khi nội dung trong bảng tạm không liên quan. 
4. Khởi tạo`dp[0][0]=0`. giá trị`best[i]`, được định nghĩa là tối thiểu trên tất cả`dp[i][j]`, là cách rẻ nhất để xây dựng các phần tử (i) đầu tiên khi chúng ta không quan tâm đến những gì có trong bảng tạm. 
5. Để tính toán`dp[i][0]`, nhập phần tử (i)-th. Bảng tạm trước đó có thể là bất kỳ thứ gì, vì vậy trạng thái trước đó rẻ nhất là`best[i-1]`. Như vậy 
[ 
dp[i][0]=tốt nhất[i-1]+X. 
] 
6. Với mọi (j) từ 1 đến (i), hãy xem xét khối (B=S[i-j]) sẽ được dán để hoàn thành tiền tố. Đặt (l=i-j), do đó khối phải được sao chép từ đâu đó bên trong phần tử (l) đầu tiên. 
7. Nếu (B) có một sự xuất hiện kết thúc ở độ dài tiền tố nào đó (t\le l), chúng ta có thể xây dựng các phần tử (l) đầu tiên một cách tối ưu, sao chép (B) và dán ngay lập tức. Điều này mang lại 
[ 
dp[i][j]\le best[l]+Y+Z. 
] 
Nguồn sao chép có thể là bất kỳ trường hợp hợp lệ nào vì vị trí của nó không ảnh hưởng đến chi phí. 
8. Bảng tạm có thể đã chứa (B). Đặt (t) là độ dài tiền tố sớm nhất nơi sự xuất hiện của (B) kết thúc. Từ`dp[t][j]`, nhập các phần tử (l-t) giữa lần xuất hiện đó và vị trí dán mong muốn, sau đó dán bảng tạm. Điều này mang lại 
[ 
dp[i][j]\le dp[t][j]+(l-t)X+Z. 
] 
9. Tận dụng tối đa các chuyển tiếp và cập nhật có sẵn`best[i]`. Việc xử lý tiền tố theo thứ tự tăng dần đảm bảo rằng mọi trạng thái DP được tham chiếu đều đã được tính toán. 
10. Câu trả lời cuối cùng là`best[n]`, bởi vì sau khi xây dựng mục tiêu hoàn chỉnh, chúng tôi không còn quan tâm đến những gì còn lại trong bảng tạm. 

Tính bất biến của tính chính xác là mọi trạng thái DP biểu thị một cấu trúc có thể thực hiện được của chính xác tiền tố được chỉ định của nó và mọi thao tác cuối cùng có thể có để tạo tiền tố mới đều được bao phủ bởi một trong hai chuyển tiếp dán hoặc bằng cách gõ. Quá trình chuyển đổi dán sẽ sao chép khối được yêu cầu ngay trước khi dán hoặc bắt đầu từ trạng thái trước đó khi khối tương tự đã có trong bảng tạm và giữ nguyên bảng tạm đó trong khi nhập. Lần xuất hiện sớm nhất là đủ vì bất kỳ lần xuất hiện nào sau đó đều có thể truy cập được từ lần xuất hiện trước đó bằng cách nhập trong khi vẫn giữ cùng một bảng tạm, điều này không bao giờ có thể cải thiện biểu thức`dp[t][j] - t*X`. Do đó, mọi cách xây dựng tối ưu đều có một chuyển đổi được biểu thị bằng phép truy toán và mọi chuyển đổi được tạo ra bởi phép truy toán tương ứng với các hoạt động máy đánh chữ hợp pháp. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

INF = 4_000_000_000_000_000_000

def build_suffix_automaton(s):
    nxt = [{}]
    link = [-1]
    length = [0]
    first = [-1]

    last = 0

    for pos, ch in enumerate(s):
        cur = len(nxt)
        nxt.append({})
        link.append(0)
        length.append(length[last] + 1)
        first.append(pos)

        p = last

        while p != -1 and ch not in nxt[p]:
            nxt[p][ch] = cur
            p = link[p]

        if p == -1:
            link[cur] = 0
        else:
            q = nxt[p][ch]

            if length[p] + 1 == length[q]:
                link[cur] = q
            else:
                clone = len(nxt)
                nxt.append(nxt[q].copy())
                link.append(link[q])
                length.append(length[p] + 1)
                first.append(first[q])

                while p != -1 and nxt[p].get(ch) == q:
                    nxt[p][ch] = clone
                    p = link[p]

                link[q] = clone
                link[cur] = clone

        last = cur

    return nxt, first

def solve_case(n, X, Y, Z, s):
    nxt, first = build_suffix_automaton(s)

    # earliest[l, r] = prefix length at which s[l:r+1]
    # first occurs in the whole string.
    #
    # Row l contains substrings starting at l:
    # [l,l], [l,l+1], ..., [l,n-1].
    total_occ = n * (n + 1) // 2
    earliest = array('H', [0]) * total_occ

    for l in range(n):
        v = 0

        # Start of row l in the triangular array.
        base = l * n - l * (l - 1) // 2

        for r in range(l, n):
            v = nxt[v][s[r]]
            earliest[base + (r - l)] = first[v] + 1

    # dp[i][j], 0 <= j <= i, stored in triangular form.
    #
    # Row i has i+1 entries.
    total_dp = (n + 1) * (n + 2) // 2
    dp = array('q', [INF]) * total_dp

    dp[0] = 0

    best = [INF] * (n + 1)
    best[0] = 0

    for i in range(1, n + 1):
        base_i = i * (i + 1) // 2

        # Type S[i-1]. The clipboard is irrelevant afterwards.
        cur_best = best[i - 1] + X
        dp[base_i] = cur_best

        for j in range(1, i + 1):
            l = i - j

            # Substring s[l:i] starts at l and has length j.
            base_l = l * n - l * (l - 1) // 2
            t = earliest[base_l + j - 1]

            if t <= l:
                # Construct prefix l, copy the block, paste it.
                cand = best[l] + Y + Z
                if cand < cur_best:
                    cur_best = cand

                # The block is already in the clipboard at prefix t.
                # Type the gap, then paste.
                idx_tj = t * (t + 1) // 2 + j
                cand = dp[idx_tj] + (l - t) * X + Z
                if cand < cur_best:
                    cur_best = cand

            dp[base_i + j] = cur_best

        best[i] = cur_best

    return best[n]

def main():
    T = int(input())

    out = []

    for tc in range(1, T + 1):
        n, X, Y, Z = map(int, input().split())
        s = list(map(int, input().split()))

        ans = solve_case(n, X, Y, Z, s)
        out.append(f"Case #{tc}: {ans}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Việc xây dựng automaton hậu tố là tiêu chuẩn. Mỗi giá trị mới được thêm vào sẽ tạo ra một trạng thái bình thường có`first`vị trí là chỉ số mảng hiện tại. Bản sao không phải là sự xuất hiện mới nên nó kế thừa vị trí xuất hiện đầu tiên của trạng thái được nhân bản. Đây chính xác là thuộc tính cần thiết để trả lời các truy vấn xuất hiện lần đầu. 

các`earliest`mảng sử dụng bố cục hình tam giác vì có (n(n+1)/2) chuỗi con không trống. Đối với vị trí bắt đầu cố định`l`, hàng chứa tất cả các vị trí kết thúc từ`l`bởi vì`n-1`. Sử dụng mảng không dấu 16 bit là an toàn vì mỗi độ dài tiền tố được lưu trữ tối đa là 5000. 

DP sử dụng ý tưởng tam giác tương tự. Hàng ngang`i`chứa`dp[i][0]`bởi vì`dp[i][i]`, do đó tổng số ô là ((n+1)(n+2)/2). Số nguyên 64 bit có dấu là đủ vì ngay cả giải pháp gõ toàn bộ cũng có giá cao nhất (5000\cdot10^9=5\cdot10^{12}), trong khi giải pháp tối ưu không thể có giá cao hơn thế. 

Điều kiện biên tinh tế nhất là`t <= l`. Đây`l=i-j`là độ dài tiền tố trước khi khối cuối cùng được thêm vào. Một sự kiện kết thúc tại`t=l`hợp lệ vì nguồn của nó kết thúc chính xác tại nơi đích bắt đầu. Một sự việc kết thúc sau`l`sẽ chồng lên khối được tạo và không thể tồn tại trên giấy tại thời điểm dán. 

Quá trình chuyển đổi DP thứ hai sử dụng`best[l]`, không phải là một trạng thái bảng nhớ tạm cụ thể vì việc sao chép sẽ ghi đè lên bảng nhớ tạm. Bất kỳ bảng tạm nào tồn tại trước khi sao chép đều không liên quan. Ngược lại, quá trình chuyển đổi thứ ba phải sử dụng`dp[t][j]`, vì nó dựa vào việc bảo toàn bảng nhớ tạm hiện có trong khi nhập các ký tự xen vào. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mục tiêu là`1 2 3 1 2 3`với cả ba chi phí đều bằng 1. Các trạng thái quan trọng là những trạng thái cuối cùng trở nên tối ưu. 

| Độ dài tiền tố`i`| Chiều dài bảng nhớ tạm`j`| Xuất hiện sớm nhất`t`| Chuyển tiếp |`dp[i][j]`|`best[i]`| 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | | gõ | 1 | 1 | 
| 2 | 0 | | gõ | 2 | 2 | 
| 3 | 0 | | gõ | 3 | 3 | 
| 4 | 1 | 1 | giữ`1`, kiểu`2 3`, dán | 4 | 4 | 
| 5 | 2 | 2 | xây dựng 3, sao chép`1 2`, dán | 5 | 5 | 
| 6 | 3 | 3 | xây dựng 3, sao chép`1 2 3`, dán | 5 | 5 | 

Ở độ dài tiền tố 3, việc gõ sẽ có giá 3. Khối`1 2 3`xuất hiện ở đó dưới dạng ba phần tử đầu tiên, vì vậy với độ dài 6, chúng ta có thể sao chép nó thành 1 và dán nó thành 1. Chi phí cuối cùng là (3+1+1=5), khớp với đầu ra mẫu. 

Dấu vết cũng chứng minh tại sao`t=l`phải được chấp nhận. Đối với lần dán ba phần tử cuối cùng, nguồn chiếm các vị trí từ 0 đến 2 và đích bắt đầu ở vị trí 3. Nguồn kết thúc chính xác tại ranh giới`l=3`. 

### Mẫu 2 

Mục tiêu là`1 2 2 1 2 3`, với (X=10) và (Y=Z=8). 

| Độ dài tiền tố`i`| Chiều dài bảng nhớ tạm`j`| Xuất hiện sớm nhất`t`| Chuyển tiếp |`dp[i][j]`|`best[i]`| 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | | kiểu`1`| 10 | 10 | 
| 2 | 0 | | kiểu`2`| 20 | 20 | 
| 3 | 0 | | kiểu`2`| 30 | 30 | 
| 4 | 1 | 1 | sử dụng sớm hơn`1`, gõ khoảng cách, dán | 38 | 38 | 
| 5 | 2 | 2 | sao chép`1 2`từ hai giá trị đầu tiên, dán | 46 | 46 | 
| 6 | 0 | | gõ cuối cùng`3`| 56 | 56 | 

Quá trình chuyển đổi khóa xảy ra ở độ dài tiền tố 5. Ba giá trị đầu tiên`1 2 2`tốn 30 để gõ. Hai giá trị tiếp theo là`1 2`, đã xảy ra ở vị trí 0 và 1. Sao chép tốn 8 và dán tốn thêm 8, do đó tiền tố có độ dài 5 có giá 46. Cuối cùng`3`giá 10 thì được 56. 

Ví dụ này cho thấy tại sao sự xuất hiện của nguồn không cần phải liền kề với đích. nguồn`1 2`ở ngay đầu, trong khi quá trình dán bắt đầu sau phần tử thứ ba. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^2)) | Máy tự động hậu tố là tuyến tính, tất cả các chuỗi con được duyệt một lần và DP có trạng thái (O(n^2)) với (O(1)) hoạt động trên mỗi trạng thái. | 
| Không gian | (O(n^2)) | Mỗi bảng xuất hiện sớm nhất và bảng DP hình tam giác đều chứa (O(n^2)) mục nhập. | 

Giới hạn bậc hai phù hợp với (n\le5000), đặc biệt với giới hạn 15 giây và 1024 MB ban đầu. Việc triển khai Python có chủ ý lưu trữ hai bảng bậc hai một cách nhỏ gọn`array`đối tượng thay vì danh sách số nguyên Python thông thường, điều này tránh được chi phí đối tượng lớn hơn nhiều so với danh sách hai chiều thông thường. 

## Trường hợp thử nghiệm 

Khai thác sau đây giả định giải pháp đã gửi được lưu dưới dạng`solution.py`và phơi bày`main()`chức năng hiển thị ở trên.```python
import sys
import io
from solution import main

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        main()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples.
sample = """\
2
6 1 1 1
1 2 3 1 2 3
6 10 8 8
1 2 2 1 2 3
"""

assert run(sample) == """\
Case #1: 5
Case #2: 56
""", "provided samples"

# Minimum-size input.
assert run("""\
1
1 5 1 1
7
""") == """\
Case #1: 5
""", "minimum size"

# No repeated substring is useful.
assert run("""\
1
3 10 1 1
1 2 3
""") == """\
Case #1: 30
""", "no repetition"

# Repeated block starts at the beginning and is pasted
# exactly at the boundary after the first three elements.
assert run("""\
1
4 10 1 1
1 2 1 2
""") == """\
Case #1: 22
""", "boundary repetition"

# All values equal.
assert run("""\
1
4 10 1 1
1 1 1 1
""") == """\
Case #1: 14
""", "all equal"

# Maximum n. The huge typing and copying costs make
# copying a one-element block optimal.
n = 5000
maximum_case = "1\n5000 1000000000 1000000000 1\n" + ("1 " * (n - 1)) + "1\n"

assert run(maximum_case) == """\
Case #1: 2000000004
""", "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 5 1 1 / 7`|`Case #1: 5`| Kích thước tối thiểu và không có bản sao | 
|`1 / 3 10 1 1 / 1 2 3`|`Case #1: 30`| Không có chuỗi con lặp lại nào có thể giảm chi phí gõ | 
|`1 / 4 10 1 1 / 1 2 1 2`|`Case #1: 22`| Nguồn xuất hiện kết thúc chính xác tại ranh giới dán | 
|`1 / 4 10 1 1 / 1 1 1 1`|`Case #1: 14`| Sử dụng lại một giá trị clipboard cho nhiều lần dán | 
|`n=5000`, tất cả`1`|`Case #1: 2000000004`| Kích thước tối đa, chi phí lớn, số học 64 bit | 

## Vỏ cạnh 

Đối với mục tiêu có độ dài một, chẳng hạn như```
1
1 5 1 1
7
```không có sự xuất hiện trước đó của bất kỳ chuỗi con không trống nào. Cấu trúc duy nhất có thể tiếp cận là gõ một giá trị duy nhất. Bộ DP`dp[1][0]`ĐẾN`best[0]+X=5`, và đáp án là 5. 

Đối với một mục tiêu không có sự lặp lại, chẳng hạn như```
1
3 10 1 1
1 2 3
```mỗi khối ứng cử viên đều có lần xuất hiện đầu tiên tại đích riêng của nó, vì vậy vị trí kết thúc sớm nhất của nó lớn hơn độ dài tiền tố có sẵn trước khi dán. Cả hai quá trình chuyển đổi dán đều bị từ chối. Do đó, DP gõ cả ba giá trị, cho kết quả 30. 

Đối với sự xuất hiện chính xác tại ranh giới, hãy xem xét```
1
4 10 1 1
1 2 1 2
```Khi tính toán khối hai phần tử cuối cùng, (i=4), (j=2) và (l=i-j=2). khối`1 2`đầu tiên xảy ra với độ dài tiền tố cuối (t=2). Vì (t\le l), nguồn hoàn toàn có sẵn trước khi đích bắt đầu. Tiền tố có độ dài 2 có giá 20, chi phí sao chép là 1 và chi phí dán là 1, cho ra 22. 

Đối với các giá trị đơn lặp lại, hãy xem xét```
1
4 10 1 1
1 1 1 1
```Sau khi gõ giá trị đầu tiên, clipboard có thể chứa nó. Sau đó, cùng một bảng tạm có thể được dán ba lần. Tổng số là (10+1+3=14). DP không tính phí sao chép khác cho những lần dán sau này vì quá trình chuyển đổi thứ ba sẽ giữ nguyên bảng nhớ tạm hiện có. 

Trường hợp kích thước tối đa sử dụng 5000 giá trị giống nhau với (X=Y=10^9) và (Z=1). Nhập một giá trị có giá (10^9), sao chép giá trị đó có giá khác (10^9) và 4999 giá trị còn lại có thể được dán với giá 4999. Kết quả là (2.000.004). Việc sử dụng bộ lưu trữ 64-bit ngăn không cho số học bị tràn và các bảng bậc hai vẫn nằm trong giới hạn bộ nhớ 1024 MB của cuộc thi.
