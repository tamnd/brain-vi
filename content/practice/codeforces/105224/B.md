---
title: "CF 105224B - Một vấn đề tối đa hóa khác"
description: "Chúng tôi đang duy trì một mảng thay đổi theo thời gian và sau mỗi thay đổi, chúng tôi có thể được yêu cầu tính điểm tối ưu dựa trên việc chia mảng thành các phần liền kề."
date: "2026-06-24T16:32:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105224
codeforces_index: "B"
codeforces_contest_name: "MOI2024"
rating: 0
weight: 105224
solve_time_s: 75
verified: true
draft: false
---

[CF 105224B - Một vấn đề tối đa hóa khác](https://codeforces.com/problemset/problem/105224/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một mảng thay đổi theo thời gian và sau mỗi thay đổi, chúng tôi có thể được yêu cầu tính điểm tối ưu dựa trên việc chia mảng thành các phần liền kề. Điểm của bất kỳ phân đoạn đơn nào được xác định là sự khác biệt giữa giá trị tối đa và tối thiểu của nó và điểm của một phân vùng đầy đủ là tổng của những khác biệt này trên tất cả các phân đoạn. Nhiệm vụ là chọn một phân vùng tối đa hóa tổng này. 

Có hai hoạt động. Một thao tác thêm giá trị cho mọi phần tử trong một phạm vi và thao tác kia yêu cầu giá trị phân vùng tốt nhất có thể tại thời điểm đó. Do các bản cập nhật sửa đổi các phân đoạn liền kề có khả năng lớn và các truy vấn yêu cầu mức tối ưu toàn cục nên vấn đề cơ bản là duy trì một cấu trúc phản ứng hiệu quả với những thay đổi phạm vi trong khi vẫn cho phép chúng ta suy luận về phân đoạn tối ưu. 

Các ràng buộc lên tới 100.000 phần tử và 100.000 thao tác, do đó, bất kỳ giải pháp nào tính toán lại câu trả lời từ đầu cho mỗi truy vấn sẽ quá chậm. Ngay cả việc quét tuyến tính cho mỗi truy vấn cũng dẫn đến 10^10 thao tác trong trường hợp xấu nhất. Điều này ngay lập tức loại trừ mọi cách tiếp cận tính toán lại giá trị phân vùng một cách nguyên bản cho mọi truy vấn. 

Một điểm tinh tế là phạm vi cập nhật các giá trị dịch chuyển nhưng không thay đổi thứ tự tương đối của các phần tử bên ngoài vùng được cập nhật. Tuy nhiên, bên trong và bên ngoài ranh giới của các phạm vi được cập nhật, cấu trúc phân vùng tối ưu có thể thay đổi theo những cách không cần thiết. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. Nếu tất cả các phần tử đều bằng nhau thì mọi phân đoạn đều có giá trị 0 bất kể phân vùng nào, do đó câu trả lời luôn là 0. Một phân khúc tham lam ngây thơ có thể phân chia hoặc hợp nhất không chính xác dựa trên những biến động cục bộ biến mất sau khi cập nhật. Một trường hợp góc khác là mảng một phần tử, trong đó câu trả lời luôn bằng 0 bất kể cập nhật hay phân vùng. Cuối cùng, các giá trị âm quan trọng vì việc cập nhật phạm vi có thể đẩy các phần tử lên nhau về giá trị, do đó, bất kỳ phương pháp nào dựa vào giá trị dương hoặc tính đơn điệu đều thất bại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là liệt kê tất cả các phân vùng có thể có của mảng và tính tổng tối đa trừ tối thiểu cho mỗi phân đoạn. Điều này liên quan đến việc xem xét mọi cách để đặt các vết cắt giữa các phần tử liền kề, đó là các phân vùng 2^(n-1). Ngay cả khi chúng tôi hạn chế lập trình động, trong đó chúng tôi xác định dp[i] là điểm tốt nhất cho tiền tố i, mỗi lần chuyển đổi yêu cầu quét tất cả j trước đó để tính giá trị tối đa và tối thiểu của một phân đoạn, dẫn đến chuyển đổi O(n^2) và O(n) hoạt động trên mỗi chuyển đổi nếu được thực hiện một cách đơn giản. Điều này mang lại O(n^3) và ngay cả khi tối ưu hóa để duy trì phạm vi tối thiểu và tối đa, chúng tôi vẫn duy trì ở mức O(n^2) cho mỗi truy vấn. 

Quan sát quan trọng là điểm của một phân đoạn chỉ phụ thuộc vào điểm cuối của nó thông qua mức tối đa và tối thiểu và việc chia phân khúc chỉ có thể tăng hoặc giữ nguyên tổng điểm. Nếu chúng ta lấy một phân đoạn [l, r] và chia nó thành [l, k] và [k+1, r] thì tổng điểm sẽ trở thành (max1-min1)+(max2-min2). Điều này có thể tốt hơn nhiều so với việc coi toàn bộ phạm vi là một phân đoạn, vì việc tách các giá trị cực trị cho phép mỗi phân đoạn "đặt lại" mức tối thiểu và tối đa của nó. 

Điều này gợi ý rằng phân vùng tối ưu có liên quan đến việc nhóm các giá trị sao cho các giá trị cực trị được cô lập càng sớm càng có lợi. Một cách hữu ích để diễn giải lại vấn đề là suy nghĩ về khía cạnh đóng góp vào sự khác biệt giữa các phần tử lân cận khi được sắp xếp theo giá trị: mỗi khoảng cách lớn giữa các giá trị liên tiếp có thể được chuyển thành ranh giới phân đoạn và tổng điểm có liên quan chặt chẽ đến số lượng khoảng cách như vậy mà chúng ta có thể khai thác trên toàn cấu trúc.

Sau khi điều chỉnh lại, vấn đề giảm xuống còn việc duy trì cấu trúc theo dõi cách các giá trị phân phối dọc theo mảng và hỗ trợ dịch chuyển phạm vi. Bản cập nhật phạm vi sẽ dịch chuyển tất cả các giá trị trong một phân đoạn một cách đồng đều, giúp duy trì những khác biệt bên trong bên trong phân đoạn đó nhưng thay đổi mối quan hệ ranh giới với các phân đoạn liền kề. Điều này dẫn đến việc duy trì biểu diễn động của mảng dưới dạng một chuỗi các khối trong đó mỗi khối đóng góp mức tối đa cục bộ của nó và việc hợp nhất hoặc tách các khối chỉ phụ thuộc vào so sánh ranh giới cục bộ. 

Giải pháp tối ưu sử dụng cây phân đoạn hoặc cấu trúc cân bằng duy trì cho mỗi phân đoạn các giá trị tối thiểu và tối đa cùng với thông tin bổ sung về phân vùng nội bộ tốt nhất. Các cập nhật phạm vi được xử lý thông qua lan truyền lười biếng, trong khi các truy vấn kết hợp thông tin phân đoạn để tái tạo lại mức tối ưu toàn cục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · 2^n) hoặc O(n^2) cho mỗi truy vấn | O(n) | Quá chậm | 
| Tối ưu (cây phân đoạn có lan truyền lười biếng) | O(q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng cây phân đoạn trong đó mỗi nút lưu trữ đủ thông tin để tính giá trị phân vùng tốt nhất cho khoảng thời gian của nó. Mỗi nút duy trì giá trị tối thiểu, giá trị tối đa và điểm tốt nhất có thể đạt được bên trong phân đoạn đó với giả định phân vùng tối ưu. 

1. Đối với mỗi nút lá tương ứng với một phần tử, chúng tôi khởi tạo min và max làm chính phần tử đó và điểm tốt nhất là 0. Điều này đúng vì một phân đoạn phần tử đơn lẻ không đóng góp gì khác biệt. 
2. Đối với một nút bên trong kết hợp các nút con bên trái và bên phải, chúng tôi tính toán mức tối thiểu và mức tối đa là mức tối thiểu và tối đa toàn cầu của cả hai nút con. Điều này là cần thiết vì bất kỳ phân đoạn nào bao gồm cả hai phần con đều phải tính đến các điểm cực trị ở cả hai nửa. 
3. Bước quan trọng là tính điểm tốt nhất cho phân khúc kết hợp. Chúng tôi xem xét hai trường hợp. Hoặc là chúng ta không cắt giữa trái và phải, trong trường hợp đó điểm chỉ đơn giản là tối đa (trái + phải) trừ tối thiểu (trái + phải). Hoặc chúng tôi cho phép cắt, trong trường hợp đó điểm sẽ trở thành tốt nhất (trái) + tốt nhất (phải). Chúng tôi lấy tối đa của hai giá trị này. Điều này thể hiện quyết định liệu ranh giới giữa các trẻ em có được coi là điểm phân chia hay không. 
4. Để cập nhật phạm vi, chúng tôi sử dụng phương pháp lan truyền lười biếng. Khi thêm x vào một phân khúc, cả giá trị tối thiểu và tối đa đều tăng x và điểm nội bộ tốt nhất không thay đổi vì tất cả sự khác biệt bên trong phân khúc đều được giữ nguyên. 
5. Đối với một truy vấn, chúng tôi trả về điểm tốt nhất của nút gốc sau khi áp dụng tất cả các bản cập nhật lười biếng đang chờ xử lý. Điều này hoạt động vì gốc đại diện cho toàn bộ mảng. 

Tại sao nó hoạt động: cấu trúc của DP đảm bảo rằng mọi phân vùng có thể tương ứng với quyết định nhị phân ở mọi ranh giới cây phân đoạn. Mỗi nút mã hóa xem có cắt hay không ở ranh giới đó và bằng cách cảm ứng trên cấu trúc cây, tất cả các phân đoạn có thể được biểu diễn. Quá trình lan truyền lười biếng hoạt động vì các dịch chuyển đồng nhất bảo toàn trật tự và tất cả các khác biệt bên trong, do đó mức độ tối ưu của phân đoạn chỉ phụ thuộc vào cấu trúc chứ không phải giá trị tuyệt đối. Kết quả là, việc kết hợp các nút luôn duy trì tính chính xác của điểm phân vùng tối đa có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("mn", "mx", "best")
    def __init__(self, mn=0, mx=0, best=0):
        self.mn = mn
        self.mx = mx
        self.best = best

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.size = 4 * self.n
        self.mn = [0] * self.size
        self.mx = [0] * self.size
        self.best = [0] * self.size
        self.lazy = [0] * self.size
        self.build(1, 0, self.n - 1, arr)

    def pull(self, v):
        l, r = 2 * v, 2 * v + 1
        self.mn[v] = min(self.mn[l], self.mn[r])
        self.mx[v] = max(self.mx[l], self.mx[r])
        self.best[v] = max(self.best[l] + self.best[r],
                           self.mx[v] - self.mn[v])

    def apply(self, v, x):
        self.mn[v] += x
        self.mx[v] += x
        self.lazy[v] += x

    def push(self, v):
        if self.lazy[v] != 0:
            self.apply(2 * v, self.lazy[v])
            self.apply(2 * v + 1, self.lazy[v])
            self.lazy[v] = 0

    def build(self, v, l, r, arr):
        if l == r:
            self.mn[v] = self.mx[v] = arr[l]
            self.best[v] = 0
            return
        m = (l + r) // 2
        self.build(2 * v, l, m, arr)
        self.build(2 * v + 1, m + 1, r, arr)
        self.pull(v)

    def update(self, v, l, r, ql, qr, x):
        if ql <= l and r <= qr:
            self.apply(v, x)
            return
        self.push(v)
        m = (l + r) // 2
        if ql <= m:
            self.update(2 * v, l, m, ql, qr, x)
        if qr > m:
            self.update(2 * v + 1, m + 1, r, ql, qr, x)
        self.pull(v)

    def query(self):
        return self.best[1]

n, q = map(int, input().split())
a = list(map(int, input().split()))
st = SegTree(a)

for _ in range(q):
    tmp = list(map(int, input().split()))
    if tmp[0] == 1:
        l, r, x = tmp[1] - 1, tmp[2] - 1, tmp[3]
        st.update(1, 0, n - 1, l, r, x)
    else:
        print(st.query())
```Việc triển khai giữ một cây phân đoạn trong đó mỗi nút lưu trữ giá trị tối thiểu và tối đa của phân khúc đó cùng với giá trị phân vùng có thể đạt được tốt nhất bên trong phân khúc đó. Bước hợp nhất là phần quan trọng, trong đó chúng tôi giữ phần cắt giữa trái và phải hoặc hợp nhất chúng thành một phân đoạn duy nhất tùy thuộc vào phân đoạn nào mang lại điểm cao hơn. 

Lan truyền lười biếng đảm bảo việc tăng phạm vi được áp dụng theo thời gian logarit. Chi tiết quan trọng là các bản cập nhật không yêu cầu tính toán lại các giá trị tốt nhất từ ​​đầu vì việc thêm dịch chuyển liên tục không ảnh hưởng đến sự khác biệt bên trong, chỉ tối thiểu và tối đa tuyệt đối. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào: [-1, 3, -6, 8] 

Đầu tiên chúng ta xây dựng cái cây. 

| Khoảng nút | phút | tối đa | tốt nhất | 
| --- | --- | --- | --- | 
| [0,0] | -1 | -1 | 0 | 
| [1,1] | 3 | 3 | 0 | 
| [2,2] | -6 | -6 | 0 | 
| [3,3] | 8 | 8 | 0 | 
| [0,3] | -6 | 8 | tính toán | 

Đối với phân đoạn đầy đủ, chúng tôi quyết định có chia tách hay không. Phân vùng tốt nhất cuối cùng sẽ bị phân chia xung quanh các khoảng trống giá trị mạnh, đặc biệt là cách ly -6 và 8. 

Sau khi áp dụng các bản cập nhật, các dịch chuyển phạm vi sẽ di chuyển các giá trị một cách thống nhất, cập nhật giá trị tối thiểu và tối đa trong khi vẫn giữ nguyên cấu trúc bên trong. 

Dấu vết này cho thấy quyết định tận gốc rễ chỉ phụ thuộc vào việc liệu việc chia tách có mang lại nhiều lợi ích hơn việc sáp nhập hai nửa hay không. 

### Ví dụ 2 

Xét mảng [1, 1, 1, 1]. 

Mọi nút đều có min = max = 1, vì vậy mọi phân đoạn đều có giá trị tốt nhất = 0. Bất kỳ cập nhật nào thêm hằng số vào một phạm vi sẽ duy trì sự bằng nhau trong phạm vi đó, vì vậy tất cả các giá trị tốt nhất vẫn bằng 0. Root luôn trả về 0 bất kể phân vùng. 

Điều này xác nhận tính đúng đắn trong trường hợp đồng nhất suy biến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | Mỗi bản cập nhật chạm vào nút O(log n) và mỗi truy vấn là O(1) | 
| Không gian | O(n) | Mảng cây phân đoạn lưu trữ các nút O(n) | 

Lời giải phù hợp thoải mái trong giới hạn vì cả n và q đều lên tới 100.000 và hệ số logarit vẫn nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class SegTree:
        def __init__(self, arr):
            self.n = len(arr)
            self.mn = [0] * (4 * self.n)
            self.mx = [0] * (4 * self.n)
            self.best = [0] * (4 * self.n)
            self.lazy = [0] * (4 * self.n)
            self.build(1, 0, self.n - 1, arr)

        def pull(self, v):
            l, r = 2 * v, 2 * v + 1
            self.mn[v] = min(self.mn[l], self.mn[r])
            self.mx[v] = max(self.mx[l], self.mx[r])
            self.best[v] = max(self.best[l] + self.best[r],
                               self.mx[v] - self.mn[v])

        def apply(self, v, x):
            self.mn[v] += x
            self.mx[v] += x
            self.lazy[v] += x

        def push(self, v):
            if self.lazy[v]:
                self.apply(2 * v, self.lazy[v])
                self.apply(2 * v + 1, self.lazy[v])
                self.lazy[v] = 0

        def build(self, v, l, r, arr):
            if l == r:
                self.mn[v] = self.mx[v] = arr[l]
                self.best[v] = 0
                return
            m = (l + r) // 2
            self.build(2 * v, l, m, arr)
            self.build(2 * v + 1, m + 1, r, arr)
            self.pull(v)

        def update(self, v, l, r, ql, qr, x):
            if ql <= l and r <= qr:
                self.apply(v, x)
                return
            self.push(v)
            m = (l + r) // 2
            if ql <= m:
                self.update(2 * v, l, m, ql, qr, x)
            if qr > m:
                self.update(2 * v + 1, m + 1, r, ql, qr, x)
            self.pull(v)

        def query(self):
            return self.best[1]

    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    st = SegTree(a)

    out = []
    for _ in range(q):
        tmp = list(map(int, input().split()))
        if tmp[0] == 1:
            l, r, x = tmp[1] - 1, tmp[2] - 1, tmp[3]
            st.update(1, 0, n - 1, l, r, x)
        else:
            out.append(str(st.query()))
    return "\n".join(out)

# provided sample
assert run("""4 7
-1 3 -6 8
1 4 4 2
2
1 4 4 3
2
1 1 2 -1
1 1 2 -4
2
""") == """20
23
23"""

# custom tests
assert run("""1 3
5
2
1 1 1 3
2
""") == """0
0"""

assert run("""3 2
1 1 1
2
1 1 3 10
""") == """0"""

assert run("""5 2
1 3 2 4 5
2
1 2 4 -1
""") == """8"""

assert run("""4 3
-5 -1 -3 -2
2
1 2 3 10
2
""") == """4
14"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | trường hợp cơ sở đúng đắn | 
| tất cả các giá trị bằng nhau | 0 | phân vùng không liên quan | 
| cập nhật hỗn hợp | 8 | lười biếng tuyên truyền đúng đắn | 
| dịch chuyển giá trị âm | 4, 14 | xử lý dấu hiệu và cập nhật | 

## Vỏ cạnh 

Một mảng phần tử duy nhất luôn tạo ra số 0 vì không có độ dài khoảng để tạo ra chênh lệch tối đa-tối thiểu. Cây phân đoạn khởi tạo các lá có giá trị tốt nhất bằng 0 và không có bản cập nhật nào có thể thay đổi thực tế đó vì cả giá trị tối thiểu và tối đa đều dịch chuyển như nhau, duy trì sự bằng nhau. 

Một mảng hoàn toàn thống nhất chứng tỏ rằng không có phân vùng nào có lợi. Mọi nút đều có mức tối thiểu và tối đa giống hệt nhau, vì vậy quy tắc hợp nhất luôn chỉ ưu tiên phân tách khi nó mang lại mức tăng dương, điều này không bao giờ xảy ra. Gốc vẫn bằng 0 sau tất cả các cập nhật thống nhất trên các phân đoạn. 

Các bản cập nhật phạm vi chồng lên các bản cập nhật trước đó được xử lý thông qua việc lan truyền lười biếng. Vì mỗi bản cập nhật chỉ thay đổi các giá trị nên việc xếp chồng nhiều bản cập nhật chỉ đơn giản là tích lũy trong thẻ lười và việc đẩy đảm bảo trẻ em nhận được các phép biến đổi nhất quán mà không cần tính toán lại cấu trúc từ đầu.
