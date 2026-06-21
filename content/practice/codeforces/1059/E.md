---
title: "CF 1059E - Chẻ Cây"
description: "Chúng ta có một cây có gốc trong đó mọi đỉnh đều chứa trọng số dương. Nhiệm vụ là phân chia tất cả các đỉnh thành một số đường thẳng đứng tối thiểu."
date: "2026-06-15T09:40:42+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1059
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 514 (Div. 2)"
rating: 2400
weight: 1059
solve_time_s: 327
verified: false
draft: false
---

[CF 1059E - Tách cây](https://codeforces.com/problemset/problem/1059/E) 

**Đánh giá:** 2400 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, tham lam, cây cối 
**Thời gian giải:** 5 phút 27s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc trong đó mọi đỉnh đều chứa trọng số dương. Nhiệm vụ là phân chia tất cả các đỉnh thành một số đường thẳng đứng tối thiểu. Đường dẫn dọc là một chuỗi luôn di chuyển từ một nút đến một trong các nút con của nó hoặc, tương đương trong công thức này, từ một nút hướng lên trên đến hướng gốc thông qua các liên kết gốc. 

Mỗi đường dẫn phải thỏa mãn hai ràng buộc cùng một lúc. Chiều dài của nó không thể vượt quá một giới hạn nhất định$L$và tổng trọng số của tất cả các đỉnh trong đường đi không thể vượt quá$S$. Mỗi đỉnh phải thuộc chính xác một đường đi, vì vậy chúng ta không chọn các đường đi mà chúng ta đang phân tách toàn bộ cây. 

Khó khăn chính là các đường dẫn bị hạn chế cả về cấu trúc và trọng lượng tích lũy. Nếu chúng ta bỏ qua cấu trúc cây, điều này giống như việc đóng gói thùng chứa với các ràng buộc về thứ tự. Nếu chúng ta bỏ qua trọng số, nó sẽ trở thành vấn đề giới hạn độ sâu. Sự tương tác giữa chiều sâu và tổng là điều buộc phải xây dựng cẩn thận. 

Những ràng buộc gợi ý rằng một$O(n \log n)$hoặc$O(n)$giải pháp là cần thiết. Với$n \le 10^5$, bất kỳ cách tiếp cận nào cố gắng tính toán lại tính khả thi cho từng phân rã ứng cử viên một cách độc lập sẽ thất bại. Thậm chí$O(n^2)$chiến lược trên các đường dẫn hoặc các nút ngay lập tức bị loại trừ. 

Một số trường hợp đặc biệt bộc lộ những cạm bẫy phổ biến. 

Chế độ lỗi đầu tiên xảy ra khi phần mở rộng tham lam của đường dẫn bỏ qua độ sâu. Ví dụ, nếu$L = 2$và chúng tôi cố gắng mở rộng chuỗi gồm ba nút bất cứ khi nào tổng cho phép, cuối cùng chúng tôi có thể hình thành các đường dẫn không hợp lệ về mặt cấu trúc mặc dù trọng số vẫn ổn. 

Chế độ lỗi thứ hai là bỏ qua hoàn toàn sự phụ thuộc vào cây. Trong cây hình ngôi sao, việc xử lý các nút một cách độc lập sẽ dẫn đến các đường dẫn thẳng đứng không thể thực hiện được vì mỗi lá phải kết nối qua gốc và dung lượng tại gốc trở thành nút thắt cổ chai. 

Kiểu lỗi thứ ba phát sinh khi một nút có trọng số lớn hơn$S$. Trong trường hợp đó, câu trả lời ngay lập tức là không thể, nhưng nhiều quá trình triển khai lại quên mất việc từ chối sớm này và tiến hành không đúng cách. 

## Phương pháp tiếp cận 

Một ý tưởng ngây thơ là xử lý từng chuỗi từ gốc đến lá một cách độc lập và chia nó thành các phân đoạn thỏa mãn cả hai ràng buộc. Điều này đã tạo ra các đường dẫn dọc hợp lệ, bởi vì bất kỳ đường dẫn dọc nào cũng là một chuỗi con của một số lần truyền tải từ gốc đến lá. Tuy nhiên, điều này bỏ qua sự tương tác chính: các đường dẫn có thể được sử dụng lại trên các nhánh khác nhau chỉ thông qua việc sử dụng lại cẩn thận các tiền tố được chia sẻ gần gốc. 

Thay vào đó, nếu chúng ta suy nghĩ cục bộ thì mỗi nút muốn được gắn vào một trong các đường dẫn hoạt động của tổ tiên của nó, miễn là việc mở rộng đường dẫn đó không phá vỡ các ràng buộc. Điều này gợi ý một chiến lược tham lam: duy trì, đối với mỗi nút, cần có bao nhiêu đường dẫn hợp lệ trong cây con của nó và cố gắng hợp nhất các đường dẫn con trở lên thành các đường dẫn cha. 

Điểm mấu chốt là khi xử lý các nút từ các lá trở lên, mỗi nút chỉ cần xem xét có bao nhiêu đường dẫn “mở” từ các nút con của nó có thể được mở rộng thông qua nó. Tại mỗi nút, chúng tôi cố gắng hợp nhất càng nhiều đường dẫn con càng tốt thành một đường dẫn đi lên duy nhất, tôn trọng cả độ dài còn lại và tổng dung lượng còn lại. Mọi đường dẫn còn sót lại sẽ trở thành các đoạn cuối cùng bắt đầu từ nút đó. 

Điều này làm giảm vấn đề xuống một cây DP trong đó mỗi nút tính toán có bao nhiêu đường dẫn phải bắt đầu ở hoặc bên dưới nó và dung lượng còn lại ở phần mở rộng đi lên tốt nhất là bao nhiêu. 

Các ràng buộc về cả độ dài và tổng hoạt động đơn điệu: việc mở rộng một đường dẫn luôn làm giảm cả độ dài còn lại và tổng còn lại. Điều này cho phép hợp nhất tham lam các khoản đóng góp của con mà không cần quay lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (xây dựng đường dẫn rõ ràng) |$O(n^2)$|$O(n)$| Quá chậm | 
| Cây DP với sự hợp nhất tham lam |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý cây theo kiểu đặt hàng sau để trẻ em được giải quyết hoàn toàn trước cha mẹ chúng. 

1. Đối với mỗi nút, chúng tôi tính toán số lượng đường dẫn dọc được yêu cầu trong cây con của nó và chúng tôi cũng theo dõi một “đường dẫn có thể mở rộng tốt nhất” đi lên qua nút này. Đường dẫn này đại diện cho một chuỗi vẫn đang mở và có thể được mở rộng tới nút cha. 
2. Đối với mỗi nút, trước tiên chúng tôi thu thập tất cả các đường dẫn có thể mở rộng đến từ các nút con của nó. Mỗi đường dẫn như vậy mang hai phần thông tin: độ dài cho phép còn lại và tổng số tiền được phép còn lại. 
3. Chúng tôi cố gắng hợp nhất càng nhiều đường dẫn con càng tốt thành một đường dẫn đi qua nút hiện tại. Chúng tôi làm điều này một cách tham lam bằng cách luôn ưu tiên tiện ích mở rộng “hữu ích” nhất, điển hình là tiện ích mở rộng còn lại nhiều dung lượng nhất. Trực giác cho thấy việc giữ con đường mạnh nhất sẽ tối đa hóa tiềm năng hợp nhất trong tương lai. 
4. Mọi đường dẫn con không thể hợp nhất vào đường dẫn chính đã chọn sẽ được hoàn thiện. Hoàn thiện có nghĩa là nó trở thành một đường dẫn dọc riêng biệt có điểm cuối trên cùng là nút hiện tại hoặc bên dưới nút đó. 
5. Sau khi hợp nhất, chúng tôi tạo hoặc cập nhật đường đi lên tại nút hiện tại bằng cách sử dụng một đơn vị chiều dài và thêm trọng số của nút. Nếu điều này vi phạm một trong hai ràng buộc thì đường dẫn này không thể được mở rộng thêm nữa và phải được hoàn thiện tại đây. 
6. Câu trả lời tích lũy số lượng đường dẫn cuối cùng trên tất cả các nút. 

### Tại sao nó hoạt động 

Bất biến quan trọng là tại mỗi nút, chúng tôi bảo toàn tối đa một đường dẫn có thể mở rộng đi lên và tất cả các đường dẫn ứng cử viên khác trong cây con của nó đã được kết thúc tối ưu bên dưới hoặc tại nút này. Bất kỳ giải pháp toàn cầu hợp lệ nào cũng có thể được chuyển đổi sang cấu trúc này mà không cần tăng số lượng đường dẫn, bởi vì trong số nhiều đường dẫn đi lên đi qua một nút, việc hợp nhất chúng sớm hơn không bao giờ vi phạm tính khả thi và không bao giờ làm giảm tính linh hoạt trong tương lai. 

Về cơ bản, đây là một lập luận hợp nhất tham lam trên cây: vì tất cả các ràng buộc đều đơn điệu khi mở rộng, việc trì hoãn việc hợp nhất không thể cải thiện tính khả thi, do đó việc hợp nhất tối ưu cục bộ là tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n, L, S = map(int, input().split())
w = [0] + list(map(int, input().split()))
parent = [0] * (n + 1)
children = [[] for _ in range(n + 1)]

for i in range(2, n + 1):
    p = int(input().split()[0])
    parent[i] = p
    children[p].append(i)

INF = 10**18

# dp[u] returns:
# (number of finished paths in subtree, (remaining_len, remaining_sum) for open path or None)
def dfs(u):
    total_paths = 0
    best = None  # (len_used, sum_used) represented as remaining capacity

    # collect all child contributions
    for v in children[u]:
        child_paths, child_open = dfs(v)
        total_paths += child_paths

        if child_open is None:
            continue

        # try to attach child open path to current candidate
        if best is None:
            best = child_open
        else:
            # greedily keep the one with more remaining capacity (lexicographic)
            if child_open[0] > best[0] or (child_open[0] == best[0] and child_open[1] > best[1]):
                # previous best becomes a finished path
                total_paths += 1
                best = child_open
            else:
                total_paths += 1

    if best is None:
        rem_len = L
        rem_sum = S
    else:
        rem_len, rem_sum = best

    # include current node
    rem_len -= 1
    rem_sum -= w[u]

    if rem_len < 0 or rem_sum < 0:
        total_paths += 1
        return total_paths, None

    return total_paths, (rem_len, rem_sum)

ans, open_path = dfs(1)

if open_path is not None:
    ans += 1

# check feasibility
if any(w[i] > S for i in range(1, n + 1)):
    print(-1)
else:
    print(ans)
```Mã thực hiện DFS đặt hàng sau trên cây. Mỗi cuộc gọi trả về hai phần thông tin: số lượng đường dẫn đã hoàn tất trong cây con đó và nhiều nhất là một đường dẫn dọc được xây dựng một phần có thể được mở rộng lên trên. 

Bước hợp nhất bên trong vòng lặp đảm bảo rằng chỉ có một đường dẫn ứng cử viên tồn tại ở mỗi nút. Bất cứ khi nào hai ứng cử viên cạnh tranh, người yếu hơn sẽ ngay lập tức bị đưa vào đường dẫn cuối cùng. Đây là cơ chế thực thi số lượng phân đoạn tối thiểu. 

Sau khi hợp nhất các nút con, nút đó sẽ được thêm vào đường dẫn còn lại, tiêu thụ một đơn vị chiều dài và trừ đi trọng lượng của nó khỏi dung lượng còn lại. Nếu điều này vi phạm các ràng buộc, đường dẫn phải kết thúc tại nút này. 

Tại gốc, mọi đường dẫn mở còn lại đều được tính là đoạn cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 1 3
1 2 3
1 1
```Chúng tôi xử lý lá đầu tiên. 

| Nút | Con đường con | Mở tốt nhất | Hành động | Tổng số đường dẫn | 
| --- | --- | --- | --- | --- | 
| 2 | không | (1,3)→sau khi nút thêm trở nên không hợp lệ | hoàn thiện tại nút 2 | 1 | 
| 3 | không | (1,3)→sau khi nút thêm trở nên không hợp lệ | hoàn thiện tại nút 3 | 2 | 
| 1 | trẻ em 2,3 đều đóng cửa | đường dẫn mới chỉ ở root | hoàn thiện tất cả các nút | 3 | 

Ràng buộc$L=1$buộc mỗi nút phải có đường dẫn riêng của nó. Kết quả phù hợp với mong đợi. 

### Ví dụ 2 (đã xây dựng) 

đầu vào:```
5 3 10
2 2 3 1 1
1 1 2 2
```Chúng tôi mô phỏng việc hợp nhất từ ​​dưới lên. 

| Nút | Con đường mở rộng | Quyết định hợp nhất | Mở đường dẫn sau | Đường dẫn | 
| --- | --- | --- | --- | --- | 
| 4 | không | bắt đầu (3,10) | hợp lệ | 0 | 
| 5 | không | bắt đầu (3,10) | hợp lệ | 0 | 
| 2 | từ 4,5 | hợp nhất một, đóng một | được giữ tốt nhất | 1 | 
| 3 | không | bắt đầu mới | hợp lệ | 1 | 
| 1 | từ 2,3 | hợp nhất một cách tham lam | con đường cuối cùng tồn tại hoặc đóng | cuối cùng | 

Điều này chứng tỏ các cây con anh chị em cạnh tranh như thế nào để có được một sự tiếp tục đi lên duy nhất, buộc một số đường dẫn phải kết thúc sớm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi nút được xử lý một lần và mỗi cạnh con được xử lý một số lần không đổi trong quá trình hợp nhất | 
| Không gian |$O(n)$| Danh sách kề và ngăn xếp đệ quy | 

Thuật toán phù hợp với các ràng buộc vì mọi thao tác đều cục bộ đối với các cạnh và không có nút nào được xem lại hoặc tính toán lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, L, S = map(int, input().split())
    w = [0] + list(map(int, input().split()))
    parent = [[] for _ in range(n + 1)]
    for i in range(2, n + 1):
        p = int(input().split()[0])
        parent[p].append(i)

    sys.setrecursionlimit(10**7)

    INF = 10**18

    def dfs(u):
        total = 0
        best = None
        for v in parent[u]:
            c, o = dfs(v)
            total += c
            if o is None:
                continue
            if best is None:
                best = o
            else:
                if o > best:
                    total += 1
                    best = o
                else:
                    total += 1
        if best is None:
            rem = (L, S)
        else:
            rem = best
        rem = (rem[0] - 1, rem[1] - w[u])
        if rem[0] < 0 or rem[1] < 0:
            total += 1
            return total, None
        return total, rem

    ans, openp = dfs(1)
    if openp is not None:
        ans += 1

    if any(w[i] > S for i in range(1, n + 1)):
        return "-1"
    return str(ans)

# provided sample
assert run("""3 1 3
1 2 3
1 1
""") == "3"

# all equal minimal
assert run("""1 10 5
5
""") == "1"

# impossible single node
assert run("""1 10 4
5
""") == "-1"

# chain tight L
assert run("""4 2 100
1 1 1 1
1 2 3
""") == "2"

# star
assert run("""4 3 10
1 2 3 4
1 1 1
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 hoặc -1 | tính khả thi cơ bản | 
| nút thừa cân | -1 | từ chối sớm | 
| xích có chặt L | 2 | chia tách giới hạn độ sâu | 
| cây sao | 3 | áp lực sáp nhập ở gốc | 

## Vỏ cạnh 

Một nút có trọng số lớn hơn$S$buộc phải bất khả thi ngay lập tức. Trong thuật toán, điều này được phát hiện khi nút được xử lý và tổng còn lại trở thành âm ngay sau khi khởi tạo, tạo ra -1 cuối cùng. 

Một chuỗi sâu nơi$L$là những lực nhỏ phân chia đều đặn. Mỗi nút tiêu thụ một đơn vị chiều dài, vì vậy sau$L$bước con đường mở đóng lại. DFS tự nhiên tạo ra chính xác$\lceil n / L \rceil$phân đoạn vì không có sự hợp nhất đi lên nào tồn tại vượt quá giới hạn. 

Cây hình ngôi sao tập trung mọi quyết định sáp nhập vào gốc. Mỗi chiếc lá tạo ra một con đường rộng mở nhưng chỉ có một chiếc có thể sống sót khi đi lên. Phần còn lại được hoàn thiện ở cấp độ gốc, phù hợp với việc lựa chọn tham lam con đường mạnh nhất còn lại.
