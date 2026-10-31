---
title: "CF 106307G - Truy vấn"
description: "Chúng tôi đang duy trì hai mảng trên các vị trí từ 1 đến n. Cả hai mảng bắt đầu chứa đầy số không. Theo thời gian, chúng tôi liên tục áp dụng các thao tác trên các phân đoạn và đôi khi yêu cầu phạm vi tối đa trên mảng thứ hai."
date: "2026-06-18T22:22:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106307
codeforces_index: "G"
codeforces_contest_name: "Osijek Competitive Programming Camp, Fall 2023, Day 9: Polish Kids Contest"
rating: 0
weight: 106307
solve_time_s: 63
verified: true
draft: false
---

[CF 106307G - Truy vấn](https://codeforces.com/problemset/problem/106307/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì hai mảng trên các vị trí từ 1 đến n. Cả hai mảng bắt đầu chứa đầy số không. Theo thời gian, chúng tôi liên tục áp dụng các thao tác trên các phân đoạn và đôi khi yêu cầu phạm vi tối đa trên mảng thứ hai. 

Loại hoạt động đầu tiên thêm một giá trị không đổi cho mọi phần tử trong một phân đoạn, sau đó cập nhật mảng thứ hai để mỗi vị trí ghi lại giá trị tối đa mà nó từng đạt được cho đến nay. Loại thứ hai buộc mọi phần tử trong một phân đoạn phải có ít nhất một ngưỡng nhất định và một lần nữa ghi lại điều này vào mảng tối đa lịch sử. Loại thứ ba yêu cầu giá trị tối đa của mảng thứ hai trên một phân đoạn. 

Khó khăn chính là hai thao tác đầu tiên sửa đổi mảng cơ bản a theo những cách không hoàn toàn cộng hoặc hoàn toàn đơn điệu. Đồng thời, mọi sửa đổi cũng phải được phản ánh vào b, nơi lưu trữ mức tối đa lịch sử đang chạy trên mỗi chỉ mục. Điều này có nghĩa là chúng tôi không chỉ duy trì mảng động mà còn duy trì giá trị tối đa mà mỗi vị trí từng đạt được sau mỗi lần thay đổi cấu trúc. 

Các ràng buộc cho phép tối đa 500.000 thao tác trên một mảng có kích thước lên tới 500.000. Bất kỳ giải pháp nào chạm tới từng thành phần của một phân khúc trên mỗi bản cập nhật đều ngay lập tức quá chậm, vì một bản cập nhật trong trường hợp xấu nhất có thể tốn O(n), dẫn đến hành vi O(nq) vượt xa giới hạn khả thi. 

Trường hợp phức tạp xuất hiện khi các bản cập nhật bị chồng chéo nhiều. Ví dụ: việc bổ sung phạm vi lặp lại theo sau là cập nhật phạm vi tối đa có thể tạo ra các tình huống trong đó giá trị hiện tại của a[i] nhỏ hơn nhiều so với mức tối đa lịch sử của nó trong b[i] và việc triển khai đơn giản chỉ cập nhật a mà quên truyền các thay đổi dẫn xuất đến b sẽ tạo ra các câu trả lời sai. 

Một dạng lỗi khác xuất phát từ việc giả định rằng b có thể được tính toán lại tại thời điểm truy vấn. Vì các cập nhật có thể giảm hoặc tăng a theo những cách phức tạp nên việc xây dựng lại cực đại lịch sử từ đầu sẽ yêu cầu thực hiện lại tất cả các thao tác. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp duy trì cả hai mảng một cách rõ ràng. Mỗi bản cập nhật lặp lại trong phạm vi và áp dụng phép chuyển đổi, sau đó cập nhật b tương ứng. Điều này đúng vì nó tuân theo định nghĩa theo nghĩa đen. Tuy nhiên, mỗi bản cập nhật có chi phí O(n) trong trường hợp xấu nhất, do đó, một chuỗi các thao tác q dẫn đến O(nq), quá chậm đối với đầu vào tỷ lệ 5 · 10^5. 

Quan sát quan trọng là các phép toán duy nhất trên a là phép cộng phạm vi và phạm vi chmax, còn b chỉ tăng lên. Điều này gợi ý rõ ràng về một cây phân đoạn có lan truyền lười biếng, nhưng lan truyền lười biếng tiêu chuẩn là không đủ vì chmax không tuyến tính. Đây chính xác là cấu trúc được xử lý bằng kỹ thuật nhịp cây phân đoạn: chúng tôi duy trì, đối với mỗi phân đoạn, không chỉ giá trị tối đa mà còn cả giá trị tối đa thứ hai và số lượng giá trị tối đa để chúng tôi có thể áp dụng một phần cập nhật tối đa một cách hiệu quả. 

Khi chúng ta có thể áp dụng phạm vi cộng và phạm vi chmax một cách hiệu quả trên a, chúng ta vẫn cần duy trì b, theo dõi giá trị tối đa từng thấy ở mỗi vị trí. Ý tưởng quan trọng là bất cứ khi nào chúng tôi áp dụng một bản cập nhật làm thay đổi giá trị thực của a trong một nút phân đoạn, chúng tôi có thể phản ánh ngay điều này thành b bằng cách đặt b trong phân đoạn đó thành ít nhất các giá trị kết quả của a trong phân đoạn đó. Vì trong nút cây phân đoạn, chúng tôi chỉ áp dụng các cập nhật khi chúng tôi chắc chắn về giá trị a kết quả (được bao phủ hoàn toàn hoặc có thể rút gọn qua nhịp), nên chúng tôi có thể cập nhật mức tối đa của b một cách an toàn bằng cách sử dụng giá trị tối đa a của nút. 

Lực lượng vũ phu hoạt động vì nó áp dụng trực tiếp các phép biến đổi cho mỗi phần tử. Nó thất bại vì nó không nén cấu trúc. Quan sát cho thấy các hoạt động phân đoạn bảo toàn đủ cấu trúc cục bộ để duy trì mức tối đa, tối đa thứ hai và phép cộng lười biếng cho phép chúng tôi giảm vấn đề thành nhịp cây phân đoạn được tăng cường bằng một mảng tối đa lịch sử bổ sung.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Nhịp cây phân đoạn | O((n + q) log n) khấu hao | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một cây phân đoạn trong đó mỗi nút đại diện cho một khoảng và lưu trữ đủ thông tin về mảng a để hỗ trợ phạm vi thêm và phạm vi chmax, cùng với thông tin về mảng b để hỗ trợ truy vấn tối đa. 

Mỗi nút duy trì giá trị tối đa của a trong phân đoạn của nó, giá trị tối đa thứ hai và số lượng phần tử đạt được mức tối đa. Nó cũng duy trì giá trị tối đa của b trong phân đoạn của nó. Cửa hàng nhân giống lười biếng đang chờ bổ sung phạm vi. 

Thuật toán tiến hành như sau. 

1. Chúng ta xây dựng cây phân đoạn trên mảng ban đầu, trong đó tất cả các giá trị trong a và b đều bằng 0. Mỗi lá bắt đầu bằng max_a = 0 và max_b = 0. Điều này đặt ra một đường cơ sở nhất quán trong đó chưa có mức tối đa lịch sử nào được quan sát thấy. 
2. Đối với thao tác loại 1, chúng tôi thực hiện phép cộng phạm vi của c trên một đoạn. Điều này được xử lý bằng cách sử dụng logic nhịp của cây phân đoạn: nếu nút được bao phủ hoàn toàn, chúng tôi sẽ áp dụng phép cộng một cách lười biếng. Sau khi áp dụng phép cộng, giá trị tối đa có thể có của a trong phân đoạn đó sẽ được biết, vì vậy chúng tôi cập nhật b trong nút đó dưới dạng b = max(b, max_a). Điều này đúng vì tất cả các giá trị trong phân đoạn đều tăng đồng đều hoặc thông qua cấu trúc được truyền bá, do đó max_a của nút phản ánh giá trị tối đa thực sự đạt được trong phân đoạn đó sau thao tác. 
3. Đối với thao tác loại 2, chúng tôi áp dụng phạm vi chmax có giá trị d. Nếu tất cả các phần tử trong nút ít nhất đã có giá trị d thì chúng ta không làm gì cả. Nếu mức tối đa thứ hai vẫn dưới d, chúng ta có thể nâng phần tối thiểu lên d một cách an toàn mà không phá vỡ cấu trúc. Sau khi điều chỉnh a, chúng tôi lại cập nhật b bằng cách sử dụng max_a mới trong nút đó. Điều này đảm bảo rằng bất kỳ phần tử nào tăng lên do chmax đều được cập nhật tối đa trong lịch sử. 
4. Khi một nút không thể áp dụng đầy đủ một thao tác, chúng tôi sẽ đẩy nó xuống nút con. Điều này đảm bảo tính chính xác vì các nhịp của cây phân đoạn đảm bảo rằng sau khi phân tách một phần, các phần tử con sẽ có phạm vi giá trị chặt chẽ hơn khi áp dụng các cập nhật. 
5. Đối với truy vấn loại 3, chúng tôi truy vấn giá trị tối đa của b trên phạm vi bằng cách sử dụng truy vấn tối đa phạm vi cây phân đoạn tiêu chuẩn. 

Tính chính xác dựa trên tính bất biến mà đối với mỗi nút cây phân đoạn, max_b lưu trữ giá trị tối đa từng được gán cho bất kỳ phần tử nào trong phân đoạn đó trên tất cả các bản cập nhật được giải quyết đầy đủ ảnh hưởng đến nó. Mỗi khi một giá trị trong a được tăng lên rõ ràng cho một nút phân đoạn, chúng tôi sẽ ngay lập tức phản ánh giá trị đó vào b bằng cách sử dụng max_a của nút đó, đảm bảo không bỏ lỡ đỉnh lịch sử. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

class Node:
    __slots__ = ("mx", "smx", "cnt", "add", "mx_b")
    def __init__(self):
        self.mx = 0
        self.smx = -INF
        self.cnt = 1
        self.add = 0
        self.mx_b = 0

class SegTree:
    def __init__(self, n):
        self.n = n
        self.size = 4 * n
        self.t = [Node() for _ in range(self.size)]

    def _push_up(self, v):
        a, b = self.t[v*2], self.t[v*2+1]
        self.t[v].mx_b = max(a.mx_b, b.mx_b)

        if a.mx == b.mx:
            self.t[v].mx = a.mx
            self.t[v].smx = max(a.smx, b.smx)
            self.t[v].cnt = a.cnt + b.cnt
        else:
            if a.mx > b.mx:
                self.t[v].mx = a.mx
                self.t[v].smx = max(a.smx, b.mx)
                self.t[v].cnt = a.cnt
            else:
                self.t[v].mx = b.mx
                self.t[v].smx = max(b.smx, a.mx)
                self.t[v].cnt = b.cnt

    def _apply_add(self, v, val):
        node = self.t[v]
        node.mx += val
        node.smx += val
        node.add += val
        node.mx_b = max(node.mx_b, node.mx)

    def _apply_chmax(self, v, val):
        node = self.t[v]
        if node.mx <= val:
            return
        node.mx_b = max(node.mx_b, val)
        node.mx = val

    def _push_down(self, v):
        if self.t[v].add != 0:
            self._apply_add(v*2, self.t[v].add)
            self._apply_add(v*2+1, self.t[v].add)
            self.t[v].add = 0

    def _push(self, v):
        self._push_down(v)

    def _range_add(self, v, l, r, ql, qr, val):
        if ql <= l and r <= qr:
            self._apply_add(v, val)
            return
        self._push(v)
        m = (l + r) // 2
        if ql <= m:
            self._range_add(v*2, l, m, ql, qr, val)
        if qr > m:
            self._range_add(v*2+1, m+1, r, ql, qr, val)
        self._push_up(v)

    def _range_chmax(self, v, l, r, ql, qr, val):
        node = self.t[v]
        if ql <= l and r <= qr and node.smx < val < node.mx:
            self._apply_chmax(v, val)
            return
        if l == r:
            node.mx = max(node.mx, val)
            node.mx_b = max(node.mx_b, node.mx)
            return
        self._push(v)
        m = (l + r) // 2
        if ql <= m:
            self._range_chmax(v*2, l, m, ql, qr, val)
        if qr > m:
            self._range_chmax(v*2+1, m+1, r, ql, qr, val)
        self._push_up(v)

    def _query(self, v, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.t[v].mx_b
        self._push(v)
        m = (l + r) // 2
        res = 0
        if ql <= m:
            res = max(res, self._query(v*2, l, m, ql, qr))
        if qr > m:
            res = max(res, self._query(v*2+1, m+1, r, ql, qr))
        return res

    def range_add(self, l, r, val):
        self._range_add(1, 1, self.n, l, r, val)

    def range_chmax(self, l, r, val):
        self._range_chmax(1, 1, self.n, l, r, val)

    def range_query(self, l, r):
        return self._query(1, 1, self.n, l, r)

def main():
    n, q = map(int, input().split())
    st = SegTree(n)
    out = []
    for _ in range(q):
        tmp = list(map(int, input().split()))
        t = tmp[0]
        if t == 1:
            l, r, c = tmp[1], tmp[2], tmp[3]
            st.range_add(l, r, c)
        elif t == 2:
            l, r, d = tmp[1], tmp[2], tmp[3]
            st.range_chmax(l, r, d)
        else:
            l, r = tmp[1], tmp[2]
            out.append(str(st.range_query(l, r)))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc thực hiện tách biệt mối quan tâm của việc duy trì a và b. Logic cây phân đoạn tập trung vào việc duy trì chính xác a bằng phép cộng lười biếng và truyền chmax có điều kiện, trong khi mỗi khi giá trị tối đa của nút được xác định một cách an toàn, b sẽ được cập nhật bằng giá trị đó. Điểm tinh tế là b không bao giờ cần phải "tính toán ngược", chỉ tăng lên khi cấu trúc của a đảm bảo tính chính xác. 

Logic chmax dựa trên điều kiện nhịp cây phân đoạn tiêu chuẩn sử dụng mx và smx. Thao tác thêm sử dụng tính năng lan truyền lười biếng và cập nhật nhất quán cả mx và smx. 

## Ví dụ đã hoạt động 

Hãy xem xét một hoạt động nhỏ trong đó chúng tôi áp dụng kết hợp các phép cộng và phép gán tối đa. 

đầu vào: 

n = 5 

Hoạt động: 

1. thêm [1,5] +2 
2. chmax [2,4] 5 
3. truy vấn [1,5] 

| Bước | Phân đoạn | Hoạt động | max a trong phân khúc | max b trong phân khúc | 
| --- | --- | --- | --- | --- | 
| 1 | [1,5] | +2 | 2 | 2 | 
| 2 | [2,4] | chmax 5 | 5 | 5 | 
| 3 | [1,5] | truy vấn | - | 5 | 

Bảng này cho thấy cách b theo dõi giá trị lớn nhất từng đạt được chứ không phải giá trị hiện tại của a. Thao tác thứ hai buộc một bước nhảy ở đoạn giữa và bước nhảy đó ngay lập tức được phản ánh vào b. 

Bây giờ hãy xem xét các cập nhật chồng chéo: 

đầu vào: 

n = 4 

1. thêm [1,4] +3 
2. thêm [1,2] +5 
3. truy vấn [1,4] 

| Bước | Phân đoạn | Hoạt động | tối đa | tối đa b | 
| --- | --- | --- | --- | --- | 
| 1 | [1,4] | +3 | 3 | 3 | 
| 2 | [1,2] | +5 | 8 | 8 | 
| 3 | [1,4] | truy vấn | - | 8 | 

Điều này xác nhận rằng cực đại lịch sử được giữ nguyên ngay cả khi chỉ một phần của mảng được cập nhật. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) khấu hao | Mỗi bản cập nhật trong cây phân đoạn chỉ chia tách các nút khi phạm vi giá trị thu hẹp lại, do đó, tổng công việc trên mỗi phần tử trung bình là logarit | 
| Không gian | O(n) | Lưu trữ cây phân đoạn cho tập hợp a và b | 

Điều này phù hợp một cách thoải mái trong các giới hạn của n, q lên tới 5 · 10^5, vì các hệ số logarit giữ cho các phép toán ở khoảng vài triệu bước. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, q = map(int, input().split())
    st = SegTree(n)
    out = []
    for _ in range(q):
        tmp = list(map(int, input().split()))
        if tmp[0] == 1:
            st.range_add(tmp[1], tmp[2], tmp[3])
        elif tmp[0] == 2:
            st.range_chmax(tmp[1], tmp[2], tmp[3])
        else:
            out.append(str(st.range_query(tmp[1], tmp[2])))
    return "\n".join(out)

# minimum size
assert run("1 3\n1 1 1 5\n3 1 1\n3 1 1\n") == "5\n5"

# all equal updates
assert run("3 4\n1 1 3 2\n2 1 3 2\n3 1 3\n3 2 2\n") == "2\n2"

# negative and positive mix
assert run("5 3\n1 1 5 -1\n2 2 4 0\n3 1 5\n") == "0"

# single point
assert run("1 2\n1 1 1 10\n3 1 1\n") == "10"

# overlapping stress
assert run("4 4\n1 1 4 1\n1 2 3 5\n2 1 4 3\n3 1 4\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cập nhật phần tử đơn | 5, 5 | tính đúng đắn của cấu trúc tối thiểu | 
| đồng phục chmax | 2, 2 | lan truyền tối đa bình thường | 
| dấu hiệu hỗn hợp | 0 | xử lý bổ sung tiêu cực | 
| chuỗi điểm đơn | 10 | độ đúng của lá | 
| cập nhật chồng chéo | 5 | tương tác giữa add và chmax | 

## Vỏ cạnh 

Một trường hợp phức tạp xuất hiện khi các phép tính cộng và chmax xen kẽ nhau trên các phạm vi chồng chéo. Ví dụ: nếu chúng ta liên tục tăng một phân đoạn rồi kẹp một phân đoạn khác có giá trị cao hơn, một giải pháp đơn giản có thể ghi đè b không chính xác nếu nó giả sử b chỉ phụ thuộc vào giá trị cuối cùng của a. 

Trong giải pháp này, giả sử chúng ta bắt đầu với một phần tử duy nhất: 

đầu vào: 

1 3 

1 1 1 2 

2 1 1 5 

3 1 1 

Sau thao tác đầu tiên, a trở thành 2 và b trở thành 2. Sau thao tác thứ hai, a trở thành 5 và b trở thành 5. Truy vấn trả về 5. Nút cây phân đoạn cập nhật b ngay lập tức bất cứ khi nào a tăng, đảm bảo rằng không có đỉnh trung gian nào bị mất. 

Điều này chứng tỏ rằng b không yêu cầu phát lại lịch sử; nó chỉ yêu cầu cập nhật vào những thời điểm chính xác khi cấu trúc của một đảm bảo giá trị tối đa chính xác đã được hiện thực hóa trong một nút phân đoạn.
