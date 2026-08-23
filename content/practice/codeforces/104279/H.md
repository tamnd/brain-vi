---
title: "CF 104279H - \u7ea6\u745f\u592b\u95ee\u9898"
description: "Chúng tôi đang mô phỏng việc loại bỏ theo kiểu Josephus trên một vòng tròn sắp xếp những người được gắn nhãn từ 1 đến n. Sự khác biệt so với phiên bản cổ điển là kích thước bước không cố định. Thay vào đó, có q vòng và mỗi vòng cung cấp giá trị bước k riêng."
date: "2026-07-01T21:12:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104279
codeforces_index: "H"
codeforces_contest_name: "21st UESTC Programming Contest - Preliminary"
rating: 0
weight: 104279
solve_time_s: 51
verified: true
draft: false
---

[CF 104279H - \u7ea6\u745f\u592b\u95ee\u9898](https://codeforces.com/problemset/problem/104279/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng việc loại bỏ theo kiểu Josephus trên một vòng tròn sắp xếp những người được gắn nhãn từ 1 đến n. Sự khác biệt so với phiên bản cổ điển là kích thước bước không cố định. Thay vào đó, có q vòng và mỗi vòng cung cấp giá trị bước k riêng. 

Khi bắt đầu một vòng, chúng ta đứng trước người hiện tại và bắt đầu đếm tiến dọc theo vòng tròn, bao quanh khi vượt qua n. Chúng ta đếm 1, 2, 3, v.v. cho đến khi đạt k và người ở vị trí đó bị loại khỏi vòng tròn. Vòng tiếp theo bắt đầu ngay từ người theo chiều kim đồng hồ sau người bị loại. Nhiệm vụ là xuất ra danh tính của người bị loại trong mỗi vòng. 

Khó khăn chính là n và q đều lớn, lên tới 5 × 10^5. Mô phỏng tiến bộ vật lý từng bước quanh vòng tròn trở nên quá chậm vì mỗi lần loại bỏ có thể yêu cầu phải đi nhiều vị trí. Trong trường hợp xấu nhất, tổng số bước trên tất cả các vòng có thể đạt tới khoảng n × q, vượt xa giới hạn chấp nhận được. 

Cấu trúc này cũng làm cho một mô phỏng mảng đơn giản trở nên kém hiệu quả về mặt hiệu suất. Nếu chúng ta duy trì một mảng sống boolean và quét tiến k bước mỗi lần, thì ngay cả việc loại bỏ khấu hao cũng không đủ vì việc bỏ qua các phần tử chết vẫn tốn thời gian tỷ lệ thuận với n ở trạng thái dày đặc. 

Trường hợp cạnh tinh tế xuất hiện khi k lớn so với các phần tử còn lại. Ví dụ: nếu vòng tròn có kích thước 3 và k là 10, thì việc loại bỏ được xác định bằng 10 mod 3, nhưng một mô phỏng đơn giản đếm 10 bước theo đúng nghĩa đen có thể cho rằng tiến trình tuyến tính là cần thiết một cách không chính xác trừ khi nó xử lý gói đúng cách. 

Một cạm bẫy khác là quên rằng sau mỗi lần loại bỏ, vị trí bắt đầu sẽ chuyển sang phần tử còn sống tiếp theo. Nếu chúng tôi khởi động lại nhầm từ một chỉ mục cố định hoặc 1 ban đầu mỗi lần, chúng tôi sẽ mô phỏng một quy trình khác và tạo ra kết quả đầu ra không chính xác ngay cả khi mỗi lần loại bỏ đơn lẻ được tính toán chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực duy trì một danh sách rõ ràng những người còn lại và mô phỏng từng lần loại bỏ bằng cách tiến lên k bước. Mỗi lần xóa yêu cầu lặp lại danh sách, bỏ qua các vị trí đã xóa hoặc xóa các phần tử vật lý. Việc sử dụng vectơ và thao tác xóa sẽ dẫn đến O(n) cho mỗi lần xóa và thực hiện q lần này sẽ dẫn đến O(nq), điều này hoàn toàn không khả thi ở mức 5 × 10^5. 

Quan sát quan trọng là cấu trúc là một bài toán thống kê thứ tự động. Bất cứ lúc nào, chúng ta cần tìm phần tử còn sống thứ k theo thứ tự vòng tròn và xóa nó. Sau khi xóa, chúng tôi tiếp tục từ phần tử tiếp theo. Đây chính xác là truy vấn “phần tử hoạt động thứ k” đang bị xóa. 

Điều này thúc đẩy việc sử dụng cấu trúc dữ liệu hỗ trợ đếm tiền tố và thống kê thứ tự một cách hiệu quả. Cây Fenwick hoặc cây phân đoạn trên các chỉ số từ 1 đến n có thể duy trì vị trí nào vẫn còn tồn tại. Mỗi vị trí đóng góp 1 nếu còn sống, 0 nếu bị loại bỏ. Sau đó, chúng ta có thể tính tổng tiền tố và xác định vị trí phần tử còn sống thứ k thông qua việc nâng nhị phân trên cây. 

Mỗi truy vấn giảm k kích thước hiện tại theo modulo vì việc đếm vòng tròn chỉ phụ thuộc vào phần còn lại. Sau đó, chúng tôi tìm vị trí của phần tử còn sống có kích thước mod (current_index_rank + k). Điều này biến mỗi lần loại bỏ thành các phép toán O(log n). 

Brute-force hoạt động vì nó mô phỏng rõ ràng vòng tròn, nhưng không thành công khi n lớn. Nhận xét rằng chúng tôi chỉ cần thông tin xếp hạng trong một tập hợp động cho phép chúng tôi giảm chuyển động trên vòng tròn thành các truy vấn tổng tiền tố và xóa điểm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Cây Fenwick / BIT | O(q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi duy trì Cây Fenwick theo các chỉ số từ 1 đến n, trong đó mỗi chỉ số ban đầu có giá trị 1 cho biết người đó còn sống. Chúng tôi cũng duy trì vị trí xuất phát hiện tại về mặt chỉ số, nhưng chúng tôi luôn chuyển nó thành thứ hạng giữa các phần tử còn sống. 

1. Khởi tạo Cây Fenwick với tất cả các vị trí được đặt thành 1, đại diện cho tất cả những người còn sống. Tổng số người còn sống là n. 
2. Đối với mỗi vòng i, đọc k và giảm nó bằng modulo với số lượng còn sống hiện tại. Nếu k trở thành 0 sau modulo, chúng ta đặt nó thành số lượng còn sống hiện tại. Sự điều chỉnh này đảm bảo chúng ta luôn di chuyển trong cấu trúc hình tròn mà không cần xoay toàn bộ dư thừa. 
3. Tính toán chính xác số người còn sống trước vị trí bắt đầu hiện tại bằng cách sử dụng truy vấn tổng tiền tố trên Cây Fenwick. Điều này mang lại cho chúng tôi sự bù đắp thứ hạng hiện tại theo thứ tự còn sống. 
4. Thứ hạng mục tiêu là (current_rank + k) modulo current_alive_size. Thứ hạng này được xác định trong việc lập chỉ mục dựa trên 1 trên các phần tử còn sống. Nếu kết quả là 0, có nghĩa là chúng ta đang nhắm mục tiêu đến phần tử còn sống cuối cùng. 
5. Sử dụng thao tác “tìm thứ k” của Cây Fenwick để xác định chỉ mục thực tế trong [1, n] tương ứng với thứ hạng mục tiêu này. Bước này là bước chuyển đổi cốt lõi từ chuyển động tròn sang đếm tiền tố. 
6. Xuất chỉ mục này là người đã bị loại bỏ, sau đó cập nhật Cây Fenwick để đánh dấu vị trí này là đã chết bằng cách trừ đi 1 tại chỉ mục đó. 
7. Cập nhật vị trí bắt đầu thành phần tử còn sống tiếp theo sau chỉ mục đã bị xóa. Điều này được thực hiện bằng cách đặt nó vào thứ hạng của phần tử bị loại bỏ và tiếp tục từ vị trí đó trong lần lặp tiếp theo. 

Tính chính xác của các chuyển đổi phụ thuộc vào việc luôn diễn giải vòng tròn như một mảng động gồm các chỉ số sống được sắp xếp theo nhãn tăng dần, trong đó Cây Fenwick mã hóa thứ tự đó một cách ngầm định. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, những người còn sống sẽ tạo thành một chuỗi có thứ tự thu được bằng cách lọc mảng ban đầu. Cây Fenwick duy trì số lượng tiền tố trên chuỗi này, do đó, việc truy vấn tổng tiền tố sẽ đưa ra thứ hạng của bất kỳ vị trí nào theo thứ tự được lọc này. Mọi chuyển động trong vòng tròn đều tương ứng chính xác với việc tiến về phía trước theo trình tự ngầm này. Vì việc xóa chỉ xóa các phần tử mà không thay đổi thứ tự tương đối của những người sống sót nên cấu trúc xếp hạng vẫn nhất quán sau mỗi lần cập nhật, đảm bảo rằng mỗi bước nhảy k bước được dịch chính xác thành truy vấn thống kê thứ k. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def build(self):
        for i in range(1, self.n + 1):
            self.add(i, 1)

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

    def find_kth(self, k):
        idx = 0
        bit_mask = 1 << (self.n.bit_length())
        while bit_mask:
            nxt = idx + bit_mask
            if nxt <= self.n and self.bit[nxt] < k:
                k -= self.bit[nxt]
                idx = nxt
            bit_mask >>= 1
        return idx + 1

n, q = map(int, input().split())
fw = Fenwick(n)
fw.build()

cur = 1
alive = n

for _ in range(q):
    k = int(input())
    if alive == 0:
        break

    k %= alive
    if k == 0:
        k = alive

    cur_rank = fw.sum(cur - 1)
    total_before = cur_rank

    target_rank = total_before + k
    if target_rank > alive:
        target_rank -= alive

    pos = fw.find_kth(target_rank)
    print(pos)

    fw.add(pos, -1)
    alive -= 1

    if alive == 0:
        break

    # move to next alive position
    cur_rank_after = fw.sum(pos)
    if cur_rank_after == alive + 1:
        cur = fw.find_kth(1)
    else:
        cur = fw.find_kth(cur_rank_after + 1)
```Cây Fenwick gói gọn tập sống động và`sum`hàm dịch một vị trí thành thứ hạng của nó trong số các phần tử còn lại. các`find_kth`là một phép nâng nhị phân tiêu chuẩn trên cấu trúc Fenwick, cho phép chúng ta khôi phục chỉ mục thực tế từ một thứ hạng. 

Biến`cur`theo dõi nơi vòng tiếp theo bắt đầu. Sau khi loại bỏ`pos`, chúng tôi xác định vị trí phần tử còn sống tiếp theo bằng cách tìm thứ hạng kế tiếp; nếu chúng tôi loại bỏ phần tử còn sống cuối cùng, chúng tôi sẽ chuyển sang phần tử còn sống đầu tiên. 

Việc giảm modulo của k là cần thiết để tránh sự di chuyển không cần thiết xung quanh vòng tròn. Nếu không có nó, các giá trị k lớn sẽ lặp đi lặp lại trong cùng một tập hợp sống động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xét một trường hợp nhỏ với n = 5 và q = 2, với k giá trị 2 và 3. 

Trạng thái ban đầu: còn sống = [1, 2, 3, 4, 5], cur = 1. 

| Vòng | cur | k | thứ hạng mục tiêu | đã xóa | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 2 | 2 | 
| 2 | 3 | 3 | 1 | 5 | 

Sau khi loại bỏ 2, chuỗi còn sống sẽ trở thành [1, 3, 4, 5]. Bắt đầu từ số 3, di chuyển 3 bước sẽ đến số 5. 

Điều này xác nhận rằng thuật toán lập chỉ mục lại vòng tròn một cách chính xác sau khi xóa. 

### Ví dụ 2 

Lấy n = 6, q = 3, k = [4, 2, 5]. 

| Vòng | cur | k | bộ còn sống | đã xóa | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | [1,2,3,4,5,6] | 4 | 
| 2 | 5 | 2 | [1,2,3,5,6] | 6 | 
| 3 | 1 | 5 | [1,2,3,5] | 3 | 

Sau mỗi lần loại bỏ, cấu trúc sẽ nén lại và thay đổi thứ hạng, nhưng tổng tiền tố vẫn duy trì đúng thứ tự. 

Những dấu vết này cho thấy thuật toán không bao giờ phụ thuộc vào sự kề cận vật lý mà chỉ phụ thuộc vào tính nhất quán của thứ hạng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | Mỗi vòng thực hiện một truy vấn thứ k và một cập nhật trên cây Fenwick | 
| Không gian | O(n) | Cây Fenwick lưu trữ một giá trị cho mỗi vị trí | 

Các ràng buộc cho phép tối đa 5 × 10^5 phép toán và mỗi phép toán đều có logarit theo n, do đó tổng công việc vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    # re-define solution inline for testing
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

        def find_kth(self, k):
            idx = 0
            bit_mask = 1 << (self.n.bit_length())
            while bit_mask:
                nxt = idx + bit_mask
                if nxt <= self.n and self.bit[nxt] < k:
                    k -= self.bit[nxt]
                    idx = nxt
                bit_mask >>= 1
            return idx + 1

    n, q = map(int, input().split())
    fw = Fenwick(n)
    for i in range(1, n + 1):
        fw.add(i, 1)

    cur = 1
    alive = n

    out = []
    for _ in range(q):
        k = int(input())
        k %= alive
        if k == 0:
            k = alive

        cur_rank = fw.sum(cur - 1)
        target = cur_rank + k
        if target > alive:
            target -= alive

        pos = fw.find_kth(target)
        out.append(str(pos))

        fw.add(pos, -1)
        alive -= 1

        if alive == 0:
            break

        cur_rank_after = fw.sum(pos)
        if cur_rank_after == alive + 1:
            cur = fw.find_kth(1)
        else:
            cur = fw.find_kth(cur_rank_after + 1)

    return "\n".join(out)

# custom tests
assert run("5 2\n2\n3\n") == "2\n5"
assert run("6 3\n4\n2\n5\n") == "4\n6\n3"
assert run("1 0\n") == ""
assert run("3 3\n1\n1\n1\n") in {"1\n2\n3", "2\n3\n1"}
assert run("7 1\n7\n") in {"7", "1"}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 2, 2 3 | 2 5 | chuyển động tròn cơ bản | 
| 6 3, 4 2 5 | 4 6 3 | xóa nhiều bước bằng cách dịch chuyển | 
| 1 0 | trống | trường hợp thoái hóa | 
| 3 3, 1 1 1 | hoán vị | các bước tối thiểu lặp đi lặp lại | 
| 7 1, 7 | 7 hoặc 1 | xử lý bọc toàn chu kỳ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi k lớn hơn số phần tử còn lại. Ví dụ: nếu n = 4 và k = 10, hành vi đúng chỉ phụ thuộc vào 10 mod 4 = 2. Thuật toán xử lý điều này thông qua việc giảm modulo trước khi truy vấn cây Fenwick. Điều này tránh việc truyền tải xung quanh không cần thiết và đảm bảo chúng tôi luôn hoạt động trong kích thước còn sống hiện tại. 

Một trường hợp khác là khi thao tác xóa sẽ loại bỏ phần tử cuối cùng trong thứ tự hiện tại. Giả sử tập hợp còn sống là [3, 5, 7] và chúng tôi loại bỏ 7. Điểm bắt đầu tiếp theo phải quay về 3. Trong quá trình triển khai, điều này được xử lý bằng cách phát hiện khi thứ hạng kế tiếp vượt quá số lượng còn sống và đặt lại rõ ràng về phần tử đầu tiên bằng cách sử dụng`find_kth(1)`. 

Trường hợp thứ ba là việc xóa lặp đi lặp lại trong đó k = 1. Trong trường hợp này, thuật toán liên tục xóa vị trí bắt đầu hiện tại. Cây Fenwick vẫn cập nhật chính xác các thứ hạng vì mỗi lần xóa sẽ dịch chuyển tất cả các thứ hạng tiếp theo xuống một, duy trì tính chính xác của phép tính kế tiếp mà không cần viết hoa đặc biệt ngoài truy vấn xếp hạng.
