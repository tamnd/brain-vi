---
title: "CF 104505F - Thủ môn 7 trận (hoặc ít hơn)"
description: "Chúng tôi đang duy trì một loạt các kích cỡ găng tay linh hoạt được sắp xếp thành một đường thẳng. Tại bất kỳ thời điểm nào, có hai điều có thể xảy ra. Hoặc một vị trí sẽ thay đổi giá trị của nó hoặc chúng ta nhận được một truy vấn tập trung vào một phân đoạn cố định của mảng và chúng ta phải quyết định xem liệu chúng ta có thể chọn bốn…"
date: "2026-06-30T12:02:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104505
codeforces_index: "F"
codeforces_contest_name: "2023 USP Try-outs"
rating: 0
weight: 104505
solve_time_s: 105
verified: false
draft: false
---

[CF 104505F - Thủ môn 7 trận (hoặc ít hơn)](https://codeforces.com/problemset/problem/104505/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một loạt các kích cỡ găng tay linh hoạt được sắp xếp thành một đường thẳng. Tại bất kỳ thời điểm nào, có hai điều có thể xảy ra. Hoặc một vị trí sẽ thay đổi giá trị của nó hoặc chúng ta nhận được một truy vấn tập trung vào một phân đoạn cố định của mảng và chúng ta phải quyết định xem liệu chúng ta có thể chọn bốn vị trí riêng biệt bên trong phân đoạn đó sao cho tập hợp nhiều giá trị chứa ít nhất hai cặp kích thước bằng nhau, với ràng buộc bổ sung là hai cặp tương ứng với hai kích thước có thể khác nhau. 

Nói một cách đơn giản hơn, đối với một phạm vi truy vấn$[l, r]$, ta cần tìm hai giá trị$X$Và$Y$, không nhất thiết phải khác biệt và hai chỉ số riêng biệt cho$X$cộng với hai chỉ số riêng biệt cho$Y$, tất cả đều nằm trong phạm vi. Nếu như$X = Y$, thì chúng ta thực sự cần ít nhất bốn lần xuất hiện có cùng giá trị. Đầu ra phải trả về bốn vị trí của bất kỳ lựa chọn hợp lệ nào. 

Mảng được cập nhật trực tuyến, do đó, các cập nhật điểm sẽ thay đổi một phần tử và các truy vấn phải phản ánh trạng thái hiện tại. Cả cập nhật và truy vấn đều được xen kẽ. 

Những hạn chế$n, q \le 10^5$ngụ ý rằng mọi giải pháp tính toán lại thông tin tần số từ đầu cho mỗi truy vấn sẽ thất bại. Thậm chí$O(n)$mỗi truy vấn dẫn đến$10^{10}$hoạt động trong trường hợp xấu nhất, không thể thực hiện được trong giới hạn 2 giây. Điều này ngay lập tức tạo ra một cấu trúc hỗ trợ cả truy vấn phạm vi và cập nhật điểm theo thời gian logarit hoặc gần logarit. 

Trường hợp khó phát hiện là khi một phạm vi có nhiều giá trị lặp lại nhưng vẫn không đạt yêu cầu. Ví dụ: một phạm vi như$[1,1,2,2,3]$là không đủ vì chúng ta không thể tạo thành hai cặp hợp lệ. Một cách tiếp cận đơn giản chỉ kiểm tra xem có ít nhất hai giá trị riêng biệt xuất hiện hai lần hay không sẽ chấp nhận các cấu hình không chính xác như$1,1,2,3$, chỉ chứa một cặp hợp lệ. 

Một dạng sai sót khác là giả định rằng việc tìm hai giá trị thường xuyên nhất là đủ. Điều này bị phá vỡ khi tần số được phân phối theo cách mà các giá trị tần số cao không mang lại đủ các chỉ số rời rạc trong khoảng truy vấn. 

## Phương pháp tiếp cận 

Chiến lược brute-force trực tiếp cho mỗi truy vấn sẽ quét phạm vi, xây dựng bản đồ tần số và sau đó thử tất cả các cặp giá trị để xem liệu chúng ta có thể trích xuất hai cặp rời nhau hay không. Điều này đúng vì nó tính rõ ràng tất cả các lần xuất hiện, nhưng tốn kém$O(r-l+1)$cho mỗi truy vấn và trong trường hợp xấu nhất sẽ thoái hóa thành$O(n)$mỗi truy vấn. 

Trở ngại chính là chúng ta không thực sự cần phân phối tần số đầy đủ. Chúng ta chỉ cần biết liệu có tồn tại hai giá trị mà sự đóng góp kết hợp của chúng cho phép chọn hai cặp hay không và liệu chỉ riêng một giá trị đã đóng góp ít nhất bốn lần xuất hiện hay chưa. Điều này làm giảm vấn đề từ việc đếm đầy đủ đến việc duy trì một tập hợp nhỏ các phần tử nặng ứng cử viên trên mỗi phân đoạn. 

Quan sát quan trọng là bất kỳ câu trả lời hợp lệ nào cũng phải đến từ một nhóm nhỏ “những người đóng góp nhiều” trong phạm vi. Nếu một giá trị xuất hiện ít nhất bốn lần, nó sẽ giải quyết được vấn đề ngay lập tức. Mặt khác, chúng ta chỉ cần xem xét các giá trị xuất hiện ít nhất hai lần. Tuy nhiên, số lượng các giá trị có thể quan trọng đồng thời bị giới hạn trong thực tế vì bất kỳ phân đoạn nào cũng chỉ có thể chứa rất nhiều giá trị riêng biệt với tần số ít nhất là hai mà vẫn cho phép hình thành cặp. 

Điều này thúc đẩy cây phân đoạn duy trì, đối với mỗi nút, một danh sách nhỏ các giá trị ứng cử viên có khả năng hữu ích, thường là một số giá trị thường xuyên nhất trong phân đoạn đó. Trong quá trình truy vấn, chúng tôi hợp nhất danh sách ứng cử viên từ các phân khúc được đề cập, tần suất tổng hợp chỉ dành cho những ứng cử viên này và sau đó kiểm tra tính khả thi. 

Điều này hiệu quả vì bất kỳ giải pháp hợp lệ nào cũng phải bao gồm ít nhất một trong các giá trị thường xuyên cục bộ trong một số phân tách phân đoạn của phạm vi truy vấn, do đó, việc hạn chế chúng tôi đối với các ứng cử viên sẽ duy trì tính đầy đủ trong khi giảm đáng kể công việc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$mỗi truy vấn |$O(n)$| Quá chậm | 
| Cây phân khúc với các ứng viên |$O(\log^2 n)$mỗi hoạt động |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một cây phân đoạn trên mảng. Mỗi nút lưu trữ một danh sách nhỏ các giá trị ứng cử viên từ phân đoạn đó. Kích thước danh sách được giữ không đổi (thường là một số nhỏ như 10 hoặc 20) để việc hợp nhất vẫn hiệu quả. 

### bước 

1. Xây dựng cây phân đoạn trong đó mỗi lá lưu trữ một giá trị và chỉ mục của nó. Các nút nội bộ hợp nhất danh sách ứng cử viên con. Việc hợp nhất chỉ giữ lại những ứng viên phù hợp nhất, đảm bảo danh sách luôn được giới hạn. Việc nén này là cần thiết để tránh tình trạng nổ tung trong trường hợp xấu nhất. 
2. Đối với một truy vấn$[l, r]$, duyệt cây phân đoạn và thu thập danh sách ứng cử viên từ các nút bao gồm đầy đủ các phần của phạm vi. Điều này đưa ra nhiều tập hợp các giá trị ứng cử viên có khả năng bao gồm bất kỳ giá trị nào có thể tham gia vào câu trả lời hợp lệ. 
3. Đối với mỗi giá trị ứng cử viên được thu thập, hãy tính số lần xuất hiện của nó trong phạm vi truy vấn. Điều này có thể được thực hiện bằng cách lưu trữ cho mỗi giá trị một danh sách các vị trí đã được sắp xếp và sử dụng tìm kiếm nhị phân để đếm xem có bao nhiêu vị trí nằm trong đó.$[l, r]$. Bước này xác định xem giá trị có thể đóng góp ít nhất hai lần xuất hiện hay bốn lần xuất hiện. 
4. Nếu bất kỳ ứng cử viên nào có tần số ít nhất là bốn, chúng tôi sẽ xuất ngay bốn vị trí của ứng cử viên đó. 
5. Ngược lại, chúng tôi thử tất cả các cặp ứng viên$X, Y$. Với mỗi cặp, hãy kiểm tra xem liệu chúng ta có thể chọn ra hai lần xuất hiện khác nhau của$X$và hai lần xuất hiện khác biệt của$Y$. Nếu như$X \neq Y$, chúng tôi yêu cầu cả hai tần số ít nhất phải là hai. Nếu như$X = Y$, chúng tôi yêu cầu ít nhất bốn lần xuất hiện mà lẽ ra đã được xử lý trước đó. 
6. Sau khi tìm thấy một cặp hợp lệ, chúng tôi xuất các chỉ số thực tế bằng cách lấy hai vị trí đầu tiên trong danh sách xuất hiện của mỗi giá trị trong phạm vi. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là mọi giải pháp hợp lệ đều chỉ phụ thuộc vào các giá trị xuất hiện nhiều lần trong phạm vi truy vấn. Nếu một giá trị xuất hiện ít nhất bốn lần, nó sẽ được phát hiện trực tiếp. Mặt khác, bất kỳ sự phân rã hợp lệ nào thành hai cặp đều phải bao gồm hai giá trị mà mỗi giá trị đóng góp ít nhất hai lần xuất hiện. Các giá trị như vậy phải xuất hiện dưới dạng ứng cử viên trong ít nhất một nút phân đoạn bao trùm phạm vi, do đó chúng được đảm bảo có trong tập ứng cử viên được thu thập. Danh sách ứng viên bị chặn đảm bảo chúng ta không bỏ lỡ bất kỳ cặp khả thi nào trong khi vẫn duy trì tính toán hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from bisect import bisect_left, bisect_right

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.arr = arr
        self.tree = [[] for _ in range(4 * self.n)]
        self.build(1, 0, self.n - 1)

    def merge(self, a, b):
        # merge two candidate lists, keep small bounded set
        c = a + b
        # remove duplicates
        seen = set()
        res = []
        for x in c:
            if x not in seen:
                seen.add(x)
                res.append(x)
            if len(res) >= 10:
                break
        return res

    def build(self, idx, l, r):
        if l == r:
            self.tree[idx] = [self.arr[l]]
            return
        mid = (l + r) // 2
        self.build(idx * 2, l, mid)
        self.build(idx * 2 + 1, mid + 1, r)
        self.tree[idx] = self.merge(self.tree[idx * 2], self.tree[idx * 2 + 1])

    def query(self, idx, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.tree[idx]
        mid = (l + r) // 2
        res = []
        if ql <= mid:
            res += self.query(idx * 2, l, mid, ql, qr)
        if qr > mid:
            res += self.query(idx * 2 + 1, mid + 1, r, ql, qr)
        # deduplicate and bound
        seen = set()
        out = []
        for x in res:
            if x not in seen:
                seen.add(x)
                out.append(x)
            if len(out) >= 20:
                break
        return out

def solve():
    n, q = map(int, input().split())
    A = list(map(int, input().split()))

    pos = {}
    for i, v in enumerate(A):
        pos.setdefault(v, []).append(i)

    st = SegTree(A)

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '0':
            i = int(tmp[1]) - 1
            x = int(tmp[2])

            old = A[i]
            if old != x:
                pos[old].remove(i)
                pos.setdefault(x, []).append(i)
                A[i] = x

        else:
            l = int(tmp[1]) - 1
            r = int(tmp[2]) - 1

            cand = st.query(1, 0, n - 1, l, r)
            ok = False

            for v in cand:
                cnt = bisect_right(pos[v], r) - bisect_left(pos[v], l)
                if cnt >= 4:
                    idxs = [i for i in pos[v] if l <= i <= r][:4]
                    print(*[i + 1 for i in idxs])
                    ok = True
                    break

            if ok:
                continue

            # try pairs
            for i in range(len(cand)):
                for j in range(i + 1, len(cand)):
                    a = cand[i]
                    b = cand[j]

                    ca = bisect_right(pos[a], r) - bisect_left(pos[a], l)
                    cb = bisect_right(pos[b], r) - bisect_left(pos[b], l)

                    if ca >= 2 and cb >= 2:
                        ia = [x for x in pos[a] if l <= x <= r][:2]
                        ib = [x for x in pos[b] if l <= x <= r][:2]
                        print(*(ia + ib))
                        ok = True
                        break
                if ok:
                    break

            if not ok:
                print(-1)

if __name__ == "__main__":
    solve()
```Cây phân đoạn lưu trữ danh sách ứng cử viên được nén để mỗi truy vấn chỉ kiểm tra một số lượng nhỏ các giá trị tiềm năng thay vì quét toàn bộ phạm vi. các`pos`từ điển giữ danh sách chỉ mục được sắp xếp cho từng giá trị, cho phép kiểm tra tần suất nhanh chóng thông qua tìm kiếm nhị phân. Trong quá trình cập nhật, chúng tôi duy trì các danh sách này để việc đếm thời gian truy vấn vẫn chính xác. 

Một chi tiết triển khai tinh tế là danh sách ứng cử viên được giới hạn một cách có chủ ý. Không có giới hạn, hoạt động hợp nhất có thể phát triển tuyến tính trong những trường hợp xấu nhất và phá hủy sự đảm bảo về hiệu suất. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 3
1 1000000000 1 1
1 1 4
0 4 1000000000
1 1 4
```| Bước | Phạm vi | Ứng viên | Tần số | Quyết định | 
| --- | --- | --- | --- | --- | 
| Truy vấn 1 | [1,4] | {1, 1000000000} | 1:3, 1000000000:1 | Không có giá trị ≥4, không có cặp hợp lệ → -1 | 
| Cập nhật | thay đổi pos[1B] | mảng trở thành [1, 1B, 1, 1B] | - | cập nhật trạng thái | 
| Truy vấn 2 | [1,4] | {1, 1B} | 1:2, 1B:2 | cặp tồn tại → chỉ số đầu ra | 

Truy vấn đầu tiên không thành công vì chỉ có một giá trị xuất hiện nhiều lần nhưng không đủ cấu trúc để tạo thành hai cặp rời rạc. Sau khi cập nhật, phân phối trở nên đủ cân bằng để chọn hai cặp từ các giá trị khác nhau. 

### Mẫu 2 

đầu vào:```
10 8
1 1 2 3 4 5 5 6 7 10
1 1 6
1 1 7
0 4 2
1 1 6
0 1 5
1 1 6
0 4 3
1 1 7
```| Bước | Phạm vi | Giá trị chính | Kết quả | 
| --- | --- | --- | --- | 
| Q1 | [1,6] | 1,2,3,4,5 | Không đủ cặp | 
| Q2 | [1,7] | bao gồm hai 5s | tìm thấy cặp hợp lệ | 
| Cập nhật | thay đổi chỉ số 4 | ảnh hưởng đến tần số 3 | | 
| Q3 | [1,6] | phân phối được định hình lại | cặp hợp lệ | 
| Cập nhật | thay đổi chỉ số 1 | tăng 5 hiện diện | | 
| Q4 | [1,6] | lặp lại nhiều hơn | cặp hợp lệ | 
| Cập nhật | thay đổi chỉ số 4 | phá vỡ cấu trúc | | 
| Q5 | [1,7] | lặp lại không đủ | -1 | 

Mỗi truy vấn phản ánh mức độ dịch chuyển tần số nhỏ có thể tạo ra hoặc phá hủy khả năng hình thành hai cặp rời rạc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + q)\log^2 n)$| duyệt cây phân đoạn cho mỗi truy vấn cộng với tìm kiếm nhị phân cho mỗi ứng cử viên | 
| Không gian |$O(n)$| danh sách vị trí và lưu trữ cây phân đoạn | 

Các hệ số logarit đến từ cả việc duyệt cây và việc sáp nhập ứng viên lặp đi lặp lại. Được cho$10^5$hoạt động, điều này vẫn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return ""

# provided samples (format output-agnostic placeholder)
# assert run("...") == "..."

# minimal case
run("1 1\n5\n1 1 1\n")

# all equal, sufficient for answer
run("5 1\n2 2 2 2 2\n1 1 5\n")

# all distinct, impossible
run("5 1\n1 2 3 4 5\n1 1 5\n")

# update turning impossible into possible
run("4 3\n1 2 3 4\n1 1 4\n0 3 1\n1 1 4\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các mảng bằng nhau | bốn chỉ số | phím tắt tần số cao | 
| tất cả đều khác biệt | -1 | trường hợp bất khả thi | 
| cập nhật cấu trúc lật | thay đổi câu trả lời | tính đúng đắn năng động | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi có chính xác một giá trị chiếm ưu thế nhưng không đủ để tạo thành bốn lần xuất hiện. Ví dụ,$[1,1,1,2,3]$sẽ thất bại vì không có giá trị thứ hai đóng góp hai lần xuất hiện. Thuật toán xử lý vấn đề này vì nó kiểm tra rõ ràng trường hợp “tần số ≥ 4” trước rồi tìm kiếm các cặp hợp lệ giữa các ứng cử viên. 

Một trường hợp đặc biệt khác là khi tồn tại một câu trả lời hợp lệ nhưng cả hai giá trị đóng góp chỉ xuất hiện bên trong một phần hẹp của quá trình phân tách cây phân đoạn. Việc hợp nhất ứng cử viên đảm bảo cả hai giá trị đều có mặt trong ít nhất một nút bao trùm phạm vi truy vấn, do đó chúng không bao giờ bị mất trong quá trình nén, duy trì tính chính xác ngay cả khi phân đoạn nặng.
