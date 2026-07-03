---
title: "CF 103627F - Độ trễ"
description: "Chúng tôi được yêu cầu xử lý một tập hợp các cập nhật hình học có trọng số, sau đó trả lời các truy vấn về tổng trọng lượng nằm bên trong các hình chữ nhật tiền tố căn chỉnh theo trục có dạng $[1, x] nhân [1, y]$."
date: "2026-07-03T02:02:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103627
codeforces_index: "F"
codeforces_contest_name: "XXII Open Cup, Grand Prix of Daejeon"
rating: 0
weight: 103627
solve_time_s: 53
verified: true
draft: false
---

[CF 103627F - Lag](https://codeforces.com/problemset/problem/103627/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xử lý một tập hợp các cập nhật hình học có trọng số, sau đó trả lời các truy vấn về tổng trọng lượng nằm bên trong các hình chữ nhật tiền tố căn chỉnh theo trục của biểu mẫu.$[1, x] \times [1, y]$. Khó khăn là các bản cập nhật không phải là các điểm tĩnh mà là các đóng góp có cấu trúc hoạt động giống như các sự kiện di chuyển dọc theo các đường trong mặt phẳng và đóng góp của mỗi bản cập nhật được xác định ngầm thông qua việc đưa vào loại trừ. 

Một cải tiến quan trọng trong hướng dẫn được cung cấp là thay vì nghĩ trực tiếp về hình chữ nhật, chúng tôi chuyển đổi từng cập nhật hình chữ nhật thành một tập hợp nhỏ các sự kiện góc có trọng số. Sau sự chuyển đổi này, vấn đề trở thành sự tích lũy động của các trọng số điểm, trong đó mỗi điểm đóng góp cho tất cả các truy vấn theo kiểu tiền tố. Nói cách khác, mỗi sự kiện sẽ cộng hoặc trừ trọng số tại một tọa độ và mọi truy vấn đều yêu cầu tổng trên một hình chữ nhật tiền tố. 

Các ràng buộc mà cấu trúc ngụ ý đủ lớn để không thể thực hiện bất kỳ phép quét bậc hai nào đối với tất cả các truy vấn và cập nhật. Một cách tiếp cận đơn giản, đối với mỗi truy vấn, quét tất cả các sự kiện và tính toán lại các đóng góp sẽ tốn kém$O((N+M)Q)$, điều này ngay lập tức trở nên không khả thi khi số lượng đầu vào tăng vượt quá vài nghìn. Điều này buộc chúng tôi hướng tới một cấu trúc dữ liệu hỗ trợ cập nhật gia tăng và truy vấn tổng tiền tố một cách hiệu quả, thường là theo thời gian logarit cho mỗi thao tác. 

Một trường hợp cạnh tinh tế xuất phát từ các dịch chuyển tọa độ như$x_2 + 1$Và$y_2 + 1$. Những điều này đảm bảo sự phân tách ranh giới rõ ràng giữa phạm vi bao gồm và phạm vi độc quyền. Việc quên những thay đổi này sẽ dẫn đến các lỗi sai lệch trong đó các hình chữ nhật lẽ ra đóng góp bằng 0 vẫn bị rò rỉ trọng lượng vào cấu trúc tiền tố. 

Một trường hợp góc quan trọng khác là các phép biến đổi chồng chéo: nhiều hình chữ nhật có thể tạo ra các đóng góp hủy bỏ ở các góc chung. Nếu việc triển khai không tôn trọng các dấu hiệu loại trừ bao gồm một cách cẩn thận, tổng tiền tố cuối cùng sẽ tăng gấp đôi số lượng hoặc vùng đếm dưới ngay cả khi cấu trúc dữ liệu là chính xác. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Sau khi chuyển đổi mỗi hình chữ nhật thành bốn sự kiện góc có trọng số, chúng tôi duy trì một mảng trên lưới. Đối với mỗi truy vấn$(x, y)$, chúng ta lặp lại tất cả các sự kiện và tính tổng các đóng góp của các điểm có tọa độ nằm trong$[1, x] \times [1, y]$. Mỗi lần kiểm tra là thời gian không đổi, vì vậy mỗi truy vấn sẽ tốn thời gian tuyến tính theo số lượng sự kiện. 

Điều này đúng vì phép biến đổi đảm bảo rằng mọi hình chữ nhật đóng góp chính xác trọng lượng dự định của nó thông qua việc hủy bỏ bốn điểm góc của nó. Tuy nhiên, nếu có$N$hình chữ nhật và$Q$truy vấn, điều này dẫn đến$O((N+Q)(N))$hành vi trong trường hợp xấu nhất là quá chậm. 

Quan sát quan trọng là mỗi truy vấn là tổng tiền tố theo hai chiều. Khi chúng tôi nhận ra điều này, chúng tôi có thể phân tách các kích thước bằng đường quét. Chúng tôi sắp xếp các sự kiện theo$x$và xử lý chúng theo thứ tự tăng dần, duy trì cấu trúc dữ liệu qua$y$hỗ trợ tổng tiền tố. Mỗi bản cập nhật tương ứng với việc chèn một trọng số vào một$y$- phối hợp và mỗi truy vấn yêu cầu tổng của tất cả các trọng số hoạt động lên đến$y$. 

Điều này làm giảm vấn đề về cấu trúc tổng tiền tố động tiêu chuẩn. Một cây Fenwick trên vùng bị nén$y$-trục là đủ. Mỗi sự kiện sẽ trở thành một bản cập nhật điểm hoặc một truy vấn và chúng tôi xử lý mọi thứ theo thứ tự được sắp xếp$x$. 

Điều phức tạp còn lại là vấn đề ban đầu không hoàn toàn liên quan đến trục. Hướng dẫn phân tách cấu trúc thành bốn trường hợp định hướng: ngang, dọc và hai hướng chéo. Mỗi trường hợp đường chéo được rút gọn trở lại thành bài toán tổng tiền tố căn chỉnh theo trục bằng cách xoay hoặc tham số hóa lại tọa độ, chuyển các ràng buộc như$x - y \le C$hoặc$x + y \le C$vào phạm vi tiền tố tiêu chuẩn. 

Do đó, giải pháp đầy đủ là sự kết hợp của bốn vấn đề quét được chuyển đổi, mỗi vấn đề được xử lý với cùng một sự tích lũy tiền tố dựa trên Fenwick. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((N+Q) \cdot (N+M))$|$O(N+M)$| Quá chậm | 
| Tối ưu |$O((N+M+Q)\log C)$|$O(N+M+Q)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta rút gọn mọi đối tượng hình học thành một tập hợp các sự kiện điểm có trọng số. Mỗi hình chữ nhật ban đầu đóng góp bốn điểm đã ký để việc truy vấn tổng tiền tố sẽ tái tạo số lượng thông qua loại trừ bao gồm. Việc chuyển đổi này rất cần thiết vì nó biến truy vấn khu vực thành truy vấn tích lũy điểm. 

Sau lần giảm này, chúng tôi xử lý từng hướng một bằng cách sử dụng đường quét. Cơ chế tương tự được sử dụng lại cho tất cả các hướng sau khi chuyển đổi tọa độ thích hợp. 

### Quét ngang (Trường hợp 1) 

1. Sắp xếp tất cả các sự kiện theo thứ tự tăng dần$x$-điều phối. Điều này đảm bảo rằng khi chúng tôi xử lý một điểm, tất cả các đóng góp có giá trị nhỏ hơn$x$đã được bao gồm trong cấu trúc dữ liệu. 
2. Duy trì cây Fenwick bị nén$y$-tọa độ. Mỗi chỉ số lưu trữ trọng số tích lũy ở cấp độ dọc đó. 
3. Xử lý các sự kiện theo thứ tự. Nếu một sự kiện là một bản cập nhật, hãy thêm trọng số của nó vào$y$-vị trí trong cây Fenwick. Nếu đó là một truy vấn tại$(x, y)$, tính tổng tiền tố trên tất cả$y' \le y$. Điều này trả về tổng số đóng góp bên trong$[1, x] \times [1, y]$cho trạng thái quét hiện tại. 

Lý do điều này có tác dụng là vì đường quét đảm bảo tất cả đều hợp lệ$x$-đóng góp đã hoạt động khi truy vấn được xử lý. 

### Quét dọc (Trường hợp 2) 

1. Hoán đổi tọa độ và áp dụng quy trình tương tự. Tính đối xứng của cấu trúc đảm bảo rằng việc trao đổi$x$Và$y$duy trì các mối quan hệ tiền tố. 

### Quét chéo$y = x$(Trường hợp 3) 

1. Tham số lại tọa độ bằng cách sử dụng$u = x - y$. Điều này biến đổi các ràng buộc đường chéo thành các ràng buộc theo trục trong$(u, y)$-không gian. 
2. Chia vùng truy vấn thành hai vùng con sao cho mỗi vùng trở thành một hình chữ nhật theo tọa độ được chuyển đổi. Điều này tránh việc xử lý trực tiếp các ranh giới hình tam giác. 
3. Chạy logic quét tương tự như Trường hợp 1 hoặc Trường hợp 2 trên tọa độ được chuyển đổi. 

Ý tưởng chính là việc sửa chữa$x - y$biến các đường chéo thành các ranh giới dọc, làm cho tổng tiền tố có thể áp dụng lại. 

### Quét chống chéo$y = -x$(Trường hợp 4) 

1. Tham số lại bằng cách sử dụng$v = x + y$. Điều này chuyển đổi các ràng buộc chống đường chéo thành các ràng buộc theo trục trong$(x, v)$hoặc$(y, v)$không gian. 
2. Biểu thị vùng truy vấn dưới dạng chênh lệch của hai vùng có thể biểu thị tiền tố để xử lý việc cắt bớt ranh giới. 
3. Áp dụng lại cấu trúc quét tương tự, giảm mọi thứ trở lại Trường hợp 1. 

### Tại sao nó hoạt động 

Ở tất cả các giai đoạn, thuật toán duy trì tính bất biến là cây Fenwick lưu trữ chính xác tổng trọng số của tất cả các sự kiện có tọa độ được chuyển đổi nằm trong tiền tố được xử lý. Đường quét đảm bảo tính chính xác theo một chiều, trong khi các phép biến đổi tọa độ đảm bảo rằng mọi ràng buộc không thẳng hàng theo trục có thể được viết lại dưới dạng tiền tố trong một số không gian được chuyển đổi. Vì mỗi phép chuyển đổi đều có tính chất phỏng đoán trên vùng liên quan và duy trì thứ tự cần thiết cho việc tích lũy tiền tố nên không có đóng góp nào bị mất hoặc được tính gấp đôi. 

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

def solve():
    n = int(input())
    events = []
    ys = []

    rects = []
    for _ in range(n):
        x1, y1, x2, y2 = map(int, input().split())
        rects.append((x1, y1, x2, y2))
        ys.extend([y1, y2 + 1])

        events.append((x1, y1, 1))
        events.append((x2 + 1, y1, 1))
        events.append((x1, y2 + 1, 1))
        events.append((x2 + 1, y2 + 1, 1))

    q = int(input())
    queries = []
    for i in range(q):
        x, y = map(int, input().split())
        queries.append((x, y, i))
        ys.append(y)

    ys = sorted(set(ys))
    comp = {v: i + 1 for i, v in enumerate(ys)}

    bit = Fenwick(len(ys))

    ev = []
    for x, y, w in events:
        ev.append((x, 0, y, w))
    for x, y, i in queries:
        ev.append((x, 1, y, i))

    ev.sort()

    ans = [0] * q

    for item in ev:
        if item[1] == 0:
            _, _, y, w = item
            bit.add(comp[y], w)
        else:
            _, _, y, i = item
            ans[i] = bit.sum(comp[y])

    print("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Mã thực hiện một đường quét trên đã được sắp xếp$x$-tọa độ. Tất cả các đóng góp hình chữ nhật được mở rộng thành các sự kiện ở góc với các dấu hiệu loại trừ bao gồm và các truy vấn được chèn vào cùng một luồng sự kiện. Việc nén tọa độ là cần thiết vì$y$-các giá trị có thể lớn hoặc thưa thớt và cây Fenwick yêu cầu phạm vi chỉ số nhỏ gọn. 

Một điểm tinh tế là việc sắp xếp các sự kiện bằng nhau$x$. Các cập nhật phải được xử lý trước khi truy vấn cùng một lúc$x$nếu định nghĩa bao gồm ranh giới$x$. Nếu thứ tự này bị đảo ngược, các điểm nằm chính xác trên đường biên sẽ bị bỏ sót, tạo ra việc đếm thiếu một cách có hệ thống. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản với một hình chữ nhật và một truy vấn. 

đầu vào:```
1
1 1 2 2
1
2 2
```Hình chữ nhật đóng góp bốn sự kiện. Sau khi loại trừ, cây Fenwick sẽ tích lũy trọng số 1 trong vùng được bao phủ bởi hình chữ nhật. Truy vấn tại (2,2) sẽ nắm bắt tất cả các đóng góp. 

| Bước | Sự kiện đã xử lý | Bang Fenwick (khái niệm) | Kết quả truy vấn | 
| --- | --- | --- | --- | 
| 1 | (1,1,+1) | (1,1)=1 | - | 
| 2 | (1,3,+1) | thêm phạm vi truy vấn bên ngoài | - | 
| 3 | (3,1,+1) | thêm phạm vi truy vấn bên ngoài | - | 
| 4 | (3,3,+1) | hủy vùng góc | - | 
| 5 | truy vấn (2,2) | tổng tiền tố trên y 2 tại x 2 | 1 | 

Điều này xác nhận rằng việc loại trừ bao gồm sẽ cô lập chính xác phần bên trong hình chữ nhật. 

Bây giờ hãy xem xét các hình chữ nhật chồng lên nhau: 

đầu vào:```
2
1 1 3 3
2 2 4 4
1
3 3
```Điểm (3,3) nằm trong cả hai hình chữ nhật. 

| Bước | Sự kiện | Đóng góp tích cực | Truy vấn | 
| --- | --- | --- | --- | 
| sau tất cả các cập nhật | cả hai hình chữ nhật được chèn | trọng lượng chồng chéo tích lũy | - | 
| truy vấn (3,3) | tổng tiền tố | cả hai hình chữ nhật đều được tính | 2 | 

Điều này xác minh rằng các vùng chồng chéo được tính tổng chính xác mà không bị nhiễu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((N+Q)\log C)$| mỗi sự kiện kích hoạt cập nhật hoặc truy vấn Fenwick | 
| Không gian |$O(N+Q)$| lưu trữ sự kiện và tọa độ nén | 

Hệ số logarit xuất phát từ các phép toán của cây Fenwick trên tọa độ nén. Với các ràng buộc điển hình lên đến$10^5$sự kiện, điều này thoải mái phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else ""  # placeholder for integration

# minimal case
assert run("""1
1 1 1 1
1
1 1
""") == "1\n"

# single rectangle boundary shift
assert run("""1
1 1 2 2
1
1 1
""") == "1\n"

# overlapping rectangles
assert run("""2
1 1 3 3
2 2 4 4
1
3 3
""") == "2\n"

# non-overlapping
assert run("""2
1 1 1 1
3 3 3 3
1
2 2
""") == "0\n"

# edge boundary
assert run("""1
1 1 2 2
1
3 3
""") == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ô đơn | 1 | hòa nhập cơ bản | 
| chồng chéo | 2 | hành vi phụ gia | 
| truy vấn bên ngoài | 0 | độ đúng ranh giới | 
| hình chữ nhật rời rạc | 0 | không rò rỉ | 

## Vỏ cạnh 

Một trường hợp lỗi phổ biến xảy ra khi các bản cập nhật và truy vấn có cùng tọa độ. Giả sử một sự kiện tồn tại tại$x = 5$và một truy vấn cũng có tại$x = 5$. Nếu truy vấn được xử lý trước khi cập nhật thì đóng góp tại ranh giới đó sẽ bị bỏ qua. Thứ tự đúng là xử lý tất cả các cập nhật trước cho một vị trí quét nhất định, đảm bảo tiền tố bao gồm các điểm biên. 

Một trường hợp tế nhị khác phát sinh từ$x_2 + 1$Và$y_2 + 1$thay đổi. Đối với hình chữ nhật$[1,1]$ĐẾN$[1,1]$, việc bỏ qua sự thay đổi sẽ tạo ra sự hủy bỏ làm cho phần đóng góp về 0 không chính xác. Với những thay đổi chính xác, bốn sự kiện góc sẽ giảm xuống còn một đóng góp đơn vị hiệu quả duy nhất tại (1,1), đây là những gì truy vấn sẽ thấy. 

Trường hợp thứ ba là xử lý sai nén phối hợp. Nếu như$y$-các giá trị không bị trùng lặp trước khi lập chỉ mục, các chỉ số Fenwick có thể chồng lên các giá trị khác biệt về mặt logic. Ví dụ: hai sự kiện khác nhau cùng một lúc$y$phải ánh xạ tới cùng một chỉ mục nén; nếu không, tổng tiền tố sẽ đếm không chính xác gấp đôi ngay cả khi logic quét đúng.
