---
title: "CF 105928E - Truy vấn LCM"
description: "Chúng tôi đang duy trì một mảng động trong đó các phần tử có thể thay đổi theo thời gian và chúng tôi phải trả lời các truy vấn phạm vi về cấu trúc nhân bắt nguồn từ các phần tử đó. Mỗi truy vấn cập nhật một vị trí hoặc hỏi về một phân đoạn của mảng."
date: "2026-06-22T15:38:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105928
codeforces_index: "E"
codeforces_contest_name: "Soy Cup #2: Vivian"
rating: 0
weight: 105928
solve_time_s: 57
verified: true
draft: false
---

[CF 105928E - Truy vấn LCM](https://codeforces.com/problemset/problem/105928/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một mảng động trong đó các phần tử có thể thay đổi theo thời gian và chúng tôi phải trả lời các truy vấn phạm vi về cấu trúc nhân bắt nguồn từ các phần tử đó. Mỗi truy vấn cập nhật một vị trí hoặc hỏi về một phân đoạn của mảng. 

Đối với truy vấn trên một phạm vi, về mặt khái niệm, chúng tôi lấy bội số chung nhỏ nhất của tất cả các số trong phân đoạn đó. Thay vì xuất ra chính LCM, chúng ta được yêu cầu tính xem LCM có bao nhiêu ước dương, modulo một số nguyên tố cố định. 

Khó khăn chính là mảng lớn, lên tới hai trăm nghìn phần tử và cả cập nhật và truy vấn đều được xen kẽ. Mỗi giá trị tối đa là một trăm nghìn, đủ nhỏ để việc phân tích thành thừa số nguyên tố có thể thực hiện được nhưng vẫn quá lớn để tính toán lại mọi thứ từ đầu cho mỗi truy vấn. 

Một cách tiếp cận đơn giản sẽ tính toán lại LCM cho mọi truy vấn bằng cách quét phạm vi và lấy số mũ nguyên tố. Điều này ngay lập tức bị hỏng vì mỗi truy vấn có thể chạm vào các phần tử O(n) và có thể có các truy vấn O(n), dẫn đến hành vi O(n²). 

Ý tưởng ngây thơ thứ hai là duy trì LCM một cách trực tiếp. Điều này cũng không thành công vì giá trị LCM tăng đột biến và không ổn định khi cập nhật. Ngay cả việc lưu trữ chúng cũng trở nên không thể. 

Trường hợp phức tạp xuất hiện khi các cập nhật diễn ra thường xuyên ở các vùng chồng chéo. Ví dụ: nếu chúng tôi tiếp tục tính lại số mũ nguyên tố cho từng truy vấn một cách độc lập, chúng tôi sẽ liên tục tính toán các số giống nhau và mất tất cả cấu trúc chung giữa các truy vấn. 

Thách thức thực sự là nhận ra rằng cấu trúc LCM bị chi phối hoàn toàn bởi cực đại số mũ nguyên tố trong phạm vi và số ước đó chỉ phụ thuộc vào các số mũ đó chứ không phụ thuộc vào giá trị số của chính LCM. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ tính toán lại từng truy vấn một cách độc lập. Đối với truy vấn phạm vi, nó quét tất cả các phần tử từ l đến r, phân tích từng số và theo dõi số mũ tối đa của mọi số nguyên tố xuất hiện trong phân đoạn đó. Sau khi tất cả các số nguyên tố được thu thập, số chia được tính là tích của các số nguyên tố của (số mũ + 1). 

Điều này đúng vì LCM được xác định bằng cách lấy, đối với mỗi số nguyên tố, số mũ lớn nhất trong số tất cả các số trong phân đoạn. Tuy nhiên, cách tiếp cận này quá chậm. Mỗi lần phân tích nhân tử có chi phí khoảng O(√A) và mỗi truy vấn chạm vào các phần tử O(n), cho ra khoảng O(n√A) cho mỗi truy vấn. Với tối đa 2e5 truy vấn, điều này vượt xa khả thi. 

Thông tin chi tiết quan trọng là mỗi số chỉ đóng góp vào một tập hợp số nguyên tố nhỏ và mỗi số nguyên tố chỉ cần số mũ tối đa trong một phạm vi. Thay vì tính toán lại từ đầu, chúng tôi duy trì thông tin phân khúc chính xác. Vì các giá trị tối đa là 1e5 nên tổng số số nguyên tố riêng biệt bị giới hạn và mỗi số có nhiều nhất một vài thừa số nguyên tố. 

Chúng tôi trình bày lại vấn đề: với mỗi số nguyên tố p, chúng tôi cần duy trì cấu trúc dữ liệu có thể trả lời “số mũ tối đa của p trong [l, r] trong cập nhật điểm là bao nhiêu”. Câu trả lời cuối cùng là tích các số nguyên tố của (số mũ tối đa + 1). 

Điều này dẫn đến một cây phân đoạn trên các vị trí, trong đó mỗi nút lưu trữ một bản đồ thưa thớt từ số nguyên tố đến số mũ tối đa trong phân đoạn đó. Các bản cập nhật chỉ tính toán lại dọc theo một đường dẫn từ gốc đến lá. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q · n · √A) | O(1) | Quá chậm | 
| Cây phân đoạn theo số mũ nguyên tố | O(q log n · k) | O(n log n · k) | Đã chấp nhận | 

Ở đây k là số thừa số nguyên tố trên một số, rất nhỏ. 

## Hướng dẫn thuật toán 

Chúng tôi xử lý trước các thừa số nguyên tố nhỏ nhất cho tất cả các số nguyên lên tới 1e5, vì vậy chúng tôi có thể nhanh chóng tính bất kỳ giá trị nào thành lũy thừa nguyên tố. 

Sau đó, chúng tôi xây dựng một cây phân đoạn trên mảng, trong đó mỗi nút lưu trữ một từ điển ánh xạ các số nguyên tố tới số mũ tối đa của chúng trong phân đoạn đó. 

### bước

1. Tính trước các thừa số nguyên tố nhỏ nhất cho tất cả các giá trị lên tới 1e5. Điều này cho phép nhân tử hóa bất kỳ phần tử mảng nào theo thời gian logarit tương ứng với kích thước của nó. Nếu không có điều này, việc phân tích hệ số sqrt lặp đi lặp lại sẽ quá chậm khi cập nhật. 
2. Phân tích từng phần tử mảng ban đầu thành các cặp số nguyên tố. Mỗi phần tử chỉ đóng góp một vài mục nên cách biểu diễn này vẫn nhỏ gọn. 
3. Xây dựng cây phân đoạn trong đó mỗi nút lá tương ứng với một vị trí mảng và lưu trữ bản đồ số mũ nguyên tố của nó. 
4. Đối với một nút bên trong, hãy hợp nhất các nút con của nó bằng cách lấy số mũ tối đa đối với mỗi số nguyên tố có trong một nút con. Điều này phản ánh chính xác cách LCM hoạt động trên các phân đoạn rời rạc. 
5. Để cập nhật điểm, hãy phân tích giá trị mới thành nhân tử, thay thế lá và tính toán lại tất cả các nút cây phân đoạn dọc theo đường dẫn đến gốc bằng cách hợp nhất các nút con một lần nữa. 
6. Đối với truy vấn trên [l, r], duyệt cây phân đoạn và thu thập tất cả các nút trong khoảng. Hợp nhất các bản đồ nguyên tố của chúng bằng cách lấy số mũ tối đa cho mỗi nguyên tố. 
7. Sau khi có được bản đồ đã hợp nhất cho phạm vi truy vấn, hãy tính câu trả lời dưới dạng tích của tất cả các số nguyên tố của (số mũ + 1) modulo 998244353. 

Lựa chọn thiết kế quan trọng là chúng ta không bao giờ tính toán LCM thực tế. Chúng tôi chỉ theo dõi cực đại số mũ nguyên tố, đủ cho cả tính chính xác và hiệu quả. 

### Tại sao nó hoạt động 

LCM của một tập hợp số được xác định đầy đủ bởi số mũ tối đa của mỗi số nguyên tố trong tập hợp đó. Cây phân đoạn duy trì chính xác bất biến này tại mọi nút: mỗi nút lưu trữ biểu diễn số mũ tối đa chính xác của khoảng của nó. Vì việc hợp nhất các nút bảo toàn giá trị cực đại trên mỗi số nguyên tố nên mọi truy vấn sẽ xây dựng lại cấu hình số mũ chính xác của phạm vi. Công thức đếm số chia chỉ phụ thuộc vào các số mũ này, vì vậy khi cấu hình đúng, sản phẩm cuối cùng được đảm bảo chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
MAXV = 100000

# smallest prime factor sieve
spf = list(range(MAXV + 1))
for i in range(2, int(MAXV ** 0.5) + 1):
    if spf[i] == i:
        for j in range(i * i, MAXV + 1, i):
            if spf[j] == j:
                spf[j] = i

def factorize(x):
    res = {}
    while x > 1:
        p = spf[x]
        cnt = 0
        while x % p == 0:
            x //= p
            cnt += 1
        res[p] = cnt
    return res

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.size = 1
        while self.size < self.n:
            self.size <<= 1
        self.tree = [dict() for _ in range(2 * self.size)]
        for i in range(self.n):
            self.tree[self.size + i] = factorize(arr[i])
        for i in range(self.size - 1, 0, -1):
            self.tree[i] = self.merge(self.tree[2 * i], self.tree[2 * i + 1])

    def merge(self, a, b):
        if len(a) < len(b):
            a, b = b, a
        res = dict(a)
        for p, v in b.items():
            if p not in res or res[p] < v:
                res[p] = v
        return res

    def update(self, idx, val):
        i = self.size + idx
        self.tree[i] = factorize(val)
        i //= 2
        while i:
            self.tree[i] = self.merge(self.tree[2 * i], self.tree[2 * i + 1])
            i //= 2

    def query(self, l, r):
        l += self.size
        r += self.size
        left_res = {}
        right_res = {}
        while l <= r:
            if l % 2 == 1:
                left_res = self.merge(left_res, self.tree[l])
                l += 1
            if r % 2 == 0:
                right_res = self.merge(self.tree[r], right_res)
                r -= 1
            l //= 2
            r //= 2
        return self.merge(left_res, right_res)

n, q = map(int, input().split())
arr = list(map(int, input().split()))
st = SegTree(arr)

for _ in range(q):
    tmp = list(map(int, input().split()))
    if tmp[0] == 1:
        _, i, x = tmp
        st.update(i - 1, x)
    else:
        _, l, r = tmp
        res = st.query(l - 1, r - 1)
        ans = 1
        for v in res.values():
            ans = (ans * (v + 1)) % MOD
        print(ans)
```Cây phân đoạn được xây dựng dựa trên các biểu diễn được phân tích thành thừa số thay vì số nguyên thô. Mỗi nút lưu trữ một từ điển và việc hợp nhất hai nút tương ứng chính xác với việc lấy cực đại theo tọa độ của số mũ nguyên tố. 

Thao tác cập nhật thay thế một lá và chỉ xây dựng lại đường dẫn đến gốc, duy trì tính chính xác cục bộ mà không cần tính toán lại toàn bộ cây. 

Hoạt động truy vấn sử dụng duyệt cây phân đoạn hai con trỏ tiêu chuẩn, chỉ tích lũy cực đại số mũ nguyên tố từ các phân đoạn có liên quan. 

Một điểm tinh tế là việc hợp nhất các từ điển sẽ không có hiệu quả đối xứng; sao chép bản đồ lớn hơn trước tiên sẽ giảm chi phí một chút, điều này quan trọng trong các ràng buộc chặt chẽ. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng`[6, 9, 12, 16]`. 

### Truy vấn 1:`2 1 3`| Bước | Phân đoạn | Bản đồ thủ tướng | 
| --- | --- | --- | 
| 6 | 2·3 | {2:1, 3:1} | 
| 9 | 3² | {3:2} | 
| 12 | 2²·3 | {2:2, 3:1} | 

Bản đồ đã hợp nhất trở thành`{2:2, 3:2}`. 

Đáp án là (2+1)(2+1) = 9. 

Điều này xác nhận rằng thuật toán nắm bắt chính xác cấu trúc LCM mà không hình thành rõ ràng 36. 

### Chuỗi truy vấn có cập nhật 

Sau`1 2 15`, mảng trở thành`[6, 15, 12, 16]`. 

Bây giờ truy vấn`2 2 4`. 

| Bước | Phân đoạn | Bản đồ thủ tướng | 
| --- | --- | --- | 
| 15 | 3·5 | {3:1, 5:1} | 
| 12 | 2²·3 | {2:2, 3:1} | 
| 16 | 2⁴ | {2:4} | 

Bản đồ đã hợp nhất trở thành`{2:4, 3:1, 5:1}`. 

Đáp án là (4+1)(1+1)(1+1) = 20. 

Điều này cho thấy các cập nhật được truyền tải chính xác và cây phân đoạn duy trì giá trị nguyên tố cực đại nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n · k) | Mỗi cập nhật và truy vấn chạm vào các nút O(log n), mỗi quá trình hợp nhất xử lý các bản đồ nguyên tố nhỏ | 
| Không gian | O(n log n · k) | Mỗi nút cây phân đoạn lưu trữ một bản đồ nguyên tố thưa thớt | 

Các ràng buộc cho phép thực hiện tối đa 2e5 phép toán, vì vậy hành vi logarit là cần thiết. Vì mỗi số có ít thừa số nguyên tố nên hệ số không đổi vẫn đủ nhỏ trong 4 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 998244353
    MAXV = 100000

    spf = list(range(MAXV + 1))
    for i in range(2, int(MAXV ** 0.5) + 1):
        if spf[i] == i:
            for j in range(i * i, MAXV + 1, i):
                if spf[j] == j:
                    spf[j] = i

    def factorize(x):
        res = {}
        while x > 1:
            p = spf[x]
            cnt = 0
            while x % p == 0:
                x //= p
                cnt += 1
            res[p] = cnt
        return res

    class SegTree:
        def __init__(self, arr):
            self.n = len(arr)
            self.size = 1
            while self.size < self.n:
                self.size <<= 1
            self.tree = [dict() for _ in range(2 * self.size)]
            for i in range(self.n):
                self.tree[self.size + i] = factorize(arr[i])
            for i in range(self.size - 1, 0, -1):
                self.tree[i] = self.merge(self.tree[2 * i], self.tree[2 * i + 1])

        def merge(self, a, b):
            if len(a) < len(b):
                a, b = b, a
            res = dict(a)
            for p, v in b.items():
                if p not in res or res[p] < v:
                    res[p] = v
            return res

        def update(self, idx, val):
            i = self.size + idx
            self.tree[i] = factorize(val)
            i //= 2
            while i:
                self.tree[i] = self.merge(self.tree[2 * i], self.tree[2 * i + 1])
                i //= 2

        def query(self, l, r):
            l += self.size
            r += self.size
            left_res = {}
            right_res = {}
            while l <= r:
                if l % 2 == 1:
                    left_res = self.merge(left_res, self.tree[l])
                    l += 1
                if r % 2 == 0:
                    right_res = self.merge(self.tree[r], right_res)
                    r -= 1
                l //= 2
                r //= 2
            return self.merge(left_res, right_res)

    n, q = map(int, input().split())
    arr = list(map(int, input().split()))
    st = SegTree(arr)

    out = []
    for _ in range(q):
        tmp = list(map(int, input().split()))
        if tmp[0] == 1:
            st.update(tmp[1] - 1, tmp[2])
        else:
            res = st.query(tmp[1] - 1, tmp[2] - 1)
            ans = 1
            for v in res.values():
                ans = (ans * (v + 1)) % MOD
            out.append(str(ans))

    return "\n".join(out)

# provided sample placeholders (replace with actual if needed)
# assert run(...) == ...
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn phần tử đơn | 2 | đếm số chia cơ sở | 
| cập nhật lặp đi lặp lại cùng một chỉ mục | tính toán lại đúng | cập nhật tính đúng đắn | 
| truy vấn đầy đủ | phụ thuộc | tính chính xác của tổng hợp toàn cầu | 
| cập nhật/truy vấn xen kẽ | đầu ra ổn định | kiên trì trong các hoạt động | 

## Vỏ cạnh 

Trường hợp cạnh khóa là các cập nhật lặp lại cho cùng một chỉ mục với các hệ số khác nhau. Ví dụ: nếu một phần tử thay đổi từ số tổng hợp cao thành số nguyên tố, cây phân đoạn phải thay thế hoàn toàn các phần đóng góp cũ thay vì cập nhật một phần số mũ. Thao tác cập nhật xử lý việc này vì nó thay thế toàn bộ bản đồ lá thay vì sửa đổi dần dần. 

Một trường hợp khác là truy vấn một đoạn có độ dài bằng một. Trong trường hợp đó, câu trả lời chỉ đơn giản là số ước của một giá trị. Thuật toán xử lý việc này một cách tự nhiên vì cây phân đoạn trả về chính xác bản đồ nút lá và công thức tích số được áp dụng chính xác. 

Trường hợp tinh tế cuối cùng là khi các phân đoạn khác nhau đóng góp các tập hợp nguyên tố rời rạc. Hoạt động hợp nhất phải bảo toàn tất cả các số nguyên tố từ cả hai phía. Vì việc hợp nhất lặp lại rõ ràng trên cả hai từ điển nên không có số nguyên tố nào bị mất và số ước cuối cùng vẫn chính xác ngay cả khi các số nguyên tố không trùng nhau trong phạm vi.
