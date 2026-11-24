---
title: "CF 928A - Xác minh đăng nhập"
description: "Chúng tôi được cấp một tên người dùng được đề xuất và một tập hợp các tên người dùng đã được đăng ký. Nhiệm vụ là quyết định xem tên người dùng mới có “an toàn” để sử dụng hay không, nghĩa là nó không được coi là tương đương với bất kỳ tên người dùng hiện có nào theo một bộ quy tắc chuyển đổi."
date: "2026-06-17T03:06:50+07:00"
tags: ["codeforces", "competitive-programming", "*special", "strings"]
categories: ["algorithms"]
codeforces_contest: 928
codeforces_index: "A"
codeforces_contest_name: "VK Cup 2018 - \u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u044f 1"
rating: 1200
weight: 928
solve_time_s: 77
verified: true
draft: false
---

[CF 928A - Xác minh đăng nhập](https://codeforces.com/problemset/problem/928/A) 

**Đánh giá:** 1200 
**Thẻ:** *đặc biệt, dây 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một tên người dùng được đề xuất và một tập hợp các tên người dùng đã được đăng ký. Nhiệm vụ là quyết định xem tên người dùng mới có “an toàn” để sử dụng hay không, nghĩa là nó không được coi là tương đương với bất kỳ tên người dùng hiện có nào theo một bộ quy tắc chuyển đổi. 

Hai tên người dùng được coi là tương đương nếu một tên người dùng có thể được chuyển đổi thành tên người dùng khác bằng cách áp dụng thay thế ký tự nhiều lần. Những sự thay thế này xác định các lớp ký tự tương đương: luôn cho phép thay đổi kiểu chữ giữa chữ hoa và chữ thường, một số chữ cái và chữ số nhất định có thể được hoán đổi theo những cách cụ thể và một số ký tự có thể hoán đổi cho nhau một cách hiệu quả thông qua chuỗi các quy tắc này. Ý tưởng chính là chúng ta không so sánh các chuỗi theo nghĩa đen mà so sánh ý nghĩa chính tắc của chúng theo các giá trị tương đương này. 

Các ràng buộc đầu vào rất nhỏ: tối đa 1000 tên người dùng hiện có, mỗi tên người dùng có độ dài lên tới 50. Điều này ngay lập tức gợi ý rằng cách tiếp cận O(n · L) là đủ, vì ngay cả việc kiểm tra từng ký tự của mỗi chuỗi cũng chỉ khoảng 50.000 thao tác. Bất kỳ giải pháp nào xử lý trước hoặc chuẩn hóa từng chuỗi một cách độc lập sẽ dễ dàng nằm trong giới hạn. 

Một vấn đề tế nhị phát sinh từ sự mơ hồ trong các quy tắc chuyển đổi. Một cách tiếp cận ngây thơ có thể cho rằng chỉ có một vài phép thay thế trực tiếp quan trọng, nhưng các quy tắc hình thành nên chuỗi tương đương. Ví dụ: nếu ký tự A có thể trở thành B và B có thể trở thành C thì A và C cũng tương đương. Bỏ qua việc đóng cửa chuyển tiếp dẫn đến so sánh không chính xác. 

Một cạm bẫy khác là coi các phép biến đổi là có hướng. Các quy tắc rõ ràng là hai chiều, do đó mọi sự thay thế phải được hiểu là một quan hệ tương đương vô hướng. 

Cuối cùng, một giải pháp bất cẩn có thể so sánh từng chuỗi ký tự sau khi chuẩn hóa đơn giản như viết thường. Điều này không thành công vì tồn tại các mối quan hệ chữ số-chữ cái (như 0 và O hoặc 1 và nhiều chữ cái), không được xử lý chỉ bằng cách chuẩn hóa kiểu chữ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: đối với mỗi lần đăng nhập hiện có, hãy cố gắng xác định xem liệu nó có thể được chuyển đổi thành thông tin đăng nhập mới bằng cách sử dụng các thay thế ký tự được phép hay không. Điều này sẽ yêu cầu chạy tìm kiếm hoặc BFS/DFS qua các phép biến đổi ký tự cho mỗi cặp chuỗi. Vì mỗi chuỗi có độ dài lên tới 50 và có tới 1000 chuỗi hiện có, điều này sẽ nhanh chóng trở nên tốn kém nếu chúng ta khám phá các trạng thái chuyển đổi trên mỗi vị trí ký tự. Trong trường hợp xấu nhất, mỗi so sánh ký tự có thể phân nhánh thành nhiều lựa chọn thay thế, dẫn đến sự bùng nổ theo cấp số nhân trên mỗi so sánh. 

Quan sát quan trọng là chúng ta thực sự không cần phải mô phỏng các phép biến đổi. Chúng ta chỉ cần một biểu diễn nhất quán của từng ký tự sao cho hai ký tự tương đương nhau khi và chỉ khi chúng ánh xạ tới cùng một đại diện. Điều này biến vấn đề thành một vấn đề xây dựng lớp tương đương cổ điển. Khi chúng tôi ánh xạ mọi ký tự thành dạng chuẩn, mọi thông tin đăng nhập sẽ trở thành một chuỗi chuẩn hóa và chúng tôi chỉ cần kiểm tra xem bất kỳ thông tin đăng nhập hiện tại đã chuẩn hóa nào có khớp với thông tin đăng nhập mới được chuẩn hóa hay không. 

Điều này làm giảm vấn đề từ tìm kiếm đồ thị trên chuỗi sang việc xử lý trước đơn giản một bộ ký tự hữu hạn, sau đó là so sánh tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm chuyển đổi vũ phu | O(n · L · phân nhánh) | O(L) | Quá chậm | 
| Bản đồ Canonical + So sánh | O(n · L + xử lý bảng chữ cái) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta xây dựng các lớp tương đương trên các ký tự. Vì bảng chữ cái nhỏ và cố định (chữ cái, chữ số, dấu gạch dưới), nên chúng ta có thể mã hóa cứng hoặc kết hợp một cách an toàn các mối quan hệ được ngụ ý bởi các quy tắc chuyển đổi.

1. Khởi tạo cấu trúc tập hợp rời rạc (DSU) trên tất cả các ký tự có thể. Mỗi ký tự bắt đầu trong bộ riêng của nó vì ban đầu chúng tôi giả định không có giá trị tương đương. 
2. Với mỗi quy tắc chuyển đổi, hãy ghép các ký tự tương ứng vào cùng một bộ. Mỗi lần hợp nhất thể hiện rằng hai ký tự có thể hoán đổi cho nhau trong biểu diễn cuối cùng. 
3. Sau khi tất cả các phép kết hợp được thực hiện, hãy xác định một đại diện chuẩn cho mỗi ký tự, ví dụ như ký tự nhỏ nhất về mặt từ điển trong tập hợp của nó. 
4. Chuyển đổi chuỗi đăng nhập mới thành dạng chuẩn bằng cách thay thế mọi ký tự bằng đại diện của nó. 
5. Đối với mỗi thông tin đăng nhập hiện có, hãy chuyển đổi nó thành dạng chuẩn bằng cách sử dụng cùng một ánh xạ. 
6. So sánh từng thông tin đăng nhập hiện tại được chuẩn hóa với thông tin đăng nhập mới được chuẩn hóa. 
7. Nếu tìm thấy bất kỳ kết quả trùng khớp nào, thông tin đăng nhập mới sẽ xung đột và chúng tôi xuất ra “Không”. Ngược lại, chúng ta xuất ra “Có”. 

Lý do điều này có tác dụng là vì cấu trúc DSU thu gọn tất cả các tương đương bắc cầu thành một lớp duy nhất. Bất kỳ chuỗi chuyển đổi hợp lệ nào giữa hai ký tự đều tương ứng chính xác với chúng thuộc cùng một thành phần được kết nối trong biểu đồ tương đương. Bằng cách thay thế mỗi ký tự bằng đại diện thành phần của nó, chúng tôi đảm bảo rằng tất cả các ký tự tương đương ánh xạ tới cùng một ký hiệu và tất cả các ký tự không tương đương ánh xạ khác nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self):
        self.parent = {}
        self.rank = {}

    def find(self, x):
        if x not in self.parent:
            self.parent[x] = x
            self.rank[x] = 0
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, a, b):
        ra, rb = self.find(a), self.find(b)
        if ra == rb:
            return
        if self.rank[ra] < self.rank[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        if self.rank[ra] == self.rank[rb]:
            self.rank[ra] += 1

# Build equivalence relations according to the problem rules
dsu = DSU()

def add_equiv(a, b):
    dsu.union(a, b)

# case-insensitive letters
for c in "abcdefghijklmnopqrstuvwxyz":
    add_equiv(c, c.upper())
    add_equiv(c.upper(), c)

# 'O' and '0'
add_equiv('O', '0')
add_equiv('0', 'O')

# '1' and {l, I}
add_equiv('1', 'l')
add_equiv('1', 'I')
add_equiv('l', 'I')

# closure is handled by DSU

def normalize(s):
    return ''.join(dsu.find(c) for c in s)

s = input().strip()
n = int(input())
target = normalize(s)

for _ in range(n):
    t = input().strip()
    if normalize(t) == target:
        print("No")
        sys.exit()

print("Yes")
```Giải pháp này xây dựng cấu trúc tìm liên kết trên các ký tự và hợp nhất tất cả các cặp được khai báo tương đương. các`normalize`hàm chuyển đổi bất kỳ thông tin đăng nhập nào thành biểu diễn chuẩn bằng cách thay thế từng ký tự bằng gốc DSU của nó. Việc so sánh sau đó trở thành đẳng thức chuỗi chính xác. 

Một chi tiết triển khai tinh tế là đảm bảo rằng tất cả các ký tự xuất hiện trong đầu vào đều được khởi tạo trong DSU một cách chậm rãi, vì không phải tất cả các ký hiệu đều nhất thiết phải có trong các liên kết được xác định trước. Một điểm quan trọng khác là các hoạt động hợp nhất phải phản ánh tính đối xứng một cách rõ ràng, ngay cả khi các quy tắc đã hàm ý tính hai chiều. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1_wat
2
2_wat
wat_1
```Chúng tôi tính toán các biểu diễn kinh điển. 

| Bước | Chuỗi | Chuẩn hóa | 
| --- | --- | --- | 
| 1 | 1_wat | một dạng như r1_wat (thay đổi tùy theo lựa chọn gốc DSU) | 
| 2 | 2_wat | gốc khác nhau cho 2 → không khớp | 
| 3 | wat_1 | 1 hợp nhất với l/I, do đó khớp trước | 

Chúng tôi so sánh: 

- thông tin đăng nhập mới so với "2_wat": không khớp 
- thông tin đăng nhập mới so với "wat_1": khớp 

Vì vậy câu trả lời là “Có”. 

Điều này cho thấy sự tương đương giữa các chữ số và chữ cái có thể tạo ra các kết quả khớp không rõ ràng từ chuỗi thô. 

### Ví dụ 2 

đầu vào:```
Codeforces
1
codef0rces
```| Bước | Chuỗi | Chuẩn hóa | 
| --- | --- | --- | 
| 1 | Lực lượng mã hóa | trường hợp gấp thống nhất + tương đương O/0 | 
| 2 | codef0rces | 0 trở thành O, sự khác biệt trong trường hợp biến mất | 

Sau khi chuẩn hóa, cả hai chuỗi trở nên giống hệt nhau, vì vậy chúng tôi phát hiện xung đột và xuất ra “Không”. 

Điều này chứng tỏ tại sao chỉ chuẩn hóa kiểu chữ đơn giản là không đủ: sự tương đương giữa chữ số và chữ cái là điều cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · L α(26)) | Mỗi chuỗi được quét một lần và mỗi lần tra cứu ký tự gần như không đổi do nén đường dẫn DSU | 
| Không gian | O(1) | Vũ trụ nhân vật cố định và nhỏ bé | 

Các ràng buộc cho phép hoạt động lên tới 50.000 ký tự, điều này không đáng kể. Ngay cả với chi phí DSU, giải pháp vẫn hoạt động thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    output = []

    class DSU:
        def __init__(self):
            self.parent = {}
            self.rank = {}

        def find(self, x):
            if x not in self.parent:
                self.parent[x] = x
                self.rank[x] = 0
            if self.parent[x] != x:
                self.parent[x] = self.find(self.parent[x])
            return self.parent[x]

        def union(self, a, b):
            ra, rb = self.find(a), self.find(b)
            if ra == rb:
                return
            if self.rank[ra] < self.rank[rb]:
                ra, rb = rb, ra
            self.parent[rb] = ra
            if self.rank[ra] == self.rank[rb]:
                self.rank[ra] += 1

    dsu = DSU()

    for c in "abcdefghijklmnopqrstuvwxyz":
        dsu.union(c, c.upper())
        dsu.union(c.upper(), c)

    dsu.union('O', '0')
    dsu.union('0', 'O')

    dsu.union('1', 'l')
    dsu.union('1', 'I')
    dsu.union('l', 'I')

    def norm(s):
        return ''.join(dsu.find(c) for c in s)

    s = input().strip()
    n = int(input())
    target = norm(s)

    for _ in range(n):
        t = input().strip()
        if norm(t) == target:
            return "No"
    return "Yes"

# provided sample
assert run("1_wat\n2\n2_wat\nwat_1\n") == "Yes"

# same string direct conflict
assert run("a\n1\na\n") == "No"

# case-insensitive match
assert run("Code\n1\ncode\n") == "No"

# digit-letter equivalence
assert run("1\n1\nl\n") == "No"

# underscore unaffected
assert run("a_b\n1\nab\n") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| một vs một | Không | khớp chính xác giống hệt nhau | 
| Mã so với mã | Không | phân biệt chữ hoa chữ thường | 
| 1 vs l | Không | tương đương chữ số | 
| a_b vs ab | Có | dấu gạch dưới không tương đương | 

## Vỏ cạnh 

Trường hợp phức tạp là khi chuỗi tương đương trải dài qua nhiều bước. Ví dụ: nếu một ký tự kết nối gián tiếp thông qua một số thay thế, thì việc so sánh từng cặp đơn giản sẽ bỏ lỡ mối quan hệ. DSU xử lý việc này bằng cách thu gọn các kết nối bắc cầu thành một gốc duy nhất. Đối với một đầu vào như`l`, hiện có`I`và trung gian`1`, cả ba cuối cùng đều có cùng một đại diện, do đó các dạng chuẩn hóa trùng khớp một cách chính xác. 

Một trường hợp khác là trộn lẫn các phép biến đổi chữ và chữ số cùng một lúc. Ví dụ: một chuỗi chứa cả hai`O`Và`o`hoặc`0`vẫn phải giải quyết một cách nhất quán. Vì cả hai cách kết hợp chữ cái và chữ số đều được áp dụng trong cùng một cấu trúc, nên đại diện cuối cùng là ổn định bất kể thứ tự chuyển đổi. 

Cuối cùng, dấu gạch dưới là các ký hiệu biệt lập không có giá trị tương đương. Thuật toán không bao giờ kết hợp chúng với bất kỳ thứ gì, vì vậy chúng vẫn là các điểm cố định, đảm bảo các chuỗi chỉ khác nhau bởi dấu gạch dưới được phân biệt chính xác.
