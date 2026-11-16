---
title: "CF 1934B - Một vấn đề về tiền xu khác"
description: "Chúng tôi được cung cấp một bộ mệnh giá tiền xu cố định: 1, 3, 6, 10 và 15. Đối với mỗi truy vấn, chúng tôi phải xây dựng tổng giá trị chính xác n bằng cách sử dụng bất kỳ số lượng đồng xu nào, với mục tiêu giảm thiểu số lượng đồng xu được sử dụng."
date: "2026-06-08T18:08:53+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1934
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 931 (Div. 2)"
rating: 1200
weight: 1934
solve_time_s: 80
verified: true
draft: false
---

[CF 1934B - Một vấn đề khác về tiền xu](https://codeforces.com/problemset/problem/1934/B) 

**Đánh giá:** 1200 
**Tags:** vũ lực, dp, tham lam, toán học 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ mệnh giá tiền xu cố định: 1, 3, 6, 10 và 15. Đối với mỗi truy vấn, chúng tôi phải xây dựng tổng giá trị chính xác n bằng cách sử dụng bất kỳ số lượng đồng xu nào, với mục tiêu giảm thiểu số lượng đồng xu được sử dụng. 

Cấu trúc then chốt ở đây là các loại coin có số lượng cố định và rất nhỏ nhưng n có thể cực lớn, lên tới một tỷ. Điều đó ngay lập tức gợi ý rằng việc liệt kê các tổ hợp hoặc thực hiện bất kỳ chương trình động dựa trên trạng thái nào trên n là không thể. Một giải pháp phải tính toán câu trả lời cho mỗi trường hợp kiểm thử trong thời gian khấu hao không đổi hoặc rất nhỏ. 

Một cách đọc ngây thơ có thể gợi ý rằng điều này giống như một con đường ngắn nhất hoặc vấn đề giảm thiểu kiểu ba lô. Tuy nhiên, các ràng buộc về giá trị đồng tiền và cấu trúc cụ thể của chúng gợi ý mạnh mẽ về cách tiếp cận phân rã tham lam hoặc có giới hạn. 

Một trường hợp khó nhận thấy là các chiến lược tham lam hoàn toàn dựa trên đồng tiền lớn nhất có thể thất bại nếu hệ thống tiền xu không chuẩn. Ví dụ: với các đồng tiền như 1, 3, 4, tham lam thất bại với 6 (4+1+1 so với 3+3). Vì vậy, chúng ta không thể cho rằng lòng tham hoạt động mà không có lý do chính đáng. Chúng ta phải dựa vào cấu trúc đặc biệt của số tam giác. 

Một trường hợp khác là các giá trị nhỏ của n trong đó sự kết hợp tốt nhất có thể kết hợp nhiều đồng xu nhỏ thay vì một đồng xu lớn. Ví dụ: n = 2 không thể sử dụng bất kỳ đồng xu nào lớn hơn 1, vì vậy nó phải rút về hai đồng xu 1. Bất kỳ chiến lược nào cũng phải xử lý chính xác các giá trị thấp này mà không giả định có sẵn các mệnh giá lớn hơn. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ coi đây như một chiếc ba lô không giới hạn cổ điển: với mỗi n, hãy tính dp[x] là số lượng xu tối thiểu cần thiết để tạo thành tổng x. Điều này yêu cầu chuyển đổi qua năm loại tiền xu, đưa ra O(n) cho mỗi trường hợp thử nghiệm. Với n tối đa 1e9 và tối đa 1e4 trường hợp thử nghiệm, điều này hoàn toàn không khả thi cả về thời gian và bộ nhớ. 

Chúng ta cần một góc nhìn khác. Quan sát quan trọng là bộ tiền xu cực kỳ nhỏ và có cấu trúc. Đồng xu lớn nhất là 15 và tất cả các đồng xu đều là bội số hoặc tổ hợp trong một phạm vi rất chặt chẽ. Điều này gợi ý rằng với n lớn, giải pháp sẽ bị chi phối bằng cách sử dụng càng nhiều 15 đồng xu càng tốt, chỉ một phần nhỏ còn lại có thể được xử lý một cách tối ưu. 

Điều này dẫn đến một chiến lược dư lượng giới hạn. Mọi lời giải tối ưu đều có thể viết dưới dạng: 

n = 15k + r, trong đó r < 15 

Ý tưởng chính là sau khi chúng tôi xác định số lượng 15 xu mà chúng tôi sử dụng, vấn đề còn lại chỉ là về phần còn lại r nhỏ, có thể được giải quyết một cách tối ưu bằng cách sử dụng vũ lực hoặc tính toán trước. Tuy nhiên, chỉ lấy sàn (n / 15) không phải lúc nào cũng tối ưu, vì việc giảm một đồng 15 xu đôi khi có thể cho phép kết hợp tốt hơn các đồng xu nhỏ hơn để giảm tổng số xu. 

Vì vậy, thay vì sửa k chính xác, chúng tôi thử một cửa sổ nhỏ xung quanh n // 15, điển hình là k, k-1, k-2, vì giải pháp tốt nhất chỉ có thể sai lệch một chút do độ chi tiết của đồng xu. Với mỗi ứng cử viên k, chúng ta tính toán số dư và giải quyết nó một cách tối ưu với các giá trị nhỏ. Phần còn lại bị giới hạn (trong thực tế nhỏ hơn 30 hoặc 45 tùy thuộc vào phạm vi điều chỉnh), do đó nó có thể được tính toán trước thông qua một DP nhỏ. 

Do đó, vấn đề giảm xuống còn việc thử một số khả năng không đổi cho mỗi trường hợp thử nghiệm, mỗi trường hợp được đánh giá bằng O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DP lên tới n | O(n) mỗi lần kiểm tra | O(n) | Quá chậm | 
| Tham lam không sửa chữa | O(1) | O(1) | Không đúng | 
| Dư lượng + DP cục bộ | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán trước một bảng DP nhỏ cho tất cả các giá trị có giới hạn cố định, ví dụ 30 hoặc 45, sử dụng đồng xu DP không giới hạn tiêu chuẩn với năm mệnh giá. Điều này đưa ra số lượng xu tối thiểu chính xác cho bất kỳ phần còn lại nhỏ nào. 

Đối với mỗi trường hợp thử nghiệm: 

1. Tính k = n // 15. Đây là số lượng cơ bản của đồng xu lớn nhất.

Điều này được chọn vì 15 là mệnh giá lớn nhất và chi phối số tiền lớn một cách hiệu quả. 
2. Đối với các giá trị ứng viên của k, hãy xem xét k, k-1 và k-2, bỏ qua các giá trị âm. 

Lý do là việc thay thế một đồng 15 xu sẽ làm tăng số tiền còn lại lên 15 và đôi khi điều này cho phép phân hủy tốt hơn đáng kể ở những đồng tiền nhỏ hơn. 
3. Với mỗi ứng viên k, tính r = n - 15 * k. 

Điều này làm giảm vấn đề xuống còn giải quyết một trường hợp có giá trị cố định nhỏ. 
4. Nếu r nằm trong phạm vi DP được tính toán trước, hãy lấy dp[r] làm chi phí. 

Điều này đưa ra cách tối ưu để hình thành phần còn lại bằng cách sử dụng các đồng tiền 1, 3, 6, 10, 15. 
5. Theo dõi giá trị tối thiểu của k + dp[r] trên tất cả các ứng cử viên. 

Điều này kết hợp việc sử dụng số tiền lớn và phân hủy phần còn lại tối ưu. 
6. Xuất ra giá trị tìm được nhỏ nhất. 

### Tại sao nó hoạt động 

Bất kỳ giải pháp tối ưu nào cũng có thể được xem là sự lựa chọn số lượng 15 xu được sử dụng, cộng với phần còn lại bao gồm các đồng tiền nhỏ hơn. Nếu chúng ta sử dụng quá nhiều 15 xu, chúng ta sẽ để lại một phần nhỏ còn lại và rất tốn kém để sửa chữa. Nếu chúng ta sử dụng quá ít, chúng ta sẽ lãng phí hiệu quả ở phạm vi phủ sóng lớn. Giải pháp tối ưu phải gần với lựa chọn tham lam n // 15 vì việc thay đổi k nhiều hơn một hằng số nhỏ sẽ dịch chuyển ít nhất 15 đơn vị giá trị và sự dịch chuyển như vậy không thể được bù đắp bằng những cải tiến trong hệ thống tiền xu nhỏ vượt quá phạm vi giới hạn. 

Do đó, việc giới hạn k trong một lân cận không đổi xung quanh lựa chọn tham lam sẽ duy trì tính tối ưu trong khi giảm vấn đề về tra cứu theo thời gian không đổi trên một không gian trạng thái hữu hạn được tính toán trước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

coins = [1, 3, 6, 10, 15]

MAX_R = 45
INF = 10**9

dp = [INF] * (MAX_R + 1)
dp[0] = 0

for i in range(1, MAX_R + 1):
    for c in coins:
        if i >= c:
            dp[i] = min(dp[i], dp[i - c] + 1)

def solve():
    n = int(input())
    k = n // 15
    ans = INF

    for dk in [0, -1, -2]:
        kk = k + dk
        if kk < 0:
            continue
        r = n - 15 * kk
        if 0 <= r <= MAX_R:
            ans = min(ans, kk + dp[r])

    print(ans)

t = int(input())
for _ in range(t):
    solve()
```Bảng DP`dp`được tính một lần cho tất cả các dư lượng nhỏ. Điều này rất quan trọng vì nó tránh được việc tính toán lại trong các trường hợp thử nghiệm. 

Bên trong`solve`, chúng tôi neo giải pháp xung quanh con số 15 xu tham lam. Chúng tôi kiểm tra rõ ràng các giá trị lân cận vì hệ thống tiền xu không hoàn toàn chuẩn mực và các điều chỉnh cục bộ có thể cải thiện tổng số lượng tiền xu. 

Việc chọn MAX_R = 45 đảm bảo rằng ngay cả sau khi giảm k tối đa 2, phần còn lại vẫn nằm trong phạm vi mà DP hợp lệ. Giới hạn không đổi này là điều đảm bảo O(1) cho mỗi trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 16 

Chúng tôi tính toán k = 1. 

| k | số dư r = n - 15k | dp[r] | tổng k + dp[r] | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | 
| 0 | 16 | dp[16] = 2 | 2 | 
| -1 | không hợp lệ | - | - | 

Tối thiểu là 2, đạt được là 15 + 1 hoặc 10 + 6. 

Điều này cho thấy rằng cả “một đồng xu lớn + dư lượng nhỏ” và “hai đồng xu trung bình” đều có thể tối ưu như nhau và thuật toán nắm bắt chính xác cả hai. 

### Ví dụ 2 

đầu vào: 

n = 20 

k = 1 

| k | phần còn lại r | dp[r] | tổng cộng | 
| --- | --- | --- | --- | 
| 1 | 5 | 3 | 4 | 
| 0 | 20 | dp[20] = 2 | 2 | 

Giải pháp tối ưu là không sử dụng đồng 15 xu nào cả. Thay vào đó, 20 = 10 + 10 chỉ sử dụng hai đồng xu. 

Điều này chứng tỏ tại sao chúng ta phải xem xét điều chỉnh k-1 hoặc k-2. Việc sử dụng hoàn toàn tham lam 15 sẽ là không tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) mỗi lần kiểm tra | Chỉ có 3 ứng viên và tra cứu DP liên tục | 
| Không gian | O(1) | Bảng DP có kích thước cố định (≤ 45) | 

Giải pháp xử lý thoải mái 10^4 trường hợp thử nghiệm vì mỗi truy vấn chỉ thực hiện số học không đổi và một vài tra cứu mảng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    coins = [1, 3, 6, 10, 15]
    MAX_R = 45
    INF = 10**9

    dp = [INF] * (MAX_R + 1)
    dp[0] = 0

    for i in range(1, MAX_R + 1):
        for c in coins:
            if i >= c:
                dp[i] = min(dp[i], dp[i - c] + 1)

    def solve_case(n):
        k = n // 15
        ans = INF
        for dk in [0, -1, -2]:
            kk = k + dk
            if kk < 0:
                continue
            r = n - 15 * kk
            if 0 <= r <= MAX_R:
                ans = min(ans, kk + dp[r])
        return ans

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        out.append(str(solve_case(n)))
    return "\n".join(out)

# provided samples
assert run("""14
1
2
3
5
7
11
12
14
16
17
18
20
98
402931328
""") == """1
2
1
3
2
2
2
3
2
3
2
2
8
26862090"""

# custom cases
assert run("1\n15\n") == "1", "exact large coin"
assert run("1\n30\n") == "2", "two large coins"
assert run("1\n2\n") == "2", "small fallback"
assert run("1\n10\n") == "1", "single medium coin"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 15 | 1 | sử dụng chính xác đồng xu lớn nhất | 
| 30 | 2 | nhiều đồng tiền lớn | 
| 2 | 2 | dự phòng thành 1 xu | 
| 10 | 1 | đồng xu trung bình duy nhất hoạt động tối ưu | 

## Vỏ cạnh 

Với n = 2, không thể sử dụng đồng xu lớn hơn 1. Thuật toán đánh giá k = 0, r = 2 và sử dụng dp[2] = 2, trả về chính xác hai đồng xu 1. 

Với n = 15, k = 1 cho r = 0, vì vậy câu trả lời là 1. Mọi nỗ lực giảm k sẽ làm tăng số lượng đồng xu nhỏ hơn và tạo ra kết quả tồi tệ hơn. 

Với n = 20, thuật toán kiểm tra rõ ràng k = 1 và k = 0. Lựa chọn thứ hai mang lại dp[20] = 2, là kết quả tối ưu. Điều này chứng tỏ tại sao việc tìm kiếm lân cận xung quanh k lại cần thiết để đảm bảo tính chính xác.
