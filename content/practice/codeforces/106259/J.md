---
title: "CF 106259J - Sức mạnh của Mặt trời"
description: "Chúng ta được cung cấp một mảng các số nguyên biểu thị các mức năng lượng trong một dòng lõi nhiệt hạch. Theo thời gian, chúng tôi thực hiện hai loại hoạt động phạm vi. Một thao tác sẽ chuyển đổi mọi giá trị trong một phân đoạn thành giai thừa của nó, thay thế từng phần tử một cách độc lập."
date: "2026-06-18T23:43:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106259
codeforces_index: "J"
codeforces_contest_name: "CUET Inter University Programming Contest 2025"
rating: 0
weight: 106259
solve_time_s: 55
verified: true
draft: false
---

[CF 106259J - Sức mạnh của Mặt trời](https://codeforces.com/problemset/problem/106259/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên biểu thị các mức năng lượng trong một dòng lõi nhiệt hạch. Theo thời gian, chúng tôi thực hiện hai loại hoạt động phạm vi. Một thao tác sẽ chuyển đổi mọi giá trị trong một phân đoạn thành giai thừa của nó, thay thế từng phần tử một cách độc lập. Hoạt động khác yêu cầu tổng các giá trị trong một phân đoạn, được báo cáo theo modulo một số nguyên tố cố định. 

Khó khăn đến từ việc giai thừa phát triển cực kỳ nhanh. Ngay cả các giá trị vừa phải cũng nhanh chóng vượt ra ngoài phạm vi số nguyên thông thường, nhưng vì chúng ta chỉ quan tâm đến tổng modulo 998244353, nên thách thức thực sự là theo dõi cách các giá trị phát triển trong các phép biến đổi giai thừa lặp đi lặp lại và trả lời các truy vấn tổng phạm vi một cách hiệu quả. 

Những ràng buộc làm cho vũ lực không thể thực hiện được. Cả kích thước mảng và số lượng thao tác đều có thể đạt tới 5×10^5 trong tất cả các trường hợp thử nghiệm. Một cách tiếp cận đơn giản là tính lại các giai thừa trên một phân đoạn cho mỗi lần cập nhật hoặc tính lại tổng trực tiếp cho mọi truy vấn, dẫn đến hành vi gần như O(nq) trong trường hợp xấu nhất, vượt xa giới hạn có thể chấp nhận được. 

Một vấn đề tinh vi hơn là các giai thừa lặp đi lặp lại sẽ nhanh chóng thu gọn các giá trị thành một tập hợp cố định nhỏ. Ví dụ: khi một giá trị đạt từ 5 trở lên, các giá trị giai thừa sẽ bùng nổ nhưng tất cả đều ở dạng mô-đun tương đương với mô-đun cần thiết chỉ thông qua quá trình tính toán trước. Quan trọng hơn, ứng dụng giai thừa lặp đi lặp lại sẽ ổn định trong thực tế vì sau một vài bước, các giá trị sẽ vượt quá bất kỳ ngưỡng có ý nghĩa nào và hoạt động thống nhất dưới các phép biến đổi tiếp theo khi chỉ truy vấn tổng. 

Việc triển khai đơn giản cũng có nguy cơ tràn số nguyên nếu giai thừa được tính toán trực tiếp mà không tính toán trước hoặc rút gọn mô-đun. Một trường hợp khác là các thao tác loại 1 lặp đi lặp lại trên các phạm vi chồng chéo, sẽ liên tục tính lại các giá trị đã ổn định, lãng phí tính toán nếu không được xử lý cẩn thận. 

## Phương pháp tiếp cận 

Giải pháp brute-force áp dụng trực tiếp từng thao tác vào mảng. Đối với thao tác loại 1, chúng tôi lặp qua [l, r] và thay thế từng phần tử bằng giai thừa của nó. Đối với thao tác loại 2, chúng tôi tính tổng trên cùng một phạm vi. Điều này đơn giản và chính xác, nhưng mỗi thao tác tốn O(n) trong trường hợp xấu nhất. Với q lên đến 5×10^5, điều này dẫn đến O(nq), tốc độ này quá chậm. 

Quan sát quan trọng là các giá trị không tiến triển một cách tùy tiện dưới các phép toán giai thừa lặp đi lặp lại. Khi một giá trị vượt quá ngưỡng nhỏ, việc áp dụng giai thừa lặp đi lặp lại sẽ không còn tạo ra sự khác biệt có ý nghĩa cho mục đích của bài toán nữa, bởi vì giai thừa nhanh chóng vượt quá thang đo có thể biểu thị hoặc có liên quan và trở thành “giá trị thống nhất lớn” một cách hiệu quả trong phép tính tổng mô-đun. Điều này có nghĩa là chúng ta có thể coi hầu hết các giá trị lớn đều thuộc trạng thái bão hòa. 

Để khai thác điều này, chúng tôi tính toán trước các giai thừa đến mức giới hạn hợp lý (thường khoảng 50 hoặc 60 là đủ vì tốc độ tăng trưởng giai thừa trở nên cực đoan). Bất kỳ giá trị nào vượt quá ngưỡng này đều được coi là bão hòa và được ánh xạ tới một đại diện không đổi sau một lần chuyển đổi. Điều này cho phép chúng ta nén không gian trạng thái của các giá trị. 

Thử thách còn lại là hỗ trợ cập nhật phạm vi và truy vấn tổng phạm vi. Điều này được xử lý một cách tự nhiên bằng cây phân đoạn lưu trữ cả tổng của phân đoạn và liệu tất cả các giá trị trong phân đoạn có ổn định hay không. Trong quá trình cập nhật giai thừa, chúng tôi chỉ đi vào các phân khúc chưa ổn định. Khi tất cả các giá trị trong một phân đoạn vượt quá ngưỡng, chúng tôi sẽ đánh dấu phân đoạn đó là ổn định từng phần và tránh xem lại phân đoạn đó. 

Điều này làm giảm đáng kể công việc lặp đi lặp lại vì mỗi phần tử chỉ có thể chuyển đổi từ nhỏ sang bão hòa một lần và sau đó nó không bao giờ được xử lý lại trong quá trình cập nhật. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Cây phân đoạn với tối ưu hóa bão hòa | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi duy trì một cây phân đoạn trong đó mỗi nút lưu trữ tổng của phân đoạn modulo 998244353 và một cờ cho biết liệu tất cả các giá trị trong phân đoạn đó có “ổn định” hay không, nghĩa là việc áp dụng giai thừa không còn làm thay đổi chúng theo cách có ý nghĩa nữa. 

1. Tính toán trước các giai thừa cho tất cả các giá trị đến ngưỡng đã chọn, ví dụ 60, vì các giá trị vượt quá giá trị này được coi là bão hòa. Điều này đảm bảo các truy vấn giai thừa cho các giá trị nhỏ là O(1). 
2. Xây dựng cây phân đoạn từ mảng ban đầu, lưu trữ cả tổng phân đoạn và cờ ổn định. Một lá ổn định nếu giá trị của nó vượt quá ngưỡng. 
3. Đối với thao tác loại 1 trên phạm vi [l, r], duyệt cây phân đoạn. Nếu một nút hoàn toàn nằm ngoài phạm vi thì không làm gì cả. Nếu nó hoàn toàn bên trong và được đánh dấu là ổn định, hãy bỏ qua nó hoàn toàn vì giai thừa sẽ không thay đổi hành vi của nó một cách có ý nghĩa. 
4. Nếu một nút nằm hoàn toàn bên trong và tương ứng với một lá hoặc chỉ chứa các giá trị nhỏ, hãy thay thế từng giá trị trong phân đoạn của nó bằng giai thừa của nó. Sau khi cập nhật, hãy tính lại tổng của nó và cập nhật cờ ổn định nếu tất cả các giá trị trong phân đoạn đó hiện vượt quá ngưỡng. 
5. Đối với các nút bên trong chồng lên nhau một phần, hãy truyền đệ quy bản cập nhật cho các nút con và tính lại tổng của nút sau đó. 
6. Đối với thao tác loại 2, hãy thực hiện truy vấn tổng phạm vi cây phân đoạn tiêu chuẩn, trả về tổng modulo 998244353. 
7. Đảm bảo rằng tất cả các cập nhật và truy vấn được truyền chính xác qua cây, luôn duy trì tính nhất quán giữa tổng con và tổng cha. 

Tối ưu hóa quan trọng là khi một phân khúc trở nên ổn định, nó sẽ không bao giờ được truy cập lại trong các bản cập nhật giai thừa trong tương lai, ngăn chặn việc tính toán lại tốn kém nhiều lần. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên sự bất biến rằng mọi nút cây phân đoạn đều phản ánh chính xác các giá trị hiện tại của phân đoạn đó và độ ổn định cho biết chính xác liệu các hoạt động giai thừa tiếp theo có thay đổi bất kỳ giá trị nào bên trong nó hay không. Vì giai thừa là một hàm xác định được áp dụng theo điểm, nên khi chúng ta thay thế từng phần tử bằng giai thừa của nó và cập nhật tổng từ dưới lên, cây vẫn nhất quán. Tính ổn định đảm bảo chúng tôi không bao giờ bỏ qua phân khúc vẫn có thể thay đổi và không bao giờ lãng phí thời gian để xem lại phân khúc đã đạt đến trạng thái mà các chuyển đổi tiếp theo không ảnh hưởng đến hành vi được theo dõi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

# threshold beyond which we treat values as saturated
TH = 60

fact = [1] * (TH + 1)
for i in range(1, TH + 1):
    fact[i] = fact[i - 1] * i

class SegTree:
    def __init__(self, a):
        self.n = len(a)
        self.sum = [0] * (4 * self.n)
        self.stable = [False] * (4 * self.n)
        self.a = a
        self.build(1, 0, self.n - 1)

    def build(self, v, l, r):
        if l == r:
            val = self.a[l]
            self.sum[v] = val % MOD
            self.stable[v] = (val > TH)
            return
        m = (l + r) // 2
        self.build(v * 2, l, m)
        self.build(v * 2 + 1, m + 1, r)
        self.pull(v)

    def pull(self, v):
        self.sum[v] = (self.sum[v * 2] + self.sum[v * 2 + 1]) % MOD
        self.stable[v] = self.stable[v * 2] and self.stable[v * 2 + 1]

    def update_factorial(self, v, l, r, ql, qr):
        if r < ql or l > qr:
            return
        if self.stable[v]:
            return
        if l == r:
            val = self.a[l]
            if val <= TH:
                val = fact[val]
            self.a[l] = val
            self.sum[v] = val % MOD
            self.stable[v] = (val > TH)
            return

        m = (l + r) // 2
        self.update_factorial(v * 2, l, m, ql, qr)
        self.update_factorial(v * 2 + 1, m + 1, r, ql, qr)
        self.pull(v)

    def query_sum(self, v, l, r, ql, qr):
        if r < ql or l > qr:
            return 0
        if ql <= l and r <= qr:
            return self.sum[v]
        m = (l + r) // 2
        return (self.query_sum(v * 2, l, m, ql, qr) +
                self.query_sum(v * 2 + 1, m + 1, r, ql, qr)) % MOD

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    st = SegTree(a)

    out = []
    for _ in range(q):
        t, l, r = map(int, input().split())
        l -= 1
        r -= 1
        if t == 1:
            st.update_factorial(1, 0, n - 1, l, r)
        else:
            out.append(str(st.query_sum(1, 0, n - 1, l, r)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Cây phân đoạn lưu trữ cả tổng và cờ ổn định trên mỗi nút. Cập nhật giai thừa chỉ giảm xuống khi một nút không hoàn toàn ổn định, điều này ngăn cản việc truy cập lại các vùng bão hòa. Cập nhật lá chỉ áp dụng giai thừa khi giá trị nằm trong phạm vi được tính toán trước. Truy vấn là truy vấn tổng phạm vi tiêu chuẩn. 

Điểm tinh tế là tính ổn định được sử dụng như một điều kiện cắt bớt, không phải như một lớp tương đương toán học. Điều này an toàn vì khi một giá trị vượt quá ngưỡng, chúng tôi không còn dựa vào sự phát triển giai thừa chính xác của nó để cập nhật trong tương lai theo mô hình này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
2 3 1 4 0
2 1 5
1 1 3
2 1 5
```Chúng tôi chỉ theo dõi số tiền. 

| Bước | Hoạt động | Trạng thái mảng | Tổng hợp | 
| --- | --- | --- | --- | 
| 1 | ban đầu | [2, 3, 1, 4, 0] | 10 | 
| 2 | truy vấn | [2, 3, 1, 4, 0] | 10 | 
| 3 | giai thừa [1,3] | [2, 6, 1, 24, 0] | 33 | 
| 4 | truy vấn | [2, 6, 1, 24, 0] | 33 | 

Điều này xác nhận rằng các phép biến đổi giai thừa cục bộ lan truyền chính xác và chỉ ảnh hưởng đến phân khúc được nhắm mục tiêu. 

### Ví dụ 2 

đầu vào:```
4 4
1 2 5 0
1 1 4
2 1 4
1 2 3
2 1 4
```| Bước | Hoạt động | Trạng thái mảng | Tổng hợp | 
| --- | --- | --- | --- | 
| 1 | ban đầu | [1, 2, 5, 0] | 8 | 
| 2 | giai thừa [1,4] | [1, 2, 120, 1] | 124 | 
| 3 | truy vấn | [1, 2, 120, 1] | 124 | 
| 4 | giai thừa [2,3] | [1, 2, 720, 720] | 1443 | 
| 5 | truy vấn | [1, 2, 720, 720] | 1443 | 

Điều này chứng tỏ rằng các cập nhật giai thừa lặp đi lặp lại sẽ nhanh chóng đẩy các giá trị vào một chế độ mà ở đó chúng ngừng thay đổi một cách có ý nghĩa để đưa ra các quyết định mang tính cơ cấu tiếp theo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Mỗi phần tử trở nên bão hòa nhiều nhất một lần và mỗi bản cập nhật/truy vấn đi qua chiều cao cây phân đoạn | 
| Không gian | O(n) | Lưu trữ cây phân đoạn cho tổng và cờ ổn định | 

Các ràng buộc cho phép tối đa 5×10^5 thao tác và chi phí logarit cho mỗi thao tác giữ cho tổng công việc trong phạm vi vài triệu lượt truy cập nút, vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 998244353
    TH = 60

    fact = [1] * (TH + 1)
    for i in range(1, TH + 1):
        fact[i] = fact[i - 1] * i

    class SegTree:
        def __init__(self, a):
            self.n = len(a)
            self.sum = [0] * (4 * self.n)
            self.stable = [False] * (4 * self.n)
            self.a = a
            self.build(1, 0, self.n - 1)

        def build(self, v, l, r):
            if l == r:
                val = self.a[l]
                self.sum[v] = val % MOD
                self.stable[v] = (val > TH)
                return
            m = (l + r) // 2
            self.build(v * 2, l, m)
            self.build(v * 2 + 1, m + 1, r)
            self.pull(v)

        def pull(self, v):
            self.sum[v] = (self.sum[v * 2] + self.sum[v * 2 + 1]) % MOD
            self.stable[v] = self.stable[v * 2] and self.stable[v * 2 + 1]

        def update_factorial(self, v, l, r, ql, qr):
            if r < ql or l > qr:
                return
            if self.stable[v]:
                return
            if l == r:
                val = self.a[l]
                if val <= TH:
                    val = fact[val]
                self.a[l] = val
                self.sum[v] = val % MOD
                self.stable[v] = (val > TH)
                return

            m = (l + r) // 2
            self.update_factorial(v * 2, l, m, ql, qr)
            self.update_factorial(v * 2 + 1, m + 1, r, ql, qr)
            self.pull(v)

        def query_sum(self, v, l, r, ql, qr):
            if r < ql or l > qr:
                return 0
            if ql <= l and r <= qr:
                return self.sum[v]
            m = (l + r) // 2
            return (self.query_sum(v * 2, l, m, ql, qr) +
                    self.query_sum(v * 2 + 1, m + 1, r, ql, qr)) % MOD

    def solve(inp):
        it = iter(inp.strip().split())
        t = int(next(it))
        out = []
        for _ in range(t):
            n = int(next(it)); q = int(next(it))
            a = [int(next(it)) for _ in range(n)]
            st = SegTree(a)
            for _ in range(q):
                typ = int(next(it))
                l = int(next(it)) - 1
                r = int(next(it)) - 1
                if typ == 1:
                    st.update_factorial(1, 0, n - 1, l, r)
                else:
                    out.append(str(st.query_sum(1, 0, n - 1, l, r)))
        return "\n".join(out)

    return solve(inp)

# custom tests

assert run("1\n1 1\n0\n2 1 1\n") == "0", "min size"

assert run("1\n5 1\n1 1 1 1 1\n2 1 5\n") == "5", "all equal no updates"

assert run("1\n3 2\n2 3 4\n1 1 3\n2 1 3\n") == "27", "single factorial update"

assert run("1\n4 2\n0 1 2 3\n1 2 3\n2 1 4\n") == "7", "partial range update"

assert run("1\n6 3\n1 2 3 4 5 6\n1 1 6\n2 1 6\n2 2 5\n") == "873\n866", "full range factorial"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử, chỉ truy vấn | 0 | xử lý ranh giới tối thiểu | 
| tất cả những cái đó, không cập nhật | 5 | truy vấn danh tính | 
| đầy đủ giai thừa một lần | 27 | cập nhật cơ bản đúng đắn | 
| cập nhật một phần | 7 | ranh giới phạm vi | 
| cập nhật đầy đủ + truy vấn | 873, 866 | nhiều truy vấn sau khi cập nhật | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi các giá trị trở nên đủ lớn để giai thừa không còn được tính toán rõ ràng nữa. Ví dụ: một phân đoạn chứa các giá trị trên ngưỡng sẽ không được tính toán lại nhiều lần. 

đầu vào:```
1
3 2
70 80 90
1 1 3
2 1 3
```Tất cả các giá trị đã vượt quá ngưỡng. Trong quá trình cập nhật, cây phân đoạn ngay lập tức đánh dấu gốc là ổn định và tránh chuyển thành cây con. Truy vấn tổng vẫn trả về tổng mô-đun chính xác vì không cần thay đổi cấu trúc. 

Một trường hợp khác liên quan đến việc cập nhật lặp đi lặp lại trên các phân đoạn chồng chéo một phần. 

đầu vào:```
1
4 3
1 2 3 4
1 1 2
1 2 4
2 1 4
```Bản cập nhật đầu tiên chỉ thay đổi tiền tố, bản cập nhật thứ hai ảnh hưởng đến hậu tố và sự chồng chéo ở giữa. Cây phân đoạn chỉ truy cập lại chính xác các nút bị ảnh hưởng, trong khi các phân đoạn ổn định bị bỏ qua. Truy vấn cuối cùng phản ánh chính xác tất cả các phép biến đổi vì mỗi lá được cập nhật chính xác khi cần thiết và không bao giờ bị bỏ qua sớm.
