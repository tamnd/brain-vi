---
title: "CF 106456C - Vượt Qua"
description: "Chúng ta được cung cấp một lưới trong đó mỗi ô chứa một số nguyên không âm. Khách du lịch bắt đầu từ ô trên cùng bên trái và phải đến ô dưới cùng bên phải. Di chuyển được phép theo bốn hướng và các ô có thể được xem lại bất kỳ số lần nào."
date: "2026-06-21T16:27:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106456
codeforces_index: "C"
codeforces_contest_name: "The 15th Huazhong Agricultural University Programming Contest"
rating: 0
weight: 106456
solve_time_s: 72
verified: true
draft: false
---

[CF 106456C - Vượt qua](https://codeforces.com/problemset/problem/106456/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới trong đó mỗi ô chứa một số nguyên không âm. Khách du lịch bắt đầu từ ô trên cùng bên trái và phải đến ô dưới cùng bên phải. Di chuyển được phép theo bốn hướng và các ô có thể được xem lại bất kỳ số lần nào. 

Một đường dẫn không được đánh giá theo độ dài hoặc chi phí theo nghĩa thông thường. Thay vào đó, chúng tôi lấy chuỗi các giá trị gặp dọc theo đường dẫn, bao gồm cả các giá trị lặp lại khi một ô được truy cập lại. Từ chuỗi này, chúng tôi tính toán hai đại lượng: bitwise OR trên tất cả các giá trị được truy cập và bitwise AND trên tất cả các giá trị được truy cập. 

Mục tiêu là chọn bất kỳ đường dẫn hợp lệ nào từ đầu đến cuối để giảm thiểu OR của tất cả các giá trị được truy cập trước tiên. Trong số tất cả các đường dẫn đạt được OR tối thiểu này, chúng tôi sẽ tối đa hóa AND. 

Khó khăn chính là việc xem lại các nút không thay đổi OR hoặc AND theo cách có lợi. Việc lặp lại một giá trị trong OR hoặc AND là không có tác dụng, do đó, tác dụng duy nhất của việc xem lại là giúp kết nối mà không đưa ra các giá trị mới. Điều này có nghĩa là vấn đề cơ bản là về việc chọn một tập hợp các ô lưới được kết nối có chứa cả hai điểm cuối, trong đó kết nối theo nghĩa lưới. 

Mỗi ô được chọn đóng góp giá trị của nó vào tập hợp OR và AND. OR của đường dẫn chính xác là OR theo bit của tất cả các giá trị trong vùng được kết nối đã chọn, trong khi AND là AND theo bit của tất cả các giá trị trong vùng đó. 

Các ràng buộc ngụ ý tổng số ô lên tới 2 × 10^5 trong các trường hợp thử nghiệm và các giá trị nằm trong phạm vi 30 bit. Điều này loại trừ mọi thứ khám phá tất cả các đường dẫn một cách rõ ràng, vì số lượng đường dẫn trong lưới tăng theo cấp số nhân. Ngay cả các biến thể đường dẫn ngắn nhất ở trạng thái mở rộng cũng sẽ quá lớn nếu chúng ta mã hóa mặt nạ bit trực tiếp trên mỗi trạng thái. 

Một điểm tinh tế xuất hiện trong cả hai mục tiêu. Mục tiêu OR phụ thuộc vào sự kết hợp của các bit trên tất cả các nút được truy cập, trong khi mục tiêu AND phụ thuộc vào giao điểm của các bit trên tất cả các nút được truy cập. Hai mục tiêu này hoạt động theo các hướng đơn điệu trái ngược nhau, khiến cho việc xây dựng DP trực tiếp trở nên khó khăn. 

Một trường hợp thất bại đối với lý luận ngây thơ là giả sử đường đi ngắn nhất là đủ. Ví dụ: đường dẫn giảm thiểu các bước có thể sớm bao gồm một ô có giá trị cao, tăng OR một cách không cần thiết. 

Một trường hợp thất bại khác là coi việc giảm thiểu OR là độc lập cho mỗi bước. Ví dụ: việc chọn cục bộ hàng xóm có giá trị nhỏ nhất có thể bẫy đường dẫn đến việc cần các ô có bit cao sau này, làm tăng OR trên toàn cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các đường dẫn đơn giản có thể có từ đầu đến cuối và tính toán OR và AND cho mỗi đường dẫn. Ngay cả khi chúng tôi không cho phép truy cập lại, số lượng đường dẫn đơn giản trong lưới vẫn theo cấp số nhân tính theo n và m, khiến điều này ngay lập tức không thể thực hiện được. 

Một ý tưởng mạnh mẽ có cấu trúc hơn là coi mỗi đường dẫn là một chuỗi các quyết định và chạy DFS hoặc BFS trên các trạng thái được xác định bởi vị trí và tập hợp đã truy cập. Điều này vẫn bùng nổ vì chỉ riêng bộ đã truy cập đã có kích thước 2^(nm). 

Quan sát quan trọng là cả OR và AND chỉ phụ thuộc vào tập hợp các ô được truy cập, không phụ thuộc vào thứ tự hoặc bội số. Bất kỳ chu trình nào trong một đường dẫn đều không thay đổi OR hoặc AND cuối cùng, vì vậy chúng ta có thể nén bất kỳ bước đi hợp lệ nào vào cây bao trùm trên tập hợp đã truy cập. Điều này làm giảm vấn đề khi chọn một tập hợp các ô được kết nối có chứa cả hai điểm cuối. 

Sau đó chúng tôi tách biệt hai mục tiêu. 

Để giảm thiểu OR, hãy nghĩ đến từng bit một cách độc lập. Nếu chúng tôi quyết định một ứng cử viên HOẶC mặt nạ B, thì chúng tôi đang hạn chế chỉ sử dụng các ô có giá trị không chứa các bit bên ngoài B. Câu hỏi đặt ra là liệu (1,1) và (n,m) có được kết nối trong sơ đồ con do các ô hợp lệ tạo ra hay không. OR tối ưu là mặt nạ nhỏ nhất vẫn cho phép kết nối.

Vì chỉ có 30 bit nên chúng ta có thể tham lam bắt đầu với tất cả các bit được đặt và thử loại bỏ từng bit một từ cao xuống thấp. Sau khi tạm thời loại bỏ một chút, chúng tôi kiểm tra kết nối bằng BFS. Nếu kết nối được giữ nguyên thì bit thực sự không cần thiết trong OR. 

Sau khi sửa mặt nạ OR tối thiểu, chúng ta chuyển sang mục tiêu AND. Bây giờ chúng tôi chỉ xem xét các ô được phép bởi mặt nạ OR đó. Trong số này, chúng tôi muốn có một đường dẫn tối đa hóa bit AND của tất cả các giá trị được truy cập. Một bit có thể xuất hiện trong AND cuối cùng chỉ khi mọi ô trên đường dẫn đều chứa bit đó. Vì vậy, đối với ứng cử viên VÀ mặt nạ C, chúng ta cần một đường dẫn trong đó mỗi ô được truy cập chứa tất cả các bit của C. 

Điều này một lần nữa trở thành một bước kiểm tra kết nối trong biểu đồ được lọc, vì vậy chúng ta có thể tham lam xây dựng mặt nạ AND từng chút một, kiểm tra tính khả thi mỗi lần sử dụng BFS. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Con đường vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Bitmask tham lam với BFS | O(60 × n × m × 30) | O(n×m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### HOẶC giai đoạn giảm thiểu 

1. Bắt đầu với mặt nạ B trong đó tất cả 30 bit được đặt. Điều này đảm bảo mọi ô ban đầu đều được cho phép. 
2. Xử lý các bit từ cao xuống thấp. Với mỗi bit, tạm thời loại bỏ nó khỏi B, tạo thành ứng cử viên B'. 
3. Chạy BFS từ đầu, chỉ truy cập các ô có giá trị không chứa bit nào ngoài B'. Nếu có thể tiếp cận được đầu cuối, hãy tháo mũi khoan ra; nếu không thì khôi phục nó. 
4. Sau khi tất cả các bit được xử lý, B là mặt nạ OR tối thiểu vẫn cho phép đường dẫn hợp lệ. 

Bước BFS đang kiểm tra sự cố kết nối bị hạn chế: liệu lưới có còn được kết nối hay không khi chúng tôi xóa tất cả các ô có giá trị không tương thích với mặt nạ hiện tại. 

### Giai đoạn tối đa hóa VÀ 

1. Giới hạn lưới chỉ ở các ô tương thích với B. 
2. Khởi tạo mặt nạ AND C là 0. 
3. Đối với mỗi bit từ cao đến thấp, tạm đặt nó ở C để tạo thành C'. 
4. Chạy lại BFS, nhưng bây giờ chỉ cho phép truyền qua các ô có giá trị bao gồm tất cả các bit trong C'. Nếu kết nối được giữ, hãy giữ bit; nếu không hãy loại bỏ nó. 
5. C cuối cùng là mức tối đa VÀ có thể đạt được trong số tất cả các đường dẫn tôn trọng ràng buộc OR tối thiểu. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là cả hai mục tiêu đều giảm tính khả thi của kết nối theo các bộ lọc bitwise. Đối với OR, tính khả thi phụ thuộc vào việc loại trừ các bit bị cấm khỏi tất cả các nút được truy cập. Đối với AND, tính khả thi phụ thuộc vào việc thực thi các bit cần thiết trong tất cả các nút được truy cập. Cả hai ràng buộc đều xác định một thuộc tính đơn điệu trên các tập hợp bit: việc thêm các ràng buộc chỉ thu nhỏ tập hợp các ô có thể sử dụng chứ không bao giờ mở rộng nó. Sự đơn điệu này làm cho việc sửa bit tham lam trở nên hợp lệ vì một khi một bit được chứng minh là không cần thiết (hoặc cần thiết), thì quyết định đó vẫn nhất quán cho tất cả các lựa chọn tiếp theo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

DIRS = [(1,0),(-1,0),(0,1),(0,-1)]

def bfs(n, m, grid, allowed_mask, require_mask):
    if (grid[0][0] & allowed_mask) != grid[0][0]:
        return False
    if (grid[0][0] & require_mask) != require_mask:
        return False
    if (grid[n-1][m-1] & allowed_mask) != grid[n-1][m-1]:
        return False
    if (grid[n-1][m-1] & require_mask) != require_mask:
        return False

    vis = [[False]*m for _ in range(n)]
    q = deque([(0,0)])
    vis[0][0] = True

    while q:
        x,y = q.popleft()
        if x == n-1 and y == m-1:
            return True
        for dx,dy in DIRS:
            nx, ny = x+dx, y+dy
            if 0 <= nx < n and 0 <= ny < m and not vis[nx][ny]:
                v = grid[nx][ny]
                if (v & allowed_mask) == v and (v & require_mask) == require_mask:
                    vis[nx][ny] = True
                    q.append((nx, ny))
    return vis[n-1][m-1]

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    grid = [list(map(int, input().split())) for _ in range(n)]

    full = (1 << 30) - 1

    # OR minimization
    B = full
    for b in range(29, -1, -1):
        if B & (1 << b):
            B2 = B ^ (1 << b)
            if bfs(n, m, grid, B2, 0):
                B = B2

    # AND maximization
    C = 0
    for b in range(29, -1, -1):
        C2 = C | (1 << b)
        if bfs(n, m, grid, B, C2):
            C = C2

    print(B, C)
```Hàm BFS được chia sẻ giữa cả hai giai đoạn, nhưng nó thực thi các ràng buộc khác nhau tùy theo giai đoạn. Trong pha OR, require_mask bằng 0 và chỉ các bit bị cấm mới quan trọng. Trong giai đoạn AND, allow_mask được cố định và require_mask thực thi các bit bắt buộc mà mọi nút được truy cập phải chứa. 

Một chi tiết triển khai tinh tế là việc từ chối sớm các nút bắt đầu và kết thúc trước BFS. Điều này tránh việc truyền tải không cần thiết khi một trong hai điểm cuối đã vi phạm các ràng buộc. Một chi tiết quan trọng khác là cả hai ràng buộc phải được giữ đồng thời bên trong quá trình kiểm tra chuyển tiếp BFS, nếu không chúng tôi sẽ vô tình cho phép các nút trung gian không hợp lệ. 

## Ví dụ đã hoạt động 

Hãy xem xét một lưới nhỏ trong đó các đường dẫn khác nhau buộc phải bao gồm các bit khác nhau. Thuật toán không quan tâm trực tiếp đến hình học; nó chỉ quan tâm đến việc lọc theo bit có duy trì được kết nối hay không. 

Đối với pha OR, giả sử loại bỏ một chút ngắt kết nối bắt đầu và kết thúc. BFS sẽ không đến được mục tiêu và bit sẽ được khôi phục. Điều này trực tiếp chứng minh rằng mọi bit được giữ đều có cấu trúc cần thiết để kết nối. 

Đối với giai đoạn AND, giả sử có một bit khi bắt đầu nhưng bị thiếu ở một số vùng trung gian cần thiết cho kết nối. Ngay cả khi điểm cuối thỏa mãn bit, BFS sẽ thất bại vì mỗi bước phải duy trì ràng buộc bit. Điều này chứng tỏ tại sao AND được xác định bởi ràng buộc giao cắt toàn cục thay vì chỉ các giá trị điểm cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(60 × n × m) cho mỗi trường hợp thử nghiệm | Mỗi bit có tối đa 30 bit được kiểm tra hai lần bằng BFS, mỗi BFS truy cập vào mỗi ô một lần | 
| Không gian | O(n×m) | Đã truy cập mảng và hàng đợi cho BFS | 

Tổng số ô trên tất cả các trường hợp thử nghiệm được giới hạn bởi 2 × 10^5, do đó các hoạt động BFS vẫn nằm trong giới hạn thoải mái. Hệ số không đổi từ hai lần truyền tham lam trên 30 bit là nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque

    DIRS = [(1,0),(-1,0),(0,1),(0,-1)]

    def bfs(n, m, grid, allowed_mask, require_mask):
        if (grid[0][0] & allowed_mask) != grid[0][0]:
            return False
        if (grid[0][0] & require_mask) != require_mask:
            return False
        if (grid[n-1][m-1] & allowed_mask) != grid[n-1][m-1]:
            return False
        if (grid[n-1][m-1] & require_mask) != require_mask:
            return False

        vis = [[False]*m for _ in range(n)]
        q = deque([(0,0)])
        vis[0][0] = True

        while q:
            x,y = q.popleft()
            if x == n-1 and y == m-1:
                return True
            for dx,dy in DIRS:
                nx, ny = x+dx, y+dy
                if 0 <= nx < n and 0 <= ny < m and not vis[nx][ny]:
                    v = grid[nx][ny]
                    if (v & allowed_mask) == v and (v & require_mask) == require_mask:
                        vis[nx][ny] = True
                        q.append((nx, ny))
        return vis[n-1][m-1]

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        grid = [list(map(int, input().split())) for _ in range(n)]

        full = (1 << 30) - 1

        B = full
        for b in range(29, -1, -1):
            if B & (1 << b):
                B2 = B ^ (1 << b)
                if bfs(n, m, grid, B2, 0):
                    B = B2

        C = 0
        for b in range(29, -1, -1):
            C2 = C | (1 << b)
            if bfs(n, m, grid, B, C2):
                C = C2

        out.append(f"{B} {C}")

    return "\n".join(out)

# provided sample placeholders
# assert run("...") == "..."
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Giá trị đơn lưới 1x1 | giá trị giá trị | kết nối tầm thường và cả hai giai đoạn | 
| tất cả lưới bằng nhau | giá trị giá trị | AND vẫn tối đa và OR tối thiểu tầm thường | 
| hành lang cưỡng bức | mặt nạ tính toán | độ nhạy kết nối để loại bỏ bit | 
| chia theo chướng ngại vật bit cao | tính toán | HOẶC độ đúng pha | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi đường dẫn hợp lệ duy nhất yêu cầu ô có bit cao. Trong trường hợp đó, các nỗ lực loại bỏ bit đó trong quá trình giảm thiểu OR sẽ không thành công trong BFS, buộc thuật toán phải giữ lại nó. BFS trực tiếp mã hóa lỗi này vì điểm cuối sẽ không thể truy cập được sau khi xóa ô. 

Một trường hợp cạnh khác xảy ra trong tối đa hóa AND khi AND tối ưu bằng 0. Điều này xảy ra khi mọi đường dẫn hợp lệ phải bao gồm ít nhất một ô bị thiếu một bit nhất định. Cấu trúc tham lam tự nhiên hội tụ về 0 vì mọi nỗ lực thiết lập một bit đều không kiểm tra được kết nối, khiến C không thay đổi.
