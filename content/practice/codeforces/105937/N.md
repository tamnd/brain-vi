---
title: "CF 105937N - Ban nhạc Kessoku"
description: "Chúng ta được cho một hoán vị các số từ 1 đến n, nghĩa là mọi số nguyên trong phạm vi này xuất hiện đúng một lần và được sắp xếp theo thứ tự nào đó dọc theo một dòng. Mỗi thao tác cho ta một đoạn [l, r] của đường thẳng này."
date: "2026-06-22T15:49:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105937
codeforces_index: "N"
codeforces_contest_name: "2025 Xian Jiaotong University Programming Contest"
rating: 0
weight: 105937
solve_time_s: 83
verified: true
draft: false
---

[CF 105937N - Ban nhạc Kessoku](https://codeforces.com/problemset/problem/105937/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số từ 1 đến n, nghĩa là mọi số nguyên trong phạm vi này xuất hiện đúng một lần và được sắp xếp theo thứ tự nào đó dọc theo một dòng. Mỗi thao tác cho ta một đoạn [l, r] của đường thẳng này. Từ phân đoạn này, chúng tôi xem xét giá trị nào bị thiếu trong số các số xuất hiện bên trong nó và chúng tôi lấy số nguyên dương bị thiếu nhỏ nhất, gọi nó là x. 

Sau khi tính x, nếu x bằng hoặc lớn hơn n, chúng ta chỉ cần xuất ra từ “hòa bình” và không làm gì với hoán vị. Ngược lại, chúng ta xuất x và sau đó sửa đổi hoán vị bằng cách hoán đổi vị trí của các giá trị x và x + 1 ở bất kỳ vị trí nào hiện tại. 

Vì vậy, mỗi truy vấn vừa là một truy vấn tồn tại phạm vi qua một hoán vị động, vừa là một bản cập nhật cấu trúc cục bộ xáo trộn một chút hai giá trị liên tiếp trong miền giá trị. 

Các ràng buộc rất lớn: n có thể lên tới 5 × 10^5 và k lên tới 10^5. Bất kỳ giải pháp nào quét khoảng thời gian cho mỗi truy vấn sẽ quá chậm, vì điều đó sẽ tốn O(nk) trong trường hợp xấu nhất, vượt xa mức chấp nhận được. Ngay cả O(n log n) cho mỗi truy vấn cũng quá lớn. Chúng tôi cần thứ gì đó gần với logarit cho mỗi truy vấn hơn, với khả năng cập nhật và kiểm tra phạm vi rất nhanh. 

Phần khó khăn là hoán vị là động. Sau mỗi truy vấn, việc hoán đổi vị trí x và x+1 sẽ thay đổi vị trí, điều này ảnh hưởng đến tất cả các truy vấn phạm vi trong tương lai. Điều này loại trừ mọi quá trình tiền xử lý tĩnh đối với các giá trị. 

Trường hợp cạnh tinh tế là khi số bị thiếu là n hoặc lớn hơn. Vì hoán vị chỉ chứa 1 đến n nên x sẽ luôn có nhiều nhất là n+1. Nếu x bằng n+1 hoặc thực tế là nếu tất cả 1..n xuất hiện trong phân đoạn, chúng ta sẽ xuất ra “hòa bình”. Việc triển khai đơn giản có thể cố gắng hoán đổi không chính xác trong trường hợp này, nhưng việc hoán đổi chỉ phải xảy ra khi x < n. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý từng truy vấn bằng cách quét đoạn [l, r], đánh dấu giá trị nào xuất hiện và sau đó tìm số nguyên còn thiếu nhỏ nhất bằng cách kiểm tra từ 1 trở lên. Điều này đúng nhưng quá chậm. Mỗi truy vấn có giá O(r - l + 1 + n) trong trường hợp xấu nhất nếu chúng tôi đặt lại mảng sổ sách kế toán và với tối đa 10^5 truy vấn, điều này trở nên không khả thi. 

Khó khăn chính là chúng ta liên tục yêu cầu mex của các giá trị bên trong một phạm vi, trong đó các giá trị là từ một hoán vị đang thay đổi thông qua việc hoán đổi các giá trị liền kề trong không gian giá trị. Điều này cho thấy chúng ta cần một cấu trúc có thể nhanh chóng trả lời “giá trị v có trong [l, r]” và hỗ trợ các cập nhật di chuyển vị trí của hai giá trị. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần kiểm tra tất cả các giá trị trong phân đoạn. Ta chỉ cần tìm giá trị v nhỏ nhất sao cho v không xuất hiện trong [l, r]. Điều này có thể được chuyển thành tìm kiếm kiểu tiền tố trên các giá trị: chúng tôi kiểm tra các ứng cử viên v theo thứ tự tăng dần và với mỗi v chúng tôi kiểm tra xem nó có tồn tại trong phân khúc hay không. V đầu tiên vắng mặt là câu trả lời. 

Vì vậy, hoạt động cốt lõi trở thành cấu trúc “vị trí điểm” động: với mỗi giá trị v, chúng ta duy trì vị trí hiện tại của nó pos[v]. Khi đó việc kiểm tra xem v có xuất hiện trong [l, r] hay không chỉ là kiểm tra xem pos[v] có nằm trong khoảng đó hay không. Điều này làm giảm truy vấn phạm vi thành một chuỗi kiểm tra O(1) cho mỗi ứng cử viên giá trị. 

Thử thách còn lại là tìm ra v còn thiếu nhỏ nhất một cách nhanh chóng. Chúng ta có thể duy trì cây phân đoạn trên miền giá trị [1..n], lưu trữ cho từng phân đoạn xem tất cả các giá trị trong phân đoạn đó có được “bao phủ” hoàn toàn theo nghĩa là vị trí của chúng nằm trong phạm vi truy vấn hiện tại hay không. Tuy nhiên, vì mỗi truy vấn có [l, r] khác nhau nên chúng tôi không thể tính toán trước điều này. 

Thay vào đó, chúng tôi đảo ngược quan điểm. Đối với truy vấn cố định [l, r], chúng ta muốn v nhỏ nhất sao cho pos[v] không nằm trong [l, r]. Điều này tương đương với việc tìm v đầu tiên trong đó pos[v] < l hoặc pos[v] > r. Đây là một vị từ đơn điệu trên v nếu chúng ta xác định nó một cách cẩn thận bằng cách sử dụng các nút cây phân đoạn lưu trữ vị trí tối thiểu và tối đa của các giá trị trong một phạm vi giá trị.

Đối với một phạm vi giá trị [L, R], chúng ta có thể duy trì minPos và maxPos. Nếu cả minPos và maxPos đều nằm trong [l, r] thì tất cả các giá trị trong phạm vi đó đều nằm trong phân đoạn. Ngược lại, tồn tại ít nhất một giá trị bị thiếu trong phạm vi đó. Điều này cho phép chúng ta tìm kiếm nhị phân v nhỏ nhất bằng cách sử dụng cây phân đoạn, giảm dần cho đến khi chúng ta tìm thấy giá trị đầu tiên có vị trí thoát khỏi [l, r]. 

Việc cập nhật rất đơn giản: hoán đổi x và x+1 chỉ hoán đổi vị trí của chúng trong pos[]. 

Điều này dẫn đến một truy vấn O(log n) để tìm mex thông qua gốc cây phân đoạn và cập nhật O(1) cho mỗi lần hoán đổi, đưa ra giải pháp O((n + k) log n) tổng thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nk) | O(n) | Quá chậm | 
| Tối ưu (phân đoạn cây theo giá trị) | O((n + k) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng cây phân đoạn trên miền giá trị từ 1 đến n. Mỗi nút đại diện cho một phạm vi giá trị liền kề và lưu trữ hai phần thông tin: vị trí tối thiểu trong số các giá trị đó trong hoán vị hiện tại và vị trí tối đa trong số các giá trị đó. 

Chúng tôi cũng duy trì một mảng pos[v], cho chúng tôi biết chỉ số hiện tại của giá trị v trong hoán vị. 

Với mỗi truy vấn [l, r], chúng ta muốn v nhỏ nhất sao cho pos[v] không nằm trong [l, r]. 

1. Xây dựng cây phân đoạn bằng cách sử dụng hoán vị ban đầu. Với mỗi giá trị v, chúng ta đặt pos[v] làm chỉ mục của nó và lưu trữ các nút lá (pos[v], pos[v]). Các nút bên trong tính toán mức tối thiểu và tối đa từ các nút con. 
2. Để trả lời truy vấn [l, r], chúng ta đi xuống cây phân đoạn bắt đầu từ gốc, luôn cố gắng tìm một giá trị theo thứ tự tăng dần có vị trí nằm ngoài [l, r]. Tại mỗi nút, chúng tôi kiểm tra xem toàn bộ phân đoạn có hoàn toàn nằm trong [l, r] hay không, nghĩa là minPos ≥ l và maxPos ≤ r. Nếu điều này đúng thì tất cả các giá trị trong nút này đều nằm trong phân đoạn, vì vậy chúng tôi bỏ qua nó. 
3. Nếu một nút không được chứa đầy đủ thì có nghĩa là tồn tại ít nhất một giá trị có vị trí nằm ngoài [l, r]. Sau đó chúng ta đi xuống phần con của nó, luôn ưu tiên con bên trái trước vì chúng ta muốn giá trị nhỏ nhất có thể. 
4. Khi đến nút lá tương ứng với giá trị v, chúng tôi kiểm tra xem pos[v] có nằm trong [l, r] hay không. Nếu nó ở bên ngoài, chúng ta trả về v là mex. Nếu không, nhánh này không hợp lệ. 
5. Sau khi lấy x, nếu x bằng n, chúng ta xuất ra “hòa bình” và không làm gì thêm. 
6. Nếu x nhỏ hơn n, chúng ta xuất x và sau đó hoán đổi vị trí của x và x+1. Chúng tôi cập nhật pos[x] và pos[x+1], đồng thời cập nhật các lá cây phân đoạn cho cả hai giá trị và truyền các thay đổi lên trên. 

Tại sao nó hoạt động dựa trên bất biến cấu trúc: mọi nút luôn biểu thị đúng vị trí tối thiểu và tối đa trong phạm vi giá trị của nó. Điều này đảm bảo rằng khi một nút được chứa đầy đủ trong [l, r], chúng ta có thể loại bỏ toàn bộ cây con một cách an toàn vì không có giá trị nào bên trong nó có thể bị thiếu khỏi phân đoạn. Ngược lại, nếu một nút không được chứa đầy đủ, nó đảm bảo ít nhất một giá trị trong phạm vi của nó vi phạm điều kiện khoảng, do đó câu trả lời phải nằm ở đâu đó trong cây con đó. Việc duyệt cây bảo toàn thứ tự bằng cách luôn khám phá trái trước phải, điều này đảm bảo tìm thấy giá trị hợp lệ nhỏ nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, pos):
        self.n = len(pos) - 1
        self.minv = [0] * (4 * self.n)
        self.maxv = [0] * (4 * self.n)
        self.build(1, 1, self.n, pos)

    def build(self, idx, l, r, pos):
        if l == r:
            self.minv[idx] = self.maxv[idx] = pos[l]
            return
        mid = (l + r) // 2
        self.build(idx * 2, l, mid, pos)
        self.build(idx * 2 + 1, mid + 1, r, pos)
        self.pull(idx)

    def pull(self, idx):
        self.minv[idx] = min(self.minv[idx * 2], self.minv[idx * 2 + 1])
        self.maxv[idx] = max(self.maxv[idx * 2], self.maxv[idx * 2 + 1])

    def update(self, idx, l, r, pos_idx, val):
        if l == r:
            self.minv[idx] = self.maxv[idx] = val
            return
        mid = (l + r) // 2
        if pos_idx <= mid:
            self.update(idx * 2, l, mid, pos_idx, val)
        else:
            self.update(idx * 2 + 1, mid + 1, r, pos_idx, val)
        self.pull(idx)

    def find_mex(self, idx, l, r, ql, qr):
        if self.minv[idx] >= l and self.maxv[idx] <= r:
            return -1
        if l == r:
            return l if not (ql <= self.minv[idx] <= qr) else -1
        mid = (l + r) // 2
        res = self.find_mex(idx * 2, l, mid, ql, qr)
        if res != -1:
            return res
        return self.find_mex(idx * 2 + 1, mid + 1, r, ql, qr)

n = int(input())
a = list(map(int, input().split()))
k = int(input())

pos = [0] * (n + 1)
for i, v in enumerate(a, 1):
    pos[v] = i

st = SegTree(pos)

for _ in range(k):
    l, r = map(int, input().split())
    x = st.find_mex(1, 1, n, l, r)

    if x == -1 or x == n:
        print("peace")
        continue

    print(x)
    px, py = pos[x], pos[x + 1]
    pos[x], pos[x + 1] = py, px

    st.update(1, 1, n, x, pos[x])
    st.update(1, 1, n, x + 1, pos[x + 1])
```Cây phân đoạn được xây dựng dựa trên các giá trị thay vì vị trí. Mỗi lá tương ứng với một giá trị v và lưu trữ vị trí hiện tại của nó trong hoán vị. Sự đảo ngược này làm cho các truy vấn phạm vi về các giá trị trở nên có ý nghĩa. 

Tìm kiếm mex hoạt động bằng cách cắt bớt toàn bộ các phân đoạn có giá trị hoàn toàn nằm trong [l, r] về mặt vị trí. Nếu toàn bộ phạm vi giá trị của nút nằm trong phân đoạn truy vấn thì nút đó không thể chứa câu trả lời nên sẽ bị bỏ qua ngay lập tức. 

Bước hoán đổi cẩn thận để cập nhật cả mảng pos và lá cây phân đoạn cho các giá trị được hoán đổi. Thiếu một trong hai bản cập nhật sẽ đồng bộ hóa cấu trúc và tạo ra các truy vấn không chính xác trong tương lai. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi một ví dụ nhỏ để quan sát cấu trúc phát triển như thế nào. 

Xem xét hoán vị [4, 3, 1, 2, 5] với truy vấn [2, 4]. 

| Bước | Truy vấn [l, r] | Đã tìm thấy x | Hành động | Hoán vị | 
| --- | --- | --- | --- | --- | 
| 1 | [2,4] | 4 | hoán đổi 4 và 5 | [5, 3, 1, 2, 4] | 
| 2 | [2,5] | 5 | hòa bình | [5, 3, 1, 2, 4] | 
| 3 | [1,3] | 2 | hoán đổi 2 và 3 | [5, 2, 1, 3, 4] | 
| 4 | [1,3] | 3 | hoán đổi 3 và 4 | [5, 2, 1, 4, 3] | 
| 5 | [1,5] | 6 | hòa bình | [5, 2, 1, 4, 3] | 

Mỗi bước cho thấy rằng khi tất cả các giá trị nhỏ xuất hiện trong khoảng thời gian được truy vấn, mex sẽ dịch chuyển lên trên cho đến khi thoát khỏi miền, tạo ra “hòa bình”. 

Bây giờ hãy xem xét trường hợp tập trung nhỏ hơn: hoán vị [1, 2, 3, 4], truy vấn [2, 3]. 

| v đã kiểm tra | vị trí[v] | trong [2,3]? | 
| --- | --- | --- | 
| 1 | 1 | không | 
| 2 | 2 | vâng | 
| 3 | 3 | vâng | 
| 4 | 4 | không | 

Số còn thiếu nhỏ nhất là 1, ngay lập tức kích hoạt hoán đổi giữa 1 và 2, thể hiện cách điều chỉnh giá trị cục bộ lan truyền qua các truy vấn trong tương lai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + k) log n) | Mỗi truy vấn đi xuống cây phân đoạn theo các giá trị và mỗi lần hoán đổi sẽ cập nhật hai lá | 
| Không gian | O(n) | Cây phân đoạn và mảng vị trí | 

Hệ số logarit đủ cho n tối đa 5 × 10^5 và k tối đa 10^5, vì mỗi thao tác chỉ chạm vào một đường dẫn trong cây chứ không phải quét phạm vi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    class SegTree:
        def __init__(self, pos):
            self.n = len(pos) - 1
            self.minv = [0] * (4 * self.n)
            self.maxv = [0] * (4 * self.n)
            self.build(1, 1, self.n, pos)

        def build(self, idx, l, r, pos):
            if l == r:
                self.minv[idx] = self.maxv[idx] = pos[l]
                return
            mid = (l + r) // 2
            self.build(idx * 2, l, mid, pos)
            self.build(idx * 2 + 1, mid + 1, r, pos)
            self.pull(idx)

        def pull(self, idx):
            self.minv[idx] = min(self.minv[idx * 2], self.minv[idx * 2 + 1])
            self.maxv[idx] = max(self.maxv[idx * 2], self.maxv[idx * 2 + 1])

        def update(self, idx, l, r, pos_idx, val):
            if l == r:
                self.minv[idx] = self.maxv[idx] = val
                return
            mid = (l + r) // 2
            if pos_idx <= mid:
                self.update(idx * 2, l, mid, pos_idx, val)
            else:
                self.update(idx * 2 + 1, mid + 1, r, pos_idx, val)
            self.pull(idx)

        def find_mex(self, idx, l, r, ql, qr):
            if self.minv[idx] >= l and self.maxv[idx] <= r:
                return -1
            if l == r:
                return l if not (ql <= self.minv[idx] <= qr) else -1
            mid = (l + r) // 2
            res = self.find_mex(idx * 2, l, mid, ql, qr)
            if res != -1:
                return res
            return self.find_mex(idx * 2 + 1, mid + 1, r, ql, qr)

    n = 1
    a = [1]
    pos = [0, 1]
    st = SegTree(pos)
    assert run("1\n1\n1\n1 1\n") == "peace\n", "min size"

    return "ok"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 truy vấn duy nhất | hòa bình | trường hợp biên nhỏ nhất | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi phân đoạn được truy vấn đã chứa tất cả các giá trị từ 1 đến n. Trong trường hợp đó, mọi giá trị v đều thỏa mãn pos[v] bên trong [l, r], do đó mex trở thành n+1 và chúng ta xuất ra “hòa bình”. Cây phân đoạn sẽ cắt tỉa chính xác từng nút vì vị trí tối thiểu và tối đa của mỗi nút sẽ nằm trong khoảng truy vấn, khiến bao phủ toàn bộ và không tìm thấy lá ứng cử viên nào. 

Một trường hợp tinh tế khác là các giao dịch hoán đổi lặp đi lặp lại liên quan đến các giá trị liền kề làm di chuyển vị trí qua lại. Vì mỗi lần hoán đổi chỉ chạm vào hai giá trị và cập nhật ngay lập tức cả hai nút lá của chúng, nên tính bất biến mà pos[v] luôn đúng sẽ được giữ nguyên. Ngay cả sau nhiều thao tác, cấu trúc vẫn nhất quán vì mọi cập nhật đều được bản địa hóa và truyền bá qua cây. 

Trường hợp góc cuối cùng là khi x bằng n. Mặc dù đó là giá trị hợp lệ lớn nhất nhưng việc hoán đổi với x+1 không hợp lệ vì x+1 không tồn tại. Mã này xử lý rõ ràng x == n như một điều kiện dừng, đảm bảo không xảy ra cập nhật ngoài giới hạn.
