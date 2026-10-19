---
title: "CF 105677I - Bói toán"
description: "Chúng ta có một tập hợp N đồ vật, mỗi đồ vật đại diện cho một tờ giấy. Mỗi bài báo chứa một danh sách các tài liệu tham khảo đến các bài báo khác, tạo thành một biểu đồ có hướng trong đó một mũi tên đi từ một bài báo đến mỗi bài báo mà nó trích dẫn."
date: "2026-06-22T05:08:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105677
codeforces_index: "I"
codeforces_contest_name: "2024-2025 ICPC Southwestern European Regional Contest (SWERC 2024)"
rating: 0
weight: 105677
solve_time_s: 49
verified: true
draft: false
---

[CF 105677I - Bói toán](https://codeforces.com/problemset/problem/105677/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp N đồ vật, mỗi đồ vật đại diện cho một tờ giấy. Mỗi bài báo chứa một danh sách các tài liệu tham khảo đến các bài báo khác, tạo thành một biểu đồ có hướng trong đó một mũi tên đi từ một bài báo đến mỗi bài báo mà nó trích dẫn. Cấu trúc được đảm bảo không có chu kỳ định hướng, vì vậy nếu bạn theo dõi các trích dẫn về phía trước, bạn sẽ không bao giờ có thể quay lại bài báo cũ. 

Nhiệm vụ là xác định xem liệu các bài báo này có thể được sắp xếp thành một chuỗi duy nhất sử dụng tất cả chúng chính xác một lần hay không, sao cho mỗi bài báo đều trích dẫn bài tiếp theo trong chuỗi. Trong thuật ngữ đồ thị, chúng ta được hỏi liệu đồ thị chu trình có hướng có chứa đường Hamilton đi theo các hướng của cạnh hay không. 

Ràng buộc N lên tới 100.000 và tổng số cạnh lên tới 500.000 hàm ý chúng ta phải hoạt động trong thời gian tuyến tính hoặc gần tuyến tính. Bất kỳ giải pháp nào cố gắng kiểm tra tất cả các hoán vị hoặc tất cả các đường dẫn một cách rõ ràng đều không khả thi ngay lập tức vì ngay cả O(N^2) cũng sẽ quá chậm ở quy mô này. 

Một điểm tinh tế là chúng ta không được hỏi liệu đồ thị có được kết nối theo nghĩa vô hướng hay không. Một biểu đồ có thể được kết nối và vẫn không đạt được điều kiện nếu nó phân nhánh. Ngược lại, nhiều thành phần bị ngắt kết nối khiến không thể tạo thành một chuỗi duy nhất bao gồm tất cả các nút. 

Một trường hợp thất bại phổ biến xuất phát từ việc giả định rằng “không theo chu kỳ” ngụ ý một thứ tự hợp lệ sẽ tự động hoạt động. Ví dụ: nếu chúng ta có ba nút có cạnh 1 → 2 và 1 → 3, thì đồ thị là DAG, nhưng không có cách nào sắp xếp cả ba nút thành một chuỗi trong đó mỗi bước đi theo một cạnh, vì sau 1, chúng ta không thể đi tới cả 2 và 3 theo trình tự. 

Một trường hợp thất bại khác xuất phát từ việc giả định bất kỳ thứ tự tôpô nào cũng hoạt động. Thứ tự tôpô có thể đặt các nút theo thứ tự phụ thuộc hợp lệ nhưng vẫn không tương ứng với đường dẫn trong đó các nút liên tiếp được kết nối trực tiếp bằng các cạnh. 

## Phương pháp tiếp cận 

Giải thích bạo lực sẽ cố gắng xây dựng một đường dẫn bắt đầu từ mọi nút, thực hiện DFS hoặc quay lui để cố gắng mở rộng đường dẫn từng bước trong khi đánh dấu các nút đã truy cập. Trong trường hợp xấu nhất, mỗi nút có thể phân nhánh thành nhiều nút khác và việc khám phá tất cả các khả năng có thể dẫn đến hành vi giai thừa hoặc hàm mũ. Ngay cả việc cắt bớt các trạng thái đã truy cập vẫn khám phá ra một số lượng lớn các hoán vị từng phần vì biểu đồ chỉ bị ràng buộc là không theo chu kỳ chứ không phải tuyến tính. 

Quan sát quan trọng là nếu tồn tại một chuỗi đầy đủ như vậy thì về cơ bản đồ thị sẽ bị ép vào một cấu trúc gần tuyến tính. Trong một giải pháp hợp lệ, mọi nút ngoại trừ nút cuối cùng phải có chính xác một lựa chọn “tiếp theo” dọc theo chuỗi. Điều này cho thấy rằng toàn bộ cấu trúc có thể được xác minh bằng cách sử dụng thứ tự tôpô, bởi vì trong DAG, bất kỳ đường dẫn Hamilton nào cũng phải xuất hiện dưới dạng thứ tự tôpô hợp lệ. 

Khi tính toán thứ tự tôpô, chúng ta chỉ cần kiểm tra xem mỗi cặp liên tiếp trong thứ tự đó có được kết nối bởi một cạnh có hướng hay không. Nếu điều đó đúng thì bản thân thứ tự đó là một đường đi Hamilton hợp lệ. Nếu nó không đúng thì không có đường đi Hamilton nào có thể tồn tại, bởi vì mọi đường đi hợp lệ sẽ phải xuất hiện dưới dạng trật tự tôpô và việc thiếu các cạnh giữa các vị trí liên tiếp cho thấy đồ thị không thể hỗ trợ một chuỗi đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quay lại DFS trên các đường dẫn | O(N!) | O(N) | Quá chậm | 
| Sắp xếp cấu trúc liên kết + kiểm tra kề | O(N + M) | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý cấu trúc trích dẫn như một biểu đồ có hướng và sử dụng quy trình sắp xếp tôpô dựa trên mức độ tiêu chuẩn.

1. Xây dựng danh sách kề cho đồ thị và tính mức độ của mỗi nút. Điều này cho phép chúng tôi xác định nút nào không có điều kiện tiên quyết, nghĩa là không có trích dẫn đến. 
2. Khởi tạo một hàng đợi với tất cả các nút có mức độ bằng 0. Các nút này có thể xuất hiện đầu tiên theo bất kỳ thứ tự hợp lệ nào vì không có gì phụ thuộc vào chúng. 
3. Liên tục xóa một nút khỏi hàng đợi, thêm nó vào thứ tự tôpô và giảm mức độ của tất cả các nút mà nó trỏ tới. Khi mức độ của một nút trở thành 0, nó sẽ được thêm vào hàng đợi. Quá trình này mô phỏng việc loại bỏ các giấy tờ “có sẵn” theo thứ tự tôn trọng sự phụ thuộc hợp lệ. 
4. Sau khi xử lý tất cả các nút, chúng ta thu được thứ tự tôpô của tất cả các giấy tờ. Vì biểu đồ được đảm bảo không theo chu kỳ nên mọi nút sẽ được xử lý. 
5. Xác minh xem thứ tự này có tạo thành một chuỗi hay không bằng cách kiểm tra từng cặp liền kề trong thứ tự. Với mỗi cặp liên tiếp (u, v), chúng ta kiểm tra xem có cạnh trực tiếp u → v hay không. 
6. Nếu tất cả các cặp liên tiếp được kết nối bằng các cạnh, chúng ta trả về 1, vì bản thân thứ tự đó là một chuỗi trích dẫn đầy đủ hợp lệ. Nếu không, chúng tôi trả về 0. 

### Tại sao nó hoạt động 

Trong DAG, bất kỳ đường đi Hamilton nào cũng phải tuân theo thứ tự tôpô vì các cạnh luôn tiến về phía trước dọc theo các phụ thuộc. Nếu đường dẫn Hamilton tồn tại, nó sẽ xác định tổng thứ tự của tất cả các nút. Sắp xếp tôpô là một phần mở rộng tuyến tính khác có cùng thứ tự từng phần. Nếu đồ thị thừa nhận một đường đi Hamilton, thì tất cả các loại tôpô hợp lệ phải nhất quán với thứ tự đường đi đó, vì việc đảo ngược bất kỳ cặp nào trong đường đi sẽ mâu thuẫn với sự tồn tại của một cạnh có hướng. 

Do đó, nếu chúng ta tính toán thứ tự tôpô và thấy rằng một số cặp liền kề không được kết nối trực tiếp, điều đó hàm ý rằng có một “khoảng trống” nơi đường đi không thể tiếp tục, nghĩa là không tồn tại đường đi Hamilton nào cả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n = int(input())
    adj = [[] for _ in range(n)]
    indeg = [0] * n

    for i in range(n):
        parts = list(map(int, input().split()))
        c = parts[0]
        for x in parts[1:]:
            x -= 1
            adj[i].append(x)
            indeg[x] += 1

    q = deque([i for i in range(n) if indeg[i] == 0])
    topo = []

    while q:
        u = q.popleft()
        topo.append(u)
        for v in adj[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                q.append(v)

    pos = [0] * n
    for i, x in enumerate(topo):
        pos[x] = i

    has_edge = set()
    for u in range(n):
        for v in adj[u]:
            has_edge.add((u, v))

    for i in range(n - 1):
        u = topo[i]
        v = topo[i + 1]
        if (u, v) not in has_edge:
            print(0)
            return

    print(1)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách xây dựng danh sách kề và mảng mức độ trực tiếp từ danh sách trích dẫn. Hàng đợi chứa tất cả các nút hiện đang trống, điều này đảm bảo chúng tôi luôn tôn trọng các ràng buộc phụ thuộc. 

Sau khi tạo thứ tự tôpô, chúng tôi xây dựng cấu trúc tra cứu nhanh cho các cạnh bằng cách sử dụng một tập hợp các cặp. Điều này làm cho việc kiểm tra kề giữa các nút liên tiếp O(1), tránh việc quét lặp đi lặp lại danh sách kề. 

Vòng cuối cùng là bước xác minh quan trọng. Nó thực thi rằng thứ tự tôpô không chỉ hợp lệ theo nghĩa phụ thuộc mà còn tạo thành một đường dẫn liên tục không có điểm dừng. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp đồ thị tạo thành một chuỗi đơn giản 1 → 2 → 3 → 4. 

| Bước | Xếp hàng | Xuất hiện | Đặt hàng Topo | Thay đổi mức độ | 
| --- | --- | --- | --- | --- | 
| 1 | [1] | 1 | [1] | 2 trở thành 0 | 
| 2 | [2] | 2 | [1,2] | 3 trở thành 0 | 
| 3 | [3] | 3 | [1,2,3] | 4 trở thành 0 | 
| 4 | [4] | 4 | [1,2,3,4] | xong | 

Mỗi cặp liên tiếp đều có một cạnh trực tiếp, vì vậy câu trả lời là 1. Điều này xác nhận rằng cấu trúc chuỗi thuần túy được nhận dạng chính xác. 

Bây giờ hãy xem xét một DAG phân nhánh trong đó 1 → 2 và 1 → 3, không có cạnh nào nằm giữa 2 và 3. 

| Bước | Xếp hàng | Xuất hiện | Đặt hàng Topo | 
| --- | --- | --- | --- | 
| 1 | [1] | 1 | [1] | 
| 2 | [2,3] | 2 | [1,2] | 
| 3 | [3] | 3 | [1,2,3] | 

Khi kiểm tra các cặp liên tiếp, chúng ta thấy rằng 1 → 2 tồn tại nhưng 2 → 3 thì không, do đó thuật toán cho ra 0. Điều này chứng tỏ rằng mặc dù đồ thị có tính tuần hoàn và được sắp xếp đầy đủ theo nghĩa tôpô, nhưng nó không tạo thành một đường Hamiltonian duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + M) | Mỗi nút được xử lý một lần trong thuật toán Kahn và mỗi cạnh được kiểm tra một số lần không đổi | 
| Không gian | O(N + M) | Danh sách kề, mảng một bậc và tập cạnh lưu trữ tất cả thông tin đồ thị | 

Các ràng buộc cho phép lên tới 100.000 nút và 500.000 cạnh, do đó việc truyền tải đồ thị theo thời gian tuyến tính nằm trong giới hạn một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return solve()

# simple chain
assert run("3\n1 2\n1 3\n1\n") in ["1\n", "1"]

# single chain 1->2->3
assert run("3\n1 2\n1 3\n0\n") in ["1\n", "1"]

# branching
assert run("3\n1 2 3\n0\n0\n") in ["0\n", "0"]

# disconnected components
assert run("4\n1 2\n0\n1 4\n0\n") in ["0\n", "0"]

# minimum size
assert run("2\n1 2\n0\n") in ["1\n", "1"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi 3 nút | 1 | đường Hamiltonian cơ bản | 
| phân nhánh từ gốc | 0 | phát hiện cấu trúc tuyến tính bị thiếu | 
| đồ thị bị ngắt kết nối | 0 | yêu cầu bảo hiểm đầy đủ | 
| tối thiểu N=2 chuỗi | 1 | trường hợp hợp lệ nhỏ nhất | 

## Vỏ cạnh 

Trường hợp cạnh khóa là một biểu đồ không có chu kỳ nhưng bị phân tách ngay lập tức. Ví dụ: nút 1 trỏ đến 2 và 3. Thuật toán xử lý nút 1 trước tiên theo sắp xếp tôpô, sau đó cả 2 và 3 đều khả dụng đồng thời. Thứ tự kết quả sẽ đặt chúng theo một số thứ tự tùy ý, chẳng hạn như [1,2,3], nhưng vì không có cạnh 2 → 3 nên việc kiểm tra liên tiếp không thành công, từ chối chính xác đầu vào. 

Một trường hợp cạnh khác là khi tồn tại nhiều bậc tôpô hợp lệ nhưng chỉ một trong số chúng có thể tương ứng với đường đi Hamilton. Ví dụ: ngay cả khi hàng đợi cho phép các lựa chọn khác nhau ở các bước khác nhau, thì bất kỳ thứ tự nào không hoàn toàn thuộc một chuỗi duy nhất sẽ nhất thiết đưa ra một cạnh bị thiếu giữa các nút liên tiếp và bước xác minh sẽ nắm bắt được điều này bất kể mối quan hệ được giải quyết như thế nào.
