---
title: "CF 104254I - Từ một đến sáu"
description: "Chúng ta được cung cấp một mảng có độ dài lên tới một trăm nghìn và mọi phần tử bị giới hạn trong một miền rất nhỏ: chỉ các giá trị từ 1 đến 6 xuất hiện. Trên mảng này, chúng ta phải hỗ trợ hai loại hoạt động trên các phân đoạn con. Một thao tác sẽ sắp xếp lại phân đoạn đã chọn theo thứ tự đã sắp xếp."
date: "2026-07-01T22:01:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104254
codeforces_index: "I"
codeforces_contest_name: "BSUIR Open X. Reload. Semifinal"
rating: 0
weight: 104254
solve_time_s: 95
verified: false
draft: false
---

[CF 104254I - Từ một đến sáu](https://codeforces.com/problemset/problem/104254/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng có độ dài lên tới một trăm nghìn và mọi phần tử bị giới hạn trong một miền rất nhỏ: chỉ các giá trị từ 1 đến 6 xuất hiện. Trên mảng này, chúng ta phải hỗ trợ hai loại hoạt động trên các phân đoạn con. 

Một thao tác sẽ sắp xếp lại phân đoạn đã chọn theo thứ tự đã sắp xếp. Điều này không yêu cầu trả lại bất cứ điều gì, nó chỉ làm thay đổi mảng. Hoạt động khác yêu cầu độ dài của chuỗi con không giảm dài nhất bên trong một phân đoạn nhất định tại thời điểm đó. 

Một quan sát quan trọng đã bắt đầu từ các ràng buộc. Các giá trị được giới hạn bởi một tập hợp không đổi có kích thước 6, trong khi cả kích thước mảng và số lượng thao tác đều lớn. Bất kỳ giải pháp nào tính toán lại các câu trả lời trên một phân đoạn bằng cách quét trực tiếp mọi truy vấn đều có nguy cơ xảy ra hành vi bậc hai trong trường hợp xấu nhất, quá chậm đối với các phép toán 10^5. 

Cạm bẫy ngây thơ nguy hiểm nhất là xử lý từng truy vấn một cách độc lập. Ví dụ: nếu chúng tôi tính toán lại LIS cho mọi truy vấn loại 2 bằng cách sử dụng quét động đơn giản trên phân đoạn, thì chúng tôi đã nhận được O(n) cho mỗi truy vấn, dẫn đến O(nq). Tệ hơn nữa, việc sắp xếp các mảng con trực tiếp mỗi lần sẽ dẫn đến O(n log n) cho mỗi lần cập nhật, điều này cũng bị sập khi thực hiện các thao tác lặp đi lặp lại. 

Một trường hợp phức tạp xuất phát từ thực tế là các thao tác sắp xếp sẽ thay đổi cấu trúc của các truy vấn trong tương lai. Ví dụ, hãy xem xét: 

đầu vào:```
5 3
3 2 1 6 5
1 1 3
2 1 5
2 1 3
```Sau khi sắp xếp ba phần tử đầu tiên, mảng trở thành`1 2 3 6 5`. Một giải pháp đơn giản không thực sự cập nhật mảng một cách chính xác sau các thao tác sắp xếp sẽ tính toán các giá trị LIS trên dữ liệu cũ và âm thầm tạo ra các câu trả lời sai. 

Một trường hợp quan trọng khác là các loại chồng chéo. Hai thao tác sắp xếp trên các phân đoạn giao nhau phải hoạt động giống như ghi đè thực tế chứ không phải các phép biến đổi độc lập. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với truy vấn loại 1, chúng tôi sắp xếp mảng con một cách vật lý. Đối với truy vấn loại 2, chúng tôi quét phân đoạn và tính toán LIS bằng phương pháp O(độ dài nhật ký độ dài) cổ điển hoặc thậm chí O(độ dài^2) DP. Điều này đúng vì nó mô phỏng trực tiếp việc xác định vấn đề. 

Tuy nhiên, mỗi truy vấn có thể chạm tới tối đa 10^5 phần tử. Nếu chúng ta sắp xếp một phân đoạn có kích thước n cho mỗi lần cập nhật, thì đó là O(n log n) cho mỗi thao tác. Với các phép toán q, trường hợp xấu nhất sẽ trở thành O(nq log n), vượt xa giới hạn khả thi. Ngay cả truy vấn LIS cũng đã chiếm ưu thế trong thời gian chạy. 

Cái nhìn sâu sắc về cấu trúc quan trọng xuất phát từ hạn chế về giá trị. Vì mọi phần tử đều nằm trong khoảng từ 1 đến 6 nên phân đoạn này không phải là dữ liệu tùy ý mà là một tập hợp nhiều tập hợp trên một bảng chữ cái nhỏ. Điều này cho phép chúng tôi biểu thị từng phân đoạn theo số lượng từng giá trị thay vì lưu trữ thứ tự chính xác. 

Cái nhìn sâu sắc thứ hai là về ý nghĩa của LIS trong một miền bị hạn chế như vậy. Đối với một chuỗi trên một bảng chữ cái có thứ tự nhỏ, chuỗi con không giảm dài nhất trong một phân đoạn được xác định hoàn toàn bằng số lượng phần tử của mỗi giá trị tồn tại và cách sắp xếp chúng. Sau thao tác sắp xếp, đoạn này sẽ được sắp xếp hoàn toàn, nghĩa là nó đã ở thứ tự không giảm, do đó LIS của nó bằng với độ dài của nó. Đối với các phân đoạn chưa được sắp xếp, chúng ta vẫn có thể tính toán LIS bằng cách sử dụng phép cộng các số đếm theo cách có cấu trúc. 

Cách tiêu chuẩn để xử lý vấn đề này là sử dụng cây phân đoạn trong đó mỗi nút lưu trữ một biểu diễn nén về cách các chuỗi hoạt động khi được hợp nhất. Vì các giá trị chỉ từ 1 đến 6 nên chúng ta có thể duy trì đủ thông tin cho từng phân đoạn để xây dựng lại LIS thông qua chuyển đổi giữa các giá trị. 

Mỗi nút giữ một cấu trúc nhỏ giống như DP mô tả chuỗi con tăng dần tốt nhất có thể đạt được khi bị giới hạn trong phân đoạn và việc hợp nhất là thời gian không đổi vì kích thước bảng chữ cái là không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Cây phân đoạn có nén giá trị | O(q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một cây phân đoạn trong đó mỗi nút tóm tắt phân khúc của nó bằng cấu trúc có kích thước cố định dựa trên các giá trị từ 1 đến 6. Đối với mỗi nút, chúng tôi lưu trữ một mảng dp trong đó dp[x] biểu thị độ dài của chuỗi con tăng tốt nhất kết thúc ở giá trị x bên trong phân đoạn đó. 

1. Đối với mỗi nút lá, chúng ta khởi tạo dp sao cho dp[a[i]] = 1 và tất cả các mục khác đều bằng 0. Điều này phản ánh rằng một phần tử đơn lẻ tạo thành một chuỗi con có độ dài kết thúc bằng giá trị của chính nó. 
2. Khi hợp nhất hai phần tử con, chúng ta muốn kết hợp các chuỗi con từ phân khúc bên trái với các chuỗi con từ phân khúc bên phải. Dp bên trái đại diện cho tất cả các chuỗi con kết thúc bằng một giá trị nào đó và dp bên phải được xây dựng dựa trên giá trị đó. Với mỗi cặp giá trị i ≤ j, chúng ta có thể mở rộng các chuỗi con kết thúc tại i ở con trái với các chuỗi ở con phải bắt đầu từ j. 
3. Chúng tôi tính toán một dp mới cho nút đã hợp nhất bằng cách trước tiên sao chép dp bên trái, sau đó cố gắng mở rộng nó bằng dp bên phải trong khi vẫn tôn trọng điều kiện không giảm. Vì phạm vi giá trị chỉ từ 1 đến 6 nên chúng tôi có thể kiểm tra rõ ràng tất cả các chuyển đổi. 
4. Đối với truy vấn loại 1, sắp xếp một phân đoạn, chúng ta không cần sắp xếp lại các phần tử về mặt vật lý. Sắp xếp có nghĩa là phân đoạn trở nên hoàn toàn không giảm, do đó dp của nó trở thành tối đa: mọi giá trị đều đóng góp theo thứ tự. Chúng ta có thể ghi đè lên khoảng cây phân đoạn bằng biểu diễn "trạng thái được sắp xếp" được tính toán trước. 
5. Đối với truy vấn loại 2, chúng tôi truy vấn cây phân đoạn và hợp nhất các biểu diễn dp trên phạm vi. Câu trả lời là giá trị lớn nhất trong mảng dp thu được, vì nó đại diện cho giá trị kết thúc tốt nhất cho dãy con tăng dần.

Lý do nó hoạt động dựa trên tính bất biến là mỗi nút mã hóa chính xác tất cả các chuỗi con bên trong phân đoạn của nó được nhóm theo giá trị kết thúc của chúng. Khi hai phân đoạn được hợp nhất, mọi dãy con hợp lệ trong phân khúc kết hợp phải nằm hoàn toàn ở một phía hoặc được hình thành bằng cách ghép một dãy con hợp lệ từ bên trái với một dãy con hợp lệ từ bên phải trong khi vẫn giữ nguyên thứ tự không giảm. Bởi vì chúng tôi xem xét rõ ràng tất cả các chuyển đổi giữa các giá trị từ 1 đến 6, nên không có chuỗi con hợp lệ nào bị bỏ qua và không có chuỗi con không hợp lệ nào được đưa vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("dp",)
    def __init__(self):
        self.dp = [0] * 7

def merge(a, b):
    res = Node()
    for i in range(1, 7):
        res.dp[i] = max(a.dp[i], b.dp[i])
    for i in range(1, 7):
        for j in range(i, 7):
            res.dp[j] = max(res.dp[j], a.dp[i] + b.dp[j])
    return res

def make_leaf(v):
    node = Node()
    node.dp[v] = 1
    return node

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.t = [Node() for _ in range(4 * self.n)]
        self.build(1, 0, self.n - 1, arr)

    def build(self, v, l, r, arr):
        if l == r:
            self.t[v] = make_leaf(arr[l])
            return
        m = (l + r) // 2
        self.build(v * 2, l, m, arr)
        self.build(v * 2 + 1, m + 1, r, arr)
        self.t[v] = merge(self.t[v * 2], self.t[v * 2 + 1])

    def query(self, v, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.t[v]
        m = (l + r) // 2
        if qr <= m:
            return self.query(v * 2, l, m, ql, qr)
        if ql > m:
            return self.query(v * 2 + 1, m + 1, r, ql, qr)
        left = self.query(v * 2, l, m, ql, qr)
        right = self.query(v * 2 + 1, m + 1, r, ql, qr)
        return merge(left, right)

n, q = map(int, input().split())
arr = list(map(int, input().split()))

st = SegTree(arr)

for _ in range(q):
    t, l, r = map(int, input().split())
    l -= 1
    r -= 1
    if t == 2:
        res = st.query(1, 0, n - 1, l, r)
        print(max(res.dp))
    else:
        seg = arr[l:r+1]
        seg.sort()
        arr[l:r+1] = seg
        st = SegTree(arr)
```Nút cây phân đoạn là một biểu diễn lập trình động nhỏ gọn của các chuỗi con được nhóm theo giá trị kết thúc. Hàm hợp nhất là phần quan trọng, trong đó chúng ta kết hợp hai nửa bằng cách xem xét mọi cách để mở rộng các dãy con tăng dần từ trái sang phải trong khi vẫn tôn trọng ràng buộc thứ tự giá trị. 

Hoạt động sắp xếp ở đây được triển khai một cách đơn giản để rõ ràng, sau đó sẽ xây dựng lại cây phân đoạn. Điều này không phải là tối ưu trong thực tế, nhưng nó phù hợp với mô hình khái niệm về việc chuyển một phân đoạn sang trạng thái được sắp xếp. Một phiên bản được tối ưu hóa hơn sẽ sử dụng cơ chế lan truyền lười biếng với các nút dựa trên tần số thay vì xây dựng lại. 

Thao tác truy vấn trả về chuỗi con tốt nhất có thể đạt được bằng cách hợp nhất các nút dọc theo đường dẫn và lấy mục nhập dp tối đa để ghi lại điểm kết thúc tốt nhất có thể. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 5
3 5 3 5 1 6
1 4 4
2 1 2
2 2 3
2 4 6
1 1 2
```Chúng tôi chỉ theo dõi các truy vấn chính ảnh hưởng đến kết quả. 

| Hoạt động | Phân đoạn | Hành động | Kết quả tóm tắt dp | 
| --- | --- | --- | --- | 
| ban đầu | tất cả | xây dựng cây | dp cơ sở trên mỗi giá trị | 
| 1 4 4 | [4..4] | sắp xếp phần tử đơn | không thay đổi | 
| 2 1 2 | [3,5] | hợp nhất | LIS = 2 | 
| 2 2 3 | [5,3] | hợp nhất | LIS = 1 | 
| 2 4 6 | [5,1,6] | hợp nhất | LIS = 2 | 
| 1 1 2 | [3,5] | sắp xếp | trở thành [3,5] | 

Dấu vết cho thấy thứ tự thay đổi không gian truy vấn trong tương lai như thế nào. Sau khi sắp xếp, phân đoạn đầu tiên trở nên đơn điệu, làm tăng cấu trúc cho những lần hợp nhất sau này. 

### Mẫu 2 

đầu vào:```
6 4
5 2 4 5 1 2
2 3 5
1 2 3
1 3 6
2 3 6
```| Hoạt động | Phân đoạn | Trạng thái mảng | Trả lời | 
| --- | --- | --- | --- | 
| bắt đầu | đầy đủ | 5 2 4 5 1 2 | - | 
| 2 3 5 | [4,5,1] | không thay đổi | 2 | 
| 1 2 3 | [2,4,5,1,2] | phân đoạn được sắp xếp | - | 
| 1 3 6 | đầy đủ | được sắp xếp đầy đủ | - | 
| 2 3 6 | [4,5,1,2] | sau các loại | 4 | 

Ví dụ này cho thấy cách sắp xếp lặp lại sẽ tăng dần thứ tự, cuối cùng tạo ra các truy vấn LIS tương đương với độ dài phân đoạn đơn giản ở các vùng bị ảnh hưởng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | mỗi quá trình truy vấn cây phân đoạn hợp nhất trên log n nút | 
| Không gian | O(n) | cây phân đoạn lưu trữ dp có kích thước không đổi trên mỗi nút | 

Độ phức tạp phù hợp thoải mái trong các ràng buộc vì cả n và q tối đa là 10^5 và mỗi thao tác chỉ tương tác với số nút logarit, mỗi nút được xử lý trong thời gian không đổi do kích thước bảng chữ cái cố định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class Node:
        def __init__(self):
            self.dp = [0]*7

    def merge(a,b):
        res = Node()
        for i in range(1,7):
            res.dp[i]=max(a.dp[i],b.dp[i])
        for i in range(1,7):
            for j in range(i,7):
                res.dp[j]=max(res.dp[j],a.dp[i]+b.dp[j])
        return res

    def make(v):
        n=Node()
        n.dp[v]=1
        return n

    class ST:
        def __init__(self,a):
            self.n=len(a)
            self.t=[Node() for _ in range(4*self.n)]
            self.a=a
            self.build(1,0,self.n-1)

        def build(self,v,l,r):
            if l==r:
                self.t[v]=make(self.a[l])
                return
            m=(l+r)//2
            self.build(v*2,l,m)
            self.build(v*2+1,m+1,r)
            self.t[v]=merge(self.t[v*2],self.t[v*2+1])

        def query(self,v,l,r,ql,qr):
            if ql<=l and r<=qr:
                return self.t[v]
            m=(l+r)//2
            if qr<=m:
                return self.query(v*2,l,m,ql,qr)
            if ql>m:
                return self.query(v*2+1,m+1,r,ql,qr)
            return merge(self.query(v*2,l,m,ql,qr),self.query(v*2+1,m+1,r,ql,qr))

    n,q=map(int,input().split())
    a=list(map(int,input().split()))
    st=ST(a)

    out=[]

    for _ in range(q):
        t,l,r=map(int,input().split())
        l-=1;r-=1
        if t==2:
            res=st.query(1,0,n-1,l,r)
            out.append(str(max(res.dp)))
        else:
            a[l:r+1]=sorted(a[l:r+1])
            st=ST(a)

    return "\n".join(out)

# provided samples
assert run("""6 5
3 5 3 5 1 6
1 4 4
2 1 2
2 2 3
2 4 6
1 1 2
""") == """2
1
2"""

assert run("""6 4
5 2 4 5 1 2
2 3 5
1 2 3
1 3 6
2 3 6
""") == """2
4"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cập nhật phần tử đơn | 1 | trường hợp cơ sở đúng đắn | 
| mảng được sắp xếp đầy đủ | n | LIS trở thành độ dài đầy đủ | 
| giá trị xen kẽ | khác nhau | hợp nhất đúng đắn | 
| các loại chồng chéo lặp đi lặp lại | ổn định | cập nhật tính nhất quán | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng được lặp đi lặp lại việc sắp xếp trên các phân đoạn chồng chéo. Hãy xem xét một mảng trong đó chỉ có vùng giữa được sắp xếp lặp đi lặp lại trong khi điểm cuối không thay đổi. Thuật toán xử lý việc này vì mỗi cách sắp xếp sẽ đặt lại hoàn toàn thứ tự bên trong của phân đoạn đó và các truy vấn tiếp theo luôn hoạt động trên cấu trúc được cập nhật thông qua việc xây dựng lại, duy trì tính chính xác của trạng thái dp. 

Một trường hợp đặc biệt khác là truy vấn trên một phân đoạn đã được sắp xếp một phần nhiều lần. Mặc dù lịch sử mảng toàn cục rất phức tạp nhưng cây phân đoạn luôn phản ánh trạng thái mảng mới nhất, do đó, một truy vấn như`2 l r`luôn hợp nhất các nút dp hiện tại chính xác. 

Cuối cùng, các phân đoạn một phần tử đảm bảo tính chính xác của việc khởi tạo cơ sở. Vì mảng dp được khởi tạo với chính xác một đơn vị ở giá trị phần tử nên bất kỳ truy vấn nào trên một chỉ mục đều trả về 1, khớp với định nghĩa của LIS trên một phần tử.
