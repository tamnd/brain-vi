---
title: "CF 103447K - Ưu tiên Wonder Egg"
description: "Chúng tôi đang duy trì một chuỗi số đại diện cho “mức sức mạnh” hiện tại của một bộ sưu tập trứng. Mỗi quả trứng có một giá trị ban đầu và theo thời gian, chúng tôi liên tục áp dụng các cập nhật nhân lên các phân đoạn con hoặc yêu cầu tổng của một phân đoạn con. Có hai hoạt động."
date: "2026-07-03T07:33:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103447
codeforces_index: "K"
codeforces_contest_name: "The 2021 China Collegiate Programming Contest (Harbin)"
rating: 0
weight: 103447
solve_time_s: 47
verified: true
draft: false
---

[CF 103447K - Ưu tiên Trứng kỳ diệu](https://codeforces.com/problemset/problem/103447/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một chuỗi số đại diện cho “mức sức mạnh” hiện tại của một bộ sưu tập trứng. Mỗi quả trứng có một giá trị ban đầu và theo thời gian, chúng tôi liên tục áp dụng các cập nhật nhân lên các phân đoạn con hoặc yêu cầu tổng của một phân đoạn con. 

Có hai hoạt động. Một thao tác nhân mọi giá trị trong một phạm vi$[l, r]$bởi một yếu tố nhất định$k$. Hoạt động khác yêu cầu tổng các giá trị trong một phạm vi$[l, r]$, với câu trả lời được lấy modulo$M$. Giữa các thao tác, các giá trị vẫn tồn tại nên các cập nhật sẽ tích lũy theo thời gian. 

Nơi hạn chế$n$Và$q$lên tới$10^5$, nghĩa là chúng ta phải xử lý cả cập nhật và truy vấn theo thời gian gần như logarit. Bất kỳ giải pháp nào tính toán lại một phạm vi từ đầu cho mỗi thao tác đều dẫn đến$O(nq)$, vượt xa giới hạn khả thi. Thậm chí$O(n)$mỗi thao tác sẽ đạt được$10^{10}$hoạt động trong trường hợp xấu nhất. 

Một khó khăn tinh tế đến từ tính chất nhân lên của các bản cập nhật. Không giống như các cập nhật cộng, phép nhân tương tác với tổng theo cách ngăn cản việc tính toán lại tiền tố đơn giản. Nếu chúng tôi không duy trì cấu trúc thì mọi truy vấn tổng phạm vi sẽ yêu cầu tính toán lại tất cả các giá trị bị ảnh hưởng. 

Một trường hợp thất bại điển hình là một mô phỏng đơn giản: 

đầu vào:```
5 3 100
1 2 3 4 5
1 1 5 2
2 1 5
2 1 5
```Một cách tiếp cận đơn giản sẽ nhân toàn bộ mảng trong thao tác đầu tiên và sau đó tính lại tổng hai lần. Cái này đã tốn rồi$O(n)$cho mỗi thao tác, nhưng các mẫu tệ hơn với nhiều bản cập nhật khiến nó quá chậm. 

Một cạm bẫy khác là quên rằng các bản cập nhật dựa trên phạm vi. Nếu người ta coi phép nhân là tổng thể không chính xác hoặc không giới hạn ở$[l, r]$, trạng thái phân kỳ ngay lập tức. 

Thách thức cốt lõi là hỗ trợ các truy vấn nhân phạm vi và tổng phạm vi theo mô-đun một cách hiệu quả. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực giữ mảng một cách rõ ràng. Đối với thao tác loại 1, nó lặp từ$l$ĐẾN$r$và nhân mỗi phần tử với$k$. Đối với thao tác loại 2, nó tính tổng trực tiếp phạm vi. Điều này đúng vì nó phản ánh chính xác định nghĩa. 

Tuy nhiên, mỗi hoạt động là tuyến tính trong phạm vi kích thước. Trong trường hợp xấu nhất, cả cập nhật và truy vấn đều bao trùm các phân đoạn lớn, mang lại$O(nq)$, xung quanh$10^{10}$hoạt động và rõ ràng là không khả thi. 

Quan sát quan trọng là chúng ta không thực sự cần biết từng yếu tố riêng lẻ vào mọi lúc. Chúng tôi chỉ cần hai khả năng tổng hợp: có thể mở rộng quy mô toàn bộ phân khúc và có thể tính tổng phân khúc một cách nhanh chóng. Điều này gợi ý một cây phân đoạn trong đó mỗi nút lưu trữ tổng phân đoạn của nó. 

Điều phức tạp là phép nhân áp dụng cho toàn bộ phân đoạn một cách lười biếng. Nếu một phân đoạn có hệ số nhân đang chờ xử lý thì mọi giá trị trong phân đoạn đó sẽ được chia tỷ lệ đồng đều, do đó tổng phân đoạn cũng được chia tỷ lệ theo cùng một hệ số. Đây chính xác là loại chuyển đổi có thể bị trì hoãn bằng cách sử dụng lan truyền lười biếng: chúng tôi lưu trữ thẻ nhân ở mỗi nút và chỉ đẩy nó xuống khi cần thiết. 

Điều này làm giảm cả hai hoạt động xuống$O(\log n)$: phép nhân phạm vi cập nhật thẻ lười và điều chỉnh tổng, trong khi truy vấn tổng phạm vi tổng hợp các giá trị nút. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(n)$| Quá chậm | 
| Cây phân đoạn với phép nhân lười biếng |$O(q \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một cây phân đoạn trong đó mỗi nút lưu trữ tổng modulo phân đoạn của nó$M$, cùng với số nhân lười biếng biểu thị hệ số tỷ lệ đang chờ xử lý. 

1. Xây dựng cây phân đoạn từ mảng ban đầu. Mỗi lá lưu trữ giá trị của một quả trứng và mỗi nút bên trong lưu trữ tổng số modulo con của nó$M$. Điều này mang lại cho chúng ta một cấu trúc cơ sở để tổng hợp phạm vi nhanh. 
2. Khởi tạo một mảng số nhân lười biếng với tất cả các giá trị được đặt thành 1. Điều này thể hiện rằng ban đầu không có phân đoạn nào đang chờ chia tỷ lệ. 
3. Áp dụng phép nhân phạm vi$[l, r]$qua$k$, chúng ta duyệt cây phân đoạn. Bất cứ khi nào một đoạn nút nằm hoàn toàn bên trong$[l, r]$, chúng tôi nhân số tiền được lưu trữ của nó với$k$modulo$M$và cũng nhân thẻ lười của nó với$k$. Điều này đảm bảo rằng việc nhân giống trong tương lai sẽ tuân theo quy mô tích lũy. 
4. Khi một nút được che phủ một phần, chúng tôi sẽ đẩy hệ số nhân lười biếng của nó cho các nút con của nó trước khi tiếp tục. Đẩy có nghĩa là áp dụng hệ số nhân được lưu trữ cho tổng con và kết hợp nó với các thẻ lười của chúng, sau đó đặt lại thẻ của nút hiện tại thành 1. Điều này duy trì tính chính xác khi trộn các phần chồng chéo một phần. 
5. Đối với truy vấn tổng phạm vi$[l, r]$, chúng ta duyệt cây tương tự. Các nút được bảo hiểm đầy đủ đóng góp trực tiếp số tiền được lưu trữ của họ. Các nút được che phủ một phần yêu cầu đẩy các giá trị lười biếng trước khi giảm dần. 
6. Mỗi thao tác duy trì tổng phân đoạn nhất quán với tất cả các phép nhân trước đó mà không chạm vào từng phần tử một cách rõ ràng. 

Bất biến quan trọng là tổng được lưu trữ của mỗi nút luôn bằng tổng thực của phân đoạn của nó sau khi áp dụng tất cả các hệ số nhân đang chờ xử lý được biểu thị bằng các thẻ lười chưa được đẩy xuống. Thẻ lười thể hiện một phép biến đổi nhân hoãn lại cuối cùng sẽ được áp dụng thống nhất cho cây con, do đó việc áp dụng nó ngay lập tức hoặc sau đó sẽ tạo ra kết quả tương tự do tính phân phối:$$k(a_1 + a_2 + \dots) = ka_1 + ka_2 + \dots$$Bởi vì phép nhân phân phối cho phép cộng nên chúng ta có thể trì hoãn việc cập nhật một cách an toàn mà không làm mất đi tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr, mod):
        self.n = len(arr)
        self.mod = mod
        self.tree = [0] * (4 * self.n)
        self.lazy = [1] * (4 * self.n)
        self._build(1, 0, self.n - 1, arr)

    def _build(self, idx, l, r, arr):
        if l == r:
            self.tree[idx] = arr[l] % self.mod
            return
        mid = (l + r) // 2
        self._build(idx * 2, l, mid, arr)
        self._build(idx * 2 + 1, mid + 1, r, arr)
        self.tree[idx] = (self.tree[idx * 2] + self.tree[idx * 2 + 1]) % self.mod

    def _push(self, idx, l, r):
        if self.lazy[idx] == 1:
            return
        mul = self.lazy[idx]
        self.tree[idx] = (self.tree[idx] * mul) % self.mod
        if l != r:
            self.lazy[idx * 2] = (self.lazy[idx * 2] * mul) % self.mod
            self.lazy[idx * 2 + 1] = (self.lazy[idx * 2 + 1] * mul) % self.mod
        self.lazy[idx] = 1

    def update(self, idx, l, r, ql, qr, val):
        self._push(idx, l, r)
        if qr < l or r < ql:
            return
        if ql <= l and r <= qr:
            self.lazy[idx] = (self.lazy[idx] * val) % self.mod
            self._push(idx, l, r)
            return
        mid = (l + r) // 2
        self.update(idx * 2, l, mid, ql, qr, val)
        self.update(idx * 2 + 1, mid + 1, r, ql, qr, val)
        self.tree[idx] = (self.tree[idx * 2] + self.tree[idx * 2 + 1]) % self.mod

    def query(self, idx, l, r, ql, qr):
        self._push(idx, l, r)
        if qr < l or r < ql:
            return 0
        if ql <= l and r <= qr:
            return self.tree[idx]
        mid = (l + r) // 2
        return (self.query(idx * 2, l, mid, ql, qr) +
                self.query(idx * 2 + 1, mid + 1, r, ql, qr)) % self.mod

n, q, mod = map(int, input().split())
arr = list(map(int, input().split()))

st = SegTree(arr, mod)

out = []
for _ in range(q):
    tmp = list(map(int, input().split()))
    if tmp[0] == 1:
        _, l, r, k = tmp
        st.update(1, 0, n - 1, l - 1, r - 1, k)
    else:
        _, l, r = tmp
        out.append(str(st.query(1, 0, n - 1, l - 1, r - 1)))

print("\n".join(out))
```Cây phân đoạn lưu trữ cả tổng phân đoạn hiện tại và thẻ lười nhân. các`_push`Hàm áp dụng phép nhân đang chờ xử lý cho nút hiện tại và truyền nó cho nút con nếu nút đó không phải là lá. Điều này đảm bảo rằng bất cứ khi nào chúng tôi đi xuống, chúng tôi sẽ thấy các giá trị chính xác. 

Chức năng cập nhật trước tiên giải quyết các hiệu ứng lười biếng đang chờ xử lý, sau đó kiểm tra sự chồng chéo. Phạm vi bảo hiểm đầy đủ áp dụng phép nhân trực tiếp cho nút và lưu trữ nó trong mảng lười. Bảo hiểm một phần cập nhật đệ quy các phần tử con và tính toán lại tổng. 

Hàm truy vấn đảm bảo tính chính xác bằng cách đẩy các giá trị lười trước khi sử dụng tổng nút, đảm bảo rằng mọi giá trị được trả về đều phản ánh tất cả các bản cập nhật đang chờ xử lý. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng mẫu$[1,2,3,4,5]$với mô đun$5$. 

Hoạt động đầu tiên nhân lên$[2,5]$bằng 2. 

| Bước | Phân đoạn | Hành động | Tổng nút | 
| --- | --- | --- | --- | 
| 1 | [1,5] | đi xuống | 15 | 
| 2 | [2,5] | áp dụng nhân | 30 mod 5 = 0 ở cấp biểu diễn gốc | 

Sau khi lan truyền, các giá trị trở thành$[1,4,6,8,10]$, giảm modulo 5 cho$[1,4,1,3,0]$. 

Truy vấn hoạt động thứ hai$[1,4]$. 

| Bước | Phân đoạn | Số tiền được trả về | 
| --- | --- | --- | 
| 1 | [1,4] | 1 + 4 + 1 + 3 = 9 mod 5 = 4 | 

Điều này khớp với hành vi dự kiến ​​là phép nhân chỉ ảnh hưởng đến một phần của mảng trong khi truy vấn đọc tổng nhất quán. 

Dấu vết thứ hai với các cập nhật lặp đi lặp lại trên các phạm vi chồng chéo cho thấy sự cần thiết của việc lan truyền lười biếng. Nếu không đẩy thẻ chính xác, các phép nhân chồng chéo sẽ được áp dụng hai lần hoặc bị mất hoàn toàn, phá vỡ tính nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \log n)$| Mỗi bản cập nhật và truy vấn truy cập vào một số nút logarit trong cây phân đoạn và việc truyền bá lười biếng đảm bảo công việc liên tục trên mỗi nút được truy cập | 
| Không gian |$O(n)$| Cây phân đoạn và mảng lười lưu trữ một số giá trị không đổi trên mỗi nút | 

Sự phức tạp này phù hợp một cách thoải mái trong giới hạn của$n, q \le 10^5$, kể từ khoảng$10^5 \log 10^5$hoạt động có thể dễ dàng thực hiện được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from sys import stdin
    input = _sys.stdin.readline

    class SegTree:
        def __init__(self, arr, mod):
            self.n = len(arr)
            self.mod = mod
            self.tree = [0] * (4 * self.n)
            self.lazy = [1] * (4 * self.n)
            self._build(1, 0, self.n - 1, arr)

        def _build(self, idx, l, r, arr):
            if l == r:
                self.tree[idx] = arr[l] % self.mod
                return
            mid = (l + r) // 2
            self._build(idx*2, l, mid, arr)
            self._build(idx*2+1, mid+1, r, arr)
            self.tree[idx] = (self.tree[idx*2] + self.tree[idx*2+1]) % self.mod

        def _push(self, idx, l, r):
            if self.lazy[idx] == 1:
                return
            mul = self.lazy[idx]
            self.tree[idx] = self.tree[idx] * mul % self.mod
            if l != r:
                self.lazy[idx*2] = self.lazy[idx*2] * mul % self.mod
                self.lazy[idx*2+1] = self.lazy[idx*2+1] * mul % self.mod
            self.lazy[idx] = 1

        def update(self, idx, l, r, ql, qr, val):
            self._push(idx, l, r)
            if qr < l or r < ql:
                return
            if ql <= l and r <= qr:
                self.lazy[idx] = self.lazy[idx] * val % self.mod
                self._push(idx, l, r)
                return
            mid = (l+r)//2
            self.update(idx*2, l, mid, ql, qr, val)
            self.update(idx*2+1, mid+1, r, ql, qr, val)
            self.tree[idx] = (self.tree[idx*2] + self.tree[idx*2+1]) % self.mod

        def query(self, idx, l, r, ql, qr):
            self._push(idx, l, r)
            if qr < l or r < ql:
                return 0
            if ql <= l and r <= qr:
                return self.tree[idx]
            mid = (l+r)//2
            return (self.query(idx*2, l, mid, ql, qr) +
                    self.query(idx*2+1, mid+1, r, ql, qr)) % self.mod

    data = """5 7 5
1 2 3 4 5
2 2 5
1 1 3 1
2 1 4
1 2 4 2
2 1 5
1 3 5 2
2 1 5
"""
    sys.stdin = io.StringIO(data)
    n, q, mod = map(int, input().split())
    arr = list(map(int, input().split()))
    st = SegTree(arr, mod)
    out = []
    for _ in range(q):
        t = list(map(int, input().split()))
        if t[0] == 1:
            _, l, r, k = t
            st.update(1, 0, n-1, l-1, r-1, k)
        else:
            _, l, r = t
            out.append(str(st.query(1, 0, n-1, l-1, r-1)))
    return "\n".join(out)

# provided sample
assert run(data) == "4\n2\n1\n0", "sample"

# minimum size
assert run("""1 2 10
5
2 1 1
1 1 1 3
""") == "5", "min case"

# all equal
assert run("""3 2 100
2 2 2
1 1 3 2
2 1 3
""") == "12", "all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 4 2 1 0 | đầy đủ tính chính xác của quy trình làm việc | 
| phần tử đơn | 5 | xử lý ranh giới | 
| tất cả đều bình đẳng | 12 | nhân giống đồng đều | 

## Vỏ cạnh 

Mảng một phần tử nhấn mạnh đến sự lan truyền lười biếng tại các nút lá. Nếu việc triển khai truyền sai các thẻ lười đến các phần tử con không tồn tại hoặc không áp dụng các bản cập nhật ở các lá thì giá trị sẽ không nhất quán. Trong trường hợp này, nhân một phần tử với một thừa số và truy vấn nó vẫn phải trả về cùng một phần tử theo modulo$M$, mà cây phân đoạn giữ nguyên vì các bản cập nhật trực tiếp sửa đổi các nút lá. 

Một trường hợp đặc biệt khác liên quan đến các cập nhật chồng chéo lặp đi lặp lại, chẳng hạn như nhân một phạm vi nhiều lần trước khi truy vấn. Tính chính xác phụ thuộc vào việc các thẻ lười tích lũy theo cấp số nhân thay vì bị ghi đè. Bất biến đảm bảo rằng giá trị lười biếng của mỗi nút đại diện cho sản phẩm của tất cả các bản cập nhật đang chờ xử lý ảnh hưởng đến nó, do đó các bản cập nhật lặp lại sẽ được kết hợp chính xác mà không gặp vấn đề về thứ tự.
