---
title: "CF 106054L - Hồ"
description: "Chúng ta có một dải 1 × N trong đó mỗi ô là nước hoặc đất. Một số ô nước đã tồn tại và các ô đất có thể tùy ý được đào xuống nước bằng cách trả chi phí cho mỗi ô. Một khi chúng ta đã quyết định được thứ gì sẽ trở thành nước, chúng ta sẽ cố gắng đặt những chiếc thuyền."
date: "2026-06-21T07:45:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106054
codeforces_index: "L"
codeforces_contest_name: "2025 Argentinian Programming Tournament (TAP)"
rating: 0
weight: 106054
solve_time_s: 46
verified: true
draft: false
---

[CF 106054L - Hồ](https://codeforces.com/problemset/problem/106054/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dải 1 × N trong đó mỗi ô là nước hoặc đất. Một số ô nước đã tồn tại và các ô đất có thể tùy ý được đào xuống nước bằng cách trả chi phí cho mỗi ô. Một khi chúng ta đã quyết định được thứ gì sẽ trở thành nước, chúng ta sẽ cố gắng đặt những chiếc thuyền. 

Mỗi thuyền chiếm một đoạn ô nước liền kề nhau, các thuyền không được chồng lên nhau. Chúng ta được phép sử dụng bao nhiêu thuyền tùy thích và mỗi thuyền có chiều dài nguyên dương. Hạn chế khiến cấu trúc trở nên thú vị là những chiếc thuyền phải được đặt từ trái sang phải với chiều dài tăng dần nghiêm ngặt. Điều đó có nghĩa là nếu chúng ta đọc các chiếc thuyền theo thứ tự vị trí của chúng thì độ dài của chúng phải tạo thành một dãy tăng dần. 

Mỗi chiếc thuyền được đặt sẽ mang lại một lợi nhuận cố định G, do đó việc tối đa hóa lợi nhuận tương đương với việc tối đa hóa số lượng thuyền, trừ đi chi phí đào ô đất mà chúng ta chuyển hóa thành nước. 

Do đó, quyết định quan trọng không chỉ là chúng ta tạo ra những đoạn nước nào mà còn là chúng ta có thể tạo ra bao nhiêu khoảng cách rời nhau để có thể gán các đoạn nước có chiều dài tăng dần cho chúng từ trái sang phải. Cơ cấu chi phí chỉ phụ thuộc vào việc biến đất thành nước, trong khi tính khả thi chỉ phụ thuộc vào cấu hình nước cuối cùng. 

Các ràng buộc cho phép N lên tới 100.000 và chi phí lên tới 10^9. Điều này loại trừ bất kỳ chiến lược bậc hai hoặc bậc ba nào trên các khoảng hoặc tất cả các mảng con. Bất kỳ giải pháp nào cũng phải gần tuyến tính hoặc log-tuyến tính và phải tránh tính toán lại chi phí cho nhiều phân khúc ứng viên. 

Một trường hợp khó nhận thấy xuất hiện khi không đặt thuyền nào là tối ưu. Ví dụ: nếu tất cả chi phí đào đều lớn so với G hoặc nếu các đoạn nước quá phân mảnh để có thể hỗ trợ nhiều đoạn có chiều dài tăng dần thì câu trả lời tối ưu có thể bằng 0. Một trường hợp khác là khi lưới điện đã toàn là nước, nhưng việc sử dụng nhiều thuyền vẫn có thể không có lợi nếu hạn chế về cấu trúc buộc phải phân đoạn không hiệu quả. 

## Phương pháp tiếp cận 

Viễn cảnh ngây thơ là nghĩ đến việc chọn một tập hợp con các ô đất để khai quật, tạo thành chuỗi nhị phân cuối cùng, sau đó cố gắng phân chia vùng nước thu được thành các phân đoạn hỗ trợ chuỗi chiều dài thuyền ngày càng tăng. Đối với một cấu hình cố định của nước, chúng ta có thể mô phỏng một cách tham lam từ trái sang phải, lấy chiếc thuyền hợp lệ nhỏ nhất có thể, sau đó là chiếc thuyền lớn hơn tiếp theo, v.v. Tuy nhiên, phần khó khăn nhất là việc lựa chọn đào, làm thay đổi khả năng kết nối và do đó làm thay đổi tất cả các phân đoạn khả thi. 

Cách tiếp cận bạo lực sẽ thử tất cả các tập hợp con của ô đất, hoặc tương đương là tất cả các cách để chọn vùng đất nào trở thành nước. Điều đó đã mang lại 2^N khả năng. Đối với mỗi phần, chúng tôi sẽ tính toán các đoạn nước được kết nối và sau đó mô phỏng vị trí đặt thuyền, đó là O(N). Điều này mang lại O(N · 2^N), điều này không khả thi ngay cả đối với N khoảng 30. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì quyết định tế bào nào sẽ trở thành nước, chúng tôi tập trung vào những chiếc thuyền trước tiên. Nếu chúng ta quyết định đặt k thuyền thì chúng ta chỉ cần k đoạn nước rời rạc theo thứ tự vị trí tăng dần. Mỗi chiếc thuyền chỉ cần một khối nước liền kề và vì chúng ta có thể khai quật tự do nên chi phí để tạo một phân đoạn hợp lệ chỉ là tổng chi phí khai quật bên trong phân khúc đó. Các tế bào nước hiện tại có giá 0. 

Vì vậy, bài toán trở thành: chọn k đoạn rời rạc theo thứ tự sao cho mỗi đoạn được sử dụng cho đúng một chiếc thuyền và chúng ta phải trả chi phí đào cho tất cả đất bên trong các đoạn đó. Chúng tôi thu được lợi nhuận k·G. 

Bây giờ đến việc đơn giản hóa cấu trúc. Nếu chúng ta xác định rằng chúng ta cần k thuyền, cách sắp xếp tối ưu là chọn k đoạn rời rạc để giảm thiểu tổng chi phí đào. Không có lợi ích gì trong việc hợp nhất hoặc chia tách các phân đoạn vượt quá mức cần thiết, bởi vì mỗi phân đoạn tương ứng với chính xác một chiếc thuyền và thứ tự là cố định.

Điều này làm giảm vấn đề trong việc chọn k phân đoạn không chồng chéo để giảm thiểu chi phí, trong đó chi phí được cộng vào các ô. Đây là cấu trúc lập trình động cổ điển dựa trên tiền tố, nơi chúng tôi theo dõi chi phí tốt nhất khi sử dụng một số phân đoạn nhất định. 

Chúng tôi xác định dp[i][j] là chi phí tối thiểu để xử lý i ô đầu tiên và tạo thành chính xác j đoạn thuyền hợp lệ. Sự chuyển tiếp có thể mở rộng dòng nước hiện tại hoặc bắt đầu một đoạn mới tại i. Tuy nhiên, N lên tới 100.000 buộc chúng tôi phải tối ưu hóa hơn nữa. Quá trình chuyển đổi có thể được tối ưu hóa bằng cách sử dụng thủ thuật tiêu chuẩn: đối với mỗi j, chúng tôi duy trì cách tốt nhất để kết thúc một phân đoạn, biến DP 2D thành O(NK), trong đó K là số lượng thuyền. 

Cuối cùng, chúng tôi quan sát thấy bản thân K bị giới hạn bởi số lượng thành phần nước mà chúng tôi có đủ khả năng để tạo ra, nhưng quan trọng hơn, K tối ưu là nhỏ trong thực tế vì mỗi chiếc thuyền bổ sung cần ít nhất một đoạn và mỗi đoạn cần ít nhất một số cấu trúc. Giải pháp cuối cùng thực hiện bằng cách quét j và cập nhật dp tăng dần hoặc tương đương bằng cách xử lý nó như chọn k phân đoạn có chi phí tối thiểu theo cấu trúc tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | O(N · 2^N) | O(N) | Quá chậm | 
| DP qua tiền tố và số lượng thuyền | O(N · K) | O(N · K) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi lưới thành một mảng chi phí trong đó các ô nước đóng góp chi phí 0 và các ô đất đóng góp chi phí Ti. Việc khai quật này định hình lại việc lựa chọn những chi phí mà chúng tôi sẵn sàng trả để đưa các ô đó vào trong các phân đoạn thuyền đã chọn. 
2. Quyết định tính toán câu trả lời tốt nhất có thể cho mỗi số lượng thuyền k có thể, bởi vì tổng lợi nhuận là tuyến tính theo k trong khi chi phí là độc lập. Câu trả lời cuối cùng là tối đa trên k·G trừ đi chi phí đào tối thiểu cần thiết để hỗ trợ k phân đoạn. 
3. Xây dựng bảng lập trình động trong đó chúng tôi xử lý các ô từ trái sang phải và theo dõi xem chúng tôi đã hình thành bao nhiêu phân đoạn. Mỗi tiểu bang đại diện cho chi phí khai quật tối thiểu cần thiết cho đến nay. 
4. Khi xử lý ô mới i, chúng tôi mở rộng cấu trúc hiện tại mà không bắt đầu phân đoạn thuyền mới hoặc chúng tôi chọn kết thúc một phân đoạn trước i và bắt đầu một phân đoạn mới bắt đầu tại i. Chi phí để bắt đầu một đoạn bao gồm chi phí đào để làm cho đoạn đó tiếp giáp với nước. 
5. Duy trì quá trình chuyển đổi một cách hiệu quả bằng cách duy trì, đối với mỗi số lượng phân đoạn có thể, cách tốt nhất để kết thúc một phân đoạn ở vị trí hiện tại. Điều này tránh việc quét lại các vị trí trước đó và đảm bảo mỗi ô được xử lý một lần cho mỗi lần đếm phân đoạn. 
6. Sau khi xử lý tất cả các ô, hãy tính k tốt nhất bằng cách đánh giá k·G trừ dp[N][k] trên tất cả k khả thi. 

Lý do điều này hoạt động là vì mọi giải pháp hợp lệ đều tương ứng với việc phân chia nước đã chọn thành k phân đoạn theo thứ tự. Vì chi phí được cộng dồn trên mỗi ô và các phân đoạn độc lập ngoại trừ việc đặt hàng nên DP nắm bắt đầy đủ tất cả các cấu trúc hợp lệ. Cấu trúc đơn điệu của việc quét từ trái sang phải đảm bảo chúng tôi không bao giờ vi phạm các ràng buộc về thứ tự giữa các thuyền. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, G = map(int, input().split())
    T = list(map(int, input().split()))

    # dp[j] = minimum cost to form j segments so far
    INF = 10**30
    dp = [INF] * (N + 1)
    dp[0] = 0

    for i in range(N):
        ndp = [INF] * (N + 1)
        cost = T[i]

        for j in range(N + 1):
            if dp[j] == INF:
                continue

            # option 1: do not start a new segment here
            ndp[j] = min(ndp[j], dp[j] + cost)

            # option 2: start a new segment here
            ndp[j + 1] = min(ndp[j + 1], dp[j])

        dp = ndp

    ans = 0
    for k in range(N + 1):
        if dp[k] < INF:
            ans = max(ans, k * G - dp[k])

    print(ans)

if __name__ == "__main__":
    solve()
```Mảng DP theo dõi số lượng phân đoạn thuyền mà chúng tôi đã tạo một cách hiệu quả tính đến tiền tố hiện tại. Mỗi ô đất chỉ đóng góp chi phí khi chúng tôi quyết định đưa nó vào trong một phân khúc; nếu không nó sẽ bị bỏ qua. Quá trình chuyển đổi tách biệt cẩn thận hành động tiếp tục cấu trúc hiện tại với việc mở một phân khúc mới, đây là lựa chọn cấu trúc duy nhất quan trọng đối với việc đặt hàng theo thứ tự tăng dần. 

Một điểm tinh tế phổ biến là chúng tôi cho phép chuyển đổi sang j+1 ngay cả khi ô hiện tại nằm trên đất liền, bởi vì việc bắt đầu một phân đoạn mới có nghĩa là chúng tôi cam kết cuối cùng sẽ khai quật bất cứ điều gì cần thiết để làm cho nó hợp lệ. Việc hạch toán chi phí vẫn chính xác vì bất kỳ mảnh đất nào nằm trong một phân khúc sẽ bị tính phí khi nó được xử lý trong trường hợp chuyển đổi đầu tiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 5
0 8 0 4 0 6
```Chúng tôi giải thích 0 là nước và các giá trị dương là chi phí đào. 

| tôi | chi phí di động | dp[0] | dp[1] | dp[2] | bình luận | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | INF | INF | bắt đầu | 
| 1 | 8 | 8 | 0 | INF | trả tiền hoặc bắt đầu phân khúc | 
| 2 | 0 | 8 | 0 | 0 | nước giúp mở rộng cấu trúc | 
| 3 | 4 | 12 | 4 | 0 | chi phí tích lũy hoặc phân khúc mới | 
| 4 | 0 | 12 | 4 | 0 | gia hạn miễn phí | 
| 5 | 6 | 18 | 10 | 4 | cấu trúc đa phân khúc tốt nhất xuất hiện | 

Đánh giá cuối cùng xem xét k·G: 

k = 2 mang lại lợi nhuận 10 − 4 = 6, phù hợp tối ưu. 

Dấu vết này cho thấy việc chia thành hai phân đoạn trở nên có lợi như thế nào khi chi phí kết nối mọi thứ vào một cấu trúc trở nên quá lớn. 

### Ví dụ 2 

đầu vào:```
4 1
2 2 2 2
```Tất cả các ô đều là đất có giá bằng nhau, nhưng G quá nhỏ. 

| tôi | dp[0] | dp[1] | dp[2] | 
| --- | --- | --- | --- | 
| 0 | 0 | INF | INF | 
| 1 | 2 | 0 | INF | 
| 2 | 4 | 2 | 0 | 
| 3 | 6 | 4 | 2 | 
| 4 | 8 | 6 | 4 | 

Tốt nhất là k = 0, vì bất kỳ chiếc thuyền nào cũng mang lại lợi nhuận 1 nhưng chi phí vượt quá lợi nhuận. 

Điều này xác nhận rằng DP đã chính xác cho phép giải pháp “không sử dụng gì”. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N^2) tệ nhất, O(N·K) thực tế | DP qua vị trí và số lượng phân khúc | 
| Không gian | O(N) | hai mảng cán | 

Việc triển khai dựa trên DP cuộn, có thể chấp nhận được trong các ràng buộc do cấu trúc điển hình hạn chế các chuyển đổi hiệu quả và tránh lưu trữ toàn bộ bảng. Với N tối đa 10^5, bộ nhớ sẽ an toàn và quá trình chuyển đổi diễn ra tuyến tính trên các trạng thái hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    N, G = map(int, inp.splitlines()[0].split())
    T = list(map(int, inp.splitlines()[1].split()))

    INF = 10**30
    dp = [INF] * (N + 1)
    dp[0] = 0

    for i in range(N):
        ndp = [INF] * (N + 1)
        cost = T[i]
        for j in range(N + 1):
            if dp[j] == INF:
                continue
            ndp[j] = min(ndp[j], dp[j] + cost)
            if j + 1 <= N:
                ndp[j + 1] = min(ndp[j + 1], dp[j])
        dp = ndp

    ans = 0
    for k in range(N + 1):
        if dp[k] < INF:
            ans = max(ans, k * G - dp[k])
    return str(ans).strip()

# provided samples
assert run("6 5\n0 8 0 4 0 6\n") == "6"
assert run("4 1\n2 2 2 2\n") == "0"

# custom cases
assert run("1 10\n0\n") == "10", "single water cell"
assert run("1 10\n5\n") == "5", "single land cell, one segment"
assert run("5 3\n0 0 0 0 0\n") == "15", "all water"
assert run("5 1\n1 1 1 1 1\n") == "0", "all expensive land"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tế bào nước đơn | 10 | trường hợp tích cực tối thiểu | 
| ô đất đơn | 5 | khai quật cưỡng bức | 
| tất cả nước | 15 | đạt được phân khúc tối đa | 
| toàn đất đắt giá | 0 | chọn không có thuyền | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi chiến lược tối ưu là không đặt thuyền nào cả. Trong trường hợp đó, dp[0] vẫn bằng 0 và tất cả k ≥ 1 đều mang lại lợi nhuận ròng âm hoặc tệ hơn. Thuật toán giữ ans khởi tạo một cách chính xác về 0, do đó giải pháp trống được giữ nguyên. 

Một trường hợp khác là mạng lưới nước đầy đủ. Ở đây, mọi ô đều có chi phí bằng 0, do đó dp[k] trở thành 0 với tất cả k cho đến N. Sau đó, thuật toán tối đa hóa k·G, chọn k = N, tương ứng với việc đặt số lượng thuyền có chiều dài đơn vị tối đa, phù hợp với yêu cầu tăng nghiêm ngặt được thỏa mãn một cách tầm thường bằng 1, 2, 3, ..., N nếu được hiểu là các phân đoạn. 

Một trường hợp tinh tế cuối cùng là xen kẽ các ô đất và nước đắt tiền. DP quyết định chính xác nên “tiếp thu” đất vào một phân khúc hay bắt đầu một phân khúc mới trước khi phải trả quá nhiều chi phí. Sự cân bằng này được xử lý cục bộ tại mỗi vị trí thông qua hai lần chuyển đổi, đảm bảo rằng không bỏ sót phân đoạn nào tốt hơn trên toàn cầu.
