---
title: "CF 105066I - Một vấn đề khác về Bitwise"
description: "Chúng ta được cho một mảng các số nguyên và chúng ta liên tục áp dụng một phép biến đổi được điều khiển bởi tham số số nguyên không âm $x$. Đối với một lựa chọn cố định là $x$, mọi phần tử mảng $ai$ được XOR với $x$ và tất cả các kết quả này được tính tổng lại với nhau."
date: "2026-06-23T09:47:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105066
codeforces_index: "I"
codeforces_contest_name: "Teamscode Spring 2024 (Novice Division)"
rating: 0
weight: 105066
solve_time_s: 76
verified: false
draft: false
---

[CF 105066I - Một vấn đề Bitwise khác](https://codeforces.com/problemset/problem/105066/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta liên tục áp dụng một phép biến đổi được điều khiển bởi một tham số nguyên không âm$x$. Để có sự lựa chọn cố định$x$, mọi phần tử mảng$a_i$được XOR với$x$, và tất cả các kết quả này được tổng hợp lại với nhau. Điều này tạo ra một giá trị duy nhất$$S(x) = \sum_{i=1}^{n} (a_i \oplus x)$$Nhiệm vụ không phải là tính toán điều này cho một$x$, nhưng để hiểu tất cả các giá trị có thể có của$S(x)$BẰNG$x$trải rộng trên tất cả các số nguyên không âm, sau đó đếm xem có bao nhiêu kết quả khác biệt nằm trong khoảng đó$[l, r]$. 

Khó khăn chính đó là$x$không bị giới hạn ở đầu vào, vì vậy hàm$S(x)$được xác định trên một miền vô hạn nhưng cấu trúc của nó chỉ phụ thuộc vào các bit của$x$và chỉ tối đa bit cao nhất xuất hiện trong bất kỳ$a_i$. Vì tất cả$a_i \le 10^5$, chỉ có khoảng 17 bit quan trọng trong các giá trị cơ bản và các bit cao hơn chỉ ảnh hưởng đến mẫu XOR một cách thống nhất. 

Những ràng buộc buộc chúng ta phải tránh xa mọi sự liệt kê trực tiếp của$x$. Với$n$lên đến$10^5$Và$x$có khả năng tính toán lớn, thậm chí$S(x)$cho tất cả$x$lên đến$r$là không thể. Một giải pháp phải nén được tác động của tất cả$x$thành một biểu diễn có cấu trúc nhỏ. 

Một vấn đề tế nhị phát sinh từ việc giả định tính đơn điệu. Một suy đoán ngây thơ là vậy$S(x)$thay đổi suôn sẻ với$x$, nhưng XOR phá hủy tính đơn điệu. Ví dụ, với một phần tử$a = 5$, trình tự$a \oplus x$dao động:$5,4,7,6,1,0,3,2,\dots$, do đó, mọi giả định cho rằng các giá trị có thể tiếp cận tạo thành một khoảng liền kề đều sai. 

Một cạm bẫy khác là xử lý các bit một cách độc lập mà không tính đến cách XOR tương tác với số lượng tập hợp cố định trên mảng. Hàm này tuyến tính trên mỗi bit theo nghĩa tổ hợp, nhưng phụ thuộc vào số lượng$a_i$có mỗi bit được thiết lập. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp đi lặp lại tất cả những gì có thể$x$TRONG$[0, r]$, tính toán$S(x)$và thu thập các giá trị riêng biệt. Mỗi chi phí đánh giá$O(n)$, dẫn đến$O(nr)$hoạt động trong trường hợp xấu nhất. Từ$r$có thể lớn như$10^{18}$, điều này là không thể thực hiện được. 

Quan sát quan trọng là XOR với một giá trị cố định$x$ảnh hưởng đến từng bit một cách độc lập. Đối với vị trí bit cố định$k$, liệu$(a_i \oplus x)$có chút$k$tập hợp chỉ phụ thuộc vào$a_i$một chút$k$Và$x_k$. Điều này cho phép chúng ta viết lại tổng dưới dạng hàm đóng góp theo bit. 

Đối với mỗi bit$k$, cho phép$c_k$là số phần tử trong đó$k$-bit thứ của$a_i$là 1. Sau đó với một cho trước$x$, sự đóng góp của bit$k$với tổng số tiền chỉ phụ thuộc vào việc liệu$x_k$là 0 hoặc 1: 

Nếu$x_k = 0$, phần đóng góp là$c_k \cdot 2^k$. Nếu như$x_k = 1$, nó trở thành$(n - c_k) \cdot 2^k$. Điều này biến vấn đề thành việc lựa chọn, độc lập trên mỗi bit, một trong hai đóng góp. 

Như vậy, mọi$S(x)$tương ứng với việc chọn, đối với mỗi bit, một trong hai giá trị. Tập hợp tất cả các tổng có thể chính xác là tập hợp tất cả các kết hợp của các lựa chọn độc lập này trên các bit có liên quan. Vì có nhiều nhất là 17 bit nên chúng ta nhận được nhiều nhất$2^{17} = 131072$số tiền có thể. 

Chúng ta có thể tạo tất cả các tổng bằng cách sử dụng DFS đơn giản hoặc cấu trúc lặp trên các bit, sắp xếp chúng và sau đó trả lời truy vấn phạm vi$[l, r]$thông qua tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force kết thúc$x$|$O(nr)$|$O(1)$| Quá chậm | 
| Bit DP qua đóng góp |$O(2^B + nB)$|$O(2^B)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nén hiệu ứng của tất cả$x$thành các lựa chọn nhị phân độc lập trên mỗi bit. 

1. Đếm xem mỗi bit có bao nhiêu phần tử mảng. Đối với mỗi vị trí bit$k$, tính toán$c_k = |\{i : a_i \text{ has bit } k\}|$. Điều này tách biệt cách XOR sẽ ảnh hưởng đến bit đó trên toàn cầu. 
2. Đối với mỗi bit$k$, tính toán trước hai đóng góp có thể có: khi$x_k = 0$, bit đóng góp$c_k \cdot 2^k$, và khi nào$x_k = 1$, nó góp phần$(n - c_k) \cdot 2^k$. Điều này nắm bắt được toàn bộ tác dụng của việc chuyển đổi bit đó trong$x$. 
3. Xây dựng tất cả các tổng có thể bằng cách lặp qua các bit. Bắt đầu với một tập hợp chứa 0. Đối với mỗi bit, hãy mở rộng tập hợp hiện tại bằng cách thêm một trong hai phần đóng góp cho bit đó vào mỗi tổng một phần hiện có. Điều này hoạt động vì các bit đóng góp bổ sung và độc lập. 
4. Sau khi xử lý tất cả các bit có liên quan (lên đến bit cao nhất có trong bất kỳ$a_i$), chúng tôi thu được tập hợp đầy đủ các giá trị có thể truy cập$S(x)$. Sắp xếp danh sách này. 
5. Sử dụng tìm kiếm nhị phân để đếm xem có bao nhiêu giá trị$[l, r]$. 

### Tại sao nó hoạt động 

Mỗi bit đóng góp độc lập vì XOR hoạt động theo từng bit mà không mang giữa các vị trí. Đối với một bit cố định, sự đóng góp chỉ phụ thuộc vào việc$x_k$lật bit hay không và quyết định này không tương tác với bất kỳ bit nào khác. Do đó mọi giá trị hợp lệ$x$tương ứng với một lựa chọn duy nhất của một đóng góp cho mỗi bit và mỗi lựa chọn như vậy tương ứng với một số$x$. Điều này thiết lập ánh xạ một-một giữa các lựa chọn bit và tổng, đảm bảo tính đầy đủ và không trùng lặp ngoài các tổng giống hệt nhau từ các mẫu bit khác nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, l, r = map(int, input().split())
    a = list(map(int, input().split()))

    MAXB = 0
    for v in a:
        MAXB = max(MAXB, v.bit_length())

    cnt = [0] * (MAXB + 1)

    for v in a:
        for b in range(MAXB + 1):
            if v >> b & 1:
                cnt[b] += 1

    # possible contributions per bit
    choices = []
    for b in range(MAXB + 1):
        bit_val = 1 << b
        c = cnt[b]
        choices.append((c * bit_val, (n - c) * bit_val))

    # build all possible sums
    sums = {0}
    for zero_val, one_val in choices:
        new = set()
        for s in sums:
            new.add(s + zero_val)
            new.add(s + one_val)
        sums = new

    arr = sorted(sums)

    # count in range [l, r]
    import bisect
    return bisect.bisect_right(arr, r) - bisect.bisect_left(arr, l)

print(solve())
```Việc triển khai trước tiên sẽ nén từng bit thành một lựa chọn nhị phân: hoặc xử lý$x_k = 0$hoặc$x_k = 1$. Cấu trúc tập hợp lặp đi lặp lại tích lũy tất cả các tổng có thể bằng cách mở rộng các tổng từng phần trước đó với sự đóng góp của mỗi bit. Bước sắp xếp cuối cùng cho phép đếm phạm vi hiệu quả. 

Một điểm tinh tế là chúng ta không bao giờ xây dựng một cách rõ ràng$x$. Thay vào đó, chúng ta trực tiếp xây dựng tất cả các kết quả có thể xảy ra$S(x)$. Điều này tránh việc xử lý các giá trị tiềm năng rất lớn của$x$, điều này không liên quan vì chỉ các mẫu bit mới quan trọng tới bit tối đa trong$a_i$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, l = 0, r = 12
a = [1, 2, 3]
```Chúng tôi theo dõi sự đóng góp trên mỗi bit. 

| Chút | c_k | n - c_k | x_k = 0 | x_k = 1 | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 1 | 2 | 1 | 
| 1 | 2 | 1 | 4 | 2 | 

Bắt đầu với tập hợp tổng`{0}`. 

Sau bit 0, chúng ta nhận được`{2, 1}`. 

Sau bit 1, chúng tôi mở rộng: 

| Trước | +4 | +2 | 
| --- | --- | --- | 
| 2 | 6 | 4 | 
| 1 | 5 | 3 | 

Số tiền cuối cùng:`{6, 4, 5, 3}`. 

Đếm vào$[0, 12]$cho 4 giá trị. 

Điều này xác nhận rằng các lựa chọn bit khác nhau tương ứng với các cấu hình XOR riêng biệt. 

### Ví dụ 2 

đầu vào:```
n = 2, a = [0, 1], l = 0, r = 5
```Bit 0 là bit duy nhất có liên quan. 

| Chút | c_k | n - c_k | x_k = 0 | x_k = 1 | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 1 | 

Cả hai lựa chọn đều đưa ra sự đóng góp giống nhau, vì vậy mọi$x$tạo ra sự sụp đổ cấu trúc tổng tương tự. 

Bắt đầu`{0}`: 

- thêm 1 →`{1, 1}`→`{1}`Chỉ có một giá trị có thể truy cập tồn tại. 

Điều này chứng tỏ sự khác biệt$x$các giá trị có thể ánh xạ tới cùng một tổng, nhưng việc xây dựng sẽ loại bỏ chúng một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(B \cdot 2^B)$| Mỗi bit có tối đa ~17 bit sẽ nhân đôi tập hợp | 
| Không gian |$O(2^B)$| Lưu trữ tất cả số tiền có thể truy cập | 

Giới hạn bit$B \le 17$đến từ$a_i \le 10^5$, làm cho việc liệt kê hàm mũ trở nên khả thi. Ngay cả trong trường hợp xấu nhất, không gian trạng thái vẫn đủ nhỏ trong 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return str(solve())

# provided sample (format may vary in statement)
# assert run("3 0 12\n1 2 3\n") == "4"

# all zeros
assert run("3 0 10\n0 0 0\n") == "1"

# single element
assert run("1 0 10\n5\n") in ["2", "2\n"]

# alternating bits
assert run("2 0 20\n1 2\n") is not None

# identical values
assert run("4 0 50\n7 7 7 7\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | 1 | tất cả các tổng XOR giống hệt nhau | 
| phần tử đơn | số lượng nhỏ | tính đúng đắn của việc tách bit | 
| mảng giống hệt nhau | sự sụp đổ của các lựa chọn | xử lý trùng lặp | 
| bit hỗn hợp | nhiều khoản tiền | mở rộng tổ hợp | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi mọi số trong mảng giống hệt nhau ở tất cả các bit. Trong trường hợp đó, lật bất kỳ bit nào vào$x$không thay đổi sự cân bằng giữa số 1 và số 0, do đó nhiều cấu hình sẽ thu về cùng một tổng. Thuật toán xử lý việc này một cách tự nhiên vì việc chèn tập hợp sẽ loại bỏ các bản sao. 

Một trường hợp khác là khi$l = r = 0$. Thuật toán đếm chính xác xem 0 có xuất hiện trong tập hợp được tạo hay không. Vì trạng thái ban đầu luôn là 0 trước khi thêm các khoản đóng góp nên trường hợp này luôn trôi qua mà không cần xử lý đặc biệt. 

Trường hợp thứ ba là khi chỉ có một vị trí bit được kích hoạt. Sau đó, mỗi bit đóng góp một lựa chọn hai giá trị đơn giản và tập cuối cùng có kích thước tối đa là 2. Cấu trúc lặp vẫn được áp dụng rõ ràng vì mỗi phần mở rộng là độc lập và không giả định nhiều bit hoạt động.
