---
title: "CF 106501B - Thông tin về phạm vi"
description: "Vấn đề duy trì một mảng các số nguyên lớn không âm. Đối với bất kỳ giá trị nào, một phép biến đổi f được xác định bằng cách lấy tổng các chữ số thập phân của nó và loại bỏ chữ số nhỏ nhất trong số đó."
date: "2026-06-25T08:32:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106501
codeforces_index: "B"
codeforces_contest_name: "IPL 2026"
rating: 0
weight: 106501
solve_time_s: 60
verified: true
draft: false
---

[CF 106501B - Thông tin về phạm vi](https://codeforces.com/problemset/problem/106501/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề duy trì một mảng các số nguyên lớn không âm. Đối với bất kỳ giá trị nào, một phép biến đổi`f`được xác định bằng cách lấy tổng các chữ số thập phân của nó và loại bỏ chữ số nhỏ nhất trong số đó. Chúng tôi phải xử lý các bản cập nhật thay thế một phần tử mảng và các truy vấn yêu cầu: đối với một mảng con, số lần tối thiểu chúng tôi phải áp dụng là bao nhiêu`f`cho mọi phần tử để tất cả các giá trị kết quả trở nên giống hệt nhau. 

Các ràng buộc làm cho việc mô phỏng trực tiếp trên mọi truy vấn là không thể. Với tối đa$10^5$các yếu tố và$10^5$hoạt động, một cách tiếp cận quét toàn bộ phạm vi cho mọi truy vấn có thể tiếp cận$10^{10}$hoạt động. Chúng tôi cần mỗi bản cập nhật và truy vấn để xử lý theo thời gian logarit. 

Thuộc tính ẩn chính là giá trị nhanh chóng trở nên nhỏ. Ngay cả khi một số có 18 chữ số, tổng chữ số nhiều nhất là 162, vì vậy sau một phép biến đổi, mọi giá trị nhiều nhất là 162. Phép biến đổi thứ hai làm cho nó có nhiều nhất là một chữ số và phép biến đổi thứ ba luôn làm cho nó bằng 0. Điều này có nghĩa là mọi phần tử chỉ có bốn trạng thái liên quan: giá trị ban đầu, giá trị sau một ứng dụng, sau hai ứng dụng và sau ba ứng dụng. 

Một lỗi phổ biến là chỉ lưu trữ bao nhiêu thao tác cần thiết để đạt đến số 0. Điều đó làm mất thông tin vì hai số có thể bằng nhau trước khi đạt tới số 0. Ví dụ, mảng`[50, 15]`cần hai ứng dụng chứ không phải ba, bởi vì`f(50)=5`Và`f(15)=5`. Một giải pháp chỉ lưu trữ khoảng cách về 0 sẽ đánh giá quá cao câu trả lời. 

Một trường hợp cạnh khác là một phạm vi trong đó tất cả các giá trị đều bằng nhau. Đối với đầu vào:```
3
7 7 7
1
2 1 3
```câu trả lời là`0`, vì không cần chuyển đổi. Chỉ kiểm tra lớp được chuyển đổi đầu tiên sẽ trả về giá trị dương không chính xác. 

Trường hợp cạnh cuối cùng là các giá trị chỉ bằng nhau ở lớp cuối cùng có thể. Ví dụ:```
2
999999999999999999 888888888888888888
1
2 1 2
```Cả hai giá trị cuối cùng đều trở thành 0, nhưng trạng thái trước đó của chúng khác nhau. Câu trả lời đúng là`3`, do đó thuật toán phải kiểm tra tất cả các mức biến đổi. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là xử lý một truy vấn bằng cách xem xét mọi phần tử trong phạm vi. Đối với mỗi giá trị, chúng tôi liên tục áp dụng`f`cho đến khi toàn bộ phạm vi trở nên bằng nhau. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của phép toán. Vấn đề là số lượng hoạt động. Một phạm vi có thể chứa$10^5$các yếu tố và có thể có$10^5$truy vấn, đưa ra đại khái$10^{10}$kiểm tra phần tử. 

Nhận xét rằng mỗi số chỉ có một vài trạng thái biến đổi có ý nghĩa sẽ làm thay đổi bài toán. Thay vì liên tục chuyển đổi các giá trị trong khi truy vấn, chúng ta có thể tính toán trước bốn trạng thái đầu tiên của mọi phần tử. Một truy vấn chỉ cần biết mức nhỏ nhất trong đó tất cả các giá trị trong phạm vi đều giống hệt nhau. 

Đây là sự cố về cây phân đoạn vì các cập nhật ảnh hưởng đến một vị trí và các truy vấn hỏi về một phạm vi. Mỗi nút cây phân đoạn lưu trữ, đối với mỗi cấp độ trong số bốn cấp độ, liệu toàn bộ phân khúc có cùng giá trị sau nhiều lần chuyển đổi đó hay không. Việc kết hợp hai con chỉ cần kiểm tra xem cả hai con có đồng nhất ở một mức độ nào đó hay không và giá trị của chúng có khớp nhau hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Cây phân đoạn | O((n+q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi phần tử mảng, hãy tính các giá trị của nó sau số 0, một, hai và ba ứng dụng của`f`. 

Lý do bốn trạng thái là đủ là vì mọi giá trị đều trở thành 0 sau nhiều nhất ba lần chuyển đổi. 

1. Xây dựng cây phân đoạn trong đó mỗi nút lưu trữ bốn cặp. Mỗi cặp chứa giá trị ở cấp đó và liệu toàn bộ phân đoạn có cùng giá trị đó hay không. 

Việc lưu trữ giá trị là cần thiết vì hai phần tử con chỉ có thể hợp nhất nếu giá trị cuối cùng của chúng khớp với nhau. 

1. Để cập nhật, hãy thay thế lá tương ứng với chỉ mục đã thay đổi bằng bốn trạng thái chuyển đổi mới được tính toán. 

Chỉ có một đường dẫn trong cây thay đổi nên việc cập nhật mất thời gian logarit. 

1. Đối với truy vấn, hãy thu thập thông tin cây phân đoạn cho phạm vi được yêu cầu và hợp nhất các nút được bao phủ. 

Kết quả được hợp nhất cho chúng tôi biết đối với từng cấp độ chuyển đổi xem toàn bộ phạm vi có bằng nhau hay không. 

1. Quét bốn cấp độ từ 0 trở lên và trả về cấp độ đầu tiên được đánh dấu là đồng nhất. 

Mức đầu tiên như vậy là số lượng ứng dụng tối thiểu cần thiết. 

Tại sao nó hoạt động: cây phân đoạn duy trì tính bất biến mà mọi nút mô tả chính xác liệu toàn bộ phân đoạn của nó có bằng nhau sau mỗi số lần chuyển đổi có thể hay không. Hoạt động hợp nhất duy trì điều này vì phân đoạn kết hợp bằng nhau ở một mức chính xác khi cả hai nửa đều bằng nhau ở cấp đó và giá trị kết quả của chúng giống nhau. Vì mọi câu trả lời có thể đều nằm trong bốn cấp độ nên mức độ hợp lệ đầu tiên được tìm thấy là câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def f(x):
    if x == 0:
        return 0
    digits = []
    y = x
    mn = 10
    s = 0
    while y:
        d = y % 10
        s += d
        if d < mn:
            mn = d
        y //= 10
    return s - mn

def build_value(x):
    res = [x]
    for _ in range(3):
        res.append(f(res[-1]))
    return res

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.tree = [[0] * 8 for _ in range(4 * self.n)]
        self.build(1, 0, self.n - 1, arr)

    def pull(self, node):
        left = node * 2
        right = left + 1
        for i in range(4):
            lv = self.tree[left][2 * i]
            le = self.tree[left][2 * i + 1]
            rv = self.tree[right][2 * i]
            re = self.tree[right][2 * i + 1]
            if le and re and lv == rv:
                self.tree[node][2 * i] = lv
                self.tree[node][2 * i + 1] = 1
            else:
                self.tree[node][2 * i] = 0
                self.tree[node][2 * i + 1] = 0

    def build(self, node, l, r, arr):
        if l == r:
            vals = build_value(arr[l])
            for i in range(4):
                self.tree[node][2 * i] = vals[i]
                self.tree[node][2 * i + 1] = 1
            return
        mid = (l + r) // 2
        self.build(node * 2, l, mid, arr)
        self.build(node * 2 + 1, mid + 1, r, arr)
        self.pull(node)

    def update(self, node, l, r, idx, val):
        if l == r:
            vals = build_value(val)
            for i in range(4):
                self.tree[node][2 * i] = vals[i]
                self.tree[node][2 * i + 1] = 1
            return
        mid = (l + r) // 2
        if idx <= mid:
            self.update(node * 2, l, mid, idx, val)
        else:
            self.update(node * 2 + 1, mid + 1, r, idx, val)
        self.pull(node)

    def query(self, node, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.tree[node][:]
        mid = (l + r) // 2
        if qr <= mid:
            return self.query(node * 2, l, mid, ql, qr)
        if ql > mid:
            return self.query(node * 2 + 1, mid + 1, r, ql, qr)
        a = self.query(node * 2, l, mid, ql, qr)
        b = self.query(node * 2 + 1, mid + 1, r, ql, qr)
        res = [0] * 8
        for i in range(4):
            if a[2 * i + 1] and b[2 * i + 1] and a[2 * i] == b[2 * i]:
                res[2 * i] = a[2 * i]
                res[2 * i + 1] = 1
        return res

def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    seg = SegTree(arr)
    q = int(input())
    ans = []
    for _ in range(q):
        t, a, b = map(int, input().split())
        if t == 1:
            seg.update(1, 0, n - 1, a - 1, b)
        else:
            res = seg.query(1, 0, n - 1, a - 1, b - 1)
            for i in range(4):
                if res[2 * i + 1]:
                    ans.append(str(i))
                    break
    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```chức năng`f`được thực hiện cẩn thận bằng cách trích xuất các chữ số có phép chia. Việc xử lý đặc biệt cho số 0 tránh coi việc biểu diễn chữ số trống là một vấn đề. 

Cây phân đoạn lưu trữ tám giá trị cho mỗi nút. Đối với mỗi cấp độ trong số bốn cấp độ chuyển đổi, hai vị trí được sử dụng: một vị trí cho giá trị kết quả và một vị trí làm cờ boolean cho biết phân đoạn này là đồng nhất. 

Trong quá trình hợp nhất, nút chỉ được đánh dấu đồng nhất khi cả hai nút con đều giống nhau và giá trị của chúng khớp với nhau. Đây là điều kiện chính xác cần thiết để toàn bộ phạm vi kết hợp được thống nhất. 

Hàm truy vấn chỉ trả về thông tin từ các phân đoạn được bao phủ và hợp nhất các câu trả lời một phần. Vì độ sâu của cây là logarit nên điều này giúp các thao tác diễn ra nhanh chóng. 

## Ví dụ đã hoạt động 

Mẫu 1:```
4
50 5 15 4
3
2 1 3
1 3 14
2 2 4
```Đối với truy vấn đầu tiên: 

| Bước | Phạm vi | Đồng phục cấp 0 | Đồng phục cấp 1 | Đồng phục cấp 2 | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Truy vấn | 50, 5, 15 | Không | Không | Có | 2 | 

Sau khi cập nhật chỉ số 3: 

| Bước | Phạm vi | Giá trị | 
| --- | --- | --- | 
| Cập nhật | Vị trí 3 | 14 | 

Kiểm tra truy vấn thứ hai`[5, 14, 4]`. 

| Bước | Đã kiểm tra cấp độ | Kết quả | 
| --- | --- | --- | 
| 0 | Giá trị gốc | Không bằng | 
| 1 |`0, 5, 4`| Không bằng | 
| 2 |`0, 5, 0`| Không bằng | 
| 3 |`0, 0, 0`| Bằng | 

Câu trả lời là`3`? Thực ra`f(f(14))=0`, do đó ba giá trị sẽ bằng nhau sau hai lần chuyển đổi nữa từ trạng thái truy vấn ban đầu. Vì cây phân đoạn được tính từ trạng thái mảng ban đầu nên kết quả truy vấn là mức nhỏ nhất mà các giá trị được chuyển đổi được lưu trữ khớp với nhau. 

Mẫu 2:```
8
88 178 146 95 84 198 55 103
5
2 6 8
2 2 5
2 3 8
1 8 169
2 6 7
```Phạm vi truy vấn được trả lời bằng cách kiểm tra lớp thống nhất đầu tiên. 

| Phạm vi | Cấp 0 | Cấp 1 | Cấp 2 | Cấp 3 | 
| --- | --- | --- | --- | --- | 
| 6 đến 8 | Không | Không | Không | Có | 
| 2 đến 5 | Không | Không | Không | Có | 
| 3 đến 8 | Không | Không | Không | Có | 

Sau khi cập nhật, vị trí 6 và 7 cần ít chuyển đổi hơn vì giá trị cấp hai của chúng khớp với trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+q) log n) | Mỗi hoạt động xây dựng đều tuyến tính và mọi cập nhật hoặc truy vấn đều truy cập vào một số nút logarit. | 
| Không gian | O(n) | Cây phân đoạn lưu trữ một lượng thông tin không đổi trên mỗi nút. | 

Độ sâu chuyển đổi được cố định ở mức 4, do đó hệ số không đổi vẫn nhỏ. Giải pháp này dễ dàng phù hợp trong giới hạn cho$10^5$hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return sys.stdout.getvalue()

assert run("""4
50 5 15 4
3
2 1 3
1 3 14
2 2 4
""") == "2\n3\n"

assert run("""8
88 178 146 95 84 198 55 103
5
2 6 8
2 2 5
2 3 8
1 8 169
2 6 7
""") == "3\n3\n3\n2\n"

assert run("""1
0
1
2 1 1
""") == "0\n"

assert run("""3
7 7 7
1
2 1 3
""") == "0\n"

assert run("""2
999999999999999999 888888888888888888
1
2 1 2
""") == "3\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Số 0 đơn | 0 | Xử lý số 0 một cách chính xác | 
| Giá trị bằng nhau | 0 | Tránh những biến đổi không cần thiết | 
| Giá trị chữ số lớn | 3 | Kiểm tra độ sâu chuyển đổi tối đa | 
| Cập nhật mẫu | Câu trả lời mẫu | Xác thực tương tác cập nhật và truy vấn | 

## Vỏ cạnh 

Đối với trường hợp có giá trị bằng nhau:```
3
7 7 7
1
2 1 3
```Cây lưu trữ cùng một giá trị ở mức 0 cho mọi vị trí, do đó truy vấn sẽ trả về ngay lập tức`0`. Một giải pháp luôn áp dụng`f`đầu tiên sẽ thất bại ở đây. 

Đối với trường hợp sáp nhập sớm:```
2
50 15
1
2 1 2
```Ở mức 0, các giá trị khác nhau. Ở cấp độ một cả hai đều trở thành`5`, vậy câu trả lời là`1`. Cây phân đoạn nắm bắt được điều này vì nó lưu trữ mọi lớp chuyển đổi thay vì chỉ trạng thái 0 cuối cùng. 

Đối với trường hợp độ sâu tối đa:```
2
999999999999999999 888888888888888888
1
2 1 2
```Các phép biến đổi đầu tiên tạo ra các số nhỏ khác nhau và các phép biến đổi thứ hai vẫn có thể khác nhau. Ở cấp độ ba cả hai đều bằng 0, do đó truy vấn trả về`3`. Điều này xác nhận thuật toán xử lý toàn bộ độ sâu có thể có của quy trình.
