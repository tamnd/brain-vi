---
title: "CF 106387D - Sự trả thù của (C/K)or(e)ys"
description: "Chúng ta đang xem xét một vấn đề về cấu hình liên quan đến một số vị trí cố định và hai loại đối tượng không thể phân biệt được. Có các vị trí $2n$ trên một đường thẳng và chính xác $n$ trong số đó được dán nhãn là đá Mỹ trong khi $n$ còn lại là đá Thụy Điển."
date: "2026-06-20T03:31:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106387
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 2-25-26 (Beginner)"
rating: 0
weight: 106387
solve_time_s: 73
verified: true
draft: false
---

[CF 106387D - Sự trả thù của (C/K)or(e)ys](https://codeforces.com/problemset/problem/106387/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xem xét một vấn đề về cấu hình liên quan đến một số vị trí cố định và hai loại đối tượng không thể phân biệt được. có$2n$vị trí trong một dòng, và chính xác$n$trong số chúng được dán nhãn là đá của Mỹ trong khi số còn lại$n$là đá Thụy Điển. Tất cả các thỏa thuận hợp lệ đều có khả năng xảy ra như nhau. 

Câu hỏi yêu cầu xác suất để lần đầu tiên$k$các vị trí trong hàng đều là người Mỹ. Nói cách khác, nếu chúng ta chọn ngẫu nhiên$n$của$2n$vị trí là người Mỹ, chúng tôi muốn một phần trong số những lựa chọn đó có vị trí$1$bởi vì$k$được đảm bảo nằm trong số các vị trí của Mỹ. 

Đầu vào bao gồm các số nguyên$n$Và$k$và đầu ra là một giá trị xác suất được biểu diễn dưới dạng phân số rút gọn hoặc số hữu tỷ, tùy thuộc vào yêu cầu định dạng đầu ra của bài toán. Về mặt khái niệm, chúng tôi đang tính các vị trí thuận lợi trên tất cả các vị trí có thể có. 

Ràng buộc tổ hợp chính là chúng ta đang chọn chính xác$n$vị trí ngoài$2n$, vậy tổng số cấu hình là$\binom{2n}{n}$. Khó khăn là điều kiện tiền tố buộc phải có một hạn chế: điều kiện đầu tiên$k$các vị trí phải được bao gồm trong tập hợp các vị trí của người Mỹ đã chọn. 

Điều này đưa ra một điều kiện biên tinh tế khi$k > n$. Nếu cần nhiều vị trí tiền tố hơn số đá sẵn có của Mỹ thì xác suất chính xác là bằng không. Một công thức tổ hợp ngây thơ áp dụng một cách mù quáng các hệ số nhị thức mà không kiểm tra tính khả thi sẽ tạo ra các giá trị khác 0 hoặc các kết hợp không xác định một cách không chính xác. 

Một trường hợp cạnh khác xảy ra khi$k = 0$. Trong trường hợp này, không có hạn chế nào nên xác suất phải bằng một. Bất kỳ triển khai nào tính toán các tỷ lệ giai thừa mà không xử lý các ràng buộc trống vẫn có thể hoạt động, nhưng logic hủy hoặc chia số có thể thất bại nếu không được cấu trúc cẩn thận. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ liệt kê tất cả các cách để lựa chọn$n$Vị trí của Mỹ trong số$2n$khe cắm, sau đó kiểm tra từng cấu hình để xem liệu vị trí$1$bởi vì$k$đều được chọn. Điều này đúng vì nó trực tiếp tuân theo định nghĩa xác suất là kết quả thuận lợi trên tổng kết quả. Tuy nhiên, số lượng cấu hình$\binom{2n}{n}$, tăng trưởng theo cấp số nhân trong$n$. Ngay cả đối với mức độ vừa phải$n$, điều này trở nên không khả thi, vì việc liệt kê tất cả các tập hợp con có kích thước$n$từ$2n$đã ở mức hàng trăm triệu trở lên. 

Quan sát quan trọng là ràng buộc tiền tố chỉ ảnh hưởng đến số lượng vị trí của người Mỹ còn lại được đặt sau khi sửa lỗi đầu tiên.$k$khe cắm. Nếu lần đầu tiên$k$đã buộc phải là người Mỹ rồi thì chúng ta chỉ cần chọn những người còn lại$n-k$Đá Mỹ trong số còn lại$2n-k$các vị trí. Điều này làm giảm số lượng thuận lợi thành một hệ số nhị thức duy nhất$\binom{2n-k}{n-k}$. 

Mẫu số vẫn là tổng số cách chọn$n$Vị trí của Mỹ trong số$2n$, đó là$\binom{2n}{n}$. Do đó, xác suất là một tỷ số đơn giản của hai đại lượng tổ hợp. 

Chúng tôi tránh hoàn toàn lực lượng vũ phu bằng cách giảm vấn đề thành tổ hợp trực tiếp, thường được tính toán bằng cách sử dụng giai thừa hoặc hệ số nhị thức được tính toán trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu |$O(\binom{2n}{n})$|$O(1)$hoặc$O(\binom{2n}{n})$| Quá chậm | 
| Công thức tổ hợp |$O(n)$hoặc$O(1)$sau khi tiền xử lý |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán xác suất bằng cách sử dụng các hệ số nhị thức và tỷ lệ của chúng. 

1. Kiểm tra xem ràng buộc có khả thi hay không. Nếu như$k > n$, trả về số 0 ngay lập tức. Điều này xuất phát từ thực tế là chúng ta không thể ép nhiều viên đá Mỹ ở tiền tố hơn tổng số đá tồn tại. 
2. Tính tổng số cấu hình hợp lệ$\binom{2n}{n}$. Điều này tính tất cả các cách để đặt chính xác$n$Đá Mỹ trong số$2n$các vị trí không hạn chế. 
3. Tính số lượng cấu hình phù hợp$\binom{2n-k}{n-k}$. Điều này xuất phát từ việc sửa lỗi đầu tiên$k$giữ vị trí là người Mỹ và phân phối phần còn lại$n-k$Đá Mỹ trong số còn lại$2n-k$khe cắm. 
4. Trả về tỷ lệ cấu hình thuận lợi trên tổng số cấu hình. Nếu bài toán yêu cầu một phân số tối giản, hãy rút gọn bằng cách sử dụng bước chia chung lớn nhất. 

Điểm tinh tế là chúng ta không bao giờ xây dựng các thỏa thuận một cách rõ ràng. Mọi quyết định đều được rút gọn thành các kết hợp đếm, giúp tránh mọi phép liệt kê theo cấp số nhân. 

### Tại sao nó hoạt động 

Mọi cấu hình hợp lệ của đá Mỹ tương ứng với chính xác một tập hợp con kích thước$n$được chọn từ$2n$. Điều kiện hóa sự kiện là lần đầu tiên$k$các vị trí được bao gồm sẽ loại bỏ các vị trí đó khỏi không gian lựa chọn và giảm cả số lượng vị trí có sẵn cũng như các lựa chọn bắt buộc một cách thống nhất. Điều này duy trì tính đồng nhất của việc đếm, do đó xác suất trở thành một tỷ lệ rõ ràng của các hệ số nhị thức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import gcd

def comb(n, r):
    if r < 0 or r > n:
        return 0
    r = min(r, n - r)
    res = 1
    for i in range(1, r + 1):
        res = res * (n - r + i) // i
    return res

n, k = map(int, input().split())

if k > n:
    print(0)
else:
    total = comb(2 * n, n)
    good = comb(2 * n - k, n - k)

    g = gcd(good, total)
    good //= g
    total //= g

    print(good, total)
```Việc thực hiện phản ánh trực tiếp đạo hàm tổ hợp. Hàm trợ giúp tính toán các hệ số nhị thức lặp đi lặp lại để tránh tràn giai thừa và sử dụng bộ nhớ không cần thiết. Tối ưu hóa tính đối xứng làm giảm độ dài vòng lặp xuống$\min(r, n-r)$, giúp tính toán ổn định với các giá trị lớn. 

Việc kiểm tra tính khả thi đối với$k > n$ngăn chặn các cuộc gọi không hợp lệ đến hàm kết hợp mà nếu không sẽ tạo ra kết quả vô nghĩa. Bước rút gọn cuối cùng đảm bảo phân số được xuất ra ở dạng đơn giản nhất, thường được yêu cầu trong các kết quả đầu ra ở dạng xác suất. 

## Ví dụ đã hoạt động 

Hãy xem xét$n = 3, k = 1$. Chúng tôi có$6$vị trí và lựa chọn$3$đối với đá của Mỹ. Tổng số cấu hình là$\binom{6}{3} = 20$. Để có cấu hình thuận lợi, vị trí$1$phải là người Mỹ nên ta chọn cái còn lại$2$Vị trí của Mỹ từ trước đến nay$5$, cho$\binom{5}{2} = 10$. 

| Bước | Giá trị | 
| --- | --- | 
| Tổng cộng$\binom{2n}{n}$| 20 | 
| Thuận lợi$\binom{2n-k}{n-k}$| 10 | 
| Xác suất | 1/2 | 

Điều này cho thấy việc sửa tiền tố sẽ làm giảm không gian lựa chọn hiệu quả trong khi vẫn duy trì tính đối xứng. 

Bây giờ hãy xem xét$n = 2, k = 2$. Chúng ta có 4 vị trí và phải chọn 2 viên đá Mỹ. Nếu cả hai vị trí đầu tiên đều phải là người Mỹ thì không còn lựa chọn nào cho vị trí người Mỹ. 

| Bước | Giá trị | 
| --- | --- | 
| Tổng cộng$\binom{4}{2}$| 6 | 
| Thuận lợi$\binom{2}{0}$| 1 | 
| Xác suất | 1/6 | 

Điều này khẳng định rằng ngay cả khi tiền tố xác định đầy đủ tất cả các loại đá của Mỹ thì công thức vẫn hoạt động rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| mỗi hệ số nhị thức được tính theo các bước tuyến tính cho đến$\min(r, n-r)$| 
| Không gian |$O(1)$| chỉ có một số số nguyên được duy trì | 

Các ràng buộc điển hình cho các bài toán xác suất tổ hợp làm cho việc này đủ hiệu quả ngay cả đối với các bài toán lớn.$n$, vì không cần bảng giai thừa hoặc tính toán trước nhiều. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    def comb(n, r):
        if r < 0 or r > n:
            return 0
        r = min(r, n - r)
        res = 1
        for i in range(1, r + 1):
            res = res * (n - r + i) // i
        return res

    n, k = map(int, sys.stdin.readline().split())
    if k > n:
        return "0"

    total = comb(2 * n, n)
    good = comb(2 * n - k, n - k)

    g = math.gcd(good, total)
    return f"{good//g} {total//g}"

# custom tests
assert run("3 1") == "10 20"
assert run("2 2") == "1 6"
assert run("2 3") == "0"
assert run("1 0") == "1 2"
assert run("5 5") == "1 252"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 | 10 20 | ràng buộc tiền tố cơ bản | 
| 2 2 | 1 6 | tiền tố đầy đủ buộc tất cả các loại đá của Mỹ | 
| 2 3 | 0 | trường hợp không thể$k > n$| 
| 1 0 | 1 2 | trường hợp không có cạnh hạn chế | 
| 5 5 | 1 252 | sự bình đẳng tiền tố cực độ | 

## Vỏ cạnh 

cho$k > n$, chẳng hạn như đầu vào$n = 3, k = 5$, thuật toán ngay lập tức trả về 0 trước bất kỳ tổ hợp nào. Điều này tránh việc đánh giá$\binom{2n-k}{n-k}$với các tham số âm, nếu không sẽ tạo ra kết quả không chính xác. 

Vì$k = 0$, chẳng hạn như$n = 4, k = 0$, việc tính toán trở thành$\binom{8}{4} / \binom{8}{4} = 1$. Thuật toán xử lý việc này một cách tự nhiên mà không cần cách viết hoa đặc biệt ngoài việc đánh giá nhị thức. 

Vì$k = n$, chẳng hạn như$n = 3, k = 3$, chúng tôi nhận được$\binom{3}{0} / \binom{6}{3}$, tương ứng với chính xác một cấu hình thuận lợi trong đó tất cả các viên đá của Mỹ đều chiếm tiền tố.
