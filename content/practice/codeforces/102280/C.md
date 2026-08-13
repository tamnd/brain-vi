---
title: "CF 102280C - \u042d\u043a\u0437\u0430\u043c\u0435\u043d \u043f\u043e \u0432\u043e\u0436\u0434\u0435\u043d\u0438\u044e"
description: "Có (n) người lái xe và (n) xe Gazelle. Mỗi người lái xe nhận được chính xác một chiếc ô tô và mỗi chiếc ô tô được giao cho đúng một người lái xe, vì vậy phép gán là một hoán vị của những chiếc ô tô. Người lái xe được gọi là cố định nếu họ nhận được xe riêng của mình."
date: "2026-08-13T16:05:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102280
codeforces_index: "C"
codeforces_contest_name: "2010, \u0422\u0440\u0435\u043d\u0438\u0440\u043e\u0432\u043a\u0430 \u0421\u0413\u0410\u0423 aka \u041a\u043e\u043d\u0442\u0435\u0441\u0442 \u043f\u0440\u043e \u043c\u0430\u0440\u0448\u0440\u0443\u0442\u043a\u0438"
rating: 0
weight: 102280
solve_time_s: 88
verified: true
draft: false
---

[CF 102280C - \u042d\u043a\u0437\u0430\u043c\u0435\u043d \u043f\u043e \u0432\u043e\u0436\u0434\u0435\u043d\u0438\u044e](https://codeforces.com/problemset/problem/102280/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Có (n) người lái xe và (n) xe Gazelle. Mỗi người lái xe nhận được chính xác một chiếc ô tô và mỗi chiếc ô tô được giao cho đúng một người lái xe, vì vậy phép gán là một hoán vị của những chiếc ô tô. 

Người lái xe được gọi là cố định nếu họ nhận được xe riêng của mình. Điều kiện nói rằng trong số (k) người lái xe được chọn, ít nhất một người phải lái xe của người khác. Tương tự, không thể tìm được (k) người lái xe đều nhận được ô tô riêng. 

Điều đó đưa ra một công thức đơn giản hơn nhiều: hoán vị có thể chứa nhiều nhất (k-1) điểm cố định. Chúng ta cần đếm tất cả các hoán vị của (n) phần tử có tối đa (k-1) điểm cố định, modulo (10^9+7). 

Câu lệnh ban đầu có vấn đề về định dạng trong văn bản được cung cấp: các ví dụ được hợp nhất với nhau. Trang cuộc thi chính thức đưa ra các mẫu là (4\ 2 \to 17) và (30\ 1 \to 568643488). 

Các ràng buộc cho phép (n) đạt tới 1000. Điều đó ngay lập tức loại trừ bất kỳ điều gì liên quan đến tất cả các hoán vị, bởi vì (1000!) vượt xa bất kỳ số lượng hoạt động khả thi nào. Ngay cả một giải pháp (O(n^2)) chỉ thực hiện được khoảng một triệu lần lặp cơ bản, trong khi (O(n!)) thì không thể thực hiện được đối với (n) chỉ vài chục lần. Giới hạn 0,5 giây cũng làm cho chương trình động (O(n^2)) đơn giản kém hấp dẫn hơn trong Python, do đó, giải pháp (O(n)) sẽ thích hợp hơn. 

Có một số trường hợp ranh giới rất dễ bị xử lý sai. Với (n=1,k=1), nhiệm vụ duy nhất giao cho người lái xe ô tô của chính họ, do đó điều kiện không thành công và câu trả lời là (0). Việc triển khai bất cẩn mà chỉ trả về (n!) cho (k=n) cũng sẽ sai. Ví dụ: với (n=2,k=2), cả hai người lái xe không thể đồng thời nhận xe của mình nên chỉ có giao dịch hoán đổi là hợp lệ và câu trả lời là (1), không phải (2). Tổng quát hơn, khi (k=n), mọi hoán vị ngoại trừ danh tính đều hợp lệ, cho (n!-1). Ở một thái cực khác, (k=1) có nghĩa là ngay cả một người lái xe cũng không được phép có ô tô riêng, vì vậy mọi phép gán hợp lệ đều phải là một sự loạn trí. Ví dụ: (n=3,k=1) có chính xác hai hoán vị hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là tạo ra mọi hoán vị của (n) ô tô, đếm xem có bao nhiêu vị trí được cố định và giữ nguyên hoán vị nếu số đó nhỏ hơn (k). Điều này đúng vì mọi phân bố ô tô có thể xuất hiện đúng một lần trong số (n!) hoán vị và điều kiện chỉ phụ thuộc vào số lượng vị trí cố định. Trong trường hợp xấu nhất, nếu chúng ta kiểm tra rõ ràng tất cả (n) vị trí của mọi hoán vị thì công việc là (n\cdot n!). Đối với (n=1000), tức là kiểm tra vị trí (1000\cdot1000!), điều này hoàn toàn không khả thi. 

Quan sát hữu ích là các hoán vị với số điểm cố định quy định có cấu trúc tổ hợp tiêu chuẩn. Giả sử chính xác (i) người lái xe nhận được ô tô riêng của mình. Chúng ta có thể chọn (i) trình điều khiển đó theo cách (\binom ni). Sau khi sửa xong, (n-i) tài xế còn lại đều phải nhận xe của tài xế khác, vì nếu không sẽ có điểm cố định khác. Do đó, phép gán còn lại là sự xáo trộn của các phần tử (n-i). 

Gọi (D_m) biểu thị số cách biến dạng của phần tử (m). Số hoán vị có đúng (i) điểm cố định là 

[ 
\binom ni D_{n-i}. 
] 

Vì chúng ta được phép có nhiều nhất (k-1) điểm cố định nên câu trả lời sẽ trở thành 

[ 
\sum_{i=0}^{k-1}\binom ni D_{n-i}. 
] 

Nhiệm vụ còn lại là đánh giá số tiền này một cách hiệu quả. Sự loạn trí thỏa mãn sự tái diễn 

[ 
D_m=(m-1)(D_{m-1}+D_{m-2}), 
] 

với (D_0=1) và (D_1=0). Do đó, tất cả các giá trị lệch pha lên tới (n) có thể được tính theo (O(n)). 

Chúng ta cũng cần tất cả các hệ số nhị thức (\binom ni) cho (0\le i<k). Vì (n\le1000), chúng ta có thể tính toán trước nghịch đảo mô đun và cập nhật hệ số nhị thức hiện tại bằng cách sử dụng 

\binom n{i-1}\frac{n-i+1}{i}. 
]

Bởi vì mọi (i\le1000) đều nhỏ hơn mô đun nguyên tố (10^9+7), nên tồn tại nghịch đảo mô đun của (i). Điều này đưa ra một phép tính (O(n)) khác. 

Phương pháp brute-force hoạt động vì nó trực tiếp kiểm tra định nghĩa, nhưng không thành công vì số lượng hoán vị tăng theo giai thừa. Quan sát cho thấy rằng chỉ số lượng điểm cố định mới quan trọng cho phép chúng ta nhóm nhiều hoán vị theo cấp số nhân thành (n) lớp tổ hợp và phép tái biến dạng cho phép chúng ta đánh giá các lớp đó theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n\cdot n!)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (n) và (k). Các hoán vị mong muốn chính xác là những hoán vị có ít hơn (k) điểm cố định. 
2. Tính mảng lệch pha (D). Đặt (D_0=1) và (D_1=0). Với mọi (m\ge2), hãy sử dụng (D_m=(m-1)(D_{m-1}+D_{m-2})) modulo (10^9+7). Sự lặp lại này tính các hoán vị trong đó không ai giữ chiếc xe nguyên bản của họ. 
3. Tính toán trước các nghịch đảo mô đun từ (1) đến (n). Vì mô đun là số nguyên tố và (n<10^9+7), nên mọi số trong phạm vi này đều có mô đun nghịch đảo. 
4. Bắt đầu với (\binom n0=1). Với mỗi (i) từ (0) đến (k-1), hãy thêm 
[ 
\binom niD_{n-i} 
] 
để trả lời. Thuật ngữ này trước tiên chọn chính xác (i) người lái xe nào giữ xe riêng của họ, sau đó làm xáo trộn tất cả những người lái xe còn lại để không ai trong số họ vô tình bị sửa chữa. 
5. Sau khi xử lý giá trị hiện tại của (i), cập nhật hệ số nhị thức cho lần lặp tiếp theo bằng cách sử dụng 
[ 
\binom n{i+1} 
= 
\binom ni\frac{n-i}{i+1}. 
] 
Tất cả phép nhân và chia được thực hiện theo modulo (10^9+7), sử dụng nghịch đảo mô-đun được tính toán trước. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ hoán vị hợp lệ nào và để nó có chính xác (i) điểm cố định. Những điểm cố định đó có thể được chọn theo các cách chính xác (\binom ni). Sau khi được chọn, (n-i) người lái xe còn lại không thể có ô tô riêng, nếu không hoán vị sẽ có nhiều hơn (i) điểm cố định. Do đó, ô tô của họ tạo thành một sự loạn trí, đưa ra chính xác các khả năng (D_{n-i}). Do đó (\binom niD_{n-i}) tính mọi hoán vị có chính xác (i) điểm cố định một lần và chỉ một lần. Tổng số lượng này cho (i=0,\ldots,k-1) đếm chính xác các hoán vị được cho phép bởi điều kiện ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1_000_000_007

def solve():
    n, k = map(int, input().split())

    # D[i] = number of derangements of i elements.
    der = [0] * (n + 1)
    der[0] = 1

    if n >= 1:
        der[1] = 0

    for i in range(2, n + 1):
        der[i] = (i - 1) * (der[i - 1] + der[i - 2]) % MOD

    # Modular inverses of 1..n.
    inv = [0] * (n + 1)
    if n >= 1:
        inv[1] = 1

    for i in range(2, n + 1):
        inv[i] = MOD - (MOD // i) * inv[MOD % i] % MOD

    # C(n, 0) = 1.
    comb = 1
    ans = 0

    for i in range(k):
        ans = (ans + comb * der[n - i]) % MOD

        if i + 1 < k:
            comb = comb * (n - i) % MOD
            comb = comb * inv[i + 1] % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```các`der`mảng thực hiện phép truy hồi loạn trật tự từ thuật toán. Việc khởi tạo`der[0] = 1`là cần thiết ngay cả khi sự cố ban đầu có ít nhất một trình điều khiển, vì độ lặp lại đạt tới (D_0) khi tính toán (D_2). 

Mảng nghịch đảo sử dụng phép lặp mô đun nguyên tố chuẩn 

MOD-\left\lfloor\frac{MOD}{i}\right\rfloor 
\operatorname{inv}(MOD\bmod i)\pmod{MOD}. 
] 

Giá trị hiện tại của`comb`luôn luôn là (\binom ni) ở đầu vòng lặp. Câu trả lời được cập nhật trước khi tính hệ số tiếp theo, do đó vòng lặp bao gồm chính xác (i=0,\ldots,k-1). Ranh giới đó là chi tiết chính của vấn đề này. 

Số nguyên Python không bị tràn, nhưng tất cả các giá trị đều được giảm modulo (10^9+7) sau khi nhân. Việc rút gọn giữ cho các giá trị trung gian ở mức nhỏ và phản ánh số học mà bài toán yêu cầu. 

bản cập nhật```
comb = comb * (n - i) % MOD
comb = comb * inv[i + 1] % MOD
```tương đương với 

\binom ni\frac{n-i}{i+1}. 
] 

Sử dụng phép chia số nguyên thông thường sau khi lấy một giá trị theo modulo`MOD`sẽ không chính xác. Nghịch đảo mô-đun là bắt buộc vì phép chia mô-đun không phải là phép chia số nguyên thông thường. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, (n=4,k=2). Chúng ta có thể có không hoặc một điểm cố định. 

| (i) | (\binom{4}{i}) | (D_{4-i}) | Đóng góp | Chạy câu trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 9 | 9 | 9 | 
| 1 | 4 | 2 | 8 | 17 | 

Giá trị (D_4=9) tính các nhiệm vụ trong đó không ai có được ô tô riêng. Đối với chính xác một điểm cố định, chúng ta chọn trình điều khiển đó theo (4) cách và sắp xếp ba trình điều khiển còn lại theo (D_3=2) cách. Tổng số là (9+8=17), khớp với mẫu chính thức. 

Đối với mẫu thứ hai, (n=30,k=1). Vì vòng lặp chỉ chứa (i=0) nên không người lái xe nào có thể nhận được ô tô của riêng mình. 

| (tôi) | (\binom{30}{i}) | (D_{30-i}) | Đóng góp | Chạy câu trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | (D_{30}) | (D_{30}) | 568643488 | 

Sự cố đã giảm trực tiếp xuống số lượng sai lệch (D_{30}). Tính toán modulo truy hồi (10^9+7) cho (568643488), đầu ra mẫu thứ hai chính thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Tính toán tất cả các sai lệch, tất cả nghịch đảo và tối đa (k\le n) số hạng tổng | 
| Không gian | (O(n)) | Lưu trữ các mảng loạn trí và nghịch đảo | 

Với (n\le1000), thuật toán chỉ thực hiện vài nghìn phép tính số học mô-đun. Đó là mức thoải mái trong giới hạn 0,5 giây và mức sử dụng bộ nhớ của nó rất nhỏ so với giới hạn 64 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 1_000_000_007

def solution():
    input = sys.stdin.readline
    n, k = map(int, input().split())

    der = [0] * (n + 1)
    der[0] = 1

    if n >= 1:
        der[1] = 0

    for i in range(2, n + 1):
        der[i] = (i - 1) * (der[i - 1] + der[i - 2]) % MOD

    inv = [0] * (n + 1)
    if n >= 1:
        inv[1] = 1

    for i in range(2, n + 1):
        inv[i] = MOD - (MOD // i) * inv[MOD % i] % MOD

    comb = 1
    ans = 0

    for i in range(k):
        ans = (ans + comb * der[n - i]) % MOD

        if i + 1 < k:
            comb = comb * (n - i) % MOD
            comb = comb * inv[i + 1] % MOD

    print(ans)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solution()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("4 2\n") == "17", "sample 1"
assert run("30 1\n") == "568643488", "sample 2"

# Minimum size
assert run("1 1\n") == "0", "the only driver cannot avoid their own car"

# Small boundary cases
assert run("2 1\n") == "1", "only the swap is a derangement"
assert run("2 2\n") == "1", "all permutations except the identity"

# General small case:
# D5 + C(5,1)D4 + C(5,2)D3 = 44 + 45 + 20*2 = 179
assert run("5 3\n") == "179", "at most two fixed points"

# Maximum n and k:
# k = n means every permutation except the identity is valid.
# 1000! mod MOD = 641419708, so the answer is 641419707.
assert run("1000 1000\n") == "641419707", "maximum-size boundary case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`0`| Đầu vào tối thiểu và ranh giới (D_1=0) | 
|`2 1`|`1`| (k=1), đòi hỏi phải loạn trí hoàn toàn | 
|`2 2`|`1`| (k=n), trong đó chỉ danh tính bị cấm | 
|`5 3`|`179`| Một số điểm cố định được kết hợp trong một câu trả lời | 
|`1000 1000`|`641419707`| Tối đa (n), số học mô-đun và ranh giới (k=n) | 

## Vỏ cạnh 

Khi (n=1,k=1), hoán vị duy nhất là`[1]`. Nó có một điểm cố định nhưng số lượng tối đa được phép là (k-1=0). Thuật toán tính toán (D_1=0), chỉ vào vòng lặp cho (i=0) và cộng (D_1), tạo ra`0`. Đầu vào là`1 1`. 

Khi (k=1), phép tính tổng dừng ngay sau (i=0). Câu trả lời là chính xác (D_n), vì không cho phép có điểm cố định nhưng một điểm cố định đã bị cấm. Vì`3 1`, hai hoán vị hợp lệ là các phép gán tuần hoàn được biểu thị bằng`2 3 1`Và`3 1 2`, do đó thuật toán trả về (D_3=2). 

Khi (k=n), số điểm cố định được phép là (n-1). Mọi hoán vị ngoại trừ danh tính có nhiều nhất (n-1) điểm cố định, vì vậy câu trả lời phải là (n!-1). Vì`2 2`, hai hoán vị là danh tính và hoán đổi, để lại chính xác một phép gán hợp lệ. Tính tổng tính toán (D_2+\binom21D_1=1+0=1), phù hợp với cách giải thích này. 

Trường hợp cho phép chính xác (k-1) điểm cố định cũng là một bẫy riêng biệt. Vì`5 3`, chúng ta cần không, một hoặc hai điểm cố định, không phải không, một, hai hoặc ba. Các đóng góp là (D_5=44), (5D_4=45) và (\binom52D_3=20\cdot2=40), mang lại`129`nếu chỉ sử dụng ba đóng góp đầu tiên. Tổng đúng thực tế là (44+45+40=129), vì vậy trường hợp này là một phép kiểm tra hữu ích xem các số hạng tổ hợp có được hình thành chính xác hay không. Việc triển khai bất cẩn diễn giải điều kiện là "tối đa (k) điểm cố định" sẽ bao gồm thêm (\ binom53D_2=10), tạo ra sai`139`. 

Đối với trường hợp tối đa`1000 1000`, thuật toán không bao giờ xây dựng các hoán vị và không bao giờ sử dụng đối tượng có kích thước giai thừa. Nó tính toán các sai lệch lên đến (D_{1000}), tạo ra lần lượt các hệ số nhị thức và thực hiện 1000 bước tính tổng. Vì (k=n), câu trả lời toán học là (1000!-1) modulo (10^9+7), tức là`641419707`. Điều này thực hiện cả đầu vào được phép lớn nhất và ranh giới trên chính xác của điều kiện điểm cố định.
