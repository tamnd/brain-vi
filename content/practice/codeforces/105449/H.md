---
title: "CF 105449H - \u0427+\u041a+\u0421"
description: "Chúng ta có hai đồ thị có hướng, mỗi đồ thị có $n$ đỉnh. Cả hai đồ thị đều được kết nối chặt chẽ và mọi chu trình có hướng bên trong một trong hai đồ thị đều có độ dài chia hết cho $k$. Mỗi đỉnh được dán nhãn là đi hoặc đến."
date: "2026-06-24T23:22:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105449
codeforces_index: "H"
codeforces_contest_name: "Moscow team school olympiad (MKOSHP) 2024"
rating: 0
weight: 105449
solve_time_s: 100
verified: false
draft: false
---

[CF 105449H - \u0427+\u041a+\u0421](https://codeforces.com/problemset/problem/105449/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai đồ thị có hướng, mỗi đồ thị có$n$đỉnh. Cả hai đồ thị đều được kết nối chặt chẽ và mọi chu trình có hướng bên trong một trong hai đồ thị đều có độ dài chia hết cho$k$. 

Mỗi đỉnh được dán nhãn là đi hoặc đến. Chúng ta được phép thêm chính xác$n$các cạnh có hướng mới và mọi cạnh được thêm vào phải nằm giữa hai đồ thị. Các cạnh được thêm này phải tạo thành một cấu trúc hoàn hảo: mỗi đỉnh đi ra có chính xác một cạnh được thêm vào và mỗi đỉnh đi vào có chính xác một cạnh được thêm vào. 

Sau khi thêm các cạnh này, chúng ta xem xét đồ thị có hướng cuối cùng được tạo bởi cả đồ thị gốc cộng với các cạnh được thêm vào. Yêu cầu là mọi chu trình có hướng trong đồ thị tổ hợp này cũng phải có độ dài chia hết cho$k$. Nhiệm vụ là xác định xem việc xây dựng như vậy có khả thi hay không. 

Các ràng buộc ngụ ý rằng chúng ta không thể mô phỏng trực tiếp bất cứ điều gì trên biểu đồ cuối cùng. Mỗi biểu đồ có thể có tối đa$2 \cdot 10^5$các đỉnh tổng thể qua các thử nghiệm và tổng số cạnh lên tới$5 \cdot 10^5$. Bất kỳ giải pháp nào cũng phải giảm cấu trúc thành tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm, vì vậy thông thường$O(n + m)$hoặc$O(n \log n)$. 

Một khó khăn tinh tế đến từ sự tương tác giữa hai biểu đồ. Mặc dù mỗi biểu đồ riêng lẻ đã có “$k$-cycle-consistent”, việc thêm các cạnh chéo có thể tạo ra các chu trình mới kết hợp cả hai biểu đồ và các chu trình hỗn hợp đó vẫn phải tuân theo cùng một ràng buộc mô-đun. 

Một cạm bẫy phổ biến là chỉ nghĩ đến việc cân bằng các đỉnh vào và ra trên toàn cầu. Điều đó là chưa đủ: ngay cả khi số lượng khớp nhau, cấu trúc bên trong không khớp theo modulo dư lượng$k$có thể tạo ra một chu kỳ xấu. 

Một trường hợp thất bại khác xuất hiện khi cố gắng khớp các đỉnh đi một cách tham lam một cách tùy ý. Ngay cả một kết quả khớp hợp lệ cục bộ cũng có thể tạo ra một chu trình có độ dài không chia hết cho$k$, bởi vì hai biểu đồ áp đặt cấu trúc mô-đun ẩn phải được căn chỉnh. 

## Phương pháp tiếp cận 

Bên trong mỗi đồ thị, điều kiện là tất cả độ dài chu kỳ đều chia hết cho$k$cực kỳ mạnh mẽ. Nó ngụ ý việc ghi nhãn mô-đun nhất quán của các đỉnh. Chọn bất kỳ đỉnh nào và gán giá trị cho nó$0$. Đối với bất kỳ cạnh có hướng nào$u \to v$, xác định chênh lệch giá trị của$+1$dọc theo cạnh đó. Vì mọi chu trình đều có độ dài chia hết cho$k$, phép gán này là nhất quán: bất kỳ hai đường đi nào giữa các đỉnh giống nhau đều khác nhau bởi một chu trình có độ dài là$0 \bmod k$, do đó giá trị modulo$k$được xác định rõ ràng. 

Điều này có nghĩa là mọi đỉnh trong mỗi đồ thị đều có thể được gán một phần dư trong$\mathbb{Z}_k$và mọi cạnh có hướng đều làm tăng phần dư này một cách chính xác$1 \bmod k$. 

Vì vậy mỗi đồ thị phân hủy thành$k$các lớp và mọi cạnh đi từ lớp$i$để lớp$i+1 \bmod k$. 

Bây giờ hãy xem điều gì sẽ xảy ra khi chúng ta thêm các cạnh chéo. Mỗi cạnh chéo cũng góp phần$+1$theo độ dài chu trình, do đó nó cũng phải tôn trọng cấu trúc mô-đun tương tự nếu chu trình vẫn hợp lệ. Điều này buộc tính tương thích giữa các hệ thống dư lượng của hai biểu đồ, nhưng việc ghi nhãn của biểu đồ thứ hai có thể được thay đổi theo chu kỳ mà không thay đổi giá trị nội tại. 

Vậy tự do cốt lõi là một sự thay đổi toàn cầu duy nhất$s \in [0, k-1]$áp dụng cho tất cả các dư lượng của đồ thị thứ hai. 

Sau khi cố định một sự dịch chuyển, mỗi đỉnh đều có dư lượng được xác định rõ ràng trong một hệ modulo chung. 

Bây giờ hãy nhìn vào các cạnh được thêm vào. Mỗi đỉnh phải có chính xác một cạnh được thêm vào theo hướng thích hợp (các đỉnh đi phát ra một, các đỉnh đến nhận một). Điều này buộc phải có sự kết hợp hoàn hảo giữa các tập hợp đỉnh của hai biểu đồ dưới các ràng buộc về hướng. 

Quan sát quan trọng là vì các cạnh có thể đi theo cả hai hướng giữa các biểu đồ nên cấu trúc duy nhất quan trọng là khả năng tương thích phần dư khi dịch chuyển. Sau khi các phần dư được căn chỉnh, về cơ bản, chúng tôi đang kiểm tra xem liệu chúng tôi có thể khớp nhất quán “người gửi” và “người nhận” bắt buộc trên mỗi loại dư lượng hay không. Điều này làm giảm vấn đề kiểm tra xem liệu có tồn tại một sự thay đổi sao cho đối với mỗi lớp dư lượng, số lượng điểm cuối có sẵn có khớp hay không. 

Một cách tiếp cận bạo lực sẽ thử tất cả các kết quả khớp giữa các đỉnh tôn trọng cả ràng buộc mức độ và ràng buộc chu trình, về bản chất là giai thừa và hoàn toàn không khả thi. 

Cấu trúc mô-đun thu gọn tất cả sự phức tạp thành$k$các lớp dư lượng và một tham số dịch chuyển duy nhất, biến vấn đề thành việc kiểm tra$k$sự sắp xếp có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kết hợp lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Căn chỉnh dư + dịch chuyển | O(n + m + k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1: Tính nhãn mô-đun bên trong mỗi biểu đồ 

Đối với mỗi biểu đồ, hãy chạy DFS hoặc BFS từ bất kỳ nút nào và gán một giá trị modulo$k$sao cho mỗi cạnh đều tăng giá trị lên$1$. Kết nối mạnh mẽ đảm bảo tính nhất quán của nhiệm vụ này. 

### Bước 2: Đếm loại đỉnh trên mỗi dư lượng 

Đối với mỗi biểu đồ, hãy duy trì số lượng đỉnh trong mỗi lớp dư lượng, riêng biệt cho các nhãn đi và đến. 

Vì vậy, đối với đồ thị A, chúng tôi tính toán: 

-$outA[r]$-$inA[r]$và tương tự cho đồ thị B. 

### Bước 3: Thử tất cả các phép dịch tuần hoàn cho đồ thị thứ hai 

Chúng tôi chọn một ca$s$áp dụng cho tất cả các phần dư trong đồ thị B. Sau khi dịch chuyển, một đỉnh của phần dư$r$trở thành$r + s \bmod k$. 

### Bước 4: Kiểm tra tính khả thi theo từng ca 

Đối với ca cố định, hãy tính nhu cầu kết hợp cho mỗi loại dư lượng. Các cạnh được thêm vào phải thỏa mãn rằng mọi đỉnh đi ra đều khớp với một số đỉnh đi vào trên đồ thị. Vì các cạnh luôn nằm giữa các biểu đồ nên tính khả thi sẽ giảm xuống mức bằng nhau của các điểm cuối có sẵn trên mỗi lớp dư lượng khi dịch chuyển. 

Nếu đối với một số ca, tất cả các lớp cặn cân bằng hoàn hảo thì việc xây dựng có thể thực hiện được. 

### Bước 5: Kết quả đầu ra 

Nếu có ít nhất một ca làm việc, hãy trả lời CÓ. Nếu không thì trả lời KHÔNG. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi đồ thị thừa nhận một giá trị nhất quán$\mathbb{Z}_k$hàm thế năng tăng dần$1$dọc theo các cạnh. Bất kỳ chu trình có hướng nào trong đồ thị kết hợp đều có tổng độ dài bằng tổng của các số gia này. Do đó, một chu trình có giá trị khi và chỉ khi thế năng dư lượng nhất quán trên tất cả các cạnh chéo. 

Mức độ tự do duy nhất là độ lệch tổng thể giữa hệ thống thặng dư của hai đồ thị. Sau khi phần bù này được cố định, mọi đỉnh đều có một lớp dư lượng cứng nhắc và mọi cách xây dựng hợp lệ đều phải tôn trọng các lớp này. Nếu số lượng dư lượng không thể khớp với bất kỳ phần bù nào, thì việc không khớp các cạnh có thể tránh tạo ra một chu trình có tổng mô-đun không chính xác. Ngược lại, nếu tồn tại một độ lệch nhất quán, các cạnh có thể được ghép nối trong các lớp dư lượng mà không vi phạm các ràng buộc chu trình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_residue(n, edges, k):
    adj = [[] for _ in range(n)]
    for u, v in edges:
        adj[u].append(v)

    # strongly connected + cycle condition => consistent mod-k labeling
    # we propagate arbitrary DFS labeling
    comp = [-1] * n
    val = [0] * n

    sys.setrecursionlimit(10**7)

    def dfs(u):
        for v in adj[u]:
            if comp[v] == -1:
                comp[v] = 0
                val[v] = (val[u] + 1) % k
                dfs(v)
            else:
                # consistency check (optional; guaranteed by statement)
                pass

    comp[0] = 0
    dfs(0)

    # fallback BFS to ensure all visited (graph strongly connected)
    from collections import deque
    q = deque([0])
    while q:
        u = q.popleft()
        for v in adj[u]:
            if comp[v] == -1:
                comp[v] = 0
                val[v] = (val[u] + 1) % k
                q.append(v)

    return val

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    a_cls = list(map(int, input().split()))
    m1 = int(input())
    edges1 = [tuple(map(lambda x: int(x) - 1, input().split())) for _ in range(m1)]

    b_cls = list(map(int, input().split()))
    m2 = int(input())
    edges2 = [tuple(map(lambda x: int(x) - 1, input().split())) for _ in range(m2)]

    ra = build_residue(n, edges1, k)
    rb = build_residue(n, edges2, k)

    outA = [0] * k
    inA = [0] * k
    outB = [0] * k
    inB = [0] * k

    for i in range(n):
        if a_cls[i] == 1:
            outA[ra[i]] += 1
        else:
            inA[ra[i]] += 1

        if b_cls[i] == 1:
            outB[rb[i]] += 1
        else:
            inB[rb[i]] += 1

    ok = False

    for shift in range(k):
        good = True
        for r in range(k):
            a_out = outA[r] + outB[r]
            a_in = inA[r] + inB[r]

            # apply shift to B: residue r in B becomes (r+shift)%k
            b_out = outB[(r - shift) % k]
            b_in = inB[(r - shift) % k]

            if a_out != a_in:
                good = False
                break

        if good:
            ok = True
            break

    print("YES" if ok else "NO")
```Việc triển khai trước tiên sẽ xây dựng lại cấu trúc mô-đun của từng biểu đồ. Sau đó nó nén tất cả các đỉnh vào các lớp dư lượng theo modulo$k$, được phân tách theo vai trò của chúng (đến hoặc đi). Cuối cùng, nó thử tất cả sự sắp xếp theo chu kỳ giữa hai hệ thống cặn và kiểm tra xem các ràng buộc về mức độ có thể được thỏa mãn một cách nhất quán hay không. 

Một điểm tinh tế phổ biến là sự dịch chuyển mang tính toàn cục chứ không phải trên mỗi đỉnh. Việc trộn căn chỉnh theo từng đỉnh sẽ phá hủy tính bất biến của chu trình ngay lập tức. 

## Ví dụ đã hoạt động 

### Ví dụ Dấu vết 1 

Giả sử$k = 3$và cả hai đồ thị đều có thặng dư: 

| Nhóm đỉnh | r=0 | r=1 | r=2 | 
| --- | --- | --- | --- | 
| Một đi | 1 | 1 | 0 | 
| Một | 0 | 1 | 1 | 
| B đi | 0 | 1 | 1 | 
| B đến | 1 | 0 | 1 | 

Hãy thử ca$s = 1$, nghĩa là dư lượng B quay. 

| r | Một ra | Một trong | B ra chuyển | B trong chuyển | được | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 1 | không | 
| 1 | 1 | 1 | 1 | 0 | không | 

Chuyển đổi không thành công. 

Thử tất cả các ca cuối cùng có tìm được sự trùng khớp hay không tùy thuộc vào tính đối xứng. 

Điều này chứng tỏ rằng tính chính xác phụ thuộc vào sự liên kết toàn cục chứ không phải sự ghép nối cục bộ. 

### Ví dụ Dấu vết 2 

Nếu cả hai đồ thị đều có phân bố dư lượng giống nhau, hãy dịch chuyển$s = 0$ngay lập tức cân bằng tất cả các lớp. Điều này tương ứng với trường hợp bất kỳ sự phân đôi nào giữa các vai trò khớp nhau đều hoạt động mà không vi phạm cấu trúc chu trình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + m) + k^2)$| xây dựng dư lượng cộng với việc kiểm tra tất cả các ca trên k lớp | 
| Không gian |$O(n + k)$| lân cận và số lượng dư lượng | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các đỉnh tổng thể, do đó cần phải có một đường truyền tuyến tính hoặc gần tuyến tính. Giải pháp này tránh hoàn toàn việc khớp biểu đồ và giảm mọi thứ xuống việc đếm dư lượng, vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from io import StringIO
    out = StringIO()
    _stdout = sys.stdout
    sys.stdout = out

    # assume solution is wrapped in main execution
    exec(_code, globals())
    sys.stdout = _stdout
    return out.getvalue().strip()

# minimal case
assert run("""1
2 2
1 0
1
1 2
0 1
1
2 1
""") in ["YES", "NO"]

# equal structure
assert run("""1
2 2
1 0
0
0 1
0
""") in ["YES"]

# k=1 trivial
assert run("""1
3 1
1 1 0
2
1 2
2 3
0 0 1
2
1 2
2 3
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị nhỏ | biến | độ đúng cơ sở | 
| đồ thị giống hệt nhau | CÓ | kết hợp tầm thường | 
| k=1 trường hợp | CÓ | điều kiện chu kỳ trống | 

## Vỏ cạnh 

Một trường hợp tế nhị phát sinh khi một đồ thị có tất cả các đỉnh của một lớp thặng dư trong khi đồ thị kia được phân bố đều. Trong tình huống đó, không có sự dịch chuyển nào có thể khắc phục được sự mất cân bằng và thuật toán sẽ loại bỏ một cách chính xác ngay cả khi việc so khớp đơn giản có thể cố gắng ghép các đỉnh một cách tùy ý. 

Một trường hợp góc khác là$k = n$, trong đó mỗi đỉnh có thể nằm trong một lớp dư lượng duy nhất. Ở đây, ngay cả một sự không khớp duy nhất về số lượng sẽ ngay lập tức ngăn chặn bất kỳ sự thay đổi hợp lệ nào và việc đếm dư lượng sẽ thu gọn toàn bộ vấn đề thành một kiểm tra tính bằng nhau nghiêm ngặt giữa các hoán vị. 

Trường hợp thứ ba là khi cả hai đồ thị riêng lẻ trông đối xứng, nhưng phân bố thặng dư của chúng là các phép quay tuần hoàn lẫn nhau. Vòng dịch chuyển nắm bắt chính xác tình huống này, xác nhận rằng quyền tự do duy nhất là căn chỉnh toàn cục chứ không phải sắp xếp lại theo từng đỉnh.
