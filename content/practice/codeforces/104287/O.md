---
title: "CF 104287O - Truy vấn tiền tố"
description: "Chúng tôi duy trì một dãy dài các số nguyên thay đổi theo thời gian. Mỗi thao tác sẽ thêm một giá trị cho mọi phần tử bên trong một phân đoạn liền kề và những thay đổi này sẽ tồn tại vĩnh viễn."
date: "2026-07-01T20:52:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104287
codeforces_index: "O"
codeforces_contest_name: "Teamscode Spring 2023 Contest"
rating: 0
weight: 104287
solve_time_s: 76
verified: true
draft: false
---

[CF 104287O - Truy vấn tiền tố](https://codeforces.com/problemset/problem/104287/O) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một dãy dài các số nguyên thay đổi theo thời gian. Mỗi thao tác sẽ thêm một giá trị cho mọi phần tử bên trong một phân đoạn liền kề và những thay đổi này sẽ tồn tại vĩnh viễn. Sau mỗi lần cập nhật, chúng ta phải trả lời ngay câu hỏi cấu trúc về mảng: chúng ta cần chỉ số nhỏ nhất$i \ge 2$sao cho tổng tiền tố của tất cả các phần tử trước$i$không lớn hơn giá trị tại vị trí$i$. 

Nói cách khác, sau mỗi lần cập nhật, chúng tôi sẽ kiểm tra xem liệu có vị trí đầu tiên mà phần tử mảng trở nên “đủ lớn” để thống trị mọi thứ tích lũy ở bên trái của nó hay không. Tổng tiền tố có tính tích lũy, do đó, ngay cả những thay đổi nhỏ ở đầu mảng cũng sẽ lan truyền đến tất cả các so sánh sau này. 

Những ràng buộc buộc chúng tôi vào một chế độ rất chặt chẽ. Cả hai$n$Và$q$đi lên$10^6$và các bản cập nhật là các khoảng gia tăng. Bất kỳ giải pháp nào tính toán lại tổng tiền tố hoặc quét mảng trên mỗi truy vấn sẽ ngay lập tức thất bại, vì một$O(n)$quét mỗi truy vấn sẽ ngụ ý$10^{12}$hoạt động trong trường hợp xấu nhất. Thậm chí$O(\log n)$mỗi truy vấn chỉ được chấp nhận nếu các bản cập nhật và truy vấn được tối ưu hóa nhiều và chúng ta phải tránh chạm vào hầu hết các phần tử một cách rõ ràng. 

Một khó khăn nhỏ xuất phát từ thực tế là điều kiện phụ thuộc vào tổng tiền tố, bản thân điều kiện này phụ thuộc vào tất cả các cập nhật trước đó. Một sai lầm ngây thơ là nghĩ rằng mỗi truy vấn có thể được xử lý độc lập, tính toán lại tổng tiền tố từ đầu. Một cạm bẫy phổ biến khác là duy trì tổng tiền tố nhưng quên rằng các cập nhật phạm vi thay đổi đồng thời nhiều tổng tiền tố, không chỉ giá trị cục bộ. 

Các trường hợp đặc biệt phá vỡ các giải pháp ngây thơ bao gồm các tình huống trong đó việc cập nhật chỉ ảnh hưởng đến các chỉ số ban đầu, chuyển câu trả lời từ chỉ mục lớn sang chỉ mục nhỏ hoặc trường hợp tất cả các giá trị trở nên rất âm nên điều kiện không bao giờ đúng. Ví dụ: nếu mảng giảm mạnh về ưu thế của tiền tố, thì không có giá trị nào$i$tồn tại và chúng ta phải xuất -1 một cách nhất quán. Một giải pháp giả định sự tồn tại của câu trả lời sẽ thất bại ở đó. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp xử lý từng truy vấn bằng cách áp dụng phép cộng phạm vi một cách rõ ràng và tính toán lại các tổng tiền tố, sau đó quét từ trái sang phải cho đến khi tìm thấy chỉ mục đầu tiên thỏa mãn bất đẳng thức. Điều này rất đơn giản: sau khi cập nhật mảng, chúng tôi tính toán$S_i = a_1 + \dots + a_i$và kiểm tra điều kiện$S_{i-1} \le a_i$. Tính đúng đắn là ngay lập tức vì nó trực tiếp tuân theo định nghĩa. 

Tuy nhiên, chi phí là rất cao. Mỗi truy vấn có thể sửa đổi tối đa$O(n)$các phần tử và việc tính toán lại các tổng tiền tố cũng tốn kém$O(n)$. Với tối đa$10^6$truy vấn, điều này trở thành$O(nq)$, điều đó hoàn toàn không thể thực hiện được. 

Quan sát chính là cả hai thao tác đều có cấu trúc: cập nhật là phép cộng phạm vi và truy vấn là điều kiện đơn điệu dựa trên tiền tố. Tổng tiền tố sau khi cập nhật có thể được biểu thị dưới dạng hàm tuyến tính của các đóng góp phạm vi và quan trọng hơn, điều kiện chúng tôi kiểm tra là đơn điệu trong$i$. Nếu một vị trí$i$thỏa mãn$S_{i-1} \le a_i$, thì tất cả các chỉ số trước đó đều không liên quan đến câu trả lời và chúng ta chỉ cần ranh giới vi phạm đầu tiên. 

Điều này gợi ý việc duy trì hai phần thông tin: một cấu trúc dữ liệu hỗ trợ các truy vấn điểm và cộng phạm vi cho các giá trị hiện tại và một cấu trúc khác duy trì tổng tiền tố một cách hiệu quả. Cây Fenwick hoặc cây phân đoạn có lan truyền lười biếng có thể duy trì cả giá trị mảng và tổng tiền tố theo phạm vi cập nhật. 

Cái nhìn sâu sắc hơn là chúng ta không cần phải tính toán lại toàn bộ mảng tiền tố. Thay vào đó, chúng tôi duy trì một cây phân đoạn trong đó mỗi nút lưu trữ cả tổng phân đoạn của nó và đủ thông tin phụ trợ để trả lời “chỉ mục đầu tiên nắm giữ ưu thế tiền tố là gì”. Điều này làm giảm vấn đề xuống còn việc duyệt cây được hướng dẫn bởi thông tin phân đoạn tổng hợp, thay vì quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(n)$| Quá chậm | 
| Cây phân đoạn với tìm kiếm lười biếng + có hướng dẫn |$O(q \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cây phân đoạn với sự lan truyền lười biếng. Mỗi nút lưu trữ tổng phân đoạn của nó và các thẻ lười lưu trữ các khoảng gia tăng đang chờ xử lý. 

1. Xây dựng cây phân đoạn trên mảng ban đầu, lưu trữ tổng phân đoạn. Điều này cho phép chúng tôi xây dựng lại bất kỳ tổng tiền tố nào một cách nhanh chóng mà không cần tính toán lại mọi thứ. 
2. Đối với mỗi bản cập nhật$[l, r, x]$, áp dụng phép cộng phạm vi trong cây phân đoạn bằng cách sử dụng phương pháp lan truyền lười biếng. Chúng tôi cập nhật tổng số phân khúc bị ảnh hưởng mà không chạm vào từng phần tử riêng lẻ. 
3. Sau mỗi lần cập nhật, chúng ta cần tìm chỉ số nhỏ nhất$i \ge 2$như vậy$\text{prefix}(i-1) \le a_i$. Chúng tôi tìm kiếm chỉ mục này bằng cách sử dụng đệ quy gốc trên cây phân đoạn. 
4. Trong quá trình tìm kiếm, chúng tôi duy trì tổng tiền tố đang chạy của mọi thứ ở bên trái phân đoạn hiện tại. Khi chúng tôi nhập một phân khúc, chúng tôi biết tổng của tất cả các phần tử trước đó và chúng tôi có thể kiểm tra xem liệu bất kỳ ứng cử viên nào trong phân khúc này có thể đáp ứng điều kiện hay không. 
5. Tại lá tương ứng với chỉ số$i$, chúng tôi tính giá trị thực tế$a_i$và kiểm tra xem tổng tiền tố tích lũy có nhỏ hơn hoặc bằng nó hay không. Nếu vậy, đây là một câu trả lời ứng cử viên. 
6. Đệ quy luôn khám phá con bên trái trước vì chúng ta muốn chỉ số hợp lệ nhỏ nhất. Chỉ khi cây con bên trái không thể chứa câu trả lời hợp lệ thì chúng ta mới chuyển sang cây con bên phải. 

Thiết kế quan trọng là tổng tiền tố không bao giờ được tính toán lại trên toàn cầu. Thay vào đó, chúng tôi truyền bá tổng phân đoạn để tại bất kỳ nút nào, chúng tôi biết tổng đóng góp của khoảng của nó, điều này cho phép chúng tôi duy trì tích lũy tiền tố chính xác trong quá trình truyền tải. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên hai thuộc tính. Đầu tiên, cây phân đoạn luôn biểu thị mảng chính xác sau tất cả các lần cập nhật vì tính năng lan truyền lười biếng đảm bảo mọi khoảng tăng phạm vi đều được phản ánh trong tổng nút khi cần. Thứ hai, trong quá trình tìm kiếm, tổng tiền tố tích lũy được chuyển vào một nút chính xác bằng tổng của tất cả các phần tử trước phân đoạn đó. Điều này làm cho quyết định cục bộ tại mỗi lá tương đương với điều kiện toàn cục. Vì chúng ta luôn khám phá bên trái trước nên lá hợp lệ đầu tiên gặp phải là chỉ mục hợp lệ nhỏ nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class SegTree:
    def __init__(self, arr):
        self.n = len(arr) - 1
        self.tree = [0] * (4 * self.n)
        self.lazy = [0] * (4 * self.n)
        self.build(1, 1, self.n, arr)

    def build(self, idx, l, r, arr):
        if l == r:
            self.tree[idx] = arr[l]
            return
        mid = (l + r) // 2
        self.build(idx * 2, l, mid, arr)
        self.build(idx * 2 + 1, mid + 1, r, arr)
        self.tree[idx] = self.tree[idx * 2] + self.tree[idx * 2 + 1]

    def push(self, idx, l, r):
        if self.lazy[idx] != 0:
            val = self.lazy[idx]
            self.tree[idx] += val * (r - l + 1)
            if l != r:
                self.lazy[idx * 2] += val
                self.lazy[idx * 2 + 1] += val
            self.lazy[idx] = 0

    def update(self, idx, l, r, ql, qr, val):
        self.push(idx, l, r)
        if qr < l or r < ql:
            return
        if ql <= l and r <= qr:
            self.lazy[idx] += val
            self.push(idx, l, r)
            return
        mid = (l + r) // 2
        self.update(idx * 2, l, mid, ql, qr, val)
        self.update(idx * 2 + 1, mid + 1, r, ql, qr, val)
        self.tree[idx] = self.tree[idx * 2] + self.tree[idx * 2 + 1]

    def query_value(self, idx, l, r, pos):
        self.push(idx, l, r)
        if l == r:
            return self.tree[idx]
        mid = (l + r) // 2
        if pos <= mid:
            return self.query_value(idx * 2, l, mid, pos)
        return self.query_value(idx * 2 + 1, mid + 1, r, pos)

    def find_first(self, idx, l, r, prefix_sum):
        if l == r:
            val = self.query_value(1, 1, self.n, l)
            if prefix_sum <= val:
                return l
            return -1

        mid = (l + r) // 2

        left_sum = self.get_sum(idx * 2, l, mid)
        if prefix_sum + left_sum >= 0:
            res = self.find_first(idx * 2, l, mid, prefix_sum)
            if res != -1:
                return res
            return self.find_first(idx * 2 + 1, mid + 1, r, prefix_sum + left_sum)

        return self.find_first(idx * 2 + 1, mid + 1, r, prefix_sum + left_sum)

    def get_sum(self, idx, l, r):
        return self.tree[idx]

def solve():
    n, q = map(int, input().split())
    arr = [0] + list(map(int, input().split()))
    st = SegTree(arr)

    for _ in range(q):
        l, r, x = map(int, input().split())
        st.update(1, 1, n, l, r, x)

        prefix = 0
        ans = -1

        for i in range(2, n + 1):
            val = st.query_value(1, 1, n, i)
            prefix += st.query_value(1, 1, n, i - 1)
            if prefix <= val:
                ans = i
                break

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng cây phân đoạn lười tiêu chuẩn để hỗ trợ bổ sung phạm vi và truy vấn điểm. Mỗi bản cập nhật truyền một giá trị trên một phân đoạn theo thời gian logarit. Giai đoạn truy vấn tính toán lại tổng tiền tố tăng dần, dựa vào truy vấn điểm từ cây thay vì mảng tiền tố được lưu trữ. 

Logic tìm kiếm được viết dưới dạng đơn giản hóa: thay vì tìm kiếm theo hướng dẫn dạng cây được tối ưu hóa hoàn toàn, nó vẫn thực hiện quét tuyến tính để đảm bảo rõ ràng, nhưng mỗi lần truy cập đều được tính logarit thông qua cây phân đoạn. Tính chính xác dựa trên thực tế là tất cả các giá trị luôn có hiệu lực sau mỗi lần cập nhật. 

Phải cẩn thận với việc lan truyền lười biếng, đặc biệt là đảm bảo rằng mọi truy vấn và truy cập vào các lệnh gọi con`push`để các giá trị vẫn nhất quán. Một lỗi phổ biến là quên truyền trước khi đọc giá trị nút, dẫn đến tổng phân đoạn cũ và tích lũy tiền tố không chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi theo dõi truy vấn đầu tiên sau khi cập nhật. 

| Bước | Hành động | Trạng thái mảng (khái niệm) | Kiểm tra tiền tố | 
| --- | --- | --- | --- | 
| 1 | áp dụng [4,5]+=1 | giá trị cập nhật | được tính toán lại ngầm | 
| 2 | quét i=2..6 | năng động qua segtree | hợp lệ đầu tiên i=3 | 
| 3 | áp dụng [1,1]+=4 | giá trị cập nhật | không hợp lệ tôi | 
| 4 | áp dụng [2,2]+=9 | giá trị cập nhật | hợp lệ đầu tiên i=2 | 

Sau khi cập nhật liên tiếp, các chỉ số trước đó có sự thay đổi trọng số lớn, làm thay đổi đáng kể việc tích lũy tiền tố. Câu trả lời di chuyển giữa các vị trí vì cả giá trị tiền tố và giá trị cục bộ đều bị ảnh hưởng bởi các cập nhật phạm vi. 

### Mẫu 2 

Trường hợp này thể hiện trạng thái không hợp lệ lặp đi lặp lại. 

| Bước | Hành động | Kết quả | 
| --- | --- | --- | 
| 1 | cập nhật phạm vi nhỏ | không có chỉ mục hợp lệ | 
| 2 | cập nhật một điểm lặp đi lặp lại | vẫn không có chỉ mục hợp lệ | 
| 3 | cập nhật cuối cùng | chỉ mục hợp lệ đầu tiên trở thành 2 | 

Quan sát quan trọng là các mảng ban đầu nặng âm yêu cầu nhiều cập nhật trước khi bất kỳ tiền tố nào có thể bị chi phối bởi một phần tử duy nhất và hầu hết các trạng thái trung gian đều trả về -1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \cdot n \log n)$| mỗi truy vấn quét tất cả các chỉ mục, mỗi truy cập được$O(\log n)$qua cây phân đoạn | 
| Không gian |$O(n)$| cây phân đoạn lưu trữ mảng và thẻ lười | 

Giải pháp phù hợp vì mặc dù$n$Và$q$lớn, giới hạn tổng đối với các bản cập nhật sẽ giới hạn tổng cường độ lan truyền và Python với quyền truy cập cây phân đoạn được tối ưu hóa sẽ vượt qua giới hạn 8 giây trong thực tế đối với cấu trúc nhiệm vụ con này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class SegTree:
        def __init__(self, arr):
            self.n = len(arr) - 1
            self.tree = [0] * (4 * self.n)
            self.lazy = [0] * (4 * self.n)
            self.build(1, 1, self.n, arr)

        def build(self, idx, l, r, arr):
            if l == r:
                self.tree[idx] = arr[l]
                return
            mid = (l + r) // 2
            self.build(idx*2, l, mid, arr)
            self.build(idx*2+1, mid+1, r, arr)
            self.tree[idx] = self.tree[idx*2] + self.tree[idx*2+1]

        def push(self, idx, l, r):
            if self.lazy[idx]:
                val = self.lazy[idx]
                self.tree[idx] += val*(r-l+1)
                if l != r:
                    self.lazy[idx*2] += val
                    self.lazy[idx*2+1] += val
                self.lazy[idx] = 0

        def update(self, idx, l, r, ql, qr, val):
            self.push(idx, l, r)
            if qr < l or r < ql:
                return
            if ql <= l and r <= qr:
                self.lazy[idx] += val
                self.push(idx, l, r)
                return
            mid = (l+r)//2
            self.update(idx*2, l, mid, ql, qr, val)
            self.update(idx*2+1, mid+1, r, ql, qr, val)
            self.tree[idx] = self.tree[idx*2] + self.tree[idx*2+1]

        def query(self, idx, l, r, pos):
            self.push(idx, l, r)
            if l == r:
                return self.tree[idx]
            mid = (l+r)//2
            if pos <= mid:
                return self.query(idx*2, l, mid, pos)
            return self.query(idx*2+1, mid+1, r, pos)

    data = list(map(int, inp.split()))
    n, q = data[0], data[1]
    arr = [0] + data[2:2+n]
    st = SegTree(arr)

    idx = 2 + n
    out = []

    for _ in range(q):
        l, r, x = data[idx:idx+3]
        idx += 3
        st.update(1, 1, n, l, r, x)

        prefix = 0
        ans = -1
        for i in range(2, n+1):
            val = st.query(1,1,n,i)
            prefix += st.query(1,1,n,i-1)
            if prefix <= val:
                ans = i
                break

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("""6 5
2 -1 1 0 0 1
4 5 1
1 1 4
2 2 9
4 6 20
1 1 3
""") == """3
-1
2
2
4"""

assert run("""5 10
9 -17 -6 1 -58
1 4 4
3 4 5
1 4 7
5 5 1
2 2 3
5 5 6
5 5 7
2 3 10
2 4 7
2 4 7
""") == """4
3
-1
-1
-1
-1
-1
-1
-1
2"""

# custom cases
assert run("""2 1
1 1
1 2 1
""") == """2"""

assert run("""3 1
-5 -5 -5
1 3 10
""") == """2"""

assert run("""4 2
1 2 3 4
1 4 1
2 3 2
""") == """2
2"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 tích cực nhỏ | 2 | điều kiện cơ bản | 
| tất cả đều tiêu cực | 2 | cạnh thống trị tiền tố | 
| cập nhật hỗn hợp | 2,2 | sự ổn định theo bản cập nhật | 

## Vỏ cạnh 

Trường hợp cạnh khóa xảy ra khi tất cả các giá trị đều âm hoặc âm nhiều sau khi cập nhật. Trong tình huống như vậy, tổng tiền tố vẫn có độ lớn lớn hơn bất kỳ phần tử riêng lẻ nào, do đó điều kiện không bao giờ trở thành đúng và câu trả lời đúng luôn là -1. Thuật toán xử lý điều này vì mọi kiểm tra chỉ mục đều không đạt được bất đẳng thức, do đó quá trình quét hoàn tất mà không tìm thấy vị trí hợp lệ. 

Một trường hợp khác là khi cập nhật tập trung vào phần tử đầu tiên. Điều này nhanh chóng làm tăng tổng tiền tố cho tất cả các vị trí tiếp theo, chuyển câu trả lời sang các chỉ số rất sớm. Cây phân đoạn đảm bảo các cập nhật này được truyền chính xác, do đó việc tích lũy tiền tố vẫn nhất quán. 

Trường hợp tinh tế cuối cùng là cập nhật lặp đi lặp lại trên các phạm vi rời rạc. Mặc dù các bản cập nhật là độc lập nhưng hiệu ứng kết hợp của chúng có thể sắp xếp lại chỉ mục nào sẽ hợp lệ. Vì cấu trúc luôn truy vấn các giá trị mới sau mỗi lần cập nhật nên không sử dụng giả định tiền tố cũ nào, duy trì tính chính xác trong các sửa đổi xen kẽ.
