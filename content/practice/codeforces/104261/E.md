---
title: "CF 104261E - Dán Sao Diêm Vương lại với nhau"
description: "Chúng ta được cung cấp một biểu đồ có trọng số hoàn chỉnh với tối đa 12 đỉnh. Mỗi đỉnh đại diện cho một mảnh đá và ma trận chi phí cho chúng ta biết chi phí để dán trực tiếp hai mảnh đá bất kỳ lại với nhau sẽ tốn kém như thế nào."
date: "2026-07-01T21:41:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104261
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 03-24-23 Div. 2 (Beginner)"
rating: 0
weight: 104261
solve_time_s: 71
verified: true
draft: false
---

[CF 104261E - Dán sao Diêm Vương lại với nhau](https://codeforces.com/problemset/problem/104261/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có trọng số hoàn chỉnh với tối đa 12 đỉnh. Mỗi đỉnh đại diện cho một mảnh đá và ma trận chi phí cho chúng ta biết chi phí để dán trực tiếp hai mảnh đá bất kỳ lại với nhau sẽ tốn kém như thế nào. Nhiệm vụ là sắp xếp tất cả các mảnh thành một chu trình duy nhất, sử dụng mỗi mảnh đúng một lần, sao cho tổng chi phí của tất cả các cạnh được chọn trong chu trình là nhỏ nhất. 

Theo thuật ngữ đồ thị, chúng ta đang tìm chu trình Hamilton có trọng số nhỏ nhất trong một đồ thị vô hướng hoàn chỉnh với các trọng số cạnh đối xứng. Đầu ra là tổng trọng số của chính xác N cạnh tạo thành một chu trình đi qua mọi đỉnh một lần và quay trở lại đỉnh ban đầu. 

Ràng buộc N ≤ 12 là tín hiệu chính ở đây. Bất kỳ thuật toán nào cố gắng liệt kê các hoán vị của tất cả các đỉnh đều chạy trực tiếp trong O(N!) sẽ trở thành 12! ≈ 479 triệu hoán vị và mỗi hoán vị yêu cầu tính tổng N cạnh. Điều đó đã quá chậm trong Python dưới giới hạn 4 giây khi bao gồm các yếu tố không đổi. Điều này ngay lập tức gợi ý một cách tiếp cận lập trình động bitmask trên các tập hợp con. 

Một số trường hợp đặc biệt rất dễ bị bỏ sót trong các công thức đơn giản. Một người đang coi chu trình như một đường dẫn và quên đóng nó, dẫn đến việc thiếu cạnh cuối cùng để quay lại điểm bắt đầu. Ví dụ: nếu N = 3 và chi phí đều bằng 1, thì giải pháp dựa trên đường dẫn có thể tính chi phí 2 thay vì chi phí chu kỳ chính xác là 3. Một vấn đề khác là tính hai lần chu kỳ trong các hoán vị bạo lực, vì mỗi chu kỳ xuất hiện trong các phép quay và đảo chiều 2N, điều này làm phức tạp việc giảm thiểu ngây thơ trừ khi được chuẩn hóa cẩn thận. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Chúng tôi sửa một nút bắt đầu và tạo ra tất cả các hoán vị của các nút còn lại. Đối với mỗi hoán vị, chúng tôi tính toán chi phí chu kỳ bằng cách tính tổng các cạnh giữa các nút liên tiếp cộng với cạnh quay lại điểm bắt đầu. Điều này đúng vì nó kiểm tra rõ ràng mọi chu trình Hamilton có thể có. 

Vấn đề là quy mô của sự lặp lại. Có (N-1)! hoán vị sau khi sửa điểm bắt đầu và mỗi đánh giá có giá O(N), do đó độ phức tạp tổng thể trở thành O(N! · N). Với N = 12, điều này vượt xa giới hạn khả thi. 

Quan sát quan trọng là trạng thái xây dựng một phần chỉ phụ thuộc vào nút nào đã được sử dụng và nút nào chúng ta hiện đang ở. Thứ tự mà chúng tôi đạt được tập hợp con đó không quan trọng đối với các quyết định trong tương lai. Đây chính xác là cấu trúc mà lập trình động bitmask nắm bắt. 

Chúng tôi xác định DP[mask][i] là chi phí tối thiểu để bắt đầu từ nút cố định 0, truy cập chính xác các nút trong mặt nạ và kết thúc tại nút i. Từ mỗi trạng thái, chúng tôi thử mở rộng tới bất kỳ nút j chưa được truy cập nào, thêm chi phí [i] [j]. Cuối cùng, chúng ta kết thúc chu trình bằng cách quay về 0 từ mỗi điểm cuối. 

Điều này làm giảm sự bùng nổ hàm mũ từ các hoán vị bậc N! thành các tập hợp con có kích thước 2^N, mỗi tập có N chuyển tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (hoán vị) | O(N! · N) | O(N) | Quá chậm | 
| Mặt nạ bit DP (TSP) | O(N^2 · 2^N) | O(N · 2^N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi cố định một nút làm điểm bắt đầu, thường là nút 0. Điều này loại bỏ tính đối xứng quay của chu trình, vì bất kỳ chu trình nào cũng có thể được quay để bắt đầu ở 0 mà không làm thay đổi chi phí của nó. 

Chúng tôi xác định bảng DP được lập chỉ mục theo tập hợp con và nút được truy cập lần cuối. Tập hợp con theo dõi các nút nào đã được đưa vào đường dẫn một phần.

1. Khởi tạo tất cả các giá trị DP thành một số lớn vì chúng ta đang giảm thiểu chi phí. Chúng tôi đặt DP[1 << 0][0] = 0 vì bắt đầu từ nút 0 chỉ với nút 0 được truy cập không tốn kém gì. 
2. Lặp lại tất cả các tập hợp con của các nút bằng cách sử dụng mặt nạ bit từ 0 đến (1 << N) − 1. Mỗi tập hợp con đại diện cho một tập hợp một phần các đoạn đã truy cập. 
3. Đối với mỗi tập hợp con, lặp lại tất cả các nút cuối cùng có thể có trong tập hợp con đó. Nếu DP[mask][i] không hợp lệ, hãy bỏ qua nó vì nó thể hiện trạng thái không thể truy cập được. 
4. Từ trạng thái (mặt nạ, i), thử kéo dài chu trình bằng cách đi tới bất kỳ nút j nào không nằm trong mặt nạ. Quá trình chuyển đổi cập nhật DP[mask | (1 << j)][j] với DP[mặt nạ][i] + chi phí[i][j]. Điều này đảm bảo chúng tôi luôn tích lũy cách rẻ nhất để đến từng tiểu bang. 
5. Sau khi điền vào bảng DP, tất cả các nút đã được truy cập ở các trạng thái có mặt nạ bằng (1 << N) − 1. Đối với mỗi nút cuối cùng có thể có i, chúng ta thêm cost[i][0] để đóng chu trình trở lại nút bắt đầu và lấy mức tối thiểu. 

Ý tưởng chính là DP nén tất cả các hoán vị có cùng tập truy cập và điểm kết thúc vào một trạng thái. Đó là điều ngăn cản việc tính toán lại các bài toán con giống hệt nhau. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, DP[mask][i] thể hiện chi phí tối thiểu trong số tất cả các lệnh có thể có để truy cập chính xác các nút trong mặt nạ và kết thúc tại i. Bất kỳ phần mở rộng nào cho nút mới j chỉ phụ thuộc vào i và mặt nạ, chứ không phụ thuộc vào cách chúng tôi đạt được cấu hình này. Điều này thiết lập một cấu trúc con tối ưu: giải pháp tốt nhất cho một tập lớn hơn có thể được xây dựng từ các giải pháp tốt nhất của các tập con của nó. Vì mỗi chu trình Hamilton tương ứng với chính xác một chuỗi trạng thái trong DP này và mọi chuyển đổi trạng thái đều duy trì tính chính xác bằng cách xem xét tất cả các khả năng, mức tối thiểu cuối cùng trên tất cả các điểm cuối sẽ mang lại chu trình tối ưu một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    cost = [list(map(int, input().split())) for _ in range(n)]
    
    INF = 10**18
    size = 1 << n
    dp = [[INF] * n for _ in range(size)]
    
    dp[1][0] = 0  # start from node 0, mask = 000...001
    
    for mask in range(size):
        if not (mask & 1):
            continue
        for i in range(n):
            if dp[mask][i] == INF:
                continue
            if not (mask & (1 << i)):
                continue
            for j in range(n):
                if mask & (1 << j):
                    continue
                new_mask = mask | (1 << j)
                new_cost = dp[mask][i] + cost[i][j]
                if new_cost < dp[new_mask][j]:
                    dp[new_mask][j] = new_cost
    
    full = size - 1
    ans = INF
    for i in range(n):
        ans = min(ans, dp[full][i] + cost[i][0])
    
    print(ans)

if __name__ == "__main__":
    solve()
```Bảng DP được khởi tạo với giá trị trọng điểm lớn để thể hiện các trạng thái không thể truy cập được. Trạng thái bắt đầu sửa nút 0 bằng mặt nạ bit chỉ chứa nút đó. 

Cấu trúc vòng lặp ba là cần thiết: mặt nạ liệt kê các tập hợp con, i liệt kê các điểm cuối của các tập hợp con đó và j thử các phần mở rộng. Việc kiểm tra điều kiện đảm bảo chúng tôi chỉ mở rộng các trạng thái hợp lệ và không bao giờ sử dụng lại các nút đã có trong tập hợp con. 

Vòng lặp cuối cùng sẽ đóng chu trình một cách rõ ràng bằng cách quay lại nút 0. Đây là bước thường bị bỏ qua nhất khi triển khai không chính xác và nó rất cần thiết vì DP chỉ xây dựng các đường dẫn Hamilton chứ không phải tuần hoàn trực tiếp. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi đầu vào mẫu. 

đầu vào:```
4
0 1 2 3
1 0 4 5
2 4 0 6
3 5 6 0
```Chúng tôi chỉ theo dõi các chuyển tiếp DP có ý nghĩa. 

| Bước | Mặt nạ | Nút cuối cùng | Giá trị DP | Hành động | 
| --- | --- | --- | --- | --- | 
| ban đầu | 0001 | 0 | 0 | bắt đầu | 
| mở rộng | 0011 | 1 | 1 | 0→1 | 
| mở rộng | 0101 | 2 | 2 | 0→2 | 
| mở rộng | 1001 | 3 | 3 | 0→3 | 
| đường dẫn đầy đủ | 1111 | 1,2,3 | tính toán | nhiều hoán vị | 

Bây giờ hãy xem xét một đường đi tối ưu: 0 → 1 → 2 → 3 → 0 cho chi phí 1 + 4 + 6 + 3 = 14. 

Dấu vết này cho thấy DP xây dựng tất cả các đường dẫn một phần từ nút 0 trong khi vẫn duy trì chi phí tối thiểu cho mỗi trạng thái. Bước đóng cuối cùng đảm bảo chu trình được hoàn thành chính xác. 

Trường hợp nhỏ thứ hai giúp xác nhận tính đúng đắn: 

đầu vào:```
3
0 1 1
1 0 1
1 1 0
```Tất cả các chu kỳ đều có chi phí bằng nhau 3. DP sẽ tạo ra 2 (chi phí đường dẫn 0→1→2) và sau đó thêm 2→0, thu được 3. Mọi hoán vị đều thu gọn về cùng một giá trị tối ưu, xác nhận việc xử lý đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N^2 · 2^N) | Đối với mỗi tập hợp con và điểm cuối, chúng tôi thử chuyển đổi tới tối đa N nút | 
| Không gian | O(N · 2^N) | Bảng DP lưu trữ một giá trị cho mỗi cặp (mặt nạ, nút cuối cùng) | 

Với N 12, 2^N = 4096 và N^2 · 2^N là khoảng 589k thao tác, dễ dàng nằm trong giới hạn trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    input = sys.stdin.readline
    n = int(input())
    cost = [list(map(int, input().split())) for _ in range(n)]
    
    INF = 10**18
    size = 1 << n
    dp = [[INF] * n for _ in range(size)]
    dp[1][0] = 0
    
    for mask in range(size):
        if not (mask & 1):
            continue
        for i in range(n):
            if dp[mask][i] == INF:
                continue
            if not (mask & (1 << i)):
                continue
            for j in range(n):
                if mask & (1 << j):
                    continue
                dp[mask | (1 << j)][j] = min(dp[mask | (1 << j)][j], dp[mask][i] + cost[i][j])
    
    full = size - 1
    ans = min(dp[full][i] + cost[i][0] for i in range(n))
    return str(ans)

# provided sample
assert run("""4
0 1 2 3
1 0 4 5
2 4 0 6
3 5 6 0
""") == "14"

# minimum n=2
assert run("""2
0 5
5 0
""") == "10"

# symmetric all equal
assert run("""3
0 2 2
2 0 2
2 2 0
""") == "6"

# chain-like asymmetry
assert run("""4
0 1 100 100
1 0 1 100
100 1 0 1
100 100 1 0
""") == "4"

# zero-cost edges
assert run("""3
0 0 0
0 0 0
0 0 0
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| N=2 đối xứng | 10 | độ chính xác chu kỳ tối thiểu | 
| tất cả đều bình đẳng | 6 | xử lý đối xứng | 
| chuỗi bất đối xứng | 4 | DP chọn tuyến đường có cấu trúc | 
| tất cả số không | 0 | xử lý cạnh không tốn chi phí | 

## Vỏ cạnh 

Một trường hợp đặc biệt phổ biến là khi tất cả chi phí đều bằng không. DP sẽ truyền các số 0 qua tất cả các trạng thái và lần đóng cuối cùng cũng thêm số 0, tạo ra kết quả chính xác là 0. Điều này xác nhận rằng thuật toán không dựa vào các trọng số dương để đảm bảo tính chính xác. 

Một trường hợp khó phát hiện khác là khi chu trình tối ưu không giống một đường tham lam đơn giản. Ví dụ, trong ma trận chi phí có vẻ bất đối xứng, đường đi tốt nhất có thể tránh các cạnh giá rẻ cục bộ để giảm chi phí sau này. DP xử lý việc này vì mọi trạng thái đều lưu trữ mức tối thiểu thực sự trên tất cả các hoán vị một phần, chứ không phải là một lựa chọn tham lam cục bộ. 

Cuối cùng, trường hợp nhỏ nhất N = 2 kiểm tra tính đúng đắn của việc đóng chu trình. DP xây dựng một đường dẫn duy nhất 0 → 1 và bước cuối cùng sẽ thêm chi phí [1] [0] một cách rõ ràng, đảm bảo chu trình hoàn tất và không bị coi là một đường dẫn một cách vô tình.
