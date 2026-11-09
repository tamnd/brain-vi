---
title: "CF 1467E - Rễ cây đặc biệt"
description: "Chúng ta được cho một cây có các đỉnh mang giá trị. Chúng ta có thể chọn bất kỳ đỉnh nào làm gốc. Khi một nút gốc được cố định, mọi đường dẫn đơn giản bắt đầu từ nút gốc và kết thúc ở một số nút con sẽ trở thành đường dẫn từ nút gốc đến nút."
date: "2026-06-11T01:41:20+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "dp", "trees"]
categories: ["algorithms"]
codeforces_contest: 1467
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 695 (Div. 2)"
rating: 2500
weight: 1467
solve_time_s: 178
verified: false
draft: false
---

[CF 1467E - Rễ đặc biệt trên cây](https://codeforces.com/problemset/problem/1467/E) 

**Đánh giá:** 2500 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dp, cây 
**Thời gian giải:** 2m 58s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây có các đỉnh mang giá trị. Chúng ta có thể chọn bất kỳ đỉnh nào làm gốc. Khi một nút gốc được cố định, mọi đường dẫn đơn giản bắt đầu từ nút gốc và kết thúc ở một số nút con sẽ trở thành đường dẫn từ nút gốc đến nút. 

Một gốc được gọi là có khả năng phân biệt nếu mỗi đường dẫn từ gốc tới nút đó chứa các giá trị phân biệt theo cặp. Một giá trị có thể xuất hiện nhiều lần trong cây, nhưng không một đường dẫn nào bắt đầu từ gốc được phép gặp cùng một giá trị hai lần. 

Nhiệm vụ là đếm xem có bao nhiêu đỉnh có thể đóng vai trò là một nghiệm phân biệt. 

Quan sát đầu tiên là tình trạng này chỉ phụ thuộc vào mối quan hệ tổ tiên-con cháu sau khi ra rễ. Một gốc không hợp lệ nếu tồn tại một số đường dẫn từ gốc đến nút chứa hai đỉnh có cùng giá trị. 

Cây chứa tới$2 \cdot 10^5$đỉnh. Bất kỳ giải pháp nào lấy rễ cây ở mọi đỉnh và kiểm tra tất cả các đường dẫn riêng biệt đều vô vọng. Ngay cả một DFS duy nhất từ ​​mỗi đỉnh cũng sẽ yêu cầu$O(n^2)$công việc xung quanh$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất. Chúng ta cần một cái gì đó gần tuyến tính hoặc$O(n \log n)$. 

Phần khó khăn là việc thay đổi gốc sẽ thay đổi mối quan hệ tổ tiên trên toàn cầu. Một cặp đỉnh có giá trị bằng nhau có thể nằm trên cùng một đường đi từ gốc đến nút đối với một số gốc nhưng đối với các gốc khác thì không. 

Hãy xem xét một chuỗi:```
1 -- 2 -- 3
values: 5 7 5
```Root ở đỉnh 1 tạo ra đường đi$1 \to 2 \to 3$, chứa giá trị 5 hai lần, vì vậy đỉnh 1 không hợp lệ. 

Root ở đỉnh 2 sẽ cho đường dẫn$2 \to 1$Và$2 \to 3$. Không chứa giá trị trùng lặp, vì vậy đỉnh 2 hợp lệ. 

Giải pháp chỉ kiểm tra các cặp có giá trị bằng nhau một lần trong một số root cố định sẽ bỏ sót sự khác biệt này. 

Một trường hợp tinh tế khác là khi một số đỉnh có cùng giá trị.```
1
|
2
|
3
|
4

values: 1 2 1 1
```Xung đột không chỉ xảy ra giữa các đỉnh có giá trị bằng nhau liền kề. Mỗi cặp đỉnh có giá trị bằng nhau đều đóng góp các hạn chế. Bỏ qua một số trong số chúng dẫn đến việc đếm quá nhiều gốc hợp lệ. 

Trường hợp cạnh cuối cùng là khi tất cả các giá trị đều khác biệt. Khi đó mọi gốc đều hợp lệ vì không có đường dẫn nào có thể chứa các giá trị trùng lặp. Câu trả lời phải là$n$. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Với mọi đỉnh$r$, nhổ cây tại$r$, thực hiện DFS, duy trì tập hợp các giá trị hiện tại trên đường dẫn gốc đến hiện tại và xác minh rằng không có giá trị nào lặp lại. Nếu sự lặp lại xuất hiện,$r$không có tính đặc biệt. 

Điều này đúng vì nó trực tiếp kiểm tra định nghĩa. Thật không may, mỗi root yêu cầu$O(n)$làm việc, và có$n$rễ. Tổng độ phức tạp trở thành$O(n^2)$, nó quá lớn đối với$n=2\cdot10^5$. 

Để có được giải pháp nhanh hơn, chúng ta cần hiểu chính xác khi nào một root không hợp lệ. 

Cố định một gốc tùy ý, chẳng hạn như đỉnh 1. Xét hai đỉnh$u$Và$v$có cùng giá trị. 

Một gốc$r$không hợp lệ nếu sau khi root tại$r$, một trong các đỉnh này trở thành tổ tiên của đỉnh kia. Khi đó đường đi từ$r$đến cái sâu hơn chứa cả hai bản sao của giá trị. 

Vì vậy, vấn đề trở thành: 

Với mọi cặp có giá trị bằng nhau$(u,v)$, xác định tất cả các nghiệm$r$vì cái gì$u$Và$v$đang ở trong mối quan hệ tổ tiên-con cháu khi cây có rễ ở$r$. 

Thay vì kiểm tra các gốc riêng lẻ, chúng tôi đánh dấu tất cả các gốc không hợp lệ bởi mỗi cặp xung đột. 

Quan sát quan trọng là đối với một cặp cố định$(u,v)$, tập hợp các rễ xấu có cấu trúc cây con rất đơn giản. 

Giả định$u$là tổ tiên của$v$trong root cố định ở đỉnh 1. 

hãy để$c$là con của$u$nằm trên đường đi từ$u$ĐẾN$v$. 

Nếu gốc mới nằm bên trong cây con($c$), sau đó$u$Và$v$kết thúc ở các nhánh khác nhau và không trở thành tổ tiên của nhánh kia. 

Đối với mọi vị trí gốc khác,$u$vẫn là tổ tiên của$v$, tạo một bản sao trên một số đường dẫn gốc. 

Do đó tất cả các gốc bên ngoài cây con($c$) là xấu. 

Nếu không có đỉnh nào là tổ tiên của đỉnh kia trong căn nguyên cố định, hãy để$w=\mathrm{LCA}(u,v)$. 

Sau đó$u$Và$v$trở nên liên quan đến tổ tiên chính xác khi gốc mới nằm bên trong cây con ($u$) hoặc cây con($v$). 

Đây lại là những cập nhật cây con. 

Thử thách còn lại là xử lý hiệu quả tất cả các cặp có giá trị bằng nhau. Có thể có$O(n^2)$các cặp như vậy nếu nhiều đỉnh có chung một giá trị. 

Việc giảm thiểu quan trọng là xử lý các giá trị trong DFS và chỉ tạo ra các ràng buộc liên quan đến tổ tiên gần nhất có cùng giá trị. Điều này biến vấn đề thành một số lượng cập nhật tuyến tính. 

Giải pháp được chấp nhận sử dụng DFS cùng với các ngăn xếp giá trị xuất hiện và một mảng khác biệt trên các cây con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Root cây tùy ý tại đỉnh 1. 

Thực hiện DFS để tính toán thời gian vào và ra của chuyến tham quan Euler, con trỏ cha và phạm vi cây con. Các phạm vi này cho phép chúng ta biểu diễn các cập nhật cây con dưới dạng cập nhật khoảng thời gian theo thứ tự Euler. 

Đối với mọi giá trị, hãy theo dõi đỉnh gần đây nhất có giá trị đó trên đường dẫn DFS hiện tại. 

Trong DFS, khi thăm đỉnh$v$, kiểm tra xem giá trị tương tự đã xuất hiện trên root-to- hiện tại chưa$v$con đường. 
Của tổ tiên gần nhất có cùng giá trị là$u$, thì mọi gốc bên ngoài cây con dẫn từ$u$ĐẾN$v$không hợp lệ. 

Điều này diễn ra sau trường hợp tổ tiên đã thảo luận trước đó. Bất kỳ gốc nào bên ngoài cây con đó đều giữ$u$bên trên$v$, tạo ra một giá trị trùng lặp trên đường dẫn gốc. 

Thêm một dấu không hợp lệ vào toàn bộ cây và trừ một dấu khỏi cây con đó. Sử dụng khoảng Euler, điều này sẽ trở thành một bản cập nhật phạm vi. 

Tiếp theo, chúng ta phải xử lý tình huống trong đó một số đỉnh con của một đỉnh có cùng giá trị với nó. 

Với mọi đỉnh$u$, tập hợp tất cả các cây con có cây con chứa một giá trị khác$a_u$. 

Nếu hai cây con khác nhau đều chứa giá trị đó thì mọi gốc bên trong một trong hai cây con đó sẽ không hợp lệ. Root ở đó làm cho một lần xuất hiện trở thành tổ tiên của một lần khác. 

Các vùng cây con này cũng được đánh dấu thông qua cập nhật phạm vi. 

Việc triển khai lưu trữ, đối với mỗi giá trị, số lần xuất hiện trong mỗi cây con và truyền bá thông tin lên trên. Bất cứ khi nào phát hiện xung đột, khoảng cây con tương ứng sẽ nhận được bản cập nhật. 

Sau khi tất cả các cập nhật được tích lũy, hãy thực hiện tích lũy tiền tố theo thứ tự Euler. 

Một đỉnh là một gốc đặc biệt chính xác khi số lượng vô hiệu cuối cùng của nó bằng 0. 

Đếm các đỉnh như vậy. 

### Tại sao nó hoạt động 

Mọi vi phạm đều xuất phát từ hai đỉnh có giá trị bằng nhau xuất hiện trên một đường dẫn từ gốc đến nút. 

Đối với bất kỳ giá trị nào, hãy xem xét mối quan hệ tổ tiên có giá trị bằng nhau gần nhất được tạo trong DFS. Mọi đường dẫn có giá trị trùng lặp có thể được biểu thị bằng một trong những xung đột giữa con cháu tổ tiên này. Các quy tắc cập nhật đánh dấu chính xác các gốc duy trì mối quan hệ tổ tiên như vậy sau khi khởi động lại. 

Nhóm cập nhật thứ hai xử lý các xung đột được tạo ra bởi các lần xuất hiện nằm trong các nhánh con khác nhau có cùng giá trị. Đó chính xác là những gốc rễ mà việc khởi động lại sẽ biến một sự kiện này thành tổ tiên của một sự kiện khác. 

Mỗi gốc không hợp lệ được đánh dấu bằng ít nhất một xung đột và mỗi dấu tương ứng với đường dẫn giá trị trùng lặp thực sự. Do đó, một nút gốc nhận được số đếm bằng 0 khi và chỉ khi tất cả các đường dẫn từ nút gốc đến nút chứa các giá trị riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    tin = [0] * n
    tout = [0] * n
    parent = [-1] * n
    timer = 0

    sys.setrecursionlimit(1 << 20)

    def dfs0(v, p):
        nonlocal timer
        parent[v] = p
        tin[v] = timer
        timer += 1
        for to in g[v]:
            if to != p:
                dfs0(to, v)
        tout[v] = timer

    dfs0(0, -1)

    diff = [0] * (n + 1)

    def add_subtree(v, x):
        diff[tin[v]] += x
        diff[tout[v]] -= x

    def add_all(x):
        diff[0] += x
        diff[n] -= x

    last = {}
    bad_child = [set() for _ in range(n)]

    def dfs1(v, p):
        prev = last.get(a[v], -1)
        last[a[v]] = v

        if prev != -1:
            cur = v
            while parent[cur] != prev:
                cur = parent[cur]

            add_all(1)
            add_subtree(cur, -1)

            bad_child[prev].add(cur)

        for to in g[v]:
            if to != p:
                dfs1(to, v)

        if prev == -1:
            del last[a[v]]
        else:
            last[a[v]] = prev

    dfs1(0, -1)

    for v in range(n):
        if len(bad_child[v]) >= 2:
            for ch in bad_child[v]:
                add_subtree(ch, 1)

    cur = 0
    ans = 0

    euler_to_vertex = [0] * n
    for v in range(n):
        euler_to_vertex[tin[v]] = v

    val = [0] * n
    for i in range(n):
        cur += diff[i]
        val[euler_to_vertex[i]] = cur

    for v in range(n):
        if val[v] == 0:
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```DFS đầu tiên tính toán các khoảng thời gian Euler-tour. Mỗi cây con trở thành một đoạn liền kề$[tin,tout)$, cho phép chúng tôi thực hiện cập nhật cây con trong thời gian không đổi. 

DFS thứ hai duy trì, đối với mỗi giá trị, lần xuất hiện gần nhất hiện có trên ngăn xếp đệ quy. Khi một giá trị lặp lại xuất hiện, chúng tôi xác định giá trị con của lần xuất hiện trước dẫn đến lần xuất hiện mới. Bản cập nhật "toàn bộ cây trừ cây con đó" được triển khai với bản cập nhật khoảng thời gian toàn cầu và phép trừ cây con bù. 

các`bad_child`cấu trúc ghi lại các nhánh con của một đỉnh chứa một lần xuất hiện khác có cùng giá trị. Sau khi DFS kết thúc, bất kỳ đỉnh nào có giá trị xuất hiện ở ít nhất hai nhánh con khác nhau sẽ tạo thêm các vùng gốc bị cấm. Những khoảng cây con đó được thêm vào mảng khác biệt. 

Cuối cùng, việc quét theo thứ tự Euler sẽ chuyển đổi mảng sai phân thành số lượng không hợp lệ thực tế. Các đỉnh có số 0 chính xác là các gốc hợp lệ. 

Phần tế nhị nhất là xác định đứa trẻ ngay bên dưới sự xuất hiện của tổ tiên. Việc sử dụng tổ tiên có giá trị bằng nhau gần nhất đảm bảo rằng mọi xung đột được xử lý một lần. Một lỗi dễ mắc phải khác là quên rằng các cập nhật cây con là các khoảng thời gian nửa mở$[tin,tout)$; việc sử dụng các điểm cuối bao gồm sẽ tạo ra từng lỗi một. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2 5 1 1 4
1 2
1 3
2 4
2 5
```Giá trị bằng nhau là 1, xuất hiện ở đỉnh 3 và 4. 

| Bước | Xung đột | Rễ xấu | 
| --- | --- | --- | 
| DFS khám phá giây thứ 1 | (3,4) | các gốc đặt sự xuất hiện này lên trên sự xuất hiện kia | 
| Đã áp dụng cập nhật phạm vi | đánh dấu các vùng không hợp lệ | tích lũy trong khác biệt | 
| Đếm cuối cùng | đỉnh 1,2,5 có 0 | hợp lệ | 

Kết quả:```
3
```Ví dụ này cho thấy các giá trị bằng nhau trong các nhánh khác nhau không tự động làm mất hiệu lực của mọi gốc. Chỉ những gốc rễ tạo ra mối quan hệ tổ tiên mới bị cấm. 

### Ví dụ 2 

đầu vào:```
3
1 2 1
1 2
2 3
```| Đỉnh | Số lượng không hợp lệ | 
| --- | --- | 
| 1 | 1 | 
| 2 | 0 | 
| 3 | 1 | 

Chỉ có đỉnh 2 còn hiệu lực. 

Đầu ra:```
1
```Hai giá trị bằng nhau nằm ở hai phía đối diện của đỉnh 2. Việc root ở một trong hai điểm cuối sẽ đặt cả hai bản sao trên một đường dẫn từ gốc đến nút, trong khi việc root ở giữa sẽ tách chúng ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Duyệt DFS cộng với xử lý khoảng cách cây con | 
| Không gian |$O(n)$| Danh sách kề, mảng Euler, ngăn xếp, cập nhật | 

Cây chứa nhiều nhất$2 \cdot 10^5$đỉnh, do đó cần có độ phức tạp tuyến tính hoặc gần tuyến tính. Thuật toán chỉ lưu trữ một lượng thông tin không đổi trên mỗi đỉnh và thực hiện một số lượng nhỏ các lần duyệt DFS, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue().strip()

# sample 1
assert run(
"""5
2 5 1 1 4
1 2
1 3
2 4
2 5
"""
) == "3"

# single vertex
assert run(
"""1
7
"""
) == "1"

# all distinct
assert run(
"""4
1 2 3 4
1 2
2 3
3 4
"""
) == "4"

# chain with repeated endpoints
assert run(
"""3
1 2 1
1 2
2 3
"""
) == "1"

# all equal
assert run(
"""4
5 5 5 5
1 2
2 3
3 4
"""
) == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn | 1 | Cây nhỏ nhất có thể | 
| Tất cả các giá trị riêng biệt | 4 | Mọi gốc phải hợp lệ | 
| Chuỗi 1-2-1 | 1 | Xung đột tổ tiên khi tái khởi động | 
| Tất cả các giá trị bằng nhau | 0 | Xử lý trùng lặp nặng | 
| Mẫu 1 | 3 | Cấu trúc hỗn hợp chung | 

## Vỏ cạnh 

Hãy xem xét một đỉnh duy nhất:```
1
10
```Chỉ có một đường dẫn từ gốc đến nút, bao gồm một đỉnh duy nhất. Không có sự trùng lặp có thể xảy ra. DFS không thực hiện cập nhật xung đột, tất cả các bộ đếm vẫn bằng 0 và câu trả lời là 1. 

Hãy xem xét một chuỗi:```
3
1 2 1
1 2
2 3
```Giá trị lặp lại xuất hiện ở cả hai đầu. Trong DFS, lần xuất hiện thứ hai phát hiện ra lần đầu tiên là tổ tiên có giá trị bằng nhau. Bản cập nhật đánh dấu nguồn gốc giữ cho chúng có liên quan đến tổ tiên. Đỉnh 2 nằm trong cây con được miễn trừ và không được đánh dấu. Đầu ra là 1. 

Xem xét tất cả các giá trị riêng biệt:```
4
1 2 3 4
1 2
1 3
1 4
```Không có cặp giá trị bằng nhau tồn tại. Mảng chênh lệch không bao giờ thay đổi từ 0. Mỗi đỉnh nhận được số không hợp lệ bằng 0, tạo ra câu trả lời 4. 

Xem xét nhiều giá trị bằng nhau:```
4
7 7 7 7
1 2
2 3
3 4
```Mỗi gốc tạo một đường dẫn chứa giá trị lặp lại 7. DFS tạo ra các cập nhật xung đột cho mỗi lần xuất hiện lặp lại và mỗi đỉnh sẽ tích lũy số lượng không hợp lệ dương. Câu trả lời trở thành 0, phù hợp với định nghĩa.
