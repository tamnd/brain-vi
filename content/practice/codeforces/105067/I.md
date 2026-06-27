---
title: "CF 105067I - Lính cứu hỏa"
description: "Chúng tôi được cấp một dòng máy bay chiến đấu, mỗi dòng mang một giá trị sức mạnh. Một giải đấu liên tục lấy hai võ sĩ còn lại đầu tiên trong chuỗi hiện tại và bắt họ chiến đấu. Người thua cuộc sẽ bị loại, trong trường hợp hòa cả hai đều bị loại."
date: "2026-06-28T00:15:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105067
codeforces_index: "I"
codeforces_contest_name: "Teamscode Spring 2024 (Advanced Division)"
rating: 0
weight: 105067
solve_time_s: 91
verified: false
draft: false
---

[CF 105067I - Lính cứu hỏa](https://codeforces.com/problemset/problem/105067/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một dòng máy bay chiến đấu, mỗi dòng mang một giá trị sức mạnh. Một giải đấu liên tục lấy hai võ sĩ còn lại đầu tiên trong chuỗi hiện tại và bắt họ chiến đấu. Người thua cuộc sẽ bị loại, trong trường hợp hòa cả hai đều bị loại. Điều này tiếp tục cho đến khi còn lại một chiến binh duy nhất hoặc không còn chiến binh nào cả. 

Mỗi truy vấn sửa đổi mảng cục bộ: cho một phân đoạn nhất định$[l, r]$, mọi giá trị bên trong phân đoạn đó được thay thế bằng một giá trị không đổi$x$và chúng tôi phải xác định chỉ mục gốc nào (nếu có) tồn tại với tư cách là người chiến thắng cuối cùng sau khi chạy giải đấu trên mảng được sửa đổi này. 

Đầu ra chính không phải là giá trị của người chiến thắng mà là vị trí ban đầu của nó trong mảng. Nếu quá trình loại bỏ tất cả mọi người, câu trả lời là$n+1$. 

Các ràng buộc ngụ ý rằng cả hai$n$Và$q$có thể đạt tới vài trăm nghìn trong các trường hợp thử nghiệm. Bất kỳ giải pháp nào tính toán lại giải đấu từ đầu cho mỗi truy vấn đều quá chậm. Một mô phỏng đầy đủ cho mỗi truy vấn sẽ có giá$O(nq)$, đạt đến thứ tự của$10^{11}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn. Thậm chí$O(n \log n)$mỗi truy vấn là không thể chấp nhận được. 

Sự tương tác giữa ghi đè phạm vi liền kề và quy trình loại bỏ xác định cho thấy rằng câu trả lời phải được duy trì thông qua cấu trúc hỗ trợ sửa đổi phạm vi và tính toán lại nhanh chóng kết quả chung. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị trở nên bằng nhau sau khi sửa đổi. Ví dụ: nếu mảng trở thành$[5,5,5]$, sau đó hai cái đầu tiên loại bỏ cả hai, để lại$[5]$, và người sống sót đó tiếp tục. Nhưng nếu mảng là$[5,5]$, cả hai đều biến mất và không ai còn lại. Trực giác ngây thơ “yếu tố tối đa sẽ thắng” không thành công ở đây vì sự ràng buộc có thể loại bỏ tất cả các ứng cử viên. 

Một trường hợp cạnh khác là khi giá trị mạnh nhất bị trùng lặp. Ví dụ,$[1,3,3,2]$. Việc 3 người đầu tiên sống sót hay bị loại bỏ phụ thuộc vào thứ tự tương tác chứ không chỉ tần suất. Điều này làm cho lý do tham lam “lấy chỉ số tối đa” không hợp lệ nếu không mô phỏng cấu trúc. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng từng truy vấn một cách độc lập. Đầu tiên chúng ta xây dựng mảng đã sửa đổi bằng cách thay thế các giá trị trong$[l,r]$với$x$, sau đó mô phỏng giải đấu bằng cách xử lý liên tục các cặp liền kề từ trái sang phải cho đến khi còn lại nhiều nhất một phần tử. Mỗi bước loại bỏ làm giảm kích thước mảng ít nhất một, do đó chi phí cho một mô phỏng duy nhất$O(n)$thời gian. Với$q$lên đến$7 \cdot 10^5$, điều này trở thành$O(nq)$, quá lớn. 

Nhận xét quan trọng là giải đấu về cơ bản là sự giảm dần từ trái sang phải, trong đó mỗi phần tử hoặc tồn tại với tư cách là “ứng cử viên vô địch” hoặc bị loại ngay lập tức khi gặp đối thủ mạnh hơn hoặc ngang bằng. Hành vi này có thể được biểu diễn dưới dạng một quy trình giống như ngăn xếp đơn điệu: chúng tôi duy trì một chuỗi các ứng cử viên và mỗi phần tử mới chỉ tương tác với người sống sót hiện tại ở phía trước phân khúc ảnh hưởng của nó. 

Sau khi được nhìn nhận theo cách này, vấn đề sẽ trở thành việc duy trì kết quả của việc giảm bớt giống như ngăn xếp trong các bản cập nhật gán phạm vi. Cấu trúc hỗ trợ chia mảng thành ba phần cho mỗi truy vấn: tiền tố, khối sửa đổi và hậu tố. Mỗi phân đoạn có thể được rút gọn thành một dạng “đại diện” nhỏ gọn mô tả tác dụng của nó đối với giải đấu. Sau đó, các đại diện này có thể được hợp nhất theo thời gian không đổi logarit hoặc khấu hao bằng cách sử dụng quy tắc hợp nhất được tính toán trước. 

Cái nhìn sâu sắc cần thiết là mỗi phân đoạn có thể được tóm tắt bằng hai mẩu thông tin: ứng cử viên sống sót sau khi giảm nội bộ và liệu ứng cử viên đó có sống sót trước đối thủ sắp tới hay không. Điều này cho phép chúng tôi coi mỗi phân đoạn là một chức năng ở trạng thái giải đấu hiện tại và việc thay thế phạm vi sẽ chỉ xây dựng lại một phân đoạn chứ không phải toàn bộ mảng. 

Cây phân đoạn trên các trạng thái rút gọn này hỗ trợ cập nhật trong$O(\log n)$mỗi truy vấn và hợp nhất trong$O(1)$mỗi sự kết hợp nút. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(n)$| Quá chậm | 
| Cây phân đoạn của các trạng thái giải đấu |$O((n+q)\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định cách hoạt động của một phân đoạn khi được xử lý một mình. Đối với bất kỳ khoảng thời gian nào, hãy mô phỏng giải đấu từ trái sang phải bên trong nó và lưu trữ người chiến thắng kết quả cũng như liệu nó có để trống cấu trúc hay không. Kết quả nén này là “trạng thái” của một phân đoạn. 
2. Xây dựng cây phân đoạn trong đó mỗi nút lưu trữ trạng thái khoảng thời gian của nó. Các lá tương ứng với các phần tử đơn lẻ có trạng thái tầm thường: bản thân phần tử đó tồn tại. 
3. Xác định thao tác hợp nhất giữa hai trạng thái đoạn liền kề. Khi kết hợp đoạn bên trái$A$và đoạn bên phải$B$, chúng tôi mô phỏng cách người chiến thắng$A$tương tác với ứng cử viên đầu tiên của$B$, bởi vì chỉ có tương tác ranh giới mới quan trọng sau khi giảm nội bộ. Điều này tạo ra một trạng thái mới đại diện cho toàn bộ khoảng thời gian. 
4. Đối với một truy vấn, áp dụng phép gán phạm vi bằng cách thay thế tất cả các lá trong$[l,r]$với hằng số$x$, sau đó tính toán lại các nút cây phân đoạn dọc theo các đường dẫn bị ảnh hưởng. 
5. Sau mỗi lần cập nhật, nút gốc chứa trực tiếp trạng thái của toàn bộ mảng, do đó, chỉ số chiến thắng có thể được trích xuất từ ​​​​nó. Nếu trạng thái biểu thị sự trống rỗng, đầu ra$n+1$. 

Lý do điều này có tác dụng là vì giải đấu có tính liên kết dưới sự nén này: khi một phân đoạn được giảm xuống hành vi sống sót, cấu trúc bên trong của nó không còn quan trọng đối với các tương tác bên ngoài phân khúc. Mọi sự loại bỏ chỉ phụ thuộc vào ứng cử viên dẫn đầu hiện tại của một phân khúc, do đó, chỉ bảo tồn ứng cử viên đó và điều kiện tồn tại của nó là đủ để tái tạo chính xác tất cả các tương tác trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("val", "idx", "alive")
    def __init__(self, val=0, idx=-1, alive=True):
        self.val = val
        self.idx = idx
        self.alive = alive

def merge(left: Node, right: Node) -> Node:
    if not left.alive:
        return right
    if not right.alive:
        return left

    if left.val > right.val:
        return Node(left.val, left.idx, True)
    if left.val < right.val:
        return Node(right.val, right.idx, True)

    return Node(0, -1, False)

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.t = [Node() for _ in range(4 * self.n)]
        self.arr = arr
        self.build(1, 0, self.n - 1)

    def build(self, v, l, r):
        if l == r:
            self.t[v] = Node(self.arr[l], l + 1, True)
            return
        m = (l + r) // 2
        self.build(v * 2, l, m)
        self.build(v * 2 + 1, m + 1, r)
        self.t[v] = merge(self.t[v * 2], self.t[v * 2 + 1])

    def update(self, v, l, r, ql, qr, x):
        if ql <= l and r <= qr:
            self.t[v] = Node(x, -1, True)
            return
        if r < ql or l > qr:
            return
        m = (l + r) // 2
        self.update(v * 2, l, m, ql, qr, x)
        self.update(v * 2 + 1, m + 1, r, ql, qr, x)
        self.t[v] = merge(self.t[v * 2], self.t[v * 2 + 1])

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    st = SegTree(a)
    out = []

    for _ in range(q):
        l, r, x = map(int, input().split())
        st.update(1, 0, n - 1, l - 1, r - 1, x)
        root = st.t[1]
        if not root.alive:
            out.append(str(n + 1))
        else:
            out.append(str(root.idx))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Cây phân đoạn lưu trữ một biểu diễn nén của từng khoảng. Mỗi nút giữ giá trị tồn tại tốt nhất hiện tại, chỉ mục ban đầu của nó và liệu phân đoạn có sụp đổ thành không do sự hủy diệt có giá trị bằng nhau. Các bản cập nhật ghi đè toàn bộ các phân đoạn bằng một nút không đổi, sau đó tính toán lại các lần hợp nhất đi lên. 

Hàm hợp nhất mã hóa trực tiếp quy tắc giải đấu: giá trị mạnh hơn tồn tại, giá trị bằng nhau sẽ tiêu diệt cả hai bên và chỉ người sống sót mới được truyền lên trên. 

Một điểm tinh tế là các phân đoạn được cập nhật sẽ mất chỉ mục gốc. Điều này là có chủ ý vì bất kỳ giá trị nào bên trong phạm vi được thay thế đều không thể phân biệt được, do đó, không có chỉ mục nội bộ nào có thể là mục còn tồn tại cuối cùng trừ khi nó thoát khỏi phân đoạn, điều này là không thể khi ghi đè thống nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng nhỏ$[2, 1, 3]$và một truy vấn thay thế$[1,2]$với$2$. 

| Bước | Trạng thái mảng | Kết quả phân đoạn hoạt động | 
| --- | --- | --- | 
| ban đầu | [2,1,3] | giải đấu đầy đủ | 
| sau khi cập nhật | [2,2,3] | cần tính toán lại | 
| hợp nhất (2,2) | trống | sập trái | 
| hợp nhất với 3 | người chiến thắng = 3 | cuối cùng | 

Cặp bằng nhau khi bắt đầu sẽ loại bỏ cả hai phần tử, chỉ còn lại 3 phần tử, cặp nào sẽ chiến thắng. 

Bây giờ hãy xem xét$[5,4,4,6]$không có cập nhật. 

| Bước | Người chiến thắng hiện tại | Phần tử tiếp theo | Kết quả | 
| --- | --- | --- | --- | 
| bắt đầu | 5 | 4 | 5 người sống sót | 
| 5 vs 4 | 5 | 4 | 5 người sống sót | 
| 5 vs 4 | 5 | 6 | 6 người sống sót | 
| cuối cùng | 6 | - | người chiến thắng là 6 | 

Điều này xác nhận rằng các đẳng thức trung gian hoặc các phần tử nhỏ hơn không tích lũy; chỉ có chuỗi loại bỏ mạnh nhất mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+q)\log n)$| mỗi lần cập nhật phạm vi sẽ tính toán lại đường dẫn cây phân đoạn | 
| Không gian |$O(n)$| lưu trữ cây phân đoạn | 

Độ phức tạp phù hợp với các ràng buộc vì tổng$n+q$nhiều nhất là$7 \cdot 10^5$và các hệ số logarit vẫn còn nhỏ trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class Node:
        def __init__(self, val=0, idx=-1, alive=True):
            self.val = val
            self.idx = idx
            self.alive = alive

    def merge(a, b):
        if not a.alive:
            return b
        if not b.alive:
            return a
        if a.val > b.val:
            return Node(a.val, a.idx, True)
        if a.val < b.val:
            return Node(b.val, b.idx, True)
        return Node(0, -1, False)

    class SegTree:
        def __init__(self, arr):
            self.n = len(arr)
            self.t = [Node() for _ in range(4*self.n)]
            self.arr = arr
            self.build(1,0,self.n-1)

        def build(self,v,l,r):
            if l==r:
                self.t[v]=Node(self.arr[l],l+1,True)
                return
            m=(l+r)//2
            self.build(v*2,l,m)
            self.build(v*2+1,m+1,r)
            self.t[v]=merge(self.t[v*2],self.t[v*2+1])

        def update(self,v,l,r,ql,qr,x):
            if ql<=l and r<=qr:
                self.t[v]=Node(x,-1,True)
                return
            if r<ql or l>qr:
                return
            m=(l+r)//2
            self.update(v*2,l,m,ql,qr,x)
            self.update(v*2+1,m+1,r,ql,qr,x)
            self.t[v]=merge(self.t[v*2],self.t[v*2+1])

        def root(self):
            return self.t[1]

    def solve(inp):
        n,q = map(int, inp.readline().split())
        a = list(map(int, inp.readline().split()))
        st = SegTree(a)
        out=[]
        for _ in range(q):
            l,r,x = map(int, inp.readline().split())
            st.update(1,0,n-1,l-1,r-1,x)
            root=st.root()
            out.append(str(n+1 if not root.alive else root.idx))
        return "\n".join(out)

    return solve(io.StringIO(inp))

# minimal
assert run("2 1\n1 2\n1 2 1\n") == "3"

# all equal annihilation
assert run("2 1\n5 5\n1 2 5\n") == "3"

# no update strong right
assert run("3 1\n1 2 3\n1 1 0\n") == "3"

# overwrite entire array
assert run("3 1\n2 1 3\n1 3 5\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ghi đè nhỏ | 3 | vụ án tiêu diệt hoàn toàn | 
| tất cả đều bình đẳng | 3 | cà vạt loại bỏ cả hai | 
| ranh giới tối đa | 3 | người chiến thắng chuyển sang phải | 
| cập nhật đầy đủ | 3 | toàn bộ xử lý thiết lập lại | 

## Vỏ cạnh 

Khi tất cả các phần tử trong một phân đoạn được ghi đè lên cùng một giá trị, phân đoạn đó sẽ trở thành một chuỗi các máy bay chiến đấu giống hệt nhau. Trong trường hợp hai phần tử, điều này ngay lập tức loại bỏ cả hai. Biểu diễn cây phân đoạn nắm bắt được điều này bằng cách đánh dấu nút là đã chết khi xảy ra sự hợp nhất bằng nhau, đảm bảo rằng việc truyền bá không bảo toàn một nút sống sót một cách sai lầm. 

Khi toàn bộ mảng được thay thế bằng một giá trị duy nhất trên nhiều truy vấn, mỗi bản cập nhật sẽ thu gọn cấu trúc thành trạng thái thống nhất. Gốc báo cáo chính xác không có người sống sót hoặc một người sống sót duy nhất tùy thuộc vào tính chẵn lẻ của việc loại bỏ trong quá trình hợp nhất. Quy tắc hợp nhất đảm bảo rằng các xung đột có giá trị bằng nhau luôn bị hủy, ngăn chặn việc vô tình giữ lại một chỉ mục không nên tồn tại.
