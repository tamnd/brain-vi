---
title: "CF 102961D - Vé hòa nhạc"
description: "Nhiệm vụ xoay quanh một thị trường bán vé buổi hòa nhạc có giá cố định và một chuỗi người mua lần lượt đến. Mỗi vé có một mức giá và mỗi người mua có số tiền tối đa mà họ sẵn sàng trả."
date: "2026-07-04T06:50:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102961
codeforces_index: "D"
codeforces_contest_name: "CSES Problem Set: Sorting and Searching"
rating: 0
weight: 102961
solve_time_s: 51
verified: true
draft: false
---

[CF 102961D - Vé buổi hòa nhạc](https://codeforces.com/problemset/problem/102961/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ xoay quanh một thị trường bán vé buổi hòa nhạc có giá cố định và một chuỗi người mua lần lượt đến. Mỗi vé có một mức giá và mỗi người mua có số tiền tối đa mà họ sẵn sàng trả. Khi người mua đến, họ chọn một vé duy nhất có giá không vượt quá ngân sách của họ. Trong số tất cả các vé có sẵn như vậy, họ luôn lấy chiếc đắt nhất mà họ có thể mua được. Sau khi vé được bán, nó sẽ biến mất và không thể sử dụng lại. 

Đầu vào mô tả nhiều tập hợp giá vé ban đầu, theo sau là danh sách người mua, mỗi người có một ràng buộc về ngân sách. Đối với mỗi người mua, chúng tôi phải xác định xem họ sẽ mua vé nào hoặc báo cáo rằng không có vé phù hợp. 

Cấu trúc ngay lập tức gợi ý rằng cả thứ tự chèn và xóa đều quan trọng. Thao tác chính không chỉ là truy vấn xem phiếu có tồn tại dưới ngưỡng hay không mà còn liên tục tìm và loại bỏ ứng cử viên khả thi nhất trong một tập động. 

Nếu số lượng vé và người mua ở mức 200.000 hoặc tương tự, thì bất kỳ phương pháp nào quét toàn bộ danh sách vé cho từng người mua đều dẫn đến hành vi bậc hai. Một lần quét tuyến tính đơn giản cho mỗi truy vấn sẽ yêu cầu kiểm tra tối đa tất cả các vé còn lại, tạo ra khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn khả thi trong khoảng thời gian thực thi 2 giây. 

Điều này đã loại trừ các lần quét toàn bộ lặp đi lặp lại hoặc sắp xếp đơn giản bằng tính năng lọc theo truy vấn. 

Một số trường hợp khó khăn xuất hiện một cách tự nhiên. 

Một là khi tất cả các vé đều đắt hơn ngân sách của mọi người mua. Ví dụ, vé là`[100, 200, 300]`và người mua là`[10, 50]`. Đầu ra đúng là`-1 -1`. Việc triển khai bất cẩn không xử lý đúng cách trường hợp "không tìm thấy ứng viên" có thể trả lại sai vé nhỏ nhất hoặc sử dụng lại câu trả lời trước đó. 

Một trường hợp khác là khi nhiều vé có cùng mức giá. Ví dụ như vé`[50, 50, 50]`và người mua`[50, 50]`. Mỗi người mua sẽ nhận được một vé riêng biệt. Giải pháp không loại bỏ chính xác các phiếu đã sử dụng khỏi cấu trúc của nó có thể liên tục gán cùng một phiếu logic nhiều lần. 

Trường hợp tinh vi thứ ba là khi người mua đến theo thứ tự ngân sách giảm dần, điều này có thể đánh lừa các giải pháp dựa trên danh sách được sắp xếp mà không có hỗ trợ loại bỏ thích hợp. Ví dụ như vé`[20, 40, 60]`và người mua`[70, 50, 30]`yêu cầu các kết quả khớp nhỏ hơn dần dần với các cập nhật trạng thái sau mỗi lần phân công. 

## Phương pháp tiếp cận 

Giải pháp bạo lực trực tiếp duy trì danh sách các vé còn lại. Đối với mỗi người mua, chúng tôi lặp lại tất cả các vé, kiểm tra những vé không vượt quá ngân sách và chọn mức tối đa trong số đó. Sau khi chọn vé, chúng tôi xóa nó khỏi danh sách. 

Tính năng này hoạt động vì nó mô phỏng quy trình chính xác như được mô tả, duy trì tính chính xác trong quá trình xây dựng. Tuy nhiên, chi phí bị chi phối bởi việc quét toàn bộ bộ vé cho mỗi người mua. Với`n`vé và`m`người mua, điều này dẫn đến`O(nm)`sự phức tạp về mặt thời gian. Khi cả hai đều lớn, điều này trở nên khó quản lý. 

Sự kém hiệu quả xuất phát từ việc liên tục tìm kiếm ứng viên tốt nhất từ ​​đầu, mặc dù bộ vé chỉ thay đổi dần dần thông qua việc xóa. Cấu trúc chúng ta cần là cấu trúc hỗ trợ hai thao tác một cách hiệu quả: tìm phần tử lớn nhất không vượt quá giá trị và loại bỏ nó. 

Đây chính xác là những gì một cấu trúc cân bằng có trật tự mang lại. Nếu chúng tôi sắp xếp vé và duy trì chúng theo cấu trúc hỗ trợ các truy vấn trước đó, mỗi người mua có thể được phục vụ bằng cách định vị giá vé ngoài cùng bên phải`<= budget`. Sau đó, chúng tôi xóa nó. Cây tìm kiếm nhị phân cân bằng hoặc nhiều tập hợp có thứ tự hỗ trợ cả hai thao tác theo thời gian logarit. 

Trong Python, thư viện chuẩn không cung cấp nhiều tập hợp dựa trên cây, vì vậy chúng tôi mô phỏng nó bằng cách sử dụng các vùng chứa được sắp xếp hoặc phổ biến hơn trong lập trình cạnh tranh, sử dụng`bisect`trên danh sách được sắp xếp kết hợp với cây Fenwick hoặc cây phân đoạn. Giải pháp khái niệm rõ ràng nhất là cây phân đoạn trên tọa độ nén, trong đó mỗi nút lưu trữ chỉ mục hoặc số lượng tối đa có sẵn, cho phép chúng tôi truy vấn vé khả thi nhất và loại bỏ nó. 

Quan sát quan trọng là giá không đổi trong phạm vi nhưng tình trạng còn hàng thay đổi linh hoạt và chúng tôi chỉ cần hỗ trợ truy vấn “tốt nhất <= x” khi xóa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Cây phân đoạn / Cấu trúc theo thứ tự | O((n + m) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách nén giá vé và xây dựng cấu trúc theo dõi số lượng vé còn lại ở mỗi mức giá. 

1. Chúng tôi bắt đầu bằng cách sắp xếp tất cả giá vé và nén chúng thành một mảng duy nhất được sắp xếp. Điều này cho phép chúng ta làm việc với các chỉ số thay vì các giá trị thô. Lý do nén hữu ích là vì chúng ta chỉ quan tâm đến thứ tự tương đối chứ không phải độ lớn thực tế. 
2. Chúng tôi xây dựng một mảng tần số trên các chỉ số nén này, lưu trữ số lượng vé tồn tại ở mỗi mức giá. Điều này ghi lại các bản sao một cách tự nhiên. 
3. Chúng tôi xây dựng một cây phân đoạn trên mảng tần số này, trong đó mỗi nút lưu trữ tổng số vé có sẵn trong phân khúc của nó. Điều này cho phép chúng tôi nhanh chóng xác định xem có vé nào tồn tại trong một phạm vi hay không. 
4. Đối với mỗi người mua có ngân sách`x`, chúng ta tìm kiếm nhị phân trong mảng nén để tìm chỉ số lớn nhất có giá là`<= x`. Điều này làm giảm không gian tìm kiếm chỉ còn những ứng viên hợp lệ. 
5. Chúng ta truy vấn cây phân đoạn trong phạm vi`[0, idx]`để tìm vị trí ngoài cùng bên phải nơi vẫn còn vé. Bước này xác định vé giá cả phải chăng nhất. 
6. Nếu không có vị trí như vậy tồn tại, chúng tôi xuất ra`-1`. Ngược lại, chúng ta xuất giá vé tương ứng, giảm số lượng và cập nhật cây phân đoạn. 

Lý do điều này hoạt động hiệu quả là vì cả việc tìm kiếm ranh giới giá hợp lệ và tìm kiếm vé có sẵn bên trong ranh giới đó đều là các phép toán logarit và mỗi vé được xóa đúng một lần. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, cây phân đoạn thể hiện chính xác nhiều tập phiếu còn lại. Truy vấn luôn chọn chỉ mục tối đa có số lượng dương, tương ứng chính xác với vé đắt nhất hiện có nhưng không vượt quá ngân sách của người mua. Vì các bản cập nhật chỉ xóa vé nên cấu trúc sẽ co lại một cách đơn điệu, duy trì tính chính xác của tất cả các truy vấn trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.t = [0] * (4 * self.n)
        self.arr = arr
        self._build(1, 0, self.n - 1)

    def _build(self, v, l, r):
        if l == r:
            self.t[v] = 1
            return
        m = (l + r) // 2
        self._build(v*2, l, m)
        self._build(v*2+1, m+1, r)
        self.t[v] = self.t[v*2] + self.t[v*2+1]

    def _query(self, v, l, r, ql, qr):
        if ql > r or qr < l:
            return -1
        if l == r:
            return l if self.t[v] > 0 else -1
        if ql <= l and r <= qr:
            if self.t[v] == 0:
                return -1
            m = (l + r) // 2
            right = self._query(v*2+1, m+1, r, ql, qr)
            if right != -1:
                return right
            return self._query(v*2, l, m, ql, qr)

        m = (l + r) // 2
        right = self._query(v*2+1, m+1, r, ql, qr)
        left = self._query(v*2, l, m, ql, qr)
        return max(right, left)

    def update(self, v, l, r, idx):
        if l == r:
            self.t[v] -= 1
            return
        m = (l + r) // 2
        if idx <= m:
            self.update(v*2, l, m, idx)
        else:
            self.update(v*2+1, m+1, r, idx)
        self.t[v] = self.t[v*2] + self.t[v*2+1]

n, m = map(int, input().split())
tickets = list(map(int, input().split()))
buyers = list(map(int, input().split()))

vals = sorted(set(tickets))
idx = {v:i for i, v in enumerate(vals)}

freq = [0] * len(vals)
for t in tickets:
    freq[idx[t]] += 1

seg = SegTree(vals)
seg.t = seg.t  # structure initialized over presence; we adjust via updates

# rebuild tree properly with freq
def build(v, l, r):
    if l == r:
        seg.t[v] = freq[l]
        return
    m = (l + r) // 2
    build(v*2, l, m)
    build(v*2+1, m+1, r)
    seg.t[v] = seg.t[v*2] + seg.t[v*2+1]

build(1, 0, len(vals)-1)

def query_rightmost(v, l, r, ql, qr):
    if ql > r or qr < l or seg.t[v] == 0:
        return -1
    if l == r:
        return l
    m = (l + r) // 2
    res = query_rightmost(v*2+1, m+1, r, ql, qr)
    if res != -1:
        return res
    return query_rightmost(v*2, l, m, ql, qr)

out = []

for b in buyers:
    pos = bisect = None
    # binary search manually
    lo, hi = 0, len(vals) - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if vals[mid] <= b:
            pos = mid
            lo = mid + 1
        else:
            hi = mid - 1

    if pos is None:
        out.append("-1")
        continue

    res = query_rightmost(1, 0, len(vals)-1, 0, pos)
    if res == -1:
        out.append("-1")
    else:
        out.append(str(vals[res]))
        # remove one ticket
        def upd(v, l, r, idx):
            if l == r:
                seg.t[v] -= 1
                return
            m = (l + r) // 2
            if idx <= m:
                upd(v*2, l, m, idx)
            else:
                upd(v*2+1, m+1, r, idx)
            seg.t[v] = seg.t[v*2] + seg.t[v*2+1]

        upd(1, 0, len(vals)-1, res)

print("\n".join(out))
```Giải pháp trước tiên nén giá sao cho các chỉ số của cây phân đoạn tương ứng với các giá trị vé được sắp xếp. Bước tìm kiếm nhị phân tìm chỉ số giá phải chăng cao nhất và truy vấn cây phân khúc tìm thấy vé tốt nhất vẫn còn trống trong phạm vi đó. Sau khi xuất vé, chúng tôi giảm số lượng của nó thông qua cập nhật điểm để không thể sử dụng lại. 

Một điểm tinh tế là cả truy vấn tìm kiếm nhị phân và cây phân đoạn đều cần thiết. Tìm kiếm nhị phân giới hạn miền ở mức giá phải chăng, trong khi cây phân đoạn thực thi tính khả dụng khi xóa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Vé là`[5, 3, 7]`, người mua là`[4, 8, 3]`. 

| Người mua | Ngân sách | Chỉ số giá cả phải chăng tối đa | Vé đã chọn | Còn lại nhiều bộ | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 3 (giá trị 5 quá lớn nên chỉ số là 3) | 3 | [5, 7] | 
| 2 | 8 | 2 | 7 | [5] | 
| 3 | 3 | 1 | -1 | [5] | 

Dấu vết này cho thấy cách cấu trúc luôn chọn vé có sẵn tốt nhất trong tập hợp đã giảm hiện tại. 

### Ví dụ 2 

Vé là`[10, 10, 20]`, người mua là`[10, 10, 10]`. 

| Người mua | Ngân sách | Có sẵn tốt nhất | Vé đã chọn | Còn lại nhiều bộ | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 10 | 10 | [10, 20] | 
| 2 | 10 | 10 | 10 | [20] | 
| 3 | 10 | -1 | -1 | [20] | 

Điều này thể hiện việc xử lý chính xác các bản sao, trong đó các mức giá giống nhau lặp đi lặp lại được sử dụng từng cái một. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | mỗi người mua thực hiện tìm kiếm nhị phân cộng với truy vấn và cập nhật cây phân đoạn | 
| Không gian | O(n) | lưu trữ cây phân đoạn và mảng nén | 

Hệ số logarit đảm bảo khả năng mở rộng cho hàng trăm nghìn vé và truy vấn, phù hợp thoải mái trong giới hạn cuộc thi thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    tickets = list(map(int, input().split()))
    buyers = list(map(int, input().split()))

    vals = sorted(set(tickets))
    idx = {v:i for i, v in enumerate(vals)}

    freq = [0] * len(vals)
    for t in tickets:
        freq[idx[t]] += 1

    class Seg:
        def __init__(self):
            self.n = len(vals)
            self.t = [0] * (4*self.n)

        def build(self, v, l, r):
            if l == r:
                self.t[v] = freq[l]
                return
            m = (l+r)//2
            self.build(v*2,l,m)
            self.build(v*2+1,m+1,r)
            self.t[v]=self.t[v*2]+self.t[v*2+1]

        def query(self,v,l,r,ql,qr):
            if ql>r or qr<l or self.t[v]==0:
                return -1
            if l==r:
                return l
            m=(l+r)//2
            res=self.query(v*2+1,m+1,r,ql,qr)
            if res!=-1:
                return res
            return self.query(v*2,l,m,ql,qr)

        def upd(self,v,l,r,i):
            if l==r:
                self.t[v]-=1
                return
            m=(l+r)//2
            if i<=m:
                self.upd(v*2,l,m,i)
            else:
                self.upd(v*2+1,m+1,r,i)
            self.t[v]=self.t[v*2]+self.t[v*2+1]

    seg = Seg()
    seg.build(1,0,len(vals)-1)

    out=[]
    for b in buyers:
        pos=None
        lo,hi=0,len(vals)-1
        while lo<=hi:
            mid=(lo+hi)//2
            if vals[mid]<=b:
                pos=mid
                lo=mid+1
            else:
                hi=mid-1
        if pos is None:
            out.append("-1")
            continue
        res=seg.query(1,0,len(vals)-1,0,pos)
        if res==-1:
            out.append("-1")
        else:
            out.append(str(vals[res]))
            seg.upd(1,0,len(vals)-1,res)

    return "\n".join(out)

# provided samples
assert run("3 3\n5 3 7\n4 8 3\n") == "3\n7\n-1"

# custom cases
assert run("1 1\n10\n9\n") == "-1", "below all tickets"
assert run("1 2\n5\n5 5\n") == "5\n-1", "single ticket exhaustion"
assert run("4 4\n1 1 1 1\n1 1 1 1\n") == "1\n1\n1\n1", "all equal consumption"
assert run("3 3\n10 20 30\n5 15 25\n") == "-1\n10\n20", "boundary stepping"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| vé đơn dưới ngân sách | -1 | không có cách xử lý lựa chọn hợp lệ | 
| lặp lại cùng một vé | 5, -1 | sự suy giảm đúng đắn | 
| tất cả các giá trị bằng nhau | kết quả đầu ra lặp đi lặp lại | đếm nhiều bộ | 
| tăng ngưỡng | lựa chọn từng bước | khớp tham lam đúng | 

## Vỏ cạnh 

Khi tất cả các vé đều vượt quá ngân sách của mọi người mua, các truy vấn cây phân đoạn luôn trả về trạng thái trống sau khi tìm kiếm nhị phân thu hẹp phạm vi thành tiền tố hợp lệ. Đối với đầu vào như`tickets = [100, 200]`Và`buyers = [10, 20]`, mọi phạm vi truy vấn đều hợp lệ nhưng cây phân đoạn báo cáo tính khả dụng bằng 0, dẫn đến`-1`đầu ra một cách nhất quán. Cấu trúc không bao giờ cố gắng truy cập một chỉ mục không hợp lệ vì giới hạn tìm kiếm nhị phân đảm bảo phạm vi truy vấn luôn được xác định rõ. 

Khi tồn tại nhiều vé giống hệt nhau, mỗi bản cập nhật sẽ giảm tần suất ở một lá. Trong trường hợp như`tickets = [50, 50]`, truy vấn đầu tiên sẽ tìm thấy chỉ mục 0 hoặc 1 tùy thuộc vào độ nén và sau khi cập nhật, chỉ còn lại một chỉ mục. Truy vấn thứ hai vẫn tìm kiếm trong phạm vi tương tự nhưng bây giờ trả về chính xác lần xuất hiện còn lại trước khi sử dụng hết.
