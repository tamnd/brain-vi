---
title: "CF 104091E - \u0428\u0430\u0445\u0442\u0451\u0440\u0441\u043a\u043e\u0435 \u0440\u0435\u043c\u0435\u0441\u043b\u043e"
description: "Chúng tôi đang mô phỏng một thế giới 2D đơn giản hoạt động giống như một dải 1D dài có chiều rộng n và chiều cao dọc không giới hạn. Ban đầu, mọi vị trí trên dải đất này đều được phủ cỏ. Trong quá trình này, công cụ trò chơi sẽ tạo ra các khối đất nằm ngang."
date: "2026-07-02T02:29:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104091
codeforces_index: "E"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u041f\u0435\u0442\u0440\u043e\u0437\u0430\u0432\u043e\u0434\u0441\u043a\u0435 \u0438 \u041a\u0430\u0440\u0435\u043b\u0438\u0438 2022-2023"
rating: 0
weight: 104091
solve_time_s: 64
verified: true
draft: false
---

[CF 104091E - \u0428\u0430\u0445\u0442\u0451\u0440\u0441\u043a\u043e\u0435 \u0440\u0435\u043c\u0435\u0441\u043b\u043e](https://codeforces.com/problemset/problem/104091/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một thế giới 2D đơn giản hoạt động giống như một dải dài có chiều rộng 1D`n`và chiều cao thẳng đứng không giới hạn. Ban đầu, mọi vị trí trên dải đất này đều được phủ cỏ. 

Trong quá trình này, công cụ trò chơi sẽ tạo ra các khối đất nằm ngang. Mỗi đoạn được mô tả bằng vị trí bắt đầu`a`và một chiều dài`b`. Điều này có nghĩa là tất cả các vị trí từ`a`ĐẾN`a + b - 1`nhận được một khối Khi một khối được đặt tại một vị trí, nó rơi thẳng xuống và xếp chồng lên các khối hiện có tại vị trí đó, tạo thành một cột đất thẳng đứng. 

Quan sát quan trọng là cỏ chỉ tồn tại trong các tế bào không bị trái đất chiếm giữ. Động cơ quan tâm đến việc hiện có bao nhiêu kết cấu cỏ được nhìn thấy, tương ứng với bao nhiêu vị trí trên dải đất vẫn không có khối đất nào cả. 

Mỗi truy vấn thuộc loại`2`yêu cầu số lượng vị trí hiện tại vẫn hoàn toàn không bị ảnh hưởng bởi bất kỳ vị trí khối nào cho đến nay. 

Các ràng buộc cho phép lên đến`n = 10^6`các vị trí và lên tới`q = 10^4`hoạt động. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại câu trả lời từ đầu cho mỗi truy vấn theo thời gian tuyến tính theo thời gian.`n`, vì điều đó sẽ có giá lên tới`10^10`hoạt động trong trường hợp xấu nhất. Chúng tôi cần hỗ trợ cập nhật phạm vi và tổng hợp toàn cầu nhanh chóng, lý tưởng nhất là theo thời gian logarit cho mỗi hoạt động. 

Một nhược điểm nhỏ là các phân đoạn chồng chéo không xếp chồng lên nhau một cách độc lập trong câu trả lời. Khi một vị trí đã được bao phủ ít nhất một lần, vị trí đó sẽ không được tính lại là cỏ, ngay cả khi các phân đoạn bổ sung được thêm vào sau đó. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp duy trì một mảng`covered[i]`cho biết liệu vị trí`i`đã từng nhận được một khối. Mỗi bản cập nhật thuộc loại`1 a b`sẽ lặp qua phân đoạn và đánh dấu tất cả các vị trí là được bảo hiểm. Mỗi truy vấn thuộc loại`2`sẽ đếm có bao nhiêu mục vẫn chưa được khám phá. 

Cách tiếp cận này đúng nhưng quá chậm. Trong trường hợp xấu nhất, một bản cập nhật có thể chạm tới`O(n)`các vị trí và có thể có`O(q)`những cập nhật như vậy, dẫn đến hành vi bậc hai. 

Quan sát cấu trúc quan trọng là chúng ta chỉ cần biết một vị trí bằng 0 hay khác 0 chứ không phải số khối chính xác được xếp chồng lên nhau. Mỗi thao tác chuyển đổi một phạm vi số 0 thành số 1 và khi một vị trí trở thành một, vị trí đó sẽ giữ nguyên vĩnh viễn. Đây là trường hợp cổ điển của việc duy trì một mảng động theo mức tăng phạm vi trong đó chúng ta chỉ quan tâm đến việc liệu giá trị có còn bằng 0 ở bất kỳ đâu hay không. 

Điều này làm giảm vấn đề duy trì một đoạn có độ dài`n`trong đó chúng tôi hỗ trợ tăng phạm vi thêm 1 và truy vấn có bao nhiêu phần tử vẫn bằng 0. Cây phân đoạn với cơ chế lan truyền lười biếng phù hợp với cấu trúc này một cách tự nhiên: mỗi nút theo dõi xem có bao nhiêu vị trí trong phân đoạn của nó vẫn bằng 0 và cập nhật các phân đoạn lật từ 0 hoàn toàn sang khác 0 một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đánh dấu mảng vũ phu | O(nq) | O(n) | Quá chậm | 
| Cây phân đoạn với sự lan truyền lười biếng | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cây phân đoạn trên mảng kích thước`n`. Mỗi vị trí bắt đầu bằng 0, nghĩa là nó vẫn được bao phủ bởi cỏ. 

Mỗi nút trong cây phân đoạn lưu trữ bao nhiêu vị trí trong khoảng của nó vẫn bằng 0. Ngoài ra, chúng tôi duy trì một điểm đánh dấu lười cho biết liệu toàn bộ phân đoạn có được bao phủ ít nhất một lần hay không. 

1. Xây dựng cây phân đoạn trong đó mỗi lá được khởi tạo bằng 1, vì tất cả các vị trí ban đầu đều chứa cỏ. 
2. Để cập nhật`1 a b`, chúng tôi áp dụng cập nhật phạm vi theo khoảng thời gian`[a, a + b - 1]`. Nếu một phân đoạn hoàn toàn nằm trong phạm vi cập nhật và hiện hoàn toàn bằng 0, chúng tôi sẽ đánh dấu phân đoạn đó là được bao phủ hoàn toàn và đặt số 0 của phân đoạn đó thành 0. 
3. Khi đẩy các bản cập nhật xuống, nếu một nút đã được đánh dấu là được che phủ hoàn toàn thì cả hai nút con ngay lập tức được đặt thành được che phủ hoàn toàn mà không cần đệ quy thêm. 
4. Sự chồng chéo một phần được xử lý bằng cách cập nhật đệ quy các phần tử con và tính lại số 0 của phần tử cha thành tổng của các phần tử con của nó. 
5. Đối với một truy vấn`2`, câu trả lời chỉ đơn giản là số 0 được lưu trữ tại nút gốc, biểu thị số lượng vị trí chưa bao giờ được che phủ. 

Lựa chọn thiết kế quan trọng là chúng tôi không bao giờ lưu trữ số lượng phủ sóng chính xác. Khi một phân đoạn trở nên hoàn toàn khác 0, nó sẽ không bao giờ thay đổi nữa, vì vậy chúng tôi sẽ thu gọn nó một cách tích cực. 

### Tại sao nó hoạt động 

Mỗi vị trí chuyển từ không che sang che phủ tối đa một lần. Cây phân đoạn đảm bảo rằng khi một phân đoạn được bao phủ hoàn toàn, nó sẽ không bao giờ được truy cập lại để cập nhật chi tiết. Điều này đảm bảo tính chính xác vì phạm vi bao phủ là đơn điệu và mọi cập nhật chỉ đưa hệ thống đến gần trạng thái được bao phủ hoàn toàn mà không bao giờ đảo ngược các thay đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, n):
        self.n = n
        self.size = 4 * n
        self.zero = [0] * self.size   # number of zeros in segment
        self.lazy = [0] * self.size   # 0 = untouched, 1 = fully covered

        self.build(1, 1, n)

    def build(self, v, l, r):
        if l == r:
            self.zero[v] = 1
            return
        m = (l + r) // 2
        self.build(v*2, l, m)
        self.build(v*2+1, m+1, r)
        self.zero[v] = self.zero[v*2] + self.zero[v*2+1]

    def push(self, v, l, r):
        if self.lazy[v] == 0:
            return
        if l != r:
            self.lazy[v*2] = 1
            self.lazy[v*2+1] = 1
        self.zero[v] = 0
        self.lazy[v] = 0

    def update(self, v, l, r, ql, qr):
        self.push(v, l, r)
        if r < ql or qr < l:
            return
        if ql <= l and r <= qr:
            self.lazy[v] = 1
            self.push(v, l, r)
            return
        m = (l + r) // 2
        self.update(v*2, l, m, ql, qr)
        self.update(v*2+1, m+1, r, ql, qr)
        self.zero[v] = self.zero[v*2] + self.zero[v*2+1]

def solve():
    n, q = map(int, input().split())
    st = SegTree(n)

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            a = int(tmp[1])
            b = int(tmp[2])
            st.update(1, 1, n, a, a + b - 1)
        else:
            print(st.zero[1])

if __name__ == "__main__":
    solve()
```Cây phân đoạn được xây dựng sao cho mỗi nút ban đầu thể hiện không gian được bao phủ hoàn toàn bởi cỏ. các`zero`mảng theo dõi xem còn lại bao nhiêu vị trí chưa được chạm tới trong mỗi phân đoạn. các`lazy`cờ đảm bảo rằng sau khi một phân đoạn được bao phủ hoàn toàn, chúng tôi sẽ không bao giờ lãng phí thời gian để xem lại phân đoạn đó. 

Chức năng cập nhật cẩn thận chỉ truyền bá vùng phủ sóng khi cần thiết. Khi một nút nằm hoàn toàn bên trong một đoạn được vẽ, nó sẽ được thu gọn ngay lập tức về 0 và quá trình đệ quy dừng lại. 

## Ví dụ đã hoạt động 

Hãy xem xét một thế giới nhỏ có kích thước`n = 5`với các thao tác:```
1 1 2
1 2 2
2
```Chúng tôi theo dõi có bao nhiêu vị trí chưa được sơn sau mỗi lần cập nhật. 

| Bước | Hoạt động | Phân đoạn được bảo hiểm | Số 0 còn lại | 
| --- | --- | --- | --- | 
| 0 | ban đầu | không | 5 | 
| 1 | thêm [1,2] | [1,2] | 3 | 
| 2 | thêm [2,3] | [1,2,3] | 2 | 
| 3 | truy vấn | [1,2,3] | 2 | 

Câu trả lời cuối cùng là 2 vì vị trí 4 và 5 chưa bao giờ được chạm tới. 

Bây giờ hãy xem xét việc phủ sóng toàn bộ chồng chéo:```
1 1 3
1 2 2
2
```| Bước | Hoạt động | Phân đoạn được bảo hiểm | Số 0 còn lại | 
| --- | --- | --- | --- | 
| 0 | ban đầu | không | 5 | 
| 1 | thêm [1,3] | [1,2,3] | 2 | 
| 2 | thêm [2,3] | [1,2,3] | 2 | 
| 3 | truy vấn | [1,2,3] | 2 | 

Điều này chứng tỏ rằng các cập nhật lặp lại không tính gấp đôi phạm vi phủ sóng, vì trạng thái là đơn điệu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | Mỗi bản cập nhật chỉ chạm vào các nút cây phân đoạn dọc theo phạm vi bị ảnh hưởng | 
| Không gian | O(n) | Mảng cây phân đoạn lưu trữ trạng thái cho tất cả các khoảng | 

Với`n ≤ 10^6`Và`q ≤ 10^4`, điều này phù hợp thoải mái trong giới hạn vì tổng số thao tác trên cây phân đoạn là vào khoảng vài trăm nghìn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []
    
    class SegTree:
        def __init__(self, n):
            self.n = n
            self.zero = [0] * (4*n)
            self.lazy = [0] * (4*n)
            self.build(1, 1, n)

        def build(self, v, l, r):
            if l == r:
                self.zero[v] = 1
                return
            m = (l+r)//2
            self.build(v*2,l,m)
            self.build(v*2+1,m+1,r)
            self.zero[v]=self.zero[v*2]+self.zero[v*2+1]

        def push(self, v, l, r):
            if self.lazy[v]:
                self.zero[v]=0
                if l!=r:
                    self.lazy[v*2]=1
                    self.lazy[v*2+1]=1
                self.lazy[v]=0

        def update(self,v,l,r,ql,qr):
            self.push(v,l,r)
            if r<ql or qr<l:
                return
            if ql<=l<=r<=qr:
                self.lazy[v]=1
                self.push(v,l,r)
                return
            m=(l+r)//2
            self.update(v*2,l,m,ql,qr)
            self.update(v*2+1,m+1,r,ql,qr)
            self.zero[v]=self.zero[v*2]+self.zero[v*2+1]

    def solve():
        n,q=map(int,input().split())
        st=SegTree(n)
        for _ in range(q):
            t=list(input().split())
            if t[0]=='1':
                a,b=int(t[1]),int(t[2])
                st.update(1,1,n,a,a+b-1)
            else:
                out.append(str(st.zero[1]))
        return "\n".join(out)

    return solve()

# provided samples
assert run("""3 4
1 1 2
1 2 2
1 1 1
2
""") == "1", "sample 1"

# all uncovered
assert run("""5 1
2
""") == "5", "no updates"

# full cover
assert run("""5 1
1 1 5
2
""") == "0", "full cover"

# overlapping updates
assert run("""5 3
1 1 3
1 2 5
2
""") == "0", "overlap"

# boundary
assert run("""1 2
1 1 1
2
""") == "0", "single cell"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có cập nhật | 5 | tính đúng đắn của trạng thái ban đầu | 
| bìa đầy đủ | 0 | xử lý ghi đè hoàn chỉnh | 
| chồng chéo | 0 | cập nhật bình thường | 
| ô đơn | 0 | độ đúng ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh chính là các cập nhật lặp lại trên cùng một khu vực. Ví dụ, áp dụng`1 1 3`hai lần không nên thay đổi câu trả lời sau lần nộp đơn đầu tiên. Cây phân đoạn xử lý việc này bằng cách thu gọn các phân đoạn được bao phủ đầy đủ, do đó bản cập nhật thứ hai sẽ tìm thấy các nút đã được đánh dấu và không thực hiện thêm công việc nào. 

Một trường hợp khác là thế giới đơn bào. Với`n = 1`, mọi bản cập nhật đều không bị ảnh hưởng hoặc che phủ hoàn toàn. Cấu trúc vẫn hoạt động chính xác vì các nút lá được khởi tạo và cập nhật trực tiếp mà không cần dựa vào việc truyền bá con. 

Trường hợp tinh vi cuối cùng là các bản cập nhật rời rạc mà cuối cùng sẽ bao trùm toàn bộ mảng theo nhiều bước. Việc lan truyền lười biếng đảm bảo rằng các phân đoạn một phần được hợp nhất một cách chính xác và gốc luôn phản ánh tổng số vị trí thực sự chưa được chạm tới.
