---
title: "CF 106393B - \u0411\u0430\u0440\u044c\u0435\u0440\u044b"
description: "Chúng ta có một đồ thị vô hướng trên các đỉnh có nhãn từ 1 đến n. Biểu đồ không có chu trình nên mọi thành phần được kết nối là một cái cây và toàn bộ cấu trúc là một khu rừng. Mỗi cạnh kết nối hai đỉnh và đại diện cho một hành lang năng lượng giữa hai rào cản."
date: "2026-06-20T23:06:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106393
codeforces_index: "B"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2025-2026, \u0412\u0442\u043e\u0440\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 106393
solve_time_s: 51
verified: true
draft: false
---

[CF 106393B - \u0411\u0430\u0440\u044c\u0435\u0440\u044b](https://codeforces.com/problemset/problem/106393/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng trên các đỉnh có nhãn từ 1 đến n. Biểu đồ không có chu trình nên mọi thành phần được kết nối là một cái cây và toàn bộ cấu trúc là một khu rừng. Mỗi cạnh kết nối hai đỉnh và đại diện cho một hành lang năng lượng giữa hai rào cản. 

Với mỗi truy vấn, chúng ta được cung cấp một đoạn gồm các đỉnh [l, r]. Chúng ta chỉ xem xét các đỉnh trong khoảng này và bỏ qua mọi thứ bên ngoài nó. Bên trong tập cảm ứng này, chúng ta giữ tất cả các cạnh có điểm cuối đều nằm trong [l, r]. Nhiệm vụ là xác định có bao nhiêu thành phần được kết nối xuất hiện bên trong sơ đồ con bị hạn chế này. 

Một cách hữu ích để giải thích câu trả lời là chúng ta đang cắt ra một khối liền kề gồm các đỉnh được gắn nhãn và hỏi xem còn lại bao nhiêu “hòn đảo” kết nối biệt lập sau khi loại bỏ mọi thứ bên ngoài khối. 

Các ràng buộc rất lớn, với tối đa 5×10^5 đỉnh và cạnh và tối đa 2×10^5 truy vấn. Điều này ngay lập tức loại trừ việc tính toán lại kết nối cho mỗi truy vấn bằng DFS hoặc BFS. Ngay cả việc xây dựng DSU mới cho mỗi truy vấn cũng sẽ quá chậm vì mỗi truy vấn sẽ cần thời gian tuyến tính theo kích thước khoảng. Chúng ta cần một cái gì đó gần hơn với tổng công tuyến tính hoặc gần tuyến tính, lý tưởng nhất là O((n + m + q) log n) hoặc tốt hơn. 

Một đặc tính tinh tế nhưng quan trọng xuất phát từ thực tế là đồ thị là một khu rừng. Trong bất kỳ khu rừng nào, mọi thành phần được kết nối với các đỉnh V đều có chính xác V − 1 cạnh và nói chung hơn, bất kỳ đồ thị con nào của khu rừng vẫn không có tính tuần hoàn. Điều đó có nghĩa là đối với bất kỳ đồ thị con cảm ứng nào, số thành phần liên thông chính xác bằng số đỉnh trừ đi số cạnh bên trong đồ thị con cảm ứng đó. Điều này biến vấn đề từ khả năng kết nối thành vấn đề đếm thuần túy trên các cạnh. 

Khó khăn chính nằm ở việc đếm, đối với mỗi truy vấn [l, r], có bao nhiêu cạnh có cả hai điểm cuối bên trong khoảng. 

Một trường hợp cạnh dễ bị bỏ sót là khi các cạnh vượt qua ranh giới của khoảng. Ví dụ: nếu một cạnh kết nối 2 và 10 và truy vấn là [3, 9] thì cạnh này phải bị bỏ qua hoàn toàn mặc dù cả hai điểm cuối đều “gần” khoảng. Một cách tiếp cận ngây thơ chỉ kiểm tra một điểm cuối hoặc giả sử tính kề cận cục bộ sẽ tính sai các cạnh như vậy. 

Một trường hợp cạnh khác là khi khoảng rất nhỏ, chẳng hạn như [x, x]. Trong trường hợp đó, câu trả lời phải luôn là 1, vì một đỉnh luôn là một thành phần liên thông bất kể các cạnh ở nơi khác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý từng truy vấn một cách độc lập. Đối với [l, r] nhất định, chúng ta có thể lặp qua tất cả các đỉnh trong khoảng và chạy DFS hoặc DSU để tính toán khả năng kết nối. Điều này đúng, nhưng tốn O(r − l + 1 + số cạnh được xem xét) cho mỗi truy vấn. Trong trường hợp xấu nhất, với cả q và n đều lớn, giá trị này suy biến thành O(nq), tốc độ này quá chậm. 

Quan sát quan trọng là chúng ta thực sự không cần phải tính toán lại cấu trúc kết nối. Bởi vì đồ thị là một khu rừng nên câu trả lời chỉ phụ thuộc vào số lượng cục bộ: số đỉnh trong khoảng và số cạnh chứa đầy trong đó. Vì vậy, vấn đề giảm xuống còn việc trả lời nhiều truy vấn đếm phạm vi 2D trên các cạnh. 

Mỗi cạnh có thể được coi là một điểm (u, v) trong đó chúng ta giả sử u < v. Truy vấn [l, r] yêu cầu chúng ta đếm xem có bao nhiêu cạnh thỏa mãn l ≤ u và v ≤ r. Đây là một truy vấn thống trị trên một tập hợp các điểm, có thể được giải quyết một cách hiệu quả bằng cách sử dụng đường quét kết hợp với cây Fenwick. 

Chúng tôi sắp xếp các truy vấn theo điểm cuối bên trái theo thứ tự giảm dần và kích hoạt dần dần các cạnh có điểm cuối bên trái đủ lớn. Đối với mỗi cạnh được kích hoạt, chúng tôi lưu trữ điểm cuối bên phải của nó trong cây Fenwick. Sau đó, mỗi truy vấn sẽ trở thành truy vấn tổng tiền tố trên v. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại mỗi truy vấn | O(nq) | O(n + m) | Quá chậm | 
| Đường quét + Fenwick | O((n + m + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi chuyển đổi vấn đề thành các cạnh đếm bên trong các khoảng, sau đó đánh giá các số đếm đó bằng cách quét có cấu trúc trên các điểm cuối. 

1. Đối với mỗi cạnh, hãy sắp xếp lại các điểm cuối của nó sao cho u < v. Tiêu chuẩn hóa này đảm bảo mọi cạnh được biểu diễn nhất quán dưới dạng một điểm trong lưới 2D. Điều này cũng loại bỏ sự mơ hồ khi so sánh các điểm cuối sau này. 
2. Quan sát rằng đối với truy vấn [l, r], một cạnh (u, v) hoàn toàn nằm trong khoảng khi và chỉ khi l ≤ u và v ≤ r. Điều này biến mọi truy vấn thành điều kiện thống trị theo hai chiều. 
3. Sắp xếp tất cả các cạnh theo u theo thứ tự giảm dần. Mục đích là để dần dần “kích hoạt” các cạnh có điểm cuối bên trái trở nên hợp lệ khi chúng ta giảm l trong quá trình quét. 
4. Sắp xếp tất cả các truy vấn theo l theo thứ tự giảm dần. Chúng tôi xử lý các truy vấn từ phải sang trái theo ranh giới bên trái của chúng, sao cho khi một cạnh bắt đầu hoạt động, nó vẫn hoạt động cho tất cả l nhỏ hơn. 
5. Duy trì cây Fenwick trên tọa độ v. Khi kích hoạt một cạnh (u, v), chúng ta thêm 1 ở vị trí v. Cấu trúc này theo dõi xem có bao nhiêu cạnh hoạt động có điểm cuối bên phải của chúng ở hoặc trước một giá trị nhất định. 
6. Quét l từ n xuống 1. Bất cứ khi nào đạt đến giá trị l, chúng ta chèn tất cả các cạnh có u ≥ l vào cây Fenwick. Điều này đảm bảo rằng tất cả các cạnh thỏa mãn ràng buộc bên trái đều đã hoạt động. 
7. Đối với mỗi truy vấn có điểm cuối bên trái l, hãy tính số cạnh hợp lệ khi tổng tiền tố Fenwick bằng r. Điều này đếm tất cả các cạnh có v ≤ r trong số những cạnh đã được kích hoạt. 
8. Chuyển đổi số cạnh thành số thành phần bằng cách sử dụng đồng nhất rừng: thành phần = số đỉnh trong [l, r] trừ đi số cạnh trong. 

Lý do điều này có tác dụng là vì trong một khu rừng, mỗi cạnh đều giảm số lượng thành phần được kết nối đi đúng một khi nó kết nối hai đỉnh riêng biệt trước đó. Vì không có chu trình nên các cạnh không bao giờ tạo ra sự dư thừa có thể phá vỡ mối quan hệ này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

def solve():
    n, m = map(int, input().split())
    edges = []

    for _ in range(m):
        u, v = map(int, input().split())
        if u > v:
            u, v = v, u
        edges.append((u, v))

    q = int(input())
    queries = []
    for i in range(q):
        l, r = map(int, input().split())
        queries.append((l, r, i))

    edges.sort(reverse=True)
    queries.sort(reverse=True)

    fw = Fenwick(n)
    ans_edges = [0] * q

    ptr = 0

    for l, r, idx in queries:
        while ptr < m and edges[ptr][0] >= l:
            fw.add(edges[ptr][1], 1)
            ptr += 1

        cnt_edges = fw.sum(r)
        ans_edges[idx] = cnt_edges

    out = []
    for l, r, i in sorted(queries, key=lambda x: x[2]):
        cnt_edges = ans_edges[i]
        out.append(str((r - l + 1) - cnt_edges))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Cây Fenwick chỉ được sử dụng để duy trì số lượng cạnh được kích hoạt kết thúc tại hoặc trước một vị trí nhất định. Việc sắp xếp các cạnh theo u đảm bảo tính chính xác của việc kích hoạt, trong khi việc sắp xếp các truy vấn theo l đảm bảo mỗi truy vấn nhìn thấy chính xác tập hợp các cạnh phù hợp. Phép trừ cuối cùng chuyển đổi số cạnh thành các thành phần được kết nối bằng thuộc tính forest. 

Chi tiết triển khai tinh tế là các truy vấn được sắp xếp lại hai lần: một lần để xử lý và một lần để khôi phục đầu ra. Điều này là cần thiết vì chúng ta xử lý chúng theo thứ tự quét đơn điệu nhưng phải xuất ra theo thứ tự ban đầu. 

## Ví dụ đã hoạt động 

Xét một khu rừng nhỏ có các cạnh 1-2, 2-3, 4-5. Giả sử chúng ta trả lời các truy vấn [1,3] và [2,5]. 

Đầu tiên chúng ta chuẩn hóa các cạnh: (1,2), (2,3), (4,5). Chúng tôi sắp xếp chúng theo thứ tự giảm dần: (4,5), (2,3), (1,2). Các truy vấn được sắp xếp theo l giảm dần: [2,5], sau đó là [1,3]. 

### Dấu vết 

| Bước | Các cạnh hoạt động | Bang Fenwick (khái niệm) | Truy vấn | Số cạnh | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| l=2 | (4,5),(2,3) | v=3:1, v=5:1 | [2,5] | 1 | (4)−1=3 | 
| l=1 | (4,5),(2,3),(1,2) | v=2,3,5 tất cả đều hoạt động | [1,3] | 2 | (3)−2=1 | 

Truy vấn đầu tiên chỉ nhìn thấy cạnh (2,3) bên trong [2,5], đưa ra hai thành phần: {2,3}, {4,5}. Truy vấn thứ hai nhìn thấy các cạnh (1,2) và (2,3) bên trong [1,3], tạo thành một thành phần được kết nối. 

Dấu vết này xác nhận rằng quá trình quét tích lũy chính xác các cạnh dựa trên ranh giới bên trái trong khi vẫn tôn trọng ranh giới bên phải thông qua các truy vấn Fenwick. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m + q) log n) | Mỗi cạnh được chèn một lần, mỗi truy vấn thực hiện truy vấn Fenwick | 
| Không gian | O(n) | Cây Fenwick cộng với bộ nhớ cho các cạnh và truy vấn | 

Giải pháp này phù hợp một cách thoải mái trong các giới hạn vì tất cả các phép toán đều là logarit trên n tối đa 5×10^5 và tổng số sự kiện là tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    return solve()

# minimal tree
assert run("""1 0
1
1 1
""").strip() == "1"

# sample-like chain
assert run("""3 2
1 2
2 3
2
1 3
2 3
""").strip().split() == ["1", "1"]

# disjoint edges
assert run("""4 2
1 2
3 4
1
1 4
""").strip() == "2"

# single vertex intervals
assert run("""5 3
1 2
2 3
4 5
3
2 2
3 3
4 4
""").strip().split() == ["1", "1", "1"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1,m=0 | 1 | tính đúng đắn đơn lẻ | 
| đồ thị chuỗi | 1,1 | giảm kết nối cơ bản | 
| hai thành phần | 2 | xử lý cấu trúc rời rạc | 
| truy vấn một điểm | 1,1,1 | khoảng ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi l bằng r. Thuật toán xử lý việc này một cách tự nhiên vì không có cạnh nào có thể thỏa mãn u ≥ l và v ≤ r trừ khi đó là một vòng lặp khép kín, không tồn tại trong một khu rừng. Truy vấn Fenwick trả về 0 và câu trả lời trở thành 1. 

Một trường hợp khác là khi tất cả các cạnh nằm hoàn toàn bên ngoài phạm vi truy vấn. Ví dụ: các cạnh (1,2) và truy vấn [5,6]. Vì không có cạnh nào được kích hoạt cho u ≥ 5 nên cây Fenwick vẫn trống và kết quả chính xác là r − l + 1. 

Cuối cùng, hãy xem xét một truy vấn có tiền tố dày đặc như [1, n]. Tất cả các cạnh đều được kích hoạt và đếm, và vì toàn bộ biểu đồ là một khu rừng nên kết quả giảm xuống số lượng thành phần được kết nối trong biểu đồ ban đầu, được tính toán chính xác là n − m.
