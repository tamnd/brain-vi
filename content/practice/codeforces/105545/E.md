---
title: "CF 105545E - \u041f\u043e\u0440\u0442\u0430\u043b\u044c\u043d\u044b\u0435 \u0441\u043e\u043a\u0440\u043e\u0432\u0438\u0449\u0430"
description: "Chúng ta được cung cấp một đồ thị vô hướng biểu thị các vị trí được kết nối bằng các đoạn văn. Nhiệm vụ là xác định phần nào của biểu đồ này vẫn có thể sử dụng được nếu chúng ta liên tục đi vào các khu vực nơi chúng ta có thể khám phá đầy đủ và quay lại điểm vào, nhưng chúng ta không thể để bị mắc kẹt trong…"
date: "2026-06-22T19:24:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105545
codeforces_index: "E"
codeforces_contest_name: "\u0423\u0440\u0430\u043b\u044c\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2024"
rating: 0
weight: 105545
solve_time_s: 54
verified: true
draft: false
---

[CF 105545E - \u041f\u043e\u0440\u0442\u0430\u043b\u044c\u043d\u044b\u0435 \u0441\u043e\u043a\u0440\u043e\u0432\u0438\u0449\u0430](https://codeforces.com/problemset/problem/105545/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị vô hướng biểu thị các vị trí được kết nối bằng các đoạn văn. Nhiệm vụ là xác định phần nào của biểu đồ này vẫn có thể sử dụng được nếu chúng ta liên tục đi vào các vùng mà chúng ta có thể khám phá đầy đủ và quay lại điểm vào, nhưng chúng ta không thể mắc kẹt ở một nơi không có đường quay lại phần còn lại của biểu đồ. 

Ý tưởng chính là một số đỉnh thực sự là ngõ cụt: một khi bạn đi vào chúng, bạn không thể đảm bảo đường quay trở lại qua các cạnh chưa được khám phá khác. Đầu ra của bài toán là đồ thị con còn lại sau khi loại bỏ tất cả các đỉnh “không an toàn” như vậy, trong đó mọi đỉnh còn lại là một phần của cấu trúc cho phép vào và ra mà không bị mắc kẹt. 

Kích thước đầu vào ngụ ý một biểu đồ có tối đa khoảng 200.000 đỉnh và cạnh trong trường hợp xấu nhất. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng tất cả các lần duyệt có thể hoặc thực hiện tìm kiếm từ mọi đỉnh một cách độc lập. Bất kỳ giải pháp nào cũng phải chạy trong thời gian tuyến tính hoặc gần tuyến tính theo kích thước của biểu đồ, thường là O(n + m), vì ngay cả O(n²) cũng đã quá chậm. 

Một trường hợp tinh tế phát sinh khi đồ thị trông gần giống một chu trình nhưng có gắn những cây treo lủng lẳng. Xét một chu trình gồm bốn đỉnh với một chuỗi gồm hai đỉnh được gắn vào một nút. Các đỉnh của chuỗi không phải là một phần của bất kỳ chu trình nào, do đó, trực giác ngây thơ có thể giữ chúng một cách không chính xác nếu nó chỉ kiểm tra khả năng tiếp cận từ một phía. Tuy nhiên, những đỉnh đó đều là ngõ cụt vì một khi đã đi vào, chúng buộc bạn phải đi lùi theo cùng một con đường. 

Một trường hợp phức tạp khác là một cạnh duy nhất giữa hai đỉnh. Cả hai đỉnh đều được kết nối về mặt kỹ thuật, nhưng không cho phép hành vi “vào và ra” có ý nghĩa trong một cấu trúc lớn hơn, do đó, tùy theo cách giải thích, chúng sẽ không tồn tại trong quá trình cắt tỉa lặp đi lặp lại trừ khi định nghĩa rõ ràng cho phép cấu trúc giống như chu trình hai nút. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để suy nghĩ về vấn đề này là mô phỏng trực tiếp ý tưởng khám phá. Đối với mỗi đỉnh, chúng tôi cố gắng xem liệu chúng tôi có thể vào đó và vẫn quay lại đỉnh đó trong khi tiếp tục khám phá ở nơi khác hay không. Điều đó gợi ý việc chạy DFS hoặc BFS từ mỗi nút và kiểm tra xem liệu có tồn tại một chu trình hoặc tuyến đường thay thế cho phép quay lại mà không cần truy lại cạnh cuối cùng hay không. 

Điều này ngay lập tức trở nên tốn kém vì mỗi lần kiểm tra như vậy có thể tốn O(n + m) và việc thực hiện nó cho tất cả các đỉnh sẽ dẫn đến O(n(n + m)), vượt xa giới hạn. 

Quan sát cấu trúc quan trọng là khả năng “nhập và quay lại” tương đương với việc thuộc về một thành phần trong đó không có đỉnh nào là điểm cuối bắt buộc. Trong thuật ngữ đồ thị, các đỉnh an toàn là những đỉnh không rời khỏi theo nghĩa của quá trình cắt tỉa lặp đi lặp lại: nếu một đỉnh có bậc 1, thì nó không thể là một phần của bất kỳ cấu trúc giống chu trình nào cho phép xem lại mà không cần quay lại. Khi một đỉnh như vậy bị loại bỏ, đỉnh lân cận của nó cũng có thể trở thành một chiếc lá, truyền hiệu ứng vào bên trong. 

Điều này làm giảm vấn đề liên tục loại bỏ các đỉnh bậc 1 cho đến khi ổn định. Những gì còn lại chính xác là đồ thị con trong đó mọi đỉnh đều có bậc ít nhất là 2, tương ứng với hợp của tất cả các phần chứa chu trình của đồ thị, thường được gọi là lõi 2 của đồ thị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n(n + m)) | O(n + m) | Quá chậm | 
| Tỉa Lá (2 lõi) | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng biểu diễn danh sách kề của biểu đồ và tính bậc của mỗi đỉnh. Điều này là cần thiết vì toàn bộ quá trình chỉ phụ thuộc vào số cạnh mà mỗi đỉnh hiện có. 
2. Khởi tạo một hàng đợi với tất cả các đỉnh có bậc chính xác bằng 1. Đây là những ngõ cụt ngay lập tức vì việc đi vào chúng buộc phải quay lại một đường thoát duy nhất. 
3. Liên tục loại bỏ các đỉnh khỏi hàng đợi. Khi một đỉnh bị loại bỏ, về mặt khái niệm, hãy xóa nó khỏi biểu đồ và giảm bậc của tất cả các đỉnh lân cận của nó. Điều này mô phỏng việc cắt bỏ ngõ cụt và cập nhật những gì sẽ trở thành ngõ cụt mới. 
4. Nếu kết quả là độ của bất kỳ hàng xóm nào giảm xuống 1, hãy thêm nó vào hàng đợi. Đây là bước nhân giống thể hiện việc cắt tỉa một chiếc lá có thể tạo ra một chiếc lá khác như thế nào. 
5. Tiếp tục cho đến khi không còn đỉnh bậc 1 nào trong hàng đợi. Tại thời điểm này, mọi đỉnh còn lại đều có bậc ít nhất là 2 trong đồ thị rút gọn. 
6. In ra tất cả các đỉnh chưa bao giờ bị xóa trong quá trình này. 

Lý do chính khiến việc xóa tham lam này có tác dụng là vì bất kỳ đỉnh nào trở thành lá trong quá trình này đều không thể thuộc về cấu trúc cho phép di chuyển ngang với kết quả được đảm bảo. Việc loại bỏ nó không phá hủy bất kỳ cấu trúc dựa trên chu trình hợp lệ nào, bởi vì các đỉnh trong một chu trình luôn duy trì ít nhất hai cạnh phụ trong chu trình. 

### Tại sao nó hoạt động 

Điều bất biến là ở mỗi bước, tất cả các đỉnh bị loại bỏ đều được đảm bảo không là một phần của bất kỳ sơ đồ con nào trong đó mỗi đỉnh có ít nhất hai đỉnh lân cận trong cấu trúc còn lại. Bất kỳ đỉnh nào bị loại bỏ đều có bậc nhiều nhất là 1 trong đồ thị rút gọn hiện tại, nghĩa là nó không thể nằm trên một chu trình hoặc bất kỳ cấu trúc nào cho phép các đường quay về thay thế. Vì các chu trình không bao giờ mất đi tính chất là mọi đỉnh đều có bậc ít nhất là 2 bên trong chu trình, nên không có đỉnh nghiệm hợp lệ nào bị loại bỏ. Khi quá trình dừng lại, mọi đỉnh còn lại đều có bậc trong ít nhất là 2, do đó không thể cắt tỉa thêm nữa và tập còn lại là cực đại đối với thuộc tính này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m = map(int, input().split())
    g = [[] for _ in range(n)]
    deg = [0] * n

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)
        deg[u] += 1
        deg[v] += 1

    q = deque()
    removed = [False] * n

    for i in range(n):
        if deg[i] == 1:
            q.append(i)
            removed[i] = True

    while q:
        u = q.popleft()
        for v in g[u]:
            if removed[v]:
                continue
            deg[v] -= 1
            if deg[v] == 1:
                removed[v] = True
                q.append(v)

    res = [str(i + 1) for i in range(n) if not removed[i]]
    print(len(res))
    if res:
        print(" ".join(res))

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì các danh sách kề và một mảng độ sao cho mỗi cạnh chỉ được xử lý khi một trong các điểm cuối của nó bị loại bỏ. Hàng đợi đảm bảo rằng các đỉnh được xử lý theo thứ tự bong tróc tự nhiên, luôn xử lý các lá mới hình thành ngay lập tức. 

Một chi tiết tinh tế là đánh dấu các đỉnh bị loại bỏ khi chúng vào hàng đợi chứ không phải khi chúng được xử lý. Điều này ngăn chặn nhiều hoạt động xếp hàng cho cùng một đỉnh khi bậc của nó giảm qua các đỉnh khác nhau. 

## Ví dụ đã hoạt động 

Xét một đồ thị có hình dạng như một chu trình gồm bốn đỉnh có một đuôi gắn với một nút. Đuôi bao gồm hai đỉnh. 

Mức độ ban đầu được hiển thị dưới đây. 

| Bước | Đã xóa hàng đợi | Độ (A,B,C,D,E,F) | 
| --- | --- | --- | 
| Bắt đầu | [E, F] | A=3, B=2, C=2, D=2, E=1, F=1 | 
| Xóa E | [F] | A=2, B=2, C=2, D=2, F=1 | 
| Xóa F | [] | A=2, B=2, C=2, D=2 | 

Sau khi cắt tỉa, chỉ còn lại chu kỳ. Điều này chứng tỏ rằng các đỉnh của chuỗi bị loại bỏ trước tiên và việc loại bỏ chúng không ảnh hưởng đến cấu trúc chu trình cốt lõi. 

Bây giờ hãy xem xét một biểu đồ đường đơn giản gồm năm đỉnh. 

| Bước | Đã xóa hàng đợi | Độ (1-5) | 
| --- | --- | --- | 
| Bắt đầu | [1,5] | 1=1, 2=2, 3=2, 4=2, 5=1 | 
| Xóa 1 | [5] | 2=1, 3=2, 4=2, 5=1 | 
| Xóa 5 | [2] | 2=1, 3=2, 4=2 | 
| Xóa 2 | [] | 3=2, 4=2 | 
| Loại bỏ 3,4 ngầm ổn định | [] | lõi trống | 

Điều này cho thấy rằng trong một cái cây, mọi thứ cuối cùng đều biến mất vì mọi đỉnh cuối cùng đều lộ ra dưới dạng một chiếc lá. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi đỉnh được xếp vào hàng đợi nhiều nhất một lần và mỗi cạnh được xử lý nhiều nhất hai lần trong quá trình cập nhật độ | 
| Không gian | O(n + m) | Danh sách kề cộng với các mảng phụ trợ để theo dõi mức độ và loại bỏ | 

Độ phức tạp tuyến tính phù hợp với các ràng buộc đối với đồ thị lên tới hàng trăm nghìn nút và cạnh, đảm bảo giải pháp chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n, m = map(int, input().split())
    g = [[] for _ in range(n)]
    deg = [0] * n

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)
        deg[u] += 1
        deg[v] += 1

    q = deque()
    removed = [False] * n

    for i in range(n):
        if deg[i] == 1:
            q.append(i)
            removed[i] = True

    while q:
        u = q.popleft()
        for v in g[u]:
            if not removed[v]:
                deg[v] -= 1
                if deg[v] == 1:
                    removed[v] = True
                    q.append(v)

    res = [str(i + 1) for i in range(n) if not removed[i]]
    return str(len(res)) + ("\n" + " ".join(res) if res else "")

# custom cases
assert run("1 0\n") == "1\n1", "single node"
assert run("2 1\n1 2\n") == "0", "single edge disappears"
assert run("4 4\n1 2\n2 3\n3 4\n4 1\n") == "4\n1 2 3 4", "cycle survives"
assert run("5 4\n1 2\n2 3\n3 4\n4 5\n") == "0", "path disappears"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 nút | trường hợp cơ bản tầm thường | 
| cạnh đơn | trống | loại bỏ lá lẫn nhau | 
| 4 chu kỳ | tất cả các nút | bảo quản chu kỳ | 
| 5 đường | trống | cắt tỉa đầy đủ | 

## Vỏ cạnh 

Một cạnh duy nhất giữa hai đỉnh sẽ kích hoạt việc loại bỏ ngay lập tức cả hai điểm cuối. Hàng đợi ban đầu chứa cả hai đỉnh vì mỗi đỉnh có độ 1. Việc loại bỏ đỉnh đầu tiên sẽ làm giảm độ của hàng xóm xuống 0 hoặc 1 tùy thuộc vào cách biểu diễn và nó cũng bị xóa. Đầu ra cuối cùng trống, phù hợp với ý tưởng rằng không có đỉnh nào có thể hỗ trợ quá trình truyền tải có thể trả về. 

Một chu kỳ thuần túy cho thấy sự ổn định ngay từ đầu. Mọi đỉnh đều có bậc 2, do đó hàng đợi ban đầu trống và không có sự loại bỏ nào xảy ra. Thuật toán ngay lập tức trả về tất cả các đỉnh, chứng tỏ rằng các chu trình là các điểm cố định của quá trình cắt tỉa. 

Một cái cây chứng tỏ sự sụp đổ hoàn toàn. Mọi lá đều bị loại bỏ trước tiên, lá này lan truyền vào trong cho đến khi không còn đỉnh nào. Mỗi bước làm giảm kích thước cây mà không bao giờ tạo ra lõi ổn định, xác nhận rằng cây không chứa cấu trúc có thể sống sót theo chu kỳ.
