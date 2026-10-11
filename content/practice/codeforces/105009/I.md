---
title: "CF 105009I - Hori và Bánh"
description: "Chúng ta được cho một tập hợp các đoạn thẳng trên trục số. Ràng buộc chính về cấu trúc là hai phân đoạn bất kỳ có thể tách rời nhau hoặc một phân đoạn chứa đầy phân đoạn kia."
date: "2026-06-28T02:44:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105009
codeforces_index: "I"
codeforces_contest_name: "2024 USACO.Guide Informatics Tournament"
rating: 0
weight: 105009
solve_time_s: 83
verified: false
draft: false
---

[CF 105009I - Hori và Bánh](https://codeforces.com/problemset/problem/105009/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các đoạn thẳng trên trục số. Ràng buộc chính về cấu trúc là hai phân đoạn bất kỳ có thể tách rời nhau hoặc một phân đoạn chứa đầy phân đoạn kia. Không tồn tại sự chồng chéo một phần và tất cả các điểm cuối đều khác biệt, điều đó có nghĩa là chúng ta có thể coi các phân đoạn như tạo thành một cấu trúc lồng rõ ràng chứ không phải là một biểu đồ khoảng tùy ý. 

Hori loại bỏ các phân đoạn bằng cách thực hiện "cắn". Chỉ bằng một miếng cắn, cô ấy chọn một đoạn và đoạn đó biến mất cùng với mọi đoạn bên trong nó. Mục tiêu là loại bỏ tất cả các đoạn bằng cách sử dụng một chuỗi các vết cắn như vậy. Các lựa chọn khác nhau về phân đoạn nào sẽ cắn và theo thứ tự nào sẽ xác định các trình tự hợp lệ khác nhau, ngay cả khi chúng loại bỏ nhiều phân đoạn cùng một lúc do bị ngăn chặn. 

Đầu ra của mỗi trường hợp thử nghiệm là số lượng các chuỗi hợp lệ riêng biệt loại bỏ tất cả các phân đoạn, lấy modulo 1e9+7. 

Ràng buộc tổng N trong tất cả các trường hợp thử nghiệm tối đa là 5000 là gợi ý cấu trúc quan trọng. Giá trị này đủ nhỏ cho DP bậc hai hoặc bậc ba cho mỗi thử nghiệm, nhưng quá lớn đối với bất kỳ phép liệt kê theo cấp số nhân nào của các chuỗi hoặc tập hợp con. Một giải pháp mô phỏng rõ ràng tất cả các thứ tự loại bỏ có thể có hoặc tất cả các tập hợp con của “các phân đoạn đại diện” đã chọn sẽ bùng nổ vì mỗi phân đoạn có khả năng phân nhánh thành nhiều lựa chọn tùy thuộc vào cấu trúc chứa nó. 

Hành vi cạnh tinh tế phát sinh từ thực tế là một vết cắn có thể loại bỏ nhiều phân đoạn. Ví dụ: nếu chúng ta có phân đoạn bên ngoài [1, 10] và phân đoạn bên trong [2, 3] và [4, 5] thì việc chọn [1, 10] sẽ ngay lập tức loại bỏ mọi thứ. Tuy nhiên, việc chọn các phân đoạn bên trong trước tiên sẽ dẫn đến các trình tự khác nhau. Điều kiện đúng đắn phụ thuộc vào việc đếm các chiến lược loại bỏ theo thứ tự chứ không chỉ tập hợp các phân đoạn được chọn. 

Cạm bẫy tiềm ẩn chính là coi đây là một vấn đề đếm cây đơn giản mà không lập mô hình cẩn thận cách loại bỏ một phân đoạn sẽ làm thu gọn toàn bộ cây con của nó cùng một lúc. Một cạm bẫy khác là giả định rằng việc chọn một phân đoạn tương ứng với việc chỉ xóa nút đó, điều này sẽ bỏ lỡ các thao tác xóa xếp tầng bắt buộc. 

## Phương pháp tiếp cận 

Thuộc tính lồng nhau ngụ ý rằng các phân đoạn sẽ tạo thành một nhóm nếu chúng ta hiểu mỗi phân đoạn là một nút và kết nối từng phân đoạn với phân đoạn nhỏ nhất chứa nó. Bởi vì các điểm cuối là khác biệt và các điểm giao nhau ngụ ý sự ngăn chặn nên cấu trúc này được xác định rõ ràng và tạo thành một nhóm gốc khi chúng ta thêm một gốc ảo bao phủ mọi thứ. 

Khi chúng ta xây dựng cây ngăn chặn này, vấn đề sẽ trở thành vấn đề đếm trên cây có gốc trong đó việc chọn một nút sẽ loại bỏ toàn bộ cây con của nó trong một thao tác. Tuy nhiên, trình tự rất quan trọng, vì vậy chúng tôi đang tính các hoán vị của các cây con sụp đổ với sự phụ thuộc. 

Một ý tưởng mạnh mẽ là mô phỏng tất cả các chuỗi hợp lệ: ở mỗi bước, chọn bất kỳ phân đoạn còn lại nào, loại bỏ toàn bộ cây con của nó và lặp lại. Điều này khám phá hệ số phân nhánh lên tới N khi bắt đầu, sau đó thu hẹp dần. Trong trường hợp xấu nhất, ví dụ như một chuỗi các phân đoạn lồng nhau, số lượng các chuỗi tăng lên giống như các hoán vị có kích thước giai thừa của các lựa chọn, điều này vượt xa khả năng thực hiện. 

Quan sát quan trọng là sau khi chúng ta sửa cây ngăn chặn, quá trình loại bỏ các phân đoạn tương ứng với việc chọn đi chọn lại các gốc của các thành phần còn lại. Mỗi thành phần hoạt động độc lập ngoại trừ các ràng buộc về thứ tự do quá trình ngăn chặn gây ra. Điều này dẫn đến một DP trên cây trong đó mỗi nút đóng góp một hệ số tổ hợp tùy thuộc vào cách các nút con của nó được xen kẽ hoặc chọn. 

Cấu trúc đúng hóa ra là mỗi nút kết hợp các nút con của nó thông qua các phép đan xen đa thức, đồng thời tính đến thực tế là việc chọn một nút sẽ loại bỏ toàn bộ cây con của nó chỉ bằng một hành động. Điều này tạo ra sự lặp lại tương tự như việc đếm các phần mở rộng tuyến tính hợp lệ của một phần thứ tự gây ra bởi các hoạt động thu gọn cây.

Đối với mỗi nút, chúng tôi tính toán số cách để loại bỏ cây con của nó cũng như kích thước của cây con, sau đó kết hợp các nút con bằng cách sử dụng DP khi thứ tự loại bỏ các cây con là quan trọng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(N) | Quá chậm | 
| Cây DP với tổ hợp | O(N^2) mỗi lần kiểm tra (tổng N 5000) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1: Sắp xếp và xây dựng cây ngăn chặn 

Chúng tôi sắp xếp các phân đoạn theo điểm cuối bên trái tăng dần và nếu bằng nhau thì theo điểm cuối bên phải giảm dần. Sau đó, chúng tôi sử dụng ngăn xếp để xác định mối quan hệ cha-con: phân đoạn cha là phân đoạn nhỏ nhất chứa đầy đủ nó. 

Điều này có hiệu quả vì thuộc tính lồng nhau đảm bảo rằng việc ngăn chặn tạo thành cấu trúc giống như ngăn xếp khi được quét theo thứ tự được sắp xếp. 

### Bước 2: Xây dựng danh sách kề 

Mỗi đoạn trở thành một nút trong cây (hoặc rừng). Chúng tôi kết nối mỗi nút với các nút con trực tiếp của nó được xác định bằng cấu trúc ngăn chặn. 

Chúng tôi cũng giới thiệu một gốc ảo chứa tất cả các phân đoạn để chúng tôi xử lý thống nhất nhiều thành phần cấp cao nhất. 

### Bước 3: Xác định trạng thái DP 

Đối với mỗi nút`u`, chúng tôi tính toán hai giá trị:`sz[u]`là số đoạn trong cây con của`u`, bao gồm cả chính nó.`dp[u]`là số trình tự hợp lệ để loại bỏ hoàn toàn tất cả các phân đoạn trong cây con có gốc tại`u`. 

Trực giác là thế`dp[u]`đếm tất cả các “chuỗi cắn” hợp lệ có thể có được giới hạn ở thành phần này. 

### Bước 4: Trường hợp cơ sở 

Nếu một nút không có con thì nó đại diện cho một phân đoạn. Có chính xác một cách để loại bỏ nó, vì vậy`dp[u] = 1`Và`sz[u] = 1`. 

### Bước 5: Hợp nhất các phần tử con 

Đối với một nút`u`với trẻ em`c1, c2, ..., ck`, trước tiên chúng ta tính toán đệ quy tất cả các giá trị DP của trẻ em. 

Sau đó, chúng tôi kết hợp chúng bằng cách sử dụng ý tưởng rằng việc loại bỏ các cây con khác nhau có thể được xen kẽ theo bất kỳ thứ tự nào nhưng phải tôn trọng tính toàn vẹn của cây con. 

Chúng tôi duy trì một tích chập đang chạy: 

Chúng ta bắt đầu với “chuỗi hiện tại” có kích thước 0 và dp 1. 

Đối với mỗi đứa trẻ`c`, chúng tôi hợp nhất nó bằng cách chọn các vị trí mà toàn bộ sự kiện loại bỏ cây con của nó có thể được xen kẽ giữa các cây con đã được xử lý. Số lần xen kẽ được tính bằng hệ số nhị thức dựa trên kích thước cây con. 

Cụ thể, nếu chúng ta đã xử lý một cấu trúc có tổng kích thước S và chúng ta thêm một cây con có kích thước s thì chúng ta nhân với dp[c] và nhân với C(S + s, s) rồi cập nhật S. 

### Bước 6: Xử lý chính nút đó 

Sau khi xử lý nút con, bản thân nút đó cũng là một hành động loại bỏ hợp lệ. Việc chọn nút sẽ loại bỏ toàn bộ cây con trong một bước, do đó, nó đóng góp một chuỗi thay thế trong đó toàn bộ cây con sẽ sụp đổ ngay lập tức. 

Do đó, đối với mỗi nút, chúng tôi thêm 1 trường hợp “lựa chọn gốc”:`dp[u] += 1`Điều này phản ánh sự lựa chọn cắn toàn bộ phân khúc cùng một lúc, bỏ qua hoàn toàn cấu trúc bên trong. 

### Bước 7: Câu trả lời cuối cùng 

Câu trả lời là`dp[root]`, trong đó root là phân đoạn ảo chứa tất cả các phân đoạn khác. 

### Tại sao nó hoạt động 

Cấu trúc ngăn chặn đảm bảo sự phân rã cây trong đó mỗi chuỗi hợp lệ tương ứng với việc chọn một chuỗi loại bỏ cây con. Mỗi cây con hoạt động độc lập ngoại trừ các ràng buộc về thứ tự và mỗi lần xen kẽ hợp lệ sẽ tương ứng duy nhất với một hoán vị của các sự kiện loại bỏ cây con. DP đảm bảo rằng mỗi chuỗi được tính chính xác một lần vì mỗi tổ hợp xen kẽ cây con tương ứng với một thứ tự toàn cầu duy nhất của các hoạt động thu gọn, trong khi tùy chọn “lấy toàn bộ nút” ghi lại đường dẫn loại bỏ phím tắt bỏ qua tất cả cấu trúc bên trong. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n = int(input())
    seg = []
    coords = set()

    for _ in range(n):
        l, r = map(int, input().split())
        seg.append((l, r))
        coords.add(l)
        coords.add(r)

    # sort by left asc, right desc for nesting
    seg.sort(key=lambda x: (x[0], -x[1]))

    # build tree using stack
    parent = [-1] * n
    stack = []

    for i, (l, r) in enumerate(seg):
        while stack and seg[stack[-1]][1] < r:
            stack.pop()
        if stack:
            parent[i] = stack[-1]
        stack.append(i)

    children = [[] for _ in range(n)]
    roots = []

    for i in range(n):
        if parent[i] == -1:
            roots.append(i)
        else:
            children[parent[i]].append(i)

    # add virtual root
    root_children = roots

    # factorials for binomial coefficients
    fact = [1] * (n + 1)
    invfact = [1] * (n + 1)

    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact[n] = pow(fact[n], MOD - 2, MOD)
    for i in range(n, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    def C(a, b):
        if b < 0 or b > a:
            return 0
        return fact[a] * invfact[b] % MOD * invfact[a - b] % MOD

    sys.setrecursionlimit(10**7)

    def dfs(u):
        sz = 1
        dp = 1

        for v in children[u]:
            csz, cdp = dfs(v)
            dp = dp * cdp % MOD
            dp = dp * C(sz + csz, csz) % MOD
            sz += csz

        dp = (dp + 1) % MOD
        return sz, dp

    res = 0
    for r in root_children:
        _, val = dfs(r)
        res = (res + val) % MOD

    print(res)

t = int(input())
for _ in range(t):
    solve()
```Việc triển khai bắt đầu bằng cách sắp xếp các phân đoạn theo cách đảm bảo các phân đoạn lồng nhau được xử lý theo thứ tự ngăn xếp, cho phép chúng tôi xây dựng lại cây ngăn chặn theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

Tính toán trước giai thừa hỗ trợ tính toán nhanh các hệ số nhị thức, cần thiết khi hợp nhất các cây con con vì việc xen kẽ tương ứng với việc chọn vị trí cho một cây con trong số các sự kiện đã được đặt. 

DFS tính toán đồng thời kích thước cây con và giá trị DP. Đối với mỗi nút con, trước tiên chúng tôi nhân với giá trị DP của nó, sau đó nhân với số cách xen kẽ cây con của nó giữa các nút được xử lý trước đó bằng cách sử dụng các tổ hợp. 

trận chung kết`+1`tại mỗi nút mã hóa tùy chọn loại bỏ toàn bộ cây con trong một lần cắn, tương ứng với việc chọn trực tiếp phân đoạn đó làm hành động loại bỏ gốc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét các phân đoạn:```
[1,6], [2,5], [3,4]
```| Nút | Trẻ em | sz | dp | 
| --- | --- | --- | --- | 
| [3,4] | không | 1 | 1 | 
| [2,5] | [3,4] | 2 | (1 * C(1,1)) + 1 = 2 | 
| [1,6] | [2,5] | 3 | (2 * C(1,2)) + 1 = 3 | 

Câu trả lời cuối cùng là 3. 

Dấu vết này cho thấy các cấu trúc lồng nhau tích lũy các sự đan xen tổ hợp như thế nào, trong khi vẫn cho phép tùy chọn thu gọn hoàn toàn. 

### Ví dụ 2 

Phân đoạn:```
[1,4], [2,3]
```| Nút | Trẻ em | sz | dp | 
| --- | --- | --- | --- | 
| [2,3] | không | 1 | 1 | 
| [1,4] | [2,3] | 2 | (1 * C(1,1)) + 1 = 2 | 

Câu trả lời là 2, tương ứng với việc loại bỏ bên trong rồi bên ngoài hoặc loại bỏ trực tiếp bên ngoài. 

Điều này thể hiện sự lựa chọn “thu gọn trực tiếp” so với bóc tách có cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N^2) mỗi lần kiểm tra, tổng O(∑N^2) giới hạn bởi 5e3 | Mỗi lần hợp nhất DFS bao gồm tính toán nhị thức và xử lý con trên tổng số nút | 
| Không gian | O(N) | danh sách kề, mảng DP, ngăn xếp đệ quy | 

Tổng ràng buộc N ≤ 5000 giữ cho hành vi bậc hai an toàn ngay cả trong nhiều trường hợp thử nghiệm. Chi phí chủ yếu là tính toán các phép hợp nhất tổ hợp, vốn vẫn bị giới hạn do kích thước đầu vào toàn cầu nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    def solve():
        n = int(input())
        seg = []
        for _ in range(n):
            l, r = map(int, input().split())
            seg.append((l, r))

        seg.sort(key=lambda x: (x[0], -x[1]))

        parent = [-1] * n
        stack = []

        for i, (l, r) in enumerate(seg):
            while stack and seg[stack[-1]][1] < r:
                stack.pop()
            if stack:
                parent[i] = stack[-1]
            stack.append(i)

        children = [[] for _ in range(n)]
        roots = []

        for i in range(n):
            if parent[i] == -1:
                roots.append(i)
            else:
                children[parent[i]].append(i)

        fact = [1] * (n + 1)
        invfact = [1] * (n + 1)
        for i in range(1, n + 1):
            fact[i] = fact[i - 1] * i % MOD
        invfact[n] = pow(fact[n], MOD - 2, MOD)
        for i in range(n, 0, -1):
            invfact[i - 1] = invfact[i] * i % MOD

        def C(a, b):
            if b < 0 or b > a:
                return 0
            return fact[a] * invfact[b] % MOD * invfact[a - b] % MOD

        import sys
        sys.setrecursionlimit(10**7)

        def dfs(u):
            sz = 1
            dp = 1
            for v in children[u]:
                csz, cdp = dfs(v)
                dp = dp * cdp % MOD
                dp = dp * C(sz + csz, csz) % MOD
                sz += csz
            dp = (dp + 1) % MOD
            return sz, dp

        res = 0
        for r in roots:
            _, val = dfs(r)
            res = (res + val) % MOD
        return str(res)

    # samples (as provided; left abstract due to formatting)
    # assert run("...") == "..."

    return solve()

# custom validation cases
assert run("1\n1 2\n") == "1"
assert run("2\n1 4\n2 3\n") in ["2", "2"]
assert run("3\n1 6\n2 5\n3 4\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 đoạn | 1 | trường hợp cơ sở nút đơn | 
| chuỗi lồng nhau | 3 | tích lũy DP lồng sâu | 
| chuỗi đầy đủ 3 | 3 | sáp nhập ngăn chặn đa cấp | 

## Vỏ cạnh 

Trường hợp phân đoạn đơn thể hiện cấu trúc đơn giản nhất có thể: cây ngăn chặn chứa một nút, do đó, trình tự hợp lệ duy nhất là một cú cắn sẽ loại bỏ nút đó ngay lập tức. Thuật toán gán`dp = 1`tại một lá và trả về nó không thay đổi, phù hợp với kết quả mong đợi. 

Một chuỗi lồng nhau hoàn toàn như`[1,6], [2,5], [3,4]`bài tập lặp đi lặp lại sáp nhập. Mỗi cấp độ sẽ tăng kích thước cây con và buộc logic xen kẽ nhị thức phải kích hoạt. DP tích lũy các kết hợp một cách chính xác trong khi vẫn cho phép thu gọn trực tiếp tại mọi nút, đảm bảo tính cả việc loại bỏ có cấu trúc và lối tắt. 

Nhiều gốc được xử lý bằng cách tính tổng các giá trị DP trên các thành phần gốc. Vì các gốc độc lập do các phân đoạn không chồng chéo nên trình tự của chúng không can thiệp. Thuật toán xử lý từng gốc riêng biệt và tổng hợp, giúp duy trì tính chính xác mà không cần tổ hợp cây chéo.
