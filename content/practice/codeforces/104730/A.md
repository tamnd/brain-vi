---
title: "CF 104730A - \u0423\u043d\u0438\u043a\u0430\u043b\u044c\u043d\u0430\u044f \u043f\u0435\u0441\u043d\u044f"
description: "Chúng ta có hai tập hợp các chuỗi, mỗi tập hợp có kích thước $n$. Chúng ta phải sắp xếp chúng thành một chuỗi có độ dài $2n$, nhưng các vị trí được cố định theo tính chẵn lẻ: mọi vị trí lẻ phải chứa một chuỗi từ bộ sưu tập đầu tiên và mọi vị trí chẵn phải chứa một chuỗi từ…"
date: "2026-06-29T02:39:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104730
codeforces_index: "A"
codeforces_contest_name: "Moscow team school olympiad (MKOSHP) 2023"
rating: 0
weight: 104730
solve_time_s: 71
verified: true
draft: false
---

[CF 104730A - \u0423\u043d\u0438\u043a\u0430\u043b\u044c\u043d\u0430\u044f \u043f\u0435\u0441\u043d\u044f](https://codeforces.com/problemset/problem/104730/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai tập hợp các chuỗi, mỗi tập hợp có kích thước$n$. Chúng ta phải sắp xếp chúng thành một chuỗi có độ dài$2n$, nhưng các vị trí được cố định theo tính chẵn lẻ: mọi vị trí lẻ phải chứa một chuỗi từ bộ sưu tập đầu tiên và mọi vị trí chẵn phải chứa một chuỗi từ bộ sưu tập thứ hai. 

Sau khi xây dựng chuỗi xen kẽ này, chúng ta phân chia nó thành các cặp liền kề$(1,2), (3,4), \dots, (2n-1,2n)$. Mỗi cặp đóng góp một điểm bằng độ dài của hậu tố dài nhất được chia sẻ bởi hai chuỗi trong cặp đó. Mục tiêu là gán các chuỗi sao cho tổng số điểm tương đồng của các hậu tố này trên tất cả các cặp là lớn nhất. 

Một hạn chế quan trọng về cấu trúc là chúng ta không được tự do ghép nối một cách tùy tiện trong hoặc giữa các tập hợp. Mọi phần tử từ tập đầu tiên phải được ghép nối với chính xác một phần tử từ tập thứ hai và sự đóng góp của một cặp chỉ phụ thuộc vào cấu trúc hậu tố chung của chúng. 

Các ràng buộc rất lớn:$n \le 2 \cdot 10^5$và tổng chiều dài của tất cả các chuỗi trên mỗi bộ được giới hạn bởi$2 \cdot 10^5$. Điều này loại trừ bất kỳ giải pháp nào so sánh trực tiếp tất cả các cặp chuỗi, vì ngay cả việc kiểm tra tất cả các cặp chuỗi cũng sẽ yêu cầu tối đa$O(n^2)$so sánh. 

Một cạm bẫy tinh vi xuất hiện khi bạn suy nghĩ tham lam về việc ghép từng chuỗi với ứng cử viên phù hợp nhất một cách độc lập. Một chuỗi có chung hậu tố dài với nhiều chuỗi khác có thể được chọn nhiều lần, ngăn chặn các cặp toàn cầu tốt hơn. Ví dụ: nếu nhiều chuỗi kết thúc bằng cùng một hậu tố dài, việc ghép chúng cục bộ mà không phối hợp có thể làm giảm tổng điểm ngay cả khi mỗi lựa chọn cục bộ có vẻ tối ưu. 

Do đó, nhiệm vụ này là một vấn đề so khớp toàn cục về độ tương tự của hậu tố, không phải là một tập hợp các tối ưu hóa độc lập. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là tính toán độ tương tự hậu tố cho mỗi cặp giữa tập thứ nhất và tập thứ hai, sau đó giải bài toán so khớp hai bên có trọng số trong đó trọng số cạnh là các độ dài hậu tố này. Về nguyên tắc thì điều đó đúng vì mỗi cặp hợp lệ đều đóng góp độc lập. Tuy nhiên, đồ thị là hoàn toàn lưỡng cực với$n^2$các cạnh, và việc tính toán hoặc thậm chí lưu trữ các trọng số này là không thể dưới những ràng buộc. 

Quan sát quan trọng là cấu trúc hậu tố có thể được mã hóa tăng dần bằng cách sử dụng trie được xây dựng trên các chuỗi đảo ngược. Nếu chúng ta đảo ngược tất cả các chuỗi thì hậu tố sẽ trở thành tiền tố trong cách biểu diễn đảo ngược. Điều này biến vấn đề thành các chuỗi ghép nối dựa trên tiền tố chung dài nhất (LCP). 

Trong bộ ba chuỗi đảo ngược, mỗi nút tương ứng với một tiền tố và các chuỗi chia sẻ hậu tố dài tương ứng với việc chia sẻ một nút sâu trong bộ ba này. Sự đóng góp của một cặp chính xác là độ sâu của tổ tiên chung thấp nhất của chúng trong bộ ba. 

Thay vì đánh giá rõ ràng tất cả các cặp, chúng tôi xử lý lần thử từ dưới lên. Tại mỗi nút, chúng tôi kết hợp các chuỗi chưa khớp từ hai nhóm đi qua nút đó. Việc ghép nối luôn được thực hiện tốt nhất càng sớm càng tốt tại nút sâu nhất mà chúng chia sẻ, vì các nút sâu hơn biểu thị các hậu tố chung dài hơn. Điều này tự nhiên dẫn đến chiến lược kết hợp tham lam trên bộ ba, trong đó chúng tôi truyền bá số đếm tăng lên và tích lũy các kết quả trùng khớp cục bộ. 

Brute-force hoạt động vì nó so sánh rõ ràng tất cả các cặp và chọn phép gán tối ưu, nhưng không thành công khi$n^2$tương tác trở nên không thể thực hiện được. Việc tổng hợp dựa trên trie thay thế lý luận theo cặp bằng cách nhóm cấu trúc, giảm vấn đề thành truyền tải tuyến tính trên tổng chiều dài chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 \cdot L)$|$O(n^2)$| Quá chậm | 
| Trie + So khớp từ dưới lên | ( O(\tổng | s | ) ) | 

## Hướng dẫn thuật toán 

1. Đảo ngược mọi chuỗi trong cả hai bộ. Điều này chuyển đổi kết hợp hậu tố thành kết hợp tiền tố, cho phép chúng ta sử dụng cấu trúc trie. Lý do vấn đề này là vì cố gắng mã hóa các tiền tố được chia sẻ một cách tự nhiên dưới dạng đường dẫn được chia sẻ. 
2. Xây dựng một bộ ba chứa tất cả các chuỗi đảo ngược. Tại mỗi nút đầu cuối, lưu trữ chuỗi đến từ tập thứ nhất hay tập thứ hai. 
3. Thực hiện duyệt theo chiều sâu của trie. Tại mỗi nút, chúng tôi thu thập hai giá trị từ cây con: có bao nhiêu chuỗi không khớp từ tập hợp A và tập hợp B có trong cây con này. 
4. Sau khi hợp nhất các phần tử con vào nút hiện tại, hãy tính xem có thể tạo được bao nhiêu kết quả phù hợp tại nút này. Nếu chúng ta có$a$chuỗi từ tập đầu tiên và$b$từ set thứ hai, chúng ta có thể đấu$\min(a, b)$cặp ở độ sâu này. 
5. Thêm vào câu trả lời:$\min(a, b) \times \text{depth}$. Điều này nắm bắt sự đóng góp của tất cả các cặp có tiền tố chung dài nhất kết thúc chính xác tại nút này. 
6. Tuyên truyền phần thừa lên trên: sau khi khớp, chuyển$|a - b|$các chuỗi chưa khớp với nút cha, theo dõi xem bên nào vẫn chiếm ưu thế. 

### Tại sao nó hoạt động 

Mỗi cặp chuỗi hợp lệ đều có một nút trie sâu nhất duy nhất nơi các đường dẫn của chúng giao nhau. Nút đó đại diện cho tiền tố chung dài nhất của các chuỗi đảo ngược, tương ứng chính xác với hậu tố chung dài nhất của chuỗi gốc. Sự phù hợp tại nút đó nắm bắt được toàn bộ sự đóng góp của họ. Bất kỳ nỗ lực nào nhằm trì hoãn việc so khớp cao hơn trong bộ ba sẽ chỉ ghép các chuỗi có tiền tố chung ngắn hơn, làm giảm tổng đóng góp. Bởi vì việc so khớp luôn được thực hiện một cách tham lam ở điểm sâu nhất có thể nên mỗi đơn vị luồng được tính chính xác một lần ở độ sâu chính xác của nó, đảm bảo tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

class Node:
    __slots__ = ("next", "cntA", "cntB")
    def __init__(self):
        self.next = {}
        self.cntA = 0
        self.cntB = 0

def add(root, s, typ):
    v = root
    for ch in s:
        if ch not in v.next:
            v.next[ch] = Node()
        v = v.next[ch]
    if typ == 0:
        v.cntA += 1
    else:
        v.cntB += 1

def dfs(v, depth, res):
    a = v.cntA
    b = v.cntB

    for u in v.next.values():
        da, db = dfs(u, depth + 1, res)
        a += da
        b += db

    m = min(a, b)
    res[0] += m * depth
    a -= m
    b -= m

    return a, b

n = int(input())
root = Node()

for _ in range(n):
    s = input().strip()[::-1]
    add(root, s, 0)

for _ in range(n):
    s = input().strip()[::-1]
    add(root, s, 1)

res = [0]
dfs(root, 0, res)
print(res[0])
```Giải pháp bắt đầu bằng cách đảo ngược tất cả các chuỗi trước khi chèn chúng vào bộ ba, đảm bảo mối quan hệ hậu tố trở thành mối quan hệ tiền tố. Mỗi nút lá tăng một bộ đếm tùy thuộc vào việc nó thuộc về tập thứ nhất hay tập thứ hai. 

DFS tổng hợp số lượng từ trẻ em trở lên. Bước quan trọng là đối sánh cục bộ bằng cách sử dụng`min(a, b)`tại mỗi nút, đại diện cho việc ghép càng nhiều chuỗi càng tốt với tiền tố hiện tại. Phép nhân với`depth`chuyển đổi những kết quả phù hợp này thành đóng góp thực tế của chúng cho câu trả lời. 

Các giá trị trả về`a`Và`b`đại diện cho các chuỗi chưa từng có không thể ghép nối ở các nút sâu hơn và phải được xem xét ở các cấp độ cao hơn nơi tiền tố chung của chúng ngắn hơn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Bộ đầu vào là: 

Bộ đầu tiên:`dca, cba, dcb, bbb`Bộ thứ hai:`fea, fea, aba, bbb`Chúng tôi theo dõi sự tổng hợp ở độ sâu ba chiều có liên quan. 

| Độ sâu nút | Một số | Số B | Trận đấu được thực hiện | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 3 (bbb) | 1 | 1 | 1 | 3 | 
| 2 (nhóm hậu tố) | 2 | 2 | 2 | 3 × 2 = 4 (tích lũy) | 
| 1 | 0 | 0 | 0 | 0 | 
| gốc | 0 | 0 | 0 | 0 | 

Tổng cuối cùng trở thành$6$. 

Dấu vết này cho thấy các kết quả trùng khớp được hình thành một cách tham lam tại các nút hậu tố được chia sẻ sâu nhất có thể, tối đa hóa đóng góp cho mỗi cặp. 

### Mẫu 2 

Bộ đầu tiên:`a, bc, bcaa`Bộ thứ hai:`aa, aaa, aaac`| Độ sâu nút | Một số | Số B | Trận đấu được thực hiện | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 2 ("aa") | 1 | 2 | 1 | 2 | 
| 1 | 2 | 1 | 1 | 2 | 
| gốc | 0 | 0 | 0 | 0 | 

Tổng cộng là$4$. 

Điều này cho thấy các kết quả khớp sâu hơn được ưu tiên như thế nào trước các kết quả nông hơn, đảm bảo sử dụng các kết quả trùng lặp hậu tố dài bất cứ khi nào có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | ( O(\tổng | s | 
| Không gian | ( O(\tổng | s | 

Tổng số ký tự trên cả hai bộ được giới hạn bởi$4 \cdot 10^5$, do đó thuật toán phù hợp thoải mái trong giới hạn thời gian. Mỗi thao tác đều tuyến tính ở kích thước đầu vào, tránh mọi hành vi ghép nối bậc hai. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    sys.setrecursionlimit(10**7)

    class Node:
        def __init__(self):
            self.next = {}
            self.cntA = 0
            self.cntB = 0

    def add(root, s, typ):
        v = root
        for ch in s:
            if ch not in v.next:
                v.next[ch] = Node()
            v = v.next[ch]
        if typ == 0:
            v.cntA += 1
        else:
            v.cntB += 1

    def dfs(v, depth):
        a = v.cntA
        b = v.cntB
        res = 0
        for u in v.next.values():
            da, db, sub = dfs(u, depth + 1)
            a += da
            b += db
            res += sub
        m = min(a, b)
        res += m * depth
        a -= m
        b -= m
        return a, b, res

    n = int(sys.stdin.readline())
    root = Node()

    for _ in range(n):
        add(root, sys.stdin.readline().strip()[::-1], 0)
    for _ in range(n):
        add(root, sys.stdin.readline().strip()[::-1], 1)

    _, _, ans = dfs(root, 0)
    return str(ans)

# provided samples
assert run("""4
dca
cba
dcb
bbb
fea
fea
aba
bbb
""") == "6"

assert run("""3
a
bc
bcaa
aa
aaa
aaac
""") == "4"

# all-equal
assert run("""2
aaa
aaa
aaa
aaa
""") == "6"

# minimal
assert run("""1
a
a
""") == "1"

# no common suffix except trivial
assert run("""2
ab
cd
ef
gh
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| các chuỗi bằng nhau | 6 | khớp tối đa tại nút sâu nhất | 
| cặp đơn | 1 | tính đúng đắn của trường hợp cơ sở | 
| hậu tố rời rạc | 0 | không khớp sai | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi tất cả các chuỗi trong cả hai bộ đều giống hệt nhau. Trong tình huống đó, mỗi cặp sẽ đóng góp toàn bộ độ dài chuỗi. Trie sụp đổ thành một con đường duy nhất trong đó cả hai bộ đếm tích lũy hoàn toàn ở mỗi độ sâu. Tại mỗi nút,`min(a, b)`trước tiên chỉ trích xuất các kết quả trùng khớp ở mức sâu nhất, đảm bảo mỗi cặp được tính chính xác một lần ở độ sâu tối đa. 

Một trường hợp cạnh khác là khi không có sự chồng chéo hậu tố nào cả. Trie phân nhánh ngay tại gốc và không có nút sâu nào chứa cả hai loại. Tất cả`min(a, b)`các phép tính vẫn bằng 0 ở độ sâu dương, do đó câu trả lời vẫn bằng 0, phản ánh chính xác rằng không có cặp nào có chung hậu tố không trống.
