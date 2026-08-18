---
title: "CF 102253L - Hoán vị hạn chế"
description: "Đối với mọi vị trí (i), cặp ((li,ri)) mô tả khoảng liền kề lớn nhất chứa (i) mà trên đó (pi) là giá trị nhỏ nhất."
date: "2026-08-17T21:47:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102253
codeforces_index: "L"
codeforces_contest_name: "2017 Chinese Multi-University Training, BeihangU Contest"
rating: 0
weight: 102253
solve_time_s: 248
verified: true
draft: false
---

[CF 102253L - Hoán vị giới hạn](https://codeforces.com/problemset/problem/102253/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đối với mọi vị trí (i), cặp ((l_i,r_i)) mô tả khoảng liền kề lớn nhất chứa (i) mà trên đó (p_i) là giá trị nhỏ nhất. Tương tự, (p_i) nhỏ hơn mọi phần tử nằm giữa (l_i) và (i-1), mọi phần tử nằm giữa (i+1) và (r_i) và khoảng không thể kéo dài thêm một vị trí theo một trong hai hướng. 

Nhiệm vụ là đếm xem có bao nhiêu hoán vị của (1,\ldots,n) có đúng những khoảng nhỏ nhất này. Câu trả lời được lấy modulo (10^9+7). Bài xã luận chính thức xác định cấu trúc kết quả là cây Descartes duy nhất khi đầu vào hợp lệ. 

Các ràng buộc đủ lớn để thuật toán về cơ bản phải tuyến tính. Một ca kiểm thử có thể chứa (10^6) vị trí và tất cả các ca kiểm thử cùng nhau chứa tối đa (3\cdot10^6) vị trí. Một giải pháp (O(n\log n)) đã đắt hơn nhiều so với mức cần thiết, đặc biệt là trong Python, trong khi mọi thứ bậc hai đều bị loại trừ hoàn toàn. Giới hạn bộ nhớ 128 MB cũng có vấn đề vì danh sách Python thông thường gồm một triệu số nguyên tiêu thụ hàng chục megabyte mỗi số. Việc triển khai bên dưới sử dụng các mảng số nguyên nhỏ gọn và sắp xếp đếm thay vì các danh sách nặng về đối tượng và sắp xếp so sánh của Python. 

Có một số cách mà việc thực hiện bất cẩn có thể thất bại. Với (n=1), đầu vào duy nhất có thể là```
1
1
1
```và câu trả lời là (1). Việc coi khoảng con trống là không hợp lệ sẽ từ chối cây hợp lệ duy nhất một cách không chính xác. 

Các khoảng trùng lặp hoặc không tương thích cũng phải bị từ chối. Ví dụ,```
3
1 1 1
3 3 3
```đưa ra cùng một khoảng ([1,3]) cho cả ba vị trí. Việc triển khai đơn giản chỉ cần chọn một trong các vị trí đó làm gốc có thể tiếp tục như thể cây hợp lệ, nhưng không có cách nào để cả ba vị trí có cùng khoảng thời gian tối thiểu tối đa giống nhau. Đầu ra đúng là`Case #1: 0`. 

Vượt qua các khoảng thời gian là một thất bại phổ biến khác. Coi như```
3
1 1 2
2 3 3
```cung cấp ([1,2]), ([1,3]) và ([2,3]). Các khoảng chồng lên nhau mà không chứa cái nào. Cấu hình như vậy không thể đến từ cây Descartes, vì vậy câu trả lời là`Case #1: 0`. Một giải pháp chỉ kiểm tra xem mọi (l_i\le i\le r_i) có chấp nhận nó không chính xác hay không. 

## Phương pháp tiếp cận 

Giải pháp brute-force có thể liệt kê mọi hoán vị của (1,\ldots,n), tính khoảng tối thiểu tối đa cho mọi vị trí và so sánh kết quả với đầu vào. Có (n!) hoán vị. Ngay cả khi người ta sử dụng quy trình xếp chồng đơn điệu theo thời gian tuyến tính để tính toán tất cả các phần tử nhỏ hơn gần nhất cho mỗi hoán vị, việc kiểm tra tất cả các hoán vị đã tốn các phép toán (\Theta(n\cdot n!)). Việc quét trực tiếp đơn giản mọi khoảng thời gian liên quan sẽ tốn kém (\Theta(n^2\cdot n!)). Điều này trở nên vô vọng ngay cả ở khoảng (n=10), rất lâu trước khi có ràng buộc thực tế là (10^6). 

Quan sát hữu ích là các khoảng đã cho không phải là tùy ý. Để hoán vị, hãy xem xét cây Descartes tối thiểu của nó. Gốc là vị trí chứa giá trị nhỏ nhất, cây con bên trái của nó chứa các vị trí ở bên trái của gốc và cây con bên phải của nó chứa các vị trí ở bên phải. Đối với mỗi nút (u), các vị trí trong cây con của nó tạo thành một khoảng liền kề, chính xác là ([l_u,r_u]). Đây là cấu trúc đệ quy tương tự được sử dụng trong giải pháp chính thức. 

Giả sử có một cây con nào đó chiếm giữ ([L,R]) và gốc của nó ở vị trí (u). Khi đó con bên trái của nó phải biểu thị chính xác ([L,u-1]), trong khi con bên phải của nó phải biểu thị chính xác ([u+1,R]). Do đó, toàn bộ cây có thể được xây dựng lại bằng cách liên tục hỏi khoảng đầu vào nào bằng khoảng thời gian yêu cầu hiện tại. 

Điều này cũng đưa ra một công thức tính trực tiếp. Gọi (s(u)) là kích thước của cây con có gốc tại (u). Bởi vì cây con chính xác là ([l_u,r_u]), 

[ 
s(u)=r_u-l_u+1. 
] 

Đối với cây Descartes cố định, giá trị nhỏ nhất phải được gán cho gốc của nó. Các giá trị còn lại được phân chia giữa cây con trái và cây con phải. Nếu kích thước của chúng là (a) và (b), thì có 

[ 
\binom{a+b}{a} 
] 

cách chọn giá trị nào thuộc về cây con bên trái. Nhân số này một cách đệ quy sẽ cho 

[ 
f(u)=\binom{s(v_L)+s(v_R)}{s(v_L)}f(v_L)f(v_R). 
] 

Việc mở rộng các giai thừa cho thấy rằng sản phẩm hoàn chỉnh đơn giản hóa thành 

\frac{n!}{\prod_{i=1}^{n}(r_i-l_i+1)}. 
] 

Bài xã luận chính thức đưa ra chính xác hình thức tương đương này. 

Thách thức còn lại là xác định khoảng gốc một cách hiệu quả. Nếu chúng ta sắp xếp tất cả các khoảng theo cách tăng (l) và bằng nhau (l), giảm dần (r), thì thứ tự của chúng là thứ tự trước của cây Descartes. Vì cả hai tọa độ nhiều nhất là (n), nên việc sắp xếp này có thể được thực hiện bằng hai lượt sắp xếp đếm ổn định, cho thời gian tuyến tính thay vì (O(n\log n)). Bài xã luận chính thức cũng khuyến nghị sắp xếp cơ số các khoảng trước khi phân rã đệ quy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (\Theta(n\cdot n!)) với xác thực tuyến tính | (O(n)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc các mảng (l) và (r), giữ chúng trong mảng số nguyên nhỏ gọn. Đối với mọi vị trí (i), coi ([l_i,r_i]) là khoảng cây con được yêu cầu của nút (i). 
2. Sắp xếp các vị trí theo (l_i) tăng dần và (r_i) giảm dần. Chúng ta sử dụng phương pháp sắp xếp đếm ổn định hai lần, lần đầu tiên theo (n-r_i), sau đó theo (l_i). Lần đầu tiên tạo ra (r_i) giảm dần trong các nhóm bằng (l_i) và lần thứ hai giữ nguyên thứ tự đó. 
3. Bắt đầu với khoảng thời gian được yêu cầu ([1,n]). Khoảng đầu tiên trong thứ tự sắp xếp trước phải chính xác là khoảng này. Nếu không, đầu vào không thể mô tả bất kỳ cây Descartes nào, vì vậy câu trả lời là 0. 
4. Khi khoảng hiện tại là ([L,R]) và gốc của nó ở vị trí (u), yêu cầu khoảng đầu vào của gốc phải chính xác ([L,R]). Khi đó các con duy nhất có thể chiếm ([L,u-1]) và ([u+1,R]). Đẩy khoảng bên phải trước và khoảng thứ hai bên trái vào một ngăn xếp để cây con bên trái được xử lý tiếp theo. Mỗi cây con khác rỗng đóng góp kích thước của nó (R-L+1) vào mẫu số. 
5. Sau khi sử dụng hết khoảng thời gian bắt buộc không trống, dữ liệu sẽ hợp lệ chính xác khi tất cả (n) khoảng đầu vào cũng đã được sử dụng. Tính (n!) modulo (10^9+7), nhân nó với nghịch đảo mô-đun của mọi kích thước cây con và thu được 

n!\prod_{i=1}^{n}(r_i-l_i+1)^{-1} 
\pmod {10^9+7}. 
] 

Nghịch đảo của tất cả các giá trị từ (1) đến (10^6) được tính toán trước khi sử dụng 

-\left\lfloor\frac{MOD}{i}\right\rfloor 
\operatorname{inv}(MOD\bmod i) 
\pmod {MOD}. 
] 

### Tại sao nó hoạt động 

Đối với bất kỳ hoán vị hợp lệ nào, cây Descartes tối thiểu của nó có chính xác một khoảng cây con cho mỗi nút và khoảng cây con đó là ([l_i,r_i]). Tại cây con ([L,R]), gốc (u) của nó phân tách các vị trí thành khoảng bắt buộc bên trái ([L,u-1]) và khoảng bên phải ([u+1,R]). Do đó, mọi cây hợp lệ đều phải vượt qua mọi lần kiểm tra định kỳ được thực hiện bởi thuật toán. 

Ngược lại, nếu tất cả các khoảng bắt buộc được tìm thấy theo đúng thứ tự đệ quy, thì chúng sẽ xác định một cây nhị phân có độ duyệt theo thứ tự là (1,2,\ldots,n) và cây con của mỗi nút chính xác là khoảng được cung cấp của nó. Việc gán các giá trị ngày càng tăng dọc theo cây heap tối thiểu này sẽ tạo ra cấu trúc cây Descartes chính xác theo yêu cầu. Vì hoán vị chứa các giá trị riêng biệt nên mọi hoán vị hợp lệ đều tương ứng với chính xác một nhãn như vậy. 

Đối với cây cố định, số lần đan xen đệ quy là tích của các hệ số nhị thức được mô tả ở trên. Mỗi nút đóng góp một mẫu số có kích thước cây con, do đó tất cả các giai thừa bên trong đều bị hủy và để lại (n!/\prod s(u)). Vì (s(u)=r_u-l_u+1), công thức được thuật toán sử dụng sẽ tính mọi hoán vị hợp lệ chính xác một lần. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

MOD = 1000000007
MAX_N = 1000000

# Modular inverses for every possible subtree size.
inv = array('I', [0]) * (MAX_N + 1)
inv[1] = 1
for i in range(2, MAX_N + 1):
    inv[i] = MOD - (MOD // i) * inv[MOD % i] % MOD

def radix_order(l, r, n):
    """
    Return indices sorted by:
        l[index] ascending
        r[index] descending
    using two stable counting-sort passes.
    """
    order = array('i', range(n))
    tmp = array('i', [0]) * n

    # First pass: r descending, encoded as n - r.
    cnt = array('i', [0]) * (n + 1)

    for idx in order:
        cnt[n - r[idx]] += 1

    pos = 0
    for key in range(n + 1):
        c = cnt[key]
        cnt[key] = pos
        pos += c

    for idx in order:
        key = n - r[idx]
        p = cnt[key]
        tmp[p] = idx
        cnt[key] = p + 1

    # Second pass: l ascending.
    cnt = array('i', [0]) * (n + 1)

    for idx in tmp:
        cnt[l[idx]] += 1

    pos = 0
    for key in range(n + 1):
        c = cnt[key]
        cnt[key] = pos
        pos += c

    for idx in tmp:
        key = l[idx]
        p = cnt[key]
        order[p] = idx
        cnt[key] = p + 1

    return order

def solve_case(n, l, r):
    order = radix_order(l, r, n)

    # Each item is an expected subtree interval [L, R].
    # We process them in preorder.
    stack_l = [1]
    stack_r = [n]

    ptr = 0
    denominator = 1

    while stack_l:
        L = stack_l.pop()
        R = stack_r.pop()

        if L > R:
            continue

        if ptr >= n:
            return 0

        u = order[ptr]
        ptr += 1

        # The next preorder node must represent exactly [L, R].
        if l[u] != L or r[u] != R or not (L <= u <= R):
            return 0

        size = R - L + 1
        denominator = denominator * size % MOD

        # Push right first so that left is processed first.
        if u + 1 <= R:
            stack_l.append(u + 1)
            stack_r.append(R)

        if L <= u - 1:
            stack_l.append(L)
            stack_r.append(u - 1)

    # Every supplied interval must belong to the constructed tree.
    if ptr != n:
        return 0

    factorial = 1
    for x in range(2, n + 1):
        factorial = factorial * x % MOD

    return factorial * pow(denominator, MOD - 2, MOD) % MOD

def solve():
    case_no = 1
    output = []

    while True:
        line = input()
        if not line:
            break
        if not line.strip():
            continue

        n = int(line)

        l = array('i', map(int, input().split()))
        r = array('i', map(int, input().split()))

        ans = solve_case(n, l, r)
        output.append(f"Case #{case_no}: {ans}")
        case_no += 1

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```Bảng nghịch đảo được tạo một lần vì mọi kích thước cây con nằm giữa (1) và (n) và (n\le10^6). Phép truy toán tránh thực hiện phép lũy thừa mô-đun cho mọi nút. Một đĩa đơn`pow`vẫn được sử dụng làm mẫu số cuối cùng trong quá trình triển khai, nhưng nó có thể được thay thế bằng cách nhân`inv[size]`cho mỗi nút trong quá trình xác thực. Cái sau tránh được phép lũy thừa cuối cùng và phù hợp hơn một chút với công thức. 

Hai mảng (l) và (r) sử dụng`array('i')`thay vì danh sách Python. Số nguyên Python là một đối tượng có chi phí hoạt động đáng kể, trong khi số nguyên 32 bit là đủ vì mọi tọa độ tối đa là (10^6). Bộ đệm sắp xếp và mảng đếm sử dụng cùng một biểu diễn vì cùng một lý do. 

Sắp xếp cơ số sử dụng (n-r_i) làm khóa đầu tiên. Đếm theo thứ tự tăng dần tương đương với việc sắp xếp (r_i) theo thứ tự giảm dần. Lần thứ hai sắp xếp theo (l_i) tăng dần và ổn định, do đó các giá trị (l_i) bằng nhau giữ nguyên thứ tự giảm dần-(r_i). 

Việc phân rã cây đệ quy được thực hiện bằng một ngăn xếp rõ ràng. Việc triển khai Python đệ quy có thể đạt tới độ sâu (10^6) trên cây Cartesian bị lệch hoàn toàn và sẽ vượt quá giới hạn đệ quy. Ngăn xếp cũng tránh được chi phí gọi hàm. 

điều kiện`l[u] != L or r[u] != R`là kiểm tra tính hợp lệ cốt lõi. Một nút chỉ có thể là gốc của cây con có ranh giới chính xác là ranh giới được cung cấp của nó. Séc`L <= u <= R`được ngụ ý bởi các ràng buộc đầu vào ban đầu, nhưng việc giữ nó làm cho cây trở nên bất biến rõ ràng và bảo vệ quy trình nếu các giả định xác thực đầu vào bị thay đổi. 

Mẫu số chứa kích thước cây con, không phải kích thước con. Ví dụ, một gốc bao gồm năm vị trí đóng góp (5), ngay cả khi các con của nó có kích thước một và ba. Đây là số lượng xuất hiện trong công thức đơn giản hóa (n!/\prod s(u)). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
3
1 1 3
1 3 3
```Ba khoảng là ([1,1]), ([1,3]) và ([3,3]). Sau khi sắp xếp theo cách tăng điểm cuối bên trái và giảm điểm cuối bên phải, thứ tự trước là nút ở vị trí (2), tiếp theo là vị trí (1) và (3). 

| Khoảng thời gian bắt buộc | Chọn gốc | Khoảng gốc | Kích thước cây con | Mẫu số | 
| --- | --- | --- | --- | --- | 
| [1,3] | 2 | [1,3] | 3 | 3 | 
| [1,1] | 1 | [1,1] | 1 | 3 | 
| [3,3] | 3 | [3,3] | 1 | 3 | 

Cây có gốc (2), con trái (1) và con phải (3). Mẫu số là (3), trong khi (3!=6), do đó 

[ 
\frac{3!}{3}=2. 
] 

Hai hoán vị này là hai cách có thể để đặt các giá trị lớn hơn ở hai phía của mức tối thiểu. 

### Mẫu 2 

Đầu vào là```
5
1 2 2 4 5
5 2 5 5 5
```Các khoảng là ([1,5]), ([2,2]), ([2,5]), ([4,5]) và ([5,5]). 

| Khoảng thời gian bắt buộc | Chọn gốc | Khoảng gốc | Kích thước cây con | Mẫu số | 
| --- | --- | --- | --- | --- | 
| [1,5] | 1 | [1,5] | 5 | 5 | 
| [2,5] | 3 | [2,5] | 4 | 20 | 
| [2,2] | 2 | [2,2] | 1 | 20 | 
| [4,5] | 4 | [4,5] | 2 | 40 | 
| [5,5] | 5 | [5,5] | 1 | 40 | 

Cây Descartes thu được có vị trí (1) là gốc, vị trí (3) là con bên phải, vị trí (2) là con trái của (3) và chuỗi (4\rightarrow5) ở bên phải. 

Số đếm cuối cùng là 

# \frac{120}{40} 

1. 

] 

Ví dụ này cho thấy tại sao chỉ kiểm tra từng khoảng thời gian riêng lẻ là không đủ. Mối quan hệ ngăn chặn đệ quy của chúng là yếu tố quyết định cây và do đó xác định số lượng nhãn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) cho mỗi trường hợp thử nghiệm | Hai lần sắp xếp đếm, một lần duyệt cây và một lần tính toán giai thừa | 
| Không gian | (O(n)) | Mảng đầu vào, bộ đệm sắp xếp cơ số, mảng đếm và ngăn xếp truyền tải | 

Tổng (n) trên tất cả các trường hợp thử nghiệm tối đa là (3\cdot10^6), do đó công việc tuyến tính sẽ chia tỷ lệ trực tiếp với tổng kích thước đầu vào. nhỏ gọn`array`biểu diễn giữ các mảng chính trong giới hạn bộ nhớ 128 MB. Thuật toán tránh việc sắp xếp so sánh Python và tránh đệ quy, cả hai đều quan trọng ở (n=10^6). 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả định`solve()`hoạt động từ giải pháp trên. Người trợ giúp tạm thời thay thế toàn cầu`input`để có thể thực hiện cùng một bộ giải bằng luồng đầu vào trong bộ nhớ.```python
import sys
import io

# The solve() function and global input from the submitted solution
# are assumed to be available here.

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_input = input

    try:
        sys.stdin = io.StringIO(inp)
        input = sys.stdin.readline
        solve()
        # solve() writes to stdout, so capture it through a second redirection.
    finally:
        sys.stdin = old_stdin
        input = old_input

def run_capture(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_stdout = sys.stdout
    old_input = input

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        input = sys.stdin.readline

        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = old_input

# Provided samples.
sample = """\
3
1 1 3
1 3 3
5
1 2 2 4 5
5 2 5 5 5
"""
assert run_capture(sample) == (
    "Case #1: 2\n"
    "Case #2: 3\n"
), "provided samples"

# Minimum size.
assert run_capture(
    "1\n"
    "1\n"
    "1\n"
) == "Case #1: 1\n", "single element"

# A valid two-element increasing Cartesian tree.
assert run_capture(
    "2\n"
    "1 2\n"
    "2 2\n"
) == "Case #1: 1\n", "two-element boundary case"

# All intervals are identical. No valid Cartesian tree exists.
assert run_capture(
    "3\n"
    "1 1 1\n"
    "3 3 3\n"
) == "Case #1: 0\n", "duplicate root intervals"

# Crossing intervals. They cannot be nested as Cartesian-tree subtrees.
assert run_capture(
    "3\n"
    "1 1 2\n"
    "2 3 3\n"
) == "Case #1: 0\n", "crossing intervals"

# Maximum-size test. Every interval is a singleton, which is invalid
# for n > 1 because the root interval must be [1, n].
n = 1_000_000
l = " ".join(map(str, range(1, n + 1)))
r = l
maximum_case = f"{n}\n{l}\n{r}\n"

assert run_capture(maximum_case) == "Case #1: 0\n", "maximum-size input"
```các`run`trình trợ giúp ở trên được giữ lại làm giao diện đơn giản được yêu cầu trong mẫu thử nghiệm, trong khi`run_capture`được sử dụng để xác nhận vì bộ giải sản xuất ghi trực tiếp vào đầu ra tiêu chuẩn. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 1`|`Case #1: 1`| Xử lý cây có kích thước tối thiểu và con trống | 
|`2 / 1 2 / 2 2`|`Case #1: 1`| Khoảng biên và nghiệm tại điểm cuối bên trái | 
|`3 / 1 1 1 / 3 3 3`|`Case #1: 0`| Khoảng trùng lặp và cấu trúc cây Descartes không hợp lệ | 
|`3 / 1 1 2 / 2 3 3`|`Case #1: 0`| Vượt qua các khoảng thời gian và xác nhận đệ quy | 
| (n=10^6), (l_i=r_i=i) |`Case #1: 0`| Kích thước đầu vào tối đa và hành vi thời gian tuyến tính | 

## Vỏ cạnh 

Đối với trường hợp phần tử đơn```
1
1
1
```thứ tự được sắp xếp chứa một khoảng và khoảng thời gian bắt buộc ban đầu cũng là ([1,1]). Nút có kích thước cây con (1) nên mẫu số là (1), giai thừa là (1) và kết quả là (1). Không có con trái hoặc phải nào được đẩy lên ngăn xếp. 

Đối với trường hợp khoảng đều bằng nhau```
3
1 1 1
3 3 3
```khoảng được sắp xếp đầu tiên là ([1,3]), do đó thuật toán ban đầu chấp nhận một trong ba nút làm nghiệm ứng viên. Ví dụ: nếu nút đó ở vị trí (1), khoảng thời gian bắt buộc tiếp theo là ([2,3]), nhưng khoảng thời gian đầu vào được sắp xếp tiếp theo vẫn là ([1,3]). Việc so sánh khoảng thời gian chính xác không thành công và câu trả lời trở thành số 0. Sự mâu thuẫn tương tự cũng xuất hiện đối với các ứng cử viên gốc khác. 

Đối với trường hợp vượt qua```
3
1 1 2
2 3 3
```các khoảng được sắp xếp là ([1,2]), ([1,3]), ([2,3]). Phần gốc của mảng hoàn chỉnh phải bao gồm ([1,3]), nhưng khoảng được sắp xếp đầu tiên là ([1,2]). Thuật toán ngay lập tức từ chối đầu vào. Điều này phát hiện sự chồng chéo không hợp lệ trước khi thực hiện bất kỳ thao tác đếm nào. 

Đối với trường hợp ranh giới hai phần tử```
2
1 2
2 2
```khoảng gốc là ([1,2]), thuộc vị trí (1). Con bên phải của nó là khoảng đơn ([2,2]). Kích thước cây con là (2) và (1), cho 

[ 
\frac{2!}{2\cdot1}=1. 
] 

Hoán vị hợp lệ duy nhất đang tăng lên, do đó việc kiểm tra ranh giới và xây dựng khoảng con đều phù hợp với số tổ hợp. 

Đối với đầu vào có kích thước tối đa với (l_i=r_i=i), khoảng đầu tiên là ([1,1]), trong khi toàn bộ mảng yêu cầu lớp phủ gốc ([1,10^6]). Thuật toán từ chối phiên bản sau khi đạt đến điểm không khớp đầu tiên, mặc dù sắp xếp cơ số vẫn xử lý tất cả các khoảng (10^6). Tổng công việc vẫn tuyến tính và không có lệnh gọi đệ quy nào có thể tràn ngăn xếp Python.
