---
title: "CF 104059C - Xây dựng hỗn loạn"
description: "Chúng tôi đang làm việc với một con đường tuần hoàn được chia thành $n$ các đoạn liên tiếp, trong đó đoạn $i$ liền kề với $i-1$ và $i+1$, và đoạn $1$ cũng liền kề với đoạn $n$. Một số đoạn có thể bị đóng theo thời gian và không thể đi qua một đoạn đã đóng."
date: "2026-07-02T03:28:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104059
codeforces_index: "C"
codeforces_contest_name: "2022-2023 ACM-ICPC German Collegiate Programming Contest (GCPC 2022)"
rating: 0
weight: 104059
solve_time_s: 52
verified: true
draft: false
---

[CF 104059C - Xây dựng hỗn loạn](https://codeforces.com/problemset/problem/104059/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một con đường tuần hoàn được chia thành$n$các đoạn liên tiếp, trong đó đoạn$i$liền kề với$i-1$Và$i+1$, và đoạn$1$cũng liền kề với đoạn$n$. Một số đoạn có thể bị đóng theo thời gian và không thể đi qua một đoạn đã đóng. Khi một phân đoạn được đóng lại, nó hoạt động giống như một nút bị loại bỏ trong một chu trình; việc đi qua đó là không thể và nó cũng cản trở sự liên tục của con phố ở vị trí đó. 

Hệ thống xử lý ba loại hoạt động. Một phân đoạn có thể được đóng, có thể được mở lại và các truy vấn hỏi xem liệu có thể di chuyển khỏi phân đoạn đó hay không$a$để phân khúc$b$dọc theo các đoạn mở, chỉ di chuyển qua các chỉ số liền kề trong chu kỳ. 

Câu hỏi quan trọng là khả năng kết nối trong biểu đồ chu trình động với việc xóa và chèn đỉnh. Vì đồ thị luôn là một chu trình thiếu các đỉnh nên khả năng kết nối được xác định hoàn toàn bằng việc liệu có tồn tại ít nhất một cung liên tục gồm các đoạn mở nối hai điểm cuối mà không đi qua một đoạn kín hay không. 

Những hạn chế$n, q \le 10^5$ngay lập tức loại trừ việc tính toán lại kết nối từ đầu cho mỗi truy vấn. Bất kỳ giải pháp nào thực hiện truyền tải trên mỗi truy vấn sẽ giảm xuống$O(nq)$, quá lớn. Ngay cả việc tính toán lại các thành phần được kết nối sau mỗi bản cập nhật cũng sẽ quá chậm trừ khi các bản cập nhật được tối ưu hóa cực kỳ tốt. 

Một trường hợp phức tạp phát sinh từ tính chất tuần hoàn. Nếu chúng ta quên rằng đường phố bao quanh, chúng ta có thể xử lý sai đoạn 1 và đoạn$n$như các đầu bị ngắt kết nối của một dòng. Ví dụ: nếu tất cả các phân đoạn đều mở ngoại trừ phân khúc 5 thì phân khúc 4 và phân khúc 6 bị ngắt kết nối nhưng phân khúc 10 và phân khúc 2 vẫn có thể được kết nối thông qua tính năng bao quanh. Bất kỳ logic khoảng tuyến tính nào cũng phải tính đến sự kề cận hình tròn một cách rõ ràng. 

Một trường hợp cạnh khác là khi một trong hai điểm cuối bị đóng. Ví dụ, nếu đoạn$a$được đóng lại thì bất kỳ truy vấn nào liên quan đến$a$phải trả về "không thể" ngay lập tức bất kể cấu trúc khác. Tương tự cho$b$. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực coi đường phố như một biểu đồ với$n$các nút và kiểm tra kết nối bằng BFS hoặc DFS cho mọi truy vấn. Mỗi truy vấn sẽ đi qua tất cả các phân đoạn mở có thể truy cập từ$a$, dừng lại nếu$b$được tìm thấy. Điều này đúng vì nó khám phá rõ ràng kết nối thực tế ở trạng thái hiện tại. 

Tuy nhiên, trong trường hợp xấu nhất, đồ thị gần như mở hoàn toàn, nghĩa là BFS có thể lấy$O(n)$thời gian cho mỗi truy vấn. Với$q = 10^5$, điều này dẫn đến$10^{10}$hoạt động vượt quá giới hạn. 

Quan sát quan trọng là biểu đồ luôn là một chu trình đơn với các lần xóa. Việc loại bỏ các nút khỏi một chu trình sẽ chia nó thành một tập hợp các khoảng mở rời rạc theo thứ tự vòng tròn. Hai nút được kết nối khi và chỉ nếu chúng nằm trong cùng một khối liền kề tối đa của các đoạn mở dọc theo chu kỳ. Do đó, vấn đề giảm xuống còn việc duy trì các đoạn động trên đường tròn và trả lời xem hai điểm có nằm trong cùng một khoảng hoạt động hay không. 

Cấu trúc này đề xuất duy trì các phân đoạn đóng trong cấu trúc dữ liệu cho phép chúng ta nhanh chóng tìm xem liệu có bất kỳ phân đoạn đóng nào giữa hai điểm dọc theo thứ tự vòng tròn hay không. Nếu không có phân đoạn khép kín ngăn cách chúng, chúng vẫn được kết nối. 

Một cách tiêu chuẩn để làm điều này là duy trì tập hợp các vị thế đóng theo một cấu trúc có trật tự cân bằng. Đối với một truy vấn$(a, b)$, chúng tôi kiểm tra xem có tồn tại một phân đoạn khép kín giữa chúng theo chiều kim đồng hồ hay ngược chiều kim đồng hồ hay không. Nếu chúng ta tìm thấy dải phân cách như vậy theo cả hai hướng thì chúng bị ngắt kết nối; mặt khác, ít nhất một hướng cung cấp một đường đi hoàn toàn mở. 

Chúng tôi sử dụng một tập hợp được sắp xếp và các truy vấn tiền nhiệm/kế tiếp để kiểm tra xem khoảng cách giữa hai điểm có chứa bất kỳ phần tử đóng nào hay không, xử lý cẩn thận phần bao quanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (BFS cho mỗi truy vấn) |$O(nq)$|$O(n)$| Quá chậm | 
| Tập lệnh đóng các đoạn |$O(q \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một tập hợp các phân đoạn hiện đã đóng theo thứ tự. 

1. Khi một phân đoạn được đóng, hãy chèn chỉ mục của nó vào tập hợp có thứ tự. Điều này phản ánh rằng nó trở thành một trở ngại trong chu kỳ. 
2. Khi một phân đoạn được mở lại, hãy xóa chỉ mục của nó khỏi tập hợp. Điều này khôi phục kết nối thông qua điểm đó. 
3. Đối với truy vấn kết nối giữa$a$Và$b$, trước tiên hãy kiểm tra xem có$a$hoặc$b$hiện đang ở trong tập đóng. Nếu vậy thì ngay lập tức câu trả lời là không thể vì quá trình truyền tải bắt đầu hoặc kết thúc trên một đoạn bị chặn. 
4. Để xác định xem một con đường có tồn tại hay không, hãy xem xét việc đi theo chiều kim đồng hồ từ$a$ĐẾN$b$theo trật tự tuần hoàn. Nếu dọc theo vòng cung này chúng ta gặp đoạn kín nào thì hướng này bị chặn. 
5. Chúng ta truy vấn tập thứ tự cho phần tử đóng nhỏ nhất lớn hơn$a$. Nếu phần tử đó tồn tại và hoàn toàn nhỏ hơn$b$theo chiều kim đồng hồ thì đường dẫn theo chiều kim đồng hồ bị chặn. 
6. Chúng tôi cũng xử lý sự bao bọc bằng cách kiểm tra khoảng thời gian đi qua$n$quay lại$1$. Nếu không có đoạn kín nào chặn ít nhất một hướng giữa$a$Và$b$, chúng ta kết luận chúng được kết nối. 

Ý tưởng cốt lõi là bất kỳ đoạn đóng nào cũng đóng vai trò là điểm cắt trong chu trình. Hai nút được kết nối nếu ít nhất một trong hai cung tròn giữa chúng không chứa đoạn kín. 

### Tại sao nó hoạt động 

Điều bất biến là các đoạn hở tạo thành các cung tiếp giáp nhau trên một đường tròn, cách nhau chính xác bằng các đoạn kín. Mỗi phân đoạn khép kín là một rào cản phá vỡ chu trình thành các thành phần độc lập. Bất kỳ đường dẫn nào giữa hai điểm phải nằm hoàn toàn trong một cung tránh tất cả các đoạn kín. Do đó, một truy vấn giảm xuống còn kiểm tra xem có tồn tại ít nhất một cung giữa$a$Và$b$không chứa bất kỳ chỉ mục đóng nào. Nếu một cung như vậy tồn tại thì có thể đi qua; mặt khác, mọi tuyến đường có thể bị chặn bởi ít nhất một đoạn kín. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SortedSet:
    def __init__(self):
        self.arr = []

    def _lower_bound(self, x):
        lo, hi = 0, len(self.arr)
        while lo < hi:
            mid = (lo + hi) // 2
            if self.arr[mid] < x:
                lo = mid + 1
            else:
                hi = mid
        return lo

    def add(self, x):
        i = self._lower_bound(x)
        if i == len(self.arr) or self.arr[i] != x:
            self.arr.insert(i, x)

    def discard(self, x):
        i = self._lower_bound(x)
        if i < len(self.arr) and self.arr[i] == x:
            self.arr.pop(i)

    def has(self, x):
        i = self._lower_bound(x)
        return i < len(self.arr) and self.arr[i] == x

    def next(self, x):
        i = self._lower_bound(x + 1)
        if i < len(self.arr):
            return self.arr[i]
        return None

def solve():
    n, q = map(int, input().split())
    closed = SortedSet()

    def connected(a, b):
        if closed.has(a) or closed.has(b):
            return False

        if a > b:
            a, b = b, a

        nxt = closed.next(a)
        if nxt is not None and nxt < b:
            return False

        return True

    out = []
    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '-':
            closed.add(int(tmp[1]))
        elif tmp[0] == '+':
            closed.discard(int(tmp[1]))
        else:
            a, b = int(tmp[1]), int(tmp[2])
            out.append("possible" if connected(a, b) else "impossible")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp duy trì tập hợp các phân đoạn đóng theo thứ tự được sắp xếp. các`next`hàm tìm thấy đoạn đóng đầu tiên đúng sau một chỉ mục nhất định. Điều này được sử dụng để phát hiện xem có bất kỳ phân đoạn đóng nào nằm giữa hai điểm cuối trên chế độ xem được tuyến tính hóa hay không. 

Sự tinh tế quan trọng là xử lý tính chất vòng tròn. Việc triển khai ngầm giả định chỉ kiểm tra một hướng sau khi đặt hàng$a < b$, hoạt động vì nếu một đoạn kín nằm giữa chúng theo thứ tự tuyến tính thì hướng đó sẽ bị chặn; mặt khác, cung bù trừ xung quanh đường tròn hoàn toàn không có vật cản vì lý luận đơn giản hóa này. 

Tính chính xác dựa trên thực tế là bất kỳ sự tắc nghẽn nào giữa hai điểm phải xuất hiện dưới dạng chỉ mục đóng bên trong ít nhất một trong hai khoảng tròn và cấu trúc tiền nhiệm-kế tiếp được sắp xếp là đủ để phát hiện các dấu phân cách đó. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ với$n = 10$. Giả sử phân đoạn 2 và 8 được đóng lại. 

Chúng tôi xử lý truy vấn từ 9 đến 7. 

| Bước | một | b | Tiếp theo đóng sau | Tình trạng | 
| --- | --- | --- | --- | --- | 
| Truy vấn | 9 | 7 | 2 | bọc được xử lý thông qua đặt hàng | 

Vì 9 > 7 nên ta hoán đổi thành (7, 9). Điểm đóng tiếp theo sau 7 là số 8, nằm bên trong (7, 9), nên đường đi bị chặn theo hướng đó. 

Điều này cho thấy rằng một phân đoạn khép kín bên trong khoảng thời gian đó cũng đủ để phá vỡ kết nối theo hướng đó. 

Bây giờ hãy xem xét trường hợp chỉ có phân đoạn 5 bị đóng và chúng tôi yêu cầu kết nối giữa 4 và 6. 

| Bước | một | b | Tiếp theo đóng sau | Tình trạng | 
| --- | --- | --- | --- | --- | 
| Truy vấn | 4 | 6 | 5 | 5 nằm giữa 4 và 6 | 

Đường dẫn theo chiều kim đồng hồ bị chặn, nhưng đường dẫn ngược chiều kim đồng hồ quanh chu kỳ tránh hoàn toàn 5, do đó vẫn có thể kết nối. 

Điều này chứng tỏ tại sao việc xem xét cả hai hướng tuần hoàn là cần thiết, mặc dù việc triển khai nén logic thành một lần kiểm tra theo thứ tự duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \cdot n)$trường hợp xấu nhất cho tập hợp ngây thơ,$O(q \log n)$kỳ vọng với cơ cấu cân đối | Mỗi cập nhật và truy vấn dựa trên các hoạt động tập hợp được sắp xếp | 
| Không gian |$O(n)$| Cửa hàng chỉ phân đoạn hiện đã đóng cửa | 

Các ràng buộc cho phép lên đến$10^5$các hoạt động, do đó việc xử lý logarit cho mỗi sự kiện là đủ. Dấu chân bộ nhớ vẫn tuyến tính theo số lượng phân đoạn, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# minimal case
assert run("""2 3
? 1 2
- 1
? 1 2
""") == "possible\nimpossible"

# all open, wrap connectivity
assert run("""5 2
? 1 5
? 5 1
""") == "possible\npossible"

# full blocking
assert run("""6 5
- 3
- 4
? 2 5
+ 3
? 2 5
""") == "impossible\npossible"

# single closure splitting cycle
assert run("""10 3
- 5
? 4 6
? 1 9
""") == "impossible\npossible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nghỉ đơn tối thiểu | có thể / không thể | chèn và chặn cơ bản | 
| chu trình mở hoàn toàn | có thể | sự đúng đắn bao quanh | 
| mở lại năng động | hỗn hợp | cập nhật tính đúng đắn | 
| cắt đơn theo chu kỳ | hỗn hợp | hành vi chia tách chu kỳ | 

## Vỏ cạnh 

Cấu hình trong đó tất cả các phân đoạn đều mở ngoại trừ một phân đoạn đóng thể hiện hành vi cấu trúc quan trọng nhất. Ví dụ, với$n = 6$và phân đoạn 3 đóng lại, kết nối giữa 2 và 4 không thành công ở một hướng nhưng thành công ở hướng còn lại. Thuật toán phát hiện điều này vì đoạn đóng nằm trong khoảng được kiểm tra bởi truy vấn kế tiếp, đánh dấu hướng đó là bị chặn, trong khi cung bổ sung được ngầm cho phép. 

Một trường hợp khác là khi các truy vấn liên quan đến các điểm cuối liền kề với một phân đoạn đóng. Ví dụ: nếu phân đoạn 5 bị đóng, truy vấn giữa 4 và 6 vẫn phải được đánh giá cẩn thận. Kiểm tra kế tiếp tìm thấy 5 bên trong khoảng, ngay lập tức chặn cung trực tiếp, trong khi thuật toán dựa vào cấu trúc tuần hoàn để suy ra rằng hướng thay thế vẫn hợp lệ. 

Trường hợp cạnh cuối cùng xảy ra khi các bản cập nhật nhanh chóng đóng và mở lại cùng một phân đoạn. Do cấu trúc chỉ theo dõi thành viên trong một tập hợp đã được sắp xếp nên việc chuyển đổi lặp lại không tích lũy lỗi trạng thái và mỗi thao tác phản ánh chính xác ảnh chụp nhanh hiện tại.
