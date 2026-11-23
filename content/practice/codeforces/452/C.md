---
title: "CF 452C - Trò ảo thuật"
description: "Có $m$ bộ bài hoàn chỉnh, mỗi bộ chứa cùng giá trị lá bài $n$. Sau khi trộn tất cả các bộ bài lại với nhau, tổng cộng có $mn$ thẻ vật lý và mọi giá trị xuất hiện chính xác $m$ lần."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 452
codeforces_index: "C"
codeforces_contest_name: "MemSQL Start[c]UP 2.0 - Round 1"
rating: 2100
weight: 452
solve_time_s: 104
verified: true
draft: false
---

[CF 452C - Trò ảo thuật](https://codeforces.com/problemset/problem/452/C) 

**Xếp hạng:** 2100 
**Tags:** tổ hợp, toán, xác suất 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

có$m$bộ bài hoàn chỉnh, mỗi bộ bài đều chứa những thông tin giống nhau$n$giá trị thẻ. Sau khi trộn tất cả các bộ bài lại với nhau, sẽ có$mn$tổng số thẻ vật lý và mọi giá trị xuất hiện chính xác$m$lần. 

Alex lấy ngẫu nhiên$n$các lá bài từ đống lớn này và sử dụng chúng làm bộ bài để thực hiện trò lừa. 

Sau đó, khán giả chọn một lá bài ngẫu nhiên từ bộ bài đó, ghi nhớ giá trị của nó và đặt nó trở lại. Alex xáo trộn và rút một lá bài ngẫu nhiên. Giả sử Alex không thực hiện bất kỳ phép thuật nào và chỉ rút ngẫu nhiên một cách thống nhất, chúng ta phải tính xác suất để lá bài được rút có cùng giá trị với lá bài đã nhớ. 

Đầu vào bao gồm$n$Và$m$. Đầu ra là một số dấu phẩy động duy nhất, xác suất thủ thuật thành công. 

Những hạn chế là nhỏ, chỉ lên đến$1000$, nhưng phần thú vị là tìm xác suất bằng phương pháp phân tích. Bất kỳ cách tiếp cận nào cố gắng liệt kê tất cả những gì có thể$n$-không thể lựa chọn thẻ vì số lượng lựa chọn như vậy là$$\binom{mn}{n},$$trở nên lớn về mặt thiên văn ngay cả đối với các giá trị vừa phải. 

Một vài trường hợp đáng chú ý. 

Khi$n=1$, bộ bài luôn có đúng một lá bài. Khán giả chọn lá bài đó và Alex phải rút lá bài giống nhau nên câu trả lời luôn là$1$. Ví dụ:```
1 5
```Đầu ra đúng là:```
1
```Công thức chứa hệ số$mn-1$ở mẫu số sẽ chia cho 0 khi$n=m=1$, nên trường hợp đó phải được xử lý riêng. 

Một sai lầm dễ mắc phải khác là cho rằng mỗi giá trị xuất hiện nhiều nhất một lần trong bảng làm việc. Ví dụ:```
2 2
```Bộ bài được chọn có thể$\{A,A\}$, điều đó làm nên sự thành công chắc chắn. Bỏ qua các bản sao sẽ tạo ra xác suất sai. 

## Phương pháp tiếp cận 

Quan điểm bạo lực rất hữu ích vì nó tiết lộ số lượng chúng ta thực sự cần. 

Giả sử một bộ bài được chọn cụ thể có chứa số lượng$$c_1,c_2,\dots,c_n,$$Ở đâu$c_i$là số bản sao của giá trị$i$, Và$$c_1+c_2+\cdots+c_n=n.$$Người xem chọn giá trị$i$với xác suất$c_i/n$. Sau khi thay thế, Alex rút ra giá trị$i$với xác suất$c_i/n$. Đối với bộ bài cố định này, xác suất thành công là$$\sum_{i=1}^{n}\frac{c_i}{n}\cdot\frac{c_i}{n} = \frac{1}{n^2}\sum_{i=1}^{n} c_i^2.$$Một giải pháp brute-force sẽ liệt kê mọi bộ bài được chọn có thể, tính toán số lượng này và tính trung bình cho tất cả các lựa chọn. Số lựa chọn là$\binom{mn}{n}$, điều đó hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta chỉ cần giá trị kỳ vọng của$$\sum c_i^2.$$Tất cả các giá trị thẻ đều đối xứng. Cho phép$X$là số lượng bản sao của một giá trị cụ thể trong bộ bài đã chọn. 

Sau đó$$E\!\left[\sum_{i=1}^{n} c_i^2\right] = nE[X^2].$$Vì vậy, câu trả lời trở thành$$\frac{E[X^2]}{n}.$$Hiện nay$X$tuân theo một phân phối siêu hình học. 

có$mn$tổng số thẻ. 

Chính xác$m$trong số chúng có giá trị được chọn. 

Chúng tôi vẽ$n$thẻ. 

Như vậy$$X \sim \text{Hypergeometric}(N=mn,\ K=m,\ n).$$Đối với một biến ngẫu nhiên siêu hình học,$$E[X] = \frac{nK}{N},$$Và$$\mathrm{Var}(X) = n\frac{K}{N} \left(1-\frac{K}{N}\right) \frac{N-n}{N-1}.$$Thay thế$N=mn$Và$K=m$,$$E[X]=1,$$Và$$\mathrm{Var}(X) = \frac{(n-1)(m-1)}{mn-1}.$$Từ$$E[X^2] = \mathrm{Var}(X)+E[X]^2,$$chúng tôi có được$$E[X^2] = 1+\frac{(n-1)(m-1)}{mn-1}.$$Kể từ đây$$P = \frac{1}{n} \left( 1+\frac{(n-1)(m-1)}{mn-1} \right).$$Điều này đã đủ để thực hiện. 

### So sánh phương pháp tiếp cận 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê Brute Force của tất cả các bộ bài được chọn | số mũ trong$n$| Hàm mũ | Quá chậm | 
| Công thức kỳ vọng siêu hình học |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$Và$m$. 
2. Xử lý trường hợp đặc biệt$n=m=1$. 

Trong tình huống này chỉ có một lá bài tồn tại nên xác suất thành công là chính xác$1$. 
3. Tính toán$$\mathrm{Var}(X) = \frac{(n-1)(m-1)}{mn-1}.$$Đây là phương sai của số lượng bản sao của một giá trị cố định xuất hiện trong bộ bài đã chọn. 

1. Tính toán$$E[X^2] = 1+\mathrm{Var}(X).$$Ý nghĩa của$X$bằng$1$, Vì thế$E[X]^2=1$. 

1. Tính toán$$P=\frac{E[X^2]}{n}.$$Theo tính đối xứng, giá trị kỳ vọng của$\sum c_i^2$bằng$nE[X^2]$. 

1. In kết quả dưới dạng số dấu phẩy động. 

### Tại sao nó hoạt động 

Đối với một bộ bài được chọn cố định, xác suất thành công bằng$$\frac{1}{n^2}\sum c_i^2.$$Câu trả lời tổng thể là sự kỳ vọng về số lượng này đối với tất cả các lựa chọn bộ bài ngẫu nhiên. Tính tuyến tính của kỳ vọng cho phép chúng ta thay thế tổng của$n$thuật ngữ đối xứng bởi$nE[X^2]$, Ở đâu$X$đếm các bản sao của một giá trị duy nhất. Từ$X$là siêu hình học, giá trị trung bình và phương sai của nó được biết chính xác. Việc thay thế các công thức đó sẽ mang lại xác suất thành công chính xác như mong đợi. Mỗi bước đều bằng nhau nên thuật toán sẽ tính xác suất thực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    if n == 1 and m == 1:
        print(1.0)
        return

    var = (n - 1) * (m - 1) / (n * m - 1)
    ans = (1.0 + var) / n

    print(ans)

solve()
```Việc thực hiện tuân theo đạo hàm trực tiếp. 

Cách xử lý đặc biệt duy nhất là$(n,m)=(1,1)$. Phương sai dạng đóng chứa yếu tố$mn-1$ở mẫu số, nó trở thành số 0 ở đó. Có lẽ câu trả lời là tầm thường$1$bởi vì chỉ có một thẻ tồn tại. 

Tất cả số học được thực hiện trong dấu phẩy động. Độ chính xác yêu cầu chỉ$10^{-6}$, vậy là Python tiêu chuẩn`float`số học là quá đủ. 

Không cần vòng lặp vì toàn bộ giải pháp giảm xuống việc đánh giá một công thức có kích thước không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
```| Biến | Giá trị | 
| --- | --- | 
|$n$| 2 | 
|$m$| 2 | 
|$mn-1$| 3 | 
|$(n-1)(m-1)$| 1 | 
| Phương sai |$1/3$| 
|$E[X^2]$|$4/3$| 
| Trả lời |$2/3$| 

Đầu ra:```
0.6666666666666666
```Đây là mẫu từ tuyên bố. Các giá trị trùng lặp có thể xuất hiện trong bộ bài đã chọn, làm tăng xác suất lên trên$1/2$. 

### Ví dụ 2 

đầu vào:```
3 1
```| Biến | Giá trị | 
| --- | --- | 
|$n$| 3 | 
|$m$| 1 | 
|$mn-1$| 2 | 
|$(n-1)(m-1)$| 0 | 
| Phương sai | 0 | 
|$E[X^2]$| 1 | 
| Trả lời |$1/3$| 

Đầu ra:```
0.3333333333333333
```Chỉ với một bản sao của mỗi giá trị, bộ bài được chọn luôn là bộ bài bình thường chứa ba lá bài riêng biệt. Xác suất trùng với thẻ đã nhớ là chính xác$1/3$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ một số phép tính số học được thực hiện | 
| Không gian |$O(1)$| Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các hạn chế là rất nhỏ so với những gì giải pháp này có thể xử lý. Vì tính toán là thời gian không đổi và bộ nhớ không đổi nên nó dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n, m = map(int, input().split())

    if n == 1 and m == 1:
        return "1.0"

    var = (n - 1) * (m - 1) / (n * m - 1)
    ans = (1.0 + var) / n
    return str(ans)

# provided sample
assert abs(float(run("2 2\n")) - 0.6666666666666666) < 1e-9, "sample 1"

# minimum size
assert abs(float(run("1 1\n")) - 1.0) < 1e-9, "single card"

# one deck only
assert abs(float(run("3 1\n")) - (1.0 / 3.0)) < 1e-9, "all values distinct"

# one card chosen
assert abs(float(run("1 1000\n")) - 1.0) < 1e-9, "deck size one"

# large boundary
expected = (1.0 + (999 * 999) / (1000000 - 1)) / 1000
assert abs(float(run("1000 1000\n")) - expected) < 1e-9, "maximum bounds"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2`|`0.6666666666666666`| Trường hợp mẫu | 
|`1 1`|`1.0`| Trường hợp đặc biệt chia cho 0 | 
|`3 1`|`0.3333333333333333`| Không thể có giá trị trùng lặp | 
|`1 1000`|`1.0`| Bộ bài làm việc một thẻ | 
|`1000 1000`| Giá trị công thức | Hạn chế tối đa | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1 1
```Chỉ có một thẻ tồn tại. Khán giả chọn lá bài đó và Alex rút lá bài tương tự. Thuật toán ngay lập tức trở lại`1.0`trước khi đánh giá công thức phương sai, tránh chia cho 0 và đưa ra câu trả lời đúng. 

Coi như:```
3 1
```Mỗi giá trị tồn tại chính xác một lần. Bộ bài được chọn luôn là bộ hoàn chỉnh gồm ba lá bài riêng biệt. Thuật toán tính toán$$\mathrm{Var}(X)=0,$$bởi vì một giá trị cố định xuất hiện chính xác một lần một cách chắc chắn. Sau đó$$P=\frac{1}{3}.$$Điều này phù hợp với xác suất trực quan của việc rút thẻ nhớ từ ba thẻ riêng biệt. 

Coi như:```
1 5
```Bộ bài làm việc chứa chính xác một lá bài, mặc dù có năm bản sao của giá trị đó tồn tại trong một chồng bài lớn. Thuật toán tính toán$$\mathrm{Var}(X)=0, \qquad P=1.$$Khán giả và Alex buộc phải tương tác với cùng một thẻ, do đó thành công được đảm bảo. 

Cuối cùng, hãy xem xét trường hợp trùng lặp quan trọng:```
2 2
```Một giải pháp bất cẩn có thể cho rằng hai giá trị riêng biệt luôn được chọn và trả lời$1/2$. Phép tính siêu hình học tính toán chính xác cho các lựa chọn như$\{A,A\}$, làm tăng xác suất. Câu trả lời kết quả là$$\frac{2}{3},$$phù hợp với đầu ra mẫu.
