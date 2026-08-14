---
title: "CF 102331B - Bitwise Xor"
description: "Chúng tôi có một mảng lên tới (300000) số nguyên, mỗi số sử dụng tối đa 60 bit và một ngưỡng (x). Một chuỗi con được coi là tốt khi mỗi cặp phần tử mảng được chọn có ít nhất XOR (x)."
date: "2026-08-14T01:11:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102331
codeforces_index: "B"
codeforces_contest_name: "2019 Summer Petrozavodsk Camp, Day 2: 300iq Contest 2 (XX Open Cup, Grand Prix of Kazan)"
rating: 0
weight: 102331
solve_time_s: 367
verified: true
draft: false
---

[CF 102331B - Bitwise Xor](https://codeforces.com/problemset/problem/102331/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 7 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng lên tới (300000) số nguyên, mỗi số sử dụng tối đa 60 bit và một ngưỡng (x). Một chuỗi con được coi là tốt khi mỗi cặp phần tử mảng được chọn có ít nhất XOR (x). Nhiệm vụ là đếm tất cả các dãy con tốt không trống, với kết quả lấy theo modulo (998244353). Sự cố ban đầu sử dụng giới hạn thời gian 2 giây và giới hạn bộ nhớ 1 GiB. 

Thứ tự ban đầu của mảng chỉ xác định chỉ số nào tạo thành một dãy con. Bản thân điều kiện chỉ phụ thuộc vào các giá trị được chọn. Điều này có nghĩa là trước tiên chúng ta có thể sắp xếp các giá trị và đếm các lựa chọn hợp lệ theo thứ tự được sắp xếp đó. Việc sắp xếp không làm mất bất kỳ chuỗi con nào: các giá trị bằng nhau vẫn là các phần tử riêng biệt và sau khi sắp xếp từng tập hợp con của các vị trí vẫn tương ứng với chính xác một tập hợp con của các vị trí ban đầu. 

Giới hạn (n\le300000) ngay lập tức loại trừ mọi số bậc hai trong (n), vì (n^2) bằng khoảng (9\cdot10^{10}). Giới hạn 60 bit hữu ích hơn nhiều: các thao tác kiểm tra từng bit một có thể thực hiện khoảng 60 công việc cho mỗi phần tử, đưa ra thuật toán (O(60n)). Giới hạn bộ nhớ lớn cũng làm cho trie trở nên tự nhiên trong C++, nhưng trie 60 cấp đơn giản có thể chứa hàng chục triệu nút, điều này gây tốn kém một cách không cần thiết trong Python. Việc triển khai bên dưới sử dụng trie nhị phân nén, chỉ giữ các nút phân nhánh, do đó kích thước của nó là tuyến tính theo số lượng giá trị riêng biệt. 

Có một số trường hợp nguy hiểm có thể âm thầm phá vỡ việc triển khai bất cẩn. Khi (x=0), mọi XOR đều không âm, do đó mọi dãy con không trống đều hợp lệ. Ví dụ, đầu vào`3 0`với các giá trị`0 1 2`có đáp án (2^3-1=7). Một giải pháp coi đẳng thức là không hợp lệ sẽ loại trừ không chính xác các cặp có XOR bằng 0. 

Khi xuất hiện các giá trị bằng nhau và (x>0), hai bản sao của cùng một giá trị không thể được chọn cùng nhau vì XOR của chúng bằng 0. Ví dụ,`3 1`với các giá trị`5 5 5`có câu trả lời`3`, vì chỉ có ba chuỗi con đơn lẻ hoạt động. Một giải pháp bất cẩn loại bỏ mảng trùng lặp sẽ nhận được`1`, vì ba vị trí bằng nhau vẫn là ba lựa chọn dãy con khác nhau. 

Biên (a_i\oplus a_j=x) hợp lệ vì điều kiện lớn hơn hoặc bằng (x). Ví dụ,`2 2`với các giá trị`0 2`có câu trả lời`3`: cả đơn và cặp đều hợp lệ vì (0\oplus2=2). Một triển khai sử dụng`>`thay vì`>=`sẽ trở lại`2`. 

Cuối cùng, việc sắp xếp là cần thiết cho việc rút gọn, nhưng các vị trí đã sắp xếp không phải là chỉ số dãy con ban đầu. Ví dụ,`3 2`với các giá trị`2 0 1`vẫn có câu trả lời`5`. Giải pháp giả sử mảng ban đầu đã được sắp xếp sẽ bỏ lỡ các chuyển tiếp hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là liệt kê mọi dãy con không trống và kiểm tra tất cả các cặp bên trong nó. Đối với một tập hợp con có kích thước (k), việc này cần (\binom{k}{2}) so sánh XOR. Tổng tất cả các tập con, số kiểm tra cặp trong trường hợp xấu nhất là 

\binom n2 2^{n-2}. 
] 

Với (n=300000), điều này vượt quá mọi giới hạn thực tế. Ngay cả việc liệt kê các tập hợp con mà không kiểm tra tất cả các cặp cũng đã yêu cầu các thao tác (2^n). 

Quan sát hữu ích đầu tiên đến từ việc sắp xếp các giá trị. Giả sử (a\le b\le c). Nhìn vào bit cao nhất nơi ba số không bằng nhau. Chỉ có hai khả năng. Hoặc (a) khác với cả (b) và (c) ở bit đó hoặc (c) khác với cả (a) và (b). Trong trường hợp đầu tiên, (a\oplus b) và (a\oplus c) có bit cao được đặt trong khi (b\oplus c) thì không, vì vậy (b\oplus c) nhỏ hơn cả hai. Trong trường hợp thứ hai, (a\oplus b) nhỏ hơn cả hai XOR khác. Do đó, 

[ 
\min(a\oplus b,b\oplus c)\le a\oplus c. 
] 

Tính chất này có một hệ quả mạnh mẽ. Lấy bất kỳ giá trị đã chọn nào theo thứ tự được sắp xếp, 

[ 
b_1\le b_2\le\cdots\le b_k. 
] 

Nếu mỗi cặp giá trị được chọn liên tiếp có ít nhất XOR (x), thì mọi cặp giá trị không liên tiếp cũng có ít nhất XOR (x). Đối với ba giá trị được chọn liên tiếp, bất đẳng thức trên cho biết XOR của cặp ngoài ít nhất là XOR nhỏ hơn của hai cặp lân cận. Áp dụng cùng một đối số nhiều lần sẽ mở rộng điều này đến các khoảng cách tùy ý. 

Vì vậy, sau khi sắp xếp, điều kiện tất cả các cặp ban đầu trở thành điều kiện cục bộ: một dãy con hợp lệ chính xác khi cứ hai giá trị được chọn liên tiếp có ít nhất XOR (x). 

Điều này biến bài toán đếm thành một chương trình động. Đặt (f_i) là số dãy con hợp lệ có phần tử được chọn cuối cùng là giá trị thứ (i) trong mảng được sắp xếp. Phần tử cuối cùng có thể đứng một mình, tạo ra một dãy con. Mặt khác, chúng ta nối thêm (a_i) vào bất kỳ dãy con hợp lệ nào kết thúc tại một số (j<i) với 

[ 
a_j\oplus a_i\ge x. 
] 

Như vậy 

1+ 
\sum_{\substack{j<i\a_j\oplus a_i\ge x}}f_j. 
] 

Vấn đề còn lại là tính toán ngưỡng truy vấn XOR có trọng số này một cách hiệu quả. 

Trie nhị phân tiêu chuẩn giải quyết truy vấn trong (O(60)). Tại mỗi bit, chúng ta so sánh bit XOR với bit tương ứng của (x). Nếu bit tương ứng của (x) bằng 0, việc lấy bit XOR bằng 1 sẽ làm cho toàn bộ XOR lớn hơn bất kể các bit thấp hơn, do đó toàn bộ cây con trie có thể được thêm vào ngay lập tức. Nếu bit của (x) là một thì bit XOR cũng phải là một để giữ nguyên bằng tiền tố của (x), do đó chỉ một nhánh có thể tiếp tục. 

Trie thông thường có tới (60n) nút. Điều đó có thể chấp nhận được trong ngôn ngữ cấp thấp, nhưng nó thể hiện kém trong Python. Vì tất cả các giá trị đều được biết trước khi DP bắt đầu, chúng ta có thể xây dựng một trie nhị phân nén chỉ chứa các điểm phân nhánh. Một trie nén có các nút (O(n)), trong khi một truy vấn vẫn tuân theo tối đa 60 cấp độ phân nhánh. 

Kết quả so sánh là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2 2^n)) | (O(n)) | Quá chậm | 
| Trie nhị phân chuẩn DP | (O(n\log A)) | (O(n\log A)) | Được chấp nhận ở các ngôn ngữ cấp thấp | 
| Nén nhị phân trie DP | (O(n\log A)) | (O(n)) | Đã chấp nhận | 

Ở đây (\log A\le60). 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự không giảm. Bây giờ, điều kiện tất cả các cặp ban đầu có thể được thay thế bằng cách chỉ kiểm tra các giá trị được chọn liên tiếp, vì đối với (a\le b\le c), 

[ 
\min(a\oplus b,b\oplus c)\le a\oplus c. 
] 

Do đó, nếu các cặp được chọn liên tiếp đều thỏa mãn ngưỡng thì mọi cặp khác cũng vậy.

1. Nhóm các giá trị bằng nhau vào một lá của bộ ba nhị phân nén. DP vẫn xử lý các lần xuất hiện bằng nhau một cách riêng biệt, vì vậy đây chỉ là nén cấu trúc của không gian giá trị chứ không phải loại bỏ các phần tử mảng.
 2. Xây dựng trie nén theo cách đệ quy. Đối với một tập hợp có ít nhất hai giá trị riêng biệt, hãy tìm bit cao nhất mà giá trị nhỏ nhất và lớn nhất khác nhau. Bit đó tách tập hợp thành nhóm 0 và nhóm một, cả hai đều liền kề nhau vì các giá trị được sắp xếp. Xây dựng đệ quy hai nhóm. 
3. Cung cấp cho mỗi nút trie một tổng cây con. Tổng này đại diện cho tổng số (f_j) của các phần tử mảng đã được xử lý có giá trị thuộc về cây con đó. Ban đầu mọi tổng đều bằng 0. 
4. Xử lý mảng đã sắp xếp từ trái sang phải. Đối với giá trị hiện tại (v), hãy truy vấn tri để biết tổng trọng số DP của các giá trị trước đó (y) thỏa mãn 

[ 
v\oplus y\ge x. 
] 

Gọi tổng trả về (q). Sau đó 

[ 
f=1+q. 
] 

các`1`đại diện cho chuỗi con đơn chỉ chứa vị trí hiện tại. 

1. Thêm (f) vào câu trả lời chung và chèn trọng số DP này vào lá biểu thị (v). Trọng số cũng được truyền qua mọi tổ tiên để các truy vấn trong tương lai thấy được tổng cây con chính xác. 
2. Để thực hiện truy vấn nén-trie, hãy giữ nguyên so sánh với (x). Tại một nút bên trong, bit phân nhánh của nó là bit tiếp theo có thể khác nhau giữa các giá trị ở hai nút con của nó. Trước khi sử dụng bit đó, các bit bị bỏ qua khi nén sẽ được cố định cho toàn bộ cây con. So sánh các bit cố định đó với (x). Nếu tiền tố XOR cố định đã lớn hơn (x), toàn bộ cây con có thể được thêm vào. Nếu nó nhỏ hơn thì toàn bộ cây con có thể bị loại bỏ. 
3. Nếu tiền tố cố định bằng nhau, hãy kiểm tra bit phân nhánh. Khi bit tương ứng của (x) bằng 0, bit con tạo ra bit XOR một hoàn toàn hợp lệ và có thể được thêm vào, trong khi bit con tạo ra bit 0 XOR tiếp tục con đường đẳng thức. Khi bit tương ứng của (x) là một, chỉ con tạo ra bit XOR mới có thể tiếp tục. 
4. Tại một lá, tất cả các bit đều cố định, vì vậy hãy kiểm tra trực tiếp xem giá trị XOR của nó so với giá trị hiện tại có nhỏ nhất là (x) hay không. Tổng được lưu trữ của lá sau đó được bao gồm hoặc bỏ qua. 
5. Lấy mọi giá trị DP theo modulo (998244353). Số nguyên Python không bị tràn, nhưng việc giảm sau khi cộng sẽ giữ cho tổng của cây con được lưu trữ ở mức nhỏ và khớp với mô-đun đầu ra được yêu cầu. 

Tại sao nó hoạt động: sau khi sắp xếp, bất biến chính là một phần chuỗi được tính bởi (f_i) hợp lệ chính xác khi các giá trị được chọn liên tiếp của nó thỏa mãn ngưỡng XOR. Quá trình chuyển đổi xem xét mọi điểm cuối có thể có trước đó thỏa mãn ngưỡng đó, vì vậy mọi chuỗi con hợp lệ kết thúc tại (i) đều được tính chính xác một lần. Trie nén không thay đổi tập hợp các giá trị được lưu trữ. Nó chỉ bỏ qua các vị trí bit trong đó mọi giá trị trong cây con có cùng một bit và những bit bị bỏ qua đó có thể được so sánh với (x) dưới dạng tiền tố cố định. Do đó, mọi cây con được truy vấn thêm vào đều bao gồm toàn bộ các cây trước đó hợp lệ, trong khi mọi cây con bị loại bỏ đều bao gồm toàn bộ các cây trước đó không hợp lệ. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

MOD = 998244353
TOP = 59

def solve():
    n, x = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    # Only distinct values need structural nodes.
    vals = []
    for v in a:
        if not vals or vals[-1] != v:
            vals.append(v)

    m = len(vals)

    # Compressed binary trie.
    #
    # bit[u]  : branching bit, -1 for a leaf
    # left[u] : child with bit 0
    # right[u]: child with bit 1
    # rep[u]  : any representative value in the subtree
    # total[u]: sum of dp values currently stored in the subtree
    # parent[u]: parent node
    bit = []
    left = []
    right = []
    rep = []
    total = []
    parent = []

    leaf_of = {}

    def new_node(b, lch, rch, representative, par):
        idx = len(bit)
        bit.append(b)
        left.append(lch)
        right.append(rch)
        rep.append(representative)
        total.append(0)
        parent.append(par)
        return idx

    def build(lo, hi, par):
        if hi - lo == 1:
            v = vals[lo]
            u = new_node(-1, -1, -1, v, par)
            leaf_of[v] = u
            return u

        diff = vals[lo] ^ vals[hi - 1]
        b = diff.bit_length() - 1
        threshold = ((vals[lo] >> (b + 1)) << (b + 1)) | (1 << b)

        mid = bisect_left(vals, threshold, lo, hi)

        u = new_node(b, -1, -1, vals[lo], par)
        lc = build(lo, mid, u)
        rc = build(mid, hi, u)
        left[u] = lc
        right[u] = rc
        return u

    root = build(0, m, -1)

    def query(v):
        """
        Return the sum of dp values stored at y such that
        y xor v >= x.
        """
        u = root
        ans = 0

        # All bits above `top` are already known to be equal
        # between (representative xor v) and x.
        top = TOP

        while True:
            b = bit[u]

            if b == -1:
                if (rep[u] ^ v) >= x:
                    ans += total[u]
                    if ans >= MOD:
                        ans -= MOD
                return ans

            z = rep[u] ^ v

            # Bits b+1 ... top are fixed for the whole subtree.
            # If they already differ from x, the whole subtree has
            # the same comparison result.
            d = (z ^ x) >> (b + 1)
            if d:
                highest = b + 1 + d.bit_length() - 1
                if (z >> highest) & 1:
                    ans += total[u]
                    if ans >= MOD:
                        ans -= MOD
                return ans

            vb = (v >> b) & 1
            xb = (x >> b) & 1

            if xb == 0:
                # XOR bit 1 is already larger than x at this bit.
                greater_child = right[u] if vb == 0 else left[u]
                if greater_child != -1:
                    ans += total[greater_child]
                    if ans >= MOD:
                        ans -= MOD

                # XOR bit 0 keeps the prefix equal.
                equal_child = left[u] if vb == 0 else right[u]
            else:
                # XOR bit 0 would make the result smaller.
                # Only XOR bit 1 can keep equality.
                equal_child = right[u] if vb == 0 else left[u]

            if equal_child == -1:
                return ans

            u = equal_child
            top = b - 1

    answer = 0

    for v in a:
        dp = query(v) + 1
        if dp >= MOD:
            dp -= MOD

        answer += dp
        if answer >= MOD:
            answer -= MOD

        u = leaf_of[v]
        while u != -1:
            total[u] += dp
            if total[u] >= MOD:
                total[u] -= MOD
            u = parent[u]

    print(answer)

if __name__ == "__main__":
    solve()
```Phần đầu tiên sắp xếp mảng và tạo`vals`, danh sách được sắp xếp của các giá trị riêng biệt. Các bản sao được giữ lại trong`a`, bởi vì mỗi lần xuất hiện đại diện cho một vị trí mảng khác nhau và do đó có thể có một chuỗi con khác nhau. Tri nén chỉ lưu trữ một lá cấu trúc cho mỗi giá trị riêng biệt. 

các`build`hàm tìm bit cao nhất khác nhau giữa giá trị nhỏ nhất và lớn nhất của cây con. Vì đó là bit khác nhau đầu tiên nên tất cả các giá trị bên dưới nó đều có cùng tiền tố bit cao hơn. Các giá trị có bit này bằng 0 tạo thành một phạm vi liền kề và các giá trị có bit này bằng một phạm vi khác. Cây kết quả có tối đa (2m-1) nút cho (m) giá trị riêng biệt. 

các`total`mảng lưu trữ tổng giá trị DP trong mỗi cây con. Do đó, việc cập nhật một lá đòi hỏi phải duyệt qua tổ tiên của nó. Một đường dẫn có tối đa 60 cấp độ phân nhánh, do đó chi phí cập nhật (O(60)). 

Phần tinh tế là`query`. Một trie nhị phân bình thường có một nút ở mọi vị trí bit. Quá trình thử nén bỏ qua các vị trí mà tất cả các giá trị đều đồng ý. Tại một nút nén,`rep[u]`đại diện cho các bit bị bỏ qua cho mọi giá trị trong cây con đó. biểu thức```
d = (z ^ x) >> (b + 1)
```kiểm tra xem tiền tố bị bỏ qua có khác với (x) hay không. Nếu đúng như vậy, bit khác biệt cao nhất sẽ xác định sự so sánh cho mọi giá trị trong cây con, do đó truy vấn có thể kết thúc ngay lập tức. 

Khi các tiền tố bằng nhau, bit phân nhánh hiện tại được xử lý chính xác giống như ngưỡng XOR thông thường. Đối với bit (x) bằng 0, nhánh tạo ra bit XOR một là hoàn toàn hợp lệ. Đối với bit (x) của một, nhánh tạo ra bit 0 XOR là hoàn toàn không hợp lệ. Chỉ có nhánh bình đẳng cần kiểm tra thêm. 

Truy vấn được thực hiện trước khi chèn giá trị DP hiện tại. Đây là điều đảm bảo rằng (f_i) chỉ sử dụng (j<i). Sự đóng góp đơn lẻ sau đó được thêm vào một cách riêng biệt với`+1`. 

Các giá trị có thể lớn bằng (2^{60}-1), do đó bit có liên quan cao nhất là 59. Python có các số nguyên có độ chính xác tùy ý nên không cần xử lý tràn. Mô-đun này được áp dụng cho mọi tổng được lưu trữ và cho câu trả lời sau mỗi lần cộng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là`n=3`,`x=0`và các giá trị được sắp xếp là`0,1,2`. Vì mọi XOR không âm ít nhất bằng 0 nên mọi dãy con đều hợp lệ. 

| Bước | Giá trị (v) | Kết quả truy vấn | (dp) | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | 1 | 
| 2 | 1 | 1 | 2 | 3 | 
| 3 | 2 | 3 | 4 | 7 | 

Ở phần tử đầu tiên không có giá trị nào trước đó nên chỉ tính phần tử đơn. Ở phần tử thứ hai, phần tử đơn và chuỗi con`[0,1]`là hợp lệ. Ở phần tử thứ ba, cả bốn dãy con đều kết thúc bằng`2`là hợp lệ. Câu trả lời cuối cùng là (7), khớp (2^3-1). 

### Mẫu 2 

Bây giờ (x=2), với các giá trị được sắp xếp`0,1,2`. 

| Bước | Giá trị (v) | Tổng DP hợp lệ trước đó | (dp) | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | 1 | 
| 2 | 1 | 0 | 1 | 2 | 
| 3 | 2 | 2 | 3 | 5 | 

Vì`v=1`, giá trị trước đó duy nhất là`0`và (0\oplus1=1<2), do đó cặp này không thể được hình thành. Vì`v=2`, cả hai giá trị trước đó đều hoạt động vì (0\oplus2=2) và (1\oplus2=3). Do đó, ba dãy con hợp lệ kết thúc tại`2`là`[2]`,`[0,2]`, Và`[1,2]`. Câu trả lời là`5`. 

Dấu vết thứ hai cũng thực hiện ranh giới đẳng thức: XOR chính xác bằng (x) được chấp nhận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n+n\cdot60)) | Chi phí sắp xếp (O(n\log n)), trong khi mỗi truy vấn và cập nhật trie tuân theo tối đa 60 bit phân nhánh | 
| Không gian | (O(n)) | Trie nén có tối đa (2m-1) nút cho (m\le n) giá trị riêng biệt | 

Đầu vào lớn nhất chứa (300000) giá trị, do đó bước sắp xếp (O(n\log n)) và hệ số 60 bit không đổi đều thực tế. Biểu diễn nén đặc biệt hữu ích trong Python vì nó tránh được sự bùng nổ nút khoảng (60n) của một trie nhị phân không nén. Vấn đề ban đầu cung cấp 1024 MiB bộ nhớ và việc triển khai nén sử dụng không gian tuyến tính. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây giả định giải pháp đã gửi được lưu dưới dạng`solution.py`và phơi bày`solve()`chức năng hiển thị ở trên.```python
import sys
import io
import random

from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run(
    "3 0\n"
    "0 1 2\n"
) == "7", "sample 1"

assert run(
    "3 2\n"
    "0 1 2\n"
) == "5", "sample 2"

assert run(
    "3 3\n"
    "0 1 2\n"
) == "4", "sample 3"

assert run(
    "7 4\n"
    "11 5 5 8 3 1 3\n"
) == "35", "sample 4"

# Minimum-size input
assert run(
    "1 123456789\n"
    "42\n"
) == "1", "single element"

# x = 0: every non-empty subsequence is valid
assert run(
    "4 0\n"
    "7 7 7 7\n"
) == "15", "x = 0"

# Equal values with positive x: only singletons are valid
assert run(
    "4 1\n"
    "5 5 5 5\n"
) == "4", "all equal values"

# Equality boundary: XOR exactly x must be accepted
assert run(
    "2 2\n"
    "0 2\n"
) == "3", "xor exactly x"

# Maximum 60-bit values
M = (1 << 60) - 1
assert run(
    f"3 {M}\n"
    f"0 {M} {M - 1}\n"
) == "4", "60-bit boundary"

# Maximum-size input, x = 0.
# Every one of the 2^n - 1 non-empty subsequences is valid.
n = 300000
big_input = f"{n} 0\n" + ("0 " * n) + "\n"
expected = (pow(2, n, 998244353) - 1) % 998244353
assert run(big_input) == str(expected), "maximum n"

# Small randomized cross-check against brute force.
def brute(a, x):
    n = len(a)
    ans = 0

    for mask in range(1, 1 << n):
        chosen = [a[i] for i in range(n) if mask >> i & 1]
        ok = True

        for i in range(len(chosen)):
            for j in range(i + 1, len(chosen)):
                if (chosen[i] ^ chosen[j]) < x:
                    ok = False
                    break
            if not ok:
                break

        if ok:
            ans += 1

    return ans % 998244353

rng = random.Random(0)

for n in range(1, 8):
    for _ in range(50):
        a = [rng.randrange(16) for _ in range(n)]
        x = rng.randrange(16)

        inp = f"{n} {x}\n" + " ".join(map(str, a)) + "\n"
        expected = brute(a, x)

        assert run(inp) == str(expected), (
            f"random test failed: n={n}, x={x}, a={a}"
        )
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 123456789 / 42`|`1`| Kích thước tối thiểu và xử lý đơn lẻ | 
|`4 0 / 7 7 7 7`|`15`| Trường hợp đặc biệt (x=0) và vị trí trùng lặp | 
|`4 1 / 5 5 5 5`|`4`| Các giá trị bằng nhau không thể tạo thành cặp khi (x>0) | 
|`2 2 / 0 2`|`3`| các`>= x`ranh giới | 
|`3 (2^60-1) / 0, 2^60-1, 2^60-2`|`4`| Bit được hỗ trợ cao nhất | 
|`300000 0 / 300000 zeros`| (2^{300000}-1\bmod998244353) | Tối đa (n), câu trả lời lớn và hiệu suất | 
| Các trường hợp ngẫu nhiên với (n\le7) | Kết quả vũ phu | Kiểm tra chéo DP hoàn chỉnh và logic trie | 

## Vỏ cạnh 

Với (x=0), hãy xét`4 0`với các giá trị`7 7 7 7`. Mọi cặp đều có XOR ít nhất bằng 0, kể cả các cặp có giá trị bằng nhau có XOR bằng 0. Truy vấn DP chấp nhận mọi giá trị trước đó, vì vậy các giá trị DP là (1,2,4,8), cho ra (15). Trie được nén không cần đường dẫn mã đặc biệt cho (x=0), vì tại mỗi bit, một bit XOR của một đã lớn hơn bit 0 tương ứng, trong khi nhánh đẳng thức cuối cùng sẽ đến lá và chấp nhận XOR 0. 

Đối với tất cả các giá trị bằng nhau có dương (x), hãy xem xét`4 1`với các giá trị`5 5 5 5`. Mọi cặp giá trị bằng nhau đều có XOR bằng 0, do đó chỉ có bốn chuỗi con đơn lẻ là hợp lệ. Trie chứa một lá cho giá trị`5`, nhưng trọng lượng lưu trữ của nó được cập nhật sau mỗi lần xuất hiện. Mọi truy vấn sau đó đều đến lá đó và từ chối nó vì`5 xor 5 = 0 < 1`. Do đó mỗi giá trị DP vẫn là một. 

Đối với ranh giới đẳng thức, hãy xem xét`2 2`với các giá trị`0 2`. Sau khi xử lý`0`, giá trị DP của nó là một. Khi xử lý`2`, truy vấn trie sẽ thấy (0\oplus2=2), chính xác là (x), do đó nó bao gồm giá trị DP trước đó. Do đó, giá trị DP thứ hai là hai, biểu thị`[2]`Và`[0,2]`, và câu trả lời cuối cùng là ba. 

Đối với ranh giới 60-bit, hãy xem xét```
3 1152921504606846975
0 1152921504606846975 1152921504606846974
```Ngưỡng là (2^{60}-1). Cặp đôi`0`với (2^{60}-1) có XOR chính xác bằng ngưỡng và hợp lệ. Hai cặp còn lại có XOR dưới ngưỡng, do đó, các chuỗi con hợp lệ là ba đơn vị cộng với một cặp đó, cho kết quả`4`. Quá trình triển khai sẽ kiểm tra các bit từ 59 đến 0, do đó bit cao nhất được phép sẽ được xử lý mà không xảy ra lỗi nào. 

Đối với trường hợp kích thước tối đa có (x=0), lấy (300000) số không. Mọi tập hợp con không trống của 300000 vị trí đều hợp lệ, vì vậy câu trả lời là (2^{300000}-1) modulo (998244353). DP đếm các tập hợp con này mà không bao giờ liệt kê chúng. Mỗi số 0 mới có thể mở rộng mọi dãy con được tính trước đó, tạo ra dãy nhân đôi quen thuộc (1,2,4,\ldots), trong khi trie nén chỉ lưu trữ một lá vì tất cả các giá trị đều giống hệt nhau.
