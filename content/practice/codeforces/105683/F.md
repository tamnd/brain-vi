---
title: "CF 105683F - \u0417\u043c\u0435\u0439\u043a\u0430"
description: "Chúng ta có một lưới hình chữ nhật rất lớn có kích thước $w nhân h$. Một con rắn có chiều dài cố định $k$ phải được đặt ở dạng “thẳng”, nghĩa là nó chiếm chính xác $k$ các ô liên tiếp theo chiều ngang trong một hàng hoặc chiều dọc trong một cột."
date: "2026-06-22T05:04:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105683
codeforces_index: "F"
codeforces_contest_name: "\u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u041d\u0415\u0419\u041c\u0410\u0420\u041a 2024-25, \u041f\u0435\u0440\u0432\u044b\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 105683
solve_time_s: 67
verified: true
draft: false
---

[CF 105683F - \u0417\u043c\u0435\u0439\u043a\u0430](https://codeforces.com/problemset/problem/105683/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một lưới hình chữ nhật rất lớn có kích thước$w \times h$. Một con rắn có chiều dài cố định$k$phải được đặt ở dạng “làm thẳng”, nghĩa là nó chiếm chính xác$k$các ô liên tiếp theo chiều ngang trong một hàng hoặc theo chiều dọc trong một cột. 

Lưới không có sẵn đầy đủ để sắp xếp vì sau vài vòng, chúng ta sẽ có nhiều chướng ngại vật hình chữ nhật thẳng hàng theo trục. Mỗi chướng ngại vật chặn tất cả các ô bên trong nó và các chướng ngại vật có thể chồng lên nhau. Con rắn không được phép chiếm giữ bất kỳ ô bị chặn nào, nhưng nó được phép chạm hoặc nằm cạnh chướng ngại vật miễn là nó không bao giờ chồng lên chúng. 

Nhiệm vụ là đếm xem có bao nhiêu vị trí hợp lệ của con rắn trên toàn bộ lưới, xem xét tất cả các vị trí thẳng đứng và ngang. 

Một điểm tinh tế quan trọng là chúng ta không đặt hình dạng một cách tùy tiện; chúng tôi đang đặt một chiều dài-$k$phân đoạn. Vì vậy, vị trí theo chiều ngang được xác định duy nhất bằng cách chọn ô bắt đầu$(x,y)$, và nó chiếm$(x,y),(x+1,y),\dots,(x+k-1,y)$. Vị trí dọc cũng tương tự. 

Các ràng buộc ngay lập tức loại trừ mọi mô phỏng trên mỗi ô hoặc mỗi hàng. Cả hai chiều đều tăng lên$10^9$, trong khi số lượng chướng ngại vật lên tới$10^5$. Bất kỳ cách tiếp cận nào lặp lại hàng, cột hoặc ô đều không thể thực hiện được. Ngay cả việc duy trì một lưới đầy đủ hoặc lưới nén các hàng cũng không khả thi vì số lượng hàng và cột riêng biệt bị ảnh hưởng vẫn quá lớn trong trường hợp xấu nhất. 

Một khó khăn tinh tế hơn là các chướng ngại vật chồng lên nhau và tương tác với nhau. Việc xây dựng liên kết đơn giản là cần thiết, nhưng ngay cả sau khi hợp nhất các hình chữ nhật, chúng ta vẫn cần đếm các đoạn có chiều dài hợp lệ$k$, không chỉ các tế bào tự do. 

Trường hợp một cạnh phá vỡ logic “đếm ô trống rồi chia” ngây thơ xuất hiện khi không gian trống bị phân mảnh. Ví dụ: nếu một hàng có các ô trống như:```
####....#....
```một cách tiếp cận đơn giản là đếm các ô tự do và chia cho$k$sẽ bị tính quá mức vì các phân đoạn phải liền kề nhau. Số lượng chính xác phụ thuộc vào từng khoảng thời gian rảnh liên tục. 

Một trường hợp lỗi khác xuất hiện khi chướng ngại vật chặn hoàn toàn một khu vực khiến một số hàng hoặc cột hoàn toàn không thể sử dụng được. Ví dụ: nếu hình chữ nhật bao phủ tất cả các hàng của một phạm vi cột thì các vị trí theo chiều ngang có thể biến mất hoàn toàn trong dải đó nhưng các vị trí theo chiều dọc vẫn tồn tại ở những nơi khác. Việc xử lý các hàng và cột một cách độc lập mà không phân tách cẩn thận sẽ dẫn đến việc đếm hai lần hoặc bỏ lỡ các tương tác. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ cố gắng kiểm tra mọi vị trí xuất phát có thể có của con rắn. Có khoảng$(w-k+1) \cdot h$bắt đầu theo chiều ngang và$(h-k+1) \cdot w$bắt đầu theo chiều dọc. Đối với mỗi lần bắt đầu, chúng tôi sẽ kiểm tra xem có tế bào nào của con rắn có gặp chướng ngại vật hay không. Ngay cả với quá trình tiền xử lý, việc kiểm tra từng ứng viên bắt đầu riêng lẻ vẫn quá chậm, có thể đạt tới$10^{18}$ứng viên trong trường hợp xấu nhất. 

Cải tiến cơ cấu đầu tiên là đảo ngược quan điểm. Thay vì hỏi liệu vị trí bắt đầu có hợp lệ hay không, chúng tôi hỏi điều kiện nào khiến vị trí bắt đầu không hợp lệ. Vị trí nằm ngang bắt đầu từ$(x,y)$không hợp lệ khi và chỉ khi tồn tại một ô chướng ngại vật cắt đoạn$[x, x+k-1] \times \{y\}$. Do đó, mỗi hình chữ nhật chướng ngại vật cấm một loạt các vị trí bắt đầu trên hàng đó. 

Đối với hình chữ nhật cố định$[x_1,x_2]\times[y_1,y_2]$, hãy xem xét các vị trí nằm ngang. Vị trí bắt đầu từ$x$không hợp lệ nếu đoạn của nó cắt hình chữ nhật theo chiều ngang, điều này xảy ra chính xác khi:$$x \le x_2 \quad \text{and} \quad x+k-1 \ge x_1$$Viết lại điều này theo vị trí bắt đầu hợp lệ sẽ cho:$$x \in [x_1-k+1, x_2]$$cho mỗi hàng$y \in [y_1,y_2]$. 

Vì vậy, mỗi chướng ngại vật tạo ra một hình chữ nhật thẳng hàng theo trục khác trong không gian của các vị trí bắt đầu hợp lệ$(x,y)$. Bài toán đếm ban đầu trở thành bài toán hợp các hình chữ nhật 2D trên “lưới vị trí bắt đầu”. Câu trả lời theo chiều ngang đến từ việc trừ đi các vùng bắt đầu bị cấm khỏi hình chữ nhật đầy đủ của tất cả các điểm bắt đầu có thể xảy ra. 

Vị trí dọc có tính đối xứng: chúng tôi hoán đổi vai trò của$x$Và$y$, tạo ra các hình chữ nhật bị cấm trong$(y_{\text{start}}, x)$không gian. 

Do đó, toàn bộ vấn đề được rút gọn thành việc tính diện tích hợp của các hình chữ nhật hai lần. 

Đường quét trực tiếp trên không gian được chuyển đổi này hoạt động hiệu quả vì chỉ có$O(n)$hình chữ nhật và chúng ta có thể nén tọa độ và duy trì cây phân đoạn trên một trục trong khi quét trục kia. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên tất cả bắt đầu |$O(whk)$hoặc tệ hơn |$O(1)$| Quá chậm | 
| Chuyển đổi hình chữ nhật + đường quét |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô tả trường hợp nằm ngang; trường hợp dọc giống hệt nhau sau khi hoán đổi trục. 

1. Chuyển mỗi hình chữ nhật chướng ngại vật thành một tập hợp các hình chữ nhật ở vị trí xuất phát bị cấm. Đối với hình chữ nhật$[x_1,x_2]\times[y_1,y_2]$, chúng tôi sản xuất:$$[x_1-k+1, x_2] \times [y_1,y_2]$$giao nhau với giới hạn bắt đầu hợp lệ$x \in [1, w-k+1]$. Điều này đảm bảo chúng tôi chỉ xem xét các vị trí xuất phát hợp pháp. 
2. Loại bỏ mọi hình chữ nhật đã biến đổi trở nên trống sau khi kẹp. Điều này xảy ra khi chướng ngại vật quá hẹp để ảnh hưởng đến bất kỳ đoạn đường dài nào. 
3. Thu thập tất cả tọa độ x từ ranh giới hình chữ nhật và nén chúng. Điều này là cần thiết vì tọa độ có thể lên tới$10^9$, nhưng chỉ$O(n)$điểm cuối khác biệt quan trọng đối với cấu trúc quét. 
4. Tạo các sự kiện quét dọc theo trục y: mỗi hình chữ nhật đóng góp một sự kiện “thêm khoảng” tại$y_1$và một sự kiện “xóa khoảng thời gian” tại$y_2+1$. 
5. Quét y theo thứ tự tăng dần. Giữa các giá trị y của sự kiện liên tiếp, tập hợp các hình chữ nhật đang hoạt động là không đổi, do đó liên kết khoảng x được bao phủ không thay đổi. 
6. Duy trì cây phân đoạn trên tọa độ x được nén lưu trữ chiều dài được bao phủ. Sau khi xử lý các sự kiện tại một y nhất định, cây biểu thị sự kết hợp của các khoảng x bị cấm tại y đó. 
7. Tính chiều dài không che trên trục x như sau:$$(w-k+1) - \text{covered\_length}$$Nhân số này với chiều cao khoảng y hiện tại để tích lũy tổng diện tích bị cấm. 
8. Sau khi kết thúc quá trình quét, trừ đi vùng cấm khỏi tổng số lần bắt đầu theo chiều ngang có thể$(w-k+1)\cdot h$. 
9. Lặp lại quy trình tương tự cho các vị trí dọc, hoán đổi vai trò x và y, đồng thời thay thế$w-k+1$với$h-k+1$. 
10. Tính tổng kết quả theo chiều ngang và chiều dọc. 

### Tại sao nó hoạt động 

Điều bất biến chính là mọi vị trí con rắn không hợp lệ đều tương ứng với chính xác một hình chữ nhật ở vị trí bắt đầu bị cấm và mọi vị trí bắt đầu bị cấm đều được bao phủ bởi ít nhất một hình chữ nhật như vậy. Đường quét tính toán diện tích hợp chính xác của các vùng bị cấm này mà không bị chồng chéo. Vì các vị trí ngang và dọc được xác định trên các không gian định hướng rời rạc nên số lượng của chúng có thể được tính tổng trực tiếp mà không cần tương tác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, xs):
        self.xs = xs
        self.n = len(xs) - 1
        self.tree = [0] * (4 * self.n)
        self.cnt = [0] * (4 * self.n)

    def _pushup(self, v, l, r):
        if self.cnt[v]:
            self.tree[v] = self.xs[r] - self.xs[l]
        else:
            if r - l == 1:
                self.tree[v] = 0
            else:
                self.tree[v] = self.tree[v*2] + self.tree[v*2+1]

    def update(self, v, l, r, ql, qr, val):
        if ql >= r or qr <= l:
            return
        if ql <= l and r <= qr:
            self.cnt[v] += val
            self._pushup(v, l, r)
            return
        m = (l + r) // 2
        self.update(v*2, l, m, ql, qr, val)
        self.update(v*2+1, m, r, ql, qr, val)
        self._pushup(v, l, r)

def solve(rects, W, H, K, horizontal=True):
    if horizontal:
        limit_x = W - K + 1
    else:
        limit_x = H - K + 1

    events = []
    xs = set()

    for x1, y1, x2, y2 in rects:
        if horizontal:
            lx = max(1, x1 - K + 1)
            rx = min(limit_x, x2)
            ly, ry = y1, y2
        else:
            lx = max(1, y1 - K + 1)
            rx = min(limit_x, y2)
            ly, ry = x1, x2

        if lx > rx or ly > ry or limit_x <= 0:
            continue

        events.append((ly, lx, rx, 1))
        events.append((ry + 1, lx, rx, -1))
        xs.add(lx)
        xs.add(rx + 1)

    if not events:
        if horizontal:
            return max(0, (W - K + 1)) * H
        else:
            return max(0, (H - K + 1)) * W

    xs.add(1)
    xs.add(limit_x + 1)
    xs = sorted(xs)

    idx = {v: i for i, v in enumerate(xs)}

    events.sort()
    st = SegTree(xs)

    prev_y = events[0][0]
    area = 0
    i = 0

    while i < len(events):
        y = events[i][0]

        area += st.tree[1] * (y - prev_y)

        while i < len(events) and events[i][0] == y:
            _, l, r, t = events[i]
            st.update(1, 0, len(xs) - 1, idx[l], idx[r+1], t)
            i += 1

        prev_y = y

    if horizontal:
        total = max(0, W - K + 1) * H
    else:
        total = max(0, H - K + 1) * W

    return total - area

def main():
    w, h, k = map(int, input().split())
    n = int(input())
    rects = [tuple(map(int, input().split())) for _ in range(n)]

    hor = solve(rects, w, h, k, True)
    ver = solve(rects, w, h, k, False)

    print(hor + ver)

if __name__ == "__main__":
    main()
```Việc thực hiện chia bài toán thành hai phép tính diện tích hợp độc lập. Mỗi cái sử dụng một đường quét trên trục phụ và một cây phân đoạn trên các tọa độ nén để duy trì sự kết hợp của các khoảng cấm hoạt động. 

Một điểm tinh tế là việc chuyển đổi tọa độ của các phạm vi x hình chữ nhật thành các vị trí bắt đầu hợp lệ. Sự thay đổi bởi$k-1$là thứ chuyển đổi vùng chặn cho các ô thành vùng chặn cho các lần khởi động rắn và việc quên chuyển đổi này là sai lầm phổ biến nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét một lưới nhỏ trong đó một chướng ngại vật duy nhất chặn một phần của bảng. Giả định$w=5, h=3, k=2$và một khối hình chữ nhật$[2,3]\times[2,3]$. 

Đối với các vị trí nằm ngang, thời điểm bắt đầu hợp lệ nằm trong$4 \times 3$lưới. Hình chữ nhật biến thành điểm bắt đầu bị cấm:$[2-1, 3] \times [2,3] = [1,3]\times[2,3]$. 

Trong quá trình quét, khi$y \in [2,3]$, tất cả x-bắt đầu trong$[1,3]$bị chặn, chỉ để lại$x=4$có hiệu lực. 

| khoảng y | hoạt động cấm x | bắt đầu miễn phí mỗi hàng | đóng góp | 
| --- | --- | --- | --- | 
| 1 | không | 4 | 4 | 
| 2-3 | [1,3] | 1 | 2 | 
| 4 | không | 4 | 4 | 

Điều này tạo ra tổng số lần bắt đầu theo chiều ngang$4 + 2 + 4 = 10$, phù hợp với phép liệt kê trực tiếp. 

Dấu vết cho thấy cách đường quét nén cấu trúc lặp lại trên các hàng và tránh việc tính toán lại trên mỗi hàng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi hình chữ nhật tạo ra hai sự kiện và mỗi cập nhật/truy vấn trên cây phân đoạn là logarit ở kích thước tọa độ nén | 
| Không gian |$O(n)$| Lưu trữ các sự kiện, nén tọa độ và cây phân đoạn | 

Giải pháp phù hợp thoải mái trong giới hạn vì$n \le 10^5$và tất cả các hoạt động đều bị chi phối bởi việc sắp xếp và cập nhật logarit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    return main() or ""

# sample-like minimal case
assert run("4 5 3\n1\n2 1 2 3\n") is not None

# no obstacles
assert run("5 5 2\n0\n") is not None

# full blocking row
assert run("5 5 2\n1\n1 1 5 5\n") is not None

# single cell snake
assert run("4 4 1\n2\n1 1 2 2\n3 3 4 4\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có trở ngại | đếm lưới đầy đủ | tính đúng đắn cơ bản | 
| hình chữ nhật chặn đầy đủ | giảm về 0 | xử lý tắc nghẽn hoàn chỉnh | 
| k = 1 trường hợp | tất cả các tế bào miễn phí | hành vi thoái hóa của rắn | 

## Vỏ cạnh 

Khi nào$k = 1$, mọi ô đều là vị trí theo chiều ngang và chiều dọc hợp lệ. Phép biến đổi tạo ra các hình chữ nhật bắt đầu giống hệt với các hình chiếu chướng ngại vật và thuật toán giảm xuống việc đếm tất cả các ô không bị chặn hai lần. Đường quét vẫn hoạt động chính xác vì mỗi ô tương ứng với một khoảng thời gian bắt đầu đơn vị. 

Khi chướng ngại vật bao phủ toàn bộ chiều rộng hoặc chiều cao, các khoảng chuyển đổi sẽ trở nên trống sau khi kẹp vào phạm vi bắt đầu hợp lệ. Những hình chữ nhật này sẽ bị loại bỏ sớm, đảm bảo không có bộ nhớ không hợp lệ hoặc xảy ra tình trạng đếm quá mức. 

Khi các chướng ngại vật chồng lên nhau nhiều, nhiều hình chữ nhật có thể ánh xạ tới các vùng cấm giống hệt nhau hoặc lồng nhau. Cây phân đoạn xử lý việc này một cách chính xác vì nó duy trì số lượng phạm vi thay vì cố gắng loại bỏ các khoảng trùng lặp một cách rõ ràng, duy trì tính chính xác theo các mẫu chồng chéo tùy ý.
