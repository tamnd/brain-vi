---
title: "CF 102407B - Điệu nhảy cuồng nhiệt"
description: "Joker đếm giây bắt đầu từ một. Ở giây (t), anh ta nói cách biểu diễn (t) trong cơ số (a), không có số 0 đứng đầu. Ví dụ, trong cơ số (3), dãy bắt đầu bằng (1,2,10,11,12,ldots)."
date: "2026-08-11T23:47:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102407
codeforces_index: "B"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102407
solve_time_s: 262
verified: true
draft: false
---

[CF 102407B - Điệu nhảy điên cuồng](https://codeforces.com/problemset/problem/102407/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Joker đếm giây bắt đầu từ một. Ở giây (t), anh ta nói cách biểu diễn (t) trong cơ số (a), không có số 0 đứng đầu. Ví dụ, trong cơ số (3), dãy bắt đầu bằng (1,2,10,11,12,\ldots). 

Đối với mỗi chữ số (i), đầu vào cho ra (b_i), số lần chính xác của chữ số (i) được cho là đã được nói khi điệu nhảy dừng lại. Chúng ta phải xác định giây (n) duy nhất, nếu tồn tại, sao cho phép nối các biểu diễn cơ sở-(a) của 

[ 
1,2,\ldots,n 
] 

chứa chính xác (b_i) bản sao của mọi chữ số (i). Nếu không tồn tại (n) như vậy thì Joker không bao giờ thỏa mãn điều kiện dừng của mình, vì vậy câu trả lời là (-1). 

Cơ số có thể lớn bằng (100000) và mỗi số chữ số được yêu cầu có thể lớn bằng (10^9). Do đó, tổng số lượng được yêu cầu có thể đạt tới (10^{14}). Sau đó, một mô phỏng xử lý mọi chữ số được nói sẽ phải thực hiện theo thứ tự cập nhật chữ số (10^{14}), vượt xa mọi giới hạn thời gian thực tế. Thuật toán phải hoạt động với toàn bộ dãy số thay vì tự liệt kê các số đó. 

Có một số trường hợp tế nhị khi giải pháp hấp dẫn không thành công. Nếu mọi (b_i) bằng 0 thì câu trả lời là (-1), vì Joker đã nói chữ số (1) sau giây đầu tiên, nên điều kiện dừng không bao giờ có thể giữ được ở thời điểm 0. 

Ví dụ,```
5
0 0 0 0 0
```có câu trả lời```
-1
```Vấn đề thứ hai là tổng số chữ số nói là không đủ. Coi như```
2
2 1
```Tổng số yêu cầu là (3). Chính xác ba chữ số đã được nói sau giây (1) và (2), cụ thể là (1,10). Tần số của chúng là hai bản sao của chữ số (1) và một bản sao của chữ số (0), vì vậy đầu vào cụ thể này thực sự có câu trả lời (2). Ngược lại,```
2
1 2
```cũng có tổng (3), nên nó cũng trỏ đến (n=2), nhưng tần số thực tế là ([1,2]), nghĩa là chữ số (0) xuất hiện một lần và chữ số (1) xuất hiện hai lần. Điều này minh họa tại sao, sau khi khôi phục (n) từ tổng, chúng ta vẫn phải kiểm tra từng chữ số riêng lẻ. 

Một lỗi biên đặc biệt dễ xảy ra khi (n) vượt qua lũy thừa của cơ số. Trong cơ số (3), các số (1,2) mỗi số có một chữ số, trong khi (3,4,5) có hai chữ số. Một phép tính coi độ dài chữ số là không đổi trên ranh giới này sẽ cho thời gian dừng sai. 

Số 0 đứng đầu là một nguồn sai lầm khác. Khi đếm chữ số (0), biểu diễn của (7) trong cơ số (10) không chứa số 0, mặc dù biểu diễn có chiều rộng cố định như (007) sẽ có. Công thức số 0 phải loại trừ rõ ràng những vị trí dẫn đầu không tồn tại đó. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng điệu nhảy. Bắt đầu từ (1), chuyển đổi mọi số nguyên thành cơ số (a), thêm một vào bộ đếm của mỗi chữ số xuất hiện trong biểu diễn đó và dừng khi tất cả các bộ đếm bằng mục tiêu của chúng. Điều này đúng vì nó tuân theo chính xác quy trình được mô tả bởi vấn đề. 

Vấn đề là quy mô. Tổng số lần xuất hiện chữ số mà chúng tôi có thể cần xử lý là (\sum b_i), có thể lớn tới (10^{14}). Ngay cả trước khi tính đến chi phí chuyển đổi số nguyên, điều đó có nghĩa là khoảng (10^{14}) cập nhật bộ đếm cơ bản. Đây là điểm mà vũ lực trở nên không thể. 

Quan sát quan trọng là chúng ta thực sự không cần vectơ chữ số để tìm thời gian ứng cử viên. Mỗi giây đóng góp ít nhất một chữ số và mỗi số mới đóng góp toàn bộ cách biểu diễn của nó. Do đó tổng số chữ số được nói sau giây (n) là 

[ 
D(n)=\sum_{x=1}^{n}\operatorname{digits__a(x). 
] 

Hàm này tăng nghiêm ngặt vì việc chuyển từ (n) sang (n+1) sẽ thêm ít nhất một chữ số. Do đó, nếu tồn tại một nghiệm thì thời gian dừng của nó được xác định duy nhất bởi 

[ 
D(n)=\sum_{i=0}^{a-1}b_i. 
] 

Chúng ta có thể đảo ngược hàm này một cách trực tiếp bằng cách nhóm các số theo số chữ số cơ số (a) của chúng. Có (a-1) số có một chữ số, ((a-1)a) số có hai chữ số, ((a-1)a^2) số có ba chữ số, v.v. Đối với mỗi khối, tổng đóng góp của nó chỉ đơn giản là số lượng số trong khối nhân với độ dài chữ số của nó. 

Khi đã biết (n), chúng ta tính tần số của mỗi chữ số trong tất cả các số từ (1) đến (n). Đối với vị trí kiểu thập phân cố định (p=1,a,a^2,\ldots), phân tách cao/hiện tại/thấp thông thường hoạt động theo cách giống hệt nhau trong một cơ số tùy ý. Điều này đưa ra số lần xuất hiện của mỗi chữ số tại vị trí đó mà không liệt kê các số riêng lẻ. 

Phương pháp brute-force hoạt động vì nó xây dựng rõ ràng chính xác trình tự mà chúng ta cần. Nó thất bại vì chuỗi có thể chứa một số lượng lớn các chữ số. Quan sát thấy rằng tổng số chữ số đang tăng lên một cách nghiêm ngặt cho phép chúng ta thu gọn việc tìm kiếm thời gian dừng thành một vài khối có độ dài chữ số và công thức đếm vị trí cho phép chúng ta xác minh tất cả tần số chữ số mà không cần duyệt qua chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(S)), trong đó (S=\sum b_i) ở thang đo tệ nhất | (O(a)) | Quá chậm | 
| Tối ưu | (O(a\log_a n)) | (O(a)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán 

[ 
S=b_0+b_1+\cdots+b_{a-1}. 
] 

Đây là tổng số lần xuất hiện chữ số phải được nói. Nếu (S=0), không có số dương nào thỏa mãn điều kiện nên ta trả về ngay (-1). 
2. Tìm khối có độ dài chữ số nào chứa thời gian dừng. 

Các số có chính xác (k) chữ số cơ số (a) là 

[ 
a^{k-1},a^{k-1}+1,\ldots,a^k-1. 
] 

có 

[ 
(a-1)a^{k-1} 
] 

những con số như vậy, và chúng cùng nhau đóng góp 

[ 
k(a-1)a^{k-1} 
] 

chữ số được nói.

Trừ các khối hoàn chỉnh khỏi (S) cho đến khi số lượng còn lại thuộc về một khối. Vì cơ số ít nhất là (2) nên số khối ở đáp án cuối cùng là logarit. 
3. Giả sử số tiền còn lại là (R) và khối hiện tại chứa các số có (k). 

Mỗi số trong khối này đóng góp chính xác (k) chữ số. Do đó, (R) phải chia hết cho (k). Nếu 

[ 
R\bmod k\ne0, 
] 

thì không có số nguyên nào của biểu diễn chữ số (k) có thể tạo ra chính xác (R) chữ số, vì vậy câu trả lời là (-1). 

Nếu không, 

[ 
q=R/k 
] 

là số các số có (k) chữ số đã được nói bên trong khối này. Nếu khối bắt đầu tại (a^{k-1}), thời gian dừng ứng viên là 

[ 
n=a^{k-1}+q-1. 
] 
4. Đếm số lần xuất hiện của mỗi chữ số trong (1,\ldots,n). 

Cố định một vị trí (p=a^j). Với số (x), hãy viết 

[ 
x=(\text{high})\cdot(ap)+(\text{current})\cdot p+\text{low}, 
] 

ở đâu 

[ 
\text{high}=\left\lfloor\frac{x}{ap}\right\rfloor, 
\qquad 
\text{current}=\left\lfloor\frac{x}{p}\right\rfloor\bmod a, 
\qquad 
\text{thấp}=x\bmod p. 
] 

Đối với một chữ số khác 0 (d), mỗi chu kỳ hoàn chỉnh có độ dài (ap) đóng góp chính xác (p) bản sao của (d), tạo ra số lượng cơ bản là 

[ 
\text{cao}\cdot p. 
] 

Nếu chữ số hiện tại lớn hơn (d), một nhóm lần xuất hiện (p) bổ sung sẽ xuất hiện. Nếu nó bằng (d), chỉ một phần nhóm số (\text{low}+1) mới đóng góp. 
5. Xử lý riêng chữ số 0. 

Số 0 không thể chiếm vị trí đầu của một số nên công thức của nó có ít hơn một chu trình đầy đủ. Khi (\text{high}>0), đóng góp hoàn chỉnh là 

[ 
(\text{cao}-1)p. 
] 

Nếu chữ số hiện tại bằng 0, hãy thêm (\text{low}+1). Ngược lại thì thêm (p). 

Đây chính xác là sự điều chỉnh ngăn cản việc tính các biểu diễn như (007). 
6. So sánh mảng tần số tính toán với (b). 

Nếu mỗi chữ số có chính xác tần số được yêu cầu thì xuất ra (n). Mặt khác, tổng số chữ số trỏ đến một ứng cử viên duy nhất (n), nhưng ứng cử viên đó có phân phối chữ số sai, vì vậy câu trả lời đúng là (-1). 

### Tại sao nó hoạt động 

Bất biến trung tâm là sau khi xử lý các khối có độ dài chữ số hoàn chỉnh, giá trị còn lại của (S) chính xác là số lần xuất hiện chữ số vẫn được yêu cầu từ khối tiếp theo. Vì mọi số trong khối đó có cùng độ dài (k), điểm dừng chính xác tồn tại ở đó khi và chỉ khi số còn lại chia hết cho (k) và thương số xác định ứng cử viên duy nhất (n). 

Đối với ứng cử viên đó, công thức tính vị trí sẽ tính mỗi lần xuất hiện tại mỗi vị trí chính xác một lần. Công thức số 0 riêng biệt sẽ loại bỏ các vị trí số 0 đứng đầu không có trong cách biểu diễn thông thường. Do đó vectơ được tính toán chính xác là vectơ có tần số chữ số trong (1,\ldots,n). Do đó, việc kiểm tra sự bằng nhau cuối cùng là cần thiết và đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def find_time(a, total):
    if total == 0:
        return -1

    power = 1
    length = 1
    remaining = total

    while True:
        count = (a - 1) * power
        block_digits = count * length

        if remaining > block_digits:
            remaining -= block_digits
            power *= a
            length += 1
        else:
            if remaining % length != 0:
                return -1

            q = remaining // length
            if q == 0 or q > count:
                return -1

            return power + q - 1

def count_digits(n, a):
    cnt = [0] * a
    p = 1

    while p <= n:
        high = n // (p * a)
        cur = (n // p) % a
        low = n % p

        base = high * p

        for d in range(1, a):
            value = base

            if cur > d:
                value += p
            elif cur == d:
                value += low + 1

            cnt[d] += value

        if high > 0:
            cnt[0] += (high - 1) * p

            if cur == 0:
                cnt[0] += low + 1
            else:
                cnt[0] += p

        p *= a

    return cnt

def solve():
    a = int(input())
    b = list(map(int, input().split()))

    total = sum(b)

    n = find_time(a, total)
    if n == -1:
        print(-1)
        return

    cnt = count_digits(n, a)

    if cnt == b:
        print(n)
    else:
        print(-1)

if __name__ == "__main__":
    solve()
```Hàm đầu tiên sử dụng tính đơn điệu của tổng số chữ số.`power`là số đầu tiên trong khối có độ dài chữ số hiện tại, trong khi`length`là số chữ số của mỗi số trong khối đó.`block_digits`do đó là số chữ số được nói chính xác được đóng góp bởi toàn bộ khối. 

điều kiện`remaining > block_digits`cố tình sử dụng một bất đẳng thức nghiêm ngặt. Nếu tổng số còn lại chính xác bằng kích thước của khối thì câu trả lời là số cuối cùng trong khối đó, do đó khối hiện tại phải được xử lý thay vì bị bỏ qua. 

Kiểm tra khả năng chia hết ngăn ngừa lỗi kiểu từng cái một trong đó số biểu diễn phân số tùy ý sẽ được coi là số nguyên giây. Sau khi chia,`q`đếm có bao nhiêu số từ khối hiện tại được bao gồm, vì vậy`power + q - 1`là con số cuối cùng được nói. 

Hàm thứ hai áp dụng công thức vị trí một cách độc lập cho (p=1,a,a^2,\ldots). Vòng lặp kết thúc`range(1, a)`xử lý tất cả các chữ số khác 0. Số 0 được xử lý riêng vì các số 0 đứng đầu không tồn tại trong cách biểu thị bằng giọng nói. 

Số nguyên Python có độ chính xác tùy ý, do đó các giá trị như (10^{14}), tích số có lũy thừa cơ số và tần số chữ số thu được không bị tràn. phép nhân`p * a`cũng an toàn vì số nguyên Python tự động tăng khi cần thiết. 

Việc kiểm tra đẳng thức chỉ được thực hiện sau khi ứng viên (n) đã được phục hồi. Sự phân tách này rất hữu ích vì tổng số tìm thấy thời gian dừng duy nhất có thể, trong khi phép tính vị trí quyết định liệu ứng cử viên đó có thực sự có phân phối chữ số được yêu cầu hay không. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
10
1 2 1 1 1 1 1 1 1 1
```Tổng số chữ số được yêu cầu là 

[ 
1+2+8=11. 
] 

Đối với cơ số (10), các số có một chữ số (1) đến (9) đóng góp (9) chữ số. Còn lại hai chữ số, vì vậy khối tiếp theo chứa các số có hai chữ số và chúng ta cần chính xác một trong số chúng. 

|`length`|`power`|`block_digits`|`remaining`| Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 9 | 11 | Bỏ qua khối | 
| 2 | 10 | 180 | 2 | Lấy 1 số | 

Ứng viên là 

[ 
10+1-1=10. 
] 

Các số được nói là (1,2,\ldots,10). Các chữ số (1) đến (9), mỗi chữ số xuất hiện một lần trong các số có một chữ số và chữ số (1) xuất hiện một lần nữa trong (10), trong khi chữ số (0) xuất hiện một lần. Tần số kết quả là chính xác 

[ 
[1,2,1,1,1,1,1,1,1,1]. 
] 

Như vậy câu trả lời là`10`. 

### Mẫu 2 

Đầu vào là```
2
3 5
```Tổng số yêu cầu là 

[ 
3+5=8. 
] 

Trong cơ số (2), khối một chữ số chỉ chứa số (1), đóng góp một chữ số. Tổng số còn lại là (7). Khối hai chữ số chứa (2) số, đóng góp (4) chữ số nên bị bỏ qua. Tổng còn lại là (3), thuộc khối ba chữ số. 

|`length`|`power`|`block_digits`|`remaining`| Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 8 | Bỏ qua, còn lại = 7 | 
| 2 | 2 | 4 | 7 | Bỏ qua, còn lại = 3 | 
| 3 | 4 | 12 | 3 | (3\bmod3=0) | 

Chúng tôi lấy 

[ 
q=3/3=1, 
] 

vậy ứng cử viên là 

[ 
4+1-1=4. 
] 

Các số được nói là```
1
10
11
100
```Số chữ số của chúng là ba số 0 và năm số một. 

|`digit`| Bắt buộc | Đã tính toán | 
| --- | --- | --- | 
| 0 | 3 | 3 | 
| 1 | 5 | 5 | 

Ứng viên hợp lệ, vì vậy câu trả lời là`4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(a\log_a n)) | Có (O(\log_a n)) vị trí chữ số và mọi vị trí đều kiểm tra tất cả (a-1) chữ số khác 0. | 
| Không gian | (O(a)) | Mảng tần số chứa một bộ đếm cho mỗi chữ số có thể. | 

Tổng số được yêu cầu tối đa là (10^9a), do đó ứng viên (n) có thể rất lớn, nhưng số chữ số cơ số (a) của nó vẫn là logarit. Ngay cả đối với (a=100000), chỉ một số ít vị trí chữ số có thể xuất hiện ở thang đo liên quan lớn nhất. Công việc (O(a\log_a n)) thu được có thể dễ dàng quản lý đối với (a\le100000), trong khi mức sử dụng bộ nhớ (O(a)) cũng nhỏ. 

## Trường hợp thử nghiệm 

Bộ dây thử nghiệm sau đây sử dụng cùng một`solve()`hoạt động như giải pháp được gửi.```python
import sys
import io

input = sys.stdin.readline

def find_time(a, total):
    if total == 0:
        return -1

    power = 1
    length = 1
    remaining = total

    while True:
        count = (a - 1) * power
        block_digits = count * length

        if remaining > block_digits:
            remaining -= block_digits
            power *= a
            length += 1
        else:
            if remaining % length != 0:
                return -1

            q = remaining // length
            if q == 0 or q > count:
                return -1

            return power + q - 1

def count_digits(n, a):
    cnt = [0] * a
    p = 1

    while p <= n:
        high = n // (p * a)
        cur = (n // p) % a
        low = n % p

        base = high * p

        for d in range(1, a):
            value = base

            if cur > d:
                value += p
            elif cur == d:
                value += low + 1

            cnt[d] += value

        if high > 0:
            cnt[0] += (high - 1) * p

            if cur == 0:
                cnt[0] += low + 1
            else:
                cnt[0] += p

        p *= a

    return cnt

def solve():
    a = int(input())
    b = list(map(int, input().split()))

    total = sum(b)
    n = find_time(a, total)

    if n == -1:
        print(-1)
        return

    if count_digits(n, a) == b:
        print(n)
    else:
        print(-1)

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out

    try:
        solve()
        return out.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = old_input

# Provided samples
assert run("10\n1 2 1 1 1 1 1 1 1 1\n") == "10", "sample 1"
assert run("2\n3 5\n") == "4", "sample 2"
assert run("5\n0 0 0 0 0\n") == "-1", "sample 3"

# Minimum-size valid case: only the number 1 is spoken.
assert run("2\n0 1\n") == "1", "minimum valid input"

# Crossing the first digit-length boundary in base 3.
# 1, 2, 10, 11, 12 gives counts [1, 3, 2].
assert run("3\n1 3 2\n") == "5", "digit-length boundary"

# All target values equal, but no stopping time has those frequencies.
assert run("2\n2 2\n") == "-1", "all-equal impossible target"

# Maximum base and maximum array size.
# For n = 99999 in base 100000, every spoken number is one digit.
large_b = [0] + [1] * 99999
large_input = "100000\n" + " ".join(map(str, large_b)) + "\n"
assert run(large_input) == "99999", "maximum base and array size"

# Same total as n=2 in base 2, but wrong digit distribution.
assert run("2\n1 2\n") == "-1", "total count alone is insufficient"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 0 1`|`1`| Điệu nhảy hợp lệ nhỏ nhất có thể và chữ số được nói đầu tiên | 
|`3 / 1 3 2`|`5`| Xử lý đúng khi số chữ số tăng | 
|`2 / 2 2`|`-1`| Các giá trị mục tiêu hoàn toàn bằng nhau và không có thời gian dừng hợp lệ | 
|`100000 / 0 1 1 ... 1`|`99999`| Kích thước mảng chữ số và cơ sở tối đa | 
|`2 / 1 2`|`-1`| Ứng viên được xác định theo tổng số nhưng bị từ chối theo tần số chữ số | 

## Vỏ cạnh 

### Tổng số không 

cho```
5
0 0 0 0 0
```tổng số chữ số được yêu cầu bằng không. Thuật toán phát hiện điều này trước khi cố gắng xác định vị trí khối có độ dài chữ số và trả về (-1). Không có giây dương nào mà biểu diễn của nó đóng góp các chữ số bằng 0. 

### Tổng số không tương ứng với số nguyên biểu diễn 

Hãy xem xét```
2
2 2
```Tổng số là (4). Trong cơ số (2), sau số có một chữ số (1), khối tiếp theo gồm các số có hai chữ số và có tổng cộng bốn chữ số. Do đó tổng (4) xác định duy nhất (n=2). 

Tuy nhiên, trình tự thực tế là```
1
10
```có tần số chữ số là ([1,2]), không phải ([2,2]). Phép so sánh cuối cùng loại bỏ ứng viên và đưa ra kết quả (-1). Điều này chứng tỏ tại sao việc khôi phục (n) từ tổng chỉ là nửa đầu của giải pháp. 

### Vượt qua sức mạnh của căn cứ 

cho```
3
1 3 2
```tổng số là (6). Trong cơ số (3), các số (1) và (2) đóng góp hai chữ số, trong khi (3,4,5) mỗi số đóng góp thêm hai chữ số nữa. Khối đầu tiên đóng góp (2), bỏ ra (4), nên ứng cử viên là (5). 

Các đại diện là```
1
2
10
11
12
```và tần số chữ số của chúng là một số không, ba số một và hai số hai. Câu trả lời là`5`. Việc tính toán khối tránh việc xử lý không chính xác tất cả các số có cùng độ dài. 

### Số 0 đứng đầu 

lấy```
10
1 1 1 1 1 1 1 1 1 1
```Tổng số là (10), xác định (n=10). số`10`chứa một số 0, trong khi các số`1`bởi vì`9`không chứa gì cả. Do đó số 0 chính xác là một. 

Phương pháp đếm có chiều rộng cố định có thể đếm không chính xác các số 0 trong`01`,`02`, vân vân. Công thức 

[ 
(\text{cao}-1)p 
] 

bằng 0 sẽ loại bỏ các vị trí dẫn đầu giả tạo đó. 

### Cơ sở tối đa 

cho```
100000
0 1 1 1 ... 1
```với một số 0 theo sau là (99999) số một, tổng số được yêu cầu là (99999). Mọi số từ (1) đến (99999) được biểu thị bằng chính xác một chữ số cơ sở (100000), do đó ứng cử viên là (99999). Mỗi chữ số khác 0 xuất hiện một lần và số 0 xuất hiện 0 lần, đưa ra vectơ được yêu cầu chính xác. 

Trường hợp này thực hiện mảng chữ số lớn nhất có thể và xác nhận rằng thuật toán không dựa vào cơ số nhỏ.
