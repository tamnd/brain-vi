---
title: "CF 102309D - Giám đốc Orz Pandas"
description: "Chúng tôi có hai nhóm tính năng. Nhóm đầu tiên chứa (n) tính năng và nhóm thứ hai chứa (m) tính năng. Mọi đặc điểm đều có trọng số dương và một số cặp được tuyên bố là không tương thích."
date: "2026-08-13T23:50:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102309
codeforces_index: "D"
codeforces_contest_name: "The 2019 \u201cOrz Panda\u201d Cup Programming Contest"
rating: 0
weight: 102309
solve_time_s: 62
verified: true
draft: false
---

[CF 102309D - Giám đốc Orz Pandas](https://codeforces.com/problemset/problem/102309/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai nhóm tính năng. Nhóm đầu tiên chứa (n) tính năng và nhóm thứ hai chứa (m) tính năng. Mọi đặc điểm đều có trọng số dương và một số cặp được tuyên bố là không tương thích. Đầu vào đảm bảo rằng mọi cặp không tương thích đều chứa một đặc điểm từ nhóm thứ nhất và một đặc điểm từ nhóm thứ hai, do đó biểu đồ xung đột là biểu đồ lưỡng cực. 

Chúng ta cần chọn một tập hợp các tính năng có tổng trọng số tối đa sao cho không có xung đột nào được chọn cả hai điểm cuối. Trong thuật ngữ đồ thị, đây là tập độc lập có trọng số tối đa trong biểu đồ hai bên. 

Với mỗi test, dòng đầu tiên ghi (n) và (m). Dòng tiếp theo cung cấp trọng số đặc trưng (n+m). Sau đó (k) các cặp xung đột theo sau. Một cặp ((p,q)) có nghĩa là các đặc điểm (p) và (q) không thể được chọn đồng thời. Vì (p\le n<q), mọi cạnh đều đi từ phần bên trái của đồ thị hai bên đến phần bên phải. 

Đầu ra được yêu cầu là tổng trọng số lớn nhất có thể có của một tập hợp các tính năng không có xung đột. Có thể có nhiều trường hợp thử nghiệm và chúng tiếp tục cho đến hết tệp. 

Ở đây (n,m\le100), vậy đồ thị có nhiều nhất là 200 đỉnh. Số lượng xung đột có thể lên tới 10.000, gần với số cạnh tối đa có thể có giữa hai nhóm 100 đỉnh. Một tìm kiếm mạnh mẽ trên tất cả các tập hợp con sẽ có tới (2^{200}) khả năng, vượt xa khả năng mà giải pháp một giây có thể xử lý. Số lượng đỉnh nhỏ ban đầu có thể gợi ý việc liệt kê tập hợp con, nhưng hệ số mũ là trở ngại thực sự. Giới hạn cạnh dày đặc cũng có nghĩa là một thuật toán sẽ có thể xử lý thoải mái khoảng 10.000 cạnh. 

Trọng số có thể lớn bằng (10^7). Vì có tối đa 200 tính năng nên câu trả lời có thể đạt tới (2\cdot10^9). Số nguyên Python có độ chính xác tùy ý, trong khi việc triển khai C++ vẫn phù hợp với giới hạn cụ thể này bên trong số nguyên 32 bit đã ký, mặc dù sử dụng dung lượng 64 bit là lựa chọn an toàn tiêu chuẩn để triển khai luồng. 

Có một số trường hợp việc triển khai có vẻ đơn giản lại có thể thất bại. Đầu tiên, xung đột không có nghĩa là toàn bộ một nhóm phải được chọn hoặc bị từ chối. Ví dụ,```
1 1
5 7
1
1 2
```có câu trả lời`7`, bởi vì chúng ta có thể chọn tính năng thứ hai và bỏ chọn tính năng đầu tiên. Một phương pháp tham lam chọn tính năng nặng hơn đã có hiệu quả ở đây, nhưng ý tưởng đó không thành công khi có một số xung đột tương tác với nhau. 

Ví dụ,```
2 2
6 5 7 7
3
1 3
1 4
2 3
```có câu trả lời`12`, thu được bằng cách chọn đặc điểm 2 và 4. Một quyết định tham lam về đặc điểm 1 có thể chặn cả hai đặc điểm bên phải và tạo ra một câu trả lời nhỏ hơn. 

Một trường hợp ranh giới khác là một đặc điểm không có xung đột. Vì```
1 1
4 9
1
1 2
```chúng ta không thể lấy cả hai đặc điểm, nhưng chúng ta có thể lấy đặc điểm trọng số 9, vì vậy câu trả lời là`9`. Trong xây dựng luồng, mỗi đỉnh vẫn phải nhận được trọng lượng riêng của nó ngay cả khi nó không có cạnh xung đột sự cố. 

Bẫy triển khai cuối cùng là khả năng sử dụng cho các cạnh xung đột. Nó phải lớn hơn tổng của tất cả các trọng số đối tượng. Nếu dung lượng được chọn quá nhỏ, mức cắt tối thiểu có thể thích cắt một cạnh xung đột hơn là trả tiền cho một đỉnh, điều này không tương ứng với việc loại bỏ một tính năng. Lựa chọn`sum(weights) + 1`tránh được vấn đề đó hoàn toàn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi tập hợp con của các đặc điểm (n+m). Đối với mỗi tập hợp con, chúng tôi kiểm tra xem nó có chứa cả hai điểm cuối của bất kỳ cạnh xung đột nào hay không. Nếu nó hợp lệ, chúng tôi tính toán trọng lượng của nó và giữ mức tối đa. Điều này đúng vì mọi lựa chọn có thể xuất hiện chính xác một lần trong số các tập hợp con. 

Tuy nhiên, với tối đa 200 đặc điểm, có thể có (2^{200}) tập hợp con. Ngay cả khi việc kiểm tra một tập hợp con chỉ mất thời gian không đổi thì đây cũng là trạng thái xấp xỉ (1.6\cdot10^{60}). Việc kiểm tra tới 10.000 xung đột cho mỗi tập hợp con sẽ khiến công việc lý thuyết trở nên lớn hơn. Giải pháp vũ phu rất hữu ích để hiểu vấn đề, nhưng không có cơ hội làm cho nó phù hợp với thời hạn. 

Cấu trúc của các cuộc xung đột cho chúng ta một con đường mạnh mẽ hơn nhiều. Biểu đồ là lưỡng cực và chúng tôi đang tìm kiếm một tập độc lập có trọng số tối đa. Mỗi tập độc lập chính xác là phần bù của một bìa đỉnh: nếu chúng ta loại bỏ tất cả các đỉnh bên ngoài một tập độc lập thì mọi cạnh xung đột phải có ít nhất một điểm cuối bị loại bỏ. Vì tất cả trọng lượng đều dương nên việc tối đa hóa trọng lượng được giữ lại tương đương với việc giảm thiểu tổng trọng lượng đã loại bỏ. 

Vì vậy, bài toán trở thành phủ đỉnh có trọng số nhỏ nhất trong đồ thị hai bên. Phiên bản có trọng số của định lý König có thể được giải bằng cách cắt (s)-(t) tối thiểu. 

Xây dựng một mạng luồng với (các) nguồn, tất cả các đỉnh nhóm thứ nhất, tất cả các đỉnh nhóm thứ hai và một điểm chìm (t). Cho mỗi đỉnh nhóm thứ nhất một cạnh từ (s) có dung tích bằng trọng số của nó. Cho mỗi đỉnh nhóm thứ hai một cạnh (t) với dung tích bằng trọng số của nó. Đối với mọi xung đột từ đỉnh trái sang đỉnh phải, hãy thêm một cạnh có hướng có dung lượng lớn hơn tổng trọng lượng của tất cả các đỉnh. 

Hãy xem xét việc cắt (s)-(t). Nếu đỉnh trái nằm ở cạnh nguồn thì cạnh nguồn của nó không bị cắt. Nếu nó di chuyển về phía nguồn, cạnh nguồn của nó bị cắt và chúng ta phải chịu trọng lượng của nó. Tương tự như vậy, một đỉnh bên phải ở phía nguồn làm cho cạnh của nó tới bồn bị cắt, gây ra trọng lượng của nó. 

Các cạnh xung đột rất lớn ngăn cản việc cắt tối thiểu đặt đỉnh trái và đỉnh phải xung đột ở các cạnh đối diện theo hướng sai. Vì luôn có sự cắt giảm chi phí nhiều nhất là tổng trọng lượng của tất cả các đỉnh, nên cạnh xung đột có dung lượng lớn hơn tổng đó không bao giờ có thể là một phần của mức cắt tối thiểu. Do đó, mỗi lần cắt tối thiểu tương ứng với một nắp đỉnh và dung lượng của nó chính xác là trọng lượng của nắp đó. 

Nếu lớp phủ đỉnh tối thiểu có trọng số (C), thì tập độc lập tối đa có trọng số 

[ 
\sum_i w_i-C. 
] 

Do đó, một phép tính luồng cực đại sẽ đưa ra câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^{n+m}k)) | (O(n+m+k)) | Quá chậm | 
| Tối ưu | (O(V^2E)) với Dinic | (O(V+E)) | Đã chấp nhận | 

Ở đây (V=n+m+2) và (E=k+n+m). Giới hạn (O(V^2E)) đã nêu là giới hạn tiêu chuẩn trong trường hợp xấu nhất đối với thuật toán Dinic trên các mạng có dung lượng nguyên chung. Với tối đa khoảng 202 đỉnh và 10.200 cạnh phía trước, điều này có thể dễ dàng quản lý được. 

## Hướng dẫn thuật toán

1. Đọc một trường hợp thử nghiệm và lưu trữ trọng số của tất cả (n+m) tính năng. Tính tổng trọng lượng của chúng. Cuối cùng chúng ta sẽ trừ đi trọng số tối thiểu của các đỉnh phải loại bỏ. 
2. Tạo mạng luồng chứa nguồn, (n) đỉnh đối tượng bên trái, (m) đỉnh đối tượng bên phải và phần chìm. Sử dụng các chỉ mục đỉnh bên trong dựa trên 0, đồng thời chuyển đổi từng số tính năng đầu vào từ lập chỉ mục dựa trên một. 
3. Thêm một cạnh từ nguồn vào mọi tính năng bên trái với dung lượng bằng trọng lượng của tính năng đó. Cắt bỏ cạnh này có nghĩa là loại bỏ tính năng đó khỏi tập hợp đã chọn, do đó chi phí cho việc làm đó chính xác là trọng lượng của nó. 
4. Thêm một cạnh từ mọi tính năng bên phải vào bồn rửa với dung tích bằng trọng lượng của nó. Việc cắt một cạnh như vậy có cách giải thích tương tự đối với đặc điểm bên phải. 
5. Với mọi xung đột ((p,q)), thêm một cạnh từ đỉnh trái tương ứng vào đỉnh phải tương ứng với dung lượng`total_weight + 1`. Dung lượng này lớn hơn một cách có chủ ý so với chi phí loại bỏ mọi tính năng. Một vết cắt tối thiểu sẽ không bao giờ chọn cắt một cạnh như vậy, do đó mọi xung đột phải được giải quyết bằng cách di chuyển ít nhất một trong các đỉnh điểm cuối của nó sang phía thích hợp của vết cắt. 
6. Chạy thuật toán luồng tối đa trên mạng kết quả. Theo định lý cắt tối thiểu luồng cực đại, giá trị luồng kết quả bằng công suất cắt tối thiểu. Theo cách xây dựng, vết cắt tối thiểu đó chính xác là trọng lượng tối thiểu của lớp phủ đỉnh. 
7. Trừ trọng lượng tối thiểu của đỉnh khỏi tổng trọng lượng đối tượng. Các đặc điểm còn lại tạo thành một tập độc lập có trọng số tối đa, vì vậy sự khác biệt này là câu trả lời bắt buộc. 

Tại sao nó hoạt động: xem xét mọi mức cắt tối thiểu và xem xét các đỉnh ở phía nguồn. Một đỉnh bên trái ở phía chìm đóng góp cạnh nguồn của nó vào vết cắt, trong khi một đỉnh bên phải ở phía nguồn đóng góp cạnh chìm của nó. Vì mọi cạnh xung đột có dung lượng lớn hơn tổng trọng số của tất cả các đỉnh nên không có đường cắt tối thiểu nào sử dụng cạnh xung đột. Do đó, mọi xung đột đều có ít nhất một điểm cuối bên ngoài tập hợp độc lập phía nguồn. Các đỉnh bị loại bỏ khỏi tập hợp đó tạo thành một bìa đỉnh và khả năng cắt bằng chính xác tổng trọng lượng của chúng. Ngược lại, bất kỳ lớp phủ đỉnh nào cũng có thể xác định một đường cắt có cùng trọng số bằng cách đặt các đỉnh bên trái đã loại bỏ ở phía sink và các đỉnh bên phải đã loại bỏ ở phía nguồn. Do đó, mức cắt tối thiểu chính xác là lớp phủ đỉnh có trọng lượng tối thiểu. Lấy phần bù của nó sẽ cho tập độc lập có trọng số tối đa, đây chính xác là lựa chọn tính năng cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Dinic:
    def __init__(self, n):
        self.n = n
        self.g = [[] for _ in range(n)]

    def add_edge(self, u, v, cap):
        forward = [v, cap, None]
        backward = [u, 0, forward]
        forward[2] = backward
        self.g[u].append(forward)
        self.g[v].append(backward)

    def bfs(self, s, t):
        level = [-1] * self.n
        level[s] = 0
        queue = [s]
        head = 0

        while head < len(queue):
            u = queue[head]
            head += 1

            for v, cap, rev in self.g[u]:
                if cap > 0 and level[v] == -1:
                    level[v] = level[u] + 1
                    queue.append(v)

        return level[t] != -1, level

    def dfs(self, u, t, pushed, level, it):
        if u == t:
            return pushed

        while it[u] < len(self.g[u]):
            edge = self.g[u][it[u]]
            v, cap, rev = edge

            if cap > 0 and level[v] == level[u] + 1:
                flow = self.dfs(v, t, min(pushed, cap), level, it)

                if flow:
                    edge[1] -= flow
                    rev[1] += flow
                    return flow

            it[u] += 1

        return 0

    def max_flow(self, s, t):
        flow = 0
        INF = 10**30

        while True:
            found, level = self.bfs(s, t)
            if not found:
                break

            it = [0] * self.n

            while True:
                pushed = self.dfs(s, t, INF, level, it)
                if pushed == 0:
                    break
                flow += pushed

        return flow

def solve(data):
    pos = 0
    out = []

    while pos < len(data):
        n = data[pos]
        m = data[pos + 1]
        pos += 2

        weights = data[pos:pos + n + m]
        pos += n + m

        k = data[pos]
        pos += 1

        conflicts = []
        for _ in range(k):
            p = data[pos] - 1
            q = data[pos + 1] - 1
            pos += 2
            conflicts.append((p, q))

        total = sum(weights)

        source = n + m
        sink = source + 1
        dinic = Dinic(n + m + 2)

        for i in range(n):
            dinic.add_edge(source, i, weights[i])

        for j in range(n, n + m):
            dinic.add_edge(j, sink, weights[j])

        inf = total + 1

        for p, q in conflicts:
            dinic.add_edge(p, q, inf)

        cover_weight = dinic.max_flow(source, sink)
        out.append(str(total - cover_weight))

    return "\n".join(out)

def main():
    data = list(map(int, sys.stdin.buffer.read().split()))
    sys.stdout.write(solve(data))

if __name__ == "__main__":
    main()
```các`Dinic`lớp lưu trữ từng cạnh dư cùng với một tham chiếu đến cạnh ngược của nó. Khi dòng chảy được đẩy về phía trước, công suất chuyển tiếp giảm và công suất ngược tăng lên. Khả năng đảo ngược đó là thứ cho phép các đường dẫn tăng cường sau này hoàn tác một quyết định trước đó. 

BFS xây dựng biểu đồ mức được Dinic sử dụng. Nó chỉ đi qua các cạnh có dung lượng dư dương, do đó các đỉnh hiện không thể nhận được nhiều luồng hơn sẽ bị bỏ qua. 

DFS gửi luồng chặn qua các cạnh tiến lên chính xác một cấp tại một thời điểm. các`it`mảng ghi nhớ cạnh đi ra nào được xem xét lần cuối cho mỗi đỉnh. Nếu không có nó, các cạnh bão hòa hoặc không sử dụng được có thể được quét liên tục, khiến quá trình triển khai chậm hơn nhiều. 

các`solve`đầu tiên hàm đọc toàn bộ dữ liệu đầu vào dưới dạng số nguyên. Điều này thuận tiện vì các trường hợp kiểm thử tiếp tục cho đến khi EOF và đầu vào không có số lượng trường hợp kiểm thử rõ ràng. vị trí`pos`di chuyển qua mảng số nguyên phẳng và tiêu thụ chính xác số lượng giá trị thuộc từng trường hợp thử nghiệm. 

Nguồn được lập chỉ mục là`n + m`, và bồn rửa như`n + m + 1`. Một tính năng được đánh số (p) trong đầu vào sẽ trở thành đỉnh`p - 1`, điều này là cần thiết vì đầu vào sử dụng lập chỉ mục dựa trên một trong khi mảng Python sử dụng lập chỉ mục dựa trên 0. 

Năng lực xung đột là`total + 1`, không phải là hằng số tùy ý như (10^9). Điều này là đủ vì việc cắt bỏ mọi tính năng có giá chính xác`total`, do đó, việc cắt giảm tối thiểu không bao giờ có thể khiến cạnh xung đột có giá cao hơn`total`. 

Không có vấn đề tràn số nguyên trong Python. Dung lượng hữu ích lớn nhất chỉ cao hơn một chút so với tổng của tất cả các trọng số, tối đa là (2\cdot10^9), nhưng các số nguyên có độ chính xác tùy ý của Python giúp việc triển khai trở nên mạnh mẽ ngay cả khi giới hạn đó thay đổi. 

Câu trả lời cuối cùng là`total - cover_weight`, trực tiếp thực hiện mối quan hệ phần bù giữa một tập độc lập và một bìa đỉnh. 

## Ví dụ đã hoạt động 

Câu lệnh được cung cấp chỉ chứa một mẫu thực tế, vì vậy dấu vết thứ hai sử dụng một trường hợp thử nghiệm được xây dựng nhỏ. 

Đối với Mẫu 1, bốn trọng số đặc trưng là (4,3,8,2). Các xung đột là (1)-(3), (1)-(4) và (2)-(4). 

Tổng trọng lượng là (17). Mạng luồng có công suất nguồn (4) và (3) cho các đỉnh bên trái và khả năng chứa (8) và (2) cho các đỉnh bên phải. 

| Bước | Trạng thái tính năng bên trái | Trạng thái tính năng phù hợp | Chi phí bảo hiểm tối thiểu | Trọng lượng đặt độc lập | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 1, 2 có sẵn | 3, 4 có sẵn | 0 | 17 | 
| Giải quyết xung đột với 1 | 1 hoặc đã xóa | 3, 4 | 4 hoặc 10 | phụ thuộc vào việc cắt | 
| Giải quyết xung đột 2-4 | 2 hoặc đã xóa | 4 hoặc bị loại bỏ | tối thiểu trở thành 5 | 12 | 
| Cuối cùng | loại bỏ tính năng 2 | giữ 3 và 4 | 5 | 12 | 

Lựa chọn tối ưu là đặc điểm 1 và 3, có trọng số (4+8=12), hoặc tương đương đặc điểm 3 và 4, cũng có trọng số (8+2=10). Lớp phủ đỉnh tối thiểu có trọng lượng (5), thu được bằng cách loại bỏ tính năng 1? Trên thực tế, tính năng 1 bao gồm xung đột với cả 3 và 4, nhưng xung đột 2-4 vẫn yêu cầu tính năng 2 hoặc 4. Chọn tính năng 1 và 2 làm chi phí trang trải (4+3=7), trong khi chọn tính năng 3 và chi phí 4 (10), và chọn chi phí tính năng 1 và 4 (6). Do đó, phạm vi bảo hiểm tối thiểu là tính năng 1 và 4, với chi phí (6), cho (17-6=11). Điều này cho thấy tại sao dấu vết phải tuân theo vết cắt thực tế thay vì đoán từ các xung đột riêng lẻ. 

Giá trị lưu lượng tối đa chính xác là`6`, vì vậy câu trả lời mẫu là`11`. 

Đối với ví dụ thứ hai, hãy xem xét:```
2 2
6 5 7 7
3
1 3
1 4
2 3
```Các lựa chọn hữu ích có thể có bao gồm tính năng 2 và 4, với tổng trọng số (5+7=12). Cấu trúc luồng tìm thấy bìa đỉnh tối thiểu có trọng số (13), ví dụ bằng cách loại bỏ các tính năng 1 và 2. Trọng số được đặt độc lập thu được là (25-13=12). 

| Bước | Lựa chọn phía nguồn | Đã xóa đỉnh | Cắt giảm chi phí | Giữ cân | 
| --- | --- | --- | --- | --- | 
| Ban đầu | tất cả các đỉnh được xem xét | không | 0 | 25 | 
| Bìa cạnh 1-3 | xóa 1 hoặc 3 | ứng cử viên 1 | 6 | 19 | 
| Bìa cạnh 1-4 | 1 đã bị xóa | ứng cử viên không thay đổi | 6 | 19 | 
| Bìa cạnh 2-3 | loại bỏ 2 hoặc 3 | ứng viên 1, 2 | 11 | 14 | 
| Cắt tối thiểu | xóa 1 và 2 | 1, 2 | 11 | 14 | 

Phạm vi tối thiểu thực tế ở đây là tính năng 1 và 2 với chi phí (6+5=11), do đó, tập độc lập tối đa có trọng số (25-11=14), thu được bằng cách chọn tính năng 3 và 4. Ví dụ này chứng minh tại sao việc tối ưu hóa phải xem xét toàn bộ cấu trúc xung đột thay vì xử lý một cách tham lam từng cạnh một. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(V^2E)) trường hợp xấu nhất với Dinic | (V=n+m+2\le202), trong khi (E=O(n+m+k)\le10200) | 
| Không gian | (O(V+E)) | Biểu đồ dư lưu trữ cả hai hướng của mọi cạnh mạng | 

Đồ thị rất nhỏ xét về các đỉnh và có nhiều nhất khoảng 10.200 cạnh tiến, do đó ngay cả trường hợp xấu nhất chung bị ràng buộc đối với Dinic cũng có tính thực tế ở đây. Bản thân việc xây dựng là tuyến tính về số lượng tính năng và xung đột. Việc sử dụng bộ nhớ cũng thoải mái dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

class Dinic:
    def __init__(self, n):
        self.n = n
        self.g = [[] for _ in range(n)]

    def add_edge(self, u, v, cap):
        f = [v, cap, None]
        r = [u, 0, f]
        f[2] = r
        self.g[u].append(f)
        self.g[v].append(r)

    def bfs(self, s, t):
        level = [-1] * self.n
        level[s] = 0
        q = [s]
        head = 0

        while head < len(q):
            u = q[head]
            head += 1

            for v, cap, rev in self.g[u]:
                if cap > 0 and level[v] == -1:
                    level[v] = level[u] + 1
                    q.append(v)

        return level[t] != -1, level

    def dfs(self, u, t, pushed, level, it):
        if u == t:
            return pushed

        while it[u] < len(self.g[u]):
            edge = self.g[u][it[u]]
            v, cap, rev = edge

            if cap > 0 and level[v] == level[u] + 1:
                f = self.dfs(v, t, min(pushed, cap), level, it)
                if f:
                    edge[1] -= f
                    rev[1] += f
                    return f

            it[u] += 1

        return 0

    def max_flow(self, s, t):
        ans = 0
        INF = 10**30

        while True:
            ok, level = self.bfs(s, t)
            if not ok:
                break

            it = [0] * self.n

            while True:
                f = self.dfs(s, t, INF, level, it)
                if f == 0:
                    break
                ans += f

        return ans

def solve(data):
    pos = 0
    ans = []

    while pos < len(data):
        n, m = data[pos], data[pos + 1]
        pos += 2

        w = data[pos:pos + n + m]
        pos += n + m

        k = data[pos]
        pos += 1

        edges = []
        for _ in range(k):
            p, q = data[pos] - 1, data[pos + 1] - 1
            pos += 2
            edges.append((p, q))

        total = sum(w)
        s = n + m
        t = s + 1

        flow = Dinic(t + 1)

        for i in range(n):
            flow.add_edge(s, i, w[i])

        for i in range(n, n + m):
            flow.add_edge(i, t, w[i])

        inf = total + 1

        for p, q in edges:
            flow.add_edge(p, q, inf)

        ans.append(str(total - flow.max_flow(s, t)))

    return "\n".join(ans)

def run(inp: str) -> str:
    data = list(map(int, inp.split()))
    return solve(data)

sample1 = """\
2 2
4 3 8 2
3
1 3
1 4
2 4
"""
assert run(sample1) == "11", "sample 1"

minimum = """\
1 1
5 7
1
1 2
"""
assert run(minimum) == "7", "minimum-size case"

all_equal = """\
2 2
5 5 5 5
4
1 3
1 4
2 3
2 4
"""
assert run(all_equal) == "10", "all-equal complete bipartite graph"

boundary = """\
2 2
1 100 99 2
2
1 3
2 4
"""
assert run(boundary) == "199", "large boundary weight"

no_real_conflict_choice = """\
1 1
10000000 10000000
1
1 2
"""
assert run(no_real_conflict_choice) == "10000000", "maximum weight boundary"

max_size_input = "100 100\n" + " ".join(["1"] * 200) + "\n10000\n"
max_size_input += "".join(
    f"{i} {100 + j}\n"
    for i in range(1, 101)
    for j in range(1, 101)
)
assert run(max_size_input) == "100", "maximum-size dense graph"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 / 4 3 8 2 / 3 conflicts`|`11`| Cung cấp mẫu và mức giảm cơ bản đến mức cắt tối thiểu | 
|`1 1 / 5 7 / 1 conflict`|`7`| Kích thước biểu đồ tối thiểu và trọng số không đối xứng | 
|`2 2 / 5 5 5 5 / 4 conflicts`|`10`| Biểu đồ lưỡng cực hoàn chỉnh và trọng số bằng nhau | 
|`2 2 / 1 100 99 2 / 2 conflicts`|`199`| Chênh lệch trọng lượng lớn và quyết định độc lập | 
|`1 1 / 10000000 10000000 / 1 conflict`|`10000000`| Trọng lượng cá nhân tối đa và khả năng xử lý | 
|`100 100 / 200 unit weights / 10000 conflicts`|`100`| Kích thước biểu đồ tối đa và tập hợp xung đột dày đặc | 

Thử nghiệm kích thước tối đa sử dụng mọi xung đột từ trái sang phải có thể xảy ra, tạo ra một kết quả hoàn chỉnh (K_{100,100}). Vì mọi cặp trong hai nhóm xung đột nhau, nên một lựa chọn không xung đột có thể chứa các đỉnh chỉ từ một phía, cho trọng số 100 vì tất cả 200 đỉnh đều có trọng số đơn vị. Thử nghiệm này đặc biệt hữu ích để phát hiện các triển khai vô tình bỏ qua một số cạnh hoặc sử dụng ranh giới chỉ mục tính năng không chính xác. 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là một xung đột duy nhất có trọng số không bằng nhau:```
1 1
5 7
1
1 2
```Tổng trọng số là 12. Mạng chứa các dung lượng 5 từ nguồn đến đỉnh trái và 7 từ đỉnh phải đến phần chìm, với cạnh xung đột có dung lượng 13. Việc cắt tối thiểu chọn đỉnh rẻ hơn để loại bỏ, cụ thể là đỉnh trái có trọng số 5. ​​Trọng số được đặt độc lập tối đa là (12-5=7). Việc triển khai bất cẩn luôn loại bỏ điểm cuối phù hợp sẽ trả về sai 5. 

Trường hợp cạnh thứ hai chứng minh tại sao xung đột phải được xem xét trên toàn cầu:```
2 2
6 5 7 7
3
1 3
1 4
2 3
```Tổng trọng số là 25. Chọn tính năng 3 và 4 sẽ cho 14 và không có xung đột. Lớp phủ đỉnh tối thiểu tương ứng có trọng số 11, bao gồm các tính năng 1 và 2. Do đó, giá trị luồng là 11 và câu trả lời là 14. Một phương pháp tham lam xử lý xung đột đầu tiên và loại bỏ ngay lập tức điểm cuối rẻ hơn cục bộ có thể đưa ra lựa chọn tương tác xấu với các xung đột sau này. 

Trường hợp cạnh thứ ba là đồ thị hai bên được kết nối hoàn toàn:```
2 2
5 5 5 5
4
1 3
1 4
2 3
2 4
```Mọi đặc điểm bên trái đều xung đột với mọi đặc điểm bên phải. Do đó, một lựa chọn hợp lệ chỉ có thể sử dụng các đỉnh từ một phía. Mỗi bên đóng góp 10, vì vậy câu trả lời là 10. Mạng luồng có độ phủ đỉnh tối thiểu có trọng số 10 và phần bù cũng có trọng số 10. Điều này kiểm tra xem công trình có xử lý nhiều cạnh xung đột xảy ra với mọi đỉnh hay không. 

Trường hợp cạnh thứ tư kiểm tra thang công suất hữu ích lớn nhất:```
1 1
10000000 10000000
1
1 2
```Tổng trọng lượng là 20.000.000 nên khả năng xung đột trở thành 20.000.001. Mức cắt giảm tối thiểu sẽ loại bỏ một trong hai tính năng với giá 10.000.000, để lại tính năng còn lại được chọn. Câu trả lời là 10.000.000. sử dụng`total + 1`vì dung lượng vô hạn là đủ và tránh dựa vào số ma thuật được mã hóa cứng. 

Trường hợp ranh giới cuối cùng là một đồ thị dày đặc với 100 đỉnh mỗi cạnh và tất cả 10.000 xung đột có thể xảy ra. Thuật toán thêm mọi cạnh xung đột và vì dung lượng của nó vượt quá tổng trọng lượng đỉnh nên mức cắt tối thiểu không thể cắt bất kỳ cạnh nào trong số đó. Thay vào đó, nó phải chọn toàn bộ một cạnh làm giải pháp thay thế che đỉnh rẻ hơn. Với trọng số đơn vị trên mỗi đỉnh, lớp phủ tối thiểu có giá 100 và tập độc lập tối đa cũng nặng 100. Điều này thực hiện các giá trị tối đa của cả số đỉnh và cạnh trong khi vẫn giữ nguyên bất biến toán học được sử dụng trong các trường hợp nhỏ hơn.
