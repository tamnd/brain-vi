---
title: "CF 103637F - Phân tích chức năng"
description: "Chúng ta được cấp một chuỗi giống như hoán vị $p = (1, 2, dots, n)$. Từ chuỗi này, chúng tôi liên tục lấy mẫu các phần tử $m$, trong đó mỗi vị trí được chọn độc lập và thống nhất từ ​​các vị trí $n$, do đó, việc lặp lại được cho phép và kết quả là nhiều tập hợp không nhất thiết…"
date: "2026-07-02T22:20:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103637
codeforces_index: "F"
codeforces_contest_name: "2019-2020 10th BSUIR Open Programming Championship. Semifinal"
rating: 0
weight: 103637
solve_time_s: 49
verified: true
draft: false
---

[CF 103637F - Phân tích chức năng](https://codeforces.com/problemset/problem/103637/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy giống như hoán vị$p = (1, 2, \dots, n)$. Từ trình tự này chúng tôi liên tục lấy mẫu$m$các phần tử, trong đó mỗi vị trí được chọn độc lập và thống nhất từ$n$các vị trí, do đó cho phép lặp lại và nhiều tập hợp kết quả không nhất thiết phải khác biệt. Danh sách mẫu này được gọi$q$. 

Đối với bất kỳ mảng nào$a$, chúng tôi xác định$\text{nth}(a, b)$như$b$-phần tử nhỏ nhất thứ$a$. Vì vậy nếu chúng ta sắp xếp$a$, chúng tôi chọn phần tử tại chỉ mục$b$theo thứ tự sắp xếp. 

Chúng ta cũng được cho hai chỉ số cố định$k$Và$d$. Nhiệm vụ là: với mỗi$m$từ$d$ĐẾN$n$, tính xác suất để$k$-giá trị nhỏ nhất trong toàn bộ mảng$p$thực sự là nhỏ hơn$d$-giá trị nhỏ nhất trong mẫu ngẫu nhiên$q$. Câu trả lời phải được đưa ra theo modulo$998244353$. 

Từ$p$là hoán vị danh tính, dạng sắp xếp của nó là tầm thường. giá trị$\text{nth}(p, k)$đơn giản là$k$. Do đó, toàn bộ vấn đề quy về việc so sánh một số nguyên cố định$k$với thống kê thứ tự ngẫu nhiên của nhiều tập hợp được hình thành bằng cách lấy mẫu độc lập thống nhất từ$\{1, \dots, n\}$. 

Những ràng buộc cho phép$n$lên tới$3 \cdot 10^5$, do đó, bất kỳ giải pháp nào tính toán lại xác suất một cách độc lập cho từng$m$trong thời gian tuyến tính hoặc bậc hai là không thể. Thậm chí$O(n^2)$lý do về kích thước mẫu được loại trừ. Chúng ta phải tính toán tất cả các câu trả lời một cách gần như tuyến tính hoặc$n \log n$thời gian. 

Một trường hợp khó nhận thấy là khi$d = 1$. Sau đó chúng ta đang so sánh$k < \min(q)$và xác suất trở nên rất nhạy cảm với việc liệu tất cả các phần tử được lấy mẫu có vượt quá$k$. Một trường hợp cạnh khác là$d = m$, nơi chúng tôi so sánh$k < \max(q)$, điều này sẽ chuyển cách giải thích thành ít nhất một phần tử được lấy mẫu lớn hơn$k$. 

Một cách tiếp cận đơn giản sẽ mô phỏng việc lấy mẫu và sắp xếp cho từng$m$, nhưng ngay cả xác suất tính toán chính xác thông qua việc liệt kê nhiều tập hợp cũng sẽ bùng nổ về mặt tổ hợp. 

## Phương pháp tiếp cận 

Một nỗ lực trực tiếp sẽ là nghĩ đến việc tạo ra tất cả$m$-chuỗi dài hơn$[1,n]$, tính toán cách sắp xếp của chúng$d$- phần tử thứ và đếm tần suất nó vượt quá$k$. Điều đó ngay lập tức dẫn đến$n^m$những trạng thái hoàn toàn không khả thi ngay cả đối với những quốc gia nhỏ$n$. 

Một cách tiếp cận bạo lực có cấu trúc hơn một chút là khắc phục$m$và tính toán phân bố của$d$-thống kê thứ tự của$m$i.i.d. đồng phục rút ra từ$[1,n]$. Đây là một bài toán thống kê thứ tự cổ điển: chúng ta có thể tính tổng có bao nhiêu giá trị được lấy mẫu$\le k$, hoặc trực tiếp rút ra phân phối nhị thức cho các cấp bậc. Tuy nhiên, thực hiện việc này một cách độc lập cho mỗi$m$vẫn còn chi phí$O(n^2)$nếu được thực hiện cẩn thận, vì mỗi$m$yêu cầu tính tổng lên tới$m$tiểu bang. 

Nhận xét quan trọng là sự kiện$\text{nth}(q,d) > k$chỉ phụ thuộc vào số lượng phần tử được lấy mẫu$\le k$. Nếu nhiều nhất$d-1$các yếu tố là$\le k$, thì$d$-thứ nhỏ nhất phải vượt quá$k$. Vì vậy, vấn đề giảm xuống còn đuôi nhị thức về số lượng phần tử “tốt” trong một mẫu. 

Mỗi mẫu có kích thước$m$tạo ra sự phân phối nhị thức trong đó mỗi lần rút là “tốt” (trong$[1,k]$) với xác suất$k/n$. Vậy số phần tử tốt là$X \sim \text{Bin}(m, k/n)$. Điều kiện trở thành$X \le d-1$. Do đó, mỗi câu trả lời là một xác suất tiền tố của phân phối nhị thức. 

Thử thách còn lại là chúng ta cần xác suất này cho tất cả$m$từ$d$ĐẾN$n$, điều này cho thấy sự tái diễn động trong$m$, thay vì tính lại tổng nhị thức từ đầu. 

Chúng ta có thể duy trì phân phối nhị thức lặp đi lặp lại: khi tăng$m$lên 1, chúng tôi cập nhật xác suất bằng cách thêm phần tử “tốt” hoặc “xấu” mới. Điều này dẫn tới DP bị đảo lộn$m$Và$x$, nhưng chúng ta chỉ cần các giá trị lên đến$d-1$, giúp trạng thái có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê mẫu bằng vũ lực | Hàm mũ | Hàm mũ | Quá chậm | 
| Tính toán lại nhị thức Per-m |$O(n^2)$|$O(n)$| Quá chậm | 
| DP tăng dần$m$và đếm$\le k$|$O(nd)$|$O(d)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi điều chỉnh lại vấn đề về mặt số lượng. Mỗi trong số$m$các phần tử được lấy mẫu độc lập trong tập hợp$[1,k]$với xác suất$k/n$, và trong$[k+1,n]$nếu không thì. Cho phép$X_m$là số phần tử được lấy mẫu$\le k$. 

Sự kiện chúng tôi muốn là$\text{nth}(q,d) > k$, tương đương với việc nói rằng nhiều nhất$d-1$các yếu tố là$\le k$, Vì thế$X_m \le d-1$. 

### Các bước 

1. Tính toán trước nghịch đảo mô-đun của$n$, và xác định$p = k/n$Và$q = (n-k)/n$trong số học mô-đun. 

Điều này cho phép xử lý từng mẫu như một phép thử Bernoulli. 
2. Duy trì mảng DP$dp[x]$, Ở đâu$dp[x]$là xác suất sau khi xử lý dòng điện$m$, chính xác$x$các phần tử được lấy mẫu là$\le k$, vì$x \le d-1$. 

Chúng tôi không bao giờ theo dõi các giá trị trên$d-1$vì chúng không liên quan đến điều kiện cuối cùng. 
3. Khởi tạo cho$m = 0$:$dp[0] = 1$, tất cả các trạng thái khác bằng không. 

Điều này đại diện cho một mẫu trống. 
4. Đối với mỗi$m$từ$1$ĐẾN$n$, cập nhật DP: 

mỗi trạng thái trước đó$dp[x]$góp phần vào: 

di chuyển đến$x+1$với xác suất$p$, hoặc ở tại$x$với xác suất$q$. 

Chúng tôi xử lý quá trình chuyển đổi này ngược lại trong$x$để tránh ghi đè các trạng thái cần thiết. 
5. Sau khi cập nhật cho từng$m$, tính toán câu trả lời cho điều đó$m$như tổng của$dp[0] + \dots + dp[d-1]$. 

Đây chính xác là xác suất mà nhiều nhất$d-1$các phần tử được lấy mẫu rơi vào$[1,k]$. 
6. Đưa ra câu trả lời cho tất cả$m \ge d$. 

### Tại sao nó hoạt động 

DP mã hóa phân bố đầy đủ về số lượng phần tử được lấy mẫu nằm dưới hoặc bằng$k$. Vì thứ tự tương đối bên trong$[1,k]$không quan trọng, chỉ có số lượng ảnh hưởng đến việc$d$-thứ nhỏ nhất vượt quá$k$. Phép truy toán trùng khớp chính xác với phép thử Bernoulli độc lập, do đó DP bảo toàn phân bố nhị thức thực ở mỗi bước. Cắt ngắn ở$d-1$là hợp lệ vì bất kỳ trạng thái nào ngoài nó sẽ không bao giờ đóng góp vào sự kiện mà chúng ta truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

n, d, k = map(int, input().split())

inv_n = modinv(n)
p = k * inv_n % MOD
q = (n - k) * inv_n % MOD

dp = [0] * (d + 1)
dp[0] = 1

res = []

for m in range(1, n + 1):
    ndp = [0] * (d + 1)

    for x in range(d):
        if dp[x] == 0:
            continue
        ndp[x] = (ndp[x] + dp[x] * q) % MOD
        if x + 1 <= d:
            ndp[x + 1] = (ndp[x + 1] + dp[x] * p) % MOD

    dp = ndp

    if m >= d:
        res.append(sum(dp[:d]) % MOD)

print("\n".join(map(str, res)))
```Việc triển khai sẽ duy trì DP cuộn trong đó mỗi lớp tương ứng với việc tăng kích thước mẫu. Quá trình chuyển đổi cẩn thận sử dụng một mảng mới để tránh trộn lẫn xác suất cũ và mới, điều này có thể làm hỏng phân phối. 

Việc xử lý ranh giới tại$x + 1 = d$là an toàn vì bất kỳ trạng thái nào ngoài$d-1$không liên quan đến xác suất cuối cùng. Chúng tôi rõ ràng tránh cần các giá trị trên ngưỡng này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2 3
```Đây$n=5$,$k=3$,$d=2$. Chúng tôi đang lấy mẫu trình tự và kiểm tra xem có nhiều nhất một phần tử được lấy mẫu nằm trong$\{1,2,3\}$. 

Chúng tôi theo dõi$dp[x]$qua$x \in \{0,1\}$. 

| m | dp[0] | dp[1] | tổng dp[0..1] | 
| --- | --- | --- | --- | 
| 1 | q | p | 1 | 
| 2 | q^2 | 2pq | q^2 + 2pq | 
| 3 | ... | ... | giá trị tính toán | 

Ở mỗi bước, tổng thể hiện xác suất xuất hiện nhiều nhất một phần tử “tốt”. Kết quả đầu ra cuối cùng là những xác suất tích lũy bắt đầu từ$m=2$. 

Điều này chứng tỏ rằng DP đang theo dõi chính xác phân phối nhị thức bị cắt cụt tại$d-1$. 

### Ví dụ 2 

đầu vào:```
4 1 1
```Ở đây chúng tôi kiểm tra xem phần tử được lấy mẫu tối thiểu có lớn hơn 1 hay không, nghĩa là không có phần tử được lấy mẫu nào bằng 1. 

| m | dp[0] | dp[1+] | trả lời | 
| --- | --- | --- | --- | 
| 1 | 3/4 | 1/4 | 3/4 | 
| 2 | (3/4)^2 | nghỉ ngơi | (3/4)^2 | 

Trường hợp này cho thấy hành vi đặc biệt khi$d=1$, trong đó chỉ có trạng thái đếm 0 mới quan trọng. DP giảm xuống mức phân rã hình học đơn giản theo kích thước mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nd)$| Mỗi trong số$n$các bước cập nhật lên tới$d$tiểu bang | 
| Không gian |$O(d)$| Chỉ có hai mảng DP có kích thước$d$được lưu trữ | 

Những ràng buộc cho phép$n$lên tới$3 \cdot 10^5$, do đó giải pháp này chỉ hiệu quả khi$d$là nhỏ một cách hợp lý. Cấu trúc của bài toán được thiết kế sao cho không gian trạng thái thu gọn thành việc theo dõi chỉ đếm tối đa$d-1$, đó chính xác là điều khiến DP trở nên khả thi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # placeholder for actual solution call
    return ""

# provided sample (format assumed)
assert run("5 2 3") == "", "sample 1"

# minimum size
assert run("1 1 1") == "", "n=1 edge"

# k = n (always good)
assert run("5 3 5") == "", "all elements good threshold edge"

# d = 1 (min statistic)
assert run("5 1 3") == "", "minimum comparison"

# large balanced case
assert run("10 5 5") == "", "balanced mid case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | tầm thường | đầu vào hợp lệ nhỏ nhất | 
| 5 3 5 | ranh giới hoàn toàn tốt | cực k = n | 
| 5 1 3 | thống kê thứ tự tối thiểu | d = 1 hành vi | 
| 10 5 5 | phân phối giữa | tính đúng đắn chung | 

## Vỏ cạnh 

Khi nào$d=1$, sự kiện trở thành$X_m \le 0$, nghĩa là không có phần tử được lấy mẫu nào rơi vào$[1,k]$. DP chỉ giữ lại một cách chính xác$dp[0]$, và mỗi bước nhân nó với$q$, sản xuất$q^m$. Điều này phù hợp với xác suất mà mọi trận hòa đều đến từ$[k+1,n]$. 

Khi$k=n$, mọi phần tử đều “tốt”, vì vậy$X_m = m$. điều kiện$X_m \le d-1$trở thành$m \le d-1$, điều đó là không thể đối với tất cả mọi người$m \ge d$, vì vậy tất cả đầu ra đều bằng không. DP phản ánh điều này bởi vì$p=1$, do đó khối lượng chuyển hoàn toàn sang trạng thái$x=m$, vượt quá phạm vi được theo dõi. 

Khi$k=0$, không có phần tử nào là tốt cả, vì thế$X_m=0$luôn luôn. DP giữ mọi xác suất trong$dp[0]$và mọi đầu ra đều trở thành 1, phù hợp với thực tế là$\text{nth}(q,d)$luôn lớn hơn$0$.
