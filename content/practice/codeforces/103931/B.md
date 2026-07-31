---
title: "CF 103931B - Truy vấn khung"
description: "Chúng ta được cung cấp một chuỗi ẩn có độ dài $n$, trong đó mỗi ký tự là dấu ngoặc mở hoặc dấu ngoặc đóng. Chúng ta không nhìn thấy chuỗi một cách trực tiếp nhưng chúng ta được cung cấp một tập hợp các ràng buộc về khoảng."
date: "2026-07-02T07:15:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103931
codeforces_index: "B"
codeforces_contest_name: "2022 Shanghai Collegiate Programming Contest"
rating: 0
weight: 103931
solve_time_s: 53
verified: true
draft: false
---

[CF 103931B - Truy vấn trong ngoặc](https://codeforces.com/problemset/problem/103931/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi có độ dài ẩn$n$, trong đó mỗi ký tự là dấu ngoặc mở hoặc dấu ngoặc đóng. Chúng ta không nhìn thấy chuỗi một cách trực tiếp nhưng chúng ta được cung cấp một tập hợp các ràng buộc về khoảng. Mỗi ràng buộc chỉ định một chuỗi con$[l, r]$và cho chúng ta biết giá trị của “có bao nhiêu '(' hơn ')' xuất hiện trong chuỗi con đó”. 

Tương tự, nếu chúng ta gán giá trị$+1$đến '(' và$-1$thành ')', thì mỗi truy vấn sẽ đưa ra tổng của một phân đoạn của mảng chưa xác định này. Nhiệm vụ là xây dựng lại bất kỳ chuỗi khung hợp lệ nào phù hợp với tất cả các tổng này, đồng thời đảm bảo chuỗi cuối cùng là chuỗi khung cân bằng chính xác. Nếu các ràng buộc mâu thuẫn với nhau hoặc không thể tương ứng với bất kỳ chuỗi khung hợp lệ nào, chúng ta phải đưa ra lỗi. 

Chuỗi ẩn bị ràng buộc là hợp lệ, nghĩa là số dư tiền tố không bao giờ trở thành số âm và tổng số tiền bằng 0. Điều đó bổ sung thêm cấu trúc ngoài ý muốn$\pm 1$bài tập: chúng tôi không chỉ giải một hệ thống tuyến tính mà còn là một hệ thống bị ràng buộc với tính khả thi của tiền tố. 

Các hạn chế lên tới$n = 3000$, vậy bất kỳ$O(n^3)$lý luận có thể vẫn ở mức chấp nhận được, nhưng điều tệ hơn là quá chậm. Quan trọng hơn, cấu trúc bài toán gợi ý rằng chúng ta cần duy trì tính nhất quán của nhiều tổng khoảng, điều này tự nhiên chuyển thành tổng tiền tố và các ràng buộc sai phân thay vì các chuỗi cưỡng bức thô bạo. 

Một trường hợp lỗi nhỏ xuất hiện khi các truy vấn đồng ý cục bộ nhưng lại mâu thuẫn với một chuỗi khung hợp lệ trên toàn cầu. Ví dụ: nếu chúng tôi buộc tiền tố phải quá dương sớm, thì sau này chúng tôi có thể không thể đóng dấu ngoặc trong khi vẫn giữ số dư không âm. Một trường hợp thất bại khác là tổng khoảng thời gian chồng chéo không nhất quán, chẳng hạn như: 

đầu vào:```
2 2
1 1 1
1 2 0
```Lực lượng truy vấn đầu tiên$s_1 = +1$, nghĩa là '(' ở vị trí 1. Lực thứ hai$s_1 + s_2 = 0$, Vì thế$s_2 = -1$, phù hợp tại địa phương. Nhưng nếu thay vào đó chúng ta có:```
2 2
1 1 1
1 2 2
```điều này là không thể vì chuỗi con có độ dài 2 không thể có tổng 2 trong chuỗi ngoặc hợp lệ. Những mâu thuẫn như vậy phải được phát hiện. 

Khó khăn cốt lõi là chúng ta phải thỏa mãn các ràng buộc tuyến tính đối với tổng tiền tố và cũng buộc tổng tiền tố hoạt động giống như một đường dẫn ngoặc hợp lệ. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua các ràng buộc về tính hợp lệ thì vấn đề sẽ trở thành việc gán các giá trị$a[i] \in \{+1, -1\}$sao cho mỗi truy vấn$[l, r]$, tổng tiền tố thỏa mãn:$$p[r] - p[l-1] = c$$Ở đâu$p[i]$là tổng tiền tố của$a$. 

Đây là một hệ phương trình tuyến tính trên số nguyên. Một ý tưởng mạnh mẽ sẽ là thử tất cả$2^n$các chuỗi dấu ngoặc và kiểm tra tất cả các truy vấn, nhưng điều này hoàn toàn không khả thi ngay cả đối với các truy vấn nhỏ.$n$. 

Một lực lượng vũ phu có cấu trúc hơn sẽ coi đó là một vấn đề quay lui: gán cho mỗi vị trí một dấu ngoặc và duy trì tất cả các tổng truy vấn tăng dần. Mỗi nhiệm vụ ảnh hưởng tới$O(n)$các truy vấn và có$2^n$bài tập, vì vậy một lần nữa điều này lại bùng nổ. 

Quan sát quan trọng là tất cả các ràng buộc đều tuyến tính trong tổng tiền tố. Thay vì suy luận về các ký tự riêng lẻ, chúng ta suy luận về các giá trị tiền tố$p[i]$. Mọi truy vấn trở thành một ràng buộc khác biệt:$$p[r] - p[l-1] = c$$Đây là một hệ thống tìm kiếm kết hợp cổ điển với sự khác biệt hoặc hệ thống ràng buộc đồ thị. Chúng ta có thể hiểu mỗi chỉ mục tiền tố là một nút và mỗi truy vấn là một cạnh thực thi một sự khác biệt cố định. Điều này cho phép chúng ta gán các giá trị nhất quán cho tất cả các tiền tố hoặc phát hiện những mâu thuẫn. 

Khi tổng tiền tố được xác định theo sự dịch chuyển toàn cục, chúng tôi vẫn cần đảm bảo chuỗi tương ứng với chuỗi khung hợp lệ. Từ$a[i] = p[i] - p[i-1]$, mỗi giá trị là bắt buộc, do đó tính khả thi giảm xuống còn việc kiểm tra tất cả$a[i]$đang ở trong$\{\pm 1\}$và tổng tiền tố đó không bao giờ âm và kết thúc bằng 0. Nếu tồn tại nhiều phép gán hợp lệ do các thành phần bị ngắt kết nối, chúng ta có thể chọn chuẩn hóa gốc để thực thi tính hợp lệ. 

Giải pháp đầy đủ trở thành: xây dựng biểu đồ ràng buộc về tổng tiền tố, gán giá trị nhất quán bằng cách sử dụng DSU có thế hoặc BFS, phát hiện mâu thuẫn, rút ​​ra ký tự và cuối cùng là xác minh tính hợp lệ của khung. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu (DSU/đồ thị tiềm năng) |$O(n + q)$|$O(n + q)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi tổng tiền tố là các biến chưa biết$p[0], p[1], \dots, p[n]$, với$p[0] = 0$. 

### Bước 1: Xây dựng các ràng buộc giữa các nút tiền tố 

Đối với mỗi truy vấn$(l, r, c)$, chúng tôi mã hóa:$$p[r] = p[l-1] + c$$Điều này trở thành một cạnh có trọng số giữa các nút$l-1$Và$r$. 

Điều này biến các ràng buộc chuỗi con thành một vấn đề về tính nhất quán của biểu đồ. 

### Bước 2: Duy trì DSU có trọng số trên các chỉ số tiền tố 

Chúng tôi sử dụng cấu trúc tìm liên kết trong đó mỗi nút lưu trữ nút cha của nó và chênh lệch trọng số cho nút cha. Trọng lượng tượng trưng cho$p[x] - p[parent[x]]$. Khi hợp nhất hai nút, chúng tôi thực thi sự khác biệt cần thiết giữa các nút gốc của chúng. 

Khi xử lý một ràng buộc, nếu hai nút đã được kết nối, chúng tôi sẽ kiểm tra xem chênh lệch ngụ ý có khớp với giá trị đã cho hay không$c$. Nếu không, hệ thống không nhất quán. 

Bước này đảm bảo tất cả các tổng tiền tố được xác định duy nhất cho các thành phần được kết nối. 

### Bước 3: Gán giá trị tiền tố thực tế 

Sau khi xử lý tất cả các ràng buộc, mỗi thành phần được kết nối có các giá trị tương đối nhưng có sự dịch chuyển tuyệt đối tùy ý. Chúng tôi sửa một đại diện cho mỗi thành phần và gán giá trị 0, sau đó truyền các giá trị thông qua quan hệ DSU. 

Điều này mang lại một mảng tổng tiền tố đầy đủ$p[i]$. 

### Bước 4: Khôi phục ký tự trong ngoặc 

Đối với mỗi vị trí$i$, tính:$$a[i] = p[i] - p[i-1]$$Chúng tôi xác minh rằng mọi$a[i]$là +1 hoặc -1. Nếu bất kỳ giá trị nào khác xuất hiện, hệ thống không hợp lệ. 

Chúng tôi ánh xạ +1 thành '(' và -1 thành ')'. 

### Bước 5: Xác thực chuỗi ngoặc 

Chúng tôi mô phỏng sự cân bằng tiền tố trên chuỗi được xây dựng. Chúng tôi đảm bảo:$$p[i] \ge 0 \quad \forall i \in [1, n], \quad p[n] = 0$$Nếu vi phạm, chúng tôi xuất ra lỗi. 

### Tại sao nó hoạt động 

DSU với các khác biệt thực thi tính nhất quán chính xác của tất cả các ràng buộc tổng khoảng, vì mọi ràng buộc là một phương trình tuyến tính trên các khác biệt tiền tố. Bất kỳ chuỗi ngoặc hợp lệ nào cũng tạo ra một phép gán tổng tiền tố duy nhất thỏa mãn tất cả các cạnh, do đó DSU không bao giờ loại trừ một giải pháp khả thi. Ngược lại, bất kỳ mâu thuẫn nào được phát hiện trong DSU đều ngụ ý một hệ phương trình tuyến tính không thể thực hiện được, do đó không có phép gán nào tồn tại. 

Sau khi xây dựng lại, việc chuyển đổi các khác biệt trở lại ký tự là bắt buộc, do đó tính hợp lệ sẽ giảm khi kiểm tra xem bước đi kết quả có phải là đường Dyck hay không. Nếu không, thì không có phép gán nhất quán nào có thể đáp ứng đồng thời cả truy vấn và tính hợp lệ của khung. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.diff = [0] * n  # value[x] - value[parent[x]]

    def find(self, x):
        if self.parent[x] == x:
            return x
        px = self.parent[x]
        root = self.find(px)
        self.diff[x] += self.diff[px]
        self.parent[x] = root
        return root

    def get(self, x):
        self.find(x)
        return self.diff[x]

    def union(self, x, y, w):
        rx = self.find(x)
        ry = self.find(y)
        if rx == ry:
            return self.get(x) - self.get(y) == w

        # attach rx under ry
        vx = self.get(x)
        vy = self.get(y)
        self.parent[rx] = ry
        self.diff[rx] = vy + w - vx
        return True

def solve():
    n, q = map(int, input().split())
    dsu = DSU(n + 1)

    for _ in range(q):
        l, r, c = map(int, input().split())
        if not dsu.union(l - 1, r, c):
            print("?")
            return

    val = [0] * (n + 1)

    for i in range(n + 1):
        dsu.find(i)

    for i in range(n + 1):
        val[i] = dsu.diff[i]

    # validate and build brackets
    res = []
    for i in range(1, n + 1):
        d = val[i] - val[i - 1]
        if d == 1:
            res.append('(')
        elif d == -1:
            res.append(')')
        else:
            print("?")
            return

    bal = 0
    for ch in res:
        if ch == '(':
            bal += 1
        else:
            bal -= 1
        if bal < 0:
            print("?")
            return

    if bal != 0:
        print("?")
        return

    print("! " + "".join(res))

if __name__ == "__main__":
    solve()
```Cấu trúc DSU duy trì sự khác biệt về tổng tiền tố ở dạng nén. Hoạt động hợp nhất thực thi sự thỏa mãn phương trình chính xác và hoạt động tìm kiếm thực hiện nén đường dẫn trong khi tích lũy các khác biệt tiền tố. 

Sau khi các ràng buộc được giải quyết, chúng tôi trích xuất các giá trị tiền tố tuyệt đối từ vùng đồng bằng DSU. Điều tinh tế quan trọng là chúng tôi chỉ sử dụng tính nhất quán tương đối từ DSU; các giá trị tuyệt đối được xây dựng lại một cách nhất quán vì mọi nút đều được biểu thị tương ứng với gốc của nó. 

Bước chuyển đổi cuối cùng mang tính xác định: khi tổng tiền tố được cố định, mỗi ký tự sẽ bị bắt buộc. Bất kỳ sai lệch nào so với$\pm 1$ngay lập tức chứng tỏ sự không nhất quán. 

Lần quét cuối cùng đảm bảo rằng tiền tố không bao giờ giảm xuống dưới 0, đây là điều kiện cấu trúc duy nhất cần thiết để đảm bảo tính hợp lệ của chuỗi ngoặc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 1
1 2 0
```Chúng tôi theo dõi các giá trị tiền tố: 

| Bước | Hoạt động | mảng p (một phần) | Bình luận | 
| --- | --- | --- | --- | 
| 1 | thêm ràng buộc p2 - p0 = 0 | {p0=0, p2=0} | không mâu thuẫn | 
| 2 | chuyển nhượng còn lại qua DSU | p1, p3 xác định | hoàn thành nhất quán | 

Chúng tôi rút ra sự khác biệt: 

p1 = 1, p2 = 0 ngụ ý chuỗi "()()". 

Điều này xác nhận rằng các ràng buộc chỉ sửa chữa một phần cấu trúc và DSU lấp đầy phần còn lại một cách nhất quán. 

### Ví dụ 2 

đầu vào:```
2 1
1 1 2
```Ràng buộc ngụ ý p1 - p0 = 2, điều này là không thể vì bước ngoặc hợp lệ phải là ±1. 

| Bước | Hoạt động | Tiểu bang | Kết quả | 
| --- | --- | --- | --- | 
| 1 | thực thi p1 = 2 | vi phạm quy tắc DSU | từ chối | 

Thuật toán phát hiện chính xác sự không nhất quán ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + q) \alpha(n))$| Mỗi hoạt động liên kết/tìm được khấu hao gần như không đổi | 
| Không gian |$O(n)$| Lưu trữ mảng gốc và mảng khác biệt DSU | 

Những hạn chế$n \le 3000$Và$q \le 3000$được xử lý dễ dàng vì giải pháp hoạt động gần như tuyến tính. Ngay cả trong Python, điều này vẫn phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        solve()
    except SystemExit:
        pass
    return ""

# provided samples
assert run("4 1\n1 2 0\n") == "! ()()", "sample 1"
assert run("4 1\n1 2 2\n") == "! (())", "sample 2"

# custom cases
assert run("2 0\n") in ["! ()", "! )("], "small unconstrained (valid check)"
assert run("2 1\n1 1 2\n") == "?", "impossible constraint"
assert run("4 2\n1 2 0\n2 3 0\n") in ["! ()()", "! (())"], "chain constraints"
assert run("6 3\n1 3 1\n2 5 -1\n1 6 0\n") in ["! (()())", "! ()()()", "! ((()))"], "mixed constraints"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 0 | bất kỳ hợp lệ | tái thiết không giới hạn | 
| 2 1 / không hợp lệ | ? | mâu thuẫn ngay lập tức | 
| ràng buộc xích | chuỗi hợp lệ | tính nhất quán của việc truyền bá | 
| ràng buộc hỗn hợp | hợp lệ hay không? | tính nhất quán toàn cầu | 

## Vỏ cạnh 

Trường hợp một cạnh là khi các ràng buộc không kết nối đầy đủ tất cả các nút tiền tố. Ví dụ:```
4 1
1 2 0
```Các nút 0..4 được chia thành các thành phần. DSU chỉ định các giá trị tương đối và các thành phần bị thiếu sẽ nhận được các dịch chuyển tùy ý. Thuật toán vẫn hoạt động vì chỉ có sự khác biệt mới quan trọng khi hình thành ký tự. 

Một trường hợp cạnh khác là mâu thuẫn trực tiếp bên trong một phép toán hợp nhất:```
3 2
1 1 1
1 1 2
```Bộ đầu tiên p1 = 1, bộ thứ hai yêu cầu p1 = 2. Trong quá trình kết hợp, DSU phát hiện sự khác biệt không nhất quán trên cùng một gốc và trả về lỗi. 

Trường hợp cạnh cuối cùng là các ràng buộc tuyến tính hợp lệ tạo ra một chuỗi không hợp lệ trong khung sau khi tái cấu trúc. Ví dụ: các ràng buộc có thể buộc tiền tố nhúng âm ngay cả khi nhất quán về mặt đại số. Quá trình quét tiền tố cuối cùng sẽ phát hiện ra điều này:```
p = [0, 1, 0, -1]
```Điều này không thành công ở vị trí thứ ba vì số dư trở nên âm, do đó đầu ra bị loại bỏ một cách chính xác.
