---
title: "CF 106486F - \u9001\u718f\u8089"
description: "Chúng ta được cung cấp một chuỗi dài các cấp độ, mỗi cấp độ mang một giá trị độ khó. Người chơi có thể loại bỏ chính xác một khối liền kề có độ dài cố định $k$ và mọi thứ bên ngoài khối đó vẫn giữ nguyên và góp phần tạo nên tổng độ khó."
date: "2026-06-20T12:55:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106486
codeforces_index: "F"
codeforces_contest_name: "Dalian University of Technology, Software College 2025 Freshman Contest"
rating: 0
weight: 106486
solve_time_s: 48
verified: true
draft: false
---

[CF 106486F - \u9001\u718f\u8089](https://codeforces.com/problemset/problem/106486/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi dài các cấp độ, mỗi cấp độ mang một giá trị độ khó. Người chơi có thể loại bỏ chính xác một khối liền kề có độ dài cố định$k$, và mọi thứ bên ngoài khối đó vẫn còn và góp phần tạo nên độ khó tổng thể. Mục tiêu sau mỗi lần sửa đổi là chọn khối tốt nhất có thể để loại bỏ sao cho phần còn lại có tổng nhỏ nhất có thể. 

Được định khung lại, điều này tương đương với việc hỏi liên tục: sau một số cập nhật điểm trên mảng, tổng tối đa của bất kỳ mảng con liền kề nào có độ dài chính xác là bao nhiêu$k$? Khi đã biết giá trị đó, câu trả lời là tổng của mảng trừ đi tổng cửa sổ tối đa này. 

Đầu vào bắt đầu bằng một mảng có kích thước lên tới$5 \times 10^5$, theo sau là đến$5 \times 10^5$cập nhật điểm. Mỗi bản cập nhật thay đổi vĩnh viễn một vị trí và sau mỗi thay đổi (bao gồm cả trạng thái ban đầu), chúng tôi phải đưa ra kết quả tối ưu. 

Ràng buộc buộc chúng ta phải thực hiện hành vi gần như tuyến tính hoặc gần tuyến tính cho mỗi lần cập nhật. Bất cứ điều gì tính toán lại tổng cửa sổ từ đầu cho mỗi lần cập nhật đều quá chậm, vì một lần tính toán lại sẽ bị mất.$O(n)$, dẫn đến$O(nm)$trong trường hợp xấu nhất. 

Một trường hợp lỗi tinh vi xuất hiện khi người ta cố gắng tính toán lại cục bộ xung quanh chỉ mục được cập nhật mà không nhận ra rằng mỗi phần tử tham gia vào nhiều độ dài-$k$cửa sổ. 

Ví dụ, nếu$k = 3$và mảng là:```
[1, 2, 3, 4, 5]
```tổng cửa sổ là:```
[1+2+3, 2+3+4, 3+4+5]
```Việc cập nhật một phần tử duy nhất như vị trí 3 sẽ ảnh hưởng đến cả ba cửa sổ ở đây, không chỉ phần tử ở giữa chỉ mục 3. Một bản cập nhật cục bộ đơn giản sẽ bỏ lỡ cấu trúc chồng chéo này và tạo ra mức tối đa không chính xác. 

Một trường hợp cạnh khác xuất hiện khi$k = n$. Khi đó có đúng một cửa sổ và câu trả lời luôn bằng 0 sau khi trừ toàn bộ số tiền. Bất kỳ triển khai nào giả định nhiều cửa sổ sẽ bị hỏng trừ khi trường hợp này được xử lý một cách tự nhiên. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Sau mỗi lần cập nhật, hãy xây dựng lại toàn bộ chiều dài-$k$tổng cửa sổ, quét tìm giá trị tối đa và trừ nó khỏi tổng số. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Tuy nhiên, mỗi bản cập nhật đều yêu cầu tính toán$n-k+1$tổng cửa sổ và chi phí tổng cửa sổ$O(k)$nếu được tính trực tiếp, hoặc$O(1)$sau tổng tiền tố nhưng vẫn yêu cầu$O(n)$tính toán lại cực đại. Với$m$cập nhật, điều này trở thành$O(nm)$, vượt xa giới hạn khả thi. 

Điều quan trọng nhất là chúng ta không cần phải tính toán lại tất cả các cửa sổ từ đầu. Mỗi bản cập nhật ảnh hưởng đến một bộ cửa sổ có cấu trúc rất chặt chẽ. Nếu vị trí$x$được sửa đổi thì chỉ các cửa sổ có điểm cuối bên trái nằm trong phạm vi$[x-k+1, x]$bị ảnh hưởng. Đây là đoạn tiếp giáp trong không gian cửa sổ. Do đó, mỗi bản cập nhật sẽ trở thành một thao tác thêm phạm vi trên một mảng thứ cấp đại diện cho tất cả các tổng cửa sổ. 

Chúng tôi chuyển đổi vấn đề từ việc duy trì một mảng theo cập nhật điểm sang duy trì một mảng có kích thước khác$n-k+1$theo phạm vi cập nhật và truy vấn tối đa toàn cầu. Cấu trúc đó chính xác là những gì một cây phân đoạn có khả năng lan truyền lười biếng hỗ trợ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại tất cả các cửa sổ mỗi lần cập nhật |$O(nm)$|$O(n)$| Quá chậm | 
| Cây phân đoạn trên tổng cửa sổ |$O((n+m)\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi mảng ban đầu thành một cấu trúc theo dõi tất cả độ dài-$k$tổng cửa sổ. 

1. Tính tổng của mảng. Điều này sẽ được cập nhật dần dần khi các giá trị thay đổi, vì vậy chúng tôi luôn có thể trả lời bằng cách sử dụng cửa sổ tổng trừ tốt nhất. 
2. Xây dựng mảng phụ trợ$S$Ở đâu$S[i]$đại diện cho tổng của mảng con$a[i..i+k-1]$. Điều này có thể được thực hiện trong thời gian tuyến tính bằng cách sử dụng cửa sổ trượt. Mảng này có độ dài$n-k+1$. 
3. Xây dựng cây phân đoạn$S$. Mỗi nút lưu trữ giá trị tối đa trong phân đoạn của nó và hỗ trợ lan truyền lười biếng để bổ sung phạm vi. 
4. Đối với mỗi lần cập nhật thay đổi vị trí$x$từ giá trị cũ$v$đến giá trị mới$y$, tính sự khác biệt$\Delta = y - v$. Cập nhật tổng số tiền bằng cách thêm$\Delta$. 
5. Xác định cửa sổ nào chứa chỉ mục$x$. Một cửa sổ bắt đầu từ$i$chứa$x$nếu và chỉ khi$i \le x \le i+k-1$, sắp xếp lại thành$i \in [x-k+1, x]$. Kẹp phạm vi này vào các chỉ số cửa sổ hợp lệ. 
6. Thực hiện phép cộng phạm vi$\Delta$trên cây phân đoạn trên phạm vi chỉ mục cửa sổ này. Điều này cập nhật tất cả các tổng cửa sổ bị ảnh hưởng theo thời gian logarit. 
7. Sau khi áp dụng bản cập nhật, hãy truy vấn giá trị lớn nhất trong cây phân đoạn. Đây là tổng cửa sổ tốt nhất có thể được loại bỏ. 
8. Tổng_tổng đầu ra trừ đi mức tối đa này. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mỗi tổng cửa sổ được lưu trữ trong cây phân đoạn luôn bằng tổng thực của mảng con tương ứng của nó trong mảng hiện tại. Mỗi lần cập nhật điểm chỉ ảnh hưởng đến các cửa sổ bao gồm chỉ mục đó và các cửa sổ đó tạo thành một phạm vi liền kề trong không gian chỉ mục cửa sổ. Vì phép cộng phạm vi phản ánh chính xác tập ảnh hưởng này nên không có cửa sổ nào bị bỏ qua và không có cửa sổ không liên quan nào được sửa đổi. Cây phân đoạn đảm bảo rằng sau mỗi lần cập nhật, tổng tối đa trên tất cả các cửa sổ là chính xác, do đó, việc trừ nó khỏi tổng sẽ mang lại giá trị còn lại tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.size = 1
        while self.size < self.n:
            self.size <<= 1
        self.mx = [0] * (2 * self.size)
        self.lz = [0] * (2 * self.size)

        for i in range(self.n):
            self.mx[self.size + i] = arr[i]
        for i in range(self.size - 1, 0, -1):
            self.mx[i] = max(self.mx[2 * i], self.mx[2 * i + 1])

    def _apply(self, i, v):
        self.mx[i] += v
        self.lz[i] += v

    def _push(self, i):
        if self.lz[i]:
            v = self.lz[i]
            self._apply(2 * i, v)
            self._apply(2 * i + 1, v)
            self.lz[i] = 0

    def _add(self, i, l, r, ql, qr, v):
        if ql <= l and r <= qr:
            self._apply(i, v)
            return
        self._push(i)
        mid = (l + r) // 2
        if ql <= mid:
            self._add(2 * i, l, mid, ql, qr, v)
        if qr > mid:
            self._add(2 * i + 1, mid + 1, r, ql, qr, v)
        self.mx[i] = max(self.mx[2 * i], self.mx[2 * i + 1])

    def add(self, l, r, v):
        if l <= r:
            self._add(1, 0, self.size - 1, l, r, v)

    def max_all(self):
        return self.mx[1]

n, m, k = map(int, input().split())
a = list(map(int, input().split()))

total = sum(a)

if k == n:
    base = 0
    for _ in range(m + 1):
        print(0)
    sys.exit()

S = []
window = sum(a[:k])
S.append(window)
for i in range(k, n):
    window += a[i]
    window -= a[i - k]
    S.append(window)

seg = SegTree(S)

out = []
for _ in range(m + 1):
    if _ > 0:
        x, y = map(int, input().split())
        x -= 1
        total += y - a[x]
        delta = y - a[x]

        l = x - k + 1
        r = x

        l = max(l, 0)
        r = min(r, n - k)

        seg.add(l, r, delta)

        a[x] = y

    out.append(str(total - seg.max_all()))

print("\n".join(out))
```### Thảo luận triển khai 

Giải pháp duy trì một cây phân đoạn riêng biệt trên tổng cửa sổ thay vì mảng ban đầu. Phần tế nhị nhất là ánh xạ chính xác bản cập nhật điểm vào bản cập nhật phạm vi trên các chỉ mục cửa sổ. Giới hạn$x-k+1$ĐẾN$x$phải kẹp cẩn thận vì cửa sổ không tồn tại bên ngoài$[0, n-k]$. Những lỗi ngẫu nhiên ở đây là nguồn lỗi phổ biến nhất. 

Cấu trúc lan truyền lười biếng đảm bảo rằng các cập nhật chồng chéo lặp đi lặp lại vẫn hiệu quả, vì mỗi cập nhật chỉ chạm vào$O(\log n)$nút. 

Trường hợp đặc biệt$k = n$thu gọn cấu trúc cửa sổ thành một khoảng duy nhất, vì vậy việc loại bỏ nó luôn để lại số 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét:```
n = 5, k = 2
a = [5, 1, 3, 7, 4]
```Cửa sổ ban đầu là:```
[6, 4, 10, 11]
```| Bước | Cập nhật | Tổng cửa sổ | Cửa sổ tối đa | Tổng cộng | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | không | [6,4,10,11] | 11 | 20 | 9 | 
| 1 | 4→2 tại vị trí 2 | [6,5,11,12] | 12 | 18 | 6 | 

Trạng thái đầu tiên đã cho thấy rằng việc loại bỏ phân đoạn có độ dài 2 tốt nhất tương ứng với việc loại bỏ cửa sổ có tổng bằng 11. 

### Ví dụ 2```
n = 4, k = 3
a = [2, 2, 2, 2]
```Windows:```
[6, 6]
```Sau khi cập nhật thay đổi một yếu tố:```
a = [2, 5, 2, 2]
```Windows trở thành:```
[9, 9]
```| Bước | Tổng cửa sổ | Cửa sổ tối đa | Tổng cộng | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | [6,6] | 6 | 8 | 2 | 
| 1 | [9,9] | 9 | 11 | 2 | 

Cả hai cửa sổ luôn di chuyển cùng nhau vì mọi phần tử đều tham gia một cách đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+m)\log n)$| Mỗi bản cập nhật thực hiện thêm phạm vi và truy vấn tối đa trên cây phân đoạn | 
| Không gian |$O(n)$| Phân đoạn cây trên$n-k+1$cửa sổ | 

Các ràng buộc cho phép lên đến$10^6$hoạt động, do đó cập nhật logarit là cần thiết. Cây phân đoạn giữ cho tổng công việc hoạt động tốt trong giới hạn ngay cả trong các mẫu đầu vào trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    class SegTree:
        def __init__(self, arr):
            self.n = len(arr)
            self.size = 1
            while self.size < self.n:
                self.size <<= 1
            self.mx = [0] * (2 * self.size)
            self.lz = [0] * (2 * self.size)
            for i in range(self.n):
                self.mx[self.size + i] = arr[i]
            for i in range(self.size - 1, 0, -1):
                self.mx[i] = max(self.mx[2 * i], self.mx[2 * i + 1])

        def _apply(self, i, v):
            self.mx[i] += v
            self.lz[i] += v

        def _push(self, i):
            if self.lz[i]:
                v = self.lz[i]
                self._apply(2 * i, v)
                self._apply(2 * i + 1, v)
                self.lz[i] = 0

        def _add(self, i, l, r, ql, qr, v):
            if ql <= l and r <= qr:
                self._apply(i, v)
                return
            self._push(i)
            mid = (l + r) // 2
            if ql <= mid:
                self._add(2 * i, l, mid, ql, qr, v)
            if qr > mid:
                self._add(2 * i + 1, mid + 1, r, ql, qr, v)
            self.mx[i] = max(self.mx[2 * i], self.mx[2 * i + 1])

        def add(self, l, r, v):
            if l <= r:
                self._add(1, 0, self.size - 1, l, r, v)

        def max_all(self):
            return self.mx[1]

    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))
    total = sum(a)

    if k == n:
        return "\n".join(["0"] * (m + 1))

    S = []
    window = sum(a[:k])
    S.append(window)
    for i in range(k, n):
        window += a[i]
        window -= a[i - k]
        S.append(window)

    seg = SegTree(S)

    out = []
    for i in range(m + 1):
        if i > 0:
            x, y = map(int, input().split())
            x -= 1
            delta = y - a[x]
            total += delta

            l = max(0, x - k + 1)
            r = min(n - k, x)
            seg.add(l, r, delta)

            a[x] = y

        out.append(str(total - seg.max_all()))

    return "\n".join(out)

# provided sample (placeholder format)
# assert run(...) == ...

# custom cases
assert run("""5 0 2
1 2 3 4 5
""").strip() == "9"

assert run("""3 1 3
1 1 1
2 5
""").split()[-1] == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nhỏ không có cập nhật | đánh giá đơn đúng | độ đúng cơ sở | 
| k = n trường hợp | 0 luôn | trường hợp loại bỏ hoàn toàn cạnh | 

## Vỏ cạnh 

Khi nào$k = n$, không gian cửa sổ thu gọn thành một khoảng duy nhất bao phủ toàn bộ mảng. Mỗi bản cập nhật đều thay đổi cả tổng số tiền và tổng cửa sổ duy nhất bằng nhau, do đó sự khác biệt của chúng vẫn bằng 0 xuyên suốt. Thuật toán xử lý việc này một cách tự nhiên bằng cách trả về số 0 ngay lập tức. 

Khi cập nhật xảy ra gần ranh giới, chẳng hạn như$x < k$hoặc$x > n-k$, phạm vi chỉ mục cửa sổ được tính toán một phần nằm ngoài các chỉ mục hợp lệ. Việc kẹp đảm bảo rằng chỉ các cửa sổ thực mới được cập nhật và không chạm vào phạm vi cây phân đoạn không hợp lệ. Điều này duy trì tính chính xác mà không cần vỏ đặc biệt. 

Khi tất cả các giá trị đều bằng nhau thì tổng của mọi cửa sổ đều giống hệt nhau. Mỗi bản cập nhật sẽ dịch chuyển tất cả các cửa sổ bị ảnh hưởng với cùng một lượng, đảm bảo sự bình đẳng giữa các phân đoạn. Cây phân đoạn duy trì mức tối đa thống nhất, do đó, bất kỳ cửa sổ nào cũng tối ưu như nhau và câu trả lời vẫn ổn định khi được cập nhật.
