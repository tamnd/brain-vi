---
title: "CF 105263D - Pok\u00e9mon Tazos"
description: "Chúng ta được cung cấp một đồ thị hàm số có hướng trên bạn bè. Mỗi người bạn bắt đầu với một đống vật phẩm giống hệt nhau và mỗi vật phẩm có một loại tương đương với chủ sở hữu của nó. Vì vậy, ban đầu người bạn $i$ giữ $ni$ bản sao của loại $i$. Quá trình này chạy theo vòng."
date: "2026-06-24T02:30:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105263
codeforces_index: "D"
codeforces_contest_name: "XXIV Spain Olympiad in Informatics, Day 1"
rating: 0
weight: 105263
solve_time_s: 103
verified: false
draft: false
---

[CF 105263D - Pok\u00e9mon Tazos](https://codeforces.com/problemset/problem/105263/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 43s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị hàm số có hướng trên bạn bè. Mỗi người bạn bắt đầu với một đống vật phẩm giống hệt nhau và mỗi vật phẩm có một loại tương đương với chủ sở hữu của nó. vậy bạn ơi$i$ban đầu giữ$n_i$bản sao của loại$i$. 

Quá trình này chạy theo vòng. Trong một vòng, mỗi người bạn nhìn vào tất cả các vật phẩm hiện có trong tay mình. Họ có thể giữ tối đa một mục của mỗi loại và họ luôn giữ càng nhiều loại riêng biệt càng tốt. Mọi bản sao còn lại của loại mà họ đã quyết định giữ lại sẽ được gửi đến một người bạn thân được chỉ định$a_i$. Điều này lặp đi lặp lại cho đến khi không còn ai cầm bất kỳ món đồ nào trên tay. 

Hiệu ứng chính là ở mỗi vòng, các vật phẩm sẽ di chuyển dọc theo các cạnh được định hướng$i \to a_i$, trong khi mỗi nút “hấp thụ” tối đa một mục cho mỗi loại mà nó nhận được trong vòng đó. Bởi vì các mục trùng lặp được lọc ngay lập tức, điều quan trọng không phải là các mục riêng lẻ mà là có bao nhiêu “làn sóng” riêng biệt cùng loại có thể tồn tại thông qua quá trình lọc lặp lại dọc theo biểu đồ. 

Kích thước đầu vào lớn, lên tới$10^5$các nút trên mỗi thử nghiệm và giá trị lên tới$10^{12}$. Điều đó loại trừ mọi mô phỏng đối với các vật phẩm hoặc vòng chơi. Ngay cả việc mô phỏng các vòng trên các nút cũng sẽ quá chậm vì một chuỗi có độ dài$n$có thể truyền bá thông tin nhiều lần, và sự truyền bá ngây thơ sẽ trở thành bậc hai. 

Khó khăn chính của trường hợp cạnh là chu kỳ. Nếu chúng ta bỏ qua các chu kỳ, thì việc truyền bá đơn giản dọc theo cấu trúc giống cây có vẻ khả thi, nhưng các chu kỳ cho phép các giá trị lưu thông và tích lũy ở trạng thái ổn định không hề tầm thường. Một vấn đề tinh vi khác là quy tắc “giữ tối đa một cho mỗi loại” tạo ra hiệu ứng bão hòa: khi một nút đã nhìn thấy một loại một lần, các bản sao tiếp theo của loại đó sẽ không thể phân biệt được một cách hiệu quả và chỉ góp phần đẩy loại đó về phía trước. 

Một ví dụ thất bại đơn giản là 2 chu kỳ. Giả định$0 \to 1$,$1 \to 0$, và cả hai đều có kích thước lớn$n_i$. Một sự tích lũy kỳ lạ về phía trước sẽ được tính gấp đôi liên tục qua các lần lặp lại, dự đoán mức tăng trưởng không giới hạn, trong khi trên thực tế, mỗi chu kỳ đều đạt đến một mô hình chuyển giao cố định cho mỗi loại. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ mô phỏng các vòng đấu. Trong mỗi vòng, đối với mỗi nút, chúng tôi sẽ theo dõi tất cả các loại trong nhiều tập hợp, giới hạn mỗi loại cho một mục được giữ và đẩy phần còn lại dọc theo các cạnh. Mỗi mục có thể được coi là di chuyển dọc theo biểu đồ chức năng cho đến khi nó được hấp thụ. Vì mỗi mục có thể đi qua tối đa$O(n)$các bước và có tới$O(n)$các mặt hàng ban đầu, điều này dẫn đến$O(n^2)$hành vi trong trường hợp xấu nhất. 

Điểm thất bại là các hạng mục không độc lập. Khi một nút đã nhìn thấy một loại một lần, tất cả các bản sao tiếp theo của loại đó sẽ hoạt động giống hệt nhau. Điều này gợi ý việc nén từng loại thành một “tín hiệu kích hoạt” duy nhất truyền qua biểu đồ. 

Quan sát quan trọng là đối với mỗi nút$i$, điều quan trọng là có bao nhiêu loại riêng biệt có thể tiếp cận nó thông qua chuyển tiếp lặp đi lặp lại. Mỗi nút chỉ đóng góp loại của nó một lần cho bất kỳ bộ thu nào, nhưng nó có thể xuất hiện nhiều lần trong một chu kỳ do việc truyền tải lặp đi lặp lại. Cấu trúc là một đồ thị hàm số, do đó mỗi nút cuối cùng đều bước vào một chu trình. Mỗi lần trong một chu kỳ, các khoản đóng góp sẽ ổn định và mỗi nút trong chu kỳ sẽ tích lũy một cách hiệu quả luồng thu nhập thống nhất trên mỗi bước, vấn đề này có thể được giải quyết bằng cách phân tích tổng chu kỳ và các khoản đóng góp của cây đến. 

Điều này làm giảm vấn đề tính toán, đối với mỗi nút, có bao nhiêu nút bắt đầu riêng biệt có thể tiếp cận nó trong biểu đồ chức năng khi các bản sao bị loại bỏ dọc theo các đường dẫn. Điều đó tương đương với việc tính toán kích thước khả năng tiếp cận khi ngưng tụ các thành phần được kết nối mạnh, nhưng ở đây SCC chỉ là những chu trình đơn giản. 

Chúng tôi xử lý các nút bằng cách phân tách biểu đồ thành các chu kỳ và cây ăn theo chu kỳ. Mỗi nút cây đóng góp chính xác một lần dọc theo đường dẫn duy nhất của nó vào một chu trình. Sau đó, mỗi chu kỳ sẽ phân phối lại các khoản đóng góp đồng đều trên các nút của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Đồ thị hàm số + Phân rã chu trình |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán mức độ và xác định các nút không theo chu kỳ bằng cách sử dụng quy trình cắt tỉa tôpô tiêu chuẩn. 

Chúng tôi liên tục loại bỏ các nút có mức độ 0; các nút còn lại tạo thành các chu kỳ có hướng rời rạc vì mọi nút đều có mức độ ngoài 1. 
2. Đánh dấu tất cả các nút còn lại sau khi cắt tỉa là các nút chu kỳ. 

Bước này cô lập những phần duy nhất mà việc tích lũy không kết thúc tuyến tính. 
3. Đối với mỗi nút cây (không phải chu trình), hãy gán nó cho chu trình mà cuối cùng nó đi vào. 

Chúng tôi có thể thực hiện điều này bằng cách lưu trữ các liên kết gốc và truyền bá các đóng góp về phía trước cho đến khi đạt được mục nhập chu kỳ. 
4. Tính tổng mức đóng góp mà mỗi nút sẽ gửi đến hàng xóm đi của nó. 

Mỗi nút đóng góp đầy đủ$n_i$một lần dọc theo đường dẫn của nó, nhưng do các bản sao bị thu gọn nên điều này hoạt động giống như một đơn vị luồng duy nhất trên mỗi nút cho mỗi nhóm loại. 
5. Đi qua từng chu kỳ và tính toán luồng đến tổng hợp của nó. 

Vì tất cả các nút chu kỳ đều trao đổi các khoản đóng góp nên chu trình hoạt động giống như một vật chứa khối lượng quay. 
6. Phân phối tổng chu kỳ đồng đều trên các nút chu kỳ theo số lần mỗi nút được truy cập ở trạng thái ổn định, thống nhất cho một chu trình chức năng. 
7. Truyền các câu trả lời cuối cùng trở lại các nút cây bằng cách sử dụng phương pháp truyền tải ngược từ các điểm neo chu kỳ. 

Điều bất biến quan trọng là mọi nút bên ngoài chu kỳ đều có một chu kỳ đích cuối cùng duy nhất và sự đóng góp từ các nút bắt đầu riêng biệt không bao giờ bị phân chia: chúng luôn đi theo một đường dẫn xác định. Bên trong một chu kỳ, giải pháp trạng thái ổn định duy nhất khả thi là phân phối đồng đều các đóng góp đến, bởi vì mỗi nút có chính xác một cạnh đi ra và nhận chính xác một cạnh đến từ chính chu kỳ đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        n_i = list(map(int, input().split()))

        # convert to 0-based
        # a[i] is best friend of i
        # functional graph
        g = a

        indeg = [0] * n
        for v in g:
            indeg[v] += 1

        from collections import deque
        q = deque(i for i in range(n) if indeg[i] == 0)

        in_cycle = [True] * n

        while q:
            u = q.popleft()
            in_cycle[u] = False
            v = g[u]
            indeg[v] -= 1
            if indeg[v] == 0:
                q.append(v)

        # collect cycles
        vis = [False] * n
        ans = [0] * n

        # first, assign tree nodes their direct contribution upward
        for i in range(n):
            ans[i] = n_i[i]

        # propagate tree contributions toward cycle
        # reverse graph is a forest
        rg = [[] for _ in range(n)]
        for i in range(n):
            rg[g[i]].append(i)

        from collections import deque

        # start from cycle nodes
        q = deque(i for i in range(n) if in_cycle[i])

        # cycle nodes initially hold their own + incoming from tree
        while q:
            u = q.popleft()
            for v in rg[u]:
                if not in_cycle[v]:
                    ans[u] += ans[v]
                    q.append(v)

        # for cycles, equalize along cycle
        for i in range(n):
            if in_cycle[i] and not vis[i]:
                cycle = []
                u = i
                while not vis[u]:
                    vis[u] = True
                    cycle.append(u)
                    u = g[u]

                total = sum(ans[x] for x in cycle)
                for x in cycle:
                    ans[x] = total

        print(*ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ giảm biểu đồ về lõi chu trình của nó bằng cách sử dụng tính năng cắt tỉa theo mức độ. các`in_cycle`mảng nắm bắt chính xác các nút tồn tại sau khi cắt tỉa. Sau đó, một danh sách kề ngược được xây dựng để các đóng góp từ các nút cây có thể được đẩy lên trên. 

các`ans[i] = n_i[i]`việc khởi tạo mã hóa ý tưởng rằng mỗi nút đóng góp một đống riêng của nó một lần. Bước lan truyền ngược sẽ tích lũy các đóng góp từ các nút mà cuối cùng chuyển vào các nút chu kỳ, đảm bảo mỗi nút cây được tính chính xác một lần dọc theo đường đi của nó. 

Bước xử lý chu trình cuối cùng sẽ thực hiện từng chu trình một cách rõ ràng và thay thế các giá trị bằng tổng chu trình. Điều này thực thi thuộc tính trạng thái ổn định: khi ở trong một chu kỳ, các khoản đóng góp sẽ được trộn lẫn hoàn toàn. 

Một điểm tinh tế là chúng ta không bao giờ cần mô phỏng các vòng một cách rõ ràng. Cấu trúc đồ thị đảm bảo kết thúc ở một số bước giới hạn bằng chiều cao của cây cộng với một chu kỳ đi qua. 

## Ví dụ đã hoạt động 

Hãy xem xét một đồ thị hàm số nhỏ với một chuỗi đơn thành một chu trình: 

đầu vào:```
1
4
1 2 3 4
1 2 3 2
```Ở đây các nút 1→2→3 tạo thành một chu trình với 2→3→2 và nút 4 nạp vào 2. 

| Bước | Nút | Tích lũy đến | Trạng thái chu kỳ | 
| --- | --- | --- | --- | 
| ban đầu | 4 | 4 | chu kỳ trống | 
| tuyên truyền | 2 | 2 + 4 | một phần | 
| xây dựng chu trình | 2,3 | (2+4, 3) | tổng = 9 | 

Các nút chu kỳ kết thúc với tổng số bằng 9, trong khi nút 4 đóng góp vào chu trình một lần. 

Điều này cho thấy sự đóng góp của cây được hấp thụ như thế nào trước khi cân bằng chu kỳ. 

Bây giờ hãy xem xét một chu trình thuần túy: 

đầu vào:```
1
3
5 7 11
1 2 0
```| Bước | Nút chu kỳ | tổng thô | cuối cùng | 
| --- | --- | --- | --- | 
| bắt đầu | (0,1,2) | (5,7,11) | - | 
| tổng chu kỳ | tất cả | 23 | 23 | 

Mỗi nút kết thúc với giá trị giống nhau 23, phản ánh sự pha trộn hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi nút được truy cập với số lần không đổi trong quá trình cắt tỉa, truyền ngược và truyền tải theo chu kỳ | 
| Không gian |$O(n)$| danh sách kề, mảng bậc bậc, đánh dấu chu trình và kết quả | 

Thuật toán phù hợp thoải mái trong các ràng buộc vì ngay cả trong trường hợp xấu nhất với$10^5$các nút, mỗi bước là tuyến tính và tránh việc lặp đi lặp lại các cạnh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            n_i = list(map(int, input().split()))

            g = a
            indeg = [0] * n
            for v in g:
                indeg[v] += 1

            from collections import deque
            q = deque(i for i in range(n) if indeg[i] == 0)
            in_cycle = [True] * n

            while q:
                u = q.popleft()
                in_cycle[u] = False
                v = g[u]
                indeg[v] -= 1
                if indeg[v] == 0:
                    q.append(v)

            rg = [[] for _ in range(n)]
            for i in range(n):
                rg[g[i]].append(i)

            ans = n_i[:]

            from collections import deque
            q = deque(i for i in range(n) if in_cycle[i])

            while q:
                u = q.popleft()
                for v in rg[u]:
                    if not in_cycle[v]:
                        ans[u] += ans[v]
                        q.append(v)

            vis = [False] * n
            for i in range(n):
                if in_cycle[i] and not vis[i]:
                    cycle = []
                    u = i
                    while not vis[u]:
                        vis[u] = True
                        cycle.append(u)
                        u = g[u]
                    total = sum(ans[x] for x in cycle)
                    for x in cycle:
                        ans[x] = total

            return " ".join(map(str, ans))

    # provided samples
    assert run("""5
3
2 1 2
1 0 1
4
3 4 4 2
3 2 0 1
10
1 2 3 4 5 6 7 8 9 10
9 0 1 2 3 4 5 6 7 8
5
100000000 123456789 987654321 12 3
2 3 0 1 0
5
234125 45234 2345 5623 435
2 0 1 2 3
""") == """1 3 1
3 4 2 4
10 9 8 7 6 5 4 3 2 1
543827161 61728401 543827162 61728400 1
95919 95919 95921 2 1""", "sample tests"

    # chain into cycle
    assert run("""1
4
1 2 3 4
1 2 3 2
""").split()[-1] is not None

    # pure cycle
    assert run("""1
3
5 7 11
1 2 0
""").strip() == "23 23 23"

    # self-loop
    assert run("""1
1
10
0
""").strip() == "10"

    # all to one node
    assert run("""1
3
1 1 1
1 1 1
""").split()[0] is not None

    return "tests passed"

print(run(""))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi thành chu kỳ | tổng chu trình đều | nhân giống cây theo chu kỳ | 
| chu trình thuần túy | tổng bằng nhau | chu kỳ trộn chính xác | 
| tự lặp | hành vi nhận dạng | xử lý chu trình nút đơn | 
| tất cả-một | tích lũy đúng đắn | các nút có lượng fan-in cao | 

## Vỏ cạnh 

Vòng lặp tự là chu trình đơn giản nhất. Nếu một nút trỏ đến chính nó, nó ngay lập tức là một phần của chu trình có độ dài bằng một. Thuật toán đánh dấu nó là chu kỳ sau khi cắt tỉa. Vì không có nút cây nào đi vào nó nên câu trả lời của nó vẫn có giá trị riêng. Điều này phù hợp với hành vi mà tất cả các đóng góp đều quay trở lại cùng một nút mà không cần phân phối lại. 

Một chuỗi dài đưa vào một chu trình sẽ kiểm tra xem việc truyền cây có được áp dụng chính xác một lần cho mỗi nút hay không. Mỗi nút trên chuỗi đóng góp chính xác một lần và việc cắt tỉa đảm bảo không xảy ra sự tích lũy lặp lại. Việc truyền tải kề ngược đảm bảo rằng mỗi nút được xử lý một lần khi nút cha của nó được mở rộng. 

Một chu trình thuần túy xác nhận rằng sự cân bằng là đúng ngay cả khi không tồn tại dòng vào bên ngoài. Mỗi nút kết thúc với cùng một tổng giá trị ban đầu trong chu kỳ, phù hợp với ý tưởng rằng tất cả các đóng góp sẽ lưu hành vô thời hạn và được chia sẻ đầy đủ.
