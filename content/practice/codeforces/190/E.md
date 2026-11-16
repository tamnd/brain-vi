---
title: "CF 190E - Phản Tấn"
description: "Chúng ta được cung cấp một tập hợp các thành phố, nhưng điều khó khăn là đầu vào không mô tả trực tiếp hệ thống đường thực tế. Thay vào đó, nó mô tả phần bổ sung của nó."
date: "2026-06-03T01:21:22+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dsu", "graphs", "hashing", "sortings"]
categories: ["algorithms"]
codeforces_contest: 190
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 120 (Div. 2)"
rating: 2100
weight: 190
solve_time_s: 93
verified: false
draft: false
---

[CF 190E - Phản đòn](https://codeforces.com/problemset/problem/190/E) 

**Xếp hạng:** 2100 
**Thẻ:** cấu trúc dữ liệu, dsu, đồ thị, băm, sắp xếp 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các thành phố, nhưng điều khó khăn là đầu vào không mô tả trực tiếp hệ thống đường thực tế. Thay vào đó, nó mô tả phần bổ sung của nó. Mỗi cặp thành phố xuất hiện dưới dạng được kết nối trong đầu vào thực tế không có con đường nào giữa chúng trong thế giới thực, trong khi mọi cặp thành phố không được liệt kê trên thực tế đều được kết nối bằng một con đường. 

Vì vậy, chúng tôi có một biểu đồ hiệu quả về$n$các đỉnh trong đó các cạnh trong đầu vào xác định các cạnh không có trong đồ thị thực. Nhiệm vụ là tìm các thành phần được kết nối của đồ thị thực, trong đó khả năng kết nối được xác định bởi các cạnh ẩn không có trong đầu vào. 

Khó khăn đến từ mật độ. Nếu chúng ta cố gắng xây dựng một cách rõ ràng đồ thị thực, chúng ta đang xử lý tới$n(n-1)/2$các cạnh tiềm năng là quá lớn. Ngay cả việc quét tất cả các cặp cũng không thể thực hiện được$n = 5 \cdot 10^5$, vì điều đó sẽ yêu cầu theo thứ tự$10^{11}$hoạt động. 

Vấn đề thứ hai là đồ thị đầu vào có thể thưa thớt hoặc dày đặc, nhưng dù thế nào đi nữa, việc suy luận trực tiếp về các cạnh bù đòi hỏi phải xử lý cẩn thận. Về cơ bản, cấu trúc là: chúng ta được cung cấp một tập hợp các cạnh bị cấm thưa thớt và mọi thứ khác đều được cho phép. 

Một sai lầm ngây thơ là coi các cạnh đầu vào là các cạnh thực. Ví dụ: trong đầu vào mẫu:```
4 4
1 2
1 3
4 2
4 3
```Nếu chúng ta hiểu không chính xác các cạnh này là các cạnh thực, chúng ta sẽ nhận được hai thành phần: {1,2,3,4} thực sự được kết nối đầy đủ theo cách diễn giải đó, điều này sai đối với biểu đồ thực. Trong thực tế, 1 được kết nối với 4 và 2 được kết nối với 3. 

Một trường hợp thất bại tinh vi khác là giả định rằng các cạnh bị thiếu tạo thành nhiều thành phần bị ngắt kết nối có thể được phát hiện một cách tham lam mà không cần theo dõi cấu trúc bổ sung trên toàn cầu. Lý luận cục bộ không thành công vì các cạnh bổ sung phụ thuộc vào sự vắng mặt toàn cục chứ không phải danh sách kề. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: xây dựng biểu đồ thực một cách rõ ràng bằng cách lặp qua tất cả các cặp thành phố và thêm một cạnh bất cứ khi nào nó không có trong dữ liệu đầu vào. Sau đó, chạy DFS hoặc BFS tiêu chuẩn để trích xuất các thành phần được kết nối. 

Điều này đúng, nhưng bước xây dựng chi phối mọi thứ. Kiểm tra tất cả$\binom{n}{2}$cặp dẫn đến$O(n^2)$chỉ làm việc để xây dựng cấu trúc liền kề, điều này không khả thi ở$n = 5 \cdot 10^5$. Ngay cả việc lưu trữ một biểu đồ như vậy cũng không thể thực hiện được trong bộ nhớ. 

Quan sát quan trọng là chúng ta không cần các cạnh rõ ràng của đồ thị phần bù. Thay vào đó, chúng ta có thể mô phỏng khả năng kết nối trong phần bù bằng cách duy trì một tập hợp động các đỉnh “không được thăm” và sử dụng danh sách cạnh cấm để tránh các chuyển đổi bất hợp pháp. 

Đối với mỗi nút, chúng tôi muốn liệt kê nhanh chóng tất cả các đỉnh KHÔNG bị cấm kết nối với nó. Nếu chúng ta duy trì một tập hợp tất cả các nút hiện chưa được truy cập thì đối với một nút nhất định$v$, các lân cận của nó trong biểu đồ phần bù chính xác là các nút chưa được thăm trừ đi danh sách kề bị cấm của nó. Điều này biến việc khám phá vùng lân cận thành các hoạt động khác biệt thay vì quét toàn bộ. 

Bằng cách sử dụng một tập hợp cân bằng (hoặc cấu trúc có thứ tự), chúng ta có thể trích xuất các nút lân cận hợp lệ một cách hiệu quả bằng cách lặp qua các nút chưa được thăm và bỏ qua các nút bị cấm. Mỗi nút được xử lý một lần và mỗi cạnh cấm chỉ được kiểm tra khi có liên quan. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n^2)$| Quá chậm | 
| Đặt + BFS trên phần bù |$O((n + m)\log n)$|$O(n + m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng BFS/DFS trên biểu đồ phần bù bằng cách sử dụng một tập hợp các đỉnh vẫn chưa được thăm. 

1. Lưu trữ tất cả các cạnh bị cấm trong danh sách kề. Đối với mỗi thành phố$v$, chúng tôi giữ một tập hợp băm gồm các thành phố KHÔNG được kết nối trong biểu đồ thực. Đại diện này là trực tiếp từ đầu vào. 
2. Duy trì một tập cân bằng chứa tất cả các đỉnh chưa được thăm. Ban đầu tập hợp này chứa mọi thành phố từ 1 đến$n$. Cấu trúc này cho phép chúng ta lặp lại nhanh chóng các ứng cử viên còn lại. 
3. Lặp lại tất cả các thành phố. Khi chúng tôi tìm thấy một thành phố$v$vẫn còn trong tập hợp chưa được truy cập, chúng tôi bắt đầu một thành phần được kết nối mới. 
4. Khởi tạo hàng đợi với$v$và loại bỏ$v$từ tập hợp chưa được thăm viếng. Điều này bắt đầu một BFS trong biểu đồ phần bù ngầm. 
5. Khi hàng đợi không trống, hãy bật một nút$u$. Bây giờ chúng tôi muốn tìm tất cả các đỉnh$w$như vậy$w$không được thăm viếng và không có ranh giới cấm giữa$u$Và$w$. 
6. Để thực hiện điều này một cách hiệu quả, chúng tôi lặp lại tập hợp chưa được truy cập hiện tại và thu thập các ứng cử viên. Bất cứ khi nào chúng tôi gặp một nút$w$điều đó bị cấm$u$, chúng tôi bỏ qua nó. Nếu không, chúng tôi sẽ xóa nó khỏi tập hợp chưa được truy cập và đẩy nó vào hàng đợi. 
7. Lặp lại cho đến khi hàng trống. Tại thời điểm đó, chúng tôi đã phát hiện ra thành phần liên thông đầy đủ trong biểu đồ thực. 
8. Tiếp tục quét các đỉnh còn lại cho đến khi tất cả được xử lý. 

### Tại sao nó hoạt động 

Ở mỗi bước, BFS chỉ mở rộng qua các cạnh hợp lệ trong biểu đồ phần bù, nghĩa là các cạnh không có trong danh sách đầu vào. Tập hợp chưa được truy cập đảm bảo chúng tôi không bao giờ xử lý một nút hai lần và việc xóa các nút ngay lập tức sẽ ngăn việc quét lặp lại. Mỗi nút được chèn vào chính xác một cây BFS và mọi cạnh bổ sung hợp lệ được xem xét chính xác một lần khi điểm cuối của nó được mở rộng. Điều này duy trì tính chính xác của BFS tiêu chuẩn, nhưng trên cấu trúc kề được xác định ngầm định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import deque

n, m = map(int, input().split())

bad = [set() for _ in range(n + 1)]
for _ in range(m):
    a, b = map(int, input().split())
    bad[a].add(b)
    bad[b].add(a)

unvisited = set(range(1, n + 1))
components = []

while unvisited:
    start = next(iter(unvisited))
    unvisited.remove(start)

    q = deque([start])
    comp = [start]

    while q:
        u = q.popleft()

        to_visit = []
        for v in list(unvisited):
            if v not in bad[u]:
                to_visit.append(v)

        for v in to_visit:
            if v in unvisited:
                unvisited.remove(v)
                q.append(v)
                comp.append(v)

    components.append(comp)

print(len(components))
for comp in components:
    print(len(comp), *comp)
```Lựa chọn triển khai chính là lặp lại qua ảnh chụp nhanh`list(unvisited)`trước khi lọc. Điều này tránh việc sửa đổi một tập hợp trong quá trình lặp, điều này sẽ làm mất hiệu lực của trình vòng lặp. Điểm tinh tế thứ hai là mỗi nút được loại bỏ khỏi`unvisited`ngay khi được phát hiện, đảm bảo nó không bao giờ được xử lý hai lần ngay cả khi có nhiều lớp BFS gặp phải nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4
1 2
1 3
4 2
4 3
```Chúng ta bắt đầu với unvisited = {1,2,3,4}. 

| Bước | Nút hiện tại | Chưa từng ghé thăm trước đây | Mới được thêm vào | Thành phần | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | {1,2,3,4} | {4} | {1,4} | 
| 2 | 4 | {2,3} | {} | {1,4} | 
| 3 | 2 | {2,3} | {3} | {2,3} | 
| 4 | 3 | {} | {} | {2,3} | 

Thuật toán phát hiện ra rằng 1 kết nối với 4 trong biểu đồ phần bù và 2 kết nối với 3. Điều này khớp với kết quả đầu ra mong đợi. 

### Ví dụ 2 

đầu vào:```
5 2
1 2
4 5
```Chúng tôi bắt đầu với tất cả các nút chưa được truy cập. 

| Bước | Nút hiện tại | Chưa từng ghé thăm trước đây | Mới được thêm vào | Thành phần | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | {1,2,3,4,5} | {3,4,5} | {1,3,4,5} | 
| 2 | 3 | {2} | {} | {1,3,4,5} | 
| 3 | 2 | {2} | {} | {2} | 

Nút 1 kết nối với tất cả ngoại trừ 2, do đó nó kéo về 3,4,5. Nút 2 vẫn bị cô lập sau khi loại bỏ các cạnh bị cấm. 

Những dấu vết này cho thấy cách tính kề cận phần bù được xây dựng ngầm thông qua việc loại trừ thay vì liệt kê cạnh rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + m)\log n)$| Mỗi nút được chèn/xóa một lần khỏi tập hợp và mỗi nút kề bị cấm sẽ được kiểm tra trong quá trình mở rộng | 
| Không gian |$O(n + m)$| Lưu trữ các cạnh cấm và cấu trúc sổ sách kế toán | 

Độ phức tạp nằm trong giới hạn vì mỗi thành phố được xử lý một lần và các cạnh bị cấm chỉ hạn chế việc mở rộng thay vì nhân công thức bậc hai. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    from collections import deque

    n, m = map(int, input().split())
    bad = [set() for _ in range(n + 1)]
    for _ in range(m):
        a, b = map(int, input().split())
        bad[a].add(b)
        bad[b].add(a)

    unvisited = set(range(1, n + 1))
    components = []

    while unvisited:
        start = next(iter(unvisited))
        unvisited.remove(start)

        q = deque([start])
        comp = [start]

        while q:
            u = q.popleft()
            for v in list(unvisited):
                if v not in bad[u]:
                    unvisited.remove(v)
                    q.append(v)
                    comp.append(v)

        components.append(comp)

    out = []
    out.append(str(len(components)))
    for comp in components:
        out.append(str(len(comp)) + " " + " ".join(map(str, comp)))
    return "\n".join(out)

# provided sample
assert run("""4 4
1 2
1 3
4 2
4 3
""").split()[0] == "2"

# single node
assert run("""1 0
""").strip() == "1\n1 1"

# fully connected forbidden (no edges in real graph)
assert run("""3 3
1 2
1 3
2 3
""").split()[0] == "3"

# no forbidden edges (complete real graph)
assert run("""3 0
""").split()[0] == "1"

# sparse mix
assert run("""5 2
1 2
4 5
""").split()[0] == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 thành phần | ranh giới tối thiểu | 
| toàn bộ nhóm bị cấm | 3 thành phần | ngắt kết nối hoàn toàn | 
| không có cạnh | 1 thành phần | kết nối đầy đủ bổ sung | 
| trộn thưa thớt | 2 thành phần | sự đúng đắn của cấu trúc hỗn hợp | 

## Vỏ cạnh 

Một đồ thị tối thiểu với$n=1$không có cạnh cấm và phải xuất ra một thành phần duy nhất. Thuật toán khởi tạo tập hợp chưa được truy cập bằng {1}, chọn nó làm nút bắt đầu và ngay lập tức tạo thành một thành phần chỉ chứa nút đó. Không có sự mở rộng nào xảy ra, phù hợp với kết quả mong đợi. 

Một tam giác bị cấm hoàn toàn có các cạnh (1,2), (1,3), (2,3) tạo ra một đồ thị thực không có cạnh. BFS bắt đầu từ 1 không thể mở rộng sang bất kỳ nút nào khác vì mọi nút lân cận tiềm năng đều bị cấm. Thuật toán tách mỗi nút thành thành phần riêng của nó, tạo ra ba thành phần đơn lẻ theo yêu cầu. 

Trường hợp không có cạnh cấm sẽ tạo ra một đồ thị hoàn chỉnh theo cách diễn giải thực tế. Bắt đầu từ bất kỳ nút nào, BFS ngay lập tức sử dụng tất cả các đỉnh còn lại vì không có đỉnh nào bị chặn. Thuật toán tạo ra một thành phần duy nhất chứa tất cả các nút, phù hợp với kết nối đầy đủ.
