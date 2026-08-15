---
title: "CF 102420J - \u041c\u0430\u043b\u0435\u0444\u0438\u0441\u0443\u043c\u043c\u0430"
description: "Chúng ta có một mảng gồm (n) số nguyên không âm (a1,a2,ldots,an). Chúng ta cần tổng tích của ba phần tử riêng biệt, trong đó các chỉ số phải thỏa mãn (i<j<k): [ sum{1le i<j<kle n} ai aj ak."
date: "2026-08-12T00:54:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102420
codeforces_index: "J"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102420
solve_time_s: 106
verified: true
draft: false
---

[CF 102420J - \u041c\u0430\u043b\u0435\u0444\u0438\u0441\u0443\u043c\u043c\u0430](https://codeforces.com/problemset/problem/102420/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng gồm (n) số nguyên không âm (a_1,a_2,\ldots,a_n). Chúng ta cần tổng tích của ba phần tử riêng biệt, trong đó các chỉ số phải thỏa mãn (i<j<k): 

[ 
\sum_{1\le i<j<k\le n} a_i a_j a_k. 
] 

Thứ tự bên trong bộ ba được chọn không quan trọng, nhưng mỗi bộ ba vị trí khác nhau phải đóng góp chính xác một lần. Câu trả lời là bắt buộc theo modulo (10^9+7). 

Khó khăn đến từ kích thước của (n). Với (n\le 10^6), việc kiểm tra từng bộ ba là không thực tế chút nào. Số bộ ba là 

[ 
\binom{n}{3}, 
] 

đạt tới 

[ 
\binom{10^6}{3}=166666166667000000 
] 

ở kích thước tối đa. Ngay cả khi việc tính toán một sản phẩm chỉ mất một vài thao tác nguyên thủy, việc lặp lại khoảng ba lần (1,67\cdot10^{17}) là vượt xa mọi giới hạn thời gian hợp lý. 

Các giá trị (a_i) cũng có thể lớn bằng (10^6), vì vậy câu trả lời toán học là rất lớn. Chúng ta phải thực hiện modulo số học (10^9+7) và việc triển khai sẽ giảm các giá trị trung gian một cách thường xuyên thay vì cho phép tích lũy các số nguyên lớn không cần thiết. 

Có một số trường hợp đặc biệt có thể dẫn đến việc triển khai không chính xác. Với số lượng phần tử tối thiểu, đầu vào```
3
1 2 3
```có đúng một bộ ba, nên đáp án là (1\cdot2\cdot3=6). Việc triển khai vô tình yêu cầu bốn phần tử hoặc bắt đầu cập nhật tổng cặp trước khi thêm phần đóng góp vào câu trả lời, có thể xảy ra trường hợp ranh giới này sai. 

Giá trị 0 là một trường hợp hữu ích khác. Vì```
4
0 5 6 7
```mọi bộ ba chứa số 0 không đóng góp gì cả, chỉ để lại (5\cdot6\cdot7=210). Một công thức dựa trên phép chia hoặc dựa trên giả định rằng mọi giá trị đều dương có thể hoạt động không chính xác ở đây. 

Các giá trị lặp lại yêu cầu các vị trí, thay vì các giá trị số, phải được coi là các lựa chọn riêng biệt. Ví dụ,```
4
2 2 2 2
```có bốn bộ ba, mỗi bộ đóng góp (8), nên đáp án là (32). Việc triển khai cố gắng liệt kê các kết hợp giá trị riêng biệt thay vì kết hợp chỉ mục sẽ tính sai điều này. 

Cuối cùng, các giá trị rất lớn vẫn phải được xử lý chính xác theo modulo (10^9+7). Ví dụ,```
3
1000000 1000000 1000000
```có đáp án (10^{18}), bằng (49) modulo (10^9+7). Các giá trị đầu vào riêng lẻ vừa vặn thoải mái với các loại số nguyên tiêu chuẩn, nhưng tích và tổng tăng nhanh hơn nhiều. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp tuân theo định nghĩa theo nghĩa đen. Chúng ta chọn (i), sau đó (j>i), rồi (k>j), tính toán (a_i a_j a_k) và thêm nó vào câu trả lời. Điều này đúng vì mỗi bộ ba chỉ số hợp lệ được truy cập chính xác một lần. 

Vấn đề là số lần lặp lại. Đối với (n=10^6), ba vòng lặp lồng nhau thực hiện một lần cho mỗi vòng lặp 

[ 
\binom{10^6}{3}=166666166667000000 
] 

có thể gấp ba lần. Đó là (O(n^3)), quá chậm. 

Quan sát hữu ích là bộ ba chứa phần tử hiện tại không cần phải được xây dựng một cách rõ ràng. Giả sử chúng ta xử lý mảng từ trái sang phải và hiện đang kiểm tra (a_k). Mọi bộ ba mới kết thúc ở vị trí (k) đều có dạng 

[ 
a_i a_j a_k,\qquad i<j<k. 
] 

Chúng ta có thể tính ra (a_k): 

[ 
a_k\sum_{i<j<k}a_i a_j. 
] 

Vì vậy, trước khi xử lý (a_k), chúng ta chỉ cần biết tổng tích của từng cặp trong số các phần tử đã được xử lý. 

Gọi số lượng này`pair_sum`. Chúng ta cũng cần tổng của tất cả các phần tử trước đó, bởi vì khi (a_k) trở thành một phần của một cặp, mọi phần tử trước đó sẽ tạo thành một cặp như vậy với nó: 

a_k\sum_{i<k}a_i. 
] 

Như vậy hai giá trị đang chạy là đủ.`sum1`lưu trữ tổng của các phần tử trước đó và`sum2`lưu trữ tổng tích của tất cả các cặp giữa các phần tử trước đó. 

Khi xử lý (x=a_k), hiện có`sum2`biểu thị chính xác các cặp ((i,j)) với (i<j<k). Nhân nó với (x) sẽ thêm mọi bộ ba mới kết thúc tại (k): 

[ 
\text{answer}\mathrel{+}=x\cdot\text{sum2}. 
] 

Sau đó, (x) phải trở thành một phần của tất cả các cặp trong tương lai. Những cặp mới đó có tổng sản phẩm 

[ 
x\cdot\text{sum1}, 
] 

vì vậy chúng tôi cập nhật 

[ 
\text{sum2}\mathrel{+}=x\cdot\text{sum1}. 
] 

Cuối cùng, (x) có sẵn như một phần tử trước đó: 

[ 
\text{sum1}\mathrel{+}=x. 
] 

Thứ tự của những cập nhật này là cần thiết. Câu trả lời phải sử dụng câu cũ`sum2`, vì các bộ ba kết thúc ở vị trí hiện tại phải sử dụng hai vị trí trước đó. Nếu chúng tôi cập nhật`sum2`đầu tiên, phần tử hiện tại có thể vô tình được sử dụng hai lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^3)) | (O(1)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(1)) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`sum1 = 0`,`sum2 = 0`, Và`answer = 0`. Trước khi xử lý bất kỳ thứ gì, không có phần tử nào trước đó, không có cặp trước đó và không có bộ ba trước đó. 
2. Đọc từng giá trị mảng (x) từ trái qua phải. Chúng tôi xử lý các vị trí theo thứ tự tự nhiên để mỗi bộ ba mới được tạo tự động có các chỉ số theo thứ tự tăng dần. 
3. Thêm (x\cdot\text{sum2}) vào`answer`. Vào đúng thời điểm này,`sum2`chứa tất cả các sản phẩm (a_i a_j) với cả hai chỉ số ngay trước vị trí hiện tại. Nhân từng số đó với (x) sẽ tạo ra mọi bộ ba có chỉ số cuối cùng là vị trí hiện tại. 
4. Cập nhật`sum2`bằng cách thêm (x\cdot\text{sum1}). Đây`sum1`chứa mọi giá trị được xử lý trước đó, do đó, điều này tạo ra chính xác tất cả các cặp bao gồm phần tử hiện tại và một phần tử trước đó. 
5. Cập nhật`sum1`bằng cách thêm (x). Phần tử hiện tại bây giờ là phần tử trước đó cho mọi vị trí được xử lý sau này. 
6. Thực hiện modulo số học (10^9+7). Vì phép cộng và phép nhân tôn trọng số học mô-đun, nên việc giảm từng số lượng đang chạy theo mô-đun sẽ cho kết quả chính xác là số dư cuối cùng giống như khi tính số nguyên đầy đủ trước tiên. 

### Tại sao nó hoạt động 

Bất biến sau khi xử lý phần tử (k) đầu tiên là`sum1`bằng 

[ 
\sum_{i=1}^{k}a_i, 
] 

trong khi`sum2`bằng 

[ 
\sum_{1\le i<j\le k}a_i a_j, 
] 

và`answer`bằng 

[ 
\sum_{1\le i<j<l\le k}a_i a_j a_l. 
] 

Khi giá trị tiếp theo (a_{k+1}) xuất hiện, nhân giá trị cũ`sum2`bởi (a_{k+1}) cộng chính xác tất cả các bộ ba có chỉ số lớn nhất là (k+1). Đang cập nhật`sum2`sau đó thêm chính xác tất cả các cặp mới liên quan đến (a_{k+1}) và cập nhật`sum1`thêm phần tử mới vào chính nó. Không thể thêm bộ ba nào hai lần vì mỗi bộ ba được thêm vào khi chỉ mục lớn nhất của nó được xử lý và không bộ ba không hợp lệ nào có thể xuất hiện vì`sum2`chỉ chứa các cặp từ các vị trí trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1_000_000_007

def solve():
    n = int(input())

    sum1 = 0
    sum2 = 0
    answer = 0

    for _ in range(n):
        x = int(input().split()[0]) if False else None
```Đầu vào chứa tất cả (n) số trên dòng thứ hai, do đó, một vòng lặp truyền trực tiếp qua`input().split()`là thích hợp hơn trong Python. Việc thực hiện đầy đủ là:```python
import sys
input = sys.stdin.readline

MOD = 1_000_000_007

def solve():
    n = int(input())
    a = map(int, input().split())

    sum1 = 0
    sum2 = 0
    answer = 0

    for x in a:
        answer = (answer + x * sum2) % MOD
        sum2 = (sum2 + x * sum1) % MOD
        sum1 = (sum1 + x) % MOD

    print(answer)

if __name__ == "__main__":
    solve()
```Biến trạng thái đầu tiên,`sum1`, biểu thị tổng đối xứng cơ bản của bậc một trên tiền tố được xử lý. Thứ hai,`sum2`, đại diện cho tổng đối xứng bậc hai. Câu trả lời chính là tổng đối xứng bậc ba. 

Câu trả lời được cập nhật trước`sum2`. Đây là phần tinh tế nhất của việc thực hiện. Hãy xem xét giá trị hiện tại (x). Trước khi xử lý nó,`sum2`chứa các cặp được hình thành độc quyền từ các phần tử trước đó, đây chính xác là những gì cần thiết để tạo bộ ba hợp lệ với (x). Sau khi cập nhật câu trả lời, thêm (x\cdot\text{sum1}) vào`sum2`chuẩn bị trạng thái cho các phần tử sau này. 

Mã lấy các giá trị với`map`, do đó, nó không xây dựng danh sách số nguyên Python thứ hai từ đầu vào. Bản thân dòng đầu vào vẫn được mã hóa bởi`split`, phù hợp với quy mô bài toán này và giữ cho việc thực hiện đơn giản. 

Số nguyên Python không bị tràn nhưng việc giảm mô-đun vẫn được thực hiện sau mỗi lần cập nhật trạng thái. Bên cạnh việc giữ cho các con số nhỏ, điều này làm cho sự tương ứng với tính toán mô-đun toán học trở nên rõ ràng. 

Không có cách xử lý đặc biệt nào cho hai yếu tố đầu tiên. Ban đầu`sum2`bằng 0, vì vậy hai giá trị đầu tiên không thể đóng góp một bộ ba. Sau khi hai giá trị được xử lý,`sum2`chứa sản phẩm của họ và giá trị thứ ba tạo ra bộ ba có thể đầu tiên một cách tự nhiên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
3
1 2 3
```Bảng sau theo dõi trạng thái trước và sau khi mỗi giá trị được xử lý. 

| Giá trị hiện tại (x) |`sum1`trước |`sum2`trước |`answer`trước |`answer`sau |`sum2`sau |`sum1`sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 0 | 0 | 1 | 
| 2 | 1 | 0 | 0 | 0 | 2 | 3 | 
| 3 | 3 | 2 | 0 | 6 | 11 | 6 | 

Sau khi xử lý (1) vẫn chưa có cặp nào. Sau khi xử lý (2), cặp duy nhất có tích (1\cdot2=2). Khi (3) đến, cái cũ`sum2`là (2), do đó thuật toán sẽ thêm (3\cdot2=6) vào câu trả lời. Đây chính xác là bộ ba duy nhất có thể. 

### Mẫu 2 

Đầu vào là```
4
0 5 6 7
```Nhà nước phát triển như sau. 

| Giá trị hiện tại (x) |`sum1`trước |`sum2`trước |`answer`trước |`answer`sau |`sum2`sau |`sum1`sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 0 | 0 | 0 | 
| 5 | 0 | 0 | 0 | 0 | 0 | 5 | 
| 6 | 5 | 0 | 0 | 0 | 30 | 11 | 
| 7 | 11 | 30 | 0 | 210 | 107 | 18 | 

Số 0 không đóng góp gì cho mọi sản phẩm, do đó, hai giá trị khác 0 đầu tiên cuối cùng sẽ tạo ra cặp (5\cdot6=30). Khi (7) được xử lý, cặp đó tạo ra (30\cdot7=210). Các bộ ba khác đều chứa số 0 ban đầu và đóng góp số 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Mỗi phần tử mảng được xử lý chính xác một lần với số học theo thời gian không đổi. | 
| Không gian | (O(1)) thêm | Chỉ có ba tổng mô-đun đang chạy được duy trì. | 

Đối với (n\le10^6), một đường chuyền tuyến tính chỉ thực hiện một vài phép toán số học cho mỗi phần tử, trong khi phương pháp brute-force sẽ yêu cầu khoảng (1,67\cdot10^{17}) lần lặp ba lần ở giới hạn trên. Giải pháp tuyến tính nằm trong phạm vi dự định của vấn đề một cách thoải mái và tránh lưu trữ bất kỳ cấu trúc mảng bổ sung nào. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm bên dưới sử dụng phép lặp tương tự như giải pháp đã gửi nhưng gói nó trong một hàm để có thể kiểm tra mọi trường hợp bằng`assert`. Trường hợp kích thước tối đa sử dụng một triệu số 0, giúp giữ cho đầu vào được tạo đơn giản trong khi thực hiện ranh giới đầu vào và vòng lặp thực tế.```python
import sys
import io

MOD = 1_000_000_007

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = map(int, input().split())

    sum1 = 0
    sum2 = 0
    answer = 0

    for x in a:
        answer = (answer + x * sum2) % MOD
        sum2 = (sum2 + x * sum1) % MOD
        sum1 = (sum1 + x) % MOD

    return str(answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        return solve()
    finally:
        sys.stdin = old_stdin

# Provided samples
assert run("3\n1 2 3\n") == "6", "sample 1"
assert run("4\n0 5 6 7\n") == "210", "sample 2"

# Minimum-size input
assert run("3\n0 0 0\n") == "0", "minimum size with zeros"

# All values equal:
# C(4, 3) * 2^3 = 4 * 8 = 32
assert run("4\n2 2 2 2\n") == "32", "all equal values"

# Maximum allowed value
# 1,000,000^3 mod 1,000,000,007 = 49
assert run("3\n1000000 1000000 1000000\n") == "49", "maximum value boundary"

# Maximum n, exercising the O(n) loop.
max_n = 1_000_000
max_input = str(max_n) + "\n" + ("0 " * (max_n - 1)) + "0\n"
assert run(max_input) == "0", "maximum n"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 0 0 0`|`0`| Tối thiểu (n), tích bằng 0 và thực tế là không có bộ ba nào được tính trước khi tồn tại ba phần tử | 
|`4 / 2 2 2 2`|`32`| Giá trị lặp lại và đếm chính xác theo chỉ số | 
|`3 / 1000000 1000000 1000000`|`49`| Giá trị đầu vào tối đa và phép nhân mô-đun | 
| (n=10^6), tất cả các giá trị bằng 0 |`0`| Kích thước đầu vào tối đa và xử lý thời gian tuyến tính | 

## Vỏ cạnh 

### Chính xác là ba phần tử 

cho```
3
1 2 3
```trạng thái ban đầu là`sum1 = 0`,`sum2 = 0`,`answer = 0`. Chỉ xử lý (1) thay đổi`sum1`, cho (1). Việc xử lý (2) tạo ra cặp tổng (2), trong khi câu trả lời vẫn bằng 0 vì vẫn chỉ có hai phần tử. Quá trình xử lý (3) sử dụng tổng cặp cũ, vì vậy`answer = 3 * 2 = 6`. Đầu ra là`6`. 

Điều này phát hiện ra một lỗi nhỏ trong đó việc triển khai có thể cập nhật trạng thái cặp trước khi tính toán mức đóng góp của phần tử hiện tại. 

### Phần tử bằng 0 

cho```
4
0 5 6 7
```xử lý số 0 khiến cả ba trạng thái ở mức 0. Xử lý (5) cho`sum1 = 5`. Xử lý (6) tạo ra`sum2 = 30`. Khi (7) đến, nó đóng góp (7\cdot30=210). Đầu ra là`210`. 

Sự tái diễn không cần trường hợp 0 ​​đặc biệt. Vì số 0 tự nhiên đóng góp số 0 cho cả cặp mới và bộ ba mới, nên bất biến giống nhau có tác dụng không thay đổi. 

### Giá trị lặp lại 

cho```
4
2 2 2 2
```hai giá trị đầu tiên tạo ra`sum2 = 4`. Giá trị thứ ba đóng góp (2\cdot4=8) và trạng thái cặp trở thành (12). Giá trị thứ tư đóng góp (2\cdot12=24). Câu trả lời cuối cùng là (8+24=32). 

Bốn bộ ba chỉ số là ((1,2,3)), ((1,2,4)), ((1,3,4)) và ((2,3,4)). Mỗi người có sản phẩm (8), nên tổng số là (32). Phép truy toán tính chúng theo chỉ số lớn nhất của chúng, mang lại ba bộ ba khi xử lý phần tử thứ tư và một bộ ba khi xử lý phần tử thứ ba. 

### Giá trị tối đa và số học mô-đun 

cho```
3
1000000 1000000 1000000
```có một bộ ba, với sản phẩm 

[ 
10^6\cdot10^6\cdot10^6=10^{18}. 
] 

Kể từ khi 

[ 
10^{18}\bmod 1.000.000.007=49, 
] 

đầu ra đúng là`49`. 

Việc triển khai tính toán kết quả tương tự bằng cách giảm trạng thái sau mỗi lần cập nhật. Bản thân Python có các số nguyên có độ chính xác tùy ý, nhưng việc rút gọn theo mô-đun giữ cho trạng thái đang chạy bị giới hạn và khớp với số học cần thiết. 

### Kích thước mảng tối đa 

Đối với (n=10^6) và mọi giá trị bằng 0, dữ liệu đầu vào chứa một triệu phần tử và câu trả lời vẫn là 0. Thuật toán thực hiện chính xác một lần lặp theo thời gian không đổi cho mỗi phần tử. Nó không bao giờ tạo ra một bảng liệt kê lồng nhau của các bộ ba, do đó thời gian chạy của nó tăng theo tuyến tính thay vì theo khối. 

Cấu trúc vòng lặp tương tự cũng xử lý một triệu giá trị khác 0. Các giá trị của`sum1`,`sum2`, Và`answer`vẫn giảm modulo (10^9+7), do đó kích thước của trạng thái duy trì không phụ thuộc vào (n).
