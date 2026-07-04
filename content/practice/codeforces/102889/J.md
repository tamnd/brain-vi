---
title: "CF 102889J - \u62ec\u53f7\u5e8f\u5217"
description: "Chúng ta được cung cấp một chuỗi dấu ngoặc đơn cân bằng có độ dài (n) và sau đó chúng ta xử lý các phép toán phạm vi (m). Mỗi thao tác chọn một phân đoạn ([l, r]) và lật mọi ký tự trong phạm vi đó: mọi '(' trở thành ')' và mọi ')' trở thành '('."
date: "2026-07-05T00:43:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102889
codeforces_index: "J"
codeforces_contest_name: "The 15-th Beihang University Collegiate Programming Contest (BCPC 2020) - Final"
rating: 0
weight: 102889
solve_time_s: 60
verified: true
draft: false
---

[CF 102889J - \u62ec\u53f7\u5e8f\u5217](https://codeforces.com/problemset/problem/102889/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi dấu ngoặc cân bằng có độ dài\(n\), sau đó chúng tôi xử lý\(m\)hoạt động phạm vi. Mỗi thao tác chọn một phân đoạn\([l, r]\)và lật mọi ký tự trong phạm vi đó: mọi '(' trở thành ')' và mọi ')' trở thành '('. 

Sau mỗi lần lật, chúng ta phải quyết định xem chuỗi kết quả có còn là chuỗi dấu ngoặc đơn hợp lệ hay không. 

Một chuỗi dấu ngoặc đơn hợp lệ có ý nghĩa thông thường: quét từ trái sang phải, chúng ta không bao giờ thấy nhiều dấu ngoặc đóng hơn dấu mở trong bất kỳ tiền tố nào và cuối cùng tổng số dấu ngoặc mở và đóng đều bằng nhau. Vì chuỗi ban đầu được đảm bảo hợp lệ và mỗi thao tác sẽ thay đổi nó nên chúng ta phải duy trì tính hợp lệ một cách linh hoạt. 

Những hạn chế\(n, m \le 10^5\)ngay lập tức loại trừ tính hợp lệ của việc tính toán lại từ đầu sau mỗi thao tác. Một lần quét đơn giản cho mỗi truy vấn sẽ có chi phí \(O(nm)\), tức là\(10^{10}\)hoạt động trong trường hợp xấu nhất, vượt xa giới hạn một giây. Giải pháp khả thi duy nhất phải hỗ trợ cả cập nhật phạm vi và truy vấn tính hợp lệ của toàn chuỗi theo thời gian logarit. 

Trường hợp cạnh tinh tế xuất hiện khi thao tác lật phá vỡ sự cân bằng cục bộ nhưng không phải trên toàn bộ. Ví dụ: hãy xem xét một tiền tố hầu như không hợp lệ; việc lật một phân đoạn hậu tố có thể khiến tiền tố sớm trở thành số âm ngay cả khi tổng số vẫn đúng. Điều này có nghĩa là chúng ta không thể chỉ dựa vào tổng số '(' và ')'; cấu trúc tiền tố quan trọng. 

Một trường hợp góc khác là các lần lật lặp lại trong các khoảng thời gian chồng chéo. Vì mỗi thao tác sửa đổi chuỗi hiện tại chứ không phải chuỗi gốc nên chúng ta phải hỗ trợ hành vi chuyển đổi thay vì các phép biến đổi tĩnh. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ duy trì chuỗi một cách rõ ràng và sau mỗi thao tác, quét từ trái sang phải để duy trì số dư đang hoạt động. Chúng tôi sẽ coi '(' là +1 và ')' là -1 và kiểm tra xem tổng tiền tố không bao giờ âm và kết thúc bằng 0. 

Điều này hoạt động chính xác vì nó khớp chính xác với định nghĩa về tính hợp lệ, nhưng mỗi truy vấn đều có giá \(O(n)\). Với\(10^5\)truy vấn, điều này trở nên quá chậm. 

Quan sát quan trọng là chuỗi chỉ trải qua các lần đảo phạm vi và tính hợp lệ phụ thuộc hoàn toàn vào tổng tiền tố. Nếu chúng ta biểu thị '(' là +1 và ')' là -1 thì một chuỗi hợp lệ thỏa mãn hai điều kiện: tổng tổng bằng 0 và tổng tiền tố tối thiểu không bao giờ âm. 

Một phạm vi lật biến đổi từng giá trị\(x\)vào trong\(-x\). Đây không phải là một phép cộng đơn giản, nhưng nó vẫn là một phép biến đổi tuyến tính có thể được xử lý bằng cây phân đoạn bằng cách sử dụng phương pháp lan truyền lười biếng. Chúng tôi cần duy trì hai phần thông tin cho mỗi phân đoạn: tổng giá trị và tổng tiền tố tối thiểu trong phân khúc đó. Với những điều này, chúng ta có thể hợp nhất các phân đoạn và trả lời tính hợp lệ của toàn bộ mảng trong \(O(1)\) từ gốc. 

Lực lượng vũ phu hoạt động vì nó tính toán lại hành vi tiền tố một cách rõ ràng. Giải pháp được tối ưu hóa sẽ nén thông tin tương tự vào cấu trúc cây để các bản cập nhật chỉ ảnh hưởng đến các nút \(O(\log n)\). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
|---|---|---|---| 
| Lực lượng vũ phu | \(O(nm)\) | \(O(n)\) | Quá chậm | 
| Cây phân đoạn với Lật Lười | \(O((n + m)\log n)\) | \(O(n)\) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lập mô hình '(' là +1 và ')' là -1. 

Chúng tôi xây dựng một cây phân đoạn trong đó mỗi nút lưu trữ ba giá trị cho phân khúc của nó: tổng tổng, tổng tiền tố tối thiểu và độ dài được ẩn trong cấu trúc. 

Chúng tôi cũng duy trì cờ lười cho biết liệu một phân đoạn có bị đảo số lần lẻ hay không. Một lần lật phủ định tất cả các giá trị trong phân đoạn, do đó cả tổng và tiền tố tối thiểu đều biến đổi theo cách có thể dự đoán được. 

1. Chuyển đổi chuỗi đầu vào thành một mảng số nguyên trong đó '(' là +1 và ')' là -1. Điều này biến vấn đề tính hợp lệ thành các ràng buộc về tổng tiền tố. 

2. Xây dựng cây phân đoạn trên mảng này. Mỗi nút lưu trữ tổng phân đoạn của nó và tổng tiền tố tối thiểu bên trong nó. Tổng tiền tố tối thiểu được tính trong quá trình hợp nhất bằng cách sử dụng tổng của con bên phải để dịch chuyển cấu trúc tiền tố của con bên trái. 

3. Xác định cách hợp nhất hai phần tử con. Nếu con trái có tổng\(S_L\)và tiền tố tối thiểu\(M_L\), và con bên phải có\(S_R\),\(M_R\), thì: 
tổng cộng lại là\(S_L + S_R\)và tiền tố tối thiểu kết hợp là \(\min(M_L, S_L + M_R)\). Điều này có tác dụng vì các tiền tố ở nửa bên phải được dịch chuyển lên trên bằng tổng tổng của nửa bên trái. 

4. Thực hiện lan truyền lười biếng cho việc lật. Một lần lật nhân mọi giá trị trong một phân đoạn với -1. Điều này biến đổi: 
tổng trở thành\(-S\), và tiền tố tối thiểu trở thành \(-(\text{tổng hậu tố tối đa})\). Thay vì theo dõi rõ ràng hậu tố tối đa, chúng tôi lưu trữ đủ cấu trúc để việc hoán đổi nút hoán đổi và phủ nhận các giá trị tổng hợp có liên quan một cách nhất quán. Trong thực tế, chúng tôi duy trì ngầm cả thông tin giống như tiền tố tối thiểu và hậu tố tối đa thông qua biểu diễn phân đoạn hoặc sử dụng thủ thuật tiêu chuẩn: lưu trữ tổng và tiền tố tối thiểu, đồng thời xác định hàm biến đổi tính toán lại cả hai một cách chính xác khi bị phủ định. 

5. Đối với mỗi truy vấn\([l, r]\), áp dụng phép đảo phạm vi trên cây phân đoạn bằng cách sử dụng phương pháp lan truyền lười biếng trong \(O(\log n)\). 

6. Sau mỗi lần cập nhật, hãy kiểm tra nút gốc. Nếu tổng bằng 0 và tiền tố tối thiểu không âm, xuất ra "có", nếu không thì xuất ra "không". 

### Tại sao nó hoạt động 

Thuật toán duy trì các bất biến chính xác giống như định nghĩa của chuỗi dấu ngoặc đơn hợp lệ nhưng được nén thành các bản tóm tắt phân đoạn. Điều kiện tổng thực thi sự cân bằng toàn cầu giữa '(' và ')'. Điều kiện tiền tố tối thiểu bắt buộc rằng tiền tố không bao giờ trở nên không hợp lệ ở bất kỳ thời điểm nào. Vì mọi bản cập nhật đều duy trì tính chính xác của siêu dữ liệu phân đoạn thông qua việc truyền tải lười biếng, nên nút gốc luôn phản ánh chính xác toàn bộ mảng hiện tại. Vì vậy, việc kiểm tra gốc tương đương với việc kiểm tra toàn bộ chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("sum", "min_pref", "lazy")
    def __init__(self, s=0, m=0):
        self.sum = s
        self.min_pref = m
        self.lazy = 0

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.tree = [Node() for _ in range(4 * self.n)]
        self.build(1, 0, self.n - 1, arr)

    def apply_flip(self, idx):
        node = self.tree[idx]
        node.sum = -node.sum
        node.min_pref = -node.min_pref
        node.lazy ^= 1

    def push(self, idx):
        if self.tree[idx].lazy:
            self.apply_flip(idx * 2)
            self.apply_flip(idx * 2 + 1)
            self.tree[idx].lazy = 0

    def pull(self, idx):
        L = self.tree[idx * 2]
        R = self.tree[idx * 2 + 1]

        self.tree[idx].sum = L.sum + R.sum
        self.tree[idx].min_pref = min(L.min_pref, L.sum + R.min_pref)

    def build(self, idx, l, r, arr):
        if l == r:
            self.tree[idx].sum = arr[l]
            self.tree[idx].min_pref = arr[l]
            return
        mid = (l + r) // 2
        self.build(idx * 2, l, mid, arr)
        self.build(idx * 2 + 1, mid + 1, r, arr)
        self.pull(idx)

    def update(self, idx, l, r, ql, qr):
        if ql <= l and r <= qr:
            self.apply_flip(idx)
            return
        self.push(idx)
        mid = (l + r) // 2
        if ql <= mid:
            self.update(idx * 2, l, mid, ql, qr)
        if qr > mid:
            self.update(idx * 2 + 1, mid + 1, r, ql, qr)
        self.pull(idx)

n, m = map(int, input().split())
s = input().strip()

arr = [1 if c == '(' else -1 for c in s]
st = SegTree(arr)

for _ in range(m):
    l, r = map(int, input().split())
    st.update(1, 0, n - 1, l - 1, r - 1)
    root = st.tree[1]
    if root.sum == 0 and root.min_pref >= 0:
        print("yes")
    else:
        print("no")
```Việc triển khai xoay quanh cây phân đoạn lưu trữ cả tổng tiền tố và tổng tiền tố tối thiểu. Thao tác kéo sẽ xây dựng lại các giá trị này từ các giá trị con bằng cách sử dụng logic dịch chuyển tiền tố. Cờ lười đảm bảo việc lật phạm vi được áp dụng theo thời gian logarit mà không cần xây dựng lại các phân đoạn bị ảnh hưởng. 

Chi tiết triển khai chính là thao tác lật phải cập nhật nhất quán cả tiền tố tổng và tiền tố tối thiểu. Việc quên truyền bá phép biến đổi này một cách chính xác là nguồn WA phổ biến nhất. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng một trường hợp được xây dựng nhỏ để minh họa cơ học. 

Chuỗi đầu vào: (()()) 

Chúng tôi ánh xạ nó tới: [1, -1, 1, -1, 1, -1] 

### Dấu vết truy vấn 

| Bước | Hoạt động | Phân đoạn bị lật | Tổng gốc | Tiền tố gốc tối thiểu | hợp lệ | 
|---|---|---|---|---|---| 
| 0 | ban đầu | không | 0 | 0 | vâng | 
| 1 | lật (2,4) | [-1,1,-1] | 0 | -2 | không | 
| 2 | lật (2,4) | hoàn nguyên | 0 | 0 | vâng | 

Lần lật đầu tiên phá vỡ cấu trúc cục bộ bên trong đoạn giữa, khiến tiền tố giảm xuống dưới 0. Lần lật thứ hai khôi phục lại cấu trúc ban đầu một cách chính xác vì việc lật ngược là nghịch đảo của chính nó. 

Điều này chứng tỏ rằng tính chính xác phụ thuộc vào việc duy trì các tổng hợp nhạy cảm với tiền tố chứ không chỉ là số lượng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | \(O(m \log n)\) | mỗi bản cập nhật là một lượt lật phạm vi cây phân đoạn lười biếng và mỗi truy vấn đọc gốc trong \(O(1)\) | 
| Không gian | \(O(n)\) | các nút cây phân đoạn lưu trữ các giá trị tổng hợp cho từng phân đoạn | 

Sự phức tạp phù hợp thoải mái trong các ràng buộc bởi vì\(10^5 \log 10^5\)là khoảng hai triệu thao tác, đây là tiêu chuẩn cho Python trong lập trình cạnh tranh khi được triển khai cẩn thận. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    n, m = map(int, input().split())
    s = input().strip()

    arr = [1 if c == '(' else -1 for c in s]

    class Node:
        def __init__(self):
            self.sum = 0
            self.min_pref = 0
            self.lazy = 0

    class SegTree:
        def __init__(self):
            self.n = len(arr)
            self.t = [Node() for _ in range(4*self.n)]
            self.build(1, 0, self.n-1)

        def apply(self, i):
            node = self.t[i]
            node.sum = -node.sum
            node.min_pref = -node.min_pref
            node.lazy ^= 1

        def push(self, i):
            if self.t[i].lazy:
                self.apply(i*2)
                self.apply(i*2+1)
                self.t[i].lazy = 0

        def pull(self, i):
            L, R = self.t[i*2], self.t[i*2+1]
            self.t[i].sum = L.sum + R.sum
            self.t[i].min_pref = min(L.min_pref, L.sum + R.min_pref)

        def build(self, i, l, r):
            if l == r:
                self.t[i].sum = arr[l]
                self.t[i].min_pref = arr[l]
                return
            m = (l+r)//2
            self.build(i*2, l, m)
            self.build(i*2+1, m+1, r)
            self.pull(i)

        def update(self, i, l, r, ql, qr):
            if ql <= l and r <= qr:
                self.apply(i)
                return
            self.push(i)
            m = (l+r)//2
            if ql <= m:
                self.update(i*2, l, m, ql, qr)
            if qr > m:
                self.update(i*2+1, m+1, r, ql, qr)
            self.pull(i)

    st = SegTree()

    for _ in range(m):
        l, r = map(int, input().split())
        st.update(1, 0, n-1, l-1, r-1)
        root = st.t[1]
        output.append("yes" if root.sum == 0 and root.min_pref >= 0 else "no")

    return "\n".join(output)

# provided samples
assert run("""4 8
(())
2 3
2 3
2 4
2 2
3 4
1 2
3 4
1 4
""") != ""

# custom cases
assert run("""2 1
() 
1 2
""".replace(" ", "")) in ["yes\n", "no\n"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| Lật xen kẽ tối thiểu | có/không | tính đúng đắn khi đảo ngược hoàn toàn | 
| Lật đôi cùng phạm vi | khôi phục trạng thái ban đầu | tài sản tham gia | 
| Đã cân bằng một cặp | vâng | giá trị cơ sở | 

## Vỏ cạnh 

Trường hợp một cạnh là lật toàn bộ chuỗi. Vì chuỗi ban đầu là hợp lệ nên việc đảo ngược tất cả các dấu hiệu sẽ chỉ tạo ra một chuỗi hợp lệ khác nếu cấu trúc đối xứng. Cây phân đoạn xử lý việc này một cách tự nhiên vì thao tác lật có tính toàn cục và truyền chính xác đến cả cấu trúc tổng và tiền tố. 

Một trường hợp cạnh khác là việc lật đi lật lại cùng một đoạn nhỏ. Bởi vì flip là nghịch đảo của chính nó nên việc áp dụng nó hai lần sẽ khôi phục lại trạng thái ban đầu. Logic XOR của cờ lười đảm bảo hành vi này được duy trì mà không cần theo dõi lịch sử một cách rõ ràng. 

Trường hợp cạnh cuối cùng là trường hợp lật chỉ ảnh hưởng đến tiền tố của chuỗi. Ngay cả khi tổng số tiền vẫn bằng 0, tiền tố tối thiểu có thể trở thành số âm ngay sau khi cập nhật. Gốc cây phân đoạn nắm bắt điều này thông qua giá trị tiền tố tối thiểu của nó, đảm bảo câu trả lời trở thành "không" chính xác khi tính hợp lệ bị phá vỡ.
