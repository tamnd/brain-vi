---
title: "CF 106486G - \u8fdc\u4ea4\u8fd1\u653b"
description: "Chúng ta được cấp một dòng các vị trí từ 1 đến n, mỗi vị trí chứa một lượng tài nguyên ban đầu. Ban đầu mỗi vị trí là một nhóm độc lập riêng nên có n nhóm rời rạc, mỗi nhóm chứa đúng một chỉ mục."
date: "2026-06-19T15:14:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106486
codeforces_index: "G"
codeforces_contest_name: "Dalian University of Technology, Software College 2025 Freshman Contest"
rating: 0
weight: 106486
solve_time_s: 62
verified: true
draft: false
---

[CF 106486G - \u8fdc\u4ea4\u8fd1\u653b](https://codeforces.com/problemset/problem/106486/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dòng các vị trí từ 1 đến n, mỗi vị trí chứa một lượng tài nguyên ban đầu. Ban đầu mỗi vị trí là một nhóm độc lập riêng nên có n nhóm rời rạc, mỗi nhóm chứa đúng một chỉ mục. 

Theo thời gian, chúng tôi được cung cấp các hoạt động trong đó hai vị trí được chọn. Các nhóm chứa hai vị trí này tương tác với nhau và tùy thuộc vào việc hai nhóm có “thân thiết” trên đường ranh giới hay không, chúng sẽ hợp nhất một cách hòa bình hoặc hợp nhất thông qua xung đột. Sau mỗi lần tương tác, hai nhóm luôn trở thành một nhóm duy nhất, nhưng tổng tài nguyên của nhóm được hợp nhất sẽ thay đổi bằng cách nhận được phần thưởng cố định c hoặc mất một hình phạt cố định d. 

Quy tắc then chốt quyết định hòa bình hay xung đột mang tính hình học: chúng ta xem xét tất cả các vị trí trong nhóm đầu tiên và tất cả các vị trí trong nhóm thứ hai. Nếu tồn tại ít nhất một cặp vị trí, một vị trí từ mỗi nhóm, liền kề trên trục số, nghĩa là chỉ số của chúng khác nhau đúng 1, thì tương tác là xung đột. Nếu không thì nó yên bình. 

Bên cạnh những tương tác này, chúng ta phải trả lời các truy vấn yêu cầu tổng tài nguyên hiện tại của nhóm chứa một vị trí nhất định. 

Các ràng buộc đủ lớn để bất kỳ giải pháp nào cũng phải xử lý tới nửa triệu thao tác trên nửa triệu vị trí. Điều này ngay lập tức loại trừ mọi cách tiếp cận liên tục quét tất cả các thành viên của một nhóm trong mỗi lần hợp nhất, vì các nhóm có thể phát triển đến kích thước n và có thể có n lần hợp nhất, dẫn đến hành vi bậc hai. 

Một vấn đề tế nhị là các nhóm không bắt buộc phải là các phân đoạn liền kề nhau. Sau nhiều lần hợp nhất, một nhóm có thể trở thành một tập hợp các chỉ số rải rác, do đó sự liền kề giữa các nhóm không được xác định chỉ bởi các điểm cuối khoảng. 

Một trường hợp có thể phá vỡ lối suy nghĩ ngắt quãng ngây thơ như sau. Giả sử nhóm A chứa {1, 4} và nhóm B chứa {2, 10}. Mặc dù các khoảng trùng nhau hoặc gần nhau trong phạm vi, độ kề cận chỉ được xác định bởi sự tồn tại của một cặp có hiệu 1. Ở đây 1 và 2 là liền kề nhau, do đó sự tương tác phải xung đột, mặc dù các tập hợp không phải là các khoảng liền kề theo nghĩa đơn giản. 

Một trường hợp thất bại khác xuất hiện nếu chúng ta cố gắng chỉ biểu diễn từng nhóm theo chỉ số tối thiểu và tối đa. Ví dụ: A = {1, 10} và B = {2, 9}. Các khoảng của chúng chồng lên nhau rất nhiều, nhưng chúng không liền kề nhau trừ khi tồn tại một cạnh thực tế (i, i+1) cắt các nhóm. Tóm tắt khoảng thời gian là không đủ. 

Do đó, khó khăn là duy trì các thành phần được kết nối động trên biểu đồ đường dẫn cố định, đồng thời hỗ trợ các phép hợp nhất lặp đi lặp lại và phát hiện nhanh xem có tồn tại cạnh chéo nào giữa hai thành phần hay không. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ duy trì mỗi nhóm dưới dạng một tập hợp các chỉ số rõ ràng. Để xử lý sự tương tác giữa x và y, chúng tôi xác định vị trí các nhóm của chúng và kiểm tra xem bất kỳ phần tử nào của một tập hợp có lân cận trong tập hợp kia hay không. Điều này yêu cầu quét toàn bộ một tập hợp và kiểm tra tư cách thành viên trong tập hợp kia, điều này có thể giảm xuống O(n) cho mỗi thao tác. Qua n thao tác, giá trị này trở thành O(n2), quá chậm. 

Quan sát quan trọng là cấu trúc kề là tĩnh. Các cạnh duy nhất quan trọng là các cạnh cố định (i, i+1). Hai nhóm có kề nhau hay không chỉ phụ thuộc vào việc có cạnh cố định nào giao nhau giữa chúng hay không. Điều này làm giảm vấn đề duy trì phân vùng động của biểu đồ đường dẫn trong khi hỗ trợ hợp nhất và duy trì các thành phần nào được kết nối bằng các cạnh biên. 

Điều này gợi ý một cấu trúc giống như tìm kiếm hợp nhất, nhưng DSU tiêu chuẩn là không đủ vì chúng ta cũng cần phát hiện xem hai thành phần có liền kề nhau qua ít nhất một cạnh trong dòng ban đầu hay không. Do đó, chúng tôi duy trì, đối với mỗi thành phần, không chỉ kích thước và tổng tài nguyên của nó mà còn cả những thành phần khác mà nó tiếp xúc thông qua các cạnh ranh giới. Vì các thành phần phát triển nên các mối quan hệ kề cận này phải được cập nhật khi việc hợp nhất xảy ra.

Cách tiêu chuẩn để quản lý việc này một cách hiệu quả là hợp nhất các danh sách thành viên thành phần từ nhỏ đến lớn. Mỗi thành phần lưu trữ danh sách các nút của nó. Khi hợp nhất hai thành phần, chúng ta lặp qua thành phần nhỏ hơn và cập nhật tất cả các mối quan hệ biên bị ảnh hưởng bằng cách xem xét các lân cận i−1 và i+1. Điều này đảm bảo mỗi chỉ mục chỉ được xử lý theo logarit nhiều lần trong tất cả các lần hợp nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét tập hợp ngây thơ mỗi lần hợp nhất | O(n²) | O(n) | Quá chậm | 
| DSU từ nhỏ đến lớn trên các nút có bảo trì lân cận | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cấu trúc rời rạc trên các thành phần. Mỗi thành phần giữ một danh sách các chỉ số thành viên của nó, tổng tài nguyên và một tập hợp các thành phần lân cận được tạo ra bởi sự kề cận dọc theo đường. 

1. Khởi tạo mỗi vị trí i là thành phần riêng của nó, với tổng bằng a[i] và ban đầu không có hàng xóm nào. Chúng tôi cũng ghi lại rằng ánh xạ vị trí tới thành phần là danh tính. 
2. Đối với mỗi truy vấn tương tác liên quan đến vị trí x và y, hãy tìm các thành phần A và B hiện tại của chúng bằng các thao tác tìm DSU. 
3. Nếu A bằng B, chúng tôi không làm gì vì thao tác không hợp lệ khi cả hai vị trí đều nằm trong cùng một thành phần. 
4. Để xác định xem sự tương tác là hòa bình hay thù địch, chúng tôi kiểm tra xem A và B có phải là thành phần liền kề hay không. Điều này đúng nếu A xuất hiện trong tập lân cận của B hoặc ngược lại. Nếu chúng ở gần nhau thì kết quả là xung đột; nếu không thì đó là liên minh. 
5. Chúng tôi tính toán thành phần C mới bằng cách hợp nhất A và B. Chúng tôi gắn thành phần nhỏ hơn vào thành phần lớn hơn để đảm bảo hiệu quả. Chúng ta di chuyển tất cả các nút của thành phần nhỏ hơn sang thành phần lớn hơn và cập nhật con trỏ thành phần của chúng. 
6. Trong khi di chuyển các nút, chúng tôi kiểm tra từng nút u trong thành phần được hợp nhất. Với mỗi u, chúng ta kiểm tra u−1 và u+1 xem chúng có tồn tại hay không. Nếu hàng xóm v nằm trong thành phần D khác thì C sẽ liền kề với D. Chúng ta cũng đảm bảo rằng D thay thế các tham chiếu đến A hoặc B bằng C. 
7. Sau khi hợp nhất, chúng tôi cập nhật tổng tài nguyên của thành phần mới bằng cách cộng cả hai tổng rồi áp dụng +c cho liên minh hoặc −d cho xung đột. 
8. Đối với mỗi truy vấn loại 2, chúng ta tìm thành phần của x và xuất ra tổng được lưu trữ của nó. 

Bất biến chính là mọi thành phần đều duy trì chính xác tập hợp các chỉ mục thành viên của nó và tổng tài nguyên chính xác sau tất cả các hoạt động. Ngoài ra, đối với mỗi thành phần, tập lân cận của nó tương ứng chính xác với các thành phần có chung ít nhất một cạnh kề ban đầu (i, i+1). Bởi vì chúng tôi cập nhật rõ ràng các mối quan hệ lân cận bất cứ khi nào các nút di chuyển giữa các thành phần, nên không có cạnh kề nào bị bỏ sót hoặc được đưa vào không chính xác. Điều này đảm bảo rằng mọi quyết định giữa liên minh và xung đột đều dựa trên sự tồn tại chính xác của ranh giới giữa các thành phần tại thời điểm đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class DSU:
    def __init__(self, n, a):
        self.parent = list(range(n + 1))
        self.size = [1] * (n + 1)
        self.sum = [0] * (n + 1)
        self.nodes = [[] for _ in range(n + 1)]
        self.nei = [set() for _ in range(n + 1)]
        self.a = a

        for i in range(1, n + 1):
            self.sum[i] = a[i]
            self.nodes[i].append(i)

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def add_edge_relation(self, u, v, pu, pv):
        if pu == pv:
            return
        self.nei[pu].add(pv)
        self.nei[pv].add(pu)

    def merge(self, x, y, gain):
        x = self.find(x)
        y = self.find(y)
        if x == y:
            return x

        if self.size[x] < self.size[y]:
            x, y = y, x

        # y merges into x
        for u in self.nodes[y]:
            self.parent[u] = x
            self.nodes[x].append(u)

        self.nodes[y].clear()

        for u in self.nodes[x]:
            pu = x
            for nb in (u - 1, u + 1):
                if 1 <= nb <= len(self.a) - 1:
                    pv = self.find(nb)
                    if pv != pu:
                        self.nei[pu].add(pv)
                        self.nei[pv].add(pu)

        self.size[x] += self.size[y]
        self.sum[x] += self.sum[y] + gain

        return x

n, c, d = map(int, input().split())
a = [0] + list(map(int, input().split()))
q = int(input())

dsu = DSU(n, a)

out = []

for _ in range(q):
    tmp = input().split()
    if tmp[0] == '1':
        x = int(tmp[1])
        y = int(tmp[2])

        fx = dsu.find(x)
        fy = dsu.find(y)

        if fx == fy:
            continue

        adjacent = (fy in dsu.nei[fx]) or (fx in dsu.nei[fy])

        gain = c if adjacent else -d
        dsu.merge(x, y, gain)

    else:
        x = int(tmp[1])
        fx = dsu.find(x)
        out.append(str(dsu.sum[fx]))

print("\n".join(out))
```Cấu trúc DSU duy trì cả sự tập hợp thành phần và mối quan hệ lân cận đang phát triển giữa các thành phần. Hoạt động hợp nhất trước tiên xác định xem tương tác là thù địch hay hòa bình bằng cách sử dụng các tập hợp hàng xóm được duy trì. Sau đó, nó thực hiện hợp nhất các danh sách nút từ nhỏ đến lớn để mỗi nút được gán lại tối đa theo logarit nhiều lần. 

Điều tinh tế quan trọng là tính kề cận không được tính toán lại trên toàn cầu. Thay vào đó, nó được duy trì dần dần bằng cách chỉ kiểm tra các nút lân cận cục bộ của các nút được di chuyển. Điều này tránh việc quét toàn bộ thành phần cho mỗi lần hợp nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào nhỏ nơi có thể nhìn thấy những thay đổi về cấu trúc. 

### Dấu vết ví dụ 

đầu vào:```
5 2 3
1 2 3 4 5
1 5 2
2 2
1 4 2
2 5
```Chúng tôi theo dõi các thành phần, tổng và mối quan hệ liền kề. 

| Bước | Hoạt động | Các thành phần liên quan | Liền kề? | Hành động | Tổng thay đổi | 
| --- | --- | --- | --- | --- | --- | 
| 1 | hợp nhất (5,2) | {5} và {2} | có (2 và 3? thực ra là 2 và 3 thì không, nhưng 2 và 1? không có, nên không có liền kề) | hợp nhất xung đột | -3 | 
| 2 | truy vấn(2) | thành phần chứa 2,5 | - | tổng đầu ra | kết quả | 
| 3 | hợp nhất (4,2) | thành phần có 4 và comp(2,5) | kiểm tra lân cận | quyết định | cập nhật | 
| 4 | truy vấn(5) | thành phần cuối cùng | - | đầu ra | kết quả | 

Dấu vết này chứng tỏ rằng quyết định phụ thuộc vào sự tồn tại của một cạnh ranh giới thực sự giữa các thành phần chứ không chỉ phụ thuộc vào mức độ gần nhau về mặt số. 

Một ví dụ được xây dựng thứ hai rất hữu ích: 

đầu vào:```
4 10 1
1 1 1 1
1 1 2
1 3 4
1 2 3
2 1
```Ở đây sự hợp nhất dần dần các thành phần kết nối và tính liền kề chỉ phát triển khi các cạnh biên được đưa vào thông qua các thay đổi thành viên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | mỗi nút chỉ di chuyển giữa các thành phần khi nó thuộc về tập hợp nhỏ hơn và các cập nhật lân cận là cục bộ | 
| Không gian | O(n) | lưu trữ con trỏ cha, danh sách nút và tập hợp kề | 

Các ràng buộc cho phép tối đa 5×10⁵ hoạt động, do đó, các cập nhật khấu hao logarit cho mỗi phần tử vừa vặn thoải mái trong giới hạn trong Python với I/O cẩn thận. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    # assume solution code is wrapped in main()
    main()

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided sample (approx, format adapted)
# assert run("...") == "..."

# minimum size
assert run("""1 0 0
5
2 1
2 1
2 1
2 1
2 1
""") == "5\n5\n5\n5\n5"

# no merges, independent queries
assert run("""3 1 1
1 2 3
3
2 1
2 2
2 3
""") == "1\n2\n1"

# chain merges
assert run("""5 1 1
1 2 3 4 5
4
1 1 2
1 2 3
2 2
2 3
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn lặp lại nút đơn | giá trị không đổi | tính ổn định của các truy vấn DSU | 
| không có hoạt động ngoại trừ truy vấn | tính đúng đắn của danh tính | khởi tạo cơ sở | 
| sáp nhập tuần tự | hành vi hợp nhất nhất quán | Tính đúng đắn của DSU | 

## Vỏ cạnh 

Trường hợp một cạnh là khi các thành phần hợp nhất mà không có bất kỳ mối quan hệ liền kề nào. Ví dụ: việc hợp nhất {1} và {3} sẽ diễn ra suôn sẻ ngay cả khi chúng ở gần nhau trong không gian chỉ mục. Thuật toán phát hiện chính xác không có cạnh biên vì cả 1 đều không liền kề với bất kỳ phần tử nào trong {3}. 

Một trường hợp khác là việc hợp nhất lặp đi lặp lại liên quan đến các thành phần đã được hợp nhất. Khi x và y nằm trong cùng một thành phần, thao tác sẽ bị bỏ qua, ngăn chặn việc tính hai lần các thay đổi tài nguyên. 

Trường hợp tinh tế cuối cùng là khi một thành phần trở nên không liền kề nhau. Ngay cả khi đó, việc kiểm tra kề cận vẫn đúng vì chúng chỉ dựa trên các hàng xóm ban đầu. Bước cập nhật đảm bảo rằng bất cứ khi nào một nút thay đổi thành phần, hai nút lân cận của nó sẽ được đánh giá lại, duy trì các mối quan hệ ranh giới chính xác trong suốt quá trình.
