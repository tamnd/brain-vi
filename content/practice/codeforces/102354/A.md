---
title: "CF 102354A - Phân vùng căn bậc hai"
description: "Chúng ta có một mảng các số nguyên dương (a1,dots,an). Căn bậc hai đầu tiên luôn được lấy bằng dấu cộng, trong khi mọi số hạng sau đó có thể nhận độc lập (+) hoặc (-). Chúng ta cần đếm có bao nhiêu lựa chọn [ sqrt{a1}pmsqrt{a2}pmcdotspmsqrt{an}=0."
date: "2026-08-13T00:25:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102354
codeforces_index: "A"
codeforces_contest_name: "2018-2019 Summer Petrozavodsk Camp, Oleksandr Kulkov Contest 2"
rating: 0
weight: 102354
solve_time_s: 148
verified: true
draft: false
---

[CF 102354A - Phân vùng căn bậc hai](https://codeforces.com/problemset/problem/102354/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng các số nguyên dương (a_1,\dots,a_n). Căn bậc hai đầu tiên luôn được lấy bằng dấu cộng, trong khi mọi số hạng sau đó có thể nhận độc lập (+) hoặc (-). Chúng ta cần đếm xem có bao nhiêu lựa chọn 

[ 
\sqrt{a_1}\pm\sqrt{a_2}\pm\cdots\pm\sqrt{a_n}=0. 
] 

Câu trả lời tính các lựa chọn dấu hiệu, không tính tổng kết quả khác biệt. Vì dấu đầu tiên cố định nên có (2^{n-1}) lựa chọn có thể có trước khi áp dụng phương trình. 

Ràng buộc (n\le 36) là tín hiệu thuật toán chính. Việc liệt kê tất cả (2^{35}) mẫu dấu hiệu được phép là khoảng (3,44\time10^{10}), vượt xa giới hạn ba giây. Việc giảm mức độ gặp nhau ở giữa thông thường từ (2^n) xuống gần như (2^{n/2}) chính xác là kích thước của (n) gợi ý. 

Giới hạn lớn hơn nhiều trên (a_i) là phần bất thường. Một (a_i) có thể có hơn 100.000 chữ số thập phân, do đó, việc phân tích các con số hoặc thậm chí chuyển đổi chúng trực tiếp thành số nguyên Python thông thường không phải là một chiến lược triển khai hợp lý. Trên thực tế, các phiên bản Python có giới hạn chuyển đổi chuỗi số nguyên tiêu chuẩn sẽ từ chối đầu vào đó trừ khi giới hạn đó được thay đổi. Chúng ta có thể tránh hoàn toàn vấn đề này bằng cách đọc từng số dưới dạng chuỗi thập phân và giảm nó theo từng chữ số nguyên tố cố định. 

Có một số trường hợp nguy hiểm bộc lộ các giải pháp bất cẩn. Với```
2
1 1
```câu trả lời là (1), vì (1-1=0). Một giải pháp coi cả hai dấu hiệu là độc lập, tính toàn bộ (2^2) phép gán và quên tính đối xứng dấu hiệu chung sẽ nhận được (2). 

Với```
3
2 2 8
```câu trả lời là (1), bởi vì 

[ 
\sqrt2+\sqrt2-\sqrt8=0. 
] 

Việc triển khai dấu phẩy động có thể âm thầm biến điều này thành một so sánh không đáng tin cậy, đặc biệt đối với các giá trị khổng lồ được đầu vào cho phép. 

Trường hợp nguy hiểm thứ hai là```
4
4 9 25 49
```câu trả lời của họ là (0). Mỗi căn bậc hai là một số nguyên, vì vậy đầu vào cụ thể này trông giống như một tập hợp con có dấu thông thường, nhưng không có sự lựa chọn các dấu hiệu cho (2,3,5,7) tổng bằng 0. Một giải pháp chỉ kiểm tra xem tổng số có phải là số chẵn hay vô tình cho phép các hệ số tùy ý thay vì chính xác (\pm1), có thể chấp nhận nó không chính xác. 

Cuối cùng, các giá trị có thể lớn đến mức bản thân dữ liệu đầu vào phải được coi là văn bản. Ví dụ: (10^{100000}) là hợp pháp. Cố gắng phân tích nó hoặc thực hiện căn bậc hai dấu phẩy động sẽ làm mất cấu trúc chính xác mà bài toán yêu cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi phép gán dấu có thể. Đối với mỗi bài tập, chúng tôi đánh giá biểu thức và kiểm tra xem nó có bằng 0 hay không. Điều này đúng vì mỗi phép gán pháp lý đều xuất hiện đúng một lần. Nếu chúng ta liệt kê dấu hiệu đầu tiên cố định cùng với tất cả các dấu hiệu khác thì sẽ có (2^n) phép gán hoặc (2^{36}=68,719,476,736) trong trường hợp xấu nhất. Ngay cả với số học theo thời gian không đổi, tốc độ đó vẫn quá chậm. 

Đối với các số nguyên có kích thước thông thường, bước tự nhiên tiếp theo sẽ là viết 

[ 
\sqrt{x}=c\sqrt{d}, 
] 

trong đó (d) không bình phương, nhóm các số hạng có cùng (d) và giải tổng các tập hợp con có dấu thu được một cách độc lập. Điều đó rõ ràng về mặt toán học, nhưng để có được phần bình phương của một số nguyên 100.000 chữ số về cơ bản đòi hỏi cùng một thông tin phân tích nhân tử khó mà các ràng buộc được thiết kế để làm cho không có sẵn. 

Quan sát quan trọng là chúng ta thực sự không cần phân rã bình phương. Chúng ta chỉ cần biểu diễn căn bậc hai trong đó đẳng thức cộng được bảo toàn đủ tốt để phân biệt các tổng có dấu liên quan. Giải pháp dự định sử dụng một số dư nguyên tố và bậc hai lớn để xây dựng một biểu diễn như vậy, sau đó là phần gặp nhau ở giữa. Cách tiếp cận mô-đun này cũng được mô tả trong bản ghi giải pháp có sẵn cho vấn đề này. 

Chọn số nguyên tố lớn (P) thỏa mãn (P\equiv3\pmod4). Chúng tôi sử dụng 

[ 
f(x)=x^{(P+1)/4}\pmod P. 
] 

Đối với một giá trị khác 0 (x), tiêu chí Euler đưa ra hai trường hợp. Nếu (x) là thặng dư bậc hai modulo (P), thì 

[ 
f(x)^2=x. 
] 

Nếu (x) là số dư thì 

[ 
f(x)^2=-x. 
] 

Trường hợp thứ hai có vẻ có vấn đề vì nó không cho ra căn bậc hai của (x) theo đúng nghĩa đen. Thuộc tính hữu ích là bản đồ có tính nhân: 

[ 
f(xy)=f(x)f(y). 
] 

Vì vậy, thay vì xác định rõ ràng phần bình phương của mỗi (a_i), bản đồ này gán cho mỗi căn thức một đại diện đại số nhất quán. Trong một lớp căn thức, hệ số bổ sung có thể có là cố định và việc chọn (+) hoặc (-) đã cho phép dấu hiệu cố định đó được hấp thụ vào lựa chọn dấu hiệu. Do đó, số lượng kết hợp có dấu bằng 0 được giữ nguyên. 

Vấn đề duy nhất còn lại là một trường hữu hạn có thể xác định hai đại lượng đại số khác nhau khác nhau về các số hữu tỷ. Với số nguyên tố khoảng 60 bit, điều này có xác suất theo thứ tự (1/P) cho một xung đột không liên quan, điều này không đáng kể đối với cách giải thích băm ngẫu nhiên dự kiến. Giải pháp ban đầu sử dụng số nguyên tố cố định lớn cho chính mục đích này. 

Khi mọi (\sqrt{a_i}) đã được thay thế bởi đại diện mô-đun (b_i) của nó, vấn đề sẽ trở thành vấn đề tổng có chữ ký tiêu chuẩn: 

[ 
\pm b_1\pm b_2\pm\cdots\pm b_n\equiv0\pmod P. 
] 

Chia các điều khoản thành hai nửa. Tạo mọi tổng có chữ ký của nửa đầu và đếm tần số của nó. Sau đó tính tổng có dấu của nửa sau và tra số âm của nó trong bảng nửa đầu. Mỗi cặp đưa ra một bài tập có tổng bằng 0 hoàn chỉnh. 

Chúng tôi liệt kê tất cả (2^n) phép gán dấu thay vì sửa dấu đầu tiên. Mọi phép gán pháp lý với dấu đầu tiên cố định tương ứng với chính xác hai phép gán dấu đầy đủ, một phép gán là phủ định toàn cục của phép gán kia. Do đó số lượng mô-đun cuối cùng được chia cho hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^n n)) | (O(1)) | Quá chậm | 
| Tối ưu | (O(n2^{n/2})) | (O(2^{n/2})) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc mọi (a_i) dưới dạng chuỗi thập phân và rút gọn nó theo modulo (P). Chúng tôi không bao giờ xây dựng số nguyên Python có khả năng 100.001 chữ số, bởi vì chỉ cần modulo dư (P) của nó cho phép tính mô-đun. 
2. Với mỗi giá trị rút gọn (x_i), hãy tính 

[ 
b_i=x_i^{(P+1)/4}\bmod P. 
] 

Số mũ được chọn vì (P\equiv3\pmod4), mang lại hành vi thặng dư bậc hai được mô tả ở trên. 

1. Chia các giá trị được chuyển đổi (n) thành hai nửa kích thước (\lfloor n/2\rfloor) và (\lceil n/2\rceil). Hai nửa có nhiều nhất 18 phần tử, mỗi bên có nhiều nhất (2^{18}=262.144) tổng có dấu. 
2. Tạo mọi số tiền đã ký của nửa đầu. Lưu trữ số âm của mỗi tổng trong từ điển tần số. Việc lưu trữ phủ định có nghĩa là tổng của nửa sau có thể được truy vấn trực tiếp. 
3. Tạo mọi số tiền đã ký của nửa sau. Đối với (các) tổng, hãy tra cứu xem có bao nhiêu tổng nửa đầu bằng (-s) modulo (P). Thêm tần số đó vào câu trả lời vì mỗi cặp như vậy tạo thành một bài tập có dấu hoàn chỉnh có tổng chuyển đổi bằng 0. 
4. Chia số lượng tích lũy cho hai. Việc liệt kê cho phép cả hai dấu hiệu ở số hạng đầu tiên, trong khi biểu thức thực tế sửa số hạng đầu tiên là dương. Việc phủ định mọi dấu sẽ thay đổi một biểu thức 0 thành một biểu thức 0 khác và ghép nối chính xác các phép gán đầy đủ. Mỗi nhiệm vụ pháp lý có ký hiệu đầu tiên cố định có đúng một đối tác có ký hiệu âm đầu tiên như vậy. 
5. In số nguyên thu được. 

Bất biến quan trọng là từ điển chứa chính xác các phần bổ sung mô-đun cần thiết để hoàn thành mỗi phép gán nửa đầu với tổng có dấu bằng 0. Mỗi cặp được tính bằng cách so khớp hai nửa tương ứng với một phép gán đầy đủ có chữ ký với tổng bằng 0 được chuyển đổi và mọi phép gán bằng 0 được chuyển đổi sẽ chia duy nhất thành một tổng nửa đầu và một tổng nửa sau. Việc xây dựng trường hữu hạn chỉ được sử dụng để thay thế các căn thức lớn bằng các biểu diễn thu gọn mà không yêu cầu phân tích nhân tử. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# A large prime with P % 4 == 3.
# 411191981019260843 is the prime used by the standard solution.
P = 411191981019260843
EXP = (P + 1) // 4

def read_mod(s):
    x = 0
    for c in s:
        if '0' <= c <= '9':
            x = (x * 10 + ord(c) - 48) % P
    return x

def signed_sums(values):
    sums = [0]

    for x in values:
        old = sums
        sums = [v - x for v in old] + [v + x for v in old]
        sums = [v % P for v in sums]

    return sums

def solve():
    n = int(input())
    raw = input().split()

    values = [pow(read_mod(s), EXP, P) for s in raw]

    m = n // 2
    left = values[:m]
    right = values[m:]

    left_sums = signed_sums(left)

    freq = {}
    for x in left_sums:
        freq[x] = freq.get(x, 0) + 1

    answer = 0

    right_sums = signed_sums(right)
    for x in right_sums:
        answer += freq.get((-x) % P, 0)

    print(answer // 2)

if __name__ == "__main__":
    solve()
```các`read_mod`chức năng là một trong những chi tiết triển khai quan trọng nhất. Nó tính toán 

[ 
(((d_1\cdot10+d_2)\cdot10+d_3)\cdots)\bmod P 
] 

khi đọc các chữ số thập phân. Điều này làm cho kích thước đầu vào tuyến tính theo số chữ số thập phân và tránh giới hạn chuyển đổi của Python đối với các số nguyên lớn. 

Cuộc gọi tới`pow(x, EXP, P)`sử dụng phép lũy thừa mô-đun tích hợp sẵn của Python, thực hiện phép lũy thừa bằng cách bình phương và không bao giờ tạo ra một số nguyên trung gian lớn. Số mũ chỉ có khoảng 59 bit cho mô đun này, vì vậy mỗi phép biến đổi không tốn kém. 

các`signed_sums`hàm bắt đầu bằng tổng có dấu trống (0). Khi một giá trị (x) được thêm vào, mỗi tổng hiện có sẽ tạo ra hai tổng mới, (s-x) và (s+x). Sau khi xử lý các giá trị (k), có chính xác (2^k) mục nhập. Hoạt động modulo giữ mọi giá trị trong một phạm vi cố định. 

Từ điển lưu trữ tần số thay vì chỉ tập hợp các khoản tiền. Các mẫu ký hiệu khác nhau có thể tạo ra tổng mô-đun giống nhau và mỗi mẫu trong số chúng phải đóng góp vào câu trả lời. Việc quên số tần số sẽ đếm thiếu các tổng lặp lại, đặc biệt đối với các mảng chứa các giá trị bằng nhau. 

Không có tràn số nguyên trong Python. Tất cả số học đều có độ chính xác tùy ý, trong khi bản thân các giá trị mô-đun chỉ có khoảng 59 bit. 

Phép chia cuối cùng cho hai không phải là một sự điều chỉnh tùy ý. Mã cố tình liệt kê cả hai dấu hiệu cho thuật ngữ đầu tiên. Nếu một vectơ dấu đầy đủ là ((s_1,\dots,s_n)), thì phủ định của nó ((-s_1,\dots,-s_n)) cũng có tổng bằng 0. Chính xác một trong các cặp có (s_1=+1), đây là quy ước được yêu cầu bởi biểu thức ban đầu. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
3
2 2 8
```mối quan hệ thực sự chính xác là 

[ 
\sqrt2+\sqrt2-\sqrt8=0. 
] 

Đặt (f(x)=x^{(P+1)/4}\bmod P). Mối quan hệ cấu trúc quan trọng là (8=2^3), do đó giá trị biến đổi của (8) có quan hệ nhân với giá trị biến đổi của (2). Biểu diễn mô-đun có thể chọn một hướng cố định khác cho một lớp căn bản, nhưng các dấu hiệu sẵn có sẽ tiếp thu hướng đó. 

Với (n=3), phần chia là (1+2). 

| Sân khấu | Trạng thái trái | Đúng trạng thái | Đóng góp | 
| --- | --- | --- | --- | 
| Chuyển đổi | (f(2)) | (f(2), f(8)) | 0 | 
| Tạo trái | (+f(2),-f(2)) | chưa được tạo | 2 mục | 
| Tạo đúng | không thay đổi | bốn khoản tiền đã ký | 4 mục | 
| Kết hợp bổ sung | tổng trái phủ định được lưu trữ | truy vấn mỗi tổng bên phải | 2 bài tập không có mô-đun đầy đủ | 
| Xóa dấu hiệu toàn cầu | 2 | 2 | 1 | 

Phép chia cuối cùng đưa ra (1), khớp lựa chọn hợp pháp duy nhất với dấu hiệu cố định đầu tiên. Ví dụ này giải thích tại sao chúng ta đếm các vectơ dấu đầy đủ và chỉ chuẩn hóa ở cuối. 

Đối với mẫu 2,```
4
4 9 25 49
```căn bậc hai là (2,3,5,7). Không có dấu hiệu nào có thể làm cho tổng của chúng bằng không. Sự phân chia là (2+2). 

| Sân khấu | Số tiền ký bên trái | Số tiền đã ký bên phải | Kết quả phù hợp | 
| --- | --- | --- | --- | 
| Chuyển đổi | đại diện của (2,3) | đại diện của (5,7) | 0 | 
| Bảng liệt kê bên trái | (2+2=4), (2-3=-1), (-2+3=1), (-2-3=-5) | chưa | 4 khoản | 
| Liệt kê đúng | bổ sung được lưu trữ | (5+7=12), (5-7=-2), (-5+7=2), (-5-7=-12) | 0 | 
| Phân chia cuối cùng | 0 | 0 | 0 | 

Dấu vết này minh họa bất biến gặp nhau ở giữa cơ bản. Mỗi bài tập hoàn chỉnh sẽ phải chia thành tổng bên trái và tổng bên phải hoàn toàn đối lập với mô-đun. Không có cặp như vậy tồn tại ở đây. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(D+n\log P+n2^{n/2})) | (D) là tổng số chữ số đầu vào; chi phí lũy thừa mô-đun (O(\log P)) cho mỗi giá trị và hai nửa chứa tối đa (2^{18}) tổng | 
| Không gian | (O(2^{n/2})) | Từ điển tần số lưu trữ tối đa (2^{18}) tổng nửa bên trái riêng biệt, cộng với mảng tổng được tạo | 

Đối với (n=36), bên gặp nhau có nhiều nhất (262.144) nhiệm vụ. Đó là nhiều mệnh lệnh có cường độ nhỏ hơn (68,7) tỷ bài tập của vũ lực. Giới hạn 100.000 chữ số chỉ ảnh hưởng đến phân tích cú pháp thập phân ban đầu, có tính chất tuyến tính trong kích thước đầu vào. Thuật toán không bao giờ phân tích hoặc xây dựng những số nguyên khổng lồ đó. 

Cấu trúc mô-đun là sự giảm trường hữu hạn kiểu băm, do đó phương pháp lý thuyết có xác suất va chạm không đáng kể thay vì đảm bảo xác định tuyệt đối. Số nguyên tố cố định đủ lớn cho bối cảnh cuộc thi dự định và kỹ thuật này là phương pháp tiêu chuẩn được sử dụng cho vấn đề này. 

## Trường hợp thử nghiệm```python
# This test harness uses the same solve() function as the submitted solution.
import sys
import io

P = 411191981019260843
EXP = (P + 1) // 4

def read_mod(s):
    x = 0
    for c in s:
        if '0' <= c <= '9':
            x = (x * 10 + ord(c) - 48) % P
    return x

def signed_sums(values):
    sums = [0]
    for x in values:
        sums = [v - x for v in sums] + [v + x for v in sums]
        sums = [v % P for v in sums]
    return sums

def solve():
    input = sys.stdin.readline

    n = int(input())
    raw = input().split()

    values = [pow(read_mod(s), EXP, P) for s in raw]

    m = n // 2
    left = values[:m]
    right = values[m:]

    freq = {}
    for x in signed_sums(left):
        freq[x] = freq.get(x, 0) + 1

    answer = 0
    for x in signed_sums(right):
        answer += freq.get((-x) % P, 0)

    print(answer // 2)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_input = globals().get("input")

    sys.stdin = io.StringIO(inp)
    globals()["input"] = sys.stdin.readline

    try:
        old_stdout = sys.stdout
        sys.stdout = io.StringIO()

        solve()

        result = sys.stdout.getvalue().strip()
        sys.stdout = old_stdout
        return result
    finally:
        sys.stdin = old_stdin
        if old_input is not None:
            globals()["input"] = old_input

# Provided samples
assert run("3\n2 2 8\n") == "1", "sample 1"
assert run("4\n4 9 25 49\n") == "0", "sample 2"

# Minimum n, equal values
assert run("2\n1 1\n") == "1", "minimum size with a solution"

# Minimum n, unequal square roots
assert run("2\n1 4\n") == "0", "minimum size without a solution"

# All four values equal:
# among four signs, exactly two must be positive and two negative.
# Six full assignments / 2 = three assignments with the first sign fixed.
assert run("4\n1 1 1 1\n") == "3", "all equal values"

# Boundary-sized decimal integers.
# sqrt(A) + sqrt(A) - sqrt(4A) = 0.
big = "1" + "0" * 100000
four_big = "4" + "0" * 100000
assert run(f"3\n{big} {big} {four_big}\n") == "1", "100001-digit values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 1`|`1`| Tối thiểu (n) và chia cho hai để đối xứng dấu toàn cục | 
|`2 / 1 4`|`0`| Tối thiểu (n) không có phân vùng hợp lệ | 
|`4 / 1 1 1 1`|`3`| Lặp lại các giá trị bằng nhau và đếm tần số trong bảng gặp nhau ở giữa | 
|`3 / A A 4A`, trong đó (A=10^{100000}) |`1`| Độ dài thập phân tối đa và phân tích chuỗi mô-đun chính xác | 

Trường hợp hoàn toàn bằng nhau đặc biệt hữu ích để nắm bắt việc triển khai từ điển chỉ lưu trữ xem tổng có tồn tại hay không. Nhiều mẫu dấu tạo ra cùng một tổng, do đó tần số của chúng phải được tích lũy. 

Bài kiểm tra số lượng lớn phát hiện ra một loại lỗi hoàn toàn khác. Các giá trị chứa 100.001 chữ số thập phân, nhưng nghiệm chỉ duy trì môđun dư (P). Không cần chuyển đổi số nguyên lớn hoặc phân tích hệ số của Python. 

## Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên,```
2
1 1
```các giá trị được chuyển đổi là giống hệt nhau. Bốn phép gán dấu đầy đủ chứa hai tổng bằng 0, đó là (+,+) và (-,-). Hai cái còn lại cho tổng khác không. Thuật toán ghi lại hai cặp khớp nhau và chia cho hai, tạo ra (1), tương ứng với yêu cầu (+\sqrt1-\sqrt1). 

Đối với trường hợp cạnh thứ hai,```
2
1 4
```biểu thức thực là (1\pm2), không bao giờ bằng 0. Thủ tục gặp nhau ở giữa chỉ có một giá trị trong mỗi nửa. Cả hai giá trị có dấu đều không thể là giá trị phủ định của giá trị kia, do đó số lượng mô-đun bằng 0 và đầu ra vẫn giữ nguyên (0). 

Đối với các giá trị lặp lại,```
4
1 1 1 1
```số 0 cần có hai dấu dương và hai dấu âm. có 

[ 
\binom42=6 
] 

vectơ dấu đầy đủ và chính xác một nửa có dấu dương đầu tiên. Câu trả lời là (6/2=3). Từ điển tần số là cần thiết vì nhiều phép gán khác nhau tạo ra tổng trung gian giống nhau. 

Đối với ranh giới đầu vào lớn,```
3
10^100000 10^100000 4*10^100000
```trong đó ký hiệu đại diện cho chuỗi thập phân tương ứng, căn bậc hai là 

[ 
10^{50000},\quad10^{50000},\quad2\cdot10^{50000}. 
] 

Do đó các dấu (+,+,-) cho kết quả bằng 0 nên đáp án là (1). Việc triển khai không bao giờ đánh giá các số có 100.001 chữ số là số nguyên Python. Nó đọc các biểu diễn thập phân của chúng và rút gọn chúng theo modulo (P), sau đó áp dụng cùng một quy trình mô-đun và gặp nhau ở giữa. 

Trường hợp cạnh tinh vi nhất là căn không phải bình phương, chẳng hạn như (\sqrt2). Phép lũy thừa mô đun không phải lúc nào cũng trả về căn bậc hai thông thường của số nguyên modulo (P), bởi vì (2) có thể là một số không dư bậc hai. Đó không phải là một lỗi trong phương pháp. Việc xây dựng có chủ ý hoạt động với sự tương tự trường hữu hạn của căn thức, trong đó trường hợp không dư đóng góp một hệ số mở rộng bậc hai cố định. Bởi vì mọi thuật ngữ đã có sự lựa chọn độc lập (+) hoặc (-), nên những hướng cố định đó không làm thay đổi số lượng kết hợp có dấu bằng 0. Đây là lý do chính khiến thủ thuật mô-đun có thể thay thế một hệ số tự do bình phương không thể thực hiện được.
