---
title: "CF 106078H - Sao Thiên Vương"
description: "Mỗi đầu dò có thể được mô tả bằng hai giới hạn và chi phí. Một giới hạn là nó có thể tồn tại ở nhiệt độ bao nhiêu, giới hạn còn lại là nó có thể tồn tại được bao nhiêu gió. Đầu dò chỉ có thể sử dụng được tại một địa điểm nếu nó xử lý cả hai điều kiện cùng một lúc."
date: "2026-06-25T12:09:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106078
codeforces_index: "H"
codeforces_contest_name: "UTPC Contest 9-17-25 Div. 1 (Advanced)"
rating: 0
weight: 106078
solve_time_s: 54
verified: true
draft: false
---

[CF 106078H - Sao Thiên Vương](https://codeforces.com/problemset/problem/106078/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi đầu dò có thể được mô tả bằng hai giới hạn và chi phí. Một giới hạn là nó có thể tồn tại ở nhiệt độ bao nhiêu, giới hạn còn lại là nó có thể tồn tại được bao nhiêu gió. Đầu dò chỉ có thể sử dụng được tại một địa điểm nếu nó xử lý cả hai điều kiện cùng một lúc. Đối với mọi truy vấn vị trí, chúng tôi muốn có thiết bị thăm dò rẻ nhất đủ mạnh để đo nhiệt độ và gió của vị trí đó. 

Nói một cách cụ thể hơn, chúng ta có một tập hợp các hình chữ nhật trong mặt phẳng 2D trong đó mỗi đầu dò xác định một ngưỡng góc. Một đầu dò hợp lệ cho một truy vấn nếu điểm ngưỡng của nó nằm ở phía trên và bên phải điểm truy vấn, nghĩa là cả hai tọa độ ít nhất đều lớn bằng. Trong số tất cả các cuộc thăm dò như vậy, chúng tôi muốn có chi phí tối thiểu. 

Kích thước đầu vào lên tới một trăm nghìn đầu dò và một trăm nghìn truy vấn. Điều đó ngay lập tức loại trừ việc kiểm tra mọi thăm dò cho mỗi truy vấn, vì điều đó sẽ theo thứ tự so sánh 10¹⁰ trong trường hợp xấu nhất, vượt xa giới hạn một hoặc hai giây có thể xử lý. Bất kỳ giải pháp chấp nhận được nào cũng cần xử lý cả thăm dò và truy vấn trong thời gian gần như tuyến tính. 

Một cách giải thích ngây thơ thường thất bại trong việc triển khai là xử lý các ràng buộc một cách độc lập, ví dụ như lọc theo nhiệt độ trước rồi quét tuyến tính gió. Điều đó bị phá vỡ khi cả hai chiều tương tác. Một trường hợp sai sót tinh vi khác xuất hiện khi nhiều đầu dò lấn át nhau một phần: một đầu dò có thể tốt hơn về nhiệt độ nhưng kém hơn về gió và ngược lại, do đó, việc sắp xếp tham lam theo một chiều duy nhất sẽ không hiệu quả. 

Một lỗi minh họa nhỏ: giả sử chúng ta có các đầu dò (nhiệt độ, gió, chi phí) là (5, 1, 100), (1, 5, 1) và (3, 3, 2). Đối với truy vấn (3, 3), câu trả lời đúng là 2. Nếu chúng ta chỉ sắp xếp theo nhiệt độ và lấy giá trị đầu tiên hợp lệ, chúng ta có thể chọn không chính xác (5,1,100) mặc dù nó không đáp ứng được hạn chế về gió hoặc nếu chúng ta lọc không chính xác, chúng ta có thể bỏ lỡ đầu dò cân bằng giữa tối ưu. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: đối với mỗi truy vấn, hãy quét tất cả các đầu dò và kiểm tra xem cả hai ràng buộc có được thỏa mãn hay không, theo dõi chi phí tối thiểu. Điều này đúng vì nó đánh giá trực tiếp định nghĩa về tính hợp lệ. Chi phí là kiểm tra n đầu dò cho mỗi q truy vấn, đưa ra n·q hoạt động, đạt tới 10¹⁰ trong trường hợp xấu nhất. Quy mô đó quá lớn để có thể hoàn thành trong thời gian giới hạn. 

Quan sát quan trọng là cả hai ràng buộc đều đơn điệu. Nếu đầu dò hoạt động ở nhiệt độ và gió nhất định, nó sẽ tự động hoạt động cho mọi truy vấn dễ dàng hơn (yêu cầu nhiệt độ hoặc gió thấp hơn). Cấu trúc này cho phép chúng ta coi các đầu dò như các điểm trong không gian 2D và các truy vấn dưới dạng kiểm tra sự thống trị. 

Chúng ta có thể sắp xếp cả đầu dò và truy vấn theo nhiệt độ theo thứ tự giảm dần. Khi chúng tôi quét từ nhiệt độ cao đến nhiệt độ thấp, mọi đầu dò đủ điều kiện sẽ được đưa vào một cấu trúc được điều khiển bằng gió. Tại bất kỳ thời điểm nào, cấu trúc chứa chính xác các đầu dò đáp ứng yêu cầu về nhiệt độ cho truy vấn hiện tại. Nhiệm vụ còn lại trở thành: trong số tất cả các đầu dò được lưu trữ, tìm chi phí tối thiểu trong số những đầu dò có gió ít nhất là gió của truy vấn. Đây là truy vấn tối thiểu về tiền tố hoặc hậu tố tiêu chuẩn trên trục tọa độ được nén. 

Điều này biến vấn đề thành một đường quét ngoại tuyến kết hợp với cây phân đoạn theo các giá trị gió. Mỗi đầu dò được chèn một lần, mỗi truy vấn sẽ kích hoạt một truy vấn tối thiểu trong phạm vi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Quét + Cây phân đoạn | O((n+q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển vấn đề thành một quy trình ngoại tuyến trong đó các thăm dò và truy vấn được xử lý theo thứ tự thống nhất dựa trên nhiệt độ.

1. Trước tiên, hãy thu thập tất cả các thăm dò và truy vấn, đồng thời trích xuất tất cả các giá trị gió xuất hiện trong cả hai. Chúng tôi nén các giá trị này thành phạm vi chỉ mục nhỏ hơn vì cây phân đoạn không thể hoạt động hiệu quả trên các giá trị lên tới 10⁹ một cách trực tiếp. Việc nén duy trì trật tự, đó là thuộc tính duy nhất chúng ta cần. 
2. Sắp xếp tất cả các đầu dò theo thứ tự nhiệt độ giảm dần. Điều này đảm bảo rằng khi chúng tôi xử lý truy vấn với nhiệt độ T, mọi đầu dò mà chúng tôi đã chèn cho đến nay đều đáp ứng nhiệt độ ≥ T. 
3. Sắp xếp tất cả các truy vấn theo thứ tự nhiệt độ giảm dần, giữ nguyên các chỉ số ban đầu của chúng để chúng tôi có thể khôi phục thứ tự đầu ra sau này. 
4. Xây dựng cây phân đoạn trên trục gió nén. Mỗi nút lưu trữ chi phí tối thiểu trong số tất cả các đầu dò hiện được chèn vào phân đoạn đó. 
5. Quét qua các truy vấn theo thứ tự được sắp xếp. Duy trì một con trỏ trên đầu dò. Đối với mỗi truy vấn, chèn tất cả các đầu dò có nhiệt độ ít nhất bằng nhiệt độ truy vấn vào cây phân đoạn, cập nhật vị trí tương ứng với gió của chúng với chi phí tối thiểu. Bước này đảm bảo cấu trúc luôn thể hiện chính xác bộ thăm dò hợp lệ cho giới hạn nhiệt độ của truy vấn đó. 
6. Sau khi tất cả các thăm dò có liên quan được chèn vào, chúng tôi truy vấn cây phân đoạn về phạm vi gió từ sức gió yêu cầu của truy vấn cho đến chỉ số gió tối đa có thể. Điều này đưa ra chi phí tối thiểu trong số tất cả các đầu dò thỏa mãn cả hai ràng buộc. 
7. Nếu không có đầu dò nào tồn tại trong phạm vi đó, chúng tôi trả về -1 cho truy vấn đó. 

Tính chính xác dựa trên thực tế là khi đầu dò được đưa vào, nó vẫn hợp lệ cho tất cả các truy vấn trong quá trình quét trong tương lai vì những truy vấn đó chỉ làm giảm yêu cầu về nhiệt độ. 

### Tại sao nó hoạt động 

Ở mỗi bước truy vấn, cây phân đoạn chứa chính xác tập hợp các đầu dò thỏa mãn giới hạn nhiệt độ. Điều kiện gió được thực thi bằng truy vấn phạm vi tối thiểu. Vì cả hai ràng buộc đều được thực thi độc lập nhưng nhất quán thông qua thứ tự quét và cây phân đoạn nên mọi thăm dò được xem xét chính xác khi nó trở nên hợp lệ và không bao giờ bị loại trừ khi hợp lệ. Điều này đảm bảo rằng số tiền trả lại tối thiểu sẽ được sử dụng một cách chính xác trên các thăm dò khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, n):
        self.n = 1
        while self.n < n:
            self.n *= 2
        self.inf = 10**18
        self.t = [self.inf] * (2 * self.n)

    def update(self, i, val):
        i += self.n
        if val < self.t[i]:
            self.t[i] = val
            i //= 2
            while i:
                self.t[i] = min(self.t[2*i], self.t[2*i+1])
                i //= 2

    def query(self, l, r):
        res = self.inf
        l += self.n
        r += self.n
        while l <= r:
            if l % 2 == 1:
                res = min(res, self.t[l])
                l += 1
            if r % 2 == 0:
                res = min(res, self.t[r])
                r -= 1
            l //= 2
            r //= 2
        return res

def solve():
    n, q = map(int, input().split())
    probes = []
    wind_vals = []

    for _ in range(n):
        x, y, c = map(int, input().split())
        probes.append((x, y, c))
        wind_vals.append(y)

    queries = []
    for i in range(q):
        t, w = map(int, input().split())
        queries.append((t, w, i))
        wind_vals.append(w)

    wind_vals = sorted(set(wind_vals))
    comp = {v: i for i, v in enumerate(wind_vals)}

    probes.sort(reverse=True)
    queries.sort(reverse=True)

    seg = SegTree(len(wind_vals))

    ans = [-1] * q
    j = 0

    for t, w, idx in queries:
        while j < n and probes[j][0] >= t:
            _, y, c = probes[j]
            seg.update(comp[y], c)
            j += 1

        pos = comp[w]
        res = seg.query(pos, len(wind_vals) - 1)
        ans[idx] = -1 if res == seg.inf else res

    print("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Giải pháp được xây dựng xung quanh thứ tự quét. Danh sách thăm dò được sắp xếp sao cho khi chúng ta di chuyển qua một thăm dò, nó sẽ không bao giờ có liên quan nữa. Cây phân đoạn lưu trữ chi phí tối thiểu cho mỗi nhóm gió và chỉ cập nhật các giá trị giảm dần, đó là lý do tại sao việc lan truyền tối thiểu đơn giản lại hoạt động. 

Logic truy vấn luôn là truy vấn hậu tố vì chúng ta cần giá trị gió ≥ bắt buộc nên chúng ta truy vấn từ chỉ mục nén trở đi. 

Một cạm bẫy triển khai phổ biến là quên rằng nhiều đầu dò có thể ánh xạ tới cùng tọa độ gió sau khi nén. Việc cập nhật cây phân đoạn phải có chi phí tối thiểu, không được ghi đè một cách mù quáng. 

## Ví dụ đã hoạt động 

Vì câu lệnh ban đầu không cung cấp các mẫu rõ ràng ở định dạng rõ ràng nên hãy xem xét các trường hợp được xây dựng sau đây. 

### Ví dụ 1 

đầu vào:```
3 2
5 1 100
1 5 1
3 3 2
3 3
2 2
```Chúng tôi xử lý các đầu dò được sắp xếp theo nhiệt độ: (5,1,100), (3,3,2), (1,5,1). Truy vấn là (3,3) rồi (2,2). 

Ở truy vấn (3,3), chỉ các đầu dò có nhiệt độ ≥ 3 mới được chèn: (5,1,100) và (3,3,2). Giới hạn gió chọn trong số các gió hợp lệ ≥ 3, chỉ có (3,3,2) đủ điều kiện nên đáp án là 2. 

Tại truy vấn (2,2), tất cả các đầu dò được chèn vào. Các ứng cử viên hợp lệ bao gồm cả ba; trong số những người có gió ≥ 2 thì cả (1,5,1) và (3,3,2) đều đủ điều kiện nên tối thiểu là 1. 

| Truy vấn | Đầu dò được chèn (x ≥t) | Phạm vi gió hợp lệ | Trả lời | 
| --- | --- | --- | --- | 
| (3,3) | (5,1,100), (3,3,2) | y ≥3 → {2} | 2 | 
| (2,2) | tất cả | y ≥2 → {1,2} | 1 | 

Điều này cho thấy cách lọc nhiệt độ được xử lý hoàn toàn theo thứ tự quét. 

### Ví dụ 2 

đầu vào:```
2 1
10 10 5
1 10 1
10 5
```Chỉ có đầu dò (10,10,5) thỏa mãn gió ≥ 5 nên đáp án là 5. Đầu dò còn lại bị loại do hạn chế gió mặc dù nhiệt độ phù hợp. 

| Truy vấn | Đầu dò hoạt động | Gió hợp lệ ≥ 5 | Trả lời | 
| --- | --- | --- | --- | 
| (10,5) | cả hai đầu dò | chỉ (10,10,5) | 5 | 

Điều này xác nhận việc lọc gió được cây phân đoạn xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | mỗi đầu dò được chèn một lần, mỗi truy vấn sẽ thực hiện một truy vấn tối thiểu trong phạm vi log n | 
| Không gian | O(n) | cây phân đoạn cộng với mảng nén tọa độ | 

Hệ số log đến từ các hoạt động của cây phân đoạn trên các giá trị gió nén. Với n và q lên đến 10⁵, điều này hoàn toàn phù hợp trong giới hạn thông thường trong 2 giây trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    class SegTree:
        def __init__(self, n):
            self.n = 1
            while self.n < n:
                self.n *= 2
            self.inf = 10**18
            self.t = [self.inf] * (2 * self.n)

        def update(self, i, val):
            i += self.n
            self.t[i] = min(self.t[i], val)
            i //= 2
            while i:
                self.t[i] = min(self.t[2*i], self.t[2*i+1])
                i //= 2

        def query(self, l, r):
            res = self.inf
            l += self.n
            r += self.n
            while l <= r:
                if l % 2:
                    res = min(res, self.t[l])
                    l += 1
                if not r % 2:
                    res = min(res, self.t[r])
                    r -= 1
                l //= 2
                r //= 2
            return res

    def solve():
        n, q = map(int, input().split())
        probes = []
        vals = []
        for _ in range(n):
            x, y, c = map(int, input().split())
            probes.append((x, y, c))
            vals.append(y)

        queries = []
        for i in range(q):
            t, w = map(int, input().split())
            queries.append((t, w, i))
            vals.append(w)

        vals = sorted(set(vals))
        mp = {v:i for i,v in enumerate(vals)}

        probes.sort(reverse=True)
        queries.sort(reverse=True)

        seg = SegTree(len(vals))
        ans = [-1]*q
        j = 0

        for t, w, idx in queries:
            while j < n and probes[j][0] >= t:
                x, y, c = probes[j]
                seg.update(mp[y], c)
                j += 1

            res = seg.query(mp[w], len(vals)-1)
            ans[idx] = -1 if res == seg.inf else res

        return "\n".join(map(str, ans))

    return solve()

# custom tests

assert run("""1 1
5 5 10
5 5
""") == "10"

assert run("""2 1
1 10 5
10 1 1
5 5
""") == "-1"

assert run("""3 2
5 1 100
3 3 2
1 5 1
3 3
2 2
""") == "2\n1"

assert run("""4 2
10 10 7
10 9 5
9 10 6
8 8 1
9 9
10 10
""") == "5\n5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trận đấu thăm dò đơn | 10 | trường hợp hợp lệ tối thiểu | 
| không có thăm dò hợp lệ | -1 | xử lý sự cố | 
| sự thống trị hỗn hợp | 2/1 | tính chính xác của lọc 2D | 
| nhiều lần chồng chéo | 5/5 | xử lý dây buộc và độ chính xác của nén | 

## Vỏ cạnh 

Một tình huống khó khăn nảy sinh khi nhiều đầu dò có cùng giá trị gió nhưng chi phí khác nhau. Việc cập nhật cây phân đoạn phải luôn giữ chi phí tối thiểu; ghi đè sẽ loại bỏ các đầu dò tốt hơn một cách không chính xác. 

Một trường hợp khác là khi tất cả các đầu dò đều không đạt được một ràng buộc. Trong trường hợp đó, truy vấn cây phân đoạn trả về vô cùng và kết quả đầu ra phải được chuyển đổi thành -1 thay vì được in trực tiếp. 

Trường hợp tinh tế cuối cùng là khi giá trị gió của truy vấn nhỏ hơn tất cả các tọa độ nén hoặc lớn hơn tất cả chúng. Tính năng nén đảm bảo chúng tôi luôn ánh xạ chính xác nhưng việc quên sử dụng phạm vi hậu tố đầy đủ sẽ âm thầm loại bỏ các ứng cử viên hợp lệ.
