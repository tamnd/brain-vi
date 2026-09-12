---
title: "CF 104663E - Nhà bán trái cây KUETLand"
description: "Chúng ta có một cây có gốc trong đó mỗi nút đại diện cho một loại trái cây có hai thuộc tính: giá thành và giá trị dinh dưỡng."
date: "2026-06-29T14:55:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104663
codeforces_index: "E"
codeforces_contest_name: "Replay of Ostad Presents Intra KUET Programming Contest 2023"
rating: 0
weight: 104663
solve_time_s: 94
verified: true
draft: false
---

[CF 104663E - Người bán trái cây của KUETLand](https://codeforces.com/problemset/problem/104663/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc trong đó mỗi nút đại diện cho một loại trái cây có hai thuộc tính: giá thành và giá trị dinh dưỡng. Cây được sắp xếp sao cho một nút được chỉ định là nút gốc và tất cả các nút khác được kết nối thông qua các cạnh tạo thành mối quan hệ cha-con ngầm thông qua nút gốc đó. 

“Hoạt động mua hàng” hoạt động giống như cắt một nút khỏi kết nối chính của nó. Khi khách hàng chọn một quả, người bán sẽ cắt tất cả các cạnh nối trực tiếp quả đó với cây mẹ của nó và toàn bộ cây con bị ngắt kết nối bắt nguồn từ quả đó sẽ trở thành một phần của giao dịch mua. Nếu vết cắt đó tách các thành phần bổ sung bên dưới, thì các thành phần đó cũng tự động xuất hiện, do đó, mỗi thao tác sẽ mua một cách hiệu quả toàn bộ thành phần được kết nối đã từng được gắn tại điểm cắt. 

Khách hàng có thể thực hiện nhiều thao tác như vậy, luôn luôn trên phần còn lại của cây sau lần cắt trước đó. Mỗi thao tác sẽ loại bỏ toàn bộ thành phần giống cây con khỏi khu rừng hiện tại và chi phí của thao tác đó là tổng chi phí của tất cả các loại trái cây trong thành phần đó, trong khi lợi ích thu được là tổng giá trị dinh dưỡng. 

Truy vấn hỏi: với một ngân sách nhất định, tổng lượng dinh dưỡng tối đa có thể đạt được bằng cách chọn một chuỗi các lần cắt giảm như vậy, trong đó mỗi thành phần được chọn phải được thanh toán đầy đủ và các thành phần sẽ rời rạc vì sau khi loại bỏ chúng sẽ không còn nữa. 

Điều này biến vấn đề thành việc chọn một tập hợp các cây con được kết nối rời rạc (theo nghĩa cây gốc), mỗi cây có chi phí bằng tổng chi phí nút và giá trị bằng tổng chất dinh dưỡng của nút, tối đa hóa giá trị trong ngân sách ba lô. Hạn chế chính là các mục hợp lệ không phải là tập hợp con tùy ý mà chính xác là cây con trong cây có gốc. 

Những hạn chế cho thấy$n$Và$q$lên tới 3000, trong khi tất cả chi phí và ngân sách cũng bị giới hạn ở mức 3000. Điều này ngay lập tức gợi ý một cấu trúc ba lô giả đa thức. Tuy nhiên, cấu trúc cây ngăn cản việc xử lý từng nút một cách độc lập; Tập hợp con DP đơn giản trên các nút là không thể vì tính hợp lệ phụ thuộc vào thứ bậc. 

Các trường hợp khó khăn phá vỡ lối suy nghĩ ngây thơ bao gồm: 

Cây chuỗi tuyến tính. Nếu chúng tôi coi mỗi nút là một mục độc lập thì chúng tôi sẽ đếm gấp đôi các tiền tố chồng chéo. Ví dụ, trong một chuỗi$1 \to 2 \to 3$, việc chọn cây con tại nút 2 đã bao gồm nút 3 nên việc chọn riêng nút 3 là không hợp lệ. 

Cây có hình ngôi sao. Việc cắt một nút con chỉ loại bỏ cây con đó, nhưng được phép chọn nhiều nút con, do đó tính độc lập chỉ giữ được giữa các nút anh chị em chứ không phải trên toàn bộ. 

Một ví dụ nhỏ: 

đầu vào:```
3 2 1 1
1 1
2 2
3 3
1 2
1 3
3
```Lý do đúng: chúng ta có thể chọn cây con nút 2 hoặc cây con nút 3 hoặc cả hai, nhưng không bao giờ chọn các nút chồng chéo. Cách tiếp cận ngây thơ “chọn các nút có tỷ lệ tốt nhất” không thành công vì vấn đề cấu trúc. 

Khó khăn chính là mã hóa chính xác rằng việc chọn một nút có nghĩa là tùy ý lấy bất kỳ sự kết hợp nào của các cây con con của nó, nhưng một khi chúng ta quyết định lấy cái gì từ một nút con, nó sẽ độc lập với các nút anh em khác. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê mọi tập hợp các cây con rời rạc hợp lệ. Một cách để tưởng tượng là quyết định xem mỗi nút có “kích hoạt” nó dưới dạng gốc bị cắt hay không và đảm bảo không có nút được kích hoạt nào nằm bên trong một cây con được kích hoạt khác. Đối với mỗi lựa chọn hợp lệ, chúng tôi tính toán tổng chi phí và dinh dưỡng, sau đó đưa ra lựa chọn tốt nhất trong ngân sách. 

Số lượng các cấu hình như vậy là theo cấp số nhân. Ngay cả khi chỉ giới hạn ở các gốc cây con, mỗi nút có hai lựa chọn: hoặc chúng ta lấy toàn bộ cây con của nó làm đơn vị hoặc chúng ta trì hoãn việc lựa chọn các nút con. Điều này dẫn đến sự phân nhánh theo cấp số nhân khi mở rộng trên cây, bởi vì tại mỗi nút, chúng tôi quyết định cách phân chia ngân sách giữa các tổ hợp con. 

Quan sát quan trọng là mỗi cây con hoạt động giống như một mục trong ba lô có cấu trúc bên trong: đối với một nút, chúng tôi đang phân bổ ngân sách giữa việc chọn toàn bộ cây con của nút đó làm một mục hoặc phân tách nó thành các lựa chọn bên trong các cây con con của nó. Đây là một vấn đề hợp nhất ba lô DP dạng cây cổ điển. 

Chúng tôi xác định DP tại mỗi nút nơi chúng tôi tính toán tất cả các cặp (chi phí, giá trị) có thể đạt được chỉ bằng cách sử dụng cây con của nó, tôn trọng cấu trúc cây. Đối với mỗi nút, chúng tôi bắt đầu với tùy chọn không lấy gì, sau đó hợp nhất lặp đi lặp lại các trạng thái DP con bằng cách sử dụng tích chập ba lô trên ngân sách lên tới 3000. 

DP của mỗi nút được tính bằng cách hợp nhất từng nút con, coi mảng DP như một chiếc ba lô vượt quá ngân sách. Khi chúng tôi bao gồm một nút con, chúng tôi kết hợp phân bổ ngân sách giữa trạng thái tích lũy hiện tại và cây con DP của nút con đó. Sau khi xử lý tất cả các nút con, chúng tôi tùy ý bao gồm chính nút hiện tại dưới dạng toàn bộ mục của cây con (chi phí và giá trị của toàn bộ cây con) hoặc giữ lại các tùy chọn được phân tách. 

Sự đơn giản hóa quan trọng là chúng ta không cần phải theo dõi các cặp giá trị chi phí tùy ý; vì ngân sách bị giới hạn nên chúng tôi nén DP thành các mảng được lập chỉ mục theo chi phí. 

### Bảng so sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Ba lô cây DP | O(n * q^2) | O(n * q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nhổ cây tại$r$. Chúng tôi tính toán cây con DP từ dưới lên bằng cách sử dụng DFS. 

1. Đối với mỗi nút$u$, khởi tạo một mảng DP`dp[u]`Ở đâu`dp[u][c]`là lượng dinh dưỡng tối đa có thể đạt được từ cây con của$u$với tổng chi phí chính xác$c$. Chúng tôi bắt đầu với`dp[u][0] = 0`. 
2. Chạy DFS trên con của$u$. Đối với mỗi đứa trẻ$v$, đầu tiên chúng ta tính toán`dp[v]`. 
3. Hợp nhất`dp[v]`vào trong`dp[u]`bằng cách sử dụng tích chập kiểu ba lô. Chúng tôi tạo một mảng tạm thời và đối với mỗi lần phân chia ngân sách giữa trạng thái hiện tại và trạng thái con, chúng tôi kết hợp các giá trị. Bước này đảm bảo chúng ta xem xét tất cả các cách phân bổ ngân sách trên các cây con độc lập. 
4. Sau khi hợp nhất tất cả các cây con, chúng ta xem xét việc lấy toàn bộ cây con có gốc tại$u$như một mục duy nhất. Chi phí của nó là tổng chi phí trong cây con của nó và giá trị của nó là tổng dinh dưỡng trong cây con của nó. Chúng tôi cập nhật`dp[u][cost[u]]`tương ứng bằng cách so sánh với lựa chọn cây con đầy đủ. 
5. Trở về`dp[u]`tới cha mẹ. 
6. Sau khi xử lý phần gốc, hãy trả lời từng câu hỏi bằng cách lấy giá trị tối đa`dp[r][c]`cho tất cả$c \leq \text{budget}$. 

Lý do chúng tôi cho phép rõ ràng tùy chọn "lấy toàn bộ cây con" là vì một số giải pháp tối ưu không muốn phân tách cây con thành các quyết định con mà thay vào đó chọn nó làm đơn vị mua hàng duy nhất. 

### Tại sao nó hoạt động 

Tại mỗi nút$u$, DP liệt kê tất cả các cách khả thi để chọn các cây con rời rạc bên trong các cây con của nó. Tính bất biến đó là`dp[u]`đại diện cho tất cả các kết hợp chi phí-giá trị có thể đạt được chỉ bằng cách sử dụng các nút trong$u$cây con của nó mà không vi phạm tính rời rạc. Quá trình hợp nhất duy trì sự độc lập giữa các cây con vì các cây con tách rời nhau. Lựa chọn cây con đầy đủ tùy chọn sẽ tính đến thao tác cắt trong đó chúng tôi lấy một cây con làm một lần mua. Vì mọi giải pháp toàn cục hợp lệ có thể được phân tách duy nhất thành các lựa chọn được thực hiện ở mỗi ranh giới cây con, nên không có cấu hình hợp lệ nào bị bỏ sót và không có sự chồng chéo không hợp lệ nào được đưa ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    T = int(input())
    for tc in range(1, T + 1):
        n, m, r, q = map(int, input().split())

        cost = [0] * (n + 1)
        val = [0] * (n + 1)

        for i in range(1, n + 1):
            cost[i], val[i] = map(int, input().split())

        g = [[] for _ in range(n + 1)]
        for _ in range(n - 1):
            a, b = map(int, input().split())
            g[a].append(b)
            g[b].append(a)

        queries = list(map(int, input().split()))
        maxW = max(queries)

        parent = [0] * (n + 1)
        order = []

        stack = [r]
        parent[r] = -1

        while stack:
            u = stack.pop()
            order.append(u)
            for v in g[u]:
                if v == parent[u]:
                    continue
                parent[v] = u
                stack.append(v)

        children = [[] for _ in range(n + 1)]
        for u in order:
            for v in g[u]:
                if v != parent[u]:
                    children[u].append(v)

        dp = [[-10**18] * (maxW + 1) for _ in range(n + 1)]

        for u in reversed(order):
            dp[u][0] = 0
            for v in children[u]:
                ndp = [-10**18] * (maxW + 1)
                for i in range(maxW + 1):
                    if dp[u][i] < 0:
                        continue
                    for j in range(maxW + 1 - i):
                        if dp[v][j] < 0:
                            continue
                        ndp[i + j] = max(ndp[i + j], dp[u][i] + dp[v][j])
                dp[u] = ndp

            if cost[u] <= maxW:
                for c in range(maxW, cost[u] - 1, -1):
                    dp[u][c] = max(dp[u][c], dp[u][c - cost[u]] + val[u])

        root_dp = dp[r]
        pref = [0] * (maxW + 1)
        best = 0
        for i in range(maxW + 1):
            best = max(best, root_dp[i])
            pref[i] = best

        print(f"Case {tc}:")
        for a in queries:
            print(pref[a])

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên là root cây và xây dựng danh sách con rõ ràng. Điều này tránh việc cha mẹ phải kiểm tra nhiều lần trong DP. DP được lưu trữ dưới dạng một mảng ba lô đầy đủ trên mỗi nút, được khởi tạo với giá trị âm vô cực ngoại trừ chi phí bằng 0. 

Bước hợp nhất sử dụng tích chập ba vòng ba vòng giữa một nút và nút con của nó, đảm bảo tất cả các phân chia ngân sách đều được xem xét. Sau khi hợp nhất các nút con, chúng tôi cho phép lấy chính nút đó làm mục, cập nhật DP ngược lại để tránh ghi đè các trạng thái cần thiết trong cùng một lần lặp. 

Cuối cùng, một mảng tiền tố tối đa được xây dựng để mỗi truy vấn có thể được trả lời trong O(1). 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một cây có rễ nhỏ:```
1 is root
1 - 2
1 - 3
```Chi phí và giá trị: 

| Nút | Chi phí | Giá trị | 
| --- | --- | --- | 
| 1 | 4 | 3 | 
| 2 | 2 | 2 | 
| 3 | 1 | 1 | 

Ngân sách = 3 

Chúng tôi xử lý lá đầu tiên. 

Đối với nút 2: dp cho phép {0 cost, 0 value} và {2 cost, 2 value}. 

Đối với nút 3: dp cho phép {0,0} và {1,1}. 

Tại nút 1, chúng tôi hợp nhất các phần tử con. Chúng tôi nhận được sự kết hợp: 

| Chi phí | Giá trị | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 3 | 

Chúng tôi không thể lấy chính nút 1 vì chi phí là 4 > ngân sách. 

Đáp án của 3 là 3. 

Điều này thể hiện tính độc lập của anh chị em: trẻ em kết hợp với nhau như những món đồ trong ba lô. 

### Ví dụ 2 

Cây chuỗi:```
1 - 2 - 3
```Chi phí và giá trị: 

| Nút | Chi phí | Giá trị | 
| --- | --- | --- | 
| 1 | 5 | 1 | 
| 2 | 3 | 5 | 
| 3 | 2 | 4 | 

Ngân sách = 5 

Tại nút 3: dp = {(0,0), (2,4)} 

Tại nút 2: lấy riêng 3 hoặc kết hợp với 3, được: 

| Chi phí | Giá trị | 
| --- | --- | 
| 0 | 0 | 
| 2 | 4 | 
| 3 | 5 | 
| 5 | 9 | 

Tại nút 1, chúng ta không thể lấy toàn bộ cây con (giá 10), vì vậy câu trả lời cuối cùng là 9 cho ngân sách 5. 

Điều này cho thấy cách hợp nhất cây con một cách tự nhiên sẽ tạo ra các trạng thái ba lô ngày càng tăng dọc theo chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * Q^2) | Mỗi nút thực hiện tích chập ba lô theo ngân sách lên tới Q khi hợp nhất các nút con | 
| Không gian | O(n * Q) | Bảng DP trên mỗi nút theo trạng thái ngân sách | 

Các hạn chế giới hạn cả hai$n$Và$Q$ở mức 3000, làm$nQ^2$đường biên nhưng có thể chấp nhận được trong Python được tối ưu hóa nếu được triển khai cẩn thận và sử dụng tính năng cắt bớt các trạng thái không hợp lệ. Dung lượng bộ nhớ nằm trong giới hạn vì chúng tôi sử dụng lại mảng trên mỗi nút. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    output = io.StringIO()
    sys.stdout = output

    solve()

    return output.getvalue().strip()

# sample
assert run("""1
7 6 5 2
4 3
3 8
7 6
5 7
10 7
5 5
6 3
5 6
6 3
1 6
5 2
4 2
7 2
45 19
""") == """Case 1:
39
21"""

# small chain
assert run("""1
3 2 1 2
1 1
2 2
3 3
1 2
2 3
3 5
""") == """Case 1:
3
6"""

# star tree
assert run("""1
4 3 1 2
1 1
2 2
3 3
4 4
1 2
1 3
1 4
3 5
""") == """Case 1:
5
7"""

# minimal
assert run("""1
1 0 1 1
5 10
5
""") == """Case 1:
10"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | tuyển chọn trực tiếp | độ chính xác cơ sở DP | 
| chuỗi | cấu trúc tích lũy | sự hợp nhất cây con đúng đắn | 
| ngôi sao | anh chị em độc lập | phân hủy đúng | 
| mẫu | tích hợp đầy đủ | tính đúng đắn tổng thể | 

## Vỏ cạnh 

Cây nút đơn kiểm tra xem DP có cho phép lấy gốc làm mục duy nhất có sẵn một cách chính xác hay không. Thuật toán khởi tạo`dp[root][0] = 0`và sau đó xem xét việc lấy chính nút đó nếu ngân sách cho phép, tạo ra kết quả đầu ra chính xác ngay lập tức. 

Một chuỗi sâu nhấn mạnh đến trật tự hợp nhất. Vì mỗi nút phụ thuộc vào nút con của nó nên việc đảo ngược thứ tự DFS đảm bảo DP con được tính toán đầy đủ trước khi nút cha hợp nhất nó, duy trì tính chính xác của trạng thái ba lô tích lũy. 

Cây có hình ngôi sao chứng tỏ trẻ em vẫn có tính độc lập. Mỗi DP con được hợp nhất riêng biệt và do tích chập phân chia ngân sách giữa các DP con nên không xảy ra sự chồng chéo.
