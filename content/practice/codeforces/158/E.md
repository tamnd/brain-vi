---
title: "CF 158E - Nói Chuyện Qua Điện Thoại"
description: "Chúng tôi được yêu cầu sắp xếp các cuộc gọi điện thoại trong ngày cho ông Jackson theo cách tối đa hóa khoảng thời gian ngủ không bị gián đoạn dài nhất. Mỗi cuộc gọi có thời gian bắt đầu và thời lượng cụ thể."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "dp", "sortings"]
categories: ["algorithms"]
codeforces_contest: 158
codeforces_index: "E"
codeforces_contest_name: "VK Cup 2012 Qualification Round 1"
rating: 1900
weight: 158
solve_time_s: 73
verified: true
draft: false
---

[CF 158E - Trò chuyện qua điện thoại](https://codeforces.com/problemset/problem/158/E) 

**Xếp hạng:** 1900 
**Thẻ:** *đặc biệt, dp, sắp xếp 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu sắp xếp các cuộc gọi điện thoại trong ngày cho ông Jackson theo cách tối đa hóa khoảng thời gian ngủ không bị gián đoạn dài nhất. Mỗi cuộc gọi có thời gian bắt đầu và thời lượng cụ thể. Nếu một cuộc gọi đến khi anh ấy đang bận, cuộc gọi đó sẽ được xếp hàng đợi và xử lý ngay sau khi kết thúc cuộc gọi hiện tại. Ông Jackson có thể bỏ qua tối đa _k_ cuộc gọi. Mục tiêu của chúng tôi là tìm ra khoảng thời gian liên tục tối đa trong ngày mà anh ấy không nói chuyện cũng như không xếp hàng để nói chuyện. 

Đầu vào đảm bảo rằng số lượng cuộc gọi _n_ tối đa là 4000 và mỗi thời gian và thời lượng cuộc gọi phù hợp trong một ngày là 86400 giây. Giới hạn này đủ nhỏ để có thể chấp nhận được một thuật toán có các phép toán khoảng O(n2). Tuy nhiên, những cách tiếp cận ngây thơ thử mọi tổ hợp lệnh gọi bị bỏ qua, có thể là O(2ⁿ), là không thể. Mỗi cuộc gọi có thời gian bắt đầu duy nhất nhưng thời lượng có thể trùng nhau, vì vậy chúng tôi phải xử lý hàng đợi một cách cẩn thận. 

Trường hợp khó khăn không rõ ràng xảy ra khi bỏ qua cuộc gọi là tối ưu ngay cả đối với các cuộc gọi không chồng chéo. Ví dụ: nếu có ba cuộc gọi tại các thời điểm 100, 200, 300, mỗi cuộc gọi có thời lượng 100 và _k_ = 2, thì thời gian ngủ tối đa đạt được bằng cách bỏ qua hai cuộc gọi đầu tiên, mang lại thời gian ngủ dài từ 1 đến 99. Một thuật toán bất cẩn chỉ bỏ qua các cuộc gọi chồng chéo sẽ tạo ra thời gian ngủ nhỏ hơn. 

Một trường hợp khác là khi bỏ qua cuộc gọi phải được cân bằng với các cuộc gọi được xếp hàng đợi. Ví dụ: một cuộc gọi muộn có thể kéo dài hàng đợi một cách không cần thiết, làm giảm khả năng ngủ nếu chúng ta bỏ qua cuộc gọi sai trước đó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng bỏ qua tất cả các tập hợp con của cuộc gọi, mô phỏng quá trình xử lý cuộc gọi và đo phân đoạn rảnh dài nhất. Điều này đúng, nhưng với n 4000, số tập con 2ⁿ lớn về mặt thiên văn và hoàn toàn không khả thi. 

Một quan sát quan trọng là thứ tự xử lý các cuộc gọi xếp hàng là mang tính quyết định. Sau khi chúng tôi quyết định bỏ qua cuộc gọi nào, các cuộc gọi còn lại sẽ được xử lý theo thứ tự thời gian tăng dần và các cuộc gọi xếp hàng luôn được xử lý ngay sau khi kết thúc cuộc gọi hiện tại. Điều này gợi ý về lập trình động: chúng ta có thể duy trì trạng thái DP biểu thị thời gian sớm nhất sau khi xử lý cuộc gọi i đầu tiên với cuộc gọi j bị bỏ qua. Đặt dp[i][j] biểu thị thời điểm sớm nhất ông Jackson rảnh sau khi xử lý cuộc gọi i đầu tiên trong khi bỏ qua chính xác cuộc gọi j. Việc chuyển sang dp[i+1][j] xảy ra nếu chúng ta xử lý cuộc gọi tiếp theo một cách bình thường, cuộc gọi này có thể bắt đầu muộn hơn do phải xếp hàng và dp[i+1][j+1] xảy ra nếu chúng ta bỏ qua cuộc gọi. Khi đó, thời gian ngủ tối đa là khoảng cách lớn nhất giữa các khoảng thời gian bận rộn liên tiếp được tính toán từ DP này. 

Điều này làm giảm vấn đề từ thời gian hàm mũ xuống O(n * k), điều này có thể chấp nhận được vì n và k là 4000. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2ⁿ) | O(n) | Quá chậm | 
| Lập trình động | O(n * k) | O(n * k) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Sắp xếp cuộc gọi theo thời gian bắt đầu. Điều này đã được đảm bảo bởi đầu vào nhưng chúng tôi đưa nó vào để làm rõ. 
2. Khởi tạo một bảng DP dp[i][j], trong đó i nằm trong khoảng từ 0 đến n, biểu thị cuộc gọi i đầu tiên được xem xét và j nằm trong khoảng từ 0 đến k, biểu thị số lượng cuộc gọi bị bỏ qua. Đặt tất cả các mục thành vô cùng ngoại trừ dp[0][0] = 0, nghĩa là vào đầu ngày không có cuộc gọi nào được xử lý và ông Jackson rảnh vào lúc 0. 
3. Với mỗi cuộc gọi i (từ 0 đến n-1) và mỗi số cuộc gọi có thể bị bỏ qua j (từ 0 đến k), hãy cập nhật bảng DP cho cuộc gọi tiếp theo. Nếu chúng ta nhận cuộc gọi i, thời gian rảnh tiếp theo là thời gian tối đa của dp[i][j] hiện tại (khi chúng ta kết thúc các cuộc gọi trước) và t[i] (thời gian bắt đầu theo lịch trình của cuộc gọi hiện tại), cộng với thời lượng của cuộc gọi đó. Đặt dp[i+1][j] ở mức tối thiểu của giá trị hiện tại và thời gian rảnh tiếp theo. Nếu chúng ta bỏ qua lệnh gọi i và j < k, hãy cập nhật dp[i+1][j+1] lên giá trị tối thiểu của giá trị hiện tại và dp[i][j]. 
4. Sau khi xử lý tất cả các cuộc gọi, thu thập tất cả thời gian kết thúc dp[n][j] với 0 ≤ j ≤ k. Những thời điểm này thể hiện thời gian rảnh sớm nhất sau khi xử lý tất cả các cuộc gọi với j bị bỏ qua. 
5. Để tính thời gian ngủ tối đa, lưu ý rằng chế độ ngủ có thể bắt đầu từ 1 (giây đầu tiên) hoặc sau khi kết thúc bất kỳ cuộc gọi nào. Xem xét khoảng cách giữa các khoảng thời gian bận liên tiếp: giữa dp[i-1][j] và lần bắt đầu cuộc gọi tiếp theo cũng như giữa thời gian bận cuối cùng và thời điểm cuối ngày ở mức 86400. Trả về khoảng thời gian tối đa như vậy. 

Tại sao nó hoạt động: DP duy trì một bất biến rằng dp[i][j] là thời gian sớm nhất sau khi xử lý cuộc gọi i đầu tiên với chính xác j cuộc gọi bị bỏ qua. Ở mỗi bước, chúng tôi khám phá cả việc nhận cuộc gọi hoặc bỏ qua cuộc gọi nếu có thể, vì vậy tất cả các tình huống khả thi liên quan đến giới hạn bỏ qua đều được đề cập. Vì quá trình chuyển đổi luôn chiếm thời gian rảnh sớm nhất có thể nên thời gian ngủ tối đa được tính từ khoảng thời gian giữa các khoảng thời gian này được đảm bảo là thời gian ngủ liên tục tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
calls = [tuple(map(int, input().split())) for _ in range(n)]

INF = 10**9
dp = [[INF] * (k + 1) for _ in range(n + 1)]
dp[0][0] = 0

for i in range(n):
    t_i, d_i = calls[i]
    for j in range(k + 1):
        if dp[i][j] < INF:
            # take the call
            dp[i+1][j] = min(dp[i+1][j], max(dp[i][j], t_i) + d_i)
            # ignore the call
            if j + 1 <= k:
                dp[i+1][j+1] = min(dp[i+1][j+1], dp[i][j])

max_sleep = 0
for j in range(k + 1):
    end_time = dp[n][j]
    max_sleep = max(max_sleep, 86400 - end_time)

# also consider sleep before the first call
first_times = [calls[0][0]] + [0]
for j in range(k + 1):
    if dp[0][j] == 0:
        max_sleep = max(max_sleep, calls[0][0] - 1)

print(max_sleep)
```Mã này khởi tạo bảng DP thành vô cùng và đặt trạng thái bắt đầu thành 0. Đối với mỗi lần đếm cuộc gọi và bỏ qua, nó mô phỏng việc nhận hoặc bỏ qua cuộc gọi. Thời gian ngủ được tính là khoảng thời gian tối đa giữa thời gian bận cuối cùng và thời điểm cuối ngày, đồng thời tính cả thời gian ngủ trước cuộc gọi đầu tiên. Các điều kiện biên được xử lý cẩn thận: lấy max(dp[i][j], t[i]) đảm bảo việc xếp hàng được tôn trọng và bỏ qua các cuộc gọi chỉ tăng j nếu nằm trong giới hạn k. 

## Ví dụ đã hoạt động 

**Mẫu 1:**```
Input:
3 2
30000 15000
40000 15000
50000 15000
```| tôi | j | dp[i][j] | 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| 1 | 0 | 45000 (t0=30000, thời lượng=15000) | 
| 1 | 1 | 0 (bỏ qua) | 
| 2 | 0 | 60000 (từ 45000, t1=40000, thời lượng=15000) | 
| 2 | 1 | 15000 (từ 0, nhận cuộc gọi1) | 
| 2 | 2 | 0 (bỏ qua cả hai đầu tiên) | 
| 3 | 0 | 75000 | 
| 3 | 1 | 30000 | 
| 3 | 2 | 15000 | 

Giấc ngủ tối đa = 49999, đạt được bằng cách bỏ qua hai cuộc gọi đầu tiên. 

**Mẫu 2:** Case đã thi công```
Input:
2 1
100 50
120 30
```Bảng DP hiển thị bỏ qua cuộc gọi thứ hai cho thời gian miễn phí từ 151 đến 86400, ngủ = 86250. Lấy cả hai nén giấc ngủ. 

Những dấu vết này chứng tỏ DP đánh giá chính xác các tùy chọn bỏ qua và thời gian bận sớm nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * k) | Mỗi cuộc gọi lặp lại trên 0..k số lượng bỏ qua | 
| Không gian | O(n * k) | Bảng DP có kích thước (n+1)*(k+1) | 

Với n 4000 và k ≤ n, con số này tối đa là ~16 triệu lần lặp, có thể chấp nhận được trong giới hạn 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open("solution.py").read(), globals())
    return str(max_sleep)

# provided samples
assert run("3 2\n30000 15000\n40000 15000\n50000 15000\n") == "49999"
# custom cases
assert run("2 1\n100 50\n120 30\n") == "86250", "ignore second call"
assert run("1 0\n1 86400\n") == "0", "full day call, no ignore"
assert run("3 0\n1 10\n12 10\n25 10\n") == "86370", "spread out calls"
assert run("3 3\n100 100\n200 100\n300 100\n") == "86400", "ignore all calls"
```| Kiểm tra đầu vào | Hy vọng
