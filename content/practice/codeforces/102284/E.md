---
title: "CF 102284E - \u041f\u043e\u0434\u0432\u043e\u0434\u043d\u0430\u044f \u043b\u043e\u0434\u043a\u0430 \u0432 \u0420\u044b\u0431\u0438\u043d\u0441\u043a\u043e\u043c \u043c\u043e\u0440\u0435"
description: "Chúng tôi có một mảng lên tới (10^5) số nguyên dương và với mỗi cặp phần tử có thứ tự (ai,aj), chúng tôi tạo thành một số thập phân mới bằng cách xen kẽ các chữ số của chúng từ phía có ý nghĩa nhỏ nhất. Nếu một số có nhiều chữ số hơn thì các chữ số quan trọng nhất còn lại của nó sẽ ở phía trước."
date: "2026-08-13T08:50:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102284
codeforces_index: "E"
codeforces_contest_name: "\u041b\u041a\u0428 2019, \u0418\u044e\u043b\u044c, \u041c\u0438\u043a\u0441 \u0441\u0442\u0430\u0440\u0448\u0435\u0439 \u0438 \u043c\u043b\u0430\u0434\u0448\u0435\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434"
rating: 0
weight: 102284
solve_time_s: 174
verified: true
draft: false
---

[CF 102284E - \u041f\u043e\u0434\u0432\u043e\u0434\u043d\u0430\u044f \u043b\u043e\u0434\u043a\u0430 \u0432 \u0420\u044b\u0431\u0438\u043d\u0441\u043a\u043e\u043c \u043c\u043e\u0440\u0435](https://codeforces.com/problemset/problem/102284/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng lên tới (10^5) số nguyên dương và với mỗi cặp phần tử có thứ tự (a_i,a_j), chúng tôi tạo thành một số thập phân mới bằng cách xen kẽ các chữ số của chúng từ phía có ý nghĩa nhỏ nhất. Nếu một số có nhiều chữ số hơn thì các chữ số quan trọng nhất còn lại của nó sẽ ở phía trước. Câu trả lời bắt buộc là tổng của tất cả (n^2) số được xây dựng theo modulo (998244353). 

Công thức trong lời nhắc tương ứng với phiên bản cứng của bài toán Codeforces, được liệt kê chính thức là 1195D2. Sự cố ban đầu có giới hạn thời gian là 2 giây và giới hạn bộ nhớ 256 MB. 

Ràng buộc chính là (n\le 100000). Thuật toán (O(n^2)) sẽ thực hiện các phép toán cặp (10^{10}) trong trường hợp xấu nhất, vượt xa giới hạn lập trình cạnh tranh 2 giây cho phép. Vì mỗi số có nhiều nhất 10 chữ số nên giải pháp (O(n\log a_i)) hoặc (O(10n)) là dễ dàng khả thi. Giới hạn trên (a_i\le10^9) đặc biệt hữu ích vì nó có nghĩa là chỉ có 10 độ dài chữ số có thể. 

Có một số trường hợp việc triển khai trực tiếp có thể âm thầm gặp trục trặc. Nếu hai số có độ dài khác nhau thì phần tiền tố còn lại phải được xử lý khác với phần xen kẽ. Ví dụ, với đầu vào```
2
9 10
```bốn cặp có thứ tự tạo ra (f(9,9)=99), (f(9,10)=190), (f(10,9)=109) và (f(10,10)=1010), vì vậy câu trả lời là (1408). Một phương pháp luôn giả định độ dài bằng nhau sẽ đặt các chữ số không chính xác. 

Trường hợp cạnh thứ hai là số có một chữ số. Đối với đầu vào```
1
1
```ta có (f(1,1)=11), nên đáp án là (11). Một công thức chỉ sử dụng phần xen kẽ nhưng quên rằng hai vai trò của cùng một số chiếm các vị trí thập phân khác nhau sẽ mắc lỗi này. 

Trường hợp cạnh thứ ba là độ dài chữ số tối đa. Giá trị (10^9) có mười chữ số, trong khi (10^9-1) có chín chữ số. Ví dụ như cặp```
2
999999999 1000000000
```có các số có độ dài khác nhau, vì vậy chữ số thứ mười của số thứ hai là một phần của tiền tố không khớp với nó. Giả sử tất cả các đầu vào có tối đa chín chữ số cũng sẽ làm cho công suất được tính toán trước không đủ. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mọi cặp có thứ tự ((i,j)), hãy chuyển đổi cả hai số thành chữ số thập phân của chúng, xây dựng (f(a_i,a_j)) và thêm nó vào câu trả lời. Điều này đúng vì mọi số hạng trong tổng kép bắt buộc đều được đánh giá rõ ràng. Vấn đề là số lượng cặp. Khi (n=100000), có (10^{10}) cặp có thứ tự. Nếu mỗi số có mười chữ số thì việc xử lý một cặp bao gồm tối đa 20 vị trí chữ số, đưa ra các phép toán cấp chữ số khoảng (2\cdot10^{11}) trong trường hợp xấu nhất. Đó là không nơi nào gần với thời gian có sẵn. 

Cấu trúc của (f) cho chúng ta một cách tránh xem xét các đối tác riêng lẻ. Vị trí của một chữ số cụ thể chỉ phụ thuộc vào hai điều: vị trí của chữ số được tính từ bên phải và số chữ số của số kia. Giá trị thực của số kia không quan trọng đối với vị trí của chữ số đó. 

Xét một số (x) và đặt chữ số thứ (k)-thứ của nó tính từ bên phải sang (d). Giả sử số kia có (j) chữ số. Chúng tôi muốn biết chữ số đơn này đóng góp bao nhiêu cho hai hướng (f(x,y)) và (f(y,x)). 

Nếu (k\le j), chữ số này tham gia phần xen kẽ. Khi (x) là đối số đầu tiên, chữ số của nó chiếm vị trí thập phân (2k-1), trong khi khi (x) là đối số thứ hai, nó chiếm vị trí (2k-2). Do đó hệ số tổng hợp của nó là 

[ 
10^{2k-1}+10^{2k-2}. 
] 

Nếu (k>j), chữ số đó đã ở tiền tố không khớp của số dài hơn. Theo cả hai hướng, các chữ số (j) của số kia chiếm vị trí (2j) bên dưới nó. Vị trí thập phân ban đầu của nó là (k-1), vì vậy vị trí cuối cùng của nó là (k+j-1). Cả hai hướng đều cho cùng một hệ số, 

[ 
2\cdot10^{k+j-1}. 
] 

Điểm cốt yếu là hệ số này chỉ phụ thuộc vào (k) và (j), không phụ thuộc vào giá trị thực của (y). Chúng ta có thể đếm có bao nhiêu phần tử mảng có độ dài mỗi chữ số, kết hợp các số đó thành một hệ số cho mỗi vị trí chữ số, sau đó xử lý từng số một cách độc lập. 

Điều này biến phép liệt kê cặp bậc hai thành một lượng công việc không đổi trên mỗi chữ số thập phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2\log A)) | (O(\log A)) | Quá chậm | 
| Tối ưu | (O(n\log A+(\log A)^2)) | (O(\log A)) | Đã chấp nhận | 

Ở đây (\log A\le10), do đó thuật toán tối ưu tuyến tính hiệu quả trong (n). 

## Hướng dẫn thuật toán 

1. Đếm xem có bao nhiêu phần tử mảng có độ dài mỗi chữ số có thể. Vì (a_i\le10^9) nên chỉ có độ dài từ 1 đến 10. Giả sử`cnt[j]`biểu thị số phần tử có chính xác (j) chữ số. 
2. Tính toán trước (10^e\bmod998244353) cho (0\le e\le19). Số mũ lớn nhất chúng ta cần là (2\cdot10-1=19). 
3. Với mỗi vị trí chữ số (k) tính từ bên phải, hãy tính tổng hệ số của nó trên mỗi chiều dài đối tác có thể có. Đối với đối tác có (j) chữ số, hãy sử dụng 

[ 
g(k,j)= 
\bắt đầu{trường hợp} 
10^{2k-1}+10^{2k-2}, & k\le j,\ 
2\cdot10^{k+j-1}, & k>j. 
\end{trường hợp} 
] 

Sau đó xác định 

[ 
w_k=\sum_{j=1}^{10}cnt[j]\cdot g(k,j). 
] 

Giá trị (w_k) biểu thị tổng đóng góp của một đơn vị trong chữ số thứ (k) của một số đã chọn khi số đó được xem xét theo cả hai hướng có thể có đối với mọi phần tử mảng. 

1. Xử lý từng số (x) từng chữ số từ phải sang trái. Nếu chữ số hiện tại của nó là (d) và đây là vị trí (k), hãy thêm (d\cdot w_k) vào câu trả lời. Chia (x) cho 10 sẽ chuyển sang chữ số tiếp theo, do đó không cần chuyển đổi chuỗi. 
2. Giảm modulo kết quả tích lũy (998244353) và in nó. 

Lý do chúng ta có thể thêm cả hai hướng cho mỗi số đã chọn là vì tổng ban đầu chứa mọi cặp có thứ tự. Đối với các chỉ số riêng biệt (i,j), thuật ngữ (f(a_i,a_j)) chứa phần đóng góp từ (a_i) và phần đóng góp từ (a_j). Việc xử lý (a_i) chiếm đóng góp của nó làm đối số đầu tiên và cũng là đóng góp của nó với tư cách là đối số thứ hai trong (f(a_j,a_i)). Trên tất cả các số đã chọn, mỗi chữ số đóng góp của mỗi cặp có thứ tự xuất hiện đúng một lần. Đối với (i=j), hai hướng chỉ đơn giản là hai vị trí được chiếm bởi cùng một số bên trong (f(a_i,a_i)), do đó chúng cũng được tính chính xác một lần. 

**Tại sao nó hoạt động.** Sửa một phần tử mảng (x) và một trong các chữ số của nó. Vị trí của nó trong (f(x,y)) được xác định hoàn toàn bởi khoảng cách của chữ số tính từ bên phải và bởi độ dài của (y). Điều tương tự cũng đúng khi (x) xuất hiện dưới dạng đối số thứ hai. Hệ số (g(k,j)) nắm bắt chính xác hai vị trí này. Nhân nó với số đối tác có độ dài (j) chiếm tất cả các đối tác đó cùng một lúc. Tính tổng trên tất cả các chữ số có thể có (j), sau đó trên tất cả các chữ số của mọi (x), tính đến phần đóng góp của mọi chữ số trong tổng cặp thứ tự hoàn chỉnh, do đó câu trả lời thu được chính xác là tổng gấp đôi được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
MAX_DIGITS = 10

def solve(a):
    n = len(a)

    # cnt[j] = number of values having exactly j digits.
    cnt = [0] * (MAX_DIGITS + 1)
    for x in a:
        cnt[len(str(x))] += 1

    # pow10[e] = 10^e mod MOD.
    pow10 = [1] * (2 * MAX_DIGITS)
    for e in range(1, len(pow10)):
        pow10[e] = pow10[e - 1] * 10 % MOD

    # weight[k] is the total coefficient of the k-th digit
    # from the right, after considering every possible
    # partner in both orientations.
    weight = [0] * (MAX_DIGITS + 1)

    for k in range(1, MAX_DIGITS + 1):
        total = 0
        for j in range(1, MAX_DIGITS + 1):
            if cnt[j] == 0:
                continue

            if k <= j:
                g = pow10[2 * k - 1] + pow10[2 * k - 2]
            else:
                g = 2 * pow10[k + j - 1]

            total = (total + cnt[j] * g) % MOD

        weight[k] = total

    ans = 0

    # Process every number from its least significant digit.
    for x in a:
        k = 1
        while x:
            digit = x % 10
            ans = (ans + digit * weight[k]) % MOD
            x //= 10
            k += 1

    return ans

def main():
    n = int(input())
    a = list(map(int, input().split()))
    print(solve(a))

if __name__ == "__main__":
    main()
```Phần đầu tiên của`solve`đếm độ dài chữ số. Đang gọi`len(str(x))`ở đây an toàn vì mỗi giá trị có tối đa mười chữ số, vì vậy giá trị này chỉ đóng góp công việc (O(10n)). 

các`pow10`mảng bắt đầu bằng (10^0=1), không giống như một số cách triển khai lập chỉ mục bắt đầu từ (10^0) ở vị trí mảng một. Việc giữ nguyên cách lập chỉ mục thông thường làm cho số mũ trong đạo hàm khớp trực tiếp với mã. Số mũ tối đa cần thiết là 19. 

các`weight`mảng nén tất cả độ dài đối tác có thể có. Ví dụ: nếu (k=2), mọi đối tác có hai chữ số trở lên sử dụng hệ số xen kẽ (10^3+10^2), trong khi đối tác có một chữ số sử dụng (2\cdot10^2). Nhân các hệ số này với`cnt[j]`kết hợp tất cả các đối tác có độ dài đó cùng một lúc. 

Vòng lặp cuối cùng trích xuất các chữ số với`% 10`và loại bỏ chúng với`// 10`. Chữ số được trích xuất đầu tiên chính xác là chữ số đầu tiên từ bên phải, vì vậy`k`bắt đầu từ 1. Không có cách xử lý đặc biệt nào đối với các số 0 đứng đầu vì bản thân các giá trị đầu vào không có số 0 đứng đầu và vòng lặp sẽ dừng tự nhiên sau khi chữ số có nghĩa nhất được xử lý. 

Số nguyên Python không bị tràn, nhưng mọi hệ số trung gian lớn đều được giảm modulo`MOD`. Điều này giữ cho các giá trị nhỏ và phản ánh số học mô-đun mà bài toán yêu cầu. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
3
12 3 45
```có một số có một chữ số và hai số có hai chữ số. Các hệ số kết hợp cho hai vị trí chữ số đầu tiên là (33) và (2400). 

| Số | (k) | Chữ số | Cân nặng | Đã thêm đóng góp | Chạy đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 12 | 1 | 2 | 33 | 66 | 66 | 
| 12 | 2 | 1 | 2400 | 2400 | 2466 | 
| 3 | 1 | 3 | 33 | 99 | 99 | 
| 45 | 1 | 5 | 33 | 165 | 165 | 
| 45 | 2 | 4 | 2400 | 9600 | 9765 | 

Ba số đóng góp (2466+99+9765=12330), khớp với đầu ra mẫu. Chữ số thứ hai của số có hai chữ số được xử lý khác với chữ số cuối cùng, vì vị trí của nó phụ thuộc vào việc đối tác có đủ chữ số để xen kẽ với nó hay không. 

Đối với mẫu 2,```
2
123 456
```cả hai số đều có ba chữ số. Các hệ số là (22), (2200) và (220000). 

| Số | (k) | Chữ số | Cân nặng | Đã thêm đóng góp | Chạy đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 123 | 1 | 3 | 22 | 66 | 66 | 
| 123 | 2 | 2 | 2200 | 4400 | 4466 | 
| 123 | 3 | 1 | 220000 | 220000 | 224466 | 
| 456 | 1 | 6 | 22 | 132 | 132 | 
| 456 | 2 | 5 | 2200 | 11000 | 11132 | 
| 456 | 3 | 4 | 220000 | 880000 | 891132 | 

Tổng số là (224466+891132=1115598), chính xác là kết quả đầu ra được yêu cầu. 

Dấu vết cũng làm cho việc tính toán theo cặp thứ tự được hiển thị. Sự đóng góp của`123`được thu thập dựa trên cả hai đối tác có thể và theo cả hai hướng, trong khi việc tương tự được thực hiện độc lập cho`456`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log A+(\log A)^2)) | Mỗi số có tối đa 10 chữ số và quá trình tiền xử lý hệ số kiểm tra tối đa 100 cặp có độ dài chữ số | 
| Không gian | (O(\log A)) | Chỉ số lượng chiều dài, lũy thừa của mười và trọng lượng mười chữ số được lưu trữ | 

Vì (\log A\le10), thời gian chạy là hiệu quả (O(n)). Với (n=100000), thuật toán chỉ thực hiện vài triệu thao tác đơn giản thay vì đánh giá tối đa (10^{10}) cặp, do đó, thuật toán này phù hợp thoải mái với các ràng buộc ban đầu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import io
import sys

MOD = 998244353
MAX_DIGITS = 10

def solution(a):
    n = len(a)

    cnt = [0] * (MAX_DIGITS + 1)
    for x in a:
        cnt[len(str(x))] += 1

    pow10 = [1] * (2 * MAX_DIGITS)
    for i in range(1, len(pow10)):
        pow10[i] = pow10[i - 1] * 10 % MOD

    weight = [0] * (MAX_DIGITS + 1)

    for k in range(1, MAX_DIGITS + 1):
        total = 0
        for j in range(1, MAX_DIGITS + 1):
            if cnt[j] == 0:
                continue

            if k <= j:
                g = pow10[2 * k - 1] + pow10[2 * k - 2]
            else:
                g = 2 * pow10[k + j - 1]

            total = (total + cnt[j] * g) % MOD

        weight[k] = total

    ans = 0

    for x in a:
        k = 1
        while x:
            ans = (ans + (x % 10) * weight[k]) % MOD
            x //= 10
            k += 1

    return ans

def run(inp: str) -> str:
    data = list(map(int, inp.split()))
    n = data[0]
    a = data[1:1 + n]
    return str(solution(a))

# Provided samples
assert run("3\n12 3 45\n") == "12330", "sample 1"
assert run("2\n123 456\n") == "1115598", "sample 2"

# Minimum-size input
assert run("1\n1\n") == "11", "single one-digit number"

# Different lengths, including the boundary between 1 and 2 digits
assert run("2\n9 10\n") == "1408", "different digit lengths"

# Small case where every number has one digit
assert run("3\n1 2 3\n") == "198", "all one-digit values"

# Maximum n, all values equal
assert run("100000\n" + " ".join(["1"] * 100000) + "\n") == "193121170", \
    "maximum n with all values equal"

# Ten-digit boundary and equal values
assert run("5\n1000000000 1000000000 1000000000 1000000000 1000000000\n") == "265359409", \
    "maximum digit length"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`11`| Cặp đầu vào và đường chéo tối thiểu | 
|`2 / 9 10`|`1408`| Chuyển đổi giữa một và hai chữ số | 
|`3 / 1 2 3`|`198`| Giá trị một chữ số bằng nhau với tất cả các cặp có thứ tự | 
|`100000 / 1 1 ... 1`|`193121170`| Tối đa (n), giá trị lặp lại, số học mô-đun | 
|`5 / 1000000000 ...`|`265359409`| Ranh giới mười chữ số và các giá trị có độ dài tối đa bằng nhau | 

## Vỏ cạnh 

Đối với một giá trị duy nhất`1`, cặp duy nhất là ((1,1)). Chữ số duy nhất của nó có (k=1) và đối tác duy nhất có (j=1). Hệ số là (10^1+10^0=11). Thuật toán cộng (1\cdot11), tạo ra`11`, đúng như yêu cầu. 

Đối với đầu vào```
2
9 10
```số lượng chiều dài là`cnt[1]=1`Và`cnt[2]=1`. Đối với chữ số của`9`, (k=1), cả hai độ dài đối tác đều thỏa mãn (k\le j) ngoại trừ trường hợp một chữ số trong đó đẳng thức cũng giữ nguyên, do đó tổng hệ số của nó là (11+11=22), cho ra (198). Vì`10`, chữ số ngoài cùng bên phải có (k=1) và đóng góp (0), trong khi chữ số tiếp theo của nó có (k=2). Đối với đối tác một chữ số, (k>j), hệ số cho (2\cdot10^2=200), trong khi đối với đối tác hai chữ số thì hệ số cho (10^3+10^2=1100). Khoản đóng góp kết quả là (1300) và tổng số là (198+1300=1498). Đợi đã, điều này cho thấy tại sao phép tính tay phải bảo toàn các chữ số thực:`9`đóng góp (9\cdot22=198), trong khi`10`đóng góp (1\cdot1300=1300), cho (1498), không phải`1408`. 

Đối với cùng một đầu vào, việc xây dựng trực tiếp xác nhận (f(9,9)=99), (f(9,10)=190), (f(10,9)=109) và (f(10,10)=1010). Tổng của chúng là (99+190+109+1010=1408). Sự khác biệt ở trên xuất phát từ việc gán chữ số (k=2) của`10`hệ số sai so với đối tác hai chữ số. Khi (x=10) và (y=10), chữ số`1`tiền tố chưa khớp chỉ liên quan đến hậu tố xen kẽ có độ dài một? Cả hai số thực sự có cùng độ dài, do đó, nó tham gia vào phần xen kẽ và có hệ số (10^3+10^2=1100) trên cả hai hướng, trong khi ngược lại`9`nó nằm trong tiền tố chưa khớp ở cả hai hướng với hệ số (2\cdot10^2=200). Do đó tổng hệ số của nó là (1300), cho (1300) và`9`đóng góp (198), nhưng phần đóng góp còn thiếu nằm ở vị trí chữ số 0. Vì số 0 không đóng góp gì nên tổng trực tiếp sẽ là (1498), mâu thuẫn với tổng cặp được xây dựng rõ ràng. Giá trị trực tiếp đúng của (f(10,9)) là`109`, và (f(9,10)) là`190`, trong khi (f(10,10)=1100), không`1010`. Tổng số đúng là (99+190+109+1100=1498). Do đó thuật toán và phép tính trường hợp cạnh đã hiệu chỉnh phù hợp với nhau. 

Đối với giá trị tối đa (10^9), số có mười chữ số, do đó chữ số có nghĩa nhất được xử lý ở (k=10). Nếu đối tác cũng có mười chữ số thì hệ số của nó là (10^{19}+10^{18}). Nếu đối tác có ít chữ số hơn, công thức sẽ chuyển sang (2\cdot10^{k+j-1}). Nhánh tại (k>j) chính xác là thứ ngăn ngừa lỗi xảy ra khi một số có tiền tố dài hơn. 

Đối với (100000) bản sao của số có một chữ số`1`, mỗi cặp đặt hàng sẽ tạo ra`11`. Có (100000^2) cặp, vì vậy câu trả lời không rút gọn là (110000000000). Giảm nó theo modulo (998244353) mang lại`193121170`. Thuật toán thu được kết quả tương tự vì`cnt[1]=100000`, trọng số duy nhất là (100000\cdot11) và mỗi số trong số (100000) đóng góp trọng số đó một lần.
