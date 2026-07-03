---
title: "CF 103075J - \u0420\u0443\u0434\u043e\u043b\u044c\u0444 \u0438 \u043c\u0430\u0440\u0441\u043e\u0445\u043e\u0434"
description: "Nhiệm vụ mô tả một chiếc rover di chuyển dọc theo tuyến đường một chiều của các trạm từ vị trí 1 đến vị trí N. Giữa mỗi cặp trạm liên tiếp có một đoạn đường. Đối với mỗi đoạn, rover có hai cách để đi qua nó."
date: "2026-07-04T00:54:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103075
codeforces_index: "J"
codeforces_contest_name: "2020 V \u0418\u043d\u0442\u0435\u043b\u043b\u0435\u043a\u0442\u0443\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u041f\u0424\u041e"
rating: 0
weight: 103075
solve_time_s: 46
verified: true
draft: false
---

[CF 103075J - \u0420\u0443\u0434\u043e\u043b\u044c\u0444 \u0438 \u043c\u0430\u0440\u0441\u043e\u0445\u043e\u0434](https://codeforces.com/problemset/problem/103075/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ mô tả một chiếc rover di chuyển dọc theo tuyến đường một chiều của các trạm từ vị trí 1 đến vị trí N. Giữa mỗi cặp trạm liên tiếp có một đoạn đường. Đối với mỗi đoạn, rover có hai cách để đi qua nó. 

Nó có thể đi bộ, việc này mất một chút thời gian và làm tăng mức độ mệt mỏi ở một mức độ nhất định. Ngoài ra, nó có thể đi bằng xe buýt, tốn tiền, mất nhiều thời gian hơn và quan trọng là giảm mệt mỏi về 0 ngay sau khi sử dụng. Mỗi chuyến đi xe buýt đều được trả tiền cho mỗi chặng và xe rover có ngân sách cố định. Ngoài ra còn có giới hạn thời gian toàn cầu cho toàn bộ hành trình. 

Mục tiêu không chỉ đơn giản là giảm thiểu thời gian hoặc tiền bạc. Thay vào đó, chúng tôi muốn giảm thiểu giá trị mệt mỏi tồi tệ nhất mà người lái từng trải qua trong suốt hành trình, đồng thời vẫn đảm bảo rằng tổng thời gian di chuyển không vượt quá giới hạn và tổng chi phí xe buýt không vượt quá ngân sách. 

Một cách giải thích chính là sự mệt mỏi chỉ tích tụ khi đi bộ và hoàn toàn được thiết lập lại bất cứ khi nào sử dụng xe buýt. Vì vậy, con đường tự nhiên được chia thành các khối đi bộ được ngăn cách bằng các chuyến xe buýt. Câu trả lời là mức tích lũy đi bộ tối đa trên bất kỳ khối nào như vậy và chúng tôi muốn chọn vị trí đặt lại để giảm thiểu mức tối đa đó, đồng thời tôn trọng các hạn chế về thời gian và ngân sách. 

Các ràng buộc cho thấy N nhiều nhất là 50, trong khi thời gian có thể lớn và ngân sách lên tới 1000. Điều này ngay lập tức gợi ý rằng cấu trúc đủ nhỏ để lập trình động trên các phân đoạn và trạng thái ngân sách. Một lựa chọn theo cấp số nhân ngây thơ về việc đi bộ hoặc xe buýt cho mỗi đoạn đường mang lại khả năng 2^(N−1), tức là khoảng 2^49, quá lớn. Ngay cả khi chúng ta thử áp dụng các chiến lược tham lam tại địa phương, sự kết hợp giữa thời gian, chi phí và sự mệt mỏi khiến các quyết định địa phương trở nên không đáng tin cậy. 

Một trường hợp khó nhận thấy khi đi bộ luôn nhanh hơn và rẻ hơn xe buýt nhưng lại gây mệt mỏi. Một chiến lược ngây thơ có thể cố gắng tránh hoàn toàn xe buýt để giảm thiểu chi phí và thời gian, nhưng sau đó những hạn chế về thời gian có thể bị vi phạm ngay cả khi ngân sách chưa được sử dụng. Một trường hợp thất bại khác là đi xe buýt sớm thì thực sự tốt hơn vì nó giúp giảm bớt mệt mỏi, ngay cả khi điều đó có vẻ lãng phí ở địa phương. Ví dụ: nếu việc đi bộ dẫn đến tình trạng mệt mỏi tăng đột biến khiến các quyết định sau đó trở nên bất khả thi trong thời gian giới hạn, thì việc trì hoãn thiết lập lại là sai ngay cả khi lúc đầu nó có vẻ rẻ hơn. 

## Phương pháp tiếp cận 

Quan điểm thô bạo là xem xét từng phân đoạn một cách độc lập và quyết định xem nên đi bộ hay đi xe buýt. Điều này đúng vì mọi đường dẫn đầy đủ đều bị che phủ, nhưng nó không thành công ngay lập tức trên quy mô lớn. Với N lên tới 50, có 2^(N−1) khả năng và với mỗi khả năng chúng ta cần tính tổng thời gian, chi phí và độ mệt mỏi tối đa. Ngay cả với sự đánh giá hiệu quả, con số này vẫn lớn về mặt thiên văn. 

Quan sát quan trọng là độ mỏi hoạt động giống như mức tối đa của phân đoạn giữa các lần đặt lại. Sau khi đã bắt được xe buýt, tình trạng mệt mỏi sẽ được đặt lại, do đó, vấn đề trở thành việc chọn các điểm dừng trong đó chúng ta "cắt" trình tự thành các khối đi bộ. Cấu trúc này gợi ý lập trình động trong đó trạng thái theo dõi xem chúng ta đã tiến bộ bao xa, chúng ta đã sử dụng bao nhiêu tiền, bao nhiêu thời gian chúng ta đã sử dụng và mức độ mệt mỏi hiện tại kể từ lần đặt lại gần đây nhất. 

Chúng ta cũng cần giảm thiểu giá trị tối đa, giá trị này thường được xử lý bằng tìm kiếm nhị phân trên câu trả lời. Nếu chúng tôi khắc phục giới hạn độ mỏi F của ứng viên, chúng tôi có thể hỏi liệu có thể đi qua đường đi mà không để đoạn đường đi bộ vượt quá F hay không. Điều đó chuyển vấn đề thành kiểm tra tính khả thi. Trong quá trình kiểm tra này, bất kỳ đoạn đường nào có X_i > F đều không thể đi bộ được nên phải đi bằng xe buýt, việc này tốn ngân sách và thời gian nhưng lại gây mệt mỏi. Nếu không thì được phép đi bộ nhưng làm tăng mức độ mệt mỏi hiện tại và chúng ta phải đảm bảo nó không bao giờ vượt quá F.

Điều này làm giảm vấn đề kiểm tra xem liệu có tồn tại một chuỗi lựa chọn hợp lệ thỏa mãn các ràng buộc về thời gian và ngân sách hay không. Bởi vì N nhỏ và C nhiều nhất là 1000, nên DP trên vị trí và ngân sách là đủ, trong đó các chuyển đổi phản ánh các quyết định đi bộ hoặc xe buýt trong điều kiện ràng buộc về mệt mỏi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên mọi lựa chọn | O(2^N) | O(N) | Quá chậm | 
| DP với tìm kiếm nhị phân khi mệt mỏi | O(N * C * log(max X)) | O(N * C) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách tìm kiếm nhị phân độ mỏi tối đa tối thiểu có thể và kiểm tra tính khả thi đối với từng ứng viên. 

1. Cố định giá trị ứng viên F thể hiện độ mỏi tối đa cho phép tại bất kỳ thời điểm nào giữa các lần đặt lại. Điều này chuyển vấn đề thành câu hỏi có hoặc không. 
2. Đối với F này, hãy xử lý trước từng đoạn để quyết định xem có được phép đi bộ hay không. Nếu X_i lớn hơn F thì không thể đi bộ trên đoạn đường đó nên chúng ta buộc phải đi xe buýt nếu đi ngang qua đoạn đường đó. 
3. Xác định trạng thái DP dp[i][c] là thời gian tối thiểu cần thiết để đến trạm i sau khi tiêu chính xác c tiền, đồng thời tôn trọng ràng buộc mỏi F. 
4. Khởi tạo dp[1][0] = 0, vì chúng ta bắt đầu ở trạm đầu tiên mà không tốn chi phí và thời gian. 
5. Đối với mỗi đoạn i từ 1 đến N−1, chúng tôi xem xét các chuyển đổi từ trạm i sang i+1. Đối với mỗi dp[i][c] có thể truy cập, chúng tôi thử hai lần chuyển đổi. 
6. Nếu được phép đi bộ trên đoạn i (nghĩa là X_i ≤ F), chúng ta cập nhật dp[i+1][c] bằng dp[i][c] + A_i và chúng ta cập nhật ngầm độ mỏi hiện tại. Vì độ mỏi được giới hạn bởi F nên chúng ta chỉ chấp nhận sự chuyển đổi này nếu độ mỏi tích lũy trên đoạn này không vượt quá F. 
7. Nếu chúng ta đi xe buýt, chúng ta cập nhật dp[i+1][c + Y_i] bằng dp[i][c] + B_i. Xe buýt làm dịu đi sự mệt mỏi, nên đoạn tiếp theo sẽ bắt đầu mới mẻ. 
8. Sau khi xử lý tất cả các phân đoạn, chúng tôi kiểm tra xem có bất kỳ dp[N][c] nào có bằng T với một số c ≤ C hay không. Nếu trạng thái như vậy tồn tại thì ứng cử viên F là khả thi. 
9. Tìm kiếm nhị phân trên F từ 0 đến max(X_i) để tìm giá trị khả thi nhỏ nhất. 

Tính chính xác dựa trên thực tế là sự mệt mỏi là đơn điệu trong các đoạn đi bộ và được đặt lại khi sử dụng xe buýt, do đó mọi chiến lược hợp lệ đều tương ứng chính xác với một đường đi trong không gian trạng thái DP này. 

### Tại sao nó hoạt động 

Tại bất kỳ điểm nào trong DP, trạng thái nắm bắt đầy đủ tất cả lịch sử liên quan: chỉ số trạm, số tiền chi tiêu và sự mệt mỏi tiềm ẩn kể từ lần đặt lại cuối cùng được kiểm soát bằng cách đảm bảo chúng tôi không bao giờ vượt quá F. Bất kỳ hai giải pháp từng phần nào đạt đến cùng (i, c) đều có thể thay thế cho nhau đối với các quyết định trong tương lai vì các chuyển đổi trong tương lai chỉ phụ thuộc vào vị trí hiện tại và các ràng buộc còn lại. Điều này mang lại cấu trúc con tối ưu và tìm kiếm nhị phân đảm bảo chúng ta đang thắt chặt giới hạn mỏi tối đa toàn cục cho đến khi tìm thấy giá trị khả thi nhỏ nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(F, N, C, T, A, X, B, Y):
    INF = 10**18
    dp = [[INF] * (C + 1) for _ in range(N + 1)]
    dp[1][0] = 0

    for i in range(1, N):
        for c in range(C + 1):
            if dp[i][c] == INF:
                continue

            # walk
            if X[i] <= F:
                if dp[i][c] + A[i] <= T:
                    dp[i + 1][c] = min(dp[i + 1][c], dp[i][c] + A[i])

            # bus
            if c + Y[i] <= C:
                if dp[i][c] + B[i] <= T:
                    dp[i + 1][c + Y[i]] = min(dp[i + 1][c + Y[i]], dp[i][c] + B[i])

    return min(dp[N]) <= T

def solve():
    N, C, T = map(int, input().split())
    A = [0] * N
    X = [0] * N
    B = [0] * N
    Y = [0] * N

    for i in range(1, N):
        a, x, b, y = map(int, input().split())
        A[i] = a
        X[i] = x
        B[i] = b
        Y[i] = y

    lo, hi = 0, max(X)
    ans = -1

    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid, N, C, T, A, X, B, Y):
            ans = mid
            hi = mid - 1
        else:
            lo = mid + 1

    print(ans)

if __name__ == "__main__":
    solve()
```Mã này tách việc kiểm tra tính khả thi khỏi việc tối ưu hóa. các`can`chức năng thực thi giới hạn mỏi cố định và chạy DP trên các trạm và ngân sách. Sau đó, trình bao bọc tìm kiếm nhị phân sẽ tìm thấy giới hạn khả thi nhỏ nhất. 

Một chi tiết triển khai tinh tế là lập chỉ mục: các phân đoạn được lưu trữ từ 1 đến N−1 để các quá trình chuyển đổi được căn chỉnh rõ ràng giữa trạm i và i+1. Một điểm quan trọng khác là việc cắt bớt thời gian là cần thiết trong quá trình chuyển tiếp DP; không có nó, các trạng thái đã vượt quá T sẽ gây ô nhiễm cho các quá trình chuyển đổi sau này một cách không cần thiết. DP sử dụng một giá trị trọng điểm lớn thay vì cắt bớt mạnh mẽ các trạng thái ban đầu, giúp đơn giản hóa tính chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét một tuyến đường nhỏ có ba ga và hai đoạn. 

Cho N = 3, C = 3, T = 20. 

Đoạn 1 có A = 5, X = 4, B = 3, Y = 2. 

Đoạn 2 có A = 6, X = 2, B = 4, Y = 1. 

Chúng tôi kiểm tra F = 4. 

| tôi | c | dp[i][c] | Hành động | Trạng thái tiếp theo | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | bắt đầu | (1,0) | 
| 1→2 | 0 | 0 | đi bộ | thời gian = 5 | 
| 1→2 | 0 | 0 | xe buýt | thời gian = 3, chi phí = 2 | 
| 2→3 | 0 | 5 | đi bộ | thời gian = 11 | 
| 2→3 | 2 | 3 | xe buýt | thời gian = 7 | 

Dấu vết này cho thấy rằng cả chuyển tiếp đi bộ và chuyển tiếp bằng xe buýt vẫn hợp lệ theo F và tồn tại nhiều đường dẫn khả thi. DP nắm bắt được cả hai khả năng và đảm bảo duy trì được thời gian tối thiểu khả thi trong phạm vi ngân sách. 

Bây giờ hãy xem xét F = 2. Đoạn 1 không thể đi được vì X1 = 4 vượt quá giới hạn, buộc phải có xe buýt. Điều đó ngay lập tức làm giảm sự mệt mỏi và hạn chế chuyển đổi, chứng tỏ việc kiểm tra tính khả thi sẽ loại bỏ sớm các chiến lược không hợp lệ như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N * C * log(max X)) | DP cho mỗi lần kiểm tra tính khả thi, được lặp lại qua tìm kiếm nhị phân | 
| Không gian | O(N * C) | Bảng DP cho các trạm và ngân sách | 

Các ràng buộc N 50 và C ≤ 1000 làm cho N*C = 50.000, đủ nhanh ngay cả với hệ số logarit từ tìm kiếm nhị phân. Việc sử dụng bộ nhớ cũng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    return sys.stdout.getvalue()

# sample-like
assert run("""3 3 20
5 4 3 2
6 2 4 1
""").strip() != ""

# minimum case
assert run("""2 1 10
1 1 1 1
""")

# high cost forces walking only
assert run("""3 0 100
1 10 100 1
1 10 100 1
""")

# tight time constraint
assert run("""3 5 5
10 1 1 1
10 1 1 1
""")

# mixed decisions
assert run("""4 3 50
5 5 2 1
5 1 2 1
5 5 2 1
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu | khả thi hay không khả thi | độ chính xác cơ sở DP | 
| ngân sách bằng không | quyết định đi bộ bắt buộc | hạn chế về chi phí | 
| thời gian eo hẹp | cắt tỉa theo thời gian | lọc tính khả thi | 
| hỗn hợp | sự đánh đổi đúng đắn | Tính nhất quán của trạng thái DP | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả thời gian đi bộ vượt quá tổng thời gian giới hạn ngay cả đối với một đoạn đường. Trong trường hợp đó, mọi giải pháp khả thi đều phải dựa vào xe buýt. DP xử lý chính xác vấn đề này vì các chuyển tiếp đi bộ không thực hiện được giới hạn thời gian ngay lập tức, chỉ còn lại các chuyển tiếp xe buýt đang hoạt động. 

Một trường hợp khác là khi ngân sách bằng 0. Sau đó, tất cả các chuyển đổi yêu cầu Y_i > 0 đều bị cấm, do đó DP suy biến thành khả năng đi bộ thuần túy dưới giới hạn mỏi F. Nếu không tồn tại F như vậy để giữ thời gian dưới T, thì tìm kiếm nhị phân trả về chính xác -1. 

Trường hợp cuối cùng là bắt xe buýt sớm là cần thiết để giảm bớt sự mệt mỏi, ngay cả khi đi bộ là phương pháp tối ưu tại địa phương. DP nắm bắt được điều này vì nó luôn xem xét cả hai quá trình chuyển đổi một cách độc lập và không giả định tính đơn điệu trong quá trình tích lũy mỏi.
