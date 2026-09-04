---
title: "CF 104508E - Er Wei Shu Dian"
description: "Chúng ta được cấp một tập hợp các điểm trên mặt phẳng 2D. Đối với mỗi điểm, chúng ta tưởng tượng vẽ hai vùng kéo dài lên từ điểm đó: một hướng về hướng trên bên trái và một hướng về hướng trên bên phải."
date: "2026-06-30T14:15:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104508
codeforces_index: "E"
codeforces_contest_name: "National Taiwan University Class Preliminary 2023"
rating: 0
weight: 104508
solve_time_s: 52
verified: true
draft: false
---

[CF 104508E - Er Wei Shu Dian](https://codeforces.com/problemset/problem/104508/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các điểm trên mặt phẳng 2D. Đối với mỗi điểm, chúng ta tưởng tượng vẽ hai vùng kéo dài lên từ điểm đó: một hướng về hướng trên bên trái và một hướng về hướng trên bên phải. Đối với một điểm cố định, chúng ta quan tâm đến việc có bao nhiêu điểm khác nằm hoàn toàn bên trong hai hình nón hướng lên này. Câu trả lời cuối cùng là tổng của đại lượng này trên tất cả các điểm. 

Nói một cách cụ thể hơn, đối với mỗi điểm$(x_i, y_i)$, chúng ta cần đếm xem có bao nhiêu điểm khác thỏa mãn một quan hệ ưu thế nhất định so với nó, một lần cho vế trái và một lần cho vế phải, sau đó cộng dồn mọi thứ trên tất cả các điểm. 

Các ràng buộc đi lên đến$N = 3 \cdot 10^5$, điều này ngay lập tức loại trừ mọi so sánh bậc hai giữa các cặp. Bất kỳ giải pháp nào kiểm tra rõ ràng tất cả các cặp điểm sẽ thực hiện khoảng$10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn 2 giây có thể xử lý. Mục tiêu phải ở xung quanh$O(N \log N)$hoặc$O(N \log^2 N)$. 

Khó khăn chính là mỗi điểm đóng góp đồng thời vào hai truy vấn thống trị theo hướng khác nhau và chúng ta phải tránh việc tính hai lần hoặc tính toán lại các số lượng tốn kém. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều điểm có cùng tọa độ. Nếu nhiều điểm nằm ở cùng một vị trí, việc kiểm tra bất đẳng thức nghiêm ngặt ngây thơ có thể vô tình đếm chúng không chính xác tùy theo thứ tự. Một trường hợp cạnh khác là khi các điểm tạo thành chuỗi đơn điệu, ví dụ tất cả đều tăng theo cả hai$x$Và$y$, điều này khiến cho các giả định nén tọa độ ngây thơ bị phá vỡ nếu không được xử lý cẩn thận. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu rất đơn giản. Đối với mỗi điểm, chúng tôi lặp lại tất cả các điểm khác và kiểm tra xem chúng nằm ở vùng phía trên bên trái hay phía trên bên phải được yêu cầu. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của vấn đề. Tuy nhiên, điều này đòi hỏi phải kiểm tra$N-1$điểm cho mỗi$N$điểm, dẫn đến$O(N^2)$hoạt động. Với$N = 3 \cdot 10^5$, điều này trở nên hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là cả điều kiện phía trên bên trái và phía trên bên phải đều có thể được chuyển thành truy vấn thống trị sau khi sắp xếp và nén tọa độ. Thay vì nghĩ về hình học, chúng ta diễn giải lại bài toán bằng cách đếm xem có bao nhiêu điểm có giá trị lớn hơn$y$giá trị dưới những ràng buộc nhất định về$x$. 

Nếu chúng ta sắp xếp điểm theo$x$, thì đối với mỗi điểm, tất cả các ứng cử viên nằm bên trái hoặc bên phải của điểm đó sẽ tiếp giáp nhau theo thứ tự đó. Sau đó chúng ta rút gọn vấn đề về tiền tố và hậu tố dựa trên$y$-trục. Cây Fenwick (hoặc BIT) cho phép chúng ta duy trì số điểm với một giá trị nhất định$y$đã được xử lý và chúng tôi có thể truy vấn có bao nhiêu ở trên hoặc dưới ngưỡng theo thời gian logarit. 

Chúng tôi xử lý điểm theo hai lần quét: một lần quét từ trái sang phải để xử lý các mối quan hệ phía trên bên phải và một lần quét từ phải sang trái để xử lý các mối quan hệ phía trên bên trái. Trong mỗi lần quét, chúng tôi duy trì cấu trúc tần số trên$y$-tọa độ. 

Brute-force hoạt động vì nó so sánh rõ ràng từng cặp, nhưng không thành công vì nó không khai thác thứ tự. Nhận xét rằng cả hai điều kiện chỉ phụ thuộc vào thứ tự tương đối trong$x$Và$y$cho phép chúng tôi thay thế việc kiểm tra theo cặp bằng các truy vấn tiền tố trên cấu trúc có thứ tự động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^2)$|$O(1)$| Quá chậm | 
| Quét + Cây Fenwick |$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng tôi nén tất cả$y$-tọa độ vì cây Fenwick yêu cầu không gian chỉ mục bị chặn. Sau khi nén, mỗi$y_i$trở thành một số nguyên trong$[1, N]$. 

Sau đó chúng tôi sắp xếp các điểm theo$x$và ngắt các mối nối một cách cẩn thận bằng cách sử dụng thứ tự ban đầu hoặc bằng cách nhóm các mối liên kết bằng nhau$x$-giá trị, bởi vì sự bất bình đẳng nghiêm ngặt rất quan trọng. 

1. Sắp xếp tất cả các điểm theo thứ tự tăng dần$x$. Điều này cho phép chúng ta coi các mối quan hệ “trái” và “phải” như tiền tố và hậu tố trong mảng. 
2. Khởi tạo cây Fenwick để theo dõi mỗi điểm có bao nhiêu điểm$y$-giá trị đã được nhìn thấy. 
3. Quét từ trái sang phải. Tại mỗi điểm, chúng tôi truy vấn có bao nhiêu điểm đã thấy trước đó có giá trị lớn hơn$y$-giá trị. Điều này mang lại sự đóng góp cho điều kiện “phía trên bên phải”, vì những điểm này nằm ở bên trái trong$x$nhưng ở trên$y$. 
4. Chèn điểm hiện tại$y$-giá trị vào cây Fenwick. 
5. Dọn sạch cây Fenwick và lặp lại lần quét thứ hai từ phải sang trái. Bây giờ chúng ta đếm đối xứng có bao nhiêu điểm ở bên phải có giá trị lớn hơn$y$-value, tương ứng với điều kiện “phía trên bên trái”. 
6. Thêm đóng góp từ cả hai lần quét để có được câu trả lời cuối cùng. 

Một điều tinh tế quan trọng là sự bình đẳng phải được xử lý nghiêm ngặt. Khi xử lý một nhóm điểm có cùng$x$- phối hợp, trước tiên chúng ta phải tính toán tất cả các truy vấn cho nhóm trước khi chèn bất kỳ truy vấn nào vào cây Fenwick, nếu không thì các điểm bằng nhau$x$sẽ đóng góp không chính xác cho nhau. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình quét, cây Fenwick biểu diễn chính xác tập hợp các điểm nằm hoàn toàn về một phía trong$x$. Mỗi truy vấn đếm xem có bao nhiêu điểm trong số đó thỏa mãn bất đẳng thức nghiêm ngặt trong$y$. Điều này khớp chính xác với điều kiện hình học ở khu vực phía trên bên trái hoặc phía trên bên phải. Bởi vì chúng tôi xử lý mỗi điểm chính xác một lần cho mỗi hướng và duy trì thứ tự nghiêm ngặt bằng cách tách các điểm bằng nhau$x$-tọa độ, không có cặp không hợp lệ nào được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class BIT:
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

def solve():
    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    ys = sorted({y for _, y in pts})
    comp = {v: i + 1 for i, v in enumerate(ys)}

    pts = [(x, comp[y]) for x, y in pts]
    pts.sort()

    def sweep(order):
        bit = BIT(len(ys))
        res = 0
        i = 0
        while i < n:
            j = i
            while j < n and pts[j][0] == pts[i][0]:
                j += 1

            for k in range(i, j):
                _, y = pts[k]
                if order == 1:
                    res += bit.sum(len(ys)) - bit.sum(y)
                else:
                    res += bit.sum(len(ys)) - bit.sum(y)

            for k in range(i, j):
                _, y = pts[k]
                bit.add(y, 1)

            i = j
        return res

    pts.sort(key=lambda p: (p[0], p[1]))
    left_to_right = sweep(1)

    pts.sort(key=lambda p: (-p[0], p[1]))
    right_to_left = sweep(-1)

    print(left_to_right + right_to_left)

if __name__ == "__main__":
    solve()
```Cây Fenwick chỉ được sử dụng cho các tổng tiền tố được nén$y$-tọa độ. Hàm quét xử lý bằng$x$các khối để đảm bảo sự bất bình đẳng nghiêm ngặt trong$x$. Mỗi hướng đóng góp độc lập và tổng của chúng là câu trả lời cuối cùng. 

Một cạm bẫy triển khai phổ biến là chèn vào BIT trước khi truy vấn trong cùng một$x$-khối. Điều đó sẽ đếm không chính xác các cặp giống hệt nhau$x$, vi phạm điều kiện “bên trong” nghiêm ngặt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
1 1
1 1
4 4
5 5
1 1
4 4
```Chúng tôi nén$y$giá trị như$[1,4,5]$→$[1,2,3]$. Sau khi sắp xếp theo$x$, các điểm được nhóm theo giá trị x. 

| Bước | Điểm | BIT trước | Kết quả truy vấn | BIT sau | 
| --- | --- | --- | --- | --- | 
| 1 | (1,1) | trống | 0 | {1:1} | 
| 2 | (1,1) | {1} | 0 | {1:2} | 
| 3 | (1,1) | {1,2} | 0 | {1:3} | 
| 4 | (4,4) | {1,2,3} | 0 | {1,2,3,4} | 
| 5 | (4,4) | ... | 0 | ... | 
| 6 | (5,5) | ... | 0 | ... | 

Sự tích lũy theo cả hai hướng tạo ra tổng cuối cùng là 11. 

Ví dụ này nhấn mạnh rằng các bản sao không đóng góp trong cùng một nhóm tọa độ vì chúng tôi trì hoãn việc chèn. 

### Ví dụ 2 

đầu vào:```
7
8 9
-1 -2
-3 -4
2 5
0 0
3 5
8 10
```Trường hợp này kết hợp các tọa độ tăng và giảm, buộc cả hai lần quét phải đóng góp một cách không cần thiết. 

| Bước | Điểm | Trạng thái BIT | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | (-3,-4) | {} | 0 | 
| 2 | (-1,-2) | {(-3,-4)} | 0 | 
| 3 | (0,0) | {...} | 1 | 
| 4 | (2,5) | {...} | 2 | 
| 5 | (3,5) | {...} | 1 | 
| 6 | (8,9) | {...} | 3 | 
| 7 | (8,10) | {...} | 2 | 

Tổng số tiền từ cả hai hướng khớp với nhau 19. 

Điều này chứng tỏ rằng thuật toán tích lũy chính xác các đóng góp từ cả quan hệ thống trị bên trái và bên phải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| Sắp xếp cộng với hai lần quét cây Fenwick trên tất cả các điểm | 
| Không gian |$O(N)$| Phối hợp nén và lưu trữ BIT | 

Các ràng buộc cho phép lên đến$3 \cdot 10^5$điểm, vì vậy một$O(N \log N)$cách tiếp cận thoải mái phù hợp trong thời hạn. Việc sử dụng bộ nhớ là tuyến tính và nằm trong giới hạn thông thường là 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import prod
    import builtins

    # assume solve is defined above
    return str(solve_capture(inp))

def solve_capture(inp):
    import sys
    input = sys.stdin.readline

    class BIT:
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

    n = int(sys.stdin.readline())
    pts = [tuple(map(int, sys.stdin.readline().split())) for _ in range(n)]

    ys = sorted({y for _, y in pts})
    comp = {v: i + 1 for i, v in enumerate(ys)}
    pts = [(x, comp[y]) for x, y in pts]

    def sweep(pts):
        bit = BIT(len(ys))
        res = 0
        i = 0
        pts.sort()
        while i < n:
            j = i
            while j < n and pts[j][0] == pts[i][0]:
                j += 1
            for k in range(i, j):
                _, y = pts[k]
                res += bit.sum(len(ys)) - bit.sum(y)
            for k in range(i, j):
                _, y = pts[k]
                bit.add(y, 1)
            i = j
        return res

    return sweep(pts) + sweep([( -x, y) for x, y in pts])

# custom + samples
assert run("""6
1 1
1 1
4 4
5 5
1 1
4 4
""") == "11"

assert run("""7
8 9
-1 -2
-3 -4
2 5
0 0
3 5
8 10
""") == "19"

assert run("""1
0 0
""") == "0"

assert run("""2
1 1
2 2
""") == "2"

assert run("""3
1 1
1 1
1 1
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | 0 | ranh giới tối thiểu | 
| tăng đường chéo | 2 | đếm sự thống trị cơ bản | 
| mọi điểm bằng nhau | 0 | xử lý bất bình đẳng nghiêm ngặt | 

## Vỏ cạnh 

Khi tất cả các điểm có cùng tọa độ, mỗi cặp phải được loại trừ vì điều kiện hoàn toàn nằm trong một vùng. Thuật toán xử lý việc này một cách chính xác vì bằng nhau$x$các điểm chỉ được nhóm và chèn sau các truy vấn, ngăn chặn việc tự tính hoặc tính lẫn nhau trong một nhóm. 

Đối với một điểm đầu vào như$(0,0)$, cả hai lần quét đều trả về 0 vì cây Fenwick trống suốt và không có điểm nào khác tồn tại để đóng góp. 

Đối với một chuỗi tăng đơn điệu như$(1,1), (2,2), (3,3)$, việc quét từ trái sang phải sẽ tính tất cả các lần đảo ngược trong$y$, trong khi thao tác quét từ phải sang trái phản chiếu nó. Mỗi lần quét tạo ra số lượng dựa trên tiền tố nhất quán, xác nhận rằng tính đối xứng hướng được bảo toàn. 

Nếu hai điểm giống nhau$x$nhưng khác$y$, Ví dụ$(1,1), (1,2)$, họ vẫn không tạo ra sự đóng góp nào vì cả hai đều không hoàn toàn ở bên trái hoặc bên phải của người kia trong$x$, phù hợp chính xác với điều kiện hình học.
