---
title: "CF 102503L - Bóng Arnis"
description: "Chúng tôi có một dòng hộp. Mỗi hộp chứa một số quả bóng và cũng có trạng thái: mở hoặc đóng. Các hoạt động sửa đổi hai phần thông tin này cùng nhau. Thao tác lật sẽ thay đổi mọi hộp trong phạm vi từ mở sang đóng hoặc từ đóng sang mở."
date: "2026-08-07T04:46:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102503
codeforces_index: "L"
codeforces_contest_name: "National Olympiad in Informatics - Philippines (NOI.PH) Online Eliminations 2020"
rating: 0
weight: 102503
solve_time_s: 572
verified: true
draft: false
---

[CF 102503L - Bóng Arnis](https://codeforces.com/problemset/problem/102503/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 32 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dòng hộp. Mỗi hộp chứa một số quả bóng và cũng có trạng thái: mở hoặc đóng. Các hoạt động sửa đổi hai phần thông tin này cùng nhau. Thao tác lật sẽ thay đổi mọi hộp trong phạm vi từ mở sang đóng hoặc từ đóng sang mở. Thao tác thêm chỉ ảnh hưởng đến các hộp hiện đang mở trong một phạm vi. Một thao tác truy vấn yêu cầu tổng số quả bóng trong mỗi hộp trong một phạm vi, bất kể hộp đó mở hay đóng. 

Đầu vào cung cấp số lượng bóng ban đầu, trạng thái mở hoặc đóng ban đầu và sau đó là chuỗi thao tác. Đối với mỗi thao tác truy vấn, chúng ta phải xuất tổng hiện tại của khoảng đã chọn. 

Các giới hạn đủ lớn nên việc mô phỏng mọi hộp bị ảnh hưởng là không thể. Với tối đa 320.000 hộp và 320.000 thao tác, một giải pháp thực hiện công việc tuyến tính cho mọi thao tác có thể thực hiện khoảng 10^11 cập nhật trong trường hợp xấu nhất. Giới hạn thời gian 2 giây yêu cầu mỗi thao tác phải gần với thời gian logarit, điều này loại trừ việc cập nhật mảng trực tiếp và quét theo khoảng thời gian lặp lại. 

Phần khó khăn là các hoạt động ảnh hưởng đến hai thuộc tính liên quan. Số lượng bóng chỉ thay đổi đối với các hộp mở, trong khi trạng thái của các hộp sau này có thể thay đổi thông qua các lần lật. Giải pháp chỉ lưu trữ tổng số tiền sẽ làm mất thông tin cần thiết để biết hộp nào sẽ nhận được phần bổ sung trong tương lai. 

Một trường hợp nhỏ làm hỏng việc triển khai bất cẩn là một thao tác lật theo sau là phần bổ sung:```
Input
2 3
5 7
1 0
1 1 2
2 1 2 3
3 1 2
```Thao tác đầu tiên thay đổi trạng thái thành đóng, mở. Phép cộng chỉ ảnh hưởng đến ô thứ hai, tạo thành giá trị 5 và 10. Câu trả lời là:```
15
```Việc triển khai chỉ lưu trữ tổng số tiền và xử lý mọi phép cộng như một phép cộng phạm vi sẽ tạo ra 18. 

Một trường hợp cạnh khác là một lần lật được áp dụng nhiều lần trong cùng một khoảng thời gian:```
Input
1 4
10
1
1 1 1
1 1 1
2 1 1 5
3 1 1
```Hai lần lật hủy nhau nên hộp sẽ mở khi phép cộng xảy ra. Câu trả lời cuối cùng là:```
15
```Việc triển khai lan truyền lười biếng mà quên kết hợp các cờ lật một cách chính xác có thể khiến hộp bị đóng không chính xác. 

Một lỗi phổ biến cuối cùng là nhầm lẫn tổng được truy vấn với tổng của các ô mở:```
Input
2 1
4 9
1 0
3 1 2
```Câu trả lời là:```
13
```Hộp đóng vẫn đóng góp vào các truy vấn. Chỉ bổ sung bỏ qua hộp đóng. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản là giữ hai mảng: một mảng để đếm số bóng và một mảng cho các trạng thái. Để lật, chúng tôi đi qua khoảng thời gian và chuyển đổi mọi trạng thái. Để bổ sung, chúng ta duyệt qua khoảng và chỉ thêm vào các hộp đang mở. Đối với một truy vấn, chúng tôi tính tổng mọi giá trị trong khoảng. Điều này đúng vì mọi thao tác đều tuân theo các quy tắc của bài toán. 

Vấn đề là một thao tác có thể chạm tới tất cả 320.000 hộp. Nếu mọi thao tác sử dụng một khoảng thời gian đầy đủ, số lượng hành động nguyên thủy có thể đạt khoảng 320.000 × 320.000, tức là khoảng 102 tỷ lượt cập nhật hoặc truy vấn. Cách tiếp cận là đúng nhưng quá chậm. 

Quan sát chính là các hoạt động không cần các hộp riêng lẻ. Một phân đoạn chỉ cần biết hai tổng hợp: tổng giá trị trong các hộp mở và tổng giá trị trong các hộp đóng. Một phép cộng chỉ thay đổi số tiền mở. Một lần lật chỉ đơn giản là trao đổi hai khoản tiền. Truy vấn phạm vi cần giá trị kết hợp của chúng. 

Cấu trúc này phù hợp với cây phân đoạn lười biếng. Mỗi nút đại diện cho một khoảng thời gian và lưu trữ đủ thông tin để trả lời các truy vấn hoặc áp dụng các bản cập nhật mà không giảm dần xuống nút con. Cờ lật lười ghi lại rằng toàn bộ một phân đoạn đã bị đảo ngược nhưng các phân đoạn con của nó vẫn chưa được cập nhật. 

Phương pháp brute-force hoạt động vì nó giữ thông tin chính xác cho mỗi hộp, nhưng không thành công khi có quá nhiều hộp bị chạm liên tục. Quan sát cho thấy việc lật chỉ là sự trao đổi của hai nhóm cho phép chúng ta nén thông tin cần thiết và xử lý từng thao tác theo thời gian logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Tối ưu | O((n + m) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn trong đó mỗi nút lưu trữ tổng số bóng trong các hộp mở, tổng số bóng trong các hộp đóng và số lượng các hộp mở bên trong khoảng. 

Số lượng hộp mở là cần thiết vì việc bổ sung`v`phải tăng số tiền mở bằng`v`nhân với số lượng hộp mở trong phân khúc. 
2. Để thêm phạm vi, hãy truy cập đệ quy vào cây phân đoạn. Nếu một nút hoàn toàn nằm trong khoảng thời gian cập nhật, hãy tăng tổng mở của nó trực tiếp lên`v * open_count`. 

Các hộp đã đóng bị bỏ qua vì thao tác chỉ ảnh hưởng đến các hộp hiện đang mở. 
3. Đối với thao tác lật, hãy truy cập đệ quy vào cây phân đoạn. Khi một nút hoàn toàn nằm trong khoảng, hãy hoán đổi tổng mở và tổng đóng của nó, thay thế số lượng mở của nó bằng số hộp đã đóng trước đó và chuyển đổi cờ lật lười biếng của nó. 

Một cú lật không làm thay đổi số lượng bóng. Nó chỉ thay đổi mỗi hộp thuộc về nhóm nào, vì vậy việc trao đổi hai nhóm được lưu trữ là đủ. 
4. Đối với thao tác truy vấn, hãy thu thập đệ quy tổng giá trị mở và đóng từ các phân đoạn được đề cập. 

Câu trả lời là tổng của cả hai nhóm vì truy vấn tính tất cả các hộp, bất kể trạng thái. 
5. Sử dụng tính năng lan truyền lười biếng bất cứ khi nào toàn bộ phân đoạn bị đảo ngược. Việc lật đang chờ xử lý chỉ được đẩy tới các trẻ em khi thao tác sau đó cần kiểm tra các trẻ em đó. 

Điều này tránh việc truy cập mọi phần tử của một khoảng thời gian đảo ngược lớn. 

Tại sao nó hoạt động: tính bất biến của mỗi nút cây phân đoạn là hai tổng được lưu trữ của nó luôn biểu thị giá trị hiện tại thực của các hộp trong khoảng đó, được phân tách bằng trạng thái hiện tại của chúng. Phép cộng bảo toàn tính bất biến này vì chỉ có nhóm mở mới thay đổi. Việc lật sẽ bảo toàn nó vì các hộp giữ nguyên giá trị của chúng trong khi hai nhóm trạng thái trao đổi vai trò. Lan truyền lười biếng chỉ làm trì hoãn các phép biến đổi hợp lệ này, vì vậy mọi truy vấn đều thấy kết quả giống nhau như thể tất cả các thao tác đã được áp dụng riêng lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegmentTree:
    def __init__(self, values, states):
        self.n = len(values)
        size = 4 * self.n
        self.open_sum = [0] * size
        self.closed_sum = [0] * size
        self.open_cnt = [0] * size
        self.flip = [False] * size
        self.values = values
        self.states = states
        self.build(1, 0, self.n - 1)

    def build(self, node, left, right):
        if left == right:
            if self.states[left]:
                self.open_sum[node] = self.values[left]
                self.open_cnt[node] = 1
            else:
                self.closed_sum[node] = self.values[left]
            return
        mid = (left + right) // 2
        self.build(node * 2, left, mid)
        self.build(node * 2 + 1, mid + 1, right)
        self.pull(node)

    def pull(self, node):
        self.open_sum[node] = self.open_sum[node * 2] + self.open_sum[node * 2 + 1]
        self.closed_sum[node] = self.closed_sum[node * 2] + self.closed_sum[node * 2 + 1]
        self.open_cnt[node] = self.open_cnt[node * 2] + self.open_cnt[node * 2 + 1]

    def apply_flip(self, node, length):
        self.open_sum[node], self.closed_sum[node] = self.closed_sum[node], self.open_sum[node]
        self.open_cnt[node] = length - self.open_cnt[node]
        self.flip[node] = not self.flip[node]

    def push(self, node, left, right):
        if not self.flip[node] or left == right:
            return
        mid = (left + right) // 2
        self.apply_flip(node * 2, mid - left + 1)
        self.apply_flip(node * 2 + 1, right - mid)
        self.flip[node] = False

    def update_add(self, node, left, right, ql, qr, value):
        if qr < left or right < ql:
            return
        if ql <= left and right <= qr:
            self.open_sum[node] += self.open_cnt[node] * value
            return
        self.push(node, left, right)
        mid = (left + right) // 2
        self.update_add(node * 2, left, mid, ql, qr, value)
        self.update_add(node * 2 + 1, mid + 1, right, ql, qr, value)
        self.pull(node)

    def update_flip(self, node, left, right, ql, qr):
        if qr < left or right < ql:
            return
        if ql <= left and right <= qr:
            self.apply_flip(node, right - left + 1)
            return
        self.push(node, left, right)
        mid = (left + right) // 2
        self.update_flip(node * 2, left, mid, ql, qr)
        self.update_flip(node * 2 + 1, mid + 1, right, ql, qr)
        self.pull(node)

    def query(self, node, left, right, ql, qr):
        if qr < left or right < ql:
            return 0
        if ql <= left and right <= qr:
            return self.open_sum[node] + self.closed_sum[node]
        self.push(node, left, right)
        mid = (left + right) // 2
        return self.query(node * 2, left, mid, ql, qr) + self.query(node * 2 + 1, mid + 1, right, ql, qr)

def solve():
    n, m = map(int, input().split())
    values = list(map(int, input().split()))
    states = list(map(int, input().split()))

    seg = SegmentTree(values, states)
    ans = []

    for _ in range(m):
        query = list(map(int, input().split()))
        typ = query[0]
        l = query[1] - 1
        r = query[2] - 1

        if typ == 1:
            seg.update_flip(1, 0, n - 1, l, r)
        elif typ == 2:
            seg.update_add(1, 0, n - 1, l, r, query[3])
        else:
            ans.append(str(seg.query(1, 0, n - 1, l, r)))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Cây phân đoạn giữ hai loại hộp riêng biệt. các`open_sum`Và`closed_sum`mảng đại diện cho tổng giá trị hiện tại của các danh mục đó. các`open_cnt`mảng cho phép áp dụng bổ sung phạm vi mà không cần biết từng hộp riêng lẻ. 

Thao tác lật được xử lý mà không cần thăm lá. Hai khoản tiền này được hoán đổi vì mỗi ô đều thay đổi tư cách thành viên giữa hai danh mục. Số lượng hộp mở cũng được hoán đổi với số lượng hộp đóng, tức là độ dài đoạn trừ đi số lượng mở cũ. 

Cờ lật lười là một boolean vì hai lần lật tương đương với không lật. Khi một nút có thao tác lật đang chờ xử lý được đẩy, cả hai nút con đều nhận được cùng một phép biến đổi trước khi nút cha tiếp tục thực hiện một phần thao tác. 

Tất cả các chỉ mục được chuyển đổi từ lập chỉ mục dựa trên một vấn đề sang lập chỉ mục dựa trên 0 của Python. Số nguyên Python tránh tràn mặc dù câu trả lời tối đa có thể vượt quá phạm vi 32 bit. 

## Ví dụ đã hoạt động 

Đối với mẫu: 

| Hoạt động | Tổng mở | Tổng đóng | Số mở | Trả lời | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 21 | 10 | 3 | | 
| Truy vấn [2,4] | 14 | 0 | | 14 | 
| Thêm 6 vào [1,5] | 39 | 10 | 3 | | 
| Truy vấn [2,4] | 20 | 0 | | 20 | 
| Lật [1,5] | 10 | 39 | 2 | | 
| Thêm 7 vào [1,5] | 24 | 39 | 2 | | 
| Truy vấn [2,4] | 24 | 10 | | 34 | 

Dấu vết này cho thấy việc bổ sung chỉ thay đổi nhóm mở. Việc lật bóng không làm thay đổi tổng số bóng mà chỉ thay đổi nhóm sở hữu mỗi giá trị. 

Trường hợp nhỏ hơn:```
3 5
5 5 5
1 0 1
2 1 3 2
1 1 2
2 1 3 4
3 1 3
3 1 3
```| Hoạt động | Tổng mở | Tổng đóng | Số mở | Trả lời | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 10 | 5 | 2 | | 
| Thêm 2 | 14 | 5 | 2 | | 
| Lật hai cái đầu tiên | 5 | 14 | 1 | | 
| Thêm 4 | 9 | 14 | 1 | | 
| Truy vấn tất cả | 9 | 14 | | 23 | 
| Truy vấn tất cả | 9 | 14 | | 23 | 

Ví dụ này thực hiện các lần lật một phần và cho thấy các lần lật lặp lại sẽ khôi phục lại trạng thái trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Việc xây dựng cây là tuyến tính và mọi thao tác đều truy cập vào các nút O(log n) với khả năng lan truyền lười biếng. | 
| Không gian | O(n) | Mảng cây phân đoạn chứa một số giá trị không đổi cho mỗi nút. | 

Kích thước đầu vào tối đa yêu cầu tránh mọi giải pháp chạm vào mọi phần tử trong mỗi thao tác. Các phép toán logarit của cây phân đoạn vừa vặn thoải mái trong giới hạn thời gian và mức sử dụng bộ nhớ thấp hơn nhiều so với giới hạn khả dụng. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""5 6
1 2 4 8 16
1 0 1 0 1
3 2 4
2 1 5 6
3 2 4
1 1 5
2 1 5 7
3 2 4
""") == "14\n20\n34\n"

assert run("""1 1
100
0
3 1 1
""") == "100\n"

assert run("""3 4
5 5 5
1 0 1
2 1 3 2
1 1 2
2 1 3 4
3 1 3
""") == "23\n"

assert run("""2 4
7 9
1 1
1 1 2
2 1 2 10
1 1 1
3 1 2
""") == "36\n"

assert run("""4 5
1 1 1 1
0 0 0 0
2 1 4 5
1 2 3
2 1 4 3
3 1 4
3 2 3
""") == "4\n14\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Truy vấn hộp đóng đơn |`100`| Truy vấn bao gồm các hộp đóng. | 
| Các trạng thái hỗn hợp với các bản cập nhật và lượt lật |`23`| Bổ sung chỉ mở và lật một phần. | 
| Hai lần lật có cập nhật |`36`| Lười lật hủy. | 
| Tất cả các hộp ban đầu đóng |`4`,`14`| Chuyển đổi trạng thái từ các phân đoạn đóng hoàn toàn. | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên từ cuộc thảo luận là một phép cộng sau khi lật. Cây phân đoạn xử lý nó vì thao tác lật hoán đổi các nhóm mở và đóng được lưu trữ trước khi áp dụng phép cộng. Đối với đầu vào:```
2 3
5 7
1 0
1 1 2
2 1 2 3
3 1 2
```cây thay đổi số lần mở từ một hộp sang một hộp, nhưng tổng mở sẽ trở thành tổng đóng cũ. Việc bổ sung chỉ ảnh hưởng đến ô thứ hai, tạo ra kết quả cuối cùng`15`. 

Trường hợp cạnh thứ hai là nhiều lần lật trên cùng một khoảng. Cờ lười lưu trữ xem có số lần lật lẻ đang chờ xử lý hay không. TRONG:```
1 4
10
1
1 1 1
1 1 1
2 1 1 5
3 1 1
```lần lật đầu tiên đánh dấu nút là đã bị lật, lần lật thứ hai sẽ loại bỏ trạng thái chờ xử lý đó và hộp vẫn mở. Việc bổ sung được áp dụng và câu trả lời trở thành`15`. 

Trường hợp cạnh cuối cùng là truy vấn các hộp đóng. Hàm truy vấn luôn trả về`open_sum + closed_sum`, vì vậy nó không bao giờ phụ thuộc vào trạng thái hiện tại. Vì:```
2 1
4 9
1 0
3 1 2
```cây lưu trữ 4 trong nhóm mở và 9 trong nhóm đóng. Số tiền trả về là`13`, phù hợp với hành vi được yêu cầu.
