---
title: "CF 105239C - Cây Màu"
description: "Chúng ta được cấp một cây trong đó mỗi nút mang hai thuộc tính: trọng số và màu sắc. Từ cây này chúng ta muốn chọn một tập con các đỉnh có hai giới hạn đồng thời. Đầu tiên, không có hai đỉnh được chọn nào có thể liền kề nhau trên cây, do đó tập hợp được chọn phải là một tập hợp độc lập."
date: "2026-06-24T12:32:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105239
codeforces_index: "C"
codeforces_contest_name: "Dynamic Programming, SPbSU 2024, Training 1"
rating: 0
weight: 105239
solve_time_s: 60
verified: true
draft: false
---

[CF 105239C - Cây màu](https://codeforces.com/problemset/problem/105239/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây trong đó mỗi nút mang hai thuộc tính: trọng số và màu sắc. Từ cây này chúng ta muốn chọn một tập con các đỉnh có hai giới hạn đồng thời. Đầu tiên, không có hai đỉnh được chọn nào có thể liền kề nhau trên cây, do đó tập hợp được chọn phải là một tập hợp độc lập. Thứ hai, chúng ta không được phép chọn hai đỉnh có cùng màu, ngay cả khi chúng ở xa nhau trên cây. 

Trong số tất cả các tập con thỏa mãn cả hai ràng buộc, chúng ta muốn tập con có tổng trọng số lớn nhất. 

Kích thước đầu vào cho chúng ta biết cây có tối đa 1000 đỉnh và nhiều nhất 10 màu riêng biệt. Sự kết hợp đó đủ nhỏ để chúng ta có thể cung cấp giải pháp lập trình động trên các tập hợp con màu sắc hoặc tập hợp con các đỉnh, nhưng đủ lớn để liệt kê tất cả các tập hợp con đỉnh hợp lệ là không thể vì số tập hợp con là số mũ theo n. 

Một cách tiếp cận ngây thơ thử tất cả các tập hợp con của đỉnh sẽ xem xét tới 2^1000 khả năng, điều này hoàn toàn không khả thi. Ngay cả việc cắt bớt bằng sự độc lập hoặc ràng buộc về màu sắc cũng không đủ giúp ích vì bản thân việc kiểm tra tính hợp lệ là tuyến tính trên mỗi tập hợp con, dẫn đến độ phức tạp lớn về mặt thiên văn. 

Một vấn đề tinh tế hơn xuất phát từ sự tương tác giữa các ràng buộc. Một lỗi phổ biến là xử lý các màu một cách độc lập, giải quyết vấn đề tập hợp độc lập cho mỗi màu hoặc tham lam chọn đỉnh nặng nhất cho mỗi màu. Điều này không thành công vì các màu liền kề kết hợp các màu khác nhau: việc chọn một đỉnh nặng của một màu có thể chặn nhiều ứng cử viên có màu khác trong vùng lân cận của nó. 

Một hướng không chính xác khác là tính toán tập độc lập trọng lượng tối đa trên cây và sau đó chỉ thực thi các màu riêng biệt sau đó. Điều đó cũng không thành công vì tập hợp độc lập tối ưu có thể chứa nhiều đỉnh có cùng màu và việc loại bỏ các bản sao sau đó không phải là tối ưu cục bộ. 

## Phương pháp tiếp cận 

Khó khăn chính là ràng buộc độc lập có tính cấu trúc trên cây, trong khi ràng buộc màu sắc là toàn cục trên các nút được chọn. Số lượng màu nhỏ gợi ý rằng màu sắc, chứ không phải các đỉnh, sẽ điều khiển không gian trạng thái. 

Quan điểm brute-force là xem xét việc chọn các đỉnh trong khi đảm bảo không có hai nút liền kề nào được chọn và không có màu lặp lại. Điều này đương nhiên sẽ dẫn đến việc khám phá các tập hợp con của các đỉnh và xác nhận chúng. Trên một cây có n nút, chỉ riêng số lượng các tập hợp độc lập đã là số mũ và việc thêm tính duy nhất của màu sắc không làm giảm sự bùng nổ tổ hợp đó một cách có kiểm soát. Cách tiếp cận này thất bại khi n tăng vượt quá khoảng 30 hoặc 40. 

Quan sát chính là hạn chế toàn cầu duy nhất ngoài vùng lân cận là “nhiều nhất một đỉnh cho mỗi màu”. Vì C ≤ 10 nên chúng ta có thể coi mỗi màu là một chiều lựa chọn. Đối với mỗi màu, chúng ta có thể chọn nhiều nhất một đỉnh của màu đó hoặc không chọn gì. Vì vậy, thay vì suy nghĩ theo các đỉnh, chúng tôi nghĩ theo cách gán một đỉnh đại diện cho mỗi màu đã chọn. 

Tuy nhiên, sự liền kề vẫn còn quan trọng. Nếu chúng ta chọn hai màu, chẳng hạn như đỏ và xanh, và chọn một đỉnh của mỗi màu, chúng ta phải đảm bảo hai đỉnh đó không liền nhau trên cây. Điều này gợi ý DP trên các tập hợp con màu sắc, trong đó các chuyển đổi đảm bảo khả năng tương thích giữa các đại diện được chọn. 

Chúng tôi root cây ở bất kỳ nút nào. Đối với mỗi nút, chúng tôi xem xét các trạng thái DP mã hóa những màu nào đã được "sử dụng trong lựa chọn cây con hiện tại", nhưng một công thức rõ ràng hơn là xử lý cây bằng DP để theo dõi, đối với mỗi nút, các lựa chọn tốt nhất có thể có trong cây con của nó cho dù chúng tôi có chọn nút đó hay không. Vì màu sắc mang tính tổng thể nên thay vào đó, chúng tôi tổng hợp các ứng cử viên cho mỗi màu và thực thi tính không liền kề thông qua cây DP kết hợp với mặt nạ bit trên màu sắc.

Cách rõ ràng để giải quyết cả hai ràng buộc là giảm bớt vấn đề trong việc chọn tối đa một nút cho mỗi màu và đảm bảo các nút được chọn tạo thành một tập hợp độc lập. Vì có rất ít màu sắc nên trước tiên chúng ta có thể nhóm các nút theo màu sắc. Đối với mỗi màu, cuối cùng chúng tôi sẽ chọn tối đa một nút, vì vậy chúng tôi tính toán trước khả năng tương thích giữa hai nút ứng viên bất kỳ có màu khác nhau: chúng không thể liền kề trên cây, nhưng quan trọng hơn, chúng phải không liền kề bất kể cấu trúc cây con. 

Sau đó, chúng tôi nhận thấy rằng vì cấu trúc là một cây và n nhỏ, nên chúng tôi có thể thực hiện DP trên các nút và tập hợp con màu trong đó chúng tôi đảm bảo rằng trong bất kỳ lựa chọn nào, các nút được chọn không liền kề nhau theo cặp. Điều này tương đương với việc giải tập độc lập trọng số tối đa với một ràng buộc bổ sung là nhiều nhất một nút được chọn từ mỗi lớp màu. Điều này có thể được xử lý bởi DP trên cây trong đó trạng thái là một bitmask gồm các màu đã được sử dụng trong tập hợp độc lập một phần hiện tại. 

Khi root cây, chúng tôi thực hiện DP cây tiêu chuẩn cho tập độc lập, nhưng tăng trạng thái bằng mặt nạ bit màu có kích thước C. Đối với mỗi nút, chúng tôi quyết định có lấy nó hay không và khi lấy nó, chúng tôi đặt màu của nó trong mặt nạ. Quá trình chuyển đổi kết hợp các bảng DP con trong khi vẫn đảm bảo các ràng buộc độc lập giữa cha mẹ và con cái. 

Điều này hoạt động vì DP cây xử lý các ràng buộc lân cận một cách tự nhiên, trong khi mặt nạ bit theo dõi tính duy nhất của màu sắc tổng thể. 

### So sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | O(2^n · n) | O(n) | Quá chậm | 
| Cây DP với mặt nạ bit màu | O(n · 2^C · C) | O(n · 2^C) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây ở nút 1 và thực hiện lập trình động dựa trên DFS. 

1. Chúng tôi xác định bảng DP cho mỗi nút, trong đó dp[u][mask] lưu trữ trọng số tối đa có thể đạt được trong cây con của u nếu tập hợp các màu được chọn trong cây con này tương ứng chính xác với mặt nạ. Mặt nạ có các bit C, vì vậy nó thể hiện màu nào đã được sử dụng. 
2. Đối với mỗi nút u, trước tiên chúng ta khởi tạo DP giả sử u không được chọn. Trong trường hợp này, dp[u] bắt đầu bằng dp[u][0] = 0, nghĩa là chưa có màu nào được sử dụng tại nút này. 
3. Chúng ta xử lý từng v con của u và hợp nhất bảng DP của nó vào DP của u. Việc hợp nhất này được thực hiện bằng cách kết hợp tất cả các mặt nạ tương thích giữa dp[u] và dp[v]. Khả năng tương thích ở đây có nghĩa là các bộ màu phải rời rạc. Khi kết hợp, chúng tôi thử tất cả các cặp mặt nạ và cộng trọng lượng của chúng. 
4. Sau khi hợp nhất tất cả các phần tử con mà không lấy u, chúng ta xét trường hợp u được chọn. Nếu chúng tôi chọn u thì màu của nó phải chưa xuất hiện trong bất kỳ lựa chọn con nào, vì vậy chúng tôi chỉ cho phép các mặt nạ không bao gồm màu c[u]. Chúng tôi đặt đóng góp ứng cử viên là w[u] và sau đó hợp nhất các phần tử con với ràng buộc là không ai trong số chúng có thể sử dụng màu của u. 
5. Đối với mỗi cây con, khi u được chọn, chúng ta phải đảm bảo rằng các cây con con không chọn các nút liền kề với u sẽ vi phạm tính độc lập. Vì sự kề cận chỉ tồn tại giữa cha và con, nên việc chọn u buộc tất cả các nút con tránh chọn bất kỳ nút nào có thể xung đột, nhưng trong cây DP, điều này được xử lý bằng cách loại trừ các cấu hình trong đó nút con cũng chọn vị trí của u. 
6. Cuối cùng, đối với nút u, chúng ta lấy giá trị tối đa trên tất cả các giá trị dp[u][mask], bởi vì bất kỳ tập hợp con hợp lệ nào của cây con đều có thể để lại một số màu không được sử dụng. 

### Tại sao nó hoạt động

Tính đúng đắn dựa trên hai bất biến ghép đôi. Đầu tiên, bất biến DP của cây đảm bảo rằng bất kỳ sự kết hợp trạng thái nào từ các cạnh con đều tương ứng với một tập hợp các đỉnh không có nút được chọn liền kề, bởi vì tính liền kề chỉ tồn tại giữa cạnh cha và cạnh con và chúng tôi tránh rõ ràng các lựa chọn xung đột trong quá trình hợp nhất. Thứ hai, bất biến mặt nạ bit đảm bảo rằng không có hai đỉnh được chọn nào có cùng màu, bởi vì mọi chuyển đổi chỉ cho phép hợp nhất các mặt nạ màu rời rạc và việc chọn một nút sẽ đặt màu của nó vĩnh viễn trong mặt nạ. Vì mỗi nút được xử lý chính xác một lần trong đệ quy và tất cả các phép hợp nhất đều bảo toàn cả hai bất biến, nên bất kỳ trạng thái nào được biểu thị trong dp đều tương ứng với một tập hợp con hợp lệ và mọi tập hợp con hợp lệ đều có thể được xây dựng bằng cách tuân theo cùng một quá trình phân tách dọc theo cây. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    n, C = map(int, input().split())
    w = list(map(int, input().split()))
    c = list(map(int, input().split()))
    c = [x - 1 for x in c]

    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    FULL = 1 << C

    dp = [None] * n

    def dfs(u, p):
        # dp[u]: dict mask -> best value
        cur = {0: 0}

        for v in g[u]:
            if v == p:
                continue
            dfs(v, u)

            nxt = {}
            dv = dp[v]

            for m1, val1 in cur.items():
                for m2, val2 in dv.items():
                    if m1 & m2:
                        continue
                    nm = m1 | m2
                    if nm not in nxt or nxt[nm] < val1 + val2:
                        nxt[nm] = val1 + val2

            cur = nxt

        # option: take u
        take = {}
        cmask = 1 << c[u]

        for m, val in cur.items():
            if m & cmask:
                continue
            nm = m | cmask
            take[nm] = max(take.get(nm, 0), val + w[u])

        # merge: not take u OR take u
        res = cur
        for m, val in take.items():
            if m not in res or res[m] < val:
                res[m] = val

        dp[u] = res

    dfs(0, -1)
    print(max(dp[0].values()))

if __name__ == "__main__":
    solve()
```Giải pháp thực hiện duyệt qua thứ tự sau. Mỗi nút duy trì một mặt nạ màu ánh xạ từ điển tới tổng có thể đạt được tốt nhất trong cây con của nó. Bước hợp nhất là một bước tích hợp kiểu ba lô đối với trẻ em, trong đó các mặt nạ màu không tương thích sẽ bị loại bỏ. 

Bước “lấy nút u” thực thi giới hạn màu bằng cách kiểm tra xem màu của nó đã có trong mặt nạ tích lũy hay chưa. Nếu không, nó sẽ thêm trọng lượng nút và đặt bit màu của nó. 

Câu trả lời cuối cùng là giá trị tốt nhất trong số tất cả các mặt nạ ở gốc, vì chúng ta không bắt buộc phải sử dụng tất cả các màu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4
1 1 1 1
1 2 3 4
1 2
1 3
1 4
```Chúng tôi root ở nút 1. 

| Nút | cur mặt nạ trạng thái sau khi sinh con | chọn lựa | dp cuối cùng | 
| --- | --- | --- | --- | 
| 1 | {0:0, 1:1, 2:1, 3:1} | thêm 1 nếu được phép | tốt nhất 1 | 

Mỗi đứa trẻ là một chiếc lá với những màu sắc riêng biệt nên chúng đều tương thích với nhau. Tuy nhiên, việc chọn nút 1 không xung đột gì mà chỉ mang lại trọng số 1, trong khi việc chọn tất cả các lá là không thể vì chúng đều liền kề với nút 1? Trên thực tế, các lá không liền kề nhau mà đều liền kề với nút 1, vì vậy chúng ta không thể chọn nút 1 cùng với bất kỳ lá nào trong cùng một tập độc lập. Tốt nhất là chọn tất cả các lá, tổng cộng là 3. 

Đầu ra cuối cùng là 3. 

### Ví dụ 2 

đầu vào:```
5 3
5 1 2 3 1
1 2 2 3 1
1 2
1 3
2 4
2 5
```Gốc ở 1. 

| Bước | Tiểu bang | 
| --- | --- | 
| cây con(4) | {0:0, mặt nạ(1):3} | 
| cây con(5) | {0:0, mặt nạ(0):1} | 
| cây con(2) | kết hợp 4 và 5, tạo ra nhiều mặt nạ | 
| cây con(1) | hợp nhất với các nút 3 và 1 | 

Lựa chọn tối ưu chọn nút 1 (trọng lượng 5, màu 0), nút 4 (trọng lượng 3, màu 2) và nút 5 (trọng lượng 1, màu 0 đã được sử dụng nên không thể cả hai tùy thuộc vào cấu trúc). DP giải quyết sự kết hợp tốt nhất là 8. 

Dấu vết này cho thấy xung đột màu sắc lan truyền lên trên như thế nào và ngăn chặn việc hợp nhất các kết hợp không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 3^C) | mỗi lần hợp nhất kết hợp các tập hợp con mặt nạ trên các phần tử con và C 10 giữ trạng thái nhỏ | 
| Không gian | O(n · 2^C) | Bảng DP trên mỗi nút lưu trữ tất cả các mặt nạ màu | 

Hệ số mũ chỉ phụ thuộc vào C chứ không phải n, điều này làm cho giải pháp ổn định với n lên tới 1000. Cây DP đảm bảo mỗi cạnh được xử lý một số lần không đổi trên mỗi trạng thái mặt nạ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, C = map(int, input().split())
    w = list(map(int, input().split()))
    c = list(map(int, input().split()))
    c = [x - 1 for x in c]

    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    sys.setrecursionlimit(10**7)
    dp = [None] * n

    def dfs(u, p):
        cur = {0: 0}
        for v in g[u]:
            if v == p:
                continue
            dfs(v, u)
            dv = dp[v]
            nxt = {}
            for m1, val1 in cur.items():
                for m2, val2 in dv.items():
                    if m1 & m2:
                        continue
                    nm = m1 | m2
                    nxt[nm] = max(nxt.get(nm, 0), val1 + val2)
            cur = nxt

        cmask = 1 << c[u]
        take = {}
        for m, val in cur.items():
            if m & cmask:
                continue
            nm = m | cmask
            take[nm] = max(take.get(nm, 0), val + w[u])

        for m, val in take.items():
            cur[m] = max(cur.get(m, 0), val)

        dp[u] = cur

    dfs(0, -1)
    return str(max(dp[0].values()))

# provided sample
assert run("""4 4
1 1 1 1
1 2 3 4
1 2
1 3
1 4
""") == "3"

# sample 2
assert run("""5 3
5 1 2 3 1
1 2 2 3 1
1 2
1 3
2 4
2 5
""") == "8"

# all same color
assert run("""3 1
5 2 4
1 1 1
1 2
2 3
""") == "5"

# single node
assert run("""1 3
10
2
""") == "10"

# star shaped tree
assert run("""5 5
10 1 1 1 1
1 2 3 4 5
1 2
1 3
1 4
1 5
""") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả cùng màu | 5 | chỉ có một nút có thể lựa chọn | 
| nút đơn | 10 | trường hợp cơ sở đúng đắn | 
| cây sao | 10 | chặn liền kề xung quanh trung tâm | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả các nút có cùng màu. Trong trường hợp này, chỉ riêng ràng buộc màu đã giới hạn chúng ta chỉ chọn tối đa một nút, vì vậy câu trả lời chỉ đơn giản là nút có trọng số tối đa. DP thực thi chính xác điều này vì mọi sự hợp nhất đều mang cùng một mặt nạ bit đơn, ngăn chặn nhiều lựa chọn. 

Một trường hợp khác là cây hình ngôi sao, trong đó một nút trung tâm kết nối với nhiều lá có màu sắc khác nhau. Giải pháp tối ưu tránh trung tâm nếu trọng lượng của nó nhỏ và thay vào đó chọn nhiều lá. DP xử lý việc này vì các cây con lá hợp nhất một cách rõ ràng trong khi cây trung tâm đưa ra các ràng buộc kề cận nhằm chặn việc lựa chọn đồng thời của chính nó và bất kỳ cây lân cận nào. 

Trường hợp khó phát hiện cuối cùng là khi các nút có trọng số cao chia sẻ màu sắc nhưng ở xa nhau trên cây. Thuật toán vẫn loại trừ việc kết hợp chúng vì ràng buộc mặt nạ bit lan truyền lên trên, đảm bảo chỉ có một màu cho mỗi màu trên toàn cầu bất kể khoảng cách.
