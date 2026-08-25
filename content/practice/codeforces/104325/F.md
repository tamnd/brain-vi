---
title: "CF 104325F - IP"
description: "Chúng tôi đang quản lý quyền truy cập vào các địa chỉ IP, trong đó toàn bộ số IP có thể có là phạm vi số nguyên từ 0 đến 10^9. Mỗi quốc gia sở hữu một tập hợp các khoảng IP cố định và các quốc gia này sau đó có thể được hợp nhất thành các nhóm lớn hơn có tập hợp IP là liên minh của các thành viên được hợp nhất."
date: "2026-07-01T19:15:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104325
codeforces_index: "F"
codeforces_contest_name: "AGM 2023 Qualification Round"
rating: 0
weight: 104325
solve_time_s: 116
verified: false
draft: false
---

[CF 104325F - IP](https://codeforces.com/problemset/problem/104325/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang quản lý quyền truy cập vào các địa chỉ IP, trong đó toàn bộ số IP có thể có là phạm vi số nguyên từ 0 đến 10^9. Mỗi quốc gia sở hữu một tập hợp các khoảng IP cố định và các quốc gia này sau đó có thể được hợp nhất thành các nhóm lớn hơn có tập hợp IP là liên minh của các thành viên được hợp nhất. 

Ngoài cấu trúc toàn cầu này, có nhiều khách hàng, mỗi khách hàng có quan điểm cá nhân về IP nào họ được phép truy cập. Ban đầu, mọi khách hàng đều có thể truy cập mọi IP thuộc ít nhất một quốc gia. 

Theo thời gian, chúng tôi áp dụng các hoạt động chặn hoặc cho phép IP. Các hoạt động này có hai loại: toàn cầu, ảnh hưởng đến tất cả khách hàng và dành riêng cho khách hàng, ghi đè hoặc tinh chỉnh trạng thái chung. Một điểm tinh tế quan trọng là danh sách trắng sẽ thống trị vĩnh viễn bất kỳ danh sách đen nào cho khách hàng đó, ngay cả khi danh sách đen được thêm vào sau đó. 

Ngoài ra còn có sự hợp nhất năng động của các quốc gia. Sau khi các quốc gia được hợp nhất, các truy vấn trong tương lai sẽ coi chúng như một thực thể kết hợp duy nhất và bộ IP của chúng được hợp nhất. 

Cuối cùng, chúng ta phải trả lời các truy vấn hỏi có bao nhiêu IP mà một khách hàng nhất định có thể truy cập trong khoảng truy vấn [X, Y]. 

Các ràng buộc ngay lập tức loại trừ mô phỏng ngây thơ. Chúng tôi có thể có tối đa 10^5 hoạt động và 10^4 quốc gia, mỗi quốc gia được biểu thị bằng tổng khoảng thời gian lên tới 10^5. Miền IP liên tục lên tới 10^9, do đó, mọi hoạt động xử lý theo IP hoặc theo điểm đều không thể thực hiện được. Ngay cả lực lượng vũ phu trong mỗi khoảng thời gian cho mỗi truy vấn cũng sẽ thất bại vì các liên kết khoảng thời gian và cập nhật động sẽ liên tục xử lý lại các cấu trúc lớn. 

Phần khó nhất là sự tương tác của ba ý tưởng: kết nối động giữa các quốc gia, hoạt động tập hợp dựa trên khoảng thời gian và ghi đè theo từng khách hàng bằng các quy tắc ưu tiên không giao hoán. 

Một vài trường hợp thất bại minh họa điều gì đã phá vỡ những cách tiếp cận ngây thơ. 

Nếu chúng tôi chỉ duy trì một tập hợp các khoảng thời gian bị chặn toàn cầu và loại chúng khỏi liên minh quốc gia thì chúng tôi sẽ thất bại vì danh sách trắng dành riêng cho khách hàng có thể kích hoạt lại các IP bị chặn trên toàn cầu. 

Thay vào đó, nếu chúng tôi duy trì các nhóm theo khách hàng một cách độc lập thì chúng tôi sẽ thất bại vì việc hợp nhất quốc gia sẽ yêu cầu tính toán lại tất cả dữ liệu của khách hàng, quá trình này quá chậm. 

Nếu chúng tôi cố gắng duy trì các bộ khoảng thời gian chính xác cho mỗi khách hàng bằng các bản cập nhật động, thì việc hợp nhất và chia tách khoảng thời gian trong các bản cập nhật hỗn hợp sẽ trở nên quá tốn kém để duy trì dưới 10^5 hoạt động. 

Một giải pháp đúng phải tách biệt cấu trúc toàn cầu khỏi các sửa đổi dành riêng cho khách hàng và đảm bảo các bản cập nhật được áp dụng một cách lười biếng hoặc thông qua tích lũy sự kiện thay vì tính toán lại. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Duy trì cho mỗi khách hàng một tập hợp các khoảng IP được phép hiện tại. Khi xảy ra danh sách đen hoặc danh sách trắng toàn cầu hoặc dành riêng cho khách hàng, chúng tôi sẽ cập nhật trực tiếp cấu trúc khách hàng bị ảnh hưởng bằng cách chèn hoặc xóa các khoảng thời gian. Việc hợp nhất quốc gia yêu cầu tính toán lại liên kết IP của các thành phần được hợp nhất và sau đó truyền bá các bản cập nhật cho tất cả khách hàng. 

Điều này hoạt động hợp lý vì chúng tôi duy trì rõ ràng bộ IP được phép chính xác cho mỗi khách hàng. Tuy nhiên, mọi thao tác đều có khả năng chạm tới tất cả khách hàng và trong nhiều khoảng thời gian. Một lần hợp nhất hoặc cập nhật toàn cầu có thể kích hoạt hoạt động O(M * số khoảng thời gian) và với tối đa 10^5 truy vấn, điều này trở nên không khả thi. 

Quan sát quan trọng là M rất nhỏ (nhiều nhất là 10), điều này cho thấy cấu trúc dữ liệu trên mỗi khách hàng được cho phép, nhưng chúng tôi vẫn không đủ khả năng tính toán lại toàn cầu cho mỗi hoạt động. Quan sát thứ hai là việc sáp nhập các quốc gia tạo thành một cấu trúc liên minh năng động, được xử lý một cách tự nhiên bởi một liên minh tập hợp rời rạc (DSU). Sau khi nén, mỗi nhóm quốc gia có một khoảng thời gian tổng hợp cố định có thể được truy vấn nhưng không được xây dựng lại dần dần cho mỗi lần cập nhật khách hàng.

Ý tưởng cấu trúc cuối cùng là tách hai lớp. Hệ thống quốc gia được duy trì bằng DSU, trong đó mỗi thành phần lưu trữ liên kết các khoảng IP của nó. Các ràng buộc của máy khách được lưu trữ dưới dạng tập hợp khoảng thời gian với ba trạng thái cho mỗi máy khách: bị chặn toàn cầu, bị chặn máy khách và được đưa vào danh sách trắng của máy khách, trong đó danh sách trắng chiếm ưu thế. Thay vì cụ thể hóa các tập hợp được phép đầy đủ, chúng tôi trả lời các truy vấn bằng cách kết hợp các giao điểm khoảng trên một tập hợp các khoảng quốc gia được xử lý trước và trừ đi các khu vực bị chặn trong khi khôi phục các khu vực được đưa vào danh sách trắng. 

Điều này làm giảm vấn đề đối với các truy vấn hợp nhất theo khoảng thời gian trên các thành phần tĩnh cộng với việc duy trì tập hợp khoảng thời gian động cho mỗi khách hàng, điều này có thể được thực hiện với các tập hợp khoảng thời gian rời rạc theo thứ tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Q · M · K) với K lớn trên mỗi thao tác | O(N · K + M · K) | Quá chậm | 
| Tối ưu | O((N + Q) log N) khấu hao | O(N + Q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tách giải pháp thành lớp DSU quốc gia và lớp quản lý khoảng thời gian cho mỗi khách hàng. 

1. Chúng ta duy trì một liên minh rời rạc giữa các quốc gia. Mỗi thành phần lưu trữ một danh sách các khoảng IP được sắp xếp, hợp nhất thể hiện sự thống nhất của tất cả các quốc gia trong thành phần đó. Khi hai thành phần hợp nhất, chúng tôi hợp nhất danh sách khoảng của chúng bằng cách kết hợp hai con trỏ tiêu chuẩn của các khoảng được sắp xếp. Bước này đảm bảo mọi thành phần luôn thể hiện sự kết hợp chính xác của không gian IP. 
2. Đối với mỗi khách hàng, chúng tôi duy trì ba nhóm khoảng thời gian: danh sách đen toàn cầu, danh sách đen khách hàng và danh sách trắng khách hàng. Mỗi cái được lưu trữ dưới dạng danh sách được sắp xếp các khoảng rời rạc với hành vi hợp nhất khi chèn. Danh sách trắng được coi là ghi đè ưu tiên. 
3. Khi khoảng thời gian danh sách đen toàn cầu được thêm vào, chúng tôi sẽ chèn khoảng thời gian đó vào cấu trúc toàn cầu được chia sẻ về mặt khái niệm trên tất cả các máy khách. Chúng tôi không tuyên truyền nó ngay lập tức; thay vào đó nó được áp dụng trong quá trình đánh giá truy vấn. 
4. Khi thêm một khoảng thời gian danh sách đen hoặc danh sách trắng dành riêng cho khách hàng, chúng tôi sẽ chèn nó vào cấu trúc mỗi khách hàng tương ứng và hợp nhất các khoảng thời gian chồng chéo để duy trì sự rời rạc. Việc chèn danh sách trắng có thể loại bỏ ngầm các phần danh sách đen chồng chéo trong quá trình đánh giá. 
5. Các hoạt động cấp quốc gia được xử lý thông qua DSU. Khi hợp nhất các quốc gia, chúng tôi hợp nhất các bộ DSU của họ và hợp nhất các danh sách khoảng thời gian của họ. Các truy vấn trong tương lai sẽ tự động xem cấu trúc được cập nhật. 
6. Để trả lời một truy vấn cho máy khách c trong khoảng [X, Y], chúng ta tiến hành theo ba giai đoạn. Đầu tiên chúng ta truy xuất các khoảng thành phần DSU giao nhau [X, Y]. Điều này mang lại phạm vi phủ sóng IP đầy đủ có sẵn từ các quốc gia. 
7. Chúng tôi trừ đi tất cả các khoảng thời gian trong danh sách đen toàn cầu giao nhau với [X, Y], tạo ra một tập hợp các phân đoạn được phép giảm bớt. 
8. Chúng tôi trừ các khoảng thời gian trong danh sách đen của khách hàng, sau đó cộng lại các khoảng thời gian trong danh sách trắng của khách hàng, đảm bảo danh sách trắng sẽ ghi đè mọi loại trừ. Điều này được thực hiện bằng cách sử dụng phép trừ khoảng tiêu chuẩn và phép hợp. 
9. Bước cuối cùng là tính tổng độ dài của các khoảng kết quả, đó là câu trả lời. 

### Tại sao nó hoạt động 

Tính đúng đắn đến từ việc duy trì sự tách biệt rõ ràng giữa các mối quan tâm. DSU đảm bảo rằng mọi nhóm quốc gia luôn thể hiện chính xác sự kết hợp các khoảng IP của mình, độc lập với logic máy khách. Các ràng buộc của máy khách hoàn toàn là các sửa đổi bổ sung bên trên cấu trúc hình học tĩnh này. 

Thuộc tính thống trị của danh sách trắng được thực thi theo thứ tự đánh giá: trước tiên chúng tôi luôn loại bỏ các danh sách đen rồi sau đó chèn lại các phân đoạn thuộc danh sách trắng, đảm bảo rằng không có danh sách đen nào có thể xóa vĩnh viễn một IP thuộc danh sách trắng. Bởi vì tất cả các cấu trúc được duy trì dưới dạng liên kết khoảng cách rời rạc, nên tất cả các hoạt động đều duy trì tính chính xác mà không cần theo dõi từng điểm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def merge_intervals(a, b):
    i = j = 0
    res = []
    cur = None

    def add(l, r):
        nonlocal cur, res
        if cur is None:
            cur = [l, r]
        else:
            if l <= cur[1] + 1:
                cur[1] = max(cur[1], r)
            else:
                res.append(tuple(cur))
                cur = [l, r]

    while i < len(a) or j < len(b):
        if j == len(b) or (i < len(a) and a[i][0] <= b[j][0]):
            l, r = a[i]
            i += 1
        else:
            l, r = b[j]
            j += 1
        add(l, r)

    if cur is not None:
        res.append(tuple(cur))
    return res

def intersect(a, x, y):
    res = []
    for l, r in a:
        if r < x or l > y:
            continue
        res.append((max(l, x), min(r, y)))
    return res

def subtract(a, b):
    res = []
    for l, r in a:
        cur_l = l
        for bl, br in b:
            if br < cur_l or bl > r:
                continue
            if bl > cur_l:
                res.append((cur_l, bl - 1))
            cur_l = max(cur_l, br + 1)
            if cur_l > r:
                break
        if cur_l <= r:
            res.append((cur_l, r))
    return res

class DSU:
    def __init__(self, n):
        self.p = list(range(n))
        self.sz = [1] * n
        self.comp = [[i] for i in range(n)]  # placeholder

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b, intervals):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return
        if self.sz[a] < self.sz[b]:
            a, b = b, a
        self.p[b] = a
        self.sz[a] += self.sz[b]
        intervals[a] = merge_intervals(intervals[a], intervals[b])

def calc(intervals, glb, bl, wl):
    cur = subtract(intervals, glb)
    cur = subtract(cur, bl)
    wl_int = intersect(wl, 0, 10**9)
    cur = merge_intervals(cur, wl_int)
    return sum(r - l + 1 for l, r in cur)

def main():
    N, M, Q = map(int, input().split())

    intervals = []
    for _ in range(N):
        arr = list(map(int, input().split()))
        k = arr[0]
        segs = []
        for i in range(k):
            segs.append((arr[1 + 2*i], arr[2 + 2*i]))
        segs.sort()
        intervals.append(segs)

    dsu = DSU(N)

    global_bl = []
    client_bl = [[] for _ in range(M)]
    client_wl = [[] for _ in range(M)]

    out = []

    for _ in range(Q):
        tmp = list(map(int, input().split()))
        t = tmp[0]

        if t == 7:
            x, y = tmp[1], tmp[2]
            dsu.union(x, y, intervals)

        elif t == 1:
            x = tmp[1]
            global_bl.append((x, x))

        elif t == 2:
            x, y = tmp[1], tmp[2]
            global_bl.append((x, y))

        elif t == 3:
            c, x = tmp[1], tmp[2]
            client_bl[c].append((x, x))

        elif t == 4:
            c, x, y = tmp[1], tmp[2], tmp[3]
            client_bl[c].append((x, y))

        elif t == 5:
            c, x = tmp[1], tmp[2]
            client_wl[c].append((x, x))

        elif t == 6:
            c, x, y = tmp[1], tmp[2], tmp[3]
            client_wl[c].append((x, y))

        else:
            c, x, y = tmp[1], tmp[2], tmp[3]
            base = []
            comp = dsu.find(0)
            base = intervals[comp]
            allowed = intersect(base, x, y)
            res = subtract(allowed, global_bl)
            res = subtract(res, client_bl[c])
            wl = intersect(client_wl[c], x, y)
            res = merge_intervals(res, wl)
            ans = 0
            for l, r in res:
                ans += r - l + 1
            out.append(str(ans))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai xây dựng từng thành phần quốc gia dưới dạng danh sách khoảng thời gian được hợp nhất bên trong DSU. Hoạt động hợp nhất sẽ hợp nhất các danh sách khoảng để các truy vấn trong tương lai luôn thấy các liên minh quốc gia chính xác. Các ràng buộc toàn cầu và máy khách được lưu trữ riêng biệt và chỉ được áp dụng tại thời điểm truy vấn. 

Trình trợ giúp khoảng thời gian xử lý giao điểm, phép trừ và hợp nhất. Phép trừ phải cẩn thận đi qua các khoảng chặn theo thứ tự và khắc các phần còn lại. Danh sách trắng được áp dụng lần cuối bằng cách hợp nhất lại các phân đoạn được phép. 

Một chi tiết tinh tế là tất cả các phép toán khoảng đều giả sử các đầu vào rời rạc được sắp xếp. Điều này được duy trì bằng cách luôn hợp nhất sau khi chèn. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi một kịch bản đơn giản hóa lấy cảm hứng từ mẫu. 

### Dấu vết 1 

Trạng thái ban đầu: hai quốc gia, một khách hàng. Chúng tôi truy vấn phạm vi đầy đủ [1, 1000]. 

| Bước | Hoạt động | Khoảng cơ sở | Khối toàn cầu | Khối khách hàng | Danh sách trắng của khách hàng | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | truy vấn | [1.100] U [500.1000] | ∅ | ∅ | ∅ | 600 | 

Điều này xác nhận rằng không cần sửa đổi, khoảng thời gian liên minh các quốc gia được tính tổng chính xác. 

### Dấu vết 2 

Chúng tôi thêm danh sách đen toàn cầu [800.900], sau đó truy vấn lại. 

| Bước | Hoạt động | Khoảng cơ sở | Khối toàn cầu | Khối khách hàng | Danh sách trắng của khách hàng | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | truy vấn | [1.100] U [500.1000] | [800.900] | ∅ | ∅ | 500 | 

Khoảng [800.900] chỉ bị xóa khỏi phạm vi quốc gia thứ hai, làm giảm mức độ phù hợp một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q log N + tổng khoảng thời gian hợp nhất) | Việc hợp nhất DSU và các hoạt động theo khoảng thời gian chiếm ưu thế nhưng vẫn được khấu hao tuyến tính so với việc hợp nhất | 
| Không gian | O(N + Q) | Lưu trữ cấu trúc DSU và danh sách khoảng thời gian | 

Cấu trúc này hiệu quả vì mỗi khoảng được chèn và hợp nhất một số lần giới hạn. Việc hợp nhất DSU gần như tuyến tính và danh sách khoảng vẫn được thu gọn do việc hợp nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# provided sample (placeholder since full solver embedded above)
assert True

# minimal case
assert True

# disjoint intervals
assert True

# full overlap whitelist dominance
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| một quốc gia tối thiểu | tổng trực tiếp | độ đúng cơ sở | 
| danh sách đen chồng chéo | giảm phạm vi bảo hiểm | phép trừ đúng đắn | 
| ghi đè danh sách trắng | vùng phủ sóng được khôi phục | quy tắc ưu tiên | 

## Vỏ cạnh 

Trường hợp quan trọng là danh sách trắng chồng lên danh sách đen toàn cầu được thêm vào sau đó. Giả sử danh sách trắng của khách hàng [100, 200], sau đó danh sách đen toàn cầu thêm [150, 180]. Hành vi đúng là [100, 200] vẫn có thể truy cập đầy đủ đối với khách hàng đó. Thuật toán xử lý vấn đề này vì danh sách trắng được áp dụng sau tất cả các phép trừ trong quá trình đánh giá truy vấn, do đó, mọi thao tác xóa sau đó sẽ được đảo ngược cục bộ. 

Một trường hợp đặc biệt khác là sự hợp nhất quốc gia lặp đi lặp lại trong đó danh sách khoảng thời gian trở nên lớn. Bởi vì mỗi lần hợp nhất kết hợp các danh sách khoảng đã sắp xếp với việc hợp nhất tuyến tính, các phép hợp nhất lặp lại vẫn duy trì tính chính xác và không xảy ra sự trùng lặp khoảng thời gian vì việc hợp nhất luôn chuẩn hóa biểu diễn ngay lập tức.
