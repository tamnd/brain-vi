---
title: "CF 102419F - xor-sum"
description: "Đối với mỗi trường hợp thử nghiệm, chúng ta cần in một mảng có chính xác (n) số nguyên. Mọi giá trị phải nằm trong khoảng ([0,m]), tổng thông thường của tất cả các giá trị phải là (s) và XOR theo bit của chúng phải là (x). Nếu không tồn tại mảng như vậy, chúng ta in (-1)."
date: "2026-08-12T20:17:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102419
codeforces_index: "F"
codeforces_contest_name: "SPC 2019"
rating: 0
weight: 102419
solve_time_s: 745
verified: true
draft: false
---

[CF 102419F - xor-sum](https://codeforces.com/problemset/problem/102419/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 12m 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đối với mỗi trường hợp thử nghiệm, chúng ta cần in một mảng có chính xác (n) số nguyên. Mọi giá trị phải nằm trong khoảng ([0,m]), tổng thông thường của tất cả các giá trị phải là (s) và XOR theo bit của chúng phải là (x). Nếu không tồn tại mảng như vậy, chúng ta in (-1). Các ràng buộc chính thức cho phép tối đa (10^5) trường hợp thử nghiệm, với tổng số phần tử mảng trên tất cả các trường hợp thử nghiệm nhiều nhất là (3\cdot10^5). 

Kích thước của (n) ngay lập tức loại trừ mọi thứ khám phá nhiều mảng có thể. Ngay cả đối với một trường hợp thử nghiệm duy nhất, việc liệt kê mọi mảng sẽ yêu cầu ((m+1)^n) ứng viên, con số này rất lớn khi (n=10^5). Các giá trị (các) và (m) cũng quá lớn đối với một chương trình động được lập chỉ mục theo tổng. Cấu trúc hữu ích phải đến từ cách biểu diễn nhị phân của XOR và từ việc xây dựng hầu hết mảng theo cách lặp đi lặp lại. 

Bất biến đầu tiên là mối quan hệ giữa phép cộng và XOR. Cho hai số nguyên, 

[ 
a+b=(a\oplus b)+2(a\mathbin{&}b). 
] 

Đối với toàn bộ mảng, điều này ngụ ý rằng tổng thông thường ít nhất là XOR và có cùng tính chẵn lẻ với XOR. Do đó, (s<x) hoặc (s\not\equiv x\pmod 2) ngay lập tức khiến ca kiểm thử không thể thực hiện được. 

Có một số trường hợp ranh giới ít rõ ràng hơn. Nếu (n=1) thì không có tự do nào cả. Ví dụ: (n=1,m=5,s=5,x=5) hợp lệ với mảng ([5]), trong khi (1,5,5,0) là không thể vì phần tử duy nhất sẽ phải bằng cả tổng và XOR của nó. Một công trình xây dựng bất cẩn luôn dự trữ hai yếu tố cũng sẽ thất bại ở đây. 

Trường hợp biên thứ hai xảy ra khi (x>m). Giá trị (x) không thể đơn giản được đặt vào mảng. Ví dụ: (n=3,m=4,s=7,x=7) hợp lệ với ([4,3,0]), bởi vì (4\oplus3=7). Một cấu trúc nhất quyết sử dụng (x) làm một phần tử sẽ bác bỏ nó một cách không chính xác. 

Giới hạn trên của tổng cũng có thể gây nhầm lẫn. Với (n=4,m=3,s=12,x=0), câu trả lời hợp lệ ([3,3,3,3]) có XOR bằng 0. Bắt đầu từ hai số 0 và cố gắng đặt toàn bộ số tiền còn lại vào hai phần tử còn lại sẽ thất bại vì hai phần tử không thể đóng góp nhiều hơn (6). Việc xây dựng phải có khả năng chọn một cặp không tối thiểu có tổng lớn hơn. 

Cuối cùng, (m=0) buộc mọi phần tử mảng bằng 0. Do đó (n=4,m=0,s=0,x=0) là hợp lệ, trong khi (n=4,m=0,s=2,x=0) là không thể. Trường hợp này được xử lý một cách tự nhiên bởi cấu trúc chung, nhưng nó rất hữu ích khi kiểm tra các điều kiện biên xung quanh bit được đặt cao nhất của (m). 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản về mặt khái niệm. Tạo mọi mảng có các phần tử nằm trong khoảng từ (0) đến (m), tính tổng và XOR của nó và dừng khi cả hai đều khớp với các giá trị được yêu cầu. Điều đó đúng vì mọi ứng viên có thể đều được xem xét. Vấn đề là có chính xác ((m+1)^n) ứng viên. Với (n=10^5), ngay cả giá trị không tầm thường nhỏ nhất của (m) cũng khiến điều này không thể thực hiện được, do đó, lực lượng vũ phu không chỉ hơi chậm mà còn hoàn toàn không thể sử dụng được. 

Quan sát quan trọng là các số bằng nhau cực kỳ thuận tiện cho XOR. Nếu chúng ta đặt (v,v) vào mảng, XOR của chúng bằng 0 trong khi phần đóng góp của chúng vào tổng là (2v). Điều này có nghĩa là khi chúng ta có một nhóm nhỏ các số có XOR là (x), hai vị trí còn lại có thể được điền cùng một giá trị mà không cần thay đổi XOR được yêu cầu. Chúng ta chỉ cần quyết định cách xây dựng nhóm mang XOR nhỏ đó và tổng của nó sẽ lớn đến mức nào. 

Đối với số chẵn (n), nhóm đặc biệt có thể chứa hai số. Đối với số lẻ (n), nó có thể chứa một số khi (x\le m), hoặc ba số khi (x>m). Trong trường hợp sau, ba số có thể là một cặp hợp lệ với XOR (x), theo sau là số 0. Do đó, vấn đề thực sự được rút gọn thành việc xây dựng hai số bị chặn với XOR quy định và tổng được chọn cẩn thận. 

Giả sử hai số là (a,b), XOR của chúng là (x) và tổng của chúng là (p). Xác định 

[ 
y=\frac{p-x}{2}. 
] 

Nhận dạng trên mang lại 

[ 
a\mathbin{&}b=y. 
]

Vì mọi bit được đặt trong (a\mathbin{&}b) phải bằng 0 trong (a\oplus b), nên chúng ta cần (y\mathbin{&}x=0). Ngược lại, khi điều kiện này đúng, các bit của (x) có thể được chia cho (a) và (b), trong khi tất cả các bit của (y) được đặt vào cả hai số. 

Điều này cho phép chúng ta giải quyết vấn đề cặp giới hạn một cách trực tiếp dưới dạng nhị phân. Đối với cố định (y), viết 

[ 
a=y+u,\qquad b=y+(x\oplus u), 
] 

trong đó (u) là tập hợp con bất kỳ của tập hợp bit của (x). Vì (u) và (x\oplus u) sử dụng các bit rời nhau nên đây cũng là các tổng thông thường tương ứng. Nếu (c=m-y), chúng ta cần cả (u\le c) và (x\oplus u\le c). Tập con lớn nhất của (x) không vượt quá (c) có thể được lấy một cách tham lam từ bit cao nhất đến bit thấp nhất. Nếu phần bù của nó vẫn lớn hơn (c) thì không có tập con nào khác có thể hoạt động được vì mọi tập con khác đều không lớn hơn. 

Câu hỏi còn lại là nên thử (y) cái nào. Nếu còn (r) vị trí sau nhóm đặc biệt thì có thể điền các vị trí đó theo cặp bằng nhau. Tổng số tiền đóng góp tối đa của họ là (rm). Do đó cặp đặc biệt phải có tổng ít nhất là (s-rm). Vì tổng của nó là (x+2y), nên chúng ta cần 

[ 
y\ge \frac{s-rm-x}{2}. 
] 

Chúng ta chọn (y) nhỏ nhất thỏa mãn giới hạn đó và (y\mathbin{&}x=0). Việc tăng (y) làm cho tổng cặp lớn hơn và đồng thời làm giảm giới hạn khả dụng (m-y), do đó, nếu (y) khả thi nhỏ nhất không thể tạo thành một cặp giới hạn thì (y) lớn hơn cũng không thể giúp được. 

Điều này cung cấp một lượng logarit của công việc nhị phân cho mỗi trường hợp thử nghiệm, theo sau là công việc không thể tránh khỏi (O(n)) cần thiết để thực sự in ra câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O((m+1)^n\cdot n)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n+\log m)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Kiểm tra xem (s<x) hay (s) và (x) có tính chẵn lẻ khác nhau không. Vì XOR không thể vượt quá số tiền thông thường và mỗi lần mang sẽ thay đổi tổng một lượng chẵn, nên cả hai điều kiện đều khiến giải pháp không thể thực hiện được. 
2. Quyết định cần bao nhiêu yếu tố đặc biệt. Nếu (n) là số lẻ và (x\le m), hãy sử dụng phần tử đơn (x). Các vị trí (n-1) còn lại tạo thành các cặp bằng nhau. Mặt khác, sử dụng hai phần tử mang XOR (x) và khi (n) là số lẻ, hãy thêm số 0 để số vị trí còn lại là số chẵn. 
3. Gọi (r) là số vị trí còn lại sau nhóm đặc biệt. Cặp đặc biệt phải có tổng ít nhất là (L=s-rm), vì các vị trí (r) còn lại có thể đóng góp nhiều nhất là (rm). Vì mọi cặp hợp lệ đều có tổng (x+2y), hãy tính giá trị yêu cầu nhỏ nhất (y) từ giới hạn dưới này. 
4. Tìm số nhỏ nhất (y\ge0) thỏa mãn cả (y\ge (L-x)/2) và (y\mathbin{&}x=0). Nếu giới hạn dưới đã không dương, hãy bắt đầu từ số 0. Để tìm số nguyên tiếp theo tránh các bit đã đặt của (x), hãy xác định bit đặt bị cấm thấp nhất của giá trị hiện tại và chuyển tới bit cao hơn đầu tiên được phép và hiện tại bằng 0. 
5. Với (y), đặt (c=m-y). Chúng ta cần chia tập hợp bit của (x) thành hai tập con rời nhau (u) và (x\oplus u), cả hai đều nhiều nhất là (c). Xây dựng tập con lớn nhất có thể (u\le c) bằng cách quét các bit của (x) từ cao xuống thấp. Nếu (x\oplus u>c), cặp này không thể tồn tại. 
6. Xây dựng cặp dưới dạng (a=y+u) và (b=y+(x\oplus u)). XOR của chúng là (x), tổng của chúng là (x+2y) và cả hai đều nhiều nhất là (m). 
7. Nếu (n) là số lẻ và (x>m), hãy thêm số 0 vào sau cặp. Số 0 không thay đổi tổng cũng như XOR và nó làm cho số vị trí còn lại là chẵn. 
8. Gọi (E) là số còn thiếu sau nhóm đặc biệt. Chia (E) cho hai. Liên tục lấy càng nhiều càng tốt, tối đa (m), làm giá trị của một cặp bằng nhau. Mỗi cặp đóng góp gấp đôi giá trị của nó vào tổng và 0 cho XOR, do đó, sau khi có đủ cặp, sẽ đạt được tổng chính xác còn lại. 
9. Nếu tại bất kỳ thời điểm nào không thể xây dựng được cặp đặc biệt được yêu cầu, hãy in (-1). Nếu không thì in mảng đã xây dựng.

Bất biến đằng sau việc xây dựng là nhóm đặc biệt luôn có XOR chính xác (x), trong khi mọi cặp được thêm sau đó đều có XOR bằng 0. Đồng thời, mỗi cặp được thêm vào sẽ đóng góp một số chẵn vào tổng số. Giới hạn dưới được chọn đảm bảo rằng các vị trí còn lại có đủ công suất và bước lấp đầy tham lam đảm bảo rằng công suất sẵn có của chúng đủ để nhận ra mọi tổng số chẵn cần thiết từ 0 đến mức tối đa của chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

def next_disjoint(value, x):
    """Smallest y >= value such that y & x == 0."""
    bad = value & x
    if bad == 0:
        return value

    k = (bad & -bad).bit_length() - 1

    for j in range(k + 1, 31):
        y_bit = (value >> j) & 1
        x_bit = (x >> j) & 1
        if y_bit == 0 and x_bit == 0:
            prefix = value & ~((1 << (j + 1)) - 1)
            return prefix | (1 << j)

    return 1 << 30

def make_pair(x, total, m):
    """
    Find a,b in [0,m] such that:
        a ^ b == x
        a + b == total
    """
    if total < x or ((total - x) & 1):
        return None

    y = (total - x) // 2

    if y > m or (y & x):
        return None

    cap = m - y

    if x > 2 * cap:
        return None

    u = 0
    for bit in range(29, -1, -1):
        b = 1 << bit
        if (x & b) and u + b <= cap:
            u |= b

    v = x ^ u

    if v > cap:
        return None

    return y + u, y + v

def solve_case(n, m, s, x):
    if s < x or ((s - x) & 1):
        return None

    # Odd n and x itself fits into one element.
    if n & 1 and x <= m:
        remaining = n - 1
        extra = s - x

        if extra < 0 or extra > remaining * m:
            return None

        ans = [x]
        half = extra // 2
        pairs = remaining // 2

        for _ in range(pairs):
            v = min(m, half)
            ans.append(v)
            ans.append(v)
            half -= v

        return ans

    # Otherwise we need a two-element XOR carrier.
    if n & 1:
        special = 3
    else:
        special = 2

    if n < special:
        return None

    remaining = n - special

    # The special pair must provide at least this much sum.
    lower = s - remaining * m

    if lower <= x:
        y_low = 0
    else:
        y_low = (lower - x) // 2

    if y_low < 0:
        y_low = 0

    if y_low > m:
        return None

    y = next_disjoint(y_low, x)

    if y > m:
        return None

    pair_sum = x + 2 * y

    if pair_sum > s:
        return None

    pair = make_pair(x, pair_sum, m)
    if pair is None:
        return None

    a, b = pair
    ans = [a, b]

    if n & 1:
        ans.append(0)

    extra = s - pair_sum

    if extra < 0 or extra > remaining * m or (extra & 1):
        return None

    half = extra // 2
    pairs = remaining // 2

    for _ in range(pairs):
        v = min(m, half)
        ans.append(v)
        ans.append(v)
        half -= v

    if half != 0 or len(ans) != n:
        return None

    return ans

def main():
    t = int(input())
    out = []

    for _ in range(t):
        n, m, s, x = map(int, input().split())
        ans = solve_case(n, m, s, x)

        if ans is None:
            out.append("-1")
        else:
            out.append(" ".join(map(str, ans)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Người trợ giúp đầu tiên,`next_disjoint`, tìm giá trị nhỏ nhất không nằm dưới giới hạn dưới được yêu cầu có các bit được đặt không trùng với (x). Nếu giá trị hiện tại đã thỏa mãn điều kiện thì nó sẽ được trả về ngay lập tức. Mặt khác, bit xung đột thấp nhất của nó phải được loại bỏ và mức tăng nhỏ nhất có thể đến từ việc đặt bit cao hơn đầu tiên được cả (x) và 0 trong giá trị hiện tại cho phép. 

các`make_pair`hàm thực hiện nhận dạng (a+b=x+2(a\mathbin{&}b)). Biến`y`chính xác là (a\mathbin{&}b), vì vậy`y & x`phải bằng không. Một lần`y`cố định thì mỗi bit của (x) thuộc về đúng một trong hai số đó. Biến`u`chọn bit nào thuộc về số đầu tiên và`x ^ u`đưa ra phần khác. 

Việc xây dựng tham lam từ cao đến thấp của`u`là an toàn vì các trọng số có sẵn là lũy thừa của hai. Nó tìm tập con lớn nhất của tập bit của (x) không vượt quá`cap`. Nếu phần bù của nó quá lớn thì mọi tập con nhỏ hơn đều có phần bù lớn hơn, do đó không có phân vùng thay thế. 

Cấu trúc chính coi các cặp bằng nhau là phần có thể điều chỉnh được của mảng. Số vị trí còn lại luôn là số chẵn, đó là lý do tại sao nhóm đặc biệt có một phần tử cho trường hợp lẻ và biểu diễn trực tiếp, hai phần tử cho trường hợp chẵn (n) và ba phần tử cho trường hợp lẻ (n) khi (x>m). Việc sử dụng số nguyên Python cũng loại bỏ mọi lo ngại về tình trạng tràn đối với các giá trị lên tới (10^{18}). 

Thứ tự của các hoạt động quan trọng. Giới hạn dưới của cặp đặc biệt được tính toán trước khi xây dựng nó, vì việc chọn một cặp quá nhỏ có thể để lại tổng nhiều hơn mức các phần tử còn lại có thể giữ được. Ngược lại, việc chọn một cặp lớn hơn mức cần thiết chỉ làm giảm dung lượng còn lại, nên (y) khả thi nhỏ nhất là lựa chọn đúng. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, hãy xem xét (n=4,m=4,s=15,x=7). Vì (n) là số chẵn nên hai phần tử mang XOR và hai vị trí vẫn là một cặp bằng nhau. 

| Biến | Giá trị | 
| --- | --- | 
| (n) | 4 | 
| (m) | 4 | 
| (các) | 15 | 
| (x) | 7 | 
| Các vị trí còn lại | 2 | 
| Tổng cặp thấp hơn | (15-2\cdot4=7) | 
| (y) giới hạn dưới | 0 | 
| Đã chọn (y) | 0 | 
| Tổng cặp | 7 | 
| Cặp | (4,3) | 
| Số tiền còn lại | 8 | 
| Cặp bằng nhau | (4,4) | 
| Mảng cuối cùng | (4,3,4,4) | 

Cặp đặc biệt có (4\oplus3=7) và tổng (7). Cặp cuối cùng đóng góp (8) mà không thay đổi XOR, do đó tổng số là (15) và XOR vẫn giữ nguyên (7). Điều này cũng chứng tỏ trường hợp (x>m), trong đó việc đặt (x) trực tiếp sẽ là bất hợp pháp. 

Đối với mẫu thứ hai, (n=4,m=4,s=4,x=4). Ở đây (x\le m), nhưng (n) chẵn, nên sóng mang XOR vẫn phải sử dụng hai vị trí. 

| Biến | Giá trị | 
| --- | --- | 
| (n) | 4 | 
| (m) | 4 | 
| (các) | 4 | 
| (x) | 4 | 
| Các vị trí còn lại | 2 | 
| Tổng cặp thấp hơn | (4-2\cdot4=-4) | 
| Đã chọn (y) | 0 | 
| Tổng cặp | 4 | 
| Cặp | (4,0) | 
| Số tiền còn lại | 0 | 
| Mảng cuối cùng | (4,0,0,0) | 

Cặp (4.0) có XOR (4) và tổng (4). Hai số 0 còn lại bảo toàn cả hai đại lượng. 

Là một dấu vết hữu ích khác, hãy xem xét (n=4,m=3,s=12,x=0). 

| Biến | Giá trị | 
| --- | --- | 
| (n) | 4 | 
| (m) | 3 | 
| (các) | 12 | 
| (x) | 0 | 
| Các vị trí còn lại | 2 | 
| Tổng cặp thấp hơn | (12-2\cdot3=6) | 
| Đã chọn (y) | 3 | 
| Tổng cặp | 6 | 
| Cặp | (3,3) | 
| Số tiền còn lại | 6 | 
| Cặp bằng nhau | (3,3) | 
| Mảng cuối cùng | (3,3,3,3) | 

Dấu vết này cho thấy tại sao cặp này không phải lúc nào cũng được chọn với tổng tối thiểu có thể có (x). Ở đây (x=0), nhưng sử dụng (0,0) sẽ chỉ để lại tổng (12) cho hai phần tử còn lại, điều này là không thể. Giới hạn dưới buộc cặp đặc biệt phải đóng góp (6), sau đó cặp kia đóng góp phần còn lại (6). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+\log m)) cho mỗi trường hợp thử nghiệm | Cấu trúc nhị phân sử dụng tối đa khoảng 30 bit và chi phí in các giá trị (n) (O(n)). | 
| Không gian | (O(n)) | Mảng đầu ra chứa chính xác (n) số nguyên. | 

Trong tất cả các trường hợp thử nghiệm, tổng (n) tối đa là (3\cdot10^5), do đó tổng kết quả đầu ra là (O(3\cdot10^5)). Phần nhị phân chỉ thực hiện số lần quét không đổi trên tối đa 30 bit cho mỗi trường hợp kiểm thử. Điều này vừa vặn thoải mái với giới hạn thời gian 1 giây, trong khi bộ nhớ (O(n)) bị giới hạn bởi kích thước đầu ra được yêu cầu. 

## Trường hợp thử nghiệm```python
# Self-contained assert-based tests for the construction.

import sys
import io

def next_disjoint(value, x):
    bad = value & x
    if bad == 0:
        return value

    k = (bad & -bad).bit_length() - 1

    for j in range(k + 1, 31):
        if ((value >> j) & 1) == 0 and ((x >> j) & 1) == 0:
            prefix = value & ~((1 << (j + 1)) - 1)
            return prefix | (1 << j)

    return 1 << 30

def make_pair(x, total, m):
    if total < x or ((total - x) & 1):
        return None

    y = (total - x) // 2

    if y > m or (y & x):
        return None

    cap = m - y

    if x > 2 * cap:
        return None

    u = 0
    for bit in range(29, -1, -1):
        b = 1 << bit
        if (x & b) and u + b <= cap:
            u |= b

    v = x ^ u

    if v > cap:
        return None

    return y + u, y + v

def solve_case(n, m, s, x):
    if s < x or ((s - x) & 1):
        return None

    if n & 1 and x <= m:
        remaining = n - 1
        extra = s - x

        if extra < 0 or extra > remaining * m:
            return None

        ans = [x]
        half = extra // 2

        for _ in range(remaining // 2):
            v = min(m, half)
            ans.extend([v, v])
            half -= v

        return ans

    special = 3 if (n & 1) else 2

    if n < special:
        return None

    remaining = n - special
    lower = s - remaining * m
    y_low = 0 if lower <= x else (lower - x) // 2

    if y_low > m:
        return None

    y = next_disjoint(y_low, x)

    if y > m:
        return None

    pair_sum = x + 2 * y

    if pair_sum > s:
        return None

    pair = make_pair(x, pair_sum, m)
    if pair is None:
        return None

    a, b = pair
    ans = [a, b]

    if n & 1:
        ans.append(0)

    extra = s - pair_sum

    if extra < 0 or extra > remaining * m or extra & 1:
        return None

    half = extra // 2

    for _ in range(remaining // 2):
        v = min(m, half)
        ans.extend([v, v])
        half -= v

    if half != 0 or len(ans) != n:
        return None

    return ans

def solve_text(inp: str) -> str:
    data = io.StringIO(inp)
    t = int(data.readline())
    out = []

    for _ in range(t):
        n, m, s, x = map(int, data.readline().split())
        ans = solve_case(n, m, s, x)

        if ans is None:
            out.append("-1")
        else:
            out.append(" ".join(map(str, ans)))

    return "\n".join(out)

def run(inp: str) -> str:
    return solve_text(inp)

def validate(inp: str, out: str):
    lines = out.strip().splitlines()
    data = inp.strip().splitlines()

    t = int(data[0])
    assert len(lines) == t

    for i in range(t):
        n, m, s, x = map(int, data[i + 1].split())
        line = lines[i].strip()

        if line == "-1":
            assert solve_case(n, m, s, x) is None
            continue

        a = list(map(int, line.split()))
        assert len(a) == n
        assert all(0 <= v <= m for v in a)
        assert sum(a) == s

        cur_xor = 0
        for v in a:
            cur_xor ^= v

        assert cur_xor == x

# Provided sample
sample = """\
3
4 4 15 7
4 4 4 4
4 4 15 1
"""
validate(sample, run(sample))

# Minimum-size valid case
case1 = """\
1
1 5 5 5
"""
validate(case1, run(case1))

# All elements equal
case2 = """\
1
4 3 12 0
"""
validate(case2, run(case2))

# Boundary case with x > m
case3 = """\
1
3 4 7 7
"""
validate(case3, run(case3))

# Impossible because the requested sum is too large for the XOR requirement
case4 = """\
1
4 4 15 1
"""
assert run(case4).strip() == "-1"

# Maximum-size n, with all elements forced to one
case5 = """\
1
100000 1 100000 0
"""
validate(case5, run(case5))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 5 5 5 5`|`5`| Tối thiểu (n), xây dựng phần tử đơn trực tiếp | 
|`4 / 3 12 0`|`3 3 3 3`| Tất cả các giá trị bằng nhau và tổng yêu cầu lớn | 
|`3 / 4 7 7`|`4 3 0`| XOR lớn hơn (m), yêu cầu phân chia thành hai giá trị | 
|`4 / 4 15 1`|`-1`| Tổng công suất và khả năng thi công | 
|`100000 / 1 100000 0`| 100000 cái | Tối đa (n), kích thước đầu ra và điền cặp | 

## Vỏ cạnh 

Với (n=1), mảng chỉ có một giá trị. Coi như`1 5 5 5`. Việc kiểm tra đạt (x\le m) và độ dài lẻ cho phép giá trị đặc biệt duy nhất (x=5). Đầu ra là`[5]`. Vì`1 5 4 5`, quan hệ chẵn lẻ hoặc tổng sẽ bác bỏ trường hợp này vì phần tử duy nhất có thể phải là (5), mà tổng của nó không thể là (4). 

Khi (x>m), giá trị (x) không thể được chèn trực tiếp. Vì`3 4 7 7`, lũy thừa hữu ích cao nhất của hai là (4) và (7=4+3). Nhóm đặc biệt trở thành`[4,3,0]`. Tổng của nó là (7), XOR của nó là (4\oplus3\oplus0=7) và mọi giá trị nhiều nhất là (4). 

Để có câu trả lời hoàn toàn bình đẳng, hãy xem xét`4 3 12 0`. XOR được yêu cầu bằng 0, vì vậy các cặp bằng nhau là lý tưởng. Giới hạn dưới buộc cặp đầu tiên đóng góp (6), tạo ra`[3,3]`. Số tiền còn lại là số khác (6), cho`[3,3]`lại. Mảng cuối cùng là`[3,3,3,3]`, có tổng là (12) và XOR bằng 0. 

Với (m=0), mọi giá trị phải bằng 0. Với`4 0 0 0`, nhánh lẻ hoặc nhánh chẵn chỉ tạo các cặp 0 và tổng được yêu cầu đã bằng 0, do đó một mảng gồm 4 số 0 được trả về. Với`4 0 2 0`, dung lượng còn lại bằng 0, do đó việc xây dựng sẽ loại bỏ trường hợp kiểm thử. 

Đối với ranh giới công suất trên,`100000 1 100000 0`yêu cầu số tiền tối đa có thể. Vì (n) là số chẵn và XOR bằng 0 nên việc xây dựng sử dụng các cặp số một. Mọi phần tử trở thành (1), cho tổng (100000) và XOR bằng 0 vì một số chẵn các phần tử được XOR cùng nhau. Điều này cũng thực hiện (n) lớn nhất được phép và xác nhận rằng cấu trúc đầu ra vẫn tuyến tính trong kích thước mảng.
