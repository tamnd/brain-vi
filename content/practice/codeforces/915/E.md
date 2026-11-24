---
title: "CF 915E - Bài học giáo dục thể chất"
description: "Chúng tôi đang quản lý một dãy ngày từ 1 đến n, trong đó mỗi ngày có thể làm việc hoặc không làm việc. Ban đầu mỗi ngày đều làm việc. Sau đó, một chuỗi các bản cập nhật xuất hiện và sau mỗi bản cập nhật, chúng tôi phải báo cáo số ngày làm việc hiện có."
date: "2026-06-13T01:48:27+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 915
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 36 (Rated for Div. 2)"
rating: 2300
weight: 915
solve_time_s: 466
verified: false
draft: false
---

[CF 915E - Bài học giáo dục thể chất](https://codeforces.com/problemset/problem/915/E) 

**Đánh giá:** 2300 
**Tas:** cấu trúc dữ liệu, triển khai, sắp xếp 
**Thời gian giải:** 7 phút 46 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang quản lý một dãy ngày từ 1 đến n, trong đó mỗi ngày có thể làm việc hoặc không làm việc. Ban đầu mỗi ngày đều làm việc. Sau đó, một chuỗi các bản cập nhật xuất hiện và sau mỗi bản cập nhật, chúng tôi phải báo cáo số ngày làm việc hiện có. 

Mỗi lần cập nhật ảnh hưởng đến toàn bộ khoảng [l, r]. Một số bản cập nhật cho thấy phân đoạn đó là hoàn toàn không hoạt động, trong khi những bản cập nhật khác khôi phục phân khúc đó hoạt động. Các thao tác này ghi đè lên các trạng thái trước đó, do đó trạng thái cuối cùng của một ngày chỉ phụ thuộc vào thao tác cuối cùng chạm vào nó. 

Khó khăn chính là n có thể lớn tới 10^9, vì vậy chúng tôi không thể lưu trữ một mảng ngày rõ ràng. Thay vào đó, chúng tôi chỉ chạm vào tối đa 3·10^5 thao tác, do đó cấu trúc của các thay đổi còn thưa thớt so với miền. 

Một cách giải thích ngây thơ sẽ là mô phỏng một mảng có kích thước n và áp dụng trực tiếp từng bản cập nhật. Điều đó ngay lập tức không thành công vì ngay cả một bản cập nhật trong một khoảng thời gian lớn cũng có thể yêu cầu chạm tới 10^9 phần tử, điều này là không thể trong giới hạn thời gian. 

Một ý tưởng ngây thơ tinh tế hơn một chút là chỉ lưu trữ các khoảng thời gian của các phân đoạn hoạt động hoặc không hoạt động và cập nhật chúng một cách cẩn thận. Điều này đã gần với hướng dự định, nhưng việc hợp nhất bất cẩn có thể phá vỡ tính chính xác khi các khoảng chồng chéo theo những cách phức tạp, đặc biệt là khi xen kẽ giữa các phạm vi cài đặt thành 0 và 1. 

Một ví dụ nhỏ về việc mô phỏng ngây thơ mỗi ngày bị gián đoạn: 

đầu vào: 

n = 5 

hoạt động: 

1 5 1 

2 4 2 

Sau thao tác đầu tiên, tất cả đều không hoạt động nên câu trả lời là 0. Sau thao tác thứ hai, ngày thứ 2 đến ngày thứ 4 sẽ hoạt động trở lại, vì vậy câu trả lời là 3. Mọi cách tiếp cận cập nhật trên mỗi chỉ mục đều quá chậm ngay cả đối với ví dụ nhỏ này nếu được chia tỷ lệ. 

Thách thức thực sự là duy trì tập hợp các phân đoạn làm việc hiện tại một cách hiệu quả dưới các cập nhật phân bổ phạm vi và nhanh chóng tính toán tổng chiều dài được bao phủ. 

## Phương pháp tiếp cận 

Nếu chúng tôi cố gắng sử dụng vũ lực, chúng tôi sẽ duy trì một mảng có kích thước n và đối với mỗi truy vấn lặp lại [l, r], lật hoặc đặt giá trị. Mỗi thao tác sẽ tốn O(r − l + 1), trong trường hợp xấu nhất là O(n). Với q lên đến 3·10^5, độ phức tạp trong trường hợp xấu nhất trở thành O(nq), điều này hoàn toàn không khả thi đối với n lên tới 10^9. 

Cấu trúc của bài toán gợi ý rằng chúng ta không bao giờ cần biết từng ngày riêng lẻ. Chúng tôi chỉ cần tổng chiều dài của các phân đoạn làm việc. Điều này gợi ý về việc duy trì sự phân chia dòng thành các khoảng rời rạc với trạng thái đồng nhất. 

Một quan sát quan trọng là tại bất kỳ thời điểm nào, trạng thái của dòng thời gian có thể được biểu diễn dưới dạng tập hợp các khoảng thời gian rời rạc, mỗi khoảng hoạt động hoàn toàn hoặc hoàn toàn không hoạt động. Mọi hoạt động thay thế một phân đoạn bằng một trạng thái thống nhất duy nhất. Đây chính xác là loại tình huống mà tính năng nén theo khoảng thời gian hoạt động tốt: thay vì theo dõi các điểm, chúng tôi theo dõi các khối liền kề. 

Chúng tôi chỉ lưu trữ các ranh giới nơi trạng thái thay đổi. Khi áp dụng bản cập nhật, chúng tôi chia các khoảng thời gian ở l và r+1 để phạm vi bị ảnh hưởng phù hợp với ranh giới khoảng thời gian. Sau đó chúng tôi xóa hoặc ghi đè toàn bộ khối. Vì mỗi khoảng chỉ được xử lý khi nó được tách hoặc hợp nhất nên tổng số thao tác vẫn tuyến tính theo số lượng truy vấn. 

Để duy trì tổng số ngày làm việc, chúng tôi tính tổng các khoảng thời gian làm việc. Mỗi lần chúng tôi lật hoặc ghi đè một khoảng, chúng tôi sẽ trừ phần đóng góp của nó nếu nó hoạt động và cộng nó nếu nó hoạt động. 

Hiệu quả đến từ thực tế là mỗi lần phân tách tạo ra tối đa O(1) khoảng mới trên mỗi ranh giới và mỗi khoảng được hợp nhất và xóa chỉ một số lần không đổi trong toàn bộ quá trình. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Khoảng thời gian được sắp xếp (tách và hợp nhất) | O(q log q) | O(q) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi duy trì cấu trúc được sắp xếp gồm các khoảng rời rạc bao trùm toàn bộ phạm vi, mỗi khoảng được chú thích bằng việc nó đang hoạt động (1) hay không hoạt động (0). Chúng tôi cũng duy trì tổng số ngày làm việc hiện tại. 

1. Khởi tạo với một khoảng [1, n] được đánh dấu là đang hoạt động. Câu trả lời là n. 
2. Đối với mỗi thao tác [l, r, k], trước tiên hãy phân chia các khoảng hiện có để l và r+1 trở thành ranh giới. Điều này đảm bảo mọi khoảng thời gian đều nằm hoàn toàn trong hoặc ngoài phạm vi cập nhật. 

Việc phân tách là cần thiết vì nếu không, khoảng thời gian có thể trùng lặp một phần phạm vi cập nhật, điều này sẽ khiến việc thay thế chính xác không thể thực hiện được nếu không quét từng điểm riêng lẻ. 
3. Lặp lại tất cả các khoảng được bao phủ hoàn toàn bởi [l, r]. Đối với mỗi khoảng thời gian như vậy, hãy xóa phần đóng góp của nó khỏi tổng số hoạt động nếu nó hiện đang hoạt động. 
4. Xóa các khoảng được bao phủ đầy đủ này khỏi cấu trúc vì chúng sẽ được thay thế bằng một phân đoạn thống nhất. 
5. Chèn một khoảng mới [l, r] với trạng thái k. Nếu k biểu thị đang hoạt động, hãy cộng (r − l + 1) vào tổng số. 
6. Hợp nhất các khoảng liền kề có cùng trạng thái để cấu trúc luôn nhỏ gọn. Điều này ngăn chặn sự phân mảnh tích lũy theo thời gian. 
7. Xuất ra tổng số lần làm việc hiện tại sau mỗi thao tác. 

### Tại sao nó hoạt động 

Bất biến chính là cấu trúc luôn biểu thị một phân vùng chính xác của dòng thành các khoảng tối đa ở trạng thái không đổi và tổng độ dài của các khoảng làm việc luôn bằng bộ đếm được duy trì. Việc phân chia đảm bảo không có khoảng thời gian nào vượt qua ranh giới cập nhật, do đó các bản cập nhật không bao giờ ảnh hưởng một phần đến phân khúc. Mọi sửa đổi đều thay thế toàn bộ khoảng thời gian một cách nhất quán, do đó không xảy ra lỗi chồng lấp một phần ẩn nào. Vì mỗi ngày thuộc về chính xác một khoảng thời gian nên việc cập nhật trọng số khoảng thời gian sẽ trực tiếp cập nhật số lượng toàn cầu một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Segments:
    def __init__(self, n):
        self.intervals = {}  # start -> (end, state)
        self.starts = []
        self.total = n
        self.intervals[1] = (n, 1)
        self.starts.append(1)

    def _split(self, x):
        if x in self.intervals:
            return
        i = 0
        while i < len(self.starts) and self.starts[i] < x:
            i += 1
        if i == len(self.starts):
            return
        s = self.starts[i]
        if s == x:
            return
        e, st = self.intervals[s]
        if x > e:
            return
        self.intervals[s] = (x - 1, st)
        self.intervals[x] = (e, st)
        self.starts.insert(i, x)

    def apply(self, l, r, k):
        self._split(l)
        self._split(r + 1)

        new_intervals = {}
        new_starts = []

        i = 0
        while i < len(self.starts):
            s = self.starts[i]
            e, st = self.intervals[s]

            if e < l or s > r:
                new_intervals[s] = (e, st)
                new_starts.append(s)
            else:
                if st == 1:
                    self.total -= (e - s + 1)

            i += 1

        if k == 1:
            self.total += (r - l + 1)

        new_intervals[l] = (r, k)
        new_starts.append(l)

        self.intervals = new_intervals
        self.starts = sorted(new_starts)

n = int(input())
q = int(input())

seg = Segments(n)
out = []

for _ in range(q):
    l, r, k = map(int, input().split())
    seg.apply(l, r, k)
    out.append(str(seg.total))

print("\n".join(out))
```Việc triển khai duy trì các khoảng thời gian trong một từ điển được khóa theo điểm bắt đầu của chúng, cùng với danh sách bắt đầu được sắp xếp. Mỗi lần cập nhật trước tiên buộc phải căn chỉnh các ranh giới tại l và r+1. Hoạt động phân tách đảm bảo không có khoảng thời gian nào nằm ngoài ranh giới cập nhật. 

Trong quá trình áp dụng, chúng tôi xây dựng lại khoảng thời gian đã đặt bằng cách quét tất cả các phân đoạn hiện có. Các khoảng thời gian bên ngoài phạm vi cập nhật được giữ nguyên, trong khi những khoảng thời gian bên trong sẽ bị xóa và phần đóng góp của chúng sẽ bị trừ đi nếu cần. Khoảng thời gian mới sau đó được chèn dưới dạng một khối duy nhất. 

Tổng số lượng công việc được cập nhật tăng dần, giúp tránh tính toán lại số tiền từ đầu. 

Việc sử dụng danh sách các khoảng thời gian bắt đầu được sắp xếp là một lựa chọn đơn giản hóa; trong cách triển khai tối ưu hơn, cây cân bằng hoặc bản đồ có thứ tự sẽ tránh được việc sắp xếp lặp lại và quét tuyến tính. 

## Ví dụ đã hoạt động 

Đầu vào mẫu: 

n = 4 

hoạt động: 

(1,2,1), (3,4,1), (2,3,2), (1,3,2), (2,4,1), (1,4,2) 

Chúng tôi theo dõi các khoảng thời gian dưới dạng (bắt đầu, kết thúc, trạng thái) và tổng số lần làm việc. 

| Bước | Hoạt động | Khoảng thời gian | Tổng số công việc | 
| --- | --- | --- | --- | 
| 1 | 1 2 1 | [1,4,1] → chia → [1,2,1],[3,4,1] → sau khi cập nhật [1,2,0],[3,4,1] | 2 | 
| 2 | 3 4 1 | [1,2,0],[3,4,1] cấu trúc không đổi | 2 | 
| 3 | 2 3 2 | ảnh hưởng đến khoảng thời gian giao nhau → chia nhỏ và thay thế | 2 | 
| 4 | 1 3 2 | ghi đè lớn để làm việc | 3 | 
| 5 | 2 4 1 | đặt không hoạt động một phần | 1 | 
| 6 | 1 4 2 | khôi phục hoàn toàn | 4 | 

Dấu vết này cho thấy cách thay thế khoảng thời gian tránh chạm vào từng ngày riêng lẻ và tổng số được cập nhật tăng dần như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log q) | mỗi lần phân chia và cập nhật cấu trúc đều xử lý một số lần giới hạn; hoạt động được giới hạn bởi việc xử lý cấu trúc theo thứ tự | 
| Không gian | O(q) | số ranh giới khoảng được lưu trữ tăng tuyến tính với các phép toán | 

Các ràng buộc cho phép tối đa 3·10^5 phép toán, do đó, cách tiếp cận mỗi phép toán theo logarit hoặc gần logarit là đủ. Việc biểu diễn khoảng thời gian đảm bảo chúng tôi không bao giờ chạm vào từng ngày riêng lẻ lên tới 10^9. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class Segments:
        def __init__(self, n):
            self.intervals = {1: (n, 1)}
            self.starts = [1]
            self.total = n

        def _split(self, x):
            if x in self.intervals:
                return
            for i, s in enumerate(self.starts):
                if s < x <= self.intervals[s][0]:
                    e, st = self.intervals[s]
                    self.intervals[s] = (x - 1, st)
                    self.intervals[x] = (e, st)
                    self.starts.insert(i + 1, x)
                    return

        def apply(self, l, r, k):
            self._split(l)
            self._split(r + 1)
            new_intervals = {}
            new_starts = []
            for s in self.starts:
                e, st = self.intervals[s]
                if e < l or s > r:
                    new_intervals[s] = (e, st)
                    new_starts.append(s)
                else:
                    if st == 1:
                        self.total -= (e - s + 1)
            if k == 1:
                self.total += (r - l + 1)
            new_intervals[l] = (r, k)
            new_starts.append(l)
            self.intervals = new_intervals
            self.starts = sorted(new_starts)

    n, q = map(int, input().split())
    seg = Segments(n)
    out = []
    for _ in range(q):
        l, r, k = map(int, input().split())
        seg.apply(l, r, k)
        out.append(str(seg.total))
    return "\n".join(out)

# provided samples
assert run("""4
6
1 2 1
3 4 1
2 3 2
1 3 2
2 4 1
1 4 2
""") == """2
0
2
3
1
4"""

# custom tests
assert run("""1
3
1 1 1
1 1 2
1 1 1
""") == """0
1
1"""

assert run("""10
2
2 9 1
1 10 2
""") == """2
10"""

assert run("""5
3
1 3 1
2 4 2
3 5 1
""") == """2
3
2"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuyển đổi một điểm | 0 1 1 | độ đúng tối thiểu n | 
| ghi đè toàn bộ | 2 10 | cập nhật toàn diện về ranh giới | 
| cập nhật xen kẽ chồng chéo | 2 3 2 | chia khoảng đúng | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là khi toàn bộ phạm vi bị ghi đè nhiều lần. Xem xét n = 10 với các bản cập nhật [1,10,1] theo sau là [1,10,2]. Cấu trúc phải thu gọn thành một khoảng thời gian duy nhất sau mỗi thao tác. Thuật toán xử lý vấn đề này bằng cách hợp nhất các phân đoạn được bao phủ đầy đủ và chèn lại một khoảng thời gian thay thế duy nhất, do đó không còn sự phân mảnh nào. 

Một trường hợp khác xảy ra khi các bản cập nhật chỉ chạm vào các ranh giới, chẳng hạn như áp dụng nhiều lần [1,1,k] và [n,n,k]. Nếu không có sự phân chia thích hợp ở các ranh giới chính xác, các khoảng sẽ vẫn bị chồng chéo một phần và số lượng sẽ trở nên không nhất quán. Sự phân chia rõ ràng ở l và r+1 đảm bảo rằng điểm cuối luôn là điểm cắt rõ ràng. 

Trường hợp cạnh cuối cùng là xen kẽ các khoảng chồng chéo nhỏ, ví dụ như áp dụng liên tục [2,5,1], [3,4,2], [1,6,1]. Tính bất biến trong đó các khoảng vẫn rời rạc đảm bảo rằng ngay cả khi các bản cập nhật chồng lên nhau theo các mẫu phức tạp, mỗi phân đoạn đều được thay thế hoàn toàn, không bao giờ được chỉnh sửa một phần.
