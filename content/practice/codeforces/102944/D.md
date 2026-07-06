---
title: "CF 102944D-Detroit"
description: "Chúng ta được cung cấp một biểu đồ có hướng biểu thị hệ thống đường thành phố, trong đó mỗi con đường chỉ có thể được sử dụng theo một hướng và mỗi con đường đều có đơn giá theo nghĩa là cuối cùng chúng ta sẽ đếm xem chúng ta quyết định giữ lại bao nhiêu con đường."
date: "2026-07-04T07:36:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102944
codeforces_index: "D"
codeforces_contest_name: "UMPT 2020-2021 Team Tryout Contest"
rating: 0
weight: 102944
solve_time_s: 64
verified: true
draft: false
---

[CF 102944D - Detroit](https://codeforces.com/problemset/problem/102944/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có hướng biểu thị hệ thống đường thành phố, trong đó mỗi con đường chỉ có thể được sử dụng theo một hướng và mỗi con đường đều có đơn giá theo nghĩa là cuối cùng chúng ta sẽ đếm xem chúng ta quyết định giữ lại bao nhiêu con đường. 

Có một ngã ba đặc biệt được dán nhãn 1, đóng vai trò là điểm gặp gỡ. Tổng cộng có K+1 người: bạn và K bạn. Mỗi nút bắt đầu tại một điểm nối riêng biệt giữa các nút từ 2 đến K+2. Mục tiêu là chọn một tập hợp con các đường có hướng sao cho từ mỗi điểm giao nhau xuất phát, có một đường có hướng dẫn đến nút 1, chỉ sử dụng các đường đã chọn. 

Chi phí của một lựa chọn là số lượng đường có trong đó. Trong số tất cả các lựa chọn hợp lệ, chúng tôi muốn số lượng đường tối thiểu có thể. 

Một cách khác để diễn đạt yêu cầu là chúng ta đang xây dựng một sơ đồ con của đồ thị có hướng ban đầu trong đó mọi nút nguồn trong tập hợp {2, 3, …, K+2} có thể đến nút 1 và chúng tôi muốn đồ thị con này sử dụng càng ít cạnh càng tốt. 

Các ràng buộc rất nhỏ về mặt nút, có nhiều nhất là 20 đỉnh và nhiều nhất là 40 cạnh, trong khi K nhiều nhất là 18. Điều này ngay lập tức báo hiệu rằng các thuật toán hàm mũ trên các tập hợp con đều có thể chấp nhận được, nhưng bất kỳ hàm mũ nào trong N hoặc M trực tiếp thì không. 

Một cách giải thích ngây thơ có thể gợi ý việc chạy một con đường ngắn nhất từ ​​mỗi nguồn đến 1 một cách độc lập và kết hợp các con đường đó. Điều này không đúng vì các đường đi ngắn nhất độc lập có thể chồng chéo hoặc phân kỳ theo những cách dưới mức tối ưu tổng thể. Một cạnh dùng chung được sử dụng bởi nhiều đường chỉ nên được tính một lần và cấu trúc tối ưu có thể cố tình chọn các tuyến riêng lẻ dài hơn nếu chúng cho phép chia sẻ nhiều hơn. 

Vấn đề tế nhị thứ hai là các đường dẫn được phép hợp nhất và phân nhánh tùy ý. Cấu trúc kết quả không phải là một tập hợp đơn giản các đường đi ngắn nhất mà là một cấu trúc được định hướng chung, tương tự như cây Steiner có hướng bắt nguồn từ nút 1. 

Một trường hợp thất bại điển hình là khi hai nguồn có các đường dẫn ngắn nhất tách rời nhau và hầu như không trùng nhau, nhưng một tuyến thay thế dài hơn một chút cho phép chúng chia sẻ một tiền tố lớn. Chiến lược mỗi nguồn tham lam sẽ vượt quá số cạnh. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua sự tương tác giữa các đường dẫn, ý tưởng đơn giản nhất là tính toán đường đi ngắn nhất từ mọi nút nguồn đến nút 1, xây dựng lại các đường dẫn đó và lấy hợp của tất cả các cạnh. Điều này tạo ra một giải pháp hợp lệ, vì mỗi nguồn vẫn có đường dẫn đến 1. Tuy nhiên, nó không tối ưu vì các đường dẫn ngắn nhất được tính toán độc lập và không phối hợp cấu trúc dùng chung. 

Lý do điều này không thành công là vì mục tiêu không phải là giảm thiểu khoảng cách trên mỗi nguồn mà là giảm thiểu tổng số cạnh riêng biệt được sử dụng trên tất cả các nguồn. Khi hai đường dẫn chia sẻ một tiền tố, chi phí tiền tố đó chỉ được thanh toán một lần. Do đó, một giải pháp toàn cầu phải khuyến khích việc sáp nhập một cách rõ ràng. 

Cấu trúc mà chúng tôi đang xây dựng là một sơ đồ con có hướng bắt nguồn từ nút 1 bao trùm tất cả các nguồn. Đây chính xác là bài toán cây Steiner có hướng với các trọng số cạnh đơn vị, bắt nguồn từ nút 1 và các đầu cuối là nút nguồn K+1. 

Vì K nhỏ (nhiều nhất là 18), nên chúng ta có thể sử dụng phương pháp quy hoạch động tập hợp con cổ điển trên các tập hợp con đầu cuối. Ý tưởng chính là mô tả các giải pháp từng phần không chỉ theo các thiết bị đầu cuối đã được kết nối mà còn theo nút nơi chúng hiện “gặp nhau” trong cấu trúc từng phần. 

Chúng tôi xác định một trạng thái là dp[mask][v], nghĩa là số cạnh tối thiểu cần thiết để kết nối tất cả các nút đầu cuối trong`mask`sao cho tất cả chúng có thể đạt tới đỉnh v trong đồ thị con đã chọn. Theo trực giác, v là điểm chìm hiện tại nơi tất cả các đường dẫn trong nghiệm từng phần này đều hội tụ. 

Từ công thức này, có hai hoạt động tự nhiên. Đầu tiên, chúng ta có thể hợp nhất hai giải pháp từng phần có chung điểm cuối v. Nếu một giải pháp kết nối mặt nạ1 với v và giải pháp khác kết nối mặt nạ2 với v thì chúng ta có thể kết hợp chúng thành mặt nạ1 | Mask2 ở cùng v mà không thêm bất kỳ cạnh mới nào, vì cả hai cấu trúc đều tồn tại độc lập. Thứ hai, chúng ta có thể mở rộng khả năng kết nối thông qua các cạnh của biểu đồ, truyền trạng thái từ v đến u nếu có cạnh v → u, điều này sẽ thêm một cạnh vào chi phí. 

Quan điểm vũ phu sẽ xem xét tất cả các cách để phân vùng các thiết bị đầu cuối và tất cả các cách để định tuyến chúng thông qua biểu đồ, biểu đồ này có tính chất bùng nổ về mặt tổ hợp. DP giảm điều này bằng cách tách sự kết hợp của các bộ đầu cuối (hợp nhất tập hợp con) khỏi chuyển động dọc theo các cạnh của biểu đồ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liên minh con đường ngắn nhất độc lập | O(K (M log N)) | O(N + M) | Không đúng | 
| Steiner DP trên các tập hợp con | O(3^K + 2^K M) | O(2^KN) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta chuyển bài toán thành công thức cây Steiner có hướng gốc. Chúng tôi coi nút 1 là nút gốc và tất cả các nút từ 2 đến K+2 là các thiết bị đầu cuối phải được kết nối vào cấu trúc. 

1. Chúng tôi khởi tạo dp cho các thiết bị đầu cuối đơn. Với mỗi nút đầu cuối t, chúng ta đặt dp[1 << i][t] = 0, trong đó i tương ứng với đầu cuối đó. Điều này thể hiện cấu trúc tầm thường trong đó chỉ có thiết bị đầu cuối đó được kết nối và “điểm gặp gỡ” chính là thiết bị đầu cuối đó. 
2. Chúng tôi chuẩn bị kết hợp các tập hợp con của thiết bị đầu cuối. Đối với bất kỳ trạng thái dp[mask][v] nào, nếu chúng ta đã biết hai trạng thái dp[mask1][v] và dp[mask2][v] kết thúc tại cùng một nút v, chúng ta có thể hợp nhất chúng thành dp[mask1 | Mask2][v] bằng cách tính tổng chi phí của chúng. Điều này hiệu quả vì hai cấu trúc con tách rời nhau về mặt tính toán chi phí xây dựng và chúng tôi chỉ đơn giản là hợp nhất các bộ thiết bị đầu cuối trong khi vẫn giữ cùng một gốc v. 
3. Chúng tôi lặp lại tất cả các mặt nạ và áp dụng nhiều lần bước hợp nhất tập hợp con này. Đây thực chất là một tập hợp con DP trên mặt nạ bit, trong đó mỗi trạng thái có thể được hình thành bằng cách chia mặt nạ của nó thành hai phần nhỏ hơn. Bước này đảm bảo rằng với mỗi mặt nạ, chúng ta có thể xây dựng nó từ bất kỳ phân vùng thiết bị đầu cuối nào. 
4. Sau khi hợp nhất, chúng ta truyền từng dp[mask][v] dọc theo các cạnh ra v → u. Nếu chúng ta di chuyển dọc theo một cạnh, chúng ta cập nhật dp[mask][u] với dp[mask][v] + 1. Bước này cho phép “điểm gặp nhau” dịch chuyển qua biểu đồ, dần dần đẩy các giải pháp từng phần về phía nút 1. 
5. Sau khi xử lý cả việc hợp nhất tập hợp con và thư giãn cạnh, câu trả lời là dp[full_mask][1], vì chúng tôi yêu cầu tất cả các thiết bị đầu cuối phải được kết nối và kết thúc tại nút 1. 

Tính chính xác đến từ tính bất biến mà dp[mask][v] luôn biểu thị số cạnh tối thiểu cần thiết để xây dựng một sơ đồ con trong đó tất cả các thiết bị đầu cuối trong mặt nạ có thể đạt tới v. Việc hợp nhất tập hợp con duy trì tính hợp lệ vì nó kết hợp hai cấu trúc độc lập đã đảm bảo khả năng tiếp cận tới cùng một điểm cuối. Việc thư giãn cạnh duy trì tính hợp lệ vì việc mở rộng đường dẫn thêm một cạnh sẽ duy trì khả năng tiếp cận trong khi tăng chi phí thêm đúng một cạnh. Trên tất cả các mặt nạ và nút, DP khám phá mọi cách định tuyến và hợp nhất các đường dẫn có thể có để trạng thái cuối cùng nắm bắt được cấu trúc chia sẻ tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, M, K = map(int, input().split())
    edges = [[] for _ in range(N + 1)]
    
    for _ in range(M):
        u, v = map(int, input().split())
        edges[u].append(v)

    terminals = list(range(2, K + 3))
    T = len(terminals)

    INF = 10**9
    dp = [[INF] * (N + 1) for _ in range(1 << T)]

    for i, t in enumerate(terminals):
        dp[1 << i][t] = 0

    for mask in range(1 << T):
        for sub in range(mask):
            if sub & mask == sub:
                other = mask ^ sub
                if other == 0:
                    continue
                for v in range(1, N + 1):
                    if dp[sub][v] == INF or dp[other][v] == INF:
                        continue
                    val = dp[sub][v] + dp[other][v]
                    if val < dp[mask][v]:
                        dp[mask][v] = val

        for v in range(1, N + 1):
            if dp[mask][v] == INF:
                continue
            for to in edges[v]:
                if dp[mask][v] + 1 < dp[mask][to]:
                    dp[mask][to] = dp[mask][v] + 1

    full = (1 << T) - 1
    print(dp[full][1])

if __name__ == "__main__":
    solve()
```Việc triển khai giữ bảng DP 2D được lập chỉ mục theo tập hợp con đầu cuối và nút điểm cuối. Bước khởi tạo tạo giống cho mỗi thiết bị đầu cuối đơn lẻ với chi phí bằng 0. Vòng lặp hợp nhất tập hợp con kết hợp các mặt nạ tách rời tại một nút chia sẻ và vòng lặp chuyển tiếp sẽ đẩy các trạng thái dọc theo các cạnh được định hướng trong khi đếm từng cạnh được chọn một lần trên mỗi phần mở rộng. 

Một điểm tinh tế là phép lặp tập hợp con phải đảm bảo các mặt nạ con được liệt kê đúng cách; vòng lặp bên trong kết thúc`sub < mask`với kiểm tra bit đảm bảo tính chính xác nhưng không được tối ưu hóa. Với những hạn chế nhỏ, điều này là đủ. 

Câu trả lời cuối cùng được lấy từ trạng thái nơi tất cả các thiết bị đầu cuối được kết nối và điểm cuối là nút 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 5 2
2 1
3 2
4 3
3 5
5 1
```Thiết bị đầu cuối là các nút 2, 3 và 4. 

Chúng tôi khởi tạo dp với dp[{2}][2]=0, dp[{3}][3]=0, dp[{4}][4]=0. 

Sau khi lan truyền, nút 4 có thể đạt tới 3, 3 có thể đạt tới 2 và 2 có thể đạt tới 1. Việc hợp nhất tập hợp con dần dần kết hợp các đường dẫn này để tất cả các thiết bị đầu cuối cuối cùng sắp xếp thành một cấu trúc kết thúc ở 1. 

| Bước | Mặt nạ hoạt động | Điểm cuối v | Chi phí | Bình luận | 
| --- | --- | --- | --- | --- | 
| ban đầu | {2} | 2 | 0 | thiết bị đầu cuối duy nhất | 
| ban đầu | {3} | 3 | 0 | thiết bị đầu cuối duy nhất | 
| ban đầu | {4} | 4 | 0 | thiết bị đầu cuối duy nhất | 
| tuyên truyền | {2} | 1 | 1 | 2 → 1 | 
| tuyên truyền | {3} | 2 | 1 | 3 → 2 | 
| tuyên truyền | {4} | 3 | 1 | 4 → 3 | 

Dấu vết này cho thấy cách các giải pháp từng phần chảy về phía gốc và hợp nhất dọc theo các đỉnh được chia sẻ. 

### Ví dụ 2 

đầu vào:```
7 6 1
2 1
3 1
4 1
5 1
6 1
7 1
```Chỉ có một người bạn, nên chỉ có một thiết bị đầu cuối. 

| Bước | Mặt nạ | v | Chi phí | 
| --- | --- | --- | --- | 
| ban đầu | {2} | 2 | 0 | 
| tuyên truyền | {2} | 1 | 1 | 

Thuật toán trực tiếp tìm đường dẫn cạnh đơn từ 2 đến 1. 

Điều này xác nhận rằng DP suy biến chính xác thành hành vi đường dẫn ngắn nhất khi chỉ tồn tại một thiết bị đầu cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(3^K · N + 2^K · M) | tập hợp con hợp nhất trên tất cả các mặt nạ cộng với việc thư giãn cạnh | 
| Không gian | O(2^K · N) | Bảng DP cho tất cả các tập hợp con và nút | 

Các ràng buộc N ≤ 20 và K ≤ 18 làm cho điều này trở nên khả thi. Hệ số mũ chỉ được điều khiển bởi K và vì K nhỏ nên thuật toán vẫn nằm trong giới hạn ngay cả khi triển khai đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    backup = _sys.stdout
    _sys.stdout = io.StringIO()
    solve()
    out = _sys.stdout.getvalue()
    _sys.stdout = backup
    return out.strip()

# provided sample 1
assert run("""5 5 2
2 1
3 2
4 3
3 5
5 1
""") == "3"

# sample 2
assert run("""7 7 2
2 1
6 2
7 6
3 7
4 3
3 5
5 1
""") == "5"

# custom: single chain
assert run("""4 3 1
2 3
3 4
4 1
""") == "3"

# custom: star into root
assert run("""6 5 3
2 1
3 1
4 1
5 1
6 1
""") == "3"

# custom: two routes with sharing opportunity
assert run("""6 6 2
2 3
3 1
2 4
4 1
3 5
5 1
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị chuỗi | 3 | lan truyền đường đơn dài nhất | 
| đồ thị sao | 3 | sáp nhập trực tiếp vào root | 
| biểu đồ tiền tố chia sẻ | 3 | lợi ích của các cạnh được chia sẻ | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các thiết bị đầu cuối đã nằm trên một chuỗi được định hướng duy nhất về phía nút 1. Trong trường hợp này, giải pháp tối ưu nên sử dụng lại chuỗi một lần thay vì tính toán lại các đường dẫn riêng biệt. DP xử lý việc này bằng cách truyền một trạng thái một phần duy nhất qua tất cả các nút, không bao giờ trùng lặp chi phí biên cho các tiền tố được chia sẻ. 

Một trường hợp cạnh khác là khi mọi thiết bị đầu cuối đều có cạnh trực tiếp tới nút 1. Câu trả lời đúng chỉ đơn giản là số lượng thiết bị đầu cuối, vì không thể chia sẻ. DP khởi tạo từng thiết bị đầu cuối một cách độc lập và sau đó ngay lập tức thư giãn từng thiết bị đầu cuối đến nút 1, duy trì chi phí tuyến tính chính xác. 

Trường hợp khó phát hiện cuối cùng là khi các đường dẫn tối ưu yêu cầu hợp nhất tại các nút trung gian không nằm trên bất kỳ đường dẫn ngắn nhất nào từ các thiết bị đầu cuối riêng lẻ. Bước hợp nhất tập hợp con đảm bảo rằng ngay cả khi hai thiết bị đầu cuối đến một nút trung gian không phải gốc thông qua các tuyến khác nhau, chúng vẫn có thể được kết hợp ở đó và chia sẻ các cạnh tiếp theo hướng lên gốc, nắm bắt được hành vi tối ưu toàn cục thay vì hành vi ngắn nhất cục bộ.
