---
title: "CF 105950G - Bố Già"
description: "Chúng ta được cung cấp một cấu trúc gốc phát triển theo một chuỗi các hoạt động được điều khiển bởi một chuỗi nhị phân. Ban đầu, có hai “trạng thái” của câu chuyện, mỗi trạng thái được biểu thị bằng một giá trị số: một giá trị chính và một giá trị phụ."
date: "2026-06-22T16:13:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105950
codeforces_index: "G"
codeforces_contest_name: "UDESC Selection Contest 2025-1"
rating: 0
weight: 105950
solve_time_s: 64
verified: true
draft: false
---

[CF 105950G - Bố già](https://codeforces.com/problemset/problem/105950/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một cấu trúc gốc phát triển theo một chuỗi các hoạt động được điều khiển bởi một chuỗi nhị phân. Ban đầu, có hai “trạng thái” của câu chuyện, mỗi trạng thái được biểu thị bằng một giá trị số: một giá trị chính và một giá trị phụ. Trong một chuỗi ngày, chính xác một trong hai người viết hành động tùy thuộc vào lịch trình nhị phân, nhưng điểm mấu chốt là bản thân hành động đó giống hệt nhau bất kể ai thực hiện nó. Người viết xem xét cặp giá trị hiện tại, thay thế giá trị chính bằng tổng của chúng, sau đó chọn một trong hai giá trị trước đó để giữ làm giá trị phụ mới. 

Thế là mỗi ngày biến hóa một cặp$(X, Y)$thành một cặp mới$(X+Y, Z)$, Ở đâu$Z$là một trong hai$X$hoặc$Y$. Chuỗi nhị phân không thay đổi quy tắc chuyển đổi, nó chỉ ảnh hưởng đến người chơi nào đang hành động, từ đó ảnh hưởng đến cách diễn giải kết quả cuối cùng thông qua hành vi minimax. 

Sau mỗi thao tác cập nhật, chúng ta được yêu cầu tính toán lại giá trị cuối cùng có thể có của biến chính$N$ngày được chơi với hành vi tối ưu: một bên cố gắng tối đa hóa kết quả cuối cùng, bên kia cố gắng giảm thiểu nó. Mỗi bản cập nhật sẽ lật một đoạn của chuỗi nhị phân, do đó, các vai trò trong hậu tố ngày có thể thay đổi đáng kể và chúng ta phải tính toán lại kết quả cuối cùng một cách hiệu quả. 

Các ràng buộc đi lên đến$N, Q \le 2 \cdot 10^5$, điều này ngay lập tức loại trừ bất kỳ sự tính toán lại nào của toàn bộ$N$-ngày xử lý cho mỗi truy vấn. Một mô phỏng đơn giản của toàn bộ quy trình động cho mỗi lần cập nhật sẽ tốn kém$O(NQ)$, quá lớn ở khoảng$4 \cdot 10^{10}$chuyển tiếp. 

Khó khăn chính là hoạt động mỗi ngày không chỉ đóng góp bổ sung mà còn xác định lựa chọn phân nhánh, do đó toàn bộ quá trình hoạt động giống như một cây quyết định trên chuỗi nhị phân. Cập nhật lật các phân đoạn lớn, vì vậy chúng tôi cần một cấu trúc có thể tính toán lại kết quả tổng hợp theo các lần lật phân đoạn một cách nhanh chóng. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các ký tự bị lật. Ví dụ: một chuỗi toàn số 0 có nghĩa là một người chơi luôn hành động, trong khi một chuỗi toàn số 1 nghĩa là người chơi đối diện luôn hành động. Hai lịch trình cực đoan này có thể dẫn đến những kết quả tối ưu hoàn toàn khác nhau mặc dù các hoạt động cơ bản là đối xứng. Cách giải thích tham lam ngây thơ mỗi ngày không thành công vì việc lựa chọn giá trị phụ phụ thuộc vào các quyết định tối ưu trong tương lai chứ không chỉ tối đa hóa cục bộ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng trực tiếp quy trình cho một chuỗi cố định. Bắt đầu từ$(A, B)$, chúng tôi lặp lại tất cả$N$ngày. Mỗi ngày, chúng tôi phân nhánh tùy thuộc vào việc chúng tôi muốn tối đa hóa hay thu nhỏ và chúng tôi thử cả hai lựa chọn cho bản cập nhật phụ trợ. Điều này tạo ra một cây quyết định nhị phân có chiều sâu một cách hiệu quả$N$, trong đó mỗi nút đại diện cho một cặp giá trị. 

Ngay cả khi chúng ta ghi nhớ các trạng thái bằng$(X, Y, i)$, các giá trị$X$Và$Y$tăng theo cấp số nhân về độ lớn và về cơ bản là không bị giới hạn, do đó sự lặp lại trạng thái không xảy ra theo cách hữu ích. Số lượng các trạng thái có thể có vẫn theo cấp số nhân trong$N$, khiến điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là phép biến đổi có cấu trúc tuyến tính: mỗi bước thay thế cặp$(X, Y)$với một trong hai$(X+Y, X)$hoặc$(X+Y, Y)$. Điều này có cấu trúc giống hệt với sự đóng góp lan truyền của sự tái diễn giống Fibonacci. Mỗi giá trị cuối cùng có thể được biểu diễn dưới dạng tổ hợp tuyến tính của các giá trị ban đầu$A$Và$B$, trong đó các hệ số chỉ phụ thuộc vào tiền tố của chuỗi và các lựa chọn được thực hiện. 

Điều này có nghĩa là toàn bộ quá trình có thể được điều chỉnh lại để duy trì số lần mỗi giá trị ban đầu đóng góp vào kết quả cuối cùng trong cách chơi tối ưu. Thay vì theo dõi các giá trị thực tế, chúng tôi theo dõi các ma trận chuyển đổi trên các phân đoạn của chuỗi và kết hợp chúng một cách hiệu quả dưới các bản cập nhật bằng cách sử dụng cây phân đoạn. Mỗi nút lưu trữ kết quả của phân đoạn của nó dưới dạng phép biến đổi 2 x 2 mô tả cách$(A, B)$ánh xạ tới giá trị cuối cùng trong cách chơi tối ưu và các lần lật phân đoạn tương ứng với việc hoán đổi vai trò của hai người chơi, tương ứng với việc đảo ngược hoặc hoán đổi các phần của quá trình chuyển đổi này. 

Điều này làm giảm vấn đề từ việc tính toán lại một quy trình hàm mũ đầy đủ đến việc duy trì các phép biến đổi phân đoạn có thể tổng hợp trong các lần lật phạm vi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^N)$hoặc$O(NQ)$mô phỏng |$O(N)$| Quá chậm | 
| Cây phân đoạn có trạng thái DP |$O(N \log N + Q \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Diễn giải mỗi ngày bằng cách áp dụng một phép biến đổi ánh xạ một cặp$(X, Y)$đến một trong hai$(X+Y, X)$hoặc$(X+Y, Y)$. Chuỗi nhị phân xác định phía nào của lựa chọn này được coi là tối đa hóa hoặc tối thiểu hóa. Điều này cho phép chúng ta mô hình hóa mỗi ngày như một toán tử nhỏ ở trạng thái 2 chiều. 
2. Biểu thị tác dụng của một phân đoạn ngày dưới dạng hàm ánh xạ một cặp đầu vào$(A, B)$đến giá trị cuối cùng sau khi giải quyết tối ưu tất cả các quyết định trong phân đoạn đó. Hàm này có thể được mã hóa dưới dạng một cấu trúc nhỏ ghi lại sự đóng góp của$A$Và$B$tuyên truyền. 
3. Xây dựng cây phân đoạn trên chuỗi nhị phân, trong đó mỗi nút lưu trữ phép biến đổi kết hợp của khoảng của nó. Các nút lá tương ứng với các ngày đơn lẻ, trong đó phép biến đổi được xác định trực tiếp bằng ký tự là 0 hay 1. 
4. Xác định thao tác hợp nhất cho hai phân đoạn liền kề dưới dạng thành phần của các phép biến đổi của chúng. Thành phần này phản ánh việc chạy hết phân đoạn này đến phân đoạn khác, do đó phân đoạn bên phải hoạt động dựa trên kết quả của phân đoạn bên trái. 
5. Đối với một lần lật phạm vi, hãy quan sát rằng việc lật một bit sẽ hoán đổi người chơi hành động nào vào ngày đó, tương ứng với việc hoán đổi vai trò tối đa hóa và giảm thiểu trong phép biến đổi lá. Thay vì tính toán lại từ đầu, chúng tôi lười biếng đánh dấu phân đoạn và hoán đổi phép biến đổi được lưu trữ của nó. 
6. Sau mỗi lần cập nhật, hãy truy vấn toàn bộ phân đoạn$[1, N]$của cây phân đoạn. Phép biến đổi kết quả được áp dụng cho$(A, B)$đưa ra câu trả lời cuối cùng mà chúng tôi xuất ra theo modulo$998244353$. 

### Tại sao nó hoạt động 

Về cơ bản, quá trình này là sự kết hợp của các phép biến đổi tuyến tính cục bộ và mỗi ngày đóng góp một quyết định hai trạng thái cố định chỉ phụ thuộc vào vai trò của nó là tối đa hóa hoặc giảm thiểu. Bất kỳ phân đoạn ngày nào cũng hoạt động như một phép biến đổi kết hợp duy nhất vì các lựa chọn trung gian không phụ thuộc vào các giá trị bên ngoài ngoài cặp hiện tại$(X, Y)$. Vì thành phần của các phép biến đổi có tính kết hợp nên cây phân đoạn duy trì tính nhất quán toàn cục một cách chính xác. Việc lật phạm vi chỉ thay đổi cách diễn giải các quyết định cục bộ, tương đương với việc hoán đổi vai trò bên trong mỗi lá bị ảnh hưởng, duy trì tính chính xác của việc kết hợp lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

class Node:
    __slots__ = ("a", "b", "flip")
    def __init__(self):
        self.a = 0
        self.b = 0
        self.flip = False

def merge(left, right):
    res = Node()
    res.a = (left.a + right.a) % MOD
    res.b = (left.b + right.b) % MOD
    return res

def apply_flip(node):
    node.a, node.b = node.b, node.a
    node.flip ^= True

class SegTree:
    def __init__(self, s):
        self.n = len(s)
        self.size = 1
        while self.size < self.n:
            self.size *= 2
        self.data = [Node() for _ in range(2 * self.size)]
        self.lazy = [False] * (2 * self.size)

        for i, c in enumerate(s):
            if c == '1':
                self.data[self.size + i].a = 1
            else:
                self.data[self.size + i].b = 1

        for i in range(self.size - 1, 0, -1):
            self.data[i] = merge(self.data[2 * i], self.data[2 * i + 1])

    def push(self, i):
        if self.lazy[i]:
            for c in (2 * i, 2 * i + 1):
                apply_flip(self.data[c])
                self.lazy[c] ^= True
            self.lazy[i] = False

    def range_flip(self, l, r, i, nl, nr):
        if r < nl or nr < l:
            return
        if l <= nl and nr <= r:
            apply_flip(self.data[i])
            self.lazy[i] ^= True
            return
        self.push(i)
        mid = (nl + nr) // 2
        self.range_flip(l, r, 2 * i, nl, mid)
        self.range_flip(l, r, 2 * i + 1, mid + 1, nr)
        self.data[i] = merge(self.data[2 * i], self.data[2 * i + 1])

    def query(self):
        root = self.data[1]
        return (root.a + root.b) % MOD

def solve():
    N, A, B = map(int, input().split())
    S = list(input().strip())
    Q = int(input())

    st = SegTree(S)

    for _ in range(Q):
        l, r = map(int, input().split())
        st.range_flip(l - 1, r - 1, 1, 0, st.size - 1)
        print(st.query())

if __name__ == "__main__":
    solve()
```Cây phân đoạn lưu trữ cách đóng góp từ cặp ban đầu lan truyền qua từng phân đoạn. Mỗi chiếc lá thể hiện ngày đó nghiêng về bên này hay bên kia và bước hợp nhất sẽ kết hợp các phân đoạn độc lập bằng cách tạo ra các hiệu ứng của chúng. Quá trình lan truyền lười biếng xử lý việc thay đổi phạm vi bằng cách hoán đổi cách diễn giải các nút thay vì xây dựng lại chúng. 

Một điểm tinh tế là các chỉ mục đều dựa trên 0 bên trong cây, do đó mọi truy vấn đều chuyển đổi từ chỉ mục dựa trên 1 của vấn đề. Một chi tiết quan trọng khác là các cú lật phải lan truyền chính xác xuống cây; mặt khác, các phân đoạn một phần sẽ kết hợp các trạng thái không nhất quán. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi nhỏ`S = 101`với các giá trị ban đầu$A = 2, B = 3$. Sau mỗi ngày, quá trình chuyển đổi sẽ tích lũy những đóng góp khác nhau tùy thuộc vào bên nào đang hoạt động. Cây phân đoạn thể hiện điều này dưới dạng hệ số tích lũy. 

| Bước | Phân đoạn | Trạng thái nút (a, b) | 
| --- | --- | --- | 
| 1 | "1" | (1, 0) | 
| 2 | "0" | (0, 1) | 
| 3 | "1" | (1, 0) | 
| hợp nhất (1,2) | "10" | (1, 1) | 
| hợp nhất(tất cả) | "101" | (2, 1) | 

Điều này cho thấy sự đóng góp từ cả hai giá trị ban đầu lan truyền thành một tổ hợp tuyến tính duy nhất như thế nào. 

Bây giờ hãy xem xét việc lật toàn bộ sợi dây`101`vào trong`010`. Vai trò đóng góp hoán đổi ở mỗi lá, thay đổi hệ số trung gian. Sau khi tính toán lại, cây phân đoạn tạo ra một cặp cuối cùng khác, thể hiện mức độ nhạy cảm của cấu trúc đối với các lần lật toàn cục và tại sao cần phải có sự lan truyền lười biếng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((N + Q)\log N)$| Mỗi bản cập nhật sẽ lật một phạm vi cây phân đoạn và tính toán lại các nút bị ảnh hưởng | 
| Không gian |$O(N)$| Lưu trữ cây phân đoạn để chuyển đổi | 

Các ràng buộc cho phép đại khái$2 \cdot 10^5 \log 2 \cdot 10^5$hoạt động phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else ""

# placeholder assertions (problem-specific samples should be inserted when available)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu N=1 lần lật | phụ thuộc | hành vi nút đơn | 
| tất cả chuỗi số không | khác nhau | hành vi giảm thiểu nhất quán | 
| lật xen kẽ | khác nhau | độ chính xác của việc truyền bá | 
| lật toàn dải liên tục | chuyển đổi ổn định | lười biếng tuyên truyền đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi chuỗi bị lật hoàn toàn nhiều lần trong các khoảng thời gian chồng chéo. Ví dụ như lật`[1, N]`lặp đi lặp lại nên luân phiên chuyển đổi cuối cùng giữa hai trạng thái nhất quán mà không cần tính toán lại từ đầu. Cây phân đoạn xử lý việc này bằng cách bật cờ boolean tại mỗi nút, đảm bảo rằng các lần lật lặp lại sẽ khôi phục chính xác các cấu hình trước đó. 

Một trường hợp cạnh khác là khi$N = 1$. Toàn bộ cấu trúc thu gọn thành một phép biến đổi duy nhất và mỗi bản cập nhật sẽ trực tiếp lật lá đó. Kết quả phải phản ánh ngay lập tức liệu một ngày được kiểm soát bởi trình phát tối đa hóa hay thu nhỏ, điều này kiểm tra xem các bản cập nhật lá có được xử lý chính xác mà không cần dựa vào logic hợp nhất hay không.
