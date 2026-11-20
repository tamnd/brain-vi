---
title: "CF 2184F - Cây Anh Đào"
description: "Chúng ta có một cây có rễ, mỗi lá chứa đúng một quả anh đào. Một cú rung được thực hiện ở đỉnh v làm cho quả anh đào rơi từ mọi lá bên trong cây con của v. Một khi quả anh đào đã rơi, nó không bao giờ bị ảnh hưởng bởi một lần rung khác."
date: "2026-06-07T21:40:27+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 2184
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1072 (Div. 3)"
rating: 1900
weight: 2184
solve_time_s: 224
verified: true
draft: false
---

[CF 2184F - Cây anh đào](https://codeforces.com/problemset/problem/2184/F) 

**Xếp hạng:** 1900 
**Thẻ:** dfs và tương tự, dp, đồ thị, cây 
**Thời gian giải:** 3 phút 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có rễ, mỗi lá chứa đúng một quả anh đào. 

Một cú lắc được thực hiện ở đỉnh`v`làm cho quả anh đào rơi từ mỗi chiếc lá bên trong cây con của`v`. Một khi quả anh đào đã rụng, nó sẽ không bao giờ bị ảnh hưởng bởi một cú rung lắc nào nữa. Nếu bất kỳ lá nào thuộc về hai lần lắc khác nhau thì quá trình này không hợp lệ. 

Mục tiêu là thu thập từng quả anh đào và sử dụng số lần lắc chia hết cho ba. 

Cây chứa tới`2·10^5`đỉnh trên tất cả các trường hợp thử nghiệm. Bất kỳ thuật toán nào quét liên tục các cây con hoặc thử nhiều tổ hợp đỉnh sẽ ngay lập tức bị loại trừ. Với kích thước đầu vào này, chúng ta cần thứ gì đó gần với thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

Phần khó khăn là hiểu được tập hợp đỉnh nào có thể bị rung chuyển đồng thời. 

Giả sử chúng ta lắc một đỉnh và cũng lắc một trong các con cháu của nó. Sau đó, mỗi lá trong cây con của con cháu bị ảnh hưởng hai lần, một lần bởi tổ tiên và một lần bởi con cháu. Điều đó bị cấm. 

Kết quả là, các đỉnh được chọn phải tạo thành một phản chuỗi đối với mối quan hệ tổ tiên. Mỗi lá phải thuộc về đúng một cây con được chọn. 

Một số trường hợp đặc biệt rất dễ bị hiểu sai. 

Hãy xem xét một cái cây chỉ có hai lá:```
1
├─2
└─3
```Những cách hợp lệ duy nhất để che tất cả các lá là:```
{1}      -> 1 shake
{2,3}    -> 2 shakes
```Cả hai số đều không chia hết cho ba nên đáp án là`NO`. 

Một trường hợp quan trọng khác là cây con một lá.```
1
└─2
```Chiếc lá phải được che phủ bằng cách nào đó. Hoặc chúng ta lắc đỉnh`1`hoặc đỉnh`2`. Chúng ta không thể để cây con không được che phủ và hy vọng rằng một số anh chị em sẽ đóng góp, vì mỗi lá riêng lẻ đều cần có lớp phủ. 

Trường hợp tinh tế thứ ba là khi một số cây con có nhiều số lần lắc hợp lệ. Chúng tôi không quan tâm đến con số chính xác. Vì chỉ chia hết cho ba chất nên giữ số đếm theo modulo ba là đủ. Việc thiếu quan sát này sẽ dẫn đến sự bùng nổ về số lượng trạng thái. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi tập hợp con của các đỉnh, kiểm tra xem các đỉnh được chọn có tạo thành một lớp phủ hợp lệ cho tất cả các lá hay không, sau đó kiểm tra xem số đỉnh được chọn có chia hết cho ba hay không. 

Điều này đúng nhưng hoàn toàn không thực tế. Một cái cây với`n = 2·10^5`đỉnh có`2^n`tập hợp con, vượt xa mọi thứ có thể được xử lý. 

Cấu trúc của các giải pháp hợp lệ sẽ đưa ra hướng đi tốt hơn nhiều. 

Tập trung vào một cây con có gốc duy nhất. 

Đối với một đỉnh`v`, chỉ có hai khả năng. 

Hoặc chúng ta lắc`v`chính nó. Sau đó, mọi lá trong cây con của nó sẽ bị che phủ ngay lập tức và không con cháu nào có thể bị lay chuyển. 

Hoặc chúng ta không lắc`v`. Sau đó, mỗi cây con phải bao phủ tất cả các lá của nó một cách độc lập. 

Điều này tự nhiên tạo ra một cây DP. 

Cho phép`D[v]`là tập hợp các số lần rung có thể có theo modulo ba có thể bao phủ tất cả các lá bên trong cây con của`v`. 

Đối với một chiếc lá, khả năng duy nhất là lắc chiếc lá đó để đếm`1`. 

Đối với một đỉnh bên trong: 

1. Chúng ta có thể rung chuyển`v`, số lượng đóng góp`1`. 
2. Chúng tôi có thể bỏ qua`v`, trong trường hợp đó mỗi đứa trẻ chọn một số dư hợp lệ và các số dư đó được cộng theo modulo ba. 

Vì chỉ có ba dư lượng nên mỗi trạng thái chứa tối đa ba giá trị. Việc kết hợp các phần tử con sẽ trở thành một tích chập modulo-ba nhỏ. 

Cách tiếp cận brute-force theo dõi các tập hợp con chính xác của các đỉnh. Quan sát quan trọng là chỉ có số modulo ba quan trọng và mọi cây con chỉ tương tác với phần còn lại của cây thông qua phần dư đó. Điều này thu gọn việc tìm kiếm hàm mũ thành cây tuyến tính DP. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| DP tối ưu trên cây | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gốc cây tại đỉnh`1`. 
2. Xử lý các đỉnh theo thứ tự sau sao cho mọi nút con đều được tính trước nút cha của nó. 
3. Đối với đỉnh lá, đặt```
D[v] = {1}
```Chiếc lá phải được lắc nếu không có tổ tiên nào ở cùng trạng thái DP được chọn. 
4. Đối với một đỉnh trong, trước tiên hãy tính trường hợp trong đó`v`không bị rung chuyển. 

Bắt đầu với dư lượng`{0}`. 
5. Đối với mỗi trẻ, kết hợp bộ dư lượng hiện tại với bộ dư lượng của trẻ. 

Nếu dư lượng`a`đã có thể đạt được và dư lượng`b`có thể đạt được ở trẻ, sau đó dư lượng`(a + b) mod 3`trở nên có thể đạt được sau khi xử lý đứa trẻ đó. 
6. Sau khi tất cả các phần tử con được xử lý, tập kết quả đại diện cho mọi phần dư có thể đạt được khi`v`bản thân nó không bị rung chuyển. 
7. Thêm phương án thay thế vào đâu`v`bị rung chuyển. 

Lắc`v`đóng góp chính xác một lần lắc, do đó cặn`1`luôn luôn có thể đạt được. 
8. Cửa hàng```
D[v] = D_not_choose ∪ {1}
```9. Sau khi tính toán gốc, kiểm tra xem phần dư có`0`thuộc về`D[1]`. 

Nếu đúng như vậy, một số lớp phủ hợp lệ sử dụng số lần lắc chia hết cho ba, vì vậy hãy in`"YES"`. Nếu không thì in`"NO"`. 

### Tại sao nó hoạt động 

Với mỗi cây con,`D[v]`chứa chính xác phần dư của tất cả các cách hợp lệ để bao phủ mọi lá bên trong cây con đó mà không sử dụng bất kỳ tổ tiên nào của`v`. 

Định nghĩa đã hoàn tất vì mọi giải pháp hợp lệ cho cây con đều rơi vào đúng một trong hai loại:`v`được chọn hoặc không. 

Nếu như`v`được chọn, toàn bộ cây con sẽ được bao phủ bởi một lần rung. Nếu như`v`không được chọn, mỗi cây con con phải giải quyết cùng một vấn đề một cách độc lập và tổng số lần lắc là tổng số lần đếm con. 

Không có giải pháp hợp lệ nào bị bỏ qua và không có giải pháp không hợp lệ nào được đưa ra. Bằng cách quy nạp kích thước cây con, mọi dư lượng được tính toán đều có thể đạt được và mọi dư lượng có thể đạt được đều được tính toán. Như vậy dư lượng`0`ở gốc tương đương với sự tồn tại của một giải pháp toàn cục hợp lệ có số lần lắc là bội số của ba. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())

        g = [[] for _ in range(n)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            g[u].append(v)
            g[v].append(u)

        parent = [-1] * n
        order = [0]
        parent[0] = 0

        for v in order:
            for to in g[v]:
                if parent[to] == -1:
                    parent[to] = v
                    order.append(to)

        dp = [0] * n

        for v in reversed(order):
            children = [to for to in g[v] if to != parent[v]]

            if not children:
                dp[v] = 1 << 1
                continue

            cur = 1 << 0

            for ch in children:
                nxt = 0
                child_mask = dp[ch]

                for r1 in range(3):
                    if not (cur & (1 << r1)):
                        continue

                    for r2 in range(3):
                        if child_mask & (1 << r2):
                            nxt |= 1 << ((r1 + r2) % 3)

                cur = nxt

            dp[v] = cur | (1 << 1)

        ans.append("YES" if (dp[0] & 1) else "NO")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```DFS được triển khai lặp đi lặp lại để tránh giới hạn đệ quy Python trên cây sâu. 

Mỗi trạng thái DP được lưu trữ dưới dạng mặt nạ ba bit. Chút`r`được thiết lập nếu dư lượng`r`modulo ba là có thể đạt được. 

Ví dụ:```
mask = 0b101
```có nghĩa là dư lượng`0`Và`2`là có thể. 

Bước kết hợp con là một tích chập modulo-ba. Vì chỉ có ba phần dư nên công của mỗi đứa trẻ là không đổi. 

Trường hợp lá xứng đáng được chú ý. Một lá không có con, nhưng cây con của nó vẫn chứa một quả anh đào. Hành động cục bộ hợp lệ duy nhất là lắc chiếc lá, vì vậy trạng thái của nó chính xác là`{1}`. 

Câu trả lời gốc kiểm tra xem dư lượng`0`đang có mặt. Điều đó tương ứng với tổng số lần lắc chia hết cho ba. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
1 2
1 3
1 4
```Rễ có ba lá con. 

| Đỉnh | Dư lượng trẻ em | Dư lượng không được chọn | Dư lượng cuối cùng | 
| --- | --- | --- | --- | 
| 2 | - | - | {1} | 
| 3 | - | - | {1} | 
| 4 | - | - | {1} | 
| 1 | {1},{1},{1} | {0} | {0,1} | 

Dư lượng`0`có thể đạt được ở gốc, vì vậy câu trả lời là`YES`. 

Điều này tương ứng với các đỉnh rung`2`,`3`, Và`4`, sử dụng đúng ba lần lắc. 

### Mẫu 2 

đầu vào:```
3
1 2
1 3
```| Đỉnh | Dư lượng trẻ em | Dư lượng không được chọn | Dư lượng cuối cùng | 
| --- | --- | --- | --- | 
| 2 | - | - | {1} | 
| 3 | - | - | {1} | 
| 1 | {1},{1} | {2} | {1,2} | 

Dư lượng`0`không xuất hiện. 

Số lần lắc có thể là`1`Và`2`, không chia hết cho 3 nên đáp án là`NO`. 

Ví dụ này cho thấy tại sao chỉ đếm lá là không đủ. Bản thân gốc có thể bị rung chuyển, thay đổi số đếm từ`2`ĐẾN`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cạnh được xử lý với số lần không đổi và mỗi lần hợp nhất DP chỉ sử dụng chuyển tiếp dư lượng 3 × 3 | 
| Không gian | O(n) | Danh sách kề, mảng cha, thứ tự truyền tải và mặt nạ DP | 

Tổng số đỉnh của tất cả các trường hợp thử nghiệm nhiều nhất là`2·10^5`, do đó, một giải pháp tuyến tính dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline
        t = int(input())
        out = []

        for _ in range(t):
            n = int(input())
            g = [[] for _ in range(n)]

            for _ in range(n - 1):
                u, v = map(int, input().split())
                u -= 1
                v -= 1
                g[u].append(v)
                g[v].append(u)

            parent = [-1] * n
            order = [0]
            parent[0] = 0

            for v in order:
                for to in g[v]:
                    if parent[to] == -1:
                        parent[to] = v
                        order.append(to)

            dp = [0] * n

            for v in reversed(order):
                children = [to for to in g[v] if to != parent[v]]

                if not children:
                    dp[v] = 1 << 1
                    continue

                cur = 1

                for ch in children:
                    nxt = 0
                    for a in range(3):
                        if cur & (1 << a):
                            for b in range(3):
                                if dp[ch] & (1 << b):
                                    nxt |= 1 << ((a + b) % 3)
                    cur = nxt

                dp[v] = cur | (1 << 1)

            out.append("YES" if dp[0] & 1 else "NO")

        return "\n".join(out)

    return solve()

# provided sample
assert run("""3
4
1 2
1 3
1 4
3
1 2
1 3
9
1 2
3 1
2 4
5 2
5 6
3 7
8 3
8 9
""") == """YES
NO
YES"""

# minimum tree
assert run("""1
2
1 2
""") == "NO"

# star with 3 leaves
assert run("""1
4
1 2
1 3
1 4
""") == "YES"

# chain of length 4, shaking root only
assert run("""1
4
1 2
2 3
3 4
""") == "NO"

# root with 6 leaf children
assert run("""1
7
1 2
1 3
1 4
1 5
1 6
1 7
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây một cạnh | KHÔNG | Trường hợp kích thước tối thiểu | 
| Rễ 3 lá | CÓ | Có thể chính xác ba lần lắc | 
| Chuỗi dài | KHÔNG | Chỉ có một lá tồn tại, số lượng là |
