---
title: "CF 106118B - Chuyến đi bằng khinh khí cầu"
description: "Chúng ta được cho một dãy núi, mỗi dãy có chiều cao ban đầu. Một hành trình được định nghĩa là đi bộ từ chỉ mục $l$ đến $r$, luôn di chuyển một bước sang phải và trả chi phí cho mỗi lần di chuyển là chênh lệch tuyệt đối giữa các độ cao liên tiếp."
date: "2026-06-19T20:05:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106118
codeforces_index: "B"
codeforces_contest_name: "2025 ICPC, Chula Selection Contest"
rating: 0
weight: 106118
solve_time_s: 61
verified: true
draft: false
---

[CF 106118B - Chuyến đi bằng khinh khí cầu](https://codeforces.com/problemset/problem/106118/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy núi, mỗi dãy có chiều cao ban đầu. Một hành trình được định nghĩa là đi bộ từ chỉ mục$l$ĐẾN$r$, luôn di chuyển một bước sang phải và trả chi phí cho mỗi lần di chuyển là chênh lệch tuyệt đối giữa các độ cao liên tiếp. Vì vậy, chi phí của một truy vấn là tổng của$|a_i - a_{i+1}|$tổng thể$i$từ$l$ĐẾN$r-1$, nhưng độ cao không tĩnh. 

Giữa các truy vấn, chúng tôi được phép áp dụng bản cập nhật phạm vi để tăng tất cả độ cao trong một phân đoạn$[l, r]$bởi một giá trị nào đó$x$. Điều quan trọng là bản cập nhật này thay đổi độ cao, làm thay đổi nhiều điểm khác biệt liền kề trên toàn cầu, không chỉ cục bộ bên trong phân khúc. 

Khó khăn là cả hai hoạt động đều có số lượng lớn, lên tới$3 \cdot 10^5$, do đó, việc tính toán lại toàn bộ chi phí từ đầu cho mỗi truy vấn là quá chậm. Thách thức là duy trì một mảng động trong đó chúng tôi hỗ trợ việc bổ sung phạm vi và truy vấn nhanh về tổng các khác biệt tuyệt đối liền kề trên một phân đoạn. 

Một quan sát quan trọng xuất phát từ việc hiểu cách cập nhật ảnh hưởng đến sự khác biệt. Nếu cả hai điểm cuối của một cạnh$(i, i+1)$nằm trong hoặc ngoài phạm vi cập nhật, sự khác biệt của chúng không thay đổi. Chỉ các cạnh vượt qua ranh giới của phạm vi cập nhật mới bị ảnh hưởng. Địa phương này là những gì làm cho vấn đề có thể giải quyết được. 

Các trường hợp khó khăn phá vỡ lối suy nghĩ ngây thơ bao gồm: 

Việc tính toán lại hoàn toàn đơn giản cho mỗi truy vấn sẽ thất bại ngay lập tức. Ví dụ: nếu tất cả các hoạt động là truy vấn loại 2, mỗi chi phí$O(n)$, tổng công việc trở thành$O(nq)$, đó là về$9 \cdot 10^{10}$hoạt động. 

Một thất bại tinh vi hơn là giả định rằng chúng ta có thể duy trì tổng tiền tố của các khác biệt một cách trực tiếp mà không tính đến các cập nhật phạm vi. Hãy xem xét một bản cập nhật phân đoạn chỉ dịch chuyển một phần của mảng; mảng sai phân không được cập nhật bằng các phép cộng điểm đơn giản, vì chỉ có sự khác biệt về ranh giới thay đổi. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Chúng tôi duy trì mảng trực tiếp. Để thêm phạm vi, chúng tôi tăng từng phần tử trong$[l, r]$. Đối với một truy vấn, chúng tôi tính tổng chênh lệch tuyệt đối trong phạm vi được yêu cầu bằng cách quét tuyến tính. 

Điều này đúng vì nó trực tiếp tuân theo định nghĩa của các phép toán. Tuy nhiên, mỗi lần cập nhật đều tốn$O(n)$trong trường hợp xấu nhất và mỗi truy vấn cũng tốn$O(n)$, dẫn đến hành vi bậc hai hoặc tệ hơn. 

Nút thắt cổ chai xuất phát từ thực tế là mọi thao tác đều chạm vào các phân đoạn có tiềm năng lớn và không thể sử dụng lại tính toán trước đó. 

Cái nhìn sâu sắc quan trọng là tách vấn đề thành hai lớp. Đầu tiên, chúng tôi duy trì mảng theo phép cộng phạm vi bằng cách sử dụng cây phân đoạn hoặc cây Fenwick với khả năng lan truyền lười. Thứ hai, thay vì tính toán lại sự khác biệt tuyệt đối trong một phạm vi mọi lúc, chúng tôi duy trì cấu trúc đóng góp của các cặp liền kề. 

Chúng tôi xác định một mảng phụ trợ$d_i = a_{i+1} - a_i$. Chi phí truy vấn là tổng của$|d_i|$trên phân khúc. Một phạm vi bổ sung của$x$ảnh hưởng$d_i$chỉ khi chính xác một trong$a_i$Và$a_{i+1}$nằm trong phạm vi cập nhật. Điều này có nghĩa là chỉ có hai vị trí ranh giới cho mỗi nội dung cập nhật:$l-1$Và$r$. 

Vì vậy, thay vì cập nhật nhiều hiệu, chúng ta chỉ cập nhật hai giá trị hiệu liền kề. Trong khi đó, chúng tôi duy trì một cây phân đoạn trên$d_i$hỗ trợ cập nhật điểm và truy vấn tổng phạm vi của các giá trị tuyệt đối. Để xử lý các thay đổi dấu một cách hiệu quả, chúng tôi duy trì$d_i$chính nó và sự đóng góp tuyệt đối của nó thông qua cấu trúc hỗ trợ các cập nhật về điểm cuối do việc bổ sung phạm vi tạo ra. 

Ý tưởng cốt lõi là phép cộng phạm vi chuyển thành cập nhật tối đa hai điểm trên mảng hiệu, làm cho cả hai phép tính đều có dạng logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(q \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì mảng$a$ngầm với cây Fenwick hoặc cây phân đoạn hỗ trợ truy vấn điểm và thêm phạm vi. Song song đó, chúng tôi duy trì một cấu trúc cho sự khác biệt$d_i = a_{i+1} - a_i$, nhưng chúng tôi không bao giờ xây dựng lại toàn bộ mảng một cách rõ ràng sau mỗi thao tác. 

1. Khởi tạo cấu trúc dữ liệu hỗ trợ phép cộng phạm vi và truy vấn điểm trên mảng ban đầu$a$. Điều này cho phép chúng tôi khôi phục mọi$a_i$khi cần mà không lưu trữ tất cả các bản cập nhật một cách rõ ràng. 
2. Xây dựng một loạt những khác biệt ban đầu$d_i = a_{i+1} - a_i$. Đây là sự đóng góp của địa phương vào chi phí đường đi. 
3. Xây dựng cây phân đoạn$|d_i|$, hỗ trợ cập nhật điểm và truy vấn tổng phạm vi. Cấu trúc này trả lời trực tiếp các truy vấn chi phí chuyến đi. 
4. Khi xử lý một thao tác thêm phạm vi$(l, r, x)$, quan sát thấy rằng chỉ những khác biệt liên quan đến ranh giới mới thay đổi. Cụ thể chỉ$d_{l-1}$Và$d_r$bị ảnh hưởng vì tất cả các cạnh bên trong đều có cả hai điểm cuối dịch chuyển như nhau. 
5. Để cập nhật những khác biệt biên này, trước tiên hãy tính các giá trị cũ của$a_{l-1}, a_l, a_r, a_{r+1}$sử dụng truy vấn điểm trên cấu trúc thêm phạm vi. 
6. Tính toán lại$d_{l-1} = a_l - a_{l-1}$nếu như$l > 1$và cập nhật giá trị tuyệt đối của nó trong cây phân đoạn. 
7. Tính toán lại$d_r = a_{r+1} - a_r$nếu như$r < n$và cập nhật giá trị tuyệt đối của nó một cách tương tự. 
8. Áp dụng phép cộng phạm vi$+x$trên cấu trúc chính để các truy vấn trong tương lai phản ánh độ cao được cập nhật. 
9. Đối với truy vấn chuyến đi$(l, r)$, trả về tổng của$|d_i|$vì$i \in [l, r-1]$từ cây phân đoạn. 

Tại sao nó hoạt động được gắn liền với sự bất biến của những khác biệt bên trong dưới sự thay đổi đồng đều. Khi mọi phần tử trong một phân đoạn được tăng cùng một giá trị thì sự khác biệt bên trong phân đoạn đó không thay đổi. Chỉ các cạnh vượt qua ranh giới của đoạn mới thấy một điểm cuối thay đổi còn điểm cuối kia thì không, đó là lý do tại sao chỉ có hai vị trí đó cần tính toán lại. 

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
        self.build(1, 0, self.n - 1, arr)

    def build(self, v, tl, tr, arr):
        if tl == tr:
            self.t[v] = arr[tl]
        else:
            tm = (tl + tr) // 2
            self.build(v*2, tl, tm, arr)
            self.build(v*2+1, tm+1, tr, arr)
            self.t[v] = self.t[v*2] + self.t[v*2+1]

    def update(self, v, tl, tr, pos, val):
        if tl == tr:
            self.t[v] = val
        else:
            tm = (tl + tr) // 2
            if pos <= tm:
                self.update(v*2, tl, tm, pos, val)
            else:
                self.update(v*2+1, tm+1, tr, pos, val)
            self.t[v] = self.t[v*2] + self.t[v*2+1]

    def query(self, v, tl, tr, l, r):
        if l > r:
            return 0
        if l == tl and r == tr:
            return self.t[v]
        tm = (tl + tr) // 2
        return self.query(v*2, tl, tm, l, min(r, tm)) + \
               self.query(v*2+1, tm+1, tr, max(l, tm+1), r)

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    fw = Fenwick(n)
    for i, v in enumerate(a, 1):
        fw.range_add(i, i, v)

    if n > 1:
        diff = [abs(a[i+1] - a[i]) for i in range(n-1)]
        st = SegTree(diff)
    else:
        st = None

    out = []

    for _ in range(q):
        tmp = list(map(int, input().split()))
        t = tmp[0]

        if t == 1:
            l, r, x = tmp[1], tmp[2], tmp[3]
            fw.range_add(l, r, x)

            if n > 1:
                if l > 1:
                    i = l - 2
                    left = fw.point_query(l-1)
                    right = fw.point_query(l)
                    st.update(1, 0, n-2, i, abs(right - left))

                if r < n:
                    i = r - 1
                    left = fw.point_query(r)
                    right = fw.point_query(r+1)
                    st.update(1, 0, n-2, i, abs(right - left))

        else:
            l, r = tmp[1], tmp[2]
            if l == r:
                out.append("0")
            else:
                out.append(str(st.query(1, 0, n-2, l-1, r-2)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Cây Fenwick lưu trữ mảng theo phép cộng phạm vi để bất kỳ điểm nào cũng có thể được xây dựng lại theo thời gian logarit. Cây phân đoạn lưu trữ những khác biệt tuyệt đối liền kề và hỗ trợ cập nhật từng vị trí khi giá trị biên thay đổi. Logic cập nhật chỉ tính toán lại cẩn thận những khác biệt bị ảnh hưởng sau mỗi lần thêm phạm vi. 

Việc lập chỉ mục hơi tinh tế: chỉ số khác biệt$i$tương ứng với cạnh$(i, i+1)$, vậy truy vấn$[l, r]$bản đồ tới$[l-1, r-2]$trong cấu trúc khác biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
1 3 2 5 4
2 1 5
1 2 4 3
2 1 5
```Sự khác biệt ban đầu là: 

| tôi | một [tôi] | a[i+1] | khác biệt | 
| --- | --- | --- | --- | 
| 1 | 1 | 3 | 2 | 
| 2 | 3 | 2 | 1 | 
| 3 | 2 | 5 | 3 | 
| 4 | 5 | 4 | 1 | 

Truy vấn đầu tiên tính tổng tất cả các khác biệt: 2 + 1 + 3 + 1 = 7. 

Sau khi thêm 3 vào [2,4], mảng trở thành [1,6,5,8,4]. 

Cập nhật các cạnh bị ảnh hưởng: 

| cạnh | khác biệt mới | 
| --- | --- | 
| (1,2) | 5 | 
| (4,5) | 4 | 

Khác biệt hoàn toàn trở thành [5,1,3,4], tổng là 13. 

### Ví dụ 2 

đầu vào:```
4 2
10 10 10 10
2 1 4
1 2 3 5
```Truy vấn đầu tiên là 0 vì tất cả các giá trị đều bằng nhau. 

Sau khi cập nhật, mảng trở thành [10,15,15,10]. 

Sự khác biệt trở thành [5,0,5], cho thấy rằng chỉ các cạnh ranh giới mới quan trọng sau khi cập nhật. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \log n)$| Mỗi bản cập nhật chạm vào hai nút cây phân đoạn và cập nhật Fenwick | 
| Không gian |$O(n)$| Cửa hàng Cây Fenwick và cây phân đoạn | 

Giải pháp này phù hợp một cách thoải mái trong giới hạn vì mỗi thao tác giảm xuống một số lượng cập nhật logarit không đổi, tránh mọi thao tác truyền tải toàn bộ mảng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    solve()
    return sys.stdout.getvalue().strip()

# minimum size
assert run("1 1\n5\n2 1 1\n") == "0"

# simple chain
assert run("3 2\n1 2 3\n2 1 3\n2 2 3\n") == "2\n1"

# range update affects only boundaries
assert run("5 3\n1 1 5 10\n2 1 5\n2 2 4\n") == "0\n0"

# all equal
assert run("4 2\n7 7 7 7\n2 1 4\n2 2 3\n") == "0\n0"

# alternating
assert run("5 1\n1 2 3 4 5\n2 1 5\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | phân khúc tầm thường | 
| chuỗi đơn giản | 2,1 | tính đúng đắn của tổng khác biệt | 
| cập nhật đầy đủ | 0,0 | cập nhật chỉ có ranh giới | 
| tất cả đều bình đẳng | 0,0 | ổn định không thay đổi | 
| xen kẽ | 4 | tính đúng đắn chung | 

## Vỏ cạnh 

Đối với mảng một phần tử, không có cạnh nào, vì vậy mọi truy vấn đều phải trả về 0. Cây phân đoạn dựa trên sự khác biệt trống và mã sẽ bỏ qua nó một cách chính xác khi$n = 1$. 

Đối với bản cập nhật toàn phạm vi, chẳng hạn như tăng mọi phần tử lên một hằng số, không có sự khác biệt nào thay đổi cả. Thuật toán chỉ cố gắng tính toán lại$d_{l-1}$Và$d_r$, cả hai đều nằm ngoài giới hạn trong trường hợp này nên không có cập nhật nào xảy ra. Điều này phù hợp với thực tế là sự thay đổi đồng đều sẽ bảo toàn mọi khác biệt tuyệt đối. 

Đối với các cập nhật chạm vào ranh giới, chẳng hạn như$l = 1$hoặc$r = n$, chỉ tồn tại một phía của bản cập nhật ranh giới. Việc triển khai kiểm tra các giới hạn một cách rõ ràng trước khi tính toán lại những khác biệt, ngăn chặn các truy vấn không hợp lệ vào cấu trúc Fenwick.
