---
title: "CF 1866B - Chiến Đấu Với Những Con Số"
description: "Chúng ta có hai số nguyên, nhưng thay vì được viết ở dạng thập phân, chúng được mô tả bằng hệ số nguyên tố. Một số, gọi là $X$, được xác định đầy đủ bởi một danh sách các số nguyên tố và số mũ của chúng. Số $Y$ khác được mô tả theo cách tương tự."
date: "2026-06-08T23:43:57+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1866
codeforces_index: "B"
codeforces_contest_name: "COMPFEST 15 - Preliminary Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 1400
weight: 1866
solve_time_s: 83
verified: true
draft: false
---

[CF 1866B - Đấu tranh với những con số](https://codeforces.com/problemset/problem/1866/B) 

**Đánh giá:** 1400 
**Tags:** tổ hợp, toán, lý thuyết số 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai số nguyên, nhưng thay vì được viết ở dạng thập phân, chúng được mô tả bằng hệ số nguyên tố. Một số, gọi nó$X$, được xác định đầy đủ bởi danh sách các số nguyên tố và số mũ của chúng. Số khác$Y$được mô tả theo cùng một cách. 

Nhiệm vụ là đếm xem có bao nhiêu cặp có thứ tự$(p, q)$các số nguyên dương tồn tại sao cho bội số chung nhỏ nhất của$p$Và$q$bằng$X$, và ước chung lớn nhất của$p$Và$q$bằng$Y$. Câu trả lời phải được tính theo modulo$998244353$. 

Một cách hữu ích để suy nghĩ về các ràng buộc là cả hai con số đều có thể cực kỳ lớn, lên tới khoảng$2 \times 10^6$trong các giá trị nguyên tố và số mũ lớn. Số số nguyên tố phân biệt lên tới$10^5$, do đó, bất kỳ giải pháp nào cũng phải xử lý từng số nguyên tố một cách độc lập và hoạt động về cơ bản là tuyến tính theo thời gian trên danh sách thừa số. Bất cứ điều gì bậc hai về số số nguyên tố hoặc số mũ đều không thể thực hiện được trong vòng một giây. 

Yêu cầu cấu trúc quan trọng xuất hiện ngay từ định nghĩa của gcd và lcm. Nếu một số nguyên tố xuất hiện trong$Y$, nó cũng phải xuất hiện trong$X$, vì gcd không thể chứa các số nguyên tố không có trong cả hai số và lcm phải chứa tất cả các số nguyên tố từ cả hai số. Nếu điều kiện này không thành công với bất kỳ số nguyên tố nào thì câu trả lời phải bằng 0. Một giải pháp bất cẩn mà bỏ qua việc kiểm tra tính nhất quán này sẽ tạo ra những câu trả lời tích cực không chính xác trong những trường hợp như$X = 2^1 \cdot 5^1$,$Y = 3^1$, nơi không có cặp nào có thể tồn tại. 

Một vấn đề tế nhị khác là các số nguyên tố không có trong$Y$nhưng có mặt ở$X$vẫn đóng góp sự lựa chọn. Nhiều giải pháp không chính xác hoặc quên xử lý các số nguyên tố này hoặc xử lý chúng một cách đối xứng bằng các số nguyên tố gcd, điều này phá vỡ tính chính xác. 

Cuối cùng, vấn đề thứ tự: cặp$(p,q)$được sắp xếp theo thứ tự, do đó việc hoán đổi chúng sẽ tạo ra các nghiệm hợp lệ khác nhau trừ khi$p = q$. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng xây dựng tất cả các cặp$(p,q)$sao cho gcd và lcm của chúng khớp với các giá trị yêu cầu. Người ta có thể thử liệt kê tất cả các phép gán số mũ có thể có cho mỗi số nguyên tố trong cả hai số. Đối với một số nguyên tố cho trước, nếu số mũ của nó bằng$X$là$a$, thì số mũ trong$p$Và$q$phải nằm giữa các giới hạn nhất định được quy định bởi các ràng buộc gcd và lcm. Tuy nhiên, ngay cả đối với một số nguyên tố duy nhất, việc liệt kê tất cả các cặp số mũ hợp lệ cũng được, nhưng thực hiện việc đó một cách độc lập trên tối đa$10^5$số nguyên tố vẫn yêu cầu tính toán cẩn thận hơn là tạo rõ ràng. 

Quan sát quan trọng là các ràng buộc gcd và lcm tách rời hoàn toàn trên các số nguyên tố. Đối với mỗi số nguyên tố$x$, giả sử số mũ của nó trong$X$là$a$và trong$Y$là$b$. Nếu như$b > a$, không có nghiệm nào, vì gcd không thể vượt quá lcm ở bất kỳ số mũ nguyên tố nào. 

Bây giờ sửa một số nguyên tố duy nhất. Đặt số mũ của số nguyên tố đó vào$p$là$i$, và trong$q$là$j$. Khi đó điều kiện gcd sẽ buộc$\min(i,j) = b$, và các lực điều kiện lcm$\max(i,j) = a$. Điều này ngay lập tức ngụ ý rằng cả hai$i$Và$j$phải nằm trong tập hợp$\{b, a\}$, nhưng có ít nhất một giá trị bằng mỗi điểm cuối. Điều đó mang lại chính xác hai khả năng cho mỗi lần gán cặp theo thứ tự: hoặc$i=b, j=a$hoặc$i=a, j=b$, nhưng chỉ khi$a \ne b$. Nếu như$a = b$, thì cả hai phải bằng nhau, đưa ra chính xác một lựa chọn. 

Đối với các số nguyên tố không xuất hiện trong$Y$, chúng tôi có$b = 0$. Trong trường hợp đó,$\min(i,j)=0$buộc ít nhất một trong số$i,j$bằng 0, trong khi$\max(i,j)=a$buộc người khác phải chính xác$a$. Một lần nữa, chúng ta nhận được chính xác hai cấu hình có thứ tự cho mỗi số nguyên tố như vậy, miễn là$a > 0$. 

Toàn bộ vấn đề quy về việc nhân các đóng góp độc lập trên tất cả các số nguyên tố trong$X$, nhưng chỉ khi tất cả các số nguyên tố trong$Y$được chứa trong$X$. Nếu một số nguyên tố tồn tại trong$Y$nhưng không ở trong$X$, câu trả lời là 0 ngay lập tức. 

Vì vậy, mỗi số nguyên tố đóng góp 1 hoặc 2 cách tùy thuộc vào số mũ của nó trong$X$bằng với điều đó trong$Y$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ của số nguyên tố | O(1)-O(n) | Quá chậm | 
| Tối ưu | O(N + M) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý cả hai danh sách thừa số nguyên tố dưới dạng bài toán so sánh giữa hai bản đồ. 

1. Lưu số mũ của mỗi số nguyên tố vào$X$trong bản đồ băm hoặc từ điển. 

Điều này cho phép tra cứu liên tục khi kiểm tra các số nguyên tố từ$Y$. 
2. Khởi tạo câu trả lời là 1. 

Chúng tôi sẽ nhân các khoản đóng góp độc lập từ mỗi số nguyên tố. 
3. Lặp lại từng số nguyên tố$C_j$TRONG$Y$. 

Đối với mỗi cái, hãy kiểm tra xem nó có tồn tại trong$X$. Nếu nó không tồn tại thì gcd không có cách nào chứa một số nguyên tố vắng mặt trong lcm, do đó đáp án trở thành 0 và chúng ta có thể dừng lại. 
4. Đặt số mũ của số nguyên tố này trong$X$là$a$, và trong$Y$là$b$. 

Nếu như$b > a$, một lần nữa chúng ta kết luận ngay là không thể vì gcd không thể vượt quá lcm ở bất kỳ thành phần nguyên tố nào. 
5. Nếu$a = b$, số nguyên tố này đóng góp đúng 1 cách. 

Cả hai$p$Và$q$phải chứa số nguyên tố này có cùng số mũ, nên không tồn tại tự do. 
6. Nếu$a > b$, số nguyên tố này đóng góp đúng 2 cách. 

Một số lấy số mũ$a$và cái còn lại lấy số mũ$b$và vì cặp này được sắp thứ tự nên cả hai phép gán đều hợp lệ. 
7. Nhân phần đóng góp vào câu trả lời modulo$998244353$. 

Sau khi xử lý tất cả các số nguyên tố trong$Y$, mọi số nguyên tố trong$X$cái đó không có trong$Y$ngầm cư xử như$b=0$, và chúng đã được tính đến nếu chúng ta mở rộng lý do tương tự: mỗi số nguyên tố như vậy cũng đóng góp đúng 2 cách. Tuy nhiên, chúng ta không cần phải lặp lại chúng một cách riêng biệt nếu chúng ta cấu trúc tính toán một cách cẩn thận: chúng ta theo dõi có bao nhiêu số nguyên tố trong$X$cuộc thi đấu$Y$, và có bao nhiêu điểm khác nhau. 

Việc triển khai rõ ràng sẽ lặp lại trực tiếp trên tất cả các số nguyên tố trong$X$, kiểm tra sự hiện diện trong$Y$và nhân các khoản đóng góp tương ứng. 

### Tại sao nó hoạt động 

Đối với mỗi số nguyên tố, các ràng buộc$\gcd(p,q)$Và$\text{lcm}(p,q)$áp đặt các điều kiện độc lập lên số mũ của nó trong$p$Và$q$. Vì gcd và lcm được tính riêng cho từng số nguyên tố nên việc lựa chọn một số nguyên tố không ảnh hưởng đến số nguyên tố khác. Điều này tạo ra một cấu trúc sản phẩm trên số nguyên tố. Mỗi số nguyên tố đóng góp một số lượng phép gán số mũ hợp lệ cố định và tổng số cặp là tích của những đóng góp này. Tính đúng đắn xuất phát từ tính duy nhất của việc phân tích thừa số nguyên tố và tính độc lập của các ràng buộc số mũ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n = int(input())
    A = list(map(int, input().split()))
    B = list(map(int, input().split()))
    
    x_exp = {}
    for p, e in zip(A, B):
        x_exp[p] = e

    m = int(input())
    C = list(map(int, input().split()))
    D = list(map(int, input().split()))
    
    y_exp = {}
    for p, e in zip(C, D):
        y_exp[p] = e

    # consistency check: all primes in Y must exist in X
    for p, e in y_exp.items():
        if p not in x_exp or e > x_exp[p]:
            print(0)
            return

    ans = 1

    # process primes in X (covers both shared and extra primes)
    for p, a in x_exp.items():
        b = y_exp.get(p, 0)

        if a == b:
            ans = (ans * 1) % MOD
        else:
            ans = (ans * 2) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng hai từ điển ánh xạ các số nguyên tố thành số mũ. Vòng lặp đầu tiên đảm bảo tính khả thi: mọi số nguyên tố trong$Y$phải xuất hiện trong$X$với số mũ ít nhất bằng lớn. Vòng lặp thứ hai tính toán đóng góp theo cấp số nhân của mỗi số nguyên tố trong$X$. Thiếu số nguyên tố trong$Y$được coi là số mũ bằng 0, mô hình chính xác các ràng buộc gcd. 

Một cạm bẫy phổ biến là chỉ lặp lại các số nguyên tố trong$Y$. Điều đó tính thấp hơn sự đóng góp từ các số nguyên tố có trong$X$nhưng vắng mặt ở$Y$, mỗi trong số đó vẫn cung cấp hệ số 2. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
X: 2^2 * 3^1 * 5^1 * 7^2
Y: 3^1 * 7^1
```Chúng tôi tính toán trên mỗi khoản đóng góp chính. 

| Thủ tướng | kinh nghiệm trong X (a) | kinh nghiệm ở Y (b) | Đóng góp | 
| --- | --- | --- | --- | 
| 2 | 2 | 0 | 2 | 
| 3 | 1 | 1 | 1 | 
| 5 | 1 | 0 | 2 | 
| 7 | 2 | 1 | 2 | 

Câu trả lời cuối cùng:$2 \cdot 1 \cdot 2 \cdot 2 = 8$Điều này khẳng định rằng mọi sự không phù hợp giữa$a$Và$b$nhân đôi số lượng bài tập, trong khi sự bình đẳng cố định cấu trúc. 

### Ví dụ 2 

đầu vào:```
X = 2^1 * 5^1
Y = 2^1 * 3^1
```| Thủ tướng | ở X | ở Y | Có hiệu lực? | 
| --- | --- | --- | --- | 
| 2 | 1 | 1 | được | 
| 5 | 1 | 0 | được | 
| 3 | 0 | 1 | không hợp lệ | 

Sự có mặt của số nguyên tố 3 trong$Y$nhưng không ở trong$X$ngay lập tức làm mất hiệu lực việc xây dựng, tạo ra đầu ra 0. Điều này cho thấy tại sao việc kiểm tra tính khả thi là cần thiết trước khi tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + M) | mỗi số nguyên tố được xử lý một lần trong quá trình xây dựng và truyền tải bản đồ băm | 
| Không gian | O(N + M) | lưu trữ bản đồ số mũ | 

Các ràng buộc cho phép lên đến$10^5$số nguyên tố, do đó quét tuyến tính với các thao tác bản đồ băm phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    A = list(map(int, input().split()))
    B = list(map(int, input().split()))

    x = {}
    for p, e in zip(A, B):
        x[p] = e

    m = int(input())
    C = list(map(int, input().split()))
    D = list(map(int, input().split()))

    y = {}
    for p, e in zip(C, D):
        y[p] = e

    for p, e in y.items():
        if p not in x or e > x[p]:
            return "0"

    ans = 1
    for p, a in x.items():
        b = y.get(p, 0)
        if a != b:
            ans = (ans * 2) % MOD

    return str(ans)

# provided sample
assert run("""4
2 3 5 7
2 1 1 2
2
3 7
1 1
""") == "8"

# Y not subset of X
assert run("""2
2 5
1 1
1
3
1
""") == "0"

# identical X and Y
assert run("""2
2 3
1 1
2
2 3
1 1
""") == "1"

# all primes differ
assert run("""3
2 3 5
2 2 2
0

""") == "8"

# large single prime equal
assert run("""1
2
5
1
2
5
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Y không phải tập hợp con | 0 | phát hiện nguyên tố gcd không hợp lệ | 
| giống hệt nhau X=Y | 1 | mọi ràng buộc chặt chẽ, không có tự do | 
| tất cả các số nguyên tố đều khác nhau | 8 | mỗi số nguyên tố đóng góp hệ số 2 | 
| số nguyên tố đơn bằng nhau | 1 | trường hợp biên có đẳng thức | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là khi$Y$chứa một số nguyên tố không có trong$X$. Trong tình huống đó, gcd sẽ yêu cầu một thừa số nguyên tố mà lcm không có, điều này là không thể. Thuật toán nắm bắt điều này ngay lập tức trong vòng lặp khả thi trước khi xảy ra bất kỳ phép nhân nào. 

Một trường hợp cạnh khác là khi$X = Y$. Mọi số mũ nguyên tố đều khớp chính xác, do đó không có sự linh hoạt trong việc gán số mũ cho$p$Và$q$. Mỗi số nguyên tố đóng góp chính xác một cấu hình và câu trả lời cuối cùng là 1. Thuật toán xử lý việc này một cách tự nhiên vì không có số nguyên tố nào kích hoạt quy tắc nhân đôi. 

Trường hợp cạnh thứ ba xảy ra khi$Y = 1$. Ở đây tất cả các số nguyên tố trong$X$thực sự là miễn phí và mỗi phép tính đóng góp hai phép tính tương ứng với việc phân phối số mũ đầy đủ giữa$p$Và$q$. Thuật toán giảm xuống nhân 2 cho mỗi số nguyên tố trong$X$, sản xuất$2^{|X|}$, điều đó đúng. 

Trường hợp cuối cùng là khi$X$có một số nguyên tố duy nhất. Thuật toán rút gọn thành một lựa chọn nhị phân đơn giản tùy thuộc vào việc số nguyên tố đó có xuất hiện trong$Y$có cùng số mũ hoặc số 0, xác nhận tính đúng đắn trong cấu trúc không tầm thường nhỏ nhất.
