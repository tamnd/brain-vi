---
title: "CF 102192I - Làm ZYB Hạnh Phúc"
description: "Chúng tôi có (n) danh hiệu. Tiêu đề (ti) có giá trị hạnh phúc (hi). Đối với bất kỳ chuỗi (x) nào, hãy xem mọi tiêu đề trong đó (x) xuất hiện ít nhất một lần. Niềm vui của ZYB khi nói (x) chính là kết quả của (hi) tương ứng."
date: "2026-08-18T02:09:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102192
codeforces_index: "I"
codeforces_contest_name: "2018 Chinese Multi-University Training, Nanjing U Contest"
rating: 0
weight: 102192
solve_time_s: 267
verified: true
draft: false
---

[CF 102192I - Làm cho ZYB hạnh phúc](https://codeforces.com/problemset/problem/102192/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có (n) danh hiệu. Tiêu đề (t_i) có giá trị hạnh phúc (h_i). Đối với bất kỳ chuỗi (x) nào, hãy xem mọi tiêu đề trong đó (x) xuất hiện ít nhất một lần. Niềm vui của ZYB khi nói (x) chính là tích của (h_i) tương ứng. Xảy ra nhiều lần trong cùng một tiêu đề sẽ không nhân giá trị lên nữa. 

Đối với một truy vấn (m), mọi chuỗi chữ thường không trống có độ dài tối đa (m) được chọn với xác suất bằng nhau. Nếu một chuỗi không phải là chuỗi con của bất kỳ tiêu đề nào thì mức độ hạnh phúc của nó bằng không. Chúng ta cần modulo hạnh phúc mong đợi (10^9+7). 

Dữ liệu đầu vào chứa tối đa (10^4) tiêu đề, trong khi tổng độ dài của chúng tối đa là (3\cdot10^5). Tổng chiều dài này là tham số quan trọng đối với cấu trúc dữ liệu chuỗi, bởi vì một máy tự động hậu tố có kích thước tuyến tính trong tổng chiều dài đầu vào. Số lượng truy vấn cũng có thể đạt tới (3\cdot10^5), vì vậy việc trả lời từng truy vấn bằng cách quét tất cả các độ dài là quá tốn kém. Độ dài truy vấn có thể đạt tới (10^6), có nghĩa là mẫu số của phân bố xác suất phải được xử lý độc lập với độ dài tiêu đề. 

Với (m) cố định, số chuỗi có thể có là 

[ 
D_m=26^1+26^2+\cdots+26^m. 
] 

Tử số là tổng các giá trị hạnh phúc của tất cả các chuỗi có độ dài tối đa khác nhau (m). Việc liệt kê trực tiếp là không thể. Ngay cả một tiêu đề có độ dài (300000) cũng có (300000\cdot300001/2=45000150000) lần xuất hiện chuỗi con, trước khi xóa các bản sao. 

Có một số chỗ dễ mắc phải sai lầm thầm lặng. Đầu tiên, những lần xuất hiện lặp đi lặp lại trong một danh hiệu không được phép nhân lên niềm hạnh phúc nhiều hơn một lần. Ví dụ,```
1
aaa
2
1
1
```Chuỗi dài một hữu ích duy nhất là`a`, và hạnh phúc của nó là (2), không phải (2^3), nên câu trả lời là (2/26=1/13), cụ thể là (153846155) modulo số nguyên tố đã cho. Việc triển khai dựa trên sự xuất hiện sẽ xử lý không chính xác ba bản sao của`a`như ba đóng góp độc lập. 

Thứ hai, cùng một chuỗi xuất hiện trong nhiều tiêu đề phải nhân giá trị của chúng lên. Ví dụ,```
2
a
a
2 3
1
1
```Chuỗi`a`xuất hiện ở cả hai tựa đề, nên hạnh phúc của nó là (2\cdot3=6). Câu trả lời đúng là (26/6=461538465). Việc triển khai chỉ lưu trữ mỗi chuỗi riêng biệt một lần mà không có bộ tiêu đề của nó có thể sử dụng sai (2+3) hoặc một trong hai giá trị. 

Thứ ba, một truy vấn có thể dài hơn mọi tiêu đề. Coi như```
1
a
1
1
2
```Với (m=2), chỉ`a`đóng góp vào tử số nên tử số vẫn là (1), nhưng mẫu số là (26+26^2=702). Câu trả lời là (702^{-1}=206552708). Mẫu số không được giới hạn ở tiêu đề dài nhất. 

Kho lưu trữ chính thức chứa vấn đề ban đầu và dữ liệu mẫu. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản về mặt khái niệm. Liệt kê mọi chuỗi ứng cử viên có độ dài tối đa (m), tìm kiếm nó trong mọi tiêu đề, xác định tiêu đề nào chứa nó, nhân giá trị hạnh phúc của chúng và cộng kết quả. Điều này đúng vì nó trực tiếp tuân theo định nghĩa về giá trị kỳ vọng. Thật không may, số lượng ứng cử viên là 

[ 
26+26^2+\cdots+26^m=\Theta(26^m). 
] 

Với (m=10^6), điều này vượt xa mọi phép tính có ý nghĩa. Ngay cả khi chúng ta tránh liệt kê các ứng cử viên không bao giờ xuất hiện và thay vào đó liệt kê tất cả các chuỗi con của tiêu đề, thì một tiêu đề có độ dài (300000) có (45000150000) lần xuất hiện chuỗi con. 

Lực lượng vũ phu hoạt động vì mọi chuỗi riêng lẻ có thể được đánh giá độc lập, nhưng nó thất bại vì hầu hết tất cả công việc được lặp lại giữa các chuỗi con chồng chéo cao. Quan sát quan trọng là các chuỗi con nhóm hậu tố automata có cùng một tập hợp các vị trí kết thúc. Đặc biệt, tất cả các chuỗi được biểu thị bằng một trạng thái tự động hậu tố có cùng một tập hợp xuất hiện, do đó chúng cũng xuất hiện trong cùng một tập hợp tiêu đề. Do đó, giá trị hạnh phúc của họ là giống hệt nhau. 

Điều này làm cho một hậu tố tổng quát được nén tự động. Chúng tôi xây dựng một máy tự động chứa tất cả các tiêu đề, đặt lại trạng thái hiện tại về gốc trước khi chèn từng tiêu đề mới. Đối với mỗi tiêu đề, chúng tôi sẽ xem qua nó trong máy tự động. Ở mỗi trạng thái đạt được, tổ tiên liên kết hậu tố của nó biểu thị các hậu tố của tiền tố hiện tại, vì vậy tất cả các trạng thái đó tương ứng với các chuỗi con xuất hiện trong tiêu đề này. Chúng tôi nhân giá trị của trạng thái với (h_i) của tiêu đề, nhưng chỉ một lần cho mỗi tiêu đề. 

Sau đó, một trạng thái tự động hậu tố vẫn biểu thị một số độ dài chuỗi con khác nhau. Nếu trạng thái (v) có độ dài`len[v]`và liên kết hậu tố (fa[v]), thì nó đại diện chính xác một chuỗi con riêng biệt cho mỗi độ dài trong 

[ 
[\text{len tính [fa[v]]+1,\text{len[v]]. 
] 

Tất cả các chuỗi con đó có cùng tập xuất hiện và do đó có cùng giá trị hạnh phúc. Chúng ta có thể thêm giá trị đó vào một khoảng bằng cách sử dụng mảng sai phân. Sau đó, hai tổng tiền tố biến các khoảng trạng thái này thành tổng hạnh phúc cho mọi độ dài chính xác và cuối cùng là tổng hạnh phúc cho mọi độ dài lên đến (m). 

Cấu trúc automaton hậu tố tổng quát được sử dụng ở đây là cấu trúc tiêu chuẩn để chèn một số chuỗi độc lập với gốc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (\Theta(26^m)) hoặc (\Theta(L^2)) chỉ để liệt kê các lần xuất hiện chuỗi con | Có khả năng (\Theta(26^m)) | Quá chậm | 
| SAM tổng quát | (O(L+M+Q)) được khấu hao, trong đó (L) là tổng chiều dài tiêu đề và (M) là truy vấn tối đa | (O(L+M+Q)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các tiêu đề trước và gọi (L) là tổng độ dài của chúng. Một máy tự động hậu tố được xây dựng bởi tiện ích mở rộng thông thường có nhiều nhất (2L+1) trạng thái, vì vậy chúng tôi có thể dự trữ đủ dung lượng lưu trữ trước khi xây dựng nó. 
2. Xây dựng một máy tự động hậu tố tổng quát. Trước khi chèn mỗi tiêu đề, hãy đặt`last`tới tận gốc. Khi quá trình chuyển đổi đã tồn tại từ trạng thái hiện tại, hãy sử dụng lại nó nếu độ dài của nó lớn hơn chính xác một độ dài so với độ dài của trạng thái hiện tại. Nếu không, hãy tạo một bản sao, chính xác như trong cấu trúc ô tô hậu tố thông thường. 

Việc xử lý đặc biệt quá trình chuyển đổi hiện có là điều cho phép một số tựa game độc ​​lập chia sẻ cùng một máy tự động mà không cần chèn các ký tự phân tách nhân tạo. 
3. Khởi tạo giá trị hạnh phúc của mọi trạng thái thành (1). Đối với mỗi tiêu đề (t_i), hãy duyệt qua tiêu đề từ gốc. Sau khi đạt đến trạng thái tương ứng với tiền tố hiện tại, hãy đi theo các liên kết hậu tố hướng lên trên. Mỗi trạng thái được truy cập biểu thị các chuỗi con xuất hiện trong (t_i), do đó hãy nhân giá trị được lưu trữ của nó với (h_i). 

Một trạng thái chỉ cần được cập nhật một lần cho một tiêu đề. Lưu trữ chỉ mục của tiêu đề hiện tại trong`seen[state]`. Khi bước đi lên đến trạng thái đã được đánh dấu bằng chỉ mục tiêu đề đó, hãy dừng lại. Tất cả tổ tiên liên kết hậu tố của nó đã được đánh dấu trong lần đi bộ trước đó. 
4. Sau khi xử lý mọi nhan đề, trạng thái (v) có giá trị bằng tích của (h_i) trên chính xác những nhan đề chứa chuỗi con được biểu thị bằng (v). 

Điều này hiệu quả vì các trạng thái tự động hóa hậu tố là các lớp tương đương của chuỗi có tập hợp vị trí cuối bằng nhau. Các tập hợp vị trí cuối bằng nhau ngụ ý các tập hợp chức danh-thành viên bằng nhau, mặc dù vị trí xuất hiện thực tế có thể khác nhau giữa các chức danh. 
5. Với mọi trạng thái không phải gốc (v), hãy thêm`value[v]`đến khoảng độ dài 

[ 
[\text{len tính [fa[v]]+1,\text{len[v]]. 
] 

Lưu trữ cái này với một mảng khác biệt: 

[ 
khác biệt[\text{len[fa[v]]+1] += giá trị[v], 
] 

[ 
diff[\text{len[v]+1] -= giá trị[v]. 
] 

Không cần phải duyệt qua cây liên kết hậu tố ở đây. Mọi trạng thái đều đã biết liên kết hậu tố và độ dài của nó, vì vậy tất cả các khoảng đều có thể được xử lý trực tiếp. 
6. Lấy tổng tiền tố của mảng sai phân. Sau lần vượt qua này,`by_len[k]`là tổng giá trị hạnh phúc của tất cả các chuỗi riêng biệt có độ dài chính xác (k). 
7. Lấy tổng tiền tố thứ hai. Hiện nay`prefix[k]`là tổng hạnh phúc của mọi chuỗi phân biệt có độ dài nằm trong khoảng từ (1) đến (k). Các chuỗi không phải là chuỗi con của bất kỳ tiêu đề nào sẽ tự động đóng góp bằng 0. 
8. Đối với mỗi truy vấn (m), kỳ vọng mong muốn là 

[ 
\frac{\text{tiền tố[m]} 
{26^1+26^2+\cdots+26^m} 
\pmod {10^9+7}. 
] 

Tử số trở thành không đổi khi (m) vượt quá tiêu đề dài nhất, nhưng mẫu số vẫn tiếp tục tăng. Đây là lý do tại sao các truy vấn lớn hơn tiêu đề dài nhất vẫn phải sử dụng (m) gốc của chúng. 
9. Vì có thể có (3\cdot10^5) truy vấn, việc tính toán nghịch đảo mô đun riêng biệt cho mỗi mẫu số sẽ có tác dụng cộng (O(Q\log MOD)). Thay vào đó, hãy sắp xếp các giá trị truy vấn riêng biệt, tính toán mẫu số của chúng trong khi duyệt qua các độ dài một lần và sử dụng tính năng đảo ngược hàng loạt. Tích của tất cả các mẫu số được đảo ngược một lần, sau đó mọi nghịch đảo riêng lẻ được phục hồi theo thời gian tuyến tính. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ trạng thái tự động hậu tố tổng quát nào (v). Tất cả các chuỗi được biểu diễn của nó đều có cùng một tập hợp vị trí cuối, vì vậy chúng xuất hiện trong cùng một tiêu đề. Do đó, phép nhân được thực hiện trong khi xử lý mọi tiêu đề sẽ mang lại chính xác giá trị hạnh phúc của mỗi chuỗi được biểu thị bằng (v). Cấu trúc liên kết hậu tố phân chia tất cả các chuỗi con riêng biệt thành các khoảng độ dài rời nhau ((\text{len[fa[v]],\text{len[v]]), với một chuỗi con riêng biệt cho mỗi độ dài trong khoảng đó. Việc thêm giá trị của trạng thái vào khoảng này sẽ đếm mọi chuỗi xuất hiện riêng biệt chính xác một lần. Hai tổng tiền tố chuyển đổi những đóng góp theo độ dài này thành tử số cho mọi truy vấn, trong khi mẫu số đếm mọi chuỗi ngẫu nhiên có thể có, kể cả các chuỗi không xuất hiện. Do đó, phân số cuối cùng chính xác là mong đợi cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from array import array

MOD = 1000000007
ALPHA = 26

def solve():
    n = int(input())
    titles = [input().strip().encode() for _ in range(n)]
    total_len = sum(len(s) for s in titles)
    max_title_len = max(len(s) for s in titles)

    happiness = list(map(int, input().split()))

    q = int(input())
    queries = [int(input()) for _ in range(q)]
    max_query = max(queries)

    # A SAM built from L characters has at most 2L+1 states.
    max_states = 2 * total_len + 5

    # Compact 32-bit arrays are necessary in Python.
    # transitions[state * 26 + c] stores the destination.
    trans = array('i', [0]) * (max_states * ALPHA)
    link = array('i', [0]) * max_states
    length = array('i', [0]) * max_states
    seen = array('i', [0]) * max_states
    value = array('i', [1]) * max_states

    # Root is state 1.
    size = 1

    for s in titles:
        last = 1

        for ch in s:
            c = ch - 97
            p = last
            edge = trans[p * ALPHA + c]

            if edge:
                # The transition already exists.
                qstate = edge

                if length[qstate] == length[p] + 1:
                    last = qstate
                    continue

                # The existing transition is too long, so clone it.
                clone = size + 1
                size = clone

                length[clone] = length[p] + 1
                link[clone] = link[qstate]

                src = qstate * ALPHA
                dst = clone * ALPHA
                trans[dst:dst + ALPHA] = trans[src:src + ALPHA]

                while p and trans[p * ALPHA + c] == qstate:
                    trans[p * ALPHA + c] = clone
                    p = link[p]

                link[qstate] = clone
                last = clone
                continue

            # Create the usual new state.
            new_state = size + 1
            size = new_state
            length[new_state] = length[p] + 1
            last = new_state

            while p and trans[p * ALPHA + c] == 0:
                trans[p * ALPHA + c] = new_state
                p = link[p]

            if p == 0:
                link[new_state] = 1
                continue

            qstate = trans[p * ALPHA + c]

            if length[qstate] == length[p] + 1:
                link[new_state] = qstate
                continue

            # Split qstate with a clone.
            clone = size + 1
            size = clone

            length[clone] = length[p] + 1
            link[clone] = link[qstate]

            src = qstate * ALPHA
            dst = clone * ALPHA
            trans[dst:dst + ALPHA] = trans[src:src + ALPHA]

            link[qstate] = clone
            link[new_state] = clone

            while p and trans[p * ALPHA + c] == qstate:
                trans[p * ALPHA + c] = clone
                p = link[p]

    # For each title, mark every SAM state whose represented strings occur
    # in that title, and multiply its happiness exactly once.
    for tag, (s, h) in enumerate(zip(titles, happiness), 1):
        cur = 1

        for ch in s:
            cur = trans[cur * ALPHA + ch - 97]

            v = cur
            while v and seen[v] != tag:
                seen[v] = tag
                value[v] = value[v] * h % MOD
                v = link[v]

    # Difference array over substring lengths.
    diff = array('i', [0]) * (max_title_len + 2)

    for v in range(2, size + 1):
        left = length[link[v]] + 1
        right = length[v]

        diff[left] += value[v]
        if diff[left] >= MOD:
            diff[left] -= MOD

        diff[right + 1] -= value[v]
        if diff[right + 1] < 0:
            diff[right + 1] += MOD

    # First prefix sum gives the contribution of each exact length.
    # Second prefix sum gives the contribution of all lengths <= m.
    current = 0
    cumulative = 0

    for i in range(1, max_title_len + 1):
        current += diff[i]
        if current >= MOD:
            current -= MOD

        cumulative += current
        if cumulative >= MOD:
            cumulative -= MOD

        diff[i] = cumulative

    # Compute denominators for the distinct queried lengths.
    unique_queries = sorted(set(queries))
    denominators = []

    power = 1
    denominator = 0
    position = 0

    for m in unique_queries:
        while position < m:
            power = power * 26 % MOD
            denominator += power
            if denominator >= MOD:
                denominator -= MOD
            position += 1

        denominators.append(denominator)

    # Batch inversion of all distinct denominators.
    k = len(denominators)
    prefix_product = [1] * k
    product = 1

    for i, d in enumerate(denominators):
        prefix_product[i] = product
        product = product * d % MOD

    inverse_product = pow(product, MOD - 2, MOD)
    inverses = [0] * k

    for i in range(k - 1, -1, -1):
        inverses[i] = inverse_product * prefix_product[i] % MOD
        inverse_product = inverse_product * denominators[i] % MOD

    inverse_by_query = {
        m: inv for m, inv in zip(unique_queries, inverses)
    }

    output = []

    for m in queries:
        if m <= max_title_len:
            numerator = diff[m]
        else:
            numerator = diff[max_title_len]

        answer = numerator * inverse_by_query[m] % MOD
        output.append(str(answer))

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai sẽ đọc tất cả các tiêu đề trước khi phân bổ máy tự động. Điều đó cho biết trước tổng chiều dài, do đó bảng chuyển tiếp có thể được lưu trữ trong một tập tin nhỏ gọn.`array('i')`với (26(2L+5)) mục nhập. Danh sách danh sách Python sẽ sử dụng nhiều bộ nhớ hơn vì mọi số nguyên và mọi danh sách đều mang chi phí đối tượng Python. 

Mã chèn là phiên bản tổng quát của phần mở rộng tự động hậu tố thông thường. Trạng thái hiện tại được đặt lại về gốc cho mỗi tiêu đề. Khi một quá trình chuyển đổi hiện có có độ dài chính xác được yêu cầu, nó có thể biểu thị trực tiếp tiền tố mới. Nếu nó dài hơn yêu cầu thì cần phải nhân bản để tách các lớp tương đương. Bản sao nhận được bản sao của các chuyển đổi đi và kế thừa liên kết hậu tố cũ. 

các`seen`mảng được lập chỉ mục theo số tiêu đề. Điều này rẻ hơn so với việc xóa một mảng boolean cho mỗi tiêu đề. Trong quá trình di chuyển của một tiêu đề, đạt đến trạng thái có`seen`giá trị đã bằng tiêu đề hiện tại có nghĩa là trạng thái và tất cả tổ tiên liên kết hậu tố phía trên nó đã được xử lý. 

Phần gốc bị bỏ qua một cách có chủ ý khi xây dựng mảng khác biệt. Nó đại diện cho chuỗi rỗng, trong khi chuỗi ngẫu nhiên trong bài toán phải khác rỗng. Khoảng thời gian bắt đầu lúc`length[link[v]] + 1`, và điểm cuối là`length[v]`, do đó ranh giới bên phải là bao hàm. Phép trừ được đặt tại`right + 1`, đây là quy ước về mảng sai phân tiêu chuẩn. 

Tất cả các sản phẩm hạnh phúc đều được giảm modulo (10^9+7) ngay lập tức. Số nguyên Python không bị tràn, nhưng việc trì hoãn việc giảm mô-đun sẽ làm cho các giá trị lớn không cần thiết và làm chậm quá trình nhân. 

Mẫu số không được lưu trữ cho mọi độ dài. Các truy vấn được sắp xếp và khả năng hoạt động của (26) chỉ được nâng cao khi cần thiết. Điều này sử dụng thời gian (O(M)) trong đó (M) là truy vấn lớn nhất. Đảo ngược hàng loạt sau đó giảm tất cả các đảo ngược mẫu số thành một lũy thừa mô-đun cộng với công việc tuyến tính. 

Mảng tử số chỉ cần mở rộng đến tiêu đề dài nhất. Ngoài điểm đó không có chuỗi mới nào xuất hiện nên tử số không đổi. Tuy nhiên, mẫu số vẫn tiếp tục tăng đối với mọi truy vấn lớn hơn. 

## Ví dụ đã hoạt động 

Mẫu chính thức là```
2
zybnb
ybyb
3 5
4
1
2
3
4
```Đối với tiêu đề đầu tiên, giá trị hạnh phúc là (3) và đối với tiêu đề thứ hai là (5). Các chuỗi xuất hiện riêng biệt được nhóm theo độ dài có tổng số hạnh phúc như sau. 

| Chiều dài | Các chuỗi xuất hiện khác biệt | Đóng góp của chiều dài này | Tử số tích lũy | 
| --- | --- | --- | --- | 
| 1 |`z`,`y`,`b`,`n`| (3+15+15+3=36) | 36 | 
| 2 |`zy`,`yb`,`bn`,`nb`,`by`| (3+15+3+3+5=29) | 65 | 
| 3 |`zyb`,`ybn`,`bnb`,`yby`,`byb`| (3+3+3+5+5=19) | 84 | 
| 4 |`zybn`,`ybnb`,`ybyb`| (3+3+5=11) | 95 | 

Với (m=1), mẫu số là (26), do đó kỳ vọng là (36/26=18/13), cho`769230776`. Với (m=2), mẫu số là (26+676=702), và tử số là (65), cho`425925929`. Hai truy vấn còn lại sử dụng tử số (84) và (95), tạo ra kết quả đầu ra chính thức`891125950`Và`633120399`. 

Tính toán SAM cấp tiểu bang được nén vào các đóng góp độ dài đó. Ví dụ: một trạng thái có khoảng thời gian từ độ dài (2) đến (4) đóng góp giá trị hạnh phúc duy nhất của nó cho cả ba độ dài đó, đó chính xác là những gì mảng khác biệt thể hiện. 

Ví dụ thứ hai, lấy một tiêu đề:```
1
ab
2
3
1
2
3
```Các chuỗi hữu ích là`a`,`b`, Và`ab`, mỗi người đều có hạnh phúc (2). Việc xử lý theo chiều dài là: 

| Chiều dài | Đóng góp của nhà nước sau khoảng thời gian liên kết hậu tố | Tổng chiều dài chính xác | Tử số tích lũy | Mẫu số | 
| --- | --- | --- | --- | --- | 
| 1 |`a`,`b`mỗi người đóng góp 2 | 4 | 4 | 26 | 
| 2 |`ab`đóng góp 2 | 2 | 6 | 702 | 
| 3 | không có chuỗi nào xảy ra | 0 | 6 | 18278 | 

Do đó, các câu trả lời là (4/26=307692310), (6/702=239316241) và (6/18278) modulo (10^9+7). 

Ví dụ này thực hiện ranh giới của khoảng trạng thái. Chuỗi con`ab`phải đóng góp với độ dài chính xác (2), trong khi hậu tố của nó`a`Và`b`được đại diện thông qua các tiểu bang khác. Nó cũng chứng minh rằng khi truy vấn vượt quá độ dài tiêu đề tối đa, tử số sẽ ngừng thay đổi nhưng mẫu số thì không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(L+M+Q)) khấu hao | Cấu trúc SAM và đánh dấu tiêu đề là tuyến tính trong tổng kích thước tiêu đề, tổng hợp độ dài là tuyến tính, mẫu số lấy (O(M)) và đảo ngược hàng loạt cộng với đầu ra lấy (O(Q)) | 
| Không gian | (O(L+M+Q)) | SAM có các trạng thái và chuyển tiếp (O(L)), mảng chênh lệch độ dài có các mục nhập (O(L)) và các truy vấn cộng với mảng đảo ngược hàng loạt sử dụng khoảng trắng (O(Q)) | 

Ở đây (L\le3\cdot10^5), (M\le10^6) và (Q\le3\cdot10^5). Các mảng số nguyên nhỏ gọn đặc biệt hữu ích trong Python vì bảng chuyển đổi chứa khoảng (26\cdot2L) số nguyên bốn byte thay vì hàng triệu đối tượng Python. Dung lượng bộ nhớ thu được vẫn ở mức thoải mái dưới giới hạn 256 MB đã nêu, trong khi thuật toán tránh được mọi thao tác tùy thuộc theo cấp số nhân vào độ dài truy vấn. 

## Trường hợp thử nghiệm 

Dây nịt sau đây giả định`solve()`hoạt động từ giải pháp trên. Người trợ giúp`fraction_mod`tính toán trực tiếp các giá trị mong đợi nhỏ, trong khi trường hợp cuối cùng kiểm tra độ dài truy vấn tối đa được phép mà không liệt kê bất kỳ chuỗi nào.```python
import sys
import io

MOD = 1000000007

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_stdout = sys.stdout
    old_input = input

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = old_input

def fraction_mod(numerator, m):
    denominator = 26 * (pow(26, m, MOD) - 1) % MOD
    denominator = denominator * pow(25, MOD - 2, MOD) % MOD
    return numerator * pow(denominator, MOD - 2, MOD) % MOD

# Provided sample
sample = """\
2
zybnb
ybyb
3 5
4
1
2
3
4
"""

assert run(sample) == (
    "769230776\n"
    "425925929\n"
    "891125950\n"
    "633120399\n"
), "sample"

# Minimum-size input
case_min = """\
1
a
1
1
1
"""

assert run(case_min) == "576923081\n", "minimum case"

# Same string in three titles, all happiness values equal.
# The string a must contribute 2*2*2 = 8, not 2.
case_equal = """\
3
a
a
a
2 2 2
2
1
2
"""

assert run(case_equal) == (
    str(fraction_mod(8, 1)) + "\n" +
    str(fraction_mod(8, 2)) + "\n"
), "equal values and repeated titles"

# Boundary between exact substring lengths.
# a and b have contribution 2 each, while ab contributes 2.
case_boundary = """\
1
ab
2
3
1
2
3
"""

assert run(case_boundary) == (
    str(fraction_mod(4, 1)) + "\n" +
    str(fraction_mod(6, 2)) + "\n" +
    str(fraction_mod(6, 3)) + "\n"
), "substring-length boundary"

# Maximum permitted query length.
# The numerator is always 1, but the denominator contains 10^6 length levels.
case_max_query = """\
1
a
1
1
1000000
"""

expected_max = fraction_mod(1, 1000000)
assert run(case_max_query) == str(expected_max) + "\n", "maximum query length"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Việc cung cấp`zybnb`,`ybyb`mẫu |`769230776`,`425925929`,`891125950`,`633120399`| Hoàn thành trường hợp tham chiếu và các chuỗi con chồng chéo | 
| Một tiêu đề`a`, hạnh phúc 1, truy vấn 1 |`576923081`| Đầu vào có kích thước tối thiểu và ranh giới chuỗi gốc/không trống | 
| Ba tựa đề giống hệt nhau`a`, mọi hạnh phúc 2 |`307692310`,`652421657`| Phép nhân giữa các danh hiệu và giá trị hạnh phúc bằng nhau | 
| Một tiêu đề`ab`, hạnh phúc 2, truy vấn 1, 2, 3 |`307692310`,`239316241`,`6/18278 mod MOD`| Điểm cuối khoảng thời gian chính xác và các truy vấn ngoài tiêu đề | 
| Một tiêu đề`a`, hạnh phúc 1, truy vấn (10^6) |`S_1000000^{-1} mod MOD`| Độ dài truy vấn tối đa và thực tế là mẫu số tiếp tục tăng | 

## Vỏ cạnh 

Các lần xuất hiện lặp đi lặp lại bên trong một tiêu đề sẽ được xử lý bởi`seen`mảng. Vì```
1
aaa
2
1
```tiêu đề được xử lý từ trái sang phải và các bước đi liên kết hậu tố có thể gặp trạng thái đại diện`a`nhiều lần. Lần gặp đầu tiên nhân giá trị của nó với (2), trong khi những lần gặp tiếp theo sẽ thấy`seen[state] == 1`và dừng lại ở điểm đó. Như vậy`a`,`aa`, Và`aaa`mỗi người nhận được hạnh phúc (2), thay vì nhân (2) một lần cho mỗi lần xuất hiện. 

Sự khác biệt giữa tư cách thành viên tiêu đề và số lần xuất hiện cũng được xử lý chính xác cho```
2
a
a
2 3
1
1
```Tiêu đề đầu tiên đánh dấu trạng thái của`a`và thay đổi giá trị của nó từ (1) thành (2). Tiêu đề thứ hai có một thẻ khác nên nó đánh dấu lại trạng thái tương tự và thay đổi giá trị từ (2) thành (6). Tử số kết quả cho độ dài một là (6) và câu trả lời là (6/26=461538465). Số lần`a`xuất hiện bên trong một trong hai tiêu đề không bao giờ được đưa vào phép tính. 

Ranh giới khoảng thời gian liên kết hậu tố được xử lý bằng cách thêm giá trị của trạng thái tại`len[fa[v]] + 1`và loại bỏ nó tại`len[v] + 1`. Vì```
1
ab
2
1
2
```các trạng thái tương ứng với các chuỗi con một ký tự có độ dài bằng một, trong khi trạng thái biểu thị`ab`đóng góp ở độ dài hai. Tử số thu được là (4) và (6). Nếu phép trừ được đặt ở`len[v]`thay vì`len[v] + 1`, phần đóng góp có độ dài hai sẽ biến mất. 

Một truy vấn dài hơn mỗi tiêu đề sẽ có một ranh giới khác nhau. Vì```
1
a
1
1
2
```chuỗi hạnh phúc tích cực duy nhất là`a`, do đó tử số vẫn là (1) cho cả hai truy vấn. Tuy nhiên, đối với (m=2), có thể có (26+676=702) chuỗi, cho kết quả (1/702=206552708). Việc triển khai giữ tử số ở giá trị được tính toán cuối cùng trong khi tiếp tục mở rộng mẫu số qua mọi độ dài được truy vấn. 

Cuối cùng, trạng thái gốc không bao giờ được thêm vào mảng khác biệt. Chuỗi biểu diễn của nó là chuỗi trống, trong khi lựa chọn ngẫu nhiên chứa ít nhất một ký tự. Việc bao gồm cả gốc sẽ thêm phần đóng góp có độ dài bằng 0 hư cấu và thay đổi mọi câu trả lời.
