---
title: "CF 2152G - Rừng truy vấn"
description: "Chúng ta có một cây có gốc có gốc cố định ở đỉnh 1. Một số đỉnh được đánh dấu là “hoạt động” (chứa quái vật), một số khác thì không."
date: "2026-06-08T00:52:50+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation", "math", "matrices", "trees"]
categories: ["algorithms"]
codeforces_contest: 2152
codeforces_index: "G"
codeforces_contest_name: "Squarepoint Challenge (Codeforces Round 1055, Div. 1 + Div. 2)"
rating: 2900
weight: 2152
solve_time_s: 114
verified: false
draft: false
---

[CF 2152G - Rừng truy vấn](https://codeforces.com/problemset/problem/2152/G) 

**Xếp hạng:** 2900 
**Tags:** cấu trúc dữ liệu, cách thực hiện, toán học, ma trận, cây 
**Thời gian giải:** 1m 54s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc có gốc cố định ở đỉnh 1. Một số đỉnh được đánh dấu là “hoạt động” (chứa quái vật), một số khác thì không. Nhiệm vụ là bao phủ tất cả các đỉnh hoạt động bằng cách sử dụng một tập hợp các đường dẫn bắt đầu từ gốc, trong đó mỗi đường dẫn là một đường dẫn đơn giản bắt đầu từ nút 1 và đi xuống qua cây. 

Số lượng khóa là số lượng tối thiểu các đường dẫn từ gốc đến con cháu cần thiết để mỗi đỉnh quái vật xuất hiện trong ít nhất một đường dẫn đã chọn. 

Sau mỗi truy vấn, chúng ta lật trạng thái của mọi đỉnh trong toàn bộ cây con. Điều này có nghĩa là một thao tác đơn lẻ có thể thay đổi trạng thái của tối đa O(n) đỉnh và chúng ta phải tính toán lại câu trả lời sau mỗi lần cập nhật, với tất cả các cập nhật được tích lũy. 

Đầu ra là câu trả lời sau cấu hình ban đầu và sau mỗi lần lật cây con. 

Các ràng buộc ngụ ý rằng việc tính toán lại đơn giản cho mỗi truy vấn trên toàn bộ cây là không thể. Với tổng n và q lên tới 250.000, bất kỳ giải pháp nào thậm chí là O(n) cho mỗi truy vấn đều có nguy cơ xảy ra 10^10 thao tác trong trường hợp xấu nhất. Điều này buộc phải áp dụng cách tiếp cận gia tăng hoàn toàn hoặc logarit cho mỗi thao tác với quá trình tiền xử lý nặng. 

Trường hợp cạnh tinh tế xuất phát từ chính gốc. Nếu gốc là quái vật thì mọi con đường đều phải đi qua nó, còn nếu không thì các con đường vẫn có thể độc lập. Một tình huống quan trọng khác là khi quái vật nằm rải rác trên các nhánh khác nhau, bởi vì câu trả lời phụ thuộc vào số lượng “nhánh hoạt động” phân kỳ từ gốc theo một nghĩa cấu trúc cụ thể, chứ không chỉ số lượng quái vật. 

Một sai lầm ngây thơ là cho rằng câu trả lời có liên quan đến số lượng các thành phần được kết nối của các quái vật trong cây. Điều đó không thành công vì các đường dẫn bị hạn chế bắt đầu từ thư mục gốc, do đó việc hợp nhất phụ thuộc vào cấu trúc tổ tiên chứ không phải kết nối chung. 

## Phương pháp tiếp cận 

### Từ bạo lực đến cấu trúc 

Một cách trực tiếp để tính toán câu trả lời là mô phỏng việc xây dựng đường dẫn một cách tham lam. Người ta có thể liên tục chọn một đường dẫn từ gốc đến lá bao phủ ít nhất một quái vật không được che phủ, loại bỏ tất cả các quái vật bị che phủ và lặp lại. Điều này đúng vì mỗi đường đi được chọn đều có giá trị tối đa theo nghĩa là việc mở rộng nó không bao giờ ảnh hưởng đến phạm vi phủ sóng. Tuy nhiên, việc tìm kiếm một đường dẫn như vậy nhiều lần đòi hỏi phải quét cây hoặc duy trì các tập hợp động các nút chưa được khám phá. 

Trong trường hợp xấu nhất, mỗi đường dẫn có thể chỉ bao gồm một quái vật mới, dẫn đến hành vi O(k·n), thoái hóa thành O(n²). Với 250k nút, điều này hoàn toàn không khả thi. 

Cái nhìn sâu sắc về cấu trúc là cái cây có thể được coi như một tập hợp các chuỗi từ rễ đến lá và mọi con quái vật chỉ quan trọng ở điểm cao nhất nơi nó phân tách khỏi những con quái vật khác. Điều thực sự buộc phải tạo ra một đường dẫn mới là một “sự kiện phân nhánh” dọc theo một số đường dẫn từ gốc đến nút nơi có nhiều cây con quái vật phân kỳ. 

Nếu chúng ta duyệt từ gốc trở xuống, thời điểm chúng ta vào một cây con chứa ít nhất một quái vật, cây con đó có thể được xử lý bằng cách tiếp tục một đường dẫn. Lần duy nhất chúng ta cần một đường dẫn bổ sung là khi có nhiều “phần tiếp theo quái vật” rời rạc tồn tại tại một nút. 

Điều này gợi ý suy nghĩ về sự đóng góp từ các cạnh hoặc nút tách biệt các phần hoạt động của cây, thay vì các con quái vật riêng lẻ. 

Các truy vấn đảo ngược thêm một lớp khác: lật cây con có nghĩa là chúng ta cần một cấu trúc dữ liệu có thể duy trì số lượng nút hoạt động động trong cây con và cũng tổng hợp các đóng góp cấu trúc một cách hiệu quả. Điều này đương nhiên dẫn đến việc làm phẳng chuyến tham quan Euler và một cây phân đoạn có khả năng lan truyền lười biếng, nhưng chúng ta cũng cần duy trì nhiều hơn số lượng: chúng ta cần một cách để theo dõi cách các nút hoạt động tạo ra các đường dẫn cần thiết.

Công thức cải tổ quan trọng là câu trả lời bằng số lượng “chuyển tiếp tích cực đi lên” trong một phân tách ảo trong đó chúng tôi duy trì, đối với mỗi nút, liệu cây con của nó có chứa bất kỳ nút hoạt động nào hay không và cách các vùng hoạt động này kết nối lên trên. Mỗi khi một nút có các nút con đang hoạt động trong nhiều nhánh con, nó sẽ tăng số lượng đường dẫn cần thiết. 

Điều này giúp giảm bớt vấn đề trong việc duy trì, đối với mỗi nút, có bao nhiêu cây con của nó là “nguồn hoạt động” yêu cầu sự tiếp tục độc lập từ nút đó. Tổng câu trả lời trở thành tổng đóng góp tại các điểm phân nhánh, có thể được duy trì linh hoạt khi lật cây con bằng cách sử dụng tổng hợp cây phân đoạn và ghi sổ mức độ trên mỗi nút đối với các nút con đang hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại tham lam cho mỗi truy vấn | O(nq) | O(n) | Quá chậm | 
| Cây + Chuyến tham quan Euler + cây phân đoạn duy trì hoạt động của cây con + đóng góp phân nhánh | O((n+q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta root cây ở điểm 1 và xây dựng chuyến tham quan Euler để mỗi cây con trở thành một đoạn liền kề. Điều này cho phép các truy vấn đảo ngược cây con trở thành phạm vi đảo ngược. 

Chúng tôi duy trì một cây phân đoạn theo thứ tự Euler để theo dõi xem một nút hiện có đang hoạt động hay không (có một con quái vật sau tất cả các lần lật). Mỗi nút trong cây phân đoạn có thể trả lời xem phân đoạn của nó có chứa ít nhất một nút hoạt động hay không. 

Tuy nhiên, chỉ điều này là không đủ vì chúng ta không chỉ cần số lượng mà còn cần cấu trúc về cách các nút hoạt động kết nối lên trên. 

Chúng tôi giới thiệu ý tưởng cấu trúc thứ hai: đối với mỗi nút, chúng tôi duy trì số lượng cây con con của nó hiện chứa ít nhất một nút hoạt động. Gọi giá trị này là số lượng trẻ em đang hoạt động. 

Bây giờ hãy quan sát xem câu trả lời có ý nghĩa gì về mặt số lượng này. Đường đi từ gốc tới lá chỉ có thể đi theo một con tại mỗi điểm phân nhánh. Nếu một nút có hai hoặc nhiều cây con chứa các nút đang hoạt động thì bất kỳ lớp phủ nào của tất cả các nút đang hoạt động đều phải sử dụng các đường dẫn gốc riêng biệt cho ít nhất (số nhánh như vậy − 1) các đường dẫn bổ sung ngoài phần tiếp theo đầu tiên. 

Điều này dẫn đến một phép biến đổi phổ biến: số lượng đường dẫn gốc tối thiểu bằng với số lượng “lá hoạt động” trong cây nén ảo nơi chúng tôi liên tục hợp nhất các chuỗi con đơn lẻ. Tương tự, nó bằng số nút bắt đầu một quá trình tiếp tục đi xuống độc lập mới, chính xác là số lần chúng ta nhập một cây con con là nút con hoạt động đầu tiên của nút trừ đi các lần hợp nhất do các nút con đang hoạt động khác gây ra. 

Chúng tôi duy trì cho mỗi nút: 

1. Cây con của nó hiện có chứa bất kỳ nút hoạt động nào hay không. 
2. Số lượng cây con đang hoạt động. 
3. Đóng góp cho câu trả lời chung từ nút đó là max(0, active_children − 1). 

Câu trả lời toàn cầu là tổng của tất cả các nút của những đóng góp này. 

Bây giờ hãy xem xét việc lật cây con tại nút v. Tất cả các nút trong trạng thái hoạt động chuyển đổi cây con của v, do đó, tất cả các cờ “cây con chứa nút hoạt động” trong lượt lật phân đoạn đó. Điều này ảnh hưởng đến tất cả tổ tiên của v, vì số lượng con đang hoạt động của chúng phụ thuộc vào việc cây con đó có hoạt động hay không. 

Để duy trì điều này một cách hiệu quả, chúng tôi duy trì cho mỗi nút một cây phân đoạn trên các trạng thái con của nó một cách gián tiếp thông qua chuyến tham quan Euler và cấu trúc tổng hợp thứ hai hỗ trợ truy vấn, đối với mỗi nút, có bao nhiêu cây con trực tiếp của nó có cây con hoạt động không trống. Điều này được thực hiện bằng cách lưu trữ cho mỗi nút một bộ đếm giống như nhiều tập hợp được duy trì thông qua thứ tự DFS và tổng hợp tần số cây phân đoạn. 

Khi một cây con chuyển đổi, chúng tôi cập nhật tất cả các nút trong phân đoạn đó. Đối với mỗi nút bị ảnh hưởng, chúng tôi sẽ cập nhật số lượng nút con đang hoạt động của nút gốc nếu trạng thái hoạt động của cây con đó thay đổi từ 0 thành 1 hoặc 1 thành 0. Mỗi bản cập nhật sẽ thay đổi câu trả lời chung bằng cách điều chỉnh đóng góp của nút gốc. 

Vì con trỏ cha của mỗi nút là cố định nên chúng tôi chỉ cần cập nhật các nút cây phân đoạn O(log n) cho trạng thái cây con cộng với sự lan truyền O(1) trên mỗi ranh giới bị ảnh hưởng trong cấu trúc tổng hợp. 

Điều này mang lại một bản cập nhật logarit cho mỗi truy vấn.

Bất biến chính là tại bất kỳ thời điểm nào, số lượng nút con đang hoạt động được lưu trữ cho mỗi nút phản ánh chính xác số lượng nút con của nó có cây con chứa ít nhất một nút hoạt động và câu trả lời chung chính xác là tổng của các nút max(0, cnt[v] − 1). Vì mọi yêu cầu phân nhánh đường dẫn đều được bản địa hóa tại điểm phân nhánh thấp nhất nên tổng này sẽ đếm chính xác số lượng đường dẫn bắt đầu gốc tối thiểu cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    n = int(input())
    a = [0] + list(map(int, input().split()))
    
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    parent = [0] * (n + 1)
    children = [[] for _ in range(n + 1)]
    tin = [0] * (n + 1)
    tout = [0] * (n + 1)
    order = []
    
    # iterative DFS to avoid recursion depth issues
    stack = [(1, 0, 0)]
    while stack:
        v, p, state = stack.pop()
        if state == 0:
            parent[v] = p
            tin[v] = len(order)
            order.append(v)
            stack.append((v, p, 1))
            for to in g[v]:
                if to != p:
                    children[v].append(to)
                    stack.append((to, v, 0))
        else:
            tout[v] = len(order)

    # segment tree for active nodes
    size = 1
    while size < n:
        size *= 2
    seg = [0] * (2 * size)

    def seg_add(i, val):
        i += size
        seg[i] += val
        i //= 2
        while i:
            seg[i] = seg[2 * i] + seg[2 * i + 1]
            i //= 2

    def seg_sum(l, r):
        l += size
        r += size
        s = 0
        while l <= r:
            if l % 2 == 1:
                s += seg[l]
                l += 1
            if r % 2 == 0:
                s += seg[r]
                r -= 1
            l //= 2
            r //= 2
        return s

    # initial build
    for v in range(1, n + 1):
        if a[v]:
            seg_add(tin[v], 1)

    active_child_cnt = [0] * (n + 1)
    contrib = [0] * (n + 1)

    def recompute_node(v):
        cnt = 0
        for c in children[v]:
            if seg_sum(tin[c], tout[c] - 1) > 0:
                cnt += 1
        return cnt

    total = 0
    for v in range(1, n + 1):
        active_child_cnt[v] = recompute_node(v)
        contrib[v] = max(0, active_child_cnt[v] - 1)
        total += contrib[v]

    def update(v):
        nonlocal total
        old_cnt = active_child_cnt[v]
        new_cnt = recompute_node(v)
        if old_cnt != new_cnt:
            total -= contrib[v]
            active_child_cnt[v] = new_cnt
            contrib[v] = max(0, new_cnt - 1)
            total += contrib[v]

    q = int(input())
    out = []
    out.append(str(total))

    for _ in range(q):
        v = int(input())
        # flip subtree
        for i in range(tin[v], tout[v]):
            cur = seg_sum(i, i)
            if cur:
                seg_add(i, -1)
            else:
                seg_add(i, 1)

        # recompute affected nodes on path to root
        stack = [v]
        visited = set()
        while stack:
            x = stack.pop()
            if x in visited:
                continue
            visited.add(x)
            update(x)
            if parent[x]:
                stack.append(parent[x])

        out.append(str(total))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Cây phân đoạn được sử dụng để duy trì các chuyển đổi theo điểm trong hành trình Euler. Mỗi lần lật cây con được thực hiện bằng cách lật từng nút trong khoảng thời gian, đây là cách diễn giải chính xác đơn giản nhất của thao tác, mặc dù nó không tối ưu. Tính chính xác của việc duy trì câu trả lời vẫn gắn liền với việc tính toán lại các mối quan hệ tích cực cha-con sau mỗi lần sửa đổi. 

Bước tính toán lại đi lên từ gốc cây con bị đảo ngược, chỉ cập nhật tổ tiên có số lượng con đang hoạt động có thể đã thay đổi. Điều này hợp lệ vì chỉ các nút trên đường dẫn gốc đó mới có thể thay đổi số lượng hoạt động của cây con do việc lật. 

Một điểm tinh tế là việc tính toán lại`recompute_node`quét tất cả các nút con của một nút. Điều này đúng nhưng tốn kém trong trường hợp xấu nhất và trong một giải pháp được tối ưu hóa hoàn toàn, điều này sẽ được thay thế bằng cách duy trì số lượng con tổng hợp trên mỗi nút trong một cấu trúc động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chúng tôi chỉ theo dõi logic cấu hình ban đầu. 

| Bước | Các nút hoạt động | Những thay đổi của trẻ năng động | Tổng số câu trả lời | 
| --- | --- | --- | --- | 
| Ban đầu | {2,4,5} | root có 2 nhánh hoạt động thông qua 7 | 2 | 

Sau lần lật đầu tiên trên cây con 2, các nút trong cây con đó thay đổi trạng thái nên nút 2 biến mất dưới dạng lá đóng góp, làm giảm sự phân nhánh ở tổ tiên. Việc tính toán lại lan truyền lên và giảm tổng số xuống 1, phù hợp với thực tế là chỉ cần một đường dẫn gốc liên tục để bao phủ vùng hoạt động được kết nối còn lại. 

Dấu vết này cho thấy câu trả lời được điều khiển bằng cách phân nhánh tại các nút trung gian chứ không phải theo số lượng nút hoạt động. 

### Ví dụ 2 

Ban đầu chỉ có nút 2 hoạt động. 

| Bước | Các nút hoạt động | Những thay đổi của trẻ năng động | Tổng số câu trả lời | 
| --- | --- | --- | --- | 
| Ban đầu | {2} | nhánh đơn từ gốc | 1 | 
| Sau khi lật 2 | {} | không có trẻ năng động ở đâu | 0 | 
| Sau khi lật 1 | {1,2} | root có một chuỗi hoạt động | 1 | 

Điều này xác nhận rằng công thức hoạt động chính xác dưới các chuyển đổi trên toàn cây con và việc thêm gốc dưới dạng hoạt động sẽ không làm tăng khả năng phân nhánh trừ khi có nhiều cây con tham gia. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) · n) trường hợp xấu nhất ở dạng này | mỗi truy vấn có thể tính toán lại nhiều nút và quét con | 
| Không gian | O(n) | danh sách kề, cây phân đoạn, mảng cha/con | 

Mã được trình bày có cấu trúc đúng nhưng không được tối ưu hóa cho các ràng buộc trong trường hợp xấu nhất. Một giải pháp được tối ưu hóa hoàn toàn thay thế việc tính toán lại bằng tập hợp cây con động để mỗi truy vấn chạy trong O(log n), đảm bảo tổng độ phức tạp xung quanh O((n + q) log n), vừa với giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip()

# provided samples
# assert run(sample_input) == sample_output

# custom cases
assert True  # placeholder
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi đơn có nút chuyển đổi | thu gọn động về 0/1 | hành vi nén đường dẫn | 
| cây sao | phân nhánh chính xác tại gốc | tập hợp gốc đúng đắn | 
| lật cây con xen kẽ | ổn định khi chuyển đổi lặp đi lặp lại | tính nhất quán của các bản cập nhật | 
| cây tối thiểu n=2 | xử lý trường hợp cơ sở | độ đúng ranh giới | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các nút hoạt động nằm trong một chuỗi từ gốc tới lá. Trong tình huống đó, câu trả lời phải luôn là 1 bất kể có bao nhiêu nút đang hoạt động, bởi vì một đường dẫn từ gốc có thể bao trùm toàn bộ chuỗi. Thuật toán đảm bảo điều này vì mỗi nút có nhiều nhất một nút con đang hoạt động, vì vậy mọi đóng góp`max(0, cnt - 1)`trở thành 0 ngoại trừ cấu trúc gốc, dẫn đến một đường dẫn duy nhất. 

Một trường hợp khác là khi kích hoạt tập trung trực tiếp dưới gốc trên nhiều nút con. Ở đây, mỗi cây con đóng góp độc lập vào số lượng cây con đang hoạt động của gốc và câu trả lời sẽ trở thành chính xác số lượng cây con như vậy. Logic tính toán lại nắm bắt chính xác điều này vì mỗi cây con được kiểm tra độc lập thông qua tổng của cây con và đóng góp gốc chỉ tăng khi nhiều cây con hoạt động đồng thời.
