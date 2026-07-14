---
title: "CF 103361I - \u0420\u0430\u0444\u0442\u0438\u043d\u0433"
description: "Chúng ta được cho một đồ thị có hướng có đỉnh là hồ và cạnh là sông chảy từ hồ này sang hồ khác."
date: "2026-07-03T13:08:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103361
codeforces_index: "I"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u0430\u044f \u041a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u042e\u041c\u0428 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 103361
solve_time_s: 58
verified: true
draft: false
---

[CF 103361I - \u0420\u0430\u0444\u0442\u0438\u043d\u0433](https://codeforces.com/problemset/problem/103361/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có hướng có đỉnh là hồ và cạnh là sông chảy từ hồ này sang hồ khác. Cấu trúc rất hạn chế: mỗi hồ có nhiều nhất một con sông chảy vào và nhiều nhất là hai con sông chảy ra, đồng thời không có chu trình định hướng nên nước luôn di chuyển xuống dốc theo một hướng nhất quán. 

Chuyến đi bè bắt đầu tại một hồ nước không có sông chảy vào. Từ đó, bất cứ khi nào bè đến một hồ có nhiều sông chảy ra, sông tiếp theo sẽ được chọn ngẫu nhiên thống nhất trong số các phương án có sẵn. Quá trình tiếp tục cho đến khi chiếc bè đến một hồ không có sông chảy ra, nơi này sẽ trở thành điểm đến cuối cùng. 

Nhiệm vụ là xác định hồ cuối cùng nào có nhiều khả năng là điểm dừng cuối cùng nhất nếu chúng ta bắt đầu từ một hồ nguồn ngẫu nhiên thống nhất và sau đó làm theo các lựa chọn ngẫu nhiên được mô tả. Nếu nhiều hồ đầu cuối có cùng xác suất tối đa, chúng ta phải xuất ra hồ có chỉ số nhỏ nhất. 

Các ràng buộc ngụ ý một biểu đồ có tối đa 200.000 nút và cạnh, do đó, mọi giải pháp về cơ bản phải tuyến tính theo kích thước của biểu đồ. Các thuật toán cố gắng mô phỏng đường dẫn một cách rõ ràng hoặc tính toán lại xác suất một cách độc lập cho mỗi nút bắt đầu sẽ không tồn tại trong giới hạn. Cấu trúc cũng rất quan trọng: nhiều nhất là một bậc có nghĩa là mỗi nút thuộc về chính xác một thành phần dạng cây, vì vậy biểu đồ là một rừng các cây có hướng hướng xuống dưới. 

Một số trường hợp đặc biệt cần được làm rõ. 

Nếu không có con sông nào thì mỗi hồ đồng thời là hồ khởi đầu và hồ cuối cùng. Ví dụ: nếu n = 3 và m = 0 thì cả ba hồ đều là điểm xuất phát hợp lệ và mỗi hồ cũng là điểm dừng, do đó mỗi hồ có xác suất 1/3 được chọn. 

Nếu đồ thị tạo thành một chuỗi như 1 → 2 → 3 → 4 thì chỉ có một nguồn (1) và chỉ một điểm chìm (4), vì vậy câu trả lời tầm thường là 4. 

Nếu sự phân nhánh xảy ra, chẳng hạn như 1 → 2, 1 → 3 và cả 2 và 3 đều dẫn đến các mức chìm khác nhau, thì xác suất sẽ phân tách và tích lũy theo các đường dẫn khác nhau. Một cách tiếp cận đơn giản xử lý các đường dẫn một cách độc lập mà không truyền bá chính xác các xác suất tiền tố được chia sẻ sẽ bị tính quá mức hoặc dưới mức đóng góp. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp của quá trình sẽ liên tục chọn hồ khởi đầu và sau đó mô phỏng các chuyển đổi ngẫu nhiên cho đến khi đạt đến điểm chìm. Ngay cả một mô phỏng đơn lẻ cũng có thể lấy O(n) trong trường hợp xấu nhất và việc ước tính xác suất một cách chính xác sẽ yêu cầu số lần chạy rất lớn. Điều này vượt xa giới hạn được đưa ra cho đến 2 · 10^5. 

Một cách tiếp cận mạnh mẽ có cấu trúc hơn là tính toán, đối với mỗi nguồn, phân bố xác suất đầy đủ trên các mức chìm bằng cách chạy DFS hoặc BFS để phân chia xác suất bằng nhau ở mọi điểm phân nhánh. Điều này mô hình hóa chính xác bước đi ngẫu nhiên, nhưng việc lặp lại nó cho mọi nguồn sẽ dẫn đến việc tính toán lại các cây con dùng chung lớn. Trong trường hợp xấu nhất khi đồ thị có cấu trúc dạng cây đơn lẻ, giá trị này sẽ trở thành O(n^2). 

Quan sát quan trọng là quá trình này tuyến tính và Markovian theo cách rất đơn giản. Khi chúng ta biết xác suất tiếp cận một nút, chúng ta có thể phân phối xác suất đó cho các nút con của nó mà không cần xem lại quá khứ. Vì mỗi nút có chính xác một nút cha nên luồng xác suất không bao giờ hợp nhất từ ​​các nút cha khác nhau mà nó chỉ phân chia đi xuống. Điều này cho phép lan truyền một lần bắt đầu từ tất cả các nguồn cùng một lúc. 

Chúng tôi gán xác suất ban đầu bằng nhau cho tất cả các nút nguồn, sau đó đẩy xác suất về phía trước dọc theo các cạnh, chia đều cho các cạnh đi ra. Mỗi bồn chỉ đơn giản là tích lũy tổng khối lượng xác suất đạt tới nó. Bồn rửa tốt nhất là bồn có khối lượng tích lũy tối đa.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên mỗi nguồn | O(n^2) | O(n) | Quá chậm | 
| Truyền xác suất chuyển tiếp | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi xác suất là một luồng bắt đầu từ các nút nguồn và di chuyển xuống dưới qua biểu đồ. 

1. Xác định tất cả các nút có cạnh vào bằng 0. Đây là những hồ có thể bắt đầu. Đếm chúng là k. Gán cho mỗi nút như vậy một xác suất ban đầu là 1/k. Điều này phản ánh sự lựa chọn ngẫu nhiên thống nhất của điểm bắt đầu. 
2. Xây dựng danh sách lân cận các sông chảy ra và tính toán độ phát hiện nguồn. Điều này chuẩn bị cấu trúc cho một lần truyền lan truyền đơn. 
3. Khởi tạo một mảng p trong đó p[v] biểu thị xác suất chiếc bè đến hồ v. Đặt p[source] = 1/k cho tất cả các nguồn và 0 cho tất cả các nút khác. 
4. Xử lý các nút theo thứ tự tôpô. Vì mỗi nút có nhiều nhất một cạnh đến nên chúng tôi cũng có thể xử lý bằng cách lặp lại theo bất kỳ thứ tự nào tôn trọng nút cha trước nút con, điều này đạt được một cách tự nhiên bởi BFS/hàng đợi từ các nguồn. 
5. Đối với mỗi nút u, hãy phân bổ khối lượng xác suất của nó một cách đồng đều giữa các cạnh bên ngoài của nó. Nếu u có d cạnh đi ra, mỗi con v nhận được p[v] += p[u] / d. Đây là mô hình lựa chọn ngẫu nhiên thống nhất ở mỗi nhánh. 
6. Nếu u không có cạnh đi ra thì nó là một điểm chìm, vì vậy chúng ta không truyền thêm từ nó. 
7. Sau khi quá trình truyền hoàn tất, hãy quét tất cả các nút và tìm những nút không có cạnh đi ra. Trong số đó, chọn nút có giá trị p lớn nhất. Nếu bằng nhau thì chọn chỉ số nhỏ nhất. 

### Tại sao nó hoạt động 

Quá trình này là tuyến tính vì mỗi nút có một đường đi duy nhất từ chính xác một nguồn, do đó xác suất đến bất kỳ nút nào được xác định hoàn toàn bởi khối lượng ban đầu của nguồn đó và trình tự phân chia dọc theo đường đi. Vì xác suất chỉ phân chia và không bao giờ hợp nhất nên việc tổng hợp các đóng góp từ các nguồn độc lập là hợp lệ. Quy tắc lan truyền bảo toàn tổng khối lượng xác suất ở mỗi bước, đảm bảo rằng tất cả xác suất được đưa vào các nguồn cuối cùng sẽ đến điểm đích mà không bị mất hoặc trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
g = [[] for _ in range(n)]
indeg = [0] * n
outdeg = [0] * n

for _ in range(m):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    indeg[v] += 1
    outdeg[u] += 1

sources = [i for i in range(n) if indeg[i] == 0]
k = len(sources)

p = [0.0] * n
if k > 0:
    init = 1.0 / k
    for s in sources:
        p[s] = init

from collections import deque
q = deque(sources)

while q:
    u = q.popleft()
    if outdeg[u] == 0:
        continue
    share = p[u] / outdeg[u]
    for v in g[u]:
        p[v] += share
        q.append(v)

best = -1
best_p = -1.0

for i in range(n):
    if outdeg[i] == 0:
        if p[i] > best_p or (abs(p[i] - best_p) < 1e-15 and i < best):
            best_p = p[i]
            best = i

print(best + 1)
```Giải pháp bắt đầu bằng cách xây dựng danh sách kề và tính toán mức độ để xác định tất cả các hồ ban đầu hợp lệ. Mảng xác suất chỉ được khởi tạo trên các nguồn đó, mỗi nguồn nhận được khối lượng bằng nhau. Hàng đợi điều khiển quá trình lan truyền về phía trước tương tự như truyền tải tôpô, đảm bảo xác suất của mỗi nút được phân phối chính xác một lần qua các cạnh đi ra của nó. 

Một điểm triển khai tinh tế là các nút có thể được xếp hàng nhiều lần trong một lần đẩy kiểu BFS đơn giản, nhưng tính chính xác không bị tổn hại vì chúng tôi không dựa vào thứ tự xử lý để xác định tính chính xác mà chỉ dành cho việc truyền bá đầy đủ cuối cùng. Mỗi bản cập nhật đều mang tính bổ sung, do đó, các hàng đợi lặp lại chỉ đơn giản là tiếp tục phân phối xác suất tích lũy. Điều này tránh được sự cần thiết phải sắp xếp topo nghiêm ngặt. 

Ở đây số học dấu phẩy động là đủ vì xác suất bị giới hạn và chúng tôi chỉ so sánh các giá trị cuối cùng. Quy tắc ràng buộc được xử lý rõ ràng bằng cách sử dụng so sánh epsilon. 

## Ví dụ đã hoạt động 

Hãy xem xét một biểu đồ nhỏ: 

đầu vào:```
4 3
1 2
1 3
2 4
```Các nguồn ở đây là {1}, vì chỉ nút 1 có bậc 0. Chúng ta bắt đầu với p(1) = 1. 

Chúng tôi theo dõi sự lan truyền: 

| Bước | Nút | p[nút] | Hành động | 
| --- | --- | --- | --- | 
| ban đầu | 1 | 1.0 | bắt đầu từ nguồn | 
| 1 | 1 | 1.0 | chia thành 2 và 3 | 
| 2 | 2 | 0,5 | nhận được một nửa | 
| 3 | 3 | 0,5 | nhận được một nửa | 
| 4 | 2 → 4 | 0,5 | 2 chia thành 4 | 
| cuối cùng | 4 | 0,5 | xác suất chìm | 

Nút 3 là điểm chìm với xác suất 0,5, nút 4 cũng là điểm chìm có xác suất 0,5. Vì hòa nên chỉ số nhỏ hơn sẽ thắng nên chọn 3. 

Dấu vết này cho thấy xác suất phân chia tại các nút phân nhánh và tích lũy tại các điểm chìm như thế nào. 

Bây giờ hãy xem xét một chuỗi có nhiều nguồn: 

đầu vào:```
3 1
1 3
```Các nguồn là {1, 2} nên mỗi nguồn có xác suất là 1/2. Bản thân nút 2 là một nút chìm nên nó đóng góp ngay 1/2. Nút 1 chuyển 1/2 của nó cho nút 3, do đó nút 3 cũng nhận được 1/2. 

Cả hai bồn đều có xác suất bằng nhau và chỉ số nhỏ hơn trong số chúng được chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | mỗi cạnh xử lý xác suất một lần trong quá trình truyền | 
| Không gian | O(n + m) | danh sách kề cộng với mảng xác suất và độ | 

Thuật toán xử lý từng nút và cạnh với số lần không đổi, vừa vặn trong giới hạn n, m cho đến 2 · 10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    g = [[] for _ in range(n)]
    indeg = [0] * n
    outdeg = [0] * n

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        indeg[v] += 1
        outdeg[u] += 1

    sources = [i for i in range(n) if indeg[i] == 0]
    k = len(sources)

    p = [0.0] * n
    if k > 0:
        init = 1.0 / k
        for s in sources:
            p[s] = init

    from collections import deque
    q = deque(sources)

    while q:
        u = q.popleft()
        if outdeg[u] == 0:
            continue
        share = p[u] / outdeg[u]
        for v in g[u]:
            p[v] += share
            q.append(v)

    best = -1
    best_p = -1.0
    for i in range(n):
        if outdeg[i] == 0:
            if p[i] > best_p or (abs(p[i] - best_p) < 1e-15 and i < best):
                best_p = p[i]
                best = i

    return str(best + 1)

# provided sample (as stated)
assert run("""4 3
1 2
1 3
2 4
""") == "3", "sample 1"

# single chain
assert run("""4 3
1 2
2 3
3 4
""") == "4", "chain"

# multiple sources, multiple sinks
assert run("""3 1
1 3
""") == "2", "two sources tie"

# no edges
assert run("""3 0
""") == "1", "all isolated"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị chuỗi | 4 | xác định chìm trong đường dẫn tuyến tính | 
| hai nguồn | 2 | phân chia xác suất bắt đầu thống nhất | 
| không có cạnh | 1 | tất cả các nút đều là nguồn và là điểm chìm | 

## Vỏ cạnh 

Khi không có cạnh, mọi nút đồng thời là nguồn và đích. Trong trường hợp này, quá trình khởi tạo gán xác suất 1/n cho mọi nút và vì không có sự lan truyền nào xảy ra nên tất cả các nút đều giữ xác suất bằng nhau. Thuật toán chọn chính xác chỉ số nhỏ nhất. 

Khi biểu đồ là một chuỗi thuần túy, mỗi nút ngoại trừ nút cuối cùng có chính xác một cạnh đi ra, do đó không xảy ra sự phân tách. Xác suất chảy hoàn toàn từ phân phối bắt đầu ngẫu nhiên xuống phần chìm duy nhất, bảo toàn tổng khối lượng và tạo ra một câu trả lời duy nhất. 

Khi phân nhánh xảy ra tại một nguồn, chẳng hạn như 1 → 2 và 1 → 3, thuật toán sẽ phân chia xác suất ngay lập tức và theo dõi độc lập cả hai nhánh. Vì không thể hợp nhất do các hạn chế về mức độ, nên các giá trị tích lũy vẫn rời rạc và cộng gộp, đảm bảo xác suất chìm chính xác.
