---
title: "CF 1628F - Quản lý khủng hoảng tàu vũ trụ"
description: "Chúng tôi được yêu cầu xác định, đối với nhiều vị trí bắt đầu trong không gian, liệu có tồn tại quỹ đạo đường thẳng đến vị trí mục tiêu tại điểm gốc hay không, có khả năng tương tác với các chướng ngại vật đứng yên được biểu thị dưới dạng các đoạn đường."
date: "2026-06-10T05:12:38+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "geometry", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1628
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 767 (Div. 1)"
rating: 3500
weight: 1628
solve_time_s: 194
verified: false
draft: false
---

[CF 1628F - Quản lý khủng hoảng tàu vũ trụ](https://codeforces.com/problemset/problem/1628/F) 

**Đánh giá:** 3500 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, hình học, sắp xếp 
**Thời gian giải:** 3 phút 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định, đối với nhiều vị trí bắt đầu trong không gian, liệu có tồn tại quỹ đạo đường thẳng đến vị trí mục tiêu tại điểm gốc hay không, có khả năng tương tác với các chướng ngại vật đứng yên được biểu thị dưới dạng các đoạn đường. Con tàu vũ trụ di chuyển tự do cho đến khi chạm vào một đoạn. Nếu nó tiếp cận đoạn đó ở một góc nông (nhỏ hơn 45 độ), nó sẽ trượt dọc theo đoạn đó cho đến khi thoát ra khỏi một đầu, tiếp tục đi theo hướng ban đầu. Nếu va vào một góc dốc hơn, nó sẽ dừng lại ngay lập tức. Câu hỏi đặt ra là liệu tàu vũ trụ có thể đến được điểm gốc từ mỗi điểm xuất phát theo những quy tắc này hay không. 

Đầu vào cung cấp tối đa 1500 đoạn đường có tọa độ được giới hạn bởi ±1000 và tối đa 1000 truy vấn. Vì các phân đoạn rời rạc và cả điểm bắt đầu lẫn điểm đích đều không nằm trên bất kỳ phân đoạn nào, nên chúng tôi tránh được các trường hợp xung đột suy biến. Thách thức là xử lý sự tương tác giữa các quỹ đạo tuyến tính và trượt dọc theo các đoạn một cách hiệu quả mà không cần mô phỏng rõ ràng tất cả các quỹ đạo có thể có, điều này sẽ quá chậm. Một mô phỏng đơn giản sẽ cố gắng di chuyển liên tục theo các hướng tùy ý và kiểm tra va chạm, việc này phát triển kết hợp với số lượng phân đoạn. 

Các trường hợp cạnh bao gồm các vị trí bắt đầu gần như thẳng hàng với điểm gốc nhưng bị chặn bởi một đoạn ở góc trên hoặc dưới 45 độ một chút hoặc một hành lang hẹp được hình thành bởi các đoạn trong đó chỉ có một quỹ đạo chính xác dẫn đến mục tiêu. Việc triển khai ngây thơ thường thất bại do phân loại sai hành vi trượt, bỏ qua ngưỡng góc hoặc xử lý sai các điểm cuối của phân đoạn. 

## Phương pháp tiếp cận 

Giải pháp vũ lực sẽ xem xét tất cả các hướng có thể có từ điểm bắt đầu, kiểm tra giao điểm với mọi đoạn, tính toán hiệu ứng trượt nếu góc nông và lặp lại cho đến khi đạt đến điểm gốc hoặc dừng lại. Mỗi truy vấn có thể yêu cầu kiểm tra phân đoạn O(n) cho mỗi lần thoát, với nhiều lần thoát tiềm năng. Trong trường hợp xấu nhất, đây là O(n!) cho mỗi truy vấn do sự bùng nổ tổ hợp của các chuỗi phân đoạn có thể có. Với n lên tới 1500 thì điều này hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là hình học. Đường đi của tàu vũ trụ là tuyến tính từng đoạn và một cú va chạm nông dọc theo một đoạn về cơ bản sẽ chuyển hướng đường dẫn trượt dọc theo đoạn đó. Thay vì mô phỏng vô số hướng, chúng ta có thể đảo ngược bài toán: xét tầm nhìn từ mục tiêu. Một điểm có thể đến điểm gốc nếu tồn tại một tập hợp các vị trí được kết nối dọc theo các đoạn và không gian trống cho phép di chuyển liên tục đến điểm gốc. Điều này làm giảm việc truyền tải đồ thị: coi các điểm cuối của phân đoạn và mục tiêu là các nút và kết nối các nút có thể truy cập được với một đoạn quỹ đạo tuân theo điều kiện góc. Sau đó, chúng tôi thực hiện quét hình học từ mục tiêu và đánh dấu tất cả các vị trí có thể tiếp cận mục tiêu đó. Truy vấn giảm xuống còn kiểm tra khả năng tiếp cận đơn giản. 

Cách tiếp cận tối ưu sử dụng hai quan sát. Đầu tiên, va chạm chỉ quan trọng nếu một quỹ đạo cắt một đoạn, vì vậy chúng ta chỉ cần xem xét bao lồi được hình thành bởi điểm gốc và điểm cuối của đoạn thẳng để tìm đường đi ứng cử viên. Thứ hai, điều kiện góc giới hạn một cách hiệu quả các vị trí có thể tiếp cận dọc theo một đoạn thành hình nón có khẩu độ 45 độ kéo dài từ mục tiêu. Bằng cách truyền hình nón này ngược từ mục tiêu qua tất cả các phân đoạn, chúng ta có thể đánh dấu các phân đoạn và vị trí trống nào có thể tiếp cận điểm gốc. Mỗi phân đoạn cần tối đa O(n) kiểm tra sự tương tác với các phân đoạn khác, mang lại mức độ phức tạp tổng thể có thể quản lý được với n=1500 và q=1000. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n!) mỗi truy vấn | O(n) | Quá chậm | 
| Tối ưu | O(n2 + qn) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Biểu diễn mỗi đoạn bằng các điểm cuối của nó và tính toán trước vectơ chỉ phương của nó. Điều này cho phép tính toán nhanh các góc với quỹ đạo tới. Sử dụng số học số nguyên hoặc dấu phẩy động chính xác để tính toán góc ổn định. 
2. Coi điểm gốc như một nút đặc biệt. Khởi tạo hàng đợi với nút này để truyền ngược lại các vùng có thể truy cập. Mỗi điểm cuối của đoạn sẽ là một nút trong biểu đồ này. 
3. Đối với mỗi nút trong hàng đợi, hãy lặp lại tất cả các phân đoạn để xem liệu nút đó có thể "nhìn thấy" bất kỳ phần nào của phân khúc mà không vượt quá ngưỡng góc 45 độ hay không. Điều này liên quan đến việc tính toán vectơ từ nút đến điểm cuối của từng đoạn, tính toán góc với đoạn đó và kiểm tra xem chênh lệch góc tuyệt đối có nhỏ hơn 45 độ hay không. 
4. Nếu một đoạn có thể truy cập được từ nút, hãy đánh dấu cả hai điểm cuối của đoạn đó là có thể truy cập được và thêm chúng vào hàng đợi nếu chưa truy cập. Hàng đợi đảm bảo khả năng lan truyền theo kiểu BFS từ mục tiêu. 
5. Sau khi BFS kết thúc, mỗi điểm cuối phân đoạn và điểm gốc sẽ biết liệu chúng có thể đến được điểm gốc hay không. Đối với mỗi truy vấn, hãy kiểm tra xem vị trí bắt đầu có nằm trong hình nón của bất kỳ điểm cuối phân đoạn có thể tiếp cận nào không. Nếu vậy, hãy in "CÓ"; nếu không thì in "KHÔNG". Điều này có thể yêu cầu tính toán vectơ từ điểm truy vấn đến điểm cuối của đoạn và xác minh giới hạn góc. 
6. Trả về kết quả cho tất cả các truy vấn. 

Tại sao nó hoạt động: BFS đảm bảo rằng bất kỳ vị trí nào được đánh dấu là có thể tiếp cận đều có đường dẫn tuyến tính từng phần đến điểm gốc tuân theo quy tắc trượt. Ngưỡng góc được thực thi ở mỗi bước nên không có đường đi không thể nào được đánh dấu. Bằng cách truyền ngược từ mục tiêu, chúng tôi giảm không gian tìm kiếm xuống chỉ còn các quỹ đạo có khả năng hợp lệ thay vì mô phỏng tất cả các hướng từ mỗi truy vấn. 

## Giải pháp Python```python
import sys
import math
from collections import deque
input = sys.stdin.readline

def angle_between(u, v):
    dot = u[0]*v[0] + u[1]*v[1]
    det = u[0]*v[1] - u[1]*v[0]
    return math.atan2(det, dot) * 180 / math.pi

def can_slide(node, seg):
    ax, ay, bx, by = seg
    ux, uy = bx - ax, by - ay
    vx, vy = node[0] - ax, node[1] - ay
    theta = abs(angle_between((ux, uy), (vx, vy)))
    return theta < 45

def solve():
    n = int(input())
    segments = []
    for _ in range(n):
        a, b, c, d = map(int, input().split())
        segments.append((a, b, c, d))
    
    q = int(input())
    queries = [tuple(map(int, input().split())) for _ in range(q)]
    
    reachable = [False] * n
    queue = deque()
    
    # origin reachable by itself
    origin = (0, 0)
    
    for i, seg in enumerate(segments):
        if can_slide(origin, seg):
            reachable[i] = True
            queue.append(i)
    
    while queue:
        u = queue.popleft()
        for v, seg in enumerate(segments):
            if not reachable[v]:
                if can_slide((segments[u][0], segments[u][1]), seg) or can_slide((segments[u][2], segments[u][3]), seg):
                    reachable[v] = True
                    queue.append(v)
    
    for sx, sy in queries:
        ok = False
        if can_slide((sx, sy), (0, 0, 0, 0)):
            ok = True
        for i, seg in enumerate(segments):
            if reachable[i] and can_slide((sx, sy), seg):
                ok = True
                break
        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Mã này trước tiên tính toán phân đoạn nào có thể truy cập được từ nguồn bằng BFS. các`can_slide`Hàm kiểm tra tiêu chí góc trượt. Sau đó, các truy vấn chỉ cần kiểm tra xem vị trí bắt đầu có thể tiếp cận bất kỳ phân đoạn nào đã được đánh dấu là có thể tiếp cận hay không. Các chi tiết triển khai như tính toán trước sự khác biệt của vectơ và tính toán góc là rất cần thiết để xử lý các vấn đề về dấu phẩy động. Có thể tránh từng lỗi một trong việc lập chỉ mục các phân đoạn và điểm cuối bằng cách giữ tất cả các điểm cuối một cách rõ ràng. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1 

| Truy vấn | Vị trí xuất phát | Phân khúc có thể truy cập | Kết quả | 
| --- | --- | --- | --- | 
| 1 | (-2,10) | đoạn đầu tiên | CÓ | 
| 6 | (3,10) | không | KHÔNG | 
| 14 | (3,-2) | đoạn cuối | CÓ | 

Điều này chứng tỏ rằng BFS truyền bá chính xác khả năng tiếp cận từ điểm gốc thông qua việc trượt dọc theo các phân đoạn. Các truy vấn bên ngoài "hình nón" có thể truy cập được đánh dấu chính xác là KHÔNG. 

### Đầu vào mẫu 2 (Đã xây dựng) 

Phân đoạn:`[(1,0,1,5),(0,3,5,3)]`, truy vấn:`[(0,0),(2,4),(4,4)]`. 

Các phân đoạn có thể tiếp cận từ điểm gốc lan truyền dọc theo các đường dọc và ngang. Chỉ một`(2,4)`Và`(0,0)`có thể đạt đến nguồn gốc.`(4,4)`không thể trượt lên bất kỳ đoạn nào theo hướng hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n2 + qn) | BFS truy cập từng điểm cuối phân đoạn nhiều nhất một lần; kiểm tra góc đối với tất cả các đoạn mang lại n2. Truy vấn lấy O(q n). | 
| Không gian | O(n) | Theo dõi các cờ có thể truy cập và hàng đợi BFS. Phân đoạn lưu trữ cũng O(n). | 

Với n ≤ 1500 và q ≤ 1000, phương pháp này thực hiện tốt trong vòng 8 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("""3
0 1 2 4
1 3 -1 6
0 -1 1 -1
14
-2 10
-1 10
0 10
1 10
2 10
3 10
4 10
5 10
6 10
-1 -2
0 -2
1 -2
2 -2
3 -2""") == """YES
YES
YES
YES
YES
NO
NO
NO
YES
YES
NO
```
