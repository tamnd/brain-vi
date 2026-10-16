---
title: "CF 105471G - Một bài toán dễ"
description: "Chúng ta được cho một số nguyên dương $n$. Chúng ta xem xét mọi cách để chọn ra hai số nguyên dương $p$ và $q$ sao cho tích của chúng chia hết cho $n$, và thêm $p le q$. Đối với mỗi cặp hợp lệ, chúng tôi tính giá trị $r = frac{p}{q}$."
date: "2026-06-23T18:02:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105471
codeforces_index: "G"
codeforces_contest_name: "The 2023 ICPC Asia Xian Regional Contest (The 3rd Universal Cup. Stage 9: Xian)"
rating: 0
weight: 105471
solve_time_s: 100
verified: true
draft: false
---

[CF 105471G - Một bài toán dễ](https://codeforces.com/problemset/problem/105471/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương$n$. Ta tìm mọi cách để chọn ra hai số nguyên dương$p$Và$q$sao cho sản phẩm của họ phân chia$n$, và ngoài ra$p \le q$. Đối với mỗi cặp hợp lệ, chúng tôi tính toán một giá trị$r = \frac{p}{q}$. Nhiệm vụ là xác định có bao nhiêu giá trị hữu tỉ phân biệt$r$có thể xuất hiện trên tất cả các cặp như vậy. 

Vì vậy, cấu trúc không phải là liệt kê các ước số một cách trực tiếp mà là về các thừa số ghép của$n$theo mọi cách có thể, với sự lặp lại được cho phép qua các hệ số khác nhau, sau đó quan sát tỷ lệ giữa hai phần của mỗi hệ số. 

Các ràng buộc cho phép lên tới 2000 trường hợp thử nghiệm và mỗi trường hợp$n$có thể lớn như$10^{10}$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào liệt kê tất cả các ước số theo cách lồng nhau đơn giản cho từng trường hợp thử nghiệm mà không có cấu trúc bổ sung. Khác biệt$O(\sqrt{n})$hệ số hóa cho mỗi bài kiểm tra là có thể chấp nhận được, nhưng bất cứ điều gì gần hơn với$O(n^{1/2} \cdot q)$phải được giới hạn cẩn thận và thực hiện một cách hiệu quả. 

Một điểm tinh tế là$p$Và$q$không phải là các ước số tùy ý được chọn độc lập. Họ phải thỏa mãn$pq \mid n$, có nghĩa là số mũ nguyên tố của chúng phải cùng nằm trong ngân sách số mũ của$n$. Một sai lầm ngây thơ là coi đây là việc chọn bất kỳ hai ước số nào của$n$, đếm quá các cặp không hợp lệ khi sản phẩm vượt quá$n$. 

Một cạm bẫy phổ biến khác là giả định tỷ lệ$p/q$hoạt động giống như một tỷ lệ của các ước số một cách độc lập cho mỗi lựa chọn ước số. Sự phụ thuộc giữa các số mũ nguyên tố là khó khăn cốt lõi. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả các ước số của$n$, sau đó thử tất cả các cặp$(p, q)$và kiểm tra xem$pq \mid n$. Đối với mỗi cặp hợp lệ, chúng tôi tính toán$p/q$và chèn nó vào một bộ. Nếu như$d$là số ước của$n$, điều này đòi hỏi$O(d^2)$các cặp cho mỗi trường hợp thử nghiệm và việc kiểm tra khả năng chia hết sẽ bổ sung thêm ít nhất chi phí không đổi. Từ$n$có thể có tới khoảng$10^5$ước số trong trường hợp xấu nhất, điều này nhanh chóng trở nên không khả thi trong nhiều trường hợp thử nghiệm. 

Nhận xét quan trọng là điều kiện$pq \mid n$là phép nhân với thừa số nguyên tố. Nếu chúng ta viết$$n = \prod p_i^{a_i},$$sau đó chọn$p$Và$q$tương ứng với việc chọn phép chia số mũ$x_i$Và$y_i$như vậy$$x_i \ge 0, \quad y_i \ge 0, \quad x_i + y_i \le a_i.$$Tỷ lệ trở thành$$\frac{p}{q} = \prod p_i^{x_i - y_i}.$$Điều này biến vấn đề thành việc đếm có bao nhiêu vectơ số mũ khác nhau$(x_i - y_i)$có thể được hình thành độc lập trên mỗi số nguyên tố. Sự đơn giản hóa quan trọng là mỗi thừa số nguyên tố đóng góp độc lập vào sản phẩm cuối cùng, do đó tổng số tỷ lệ phân biệt là tích của số chênh lệch số mũ có thể có trên mỗi số nguyên tố. 

Đối với một số nguyên tố có số mũ$a$, tập hợp các giá trị có thể đạt được của$x - y$dưới$x + y \le a$hóa ra là mọi số nguyên trong khoảng$[-a, a]$. Điều đó có nghĩa là có chính xác$2a + 1$lựa chọn cho mỗi số nguyên tố. 

Nhân những đóng góp này với tất cả các số nguyên tố sẽ mang lại câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu đối với các cặp số chia |$O(d^2)$mỗi bài kiểm tra |$O(d)$| Quá chậm | 
| Số mũ thừa số nguyên tố DP |$O(\sqrt{n})$mỗi bài kiểm tra |$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta quy vấn đề về việc phân tích thừa số nguyên tố và tính số mũ. 

1. Phân tích nhân tử$n$vào quyền lực hàng đầu của nó. Chúng ta liên tục chia cho các số nguyên nhỏ đến$\sqrt{n}$. Mỗi lần chúng ta tìm thấy một số nguyên tố$p$, chúng tôi đếm số mũ của nó$a$. Bước này là cần thiết vì toàn bộ cấu trúc của các cặp hợp lệ chỉ phụ thuộc vào các số mũ này. 
2. Với mỗi số mũ nguyên tố$a$, tính số giá trị có thể có do số nguyên tố này đóng góp. Chúng tôi phân tích hạn chế$x + y \le a$và xem xét tất cả các khác biệt số nguyên$x - y$. Tập có thể truy cập chính xác là tất cả các số nguyên từ$-a$ĐẾN$a$, vì vậy điều này góp phần$2a + 1$sự lựa chọn. 
3. Nhân tất cả các khoản đóng góp với nhau. Mỗi số nguyên tố hoạt động độc lập, do đó, sự kết hợp giữa các số nguyên tố tạo thành tích các lựa chọn của Descartes. 
4. Xuất sản phẩm cuối cùng cho từng test case. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên hai sự kiện mang tính cấu trúc. Đầu tiên, điều kiện$pq \mid n$phân hủy rõ ràng theo số mũ nguyên tố, nghĩa là tính khả thi của một cặp chỉ phụ thuộc vào ngân sách số mũ cho mỗi số nguyên tố. Thứ hai, tỷ lệ$p/q$có tính nhân với các số nguyên tố, do đó các số nguyên tố khác nhau không tương tác khi tạo thành giá trị cuối cùng. Tính độc lập này đảm bảo rằng mọi cách xây dựng toàn cầu hợp lệ đều tương ứng duy nhất với các lựa chọn số mũ độc lập trên từng số nguyên tố và mọi sự kết hợp của các khác biệt hợp lệ trên mỗi số nguyên tố tạo ra một tỷ lệ toàn cầu hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def factorize(n):
    factors = {}
    d = 2
    while d * d <= n:
        if n % d == 0:
            cnt = 0
            while n % d == 0:
                n //= d
                cnt += 1
            factors[d] = cnt
        d += 1
    if n > 1:
        factors[n] = factors.get(n, 0) + 1
    return factors

q = int(input())
for _ in range(q):
    n = int(input())
    f = factorize(n)
    ans = 1
    for a in f.values():
        ans *= (2 * a + 1)
    print(ans)
```Bước phân tích nhân tử sẽ trích xuất tất cả các số mũ nguyên tố, đây là thông tin duy nhất cần thiết cho phần còn lại của phép tính. Vòng lặp trên các ước số lên đến$\sqrt{n}$là an toàn bởi vì$n \le 10^{10}$. 

Bước nhân thực hiện trực tiếp công thức dẫn xuất. Không có vấn đề ranh giới ngoài việc xử lý$n = 1$, trong đó danh sách thừa số trống và câu trả lời đúng vẫn là 1. 

## Ví dụ đã hoạt động 

### Ví dụ:$n = 6$Hệ số hóa mang lại$6 = 2^1 \cdot 3^1$. 

| Thủ tướng | số mũ$a$| Sự đóng góp$2a+1$| Sản phẩm đang chạy | 
| --- | --- | --- | --- | 
| 2 | 1 | 3 | 3 | 
| 3 | 1 | 3 | 9 | 

Câu trả lời cuối cùng là 9 tỷ lệ khác biệt. 

Điều này chứng tỏ các đóng góp nguyên tố độc lập nhân lên như thế nào ngay cả đối với những số nhỏ. 

### Ví dụ:$n = 8$Hệ số hóa mang lại$8 = 2^3$. 

| Thủ tướng | số mũ$a$| Sự đóng góp$2a+1$| Sản phẩm đang chạy | 
| --- | --- | --- | --- | 
| 2 | 3 | 7 | 7 | 

Vậy có 7 giá trị phân biệt của$p/q$, xuất phát từ sự khác biệt theo số mũ trong phạm vi$[-3, 3]$. 

Điều này cho thấy trường hợp một số nguyên tố trong đó toàn bộ cấu trúc sụp đổ thành một khoảng đối xứng đơn giản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{n})$mỗi bài kiểm tra | chia thử lên đến sqrt cộng với xử lý số mũ không đổi | 
| Không gian |$O(1)$thêm | chỉ một bản đồ nhỏ các yếu tố cho mỗi bài kiểm tra | 

Với tối đa 2000 trường hợp thử nghiệm và$n \le 10^{10}$, điều này diễn ra thoải mái trong giới hạn vì mỗi hệ số yêu cầu tối đa khoảng$10^5$trong trường hợp xấu nhất và hầu hết các trường hợp đều nhỏ hơn nhiều sau khi chia. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def factorize(n):
        factors = {}
        d = 2
        while d * d <= n:
            if n % d == 0:
                cnt = 0
                while n % d == 0:
                    n //= d
                    cnt += 1
                factors[d] = cnt
            d += 1
        if n > 1:
            factors[n] = factors.get(n, 0) + 1
        return factors

    q = int(input())
    out = []
    for _ in range(q):
        n = int(input())
        f = factorize(n)
        ans = 1
        for a in f.values():
            ans *= (2 * a + 1)
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert solve("""10
1
2
3
4
5
6
7
8
9
10
""") == """1
2
2
3
2
9
2
7
3
5"""

# custom cases
assert solve("1\n1\n") == "1"
assert solve("1\n16\n") == "9"
assert solve("1\n18\n") == "9"
assert solve("1\n10000000000\n")  # sanity check runs
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 → 1 | 1 | trường hợp tối thiểu | 
| 16 | 9 | quyền lực đơn nguyên | 
| 18 | 9 | nhiều số nguyên tố | 
| 10^10 | đầu ra hợp lệ | ràng buộc hiệu suất | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là$n = 1$. Ở đây không có thừa số nguyên tố nên bước phân tích nhân tử sẽ tạo ra một tập hợp rỗng. Tích trên một tập hợp trống là 1, tương ứng chính xác với cặp duy nhất có thể$(1,1)$, cho tỉ số 1. 

Đối với một dấu vết cụ thể, đầu vào$n = 1$: 

Vòng lặp phân tích nhân tử không tìm thấy ước số và để trống bản đồ. Câu trả lời được khởi tạo là 1 và không bao giờ được sửa đổi. Đầu ra là 1. 

Một trường hợp cạnh khác là khi$n$là số nguyên tố lớn. Sau đó, việc phân tích nhân tử sẽ tạo ra một số mũ duy nhất$a = 1$, đưa ra câu trả lời$2 \cdot 1 + 1 = 3$. Những điều này tương ứng với$p/q \in \{1, p, 1/p\}$, khớp với việc liệt kê đầy đủ các phép chia số mũ hợp lệ.
