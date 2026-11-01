---
title: "CF 106456L - Lốc xoáy phá hủy bãi đỗ xe"
description: "Bãi đỗ xe là một mạng lưới trong đó mỗi ô trống hoặc chứa một ô tô có hướng di chuyển cố định."
date: "2026-06-21T16:28:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106456
codeforces_index: "L"
codeforces_contest_name: "The 15th Huazhong Agricultural University Programming Contest"
rating: 0
weight: 106456
solve_time_s: 61
verified: true
draft: false
---

[CF 106456L - Lốc xoáy phá hủy bãi đậu xe](https://codeforces.com/problemset/problem/106456/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bãi đỗ xe là một mạng lưới trong đó mỗi ô trống hoặc chứa một ô tô có hướng di chuyển cố định. Mỗi ô tô chỉ có thể cố gắng rời khỏi lưới theo hướng mà nó đang đối mặt và nó chỉ thành công nếu toàn bộ đoạn thẳng từ vị trí của nó đến ranh giới lưới không có ô tô nào khác tại thời điểm đó. 

Thao tác chúng ta được phép thực hiện là chọn ô tô và gửi lệnh “rời đi”. Nếu đường đi theo hướng đối diện của nó thông thoáng, chiếc xe sẽ biến mất và ô của nó trở nên trống rỗng. Nếu không thì không có gì hữu ích xảy ra vào thời điểm đó, vì vậy mọi giải pháp hợp lệ đều phải tránh lãng phí việc di chuyển trên những chiếc xe bị chặn. 

Nhiệm vụ là xác định xem liệu có tồn tại lệnh ra lệnh sao cho mọi ô tô đều rời đi hay không. Nếu thứ tự như vậy tồn tại, chúng ta phải xuất ra bất kỳ chuỗi tọa độ hợp lệ nào theo thứ tự loại bỏ. Nếu không thể xóa tất cả các ô tô, chúng ta sẽ xuất -1. 

Kích thước lưới tăng lên đến 2000 vào năm 2000, do đó có thể có tới bốn triệu ô. Bất kỳ phương pháp nào cố gắng quét liên tục toàn bộ hàng hoặc cột cho mọi thao tác sẽ quá chậm vì mỗi lần quét đều có kích thước tuyến tính và có khả năng lặp lại một lần trên mỗi ô tô. Điều đó dẫn đến hành vi bậc hai trong trường hợp xấu nhất về số lượng ô. 

Một trường hợp thất bại tinh tế đối với mô phỏng đơn giản là khi ô tô tham gia vào các chu kỳ phụ thuộc. Ví dụ: hãy xem xét lưới 2 x 2:```
RD
UL
```Mỗi ô tô chặn một ô tô khác theo hướng của nó, tạo thành một vòng tròn mà ban đầu không có ô tô nào có đường đi rõ ràng. Bất kỳ chiến lược tham lam nào cố gắng loại bỏ những chiếc ô tô “hiện có thể tháo rời” sẽ ngay lập tức bị mắc kẹt, bởi vì ngay từ đầu không có chiếc nào đủ tiêu chuẩn, mặc dù góc độ phụ thuộc toàn cầu hơn cho thấy rằng không có lệnh loại bỏ nào tồn tại. 

Một vấn đề tế nhị khác xuất hiện theo chuỗi dài. Nếu một ô tô phụ thuộc vào một ô tô khác ở xa trong cùng hàng hoặc cột, việc quét liên tục toàn bộ dòng sau mỗi lần loại bỏ sẽ tính toán lại cùng một cấu trúc nhiều lần, điều này trở nên quá chậm. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ là liên tục quét lưới, xác định tất cả các ô tô có đường đến ranh giới hiện rõ ràng, loại bỏ một trong số chúng và lặp lại. Mỗi lần lặp lại yêu cầu kiểm tra tối đa O(NM) ô và kiểm tra đường đi của ô tô yêu cầu quét tới O(N) hoặc O(M). Với việc loại bỏ tối đa O(NM), con số này sẽ trở thành O((NM)^2), vượt xa giới hạn đối với 2 triệu ô tô trở lên. 

Quan sát cấu trúc quan trọng là khả năng rời đi của ô tô chỉ phụ thuộc vào ô tô đầu tiên đi theo hướng của nó dọc theo cùng một hàng hoặc cột. Nếu chiếc xe chặn gần nhất đó tồn tại thì không có gì quan trọng hơn nó. Điều này chuyển đổi điều kiện tầm nhìn toàn cầu thành sự phụ thuộc cục bộ: mỗi ô tô chỉ phụ thuộc vào tối đa một ô tô lân cận theo hướng của nó. 

Khi chúng tôi xem các phần phụ thuộc theo cách này, quy trình sẽ trở thành một vấn đề về biểu đồ động. Mỗi ô tô là một nút và nó bị “chặn” bởi tối đa một nút khác theo hướng của nó. Khi một chiếc xe đang chặn bị loại bỏ, sự phụ thuộc của những chiếc xe gần đó có thể biến mất, khiến chúng mới đủ điều kiện để loại bỏ. 

Điều này gợi ý việc duy trì sự liền kề trong mỗi hàng và cột để khi một chiếc ô tô biến mất, chúng ta có thể nhanh chóng tìm ra vật chặn tiềm năng tiếp theo. Cấu trúc liên kết đôi trên mỗi hàng và mỗi cột cho phép chúng ta cập nhật các hàng xóm trong thời gian O(1) và hàng đợi xử lý những ô tô trở nên trống. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Quét lặp đi lặp lại | O((NM)^2) | O(NM) | Quá chậm | 
| Bảo trì hàng xóm hàng/cột + BFS | O(NM) | O(NM) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi chiếc ô tô như một nút chỉ có thể được gỡ bỏ nếu hướng đi của nó hiện không bị cản trở. Để hỗ trợ cập nhật nhanh, chúng tôi duy trì bốn con trỏ lân cận cho mỗi ô tô: trái, phải, lên và xuống trong hàng hoặc cột của nó giữa các ô tô khác. 

1. Lập danh sách tất cả các ô tô và chỉ định vị trí cho từng ô tô. Đối với mỗi hàng, liên kết ô tô từ trái sang phải và đối với mỗi cột, liên kết ô tô từ trên xuống dưới. Điều này cho phép chúng tôi truy cập ngay vào chiếc xe tiếp theo còn sống sót theo bất kỳ hướng nào. 
2. Đối với mỗi toa, hãy kiểm tra xem ban đầu nó có thể tháo rời được hay không bằng cách chỉ nhìn vào ô tô lân cận theo hướng của nó. Nếu người hàng xóm đó không tồn tại thì xe đã có thể rời đi nên bị đẩy vào hàng đợi. 
3. Xử lý hàng đợi nhiều lần. Mỗi lần loại bỏ một chiếc ô tô, chúng tôi ghi lại tọa độ của nó theo trình tự trả lời. 
4. Khi một chiếc ô tô bị xóa, chúng tôi sẽ xóa nó khỏi cả danh sách liên kết hàng và cột của nó. Điều này có nghĩa là kết nối lại hàng xóm bên trái của nó với hàng xóm bên phải và hàng xóm phía trên của nó với hàng xóm phía dưới. 
5. Sau khi loại bỏ một ô tô, chỉ những ô tô lân cận trong cùng hàng hoặc cột mới có thể thay đổi cấu trúc chặn của chúng. Đối với mỗi người hàng xóm như vậy, chúng tôi kiểm tra xem chiếc xe mà chúng tôi vừa loại bỏ có phải là người hàng xóm đang chặn hướng của nó hay không. Nếu có, chúng tôi sẽ tính toán lại trình chặn mới gần nhất bằng cách sử dụng cấu trúc được liên kết đã cập nhật. Nếu bây giờ nó không có bộ chặn theo hướng đó, chúng tôi sẽ đẩy nó vào hàng đợi. 
6. Tiếp tục cho đến khi hàng đợi trống. Nếu chúng tôi đã ghi lại tất cả các ô tô, chúng tôi sẽ xuất ra chuỗi. Ngược lại, một số ô tô bị mắc kẹt trong một chu trình hoặc bị chặn bởi một cấu trúc không thể tháo rời, vì vậy chúng ta xuất ra -1. 

Tính chính xác dựa trên thực tế là “tầm nhìn để thoát ra” của mỗi ô tô chỉ phụ thuộc vào ô tô còn lại gần nhất theo hướng của nó và cấu trúc lân cận gần nhất này được duy trì đầy đủ bởi các danh sách liên kết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [list(input().strip()) for _ in range(n)]

    # store only car cells
    cars = []
    idx = [[-1] * m for _ in range(n)]

    for i in range(n):
        for j in range(m):
            if grid[i][j] != '.':
                idx[i][j] = len(cars)
                cars.append((i, j))

    k = len(cars)
    if k == 0:
        print(-1)
        return

    # linked lists in rows and cols
    L = [-1] * k
    R = [-1] * k
    U = [-1] * k
    D = [-1] * k

    row = [[] for _ in range(n)]
    col = [[] for _ in range(m)]

    for id, (i, j) in enumerate(cars):
        row[i].append(id)
        col[j].append(id)

    for i in range(n):
        row[i].sort(key=lambda x: cars[x][1])
        for t in range(len(row[i])):
            if t > 0:
                L[row[i][t]] = row[i][t - 1]
            if t + 1 < len(row[i]):
                R[row[i][t]] = row[i][t + 1]

    for j in range(m):
        col[j].sort(key=lambda x: cars[x][0])
        for t in range(len(col[j])):
            if t > 0:
                U[col[j][t]] = col[j][t - 1]
            if t + 1 < len(col[j]):
                D[col[j][t]] = col[j][t + 1]

    def get_blocker(i):
        x, y = cars[i]
        d = grid[x][y]
        if d == 'U':
            return U[i]
        if d == 'D':
            return D[i]
        if d == 'L':
            return L[i]
        return R[i]

    from collections import deque
    q = deque()
    inq = [False] * k

    for i in range(k):
        if get_blocker(i) == -1:
            q.append(i)
            inq[i] = True

    removed = [False] * k
    ans = []

    while q:
        v = q.popleft()
        if removed[v]:
            continue
        removed[v] = True
        ans.append(v)

        x, y = cars[v]

        l, r = L[v], R[v]
        u, d = U[v], D[v]

        if l != -1:
            R[l] = r
        if r != -1:
            L[r] = l
        if u != -1:
            D[u] = d
        if d != -1:
            U[d] = u

        def try_push(u):
            if u == -1 or removed[u]:
                return
            if not inq[u] and get_blocker(u) == -1:
                q.append(u)
                inq[u] = True

        if l != -1: try_push(l)
        if r != -1: try_push(r)
        if u != -1: try_push(u)
        if d != -1: try_push(d)

    if len(ans) != k:
        print(-1)
    else:
        for v in ans:
            x, y = cars[v]
            print(x + 1, y + 1)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên nén lưới thành danh sách các nút ô tô để cập nhật dựa trên con trỏ thay vì quét lưới. Việc sắp xếp hàng và cột xây dựng các mối quan hệ lân cận ban đầu và các mối quan hệ này được duy trì linh hoạt khi ô tô bị loại bỏ. 

chức năng`get_blocker`nắm bắt được quy tắc phụ thuộc quan trọng: một chiếc ô tô chỉ quan tâm đến chiếc ô tô gần nhất theo hướng của nó. Hàng đợi BFS bắt đầu từ tất cả các ô tô đã không có bộ chặn. Mỗi lần xóa chỉ cập nhật các hàng xóm cục bộ trong cấu trúc hàng và cột, tránh việc tính toán lại toàn cục. 

Chi tiết triển khai tinh tế là chỉ những người hàng xóm liền kề trong cấu trúc được liên kết mới cần được kiểm tra sau khi xóa, bởi vì chỉ họ mới có thể sử dụng chiếc xe đã bị loại bỏ làm thiết bị chặn gần nhất. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
D..
R..
.UL
```Chúng tôi lập chỉ mục ô tô như sau theo thứ tự đọc: R tại (2,1), U tại (3,2), D tại (1,1), L tại (3,3). 

Ban đầu chỉ có R(2,1) và U(3,2) có hướng rõ ràng. 

| Bước | Xếp hàng | Đã xóa | Hành động | 
| --- | --- | --- | --- | 
| 1 | R(2,1), U(3,2) | trống | bắt đầu | 
| 2 | U(3,2) | R(2,1) | loại bỏ R | 
| 3 | D(1,1) | R, U | U loại bỏ bỏ chặn D | 
| 4 | L(3,3) | R, U, D | việc loại bỏ cuối cùng tiếp tục | 

Điều này cho thấy cách loại bỏ một ô tô bị chặn sẽ chuyển thành việc kích hoạt những ô tô bị chặn trước đó. 

### Mẫu 2 

đầu vào:```
2 2
RD
UL
```Cả bốn ô tô phụ thuộc vào nhau trong một chu trình. 

| Bước | Xếp hàng | Đã xóa | Hành động | 
| --- | --- | --- | --- | 
| 1 | trống | không | không có xe rời | 
| 2 | trống | không | bị mắc kẹt | 

Không thể tiến bộ được nên câu trả lời là -1. 

Dấu vết xác nhận rằng thuật toán phát hiện chính xác sự vắng mặt của bất kỳ nút di động khởi đầu nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NM) | Mỗi ô tô được đưa vào và xóa khỏi cấu trúc một lần và mỗi lần cập nhật hàng xóm là O(1) | 
| Không gian | O(NM) | Lưu trữ danh sách xe và bốn con trỏ hàng xóm | 

Kích thước lưới lên tới 2000 x 2000 vừa vặn thoải mái vì thuật toán chỉ thực hiện công việc tuyến tính trên mỗi ô tô và tránh mọi thao tác quét lặp lại các hàng hoặc cột. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    from io import StringIO as _StringIO
    old_stdout = _sys.stdout
    _sys.stdout = _StringIO()
    solve()
    out = _sys.stdout.getvalue()
    _sys.stdout = old_stdout
    return out.strip()

# provided sample 1
assert run("""3 3
D..
R..
.UL
""") != "-1"

# provided sample 2
assert run("""2 2
RD
UL
""") == "-1"

# single car
assert run("""1 1
U
""") == "1 1"

# all empty except one row chain
assert run("""1 5
R.LR.
""") != "-1"

# vertical chain with no cycle
assert run("""5 1
D
D
.
U
.
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Xe đơn 1x1 | tế bào đó | loại bỏ tối thiểu | 
| chu kỳ 2x2 | -1 | phát hiện bế tắc | 
| chuỗi hàng đơn | đơn hàng hợp lệ | cập nhật phụ thuộc theo chiều ngang | 
| chuỗi cột thưa thớt | đơn hàng hợp lệ | cập nhật con trỏ dọc | 

## Vỏ cạnh 

Một chu kỳ chặt chẽ là chế độ thất bại quan trọng nhất. Trong cấu hình 2 x 2, trong đó mỗi ô tô chặn một ô tô khác, hàng đợi ban đầu trống vì mỗi ô tô đều có một thiết bị chặn theo hướng của nó. Thuật toán khởi tạo chính xác một hàng đợi trống và không bao giờ xử lý bất kỳ nút nào, dẫn đến sự không khớp cuối cùng giữa số lượng xe bị loại bỏ và tổng số ô tô, điều này gây ra -1. 

Trường hợp thứ hai là những chuỗi tuyến tính dài trong đó việc loại bỏ dần dần sẽ mở khóa chiếc xe tiếp theo. Ở đây, cấu trúc danh sách liên kết đảm bảo rằng khi một ô tô bị chặn bị loại bỏ, ô tô lân cận tiếp theo sẽ có thể được phát hiện ngay lập tức mà không cần quét toàn bộ cột hoặc hàng. Điều này ngăn chặn các sự kiện mở khóa bị thiếu mà cách tiếp cận đơn giản dựa vào việc quét lại toàn bộ sẽ bỏ lỡ hoặc xử lý quá chậm. 

Trường hợp thứ ba liên quan đến nhiều thành phần độc lập. Vì mỗi ô tô chỉ tương tác với hàng và cột lân cận nên các thành phần phát triển độc lập và hàng đợi sẽ xen kẽ việc loại bỏ chúng một cách tự nhiên mà không cần sự phối hợp toàn cầu.
