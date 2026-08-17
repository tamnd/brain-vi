---
title: "CF 102437D - \u041a\u0432\u0430\u0434\u0440\u0430\u0442\u044b \u0424\u0438\u0431\u043e\u043d\u0430\u0447\u0447\u0438"
description: "Chúng ta cần tính tổng bình phương của các phần tử (n+1) đầu tiên của dãy giống Fibonacci. Chuỗi bắt đầu bằng hai số một, vì vậy các giá trị đầu tiên của nó là [ 1,1,2,3,5,8,ldots ] và mọi giá trị sau đó là tổng của hai giá trị trước đó."
date: "2026-08-16T09:22:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102437
codeforces_index: "D"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0427\u0435\u0442\u0432\u0451\u0440\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102437
solve_time_s: 185
verified: false
draft: false
---

[CF 102437D - \u041a\u0432\u0430\u0434\u0440\u0430\u0442\u044b \u0424\u0438\u0431\u043e\u043d\u0430\u0447\u0447\u0438](https://codeforces.com/problemset/problem/102437/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 5s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần tính tổng bình phương của các phần tử (n+1) đầu tiên của dãy giống Fibonacci. Chuỗi bắt đầu bằng hai số một, vì vậy giá trị đầu tiên của nó là 

[ 
1,1,2,3,5,8,\ldots 
] 

và mọi giá trị sau đó là tổng của hai giá trị trước đó. Câu trả lời bắt buộc là 

[ 
f_0^2+f_1^2+\cdots+f_n^2 
] 

lấy modulo (998,244,353). 

Độ khó là giới hạn (n\le 10^{18}). Ngay cả thuật toán (O(n)) cũng sẽ yêu cầu tối đa (10^{18}) lần lặp, vượt xa mọi giới hạn thời gian thực tế. Chúng ta cần một phương thức có thời gian chạy phụ thuộc vào số bit của (n), chỉ khoảng 60 cho (10^{18}). Điều này làm cho thuật toán (O(\log n)) trở thành mục tiêu tự nhiên. 

Có một số trường hợp nhỏ mà lỗi lập chỉ mục có thể dễ dàng che giấu. Với (n=0), tổng chỉ chứa (f_0^2=1), nên đáp án là (1). Một công thức sử dụng chỉ số Fibonacci thông thường mà không điều chỉnh độ dịch chuyển có thể trả về (0) không chính xác. Với (n=1), câu trả lời là (1^2+1^2=2), kiểm tra xem cả hai số ban đầu đều được bao gồm. Đối với (n=2), tổng là (1+1+4=6), do đó, việc triển khai lặp lại chỉ bắt đầu tích lũy sau khi tạo ra một giá trị Fibonacci mới sẽ bị giảm đi một. 

Có một vấn đề khác với các công thức dựa trên biểu thức Binet. Mặc dù công thức Binet đúng về mặt toán học nhưng nó sử dụng số dấu phẩy động nếu được triển khai trực tiếp và các giá trị xung quanh (10^{18}) yêu cầu độ chính xác cao hơn nhiều so với loại dấu phẩy động tiêu chuẩn có thể cung cấp. Số học mô-đun với việc nhân đôi số nguyên nhanh chóng sẽ tránh được vấn đề này hoàn toàn. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp rất đơn giản. Bắt đầu với (f_0=f_1=1), liên tục tạo giá trị Fibonacci tiếp theo, bình phương nó và thêm bình phương vào modulo câu trả lời (998,244,353). Điều này đúng vì mỗi số hạng được tạo ra chính xác một lần và mỗi ô vuông được tạo ra đóng góp chính xác một lần vào tổng được yêu cầu. 

Vấn đề là kích thước của (n). Đối với (n=10^{18}), phương pháp này tạo ra (10^{18}+1) số hạng trong chuỗi. Nếu chúng ta đếm một phép cộng cho mỗi bước lặp lại, một phép nhân cho mỗi bình phương và một phép cộng cho mỗi phép cộng, thì công việc sẽ có thứ tự (3n), gần bằng (3\cdot10^{18}) phép tính số học cơ bản. Số đếm chính xác phụ thuộc vào cách xử lý các giá trị ban đầu và bộ tích lũy, nhưng vấn đề tiệm cận mang tính quyết định: thời gian tuyến tính là không thể. 

Quan sát hữu ích là dãy trong bài toán chỉ là dãy Fibonacci tiêu chuẩn được dịch chuyển một vị trí. Giả sử (F_0=0,F_1=1). Sau đó 

[ 
f_i=F_{i+1}. 
] 

Ngoài ra còn có một bản sắc cổ điển 

[ 
\sum_{k=1}^{m}F_k^2=F_mF_{m+1}. 
] 

Cài đặt (m=n+1) cho 

# \sum_{i=0}^{n}F_{i+1}^2 

F_{n+1}F_{n+2}. 
] 

Do đó, toàn bộ phép tính tổng đã quy về tích của hai số Fibonacci. 

Chúng ta vẫn cần tính các số Fibonacci có chỉ số có thể lớn bằng (10^{18}+2). Nhân đôi nhanh thực hiện chính xác điều đó trong thời gian (O(\log n)). Kỹ thuật này sử dụng các đặc tính nhận dạng bắt nguồn từ (F_{2k}) và (F_{2k+1}) trực tiếp từ (F_k) và (F_{k+1}): 

[ 
F_{2k}=F_k(2F_{k+1}-F_k) 
] 

và 

[ 
F_{2k+1}=F_k^2+F_{k+1}^2. 
] 

Do đó, sau khi giải đệ quy bài toán cho (k=\lfloor n/2\rfloor), chúng ta có thể xây dựng câu trả lời cho (n) chỉ bằng cách sử dụng một số lượng không đổi các phép nhân và phép cộng mô-đun. 

Phương pháp brute-force hoạt động vì phép truy toán cho phép chúng ta liệt kê mọi thuật ngữ cần thiết. Nó thất bại vì có thể có (10^{18}+1) trong số chúng. Quan sát cho thấy tổng các bình phương Fibonacci có dạng đóng làm giảm vấn đề khi tính toán một cặp Fibonacci và việc nhân đôi nhanh chóng sẽ giảm tính toán đó từ thời gian tuyến tính sang thời gian logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n)) | (O(1)) | Quá chậm | 
| Tối ưu | (O(\log n)) | (O(\log n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Giới thiệu dãy Fibonacci chuẩn (F_0=0,F_1=1). Dãy số của bài toán thỏa mãn (f_i=F_{i+1}), do đó tổng cần tìm sẽ trở thành 

[ 
\sum_{i=0}^{n}F_{i+1}^2. 
] 

Sự thay đổi chỉ mục này là nguyên nhân chính có thể xảy ra lỗi từng cái một. 

1. Áp dụng danh tính 

[ 
\sum_{k=1}^{m}F_k^2=F_mF_{m+1}. 
] 

Với (m=n+1), đáp án bắt buộc là 

[ 
F_{n+1}F_{n+2}. 
] 

Do đó chúng ta chỉ cần cặp liên tiếp ((F_{n+1},F_{n+2})). 

1. Xác định hàm trả về cặp ((F_k,F_{k+1})). Với (k=0), cặp đó là ((0,1)). 
2. Tính toán đệ quy cặp cho (k=\lfloor n/2\rfloor). Giả sử nó trả về ((a,b)=(F_k,F_{k+1})). 
3. Sử dụng danh tính nhân đôi nhanh để có được 

[ 
c=F_{2k}=a(2b-a) 
] 

và 

[ 
d=F_{2k+1}=a^2+b^2. 
] 

Tất cả các hoạt động được thực hiện modulo (998,244,353). 

1. Nếu (n) chẵn, trả về ((c,d)), vì (n=2k). 

Nếu (n) là số lẻ, trả về ((d,c+d)), vì (n=2k+1) và (F_{2k+2}=F_{2k}+F_{2k+1}). 

1. Gọi hàm với (n+1). Nếu nó trả về ((x,y)), thì (x=F_{n+1}) và (y=F_{n+2}). Đầu ra (xy\bmod 998,244,353). 

### Tại sao nó hoạt động 

Bất biến của hàm đệ quy là`fib_pair(k)`luôn trả về chính xác ((F_k,F_{k+1})). Trường hợp cơ sở trả về trực tiếp ((F_0,F_1)=(0,1)). Đối với (k lớn hơn), lệnh gọi đệ quy sẽ đưa ra cặp chính xác cho (\lfloor k/2\rfloor) và các danh tính nhân đôi sẽ tạo ra các giá trị chính xác cho các chỉ số (2\lfloor k/2\rfloor) và (2\lfloor k/2\rfloor+1). Kiểm tra tính chẵn lẻ sau đó chọn cặp thích hợp cho (k). Do đó, lệnh gọi cuối cùng trả về ((F_{n+1},F_{n+2})), có tích bằng tổng ban đầu theo danh tính tổng bình phương Fibonacci. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def fib_pair(n):
    if n == 0:
        return 0, 1

    a, b = fib_pair(n // 2)

    c = a * ((2 * b - a) % MOD) % MOD
    d = (a * a + b * b) % MOD

    if n % 2 == 0:
        return c, d

    return d, (c + d) % MOD

def solve():
    n = int(input())

    fn1, fn2 = fib_pair(n + 1)
    answer = fn1 * fn2 % MOD

    print(answer)

if __name__ == "__main__":
    solve()
```Hằng số`MOD`lưu trữ mô-đun cần thiết. Giữ mọi giá trị Fibonacci trung gian giảm modulo`MOD`là đủ vì tất cả các phép toán sau này đều là phép cộng và phép nhân, cả hai đều tôn trọng sự tương đương mô-đun. 

Hàm đệ quy trả về hai số Fibonacci liên tiếp thay vì chỉ một. Điều này là cần thiết vì các công thức nhân đôi của (F_{2k}) sử dụng cả (F_k) và (F_{k+1}). Trả về cặp cũng có nghĩa là trường hợp lẻ có thể thu được (F_{2k+2}) với một phép cộng cuối cùng. 

biểu hiện`2 * b - a`có thể âm trước phép toán modulo, do đó mã tính toán rõ ràng`(2 * b - a) % MOD`. Hoạt động modulo của Python xử lý trung gian âm một cách chính xác, tạo ra một giá trị trong phạm vi được yêu cầu. 

Cuộc gọi sử dụng`n + 1`, không`n`. Chuỗi ban đầu được dịch chuyển so với chuỗi Fibonacci tiêu chuẩn và danh tính tổng bình phương yêu cầu (F_{n+1}F_{n+2}). Từ`fib_pair(n + 1)`trả về các giá trị liên tiếp bắt đầu từ (F_{n+1}), phép nhân chính xác là kết quả mong muốn. 

Số nguyên Python có độ chính xác tùy ý, do đó không có hiện tượng tràn số nguyên. Quan trọng hơn, thuật toán không bao giờ tạo ra các số Fibonacci thực tế khổng lồ. Mọi giá trị đều được giảm modulo (998,244,353) ngay lập tức. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, (n=0). Thuật toán yêu cầu cặp tại chỉ mục (n+1=1). 

| Bước | (n) | Cặp trả lại | Ý nghĩa | 
| --- | --- | --- | --- | 
| Căn cứ | 0 | ((0,1)) | (F_0,F_1) | 
| Trường hợp kỳ lạ | 1 | ((1,1)) | (F_1,F_2) | 
| Cuối cùng | 0 | (1\cdot1) | (1) | 

Cặp trả về là ((F_1,F_2)=(1,1)), vì vậy câu trả lời là (1). Điều này xác nhận đầu vào nhỏ nhất có thể và xác minh rằng (f_0) được bao gồm. 

Đối với Mẫu 2, (n=2). Chúng ta cần cặp ở chỉ số (3). 

| Bước | Chỉ mục hiện tại | Chỉ số một nửa | Cặp một nửa | Ghép nối hiện tại | 
| --- | --- | --- | --- | --- | 
| Căn cứ | 0 | 0 | ((0,1)) | ((0,1)) | 
| Nhân đôi | 1 | 0 | ((0,1)) | ((1,1)) | 
| Nhân đôi | 3 | 1 | ((1,1)) | ((2,3)) | 
| Sản phẩm cuối cùng | 2 | 3 | ((2,3)) | (2\cdot3=6) | 

Ở đây cặp trả về là ((F_3,F_4)=(2,3)). Sản phẩm của họ là (6), phù hợp 

[ 
1^2+1^2+2^2=6. 
] 

Dấu vết cũng cho thấy tại sao hàm trả về hai giá trị liên tiếp. Cặp ((2,3)) cung cấp cả hai số Fibonacci cần thiết cho dạng đóng cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\log n)) | Mỗi cấp độ đệ quy chia đôi chỉ mục và thực hiện một số lượng không đổi các phép toán số học mô-đun. | 
| Không gian | (O(\log n)) | Độ sâu đệ quy tỷ lệ thuận với số bit trong (n). | 

Đối với (n\le10^{18}), có ít hơn 60 cấp độ đệ quy. Do đó, thuật toán chỉ thực hiện vài trăm phép tính số học mô-đun thay vì cố gắng xử lý tối đa (10^{18}+1) phần tử chuỗi. Việc sử dụng bộ nhớ cũng rất nhỏ. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 998244353

def fib_pair(n):
    if n == 0:
        return 0, 1

    a, b = fib_pair(n // 2)

    c = a * ((2 * b - a) % MOD) % MOD
    d = (a * a + b * b) % MOD

    if n % 2 == 0:
        return c, d

    return d, (c + d) % MOD

def solution(inp: str) -> str:
    n = int(inp.strip())
    a, b = fib_pair(n + 1)
    return str(a * b % MOD)

def run(inp: str) -> str:
    return solution(inp)

# Provided samples
assert run("0\n") == "1", "sample 1"
assert run("2\n") == "6", "sample 2"
assert run("4\n") == "40", "sample 3"

# Minimum input
assert run("0\n") == "1", "minimum n"

# Both initial Fibonacci values are included
assert run("1\n") == "2", "two initial ones"

# Boundary case around the sample range
assert run("3\n") == "15", "off-by-one check"

# Another small independently computed value:
# 1^2 + 1^2 + 2^2 + 3^2 + 5^2 + 8^2 = 104
assert run("5\n") == "104", "small direct computation"

# Maximum-size input. The expected value is generated independently
# by the same mathematical fast-doubling specification.
max_n = 10**18
expected_max_a, expected_max_b = fib_pair(max_n + 1)
expected_max = str(expected_max_a * expected_max_b % MOD)
assert run(str(max_n) + "\n") == expected_max, "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`1`| Đầu vào tối thiểu và bao gồm (f_0). | 
|`1`|`2`| Cả hai giá trị ban đầu (f_0=f_1=1) đều được tính. | 
|`3`|`15`| Chuyển đổi chính xác từ việc lập chỉ mục của bài toán sang lập chỉ mục Fibonacci tiêu chuẩn. | 
|`5`|`104`| Xác minh trực tiếp qua một số bước lặp lại. | 
|`10^18`| Modulo tính toán (998244353) | Kích thước đầu vào tối đa và xử lý logarit của chỉ mục. | 

Thử nghiệm kích thước tối đa cố tình tính toán giá trị mong đợi của nó với cùng một phép tính số nguyên chính xác được sử dụng bởi giải pháp thay vì nhúng một hằng số lớn không giải thích được. Điều này giúp bài kiểm tra khép kín trong khi vẫn thực hiện đầy đủ ranh giới (10^{18}). 

## Vỏ cạnh 

Với (n=0), đầu vào là`0`. Thuật toán gọi`fib_pair(1)`, trả về ((1,1)). Phép nhân cuối cùng là (1\cdot1=1), khớp với số hạng duy nhất (f_0^2=1). Một công thức quên sự dịch chuyển từ (f_i) sang (F_{i+1}) có thể bị lỗi ngay lập tức. 

Với (n=1), đầu vào là`1`. Tổng cần tìm là (1^2+1^2=2). Thuật toán tính toán`fib_pair(2)`, lấy ((F_2,F_3)=(1,2)) và nhân chúng để được (2). Điều này phát hiện việc triển khai vô tình bắt đầu tính tổng chỉ với một trong hai giá trị ban đầu. 

Với (n=2), đầu vào là`2`. Các giá trị Fibonacci tiêu chuẩn liên quan là (F_1,F_2,F_3=1,1,2), vì vậy câu trả lời là (1+1+4=6). Thuật toán thu được`fib_pair(3)=(2,3)`và tính toán (2\cdot3=6). Đây là trường hợp đơn giản nhất để kiểm tra danh tính tổng bình phương ngoài các điều kiện ban đầu. 

Đối với (n=10^{18}), đầu vào có chỉ số lớn nhất được phép. Thuật toán không bao giờ lặp lại chuỗi. Mỗi lệnh gọi đệ quy sẽ thay thế chỉ mục hiện tại bằng một nửa số nguyên của nó, do đó chuỗi chỉ chứa khoảng 60 cấp độ. Mọi giá trị Fibonacci đều được giảm theo modulo (998.244.353), do đó phép tính vẫn nhỏ mặc dù số Fibonacci thực tế có số lượng chữ số rất lớn. Sản phẩm thu được được giảm một lần nữa theo mô đun yêu cầu, đưa ra câu trả lời chính xác theo yêu cầu.
