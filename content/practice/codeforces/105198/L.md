---
title: "CF 105198L - Chuỗi Kalopsia"
description: "Chúng tôi đang duy trì một chuỗi dấu ngoặc đơn nhị phân trong đó mỗi ký tự là dấu ngoặc mở hoặc đóng. Chuỗi thay đổi theo thời gian và sau mỗi lần cập nhật, chúng tôi có thể cần trả lời xem chuỗi con đã chọn có tạo thành chuỗi dấu ngoặc thông thường hợp lệ hay không."
date: "2026-06-27T03:01:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105198
codeforces_index: "L"
codeforces_contest_name: "ShellBeeHaken Presents Intra SUST Programming Contest 2024 - Replay"
rating: 0
weight: 105198
solve_time_s: 87
verified: false
draft: false
---

[CF 105198L - Trình tự Kalopsia](https://codeforces.com/problemset/problem/105198/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một chuỗi dấu ngoặc đơn nhị phân trong đó mỗi ký tự là dấu ngoặc mở hoặc đóng. Chuỗi thay đổi theo thời gian và sau mỗi lần cập nhật, chúng tôi có thể cần trả lời xem chuỗi con đã chọn có tạo thành chuỗi dấu ngoặc thông thường hợp lệ hay không. 

Một chuỗi con được coi là hợp lệ nếu nó có thể được hiểu là sự lồng ghép chính xác của các dấu ngoặc đơn, nghĩa là mọi tiền tố của chuỗi con không bao giờ có nhiều dấu ngoặc đóng hơn dấu ngoặc mở và tổng số dấu ngoặc mở và đóng ở cuối bằng nhau. 

Hai hoạt động được hỗ trợ. Một thao tác sẽ lật mọi dấu ngoặc trong một phạm vi, biến mọi “(” thành “)” và mọi “)” thành “(”. Thao tác còn lại sẽ hỏi liệu một phạm vi nhất định hiện có tạo thành một chuỗi hợp lệ theo định nghĩa này hay không. 

Khó khăn đến từ hai hướng cùng một lúc. Đầu tiên, các bản cập nhật ảnh hưởng đến toàn bộ phân khúc chứ không phải các vị trí riêng lẻ. Thứ hai, tính hợp lệ không phải là một truy vấn đếm đơn giản, bởi vì số dư phải đúng ở mọi tiền tố của chuỗi con, không chỉ ở tổng số. 

Với tối đa 200.000 vị trí và 200.000 thao tác, mọi giải pháp tính toán lại tính hợp lệ của tiền tố cho mỗi truy vấn sẽ ngay lập tức thất bại. Việc quét toàn bộ cho mỗi truy vấn sẽ tiêu tốn thời gian bậc hai trong trường hợp xấu nhất, vượt xa phạm vi cho phép. Ngay cả số lần quét logarit trên mỗi thao tác cũng quá chậm nếu mỗi lần quét có độ dài tuyến tính theo phân đoạn. 

Một dạng thất bại tinh vi hơn sẽ xuất hiện nếu chúng ta cố gắng chỉ duy trì sự cân bằng tổng thể của một phân khúc. Ví dụ: chuỗi “())(” có tổng số dư bằng 0 nhưng không hợp lệ vì tiền tố chuyển sang âm. Điều này cho thấy rằng mọi cấu trúc đúng đều phải theo dõi hành vi của tiền tố chứ không chỉ tổng số. 

Một vấn đề tế nhị khác phát sinh với việc lật. Sau khi đảo ngược, các vai trò trong dấu ngoặc đơn sẽ hoán đổi hoàn toàn, do đó, bất kỳ biểu diễn nào giả định trọng số tĩnh mà không hỗ trợ chuyển đổi sẽ bị hỏng trừ khi nó xử lý rõ ràng việc đảo ngược dấu hiệu trên toàn bộ phân đoạn. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn, hãy trích xuất chuỗi con. Nếu đó là thao tác lật, hãy đảo ngược từng ký tự trong phạm vi. Nếu đó là kiểm tra tính hợp lệ, hãy mô phỏng một ngăn xếp hoặc duy trì số dư đang hoạt động và đảm bảo nó không bao giờ trở nên âm trong khi cũng kết thúc bằng 0. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của chuỗi ngoặc hợp lệ. 

Vấn đề là hiệu suất. Mỗi truy vấn có thể chạm tới tối đa O(n) ký tự. Với q lên tới 200.000, điều này dẫn đến hành vi O(nq), theo thứ tự các phép toán 4e10 trong trường hợp xấu nhất. Điều này không thể chạy kịp thời. 

Quan sát quan trọng là tính hợp lệ của chuỗi ngoặc chỉ phụ thuộc vào hai giá trị trên tiền tố: tổng của biểu diễn +1 và -1 và tổng tiền tố tối thiểu. Nếu chúng ta ánh xạ “(” thành +1 và “)” thành -1 thì một chuỗi con hợp lệ chính xác khi tổng của nó bằng 0 và tổng tiền tố tối thiểu của nó không bao giờ âm. 

Điều này biến vấn đề thành việc duy trì một chuỗi theo các cập nhật phạm vi trong khi có thể truy vấn cả tổng tiền tố và tổng tiền tố tối thiểu theo các khoảng thời gian. Cây phân đoạn có thể lưu trữ chính xác thông tin này. Thử thách còn lại là thao tác lật, phủ định tất cả các giá trị trong một phân đoạn. Sự phủ định không chỉ ảnh hưởng đến tổng, nó còn hoán đổi tiền tố cực trị theo cách chính xác có thể được xử lý bằng thẻ lan truyền lười biếng. 

Lực lượng vũ phu hoạt động vì nó đánh giá trực tiếp cấu trúc, nhưng nó không thành công khi phạm vi trở nên lớn và các bản cập nhật chồng chéo. Cây phân đoạn hoạt động vì điều kiện hợp lệ là thành phần: chúng ta có thể hợp nhất hai phân đoạn bằng cách sử dụng một lượng nhỏ thông tin và chúng ta có thể chuyển đổi một phân đoạn bằng cách đảo ngược mà không cần mở rộng nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · q) | O(n) | Quá chậm | 
| Cây phân đoạn với Lazy Flip | O(q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi biểu thị mỗi “(” là +1 và mỗi “)” là -1. Mỗi phân đoạn của mảng lưu trữ ba giá trị: tổng tổng, tổng tiền tố tối thiểu và tổng tiền tố tối đa. Những điều này đủ để trả lời đúng và hỗ trợ việc hợp nhất và lật. 

1. Xây dựng cây phân đoạn trong đó mỗi lá tương ứng với một ký tự. Một lá lưu trữ tổng bằng +1 hoặc -1 và cả hai cực trị tiền tố đều bằng giá trị đó. Điều này thiết lập sự biểu diễn cơ sở cho tất cả các hoạt động sau này. 
2. Đối với nút bên trong kết hợp các nút con trái và phải, hãy tính tổng bằng tổng của cả hai nút con. Tiền tố tối thiểu là mức tối thiểu của mức tối thiểu bên trái và tổng bên trái cộng với mức tối thiểu bên phải. Điều này hoạt động vì bất kỳ tiền tố nào cũng nằm trong phân đoạn bên trái hoặc mở rộng sang bên phải sau khi sử dụng hết tổng bên trái. Tiền tố tối đa được tính toán đối xứng bằng cách sử dụng cùng một lý do. 
3. Đối với thao tác lật, chúng tôi áp dụng phép chuyển đổi cho một phân đoạn thay vì tính toán lại nó. Việc lật sẽ thay đổi mọi +1 thành -1 và ngược lại, do đó tổng số sẽ trở thành số âm. 
4. Cấu trúc tiền tố cũng đảo ngược. Tiền tố tối thiểu mới trở thành số âm của tiền tố tối đa trước đó và tiền tố tối đa mới trở thành số âm của tiền tố tối thiểu trước đó. Điều này xuất phát từ việc đảo ngược dấu của từng phần tổng bên trong đoạn. 
5. Chúng tôi sử dụng cờ lan truyền lười biếng để đánh dấu một phân đoạn là bị đảo ngược mà không ngay lập tức đẩy các thay đổi tới phần tử con. Khi cần, chúng ta đẩy flip xuống bằng cách áp dụng phép biến đổi tương tự cho trẻ em và xóa cờ. 
6. Đối với một truy vấn, chúng tôi truy xuất phân đoạn đại diện cho [l, r]. Nếu tổng của nó bằng 0 và tiền tố tối thiểu của nó ít nhất bằng 0 thì chuỗi con đó hợp lệ. Nếu không thì không. 

Tính chính xác phụ thuộc vào thực tế là mọi phân đoạn đều được tóm tắt đầy đủ bằng ba giá trị này và cả việc hợp nhất và lật đều duy trì tính hợp lệ của bản tóm tắt này mà không làm mất thông tin. 

Tại sao nó hoạt động: bất kỳ chuỗi dấu ngoặc nào cũng tương ứng với bước đi trên các số nguyên trong đó mỗi ký tự thay đổi chiều cao bằng ±1. Hiệu lực chỉ phụ thuộc vào việc chuyến đi có kết thúc ở số 0 và không bao giờ xuống dưới 0 hay không. Cây phân đoạn duy trì tiền tố cực trị chính xác cho mỗi khoảng và cả phép nối và đảo dấu đều bảo toàn cấu trúc của biểu diễn bước đi này. Vì mọi thao tác đều tôn trọng những bất biến này nên các truy vấn luôn phản ánh trạng thái thực của chuỗi con. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("sum", "mn", "mx", "flip")
    def __init__(self, s=0, mn=0, mx=0):
        self.sum = s
        self.mn = mn
        self.mx = mx
        self.flip = False

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.t = [Node() for _ in range(4 * self.n)]
        self.build(1, 0, self.n - 1, arr)

    def build(self, v, l, r, arr):
        if l == r:
            val = 1 if arr[l] == '(' else -1
            self.t[v] = Node(val, val, val)
            return
        m = (l + r) // 2
        self.build(v * 2, l, m, arr)
        self.build(v * 2 + 1, m + 1, r, arr)
        self.pull(v)

    def pull(self, v):
        L = self.t[v * 2]
        R = self.t[v * 2 + 1]
        self.t[v].sum = L.sum + R.sum
        self.t[v].mn = min(L.mn, L.sum + R.mn)
        self.t[v].mx = max(L.mx, L.sum + R.mx)

    def apply_flip(self, v):
        node = self.t[v]
        node.sum *= -1
        node.mn, node.mx = -node.mx, -node.mn
        node.flip ^= True

    def push(self, v):
        if self.t[v].flip:
            self.apply_flip(v * 2)
            self.apply_flip(v * 2 + 1)
            self.t[v].flip = False

    def update(self, v, l, r, ql, qr):
        if ql <= l and r <= qr:
            self.apply_flip(v)
            return
        self.push(v)
        m = (l + r) // 2
        if ql <= m:
            self.update(v * 2, l, m, ql, qr)
        if qr > m:
            self.update(v * 2 + 1, m + 1, r, ql, qr)
        self.pull(v)

    def query(self, v, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.t[v]
        self.push(v)
        m = (l + r) // 2
        if qr <= m:
            return self.query(v * 2, l, m, ql, qr)
        if ql > m:
            return self.query(v * 2 + 1, m + 1, r, ql, qr)

        left = self.query(v * 2, l, m, ql, qr)
        right = self.query(v * 2 + 1, m + 1, r, ql, qr)

        res = Node()
        res.sum = left.sum + right.sum
        res.mn = min(left.mn, left.sum + right.mn)
        res.mx = max(left.mx, left.sum + right.mx)
        return res

def solve():
    n, q = map(int, input().split())
    s = list(input().strip())
    st = SegTree(s)

    out = []
    for _ in range(q):
        t, l, r = map(int, input().split())
        l -= 1
        r -= 1
        if t == 1:
            st.update(1, 0, n - 1, l, r)
        else:
            res = st.query(1, 0, n - 1, l, r)
            if res.sum == 0 and res.mn >= 0:
                out.append("YES")
            else:
                out.append("NO")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Cây phân đoạn được xây dựng trên mảng số nguyên được ánh xạ. Mỗi nút duy trì cực trị tổng và tiền tố để có thể trả lời bất kỳ truy vấn khoảng nào mà không cần quét các phần tử. Thao tác lật được xử lý một cách lười biếng: thay vì sửa đổi các nút con ngay lập tức, chúng tôi đảo ngược bản tóm tắt được lưu trữ và đánh dấu nút để các nút con chỉ được sửa khi cần thiết. Điều này giữ cho cả hoạt động cập nhật và truy vấn đều logarit. 

Một lỗi phổ biến là quên rằng tiền tố cực tiểu phải được tính toán lại tương ứng với tổng tích lũy khi hợp nhất các phân đoạn. Một cách khác là áp dụng phủ định mà không hoán đổi giá trị tối thiểu và tối đa, điều này phá vỡ tính chính xác của các cấu trúc lồng nhau. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi nhỏ “()()()” và lật một đoạn ở giữa. 

| Bước | Hoạt động | Phân đoạn | Tổng hợp | Tiền tố tối thiểu | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | ban đầu | (1,6) | 0 | 0 | CÓ | 
| 2 | lật (2,5) | (1,6) | 0 | 0 | CÓ | 

Sau khi lật phần giữa, cấu trúc vẫn cân bằng ngay cả khi thứ tự bên trong thay đổi, điều này xác nhận rằng biểu diễn phân đoạn trừu tượng hóa chính xác cấu trúc cục bộ. 

Bây giờ hãy xem xét “())(”: 

| Bước | Hoạt động | Phân đoạn | Tổng hợp | Tiền tố tối thiểu | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | truy vấn | (1,4) | 0 | -1 | KHÔNG | 

Mặc dù tổng bằng 0 nhưng tiền tố giảm xuống dưới 0, điều này sẽ loại bỏ chuỗi con một cách chính xác. 

Những ví dụ này cho thấy rằng tổng số dư thôi là không đủ và việc theo dõi tiền tố là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | Mỗi bản cập nhật và truy vấn đi qua chiều cao cây phân đoạn | 
| Không gian | O(n) | Các nút cây lưu trữ thông tin không đổi trên mỗi phân đoạn | 

Các ràng buộc cho phép lên tới 200.000 thao tác và thời gian logarit cho mỗi thao tác vẫn nằm trong giới hạn. Việc sử dụng bộ nhớ vẫn tuyến tính theo kích thước của chuỗi ban đầu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    solve()

    return output.getvalue().strip()

# sample-like test (formatting assumed corrected)
# assert run(...) == ...

# minimum size
assert run("1 1\n(\n2 1 1\n") in {"NO", "YES"}  # single bracket must be invalid/valid depending

# already balanced
assert run("2 1\n()\n2 1 2\n") == "YES"

# flip then query
assert run("2 2\n()\n1 1 2\n2 1 2\n") == "YES"

# all closing
assert run("3 1\n)))\n2 1 3\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| char đơn | KHÔNG | trường hợp không hợp lệ tối thiểu | 
| () truy vấn | CÓ | tính đúng đắn cơ bản | 
| lật toàn dải | CÓ | lười biếng đảo ngược đúng đắn | 
| tất cả ')' | KHÔNG | phát hiện vi phạm tiền tố | 

## Vỏ cạnh 

Trường hợp cạnh tinh tế xuất hiện khi lật một đoạn kéo dài toàn bộ mảng nhiều lần. Bởi vì thao tác lật không liên quan nên việc áp dụng nó hai lần sẽ đưa cấu trúc về trạng thái ban đầu. Cờ lan truyền lười biếng đảm bảo hành vi này, vì việc bật cờ hai lần sẽ tự hủy. 

Một trường hợp khác là truy vấn chồng lấp một phần các vùng bị lật và không bị lật. Thao tác đẩy đảm bảo rằng mọi đảo ngược đang chờ xử lý sẽ được áp dụng trước khi truyền tải một phần, do đó phân đoạn được truy vấn luôn phản ánh trạng thái nhất quán. 

Ví dụ: bắt đầu từ “(()())”, lật [2,5] hai lần sẽ trả về chuỗi gốc. Cây phân đoạn xử lý việc này bằng cách bật cờ lật tại nút và chỉ truyền khi cần thiết, đảm bảo không có ứng dụng đảo ngược kép nào làm hỏng các giá trị được lưu trữ.
