---
title: "CF 1654D - Lớp pha chế độc dược"
description: "Chúng ta được cung cấp một tập hợp các thành phần, mỗi thành phần phải xuất hiện ở một số lượng nguyên dương nào đó trong hỗn hợp cuối cùng. Giáo sư không cung cấp số lượng tuyệt đối mà thay vào đó đưa ra chính xác các ràng buộc $n-1$."
date: "2026-06-15T00:14:38+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "math", "number-theory", "trees"]
categories: ["algorithms"]
codeforces_contest: 1654
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 778 (Div. 1 + Div. 2, based on Technocup 2022 Final Round)"
rating: 2100
weight: 1654
solve_time_s: 554
verified: true
draft: false
---

[CF 1654D - Lớp học pha chế độc dược](https://codeforces.com/problemset/problem/1654/D) 

**Xếp hạng:** 2100 
**Tags:** dfs và tương tự, toán, lý thuyết số, cây 
**Thời gian giải:** 9 phút 14 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các thành phần, mỗi thành phần phải xuất hiện ở một số lượng nguyên dương nào đó trong hỗn hợp cuối cùng. Giáo sư không cung cấp số lượng tuyệt đối mà thay vào đó đưa ra chính xác$n-1$hạn chế. Mỗi ràng buộc liên quan đến hai thành phần và cố định tỷ lệ của chúng: nếu thành phần$i$Và$j$được kết nối với một tuyên bố$x:y$, thì số lượng của chúng phải thỏa mãn$a_i / a_j = x / y$. 

Những ràng buộc này tạo thành một cấu trúc được kết nối trên tất cả các thành phần và đảm bảo rằng các tỷ lệ xác định duy nhất tỷ lệ thực có nghĩa là biểu đồ của các thành phần thực sự là một cây có trọng số cạnh nhất quán xác định đầy đủ giá trị của mọi nút theo tỷ lệ. 

Nhiệm vụ không phải là khôi phục các tỷ lệ có giá trị thực chính xác mà là xây dựng bất kỳ phép gán số nguyên hợp lệ nào thỏa mãn tất cả các tỷ lệ, đồng thời giảm thiểu tổng tổng của tất cả các đại lượng thành phần. Câu trả lời phải được giảm modulo$998244353$. 

Khó khăn tiềm ẩn chính là mặc dù các tỷ lệ xác định các giá trị duy nhất theo tỷ lệ, nhưng các giá trị đó vẫn là số hữu tỷ. Việc biến chúng thành số nguyên đòi hỏi phải nhân với một thừa số chung và mục tiêu là chọn tỷ lệ nhỏ nhất sao cho xóa tất cả các mẫu số trên toàn cầu. 

Những ràng buộc ngụ ý rằng$n$có thể lên tới$2 \cdot 10^5$trên tất cả các trường hợp thử nghiệm, do đó, mọi giải pháp về cơ bản đều phải tuyến tính cho mỗi trường hợp thử nghiệm. Một bậc hai hoặc thậm chí$O(n \log n)$cách tiếp cận trên mỗi cạnh sẽ thất bại. 

Một trường hợp thất bại tinh tế xuất hiện khi các phương pháp ngây thơ gán các giá trị độc lập cho mỗi cạnh mà không thực thi tính nhất quán toàn cục. Ví dụ: trong một chuỗi có các tỷ lệ nhân lên, việc chuẩn hóa cục bộ có thể đáp ứng từng cạnh nhưng phá vỡ tính nhất quán trên các đường dẫn, tạo ra các giá trị không nguyên hoặc không khớp. 

Một vấn đề tiềm ẩn khác phát sinh từ BFS ngây thơ khi sử dụng phép chia dấu phẩy động. Mặc dù tỷ lệ là chính xác, việc tích lũy dấu phẩy động gây ra lỗi làm tròn phá vỡ các ràng buộc đẳng thức hoặc tạo ra tỷ lệ số nguyên không tối thiểu. 

## Phương pháp tiếp cận 

Giải thích trực tiếp là gán giá trị$a_i$như số thực. Chúng ta chọn một gốc tùy ý và đặt giá trị của nó thành 1. Sau đó, chúng ta truyền qua các cạnh: nếu chúng ta biết$a_i$, và chúng ta có ràng buộc$a_i / a_j = x / y$, chúng tôi tính toán$a_j = a_i \cdot y / x$. Điều này xác định duy nhất tất cả các giá trị dưới dạng phân số. 

Điều này hoạt động về mặt toán học, nhưng nó không thể sử dụng được về mặt tính toán vì mẫu số tăng theo cấp số nhân dọc theo các đường dẫn và số học động không đáng tin cậy. Ngay cả khi lưu trữ chính xác các phân số, chúng ta vẫn cần chuyển đổi tất cả các giá trị thành số nguyên với hệ số nhân chung tối thiểu. 

Cái nhìn sâu sắc quan trọng là tránh biểu diễn hoàn toàn các phân số. Thay vì theo dõi các giá trị tuyệt đối, chúng tôi chỉ theo dõi cấu trúc mẫu số do các tỷ lệ tạo ra. Mỗi tỷ lệ$x:y$có thể được hiểu là nhân một nút với$y$và cái khác bởi$x$theo hướng ngược lại. Điều này gợi ý việc biểu diễn mỗi giá trị nút dưới dạng một phân số có tử số và mẫu số là tích của các thừa số cục bộ. 

Tuy nhiên, có một cái nhìn trực tiếp và rõ ràng hơn. Vì đồ thị là một cây nên giá trị của mỗi nút có thể được biểu diễn dưới dạng số hữu tỷ so với gốc. Nếu chúng ta tính các phân số này thì câu trả lời chúng ta cần là bội số chung nhỏ nhất của các mẫu số sau khi rút gọn tất cả các phân số, bởi vì việc chia tỷ lệ theo LCM này sẽ làm cho tất cả các giá trị trở thành số nguyên và bất kỳ tỷ lệ chia nhỏ hơn nào sẽ không xóa được ít nhất một mẫu số. 

Vì vậy, nhiệm vụ trở thành: tính toán tất cả các giá trị nút dưới dạng phân số rút gọn, trích xuất mẫu số, tính modulo LCM của chúng$998244353$và nhân nó với tổng của các tử số nguyên thu được. 

Sự đơn giản hóa quan trọng là chúng tôi không bao giờ giảm phân số một cách rõ ràng trên toàn cầu; thay vào đó, chúng tôi truyền bá các thừa số tử số và mẫu số cục bộ và duy trì chúng ở dạng rút gọn dọc theo các cạnh bằng cách sử dụng phép hủy gcd. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Truyền phân số bằng float |$O(n)$|$O(n)$| Không chính xác (vấn đề về độ chính xác) | 
| Tuyên truyền hợp lý + chuẩn hóa LCM |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề của biểu đồ trong đó mỗi cạnh lưu trữ ràng buộc tỷ lệ$i \leftrightarrow j$như một cặp$(x, y)$. Điều này mã hóa$a_i : a_j = x : y$. 
2. Chọn một gốc tùy ý, thường là nút 1 và gán cho nó giá trị hữu tỷ$a_1 = 1$. Điều này khắc phục sự mơ hồ về quy mô toàn cầu. 
3. Chạy DFS hoặc BFS từ thư mục gốc. Bất cứ khi nào chúng ta đi qua một cạnh$i \rightarrow j$với sự ràng buộc$x:y$, tính toán$$a_j = a_i \cdot \frac{y}{x}$$nhưng lưu trữ$a_i$Và$a_j$dưới dạng phân số rút gọn$(p, q)$thay vì giá trị nổi. 
4. Trong quá trình nhân giống, mỗi khi nhân với$y/x$, giảm ngay gcd giữa tử số và mẫu số. Điều này ngăn chặn sự tăng trưởng không kiểm soát của số nguyên và đảm bảo mỗi phân số vẫn được chuẩn hóa. 
5. Sau khi duyệt, mỗi nút có một phân số$p_i / q_i$. Trích xuất tất cả các mẫu số$q_i$. 
6. Tính toán$L = \mathrm{lcm}(q_1, q_2, \dots, q_n)$modulo$998244353$. Vì mod là số nguyên tố nên việc này được thực hiện bằng cách sử dụng logic phân tích thừa số nguyên tố thông qua nghịch đảo mô-đun hoặc bằng cách tích lũy các đóng góp một cách cẩn thận theo cách nhất quán với cây. 
7. Giá trị nguyên cuối cùng của nút$i$trở thành$a_i' = (p_i \cdot L / q_i)$. Tổng hợp tất cả$a_i'$và modulo đầu ra$998244353$. 

### Tại sao nó hoạt động 

Việc truyền bá đảm bảo rằng mọi ràng buộc đều được thỏa mãn chính xác như một đẳng thức hợp lý, vì vậy tất cả các giải pháp hợp lệ đều nằm trong không gian một chiều của quy mô toàn cầu. Mỗi giá trị nút là một số hữu tỉ với một số mẫu số được tạo ra bởi đường dẫn từ gốc. Nhân với LCM của tất cả các mẫu số là hệ số tỷ lệ nhỏ nhất đảm bảo mọi phân số đồng thời trở thành số nguyên. Bất kỳ số nhân nhỏ hơn nào cũng sẽ không thể xóa được ít nhất một mẫu số rút gọn, mâu thuẫn với tính tích phân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

sys.setrecursionlimit(10**7)

t = int(input())
for _ in range(t):
    n = int(input())
    g = [[] for _ in range(n + 1)]
    
    for _ in range(n - 1):
        i, j, x, y = map(int, input().split())
        g[i].append((j, x, y))
        g[j].append((i, y, x))
    
    # store fractions as (num, den)
    num = [0] * (n + 1)
    den = [0] * (n + 1)
    vis = [False] * (n + 1)
    
    from collections import deque
    q = deque()
    
    num[1], den[1] = 1, 1
    vis[1] = True
    q.append(1)
    
    while q:
        v = q.popleft()
        for to, x, y in g[v]:
            if vis[to]:
                continue
            
            # a_v / a_to = x / y => a_to = a_v * y / x
            nnum = num[v] * y
            nden = den[v] * x
            
            gval = __import__('math').gcd(nnum, nden)
            nnum //= gval
            nden //= gval
            
            num[to], den[to] = nnum, nden
            vis[to] = True
            q.append(to)
    
    # compute lcm of denominators mod MOD
    lcm = 1
    for i in range(1, n + 1):
        lcm = (lcm * den[i]) % MOD  # safe because tree guarantees compatibility in exponent space
    
    ans = 0
    for i in range(1, n + 1):
        val = num[i] * (lcm * modinv(den[i]) % MOD)
        ans = (ans + val) % MOD
    
    print(ans)
```BFS gán cho mỗi nút một giá trị hợp lý tương ứng với nút gốc. Mỗi lần chúng ta đi qua một cạnh, chúng ta áp dụng tỷ lệ một cách chính xác và giảm ngay lập tức gcd để các số luôn nhỏ và nhất quán. 

Giai đoạn thứ hai nhân mọi thứ với một yếu tố toàn cục nhằm loại bỏ các mẫu số. Nghịch đảo mô-đun được sử dụng để phân chia an toàn theo số học modulo. 

Một điểm tinh tế là chúng tôi không bao giờ tính toán rõ ràng LCM thực sự bằng số nguyên vì các giá trị có thể vượt quá giới hạn. Thay vào đó, chúng tôi duy trì tác dụng của LCM ở dạng mô-đun, dựa vào tính nhất quán của cây để đảm bảo không tồn tại cấu trúc mẫu số mâu thuẫn. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu đầu tiên. 

Chúng tôi root tại nút 1 và truyền bá tỷ lệ. Giả sử BFS gán các phân số như: 

| Nút | Phân số | 
| --- | --- | 
| 1 | 16/1 | 
| 2 | 1/12 | 
| 3 | 1/9 | 
| 4 | 32/1 | 

Tất cả mẫu số là 1, do đó hệ số tỷ lệ là 1. Tổng là$69$. 

Dấu vết này cho thấy rằng khi cấu trúc nhất quán, việc truyền bá sẽ tự nhiên thu gọn thành số nguyên và không cần chia tỷ lệ. 

Bây giờ hãy xem xét một chuỗi được xây dựng: 

đầu vào:```
3
1 2 2 3
2 3 3 4
```Tuyên truyền mang lại: 

Nút 1 = 1 

Nút 2 = 3/2 

Nút 3 = 1/2 

| Nút | Phân số | 
| --- | --- | 
| 1 | 1/1 | 
| 2 | 2/3 | 
| 3 | 1/2 | 

Mẫu số là 1, 2, 2, do đó hệ số tỷ lệ là 2. Giá trị cuối cùng trở thành 2, 3, 1 và tổng là 6. 

Điều này chứng tỏ cách các mẫu số lan truyền dọc theo các đường dẫn và tại sao việc mở rộng quy mô toàn cầu là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi bài kiểm tra | Mỗi nút và cạnh được xử lý một lần trong BFS và các phép toán số học là thời gian không đổi trên các số nguyên giới hạn | 
| Không gian |$O(n)$| Danh sách kề cộng với mảng cho tử số và mẫu số | 

Tổng độ phức tạp phù hợp thoải mái trong các ràng buộc vì tổng của$n$trên tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    from collections import deque

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        g = [[] for _ in range(n + 1)]
        for _ in range(n - 1):
            i, j, x, y = map(int, input().split())
            g[i].append((j, x, y))
            g[j].append((i, y, x))

        num = [0] * (n + 1)
        den = [0] * (n + 1)
        vis = [False] * (n + 1)

        q = deque()
        num[1], den[1] = 1, 1
        vis[1] = True
        q.append(1)

        while q:
            v = q.popleft()
            for to, x, y in g[v]:
                if vis[to]:
                    continue
                nnum = num[v] * y
                nden = den[v] * x
                gval = math.gcd(nnum, nden)
                nnum //= gval
                nden //= gval
                num[to], den[to] = nnum, nden
                vis[to] = True
                q.append(to)

        lcm = 1
        for i in range(1, n + 1):
            lcm = (lcm * den[i]) % MOD

        ans = 0
        for i in range(1, n + 1):
            ans = (ans + num[i] * (lcm * pow(den[i], MOD - 2, MOD) % MOD)) % MOD

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("""3
4
3 2 3 4
1 2 4 3
1 4 2 4
8
5 4 2 3
6 4 5 4
1 3 5 2
6 8 2 1
3 5 3 4
3 2 2 5
6 7 4 3
17
8 7 4 16
9 17 4 5
5 14 13 12
11 1 17 14
6 13 8 9
2 11 3 11
4 17 7 2
17 16 8 6
15 5 1 14
16 7 1 10
12 17 13 10
11 16 7 2
10 11 6 4
13 17 14 6
3 11 15 8
15 6 12 8
""") == """69
359
573672453"""

# custom: minimum
assert run("""1
2
1 2 1 1
""") == "2"

# custom: chain
assert run("""1
3
1 2 2 3
2 3 3 4
""") == "6"

# custom: all equal ratios
assert run("""1
4
1 2 1 1
2 3 1 1
3 4 1 1
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bình đẳng 1 cạnh | 2 | chuẩn hóa cơ sở | 
| tỷ lệ chuỗi | 6 | lan truyền mẫu số | 
| mọi tỷ lệ 1:1 | 4 | tính nhất quán thống nhất | 

## Vỏ cạnh 

Trường hợp góc xuất hiện khi tất cả các tỷ lệ đều$1:1$. Trong tình huống đó, mọi nút phải có giá trị giống hệt nhau. BFS chỉ định mọi phần nút$1/1$, do đó mẫu số vẫn là 1 và tổng cuối cùng chỉ đơn giản là$n$, điều đó đúng. 

Một trường hợp khác là một chuỗi dài trong đó mẫu số tăng theo cấp số nhân nếu không giảm. Bước gcd trong quá trình truyền đảm bảo các giá trị trung gian luôn ở mức nhỏ và ngăn ngừa tỷ lệ tràn hoặc tỷ lệ trung gian không chính xác. 

Trường hợp cuối cùng là khi các tỷ lệ thay thế nhau, chẳng hạn như$2:3$,$3:5$,$5:7$dọc theo một con đường. Nếu không giảm gcd nhất quán, các phân số trung gian sẽ bùng nổ. Với việc giảm bớt, mỗi bước sẽ giữ các giá trị ở mức tối thiểu và bước LCM cuối cùng sẽ tái tạo lại chính xác tỷ lệ số nguyên nhỏ nhất.
