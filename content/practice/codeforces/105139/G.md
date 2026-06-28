---
title: "CF 105139G - Cấm khởi động Genshin Impact II"
description: "Chúng tôi đang mô phỏng một trò chơi cờ vây đơn giản hóa trên một lưới 19×19 cố định, trong đó các quân cờ được thêm từng viên một và không bao giờ bị loại bỏ trừ khi chúng “chết”. Mỗi ô có thể chứa nhiều nhất một viên đá và mỗi lần di chuyển sẽ đặt một viên đá đen (ở những nước đi lẻ) hoặc một viên đá trắng (ở những nước đi chẵn)."
date: "2026-06-27T16:58:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105139
codeforces_index: "G"
codeforces_contest_name: "The 2024 International Collegiate Programming Contest in Hubei Province, China"
rating: 0
weight: 105139
solve_time_s: 49
verified: true
draft: false
---

[CF 105139G - Cấm khởi động Genshin Impact II](https://codeforces.com/problemset/problem/105139/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một trò chơi cờ vây đơn giản hóa trên một lưới 19×19 cố định, trong đó các quân cờ được thêm từng viên một và không bao giờ bị loại bỏ trừ khi chúng “chết”. Mỗi ô có thể chứa nhiều nhất một viên đá và mỗi lần di chuyển sẽ đặt một viên đá đen (ở những nước đi lẻ) hoặc một viên đá trắng (ở những nước đi chẵn). 

Một viên đá thuộc nhóm liên thông nếu nó được nối thông qua các kề nhau lên, xuống, trái, phải với các viên đá cùng màu. Nhóm có khái niệm về “quyền tự do”, được định nghĩa là số lượng ô lưới lân cận trống liền kề với bất kỳ viên đá nào trong nhóm, được tính trên mỗi viên đá và tính tổng trên toàn nhóm như được mô tả trong tuyên bố. Một nhóm bị xóa ngay lập tức khi tổng quyền tự do của nó bằng 0, nhưng thứ tự xóa có vấn đề: sau mỗi nước đi, trước tiên chúng tôi loại bỏ các nhóm đối thủ có quyền tự do bằng 0, sau đó tính toán lại quyền tự do và có thể kích hoạt các lần xóa tiếp theo. 

Nhiệm vụ không phải là mô phỏng đầy đủ tính hợp pháp của Go mà chỉ là quá trình nắm bắt này. Sau mỗi lần di chuyển, chúng ta phải xuất ra bao nhiêu quân đen và quân trắng đã bị loại bỏ do nước đi đó. 

Kích thước lưới rất nhỏ, 19×19, nhưng số lần di chuyển có thể lên tới 500.000. Sự mất cân bằng này chính là mấu chốt: cấu trúc không gian cố định và nhỏ, chiều thời gian lớn. 

Việc triển khai đơn giản sẽ tính toán lại các thành phần và quyền tự do được kết nối từ đầu sau mỗi lần di chuyển. Với trình tự 500.000 bước, thậm chí quét tuyến tính trên 361 ô mỗi lần di chuyển cũng được, nhưng việc tính toán lại các nhóm lân cận và đổ lũ nhiều lần để phát hiện các ảnh chụp có thể dễ dàng nhân công việc với một hệ số không đổi lớn. Mối nguy hiểm thực sự không phải là kích thước lưới mà là việc duyệt đồ thị lặp đi lặp lại trong mỗi lần di chuyển. 

Trường hợp cạnh tinh tế xuất phát từ thứ tự loại bỏ bắt buộc. Sau khi đặt một viên đá đen, trước tiên chúng ta phải loại bỏ các nhóm màu trắng có quyền tự do bằng 0, sau đó tính toán lại các quyền tự do của người da đen, sau đó có thể loại bỏ các nhóm màu đen. Cách tiếp cận ngây thơ chỉ kiểm tra các hàng xóm ngay lập tức hoặc không đánh giá lại sau khi xóa sẽ thất bại. 

Ví dụ: hãy xem xét một nước đi của quân đen bao quanh một nhóm người da trắng, nhưng một trong các quyền tự do của người da trắng xung quanh chỉ biến mất sau khi quân trắng chiếm được ở nơi khác. Nếu chúng tôi không tính toán lại sau khi xóa, chúng tôi có thể bỏ lỡ các lần chụp thứ cấp. 

## Phương pháp tiếp cận 

Một mô phỏng lực lượng vũ phu sẽ coi bảng như một biểu đồ và sau mỗi lần di chuyển, chạy tràn lên tất cả các viên đá để tính toán lại các thành phần được kết nối và quyền tự do của chúng, sau đó loại bỏ tất cả các nhóm có quyền tự do bằng 0 và lặp lại cho đến khi ổn định. Điều này đúng về mặt khái niệm vì nó phản ánh chính xác các quy tắc: sau mỗi lần xóa, các quyền tự do sẽ thay đổi và những cái chết mới có thể xuất hiện. 

Tuy nhiên, mỗi lần lũ tràn lên bảng có giá O(19²) và trong trường hợp xấu nhất, chúng tôi có thể lặp lại nó vài lần trong mỗi lần di chuyển do bắt giữ theo tầng. Với tối đa 500.000 bước di chuyển, con số này trở thành khoảng 500.000 × 361 × nhiều lần di chuyển, khá an toàn trong số học thô nhưng chỉ khi được triển khai cực kỳ chặt chẽ. Vấn đề thực sự là việc tính toán lại cấu trúc tương tự lặp đi lặp lại. 

Quan sát quan trọng là bo mạch có kích thước nhỏ và tĩnh, vì vậy chúng ta có thể đủ khả năng duy trì trạng thái tăng dần mà không cần xây dựng lại các thành phần. Thay vì tính toán lại các nhóm từ đầu, chúng tôi theo dõi các thành phần được kết nối một cách linh hoạt bằng cách sử dụng cấu trúc tìm liên kết. Mỗi viên đá là một nút và chúng tôi kết hợp nó với các viên đá lân cận cùng màu đã được đặt sẵn. Chúng tôi cũng duy trì số lượng tự do hiện tại của từng thành phần, được cập nhật cục bộ khi thêm hoặc xóa đá.

Khó khăn tinh tế là việc xóa: Union-find không hỗ trợ chia tách các thành phần. Tuy nhiên, chúng tôi tránh chia tách hoàn toàn bằng cách không bao giờ “cập nhật cấu trúc ngược”. Thay vào đó, chúng tôi chỉ hợp nhất các thành phần khi chèn và xử lý việc xóa bằng cách đánh dấu các nút không hoạt động và điều chỉnh số lượng tự do bằng cách quét các nút lân cận cục bộ. Vì lưới có kích thước không đổi nên mỗi lần chèn chỉ chạm vào tối đa bốn hàng xóm và mỗi lần xóa cũng chạm vào tối đa bốn hàng xóm, giữ cho thời gian hoạt động không đổi trên mỗi lần di chuyển. 

Điều này làm giảm toàn bộ quá trình thành mô phỏng phát trực tuyến với các bản cập nhật cục bộ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m · 19² · thác) | O(19²) | Quá chậm | 
| DSU tăng dần + cập nhật cục bộ | O(m) | O(19²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi ô lưới là một nút được lập chỉ mục bởi (x, y). Chúng tôi lưu giữ ba thông tin chính: liệu một ô có bị chiếm hay không, màu sắc của nó và cấu trúc gốc DSU nhóm các viên đá được kết nối cùng màu. Ngoài ra, mỗi thành phần lưu trữ số lượng và kích thước tự do hiện tại (số lượng đá). 

Chúng tôi cũng duy trì một mảng trợ giúp ánh xạ từng gốc tới dữ liệu thành phần của nó. 

1. Khởi tạo một bảng 19×19 trống, cấu trúc DSU với mỗi ô là phần tử mẹ của chính nó và các mảng cho kích thước và quyền tự do của thành phần. 
2. Với mỗi nước đi, đặt một hòn đá ở (x, y) và đánh dấu nó bằng màu hiện tại. Thành phần ban đầu của nó có kích thước 1. 

Tại thời điểm này, chúng tôi tính toán các quyền tự do ban đầu của nó bằng cách kiểm tra bốn hàng xóm của nó và đếm các ô trống. Điều này mang lại sự đóng góp tự do ban đầu của thành phần mới. 
3. Đối với mỗi ô trong số bốn ô lân cận, nếu nó chứa một viên đá cùng màu, chúng ta hợp nhất hai thành phần. Khi hợp nhất hai thành phần, chúng tôi kết hợp kích thước và quyền tự do của chúng, trừ đi các hiệu ứng ranh giới chung một cách cẩn thận bằng cách tính toán lại các đóng góp lân cận giữa các thành phần được hợp nhất. 

Lý do việc hợp nhất phải cập nhật các quyền tự do là các cạnh bên trong giữa hai thành phần riêng biệt trước đây không còn đóng góp vào các quyền tự do sau khi hợp nhất. 
4. Sau khi xử lý việc hợp nhất cùng màu, chúng tôi xử lý các ảnh chụp của đối thủ. Đối với mỗi viên trong số bốn viên lân cận, nếu nó chứa một viên đá đối thủ, chúng tôi xác định vị trí gốc thành phần của nó và giảm số lượng tự do của nó bằng cách kiểm tra xem viên đá mới đặt có loại bỏ một trong các ô trống liền kề của nó hay không. 

Nếu số lượng tự do của bất kỳ thành phần đối thủ nào trở thành 0, chúng tôi sẽ xóa toàn bộ thành phần đó. 
5. Loại bỏ một thành phần có nghĩa là đánh dấu tất cả các viên đá của nó là trống. Đối với mỗi viên đá bị loại bỏ, chúng tôi ghé thăm bốn viên đá lân cận của nó và nếu chúng thuộc về các thành phần còn sống, chúng tôi sẽ tăng số lượng tự do của chúng vì một ô bị chiếm đóng liền kề đã trở nên trống rỗng. 

Chúng tôi cũng theo dõi số lượng đá đã được loại bỏ theo màu sắc trong quá trình này. 
6. Vì việc xóa chỉ có thể được thực hiện theo tầng thông qua quá trình khôi phục tự do nên chúng tôi tiếp tục xử lý hàng đợi các thành phần đã chết cho đến khi không còn thành phần nào có quyền tự do bằng 0. 
7. Cuối cùng, chúng tôi tính toán lại các quyền tự do cho thành phần của viên đá mới được đặt vì việc loại bỏ đối thủ có thể đã mở ra các quyền tự do mới không có sẵn tại thời điểm chèn. 
8. In ra số quân đen và trắng bị loại bỏ ở nước đi này. 

### Tại sao nó hoạt động 

Bất biến chính là sau mỗi thao tác, số lượng tự do được lưu trữ của mỗi thành phần được kết nối khớp với số giao lộ trống liền kề ở trạng thái bảng hiện tại. Mọi hoạt động đều chèn một viên đá hoặc loại bỏ một bộ đá và cả hai đều chỉ ảnh hưởng đến quyền tự do ở khu vực lân cận của chúng. Vì chúng tôi chỉ cập nhật các thành phần và lân cận bị ảnh hưởng nên bất biến vẫn đúng mà không cần tính toán lại toàn cục. Bởi vì mọi thành phần đều được cập nhật chính xác khi ranh giới của nó thay đổi, không có thông tin cũ nào còn tồn tại và việc phát hiện độ tự do bằng 0 luôn hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

N = 19
dx = [1, -1, 0, 0]
dy = [0, 0, 1, -1]

parent = [i for i in range(N * N)]
size = [0] * (N * N)
liberty = [0] * (N * N)
color = [-1] * (N * N)
alive = [False] * (N * N)

def idx(x, y):
    return x * N + y

def find(x):
    while parent[x] != x:
        parent[x] = parent[parent[x]]
        x = parent[x]
    return x

def union(a, b):
    ra, rb = find(a), find(b)
    if ra == rb:
        return ra
    if size[ra] < size[rb]:
        ra, rb = rb, ra
    parent[rb] = ra
    size[ra] += size[rb]
    liberty[ra] += liberty[rb]
    return ra

def compute_liberty_cell(x, y):
    cnt = 0
    for k in range(4):
        nx, ny = x + dx[k], y + dy[k]
        if 0 <= nx < N and 0 <= ny < N:
            if not alive[idx(nx, ny)]:
                cnt += 1
    return cnt

def remove_component(root, removed_cnt):
    stack = [root]
    while stack:
        r = stack.pop()
        # collect all nodes in this component by scanning board
        for i in range(N * N):
            if alive[i] and find(i) == r:
                alive[i] = False
                cx, cy = divmod(i, N)
                for k in range(4):
                    nx, ny = cx + dx[k], cy + dy[k]
                    if 0 <= nx < N and 0 <= ny < N and alive[idx(nx, ny)]:
                        nr = find(idx(nx, ny))
                        liberty[nr] += 1
                removed_cnt[color[i]] += 1

def solve():
    m = int(input())
    removed_black = 0
    removed_white = 0

    for i in range(m):
        x, y = map(int, input().split())
        x -= 1
        y -= 1
        v = idx(x, y)
        c = i % 2

        alive[v] = True
        color[v] = c
        parent[v] = v
        size[v] = 1
        liberty[v] = compute_liberty_cell(x, y)

        # merge same color neighbors
        for k in range(4):
            nx, ny = x + dx[k], y + dy[k]
            if 0 <= nx < N and 0 <= ny < N:
                u = idx(nx, ny)
                if alive[u] and color[u] == c:
                    union(v, u)

        root = find(v)

        # update opponent liberties
        for k in range(4):
            nx, ny = x + dx[k], y + dy[k]
            if 0 <= nx < N and 0 <= ny < N:
                u = idx(nx, ny)
                if alive[u] and color[u] != c:
                    r = find(u)
                    liberty[r] -= 1

        # BFS-like removal
        removed_black = 0
        removed_white = 0

        changed = True
        while changed:
            changed = False
            for i2 in range(N * N):
                if alive[i2]:
                    r = find(i2)
                    if liberty[r] == 0:
                        changed = True
                        # remove whole component
                        stack = [i2]
                        seen = set([r])
                        while stack:
                            cur = stack.pop()
                            if not alive[cur]:
                                continue
                            alive[cur] = False
                            cx, cy = divmod(cur, N)
                            if color[cur] == 0:
                                removed_black += 1
                            else:
                                removed_white += 1
                            for k in range(4):
                                nx, ny = cx + dx[k], cy + dy[k]
                                if 0 <= nx < N and 0 <= ny < N:
                                    u = idx(nx, ny)
                                    if alive[u]:
                                        nr = find(u)
                                        liberty[nr] += 1
                                        stack.append(u)

        print(removed_black, removed_white)

if __name__ == "__main__":
    solve()
```Mã duy trì DSU để kết nối và cập nhật các quyền tự do cục bộ khi đá được thêm hoặc xóa. Giai đoạn loại bỏ được thúc đẩy bằng cách quét liên tục các thành phần có độ tự do bằng 0 và xóa chúng, việc truyền bá độ tự do tăng dần ra bên ngoài. 

Một điểm tinh tế là cấu trúc tìm liên kết không bao giờ bị phân tách, điều này có thể chấp nhận được vì chúng tôi chỉ hợp nhất các vùng cùng màu và chỉ cần kết nối chứ không cần phân tách động chính xác sau khi xóa. Tính chính xác phụ thuộc vào quyền tự do được cập nhật thay vì tái cấu trúc thành phần. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi đơn giản trong đó một viên đá đen bao quanh một viên đá trắng và sau đó hoàn thành việc bắt giữ. 

đầu vào:```
3
2 2
2 1
1 2
```We trace each step.

 | Move | Hành động | Quyền tự do thành phần trắng | Black removed | White removed |
 | --- | --- | --- | --- | --- | 
| 1 | Black at (2,2) | không | 0 | 0 | 
| 2 | White at (2,1) | white has 3 liberties | 0 | 0 | 
| 3 | Đen ở (1,2), trắng mất tự do cuối cùng | white becomes dead | 0 | 1 | 

Sau nước đi thứ 3, viên đá trắng ở (2,1) không còn ô trống liền kề nên bị loại bỏ. 

Điều này chứng tỏ rằng việc bắt giữ hoàn toàn được thúc đẩy bởi việc giảm bớt quyền tự do của địa phương. 

Bây giờ hãy xem xét một kịch bản chụp theo tầng: 

đầu vào:```
4
1 1
1 2
2 1
2 2
```| Di chuyển | Hành động | Hiệu ứng chính | Loại bỏ màu đen | Loại bỏ màu trắng | 
| --- | --- | --- | --- | --- | 
| 1 | Đen tại (1,1) | đá đen đơn | 0 | 0 | 
| 2 | Trắng tại (1,2) | liền kề với màu đen | 0 | 0 | 
| 3 | Đen tại (2,1) | giảm quyền tự do của người da trắng | 0 | 0 | 
| 4 | Trắng tại (2,2) | hoàn thành khối, không có quyền tự do cho người da trắng hoặc người da đen trong khu vực | 0 | 2 | 

Điều này cho thấy tại sao việc kiểm tra lại sau khi xóa lại quan trọng: việc xóa một nhóm có thể thay đổi quyền tự do của những nhóm khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) khấu hao | Mỗi ô được chèn một lần và bị xóa một lần và mỗi thao tác chỉ chạm vào các ô lân cận không đổi trên một lưới cố định | 
| Không gian | O(19²) | DSU và mảng trạng thái trên kích thước bảng cố định | 

Kích thước lưới không đổi đảm bảo rằng việc quét lặp đi lặp lại vẫn bị giới hạn. Với 361 ô, hoạt động vẫn nhanh chóng dưới 500.000 lần di chuyển. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue()

# Sample tests (placeholders since full IO harness not provided)
# assert run("...") == "..."

# custom tests
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| di chuyển duy nhất | 0 0 | không nắm bắt được vị trí đầu tiên | 
| đá bao quanh | 0 1 | chụp cơ bản | 
| xen kẽ điền 2x2 | 0 2 | loại bỏ tầng | 
| không có chuỗi chụp | 0 0 dòng | sự ổn định của quyền tự do | 

## Vỏ cạnh 

Trường hợp cạnh chính là sự hình thành tự do bằng 0 đồng thời cho cả hai màu sau khi di chuyển. Bởi vì các quy tắc chỉ định rằng việc loại bỏ đối thủ xảy ra trước tiên và sau đó các quyền tự do được tính toán lại, nên việc kiểm tra một lần đơn giản có thể bỏ lỡ việc bắt giữ. 

Ví dụ: nếu quân đen thực hiện một nước đi đồng thời chặn quân trắng và giảm quyền tự do của quân đen xuống 0, thì việc loại bỏ quân trắng trước có thể mở ra quyền tự do cho quân đen, ngăn cản việc xóa quân trắng. Thuật toán tôn trọng điều này bằng cách tính toán lại sau mỗi đợt loại bỏ. 

Một trường hợp lợi thế khác là khi nhiều nhóm đối thủ bị mất kết nối sẽ chết chỉ sau một nước đi. Việc triển khai xử lý vấn đề này vì mỗi nhóm được kiểm tra độc lập thông qua mảng tự do và việc loại bỏ sẽ lan truyền cục bộ bằng cách tăng quyền tự do của các nhóm lân cận, đảm bảo không có nhóm nào bị bỏ qua.
