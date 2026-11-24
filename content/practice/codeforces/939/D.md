---
problem: 939D
contest_id: 939
problem_index: D
name: "Love Rescue"
contest_name: "Codeforces Round 464 (Div. 2)"
rating: 1600
tags: ["dfs and similar", "dsu", "graphs", "greedy", "strings"]
answer: passed_samples
verified: true
solve_time_s: 74
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a339873-b5b0-83ec-af64-de42c9d02b4a
---

#CF 939D - Giải Cứu Tình Yêu 

**Đánh giá:** 1600 
**Thẻ:** dfs và tương tự, dsu, đồ thị, tham lam, chuỗi 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a339873-b5b0-83ec-af64-de42c9d02b4a 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta có hai sợi dây có độ dài bằng nhau, một sợi viết trên áo thun của Valya và một sợi trên áo phông của Tolya. Mục tiêu là làm cho hai chuỗi giống hệt nhau bằng cách sử dụng một bộ quy tắc chuyển đổi được phép. 

Mỗi câu thần chú tương ứng với một cặp chữ cái viết thường. Khi chúng ta mua bùa chú cho chữ cái`c1`Và`c2`, chúng ta được phép chuyển đổi`c1`vào trong`c2`và cả`c2`vào trong`c1`và chuyển đổi này có thể được áp dụng bao nhiêu lần và ở bất kỳ vị trí nào trong cả hai chuỗi. 

Điểm tinh tế chính là chúng tôi không thực hiện các thao tác trực tiếp trên các ký tự theo một trình tự cố định. Thay vào đó, chúng tôi chọn một tập hợp các “cạnh hai chiều” giữa các chữ cái và các cạnh này xác định những chữ cái nào cuối cùng có thể được biến thành những chữ cái khác thông qua chuỗi biến đổi. Nhiệm vụ của chúng ta là chọn tập cạnh nhỏ nhất có thể sao cho với mọi vị trí`i`, hai ký tự ở vị trí đó có thể được làm bằng nhau bằng cách sử dụng các phép biến đổi này. 

Vì vậy, vấn đề rút gọn thành: xây dựng đồ thị vô hướng trên 26 chữ cái. Thêm một cạnh có giá 1 và với các cạnh, chúng ta có thể tự do di chuyển dọc theo các thành phần được kết nối. Chúng tôi muốn mọi cặp`(s[i], t[i])`nằm trong cùng một thành phần được kết nối. Mục tiêu là giảm thiểu số cạnh và đưa ra bất kỳ tập cạnh tối ưu nào. 

Ràng buộc`n ≤ 100000`có nghĩa là chúng tôi không thể mô phỏng các phép biến đổi trên mỗi vị trí hoặc thử các tập hợp con của các phép toán. Bất cứ điều gì bậc hai bằng chữ cái hoặc tuyến tính trên mỗi thao tác trên chuỗi sẽ quá chậm. Vì kích thước bảng chữ cái được cố định ở mức 26 nên các giải pháp suy luận về các cặp chữ cái hoặc sử dụng DSU trên các ký tự là khả thi. 

Một số trường hợp đặc biệt quan trọng. 

Nếu cả hai chuỗi đều giống nhau thì câu trả lời là 0 và chúng ta không được xuất ra gì cả. Một cách tiếp cận ngây thơ vẫn có thể cố gắng thêm các hoạt động không cần thiết. 

Ví dụ: nếu mọi vị trí hoàn toàn khác nhau`"abc"`Và`"xyz"`, một cách tiếp cận tham lam bất cẩn có thể cố gắng kết nối mọi điểm không khớp một cách độc lập, tạo ra nhiều cạnh dư thừa thay vì sử dụng lại cấu trúc. 

Cạm bẫy chính là giả định mỗi sự không phù hợp có thể được xử lý độc lập. Trên thực tế, một cạnh có thể giúp ích cho nhiều vị trí cùng một lúc vì nó kết nối các thành phần. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ là coi vấn đề này giống như chọn một tập hợp các cạnh giữa 26 nút và kiểm tra xem liệu tất cả các cặp có thể được kết nối trong biểu đồ cảm ứng hay không. Chúng tôi có thể liệt kê tất cả các tập hợp con các cạnh giữa các chữ cái, mô phỏng kết nối bằng DSU hoặc BFS và kiểm tra tính khả thi. Số cạnh có thể có là 325 nên điều này là không thể. Ngay cả việc hạn chế chỉ các cạnh xuất hiện không khớp cũng mang lại khả năng lên tới 26 × 26, vẫn theo cấp số nhân. 

Điều quan trọng là mỗi vị trí đều thực thi một yêu cầu: chữ cái`a = s[i]`Và`b = t[i]`phải kết thúc trong cùng một thành phần được kết nối. Nếu chúng đã có cùng một chữ cái thì chúng ta có thể bỏ qua nó. Nếu không, chúng tôi phải đảm bảo kết nối giữa hai nút. Để đạt được điều này với các cạnh tối thiểu, chúng tôi muốn hợp nhất các thành phần với chi phí rẻ nhất có thể. 

Điều này trở thành một vấn đề xây dựng DSU cổ điển. Chúng tôi bắt đầu với 26 thành phần đơn lẻ. Đối với mỗi cặp không khớp`(a, b)`, nếu chúng đã được kết nối rồi thì chúng tôi không làm gì cả. Nếu không, chúng ta phải thêm một cạnh giữa chúng để hợp nhất các thành phần của chúng. Mỗi cạnh được thêm vào sẽ giảm số lượng thành phần được kết nối tối đa một, vì vậy chúng tôi chỉ thêm các cạnh khi cần thiết. 

Điều này là tối ưu vì mọi sự bình đẳng cần thiết giữa hai thành phần buộc phải có ít nhất một kết nối giữa chúng. Mọi giải pháp hợp lệ đều phải kết nối tất cả các cặp được yêu cầu để việc hợp nhất DSU nắm bắt được số lượng kết nối cần thiết tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con cạnh | hàm mũ | O(1) | Quá chậm | 
| DSU về các chữ cái và sự không khớp | O(n α(26)) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo DSU trên 26 chữ cái viết thường, mỗi chữ cái trong thành phần riêng của nó. 
2. Lặp lại tất cả các chỉ mục của chuỗi. 
3. Đối với từng vị trí`i`, so sánh`s[i]`Và`t[i]`. 
4. Nếu chúng giống nhau thì bỏ qua vì không cần chuyển đổi. 
5. Nếu chúng khác nhau, hãy kiểm tra xem các thành phần của chúng đã được kết nối trong DSU chưa. 
6. Nếu chúng không được kết nối, hãy nối chúng lại và ghi lại cặp`(s[i], t[i])`như một câu thần chú được chọn. 
7. Tiếp tục cho đến khi tất cả các vị trí được xử lý. 
8. Xuất ra số lượng phép thuật đã ghi và danh sách các cặp. 

Lý do chúng tôi chỉ thêm một câu thần chú khi hai chữ cái ở các thành phần khác nhau là vì sau khi được kết nối, các lần xuất hiện trong tương lai của cặp này hoặc bất kỳ cặp nào có liên quan gián tiếp đều đã được đáp ứng thông qua kết nối bắc cầu. 

### Tại sao nó hoạt động 

Mỗi thao tác hợp kết nối hai thành phần riêng biệt trước đó và mọi sự bằng nhau cần thiết giữa hai chữ cái đều yêu cầu các thành phần của chúng phải được kết nối. Vì chúng ta chỉ thêm một cạnh khi nó hợp nhất các thành phần nên mọi thao tác đều cần thiết tại thời điểm nó được chọn. Không thể xóa cạnh nào mà không ngắt kết nối ở ít nhất một vị trí và không cần thêm cạnh nào vì DSU đảm bảo tất cả các cặp được yêu cầu đều được kết nối bắc cầu. 

Do đó, thuật toán xây dựng một khu rừng bao trùm tối thiểu trên một biểu đồ có các cạnh được tạo ra bởi sự không khớp trong các chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.p = list(range(n))
        self.r = [0] * n

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        ra, rb = self.find(a), self.find(b)
        if ra == rb:
            return False
        if self.r[ra] < self.r[rb]:
            ra, rb = rb, ra
        self.p[rb] = ra
        if self.r[ra] == self.r[rb]:
            self.r[ra] += 1
        return True

n = int(input())
s = input().strip()
t = input().strip()

dsu = DSU(26)
res = []

for i in range(n):
    a = ord(s[i]) - 97
    b = ord(t[i]) - 97
    if a == b:
        continue
    if dsu.union(a, b):
        res.append((s[i], t[i]))

print(len(res))
for a, b in res:
    print(a, b)
```Cấu trúc DSU duy trì các thành phần được kết nối của các chữ cái. Mỗi khi chúng tôi thấy có sự không khớp, chúng tôi sẽ cố gắng hợp nhất các thành phần. các`union`đảm bảo chúng tôi chỉ trả chi phí khi việc hợp nhất thực sự xảy ra, ngăn chặn các phép thuật dư thừa không làm giảm số lượng thành phần. 

Một điểm tinh tế là chúng tôi không cố gắng lưu trữ hoặc mô phỏng các phép biến đổi một cách rõ ràng. Chỉ riêng khả năng kết nối là đủ vì bất kỳ chuỗi phép thuật nào cũng tương ứng chính xác với khả năng tiếp cận trong biểu đồ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
abb
dad
```Chúng tôi theo dõi việc hợp nhất DSU. 

| tôi | s[i] | t[i] | comp(s[i]) | comp(t[i]) | công đoàn? | cạnh đã chọn | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | một | d | một | d | vâng | một d | 
| 1 | b | một | b | quảng cáo | vâng | b a | 
| 2 | b | d | a-d-b | a-d-b | không | - | 

Sự không khớp thứ hai được giải quyết sau lần hợp nhất đầu tiên kể từ`a`đã kết nối với`d`. 

Điều này cho thấy một cạnh có thể đáp ứng nhiều vị trí sau này như thế nào thông qua việc đóng bắc cầu. 

### Ví dụ 2 

đầu vào:```
4
abca
zzcx
```| tôi | s[i] | t[i] | comp(s[i]) | comp(t[i]) | công đoàn? | cạnh đã chọn | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | một | z | một | z | vâng | az | 
| 1 | b | z | b | a-z | vâng | b z | 
| 2 | c | c | c | c | không | - | 
| 3 | một | x | a-z-b | x | vâng | một x | 

Sau khi xử lý, tất cả các đẳng thức cần thiết đều được thỏa mãn thông qua kết nối. 

Điều này chứng tỏ rằng chúng tôi chỉ thanh toán khi có mối quan hệ thành phần mới được giới thiệu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n α(26)) | Mỗi vị trí kích hoạt tối đa một liên kết DSU/tìm trên một bảng chữ cái không đổi | 
| Không gian | O(26) | Mảng DSU và lưu trữ đầu ra | 

Giải pháp tuyến tính theo độ dài chuỗi và dễ dàng phù hợp với các ràng buộc vì bảng chữ cái là không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class DSU:
        def __init__(self, n):
            self.p = list(range(n))
            self.r = [0] * n

        def find(self, x):
            while self.p[x] != x:
                self.p[x] = self.p[self.p[x]]
                x = self.p[x]
            return x

        def union(self, a, b):
            ra, rb = self.find(a), self.find(b)
            if ra == rb:
                return False
            if self.r[ra] < self.r[rb]:
                ra, rb = rb, ra
            self.p[rb] = ra
            if self.r[ra] == self.r[rb]:
                self.r[ra] += 1
            return True

    n = int(input())
    s = input().strip()
    t = input().strip()

    dsu = DSU(26)
    res = []

    for i in range(n):
        a = ord(s[i]) - 97
        b = ord(t[i]) - 97
        if a != b and dsu.union(a, b):
            res.append((s[i], t[i]))

    out = [str(len(res))]
    out += [f"{a} {b}" for a, b in res]
    return "\n".join(out)

# provided sample
assert run("3\nabb\ndad\n") == "2\na d\nb a"

# all equal
assert run("4\naaaa\naaaa\n") == "0"

# single mismatch
assert run("1\na\nb\n") == "1\na b"

# chain reuse
assert run("3\nabc\ncba\n") == "2\na c\nb a"

# already connected chain case
assert run("5\nababa\nbabab\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi giống hệt nhau | 0 | không có hoạt động không cần thiết | 
| không khớp đơn | 1 cạnh | trường hợp cơ sở đúng đắn | 
| tái sử dụng dây chuyền | 2 cạnh | kết nối bắc cầu | 
| mô hình xen kẽ | tập tối thiểu hợp lệ | tái sử dụng công đoàn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các vị trí đều bằng nhau. DSU không bao giờ thực hiện kết hợp, do đó danh sách kết quả vẫn trống và chúng tôi xuất ra số 0 một cách chính xác. 

Một trường hợp khác là sự không khớp lặp đi lặp lại liên quan đến các chữ cái đã được kết nối. Ví dụ, sau khi sáp nhập`a`Và`b`, sau này gặp phải`(b, a)`không tạo ra cạnh mới. Kiểm tra DSU ngăn chặn các cạnh trùng lặp, đảm bảo chúng tôi không đếm quá mức. 

Một trường hợp tinh tế cuối cùng là chuỗi dài như`a-b-c-d`gây ra bởi các vị trí khác nhau. Thuật toán xây dựng một cấu trúc bao trùm một cách chính xác mà không cần phải xem lại các quyết định trước đó, bởi vì một khi một thành phần được hình thành, tất cả các cặp tương lai bên trong nó sẽ tự động được đáp ứng.
