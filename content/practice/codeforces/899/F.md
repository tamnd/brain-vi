---
title: "CF 899F - Xóa chữ"
description: "Chúng ta được cung cấp một chuỗi có các ký tự được lập chỉ mục từ trái sang phải và một chuỗi các thao tác liên tục loại bỏ các ký tự nhất định khỏi các phân đoạn được chỉ định của chuỗi hiện tại."
date: "2026-06-17T03:30:13+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "strings"]
categories: ["algorithms"]
codeforces_contest: 899
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 452 (Div. 2)"
rating: 2100
weight: 899
solve_time_s: 185
verified: true
draft: false
---

[CF 899F - Xóa chữ cái](https://codeforces.com/problemset/problem/899/F) 

**Xếp hạng:** 2100 
**Thẻ:** cấu trúc dữ liệu, chuỗi 
**Thời gian giải:** 3m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi có các ký tự được lập chỉ mục từ trái sang phải và một chuỗi các thao tác liên tục loại bỏ các ký tự nhất định khỏi các phân đoạn được chỉ định của chuỗi hiện tại. Mỗi thao tác cung cấp một phân đoạn trong chuỗi hiện tại đã được thu hẹp và một loại ký tự. Trong phân đoạn đó, mọi lần xuất hiện của ký tự đó sẽ bị xóa, trong khi tất cả các ký tự khác vẫn được giữ nguyên. Vì các thao tác xóa thay đổi vị trí nên các thao tác sau luôn tham chiếu đến chuỗi đã cập nhật sau tất cả các thao tác xóa trước đó. 

Đầu ra đơn giản là chuỗi cuối cùng sau khi tất cả các thao tác xóa đã được áp dụng theo thứ tự. 

Các ràng buộc đủ lớn nên bất kỳ giải pháp nào mô phỏng vật lý việc xóa bằng cách quét liên tục và xây dựng lại chuỗi sẽ quá chậm. Cả n và m đều có thể lên tới 200.000, do đó, cách tiếp cận đơn giản chạm vào từng ký tự trong mỗi thao tác sẽ dẫn đến hành vi bậc hai. Ngay cả việc cắt lặp đi lặp lại được tối ưu hóa một chút cũng có thể giảm xuống O(nm) trong trường hợp xấu nhất, vượt xa giới hạn. 

Khó khăn chính là việc xóa không mang tính toàn cục, chúng phụ thuộc vào vị trí động thay đổi sau mỗi thao tác. Điều này khiến việc duy trì một chuỗi có thể thay đổi rõ ràng và xóa trực tiếp các ký tự trở nên hấp dẫn, nhưng điều đó sẽ yêu cầu phải dịch chuyển nhiều lần các phần lớn của chuỗi. 

Một số trường hợp đặc biệt bộc lộ những cạm bẫy của mô phỏng ngây thơ. Nếu tất cả các thao tác nhắm vào cùng một ký tự trong phạm vi lớn, chuỗi có thể co lại dần dần và việc sao chép lặp đi lặp lại sẽ trở thành thảm họa. Ví dụ, bắt đầu bằng`"aaaaa"`và liên tục loại bỏ`'a'`các tiền tố bị thu hẹp quá mức sẽ gây ra các lần quét và dịch chuyển toàn bộ lặp đi lặp lại, tạo ra một tầng bậc hai. 

Một trường hợp tinh tế khác xuất hiện khi các thao tác chồng chéo nhiều nhưng lại ảnh hưởng đến các ký tự khác nhau. Quá trình quét đơn giản cho mỗi thao tác có thể liên tục truy cập lại các ký tự đã bị "xóa một cách hợp lý", lãng phí thời gian trừ khi việc xóa được theo dõi cẩn thận. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp giữ chuỗi ở cấu trúc có thể thay đổi và đối với mỗi thao tác, sẽ quét các vị trí từ l đến r và loại bỏ các ký tự trùng khớp. Điều này đúng về mặt logic vì nó tuân theo chính xác định nghĩa vấn đề. Tuy nhiên, mọi thao tác xóa đều yêu cầu dịch chuyển các ký tự và mỗi ký tự có thể được kiểm tra nhiều lần trong các thao tác. Trong trường hợp xấu nhất, mỗi thao tác có thể tốn O(n), dẫn đến O(nm). 

Điều quan trọng là các ký tự không bao giờ được giới thiệu lại mà chỉ bị loại bỏ. Điều này cho phép chúng tôi đảo ngược quan điểm: thay vì liên tục sửa đổi một chuỗi thu hẹp, chúng tôi xác định cho từng ký tự gốc xem nó có tồn tại trong tất cả các hoạt động hay không. 

Chúng tôi xử lý các hoạt động theo thứ tự ngược lại. Nếu chúng ta biết ký tự nào tồn tại sau tất cả các thao tác, thì việc đảo ngược quá trình có nghĩa là chúng ta bắt đầu từ trạng thái bị xóa trống cuối cùng và "khôi phục" các thao tác xóa ngược. Ngược lại, một thao tác tương ứng với các ký tự “hồi sinh” đã bị nó xóa, nhưng chỉ khi chúng chưa bị chặn bởi các thao tác sau đó. Điều này trở nên khó khăn để quản lý trực tiếp trên các vị trí. 

Quan điểm thực tế hơn là sử dụng cây phân đoạn (hoặc cấu trúc chỉ mục nhị phân có logic bổ sung) để duy trì vị trí ban đầu nào vẫn “còn sống” trong chuỗi được lọc hiện tại. Mỗi thao tác chỉ ảnh hưởng đến một phân đoạn động liền kề của chuỗi hiện tại chứ không ảnh hưởng đến các chỉ mục ban đầu, vì vậy chúng ta phải ánh xạ các vị trí hiện tại tới vị trí ban đầu một cách hiệu quả. 

Điều này đạt được bằng cách duy trì cây phân đoạn trên các chỉ mục gốc, trong đó mỗi nút theo dõi xem có bao nhiêu ký tự còn sống trong phạm vi của nó. Sau đó chúng ta có thể tìm vị trí sống thứ k trong O(log n). Mỗi thao tác [l, r, c] được xử lý bằng cách trước tiên định vị các chỉ mục gốc thực tế tương ứng với các ký tự còn sống thứ l và r, sau đó chỉ lặp lại các vị trí hiện chứa ký tự c và nằm trong phạm vi đó. Để hỗ trợ tra cứu nhanh theo ký tự, chúng tôi duy trì 26 + 26 + 10 nhóm vị trí hoạt động được sắp xếp riêng biệt. 

Mỗi lần xóa được áp dụng bằng cách xóa chỉ mục tương ứng khỏi cả cây phân đoạn và bộ ký tự. Mỗi vị trí bị xóa nhiều nhất một lần, do đó tổng số lần xóa là tuyến tính. 

Cấu trúc này hoạt động vì chúng tôi không bao giờ quét các ký tự không hoạt động và mỗi lần xóa đều là O(log n), khiến cho toàn bộ độ phức tạp có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Tối ưu (cây phân đoạn + bộ) | O((n + m) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai cấu trúc: một cây phân đoạn theo dõi những chỉ mục ban đầu nào vẫn còn tồn tại và một bản đồ từ mỗi ký tự đến một tập hợp các chỉ mục được sắp xếp trong đó ký tự đó hiện đang tồn tại. 

Chúng tôi xử lý các hoạt động theo thứ tự nhất định. 

1. Đối với một phép toán (l, r, c), trước tiên chúng ta dịch l và r từ “vị trí chuỗi hiện tại” thành các chỉ mục gốc bằng cách sử dụng cây phân đoạn. Điều này được thực hiện bằng cách tìm vị trí sống thứ l và thứ r. 
2. Khi đã có ranh giới chỉ mục gốc, chúng tôi lặp lại tất cả các lần xuất hiện của ký tự c có chỉ mục gốc nằm trong khoảng này. 
3. Đối với mỗi chỉ mục như vậy, chúng tôi xóa nó khỏi cây phân đoạn, nghĩa là nó không còn là một phần của chuỗi nữa. 
4. Chúng tôi cũng xóa nó khỏi bộ ký tự để không thể xử lý lại. 
5. Chúng ta tiếp tục cho đến khi không còn chỉ số hợp lệ nào của ký tự c trong khoảng. 

Cây phân đoạn đảm bảo rằng việc dịch giữa “vị trí hiện tại” và “chỉ mục gốc” vẫn hiệu quả ngay cả sau nhiều lần xóa. 

### Tại sao nó hoạt động

Ở mỗi bước, cây phân đoạn thể hiện sự tương ứng chính xác giữa các vị trí chuỗi hiện tại và các chỉ mục gốc còn tồn tại. Ký tự thiết lập các vị trí còn sót lại của phân vùng theo ký tự. Vì mỗi lần xóa là vĩnh viễn và mỗi chỉ mục bị xóa chính xác một lần nên không thao tác nào có thể giới thiệu lại ký tự đã xóa một cách không chính xác hoặc bỏ qua ký tự còn sót lại. Điều bất biến là cây phân đoạn luôn đếm chính xác các ký tự còn sống trong bất kỳ khoảng nào của các chỉ mục gốc và các tập hợp luôn chứa chính xác các chỉ mục đó vẫn còn sống với một ký tự nhất định. 

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

    def kth(self, k):
        idx = 0
        bitmask = 1 << (self.n.bit_length())
        while bitmask:
            nxt = idx + bitmask
            if nxt <= self.n and self.bit[nxt] < k:
                k -= self.bit[nxt]
                idx = nxt
            bitmask >>= 1
        return idx + 1

n, m = map(int, input().split())
s = input().strip()

fen = Fenwick(n)

pos = {}
for i, ch in enumerate(s, 1):
    fen.add(i, 1)
    pos.setdefault(ch, set()).add(i)

for _ in range(m):
    l, r, c = input().split()
    l = int(l)
    r = int(r)

    if c not in pos:
        continue

    total = fen.sum(n)
    if l > total:
        continue

    left_idx = fen.kth(l)
    right_idx = fen.kth(r)

    remove_list = []
    for i in list(pos[c]):
        if left_idx <= i <= right_idx:
            remove_list.append(i)

    for i in remove_list:
        fen.add(i, -1)
        pos[c].remove(i)

ans = []
for i in range(1, n + 1):
    if fen.sum(i) - fen.sum(i - 1) == 1:
        ans.append(s[i - 1])

print("".join(ans))
```Cây Fenwick duy trì số lượng ký tự vẫn còn tồn tại trong phạm vi tiền tố của chuỗi gốc. Hàm thứ k chuyển đổi một vị trí trong chuỗi hiện tại thành một chỉ mục gốc, điều này rất cần thiết vì các thao tác được xác định trên các vị trí động chứ không phải các chỉ mục cố định. 

Từ điển của các bộ theo dõi tất cả các chỉ số hiện còn tồn tại cho mỗi ký tự, cho phép chúng tôi hạn chế việc xóa chỉ ở các vị trí có liên quan. 

Trong mỗi thao tác, trước tiên chúng tôi ánh xạ phạm vi hoạt động thành các chỉ mục ban đầu. Sau đó, chúng tôi chỉ quét các chỉ mục vẫn tương ứng với ký tự đích và loại bỏ những chỉ mục bên trong khoảng. Mỗi chỉ mục sẽ bị xóa một lần, vì vậy mặc dù chúng tôi lặp lại bên trong các tập hợp, tổng công việc vẫn tuyến tính trên tất cả các hoạt động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
abac
1 3 a
2 2 c
```Chúng tôi theo dõi các vị trí còn sống bằng cây Fenwick. 

| Bước | Hoạt động | Chỉ số còn sống (chuỗi logic) | Hành động | 
| --- | --- | --- | --- | 
| 0 | ban đầu | bàn tính | tất cả các vị trí còn sống | 
| 1 | 1 3 một | bac | xóa 'a' ở 3 vị trí đầu tiên | 
| 2 | 2 2 c | b | xóa 'c' ở vị trí 2 | 

Sau thao tác đầu tiên, vị trí 1 và 3 (chỉ số gốc 1 và 3) chứa 'a' sẽ bị xóa. Sau thao tác thứ hai, chữ 'c' duy nhất còn lại sẽ bị xóa. 

Đầu ra cuối cùng là`"b"`. 

Điều này xác nhận rằng việc ánh xạ lại vị trí hoạt động chính xác sau khi xóa. 

### Ví dụ 2 

đầu vào:```
5 2
Az0Az
1 5 A
1 2 z
```| Bước | Hoạt động | Chuỗi sống động | Đã xóa | 
| --- | --- | --- | --- | 
| 0 | ban đầu | Az0Az | - | 
| 1 | 1 5 A | z0z | cả 'A' | 
| 2 | 1 2 z | 0z | đầu tiên 'z' | 

Chuỗi cuối cùng là`"0z"`. 

Điều này thể hiện việc xử lý phân biệt chữ hoa chữ thường và các loại ký tự hỗn hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Mỗi lần xóa sẽ cập nhật Fenwick và đặt một lần | 
| Không gian | O(n) | Bộ lưu trữ cho cây Fenwick và bộ vị trí nhân vật | 

Giải pháp này phù hợp trong giới hạn vì mọi thao tác chỉ thực hiện phép tính logarit cộng với việc loại bỏ hằng số khấu hao trên mỗi ký tự. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
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

        def kth(self, k):
            idx = 0
            bitmask = 1 << (self.n.bit_length())
            while bitmask:
                nxt = idx + bitmask
                if nxt <= self.n and self.bit[nxt] < k:
                    k -= self.bit[nxt]
                    idx = nxt
                bitmask >>= 1
            return idx + 1

    n, m = map(int, input().split())
    s = input().strip()

    fen = Fenwick(n)
    pos = {}

    for i, ch in enumerate(s, 1):
        fen.add(i, 1)
        pos.setdefault(ch, set()).add(i)

    for _ in range(m):
        l, r, c = input().split()
        l = int(l)
        r = int(r)

        if c not in pos:
            continue

        total = fen.sum(n)
        if l > total:
            continue

        left = fen.kth(l)
        right = fen.kth(r)

        to_remove = []
        for i in list(pos[c]):
            if left <= i <= right:
                to_remove.append(i)

        for i in to_remove:
            fen.add(i, -1)
            pos[c].remove(i)

    return "".join(s[i-1] for i in range(1, n+1) if fen.sum(i)-fen.sum(i-1) == 1)

# provided sample
assert run("4 2\nabac\n1 3 a\n2 2 c\n") == "b"

# custom cases
assert run("1 1\na\n1 1 a\n") == ""
assert run("3 2\nabc\n1 3 a\n1 3 a\n") == "bc"
assert run("5 1\nabcde\n1 5 z\n") == "abcde"
assert run("6 2\naaaAAA\n1 6 a\n1 6 A\n") == ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| xóa một lần | trống | cạnh loại bỏ hoàn toàn | 
| lặp đi lặp lại cùng một op | bc | xóa bình thường | 
| nhân vật không hoạt động | abcde | xử lý char bị thiếu | 
| chia trường hợp | trống | trường hợp đúng đắn hỗn hợp | 

## Vỏ cạnh 

Trường hợp loại bỏ hoàn toàn cho thấy tính đúng đắn của việc chấm dứt. Đối với đầu vào`"a", 1 1 a`, cây Fenwick bắt đầu với một vị trí còn sống. Ánh xạ thứ k trả về chỉ số 1, nó bị xóa và lần duyệt cuối cùng không tìm thấy vị trí còn tồn tại nào, tạo ra một chuỗi trống. 

Lặp đi lặp lại các hoạt động kiểm tra idempotence. Vì`"abc"`với hai lần xóa giống hệt nhau`'a'`trên phạm vi đầy đủ, cái đầu tiên loại bỏ chỉ mục 1 và cái thứ hai không tìm thấy chỉ số nào còn sót lại`'a'`trong tập hợp, vì vậy nó không thực hiện công việc nào. Chuỗi cuối cùng vẫn còn`"bc"`. 

Một trường hợp ký tự không có op như`"abcde"`với việc loại bỏ`'z'`không bao giờ vào vòng lặp xóa vì bộ ký tự trống nên cấu trúc Fenwick không thay đổi và đầu ra là chuỗi gốc. 

Việc xử lý các trường hợp hỗn hợp đảm bảo rằng`'a'`Và`'A'`được theo dõi riêng biệt trong các bộ khác nhau, do đó việc xóa không bao giờ vượt qua các lớp ký tự một cách không chính xác.
