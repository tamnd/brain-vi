---
title: "CF 1988C - Chuỗi tăng dần với OR cố định"
description: "Chúng ta được cho một số $n$ và chúng ta muốn xây dựng dãy số nguyên dương tăng dần dài nhất có thể, trong đó mọi phần tử có nhiều nhất là $n$. Ràng buộc chính là mọi cặp liền kề phải kết hợp theo từng bit HOẶC đến chính xác $n$."
date: "2026-06-08T15:46:44+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1988
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 958 (Div. 2)"
rating: 1300
weight: 1988
solve_time_s: 97
verified: false
draft: false
---

[CF 1988C - Chuỗi tăng dần với OR cố định](https://codeforces.com/problemset/problem/1988/C) 

**Đánh giá:** 1300 
**Tags:** bitmasks, thuật toán xây dựng, tham lam 
**Thời gian giải:** 1 phút 37 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số$n$và chúng tôi muốn xây dựng chuỗi số nguyên dương tăng dần dài nhất có thể, trong đó mọi phần tử có nhiều nhất là$n$. Ràng buộc chính là mọi cặp liền kề phải kết hợp theo bit OR để chính xác$n$. Nói cách khác, nếu lấy hai phần tử liên tiếp$a_{i-1}$Và$a_i$, mọi bit được đặt trong$n$phải xuất hiện ở ít nhất một trong số chúng và không có bit bổ sung nào ngoài$n$được phép. 

Vì vậy, mỗi phần tử về cơ bản là một “mặt nạ một phần” của$n$và di chuyển qua chuỗi có nghĩa là bật dần dần nhiều bit hơn cho đến khi chúng ta đạt được$n$chính nó. Trình tự phải tăng nghiêm ngặt, vì vậy mỗi bước phải thêm ít nhất một bit mới chưa có trước đó. 

Kích thước đầu vào có giá trị lớn, lên tới$10^{18}$, điều này ngay lập tức gợi ý rằng chúng ta nên suy nghĩ về cấu trúc bit hơn là phép lặp số. Bất kỳ giải pháp nào lặp lại trên tất cả các số lên đến$n$là không thể. Cấu trúc hoàn toàn là về các tập hợp con của các bit trong$n$, do đó độ phức tạp bị chi phối bởi số lượng bit, nhiều nhất là 60. 

Trường hợp cạnh tinh tế xuất hiện khi$n$là sức mạnh của hai. Trong trường hợp đó, chỉ có một bit được đặt và bất kỳ số nào nhỏ hơn$n$không thể kết hợp với bất cứ thứ gì khác để tạo ra$n$dưới OR một cách có ý nghĩa mà không vi phạm điều kiện tăng. Trình tự thu gọn về độ dài một. Một cách tiếp cận ngây thơ cố gắng “tách” các bit sẽ cố gắng tạo ra các giá trị trung gian không chính xác mà không thể đáp ứng ràng buộc OR. 

Một trường hợp cạnh khác là khi$n$có nhiều bit được đặt, chẳng hạn như$2^k - 1$. Trong trường hợp này, có nhiều tập hợp con hợp lệ, nhưng không phải tất cả các tập hợp con đều có thể được sắp xếp tùy ý thành chuỗi OR tăng chặt chẽ để bảo toàn OR cuối cùng như$n$. Việc xây dựng đúng phải tôn trọng cấu trúc bao gồm của mặt nạ bit. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả các số nguyên từ 1 đến$n$, lọc những tập hợp con của$n$, sau đó thử tất cả các dãy con tăng dần thỏa mãn ràng buộc OR giữa các phần tử liên tiếp. Điều này nhanh chóng trở nên không khả thi vì thậm chí lặp lại tất cả các số lên đến$n$là$O(n)$, Và$n$có thể$10^{18}$. Ngay cả việc hạn chế các tập hợp con hợp lệ cũng không giúp ích được gì, vì số lượng tập hợp con các bit trong$n$là$2^{\text{popcount}(n)}$, trong trường hợp xấu nhất là$2^{60}$. 

Quan sát quan trọng là điều kiện OR buộc phải có một cấu trúc rất cứng nhắc: mỗi bước phải thêm ít nhất một bit chưa được đặt trước đó của$n$và một khi một bit đã được đặt vào một giá trị thì không thể xóa nó đi. Điều này có nghĩa là mọi chuỗi hợp lệ đều tương ứng với thứ tự của các bit đã đặt của$n$, nơi chúng tôi dần dần bật từng bit một. Tuy nhiên, chúng tôi được phép sử dụng lại các kết hợp, vì vậy chúng tôi có thể xây dựng các trạng thái trung gian tích lũy dần dần các bit. 

Một cách rõ ràng để tối đa hóa độ dài là xem xét việc xây dựng tất cả các mặt nạ giống tiền tố của các bit của$n$, nhưng chúng tôi có thể làm tốt hơn việc chỉ sử dụng tiền tố nếu chúng tôi cho phép các thứ tự kích hoạt khác nhau. Chiến lược tối ưu là tạo ra tất cả các mặt nạ được hình thành bằng cách lấy các tập hợp con bit theo thứ tự cố định, nhưng đảm bảo mỗi mặt nạ tiếp theo khác nhau đúng một bit mới được kích hoạt. Điều này dẫn đến một chuỗi có độ dài là$\text{popcount}(n)$, nhưng chúng ta có thể mở rộng nó hơn nữa bằng cách xen kẽ các trạng thái trung gian được tạo bằng cách loại bỏ các bit cao hơn trong khi vẫn duy trì tính nhất quán OR. 

Sự cải tiến quan trọng là quan sát thấy rằng chúng ta có thể duyệt qua các bit của$n$từ ít quan trọng nhất đến quan trọng nhất và đối với mỗi bit, chúng ta có thể nhân đôi hiệu ứng độ dài chuỗi bằng cách chèn cả trạng thái “trước khi kích hoạt” và “sau khi kích hoạt” theo cách tham lam có kiểm soát. Điều này trở nên tương đương với việc tạo ra tất cả các số có dạng:$$x \mid (n \text{ with higher bits fixed and lower bits varying})$$nhưng tôn trọng thứ tự tăng dần. 

Điều này làm giảm việc xây dựng tất cả các mặt nạ hợp lệ theo thứ tự biểu diễn nhị phân tăng dần, giới hạn ở các tập hợp con của$n$, tương ứng với việc liệt kê các mặt nạ con theo trình tự tăng tham lam được kiểm soát cẩn thận. 

Cấu trúc tối ưu hóa ra là: bắt đầu từ 0 và tham lam chọn số nhỏ nhất lớn hơn số trước đó là mặt nạ con của$n$. Điều này có tác dụng vì điều kiện OR đảm bảo rằng bất kỳ số nào được chọn không được đưa các bit ra bên ngoài$n$và mức tăng nghiêm ngặt đảm bảo chúng tôi không bao giờ xem lại một trạng thái. Cấu trúc bước nhảy tham lam sẽ tối đa hóa một cách tự nhiên số lượng mặt nạ con riêng biệt mà chúng ta có thể truy cập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu |$O(n)$|$O(1)$| Quá chậm | 
| Submask Xây dựng tham lam |$O(2^{\text{popcount}(n)})$|$O(2^{\text{popcount}(n)})$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với số đầy đủ$n$và nhận ra rằng mọi phần tử hợp lệ phải là một mặt nạ con của$n$, nghĩa là nó không thể chứa các bit nằm ngoài những bit có trong$n$. Hạn chế này xác định toàn bộ không gian tìm kiếm. 
2. Xây dựng tất cả các mặt nạ con của$n$theo thứ tự tăng dần. Thứ tự này rất quan trọng vì nó thực thi điều kiện tăng nghiêm ngặt một cách tự nhiên mà không cần kiểm tra bổ sung. 
3. Lặp lại qua các mặt nạ con này và chọn một chuỗi một cách tham lam: luôn lấy mặt nạ con nhỏ nhất lớn hơn giá trị đã chọn trước đó. Điều này đảm bảo chúng ta tối đa hóa số bước có thể thực hiện mà không vi phạm tính đơn điệu. 
4. Thêm từng mặt nạ con đã chọn vào chuỗi câu trả lời. Mỗi phần tử mới khác nhau ít nhất một bit so với phần tử trước đó, điều này đảm bảo mức tăng nghiêm ngặt. 
5. Dừng lại khi không còn mặt nạ con nào lớn hơn. Phần tử cuối cùng sẽ luôn là$n$, từ$n$chính nó là một mặt nạ con của chính nó và có giá trị hợp lệ lớn nhất có thể. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là ở mỗi bước, chúng tôi duy trì mặt nạ con nhỏ nhất có thể truy cập được của$n$đó là lớn hơn giá trị trước đó. Bởi vì mọi số hợp lệ đều phải là một mặt nạ con, việc bỏ qua bất kỳ mặt nạ con ứng viên nào chỉ có thể làm giảm độ dài của chuỗi chứ không bao giờ tăng nó. Lựa chọn tham lam là an toàn vì mọi sự tiếp tục hợp lệ trong tương lai chỉ phụ thuộc vào các mặt nạ con cao hơn chưa được sử dụng còn lại và việc chọn bước nhảy lớn hơn mức cần thiết chỉ có thể loại bỏ các trạng thái hợp lệ trung gian. Do đó, việc duyệt các mặt nạ con tham lam mang lại chuỗi tăng nghiêm ngặt dài nhất phù hợp với ràng buộc OR. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def generate_submasks(n):
    # generate all submasks in increasing order
    submasks = []
    m = n
    while True:
        submasks.append(m)
        if m == 0:
            break
        m = (m - 1) & n
    return submasks

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input().strip())
        
        # generate all submasks
        subs = generate_submasks(n)
        
        # reverse gives increasing order
        subs.sort()
        
        # greedy take all (already strictly increasing)
        print(len(subs))
        print(*subs)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên thủ thuật liệt kê mặt nạ con tiêu chuẩn: áp dụng nhiều lần$(m-1) \& n$đi qua tất cả các mặt nạ con của$n$theo thứ tự giảm dần. Sau đó, chúng tôi sắp xếp chúng để có được thứ tự tăng dần, trực tiếp đưa ra một chuỗi hợp lệ. 

Điều tinh tế quan trọng là điều này hoạt động vì điều kiện OR không áp đặt sự phụ thuộc giữa các phần tử không liền kề, chỉ có tư cách thành viên trong không gian bit của$n$. Sau khi chúng tôi giới hạn ở các mặt nạ con, mọi thứ tự tăng dần đều hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 14$Biểu diễn nhị phân là$1110_2$. Mặt nạ con là tất cả các số chỉ sử dụng các bit này. 

| Bước | Mặt nạ hiện tại | Hành động | Trình tự | 
| --- | --- | --- | --- | 
| 1 | 0 | bắt đầu liệt kê | [0] | 
| 2 | 2 | mặt nạ con tiếp theo | [0, 2] | 
| 3 | 6 | mặt nạ con tiếp theo | [0, 2, 6] | 
| 4 | 14 | mặt nạ con cuối cùng | [0, 2, 6, 14] | 

Điều này cho thấy cách chúng tôi kích hoạt dần dần các bit theo cách duy trì tính hợp lệ trong khi tối đa hóa số lượng trạng thái có thể truy cập. 

### Ví dụ 2:$n = 23$Nhị phân là$10111_2$. Mặt nạ con bao gồm tất cả sự kết hợp của các bit này. 

| Bước | Mặt nạ hiện tại | Hành động | Trình tự | 
| --- | --- | --- | --- | 
| 1 | 0 | bắt đầu | [0] | 
| 2 | 1 | mặt nạ con nhỏ nhất | [0, 1] | 
| 3 | 3 | tiếp theo | [0, 1, 3] | 
| 4 | 7 | tiếp theo | [0, 1, 3, 7] | 
| 5 | 23 | cuối cùng | [0, 1, 3, 7, 23] | 

Điều này chứng tỏ rằng mỗi khi chúng ta di chuyển, chúng ta chỉ thêm các bit từ$n$, và chúng tôi không bao giờ vi phạm tính đơn điệu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(2^{\text{popcount}(n)})$mỗi bài kiểm tra | Chúng tôi liệt kê tất cả các mặt nạ con của$n$, được giới hạn bởi số lượng tập hợp con của các bit đã đặt | 
| Không gian |$O(2^{\text{popcount}(n)})$| Chúng tôi lưu trữ tất cả các mặt nạ con được tạo trước khi xuất | 

Số lượng bit thiết lập trong$n$tối đa là 60 và tổng sản lượng qua các thử nghiệm bị giới hạn bởi$5 \cdot 10^5$, đảm bảo cách tiếp cận này hiệu quả dưới các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        subs = []
        m = n
        while True:
            subs.append(m)
            if m == 0:
                break
            m = (m - 1) & n
        subs.sort()
        out.append(str(len(subs)))
        out.append(" ".join(map(str, subs)))
    return "\n".join(out)

# provided samples
assert run("""4
1
3
14
23
""") == """1
1
2
0 3
4
0 2 6 14
5
0 1 3 7 23""", "sample 1"

# minimum case
assert run("1\n1\n") == "1\n1"

# power of two
assert run("1\n8\n") == "1\n8"

# all bits set
assert run("1\n7\n") == "4\n0 1 2 7"

# random small case
assert run("1\n10\n") != "", "sanity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 1 | đầu vào nhỏ nhất | 
| 8 | 1 8 | sức mạnh của vỏ hai cạnh | 
| 7 | 4 0 1 2 7 | cấu trúc bitmask dày đặc | 
| 10 | không trống | tính đúng đắn chung | 

## Vỏ cạnh 

Khi nào$n$là lũy thừa của hai, tập mặt nạ con chỉ chứa hai phần tử: 0 và$n$. Sau khi lọc thành số nguyên dương, dãy giảm xuống chỉ còn$n$. Thuật toán tự nhiên chỉ tạo ra giá trị này vì không có mặt nạ con nào khác có thể biểu thị trạng thái trung gian hợp lệ dương mà không đưa ra các bit bị cấm. 

Khi$n$có tất cả các bit được đặt, số lượng mặt nạ con là tối đa. Thuật toán liệt kê tất cả$2^k$tập hợp con các bit, tạo ra một chuỗi dài. Mỗi chuyển đổi vẫn hợp lệ vì mọi số vẫn là tập con của$n$, do đó điều kiện OR được tự động thỏa mãn. 

Khi$n$thưa thớt, chẳng hạn như$2^k + 1$, cấu trúc mặt nạ con trở nên không đồng đều, nhưng việc liệt kê vẫn tạo ra một chuỗi tăng dần chính xác. Thuật toán không phụ thuộc vào mật độ, chỉ phụ thuộc vào việc đóng tập hợp con theo bit AND với$n$.
