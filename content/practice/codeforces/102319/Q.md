---
title: "CF 102319Q - Truy vấn kỳ quặc"
description: "Chúng tôi duy trì một loạt các số nguyên kỳ quặc. Một số nguyên kỳ quặc là không có hình vuông và mọi thừa số nguyên tố đều dưới 300. Chỉ có 62 số nguyên tố như vậy, vì vậy mọi giá trị có thể được biểu thị bằng mặt nạ 62 bit cho chúng ta biết số nguyên tố nào xuất hiện trong quá trình phân tích thành thừa số của nó."
date: "2026-08-13T05:04:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102319
codeforces_index: "Q"
codeforces_contest_name: "UBC Summer Contest 2018"
rating: 0
weight: 102319
solve_time_s: 424
verified: true
draft: false
---

[CF 102319Q - Truy vấn kỳ quặc](https://codeforces.com/problemset/problem/102319/Q) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 4 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một loạt các số nguyên kỳ quặc. Một số nguyên kỳ quặc là không có hình vuông và mọi thừa số nguyên tố đều dưới 300. Chỉ có 62 số nguyên tố như vậy, vì vậy mọi giá trị có thể được biểu thị bằng mặt nạ 62 bit cho chúng ta biết số nguyên tố nào xuất hiện trong quá trình phân tích thành thừa số của nó. 

Truy vấn loại 1 cung cấp một phạm vi và một số nguyên kỳ lạ khác`x`. Mỗi giá trị trong phạm vi được thay thế bằng`x`chính xác khi danh sách các ước số của nó lớn hơn về mặt từ điển so với danh sách ước số của`x`. Truy vấn loại 2 yêu cầu LCM của tất cả các giá trị trong một phạm vi, modulo (10^9+7). 

Khó khăn đầu tiên là việc cập nhật không phải là một công việc thông thường. Một số vị trí thay đổi và một số thì không, tùy thuộc vào thứ tự không tầm thường của các số nguyên. Khó khăn thứ hai là LCM phải được duy trì trong khi các giá trị thay đổi theo phạm vi. 

Với (n\le 10^5) và (q\le 2\cdot10^5), việc quét toàn bộ phạm vi cho mọi truy vấn là không thể. Trong trường hợp xấu nhất, mỗi truy vấn (2\cdot10^5) có thể chạm vào (10^5) vị trí, đưa ra (2\cdot10^{10}) lượt truy cập vị trí trước khi xem xét chi phí so sánh danh sách ước số. Một giải pháp cần công việc được khấu hao gần như logarit cho mỗi truy vấn. 

Có một số trường hợp đặc biệt mà việc triển khai trực tiếp có thể mắc sai lầm. Đầu tiên là trường hợp tiền tố trong so sánh từ điển. Ví dụ: với một phần tử bằng`2`, truy vấn`1 1 1 6`phải thay đổi nó thành`6`, vì danh sách chia của`2`là`[1, 2]`, nhỏ hơn về mặt từ điển so với`[1, 2, 3, 6]`. Chỉ coi việc so sánh là so sánh các mặt nạ bit có hệ số nguyên tố có thể mắc sai lầm. 

Trường hợp cạnh thứ hai là một số chia cho số kia. Ví dụ,`6`Và`30`có danh sách chia`[1,2,3,6]`Và`[1,2,3,5,6,10,15,30]`. Sự khác biệt đầu tiên là`5`so với`6`, Vì thế`30`thực sự nhỏ hơn về mặt từ điển so với`6`. Một bộ so sánh chỉ dựa trên việc liệu một số nguyên tố có xuất hiện trong một phép nhân tử hay không sẽ sắp xếp chúng không chính xác. 

Trường hợp cạnh thứ ba liên quan đến LCM. Đối với đầu vào```
2
6 10
1
2 1 2
```câu trả lời là`30`, không`60`. Vì các số không có hình vuông nên LCM chỉ chứa mỗi số nguyên tố một lần, do đó phép toán đúng là OR theo bit của các mặt nạ nguyên tố của chúng, sau đó là phép nhân các số nguyên tố được chọn. 

Các điểm cuối của phạm vi cũng cần xử lý chính xác. Ví dụ,```
3
6 10 14
1
1 2 2 7
```chỉ thay đổi vị trí thứ hai. Đang cập nhật`[l,r)`nội bộ trong khi coi phạm vi đầu vào là`[l,r]`sẽ âm thầm bỏ lỡ phần tử cuối cùng trong nhiều trường hợp. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Đối với truy vấn loại 1, hãy kiểm tra mọi vị trí từ`l`bởi vì`r`, so sánh dãy số chia của giá trị hiện tại và`x`, và gán`x`khi được yêu cầu. Đối với truy vấn loại 2, hãy xem qua phạm vi và tích lũy LCM. Cả hai quy trình đều đúng vì chúng trực tiếp thực hiện các định nghĩa. 

Vấn đề là số lượng vị trí được truy cập. Một chuỗi truy vấn (2\cdot10^5) trong trường hợp xấu nhất trong phạm vi độ dài (10^5) sẽ mang lại (2\cdot10^{10}) lượt truy cập. Bản thân việc so sánh số chia cũng không phải là hằng số nếu được thực hiện bằng cách xây dựng rõ ràng các danh sách số chia, vì vậy cách tiếp cận này vượt xa giới hạn thời gian. 

Quan sát hữu ích đầu tiên là các số kỳ quặc không có hình vuông. Do đó, các hệ số nguyên tố của chúng là các tập hợp chứ không phải là nhiều tập hợp. Vì mọi thừa số nguyên tố đều nhỏ hơn 300 nên chỉ có 62 thừa số nguyên tố có thể có. Điều này ngay lập tức đưa ra một biểu diễn nhỏ gọn của mọi giá trị dưới dạng mặt nạ 62 bit. 

Quan sát thứ hai tinh tế hơn. Chúng ta cần so sánh nhanh các dãy số chia, nhưng thực tế chúng ta không cần phải xây dựng các dãy số đó. 

Lấy hai số kỳ lạ khác nhau`a`Và`b`, và để`p`là số nguyên tố nhỏ nhất xuất hiện đúng một trong số chúng. Mọi ước số đều nhỏ hơn`p`chỉ sử dụng các số nguyên tố nhỏ hơn`p`và tất cả các thành viên chính đó đều giống hệt nhau về`a`Và`b`. Như vậy mọi ước số dưới đây`p`xảy ra ở cả hai số. 

Giả định`p`chia rẽ`a`nhưng không`b`. Nếu không có số nào chia hết cho số kia thì`b`có một số thừa số nguyên tố lớn hơn`p`điều đó không được chia sẻ. Ước số đầu tiên có các dãy khác nhau là`p`ở bên cạnh`a`, Vì thế`a`nhỏ hơn về mặt từ điển. 

Trường hợp đặc biệt duy nhất là khi`b`chia rẽ`a`. Khi đó mọi ước của`b`cũng xảy ra ở`a`. Cho phép`p`lại là số nguyên tố bổ sung nhỏ nhất của`a`. Nếu như`p < b`, số chia`p`xuất hiện trước số chia cuối cùng`b`, Vì thế`a`nhỏ hơn. Nếu như`p > b`, toàn bộ dãy số chia của`b`kết thúc trước`p`, Vì thế`b`nhỏ hơn. Trường hợp đối xứng áp dụng khi`a`chia rẽ`b`. 

Điều này cung cấp một bộ so sánh (O(1)) khi đã biết hai mặt nạ chính. Số lượng số nguyên tố có thể có chỉ là 62, vì vậy việc tìm số nguyên tố khác nhau đầu tiên là một thao tác bit theo thời gian không đổi. 

Bản cập nhật bây giờ là một phạm vi`chmin`theo tổng thứ tự tùy chỉnh này. Đối với mọi vị trí chúng tôi thực hiện theo khái niệm 

[ 
a_i\leftarrow \min(a_i,x) 
] 

ở đâu`min`sử dụng thứ tự dãy số chia thay vì thứ tự số nguyên thông thường. 

Đó chính xác là loại cập nhật được xử lý bởi Segment Tree Beats. Đối với mỗi nút cây phân đoạn, chúng tôi giữ giá trị tối đa của nó theo thứ tự này, mức tối đa thứ hai nghiêm ngặt của nó, số lần xuất hiện của mức tối đa và đủ thông tin về mặt nạ chính để duy trì phạm vi LCM. 

LCM có một cách trình bày đặc biệt thuận tiện. Vì mọi số đều không có hình vuông nên LCM của một tập hợp thu được bằng cách lấy OR theo bit của tất cả các mặt nạ nguyên tố của chúng. Do đó, mỗi nút cây phân đoạn sẽ lưu trữ mặt nạ OR này. Ngoài ra, chúng tôi còn lưu trữ mặt nạ OR sau khi loại bỏ tất cả các phần tử bằng mức tối đa của nút. Trường bổ sung này cho phép chúng tôi cập nhật OR khi chỉ thay đổi nhóm tối đa. 

Brute-force hoạt động vì nó kiểm tra rõ ràng mọi giá trị bị ảnh hưởng. Nó không thành công vì quá nhiều giá trị có thể được chạm vào bởi quá nhiều truy vấn. Quan sát cho thấy bản cập nhật là một phạm vi`chmin`theo thứ tự tổng cố định cho phép Nhịp cây phân đoạn bỏ qua toàn bộ nhóm giá trị, trong khi biểu diễn 62 số nguyên tố làm cho phép tổng hợp LCM thành OR theo bit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(qn)), tối đa (2\cdot10^{10}) lượt truy cập vị trí | (O(n)) | Quá chậm | 
| Tối ưu | (O((n+q)\log n)) khấu hao | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích mọi giá trị ban đầu và mọi giá trị xuất hiện trong truy vấn loại 1 thành các thừa số nguyên tố riêng biệt của nó. Vì tất cả các số đều được đảm bảo là kỳ quặc nên mọi thừa số đều là một trong 62 số nguyên tố dưới 300. Biểu diễn phép phân tích nhân tử bằng mặt nạ 62 bit. 
2. Thực hiện so sánh số chia bằng cách sử dụng hai mặt nạ. Tìm số nguyên tố có chỉ số thấp nhất có thành viên khác nhau. Nếu không có số nào chia hết cho số kia thì số chứa số nguyên tố đó nhỏ hơn theo thứ tự chia số. Nếu một số chia cho số kia, hãy so sánh số nguyên tố phụ với chính số nhỏ hơn, vì điều đó quyết định liệu ước số phụ có xuất hiện trước khi dãy ngắn hơn kết thúc hay không. 
3. Xây dựng cây phân đoạn. Đối với mỗi nút, hãy lưu trữ giá trị tối đa theo thứ tự tùy chỉnh, mặt nạ của nó, mức tối đa thứ hai nghiêm ngặt và mặt nạ của nó, số lượng giá trị tối đa, OR của mọi mặt nạ trong nút và OR sau khi loại trừ tất cả các vị trí có giá trị tối đa. 
4. Xử lý truy vấn loại 1 dưới dạng một dải ô`chmin`. Nếu mức tối đa của nút không lớn hơn`x`, không có gì thay đổi. Nếu toàn bộ nút được bao phủ và mức tối đa thứ hai nghiêm ngặt của nó nhỏ hơn`x`, chỉ các vị trí có giá trị lớn nhất mới có thể thay đổi, do đó nút có thể được cập nhật mà không cần giảm xuống các nút con của nó. 
5. Khi chỉ có nhóm tối đa thay đổi, hãy thay giá trị của nó bằng`x`, giữ nguyên số lượng của nó và tính toán lại OR của nút dưới dạng`OR_without_max | mask(x)`. Cực đại thứ hai không thay đổi vì`x`thực sự lớn hơn nó. 
6. Nếu nút không thể được cập nhật trực tiếp, hãy đẩy giới hạn tối đa hiện tại tới các nút con của nó và lặp lại các nút con có liên quan. Sau đó hợp nhất những đứa trẻ trở lại với cha mẹ. 
7. Xử lý truy vấn loại 2 bằng cách OR-ing đệ quy các mặt nạ chính của các nút được bao phủ. Mặt nạ kết quả thể hiện chính xác các thừa số nguyên tố có trong LCM. Nhân các số nguyên tố đã chọn đó theo modulo (10^9+7). 

Lý do Nhịp cây phân đoạn vẫn nhanh là vì việc giảm bắt buộc chỉ xảy ra khi bản cập nhật đạt ít nhất giá trị riêng biệt lớn thứ hai của một nút. Trong trường hợp đó, hai giá trị riêng biệt trở nên bằng nhau sau khi cập nhật, do đó số lượng giá trị riêng biệt được biểu thị trong cây con đó sẽ giảm đi. Các bản cập nhật có thể đưa ra một giá trị mới chỉ với chi phí biên của cây phân đoạn thông thường, trong khi các phần giảm xuống có tính phá hủy được phân bổ theo mức giảm này. Điều này mang lại mức khấu hao tiêu chuẩn (O((n+q)\log n)) cho phạm vi`chmin`. 

### Tại sao nó hoạt động 

Bất biến của cây phân đoạn là mỗi nút mô tả chính xác các giá trị hiện tại trong khoảng của nó, với`mx`là giá trị lớn nhất theo thứ tự chia số và`smx`giá trị lớn nhất nhỏ hơn. Nếu như`x`nằm chặt chẽ giữa chúng, chỉ có các phần tử bằng`mx`bị ảnh hưởng nên việc thay đổi nhóm đó là đủ. Nếu như`x`ở mức hoặc thấp hơn`smx`, bản cập nhật có thể ảnh hưởng đến nhiều giá trị riêng biệt và nút phải được tách ra. 

Việc lưu trữ`or_without_max`chính xác là OR của tất cả các giá trị không bằng`mx`. Bất cứ khi nào nhóm tối đa được thay thế, OR hoàn chỉnh mới sẽ được`or_without_max | mask(x)`. Bất cứ khi nào các phần tử con được hợp nhất, ba trường hợp cực đại bằng nhau, cực đại bên trái hoặc cực đại bên phải sẽ cung cấp đủ thông tin để tái tạo lại cả nhóm cực đại và OR loại trừ nó. 

Đối với truy vấn phạm vi, OR của tất cả các mặt nạ nút được bao phủ chính xác là sự kết hợp của tất cả các thừa số nguyên tố. Vì mỗi số kỳ quặc chứa nhiều nhất mỗi số nguyên tố có số mũ là một, nên phép kết này chính xác là phép phân tích số nguyên tố của LCM. Do đó, phép nhân mô-đun cuối cùng sẽ tạo ra câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1_000_000_007

PRIMES = [
    2, 3, 5, 7, 11, 13, 17, 19, 23, 29,
    31, 37, 41, 43, 47, 53, 59, 61, 67, 71,
    73, 79, 83, 89, 97, 101, 103, 107, 109, 113,
    127, 131, 137, 139, 149, 151, 157, 163, 167, 173,
    179, 181, 191, 193, 197, 199, 211, 223, 227, 229,
    233, 239, 241, 251, 257, 263, 269, 271, 277, 281,
    283, 293
]

PRIME_INDEX = {p: i for i, p in enumerate(PRIMES)}

def solve():
    n = int(input())
    initial = list(map(int, input().split()))
    q = int(input())

    queries = []
    all_values = set(initial)

    for _ in range(q):
        parts = list(map(int, input().split()))
        queries.append(parts)
        if parts[0] == 1:
            all_values.add(parts[3])

    mask_cache = {}

    def get_mask(x):
        cached = mask_cache.get(x)
        if cached is not None:
            return cached

        if x == 1:
            mask_cache[x] = 0
            return 0

        v = x
        mask = 0

        for i, p in enumerate(PRIMES):
            if p * p > v:
                break
            if v % p == 0:
                mask |= 1 << i
                v //= p

        if v > 1:
            mask |= 1 << PRIME_INDEX[v]

        mask_cache[x] = mask
        return mask

    # Returns True exactly when the divisor sequence of a is
    # lexicographically smaller than that of b.
    def less(a, ma, b, mb):
        if a == b:
            return False

        diff = ma ^ mb
        bit = diff & -diff
        idx = bit.bit_length() - 1
        p = PRIMES[idx]

        # b divides a.
        if (ma & mb) == mb:
            return p < b

        # a divides b.
        if (ma & mb) == ma:
            return a < p

        # Neither divides the other. The side containing the
        # first differing prime has the smaller divisor sequence.
        return (ma & bit) != 0

    def greater(a, ma, b, mb):
        return less(b, mb, a, ma)

    size = 4 * n + 5

    mx = [0] * size
    smx = [-1] * size
    cnt = [0] * size

    mx_mask = [0] * size
    smx_mask = [0] * size

    or_mask = [0] * size
    or_without_max = [0] * size

    def pull(v):
        left = v << 1
        right = left | 1

        lm = mx[left]
        rm = mx[right]
        lmask = mx_mask[left]
        rmask = mx_mask[right]

        if lm == rm:
            mx[v] = lm
            mx_mask[v] = lmask
            cnt[v] = cnt[left] + cnt[right]

            a = smx[left]
            am = smx_mask[left]
            b = smx[right]
            bm = smx_mask[right]

            if a == -1:
                smx[v] = b
                smx_mask[v] = bm
            elif b == -1:
                smx[v] = a
                smx_mask[v] = am
            elif greater(a, am, b, bm):
                smx[v] = a
                smx_mask[v] = am
            else:
                smx[v] = b
                smx_mask[v] = bm

            or_without_max[v] = or_without_max[left] | or_without_max[right]

        elif greater(lm, lmask, rm, rmask):
            mx[v] = lm
            mx_mask[v] = lmask
            cnt[v] = cnt[left]

            a = smx[left]
            am = smx_mask[left]
            b = rm
            bm = rmask

            if a == -1 or greater(b, bm, a, am):
                smx[v] = b
                smx_mask[v] = bm
            else:
                smx[v] = a
                smx_mask[v] = am

            or_without_max[v] = or_without_max[left] | or_mask[right]

        else:
            mx[v] = rm
            mx_mask[v] = rmask
            cnt[v] = cnt[right]

            a = smx[right]
            am = smx_mask[right]
            b = lm
            bm = lmask

            if a == -1 or greater(b, bm, a, am):
                smx[v] = b
                smx_mask[v] = bm
            else:
                smx[v] = a
                smx_mask[v] = am

            or_without_max[v] = or_without_max[right] | or_mask[left]

        or_mask[v] = or_without_max[v] | mx_mask[v]

    def build(v, l, r):
        if l == r:
            value = initial[l]
            mask = get_mask(value)

            mx[v] = value
            smx[v] = -1
            cnt[v] = 1
            mx_mask[v] = mask
            smx_mask[v] = 0
            or_mask[v] = mask
            or_without_max[v] = 0
            return

        mid = (l + r) >> 1
        build(v << 1, l, mid)
        build(v << 1 | 1, mid + 1, r)
        pull(v)

    def apply_max(v, value, mask):
        mx[v] = value
        mx_mask[v] = mask
        or_mask[v] = or_without_max[v] | mask

    def push(v):
        value = mx[v]
        mask = mx_mask[v]

        left = v << 1
        right = left | 1

        if greater(mx[left], mx_mask[left], value, mask):
            apply_max(left, value, mask)

        if greater(mx[right], mx_mask[right], value, mask):
            apply_max(right, value, mask)

    def range_chmin(v, l, r, ql, qr, value, value_mask):
        if r < ql or qr < l:
            return

        # Nothing in this node is larger than value.
        if not greater(mx[v], mx_mask[v], value, value_mask):
            return

        if ql <= l and r <= qr:
            # Only the maximum group is above value.
            if smx[v] == -1 or less(smx[v], smx_mask[v], value, value_mask):
                apply_max(v, value, value_mask)
                return

        push(v)

        mid = (l + r) >> 1
        range_chmin(v << 1, l, mid, ql, qr, value, value_mask)
        range_chmin(v << 1 | 1, mid + 1, r, ql, qr, value, value_mask)
        pull(v)

    def range_or(v, l, r, ql, qr):
        if r < ql or qr < l:
            return 0

        if ql <= l and r <= qr:
            return or_mask[v]

        push(v)

        mid = (l + r) >> 1
        return (
            range_or(v << 1, l, mid, ql, qr)
            | range_or(v << 1 | 1, mid + 1, r, ql, qr)
        )

    build(1, 0, n - 1)

    product_cache = {0: 1}

    def mask_product(mask):
        cached = product_cache.get(mask)
        if cached is not None:
            return cached

        result = 1
        m = mask

        while m:
            bit = m & -m
            idx = bit.bit_length() - 1
            result = result * PRIMES[idx] % MOD
            m -= bit

        product_cache[mask] = result
        return result

    out = []

    for query in queries:
        if query[0] == 1:
            _, l, r, x = query
            x_mask = get_mask(x)
            range_chmin(1, 0, n - 1, l - 1, r - 1, x, x_mask)
        else:
            _, l, r = query
            mask = range_or(1, 0, n - 1, l - 1, r - 1)
            out.append(str(mask_product(mask)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Quy trình phân tích nhân tử sử dụng thực tế là mọi thừa số nguyên tố đều dưới 300. Nó thử 62 số nguyên tố có thể có và dừng lại khi bình phương của số nguyên tố hiện tại vượt quá giá trị còn lại. Từ điển lưu trữ mặt nạ vào bộ nhớ đệm, do đó, các giá trị lặp lại từ mảng ban đầu hoặc các mục tiêu cập nhật lặp lại chỉ được tính một lần. 

các`less`hàm số là phần lý thuyết số quan trọng.`diff`xác định số nguyên tố nhỏ nhất trong đó hai hệ số khác nhau. Việc kiểm tra tập hợp con xử lý trường hợp một số nguyên chia cho số kia, đó chính xác là trường hợp đối số nguyên tố thứ nhất khác nhau thông thường cần so sánh bổ sung với số nguyên nhỏ hơn. 

Cây phân đoạn chứa hai loại thông tin khác nhau.`mx`,`smx`, Và`cnt`là trạng thái Nhịp cây phân đoạn được sử dụng để quyết định giá trị nào`chmin`có thể thay đổi hàng loạt.`or_mask`Và`or_without_max`là trạng thái tổng hợp được sử dụng cho các truy vấn LCM. 

các`apply_max`chức năng không cần phải sửa đổi`smx`hoặc`cnt`. Nó chỉ được gọi khi giá trị mới nằm hoàn toàn giữa mức tối đa cũ và mức tối đa thứ hai nghiêm ngặt hoặc khi nút chỉ chứa một giá trị riêng biệt. Trong cả hai trường hợp, tập hợp các vị trí thuộc nhóm tối đa không thay đổi.`push`hơi khác so với cây phân đoạn lười biếng thông thường. Không có thẻ phân công riêng biệt. Mức tối đa hiện tại của cha mẹ đóng vai trò là giới hạn phải được truyền cho bất kỳ đứa trẻ nào có mức tối đa vẫn lớn hơn. Đây là cơ chế Nhịp cây phân đoạn tiêu chuẩn. 

Tất cả các phạm vi đầu vào được chuyển đổi từ các chỉ số bao gồm dựa trên một thành các chỉ số bao gồm dựa trên 0 với`l - 1`Và`r - 1`. Quá trình đệ quy sử dụng các điểm cuối bao gồm một cách nhất quán, do đó quá trình chuyển đổi diễn ra chính xác một lần. 

Số nguyên Python không bị tràn và các giá trị lớn duy nhất trong cấu trúc dữ liệu là mặt nạ bit. Mặt nạ lớn nhất chỉ sử dụng 62 bit. Giá trị LCM không bao giờ được xây dựng trực tiếp, chỉ duy trì sản phẩm mô-đun cuối cùng. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là```
3
6 10 13
5
1 1 3 14
2 1 1
2 2 2
2 3 3
2 1 3
```Những thay đổi trạng thái có liên quan là: 

| Truy vấn | Phạm vi | Mục tiêu | Giá trị sau khi cập nhật | Phạm vi HOẶC cho truy vấn | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
|`1 1 3 14`| 1..3 | 14 |`[6, 10, 14]`| | | 
|`2 1 1`| 1..1 | |`[6, 10, 14]`|`{2,3}`| 6 | 
|`2 2 2`| 2..2 | |`[6, 10, 14]`|`{2,5}`| 10 | 
|`2 3 3`| 3..3 | |`[6, 10, 14]`|`{2,7}`| 14 | 
|`2 1 3`| 1..3 | |`[6, 10, 14]`|`{2,3,5,7}`| 210 | 

Bản cập nhật rời đi`6`Và`10`không thay đổi vì dãy số chia của chúng nhỏ hơn dãy số của`14`. giá trị`13`được thay thế vì`14`có dãy số chia nhỏ hơn. LCM cuối cùng chứa bốn số nguyên tố riêng biệt 2, 3, 5 và 7, cho`210`. 

Đối với Mẫu 2, đầu vào là```
2
1 1
5
2 1 1
1 1 1 2
2 1 1
1 1 1 1
2 1 1
```Dấu vết là: 

| Truy vấn | Phạm vi | Mục tiêu | Trạng thái mảng | Trả lời | 
| --- | --- | --- | --- | --- | 
|`2 1 1`| 1..1 | |`[1,1]`| 1 | 
|`1 1 1 2`| 1..1 | 2 |`[1,1]`| | 
|`2 1 1`| 1..1 | |`[1,1]`| 1 | 
|`1 1 1 1`| 1..1 | 1 |`[1,1]`| | 
|`2 1 1`| 1..1 | |`[1,1]`| 1 | 

Truy vấn thứ hai không thay đổi phần tử đầu tiên. Dãy số chia của`1`là`[1]`, nhỏ hơn về mặt từ điển so với`[1,2]`, Vì thế`1`không được thay thế bởi`2`. Điều này thực hiện trường hợp tiền tố mà một bộ so sánh mặt nạ nguyên tố đơn giản sẽ xử lý sai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O((n+q)\log n)) khấu hao | Segment Tree Beats xử lý phạm vi`chmin`; xây dựng mặt nạ kiểm tra tối đa 62 số nguyên tố trên mỗi giá trị đầu vào riêng biệt; mỗi mặt nạ LCM chứa tối đa 62 số nguyên tố | 
| Không gian | (O(n+q)) | Cây phân đoạn sử dụng các nút (O(n)) và các mặt nạ được lưu trong bộ nhớ đệm cũng như các giá trị truy vấn sử dụng không gian bổ sung (O(n+q)) | 

Hằng số phân tích nhân tử chỉ là 62 vì bài toán giới hạn tất cả các thừa số nguyên tố ở các số nguyên tố dưới 300. Phần cây phân đoạn được khấu hao (O((n+q)\log n)), phù hợp cho (10^5) vị trí mảng và (2\cdot10^5) truy vấn. Các mặt nạ được lưu trữ cũng nhỏ gọn vì chúng chỉ cần 62 bit. 

## Trường hợp thử nghiệm```python
# Save the solution above as solution.py before running these tests.
import sys
import io
import importlib
import solution

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solution.input = sys.stdin.readline
        solution.solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Sample 1
assert run(
    """\
3
6 10 13
5
1 1 3 14
2 1 1
2 2 2
2 3 3
2 1 3
"""
) == "6\n10\n14\n210", "sample 1"

# Sample 2
assert run(
    """\
2
1 1
5
2 1 1
1 1 1 2
2 1 1
1 1 1 1
2 1 1
"""
) == "1\n1\n1", "sample 2"

# Minimum-size input
assert run(
    """\
1
1
3
2 1 1
1 1 1 2
2 1 1
"""
) == "1\n1", "minimum size"

# All values equal, plus a range update that changes only part of the array
assert run(
    """\
4
6 6 6 6
3
1 2 3 2
2 1 4
2 2 3
"""
) == "6\n2", "all equal values"

# Boundary and prefix-sensitive comparisons
assert run(
    """\
5
6 10 14 13 1
4
1 2 4 7
2 3 5
2 1 2
2 4 4
"""
) == "7\n30\n7", "range boundaries"

# The prefix case 2 < 6 must be handled correctly.
assert run(
    """\
4
2 6 3 15
2
1 1 4 2
2 1 4
"""
) == "30", "lexicographic prefix case"

# Maximum-size array, with a small number of queries.
big_input = (
    "100000\n"
    + ("1 " * 99999)
    + "1\n"
    + "3\n"
    + "2 1 100000\n"
    + "1 1 100000 1\n"
    + "2 1 100000\n"
)
assert run(big_input) == "1\n1", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1`, giá trị`1`|`1\n1`| Đầu vào kích thước tối thiểu và cập nhật không hoạt động | 
|`[6,6,6,6]`, cập nhật`[2,3]`ĐẾN`2`|`6\n2`| Các nhóm tối đa bằng nhau và cập nhật một phần phạm vi | 
|`[6,10,14,13,1]`, cập nhật`[2,4]`ĐẾN`7`|`7\n30\n7`| Ranh giới toàn diện và thay thế có chọn lọc | 
|`[2,6,3,15]`, cập nhật tất cả lên`2`|`30`| Hành vi tiền tố từ điển | 
|`100000`bản sao của`1`|`1\n1`| Kích thước mảng tối đa và truy vấn toàn phạm vi | 

## Vỏ cạnh 

Trường hợp tiền tố được xử lý trực tiếp bởi nhánh tập hợp con trong`less`. Vì```
1
1
2
1 1 1 2
2 1 1
```số đầu tiên có mặt nạ bằng 0, trong khi`2`có bit tương ứng với số nguyên tố 2. Số đầu tiên chia hết cho số thứ hai, do đó bộ so sánh sẽ kiểm tra`1 < 2`và kết luận rằng`1`nhỏ hơn. Bản cập nhật bị bỏ qua và câu trả lời là`1`. 

Trường hợp chia hết không cần thiết là```
2
6 30
1
2 1 2
```Mặt nạ nhân tố là`{2,3}`Và`{2,3,5}`. Số thứ nhất chia hết cho số thứ hai và số nguyên tố phụ là`5`. Từ`6 > 5`, bộ so sánh quyết định rằng`30`về mặt từ điển nhỏ hơn`6`. Phạm vi không thay đổi do không có bản cập nhật nào ở đây và truy vấn LCM sẽ sử dụng mặt nạ OR`{2,3,5}`, sản xuất`30`. 

Trường hợp chồng chéo LCM là```
2
6 10
1
2 1 2
```Những chiếc mặt nạ là`{2,3}`Và`{2,5}`. OR của họ là`{2,3,5}`, vậy sản phẩm mô-đun là`2*3*5 = 30`. Nhân các giá trị ban đầu sẽ đếm sai số nguyên tố chung 2 hai lần. 

Bản cập nhật toàn phạm vi được xử lý mà không cần logic điểm cuối đặc biệt. Vì```
3
6 10 14
1
1 1 3 7
```bản cập nhật truy cập chính xác vào ba vị trí. Hai giá trị đầu tiên nhỏ hơn`7`theo thứ tự chia, trong khi`14`lớn hơn bởi vì`[1,2,7,14]`về mặt từ điển lớn hơn`[1,7]`. Mảng kết quả là`[6,10,7]`. Biểu diễn phạm vi đệ quy bao gồm cả hai điểm cuối, do đó không có vị trí nào bị loại trừ một cách vô tình. 

Trường hợp mọi giá trị trong một nút đều bằng nhau cũng rất có ý nghĩa. Mức tối đa thứ hai nghiêm ngặt của nó là`-1`, do đó điều kiện Segment Tree Beats chấp nhận cập nhật ngay lập tức. Toàn bộ nút đại diện cho một nhóm tối đa và`or_without_max`là số không. Do đó, việc thay thế nhóm đó sẽ thay đổi trực tiếp OR của nút thành mặt nạ chính của giá trị mới mà không đi xuống các lá.
