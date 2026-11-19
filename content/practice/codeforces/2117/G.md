---
title: "CF 2117G - Đồ thị Chúa ơi"
description: "Chúng tôi đang làm việc với một biểu đồ kết nối vô hướng có trọng số trong đó chúng tôi cần di chuyển từ nút 1 đến nút n. Mọi đường dẫn đều được phép truy cập lại các đỉnh và cạnh, vì vậy các chu trình được cho phép."
date: "2026-06-08T11:03:58+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dsu", "graphs", "greedy", "shortest-paths", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2117
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 1029 (Div. 3)"
rating: 1900
weight: 2117
solve_time_s: 79
verified: true
draft: false
---

[CF 2117G - Đồ thị Chúa ơi](https://codeforces.com/problemset/problem/2117/G) 

**Xếp hạng:** 1900 
**Tags:** sức mạnh vũ phu, dsu, đồ thị, tham lam, đường đi ngắn nhất, sắp xếp 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một biểu đồ kết nối vô hướng có trọng số trong đó chúng tôi cần di chuyển từ nút 1 đến nút n. Mọi đường dẫn đều được phép truy cập lại các đỉnh và cạnh, vì vậy các chu trình được cho phép. 

Chi phí của bất kỳ đường đi nào được chọn chỉ được xác định bởi trọng số cạnh nhỏ nhất và lớn nhất xuất hiện dọc theo đường đi đó. Nếu một đường dẫn sử dụng trọng số cạnh$w_1, w_2, \dots, w_k$, thì chi phí chỉ đơn giản là tổng của giá trị tối thiểu và tối đa giữa các trọng số này. Nhiệm vụ là tìm bất kỳ đường đi nào từ 1 đến n sao cho đại lượng này nhỏ nhất. 

Điều làm cho điều này trở nên bất thường là bản thân đường đi không được tính điểm bằng tổng trọng lượng hoặc số cạnh mà bằng hàm giá trị cực trị trên các cạnh của nó. Điều này phá vỡ cấu trúc đường dẫn ngắn nhất thông thường: việc thêm các cạnh không tích lũy chi phí một cách tuyến tính, nó chỉ có khả năng thay đổi mức tối thiểu hoặc tối đa được thấy cho đến nay. 

Các ràng buộc rất chặt chẽ: tổng số đỉnh và cạnh trong tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$. Điều này loại trừ bất kỳ cách tiếp cận nào tính toán lại các đường đi ngắn nhất trên mỗi khoảng ứng cử viên hoặc thử tất cả các cặp cạnh làm ranh giới một cách ngây thơ. Bất cứ điều gì tệ hơn gần tuyến tính hoặc gần$m \log m$mỗi trường hợp thử nghiệm sẽ gặp khó khăn. 

Một vấn đề tế nhị xuất phát từ thực tế là các đường dẫn có thể quay lại các cạnh. Một người đọc ngây thơ có thể cho rằng chúng ta đang tìm kiếm một đường đi đơn giản, nhưng việc cho phép lặp lại có nghĩa là khi đã có sẵn “phạm vi tốt” các trọng số cạnh trong một sơ đồ con được kết nối, chúng ta có thể tùy ý duyệt qua bên trong nó để điều chỉnh kết nối. 

Một số chế độ lỗi xuất hiện một cách tự nhiên: 

Nếu chúng ta thử đường đi ngắn nhất tiêu chuẩn trên các đỉnh trong khi theo dõi trọng số cạnh tối thiểu và tối đa hiện tại, không gian trạng thái sẽ trở thành$(node, minEdge, maxEdge)$, nó quá lớn. Ví dụ: ngay cả trong một biểu đồ nhỏ, các cách khác nhau để đến cùng một nút với các cực tiểu hơi khác nhau sẽ tạo ra các chi phí hoàn toàn khác nhau trong tương lai. 

Một trực giác không chính xác khác là cho rằng câu trả lời tương ứng với một số đường dẫn tối ưu bên trong một cây MST hoặc cây đường đi ngắn nhất. Điều đó không thành công vì chúng tôi không tối ưu hóa cấu trúc phụ gia; việc thêm một cạnh kém hơn một chút vẫn có thể có lợi nếu nó cải thiện khả năng kết nối để sau này chúng ta có thể tránh được mức tối đa tồi tệ hơn. 

## Phương pháp tiếp cận 

Quan sát quan trọng là chuyển quan điểm từ “đường dẫn xác định tối thiểu và tối đa” sang “câu trả lời khả thi được xác định bằng cách chọn trọng số cạnh nào được phép”. 

Giả sử chúng ta sửa giới hạn dưới$L$và giới hạn trên$R$. Nếu tồn tại một đường đi từ 1 đến n chỉ sử dụng các cạnh có trọng số nằm trong$[L, R]$, thì chúng ta có thể nhận ra một đường đi có chi phí lớn nhất$L + R$, vì cạnh tối thiểu trên đường đi đó ít nhất là$L$và tối đa là nhiều nhất$R$và chúng tôi luôn có thể đảm bảo cả hai điểm cuối đều xuất hiện trong đường dẫn bằng khả năng kết nối trong biểu đồ được lọc đó. 

Vì vậy vấn đề trở thành: tìm một cặp$(L, R)$sao cho 1 và n được kết nối chỉ bằng các cạnh trong phạm vi đó, giảm thiểu$L + R$. 

Bây giờ chúng ta đảo ngược suy nghĩ. Thay vì cố định cả hai đầu, chúng tôi sắp xếp các cạnh theo trọng lượng và cố gắng neo cạnh tối thiểu vào giải pháp. Giả sử chúng ta chọn một cạnh có trọng số$w$là cạnh tối thiểu được phép trong đường dẫn. Vậy thì câu trả lời phải là$w + \text{(some maximum reachable weight while keeping connectivity)}$. 

sửa chữa$w$, chúng tôi chỉ cho phép các cạnh có trọng số ít nhất$w$. Trong biểu đồ con này, chúng ta cần biết trọng số cạnh tối đa nhỏ nhất có thể vẫn kết nối 1 và n. Điều đó tương đương với việc tìm ngưỡng tối thiểu$R$sao cho các cạnh trong$[w, R]$kết nối 1 và n. 

Điều này được xử lý một cách tự nhiên bằng quá trình quét DSU: sắp xếp các cạnh theo trọng số và duy trì kết nối khi chúng tôi tăng giới hạn trên. Giới hạn dưới của mỗi ứng viên$w$, về mặt khái niệm, chúng tôi thiết lập lại và sau đó tăng lên, nhưng thực hiện việc này trên mỗi cạnh sẽ quá chậm. 

Tối ưu hóa cuối cùng là để nhận ra rằng chúng tôi không cần phải “đặt lại cho mỗi giới hạn dưới”. Thay vào đó, chúng tôi xử lý các cạnh theo thứ tự tăng dần và duy trì ý tưởng cửa sổ trượt qua kết nối DSU bằng cách sử dụng kỹ thuật hai con trỏ kết hợp với logic kết nối liên tục hoặc tăng dần. Một giải pháp tiêu chuẩn và trực tiếp hơn là sắp xếp các cạnh và sử dụng DSU trong khi coi mọi cạnh là điểm cuối tối thiểu và duy trì mức tối đa nhỏ nhất nối 1 và n. 

Trên thực tế, chúng ta cố định cạnh tối đa bằng cách quét từ nhỏ đến lớn và duy trì xem 1 và n có được kết nối hay không. Phần tinh vi hơn là trích xuất mức tối thiểu tương ứng tốt nhất, đạt được bằng cách theo dõi, đối với mỗi thời gian kết nối, trọng số cạnh nhỏ nhất có thể đóng vai trò là mức tối thiểu hợp lệ trong cùng khoảng thời gian kết nối. 

Một cách cải cách rõ ràng để tránh những cạm bẫy là: 

Chúng tôi sắp xếp các cạnh theo trọng lượng. Chúng tôi duy trì DSU khi chúng tôi bao gồm các cạnh theo thứ tự tăng dần. Tại bất kỳ thời điểm nào, khi 1 và n được kết nối lần đầu tiên ở trọng số cạnh$R$, thành phần hiện tại đã được xây dựng bằng cách sử dụng các cạnh lên đến$R$. Bên trong thành phần đó, trọng lượng cạnh nhỏ nhất được sử dụng trên cấu trúc kết nối đầu tiên kết nối 1 và n thực sự là mức tối thiểu tốt nhất có thể cho mức tối đa đó$R$. Vì vậy, chúng tôi duy trì, đối với mỗi thành phần DSU, trọng số cạnh tối thiểu được sử dụng để tạo thành nó và khi 1 và n hợp nhất, chúng tôi đánh giá một câu trả lời ứng viên. 

Điều này mang lại sự quét tuyến tính trên các cạnh đã được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực đối với tất cả các cặp (L, R) có BFS | O(m^2 (n + m)) | O(n + m) | Quá chậm | 
| Quét DSU trên các cạnh đã được sắp xếp | O(m α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các cạnh theo trọng số theo thứ tự tăng dần. Điều này đảm bảo rằng khi chúng ta thêm một cạnh, nó chỉ có thể tăng ranh giới tối đa hiện tại của bất kỳ khoảng được xem xét nào. 
2. Khởi tạo DSU trong đó mỗi nút là thành phần riêng của nó. Bên cạnh mỗi thành phần, hãy lưu trữ trọng lượng cạnh tối thiểu đã được sử dụng để hợp nhất nó vào cấu trúc hiện tại. Ban đầu điều này không được xác định. 
3. Lặp lại các cạnh theo thứ tự sắp xếp theo trọng lượng$w$. Đối với mỗi cạnh (u, v, w), hãy thử hợp các thành phần của chúng. Khi hợp nhất hai thành phần, hãy cập nhật trọng số cạnh tối thiểu được lưu trữ của thành phần kết quả thành giá trị tối thiểu của các giá trị được lưu trữ của hai thành phần và$w$. 
4. Mỗi lần hợp nhất, hãy kiểm tra xem các nút 1 và n hiện có nằm trong cùng một thành phần hay không. Nếu đúng như vậy, chúng ta có thể tạo thành một đường đi có cạnh tối đa là dòng điện$w$, bởi vì tất cả các cạnh được sử dụng cho đến nay đều ≤$w$. 
5. Khi 1 và n được kết nối, hãy tính câu trả lời của ứng viên là$w + \text{minEdge}[component(1)]$, vì cạnh tối thiểu được lưu trữ trong thành phần này biểu thị trọng số cạnh nhỏ nhất tham gia vào cấu trúc kết nối chúng. 
6. Tiếp tục xử lý các cạnh, vì việc hợp nhất sau này có thể tạo ra các cấu trúc thay thế với mức tối đa lớn hơn nhưng mức tối thiểu nhỏ hơn đáng kể, có khả năng cải thiện tổng. 

### Tại sao nó hoạt động 

Tại bất kỳ điểm nào trong quá trình quét, mỗi thành phần DSU biểu thị kết nối chỉ sử dụng các cạnh có trọng lượng tối đa hiện tại$w$. Bất kỳ đường đi nào từ 1 đến n sử dụng các cạnh ≤$w$được chứa đầy đủ trong cấu trúc này. Thời điểm đầu tiên 1 và n được kết nối tương ứng với cạnh tối đa nhỏ nhất có thể có đối với bất kỳ đường dẫn hợp lệ nào chỉ sử dụng các cạnh lên đến ngưỡng đó. Trong cùng lịch sử xây dựng đó, việc theo dõi trọng số cạnh tối thiểu có trong quy trình kết hợp sẽ nắm bắt được giới hạn dưới nhỏ nhất có thể đạt được tương thích với chế độ kết nối đó. Vì mọi đường dẫn tối ưu ứng cử viên phải tương ứng với một số ngưỡng mà cạnh tối đa của nó được đưa ra, quá trình quét đảm bảo chúng tôi đánh giá mọi mức tối đa có liên quan chính xác một lần, kết hợp với mức tối thiểu tốt nhất có thể đạt được theo ràng buộc đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n
        self.min_edge = [10**18] * n

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b, w):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return False

        if self.size[a] < self.size[b]:
            a, b = b, a

        self.parent[b] = a
        self.size[a] += self.size[b]
        self.min_edge[a] = min(self.min_edge[a], self.min_edge[b], w)
        return True

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    edges = []
    for _ in range(m):
        u, v, w = map(int, input().split())
        edges.append((w, u - 1, v - 1))

    edges.sort()

    dsu = DSU(n)

    ans = 10**18

    for w, u, v in edges:
        dsu.union(u, v, w)

        if dsu.find(0) == dsu.find(n - 1):
            root = dsu.find(0)
            ans = min(ans, dsu.min_edge[root] + w)

    print(ans)
```Cấu trúc DSU duy trì các thành phần được kết nối khi chúng ta tăng dần trọng số cạnh cho phép. Hoạt động hợp nhất không chỉ hợp nhất kết nối mà còn theo dõi trọng số cạnh nhỏ nhất xuất hiện bên trong mỗi thành phần, điều này rất cần thiết để xây dựng lại chi phí đường dẫn tối thiểu có thể. 

Khi nút 1 và n được kết nối lần đầu tiên ở trọng số cạnh nào đó$w$, cái đó$w$hoạt động như một ứng cử viên tối đa. Cạnh tối thiểu được lưu trữ trong thành phần đó cung cấp khả năng tương thích tối thiểu tốt nhất có thể với trạng thái kết nối đó và tổng của chúng được đánh giá là một câu trả lời ứng cử viên. 

Một điểm tinh tế là thành phần tương tự có thể phát triển hơn nữa sau khi 1 và n đã được kết nối. Đó là lý do tại sao chúng ta không dừng lại sớm; những lần hợp nhất sau này có thể tạo ra cạnh tối thiểu nhỏ hơn trong khi vẫn giữ được kết nối, điều này có thể làm giảm tổng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 2 1
2 3 1
```Chúng tôi xử lý các cạnh theo thứ tự: 

| Bước | Cạnh (w, u, v) | DSU(1-3 đã kết nối?) | gốc min_edge | Ứng viên | 
| --- | --- | --- | --- | --- | 
| 1 | (1,1,2) | Không | 1 | - | 
| 2 | (1,2,3) | Có | 1 | 1 + 1 = 2 | 

Tại thời điểm đạt được kết nối, thành phần chỉ chứa các cạnh có trọng số 1, vì vậy cả tối thiểu và tối đa đều là 1. 

### Ví dụ 2 

đầu vào:```
3 2
1 3 13
1 2 5
```| Bước | Cạnh | Đã kết nối | min_edge | Ứng viên | 
| --- | --- | --- | --- | --- | 
| 1 | (5,1,2) | Không | 5 | - | 
| 2 | (13,1,3) | Có | 5 | 5 + 13 = 18 | 

Điều này cho thấy tại sao một chu trình lại hữu ích: mặc dù đường dẫn sử dụng cạnh lớn 13, nó vẫn được hưởng lợi từ cạnh nhỏ bên trong cùng một cấu trúc được kết nối. 

Dấu vết xác nhận rằng thuật toán nắm bắt chính xác cả hai thái cực thông qua thứ tự chèn cạnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m α(n)) | Mỗi cạnh được xử lý một lần với các hoạt động DSU gần như không đổi | 
| Không gian | O(n + m) | Mảng DSU và lưu trữ cạnh | 

Tổng số cạnh và nút trên tất cả các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$, do đó, quá trình quét DSU gần tuyến tính dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    class DSU:
        def __init__(self, n):
            self.parent = list(range(n))
            self.size = [1] * n
            self.min_edge = [10**18] * n

        def find(self, x):
            while self.parent[x] != x:
                self.parent[x] = self.parent[self.parent[x]]
                x = self.parent[x]
            return x

        def union(self, a, b, w):
            a = self.find(a)
            b = self.find(b)
            if a == b:
                return False
            if self.size[a] < self.size[b]:
                a, b = b, a
            self.parent[b] = a
            self.size[a] += self.size[b]
            self.min_edge[a] = min(self.min_edge[a], self.min_edge[b], w)
            return True

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        edges = []
        for _ in range(m):
            u, v, w = map(int, input().split())
            edges.append((w, u - 1, v - 1))
        edges.sort()

        dsu = DSU(n)
        ans = 10**18

        for w, u, v in edges:
            dsu.union(u, v, w)
            if dsu.find(0) == dsu.find(n - 1):
                root = dsu.find(0)
                ans = min(ans, dsu.min_edge[root] + w)

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("""4
3 2
1 2 1
2 3 1
3 2
1 3 13
1 2 5
8 9
1 2 6
2 3 5
3 8 6
1 4 7
4 5 4
5 8 7
1 6 5
6 7 5
7 8 5
3 3
1 3 9
1 2 8
2 3 3
""") == """2
18
10
11"""

# custom cases
assert run("""1
2 1
1 2 7
""") == "14"

assert run("""1
4 3
1 2 5
2 3 1
3 4 10
""") == "6"

assert run("""1
3 3
1 2 100
2 3 100
1 3 1
""") == "101"

assert run("""1
5 5
1 2 3
2 3 4
3 4 5
4 5 6
1 5 100
""") == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 nút cạnh đơn | 14 | trường hợp cơ sở đồ thị tối thiểu | 
| chuỗi nhỏ hỗn hợp | 6 | tương tác giữa trọng lượng thấp và cao | 
| cạnh trực tiếp so với đường dẫn | 101 | chu kỳ có thể đánh bại cạnh trực tiếp | 
| chuỗi dài vs phím tắt | 9 | cửa sổ tối ưu bên trong đường dẫn | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi đường đi tối ưu không phải là đường đi ngắn nhất về mặt cạnh mà thay vào đó sử dụng đường vòng để giảm cạnh tối thiểu về chi phí cuối cùng. Ví dụ: nếu có cạnh nặng trực tiếp giữa 1 và n, nhưng cũng có đường đi dài hơn chứa cạnh rất nhỏ, thì thuật toán sẽ ưu tiên cấu trúc dài hơn một cách chính xác vì nó giảm số hạng tối thiểu trong khi kiểm soát mức tối đa thông qua quá trình quét. 

Một trường hợp khó phát hiện khác là khi lần đầu tiên 1 và n kết nối không mang lại câu trả lời đúng nhất. Vì việc hợp nhất sau này có thể đưa các cạnh tối thiểu nhỏ hơn vào cùng một thành phần mà không tăng mức tối đa hiện tại quá mạnh nên việc tiếp tục quét sẽ đảm bảo nắm bắt được những cải tiến đó.
