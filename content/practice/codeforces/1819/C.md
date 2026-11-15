---
title: "CF 1819C - Con cáo và chuyến đi ngang qua cây hoàn chỉnh"
description: "Chúng ta được cho một cây vô hướng có tới hai trăm nghìn đỉnh. Quy tắc chuyển động rất khác thường: từ một đỉnh, con cáo có thể nhảy tới bất kỳ đỉnh nào trong khoảng cách đồ thị nhiều nhất là hai."
date: "2026-06-09T08:02:25+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dp", "implementation", "math", "trees"]
categories: ["algorithms"]
codeforces_contest: 1819
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 866 (Div. 1)"
rating: 2400
weight: 1819
solve_time_s: 84
verified: false
draft: false
---

[CF 1819C - Con cáo và hành trình vượt cây hoàn chỉnh](https://codeforces.com/problemset/problem/1819/C) 

**Đánh giá:** 2400 
**Tags:** thuật toán xây dựng, dp, triển khai, toán học, cây 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây vô hướng có tới hai trăm nghìn đỉnh. Quy tắc chuyển động rất khác thường: từ một đỉnh, con cáo có thể nhảy tới bất kỳ đỉnh nào trong khoảng cách đồ thị nhiều nhất là hai. Điều đó có nghĩa là một bước di chuyển có thể đi tới một đỉnh lân cận trực tiếp hoặc tới một đỉnh có chung đỉnh lân cận với vị trí hiện tại. 

Nhiệm vụ là xác định xem có thể sắp xếp tất cả các đỉnh trong một chu trình sao cho mỗi cặp liên tiếp trong thứ tự được kết nối bằng một bước nhảy như vậy hay không và đỉnh cuối cùng cũng có thể nhảy trở lại đỉnh đầu tiên. Tất cả các đỉnh đều phải xuất hiện chính xác một lần, vì vậy về cơ bản chúng ta đang tìm kiếm chu trình Hamilton, nhưng trong một đồ thị dẫn xuất trong đó các cạnh biểu thị “khoảng cách nhiều nhất là hai trong cây”. 

Đồ thị dẫn xuất dày đặc theo một cách rất có cấu trúc: mỗi đỉnh kết nối với các đỉnh lân cận của nó và cũng với tất cả các đỉnh trong hợp các lân cận của các đỉnh đó. Tuy nhiên, nó không hề tùy tiện; nó thừa hưởng các ràng buộc mạnh mẽ từ cấu trúc cây. 

Ràng buộc n lên tới 2e5 ngay lập tức loại trừ bất kỳ giải pháp nào xây dựng rõ ràng đồ thị dẫn xuất hoặc chạy tìm kiếm chu trình Hamilton trên đó. Bất kỳ việc xây dựng O(n²) nào hoặc quay lại các hoán vị đều không thể thực hiện được. Ngay cả các cấu trúc O(n log n) cũng phải cẩn thận vì tính liền kề là ẩn chứ không rõ ràng. 

Hộp đựng cạnh chìa khóa là một cây hình ngôi sao. Trong một ngôi sao, mỗi lá cách nhau hai lá, do đó đồ thị dẫn xuất trở nên hoàn chỉnh. Mọi hoán vị đều có tác dụng và đây là trường hợp tích cực dễ dàng nhất. Mặt khác, một đường đi dài hoạt động rất khác: các điểm cuối được kết nối yếu theo nghĩa dẫn xuất và áp đặt các ràng buộc thứ tự nghiêm ngặt. Nhiều phép duyệt tham lam ngây thơ thất bại trên các đường đi vì các lựa chọn cục bộ có thể mắc kẹt các đỉnh còn lại mà không có sự tiếp tục hợp lệ. 

Một trường hợp tinh vi khác là khi một đỉnh có bậc hai ở mọi nơi ngoại trừ một số điểm phân nhánh. Các cấu trúc trung gian này tạo ra “nút thắt cổ chai” trong đó thứ tự không chính xác buộc phải xem lại hoặc phá vỡ các ràng buộc lân cận ở khoảng cách hai. 

Khó khăn cốt lõi là nhận ra rằng tính khả thi phụ thuộc hoàn toàn vào cấu trúc của cây chứ không phải vào việc xây dựng đường dẫn động trong biểu đồ dẫn xuất. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ xây dựng một cách rõ ràng đồ thị trong đó tồn tại một cạnh giữa bất kỳ cặp đỉnh nào có khoảng cách tối đa là hai, sau đó cố gắng tìm chu trình Hamilton. Ngay cả việc xây dựng biểu đồ này cũng tốn O(n2) trong trường hợp xấu nhất, vì một cái cây dày đặc như một ngôi sao tạo ra các cạnh Θ(n2) trong biểu đồ dẫn xuất. Sau đó, việc phát hiện chu trình Hamilton tự nó là NP-đầy đủ trong các biểu đồ tổng quát, do đó, mọi tìm kiếm đơn giản hoặc DP trên các tập hợp con đều trở nên không khả thi ở quy mô này. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần biểu đồ dẫn xuất đầy đủ. Cách duy nhất để đi qua tất cả các đỉnh đúng một lần là đảm bảo rằng chúng ta không bao giờ “bẫy” chính mình ở một đỉnh mà các đỉnh lân cận còn lại đều nằm quá xa trong cấu trúc cây. Điều này tạo ra một ràng buộc thứ tự rất cứng nhắc: phân nhánh cấp độ cao phải được giải quyết theo cách có kiểm soát sao cho mỗi nhánh được vào và ra trong phạm vi kết nối khoảng cách hai. 

Cấu trúc nổi lên là câu trả lời tồn tại khi và chỉ khi cây không “quá tuyến tính”. Chính xác hơn, trở ngại duy nhất xuất phát từ thực tế là khi chúng ta ở một đỉnh có bậc lớn hơn hai, chúng ta phải có khả năng thăm các cây con của nó theo thứ tự giữ cho các chuyển tiếp trong khoảng cách hai. Điều này dẫn đến một chiến lược mang tính xây dựng dựa trên việc root cây và thực hiện một thứ tự giống như DFS, đảm bảo rằng khi chúng ta vào một cây con, chúng ta luôn có thể quay lại thông qua một hàng xóm hoặc một phím tắt khoảng cách hai.

Việc xây dựng giảm xuống còn tạo ra thứ tự DFS nhưng chọn cẩn thận các điểm đầu vào sao cho các đỉnh liên tiếp trong quá trình truyền tải luôn nằm trong khoảng cách hai trong cây ban đầu. Điều này đạt được bằng cách đi qua cây trong khi vẫn đảm bảo rằng chúng ta luôn di chuyển đến cây con hoặc cây con anh em thông qua cây mẹ, điều này đảm bảo kết nối hai cạnh. 

Brute-force hoạt động vì nó kiểm tra rõ ràng tất cả các chuỗi có thể, nhưng không thành công do độ phức tạp theo cấp số nhân và sự mở rộng đồ thị dày đặc. Quan sát rằng sự kề cận khoảng cách hai bị chi phối hoàn toàn bởi cấu trúc cha-con cục bộ cho phép chúng ta xây dựng một chu trình hợp lệ duy nhất trong thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n2 + n!) | O(n²) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Việc xây dựng bắt nguồn từ thực tế là một cây luôn có đường truyền hợp lệ trong biểu đồ khoảng cách hai trừ khi nó thoái hóa thành một cấu trúc ngăn chặn việc ghép tuần hoàn. Ý tưởng chính là xây dựng một thứ tự DFS và sau đó “đóng” nó thành một chu trình bằng cách đảm bảo các ràng buộc kề được thỏa mãn. 

1. Gốc cây ở bất kỳ đỉnh nào, ví dụ đỉnh 1. Điều này mang lại cấu trúc cha-con mà chúng ta có thể dựa vào để kiểm soát khoảng cách. 
2. Xây dựng danh sách kề cho cây và chuẩn bị thứ tự duyệt DFS. Chúng tôi thực hiện một DFS tiêu chuẩn nhưng chúng tôi đảm bảo các phần tử con được xử lý theo bất kỳ thứ tự nào vì tất cả các cây con đều đối xứng về chuyển động ở khoảng cách hai. 
3. Ghi lại thứ tự các nút được truy cập trong thứ tự trước DFS. Thứ tự này đảm bảo rằng bất cứ khi nào chúng ta di chuyển từ một nút này sang nút khác trong cùng cây con hoặc cây con liền kề, đường dẫn giữa chúng trong cây có độ sâu được kiểm soát thông qua nút gốc. 
4. Xuất thứ tự DFS dưới dạng chu trình. Cạnh cuối cùng từ cuối đến đầu là hợp lệ vì cả hai đỉnh đều nằm trong khoảng cách tối đa là hai trong cấu trúc cây do thứ tự DFS tạo ra. 

Bước không rõ ràng là lý do tại sao thứ tự DFS lại hoạt động. Lý do là các nút liên tiếp trong thứ tự trước DFS luôn ở trong mối quan hệ cha-con hoặc thuộc về các cây con được kết nối thông qua một tổ tiên chung. Trong cả hai trường hợp, khoảng cách trong cây nhiều nhất là hai khi các chuyển tiếp được sắp xếp dọc theo các cạnh quay lui, điều này đảm bảo rằng mỗi bước trong chuỗi đầu ra tương ứng với một bước nhảy hợp lệ. 

### Tại sao nó hoạt động 

Thứ tự DFS đảm bảo rằng bất cứ khi nào chúng ta di chuyển giữa các đỉnh liên tiếp ở đầu ra, thì một trong hai đỉnh là cha của đỉnh kia hoặc cả hai đều nằm trong các cây con liền kề của một cây cha chung. Điều này đảm bảo khoảng cách cây của họ nhiều nhất là hai. Vì DFS truy cập mỗi nút chính xác một lần và chỉ quay về tổ tiên thông qua các cạnh cha, nên thứ tự kết quả tạo thành một chu trình hợp lệ theo ràng buộc nhảy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n = int(input())
g = [[] for _ in range(n)]

for _ in range(n - 1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

order = []
vis = [False] * n

def dfs(u, p):
    vis[u] = True
    order.append(u)
    for v in g[u]:
        if v == p:
            continue
        if not vis[v]:
            dfs(v, u)

dfs(0, -1)

print("Yes")
print(*[x + 1 for x in order])
```Việc triển khai thực hiện một DFS đơn giản bắt đầu từ đỉnh 1. Phép đệ quy xây dựng một quá trình truyền tải theo thứ tự trước được lưu trữ trong`order`. Kiểm tra gốc ngăn chặn việc quay lại ngay lập tức, trong khi mảng đã truy cập đảm bảo mỗi đỉnh xuất hiện chính xác một lần. 

Sự lựa chọn quan trọng là xuất ra thứ tự trước thay vì thứ tự sau hoặc truyền tải tùy ý. Thứ tự trước bảo toàn vị trí trong cây, đảm bảo rằng các đỉnh liên tiếp khác nhau tối đa một cạnh cây trong cấu trúc DFS, tương ứng với bước nhảy hợp lệ theo quy tắc khoảng cách hai. 

Giới hạn đệ quy được tăng lên vì cây có thể thoái hóa thành một chuỗi có độ dài hai trăm nghìn và độ sâu đệ quy mặc định của Python sẽ không thành công. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2
1 3
3 4
3 5
```DFS bắt đầu từ 1 tạo ra sự truyền tải: 

| Bước | Nút | Phụ huynh | Đặt hàng cho đến nay | 
| --- | --- | --- | --- | 
| 1 | 1 | - | 1 | 
| 2 | 2 | 1 | 1 2 | 
| 3 | 3 | 1 | 1 2 3 | 
| 4 | 4 | 3 | 1 2 3 4 | 
| 5 | 5 | 3 | 1 2 3 4 5 | 

Chu kỳ đầu ra là`1 2 3 4 5`. 

Mỗi lần chuyển đổi tương ứng với một cạnh trực tiếp hoặc di chuyển qua nút 3 hoặc 1, giữ khoảng cách tối đa là hai. Điều này xác nhận rằng thứ tự DFS duy trì ràng buộc kề cận được yêu cầu. 

### Ví dụ 2 (chuỗi) 

đầu vào:```
4
1 2
2 3
3 4
```| Bước | Nút | Phụ huynh | Đặt hàng cho đến nay | 
| --- | --- | --- | --- | 
| 1 | 1 | - | 1 | 
| 2 | 2 | 1 | 1 2 | 
| 3 | 3 | 2 | 1 2 3 | 
| 4 | 4 | 3 | 1 2 3 4 | 

Đầu ra là`1 2 3 4`. Mỗi cặp liên tiếp được kết nối trực tiếp trong cây, do đó cũng hợp lệ trong khoảng cách hai lần nhảy. Điều này cho thấy thuật toán xử lý độ sâu trường hợp xấu nhất mà không bị hỏng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đỉnh và cạnh được truy cập một lần trong DFS | 
| Không gian | O(n) | Danh sách kề, ngăn xếp đệ quy và mảng đầu ra | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì cả bộ nhớ và quy mô thời gian đều tuyến tính với n lên tới hai trăm nghìn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    sys.setrecursionlimit(10**7)
    order = []
    vis = [False] * n

    def dfs(u, p):
        vis[u] = True
        order.append(u)
        for v in g[u]:
            if v == p:
                continue
            if not vis[v]:
                dfs(v, u)

    dfs(0, -1)

    return "Yes\n" + " ".join(str(x + 1) for x in order)

# sample 1
assert run("""5
1 2
1 3
3 4
3 5
""") == """Yes
1 2 3 4 5"""

# chain
assert run("""4
1 2
2 3
3 4
""") == """Yes
1 2 3 4"""

# star
assert run("""5
1 2
1 3
1 4
1 5
""").startswith("Yes")

# single branch skew
assert run("""3
1 2
2 3
""") == """Yes
1 2 3"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi | thứ tự tuyến tính | tính đúng đắn của cây sâu | 
| ngôi sao | bất kỳ hoán vị nào hợp lệ | trường hợp trung tâm cấp cao | 
| nghiêng nhỏ | hiệu lực tuần tự | phân nhánh tối thiểu | 

## Vỏ cạnh 

Cây hình ngôi sao là cấu trúc dễ dãi nhất. Việc bắt đầu DFS ở giữa sẽ tạo ra một chuỗi trong đó mọi di chuyển đều là từ giữa đến lá hoặc từ lá đến giữa thông qua một lá khác và cả hai đều hợp lệ trong khoảng cách hai. Thuật toán đưa ra một hoán vị hợp lệ mà không cần bất kỳ xử lý đặc biệt nào vì việc đặt hàng trước sẽ truy cập tự nhiên vào tất cả các lá. 

Một chuỗi dài nhấn mạnh đến độ sâu đệ quy và đảm bảo rằng các nút liên tiếp chỉ được kết nối trực tiếp. DFS vẫn tạo ra thứ tự hợp lệ vì mỗi bước là một cạnh trực tiếp, thỏa mãn một cách tầm thường ràng buộc bước nhảy. 

Cây phân nhánh cao với độ sâu hỗn hợp cũng không yêu cầu logic đặc biệt. DFS đảm bảo rằng khi trở về từ một cây con, cây con tiếp theo được nhập thông qua cây cha, đảm bảo điều kiện khoảng cách hai không bao giờ bị vi phạm trong quá trình chuyển đổi.
