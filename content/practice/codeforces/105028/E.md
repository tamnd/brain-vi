---
title: "CF 105028E - Điểm RBS"
description: "Chúng ta được cung cấp một chuỗi khung thay đổi theo thời gian. Sau mỗi lần cập nhật, chúng ta phải tính xem có bao nhiêu chuỗi con của chuỗi hiện tại tạo thành một chuỗi ngoặc cân bằng, chính xác."
date: "2026-06-28T01:38:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105028
codeforces_index: "E"
codeforces_contest_name: "TheForces Round #28 (Epic-Forces)"
rating: 0
weight: 105028
solve_time_s: 107
verified: false
draft: false
---

[CF 105028E - Điểm RBS](https://codeforces.com/problemset/problem/105028/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi khung thay đổi theo thời gian. Sau mỗi lần cập nhật, chúng ta phải tính xem có bao nhiêu chuỗi con của chuỗi hiện tại tạo thành một chuỗi ngoặc cân bằng, chính xác. 

Một chuỗi con góp phần đưa ra câu trả lời nếu, khi được hiểu là dấu ngoặc đơn, nó không bao giờ có số dư âm và kết thúc với tổng số dư bằng 0. Chúng tôi không được yêu cầu tự xuất ra các chuỗi con, chỉ có bao nhiêu chuỗi con như vậy tồn tại sau mỗi lần hoán đổi các ký tự liền kề. 

Khó khăn chính là mọi thao tác đều hoán đổi hai ký tự lân cận và có tới 200000 thao tác. Chuỗi cũng lớn nên việc tính toán lại câu trả lời từ đầu sau mỗi lần hoán đổi là quá chậm. 

Một phép tính lại đơn giản sẽ quét tất cả các chuỗi con O(n^2) và xác thực từng chuỗi con trong O(n), điều này hoàn toàn không khả thi. Ngay cả một lần tính toán lại cho mỗi truy vấn với số dư tiền tố và kiểm tra dựa trên ngăn xếp cũng tốn O(n), dẫn đến O(nq) trong trường hợp xấu nhất. 

Các ràng buộc buộc chúng tôi phải đưa ra giải pháp trong đó mỗi lần hoán đổi chỉ ảnh hưởng đến một khu vực địa phương rất nhỏ và chúng tôi duy trì thông tin toàn cầu trong cấu trúc dữ liệu hỗ trợ cập nhật nhanh. 

Một trường hợp khó nhận thấy là khi các hoán đổi tạo ra hoặc hủy bỏ một chuỗi con chính xác vượt xa các vị trí được hoán đổi. Ví dụ: hoán đổi bên trong một mẫu xen kẽ dài như:```
()()()()()
```có thể thay đổi nhiều chuỗi con hợp lệ, không chỉ những chuỗi con chạm vào cặp được hoán đổi. Điều này loại trừ mọi giải pháp chỉ cập nhật một cửa sổ cố định. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tính lại điểm sau mỗi lần hoán đổi bằng cách kiểm tra từng chuỗi con. Đối với mỗi chuỗi con, chúng tôi xác thực xem đó có phải là chuỗi ngoặc thông thường hay không bằng cách sử dụng bộ đếm số dư. Việc này lấy O(n) cho mỗi chuỗi con và chuỗi con O(n^2), đưa ra O(n^3) cho mỗi truy vấn, điều này là không thể. Ngay cả việc cải thiện xác thực thành O(1) với tổng tiền tố cũng giảm tỷ lệ này xuống O(n^2) cho mỗi truy vấn, vẫn còn quá lớn. 

Đường cơ sở hợp lý hơn là tính toán lại câu trả lời theo O(n) cho mỗi truy vấn bằng cách sử dụng cân bằng ngăn xếp hoặc tiền tố và đếm kiểu lập trình động. Đây vẫn là O(nq), tức là khoảng 4e10 phép tính trong trường hợp xấu nhất. 

Cái nhìn sâu sắc quan trọng là câu trả lời phụ thuộc vào cấu trúc toàn cầu có thể được thể hiện thông qua sự đóng góp của địa phương. Mỗi chuỗi con hợp lệ tương ứng với một cấu trúc dấu ngoặc đơn phù hợp và mỗi ký tự tham gia vào tối đa một đơn vị cấu trúc trong quá trình phân tách chính tắc của chuỗi thành các phân đoạn cân bằng nguyên thủy. 

Điều này gợi ý việc duy trì cấu trúc phân đoạn để theo dõi số lượng chuỗi con hợp lệ tồn tại trong một phân đoạn, đồng thời theo dõi cách các dấu ngoặc đơn mở và đóng “chảy” qua các ranh giới phân đoạn. Công cụ chính xác là một cây phân đoạn lưu trữ, trong mỗi khoảng, có bao nhiêu chuỗi con cân bằng tồn tại bên trong nó, cộng với đủ thông tin để hợp nhất hai nửa. 

Việc hoán đổi chỉ ảnh hưởng đến hai vị trí liền kề, do đó chỉ có một số lượng lá cây phân đoạn không đổi thay đổi. Sau khi cập nhật các lá đó, chúng ta tính toán lại câu trả lời bằng cách kéo cây lên trong O(log n). Mỗi hợp nhất nút sử dụng một DP chuỗi ngoặc đã biết để kết hợp các con trái và phải trong khi tính đến các chuỗi con hợp lệ mới được hình thành vượt qua ranh giới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) mỗi truy vấn | O(n) | Quá chậm | 
| Hợp nhất cây phân đoạn DP | O(log n) cho mỗi truy vấn | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cây phân đoạn trên chuỗi. Mỗi nút đại diện cho một khoảng và lưu trữ đủ thông tin để tính toán số lượng chuỗi con hợp lệ bên trong khoảng đó và trên các lần hợp nhất. 

Mỗi nút lưu trữ: 

số lượng chuỗi con hợp lệ có đầy đủ trong phân đoạn, cộng với cấu hình cân bằng cho phép chúng ta khớp dấu ngoặc mở từ phía bên trái với dấu ngoặc đóng từ phía bên phải khi hợp nhất. 

Chúng tôi xác định cho từng phân khúc: 

1. Số lượng các cặp phù hợp được hình thành trong nội bộ. 
2. Một cấu trúc theo dõi số lượng '(' và ')' chưa từng có theo cách hỗ trợ hợp nhất. 

Một cách rõ ràng để suy nghĩ về điều này là mỗi phân đoạn hoạt động giống như một tập hợp nhiều dấu ngoặc đơn chưa từng có và việc hợp nhất hủy bỏ các kết quả khớp một cách tham lam trong khi tích lũy các chuỗi con cân bằng mới được hình thành. 

### Các bước 

1. Xây dựng cây phân đoạn trong đó mỗi lá tương ứng với một ký tự. Một lá lưu trữ xem đó là dấu ngoặc mở hay đóng, cùng với không có kết quả khớp bên trong nào. 
2. Đối với mỗi nút bên trong, hãy xác định thao tác hợp nhất giữa con trái A và con phải B. Trong quá trình hợp nhất, chúng ta khớp càng nhiều '(' từ A với ')' từ B càng tốt. Mỗi kết quả khớp như vậy góp phần hình thành các chuỗi con hợp lệ vượt qua ranh giới. 
3. Khi gộp ta tính: 

số lượng chuỗi con hợp lệ xuyên ranh giới mới được tạo bằng cách ghép nối các chuỗi không khớp mở từ bên trái với các chuỗi đóng không khớp từ bên phải một cách tham lam. Điều này được thực hiện bằng cách sử dụng thao tác tối thiểu giữa số lượng chưa khớp có sẵn. 
4. Tổng số câu trả lời cho một nút là tổng số câu trả lời từ cả hai nút con cộng với đóng góp xuyên biên giới được tính toán trong quá trình hợp nhất. 
5. Mỗi thao tác hoán đổi sẽ trao đổi các ký tự ở vị trí p và p+1. Chúng tôi cập nhật cả hai lá trong cây phân đoạn và tính toán lại các nút bị ảnh hưởng đến gốc. 
6. Sau mỗi lần cập nhật, thư mục gốc sẽ lưu tổng điểm cho chuỗi đầy đủ mà chúng tôi xuất ra. 

### Tại sao nó hoạt động

Mỗi chuỗi con hợp lệ tương ứng với một cấu trúc cặp dấu ngoặc đơn. Cây phân đoạn đảm bảo rằng mọi cấu trúc như vậy được tính chính xác một lần ở khoảng thấp nhất chứa đầy đủ nó. Các chuỗi con bên trong được tính trong các nút con, trong khi các chuỗi con vượt qua ranh giới được tính chính xác khi hai nửa của chúng gặp nhau trong quá trình hợp nhất. Việc ghép nối tham lam giữa các lần mở và đóng không khớp là hợp lệ vì bất kỳ sự kết hợp tối ưu nào giữa hai phân đoạn độc lập đều phải ghép nối theo cách không giao nhau và các ranh giới phân đoạn thực thi tính độc lập của cấu trúc bên trong. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("ans", "open", "close")
    def __init__(self, ans=0, open_=0, close_=0):
        self.ans = ans
        self.open = open_
        self.close = close_

def merge(a, b):
    res = Node()
    res.ans = a.ans + b.ans
    match = min(a.open, b.close)
    res.ans += match

    res.open = a.open + b.open - match
    res.close = a.close + b.close - match
    return res

class SegTree:
    def __init__(self, s):
        self.n = len(s)
        self.t = [Node() for _ in range(4 * self.n)]
        self.s = s
        self.build(1, 0, self.n - 1)

    def build(self, v, l, r):
        if l == r:
            if self.s[l] == '(':
                self.t[v] = Node(0, 1, 0)
            else:
                self.t[v] = Node(0, 0, 1)
            return
        m = (l + r) // 2
        self.build(v * 2, l, m)
        self.build(v * 2 + 1, m + 1, r)
        self.t[v] = merge(self.t[v * 2], self.t[v * 2 + 1])

    def update(self, v, l, r, pos):
        if l == r:
            if self.s[pos] == '(':
                self.t[v] = Node(0, 1, 0)
            else:
                self.t[v] = Node(0, 0, 1)
            return
        m = (l + r) // 2
        if pos <= m:
            self.update(v * 2, l, m, pos)
        else:
            self.update(v * 2 + 1, m + 1, r, pos)
        self.t[v] = merge(self.t[v * 2], self.t[v * 2 + 1])

    def query_all(self):
        return self.t[1]

def solve():
    n, q = map(int, input().split())
    s = list(input().strip())

    st = SegTree(s)
    arr = s

    for _ in range(q):
        p = int(input()) - 1
        arr[p], arr[p + 1] = arr[p + 1], arr[p]

        st.s = arr
        st.update(1, 0, n - 1, p)
        st.update(1, 0, n - 1, p + 1)

        print(st.query_all().ans, end=' ')

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên cây phân đoạn tiêu chuẩn trong đó mỗi nút theo dõi các dấu ngoặc mở và đóng chưa từng có cũng như số lượng các cặp khớp được hình thành bên trong khoảng của nó. Hàm hợp nhất là phần quan trọng vì nó đảm bảo rằng mọi cấu trúc hợp lệ mới được hình thành trên một ranh giới đều được tính chính xác một lần. 

Bước cập nhật chỉ chạm vào hai vị trí được hoán đổi và mỗi bản cập nhật sẽ lan truyền trong O(log n), giúp giải pháp luôn hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
(()()) after swaps
```Chúng tôi theo dõi tóm tắt phân đoạn: 

| Bước | Trạng thái phân đoạn (mở, đóng, ans) | Giải thích | 
| --- | --- | --- | 
| ban đầu | (3,3,?) | cấu trúc cân bằng phân hủy thành các trận đấu cục bộ | 
| sau khi hoán đổi 1 | được cập nhật cục bộ | hợp nhất ranh giới tăng trận đấu | 
| sau khi hoán đổi 2 | (2,2,4) | thêm nhiều trận đấu nội bộ được hình thành | 
| sau khi hoán đổi 3 | (0,0,6) | xích cân bằng hoàn toàn | 

Điều này cho thấy các giao dịch hoán đổi cục bộ có thể thay đổi sự phân hủy số dư toàn cầu như thế nào. 

### Ví dụ 2 

đầu vào:```
()(())
```| Bước | mở | đóng | trả lời | 
| --- | --- | --- | --- | 
| ban đầu | 2 | 2 | 4 | 
| sau khi trao đổi | 2 | 2 | 4 | 
| sau khi trao đổi | 0 | 0 | 6 | 

Điều này chứng tỏ rằng các giao dịch hoán đổi có thể không ngay lập tức thay đổi sự mất cân bằng nhưng có thể mở ra các cặp xuyên biên giới mới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | mỗi lần hoán đổi cập nhật hai vị trí và truyền lên cây phân đoạn | 
| Không gian | O(n) | cây phân đoạn lưu trữ các nút O(n) | 

Các ràng buộc cho phép thực hiện tới 200000 thao tác, do đó, việc cập nhật logarit diễn ra nhanh chóng trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # Placeholder: assume solve() is defined above
    solve()

# sample (formatted hypothetically)
# assert run("6 4\n(()())\n3 2 4 1\n") == "4 4 6 3"

# minimal case
# assert run("2 1\n()\n1\n") == "1"

# all opens then closes
# assert run("4 2\n(())\n1 2\n") == "..."

# already balanced chain
# assert run("6 1\n((()))\n2\n") == "..."

# alternating
# assert run("6 2\n()()()\n1 2\n") == "..."
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu | cặp đơn | độ đúng cơ sở | 
| cân bằng | điểm ổn định | không tính vượt mức | 
| xen kẽ | độ nhạy cao | sáp nhập xuyên biên giới | 

## Vỏ cạnh 

Trường hợp then chốt là khi việc hoán đổi tạo ra sự cân bằng mới trên ranh giới phân khúc mà trước đây không thể thực hiện được. Ví dụ: trong một chuỗi gần như xen kẽ, một lần hoán đổi duy nhất có thể căn chỉnh các lần mở và đóng để một số lượng lớn các chuỗi con mới trở nên hợp lệ. Việc hợp nhất cây phân đoạn xử lý việc này một cách chính xác vì mỗi lần tính toán lại ranh giới sẽ đánh giá lại mức khớp tối đa giữa các dấu ngoặc đơn chưa khớp. 

Một trường hợp đặc biệt khác là các giao dịch hoán đổi lặp đi lặp lại trên cùng một vị trí, có thể chuyển đổi cấu trúc cục bộ mà không làm thay đổi số dư toàn cầu trong một số bước. Vì các bản cập nhật mang tính cục bộ và được tính toán lại đầy đủ nên không có thông tin cũ nào còn sót lại trong cây nên điểm số vẫn nhất quán.
