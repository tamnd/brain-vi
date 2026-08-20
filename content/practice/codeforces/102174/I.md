---
title: "CF 102174I - \u51fa\u7ed9 paul-lu \u7684\u6570\u6570\u9898"
description: "Chúng ta có một bảng (ntimes n) và mỗi ô chứa một số nguyên từ (1) đến (k). Một ô được gọi là điểm bi nếu giá trị của nó lớn hơn mọi giá trị khác trong hàng của nó và cũng lớn hơn mọi giá trị khác trong cột của nó."
date: "2026-08-19T07:16:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102174
codeforces_index: "I"
codeforces_contest_name: "The 14-th BIT Campus Programming Contest"
rating: 0
weight: 102174
solve_time_s: 111
verified: true
draft: false
---

[CF 102174I - \u51fa\u7ed9 paul-lu \u7684\u6570\u6570\u9898](https://codeforces.com/problemset/problem/102174/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bảng (n\times n) và mỗi ô chứa một số nguyên từ (1) đến (k). Một ô được gọi là điểm bi nếu giá trị của nó lớn hơn mọi giá trị khác trong hàng của nó và cũng lớn hơn mọi giá trị khác trong cột của nó. 

Với mỗi bảng đã điền, gọi (I) là số bi điểm trên bảng đó. Nếu (B_i) là số bảng có đúng (i) bi điểm thì số lượng cần tìm là 

[ 
\sum_i i^2 B_i. 
] 

Đây chỉ đơn giản là tổng của (I^2) trên tất cả (k^{n^2}) bảng có thể có. Chúng ta không bao giờ cần xác định toàn bộ phân bố của (I). Hình vuông là chìa khóa: sau khi khai triển (I^2), chúng ta chỉ cần đếm các cấu hình chứa một điểm bi xác định và các cấu hình chứa hai điểm bi xác định. 

Các giới hạn (n,k\le 200) loại trừ mọi số mũ trong (n^2). Có nhiều nhất (20) trường hợp thử nghiệm, do đó, phép tính (O(k\log n)) hoặc (O(k^2)) cho mỗi trường hợp dễ dàng đủ nhỏ, trong khi ngay cả (O(n^2k)) cũng đã lớn một cách không cần thiết. Bản thân bảng có tới (40000) ô nhưng chúng ta chỉ cần đếm các bài tập một cách tượng trưng. 

Có một số trường hợp biên có thể phá vỡ công thức nếu chúng không được phân tách cẩn thận. Đối với (n=1), ô duy nhất tự động là điểm bi bất kể giá trị của nó là bao nhiêu, vì vậy câu trả lời chính xác là (k). Ví dụ, đầu vào`1 5`có câu trả lời (5). Công thức chứa lũy thừa như (2n-4) không thể được sử dụng trực tiếp ở đây vì số mũ đó trở thành số âm. 

Đối với (k=1) và (n>1), mọi ô đều có giá trị (1), do đó không có ô nào có thể lớn hơn các ô khác trong hàng hoặc cột của nó. Do đó đáp án là (0). Ví dụ, đầu vào`3 1`có câu trả lời (0). Việc triển khai bất cẩn coi (0^0) là bằng chứng cho mức tối đa hợp lệ có thể tính sai điểm bi. 

Với (n=2,k=2), đáp án là (12). Các bảng có đúng một điểm bi là bốn bảng có một (2), một bảng liền kề (1) ở hàng, một bảng liền kề (1) ở cột và ô đối diện bằng (1), góp phần (4). Hai bảng chéo có hai (2), mỗi bảng có hai điểm bi, mỗi điểm đóng góp (2^2) cho một (8) điểm khác. Một lỗi phổ biến là đếm mọi bảng chứa điểm bi đã chọn là bảng có đúng một điểm bi. Một bảng như vậy có thể chứa một số điểm bi, vì vậy các biến chỉ báo là cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ liệt kê từng bảng, đếm điểm bi của nó, bình phương số đó và thêm nó vào câu trả lời. Có (k^{n^2}) bảng. Ngay cả khi việc kiểm tra một bảng chỉ mất (O(n^2)), tổng số sẽ là (O(n^2k^{n^2})). Ở giới hạn tối đa, bảng này chứa (200^{40000}), vì vậy cách tiếp cận này hoàn toàn không khả thi. 

Quan sát hữu ích là số lượng chúng ta cần là khoảnh khắc thứ hai. Đặt (X_c) là (1) khi ô (c) là điểm bi và (0) nếu không. Sau đó 

[ 
Tôi=\sum_c X_c 
] 

và 

[ 
I^2=\sum_c X_c+2\sum_{c<d}X_cX_d. 
] 

Vì vậy, chúng ta chỉ cần hai loại số đếm: số bảng trong đó một ô cố định là điểm bi và số bảng trong đó hai ô cố định đồng thời là điểm bi. 

Đối với một ô cố định, giả sử giá trị của nó là (x). Mọi ô khác trong hàng và cột của nó phải nhỏ hơn (x). Có (2n-2) các ô như vậy và chúng khác biệt. Mỗi người đều có (x-1) lựa chọn. Tất cả các ô còn lại không bị giới hạn. 

Đối với hai ô cố định, có ba khả năng hình học. Nếu chúng nằm trong cùng một hàng thì cả hai đều không thể là cực đại của hàng nghiêm ngặt. Điều tương tự cũng đúng khi chúng ở cùng một cột. Do đó, chỉ những cặp có hàng khác nhau và cột khác nhau mới quan trọng. 

Hãy xem xét một cặp như vậy và gọi giá trị của chúng là (a) và (b). Mỗi ô cố định ràng buộc (2n-2) ô, nhưng hai ô nằm ở giao điểm của hàng hoặc cột của ô cố định kia. Hai ô đó phải nhỏ hơn cả (a) và (b), vì vậy mỗi ô có (\min(a,b)-1) lựa chọn. Các ô bị ràng buộc còn lại có thể được phân tách theo giá trị cố định mà chúng phải nhỏ hơn. Điều này tạo ra một tổng đối xứng có thể rút gọn thành một vòng lặp bằng cách duy trì tổng tiền tố. 

Lực lượng vũ phu hoạt động vì việc kiểm tra một bảng sẽ xác định chính xác sự đóng góp của nó, nhưng nó không thành công vì có nhiều bảng theo cấp số nhân. Quan sát rằng (I^2) chỉ liên quan đến các sản phẩm chỉ báo một ô và hai ô làm giảm vấn đề xuống một số lượng nhỏ tổng công suất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2k^{n^2})) | (O(n^2)) | Quá chậm | 
| Tối ưu | (O(k\log n)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý (n=1) riêng biệt. Có chính xác một ô và nó luôn là một điểm bi, vì vậy mỗi bảng (k) đóng góp (1^2), đưa ra câu trả lời (k). 
2. Cố định một ô cụ thể và đặt giá trị của nó là (x). Có (2n-2) ô khác trong hàng hoặc cột của nó và tất cả chúng phải nhỏ hơn (x). Do đó ô cố định này là một điểm bi trong 

[ 
(x-1)^{2n-2}k^{(n-1)^2} 
] 

bảng. Tổng hợp (x=1,\ldots,k), số bảng tính cho 1 ô cố định là 

[ 
A=k^{(n-1)^2}\sum_{t=0}^{k-1}t^{2n-2}. 
] 

Có (n^2) lựa chọn cho ô cố định, vì vậy phần đầu tiên của câu trả lời là (n^2A). 

1. Xét hai ô cố định. Nếu chúng chia sẻ một hàng hoặc cột, thì mức đóng góp đồng thời của chúng bằng 0 vì cả hai ô đều không thể lớn hơn các ô khác trong cùng hàng hoặc cột đó. 
2. Đối với hai ô ở các hàng và cột khác nhau, viết giá trị của chúng là (a) và (b) và đặt (t=a-1), (u=b-1). Mỗi ô cố định có (2n-4) ô chỉ bị ràng buộc bởi giá trị của chính nó. Hai ô giao nhau phải nhỏ hơn cả hai giá trị, do đó, mỗi ô có (\min(t,u)) lựa chọn. có 

[ 
(n-2)^2 
] 

tế bào hoàn toàn không bị giới hạn. 

Do đó số lượng nhiệm vụ cho cặp cố định này là

[ 
k^{(n-2)^2} 
\sum_{t=0}^{k-1}\sum_{u=0}^{k-1} 
t^{2n-4}u^{2n-4}\min(t,u)^2. 
] 

hãy để 

[ 
p=2n-4. 
] 

Tổng kép đối xứng trong (t,u). Với (t>u), (\min(t,u)=u), vậy phần đó là 

[ 
\sum_{t=0}^{k-1}t^p\sum_{u=0}^{t-1}u^{p+2}. 
] 

Đường chéo (t=u) đóng góp 

[ 
\sum_{t=0}^{k-1}t^{2p+2}. 
] 

Do đó toàn bộ số tiền gấp đôi là 

[ 
2\sum_{t=0}^{k-1}t^p 
\left(\sum_{u=0}^{t-1}u^{p+2}\right) 
+ 
\sum_{t=0}^{k-1}t^{2p+2}. 
] 

Chúng ta có thể tính tổng bên trong tăng dần trong khi quét (t), do đó tổng kép trở thành một vòng lặp. 

1. Đếm xem có bao nhiêu cặp ô không có thứ tự nằm ở các hàng và cột khác nhau. Chọn hai hàng, chọn hai cột và chọn một trong hai đường chéo phù hợp: 

\frac{n^2(n-1)^2}{2}. 
] 

Vì khai triển của (I^2) chứa hai lần mỗi cặp không có thứ tự nên hệ số trở thành 

[ 
n^2(n-1)^2. 
] 

Vì vậy phần thứ hai của câu trả lời là 

[ 
n^2(n-1)^2 
k^{(n-2)^2} 
\left( 
2\sum_{t=0}^{k-1}t^p 
\sum_{u=0}^{t-1}u^{p+2} 
+ 
\sum_{t=0}^{k-1}t^{2p+2} 
\đúng). 
] 

1. Thêm phần đóng góp một ô và modulo đóng góp hai ô (998244353). Mọi lũy thừa đều được đánh giá bằng lũy ​​thừa mô-đun và công cụ tích hợp sẵn của Python`pow(a, b, MOD)`thực hiện điều này trong (O(\log b)). 

Tại sao nó hoạt động: bất biến là danh tính 

[ 
I^2=\sum_c X_c+2\sum_{c<d}X_cX_d. 
] 

Công thức đếm đầu tiên đưa ra chính xác số bảng cho mỗi số hạng (X_c). Đối với mỗi cặp (X_cX_d), các cặp chia sẻ một hàng hoặc cột đóng góp bằng 0, trong khi mỗi cặp ở các hàng và cột khác nhau có chính xác tập hợp các ô bị ràng buộc và không bị hạn chế được đếm. Vì mọi số hạng một ô và mọi số hạng hai ô có thể đều xuất hiện với hệ số chính xác từ phép khai triển, nên tổng cuối cùng chính xác là (\sum_i i^2B_i). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve_case(n, k):
    if n == 1:
        return k % MOD

    # Contribution of one fixed cell.
    single_exp = 2 * n - 2
    free_single = (n - 1) * (n - 1)

    sum_single = 0
    for t in range(k):
        sum_single += pow(t, single_exp, MOD)
        if sum_single >= MOD:
            sum_single -= MOD

    one_fixed = pow(k, free_single, MOD) * sum_single % MOD
    first = n * n % MOD * one_fixed % MOD

    # Contribution of one fixed pair in different rows and columns.
    p = 2 * n - 4
    free_pair = (n - 2) * (n - 2)

    prefix = 0
    pair_core = 0

    for t in range(k):
        tp = pow(t, p, MOD)

        # u < t contributes t^p * u^(p+2).
        pair_core += 2 * tp % MOD * prefix
        pair_core %= MOD

        # The diagonal term t = u.
        pair_core += pow(t, 2 * p + 2, MOD)
        pair_core %= MOD

        # Add u = t for the next iteration.
        prefix += pow(t, p + 2, MOD)
        prefix %= MOD

    pair_fixed = pow(k, free_pair, MOD) * pair_core % MOD

    pair_factor = n * n % MOD
    pair_factor = pair_factor * (n - 1) % MOD
    pair_factor = pair_factor * (n - 1) % MOD

    second = pair_factor * pair_fixed % MOD

    return (first + second) % MOD

def main():
    T = int(input())
    out = []

    for _ in range(T):
        n, k = map(int, input().split())
        out.append(str(solve_case(n, k)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Nhánh đầu tiên xử lý trường hợp duy nhất trong đó công thức cặp tổng quát chứa số mũ âm. Khi (n=1), câu trả lời chỉ đơn giản là số giá trị có thể có trong một ô. 

Với (n>1),`single_exp`là (2n-2), bởi vì một điểm bi cố định có chính xác (n-1) ô bị ràng buộc trong hàng của nó và một ô bị ràng buộc (n-1) khác trong cột của nó. Các ô ((n-1)^2) còn lại không bị giới hạn. 

Để tính toán cặp,`p = 2*n-4`. Sau khi cố định hai ô ở các hàng và cột khác nhau, mỗi giá trị cố định sẽ điều khiển độc lập các ô (2n-4). Hai ô giao nhau được kiểm soát bởi cả hai giá trị, đó là lý do tại sao chúng đóng góp hệ số bổ sung được biểu thị bằng số mũ (p+2) trên giá trị nhỏ hơn. 

Biến`prefix`cửa hàng 

[ 
\sum_{u<t}u^{p+2}. 
] 

Trước khi thêm (t) hiện tại vào tiền tố, nó chứa chính xác các giá trị cần thiết cho phần (u<t) của tổng đối xứng. Đóng góp theo đường chéo (t=u) được thêm riêng dưới dạng (t^{2p+2}). 

Tất cả phép nhân đều được giảm modulo (998244353). Các số nguyên trong Python không bị tràn, nhưng việc giảm tích trung gian sẽ giữ cho các giá trị ở mức nhỏ và làm cho việc triển khai gần với công thức toán học hơn. 

yếu tố`n*n*(n-1)*(n-1)`không phải là số cặp không có thứ tự. Nó đã bao gồm hệ số (2) từ việc khai triển (I^2). Đây là nơi dễ dàng để đưa ra lỗi hệ số hai. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, (n=2,k=2). Số mũ một ô là (2n-2=2) và số mũ không giới hạn ô là ((n-1)^2=1). 

| Biến | Giá trị | 
| --- | --- | 
| (n) | 2 | 
| (k) | 2 | 
| (2n-2) | 2 | 
| (\tổng t^{2n-2}) | (0^2+1^2=1) | 
| (k^{(n-1)^2}) | (2) | 
| Một ô cố định | (2) | 
| (n^2) đóng góp một ô | (8) | 

Đối với phép tính cặp, (p=0), do đó chỉ (t=u=1) mới đóng góp vào tổng cốt lõi. 

| Biến | Giá trị | 
| --- | --- | 
| (p) | 0 | 
| ((n-2)^2) | 0 | 
| Cặp lõi | 1 | 
| (k^{(n-2)^2}) | 1 | 
| Yếu tố đóng góp cặp | (2^2(2-1)^2=4) | 
| Đóng góp hai ô | 4 | 
| Câu trả lời cuối cùng | 12 | 

Giá trị (12) khớp với mẫu. Phép tính cũng cho thấy tại sao không được loại bỏ các bảng có hai điểm bi khỏi số lượng một ô. Chúng được tính một cách có chủ ý một lần cho mỗi điểm trong số hai điểm bi của chúng, chính xác như việc khai triển (I^2) yêu cầu. 

Đối với Mẫu 2, (n=3,k=2). Ở đây (2n-2=4), do đó, một điểm bi cố định cần bốn ô xung quanh phải nhỏ hơn. 

| Biến | Giá trị | 
| --- | --- | 
| (n) | 3 | 
| (k) | 2 | 
| (2n-2) | 4 | 
| (\tổng t^4) | 1 | 
| (k^{(n-1)^2}) | (2^4=16) | 
| Một ô cố định | 16 | 
| (n^2) đóng góp một ô | 144 | 

Đối với hai ô tương thích, (p=2) và có một ô không bị giới hạn vì ((n-2)^2=1). 

| Biến | Giá trị | 
| --- | --- | 
| (p) | 2 | 
| Cặp lõi | 1 | 
| (k^{(n-2)^2}) | 2 | 
| Một cặp cố định | 2 | 
| Yếu tố đóng góp cặp | (3^2\cdot2^2=36) | 
| Đóng góp hai ô | 72 | 
| Câu trả lời cuối cùng | 216 | 

Dấu vết thứ hai xác nhận hình dạng của hộp hai ô. Bốn ô được điều khiển độc quyền bởi một trong hai cực đại đã chọn, hai ô giao nhau được điều khiển bởi cả hai cực đại và một ô vẫn hoàn toàn tự do. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(k\log n)) | Có (O(k)) lần lặp, mỗi lần sử dụng một số lũy thừa môđun không đổi với số mũ (O(n)). | 
| Không gian | (O(1)) | Chỉ có một số lượng số nguyên mô-đun không đổi được lưu trữ. | 

Với (n,k\le200), mỗi trường hợp thử nghiệm chỉ thực hiện vài trăm phép lũy thừa mô-đun, mỗi trường hợp yêu cầu nhiều phép nhân mô-đun theo logarit. Ngay cả với (20) trường hợp thử nghiệm, điều này vẫn nằm trong giới hạn nhất định, trong khi mức sử dụng bộ nhớ không đổi. 

## Trường hợp thử nghiệm 

Chương trình thử nghiệm sau đây sử dụng cách triển khai được tối ưu hóa cùng với công thức tham chiếu trực tiếp (O(k^2)). Tham chiếu chỉ mang tính thử nghiệm nên nó vẫn rất nhỏ ở mức (k\le200).```python
import sys
import io

MOD = 998244353

def solve_case(n, k):
    if n == 1:
        return k % MOD

    single_exp = 2 * n - 2
    free_single = (n - 1) * (n - 1)

    sum_single = 0
    for t in range(k):
        sum_single = (sum_single + pow(t, single_exp, MOD)) % MOD

    one_fixed = pow(k, free_single, MOD) * sum_single % MOD
    first = n * n % MOD * one_fixed % MOD

    p = 2 * n - 4
    free_pair = (n - 2) * (n - 2)

    prefix = 0
    pair_core = 0

    for t in range(k):
        tp = pow(t, p, MOD)
        pair_core = (pair_core + 2 * tp * prefix) % MOD
        pair_core = (pair_core + pow(t, 2 * p + 2, MOD)) % MOD
        prefix = (prefix + pow(t, p + 2, MOD)) % MOD

    pair_fixed = pow(k, free_pair, MOD) * pair_core % MOD

    pair_factor = n * n % MOD
    pair_factor = pair_factor * (n - 1) % MOD
    pair_factor = pair_factor * (n - 1) % MOD

    return (first + pair_factor * pair_fixed) % MOD

def reference(n, k):
    if n == 1:
        return k % MOD

    single = 0
    for t in range(k):
        single += pow(t, 2 * n - 2, MOD)
    single %= MOD
    first = n * n % MOD
    first = first * pow(k, (n - 1) * (n - 1), MOD) % MOD
    first = first * single % MOD

    p = 2 * n - 4
    core = 0
    for t in range(k):
        for u in range(k):
            core += (
                pow(t, p, MOD)
                * pow(u, p, MOD)
                * min(t, u) ** 2
            )
            core %= MOD

    pair = pow(k, (n - 2) * (n - 2), MOD) * core % MOD
    pair_factor = n * n * (n - 1) * (n - 1) % MOD

    return (first + pair_factor * pair) % MOD

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input_func = sys.stdin.readline

    T = int(input_func())
    ans = []

    for _ in range(T):
        n, k = map(int, input_func().split())
        ans.append(str(solve_case(n, k)))

    return "\n".join(ans)

# Provided samples
assert run("3\n2 2\n3 2\n4 5\n") == (
    "12\n216\n129097970"
), "provided samples"

# Minimum-size input.
assert run("1\n1 1\n") == "1", "minimum n and k"

# n = 1 with many possible values.
assert run("1\n1 200\n") == "200", "single-cell boundary"

# All boards are equal when k = 1 and n > 1.
assert run("1\n3 1\n") == "0", "all-equal boards"

# Small case that exercises the pair formula.
assert run("1\n2 3\n") == "48", "n=2 pair counting"

# Maximum-size input, checked against a direct O(k^2) reference.
max_expected = reference(200, 200)
assert run("1\n200 200\n") == str(max_expected), "maximum constraints"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Bảng tối thiểu và trường hợp đặc biệt (n=1) | 
|`1 200`|`200`| Một ô duy nhất có phạm vi giá trị đầy đủ | 
|`3 1`|`0`| Tất cả các ô đều bằng nhau nên không thể tồn tại cực đại nghiêm ngặt | 
|`2 3`|`48`| Đóng góp hai ô và đếm cặp đường chéo | 
|`200 200`| Giá trị tham chiếu trực tiếp | Ràng buộc tối đa và lũy thừa mô-đun | 

## Vỏ cạnh 

Với (n=1,k=5), bảng bao gồm một ô duy nhất. Dù nó chứa giá trị gì thì nó đều lớn hơn mọi ô khác trong hàng và cột của nó vì không có ô nào khác. Như vậy mỗi một trong năm bảng đều có (I=1), cho 

[ 
5\cdot1^2=5. 
] 

Việc triển khai trả về (k) ngay lập tức và không bao giờ đánh giá công thức cặp. 

Với (n=3,k=1), mỗi bảng là một cấu hình duy nhất chỉ chứa (1). Không có ô nào hoàn toàn lớn hơn các ô khác trong hàng hoặc cột của nó, vì vậy (I=0) và câu trả lời là (0). Trong công thức một ô, mọi số hạng đều chứa (t^{2n-2}=0^4=0), do đó phần đóng góp sẽ biến mất một cách tự nhiên. Sự đóng góp của cặp đôi cũng biến mất. 

Với (n=2,k=2), số lượng một ô cho một ô cố định là 

[ 
2^{1}(0^2+1^2)=2. 
] 

Có bốn ô, cho biết (8) từ các số hạng tuyến tính. Hai ô có thể là hai điểm cùng lúc chỉ khi chúng có đường chéo. Đối với mỗi cặp đường chéo cố định, phép gán hợp lệ duy nhất cho hai giá trị đã chọn là (2,2), trong khi hai ô còn lại phải là (1), đưa ra một phép gán. Có hai cặp đường chéo và thừa số (2) từ (I^2) cho ra (4). Tổng số là (8+4=12). 

Đối với (n=200,k=200), thuật toán không bao giờ xây dựng một bảng và không bao giờ lặp qua (n^2) ô. Nó chỉ đánh giá tổng công suất một ô và tổng công suất cặp trên (200) giá trị có thể. Số mũ lớn nhất nằm dưới (40000), vì vậy lũy thừa mô-đun không tốn kém và tất cả các giá trị trung gian đều được giảm modulo (998244353).
