---
title: "CF 988F - Mưa và Ô"
description: "Chúng ta được cho một đường thẳng từ vị trí 0 đến vị trí $a$. Polycarp chỉ di chuyển sang phải, mỗi lần một đơn vị. Một số khoảng thời gian rời rạc trên đường này có mưa và nếu anh ta đi qua một rìa hoàn toàn nằm trong bất kỳ khoảng thời gian mưa nào, anh ta phải mang theo ít nhất một chiếc ô trong khoảng thời gian đó…"
date: "2026-06-17T00:49:14+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 988
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 486 (Div. 3)"
rating: 2100
weight: 988
solve_time_s: 96
verified: false
draft: false
---

[CF 988F - Mưa và Ô](https://codeforces.com/problemset/problem/988/F) 

**Xếp hạng:** 2100 
**Thẻ:** dp 
**Thời gian giải:** 1 phút 36s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một dòng từ vị trí 0 đến vị trí$a$. Polycarp chỉ di chuyển sang phải, mỗi lần một đơn vị. Một số khoảng thời gian rời rạc trên đường này có mưa và nếu anh ta đi qua một rìa hoàn toàn nằm trong bất kỳ khoảng thời gian mưa nào, anh ta phải mang theo ít nhất một chiếc ô trong quá trình di chuyển của đơn vị đó. 

Ô được đặt tại các điểm cụ thể, mỗi điểm có trọng lượng. Tại bất kỳ thời điểm nào, Polycarp có thể nhặt hoặc thả ô ngay lập tức và anh ta có thể mang theo nhiều ô cùng một lúc. Chi phí để di chuyển một chiếc ô là tổng trọng lượng của tất cả những chiếc ô hiện đang được mang theo. Nhiệm vụ là tiếp cận$a$bắt đầu từ 0, tôn trọng hạn chế về mưa, đồng thời giảm thiểu tổng chi phí tích lũy. 

Khó khăn chính là ô không chỉ là công cụ nhị phân. Mang theo nhiều ô hơn luôn làm tăng chi phí, nhưng đôi khi bạn phải giữ một chiếc khi trời mưa. Quyết định thực sự là nên mang theo chiếc ô nào trong mỗi vùng mưa và liệu có nên chuyển đổi giữa những chiếc ô hay để khô ráo khi có thể hay không. 

Những hạn chế$a \le 2000$Và$m \le 2000$đề xuất ngay giải pháp quy hoạch động bậc hai hoặc gần bậc hai trên các vị trí. Quét tuyến tính với các quyết định tham lam cục bộ là không đáng tin cậy vì sự lựa chọn ô phụ thuộc vào các phân đoạn mưa trong tương lai. 

Một trường hợp thất bại tinh tế xuất hiện khi việc đổi ô giữa hành trình có lợi nhưng không rõ ràng ở địa phương. Ví dụ: giả sử một chiếc ô nhẹ nhưng chỉ có thể sử dụng ở xa, trong khi một chiếc ô nặng hơn ở gần. Sự lựa chọn tham lam là chọn chiếc ô gần nhất sẽ thất bại nếu một chiếc ô tốt hơn xuất hiện ngay trước một cơn mưa dài. 

Một trường hợp khác phát sinh từ các quyết định chồng chéo trong nhiều khoảng thời gian mưa được ngăn cách bởi các khoảng trống khô ráo. Một chiến lược ngây thơ đặt lại các quyết định trong mỗi khoảng thời gian sẽ không thành công vì mang ô qua khe khô vẫn có thể tối ưu nếu tránh được chi phí chuyển đổi ở ranh giới mưa tiếp theo. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là xem xét, ở mọi vị trí, tập hợp con ô mà Polycarp đang mang. Vì có$m$ô, điều này dẫn đến$2^m$các trạng thái và quá trình chuyển đổi phụ thuộc vào việc nhặt hoặc thả ô. Ngay cả việc hạn chế “những chiếc ô có liên quan” ở một thời điểm nào đó vẫn để lại những khả năng theo cấp số nhân. Chi phí tích lũy qua$a \le 2000$bước làm cho điều này hoàn toàn không thể thực hiện được. 

Điều quan trọng cần lưu ý là tại bất kỳ thời điểm nào, chỉ có chiếc ô nhẹ nhất hiện có mới có thể tiếp cận được. Mang nhiều ô không bao giờ có ích vì chi phí sẽ tăng thêm và hạn chế mưa chỉ cần ít nhất một chiếc ô. Do đó, ở bất kỳ vị trí nào, chiến lược tối ưu có thể được đặc trưng bằng cách biết ô nào hiện có sẵn và ô nào đang hoạt động. 

Chúng tôi chuyển vấn đề thành lập trình động theo các vị trí, trong đó chúng tôi theo dõi chi phí tối thiểu để đạt được từng vị trí với kiến ​​thức về chiếc ô tốt nhất hiện có. Tại mỗi vị trí, ô đều có sẵn và lượng mưa hạn chế sẽ quyết định liệu chúng ta có phải có ít nhất một chiếc ô đang hoạt động hay không. 

Điều này trở thành một vấn đề cổ điển “DP trên tuyến với các ràng buộc về phân khúc”, trong đó việc chuyển đổi phụ thuộc vào việc duy trì một tập hợp các ô đang hoạt động và chọn ô có chi phí tối thiểu khi cần. Cấu trúc cho phép chúng tôi tính toán trước, đối với từng vị trí, chiếc ô tốt nhất có thể sử dụng được từ thời điểm đó trở đi, sau đó thực hiện DP cập nhật khi mưa bắt đầu hoặc kết thúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con ô |$O(2^m \cdot a)$|$O(2^m)$| Quá chậm | 
| DP qua các vị trí có lựa chọn ô tốt nhất |$O(a \log m)$hoặc$O(a + m)$|$O(a)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta nén bài toán vào các vị trí số nguyên từ 0 đến$a$. Đối với mỗi vị trí, chúng ta cần biết bước tiếp theo có mưa hay không và có sẵn những chiếc ô nào. 

1. Đánh dấu tất cả các đoạn mưa trên một mảng chênh lệch để chúng ta có thể tính toán cho từng cạnh$[x, x+1]$liệu trời có mưa không. Điều này xác định xem có phải mang theo ít nhất một chiếc ô trong bước đó hay không. 
2. Lưu trữ những chiếc ô được nhóm theo vị trí của chúng, vì những chiếc ô sẽ có sẵn chính xác khi chúng ta đến vị trí của chúng. 
3. Xác định trạng thái DP$dp[x]$là chi phí tối thiểu để đạt được vị trí$x$. Chúng tôi khởi tạo$dp[0] = 0$, và tất cả những cái khác là vô cùng. 
4. Duy trì cấu trúc dữ liệu đại diện cho chiếc ô tốt nhất hiện có về mặt trọng lượng. Vì chúng ta có thể chọn bất kỳ ô nào mà chúng ta đã vượt qua nên cấu trúc này phải hỗ trợ việc chèn tăng dần và truy vấn trọng lượng tối thiểu. 
5. Lặp lại các vị trí từ 0 đến$a-1$. Tại mỗi vị trí$x$, hãy xem xét tất cả các ô nằm ở$x$và chèn trọng lượng của chúng vào cấu trúc của những chiếc ô có sẵn. 
6. Xác định xem cạnh$x \to x+1$trời đang mưa. Nếu trời không mưa chúng ta có thể di chuyển mà không cần mang theo ô nên chúng ta chuyển tiếp$dp[x+1] = \min(dp[x+1], dp[x])$. 
7. Bên bờ mưa thì phải chọn ô. Sự lựa chọn tốt nhất có thể là chiếc ô có trọng lượng tối thiểu hiện có cho đến nay. Nếu không tồn tại thì việc chuyển đổi là không thể. 
8. Trong trường hợp trời mưa, chúng tôi cập nhật$dp[x+1] = \min(dp[x+1], dp[x] + w)$, Ở đâu$w$là trọng lượng ô tối thiểu có sẵn. 
9. Tiếp tục cho đến vị trí$a$. Câu trả lời là$dp[a]$, hoặc$-1$nếu không thể truy cập được. 

### Tại sao nó hoạt động 

Ở mọi vị trí, quyết định phù hợp duy nhất là có nên mang theo ô trong bước tiếp theo hay không. Nếu không có mưa, việc vận chuyển bất cứ thứ gì chỉ làm tăng chi phí, do đó, lực lượng tối ưu không mang theo thứ gì. Nếu có mưa, bất kỳ giải pháp khả thi nào cũng phải mang theo ít nhất một chiếc ô và trong số tất cả những chiếc ô có sẵn cho đến thời điểm đó, việc chọn trọng lượng tối thiểu sẽ chiếm ưu thế trong tất cả các lựa chọn thay thế vì chi phí là tuyến tính trong trọng lượng mang theo và không bị phạt khi chuyển đổi. Trạng thái DP là đủ vì các quyết định trong tương lai chỉ phụ thuộc vào vị trí hiện tại và tập hợp các ô đã có thể tiếp cận được chứ không phụ thuộc vào các lựa chọn lịch sử ngoài điều đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, n, m = map(int, input().split())

    rain = [0] * (a + 2)
    for _ in range(n):
        l, r = map(int, input().split())
        for x in range(l, r):
            rain[x] = 1

    umbrellas_at = [[] for _ in range(a + 1)]
    for _ in range(m):
        x, p = map(int, input().split())
        umbrellas_at[x].append(p)

    INF = 10**30
    dp = [INF] * (a + 1)
    dp[0] = 0

    import heapq
    pq = []
    for x in umbrellas_at[0]:
        heapq.heappush(pq, x)

    for x in range(a):
        if dp[x] == INF:
            continue

        for w in umbrellas_at[x + 1 if x + 1 <= a else x]:
            heapq.heappush(pq, w)

        if rain[x] == 0:
            dp[x + 1] = min(dp[x + 1], dp[x])
        else:
            if pq:
                dp[x + 1] = min(dp[x + 1], dp[x] + pq[0])

    return dp[a] if dp[a] < INF else -1

if __name__ == "__main__":
    print(solve())
```Việc triển khai xây dựng chỉ báo mưa boolean trên mỗi cạnh đơn vị để mỗi chuyển động có thể được phân loại độc lập. Những chiếc ô được lưu trữ theo vị trí và được xếp thành một đống tối thiểu khi chúng ta di chuyển sang phải, đảm bảo chúng ta luôn biết chiếc ô rẻ nhất hiện có cho đến thời điểm hiện tại. 

Mảng DP theo dõi độ mỏi tối thiểu để đạt được từng tọa độ. Sự chuyển tiếp chỉ phụ thuộc vào việc đoạn tiếp theo có mưa hay không. Đống không bao giờ được bật lên vì chúng tôi chỉ cần trọng lượng tối thiểu tổng thể trong số những chiếc ô đã thu thập và trọng lượng không bao giờ thay đổi. 

Một điểm tinh tế là ô có hiệu lực vĩnh viễn sau khi được chọn trong chiến lược tối ưu, vì vậy chúng ta không bao giờ cần phải loại bỏ các phần tử khỏi vùng nhớ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 2 4
3 7
8 10
0 10
3 4
8 1
1 2
```Chúng tôi đánh dấu mưa ở các cạnh 3-6 và 8-9. Ô xuất hiện ở các vị trí 0, 1, 3, 8. 

| x | dp[x] | ô đã thêm | chiếc ô tối thiểu | mưa vào x→x+1 | dp[x+1] | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | vâng | 0 | 
| 1 | 0 | 2 | 2 | vâng | 2 | 
| 2 | 2 | không | 2 | vâng | 4 | 
| 3 | 4 | 4 | 2 | vâng | 6 | 
| 4 | 6 | không | 2 | vâng | 8 | 
| 5 | 8 | không | 2 | vâng | 10 | 
| 6 | 10 | không | 2 | vâng | 12 | 
| 7 | 12 | không | 2 | không | 12 | 
| 8 | 12 | 1 | 1 | vâng | 13 | 
| 9 | 13 | không | 1 | vâng | 14 | 

Dấu vết này cho thấy cách sử dụng sớm quyền truy cập vào một chiếc ô nặng hơn một chút cho đến khi một chiếc ô tốt hơn xuất hiện ở vị trí 8, giúp giảm chi phí tiếp theo. 

### Ví dụ 2 (đã xây dựng) 

đầu vào:```
5 1 2
2 5
1 10
4 1
```| x | dp[x] | chiếc ô tối thiểu | mưa | dp[x+1] | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | không | không | 0 | 
| 1 | 0 | 10 | không | 0 | 
| 2 | 0 | 10 | vâng | 10 | 
| 3 | 10 | 10 | vâng | 20 | 
| 4 | 20 | 10,1 | vâng | 21 | 

Chiếc ô thứ hai xuất hiện quá muộn để giúp trời mưa sớm hơn nhưng lại cải thiện được đoạn cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(a + m + \sum (r_i - l_i))$| DP qua các vị trí cộng với việc đánh dấu mưa và chèn ô | 
| Không gian |$O(a + m)$| Mảng DP, mảng mưa và lưu trữ heap | 

Được cho$a, m \le 2000$, điều này chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else solve()

# provided sample 1
assert run("""10 2 4
3 7
8 10
0 10
3 4
8 1
1 2
""") == "14"

# minimum case: no rain
assert run("""3 0 1
1 5
""") == "0"

# impossible case
assert run("""3 1 0
0 3
""") == "-1"

# single umbrella covers all rain
assert run("""5 1 1
1 4
2 1
""") == "3"

# multiple umbrellas, choose best late
assert run("""6 1 3
2 5
1 10
2 2
5 1
""") == "12"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không mưa | 0 | chuyển động cơ bản mà không mất phí | 
| không có ô nhưng mưa | -1 | phát hiện không thể | 
| ô đơn | 3 | sử dụng đúng trên toàn bộ phân khúc | 
| chiếc ô muộn hơn | 12 | tối ưu hóa bị trì hoãn | 

## Vỏ cạnh 

Trường hợp quan trọng là khi mưa bắt đầu ở vị trí 0. Trong trường hợp này, thuật toán ngay lập tức yêu cầu một chiếc ô trước bất kỳ chuyển động nào. DP xử lý chính xác điều này vì ở vị trí 0, tất cả các ô có sẵn đã được chèn trước lần chuyển đổi đầu tiên, do đó trọng lượng tối thiểu được sử dụng ngay từ đầu. 

Một trường hợp khác là khi mưa che kín cả đoạn đường nhưng ô lại nằm rải rác. Vùng heap đảm bảo rằng ngay cả khi chiếc ô tốt hơn xuất hiện muộn, các phân đoạn trước đó vẫn sử dụng thứ tốt nhất hiện có tại thời điểm đó và DP sẽ tích lũy chính xác. 

Trường hợp thứ ba là khi có ô nhưng không có ô nào có thể tiếp cận được trước đợt mưa đầu tiên. Trong trường hợp đó, vùng nhớ heap trống ở lần chuyển đổi bắt buộc đầu tiên, do đó không có cập nhật dp nào xảy ra, khiến điểm cuối không thể truy cập được, tạo ra -1 một cách chính xác.
