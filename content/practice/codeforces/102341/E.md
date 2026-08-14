---
title: "CF 102341E - Eevee"
description: "Có (k) ngăn xếp, mỗi ngăn chứa một mảnh của mỗi (n) viên đá. Vì mỗi ngăn xếp chứa mỗi viên đá đúng một lần nên mỗi ngăn xếp là một hoán vị của (1,ldots,n)."
date: "2026-08-14T01:37:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102341
codeforces_index: "E"
codeforces_contest_name: "Radewoosh+mnbvmar Contest (supported by AIM Tech)"
rating: 0
weight: 102341
solve_time_s: 680
verified: true
draft: false
---

[CF 102341E - Eevee](https://codeforces.com/problemset/problem/102341/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 11p 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có (k) ngăn xếp, mỗi ngăn chứa một mảnh của mỗi (n) viên đá. Vì mỗi ngăn xếp chứa mỗi viên đá đúng một lần nên mỗi ngăn xếp là một hoán vị của (1,\ldots,n). 

Đối với một nhóm ngăn xếp liên tiếp được chọn ([l,r]), chúng tôi xen kẽ nội dung của chúng trong khi vẫn giữ nguyên thứ tự bên trong của mỗi ngăn xếp. Tương tự, chúng ta xây dựng một chuỗi các chỉ mục ngăn xếp, sử dụng mọi chỉ mục trong ([l,r]) chính xác (n) lần. Giá trị phân đoạn được tạo ra bằng cách chọn ngăn xếp là giá trị cao nhất hiện tại của nó. 

Một công trình bị coi là xấu nếu một số viên đá xuất hiện (r-l+1) liên tiếp. Vì các ngăn xếp được chọn đều khác nhau nên một lần chạy như vậy chứa chính xác một lần xuất hiện của viên đá đó từ mỗi ngăn xếp trong khoảng thời gian. Gọi (f(l,r)) là số lần xen kẽ tránh được mỗi lần chạy như vậy. Câu trả lời bắt buộc là tổng của (f(l,r)) trên tất cả các khoảng chứa ít nhất hai ngăn xếp. 

Các hoán vị được tạo ra một cách độc lập và thống nhất một cách ngẫu nhiên. Sự ngẫu nhiên hóa này là một phần của lập luận phức tạp dự kiến: đối với một cặp đá cố định, xác suất để thứ tự tương đối của chúng giống nhau trong mọi ngăn xếp có khoảng chiều dài (s) là (2^{-s+1}). Vấn đề chính thức được tổ chức bởi Codeforces và một bài xã luận độc lập cho cùng một vấn đề sẽ đưa ra kết quả phức tạp dự kiến ​​là (O(n^2k+nk^2)). 

Các giới hạn (n,k\le300) loại trừ bất cứ điều gì gần với việc liệt kê tất cả các phần xen kẽ. Ngay cả đối với một khoảng chứa (k) ngăn xếp, số lượng xen kẽ không hạn chế là 

[ 
\frac{(kn)!}{(n!)^k}. 
] 

Tại (n=k=300), điều này vượt xa mọi phép liệt kê thực tế. Chúng ta cần khai thác thực tế rằng một đường chạy bị cấm chứa một bản sao của cùng một viên đá từ mỗi ngăn xếp và mỗi viên đá xuất hiện chính xác một lần trong mỗi ngăn xếp. 

Có một số trường hợp ranh giới có thể dễ dàng phá vỡ quá trình triển khai. Đầu tiên, (k=2) có nghĩa là khoảng cấm bao gồm hai đoạn liền kề bằng nhau. Vì```
2 2
1 2
2 1
```có (6) sự xen kẽ không hạn chế. Chính xác có 4 điểm xấu nên đáp án là (2). Một giải pháp chỉ kiểm tra chuỗi giá trị cuối cùng mà quên rằng hai thứ tự ngăn xếp phải được giữ nguyên có thể tính các sắp xếp không hợp lệ. 

Thứ hai, cả hai viên đá đều có thể được chọn trong trường hợp loại trừ bao gồm khi chúng có cùng thứ tự tương đối trong mọi ngăn xếp. Vì```
3 2
1 2
1 2
2 1
```khoảng thời gian của hai ngăn xếp đóng góp (2) mỗi ngăn xếp, trong khi khoảng thời gian của cả ba ngăn xếp đóng góp (66). Tổng số câu trả lời là (70). Một giải pháp chỉ kiểm tra xem mỗi viên đá riêng lẻ có thể tạo thành một đường chạy hay không sẽ bỏ lỡ phần giao nhau. 

Thứ ba, các hoán vị giống hệt nhau là một phép kiểm tra tính chính xác hữu ích mặc dù chúng không đại diện cho phân bố đầu vào ngẫu nhiên. Vì```
2 3
1 2 3
1 2 3
```câu trả lời cho khoảng duy nhất là (4). Ở đây, nhiều sự kiện bị cấm tương thích đồng thời, do đó việc triển khai giả định các sự kiện xấu khác nhau là độc lập sẽ thất bại. 

Cuối cùng, một đầu vào trong đó mọi giá trị trong ngăn xếp đều bằng nhau không phải là một trường hợp kiểm thử hợp pháp. Mỗi hàng phải là một hoán vị, do đó, bài kiểm tra căng thẳng được cho là "tất cả các giá trị bằng nhau" thay vào đó phải sử dụng các hoán vị lặp lại, chẳng hạn như hai bản sao của (1,2,\ldots,n). Việc coi đầu vào là một ma trận tùy ý có thể che giấu sự khác biệt này. 

## Phương pháp tiếp cận 

Lực lượng vũ phu trực tiếp là khái niệm đơn giản. Đối với mỗi khoảng ([l,r]), hãy liệt kê mọi chuỗi lựa chọn ngăn xếp chứa chính xác (n) bản sao của mỗi chỉ số ngăn xếp (r-l+1). Đối với mỗi chuỗi, hãy mô phỏng các ngăn xếp và kiểm tra xem có xảy ra một chuỗi chiều dài (r-l+1) với một viên đá hay không. Phương pháp này đúng vì nó xem xét rõ ràng mọi sự đan xen có thể có và áp dụng chính xác định nghĩa về một công trình tốt. 

Đối với một khoảng (s) ngăn xếp, số lượng trình tự được kiểm tra là 

[ 
\frac{(sn)!}{(n!)^s}, 
] 

và kiểm tra chi phí một chuỗi (O(sn)). Vì vậy ngay cả một khoảng lớn cũng đã yêu cầu 

[ 
O\left(sn\frac{(sn)!}{(n!)^s}\right) 
] 

hoạt động. Khoảng tồi tệ nhất có (s=k), vì vậy phương pháp này hoàn toàn không khả thi. 

Quan sát hữu ích là đếm các công trình xây dựng xấu bằng cách loại trừ. Sửa một khoảng chứa (các) ngăn xếp. Với mỗi hòn đá (x), gọi (E_x) là biến cố (các) bản sao của (x) xuất hiện liên tiếp. 

Bây giờ, giả sử chúng ta chọn một số viên đá (x_1,x_2,\ldots,x_t) và yêu cầu tất cả các sự kiện của chúng. Những viên đá được chọn phải có thứ tự tương đối giống nhau trong mỗi ngăn xếp. Nếu (x) xảy ra trước (y) trong mọi ngăn xếp, hãy viết (x\prec y). Những viên đá được chọn phải tạo thành một chuỗi theo thứ tự từng phần này. 

Khi một chuỗi được cố định, chuỗi sẽ tự nhiên phân chia thành các khoảng trống. Trước viên đá được chọn đầu tiên, giữa hai viên đá được chọn liên tiếp và sau viên đá được chọn cuối cùng, mỗi ngăn xếp đều đóng góp một số mảnh thông thường. Bên trong một khoảng trống, tất cả các đoạn đó có thể được xen kẽ tùy ý trong khi vẫn giữ nguyên thứ tự ngăn xếp của chúng. 

Giả sử một khoảng trống chứa (d_1,d_2,\ldots,d_s) các đoạn thông thường từ (các) ngăn xếp. Số lần xen kẽ của nó là 

\frac{(d_1+\cdots+d_s)!}{d_1!\cdots d_s!}. 
] 

Mỗi viên đá được chọn sẽ tạo thành một khối liên tiếp. (Các) chỉ mục ngăn xếp của nó có thể xuất hiện theo bất kỳ thứ tự nào, đưa ra hệ số (s!). 

Điều này tạo ra một đường đi DP trên những viên đá. Nếu (u\prec v), quá trình chuyển đổi từ (u) sang (v) chỉ cần hệ số đa thức của các đoạn nằm giữa các vị trí của chúng. DP không cần biết có bao nhiêu viên đá được chọn đã được chọn vì việc phân tách khoảng cách đã tính đến số lượng và vị trí chính xác của tất cả các khối được chọn. 

Trong một khoảng thời gian, DP thu được lấy (O(n+m)), trong đó (m) là số cặp đá có thứ tự tương đương. Các hoán vị ngẫu nhiên làm cho (m) trung bình nhỏ. Trong một khoảng chiều dài (s), một cặp đá cố định có xác suất (2^{-(s-1)}) có cùng thứ tự tương đối trong tất cả các ngăn xếp. Tổng hợp trên tất cả các khoảng thời gian, điều này mang lại công việc cặp so sánh được mong đợi (O(n^2k)). Công việc (O(n)) còn lại cho mỗi khoảng (O(k^2)) đóng góp (O(nk^2)). Đây là độ phức tạp ngẫu nhiên dự định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O\left(kn\frac{(kn)!}{(n!)^k}\right)) cho khoảng lớn nhất | (O(kn)) | Quá chậm | 
| Tối ưu | Dự kiến ​​(O(n^2k+nk^2)) | (O(n^2+nk)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chuyển đổi mọi ngăn xếp thành mảng vị trí. Với mỗi (các) ngăn xếp và viên đá (x), hãy lưu trữ (\operatorname{pos[s][x]), vị trí của (x) trong ngăn xếp đó. Điều này cho phép đạt được mọi thử nghiệm đặt hàng và mọi kích thước khoảng cách trong thời gian không đổi. 
2. Cố định điểm cuối bên trái (L) và mở rộng điểm cuối bên phải (R) từng ngăn một. Ngăn xếp đầu tiên (L) đưa ra thứ tự cố định của tất cả các viên đá. Mỗi cặp có thể so sánh được có thể được biểu diễn dưới dạng một cạnh (u\to v) trong đó (u) xuất hiện trước (v) trong ngăn xếp (L). 
3. Với mỗi viên đá (x), giữ nguyên hệ số đa thức của tiền tố trước (x), 

[ 
B_x= 
\frac{(\sum_s(\operatorname{pos[s][x]-1))!} 
{\prod_s(\operatorname{pos[s][x]-1)!}, 
] 

và hệ số hậu tố tương ứng 

[ 
A_x= 
\frac{(\sum_s(n-\operatorname{pos[s][x]))!} 
{\prod_s(n-\operatorname{pos[s][x])!}. 
] 

Chúng mô tả số cách để xen kẽ tất cả các đoạn trước và sau khối được chọn chứa (x). 

1. Đối với mọi cặp hiện có thể so sánh được (u\prec v), duy trì hệ số đa thức (G_{u,v}) của khoảng cách giữa chúng. Nếu ngăn xếp mới có các vị trí (p_u,p_v), thì khả năng so sánh tồn tại chính xác khi (p_u<p_v). Nếu (p_u\ge p_v), hãy xóa cạnh vĩnh viễn. 
2. Khi một ngăn xếp mới được thêm vào, hãy cập nhật mọi hệ số đa thức còn sót lại theo thời gian không đổi. Nếu đa thức hiện tại có tổng (S) và ngăn xếp mới đóng góp các phần tử (d), thì 

[ 
M' = M\frac{(S+d)!}{S!,d!}. 
] 

Công thức tương tự cập nhật (B_x), (A_x) và mọi phần còn lại (G_{u,v}). 

1. Chạy DP loại trừ bao gồm theo thứ tự các viên đá trong ngăn xếp đầu tiên. Đặt (dp[x]) là phần đóng góp có dấu của tất cả các chuỗi không trống có viên đá được chọn cuối cùng là (x), bao gồm tất cả các khoảng trống trước (x). Sau đó 

-s!\left( 
B_x+ 
\sum_{u\prec x}dp[u]G_{u,x} 
\đúng). 
] 

Số hạng đầu tiên chỉ tương ứng với việc chọn (x). Mọi số hạng khác đều gắn (x) vào một chuỗi kết thúc tại (u). (Các) hệ số tính đến thứ tự của (các) lựa chọn ngăn xếp bên trong khối được chọn mới. 

1. Sau DP, thêm hậu tố sau khối được chọn cuối cùng. Việc điều chỉnh loại trừ bao gồm cho khoảng thời gian là 

[ 
\sum_x dp[x]A_x. 
] 

Số lượng xen kẽ không hạn chế là 

[ 
T_s=\frac{(sn)!}{(n!)^s}. 
] 

Do đó 

[ 
f(L,R)=T_s+\sum_x dp[x]A_x. 
] 

1. Thêm giá trị này vào câu trả lời chung cho mọi (R>L). Sau đó mở rộng (R) và cập nhật cấu trúc cặp so sánh đang hoạt động. Khi điểm cuối bên trái thay đổi, hãy xây dựng lại cấu trúc bằng cách sử dụng ngăn xếp đầu tiên mới. 

Bất biến chính là mọi cạnh hoạt động (u\to v) biểu thị chính xác điều kiện (u) xảy ra trước (v) trong mọi ngăn xếp của khoảng hiện tại. Đối với mỗi chuỗi các cạnh hoạt động, DP đóng góp chính xác một thuật ngữ loại trừ bao gồm, với một hệ số đa thức cho mỗi khoảng trống và một (các) hệ số cho mỗi viên đá được chọn. Do đó, mọi tập hợp con của các sự kiện xấu xảy ra đồng thời đều được tính chính xác một lần với dấu đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1_000_000_007

def solve_instance(k, n, a):
    # pos[s][x] = zero-based position of stone x in stack s.
    pos = [[0] * n for _ in range(k)]
    for s in range(k):
        for p, x in enumerate(a[s]):
            pos[s][x - 1] = p

    max_fact = k * n
    fact = [1] * (max_fact + 1)
    for i in range(1, max_fact + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact = [1] * (max_fact + 1)
    invfact[max_fact] = pow(fact[max_fact], MOD - 2, MOD)
    for i in range(max_fact, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    invfact_n = invfact[n]

    ans = 0

    for left in range(k - 1):
        first = pos[left]

        order = sorted(range(n), key=first.__getitem__)

        # Every pair is created according to the order in the first stack.
        # Pair id -> (u, v).
        eu = []
        ev = []

        # Global doubly linked list of active pairs.
        gnxt = []
        gprev = []

        # Doubly linked lists of active incoming pairs for every v.
        bnxt = []
        bprev = []
        head_bucket = [-1] * n

        # Multinomial data for each pair.
        gap_sum = []
        gap_val = []

        # Active flag is useful when unlinking through two lists.
        active = []

        global_head = -1
        global_tail = -1

        # Build all pairs u -> v in first-stack order.
        for ii in range(n):
            u = order[ii]
            for jj in range(ii + 1, n):
                v = order[jj]

                eid = len(eu)
                eu.append(u)
                ev.append(v)

                d = first[v] - first[u] - 1
                gap_sum.append(d)
                gap_val.append(1)

                active.append(True)

                # Insert into v's incoming list.
                old = head_bucket[v]
                bprev.append(-1)
                bnxt.append(old)
                if old != -1:
                    bprev[old] = eid
                head_bucket[v] = eid

                # Insert into global list.
                gprev.append(global_tail)
                gnxt.append(-1)
                if global_tail != -1:
                    gnxt[global_tail] = eid
                else:
                    global_head = eid
                global_tail = eid

        # For one stack every multinomial coefficient is 1.
        before_sum = [first[x] for x in range(n)]
        before_val = [1] * n

        after_sum = [n - 1 - first[x] for x in range(n)]
        after_val = [1] * n

        invfact_n_pow = invfact_n

        # Add stacks right of 'left'.
        for right in range(left + 1, k):
            s = right + 1

            cur = pos[right]

            # Add the new stack to the prefix/suffix multinomials.
            for x in range(n):
                d = cur[x]

                old_sum = before_sum[x]
                new_sum = old_sum + d
                before_val[x] = (
                    before_val[x]
                    * fact[new_sum]
                    % MOD
                    * invfact[old_sum]
                    % MOD
                    * invfact[d]
                    % MOD
                )
                before_sum[x] = new_sum

                d2 = n - 1 - cur[x]

                old_sum = after_sum[x]
                new_sum = old_sum + d2
                after_val[x] = (
                    after_val[x]
                    * fact[new_sum]
                    % MOD
                    * invfact[old_sum]
                    % MOD
                    * invfact[d2]
                    % MOD
                )
                after_sum[x] = new_sum

            # Add the new stack to every currently comparable pair.
            eid = global_head
            while eid != -1:
                nxt_eid = gnxt[eid]

                u = eu[eid]
                v = ev[eid]

                pu = cur[u]
                pv = cur[v]

                if pu >= pv:
                    active[eid] = False

                    # Remove from global list.
                    p = gprev[eid]
                    q = gnxt[eid]
                    if p != -1:
                        gnxt[p] = q
                    else:
                        global_head = q
                    if q != -1:
                        gprev[q] = p
                    else:
                        global_tail = p

                    # Remove from v's bucket.
                    p = bprev[eid]
                    q = bnxt[eid]
                    if p != -1:
                        bnxt[p] = q
                    else:
                        head_bucket[v] = q
                    if q != -1:
                        bprev[q] = p
                else:
                    d = pv - pu - 1

                    old_sum = gap_sum[eid]
                    new_sum = old_sum + d

                    gap_val[eid] = (
                        gap_val[eid]
                        * fact[new_sum]
                        % MOD
                        * invfact[old_sum]
                        % MOD
                        * invfact[d]
                        % MOD
                    )
                    gap_sum[eid] = new_sum

                eid = nxt_eid

            # Number of unrestricted interleavings.
            invfact_n_pow = invfact_n_pow * invfact_n % MOD
            total = fact[s * n] * invfact_n_pow % MOD

            # Inclusion-exclusion DP.
            dp = [0] * n
            block_factor = fact[s]

            for x in order:
                val = before_val[x]

                eid = head_bucket[x]
                while eid != -1:
                    u = eu[eid]
                    val += dp[u] * gap_val[eid]
                    if val >= MOD:
                        val %= MOD
                    eid = bnxt[eid]

                dp[x] = (-block_factor * (val % MOD)) % MOD

            good = total
            for x in range(n):
                good += dp[x] * after_val[x]
                if good >= MOD:
                    good %= MOD

            ans += good
            if ans >= MOD:
                ans %= MOD

    return ans % MOD

def solve():
    k, n = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(k)]
    print(solve_instance(k, n, a))

if __name__ == "__main__":
    solve()
```Ma trận vị trí là bước tiền xử lý cấu trúc đầu tiên. Việc lưu trữ hoán vị nghịch đảo hữu ích hơn nhiều so với việc liên tục tìm kiếm một ngăn xếp để tìm một viên đá, bởi vì mọi so sánh giữa hai viên đá sẽ trở thành một tra cứu mảng duy nhất. 

Đối với điểm cuối bên trái cố định, ngăn xếp đầu tiên xác định thứ tự cấu trúc liên kết được DP sử dụng. Mỗi cặp bắt đầu có thể so sánh được vì chỉ có một ngăn xếp. Khi một ngăn xếp khác được thêm vào, một cặp sẽ tồn tại nếu thứ tự của nó phù hợp với ngăn xếp mới hoặc biến mất vĩnh viễn. Tính đơn điệu này là điều cho phép danh sách cặp hoạt động được duy trì tăng dần. 

Các đa thức tiền tố và hậu tố sử dụng cùng một danh tính cập nhật như các khoảng trống cặp. Giả sử một đa thức hiện có các phần có tổng là (S) và một ngăn xếp mới đóng góp (d). Giá trị mới của nó là giá trị cũ nhân với 

[ 
\frac{(S+d)!}{S!d!}. 
] 

Tất cả các giai thừa và giai thừa nghịch đảo đều được tính toán trước modulo (10^9+7), vì vậy mỗi lần cập nhật đều có thời gian không đổi. 

Cấu trúc cặp sử dụng hai danh sách liên kết cho mỗi cạnh. Danh sách chung cho phép việc triển khai truy cập mọi cặp so sánh hiện đang hoạt động khi một ngăn xếp được thêm vào. Danh sách theo mục tiêu cho phép DP chỉ truy cập những viên đá tiền nhiệm đang hoạt động của một loại đá cụ thể. Một cặp bị xóa khỏi cả hai danh sách đúng một lần sau khi thứ tự tương đối của nó không nhất quán. 

Đăng nhập`dp[x]`là âm vì việc chọn thêm một sự kiện xấu sẽ đóng góp hệ số (-1) vào việc loại trừ. Sự chuyển tiếp là 

[ 
-s!\left(B_x+\sum dp[u]G_{u,x}\right), 
] 

không 

[ 
-s!B_x\left(1+\sum dp[u]G_{u,x}\right). 
] 

Cái sau sẽ đếm tiền tố trước (x) hai lần khi (x) được thêm vào chuỗi hiện có. 

Số nguyên Python không bị tràn, nhưng mỗi phép nhân đều được giảm modulo (10^9+7). Giai thừa lớn nhất cần có là (k n\le90000), đủ nhỏ để tính toán trước trực tiếp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
3 3
1 2 3
3 2 1
1 3 2
```Hãy xem xét khoảng ([1,2]). Có (20) sự xen kẽ không hạn chế bởi vì 

[ 
\frac{6!}{3!3!}=20. 
] 

Vị trí của ba hòn đá là 

| Đá | Ngăn xếp 1 | Ngăn xếp 2 | Có thể so sánh được? | 
| --- | --- | --- | --- | 
| 1 | 1 | 3 | | 
| 2 | 2 | 2 | | 
| 3 | 3 | 1 | | 

Không có cặp đá nào có thứ tự tương đối giống nhau trong cả hai ngăn xếp, vì vậy mọi chuỗi bao gồm-loại trừ đều có độ dài bằng một. 

Đối với đá (1), tiền tố chứa các phần tử (0) và (2), cho hệ số đa thức là (1). Hậu tố chứa (2) và (0), cũng cho (1). Khối có (2!) lệnh nội bộ, do đó số sự kiện xấu của nó là (2). 

Đối với đá (2), tiền tố có (1,1) phần tử và đóng góp (2). Hậu tố cũng đóng góp (2). Bao gồm hệ số chặn (2!), số sự kiện xấu của nó là (8). 

Đối với đá (3), số đếm lại là (2). 

Trạng thái DP là 

| Đá | Hệ số tiền tố | Đóng góp tiền nhiệm | (dp) | Hệ số hậu tố | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | -2 | 1 | 
| 2 | 2 | 0 | -4 | 2 | 
| 3 | 1 | 0 | -2 | 1 | 

Như vậy 

[ 
f(1,2)=20-2-8-2=8. 
] 

Đối với khoảng ([1,3]), ngăn xếp thứ ba loại bỏ một số khả năng so sánh và giữ nguyên các giá trị khác theo ba hoán vị. Chạy cùng một DP mang lại 

[ 
f(1,3)=1446. 
] 

Ba giá trị khoảng là (8,1446,10), vì vậy câu trả lời cuối cùng là 

[ 
8+1446+10=1464. 
] 

Dấu vết chứng minh tại sao DP lại dựa trên chuỗi các sự kiện xấu tương thích chứ không phải dựa trên từng viên đá riêng lẻ. Một sự kiện xấu duy nhất chỉ cần các đa thức tiền tố và hậu tố của nó, trong khi một tập hợp các sự kiện đồng thời được biểu thị bằng các cạnh hoạt động trước đó. 

### Mẫu 2 

Đầu vào là```
4 2
1 2
2 1
1 2
2 1
```Đối với hai ngăn xếp liền kề, số lượng xen kẽ không hạn chế là 

[ 
\frac{4!}{2!2!}=6. 
] 

Đối với các ngăn xếp (1,2), hai viên đá xuất hiện theo thứ tự ngược nhau nên không cặp nào có thể so sánh được. Mỗi viên đá góp phần tạo nên hai công trình xấu, để lại 

[ 
f(1,2)=6-2-2=2. 
] 

Lý luận tương tự cho 

[ 
f(2,3)=2,\qquad f(3,4)=2. 
] 

Đối với ba ngăn xếp, số lượng không hạn chế là 

[ 
\frac{6!}{2!2!2!}=90. 
] 

Hai viên đá lại có thứ tự tương đối không tương thích nên không có giao điểm. Mỗi sự kiện xấu riêng lẻ góp phần (12), mang lại 

[ 
f(1,3)=90-12-12=66. 
] 

Tương tự, 

[ 
f(2,4)=66. 
] 

Đối với tất cả bốn ngăn xếp, có 

[ 
\frac{8!}{2!^4}=2520 
] 

xen kẽ không hạn chế. Hai viên đá bây giờ có kiểu thứ tự xen kẽ và DP đưa ra 

[ 
f(1,4)=2328. 
] 

Các giá trị khoảng là 

| Khoảng thời gian | (f(l,r)) | 
| --- | --- | 
| ([1,2]) | 2 | 
| ([1,3]) | 66 | 
| ([1,4]) | 2328 | 
| ([2,3]) | 2 | 
| ([2,4]) | 66 | 
| ([3,4]) | 2 | 

Tổng của chúng là (2466), khớp với đầu ra mẫu. 

Ví dụ này thực hiện tối ưu hóa chính. Khi nhiều ngăn xếp được thêm vào, các cặp không tương thích sẽ biến mất khỏi danh sách hoạt động, do đó DP không kiểm tra lặp lại tất cả (n^2) cặp có thể có. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Dự kiến ​​(O(n^2k+nk^2)) | (O(n)) DP hoạt động trên mỗi khoảng thời gian, cộng với công việc tỷ lệ thuận với các cặp so sánh còn sót lại | 
| Không gian | (O(n^2+nk)) | Danh sách cặp và hệ số khoảng cách của chúng chiếm ưu thế | 

Đối với một cặp đá, sau khi cố định thứ tự của chúng trong một ngăn xếp, mọi ngăn xếp ngẫu nhiên độc lập bổ sung sẽ giữ nguyên thứ tự đó với xác suất (1/2). Do đó, trong một khoảng thời gian (s), một cặp tồn tại với xác suất (2^{-(s-1)}). Tính tổng tất cả các khoảng có thể sẽ mang lại khả năng xử lý cặp (O(n^2k)) dự kiến, trong khi công việc DP mỗi khoảng (O(n)) cho kết quả (O(nk^2)). Đây là sự phức tạp ngẫu nhiên được mô tả bởi các nguồn biên tập độc lập cho vấn đề này. 

Với (n,k\le300), khối lượng công việc dự kiến ​​là thực tế. Việc triển khai cũng sử dụng mảng số nguyên nhỏ gọn thay vì bộ Python hoặc từ điển cho biểu đồ cặp hoạt động, vì các hệ số không đổi quan trọng ở tỷ lệ này. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây sử dụng tương tự`solve_instance`chức năng từ giải pháp. Trường hợp kích thước tối đa kiểm tra xem việc triển khai có nằm trong phạm vi kết quả mô-đun hay không, vì việc viết một giá trị dự kiến ​​được mã hóa cứng cho một phiên bản (300\times300) sẽ khiến bản thử nghiệm đó phụ thuộc vào một giải pháp được tính toán độc lập khác.```
# Put the submitted solution in solution.py.
from solution import solve_instance

MOD = 1_000_000_007

# Sample 1
a = [
    [1, 2, 3],
    [3, 2, 1],
    [1, 3, 2],
]
assert solve_instance(3, 3, a) == 1464, "sample 1"

# Sample 2
a = [
    [1, 2],
    [2, 1],
    [1, 2],
    [2, 1],
]
assert solve_instance(4, 2, a) == 2466, "sample 2"

# Minimum size.
a = [
    [1, 2],
    [2, 1],
]
assert solve_instance(2, 2, a) == 2, "minimum-size case"

# Same permutation twice.
# There is only one interval, and its answer is 4.
a = [
    [1, 2, 3],
    [1, 2, 3],
]
assert solve_instance(2, 3, a) == 4, "identical permutations"

# Three stacks, with the third reversing the order.
# f(1,2)=2, f(2,3)=2, f(1,3)=66.
a = [
    [1, 2],
    [1, 2],
    [2, 1],
]
assert solve_instance(3, 2, a) == 70, "comparable-pair boundary"

# Maximum-size structural stress test.
# Cyclic shifts are valid permutations and avoid the invalid
# "all values equal" matrix requested by some generic test templates.
k = 300
n = 300
a = [
    [((j + i) % n) + 1 for j in range(n)]
    for i in range(k)
]
out = solve_instance(k, n, a)
assert 0 <= out < MOD, "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 / 1 2 / 2 1`| 2 | Kích thước tối thiểu và lần chạy bị cấm nhỏ nhất có thể | 
|`2 3 / 1 2 3 / 1 2 3`| 4 | Một số sự kiện loại trừ bao gồm tương thích | 
|`3 2 / 1 2 / 1 2 / 2 1`| 70 | Các cặp có thể so sánh biến mất khi ngăn xếp mới đảo ngược thứ tự của chúng | 
| (300\times300) dịch chuyển theo chu kỳ | (0\le ans<10^9+7) | Kích thước tối đa, mức sử dụng bộ nhớ, giới hạn giai thừa và hiệu suất | 

## Vỏ cạnh 

Đối với trường hợp tối thiểu```
2 2
1 2
2 1
```có (6) sự xen kẽ không hạn chế. Đá (1) có thể tạo thành một cặp liên tiếp theo (2) cách và đá (2) có thể làm điều tương tự theo (2) cách. Các sự kiện của chúng không thể xảy ra đồng thời vì hai ngăn xếp không đồng nhất về thứ tự tương đối. Do đó DP cho (6-2-2=2). Ranh giới quan trọng là độ dài khoảng chính xác là (2), do đó, một chuỗi xấu bao gồm hai đoạn bằng nhau. 

Vì```
2 3
1 2 3
1 2 3
```cả ba viên đá đều có thể so sánh được. Số sự kiện xấu là (12,8,12). Giao điểm cặp có trọng số (8,8,8) và giao điểm ba có trọng số (8). Bao gồm-loại trừ mang lại 

[ 
20-(12+8+12)+(8+8+8)-8=4. 
] 

Cấu trúc cặp hoạt động giữ cả ba cạnh của cặp vì thứ tự của chúng giống nhau trong cả hai ngăn xếp. Bài kiểm tra này phát hiện sai lầm khi xử lý mọi sự kiện xấu một cách độc lập. 

Vì```
3 2
1 2
1 2
2 1
```hai ngăn xếp đầu tiên giống nhau, do đó cặp đá có thể so sánh được trong khoảng ([1,2]). Việc thêm ngăn xếp thứ ba sẽ loại bỏ cặp đó vì thứ tự của nó bị đảo ngược. Mỗi khoảng thời gian hai ngăn xếp đóng góp (2), trong khi khoảng thời gian ba ngăn xếp có (90) xen kẽ không hạn chế và hai đóng góp sự kiện xấu riêng lẻ là (12), cho ra (66). Tổng số là (70). Điều này xác minh rằng việc xóa cặp xảy ra chính xác khi ngăn xếp mới được thêm vào đảo ngược thứ tự. 

Đối với đầu vào hợp lệ có kích thước tối đa, chẳng hạn như cấu trúc dịch chuyển theo chu kỳ được sử dụng trong khối kiểm tra, mỗi hàng vẫn là một hoán vị thực sự. Mảng giai thừa chỉ cần các chỉ số đến (90000) và mọi cập nhật đa thức đều nằm trong số học mô-đun. Danh sách cặp hoạt động ngăn DP quét các cặp đã trở nên không tương thích. Bản chất ngẫu nhiên của đầu vào ban đầu là yếu tố làm cho số lượng cặp sống sót dự kiến ​​đủ nhỏ cho giới hạn (O(n^2k+nk^2)) dự định. 

Không nên chuyển trường hợp "tất cả các giá trị bằng nhau" cho chương trình này vì nó vi phạm điều kiện đầu vào là mọi ngăn xếp đều là một hoán vị. Trường hợp căng thẳng có ý nghĩa gần nhất là lặp lại cùng một hoán vị nhiều lần. Trường hợp đó cũng hữu ích vì nó cố tình tạo ra số lượng cặp so sánh lớn nhất có thể và kiểm tra xem bản thân công thức bao gồm-loại trừ vẫn đúng ngay cả trên đầu vào có cấu trúc cao, không điển hình.
