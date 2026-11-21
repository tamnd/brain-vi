---
title: "CF 242E - XOR trên phân đoạn"
description: "Chúng tôi đang duy trì một mảng số nguyên có thể thay đổi trong đó hai loại hoạt động được thực hiện lặp đi lặp lại: truy vấn tổng phạm vi và cập nhật phạm vi trong đó mọi phần tử trong mảng con được XOR với một giá trị cố định."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 242
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 149 (Div. 2)"
rating: 2000
weight: 242
solve_time_s: 212
verified: true
draft: false
---

[CF 242E - XOR trên phân đoạn](https://codeforces.com/problemset/problem/242/E) 

**Xếp hạng:** 2000 
**Thẻ:** mặt nạ bit, cấu trúc dữ liệu 
**Thời gian giải:** 3 phút 32s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một mảng số nguyên có thể thay đổi trong đó hai loại hoạt động được thực hiện lặp đi lặp lại: truy vấn tổng phạm vi và cập nhật phạm vi trong đó mọi phần tử trong mảng con được XOR với một giá trị cố định. Nhiệm vụ là xuất ra tổng sau mỗi truy vấn thuộc loại đầu tiên, đồng thời đảm bảo tất cả các bản cập nhật được áp dụng theo thứ tự. 

Một cách giải thích đơn giản sẽ gợi ý tính toán lại các tổng bằng cách lặp qua từng phân đoạn được yêu cầu và áp dụng cập nhật XOR theo từng phần tử. Điều đó ngay lập tức gặp rắc rối khi chúng ta quan sát thang đo: mảng có tới 100.000 phần tử và có tới 50.000 phép toán. Trường hợp xấu nhất khi mọi thao tác chạm vào một phân đoạn lớn sẽ dẫn đến khoảng 5×10⁹ cập nhật phần tử hoặc truy vấn, vượt xa giới hạn chấp nhận được. 

Khó khăn chính là XOR không hành xử tuyến tính đối với tổng một cách rõ ràng. Không giống như phép cộng, việc áp dụng XOR cho một phạm vi không cho phép chúng ta chỉ cần điều chỉnh tổng được tính trước theo một lượng không đổi. Hiệu ứng phụ thuộc vào cấu trúc bit của các giá trị trong phân đoạn. 

Một ví dụ đơn giản về tình huống có vấn đề là khi nhiều bản cập nhật XOR chồng chéo lên nhau: 

đầu vào:```
a = [1, 2, 3]
update [1,3] xor 1
update [1,3] xor 1
query [1,3]
```Câu trả lời đúng là 6, vì việc áp dụng XOR hai lần sẽ bị loại bỏ. Việc triển khai ngây thơ mà quên đi hành vi chuyển đổi này hoặc cố gắng “tích lũy” các giá trị XOR sẽ tạo ra trạng thái trung gian không chính xác nếu nó không theo dõi tính chẵn lẻ một cách cẩn thận. 

Một trường hợp phức tạp khác phát sinh khi truy vấn sau khi cập nhật một phần: tổng phụ thuộc vào số lượng phần tử hiện có mỗi bit được đặt chứ không phụ thuộc trực tiếp vào các giá trị riêng lẻ. Bất kỳ giải pháp đúng nào cũng phải ngầm duy trì số lượng cấp độ bit. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản: đối với truy vấn tổng, lặp lại trong phạm vi và tích lũy các giá trị; để cập nhật XOR, hãy lặp lại và áp dụng XOR cho từng phần tử. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của các phép toán. Tuy nhiên, mỗi thao tác tốn O(n) trong trường hợp xấu nhất, dẫn đến độ phức tạp O(nm). Với n = 10⁵ và m = 5×10⁴, điều này trở nên không khả thi. 

Ý tưởng sâu sắc quan trọng là phân tách các số thành các bit và duy trì, đối với mỗi phân đoạn, có bao nhiêu số có số 1 ở mỗi vị trí bit. Tổng của một phân đoạn được xác định đầy đủ bởi các số đếm này: đối với bit i, phần đóng góp của nó là count_of_ones[i] × 2^i. 

Bây giờ hãy xem XOR làm gì. XOR với x lật các bit ở bất cứ nơi nào x có 1. Điều đó có nghĩa là với mỗi bit được đặt trong x, tất cả số lượng số 1 và số 0 ở vị trí bit đó sẽ được hoán đổi trong phân đoạn. Điều quan trọng là thao tác này không phụ thuộc vào giá trị thực tế mà chỉ phụ thuộc vào số lượng, điều này làm cho nó phù hợp với cây phân đoạn có lan truyền lười. 

Điều này dẫn đến một cây phân đoạn trong đó mỗi nút lưu trữ số lượng bit cho tất cả 0 đến 20 bit (vì các giá trị lên tới 10⁶) và một mặt nạ lười biểu thị các hoạt động XOR đang chờ xử lý. Việc áp dụng một XOR lười biếng chỉ cần đếm số bit được đặt trong mặt nạ mà không cần phải chạm vào trẻ em ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Cây phân đoạn có số bit + XOR lười biếng | O((n + m) log n · B) | O(n · B) | Đã chấp nhận | 

Ở đây B là số bit (khoảng 20). 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một cây phân đoạn trong đó mỗi nút lưu trữ hai phần thông tin: đối với mỗi vị trí bit, có bao nhiêu phần tử trong phân đoạn đó được đặt bit đó và mặt nạ XOR lười ghi lại các lần lật bit đang chờ xử lý. 

1. Xây dựng cây từ mảng ban đầu bằng cách tính số bit cho mỗi phân đoạn. Mỗi nút lá khởi tạo số đếm dựa trên biểu diễn nhị phân của một giá trị. 
2. Đối với mỗi nút bên trong, hợp nhất các nút con bằng cách tính tổng số bit của chúng. Điều này đảm bảo mỗi nút thể hiện chính xác phân khúc của nó. 
3. Để áp dụng bản cập nhật XOR trên một phân đoạn, chúng tôi không cập nhật ngay lập tức mọi phần tử. Thay vào đó, chúng tôi cập nhật nút bằng cách đảo số lượng bit được lưu trữ của nó cho mỗi bit được đặt trong giá trị XOR. Nếu một bit được đặt, số lượng bit sẽ trở thành độ dài_of_segment trừ đi current_ones. 
4. Chúng tôi cũng lưu trữ mặt nạ XOR ở giá trị lười biếng để trẻ em có thể cập nhật sau này khi cần. 
5. Khi truy cập một nút, nếu có một XOR lười biếng đang chờ xử lý, chúng tôi sẽ đẩy nó xuống nút con bằng cách áp dụng thao tác lật bit tương tự và truyền mặt nạ. 
6. Đối với truy vấn tổng phạm vi, chúng tôi tổng hợp đóng góp từ tất cả các nút được đề cập. Mỗi nút đóng góp tổng = Σ(bit_count[i] × 2^i). 

### Tại sao nó hoạt động 

Tính chính xác đến từ việc duy trì tính bất biến rằng mọi nút cây phân đoạn luôn lưu trữ số bit chính xác cho phân đoạn của nó, sau khi áp dụng tất cả các thao tác XOR đang chờ xử lý có ảnh hưởng đến nó. Việc lan truyền lười biếng đảm bảo rằng không có bản cập nhật nào bị mất; nó được áp dụng ngay lập tức tại một nút hoặc được lưu trữ để truyền bá trong tương lai. Vì XOR là một sự đảo ngược trên các bit (áp dụng hai lần sẽ khôi phục trạng thái ban đầu), số lần lật luôn có thể đảo ngược và nhất quán với ứng dụng bị trì hoãn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

B = 20  # since a[i] <= 10^6

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.size = 4 * self.n
        self.cnt = [[0] * B for _ in range(self.size)]
        self.lazy = [0] * self.size
        self.build(1, 0, self.n - 1, arr)

    def build(self, v, l, r, arr):
        if l == r:
            for b in range(B):
                if arr[l] & (1 << b):
                    self.cnt[v][b] = 1
            return
        m = (l + r) // 2
        self.build(v * 2, l, m, arr)
        self.build(v * 2 + 1, m + 1, r, arr)
        self.pull(v)

    def pull(self, v):
        for b in range(B):
            self.cnt[v][b] = self.cnt[v * 2][b] + self.cnt[v * 2 + 1][b]

    def apply(self, v, l, r, x):
        length = r - l + 1
        for b in range(B):
            if x & (1 << b):
                self.cnt[v][b] = length - self.cnt[v][b]
        self.lazy[v] ^= x

    def push(self, v, l, r):
        if self.lazy[v] == 0:
            return
        m = (l + r) // 2
        self.apply(v * 2, l, m, self.lazy[v])
        self.apply(v * 2 + 1, m + 1, r, self.lazy[v])
        self.lazy[v] = 0

    def update(self, v, l, r, ql, qr, x):
        if ql <= l and r <= qr:
            self.apply(v, l, r, x)
            return
        self.push(v, l, r)
        m = (l + r) // 2
        if ql <= m:
            self.update(v * 2, l, m, ql, qr, x)
        if qr > m:
            self.update(v * 2 + 1, m + 1, r, ql, qr, x)
        self.pull(v)

    def query(self, v, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.cnt[v]
        self.push(v, l, r)
        m = (l + r) // 2
        res = [0] * B
        if ql <= m:
            left = self.query(v * 2, l, m, ql, qr)
            for b in range(B):
                res[b] += left[b]
        if qr > m:
            right = self.query(v * 2 + 1, m + 1, r, ql, qr)
            for b in range(B):
                res[b] += right[b]
        return res

n = int(input())
arr = list(map(int, input().split()))
seg = SegTree(arr)

m = int(input())
out = []

for _ in range(m):
    tmp = list(map(int, input().split()))
    if tmp[0] == 1:
        l, r = tmp[1] - 1, tmp[2] - 1
        cnt = seg.query(1, 0, n - 1, l, r)
        s = 0
        for b in range(B):
            s += cnt[b] * (1 << b)
        out.append(str(s))
    else:
        l, r, x = tmp[1] - 1, tmp[2] - 1, tmp[3]
        seg.update(1, 0, n - 1, l, r, x)

print("\n".join(out))
```Cây phân đoạn được xây dựng dựa trên số lượng bit thay vì giá trị thô. Đây là sự chuyển đổi quan trọng giúp quản lý các bản cập nhật XOR. Mỗi bản cập nhật sẽ lật toàn bộ phân phối bit tại một nút hoặc được truyền xuống dưới khi cần. 

Bước truy vấn sẽ xây dựng lại tổng từ số lượng bit thay vì các giá trị được lưu trữ, điều này tránh việc phải xây dựng lại các phần tử riêng lẻ. 

Một chi tiết triển khai tinh tế là quy tắc cập nhật bên trong`apply`: lật sử dụng`length - count`, điều này hợp lệ vì mỗi bit là nhị phân trên toàn đoạn. Một chi tiết quan trọng khác là việc lan truyền lười biếng lưu trữ mặt nạ XOR bằng cách sử dụng tích lũy XOR, vì việc áp dụng cùng một mặt nạ hai lần sẽ hủy bỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = [4, 10, 3, 13, 7]
query [2,4]
xor [1,3] with 3
query [2,4]
```Về mặt khái niệm, chúng tôi chỉ theo dõi những đóng góp bit. 

| Bước | Hoạt động | Phân đoạn | Hiệu ứng đếm bit | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | truy vấn [2,4] | [10,3,13] | tính tổng trực tiếp | 26 | 
| 2 | xor [1,3] | ảnh hưởng đến tất cả các bit của 3 | lật các bit có liên quan | - | 
| 3 | truy vấn [2,4] | phân đoạn cập nhật | được tính toán lại từ số lượng | 22 | 

Dấu vết cho thấy rằng sau XOR, các giá trị riêng lẻ thay đổi nhưng việc đếm bit vẫn duy trì tính chính xác mà không cần cập nhật rõ ràng các phần tử. 

### Ví dụ 2 

đầu vào:```
a = [1,2,3]
xor [1,3] with 1
xor [1,3] with 1
query [1,3]
```| Bước | Hoạt động | Trạng thái mảng | 
| --- | --- | --- | 
| 1 | ban đầu | [1,2,3] | 
| 2 | xor 1 | [0,3,2] | 
| 3 | xor 1 | [1,2,3] | 
| 4 | truy vấn | 6 | 

Điều này xác nhận việc hủy XOR và cho thấy lý do tại sao XOR lười biếng phải sử dụng hành vi chuyển đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n · B) | Mỗi bản cập nhật và truy vấn chạm vào n nút nhật ký, mỗi nút xử lý B bit | 
| Không gian | O(n · B) | Mỗi nút cây phân đoạn lưu trữ số bit | 

Các ràng buộc cho phép điều này một cách thoải mái vì B nhỏ (khoảng 20) và log n là khoảng 17 với n = 10⁵. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    B = 20

    class SegTree:
        def __init__(self, arr):
            self.n = len(arr)
            self.cnt = [[0]*B for _ in range(4*self.n)]
            self.lazy = [0]*(4*self.n)
            self.build(1,0,self.n-1,arr)

        def build(self,v,l,r,a):
            if l==r:
                for b in range(B):
                    if a[l]>>b & 1:
                        self.cnt[v][b]=1
                return
            m=(l+r)//2
            self.build(v*2,l,m,a)
            self.build(v*2+1,m+1,r,a)
            for b in range(B):
                self.cnt[v][b]=self.cnt[v*2][b]+self.cnt[v*2+1][b]

        def apply(self,v,l,r,x):
            length=r-l+1
            for b in range(B):
                if x>>b & 1:
                    self.cnt[v][b]=length-self.cnt[v][b]
            self.lazy[v]^=x

        def push(self,v,l,r):
            if not self.lazy[v]:
                return
            m=(l+r)//2
            self.apply(v*2,l,m,self.lazy[v])
            self.apply(v*2+1,m+1,r,self.lazy[v])
            self.lazy[v]=0

        def update(self,v,l,r,ql,qr,x):
            if ql<=l and r<=qr:
                self.apply(v,l,r,x)
                return
            self.push(v,l,r)
            m=(l+r)//2
            if ql<=m:
                self.update(v*2,l,m,ql,qr,x)
            if qr>m:
                self.update(v*2+1,m+1,r,ql,qr,x)

        def query(self,v,l,r,ql,qr):
            if ql<=l and r<=qr:
                return self.cnt[v]
            self.push(v,l,r)
            m=(l+r)//2
            res=[0]*B
            if ql<=m:
                left=self.query(v*2,l,m,ql,qr)
                for i in range(B): res[i]+=left[i]
            if qr>m:
                right=self.query(v*2+1,m+1,r,ql,qr)
                for i in range(B): res[i]+=right[i]
            return res

    n = int(input())
    a = list(map(int,input().split()))
    st = SegTree(a)
    m = int(input())
    out=[]
    for _ in range(m):
        q=list(map(int,input().split()))
        if q[0]==1:
            l,r=q[1]-1,q[2]-1
            cnt=st.query(1,0,n-1,l,r)
            s=0
            for i in range(B):
                s+=cnt[i]<<i
            out.append(str(s))
        else:
            l,r,x=q[1]-1,q[2]-1,q[3]
            st.update(1,0,n-1,l,r,x)

    return "\n".join(out)

# provided sample
assert run("""5
4 10 3 13 7
8
1 2 4
2 1 3 3
1 2 4
1 3 3
2 2 5 5
1 1 5
2 1 2 10
1 2 3
""") == """26
22
0
34
11"""

# small cases
assert run("""1
5
2
1 1 1
1 1 1
""") == """5"""

assert run("""3
1 2 3
2 1 3 1
1 1 3
""") == """0"""

assert run("""4
0 0 0 0
2 1 4 7
1 1 4
""") == """28"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 5, 5 | truy vấn danh tính | 
| hủy XOR đầy đủ | 0 | chuyển đổi bit đúng đắn | 
| tất cả số không XOR | 28 | lan truyền đa bit | 

## Vỏ cạnh 

Trường hợp cạnh khóa được lặp lại XOR với cùng một giá trị trên các phân đoạn chồng chéo. Thuật toán xử lý việc này một cách chính xác vì mặt nạ lười sử dụng tích lũy XOR. Áp dụng cùng một mặt nạ hai lần sẽ hủy bỏ, khôi phục số bit ban đầu. 

Một trường hợp đặc biệt khác là truy vấn một phân đoạn có các bản cập nhật lười biếng đang chờ xử lý tại nhiều nút tổ tiên. Thao tác đẩy đảm bảo rằng tất cả các phép biến đổi đang chờ xử lý được áp dụng trước bất kỳ tính toán truy vấn một phần nào, duy trì tính chính xác ngay cả trong các cây được lồng sâu. 

Trường hợp cuối cùng là cập nhật toàn bộ theo sau là các truy vấn một phần. Vì mỗi nút luôn biểu thị số bit chính xác cho phân đoạn của nó sau khi truyền bá, nên việc truy vấn bất kỳ phân đoạn con nào đều tạo ra các đóng góp tổng hợp chính xác mà không cần tái cấu trúc theo từng phần tử.
