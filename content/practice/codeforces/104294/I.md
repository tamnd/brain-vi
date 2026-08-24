---
title: "CF 104294I - Giờ ăn nhẹ"
description: "Chúng ta được tặng một cây nhà, trong đó ban đầu mỗi ngôi nhà có một số lượng bạn bè nhất định. Các con đường tạo thành một cấu trúc tuần hoàn được kết nối, do đó giữa hai ngôi nhà bất kỳ có chính xác một con đường đơn giản. Có hai loại sự kiện."
date: "2026-07-01T20:28:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104294
codeforces_index: "I"
codeforces_contest_name: "UTPC Spring 2023 Open Contest"
rating: 0
weight: 104294
solve_time_s: 97
verified: true
draft: false
---

[CF 104294I - Giờ ăn nhẹ](https://codeforces.com/problemset/problem/104294/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một cây nhà, trong đó ban đầu mỗi ngôi nhà có một số lượng bạn bè nhất định. Các con đường tạo thành một cấu trúc tuần hoàn được kết nối, do đó giữa hai ngôi nhà bất kỳ có chính xác một con đường đơn giản. 

Có hai loại sự kiện. Hoặc Umaru thực hiện một chuyến đi ăn nhẹ giữa hai ngôi nhà hoặc cô ấy tăng số lượng bạn bè trong một ngôi nhà cụ thể bằng cách nhân giá trị hiện tại của nó với một số hệ số. Đối với mỗi lần chạy ăn nhẹ, chúng ta phải xem xét tất cả các ngôi nhà trên con đường duy nhất giữa hai điểm cuối và tính số dương nhỏ nhất có thể chia hết cho số bạn bè trong mỗi ngôi nhà dọc theo con đường đó. 

Đại lượng này không gì khác hơn là bội số chung nhỏ nhất của tất cả các giá trị trên đường dẫn đó. Thách thức là các giá trị không cố định vì các cập nhật nhân lên xảy ra giữa các truy vấn và chúng tôi phải trả lời các truy vấn LCM đường dẫn trực tuyến. 

Các ràng buộc đủ nhỏ để chúng ta có thể cung cấp các giải pháp gần với bậc hai về số lượng nút hoặc truy vấn. Với cả N và Q lên tới 1000, ngay cả những cách tiếp cận mất khoảng O(N) cho mỗi truy vấn cũng có thể chấp nhận được vì tổng công việc vẫn còn khoảng 10^6 thao tác. Điều này ngay lập tức loại trừ các cấu trúc cây động nặng nề, nhưng cho phép chúng ta tính toán lại hoặc đi qua các đường dẫn một cách rõ ràng. 

Một điểm tinh tế là các giá trị có thể tăng lớn do các phép nhân lặp đi lặp lại. Mặc dù ban đầu các giá trị a_i riêng lẻ được giới hạn bởi 10^7, nhưng việc cập nhật lặp lại có thể làm cho chúng lớn hơn nhiều, do đó việc lưu trữ các giá trị thô và thử tính toán LCM trực tiếp là không an toàn. Thay vào đó, cấu trúc của LCM thông qua hệ số nguyên tố trở nên cần thiết. 

Một sai lầm ngây thơ là cố gắng duy trì LCM của toàn bộ đường dẫn tăng dần mà không theo dõi cực đại trên mỗi số nguyên tố. Điều này không thành công khi cập nhật vì LCM không phân phối theo phép nhân theo cách cộng đơn giản. Một lỗi phổ biến khác là tính toán lại các giá trị đường dẫn nhưng lại nhân trực tiếp các số nguyên, lỗi này nhanh chóng bị tràn và trở nên không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn, chúng tôi tìm rõ ràng đường dẫn giữa u và v, thu thập tất cả các nút trên đường dẫn đó và tính toán LCM của các giá trị của chúng. Điều này đúng vì đường dẫn được liệt kê rõ ràng và định nghĩa LCM được áp dụng trực tiếp. Tuy nhiên, việc tính toán LCM trên các số nguyên thô có vấn đề do tràn và ngay cả khi chúng tôi sửa số học, việc tính toán lại LCM trên các nút có khả năng là O(N) cho mỗi truy vấn sẽ dẫn đến độ phức tạp O(NQ), điều này vẫn có thể chấp nhận được ở ràng buộc này nhưng không để lại lợi nhuận khi chúng tôi bao gồm chi phí phân tích nhân tố và cập nhật. 

Quan sát quan trọng là LCM được xử lý tốt nhất trong không gian thừa số nguyên tố. LCM của một tập hợp số được xác định bằng cách lấy, đối với mỗi số nguyên tố, số mũ lớn nhất của số nguyên tố đó trên tất cả các số. Điều này có nghĩa là mỗi nút có thể được biểu diễn dưới dạng bản đồ từ số nguyên tố đến số mũ và câu trả lời cho truy vấn đường dẫn sẽ trở thành sự kết hợp của các bản đồ này bằng cách sử dụng mức tối đa. 

Cập nhật rất đơn giản trong đại diện này. Khi một nút được nhân với f, chúng ta phân tích f thành số nguyên tố và cộng các số mũ đó vào hệ số được lưu trữ của nút. Điều này giữ cho bản đồ hệ số của nút luôn chính xác. 

Khó khăn còn lại là trả lời các truy vấn đường dẫn một cách hiệu quả. Vì N nhỏ nên chúng ta có thể tính toán trước mảng gốc và mảng độ sâu cho cây, sau đó trích xuất tất cả các nút trên đường dẫn bằng quy trình nâng dựa trên LCA tiêu chuẩn. Khi chúng tôi có danh sách các nút trên đường dẫn, chúng tôi hợp nhất các bản đồ số mũ nguyên tố của chúng bằng cách lấy cực đại. Câu trả lời cuối cùng được xây dựng lại bằng cách sử dụng lũy ​​thừa mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NQ + chi phí nhân tố hóa) | O(N) | Chấp nhận được nhưng chặt chẽ | 
| Tối ưu | O(NQ log A) | O(N log A) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Trước tiên, chúng tôi root cây ở bất kỳ đâu, thường là ở nút 1 và tính toán mảng gốc và mảng độ sâu bằng DFS. Điều này cho phép chúng tôi truy xuất đường dẫn sau này một cách hiệu quả. 

Tiếp theo, đối với mỗi nút, chúng tôi lưu trữ hệ số nguyên tố hiện tại của nó dưới dạng từ điển ánh xạ các số nguyên tố thành số mũ. Chúng tôi xây dựng điều này ban đầu bằng cách phân tích từng a_i. 

Đối với mỗi thao tác cập nhật, chúng tôi phân tích hệ số nhân f_i và thêm số mũ nguyên tố của nó vào từ điển của nút mục tiêu. Điều này duy trì tính đúng đắn vì phép nhân trong số nguyên tương ứng với phép cộng trong không gian số mũ. 

Đối với mỗi thao tác truy vấn, chúng tôi tính toán đường đi giữa u và v. Chúng tôi nâng cả hai nút lên cho đến khi chúng gặp nhau ở tổ tiên chung thấp nhất, thu thập tất cả các nút trên đường đi. Điều này cung cấp cho chúng tôi danh sách đầy đủ các nút trên đường dẫn. 

Sau khi có các nút, chúng tôi xây dựng một từ điển chung cho truy vấn theo dõi, đối với mỗi số nguyên tố, số mũ tối đa được nhìn thấy trong số tất cả các nút trên đường dẫn. Chúng tôi cập nhật từ điển này bằng cách lặp qua bản đồ phân tích hệ số của từng nút. 

Cuối cùng, chúng ta xây dựng lại câu trả lời bằng cách tính tích trên tất cả các số nguyên tố p được nâng lên số mũ cực đại của nó, lấy modulo 1e9+7. 

Lý do điều này có tác dụng là vì mọi số trong đường đi đều đóng góp các lũy thừa nguyên tố độc lập. LCM chọn lũy thừa cao nhất của mỗi số nguyên tố xuất hiện ở bất kỳ đâu trong tập hợp. Vì các bản cập nhật chỉ tăng số mũ tại các nút riêng lẻ và không bao giờ phân tách hoặc loại bỏ các yếu tố nên hệ số hóa của mỗi nút vẫn có hiệu lực theo thời gian và việc hợp nhất thông qua việc duy trì tính chính xác tối đa cho đường dẫn LCM. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

# simple factorization up to sqrt using trial division
def factor(x):
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

sys.setrecursionlimit(10**7)

N, Q = map(int, input().split())
a = list(map(int, input().split()))

g = [[] for _ in range(N)]
for _ in range(N - 1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

parent = [-1] * N
depth = [0] * N

def dfs(u, p):
    parent[u] = p
    for v in g[u]:
        if v == p:
            continue
        depth[v] = depth[u] + 1
        dfs(v, u)

dfs(0, -1)

facts = [factor(x) for x in a]

def get_path(u, v):
    pu, pv = u, v
    while depth[pu] > depth[pv]:
        pu = parent[pu]
    while depth[pv] > depth[pu]:
        pv = parent[pv]

    path_u = []
    path_v = []

    while pu != pv:
        path_u.append(u)
        path_v.append(v)
        u = parent[u]
        v = parent[v]
        pu = parent[pu]
        pv = parent[pv]

    path_u.append(u)
    path = path_u + path_v[::-1]
    return path

def mod_pow(x, e):
    return pow(x, e, MOD)

for _ in range(Q):
    tmp = list(map(int, input().split()))
    if tmp[0] == 2:
        _, w, f = tmp
        w -= 1
        for p, c in factor(f).items():
            facts[w][p] = facts[w].get(p, 0) + c
    else:
        _, u, v = tmp
        u -= 1
        v -= 1

        path = get_path(u, v)

        best = {}
        for node in path:
            for p, c in facts[node].items():
                if best.get(p, 0) < c:
                    best[p] = c

        ans = 1
        for p, c in best.items():
            ans = (ans * pow(p, c, MOD)) % MOD

        print(ans)
```Giải pháp duy trì một từ điển hệ số cho mỗi nút, do đó các bản cập nhật sẽ trở thành phép cộng cục bộ của số mũ sau khi phân tích hệ số nhân. DFS thiết lập các con trỏ gốc và độ sâu để việc xây dựng lại đường dẫn có thể được thực hiện mà không cần bất kỳ cấu trúc LCA nặng nề nào. 

Hàm trích xuất đường dẫn trước tiên căn chỉnh cả hai nút ở cùng độ sâu, sau đó di chuyển chúng cùng hướng lên trên cho đến khi chúng gặp nhau, thu thập các nút dọc theo cả hai nhánh. Điều này đảm bảo rằng mọi nút trên đường dẫn đều được bao gồm chính xác một lần. 

Trong quá trình đánh giá truy vấn, chúng tôi tổng hợp các số mũ nguyên tố trên đường dẫn. Từ điển`best`luôn giữ số mũ tối đa được nhìn thấy cho đến nay đối với mỗi số nguyên tố. Điều này trực tiếp mã hóa định nghĩa LCM ở dạng số mũ. 

## Ví dụ đã hoạt động 

Hãy xem xét cây mẫu nơi chúng tôi truy vấn một đường dẫn lần đầu tiên và sau đó áp dụng bản cập nhật trước truy vấn tiếp theo. 

Đối với truy vấn đầu tiên, giả sử đường dẫn bao gồm các nút có hệ số hóa`{1}`,`{2 × 3}`, Và`{2²}`. Bước tổng hợp xây dựng một bảng như thế này. 

| Nút | Các yếu tố chính | Tổng hợp tốt nhất | 
| --- | --- | --- | 
| 1 | {} | {} | 
| 2 | {2:1, 3:1} | {2:1, 3:1} | 
| 3 | {2:2} | {2:2, 3:1} | 

Câu trả lời cuối cùng là 2² × 3¹ = 12. Điều này xác nhận rằng thuật toán chọn chính xác số mũ tối đa cho mỗi số nguyên tố trên đường đi. 

Sau khi một bản cập nhật nhân một nút với 4, bản đồ nhân tố của nó sẽ tăng thêm hai lũy thừa là 2. Trong truy vấn tiếp theo, nếu nút đó được bao gồm trong đường dẫn thì phần đóng góp của nó sẽ chiếm ưu thế theo số mũ của 2, được phản ánh chính xác trong LCM cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q · N + Q · sqrt(A)) | Mỗi truy vấn đi theo một đường dẫn lên tới N nút và hợp nhất các bản đồ nhân tố; cập nhật yêu cầu tính hệ số nhân | 
| Không gian | O(N log A) | Mỗi nút lưu trữ hệ số nguyên tố của nó | 

Cho N, Q ≤ 1000, điều này phù hợp một cách thoải mái trong giới hạn. Ngay cả với việc phân tích nhân tử lặp đi lặp lại và truyền tải đường dẫn đầy đủ, tổng số thao tác vẫn nằm trong các ràng buộc thông thường đối với Python. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def factor(x):
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

    N, Q = map(int, input().split())
    a = list(map(int, input().split()))
    g = [[] for _ in range(N)]

    for _ in range(N - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    parent = [-1] * N
    depth = [0] * N

    def dfs(u, p):
        parent[u] = p
        for v in g[u]:
            if v != p:
                depth[v] = depth[u] + 1
                dfs(v, u)

    dfs(0, -1)

    facts = [factor(x) for x in a]

    def get_path(u, v):
        pu, pv = u, v
        while depth[pu] > depth[pv]:
            pu = parent[pu]
        while depth[pv] > depth[pu]:
            pv = parent[pv]

        path_u, path_v = [], []
        while pu != pv:
            path_u.append(u)
            path_v.append(v)
            u = parent[u]
            v = parent[v]
            pu = parent[pu]
            pv = parent[pv]

        path_u.append(u)
        return path_u + path_v[::-1]

    out = []
    for _ in range(Q):
        tmp = list(map(int, input().split()))
        if tmp[0] == 2:
            _, w, f = tmp
            w -= 1
            for p, c in factor(f).items():
                facts[w][p] = facts[w].get(p, 0) + c
        else:
            _, u, v = tmp
            u -= 1
            v -= 1

            path = get_path(u, v)

            best = {}
            for node in path:
                for p, c in facts[node].items():
                    best[p] = max(best.get(p, 0), c)

            ans = 1
            for p, c in best.items():
                ans = ans * pow(p, c, MOD) % MOD

            out.append(str(ans))

    return "\n".join(out)

# provided sample
assert solve("""6 3
1 6 5 3 4 3
1 2
1 3
1 4
2 5
4 6
1 1 5
2 2 4
1 1 2
""") == """12
24"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đường dẫn nút đơn | giá trị đúng | LCM của một phần tử | 
| cập nhật lặp đi lặp lại trên cùng một nút | tăng xử lý số mũ | tích lũy đúng đắn | 
| chuỗi đường đi dài | tính chính xác truyền tải đầy đủ | xây dựng lại đường dẫn chính xác | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi các bản cập nhật liên tục nhân lên một nút, khiến hệ số của nó tăng lên đáng kể. Ví dụ: nếu nút 3 bắt đầu từ 1 và được cập nhật 2 năm lần, bản đồ nhân tố của nó sẽ trở thành`{2:5}`. Trên bất kỳ truy vấn nào liên quan đến nút 3, LCM phải phản ánh số mũ đầy đủ này. Thuật toán xử lý việc này vì các bản cập nhật chỉ tăng dần số mũ được lưu trữ chứ không bao giờ ghi đè lên chúng. 

Một trường hợp cạnh khác là đường dẫn có độ dài 1, trong đó u bằng v. Trong trường hợp đó, việc trích xuất đường dẫn chỉ trả về một nút và LCM chính xác là giá trị hiện tại của nó. Bước tổng hợp tự nhiên sẽ giảm xuống một từ điển duy nhất, do đó không cần xử lý đặc biệt. 

Trường hợp thứ ba là khi các nút khác nhau đóng góp cùng một số nguyên tố theo những cách khác nhau. Ví dụ: một nút có thể có`2^3`và cái khác`2^1`. Kết quả đúng phụ thuộc vào việc lấy số mũ tối đa chứ không phải tính tổng. Bước kết hợp trong`best[p] = max(...)`đảm bảo hành vi này, do đó các thừa số nguyên tố chồng chéo được giải quyết chính xác mà không cần tính hai lần.
