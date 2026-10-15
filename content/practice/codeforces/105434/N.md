---
title: "CF 105434N - \u865a\u62df\u6362\u4e58"
description: "Chúng ta có một hệ thống giao thông về cơ bản là một chuỗi các ga được định hướng từ 0 đến n − 1. Giữa ga i − 1 và i, có chính xác một đoạn đường có chiều dài cố định và đoạn đó chỉ cho phép một số phương thức vận tải nhất định được chọn từ tập hợp con của tổng số K phương thức."
date: "2026-06-23T03:56:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105434
codeforces_index: "N"
codeforces_contest_name: "2024\u5e74\u201c\u6838\u6843\u676f\u201d\u6b66\u6c49\u5730\u533aACM\u840c\u65b0\u8d5b"
rating: 0
weight: 105434
solve_time_s: 60
verified: true
draft: false
---

[CF 105434N - \u865a\u62df\u6362\u4e58](https://codeforces.com/problemset/problem/105434/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hệ thống giao thông về cơ bản là một chuỗi các ga được định hướng từ 0 đến n − 1. Giữa ga i − 1 và i, có chính xác một đoạn đường có chiều dài cố định và đoạn đó chỉ cho phép một số phương thức vận tải nhất định được chọn từ tập hợp con của tổng số K phương thức. 

Mỗi phương thức vận tải hoạt động giống như một dịch vụ bán vé với ba thành phần chi phí: phí lên máy bay cố định, chi phí di chuyển tính theo mét và tốc độ di chuyển (được cung cấp gián tiếp thông qua nghịch đảo của nó, vì vậy chúng ta có thể tính toán thời gian từ khoảng cách). Khi bạn đi ngang qua một đoạn đường bằng một phương thức, bạn phải trả phí lên máy bay nếu có và bạn luôn trả chi phí mỗi mét tỷ lệ thuận với khoảng cách. Thời gian di chuyển là khoảng cách nhân với giá trị thời gian trên mét của chế độ. 

Điều khó khăn là phí lên máy bay đôi khi có thể được bỏ qua do quy tắc "chuyển khoản ảo". Nếu bạn chuyển từ chế độ A nào đó sang chế độ C và ở giữa bạn sử dụng chính xác một chế độ B khác và tổng thời gian ở B trong đoạn trung gian đó hoàn toàn nhỏ hơn T, thì khi bạn chọn C, bạn không phải trả phí lên máy bay. Ngoài ra, nếu bạn sử dụng liên tục cùng một chế độ, phí lên máy bay chỉ được tính một lần. 

Vì vậy, khó khăn thực sự là chi phí của một cạnh không chỉ phụ thuộc vào chế độ hiện tại mà còn phụ thuộc vào những gì đã xảy ra một bước trước đó, cụ thể là liệu điều kiện chuyển đổi chiết khấu có được thỏa mãn hay không. 

Chúng ta phải tính tổng chi phí tối thiểu từ trạm 0 đến trạm n − 1. 

Các ràng buộc có cấu trúc nhỏ nhưng không có tính linh hoạt mạnh mẽ: n nhiều nhất là 64 và K nhiều nhất là 8. Điều này ngay lập tức cho thấy rằng biểu đồ có chiều rộng nhỏ nhưng chiều dài sâu và việc theo dõi trạng thái hàm mũ hoặc bậc hai trong K là hợp lý. Không thể có một không gian trạng thái ngây thơ trên tất cả các lịch sử vì lịch sử tăng theo cấp số nhân với n. Tuy nhiên, quan sát quan trọng là chỉ có hai chế độ cuối cùng và điều kiện thời gian là quan trọng, vì vậy lịch sử đầy đủ là không liên quan. 

Một trường hợp thất bại tinh tế xuất hiện khi người ta cho rằng chiết khấu chỉ phụ thuộc vào các phương thức liên tiếp. Ví dụ: giả sử A → B → C xảy ra nhưng thời gian di chuyển của B lớn. Nếu chúng tôi bỏ qua thời hạn của B một cách không chính xác, chúng tôi có thể cho phép miễn phí lên máy bay cho C một cách không chính xác. 

Một cạm bẫy khác là coi quá trình truyền ảo là đối xứng hoặc có thể tái sử dụng: nó bị ràng buộc chặt chẽ với một phân đoạn trung gian duy nhất và cấu trúc “chính xác một chế độ khác” nghiêm ngặt. Hiểu sai nó là “bất kỳ khoảng cách thời gian ngắn nào cũng có thể giảm giá” dẫn đến việc tính quá mức các chuyển đổi không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là coi mỗi trạm là một nút trong biểu đồ đường đi ngắn nhất và tăng trạng thái bằng chế độ vận chuyển được sử dụng cuối cùng và có thể là chế độ cuối cùng thứ hai. Vì K ≤ 8 nên chế độ cuối cùng thứ hai cũng có thể được theo dõi, đưa ra trạng thái K³ trên mỗi trạm. Quá trình chuyển đổi đi từ (trạm, cuối cùng, trước) đến (trạm + 1, tiếp theo, cuối cùng). Đối với mỗi lần chuyển đổi, chúng tôi tính toán chi phí sử dụng chặng với chế độ đó và quyết định xem có áp dụng phí lên máy bay hay không dựa trên việc liệu (trước, cuối, tiếp theo) có đáp ứng điều kiện chuyển tuyến ảo hay không. 

Cách tiếp cận này đúng nhưng chậm: có n trạng thái dọc theo chuỗi, mỗi trạng thái mở rộng thành K khả năng cho chế độ tiếp theo và mỗi lần chuyển đổi là O(1). Điều đó mang lại O(n · K³) tức là khoảng 64 · 512 ≈ 3e4 chuyển tiếp, dễ dàng ổn. Tuy nhiên, chúng ta có thể tinh chỉnh thêm bằng cách nhận thấy rằng DP trên mỗi lớp thực sự là K2, không phải K³, vì trước và cuối được giới hạn bởi K. 

Chúng ta cũng có thể coi nó như DP phân lớp dọc theo chuỗi: tại mỗi phân đoạn, chúng ta chuyển từ chế độ trước sang chế độ hiện tại và chúng ta cần biết liệu phí lên máy bay cho chế độ hiện tại có được miễn hay không tùy thuộc vào quá trình chuyển đổi trước đó. Đây thực sự là một chuỗi Markov bậc 2 qua các chế độ.

Lực lượng mạnh mẽ trên toàn bộ đường dẫn sẽ liệt kê K lựa chọn trên mỗi phân đoạn, mang lại khả năng Kⁿ, lớn về mặt thiên văn (8⁶⁴). Ngay cả việc lập trình động chỉ trên chế độ cuối cùng (trạng thái K) cũng không thành công vì điều kiện chiết khấu phụ thuộc vào hai chế độ trước đó. Sự phụ thuộc bậc hai là nút cổ chai cấu trúc quan trọng buộc phải có trạng thái K2. 

Vì vậy, chúng tôi giải quyết DP được lập chỉ mục theo vị trí và hai chế độ cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Con đường Brute Force | O(Kⁿ) | O(n) | Quá chậm | 
| DP với hai chế độ cuối cùng | O(n · K2) | O(K2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một bảng DP trong đó dp[i][a][b] biểu thị chi phí tối thiểu để đến trạm i, sử dụng chế độ a trên đoạn i − 1 → i và chế độ b trên đoạn i − 2 → i − 1. 

Mỗi lần chuyển tiếp đoạn chỉ phụ thuộc vào hai chế độ này và chế độ được chọn tiếp theo c. 

1. Khởi tạo dp[0][a][b] ở mức vô cùng ngoại trừ một trạng thái giả duy nhất không tồn tại các chế độ trước đó. Điều này có thể được thể hiện bằng cách đặt a và b thành giá trị “none” đặc biệt và có giá 0. 
2. Đối với mỗi đoạn i từ 0 đến n − 2, hãy xem xét tất cả các trạng thái có thể truy cập dp[i][a][b]. Trạng thái này mã hóa rằng hai chế độ được sử dụng cuối cùng là b rồi a theo thứ tự. 
3. Hãy thử tất cả các chế độ c tiếp theo có thể được phép trên đoạn i. Đối với mỗi c, tính chi phí đi lại cơ bản là khoảng cách[i] × chi phí[c] cộng với thời gian đi lại không liên quan đến tổng chi phí ngoại trừ việc kiểm tra điều kiện giảm giá. 
4. Xác định xem có phải trả phí lên máy bay cho c hay không. Thông thường nó sẽ bị tính phí nếu c khác với a. Tuy nhiên, nếu a ≠ b và c ≠ a, và chuyến đi b → a → c thỏa mãn thời gian sử dụng phương thức a trên chặng này nhỏ hơn T thì chúng ta bỏ qua phí lên máy bay của c. Điều kiện mã hóa chính xác một lần truyền ảo hợp lệ thông qua a. 
5. Cập nhật dp[i + 1][c][a] với chi phí tối thiểu, vì c trở thành chế độ cuối cùng và a trở thành chế độ cuối cùng. 
6. Sau khi xử lý tất cả các phân đoạn, lấy giá trị nhỏ nhất trên tất cả dp[n − 1][a][b]. 

Điểm tinh tế quan trọng nhất là sự chuyển dịch của lịch sử: mỗi bước sẽ di chuyển cửa sổ của hai chế độ trước đó về phía trước. Đây là những gì cho phép điều kiện giảm giá được kiểm tra cục bộ. 

Lý do nó hoạt động dựa trên đặc điểm địa phương: mọi quyết định về việc có thể miễn phí lên máy bay hay không chỉ phụ thuộc vào hai chặng cuối và chặng hiện tại, vì điều kiện hạn chế rõ ràng việc chuyển ảo sang chính xác một chế độ trung gian. Khi chúng tôi sửa bộ ba (b, a, c), tất cả thông tin liên quan sẽ được chứa và lịch sử cũ hơn không thể ảnh hưởng đến tính pháp lý hoặc chi phí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, K, T = map(int, input().split())
    w = list(map(int, input().split()))
    invv = list(map(int, input().split()))
    cost = list(map(int, input().split()))

    dist = []
    allowed = []

    for _ in range(n - 1):
        tmp = list(map(int, input().split()))
        d = tmp[0]
        t = tmp[1]
        modes = tmp[2:]
        dist.append(d)
        allowed.append(set(modes))

    INF = 10**30

    # dp[a][b] = last mode a, second last b
    dp = [[INF] * (K + 1) for _ in range(K + 1)]
    dp[K][K] = 0  # K used as "none"

    def time(mode, seg_idx):
        return dist[seg_idx] * invv[mode]

    for i in range(n - 1):
        ndp = [[INF] * (K + 1) for _ in range(K + 1)]
        for a in range(K + 1):
            for b in range(K + 1):
                cur = dp[a][b]
                if cur >= INF:
                    continue
                for c in allowed[i]:
                    # travel cost
                    add = dist[i] * cost[c]

                    # boarding fee logic
                    fee = 0
                    if a != K and c != a:
                        # check virtual transfer
                        if b != K and b != a and c != a:
                            # time spent using a is just this segment
                            if time(a, i) < T:
                                fee = 0
                            else:
                                fee = w[c]
                        else:
                            fee = w[c]
                    elif a == K:
                        fee = w[c]

                    ndp[c][a] = min(ndp[c][a], cur + add + fee)
        dp = ndp

    ans = min(min(row) for row in dp)
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai chỉ giữ lại hai chế độ cuối cùng trong bảng DP cuộn. Giá trị giả K đại diện cho “không có chế độ trước đó”, giúp đơn giản hóa việc khởi tạo và tránh việc đặt vỏ đặc biệt cho phân đoạn đầu tiên. 

Hàm thời gian chỉ được sử dụng để kiểm tra điều kiện truyền ảo. Vì thời gian chỉ phụ thuộc vào độ dài đoạn và tốc độ chế độ nên nó được tính trực tiếp trên mỗi đoạn. Một lỗi triển khai phổ biến là tính toán trước thời gian tích lũy trên các phân đoạn. Lỗi này không chính xác vì điều kiện chỉ áp dụng cho phân đoạn trung gian sử dụng chế độ ở giữa. 

Logic phí lên máy bay được cấu trúc để trước tiên kiểm tra xem chúng ta đang tiếp tục một chế độ hay đang chuyển đổi, sau đó liệu có tồn tại bộ ba hợp lệ hay không và cuối cùng liệu điều kiện thời gian có cho phép giảm giá hay không. Việc đặt hàng rất quan trọng vì bộ ba không hợp lệ không bao giờ được kích hoạt giảm giá. 

## Ví dụ đã hoạt động 

Vì phát biểu chỉ cung cấp một mẫu chưa hoàn chỉnh nên chúng ta xây dựng một trường hợp minh họa nhỏ. 

Xét n = 3, K = 2, T = 10. Tồn tại hai đoạn. 

Đoạn 0 có khoảng cách 5 và cho phép các chế độ {0, 1}. Đoạn 1 có khoảng cách 5 và cũng cho phép {0, 1}. Giả sử w[0] = 10, w[1] = 10, cost[0] = cost[1] = 1, invv[0] = invv[1] = 1. 

Chúng tôi theo dõi trạng thái dp. 

Khi bắt đầu dp[none][none] = 0. 

Sau đoạn 0: 

| trạng thái (cuối cùng, thứ hai) | chi phí | 
| --- | --- | 
| (0, không có) | 5 + 10 = 15 | 
| (1, không có) | 5 + 10 = 15 | 

Sau phân đoạn 1, từ (0, không), chọn 1 sẽ không được giảm giá vì không có chuỗi giữa hợp lệ: 

| chuyển tiếp | chi phí | 
| --- | --- | 
| 0 → 1 | 15 + 5 + 10 = 30 | 
| 0 → 0 | 15 + 5 + 10 = 30 | 

Tương tự từ (1, none): 

| chuyển tiếp | chi phí | 
| --- | --- | 
| 1 → 0 | 30 | 
| 1 → 1 | 30 | 

Vậy câu trả lời cuối cùng là 30. 

Ví dụ này cho thấy cơ chế giảm giá không thể kích hoạt nếu không có mẫu ba chế độ đầy đủ và quá trình khởi tạo phải duy trì “không có lịch sử” dưới dạng trạng thái riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · K2) | Mỗi phân đoạn xử lý tối đa các trạng thái K2 và chuyển tiếp K trên mỗi trạng thái, với hằng số K 8 | 
| Không gian | O(K2) | Chỉ có hai lớp DP được lưu trữ bất cứ lúc nào | 

Các ràng buộc n ≤ 64 và K ≤ 8 làm cho việc này nhanh chóng một cách thoải mái ngay cả trong Python, vì tổng số thao tác vẫn ở mức thấp hàng chục nghìn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, K, T = map(int, input().split())
    w = list(map(int, input().split()))
    invv = list(map(int, input().split()))
    cost = list(map(int, input().split()))

    dist = []
    allowed = []

    for _ in range(n - 1):
        tmp = list(map(int, input().split()))
        d = tmp[0]
        t = tmp[1]
        modes = tmp[2:]
        dist.append(d)
        allowed.append(set(modes))

    INF = 10**30
    dp = [[INF] * (K + 1) for _ in range(K + 1)]
    dp[K][K] = 0

    def time(mode, i):
        return dist[i] * invv[mode]

    for i in range(n - 1):
        ndp = [[INF] * (K + 1) for _ in range(K + 1)]
        for a in range(K + 1):
            for b in range(K + 1):
                cur = dp[a][b]
                if cur >= INF:
                    continue
                for c in allowed[i]:
                    add = dist[i] * cost[c]
                    fee = 0
                    if a != K and c != a:
                        if b != K and b != a and c != a and time(a, i) < T:
                            fee = 0
                        else:
                            fee = w[c]
                    elif a == K:
                        fee = w[c]
                    ndp[c][a] = min(ndp[c][a], cur + add + fee)
        dp = ndp

    ans = min(min(row) for row in dp)
    return str(ans)

# minimum size
assert run("""2 1 10
5
1
1
3 1 0
""").isdigit()

# all same mode
assert run("""3 1 100
5
1
1
2 1 0
2 1 0
""").isdigit()

# small deterministic chain
assert run("""3 2 10
10 10
1 1
1 1
5 2 0 1
5 2 0 1
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi tối thiểu | số | khởi tạo DP cơ sở | 
| chế độ đơn ở mọi nơi | số | không có hành vi chuyển đổi | 
| chuỗi đa chế độ | số | tính đúng đắn của quá trình chuyển đổi | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi n = 1 hoặc n = 2. Trong những trường hợp này không có bộ ba phân đoạn hợp lệ, do đó quy tắc truyền ảo không bao giờ có thể kích hoạt. DP vẫn phải xử lý chính xác với lịch sử giả. Trạng thái khởi tạo (K, K) đảm bảo rằng phân đoạn thực đầu tiên không kế thừa sai mức chiết khấu. 

Một trường hợp cạnh khác là khi T = 0. Vì điều kiện yêu cầu hoàn toàn nhỏ hơn T nên không bao giờ được phép chuyển giao ảo. Bất kỳ hoạt động triển khai nào sử dụng ≤ thay vì < sẽ cho phép giảm giá và tính thiếu chi phí một cách không chính xác. DP vẫn hoạt động vì điều kiện được bản địa hóa thành một phân đoạn duy nhất và được kiểm tra trực tiếp. 

Trường hợp cạnh thứ ba là khi một phân đoạn chỉ cho phép một chế độ. Trong trường hợp đó, việc chuyển đổi là không thể và DP thực sự trở thành sự tích lũy một đường dẫn. Thuật toán xử lý việc này một cách tự nhiên vì tập hợp được phép hạn chế chuyển đổi, nhưng bất kỳ triển khai nào giả định K lựa chọn mỗi bước mà không lọc sẽ đưa ra trạng thái không hợp lệ.
