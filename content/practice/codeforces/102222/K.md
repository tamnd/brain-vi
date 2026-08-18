---
title: "CF 102222K - Vỏ Vertex"
description: "Chúng ta có một đồ thị đơn giản vô hướng có nhiều nhất 36 đỉnh. Mỗi đỉnh có trọng số nhân. Tập hợp các đỉnh được chọn là hợp lệ khi mỗi cạnh có ít nhất một điểm cuối trong tập hợp đó."
date: "2026-08-17T22:15:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102222
codeforces_index: "K"
codeforces_contest_name: "2018-2019 ACM-ICPC, China Multi-Provincial Collegiate Programming Contest"
rating: 0
weight: 102222
solve_time_s: 193
verified: true
draft: false
---

[CF 102222K - Vỏ Vertex](https://codeforces.com/problemset/problem/102222/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị đơn giản vô hướng có nhiều nhất 36 đỉnh. Mỗi đỉnh có trọng số nhân. Tập hợp các đỉnh được chọn là hợp lệ khi mỗi cạnh có ít nhất một điểm cuối trong tập hợp đó. Đối với mọi tập hợp hợp lệ (S), giá trị của nó là tích của trọng số của các đỉnh đã chọn, với tích rỗng bằng 1. Nhiệm vụ là cộng các tích này trên mọi bìa đỉnh hợp lệ và báo cáo kết quả theo modulo số nguyên tố đã cho (q). 

Đầu vào chứa một số biểu đồ độc lập. Đối với mỗi trường hợp thử nghiệm, (n) là số đỉnh, (m) là số cạnh và (q) là mô đun. Dòng tiếp theo chứa trọng số của đỉnh, tiếp theo là các cạnh. Câu trả lời cho mỗi trường hợp thử nghiệm được in cùng với số trường hợp của nó. 

Ràng buộc chính là (n\le 36). Một bảng liệt kê trực tiếp có (2^{36}=68,719,476,736) tập hợp con có thể có, vượt xa những gì một chương trình 10 giây có thể kiểm tra. Biểu đồ cũng có thể chứa các cạnh (\Theta(n^2)), do đó việc kiểm tra từng cạnh cho từng tập hợp con thậm chí còn tệ hơn. Sự đảm bảo rằng nhiều nhất 36 trường hợp thử nghiệm có (n>18) cho chúng ta biết rằng thuật toán hàm mũ được dự định, nhưng phần hàm mũ của nó phải phụ thuộc vào khoảng một nửa của (n), chứ không phải vào chính (n). 

Mô đun là số nguyên tố, nhưng giải pháp không cần phân chia mô đun. Cụ thể, trọng số của đỉnh có thể chính xác là (q), do đó trọng số của nó bằng 0 modulo (q). Bất kỳ cách tiếp cận nào phân chia một cách mù quáng theo trọng số đỉnh hoặc sử dụng nghịch đảo mô-đun sẽ cần xử lý đặc biệt bổ sung. Giải pháp bên dưới chỉ sử dụng phép nhân và phép cộng modulo (q), vì vậy trường hợp này hoạt động tự động. 

Đồ thị không có cạnh cũng là một nơi dễ mắc lỗi thầm lặng. Coi như```
1
3 0 998244353
2 3 5
```Mọi tập hợp con đều là một bìa đỉnh, bao gồm cả tập hợp con trống. Câu trả lời là 

[ 
(1+2)(1+3)(1+5)=72. 
] 

Việc triển khai bắt đầu chỉ liệt kê các bìa không trống sẽ bỏ lỡ phần đóng góp 1 từ tập trống. 

Một đỉnh không có cạnh nào có cùng điều kiện biên:```
1
1 0 998244353
5
```Có hai bìa, tập trống có giá trị 1 và tập chứa đỉnh có giá trị 5 nên đáp án là 6. 

Trọng số bằng mô đun sẽ đưa ra một trường hợp tinh tế khác:```
1
1 0 998244353
998244353
```Hai bìa có giá trị 1 và (998244353), trở thành 1 và 0 modulo (q). Câu trả lời đúng là 1. Một giải pháp dựa trên nghịch đảo mô-đun không thể đảo ngược trọng số này, trong khi giải pháp gặp nhau ở giữa không gặp phải vấn đề như vậy. 

Cuối cùng, đối với hai đỉnh được nối bởi một cạnh,```
1
2 1 998244353
2 3
1 2
```các bìa hợp lệ là ({1}), ({2}) và ({1,2}), với các giá trị 2, 3 và 6. Câu trả lời là 11. Điều này mắc phải lỗi phổ biến khi chỉ tính các bìa đỉnh tối thiểu, vì tập hợp đầy đủ cũng là một bìa đỉnh và đóng góp vào tổng. 

## Phương pháp tiếp cận 

Giải pháp vũ phu tuân theo định nghĩa trực tiếp. Liệt kê mọi tập hợp con (S) của (n) đỉnh, kiểm tra xem mọi cạnh có ít nhất một điểm cuối trong (S) hay không và nếu nó là một lớp phủ, hãy nhân trọng số của các đỉnh đã chọn của nó và cộng kết quả. 

Điều này đúng vì mỗi tập hợp con được kiểm tra chính xác một lần và phép kiểm tra chính xác là định nghĩa về một bìa đỉnh. Vấn đề là số lượng tập hợp con. Với (n=36), có (2^{36}=68,719,476,736) trong số chúng. Ngay cả khi việc kiểm tra một tập hợp con bằng cách nào đó được giảm bớt thành một số thao tác máy, thì việc này vẫn quá chậm. Việc kiểm tra từng cạnh đơn giản có thể yêu cầu tới (2^{36}\cdot 630), nhiều hơn (4\times10^{13}) kiểm tra cạnh. 

Quan sát hữu ích là lớp phủ đỉnh chính xác là phần bù của một tập độc lập. Tuy nhiên, chúng ta thực sự không cần chuyển đổi biểu thức có trọng số thành biểu thức tập hợp độc lập. Làm như vậy về mặt đại số sẽ gợi ý việc chia cho trọng số đỉnh, điều này không an toàn khi trọng số bằng 0 modulo (q). Thay vào đó, chúng tôi chia các đỉnh thành hai phần và giữ nguyên trọng lượng bìa ban đầu. 

Đặt cạnh trái chứa các đỉnh (L) và cạnh phải chứa các đỉnh (R). Hãy xem xét việc sửa các đỉnh đã chọn ở bên trái. Các cạnh hoàn toàn bên trong bên trái có thể được kiểm tra ngay lập tức. Mỗi cạnh chéo có điểm cuối bên trái không được chọn sẽ buộc điểm cuối bên phải của nó phải được chọn. Do đó, lựa chọn bên trái sẽ xác định một mặt nạ có tên`need`, chứa tất cả các đỉnh bên phải bắt buộc. 

Những gì còn lại là một truy vấn bên phải: trong số tất cả các bìa đỉnh bên phải, tổng trọng lượng của các bìa chứa mọi đỉnh trong đó là bao nhiêu?`need`? Chúng ta có thể trả lời tất cả các truy vấn như vậy cùng một lúc bằng phép biến đổi zeta tập hợp con. Ban đầu, một mảng lưu trữ phần đóng góp của từng bìa bên phải chính xác. Sau khi biến đổi,`sum[need]`chứa tổng trên mỗi bìa bên phải là tập hợp con của`need`. 

Lực lượng vũ phu hoạt động vì mọi vỏ bọc có thể có thể được kiểm tra độc lập, nhưng không thành công vì có (2^n) lựa chọn. Nhận xét rằng việc sửa một nửa chỉ áp đặt yêu cầu tập hợp con cho nửa còn lại cho phép chúng ta thay thế bảng liệt kê đầy đủ bằng hai bảng liệt kê có giá trị khoảng (2^{n/2}), theo sau là một phép biến đổi tập hợp con. 

Để triển khai, chúng tôi sử dụng thử nghiệm tập hợp độc lập tương đương để xác định xem nửa mặt nạ có phải là tấm che hợp lệ hay không. Nếu như`cover`là mặt nạ được chọn, sau đó`full ^ cover`phải không chứa cạnh bên trong. Mặt nạ độc lập có thể được nhận dạng trong (O(2^k)) bằng cách sử dụng phép lặp một bit. 

Chúng tôi chọn cách phân chia hơi bất đối xứng khi hữu ích. Phía bên phải là phía mà phép biến đổi zeta tập hợp con chạy, vì vậy kích thước của nó phải được chọn để giảm thiểu (R2^R+2^{n-R}). Đối với (n=36), điều này mang lại (R=16) và (L=20), thay vì cách chia 18 và 18 thông thường. Điều này làm giảm lượng công việc trong Python trong khi vẫn giữ nguyên độ phức tạp tiệm cận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^n·m)) | (O(n)) | Quá chậm | 
| Gặp Ở Giữa + SOS DP | (O(2^L + R2^R)), (L+R=n) | (O(2^L+2^R)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia các đỉnh thành phần bên trái có kích thước (L) và phần bên phải có kích thước (R). Việc triển khai chọn cách phân chia giảm thiểu (R2^R+2^{n-R}), do đó phía biến đổi tập hợp con đắt tiền được giữ ở mức nhỏ. 
2. Lưu trữ ba loại mặt nạ bit. Đối với mỗi đỉnh bên trái, lưu trữ các đỉnh lân cận của nó bên trong phần bên trái và các đỉnh lân cận của nó bên trong phần bên phải. Đối với mỗi đỉnh bên phải, hãy lưu trữ các đỉnh lân cận của nó vào phần bên phải. Mặt nạ bit thực hiện tất cả các lần kiểm tra lân cận theo thời gian liên tục ở quy mô của các nửa nhỏ này. 
3. Liệt kê mọi tập con của các đỉnh bên phải và tính xem phần bù của nó có phải là tập độc lập hay không. Nếu phần bù là độc lập thì bản thân tập con đó là bìa đỉnh bên phải hợp lệ. Đồng thời, tính tích của các trọng số bên phải đã chọn. 
4. Đặt sản phẩm của mọi bìa bên phải chính xác hợp lệ vào`sum[mask]`. Mặt nạ không hợp lệ sẽ không nhận được. Mảng này ban đầu mô tả chính xác các bìa, vì vậy`sum[mask]`có nghĩa là chỉ có bìa bằng`mask`. 
5. Áp dụng phép biến đổi superset subset-zeta cho`sum`. Sau khi xử lý từng bit bên phải,`sum[mask]`trở thành tổng trọng số của tất cả các bìa bên phải hợp lệ chứa`mask`. Phép biến đổi chính xác là những gì chúng ta cần vì nửa bên trái chỉ cho chúng ta biết đỉnh bên phải nào là bắt buộc. 
6. Tính toán trước`need[mask]`cho mọi mặt nạ bên trái.`need[mask]`là hợp của các cạnh bên phải của mỗi đỉnh bên trái không được chọn trong`mask`. Các đỉnh bên phải như vậy phải được chọn, nếu không cạnh chéo của chúng sẽ không có điểm cuối trên bìa. 
7. Liệt kê từng mặt nạ che bên trái. Hiệu lực bên trong của nó được kiểm tra bằng cách xem xét phần bù của mặt nạ và kiểm tra xem phần bù đó có phải là một tập độc lập hay không. Tích đỉnh được chọn của nó được tính toán độc lập. 
8. Để có mặt nạ bên trái hợp lệ, hãy nhân tích trọng lượng của nó với`sum[need[mask]]`. Cái sau tính tổng chính xác tất cả các bìa bên phải đáp ứng các yêu cầu về cạnh chéo. Thêm giá trị này vào câu trả lời modulo (q). 
9. In đáp án tích lũy cho bộ đề thi. 

Bất biến đằng sau bước kết hợp là mỗi đỉnh hoàn chỉnh có đúng một mặt nạ bên trái. Sau khi mặt nạ bên trái đó được cố định, tất cả các cạnh bên trong bên trái sẽ bị che đi hoặc mặt nạ sẽ bị loại bỏ. Mỗi cạnh chéo có điểm cuối bên trái không được chọn sẽ buộc điểm cuối bên phải của nó vào trang bìa và`need`ghi lại chính xác các đỉnh bắt buộc đó. Mảng bên phải được biến đổi zeta sau đó tính tổng chính xác tất cả các mặt nạ bên phải hợp lệ chứa các đỉnh bắt buộc đó. Do đó, mỗi bìa hoàn chỉnh sẽ đóng góp một lần và không có bìa nào không hợp lệ đóng góp. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve_case(n, m, q, weights, edges):
    # Choose R to minimize the dominant subset work:
    # R * 2^R for the SOS transform plus 2^(n-R) for the other half.
    best_r = 0
    best_cost = 1 << (n + 1)

    for r in range(n + 1):
        cost = r * (1 << r) + (1 << (n - r))
        if cost < best_cost:
            best_cost = cost
            best_r = r

    R = best_r
    L = n - R

    adj_l = [0] * L
    adj_r = [0] * R
    cross = [0] * L

    for u, v in edges:
        if u < L and v < L:
            adj_l[u] |= 1 << v
            adj_l[v] |= 1 << u
        elif u >= L and v >= L:
            u -= L
            v -= L
            adj_r[u] |= 1 << v
            adj_r[v] |= 1 << u
        else:
            if u < L:
                cross[u] |= 1 << (v - L)
            else:
                cross[v] |= 1 << (u - L)

    wl = [x % q for x in weights[:L]]
    wr = [x % q for x in weights[L:]]

    # Compute independent-set flags and cover products for the right half.
    nr = 1 << R
    full_r = nr - 1

    ind_r = bytearray(nr)
    ind_r[0] = 1

    prod_r = [0] * nr
    prod_r[0] = 1

    for mask in range(1, nr):
        bit = mask & -mask
        v = bit.bit_length() - 1
        rest = mask ^ bit

        prod_r[mask] = prod_r[rest] * wr[v] % q

        if ind_r[rest] and (adj_r[v] & rest) == 0:
            ind_r[mask] = 1

    # sum[cover] is the exact contribution of this right-side cover.
    # A cover is valid exactly when its complement is independent.
    sum_r = [0] * nr

    for cover in range(nr):
        if ind_r[full_r ^ cover]:
            sum_r[cover] = prod_r[cover]

    # Superset zeta transform.
    for bit_index in range(R):
        bit = 1 << bit_index
        step = bit << 1

        for base in range(0, nr, step):
            end = base + bit
            other = end

            for mask in range(base, end):
                x = sum_r[mask] + sum_r[other]
                if x >= q:
                    x -= q
                sum_r[mask] = x
                other += 1

    # need[mask] = right vertices forced by unselected left vertices.
    nl = 1 << L
    need = array('I', [0]) * nl

    # Compute from supersets. For a mask that is not full, choose a zero bit v.
    # need[mask] = need[mask | bit] | cross[v].
    for mask in range(nl - 2, -1, -1):
        zero_bit = (~mask) & (mask + 1)
        v = zero_bit.bit_length() - 1
        need[mask] = need[mask | zero_bit] | cross[v]

    # Compute independent-set flags and selected-weight products on the left.
    ind_l = bytearray(nl)
    ind_l[0] = 1

    prod_l = [0] * nl
    prod_l[0] = 1

    for mask in range(1, nl):
        bit = mask & -mask
        v = bit.bit_length() - 1
        rest = mask ^ bit

        prod_l[mask] = prod_l[rest] * wl[v] % q

        if ind_l[rest] and (adj_l[v] & rest) == 0:
            ind_l[mask] = 1

    full_l = nl - 1
    ans = 0

    for cover in range(nl):
        if ind_l[full_l ^ cover]:
            ans += prod_l[cover] * sum_r[need[cover]] % q
            if ans >= q:
                ans -= q

    return ans

def solve_data(data):
    tokens = list(map(int, data.split()))
    p = 0
    t = tokens[p]
    p += 1

    out = []

    for case_id in range(1, t + 1):
        n = tokens[p]
        m = tokens[p + 1]
        q = tokens[p + 2]
        p += 3

        weights = tokens[p:p + n]
        p += n

        edges = []
        for _ in range(m):
            u = tokens[p] - 1
            v = tokens[p + 1] - 1
            p += 2
            edges.append((u, v))

        ans = solve_case(n, m, q, weights, edges)
        out.append(f"Case #{case_id}: {ans}")

    return "\n".join(out)

def main():
    data = sys.stdin.buffer.read()
    sys.stdout.write(solve_data(data))

if __name__ == "__main__":
    main()
```Phần đầu tiên của`solve_case`chọn cách chia. Lựa chọn thông thường sẽ là (L=\lceil n/2\rceil) và (R=\lfloor n/2\rfloor), nhưng Python được hưởng lợi từ việc làm cho phía SOS nhỏ hơn một chút. Vòng lặp nhỏ trên tất cả các giá trị (R) có thể tìm thấy sự phân chia tốt nhất mà không ảnh hưởng đến chính thuật toán. 

Ba mảng kề nhau mã hóa biểu đồ ở dạng mà hai nửa cần.`adj_l[v]`Và`adj_r[v]`là mặt nạ kề địa phương.`cross[v]`chứa các đỉnh phải kề với đỉnh trái (v). Sự khác biệt giữa các mặt nạ này ngăn không cho các cạnh chéo vô tình bị coi là các cạnh bên trong. 

Các mảng được thiết lập độc lập là cách rõ ràng nhất để xác thực trang bìa. Đối với mặt nạ chứa đỉnh`v`, loại bỏ bit được đặt thấp nhất để có được`rest`. Mặt nạ lớn hơn độc lập chính xác khi`rest`là độc lập và`v`không có hàng xóm ở`rest`. Vì bìa và phần bù của nó tương đương với một bộ độc lập nên việc kiểm tra`ind[full ^ cover]`đưa ra tính hợp lệ của một bìa mà không cần kiểm tra mọi cạnh. 

Các mảng sản phẩm được tính toán bằng cách sử dụng cùng một phép lặp bit thấp nhất. Nếu như`mask = rest | {v}`, thì sản phẩm cho`mask`là sản phẩm dành cho`rest`nhân với trọng lượng của`v`. Mỗi phép nhân đều được rút gọn theo modulo (q), vì vậy Python không bao giờ tạo ra các tích cực kỳ chính xác. 

Bên phải`sum_r`ban đầu chứa những đóng góp cho các trang bìa chính xác. Phép biến đổi zeta thay đổi ý nghĩa của nó từ bình đẳng chính xác sang ngăn chặn. Đối với mỗi bit, mặt nạ không có bit đó sẽ nhận được sự đóng góp của mặt nạ tương ứng với tập hợp bit đó. Sau khi tất cả các bit được xử lý,`sum_r[need]`chứa mọi bìa hợp lệ có chứa`need`. 

các`need`mảng đáng được chú ý đặc biệt vì nó phụ thuộc vào các đỉnh bên trái không được chọn, không phải các đỉnh được chọn. Đối với mỗi mặt nạ, chọn một đỉnh vắng mặt nó. Nếu đỉnh đó được chọn, chúng ta sẽ có mặt nạ lớn hơn`mask | bit`. Đỉnh không được chọn đóng góp toàn bộ mặt nạ lân cận chéo của nó, tạo ra sự tái diễn 

[ 
need[mask]=need[mask\cup{v}]\mathbin{|}cross[v]. 
] 

Vòng lặp chạy ngược lại nên mặt nạ lớn hơn đã được tính toán. 

Vòng lặp cuối cùng chỉ chấp nhận các bìa trái có phần bù độc lập. Đối với mỗi trang bìa như vậy,`prod_l[cover]`là trọng lượng chính xác của nó, trong khi`sum_r[need[cover]]`chiếm mọi bìa bên phải tương thích. Sản phẩm của họ chính xác là sự đóng góp tổng thể của tất cả các trang bìa hoàn chỉnh mở rộng sự lựa chọn còn lại đó. 

Không có vấn đề tràn số nguyên trong Python. Trong ngôn ngữ có số nguyên có chiều rộng cố định, mọi sản phẩm phải được đánh giá ở loại đủ rộng trước khi lấy mô-đun. Mặt nạ bit cũng vừa vặn thoải mái bên trong một số nguyên bình thường vì có tối đa 36 đỉnh. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đồ thị là đường đi (1-2-3-4) và mỗi trọng số là 1. Với phép chia hai và hai tự nhiên, các đỉnh bên trái là 1 và 2 và các đỉnh bên phải là 3 và 4. 

Đối với nửa bên phải, bìa hợp lệ chính xác là`01`,`10`, Và`11`. Cạnh bên phải nằm giữa đỉnh 3 và 4 nên bìa bên phải trống không hợp lệ. 

| Mặt nạ bên phải | Đóng góp chính xác | Sau khi biến đổi superset | 
| --- | --- | --- | 
|`00`| 0 | 3 | 
|`01`| 1 | 2 | 
|`10`| 1 | 2 | 
|`11`| 1 | 1 | 

Cạnh trái nằm giữa đỉnh 1 và 2. Các bìa trái hợp lệ là`01`,`10`, Và`11`. Vì`01`, đỉnh 2 không được chọn và cạnh chéo của nó tới đỉnh 3 buộc bit phải`01`. Đối với hai bìa bên trái còn lại, không có đỉnh bên phải nào bị ép buộc. 

| Mặt nạ bên trái | Bìa hợp lệ |`need`| Sản phẩm còn lại | Tổng đúng | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
|`00`| Không |`11`| 1 | 1 | 0 | 
|`01`| Có |`01`| 1 | 2 | 2 | 
|`10`| Có |`00`| 1 | 3 | 3 | 
|`11`| Có |`00`| 1 | 3 | 3 | 

Tổng số là (2+3+3=8), trùng khớp`Case #1: 8`. Dấu vết thể hiện tính bất biến trung tâm: một khi bìa bên trái được cố định, các cạnh chéo chỉ trở thành các ràng buộc đỉnh bắt buộc ở bên phải. 

### Mẫu 2 

Đồ thị là (K_4), lại có tất cả các trọng số bằng 1. Một bìa đỉnh của một đồ thị hoàn chỉnh phải chứa ít nhất ba đỉnh. Với cùng một cách chia hai và hai, nửa bên phải có một cạnh trong và mọi đỉnh bên trái được nối với cả hai đỉnh bên phải. 

Các giá trị được chuyển đổi bên phải một lần nữa 

| Mặt nạ bên phải | Đóng góp chính xác | Sau khi biến đổi superset | 
| --- | --- | --- | 
|`00`| 0 | 3 | 
|`01`| 1 | 2 | 
|`10`| 1 | 2 | 
|`11`| 1 | 1 | 

Các bìa bên trái hợp lệ là`01`,`10`, Và`11`. Nếu chính xác một đỉnh bên trái được chọn thì đỉnh bên trái còn lại không được chọn và buộc cả hai đỉnh phải, cho`need = 11`. Nếu cả hai đỉnh bên trái được chọn,`need = 00`. 

| Mặt nạ bên trái | Bìa hợp lệ |`need`| Sản phẩm còn lại | Tổng đúng | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
|`00`| Không |`11`| 1 | 1 | 0 | 
|`01`| Có |`11`| 1 | 1 | 1 | 
|`10`| Có |`11`| 1 | 1 | 1 | 
|`11`| Có |`00`| 1 | 3 | 3 | 

Câu trả lời là (1+1+3=5). Ba đóng góp tương ứng chính xác với bìa bốn đỉnh có kích thước ba và bốn, với bốn bìa có kích thước ba và một bìa có kích thước bốn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(2^L + R2^R + m)) | Chi phí DP nửa mặt nạ (O(2^L+2^R)), chi phí biến đổi siêu tập hợp (O(R2^R)) và chi phí đọc/xây dựng biểu đồ (O(m)). | 
| Không gian | (O(2^L+2^R+n+m)) | Bộ nhớ chiếm ưu thế bao gồm các mảng tập hợp con cho hai nửa. | 

Ở đây (L+R=n) và quá trình triển khai chọn (R) để giảm thiểu biểu thức thực tế (R2^R+2^{n-R}). Đối với (n=36), phần phân chia được chọn là 20 và 16, do đó không gian tập hợp con lớn nhất là khoảng (2^{20}) và phép biến đổi chạm vào khoảng (16\cdot2^{16}) trạng thái. Điều này nhỏ hơn đáng kể so với việc liệt kê (2^{36}) tập hợp con của biểu đồ đầy đủ và phù hợp với quy mô cấp số nhân dự định của bài toán. 

Việc đảm bảo rằng chỉ có tối đa 36 trường hợp thử nghiệm có (n>18) đặc biệt hữu ích vì đó là những trường hợp chịu trách nhiệm về mảng tập hợp con lớn. Các trường hợp thử nghiệm nhỏ hơn có không gian trạng thái nhỏ hơn đáng kể. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm bên dưới giả định giải pháp biên tập được lưu dưới dạng`solution.py`, Ở đâu`solve_data`là hàm được xác định ở trên.```
import io
from solution import solve_data

def run(inp: str) -> str:
    return solve_data(inp).strip()

# Provided samples.
sample = """\
2
4 3 998244353
1 1 1 1
1 2
2 3
3 4
4 6 998244353
1 1 1 1
1 2
1 3
1 4
2 3
2 4
3 4
"""

assert run(sample) == """\
Case #1: 8
Case #2: 5
""", "provided samples"

# Minimum-size graph: one isolated vertex.
assert run("""\
1
1 0 998244353
5
""") == "Case #1: 6", "empty graph and empty cover"

# A weight equal to q becomes zero modulo q.
assert run("""\
1
1 0 998244353
998244353
""") == "Case #1: 1", "weight equal to modulus"

# Smallest graph with an edge, including the full cover.
assert run("""\
1
2 1 998244353
2 3
1 2
""") == "Case #1: 11", "single edge"

# Maximum n, all equal weights, no edges.
# Every subset is a cover, so the answer is 2^36 modulo 998244353.
assert run("""\
1
36 0 998244353
1 1 1 1 1 1 1 1 1 1 1 1
1 1 1 1 1 1 1 1 1 1 1 1
1 1 1 1 1 1 1 1 1 1 1 1
""") == "Case #1: 838860732", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 998244353 / 5`|`Case #1: 6`| Kích thước tối thiểu, biểu đồ trống, bìa trống | 
|`1 0 998244353 / 998244353`|`Case #1: 1`| Trọng lượng bằng mô đun | 
|`2 1 998244353 / 2 3 / 1 2`|`Case #1: 11`| Hạn chế cạnh và bao gồm toàn bộ trang bìa | 
|`36 0 998244353 / 36 ones`|`Case #1: 838860732`| Số đỉnh tối đa, tất cả trọng số bằng nhau, ranh giới trạng thái hàm mũ | 

## Vỏ cạnh 

Đối với một biểu đồ trống, mọi tập hợp con đều là một bìa đỉnh vì không có cạnh nào cần được che phủ. Đối với đầu vào```
1
1 0 998244353
5
```phía bên phải của phần phân chia trống. Mặt nạ bên phải duy nhất có đóng góp 1. Hai mặt nạ bên trái biểu thị bìa trống và đỉnh được chọn, với tích 1 và 5. Cả hai phần bù đều độc lập vì đồ thị không có cạnh, vì vậy câu trả lời là (1+5=6). 

Khi trọng số của đỉnh bằng mô đun, đóng góp của nó bằng 0 modulo (q), nhưng đỉnh vẫn là một đỉnh của đồ thị thông thường. Vì```
1
1 0 998244353
998244353
```bìa trống đóng góp 1 và bìa được chọn đóng góp 0 modulo (q). Các tích của tập hợp con được tính toán bằng phép nhân mô-đun thông thường, do đó không cần phải nghịch đảo. Kết quả là 1. 

Đối với trường hợp một cạnh```
1
2 1 998244353
2 3
1 2
```mặt nạ hợp lệ là`01`,`10`, Và`11`. Tích của họ là 2, 3 và 6, cho kết quả là 11. Trong chế độ xem gặp nhau ở giữa, bất kỳ điểm cuối nào được đặt ở bên trái, điểm cuối bên trái không được chọn sẽ buộc điểm cuối bên phải được chọn. Truy vấn zeta bao gồm bìa bên phải bắt buộc cũng như bất kỳ bìa hợp lệ lớn hơn nào, do đó, bìa đầy đủ sẽ được tính thay vì bị nhầm với bìa tối thiểu. 

Biểu đồ trống có kích thước tối đa```
1
36 0 998244353
1 1 1 1 1 1 1 1 1 1 1 1
1 1 1 1 1 1 1 1 1 1 1 1
1 1 1 1 1 1 1 1 1 1 1 1
```có tất cả (2^{36}) tập hợp con làm bìa đỉnh, vì không có cạnh. Mọi tập hợp con đều có tích 1, vì vậy câu trả lời là (2^{36}\bmod998244353=838860732). Thuật toán không bao giờ xây dựng các tập hợp con hoàn chỉnh (2^{36}). Nó xử lý hai nửa một cách độc lập và kết hợp chúng thông qua phép biến đổi tập hợp con, đó chính xác là lý do tại sao trường hợp tối đa vẫn khả thi.
