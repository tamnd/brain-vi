---
title: "CF 28C - Xếp hàng tắm"
description: "Mỗi học sinh độc lập chọn ngẫu nhiên một trong các phòng tắm giống nhau. Một phòng có thể có nhiều bồn rửa nên học sinh vào phòng đó sẽ được chia thành nhiều hàng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 28
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 28 (Codeforces format)"
rating: 2200
weight: 28
solve_time_s: 104
verified: true
draft: false
---
[CF 28C - Xếp hàng tắm](https://codeforces.com/problemset/problem/28/C) 

**Đánh giá:** 2200 
**Tags:** tổ hợp, dp, xác suất 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi học sinh độc lập chọn ngẫu nhiên một trong các phòng tắm giống nhau. Một phòng có thể có nhiều bồn rửa nên học sinh vào phòng đó sẽ được chia thành nhiều hàng. Các hàng đợi được cân bằng nhất có thể, vì học sinh tự sắp xếp để giảm thiểu độ dài hàng đợi lớn nhất trong phòng đó. 

Nếu một phòng có`x`sinh viên và`a[i]`các lưu vực, khi đó kích thước hàng đợi lớn nhất bên trong phòng đó sẽ trở thành:$$\left\lceil \frac{x}{a[i]} \right\rceil$$Giá trị chung cho một lần phân công ngẫu nhiên là kích thước hàng đợi tối đa trong số tất cả các phòng. Chúng ta phải tính giá trị kỳ vọng của mức tối đa này. 

Các ràng buộc đủ nhỏ để cho phép lập trình động, nhưng quá lớn để liệt kê lực lượng vũ phu. Có tối đa 50 sinh viên và 50 phòng. Số nhiệm vụ có thể thực hiện là:$$m^n$$Với`n = m = 50`, điều này trở thành`50^50`, điều này hoàn toàn không thể liệt kê trực tiếp. 

Giới hạn nhỏ gợi ý rằng xác suất DP dựa trên trạng thái được dự định. Bất kỳ thuật toán nào xung quanh`O(n^3 m)`hoặc`O(n^4)`vẫn ổn trong giới hạn 2 giây. Một cái gì đó theo cấp số nhân trong`n`hoặc`m`thì không. 

Phần nguy hiểm nhất của vấn đề là hiểu chính xác điều gì quyết định kích thước hàng đợi lớn nhất trong phòng. Việc thực hiện bất cẩn có thể sử dụng sai cách chia tầng. 

Coi như:```
2 1
3
```Có 2 sinh viên và 3 lưu vực. Kích thước hàng đợi lớn nhất không phải là`0`, mặc dù`2 // 3 = 0`. Mỗi học sinh chỉ cần chiếm một chậu riêng, nên câu trả lời là`1`. 

Một trường hợp tế nhị khác xuất hiện khi học sinh phân bố không đều.```
5 1
2
```Với 5 sinh viên và 2 lưu vực, cách phân chia tối ưu là xếp hàng theo kích cỡ`3`Và`2`. Hàng đợi lớn nhất là`3`, không`2`. 

Trường hợp có nhiều xác suất hơn là khi nhiều phòng có số lượng lưu vực lớn.```
2 2
100 100
```Cho dù học sinh chọn phòng như thế nào, mỗi phòng có người sử dụng đều có quy mô hàng đợi lớn nhất`1`. Giá trị mong đợi chính xác là`1`. Một mô phỏng đơn giản hoặc quá trình chuyển đổi không chính xác có thể vô tình đếm các hàng đợi trống và tạo ra thứ gì đó nhỏ hơn. 

Sai lầm lớn nhất về mặt khái niệm là cho rằng các phòng đều độc lập sau khi đã ấn định mức tối đa. Chúng không độc lập vì số lượng học sinh trong các phòng phải cộng lại bằng`n`. DP phải theo dõi số lượng học sinh đã được phân công. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Mỗi học sinh chọn một trong các`m`phòng. Sau khi xây dựng một bài tập hoàn chỉnh, hãy đếm xem có bao nhiêu học sinh vào mỗi phòng. Đối với phòng`i`, tính:$$\left\lceil \frac{cnt_i}{a_i} \right\rceil$$Sau đó lấy mức tối đa trên các phòng và trung bình trên tất cả các bài tập. 

Điều này hiệu quả vì mọi nhiệm vụ đều có khả năng như nhau. Vấn đề là số lượng bài tập:$$m^n$$Đối với các ràng buộc tối đa, giá trị này lớn về mặt thiên văn. 

Quan sát quan trọng là chúng tôi không quan tâm đến danh tính chính xác của học sinh. Chỉ có số lượng sinh viên vào mỗi phòng mới quan trọng. 

Giả sử phòng`i`nhận được`k`sinh viên. Xác suất của sự kiện này là nhị thức:$$\binom{n}{k}\left(\frac1m\right)^k$$Quan trọng hơn, kích thước hàng đợi lớn nhất do phòng đó tạo ra chỉ phụ thuộc vào`k`Và`a[i]`. 

Điều này gợi ý việc định hình lại vấn đề. Thay vì tính toán trực tiếp mức tối đa dự kiến, hãy tính:$$P(\text{maximum queue size} \le t)$$cho mọi ngưỡng`t`. 

Đối với một ngưỡng cố định`t`, phòng`i`có thể chấp nhận nhiều nhất:$$a_i \cdot t$$sinh viên, nếu không thì hàng đợi lớn nhất của nó sẽ vượt quá`t`. 

Bây giờ vấn đề trở thành việc đếm xác suất để mỗi phòng đều nằm trong giới hạn cho phép. 

Điều này biến vấn đề thành một DP đa thức cổ điển. Chúng tôi xử lý từng phòng một và theo dõi số lượng học sinh đã được phân bổ. 

Đối với mỗi phòng, chúng tôi thử phân công`k`học sinh với nó, với điều kiện:$$\left\lceil \frac{k}{a_i} \right\rceil \le t$$Tương đương:$$k \le a_i t$$Xác suất đóng góp cho việc gán chính xác`k`sinh viên đến phòng này đến từ tổ hợp. Chúng tôi chọn những học sinh còn lại vào phòng này. 

Một khi chúng ta có thể tính toán:$$P(\max \le t)$$giá trị kỳ vọng suy ra từ:$$E[\max] = \sum_{t \ge 1} P(\max \ge t)$$hoặc tương đương:$$E[\max] = \sum_{t=1}^{n} \left(1 - P(\max \le t-1)\right)$$Công thức thứ hai thuận tiện vì kích thước hàng đợi tối đa luôn nằm trong khoảng`1`Và`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m^n \cdot (n+m)) | O(m) | Quá chậm | 
| Tối ưu | O(m \cdot n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước hệ số nhị thức`C[n][k]`cho tất cả`0 ≤ n,k ≤ 50`. 

Các hệ số này biểu thị số cách chúng ta có thể chọn để học sinh vào một phòng cụ thể. 
2. Tính toán trước quyền hạn của`m`. 

Vì mọi phép gán đều có xác suất:$$\frac1{m^n}$$chúng tôi liên tục phân chia theo sức mạnh của`m`. 
3. Đối với mọi ngưỡng`t`từ`0`ĐẾN`n`, tính:$$P_t = P(\max \le t)$$Điều này có nghĩa là mỗi phòng tối đa phải có quy mô hàng đợi lớn nhất`t`. 
4. Sử dụng lập trình động trên các phòng. 

Định nghĩa:$$dp[i][j]$$như số cách hợp lệ để phân phối chính xác`j`sinh viên trong số những người đầu tiên`i`phòng trong khi vẫn tôn trọng ngưỡng`t`. 
5. Khởi tạo:$$dp[0][0] = 1$$Trước khi xử lý bất kỳ phòng nào, không có sinh viên nào được chỉ định một cách hợp lệ. 
6. Xử lý từng phòng một. 

Giả sử chúng ta đang ở trong phòng`i`và đã được giao`j`sinh viên. Hãy thử đặt`k`thêm học sinh vào phòng này. 

Quá trình chuyển đổi này chỉ được phép nếu:$$\left\lceil \frac{k}{a_i} \right\rceil \le t$$tương đương với:$$k \le a_i t$$7. Thêm phần chuyển tiếp:$$dp[i+1][j+k] += dp[i][j] \cdot \binom{n-j}{k}$$Chúng tôi chọn cái nào`k`số học sinh còn lại đi vào phòng này. 
8. Sau khi tất cả các phòng được xử lý,`dp[m][n]`bằng số lượng nhiệm vụ trong đó kích thước hàng đợi tối đa là nhiều nhất`t`. 
9. Chia cho:$$m^n$$để có được xác suất:$$P_t$$10. Phục hồi kỳ vọng bằng cách sử dụng:$$E = \sum_{x=1}^{n} \left(1 - P_{x-1}\right)$$Thuật ngữ`1 - P_{x-1}`chính xác là xác suất mà kích thước hàng đợi tối đa ít nhất là`x`. 

### Tại sao nó hoạt động 

Đối với một ngưỡng cố định`t`, DP sẽ tính chính xác các nhiệm vụ trong đó mọi phòng đều tôn trọng giới hạn. Quá trình chuyển đổi sẽ chọn số lượng học sinh vào phòng hiện tại và đếm tất cả các tập hợp con có thể có của những học sinh còn lại có thể vào phòng đó. 

Mỗi bài tập đầy đủ tương ứng với chính xác một đường dẫn qua DP, vì mỗi phòng nhận được số lượng học sinh được xác định duy nhất. Ngược lại, mọi đường dẫn DP đều tạo ra một phép gán hợp lệ. 

Công thức kỳ vọng hoạt động vì đối với bất kỳ biến ngẫu nhiên số nguyên không âm nào:$$E[X] = \sum_{x \ge 1} P(X \ge x)$$Đây`X`là kích thước hàng đợi tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXN = 55

# binomial coefficients
C = [[0] * MAXN for _ in range(MAXN)]

for n in range(MAXN):
    C[n][0] = C[n][n] = 1
    for k in range(1, n):
        C[n][k] = C[n - 1][k - 1] + C[n - 1][k]

n, m = map(int, input().split())
a = list(map(int, input().split()))

total = float(m ** n)

# prob[t] = P(max <= t)
prob = [0.0] * (n + 1)

for t in range(n + 1):
    dp = [[0.0] * (n + 1) for _ in range(m + 1)]
    dp[0][0] = 1.0

    for i in range(m):
        limit = a[i] * t

        for used in range(n + 1):
            if dp[i][used] == 0:
                continue

            for take in range(n - used + 1):
                if take > limit:
                    break

                ways = C[n - used][take]
                dp[i + 1][used + take] += dp[i][used] * ways

    prob[t] = dp[m][n] / total

ans = 0.0

for x in range(1, n + 1):
    ans += 1.0 - prob[x - 1]

print(f"{ans:.15f}")
```Phần đầu tiên tính toán trước các hệ số nhị thức với tam giác Pascal. Chúng tôi chỉ cần các giá trị lên tới 50, vì vậy việc này hầu như không tốn kém gì. 

DP được xây dựng lại độc lập cho mọi ngưỡng`t`. Nhà nước`dp[i][used]`lưu trữ có bao nhiêu bài tập hợp lệ tồn tại sau khi xử lý lần đầu tiên`i`phòng và phân công`used`sinh viên. 

Quá trình chuyển đổi là bước kết hợp cốt lõi. Giả định`used`học sinh đã được phân công rồi. có`n - used`sinh viên vẫn miễn phí. Lựa chọn chính xác`take`trong số chúng cho phòng hiện tại có thể được thực hiện trong:$$\binom{n-used}{take}$$cách. 

điều kiện`take <= a[i] * t`đảm bảo rằng hàng đợi lớn nhất trong phòng không vượt quá`t`. 

Một lỗi phổ biến là cố gắng sử dụng trực tiếp các hệ số đa thức. Việc xây dựng nhị thức tăng dần đơn giản hơn và tránh được việc đếm quá nhiều vì mỗi học sinh được chỉ định chính xác một lần trong khi xử lý các phòng một cách tuần tự. 

Một điểm tinh tế khác là ý nghĩa của`t = 0`. Ngưỡng này là không thể trừ khi không có sinh viên. Từ`n ≥ 1`,`prob[0]`tự động trở thành số 0. Giữ trường hợp này đơn giản hóa công thức kỳ vọng. 

Tổng kết cuối cùng sử dụng:$$E[X] = \sum_{x \ge 1} P(X \ge x)$$được thực hiện như:```
ans += 1 - prob[x - 1]
```## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1
2
```Có một sinh viên và một phòng có hai bồn rửa. 

Đối với ngưỡng`t = 0`: 

| Phòng đã xử lý | Sinh viên được phân công | Những cách hợp lệ | 
| --- | --- | --- | 
| 0 | 0 | 1 | 
| 1 | 1 | 0 | 

Phòng không thể nhận dù chỉ một học sinh vì`2 * 0 = 0`. 

Vì thế:$$P(\max \le 0) = 0$$Đối với ngưỡng`t = 1`: 

| Phòng đã xử lý | Sinh viên được phân công | Những cách hợp lệ | 
| --- | --- | --- | 
| 0 | 0 | 1 | 
| 1 | 1 | 1 | 

Bây giờ phòng có thể chấp nhận lên đến`2`học sinh nên bài tập có giá trị.$$P(\max \le 1) = 1$$Kỳ vọng:$$1 - P(\max \le 0) = 1$$Đầu ra:```
1.000000000000000
```Điều này xác nhận rằng ngay cả khi có nhiều lưu vực hơn số sinh viên, quy mô hàng đợi lớn nhất ít nhất vẫn là`1`. 

### Ví dụ 2 

đầu vào:```
2 2
1 1
```Mỗi phòng có một chậu rửa nên số lượng người xếp hàng bằng số học sinh trong phòng. 

Đối với ngưỡng`t = 1`, mỗi phòng có thể chứa tối đa một học sinh. 

| Phòng | Sinh viên đã qua sử dụng trước đây | Sinh viên đã thêm | Trạng thái kết quả | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | dp[1][0] += 1 | 
| 1 | 0 | 1 | dp[1][1] += 2 | 
| 2 | 0 | 0 | dp[2][0] += 1 | 
| 2 | 1 | 1 | dp[2][2] += 2 | 

có`2^2 = 4`tổng số bài tập. 

Có chính xác hai bài tập chia đều học sinh cho các phòng. 

Vì thế:$$P(\max \le 1) = \frac{2}{4} = \frac12$$Kích thước hàng đợi tối đa là`1`hoặc`2`. 

Kể từ đây:$$E = 1 \cdot \frac12 + 2 \cdot \frac12 = 1.5$$Đầu ra:```
1.500000000000000
```Ví dụ này chứng minh tại sao việc đếm xác suất trực tiếp khó hơn việc đếm các phép gán ngưỡng hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m \cdot n^3) | Đối với mỗi ngưỡng, chúng tôi lặp lại các phòng, học sinh đã sử dụng và học sinh đã thêm | 
| Không gian | O(m \cdot n) | Bàn DP qua các phòng và phân công học sinh | 

Với`n,m ≤ 50`, tổng số thao tác nằm dưới mức giới hạn một cách thoải mái. Việc sử dụng bộ nhớ rất nhỏ, chỉ vài nghìn giá trị dấu phẩy động. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    MAXN = 55

    C = [[0] * MAXN for _ in range(MAXN)]

    for n in range(MAXN):
        C[n][0] = C[n][n] = 1
        for k in range(1, n):
            C[n][k] = C[n - 1][k - 1] + C[n - 1][k]

    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    total = float(m ** n)

    prob = [0.0] * (n + 1)

    for t in range(n + 1):
        dp = [[0.0] * (n + 1) for _ in range(m + 1)]
        dp[0][0] = 1.0

        for i in range(m):
            limit = a[i] * t

            for used in range(n + 1):
                for take in range(n - used + 1):
                    if take > limit:
                        break

                    dp[i + 1][used + take] += (
                        dp[i][used] *
                        C[n - used][take]
                    )

        prob[t] = dp[m][n] / total

    ans = 0.0

    for x in range(1, n + 1):
        ans += 1.0 - prob[x - 1]

    print(f"{ans:.15f}")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run("1 1\n2\n") == "1.000000000000000"

# single basin, everyone forced together
assert run("5 1\n1\n") == "5.000000000000000"

# many basins, queues always size 1
assert run("2 2\n100 100\n") == "1.000000000000000"

# symmetric split case
res = float(run("2 2\n1 1\n"))
assert abs(res - 1.5) < 1e-9

# off-by-one ceiling division check
res = float(run("5 1\n2\n"))
assert abs(res - 3.0) < 1e-9
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 2`|`1.0`| Trường hợp không cần thiết tối thiểu | 
|`5 1 / 1`|`5.0`| Phòng đơn, chậu đơn | 
|`2 2 / 100 100`|`1.0`| Số lượng lưu vực lớn | 
|`2 2 / 1 1`|`1.5`| Phân nhánh xác suất | 
|`5 1 / 2`|`3.0`| Phân chia trần đúng cách | 

## Vỏ cạnh 

Hãy xem xét:```
2 1
3
```Phòng có nhiều chậu hơn sinh viên. Đối với ngưỡng`t = 0`, phòng có thể chấp nhận nhiều nhất`0`học sinh, vì vậy DP không thể xếp cả hai học sinh vào. Như vậy:$$P(\max \le 0)=0$$Đối với ngưỡng`t = 1`, phòng có thể chấp nhận lên đến`3`sinh viên, vì vậy tất cả các bài tập trở thành hợp lệ:$$P(\max \le 1)=1$$Kỳ vọng trở nên chính xác`1`. Thuật toán xử lý việc này một cách chính xác vì nó sử dụng điều kiện:$$k \le a_i t$$tương ứng với việc phân chia trần, không phải phân chia sàn. 

Bây giờ hãy xem xét:```
5 1
2
```Chỉ có một phòng có hai bồn rửa. Mỗi bài tập đều cử cả năm học sinh đến đó. 

Đối với ngưỡng`t = 2`, phòng có thể chấp nhận nhiều nhất`4`học sinh vì:$$2 \cdot 2 = 4$$DP từ chối nhiệm vụ. 

Đối với ngưỡng`t = 3`, phòng có thể chấp nhận lên đến`6`học sinh thì bài tập trở nên hợp lệ. 

Do đó kích thước hàng đợi tối đa chính xác là`3`. DP nắm bắt chính xác điểm chuyển tiếp này. 

Cuối cùng, hãy xem xét:```
2 2
100 100
```Mỗi phòng có thể chứa nhiều học sinh tùy ý mà vẫn giữ nguyên kích thước hàng đợi`1`. 

Đối với ngưỡng`t = 1`, giới hạn mỗi phòng trở thành`100`, vì vậy mọi phép gán có thể đều hợp lệ. DP đếm tất cả`2^2 = 4`bài tập và tính toán:$$P(\max \le 1)=1$$Vì vậy, kỳ vọng là chính xác`1`. Phòng trống không gây ra vấn đề gì vì luôn được phép chỉ định 0 học sinh.
