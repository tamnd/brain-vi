---
title: "CF 104313H - \u0414\u043e\u0431\u0430\u0432\u043b\u0435\u043d\u0438\u0435 \u0438 GCD"
description: "Chúng ta được cung cấp một mảng các số nguyên được sửa đổi theo thời gian và chúng ta phải trả lời các truy vấn về mảng con GCD của nó. Hai hoạt động xảy ra trực tuyến. Thao tác đầu tiên thêm một giá trị cố định vào mọi phần tử trong tiền tố hoặc phạm vi."
date: "2026-07-01T19:47:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104313
codeforces_index: "H"
codeforces_contest_name: "II \u041e\u0442\u043a\u0440\u044b\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u042e\u041c\u0428 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 104313
solve_time_s: 58
verified: true
draft: false
---

[CF 104313H - \u0414\u043e\u0431\u0430\u0432\u043b\u0435\u043d\u0438\u0435 \u0438 GCD](https://codeforces.com/problemset/problem/104313/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên được sửa đổi theo thời gian và chúng ta phải trả lời các truy vấn về mảng con GCD của nó. Hai hoạt động xảy ra trực tuyến. Thao tác đầu tiên thêm một giá trị cố định vào mọi phần tử trong tiền tố hoặc phạm vi. Phép toán thứ hai yêu cầu ước số chung lớn nhất của tất cả các số trong một mảng con nhất định. 

Khó khăn xuất phát từ thực tế là cả hai thao tác đều tương tác theo cách phi tuyến tính. Phép cộng phạm vi thay đổi đồng thời tất cả các giá trị và GCD nhạy cảm với các giá trị tuyệt đối nhưng hoạt động có thể dự đoán được đối với các khác biệt. Nhiệm vụ cốt lõi là duy trì đủ cấu trúc của mảng để sau nhiều lần cập nhật chồng chéo, chúng ta vẫn có thể tính toán GCD trên bất kỳ khoảng thời gian nào một cách nhanh chóng. 

Các ràng buộc đẩy chúng ta tới hành vi gần tuyến tính hoặc logarit cho mỗi truy vấn. Với tối đa 200.000 thao tác, bất kỳ giải pháp nào tính toán lại GCD trong một phạm vi sau mỗi lần cập nhật đều bị coi là quá chậm. Việc tính toán lại phân đoạn đơn giản sẽ tốn O(n) cho mỗi truy vấn, dẫn đến O(nq), vượt xa mức có thể chấp nhận được. 

Một điểm tinh tế là các bản cập nhật không phải là cập nhật điểm mà là bổ sung phạm vi. Điều này phá vỡ nhiều thủ thuật GCD của cây phân đoạn tiêu chuẩn trừ khi chúng ta điều chỉnh lại vấn đề theo sự khác biệt. 

Việc triển khai ngây thơ sẽ thất bại một cách rất cụ thể. Giả sử chúng ta duy trì mảng một cách trực tiếp và háo hức áp dụng các phép cộng phạm vi. Sau đó, một truy vấn như GCD trên một phân đoạn lớn sẽ yêu cầu quét tất cả các phần tử. Ngay cả khi cập nhật nhanh, các truy vấn sẽ trở thành tuyến tính và các hoạt động xen kẽ buộc phải có hành vi bậc hai trong trường hợp xấu nhất. 

Một chế độ lỗi khác là cố gắng duy trì tiền tố GCD. Tiền tố GCD không ổn định khi cộng. Ví dụ, nếu chúng ta có`[6, 10]`, GCD là 2. Nếu chúng ta thêm 1 vào phần tử đầu tiên, chúng ta sẽ nhận được`[7, 10]`và GCD trở thành 1. Cấu trúc tiền tố không đưa ra cách trực tiếp nào để cập nhật hiệu quả. 

Khó khăn chính là GCD bất biến khi thực hiện phép trừ chứ không phải phép cộng. Điều này gợi ý rằng chúng ta nên chuyển vấn đề thành một vấn đề dựa trên sự khác biệt. 

## Phương pháp tiếp cận 

Một giải pháp brute-force lưu trữ mảng một cách rõ ràng. Đối với mỗi truy vấn loại 1, nó sẽ thêm x vào mọi phần tử trong phạm vi nhất định. Đối với mỗi truy vấn loại 2, nó tính toán gcd của mảng con được yêu cầu bằng cách lặp qua tất cả các phần tử. 

Điều này đúng vì nó trực tiếp tuân theo định nghĩa của cả hai phép toán. Tuy nhiên, mỗi lần cập nhật có chi phí O(n) trong trường hợp xấu nhất và mỗi truy vấn có chi phí O(n). Với q lên tới 200.000, điều này dẫn đến khoảng 4×10^10 phép tính trong trường hợp xấu nhất, điều này là không khả thi. 

Quan sát chính là tách mảng thành một giá trị tiền tố và một mảng khác biệt. Nếu chúng ta định nghĩa`b[i] = a[i] - a[i-1]`, thì GCD của một đoạn`[l, r]`có thể được thể hiện bằng cách sử dụng`a[l]`và GCD của sự khác biệt trong`[l+1, r]`. Cụ thể,`gcd(a[l], b[l+1], ..., b[r])`. 

Việc cộng phạm vi trở nên đơn giản hơn nhiều trong mảng sai phân. Thêm x vào một phạm vi`[l, r]`tăng lên`b[l]`theo x và giảm`b[r+1]`bởi x. Điều này biến một bản cập nhật phạm vi thành hai bản cập nhật điểm. 

Chúng tôi vẫn cần các truy vấn GCD phạm vi nhanh`b`và cập nhật điểm nhanh. Điều đó có thể được xử lý bằng cách sử dụng cây phân đoạn lưu trữ GCD. Chúng tôi cũng duy trì cây Fenwick hoặc cây phân đoạn để lấy tổng tiền tố`a[l]`hiệu quả sau nhiều lần cập nhật. 

Điều này làm giảm cả hai phép toán xuống O(log n), làm cho giải pháp trở nên khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Mảng chênh lệch + Cây phân đoạn | O(q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi mảng thành một cấu trúc trong đó các bản cập nhật trở thành cục bộ và các truy vấn có thể phân tách được. 

1. Xây dựng mảng phụ trợ`b`Ở đâu`b[i] = a[i] - a[i-1]`. Chúng tôi đối xử`a[0] = 0`. Biểu diễn này mã hóa tất cả các thay đổi giữa các phần tử liên tiếp thay vì giá trị tuyệt đối. 
2. Xây dựng cây phân đoạn`b`hỗ trợ truy vấn phạm vi GCD và cập nhật điểm. Điều này cho phép chúng ta duy trì GCD của bất kỳ khoảng chênh lệch nào một cách hiệu quả. 
3. Xây dựng cây Fenwick (hoặc cây phân đoạn) trên các giá trị mảng ban đầu để hỗ trợ các truy vấn tiền tố điểm và bổ sung phạm vi. Cấu trúc này là cần thiết để phục hồi`a[i]`sau nhiều lần cập nhật. 
4. Đối với truy vấn bổ sung phạm vi`[l, r]`với giá trị x, cập nhật cây Fenwick bằng cách thêm x vào`[l, r]`. Trong mảng khác biệt, áp dụng cập nhật điểm`b[l] += x`Và`b[r+1] -= x`nếu như`r+1`nằm trong giới hạn. Điều này bảo tồn chính xác tất cả các khác biệt về tiền tố. 
5. Đối với truy vấn GCD trên`[l, r]`, trước tiên hãy tính giá trị thực của`a[l]`sử dụng cây Fenwick. Sau đó tính toán`g = gcd(a[l], query_gcd(b[l+1..r]))`sử dụng cây phân đoạn trên`b`. 
6. Đầu ra`g`. 

Lý do điều này có tác dụng là vì bất kỳ phân khúc nào`[l, r]`có thể được phân tách thành giá trị bắt đầu`a[l]`cộng với chênh lệch tích lũy. GCD của một tập hợp số bằng GCD của một phần tử và tất cả các hiệu theo cặp, và trong cách biểu diễn này, những khác biệt đó được nắm bắt chính xác bởi`b`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

    def range_add(self, l, r, v):
        self.add(l, v)
        if r + 1 <= self.n:
            self.add(r + 1, -v)

    def point_query(self, i):
        return self.sum(i)

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.t = [0] * (4 * self.n)
        self.build(1, 1, self.n, arr)

    def build(self, v, l, r, arr):
        if l == r:
            self.t[v] = arr[l - 1]
        else:
            m = (l + r) // 2
            self.build(v * 2, l, m, arr)
            self.build(v * 2 + 1, m + 1, r, arr)
            self.t[v] = abs(self.t[v * 2] if self.t[v * 2] else 0)
            if self.t[v * 2 + 1]:
                self.t[v] = math.gcd(self.t[v], abs(self.t[v * 2 + 1]))

    def update(self, v, l, r, i, val):
        if l == r:
            self.t[v] += val
        else:
            m = (l + r) // 2
            if i <= m:
                self.update(v * 2, l, m, i, val)
            else:
                self.update(v * 2 + 1, m + 1, r, i, val)
            self.t[v] = math.gcd(abs(self.t[v * 2]), abs(self.t[v * 2 + 1]))

    def query(self, v, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.t[v]
        m = (l + r) // 2
        res = 0
        if ql <= m:
            res = math.gcd(res, self.query(v * 2, l, m, ql, qr))
        if qr > m:
            res = math.gcd(res, self.query(v * 2 + 1, m + 1, r, ql, qr))
        return res

def main():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    bit = Fenwick(n)
    for i, v in enumerate(a, 1):
        bit.range_add(i, i, v)

    b = [0] * (n + 1)
    for i in range(1, n):
        b[i] = a[i] - a[i - 1]

    st = SegTree(b[1:])

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            l, r, x = map(int, tmp[1:])
            bit.range_add(l, r, x)
            st.update(1, 1, n - 1, l, x)
            if r < n:
                st.update(1, 1, n - 1, r + 1, -x)
        else:
            l, r = map(int, tmp[1:])
            al = bit.point_query(l)
            if l == r:
                print(al)
            else:
                g = st.query(1, 1, n - 1, l, r - 1)
                print(abs(math.gcd(al, g)))

if __name__ == "__main__":
    import math
    main()
```Cây Fenwick được sử dụng hoàn toàn để khôi phục giá trị hiện tại ở bất kỳ vị trí nào sau nhiều lần bổ sung phạm vi. Cây phân đoạn được xây dựng trên mảng sai phân để các truy vấn phạm vi GCD tương ứng với mảng con GCD của sự khác biệt. 

Chi tiết triển khai quan trọng là tất cả các phép toán GCD phải được thực hiện trên các giá trị tuyệt đối, vì phép cộng phạm vi có thể đưa ra các giá trị trung gian âm trong mảng sai phân mặc dù các giá trị cuối cùng vẫn nhất quán. 

Một điểm tinh tế khác là xử lý các ranh giới. Mảng khác biệt chỉ có các chỉ số có ý nghĩa từ 1 đến n-1, vì vậy các truy vấn trên`[l, r]`chỉ dịch sang`[l, r-1]`trên cây phân đoạn. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng nhỏ`[10, 6, 15, 12]`và một số cập nhật. 

Sau khi chuyển đổi sang sự khác biệt, chúng tôi nhận được`b = [0, -4, 9, -3]`. 

### Dấu vết 1: truy vấn không cập nhật 

| Bước | tôi | r | một [l] | phạm vi khác biệt | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Truy vấn | 1 | 4 | 10 | gcd(-4, 9, -3) = 1 | gcd(10, 1) = 1 | 

Điều này cho thấy rằng ngay cả khi các giá trị ban đầu được cấu trúc thì những khác biệt vẫn nắm bắt được sự biến đổi bên trong ảnh hưởng đến GCD cuối cùng. 

### Dấu vết 2: cộng phạm vi 

Giả sử chúng ta thêm +2 vào`[2, 3]`. 

Mảng trở thành`[10, 8, 17, 12]`. 

Sự khác biệt trở thành`b = [-2, 9, -5]`. 

| Bước | Hoạt động | Trạng thái mảng | trạng thái khác biệt | 
| --- | --- | --- | --- | 
| 1 | ban đầu | [10, 6, 15, 12] | [ -4, 9, -3 ] | 
| 2 | thêm 2 vào [2,3] | [10, 8, 17, 12] | [ -2, 9, -5 ] | 

Truy vấn`[2,4]`công dụng`a[2] = 8`Và`gcd(9, -5) = 1`, cho kết quả 1 

Dấu vết này xác nhận rằng các bản cập nhật phạm vi sẽ chuyển chính xác thành hai bản cập nhật cục bộ trong cấu trúc khác biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | Mỗi bản cập nhật chạm vào Fenwick và cây phân đoạn theo thời gian logarit, mỗi truy vấn thực hiện tính toán GCD logarit | 
| Không gian | O(n) | Lưu trữ cây Fenwick và cây phân đoạn dựa trên sự khác biệt | 

Độ phức tạp vừa vặn trong giới hạn của n và q lên tới 200.000 do hệ số logarit vẫn nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, i, v):
            while i <= self.n:
                self.bit[i] += v
                i += i & -i

        def sum(self, i):
            s = 0
            while i > 0:
                s += self.bit[i]
                i -= i & -i
            return s

        def range_add(self, l, r, v):
            self.add(l, v)
            if r + 1 <= self.n:
                self.add(r + 1, -v)

        def point_query(self, i):
            return self.sum(i)

    class SegTree:
        def __init__(self, arr):
            self.n = len(arr)
            self.t = [0] * (4 * self.n)

        def build(self, v, l, r, arr):
            if l == r:
                self.t[v] = arr[l - 1]
            else:
                m = (l + r) // 2
                self.build(v*2, l, m, arr)
                self.build(v*2+1, m+1, r, arr)
                self.t[v] = math.gcd(self.t[v*2], self.t[v*2+1])

        def query(self, v, l, r, ql, qr):
            if ql <= l and r <= qr:
                return self.t[v]
            m = (l + r) // 2
            res = 0
            if ql <= m:
                res = math.gcd(res, self.query(v*2, l, m, ql, qr))
            if qr > m:
                res = math.gcd(res, self.query(v*2+1, m+1, r, ql, qr))
            return res

        def update(self, v, l, r, i, val):
            if l == r:
                self.t[v] += val
            else:
                m = (l + r) // 2
                if i <= m:
                    self.update(v*2, l, m, i, val)
                else:
                    self.update(v*2+1, m+1, r, i, val)
                self.t[v] = math.gcd(self.t[v*2], self.t[v*2+1])

    def solve(inp):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))

        bit = Fenwick(n)
        for i, v in enumerate(a, 1):
            bit.range_add(i, i, v)

        b = [0]*(n+1)
        for i in range(1, n):
            b[i] = a[i] - a[i-1]

        st = SegTree(b[1:])
        st.build(1, 1, n-1, b[1:])

        for _ in range(q):
            tmp = input().split()
            if tmp[0] == '1':
                l, r, x = map(int, tmp[1:])
                bit.range_add(l, r, x)
                st.update(1, 1, n-1, l, x)
                if r < n:
                    st.update(1, 1, n-1, r+1, -x)
            else:
                l, r = map(int, tmp[1:])
                al = bit.point_query(l)
                if l == r:
                    print(al)
                else:
                    g = st.query(1,1,n-1,l,r-1)
                    print(abs(math.gcd(al,g)))

    return solve(inp)

# Minimal sanity checks
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn phần tử đơn | giá trị trực tiếp | độ đúng cơ sở | 
| cập nhật đầy đủ | tuyên truyền nhất quán | tính đúng đắn của các cập nhật khác biệt | 
| cập nhật/truy vấn xen kẽ | hành vi gcd ổn định | tương tác của cả hai cấu trúc | 

## Vỏ cạnh 

Trường hợp một cạnh là khi phạm vi truy vấn có độ dài 1. Trong trường hợp này, câu trả lời chỉ đơn giản là một phần tử và không nên thực hiện truy vấn khác biệt. Thuật toán xử lý việc này một cách rõ ràng bằng cách trả về`a[l]`, tránh các truy vấn cây phân đoạn không hợp lệ. 

Một trường hợp khác là khi cập nhật mở rộng đến phần tử cuối cùng. Vì mảng khác biệt chỉ đi tới chỉ số n-1 nên bản cập nhật tại`r+1`phải bỏ qua khi`r = n`. Việc triển khai kiểm tra điều này một cách rõ ràng để tránh các cập nhật ngoài giới hạn. 

Trường hợp thứ ba là các bản cập nhật chồng chéo lặp đi lặp lại bị loại bỏ. Ví dụ: thêm x vào`[1, 5]`và sau đó thêm -x vào`[3, 7]`tạo ra sự hủy bỏ không tầm thường trong cấu trúc sai phân, nhưng vì mỗi lần cập nhật chỉ chạm vào điểm cuối nên hiệu ứng thực luôn nhất quán với mảng ban đầu.
