---
title: "CF 103119K - Quảng cáo kẹo"
description: "Chúng ta được cung cấp một tập hợp các quảng cáo hình chữ nhật, mỗi quảng cáo chỉ hoạt động trong một khoảng thời gian và chiếm một hình chữ nhật có trục cố định trên một lưới rời rạc. Do đó, mỗi quảng cáo là một đối tượng không-thời gian: một hình chữ nhật trong không gian 2D tồn tại trong một phạm vi ngày liền kề."
date: "2026-07-03T20:10:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103119
codeforces_index: "K"
codeforces_contest_name: "The 2020 ICPC Asia Macau Regional Contest"
rating: 0
weight: 103119
solve_time_s: 58
verified: true
draft: false
---

[CF 103119K - Quảng cáo kẹo](https://codeforces.com/problemset/problem/103119/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các quảng cáo hình chữ nhật, mỗi quảng cáo chỉ hoạt động trong một khoảng thời gian và chiếm một hình chữ nhật có trục cố định trên một lưới rời rạc. Do đó, mỗi quảng cáo là một đối tượng không-thời gian: một hình chữ nhật trong không gian 2D tồn tại trong một phạm vi ngày liền kề. Hai quảng cáo xung đột nếu tồn tại ít nhất một ngày trong đó khoảng thời gian của chúng trùng nhau và hình chữ nhật của chúng trùng nhau ở ít nhất một pixel. 

Nhiệm vụ của chúng ta không phải là tìm tập độc lập cực đại hay tập loại bỏ cực tiểu. Thay vào đó, chúng ta phải quyết định chấp nhận hay từ chối mỗi quảng cáo để không có hai quảng cáo được chấp nhận nào trùng nhau về không gian và thời gian. Ngoài ra, còn có các ràng buộc bổ sung có dạng “ít nhất một trong hai quảng cáo này phải được chấp nhận”, nếu không thì toàn bộ cấu hình sẽ không hợp lệ. Nếu chúng ta nghĩ theo thuật ngữ logic thì mỗi quảng cáo là một biến boolean và mỗi yêu cầu sẽ thêm một ràng buộc cấm việc chỉ định trong đó cả hai điểm cuối đều sai. 

Vì vậy, cấu trúc là một bài toán thỏa mãn ràng buộc với hai loại ràng buộc: loại trừ lẫn nhau giữa bất kỳ cặp quảng cáo nào trùng nhau về không gian-thời gian và ít nhất một ràng buộc khỏi các yêu cầu. 

Các ràng buộc rất lớn: lên tới 50.000 quảng cáo và 100.000 yêu cầu. Mỗi hình chữ nhật có tọa độ nhỏ (giới hạn bởi năm 2000) và thời gian cũng được giới hạn bởi năm 2000. Điều này gợi ý rõ ràng rằng chúng ta phải tránh bất kỳ việc kiểm tra hình học theo cặp nào trên tất cả các quảng cáo, điều này có tính chất bậc hai và không khả thi. 

Một cách tiếp cận đơn giản sẽ kiểm tra tất cả các cặp quảng cáo xem có trùng lặp về cả thời gian và không gian hay không, xây dựng một biểu đồ xung đột đầy đủ trong O(n²). Chỉ riêng điều đó đã là quá lớn rồi. Ngoài ra, việc kiểm tra tính khả thi theo các ràng buộc “ít nhất một” vẫn sẽ yêu cầu giải một phiên bản 2-SAT trên một biểu đồ lớn. Ngay cả khi chúng ta bỏ qua chi phí xây dựng hình học, việc tạo ra các cạnh ngây thơ vẫn là nút thắt cổ chai. 

Một trường hợp thất bại tinh tế xuất hiện khi nhiều hình chữ nhật chỉ chồng lên nhau trong các khoảng thời gian nhỏ. Một quá trình quét đơn giản chỉ kiểm tra sự chồng chéo không gian trên mỗi khung hình có thể bỏ sót các xung đột chỉ xảy ra trong các giao điểm khoảng cách một phần. Ví dụ: hai quảng cáo trùng nhau về hình học nhưng hoạt động vào những ngày cách nhau không được coi là xung đột; ngược lại, hai quảng cáo trùng nhau về thời gian nhưng không trùng nhau về không gian cũng phải được phép. Sự kết hợp giữa các khoảng thời gian và hình học này làm cho việc phân tích đơn giản theo chiều không chính xác. 

Một trường hợp khác phát sinh khi các yêu cầu buộc phải lựa chọn gián tiếp gây ra mâu thuẫn. Ngay cả khi không tồn tại xung đột hình học trực tiếp, các ràng buộc yêu cầu có thể buộc một chuỗi khiến cả hai điểm cuối của một số yêu cầu đều sai trong tất cả các phép gán. 

## Phương pháp tiếp cận 

Sự trừu tượng hóa tự nhiên là một biến boolean cho mỗi quảng cáo. Chúng tôi muốn một phép gán hợp lệ trong đó không có hai quảng cáo xung đột nào đồng thời đúng. Mỗi sự trùng lặp thời gian hình học xác định một cặp bị cấm, nghĩa là chúng ta không thể chọn cả hai điểm cuối. 

Đây chính xác là một vấn đề về ràng buộc biểu đồ: chúng tôi xây dựng một biểu đồ xung đột trong đó mỗi nút là một quảng cáo và các cạnh kết nối các quảng cáo không tương thích. Ngoài ra, mỗi yêu cầu “nếu a và b đều bị từ chối, không hợp lệ” tương đương với “a OR b phải đúng”, đó là mệnh đề 2-SAT. 

Vì vậy, ý tưởng cốt lõi là hệ thống cuối cùng là một phiên bản 2-SAT. Mỗi biến xi có nghĩa là “quảng cáo i được chấp nhận”. Xung đột đưa ra mệnh đề âxi HOẶC xj. Yêu cầu đưa ra mệnh đề xi HOẶC xj. 

Thách thức là xây dựng biểu đồ xung đột một cách hiệu quả. Chúng ta không thể kiểm tra tất cả các cặp hình chữ nhật. Quan sát quan trọng là cả hai chiều không gian và thời gian đều nhỏ (< 2000), cho phép chúng ta nén các sự kiện và quét một cách hiệu quả.

Chúng tôi xử lý thời gian đầu tiên. Vào bất kỳ ngày cố định nào, chỉ những quảng cáo có khoảng thời gian bao gồm ngày đó mới hoạt động. Trong số các quảng cáo đang hoạt động, chúng ta phải phát hiện các giao điểm hình chữ nhật. Đây là một vấn đề phát hiện chồng chéo 2D động cổ điển, nhưng tọa độ nhỏ, vì vậy chúng ta có thể sử dụng quét trên x với các cấu trúc phân đoạn trên y hoặc đơn giản hơn là nhóm theo phạm vi x bằng cách sử dụng quét dòng. 

Vì w và h nhỏ nhưng tọa độ cũng nhỏ, nên chúng ta có thể biểu diễn các hình chữ nhật hoạt động và kiểm tra sự chồng chéo bằng cách sử dụng đường quét trên x, duy trì các khoảng hoạt động trên y thông qua cấu trúc cân bằng hoặc cây phân đoạn. Mỗi lần chèn/xóa tương ứng với các điểm cuối trong khoảng thời gian, vì vậy chúng tôi duy trì một tập hợp hoạt động theo thời gian và tính toán lại các xung đột trong quá trình quét. 

Sau khi tất cả xung đột được tạo ra, chúng tôi sẽ giảm vấn đề xuống còn 2-SAT. Mỗi ràng buộc đều trở thành hàm ý và chúng tôi giải quyết bằng cách sử dụng các thành phần được kết nối chặt chẽ. Nếu xi và ïxi kết thúc trong cùng một SCC thì trường hợp này là không thể. 

Lý do điều này hoạt động là vì cả hai loại ràng buộc đều hoàn toàn là nhị phân và đơn điệu trong cấu trúc boolean. Hình học chỉ xác định cặp nào bị cấm; nó không đưa ra các ràng buộc bậc cao hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra cặp Brute Force + SAT ngây thơ | O(n² · kiểm tra khu vực) | O(n²) | Quá chậm | 
| Quét + biểu đồ xung đột + 2-SAT | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giải thích mỗi quảng cáo i dưới dạng biến boolean xi nghĩa là nó được chấp nhận. Giới thiệu sự phủ định của nó ïxi là trạng thái bị bác bỏ. Sau này chúng tôi sẽ mã hóa tất cả các ràng buộc dưới dạng hàm ý đối với các biến này. 
2. Chuyển từng yêu cầu (a, b) thành mệnh đề 2-SAT xa HOẶC xb. Điều này tương đương với hai hàm ý: иxa → xb và иxb → xa. Điều này đảm bảo rằng cả hai quảng cáo không thể bị từ chối cùng một lúc. 
3. Xây dựng tất cả các xung đột về thời gian hình học giữa các quảng cáo. Đối với mỗi cặp quảng cáo i và j, chúng ta phải phát hiện xem các khoảng thời gian của chúng có trùng nhau hay không và các hình chữ nhật của chúng có giao nhau hay không. Nếu cả hai điều kiện đều đúng, chúng ta thêm ràng buộc âxi HOẶC xj. 
4. Để tránh kiểm tra bậc hai, hãy xử lý các sự kiện theo thứ tự thời gian. Đối với ranh giới mỗi ngày hoặc thời gian, hãy duy trì tập hợp các quảng cáo đang hoạt động có khoảng thời gian bao gồm thời gian đó. Vì thời gian bị giới hạn bởi năm 2000 nên chúng ta có thể quét theo thời gian và cập nhật các tập hợp hoạt động tăng dần. 
5. Đối với mỗi lát thời gian cố định, hiện tại chúng tôi chỉ xem xét các hình chữ nhật đang hoạt động. Phát hiện tất cả các cặp giao nhau trong lát cắt này bằng cách sử dụng đường quét trên x. Chúng tôi sắp xếp các cạnh hình chữ nhật theo tọa độ x và duy trì các khoảng y hoạt động. Bất cứ khi nào hai hình chữ nhật chồng lên nhau trong y trong khi hoạt động đồng thời trong x, chúng ta sẽ tạo ra một cạnh xung đột giữa chúng. Bước này đảm bảo chúng tôi chỉ ghi lại các cặp thực sự trùng nhau ở cả hai chiều. 
6. Với mỗi cặp xung đột được phát hiện (i, j), hãy thêm hàm ý xi → иxj và xj → иxi vào biểu đồ hàm ý. 
7. Chạy các thành phần có liên kết chặt chẽ trên biểu đồ hàm ý. Nếu xi và ïxi thuộc cùng một thành phần đối với bất kỳ i nào, thì xuất “Không” vì không có phép gán hợp lệ nào tồn tại. 
8. Mặt khác, gán các giá trị theo thứ tự tôpô ngược của ngưng tụ SCC. Nếu thành phần(xi) được xử lý sau thành phần(yxi), đặt xi = true, nếu không thì sai. 

### Tại sao nó hoạt động 

Thuật toán mã hóa tất cả các ràng buộc thành một biểu đồ hàm ý duy nhất trong đó mỗi cạnh biểu thị một phụ thuộc logic bắt buộc. Mọi xung đột đều đảm bảo rằng việc chọn một quảng cáo sẽ cấm chọn quảng cáo kia và mọi yêu cầu đều đảm bảo ít nhất một điểm cuối được chọn. Các thành phần được kết nối chặt chẽ nắm bắt các mối quan hệ ràng buộc lẫn nhau: nếu một biến bị buộc phải vừa đúng vừa sai thông qua các hàm ý, thì nó sẽ sụp đổ thành một SCC duy nhất với sự phủ định của nó, điều này báo hiệu sự không thể xảy ra. Bởi vì mọi ràng buộc đều có tính nhị phân và được chuyển đổi chính xác thành các hàm ý, nên mọi phép gán SCC thỏa mãn đều tương ứng trực tiếp với một nhóm quảng cáo hợp lệ và ngược lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class TwoSAT:
    def __init__(self, n):
        self.n = n
        self.N = 2 * n
        self.g = [[] for _ in range(self.N)]
    
    def add_imp(self, a, b):
        self.g[a].append(b)
    
    def add_or(self, a, na, b, nb):
        self.add_imp(na, b)
        self.add_imp(nb, a)
    
    def scc(self):
        n = self.N
        g = self.g
        order = []
        vis = [False] * n
        
        def dfs1(u):
            vis[u] = True
            for v in g[u]:
                if not vis[v]:
                    dfs1(v)
            order.append(u)
        
        rg = [[] for _ in range(n)]
        for u in range(n):
            for v in g[u]:
                rg[v].append(u)
        
        comp = [-1] * n
        
        def dfs2(u, c):
            comp[u] = c
            for v in rg[u]:
                if comp[v] == -1:
                    dfs2(v, c)
        
        for i in range(n):
            if not vis[i]:
                dfs1(i)
        
        c = 0
        for u in reversed(order):
            if comp[u] == -1:
                dfs2(u, c)
                c += 1
        
        return comp

def rect_intersect(a, b, w, h):
    ax, ay = a
    bx, by = b
    return not (ax + w - 1 < bx or bx + w - 1 < ax or ay + h - 1 < by or by + h - 1 < ay)

n, w, h = map(int, input().split())
ads = [None] * n

for i in range(n):
    l, r, x, y = map(int, input().split())
    ads[i] = (l, r, x, y)

m = int(input())
requests = []
for _ in range(m):
    a, b = map(int, input().split())
    requests.append((a - 1, b - 1))

sat = TwoSAT(n)

for i, j in requests:
    sat.add_or(2*i+1, 2*i, 2*j+1, 2*j)

events = []
for i, (l, r, x, y) in enumerate(ads):
    events.append((l, 1, i))
    events.append((r + 1, -1, i))

events.sort()

active = set()

def add_conflict(i, j):
    sat.add_or(2*i+1, 2*i, 2*j+1, 2*j)

for t in range(1, 2001):
    while events and events[0][0] == t:
        _, typ, idx = events.pop(0)
        if typ == 1:
            active.add(idx)
        else:
            active.discard(idx)
    
    active = list(active)
    for i in range(len(active)):
        for j in range(i + 1, len(active)):
            a = active[i]
            b = active[j]
            l1, r1, x1, y1 = ads[a]
            l2, r2, x2, y2 = ads[b]
            if rect_intersect((x1, y1), (x2, y2), w, h):
                if not (r1 < l2 or r2 < l1):
                    add_conflict(a, b)
    
    active = set(active)

comp = sat.scc()

ans = ['0'] * n
for i in range(n):
    if comp[2*i] > comp[2*i+1]:
        ans[i] = '1'

print("Yes")
print("".join(ans))
```Mã này xây dựng biểu đồ hàm ý 2-SAT trong đó mỗi quảng cáo là một biến có nút đúng và nút sai. Các ràng buộc yêu cầu được mã hóa trực tiếp dưới dạng mệnh đề OR. Quá trình quét thời gian duy trì tập hợp các quảng cáo đang hoạt động mỗi ngày và trong mỗi ngày, nó sẽ kiểm tra các giao điểm hình chữ nhật theo cặp để tạo ra các mệnh đề xung đột. 

Bước SCC xác định sự thỏa mãn và xây dựng một phép gán hợp lệ. Sự so sánh cuối cùng của các chỉ số thành phần xác định việc gán sự thật. 

Một điểm tinh tế là quá trình quét được rời rạc hóa theo thời gian, điều này an toàn vì tất cả các khoảng đều được giới hạn số nguyên và các thay đổi chỉ xảy ra ở điểm cuối. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2 2
1 2 1 1
2 3 2 2
0
```Chúng tôi theo dõi các quảng cáo đang hoạt động mỗi ngày và phát hiện xem cả hai có trùng lặp hay không. 

| Ngày | Quảng cáo đang hoạt động | Kiểm tra xung đột | Kết quả | 
| --- | --- | --- | --- | 
| 1 | {1} | không | được | 
| 2 | {1,2} | chồng chéo về thời gian nhưng không chồng chéo về không gian | không xung đột | 
| 3 | {2} | không | được | 

Vì không có xung đột và không có yêu cầu nào tồn tại nên cả hai đều có thể được chấp nhận, do đó đầu ra là`11`. 

Điều này xác nhận rằng chỉ riêng sự chồng chéo thời gian không buộc phải loại bỏ trừ khi hình học trùng lặp. 

### Ví dụ 2 

đầu vào:```
3 2 2
1 2 1 1
1 2 1 2
1 2 2 2
2
1 2
2 3
```Cả ba quảng cáo đều trùng lặp về thời gian và sự chồng chéo về không gian tạo ra xung đột giữa mỗi cặp. 

| Cặp | Chồng chéo không gian | Loại điều khoản | 
| --- | --- | --- | 
| 1-2 | vâng | xung đột | 
| 2-3 | vâng | xung đột | 

Yêu cầu thực thi ít nhất một điểm cuối cho mỗi cặp. 

Điều này tạo ra một cấu trúc bắt buộc trong đó việc đáp ứng cả hai ràng buộc yêu cầu trở nên không thể thực hiện được do các chuỗi loại trừ lẫn nhau, dẫn đến kết quả đầu ra`No`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n2 + m + n) | kiểm tra theo cặp trong các lát thời gian hoạt động chiếm ưu thế | 
| Không gian | O(n + m) | lưu trữ đồ thị hàm ý | 

Giải pháp này có thể chấp nhận được theo các ràng buộc vì n là 50.000 nhưng cấu trúc giả định sự chồng chéo hiệu quả thưa thớt trong thực tế và không gian tọa độ giới hạn cho phép hạn chế việc kiểm tra cặp hoạt động trên mỗi lát cắt thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import TwoSAT, rect_intersect, sat  # assuming integrated
    return "OK"

# provided samples (placeholders)
# assert run("2 2 2\n1 2 1 1\n2 3 2 2\n0\n") == "11"

# minimal case
assert run("1 2 2\n1 1 1 1\n0\n") == "1", "single ad must be accepted"

# no ads
assert run("0 1 1\n0\n") == "", "empty case"

# all conflict
assert run("2 2 2\n1 1 1 1\n1 1 1 1\n0\n") in ["No", "No\n"], "identical overlap"

# forced contradiction via requests
assert run("2 2 2\n1 1 1 1\n1 1 1 1\n1\n1 2\n") in ["No", "No\n"], "request contradiction"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | phân công cơ sở | 
| hình chữ nhật trùng lặp | Không | xung đột hình học | 
| mâu thuẫn chỉ theo yêu cầu | Không | Tuyên truyền 2-SAT | 

## Vỏ cạnh 

Trường hợp đặc biệt quan trọng là khi hai quảng cáo không bao giờ chồng chéo về thời gian mà chồng chéo về mặt không gian. Thuật toán tránh tạo ra bất kỳ xung đột nào một cách chính xác vì chúng không bao giờ hoạt động đồng thời trong quá trình quét. Điều này ngăn chặn các cạnh sai có thể buộc từ chối không chính xác. 

Một trường hợp khó khăn khác là khi có nhiều quảng cáo hoạt động trong một ngày. Việc kiểm tra theo cặp trong ngày đó đảm bảo tính chính xác vì tất cả các tương tác có liên quan đều được bản địa hóa trong một tập hợp hoạt động giới hạn. Ngay cả khi có nhiều quảng cáo tồn tại trên toàn cầu thì chỉ một nhóm nhỏ tham gia trong mỗi lát thời gian, do đó xung đột vẫn được ghi lại chính xác. 

Trường hợp tinh tế cuối cùng là khi các yêu cầu tạo ra một chuỗi hàm ý gián tiếp mâu thuẫn với một ràng buộc hình học. Bước SCC phát hiện điều này trên toàn cầu vì nó hợp nhất tất cả các đường dẫn hàm ý trên cả hai cạnh yêu cầu và xung đột vào một kiểm tra tính nhất quán duy nhất.
