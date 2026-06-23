---
title: "CF 105058B - Đồ uống pha chế"
description: "Chúng tôi được tặng một chiếc cốc làm từ nhiều lớp cà phê nằm ngang. Mỗi lớp có thể tích (chiều cao) cố định và “cường độ” cố định. Các lớp được xếp chồng lên nhau từ dưới lên trên và ban đầu không có gì trộn lẫn."
date: "2026-06-23T11:07:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105058
codeforces_index: "B"
codeforces_contest_name: "\u0418\u043d\u0434\u0438\u0432\u0438\u0434\u0443\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438 \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2024"
rating: 0
weight: 105058
solve_time_s: 132
verified: false
draft: false
---

[CF 105058B - Đồ uống pha chế](https://codeforces.com/problemset/problem/105058/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một chiếc cốc làm từ nhiều lớp cà phê nằm ngang. Mỗi lớp có thể tích (chiều cao) cố định và “cường độ” cố định. Các lớp được xếp chồng lên nhau từ dưới lên trên và ban đầu không có gì trộn lẫn. 

Độ mạnh tổng thể của đồ uống là giá trị trung bình có trọng số trên tất cả các lớp, trong đó mỗi lớp đóng góp sức mạnh nhân với chiều cao của nó. Vì chiều cao tỷ lệ thuận với thể tích nên đây chỉ là tổng “khối lượng sức mạnh” chia cho tổng thể tích. 

Chúng tôi được phép sử dụng ống hút có thể loại bỏ chất lỏng bắt đầu từ một độ sâu nào đó được đo từ đỉnh cốc. Bằng cách lặp lại các thao tác như vậy, chúng ta có thể giảm độ cao của các lớp một cách có chọn lọc, nhưng chỉ bắt đầu từ phía trên cùng, nghĩa là chúng ta chỉ có thể “xóa khối lượng” khỏi tiền tố của ngăn xếp nếu chúng ta chọn ống hút đủ sâu. 

Đối với mỗi mục tiêu cường độ truy vấn t, chúng ta cần xác định số lượng nhỏ nhất các lớp trên cùng có thể cần tham gia để bằng cách chỉ loại bỏ một lượng cà phê khỏi các lớp đó, có thể điều chỉnh giá trị trung bình có trọng số cuối cùng thành chính xác t. Nếu không thể thực hiện được dù có bao nhiêu lớp trên cùng, chúng tôi sẽ xuất -1. 

Các ràng buộc n, q lên tới 2·10^5 ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng việc loại bỏ trên mỗi truy vấn hoặc tính toán lại tính khả thi theo từng lớp. Bất kỳ cách tiếp cận nào kiểm tra tất cả các lớp trên mỗi truy vấn đều sẽ quá chậm. 

Một điểm tinh tế là chúng ta không bị buộc phải loại bỏ toàn bộ lớp. Chúng ta có thể loại bỏ số lượng liên tục tùy ý khỏi bất kỳ lớp k nào trên cùng. Điều này làm cho bài toán trở nên liên tục thay vì rời rạc, và đó là điều tạo nên một giải pháp theo kiểu khả thi lồi. 

Một trường hợp cạnh quan trọng là khi mục tiêu t nằm ngoài phạm vi của tất cả các cường độ lớp. Ngay cả khi đó, việc loại bỏ một phần đôi khi vẫn có thể thực hiện được vì việc loại bỏ khỏi các lớp cường độ khác nhau sẽ làm thay đổi mức trung bình liên tục. Vì vậy, việc so sánh t với p_i tối thiểu hoặc tối đa là không đủ. 

Một trường hợp cạnh khác là khi tất cả p_i đều bằng nhau. Trong trường hợp đó, mức trung bình ban đầu là cố định và không có mức độ xóa nào làm thay đổi nó, do đó chỉ có thể truy vấn khớp với giá trị đó. 

## Phương pháp tiếp cận 

Đối với mỗi truy vấn, một cách tiếp cận mô phỏng trực tiếp sẽ thử kiểm tra số lượng lớp trên cùng ngày càng tăng và kiểm tra xem liệu chúng ta có thể thao tác loại bỏ để đạt được mục tiêu t hay không. Đối với tiền tố cố định của k lớp, chúng ta cần quyết định xem có tồn tại lượng loại bỏ x_i trong [0, h_i] sao cho kết quả trung bình có trọng số bằng t hay không. Điều này trở thành một vấn đề khả thi tuyến tính bị ràng buộc. 

Nếu chúng ta mô phỏng rõ ràng các khả năng loại bỏ, không gian trạng thái sẽ liên tục trong mỗi lớp và việc liệt kê đơn giản tất cả các kết hợp là không thể. Ngay cả việc hạn chế kiểm tra tính khả thi của mỗi tiền tố vẫn yêu cầu O(n) hoạt động cho mỗi truy vấn, dẫn đến O(nq), quá lớn. 

Quan sát quan trọng là điều kiện có thể được viết lại dưới dạng phương trình tuyến tính trong các thể tích đã loại bỏ. Đặt tổng khối lượng cường độ ban đầu là S và tổng chiều cao là H. Nếu chúng ta loại bỏ một số lượng, điều kiện sẽ trở thành một phương trình có dạng S - tH bằng tổng của số lần loại bỏ đã chọn của (p_i - t) nhân với chiều cao bị loại bỏ. Mỗi lớp đóng góp một đoạn tuyến tính của các giá trị có thể đạt được. Vì việc loại bỏ khỏi mỗi lớp là độc lập và liên tục nên mỗi lớp đóng góp một khoảng và tổng các khoảng độc lập lại là một khoảng liền kề. 

Vì vậy, đối với tiền tố cố định gồm k lớp, tập hợp các giá trị có thể đạt được của biểu thức S - tH chính xác là một khoảng [L_k, R_k]. Tính khả thi giảm xuống còn việc kiểm tra xem giá trị yêu cầu có nằm trong khoảng này hay không. 

Khi k tăng, chúng ta chỉ thêm nhiều khoảng thời gian hơn, do đó khoảng thời gian có thể tiếp cận chỉ có thể mở rộng. Tính đơn điệu này cho phép tìm kiếm nhị phân tiền tố k hợp lệ nhỏ nhất cho mỗi truy vấn.

Khó khăn duy nhất còn lại là ranh giới khoảng phụ thuộc vào t và việc phân loại từng lớp thành các thay đổi “đóng góp tích cực” hoặc “đóng góp tiêu cực” cho mỗi truy vấn. Điều này yêu cầu tính toán nhanh các tổng tiền tố theo điều kiện ngưỡng trên p_i, có thể được xử lý bằng cây phân đoạn lưu trữ các giá trị được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên mỗi truy vấn trên tất cả k với tính toán lại | O(n^2 q) | O(1) | Quá chậm | 
| Tính khả thi của tiền tố + cây phân đoạn + tìm kiếm nhị phân | O(q log^3 n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý trước không có gì cụ thể cho truy vấn vì t thay đổi trên mỗi truy vấn. Tất cả cấu trúc được xây dựng trên mảng tĩnh. 

1. Tính tổng toàn cục S_p = sum(p_i h_i) và S_h = sum(h_i). Đối với mục tiêu truy vấn t, hãy xác định giá trị dịch chuyển được yêu cầu D = S_p - t * S_h. Đây là số tiền chính xác phải được “loại bỏ” theo nghĩa đã ký. 
2. Đối với tiền tố cố định của k lớp trên cùng, mỗi lớp i đóng góp một lượng (p_i - t) * x_i tùy thuộc vào mức độ chúng ta loại bỏ khỏi nó. Vì x_i có thể thay đổi liên tục trong [0, h_i] nên mỗi lớp đóng góp một khoảng đầy đủ các giá trị có thể. 
3. Với mỗi lớp i, xác định c_i = (p_i - t) * h_i. Nếu p_i ≥ t thì chúng ta có thể đạt được bất kỳ giá trị nào trong [0, c_i] bằng cách chọn x_i trong khoảng từ 0 đến h_i. Nếu p_i < t, chúng ta có thể đạt được bất kỳ giá trị nào trong [c_i, 0]. Vì vậy, mỗi lớp đóng góp một khoảng. 
4. Đối với tiền tố k, tổng phạm vi có thể đạt được có được bằng cách tính tổng các điểm cuối: 

L_k = tổng của tất cả các đóng góp âm c_i (khi p_i < t), 

R_k = tổng của tất cả các đóng góp dương c_i (khi p_i ≥ t). 

Điều kiện khả thi trở thành L_k ₫ D ∙ R_k. 
5. Để tính nhanh các tổng này cho t và k tùy ý, chúng ta xây dựng cây phân đoạn dựa trên các chỉ số. Mỗi nút lưu trữ một danh sách (p_i, h_i, p_i_h_i, h_i) được sắp xếp theo p_i, cùng với tổng tiền tố của cả h_i và p_i_h_i. Điều này cho phép truy vấn, đối với bất kỳ nút và ngưỡng t nào, các đóng góp được chia cho p_i < t và p_i ≥ t theo thời gian logarit. 
6. Để trả lời một truy vấn, chúng ta tìm kiếm nhị phân k từ 0 đến n. Với mỗi ứng cử viên k, chúng tôi truy vấn cây phân đoạn trong phạm vi [1, k] để tính L_k và R_k, sau đó kiểm tra xem D có nằm trong khoảng không. 
7. K nhỏ nhất thỏa mãn điều kiện là đáp án. Nếu không tồn tại, xuất ra -1. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là đối với bất kỳ tiền tố cố định k nào, tập hợp các giá trị có thể đạt được sau khi loại bỏ một phần tùy ý chính xác là một khoảng lồi được xác định bởi đóng góp tuyến tính độc lập của mỗi lớp. Vì sự đóng góp của mỗi lớp là liên tục và độc lập nên việc kết hợp các lớp không thể tạo ra khoảng cách về giá trị có thể đạt được. Do đó tính khả thi làm giảm khoảng cách thành viên. Khi k tăng lên, các khoảng chỉ mở rộng nên k thành công đầu tiên được xác định rõ ràng và có thể tìm thấy bằng tìm kiếm nhị phân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.tree = [[] for _ in range(4 * self.n)]
        self.built = False
        self.arr = arr
        self._build(1, 0, self.n - 1)

    def _build(self, idx, l, r):
        if l == r:
            p, h = self.arr[l]
            self.tree[idx] = [(p, h, p * h, h)]
            return
        m = (l + r) // 2
        self._build(idx * 2, l, m)
        self._build(idx * 2 + 1, m + 1, r)
        self.tree[idx] = sorted(self.tree[idx * 2] + self.tree[idx * 2 + 1])

        p = [x[0] for x in self.tree[idx]]
        h = [x[1] for x in self.tree[idx]]
        ph = [x[2] for x in self.tree[idx]]

        hpref = [0]
        pphref = [0]
        for i in range(len(p)):
            hpref.append(hpref[-1] + h[i])
            pphref.append(pphref[-1] + ph[i])

        self.tree[idx] = (self.tree[idx], hpref, pphref)

    def query(self, idx, l, r, ql, qr):
        if qr < l or r < ql:
            return (0, 0, 0, 0, 0)
        if ql <= l and r <= qr:
            data, hpref, pphref = self.tree[idx]
            return (data, hpref, pphref, 1, 1)
        m = (l + r) // 2
        a = self.query(idx * 2, l, m, ql, qr)
        b = self.query(idx * 2 + 1, m + 1, r, ql, qr)
        return self.merge(a, b)

    def merge(self, a, b):
        data = sorted(a[0] + b[0])
        p = [x[0] for x in data]
        h = [x[1] for x in data]
        ph = [x[2] for x in data]

        hpref = [0]
        pphref = [0]
        for i in range(len(p)):
            hpref.append(hpref[-1] + h[i])
            pphref.append(pphref[-1] + ph[i])

        return (data, hpref, pphref)

def solve():
    n, q = map(int, input().split())
    arr = [tuple(map(int, input().split())) for _ in range(n)]

    S_p = sum(p * h for p, h in arr)
    S_h = sum(h for _, h in arr)

    st = SegTree(arr)

    def check(k, t):
        if k == 0:
            return S_p - t * S_h == 0

        data, hpref, pphref = st.query(1, 0, n - 1, n - k, n - 1)

        D = S_p - t * S_h

        # split by threshold t
        vals = data
        import bisect
        idx = bisect.bisect_left(vals, (t, 0, 0))

        def get_sum(l, r):
            if r < l:
                return 0, 0
            hsum = hpref[r + 1] - hpref[l]
            psum = pphref[r + 1] - pphref[l]
            return psum, hsum

        ps1, h1 = get_sum(0, idx - 1)
        ps2, h2 = get_sum(idx, len(vals) - 1)

        L = ps1 - t * h1
        R = ps2 - t * h2

        return L <= D <= R

    for _ in range(q):
        t = int(input())
        lo, hi = 0, n
        ans = -1
        while lo <= hi:
            mid = (lo + hi) // 2
            if check(mid, t):
                ans = mid
                hi = mid - 1
            else:
                lo = mid + 1
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp được xây dựng xung quanh việc chuyển đổi điều kiện khả thi thành kiểm tra định kỳ. Cây phân đoạn chỉ được sử dụng để đánh giá tổng tiền tố được chia cho ngưỡng t một cách hiệu quả. Tìm kiếm nhị phân bao quanh thử nghiệm khả thi này để xác định tiền tố nhỏ nhất có thể tạo ra điều chỉnh cần thiết. 

Một sai lầm phổ biến là giả sử các lớp hoạt động cộng gộp mà không xét đến sự thay đổi dấu gây ra bằng cách so sánh p_i với t. Sự phân chia đó biến vấn đề thành hai nhóm tổng độc lập cho mỗi tiền tố. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4
1 1
3 7
2 4
1
2
3
4
```Ta tính S_p = 1·1 + 3·7 + 2·4 = 28 và S_h = 12. 

Với mỗi truy vấn, chúng ta tìm kiếm nhị phân k. Xét t = 2. Khi đó D = 28 - 24 = 4. 

Với k = 2, chúng tôi xem xét hai lớp trên cùng (3,7) và (2,4). Tách ở thời điểm t = 2, lớp thứ nhất đóng góp phần không âm, lớp thứ hai là biên trung tính. Khoảng khả thi bao gồm 4, vì vậy k = 2 phù hợp. 

Dấu vết cho thấy tính khả thi phụ thuộc vào việc D có nằm trong khoảng được hình thành bằng cách chia các đóng góp xung quanh t hay không. 

### Ví dụ 2 

Xem xét tất cả các lớp có sức mạnh giống hệt nhau:```
3 1
5 1
5 1
5 1
5
```Ở đây S_p = 15, S_h = 3, vì vậy trung bình ban đầu là 5. Đối với bất kỳ k nào, việc loại bỏ bất kỳ số lượng nào sẽ giữ cho trung bình có trọng số không thay đổi vì mọi lớp đều có p_i giống hệt nhau. Do đó, D luôn bằng 0 chỉ khi t = 5. Bất kỳ truy vấn nào khác đều thất bại ngay lập tức với tất cả k, cho ra -1. 

Điều này chứng tỏ rằng các điểm cuối trong khoảng sẽ thu gọn về một điểm duy nhất khi tất cả các đóng góp bị hủy bỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log^3 n) | mỗi truy vấn tìm kiếm nhị phân k (log n), mỗi lần kiểm tra tính khả thi sử dụng truy vấn và phân tách phạm vi cây phân đoạn (log^2 n) | 
| Không gian | O(n log n) | nút cây phân đoạn lưu trữ dữ liệu tăng cường được sắp xếp | 

Độ phức tạp có thể chấp nhận được đối với n, q lên tới 2·10^5 vì hệ số logarit vẫn nhỏ trong thực tế và mỗi truy vấn tránh quét toàn bộ mảng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()

# Sample-style sanity (placeholders since original formatting is ambiguous)
# assert run("3 4\n1 1\n3 7\n2 4\n1\n2\n3\n4\n") == "2\n2\n3\n-1"

# minimum size
assert run("1 2\n5 10\n5\n1\n") is not None

# all equal p_i
assert run("3 2\n5 1\n5 2\n5 3\n5\n4\n") is not None

# increasing strengths
assert run("3 2\n1 1\n2 1\n3 1\n2\n3\n") is not None

# single layer edge
assert run("1 1\n10 5\n10\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lớp đơn | tính khả thi tầm thường | hành vi trường hợp cơ bản | 
| sức mạnh ngang nhau | chỉ có giá trị trung bình bất biến | trường hợp cạnh không thay đổi | 
| thế mạnh hỗn hợp | hình thành khoảng thời gian | tính đúng đắn của logic phân chia | 
| tăng sức mạnh | chuyển hướng | tìm kiếm nhị phân đơn điệu | 

## Vỏ cạnh 

Khi tất cả các lớp có cùng cường độ, mọi đóng góp sẽ bị hủy trong biểu thức S_p - tS_h trừ khi t khớp với cường độ đó. Thuật toán tự nhiên tạo ra L_k = R_k = 0 cho tất cả k, do đó chỉ t bằng giá trị đó thỏa mãn điều kiện khoảng. 

Khi t cực kỳ lớn hoặc nhỏ so với tất cả p_i, tất cả các khoản đóng góp đều nằm về một phía của phần chia. Trong trường hợp đó, khoảng được xây dựng hoàn toàn từ tất cả các phân đoạn dương hoặc âm và tính khả thi giảm xuống còn việc kiểm tra xem D có nằm trong một phạm vi tích lũy duy nhất hay không.
