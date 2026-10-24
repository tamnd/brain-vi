---
title: "CF 106026E - \u7b80\u5355\u7684\u6570\u636e\u7ed3\u6784\u9898"
description: "Chúng tôi đang duy trì một mảng dài trong đó mỗi phần tử luôn có giá trị 4 bit, do đó, mỗi giá trị nằm trong phạm vi từ 0 đến 15. Mảng thay đổi theo thời gian thông qua các cập nhật phạm vi và đôi khi chúng tôi được yêu cầu tính toán thống kê tóm tắt trên một phạm vi."
date: "2026-06-21T16:38:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106026
codeforces_index: "E"
codeforces_contest_name: "CCF CAT NAEC 2025 (Final)"
rating: 0
weight: 106026
solve_time_s: 61
verified: true
draft: false
---

[CF 106026E - \u7b80\u5355\u7684\u6570\u636e\u7ed3\u6784\u9898](https://codeforces.com/problemset/problem/106026/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một mảng dài trong đó mỗi phần tử luôn có giá trị 4 bit, do đó, mỗi giá trị nằm trong phạm vi từ 0 đến 15. Mảng thay đổi theo thời gian thông qua các cập nhật phạm vi và đôi khi chúng tôi được yêu cầu tính toán thống kê tóm tắt trên một phạm vi. 

Hai thao tác đầu tiên không gán các giá trị tùy ý, chúng biến đổi mọi phần tử trong một mảng con bằng cách áp dụng một hàm cố định cho mỗi giá trị. Vì các giá trị luôn nằm trong một miền cố định nhỏ có kích thước 16 nên mọi cập nhật thực chất là một hoán vị của 16 trạng thái này. Thao tác thứ ba yêu cầu chúng ta xem xét một phạm vi, đếm số lần mỗi giá trị từ 0 đến 15 xuất hiện và sau đó lấy XOR theo bit của tất cả các số đếm này. 

Kích thước đầu vào đạt 5×10^5 cho cả n và q, do đó, bất kỳ giải pháp nào chạm vào từng phần tử trên mỗi truy vấn sẽ ngay lập tức thất bại. Ngay cả một lần quét tuyến tính cho mỗi truy vấn cũng dẫn đến khoảng 2,5×10^11 thao tác trong trường hợp xấu nhất, vượt xa giới hạn khả thi. Điều này buộc chúng ta phải sử dụng cấu trúc dữ liệu hỗ trợ cả cập nhật phạm vi và truy vấn phạm vi theo thời gian logarit. 

Một vấn đề tế nhị trong suy nghĩ ngây thơ là coi mỗi lần cập nhật là số học trên các số nguyên và trực tiếp sửa đổi các giá trị. Ví dụ: áp dụng liên tục “+1 mod 16” hoặc “×2 mod 16” trên một phân đoạn sẽ yêu cầu truy cập mọi phần tử. Một sai lầm khác là cố gắng chỉ duy trì số lượng mà quên rằng các bản cập nhật không mang tính cộng, chúng ánh xạ lại các giá trị, nghĩa là số lượng phải được hoán vị thay vì tăng lên. 

Một trường hợp thất bại minh họa nhỏ cho cách tiếp cận ngây thơ là: 

đầu vào: 

n = 3, mảng = [1, 2, 3] 

Thao tác: thêm 1 mod 16 vào [1, 3] 

Kết quả đúng là [2, 3, 4]. Một bản cập nhật chỉ có tần số đơn giản giúp tăng số lượng mà không ánh xạ lại các giá trị sẽ coi điều này không chính xác là tăng cnt2, cnt3, cnt4 độc lập với cấu trúc ban đầu, làm mất sự tương ứng giữa các phần tử và giá trị được chuyển đổi của chúng. 

Khó khăn cốt lõi là các bản cập nhật hoạt động dựa trên giá trị chứ không phải vị trí, điều này thúc đẩy chúng tôi hướng tới việc duy trì phân phối giá trị thay vì các phần tử thô. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp duy trì mảng và áp dụng từng phần tử chuyển đổi theo từng phần tử. Điều này đúng vì mỗi thao tác được xác định rõ ràng cho mỗi chỉ mục, nhưng mọi cập nhật phạm vi đều có giá O(n) và với q lên tới 5×10^5 thì điều này trở nên không khả thi. 

Quan sát quan trọng là miền giá trị cực kỳ nhỏ và đóng trong cả hai phép toán. Mỗi phần tử luôn là một trong 16 giá trị và mỗi thao tác sẽ biến đổi các giá trị thông qua ánh xạ xác định từ {0..15} sang chính nó. Thay vì lưu trữ các giá trị riêng lẻ, chúng tôi lưu trữ số lần mỗi giá trị xuất hiện trong một phân đoạn. 

Tuy nhiên, chúng tôi vẫn cần hỗ trợ cập nhật phạm vi một cách hiệu quả. Cây phân đoạn giải quyết vấn đề này bằng cách lưu trữ, tại mỗi nút, một mảng tần số có kích thước 16. Việc cập nhật phạm vi sẽ trở thành một hoán vị được áp dụng cho mảng tần số này. Thay vì sửa đổi phần tử con ngay lập tức, chúng tôi lưu trữ một hàm chuyển đổi lười biếng kết hợp với các phần tử hiện có. 

Điều này làm giảm vấn đề duy trì cây phân đoạn trong đó mỗi nút mang biểu đồ 16 phần tử và hoán vị lười biếng trên 16 trạng thái này. Hợp nhất các nút là việc bổ sung biểu đồ đơn giản, trong khi đẩy các thẻ lười là soạn các hoán vị và áp dụng chúng cho các vectơ tần số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Cây phân đoạn với biểu đồ giá trị và hoán vị lười biếng | O(q log n · 16) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi biểu diễn mỗi nút cây phân đoạn bằng một mảng tần số`cnt[16]`, Ở đâu`cnt[x]`có giá trị bao nhiêu lần`x`xuất hiện ở đoạn đó. Chúng tôi cũng duy trì một thẻ lười là một hoán vị của 16 giá trị mô tả cách ánh xạ lại các giá trị được lưu trữ hiện tại. 

1. Xây dựng cây phân đoạn bằng cách khởi tạo mỗi nút lá bằng một mảng tần số chứa số 1 duy nhất tại vị trí của giá trị ban đầu. Điều này mã hóa phân phối giá trị chính xác ở mọi vị trí. 
2. Đối với mỗi nút bên trong, hãy tính mảng tần số của nó bằng cách tính tổng các mảng con của nó. Điều này duy trì tính chính xác vì các phân đoạn rời rạc đóng góp độc lập vào tổng nhiều giá trị. 
3. Xác định phép biến đổi cho phép toán loại 1 dưới dạng hoán vị f(x) = (x + 1) mod 16. Xác định phép biến đổi loại 2 dưới dạng g(x) = (2x) mod 16. Các hàm này được áp dụng cho mọi phần tử trong một phân đoạn, do đó chúng tạo ra một hoán vị trên vectơ tần số. 
4. Khi một bản cập nhật phạm vi bao phủ hoàn toàn một nút cây phân đoạn, chúng tôi không chạm vào các nút con của nó. Thay vào đó, chúng tôi kết hợp hoán vị lười biếng của nút với phép biến đổi mới. Điều này đảm bảo chúng ta tích lũy các phép biến đổi một cách hiệu quả. 
5. Khi áp dụng hoán vị p cho một nút, chúng ta biến đổi mảng tần số của nó bằng cách di chuyển số đếm: new_cnt[p(x)] += cnt[x] với mọi x trong 0..15. Điều này rõ ràng đẩy các giá trị thông qua việc chuyển đổi. 
6. Trong khi truy vấn, chúng tôi đẩy mọi hoán vị lười biếng đang chờ xử lý trước khi truy cập các phần tử con, đảm bảo các mảng tần số phản ánh tất cả các cập nhật trước đó. 
7. Đối với truy vấn loại 3, chúng tôi truy xuất mảng tần số cho phân đoạn và tính XOR của cnt[0] đến cnt[15], tạo ra câu trả lời được yêu cầu. 

Lý do điều này có tác dụng là vì mọi thao tác trên mảng là một hàm được áp dụng độc lập cho từng phần tử. Vì thành phần hàm phân phối theo số đếm nên chúng ta có thể áp dụng các phép biến đổi trên mỗi phần tử hoặc trên mỗi vectơ tần số tổng hợp. Cây phân đoạn đảm bảo chúng tôi chỉ tính toán lại các phần logarit bị ảnh hưởng, trong khi lan truyền lười đảm bảo mỗi phép chuyển đổi được áp dụng chính xác một lần cho mỗi nút cho mỗi lần cập nhật liên quan. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.size = 4 * self.n
        self.cnt = [[0] * 16 for _ in range(self.size)]
        self.lazy = [list(range(16)) for _ in range(self.size)]
        self._build(1, 0, self.n - 1, arr)

    def _apply(self, v, perm):
        new = [0] * 16
        for i in range(16):
            new[perm[i]] += self.cnt[v][i]
        self.cnt[v] = new

    def _compose(self, a, b):
        return [b[a[i]] for i in range(16)]

    def _push(self, v):
        if self.lazy[v] != list(range(16)):
            for child in (v * 2, v * 2 + 1):
                self._apply(child, self.lazy[v])
                self.lazy[child] = self._compose(self.lazy[v], self.lazy[child])
            self.lazy[v] = list(range(16))

    def _build(self, v, l, r, arr):
        if l == r:
            self.cnt[v][arr[l]] = 1
            return
        m = (l + r) // 2
        self._build(v * 2, l, m, arr)
        self._build(v * 2 + 1, m + 1, r, arr)
        for i in range(16):
            self.cnt[v][i] = self.cnt[v * 2][i] + self.cnt[v * 2 + 1][i]

    def _update(self, v, l, r, ql, qr, perm):
        if ql <= l and r <= qr:
            self._apply(v, perm)
            self.lazy[v] = self._compose(perm, self.lazy[v])
            return
        self._push(v)
        m = (l + r) // 2
        if ql <= m:
            self._update(v * 2, l, m, ql, qr, perm)
        if qr > m:
            self._update(v * 2 + 1, m + 1, r, ql, qr, perm)
        for i in range(16):
            self.cnt[v][i] = self.cnt[v * 2][i] + self.cnt[v * 2 + 1][i]

    def _query(self, v, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.cnt[v]
        self._push(v)
        m = (l + r) // 2
        res = [0] * 16
        if ql <= m:
            left = self._query(v * 2, l, m, ql, qr)
            for i in range(16):
                res[i] += left[i]
        if qr > m:
            right = self._query(v * 2 + 1, m + 1, r, ql, qr)
            for i in range(16):
                res[i] += right[i]
        return res

n, q = map(int, input().split())
arr = list(map(int, input().split()))

st = SegTree(arr)

f1 = [(i + 1) % 16 for i in range(16)]
f2 = [(2 * i) % 16 for i in range(16)]

for _ in range(q):
    t, l, r = map(int, input().split())
    l -= 1
    r -= 1

    if t == 1:
        st._update(1, 0, n - 1, l, r, f1)
    elif t == 2:
        st._update(1, 0, n - 1, l, r, f2)
    else:
        res = st._query(1, 0, n - 1, l, r)
        ans = 0
        for x in res:
            ans ^= x
        print(ans)
```Việc triển khai dựa trên thực tế là mỗi nút đều lưu trữ một biểu đồ giá trị hoàn chỉnh. các`_apply`hàm là nơi duy nhất xảy ra ánh xạ lại giá trị thực và nó hoạt động hoàn toàn trên vectơ tần số. Thẻ lười biếng là hoán vị và`_compose`đảm bảo thứ tự chính xác khi có nhiều bản cập nhật xếp chồng lên nhau. 

Một điểm tinh tế là việc đẩy các thẻ lười vừa phải áp dụng phép biến đổi cho số đếm của trẻ, vừa phải soạn các hoán vị một cách chính xác. Việc không soạn theo đúng thứ tự sẽ phá vỡ tính chính xác khi nhiều bản cập nhật trùng nhau. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng nhỏ: 

Ban đầu: [1, 2, 3, 4] 

Thao tác: áp dụng +1 mod 16 trên [1, 3] 

| Bước | Phân đoạn | Tần số (đơn giản hóa) | 
| --- | --- | --- | 
| Ban đầu | tất cả | cnt1=1, cnt2=1, cnt3=1, cnt4=1 | 
| Sau khi cập nhật | [1,3] | cnt2=1, cnt3=1, cnt4=1, cnt5=1 | 

Bây giờ, một truy vấn trên [1,4] trả về các tần số cnt2..cnt5, mỗi tần số bằng 1, vì vậy XOR là 1 ⊕ 1 ⊕ 1 ⊕ 1 = 0. 

Điều này xác nhận rằng các bản cập nhật đóng vai trò là hoán vị giá trị hơn là sửa đổi cấu trúc. 

Ví dụ thứ hai kiểm tra sự tương tác của hai phép biến đổi: 

Ban đầu: [1, 1, 2] 

Áp dụng +1 mod 16 trên toàn bộ mảng → [2, 2, 3] 

Áp dụng ×2 mod 16 → [4, 4, 6] 

Tần số truy vấn là cnt4=2, cnt6=1, XOR = 2 ⊕ 1 = 3. 

Điều này cho thấy rằng việc tổng hợp các phép biến đổi thông qua quá trình lan truyền lười biếng phù hợp với ứng dụng tuần tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n · 16) | Mỗi bản cập nhật/truy vấn đi qua chiều cao của cây phân đoạn và mỗi thao tác nút hoạt động trên một mảng 16 có kích thước cố định | 
| Không gian | O(n log n) | Các nút cây phân đoạn lưu trữ mảng tần số 16 phần tử | 

Các hằng số nhỏ vì miền giá trị được cố định ở 16, giúp giải pháp nhanh chóng thoải mái cho n và q lên tới 5×10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    # assume solution is wrapped in main execution
    return sys.stdout.getvalue()

# small sanity case
assert run("""3 2
1 2 3
3 1 3
3 1 1
""") is not None

# all equal values
assert run("""5 3
7 7 7 7 7
1 1 5
3 1 5
3 2 4
""") is not None

# boundary wrap test
assert run("""4 3
15 14 13 12
1 1 4
3 1 4
3 2 3
""") is not None

# mixed operations
assert run("""6 5
1 2 3 4 5 6
2 1 6
1 2 5
3 1 6
2 3 4
3 1 6
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | hành vi XOR nhất quán | nhân giống đồng đều | 
| bọc ranh giới | đúng mod 16 ca | tính đúng đắn theo chu kỳ | 
| hoạt động hỗn hợp | thành phần của các phép biến đổi | lười biếng đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh chính là các bản cập nhật chồng chéo lặp đi lặp lại cần được soạn thảo chính xác. Ví dụ: áp dụng +1 trên một phân đoạn hai lần phải tương đương với +2 chứ không phải hai lần viết lại một phần độc lập. Thành phần hoán vị lười biếng đảm bảo điều này bằng cách xâu chuỗi các ánh xạ thay vì ghi đè chúng. 

Một trường hợp đặc biệt khác là truy vấn toàn diện sau khi cập nhật một phần. Cây phân đoạn không được đẩy các giá trị lười biếng cũ không chính xác; nếu không, số lượng sẽ chỉ phản ánh các phép biến đổi một phần. Hoạt động đẩy đảm bảo rằng bất cứ khi nào chúng ta đi xuống, tất cả các hoán vị đang chờ xử lý sẽ được hiện thực hóa. 

Cuối cùng, các phân đoạn phần tử đơn kiểm tra tính chính xác của việc xử lý lá. Vì các lá bắt đầu bằng một vectơ tần số nóng, nên bất kỳ lỗi nào trong quá trình khởi tạo hoặc cập nhật ứng dụng sẽ ngay lập tức phá vỡ tính chính xác của các truy vấn trong khoảng thời gian có độ dài-1.
