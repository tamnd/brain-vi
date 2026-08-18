---
title: "CF 102272D - C\u00e1nh \u0110\u1ed3ng Hoa"
description: "Chúng tôi có một mảng (N) ô hoa. Ô (i) ban đầu chứa hoa (Ai), và các thao tác được xử lý theo thứ tự. Thao tác cập nhật sẽ chọn một khoảng ([l,r]). Tại vị trí (i) bên trong khoảng đó, nó thêm chính xác (i-l+1) hoa."
date: "2026-08-17T11:10:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102272
codeforces_index: "D"
codeforces_contest_name: "HCW 19 Individual Day 1"
rating: 0
weight: 102272
solve_time_s: 218
verified: false
draft: false
---

[CF 102272D - C\u00e1nh \u0110\u1ed3ng Hoa](https://codeforces.com/problemset/problem/102272/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 38 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng (N) ô hoa. Ô (i) ban đầu chứa (A_i) hoa và các thao tác được xử lý theo thứ tự. 

Thao tác cập nhật sẽ chọn một khoảng ([l,r]). Tại vị trí (i) bên trong khoảng đó, nó thêm chính xác (i-l+1) hoa. Do đó, các giá trị gia tăng tạo thành một cấp số cộng: 

[ 
1,2,3,\ldots,r-l+1. 
] 

Thao tác truy vấn chọn ([u,v]) và yêu cầu tổng số hoa hiện tại trong khoảng thời gian đó. 

Phần khó khăn là bản cập nhật không phải là sự bổ sung liên tục. Ví dụ: cập nhật ([3,6]) thêm (1,2,3,4), do đó số lượng được thêm vào phụ thuộc tuyến tính vào vị trí: 

[ 
i-l+1=i+(1-l). 
] 

Dạng tuyến tính đó chính là chìa khóa của giải pháp. 

Có tối đa (10^5) lô và (10^5) thao tác trong một trường hợp thử nghiệm, với tối đa bốn trường hợp thử nghiệm. Một thao tác (O(N)) sẽ dẫn đến (10^{10}) hoạt động trong trường hợp xấu nhất, vượt xa giới hạn hai giây cho phép. Chúng tôi cần mỗi lần cập nhật và truy vấn để thực hiện (O(\log N)), đưa ra các thao tác khoảng (10^5\log N) cho mỗi trường hợp thử nghiệm. 

Trường hợp ranh giới đầu tiên là cập nhật một phần tử. Ví dụ,```
1
1
5
2
1 1 1
2 1 1
```Bản cập nhật thêm 1 bông hoa nên đáp án là```
6
```Việc triển khai bất cẩn coi tiến trình bắt đầu bằng 0 sẽ tạo ra (5). 

Trường hợp ranh giới thứ hai là một bản cập nhật kết thúc chính xác tại (N). Ví dụ,```
1
5
0 0 0 0 0
2
1 2 5
2 1 5
```Bản cập nhật lần lượt thêm (0,1,2,3,4) vào các vị trí (1,2,3,4,5), vì vậy câu trả lời là```
10
```Trường hợp thứ ba là một bản cập nhật có điểm cuối bên trái không phải là (1). Vì```
1
5
0 0 0 0 0
2
1 3 5
2 1 5
```các phép cộng là (0,0,1,2,3), cho```
6
```Biểu thức phải sử dụng điểm cuối bên trái thực tế (l). Thay thế nó bằng (i) hoặc giả sử mọi tiến trình đều bắt đầu từ vị trí (1), sẽ cho kết quả sai. 

Trường hợp thứ tư là cập nhật chồng chéo. Vì```
1
4
0 0 0 0
3
1 1 3
1 2 4
2 1 4
```bản cập nhật đầu tiên cho ([1,2,3,0]), bản cập nhật thứ hai cho ([1,3,5,3]) và câu trả lời cuối cùng là (12). Cập nhật là phần bổ sung nên chúng không thể ghi đè lên tác động của các thao tác trước đó. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp lưu trữ mảng thực tế. Để cập nhật ([l,r]), chúng ta chỉ cần lặp từ (l) đến (r) và thêm (i-l+1) vào mọi vị trí. Đối với một truy vấn ([u,v]), chúng tôi lặp lại khoảng đó và tính tổng của nó. Điều này đúng vì mọi thao tác đều được áp dụng chính xác vào các vị trí mà nó mô tả. 

Vấn đề là khối lượng công việc. Một bản cập nhật có thể chạm vào tất cả (N) vị trí và một truy vấn cũng có thể kiểm tra tất cả các vị trí (N). Với các phép toán (Q=10^5) và (N=10^5), một chuỗi các phép toán toàn dải có thể yêu cầu khoảng 

[ 
NQ=10^{10} 
] 

truy cập mảng. Đó là một số bậc độ lớn quá lớn. 

Sức mạnh vũ phu có tác dụng vì nó hiện thực hóa rõ ràng mọi số lượng hoa. Nó thất bại vì các bản cập nhật có cấu trúc mà chúng ta đang vứt đi. 

Quan sát giúp mở ra giải pháp nhanh hơn là mọi bản cập nhật đều bổ sung thêm một hàm tuyến tính của vị trí. Trên ([l,r]), 

[ 
i-l+1 = 1\cdot i +(1-l). 
] 

Vì vậy, thay vì coi bản cập nhật là các phần bổ sung riêng lẻ (r-l+1), chúng ta có thể coi nó như việc thêm cùng một hàm tuyến tính (ai+b) vào toàn bộ phân khúc. 

Cây phân đoạn với sự lan truyền lười biếng là sự phù hợp tự nhiên. Đối với mỗi nút cây đại diện cho ([L,R]), chúng tôi lưu trữ tổng số hoa trong đoạn đó. Thẻ lười của nó lưu hai số (a,b), nghĩa là mọi vị trí (i) trong nút này vẫn cần 

[ 
ai+b 
] 

thêm vào nó. 

Giả sử nút bao gồm ([L,R]). Tổng đóng góp của một bản cập nhật lười biếng như vậy là 

a\sum_{i=L}^{R}i+b(R-L+1). 
] 

Tổng chỉ số có dạng đóng 

\frac{(L+R)(R-L+1)}2. 
] 

Vì vậy, toàn bộ phân khúc có thể được cập nhật trong (O(1)) sau khi đã đạt đến phân khúc đó. Quá trình lan truyền lười biếng trì hoãn việc đẩy hàm tuyến tính vào các phần tử con của nó cho đến khi chúng ta thực sự cần kiểm tra chúng. 

Đối với hoạt động ban đầu, chúng tôi chỉ cần sử dụng 

[ 
a=1,\qquad b=1-l. 
] 

Cây phân đoạn tương tự có thể trả lời tổng phạm vi trong (O(\log N)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(NQ)) | (O(N)) | Quá chậm | 
| Tối ưu | (O((N+Q)\log N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn trên mảng ban đầu. Mỗi nút lưu trữ tổng số hoa trong khoảng thời gian của nó. Thông tin lười biếng bắt đầu từ 0 vì ban đầu không có bản cập nhật trì hoãn nào tồn tại. 
2. Biểu diễn cập nhật ([l,r]) dưới dạng hàm tuyến tính 

[ 
f(i)=i-l+1=i+(1-l). 
] 

Do đó độ dốc của nó là (a=1), và điểm giao nhau của nó là (b=1-l). 

1. Khi một nút cây ([L,R]) được cập nhật hoàn toàn, hãy tăng tổng lưu trữ của nó lên 

[ 
a\frac{(L+R)(R-L+1)}2+b(R-L+1). 
] 

Đồng thời, thêm (a) và (b) vào thẻ lười của nút. Chúng ta không thăm các con của nó vì toàn bộ khoảng đã nhận được hàm tuyến tính giống nhau. 

1. Khi một bản cập nhật giao nhau một phần với một nút, trước tiên hãy đẩy hàm tuyến tính đang chờ xử lý của nút đó tới các nút con của nó. Sau đó cập nhật đệ quy hai nút con và tính lại tổng của nút hiện tại từ tổng của chúng. 

Thao tác đẩy sử dụng chính xác công thức giống như một bản cập nhật thông thường. Một lớp con bao phủ ([L,R]) nhận được hàm đang chờ xử lý (ai+b), do đó tổng của nó tăng theo tổng chuỗi số học tương ứng. 

1. Đối với truy vấn tổng phạm vi ([u,v]), trả về số 0 cho nút rời rạc và trả về tổng được lưu trữ cho nút được bao phủ hoàn toàn. Đối với giao lộ một phần, hãy đẩy hàm lười biếng đang chờ xử lý trước khi truy vấn phần tử con, sau đó thêm kết quả của chúng. 
2. Xử lý tất cả các thao tác (Q) theo thứ tự ban đầu. Đối với loại (1), áp dụng cập nhật tuyến tính. Đối với loại (2), truy vấn khoảng thời gian cần thiết và in kết quả. 

### Tại sao nó hoạt động

Điều bất biến là tổng được lưu trữ của mỗi nút cây phân đoạn bằng tổng thực của mảng hiện tại trong khoảng thời gian của nút đó, bao gồm mọi cập nhật đã đến nút đó. Cặp lười ((a,b)) của nó biểu thị chính xác hàm tuyến tính vẫn phải được áp dụng cho mọi vị trí trong khoảng của nút đó và đã được đưa vào tổng được lưu trữ của nút đó. 

Khi một bản cập nhật hoàn chỉnh đến một nút, tổng số học dạng đóng sẽ cộng chính xác phần đóng góp của bản cập nhật cho mọi vị trí trong khoảng đó. Khi thẻ lười được đẩy, chức năng giống hệt nhau sẽ được áp dụng cho cả hai phần tử con, các khoảng của chúng phân chia khoảng cha. Do đó, bất biến được giữ nguyên sau mỗi lần cập nhật. 

Một truy vấn có thể lấy một nút hoàn chỉnh đã đúng hoặc kết hợp đệ quy các tổng con chính xác. Vì mọi vị trí được truy vấn đều thuộc về chính xác các nút cây rời rạc có liên quan nên giá trị trả về chính xác là tổng số hoa được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1_000_000)

class SegmentTree:
    def __init__(self, arr):
        self.n = len(arr)
        size = 4 * self.n + 5
        self.tree = [0] * size
        self.lazy_a = [0] * size
        self.lazy_b = [0] * size
        self.arr = arr
        self._build(1, 1, self.n)

    def _build(self, node, left, right):
        if left == right:
            self.tree[node] = self.arr[left - 1]
            return

        mid = (left + right) // 2
        self._build(node * 2, left, mid)
        self._build(node * 2 + 1, mid + 1, right)
        self.tree[node] = self.tree[node * 2] + self.tree[node * 2 + 1]

    @staticmethod
    def _index_sum(left, right):
        length = right - left + 1
        return (left + right) * length // 2

    def _apply(self, node, left, right, a, b):
        length = right - left + 1
        index_sum = self._index_sum(left, right)

        self.tree[node] += a * index_sum + b * length
        self.lazy_a[node] += a
        self.lazy_b[node] += b

    def _push(self, node, left, right):
        a = self.lazy_a[node]
        b = self.lazy_b[node]

        if a == 0 and b == 0:
            return

        if left != right:
            mid = (left + right) // 2
            self._apply(node * 2, left, mid, a, b)
            self._apply(node * 2 + 1, mid + 1, right, a, b)

        self.lazy_a[node] = 0
        self.lazy_b[node] = 0

    def update(self, ql, qr):
        self._update(1, 1, self.n, ql, qr)

    def _update(self, node, left, right, ql, qr):
        if qr < left or right < ql:
            return

        if ql <= left and right <= qr:
            # Add i - ql + 1 = i + (1 - ql).
            self._apply(node, left, right, 1, 1 - ql)
            return

        self._push(node, left, right)

        mid = (left + right) // 2
        self._update(node * 2, left, mid, ql, qr)
        self._update(node * 2 + 1, mid + 1, right, ql, qr)

        self.tree[node] = self.tree[node * 2] + self.tree[node * 2 + 1]

    def query(self, ql, qr):
        return self._query(1, 1, self.n, ql, qr)

    def _query(self, node, left, right, ql, qr):
        if qr < left or right < ql:
            return 0

        if ql <= left and right <= qr:
            return self.tree[node]

        self._push(node, left, right)

        mid = (left + right) // 2
        return (
            self._query(node * 2, left, mid, ql, qr)
            + self._query(node * 2 + 1, mid + 1, right, ql, qr)
        )

def solve():
    t = int(input())
    output = []

    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))

        q = int(input())
        seg = SegmentTree(arr)

        for _ in range(q):
            typ, x, y = map(int, input().split())

            if typ == 1:
                seg.update(x, y)
            else:
                output.append(str(seg.query(x, y)))

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```Ba mảng chính trong cây có vai trò khác nhau.`tree[node]`lưu trữ tổng số hoa hiện tại cho khoảng thời gian của nút.`lazy_a[node]`Và`lazy_b[node]`lưu trữ các hệ số trì hoãn của hàm (ai+b). 

các`_apply`phương pháp là tính toán trung tâm. Đối với một nút bao phủ ([L,R]), có các vị trí (R-L+1) và chỉ số của chúng có tổng bằng ((L+R)(R-L+1)/2). Do đó việc thêm (ai+b) sẽ thay đổi tổng nút bằng`a * index_sum + b * length`. 

Đối với hoạt động loại (1), bản cập nhật luôn có độ dốc (1) và phần chặn (1-l). Điểm cuối bên phải (r) chỉ xác định vị trí nào được bao phủ. Nó không xuất hiện trong chính chức năng đó. 

Các hệ số lười được thêm vào thay vì thay thế. Nếu một nút nhận trước (2i+3) và sau đó nhận (i-4), thì thao tác chờ kết hợp là (3i-1). Đây là lý do tại sao`_apply`thực hiện`+=`trên cả hai mảng lười biếng. 

Truy vấn đẩy các bản cập nhật đang chờ xử lý trước khi giảm dần. Nếu không có bước đó, phần tử con vẫn có thể chứa giá trị cũ mặc dù giá trị gốc của nó đã bao gồm giá trị cập nhật bị trì hoãn trong tổng của nó. 

Số nguyên Python tự động tăng vượt quá 64 bit, do đó không có vấn đề tràn. Các câu trả lời tối đa đủ lớn nên số học 32 bit có chiều rộng cố định sẽ không an toàn. 

Tất cả các vị trí trong quá trình thực hiện đều dựa trên một, khớp với các công thức toán học. Điều này làm cho biểu thức (i-l+1) có thể sử dụng trực tiếp và tránh được sự chuyển đổi bổ sung trong mỗi lần cập nhật. 

Độ sâu đệ quy chỉ là (O(\log N)), nhưng giới hạn đệ quy vẫn được nâng lên. Cây phân đoạn chứa (O(N)) nút, nằm trong giới hạn bộ nhớ. 

## Ví dụ đã hoạt động 

### Mẫu 1, test case đầu tiên 

Mảng ban đầu là ([2,1,3,5,2]). Bảng sau ghi lại mảng sau mỗi lần cập nhật và câu trả lời mỗi khi truy vấn xuất hiện. 

| Hoạt động | Đã thêm cập nhật | Mảng sau thao tác | Câu trả lời truy vấn | 
| --- | --- | --- | --- | 
|`1 1 3`| ([1,2,3,0,0]) | ([3,3,6,5,2]) | | 
|`2 3 5`| không | ([3,3,6,5,2]) | (6+5+2=13) | 
|`1 4 5`| ([0,0,0,1,2]) | ([3,3,6,6,4]) | | 
|`1 2 5`| ([0,1,2,3,4]) | ([3,4,8,9,8]) | | 
|`1 1 1`| ([1,0,0,0,0]) | ([4,4,8,9,8]) | | 
|`2 1 4`| không | ([4,4,8,9,8]) | (4+4+8+9=25) | 

Ví dụ, bản cập nhật`[2,5]`được biểu diễn dưới dạng (i-1). Trên một đoạn bao gồm các vị trí (2) đến (5), đóng góp của nó là 

[ 
(2-1)+(3-1)+(4-1)+(5-1)=1+2+3+4=10. 
] 

Cây không cần phải đến thăm bốn vị trí đó một cách riêng lẻ. 

### Mẫu 1, test case thứ hai 

Mảng ban đầu thứ hai là ([10,5,2,0,8,6,2]). 

| Hoạt động | Đã thêm cập nhật | Mảng sau thao tác | Câu trả lời truy vấn | 
| --- | --- | --- | --- | 
|`1 2 5`| ([0,1,2,3,4,0,0]) | ([10,6,4,3,12,6,2]) | | 
|`1 1 6`| ([1,2,3,4,5,6,0]) | ([11,8,7,7,17,12,2]) | | 
|`2 4 7`| không | ([11,8,7,7,17,12,2]) | (7+17+12+2=38) | 
|`1 1 3`| ([1,2,3,0,0,0,0]) | ([12,10,10,7,17,12,2]) | | 
|`1 5 5`| ([0,0,0,0,1,0,0]) | ([12,10,10,7,18,12,2]) | | 
|`1 1 5`| ([1,2,3,4,5,0,0]) | ([13,12,13,11,23,12,2]) | | 
|`2 1 7`| không | ([13,12,13,11,23,12,2]) | (86) | 

Dấu vết thứ hai thực hiện các cập nhật chồng chéo. Cây kết hợp các hàm tuyến tính lười biếng của chúng bằng phép cộng, đó chính xác là những gì hoạt động mảng yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N+Q\log N)) | Việc xây dựng cây mất (O(N)), trong khi mỗi lần cập nhật và truy vấn đều truy cập vào cấp độ cây (O(\log N)). | 
| Không gian | (O(N)) | Ba mảng cây phân đoạn, mỗi mảng chứa (O(N)) mục. | 

Với (N,Q\le 10^5), số hạng vượt trội xấp xỉ (10^5\log_2(10^5)), tức là khoảng (1,7) triệu cấp cây cho mỗi trường hợp thử nghiệm. Ngay cả với một số phép toán số học tại mỗi nút được truy cập, con số này vẫn thấp hơn nhiều so với (10^{10}) phép toán mà giải pháp trực tiếp yêu cầu. Việc sử dụng bộ nhớ là tuyến tính và vừa vặn thoải mái trong 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

sys.setrecursionlimit(1_000_000)

class SegmentTree:
    def __init__(self, arr):
        self.n = len(arr)
        size = 4 * self.n + 5
        self.tree = [0] * size
        self.lazy_a = [0] * size
        self.lazy_b = [0] * size
        self.arr = arr
        self._build(1, 1, self.n)

    def _build(self, node, left, right):
        if left == right:
            self.tree[node] = self.arr[left - 1]
            return

        mid = (left + right) // 2
        self._build(node * 2, left, mid)
        self._build(node * 2 + 1, mid + 1, right)
        self.tree[node] = self.tree[node * 2] + self.tree[node * 2 + 1]

    @staticmethod
    def _index_sum(left, right):
        length = right - left + 1
        return (left + right) * length // 2

    def _apply(self, node, left, right, a, b):
        length = right - left + 1
        index_sum = self._index_sum(left, right)
        self.tree[node] += a * index_sum + b * length
        self.lazy_a[node] += a
        self.lazy_b[node] += b

    def _push(self, node, left, right):
        a = self.lazy_a[node]
        b = self.lazy_b[node]

        if a == 0 and b == 0:
            return

        if left != right:
            mid = (left + right) // 2
            self._apply(node * 2, left, mid, a, b)
            self._apply(node * 2 + 1, mid + 1, right, a, b)

        self.lazy_a[node] = 0
        self.lazy_b[node] = 0

    def update(self, ql, qr):
        self._update(1, 1, self.n, ql, qr)

    def _update(self, node, left, right, ql, qr):
        if qr < left or right < ql:
            return

        if ql <= left and right <= qr:
            self._apply(node, left, right, 1, 1 - ql)
            return

        self._push(node, left, right)

        mid = (left + right) // 2
        self._update(node * 2, left, mid, ql, qr)
        self._update(node * 2 + 1, mid + 1, right, ql, qr)

        self.tree[node] = self.tree[node * 2] + self.tree[node * 2 + 1]

    def query(self, ql, qr):
        return self._query(1, 1, self.n, ql, qr)

    def _query(self, node, left, right, ql, qr):
        if qr < left or right < ql:
            return 0

        if ql <= left and right <= qr:
            return self.tree[node]

        self._push(node, left, right)

        mid = (left + right) // 2
        return (
            self._query(node * 2, left, mid, ql, qr)
            + self._query(node * 2 + 1, mid + 1, right, ql, qr)
        )

def solve():
    input = sys.stdin.readline
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        q = int(input())

        seg = SegmentTree(arr)

        for _ in range(q):
            typ, x, y = map(int, input().split())
            if typ == 1:
                seg.update(x, y)
            else:
                ans.append(str(seg.query(x, y)))

    return "\n".join(ans)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()

        result = solve()
        sys.stdout.write(result)

        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

sample = """\
2
5
2 1 3 5 2
6
1 1 3
2 3 5
1 4 5
1 2 5
1 1 1
2 1 4
7
10 5 2 0 8 6 2
7
1 2 5
1 1 6
2 4 7
1 1 3
1 5 5
1 1 5
2 1 7
"""

assert run(sample) == "13\n25\n38\n86", "provided sample"

assert run("""\
1
1
5
2
1 1 1
2 1 1
""") == "6", "minimum size"

assert run("""\
1
5
0 0 0 0 0
3
1 2 5
2 1 5
2 5 5
""") == "10\n4", "right boundary"

assert run("""\
1
5
0 0 0 0 0
4
1 3 5
2 1 5
2 3 5
2 4 4
""") == "6\n6\n2", "left boundary"

assert run("""\
1
4
0 0 0 0
3
1 1 3
1 2 4
2 1 4
""") == "12", "overlapping updates"

# Maximum-size test. Every initial value is equal and the update covers N.
n = 100000
maximum_test = (
    "1\n"
    + str(n) + "\n"
    + ("1 " * n).strip() + "\n"
    + "3\n"
    + f"1 1 {n}\n"
    + f"2 1 {n}\n"
    + f"2 {n} {n}\n"
)

expected_total = n + n * (n + 1) // 2
expected_last = 2

assert run(maximum_test) == f"{expected_total}\n{expected_last}", \
    "maximum size and all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`N=1`, một cập nhật và một truy vấn |`6`| Kích thước tối thiểu và số hạng đầu tiên của tiến trình | 
| Không có mảng, cập nhật`[2,5]`|`10`,`4`| Điểm cuối bên phải và giá trị tiến trình chính xác | 
| Không có mảng, cập nhật`[3,5]`|`6`,`6`,`2`| Truy vấn điểm cuối và phạm vi con bên trái khác 0 | 
| Hai bản cập nhật chồng chéo |`12`| Thành phần phụ gia của bản cập nhật | 
| (N=100000), tất cả các giá trị đều bằng nhau |`5000150000`,`2`| Kích thước tối đa, số tiền lớn, cập nhật toàn diện | 

## Vỏ cạnh 

Đối với trường hợp một phần tử```
1
1
5
2
1 1 1
2 1 1
```bản cập nhật có (a=1) và (b=0), bởi vì (1-l=0). Cây phân đoạn chỉ chứa gốc, do đó trường hợp che phủ hoàn chỉnh ngay lập tức thêm (1). Tổng của nó thay đổi từ (5) thành (6) và truy vấn trả về (6). Không có con nào để đẩy nên tình trạng lá được xử lý một cách tự nhiên. 

Đối với bản cập nhật kết thúc ở vị trí cuối cùng,```
1
5
0 0 0 0 0
2
1 2 5
2 1 5
```hàm lười biếng là (i-1). Nút gốc chỉ được che phủ một phần, do đó bản cập nhật sẽ giảm dần cho đến khi tìm thấy các nút được che phủ. Sự đóng góp từ các vị trí (2) đến (5) là 

[ 
1+2+3+4=10. 
] 

Truy vấn cuối cùng trả về (10). Điểm cuối bên phải chỉ được xử lý bởi các ranh giới khoảng, do đó không có trường hợp đặc biệt nào cho (r=N). 

Đối với điểm cuối bên trái không phải là một,```
1
5
0 0 0 0 0
4
1 3 5
2 1 5
2 3 5
2 4 4
```chức năng cập nhật là 

[ 
i-3+1=i-2. 
] 

Như vậy vị trí (3,4,5) nhận (1,2,3), cho tổng (6). Truy vấn`[3,5]`trả về (6), trong khi`[4,4]`trả về (2). Phần chặn (1-l) là phần làm cho công thức phụ thuộc vào vị trí bắt đầu thực tế. 

Đối với các bản cập nhật chồng chéo,```
1
4
0 0 0 0
3
1 1 3
1 2 4
2 1 4
```bản cập nhật đầu tiên là (i) bật`[1,3]`, sản xuất`[1,2,3,0]`. Thứ hai là (i-1) trên`[2,4]`, tạo ra thêm`[0,1,2,3]`. Mảng cuối cùng là`[1,3,5,3]`, có tổng là (12). Trong cây, các thẻ lười chồng chéo được thêm hệ số theo hệ số, do đó cấu trúc dữ liệu thể hiện chính xác hiệu ứng kết hợp của cả hai thao tác. 

Kiểm tra kích thước tối đa cũng kiểm tra độ an toàn số học. Với (N=100000), một mảng tất cả một, theo sau là một bản cập nhật`[1,N]`sản xuất tổng cộng 

5000150000. 
] 

Điều này vượt quá phạm vi 32 bit đã ký, nhưng số nguyên Python thể hiện chính xác nó. Do đó, cây phân đoạn trả về giá trị đúng mà không cần xử lý tràn đặc biệt nào.
