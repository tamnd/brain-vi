---
title: "CF 102201G - Bộ Tốt"
description: "Chúng tôi làm việc bên trong vũ trụ Boolean của tất cả các số nguyên (k)-bit, do đó có (2^k) phần tử có thể có. Một tập hợp tốt là một họ khác trống của các số nguyên này được đóng dưới cả hai bit AND và bitwise OR."
date: "2026-08-18T01:44:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102201
codeforces_index: "G"
codeforces_contest_name: "Moscow Pre-Finals Workshop 2019. KAIST Contest"
rating: 0
weight: 102201
solve_time_s: 329
verified: true
draft: false
---

[CF 102201G - Bộ tốt](https://codeforces.com/problemset/problem/102201/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi làm việc bên trong vũ trụ Boolean của tất cả các số nguyên (k)-bit, do đó có (2^k) phần tử có thể có. Một tập hợp tốt là một họ khác trống của các số nguyên này được đóng dưới cả hai bit AND và bitwise OR. Đầu vào đưa ra một số số nguyên riêng biệt mà tất cả đều phải thuộc về họ và nhiệm vụ là đếm mọi họ tốt thỏa mãn yêu cầu đó. 

Khó khăn chính là vũ trụ chứa nhiều nhất (128) số nguyên, nhưng một họ số nguyên có thể có (2^{128}) ứng cử viên khác nhau. Việc liệt kê trực tiếp các tập hợp con của vũ trụ là không thể. Thay vào đó, giới hạn nhỏ (k\le 7) cho chúng ta biết rằng số lượng vị trí bit là rất nhỏ. Chúng ta nên khai thác cấu trúc của một mạng con của mạng Boolean hơn là kích thước (2^k) của vũ trụ. 

Có hai trường hợp ranh giới đáng được quan tâm đặc biệt. Đầu tiên, (n=0) có nghĩa là không có số nguyên bắt buộc. Yêu cầu trống không có nghĩa là câu trả lời là (0), bởi vì mọi tập hợp hàng hóa khác trống đều hợp lệ. Ví dụ: với (k=1,n=0), các tập hợp lệ là ({0}), ({1}) và ({0,1}), vì vậy câu trả lời là (3). Thứ hai, một singleton luôn được đóng dưới AND và OR. Do đó, đối với (k=2,n=1,a_1=0), phải tính đơn lẻ ({0}). Việc quên các mạng đơn là một sai lầm dễ mắc phải khi xây dựng một biểu diễn giả sử cả tập trống và toàn bộ vũ trụ đều có mặt. 

Có một trường hợp tế nhị khác do từ “khác biệt” gây ra. Đầu vào như (k=3,n=2) có giá trị (1,1) không hợp lệ, do đó, việc kiểm tra "tất cả các giá trị bằng nhau" không thể xảy ra trong đầu vào hợp lệ. Tình huống liên quan là (n=1), trong đó giá trị yêu cầu duy nhất có thể có bất kỳ mẫu bit nào. 

## Phương pháp tiếp cận 

Sức mạnh rõ ràng là liệt kê mọi tập hợp con của (2^k) số nguyên có thể, kiểm tra xem nó có chứa tất cả các giá trị bắt buộc hay không, sau đó kiểm tra từng cặp phần tử xem có bao đóng trong AND và OR hay không. Ngay cả đối với (k=7), điều này có nghĩa là phải xem xét (2^{128}) họ, điều này nằm ngoài tầm với. Vấn đề không phải là yêu cầu chúng tôi tối ưu hóa việc quét qua (128) phần tử. Nó đòi hỏi chúng ta tránh liệt kê hoàn toàn các họ tùy tiện. 

Quan sát hữu ích là một họ tốt chính xác là một mạng con của mạng Boolean. Mỗi mạng con hữu hạn có phần tử tối thiểu thu được bằng cách AND tất cả các thành viên của nó và phần tử tối đa thu được bằng cách OR tất cả các thành viên của nó. Tọa độ luôn (0) hoặc luôn (1) có thể tách được ngay. Các tọa độ còn lại là những tọa độ thực sự khác nhau trong họ. 

Bây giờ hãy xem xét các tọa độ thay đổi đó. Hai tọa độ tương đương nhau nếu mọi thành viên của tập hợp tốt cung cấp cho chúng cùng một bit. Tương tự, chúng luôn xảy ra cùng nhau. Chúng ta có thể thay thế mỗi lớp tương đương bằng một tọa độ trừu tượng. Sau quá trình nén này, mọi tọa độ còn lại đều có thể được phân biệt thực sự bởi một số thành viên của mạng, do đó mạng con thu được có thứ hạng đầy đủ. 

Một mạng con cấp bậc đầy đủ của mạng Boolean cấp bậc (r) nằm trong song ánh với thứ tự một phần trên các phần tử được gắn nhãn (r). Cho một phần thứ tự, lấy tất cả các tập con đóng xuống của nó. Chúng bị đóng dưới giao và hợp, và vì thứ tự phản đối xứng nên chúng phân biệt tất cả (r) tọa độ trừu tượng. Ngược lại, từ một mạng con xếp hạng đầy đủ, xác định (x\le y) khi mọi phần tử mạng chứa (y) cũng chứa (x). Mối quan hệ thu được là một phần trật tự, và mạng ban đầu chính xác là họ các iđêan của nó. 

Điều này biến vấn đề ban đầu thành một bảng liệt kê rất nhỏ. Chúng tôi chọn vị trí bit nào có thể thay đổi, phân chia các vị trí đó thành các lớp tương đương và sau đó chọn thứ tự một phần trên các lớp. Các số nguyên bắt buộc chỉ áp đặt một điều kiện theo thứ tự một phần đó: mọi số nguyên bắt buộc phải tương ứng với một số lý tưởng.

Đối với một phân vùng cố định, mỗi khối có một chữ ký mô tả số nguyên bắt buộc chứa khối đó. Mối quan hệ bậc một phần (x<y) chỉ được phép khi mọi số nguyên bắt buộc chứa (y) cũng chứa (x). Thay vì xây dựng rõ ràng ràng buộc quan hệ đó, chúng ta liệt kê chính các thứ tự từng phần. Vì (k\le7), số thứ tự từng phần được gắn nhãn chỉ là (1,3,19,219,4231,130023,5941889) cho các cấp từ (1) đến (7). 

Các đơn đặt hàng một phần có thể được tạo đệ quy. Bắt đầu với thứ tự một phần trên các đỉnh có nhãn (r) và chèn một đỉnh mới. Các phần trước của nó tạo thành một tập hợp xuống (D), các phần tử kế tiếp của nó tạo thành một tập hợp lên (U) và mọi phần tử của (D) phải ở dưới mọi phần tử của (U). Mỗi cặp hợp lệ ((D,U)) cung cấp chính xác một phần mở rộng, do đó, điều này tạo ra mọi thứ tự một phần được gắn nhãn chính xác một lần. 

Việc triển khai cuối cùng không xây dựng được tất cả các lý tưởng của mọi trật tự từng phần. Đối với một đỉnh (v), đặt (down[v]) là các đỉnh đứng trước nó. Một tập con (S) không phải là tập lý tưởng một cách chính xác khi nó chứa (v) nhưng thiếu một phần tử nào đó của (down[v]). Chúng tôi mã hóa tất cả (2^r) tập hợp con dưới dạng bit của một số nguyên Python. Điều này cho phép chúng ta tính toán tập hợp các số không lý tưởng theo thứ tự một phần chỉ bằng một số thao tác bit, sau đó kiểm tra mọi phân vùng dựa trên nó bằng một số nguyên AND. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^{2^k}\cdot 2^{2k})) | (O(2^k)) | Quá chậm | 
| Bảng liệt kê kết cấu | (O(P_7\cdot k\cdot Q_7)) | Các trạng thái được tạo (O(P_7)) được truyền trực tuyến | Đã chấp nhận | 

Ở đây (P_7=5,941,889) là số thứ tự từng phần được gắn nhãn trên bảy phần tử và (Q_7) là số ràng buộc phân vùng riêng biệt có liên quan đến đầu vào. Với (k\le7), tất cả các đại lượng này đều đủ nhỏ để liệt kê cấu trúc. 

## Hướng dẫn thuật toán 

1. Đọc các số nguyên cần thiết và tính chữ ký của từng vị trí bit. Ký hiệu của tọa độ (b) là tập bit trên các giá trị đầu vào, với bit (i) được đặt chính xác khi (a_i) chứa tọa độ (b). Tọa độ có chữ ký thay đổi theo các giá trị bắt buộc không thể cố định trong một họ hợp lệ, trong khi tọa độ không đổi có thể vẫn cố định hoặc trở thành một phần của khối biến. 
2. Liệt kê mọi mặt nạ tọa độ biến đổi (V) có chứa tất cả các tọa độ khác nhau. Các tọa độ bên ngoài (V) là cố định và các giá trị cố định của chúng đã được xác định bởi mẫu đầu vào chung. 
3. Với mỗi (V), liệt kê mọi phân vùng tọa độ của nó thành các khối khác rỗng. Một khối chỉ hợp lệ khi tất cả tọa độ của nó có cùng chữ ký đầu vào. Nếu không, hai tọa độ trong cùng một lớp tương đương sẽ được phân biệt bằng một số nguyên bắt buộc, điều này là không thể. 
4. Đối với mỗi phân vùng hợp lệ, hãy ánh xạ từng số nguyên được yêu cầu vào một tập hợp con của các khối. Tập hợp con chứa khối (j) chính xác khi toàn bộ khối đó có mặt trong số nguyên được yêu cầu. Mã hóa tập hợp các tập hợp con khối bắt buộc này thành một số nguyên`req`, trong đó bit (S) được đặt nếu tập hợp con (S) xuất hiện trong số các yêu cầu. Nhóm bằng nhau`req`các giá trị và giữ tính bội số của chúng, bởi vì các phân vùng tọa độ khác nhau có thể áp đặt cùng một điều kiện cho thứ tự từng phần trừu tượng. 
5. Tạo đệ quy tất cả các đơn hàng một phần trên (0,1,\ldots,r-1). Khi thêm đỉnh mới (r), hãy chọn tập hợp xuống (D) của thứ tự cũ làm đỉnh trước đó. Những phần tử kế tiếp có thể tạo thành giới hạn trên chung của tất cả các phần tử của (D). Bất kỳ tập hợp nâng cấp (U) nào bên trong tập hợp giới hạn trên chung đó đều mang lại một phần mở rộng hợp lệ. 
6. Đối với mỗi đơn hàng một phần được tạo, hãy tính một tập hợp bit`bad`các bit tập hợp của nó chính xác là các tập con không phải là lý tưởng. Với mọi đỉnh (v), mọi tập con chứa (v) nhưng không chứa tất cả`down[v]`là xấu. Hợp của các tập hợp này trên tất cả các đỉnh là mặt nạ không lý tưởng hoàn chỉnh. 
7. Ràng buộc về phân vùng`req`được thỏa mãn chính xác khi`req & bad == 0`. Thêm bội số của mọi ràng buộc được thỏa mãn vào câu trả lời cho thứ hạng từng phần này. 
8. Vụ việc (n=0) được xử lý riêng. Không có chữ ký đầu vào để hạn chế việc xây dựng, vì vậy chúng tôi sử dụng số lượng được tính toán trước của các đơn hàng từng phần được gắn nhãn và số phân vùng theo số Stirling để đếm tất cả các lựa chọn tọa độ cố định có thể có và tất cả các phân vùng tọa độ biến đổi có thể có. 

Sau khi xử lý tất cả các đơn đặt hàng một phần, số lượng tích lũy chính xác là số lượng các bộ hàng hóa riêng biệt chứa mọi số nguyên được yêu cầu. 

### Tại sao nó hoạt động 

Mỗi tập hợp tốt có một tập hợp tọa độ duy nhất khác nhau, một phân vùng duy nhất của các tọa độ đó thành các lớp luôn hoạt động giống hệt nhau và một mạng con xếp hạng đầy đủ duy nhất trên các lớp đó. Cái sau được biểu diễn duy nhất bằng một trật tự từng phần, trong đó các lý tưởng của nó chính xác là các thành viên có thể có của tập hợp tốt. 

Các ràng buộc đầu vào được bảo toàn chính xác vì một số nguyên bắt buộc thuộc về mạng được biểu diễn khi và chỉ khi biểu diễn khối của nó là lý tưởng của thứ tự từng phần đã chọn. Việc xây dựng xem xét mọi phân tách tọa độ có thể và mọi thứ tự từng phần có thể có, trong khi mỗi tập tốt kết quả chỉ có một phân tách chính tắc như vậy. Do đó, mọi bộ hàng hóa hợp lệ đều được tính một lần và không có bộ hàng hóa không hợp lệ nào được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# Number of labeled partial orders on 0..7 elements.
POSets = [1, 1, 3, 19, 219, 4231, 130023, 5941889]

def partitions(mask):
    """Yield every unordered partition of the set bits of mask."""
    if mask == 0:
        yield ()
        return

    bit = mask & -mask
    rest = mask ^ bit

    for p in partitions(rest):
        # Put bit into an existing block.
        for i in range(len(p)):
            q = list(p)
            q[i] |= bit
            yield tuple(q)

        # Start a new block. Blocks stay ordered by their minimum bit.
        yield (bit,) + p

def solve_case(k, a):
    n = len(a)

    if n == 0:
        # g[r] = number of sublattices of B_r containing both
        # the empty set and the full set.
        # g[r] = sum_j S(r,j) * POSets[j].
        stirling = [[0] * 8 for _ in range(8)]
        stirling[0][0] = 1
        for i in range(1, 8):
            for j in range(1, i + 1):
                stirling[i][j] = stirling[i - 1][j - 1] + j * stirling[i - 1][j]

        bounded = [0] * 8
        for r in range(8):
            bounded[r] = sum(
                stirling[r][j] * POSets[j]
                for j in range(r + 1)
            )

        ans = 0
        for r in range(k + 1):
            # Choose r variable coordinates. Every other coordinate
            # can independently be fixed to 0 or 1.
            ans += (
                (1 << (k - r))
                * __import__("math").comb(k, r)
                * bounded[r]
            )
        return ans

    # Signature of every original coordinate.
    # Bit i is set when coordinate b occurs in a[i].
    sig = [0] * k
    for i, x in enumerate(a):
        bit = 1 << i
        for b in range(k):
            if (x >> b) & 1:
                sig[b] |= bit

    # Coordinates with non-constant signatures must be variable.
    varying = 0
    for b in range(k):
        if sig[b] != 0 and sig[b] != (1 << n) - 1:
            varying |= 1 << b

    # queries[r] is a dictionary:
    #   required-ideal-mask -> number of coordinate partitions producing it
    queries = [dict() for _ in range(k + 1)]

    all_coords = (1 << k) - 1

    # Every variable mask must contain all genuinely varying coordinates.
    optional = all_coords ^ varying
    sub = optional

    while True:
        V = varying | sub

        for blocks in partitions(V):
            r = len(blocks)

            # Every block must consist of coordinates with identical
            # signatures among the required elements.
            block_sig = []
            valid = True

            for block in blocks:
                first = block & -block
                b0 = first.bit_length() - 1
                s = sig[b0]

                rest = block ^ first
                while rest:
                    bit = rest & -rest
                    b = bit.bit_length() - 1
                    if sig[b] != s:
                        valid = False
                        break
                    rest ^= bit

                if not valid:
                    break
                block_sig.append(s)

            if not valid:
                continue

            # Convert every required integer into its block mask.
            req = 0
            for i in range(n):
                mask = 0
                ibit = 1 << i

                for j, s in enumerate(block_sig):
                    if s & ibit:
                        mask |= 1 << j

                req |= 1 << mask

            queries[r][req] = queries[r].get(req, 0) + 1

        if sub == 0:
            break
        sub = (sub - 1) & optional

    # For r=0 there is exactly one partial order.
    # Its only subset is the empty set, which is always an ideal.
    answer = 0
    if queries[0]:
        # A legal r=0 representation is a singleton.
        answer += sum(queries[0].values())

    # Process all partial orders of every rank in one recursive generation.
    for target in range(1, k + 1):
        if not queries[target]:
            continue

        # We generate only up to this target. Since targets are processed
        # separately, the code remains simple and k <= 7 keeps this safe.
        qitems = list(queries[target].items())

        contain_all = [0] * (1 << target)
        subset_count = 1 << target
        full_subset_bits = (1 << subset_count) - 1

        for d in range(subset_count):
            x = 0
            s = d
            while s < subset_count:
                x |= 1 << s
                s += 1
            contain_all[d] = x

        # The loop above is intentionally replaced below by a direct
        # construction, which is faster for these tiny dimensions.
        for d in range(subset_count):
            x = 0
            for s in range(subset_count):
                if (s & d) == d:
                    x |= 1 << s
            contain_all[d] = x

        contains_vertex = [
            contain_all[1 << v]
            for v in range(target)
        ]

        local_answer = 0

        def process(down):
            nonlocal local_answer

            bad = 0
            for v in range(target):
                bad |= contains_vertex[v] & (
                    full_subset_bits ^ contain_all[down[v]]
                )

            for req, multiplicity in qitems:
                if (req & bad) == 0:
                    local_answer += multiplicity

        def generate(m, down):
            if m == target:
                process(down)
                return

            old_all = (1 << m) - 1

            up = [0] * m
            for v in range(m):
                mask = 0
                for w in range(m):
                    if (down[w] >> v) & 1:
                        mask |= 1 << w
                up[v] = mask

            size = 1 << m
            is_down = [False] * size
            is_up = [False] * size
            is_down[0] = True
            is_up[0] = True

            for s in range(1, size):
                bit = s & -s
                v = bit.bit_length() - 1
                rest = s ^ bit

                is_down[s] = (
                    is_down[rest]
                    and (down[v] & ~s) == 0
                )
                is_up[s] = (
                    is_up[rest]
                    and (up[v] & ~s) == 0
                )

            xbit = 1 << m

            for D in range(size):
                if not is_down[D]:
                    continue

                # U must consist only of elements strictly above every
                # member of D.
                C = old_all
                bits = D
                while bits:
                    bit = bits & -bits
                    v = bit.bit_length() - 1
                    C &= up[v]
                    bits ^= bit

                U = C
                while True:
                    if is_up[U]:
                        nd = list(down)
                        nd.append(D)

                        bits2 = U
                        while bits2:
                            bit = bits2 & -bits2
                            v = bit.bit_length() - 1
                            nd[v] |= xbit
                            bits2 ^= bit

                        generate(m + 1, tuple(nd))

                    if U == 0:
                        break
                    U = (U - 1) & C

        generate(0, ())
        answer += local_answer

    return answer

def main():
    k, n = map(int, input().split())

    if n:
        a = list(map(int, input().split()))
    else:
        a = []

    print(solve_case(k, a))

if __name__ == "__main__":
    main()
```Phần đầu tiên của`solve_case`xử lý (n=0), trong đó không có thông tin chữ ký bắt buộc. Số lượng các mạng con giới hạn của hạng (r) có được bằng cách phân chia tọa độ (r) thành các lớp tương đương và đặt thứ tự một phần hạng đầy đủ cho các lớp đó. Số Stirling đếm các phân vùng. 

Với (n>0),`sig[b]`ghi lại chính xác giá trị bắt buộc nào chứa tọa độ (b). Mặt nạ`varying`xác định tọa độ có giá trị không đổi trên đầu vào được yêu cầu. Các tọa độ như vậy phải thay đổi trong mọi mạng hợp lệ, trong khi mọi tọa độ khác có thể cố định hoặc thay đổi. 

các`partitions`Trình tạo sử dụng tọa độ nhỏ nhất còn lại để giữ cho các khối được sắp xếp chính xác. Điều này tránh việc đếm cùng một phân vùng không có thứ tự nhiều lần. 

Từ điển`queries[r]`là cầu nối giữa các vị trí bit ban đầu và poset trừu tượng. Một khóa từ điển duy nhất mô tả tất cả các tập hợp con bắt buộc phải là lý tưởng. Tính đa dạng của nó ghi lại có bao nhiêu phân vùng tọa độ khác nhau dẫn đến cùng một yêu cầu trừu tượng. 

Đệ quy`generate`hàm xây dựng các thứ tự từng phần bằng cách chèn một đỉnh mới có nhãn lớn nhất.`D`chứa những người tiền nhiệm của nó và`U`những người kế nhiệm nó. Tập hợp tiền thân phải là tập hợp xuống, tập hợp kế tiếp phải là tập hợp nâng cao và mọi tập hợp tiền thân phải ở dưới mọi tập hợp kế tiếp. Những điều kiện này chính xác là những gì làm cho mối quan hệ thu được có tính bắc cầu. 

các`bad`bitset là thủ thuật triển khai hữu ích nhất.`contain_all[d]`chứa mọi mặt nạ tập hợp con có chứa`d`. Đối với một đỉnh (v), biểu thức`contains_vertex[v] & ~contain_all[down[v]]`đại diện cho tất cả các tập con chứa (v) nhưng thiếu ít nhất một phần trước. Sự kết hợp của họ chính xác là tập hợp của những điều không lý tưởng. 

Số nguyên Python có độ chính xác tùy ý, vì vậy các bitset ở đây có thể chứa (2^7=128) bit một cách an toàn. Không có vấn đề tràn số nguyên. Mặt nạ tập hợp con cho một phân vùng sử dụng tối đa bảy bit, trong khi mặt nạ bên ngoài`req`mặt nạ sử dụng tối đa (128) bit. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên là (k=2,n=1,a_1=0). Vì chỉ có một giá trị bắt buộc nên cả hai vị trí bit đều có chữ ký không đổi (0). Chúng có thể được cố định hoặc có thể thay đổi. 

| Tọa độ thay đổi | Phân vùng | Số khối trừu tượng | Lệnh từng phần hợp lệ chứa 0 | 
| --- | --- | --- | --- | 
| không | phân vùng trống | 0 | 1 | 
| bit 0 | ({0}) | 1 | 1 | 
| bit 1 | ({1}) | 1 | 1 | 
| cả hai | ({0,1}) | 1 | 1 | 
| cả hai | ({0},{1}) | 2 | 3 | 

Bốn biểu diễn đầu tiên cho bốn bộ tốt. Phân vùng cuối cùng có hai khối và mọi thứ tự một phần trên hai phần tử được gắn nhãn đều có tập trống là lý tưởng, vì vậy cả ba lệnh một phần đều hoạt động. Tổng số là (1+1+1+1+3=7), khớp với đầu ra mẫu. 

Dấu vết này cũng giải thích tại sao phải tính một đơn vị như ({0}). Biểu diễn khối 0 là một tập hợp tốt thực sự, không phải là một họ trống không hợp lệ. 

### Mẫu 2 

Đối với (k=4) và các giá trị bắt buộc (1,2,7), ký hiệu tọa độ đủ khác nhau để một số tọa độ buộc phải giữ nguyên thay đổi. Đối với mỗi mặt nạ biến ứng cử viên, bước phân vùng sẽ từ chối bất kỳ khối nào chứa tọa độ có chữ ký yêu cầu khác nhau. 

Đối với phân vùng còn tồn tại, mỗi số nguyên được yêu cầu sẽ trở thành tập hợp con của các khối trừu tượng. Sau đó, trình tạo thứ tự một phần chỉ đếm các thứ tự trong đó cả ba tập hợp con đó đều là lý tưởng. 

| Giá trị bắt buộc | Mặt nạ khối trừu tượng | 
| --- | --- | 
| 1 | được xác định bởi các khối chứa bit 0 | 
| 2 | được xác định bởi các khối chứa bit 1 | 
| 7 | được xác định bởi các khối chứa bit 0, 1, 2 | 

Mỗi thứ tự một phần được chấp nhận đại diện cho một tập hợp hàng hóa riêng biệt sau khi mở rộng các khối trừu tượng trở lại tọa độ ban đầu của chúng. Tổng hợp tất cả các phân vùng và đơn đặt hàng hợp lệ cho (29), đầu ra mẫu. 

Bất biến quan trọng ở đây là giá trị bắt buộc không bao giờ được kiểm tra bằng cách xây dựng rõ ràng toàn bộ mạng. Tư cách thành viên của nó được rút gọn thành câu hỏi duy nhất là liệu tập hợp con khối trừu tượng của nó có lý tưởng hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(P_7\cdot k + C_k\cdot P_7)) trong bảng liệt kê cấu trúc tồi tệ nhất | (P_7=5,941,889), trong khi (k\le7) và số lượng ràng buộc phân vùng có liên quan là rất nhỏ | 
| Không gian | (O(k2^k)) bên cạnh đệ quy | Tất cả các điều kiện tập hợp con sử dụng tối đa (128) bit và các đơn hàng một phần được truyền trực tuyến thay vì được lưu trữ | 

Sự khác biệt quan trọng so với lực lượng vũ phu là thuật toán liệt kê các đơn hàng một phần trên tối đa bảy tọa độ trừu tượng thay vì các tập hợp con tùy ý của vũ trụ phần tử (128). Số lượng đơn hàng một phần được gắn nhãn lớn nhất là khoảng (5,9) triệu, số này hữu hạn và có thể quản lý được, đồng thời trình tạo đệ quy không bao giờ lưu trữ tất cả chúng cùng một lúc. 

## Trường hợp thử nghiệm```python
# This test harness assumes the editorial solution has been placed above
# in a file named solution.py. For a standalone local test, copy the
# solve_case function and main implementation into the same file.

import sys
import io

# Reuse the solve_case function from the solution.
# The helper accepts exactly the input format used by the judge.
def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        k, n = map(int, input().split())
        a = list(map(int, input().split())) if n else []
        print(solve_case(k, a))
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("2 1\n0\n") == "7", "sample 1"
assert run("4 3\n1 2 7\n") == "29", "sample 2"

# Minimum k, one required value.
assert run("1 1\n0\n") == "2", "minimum size"

# Same boundary case, but requiring the other element.
assert run("1 1\n1\n") == "2", "upper boundary"

# Two extreme elements in B_2.
# The valid families are {0,3}, {0,1,3}, {0,2,3}, and the full B_2.
assert run("2 2\n0 3\n") == "4", "fixed minimum and maximum"

# Maximum-size input. Requiring every element forces the entire universe.
assert run(
    "7 128\n" +
    " ".join(map(str, range(128))) +
    "\n"
) == "1", "all elements required"

# No required elements.
assert run("7 0\n") == "12982681", "empty requirement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 0`| 2 | Hộp tối thiểu (k), hộp đơn và hộp đầy đủ | 
|`1 1 / 1`| 2 | Ranh giới trên đối xứng | 
|`2 2 / 0 3`| 4 | Yêu cầu tối thiểu và tối đa, bao gồm cả mạng trung gian | 
|`7 128 / 0 ... 127`| 1 | Tối đa (n), buộc vũ trụ đầy đủ duy nhất | 
|`7 0`| 12982681 | Yêu cầu trống và công thức tính riêng (n=0) | 

## Vỏ cạnh 

Với (k=1,n=1,a_1=0), đầu vào chỉ có hai số nguyên có thể là (0) và (1). Các tập hợp tốt chứa (0) là ({0}) và ({0,1}), vì vậy câu trả lời là (2). Thuật toán thu được một biểu diễn không có tọa độ thay đổi và một biểu diễn có biến tọa độ duy nhất. 

Với (k=2,n=1,a_1=0), đáp án là (7). Biểu diễn khối 0 cho ({0}), biểu diễn một khối cho ({0,1}), ({0,2}) và ({0,3}) và biểu diễn hai khối đóng góp cả ba thứ tự một phần trên hai phần tử. Tổng số là (7). 

Đối với (k=2,n=2) với các giá trị bắt buộc (0) và (3), cả hai tọa độ đều khác nhau trên đầu vào, do đó cả hai phải thuộc về phần biến. Phân vùng một khối cho ({0,3}). Phân vùng hai khối cung cấp ba mạng con xếp hạng đầy đủ tương ứng với ba thứ tự một phần trên hai phần tử được gắn nhãn. Do đó đáp án là (4). 

Với (k=7,n=128), mọi số nguyên có thể đều được yêu cầu. Mỗi vị trí bit đều khác nhau nên không thể cố định được tọa độ. Hơn nữa, mỗi tọa độ phải tạo thành lớp tương đương của riêng nó, bởi vì đầu vào chứa các giá trị phân biệt từng cặp tọa độ. Các tập hợp con khối bắt buộc là tất cả (128) tập hợp con của bảy tọa độ trừu tượng. Thứ tự một phần duy nhất mà mọi tập hợp con đều là lý tưởng là phản chuỗi, mạng lý tưởng của nó là toàn bộ mạng Boolean. Do đó đáp án chính xác là (1). 

Đối với (n=0), không có thông tin chữ ký và mọi tọa độ có thể được cố định thành (0), cố định thành (1) hoặc được đặt vào một trong các lớp tương đương biến. Đối với hạng (r), số lượng mạng con bị chặn là biến đổi Stirling của số lượng bậc một phần. Kết hợp điều này với việc lựa chọn tọa độ thay đổi và phép gán (2^{k-r}) của tọa độ cố định sẽ mang lại (12,982,681) tập hợp tốt cho (k=7). Trường hợp này không thể được xử lý bằng kiểm tra yêu cầu dựa trên chữ ký vì không có giá trị bắt buộc nào để lấy chữ ký.
