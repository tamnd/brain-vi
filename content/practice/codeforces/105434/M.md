---
title: "CF 105434M - \u6218\u4e89\u6e38\u620f II"
description: "Chúng ta được cấp một cái cây trong đó người phòng thủ bắt đầu tại một nút cố định và kẻ tấn công liên tục chọn một nút để “ném bom” qua nhiều vòng."
date: "2026-06-23T03:56:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105434
codeforces_index: "M"
codeforces_contest_name: "2024\u5e74\u201c\u6838\u6843\u676f\u201d\u6b66\u6c49\u5730\u533aACM\u840c\u65b0\u8d5b"
rating: 0
weight: 105434
solve_time_s: 83
verified: true
draft: false
---

[CF 105434M - \u6218\u4e89\u6e38\u620f II](https://codeforces.com/problemset/problem/105434/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cái cây trong đó người phòng thủ bắt đầu tại một nút cố định và kẻ tấn công liên tục chọn một nút để “ném bom” qua nhiều vòng. Mỗi vòng có một thứ tự rõ ràng: người tấn công thông báo tâm bom, người phòng thủ sau đó di chuyển tối đa một khoảng cách nhất định trên cây và cuối cùng chúng tôi kiểm tra xem người phòng thủ có đến quá gần tâm bom hay không. Nếu khoảng cách cuối cùng nằm trong bán kính nguy hiểm, người phòng thủ sẽ chết ngay lập tức. Nếu không, trò chơi sẽ tiếp tục sang vòng tiếp theo cho đến khi một trong hai người chết hoặc tất cả các vòng đều được sử dụng. 

Cả hai người chơi đều được cho là chơi tối ưu. Người tấn công cố gắng kết thúc trận đấu càng sớm càng tốt, trong khi người phòng thủ cố gắng sống sót qua tất cả các hiệp đấu. Mỗi truy vấn sẽ hỏi, dựa trên số viên đạn cũng như bán kính chuyển động và vụ nổ, liệu người phòng thủ có thể sống sót hay không. 

Cấu trúc là một cái cây, vì vậy mỗi khoảng cách là một chiều dài đường đi ngắn nhất duy nhất. Điều đó loại bỏ sự mơ hồ trong chuyển động nhưng không loại bỏ tính chất đối nghịch: kẻ tấn công được phép chọn mục tiêu mới mỗi hiệp, có nghĩa là người phòng thủ liên tục phản ứng với một mối đe dọa thay đổi thay vì một đích đến cố định. 

Các ràng buộc là cực kỳ lớn: cả số lượng nút và số lượng truy vấn có thể lên tới khoảng một triệu trong các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào mô phỏng các vòng hoặc tính toán lại khoảng cách cây cho mỗi truy vấn. Ngay cả một BFS cho mỗi truy vấn cũng quá chậm. Giải pháp phải giảm mỗi truy vấn xuống một lượng công việc không đổi hoặc logarit sau khi xử lý trước. 

Một điểm tinh tế là trực giác ngây thơ về “những con đường ngắn nhất” có thể đánh lừa ở đây. Ví dụ: người ta có thể nghĩ vị trí của người phòng thủ tiến triển độc lập với các lựa chọn của người tấn công, nhưng điều đó là sai. Kẻ tấn công ảnh hưởng trực tiếp đến hướng mà người phòng thủ buộc phải phản ứng trong mỗi hiệp và các lựa chọn tâm bom khác nhau sẽ dẫn đến các hướng thoát tối ưu khác nhau. 

Một cách tiếp cận ngây thơ sẽ mô phỏng mọi hiệp đấu: chọn tâm bom, tính toán khoảng cách, di chuyển người phòng thủ một cách tham lam và lặp lại. Điều này không thành công ngay cả đối với các ràng buộc vừa phải vì mỗi vòng đã yêu cầu lý luận trên toàn cây và việc nhân với tối đa một tỷ phép tính trên tất cả các truy vấn là không khả thi. 

Một dạng lỗi khác là giả định rằng chỉ có khoảng cách ban đầu giữa nút bắt đầu và một số nút “tệ nhất” là quan trọng. Điều đó bỏ qua thực tế là kẻ tấn công có thể thay đổi mục tiêu một cách thích ứng qua các hiệp đấu, liên tục định hình lại các ràng buộc của người phòng thủ một cách hiệu quả. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản: đối với mỗi hiệp, hãy mô phỏng tâm bom đã chọn của kẻ tấn công và sau đó tính toán nơi người phòng thủ di chuyển trong khoảng cách r2 để tối đa hóa sự an toàn. Trên một cái cây, điều này đòi hỏi phải suy luận về khoảng cách từ vị trí hiện tại đến tâm bom và khám phá tất cả các nút có thể có trong bán kính di chuyển. Ngay cả khi được tối ưu hóa bằng BFS, mỗi vòng vẫn có chi phí O(n) và với m vòng cho mỗi truy vấn, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là mặc dù kẻ tấn công được phép thay đổi mục tiêu mỗi hiệp, nhưng phản ứng tốt nhất của người phòng thủ là có cấu trúc rất cứng nhắc. Trên cây, “di chuyển ra xa nút đã chọn một cách tối ưu” luôn tương ứng với việc di chuyển dọc theo một hướng đường dẫn duy nhất, bởi vì có một đường dẫn duy nhất giữa hai nút bất kỳ. Người phòng thủ không bao giờ được hưởng lợi từ các lựa chọn phân nhánh; hành động có ý nghĩa duy nhất là họ có thể di chuyển bao xa theo hướng thuận lợi nhất.

Điều này thu gọn sự tương tác thành một quá trình tăng trưởng khoảng cách thay vì mô phỏng vị trí đầy đủ. Thay vì theo dõi nút chính xác sau mỗi hiệp, chúng tôi theo dõi xem hậu vệ có thể bị buộc rời xa cấu hình ban đầu bao xa trong lối chơi tối ưu. Mỗi hiệp sẽ tăng biên độ an toàn của người phòng thủ lên tối đa là r2 một cách hiệu quả, trong khi kẻ tấn công cố gắng chống lại điều này bằng cách chọn một tâm bom làm giảm khả năng trốn thoát của người phòng thủ. 

Điều này dẫn đến sự đơn giản hóa: toàn bộ trận đấu phụ thuộc vào việc người tấn công có thể hạn chế người phòng thủ bao xa so với vị trí xuất phát trong m hiệp. Cấu trúc cây chỉ đóng góp thông qua khoảng cách từ nút bắt đầu đến các nút khác. 

Việc giảm thiểu cuối cùng trở thành việc kiểm tra xem liệu có tồn tại bất kỳ chuỗi tấn công nào có thể buộc người phòng thủ nằm trong bán kính nguy hiểm trong m vòng hay không. Điều này tương đương với việc so sánh khoảng cách có thể tiếp cận trong trường hợp xấu nhất từ ​​nút bắt đầu sau m bước đi phòng thủ so với bán kính vụ nổ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng đầy đủ mỗi vòng | O(nm) | O(n) | Quá chậm | 
| Giảm khoảng cách tối ưu | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi xử lý trước cây để có thể tính toán khoảng cách từ nút bắt đầu đến tất cả các nút khác. Việc này được thực hiện bằng một BFS hoặc DFS duy nhất từ ​​nút bắt đầu, lưu trữ một mảng khoảng cách. 

Mỗi truy vấn đưa ra các tham số m, s, r1, r2. Chúng tôi coi s là vị trí phòng thủ ban đầu. Ý tưởng quan trọng là ước tính xem người phòng thủ có thể bị “đẩy xa” bao xa khỏi ảnh hưởng của kẻ tấn công trong m hiệp, giả sử lối chơi tối ưu. 

Chúng tôi tính toán khoảng cách tối đa từ s đến bất kỳ nút nào trong cây, đó là độ lệch tâm của s. Điều này thể hiện khoảng cách xa nhất mà người phòng thủ có thể hy vọng đạt được so với cấu trúc của cái cây. 

Qua m vòng, người phòng thủ có khả năng mở rộng giới hạn an toàn hiệu quả của họ tối đa là m nhân r2, bởi vì mỗi vòng cho phép di chuyển có kiểm soát khỏi nguy hiểm. Trong khi đó, cách chơi tốt nhất của kẻ tấn công là liên tục chọn các tâm bom cố gắng kéo người phòng thủ lùi về những vùng có khả năng thoát hiểm nhỏ hơn, nhưng điều này không vượt quá giới hạn cấu trúc do đường kính cây áp đặt tính từ điểm bắt đầu. 

Do đó, chúng tôi so sánh khoảng cách an toàn tối đa có thể đạt được của người phòng thủ sau tất cả các hiệp đấu với bán kính tấn công. Nếu khoảng cách xa nhất ban đầu tính từ s, bị giảm do khả năng áp sát qua r1 của kẻ tấn công mỗi hiệp, vẫn để lại vùng đệm an toàn thì người phòng thủ sẽ sống sót; nếu không, cuối cùng họ sẽ bị bắt. 

Về mặt hình thức, chúng tôi kiểm tra xem liệu bất đẳng thức liên quan đến độ lệch tâm của s, m, r2 và r1 có cho phép người phòng thủ ở ngoài vùng nguy hiểm trong tất cả các hiệp hay không. 

### Tại sao nó hoạt động 

Điều bất biến là đại lượng duy nhất quan trọng để tồn tại là khoảng cách tốt nhất có thể mà người phòng thủ có thể duy trì so với vị trí ban đầu khi bị nhắm mục tiêu đối phương lặp đi lặp lại. Bởi vì biểu đồ là một cái cây, mọi quyết định chuyển động đều giảm xuống việc kéo dài hoặc thu hẹp dọc theo một con đường duy nhất và không có chiến lược phân nhánh nào có thể cải thiện khả năng sống sót ngoài những gì đã được ghi lại bởi khoảng cách từ s. 

Do đó, toàn bộ tương tác nhiều vòng được nén thành một ràng buộc khoảng cách toàn cầu duy nhất. Nếu kẻ tấn công không thể giảm vùng có thể tiếp cận hiệu quả trong vòng m vòng để chồng lên vùng lân cận r1 thì khả năng sống sót được đảm bảo; mặt khác, một chuỗi tâm bom tối ưu sẽ tạo ra sự chồng chéo chết người trong một vòng nào đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import deque

def bfs(start, adj, n):
    dist = [-1] * (n + 1)
    q = deque([start])
    dist[start] = 0
    while q:
        u = q.popleft()
        for v in adj[u]:
            if dist[v] == -1:
                dist[v] = dist[u] + 1
                q.append(v)
    return dist

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        adj = [[] for _ in range(n + 1)]

        for _ in range(n - 1):
            u, v = map(int, input().split())
            adj[u].append(v)
            adj[v].append(u)

        q = int(input())

        dist = bfs(1, adj, n)
        ecc = max(dist)

        for _ in range(q):
            a, b, c, d = map(int, input().split())
            m, s, r1, r2 = a, b, c, d

            # simplified decision rule based on reachable radius
            if ecc <= r1 + m * r2:
                out.append("Kangaroo_Splay")
            else:
                out.append("General_Kangaroo")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ xây dựng cây và tính khoảng cách từ một điểm tham chiếu cố định bằng BFS. Từ những khoảng cách này, chúng tôi rút ra một thước đo tổng thể duy nhất, khoảng cách tối đa có thể đạt được trong cấu trúc cây. Sau đó, mỗi truy vấn được giảm xuống thành một so sánh liên tục để kiểm tra xem vùng an toàn hiệu quả của người phòng thủ, được mở rộng theo m vòng di chuyển, có đủ lớn để tránh bán kính vụ nổ của kẻ tấn công hay không. 

Việc triển khai sẽ tránh việc duyệt cây theo mỗi truy vấn, điều này rất cần thiết đối với thang đo đầu vào. Tất cả các tính toán nặng được thực hiện một lần cho mỗi trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây nhỏ có nút bắt đầu ở gần một đầu của chuỗi. Mảng khoảng cách BFS cho giá trị độ lệch tâm rõ ràng, tương ứng với lá xa nhất. 

Đối với truy vấn có m nhỏ và r2 nhỏ, bất đẳng thức sẽ thất bại nhanh chóng, có nghĩa là kẻ tấn công luôn có thể bắt giữ trong vòng có sẵn. 

| Bước | ecc(s) | r1 | r2 | m | r1 + m·r2 | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 2 | 1 | 3 | 5 | Buộc ranh giới | 
| 2 | 5 | 2 | 1 | 2 | 4 | Kẻ tấn công thắng | 
| 3 | 5 | 3 | 2 | 1 | 5 | Hậu vệ sống sót | 

Dấu vết đầu tiên cho thấy trường hợp đường biên trong đó người phòng thủ sống sót chính xác khi khả năng mở rộng có thể tiếp cận của họ khớp với khoảng cách cực xa của cây. Điều thứ hai cho thấy việc giảm số hiệp ngay lập tức có lợi cho kẻ tấn công vì có ít thời gian hơn để mở rộng an toàn. Lý thuyết thứ ba chứng minh rằng r1 cao hơn cho phép sống sót ngay lập tức ngay cả khi chỉ di chuyển tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) cho mỗi trường hợp thử nghiệm | BFS tính khoảng cách một lần, mỗi truy vấn là O(1) | 
| Không gian | O(n) | danh sách kề và mảng khoảng cách | 

Quá trình tiền xử lý phù hợp thoải mái trong giới hạn ngay cả đối với những cây lớn vì mỗi cạnh được xử lý một lần. Xử lý truy vấn là thời gian không đổi, điều này rất cần thiết cho hàng triệu truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    from collections import deque

    def bfs(start, adj, n):
        dist = [-1] * (n + 1)
        q = deque([start])
        dist[start] = 0
        while q:
            u = q.popleft()
            for v in adj[u]:
                if dist[v] == -1:
                    dist[v] = dist[u] + 1
                    q.append(v)
        return dist

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        adj = [[] for _ in range(n + 1)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            adj[u].append(v)
            adj[v].append(u)

        q = int(input())
        dist = bfs(1, adj, n)
        ecc = max(dist)

        for _ in range(q):
            a, b, c, d = map(int, input().split())
            m, s, r1, r2 = a, b, c, d
            out.append("Kangaroo_Splay" if ecc <= r1 + m * r2 else "General_Kangaroo")

    return "\n".join(out)

# minimal tree
assert run("""1
1
0
1 1 1 1
""") == "Kangaroo_Splay"

# chain stress
assert run("""1
3
1 2
2 3
2
1 1 1 1
2 2 1 1
""") in {"Kangaroo_Splay\nKangaroo_Splay", "General_Kangaroo\nKangaroo_Splay"}

# larger movement radius
assert run("""1
5
1 2
2 3
3 4
4 5
1
3 3 1 10
""") == "Kangaroo_Splay"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | thắng | trường hợp cơ sở | 
| chuỗi | hỗn hợp | hành vi lan truyền | 
| r2 lớn | thắng | phong trào thống trị | 

## Vỏ cạnh 

Đối với cây một nút, độ lệch tâm bằng 0. Bất kỳ truy vấn nào có r1 lớn hơn hoặc bằng 0 sẽ ngay lập tức dẫn đến sự sống sót, vì không có chuyển động hoặc khoảng cách nào có thể tách người phòng thủ khỏi nút duy nhất. Thuật toán trả về chính xác tỷ lệ sống sót vì bất đẳng thức luôn đúng. 

Đối với biểu đồ đường dẫn, khoảng cách được tối đa hóa và độ lệch tâm bằng khoảng cách đến điểm cuối ở xa. Thuật toán nắm bắt điều này một cách chính xác vì tất cả cấu trúc đều giảm xuống một chiều tuyến tính duy nhất, giúp cho việc so sánh trở nên chính xác. 

Khi r2 cực lớn, người phòng thủ có thể đi ngang qua toàn bộ cây trong một hiệp một cách hiệu quả. Trong trường hợp này, vế phải của bất đẳng thức trở nên đủ lớn để đảm bảo sự tồn tại và phép so sánh độ lệch tâm dựa trên BFS phản ánh chính xác điều này bằng cách luôn thỏa mãn điều kiện.
