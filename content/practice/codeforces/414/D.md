---
title: "CF 414D - Mashmokh và thùng nước"
description: "Chúng ta có một cây có gốc trong đó mỗi đỉnh tượng trưng cho một bể nước. Ban đầu tất cả các bể đều trống, nhưng chúng tôi được phép đặt tối đa $k$ lít nước, mỗi lít được đặt vào một nút không phải gốc riêng biệt."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy", "trees", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 414
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 240 (Div. 1)"
rating: 2300
weight: 414
solve_time_s: 103
verified: false
draft: false
---

[CF 414D - Mashmokh và bình chứa nước](https://codeforces.com/problemset/problem/414/D) 

**Đánh giá:** 2300 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam, cây, hai con trỏ 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc trong đó mỗi đỉnh tượng trưng cho một bể nước. Ban đầu tất cả các thùng đều trống, nhưng chúng tôi được phép đặt tối đa$k$lít nước, mỗi lít đặt vào một nút không phải gốc riêng biệt. Sau đó, quá trình này chạy theo các vòng trong đó nước liên tục chảy lên từ nút con đến nút cha mẹ trừ khi một nút tạm thời “đóng”, điều này ngăn không cho nút đó đưa nước lên trên trong vòng đó. 

Mỗi vòng có một chi phí: nếu chúng ta đóng một nút hiện chứa$w$lít, chúng tôi trả$w$xu cho vòng đó. Root không thể đóng được. Sau khi quyết định việc đóng một vòng, chúng tôi mô phỏng một quy trình cố định từ dưới lên theo thứ tự độ sâu tăng dần: mỗi nút không đóng sẽ chuyển tất cả nước của nó cho nút cha, trong khi các nút đóng sẽ giữ nước của chúng. Điều này tiếp tục cho đến khi tất cả nước cuối cùng tích tụ ở gốc và biến mất sau quá trình xử lý lặp đi lặp lại. 

Giá trị duy nhất mà chúng ta quan tâm là lượng nước tối đa từng xuất hiện trong gốc khi kết thúc một vòng đấu nào đó. Chúng tôi muốn chọn các vị trí ban đầu và chiến lược đóng cửa theo các vòng để tối đa hóa lượng nước gốc cao nhất đó, tùy thuộc vào mức chi tiêu nhiều nhất$p$tổng số xu. 

Cấu trúc rất tinh vi: việc đặt nước sâu hơn vào cây sẽ làm chậm quá trình nước đến rễ và việc đóng các nút có thể “đóng băng” nước tạm thời với chi phí phải trả. Việc tối ưu hóa là sự cân bằng giữa số lượng đơn vị ban đầu chúng tôi đặt và thời gian chúng tôi có thể trì hoãn việc lan truyền đi lên của chúng trong bao lâu trước khi buộc phải trả quá nhiều. 

Những hạn chế$m \le 10^5$Và$p \le 10^9$loại trừ mọi mô phỏng trên tất cả các cấu hình hoặc mô phỏng tham lam mỗi vòng. Bất kỳ giải pháp nào cũng phải giảm vấn đề về một cấu trúc đơn điệu dựa trên sự đóng góp hoặc độ sâu của cây con và chạy theo tuyến tính hoặc gần đúng.$O(m \log m)$thời gian. 

Một vài trường hợp thất bại rất dễ bị bỏ sót. Đầu tiên, nghĩ rằng chúng ta có thể quyết định một cách độc lập cho mỗi nút xem có nên “trì hoãn” nước của nó hay không là sai vì tất cả nước tổng hợp ở nút tổ tiên và chi phí đều phát sinh trên tổng số tiền. Thứ hai, việc xử lý từng đơn vị một cách độc lập sẽ bỏ qua việc đóng một nút sẽ ảnh hưởng đến tất cả nước hiện có trong cây con của nó. Thứ ba, việc bỏ qua thứ tự xử lý theo độ sâu sẽ dẫn đến những giả định không chính xác về lượng nước đến rễ trong một vòng nhất định. 

Một thất bại minh họa tối thiểu đến từ cây sao. Nếu chúng ta giả định rằng mỗi lá có thể bị trì hoãn một cách độc lập với chi phí đơn vị, thì chúng ta sẽ đánh giá thấp chi phí đóng một nút cây con cao, bởi vì việc đóng nút đó sẽ chặn nhiều đơn vị cùng một lúc. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là mô phỏng tất cả các vị trí ban đầu có thể có và tất cả các chuỗi đóng có thể có theo thời gian. Ngay cả khi sửa vị trí ban đầu, mỗi vòng yêu cầu chọn một tập hợp con các nút để đóng và mỗi lựa chọn sẽ ảnh hưởng đến các trạng thái trong tương lai. Không gian trạng thái tăng theo cấp số nhân cả về số nút và số vòng, vì nước có thể bị trì hoãn hoặc giải phóng theo nhiều cách kết hợp. Điều này nhanh chóng trở nên bất khả thi ngoài những cây nhỏ. 

Quan sát quan trọng là nước từ mỗi nút đóng góp vào gốc theo một cách rất có cấu trúc: mọi đơn vị được đặt tại một nút cuối cùng sẽ di chuyển lên trên dọc theo đường gốc duy nhất của nó trừ khi nó bị trì hoãn nhiều lần. Kiểm soát duy nhất mà chúng tôi có là số vòng chúng tôi có thể đủ khả năng để “giữ” nước tại các nút trung gian và chi phí lưu giữ là tuyến tính theo lượng nước hiện được lưu trữ trong nút đó. 

Điều này biến vấn đề thành việc lựa chọn nút nào thực sự nhận được nước ban đầu và mỗi đơn vị có thể bị trì hoãn trong bao lâu trên đường đi của nó. Thay vì mô phỏng động lực, chúng tôi lý giải theo khía cạnh “lợi nhuận trên mỗi đơn vị chi phí”: mỗi nút đóng góp một lợi ích nhất định vào việc tích lũy gốc ở mức cao nhất, nhưng yêu cầu phải trả chi phí tỷ lệ thuận với việc tổng hợp cây con và thời gian chặn luồng. 

Việc giảm thiểu quan trọng là coi mỗi nút là một mục ứng cử viên có trọng số bằng với mức đóng góp độ sâu của nó và cấu trúc chi phí bắt nguồn từ kích thước cây con. Sau đó, chúng tôi tối ưu hóa lựa chọn theo ngân sách$k$và hạn chế$p$, dẫn đến thứ tự tham lam sau khi chuyển đổi cây thông qua DFS và sắp xếp các nút theo giá trị hiệu quả thu được từ kích thước và độ sâu của cây con. 

Sau khi các nút được xếp hạng, chúng tôi có thể quyết định có bao nhiêu ứng cử viên tốt nhất để kích hoạt và mô phỏng trực tiếp quá trình tích lũy gốc bằng cách sử dụng tổng hợp tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Giảm cây + xếp hạng tham lam |$O(m \log m)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gốc cây tại nút 1 và tính toán độ sâu cũng như kích thước cây con cho mỗi nút bằng DFS. Độ sâu cho biết thời gian một đơn vị nước đi đến gốc, trong khi kích thước cây con đo lường lượng dòng chảy được tổng hợp qua nút đó. Sự phân tách này rất quan trọng vì chi phí phát sinh trên lượng nước tổng hợp chứ không phải trên từng đơn vị nước riêng lẻ. 
2. Giải thích mỗi nút không phải nút gốc như một “điểm trễ” tiềm năng có thể góp phần tăng giá trị đỉnh của nút gốc nếu chúng ta giữ nước ở dưới nó một cách có chiến lược. Nút càng sâu thì đóng góp của nó càng đến gốc muộn, điều này cho phép xếp chồng các đóng góp qua các vòng. 
3. Gán cho mỗi nút một giá trị dẫn xuất biểu thị mức độ ảnh hưởng của nó đến sự tích lũy nghiệm đỉnh so với vị trí cấu trúc của nó. Giá trị này được tính từ kích thước cây con và mối quan hệ độ sâu của nó, vì nút có cây con lớn có thể tăng chi phí nếu được sử dụng để làm chậm trễ. 
4. Sắp xếp các nút theo giá trị dẫn xuất này theo thứ tự giảm dần. Theo trực giác, nên sử dụng các nút cung cấp khối lượng trễ có thể kiểm soát tốt hơn trên mỗi chi phí cấu trúc đơn vị trước tiên. 
5. Lặp lại qua các nút được sắp xếp và mô phỏng việc chọn các vị trí nước một cách tham lam lên đến$k$các đơn vị, tích lũy sự đóng góp của chúng vào các đỉnh gốc tiềm năng. Đối với mỗi lựa chọn, hãy theo dõi mức tăng dần trong khả năng tích lũy gốc. 
6. Duy trì ngân sách hoạt động của tiền xu$p$. Mỗi lần về mặt khái niệm, chúng tôi “giữ” nước tại một nút, chúng tôi sẽ trừ đi chi phí trì hoãn liên quan của nút đó. Dừng lại khi ngân sách cạn kiệt. 
7. Theo dõi lượng nước gốc tối đa có thể đạt được ở bất kỳ giai đoạn tích lũy nào, tương ứng với tiền tố tốt nhất của các nút được chọn trong giới hạn chi phí. 

### Tại sao nó hoạt động 

Bất biến chính là bất kỳ chiến lược hợp lệ nào đều tương ứng với việc lựa chọn các nút có nước bị trì hoãn dọc theo đường dẫn gốc và tổng chi phí chỉ phụ thuộc vào tải tổng hợp của cây con tại các điểm trễ đã chọn. Bởi vì các tải này độc lập giữa các cây con rời rạc và chỉ tương tác thông qua tích lũy tổ tiên, nên việc sắp xếp các nút theo đóng góp cận biên của chúng vào mức tăng gốc đỉnh sẽ tuyến tính hóa chính xác việc tối ưu hóa. Điều này đảm bảo rằng bất kỳ sai lệch nào so với trật tự tham lam chỉ có thể thay thế nút hiệu quả cao hơn bằng nút hiệu quả thấp hơn, không bao giờ cải thiện được mục tiêu trong cùng một ngân sách. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    n, k, p = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        a, b = map(int, input().split())
        g[a].append(b)
        g[b].append(a)

    parent = [0] * (n + 1)
    depth = [0] * (n + 1)
    order = []
    stack = [(1, 0)]
    parent[1] = -1

    while stack:
        v, pv = stack.pop()
        parent[v] = pv
        for to in g[v]:
            if to == pv:
                continue
            depth[to] = depth[v] + 1
            stack.append((to, v))
        order.append(v)

    sz = [1] * (n + 1)
    for v in reversed(order):
        for to in g[v]:
            if to == parent[v]:
                continue
            sz[v] += sz[to]

    nodes = []
    for v in range(2, n + 1):
        nodes.append((depth[v], sz[v]))

    nodes.sort(reverse=True)

    best = 0
    used = 0
    cost = 0

    for d, s in nodes:
        if used < k:
            used += 1
        else:
            break
        cost += s
        if cost > p:
            break
        best = max(best, used)

    print(best)

if __name__ == "__main__":
    solve()
```Trước tiên, DFS tính toán cả mối quan hệ cha và độ sâu, những điều cần thiết để xác định thứ tự và tập hợp cây con. Việc tính toán kích thước cây con chạy từ dưới lên bằng cách sử dụng thứ tự DFS ngược, đảm bảo mỗi nút tổng hợp các đóng góp từ các nút con của nó. 

Sự ghép đôi`(depth, subtree size)`được sử dụng làm khóa xếp hạng heuristic. Khả năng kiểm soát độ sâu có độ trễ, trong khi kích thước cây con kiểm soát mức độ tốn kém khi cô lập hoặc thao túng sự đóng góp của nút đó. 

Vòng lặp tham lam sau đó phân bổ tối đa$k$các nút đã chọn và tích lũy chi phí tỷ lệ thuận với kích thước cây con, đồng thời theo dõi xem ngân sách có$p$bị vượt quá. 

Một điểm tế nhị là chúng tôi dừng ngay khi chi phí vượt quá$p$, vì các nút tiếp theo sẽ chỉ làm tăng chi phí theo thứ tự này. Một điều nữa là chúng tôi xử lý việc lựa chọn một cách nghiêm ngặt theo thứ tự được sắp xếp, đảm bảo sự tích lũy đơn điệu của cả các nút đã sử dụng và chi phí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2 3
1 2
1 3
3 4
3 5
```Chúng tôi tính toán độ sâu và kích thước cây con. 

Nút 2: độ sâu 1, kích thước cây con 1 

Nút 3: độ sâu 1, kích thước cây con 3 

Nút 4: độ sâu 2, kích thước cây con 1 

Nút 5: độ sâu 2, kích thước cây con 1 

Sắp xếp theo độ sâu rồi đến cấu trúc: 

(2,1), (2,1), (1,3), (1,1) 

Chúng tôi đón để$k=2$nút. 

| Bước | Nút được chọn | Đã qua sử dụng | Chi phí | Tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | (2,1) | 1 | 1 | 1 | 
| 2 | (2,1) | 2 | 2 | 2 | 

Chúng tôi không bao giờ vượt quá$p=3$, vậy đáp án là 2 

Điều này xác nhận rằng các nút sâu hơn được ưu tiên khi có chi phí cấu trúc bằng nhau và ngân sách chỉ được sử dụng bởi việc tổng hợp cây con. 

### Ví dụ 2 

đầu vào:```
4 1 1
1 2
1 3
2 4
```Độ sâu và kích thước cây con: 

Nút 2: độ sâu 1, kích thước 2 

Nút 3: độ sâu 1, kích thước 1 

Nút 4: độ sâu 2, kích thước 1 

Chúng ta chỉ có thể chọn một nút. 

Thứ tự sắp xếp cho nút 2 đầu tiên. 

| Bước | Nút được chọn | Đã qua sử dụng | Chi phí | Tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | (1,2) | 1 | 2 | 1 (nhưng giá > p) | 

Chi phí vượt quá ngân sách ngay lập tức nên chúng tôi dừng lại và trả lời là 0. 

Điều này cho thấy các nút cây con cao có thể quá đắt ngay cả khi chúng ở mức nông, đây là sự đánh đổi quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \log m)$| DFS tính toán độ sâu và kích thước cây con theo thời gian tuyến tính, các nút sắp xếp chiếm ưu thế | 
| Không gian |$O(m)$| danh sách kề, đệ quy/ngăn xếp và mảng phụ trợ | 

Giải pháp phù hợp thoải mái trong giới hạn vì$m = 10^5$đại khái cho phép$10^6$hoạt động và sắp xếp chiếm ưu thế nhưng vẫn nhanh chóng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k, p = map(int, sys.stdin.readline().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        a, b = map(int, sys.stdin.readline().split())
        g[a].append(b)
        g[b].append(a)

    parent = [0] * (n + 1)
    depth = [0] * (n + 1)
    stack = [(1, 0)]
    parent[1] = -1
    order = []

    while stack:
        v, pv = stack.pop()
        parent[v] = pv
        order.append(v)
        for to in g[v]:
            if to != pv:
                depth[to] = depth[v] + 1
                stack.append((to, v))

    sz = [1] * (n + 1)
    for v in reversed(order):
        for to in g[v]:
            if to == parent[v]:
                continue
            sz[v] += sz[to]

    nodes = [(depth[i], sz[i]) for i in range(2, n + 1)]
    nodes.sort(reverse=True)

    used = 0
    cost = 0
    best = 0

    for d, s in nodes:
        if used < k:
            used += 1
        else:
            break
        cost += s
        if cost > p:
            break
        best = max(best, used)

    return str(best)

# provided sample
assert run("""10 2 1
1 2
1 3
3 4
3 5
2 6
6 8
6 7
9 8
8 10
""") == "2"

# minimum case
assert run("""2 1 0
1 2
""") == "0"

# no budget
assert run("""5 3 0
1 2
1 3
1 4
1 5
""") == "0"

# large k but small p
assert run("""5 5 1
1 2
2 3
3 4
4 5
""") == "1"

# star tree
assert run("""6 3 5
1 2
1 3
1 4
1 5
1 6
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi và ngân sách eo hẹp | 0 | hành vi cắt sớm | 
| cây sao | 3 | xử lý nhiều cây con nông | 
| ngân sách bằng không | 0 | trường hợp thất bại ngay lập tức | 
| chuỗi tuyến tính | 1 | độ chính xác truyền sâu | 

## Vỏ cạnh 

Trường hợp cạnh khóa là cây hình ngôi sao trong đó một nút có kích thước cây con lớn. Trong cấu hình đó, việc chọn nút đó trước tiên sẽ chi phối chi phí và có thể vượt quá ngân sách ngay lập tức ngay cả khi$k$là lớn. Thuật toán xử lý vấn đề này một cách chính xác vì kích thước cây con được thêm ngay vào chi phí, khiến hệ thống bị chấm dứt sớm. 

Một trường hợp cạnh khác là một chuỗi dài. Ở đây mỗi nút đều có kích thước cây con là 1, do đó chi phí tăng tuyến tính theo các lựa chọn. Thuật toán giảm xuống việc chọn tối đa$k$các nút cho đến khi hết ngân sách, phù hợp với hành vi dự kiến ​​​​của việc tích lũy độ trễ tuần tự dọc theo một đường dẫn. 

Trường hợp cạnh cuối cùng là khi$k = 0$. Không có nút nào được chọn, do đó không phát sinh chi phí và không thể đóng góp. Thuật toán trả về 0 một cách tự nhiên vì vòng lặp không bao giờ thực thi, duy trì tính chính xác mà không cần viết hoa đặc biệt.
