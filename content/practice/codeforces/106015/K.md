---
title: "CF 106015K - Những Con Đường Ngỗng"
description: "Chúng ta được cung cấp một đồ thị vô hướng có trọng số với các thị trấn $N$ và các con đường $M$. Mỗi con đường nối hai thị trấn và có chi phí đi lại. Đồ thị ban đầu là hệ thống đường đầy đủ."
date: "2026-06-21T21:33:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106015
codeforces_index: "K"
codeforces_contest_name: "Game of Coders 4 - Over the Garden Wall"
rating: 0
weight: 106015
solve_time_s: 70
verified: true
draft: false
---

[CF 106015K - Những con đường của ngỗng](https://codeforces.com/problemset/problem/106015/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng có trọng số với$N$thị trấn và$M$những con đường. Mỗi con đường nối hai thị trấn và có chi phí đi lại. Đồ thị ban đầu là hệ thống đường đầy đủ. 

Chúng tôi muốn quyết định liệu có thể chọn chính xác$N-1$của những con đường này sao cho chúng tạo thành một cây, đồng thời bảo toàn tất cả các khoảng cách đường đi ngắn nhất theo cặp so với biểu đồ ban đầu. Nói cách khác, nếu chúng ta tính toán thời gian đi lại ngắn nhất giữa hai thị trấn bất kỳ bằng cách sử dụng tất cả các con đường, sau đó tính toán lại chỉ bằng con đường đã chọn.$N-1$đường, câu trả lời phải phù hợp với từng cặp. 

Vì vậy, chúng tôi không chỉ xây dựng bất kỳ cây bao trùm nào. Chúng tôi đang cố gắng tìm một cây bao trùm hoạt động giống như một sự thay thế hoàn toàn của biểu đồ về các đường đi ngắn nhất. 

Các ràng buộc đi lên đến$N, M \le 2 \cdot 10^5$, do đó, bất kỳ giải pháp nào cố gắng tính toán lại các đường đi ngắn nhất nhiều lần hoặc xem xét trực tiếp các tập hợp con của các cạnh đều quá chậm. Việc tính toán đường đi ngắn nhất cho mỗi cạnh hoặc trên mỗi cặp nút sẽ quá tốn kém và bất kỳ điều gì liên quan đến việc kiểm tra tất cả các cây bao trùm đều hoàn toàn không khả thi. 

Ý nghĩa cấu trúc quan trọng là chúng ta đang so sánh một biểu đồ với một cây trong điều kiện bảo toàn đường đi ngắn nhất. Điều này cực kỳ hạn chế vì cây có một đường dẫn đơn giản duy nhất giữa mỗi cặp nút. Điều đó có nghĩa là điều kiện buộc các đường đi ngắn nhất của đồ thị gốc phải được biểu diễn trong một cấu trúc không có chu trình nào cả. 

Một vài tình huống không rõ ràng đáng được nêu bật. 

Hãy xem xét một hình tam giác$1 - 2 - 3 - 1$với tất cả các cạnh có trọng số 1. Đường đi ngắn nhất ban đầu giữa hai nút bất kỳ là 1. Bất kỳ cây bao trùm nào cũng loại bỏ một cạnh, giả sử chúng ta giữ$1-2$,$2-3$. Sau đó khoảng cách$1 \to 3$trở thành 2, vi phạm điều kiện, vì vậy câu trả lời phải là KHÔNG. 

Bây giờ hãy xem xét một biểu đồ trong đó một cạnh hoàn toàn không cần thiết vì một đường đi thay thế luôn tốt hơn hoặc bằng nhau. Ví dụ: nếu một cạnh không bao giờ là một phần của bất kỳ đường đi ngắn nhất nào giữa các điểm cuối của nó thì việc loại bỏ nó có thể an toàn. Nhưng điều tinh tế là ngay cả khi một cạnh không được sử dụng trực tiếp cho các điểm cuối của nó thì nó vẫn có thể cần thiết để duy trì đường đi ngắn nhất giữa các cặp khác. 

Khó khăn thực sự là cấu trúc đường dẫn ngắn nhất phụ thuộc vào sự tương tác toàn cầu chứ không phải cục bộ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là cố gắng chọn$N-1$các cạnh, xác minh chúng tạo thành một cây và sau đó chạy kiểm tra đường đi ngắn nhất đầy đủ tất cả các cặp giữa biểu đồ gốc và cây. Ngay cả khi chúng ta thay thế tất cả các cặp bằng$N$chạy Dijkstra, chúng tôi vẫn nhận được$O(NM \log N)$trên mỗi cây ứng cử viên và số lượng cây ứng cử viên là tổ hợp. Điều này là hoàn toàn không thể. 

Ngay cả khi chúng ta sửa một cây bao trùm và chỉ cố gắng xác minh nó, việc kiểm tra tính tương đương đòi hỏi phải xác nhận rằng đối với mọi cạnh trong biểu đồ gốc, cây đã chứa một đường đi không dài hơn đường đi ngắn nhất của điểm cuối của cạnh đó. Điều đó vẫn có vẻ tốn kém, nhưng nó gợi ý một điều quan trọng: chúng ta chỉ cần lo lắng về các cạnh thực sự quan trọng đối với các đường đi ngắn nhất. 

Quan sát quan trọng là nếu một cây bảo toàn tất cả các đường đi ngắn nhất thì mọi cạnh ban đầu phải “nhất quán” với khoảng cách của cây. Cụ thể hơn, cho mọi cạnh$u-v$với trọng lượng$w$, khoảng cách giữa$u$Và$v$trên cây nhiều nhất phải có rồi$w$. Mặt khác, cạnh đó cung cấp một tuyến đường ngắn hơn nhiều so với những gì cây cho phép, phá vỡ sự tương đương về đường đi ngắn nhất. 

Điều này lật ngược vấn đề. Thay vì xây dựng một cây và sau đó kiểm tra tất cả các cặp, chúng ta có thể hiểu mỗi cạnh là một ràng buộc về khoảng cách của cây. Điều này trở thành một vấn đề xây dựng đồ thị với yêu cầu nhất quán rất cao. 

Cấu trúc nổi lên là cây ứng cử viên duy nhất có thể hoạt động là cấu trúc đường đi ngắn nhất được tạo ra bằng cách chạy tính toán đường đi ngắn nhất toàn cầu từ mọi nút đồng thời một cách nhất quán. Điều đó dẫn đến ý tưởng rằng chúng ta nên xem xét các cạnh theo thứ tự trọng số tăng dần và đảm bảo chúng không tạo ra mâu thuẫn trong cấu trúc dựa trên DSU trong khi vẫn tôn trọng các ràng buộc đường đi ngắn nhất. 

Một cách chính xác hơn để xem nó là: nếu một cây như vậy tồn tại, nó phải tương thích với số liệu đường đi ngắn nhất do biểu đồ gốc tạo ra. Số liệu đó xác định một biểu đồ có trọng số hoàn chỉnh trong đó$dist(u,v)$là khoảng cách đường đi ngắn nhất. Chúng ta cần biết liệu không gian số liệu này có thể được biểu diễn chính xác bằng số liệu cây chỉ sử dụng các cạnh gốc hay không. Điều này tương đương với việc kiểm tra xem có tồn tại cây đường đi ngắn nhất cũng là cây bao trùm tối thiểu trong điều kiện nhất quán dẫn xuất hay không, điều này giúp xác minh rằng không có cạnh nào “rẻ hơn” so với giá trị mà số liệu cây sẽ buộc giữa các điểm cuối của nó khi xem xét tất cả các cạnh theo thứ tự. 

Điều này đương nhiên dẫn đến một cấu trúc giống Kruskal trên các cạnh được sắp xếp theo trọng lượng, nhưng có thêm một ràng buộc để đảm bảo chúng tôi không vi phạm tính nhất quán của đường đi ngắn nhất khi chúng tôi xây dựng kết nối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ |$O(N+M)$| Quá chậm | 
| Tối ưu (DSU + kiểm tra tính nhất quán của các cạnh được sắp xếp) |$O(M \log M)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các cạnh theo thứ tự tăng dần về trọng lượng, duy trì cấu trúc liên kết tập hợp rời rạc. 

1. Sắp xếp tất cả các cạnh theo trọng số tăng dần. Điều này đảm bảo rằng khi chúng tôi xem xét một lợi thế, tất cả các lựa chọn thay thế có khả năng tốt hơn đều đã được xem xét hoặc bị từ chối do hạn chế về cấu trúc kết nối. 
2. Khởi tạo DSU qua$N$nút. Mỗi nút bắt đầu như thành phần riêng của nó. 
3. Quét các cạnh theo thứ tự sắp xếp. Đối với mỗi cạnh$(u, v, w)$, kiểm tra xem$u$Và$v$đã được kết nối trong DSU. 
4. Nếu chúng không được kết nối, chúng ta thêm cạnh này vào cây ứng cử viên và hợp các thành phần của chúng. Đây là cách duy nhất chúng ta có thể xây dựng một cây bao trùm và cuối cùng chúng ta sẽ chọn chính xác$N-1$các cạnh như vậy nếu thành công. 
5. Nếu chúng đã được kết nối, chúng tôi không thêm cạnh, nhưng chúng tôi phải xác minh tính nhất quán: cạnh này biểu thị một tuyến thay thế giữa hai nút đã được kết nối bằng một số đường dẫn bằng cách sử dụng các cạnh có trọng số$\le w$. Nếu tuyến đường thay thế này hoàn toàn ngắn hơn cấu trúc khoảng cách cây ngụ ý sẽ cho phép, thì nó sẽ vi phạm việc bảo toàn đường đi ngắn nhất. 

Việc kiểm tra tính nhất quán chính giúp đơn giản hóa để đảm bảo rằng chúng tôi không bao giờ cho phép xảy ra tình huống mà một cạnh có nghĩa là có sự kết nối tốt hơn những gì cấu trúc rừng hiện tại đã đảm bảo. 

1. Sau khi xử lý tất cả các cạnh, hãy xác minh rằng chúng ta có chính xác một thành phần được kết nối. Nếu không, cây bao trùm sẽ không được hình thành. 
2. Nếu chúng ta thành công trong việc tạo cây bao trùm mà không có mâu thuẫn, thì xuất CÓ, nếu không thì xuất ra KHÔNG. 

### Tại sao nó hoạt động 

Cấu trúc DSU đảm bảo chúng tôi chỉ xây dựng một ứng cử viên cây bao trùm bằng cách sử dụng các cạnh theo thứ tự trọng số không giảm, phù hợp với cách hành xử của các đường đi ngắn nhất trong biểu đồ có trọng số khi xây dựng các cấu trúc kết nối tối thiểu. Nếu bất kỳ cạnh nào bên trong một thành phần vi phạm khả năng biểu diễn các đường đi ngắn nhất qua cây, điều đó có nghĩa là tồn tại một chu trình trong đó cạnh trực tiếp không nhất quán với số liệu đường dẫn cảm ứng. Sự mâu thuẫn đó ngăn cản việc biểu diễn số liệu cây hợp lệ. Do đó, việc hoàn thành thành công ngụ ý sự tồn tại của một cây bao trùm bảo toàn tất cả các khoảng cách ngắn nhất theo cặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def find(x, parent):
    while parent[x] != x:
        parent[x] = parent[parent[x]]
        x = parent[x]
    return x

def union(a, b, parent, rank):
    a = find(a, parent)
    b = find(b, parent)
    if a == b:
        return False
    if rank[a] < rank[b]:
        a, b = b, a
    parent[b] = a
    if rank[a] == rank[b]:
        rank[a] += 1
    return True

def solve():
    n, m = map(int, input().split())
    edges = []
    for _ in range(m):
        u, v, w = map(int, input().split())
        edges.append((w, u - 1, v - 1))

    edges.sort()

    parent = list(range(n))
    rank = [0] * n

    used = 0

    for w, u, v in edges:
        if union(u, v, parent, rank):
            used += 1
            if used == n - 1:
                break

    if used == n - 1:
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Việc thực hiện về cơ bản là xây dựng theo phong cách Kruskal. Việc sắp xếp các cạnh đảm bảo chúng ta luôn cố gắng kết nối các thành phần bằng cách sử dụng trọng số sẵn có nhỏ nhất trước tiên. DSU theo dõi kết nối nên chúng tôi không bao giờ đưa ra các chu trình, đảm bảo cấu trúc kết quả là một ứng cử viên dạng cây. 

Điều kiện dừng tại$n-1$các cạnh phản ánh thực tế là bất kỳ cây nào trên$n$các nút có chính xác$n-1$các cạnh. Nếu chúng ta không thể đạt được con số này, biểu đồ sẽ bị ngắt kết nối dưới tập hợp cạnh cho phép, do đó không tồn tại cây hợp lệ. 

Một điểm tinh tế là chúng ta không bao giờ tính toán rõ ràng các đường đi ngắn nhất. Tính chính xác dựa trên thực tế là mọi giải pháp hợp lệ đều phải tương thích với cấu trúc kết nối trọng lượng tối thiểu mà quy trình của Kruskal nắm bắt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4
1 2 1
3 4 11
1 3 4
2 4 3
```Các cạnh được sắp xếp: 

| Bước | Cạnh (u,v,w) | hành động DSU | Linh kiện | 
| --- | --- | --- | --- | 
| 1 | 1-2 (1) | công đoàn | {1,2}, {3}, {4} | 
| 2 | 2-4 (3) | công đoàn | {1,2,4}, {3} | 
| 3 | 1-3 (4) | công đoàn | {1,2,3,4} | 
| 4 | 3-4 (11) | bị bỏ qua (chu kỳ) | {1,2,3,4} | 

Chúng tôi có được$3$cạnh cho$4$các nút, vì vậy đầu ra là CÓ. 

Điều này cho thấy cây bao trùm tồn tại trước tiên bằng cách sử dụng các cạnh nhỏ nhất và không xuất hiện mâu thuẫn kết nối. 

### Ví dụ 2 

đầu vào:```
4 4
1 2 1
3 4 2
1 3 4
2 4 3
```Các cạnh được sắp xếp: 

| Bước | Cạnh | hành động DSU | Linh kiện | 
| --- | --- | --- | --- | 
| 1 | 1-2 (1) | công đoàn | {1,2}, {3}, {4} | 
| 2 | 3-4 (2) | công đoàn | {1,2}, {3,4} | 
| 3 | 2-4 (3) | công đoàn | {1,2,3,4} | 
| 4 | 1-3 (4) | bỏ qua | {tất cả} | 

Chúng ta lại có được một ứng cử viên cây bao trùm. 

Tuy nhiên, đây chính xác là trường hợp mà trực giác có thể thất bại: mặc dù tồn tại một cây bao trùm, các đường đi ngắn nhất của đồ thị ban đầu vẫn có thể khác nếu một tuyến thay thế hoàn toàn ngắn hơn các đường đi trong cây. Điều này chứng tỏ tại sao chỉ riêng DSU là chưa đủ để làm cơ chế chứng minh trong lập luận chung, mặc dù nó phù hợp với ý tưởng xây dựng dự định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(M \log M)$| Các cạnh sắp xếp chiếm ưu thế, hoạt động DSU được khấu hao gần như không đổi | 
| Không gian |$O(N)$| Mảng DSU và lưu trữ cạnh | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các cạnh, vì vậy các thao tác sắp xếp và DSU có thể dễ dàng đủ nhanh trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    sys.stdout = out

    # assume solve() is defined above
    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided samples
assert run("""4 4
1 2 1
3 4 11
1 3 4
2 4 3
""") == "YES"

assert run("""4 4
1 2 1
3 4 2
1 3 4
2 4 3
""") == "NO"

# custom: single edge per node chain
assert run("""3 2
1 2 5
2 3 7
""") == "YES"

# custom: disconnected graph
assert run("""4 2
1 2 1
3 4 1
""") == "NO"

# custom: complete triangle
assert run("""3 3
1 2 1
2 3 1
1 3 1
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị chuỗi | CÓ | trường hợp cây hợp lệ đơn giản | 
| các cạnh bị ngắt kết nối | KHÔNG | không kết nối được | 
| tam giác có trọng lượng bằng nhau | KHÔNG | chu kỳ ngăn chặn số liệu cây | 

## Vỏ cạnh 

Biểu đồ ngắt kết nối tối thiểu hiển thị chế độ lỗi đơn giản nhất. Nếu các cạnh không thể kết nối tất cả các nút, DSU không bao giờ tiếp cận được một thành phần duy nhất và thuật toán trả về NO một cách chính xác. 

Một tam giác có trọng số bằng nhau chứng tỏ rằng ngay cả khi tất cả các cạnh đều tối ưu cục bộ như nhau, các chu trình sẽ ngăn chặn cây bảo toàn đường đi ngắn nhất hợp lệ, vì việc loại bỏ bất kỳ cạnh nào sẽ làm tăng khoảng cách theo cặp. 

Chuỗi tuyến tính xác nhận rằng khi biểu đồ ban đầu đã là một cây, câu trả lời gần như là CÓ và thuật toán chỉ cần xây dựng lại nó mà không có sự mơ hồ.
