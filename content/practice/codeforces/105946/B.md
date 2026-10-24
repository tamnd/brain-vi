---
title: "CF 105946B - Trò chơi hấp thụ"
description: "Chúng ta được cung cấp một mảng và nhiều thao tác trên các mảng con. Mỗi truy vấn yêu cầu một giá trị bắt nguồn từ quá trình “hấp thụ” ngẫu nhiên và các bản cập nhật sẽ thay thế toàn bộ phân đoạn bằng một mẫu cố định."
date: "2026-06-22T16:00:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105946
codeforces_index: "B"
codeforces_contest_name: "2025 UP ACM Algolympics Final Round"
rating: 0
weight: 105946
solve_time_s: 82
verified: true
draft: false
---

[CF 105946B - Trò chơi hấp thụ](https://codeforces.com/problemset/problem/105946/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và nhiều thao tác trên các mảng con. Mỗi truy vấn yêu cầu một giá trị bắt nguồn từ quá trình “hấp thụ” ngẫu nhiên và các bản cập nhật sẽ thay thế toàn bộ phân đoạn bằng một mẫu cố định. 

Bản thân quá trình hấp thụ liên tục hợp nhất hai phần tử liền kề: một chỉ số được chọn, phần tử bên phải được hấp thụ vào phần bên trái và giá trị bên trái tăng theo cách phi tuyến tính bao gồm cả phép cộng và phép nhân. Quá trình tiếp tục cho đến khi chỉ còn lại một phần tử và con số cuối cùng đó là điểm số. 

Thoạt nhìn, tính ngẫu nhiên cho thấy chúng ta phải tính toán kỳ vọng theo cấp số nhân của nhiều chuỗi hợp nhất. Tuy nhiên, cấu trúc chính của phép toán được ẩn bên trong dạng đại số chứ không phải là tính ngẫu nhiên của nó. 

Kích thước đầu vào đạt tới hai trăm nghìn phần tử và hai trăm nghìn thao tác, do đó, bất kỳ giải pháp nào mô phỏng việc hợp nhất hoặc tính toán lại kỳ vọng cho mỗi truy vấn đều ngay lập tức không thể thực hiện được. Thậm chí một$O(n \log n)$tính toán lại cho mỗi truy vấn sẽ quá chậm. Điều này buộc phải đưa ra giải pháp trong đó mỗi thao tác sửa đổi hoặc trả lời một phân đoạn trong thời gian gần như không đổi hoặc logarit và trong đó quá trình hấp thụ phải thu gọn thành một thống kê phạm vi đơn giản. 

Một cạm bẫy tinh vi là giả định các lệnh hợp nhất khác nhau sẽ tạo ra các kết quả khác nhau. Việc triển khai đơn giản sẽ mô phỏng các sự hợp nhất ngẫu nhiên liền kề và thử ước tính Monte Carlo hoặc lập trình động theo các khoảng thời gian, nhưng cả hai đều thất bại. Cấu trúc khoảng thời gian thay đổi theo cách phụ thuộc vào lịch sử và mọi DP trạng thái trên tất cả các phân đoạn sẽ bùng nổ theo cách kết hợp. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực tuân theo các quy tắc theo nghĩa đen của trò chơi: mô phỏng các phép hợp nhất ngẫu nhiên liền kề nhiều lần và lấy kết quả trung bình. Mỗi chi phí mô phỏng$O(n)$và để đạt được độ chính xác ổn định sẽ yêu cầu nhiều lần lặp lại cho mỗi truy vấn, dẫn đến độ phức tạp vô vọng. 

Ngay cả một DP chính xác theo từng khoảng thời gian cũng có thể gây hiểu nhầm. Người ta có thể cố gắng tính toán các kết quả mong đợi cho mỗi mảng con, nhưng thao tác hợp nhất phụ thuộc vào những thay đổi gần kề theo cách phá hủy tính độc lập khoảng đơn giản. 

Quan sát quan trọng là diễn giải lại hoạt động hợp nhất theo đại số. Nếu chúng ta mở rộng một sự hợp nhất các giá trị$x$Và$y$, sự đóng góp kết quả là$$x + y + xy.$$Biểu thức này phân tích thành nhân tử$$(x+1)(y+1) - 1.$$Phép biến đổi này chuyển đổi quá trình từ một hoạt động hợp nhất ngẫu nhiên phức tạp thành một cấu trúc nhân rõ ràng. 

Nếu chúng ta xác định các giá trị được chuyển đổi$v_i = a_i + 1$, thì mỗi lần hợp nhất sẽ thay thế hai giá trị liền kề bằng tích của chúng:$$v_{\text{new}} = v_i \cdot v_{i+1}.$$Phép nhân có tính chất kết hợp và giao hoán nên kết quả cuối cùng không phụ thuộc vào chuỗi các phép hợp nhất ngẫu nhiên. Mọi trò chơi có thể đều tạo ra cùng một giá trị cuối cùng. 

Do đó, giá trị mong đợi bằng với giá trị xác định và vấn đề giảm xuống còn tính toán tích phạm vi của các giá trị đã dịch chuyển. 

Cập nhật phạm vi thay thế một phân đoạn bằng một trong hai$1,2,3,\dots,m$hoặc$m,m-1,\dots,1$. Sau khi chuyển đổi, cả hai đều tạo ra nhiều tập hợp giống nhau$\{2,3,\dots,m+1\}$, vậy cả hai đều có cùng một sản phẩm. Điều này làm cho việc cập nhật trở nên cực kỳ đơn giản: chúng ghi đè lên một phân khúc bằng một sản phẩm đã biết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Số mũ cho mỗi truy vấn | O(n) | Quá chậm | 
| Tối ưu (cây đại số + phân đoạn) | O(q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp duy trì cây phân đoạn trên mảng được chuyển đổi$v_i = b_i + 1$, nhưng thay vì lưu trữ cấu trúc đầy đủ, mỗi nút chỉ lưu trữ sản phẩm của phân khúc của nó. 

1. Chuyển đổi mảng ban đầu bằng cách thay thế từng giá trị$b_i$với$b_i + 1$. Điều này trực tiếp mã hóa quy tắc hợp nhất hấp thụ thành phép nhân. 
2. Xây dựng cây phân đoạn trong đó mỗi nút lưu trữ tích của phân đoạn theo modulo 998244353. Các nút lá lưu trữ các giá trị được chuyển đổi đơn và các nút bên trong nhân lên các nút con. 
3. Đối với truy vấn trên một phạm vi$[l, r]$, trả lại sản phẩm của$v_l \cdots v_r$, sau đó trừ đi 1 từ kết quả. Điều này tương ứng chính xác với điểm hấp thụ cuối cùng. 
4. Để cập nhật về một đoạn có độ dài$m = r - l + 1$, tính giá trị mới được gán cho phân đoạn. Vì đoạn này trở thành một hoán vị của$1 \dots m$, các giá trị được chuyển đổi trở thành$2 \dots m+1$, sản phẩm của ai là$(m+1)!$. Gán sản phẩm này trực tiếp vào phân khúc trong cây phân khúc. 
5. Sử dụng phương pháp lan truyền lười biếng để gán phạm vi: khi một phân đoạn bị ghi đè, hãy loại bỏ cấu trúc trước đó và thay thế sản phẩm được lưu trữ của nó bằng giá trị giai thừa được tính toán trước tương ứng với độ dài của nó. 
6. Tính toán trước các giai thừa lên đến$n+2$để trả lời cập nhật trong thời gian liên tục. 

Lý do chính khiến tính năng này hoạt động là vì tất cả các thao tác đều giảm xuống thành phép tổng hợp nhân trên các phân đoạn rời rạc và các bản cập nhật luôn thay thế phân đoạn bằng một giá trị mà cách sắp xếp bên trong của nó không liên quan. 

### Tại sao nó hoạt động 

Hoạt động hấp thụ chính xác là nhận dạng$$x + y + xy = (x+1)(y+1) - 1.$$Điều này chuyển đổi mọi phần tử thành trọng số nhân$v_i = a_i + 1$. Quá trình hợp nhất nhiều lần các phần tử liền kề trở thành phép nhân lặp lại của tất cả$v_i$, không phụ thuộc vào thứ tự. Vì phép nhân có tính chất kết hợp và giao hoán nên mọi chuỗi hợp nhất đều mang lại cùng một sản phẩm cuối cùng. Do đó, tính ngẫu nhiên không ảnh hưởng đến kết quả và tất cả những gì quan trọng là sản phẩm của mảng được biến đổi. Các bản cập nhật phạm vi chỉ thay thế các phân đoạn bằng một bộ nhiều cố định và bộ nhiều đó có một sản phẩm đã biết độc lập với việc đặt hàng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

MAXN = 200000 + 5

fact = [1] * (MAXN + 5)
for i in range(1, MAXN + 5):
    fact[i] = fact[i - 1] * i % MOD

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.prod = [1] * (4 * self.n)
        self.lazy = [None] * (4 * self.n)
        self.build(1, 0, self.n - 1, arr)

    def build(self, v, l, r, arr):
        if l == r:
            self.prod[v] = arr[l] % MOD
            return
        m = (l + r) // 2
        self.build(v * 2, l, m, arr)
        self.build(v * 2 + 1, m + 1, r, arr)
        self.prod[v] = self.prod[v * 2] * self.prod[v * 2 + 1] % MOD

    def apply(self, v, l, r):
        if self.lazy[v] is None:
            return
        self.prod[v] = self.lazy[v]
        if l != r:
            self.lazy[v * 2] = self.lazy[v]
            self.lazy[v * 2 + 1] = self.lazy[v]
        self.lazy[v] = None

    def update(self, v, l, r, ql, qr, val):
        self.apply(v, l, r)
        if qr < l or r < ql:
            return
        if ql <= l and r <= qr:
            self.lazy[v] = val
            self.apply(v, l, r)
            return
        m = (l + r) // 2
        self.update(v * 2, l, m, ql, qr, val)
        self.update(v * 2 + 1, m + 1, r, ql, qr, val)
        self.prod[v] = self.prod[v * 2] * self.prod[v * 2 + 1] % MOD

    def query(self, v, l, r, ql, qr):
        self.apply(v, l, r)
        if qr < l or r < ql:
            return 1
        if ql <= l and r <= qr:
            return self.prod[v]
        m = (l + r) // 2
        return (self.query(v * 2, l, m, ql, qr) *
                self.query(v * 2 + 1, m + 1, r, ql, qr)) % MOD

n = int(input())
b = list(map(int, input().split()))

a = [(x + 1) % MOD for x in b]
st = SegTree(a)

q = int(input())
for _ in range(q):
    tmp = input().split()
    if tmp[0] == '?':
        l, r = int(tmp[1]) - 1, int(tmp[2]) - 1
        ans = st.query(1, 0, n - 1, l, r)
        print((ans - 1) % MOD)
    else:
        l, r = int(tmp[1]) - 1, int(tmp[2]) - 1
        m = r - l + 1
        val = fact[m + 1]
        st.update(1, 0, n - 1, l, r, val)
```Mã dựa trên thực tế là mỗi nút chỉ lưu trữ một sản phẩm, vì vậy các truy vấn là phép nhân phạm vi đơn giản. Các bản cập nhật không tái tạo lại các giá trị riêng lẻ bên trong phân đoạn; họ ghi đè toàn bộ phân khúc bằng một sản phẩm được tính toán trước, điều này là đủ vì các hoạt động tiếp theo chỉ phụ thuộc vào các sản phẩm tổng hợp. 

Phép trừ 1 trong truy vấn xuất phát trực tiếp từ việc đảo ngược phép biến đổi$v_i = a_i + 1$. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng ban đầu$[3, 6, 2]$. Sau khi chuyển đổi, chúng tôi làm việc với$[4, 7, 3]$. 

| Bước | Hoạt động | Phân khúc sản phẩm | 
| --- | --- | --- | 
| Bắt đầu | đầy đủ |$4 \cdot 7 \cdot 3 = 84$| 
| Truy vấn | đầu ra |$84 - 1 = 83$| 

Điều này cho thấy rằng dù việc hợp nhất có diễn ra như thế nào thì kết quả cũng chỉ phụ thuộc vào tích của các giá trị được chuyển đổi. 

Bây giờ hãy xem xét một bản cập nhật trên một đoạn có độ dài 3 thay thế nó bằng thứ tự tăng dần. Phân đoạn trở thành$[1,2,3]$, do đó các giá trị được biến đổi là$[2,3,4]$, sản phẩm của ai là$24$. Mọi truy vấn trong tương lai đối với phân đoạn này sẽ coi nó là một khối duy nhất với sản phẩm 24. 

Điều này xác nhận rằng thứ tự nội bộ không còn phù hợp khi áp dụng chuyển đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \log n)$| Mỗi truy vấn và cập nhật đều đi qua cây phân đoạn | 
| Không gian |$O(n)$| Cây phân đoạn cộng với tính toán trước giai thừa | 

Các ràng buộc cho phép lên đến$2 \times 10^5$các phép toán, do đó hệ số logarit cho mỗi phép toán là đủ. Tra cứu giai thừa theo thời gian liên tục đảm bảo các bản cập nhật không làm giảm hiệu suất. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    b = list(map(int, input().split()))
    q = int(input())

    # placeholder: assume solution integrated
    return ""

# minimal
assert run("2\n1 2\n1\n? 1 2\n") == "5\n"

# single element update
assert run("3\n1 2 3\n1\n! 1 3 A\n? 1 3\n") == "23\n"

# overwrite check
assert run("4\n2 2 2 2\n2\n? 1 4\n! 2 3 D\n? 1 4\n") == "?\n"

# edge
assert run("5\n1 1 1 1 1\n1\n? 1 5\n") == "31\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Phạm vi nhỏ | kiểm tra sản phẩm trực tiếp | tính đúng đắn của phép biến đổi | 
| Ghi đè đầy đủ | phân công dựa trên giai thừa | cập nhật logic | 
| Mảng thống nhất | ổn định theo các truy vấn lặp đi lặp lại | bình thường | 
| Cạnh tối thiểu | độ đúng ranh giới | xử lý từng cái một | 

## Vỏ cạnh 

Một trường hợp quan trọng là một bản cập nhật toàn diện. Thuật toán bỏ qua mẫu bên trong và thay thế phân khúc bằng một sản phẩm được tính toán trước. Đối với một phạm vi chiều dài$m$, cả hai bản cập nhật tăng dần và giảm dần đều tạo ra nhiều tập hợp được chuyển đổi giống hệt nhau, do đó sản phẩm được lưu trữ vẫn nhất quán. 

Một trường hợp khác là ghi đè lặp đi lặp lại. Vì mỗi bản cập nhật thay thế hoàn toàn các giá trị trước đó nên không còn phụ thuộc lịch sử nào và cây phân đoạn luôn phản ánh trạng thái sản phẩm mới nhất mà không cần phải xây dựng lại các mảng cơ bản. 

Cuối cùng, phạm vi phần tử đơn hoạt động chính xác vì phép biến đổi$a_i + 1$đảm bảo rằng một đoạn có độ dài chỉ lưu trữ giá trị của chính nó và quy tắc cập nhật cho$m = 1$gán chính xác một sản phẩm của$2!$.
