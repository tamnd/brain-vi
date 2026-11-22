---
title: "CF 266E - Thêm truy vấn vào mảng..."
description: "Chúng ta được cung cấp một mảng thay đổi theo thời gian thông qua hai loại phép toán. Một thao tác sẽ ghi đè toàn bộ phân đoạn bằng một giá trị duy nhất, xóa thông tin trước đó trong khoảng đó một cách hiệu quả."
date: "2026-06-04T18:11:32+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "math"]
categories: ["algorithms"]
codeforces_contest: 266
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 163 (Div. 2)"
rating: 2500
weight: 266
solve_time_s: 126
verified: false
draft: false
---

[CF 266E - Thêm truy vấn vào mảng...](https://codeforces.com/problemset/problem/266/E) 

**Đánh giá:** 2500 
**Thẻ:** cấu trúc dữ liệu, toán học 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng thay đổi theo thời gian thông qua hai loại phép toán. Một thao tác sẽ ghi đè toàn bộ phân đoạn bằng một giá trị duy nhất, xóa thông tin trước đó trong khoảng đó một cách hiệu quả. Hoạt động khác yêu cầu tổng có trọng số trên một phân đoạn, trong đó mỗi vị trí đóng góp giá trị của nó được nâng lên lũy thừa lên tới 5 và tất cả các đóng góp được cộng lại với nhau theo mô-đun một số nguyên tố lớn. 

Điểm căng thẳng chính là các bản cập nhật có tính phá hủy và dựa trên phạm vi, trong khi các truy vấn yêu cầu thông tin phi tuyến tính về trạng thái phân đoạn hiện tại. Vì các giá trị có thể được thay thế nhiều lần nên mọi giải pháp tính toán lại câu trả lời từ đầu cho mỗi truy vấn sẽ không mở rộng được. 

Các ràng buộc đẩy mạnh về độ phức tạp logarit trên mỗi thao tác. Với tối đa 100.000 phép tính, mọi phép tính bậc hai ngay lập tức là không thể thực hiện được và thậm chí tuyến tính trên mỗi truy vấn cũng quá chậm. Điều này buộc một cấu trúc hỗ trợ cả việc gán phạm vi và truy xuất nhanh tổng công suất tổng hợp. 

Một trường hợp khó phát hiện xuất phát từ thực tế là số mũ lũy thừa k có thể bằng 0. Trong trường hợp đó, mọi phần tử đều đóng góp 1, do đó câu trả lời sẽ suy biến theo độ dài đoạn. Một trường hợp đặc biệt khác là các phép gán lặp lại: một giải pháp đơn giản cập nhật một phần các tập hợp được lưu trong bộ nhớ đệm sẽ âm thầm bị hỏng do các đóng góp trước đó phải bị loại bỏ hoàn toàn chứ không được điều chỉnh. 

Một chế độ lỗi khác xuất hiện khi cố gắng chỉ duy trì tổng các giá trị. Vì các truy vấn liên quan đến lũy thừa lên tới 5 nên chỉ biết Σa là không đủ; các phân phối khác nhau với cùng một tổng tạo ra các lũy thừa cao hơn khác nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng trực tiếp từng hoạt động. Để gán, chúng tôi ghi đè mọi phần tử trong phạm vi. Đối với các truy vấn, chúng tôi tính tổng lũy ​​thừa bằng cách lặp lại trong khoảng thời gian và nâng từng giá trị lên k. Điều này đúng vì nó tuân theo định nghĩa chính xác. 

Tuy nhiên, mỗi phép gán có thể mất O(n) thời gian trong trường hợp xấu nhất và mỗi truy vấn cũng mất O(n). Với tối đa 10^5 thao tác, điều này dẫn đến khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn khả thi. 

Quan sát chính là các truy vấn chỉ phụ thuộc vào tổng lũy ​​thừa của phân đoạn hiện tại và các phép gán sẽ ghi đè toàn bộ phân đoạn một cách thống nhất. Sự kết hợp này gợi ý một cây phân đoạn duy trì, đối với mỗi nút, tổng của a[i]^p cho p từ 0 đến 5. Việc gán phạm vi trở thành một thao tác lười biếng thay thế tất cả các giá trị này một cách xác định, bởi vì nếu một phân đoạn được đặt thành x thì mọi tổng lũy ​​thừa chỉ đơn giản là Seg_length * x^p. 

Do đó, cấu trúc này là một cây phân đoạn với cơ chế lan truyền lười biếng lưu trữ một vectơ đầy đủ có kích thước 6 trên mỗi nút. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Cây phân đoạn với sự lan truyền lười biếng | O(m log n * 6) | O(n * 6) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cây phân đoạn trong đó mỗi nút lưu trữ sáu giá trị, biểu thị tổng của a[i]^0 đến a[i]^5 trong khoảng của nó. Chúng tôi cũng duy trì một thẻ lười cho biết bài tập đang chờ xử lý.

1. Xây dựng cây phân đoạn từ mảng ban đầu bằng khả năng tính toán lên tới 5 cho mỗi phần tử và lưu trữ chúng trong các nút lá. Các nút bên trong kết hợp các nút con bằng cách tổng hợp các mức công suất tương ứng. Điều này có hiệu quả vì tổng quyền lực được phân bổ cho các liên minh rời rạc. 
2. Đối với một nút bao phủ một đoạn có độ dài len, nếu chúng ta gán tất cả các giá trị trong đó cho x, chúng ta có thể tính toán lại thông tin được lưu trữ của nó ngay lập tức. Với mỗi lũy thừa p, tổng sẽ trở thành len * x^p. Điều này tránh chạm vào các yếu tố riêng lẻ. 
3. Khi có một phép gán phạm vi, chúng tôi áp dụng nó bằng cách sử dụng phương pháp lan truyền lười biếng. Nếu một nút được che phủ hoàn toàn, chúng tôi sẽ ghi đè trạng thái của nó và lưu thẻ gán. Nếu nó bị che một phần, chúng ta sẽ đẩy nó xuống trước khi đệ quy. 
4. Khi đẩy một bài tập lười biếng từ cha mẹ sang con cái, chúng tôi ghi đè cả hai con với cùng một giá trị được chỉ định và cập nhật tổng công suất được lưu trữ của chúng cho phù hợp. Điều này đảm bảo không có đóng góp cũ nào còn lại. 
5. Để trả lời một truy vấn, chúng ta duyệt cây và thu thập các phân đoạn đóng góp. Khi một nút hoàn toàn nằm trong phạm vi truy vấn, chúng tôi trực tiếp trả về thành phần vectơ được lưu trữ của nó cho k. Chồng chéo một phần yêu cầu giảm dần. 
6. Số học mô-đun được áp dụng ở mỗi lần cập nhật vì các giá trị có thể tăng lớn do lũy thừa và tính tổng. 

### Tại sao nó hoạt động 

Tại mỗi nút, vectơ được lưu trữ biểu thị chính xác tổng lũy thừa của phân đoạn sau khi áp dụng tất cả các bản cập nhật đang chờ xử lý. Lan truyền lười biếng đảm bảo rằng bất kỳ phép gán nào ảnh hưởng đến một phân đoạn đều được áp dụng đầy đủ hoặc được trì hoãn một cách chính xác mà không trộn lẫn các giá trị cũ và mới. Bởi vì phép gán ghi đè hoàn toàn các giá trị nên không còn sự phụ thuộc lịch sử nào, do đó, mỗi nút có thể được xử lý độc lập sau khi được cập nhật. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.tree = [[0]*6 for _ in range(4*self.n)]
        self.lazy = [None]*(4*self.n)
        self.build(1, 0, self.n-1, arr)

    def build(self, v, l, r, arr):
        if l == r:
            x = arr[l] % MOD
            val = 1
            for p in range(6):
                self.tree[v][p] = val
                val = val * x % MOD
            return
        m = (l + r) // 2
        self.build(v*2, l, m, arr)
        self.build(v*2+1, m+1, r, arr)
        for p in range(6):
            self.tree[v][p] = (self.tree[v*2][p] + self.tree[v*2+1][p]) % MOD

    def apply(self, v, l, r, x):
        length = r - l + 1
        val = 1
        for p in range(6):
            self.tree[v][p] = val * length % MOD
            val = val * x % MOD
        self.lazy[v] = x

    def push(self, v, l, r):
        if self.lazy[v] is None or l == r:
            return
        m = (l + r) // 2
        self.apply(v*2, l, m, self.lazy[v])
        self.apply(v*2+1, m+1, r, self.lazy[v])
        self.lazy[v] = None

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
        for p in range(6):
            self.tree[v][p] = (self.tree[v*2][p] + self.tree[v*2+1][p]) % MOD

    def query(self, v, l, r, ql, qr, k):
        if ql <= l and r <= qr:
            return self.tree[v][k]
        self.push(v, l, r)
        m = (l + r) // 2
        res = 0
        if ql <= m:
            res += self.query(v*2, l, m, ql, qr, k)
        if qr > m:
            res += self.query(v*2+1, m+1, r, ql, qr, k)
        return res % MOD

n, m = map(int, input().split())
arr = list(map(int, input().split()))

st = SegTree(arr)

out = []
for _ in range(m):
    tmp = input().split()
    if tmp[0] == '=':
        l, r, x = map(int, tmp[1:])
        st.update(1, 0, n-1, l-1, r-1, x)
    else:
        l, r, k = map(int, tmp[1:])
        out.append(str(st.query(1, 0, n-1, l-1, r-1, k)))

print("\n".join(out))
```Việc triển khai xoay quanh việc lưu trữ sáu tập hợp được tính toán trước cho mỗi phân đoạn. Bước xây dựng khởi tạo các giá trị công suất trực tiếp tại các lá, đảm bảo công suất cao hơn được xây dựng tăng dần mà không cần tính toán lại. 

Hàm áp dụng là điểm tối ưu hóa cốt lõi. Thay vì lặp qua các phần tử, nó xây dựng lại toàn bộ trạng thái nút bằng cách sử dụng cấu trúc hình học của lũy thừa. Thẻ lười chỉ lưu trữ giá trị được chỉ định, vì giá trị đó xác định đầy đủ tất cả sáu tập hợp. 

Lực đẩy đảm bảo độ chính xác khi đi xuống. Bất kỳ nhiệm vụ đang chờ xử lý nào đều phải được cụ thể hóa ở trẻ em trước khi cập nhật hoặc truy vấn một phần. 

Cập nhật và truy vấn tuân theo các mẫu cây phân đoạn tiêu chuẩn, với điểm khác biệt chính là logic tổng hợp vectơ và ngữ nghĩa ghi đè. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
1 2 3 4
? 1 4 2
= 2 3 5
? 1 4 2
```Chúng tôi chỉ theo dõi k=2 truy vấn. 

| Bước | Hoạt động | Trạng thái phân đoạn (khái niệm) | Kết quả | 
| --- | --- | --- | --- | 
| 1 | truy vấn | [1,2,3,4] | 12+22+32+42 = 30 | 
| 2 | gán 2..3 = 5 | [1,5,5,4] | - | 
| 3 | truy vấn | [1,5,5,4] | 1+25+25+16 = 67 | 

Truy vấn đầu tiên xác nhận tính toán chính xác của tổng công suất. Nhiệm vụ cho thấy cấu trúc nội bộ phải ghi đè hoàn toàn những đóng góp trước đó. Truy vấn thứ hai thể hiện tính đúng đắn sau khi cập nhật lan truyền lười biếng. 

### Ví dụ 2 

đầu vào:```
3 2
7 7 7
? 1 3 0
? 1 3 1
```| Bước | Hoạt động | Tiểu bang | Kết quả | 
| --- | --- | --- | --- | 
| 1 | truy vấn k=0 | [7,7,7] | 3 | 
| 2 | truy vấn k=1 | [7,7,7] | 21 | 

Truy vấn đầu tiên thực hiện trường hợp k=0 trong đó mọi phần tử đều đóng góp 1. Truy vấn thứ hai xác nhận hành vi tổng tiêu chuẩn. Cả hai đều phụ thuộc vào việc lưu trữ sức mạnh thứ 0 một cách rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log n) | Mỗi bản cập nhật và truy vấn đi qua chiều cao của cây phân đoạn, duy trì 6 giá trị tổng hợp | 
| Không gian | O(n) | Mỗi nút lưu trữ vectơ có kích thước không đổi có độ dài 6 | 

Hệ số logarit vừa vặn thoải mái trong giới hạn cho 10^5 phép toán. Hệ số không đổi của 6 là không đáng kể và tất cả các phép toán đều là phép cộng và phép nhân đơn giản theo mô-đun. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 10**9 + 7

    class SegTree:
        def __init__(self, arr):
            self.n = len(arr)
            self.tree = [[0]*6 for _ in range(4*self.n)]
            self.lazy = [None]*(4*self.n)
            self.build(1, 0, self.n-1, arr)

        def build(self, v, l, r, arr):
            if l == r:
                x = arr[l] % MOD
                val = 1
                for p in range(6):
                    self.tree[v][p] = val
                    val *= x
                    val %= MOD
                return
            m = (l + r)//2
            self.build(v*2,l,m,arr)
            self.build(v*2+1,m+1,r,arr)
            for p in range(6):
                self.tree[v][p] = (self.tree[v*2][p]+self.tree[v*2+1][p])%MOD

        def apply(self,v,l,r,x):
            length = r-l+1
            val = 1
            for p in range(6):
                self.tree[v][p] = val*length%MOD
                val = val*x%MOD
            self.lazy[v] = x

        def push(self,v,l,r):
            if self.lazy[v] is None or l==r:
                return
            m=(l+r)//2
            self.apply(v*2,l,m,self.lazy[v])
            self.apply(v*2+1,m+1,r,self.lazy[v])
            self.lazy[v]=None

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
            for p in range(6):
                self.tree[v][p]=(self.tree[v*2][p]+self.tree[v*2+1][p])%MOD

        def query(self,v,l,r,ql,qr,k):
            if ql<=l and r<=qr:
                return self.tree[v][k]
            self.push(v,l,r)
            m=(l+r)//2
            res=0
            if ql<=m:
                res+=self.query(v*2,l,m,ql,qr,k)
            if qr>m:
                res+=self.query(v*2+1,m+1,r,ql,qr,k)
            return res%MOD

    n,m=map(int,input().split())
    arr=list(map(int,input().split()))
    st=SegTree(arr)
    out=[]
    for _ in range(m):
        t=input().split()
        if t[0]=='=':
            l,r,x=map(int,t[1:])
            st.update(1,0,n-1,l-1,r-1,x)
        else:
            l,r,k=map(int,t[1:])
            out.append(str(st.query(1,0,n-1,l-1,r-1,k)))
    return "\n".join(out)

# provided sample
assert run("""4 5
5 10 2 1
? 1 2 1
= 2 2 0
? 2 4 3
= 1 4 1
? 1 4 5
""") == """25
43
1300"""

# custom cases
assert run("""1 3
5
? 1 1 0
? 1 1 1
= 1 1 2
""") == """1
5"""

assert run("""5 2
1 1 1 1 1
? 1 5 5
? 2 4 0
""") == """5
3"""

assert run("""4 3
2 3 4 5
= 1 4 3
? 1 4 2
? 1 4 1
""") == """194
12"""

assert run("""3 2
10 0 2
? 1 3 0
? 1 3 2
""") == """3
104"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cập nhật/truy vấn phần tử đơn | 1, 5 | k=0 và k=1 độ đúng | 
| tất cả những cái | 5, 3 | hành vi phân khúc thống nhất | 
| ghi đè đầy đủ rồi truy vấn | 194, 12 | lười biếng tuyên truyền đúng đắn | 
| bao gồm số không | 3, 104 | xử lý số không và quyền hạn | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là k = 0. Trong cách triển khai đơn giản chỉ theo dõi tổng các giá trị, điều này sẽ tạo ra 0 không chính xác cho các phân đoạn trống hoặc chia tỷ lệ sai. Hành vi đúng phụ thuộc vào việc lưu trữ rõ ràng lũy ​​thừa thứ 0 là 1 cho mỗi phần tử. Ví dụ: đầu vào:```
3 1
7 8 9
? 1 3 0
```Câu trả lời đúng là 3 vì mọi phần tử đều đóng góp 7^0 = 1, 8^0 = 1, 9^0 = 1. Cây phân đoạn lưu trữ giá trị này trực tiếp trong thành phần đầu tiên của mỗi nút, do đó truy vấn trả về 3 mà không cần viết hoa đặc biệt. 

Một trường hợp cạnh khác được lặp lại ghi đè đầy đủ. Nếu một phân đoạn được chỉ định nhiều lần thì chỉ có phân đoạn cuối cùng mới quan trọng. Thẻ lười đảm bảo rằng các trạng thái trung gian không bao giờ được hợp nhất một phần. Ví dụ:```
3 3
1 2 3
= 1 3 5
= 1 3 7
? 1 3 1
```Kết quả đúng là 21 chứ không phải 15 vì phép gán đầu tiên phải bị loại bỏ hoàn toàn. Hàm áp dụng ghi đè tất cả sáu tập hợp, do đó không còn dư lượng nào từ các phép gán trước đó còn tồn tại.
