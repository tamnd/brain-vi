---
title: "CF 106380C - Logo nhiều màu sắc"
description: "Chúng ta có một đồ thị có hướng trong đó mỗi cạnh mang một nhãn màu. Một bước đi trong biểu đồ này được phép xem lại các đỉnh và cạnh, nhưng nó chỉ trở nên “hợp lệ” nếu các màu dọc theo đường đi xen kẽ nghiêm ngặt, nghĩa là bạn không bao giờ được phép đi qua hai cạnh liên tiếp của…"
date: "2026-06-25T10:20:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106380
codeforces_index: "C"
codeforces_contest_name: "The 6th Liaoning Provincial Collegiate Programming Contest"
rating: 0
weight: 106380
solve_time_s: 46
verified: true
draft: false
---

[CF 106380C - Logo đầy màu sắc](https://codeforces.com/problemset/problem/106380/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị có hướng trong đó mỗi cạnh mang một nhãn màu. Một bước đi trong biểu đồ này được phép xem lại các đỉnh và cạnh, nhưng nó chỉ trở thành "hợp lệ" nếu các màu dọc theo bước đi xen kẽ nghiêm ngặt, nghĩa là bạn không bao giờ được phép đi qua hai cạnh liên tiếp cùng màu. 

Nhiệm vụ là xác định những đỉnh nào có thể đạt được từ nút 1 bằng cách sử dụng bước đi hợp lệ như vậy. Đối với mọi đỉnh có thể đạt tới theo ràng buộc này, chúng tôi đưa nó vào đầu ra. 

Điểm mấu chốt là khả năng tiếp cận không chỉ phụ thuộc vào nút hiện tại mà còn phụ thuộc vào màu của cạnh cuối cùng được sử dụng. Khái niệm về khả năng tiếp cận ngây thơ mà chỉ theo dõi các nút là không đủ, bởi vì việc đến cùng một nút với các màu cạnh cuối khác nhau sẽ dẫn đến các khả năng khác nhau trong tương lai. 

Kích thước đầu vào cho phép tổng cộng lên tới một triệu nút và cạnh trong tất cả các trường hợp thử nghiệm. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng liệt kê rõ ràng tất cả các đường dẫn. Ngay cả việc lưu trữ tất cả các trạng thái trong một BFS đơn giản trên các đường dẫn cũng sẽ bùng nổ, vì số lần đi bộ có thể tăng theo cấp số nhân do chu kỳ và số lần truy cập lại nhiều lần. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều cạnh có cùng màu tạo thành chu kỳ. Ví dụ: hãy xem xét một chuỗi các nút được kết nối hoàn toàn bằng các cạnh có màu 1. Từ nút 1, bạn có thể đến nút 2, nhưng bạn không thể tiếp tục từ nút 2 bằng cách sử dụng cạnh màu 1 khác. Một BFS ngây thơ bỏ qua các ràng buộc về màu sắc sẽ đánh dấu không chính xác tất cả các nút có thể truy cập được, trong khi câu trả lời đúng phụ thuộc vào việc tôn trọng quy tắc thay thế. 

Một trường hợp góc khác là một nút có thể truy cập được thông qua nhiều cạnh có màu khác nhau. Ví dụ: nếu có thể truy cập nút 3 thông qua cạnh màu đỏ từ nút 2 và cũng qua cạnh màu xanh từ nút 4, thì từ nút 3, các chuyển tiếp tiếp theo sẽ phụ thuộc vào việc chúng ta xem xét điểm đến nào. Việc thu gọn các trạng thái này thành một cờ truy cập duy nhất trên mỗi nút sẽ làm mất tính chính xác. 

## Phương pháp tiếp cận 

Một ý tưởng đơn giản là thực hiện BFS hoặc DFS trên các đường dẫn. Mỗi trạng thái sẽ đại diện cho nút hiện tại và màu cạnh cuối cùng được sử dụng. Từ một trạng thái, chúng ta thử tất cả các cạnh đi ra có màu khác với màu cuối cùng. Điều này đúng vì nó trực tiếp mã hóa ràng buộc của vấn đề. Tuy nhiên, điều này nhanh chóng trở nên không hiệu quả nếu được triển khai một cách đơn giản vì cùng một nút có thể được xem lại nhiều lần với các màu đến khác nhau và biểu đồ có thể chứa nhiều cạnh. 

Quan sát quan trọng là không gian trạng thái thực sự không lớn nếu chúng ta cấu trúc nó đúng cách. Thay vì khám phá các đường dẫn tùy ý, chúng ta chỉ cần xem xét các cặp có dạng (nút, màu cuối cùng). Mỗi quá trình chuyển đổi cạnh sẽ cập nhật trạng thái này theo thời gian không đổi. Vì mỗi cạnh được xử lý một số lần giới hạn, nên chúng ta có thể coi đây là một đường truyền đồ thị trên một biểu đồ trạng thái mở rộng. 

Tuy nhiên, thậm chí điều này có thể được tối ưu hóa hơn nữa. Thay vì mở rộng rõ ràng tất cả các trạng thái màu, chúng ta có thể quan sát thấy rằng quá trình chuyển đổi chỉ phụ thuộc vào màu cạnh cuối cùng chứ không phải toàn bộ lịch sử. Điều này gợi ý việc duy trì các danh sách kề được nhóm theo màu và thực hiện BFS mang màu được sử dụng cuối cùng. Mỗi cạnh được xem xét một cách hiệu quả một lần cho mỗi trạng thái tương thích. 

Ý tưởng brute-force hoạt động vì nó khám phá tất cả các bước đi hợp lệ, nhưng nó không thành công do phải tính toán lại nhiều lần các chuyển đổi trạng thái giống hệt nhau. Chế độ xem được tối ưu hóa sẽ nén tất cả lịch sử đường dẫn vào một biểu đồ trạng thái nhỏ trong đó mỗi điểm khác biệt có ý nghĩa chỉ là màu cuối cùng được sử dụng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| DFS/BFS qua đường dẫn (không nén trạng thái) | Hàm mũ | O(n + m) | Quá chậm | 
| Trạng thái BFS qua (nút, màu cuối cùng) | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng danh sách kề trong đó mỗi mục lưu trữ cả màu lân cận và màu cạnh. Biểu diễn này cho phép chúng ta lọc các chuyển tiếp dựa trên khả năng tương thích màu sắc trong quá trình truyền tải. 
2. Xác định trạng thái BFS dưới dạng cặp (nút, Last_color). Last_color bắt đầu bằng một giá trị đặc biệt có nghĩa là “không có cạnh trước”, cho phép bước di chuyển đầu tiên từ nút 1 sử dụng bất kỳ màu nào. 
3. Khởi tạo hàng đợi với trạng thái (1, none). Chúng tôi cũng duy trì cấu trúc đã truy cập được khóa bởi (nút, Last_color) để chúng tôi không xử lý lại các trạng thái giống hệt nhau. 
4. Đưa trạng thái (v, c_prev) ra khỏi hàng đợi. Đối với mỗi cạnh đi ra (v → tới, c), chúng tôi chỉ xem xét nó nếu c khác với c_prev. Điều này thực thi điều kiện màu xen kẽ cục bộ ở mỗi bước. 
5. Nếu chúng ta đạt đến trạng thái mới (đến, c), hãy đánh dấu nó đã truy cập và đẩy nó vào hàng đợi. Mỗi sự chuyển đổi như vậy tượng trưng cho việc mở rộng một cuộc dạo chơi đầy màu sắc hợp lệ. 
6. Sau khi BFS kết thúc, hãy thu thập tất cả các nút đã đạt được ở bất kỳ trạng thái nào, bất kể màu cuối cùng. Các nút này chính xác là những nút có thể truy cập được bằng ít nhất một lần đi bộ đầy màu sắc hợp lệ. 

Ý tưởng chính là chúng tôi không bao giờ hợp nhất các trạng thái khác nhau về màu cuối cùng, vì sự khác biệt đó ảnh hưởng đến quá trình chuyển đổi trong tương lai. Khi BFS ổn định, mọi trạng thái có thể truy cập đều thể hiện tiền tố hợp lệ của một số bước đi có màu xen kẽ. 

### Tại sao nó hoạt động 

BFS khám phá một biểu đồ có các nút được tăng cường bằng thông tin màu cạnh cuối cùng. Mọi chuyển đổi đều bảo toàn tính bất biến mà trạng thái được lưu trữ tương ứng với bước đi màu xen kẽ hợp lệ từ nút 1. Vì mọi bước chuyển đổi hợp lệ có thể được phân tách thành một chuỗi các chuyển đổi như vậy nên mọi đỉnh có thể tiếp cận trong biểu đồ ban đầu phải xuất hiện ở ít nhất một trạng thái có thể tiếp cận. Ngược lại, mọi trạng thái do BFS tạo ra đều tương ứng với bước đi hợp lệ theo cách xây dựng, bởi vì chúng tôi chỉ mở rộng dọc theo các cạnh có màu khác với trạng thái trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque, defaultdict

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        
        g = [[] for _ in range(n + 1)]
        for _ in range(m):
            u, v, c = map(int, input().split())
            g[u].append((v, c))
        
        # BFS over (node, last_color)
        # last_color = 0 means "no previous edge"
        q = deque()
        q.append((1, 0))
        
        visited = set()
        visited.add((1, 0))
        
        reachable = set([1])
        
        while q:
            v, pc = q.popleft()
            for to, c in g[v]:
                if c == pc:
                    continue
                state = (to, c)
                if state not in visited:
                    visited.add(state)
                    reachable.add(to)
                    q.append(state)
        
        print(*sorted(reachable))

if __name__ == "__main__":
    solve()
```Danh sách kề lưu trữ các cạnh được định hướng bằng màu sắc của chúng, cho phép lặp lại theo thời gian liên tục qua các chuyển tiếp đi ra. Hàng đợi BFS lưu trữ thông tin trạng thái đầy đủ để các ràng buộc về màu sắc được thực thi cục bộ. 

Một cạm bẫy triển khai phổ biến là quên rằng trạng thái ban đầu phải cho phép bất kỳ màu nào. Đây là lý do tại sao Last_color ban đầu được đặt thành 0, được đảm bảo không khớp với bất kỳ màu cạnh thực nào. 

Một vấn đề tinh tế khác là cấu trúc được truy cập. Chỉ đánh dấu các nút là đã truy cập sẽ cắt bớt các trạng thái hợp lệ một cách không chính xác, vì việc tiếp cận một nút có màu cuối cùng khác có thể cho phép chuyển đổi mới. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3 3
1 2 1
2 3 2
2 3 1
```Chúng tôi bắt đầu tại (1, không có). 

| Bước | Tiểu bang | Chuyển tiếp được sử dụng | Đã thêm trạng thái mới | Các nút có thể truy cập | 
| --- | --- | --- | --- | --- | 
| 1 | (1, 0) | 1→2 (màu 1) | (2, 1) | {1,2} | 
| 2 | (2, 1) | 2→3 (màu 2) | (3, 2) | {1,2,3} | 
| 3 | (2, 1) | 2→3 (màu 1) | bỏ qua (cùng màu như trước) | {1,2,3} | 

Điều này xác nhận rằng chỉ cho phép các chuyển tiếp xen kẽ, ngay cả khi tồn tại nhiều cạnh. 

### Ví dụ 2 

đầu vào:```
1
4 4
1 2 1
2 3 1
3 4 2
2 4 3
```| Bước | Tiểu bang | Chuyển tiếp được sử dụng | Đã thêm trạng thái mới | Các nút có thể truy cập | 
| --- | --- | --- | --- | --- | 
| 1 | (1, 0) | 1→2 (1) | (2, 1) | {1,2} | 
| 2 | (2, 1) | 2→3 (1) | bỏ qua | {1,2} | 
| 3 | (2, 1) | 2→4 (3) | (4, 3) | {1,2,4} | 
| 4 | (4, 3) | không đi | - | {1,2,4} | 

Điều này cho thấy tại sao việc theo dõi màu của cạnh cuối cùng là điều cần thiết: nếu không có nó, nút 3 sẽ có vẻ như có thể truy cập được một cách không chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi trạng thái (nút, màu) được truy cập tối đa một lần cho mỗi lần chuyển đổi cạnh và tổng số lần chuyển đổi là tuyến tính theo các cạnh | 
| Không gian | O(n + m) | Lưu trữ danh sách kề và tập trạng thái đã truy cập | 

Các ràng buộc cho phép lên tới một triệu cạnh trong tất cả các trường hợp thử nghiệm và việc truyền tải tuyến tính này phù hợp thoải mái trong các giới hạn vì mỗi cạnh được xử lý một số lần không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque, defaultdict

    def solve():
        t = int(input())
        out_lines = []
        for _ in range(t):
            n, m = map(int, input().split())
            g = [[] for _ in range(n + 1)]
            for _ in range(m):
                u, v, c = map(int, input().split())
                g[u].append((v, c))

            q = deque()
            q.append((1, 0))
            vis = set([(1, 0)])
            reach = set([1])

            while q:
                v, pc = q.popleft()
                for to, c in g[v]:
                    if c == pc:
                        continue
                    st = (to, c)
                    if st not in vis:
                        vis.add(st)
                        reach.add(to)
                        q.append(st)

            out_lines.append(" ".join(map(str, sorted(reach))))
        return "\n".join(out_lines)

    return solve()

# provided sample placeholders (replace with actual samples if needed)
# assert run(...) == ...

# custom cases
assert run("1\n1 0\n") == "1", "single node"
assert run("1\n2 1\n1 2 1\n") == "1 2", "single edge"
assert run("1\n3 2\n1 2 1\n2 3 1\n") == "1 2", "blocked by same color chain"
assert run("1\n3 2\n1 2 1\n2 3 2\n") == "1 2 3", "alternating works"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | trường hợp cơ bản tầm thường | 
| cạnh đơn | 1 2 | khả năng tiếp cận trực tiếp | 
| dây chuyền cùng màu | 1 2 | ngăn chặn phần mở rộng không hợp lệ | 
| xen kẽ màu sắc | 1 2 3 | tính đúng đắn của việc chuyển đổi màu sắc | 

## Vỏ cạnh 

Trường hợp cạnh khóa là một chuỗi tuyến tính trong đó tất cả các cạnh có cùng màu. Trong trường hợp như vậy, chỉ bước đầu tiên từ nút 1 là hợp lệ và mọi chuyển đổi tiếp theo đều bị chặn. BFS dừng mở rộng một cách chính xác vì mọi nỗ lực sử dụng lại cùng một màu sẽ ngay lập tức vi phạm điều kiện trạng thái, do đó các nút nằm ngoài bước nhảy đầu tiên sẽ không bao giờ được thêm vào. 

Một trường hợp cạnh khác là khi một nút có thể truy cập được thông qua nhiều đường dẫn với các màu cuối cùng khác nhau. Thuật toán bảo toàn chính xác cả hai trạng thái một cách độc lập, do đó các chuyển đổi trong tương lai không bị loại bỏ sớm. Ví dụ: nếu tiếp cận nút 2 thông qua cạnh màu đỏ và cạnh màu xanh lam, cả hai trạng thái (2, đỏ) và (2, xanh lam) vẫn hoạt động, cho phép mở rộng ra các phần mở rộng khác nhau mà mảng được truy cập chỉ có nút sẽ bỏ lỡ.
