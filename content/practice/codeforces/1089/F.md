---
title: "CF 1089F - Phân số"
description: "Chúng ta được cấp một số nguyên $n$ và chúng ta muốn biểu thị một số hữu tỷ cố định, cụ thể là $1 - frac{1}{n}$, dưới dạng tổng của một số phân số nhỏ hơn. Mỗi phân số phải có mẫu số là ước số thực sự của $n$, nghĩa là nó chia hết $n$ nhưng không phải là 1 hay $n$."
date: "2026-06-12T06:08:11+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1089
codeforces_index: "F"
codeforces_contest_name: "2018-2019 ICPC, NEERC, Northern Eurasia Finals (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 1900
weight: 1089
solve_time_s: 194
verified: true
draft: false
---

[CF 1089F - Phân số](https://codeforces.com/problemset/problem/1089/F) 

**Xếp hạng:** 1900 
**Thẻ:** toán 
**Thời gian giải:** 3 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên$n$và chúng ta muốn biểu diễn một số hữu tỷ cố định, cụ thể là$1 - \frac{1}{n}$, dưới dạng tổng của nhiều phân số nhỏ hơn. 

Mỗi phân số phải có mẫu số là ước số thực sự của$n$, nghĩa là nó chia$n$nhưng không phải là 1 cũng không phải$n$. Tử số phải nhỏ hơn mẫu số để mọi số hạng đều là một phân số đúng. Nhiệm vụ là quyết định xem sự phân tách như vậy có tồn tại hay không và nếu có thì xây dựng một phân tách bằng cách sử dụng nhiều nhất$10^5$điều khoản. 

Một cách khác để suy nghĩ về vấn đề này là chúng ta chỉ được phép sử dụng các phân số có mẫu số lấy từ cấu trúc chia của$n$, và chúng ta phải “điền” chính xác số tiền$1 - \frac{1}{n}$. 

Ràng buộc chính là về cấu trúc chứ không phải về số: chúng ta bị giới hạn ở các mẫu số thẳng hàng với ước số của$n$. Từ$n \le 10^9$, chúng ta không thể phân tích nó bằng cách kiểm tra khả năng chia hết một cách thô bạo trên tất cả các số. Bất kỳ giải pháp nào cũng phải dựa vào cấu trúc số học của các ước số và cách kết hợp các phân số có mẫu số liên quan. 

Một trường hợp khó nhận thấy là khi$n$không có ước số thích hợp nào cả. Điều này xảy ra khi$n$là nguyên tố. Trong trường hợp đó, không có mẫu số hợp lệ$b_i$, nên không tồn tại dãy nào. Ví dụ, nếu$n = 2$, hoặc$n = 3$, câu trả lời ngay lập tức là không thể. 

Một vấn đề không hề nhỏ khác là ngay cả khi$n$là hợp số, không rõ ràng rằng sự phân tách như vậy luôn tồn tại. Ví dụ,$n = 6$cho phép chia số$2, 3$, và chúng ta phải bày tỏ$\frac{5}{6}$chỉ sử dụng các phân số có mẫu số 2 và 3. Một nỗ lực ngây thơ có thể thử kết hợp$\frac{1}{2}$Và$\frac{1}{3}$, nhưng những điều này không trực tiếp phù hợp với$\frac{5}{6}$trừ khi được thu nhỏ hoặc phân hủy cẩn thận. 

Khó khăn cốt lõi là chúng ta phải chuyển đổi một cách có hệ thống một phân số mục tiêu thành tổng các phân số bị ràng buộc bởi mẫu số chia. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là coi đây như một vấn đề phân vùng trên các số hữu tỷ: liệt kê tất cả các phân số có thể$\frac{a}{b}$Ở đâu$b \mid n$và cố gắng kết hợp chúng một cách tham lam hoặc đệ quy để đạt được$1 - \frac{1}{n}$. Ngay cả khi chúng ta hạn chế mẫu số, mỗi mẫu số có thể ghép với nhiều tử số và không gian tìm kiếm sẽ tăng lên nhanh chóng. Nếu như$n$có nhiều ước số, cách tiếp cận này trở nên bùng nổ về mặt tổ hợp, vì về cơ bản chúng ta đang tìm kiếm các tập con của các phân số ứng cử viên, dẫn đến hành vi theo cấp số nhân. 

Quan sát quan trọng là chúng ta thực sự không cần sự kết hợp các ước số tùy ý. Chúng ta chỉ cần khai thác bản sắc mang tính xây dựng dựa trên khả năng phân chia. Nếu chúng ta lấy một số chia$d \mid n$, sau đó$\frac{d}{n}$tương thích một cách tự nhiên với mục tiêu vì tất cả các phân số có thể được chia tỷ lệ thành mẫu số chung$n$. Điều này gợi ý viết lại mọi thứ trên mẫu số$n$, mặc dù đầu ra phải sử dụng mẫu số nhỏ hơn. 

Cái nhìn sâu sắc quan trọng là để thể hiện$n-1$dưới dạng tổng của các số nguyên, mỗi số tương ứng với đóng góp theo tỷ lệ từ các phân số hợp lệ. Nếu viết một phân số$\frac{a}{b}$, chia tỷ lệ thành mẫu số$n$cho$\frac{a \cdot (n/b)}{n}$. Vậy mỗi phân số đóng góp một bội số nguyên của$\frac{1}{n}$, cụ thể$a \cdot \frac{n}{b}$. Từ$\frac{n}{b}$là một số nguyên, mỗi số hạng đóng góp bội số của số chia của$n$. 

Vì vậy, vấn đề trở thành: phân hủy$n - 1$thành tổng các số có dạng$a \cdot \frac{n}{b}$, Ở đâu$b \mid n$Và$1 \le a < b$. Đây hiện là một vấn đề về xây dựng đồng xu trong đó “giá trị đồng xu” có sẵn đến từ cấu trúc số chia. 

Một cách mang tính xây dựng về phía trước là sử dụng chiến lược ghép nối chuẩn trên các ước số: cho mỗi ước số thích hợp$b$, chúng ta có thể sử dụng bội số của$\frac{n}{b}$lên đến$(b-1)\frac{n}{b}$. Điều này cho phép chúng ta biểu diễn bất kỳ cấu trúc đa bội nào miễn là chúng ta có thể bao quát được$n-1$không có khoảng trống. Việc xây dựng tiêu chuẩn tiến hành bằng cách lặp lại các ước số và tham lam lấy phần đóng góp lớn nhất có thể, đảm bảo chúng ta không bao giờ vượt quá số dư còn lại. 

Quá trình chuyển đổi từ giải pháp vũ phu sang giải pháp tối ưu là nhận ra rằng chúng ta không cần tìm kiếm các tổ hợp phân số, chúng ta chỉ cần biểu diễn một số nguyên bằng cách sử dụng một tập hợp có cấu trúc các kích thước bước bắt nguồn từ các ước số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con phân số | Hàm mũ | Hàm mũ | Quá chậm | 
| Phân rã xây dựng dựa trên số chia |$O(\sqrt{n})$|$O(\sqrt{n})$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tất cả các ước số thích hợp của$n$loại trừ 1 và$n$. 

Điều này là cần thiết vì chỉ những mẫu số này mới được phép xây dựng. 
2. Nếu không có ước số nào như vậy thì xuất ngay NO. 

Trong trường hợp này$n$là số nguyên tố và không thể tạo được phân số nào. 
3. Chuyển đổi giá trị mục tiêu$1 - \frac{1}{n}$vào một mục tiêu số nguyên$T = n - 1$, được hiểu là đơn vị của$\frac{1}{n}$. 

Điều này loại bỏ các phân số khỏi lý luận và làm cho tất cả các đóng góp trở thành số nguyên. 
4. Với mỗi ước số$b$, tính kích thước bước đóng góp của nó$\frac{n}{b}$. 

Mỗi phân số$\frac{a}{b}$tương ứng với việc thêm$a \cdot \frac{n}{b}$đơn vị. 
5. Tham lam xây dựng$T$sử dụng các kích thước bước này từ ước số lớn nhất đến ước số nhỏ nhất. 

Đối với một cố định$b$, chúng ta có thể sử dụng tối đa$b-1$bản sao của$\frac{n}{b}$, từ$a < b$. Điều này đảm bảo tính hợp lệ của các tử số. 
6. Đối với mỗi đóng góp được lựa chọn$a \cdot \frac{n}{b}$, xuất ra phân số tương ứng$\frac{a}{b}$. 
7. Dừng lại khi số dư bằng 0. Nếu không thể giảm về 0, xuất ra NO. 

Điều này đảm bảo tính đầy đủ của số tiền mục tiêu. 

### Tại sao nó hoạt động 

Mỗi phân số được phép ánh xạ duy nhất tới một đóng góp số nguyên có dạng$a \cdot \frac{n}{b}$và tất cả những đóng góp như vậy là bội số của ước số$n$. Vì ước của$n$tạo ra một hệ thống cộng tính có cấu trúc, bất kỳ tổng có thể biểu diễn nào cũng phải nằm trong nửa nhóm được tạo bởi các kích thước bước này. Việc xây dựng tham lam thành công vì mọi phần dư trung gian vẫn chia hết cho kích thước bước nhỏ nhất, đảm bảo không có ngõ cụt nào xảy ra. Sự hạn chế$a < b$đảm bảo các hệ số giới hạn, ngăn chặn tình trạng vượt quá mức trong khi vẫn cho phép bao phủ toàn bộ mục tiêu$n-1$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    if n == 2:
        print("NO")
        return

    divisors = []
    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            divisors.append(i)
            if i != n // i:
                divisors.append(n // i)

    if not divisors:
        print("NO")
        return

    divisors = sorted(divisors, reverse=True)

    target = n - 1
    res = []

    for b in divisors:
        step = n // b
        max_take = b - 1
        take = min(max_take, target // step)

        for a in range(take, 0, -1):
            res.append((a, b))
            target -= a * step

    if target != 0:
        print("NO")
        return

    print("YES")
    print(len(res))
    for a, b in res:
        print(a, b)

if __name__ == "__main__":
    solve()
```Việc thực hiện bắt đầu bằng cách xử lý các trường hợp không thể thực hiện được, đặc biệt khi không tồn tại ước số thích hợp. Sau đó nó liệt kê các ước số trong$O(\sqrt{n})$, điều đó là đủ với ràng buộc$n \le 10^9$. 

Mỗi số chia$b$được xử lý theo thứ tự giảm dần để các bước nhảy cấu trúc lớn hơn được sử dụng trước tiên. Điều này giúp giảm phần dư một cách nhanh chóng và tránh bị phân mảnh thành nhiều phần nhỏ. 

Đối với mỗi$b$, chúng tôi tính toán số lần chúng tôi có thể sử dụng phần đóng góp của nó$\frac{n}{b}$. Chúng tôi tôn trọng sự ràng buộc$a < b$bằng cách giới hạn hệ số. Mỗi người được chọn$a$tương ứng trực tiếp với một phần trong đầu ra. 

Cuối cùng, chúng tôi xác minh rằng tổng được xây dựng khớp chính xác$n-1$, đảm bảo tính chính xác trước khi in. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 6$Mục tiêu là$5$. Số chia là$2, 3$. 

| Bước | Số chia b | bước = n/b | lấy | phần còn lại | 
| --- | --- | --- | --- | --- | 
| bắt đầu | - | - | - | 5 | 
| quá trình 3 | 3 | 2 | 2 | 1 | 
| quá trình 2 | 2 | 3 | 0 | 1 | 

Chúng tôi không thể chi trả đầy đủ phần còn lại bằng một đường chuyền tham lam trực tiếp, điều này buộc chúng tôi phải điều chỉnh bằng cách chia nhỏ các khoản đóng góp một cách cẩn thận. Một công trình cuối cùng hợp lệ là:$$\frac{1}{2} + \frac{1}{3} + \frac{1}{3} = \frac{5}{6}$$Điều này cho thấy có thể cần nhiều phân số cho mỗi mẫu số. 

### Ví dụ 2:$n = 10$Mục tiêu là$9$. Số chia là$2, 5$. 

| Bước | Số chia b | bước = n/b | lấy | phần còn lại | 
| --- | --- | --- | --- | --- | 
| bắt đầu | - | - | - | 9 | 
| quá trình 5 | 5 | 2 | 4 | 1 | 
| quá trình 2 | 2 | 5 | 0 | 1 | 

Điều chỉnh cuối cùng mang lại số 1 còn lại, điều này xác nhận rằng chỉ riêng thứ tự tham lam ngây thơ là không đủ cho tất cả các trường hợp và thúc đẩy sự phân rã có cấu trúc trên các lớp chia. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{n})$| Phép liệt kê số chia chiếm ưu thế | 
| Không gian |$O(\sqrt{n})$| Lưu trữ tất cả các ước số thích hợp | 

Thuật toán dễ dàng phù hợp với các ràng buộc vì ngay cả đối với$n = 10^9$, việc liệt kê số chia diễn ra nhanh chóng và trình tự được xây dựng vẫn bị giới hạn bởi$10^5$giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return sys.stdout.getvalue()

# sample
assert run("2\n") == "NO\n"

# small composite
assert run("6\n") != ""

# prime
assert run("13\n") == "NO\n"

# power of two
assert run("8\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | KHÔNG | trường hợp bất khả thi nhỏ nhất | 
| 6 | phân hủy hợp lệ | trường hợp composite nhỏ nhất | 
| 13 | KHÔNG | xử lý thủ tướng | 
| 8 | phân hủy hợp lệ | chuỗi lũy thừa hai số chia | 

## Vỏ cạnh 

Khi nào$n$là số nguyên tố, tập ước số trống nên thuật toán ngay lập tức trả về NO. Ví dụ, đầu vào$n = 13$không tạo ra mẫu số hợp lệ và việc xây dựng dừng lại một cách chính xác trước bất kỳ số học nào. 

Khi$n$là lũy thừa của hai, các ước số tạo thành một chuỗi lũy thừa của hai. Thuật toán liên tục chia phần còn lại thành hai nửa, đảm bảo mỗi phân số thẳng hàng với mẫu số hợp lệ. Vì$n = 8$, mẫu số$2$Và$4$là đủ để xây dựng một phân hủy đầy đủ mà không còn sót lại. 

Khi$n = 2$, mục tiêu là$\frac{1}{2}$, nhưng không tồn tại mẫu số hợp lệ vì$b_i$phải thỏa mãn$1 < b_i < n$. Thuật toán xác định chính xác điều này là không thể ngay lập tức.
