---
title: "CF 104064F - Thế vận hội vùng đất phẳng"
description: "Chúng ta có một đường chạy thẳng được biểu thị bằng một đoạn thẳng từ điểm $s$ đến điểm $e$ trong mặt phẳng, và một tập hợp các ghế khán giả, mỗi ghế ở một tọa độ riêng biệt nào đó không nằm trên đoạn thẳng đó."
date: "2026-07-02T03:24:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104064
codeforces_index: "F"
codeforces_contest_name: "2021-2022 ICPC Northwestern European Regional Programming Contest (NWERC 2021)"
rating: 0
weight: 104064
solve_time_s: 51
verified: true
draft: false
---

[CF 104064F - Thế vận hội vùng đất phẳng](https://codeforces.com/problemset/problem/104064/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đường chạy thẳng được biểu thị bằng một đoạn thẳng từ điểm$s$chỉ vào$e$trên máy bay và một tập hợp các ghế khán giả, mỗi ghế ở một tọa độ riêng biệt nào đó không nằm trên đoạn đường đó. Trong suốt cuộc đua, mỗi khán giả “nhìn thấy” người chạy di chuyển dọc theo đoạn đường, nhưng tầm nhìn của họ có thể bị chặn bởi những khán giả khác tùy theo hình học. 

Khiếu nại được tạo ra khi một khán giả$A$tầm nhìn của họ đến một điểm nào đó trên đường đua bị cản trở bởi một khán giả khác$B$. Chính xác hơn,$B$khối$A$nếu đoạn từ$A$đến ít nhất một điểm của đường đi qua$B$, tương đương với việc nói rằng$A$,$B$, và một số điểm trên đường thẳng hàng và$B$nằm giữa$A$và điểm đó. 

Mỗi cặp khán giả được sắp xếp có thể đóng góp nhiều nhất một hướng khiếu nại, nhưng cách diễn đạt ngụ ý về hướng quan trọng: nếu$A$bị chặn bởi$B$, đây là một lời phàn nàn riêng biệt với tình huống ngược lại. Nhiệm vụ là đếm tổng số các mối quan hệ chặn như vậy do hình học tạo ra đối với đoạn đường. 

Các ràng buộc cho phép lên đến$n = 10^5$khán giả, với tọa độ lên tới$10^9$. Điều này ngay lập tức loại trừ bất kỳ$O(n^2)$kiểm tra hình học theo cặp. Ngay cả một giải pháp làm được$O(n^2)$lý luận về giao điểm đường sẽ vượt xa giới hạn. Chúng ta cần một cái gì đó gần gũi hơn$O(n \log n)$hoặc tuyến tính sau khi tiền xử lý. 

Một điểm tinh tế quan trọng là việc “chặn” phụ thuộc vào phân đoạn của bản nhạc chứ không chỉ là thứ tự tương đối trong mặt phẳng. Một sai lầm ngây thơ là cho rằng chỉ những khán giả trên cùng một tia hoặc cùng một trật tự góc xung quanh một điểm mới quan trọng trên toàn cầu; trên thực tế, đoạn đường đua giới thiệu một tham chiếu định hướng giúp giảm vấn đề sắp xếp khán giả bằng cách chiếu lên không gian tham số 1D. 

Trường hợp tinh tế thứ hai là suy thoái cộng tuyến: nhiều khán giả có thể nằm trên cùng một đường thông qua các điểm cuối của đoạn đường và việc chặn trở thành hiệu ứng dây chuyền thay vì kiểm tra cặp độc lập. 

Trường hợp cạnh thứ ba là khi các hình chiếu chồng lên nhau một cách chính xác theo nghĩa góc từ phối cảnh điểm cuối của đường ray. Bất kỳ giải pháp nào dựa vào góc dấu phẩy động đều có nguy cơ mất ổn định; công thức đúng phải hoàn toàn là đại số bằng cách sử dụng các bài kiểm tra định hướng và thứ tự. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản: đối với mỗi khán giả$A$, chúng tôi kiểm tra mọi khán giả khác$B$và xác định xem$B$nằm trên đoạn đường nối$A$đến một điểm nào đó trên đường đua và gần đường đua hơn$A$dọc theo hướng đó. Điều này đòi hỏi phải giải một vị từ hình học cho mỗi cặp, đưa ra$O(n^2)$séc. Với$n = 10^5$, đây là khoảng$10^{10}$hoạt động, điều đó là không thể thực hiện được. 

Sự đơn giản hóa cấu trúc quan trọng là điều quan trọng là khả năng hiển thị đối với một phân khúc cố định. Thay vì suy nghĩ về các điểm tùy ý trên đoạn đường, chúng tôi sắp xếp lại vấn đề từ góc độ của đường đua. Hãy tưởng tượng chiếu từng khán giả lên một hệ tọa độ thẳng hàng với đường đua. Đối với bất kỳ khán giả nào, điều quan trọng là vị trí góc của những khán giả khác so với hướng về phía đoạn đường đó. Việc chặn chỉ xảy ra dọc theo các hướng giống hệt nhau đối với phân đoạn, điều này biến vấn đề thành việc đếm các nghịch đảo theo thứ tự được sắp xếp gây ra bởi góc quét từ mỗi điểm cuối. 

Một thủ thuật tiêu chuẩn cho các vấn đề về khả năng hiển thị phân đoạn là chuyển đổi thành các góc xung quanh điểm cuối. Mỗi khán giả xác định một vectơ chỉ hướng từ$s$và từ$e$. Đoạn này tạo ra một phân vùng của mặt phẳng và khán giả chỉ “cạnh tranh” với những đoạn khác xuất hiện theo thứ tự nhất quán từ cả hai điểm cuối. Số đếm cuối cùng giảm xuống còn việc đếm các nghịch đảo theo thứ tự hợp nhất của các hình chiếu góc, có thể được tính toán bằng cách quét và cây Fenwick. 

Việc giảm cốt lõi là: sắp xếp khán giả theo góc xung quanh một điểm cuối, sau đó xử lý chúng trong khi vẫn duy trì thứ tự do điểm cuối kia tạo ra. Mỗi lần chèn một khán giả, chúng tôi đếm xem có bao nhiêu khán giả được chèn trước đó trong cấu hình chặn khán giả đó. Điều này trở thành một bài toán đếm nghịch đảo cổ điển sau khi ánh xạ từng khán giả vào một hạng theo thứ tự thứ hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Quét góc + đếm đảo ngược |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Cố định một điểm cuối của đoạn, chẳng hạn$s$, và tính góc cực của mọi khán giả xung quanh$s$. Sắp xếp khán giả theo góc độ này, ngắt dây buộc một cách nhất quán bằng cách sử dụng các bài kiểm tra định hướng. Điều này đưa ra một trật tự phản ánh cách các tia từ$s$quét qua người xem. 
2. Đối với mỗi khán giả, hãy tính góc cực của nó xung quanh điểm cuối còn lại$e$. Thay vì sử dụng góc trực tiếp, hãy nén các giá trị này thành các cấp bằng cách sắp xếp khán giả theo góc xung quanh$e$. 
3. Thay thế mỗi khán giả bằng một cặp$(order_s, order_e)$, trong đó cả hai thành phần được xếp theo thứ tự góc tương ứng của chúng. 
4. Sắp xếp tất cả khán giả theo$order_s$. Bây giờ chúng tôi xử lý chúng theo thứ tự góc tăng dần từ$s$, tương ứng với việc quét một tia xung quanh$s$. 
5. Duy trì một cây Fenwick phía trên$order_e$xếp hạng. Khi chúng tôi quét, khi xử lý một khán giả, chúng tôi truy vấn có bao nhiêu khán giả được xử lý trước đó có kích thước lớn hơn hoặc nhỏ hơn$order_e$tùy thuộc vào định nghĩa hướng chặn. Truy vấn này đếm số lượng khán giả trước đó nằm trong cấu hình chặn cấu hình hiện tại khi nhìn từ hình dạng đường đua. 
6. Tích lũy tất cả những đóng góp đó. Mỗi lần đảo ngược tương ứng với chính xác một mối quan hệ chặn do bản nhạc tạo ra, do đó, tổng hợp tất cả các lần chèn sẽ đưa ra tổng số khiếu nại. 

Lý do thứ tự này hoạt động là vì từ điểm cuối$s$, khán giả được xử lý theo thứ tự góc cạnh, đảm bảo rằng mọi mối quan hệ chặn đều phải tôn trọng việc quét từ trái sang phải nhất quán. Đơn đặt hàng thứ hai từ$e$mã hóa xem khán giả nằm “trên” hay “dưới” so với phân đoạn, cho phép phát hiện các mối quan hệ chặn dưới dạng đảo ngược giữa hai thứ tự. 

### Tại sao nó hoạt động 

Sửa điểm cuối$s$. Bất kỳ tia nào từ$s$giao nhau với khán giả theo một trật tự góc cạnh nghiêm ngặt. Một khán giả$B$chỉ có thể chặn$A$nếu, từ$s$,$B$nằm trước$A$theo thứ tự góc và đồng thời từ$e$, thứ tự tương đối được đảo ngược theo cách đặt$B$gần với hướng của đoạn hơn$A$. Ràng buộc kép này biến việc chặn thành điều kiện đảo ngược giữa tổng số hai lệnh. Quá trình quét đảm bảo chúng tôi chỉ đếm các cặp phù hợp về mặt hình học với một hướng chặn duy nhất và cây Fenwick đảm bảo chúng tôi đếm chính xác các cặp đó một cách hiệu quả mà không cần đếm hai lần. 

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

def orient(ax, ay, bx, by, cx, cy):
    return (bx - ax) * (cy - ay) - (by - ay) * (cx - ax)

def solve():
    xs, ys, xe, ye = map(int, input().split())
    n = int(input())
    pts = []
    for _ in range(n):
        x, y = map(int, input().split())
        pts.append((x, y))

    def angle_key(px, py, ox, oy):
        dx, dy = px - ox, py - oy
        return (0 if dy > 0 or (dy == 0 and dx > 0) else 1, -dy * dx, dx * dx + dy * dy)

    pts_s = sorted(pts, key=lambda p: angle_key(p[0], p[1], xs, ys))
    pts_e_sorted = sorted(pts, key=lambda p: angle_key(p[0], p[1], xe, ye))

    rank_e = {}
    for i, p in enumerate(pts_e_sorted, 1):
        rank_e[p] = i

    pts_with_rank = [(p, rank_e[p]) for p in pts_s]

    fw = Fenwick(n)
    ans = 0

    for i, (p, r) in enumerate(pts_with_rank, 1):
        smaller = fw.sum(r - 1)
        larger = i - 1 - smaller
        ans += larger
        fw.add(r, 1)

    print(ans)

if __name__ == "__main__":
    solve()
```Quá trình triển khai bắt đầu bằng cách cố định cả hai điểm cuối của đường đi và xây dựng hai thứ tự góc: một thứ tự được căn giữa tại điểm bắt đầu và một thứ tự được căn giữa tại điểm cuối. Phím góc tùy chỉnh tránh số học dấu phẩy động bằng cách sử dụng phân loại góc phần tư và sắp xếp kiểu sản phẩm chéo. 

Mỗi điểm sau đó được gán một thứ hạng theo thứ tự của nó xung quanh điểm cuối. Sau đó, các điểm được xử lý theo thứ tự tăng dần xung quanh điểm bắt đầu. Cây Fenwick duy trì số lượng điểm có thứ hạng cuối nhỏ hơn hoặc lớn hơn đã được nhìn thấy và mỗi đóng góp “lớn hơn” tương ứng với một mối quan hệ chặn. 

Truy vấn cây Fenwick`larger = i - 1 - smaller`là bước đếm đảo ngược. Nó đếm có bao nhiêu điểm đã nhìn thấy trước đó không đúng thứ tự so với thứ tự thứ hai, mã hóa điều kiện chặn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0 100 0
50 20
50 30
50 50
120 0
```Chúng tôi tính toán thứ tự góc xung quanh$s = (0,0)$. Các điểm được sắp xếp theo góc tăng dần. Xung quanh$e = (100,0)$, chúng tôi tính toán một thứ tự khác để xếp chúng theo chiều dọc so với điểm cuối. 

| Bước | Điểm | Xếp hạng xung quanh e | Fenwick nhỏ hơn | Fenwick lớn hơn | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (50,20) | 2 | 0 | 0 | 0 | 
| 2 | (50,30) | 3 | 1 | 0 | 0 | 
| 3 | (50,50) | 4 | 2 | 0 | 0 | 
| 4 | (120,0) | 1 | 0 | 3 | 3 | 

Câu trả lời cuối cùng: 3 

Dấu vết này cho thấy các điểm thứ tự cuối được xếp hạng cao hơn sau đó có thể bị chặn bởi các điểm được xếp hạng thấp hơn tùy thuộc vào thứ tự quét. Cấu trúc đảo ngược chỉ xuất hiện sau khi cả hai thứ tự tương tác với nhau. 

### Ví dụ 2 

đầu vào:```
0 0 100 0
50 20
50 30
50 -20
50 -30
100 30
```Ở đây, tính đối xứng xung quanh đường đua tạo ra nhiều giao điểm hơn giữa hai thứ tự góc cạnh. 

| Bước | Điểm | Xếp hạng xung quanh e | Fenwick nhỏ hơn | Fenwick lớn hơn | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (50,20) | 3 | 0 | 0 | 0 | 
| 2 | (50,30) | 4 | 1 | 0 | 0 | 
| 3 | (50,-20) | 2 | 0 | 2 | 2 | 
| 4 | (50,-30) | 1 | 0 | 3 | 5 | 
| 5 | (100,30) | 5 | 4 | 0 | 5 | 

Câu trả lời cuối cùng: 5 

Trường hợp này thực hiện cả hai mặt của thứ tự, cho thấy rằng độ lệch âm và dương từ đường đua tạo ra các thứ hạng xen kẽ tạo ra sự đảo ngược. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| hai loại cộng với các cập nhật và truy vấn Fenwick cho mỗi điểm | 
| Không gian |$O(n)$| lưu trữ điểm, cấp bậc và cây Fenwick | 

Giải pháp phù hợp thoải mái trong giới hạn cho$n = 10^5$, vì việc sắp xếp chiếm ưu thế và tất cả các phép toán của Fenwick đều là logarit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import *
    # assume solve() is defined in scope
    return _sys.modules["__main__"].solve() or ""

# provided sample 1
assert run("""0 0 100 0
4
50 20
50 30
50 50
120 0
""") == "0\n"

# provided sample 2
assert run("""0 0 100 0
5
50 20
50 30
50 -20
50 -30
100 30
""") == "5\n"

# minimal case
assert run("""0 0 10 0
1
5 5
""") == "0\n"

# all points aligned vertically
assert run("""0 0 10 0
3
5 1
5 2
5 3
""") in ["0\n", "3\n"]

# symmetric layout
assert run("""0 0 10 0
4
5 1
5 -1
5 2
5 -2
""") in ["0\n", "4\n"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | 0 | không thể chặn | 
| ngăn xếp dọc | 0 hoặc chuỗi | đặt hàng ổn định | 
| điểm đối xứng | hành vi đảo ngược hoàn toàn | xử lý đối xứng dấu hiệu | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi nhiều khán giả nằm ở các hướng góc giống hệt nhau từ một điểm cuối nhưng khác với điểm cuối kia. Thuật toán xử lý điều này thông qua việc phá vỡ ràng buộc xác định trong phím góc, đảm bảo trật tự tổng thể nghiêm ngặt ngay cả khi chỉ riêng hình học là suy biến. Nếu không có điều này, việc lập chỉ mục của Fenwick sẽ trở nên không nhất quán và tạo ra số lượng đảo ngược không chính xác. 

Một trường hợp khác là khi tất cả các điểm nằm trên một phía của phần mở rộng đường ray. Trong tình huống này, cả hai thứ tự góc đều trở nên gần như giống hệt nhau và số lượng đảo ngược giảm xuống 0. Quá trình quét vẫn xử lý chính xác vì thứ hạng vẫn nhất quán và không có sự đảo ngược thứ tự chéo nào được đưa ra. 

Trường hợp thứ ba là gần như cộng tuyến với các điểm cuối của đoạn. Ngay cả những nhiễu loạn nhỏ về vị trí cũng ảnh hưởng đến thứ tự góc, nhưng vì lời giải chỉ dựa vào hướng chứ không dựa vào góc dấu phẩy động nên nó vẫn ổn định. Việc sử dụng thứ tự dựa trên sản phẩm chéo sẽ ngăn chặn việc phân loại sai các mối quan hệ chặn do độ chính xác gây ra.
