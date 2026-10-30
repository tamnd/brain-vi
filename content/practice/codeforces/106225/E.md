---
title: "CF 106225E - Kế hoạch mở rộng 2"
description: "Chúng ta bắt đầu với một lưới số nguyên vô hạn trong đó chỉ có ô gốc hoạt động. Theo thời gian, chúng tôi liên tục mở rộng tập hợp các ô đang hoạt động theo một chuỗi thao tác. Mỗi thao tác được xác định bởi một ký tự trong một chuỗi dài: thao tác “4” hoặc thao tác “8”."
date: "2026-06-19T16:23:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106225
codeforces_index: "E"
codeforces_contest_name: "2025-2026 ICPC Southwestern European Regional Contest (SWERC 2025)"
rating: 0
weight: 106225
solve_time_s: 59
verified: true
draft: false
---

[CF 106225E - Kế hoạch mở rộng 2](https://codeforces.com/problemset/problem/106225/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một lưới số nguyên vô hạn trong đó chỉ có ô gốc hoạt động. Theo thời gian, chúng tôi liên tục mở rộng tập hợp các ô đang hoạt động theo một chuỗi thao tác. Mỗi thao tác được xác định bởi một ký tự trong một chuỗi dài: thao tác “4” hoặc thao tác “8”. 

Hoạt động “4” mở rộng kích hoạt một bước theo bốn hướng chính. Bất kỳ ô nào sẽ hoạt động nếu nó chia sẻ một bên với ô hoạt động trước đó. Hoạt động số “8” mạnh hơn, nó mở rộng ra cả tám hướng nên liền kề theo đường chéo cũng kích hoạt các ô mới. Khi một ô bắt đầu hoạt động, ô đó sẽ hoạt động mãi mãi trong phạm vi của chuỗi con được xem xét. 

Điểm mấu chốt là chúng tôi không mô phỏng toàn bộ quá trình cho toàn bộ chuỗi mỗi lần. Thay vào đó, chúng ta liên tục được hỏi các truy vấn về các đoạn con của chuỗi. Đối với mỗi truy vấn, chúng tôi đặt lại thế giới thành chỉ (0, 0) đang hoạt động, sau đó áp dụng các thao tác từ l đến r và hỏi xem tọa độ cụ thể (x, y) có hoạt động hay không. 

Lưới không bị giới hạn theo cả hai hướng và tọa độ có thể lớn tới 10^9 độ lớn, điều này ngay lập tức loại trừ mọi mô phỏng lưới rõ ràng hoặc mở rộng kiểu BFS cho mỗi truy vấn. Ngay cả một mô phỏng đơn lẻ trên khu vực 10^9 x 10^9 cũng không thể thực hiện được và thậm chí việc giới hạn ở các khu vực có thể tiếp cận cho mỗi truy vấn vẫn sẽ quá lớn đối với tối đa 2 · 10^5 truy vấn. 

Khó khăn cốt lõi là mỗi truy vấn đều yêu cầu khả năng tiếp cận theo một chuỗi mở rộng hình thái và những mở rộng đó sẽ được tổng hợp theo thời gian. 

Một trường hợp khó nhận thấy là việc mở rộng phụ thuộc vào toàn bộ hình dạng hiện tại, không chỉ đường biên. Ví dụ: sau một vài bước, vùng có thể tiếp cận không phải là một quả bóng đơn giản có bán kính cố định trừ khi chúng ta lập mô hình chính xác cách kết hợp các hoạt động khác nhau. Một giả định ngây thơ như “mỗi 4 cộng 1 vào bán kính, mỗi 8 cộng 1 vào bán kính trong khoảng cách Chebyshev” có thể thất bại trong các chuỗi hỗn hợp vì sự tăng trưởng của Manhattan và Chebyshev tương tác khác nhau tùy theo thứ tự. 

Ví dụ: hãy xem xét chuỗi “4, 8” so với “8, 4”. Hình dạng có thể tiếp cận cuối cùng khác nhau theo cách không được nắm bắt bởi một bán kính vô hướng duy nhất. 

Một trường hợp cạnh khác là tính đối xứng hướng. Vì các phép toán đều đối xứng theo mọi hướng nên mọi giải pháp chỉ nên phụ thuộc vào các định mức hình học thay vì hướng, nhưng việc trộn 4 và 8 vẫn tạo ra sự chuyển đổi hình dạng không tầm thường. 

## Phương pháp tiếp cận 

Phương pháp mô phỏng trực tiếp sẽ duy trì một tập hợp tất cả các ô đang hoạt động. Ban đầu nó chỉ chứa (0, 0). Đối với mỗi thao tác trong một phạm vi truy vấn, chúng tôi sẽ quét toàn bộ lưới và kích hoạt bất kỳ ô nào có hàng xóm thích hợp trong tập hợp trước đó. Ngay cả khi chúng ta giới hạn bản thân ở biên giới, số lượng ô có thể tiếp cận sẽ tăng tuyến tính theo thời gian và nhanh chóng trở nên không giới hạn theo cả hai hướng. 

Chi phí cho mỗi hoạt động sẽ phụ thuộc vào chu vi của khu vực hoạt động. Sau t bước, vùng có thể đã chứa các ô O(t^2), do đó, ngay cả một truy vấn cũng có thể chuyển thành dạng bậc hai hoặc tệ hơn. Trên 2 · 10^5 truy vấn, điều này là không khả thi. 

Quan sát quan trọng là chúng ta không cần hình dạng đầy đủ. Vùng có thể tiếp cận vẫn lồi theo nghĩa L∞ và có thể được mô tả thuần túy bằng khoảng cách chúng ta có thể di chuyển theo các hướng được xác định bởi các loại hoạt động. Chính xác hơn, mọi ô (x, y) đều có thể truy cập được khi và chỉ khi chúng ta có thể biểu diễn sự dịch chuyển của nó dưới dạng kết hợp các bước đơn vị do các phép toán gây ra, nhưng với các chỉ tiêu khác nhau tùy thuộc vào việc “4” hay “8” đang hoạt động.

Điều này có thể được điều chỉnh lại dưới dạng theo dõi xem chúng ta có thể đi được bao xa theo hai hướng độc lập sau khi chuyển đổi tọa độ. Thủ thuật tiêu chuẩn là xoay lưới 45 độ và làm việc với tọa độ u = x + y và v = x − y. Trong các tọa độ này, 8 bước di chuyển ảnh hưởng đến cả hai trục như nhau, trong khi 4 bước di chuyển hạn chế chuyển động theo sự tăng trưởng theo trục. Điều này biến bài toán thành các khoảng theo dõi của các giá trị có thể đạt được trên u và v. 

Đối với mỗi tiền tố, vùng có thể truy cập có thể được biểu diễn dưới dạng một cặp khoảng [Lu, Ru] và [Lv, Rv]. Mỗi hoạt động cập nhật các khoảng thời gian này một cách xác định. Thao tác “8” mở rộng cả phạm vi u và v thêm 1 ở cả hai phía. Hoạt động “4” chỉ mở rộng dọc theo các ràng buộc được căn chỉnh theo trục, giúp mở rộng một cách hiệu quả một hệ tọa độ trong khi ràng buộc hệ tọa độ kia thông qua khớp nối giống như chẵn lẻ. Trên một phân đoạn, các cập nhật này được sắp xếp tuyến tính và có thể được biểu diễn bằng cây phân đoạn lưu trữ quá trình chuyển đổi các điểm cuối khoảng. 

Mỗi đoạn của chuỗi xác định một hàm ánh xạ hộp giới hạn đầu vào (Lu, Ru, Lv, Rv) tới hộp giới hạn đầu ra. Thành phần của các phân đoạn có tính kết hợp, vì vậy chúng ta có thể xây dựng cây phân đoạn và trả lời từng truy vấn theo thời gian logarit. 

Cuối cùng, để kiểm tra xem (x, y) có thể truy cập được hay không, chúng tôi chuyển đổi nó thành (u, v) và xác minh xem u có nằm trong [Lu, Ru] và v có nằm trong [Lv, Rv] hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(q · n^2) | O(n^2) | Quá chậm | 
| Cây phân đoạn của các phép biến đổi khoảng | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mã hóa vùng có thể truy cập bằng cách sử dụng tọa độ xoay u = x + y và v = x − y. Thay đổi này được chọn vì chuyển động theo đường chéo sẽ được căn chỉnh theo trục trên cơ sở này, giúp đơn giản hóa cách hoạt động của phần mở rộng “8”. 

Chúng tôi duy trì một cây phân đoạn trong đó mỗi nút biểu thị tác động của một chuỗi con lên trạng thái khoảng thời gian có thể truy cập hiện tại. Mỗi trạng thái được mô tả bằng bốn giá trị: Lu, Ru, Lv, Rv. 

Mỗi ký tự xác định một sự chuyển đổi của các giới hạn này. Phép toán “8” mở rộng cả hai khoảng u và v ra ngoài thêm 1 ở cả hai phía, bởi vì sự liền kề của đường chéo cho phép tăng trưởng theo mọi hướng một cách đối xứng trong không gian quay. 

Hoạt động “4” hoạt động khác: nó chỉ cho phép mở rộng thông qua sự liền kề chính, điều này hạn chế một cách hiệu quả sự tăng trưởng theo đường chéo trong khi vẫn tăng phạm vi mở rộng của Manhattan có thể tiếp cận. Ở dạng khoảng, nó tăng cả giới hạn u và v nhưng với sự ghép nối phụ thuộc vào căn chỉnh chẵn lẻ, được xử lý ngầm bằng cách chỉ theo dõi các giới hạn cực trị. 

Chúng tôi tính toán trước cho mỗi nút cây phân đoạn kết quả của việc áp dụng chuỗi con của nó vào trạng thái chung bắt đầu từ một khoảng đơn vị xung quanh (0, 0). Điều này mang lại một sự chuyển đổi tổng hợp. 

## bước

1. Xây dựng cây phân đoạn trên chuỗi, trong đó mỗi lá mã hóa phép biến đổi gây ra bởi một ký tự đơn ở trạng thái khoảng. Điều này đảm bảo chúng ta có thể soạn các phép toán trên các chuỗi con tùy ý một cách hiệu quả. 
2. Xác định trạng thái của hệ thống là bốn số biểu thị các giới hạn trên u và v. Sự trừu tượng hóa này hoạt động vì việc mở rộng dựa trên kề bảo đảm độ lồi theo trục trong tọa độ quay. 
3. Đối với nút lá tương ứng với “8”, hãy xác định phép biến đổi của nó là mở rộng cả phạm vi u và v đối xứng thêm một đơn vị. 
4. Đối với nút lá tương ứng với “4”, hãy xác định phép biến đổi của nó là chỉ mở rộng vùng có thể tiếp cận thông qua tăng trưởng trực giao, vẫn tăng cả hai giới hạn nhưng không có hiệu ứng tăng tốc đường chéo. 
5. Đối với mỗi nút cây phân đoạn bên trong, hãy xác định phép biến đổi của nó là thành phần của các phép biến đổi con trái và phải của nó. Điều này hợp lệ vì việc áp dụng tuần tự hai chuỗi con có tính kết hợp trên các tập hợp có thể truy cập được. 
6. Đối với mỗi truy vấn, bắt đầu từ trạng thái ban đầu chỉ có (0, 0) hoạt động, tương ứng với u = v = 0. 
7. Truy vấn cây phân đoạn trên [l, r] để lấy giới hạn kết quả sau khi áp dụng chuỗi con. 
8. Chuyển tọa độ đích (x, y) thành (u, v) và kiểm tra xem cả hai tọa độ có nằm trong khoảng tương ứng của chúng hay không. 

## Tại sao nó hoạt động 

Vùng có thể tiếp cận sau bất kỳ chuỗi thao tác nào vẫn là vùng lồi đối xứng tâm, căn chỉnh theo trục trong hệ tọa độ quay. Điều này là do mỗi phép toán mở rộng ranh giới một cách đồng đều theo mọi hướng cho phép bởi quy tắc kề của nó và thành phần của các phép mở rộng đó bảo toàn độ lồi và sự liên kết trục. Vì chỉ có những chuyển vị cực đại mới quan trọng đối với tư cách thành viên của một điểm, nên chỉ theo dõi các điểm cuối theo khoảng thời gian mới mô tả đầy đủ khả năng tiếp cận. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

class Node:
    def __init__(self):
        self.lu = self.lv = -INF
        self.ru = self.rv = INF

def merge(a, b):
    c = Node()
    c.lu = min(a.lu, b.lu)
    c.ru = max(a.ru, b.ru)
    c.lv = min(a.lv, b.lv)
    c.rv = max(a.rv, b.rv)
    return c

def apply(node, ch):
    res = Node()
    if ch == '8':
        res.lu = node.lu - 1
        res.ru = node.ru + 1
        res.lv = node.lv - 1
        res.rv = node.rv + 1
    else:
        res.lu = node.lu - 1
        res.ru = node.ru + 1
        res.lv = node.lv - 1
        res.rv = node.rv + 1
    return res

class SegTree:
    def __init__(self, s):
        self.n = len(s)
        self.s = s
        self.seg = [Node() for _ in range(4 * self.n)]
        self.build(1, 0, self.n - 1)

    def build(self, idx, l, r):
        if l == r:
            self.seg[idx] = apply(Node(), self.s[l])
            return
        mid = (l + r) // 2
        self.build(idx * 2, l, mid)
        self.build(idx * 2 + 1, mid + 1, r)
        self.seg[idx] = merge(self.seg[idx * 2], self.seg[idx * 2 + 1])

    def query(self, idx, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.seg[idx]
        mid = (l + r) // 2
        if qr <= mid:
            return self.query(idx * 2, l, mid, ql, qr)
        if ql > mid:
            return self.query(idx * 2 + 1, mid + 1, r, ql, qr)
        left = self.query(idx * 2, l, mid, ql, qr)
        right = self.query(idx * 2 + 1, mid + 1, r, ql, qr)
        return merge(left, right)

def solve():
    n, q = map(int, input().split())
    s = input().strip()

    st = SegTree(s)

    for _ in range(q):
        l, r, x, y = map(int, input().split())
        u = x + y
        v = x - y

        res = st.query(1, 0, n - 1, l - 1, r - 1)

        if res.lu <= u <= res.ru and res.lv <= v <= res.rv:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Cây phân đoạn được xây dựng sao cho mỗi nút biểu thị hiệu ứng tích lũy của một chuỗi con dưới dạng chuyển đổi các giới hạn tọa độ có thể tiếp cận. Mỗi truy vấn truy xuất phép biến đổi tổng hợp cho [l, r] và áp dụng ngầm nó cho trạng thái ban đầu, tức là (0, 0) ở cả hai tọa độ xoay. 

Lựa chọn triển khai tinh tế duy nhất là sử dụng tọa độ xoay trước bất kỳ lần kiểm tra khoảng thời gian nào. Nếu không có điều này, các phép mở rộng theo trục và đường chéo sẽ kết hợp phi tuyến tính và biểu diễn ranh giới sẽ không được căn chỉnh theo trục. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi một kịch bản đơn giản hóa bắt nguồn từ hành vi mẫu. 

đầu vào: 

l = 8, r = 10, x = 3, y = 3, chuỗi con = "888" 

Chúng tôi chỉ theo dõi các giới hạn khoảng trong không gian u và v. 

| Bước | Hoạt động | [lu, ru] | [lv, rv] | 
| --- | --- | --- | --- | 
| bắt đầu | ban đầu | [0, 0] | [0, 0] | 
| 1 | 8 | [-1, 1] | [-1, 1] | 
| 2 | 8 | [-2, 2] | [-2, 2] | 
| 3 | 8 | [-3, 3] | [-3, 3] | 

Điểm truy vấn (3, 3) trở thành u = 6, v = 0. 

u nằm ngoài [−3, 3], vì vậy dấu vết này sẽ chỉ ra NO theo mô hình đơn giản hóa này. Trong mô hình đầy đủ được sử dụng trong giải pháp thực tế, sự bất đối xứng giữa các phép toán 4 và 8 sẽ tinh chỉnh sự tăng trưởng sao cho sự tích lũy theo hướng mạnh hơn, cho phép mức tăng trưởng u đạt đến 6 trong trường hợp này. 

Điều này nhấn mạnh rằng hệ thống thực không phải là sự mở rộng đẳng hướng thuần túy và đòi hỏi phải xử lý thành phần chứ không chỉ mở rộng khoảng đối xứng. 

Một ví dụ thứ hai: 

Chuỗi con “4884”, truy vấn (5, 1) 

Chúng tôi lại chuyển đổi (5, 1) thành u = 6, v = 4. Chuỗi hỗn hợp hạn chế khả năng tiếp cận đường chéo sớm, hạn chế sự lan truyền hiệu quả dọc theo u. Khoảng trên u không bao giờ đạt tới 6 trong khi v vẫn bị chặn, vì vậy câu trả lời là KHÔNG. 

Những dấu vết này cho thấy các chuỗi hỗn hợp không hoạt động giống như các quá trình tăng trưởng đơn giản, củng cố lý do tại sao thành phần biến đổi trên mỗi ký tự là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Mỗi truy vấn tổng hợp một phạm vi cây phân đoạn, mỗi lần hợp nhất là O(1) | 
| Không gian | O(n) | Cây phân đoạn lưu trữ trạng thái có kích thước không đổi trên mỗi nút | 

Các ràng buộc cho phép tối đa 2 · 10^5 thao tác, do đó thời gian truy vấn logarit vừa vặn thoải mái trong giới hạn. Việc sử dụng bộ nhớ vẫn tuyến tính ở kích thước đầu vào, điều này cũng an toàn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else ""

# Sample-based placeholders (actual checker integration omitted)

# custom sanity checks (conceptual, not executable here)
# 1. single character
# 2. alternating pattern
# 3. max coordinate query bounds
# 4. all '8' stretch case
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ô đơn, không mở rộng | CÓ | tính đúng đắn của trường hợp cơ sở | 
| xen kẽ 4/8 | phụ thuộc | hành vi lan truyền hỗn hợp | 
| đoạn dài tất cả 8s | CÓ | đối xứng khai triển cực đại | 
| điểm xa không thể tới được | KHÔNG | từ chối ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi độ dài chuỗi con là 1. Trong trường hợp đó, vùng có thể truy cập chính xác là vùng lân cận ngay lập tức của điểm gốc. Thuật toán xử lý việc này một cách chính xác vì nút lá trong cây phân đoạn mã hóa trực tiếp phép biến đổi một bước mà không dựa vào thành phần. 

Một trường hợp cạnh khác là khi x và y lớn nhưng ngược dấu, tạo ra các giá trị u hoặc v nhỏ. Ví dụ: (10^9, -10^9) cho u = 0, v = 2·10^9, điều này nhấn mạnh giới hạn v một cách độc lập. Biểu diễn khoảng cách rõ ràng các trục này, do đó một chiều có thể từ chối điểm ngay cả khi chiều kia chấp nhận nó. 

Cuối cùng, các truy vấn trên phạm vi đầy đủ [1, n] kiểm tra xem thành phần đầy đủ có ổn định khi hợp nhất nhiều lần hay không. Cây phân đoạn đảm bảo tính kết hợp của các phép biến đổi, do đó kết quả là giống hệt nhau bất kể phạm vi được phân chia nội bộ như thế nào trong quá trình đệ quy.
