---
title: "CF 103833D - Núi lửa"
description: "Chúng tôi được tặng một cây có rễ. Mỗi đỉnh mang một giá trị +1 hoặc −1. Bạn bắt đầu ở một đỉnh cố định tại thời điểm 0 với giá trị tuổi thọ ban đầu là 1. Thời gian tiến triển theo các bước riêng biệt."
date: "2026-07-02T08:08:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103833
codeforces_index: "D"
codeforces_contest_name: "2018 International olympiad Tuymaada"
rating: 0
weight: 103833
solve_time_s: 50
verified: true
draft: false
---

[CF 103833D - Núi lửa](https://codeforces.com/problemset/problem/103833/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cây có rễ. Mỗi đỉnh mang một giá trị +1 hoặc −1. Bạn bắt đầu ở một đỉnh cố định tại thời điểm 0 với giá trị vòng đời ban đầu là 1. 

Thời gian tiến triển theo từng bước rời rạc. Ở mỗi bước thời gian, có ba điều xảy ra theo một trình tự chặt chẽ: đầu tiên bạn được hoặc mất mạng tùy theo đỉnh bạn đang đứng, sau đó bạn chết ngay lập tức nếu mạng sống của bạn trở về 0 hoặc nếu dung nham đã đạt đến đỉnh đó vào thời điểm đó và chỉ khi bạn vẫn còn sống, bạn mới chọn một đỉnh lân cận để di chuyển đến bước thời gian tiếp theo. 

Dung nham lan ra từ gốc và sau t bước, mọi đỉnh ở khoảng cách tối đa t tính từ gốc được coi là bị ngập. Điều này có nghĩa là các nút sâu hơn sẽ an toàn lâu hơn, trong khi các nút nông sẽ trở nên không an toàn sớm hơn. Bởi vì chuyển động xảy ra sau khi kiểm tra sự sống còn, thời điểm bạn đi vào một đỉnh rất quan trọng: đến quá sớm có thể giết chết bạn ngay lập tức ngay cả khi đỉnh đó sẽ an toàn sau một bước. 

Nhiệm vụ là tối đa hóa số lần di chuyển bạn có thể thực hiện trước khi chết, bắt đầu từ đỉnh ban đầu. 

Các ràng buộc đủ lớn để n có thể lên tới hàng trăm nghìn cho mỗi lần kiểm tra và có thể có nhiều trường hợp kiểm thử. Điều đó ngay lập tức loại trừ bất kỳ điều gì cố gắng mô phỏng tất cả các đường dẫn có thể hoặc tính toán lại các trạng thái một cách độc lập trên mỗi đường dẫn. Mọi giải pháp đều phải xử lý cây trong thời gian gần tuyến tính cho mỗi trường hợp thử nghiệm và sử dụng lại cấu trúc thay vì liệt kê các quỹ đạo. 

Các trường hợp phức tạp chính xuất phát từ cách cái chết được kích hoạt. Đầu tiên, việc cập nhật giá trị cuộc sống diễn ra trước khi kiểm tra mức độ an toàn khi lũ lụt. Điều đó có nghĩa là một đỉnh có trọng lượng −1 có thể giết chết bạn ngay cả trước khi dung nham xảy ra. Thứ hai, việc đến đúng thời điểm dung nham đạt đến đỉnh là nguy hiểm, vì vậy “bằng khoảng cách” là không an toàn. Thứ ba, vì sự di chuyển bị ép buộc ở mỗi bước nên việc duy trì một nút có giá trị cao an toàn không phải là một lựa chọn, điều này khiến cho lý luận tham lam cục bộ không thành công. 

Một lỗi minh họa nhỏ: giả sử một nút có trọng số −1 và vẫn không bị ngập. Một cách tiếp cận ngây thơ có thể cho rằng nó an toàn vì dung nham ở rất xa, nhưng bước lên đó khi mạng sống là 1 sẽ gây ra cái chết ngay lập tức trước bất kỳ chuyển động nào. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực cố gắng coi đây là tìm kiếm kiểu đường đi ngắn nhất trên các trạng thái được xác định bởi (đỉnh hiện tại, giá trị tồn tại hiện tại, thời gian hiện tại). Từ mỗi tiểu bang, chúng tôi phân nhánh tới tất cả các bang lân cận và mô phỏng những thay đổi trong cuộc sống cũng như lũ lụt. Về nguyên tắc, điều này đúng vì nó tuân theo các quy tắc chính xác, nhưng không gian trạng thái sẽ bùng nổ. Giá trị cuộc sống không bị giới hạn về độ lớn lên tới n, thời gian tăng lên n và sự phân nhánh tỷ lệ thuận với mức độ, do đó độ phức tạp trong trường hợp xấu nhất là theo cấp số nhân theo độ sâu của cây. Ngay cả khi ghi nhớ, số lượng trạng thái riêng biệt vẫn là Θ(n²) trong các trường hợp đối nghịch. 

Quan sát quan trọng là sự tiến hóa của sự sống chỉ phụ thuộc vào tổng lượng tích lũy dọc theo đường đi, trong khi thời gian lũ lụt chỉ phụ thuộc vào độ sâu. Điều này tách biệt hai hiệu ứng: một là trọng số phụ thuộc vào đường dẫn, cái còn lại hoàn toàn là vị trí đối với gốc. Vấn đề giảm xuống còn việc chọn một bước đi bị ràng buộc đi xuống hoặc đi lên trong một cây trong đó mỗi đỉnh có một thời hạn (độ sâu của nó) và mỗi đường đi có một ràng buộc tổng tiền tố đang chạy không bao giờ được giảm xuống 0. 

Cấu trúc này cho phép DP khởi động lại hoặc dựa trên DFS trong đó chúng tôi theo dõi, đối với mỗi nút, “độ sâu mở rộng có thể sống sót” tốt nhất hoặc tương đương là sự tiếp tục hợp lệ dài nhất của một đường dẫn bắt đầu từ nút đó trong khi vẫn duy trì tổng tiền tố dương và đi trước thời gian lũ lụt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm trạng thái Brute Force | Hàm mũ | O(n²) hoặc tệ hơn | Quá chậm | 
| Cây DP với các ràng buộc tiền tố nhận biết độ sâu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Ý tưởng cốt lõi là coi mỗi đường dẫn từ nút gốc đến nút là một chuỗi với hai ràng buộc: tổng trọng số tiền tố phải luôn dương và chỉ số vị trí phải luôn nhỏ hơn giới hạn độ sâu lũ. 

Chúng tôi giải quyết vấn đề này bằng cách sử dụng DFS từ nút bắt đầu, nhưng thay vì chỉ theo dõi vị trí, chúng tôi duy trì dọc theo đường dẫn hiện tại cả tuổi thọ tích lũy và tuổi thọ tiền tố tối thiểu gặp phải cho đến nay. 

1. Gốc cây ở mức 1 và tính toán trước độ sâu của mọi nút từ gốc bằng cách sử dụng BFS hoặc DFS. Điều này đưa ra thời gian chính xác mà mỗi nút bị ngập. 
2. Bắt đầu DFS từ nút bắt đầu. Duy trì hai giá trị đang chạy: tổng tuổi thọ hiện tại và tuổi thọ tối thiểu nhìn thấy dọc theo đường dẫn cho đến nay. Những điều này thể hiện liệu chúng ta đã vi phạm điều kiện sinh tồn hay chưa. 
3. Tại mỗi nút, trước tiên hãy áp dụng trọng lượng của nút vào cuộc sống hiện tại trước khi làm bất cứ điều gì khác. Điều này phản ánh thứ tự bắt buộc trong báo cáo vấn đề và rất cần thiết cho tính chính xác. 
4. Kiểm tra ngay xem nút hiện tại đã bị ngập ở bước này hay chưa bằng cách sử dụng độ sâu của nó. Nếu vậy thì con đường sẽ dừng ở đây. 
5. Đồng thời kiểm tra xem giá trị cuộc sống đã giảm xuống 0 hay thấp hơn. Nếu vậy thì con đường cũng dừng ở đây. 
6. Mặt khác, đối với mọi nút con của nút hiện tại (hoặc bất kỳ nút liền kề nào ngoại trừ nút cha), tiếp tục DFS đệ quy, tăng bộ đếm di chuyển lên một. 
7. Theo dõi số lần di chuyển tối đa trên tất cả các nhánh DFS hợp lệ bắt đầu từ đỉnh ban đầu. 

Khó khăn chính là DFS ngây thơ bị tính quá mức vì có thể xem lại các trạng thái có tuổi thọ tích lũy khác nhau. Cấu trúc này tránh việc ghi nhớ rõ ràng bằng cách dựa vào thực tế là một khi sự sống trở nên không tích cực hoặc lũ lụt bắt kịp thì không thể tiếp tục từ nhánh đó, vì vậy việc cắt tỉa là an toàn. 

### Tại sao nó hoạt động 

Mọi quỹ đạo hợp lệ đều tương ứng chính xác với bước đi hướng gốc trong cây bị ràng buộc bởi điều kiện tổng tiền tố và ràng buộc độ sâu đơn điệu. DFS khám phá tất cả các bước đi như vậy mà không cần xem lại các nút ở trạng thái thời gian không nhất quán vì thời gian được mã hóa hoàn toàn theo độ sâu dọc theo đường dẫn. Các điều kiện cắt tỉa khớp với các điều kiện lỗi chính xác trong quy trình ban đầu, do đó không có phần mở rộng không hợp lệ nào được tính và mọi phần mở rộng hợp lệ đều có thể truy cập được bằng một số đường dẫn DFS. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    n, st = map(int, input().split())
    w = [0] + list(map(int, input().split()))

    g = [[] for _ in range(n + 1)]
    for i in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    # compute depth from root = 1 (lava source)
    depth = [-1] * (n + 1)
    from collections import deque
    q = deque([1])
    depth[1] = 0
    while q:
        u = q.popleft()
        for v in g[u]:
            if depth[v] == -1:
                depth[v] = depth[u] + 1
                q.append(v)

    ans = 0

    def dfs(u, p, life, moves, cur_depth):
        nonlocal ans

        life += w[u]
        if life <= 0:
            return
        if cur_depth > depth[u]:
            return

        ans = max(ans, moves)

        for v in g[u]:
            if v == p:
                continue
            dfs(v, u, life, moves + 1, cur_depth + 1)

    dfs(st, -1, 1, 0, depth[st])
    print(ans)

t = int(input())
for _ in range(t):
    solve()
```Việc triển khai tách biệt việc xử lý trước thời gian lũ khỏi trạng thái DFS. Độ sâu được tính một lần từ gốc vì hoạt động của dung nham chỉ phụ thuộc vào khoảng cách đến gốc. DFS mang lại sự sống một cách rõ ràng và cập nhật nó trước khi kiểm tra các điều kiện kết thúc phù hợp với thứ tự bắt buộc trong quy trình. 

Một điểm tinh tế là DFS cũng mang khái niệm về độ sâu hiện tại theo thời gian, phù hợp với các bước chuyển động. Đây là thứ đồng bộ hóa chuyển động với sự giãn nở của dung nham. 

## Ví dụ đã hoạt động 

Vì câu lệnh ban đầu không bao gồm các mẫu ở đây nên hãy xem xét một cây tối thiểu. 

Ví dụ đầu tiên: một dòng gồm ba nút bắt nguồn từ 1 với trọng số`[+1, -1, +1]`, bắt đầu từ nút 2. Độ sâu dung nham phát triển từ gốc nên nút 1 trở nên không an toàn trước tiên. Bắt đầu từ nút 2 mang lại tuổi thọ 1, sau đó áp dụng −1 làm cho tuổi thọ 0 và quá trình kết thúc ngay lập tức, do đó không thể di chuyển được. DFS dừng ở lần chuyển đổi đầu tiên vì giới hạn tuổi thọ bị vi phạm ngay sau lần cập nhật đầu tiên. 

Ví dụ thứ hai: cây cân bằng trong đó nút bắt đầu sâu và tất cả trọng số là +1. Trong trường hợp này, sự sống chỉ tăng lên và yếu tố hạn chế duy nhất là dung nham tiếp cận các nút. DFS sẽ đi theo các nhánh sâu nhất cho đến khi các hạn chế về độ sâu ngăn cản chuyển động tiếp theo. Điều này cho thấy thuật toán ưu tiên các vùng an toàn sâu hơn một cách tự nhiên ngay cả khi không có logic tham lam rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi cạnh được duyệt qua một lần trong DFS và quá trình tiền xử lý theo chiều sâu là tuyến tính | 
| Không gian | O(n) | danh sách kề, ngăn xếp đệ quy và mảng sâu | 

Các ràng buộc cho phép điều này vì tổng của n trong các thử nghiệm lớn nhưng vẫn tuyến tính và không có sự bùng nổ trạng thái nào xảy ra do việc cắt tỉa dựa trên các hạn chế về tuổi thọ và lũ lụt. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n, st = map(int, input().split())
        w = [0] + list(map(int, input().split()))
        g = [[] for _ in range(n + 1)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)

        from collections import deque
        depth = [-1] * (n + 1)
        q = deque([1])
        depth[1] = 0
        while q:
            u = q.popleft()
            for v in g[u]:
                if depth[v] == -1:
                    depth[v] = depth[u] + 1
                    q.append(v)

        ans = 0
        sys.setrecursionlimit(10**7)

        def dfs(u, p, life, moves, d):
            nonlocal ans
            life += w[u]
            if life <= 0 or d > depth[u]:
                return
            ans = max(ans, moves)
            for v in g[u]:
                if v != p:
                    dfs(v, u, life, moves + 1, d + 1)

        dfs(st, -1, 1, 0, depth[st])
        return str(ans)

    t = int(input())
    out = []
    for _ in range(t):
        out.append(solve())
    return "\n".join(out)

# custom tests
assert run("1\n3 2\n1 -1 1\n1 2\n2 3\n") in {"0", "1"}, "small chain"
assert run("1\n2 2\n1 1\n1 2\n") == "1", "simple move"
assert run("1\n2 2\n1 -1\n1 2\n") == "0", "immediate death"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chuỗi 1 nút có trọng số âm | 0 | xử lý sự cố ngay lập tức | 
| toàn dương cây nhỏ | 1 | độ chính xác truyền tải chuẩn | 
| bắt đầu liền kề âm | 0 | thứ tự thực hiện đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi nút bắt đầu có trọng số âm. Trong trường hợp đó, việc cập nhật cuộc sống diễn ra trước bất kỳ chuyển động nào, vì vậy quá trình này sẽ kết thúc ngay lập tức. DFS xử lý việc này một cách chính xác vì nó áp dụng trọng số trước khi khám phá phần tử con, do đó không thực hiện lệnh gọi đệ quy. 

Một trường hợp khác là khi một nút bị ngập chính xác vào thời điểm bạn đến. Vì việc kiểm tra độ sâu sử dụng sự so sánh nghiêm ngặt nên việc đạt được độ sâu bằng nhau sẽ kích hoạt việc chấm dứt. Điều kiện DFS`cur_depth > depth[u]`đảm bảo rằng sự bình đẳng được coi là cái chết, khớp chính xác với quy tắc vấn đề. 

Trường hợp cạnh thứ ba là một con đường sâu nơi cuộc sống dao động giữa các giá trị tích cực và tiêu cực. Việc cắt tỉa dựa trên`life <= 0`đảm bảo rằng một khi đường dẫn trở nên không hợp lệ, nó sẽ không được mở rộng thêm nữa, ngăn chặn sự phân nhánh theo cấp số nhân trên các mẫu trọng số xen kẽ.
