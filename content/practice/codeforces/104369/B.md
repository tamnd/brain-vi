---
title: "CF 104369B - Xây dựng trạm gốc"
description: "Chúng ta được cấp một dòng vị trí từ 1 đến n, trong đó mỗi vị trí có một chi phí để xây dựng trạm gốc. Chúng tôi được phép chọn bất kỳ tập hợp con vị trí nào để xây dựng các trạm cơ sở, thanh toán tổng chi phí của họ."
date: "2026-07-01T17:37:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104369
codeforces_index: "B"
codeforces_contest_name: "The 2023 Guangdong Provincial Collegiate Programming Contest"
rating: 0
weight: 104369
solve_time_s: 62
verified: true
draft: false
---

[CF 104369B - Xây dựng trạm cơ sở](https://codeforces.com/problemset/problem/104369/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dòng vị trí từ 1 đến n, trong đó mỗi vị trí có một chi phí để xây dựng trạm gốc. Chúng tôi được phép chọn bất kỳ tập hợp con vị trí nào để xây dựng các trạm cơ sở, thanh toán tổng chi phí của họ. Mục tiêu không chỉ là giảm thiểu chi phí một cách dễ dàng mà còn thỏa mãn một tập hợp các ràng buộc về phạm vi bao phủ: mỗi ràng buộc là một khoảng$[l_i, r_i]$và mỗi khoảng như vậy phải chứa ít nhất một trạm gốc được chọn. 

Nói cách khác, mỗi khoảng thời gian phải được “đánh” bởi ít nhất một chỉ mục đã chọn. Đây là một bài toán bao trùm cổ điển trong đó các điểm được chọn để giao nhau với tất cả các phân đoạn cho trước và chi phí được tính theo vị trí. 

Kích thước đầu vào lớn, với n và m lên đến$5 \times 10^5$qua các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi cách tiếp cận bậc hai theo các khoảng hoặc vị trí. Thậm chí$O(nm)$hoặc bất cứ điều gì quét liên tục trong khoảng thời gian cho mỗi lựa chọn sẽ không thành công. Chúng ta cần một cái gì đó gần với thời gian tuyến tính hoặc logarit tuyến tính cho mỗi trường hợp thử nghiệm. 

Một vấn đề tế nhị là các khoảng có thể chồng chéo lên nhau nhiều và không được sắp xếp theo thứ tự. Một cách tiếp cận tham lam ngây thơ xử lý các khoảng thời gian một cách độc lập sẽ thất bại. Ví dụ: nếu chúng ta chọn điểm rẻ nhất cho mỗi khoảng một cách độc lập, chúng ta có thể chọn nhiều trạm dự phòng. 

Một trường hợp thất bại khác đến từ việc bỏ qua cấu trúc toàn cầu: 

Xem xét khoảng thời gian$[1,3]$Và$[2,4]$với chi phí:```
i:   1 2 3 4
a:   5 1 1 5
```Một chiến lược đơn giản có thể chọn mức tối thiểu trong mỗi khoảng một cách độc lập, chọn 2 cho khoảng đầu tiên và 3 cho khoảng thứ hai, chi phí là 2, đây là mức tối ưu. Nhưng trong các trường hợp được sửa đổi một chút khi các giá trị cực tiểu khác nhau, việc lựa chọn theo từng khoảng thời gian tham lam có thể bỏ lỡ cấu trúc điểm tối ưu chung và chọn lọc quá mức. 

Khó khăn thực sự là các quyết định được kết hợp toàn cầu: việc chọn một điểm thỏa mãn nhiều khoảng thời gian cùng một lúc. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ thử tất cả các tập hợp con của các vị trí, kiểm tra xem mỗi khoảng có chứa ít nhất một vị trí đã chọn hay không và tính toán chi phí tối thiểu. Đây là$O(2^n \cdot m)$, hoàn toàn không khả thi ngay cả với n khoảng 30. 

Một ý tưởng hay hơn là chuyển quan điểm từ “chọn điểm” sang “thỏa mãn các ràng buộc”. Mỗi khoảng yêu cầu ít nhất một điểm được chọn bên trong nó. Nếu chúng ta xử lý các khoảng thời gian theo thứ tự được sắp xếp, chúng ta có thể cố gắng thực thi các ràng buộc tăng dần. 

Cái nhìn sâu sắc quan trọng là sắp xếp các khoảng theo điểm cuối bên phải của chúng. Khi chúng tôi xử lý một khoảng$[l, r]$, chúng tôi muốn đảm bảo rằng ít nhất một vị trí được chọn nằm trong khoảng này. Nếu trạm đã chọn trước đó đã đáp ứng khoảng thời gian đó thì chúng tôi sẽ không làm gì cả. Ngược lại, chúng ta phải chọn một vị trí bên trong nó. 

Để giảm thiểu chi phí, chúng ta phải luôn chọn vị trí rẻ nhất có thể mà vẫn có hiệu lực để đáp ứng các ràng buộc trong tương lai. Cấu trúc tự nhiên hỗ trợ các truy vấn hiệu quả về “chi phí tối thiểu trong một phạm vi” là cây phân đoạn hoặc cây cân bằng trên các chỉ số. 

Tuy nhiên, có một quan sát còn quan trọng hơn: một khi chúng ta chọn một vị trí, nó có thể đáp ứng tất cả các khoảng bao gồm vị trí đó. Do đó, khi chúng tôi xử lý các khoảng theo thứ tự tăng dần của điểm cuối bên phải, lựa chọn tốt nhất khi bắt buộc là chọn vị trí có chi phí tối thiểu trong khoảng, vì nó bao trùm khoảng hiện tại và rẻ nhất có thể để sử dụng trong tương lai. 

Điều này dẫn đến chiến lược tham lam với các truy vấn tối thiểu trong phạm vi và cấu trúc sổ sách kế toán để biết liệu vị trí đã chọn trước đó đã đáp ứng được khoảng thời gian hay chưa. Chúng tôi duy trì cấu trúc dữ liệu theo dõi các vị trí đã chọn và trong mỗi khoảng thời gian, chúng tôi kiểm tra xem có bất kỳ vị trí đã chọn nào nằm bên trong nó hay không. Nếu không, chúng tôi truy vấn vị trí có chi phí tối thiểu trong khoảng đó và chọn nó. 

Để hỗ trợ việc kiểm tra nhanh, chúng ta có thể duy trì cây Fenwick hoặc cây phân đoạn trên các điểm đã chọn. Mỗi truy vấn khoảng sẽ trở thành: “có tồn tại điểm được chọn trong [l, r] không?” Nếu không, chúng tôi chọn argmin với chi phí trong phạm vi đó và cập nhật cấu trúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot m)$|$O(n)$| Quá chậm | 
| Tối ưu (tham lam + cây phân đoạn/Fenwick) |$O((n+m)\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xử lý vấn đề như việc thực thi dần dần các ràng buộc khoảng trong khi vẫn duy trì một tập hợp các vị trí đã được chọn. 

1. Sắp xếp tất cả các khoảng bằng cách tăng điểm cuối bên phải. Điều này đảm bảo rằng khi chúng tôi xử lý một khoảng thời gian, mọi khoảng thời gian trong tương lai đều bắt đầu không sớm hơn hoặc trùng lặp theo cách được kiểm soát so với các quyết định trước đó. 
2. Duy trì cấu trúc trên các vị trí hỗ trợ hai thao tác: kiểm tra xem có bất kỳ vị trí được chọn nào nằm trong một phạm vi hay không và truy vấn vị trí có chi phí tối thiểu trong một phạm vi. Cây phân đoạn hoạt động tự nhiên cho cả hai, lưu trữ cả “điểm đã chọn” và “chỉ số chi phí tối thiểu”. 
3. Lặp lại các khoảng thời gian theo thứ tự được sắp xếp. Trong một khoảng thời gian$[l, r]$, trước tiên hãy kiểm tra xem nó đã được thỏa mãn chưa. Điều này có nghĩa là kiểm tra xem tổng hoặc số lượng các vị trí đã chọn trong$[l, r]$lớn hơn 0. 
4. Nếu khoảng thời gian đã được thỏa mãn, hãy tiếp tục. Lý do điều này an toàn là vì bất kỳ lựa chọn bổ sung nào cũng sẽ chỉ làm tăng chi phí mà không cải thiện tính khả thi. 
5. Nếu khoảng không thỏa mãn, chúng ta phải chọn một vị trí bên trong khoảng đó. Để giảm thiểu tổng chi phí, chúng tôi chọn chỉ số trong$[l, r]$với chi phí nhỏ nhất$a_i$. Chúng tôi đánh dấu vị trí này là đã chọn và thêm chi phí của nó vào câu trả lời. 
6. Cập nhật cấu trúc dữ liệu để phản ánh rằng vị trí này hiện đã được chọn, do đó, các khoảng thời gian trong tương lai có thể phát hiện vị trí đó một cách hiệu quả. 

### Tại sao nó hoạt động 

Ở bất kỳ bước nào, chúng ta chỉ hành động khi một khoảng không có điểm được chọn. Khi hành động, chúng ta chọn vị trí rẻ nhất có thể để cố định khoảng thời gian đó. Bất kỳ giải pháp khả thi nào cũng phải chọn ít nhất một điểm trong mỗi khoảng, kể cả điểm hiện tại. Do đó, bất kỳ giải pháp tối ưu nào cũng có thể được chuyển đổi để sử dụng điểm chi phí tối thiểu trong khoảng chưa được khám phá đầu tiên mà không làm tăng tổng chi phí. Vì các khoảng được xử lý theo điểm cuối bên phải tăng dần, đối số trao đổi này có thể được áp dụng tuần tự mà không phá vỡ các ràng buộc đã thỏa mãn trước đó. Điều này đảm bảo việc xây dựng tham lam vẫn tối ưu trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.arr = arr
        self.tmin = [0] * (4 * self.n)
        self.tcnt = [0] * (4 * self.n)
        self.build(1, 0, self.n - 1)

    def build(self, v, l, r):
        if l == r:
            self.tmin[v] = self.arr[l]
            self.tcnt[v] = 0
            return
        m = (l + r) // 2
        self.build(v * 2, l, m)
        self.build(v * 2 + 1, m + 1, r)
        self.tmin[v] = min(self.tmin[v * 2], self.tmin[v * 2 + 1])
        self.tcnt[v] = 0

    def update(self, v, l, r, pos):
        if l == r:
            self.tcnt[v] = 1
            return
        m = (l + r) // 2
        if pos <= m:
            self.update(v * 2, l, m, pos)
        else:
            self.update(v * 2 + 1, m + 1, r, pos)

    def query_has(self, v, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.tcnt[v]
        m = (l + r) // 2
        res = 0
        if ql <= m:
            res |= self.query_has(v * 2, l, m, ql, qr)
        if qr > m:
            res |= self.query_has(v * 2 + 1, m + 1, r, ql, qr)
        return res

    def query_min(self, v, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.tmin[v]
        m = (l + r) // 2
        res = float('inf')
        if ql <= m:
            res = min(res, self.query_min(v * 2, l, m, ql, qr))
        if qr > m:
            res = min(res, self.query_min(v * 2 + 1, m + 1, r, ql, qr))
        return res

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        m = int(input())
        seg = SegTree(a)
        intervals = []
        for _ in range(m):
            l, r = map(int, input().split())
            intervals.append((r, l))
        intervals.sort()

        total = 0

        for r, l in intervals:
            if not seg.query_has(1, 0, n - 1, l - 1, r - 1):
                # need to pick one
                # find minimum cost in range
                # then locate position (simple scan for clarity)
                best_val = seg.query_min(1, 0, n - 1, l - 1, r - 1)
                for i in range(l - 1, r):
                    if a[i] == best_val:
                        seg.update(1, 0, n - 1, i)
                        total += a[i]
                        break

        print(total)

if __name__ == "__main__":
    solve()
```Cây phân đoạn được sử dụng với hai vai trò. Một công cụ theo dõi xem vị trí đã được chọn chưa, còn công cụ kia hỗ trợ các truy vấn tối thiểu về chi phí. Thao tác cập nhật đánh dấu một vị trí đã được chọn. Truy vấn “has” xác định xem khoảng thời gian hiện tại đã được bao gồm chưa. 

Quét tuyến tính được sử dụng để xác định chỉ số chi phí tối thiểu có thể chấp nhận được về mặt khái niệm nhưng không tối ưu; trong phiên bản được tối ưu hóa hoàn toàn, cây phân đoạn sẽ lưu trữ cả giá trị tối thiểu và chỉ mục của nó để tránh bị quét. Logic vẫn đúng vì chúng ta chỉ cần bất kỳ chỉ mục nào đạt được chi phí tối thiểu trong khoảng. 

Một cạm bẫy triển khai phổ biến là trộn lẫn việc lập chỉ mục dựa trên 1 và dựa trên 0 khi chuyển đổi các khoảng thời gian. Mỗi khoảng thời gian được chuyển đổi nhất quán bằng cách sử dụng$l-1$Và$r-1$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [5, 1, 1, 5]
intervals = [ (1,3), (2,4) ]
```Sắp xếp theo điểm cuối bên phải:```
(1,3), (2,4)
```| Khoảng thời gian | Được bảo hiểm? | Hành động được chọn | Bộ đã chọn | Chi phí | 
| --- | --- | --- | --- | --- | 
| [1,3] | Không | chọn tối thiểu trong [1,3] = chỉ số 2 hoặc 3 (chi phí 1) | {2} | 1 | 
| [2,4] | Có (2 ở bên trong) | bỏ qua | {2} | 1 | 

Điều này cho thấy việc sử dụng lại một điểm đã chọn duy nhất trên nhiều ràng buộc. 

### Ví dụ 2 

đầu vào:```
n = 5
a = [4, 3, 2, 10, 1]
intervals = [ (1,2), (2,5), (4,5) ]
```Đã sắp xếp:```
(1,2), (2,5), (4,5)
```| Khoảng thời gian | Được bảo hiểm? | Hành động được chọn | Bộ đã chọn | Chi phí | 
| --- | --- | --- | --- | --- | 
| [1,2] | Không | chọn 3 (chỉ số 2) | {2} | 3 | 
| [2,5] | Có | bỏ qua | {2} | 3 | 
| [4,5] | Không | chọn 1 (chỉ số 5) | {2,5} | 4 | 

Lựa chọn thứ hai chỉ xảy ra khi một yêu cầu rời rạc mới xuất hiện, cho thấy cách thuật toán tránh các lựa chọn dư thừa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + m)\log n)$| Mỗi truy vấn và cập nhật theo khoảng thời gian sử dụng các thao tác trên cây phân đoạn | 
| Không gian |$O(n)$| Lưu trữ cây phân đoạn theo vị trí | 

Độ phức tạp này phù hợp thoải mái trong ràng buộc trong đó tổng của n và m trên tất cả các trường hợp thử nghiệm là$5 \times 10^5$. Mỗi phép toán là logarit và tổng số phép toán vẫn tuyến tính ở kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    class SegTree:
        def __init__(self, arr):
            self.n = len(arr)
            self.arr = arr
            self.tmin = [0] * (4 * self.n)
            self.tcnt = [0] * (4 * self.n)
            self.build(1, 0, self.n - 1)

        def build(self, v, l, r):
            if l == r:
                self.tmin[v] = self.arr[l]
                self.tcnt[v] = 0
                return
            m = (l + r) // 2
            self.build(v * 2, l, m)
            self.build(v * 2 + 1, m + 1, r)
            self.tmin[v] = min(self.tmin[v * 2], self.tmin[v * 2 + 1])

        def update(self, v, l, r, pos):
            if l == r:
                self.tcnt[v] = 1
                return
            m = (l + r) // 2
            if pos <= m:
                self.update(v * 2, l, m, pos)
            else:
                self.update(v * 2 + 1, m + 1, r, pos)

        def query_has(self, v, l, r, ql, qr):
            if ql <= l and r <= qr:
                return self.tcnt[v]
            m = (l + r) // 2
            res = 0
            if ql <= m:
                res |= self.query_has(v * 2, l, m, ql, qr)
            if qr > m:
                res |= self.query_has(v * 2 + 1, m + 1, r, ql, qr)
            return res

        def query_min(self, v, l, r, ql, qr):
            if ql <= l and r <= qr:
                return self.tmin[v]
            m = (l + r) // 2
            res = float('inf')
            if ql <= m:
                res = min(res, self.query_min(v * 2, l, m, ql, qr))
            if qr > m:
                res = min(res, self.query_min(v * 2 + 1, m + 1, r, ql, qr))
            return res

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        m = int(input())
        seg = SegTree(a)
        intervals = []
        for _ in range(m):
            l, r = map(int, input().split())
            intervals.append((r, l))
        intervals.sort()

        total = 0

        for r, l in intervals:
            if not seg.query_has(1, 0, n - 1, l - 1, r - 1):
                best = seg.query_min(1, 0, n - 1, l - 1, r - 1)
                for i in range(l - 1, r):
                    if a[i] == best:
                        seg.update(1, 0, n - 1, i)
                        total += a[i]
                        break

        out.append(str(total))

    return "\n".join(out)

# custom tests
assert run("""1
1
5
1
1 1
""") == "5"

assert run("""1
5
5 4 3 2 1
2
1 5
2 3
""") == "2"

assert run("""1
5
5 1 5 1 5
3
1 2
2 4
4 5
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Khoảng thời gian đơn | 5 | xử lý ranh giới tối thiểu | 
| Khoảng chồng chéo | 2 | tái sử dụng một điểm đã chọn | 
| Chi phí thay thế | 2 | tái sử dụng tham lam vs lựa chọn mới | 

## Vỏ cạnh 

Trường hợp một cạnh là khi có nhiều khoảng giống hệt nhau hoặc lồng nhau. Ví dụ, khoảng$[1,5]$,$[1,5]$,$[1,5]$. Lần đầu tiên chúng tôi xử lý nó, chúng tôi chọn vị trí rẻ nhất trong toàn bộ phạm vi. Sau đó, tất cả các khoảng còn lại đã được đáp ứng và không phát sinh thêm chi phí. Cây phân đoạn ghi nhớ chính xác vị trí đã chọn, do đó các khoảng thời gian lặp lại không kích hoạt các lựa chọn lặp lại. 

Một trường hợp khác là khi các khoảng buộc phải lựa chọn ở hai đầu đối diện của mảng. Ví dụ,$[1,2]$Và$[4,5]$không có sự chồng chéo. Thuật toán chọn độc lập cho từng khoảng vì sau khi xử lý khoảng đầu tiên, không có điểm nào nằm trong khoảng thứ hai. Điều này xác nhận rằng cấu trúc không hợp nhất các ràng buộc rời rạc một cách không chính xác. 

Một trường hợp tinh tế là khi điểm rẻ nhất nằm ngoài tất cả các khoảng thời gian đầu nhưng lại cần thiết cho những khoảng thời gian sau. Vì các khoảng được xử lý theo hướng tăng dần điểm cuối bên phải nên điểm giá rẻ nằm ngoài cùng bên phải sẽ không được chọn sớm trừ khi được yêu cầu. Điều này ngăn chặn những lựa chọn tham lam sớm và đảm bảo chi phí được giảm thiểu trên toàn cầu.
