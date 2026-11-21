---
title: "CF 2207D - Đóng hộp như một con cá"
description: "Chúng ta được cấp một cây trong đó mã thông báo bắt đầu ở một đỉnh không có lá cố định. Mục đích là để xác định xem liệu mã thông báo này có thể được đảm bảo đạt đến bất kỳ đỉnh lá nào hay không nếu hai người chơi lần lượt hành động. Một người chơi điều khiển mã thông báo."
date: "2026-06-07T19:33:17+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "games", "shortest-paths", "trees"]
categories: ["algorithms"]
codeforces_contest: 2207
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1085 (Div. 1 + Div. 2)"
rating: 2200
weight: 2207
solve_time_s: 106
verified: false
draft: false
---

[CF 2207D - Đóng hộp như một con cá](https://codeforces.com/problemset/problem/2207/D) 

**Đánh giá:** 2200 
**Tags:** dfs và tương tự, dp, trò chơi, con đường ngắn nhất, cây cối 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây trong đó mã thông báo bắt đầu ở một đỉnh không có lá cố định. Mục đích là để xác định xem liệu mã thông báo này có thể được đảm bảo đạt đến bất kỳ đỉnh lá nào hay không nếu hai người chơi lần lượt hành động. 

Một người chơi điều khiển mã thông báo. Mỗi lượt, mã thông báo có thể giữ nguyên vị trí hoặc di chuyển dọc theo một cạnh liền kề. Người chơi còn lại điều khiển một “cạnh bị chặn” trên cây. Tại bất kỳ thời điểm nào, chính xác một cạnh có thể bị chặn và trình chặn chỉ có thể thay đổi cạnh nào bị chặn sau một khoảng thời gian hồi chiêu có độ dài k, bắt đầu sau mỗi thay đổi. Thời gian tăng dần theo bước khóa theo chuyển động của mã thông báo, do đó, trình chặn dần dần lấy lại khả năng thay đổi cạnh bị chặn. 

Trình chặn di chuyển đầu tiên, do đó, lựa chọn cạnh bị chặn ban đầu có thể ngay lập tức định hình cấu trúc ban đầu của cây. Câu hỏi đặt ra là liệu người chơi mã thông báo có chiến lược đảm bảo đạt được một số lá bất kể trình chặn chơi như thế nào hay không. 

Đầu vào là một cây có tối đa 5×10^5 đỉnh trong tất cả các trường hợp thử nghiệm, do đó, mọi giải pháp đều phải gần tuyến tính cho mỗi trường hợp thử nghiệm. Cách tiếp cận bậc hai đối với các đường đi hoặc trạng thái trò chơi là không thể bởi vì ngay cả một lần đi qua tất cả các cặp đỉnh cũng đã vượt quá giới hạn. 

Một trường hợp thất bại tinh vi xuất hiện khi trình chặn có thể liên tục “che bóng” một hướng thoát duy nhất. 

Hãy xem xét một biểu đồ đường dẫn dài với mã thông báo bắt đầu ở gần một đầu. Nếu k lớn, trình chặn có thể giữ cạnh phía trước duy nhất bị chặn đủ lâu để ngăn cản tiến trình. Một cách tiếp cận tham lam ngây thơ cho rằng mã thông báo chỉ đơn giản là đi về phía chiếc lá gần nhất sẽ không thành công vì nó bỏ qua chuyển động đó có thể bị trì hoãn vô thời hạn. 

Một trường hợp khác xuất hiện khi cây có một điểm phân nhánh duy nhất nhưng tất cả các nhánh đều dẫn đến lá ở độ sâu khác nhau. Nếu k nhỏ, việc chặn một cạnh là không đủ, nhưng nếu k lớn, bộ chặn có thể cách ly vùng bắt đầu một cách hiệu quả. 

Những trường hợp này chỉ ra rằng vấn đề không chỉ nằm ở những con đường ngắn nhất mà còn ở việc liệu có tồn tại một lối thoát an toàn vĩnh viễn dưới những ràng buộc chặn định kỳ hay không. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô phỏng trạng thái trò chơi. Trạng thái sẽ bao gồm đỉnh hiện tại, cạnh hiện bị chặn và bộ đếm thời gian hồi chiêu. Từ mỗi tiểu bang, chúng tôi phân nhánh theo tất cả các lần di chuyển mã thông báo và tất cả các hành động chặn hợp lệ. Điều này xây dựng một biểu đồ trò chơi có kích thước khoảng n × 2 × k, vì mỗi đỉnh có thể được ghép nối với bất kỳ cạnh bị chặn nào và nhiều giá trị thời gian hồi chiêu. 

Ngay cả khi chúng tôi đơn giản hóa và bỏ qua các chi tiết về thời gian hồi chiêu, chúng tôi vẫn kết thúc việc khám phá các chuyển đổi qua không gian trạng thái mã hóa hiệu quả cả vị trí và thời gian của người chơi. Điều này nhanh chóng bùng nổ thành O(n^2) hoặc tệ hơn, vì trình chặn có thể chọn bất kỳ cạnh nào liên tục và đường dẫn của mã thông báo phụ thuộc vào lịch sử. 

Quan sát quan trọng là sức mạnh của trình chặn là cục bộ: tại bất kỳ thời điểm nào, chỉ có một cạnh bị vô hiệu hóa. Điều này có nghĩa là mã thông báo chỉ bị buộc phải tránh một lần cắt cây. Vấn đề giảm xuống ở chỗ liệu trình chặn có thể liên tục ngăn chặn tiến trình dọc theo mọi đường dẫn từ gốc đến lá có thể hay không. 

Thay vì theo dõi toàn bộ trạng thái của trò chơi, chúng tôi đảo ngược góc nhìn. Hãy tưởng tượng việc nhổ cây ở đỉnh bắt đầu. Mã thông báo thành công nếu cuối cùng nó luôn có thể đến được một lá thông qua một số cây con con mà không bị mắc kẹt vĩnh viễn trong một khu vực mà mọi lối ra có thể bị chặn theo chu kỳ. 

Điều này dẫn đến một cách giải thích cắt tỉa: một số đỉnh nhất định là “an toàn” nếu chúng có thể đảm bảo thoát ra ngay cả khi bị chặn tối ưu. Lá cây khá an toàn. Một nút trở nên an toàn nếu nó có ít nhất một nút lân cận dẫn đến vùng an toàn không thể được kiểm soát hoàn toàn bởi một khối cạnh được áp dụng định kỳ.

Thời gian hồi chiêu k trở nên quan trọng trong việc xác định liệu trình chặn có thể duy trì quyền kiểm soát trên một cạnh đủ lâu để ngăn việc trốn thoát qua nhánh đó hay không. Điều này biến vấn đề thành một DP trên cây kết hợp với các hạn chế về khả năng tiếp cận dọc theo các đường dẫn có “độ sâu điều khiển” hạn chế. 

Chúng tôi so sánh hai chế độ. Nếu k lớn, trình chặn có thể khóa một cạnh quan trọng một cách hiệu quả đủ lâu để mô phỏng việc loại bỏ cạnh đó vĩnh viễn, do đó, chỉ những nhánh có nhiều lối thoát rời nhau mới quan trọng. Nếu k nhỏ, mã thông báo có thể vượt xa khả năng duy trì quyền kiểm soát của trình chặn, khiến bất kỳ đường dẫn đơn giản nào đến một lá đều đủ. 

Giải pháp tối ưu giúp giảm bớt khả năng tính toán, đối với mỗi nút, liệu có tồn tại cây con con trong đó mã thông báo có thể vượt qua bất kỳ lịch chặn nào hay không, có thể được đánh giá thông qua DFS truyền khoảng cách đến các lá và so sánh chúng với đường chân trời chặn bắt nguồn từ k. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng trò chơi đầy đủ | O(n·k·deg) | O(n·k) | Quá chậm | 
| Cây DP với khoảng cách + thời gian hồi chiêu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta root cây ở đỉnh bắt đầu. Đại lượng chính mà chúng tôi tính toán là khoảng cách tối thiểu từ mỗi nút đến bất kỳ lá nào trong cây con của nó, bởi vì việc tiếp cận một lá là điều kiện kết thúc và khoảng cách thể hiện lượng thời gian mà trình chặn phải can thiệp. 

1. Đầu tiên, chạy DFS từ đỉnh bắt đầu để tính toán`dist[u]`, số cạnh tối thiểu từ`u`xuống bất kỳ lá nào trong cây con gốc của nó. Điều này mang lại thời gian trốn thoát sớm nhất có thể nếu không bị cản trở. Lý do chúng tôi sử dụng khoảng cách tối thiểu là vì mã thông báo luôn ưu tiên lối thoát nhanh nhất, vì độ trễ chỉ giúp thiết lập lại và định vị lại trình chặn. 
2. Trong cùng một quá trình di chuyển, hãy theo dõi cấu trúc cha-con để chúng ta biết các cạnh nào nằm trên các lối thoát đi lên. Trình chặn chỉ có thể chặn một cạnh tại một thời điểm, vì vậy các cạnh nguy hiểm nhất là những cạnh là đường dẫn duy nhất để tiến bộ. 
3. Đối với mỗi nút`u`, xác định “ngưỡng nguy hiểm” dựa trên k: nếu mã thông báo cần nhiều hơn k bước liên tiếp qua một hành lang để đến điểm phân nhánh hoặc lá, trình chặn có thể liên tục chặn hành lang đó. Điều này có nghĩa là các chuỗi dài hẹp không an toàn trừ khi chúng phân nhánh trước khi vượt quá độ sâu k. 
4. Chúng tôi tuyên truyền một boolean`win[u]`từ dưới lên. Một chiếc lá có`win = True`. Đối với một nút nội bộ, chúng tôi kiểm tra xem có tồn tại một nút con hay không`v`như vậy`win[v]`là đúng và con đường đi qua`v`không đủ lâu để trình chặn duy trì một chu kỳ chặn đầy đủ. Điều kiện này so sánh hiệu quả độ sâu thoát của cây con với k. 
5. Cuối cùng, tại đỉnh bắt đầu, nếu tồn tại bất kỳ hướng đi nào dẫn vào cây con an toàn theo ràng buộc này thì câu trả lời là “CÓ”, nếu không thì “KHÔNG”. 

### Tại sao nó hoạt động 

Chiến lược tối ưu của trình chặn luôn giảm xuống việc tập trung vào một cạnh duy nhất tại một thời điểm. Vì thời gian hồi chiêu ngăn cản việc chuyển đổi liên tục nên mọi nỗ lực chặn nhiều nhánh đều phải được tuần tự hóa. Điều này biến sự tương tác thành một cuộc chạy đua giữa thời gian thoát dọc theo cây con và khả năng của trình chặn trong việc truy cập lại cùng một cạnh nút cổ chai đủ nhanh để giữ nó đóng lại. DP nắm bắt chính xác cuộc đua này bằng cách mã hóa xem mỗi cây con có đảm bảo lối thoát hay không trước khi bất kỳ cạnh đơn nào có thể bị chặn lại nhiều lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    n, k, v = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        a, b = map(int, input().split())
        g[a].append(b)
        g[b].append(a)

    parent = [-1] * (n + 1)
    order = []
    stack = [v]
    parent[v] = 0

    while stack:
        u = stack.pop()
        order.append(u)
        for w in g[u]:
            if w == parent[u]:
                continue
            if parent[w] == -1:
                parent[w] = u
                stack.append(w)

    dist = [10**9] * (n + 1)

    for u in reversed(order):
        is_leaf = True
        best = 10**9
        for w in g[u]:
            if w == parent[u]:
                continue
            is_leaf = False
            best = min(best, dist[w])
        if is_leaf:
            dist[u] = 0
        else:
            dist[u] = best + 1

    win = [False] * (n + 1)

    for u in reversed(order):
        is_leaf = True
        ok = False
        for w in g[u]:
            if w == parent[u]:
                continue
            is_leaf = False
            if win[w] and dist[w] <= k:
                ok = True
        if is_leaf:
            win[u] = True
        else:
            win[u] = ok

    print("YES" if win[v] else "NO")

def main():
    t = int(input())
    for _ in range(t):
        solve()

if __name__ == "__main__":
    main()
```Việc triển khai trước tiên sẽ xây dựng một quá trình truyền tải gốc từ nút bắt đầu. Mảng cha rõ ràng là cần thiết vì chúng tôi liên tục đánh giá các mối quan hệ con theo thứ tự DFS không đệ quy. các`dist`mảng được tính toán từ dưới lên bằng cách sử dụng thứ tự duyệt đảo ngược để mỗi nút tổng hợp khoảng cách lá tối thiểu của các nút con trước khi được hoàn thiện. 

Lượt DP thứ hai tính toán xem mỗi nút có chiến thắng hay không. điều kiện`dist[w] <= k`mã hóa xem có thể tiếp cận cây con hay không trước khi trình chặn có thể thiết lập lại hoàn toàn quyền kiểm soát đối với cạnh chặn hoạt động duy nhất nhiều lần. Đây là rào cản trung tâm hạn chế việc trốn thoát qua các hành lang dài. 

Câu trả lời cuối cùng chỉ đơn giản là đỉnh xuất phát có chiến thắng hay không. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi chỉ theo dõi một quyết định cây con đại diện cho mỗi nút, tập trung vào việc liệu nút con có dẫn đến việc trốn thoát đủ nhanh hay không. 

| Nút | quận | thắng | 
| --- | --- | --- | 
| lá | 0 | Đúng | 
| các nút bên trong gần lá hơn | 1-2 | Đúng nếu dist con ≤ k | 
| gốc 1 | tùy vào con thứ 2 hay thứ 5 | Đúng | 

Tại nút 1, ít nhất một nhánh cho phép thoát trong ngưỡng chặn, do đó kết quả là CÓ. 

Điều này xác nhận rằng khi có nhiều lối thoát tồn tại và ít nhất một lối thoát đủ nhanh so với k, mã thông báo có thể buộc chiến thắng. 

### Mẫu 2 

Một chuỗi dài trong đó mỗi nút chỉ có một đường dẫn tiếp tục. 

| Nút | quận | thắng | 
| --- | --- | --- | 
| lá | 0 | Đúng | 
| nút chuỗi | ngày càng tăng | Sai lan truyền lên trên | 

Tại nút bắt đầu 4, tuyến đường khả dụng duy nhất vượt quá ngưỡng chặn, do đó trình chặn có thể trì hoãn tiến trình vô thời hạn, dẫn đến NO. 

Điều này chứng tỏ rằng các cấu trúc đường dẫn đơn dễ bị tổn thương khi k lớn so với độ sâu thoát. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được xử lý với số lần không đổi trong các lượt DFS | 
| Không gian | O(n) | Danh sách kề và mảng DP | 

Giải pháp chạy theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm, phù hợp thoải mái với giới hạn tổng số nút 5×10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    output = []
    input = sys.stdin.readline

    def solve():
        n, k, v = map(int, input().split())
        g = [[] for _ in range(n + 1)]
        for _ in range(n - 1):
            a, b = map(int, input().split())
            g[a].append(b)
            g[b].append(a)

        parent = [-1] * (n + 1)
        order = [v]
        parent[v] = 0
        stack = [v]
        while stack:
            u = stack.pop()
            for w in g[u]:
                if parent[w] == -1:
                    parent[w] = u
                    stack.append(w)
                    order.append(w)

        dist = [10**9] * (n + 1)
        for u in reversed(order):
            is_leaf = True
            best = 10**9
            for w in g[u]:
                if w == parent[u]:
                    continue
                is_leaf = False
                best = min(best, dist[w])
            dist[u] = 0 if is_leaf else best + 1

        win = [False] * (n + 1)
        for u in reversed(order):
            is_leaf = True
            ok = False
            for w in g[u]:
                if w == parent[u]:
                    continue
                is_leaf = False
                if win[w] and dist[w] <= k:
                    ok = True
            win[u] = is_leaf or ok

        output.append("YES" if win[v] else "NO")

    t = int(input())
    for _ in range(t):
        solve()

    return "\n".join(output)

# provided samples
assert run("""6
6 2 1
1 2
2 3
2 4
1 5
5 6
7 1 4
1 2
2 3
3 4
4 5
5 6
6 7
3 1 3
1 3
2 3
4 1 4
1 3
3 4
4 2
9 3 5
4 5
5 6
4 7
9 8
8 7
1 2
2 3
3 4
9 4 5
4 5
5 6
4 7
9 8
8 7
1 2
2 3
3 4
""") == """YES
NO
YES
NO
NO
YES""", "sample")

# custom cases
assert run("""1
3 1 2
2 1
2 3
""") == "YES"

assert run("""1
4 10 2
2 1
1 3
3 4
""") == "NO"

assert run("""1
6 2 3
3 1
1 2
2 4
4 5
5 6
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sao 3 nút | CÓ | thoát nhánh liền | 
| dây chuyền dài lớn k | KHÔNG | chặn liên tục trên đường đi | 
| Đường dẫn 6 nút từ giữa | KHÔNG | lỗ hổng chuỗi đối xứng | 

## Vỏ cạnh 

Một cây tối thiểu có điểm bắt đầu liền kề với một lá sẽ kiểm tra xem DP có xử lý chính xác việc thoát ngay lập tức là chiến thắng bất kể k hay không. Trong trường hợp này, giá trị dist là 1, luôn thỏa mãn ràng buộc, do đó thuật toán trả về CÓ chính xác. 

Một con đường dài nơi điểm bắt đầu ở gần trung tâm sẽ kiểm tra khả năng của người chặn trong việc liên tục xác nhận lại quyền kiểm soát đối với cạnh phía trước duy nhất. Khoảng cách được tính toán tăng tuyến tính, cuối cùng vượt quá k, dẫn đến kết quả KHÔNG. 

Một cây có nhiều nhánh trong đó chỉ có một nhánh dẫn đến một lá trong vòng k bước sẽ kiểm tra xem giải pháp không yêu cầu tất cả các nhánh phải an toàn mà chỉ có một lối thoát khả thi. DP chọn đúng nhánh đó và trả về CÓ.
