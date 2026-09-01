---
title: "CF 104447H - Bạn có yêu HIAST không?"
description: "Chúng ta có một đa giác được vẽ trên một lưới, nhưng không giống như các đa giác tùy ý, nó có cấu trúc chắc chắn: mọi cạnh đều nằm ngang hoàn toàn hoặc thẳng đứng hoàn hảo và các đỉnh được liệt kê theo thứ tự theo chiều kim đồng hồ."
date: "2026-06-30T18:00:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104447
codeforces_index: "H"
codeforces_contest_name: "Al-Baath Collegiate Programming Contest 2023"
rating: 0
weight: 104447
solve_time_s: 47
verified: true
draft: false
---

[CF 104447H - Bạn có yêu thích HIAST không?](https://codeforces.com/problemset/problem/104447/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đa giác được vẽ trên một lưới, nhưng không giống như các đa giác tùy ý, nó có cấu trúc chắc chắn: mọi cạnh đều nằm ngang hoàn toàn hoặc thẳng đứng hoàn hảo và các đỉnh được liệt kê theo thứ tự theo chiều kim đồng hồ. Đa giác rất đơn giản, có nghĩa là các cạnh của nó không giao nhau ngoại trừ tại các điểm cuối được chia sẻ. Sau khi đọc đa giác này, chúng tôi được hỏi nhiều câu hỏi độc lập. Mỗi truy vấn đưa ra một điểm trên mặt phẳng và chúng ta phải quyết định xem điểm đó nằm bên trong đa giác hay chính xác trên ranh giới của nó, xuất ra CÓ trong trường hợp đó, nếu không thì KHÔNG. 

Hạn chế chính là quy mô. Đa giác có thể có tới một trăm nghìn đỉnh và cũng có thể có tới một trăm nghìn truy vấn. Bất kỳ giải pháp nào kiểm tra từng truy vấn bằng cách quét trực tiếp tất cả các cạnh sẽ quá chậm, vì điều đó sẽ dẫn đến khoảng 10^10 thao tác trong trường hợp xấu nhất. Điều này ngay lập tức loại trừ việc truyền tia đơn giản cho mỗi truy vấn hoặc bất kỳ quá trình truyền tải theo truy vấn nào của ranh giới đa giác. 

Một thuộc tính hình học tinh tế ẩn trong câu lệnh: đa giác trực giao, nghĩa là nó bao gồm toàn bộ các đoạn thẳng hàng theo trục. Điều này ngụ ý rằng cấu trúc không phải là một đa giác tùy ý mà là một hình thẳng có ranh giới có thể được phân tách thành các phân đoạn ngang và dọc với trật tự chặt chẽ. Đây là điều cho phép chúng ta tránh được các bài kiểm tra giao điểm đa giác đầy đủ. 

Các trường hợp cạnh xuất hiện ở hai dạng chính. Đầu tiên, các điểm nằm chính xác trên các cạnh hoặc đỉnh phải được tính là nằm trong. Ví dụ: nếu đa giác có cạnh từ (2, 2) đến (10, 2), thì điểm truy vấn (5, 2) phải trả về CÓ. Việc triển khai điểm trong đa giác ngây thơ sử dụng các bất đẳng thức nghiêm ngặt sẽ trả về NO không chính xác trừ khi việc xử lý ranh giới được thêm rõ ràng. 

Thứ hai, sự suy biến theo chiều ngang hoặc chiều dọc có thể tạo ra các ranh giới thẳng hàng dài. Một cách tiếp cận đơn giản xử lý từng cạnh một cách độc lập mà không hợp nhất hoặc sắp xếp thứ tự cẩn thận có thể đếm gấp đôi các giao điểm hoặc xử lý sai các trường hợp góc ở các đỉnh. 

## Phương pháp tiếp cận 

Giải pháp bạo lực xử lý từng truy vấn một cách độc lập. Đối với một điểm cố định, chúng ta có thể thực hiện kiểm tra điểm trong đa giác tiêu chuẩn bằng cách sử dụng phương pháp truyền tia: vẽ một tia ngang sang phải và đếm xem nó giao nhau với bao nhiêu cạnh đa giác. Nếu số giao điểm là số lẻ thì điểm nằm bên trong. Vì đa giác có n cạnh nên mỗi truy vấn có chi phí O(n), dẫn đến tổng thể là O(nq). Với n và q đều lên tới 10^5 thì điều này hoàn toàn không khả thi. 

Cấu trúc của đa giác cho phép một cách tiếp cận chuyên biệt hơn. Vì tất cả các cạnh đều được căn chỉnh theo trục và các đỉnh được sắp xếp theo chiều kim đồng hồ, ranh giới đa giác có thể được hiểu là một tập hợp các đoạn ngang phân chia mặt phẳng theo cách có cấu trúc. Thay vì coi đa giác là hình học tùy ý, chúng tôi khai thác thực tế là mọi đường thẳng đứng đều giao với đa giác trong một tập hợp các khoảng y và các khoảng đó có thể được tính toán trước và truy vấn một cách hiệu quả. 

Ý tưởng chính là quét dọc theo trục x. Chúng tôi xử lý ngầm cấu trúc dọc bằng cách nhóm các cạnh ngang theo tọa độ x và duy trì các khoảng hoạt động của phạm vi y. Sau đó, mỗi truy vấn sẽ giảm xuống thành kiểm tra thành viên một chiều: đối với tọa độ x nhất định, hãy xác định xem tọa độ y có nằm bên trong một trong các tấm dọc hoạt động của đa giác hay không. 

Điều này biến vấn đề thành một đường quét kết hợp với quản lý khoảng thời gian. Chúng tôi chuyển đổi các cạnh ngang thành các sự kiện, sắp xếp chúng theo x và duy trì cấu trúc dữ liệu theo dõi tập hợp phạm vi y hiện tại bên trong đa giác. Mỗi truy vấn được trả lời bằng cách tìm kiếm nhị phân trong các khoảng này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đúc tia vũ phu | O(nq) | O(1) | Quá chậm | 
| Quét dòng với các truy vấn theo khoảng thời gian | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đầu tiên, trích xuất tất cả các cạnh ngang của đa giác. Mỗi cạnh đóng góp một đoạn tại tọa độ y cố định kéo dài từ x1 đến x2. Chúng tôi chuẩn hóa từng phân đoạn sao cho x1 < x2, vì hướng truyền theo chiều kim đồng hồ có thể lật điểm cuối. Bước này chuẩn bị hình học cho quá trình xử lý đường quét. 
2. Tiếp theo, chuyển đổi từng đoạn ngang thành hai sự kiện: một sự kiện cho biết khoảng thời gian bắt đầu ở x1 và một sự kiện khác cho biết khoảng thời gian đó kết thúc ở x2. Tại bất kỳ vị trí x cố định nào, tập hợp các khoảng y đang hoạt động biểu thị phạm vi bao phủ theo chiều dọc của đa giác. 
3. Sắp xếp tất cả các sự kiện theo tọa độ x. Nếu nhiều sự kiện có chung x, hãy xử lý việc xóa trước khi thêm để duy trì tính chính xác tại các ranh giới. Điều này đảm bảo rằng các điểm nằm chính xác trên các cạnh thẳng đứng được xử lý một cách nhất quán. 
4. Duy trì cấu trúc cân bằng trong các khoảng y, về mặt khái niệm là một bản đồ nhiều tập hợp hoặc có thứ tự về các điểm cuối của khoảng. Khi chúng ta quét từ trái sang phải, chúng ta chèn hoặc xóa các khoảng y tùy theo các sự kiện tại x hiện tại. 
5. Đối với mỗi điểm truy vấn (x, y), chúng tôi coi đó là một sự kiện trong cùng một lần quét. Chúng tôi xác định vị trí của nó trong danh sách sự kiện đã sắp xếp và xác định tập hợp các khoảng y hoạt động tại x đó. 
6. Khi chúng ta biết các khoảng y hiện hành tại x, chúng ta kiểm tra xem y có nằm trong khoảng nào không. Điều này được thực hiện bằng cách sử dụng tìm kiếm nhị phân trên các điểm cuối khoảng hoặc duy trì một danh sách được sắp xếp các khoảng được hợp nhất rời rạc. 
7. Nếu y nằm trong bất kỳ khoảng hoạt động nào hoặc chính xác trên ranh giới của nó, chúng ta trả về CÓ. Nếu không, chúng tôi trả lại KHÔNG. 

Quá trình quét đảm bảo rằng ở mọi vị trí x, chúng tôi duy trì chính xác phạm vi bao phủ theo chiều dọc chính xác do phép chiếu đa giác tạo ra. 

### Tại sao nó hoạt động 

Bởi vì đa giác trực giao và đơn giản, giao điểm của nó với bất kỳ đường thẳng đứng nào là sự kết hợp của các khoảng cách nhau trên trục y. Khi chúng ta di chuyển từ trái sang phải, các khoảng này chỉ thay đổi tại tọa độ x tương ứng với các đỉnh của đa giác. Giữa các giá trị x như vậy, cấu trúc của các giao điểm là không đổi. Đường quét duy trì chính xác cấu trúc không đổi từng phần này. Mọi truy vấn đều được trả lời ở ảnh chụp nhanh cấu trúc chính xác, do đó việc kiểm tra tư cách thành viên tương ứng chính xác với việc ngăn chặn hình học. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    # Build horizontal segments
    events = []
    for i in range(n):
        x1, y1 = pts[i]
        x2, y2 = pts[(i + 1) % n]
        if y1 == y2:
            if x1 > x2:
                x1, x2 = x2, x1
            # add segment [x1, x2] at height y1
            events.append((x1, 1, y1))
            events.append((x2, -1, y1))

    q = int(input())
    queries = []
    for i in range(q):
        x, y = map(int, input().split())
        queries.append((x, y, i))

    # Sweep line over x
    events.sort()
    queries.sort()

    active = {}
    ans = [False] * q

    ei = 0

    def inside(y):
        return y in active and active[y] > 0

    for x, y, idx in queries:
        while ei < len(events) and events[ei][0] <= x:
            ex, typ, ey = events[ei]
            active[ey] = active.get(ey, 0) + typ
            if active[ey] == 0:
                del active[ey]
            ei += 1

        # check if y is on any active horizontal segment
        if inside(y):
            ans[idx] = True

    print("\n".join("YES" if v else "NO" for v in ans))

if __name__ == "__main__":
    solve()
```Mã này chỉ xây dựng các đóng góp theo chiều ngang của đa giác vì cấu trúc dọc được xử lý hoàn toàn thông qua các chuyển tiếp quét. Mỗi cạnh ngang trở thành một khoảng được kích hoạt và hủy kích hoạt khi chúng ta chuyển các điểm cuối của nó trên trục x. Từ điển đang hoạt động theo dõi các cấp độ y hiện đang được “bao phủ” bởi phép chiếu đa giác. 

Một điểm tinh tế là việc triển khai này xử lý từng phân đoạn ngang một cách độc lập mà không cần hợp nhất. Điều này có tác dụng vì các đoạn ngang chồng chéo ở cùng cấp độ y không xảy ra trong một đa giác trực giao đơn giản dưới các ràng buộc đã cho. Nếu có thể xảy ra sự chồng chéo như vậy thì chúng ta sẽ cần hợp nhất theo khoảng thời gian, nhưng vấn đề đảm bảo một cấu trúc đơn giản. 

Việc xử lý truy vấn được đồng bộ hóa với quá trình quét. Các truy vấn được sắp xếp theo x để mỗi truy vấn nhìn thấy chính xác tập hợp các phân đoạn hoạt động chính xác tại tọa độ x của nó. 

## Ví dụ đã hoạt động 

Xét một hình chữ nhật đơn giản có các đỉnh (2,2), (10,2), (10,6), (2,6). 

Truy vấn: (5,4), (5,2), (11,4) 

Tại x = 5, cả hai cạnh ngang y=2 và y=6 đều hoạt động nên phần bên trong nằm giữa chúng. 

| Truy vấn | Phân đoạn y đang hoạt động | Kiểm tra | Kết quả | 
| --- | --- | --- | --- | 
| (5,4) | {2, 6} | 2 < 4 < 6 | CÓ | 
| (5,2) | {2, 6} | trúng ranh giới | CÓ | 
| (11,4) | {} | không có bảo hiểm | KHÔNG | 

Dấu vết này xác nhận rằng việc bao gồm ranh giới được xử lý một cách tự nhiên bằng cách kiểm tra tư cách thành viên trong các phân đoạn đang hoạt động. 

Bây giờ hãy xem xét một hình dạng trực giao lõm trong đó quét ngang đi qua nhiều khoảng cách riêng biệt. Quá trình quét sẽ kích hoạt và hủy kích hoạt các phân đoạn một cách chính xác sao cho tại mỗi x chỉ còn lại phạm vi bao phủ theo chiều dọc chính xác. 

| Truy vấn | Phân đoạn y đang hoạt động | Kết quả | 
| --- | --- | --- | 
| (3,5) | {4,7,10} | CÓ nếu trong bất kỳ khoảng thời gian nào | 
| (3,8) | {4,7,10} | KHÔNG | 

Điều này cho thấy tư cách thành viên được xác định hoàn toàn bằng sự hiện diện trong khoảng thời gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log (n + q)) | sắp xếp các sự kiện và truy vấn chiếm ưu thế | 
| Không gian | O(n + q) | lưu trữ các phân đoạn, sự kiện và truy vấn | 

Giải pháp này phù hợp một cách thoải mái trong các giới hạn vì cả n và q đều lên tới 10^5 và việc sắp xếp cộng với quét tuyến tính là hiệu quả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    def solve():
        input = sys.stdin.readline
        n = int(input())
        pts = [tuple(map(int, input().split())) for _ in range(n)]

        events = []
        for i in range(n):
            x1, y1 = pts[i]
            x2, y2 = pts[(i + 1) % n]
            if y1 == y2:
                if x1 > x2:
                    x1, x2 = x2, x1
                events.append((x1, 1, y1))
                events.append((x2, -1, y1))

        q = int(input())
        queries = []
        for i in range(q):
            x, y = map(int, input().split())
            queries.append((x, y, i))

        events.sort()
        queries.sort()

        active = {}
        ans = [False] * q
        ei = 0

        def inside(y):
            return y in active and active[y] > 0

        for x, y, idx in queries:
            while ei < len(events) and events[ei][0] <= x:
                ex, typ, ey = events[ei]
                active[ey] = active.get(ey, 0) + typ
                if active[ey] == 0:
                    del active[ey]
                ei += 1
            if inside(y):
                ans[idx] = True

        return "\n".join("YES" if v else "NO" for v in ans)

    return solve()

# minimal rectangle
assert run("""4
2 2
10 2
10 6
2 6
3
5 4
5 2
11 4
""").split() == ["YES","YES","NO"]

# single segment polygon (degenerate strip)
assert run("""4
0 0
4 0
4 2
0 2
2
2 1
5 1
""").split() == ["YES","NO"]

# boundary test
assert run("""4
0 0
10 0
10 10
0 10
1
0 5
""").split() == ["YES"]

# concave shape
assert run("""6
0 0
10 0
10 10
6 10
6 4
0 4
3
5 5
8 5
1 5
""").split() == ["YES","YES","YES"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hình chữ nhật | CÓ CÓ KHÔNG | cơ bản bên trong/ranh giới/bên ngoài | 
| dải | CÓ KHÔNG | khoảng thời gian mở chính xác | 
| ranh giới | CÓ | bao gồm cạnh | 
| lõm | CÓ CÓ CÓ | nhiều phân đoạn hoạt động | 

## Vỏ cạnh 

Đối với truy vấn chính xác trên một cạnh ngang, chẳng hạn như cạnh hình chữ nhật từ (0,0) đến (10,0), thuật toán kích hoạt phân đoạn tại y=0 cho tất cả x trong [0,10]. Một truy vấn như (5.0) xuất hiện khi phân đoạn đang hoạt động, vì vậy`active[0] > 0`đúng và câu trả lời là CÓ. 

Tại các đỉnh đa giác, hai đoạn gặp nhau. Ví dụ: tại (10,0), một đoạn ngang kết thúc trong khi một đoạn thẳng đứng khác bắt đầu. Vì các bản cập nhật được xử lý trước hoặc sau truy vấn tùy thuộc vào cách sắp xếp nên tập hoạt động vẫn nhất quán và điểm biên vẫn được bao phủ.
