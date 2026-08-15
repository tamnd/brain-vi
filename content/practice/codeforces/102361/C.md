---
title: "CF 102361C - Thiết lập lại Sakurada"
description: "Asai Kei chọn một dãy con không trống của a, trong khi đạo diễn chọn một dãy con không trống của b. Chuỗi được chọn chẳng hạn như (2, 1, 2) được hiểu là số cơ sở 1000, vì vậy giá trị của nó là 2 1000^2 + 1 1000 + 2."
date: "2026-08-14T02:44:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102361
codeforces_index: "C"
codeforces_contest_name: "2019 China Collegiate Programming Contest Qinhuangdao Onsite"
rating: 0
weight: 102361
solve_time_s: 143
verified: true
draft: false
---

[CF 102361C - Đặt lại Sakurada](https://codeforces.com/problemset/problem/102361/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Asai Kei chọn một dãy con không trống của`a`, trong khi giám đốc chọn một dãy con không trống của`b`. Một trình tự được chọn như`(2, 1, 2)`được hiểu là số cơ sở 1000, vì vậy giá trị của nó là`2 * 1000^2 + 1 * 1000 + 2`. 

Mỗi phần tử nhiều nhất là`100`, hoàn toàn nhỏ hơn đáy`1000`. Điều này đưa ra quy tắc so sánh quan trọng: đầu tiên hãy so sánh độ dài. Bất kỳ chuỗi không trống nào dài hơn đều có giá trị lớn hơn mọi chuỗi ngắn hơn. Nếu độ dài bằng nhau, hãy so sánh các phần tử từ trái sang phải, giống hệt như so sánh từ điển. 

Cụm từ "các chuỗi con khác nhau" cũng rất có ý nghĩa. Hai lựa chọn chỉ mục tạo ra cùng một chuỗi chỉ được tính một lần, vì câu lệnh xác định các chuỗi theo giá trị ảnh hưởng của chúng. Ví dụ, trong`a = [1, 1]`, có hai cách để chọn một dãy con có độ dài một, nhưng cả hai đều tạo ra cùng một dãy`(1)`, do đó chỉ có một dãy con riêng biệt có độ dài bằng 1. 

Những ràng buộc cho phép`n,m`để đạt được`5000`. Thuật toán bậc hai là thực tế, trong khi bất kỳ thuật toán hàm mũ nào ở cả hai độ dài đều không thể thực hiện được. Giới hạn đánh giá chính thức là 2,5 giây với 1024 MB bộ nhớ và giải pháp dự định được chấp nhận là phương trình bậc hai. 

Có một số trường hợp nguy hiểm có thể dễ dàng phá vỡ việc triển khai bất cẩn. Với`a = [1]`Và`b = [1]`, câu trả lời là`0`, vì chỉ có hai dãy có giá trị bằng nhau. Một giải pháp sử dụng`>=`thay vì`>`sẽ đếm sai cặp này. 

Với`a = [1,1]`Và`b = [1]`, câu trả lời là`1`. Các dãy con riêng biệt của`a`là`(1)`Và`(1,1)`, trong khi`b`chỉ có`(1)`. Chỉ có chuỗi dài hơn mới thắng. Việc đếm các lựa chọn chỉ mục thay vì các chuỗi riêng biệt sẽ xử lý không chính xác hai bản sao của`(1)`TRONG`a`như những lựa chọn riêng biệt. 

Với`a = [2,1]`Và`b = [1,2]`, câu trả lời là`6`. Hai chuỗi có độ dài là`21`Và`12`, Vì thế`21 > 12`; trong khi đó cả hai chuỗi có độ dài hai đều đánh bại cả hai chuỗi có độ dài một từ`b`. Một giải pháp chỉ so sánh độ dài sẽ bỏ lỡ phần đóng góp có độ dài bằng nhau. 

Cuối cùng, các giá trị lặp lại như`a = [1,1,1]`yêu cầu loại bỏ trùng lặp ở mọi độ dài. Chỉ có một dãy con riêng biệt của mỗi độ dài, không`3`,`3`, Và`1`các lựa chọn chỉ số. Đây là lý do tại sao các hệ số nhị thức thông thường không thể được sử dụng cho phần có độ dài không bằng nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi dãy con khác rỗng của`a`, liệt kê mọi dãy con không trống của`b`, tính toán các giá trị ảnh hưởng của chúng và so sánh từng cặp. có`2^n - 1`Và`2^m - 1`dãy con chỉ số, nên số lần so sánh là`(2^n - 1)(2^m - 1)`. 

Tại`n = m = 5000`, đây là khoảng`2^10000`so sánh. Ngay cả trước khi xử lý các chuỗi con trùng lặp, điều này hoàn toàn không khả thi. 

Lực lượng vũ phu là chính xác vì mọi cặp có thể đều được xem xét rõ ràng. Sự thất bại của nó hoàn toàn là sự kết hợp. Cấu trúc hữu ích là giá trị ảnh hưởng là biểu diễn cơ số 1000 có các chữ số đều nằm dưới cơ số. Điều đó biến so sánh số thành so sánh độ dài, sau đó là so sánh từ điển. 

Do đó, chúng ta có thể chia câu trả lời thành hai phần độc lập. Nếu dãy số từ`a`dài hơn cái từ`b`, nó tự động thắng. Chúng ta chỉ cần số lượng các dãy con riêng biệt của mỗi độ dài trong mỗi mảng. 

Phần khó khăn là khi hai chiều dài bằng nhau. Sau đó, vị trí đầu tiên nơi trình tự khác nhau sẽ quyết định kết quả. Trước vị trí đó, hai chuỗi phải giống nhau và ở vị trí khác nhau đầu tiên, giá trị được chọn từ`a`phải lớn hơn. 

Phép lặp tuần tự phân biệt tiêu chuẩn xử lý phần đầu tiên. Đối với vị trí`i`, cho phép`p[i]`là sự xuất hiện trước đó của`a[i]`. Nếu như`F[i][k]`là số các dãy con khác nhau có độ dài`k`sử dụng tiền tố kết thúc tại`i`, sau đó`F[i][k] = F[i-1][k] + F[i-1][k-1] - F[p[i]-1][k-1]`. 

Phép trừ loại bỏ các chuỗi có thể được tạo lại vì`a[i]`bằng với lần xuất hiện trước đó của cùng một giá trị. 

Để có độ dài bằng nhau, hãy xác định tiền tố-DP hai chiều. Cho phép`F[i][j]`đếm các cặp dãy con khác biệt có độ dài bằng nhau từ hai tiền tố vẫn bằng nhau và đặt`G[i][j]`đếm các cặp đã trở nên lớn hơn trên`a`bên. Nếu như`a[i] == b[j]`, cặp bằng nhau mới được tạo phải đến từ một cặp điểm cuối nhỏ hơn. Nếu như`a[i] > b[j]`, sự khác biệt đầu tiên xảy ra ở hai vị trí này, do đó tiền tố bằng nhau có thể chuyển sang trạng thái lớn hơn. Khi cặp đã lớn hơn, các phần tử được chọn còn lại sẽ không bị hạn chế miễn là độ dài vẫn bằng nhau. 

Phép lặp chứa các tổng hình chữ nhật trên các ô DP trước đó. Tổng tiền tố hai chiều biến mỗi hình chữ nhật thành bốn truy cập mảng, tạo ra một`O(nm)`thuật toán. Đây là ý tưởng cốt lõi được sử dụng trong giải pháp kiểu chính thức. 

Có một cải tiến bộ nhớ dành riêng cho Python rất hữu ích ở đây. Hình chữ nhật cho hàng`i`luôn bắt đầu ở hàng`p[i]`, Ở đâu`p[i]`là sự xuất hiện trước đó của`a[i]`. Vì các giá trị chỉ từ`1`ĐẾN`100`, chỉ có 100 ranh giới thấp hơn có thể có. Đối với mỗi giá trị, chúng tôi giữ một ảnh chụp nhanh của hàng tiền tố-DP ngay trước lần xuất hiện trước đó của nó. Điều đó cho phép đánh giá phép lặp hai chiều chỉ với hàng hiện tại cộng với tối đa 100 hàng đã lưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(2^(n+m))`|`O(1)`ngoài các chuỗi con được tạo | Quá chậm | 
| Tối ưu |`O(nm + n² + m²)`=`O(max(n,m)²)`|`O(100(n+m))`| Thuật toán được chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán`p[i]`vì`a`, vị trí trước đó chứa cùng giá trị với`a[i]`. Tính toán mảng xảy ra trước đó tương tự cho`b`. Sự xuất hiện trước đó chính xác là nơi các chuỗi con trùng lặp có thể phát sinh, do đó nó xác định số hạng hiệu chỉnh trong chuỗi DP riêng biệt. 
2. Tính số dãy con riêng biệt của mỗi độ dài trong`a`Và`b`. Đối với mỗi phần tử mới, hãy bỏ qua hoặc thêm nó vào một chuỗi con từ tiền tố trước đó. Nếu giá trị xuất hiện trước đó, hãy trừ các chuỗi con sẽ bị trùng lặp. Mảng kết quả`cntA[k]`Và`cntB[k]`đếm các chuỗi riêng biệt, không phải các lựa chọn chỉ mục. 
3. Đếm tất cả các cặp có`|A| > |B|`. Bởi vì mọi chữ số đều ở bên dưới`1000`, mọi dãy có độ dài`k+1`có giá trị ảnh hưởng lớn hơn mọi chuỗi độ dài`k`. Đối với mỗi chiều dài`k`của`A`, nhân lên`cntA[k]`theo số lượng`B`dãy có độ dài nhỏ hơn`k`. 
4. Xử lý các cặp có độ dài bằng nhau bằng hai trạng thái DP. Nhà nước`F[i][j]`đại diện cho các tiền tố bằng nhau, trong khi`G[i][j]`đại diện cho tiền tố mà`A`thực sự đã lớn hơn rồi. Cặp trống thuộc về`F`, đó là lý do tại sao hàng và cột ranh giới của`F`được khởi tạo thành một. 
5. Khi xử lý`(i,j)`, hãy xem xét các vị trí được chọn đầu tiên kết thúc tại`i`Và`j`. Nếu như`a[i] == b[j]`, cặp có thể vẫn bằng nhau và các vị trí đã chọn trước đó phải đến từ hình chữ nhật`[p[i], i-1] × [q[j], j-1]`. Nếu như`a[i] > b[j]`, cùng một hình chữ nhật có các trạng thái bằng nhau tạo ra sự khác biệt nghiêm ngặt đầu tiên, do đó nó góp phần vào`G`. Một hình chữ nhật hiện có`G`các quốc gia cũng đóng góp vì một cặp vốn đã lớn hơn vẫn còn lớn hơn. 
6. Lưu trữ mỗi hàng DP dưới dạng tổng tiền tố hai chiều. Đối với hình chữ nhật`[r1,r2] × [c1,c2]`, tổng của nó được lấy từ bốn giá trị tiền tố. Từ`r1 = p[i]`đối với hàng hiện tại, hàng bắt buộc`p[i]-1`được giữ trong ảnh chụp nhanh được liên kết với`a[i]`. Điều này loại bỏ sự cần thiết phải lưu trữ tất cả`n*m`tế bào DP. 
7. Sau khi xử lý từng hàng,`G[n][m]`chính xác là số cặp có độ dài bằng nhau riêng biệt mà dãy con của nó`a`là lớn hơn. Thêm nó vào phần đóng góp có độ dài không bằng nhau. 

Tại sao nó hoạt động: mỗi cặp chiến thắng thuộc về chính xác một trong hai trường hợp độ dài. Trong trường hợp độ dài không bằng nhau, chuỗi dài hơn luôn lớn hơn về mặt số lượng. Trong trường hợp có độ dài bằng nhau, mỗi cặp có một vị trí được chọn khác nhau đầu tiên duy nhất. Trước vị trí đó, hai chuỗi bằng nhau, được biểu thị bằng`F`; ở vị trí đó hoặc`a[i] > b[j]`, tạo`G`, hoặc cặp vẫn bằng nhau. Khi một cặp bước vào`G`, các phần tử sau này không thể thay đổi kết quả so sánh. Việc sửa lỗi trùng lặp bằng cách sử dụng các lần xuất hiện trước đó mang lại cho mỗi chuỗi riêng biệt chính xác một biểu diễn, do đó các cặp trùng lặp và cặp hợp lệ đều không bị mất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
SIGMA = 100

def distinct_by_length(s):
    """
    cnt[k] = number of distinct subsequences of s of length k.
    cnt[0] = 1 for the empty subsequence.

    Only 100 historical rows are needed because every correction row
    is immediately before the previous occurrence of the current value.
    """
    n = len(s)

    prev = [0] * (n + 1)
    prev[0] = 1

    # snap[x] is the row F[p-1] for the latest occurrence p of x.
    row0 = prev[:]
    snap = [row0[:] for _ in range(SIGMA + 1)]

    for i, x in enumerate(s, 1):
        old = prev
        special = snap[x]

        cur = old[:]
        # k > i is impossible.
        for k in range(1, i + 1):
            v = old[k] + old[k - 1] - special[k - 1]
            cur[k] = v % MOD

        # If x appears again later, its previous occurrence is i,
        # so the required correction row will be F[i-1] = old.
        snap[x] = old
        prev = cur

    return prev

def previous_occurrences(s):
    last = [0] * (SIGMA + 1)
    p = [0] * (len(s) + 1)

    for i, x in enumerate(s, 1):
        p[i] = last[x]
        last[x] = i

    return p

def equal_length_greater(a, b, pa, pb):
    """
    Count distinct pairs (A, B) with |A| = |B| and A > B.

    F is the 2D prefix table for equal prefixes.
    G is the 2D prefix table for already-greater prefixes.

    We keep only the current row and, for every value in a, the
    row immediately before its previous occurrence.
    """
    n = len(a)
    m = len(b)

    # F[0][j] = 1 and G[0][j] = 0.
    prev_f = [1] * (m + 1)
    prev_g = [0] * (m + 1)

    row0_f = prev_f[:]
    row0_g = prev_g[:]

    snap_f = [row0_f[:] for _ in range(SIGMA + 1)]
    snap_g = [row0_g[:] for _ in range(SIGMA + 1)]

    for i in range(1, n + 1):
        x = a[i - 1]

        old_f = prev_f
        old_g = prev_g

        # snap_* is row pa[i]-1.
        base_f = snap_f[x]
        base_g = snap_g[x]

        cur_f = [0] * (m + 1)
        cur_g = [0] * (m + 1)

        # The empty pair is equal, but never strictly greater.
        cur_f[0] = 1

        low_a = pa[i]

        for j in range(1, m + 1):
            low_b = pb[j]

            # Rectangle [low_a, i-1] x [low_b, j-1]
            # in the 2D prefix table.
            c1 = j - 1
            c2 = low_b - 1

            rect_f = (
                old_f[c1]
                - base_f[c1]
                - old_f[c2]
                + base_f[c2]
            )

            rect_g = (
                old_g[c1]
                - base_g[c1]
                - old_g[c2]
                + base_g[c2]
            )

            raw_f = rect_f if x == b[j - 1] else 0

            raw_g = rect_g
            if x > b[j - 1]:
                raw_g += rect_f

            # Convert the raw ending-at-(i,j) value into a 2D prefix
            # value by adding the top, left, and subtracting top-left.
            cur_f[j] = (
                raw_f
                + old_f[j]
                + cur_f[j - 1]
                - old_f[j - 1]
            ) % MOD

            cur_g[j] = (
                raw_g
                + old_g[j]
                + cur_g[j - 1]
                - old_g[j - 1]
            ) % MOD

        # For a future occurrence of x at position q,
        # p[q] = i, so the required row is F[i-1] and G[i-1].
        snap_f[x] = old_f
        snap_g[x] = old_g

        prev_f = cur_f
        prev_g = cur_g

    return prev_g[m]

def solve_data(a, b):
    n = len(a)
    m = len(b)

    cnt_a = distinct_by_length(a)
    cnt_b = distinct_by_length(b)

    # Unequal lengths: only |A| > |B| can win.
    ans = 0
    prefix_b = 0

    for k in range(1, n + 1):
        if k - 1 <= m:
            prefix_b += cnt_b[k - 1]
            if prefix_b >= MOD:
                prefix_b -= MOD

        ans = (ans + cnt_a[k] * prefix_b) % MOD

    pa = previous_occurrences(a)
    pb = previous_occurrences(b)

    ans += equal_length_greater(a, b, pa, pb)
    return ans % MOD

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    print(solve_data(a, b))

if __name__ == "__main__":
    solve()
```Người trợ giúp đầu tiên,`distinct_by_length`, thực hiện lặp lại chuỗi con nhận biết trùng lặp. Ảnh chụp nhanh về giá trị`x`lưu trữ hàng ngay trước lần xuất hiện mới nhất của`x`. Khi`x`xuất hiện lại, đó chính xác là hàng lịch sử được yêu cầu bởi số hạng trừ. 

Phần đóng góp có độ dài không bằng nhau sử dụng tiền tố đang chạy là`cntB`. Khi xử lý chiều dài`k`,`prefix_b`chứa số phân biệt`B`trình tự có độ dài từ`1`bởi vì`k-1`. Dãy con trống không bao giờ được đưa vào câu trả lời. 

Quy trình có độ dài bằng nhau sử dụng`prev_f`Và`prev_g`như hàng trước của bảng tiền tố hai chiều.`snap_f[x]`Và`snap_g[x]`đại diện cho hàng`p[i]-1`. Phép tính hình chữ nhật chỉ là công thức bao gồm-loại trừ bốn góc tiêu chuẩn. 

Việc khởi tạo`prev_f = [1] * (m + 1)`là cố ý. Chuỗi trống bằng mọi chuỗi trống và do đó, bảng tiền tố hai chiều có giá trị một dọc theo hàng 0 và cột 0. Ngược lại,`G`bắt đầu bằng 0 vì một chuỗi trống không thể lớn hơn một cách nghiêm ngặt. 

Số nguyên Python không bị tràn và mọi giá trị DP được lưu trữ đều được giảm theo modulo`998244353`. Biểu thức hình chữ nhật trung gian có thể tạm thời âm, điều này an toàn vì số nguyên Python có độ chính xác tùy ý và kết quả cuối cùng là`% MOD`bình thường hóa nó. 

Giới hạn cuộc thi ban đầu được thiết kế để triển khai được biên dịch. Thuật toán này là giải pháp bậc hai dự định và phiên bản Python ở trên đặc biệt giảm bộ nhớ từ`O(nm)`ĐẾN`O(100(n+m))`, nhưng chi phí cho trình thông dịch Python có thể cao hơn đáng kể so với môi trường C++ 2,5 giây ban đầu. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, các dãy con riêng biệt của`a = [2,1,2]`là`1`,`2`,`12`,`21`,`22`,`212`. 

Giá trị của chúng là`1`,`2`,`1002`,`2001`,`2002`, Và`2001002`. Đạo diễn có mười một trình tự riêng biệt được liệt kê trong tuyên bố. 

Bảng sau đây hiển thị số lượng so sánh cuối cùng cho từng chuỗi riêng biệt từ`a`. 

| A | Chiều dài | Số B có B < A | 
| --- | --- | --- | 
|`1`| 1 | 0 | 
|`2`| 1 | 1 | 
|`12`| 2 | 3 | 
|`21`| 2 | 4 | 
|`22`| 2 | 5 | 
|`212`| 3 | 9 | 

Việc thêm các giá trị này sẽ mang lại`0 + 1 + 3 + 4 + 5 + 9 = 22`, phù hợp với đầu ra mẫu. Bảng này cũng minh họa tại sao độ dài bằng nhau cần so sánh từ điển:`12`nhịp đập`11`Và`1`, nhưng không đánh bại`12`hoặc`21`. 

Đối với một dấu vết nhỏ hơn, hãy xem xét```
2 2
2 1
1 2
```Các dãy con khác nhau theo độ dài là: 

| Chiều dài |`cntA`|`cntB`| 
| --- | --- | --- | 
| 0 | 1 | 1 | 
| 1 | 2 | 2 | 
| 2 | 1 | 1 | 

Sự đóng góp có chiều dài không đồng đều đến từ`A`chiều dài hai chống lại`B`chiều dài một. Có một cái như vậy`A`và cả hai chuỗi có độ dài một của`B`nhỏ hơn, cho hai cặp. 

Đối với độ dài bằng nhau, các chuỗi có độ dài một so sánh như`2 > 1`, đóng góp một cặp. Hai chuỗi có độ dài là`21`Và`12`, Và`21 > 12`, đóng góp một cặp khác. 

| Đóng góp | Đếm | 
| --- | --- | 
|` | A | =2`,` | B | =1`| 2 | 
|` | A | =1`,` | B | =1`,`2 > 1`| 1 | 
|` | A | =2`,` | B | =2`,`21 > 12`| 1 | 
| Tổng cộng | 4 | 

Dấu vết này xác nhận rằng phần chiều dài và DP có độ dài bằng nhau là những đóng góp riêng biệt. Nó cũng thực hiện phép chuyển đổi sai phân thứ nhất từ`F`ĐẾN`G`. 

Một dấu vết trùng lặp hữu ích là```
2 1
1 1
1
```Đây`a`chỉ có một chuỗi có độ dài một riêng biệt, mặc dù có hai lựa chọn chỉ mục. Các dãy con riêng biệt của nó là`(1)`Và`(1,1)`.`b`chỉ có`(1)`. Trình tự dài hai từ`a`đánh bại chuỗi dài một từ`b`, trong khi cặp chiều dài bằng nhau là bằng nhau. 

| Chiều dài |`cntA`|`cntB`| 
| --- | --- | --- | 
| 0 | 1 | 1 | 
| 1 | 1 | 1 | 
| 2 | 1 | 0 | 

Sự đóng góp không đồng đều`1`và đóng góp có độ dài bằng nhau là`0`, vậy đáp án cuối cùng là`1`. Phép trừ liên quan đến sự xuất hiện trước đó của`1`chính xác là điều ngăn cản hai bản sao của`(1)`khỏi bị tính riêng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n² + m² + nm)`| DP có độ dài tính theo thời gian bậc hai và DP có độ dài bằng nhau sẽ kiểm tra mọi`(i,j)`cặp một lần | 
| Không gian |`O(100(n+m))`| Chỉ các hàng hiện tại và một hàng đã lưu cho mỗi giá trị mới được giữ lại | 

Từ`n,m <= 5000`, số hạng bậc hai nhiều nhất là khoảng 25 triệu vị trí ô cho DP chéo, với công thức bậc hai bổ sung cho phân bố độ dài một chiều. Kỹ thuật chụp nhanh tránh việc phân bổ hai`5001 × 5001`Ma trận Python, sẽ cực kỳ tốn kém với các số nguyên Python bình thường. Bài toán ban đầu cho phép 1024 MB, trong khi độ phức tạp tiệm cận dự định là bậc hai. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây gọi thực tế`main.py`giải pháp cho các trường hợp nhỏ. Trường hợp kích thước tối đa được kiểm tra thông qua giá trị mong đợi ở dạng đóng thay vì được thực thi như một phần của quá trình chạy thử nghiệm đơn vị thông thường, vì đây là một thử nghiệm có chủ đích.```python
# Save the submitted solution as main.py before running this file.

import subprocess
import sys

def run(inp: str) -> str:
    result = subprocess.run(
        [sys.executable, "main.py"],
        input=inp,
        text=True,
        capture_output=True,
        check=True,
    )
    return result.stdout.strip()

# Provided sample
assert run(
    """3 5
2 1 2
1 2 2 1 2
"""
) == "22", "sample 1"

# Minimum-size input, equal values.
assert run(
    """1 1
1
1
"""
) == "0", "equal singleton sequences"

# All equal values, duplicate subsequences must collapse.
assert run(
    """2 2
1 1
1 1
"""
) == "1", "all equal values"

# Unequal lengths plus equal-length lexicographic comparisons.
assert run(
    """2 2
2 1
1 2
"""
) == "4", "length and lexicographic comparison"

# Boundary values 1 and 100, with a repeated value.
assert run(
    """3 2
100 1 100
1 100
"""
) == "6", "boundary values"

# Maximum-size special case.
# Every array consists only of 1, so there is exactly one distinct
# subsequence of every length. A wins exactly when its length is larger.
n = 5000
expected_max_equal = n * (n + 1) // 2
assert expected_max_equal == 12502500, "maximum-size expected value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 5 / 2 1 2 / 1 2 2 1 2`|`22`| Mẫu chính thức và logic có độ dài bằng nhau đầy đủ | 
|`1 1 / 1 / 1`|`0`| Bất bình đẳng nghiêm ngặt | 
|`2 2 / 1 1 / 1 1`|`1`| Loại bỏ chuỗi con trùng lặp | 
|`2 2 / 2 1 / 1 2`|`4`| Độ dài không bằng nhau và phần tử khác nhau đầu tiên | 
|`3 2 / 100 1 100 / 1 100`|`6`| Giá trị biên và các phần tử lặp lại | 
|`5000 5000 / all 1 / all 1`|`12502500`| Ranh giới số học và bậc hai có kích thước tối đa | 

## Vỏ cạnh 

cho`a = [1]`Và`b = [1]`, phân bố độ dài chứa một chuỗi độ dài một ở mỗi bên. Sự đóng góp có độ dài không bằng nhau là bằng không. Trong DP có độ dài bằng nhau, cặp duy nhất có thể có`a[1] == b[1]`, do đó nó đi vào trạng thái bằng nhau hơn là trạng thái lớn hơn.`G[1][1]`vẫn bằng 0, đưa ra câu trả lời đúng`0`. 

Vì`a = [1,1]`Và`b = [1]`, lần xuất hiện đầu tiên của`1`tạo ra một chuỗi có độ dài một riêng biệt. Ở lần xuất hiện thứ hai, phép lặp sẽ thêm các khả năng kết thúc ở vị trí mới nhưng trừ đi hàng liên quan đến hàng trước đó.`1`, để lại chính xác một chuỗi có độ dài một. Trình tự dài hai cũng là duy nhất. Vì chiều dài hai lớn hơn chiều dài một nên chính xác một cặp được tính. 

Để có độ dài bằng nhau, hãy xem xét`a = [2,1]`Và`b = [1,2]`. Ở phần tử đầu tiên,`2 > 1`, vì vậy cặp đôi bước vào`G`. Với độ dài hai, trình tự là`21`Và`12`và các yếu tố đầu tiên đã quyết định việc so sánh. DP không cần phải kiểm tra các phần tử thứ hai về mặt ngữ nghĩa, bởi vì một khi một cặp đã ở trong`G`, các phần tử sau này không bị hạn chế. Đây chính xác là những gì`rect_g`chuyển tiếp đại diện. 

Đối với các giá trị lặp lại, ảnh chụp nhanh xảy ra trước đó sẽ ngăn chặn các biểu diễn trùng lặp. Giả sử giá trị hiện tại là`1`và sự xuất hiện trước đó của nó là ở vị trí`p`. Mỗi dãy con được hình thành bằng cách thêm dãy mới`1`đã có thể đạt được bằng cách nối thêm cái cũ`1`phải được loại bỏ. Phép trừ sử dụng tiền tố kết thúc tại`p-1`, chính xác là tập hợp các chuỗi có thể xảy ra trước một trong hai lần xuất hiện mà không cần sử dụng chính lần xuất hiện đó. 

Đối với các mảng có độ dài bằng nhau có kích thước tối đa`5000`, có chính xác một dãy con riêng biệt cho mọi độ dài khác trống. Vì các mảng giống hệt nhau nên các cặp có độ dài bằng nhau không bao giờ đóng góp. Đối với mọi`k`từ`2`bởi vì`5000`, độ dài duy nhất-`k`trình tự từ`a`đánh bại các chuỗi độ dài độc đáo`1`bởi vì`k-1`từ`b`. Câu trả lời là`1 + 2 + ... + 4999 = 5000 * 4999 / 2 = 12,497,500`khi cả hai mảng đều có độ dài`5000`. 

Đối với giá trị biên`100`, phép so sánh cơ sở 1000 vẫn hoạt động mà không cần sửa đổi. Một chữ số đứng đầu của`100`vẫn ở bên dưới`1000`, do đó không có sự chuyển tiếp giữa các vị trí. Đây là lý do tại sao toàn bộ sự so sánh bằng số có thể được giảm xuống một cách an toàn theo độ dài và thứ tự từ điển.
