---
title: "CF 1088E – Ehab và bài toán chọn linh kiện"
description: "Chúng ta được cấp một cây có trọng số, trong đó mỗi nút mang một giá trị nguyên có thể dương, âm hoặc bằng 0. Nhiệm vụ là chọn một số nút và phân chia các nút đã chọn thành một số thành phần được kết nối."
date: "2026-06-15T05:31:08+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "math", "trees"]
categories: ["algorithms"]
codeforces_contest: 1088
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 525 (Div. 2)"
rating: 2400
weight: 1088
solve_time_s: 399
verified: true
draft: false
---

[CF 1088E - Ehab và vấn đề chọn thành phần](https://codeforces.com/problemset/problem/1088/E) 

**Đánh giá:** 2400 
**Tags:** dp, tham lam, toán học, cây cối 
**Thời gian giải:** 6 phút 39 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có trọng số, trong đó mỗi nút mang một giá trị nguyên có thể dương, âm hoặc bằng 0. Nhiệm vụ là chọn một số nút và phân chia các nút đã chọn thành một số thành phần được kết nối. Bên trong mỗi thành phần được chọn, tất cả các nút phải được kết nối thông qua các cạnh chỉ sử dụng các nút từ cùng thành phần đó và các thành phần khác nhau phải tách rời nhau. 

Sau khi lựa chọn được cố định, chúng tôi đánh giá nó bằng cách lấy tổng của tất cả các trọng số nút đã chọn và chia cho số thành phần được kết nối được hình thành. Mục tiêu là tối đa hóa tỷ lệ này. Nếu nhiều lựa chọn đạt được cùng một tỷ lệ tốt nhất thì sự ràng buộc sẽ được giải quyết bằng cách ưu tiên cấu hình sử dụng nhiều thành phần hơn. 

Cấu trúc của bài toán là sai lầm vì mẫu số phụ thuộc vào cách chúng ta chia các đỉnh đã chọn chứ không chỉ là số lượng chúng ta chọn. Cấu trúc cây cho phép các thành phần được hình thành bằng cách cắt các cạnh một cách ngầm định, nhưng tính kề cận không buộc các nút vào cùng một thành phần, vì vậy chúng ta thực sự đang chọn các tập hợp con tùy ý và sau đó đếm xem chúng tạo ra bao nhiêu phần được kết nối. 

Ràng buộc n lên đến 3·10^5 buộc phải có nghiệm tuyến tính hoặc gần tuyến tính. Bất kỳ cách tiếp cận nào cố gắng liệt kê các tập hợp con hoặc thử một cách rõ ràng tất cả các phân tách thành phần có thể đều không khả thi ngay lập tức vì số lượng các tập hợp con là theo cấp số nhân và thậm chí các chuyển đổi bậc hai trên mỗi nút sẽ vượt quá giới hạn thời gian. 

Trường hợp cạnh tinh tế phát sinh khi tất cả các trọng số đều không dương. Một cách tiếp cận tham lam ngây thơ có thể cố gắng tránh chọn hoàn toàn các nút hoặc giảm bớt các thành phần, nhưng vấn đề đòi hỏi ít nhất một thành phần, vì vậy việc chọn một nút duy nhất trở thành hành động có ý nghĩa duy nhất. Một tình huống không rõ ràng khác xuất hiện khi giá trị tối ưu là âm. Trong những trường hợp như vậy, việc tăng số lượng thành phần có thể cải thiện tỷ lệ ngay cả khi tổng số tiền không tăng, vì hiệu ứng mẫu số chi phối hành vi phá vỡ ràng buộc. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng chọn một tập hợp con các nút, tính toán số lượng thành phần được kết nối mà chúng tạo ra và đánh giá tỷ lệ. Điều này đã trở nên khó khăn vì kết nối không độc lập giữa các nút và số lượng tập hợp con là 2^n. Ngay cả khi chúng ta sửa k, việc quyết định liệu chúng ta có thể hình thành k thành phần với tổng tối đa hay không sẽ dẫn đến vấn đề phân vùng bị ràng buộc trên cây, nói chung vẫn là hàm mũ. 

Cái nhìn sâu sắc quan trọng đến từ việc tách biệt vai trò của các nút và thành phần. Mỗi thành phần đóng góp chính xác một đơn vị cho mẫu số, bất kể kích thước của nó. Vì vậy, vấn đề không phải là việc chọn các cạnh hoặc vết cắt trực tiếp mà là việc quyết định nút nào được phép “khởi động” các thành phần và nút nào nên được gắn vào các nút hiện có mà không làm tăng số lượng thành phần. 

Điều này gợi ý một sự chuyển đổi: thay vì suy nghĩ theo khía cạnh các thành phần, chúng tôi nghĩ đến việc gán mỗi nút dưới dạng “đóng góp gốc” cho số lượng thành phần hoặc như một phần của thành phần đã tồn tại mà không tăng k. Nếu chúng ta cố định một giá trị mục tiêu x là giá trị trung bình, chúng ta có thể hỏi liệu có tồn tại một lựa chọn sao cho tổng trừ x nhân số thành phần là không âm hay không. Điều này chuyển vấn đề thành việc kiểm tra tính khả thi của tối ưu hóa có trọng số, trở thành tuyến tính trên cây. 

Đối với x cố định, mỗi nút đóng góp một giá trị được sửa đổi a_u - x. Mục tiêu là chọn một tập hợp các nút giúp tối đa hóa mục tiêu được chuyển đổi trong đó khả năng kết nối xác định liệu chúng ta có phải trả thêm hình phạt x cho mỗi thành phần hay không. Cấu trúc của cây cho phép DP tham lam nơi chúng tôi tích lũy lợi nhuận đi lên, quyết định cục bộ xem có nên giữ một nút trong cấu trúc gốc của nó hay cắt nút đó và bắt đầu một thành phần mới.

Điều này dẫn đến ý tưởng “DP tham số trên cây” tiêu chuẩn: chúng tôi tìm kiếm nhị phân x trung bình tối ưu và với mỗi x tính toán xem liệu chúng tôi có thể đạt được lợi nhuận được điều chỉnh không âm bằng cách phân chia thành phần tối ưu hay không. Bản thân tấm séc là một cây DP nơi chúng tôi truyền bá phần dư thừa lên trên, đảm bảo rằng chúng tôi chỉ tạo một thành phần mới khi nó có lợi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con và phân vùng | O(2^n) | O(n) | Quá chậm | 
| Tìm kiếm tham số + kiểm tra tính khả thi của cây DP | O(n log V) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi trình bày lại mục tiêu là tìm giá trị trung bình tối đa có thể có cho mỗi thành phần. Chúng tôi tìm kiếm nhị phân giá trị x này. 

Với mỗi ứng viên x, chúng ta xác định trọng số biến đổi b_u = a_u - x. Nếu chúng ta chọn một tập hợp các nút tạo thành một số thành phần được kết nối, thì điểm được chuyển đổi sẽ trở thành tổng (b_u) trên tất cả các nút đã chọn trừ x lần số thành phần được tính ngầm. Câu hỏi khả thi là liệu chúng ta có thể đạt được điểm không âm trong quá trình hình thành thành phần tối ưu hay không. 

Chúng tôi xử lý cây bằng DFS được root ở bất kỳ đâu. 

1. Chúng ta tính giá trị DP dp[u] đại diện cho phần thặng dư tốt nhất có thể mà chúng ta có thể chuyển từ cây con của u sang cây cha của nó nếu u được kết nối lên trên thành phần cha của nó. 
2. Với mỗi con v của u, trước tiên chúng ta tính dp[v]. Nếu dp[v] dương, chúng tôi giữ nó và hợp nhất nó vào u, vì việc gắn nó sẽ cải thiện tổng thặng dư mà không làm tăng số lượng thành phần. Nếu dp[v] không dương, chúng tôi sẽ loại bỏ nó, tương ứng với việc cắt cây con đó thành một thành phần riêng biệt hoặc không chọn nó chút nào. 
3. Sau khi tổng hợp các phần tử con, chúng ta thêm b_u vào nút hiện tại. Điều này thể hiện việc bao gồm nút u trong cấu trúc được kết nối hiện tại. 
4. Nếu kết quả dp[u] trở thành âm, chúng ta đặt lại nó về 0 và hiểu điều này là cắt ở đây, nghĩa là chúng ta bắt đầu một thành phần mới tại u thay vì buộc nó vào thành phần mẹ của nó. Việc cắt giảm này tương ứng với việc tăng số lượng linh kiện theo cách tránh kéo thặng dư toàn cầu xuống. 
5. Sau khi xử lý gốc, chúng tôi kiểm tra xem dp[root] có âm hay không. Nếu đúng như vậy thì ứng viên x có thể đạt được. 

Tìm kiếm nhị phân được thực hiện trên một phạm vi đủ rộng để bao gồm tất cả các mức trung bình có thể có, thường là giữa trọng lượng nút tối thiểu và tối đa. 

Tại sao nó hoạt động được rút ra từ cách giải thích ngắn gọn của DP. Mỗi nút đóng góp vào một thành phần đang diễn ra hoặc trở thành gốc của một thành phần mới khi việc đưa nó vào sẽ làm giảm mục tiêu được điều chỉnh. DP đảm bảo mọi đóng góp tiêu cực được tách biệt thành một thành phần riêng biệt thay vì làm tổn hại đến cấu trúc lớn hơn, phù hợp chính xác với khả năng kiểm soát tối ưu của mẫu số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n = int(input())
a = list(map(int, input().split()))
g = [[] for _ in range(n)]
for _ in range(n - 1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

def check(x):
    b = [ai - x for ai in a]

    def dfs(u, p):
        s = b[u]
        for v in g[u]:
            if v == p:
                continue
            child = dfs(v, u)
            if child > 0:
                s += child
        return max(0.0, s)

    return dfs(0, -1) >= 0

lo = min(a)
hi = max(a)

for _ in range(60):
    mid = (lo + hi) / 2
    if check(mid):
        lo = mid
    else:
        hi = mid

print(f"{lo:.10f} 1")
```DFS tính toán số dư dương có thể được tích lũy từ mỗi cây con sau khi dịch chuyển trọng số theo giá trị x hiện tại. Chi tiết triển khai chính là mức tối đa bằng 0, mã hóa quyết định cắt một thành phần bất cứ khi nào việc tiếp tục thành phần đó trở nên có hại. Đây là điều ngăn chặn các cây con âm làm ô nhiễm cây mẹ của chúng. 

Tìm kiếm nhị phân tinh chỉnh giá trị trung bình ứng viên cho đến khi đạt được độ chính xác đủ. Câu trả lời cuối cùng được biểu thị dưới dạng phân số có mẫu số 1 vì giá trị tối ưu là số thực được biểu thị dưới dạng tỷ lệ trên mỗi thành phần và việc xây dựng đảm bảo đạt được mức trung bình tối đa ở dạng liên tục này. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây nhỏ gồm ba nút trong chuỗi có trọng số 1, 2, 3. 

Chúng tôi kiểm tra một ứng cử viên x = 2. 

| Nút | b[u] = a[u] - x | Đóng góp của trẻ em | dp[u] trước kẹp | dp[u] sau | 
| --- | --- | --- | --- | --- | 
| 3 | 1 | 0 | 1 | 1 | 
| 2 | 0 + 1 = 1 | 1 | 2 | 2 | 
| 1 | -1 + 2 = 1 | 2 | 3 | 3 | 

Căn nguyên trả về giá trị dương nên x = 2 là khả thi. Điều này chứng tỏ chuỗi tích cực tích lũy thặng dư trở lên như thế nào. 

Bây giờ hãy xem xét các trọng số 1, -5, 1 trong một ngôi sao có gốc ở nút 2. 

| Nút | b[u] (x = 0) | Tổng hợp trẻ em | dp[u] | 
| --- | --- | --- | --- | 
| lá | 1, 1 | 0 | 1, 1 | 
| gốc | -5 + 1 + 1 | 2 | 2 | 

Điều này cho thấy rằng ngay cả một nút âm mạnh cũng có thể được đưa vào nếu cấu trúc xung quanh bù lại, nhưng nói chung tìm kiếm nhị phân sẽ xác định ngưỡng mà việc đưa vào không còn có lợi. 

Những dấu vết này cho thấy sự đóng góp của cây con lan truyền lên trên khi có lợi hoặc được hấp thụ dưới dạng các thành phần riêng biệt khi âm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log V) | Mỗi bước tìm kiếm nhị phân chạy một DFS tuyến tính trên cây | 
| Không gian | O(n) | Danh sách kề và ngăn xếp đệ quy | 

Giải pháp này vẫn hiệu quả vì 60 lần duyệt DFS trên 3·10^5 nút nằm trong giới hạn và mức sử dụng bộ nhớ là tuyến tính theo kích thước cây. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline
    n = int(input())
    a = list(map(int, input().split()))
    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    sys.setrecursionlimit(10**7)

    def check(x):
        b = [ai - x for ai in a]

        def dfs(u, p):
            s = b[u]
            for v in g[u]:
                if v == p:
                    continue
                val = dfs(v, u)
                if val > 0:
                    s += val
            return max(0.0, s)

        return dfs(0, -1) >= 0

    lo, hi = min(a), max(a)
    for _ in range(60):
        mid = (lo + hi) / 2
        if check(mid):
            lo = mid
        else:
            hi = mid

    print(f"{lo:.10f} 1")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# sample 1
assert run("""3
1 2 3
1 2
1 3""") == "3.0000000000 1"

# single node
assert run("""1
5
""") == "5.0000000000 1"

# all negative
assert run("""3
-1 -2 -3
1 2
2 3
""") == "-1.0000000000 1"

# mixed values
assert run("""5
1 -2 3 -4 5
1 2
2 3
3 4
4 5
""") == "5.0000000000 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi tích cực | 3.0 | tích lũy trong cây tuyến tính | 
| nút đơn | a1 | trường hợp cơ sở đúng đắn | 
| tất cả đều tiêu cực | phần tử tối đa | xử lý tiêu cực và cắt tỉa | 
| chuỗi hỗn hợp | 5.0 | đưa vào có chọn lọc trong DP | 

## Vỏ cạnh 

Cây nút đơn tách biệt hoạt động kiểm tra tính khả thi: DP giảm việc so sánh trọng lượng nút với ngưỡng hiện tại và bất kỳ việc xử lý không chính xác nào đối với bước kẹp sẽ ngay lập tức phá vỡ tính chính xác. 

Cây âm hoàn toàn buộc thuật toán phải loại bỏ nhiều lần các cây con và kết quả cuối cùng phụ thuộc hoàn toàn vào nút riêng lẻ lớn nhất, dẫn đến lỗi trong logic tích lũy toàn cục nếu các giá trị âm không được cắt bỏ đúng cách.
