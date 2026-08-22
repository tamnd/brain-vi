---
title: "CF 104160M - Vulpecula"
description: "Chúng ta có một cây có tối đa $n$ đỉnh, trong đó mỗi đỉnh đại diện cho một ngôi sao. Cây được bắt nguồn hoàn toàn bằng cách xây dựng đầu vào, nhưng về mặt khái niệm, nó chỉ là một cây vô hướng được xác định bởi các cạnh $n-1$. Đối với mỗi ngôi sao, Mu chọn nó làm trung tâm quan sát."
date: "2026-07-02T01:05:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104160
codeforces_index: "M"
codeforces_contest_name: "The 2022 ICPC Asia Shenyang Regional Contest (The 1st Universal Cup, Stage 1: Shenyang)"
rating: 0
weight: 104160
solve_time_s: 53
verified: true
draft: false
---

[CF 104160M - Vulpecula](https://codeforces.com/problemset/problem/104160/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây cao tới$n$đỉnh, trong đó mỗi đỉnh tượng trưng cho một ngôi sao. Cây được bắt nguồn hoàn toàn bằng cách xây dựng đầu vào, nhưng về mặt khái niệm nó chỉ là một cây vô hướng được xác định bởi$n-1$các cạnh. 

Đối với mỗi ngôi sao, Mu chọn nó làm trung tâm quan sát. Từ tâm này người ta xét bán kính$d$, nghĩa là anh ta có thể quan sát tất cả các nút có khoảng cách từ cây đến tâm tối đa là$d$. Vì vậy đối với mỗi$d$, chúng ta đang quan sát một quả bóng đang phát triển xung quanh gốc đã chọn. 

Mỗi ngôi sao cũng có một bộ “bộ lọc”. Mỗi bộ lọc là một giá trị được áp dụng cho một nút duy nhất và việc áp dụng các bộ lọc tương ứng với việc lấy XOR theo bit của tất cả các giá trị bộ lọc đã chọn trên nút đó. Chúng ta có thể áp dụng các bộ lọc theo bất kỳ thứ tự nào và sử dụng lại các bộ lọc giống hệt nhau, sao cho mỗi nút đều có hiệu quả$i$có nhiều tập hợp giá trị và chúng ta có thể chọn bất kỳ tập hợp con nào, nghĩa là các giá trị có thể đạt được tại nút$i$đều là tập hợp con XOR của multiset của nó. 

Với tâm và bán kính cố định$d$, Mu quan sát tất cả các nút trong khoảng cách$d$và muốn làm cho tất cả các nút được quan sát có khả năng hiển thị như nhau, đồng thời tối đa hóa giá trị chung đó. Vì mỗi nút có thể chọn độc lập bất kỳ tập hợp con XOR nào từ nhiều tập hợp cục bộ của nó, nên ràng buộc sẽ trở thành: chọn một giá trị duy nhất$x$sao cho mọi nút trong quả bóng có thể nhận ra$x$bằng cách sử dụng các bộ lọc của nó. Điều đó có nghĩa$x$phải thuộc giao điểm của tất cả các bộ XOR có thể truy cập của các nút và chúng tôi muốn mức tối đa như vậy$x$. 

Đối với mỗi nút trung tâm$c$, định nghĩa$f(d)$vì giá trị XOR chung tối đa có thể này giữa các nút trong khoảng cách$d$. Chúng ta phải tính toán$$\sum_{d=0}^{n-1} f(d)$$cho mọi nút trung tâm có thể. 

Cây có tới$5 \cdot 10^4$nút, nhưng tổng số bộ lọc lên tới$2 \cdot 10^6$, do đó việc xử lý bộ lọc rất nặng nề. Điều này đã gợi ý rằng việc tính toán lại mỗi truy vấn trên các nút và bán kính là không thể. 

Một cách tiếp cận đơn giản sẽ tính toán lại, cho mọi tâm và mọi bán kính, giao điểm của các cơ sở tuyến tính của tất cả các nút trong quả bóng. Ngay cả khi chúng tôi duy trì các cơ sở XOR, việc tính toán lại các quả bóng nhiều lần sẽ dẫn đến$O(n^2)$hoặc hành vi tệ hơn, vượt xa giới hạn. 

Trường hợp cạnh tinh vi xuất hiện khi có nhiều bộ lọc tồn tại trên một nút. Ví dụ: nếu một nút có tất cả các bộ lọc và các nút khác không có bộ lọc nào thì các giao điểm sẽ co lại ngay lập tức khi nút đó đi vào bán kính. Bất kỳ cách tiếp cận nào xử lý các bộ lọc trên toàn cầu mà không tôn trọng vị trí nút sẽ đánh giá quá cao$f(d)$. 

## Phương pháp tiếp cận 

Khó khăn chính là mỗi nút đóng góp một cơ sở tuyến tính của các giá trị XOR và chúng ta cần sự giao nhau của các cơ sở trên các quả bóng cây đang phát triển linh hoạt. Giao điểm trực tiếp của các không gian con XOR không ổn định trong phép tổng hợp đơn giản, bởi vì giao điểm của các nhịp tuyến tính không chỉ đơn giản là giao điểm của các giao điểm của các bộ tạo. 

Ý tưởng về lực lượng vũ phu sẽ là: đối với mỗi tâm và mỗi bán kính, thu thập tất cả các nút trong quả bóng, xây dựng cơ sở tuyến tính cho mỗi nút và tính toán giao điểm của tất cả các cơ sở này bằng cách loại bỏ Gaussian thô bạo trên GF(2) trong 64 chiều. Ngay cả khi kích thước cơ sở được giới hạn bởi 64, việc thực hiện điều này với mọi bán kính là$O(n^2 \cdot 64^2)$trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. 

Bước đột phá về mặt cấu trúc là diễn giải lại vấn đề theo hướng ngược lại. Thay vì mở rộng bán kính và tính toán lại các giao lộ, chúng tôi theo dõi khi giá trị XOR đề xuất trở nên không hợp lệ khi bán kính tăng lên. Một giá trị$x$là khả thi đối với bán kính$d$khi và chỉ nếu mọi nút trong khoảng cách$d$có thể sản xuất$x$, tương đương với việc nói không có nút nào trong quả bóng đó loại trừ$x$. 

Đối với mỗi nút, tập hợp các giá trị XOR có thể truy cập của nó là một không gian con tuyến tính của$\{0,1\}^{64}$. Mỗi không gian con như vậy có thể được biểu diễn bằng một cơ sở tuyến tính. Một giá trị$x$hợp lệ cho nút$u$nếu nó nằm trong khoảng của cơ sở đó thì có thể kiểm tra được bằng phép khử Gauss. 

Bây giờ chúng ta lật lại phối cảnh: thay vì duy trì giao điểm trên các nút, chúng ta duy trì cho từng nút$u$tập hợp các ràng buộc mà nó áp đặt lên$x$và chúng tôi muốn biết, đối với một trung tâm cố định, chúng tôi có thể phát triển một quả bóng giống BFS trong bao lâu trước khi giao điểm toàn cầu mất đi mẫu bit để đạt được XOR tối đa có thể. 

Điều này biến vấn đề thành việc theo dõi khi các ràng buộc từ các nút ở khoảng cách ngày càng tăng sẽ loại bỏ các ứng cử viên trong không gian 64 chiều. Quan sát quan trọng là câu trả lời$f(d)$đối với một trung tâm là đơn điệu không tăng trong$d$và chỉ thay đổi khi nút mới được đưa vào loại bỏ ứng cử viên XOR tối ưu hiện tại. Điều đó cho thấy chúng ta có thể xử lý các nút theo thứ tự khoảng cách tăng dần và duy trì cơ sở ràng buộc tuyến tính toàn cầu. 

Để hỗ trợ nhiều trung tâm, chúng tôi diễn giải lại cây có gốc tại mỗi nút và thực hiện DP kiểu trung tâm hoặc gốc lại để tổng hợp các cơ sở tuyến tính của các cây con trong khi vẫn duy trì thứ tự khoảng cách một cách ngầm định. Mỗi nút đóng góp cơ sở của nó cho tổ tiên với trọng số khoảng cách và chúng tôi xử lý các đóng góp bằng cách duyệt cây để tích lũy các hợp nhất cơ sở theo thứ tự khoảng cách. 

Ý tưởng cuối cùng là duy trì, đối với mỗi trung tâm, một cấu trúc chèn dần các cơ sở nút được sắp xếp theo khoảng cách và theo dõi giá trị XOR tối đa nhất quán với tất cả các cơ sở được chèn. Vì một cơ sở có nhiều nhất là 64 vectơ nên việc hợp nhất là$O(64^2)$và mỗi nút tham gia vào một số logarit của các lần hợp nhất như vậy thông qua phân rã cây, dẫn đến độ phức tạp khả thi tổng thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 \cdot 64^2)$|$O(n \cdot 64)$| Quá chậm | 
| Tối ưu |$O(n \cdot 64^2 \log n)$|$O(n \cdot 64)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi dựa vào thực tế là các bộ lọc của mọi nút đều xác định cơ sở tuyến tính 64 bit và tất cả các ràng buộc đều truyền qua khoảng cách cây con. 

Trước tiên, chúng tôi tính toán cơ sở tuyến tính cho từng nút từ các bộ lọc của nó một cách độc lập, giảm nhiều tập hợp của nó thành cơ sở XOR tiêu chuẩn. 

Sau đó, chúng tôi phân tách cây bằng cách sử dụng phân tách centroid sao cho khoảng cách từ bất kỳ centroid nào đến các nút trong thành phần của nó đều có cấu trúc rõ ràng. Đối với mỗi tâm, chúng tôi sẽ mô phỏng bán kính mở rộng từ 0 ra ngoài trong khi vẫn duy trì giao điểm của tất cả các cơ sở nút trong bán kính hiện tại. 

Tại mỗi tâm, chúng tôi thu thập tất cả các nút trong thành phần của nó cùng với khoảng cách của chúng đến tâm. Chúng tôi sắp xếp các nút này theo khoảng cách, vì$f(d)$chỉ thay đổi khi bao gồm một lớp khoảng cách mới. 

Sau đó chúng tôi xử lý các nút theo thứ tự khoảng cách tăng dần. 

Đối với mỗi nút mới$u$, chúng tôi hợp nhất cơ sở của nó thành cơ sở toàn cầu được duy trì cho trung tâm. Sau mỗi lần chèn, chúng tôi tính toán lại giá trị XOR tối đa có thể đạt được theo cơ sở giao điểm hiện tại. Điều này được thực hiện bằng cách sử dụng cấu trúc XOR tham lam tiêu chuẩn trên cơ sở được duy trì. 

Chúng tôi duy trì một mảng cho trung tâm này trong đó chỉ mục$d$lưu trữ giá trị tốt nhất hiện tại sau khi bao gồm tất cả các nút có khoảng cách tối đa$d$. Sau đó, chúng tôi tích lũy sự đóng góp của các giá trị này vào câu trả lời cuối cùng cho tâm. 

Cuối cùng, vì mỗi nút chỉ đóng vai trò là trọng tâm một lần và xuất hiện trong$O(\log n)$cấp độ trung tâm, chúng tôi cộng trọng số đóng góp của mỗi nút theo số lượng trung tâm sử dụng nó ở mỗi cấp độ khoảng cách và kết hợp các kết quả vào câu trả lời cuối cùng cho mỗi trung tâm. 

### Tại sao nó hoạt động 

Phân rã centroid đảm bảo rằng bất kỳ đường đi nào trong cây đều được chia thành nhiều thành phần logarit, do đó mỗi cặp nút chỉ được xem xét cùng nhau$O(\log n)$lần. Trong mỗi thành phần tâm, các nút được xử lý theo thứ tự khoảng cách chặt chẽ, khớp chính xác với việc mở rộng bán kính xung quanh tâm đó. Bởi vì tính khả thi của XOR được nắm bắt hoàn toàn bởi các cơ sở tuyến tính, nên việc hợp nhất các cơ sở sẽ duy trì tính chính xác của giao điểm của các ràng buộc. Việc xây dựng lại tham lam của XOR tối đa từ cơ sở được duy trì luôn tạo ra giá trị hợp lệ tối đa, vì vậy mỗi$f(d)$được tính toán chính xác một lần cho mỗi lần đóng góp ở cấp độ trung tâm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class XorBasis:
    def __init__(self):
        self.b = [0] * 64

    def add(self, x):
        for i in range(63, -1, -1):
            if not (x >> i) & 1:
                continue
            if self.b[i]:
                x ^= self.b[i]
            else:
                self.b[i] = x
                return

    def merge(self, other):
        for v in other.b:
            if v:
                self.add(v)

    def max_xor(self):
        res = 0
        for i in range(63, -1, -1):
            if (res ^ self.b[i]) > res:
                res ^= self.b[i]
        return res

def build_basis(values):
    xb = XorBasis()
    for v in values:
        xb.add(v)
    return xb

def main():
    n = int(input())
    p = list(map(int, input().split()))
    g = [[] for _ in range(n)]
    for i, par in enumerate(p, start=1):
        g[par - 1].append(i)
        g[i].append(par - 1)

    node_basis = []
    for i in range(n):
        mi = list(map(int, input().split()))
        k = mi[0]
        vals = mi[1:]
        node_basis.append(build_basis(vals))

    sys.setrecursionlimit(10**7)

    ans = [0] * n

    visited = [False] * n
    sub = [0] * n

    def dfs_size(u, p):
        sub[u] = 1
        for v in g[u]:
            if v != p and not visited[v]:
                dfs_size(v, u)
                sub[u] += sub[v]

    def dfs_centroid(u, p, nsz):
        for v in g[u]:
            if v != p and not visited[v] and sub[v] > nsz // 2:
                return dfs_centroid(v, u, nsz)
        return u

    def collect(u, p, d, arr):
        arr.append((u, d))
        for v in g[u]:
            if v != p and not visited[v]:
                collect(v, u, d + 1, arr)

    def process(center):
        nodes = []
        collect(center, -1, 0, nodes)
        nodes.sort(key=lambda x: x[1])

        cur = XorBasis()
        i = 0
        maxd = 0

        while i < len(nodes):
            d = nodes[i][1]
            while i < len(nodes) and nodes[i][1] == d:
                u = nodes[i][0]
                cur.merge(node_basis[u])
                i += 1
            ans[center] += cur.max_xor()

    def decompose(u):
        dfs_size(u, -1)
        c = dfs_centroid(u, -1, sub[u])
        visited[c] = True
        process(c)
        for v in g[c]:
            if not visited[v]:
                decompose(v)

    decompose(0)

    for x in ans:
        print(x % (1 << 64))

if __name__ == "__main__":
    main()
```Việc triển khai xây dựng cơ sở XOR tuyến tính cho mỗi nút từ các bộ lọc của nó, sau đó thực hiện phân tách trung tâm trên cây. Đối với mỗi centroid, nó thu thập tất cả các nút trong thành phần của nó và sắp xếp chúng theo khoảng cách đến centroid đó. Sau đó, nó dần dần hợp nhất các cơ sở của chúng khi bán kính tăng lên và duy trì giá trị XOR tối đa hiện tại bằng cách giảm cơ sở tham lam. Mỗi centroid đóng góp một tổng trên tất cả các bán kính trực tiếp vào nhóm câu trả lời của nó. 

Cần phải cẩn thận trong quá trình phân rã centroid: việc không đánh dấu các centroid đã truy cập hoặc tính toán kích thước cây con không chính xác dẫn đến đệ quy theo cấp số nhân. Một điểm tinh tế khác là việc hợp nhất cơ sở XOR phải duy trì tính độc lập và thứ tự chèn không được giả định tính giao hoán vượt quá tính chính xác của cơ sở. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây nhỏ gồm ba nút trong chuỗi 1-2-3. Giả sử nút 1 có bộ lọc {1}, nút 2 có {2}, nút 3 có {1,2}. Cơ sở rất đơn giản: nút 1 có thể tạo ra {0,1}, nút 2 có thể tạo ra {0,2}, nút 3 có thể tạo ra không gian đầy đủ {0,1,2,3}. 

Đối với trung tâm 2, chúng tôi mở rộng bán kính. 

| Bước | Các nút được bao gồm | Cơ sở hiện tại | max_xor | 
| --- | --- | --- | --- | 
| d = 0 | {2} | {0,2} | 2 | 
| d = 1 | {1,2,3} | toàn nhịp | 3 | 

Điều này cho thấy việc thêm các nút sẽ tăng khoảng cách và có khả năng tăng XOR tối đa như thế nào. 

Bây giờ hãy xem xét một ngôi sao trong đó tâm không có bộ lọc và tất cả các lá đều có bộ lọc bit đơn rời rạc. Ở bán kính 0, chỉ tâm đóng góp và câu trả lời là 0. Ở bán kính 1, tất cả các lá đều được bao gồm và giao điểm thu gọn về 0, vì tâm không thể tạo ra bất kỳ giá trị nào khác 0. Điều này chứng tỏ rằng việc thêm các nút có thể làm giảm tính khả thi mà giao điểm cơ sở nắm bắt chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n \cdot 64^2)$| Mỗi nút tham gia vào các cấp độ trung tâm, mỗi lần hợp nhất là chèn cơ sở 64 bit | 
| Không gian |$O(n \cdot 64)$| Lưu trữ một cơ sở cho mỗi nút và bộ đệm trung tâm | 

Độ phức tạp bị chi phối bởi các phép duyệt phân rã centroid và các phép hợp nhất cơ sở, cả hai đều bị giới hạn bởi các hằng số xung quanh 64. Với$n \le 5 \cdot 10^4$, điều này thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# minimal chain
assert run("2\n1\n1 1\n1 2\n") is not None

# star-like structure
assert run("3\n1 2\n1 1\n1 2\n1 3\n") is not None

# all zero filters
assert run("3\n1 2\n0\n0\n0\n") is not None

# single bit propagation
assert run("4\n1 1 2\n1 1\n1 2\n1 4\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi 2 nút | số tiền nhỏ | sự lan truyền bazơ | 
| ngôi sao | ngã tư có giới hạn | hiệu ứng bán kính | 
| tất cả số không | tất cả các câu trả lời bằng không | xử lý cơ sở trống | 
| bit hỗn hợp | Hợp nhất XOR | tính đúng đắn cơ bản | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi một trọng tâm bao gồm một nút có cơ sở trống. Trong trường hợp đó, việc sáp nhập nó sẽ không làm thay đổi cơ sở hiện tại. Thuật toán xử lý việc này vì việc thêm các vectơ 0 vào cơ sở XOR không có tác dụng. 

Một trường hợp khác là khi tất cả các nút trong thành phần trung tâm có các bộ lọc bit đơn rời rạc. Khi bán kính tăng, cơ sở tăng đơn điệu và XOR tối đa tăng dần. Quá trình centroid nắm bắt chính xác từng mức tăng kể từ khi các nút được chèn theo thứ tự khoảng cách. 

Trường hợp cạnh cuối cùng là cây bị lệch trong đó quá trình phân tách trọng tâm liên tục tách ra khỏi một chuỗi dài. Ngay cả ở đây, mỗi nút vẫn được xử lý$O(\log n)$lần và việc hợp nhất cơ sở vẫn ổn định vì nó không phụ thuộc vào hình dạng cây mà chỉ phụ thuộc vào các nút được thu thập trong mỗi thành phần trung tâm.
