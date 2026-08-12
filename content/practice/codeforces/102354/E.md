---
title: "CF 102354E - Khai triển thập phân"
description: "Hằng số có thể được viết lại thành [ varphi=prod{k=1}^{infty}(1-10^{-k}), ] vì hệ số thứ (k) chính xác là ((10^k-1)/10^k). Chúng ta cần chữ số chiếm vị trí (n) sau dấu thập phân, trong đó (n) có thể lớn bằng (10^{18}). Có tới (10^5) truy vấn độc lập."
date: "2026-08-13T00:34:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102354
codeforces_index: "E"
codeforces_contest_name: "2018-2019 Summer Petrozavodsk Camp, Oleksandr Kulkov Contest 2"
rating: 0
weight: 102354
solve_time_s: 90
verified: true
draft: false
---

[CF 102354E - Mở rộng thập phân](https://codeforces.com/problemset/problem/102354/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hằng số có thể được viết lại thành 

[ 
\varphi=\prod_{k=1}^{\infty}(1-10^{-k}), 
] 

bởi vì hệ số thứ (k)-th chính xác là ((10^k-1)/10^k). Chúng ta cần chữ số chiếm vị trí (n) sau dấu thập phân, trong đó (n) có thể lớn bằng (10^{18}). 

Có tới (10^5) truy vấn độc lập. Một phương thức sử dụng (O(n)) hoặc thậm chí (O(\sqrt n)) hoạt động cho mỗi truy vấn là quá chậm. Ở mức tối đa (n), (O(n)) sẽ yêu cầu khoảng (10^{18}) thao tác cho một truy vấn, trong khi (O(\sqrt n)) vẫn sẽ yêu cầu khoảng (10^9) thao tác cho mỗi truy vấn. Lời giải phải khai thác cấu trúc chính xác của tích vô hạn và trả lời từng vị trí trong thời gian cơ bản không đổi. 

Cái bẫy đầu tiên là bản thân sản phẩm không hiển thị trực tiếp các chữ số thập phân. Ví dụ: hai chữ số đầu tiên là (8) và (9), mặc dù số hạng đầu tiên trong khai triển hữu ích của nó có hệ số (-1). Nếu chúng ta chỉ nhìn vào hệ số của (10^{-n}), chúng ta có thể nhận được giá trị âm và quên rằng cần phải vay số thập phân. Đối với đầu vào```
1
1
```câu trả lời là```
8
```không phải (-1), vì việc mở rộng bắt đầu bằng (0,89\ldots). 

Trường hợp biên thứ hai xảy ra khi (n) chính xác là một trong các số mũ đặc biệt xuất hiện trong khai triển. Ví dụ,```
1
5
```có câu trả lời (1). Số mũ (5) đóng góp (+10^{-5}), nhưng số hạng đứng sau nó vẫn ảnh hưởng đến việc có cần vay hay không. Chỉ coi hệ số là chữ số cuối cùng có tác dụng ở đây, nhưng phím tắt tương tự không thành công ở (n=7), trong đó hệ số là (+1) trong khi chữ số đúng là (0). 

Ranh giới thứ ba xuất hiện ngay sau số mũ đặc biệt. Vì```
1
6
```câu trả lời là (0), vì phần đóng góp (10^{-5}) đã trở thành một phần của tiền tố số nguyên khi vị trí thập phân đạt tới (6). Việc triển khai bất cẩn chỉ nhìn vào số mũ đặc biệt tiếp theo có thể trả về sai (1). 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ nhân lên các yếu tố 

[ 
(1-10^{-1})(1-10^{-2})(1-10^{-3})\cdots 
] 

đến độ chính xác đủ cao và kiểm tra vị trí thập phân mong muốn. Điều này đúng với (n) nhỏ, vì các yếu tố có chỉ số lớn hơn (n) không thể ảnh hưởng đến (n) vị trí thập phân đầu tiên một lượng đáng kể. Tuy nhiên, đối với (n=10^{18}), ngay cả việc xây dựng (n) chữ số thập phân đầu tiên cũng không thể thực hiện được. Công việc mạnh mẽ là (\Theta(n)) cho một truy vấn, vốn đã là (10^{18}) hoạt động cấp chữ số ở vị trí lớn nhất được phép. 

Quan sát quan trọng là định lý số ngũ giác của Euler. Nó mang lại sự mở rộng chính xác 

1+\sum_{k=1}^{\infty}(-1)^k 
\left( 
x^{k(3k-1)/2}+x^{k(3k+1)/2} 
\đúng). 
] 

Thay thế (x=1/10), ta thu được 

1+\sum_{k=1}^{\infty}(-1)^k 
\left( 
10^{-k(3k-1)/2} 
+ 
10^{-k(3k+1)/2} 
\đúng). 
] 

Vì vậy hầu hết mọi vị trí thập phân đều có hệ số bằng 0. Các hệ số khác 0 duy nhất xảy ra ở các số ngũ giác tổng quát 

[ 
P_k^-=\frac{k(3k-1)}2, 
\qquad 
P_k^+=\frac{k(3k+1)}2, 
] 

và cả hai vị trí đều có hệ số ((-1)^k). 

Sự thưa thớt này làm giảm vấn đề xác định hai số ngũ giác tổng quát liên tiếp xung quanh (n). Chúng ta không cần xây dựng bất kỳ tiền tố thập phân nào. Sau khi nhân khai triển với (10^n), mọi số hạng có số mũ nhỏ hơn (n) sẽ trở thành bội số nguyên của (10), do đó nó không thể ảnh hưởng đến modulo chữ số thập phân cuối cùng (10). Thông tin cục bộ duy nhất quan trọng là hệ số tại số mũ (n), cùng với việc đuôi vô hạn bắt đầu sau (n) là dương hay âm. 

Đuôi có độ lớn nhỏ hơn (1/9). Số hạng đầu tiên của nó có độ lớn lớn hơn tổng của tất cả các số hạng sau đó, vì vậy dấu của đuôi chính xác là dấu của số hạng đầu tiên của nó. Do đó, nếu số mũ ngũ giác tổng quát tiếp theo có hệ số (+1) thì không xảy ra hiện tượng vay mượn. Nếu nó có hệ số (-1) thì phải mượn một đơn vị của hệ số ở vị trí (n). 

(k) liên quan nằm trong khoảng (\sqrt{n}), nhưng chúng ta không lặp lại nó. Giải quyết 

[ 
\frac{k(3k-1)}2\le n 
] 

cho 

[ 
k\le \frac{1+\sqrt{24n+1}}6. 
] 

Căn bậc hai số nguyên cho phép chúng ta lấy trực tiếp (k) này, do đó mọi truy vấn đều thực hiện (O(1)) phép toán số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n)) mỗi truy vấn | (O(n)) | Quá chậm | 
| Tối ưu | (O(1)) mỗi truy vấn | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với truy vấn (n), hãy tính 

[ 
k=\left\lfloor\frac{1+\sqrt{24n+1}}6\right\rfloor. 
] 

Đây là (k) lớn nhất của (P_k^-\le n), trong đó (P_k^-=k(3k-1)/2). 

1. Tính hai vị trí ngũ giác tổng quát thuộc (k) này 

[ 
a=P_k^-, 
\qquad 
b=P_k^+. 
] 

Chúng xuất hiện liên tiếp trong khai triển, ngoại trừ vị trí tiếp theo sau (b) là (P_{k+1}^-). 

1. Xác định hệ số tại vị trí (n). Nếu (n=a), hệ số là ((-1)^k). Nếu (n=b), hệ số cũng là ((-1)^k). Ngược lại hệ số bằng 0. 
2. Xác định số mũ thứ nhất lớn hơn (n). Nếu (n<b) thì đó là (b). Nếu (n\ge b), nó là (P_{k+1}^-). Hệ số của nó có dấu ((-1)^k) trong trường hợp đầu tiên và ((-1)^{k+1}) trong trường hợp thứ hai. 
3. Bắt đầu với hệ số ở vị trí (n). Nếu số hạng bị bỏ qua đầu tiên là số âm, hãy trừ đi một. Cuối cùng lấy kết quả modulo (10), chuyển đổi các giá trị như (-1) thành chữ số thập phân (9). 
4. Nối chữ số kết quả vào đầu ra. Việc xử lý tất cả các truy vấn một cách độc lập sẽ đưa ra chuỗi chữ số được yêu cầu. 

Lý do của quy tắc vay mượn dễ thấy nhất sau khi nhân với (10^n). Viết 

[ 
10^n\varphi=S+R, 
]

trong đó (S) chứa tất cả các số hạng có số mũ nhiều nhất là (n) và (R) chứa các số hạng còn lại. Số (S) là một số nguyên. Phần còn lại thỏa mãn (|R|<1/9) và dấu của nó bằng dấu của số hạng đầu tiên. Do đó (\lfloor10^n\varphi\rfloor) là (S) khi (R>0) và (S-1) khi (R<0). Modulo (10), chỉ có hệ số tại số mũ (n) tồn tại trong (S), cho kết quả chính xác như thuật toán trên. 

### Tại sao nó hoạt động 

Điều bất biến là sau vị trí (n), toàn bộ phần đuôi vô hạn hoạt động nhằm mục đích xác định phần nguyên của (10^n\varphi), giống hệt như một phân số dương nhỏ hoặc một phần âm nhỏ. Thuật ngữ ngũ giác bị bỏ qua đầu tiên xác định trường hợp nào xảy ra vì giá trị tuyệt đối của nó chi phối tất cả các thuật ngữ bị bỏ qua sau đó. Trong khi đó, mọi số hạng trước đều đóng góp bội số của (10) sau khi nhân với (10^n), do đó chỉ hệ số tại số mũ (n) xác định chữ số cuối cùng của tiền tố số nguyên. Do đó, thuật toán tính đến cả các hiệu ứng có thể xảy ra, hệ số cục bộ và khoản vay duy nhất có thể, và không thể tạo ra một chữ số khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def digit(n):
    # k is the largest integer with k(3k-1)/2 <= n.
    k = (1 + (24 * n + 1) ** 0.5) // 6

    # Floating point is not safe for n up to 10^18.
    # Recompute using integer square root.
    s = math.isqrt(24 * n + 1)
    k = (1 + s) // 6

    while (k + 1) * (3 * (k + 1) - 1) // 2 <= n:
        k += 1
    while k * (3 * k - 1) // 2 > n:
        k -= 1

    a = k * (3 * k - 1) // 2
    b = k * (3 * k + 1) // 2

    if n == a or n == b:
        cur = 1 if k % 2 == 0 else -1
    else:
        cur = 0

    if n < b:
        next_sign = 1 if k % 2 == 0 else -1
    else:
        next_sign = -1 if k % 2 == 0 else 1

    if next_sign < 0:
        cur -= 1

    return cur % 10

def solve():
    t = int(input())
    ns = list(map(int, input().split()))

    ans = []
    for n in ns:
        ans.append(str(digit(n)))

    print(" ".join(ans))

if __name__ == "__main__":
    import math
    solve()
```Phép tính đầu tiên tìm khối ngũ giác tổng quát chứa (n). Căn bậc hai số nguyên được sử dụng vì (n) có thể đạt tới (10^{18}) và căn bậc hai dấu phẩy động có thể mất đủ độ chính xác gần một hình vuông hoàn hảo để di chuyển (k) một. Trong thực tế, các vòng hiệu chỉnh có thời gian không đổi và làm cho ranh giới trở nên chính xác ngay cả khi công thức căn bậc hai số nguyên rơi vào một ứng cử viên liền kề. 

Các biến`a`Và`b`là hai số mũ thuộc (k). So sánh (n) với chúng sẽ xác định hệ số hiện tại là (0) hay ((-1)^k), đồng thời cho chúng ta biết số ngũ giác nào tiếp theo. 

biểu hiện`next_sign`ghi lại sự mang hoặc mượn duy nhất mà hậu tố vô hạn có thể tạo ra. Hậu tố phủ định sẽ thay đổi tầng một, vì vậy`cur -= 1`là sự điều chỉnh mang số thập phân hoàn chỉnh. 

Số nguyên Python có độ chính xác tùy ý, vì vậy các giá trị như (24n+1) và các tích số liên quan đến (k) đều an toàn mà không cần xử lý tràn đặc biệt. Mã này cũng sử dụng đầu vào được đệm và tạo ra tất cả các câu trả lời trong một thao tác đầu ra, điều này quan trọng đối với (10^5) trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

Để bắt đầu khai triển số thập phân, số mũ và dấu ngũ giác tổng quát là 

[ 
1:-,\quad 2:-,\quad 5:+,\quad 7:+,\quad 12:-,\quad 15:-,\quad 22:+. 
] 

Đối với phần đầu tiên của Mẫu 1, thuật toán hoạt động như sau. 

| (n) | (k) | (P_k^-) | (P_k^+) | Hệ số hiện tại | Dấu hiệu tiếp theo | Chữ số | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | -1 | -1 | 8 | 
| 2 | 1 | 1 | 2 | -1 | +1 | 9 | 
| 3 | 1 | 1 | 2 | 0 | +1 | 0 | 
| 4 | 1 | 1 | 2 | 0 | +1 | 0 | 
| 5 | 2 | 5 | 7 | +1 | +1 | 1 | 
| 6 | 2 | 5 | 7 | 0 | +1 | 0 | 
| 7 | 2 | 5 | 7 | +1 | -1 | 0 | 
| 8 | 2 | 5 | 7 | 0 | -1 | 9 | 
| 9 | 2 | 5 | 7 | 0 | -1 | 9 | 
| 10 | 2 | 5 | 7 | 0 | -1 | 9 | 
| 11 | 2 | 5 | 7 | 0 | -1 | 9 | 
| 12 | 3 | 12 | 15 | -1 | -1 | 8 | 
| 13 | 3 | 12 | 15 | 0 | -1 | 9 | 
| 14 | 3 | 12 | 15 | 0 | -1 | 9 | 
| 15 | 3 | 12 | 15 | -1 | +1 | 9 | 

Vị trí đầu tiên và thứ bảy cho thấy tại sao dấu hiệu ở đuôi lại quan trọng. Tại (n=1), hệ số hiện tại là (-1) và số hạng tiếp theo cũng âm, do đó tiền tố số nguyên trở thành một số nhỏ hơn, cho ra (8). Tại (n=7), hệ số hiện tại là (+1), nhưng số hạng tiếp theo âm, do đó (1) bị mượn đi và chữ số trở thành (0). 

Dấu vết hữu ích thứ hai tập trung vào quá trình chuyển đổi xung quanh (12) và (15). 

| (n) | (k) | (P_k^-) | (P_k^+) | Hệ số hiện tại | Số mũ tiếp theo | Dấu hiệu tiếp theo | Chữ số | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 11 | 2 | 5 | 7 | 0 | 12 | -1 | 9 | 
| 12 | 3 | 12 | 15 | -1 | 15 | -1 | 8 | 
| 13 | 3 | 12 | 15 | 0 | 15 | -1 | 9 | 
| 14 | 3 | 12 | 15 | 0 | 15 | -1 | 9 | 
| 15 | 3 | 12 | 15 | -1 | 22 | +1 | 9 | 
| 16 | 3 | 12 | 15 | 0 | 22 | +1 | 0 | 
| 17 | 3 | 12 | 15 | 0 | 22 | +1 | 0 | 

Dấu vết này cho thấy hai loại vị trí ngũ giác khác nhau. Tại (n=12), bản thân hệ số này âm và hệ số tiếp theo cũng âm, tạo ra (8). Tại (n=15), hệ số âm nhưng hệ số sau dương nên không xảy ra vay thêm và chữ số vẫn giữ nguyên (9). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(t)) | Mỗi truy vấn thực hiện một số lượng không đổi các phép toán số học số nguyên và một căn bậc hai số nguyên. | 
| Không gian | (O(t)) | Các giá trị đầu vào và chuỗi đầu ra được lưu trữ để xử lý đệm. | 

Với (t\le10^5), tổng số thao tác tuyến tính dễ dàng phù hợp với giới hạn một giây. Thuật toán không bao giờ tạo tiền tố có độ dài (n), do đó, thực tế là (n) có thể là (10^{18}) không ảnh hưởng đến số lượng công việc trên mỗi truy vấn. 

## Trường hợp thử nghiệm```python
import io
import math

def solve_string(inp: str) -> str:
    data = list(map(int, inp.split()))
    t = data[0]
    ns = data[1:1 + t]

    def digit(n):
        s = math.isqrt(24 * n + 1)
        k = (1 + s) // 6

        while (k + 1) * (3 * (k + 1) - 1) // 2 <= n:
            k += 1
        while k * (3 * k - 1) // 2 > n:
            k -= 1

        a = k * (3 * k - 1) // 2
        b = k * (3 * k + 1) // 2

        if n == a or n == b:
            cur = 1 if k % 2 == 0 else -1
        else:
            cur = 0

        if n < b:
            next_sign = 1 if k % 2 == 0 else -1
        else:
            next_sign = -1 if k % 2 == 0 else 1

        if next_sign < 0:
            cur -= 1

        return cur % 10

    return " ".join(map(str, (digit(n) for n in ns)))

# Provided sample
assert solve_string(
    "15\n"
    "1 2 3 4 5 6 7 8 9 10 11 12 13 14 15\n"
) == "8 9 0 0 1 0 0 9 9 9 9 8 9 9 9", "sample 1"

# Minimum-size input
assert solve_string("1\n1\n") == "8", "minimum position"

# Consecutive positions around the first two pentagonal numbers
assert solve_string("5\n4 5 6 7 8\n") == "0 1 0 0 9", "first boundaries"

# Repeated equal values
assert solve_string("6\n3 3 3 3 3 3\n") == "0 0 0 0 0 0", "all equal"

# Boundary around the next pair
assert solve_string("7\n11 12 13 14 15 16 17\n") == "9 8 9 9 9 0 0", "second boundaries"

# Maximum allowed n
assert solve_string("1\n1000000000000000000\n") == "0", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`8`| Vị trí pháp lý nhỏ nhất và hệ số âm đầu tiên. | 
|`5 / 4 5 6 7 8`|`0 1 0 0 9`| Các ranh giới ngay trước, tại và sau cặp ngũ giác đầu tiên. | 
|`6 / 3 3 3 3 3 3`|`0 0 0 0 0 0`| Các truy vấn giống hệt nhau lặp đi lặp lại và vùng có hệ số 0. | 
|`7 / 11 12 13 14 15 16 17`|`9 8 9 9 9 0 0`| Cặp ngũ giác thứ hai và cả hai đều thay đổi dấu hiệu đuôi. | 
|`1 / 1000000000000000000`|`0`| Chỉ số lớn nhất có thể và số học căn bậc hai số nguyên. | 

## Vỏ cạnh 

Với (n=1), đầu vào```
1
1
```có (k=1), với (P_1^-=1) và (P_1^+=2). Hệ số hiện tại là (-1), hệ số tiếp theo cũng âm. Thuật toán trừ thêm một nữa, thu được (-2) và (-2\bmod10=8). Do đó, đầu ra chính xác là`8`. 

Với (n=7), đầu vào```
1
7
```hạ cánh chính xác trên (P_2^+=7). Hệ số của nó là (+1), nhưng số mũ tiếp theo là (P_3^-=12), có hệ số âm. Do đó, giá trị sàn sau khi chia tỷ lệ theo (10^7) sẽ nhỏ hơn một tiền tố số nguyên do hệ số cục bộ đề xuất. Thuật toán tính toán (1-1=0), cho kết quả đầu ra chính xác`0`. 

Với (n=8), đầu vào```
1
8
```ngay sau số mũ (7). Không có hệ số ở số mũ (8), nên giá trị hiện tại bằng 0. Số hạng bị lược bỏ đầu tiên là số hạng phủ định ở số mũ (12), buộc phải vay mượn. Kết quả là (-1\bmod10=9), khớp với phần mở rộng thập phân. 

Đối với vị trí được phép lớn nhất, đầu vào```
1
1000000000000000000
```có (k=816496580). Số mũ ngũ giác tổng quát thấp hơn của nó là 

999999997319296310, 
] 

trong khi cái trên là 

[ 
1000000813815876890. 
] 

Do đó (10^{18}) nằm chính xác giữa hai số mũ. Hệ số hiện tại bằng 0 và vì (k) chẵn nên hệ số tiếp theo là dương. Không có khoản vay nào xảy ra nên câu trả lời là`0`. Thuật toán đạt được kết quả này chỉ bằng cách sử dụng số học số nguyên và không bao giờ phụ thuộc vào bản thân việc khai triển số thập phân cực lớn.
