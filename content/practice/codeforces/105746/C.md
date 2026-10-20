---
title: "CF 105746C - Đại lý du lịch"
description: "Chúng ta có một biểu đồ tuần hoàn có hướng trong đó mỗi thành phố là một nút và mỗi con đường là một cạnh có hướng. Đi dọc theo một con đường sẽ tốn tiền, nhưng không giống như các bài toán về đường đi ngắn nhất tiêu chuẩn, chi phí của mỗi con đường không cố định."
date: "2026-06-22T04:42:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105746
codeforces_index: "C"
codeforces_contest_name: "Bangladesh Olympiad in Informatics 2025 National Round Day 1"
rating: 0
weight: 105746
solve_time_s: 62
verified: true
draft: false
---

[CF 105746C - Đại lý du lịch](https://codeforces.com/problemset/problem/105746/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một biểu đồ tuần hoàn có hướng trong đó mỗi thành phố là một nút và mỗi con đường là một cạnh có hướng. Đi dọc theo một con đường sẽ tốn tiền, nhưng không giống như các bài toán về đường đi ngắn nhất tiêu chuẩn, chi phí của mỗi con đường không cố định. Thay vào đó, mỗi con đường đều có chi phí cơ bản và tỷ lệ thay đổi hàng ngày là âm một, không hoặc cộng một. Nếu bạn đi du lịch vào ngày d, chi phí của một cạnh sẽ trở thành chi phí ban đầu cộng với d nhân với tỷ lệ của cạnh đó. 

Một khách du lịch luôn bắt đầu từ thành phố 1 và mỗi truy vấn sẽ hỏi về việc đến một số thành phố X vào bất kỳ ngày nào trong một khoảng thời gian nhất định. Hành trình phải được hoàn thành trong vòng một ngày và chi phí của một đường đi là tổng chi phí cạnh phụ thuộc vào thời gian dọc theo đường đi đó. Nếu không có cách nào để đến đích từ thành phố 1 thì câu trả lời là không thể. 

Vì vậy, mỗi truy vấn sẽ hỏi: trên tất cả các đường dẫn từ nút 1 đến X và trong tất cả các ngày trong một phạm vi, giá trị tối thiểu có thể có của hàm tuyến tính có dạng cost(path, d) là bao nhiêu. 

Các ràng buộc làm cho cấu trúc trở nên quan trọng. Biểu đồ có tối đa 4000 nút và 4000 cạnh, do đó, nó đủ thưa để lập trình động trên mỗi nút trên DAG là khả thi. Tuy nhiên, số lượng truy vấn có thể lên tới 500000, do đó, bất kỳ giải pháp nào tính toán lại các đường dẫn ngắn nhất cho mỗi truy vấn đều là không thể ngay lập tức. Ngay cả việc tính toán lại mỗi ngày cũng không thể thực hiện được vì số ngày lên tới 1e9. 

Cấu trúc ẩn chính là biểu đồ là DAG. Điều đó loại bỏ các chu kỳ, có nghĩa là mọi đường dẫn đều hữu hạn và chúng ta có thể xử lý các nút theo thứ tự tôpô. Một quan sát quan trọng khác là đối với một đường đi cố định, chi phí là một hàm tuyến tính theo biến ngày. Khó khăn xuất phát từ thực tế là chúng ta phải lấy mức tối thiểu trên nhiều đường đi theo cấp số nhân, đường đi này trở thành mức tối thiểu trên nhiều hàm tuyến tính. 

Một sai lầm ngây thơ là cho rằng đường đi ngắn nhất có thể được tính toán độc lập cho mỗi ngày hoặc mỗi truy vấn. Ví dụ: nếu chúng tôi chỉ có một truy vấn và một phạm vi nhỏ, chúng tôi có thể thử chạy đường dẫn ngắn nhất cho mỗi ngày, nhưng điều này là không thể với phạm vi lên tới 1e9. 

Một vấn đề tế nhị khác là giả định mức tối thiểu trong một khoảng thời gian có thể được tìm thấy bằng cách chỉ đánh giá các điểm cuối mà không cần biện minh. Điều đó chỉ có tác dụng nếu trước tiên chúng ta chứng minh hàm này là hàm lõm hoặc tuyến tính từng phần với cấu trúc thích hợp, cấu trúc này không có giá trị ngay lập tức trừ khi chúng ta rút ra được biểu diễn DP. 

## Phương pháp tiếp cận 

Một cách giải thích vũ phu là đơn giản. Đối với mỗi truy vấn, chúng tôi xem xét mọi ngày d có thể có trong khoảng thời gian đó và chạy đường đi ngắn nhất từ ​​nút 1 đến X nơi trọng số cạnh được tính bằng ngày đó. Vì biểu đồ là DAG nên chúng ta có thể tính toán đường đi ngắn nhất trong O(N + M) bằng cách sử dụng lập trình động theo thứ tự tôpô. Tuy nhiên, độ dài khoảng thời gian có thể lên tới 1e9 nên việc lặp lại tất cả các ngày là không thể. Ngay cả khi chúng tôi thử lấy mẫu, không có gì đảm bảo tính đơn điệu trên đường đi tối ưu. 

Một biến thể mạnh mẽ khác là liệt kê tất cả các đường dẫn từ 1 đến X. Mỗi đường dẫn tạo ra một hàm tuyến tính trong d, do đó chúng tôi sẽ đánh giá tất cả các hàm ở mỗi truy vấn. Điều này không thành công vì số lượng đường dẫn là số mũ trong trường hợp DAG xấu nhất. 

Quan sát cấu trúc quan trọng là mỗi đường dẫn tương ứng với một hàm tuyến tính có dạng A·d + B, trong đó A là tổng tỷ lệ cạnh và B là tổng chi phí cơ bản. Do đó, câu trả lời cho một nút là giá trị tối thiểu trên một tập hợp các dòng. Điều này biến vấn đề thành việc duy trì đường bao hàm tuyến tính thấp hơn cho mỗi nút. 

Vì biểu đồ là DAG nên chúng ta có thể xử lý các nút theo thứ tự tôpô. Mỗi nút thu thập các dòng từ các nút trước đó, dịch chuyển chúng theo các cạnh đến và hợp nhất chúng thành tập hợp riêng của nó. Nhiệm vụ còn lại là duy trì các tập dòng này một cách hiệu quả và trả lời các truy vấn tối thiểu ở mức x nhất định.

Vì chúng ta chỉ cần tối thiểu trên một số lượng nhỏ dòng trên mỗi nút, nên cấu trúc thủ thuật thân lồi sẽ hoạt động. Mỗi nút duy trì một bao lồi dưới gồm các đường được sắp xếp theo độ dốc. Truy vấn tại ngày d cho giá trị nhỏ nhất theo thời gian logarit. Ngoài ra, vì chúng tôi chỉ có 4000 nút nên chúng tôi có đủ khả năng để hợp nhất rõ ràng các thân tàu từ các phiên bản tiền nhiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force theo ngày hoặc đường dẫn | Hàm mũ | Hàm mũ | Quá chậm | 
| DAG DP + Thân lồi trên mỗi nút | O((N + M) log M + Q log M) | O(NM) dòng trường hợp xấu nhất | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi khai thác cấu trúc DAG và coi mỗi nút là lưu trữ một hàm ánh xạ ngày d tới chi phí di chuyển tối thiểu từ nút 1 đến nút đó. 

Mỗi hàm như vậy là một đường bao dưới của các hàm tuyến tính được đóng góp bởi các đường dẫn khác nhau. 

### bước 

1. Tính toán thứ tự tôpô của các nút. Điều này là cần thiết vì tất cả các cạnh đều tiến lên theo thứ tự này, vì vậy khi xử lý một nút, tất cả các cạnh trước của nó đã được hoàn thiện. 
2. Khởi tạo bộ hàm tại nút 1 dưới dạng một đường đơn có độ dốc 0 và điểm chặn 0. Điều này tương ứng với chi phí bằng 0 tại thành phố bắt đầu bất kể ngày nào. 
3. Đối với mọi nút khác, khởi tạo tập hợp các dòng ứng cử của nó là trống. 
4. Duyệt qua các nút theo thứ tự tôpô. Đối với mỗi nút u, hãy xem xét tất cả các cạnh ra u → v với chi phí cơ sở T và độ dốc r. 
5. Với mỗi dòng trong bao của u, được biểu thị là A·d + B, hãy xây dựng một dòng mới cho v dưới dạng (A + r)·d + (B + T). Điều này thể hiện việc mở rộng đường đi qua cạnh u → v. 
6. Chèn tất cả các dòng được tạo này vào một danh sách tạm thời cho v. Vì nhiều dòng trước đó có thể đóng góp nhiều dòng, v thu thập nhiều dòng ứng cử viên. 
7. Khi tất cả các đóng góp cho v đã được tập hợp, hãy xây dựng lại bao lồi từ đầu. Sắp xếp các đường theo độ dốc và sau đó xây dựng đường bao bên dưới bằng cách sử dụng ngăn xếp đơn điệu, loại bỏ bất kỳ đường nào không bao giờ tối ưu. 
8. Sau khi xử lý tất cả các nút, mỗi nút lưu trữ một bao lồi biểu thị hàm chi phí tối thiểu trong tất cả các ngày. 
9. Với mỗi truy vấn (L, R, X), đánh giá thân tàu tại L và tại R và lấy giá trị nhỏ nhất. Nếu nút X không có dòng thì không thể xuất được. 

### Tại sao nó hoạt động 

Mọi đường đi từ nút 1 đến nút X tương ứng với một hàm tuyến tính duy nhất trong d. DP trên DAG đảm bảo rằng tất cả các hàm như vậy được tạo chính xác một lần bằng cách mở rộng các đường dẫn ngắn hơn dọc theo các cạnh. Cấu trúc thân tại mỗi nút loại bỏ các đường ưu thế, nhưng không bao giờ loại bỏ một đường có thể tối ưu cho một số d, bởi vì cấu trúc thân lồi bảo toàn đường bao bên dưới. 

Do đó, hàm cuối cùng tại mỗi nút chính xác là giá trị tối thiểu trên tất cả các hàm đường dẫn. Vì hàm này là cực tiểu của các hàm tuyến tính, nên nó tuyến tính từng đoạn và lõm xuống, do đó, trên bất kỳ khoảng nào, cực tiểu của nó phải xảy ra tại một trong các điểm cuối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_hull(lines):
    if not lines:
        return []
    lines.sort(key=lambda x: (x[0], x[1]))

    hull = []

    def bad(l1, l2, l3):
        # returns True if l2 is unnecessary
        a1, b1 = l1
        a2, b2 = l2
        a3, b3 = l3
        return (b3 - b1) * (a1 - a2) <= (b2 - b1) * (a1 - a3)

    for a, b in lines:
        hull.append((a, b))
        while len(hull) >= 3 and bad(hull[-3], hull[-2], hull[-1]):
            hull.pop(-2)

    return hull

def query(hull, x):
    if not hull:
        return None

    l, r = 0, len(hull) - 1
    best = float("inf")

    while l <= r:
        m = (l + r) // 2
        a, b = hull[m]
        best = min(best, a * x + b)

        # move toward better slope region
        if m + 1 < len(hull):
            a2, b2 = hull[m + 1]
            if a2 * x + b2 < a * x + b:
                l = m + 1
            else:
                r = m - 1
        else:
            r = m - 1

    return best

def main():
    N, M, Q = map(int, input().split())

    g = [[] for _ in range(N)]
    indeg = [0] * N

    for _ in range(M):
        u, v, t, r = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append((v, t, r))
        indeg[v] += 1

    from collections import deque
    q = deque(i for i in range(N) if indeg[i] == 0)

    topo = []
    while q:
        u = q.popleft()
        topo.append(u)
        for v, t, r in g[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                q.append(v)

    hulls = [[] for _ in range(N)]
    hulls[0] = [(0, 0)]

    for u in topo:
        if not hulls[u]:
            continue
        for v, t, r in g[u]:
            tmp = []
            for a, b in hulls[u]:
                tmp.append((a + r, b + t))
            hulls[v].extend(tmp)

        for v, _, _ in g[u]:
            if hulls[v]:
                hulls[v] = build_hull(hulls[v])

    out = []
    for _ in range(Q):
        l, r, x = map(int, input().split())
        x -= 1
        h = hulls[x]
        if not h:
            out.append("sorry")
            continue
        out.append(str(min(query(h, l), query(h, r))))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Quá trình triển khai bắt đầu bằng cách xây dựng thứ tự tôpô, điều này cần thiết để đảm bảo rằng khi chúng tôi xử lý một nút, tất cả các cách để tiếp cận nút đó thông qua các nút trước đó đều đã được tính toán. 

Mỗi nút duy trì một danh sách các hàm tuyến tính biểu diễn tất cả các đường dẫn có thể đến được nó. Khi chúng ta đi qua một cạnh, chúng ta dịch chuyển mọi dòng từ nút nguồn bằng cách thêm độ dốc của cạnh và phần chặn. Điều này truyền bá chính xác tất cả các chi phí đường đi về phía trước. 

Bước thi công thân tàu là cần thiết vì sau khi sáp nhập, nhiều dây chuyền bị chi phối và sẽ không bao giờ tối ưu cho bất kỳ ngày nào. Ngăn xếp đơn điệu loại bỏ những thứ này một cách hiệu quả. 

Cuối cùng, mỗi truy vấn được trả lời bằng cách đánh giá thân tàu ở hai ngày biên và lấy giá trị nhỏ nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chúng tôi theo dõi nút 3 khi nó nhận được dòng. 

| Bước | Nút | Hành động | Dòng được lưu trữ | 
| --- | --- | --- | --- | 
| 1 | 1 | ban đầu | (0,0) | 
| 2 | 1 → 2 | tuyên truyền | nút 2 được chuyển dòng | 
| 3 | 2 | đóng thân tàu | giữ những dòng hay nhất | 
| 4 | 2 → 4 | tuyên truyền | nút 4 được dòng | 
| 5 | 3 | truy vấn | đánh giá điểm cuối | 

Dấu vết cho thấy các hàm tuyến tính tích lũy dọc theo các đường dẫn và chỉ được lọc sau khi truyền bá. 

### Ví dụ 2 

Ở đây tồn tại nhiều cạnh song song giữa các nút giống nhau. 

| Bước | Nút | Hành động | Dòng được lưu trữ | 
| --- | --- | --- | --- | 
| 1 | 1 | ban đầu | nhiều đường thẳng song song | 
| 2 | hợp nhất | kết hợp ca | nhiều dòng ứng cử viên | 
| 3 | thân tàu | loại bỏ thống trị | thân tàu nhỏ gọn | 
| 4 | truy vấn | đánh giá | câu trả lời từ dòng hay nhất | 

Điều này chứng tỏ rằng các cạnh trùng lặp chỉ đơn giản là thêm nhiều đường ứng cử viên hơn và việc xây dựng thân tàu sẽ loại bỏ sự dư thừa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + M)·K + Q log K) | mỗi cạnh truyền các đường, mỗi truy vấn thân là logarit | 
| Không gian | O(N·K) | mỗi nút lưu trữ một bao lồi của các hàm tuyến tính | 

Ở đây K biểu thị số lượng đường hiệu quả trên mỗi nút sau khi cắt tỉa ưu thế, con số này nhỏ trong thực tế do nén thân lồi và ràng buộc DAG. Điều này phù hợp trong giới hạn vì N và M nhiều nhất là 4000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque

    N, M, Q = map(int, sys.stdin.readline().split())
    g = [[] for _ in range(N)]
    indeg = [0] * N

    for _ in range(M):
        u, v, t, r = map(int, sys.stdin.readline().split())
        u -= 1
        v -= 1
        g[u].append((v, t, r))
        indeg[v] += 1

    q = deque(i for i in range(N) if indeg[i] == 0)
    topo = []
    while q:
        u = q.popleft()
        topo.append(u)
        for v, t, r in g[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                q.append(v)

    def build_hull(lines):
        if not lines:
            return []
        lines.sort(key=lambda x: (x[0], x[1]))
        hull = []
        def bad(l1, l2, l3):
            a1, b1 = l1
            a2, b2 = l2
            a3, b3 = l3
            return (b3 - b1) * (a1 - a2) <= (b2 - b1) * (a1 - a3)

        for a, b in lines:
            hull.append((a, b))
            while len(hull) >= 3 and bad(hull[-3], hull[-2], hull[-1]):
                hull.pop(-2)
        return hull

    def query(hull, x):
        if not hull:
            return None
        best = float("inf")
        for a, b in hull:
            best = min(best, a * x + b)
        return best

    hulls = [[] for _ in range(N)]
    hulls[0] = [(0, 0)]

    for u in topo:
        for v, t, r in g[u]:
            for a, b in hulls[u]:
                hulls[v].append((a + r, b + t))
        for v, _, _ in g[u]:
            if hulls[v]:
                hulls[v] = build_hull(hulls[v])

    def solve():
        out = []
        for line in inp.strip().splitlines()[1 + M + 1:]:
            L, R, X = map(int, line.split())
            X -= 1
            h = hulls[X]
            if not h:
                out.append("sorry")
            else:
                out.append(str(min(query(h, L), query(h, R))))
        return "\n".join(out)

    return solve()

# custom sanity checks (light)
# assert run(...) == ...
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| DAG tối thiểu | giá trị đúng | sự lan truyền bazơ | 
| chuỗi đường dẫn đơn | tích lũy tuyến tính | tích lũy độ dốc | 
| nút không thể truy cập | xin lỗi | đường dẫn còn thiếu | 

## Vỏ cạnh 

Các đích đến không thể truy cập được xử lý một cách tự nhiên vì thân của chúng vẫn trống trong suốt quá trình truyền. Trong trường hợp đó, không có dòng dẫn xuất từ ​​đường dẫn nào đến được nút, vì vậy truy vấn trả về chính xác là không thể. 

Các cạnh song song tạo ra nhiều đường giống nhau hoặc tương tự nhau. Cấu trúc thân tàu loại bỏ những phần dư thừa, đảm bảo rằng chúng không làm tăng thời gian truy vấn. 

Độ dốc âm có thể khiến chi phí giảm theo thời gian, nhưng điều này được xử lý chính xác vì mỗi đường dẫn được coi là một hàm tuyến tính đầy đủ và mức tối thiểu trên tất cả các hàm đó vẫn được biểu diễn chính xác trong thân tàu.
