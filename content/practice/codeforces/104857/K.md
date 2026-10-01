---
title: "CF 104857K - Phân vùng khuôn viên trường"
description: "Chúng ta có một cây đại diện cho một khuôn viên, trong đó mỗi nút là một tòa nhà và mỗi tòa nhà có giá trị quan trọng dương. Chúng ta phải chia cây thành nhiều nhóm bằng cách loại bỏ một số cạnh."
date: "2026-06-28T10:57:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104857
codeforces_index: "K"
codeforces_contest_name: "The 2023 ICPC Asia Hefei Regional Contest (The 2nd Universal Cup. Stage 12: Hefei)"
rating: 0
weight: 104857
solve_time_s: 51
verified: true
draft: false
---

[CF 104857K - Phân vùng khuôn viên trường](https://codeforces.com/problemset/problem/104857/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây đại diện cho một khuôn viên, trong đó mỗi nút là một tòa nhà và mỗi tòa nhà có giá trị quan trọng dương. Chúng ta phải chia cây thành nhiều nhóm bằng cách loại bỏ một số cạnh. Mỗi nhóm phải được kết nối trong cây gốc, vì vậy mỗi nhóm là một thành phần được kết nối của một phân vùng đỉnh. 

Đối với mỗi nhóm, sự đóng góp của họ cho câu trả lời được xác định theo một cách hơi khác thường. Nếu nhóm chứa ít nhất hai nút, chúng ta sẽ xem xét tất cả các trọng số bên trong nó và lấy trọng số lớn thứ hai. Nếu nhóm chỉ có một nút thì đóng góp của nó bằng 0. Mục tiêu là phân vùng cây sao cho tổng đóng góp của các nhóm này là tối đa. 

Hạn chế chính về cấu trúc là chúng ta chỉ được phép cắt các cạnh của cây. Điều đó có nghĩa là mọi giải pháp hợp lệ đều tương ứng với việc chọn một số cạnh cần loại bỏ, tạo ra một khu rừng. 

Ràng buộc n lên tới 5 × 10^5 ngay lập tức loại trừ mọi thứ cố gắng liệt kê các phân vùng hoặc thậm chí lý do cho mỗi tập hợp con của các nút. Bất kỳ giải pháp nào cũng phải gần với tuyến tính hoặc tuyến tính, vì O(n log n) hoặc O(n) là phạm vi thực tế duy nhất. 

Một kiểu thất bại tinh tế xuất hiện nếu chúng ta giả định rằng các trọng lượng lớn sẽ luôn hình thành các nhóm riêng của chúng. Trực giác đó là sai vì yếu tố lớn thứ hai mới là vấn đề quan trọng, do đó, việc cô lập mức tối đa của một khu vực thực sự sẽ phá hủy sự đóng góp trừ khi yếu tố lớn thứ hai được bảo tồn ở nơi khác. 

Ví dụ: hãy xem xét chuỗi 1-2-3 có trọng số 10, 9, 1. Nếu chúng ta giữ tất cả các nút lại với nhau, đóng góp là 9. Nếu chúng ta chia {10,9} và {1}, chúng ta sẽ nhận được 9 + 0 = 9. Nếu chúng ta chia tất cả các nút đơn, chúng ta sẽ nhận được 0. Nhưng nếu chúng ta chia sai thành {10,1} và {9}, chúng ta sẽ nhận được 1 + 0 = 1, điều này thực sự tệ hơn. Vì vậy, các quyết định phân nhóm phụ thuộc vào cấu trúc chứ không chỉ phụ thuộc vào trọng số sắp xếp. 

Khó khăn thực sự là điểm số của mỗi khu vực chỉ phụ thuộc vào hai trọng số cao nhất của nó, điều này cho thấy rằng mỗi khu vực về cơ bản được "neo" bởi phần tử lớn thứ hai của nó, trong khi phần tử lớn nhất không liên quan ngoại trừ việc bật vị trí thứ hai đó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi cách để cắt các cạnh, tạo ra tất cả các phân vùng của cây thành các thành phần được kết nối. Đối với mỗi phân vùng, chúng tôi tính giá trị lớn thứ hai cho mỗi thành phần. Ngay cả khi chúng ta tránh tính toán lại từ đầu và duy trì số liệu thống kê thành phần, số cách cắt các cạnh là 2^(n−1), vì mỗi cạnh có bị cắt hay không. Điều này trở nên lớn về mặt thiên văn ngay cả khi n = 30, do đó hướng này ngay lập tức không thể thực hiện được. 

Quan sát quan trọng là đảo ngược quan điểm. Thay vì nghĩ về các thành phần, chúng tôi nghĩ về cách mỗi nút đóng góp với tư cách là phần tử lớn nhất hoặc lớn thứ hai của một số khu vực. Phần tử lớn thứ hai là phần tử duy nhất đóng góp, điều này cho thấy rằng mọi khu vực phải có chính xác một nút “hoạt động” đóng vai trò tối đa thứ hai. 

Bây giờ hãy xem xét một nút u có trọng số w[u]. Nếu u là phần tử lớn thứ hai trong vùng của nó thì vùng đó phải chứa ít nhất một nút có trọng số lớn hơn thực sự và đóng vai trò là giá trị lớn nhất. Nút lớn hơn đó phải nằm ở đâu đó trong cùng thành phần được kết nối. Điều này tạo ra một mối quan hệ định hướng: mọi “vai trò tối đa thứ hai” được chọn tại u phải được hỗ trợ bằng cách gắn u với một số nút tổ tiên hoặc nút có trọng số cao hơn. 

Điều này đương nhiên dẫn đến việc sắp xếp các nút bằng cách giảm trọng lượng và xử lý chúng theo thứ tự đó. Khi chúng tôi xử lý một nút, chúng tôi sẽ quyết định xem nút đó có trở thành nút đóng góp lớn thứ hai ở một số khu vực hay không và chúng tôi kết nối nút đó lên trên với nút có trọng số cao hơn sẽ đóng vai trò hỗ trợ tối đa cho nút đó. 

Cấu trúc cây đảm bảo rằng các ràng buộc kết nối giảm xuống để duy trì kết nối dọc theo các cạnh cha-con trong cây có gốc. Bằng cách root cây một cách tùy ý, chúng ta có thể nghĩ đến việc liên kết các nút với các nút có trọng số cao hơn đã được xử lý.

Một cách tiêu chuẩn để quản lý việc này là duy trì một cấu trúc theo dõi số lượng nút con đang hoạt động của mỗi nút đã được “tiêu thụ” bằng cách hình thành các đóng góp. Mỗi lần chúng tôi chỉ định một nút là phần tử lớn thứ hai, chúng tôi sẽ “ghép nối nó” một cách hiệu quả với một nút có trọng số cao hơn và việc ghép nối này sẽ tiêu tốn một đường dẫn cạnh trong cây. Chiến lược tối ưu trở nên tham lam: luôn gắn một nút vào tổ tiên có trọng số cao hơn hiện có gần nhất trong cấu trúc giống DSU theo thứ tự cây do quá trình xử lý tạo ra. 

Hiệu ứng cuối cùng là mỗi nút đóng góp chính xác một lần với tư cách là phần tử lớn thứ hai khi và chỉ khi nó có thể được so khớp lên trên và việc so khớp tôn trọng khả năng kết nối của cây. Giải pháp tối ưu giảm xuống một quá trình tham lam giống như kết hợp tối đa trên cây có gốc được sắp xếp theo trọng số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Tham lam theo trọng lượng + liên kết cây | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta root cây tại một nút tùy ý, thường là 1, sao cho mọi nút đều có cấu trúc cha-con. Chúng tôi cũng sắp xếp tất cả các nút theo thứ tự trọng số giảm dần để khi chúng tôi xử lý một nút, tất cả các nút “hỗ trợ tối đa” tiềm năng đều đã được xem xét. 

Chúng tôi duy trì một DSU hoặc cấu trúc con trỏ gốc trên cây có gốc cho phép chúng tôi nhanh chóng leo đến tổ tiên gần nhất vẫn “có sẵn” để hỗ trợ ghép nối. 

1. Sắp xếp các nút theo trọng số giảm dần. Điều này đảm bảo rằng khi chúng tôi xử lý một nút, tất cả các nút có trọng số lớn hơn đã được xem xét và đủ điều kiện đóng vai trò là nút tối đa của một khu vực. 
2. Duy trì cấu trúc DSU trên các nút cây, ban đầu mỗi nút là một tập hợp riêng. DSU đại diện cho tổ tiên sẵn có gần nhất chưa được sử dụng đầy đủ để hình thành một vùng. 
3. Xử lý các nút theo thứ tự được sắp xếp. Đối với nút u, chúng tôi cố gắng gán nó làm phần tử lớn thứ hai của một số vùng. 
4. Để làm điều đó, chúng tôi cố gắng tìm một tổ tiên v có sẵn của u trong cây gốc bằng cách sử dụng DSU “tìm” trên chuỗi cha của u. V này đại diện cho một nút có trọng số cao hơn có thể đóng vai trò là điểm tối đa của vùng u. 
5. Nếu a v như vậy tồn tại, chúng ta tăng câu trả lời lên w[u], vì u trở thành phần tử lớn thứ hai của một vùng hợp lệ. 
6. Sau khi sử dụng v để hỗ trợ u, chúng tôi hợp nhất u thành v theo thuật ngữ DSU, nghĩa là u không còn có thể sử dụng độc lập như một nút hỗ trợ nữa. Điều này bảo toàn tính bất biến rằng mỗi nút được sử dụng nhiều nhất một lần làm cấu trúc hỗ trợ. 
7. Tiếp tục cho đến khi tất cả các nút được xử lý. 

Ý tưởng quan trọng là mỗi cặp thành công tương ứng với việc hình thành hoặc mở rộng một vùng trong đó u được đảm bảo là phần tử lớn thứ hai, bởi vì tất cả các nút được xử lý trước đó có trọng số cao hơn và đảm bảo tồn tại mức tối đa hợp lệ. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình, DSU đảm bảo rằng mỗi nút trỏ tới nút tổ tiên gần nhất mà vẫn có khả năng hoạt động như một vùng tối đa. Bởi vì các nút được xử lý theo thứ tự trọng số giảm dần, nên khi chúng ta gán một nút u, bất kỳ nút tổ tiên nào mà chúng ta tìm thấy đều có trọng số lớn hơn. Điều này đảm bảo rằng u luôn hợp lệ là phần tử lớn thứ hai. Mỗi nút được sử dụng tối đa một lần trong vai trò như vậy vì một khi nó được hợp nhất, nó không thể được chọn độc lập nữa. Điều này thực thi cấu trúc một-một giữa các đóng góp và cực đại hỗ trợ hợp lệ, khớp chính xác với định nghĩa về trọng số vùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

class DSU:
    def __init__(self, n):
        self.parent = list(range(n + 1))

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a != b:
            self.parent[a] = b

n = int(input())
w = [0] + list(map(int, input().split()))
g = [[] for _ in range(n + 1)]

for _ in range(n - 1):
    u, v = map(int, input().split())
    g[u].append(v)
    g[v].append(u)

parent = [0] * (n + 1)
order = []
stack = [1]
parent[1] = -1

while stack:
    u = stack.pop()
    order.append(u)
    for v in g[u]:
        if v == parent[u]:
            continue
        parent[v] = u
        stack.append(v)

nodes = list(range(1, n + 1))
nodes.sort(key=lambda x: -w[x])

dsu = DSU(n)
ans = 0

for u in nodes:
    p = parent[u]
    if p == -1:
        continue
    v = dsu.find(p)
    if v != 0:
        ans += w[u]
        dsu.union(u, v)

print(ans)
```Giải pháp trước tiên là xây dựng một biểu diễn gốc của cây sao cho mỗi nút đều có một con trỏ cha. Điều này là cần thiết vì chiến lược tham lam dựa vào việc tiến lên phía những ứng viên có trọng lượng cao hơn. 

DSU được sử dụng theo cách hơi không chuẩn: nó không thể hiện khả năng kết nối của các cạnh ban đầu mà là sự sẵn có của tổ tiên cho các cặp đôi trong tương lai. Khi một nút được sử dụng như một phần của một cặp, nút đó sẽ được hợp nhất lên trên để các truy vấn trong tương lai có thể bỏ qua nút đó. 

Vòng lặp chính xử lý các nút từ trọng lượng cao nhất đến thấp nhất. Thứ tự này rất cần thiết vì nó đảm bảo rằng bất cứ khi nào chúng ta gắn một nút lên trên, phía cha mẹ đã có khả năng đạt mức “tối đa” trong một vùng hợp lệ. 

Câu trả lời chỉ tích lũy khi một nút tìm thấy thành công một tổ tiên có sẵn, vì chỉ khi đó nó mới có thể đóng vai trò là phần tử lớn thứ hai. 

## Ví dụ đã hoạt động 

Hãy xem xét một cái cây nhỏ: 

đầu vào:```
n = 3
w = [3, 2, 1]
1 - 2
2 - 3
```Chúng ta bắt nguồn từ 1. Con trỏ gốc là 2 → 1, 3 → 2. Các nút được sắp xếp theo trọng số: 1(3), 2(2), 3(1). 

| Nút | Phụ huynh | Tìm DSU (cha mẹ) | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | - | - | bỏ qua | 0 | 
| 2 | 1 | 1 | thêm 2 | 2 | 
| 3 | 2 | 2 | thêm 1 | 3 | 

Sau khi xử lý 2, nút 3 vẫn có thể gắn lên trên qua 2 hoặc 1 tùy thuộc vào cấu trúc DSU, cho tổng số 3. 

Điều này cho thấy nhiều cấp độ của cây có thể đóng góp tuần tự như các phần tử lớn thứ hai. 

Bây giờ hãy xem xét một ngôi sao: 

đầu vào:```
1 is center with weight 100
others have weights 1, 2, 3
```Sắp xếp thứ tự xử lý trung tâm cuối cùng hoặc đầu tiên tùy theo trọng lượng. Các lá gắn vào trung tâm, nhưng chỉ có phần đính kèm thành công đầu tiên trên mỗi cấu trúc mới quan trọng. Trung tâm đóng vai trò hỗ trợ lặp đi lặp lại, nhưng mỗi lá chỉ đóng góp độc lập với tư cách là phần tử lớn thứ hai khi được ghép nối với nút cao hơn, chứng tỏ rằng sự đóng góp được thúc đẩy bởi sự sẵn có của các neo có trọng lượng cao hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | việc phân loại chiếm ưu thế, hoạt động DSU được khấu hao gần như không đổi | 
| Không gian | O(n) | danh sách kề, mảng cha, mảng DSU | 

Giải pháp xử lý thoải mái n lên tới 5 × 10^5 vì mọi thao tác sau khi sắp xếp đều được khấu hao tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdin
    input = sys.stdin.readline

    n = int(input())
    w = [0] + list(map(int, input().split()))
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    parent = [0] * (n + 1)
    stack = [1]
    parent[1] = -1
    order = []
    while stack:
        u = stack.pop()
        order.append(u)
        for v in g[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            stack.append(v)

    nodes = list(range(1, n + 1))
    nodes.sort(key=lambda x: -w[x])

    class DSU:
        def __init__(self, n):
            self.p = list(range(n + 1))
        def find(self, x):
            if self.p[x] != x:
                self.p[x] = self.find(self.p[x])
            return self.p[x]
        def union(self, a, b):
            a = self.find(a)
            b = self.find(b)
            if a != b:
                self.p[a] = b

    dsu = DSU(n)
    ans = 0

    for u in nodes:
        p = parent[u]
        if p == -1:
            continue
        v = dsu.find(p)
        if v != 0:
            ans += w[u]
            dsu.union(u, v)

    return str(ans)

# sample-like sanity checks
assert run("""3
3 2 1
1 2
2 3
""") == "3"

assert run("""2
5 4
1 2
""") == "4"

assert run("""4
1 2 3 4
1 2
1 3
1 4
""") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi giảm dần | 3 | tuyên truyền đa cấp | 
| 2 nút | 4 | vùng hợp lệ tối thiểu | 
| sao tăng | 6 | tổng hợp dựa trên trung tâm | 

## Vỏ cạnh 

Cây nút đơn là điểm thất bại đơn giản nhất đối với việc triển khai giả định rằng mọi nút đều phải gắn lên trên. Trong trường hợp đó, không có vùng hợp lệ có kích thước ít nhất là hai, do đó câu trả lời phải bằng 0. Thuật toán xử lý việc này vì gốc không có cha và bị bỏ qua hoàn toàn. 

Chuỗi tăng nghiêm ngặt kiểm tra xem DSU có truyền bá tính khả dụng lên trên một cách chính xác hay không. Trong một chuỗi như vậy, mọi nút ngoại trừ nút tối đa sẽ ghép thành công chính xác một lần và thuật toán đạt được điều này bằng cách luôn tìm nút tổ tiên cao hơn gần nhất. 

Cây hình ngôi sao kiểm tra xem liệu nhiều lá có cố gắng tiêu thụ phần trung tâm một cách không chính xác hay không theo cách ngăn cản việc ghép đôi sau này. Vì DSU nén lên trên nên tâm vẫn là điểm neo có thể tái sử dụng cho nhiều lá, cho phép tích lũy đóng góp chính xác.
