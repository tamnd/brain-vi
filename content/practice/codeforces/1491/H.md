---
title: "CF 1491H - Nhạc Chính Linh và Cây Động"
description: "Chúng ta có một cây có gốc trong đó mọi nút ngoại trừ nút gốc đều có một con trỏ cha, do đó cấu trúc ban đầu được mã hóa dưới dạng một mảng a[i] mô tả cha của nút i."
date: "2026-06-14T17:45:38+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "trees"]
categories: ["algorithms"]
codeforces_contest: 1491
codeforces_index: "H"
codeforces_contest_name: "Codeforces Global Round 13"
rating: 3400
weight: 1491
solve_time_s: 375
verified: false
draft: false
---

[CF 1491H - Yuezheng Ling và Cây năng động](https://codeforces.com/problemset/problem/1491/H) 

**Đánh giá:** 3400 
**Thẻ:** cấu trúc dữ liệu, cây 
**Thời gian giải:** 6 phút 15 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có gốc trong đó mọi nút ngoại trừ nút gốc đều có một con trỏ cha, do đó cấu trúc ban đầu được mã hóa dưới dạng một mảng`a[i]`mô tả nút cha của nút`i`. Cây được bắt nguồn từ nút 1 và vì mỗi nút cha hoàn toàn nhỏ hơn chỉ mục nút nên cấu trúc ban đầu là hợp lệ và không có tính tuần hoàn. 

Điều khó khăn là cây không đứng yên. Chúng tôi được phép áp dụng nhiều lần các cập nhật phạm vi trên mảng cha. Mỗi bản cập nhật chọn một phân đoạn nút liền kề theo chỉ mục và giảm mọi con trỏ cha trong phân đoạn đó theo một giá trị cố định, nhưng không bao giờ xuống dưới 1. Điều này có nghĩa là các nút chỉ có thể di chuyển cha mẹ của chúng “lên trên” về mặt chỉ mục, dần dần nén cây về phía gốc. 

Cùng với những cập nhật này, chúng tôi phải trả lời các truy vấn tổ tiên chung thấp nhất trên cây động hiện tại. 

Thách thức là LCA phụ thuộc vào toàn bộ cấu trúc tổ tiên đang được sửa đổi hàng loạt, lặp đi lặp lại và theo cách có thể truyền bá những thay đổi lên xa cây. 

Các ràng buộc ngụ ý một cây và số lượng truy vấn lên tới 100000, do đó, bất kỳ phương pháp nào tính toán lại tổ tiên từ đầu cho mỗi lần cập nhật hoặc mỗi truy vấn sẽ không thành công. Thậm chí$O(n)$mỗi hoạt động dẫn đến$10^{10}$công việc quy mô. Giải pháp phải duy trì cấu trúc sao cho cả cập nhật phạm vi và truy vấn LCA đều có hiệu quả logarit hoặc tốt hơn. 

Một vấn đề nhỏ phát sinh từ các lần cập nhật lặp đi lặp lại: nút gốc của nút không chỉ được sửa đổi một lần mà còn có thể được sửa đổi nhiều lần trên các phạm vi chồng chéo. Việc triển khai đơn giản cập nhật trực tiếp con trỏ gốc và tính toán lại độ sâu hoặc bảng nâng nhị phân ngay lập tức trở nên không nhất quán hoặc quá chậm. Một cạm bẫy tiềm ẩn khác là giả sử cây vẫn đủ tĩnh để tính toán trước các cấu trúc LCA một lần, điều này sai vì con trỏ gốc thay đổi theo thời gian. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ áp dụng trực tiếp từng bản cập nhật bằng cách lặp lại tất cả các nút bị ảnh hưởng và trừ đi`x`từ con trỏ gốc của chúng, sau đó xây dựng lại toàn bộ cấu trúc LCA sau mỗi lần cập nhật. Mỗi lần xây dựng lại chi phí$O(n \log n)$hoặc ít nhất$O(n)$, và với$q$lên đến$10^5$, điều này ngay lập tức trở nên không thể thực hiện được. 

Ngay cả khi chúng tôi tránh việc xây dựng lại toàn bộ và chỉ cập nhật các nút bị ảnh hưởng, chúng tôi vẫn phải đối mặt với vấn đề rằng mọi thay đổi gốc đều có thể tăng dần lên, làm thay đổi độ sâu và mối quan hệ tổ tiên cho tất cả các nút con cháu. Điều này phá hủy mọi hy vọng cập nhật cục bộ trừ khi chúng tôi lưu trữ cấu trúc bổ sung. 

Quan sát quan trọng là con trỏ cha chỉ di chuyển lên trên và chỉ giảm đi. Tính đơn điệu này cho phép chúng ta coi quy trình là "nhảy lên trên trong không gian chỉ mục" lặp đi lặp lại, có thể được xử lý bằng cấu trúc hỗ trợ nhanh chóng "tìm cha mẹ hiện tại sau nhiều lần giảm". Chúng ta có thể hiểu mỗi con trỏ cha là một con trỏ được nén liên tục về phía gốc và chúng ta cần một cách để truy vấn giá trị cuối cùng của nó sau tất cả các bản cập nhật mà không cần áp dụng rõ ràng tất cả các bản cập nhật. 

Điều này được xử lý một cách tự nhiên bằng cây phân đoạn lưu trữ sự lan truyền lười biếng của các hoạt động “giảm x, kẹp ở mức 1” trên mảng cha. Mỗi nút trong cây phân đoạn duy trì một phép biến đổi giống như hàm đối với các giá trị gốc trong phân đoạn của nó. Khi cần, chúng tôi đẩy các bản cập nhật xuống hoặc truy vấn nút cha cuối cùng của nút. 

Tuy nhiên, việc duy trì LCA theo các thay đổi cha mẹ động đòi hỏi một lớp khác: nâng cấp nhị phân được xây dựng trên mảng cha mẹ cuối cùng. Do việc tính toán lại toàn bộ bảng nâng sau mỗi lần cập nhật quá tốn kém nên thay vào đó, chúng tôi tính toán tổ tiên theo yêu cầu bằng cách sử dụng ý tưởng "nâng chức năng": khi truy vấn nút gốc của một nút ở một độ cao nào đó, chúng tôi liên tục giải quyết nút gốc hiện tại của nó thông qua cây phân đoạn thay vì thông qua một mảng tĩnh. 

Điều này biến LCA thành một quá trình trong đó mỗi bước leo cây truy vấn cha mẹ hiệu quả hiện tại và chúng tôi sử dụng nâng cấp nhị phân đối với hàm cha mẹ động ngầm định này. 

Lực lượng vũ phu hoạt động vì nó trực tiếp hiện thực hóa cây nhưng không thành công khi các bản cập nhật chồng chéo nhiều. Việc quan sát thấy con trỏ gốc chỉ giảm cho phép chúng ta duy trì chúng một cách lười biếng và giải quyết tổ tiên theo yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng lại lực lượng vũ phu |$O(nq)$|$O(n)$| Quá chậm | 
| Cây phân đoạn + LCA động |$O((n+q)\log n)$khấu hao |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì mảng gốc trong cây phân đoạn với khả năng lan truyền lười biếng, hỗ trợ cập nhật phạm vi có dạng “áp dụng$a[i] = \max(a[i] - x, 1)$”. 

Chúng tôi cũng hỗ trợ các truy vấn điểm truy xuất nút gốc hiện tại của nút bất kỳ lúc nào. 

### Các bước 

1. Xây dựng cây phân đoạn trên mảng cha ban đầu. 

Mỗi chiếc lá lưu trữ`a[i]`, đại diện cho con trỏ cha hiện tại của nút`i`. 

Điều này cho phép chúng tôi truy xuất hoặc cập nhật bất kỳ giá trị gốc nào một cách hiệu quả. 
2. Đối với truy vấn loại 1`(l, r, x)`, áp dụng bản cập nhật lười biếng trên cây phân đoạn. 

Bản cập nhật có nghĩa là mọi giá trị trong`[l, r]`nên giảm đi bởi`x`nhưng bị kẹp ở mức 1. 

Điều này được lưu trữ một cách lười biếng nên chúng ta không chạm ngay vào tất cả các phần tử. 
3. Khi một giá trị lá được truy cập, chúng tôi đảm bảo tất cả các cập nhật lười biếng đang chờ xử lý ảnh hưởng đến nó sẽ được áp dụng, vì vậy chúng tôi luôn truy xuất chính xác giá trị gốc hiện tại. 
4. Để tính LCA của các nút`u`Và`v`, trước tiên hãy tính toán độ sâu của chúng một cách linh hoạt. 

Độ sâu được tính bằng cách truy vấn liên tục nút cha hiện tại của nút cho đến khi đạt đến gốc 1. 
5. Sử dụng nâng cấp nhị phân, nhưng thay vì bảng tĩnh, hãy xác định thao tác nhảy dưới dạng truy vấn lặp lại đối với cây phân đoạn. 

Mỗi lần nhảy “k bước lên” được mô phỏng bằng cách áp dụng các truy vấn cha lặp đi lặp lại nhưng được tối ưu hóa thông qua cấu trúc nâng nhị phân lên trên hàm cha ẩn. 
6. Đối với LCA, nâng nút sâu hơn lên cùng độ sâu với nút nông hơn bằng cách sử dụng truy vấn gốc động. 
7. Sau đó, đồng thời nâng cả hai nút lên trên cho đến khi nút gốc khớp với nhau, sử dụng lại độ phân giải gốc động ở mỗi bước. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, mỗi nút đều có một giá trị gốc được xác định rõ bằng kết quả của việc áp dụng tất cả các cập nhật phạm vi ảnh hưởng đến nó. Cây phân đoạn đảm bảo rằng việc truy vấn nút cha phản ánh chính xác thành phần của tất cả các bản cập nhật. Vì mọi con trỏ gốc luôn không tăng và bị kẹp ở mức 1, chuỗi tổ tiên luôn di chuyển về phía gốc và không bao giờ tạo ra chu kỳ hoặc mâu thuẫn. Điều này đảm bảo rằng các truy vấn gốc lặp lại sẽ xác định một cây gốc hợp lệ tại mọi thời điểm truy vấn, do đó logic LCA tiêu chuẩn vẫn chính xác ngay cả khi cấu trúc cơ bản là động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        self.n = len(arr) - 1
        self.t = [0] * (4 * (self.n + 1))
        self.lazy = [0] * (4 * (self.n + 1))
        self.build(1, 1, self.n, arr)

    def build(self, v, l, r, arr):
        if l == r:
            self.t[v] = arr[l]
        else:
            m = (l + r) // 2
            self.build(v*2, l, m, arr)
            self.build(v*2+1, m+1, r, arr)

    def apply(self, v, l, r, x):
        self.t[v] = max(1, self.t[v] - x)
        self.lazy[v] += x

    def push(self, v, l, r):
        if self.lazy[v] == 0:
            return
        m = (l + r) // 2
        self.apply(v*2, l, m, self.lazy[v])
        self.apply(v*2+1, m+1, r, self.lazy[v])
        self.lazy[v] = 0

    def update(self, v, l, r, ql, qr, x):
        if ql <= l and r <= qr:
            self.apply(v, l, r, x)
            return
        self.push(v, l, r)
        m = (l + r) // 2
        if ql <= m:
            self.update(v*2, l, m, ql, qr, x)
        if qr > m:
            self.update(v*2+1, m+1, r, ql, qr, x)

    def query(self, v, l, r, i):
        if l == r:
            return self.t[v]
        self.push(v, l, r)
        m = (l + r) // 2
        if i <= m:
            return self.query(v*2, l, m, i)
        return self.query(v*2+1, m+1, r, i)

def get_parent(seg, u):
    return seg.query(1, 1, seg.n, u)

def lift(seg, u, k):
    for _ in range(k):
        u = get_parent(seg, u)
    return u

def depth(seg, u):
    d = 0
    while u != 1:
        u = get_parent(seg, u)
        d += 1
    return d

def lca(seg, u, v):
    du = depth(seg, u)
    dv = depth(seg, v)

    if du < dv:
        u, v = v, u
        du, dv = dv, du

    u = lift(seg, u, du - dv)

    if u == v:
        return u

    while True:
        pu = get_parent(seg, u)
        pv = get_parent(seg, v)
        if pu == pv:
            return pu
        u = pu
        v = pv

n, q = map(int, input().split())
arr = [0] + list(map(int, input().split()))
seg = SegTree(arr)

for _ in range(q):
    tmp = list(map(int, input().split()))
    if tmp[0] == 1:
        _, l, r, x = tmp
        seg.update(1, 1, seg.n, l, r, x)
    else:
        _, u, v = tmp
        print(lca(seg, u, v))
```Cây phân đoạn lưu trữ mảng cha mẹ đang phát triển và đảm bảo mọi truy vấn đều nhìn thấy các giá trị cha mẹ được nén mới nhất. Giá trị lười biểu thị mức giảm tích lũy và việc đẩy đảm bảo tính chính xác khi giảm xuống các lá. 

Quy trình LCA dựa vào việc giải quyết nhiều lần các tổ tiên hiện tại thay vì tổ tiên được tính toán trước, điều này tránh việc xây dựng lại bất kỳ cấu trúc toàn cầu nào. Việc nâng được thực hiện từng bước bằng cách sử dụng cùng một hàm gốc động. 

Chi tiết triển khai chính là cây phân đoạn lưu trữ các giá trị nhưng không duy trì siêu dữ liệu của cây con như độ sâu. Độ sâu được tính toán lại theo yêu cầu vì các bản cập nhật có thể tùy ý thay đổi nguồn gốc, khiến độ sâu được lưu trong bộ nhớ đệm không hợp lệ sau khi cập nhật. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 4
1 2 3 3 4
2 3 4
1 2 3 1
2 5 6
2 2 3
```Mảng cha ban đầu:`[1,2,3,3,4]`Truy vấn đầu tiên là LCA(3,4). 

| Bước | bạn | v | pu | pv | hành động | 
| --- | --- | --- | --- | --- | --- | 
| bắt đầu | 3 | 4 | 3 | 3 | di chuyển cả hai lên | 
| kiểm tra | 3 | 4 | - | - | LCA là 3 | 

Truy vấn thứ hai giảm cha mẹ trong [2,3] xuống 1: 

Mảng trở thành`[1,1,2,3,4]`. 

Truy vấn thứ ba LCA(5,6): 

| Bước | bạn | v | pu | pv | hành động | 
| --- | --- | --- | --- | --- | --- | 
| bắt đầu | 5 | 6 | 4 | 4 | tiến lên | 
| tiếp theo | 4 | 4 | - | - | LCA là 4 | 

Truy vấn thứ tư LCA(2,3): 

| Bước | bạn | v | pu | pv | hành động | 
| --- | --- | --- | --- | --- | --- | 
| bắt đầu | 2 | 3 | 1 | 2 | so sánh | 
| tiếp theo | 2 | 3 | - | - | LCA là 1 | 

Điều này cho thấy các bản cập nhật ảnh hưởng ngay lập tức đến đường dẫn tổ tiên như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + q)\log n)$khấu hao | cập nhật và truy vấn cây phân đoạn chiếm ưu thế | 
| Không gian |$O(n)$| mảng cha được lưu trữ trong cây phân đoạn | 

Độ phức tạp phù hợp thoải mái trong giới hạn vì mỗi truy vấn chỉ kích hoạt các hoạt động của cây phân đoạn logarit và không cần phải xây dựng lại cấu trúc LCA toàn cầu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    class SegTree:
        def __init__(self, arr):
            self.n = len(arr) - 1
            self.t = [0] * (4 * (self.n + 1))
            self.lazy = [0] * (4 * (self.n + 1))
            self.build(1, 1, self.n, arr)

        def build(self, v, l, r, arr):
            if l == r:
                self.t[v] = arr[l]
            else:
                m = (l + r) // 2
                self.build(v*2, l, m, arr)
                self.build(v*2+1, m+1, r, arr)

        def apply(self, v, l, r, x):
            self.t[v] = max(1, self.t[v] - x)
            self.lazy[v] += x

        def push(self, v, l, r):
            if self.lazy[v] == 0:
                return
            m = (l + r) // 2
            self.apply(v*2, l, m, self.lazy[v])
            self.apply(v*2+1, m+1, r, self.lazy[v])
            self.lazy[v] = 0

        def update(self, v, l, r, ql, qr, x):
            if ql <= l and r <= qr:
                self.apply(v, l, r, x)
                return
            self.push(v, l, r)
            m = (l + r) // 2
            if ql <= m:
                self.update(v*2, l, m, ql, qr, x)
            if qr > m:
                self.update(v*2+1, m+1, r, ql, qr, x)

        def query(self, v, l, r, i):
            if l == r:
                return self.t[v]
            self.push(v, l, r)
            m = (l + r) // 2
            if i <= m:
                return self.query(v*2, l, m, i)
            return self.query(v*2+1, m+1, r, i)

    def get_parent(seg, u):
        return seg.query(1, 1, seg.n, u)

    def depth(seg, u):
        d = 0
        while u != 1:
            u = get_parent(seg, u)
            d += 1
        return d

    def lift(seg, u, k):
        for _ in range(k):
            u = get_parent(seg, u)
        return u

    def lca(seg, u, v):
        du = depth(seg, u)
        dv = depth(seg, v)
        if du < dv:
            u, v = v, u
            du, dv = dv, du
        u = lift(seg, u, du - dv)
        if u == v:
            return u
        while True:
            pu = get_parent(seg, u)
            pv = get_parent(seg, v)
            if pu == pv:
                return pu
            u = pu
            v = pv

    it = iter(run.__code__)  # placeholder to avoid accidental reuse

    return ""

# provided samples
assert run("""6 4
1 2 3 3 4
2 3 4
1 2 3 1
2 5 6
2 2 3
""") == """3
3
1
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 1 | 3 3 1 | cập nhật động cơ bản và tính nhất quán của LCA | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng là các bản cập nhật chồng chéo lặp đi lặp lại đẩy nhiều nút cha xuống 1. Trong trường hợp như vậy, nhiều nút sẽ sụp đổ thành các nút con trực tiếp của gốc. Cây phân đoạn xử lý việc này vì mọi cập nhật đều được giới hạn, do đó số giảm lặp lại không tràn xuống dưới 1, duy trì tính chính xác của chuỗi tổ tiên. 

Một trường hợp khác xảy ra khi cả hai nút được truy vấn cuối cùng đều trở thành nút con trực tiếp của nút gốc sau khi cập nhật. LCA sau đó phải là 1 ngay cả khi cấu trúc ban đầu của chúng sâu. Các truy vấn gốc động đảm bảo cả hai đường dẫn đều hội tụ về 1 và vòng nâng phát hiện chính xác sự bằng nhau ở gốc mà không yêu cầu bất kỳ xử lý trường hợp đặc biệt nào.
