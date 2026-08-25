---
title: "CF 104328B - John và AndMax"
description: "Chúng ta được cung cấp một biểu đồ tuần hoàn có hướng trong đó mỗi đỉnh mang một giá trị nguyên 20 bit. Nhiệm vụ là chọn một đường dẫn di chuyển dọc theo các cạnh được định hướng, sử dụng chính xác các đỉnh $k$ và tính điểm được xác định là AND theo bit của tất cả các giá trị dọc theo đường dẫn."
date: "2026-07-01T19:03:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104328
codeforces_index: "B"
codeforces_contest_name: "FIICode2023"
rating: 0
weight: 104328
solve_time_s: 78
verified: true
draft: false
---

[CF 104328B – John và AndMax](https://codeforces.com/problemset/problem/104328/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ tuần hoàn có hướng trong đó mỗi đỉnh mang một giá trị nguyên 20 bit. Nhiệm vụ là chọn một đường di chuyển dọc theo các cạnh được định hướng, sử dụng chính xác$k$các đỉnh và tính điểm được xác định là AND theo từng bit của tất cả các giá trị dọc theo đường dẫn. Trong số tất cả các đường dẫn hợp lệ có độ dài$k$, chúng tôi muốn kết quả tối đa có thể có của AND này. 

Vì vậy, vấn đề không phải là tìm đường đi ngắn nhất hoặc dài nhất theo nghĩa cổ điển, mà là chọn đường đi có độ dài ràng buộc để bảo toàn càng nhiều bit tập hợp chung càng tốt trên tất cả các đỉnh đã chọn. 

Khó khăn chính xuất phát từ sự tương tác giữa cấu trúc và hoạt động theo bit. Biểu đồ hạn chế chuyển tiếp, trong khi thao tác AND phụ thuộc nhiều vào các đỉnh xuất hiện cùng nhau. Một bit 0 duy nhất ở bất kỳ đỉnh nào được chọn sẽ phá hủy vĩnh viễn bit đó trong kết quả cuối cùng. 

Các ràng buộc rất lớn: lên tới$2 \cdot 10^5$các đỉnh và các cạnh và$k$cũng có thể lớn như$n$. Bất kỳ giải pháp nào cố gắng liệt kê các đường dẫn hoặc duy trì trạng thái trên mỗi đường dẫn sẽ thất bại vì ngay cả việc lưu trữ tất cả các đường dẫn một phần cũng theo cấp số nhân trong$k$và thậm chí DP trên tất cả các đường dẫn mà không nén sẽ dẫn đến$O(nk)$đó là đường biên giới nhưng vẫn còn quá lớn$m$cũng vậy. 

Cấu trúc là một DAG là rất quan trọng. Nó đảm bảo không có chu kỳ, vì vậy chúng ta có thể xử lý các đỉnh theo thứ tự tôpô và đảm bảo rằng bất kỳ đường đi nào cũng có độ dài tối đa$n$. 

Một trường hợp thất bại tinh tế đối với những cách tiếp cận ngây thơ là cho rằng tính tham lam có tác dụng cục bộ. Ví dụ: việc chọn ở mỗi bước lân cận có giá trị tối đa không hoạt động vì đỉnh tối ưu cục bộ có thể loại bỏ các bit cần thiết cho việc tiếp tục trong tương lai. 

Một trường hợp thất bại khác là coi nó như đường đi dài nhất DP với trọng số vô hướng. Ở đây, “trọng số” không mang tính chất cộng hoặc đơn điệu, vì vậy việc hợp nhất các bài toán con bằng cách sử dụng một giá trị tốt nhất cho mỗi nút là không đủ. Chúng ta phải nhớ nhiều thông tin hơn là chỉ ghi một điểm tốt nhất cho mỗi đỉnh. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là xác định DP trên các đường dẫn: với mỗi đỉnh và mọi độ dài, hãy tính giá trị AND tốt nhất có thể có của đường dẫn kết thúc tại đỉnh đó với độ dài đó. Quá trình chuyển đổi sẽ xem xét tất cả các cạnh đến. Điều này dẫn đến sự tái diễn của hình thức$dp[v][t] = \max_{u \to v}(dp[u][t-1] \& a_v)$. 

Điều này đúng nhưng quá chậm. Không gian trạng thái là$O(nk)$và mỗi quá trình chuyển đổi sẽ quét các cạnh đến, tạo ra$O(mk)$, trong trường hợp xấu nhất đạt đến$2 \cdot 10^{10}$hoạt động. 

Quan sát quan trọng là không gian giá trị không phải là tùy ý. Mỗi số có nhiều nhất là 20 bit, do đó có nhiều nhất$2^{20}$có thể có mặt nạ và thao tác AND chỉ xóa bit. Điều này gợi ý một cấu trúc cấp độ bit: thay vì tính toán các giá trị DP chính xác, chúng tôi cố gắng xây dựng câu trả lời một cách tham lam từ mặt nạ cao nhất có thể trở xuống. 

Chúng tôi đảo ngược quan điểm: thay vì hỏi “điều gì là AND tốt nhất cho mỗi đường dẫn”, chúng tôi hỏi “chúng ta có thể đạt được một mặt nạ nhất định làm AND của đường dẫn có độ dài k không?”. Nếu một mặt nạ có thể đạt được thì tất cả các mặt nạ con của nó cũng có thể đạt được. Tính đơn điệu này cho phép tìm kiếm nhị phân hoặc xây dựng bit tham lam. 

Chúng tôi xây dựng câu trả lời từng chút một từ quan trọng nhất đến ít quan trọng nhất. Ở mỗi bước, chúng ta tạm ép một bit lên 1 và kiểm tra xem có tồn tại một đường đi có độ dài không$k$chỉ sử dụng các đỉnh có giá trị chứa tất cả các bit hiện cố định. Việc kiểm tra làm giảm khả năng tiếp cận DP bị ràng buộc trên DAG, trong đó chúng tôi chỉ truyền qua các đỉnh được phép. 

Bởi vì mỗi cuộc kiểm tra tính khả thi đều$O(n + m)$và chúng tôi thực hiện điều này với tối đa 20 bit, tổng độ phức tạp sẽ trở thành$O(20(n + m))$, hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DP qua đường dẫn |$O(mk)$|$O(nk)$| Quá chậm | 
| Tính khả thi tham lam của Bitmask đối với DAG |$O(20(n + m))$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích lại vấn đề khi xây dựng mặt nạ bit tối đa có thể xuất hiện dưới dạng AND của đường dẫn có độ dài k hợp lệ. 

### Các bước 

1. Tính toán thứ tự tôpô của DAG. 

Điều này đảm bảo chúng tôi có thể truyền bá thông tin đường dẫn trong một lần chuyển tiếp mà không cần xem lại các nút. 
2. Khởi tạo mặt nạ trả lời là 0. 

Chúng tôi sẽ cố gắng đặt bit từ cao xuống thấp. 
3. Đối với mỗi bit từ 19 xuống 0, hãy thử đặt nó vào mặt nạ trả lời. 

Chúng tôi tạm thời xác định mặt nạ ứng cử viên bao gồm tất cả các bit cố định trước đó cộng với bit mới này. 
4. Lọc các đỉnh tương thích với mặt nạ ứng cử viên, nghĩa là tất cả các bit trong ứng cử viên đều có trong giá trị đỉnh. 

Bất kỳ đỉnh nào không đạt yêu cầu này đều không thể xuất hiện trong đường dẫn hợp lệ dưới mặt nạ này. 
5. Chạy DP trên DAG theo thứ tự tôpô để tính toán độ dài đường dẫn tối đa có thể đạt được bắt đầu từ mỗi nút hợp lệ. 

Đối với mỗi nút, nếu nó hợp lệ thì độ dài chuỗi tốt nhất của nó là 1 cộng với mức tối đa trên tất cả các nút lân cận đi cũng hợp lệ. 
6. Nếu bất kỳ nút nào đạt được độ dài đường dẫn ít nhất$k$, thì mặt nạ ứng cử viên là khả thi nên chúng tôi giữ lại bit. Nếu không, chúng tôi loại bỏ nó. 
7. Sau khi xử lý tất cả các bit, xuất mặt nạ đã xây dựng. 

DP bên trong mỗi lần kiểm tra tính khả thi là bước tính toán quan trọng. Về cơ bản nó hỏi: trong đồ thị con được tạo ra bởi các đỉnh cho phép, có tồn tại một đường đi có độ dài ít nhất$k$? Bởi vì biểu đồ là DAG, điều này làm giảm vấn đề đường dẫn dài nhất trong DAG bị giới hạn ở các nút hợp lệ. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến ở mỗi bước, mặt nạ hiện tại biểu thị một giá trị có thể đạt được bằng ít nhất một đường dẫn có độ dài hợp lệ$k$. Mỗi lần kiểm tra tính khả thi đều đảm bảo rằng việc thêm một bit mới không phá hủy thuộc tính này. Vì AND chỉ loại bỏ các bit và không bao giờ giới thiệu chúng, nên bất kỳ phần mở rộng nào của mặt nạ chỉ làm cho tập đỉnh nhỏ hơn chứ không bao giờ lớn hơn. Do đó, tính khả thi là đơn điệu đối với việc loại bỏ bit, điều này biện minh cho việc xây dựng tham lam từ các bit cao trở xuống. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(mask, n, adj, topo, a, k):
    valid = [False] * n
    for i in range(n):
        if (a[i] & mask) == mask:
            valid[i] = True

    dp = [0] * n
    best = 0

    for u in topo:
        if not valid[u]:
            continue
        dp[u] = max(dp[u], 1)
        for v in adj[u]:
            if valid[v]:
                if dp[u] + 1 > dp[v]:
                    dp[v] = dp[u] + 1
        best = max(best, dp[u])

    return best >= k

def topo_sort(n, adj):
    indeg = [0] * n
    for u in range(n):
        for v in adj[u]:
            indeg[v] += 1

    stack = [i for i in range(n) if indeg[i] == 0]
    topo = []

    while stack:
        u = stack.pop()
        topo.append(u)
        for v in adj[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                stack.append(v)

    return topo

def solve():
    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))

    adj = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        adj[u].append(v)

    topo = topo_sort(n, adj)

    ans = 0
    for b in range(19, -1, -1):
        cand = ans | (1 << b)
        if can(cand, n, adj, topo, a, k):
            ans = cand

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên xây dựng một trật tự tôpô sao cho tất cả các chuyển đổi đều tuân theo hướng DAG. Điều này là cần thiết vì tính khả thi của DP phụ thuộc vào việc xử lý các nút theo thứ tự phụ thuộc mà không cần xem lại trạng thái. 

các`can`hàm thực thi ràng buộc mặt nạ bit hiện tại bằng cách lọc các đỉnh. Bất kỳ nút nào thiếu bit bắt buộc đều bị loại trừ hoàn toàn. Sau đó, DP giống như đường dẫn dài nhất sẽ chạy qua DAG nhưng chỉ giữa các nút hợp lệ. Nếu bất kỳ nút nào đạt đến độ dài chuỗi ít nhất$k$, chúng tôi chấp nhận mặt nạ. 

Vòng lặp bên ngoài tham lam xây dựng câu trả lời từ bit cao nhất đến bit thấp nhất, đảm bảo bitmask tối đa về mặt từ điển. 

Một điểm tinh tế là chúng ta không cần theo dõi các đường dẫn chính xác, chỉ cần theo dõi chuỗi hợp lệ dài nhất. Vì chúng tôi chỉ quan tâm liệu độ dài-$k$đường dẫn tồn tại, giá trị DP vượt quá$k$không liên quan. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi bắt đầu với mặt nạ 0 và cố gắng kích hoạt các bit từ 19 trở xuống. 

Tại mỗi mặt nạ ứng cử viên, chúng tôi kiểm tra xem có tồn tại đường dẫn có độ dài 4 hợp lệ chỉ sử dụng các nút tương thích hay không. 

| Bước | Đã thử mặt nạ | Các nút hợp lệ tồn tại | Con đường dài nhất được tìm thấy | Quyết định | 
| --- | --- | --- | --- | --- | 
| b=19..cao | 0 | tất cả các nút | ≥4 | giữ | 
| bit cao hơn | khác nhau | hạn chế | <4 hoặc ≥4 | chọn lọc | 
| cuối cùng | 10 | tồn tại chuỗi hợp lệ | 4 | chấp nhận | 

Mặt nạ được xây dựng cuối cùng là 10. 

Dấu vết này cho thấy các hạn chế trung gian có thể loại bỏ nhiều nút nhưng vẫn bảo toàn được chuỗi đủ dài. 

### Mẫu 2 

Chúng tôi lặp lại cùng một công trình tham lam. 

| Bước | Đã thử mặt nạ | Các nút hợp lệ tồn tại | Con đường dài nhất được tìm thấy | Quyết định | 
| --- | --- | --- | --- | --- | 
| bắt đầu | 0 | tất cả các nút | ≥4 | giữ | 
| bit cao | hạn chế dần dần | khác nhau | đôi khi <4 | vứt bỏ | 
| cuối cùng | 32 | tồn tại chuỗi hợp lệ | 4 | chấp nhận | 

Điều này chứng tỏ rằng các bit cao hơn có thể tồn tại ngay cả khi được lọc mạnh, miễn là vẫn còn đường dẫn DAG tương thích. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(20(n + m))$| Mỗi 20 bit kích hoạt một DAG DP trên tất cả các nút và cạnh | 
| Không gian |$O(n + m)$| danh sách kề, thứ tự topo và mảng DP | 

Sự phức tạp phù hợp thoải mái trong giới hạn vì$n, m \le 2 \cdot 10^5$. Ngay cả trong trường hợp xấu nhất, chúng tôi thực hiện khoảng 4 triệu lần thư giãn cạnh, chỉ trong vòng 1 giây trong Python được tối ưu hóa với danh sách kề. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def topo_sort(n, adj):
        indeg = [0]*n
        for u in range(n):
            for v in adj[u]:
                indeg[v]+=1
        stack = [i for i in range(n) if indeg[i]==0]
        topo=[]
        while stack:
            u=stack.pop()
            topo.append(u)
            for v in adj[u]:
                indeg[v]-=1
                if indeg[v]==0:
                    stack.append(v)
        return topo

    def can(mask, n, adj, topo, a, k):
        valid=[False]*n
        for i in range(n):
            if (a[i]&mask)==mask:
                valid[i]=True
        dp=[0]*n
        best=0
        for u in topo:
            if not valid[u]: continue
            dp[u]=max(dp[u],1)
            best=max(best,dp[u])
            for v in adj[u]:
                if valid[v]:
                    dp[v]=max(dp[v], dp[u]+1)
        return best>=k

    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))
    adj = [[] for _ in range(n)]
    for _ in range(m):
        u,v = map(int, input().split())
        adj[u-1].append(v-1)

    topo = topo_sort(n, adj)
    ans=0
    for b in range(19,-1,-1):
        if can(ans|(1<<b), n, adj, topo, a, k):
            ans|=(1<<b)
    return str(ans).strip()

# provided samples
assert run("""5 8 4
11 26 15 3 26
1 5
2 3
2 5
3 1
3 5
4 1
4 3
4 5
""") == "10"

assert run("""7 12 4
36 47 47 31 33 15 34
1 6
1 7
2 4
2 5
3 2
3 7
4 1
4 5
4 6
5 7
6 5
6 7
""") == "32"

# custom cases
assert run("""2 1 2
3 3
1 2
""") == "3", "minimum chain"

assert run("""3 2 3
7 7 7
1 2
2 3
""") == "7", "all equal values"

assert run("""4 3 2
8 4 2 1
1 2
2 3
3 4
""") == "0", "no common bit survives"

assert run("""5 4 3
31 31 31 31 31
1 2
2 3
3 4
4 5
""") == "31", "full preservation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi dài 2 | 3 | tính khả thi có độ dài tối thiểu | 
| tất cả các giá trị bằng nhau | 7 | tính chính xác lan truyền đầy đủ | 
| bit giảm nghiêm ngặt | 0 | loại bỏ hoàn toàn bit | 
| DAG hoàn toàn thống nhất | 31 | duy trì mặt nạ tối đa | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi chỉ có một đường đi có độ dài$k$tồn tại và tất cả các nút khác không tương thích với các bit cao hơn. Thuật toán xử lý vấn đề này một cách chính xác vì tính khả thi phụ thuộc vào sự tồn tại của bất kỳ chuỗi hợp lệ nào chứ không phải mật độ toàn cầu. 

Ví dụ:```
4 3 3
7 6 7 7
1 2
2 3
3 4
```Khi kiểm tra bit cao bị thiếu trong nút 2, nút đó sẽ bị loại trừ và chuỗi bị đứt, khiến DP không hoạt động đối với mặt nạ đó. Thuật toán sau đó loại bỏ chính xác bit đó và tiếp tục đi xuống. 

Một trường hợp cạnh khác là khi có nhiều đường dẫn tồn tại nhưng chỉ có một đường giữ lại một bit hiếm. Vì DP tính toán độ dài đường dẫn tối đa trên tất cả các nút hợp lệ nên nó sẽ tự nhiên chọn đường dẫn hiếm đó nếu nó đạt đến độ dài$k$, đảm bảo tính chính xác mà không cần theo dõi đường dẫn rõ ràng.
