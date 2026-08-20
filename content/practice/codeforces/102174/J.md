---
title: "CF 102174J - \u91d1\u8272\u4f20\u8bf4"
description: "Chúng ta cần tổng các giá trị của mọi biểu thức hợp lệ có chính xác (n) ký tự. Một biểu thức bắt đầu và kết thúc bằng một chữ số, các toán tử không bao giờ xuất hiện cạnh nhau và các toán tử duy nhất là + và -."
date: "2026-08-19T07:08:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102174
codeforces_index: "J"
codeforces_contest_name: "The 14-th BIT Campus Programming Contest"
rating: 0
weight: 102174
solve_time_s: 115
verified: true
draft: false
---

[CF 102174J - \u91d1\u8272\u4f20\u8bf4](https://codeforces.com/problemset/problem/102174/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần tổng các giá trị của mọi biểu thức hợp lệ có chính xác (n) ký tự. Một biểu thức bắt đầu và kết thúc bằng một chữ số, các toán tử không bao giờ xuất hiện cạnh nhau và các toán tử duy nhất là`+`Và`-`. Cho phép các số 0 đứng đầu, vì vậy khối chữ số tối đa được hiểu đơn giản là số nguyên thập phân thông thường, ngay cả khi nó bắt đầu bằng số 0. Câu trả lời bắt buộc là tổng của tất cả các biểu thức được đánh giá modulo (998244353). Tuyên bố chính thức và các mẫu có sẵn trên Codeforces. 

Khó khăn không phải là đánh giá một biểu thức. Một chuỗi có độ dài-(n) có nhiều khả năng theo cấp số nhân và thậm chí việc kiểm tra tất cả các chuỗi cũng đã vô vọng khi (n) đạt tới (5\times10^5). Với tối đa 500 trường hợp thử nghiệm có cùng giới hạn trên, một giải pháp có thể chấp nhận được về cơ bản cần tiền xử lý tuyến tính trong (n) lớn nhất, sau đó là công việc không đổi cho mỗi trường hợp thử nghiệm. Một lần lặp lại (O(n^2)) sẽ yêu cầu khoảng (2,5\times10^{11}) thao tác ở kích thước đầu vào lớn nhất, vượt xa giới hạn một giây. 

Có một số trường hợp ranh giới trong đó một sự tái diễn dường như tự nhiên có thể xảy ra sai sót. Với (n=1), không có toán tử nào có thể xảy ra, vì thế mười biểu thức`0`bởi vì`9`đóng góp (45), không phải bằng 0 hoặc (10). Đối với (n=2), một toán tử vẫn không thể xảy ra vì nó sẽ phải chiếm một trong hai điểm cuối, vì vậy câu trả lời là tổng của tất cả các chuỗi có hai chữ số, (4950). Phép lặp giả sử mọi biểu thức đều có toán tử sẽ thất bại trong cả hai trường hợp. 

Một lỗi phổ biến khác là chỉ đếm các khối chữ số sau toán tử đầu tiên. Ví dụ: với (n=3), các biểu thức như`1+2`Và`1-2`cả hai đều tồn tại. Đóng góp tổng hợp của chúng là (3+(-1)=2), do đó các giá trị hậu tố bị hủy trong khi giá trị tiền tố vẫn giữ nguyên hai lần. Sự hủy bỏ này là quan sát quan trọng đằng sau sự tái diễn hiệu quả. 

Số 0 đứng đầu cũng có vấn đề. Với (n=2),`00`là một biểu thức hợp lệ và đóng góp bằng 0, trong khi`09`là một biểu thức hợp lệ và đóng góp chín. Việc xử lý khối chữ số như một chuỗi thông thường thay vì yêu cầu chữ số đầu tiên của nó khác 0 là cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Tạo mọi chuỗi có độ dài (n) trên mười hai ký tự có sẵn, loại bỏ các chuỗi vi phạm quy tắc cú pháp, đánh giá từng biểu thức còn lại và thêm giá trị của nó. Có (12^n) chuỗi thô trước khi kiểm tra tính hợp lệ, vì vậy với (n=5\times10^5) số lượng ứng cử viên là rất lớn. Ngay cả việc tạo ra tất cả các biểu thức hợp lệ cũng phải theo cấp số nhân, bởi vì một biểu thức hợp lệ có thể chọn một toán tử hoặc một chữ số ở nhiều vị trí. Cách tiếp cận này đúng vì nó trực tiếp liệt kê chính xác các đối tượng có giá trị mà chúng ta cần, nhưng nó gần như không thể sử dụng được ngay lập tức. 

Một cách tiếp cận hứa hẹn hơn là tách một biểu thức ở toán tử đầu tiên của nó. Giả sử toán tử đầu tiên xuất hiện sau (i) ký tự. Tiền tố (A) bản thân nó là biểu thức hợp lệ của độ dài (i), trong khi hậu tố (B) chỉ chứa các chữ số, vì việc phân tách được thực hiện ở toán tử đầu tiên. Do đó, hậu tố có chính xác (10^{n-i-1}) chuỗi có thể. 

Với mỗi cặp cố định (A,B), có hai biểu thức (A+B) và (A-B). Giá trị của chúng thêm vào 

[ 
(A+B)+(A-B)=2A. 
] 

Hậu tố biến mất hoàn toàn khỏi tổng. Đây là sự đơn giản hóa trung tâm: để tính toán đóng góp của mọi biểu thức có toán tử đầu tiên ở một vị trí cụ thể, chúng ta chỉ cần tổng giá trị của tiền tố và số hậu tố chỉ có chữ số có thể có. 

Đặt (F_n) là tổng giá trị của tất cả các biểu thức hợp lệ có độ dài (n). Đặt (P_n) là tổng của tất cả các chuỗi (n) chữ số, trong đó cho phép các số 0 đứng đầu. Có (10^n) chuỗi như vậy và mỗi chữ số xuất hiện thường xuyên như nhau ở mọi vị trí, cho 

[ 
P_n=\frac{10^n(10^n-1)}2. 
] 

Các biểu thức không có toán tử đóng góp chính xác (P_n). Đối với các biểu thức có toán tử đầu tiên xuất hiện sau tiền tố có độ dài (i), phần đóng góp tổng hợp của`+`Và`-`phiên bản là 

[ 
2F_i\cdot10^{n-i-1}. 
] 

Do đó 

[ 
F_n=P_n+2\sum_{i=1}^{n-2}F_i10^{n-i-1}. 
] 

Vấn đề còn lại là tích chập trong công thức này. Chúng ta có thể loại bỏ nó bằng cách xác định 

[ 
S_n=\sum_{i=1}^{n-2}F_i10^{n-i-1}. 
] 

Chuyển từ (S_{n-1}) sang (S_n) sẽ nhân mọi số hạng hiện có với (10) và số hạng mới tương ứng với (i=n-2) là (10F_{n-2}). Như vậy 

[ 
S_n=10S_{n-1}+10F_{n-2}. 
] 

Vì (F_n=P_n+2S_n), nên ta có thể loại bỏ hoàn toàn (S): 

[ 
\bắt đầu{căn chỉnh} 
F_n 
&=P_n+20S_{n-1}+20F_{n-2}\ 
&=P_n+10(F_{n-1}-P_{n-1})+20F_{n-2}. 
\end{căn chỉnh} 
] 

Các thuật ngữ số thuần túy đơn giản hóa một cách độc đáo: 

[ 
P_n-10P_{n-1}=45\cdot10^{2n-2}. 
] 

Do đó sự tái phát cuối cùng là 

[ 
\boxed{F_n=10F_{n-1}+20F_{n-2}+45\cdot10^{2n-2}} 
] 

với 

[ 
F_1=45,\qquad F_2=4950. 
] 

Điều này biến phép liệt kê hàm mũ ban đầu thành phép truy toán tuyến tính. Chúng ta có thể tính toán trước tất cả các câu trả lời cho đến (n) lớn nhất được yêu cầu một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(12^n\cdot n)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(\max n+T)) | (O(\max n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các trường hợp thử nghiệm và tìm độ dài được yêu cầu lớn nhất (N). Chỉ tính toán trước tối đa (N) để tránh thực hiện những công việc không cần thiết đối với đầu vào nhỏ hơn. 
2. Tính toán trước lũy thừa của (10) modulo (998244353). Sự lặp lại cần (10^{2n-2}), do đó, một lựa chọn thuận tiện là duy trì (p_n=10^n) và sử dụng (p_{n-1}^2). 
3. Đặt (F_1=45) và (F_2=4950). Đây là những trường hợp toán tử không thể xuất hiện nên đáp án chỉ đơn giản là tổng của tất cả các chuỗi có một chữ số và hai chữ số. 
4. Với mọi (n\ge3), hãy tính 

[ 
F_n=(10F_{n-1}+20F_{n-2}+45p_{n-1}^2)\bmod998244353. 
] 

Vì (p_{n-1}=10^{n-1}), số hạng cuối cùng chính xác là (45\cdot10^{2n-2}). 
5. Lưu trữ mọi (F_n). Sau đó, mỗi truy vấn có thể được trả lời bằng cách in trực tiếp giá trị được tính toán trước. 

Tại sao nó hoạt động có thể được tóm tắt bằng cách phân tách toán tử đầu tiên. Mọi biểu thức chứa một toán tử đều có chính xác một toán tử đầu tiên. Mọi thứ trước nó là một biểu thức hợp lệ tùy ý (A) và mọi thứ sau nó là một chuỗi chữ số tùy ý (B). Hai lựa chọn của toán tử thứ nhất đóng góp (A+B) và (A-B), có tổng là (2A). Do đó, mọi biểu thức đều được tính chính xác một lần và sự đóng góp của nó được thể hiện bằng phép truy toán. Phép biến đổi đại số từ phép tích chập sang phép truy toán bậc hai bảo toàn cùng một đại lượng, do đó mỗi (F_n) được lưu trữ chính xác là tổng được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    t = int(input())
    queries = [int(input()) for _ in range(t)]
    mx = max(queries)

    ans = [0] * (mx + 1)

    if mx >= 1:
        ans[1] = 45

    if mx >= 2:
        ans[2] = 4950

    pow10 = [1] * (mx + 1)
    for i in range(1, mx + 1):
        pow10[i] = pow10[i - 1] * 10 % MOD

    for n in range(3, mx + 1):
        # 45 * 10^(2n-2) = 45 * (10^(n-1))^2
        pure_term = 45 * pow10[n - 1] % MOD * pow10[n - 1] % MOD

        ans[n] = (
            10 * ans[n - 1]
            + 20 * ans[n - 2]
            + pure_term
        ) % MOD

    sys.stdout.write("\n".join(str(ans[n]) for n in queries))

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc mọi truy vấn trước khi thực hiện lập trình động. Điều này cho phép chương trình xác định độ dài yêu cầu lớn nhất và thực hiện một phép tính trước được chia sẻ, điều này đặc biệt hữu ích vì có thể có hàng trăm trường hợp kiểm thử. 

Mảng`pow10`cửa hàng (10^i\bmod MOD). Tại vị trí (n), số hạng không đồng nhất là (45\cdot10^{2n-2}), được tính như sau`45 * pow10[n - 1] * pow10[n - 1]`. Phép nhân được giảm modulo`MOD`ở mỗi giai đoạn, vì vậy Python không bao giờ cần phải vận dụng sức mạnh chính xác to lớn của số mười. 

Việc tái phát chỉ sử dụng`ans[n - 1]`,`ans[n - 2]`, Và`pow10[n - 1]`. Các cơ sở (F_1) và (F_2) được xử lý riêng biệt vì phép truy toán mô tả các biểu thức sau khi phân rã toán tử đầu tiên và yêu cầu hai độ dài trước đó. 

Không có vấn đề tràn số nguyên trong Python và việc giảm sau mỗi phép nhân sẽ giữ cho các giá trị trung gian ở mức nhỏ. Trong ngôn ngữ có chiều rộng cố định, phép nhân phải được thực hiện với loại số nguyên đủ rộng trước khi lấy mô đun. 

## Ví dụ đã hoạt động 

Đối với truy vấn mẫu đầu tiên, (n=1), thuật toán không nhập phép truy hồi vì không có (F_0) hoặc (F_{-1}). Nó trực tiếp sử dụng giá trị cơ sở. 

| (n) | (F_n) | Lý do | 
| --- | --- | --- | 
| 1 | 45 | Tổng của`0`bởi vì`9`| 

Kết quả là (45), phù hợp với mẫu. Điều này chứng tỏ tại sao trường hợp ranh giới một ký tự cần có sự khởi tạo riêng. 

Với (n=4), sự lặp lại có thể được tìm thấy thông qua các câu trả lời trước đó. 

| (n) | (F_{n-2}) | (F_{n-1}) | (45\cdot10^{2n-2}) | (F_n) | 
| --- | --- | --- | --- | --- | 
| 3 | 45 | 4950 | 45000 | 500400 | 
| 4 | 4950 | 500400 | 4500000 | 50103000 | 

Với (n=4), 

[ 
F_4=10(500400)+20(4950)+4500000=50103000. 
] 

Việc giải thích toán tử đầu tiên cho kết quả tương tự. Chuỗi bốn chữ số thuần túy đóng góp (49.995.000). Nếu toán tử đầu tiên đứng sau một chữ số thì hai lựa chọn của toán tử sẽ đóng góp (2\cdot45\cdot100=9.000). Nếu sau hai chữ số, chúng sẽ đóng góp (2\cdot4950\cdot10=99.000). Tổng số của họ là (49.995.000+9.000+99.000=50.103.000). Sự tái phát đã nén chính xác những đóng góp này. 

Với (n=5), phép truy hồi cho 

[ 
F_5=10F_4+20F_3+45\cdot10^8. 
] 

Sử dụng (F_4=50,103,000) và (F_3=500,400), 

[ 
F_5=501,030,000+10,008,000+4,500,000,000 
= 5.011.038.000. 
] 

Lấy modulo này (998244353) sẽ cho (19,816,235), đây là câu trả lời mẫu thứ năm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\max n+T)) | lũy thừa 10 và tất cả các giá trị lặp lại được tính một lần, sau đó mọi truy vấn đều được trả lời trong (O(1)). | 
| Không gian | (O(\max n)) | Mảng câu trả lời và lũy thừa chứa một mục nhập cho mỗi độ dài cho đến truy vấn lớn nhất. | 

Với (n\le5\times10^5), quá trình tiền xử lý chỉ thực hiện tổng cộng vài triệu phép tính số học mô-đun, thay vì bất kỳ phép tính hàm mũ hoặc bậc hai nào. Mức tiêu thụ bộ nhớ là tuyến tính theo (n), nằm trong giới hạn bộ nhớ đã nêu. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây sử dụng cách triển khai lặp lại tương tự như giải pháp được gửi cho các trường hợp thông thường và phép tính lũy thừa ma trận độc lập cho trường hợp có kích thước tối đa. Cái sau kiểm tra xem phép lặp vẫn đúng ở (n=500000) mà không cần dựa vào hằng số được tính toán trước.```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 998244353

def solve():
    input = sys.stdin.readline

    t = int(input())
    queries = [int(input()) for _ in range(t)]
    mx = max(queries)

    ans = [0] * (mx + 1)

    if mx >= 1:
        ans[1] = 45
    if mx >= 2:
        ans[2] = 4950

    pow10 = [1] * (mx + 1)
    for i in range(1, mx + 1):
        pow10[i] = pow10[i - 1] * 10 % MOD

    for n in range(3, mx + 1):
        pure_term = 45 * pow10[n - 1] % MOD * pow10[n - 1] % MOD
        ans[n] = (
            10 * ans[n - 1]
            + 20 * ans[n - 2]
            + pure_term
        ) % MOD

    sys.stdout.write("\n".join(str(ans[n]) for n in queries))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()
    out = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

def mat_mul(a, b):
    return [
        [
            sum(a[i][k] * b[k][j] for k in range(3)) % MOD
            for j in range(3)
        ]
        for i in range(3)
    ]

def mat_pow(a, e):
    r = [
        [1, 0, 0],
        [0, 1, 0],
        [0, 0, 1],
    ]

    while e:
        if e & 1:
            r = mat_mul(r, a)
        a = mat_mul(a, a)
        e >>= 1

    return r

def max_case_expected(n):
    # State:
    # [F_n, F_(n-1), 10^(2n-2)]
    #
    # F_(n+1) = 10 F_n + 20 F_(n-1) + 45 * 10^(2n)
    #
    # The third component is multiplied by 100.
    trans = [
        [10, 20, 4500],
        [1, 0, 0],
        [0, 0, 100],
    ]

    if n == 1:
        return 45
    if n == 2:
        return 4950

    # At n = 2:
    # state = [F_2, F_1, 10^2]
    p = mat_pow(trans, n - 2)
    state = [4950, 45, 100]

    return sum(p[0][i] * state[i] for i in range(3)) % MOD

# Provided samples
assert run("5\n1\n2\n3\n4\n5\n") == (
    "45\n4950\n500400\n50103000\n19816235"
), "provided samples"

# Minimum size
assert run("1\n1\n") == "45", "n = 1"

# Two-digit boundary, where operators are still impossible
assert run("1\n2\n") == "4950", "n = 2"

# First length where operators can occur
assert run("1\n3\n") == "500400", "n = 3"

# All-equal / leading-zero-sensitive expressions are included at n = 2.
# The answer must still count 00, 11, ..., 99.
assert run("1\n2\n") == "4950", "leading zero and equal digits"

# Maximum allowed n, checked independently with matrix exponentiation
expected_500k = max_case_expected(500000)
actual_500k = int(run("1\n500000\n"))
assert actual_500k == expected_500k, "n = 500000"
assert 0 <= actual_500k < MOD, "modulo boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n`|`45`| Chiều dài tối thiểu và trường hợp cơ sở | 
|`1\n2\n`|`4950`| Hạn chế điểm cuối ngăn cản người vận hành | 
|`1\n3\n`|`500400`| Trường hợp đầu tiên chứa`+`Và`-`| 
|`1\n2\n`|`4950`| Các số 0 đứng đầu và các chuỗi chữ số bằng nhau, chẳng hạn như`00`| 
|`1\n500000\n`| Modulo giá trị ma trận-recurrence (998244353) | Ràng buộc tối đa và tái phát chỉ số lớn | 

## Vỏ cạnh 

Đối với (n=1), chuỗi hợp lệ duy nhất là mười chữ số riêng lẻ. Bộ khởi tạo (F_1=45), là (0+1+\cdots+9). Không có bước lặp lại nào được thử nên không có tham chiếu không hợp lệ đến biểu thức ngắn hơn. 

Đối với (n=2), toán tử sẽ phải chiếm ký tự thứ nhất hoặc thứ hai, cả hai đều bị cấm. Do đó, mọi biểu thức hợp lệ là một chuỗi có hai chữ số từ`00`bởi vì`99`. Việc khởi tạo (F_2=4950) cho kết quả chính xác. 

Với (n=3), trước tiên các toán tử có thể thực hiện được. Các chuỗi chữ số thuần túy đóng góp 

[ 
0+1+\cdots+999=499500. 
] 

Các biểu thức có toán tử có tiền tố một chữ số và hậu tố một chữ số. Đối với mỗi giá trị tiền tố (a), cặp`a+b`Và`a-b`đóng góp (2a). Tổng hợp mười tiền tố và mười hậu tố có thể có 

[ 
2\cdot45\cdot10=900. 
] 

Như vậy 

[ 
F_3=499500+900=500400. 
] 

Đây chính xác là điểm đầu tiên mà việc hủy bỏ toán tử đầu tiên trở nên có liên quan. 

Đối với các biểu thức chứa nhiều toán tử, quá trình phân tách vẫn hoạt động vì chỉ toán tử đầu tiên được chọn. Ví dụ: một biểu thức như`12-3+45`được chia thành (A=12), toán tử`-`và (B=3+45) về mặt khái niệm chỉ khi (B) được phép chứa các toán tử mà nó không nằm trong sự phân rã của chúng tôi. Thay vào đó, toán tử đầu tiên là sau`12`, vậy hậu tố thực sự là khối chữ số`3`, và phần còn lại`+45`chỉ thuộc về cấu trúc phân tách tiền tố khác khi toán tử đầu tiên được chọn sau. Trực tiếp hơn, mỗi biểu thức hoàn chỉnh có một toán tử đầu tiên duy nhất và mọi thứ trước vị trí đó là biểu thức hợp lệ trong khi mọi thứ sau nó không chứa toán tử. Tính duy nhất này ngăn chặn việc tính hai lần. 

Đối với các số 0 đứng đầu,`00`,`007`, Và`00042`đều là các khối chữ số hợp lệ. Thuật ngữ (10^k) tính tất cả các chuỗi có chữ số (k), không phải số nguyên có chính xác (k) chữ số thập phân. Đây là lý do tại sao tổng chuỗi thuần túy là 

[ 
P_k=\frac{10^k(10^k-1)}2 
] 

thay vì tổng các số nguyên từ (10^{k-1}) đến (10^k-1). 

Đối với (n=500000), thuật toán không bao giờ xây dựng biểu thức và không bao giờ đánh giá số thập phân lớn. Nó thực hiện phép lặp tương tự một lần cho mỗi độ dài, giảm từng modulo số lượng (998244353). Kích thước đầu vào lớn chỉ thay đổi số lần lặp theo thời gian tuyến tính chứ không thay đổi cấu trúc toán học của lời giải.
