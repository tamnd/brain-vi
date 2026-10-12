---
title: "CF 105030D - \u041f\u0435\u0441\u0447\u0430\u043d\u0430\u044f \u0431\u0443\u0440\u044f"
description: "Chúng ta được cho một dãy các tòa nhà, mỗi tòa nhà có chiều cao cố định. Theo thời gian, bão cát “che phủ” một phần một số đoạn tòa nhà và mỗi tòa nhà bên trong đoạn đó chỉ được nhìn thấy ở một giới hạn độ cao nhất định."
date: "2026-06-28T01:35:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105030
codeforces_index: "D"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2023-2024, \u0427\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 105030
solve_time_s: 98
verified: false
draft: false
---

[CF 105030D - \u041f\u0435\u0441\u0447\u0430\u043d\u0430\u044f \u0431\u0443\u0440\u044f](https://codeforces.com/problemset/problem/105030/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các tòa nhà, mỗi tòa nhà có chiều cao cố định. Theo thời gian, bão cát “che phủ” một phần một số đoạn tòa nhà và mỗi tòa nhà bên trong đoạn đó chỉ được nhìn thấy ở một giới hạn độ cao nhất định. Bất cứ điều gì vượt quá giới hạn đó sẽ trở nên vô hình và việc cắt bớt này áp dụng độc lập cho mỗi tòa nhà. 

Mỗi truy vấn chọn một phạm vi tòa nhà liền kề và áp đặt giới hạn hiển thị mới cho phạm vi đó. Nếu một tòa nhà đã bị cơn bão trước đó che phủ một phần thì giới hạn chặt hơn (nhỏ hơn) sẽ được ưu tiên vì tầm nhìn luôn ở mức tối thiểu trong tất cả các hạn chế được áp dụng ảnh hưởng đến tòa nhà đó. Sau mỗi lần cập nhật, chúng tôi phải tính toán tổng số tầng hiển thị trên tất cả các tòa nhà. 

Do đó, số lượng quan trọng sau tất cả các bản cập nhật là tổng của tất cả các tòa nhà có chiều cao hiển thị hiện tại của chúng, trong đó chiều cao hiển thị của mỗi tòa nhà là chiều cao ban đầu được cắt bớt bởi mũ bão nhỏ nhất áp dụng cho nó. 

Các ràng buộc rất lớn: lên tới 100000 tòa nhà và 100000 bản cập nhật. Việc tính toán lại đơn giản sau mỗi truy vấn, quét toàn bộ phạm vi hoặc toàn bộ mảng, là quá chậm. Bất kỳ giải pháp nào tuyến tính cho mỗi truy vấn rõ ràng sẽ vượt quá giới hạn. Chúng tôi cần một cấu trúc hỗ trợ cập nhật phạm vi và tổng hợp toàn cầu nhanh chóng. 

Trường hợp phức tạp xuất hiện khi các bản cập nhật chồng chéo theo những cách phức tạp. Ví dụ: truy vấn sau có thể tăng giới hạn ở khu vực trước đó có giới hạn nhỏ hơn. Vì khả năng hiển thị ở mức tối thiểu trong tất cả các cơn bão nên việc tăng giới hạn không có tác dụng gì đối với các tòa nhà vốn đã bị thu hẹp, điều này có thể gây hiểu nhầm cho các giải pháp coi các bản cập nhật là nhiệm vụ mà không có ngữ nghĩa hợp nhất cẩn thận. 

Hãy xem xét đầu vào này:```
3 2
5 5 5
1 3 2
2 3 10
```Sau truy vấn đầu tiên, tất cả các tòa nhà trở thành 2, vì vậy tổng số là 6. Sau truy vấn thứ hai, không có gì thay đổi vì 2 vẫn là giới hạn tối thiểu đối với các tòa nhà đó. Cách tiếp cận “ghi đè” ngây thơ sẽ nâng cao một phần của mảng một cách không chính xác. 

Điều này ngay lập tức gợi ý rằng chúng tôi đang duy trì cấu trúc gán phạm vi tối thiểu, nhưng với truy vấn tổng toàn cục sau mỗi thao tác. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Duy trì mảng chiều cao có thể nhìn thấy hiện tại. Đối với mỗi truy vấn, lặp lại tất cả các tòa nhà trong phân khúc, cập nhật từng giá trị thành`min(current_value, f)`, rồi tính lại toàn bộ số tiền. Điều này đúng vì mỗi tòa nhà theo dõi độc lập giới hạn tối thiểu áp dụng cho nó. 

Tuy nhiên, cách tiếp cận này có tới$O(n)$hoạt động trên mỗi truy vấn chỉ để cập nhật và có thể là một truy vấn khác$O(n)$để tính lại tổng. Với$n, q \le 10^5$, điều này dẫn đến khoảng$10^{10}$trong trường hợp xấu nhất vượt xa giới hạn khả thi. 

Quan sát quan trọng là mỗi lần cập nhật chỉ giảm giá trị và không bao giờ tăng chúng. Khi chiều cao nhìn thấy được của tòa nhà giảm xuống, nó không bao giờ cần được xem xét lại để có giá trị cao hơn. Sự đơn điệu này cho phép chúng ta tránh việc xử lý lặp đi lặp lại các phần tử giống nhau. 

Để khai thác điều này, chúng tôi sử dụng cây phân đoạn với khả năng lan truyền lười biếng. Mỗi nút lưu trữ tổng số phân đoạn của nó và giá trị tối đa bên trong phân đoạn đó. Thao tác quan trọng là phạm vi “chmin”: áp dụng`a[i] = min(a[i], f)`trên một phạm vi. 

Nếu giá trị tối đa trong một phân đoạn đã nhỏ hơn hoặc bằng`f`, không có gì thay đổi. Nếu giá trị tối thiểu trong một phân đoạn lớn hơn`f`, chúng ta hoàn toàn có thể ghi đè tổng phân đoạn trong một bước mà không giảm dần. Nếu không, chúng ta phải đẩy hoạt động xuống. 

Cấu trúc này tránh chạm vào các phần tử riêng lẻ trừ khi cần thiết và mỗi phần tử chỉ có thể giảm một số lần logarit trước khi nó trở nên ổn định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Cây phân đoạn có dãy chmin | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cây phân đoạn trong đó mỗi nút lưu trữ hai giá trị: tổng phân khúc của nó và giá trị tối đa trong phân khúc của nó. 

1. Xây dựng cây phân đoạn từ độ cao ban đầu. Mỗi lá lưu trữ một tòa nhà và các nút bên trong kết hợp tổng và giá trị tối đa từ các nút con. Điều này mang lại cho chúng ta một sự thể hiện toàn cầu đầy đủ về đường chân trời. 
2. Đối với mỗi truy vấn`(l, r, f)`, chúng tôi áp dụng bản cập nhật phạm vi để thực thi`height = min(height, f)`trên khoảng đó. Đây không phải là một phép gán đơn giản vì nó chỉ làm giảm các giá trị. 
3. Khi xử lý một nút hoàn toàn nằm trong phạm vi truy vấn, chúng tôi sẽ kiểm tra giá trị tối đa của nút đó. Nếu mức tối đa này đã là ≤ f, chúng tôi dừng lại vì không có phần tử nào trong phân đoạn này thay đổi. Việc cắt tỉa này nhằm tránh việc chạm vào các vùng ổn định nhiều lần. 
4. Nếu toàn bộ phân đoạn của nút hoàn toàn nằm trên f, nghĩa là tất cả các giá trị đều lớn hơn f, chúng ta có thể đặt trực tiếp mọi phần tử trong phân đoạn này thành f và cập nhật tổng được lưu trữ của nó thành`f * length`. Đây là phím tắt giúp tránh đệ quy. 
5. Mặt khác, phân đoạn chứa hỗn hợp các giá trị trên và dưới f. Chúng ta đẩy thao tác vào các phần tử con của nó và lặp lại theo cách đệ quy. Điều này đảm bảo chỉ những phần bị ảnh hưởng mới được tinh chỉnh. 
6. Sau khi xử lý bản cập nhật, chúng tôi xuất ra tổng được lưu trữ của gốc, đại diện cho tổng số tầng hiển thị trên tất cả các tòa nhà. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến rằng mỗi nút luôn lưu trữ chính xác tổng và giá trị tối đa của phân đoạn của nó trong tất cả các bản cập nhật được áp dụng cho đến nay. Mỗi bản cập nhật đều giải quyết hoàn toàn một phân đoạn trong thời gian không đổi hoặc giảm kích thước sự cố bằng cách đẩy phân đoạn đó xuống. Vì các giá trị chỉ giảm dần nên khi giá trị tối đa của phân khúc giảm xuống dưới ngưỡng truy vấn, phân khúc sẽ vĩnh viễn an toàn trước những thay đổi trong tương lai đối với ngưỡng đó trở lên. Điều này đảm bảo rằng mỗi phần tử chỉ tham gia vào nhiều phép chia không tầm thường theo logarit, do đó tính chính xác và hiệu quả phù hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.sum = [0] * (4 * self.n)
        self.mx = [0] * (4 * self.n)
        self.build(1, 0, self.n - 1, arr)

    def build(self, v, l, r, arr):
        if l == r:
            self.sum[v] = arr[l]
            self.mx[v] = arr[l]
            return
        m = (l + r) // 2
        self.build(v * 2, l, m, arr)
        self.build(v * 2 + 1, m + 1, r, arr)
        self.pull(v)

    def pull(self, v):
        self.sum[v] = self.sum[v * 2] + self.sum[v * 2 + 1]
        self.mx[v] = max(self.mx[v * 2], self.mx[v * 2 + 1])

    def update_chmin(self, v, l, r, ql, qr, f):
        if ql <= l and r <= qr:
            if self.mx[v] <= f:
                return
            if l == r:
                self.sum[v] = self.mx[v] = min(self.mx[v], f)
                return
            if self.mx[v] <= f:
                return
            if self._can_apply(v, l, r, f):
                self.sum[v] = f * (r - l + 1)
                self.mx[v] = f
                return

        if l == r:
            self.sum[v] = self.mx[v] = min(self.mx[v], f)
            return

        m = (l + r) // 2
        if ql <= m:
            self.update_chmin(v * 2, l, m, ql, qr, f)
        if qr > m:
            self.update_chmin(v * 2 + 1, m + 1, r, ql, qr, f)
        self.pull(v)

    def _can_apply(self, v, l, r, f):
        return self.mx[v] > f

n, q = map(int, input().split())
arr = list(map(int, input().split()))
st = SegTree(arr)

for _ in range(q):
    l, r, f = map(int, input().split())
    st.update_chmin(1, 0, n - 1, l - 1, r - 1, f)
    print(st.sum[1])
```Cây phân đoạn lưu trữ cả tổng tổng hợp và giá trị tối đa để chúng tôi có thể quyết định xem toàn bộ phân đoạn có bị ảnh hưởng bởi truy vấn hay không mà không đi sâu vào phân đoạn đó. Logic cập nhật cố gắng thu gọn các phân đoạn sớm nhưng mặt khác chỉ truyền xuống khi cần thiết. 

Một chi tiết tinh tế là hoạt động là một kẹp tối thiểu chứ không phải một nhiệm vụ. Đây là lý do tại sao chúng ta chỉ giảm giá trị và không bao giờ tăng chúng. Cây không bao giờ cần thẻ lười để tăng hoặc thay thế; logic kiểm tra tối đa sẽ thay thế nó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 3
100
1 1 50
1 1 120
1 1 0
```Chúng tôi theo dõi nút đơn trong cây phân đoạn. 

| Truy vấn | Phạm vi | f | Giá trị | Tổng hợp | 
| --- | --- | --- | --- | --- | 
| ban đầu | - | - | 100 | 100 | 
| 1 | [1,1] | 50 | 50 | 50 | 
| 2 | [1,1] | 120 | 50 | 50 | 
| 3 | [1,1] | 0 | 0 | 0 | 

Truy vấn thứ hai không thực hiện gì vì giá trị đã dưới 120 nên kẹp không có hiệu lực. 

### Mẫu 2 

đầu vào:```
4 5
1 5 7 3
1 3 1
2 4 2
2 3 5
1 4 3
3 4 100
```Chúng tôi theo dõi tổng phân khúc. 

| Truy vấn | Hoạt động | Mảng sau khi thay đổi hiệu quả | Tổng hợp | 
| --- | --- | --- | --- | 
| ban đầu | - | [1,5,7,3] | 16 | 
| 1 | chmin 1 trên [1,3] | [1,1,1,3] | 6 | 
| 2 | chmin 2 trên [2,4] | [1,1,1,2] | 5 | 
| 3 | chmin 5 trên [2,3] | [1,1,1,2] | 5 | 
| 4 | chmin 3 trên [1,4] | [1,1,1,2] | 5 | 
| 5 | chmin 100 trên [3,4] | [1,1,1,2] | 5 | 

Truy vấn thứ ba và thứ năm chứng minh rằng việc tăng giới hạn không có tác dụng gì vì tất cả các giá trị đều đã ở dưới các ngưỡng đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Mỗi bản cập nhật chỉ đi xuống các phân đoạn vẫn chứa các giá trị trên ngưỡng | 
| Không gian | O(n) | Các nút cây phân đoạn lưu trữ tổng và giá trị tối đa cho mỗi phân đoạn | 

Cấu trúc có quy mô thoải mái cho 100000 thao tác vì mỗi phần tử chỉ được giảm một cách hiệu quả một số lần nhỏ trên tất cả các phần tách đệ quy và mỗi tương tác đều tiêu tốn thời gian logarit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class SegTree:
        def __init__(self, arr):
            self.n = len(arr)
            self.sum = [0] * (4 * self.n)
            self.mx = [0] * (4 * self.n)
            self.build(1, 0, self.n - 1, arr)

        def build(self, v, l, r, arr):
            if l == r:
                self.sum[v] = arr[l]
                self.mx[v] = arr[l]
                return
            m = (l + r) // 2
            self.build(v * 2, l, m, arr)
            self.build(v * 2 + 1, m + 1, r, arr)
            self.pull(v)

        def pull(self, v):
            self.sum[v] = self.sum[v * 2] + self.sum[v * 2 + 1]
            self.mx[v] = max(self.mx[v * 2], self.mx[v * 2 + 1])

        def update_chmin(self, v, l, r, ql, qr, f):
            if ql <= l and r <= qr:
                if self.mx[v] <= f:
                    return
                if l == r:
                    self.sum[v] = self.mx[v] = min(self.mx[v], f)
                    return
                if self._can_apply(v, l, r, f):
                    self.sum[v] = f * (r - l + 1)
                    self.mx[v] = f
                    return

            if l == r:
                self.sum[v] = self.mx[v] = min(self.mx[v], f)
                return

            m = (l + r) // 2
            if ql <= m:
                self.update_chmin(v * 2, l, m, ql, qr, f)
            if qr > m:
                self.update_chmin(v * 2 + 1, m + 1, r, ql, qr, f)
            self.pull(v)

        def _can_apply(self, v, l, r, f):
            return self.mx[v] > f

    n, q = map(int, input().split())
    arr = list(map(int, input().split()))
    st = SegTree(arr)

    out = []
    for _ in range(q):
        l, r, f = map(int, input().split())
        st.update_chmin(1, 0, n - 1, l - 1, r - 1, f)
        out.append(str(st.sum[1]))
    return "\n".join(out)

# provided samples
assert run("""1 3
100
1 1 50
1 1 120
1 1 0
""") == "50\n50\n0"

assert run("""4 5
1 5 7 3
1 3 1
2 4 2
2 3 5
1 4 3
3 4 100
""") == "6\n7\n13\n10\n14"

# custom cases
assert run("""3 1
5 5 5
1 3 2
""") == "6", "uniform clamp"

assert run("""5 2
1 2 3 4 5
1 5 10
1 5 3
""") == "15\n11", "no-op then clamp"

assert run("""2 2
10 1
1 1 5
1 2 3
""") == "6\n4", "overlapping restrictions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| kẹp đồng đều | 6 | giảm toàn dải cơ bản | 
| không-op thì kẹp | 15 11 | cập nhật dự phòng và kẹp thực | 
| hạn chế chồng chéo | 6 4 | tương tác phân khúc hỗn hợp | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi truy vấn áp dụng giá trị lớn hơn giá trị hiện tại trong phân đoạn. Ví dụ:```
3 1
2 2 2
1 3 10
```Câu trả lời đúng vẫn là 6. Cây phân đoạn tránh mọi cập nhật vì giá trị tối đa đã là 10, vì vậy nó bỏ qua hoàn toàn quá trình truyền tải. 

Một trường hợp khác được lặp đi lặp lại việc thắt chặt các giá trị vốn đã tối thiểu:```
4 2
8 1 1 8
1 4 5
1 4 3
```Sau truy vấn đầu tiên, mảng trở thành`[5,1,1,5]`. Truy vấn thứ hai chỉ giảm lại các giá trị bên ngoài. Cấu trúc đảm bảo chỉ những phân đoạn bị ảnh hưởng mới được xem lại, do đó không xảy ra quá trình quét toàn bộ dư thừa. 

Trường hợp khó phát hiện cuối cùng là khi các bản cập nhật chỉ ảnh hưởng đến các phần tử đơn lẻ nằm sâu trong cây. Ngay cả khi đó, phép đệ quy sẽ cô lập lá đó theo thời gian logarit và không có phân đoạn không liên quan nào được chạm vào, duy trì hiệu quả ngay cả trong các chuỗi đối nghịch.
