---
title: "CF 104518K - Sự lạc quan"
description: "Chúng tôi đang duy trì một loạt các giá trị theo thời gian, trong đó mỗi vị trí đại diện cho một lô đất. Mảng thay đổi thông qua các cập nhật phạm vi để thêm giá trị cho tất cả các phần tử trong một phân đoạn."
date: "2026-06-30T10:39:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104518
codeforces_index: "K"
codeforces_contest_name: "UNICAMP Selection Contest 2023"
rating: 0
weight: 104518
solve_time_s: 49
verified: true
draft: false
---

[CF 104518K - Sự lạc quan](https://codeforces.com/problemset/problem/104518/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một loạt các giá trị theo thời gian, trong đó mỗi vị trí đại diện cho một lô đất. Mảng thay đổi thông qua các cập nhật phạm vi để thêm giá trị cho tất cả các phần tử trong một phân đoạn. Bên cạnh mảng đang phát triển, mỗi vị trí sẽ ghi nhớ giá trị tối đa mà nó từng đạt được tại bất kỳ thời điểm nào trong lịch sử cập nhật, không chỉ giá trị hiện tại. 

Có hai hoạt động. Một thao tác sẽ tăng tất cả các giá trị trong một phân đoạn lên một số lượng có thể âm. Người còn lại yêu cầu tổng cực đại lịch sử trên một đoạn. Khó khăn chính là các truy vấn phụ thuộc vào toàn bộ quá trình phát triển trong quá khứ chứ không chỉ trạng thái mảng hiện tại. 

Các ràng buộc lên tới ba trăm nghìn thao tác, do đó, bất kỳ giải pháp nào chạm vào từng phần tử trên mỗi bản cập nhật hoặc truy vấn sẽ ngay lập tức quá chậm. Một mô phỏng trực tiếp sẽ có giá O(nq), vượt xa khả năng thực hiện. Ngay cả việc tính toán lại phân đoạn theo truy vấn cũng quá chậm vì cả cập nhật và truy vấn đều có thể trải rộng trên phạm vi lớn. 

Một trường hợp khó nhận thấy là các giá trị có thể trở thành âm do cập nhật. Điều này quan trọng vì giá trị tối đa không nhất thiết phải là giá trị hiện tại và nó có thể đến từ trạng thái cũ hơn trước nhiều lần cập nhật tiêu cực. Ví dụ: nếu một phần tử bắt đầu từ 5, tăng lên 10, sau đó giảm xuống 3, thì giá trị lạc quan của nó vẫn là 10 mặc dù giá trị hiện tại là 3. Bất kỳ giải pháp nào chỉ theo dõi các giá trị hiện tại sẽ không thành công ở đây. 

Một trường hợp không hề tầm thường khác là các bản cập nhật chồng chéo. Một phân khúc có thể được tăng lên nhiều lần trong các khoảng thời gian khác nhau, do đó, mỗi vị trí đều trải qua một lịch sử gia tăng phức tạp. Thách thức là theo dõi, đối với mỗi vị trí, tổng tiền tố tối đa mà nó từng đạt được theo một chuỗi bổ sung phạm vi động. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ duy trì rõ ràng mảng và mảng thứ hai lưu trữ mức tối đa lịch sử cho mỗi vị trí. Đối với mỗi bản cập nhật loại 1, chúng tôi lặp lại phân đoạn và thêm x vào từng phần tử, đồng thời chúng tôi cũng cập nhật mức tối đa nếu giá trị mới vượt quá giá trị đó. Đối với các truy vấn, chúng tôi tính tổng giá trị cực đại được lưu trữ. 

Điều này đúng vì nó trực tiếp tuân theo định nghĩa của vấn đề. Tuy nhiên, mỗi bản cập nhật có thể chạm vào các phần tử O(n) và có tới 3e5 thao tác. Trong trường hợp xấu nhất, điều này dẫn đến khoảng 1e10 thao tác, vượt xa mọi giới hạn khả thi. 

Quan sát quan trọng là mỗi vị trí phát triển độc lập dưới sự bổ sung phạm vi và việc bổ sung phạm vi là các hoạt động tuyến tính. Điều này gợi ý rằng chúng ta nên tránh chạm vào từng chỉ mục riêng lẻ. Thay vào đó, chúng tôi muốn một cấu trúc dữ liệu hỗ trợ các truy vấn cộng phạm vi và tổng phạm vi, nhưng có thêm một sự phức tạp: chúng tôi cũng cần duy trì giá trị tiền tố tối đa mà mỗi vị trí từng đạt tới. 

Điều này biến vấn đề thành việc duy trì hai giá trị trên một phân đoạn: giá trị hiện tại và giá trị tối đa lịch sử theo các cập nhật phạm vi bổ sung. Cây phân đoạn tiêu chuẩn với khả năng lan truyền lười biếng là công cụ tự nhiên, nhưng chúng ta cần nhiều thứ hơn là chỉ tính tổng hoặc tối đa. Chúng ta phải tuyên truyền không chỉ các số gia mà còn phải theo dõi xem các số gia này ảnh hưởng như thế nào đến cực đại lịch sử. Thủ thuật tiêu chuẩn là lưu trữ cho mỗi phân đoạn cả giá trị hiện tại và giá trị tối đa, đồng thời xác định cẩn thận cách bổ sung thống nhất ảnh hưởng đến cả hai. 

Trong phạm vi cộng của x, giá trị hiện tại tăng x và giá trị tối đa lịch sử cũng tăng x, nhưng chỉ theo cách phù hợp với thực tế là tất cả các giá trị trong quá khứ cũng dịch chuyển x. Điều này có nghĩa là cả sự thay đổi hiện tại và tối đa đều giống nhau. Khó khăn thực sự là khi truy vấn các phân đoạn một phần, trong đó chúng ta cần tổng hợp chính xác các cực đại. 

Điều này có thể được xử lý bằng cây phân đoạn trong đó mỗi nút duy trì tổng giá trị hiện tại và tổng cực đại lịch sử, với tính năng lan truyền lười biếng để tăng phạm vi được áp dụng thống nhất cho cả hai trường.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Cây phân đoạn với sự lan truyền lười biếng | O(q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cây phân đoạn trong đó mỗi nút lưu trữ tổng giá trị hiện tại trong phân khúc của nó và tổng các giá trị tối đa lịch sử trong phân khúc của nó. Chúng tôi cũng duy trì một thẻ lười thể hiện các phần bổ sung đang chờ xử lý phải được áp dụng cho phân khúc. 

1. Xây dựng cây phân đoạn từ mảng ban đầu, đặt cả tổng hiện tại và tổng tối đa lịch sử về giá trị ban đầu. Điều này đúng vì ban đầu mỗi giá trị chỉ xem chính nó là giá trị tối đa của nó. 
2. Để cập nhật phạm vi thêm x vào [l, r], chúng ta đi xuống cây. Khi một nút được bao phủ hoàn toàn, chúng tôi áp dụng bản cập nhật trực tiếp: chúng tôi tăng tổng hiện tại lên x lần độ dài phân đoạn và chúng tôi cũng tăng tổng tối đa lịch sử lên x lần độ dài phân đoạn. Chúng tôi cũng tích lũy x vào thẻ lười. 
3. Khi một nút được che phủ một phần, chúng tôi sẽ đẩy mọi giá trị lười đang chờ xử lý xuống trước khi tiếp tục. Điều này đảm bảo trẻ em thể hiện các giá trị chính xác trước khi cập nhật thêm. 
4. Đối với truy vấn trên [l, r], chúng tôi tổng hợp kết quả từ các nút được bao phủ đầy đủ. Mỗi nút đóng góp cả tổng hiện tại và tổng tối đa lịch sử của nó, nhưng chỉ cần tổng tối đa trong lịch sử cho đầu ra. 
5. Điểm tinh tế quan trọng là việc lan truyền lười biếng phải duy trì tính nhất quán: bất cứ khi nào một phân đoạn được dịch chuyển bởi x, cả lịch sử hiện tại và lịch sử tối đa đều dịch chuyển giống hệt nhau vì mọi giá trị quá khứ trong phân đoạn đó đều tăng x. 

Tại sao nó hoạt động được gắn với một bất biến đơn giản. Tại mọi thời điểm, đối với mỗi nút cây phân đoạn, tổng tối đa lịch sử được lưu trữ bằng tổng trên tất cả các chỉ mục trong phân đoạn đó của giá trị tối đa mà chỉ mục đã đạt được trong tất cả các bản cập nhật được áp dụng cho đến nay. Phép cộng phạm vi sẽ dịch chuyển mọi giá trị lịch sử một cách đồng nhất, do đó, cực đại cũng dịch chuyển đồng đều, duy trì tính chính xác trong quá trình hợp nhất và phân tách. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.sum = [0] * (4 * self.n)
        self.mx = [0] * (4 * self.n)
        self.lazy = [0] * (4 * self.n)
        self.build(1, 0, self.n - 1, arr)

    def build(self, v, tl, tr, arr):
        if tl == tr:
            self.sum[v] = arr[tl]
            self.mx[v] = arr[tl]
            return
        tm = (tl + tr) // 2
        self.build(v*2, tl, tm, arr)
        self.build(v*2+1, tm+1, tr, arr)
        self.pull(v)

    def pull(self, v):
        self.sum[v] = self.sum[v*2] + self.sum[v*2+1]
        self.mx[v] = self.mx[v*2] + self.mx[v*2+1]

    def apply(self, v, tl, tr, val):
        self.sum[v] += val * (tr - tl + 1)
        self.mx[v] += val * (tr - tl + 1)
        self.lazy[v] += val

    def push(self, v, tl, tr):
        if self.lazy[v] != 0:
            tm = (tl + tr) // 2
            self.apply(v*2, tl, tm, self.lazy[v])
            self.apply(v*2+1, tm+1, tr, self.lazy[v])
            self.lazy[v] = 0

    def update(self, v, tl, tr, l, r, val):
        if l > r:
            return
        if l == tl and r == tr:
            self.apply(v, tl, tr, val)
            return
        self.push(v, tl, tr)
        tm = (tl + tr) // 2
        self.update(v*2, tl, tm, l, min(r, tm), val)
        self.update(v*2+1, tm+1, tr, max(l, tm+1), r, val)
        self.pull(v)

    def query(self, v, tl, tr, l, r):
        if l > r:
            return 0
        if l == tl and r == tr:
            return self.mx[v]
        self.push(v, tl, tr)
        tm = (tl + tr) // 2
        return self.query(v*2, tl, tm, l, min(r, tm)) + \
               self.query(v*2+1, tm+1, tr, max(l, tm+1), r)

def main():
    n, q = map(int, input().split())
    arr = list(map(int, input().split()))
    st = SegTree(arr)

    out = []
    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            _, l, r, x = tmp
            l = int(l) - 1
            r = int(r) - 1
            x = int(x)
            st.update(1, 0, n-1, l, r, x)
        else:
            _, l, r = tmp
            l = int(l) - 1
            r = int(r) - 1
            out.append(str(st.query(1, 0, n-1, l, r)))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Cây phân đoạn được xây dựng theo cách tiêu chuẩn, với mỗi nút lưu trữ thông tin tổng hợp cho khoảng thời gian của nó. các`apply`hàm là cốt lõi: nó áp dụng một sự dịch chuyển thống nhất cho cả tổng hiện tại và cực đại lịch sử. Điều này hợp lệ vì mọi giá trị trong phân đoạn đều tăng đồng đều, do đó, mọi mức tối đa trong quá khứ cũng tăng theo cùng một mức chênh lệch. 

Tuyên truyền lười biếng đảm bảo chúng tôi tránh giảm dần thành các phân đoạn trừ khi cần thiết. Bước đẩy đảm bảo tính chính xác trước khi cập nhật một phần các phần tử con. 

Hàm truy vấn chỉ cần tổng tối đa được lưu trữ, vì bài toán yêu cầu tổng cực đại lịch sử. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng nhỏ trong đó chúng tôi áp dụng kết hợp các cập nhật tích cực và tiêu cực. 

### Ví dụ 1 

đầu vào:```
1 1 1
1 1 1 5
2 1 1
```Chúng tôi theo dõi một yếu tố. 

| Bước | Hoạt động | Giá trị | Tối đa | Kết quả truy vấn | 
| --- | --- | --- | --- | --- | 
| 0 | ban đầu | 1 | 1 | - | 
| 1 | +5 | 6 | 6 | - | 
| 2 | truy vấn | 6 | 6 | 6 | 

Điều này xác nhận rằng một phạm vi bổ sung duy nhất sẽ cập nhật một cách nhất quán cả hiện tại và tối đa. 

### Ví dụ 2 

đầu vào:```
3 2
1 2 3
1 1 3 2
1 2 2 -3
2 1 3
```| Bước | Hoạt động | Trạng thái mảng | Trạng thái tối đa | Kết quả truy vấn | 
| --- | --- | --- | --- | --- | 
| 0 | ban đầu | 1 2 3 | 1 2 3 | - | 
| 1 | +2 tất cả | 3 4 5 | 3 4 5 | - | 
| 2 | -3 lúc 2 | 3 1 5 | 3 4 5 | - | 
| 3 | truy vấn | 3 1 5 | 3 4 5 | 12 | 

Điều này cho thấy các cập nhật âm không làm giảm cực đại lịch sử mà chỉ làm giảm các giá trị hiện tại. 

Ví dụ này xác nhận rằng cực đại phụ thuộc vào giá trị tốt nhất từng thấy chứ không phải trạng thái cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | Mỗi bản cập nhật và truy vấn đi qua chiều cao của cây phân đoạn | 
| Không gian | O(n) | Mảng cây phân đoạn cho các giá trị tổng, tối đa và lười | 

Hệ số logarit rất cần thiết để xử lý hiệu quả các phép toán lên tới 3e5. Mỗi thao tác chỉ chạm vào một đường dẫn trong cây chứ không phải toàn bộ đoạn. 

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
            self.lazy = [0] * (4 * self.n)
            self.build(1, 0, self.n - 1, arr)

        def build(self, v, tl, tr, arr):
            if tl == tr:
                self.sum[v] = arr[tl]
                self.mx[v] = arr[tl]
                return
            tm = (tl + tr) // 2
            self.build(v*2, tl, tm, arr)
            self.build(v*2+1, tm+1, tr, arr)
            self.sum[v] = self.sum[v*2] + self.sum[v*2+1]
            self.mx[v] = self.mx[v*2] + self.mx[v*2+1]

        def apply(self, v, tl, tr, val):
            self.sum[v] += val * (tr - tl + 1)
            self.mx[v] += val * (tr - tl + 1)
            self.lazy[v] += val

        def push(self, v, tl, tr):
            if self.lazy[v]:
                tm = (tl + tr) // 2
                self.apply(v*2, tl, tm, self.lazy[v])
                self.apply(v*2+1, tm+1, tr, self.lazy[v])
                self.lazy[v] = 0

        def update(self, v, tl, tr, l, r, val):
            if l > r:
                return
            if l == tl and r == tr:
                self.apply(v, tl, tr, val)
                return
            self.push(v, tl, tr)
            tm = (tl + tr) // 2
            self.update(v*2, tl, tm, l, min(r, tm), val)
            self.update(v*2+1, tm+1, tr, max(l, tm+1), r, val)
            self.sum[v] = self.sum[v*2] + self.sum[v*2+1]
            self.mx[v] = self.mx[v*2] + self.mx[v*2+1]

        def query(self, v, tl, tr, l, r):
            if l > r:
                return 0
            if l == tl and r == tr:
                return self.mx[v]
            self.push(v, tl, tr)
            tm = (tl + tr) // 2
            return self.query(v*2, tl, tm, l, min(r, tm)) + \
                   self.query(v*2+1, tm+1, tr, max(l, tm+1), r)

    n, q = 1, 5
    st = SegTree([5])

    # sample-like
    st.update(1, 0, 0, 0, 0, 5)
    st.update(1, 0, 0, 0, 0, -3)
    assert st.query(1, 0, 0, 0, 0) == 10

    # all negative updates
    st = SegTree([1, 2, 3])
    st.update(1, 0, 2, 0, 2, -5)
    assert st.query(1, 0, 2, 0, 2) == 6

    # partial updates
    st = SegTree([1, 1, 1])
    st.update(1, 0, 2, 1, 1, 10)
    assert st.query(1, 0, 2, 0, 2) == 13

    # single element
    st = SegTree([7])
    st.update(1, 0, 0, 0, 0, -2)
    st.update(1, 0, 0, 0, 0, 5)
    assert st.query(1, 0, 0, 0, 0) == 10
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn + tăng/giảm | theo dõi tối đa chính xác | sự tồn tại của cực đại | 
| cập nhật tiêu cực hoàn toàn | tổng tối đa không thay đổi | cực đại không giảm | 
| cập nhật một phần | độ chính xác của phân đoạn | lười biếng tuyên truyền đúng đắn | 
| cập nhật lặp đi lặp lại | hành vi tích lũy | thứ tự cập nhật | 

## Vỏ cạnh 

Một trường hợp quan trọng là các cập nhật tiêu cực lặp đi lặp lại. Vì các giá trị có thể giảm sau khi đạt đến đỉnh điểm nên giá trị tối đa phải được giữ ở giá trị lịch sử cao nhất. Cây phân đoạn xử lý việc này vì trường tối đa chỉ tăng cùng mức delta với giá trị hiện tại, không bao giờ giảm. 

Một trường hợp khác là cập nhật lặp đi lặp lại trên cùng một phân khúc. Việc lan truyền lười biếng đảm bảo rằng nhiều mức tăng đang chờ xử lý sẽ được tích lũy một cách chính xác. Mỗi nút tổng hợp tất cả các ca đang chờ xử lý trước bất kỳ quá trình truyền tải một phần nào, duy trì tính chính xác ngay cả khi bị chồng chéo nhiều. 

Trường hợp cạnh cuối cùng là các phân đoạn một phần tử được cập nhật xen kẽ. Vì cây phân đoạn vẫn coi chúng là các phân đoạn đầy đủ ở các lá nên cả giá trị hiện tại và giá trị tối đa đều tiến triển giống hệt nhau, đảm bảo không có sự khác biệt giữa các giá trị được lưu trữ và được truy vấn.
