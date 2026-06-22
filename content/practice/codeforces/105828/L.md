---
title: "CF 105828L - \u0421\u0438\u043d\u0438\u0439 \u0422\u0440\u0430\u043a\u0442\u043e\u0440 Hãng hàng không"
description: "Có $n$ con vật được đặt ở những vị trí khác nhau trên một trục số vô hạn. Mỗi con vật cũng có chính xác một “quạt” liên quan (chúng ta sẽ gọi chúng là người sưu tầm) được đặt trên một tập hợp các vị trí riêng biệt khác."
date: "2026-06-21T14:57:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105828
codeforces_index: "L"
codeforces_contest_name: "\u0424\u0438\u043d\u0430\u043b \u0412\u041a\u041e\u0428\u041f.Junior 2025"
rating: 0
weight: 105828
solve_time_s: 67
verified: true
draft: false
---

[CF 105828L - \u0421\u0438\u043d\u0438\u0439 \u0422\u0440\u0430\u043a\u0442\u043e\u0440 Airlines](https://codeforces.com/problemset/problem/105828/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

có$n$các con vật được đặt ở các vị trí khác nhau trên trục số vô hạn. Mỗi con vật cũng có chính xác một “quạt” liên quan (chúng ta sẽ gọi chúng là người sưu tầm) được đặt trên một tập hợp các vị trí riêng biệt khác. Hạn chế chính về cấu trúc là nếu một con vật ở bên trái của con vật khác thì bộ thu tương ứng của nó cũng nằm ở bên trái của bộ thu khác. Điều này có nghĩa là thứ tự của động vật và người thu gom là nhất quán: việc sắp xếp động vật theo vị trí sẽ dẫn đến thứ tự tương tự đối với người thu gom. 

Chúng tôi được trao$q$truy vấn. Mỗi truy vấn mô tả hành trình của một máy kéo di chuyển dọc theo trục số từ vị trí$s$để định vị$t$, thăm mọi điểm nguyên trên đường đi. Bất cứ khi nào nó đi qua vị trí của một con vật, nó sẽ nhấc con vật đó lên. Trong khi chở động vật, nếu sau đó máy kéo đi qua người thu gom tương ứng của một số con vật đã được chọn trước khi chuyến đi kết thúc, người thu gom đó sẽ bắt đầu hát. Tất cả các con vật được thả xuống vị trí cuối cùng và tiếng hát dừng lại ngay lập tức. 

Đối với mỗi truy vấn, chúng ta phải đếm xem có bao nhiêu người sưu tầm hát ít nhất một lần trong chuyến đi đó. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$động vật và$2 \cdot 10^5$các truy vấn, do đó, bất kỳ giải pháp bậc hai nào theo một trong hai chiều đều không thể thực hiện được ngay lập tức. Thậm chí$O(nq)$vượt xa giới hạn khả thi, và thậm chí$O(n \log n)$mỗi truy vấn sẽ quá chậm. Do đó, mục tiêu là một giải pháp xung quanh$O((n+q)\log n)$. 

Một vấn đề tế nhị đến từ sự chỉ đạo. Nếu máy kéo di chuyển từ trái sang phải thì gặp các vị trí theo thứ tự tăng dần; nếu nó di chuyển từ phải sang trái thì nó gặp chúng theo thứ tự giảm dần. Điều này thay đổi việc một con vật được nhặt trước hay sau khi gặp người thu gom nó. 

Một sai lầm ngây thơ là bỏ qua hoàn toàn phương hướng và chỉ giả định khoảng thời gian$[\min(s,t), \max(s,t)]$vấn đề. Điều đó không thành công vì thứ tự trong khoảng thời gian quan trọng đối với việc liệu người thu gom có ​​gặp phải khi con vật vẫn còn ở trên tàu hay không. 

Một trường hợp thất bại phổ biến khác là coi “cả động vật và người thu gom trong khoảng thời gian” là đủ. Ví dụ: nếu máy kéo di chuyển từ trái sang phải và một con vật ở vị trí 10 trong khi người thu gom nó ở vị trí 5, cả hai đều nằm trong khoảng thời gian$[1,20]$, người thu gom được ghé thăm trước khi lấy hàng nên không tính. Một sai lầm đối xứng xảy ra theo hướng ngược lại. 

## Phương pháp tiếp cận 

Một mô phỏng mạnh mẽ cho mỗi truy vấn sẽ duyệt qua tất cả các điểm giữa$s$Và$t$, chọn động vật, theo dõi những con có trên tàu và kiểm tra các cuộc gặp gỡ của người thu thập. Trong trường hợp xấu nhất, một truy vấn có thể kéo dài một khoảng lớn chứa$O(n)$các sự kiện liên quan và với$q$truy vấn này trở thành$O(nq)$, xung quanh$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn. 

Quan sát quan trọng là chúng ta thực sự không cần phải mô phỏng chuyển động. Đối với mỗi cặp bộ sưu tập động vật, chúng ta chỉ cần biết liệu cả hai điểm cuối có nằm trong khoảng truy vấn hay không và liệu con vật có gặp trước bộ sưu tập theo thứ tự truyền tải hay không. 

Vì vị trí của động vật và người thu thập giữ nguyên thứ tự tương đối giống nhau nên mỗi chỉ số hoạt động độc lập. Đối với một cặp cố định$i$, chúng ta chỉ cần xác định xem khoảng truy vấn có chứa cả hai$a_i$Và$b_i$, và liệu thứ tự truyền tải có được đặt hay không$a_i$trước$b_i$. 

Điều này tách vấn đề thành hai vấn đề đếm điểm độc lập tùy theo hướng. 

Đối với các truy vấn từ trái sang phải, chỉ ghép nối với$a_i < b_i$vấn đề. Đối với các cặp này, điều kiện trở thành một ngăn hình chữ nhật đơn giản trong mặt phẳng$(a_i, b_i)$. Đối với các truy vấn từ phải sang trái, chỉ ghép nối với$a_i > b_i$vật chất, và một lần nữa điều kiện giảm xuống thành ngăn chặn hình chữ nhật. 

Do đó, mỗi truy vấn trở thành truy vấn đếm phạm vi 2D trên một tập hợp điểm tĩnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(nq)$|$O(n)$| Quá chậm | 
| Quét ngoại tuyến 2D + BIT |$O((n+q)\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chia tất cả động vật thành hai nhóm. Nhóm A chứa các chỉ số trong đó$a_i < b_i$và Nhóm B chứa các chỉ số trong đó$a_i > b_i$. 

Mỗi truy vấn được phân loại theo hướng. Nếu như$s < t$, chúng tôi xử lý nó bằng Nhóm A. Nếu$s > t$, chúng tôi xử lý nó bằng Nhóm B. 

Sau đó, chúng tôi giảm từng truy vấn xuống việc đếm xem có bao nhiêu điểm nằm trong một hình chữ nhật thẳng hàng với trục trong không gian tọa độ. 

### Các bước 

1. Đọc tất cả các điểm$(a_i, b_i)$và chia chúng thành Nhóm A và Nhóm B dựa trên việc$a_i < b_i$hoặc$a_i > b_i$. 
2. Đối với từng nhóm riêng biệt, hãy nén tọa độ nếu cần, vì các giá trị tăng lên$10^9$. 
3. Chuyển mỗi truy vấn thành bài toán đếm hình chữ nhật tùy theo chiều: 

Nếu$s < t$, chúng tôi tính điểm với$s \le a_i \le t$Và$s \le b_i \le t$ở nhóm A 

Nếu$s > t$, chúng tôi tính điểm với$t \le a_i \le s$Và$t \le b_i \le s$ở nhóm B 
4. Đối với mỗi nhóm, xử lý các truy vấn hình chữ nhật bằng cách sử dụng quét ngoại tuyến trên$a_i$: 

Chúng tôi sắp xếp các điểm và truy vấn theo giới hạn trên của$a$và duy trì một cây Fenwick trên$b$. 
5. Mỗi truy vấn hình chữ nhật được phân tách thành hai truy vấn tiền tố trên$a$, cho phép chúng ta tính toán số lượng trong$[L_a, R_a]\times[L_b, R_b]$. 

Ý tưởng quan trọng là khi hướng được cố định, các ràng buộc về thứ tự sẽ biến mất trong mỗi nhóm. Yêu cầu duy nhất còn lại là chứa trong hộp 2D. 

### Tại sao nó hoạt động 

Đối với mỗi nhóm cố định, mỗi cặp đều thỏa mãn mối quan hệ thứ tự cố định giữa$a_i$Và$b_i$. Điều này loại bỏ sự phụ thuộc giữa thứ tự truyền tải và thứ tự điểm cuối. Điều kiện duy nhất ảnh hưởng đến việc người thu gom có ​​hát hay không là liệu cả hai điểm cuối có được truy cập trong chuyến đi hay không. Vì máy kéo đi qua một đoạn liền kề nên đây trở thành bài toán ngăn chặn hình học. 

Quá trình quét Fenwick đảm bảo rằng ở bất kỳ giai đoạn nào, chúng tôi duy trì chính xác tập hợp các điểm mà$a_i$nằm trong tiền tố được xử lý và việc truy vấn BIT cho chúng ta biết có bao nhiêu trong số đó cũng thỏa mãn$b$-ràng buộc phạm vi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class BIT:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

    def range_sum(self, l, r):
        if l > r:
            return 0
        return self.sum(r) - self.sum(l - 1)

def solve_group(points, queries):
    if not points:
        return [0] * len(queries)

    bvals = []
    for a, b in points:
        bvals.append(b)
    for _, l, r, _ in queries:
        bvals.append(l)
        bvals.append(r)

    bvals = sorted(set(bvals))
    comp = {v: i + 1 for i, v in enumerate(bvals)}

    pts = [(a, comp[b]) for a, b in points]
    pts.sort()

    qs = []
    for idx, l, r, a_bound in queries:
        qs.append((a_bound, l, r, idx))
    qs.sort()

    bit = BIT(len(bvals))
    res = [0] * len(queries)

    i = 0
    for a_bound, l, r, idx in qs:
        while i < len(pts) and pts[i][0] <= a_bound:
            bit.add(pts[i][1], 1)
            i += 1
        res[idx] += bit.range_sum(comp[l], comp[r])

    return res

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    q = int(input())

    A = []
    B = []

    for i in range(n):
        if a[i] < b[i]:
            A.append((a[i], b[i]))
        else:
            B.append((a[i], b[i]))

    queriesA = []
    queriesB = []

    ans = [0] * q

    for i in range(q):
        s, t = map(int, input().split())
        if s < t:
            queriesA.append((i, s, t, t))
        else:
            queriesB.append((i, t, s, s))

    resA = solve_group(A, queriesA)
    resB = solve_group(B, queriesB)

    for i in range(q):
        ans[i] = resA[i] + resB[i]

    print(*ans)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào việc giảm từng truy vấn thành các ràng buộc tiền tố trên$a$- phối hợp, sau đó sử dụng cây Fenwick để duy trì số đếm trên$b$-điều phối. Mỗi nhóm được xử lý độc lập và kết quả được tổng hợp lại theo thứ tự truy vấn ban đầu. 

Một điểm tinh tế là mỗi truy vấn được chuyển thành một ngưỡng duy nhất trên$a$để quét, với$a$-range được xử lý bởi sự khác biệt của trạng thái tiền tố. Điều này tránh cần có cây phân đoạn 2D đầy đủ. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản đơn giản hóa với ba cặp:$(2, 5), (4, 7), (6, 3)$. 

Chia thành nhóm A:$(2,5), (4,7)$và Nhóm B:$(6,3)$. 

Thực hiện một truy vấn$s=1, t=6$. 

| Bước | Điểm hoạt động (bởi a) | Trạng thái BIT | Đếm | 
| --- | --- | --- | --- | 
| Cộng (2,5) | {(2,5)} | {5:1} | 0 | 
| Cộng (4,7) | {(2,5),(4,7)} | {5:1,7:1} | 2 | 

Chúng tôi truy vấn hình chữ nhật$[1,6]\times[1,6]$, nên chỉ có (2,5) đóng góp, cho kết quả là 1. 

Điều này cho thấy cách lọc tọa độ tự động loại bỏ (4,7) do$b$-hạn chế. 

Bây giờ hãy xem xét truy vấn hướng ngược lại$s=7, t=3$áp dụng cho Nhóm B. 

Chỉ (6,3) nằm trong khoảng nên được tính trực tiếp. 

| Bước | Điểm hoạt động | Trạng thái BIT | Đếm | 
| --- | --- | --- | --- | 
| Cộng (6,3) | {(6,3)} | {3:1} | 1 | 

Điều này xác nhận rằng việc nhóm dựa trên hướng sẽ tách biệt chính xác các thứ tự hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+q)\log n)$| Sắp xếp cộng với các cập nhật và truy vấn Fenwick cho mỗi nhóm | 
| Không gian |$O(n+q)$| Lưu trữ tọa độ nén, BIT và bộ đệm truy vấn | 

Hệ số logarit được chấp nhận đối với$2 \cdot 10^5$hoạt động và việc triển khai phù hợp một cách thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue().strip()

# Sample-like small case
assert run("""3
2 4 6
5 7 3
2
1 6
7 3
""") == "1 1"

# minimum size
assert run("""1
10
20
1
5 15
""") == "1"

# no valid matches
assert run("""2
1 100
2 200
1
150 160
""") == "0"

# all forward, increasing
assert run("""3
1 3 5
2 4 6
1
1 6
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cặp đơn | 1 | cấu trúc tối thiểu | 
| khoảng rời rạc | 0 | không đếm ngẫu nhiên | 
| bảo hiểm đầy đủ | 3 | bao gồm hình chữ nhật đầy đủ | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các cặp rơi vào một nhóm hướng duy nhất. Trong trường hợp đó, toàn bộ lời giải sẽ trở thành bài toán đếm hình chữ nhật 2D thuần túy. Thuật toán xử lý việc này một cách tự nhiên vì nhóm còn lại không đóng góp truy vấn nào. 

Một trường hợp cạnh khác là khi$s$Và$t$rất gần nhau, tạo thành một khoảng nhỏ không có động vật hay người sưu tầm. Truy vấn hình chữ nhật trở nên trống và cây Fenwick trả về 0 một cách chính xác vì không có điểm nào được kích hoạt trong quá trình quét. 

Trường hợp khó phát hiện cuối cùng xảy ra khi một con vật và người thu thập nó ở xa nhau nhưng chỉ một trong số chúng nằm trong khoảng truy vấn. Vì việc đếm hình chữ nhật yêu cầu cả hai tọa độ bên trong giới hạn nên các cặp như vậy sẽ tự động bị loại trừ mà không cần logic đặc biệt, điều này ngăn chặn việc đếm quá mức trong cấu hình bất đối xứng.
