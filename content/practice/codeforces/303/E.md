---
title: "CF 303E - Xếp hạng ngẫu nhiên"
description: "Mỗi người tham gia nhận được điểm có giá trị thực được chọn thống nhất từ ​​khoảng $[li, ri]$. Điểm số là độc lập. Sau khi tất cả điểm được tạo ra, người tham gia sẽ được sắp xếp theo điểm. Điểm nhỏ hơn có nghĩa là thứ hạng tốt hơn, vì vậy người tham gia có số điểm cao nhất sẽ về đích cuối cùng."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 303
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 183 (Div. 1)"
rating: 3000
weight: 303
solve_time_s: 187
verified: false
draft: false
---

[CF 303E - Xếp hạng ngẫu nhiên](https://codeforces.com/problemset/problem/303/E) 

**Đánh giá:** 3000 
**Tags:** dp, toán, xác suất 
**Thời gian giải:** 3 phút 7s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi người tham gia nhận được điểm có giá trị thực được chọn thống nhất từ một khoảng thời gian$[l_i, r_i]$. Điểm số là độc lập. Sau khi tất cả điểm được tạo ra, người tham gia sẽ được sắp xếp theo điểm. Điểm nhỏ hơn có nghĩa là thứ hạng tốt hơn, vì vậy người tham gia có số điểm cao nhất sẽ về đích cuối cùng. 

Đối với mỗi người tham gia$i$và mọi cấp bậc$j$, chúng ta phải tính xác suất mà người tham gia$i$kết thúc chính xác tại vị trí$j$. 

Sự phân phối là liên tục, do đó các mối quan hệ xảy ra với xác suất bằng không. Điều đó loại bỏ tất cả sự mơ hồ trong xếp hạng. 

Khó khăn rõ ràng nhất là các khoảng chồng lên nhau theo những cách tùy ý. Đối với một người tham gia$i$, xác suất để một người tham gia khác$k$điểm dưới anh ta phụ thuộc vào giá trị chính xác được lấy mẫu cho$i$. Những sự kiện đó không độc lập trên toàn cầu, nhưng chúng trở nên độc lập sau khi được điều hòa$i$điểm số của. 

Ràng buộc$n \le 80$đủ nhỏ để lập trình động bậc ba hoặc bậc bốn, nhưng quá lớn đối với bất kỳ thứ gì theo cấp số nhân. Việc liệt kê tất cả các thứ tự tương đối sẽ yêu cầu$n!$những khả năng là vô vọng ngay cả đối với$n=15$. Tọa độ có thể lớn bằng$10^9$, do đó bất kỳ cách tiếp cận nào có thể rời rạc hóa tất cả các giá trị thực một cách trực tiếp đều không thể thực hiện được. Giải pháp chỉ phải phụ thuộc vào điểm cuối khoảng. 

Các khoảng thời gian là liên tục, tạo ra một số trường hợp cạnh tinh tế. 

Giả sử hai khoảng không trùng nhau:```
2
0 1
10 11
```Người tham gia đầu tiên luôn được xếp hạng trước người thứ hai. Việc triển khai bất cẩn bằng cách sử dụng các bất đẳng thức nghiêm ngặt một cách không nhất quán có thể vô tình gán một khối lượng xác suất nhỏ cho các thứ tự không thể thực hiện được. 

Một trường hợp phức tạp khác xuất hiện khi một khoảng được chứa hoàn toàn trong một khoảng khác:```
2
0 10
4 6
```Ở đây, người tham gia thứ hai không bao giờ có thể đạt điểm dưới 0 hoặc trên 10, do đó đường cong xác suất thay đổi chính xác tại các ranh giới khoảng thời gian. Thiếu những điểm dừng đó sẽ gây ra sự tích hợp không chính xác. 

Trường hợp quan trọng thứ ba là khi có nhiều điểm cuối trùng nhau:```
3
0 5
0 5
0 5
```Tất cả các thứ hạng đều đối xứng nên mọi người tham gia đều phải có xác suất$1/3$cho mỗi cấp bậc. Bất kỳ sự bất đối xứng nào gây ra bởi sự lệch dấu phẩy động hoặc phân vùng khoảng thời gian không chính xác sẽ ngay lập tức phát hiện ra lỗi. 

Bản chất liên tục của việc phân phối cũng có vấn đề. Coi như:```
2
0 1
1 2
```Xác suất để có điểm bằng nhau chính xác bằng 0 mặc dù các khoảng cách đều chạm vào một điểm cuối. Việc triển khai coi các khoảng thời gian là các phạm vi riêng biệt khép kín có thể đếm các mối quan hệ không chính xác. 

## Phương pháp tiếp cận 

Quan điểm vũ phu rất đơn giản. Chọn một người tham gia$i$. Với mọi điểm số có thể$x$anh ta có thể thu được, tính toán phân bố xác suất xem có bao nhiêu người tham gia khác đạt điểm dưới đây$x$. Sau đó tích hợp trên tất cả$x$. 

Nếu chúng ta biết tỉ số chính xác$x$, mỗi người tham gia$k$độc lập thỏa mãn:$$P(s_k < x)$$Thứ hạng của$i$sau đó được xác định bởi có bao nhiêu người tham gia có điểm số nhỏ hơn. 

Trở ngại vũ phu là tích phân. Biểu thức xác suất thay đổi bất cứ khi nào$x$đi qua một điểm cuối khoảng. Giữa các điểm cuối, mọi xác suất đều tuyến tính theo$x$. Một ý tưởng ngây thơ là tích hợp số trên nhiều điểm mẫu, nhưng điều đó không thể đạt được độ chính xác cần thiết một cách chắc chắn. 

Quan sát quan trọng là đường thực có thể được phân chia thành các đoạn được hình thành bởi tất cả các điểm cuối của khoảng. Bên trong một đoạn cố định$[a,b]$, mọi biểu thức xác suất đều trở thành hàm affine của$x$. 

Dành cho người tham gia$k$:$$P(s_k < x)= \begin{cases} 0 & x \le l_k \\ 1 & x \ge r_k \\ \dfrac{x-l_k}{r_k-l_k} & l_k < x < r_k \end{cases}$$Bên trong một phân khúc cố định, mỗi người tham gia thuộc một trong ba loại. 

Một số đóng góp hằng số 0. 

Một số đóng góp không đổi 1. 

Một số đóng góp một chức năng tuyến tính$\alpha x + \beta$. 

Cấu trúc đó cho phép DP đa thức. Thay vì tính toán lại các xác suất theo điểm, chúng ta duy trì một đa thức sinh có hệ số$t^j$bằng xác suất chính xác$j$người tham gia đạt điểm dưới đây$x$. 

Bản thân các hệ số trở thành đa thức trong$x$. Vì mỗi yếu tố là tuyến tính nên sau khi xử lý tất cả người tham gia, hệ số xếp hạng$j$nhiều nhất là một đa thức bậc$n$. Chúng tôi có thể tích hợp nó một cách chính xác trên toàn bộ phân khúc. 

Điều này biến một bài toán xác suất liên tục không thể thành tích phân đa thức chính xác trên$O(n)$phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tích hợp số Brute Force | Hàm mũ hoặc không ổn định | Lớn | Quá chậm/không chính xác | 
| Phân đoạn DP có tích phân đa thức |$O(n^4)$|$O(n^3)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Thu thập tất cả các điểm cuối khoảng$l_i$Và$r_i$, sắp xếp chúng và loại bỏ các bản sao. 

Các điểm cuối này phân chia dòng số thành các đoạn trong đó mọi biểu thức xác suất đều có dạng đại số cố định. 
2. Xử lý từng người tham gia$i$một cách độc lập. 

Chúng tôi tính toán phân phối xác suất của thứ hạng người tham gia$i$. 
3. Với mọi cặp điểm cuối liên tiếp$[a,b]$, chỉ xem xét phân khúc này. 

Nếu người tham gia$i$khoảng thời gian của không chồng lên phân đoạn này, hãy bỏ qua nó vì người tham gia$i$không bao giờ có thể đạt được điểm số bên trong nó. 
4. Bên trong phân khúc này, thể hiện mật độ người tham gia$i$điểm số của. 

Từ$i$điểm của là thống nhất:$$f_i(x)=\frac{1}{r_i-l_i}$$Điều này là không đổi trong khoảng thời gian. 
5. Đối với mọi người tham gia khác$k$, xác định dạng của$$p_k(x)=P(s_k < x)$$bên trong phân khúc hiện tại. 

Chỉ có ba khả năng:$$0,\quad 1,\quad \frac{x-l_k}{r_k-l_k}$$6. Xây dựng DP đối với những người tham gia. 

Cho phép:$$dp[j][d]$$biểu thị hệ số của$x^d$trong đa thức biểu thị xác suất chính xác$j$người tham gia đạt điểm dưới đây$x$. 

Ban đầu:$$dp[0][0]=1$$7. Xử lý từng người tham gia$k \ne i$. 

Nhân đa thức tạo hiện tại với:$$(1-p_k(x)) + p_k(x)t$$Hệ số của$t^j$theo dõi xác suất chính xác$j$người tham gia ở bên dưới$x$. 

Vì mọi$p_k(x)$tuyến tính, các hệ số vẫn là đa thức bậc thấp. 
8. Sau khi xử lý tất cả người tham gia, hệ số tương ứng chính xác$j-1$điểm nhỏ hơn cho biết mật độ xác suất mà người tham gia$i$có thứ hạng$j$. 
9. Tích phân đa thức này trên đoạn thẳng. 

Nếu như:$$P(x)=\sum c_d x^d$$sau đó:$$\int_a^b P(x)\,dx = \sum c_d \frac{b^{d+1}-a^{d+1}}{d+1}$$10. Thêm phần đóng góp vào ma trận câu trả lời. 

### Tại sao nó hoạt động 

Sửa người tham gia$i$và sửa giá trị điểm$x$. Có điều kiện bật$s_i=x$, mọi người tham gia khác độc lập đều có điểm dưới đây$x$hoặc cao hơn$x$. Đa thức tạo mã hóa chính xác phân bố có bao nhiêu người tham gia nằm dưới$x$. 

Hệ số của$t^{j-1}$bằng xác suất chính xác$j-1$người tham gia đạt điểm dưới đây$x$, có nghĩa là người tham gia$i$có thứ hạng$j$. 

Phân vùng dòng thực đảm bảo rằng mọi biểu thức xác suất đều tuyến tính bên trong một phân đoạn, do đó hệ số DP là đa thức thông thường. Tích hợp chính xác trên mỗi phân đoạn sẽ tích lũy xác suất vô điều kiện trên tất cả các giá trị có thể có của$s_i$. 

Vì các phân đoạn bao trùm toàn bộ phần hỗ trợ mà không trùng lặp nên việc tính tổng tất cả các đóng góp của phân đoạn sẽ mang lại xác suất cuối cùng chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    L = []
    R = []

    for _ in range(n):
        l, r = map(float, input().split())
        L.append(l)
        R.append(r)

    pts = sorted(set(L + R))

    ans = [[0.0] * n for _ in range(n)]

    for i in range(n):

        for s in range(len(pts) - 1):
            a = pts[s]
            b = pts[s + 1]

            if a == b:
                continue

            left = max(a, L[i])
            right = min(b, R[i])

            if left >= right:
                continue

            length_i = R[i] - L[i]

            # dp[j][d] = coefficient of x^d
            dp = [[0.0] * (n + 1) for _ in range(n + 1)]
            dp[0][0] = 1.0

            processed = 0

            for k in range(n):
                if k == i:
                    continue

                len_k = R[k] - L[k]

                # p(x) = A*x + B
                if right <= L[k]:
                    A = 0.0
                    B = 0.0
                elif left >= R[k]:
                    A = 0.0
                    B = 1.0
                elif L[k] <= left and right <= R[k]:
                    A = 1.0 / len_k
                    B = -L[k] / len_k
                else:
                    # because all breakpoints are endpoints,
                    # partial overlap cannot happen here
                    A = 0.0
                    B = 0.0

                ndp = [[0.0] * (n + 1) for _ in range(n + 1)]

                for j in range(processed + 1):
                    for d in range(processed + 1):

                        cur = dp[j][d]
                        if abs(cur) < 1e-15:
                            continue

                        # multiply by (1 - p)
                        ndp[j][d] += cur * (1.0 - B)
                        ndp[j][d + 1] += cur * (-A)

                        # multiply by p * t
                        ndp[j + 1][d] += cur * B
                        ndp[j + 1][d + 1] += cur * A

                dp = ndp
                processed += 1

            density = 1.0 / length_i

            for rank in range(n):
                poly = dp[rank]

                val = 0.0

                for d in range(n + 1):
                    c = poly[d]
                    if abs(c) < 1e-15:
                        continue

                    val += c * (
                        (right ** (d + 1) - left ** (d + 1))
                        / (d + 1)
                    )

                ans[i][rank] += val * density

    for i in range(n):
        print(*["{:.10f}".format(x) for x in ans[i]])

solve()
```Phần đầu tiên đọc tất cả các khoảng và thu thập mọi điểm cuối. Những điểm cuối đó xác định những nơi duy nhất mà bất kỳ công thức xác suất nào cũng có thể thay đổi. 

Đối với mỗi người tham gia$i$, thuật toán lặp qua mọi phân đoạn cơ bản giữa các điểm cuối liên tiếp. Bên trong một phân đoạn, mọi xác suất so sánh đều tuyến tính theo$x$, đây là thuộc tính quan trọng cho phép DP đa thức. 

DP lưu trữ các hệ số đa thức thay vì xác suất đơn giản. Khi người tham gia khác đóng góp xác suất$A x + B$, nhân với:$$(1-p(x)) + p(x)t$$cập nhật cả số lượng người tham gia nhỏ hơn và mức độ đa thức. 

Việc thực hiện cẩn thận tách hệ số của$x^d$từ thứ nguyên đếm thứ hạng. Thiếu sự phân biệt này là lỗi phổ biến nhất khi dịch toán thành mã. 

Một điểm tinh tế khác là sự phân loại chồng chéo. Bởi vì tất cả đường viền phân khúc đều là điểm cuối khoảng, nên người tham gia bên trong phân khúc chỉ có thể ở hoàn toàn bên dưới, hoàn toàn bên trên hoặc hoạt động hoàn toàn với xác suất tuyến tính. Không có hình học nào khác là có thể. 

Sự tích hợp cuối cùng là chính xác. Vì mỗi hệ số là một số hạng đa thức nên việc tích phân trên phân đoạn chỉ yêu cầu công thức tích phân lũy thừa tiêu chuẩn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 6
4 9
```Những điểm quan trọng là:```
1, 4, 6, 9
```Chúng tôi xử lý người tham gia 1. 

| Phân đoạn | Phạm Vi Hoạt Động |$P(s_2 < x)$| Đóng góp hạng 1 | 
| --- | --- | --- | --- | 
| [1,4] | [1,4] | 0 | luôn đầu tiên | 
| [4,6] | [4,6] |$(x-4)/5$| hỗn hợp | 

Đối với phân khúc$[1,4]$:$$\int_1^4 \frac{1}{5} dx = \frac{3}{5}$$Người tham gia 1 luôn là người đầu tiên ở đó. 

Đối với phân khúc$[4,6]$:$$P(\text{rank }1)=1-\frac{x-4}{5}$$Tích hợp:$$\frac{1}{5}\int_4^6 \left(1-\frac{x-4}{5}\right)dx = 0.32$$Tổng cộng:$$0.6 + 0.32 = 0.92$$Vậy người tham gia 1 có xác suất$0.92$về việc hoàn thiện đầu tiên. 

Ví dụ này cho thấy biểu thức xác suất thay đổi chính xác như thế nào tại các điểm cuối của khoảng. 

### Ví dụ 2 

đầu vào:```
3
0 5
0 5
0 5
```Tất cả những người tham gia đều đối xứng. 

Dành cho người tham gia 1 và ghi điểm$x$:$$P(s_k < x)=\frac{x}{5}$$cho cả những người tham gia khác. 

Đa thức sinh trở thành:$$\left(1-\frac{x}{5}+\frac{x}{5}t\right)^2$$Mở rộng: 

| Người tham gia nhỏ hơn | Đa thức | 
| --- | --- | 
| 0 |$(1-x/5)^2$| 
| 1 |$2(x/5)(1-x/5)$| 
| 2 |$(x/5)^2$| 

Tích hợp từng cái một$[0,5]$đưa ra chính xác$1/3$. 

Ví dụ này xác nhận rằng DP bảo toàn tính đối xứng một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^4)$|$O(n)$người tham gia,$O(n)$phân đoạn,$O(n^2)$Cập nhật DP | 
| Không gian |$O(n^2)$| Bảng DP hệ số đa thức | 

Với$n \le 80$,$O(n^4)$có thể chấp nhận được trong Python được tối ưu hóa vì các hệ số không đổi nhỏ và hầu hết các hệ số đa thức vẫn còn thưa thớt. Việc sử dụng bộ nhớ rất nhỏ so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    L = []
    R = []

    for _ in range(n):
        l, r = map(float, input().split())
        L.append(l)
        R.append(r)

    pts = sorted(set(L + R))

    ans = [[0.0] * n for _ in range(n)]

    for i in range(n):

        for s in range(len(pts) - 1):
            a = pts[s]
            b = pts[s + 1]

            left = max(a, L[i])
            right = min(b, R[i])

            if left >= right:
                continue

            dp = [[0.0] * (n + 1) for _ in range(n + 1)]
            dp[0][0] = 1.0

            processed = 0

            for k in range(n):
                if k == i:
                    continue

                lenk = R[k] - L[k]

                if right <= L[k]:
                    A = 0.0
                    B = 0.0
                elif left >= R[k]:
                    A = 0.0
                    B = 1.0
                else:
                    A = 1.0 / lenk
                    B = -L[k] / lenk

                ndp = [[0.0] * (n + 1) for _ in range(n + 1)]

                for j in range(processed + 1):
                    for d in range(processed + 1):
                        cur = dp[j][d]

                        ndp[j][d] += cur * (1 - B)
                        ndp[j][d + 1] += cur * (-A)

                        ndp[j + 1][d] += cur * B
                        ndp[j + 1][d + 1] += cur * A

                dp = ndp
                processed += 1

            density = 1.0 / (R[i] - L[i])

            for rank in range(n):
                res = 0.0

                for d in range(n + 1):
                    c = dp[rank][d]
                    res += c * (
                        (right ** (d + 1) - left ** (d + 1))
                        / (d + 1)
                    )

                ans[i][rank] += res * density

    out = []
    for row in ans:
        out.append(" ".join("{:.6f}".format(x) for x in row))
    print("\n".join(out))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
res = run("2\n1 6\n4 9\n")
lines = res.splitlines()

a = list(map(float, lines[0].split()))
b = list(map(float, lines[1].split()))

assert abs(a[0] - 0.92) < 1e-6
assert abs(a[1] - 0.08) < 1e-6
assert abs(b[0] - 0.08) < 1e-6
assert abs(b[1] - 0.92) < 1e-6

# minimum size
res = run("1\n0 10\n")
x = float(res.strip())
assert abs(x - 1.0) < 1e-6

# identical intervals
res = run("3\n0 5\n0 5\n0 5\n")
for line in res.splitlines():
    vals = list(map(float, line.split()))
    for v in vals:
        assert abs(v - 1/3) < 1e-6

# disjoint intervals
res = run("2\n0 1\n10 11\n")
lines = res.splitlines()

a = list(map(float, lines[0].split()))
b = list(map(float, lines[1].split()))

assert abs(a[0] - 1.0) < 1e-6
assert abs(a[1]) < 1e-6
assert abs(b[0]) < 1e-6
assert abs(b[1] - 1.0) < 1e-6

# touching endpoints
res = run("2\n0 1\n1 2\n")
lines = res.splitlines()

a = list(map(float, lines[0].split()))
b = list(map(float, lines[1].split()))

assert abs(a[0] - 1.0) < 1e-6
assert abs(b[1] - 1.0) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Người tham gia đơn lẻ | Xác suất 1 cho hạng 1 | Trường hợp cơ sở | 
| Tất cả các khoảng bằng nhau | Xác suất xếp hạng thống nhất | Tính đối xứng chính xác | 
| Khoảng rời rạc | Xếp hạng xác định | Xử lý điểm cuối | 
| Chạm vào điểm cuối | Không có xác suất hòa | Độ chính xác phân phối liên tục | 

## Vỏ cạnh 

Xem xét các khoảng thời gian chạm:```
2
0 1
1 2
```Người tham gia 1 luôn đạt điểm dưới người tham gia 2. Các khoảng chia sẻ điểm cuối 1, nhưng sự bằng nhau xảy ra với xác suất bằng 0 vì sự phân bố là liên tục. 

Các điểm phân vùng là:```
0, 1, 2
```Bên trong$[0,1]$, người tham gia 2 luôn vượt số điểm hiện tại nên:$$P(s_2 < x)=0$$Thuật toán chỉ tích hợp trên các đoạn có độ dài mở, do đó điểm đơn$x=1$đóng góp số đo bằng không. Kết quả trở nên chính xác:```
1 0
0 1
```Bây giờ hãy xem xét các khoảng giống hệt nhau:```
3
0 5
0 5
0 5
```Tất cả những người tham gia đều sử dụng cùng một xác suất tuyến tính:$$P(s_k < x)=x/5$$Đa thức sinh là đối xứng cho mọi người tham gia. Vì khoảng tích phân cũng bằng nhau nên mọi thứ hạng đều nhận được xác suất chính xác$1/3$. 

Cuối cùng hãy xem xét các khoảng lồng nhau:```
2
0 10
4 6
```Các điểm phân vùng là:```
0, 4, 6, 10
```Bên trong$[0,4]$, người tham gia 2 không bao giờ có thể đạt điểm dưới giá trị hiện tại. 

Bên trong$[4,6]$, xác suất trở thành tuyến tính:$$\frac{x-4}{2}$$Bên trong$[6,10]$, người tham gia 2 luôn ở bên dưới. 

Thuật toán xử lý từng phân đoạn riêng biệt, do đó công thức xác suất sẽ thay đổi chính xác ở những nơi cần thiết. Thiếu phân đoạn này là nguồn gốc của các câu trả lời sai trong bài toán này.
