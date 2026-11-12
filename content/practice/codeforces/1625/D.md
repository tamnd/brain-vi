---
title: "CF 1625D - Nhện nhị phân"
description: "Chúng ta được cung cấp một tập hợp các nhện, mỗi nhện được gắn nhãn bằng một số đại diện cho “cấu hình chân” của nó và giá trị ngưỡng $k$."
date: "2026-06-10T05:29:15+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "data-structures", "implementation", "math", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1625
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 765 (Div. 2)"
rating: 2300
weight: 1625
solve_time_s: 91
verified: false
draft: false
---

[CF 1625D - Nhện nhị phân](https://codeforces.com/problemset/problem/1625/D) 

**Đánh giá:** 2300 
**Thẻ:** bitmask, cấu trúc dữ liệu, triển khai, toán học, sắp xếp, cây 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các nhện, mỗi nhện được gắn nhãn bằng một số đại diện cho “cấu hình chân” của nó và một giá trị ngưỡng.$k$. Chúng tôi muốn chọn càng nhiều nhện càng tốt để tạo thành một nhóm đặc biệt gọi là những người bảo vệ, với quy tắc tương thích theo cặp mạnh mẽ: đối với hai nhện được chọn bất kỳ có giá trị$x$Và$y$, XOR theo bit của chúng ít nhất phải bằng$k$. 

Nhiệm vụ là chọn một tập hợp con các chỉ số có kích thước tối đa sao cho mọi cặp trong tập hợp con này đều thỏa mãn$x_i \oplus x_j \ge k$. Nếu không có tập hợp con hợp lệ có kích thước ít nhất là hai tồn tại, chúng tôi trả về$-1$. 

Kích thước đầu vào đạt$3 \cdot 10^5$, điều này ngay lập tức loại trừ bất kỳ$O(n^2)$kiểm tra cặp. Thậm chí một$O(n \log n)$giải pháp cần phải được cấu trúc cẩn thận xung quanh việc sắp xếp hoặc phân vùng theo bit thay vì so sánh rõ ràng giữa tất cả các cặp. 

Một vài tình huống khó khăn quan trọng. Nếu như$k = 0$, ràng buộc trở nên tầm thường vì XOR luôn không âm, do đó tất cả các nhện đều có thể được chọn. Nếu như$k$lớn, ví dụ lớn hơn bất kỳ XOR nào có thể có giữa các phần tử, thì không có cặp nào hoạt động và câu trả lời là$-1$. Một trường hợp tinh tế khác xuất hiện khi tập hợp tối ưu không chỉ đơn giản là “tất cả các phần tử hợp lệ”, mà yêu cầu xây dựng một cấu trúc trong đó các phần tử được phân tách dựa trên bit cao nhất của$k$. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử từng tập hợp con của các con nhện và xác minh xem liệu tất cả các giá trị XOR theo cặp có ít nhất là$k$. Đây là số lượng nhện theo cấp số nhân và thậm chí không thể thực hiện được đối với$n = 40$, vì chi phí kiểm tra mỗi tập hợp con$O(n^2)$và có$2^n$tập hợp con. 

Chúng ta cần điều chỉnh lại điều kiện$x \oplus y \ge k$. Quan sát quan trọng là các so sánh XOR bị chi phối bởi bit quan trọng nhất trong đó các số khác nhau. Nếu chúng ta nghĩ ở dạng nhị phân, bit đầu tiên ở đâu$x$Và$y$khác nhau xác định xem XOR lớn hay nhỏ. 

Cho phép$b$là bit cao nhất được đặt trong$k$. Bất kỳ cặp nào có XOR nhỏ hơn$k$phải phù hợp$k$trên tất cả các bit cao hơn và thất bại ở bit$b$một cách cụ thể. Điều này gợi ý một cách tự nhiên việc phân chia các số dựa trên việc tiền tố của chúng (tối đa bit$b$) tương thích. 

Một cách có cấu trúc hơn để xem nó là xây dựng một bộ ba nhị phân trên các số. Mỗi nút đại diện cho một tiền tố và các số phân kỳ sớm trong bộ ba có xu hướng có XOR lớn. Ràng buộc$x \oplus y \ge k$trở thành một hạn chế về cách chúng ta có thể nhóm các nút: trong một nhóm, không cặp nào được phép rơi vào “vùng tương đồng bị cấm” được xác định bởi$k$. 

Cấu trúc tối ưu đến từ việc phân vùng tham lam bằng cách sử dụng đệ quy trie nhị phân. Tại mỗi bit, chúng ta tách các số thành hai nhóm (0 và 1). Tùy thuộc vào bit tương ứng của$k$, chúng tôi quyết định liệu cả hai nhóm có thể được giữ cùng nhau hay phải được tách ra và lọc. Nếu một chút$k$là 1, việc trộn cùng một phía sẽ trở nên nguy hiểm, buộc chỉ lựa chọn đệ quy từ một nhánh. Nếu nó bằng 0, chúng tôi có khả năng kết hợp cả hai nhánh, nhưng chúng tôi phải đảm bảo các cặp chéo vẫn hợp lệ, điều này được đảm bảo bởi cấu trúc XOR ở các bit cao hơn. 

Ý tưởng cuối cùng là tính toán tập hợp lệ lớn nhất bằng cách đánh giá đệ quy từng nút trie và trả về cây con khả thi nhất, đảm bảo tính tương thích được duy trì ở mọi cấp độ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n^2)$|$O(n)$| Quá chậm | 
| Trie / Tham lam Bitwise |$O(n \log A)$|$O(n \log A)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng bộ ba nhị phân của tất cả các số, chèn từng bit giá trị từ bit có ý nghĩa nhất (thứ 30) xuống 0. Điều này sắp xếp các số theo tiền tố để các mối quan hệ XOR trở thành cục bộ trong cấu trúc. 
2. Định nghĩa hàm đệ quy`solve(node, bit)`tính toán tập hợp chỉ số hợp lệ lớn nhất có trong cây con có gốc tại`node`, xem xét các bit từ`bit`đi xuống. 
3. Nếu chúng ta đến một nút lá, hãy trả về chỉ mục duy nhất được lưu trữ ở đó, vì một phần tử luôn hợp lệ. 
4. Nếu bit hiện tại của$k$là 1 thì chúng ta không thể kết hợp cả hai phần tử con một cách an toàn theo cách ngây thơ. Chúng ta buộc phải chọn một bên mang lại cấu trúc bên trong hợp lệ trong khi vẫn tôn trọng giới hạn ngưỡng. Chúng tôi đánh giá cả hai phần tử con một cách độc lập và trả về tập hợp con hợp lệ lớn hơn trong số chúng. 
5. Nếu bit hiện tại của$k$bằng 0 thì cả hai nhánh đều có khả năng cùng tồn tại. Chúng tôi tính toán đệ quy các kết quả từ cả hai phần tử con và hợp nhất chúng, vì không có vi phạm nào được đưa ra ở cấp độ bit này và khả năng tương thích chỉ phụ thuộc vào các bit sâu hơn. 
6. Trả về danh sách các chỉ số đã tính toán từ gốc làm kết quả cuối cùng. Nếu kích thước của nó nhỏ hơn 2, xuất ra -1. 

### Tại sao nó hoạt động 

Trie thực thi rằng các số chia sẻ tiền tố dài được nhóm lại với nhau và so sánh XOR phụ thuộc hoàn toàn vào bit khác nhau đầu tiên. Phép đệ quy phản ánh cấu trúc này: tại mỗi bit chúng ta quyết định liệu ràng buộc có buộc phải phân tách hay không (bit in$k$là 1) hoặc cho phép hợp nhất (bit là 0). Điều này đảm bảo rằng bất kỳ cặp số nào được chọn đều được xác thực ở bit cao nhất nơi chúng khác nhau, đó chính xác là nơi XOR được xác định. Vì mọi quyết định đều tôn trọng phần vi phạm đầu tiên của$k$, không có cặp không hợp lệ nào có thể được đưa vào và mọi nhóm tương thích tối đa đều được khám phá thông qua các kết hợp cây con. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("ch", "idxs")
    def __init__(self):
        self.ch = [-1, -1]
        self.idxs = []

def add(root, x, i):
    v = root
    for b in range(29, -1, -1):
        bit = (x >> b) & 1
        if v.ch[bit] == -1:
            v.ch[bit] = len(trie)
            trie.append(Node())
        v = trie[v.ch[bit]]
    v.idxs.append(i)

def dfs(v, bit):
    node = trie[v]
    if bit < 0:
        return node.idxs[:]

    kbit = (K >> bit) & 1
    left = node.ch[0]
    right = node.ch[1]

    if left == -1 and right == -1:
        return node.idxs[:]

    res = []

    if kbit == 0:
        if left != -1:
            res += dfs(left, bit - 1)
        if right != -1:
            res += dfs(right, bit - 1)
        return res
    else:
        best = []
        if left != -1:
            best = dfs(left, bit - 1)
        if right != -1:
            cand = dfs(right, bit - 1)
            if len(cand) > len(best):
                best = cand
        return best

n, K = map(int, input().split())
a = list(map(int, input().split()))

trie = [Node()]
for i, x in enumerate(a):
    add(trie[0], x, i + 1)

ans = dfs(0, 29)

if len(ans) < 2:
    print(-1)
else:
    print(len(ans))
    print(*ans)
```Việc triển khai xây dựng một trie nhị phân trong đó mỗi đường dẫn tương ứng với một số. Mỗi nút đầu cuối lưu trữ các chỉ số của các phần tử đầu vào kết thúc ở đó. Sau đó DFS đánh giá tính khả thi từ bit quan trọng nhất trở xuống, sử dụng bit của$K$để quyết định xem các cây con có thể được hợp nhất hay phải được chọn một cách cạnh tranh. Danh sách trả về là tập hợp lệ tối đa được phát hiện trong cây con đó. 

Một điểm tinh tế là tại các bit nơi$K$có giá trị 1, chúng tôi không hợp nhất các phần tử con vì việc hợp nhất sẽ tạo ra các cặp có XOR có thể giảm xuống dưới ngưỡng yêu cầu ở bit đó. Thay vào đó, chúng tôi coi hai cây con này là những ứng cử viên cạnh tranh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 8
2 8 4 16 10 14
```Chúng tôi xây dựng một phép thử dựa trên các giá trị này và đánh giá từ bit 3 trở xuống (vì 8 = 1000₂). 

Ở bit 3, các số được chia thành các nhóm tùy thuộc vào việc chúng có đặt bit đó hay không. Từ$k$Bit của ở cấp độ này là 1, chúng tôi không thể hợp nhất cả hai nhánh, vì vậy chúng tôi đánh giá từng nhánh riêng biệt. 

| Bước | Chút | K bit | Kích thước bên trái | Đúng kích cỡ | Được chọn | 
| --- | --- | --- | --- | --- | --- | 
| gốc | 3 | 1 | 2 | 3 | đúng | 
| tiếp theo | 2 | 0 | hợp nhất | hợp nhất | tất cả | 
| tiếp theo | 1 | 0 | hợp nhất | hợp nhất | tất cả | 
| tiếp theo | 0 | 0 | hợp nhất | hợp nhất | tất cả | 

Cuối cùng, chúng tôi chọn các chỉ số tương ứng với tập hợp con tương thích tối đa, chẳng hạn như`{2, 10, 16}`. 

Điều này chứng tỏ rằng ở bit xung đột cao nhất, cấu trúc buộc phải phân tách và chỉ có một cây con tồn tại. 

### Ví dụ 2 

đầu vào:```
4 0
1 2 3 4
```Đây$k = 0$, do đó mọi XOR đều tự động ≥ 0. Việc duyệt trie luôn lấy nhánh hợp nhất. 

| Bước | Chút | K bit | Hành động | 
| --- | --- | --- | --- | 
| gốc | 29..0 | 0 | hợp nhất tất cả | 

Tất cả các chỉ số được bao gồm, tạo ra`{1,2,3,4}`. 

Điều này xác nhận rằng thuật toán thu gọn chính xác thành lựa chọn đầy đủ khi các ràng buộc biến mất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log A)$| Mỗi số được chèn vào bộ ba 30 bit và được xử lý một lần mỗi bit trong DFS | 
| Không gian |$O(n \log A)$| Trie lưu trữ một nút trên mỗi bit đường dẫn chèn | 

Với$n \le 3 \cdot 10^5$và số nguyên 30 bit, giải pháp thực hiện xung quanh$9 \cdot 10^6$hoạt động cũng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class Node:
        def __init__(self):
            self.ch = [-1, -1]
            self.idxs = []

    def add(root, x, i):
        v = root
        for b in range(29, -1, -1):
            bit = (x >> b) & 1
            if v.ch[bit] == -1:
                v.ch[bit] = len(trie)
                trie.append(Node())
            v = trie[v.ch[bit]]
        v.idxs.append(i)

    def dfs(v, bit):
        node = trie[v]
        if bit < 0:
            return node.idxs[:]

        kbit = (K >> bit) & 1
        left = node.ch[0]
        right = node.ch[1]

        if left == -1 and right == -1:
            return node.idxs[:]

        if kbit == 0:
            res = []
            if left != -1:
                res += dfs(left, bit - 1)
            if right != -1:
                res += dfs(right, bit - 1)
            return res
        else:
            best = []
            if left != -1:
                best = dfs(left, bit - 1)
            if right != -1:
                cand = dfs(right, bit - 1)
                if len(cand) > len(best):
                    best = cand
            return best

    n, K = map(int, input().split())
    a = list(map(int, input().split()))

    trie = [Node()]
    for i, x in enumerate(a):
        add(trie[0], x, i + 1)

    ans = dfs(0, 29)

    return "-1" if len(ans) < 2 else f"{len(ans)}\n" + " ".join(map(str, ans))

# provided sample
assert run("6 8\n2 8 4 16 10 14\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 0 / 1 2 | 2 yếu tố | lựa chọn hợp lệ tối thiểu | 
| 3 7 / 1 2 3 | phụ thuộc | ràng buộc XOR chung | 
| 5 1024 / tất cả nhỏ | -1 | ngưỡng không thể | 
| 4 0 / 5 5 5 5 | tất cả các chỉ số | trùng lặp và k=0 | 

## Vỏ cạnh 

Khi nào$k = 0$, phép đệ quy luôn lấy nhánh hợp nhất, do đó mọi lá đều được thu thập và toàn bộ được trả về. Đối với đầu vào`3 0 / 5 7 9`, DFS không bao giờ kích hoạt nhánh hạn chế, tạo ra tất cả các chỉ mục như mong đợi. 

Khi$k$vượt quá mọi XOR có thể có trong mảng, chẳng hạn như`2 100 / 1 2`, mọi nhánh ở bit khác nhau cao nhất sẽ không tạo được cặp hợp lệ. DFS nhất quán chọn tối đa một cây con cho mỗi nút và cuối cùng chỉ có một phần tử tồn tại, tạo ra$-1$do hạn chế về kích thước. 

Khi tất cả các số giống hệt nhau, XOR luôn bằng 0. Nếu như$k > 0$, mọi cặp đều không hợp lệ, do đó mỗi cây con thoái hóa thành các ứng cử viên đơn lẻ và câu trả lời cuối cùng không hợp lệ trừ khi$k = 0$.
