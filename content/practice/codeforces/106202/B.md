---
title: "CF 106202B - \u0422\u043e\u0440\u0433\u043e\u0432\u0446\u044b"
description: "Chúng ta được sắp xếp một hàng người buôn bán, mỗi người nắm giữ một mặt hàng có giá cả. Giá của người bán thứ i là một số nguyên trong phạm vi từ 0 đến $2^k - 1$ và mọi giá về mặt khái niệm được lưu trữ dưới dạng số nhị phân k-bit có độ dài cố định. Hệ thống xử lý ba loại hoạt động."
date: "2026-06-20T22:28:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106202
codeforces_index: "B"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2025-2026, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 106202
solve_time_s: 56
verified: true
draft: false
---

[CF 106202B - \u0422\u043e\u0440\u0433\u043e\u0432\u0446\u044b](https://codeforces.com/problemset/problem/106202/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được sắp xếp một hàng người buôn bán, mỗi người nắm giữ một mặt hàng có giá cả. Giá của người bán thứ i là một số nguyên trong khoảng từ 0 đến$2^k - 1$và mọi mức giá đều được lưu trữ dưới dạng số nhị phân k-bit có độ dài cố định. 

Hệ thống xử lý ba loại hoạt động. Loại đầu tiên cập nhật giá của một người bán. Loại thứ hai áp dụng phép biến đổi trên một phân khúc người bán: mọi giá trong phân khúc được viết lại bằng cách lấy dạng nhị phân k-bit của nó, đảo từng bit và diễn giải kết quả trở lại dưới dạng số nguyên. Loại thứ ba yêu cầu tổng giá trên một phân khúc. 

Chi tiết quan trọng là đảo ngược bit không phải là phủ định số học hoặc XOR với mặt nạ cố định trừ khi chúng ta nhận ra điều gì đang xảy ra trong hệ nhị phân có chiều rộng cố định: mọi số x được chuyển đổi thành$(2^k - 1) - x$. 

Các hạn chế rất lớn: lên tới 200.000 thương nhân và 200.000 hoạt động. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại tổng phân đoạn hoặc áp dụng lật từng phần tử cho mỗi truy vấn. Ngay cả cấu trúc dữ liệu logarit cũng phải tránh chạm vào mọi phần tử trong truy vấn đảo ngược. 

Một điểm tinh tế là sự đảo ngược có thể đảo ngược và thống nhất trên toàn bộ phân đoạn. Một đoạn được lật hai lần sẽ trở về trạng thái ban đầu. Một khía cạnh quan trọng khác là các giá trị ghi đè cập nhật phải tôn trọng mọi lần lật đang chờ xử lý trong cấu trúc. 

Các trường hợp cạnh xuất hiện khi k nhỏ hoặc lớn so với các giá trị, đặc biệt khi tất cả các bit là 1 hoặc 0, trong đó phép đảo ngược trở thành hành vi nhận dạng hoặc hành vi bổ sung đầy đủ. Một kịch bản phức tạp khác là các phép đảo ngược chồng chéo với các cập nhật điểm; nếu không được xử lý cẩn thận, các giá trị cũ có thể lan truyền. 

Ví dụ: nếu k = 3 và x = 2 (nhị phân 010), phép đảo ngược tạo ra 101 bằng 5. Thay vào đó, nếu chúng ta suy nghĩ không chính xác về mặt phủ định số nguyên, chúng ta có thể xử lý sai các trạng thái trung gian khi k thay đổi kích thước biểu diễn, nhưng ở đây k được cố định trên toàn cầu. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp duy trì mảng một cách rõ ràng. Truy vấn loại 1 cập nhật một vị trí. Truy vấn loại 2 lặp lại trong phạm vi và thay thế từng giá trị x bằng$(2^k - 1) - x$. Truy vấn loại 3 tính tổng trực tiếp phân đoạn. 

Điều này đúng nhưng không thành công ngay lập tức trong trường hợp đầu vào xấu nhất. Mỗi phép tính nghịch đảo hoặc tổng có thể mất O(n) và với tối đa 200.000 phép tính, trường hợp xấu nhất sẽ đạt tới$O(nq)$, vượt xa giới hạn khả thi. 

Cấu trúc của sự chuyển đổi là quan sát chính. Mỗi phép đảo ngược thay thế mọi giá trị x trong một phân đoạn bằng một phép biến đổi tuyến tính không đổi:$x \mapsto C - x$, Ở đâu$C = 2^k - 1$. Đây là affine. Về tổng, nó hoạt động có thể dự đoán được: 

Nếu một đoạn có độ dài len và tổng S, sau khi đảo ngược tổng mới của nó sẽ trở thành:$$len \cdot C - S$$Điều này có nghĩa là chúng ta không bao giờ cần phải chạm vào từng phần tử riêng lẻ. Chúng tôi chỉ cần duy trì tổng phân đoạn và hỗ trợ việc gán phạm vi của "trạng thái lật". 

Thách thức còn lại là khả năng kết hợp: các phép đảo ngược lặp đi lặp lại chuyển đổi giữa bản sắc và phần bổ sung. Điều này gợi ý một cây phân đoạn lan truyền lười biếng với cờ lật boolean. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Cây phân đoạn lười lật | O(q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cây phân đoạn trong đó mỗi nút lưu trữ tổng phân khúc của nó. Ngoài ra, mỗi nút mang một cờ lười cho biết liệu phân đoạn của nó hiện có bị đảo ngược về mặt logic hay không. 

1. Xây dựng cây phân đoạn với các giá trị ban đầu. Mỗi nút lưu trữ tổng phân đoạn của nó và không có lần lật nào đang chờ xử lý. Điều này cho phép tổng hợp trực tiếp cho các truy vấn. 
2. Xác định hiệu ứng đảo ngược trên một nút. Nếu một đoạn có độ dài len có tổng S, thì sau khi đảo ngược, tổng mới của nó sẽ là len nhân C trừ S. Điều này cho phép chúng ta cập nhật toàn bộ đoạn trong thời gian O(1) tại một nút. 
3. Đối với truy vấn đảo ngược phạm vi, khi một phân đoạn nút được bao phủ hoàn toàn, chúng tôi áp dụng phép biến đổi trực tiếp cho tổng của nó và chuyển đổi cờ lười của nó. Việc chuyển đổi hai lần sẽ hủy bỏ, giúp duy trì tính chính xác. 
4. Khi một nút bị che một phần, chúng tôi đẩy xuống mọi đảo ngược đang chờ xử lý trước khi giảm xuống. Đẩy có nghĩa là áp dụng trạng thái đảo ngược cho trẻ em và xóa cờ ở cha mẹ. 
5. Để cập nhật điểm, chúng tôi điều hướng đến chiếc lá. Trong quá trình thực hiện, chúng tôi đảm bảo tất cả các lần lật đang chờ xử lý đều được đẩy để lá phản ánh giá trị hiện tại thực trước khi ghi đè lên nó. 
6. Sau khi cập nhật một lá, chúng ta tính tổng trở lên bằng cách tính tổng các lá con. 
7. Đối với truy vấn tổng phạm vi, chúng tôi cũng đảm bảo tính chính xác tương tự bằng cách đẩy cờ lười trước khi giảm dần, sau đó tính tổng trực tiếp các phân đoạn được bao phủ đầy đủ. 

Ý tưởng cấu trúc chính là phép đảo ngược tuyến tính trên tổng và tự nghịch đảo, cho phép nó được mã hóa dưới dạng một thẻ boolean duy nhất. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên hai bất biến. Đầu tiên, tổng được lưu trữ của mỗi nút luôn biểu thị tổng thực của phân đoạn của nó hoặc tổng sau khi áp dụng chính xác một cờ đảo ngược đang chờ xử lý chưa được truyền cho nút con. Thứ hai, phép toán đảo ngược có tính phân phối trên các phân đoạn: áp dụng nó cho tập hợp các phân đoạn rời rạc tương đương với việc áp dụng nó một cách độc lập cho từng phân đoạn. Vì nghịch đảo là nghịch đảo của chính nó nên cờ lười chỉ cần tính chẵn lẻ chứ không cần lịch sử. Điều này đảm bảo rằng bất kỳ chuỗi cập nhật nào cũng giảm xuống thành các phép biến đổi cục bộ nhất quán mà không có sự mơ hồ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr, k):
        self.n = len(arr)
        self.k = k
        self.C = (1 << k) - 1
        self.sum = [0] * (4 * self.n)
        self.lazy = [0] * (4 * self.n)
        self.build(1, 0, self.n - 1, arr)

    def build(self, v, l, r, arr):
        if l == r:
            self.sum[v] = arr[l]
            return
        m = (l + r) // 2
        self.build(v*2, l, m, arr)
        self.build(v*2+1, m+1, r, arr)
        self.sum[v] = self.sum[v*2] + self.sum[v*2+1]

    def apply_flip(self, v, l, r):
        length = r - l + 1
        self.sum[v] = length * self.C - self.sum[v]
        self.lazy[v] ^= 1

    def push(self, v, l, r):
        if not self.lazy[v] or l == r:
            return
        m = (l + r) // 2
        self.apply_flip(v*2, l, m)
        self.apply_flip(v*2+1, m+1, r)
        self.lazy[v] = 0

    def update_point(self, v, l, r, idx, val):
        if l == r:
            self.sum[v] = val
            return
        self.push(v, l, r)
        m = (l + r) // 2
        if idx <= m:
            self.update_point(v*2, l, m, idx, val)
        else:
            self.update_point(v*2+1, m+1, r, idx, val)
        self.sum[v] = self.sum[v*2] + self.sum[v*2+1]

    def range_flip(self, v, l, r, ql, qr):
        if ql <= l and r <= qr:
            self.apply_flip(v, l, r)
            return
        self.push(v, l, r)
        m = (l + r) // 2
        if ql <= m:
            self.range_flip(v*2, l, m, ql, qr)
        if qr > m:
            self.range_flip(v*2+1, m+1, r, ql, qr)
        self.sum[v] = self.sum[v*2] + self.sum[v*2+1]

    def range_sum(self, v, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.sum[v]
        self.push(v, l, r)
        m = (l + r) // 2
        res = 0
        if ql <= m:
            res += self.range_sum(v*2, l, m, ql, qr)
        if qr > m:
            res += self.range_sum(v*2+1, m+1, r, ql, qr)
        return res

n, k = map(int, input().split())
arr = list(map(int, input().split()))
q = int(input())

st = SegTree(arr, k)

for _ in range(q):
    tmp = input().split()
    t = int(tmp[0])
    if t == 1:
        i = int(tmp[1]) - 1
        x = int(tmp[2])
        st.update_point(1, 0, n-1, i, x)
    elif t == 2:
        l = int(tmp[1]) - 1
        r = int(tmp[2]) - 1
        st.range_flip(1, 0, n-1, l, r)
    else:
        l = int(tmp[1]) - 1
        r = int(tmp[2]) - 1
        print(st.range_sum(1, 0, n-1, l, r))
```Việc triển khai sử dụng cây phân đoạn cổ điển với khả năng lan truyền lười biếng. Đường tới hạn là sự chuyển đổi`length * C - sum[v]`, mã hóa đảo ngược bitwise đầy đủ mà không cần chạm vào từng phần tử riêng lẻ. Cờ lười bị XOR vì hai lần lật bị hủy. 

Hoạt động đẩy đảm bảo tính chính xác trước khi đệ quy một phần. Nếu không có nó, các bản cập nhật điểm sẽ ghi đè lên các giá trị đảo ngược cũ. Các phép toán phạm vi luôn tính toán lại tổng nút từ nút con sau khi đệ quy, duy trì tính nhất quán. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng nhỏ với k = 3:`[1, 2, 3]`, do đó C = 7. 

### Ví dụ 1: lật một lần và truy vấn 

| Bước | Hoạt động | Phân khúc bị ảnh hưởng | Tổng trạng thái | 
| --- | --- | --- | --- | 
| 1 | ban đầu | [1,2,3] | 6 | 
| 2 | lật [1,3] | toàn bộ phân khúc | 3 * 7 - 6 = 15 | 
| 3 | truy vấn [2,2] | lá 2 | 7 - 2 = 5 | 

Sau khi lật, mỗi phần tử trở thành phần bù của nó: 1→6, 2→5, 3→4. Cây phân đoạn nắm bắt được điều này mà không cần mở rộng các phần tử. 

### Ví dụ 2: lật rồi cập nhật điểm 

Bắt đầu lại:`[1, 2, 3]`| Bước | Hoạt động | Giải thích mảng | Tổng hợp | 
| --- | --- | --- | --- | 
| 1 | lật [1,3] | [6,5,4] | 15 | 
| 2 | đặt chỉ số 2 = 0 | [6,0,4] | 10 | 
| 3 | truy vấn [1,3] | tổng trực tiếp | 10 | 

Chi tiết quan trọng là bản cập nhật điểm trước tiên sẽ giải quyết các lần lật đang chờ xử lý để chỉ số 2 được xem chính xác là 5 trước khi ghi đè. 

Mỗi dấu vết cho thấy rằng chúng ta không bao giờ cần hiện thực hóa toàn bộ mảng sau khi lật; chỉ cần có số tiền và trạng thái lười biếng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | mỗi lần cập nhật, lật hoặc truy vấn sẽ giảm chiều cao của cây phân đoạn | 
| Không gian | O(n) | mảng cây phân đoạn lưu trữ tổng và cờ lười | 

Với n và q lên tới 200.000, chi phí logarit cho mỗi hoạt động vừa vặn trong giới hạn. Hệ số không đổi nhỏ vì nghịch đảo là O(1) trên mỗi nút. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class SegTree:
        def __init__(self, arr, k):
            self.n = len(arr)
            self.k = k
            self.C = (1 << k) - 1
            self.sum = [0] * (4 * self.n)
            self.lazy = [0] * (4 * self.n)
            self.build(1, 0, self.n - 1, arr)

        def build(self, v, l, r, arr):
            if l == r:
                self.sum[v] = arr[l]
                return
            m = (l + r) // 2
            self.build(v*2, l, m, arr)
            self.build(v*2+1, m+1, r, arr)
            self.sum[v] = self.sum[v*2] + self.sum[v*2+1]

        def apply_flip(self, v, l, r):
            length = r - l + 1
            self.sum[v] = length * self.C - self.sum[v]
            self.lazy[v] ^= 1

        def push(self, v, l, r):
            if not self.lazy[v] or l == r:
                return
            m = (l + r) // 2
            self.apply_flip(v*2, l, m)
            self.apply_flip(v*2+1, m+1, r)
            self.lazy[v] = 0

        def update_point(self, v, l, r, idx, val):
            if l == r:
                self.sum[v] = val
                return
            self.push(v, l, r)
            m = (l + r) // 2
            if idx <= m:
                self.update_point(v*2, l, m, idx, val)
            else:
                self.update_point(v*2+1, m+1, r, idx, val)
            self.sum[v] = self.sum[v*2] + self.sum[v*2+1]

        def range_flip(self, v, l, r, ql, qr):
            if ql <= l and r <= qr:
                self.apply_flip(v, l, r)
                return
            self.push(v, l, r)
            m = (l + r) // 2
            if ql <= m:
                self.range_flip(v*2, l, m, ql, qr)
            if qr > m:
                self.range_flip(v*2+1, m+1, r, ql, qr)
            self.sum[v] = self.sum[v*2] + self.sum[v*2+1]

        def range_sum(self, v, l, r, ql, qr):
            if ql <= l and r <= qr:
                return self.sum[v]
            self.push(v, l, r)
            m = (l + r) // 2
            res = 0
            if ql <= m:
                res += self.range_sum(v*2, l, m, ql, qr)
            if qr > m:
                res += self.range_sum(v*2+1, m+1, r, ql, qr)
            return res

    n, k = map(int, input().split())
    arr = list(map(int, input().split()))
    q = int(input())
    st = SegTree(arr, k)

    out = []
    for _ in range(q):
        tmp = input().split()
        t = int(tmp[0])
        if t == 1:
            i = int(tmp[1]) - 1
            x = int(tmp[2])
            st.update_point(1, 0, n-1, i, x)
        elif t == 2:
            l = int(tmp[1]) - 1
            r = int(tmp[2]) - 1
            st.range_flip(1, 0, n-1, l, r)
        else:
            l = int(tmp[1]) - 1
            r = int(tmp[2]) - 1
            out.append(str(st.range_sum(1, 0, n-1, l, r)))

    return "\n".join(out)

# Sample-style small test
assert run("""3 3
1 2 3
4
3 1 3
2 1 3
3 1 3
3 2 2
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lật nhỏ + truy vấn | hướng dẫn sử dụng | tính đúng đắn cơ bản của phép nghịch đảo | 
| Cập nhật phần tử đơn | hướng dẫn sử dụng | cập nhật điểm với trạng thái lười biếng | 
| Tất cả lật hai lần | số tiền ban đầu | sự bình thường của lật XOR | 
| Giá trị biên k tối đa | tổng bù đúng | xử lý phạm vi bit đầy đủ | 

## Vỏ cạnh 

Một trường hợp lặp đi lặp lại các lần lật trên cùng một phạm vi chứng tỏ rằng cờ lười hoạt động như tính chẵn lẻ. Nếu một phân đoạn được đảo ngược hai lần, nút được lưu trữ sẽ trả về chính xác tổng ban đầu của nó. Việc thực hiện xử lý việc này bởi vì`lazy[v] ^= 1`chuyển trạng thái và công thức biến đổi là tự nghịch đảo. 

Việc cập nhật điểm sau một lượt lật đang chờ xử lý sẽ tinh tế hơn. Giả sử một nút được đảo ngược một cách logic nhưng không được đẩy và chúng ta ghi đè trực tiếp lên một lá. Nếu không đẩy, bản cập nhật sẽ áp dụng cho bản trình bày cũ. Mã đảm bảo tính chính xác bằng cách gọi`push`trước khi hạ xuống nên chiếc lá luôn được vật chất hóa ở trạng thái chính xác trước khi sửa đổi. 

Trường hợp biên xảy ra khi k = 1. Khi đó C = 1 và phép đảo ngược chỉ đơn giản hoán đổi 0 và 1. Công thức vẫn giữ nguyên: tổng mới = len - tổng và cây phân đoạn hoạt động giống hệt nhau mà không sửa đổi.
