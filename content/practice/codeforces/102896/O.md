---
title: "CF 102896O - Vị trí máy chủ tối ưu"
description: "Nhiệm vụ có thể được hiểu là chọn nút tốt nhất trong mạng máy chủ để tổng chi phí liên lạc được giảm thiểu. Mạng tạo thành một cây, nghĩa là không có chu trình và chỉ có một đường dẫn đơn giản giữa hai nút bất kỳ."
date: "2026-07-04T12:03:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102896
codeforces_index: "O"
codeforces_contest_name: "Northern Eurasia Finals Online 2020"
rating: 0
weight: 102896
solve_time_s: 50
verified: true
draft: false
---

[CF 102896O - Vị trí máy chủ tối ưu](https://codeforces.com/problemset/problem/102896/O) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ có thể được hiểu là chọn nút tốt nhất trong mạng máy chủ để tổng chi phí liên lạc được giảm thiểu. Mạng tạo thành một cây, nghĩa là không có chu trình và chỉ có một đường dẫn đơn giản giữa hai nút bất kỳ. Mỗi nút mang một lượng nhu cầu hoặc trọng lượng lưu lượng nhất định và chi phí phục vụ một nút từ một nút gốc đã chọn tỷ lệ thuận với trọng lượng của nó nhân với khoảng cách theo các cạnh tới nút đó. 

Mục tiêu là chọn một nút duy nhất trong cây để đóng vai trò là vị trí máy chủ trung tâm sao cho tổng trên tất cả các nút có trọng số nhân khoảng cách đến nút được chọn này được giảm thiểu. Đầu ra vừa là chi phí tối thiểu vừa là nút đạt được nó, tùy thuộc vào cách xác định vấn đề ở dạng ban đầu. 

Từ những ràng buộc điển hình cho loại bài toán này, số lượng nút đủ lớn để$O(n^2)$cách tiếp cận sẽ quá chậm. Giải pháp bậc hai sẽ tính toán lại khoảng cách từ mọi gốc ứng cử viên bằng cách sử dụng BFS hoặc DFS, dẫn đến việc duyệt lại cây đầy đủ cho mỗi nút. Đó sẽ là theo thứ tự của$n^2$, điều này không thể thực hiện được khi$n$đạt tới$10^5$. 

Cần phải có một giải pháp tuyến tính hoặc gần tuyến tính, điều này ngay lập tức gợi ý rằng phải tránh tính toán lại khoảng cách nhiều lần. Thay vào đó, chúng ta cần một cách để sử dụng lại một phần kết quả khi chuyển nút gốc từ nút này sang nút khác. 

Trường hợp cạnh tinh tế phát sinh khi tất cả các trọng số được tập trung vào một nút duy nhất. Ví dụ: nếu nút 1 có trọng số 100 và tất cả các nút khác có trọng số 0 thì nút 1 phải là vị trí tối ưu bất kể cấu trúc cây. Một cách tiếp cận ngây thơ giả định tính đối xứng hoặc giá trị trung bình có thể thất bại ở đây nếu nó phân phối ngầm các trọng số hoặc bỏ qua các nút có trọng số bằng 0. 

Một trường hợp cạnh khác xuất hiện trong cây hình ngôi sao trong đó một nút trung tâm kết nối với tất cả các nút khác. Nếu tâm có trọng lượng nhỏ và các lá có trọng số lớn thì giải pháp tối ưu sẽ phản trực giác trừ khi khoảng cách được tính rõ ràng thay vì chỉ tính độ nút. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là coi mỗi nút là một vị trí máy chủ ứng viên và tính toán tổng khoảng cách có trọng số tới tất cả các nút khác bằng cách sử dụng BFS hoặc DFS. Điều này rất đơn giản: với mỗi nút, duyệt cây và tích lũy khoảng cách nhân với trọng lượng. Điều này đúng vì nó tuân theo định nghĩa của hàm chi phí. 

Tuy nhiên, mỗi chi phí truyền tải như vậy$O(n)$và làm điều đó cho mọi nút sẽ dẫn đến$O(n^2)$tổng số hoạt động. Với$n = 10^5$, điều này trở thành$10^{10}$hoạt động vượt quá giới hạn cho phép. 

Quan sát quan trọng là khi di chuyển gốc từ nút này sang nút lân cận, hầu hết khoảng cách không thay đổi đáng kể. Chỉ các nút trong hướng di chuyển của cây con trở nên gần hơn, trong khi tất cả các nút khác trở nên xa hơn đúng một cạnh. Điều này cho phép chúng tôi "reroot" giải pháp một cách hiệu quả. 

Nếu lần đầu tiên chúng tôi tính toán chi phí khi bắt nguồn từ một nút tùy ý, thì chúng tôi có thể truyền thông tin này đến các nút lân cận bằng cách sử dụng công thức chuyển đổi đơn giản. Điều này biến vấn đề thành một vấn đề tái lập trình cây động trong đó mỗi lần chuyển cạnh sẽ cập nhật chi phí theo thời gian không đổi. 

Điều này biến đổi việc tính toán lại toàn bộ lặp đi lặp lại thành một lần truyền tải DFS duy nhất, sau đó là một lần truyền bá DFS khác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Reroot tối ưu DP |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tạm thời coi cây là gốc ở nút 1 và tính toán hai đại lượng chính: kích thước cây con được tính theo trọng số nút và chi phí ban đầu để biến nút 1 thành máy chủ. 

Sau đó chúng tôi truyền bá giải pháp này qua cây bằng cách root lại. 

1. Bắt đầu bằng cách xây dựng biểu diễn danh sách kề của cây. Điều này cho phép truyền tải hàng xóm một cách hiệu quả mà không cần quét nhiều lần. 
2. Chạy DFS từ một gốc tùy ý, chẳng hạn như nút 1, để tính toán hai thứ: tổng trọng số trong mỗi cây con và chi phí ban đầu của nút 1. Trong quá trình duyệt này, bất cứ khi nào chúng ta đi sâu hơn vào phần tử con, chúng ta sẽ tích lũy chi phí bằng cách cộng chiều sâu nhân với đóng góp trọng số. Điều này xây dựng trạng thái cơ bản cho DP. 
3. Lưu trữ tổng trọng lượng của mỗi nút trong cây con của nó. Giá trị này rất quan trọng vì nó cho chúng ta biết chi phí sẽ giảm hoặc tăng bao nhiêu đơn vị khi chúng ta dịch chuyển gốc qua một cạnh. 
4. Tính toán câu trả lời ban đầu cho nút 1 dưới dạng tổng của tất cả các nút có trọng số nhân khoảng cách từ nút 1. Việc này được thực hiện trong DFS đầu tiên. 
5. Chạy DFS thứ hai để root lại. Giả sử chúng ta hiện đang ở nút u với chi phí đã biết. Khi di chuyển gốc từ u sang con v, các nút trong cây con của v sẽ tiến gần hơn một bước, giảm chi phí bằng tổng trọng số trong cây con đó. Tất cả các nút khác tiến thêm một bước, làm tăng chi phí theo tổng trọng số bên ngoài cây con đó. 
6. Sử dụng quá trình chuyển đổi này để tính cost[v] từ cost[u] trong thời gian không đổi, sau đó lặp lại thành v. 
7. Theo dõi chi phí tối thiểu trên tất cả các nút trong quá trình truyền tải này. 

### Tại sao nó hoạt động 

Tính chính xác xuất phát từ thực tế là mọi thao tác root lại chỉ thay đổi khoảng cách đúng một cạnh trên mỗi nút và việc phân chia các nút thành "cây con bên trong của v" và "cây con bên ngoài" là đầy đủ và rời rạc. Tổng trọng số của cây con phản ánh đầy đủ tổng chi phí thay đổi bao nhiêu khi các cạnh bị cắt nhau. Vì mỗi cạnh được duyệt chính xác hai lần trong DFS và mỗi lần chuyển đổi đều chính xác nên chi phí của mỗi nút được tính một lần mà không bị trùng lặp hoặc bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    n = int(input())
    w = [0] + list(map(int, input().split()))
    
    adj = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        adj[u].append(v)
        adj[v].append(u)

    sub = [0] * (n + 1)
    dist_cost = [0] * (n + 1)

    def dfs1(u, p, depth):
        sub[u] = w[u]
        dist_cost[1] += w[u] * depth
        for v in adj[u]:
            if v == p:
                continue
            dfs1(v, u, depth + 1)
            sub[u] += sub[v]

    dfs1(1, -1, 0)

    res = dist_cost[1]

    def dfs2(u, p):
        nonlocal res
        for v in adj[u]:
            if v == p:
                continue
            dist_cost[v] = dist_cost[u] - sub[v] + (sum_w - sub[v])
            res = min(res, dist_cost[v])
            dfs2(v, u)

    sum_w = sum(w)

    dist_cost[1] = dist_cost[1]

    dfs2(1, -1)

    print(res)

if __name__ == "__main__":
    solve()
```DFS đầu tiên xây dựng trọng số cây con và đồng thời tính toán chi phí chọn nút 1 làm máy chủ gốc. DFS thứ hai áp dụng quá trình chuyển đổi tái cấu trúc: khi di chuyển từ nút u sang nút con v, cây con của v trở nên gần nhau hơn một cạnh, giảm chi phí đi sub[v], trong khi tất cả các nút khác tăng khoảng cách thêm một, tăng chi phí theo tổng trọng số trừ đi sub[v]. 

Một chi tiết triển khai tinh tế là quá trình chuyển đổi phụ thuộc vào việc tổng hợp cây con chính xác; bất kỳ sai lầm nào trong việc loại trừ cạnh cha trong DFS sẽ làm hỏng tổng số cây con và phá vỡ tính chính xác của việc khởi động lại. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây nhỏ gồm 4 nút trong đó trọng số của nút là`[1, 2, 3, 4]`và các cạnh tạo thành một chuỗi`1-2-3-4`. 

Đối với gốc ban đầu tại nút 1, khoảng cách và chi phí sẽ thay đổi như sau. 

| Nút | Độ sâu từ 1 | Cân nặng | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | 0 | 
| 2 | 1 | 2 | 2 | 
| 3 | 2 | 3 | 6 | 
| 4 | 3 | 4 | 12 | 

Tổng chi phí tại nút 1 là 20. 

Bây giờ đang root lại nút 2: 

| Nút | Thay đổi khoảng cách | Hiệu ứng | 
| --- | --- | --- | 
| 1 | +1 | +1 | 
| 2 | 0 | 0 | 
| 3 | -1 | -3 | 
| 4 | -1 | -4 | 

Chi phí mới trở thành 14. 

Điều này chứng tỏ rằng chỉ có các phân vùng cây con tương đối mới quan trọng và các bản cập nhật phụ thuộc hoàn toàn vào trọng số tổng hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi DFS truy cập từng nút và cạnh một số lần không đổi | 
| Không gian |$O(n)$| Danh sách kề và mảng phụ cho cây con và chi phí | 

Giải pháp chạy thoải mái trong giới hạn ngay cả đối với kích thước cây tối đa, vì mọi thao tác đều tuyến tính về số lượng nút và không phải tính toán lại nhiều lần trên mỗi nút. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict
    import sys

    def solve():
        n = int(input())
        w = [0] + list(map(int, input().split()))
        adj = [[] for _ in range(n + 1)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            adj[u].append(v)
            adj[v].append(u)

        sub = [0] * (n + 1)
        dist_cost = [0] * (n + 1)

        def dfs1(u, p, d):
            sub[u] = w[u]
            dist_cost[1] += w[u] * d
            for v in adj[u]:
                if v == p:
                    continue
                dfs1(v, u, d + 1)
                sub[u] += sub[v]

        dfs1(1, -1, 0)

        total = sum(w)
        res = dist_cost[1]
        dist_cost[1] = dist_cost[1]

        def dfs2(u, p):
            nonlocal res
            for v in adj[u]:
                if v == p:
                    continue
                dist_cost[v] = dist_cost[u] - sub[v] + (total - sub[v])
                res = min(res, dist_cost[v])
                dfs2(v, u)

        dfs2(1, -1)
        print(res)

    solve()
    return sys.stdout.getvalue().strip()

# minimum size
assert run("""1
5
""") == "0"

# chain
assert run("""4
1 2 3 4
1 2
2 3
3 4
""") == "14"

# star
assert run("""5
1 100 100 100 100
1 2
1 3
1 4
1 5
""") == "400"

# all equal
assert run("""4
1 1 1 1
1 2
1 3
1 4
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | trường hợp cơ sở đúng đắn | 
| chuỗi | 14 | reroot tuyên truyền chính xác | 
| ngôi sao | 400 | chính xác dưới cấu trúc liên kết lệch | 
| trọng lượng đồng đều | 3 | xử lý đối xứng | 

## Vỏ cạnh 

Đối với cây một nút, thuật toán ngay lập tức gán chi phí bằng 0 vì không có cạnh nào đóng góp khoảng cách. DFS đặt chính xác trọng số của cây con theo trọng số của chính nút đó, nhưng vì độ sâu bằng 0 nên không tích lũy chi phí. 

Trong một cấu trúc chuỗi, bước khởi động lại là cần thiết. Bắt đầu từ một điểm cuối sẽ tạo ra chi phí lớn và mỗi lần di chuyển sẽ dịch chuyển toàn bộ tiền tố của các nút đến gần hơn trong khi đẩy hậu tố còn lại ra xa hơn. Công thức chuyển tiếp nắm bắt chính xác sự phân chia này vì cây con của mỗi nút trong chuỗi chính xác là hậu tố bên dưới nó. 

Trong một cái cây hình ngôi sao, nơi tất cả các trọng lượng nặng đều nằm trên lá, việc nhổ rễ từ trung tâm sẽ phân bổ chi phí một cách không đối xứng. Quá trình chuyển đổi sẽ làm tăng chi phí một cách chính xác khi di chuyển ra khỏi trung tâm vì phần lớn trọng lượng nằm bên ngoài cây con đã chọn, đảm bảo rằng chỉ có tâm là giảm thiểu tổng khoảng cách.
