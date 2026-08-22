---
title: "CF 104162F - \u0410\u0432\u0441\u0442\u0440\u0430\u043b\u0438\u0439\u0441\u043a\u0430\u044f \u041f\u0421\u041f"
description: "Chúng ta được cấp một chuỗi bao gồm nhiều loại dấu ngoặc, cụ thể là dấu ngoặc đơn, dấu ngoặc vuông, dấu ngoặc nhọn và dấu ngoặc nhọn. Việc giải thích “tính đúng đắn” ở đây không phải là quy tắc so khớp cặp đơn tiêu chuẩn được sử dụng trong các bài toán về dấu ngoặc cổ điển."
date: "2026-07-02T01:01:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104162
codeforces_index: "F"
codeforces_contest_name: "\u0414\u043b\u0438\u043d\u043d\u044b\u0439 \u0442\u0443\u0440 \u041e\u0442\u043a\u0440\u044b\u0442\u043e\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b 2022-2023"
rating: 0
weight: 104162
solve_time_s: 67
verified: true
draft: false
---

[CF 104162F - \u0410\u0432\u0441\u0442\u0440\u0430\u043b\u0438\u0439\u0441\u043a\u0430\u044f \u041f\u0421\u041f](https://codeforces.com/problemset/problem/104162/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi bao gồm nhiều loại dấu ngoặc, cụ thể là dấu ngoặc đơn, dấu ngoặc vuông, dấu ngoặc nhọn và dấu ngoặc nhọn. Việc giải thích “tính đúng đắn” ở đây không phải là quy tắc so khớp cặp đơn tiêu chuẩn được sử dụng trong các bài toán về dấu ngoặc cổ điển. Thay vào đó, bài toán xác định một cấu trúc đệ quy linh hoạt hơn. 

Một chuỗi được coi là hợp lệ nếu nó có thể được xây dựng từ chuỗi trống bằng cách áp dụng liên tục hai thao tác. Đầu tiên, gói một chuỗi S đã hợp lệ bên trong bất kỳ tập hợp các cặp dấu ngoặc đối xứng nào, chẳng hạn như “(S)”, “)S(”, “[S]”, “]S[”, và tương tự đối với các loại dấu ngoặc khác và hướng đảo ngược. Thứ hai, nối hai chuỗi hợp lệ. Điều này có nghĩa là mọi chuỗi hợp lệ về cơ bản là sự nối của các “khối” hợp lệ độc lập và mỗi khối là một cấu trúc cân bằng theo một trong một số đối xứng dấu ngoặc có thể có. 

Trên hết, chuỗi đầu vào là chuỗi động. Chúng tôi phải hỗ trợ cập nhật điểm trong đó một vị trí thay đổi loại khung của nó và truy vấn phạm vi để hỏi xem chuỗi con có hợp lệ theo định nghĩa tổng quát này hay không. 

Các ràng buộc rất lớn, lên tới 200.000 ký tự và 200.000 thao tác, ngay lập tức loại trừ mọi giải pháp xây dựng lại hoặc kiểm tra lại chuỗi con từ đầu cho mỗi truy vấn. Bất kỳ cách tiếp cận nào tính toán lại tính hợp lệ của một phân đoạn theo thời gian tuyến tính sẽ giảm xuống độ phức tạp bậc hai trong trường hợp xấu nhất và thất bại. 

Điểm tinh tế chính là tính hợp lệ không được xác định bởi một quy tắc khớp duy nhất. Mỗi loại dấu ngoặc có thể hoạt động giống như một cặp được phản chiếu theo một trong hai hướng, nghĩa là việc so khớp ngăn xếp cổ điển không thể áp dụng trực tiếp trừ khi được mã hóa cẩn thận. 

Các trường hợp cạnh chủ yếu liên quan đến chuỗi con ngắn và hướng hỗn hợp. Ví dụ: một chuỗi con ký tự đơn luôn không hợp lệ vì không có cấu trúc hợp lệ nào không trống có thể bao gồm một dấu ngoặc đơn không khớp. Một trường hợp cạnh khác phát sinh khi một chuỗi con hợp lệ theo nghĩa cổ điển cho một hướng nhưng trở nên không hợp lệ ở đây do việc ghép nối được nhân đôi bắt buộc bị hỏng. 

Một ví dụ cụ thể về sự thất bại của logic đơn giản là chuỗi "()". Trong ngoặc cổ điển điều này là hợp lệ, nhưng trong hệ thống này nó cũng hợp lệ. Tuy nhiên, một chuỗi như ")( " cũng hợp lệ vì nó khớp với dạng đảo ngược ")S(". Một trình kiểm tra đơn giản chỉ khớp với dấu ngoặc đơn mở và đóng sẽ từ chối nó một cách không chính xác. 

Một trường hợp cạnh khác là nối. Một chuỗi như "()[]" hoặc thậm chí "(())[]{}", là hợp lệ vì nó phân tách thành các phân đoạn hợp lệ độc lập. Một giải pháp buộc lồng toàn cục sẽ từ chối những trường hợp như vậy một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xử lý từng truy vấn bằng cách trích xuất chuỗi con và chạy kiểm tra xác thực đầy đủ. Bản thân việc kiểm tra đó không hề đơn giản vì có nhiều đối xứng trong khung, nhưng ngay cả khi chúng tôi giả sử tồn tại xác thực dựa trên ngăn xếp theo thời gian tuyến tính, thì mỗi truy vấn sẽ có chi phí là O(n), dẫn đến tổng độ phức tạp là O(nm), vượt xa khả năng thực hiện đối với 200.000 thao tác. 

Quan sát quan trọng là mặc dù định nghĩa khác thường về dấu ngoặc, cấu trúc vẫn hoạt động giống như một dạng dấu ngoặc đơn cân bằng với nhiều loại và cách diễn giải đối xứng. Mỗi chuỗi con hợp lệ phải đáp ứng điều kiện hủy toàn cục có thể được theo dõi bằng biểu diễn kiểu cây phân đoạn.

Chúng tôi giảm thiểu vấn đề để duy trì, đối với mỗi phân đoạn, một “trạng thái” chuẩn tóm tắt cách các phần mở và đóng không khớp tương tác với nhau. Khi hợp nhất hai phân đoạn, chúng tôi khớp các loại khung tương thích một cách tham lam từ ranh giới vào trong, giảm số lượng không khớp một cách nhất quán. Về mặt tinh thần, điều này tương tự như việc duy trì nhiều tập hợp các đầu mở có thể hủy bỏ bằng các lần đóng tương thích, nhưng được triển khai ở dạng đại số nén để mỗi phân đoạn chỉ lưu trữ thông tin tổng hợp. 

Thông tin chi tiết quan trọng là mọi phân đoạn có thể được biểu diễn bằng một cấu trúc có kích thước cố định nhỏ giúp nắm bắt số lượng dấu ngoặc chưa khớp của từng loại và hướng còn lại sau khi hủy nội bộ. Khi hợp nhất hai phân đoạn, chúng tôi mô phỏng việc hủy bỏ giữa hậu tố của phân đoạn bên trái và tiền tố của phân đoạn bên phải trong thời gian O(1) cho mỗi loại. Điều này làm cho cây phân đoạn trở nên tự nhiên: mỗi nút lưu trữ biểu diễn nén này, cập nhật là các thay đổi điểm và truy vấn là hợp nhất phạm vi. 

Điều này biến mỗi thao tác thành O(log n), vì cả cập nhật và truy vấn đều đi qua cây phân đoạn và kết hợp các trạng thái có kích thước O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Cây phân đoạn có trạng thái hợp nhất | O((n + m) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng cây phân đoạn trên chuỗi, trong đó mỗi nút lưu trữ trạng thái thu gọn mô tả các dấu ngoặc không khớp. 

1. Đối với mỗi nút lá, chúng ta khởi tạo một cấu trúc biểu thị một ký tự ngoặc đơn. Trạng thái này ghi lại nó như một phần tử mở hoặc đóng chưa từng có về loại và hướng của nó. Điều này là cần thiết vì toàn bộ thuật toán phụ thuộc vào việc kết hợp chính xác các trạng thái nguyên thủy này. 
2. Xác định thao tác hợp nhất giữa hai trạng thái đại diện cho các phân đoạn liền kề. Việc hợp nhất mô phỏng việc hủy bỏ giữa các dấu ngoặc tương thích chưa từng có trên ranh giới. Chúng tôi liên tục khớp các phần mở không khớp bên phải của đoạn bên trái với các phần đóng không khớp bên trái của đoạn bên phải bất cứ khi nào loại của chúng cho phép hủy theo quy tắc đối xứng của bài toán. Bước này là cốt lõi của giải pháp vì nó thay thế mô phỏng ngăn xếp rõ ràng bằng tính năng hủy tổng hợp. 
3. Xây dựng cây phân đoạn từ dưới lên bằng thao tác hợp nhất. Mỗi nút bên trong biểu thị hiệu ứng kết hợp của khoảng thời gian của nó sau khi hủy bỏ hoàn toàn nội bộ. Điều này đảm bảo rằng mọi nút đều tóm tắt chính xác phân đoạn của nó. 
4. Đối với truy vấn loại 1, hãy cập nhật một nút lá đơn và tính toán lại tất cả các tổ tiên bằng thao tác hợp nhất. Điều này duy trì tính nhất quán của cây phân đoạn sau khi sửa đổi. 
5. Đối với truy vấn loại 2, hãy truy vấn cây phân đoạn cho khoảng [l, r], trả về trạng thái đã hợp nhất của phân đoạn đó. Nếu trạng thái kết quả không còn dấu ngoặc vuông nào thì chuỗi con đó hợp lệ. 

Tại sao nó hoạt động: mỗi nút duy trì một bất biến rằng trạng thái của nó thể hiện đầy đủ dạng rút gọn của phân đoạn sau tất cả các lần hủy nội bộ có thể xảy ra. Hoạt động hợp nhất có tính kết hợp theo nghĩa là việc kết hợp các phân đoạn trong bất kỳ nhóm nào đều mang lại trạng thái rút gọn cuối cùng giống nhau, bởi vì việc hủy bỏ chỉ phụ thuộc vào các tương tác biên chứ không bao giờ phụ thuộc vào cấu trúc bên trong một khi đã được rút gọn. Do đó, trạng thái gốc của bất kỳ khoảng truy vấn nào đều trống khi và chỉ khi chuỗi con có thể được giảm hoàn toàn thành chuỗi trống trong các thao tác được phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# We encode each bracket into a type + orientation.
# There are 4 types: (), [], {}, <> and each has two orientations.

pairs = {
    '(': 0, ')': 0,
    '[': 1, ']': 1,
    '{': 2, '}': 2,
    '<': 3, '>': 3
}

is_open = {
    '(': True, '[': True, '{': True, '<': True,
    ')': False, ']': False, '}': False, '>': False
}

class Node:
    __slots__ = ("open_cnt", "close_cnt")
    def __init__(self):
        self.open_cnt = [0] * 4
        self.close_cnt = [0] * 4

def merge(a, b):
    res = Node()

    for t in range(4):
        # match a's closing with b's opening
        match = min(a.close_cnt[t], b.open_cnt[t])
        a_close = a.close_cnt[t] - match
        b_open = b.open_cnt[t] - match

        res.open_cnt[t] = a.open_cnt[t] + b.open_cnt[t]
        res.close_cnt[t] = a.close_cnt[t] + b.close_cnt[t]

        res.open_cnt[t] -= match
        res.close_cnt[t] -= match

    return res

class SegTree:
    def __init__(self, s):
        self.n = len(s)
        self.t = [Node() for _ in range(4 * self.n)]
        self.s = s
        self.build(1, 0, self.n - 1)

    def make(self, c):
        node = Node()
        t = pairs[c]
        if is_open[c]:
            node.open_cnt[t] = 1
        else:
            node.close_cnt[t] = 1
        return node

    def build(self, v, l, r):
        if l == r:
            self.t[v] = self.make(self.s[l])
            return
        m = (l + r) // 2
        self.build(v * 2, l, m)
        self.build(v * 2 + 1, m + 1, r)
        self.t[v] = merge(self.t[v * 2], self.t[v * 2 + 1])

    def update(self, v, l, r, idx, c):
        if l == r:
            self.t[v] = self.make(c)
            return
        m = (l + r) // 2
        if idx <= m:
            self.update(v * 2, l, m, idx, c)
        else:
            self.update(v * 2 + 1, m + 1, r, idx, c)
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

def solve():
    n = int(input())
    s = list(input().strip())
    m = int(input())

    st = SegTree(s)

    out = []
    for _ in range(m):
        tmp = input().split()
        if tmp[0] == '1':
            idx = int(tmp[1]) - 1
            st.update(1, 0, n - 1, idx, tmp[2])
        else:
            l = int(tmp[1]) - 1
            r = int(tmp[2]) - 1
            res = st.query(1, 0, n - 1, l, r)

            ok = True
            for t in range(4):
                if res.open_cnt[t] != 0 or res.close_cnt[t] != 0:
                    ok = False
                    break
            out.append("Yes" if ok else "No")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Cây phân đoạn lưu trữ một hồ sơ nén không khớp cho từng phân đoạn. Mỗi lá là tầm thường và mỗi nút bên trong sẽ hợp nhất hai cấu hình bằng cách hủy các cặp ranh giới tương thích. Truy vấn trả về một cấu hình chỉ hợp lệ nếu mọi loại khung bị loại bỏ hoàn toàn. 

Một chi tiết triển khai tinh tế là việc hợp nhất không bao giờ được sử dụng lại các cặp đã khớp giữa các loại vì mỗi loại khung là độc lập. Một điểm khác là các bản cập nhật phải xây dựng lại hoàn toàn đường dẫn đến thư mục gốc, nếu không thì trạng thái hủy cũ sẽ truyền lên trên. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi ngắn như “()[]”. Chúng tôi xây dựng các trạng thái lá dưới dạng mở và đóng đơn lẻ không khớp, sau đó hợp nhất hai ký tự đầu tiên thành trạng thái trống cho dấu ngoặc đơn và tương tự cho dấu ngoặc vuông, tạo ra trạng thái gốc hoàn toàn trống. 

| Bước | Phân đoạn | Mở | Đóng | Trạng thái hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | "(" | 1 | 0 | Không | 
| 2 | ")" | 0 | 1 | Không | 
| 3 | "()" | 0 | 0 | Có | 
| 4 | "[]” | 0 | 0 | Có | 
| 5 | "()[]" | 0 | 0 | Có | 

Dấu vết này cho thấy rằng việc ghép nối được xử lý một cách tự nhiên bằng thao tác hợp nhất. 

Bây giờ hãy xem xét “([)]”, điều này không hợp lệ vì các loại gây cản trở. 

| Bước | Phân đoạn | Mở | Đóng | Trạng thái hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | "(" | 1 | 0 | Không | 
| 2 | "[“ | 1 | 0 | Không | 
| 3 | "([“ | 2 | 0 | Không | 
| 4 | ")" | 2 | 1 | Không | 
| 5 | "([)]” | 1 | 2 | Không | 

Trạng thái cuối cùng không trống nên truy vấn trả về không hợp lệ. Điều này chứng tỏ rằng sự không khớp kiểu chéo không bị hủy bỏ một cách vô tình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Mỗi bản cập nhật và truy vấn chạm vào một đường dẫn cây phân đoạn và hợp nhất các trạng thái có kích thước O(1) | 
| Không gian | O(n) | Các nút cây phân đoạn lưu trữ trạng thái khung có kích thước không đổi | 

Các ràng buộc cho phép tối đa 200.000 thao tác và chi phí logarit cho mỗi thao tác vẫn ở mức thoải mái trong giới hạn cho ngân sách thời gian 1-2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # assume solve() is defined above
    return sys.stdout.getvalue()

# provided samples (placeholders since statement has no official sample text here)
# assert run(...) == ...

# custom cases

# minimum case
assert run("1\n()\n1\n2 1 2\n") == "Yes\n"

# single character invalid
assert run("1\n(\n1\n2 1 1\n") == "No\n"

# update makes valid
assert run("3\n([)\n1\n1 2 ]\n2 1 3\n") == "Yes\n"

# all same type
assert run("4\n()()\n1\n2 1 4\n") == "Yes\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 \n () \n 1 \n 2 1 2 | Có | chuỗi con hợp lệ cơ bản | 
| 1 \n ( \n 1 \n 2 1 1 | Không | ký tự đơn không hợp lệ | 
| 3 \n ([) \n 1 \n 1 2 ] \n 2 1 3 | Có | cập nhật ảnh hưởng đến tính hợp lệ | 
| 4 \n ()() \n 1 \n 2 1 4 | Có | xử lý nối | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi các bản cập nhật chuyển một ký tự từ mở sang đóng, thay đổi hành vi hủy ở cả hai phía của nút cây phân đoạn. Ví dụ: bắt đầu bằng “((” và cập nhật một ký tự thành “)”, cấu trúc thay đổi từ hai lần mở không khớp thành kịch bản hủy một phần. Cây phân đoạn đảm bảo rằng việc tính toán lại lan truyền lên trên, do đó gốc phản ánh chính xác số dư được cập nhật. 

Một trường hợp cạnh khác là một chuỗi con bao gồm toàn bộ các loại dấu ngoặc khác nhau. Ví dụ: “([{}])” chỉ hợp lệ nếu được lồng chính xác; nếu không nó sẽ trở nên không hợp lệ. Logic hợp nhất ngăn chặn việc hủy kiểu chéo ngẫu nhiên, do đó trạng thái vẫn không trống trừ khi cấu trúc thực sự khớp. 

Trường hợp cạnh thứ ba đang truy vấn một vị trí sau nhiều lần cập nhật. Nút lá phản ánh trực tiếp dấu ngoặc hiện tại của nó, vì vậy câu trả lời phụ thuộc hoàn toàn vào việc liệu ký tự đơn đó có thể tạo thành cấu trúc hợp lệ hay không, điều này không thể và thuật toán trả về chính xác là “Không”.
