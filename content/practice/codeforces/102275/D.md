---
title: "CF 102275D - Hải sản"
description: "Percy bắt đầu ở vị trí 0 trên trục số. Mỗi vật thể đều có một vị trí, một độ cứng và một loại. Một con trai có thể được nhặt lên khi Percy đến thăm vị trí của nó. Một tảng đá có thể làm gãy mọi con nghêu hiện đang được vận chuyển có độ cứng hoàn toàn nhỏ hơn độ cứng của đá."
date: "2026-08-17T03:12:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102275
codeforces_index: "D"
codeforces_contest_name: "2019 Facebook Hacker Cup, Round 2"
rating: 0
weight: 102275
solve_time_s: 755
verified: true
draft: false
---

[CF 102275D - Hải sản](https://codeforces.com/problemset/problem/102275/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 12 phút 35 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Percy bắt đầu ở vị trí 0 trên trục số. Mỗi vật thể đều có một vị trí, một độ cứng và một loại. Một con trai có thể được nhặt lên khi Percy đến thăm vị trí của nó. Một tảng đá có thể làm gãy mọi con nghêu hiện đang được vận chuyển có độ cứng hoàn toàn nhỏ hơn độ cứng của đá. 

Bản thân phong trào là điều duy nhất tốn thời gian. Percy có thể đi ngang qua một đồ vật nhiều lần và việc thăm lại một tảng đá rất hữu ích vì một tảng đá có thể làm vỡ những con trai được nhặt sau chuyến thăm trước đó của tảng đá. Nhiệm vụ là tìm tổng quãng đường bơi tối thiểu cần thiết để ăn từng con nghêu hoặc báo cáo`-1`nếu một số con ngao không bao giờ có thể bị phá vỡ. 

Vị trí và độ cứng được tạo ra bởi hai lần lặp lại bậc hai, vì vậy trước tiên chúng ta phải xây dựng các đối tượng thực tế. Các giá trị lặp lại có thể vượt quá phạm vi 32 bit trong quá trình nhân, điều này vô hại trong Python vì số nguyên có độ chính xác tùy ý. 

Với tối đa 800.000 đối tượng trong một trường hợp thử nghiệm, thuật toán so sánh từng con ngao với từng tảng đá đã quá chậm. Thuật toán bậc hai sẽ thực hiện phép so sánh khoảng 6,4 × 10^11 trong trường hợp xấu nhất. Giải pháp dự định cần phải gần với tuyến tính hoặc O(N log N), bao gồm cả việc sắp xếp cần thiết vì các đối tượng được tạo không được đưa ra theo thứ tự vị trí. 

Trường hợp tinh tế đầu tiên là độ cứng nghiêm ngặt. Coi như```
2
1 2 0 0 0 10
5 5 0 0 0 10
CR
```Con sò và đá đều có độ cứng 5 nên đá không thể làm vỡ con sò. Câu trả lời là`-1`. Một giải pháp sử dụng`>=`thay vì`>`sẽ báo cáo sai một câu trả lời hữu hạn. 

Trường hợp tinh vi thứ hai là tảng đá cứng nhất toàn cầu không cần đủ hữu ích để đưa ra con đường ngắn nhất. Ví dụ,```
3
9 10 0 0 99 100
6 5 0 0 5 10
RCR
```Các vật thể là một tảng đá có độ cứng 6 ở mức 9, một tảng đá có độ cứng 5 ở mức 10 và một tảng đá có độ cứng 6 ở mức 100. Percy có thể bơi đến 10, trả lại một đơn vị cho tảng đá ở mức 9 và kết thúc ở đó, mất 11 giây. Đi đến vị trí 100 sẽ tệ hơn nhiều. Một giải pháp luôn chọn loại đá cứng nhất trên toàn cầu sẽ bỏ lỡ khả năng này. 

Trường hợp tinh vi thứ ba là một tảng đá đến trước một con sò không làm vỡ con sò đó chỉ vì Percy sau đó đã vượt qua con sò. TRONG```
4
10 50 0 1 40 80
50 10 0 1 38 80
RRCC
```các vật thể là đá có độ cứng 50 ở mức 10, đá có độ cứng 10 ở mức 50, đá có độ cứng 49 ở mức 11 và đá có độ cứng 8 ở mức 52. Lộ trình tối ưu là```
0 -> 11 -> 10 -> 50 -> 52 -> 50
```với giá 56. Con ngao đầu tiên bị gãy ở vị trí 10, trong khi con ngao thứ hai bị gãy ở vị trí 50 sau khi được nhặt lên. Một cách tiếp cận bất cẩn chỉ ghé thăm mọi vật thể một lần sẽ thất bại vì con ngao thứ hai được nhặt lên sau khi tảng đá ở vị trí 50 được ghé thăm lần đầu tiên. 

Cuối cùng, các vị trí được tạo ra không được sắp xếp và có thể rất lớn. Sắp xếp theo vị trí trước khi thực hiện bất kỳ lý luận hình học nào là bắt buộc. Sử dụng chỉ số thế hệ làm thứ tự không gian sẽ tạo ra mối quan hệ ưu tiên không chính xác. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp có thể thử mọi loại đá có thể cho mọi con nghêu. Đối với một con nghêu ở vị trí`p`và độ cứng`h`, chúng ta có thể kiểm tra từng tảng đá, kiểm tra xem độ cứng của nó có lớn hơn`h`, rồi cố gắng suy luận về lộ trình kết quả. Ngay cả việc chỉ tìm một máy nghiền khả thi cho mỗi con nghêu cũng mất O(N2) trong trường hợp xấu nhất. Với 800.000 đồ vật, điều này hoàn toàn không thể thực hiện được. 

Một lực lượng vũ phu hữu ích hơn sẽ liệt kê những tảng đá mà Percy sử dụng để đập trai và sau đó tìm ra con đường ngắn nhất cho lựa chọn đó. Tỷ lệ này vẫn theo cấp số nhân, bởi vì mỗi con nghêu có thể chọn một tảng đá ở hai bên và một số con có thể dùng chung một tảng đá. 

Điều quan trọng là Percy luôn khám phá các vị trí mới từ trái sang phải. Tất cả các vị trí đều dương, do đó, lần đầu tiên anh ta đạt đến một vị trí tối đa mới, mọi đối tượng ở vị trí nhỏ hơn đều đã được ghé thăm. Phần phức tạp chỉ là việc xem lại các tảng đá. 

Hãy xem xét những tảng đá mà Percy thực sự đập vỡ trai. Chúng ta có thể chuẩn hóa một giải pháp tối ưu để các đá dịch vụ này xuất hiện theo thứ tự vị trí tăng dần. Giả sử hai khối đá dịch vụ liên tiếp xuất hiện theo thứ tự vị trí giảm dần. Hãy để độ cứng của chúng được`a`Và`b`. Nếu như`a >= b`, tảng đá đầu tiên cũng có thể đập vỡ mọi con ngao mà tảng đá thứ hai đập vỡ. Nếu như`b > a`, tảng đá thứ hai cũng có thể đập vỡ mọi con ngao mà tảng đá đầu tiên đập vỡ. Trong cả hai trường hợp, một trong hai sự kiện dịch vụ là không cần thiết. Việc lặp lại điều này sẽ loại bỏ tất cả các mức giảm. 

Bây giờ lấy hai hòn đá phục vụ liên tiếp tại các vị trí`L < R`. Mỗi con nghêu giữa chúng có thể bị phá vỡ bởi`L`, sau khi Percy quay lại với nó, hoặc bằng`R`, trong khi Percy đang di chuyển sang phải. Không cần có đá phục vụ không liền kề. Nếu một con nghêu cần một hòn đá sớm hơn thay vì`L`, thì tảng đá trước đó cứng hơn`L`, và nó có thể thay thế`L`cho món nghêu được phục vụ trước đó bởi`L`cũng vậy. Lập luận tương tự cũng áp dụng cho tảng đá sau này. 

Điều này biến bài toán chuyển động thành bài toán quy hoạch động trên các tảng đá theo thứ tự vị trí tăng dần. 

Giả sử đá phục vụ trước đó là`L`và đá dịch vụ mới là`R`. Nếu mỗi con nghêu trong`(L,R)`có độ cứng nhỏ hơn`H_R`, Percy chỉ đơn giản di chuyển từ`L`ĐẾN`R`, tính chi phí`R-L`. 

Nếu không, một số con nghêu quá cứng để`R`. Từ`L`phải có khả năng phục vụ họ, mỗi con nghêu trong`(L,R)`phải có độ cứng nhỏ hơn`H_L`. Cho phép`q`là con ngao ngoài cùng bên phải trong`(L,R)`độ cứng của nó ít nhất là`H_R`. Percy có thể đi từ`L`ĐẾN`q`, quay lại`L`để phá vỡ tất cả ngao mà chỉ`L`có thể phá vỡ, và sau đó tiếp tục`R`. Chi phí là`(q-L) + (q-L) + (R-q) = R + 2q - 3L`. 

Có một cách đặc biệt hữu ích để nhận biết liệu một tảng đá trước đó có`L`có thể thực hiện kiểu chuyển tiếp thứ hai này. Cho phép`bad[L]`là con ngao đầu tiên ở bên phải của`L`độ cứng của nó ít nhất là`H_L`. Sau đó`L`có thể xử lý toàn bộ khoảng thời gian cho đến một tảng đá mới`R`chính xác khi nào`R < bad[L]`. 

Điều này đưa ra sự lặp lại với hai loại giá trị ứng cử viên. Để chuyển đổi bình thường, chúng ta cần`dp[L] - L`. 

Đối với một quá trình chuyển đổi liên quan đến việc quay trở lại`L`, chúng tôi cần`dp[L] - 3L`. 

Giá trị đầu tiên không bao giờ hết hạn. Giá trị thứ hai chỉ có thể sử dụng được khi`R < bad[L]`. Cây Fenwick xử lý tiền tố cực tiểu cho loại thứ nhất, trong khi cây phân đoạn cộng với đống hết hạn xử lý tiền tố cực tiểu cho loại thứ hai. 

Truy vấn hình học còn lại là con ngao ngoài cùng bên phải trước`R`độ cứng của nó ít nhất là`H_R`. Đây chính xác là một truy vấn ngăn xếp đơn điệu. Cùng một loại ngăn xếp, được quét từ phải sang trái, tính toán`bad[L]`. 

Cách tiếp cận kết quả là O(N log N), bị chi phối bởi việc sắp xếp và cấu trúc dữ liệu tối thiểu trong phạm vi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(N) | Quá chậm | 
| Tối ưu | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo ra tất cả các vị trí và độ cứng từ hai lần lặp lại. Lưu trữ từng đối tượng dưới dạng`(position, hardness, type)`và sắp xếp các đối tượng theo vị trí. Thứ tự vị trí là thứ tự mà Percy lần đầu tiên gặp các đồ vật. 
2. Quét các đối tượng được sắp xếp từ phải sang trái bằng một chồng đơn điệu chứa các vị trí và độ cứng của nghêu. Đối với mỗi tảng đá, loại bỏ các mục xếp chồng có độ cứng nhỏ hơn đá. Phần trên còn lại là con nghêu gần nhất bên phải có độ cứng ít nhất bằng độ cứng của đá. Lưu trữ vị trí của nó như`bad[L]`. Nếu ngăn xếp trở nên trống rỗng,`bad[L]`là vô cùng. 
3. Trong cùng một lần quét từ phải sang trái, hãy duy trì độ cứng tối đa của tất cả các con trai ở bên phải của mỗi tảng đá. Một tảng đá có thể trở thành đá phục vụ cuối cùng chính xác khi mức tối đa này hoàn toàn nhỏ hơn độ cứng của nó. Lưu trữ điều kiện đó cho câu trả lời cuối cùng. 
4. Quét các đối tượng được sắp xếp từ trái sang phải bằng một chồng nghêu đơn điệu khác. Khi một tảng đá có độ cứng`H_R`gặp phải nghêu có độ cứng nhỏ hơn`H_R`. Phần trên cùng sau những tiếng bật này là con nghêu gần nhất bên trái với độ cứng ít nhất`H_R`. Gọi vị trí của nó`q`. Nếu không có ngao như vậy tồn tại, hãy đặt`q = 0`. 
5. Xử lý đá theo thứ tự vị trí tăng dần bằng lập trình động. Cho phép`dp[R]`là khoảng cách bơi tối thiểu cần thiết để đến được tảng đá`R`sau tất cả ngao trước đây`R`đã được ăn rồi. 
6. Nếu`q = 0`, điểm bắt đầu ảo ở vị trí 0 có thể chuyển trực tiếp sang`R`, bởi vì mỗi con ngao trước đây`R`dễ dàng hơn`R`. Điều này mang lại chi phí cho ứng viên`R`. 
7. Đối với một tảng đá thực sự trước đó`L`với`L >= q`, không có ngao trong`(L,R)`có độ cứng đạt tới`H_R`. Đá`R`có thể bẻ gãy mọi con ngao trong khoảng thời gian đó trong khi Percy di chuyển sang phải. Sự chuyển tiếp là`dp[L] + R-L`, do đó cấu trúc dữ liệu lưu trữ`dp[L]-L`. 
8. Đối với một tảng đá thực sự trước đó`L < q`, con nghêu ở`q`và mọi con nghêu khác ở giữa`L`Và`R`phải được xử lý bởi`L`, bởi vì`R`không thể bẻ nghêu ở`q`. Sự chuyển đổi này chỉ có hiệu lực khi`R < bad[L]`. Chi phí của nó là`dp[L] + R + 2q - 3L`, do đó cấu trúc dữ liệu lưu trữ`dp[L]-3L`. 
9. Truy vấn giá trị chuyển tiếp chuẩn tối thiểu giữa các đá trước đó tại các vị trí ít nhất`q`và giá trị chuyển tiếp trở lại tối thiểu giữa các đá trước đó ở các vị trí bên dưới`q`. Kết hợp chúng với vị trí hiện tại và`q`để có được`dp[R]`. 
10. Chèn đá mới vào cả hai cấu trúc dữ liệu. Giá trị chuyển tiếp bình thường của nó luôn có sẵn. Giá trị chuyển tiếp trả về của nó chỉ được chèn nếu`bad[R]`hoàn toàn lớn hơn vị trí hiện tại và nó sẽ bị xóa khi quá trình quét đạt tới`bad[R]`. 
11. Sau khi tất cả đá đã được xử lý xong, chọn đá phục vụ cuối cùng`L`. Nếu mỗi con nghêu ở bên phải nó có độ cứng nhỏ hơn`H_L`, Percy có thể kết thúc bằng cách di chuyển đến con ngao ngoài cùng bên phải và quay lại`L`. Chi phí này`2(Cmax-L)`khi`L < Cmax`và bằng 0 khi con ngao cuối cùng đã ở hoặc ở bên trái của`L`. Lấy mức tối thiểu trên tất cả các loại đá cuối cùng hợp lệ. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý một tảng đá`R`,`dp[R]`là chi phí tối thiểu trong số tất cả các tuyến đường chuẩn hóa có đá phục vụ mới nhất được`R`và chuyến thăm đầu tiên của họ đã đạt đến`R`. Giữa hai tảng đá dịch vụ liên tiếp`L<R`, mỗi con nghêu được xử lý bởi`R`về phong trào bên phải hoặc bằng cách`L`trong một chuyến du ngoạn trở về. Nếu như`R`xử lý mọi thứ, chi phí là`R-L`. Nếu không thì`q`là con ngao ngoài cùng bên phải đó`R`không thể xử lý được và`L`phải xử lý toàn bộ khoảng thời gian, đưa ra chi phí chính xác`R+2q-3L`. điều kiện`R<bad[L]`chính xác là điều kiện mà`L`khó hơn mọi con nghêu trong khoảng thời gian đó. Vì mọi tuyến đường tối ưu được chuẩn hóa đều có thể được biểu diễn dưới dạng các khối đá dịch vụ ngày càng tăng nên DP xem xét mọi khả năng tối ưu và không chấp nhận chuyển đổi không hợp lệ. 

## Giải pháp Python```python
import sys
import heapq
from bisect import bisect_left

input = sys.stdin.readline

INF = 10**30

def solve():
    T = int(input())
    out = []

    for tc in range(1, T + 1):
        n = int(input())

        p1, p2, ap, bp, cp, dp_mod = map(int, input().split())
        h1, h2, ah, bh, ch, dh_mod = map(int, input().split())
        ops = input().strip()

        objects = [(p1, h1, ops[0] == 'C'),
                   (p2, h2, ops[1] == 'C')]

        pp, p = p1, p2
        hh, h = h1, h2

        for i in range(2, n):
            np = ((ap * pp + bp * p + cp) % dp_mod) + 1
            nh = ((ah * hh + bh * h + ch) % dh_mod) + 1
            objects.append((np, nh, ops[i] == 'C'))
            pp, p = p, np
            hh, h = h, nh

        objects.sort()

        # For every rock, compute:
        # bad = first clam to the right with hardness >= rock hardness
        # final_ok = every clam to the right is strictly easier
        rock_rev = []
        stack = []
        suffix_max_clam = -1

        for pos, hard, is_clam in reversed(objects):
            if is_clam:
                stack.append((pos, hard))
                if hard > suffix_max_clam:
                    suffix_max_clam = hard
            else:
                while stack and stack[-1][1] < hard:
                    stack.pop()

                bad = stack[-1][0] if stack else INF
                final_ok = suffix_max_clam < hard
                rock_rev.append((pos, hard, bad, final_ok))

        rock_rev.reverse()
        rocks = rock_rev
        m = len(rocks)

        if m == 0:
            out.append(f"Case #{tc}: -1")
            continue

        # q[i] = nearest clam to the left of rock i
        # whose hardness is >= hardness of rock i.
        q = []
        stack = []

        for pos, hard, is_clam in objects:
            if is_clam:
                stack.append((pos, hard))
            else:
                while stack and stack[-1][1] < hard:
                    stack.pop()
                q.append(stack[-1][0] if stack else 0)

        positions = [r[0] for r in rocks]
        max_clam_pos = -1

        for pos, hard, is_clam in objects:
            if is_clam:
                max_clam_pos = pos

        # Fenwick tree for minimum dp[L] - pos[L].
        # Indexes are reversed so a suffix in position order becomes
        # a prefix query in the Fenwick tree.
        bit = [INF] * (m + 1)

        def bit_update(idx, value):
            while idx <= m:
                if value < bit[idx]:
                    bit[idx] = value
                idx += idx & -idx

        def bit_query(k):
            res = INF
            while k > 0:
                if bit[k] < res:
                    res = bit[k]
                k -= k & -k
            return res

        # Segment tree for minimum dp[L] - 3*pos[L] among active
        # return-transition candidates.
        size = 1
        while size < m:
            size <<= 1

        seg = [INF] * (2 * size)

        def seg_set(idx, value):
            p = idx + size
            seg[p] = value
            p >>= 1
            while p:
                nv = seg[p << 1]
                rv = seg[p << 1 | 1]
                seg[p] = nv if nv < rv else rv
                p >>= 1

        def seg_query(k):
            # Minimum over indexes [0, k).
            if k <= 0:
                return INF

            left = size
            right = size + k
            res = INF

            while left < right:
                if left & 1:
                    if seg[left] < res:
                        res = seg[left]
                    left += 1
                if right & 1:
                    right -= 1
                    if seg[right] < res:
                        res = seg[right]
                left >>= 1
                right >>= 1

            return res

        # (expiry_position, rock_index)
        expiry = []

        dp_values = [INF] * m
        answer = INF

        for i in range(m):
            pos, hard, bad, final_ok = rocks[i]
            qi = q[i]

            # Return-transition candidates with bad <= pos are no longer valid.
            while expiry and expiry[0][0] <= pos:
                _, idx = heapq.heappop(expiry)
                seg_set(idx, INF)

            if qi == 0:
                best = pos
            else:
                k = bisect_left(positions, qi)
                best = pos + bit_query(m - k)

                # Previous rocks strictly to the left of q.
                left_best = seg_query(k)
                if left_best < INF:
                    candidate = pos + 2 * qi + left_best
                    if candidate < best:
                        best = candidate

            dp_values[i] = best

            # Insert current rock as a future previous service rock.
            rev_index = m - i
            bit_update(rev_index, best - pos)

            if bad > pos:
                seg_set(i, best - 3 * pos)
                heapq.heappush(expiry, (bad, i))

            # Can this rock be the last service rock?
            if final_ok:
                if pos >= max_clam_pos:
                    candidate = best
                else:
                    candidate = best + 2 * (max_clam_pos - pos)

                if candidate < answer:
                    answer = candidate

        if answer >= INF:
            answer = -1

        out.append(f"Case #{tc}: {answer}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai sẽ xây dựng các mảng được tạo trong khi chỉ giữ lại hai giá trị lặp lại trước đó. Các số nguyên chính xác tùy ý của Python xử lý trực tiếp các sản phẩm trung gian lớn. 

Sau khi sắp xếp, quét ngược sẽ tính toán`bad`và điều kiện để trở thành đá phục vụ cuối cùng. Ngăn xếp đơn điệu chỉ chứa nghêu. Bật lên trong khi đỉnh ngăn xếp quá mềm sẽ để lại con ngao gần nhất có độ cứng đủ lớn để đánh bại tảng đá hiện tại. 

Quá trình quét tiến thực hiện truy vấn đối xứng cho`q`. Hai lần quét là riêng biệt vì một lần quét yêu cầu loại ngao đạt tiêu chuẩn gần nhất ở bên phải và lần còn lại yêu cầu loại ngao đạt tiêu chuẩn gần nhất ở bên trái. 

Cửa hàng cây Fenwick`dp[L] - P_L`. Các chỉ số của nó bị đảo ngược, bởi vì sự truy hồi cần một hậu tố tối thiểu trên các đá có`P_L >= q`. Cây Fenwick chỉ nhận được các cập nhật tối thiểu giảm dần nên không cần xóa ở đó. 

Cửa hàng cây phân đoạn`dp[L] - 3P_L`. Những ứng cử viên này là tạm thời. Khi quá trình quét đạt tới`bad[L]`, đá`L`không còn có thể xử lý từng con ngao trong khoảng trống kết thúc ở tảng đá hiện tại nên ứng viên sẽ bị loại. Vùng heap cho chúng ta biết chính xác khi nào mỗi ứng cử viên hết hạn. 

Sự so sánh nghiêm ngặt là có chủ ý. Một tảng đá có độ cứng`h`chỉ có thể bẻ gãy những con nghêu có độ cứng dưới đây`h`, vậy một con nghêu có độ cứng bằng`h`thuộc về tập hợp bị chặn và gây ra một`bad`sự kiện. 

Phép tính cuối cùng cũng sử dụng bất đẳng thức chặt chẽ. Một tảng đá phục vụ cuối cùng có độ cứng`h`chỉ có giá trị nếu mỗi con nghêu còn lại có độ cứng nhỏ hơn`h`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Các vật thể là một con nghêu ở vị trí 5 với độ cứng 30 và một tảng đá ở vị trí 10 với độ cứng 31. 

| Vị trí | Loại | Độ cứng |`q`|`dp`| Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 5 | C | 30 | | | Nhặt nghêu | 
| 10 | R | 31 | 0 | 10 | Rock xử lý ngao khi di chuyển sang phải | 

Đá ở vị trí 10 không có con trai đủ tiêu chuẩn ở bên trái, vì độ cứng của con sò 30 nhỏ hơn 31. Điểm bắt đầu ảo có thể chuyển trực tiếp sang nó, tạo ra`dp = 10`. Không có con sò nào sau tảng đá nên nó cũng là tảng đá phục vụ cuối cùng hợp lệ. Câu trả lời là`10`. 

### Mẫu 2 

Lúc này hòn đá ở vị trí 5 với độ cứng 31 và con nghêu ở vị trí 10 với độ cứng 30. 

| Vị trí | Loại | Độ cứng |`q`|`dp`| Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 5 | R | 31 | 0 | 5 | Tiếp cận tảng đá | 
| 10 | C | 30 | | | Nhặt nghêu | 
| 5 | R | 31 | | | Trở lại và phá vỡ ngao | 

Đá ở vị trí thứ 5 trở thành đá phục vụ cuối cùng. Độ cứng của nó vượt quá độ cứng của ngao, vì vậy sau khi đạt ngao ở mức 10, Percy sẽ quay lại từ 10 đến 5. Khoảng cách cuối cùng bổ sung là`2(10-5) = 10`, cho`5 + 10 = 15`. 

Dấu vết cho thấy tại sao quá trình chuyển đổi dịch vụ cuối cùng lại khác với quá trình chuyển đổi thông thường. Percy không cần quay lại sau lần phục vụ cuối cùng, vì vậy chỉ cần một chuyến trở về từ con ngao xa nhất còn lại đến tảng đá cuối cùng. 

### Mẫu 4 

Các đối tượng là 

| Vị trí | Loại | Độ cứng | 
| --- | --- | --- | 
| 10 | R | 50 | 
| 11 | C | 49 | 
| 50 | R | 10 | 
| 52 | C | 8 | 

Đối với đá 10, không có con trai nào cứng hơn hoặc ngang bằng ở bên phải của nó, vì con ngao ở số 11 có độ cứng 49. Có thể lấy trực tiếp tảng đá đầu tiên với giá 10. 

Đối với đá 50, con nghêu gần nhất bên trái nó có độ cứng ít nhất là 10 là con ngao ở mức 11, do đó`q = 11`. Tảng đá trước đó ở vị trí 10 nằm ở bên trái`q`và độ cứng 50 của nó vượt quá mọi con ngao giữa các vị trí 10 và 50. Chi phí chuyển đổi trở lại`50 + 2*11 + (dp[10] - 3*10) = 50 + 22 + 10 - 30 = 52`. 

Điều này tương ứng với```
0 -> 11 -> 10 -> 50
```với giá 52. Con nghêu còn lại ở 52 có độ cứng 8, đá ở 50 có thể gãy. Vì đây là hòn đá phục vụ cuối cùng nên Percy trả về từ 52 đến 50, cộng thêm 4. Câu trả lời cuối cùng là 56. 

Ví dụ này chứng minh tại sao DP cần cả hai hình thức chuyển tiếp. Một con đường ngắn nhất đơn giản xuyên qua những tảng đá sẽ không nắm bắt được thực tế là con ngao ở điểm 11 phải được vớt lên trước khi quay trở lại tảng đá 10. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Chi phí sắp xếp là O(N log N), trong khi các ngăn xếp đơn điệu, cây Fenwick, cây phân đoạn và đống hết hạn thực hiện tổng công việc là O(N log N) | 
| Không gian | O(N) | Các đối tượng được sắp xếp, thông tin về đá, ngăn xếp, giá trị DP và cấu trúc dữ liệu đều sử dụng bộ nhớ tuyến tính | 

Hoạt động chủ yếu là sắp xếp các đối tượng được tạo theo vị trí. Sau đó, mọi vật thể vào và ra khỏi mỗi ngăn đơn điệu nhiều nhất một lần và mỗi tảng đá chỉ gây ra một số lượng không đổi các phép toán cấu trúc dữ liệu logarit. Với 800.000 đối tượng, đây là thang đo thích hợp cho những ràng buộc nhất định, trong khi các phương án bậc hai vượt xa ngân sách hoạt động hiện có. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm bên dưới giả định giải pháp đã gửi được lưu dưới dạng`solution.py`và phơi bày`solve()`chức năng hiển thị ở trên.```python
import sys
import io

from solution import solve

def run(inp: str) -> str:
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

sample = """\
6
2
5 10 0 0 0 50
30 31 0 0 0 50
CR
2
5 10 0 0 0 50
31 30 0 0 0 50
RC
2
5 10 0 0 0 50
30 30 0 0 0 50
RC
4
10 50 0 1 40 80
50 10 0 1 38 80
RRCC
20
415 711 3 4 3 967
9 2 1 2 9 13
CCCCCRRRCRCCCRRCRRCC
100
168981242 670860915 208968638 604295408 490937286 715757945
627165633 146096256 952913201 456337362 978266551 970054933
CRCCRRRRRRRRCRRCCCRCCRRRRCRCRRRRCCCCRCCRRRCRRRCCCCRCCRCCRCRCCCCCRRCCCRRCRRRRCCCRCRRCRRRRCCCRRCRCCCCC RR
"""

# The six official sample answers are:
# 10, 15, -1, 56, 1099, 890508817.
# The long Case 6 operation string above is split only for readability;
# when using it as an executable literal, remove the space before RR.
#
# The first five cases can be checked independently with the following input.

sample_first_five = """\
5
2
5 10 0 0 0 50
30 31 0 0 0 50
CR
2
5 10 0 0 0 50
31 30 0 0 0 50
RC
2
5 10 0 0 0 50
30 30 0 0 0 50
RC
4
10 50 0 1 40 80
50 10 0 1 38 80
RRCC
20
415 711 3 4 3 967
9 2 1 2 9 13
CCCCCRRRCRCCCRRCRRCC
"""

assert run(sample_first_five) == (
    "Case #1: 10\n"
    "Case #2: 15\n"
    "Case #3: -1\n"
    "Case #4: 56\n"
    "Case #5: 1099"
), "official sample cases 1 through 5"

assert run("""\
1
2
1 2 0 0 0 10
5 5 0 0 0 10
CR
""") == "Case #1: -1", "equal hardness is not sufficient"

assert run("""\
1
3
9 10 0 0 99 100
6 5 0 0 5 10
RCR
""") == "Case #1: 11", "a nearby left rock can beat a far right rock"

assert run("""\
1
3
9 10 0 0 10 11
5 5 0 0 5 10
RCR
""") == "Case #1: 11", "strict inequality and right-side boundary"

n = 800000
large_input = (
    "1\n"
    f"{n}\n"
    f"1 2 0 1 0 {n}\n"
    "1 2 0 1 0 2\n"
    + "CR" * (n // 2)
    + "\n"
)

assert run(large_input) == "Case #1: 800000", "maximum-size linear sweep"
```Bài kiểm tra kích thước tối đa sử dụng các vị trí`1,2,...,800000`và xen kẽ các vật thể bằng ngao và đá. Mỗi con ngao đều có độ cứng 1 và ngay sau đó là đá có độ cứng 2, vì vậy con đường tối ưu chỉ đơn giản là bơi đến vị trí cuối cùng. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`N=2`, độ cứng của ngao và đá bằng nhau |`-1`| So sánh độ cứng nghiêm ngặt | 
|`RCR`với đá ở vị trí 9 và 100 |`11`| Chọn đá phục vụ bên trái gần đó thay vì đá ngoài cùng bên phải | 
|`RCR`có độ cứng 5, 5, 6 |`11`| Sự bình đẳng không làm vỡ được một con ngao, trong khi một tảng đá bên phải cứng hơn thì có thể | 
|`N=800000`, xen kẽ`CR`|`800000`| Kích thước đầu vào tối đa, tạo lặp lại, sắp xếp và cấu trúc dữ liệu O(N log N) | 

## Vỏ cạnh 

Để có độ cứng bằng nhau, các ngăn xếp đơn điệu có chủ ý giữ những con nghêu có độ cứng bằng với đá hiện tại. Trong đầu vào```
2
1 2 0 0 0 10
5 5 0 0 0 10
CR
```độ cứng của đá không lớn hơn độ cứng của ngao. Đá không thể phục vụ ngao,`final_ok`là sai và không có tảng đá nào khác tồn tại nên câu trả lời là`-1`. 

Đối với một tảng đá bên trái gần đó so với một tảng đá bên phải ở xa, hãy xem xét```
3
9 10 0 0 99 100
6 5 0 0 5 10
RCR
```Các vị trí được tạo ra là 9, 10 và 100. Đá ở 9 có độ cứng 6 và nghêu ở 10 có độ cứng 5. DP có thể sử dụng điểm khởi đầu ảo để đến đá 9, sau đó quá trình chuyển đổi cuối cùng quay trở lại từ nghêu ở 10 sang đá 9. Chi phí của nó là`9 + 2(10-9) = 11`. Đá xa ở mức 100 không liên quan đến mức tối ưu. 

Đối với một tảng đá được ghé thăm trước một con ngao đến sau, quá trình chuyển đổi cuối cùng rõ ràng là nguyên nhân dẫn đến sự quay trở lại. Trong trường hợp mẫu thứ tư, đá 50 đã đạt đến trước khi Percy nhặt ngao ở số 52, vì vậy ngao không thể được coi là đã ăn trong lần ghé thăm đầu tiên đó. Tính toán cuối cùng thêm`2(52-50)=4`, tạo ra tổng số câu trả lời là 56 sau lần giao bóng trước đó ở tảng đá 10. 

Đối với giá trị được tạo lớn, phép truy hồi được đánh giá bằng số nguyên Python trước khi áp dụng mô đun. Mã không bao giờ chuyển đổi các sản phẩm trung gian thành giá trị 32 bit. Bước sắp xếp sau đó sẽ thiết lập thứ tự không gian thực tế một cách độc lập với thứ tự mà phép lặp tạo ra các đối tượng. 

Đối với một tảng đá không có vỏ sò đủ cứng ở bên phải,`bad`trở thành vô cùng. Ứng cử viên chuyển tiếp trở lại của nó không bao giờ hết hạn vì nó vẫn có khả năng xử lý mọi khoảng thời gian sau đó. Ngược lại, nếu`bad[L]`là vị trí nghêu thật, thí sinh bị loại ngay khi quét tới vị trí đó, phù hợp với yêu cầu khắt khe là mọi nghêu trong khoảng phải có độ cứng dưới`H_L`. 

Đá phục vụ cuối cùng được xử lý riêng biệt với các quá trình chuyển đổi thông thường vì Percy không cần phải quay lại bên phải sau khi con ngao cuối cùng đã được ăn xong. Nếu tảng đá cuối cùng ở`L`và con nghêu xa nhất còn lại là ở`C`, khoảng cách cần tìm là`2(C-L)`, không`2(C-L)+(C-L)`. Sự khác biệt này là nguyên nhân khiến chi phí cuối cùng của mẫu là 56 thay vì giá trị lớn hơn.
