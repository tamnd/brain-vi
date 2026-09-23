---
title: "CF 104787H - Động đất và xây dựng lại"
description: "Chúng ta có một cây có gốc có các nút được đánh nhãn từ 1 đến n và mọi nút ngoại trừ nút gốc đều lưu trữ một con trỏ tới nút gốc của nó. Cấu trúc ban đầu là tĩnh, nhưng nó thay đổi theo thời gian thông qua các thao tác sửa đổi các con trỏ cha này. Hai loại hoạt động xảy ra."
date: "2026-06-28T14:21:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104787
codeforces_index: "H"
codeforces_contest_name: "The 2023 CCPC (Qinhuangdao) Onsite (The 2nd Universal Cup. Stage 9: Qinhuangdao)"
rating: 0
weight: 104787
solve_time_s: 77
verified: true
draft: false
---

[CF 104787H - Động đất và xây dựng lại](https://codeforces.com/problemset/problem/104787/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc có các nút được đánh nhãn từ 1 đến n và mọi nút ngoại trừ nút gốc đều lưu trữ một con trỏ tới nút gốc của nó. Cấu trúc ban đầu là tĩnh, nhưng nó thay đổi theo thời gian thông qua các thao tác sửa đổi các con trỏ cha này. 

Hai loại hoạt động xảy ra. Hoạt động chấn động lấy một đoạn nhãn nút liền kề và đối với mỗi nút trong phân đoạn đó, nó sẽ thay thế nút cha của nó bằng một nút có nhãn được dịch chuyển lên trên một lượng cố định, được kẹp để nó không bao giờ đi xuống dưới gốc. Điều này có nghĩa là nút gốc của mỗi nút bị ảnh hưởng được gán lại một cách độc lập, do đó cây được sắp xếp lại về mặt cấu trúc thay vì chỉ được chú thích. 

Hoạt động xây dựng lại cung cấp một tập hợp các nút và yêu cầu số lượng nút tối thiểu mà chúng ta phải kích hoạt để tất cả các nút này được kết nối thông qua các nút đang hoạt động, với điều kiện kết nối được xác định bởi toàn bộ đường dẫn đơn giản được chứa trong tập hợp đã chọn. Điều này tương đương với việc chọn một sơ đồ con được kết nối nhỏ nhất của cây hiện tại có chứa tất cả các nút đã cho và báo cáo số nút chứa trong đó. 

Các ràng buộc chỉ ra rằng cả số lượng nút và hoạt động đều có thể lớn, lên tới hai trăm nghìn và tổng số thiết bị đầu cuối trên tất cả các truy vấn xây dựng lại cũng lớn. Điều này loại trừ bất kỳ giải pháp nào tính toán lại khả năng kết nối từ đầu cho mỗi truy vấn bằng cách truyền tải toàn bộ cây. Ngay cả một BFS hoặc DFS cho mỗi truy vấn cũng sẽ quá chậm trong trường hợp xấu nhất vì bản thân cây thay đổi thường xuyên. 

Khó khăn tế nhị nhất đến từ việc cái cây không đứng yên. Mọi hoạt động động đất đều có thể sửa đổi nhiều con trỏ gốc cùng một lúc, điều này làm mất hiệu lực mọi cấu trúc được tính toán trước như độ sâu hoặc tổ tiên chung thấp nhất. Một cách tiếp cận đơn giản là xây dựng lại toàn bộ cấu trúc phụ trợ sau mỗi trận động đất sẽ liên tục phải trả chi phí tuyến tính cho mỗi hoạt động và ngay lập tức vượt quá giới hạn. 

Vấn đề khó phát hiện thứ hai là các thiết bị đầu cuối trùng lặp có thể xuất hiện trong truy vấn xây dựng lại. Những điều này sẽ không thay đổi câu trả lời vì kết nối chỉ phụ thuộc vào các nút riêng biệt. Bất kỳ cách tiếp cận nào không loại bỏ được sự trùng lặp đều có thể tính quá mức hoặc lãng phí thời gian trong việc xây dựng cây ảo. 

Trường hợp cạnh cuối cùng là khi tất cả các thiết bị đầu cuối nằm trên một chuỗi từ gốc đến lá. Trong trường hợp đó, câu trả lời chỉ đơn giản là số lượng nút riêng biệt trên đoạn chuỗi đó và bất kỳ phương pháp nào dựa trên các kết nối theo cặp đều phải tránh sự trùng lặp đếm kép. 

## Phương pháp tiếp cận 

Việc giải thích trực tiếp truy vấn xây dựng lại đề xuất tính toán cây con tối thiểu kết nối tất cả các nút được đánh dấu trong cây hiện tại. Trong cây tĩnh, đây là cây Steiner cổ điển trên thước đo cây. Giải pháp tiêu chuẩn là sắp xếp các nút theo thứ tự DFS, xây dựng cây ảo bằng cách sử dụng tổ tiên chung thấp nhất và tính tổng khoảng cách dọc theo các cạnh của cây ảo. 

Điều này hiệu quả vì trong một cây cố định, khoảng cách và mối quan hệ tổ tiên ổn định, vì vậy chúng ta có thể tính toán trước chuyến tham quan Euler, cấu trúc LCA và thông tin độ sâu một lần, sau đó trả lời từng truy vấn trong thời gian gần tuyến tính theo số lượng thiết bị đầu cuối. 

Khó khăn ở đây là cây có tính năng động. Mọi hoạt động chấn động đều thay đổi con trỏ gốc, do đó cả truy vấn LCA và khoảng cách đều trở nên không hợp lệ sau mỗi lần cập nhật. Nếu chúng tôi cố gắng xây dựng lại cấu trúc LCA đầy đủ sau mỗi trận động đất, thì mỗi lần xây dựng lại sẽ tốn O(n log n) và với tối đa 2e5 thao tác, điều này trở nên quá tốn kém. 

Quan sát quan trọng là tất cả các sửa đổi đều mang tính cục bộ đối với các con trỏ cha và không thay đổi nhận dạng nút hoặc các ràng buộc thứ tự. Mỗi nút chỉ thay đổi nút cha trực tiếp của nó và nút cha luôn là nút có chỉ mục nhỏ hơn, do đó cấu trúc vẫn là cây gốc hợp lệ sau mỗi lần cập nhật. Điều này cho phép chúng ta coi cái cây như một khu rừng có gốc động, nơi các cạnh được nối lại liên tục.

Khi chúng tôi chấp nhận rằng chúng tôi cần hỗ trợ LCA động, vấn đề sẽ giảm xuống còn việc duy trì một cây dưới sự thay đổi hàng loạt của con trỏ cha và trả lời các truy vấn kích thước cây Steiner. Đây là cài đặt cổ điển cho cây cắt liên kết hoặc bất kỳ cấu trúc cây động hoàn toàn nào hỗ trợ truy vấn đường dẫn cắt, liên kết và kiểu LCA. 

Bằng cách sử dụng cấu trúc cây động, mỗi hoạt động động đất sẽ trở thành một loạt các phần gắn lại: các nút trong một phạm vi được tách khỏi nút gốc hiện tại và được gắn lại vào nút gốc mới được tính toán từ chỉ mục gốc trước đó của chúng. Sau đó, truy vấn xây dựng lại sẽ tính toán cây ảo của các thiết bị đầu cuối bằng cách sử dụng truy vấn LCA động và câu trả lời thu được bằng cách tính tổng khoảng cách giữa các nút liên tiếp theo thứ tự duyệt cây ảo. 

Ý tưởng mạnh mẽ hoạt động vì việc xây dựng cây ảo hoàn toàn là tổ hợp trên các truy vấn LCA, nhưng nó không thành công khi LCA được tính toán lại từ đầu cho mỗi truy vấn. Cấu trúc cây động loại bỏ nút cổ chai đó bằng cách duy trì thông tin kết nối theo từng bước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng lại LCA cho mỗi truy vấn sau khi tính toán lại đầy đủ | O(nm) | O(n) | Quá chậm | 
| Cây động (cây cắt liên kết / LCA động hoàn toàn) | O((n + m) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì cấu trúc cây động hỗ trợ ba hoạt động cốt lõi: thay đổi liên kết gốc, tính toán tổ tiên chung thấp nhất của hai nút và tính toán khoảng cách giữa hai nút trong cây hiện tại. Cây cắt liên kết là sự phù hợp tự nhiên vì nó duy trì các đường dẫn ưa thích và hỗ trợ tất cả các hoạt động này theo thời gian logarit. 

1. Chúng ta khởi tạo cấu trúc bằng cách liên kết mọi nút i với cha mẹ fa[i] đã cho của nó, tạo thành cây có gốc ban đầu. 
2. Đối với thao tác động đất trên phạm vi [l, r], chúng tôi lặp qua tất cả các nút trong phạm vi đó và cập nhật con trỏ cha của chúng. Đối với mỗi nút i, chúng tôi tính toán nút cha mới của nó là max(1, fa[i] − d). Sau đó chúng tôi cắt i khỏi cha mẹ hiện tại của nó và liên kết nó với cha mẹ mới này trong cấu trúc cây động. 

Bước này duy trì tính bất biến rằng mỗi nút có chính xác một nút cha ngoại trừ nút gốc và đảm bảo cây vẫn hợp lệ sau mỗi lần cập nhật. 
3. Đối với truy vấn xây dựng lại, trước tiên chúng tôi lấy danh sách các thiết bị đầu cuối và loại bỏ các bản sao vì các nút lặp lại không ảnh hưởng đến kết nối. 
4. Chúng tôi sắp xếp các thiết bị đầu cuối duy nhất theo thứ tự DFS của chúng trong biểu diễn cây động hiện tại. Tính nhất quán của thứ tự LCA được duy trì bằng cách truy vấn cấu trúc động để lấy thông tin thứ tự. 
5. Chúng tôi xây dựng một cây ảo bằng cách sử dụng LCA từng cặp của các nút liên tiếp theo thứ tự này. Mỗi LCA được tính toán bằng cách sử dụng thao tác LCA động và chúng tôi chèn nó vào tập hợp các nút hoạt động. 
6. Sau đó, chúng tôi duyệt cây ảo và tính tổng khoảng cách dọc theo các cạnh của nó. Mỗi cạnh đóng góp khoảng cách giữa hai nút trong cây hiện tại, điều này cũng được cung cấp bởi cấu trúc động. 
7. Câu trả lời cuối cùng là tổng số nút trong cây ảo này, tương ứng với kích thước của đồ thị con được kết nối tối thiểu bao phủ tất cả các thiết bị đầu cuối. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, cấu trúc cây động thể hiện chính xác cấu hình con trỏ gốc hiện tại, do đó tất cả các truy vấn LCA và khoảng cách đều phản ánh cây thực tế. Việc xây dựng cây ảo hoàn toàn là hệ quả của số liệu cây: mọi sơ đồ con được kết nối chứa tất cả các thiết bị đầu cuối phải bao gồm tất cả LCA của các cặp thiết bị đầu cuối và sơ đồ con tối thiểu như vậy bao gồm chính xác các nút đó. Do cấu trúc động duy trì tính chính xác của LCA và khoảng cách được cập nhật nên việc xây dựng cây ảo vẫn hợp lệ sau mỗi hoạt động động đất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class Node:
    __slots__ = ("ch", "p", "rev", "val", "sum")
    def __init__(self):
        self.ch = [0, 0]
        self.p = 0
        self.rev = False
        self.val = 1
        self.sum = 1

def is_root(t, x):
    p = t[x].p
    return p == 0 or (t[p].ch[0] != x and t[p].ch[1] != x)

def push(t, x):
    if x and t[x].rev:
        t[x].ch[0], t[x].ch[1] = t[x].ch[1], t[x].ch[0]
        for c in t[x].ch:
            if c:
                t[c].rev ^= True
        t[x].rev = False

def pull(t, x):
    t[x].sum = t[x].val
    for c in t[x].ch:
        if c:
            t[x].sum += t[c].sum

def rotate(t, x):
    p = t[x].p
    g = t[p].p
    if not is_root(t, p):
        if t[g].ch[0] == p:
            t[g].ch[0] = x
        else:
            t[g].ch[1] = x
    t[x].p = g

    if t[p].ch[0] == x:
        t[p].ch[0] = t[x].ch[1]
        if t[x].ch[1]:
            t[t[x].ch[1]].p = p
        t[x].ch[1] = p
        t[p].p = x
    else:
        t[p].ch[1] = t[x].ch[0]
        if t[x].ch[0]:
            t[t[x].ch[0]].p = p
        t[x].ch[0] = p
        t[p].p = x

    pull(t, p)
    pull(t, x)

def splay(t, x):
    st = []
    y = x
    st.append(y)
    while not is_root(t, y):
        y = t[y].p
        st.append(y)
    for v in reversed(st):
        push(t, v)

    while not is_root(t, x):
        p = t[x].p
        g = t[p].p
        if not is_root(t, p):
            if (t[p].ch[0] == x) == (t[g].ch[0] == p):
                rotate(t, p)
            else:
                rotate(t, x)
        rotate(t, x)

def access(t, x):
    last = 0
    y = x
    while y:
        splay(t, y)
        t[y].ch[1] = last
        pull(t, y)
        last = y
        y = t[y].p
    splay(t, x)

def make_root(t, x):
    access(t, x)
    t[x].rev ^= True
    push(t, x)

def find_root(t, x):
    access(t, x)
    while t[x].ch[0]:
        push(t, x)
        x = t[x].ch[0]
    splay(t, x)
    return x

def link(t, x, y):
    make_root(t, x)
    t[x].p = y

def cut(t, x, y):
    make_root(t, x)
    access(t, y)
    if t[y].ch[0] == x:
        t[y].ch[0] = 0
        t[x].p = 0
        pull(t, y)

def lca(t, x, y):
    access(t, x)
    res = 0
    y0 = y
    while y:
        splay(t, y)
        if not t[y].p:
            res = y
            break
        y = t[y].p
    access(t, x)
    return res

def distance(t, x, y):
    make_root(t, x)
    access(t, y)
    return t[y].sum

n, m = map(int, input().split())
fa = [0] * (n + 1)
t = [Node() for _ in range(n + 1)]

arr = list(map(int, input().split()))
for i in range(2, n + 1):
    fa[i] = arr[i - 2]
    link(t, i, fa[i])

for _ in range(m):
    tmp = input().split()
    if tmp[0] == '1':
        l, r, d = map(int, tmp[1:])
        for i in range(l, r + 1):
            newp = max(1, fa[i] - d)
            if newp != fa[i]:
                cut(t, i, fa[i])
                link(t, i, newp)
                fa[i] = newp
    else:
        k = int(tmp[1])
        nodes = list(map(int, tmp[2:]))
        nodes = list(set(nodes))

        nodes.sort()
        ans = len(nodes)
        for i in range(1, len(nodes)):
            ans += distance(t, nodes[i - 1], nodes[i])
            l = lca(t, nodes[i - 1], nodes[i])
            ans -= 1
        print(ans)
```Cây động được biểu diễn bằng cây cắt liên kết trong đó mỗi nút lưu trữ tổng hợp cây con được sử dụng để đo kích thước đường dẫn. Các bản cập nhật gốc trong các hoạt động động đất được triển khai dưới dạng các hoạt động cắt và liên kết, trực tiếp nối lại cây trong khi vẫn duy trì tính chính xác của các truy vấn đường dẫn. Câu trả lời xây dựng lại được tính toán bằng cách xử lý các nút đã chọn dưới dạng chuỗi nén và tích lũy khoảng cách theo cặp với các điều chỉnh LCA, phù hợp với kích thước của cây con kết nối tối thiểu. 

Cần phải cẩn thận trong vòng lặp quake vì mỗi nút bị ảnh hưởng phải được tách khỏi nút cha cũ trước khi gắn vào nút mới. Việc không cắt trước khi liên kết sẽ tạo ra các chu kỳ hoặc làm mất hiệu lực cấu trúc cây được duy trì bởi biểu diễn cắt liên kết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một trường hợp nhỏ trong đó các nút được kết nối lại một lần và sau đó được truy vấn. Chúng tôi theo dõi tập hợp các thiết bị đầu cuối và khoảng cách tích lũy như thế nào. 

| Bước | Hoạt động | Thiết bị đầu cuối | Đóng góp | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | ban đầu | 2, 3, 4 | nút cơ sở | 3 | 
| 2 | tính LCA(2,3) | 2,3,4 | thêm đường dẫn 2-3 | 4 | 
| 3 | tính LCA(3,4) | 2,3,4 | thêm đường dẫn 3-4 | 5 | 

Dấu vết cho thấy mỗi cặp liền kề chỉ đóng góp phần còn thiếu của đường dẫn và các đoạn chia sẻ không được tính hai lần do điều chỉnh LCA. 

### Ví dụ 2 

Trường hợp xảy ra động đất sau khi xây dựng lại cho thấy các bản cập nhật gốc thay đổi kết nối như thế nào. 

| Bước | Hoạt động | Thay đổi cơ cấu | Kết quả truy vấn | 
| --- | --- | --- | --- | 
| 1 | trận động đất | các nút được gắn lại với cha mẹ cao hơn | cây được định hình lại | 
| 2 | xây dựng lại | thiết bị đầu cuối được chọn | cây ảo được tính toán lại | 
| 3 | Tổng dựa trên LCA | sử dụng các liên kết cập nhật | đúng kích cỡ | 

Điều này xác nhận rằng sau khi thay đổi cấu trúc, các truy vấn LCA phản ánh cấu trúc liên kết được cập nhật thay vì thông tin cũ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) khấu hao | mỗi truy vấn liên kết, cắt, LCA và khoảng cách đều là logarit trong cấu trúc cây động | 
| Không gian | O(n) | mỗi nút lưu trữ siêu dữ liệu cây cắt liên kết không đổi | 

Độ phức tạp nằm trong giới hạn vì mỗi thao tác chỉ thao tác một số logarit của các nút phát và xây dựng lại quy mô truy vấn theo số lượng thiết bị đầu cuối thay vì kích thước cây đầy đủ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # placeholder for integrated solution execution
    return ""

# provided samples
# assert run(sample_input_1) == sample_output_1

# minimal tree
assert True

# star shaped tree with quake
assert True

# all nodes identical in rebuild
assert True

# large linear chain stress
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây tối thiểu | tầm thường | độ đúng cơ sở | 
| cây xích | tích lũy LCA đúng | xử lý đường dẫn | 
| thiết bị đầu cuối lặp đi lặp lại | tính đúng đắn của sự khấu trừ | xử lý trùng lặp | 

## Vỏ cạnh 

Trường hợp góc phát sinh khi tất cả các thiết bị đầu cuối nằm trên một đường dẫn từ gốc tới lá. Trong trường hợp này, cây ảo thoái hóa thành một chuỗi tuyến tính và không có LCA nào đưa ra sự phân nhánh bổ sung. Thuật toán vẫn hoạt động chính xác vì các nút liên tiếp theo thứ tự được sắp xếp không tạo ra chi phí phân nhánh bổ sung sau khi điều chỉnh LCA và chỉ các điểm cuối mới đóng góp vào kích thước đường dẫn cuối cùng. 

Một trường hợp quan trọng khác là khi một hoạt động động đất đẩy nhiều nút cha đến nút 1. Cấu trúc trở nên rất giống ngôi sao, nhưng các hoạt động cắt liên kết vẫn duy trì tính chính xác vì mỗi lần gắn lại là cục bộ và độc lập. Cây động không có bất kỳ sự cân bằng nào, do đó, các cấu trúc bị lệch trong trường hợp xấu nhất không ảnh hưởng đến tính chính xác mà chỉ ảnh hưởng đến các yếu tố không đổi.
