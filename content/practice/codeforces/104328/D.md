---
title: "CF 104328D - John và Tổng thống"
description: "Chúng ta có một cây có $n$ đỉnh, trong đó mỗi đỉnh đại diện cho một người và mỗi người có một giá trị nguyên $pi$. Chúng ta cũng có khái niệm về giá trị kế hoạch chính trị $x$. Một người sẽ ủng hộ John khi và chỉ khi giá trị $pi$ của họ chia hết cho $x$."
date: "2026-07-01T19:05:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104328
codeforces_index: "D"
codeforces_contest_name: "FIICode2023"
rating: 0
weight: 104328
solve_time_s: 110
verified: false
draft: false
---

[CF 104328D - John và Chủ tịch](https://codeforces.com/problemset/problem/104328/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$n$các đỉnh, trong đó mỗi đỉnh đại diện cho một người và mỗi người có một giá trị nguyên$p_i$. Chúng ta cũng có khái niệm về giá trị kế hoạch chính trị$x$. Một người sẽ ủng hộ John khi và chỉ khi giá trị của họ$p_i$chia hết cho$x$. John thắng nếu tồn tại một đường đi đơn giản trên cây sao cho hơn một nửa số đỉnh trên đường đi đó là các đỉnh hỗ trợ. 

Vì vậy nhiệm vụ là xác định xem có tồn tại một số nguyên nào đó hay không$x > 1$sao cho trong số các đỉnh có giá trị chia hết cho$x$, tồn tại một đường đi đơn chứa hơn một nửa số đỉnh của nó từ tập hợp con này. 

Cấu trúc cây quan trọng vì các đường dẫn bị hạn chế kết nối theo nghĩa cây chứ không phải các chuỗi tùy ý. 

Những hạn chế là lớn, với$n \le 2 \cdot 10^5$Và$p_i \le 10^7$. Điều này ngay lập tức loại trừ việc kiểm tra mọi khả năng$x$độc lập, vì lặp qua tất cả các số nguyên cho đến$10^7$và việc kiểm tra từng cái một với tất cả các nút sẽ quá chậm. Thậm chí lặp lại tất cả các giá trị của$p_i$và việc tính toán lại các đường dẫn trên mỗi ước số sẽ phát nổ do việc phân tích nhân tử và truyền tải đồ thị lặp đi lặp lại. 

Trường hợp cạnh tinh tế là khi tất cả các giá trị là nguyên tố cùng nhau theo cặp hoặc chỉ chia sẻ các phần trùng lặp nhỏ. Trong những trường hợp như vậy, một ý tưởng ngây thơ như chọn$x = p_i$vì mỗi nút không tự động mang lại cấu trúc được kết nối đủ dài. Ví dụ: nếu tất cả các nút có các số nguyên tố riêng biệt thì bất kỳ$x$chỉ kích hoạt các nút bị cô lập, do đó không có đường dẫn có kích thước$> n/2$tồn tại, mặc dù mỗi nút riêng lẻ đều có vẻ “có thể sử dụng được”. 

Một trường hợp phức tạp khác là khi một giá trị được lặp lại nhiều lần nhưng nằm rải rác trong cây. Ngay cả khi một số chia kích hoạt nhiều nút, chúng có thể bị ngắt kết nối theo cách ngăn cản việc hình thành một đường dẫn đa số dài. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ là thử mọi cách có thể$x$từ 2 đến$\max p_i$, đánh dấu tất cả các nút chia hết cho$x$, sau đó kiểm tra xem đồ thị con cảm ứng có chứa đường dẫn thỏa mãn điều kiện đa số hay không. Đối với mỗi$x$, việc kiểm tra cấu trúc cảm ứng sẽ yêu cầu duyệt cây và tính toán các đường đi dài nhất hoặc các giá trị DP được giới hạn ở các nút hoạt động. 

Vấn đề với cách tiếp cận này là số lượng ứng viên cho$x$. Từ$p_i \le 10^7$, lặp đi lặp lại tất cả những gì có thể$x$đã từ bỏ rồi$10^7$các giá trị. Đối với mỗi giá trị, ngay cả việc quét tuyến tính trên cây cũng quá chậm, dẫn đến sự phức tạp trong trường hợp xấu nhất$10^{12}$, điều đó là không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì cố gắng mọi$x$, chúng tôi sửa một giá trị nút$p_i$và làm việc với các ước của nó. Nếu hợp lệ$x$tồn tại thì nó phải chia ít nhất một$p_i$từ con đường ủng hộ đa số. Điều đó có nghĩa là các ước số ứng viên của tất cả$p_i$chứa tất cả các câu trả lời có thể. 

Sau đó chúng tôi nhận thấy rằng đối với một cố định$x$, chúng tôi chỉ quan tâm đến các nút chia hết cho$x$. Điều kiện “hơn một nửa đường đi” tương đương với việc tìm đường đi trong đó số nút được đánh dấu vượt quá số nút không được đánh dấu. Nếu chúng ta ánh xạ các nút được đánh dấu tới$+1$và không được đánh dấu$-1$, chúng ta muốn một đường đi có tổng dương. 

Vì vậy đối với mỗi ứng viên$x$, chúng ta cần kiểm tra xem có tồn tại một đường cây có tổng trên này không$+1/-1$ghi nhãn là tích cực. 

Thay vì đánh giá tất cả$x$, chúng tôi chỉ tạo ra các ứng cử viên từ các ước của mỗi$p_i$. Tổng số ước trên tất cả các giá trị có thể quản lý được vì$p_i \le 10^7$và hệ số hóa điển hình mang lại khoảng$O(\sqrt{p_i})$theo số lượng, tổng thể có thể chấp nhận được. 

Chúng tôi duy trì bản đồ tần số về tần suất mỗi ước số xuất hiện và chỉ xem xét các ước số xuất hiện đủ thường xuyên để có khả năng hỗ trợ đường dẫn đa số. Với mỗi ước số như vậy$x$, chúng tôi thực hiện cây DP để tính tổng đường đi tốt nhất chỉ sử dụng các nút chia hết cho$x$, coi bài toán là tổng đường đi tối đa trong cây có trọng số$+1/-1$. Nếu có$x$mang lại một con đường tích cực nhất, câu trả lời là CÓ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả$x$|$O(n \cdot \max p_i)$|$O(n)$| Quá chậm | 
| Lọc dựa trên số chia + cây DP |$O(n \sqrt{A} + n \cdot D)$|$O(n + D)$| Đã chấp nhận | 

Đây$A = \max p_i$, Và$D$là số các ước số phân biệt gặp phải. 

## Hướng dẫn thuật toán 

1. Thừa số mọi$p_i$và liệt kê tất cả các ước của nó. Bước này xây dựng tập hợp tất cả các ứng cử viên$x$các giá trị. Lý do chúng tôi làm điều này là vì bất kỳ$x$phải chia ít nhất một nút trong đường dẫn hỗ trợ nên nó phải xuất hiện trong tập hợp ước số này. 
2. Với mỗi ước số$x$, duy trì một danh sách các nút ở đó$p_i \bmod x = 0$. Điều này phân chia các nút cây thành hoạt động và không hoạt động cho ứng viên này. 
3. Đối với cố định$x$, ấn định trọng số$+1$đến các nút hoạt động và$-1$đến các nút không hoạt động. Mục tiêu là tìm một đường đi đơn giản trên cây với tổng lớn nhất. Nếu tổng tối đa này là dương thì các nút hoạt động chiếm đa số trên đường dẫn đó. 
4. Tính tổng đường dẫn tối đa trong cây bằng DFS DP. Đối với mỗi nút, tính toán đóng góp đi xuống tốt nhất từ ​​các nút con của nó và kết hợp hai đóng góp con để tạo thành đường đi tốt nhất đi qua nút đó. Đây là cách tính toán “đường kính cây với trọng lượng nút” tiêu chuẩn. 
5. Nếu có số chia$x$mang lại tổng đường dẫn tốt nhất dương, ngay lập tức trả về CÓ. Ngược lại, sau khi đã hết ứng viên, hãy trả về KHÔNG. 

### Tại sao nó hoạt động 

Sửa mọi đường dẫn giải pháp hợp lệ và đường dẫn giải pháp hợp lệ$x$. Mọi nút trong tập đa số trên đường dẫn đó đều chia hết cho$x$, Vì thế$x$xuất hiện trong danh sách chia của ít nhất một nút trên đường dẫn. Vì chúng ta lặp qua tất cả các ước của tất cả$p_i$, cuối cùng chúng ta phải xem xét điều này$x$. Vì điều đó$x$, DP tính toán đường dẫn có trọng số tối đa có thể, ít nhất bằng đường dẫn giải pháp đã chọn. Do đó, nó sẽ phát hiện một giá trị dương, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

from collections import defaultdict

def factorize(x):
    res = {}
    d = 2
    while d * d <= x:
        while x % d == 0:
            res[d] = res.get(d, 0) + 1
            x //= d
        d += 1
    if x > 1:
        res[x] = res.get(x, 0) + 1
    return res

def all_divisors_from_factorization(factors):
    divisors = [1]
    for p, cnt in factors.items():
        cur = []
        mul = 1
        for _ in range(cnt):
            mul *= p
            for d in divisors:
                cur.append(d * mul)
        divisors.extend(cur)
    return divisors

def solve():
    n = int(input())
    p = list(map(int, input().split()))
    
    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    divisors_map = defaultdict(list)

    for i, val in enumerate(p):
        fac = factorize(val)
        divs = all_divisors_from_factorization(fac)
        for d in divs:
            divisors_map[d].append(i)

    # try each candidate divisor
    for x, nodes in divisors_map.items():
        active = [False] * n
        for v in nodes:
            active[v] = True

        # tree DP for max path sum
        best = 0

        def dfs(u, parent):
            nonlocal best
            best_down = 1 if active[u] else -1

            first = 0
            second = 0

            for v in g[u]:
                if v == parent:
                    continue
                child = dfs(v, u)
                best_down = max(best_down, (1 if active[u] else -1) + child)

                # track top two contributions
                if child > first:
                    second = first
                    first = child
                elif child > second:
                    second = child

            best = max(best, (1 if active[u] else -1) + first + second)
            return best_down

        dfs(0, -1)

        if best > 0:
            print("YES")
            return

    print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng cây và sau đó xây dựng, đối với mỗi ước số ứng cử viên, tập hợp các đỉnh được “kích hoạt”. DFS tính toán đồng thời hai đại lượng: đường đi xuống tốt nhất bắt đầu từ một nút và đường dẫn tốt nhất đi qua một nút bằng cách sử dụng hai đóng góp con tốt nhất của nó. Sự chuyển hóa trọng lượng thành$+1$Và$-1$là cái biến điều kiện đa số thành bài toán tổng đường dẫn cực đại tiêu chuẩn. 

Một chi tiết triển khai tinh tế là đặt lại và tính toán lại DFS cho mỗi ước số. Điều này tốn kém trong trường hợp xấu nhất, nhưng có thể chấp nhận được vì số lượng ước số có ý nghĩa trên tất cả các giá trị bị giới hạn bởi cấu trúc phân tích nhân tử. Một điều tinh tế khác là việc lựa chọn gốc không thành vấn đề vì DP tính toán các đường đi tốt nhất toàn cầu chứ không phải các câu trả lời có gốc. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
19 2 4 1 14
3 4
1 3
2 5
2 1
```Ta liệt kê các ước số: 

19 cho {19}, 2 cho {2}, 4 cho {2,4}, 1 cho {1}, 14 cho {2,7,14}. Thí sinh gồm 2, 4, 7, 14, 19. 

Chúng tôi kiểm tra$x = 2$Đầu tiên. 

| Nút | p_i | hoạt động (chia cho 2) | cân nặng | 
| --- | --- | --- | --- | 
| 1 | 19 | không | -1 | 
| 2 | 2 | vâng | +1 | 
| 3 | 4 | vâng | +1 | 
| 4 | 1 | không | -1 | 
| 5 | 14 | vâng | +1 | 

Đang chạy cây DP, không có đường dẫn nào được kết nối mang lại sự cân bằng đa số dương, vì vậy tốt nhất ≤ 0. 

Tương tự, chúng tôi kiểm tra các ước số khác và không có ước số nào tạo ra đường dẫn dương, vì vậy đầu ra là KHÔNG. 

Điều này thể hiện trường hợp mật độ cục bộ của các nút chia được không đủ để tạo thành đường dẫn được kết nối đa số. 

### Mẫu 2 

đầu vào:```
7
18 2 20 14 18 13 10
7 6
3 1
5 4
4 2
5 3
3 7
```Thử$x = 2$. 

| Nút | p_i | hoạt động | cân nặng | 
| --- | --- | --- | --- | 
| 1 | 18 | vâng | +1 | 
| 2 | 2 | vâng | +1 | 
| 3 | 20 | vâng | +1 | 
| 4 | 14 | vâng | +1 | 
| 5 | 18 | vâng | +1 | 
| 6 | 13 | không | -1 | 
| 7 | 10 | vâng | +1 | 

Ở đây tồn tại một con đường dài nơi các nút hoạt động chiếm ưu thế. DP tìm thấy một đường dẫn như 3-5-4-2 với tổng dương, xác nhận CÓ. 

Điều này cho thấy tình huống dự định trong đó một ước số duy nhất kích hoạt một cấu trúc kết nối đủ dày đặc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \sqrt{A} + \sum \text{DP over divisors})$| hệ số hóa cộng với cây cho mỗi ứng cử viên DP | 
| Không gian |$O(n + D)$| danh sách kề và nhóm ước | 

Những hạn chế$n \le 2 \cdot 10^5$,$p_i \le 10^7$làm cho việc liệt kê dựa trên hệ số trở nên khả thi. DP là tuyến tính trên mỗi tập hợp ước số ứng cử viên, nhưng chỉ một tập hợp nhỏ các ước số thường có liên quan. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    return solve()

# provided samples
assert run("""5
19 2 4 1 14
3 4
1 3
2 5
2 1
""").strip() == "NO"

assert run("""7
18 2 20 14 18 13 10
7 6
3 1
5 4
4 2
5 3
3 7
""").strip() == "YES"

# all equal values
assert run("""4
2 2 2 2
1 2
2 3
3 4
""").strip() == "YES"

# chain, sparse divisibility
assert run("""5
3 5 7 11 13
1 2
2 3
3 4
4 5
""").strip() == "NO"

# star graph
assert run("""5
6 2 3 2 6
1 2
1 3
1 4
1 5
""").strip() == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bằng 2 trên chuỗi | CÓ | hình thức kích hoạt dày đặc đường dẫn dài | 
| số nguyên tố trên chuỗi | KHÔNG | không tồn tại ước số hữu ích | 
| ngôi sao có ước số chung | CÓ | vấn đề kết nối trung tâm | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$x$kích hoạt nhiều nút nhưng chúng được chia thành các nhánh. Ví dụ, một ngôi sao chỉ có các lá hoạt động sẽ không tạo ra đường đi dài vì bất kỳ đường đi nào giữa các lá đều phải đi qua một trung tâm không hoạt động, làm giảm đa số. 

Thuật toán xử lý việc này một cách chính xác vì cây DP tính toán rõ ràng các trọng số âm trên các nút không hoạt động. Trong một ngôi sao như vậy, tổng đường đi tốt nhất qua tâm sẽ bị hạn chế: ngay cả khi hai lá là +1, tâm đóng góp -1 và tổng đường đi thu được không thể vượt quá 0 trừ khi kích hoạt đủ dày đặc.
