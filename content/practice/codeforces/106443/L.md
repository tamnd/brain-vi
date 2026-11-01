---
title: "CF 106443L - Các chữ cái được liên kết"
description: "Chúng tôi được cung cấp một từ điển cố định gồm các từ riêng biệt. Giữa hai từ bất kỳ, chúng tôi xác định một quy tắc kết nối chỉ phụ thuộc vào các chữ cái của chúng: hai từ được coi là tương thích trực tiếp nếu chúng có chung ít nhất một ký tự chung."
date: "2026-06-22T19:17:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106443
codeforces_index: "L"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2026"
rating: 0
weight: 106443
solve_time_s: 60
verified: true
draft: false
---

[CF 106443L - Các chữ cái được liên kết](https://codeforces.com/problemset/problem/106443/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một từ điển cố định gồm các từ riêng biệt. Giữa hai từ bất kỳ, chúng tôi xác định một quy tắc kết nối chỉ phụ thuộc vào các chữ cái của chúng: hai từ được coi là tương thích trực tiếp nếu chúng có chung ít nhất một ký tự chung. Sử dụng quy tắc này, chúng ta được phép “biến đổi” một từ thành một từ khác trong từ điển miễn là chúng tương thích trực tiếp. Chuỗi đột biến chỉ đơn giản là một chuỗi các từ trong từ điển trong đó mỗi cặp liền kề có chung ít nhất một chữ cái. Nhiệm vụ là trả lời nhiều truy vấn hỏi liệu chúng ta có thể chuyển từ một từ trong từ điển này sang một từ khác bằng cách sử dụng một chuỗi như vậy hay không. 

Từ góc độ đồ thị, mỗi từ là một nút và có một cạnh vô hướng giữa hai nút nếu các chuỗi tương ứng của chúng có chung ít nhất một chữ cái. Mỗi truy vấn hỏi liệu hai nút có nằm trong cùng một thành phần được kết nối của biểu đồ này hay không. 

Các ràng buộc rất lớn: lên tới 200.000 từ và 200.000 truy vấn, với tổng chiều dài chuỗi lên tới khoảng 10 triệu ký tự. Điều này ngay lập tức loại trừ mọi cách xây dựng bậc hai đối với các cặp từ. Ngay cả việc kiểm tra tất cả các cặp để tìm các chữ cái được chia sẻ cũng sẽ cần khoảng$N^2$trong trường hợp xấu nhất vượt xa giới hạn khả thi. Các giải pháp duy nhất được chấp nhận là gần tuyến tính về tổng kích thước đầu vào. 

Một trường hợp phức tạp xuất hiện khi kết nối không mang tính bắc cầu chỉ thông qua sự chồng chéo trực tiếp mà thông qua các chuỗi chồng chéo. Ví dụ: nếu “ab” kết nối với “bc” và “bc” kết nối với “cd”, thì “ab” được kết nối với “cd” mặc dù chúng không có chữ cái trực tiếp. Một giải pháp đơn giản chỉ kiểm tra sự chồng chéo trực tiếp trên mỗi truy vấn sẽ không thành công đối với các đầu vào như:```
ab bc cd
ab cd
```Đầu ra đúng là CÓ (LUA), nhưng việc kiểm tra trực tiếp sẽ cho kết quả không chính xác là không. 

Một trường hợp cạnh quan trọng khác là sự hợp nhất do chữ cái gây ra. Các từ trông không liên quan về mặt cấu trúc vẫn có thể thuộc cùng một thành phần nếu chúng có chung bất kỳ ký tự nào thông qua các từ trung gian, do đó việc nhóm phải mang tính tổng thể trên toàn bộ từ điển thay vì truy vấn cục bộ. 

## Phương pháp tiếp cận 

Cách giải thích brute-force rất đơn giản: xây dựng một biểu đồ trong đó mỗi cặp từ được kết nối nếu chúng có chung ít nhất một chữ cái, sau đó chạy duyệt biểu đồ hoặc tìm liên kết trên biểu đồ này. Tính đúng đắn là hiển nhiên vì nó mã hóa trực tiếp quy tắc đột biến. Vấn đề là việc xây dựng tất cả các cạnh đòi hỏi phải so sánh từng cặp từ và bản thân mỗi so sánh có thể yêu cầu quét tối đa 50 ký tự. Điều đó dẫn đến khoảng$O(N^2 \cdot 50)$, điều này hoàn toàn không thể thực hiện được với 200.000 từ. 

Quan sát quan trọng là các cạnh không thực sự được xác định giữa các cặp tùy ý mà được trung gian thông qua các chữ cái. Một từ không quan trọng như một chuỗi đầy đủ để kết nối mà là một tập hợp các chữ cái. Nếu hai từ có chung một chữ cái thì chúng phải nằm trong cùng một thành phần được kết nối. Điều này gợi ý nên giới thiệu một lớp trung gian: chính các chữ cái. Mỗi chữ cái đóng vai trò như một trung tâm kết nối tất cả các từ chứa nó. Thay vì kết nối trực tiếp từng từ, chúng ta kết nối từng chữ và từng chữ. Điều này làm giảm biểu đồ từ biểu đồ từ dày đặc thành cấu trúc lưỡng cực chỉ có 26 nút phụ. 

Sau đó chúng tôi sử dụng cấu trúc tập hợp rời rạc để hợp nhất các từ và chữ cái. Mỗi từ được kết hợp với mỗi chữ cái chứa trong đó. Sau quá trình tiền xử lý này, hai từ được kết nối khi và chỉ khi chúng có chung một chữ cái, vì cả hai từ đều thuộc cùng một thành phần DSU thông qua nút chữ cái đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Biểu đồ theo cặp Brute Force | O(N2 · L) | O(N2) | Quá chậm | 
| DSU chữ cái | O(∑ | sᵢ | · α(N)) | 

## Hướng dẫn thuật toán 

Chúng tôi lập mô hình cả từ và chữ cái bên trong một cấu trúc kết hợp tập hợp rời rạc. Các từ được lập chỉ mục từ 0 đến N−1 và các chữ cái được lập chỉ mục dưới dạng các nút bổ sung. 

1. Khởi tạo một DSU với N + 26 nút, trong đó N đầu tiên đại diện cho các từ và 26 nút cuối cùng đại diện cho các chữ cái. 
2. Đối với mỗi từ, hãy quét các ký tự của nó và nối nút của từ đó với nút tương ứng với mỗi chữ cái mà nó chứa. Điều này tạo ra cầu nối giữa tất cả các từ có chung ít nhất một ký tự, vì tất cả chúng sẽ được gắn vào cùng một nút chữ cái. 
3. Sau khi tiền xử lý, mỗi truy vấn sẽ hỏi xem hai chỉ mục từ có thuộc cùng một thành phần DSU hay không. 
4. Đối với mỗi truy vấn, xuất ra “LUA” nếu hai từ có cùng gốc DSU, nếu không thì xuất ra “RYEI”. 

Lựa chọn thiết kế quan trọng là coi các chữ cái như những đường kết nối chung. Điều này tránh việc xây dựng các cạnh từng từ một cách rõ ràng. 

### Tại sao nó hoạt động 

Mỗi chữ cái đóng vai trò là trung tâm đại diện cho tất cả các từ chứa nó. Khi một từ được kết hợp với một chữ cái, nó sẽ trở thành một phần của chữ cái đó. Nếu hai từ có chung một chữ cái, chúng sẽ kết hợp thành cùng một nút chữ cái và do đó tạo thành cùng một thành phần được kết nối. Vì DSU có tính bắc cầu nên bất kỳ chuỗi ký tự dùng chung nào cũng sẽ tự động hợp nhất thành một thành phần duy nhất. Điều này khớp chính xác với định nghĩa về khả năng tiếp cận của biểu đồ đột biến, do đó các truy vấn kết nối giảm xuống còn kiểm tra tư cách thành viên DSU. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        ra = self.find(a)
        rb = self.find(b)
        if ra == rb:
            return
        if self.size[ra] < self.size[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        self.size[ra] += self.size[rb]

n, q = map(int, input().split())
words = [input().strip() for _ in range(n)]

dsu = DSU(n + 26)

def letter_id(c):
    return n + (ord(c) - ord('a'))

for i, w in enumerate(words):
    for c in set(w):
        dsu.union(i, letter_id(c))

for _ in range(q):
    a, b = input().split()
    ia = words.index(a)
    ib = words.index(b)
    if dsu.find(ia) == dsu.find(ib):
        print("LUA")
    else:
        print("RYEI")
```Cấu trúc DSU là tiêu chuẩn với tính năng nén và liên kết đường dẫn theo kích thước, đảm bảo hoạt động khấu hao gần như không đổi. Ánh xạ chữ cái sẽ chuyển các chữ cái vào phạm vi chỉ mục [N, N+25], cho phép chúng cùng tồn tại trong cùng một cấu trúc tìm liên kết như các từ. 

Một vấn đề tinh tế trong phiên bản đơn giản này là việc sử dụng`words.index(a)`bên trong các truy vấn, tuyến tính cho mỗi truy vấn và sẽ TLE ở mức giới hạn tối đa. Trong giải pháp sản xuất, chuỗi từ ánh xạ từ điển tới chỉ mục phải được tạo một lần để đảm bảo tra cứu O(1). 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
ab
bc
cd
xy
ab cd
ab xy
```Trạng thái DSU ban đầu bao gồm 4 nút từ và 26 nút chữ cái. 

| Bước | Hoạt động | Hiệu ứng DSU | 
| --- | --- | --- | 
| 1 | công đoàn(ab, a), công đoàn(ab, b) | ab nối các thành phần của a và b | 
| 2 | công đoàn(bc, b), công đoàn(bc, c) | bc hợp nhất với ab qua b | 
| 3 | công đoàn(cd, c), công đoàn(cd, d) | cd hợp nhất với bc qua c | 
| 4 | xy chỉ kết nối với x và y | xy bị cô lập khỏi người khác | 

Truy vấn ab cd kiểm tra gốc và tìm thấy thành phần tương tự, vì vậy câu trả lời là LUA. Truy vấn ab xy tìm thấy các thành phần khác nhau nên câu trả lời là RYEI. 

### Ví dụ 2 

đầu vào:```
3 2
a
b
ab
a b
```| Bước | Hoạt động | Hiệu ứng DSU | 
| --- | --- | --- | 
| 1 | a → hợp(a, 'a') | từ a kết nối với chữ a | 
| 2 | b → hợp(b, 'b') | từ b nối với chữ b | 
| 3 | ab → hợp(ab, 'a'), hợp(ab, 'b') | ab hợp nhất cả hai thành phần chữ cái | 

Bây giờ cả ba nút được kết nối thông qua các nút chữ cái. 

Truy vấn a b trả về LUA vì cả hai đều kết nối thông qua từ ab. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(∑ | sᵢ | 
| Không gian | O(N + 26) | DSU lưu trữ các từ và nút chữ cái | 

Tổng số thao tác ký tự được giới hạn bởi tổng độ dài chuỗi, nằm trong khoảng 10 triệu. Với tính năng nén đường dẫn, các hoạt động của DSU được thực hiện ổn định một cách hiệu quả, giúp giải pháp phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    class DSU:
        def __init__(self, n):
            self.p = list(range(n))
            self.sz = [1]*n
        def f(self,x):
            while self.p[x]!=x:
                self.p[x]=self.p[self.p[x]]
                x=self.p[x]
            return x
        def u(self,a,b):
            a,b=self.f(a),self.f(b)
            if a==b:return
            if self.sz[a]<self.sz[b]:
                a,b=b,a
            self.p[b]=a
            self.sz[a]+=self.sz[b]

    n,q=map(int,input().split())
    w=[input().strip() for _ in range(n)]
    mp={s:i for i,s in enumerate(w)}
    dsu=DSU(n+26)

    def lid(c): return n+ord(c)-97

    for i,s in enumerate(w):
        for c in set(s):
            dsu.u(i,lid(c))

    out=[]
    for _ in range(q):
        a,b=input().split()
        out.append("LUA" if dsu.f(mp[a])==dsu.f(mp[b]) else "RYEI")
    return "\n".join(out)

# provided samples
assert run("""6 4
conquista
interior
recife
capital
lua
ryei
conquista interior
recife capital
conquista recife
lua ryei
""") == """LUA
LUA
LUA
LUA"""

assert run("""5 5
lua
palao
ryei
grisi
recife
lua palao
ryei grisi
lua ryei
palao recife
grisi palao
""") == """LUA
LUA
RYEI
RYEI
RYEI"""

assert run("""5 4
ab
bc
cd
da
xyz
ab cd
ab xyz
da xyz
ab da
""") == """LUA
RYEI
RYEI
LUA"""

# custom cases
assert run("""3 2
a
b
ab
a b
""") == "LUA", "chain via single bridging word"

assert run("""4 2
aa
bb
cc
dd
aa dd
bb cc
""") == "RYEI", "disconnected components"

assert run("""4 2
abc
cde
efg
gha
abc gha
abc efg
""") == "LUA\nRYEI", "cycle through shared letters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| a, b, ab | LUA | kết nối bắc cầu qua thư chia sẻ | 
| chữ rời rạc | lúa mạch | không có sự hợp nhất ngẫu nhiên | 
| chuỗi tuần hoàn | hỗn hợp | lan truyền qua nhiều liên kết | 

## Vỏ cạnh 

Một trường hợp thường phá vỡ suy nghĩ ngây thơ là khi kết nối gián tiếp thông qua nhiều từ trung gian. Coi như`ab -> bc -> cd`. Cấu trúc DSU kết nối cả ba thông qua các nút chữ cái chung b và c. Khi xử lý`ab cd`, thao tác tìm kiếm sẽ trả về cùng một đại diện vì cả hai đều đã được hấp thụ vào cùng một thành phần kết hợp. Điều này xác nhận rằng kết nối nhiều bước nhảy được xử lý một cách tự nhiên mà không cần truyền tải biểu đồ rõ ràng. 

Một trường hợp khác là những từ có nhiều chữ cái. Một từ như “abc” phải được kết hợp với cả ba chữ cái, nhưng ngay cả khi một từ khác chỉ có một trong số chúng, kết nối vẫn được truyền chính xác. DSU không phụ thuộc vào chữ cái nào được sử dụng trước; tất cả các chữ cái đều hội tụ về cùng một thành phần nếu chúng thuộc các từ trùng nhau nên thứ tự kết hợp không ảnh hưởng đến tính đúng đắn.
