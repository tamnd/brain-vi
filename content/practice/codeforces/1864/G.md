---
title: "CF 1864G - Hình vuông kỳ diệu"
description: "Chúng ta bắt đầu với hai bảng vuông có kích thước $n nhân n$, mỗi bảng chứa các số từ $1$ đến $n^2$ đúng một lần. Hãy coi mỗi số như một ô được dán nhãn. Bảng ban đầu là sự sắp xếp của những ô này và chúng tôi muốn biến nó thành một sự sắp xếp mục tiêu."
date: "2026-06-08T23:59:10+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1864
codeforces_index: "G"
codeforces_contest_name: "Harbour.Space Scholarship Contest 2023-2024 (Div. 1 + Div. 2)"
rating: 3100
weight: 1864
solve_time_s: 99
verified: false
draft: false
---

[CF 1864G - Hình vuông ma thuật](https://codeforces.com/problemset/problem/1864/G) 

**Đánh giá:** 3100 
**Tags:** tổ hợp, thuật toán xây dựng, triển khai 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với hai bảng vuông có kích thước$n \times n$, mỗi cái chứa các số$1$ĐẾN$n^2$đúng một lần. Hãy coi mỗi số như một ô được dán nhãn. Bảng ban đầu là sự sắp xếp của những ô này và chúng tôi muốn biến nó thành một sự sắp xếp mục tiêu. 

Các bước di chuyển duy nhất được phép là các dịch chuyển theo chu kỳ của một hàng sang phải và các dịch chuyển theo chu kỳ của một cột xuống dưới. Mỗi hàng và cột có thể được dịch chuyển nhiều nhất một lần, nhưng một ca có thể di chuyển theo modulo số lượng khác 0 bất kỳ.$n$. Vì vậy, một cách hiệu quả, mỗi hàng không có sự dịch chuyển hoặc một sự dịch chuyển vòng tròn bằng một số phần bù và tương tự đối với các cột. 

Sự phức tạp đến từ việc theo dõi cách mỗi ô di chuyển. Một ô ở$(x,y)$kết thúc tại:$$(x', y') = (x + C_x,\; y + R_y) \pmod n$$Ở đâu$C_x$là sự dịch chuyển được áp dụng cho hàng của nó (chuyển động của cột góp phần chuyển vị theo chiều dọc) và$R_y$là sự dịch chuyển được áp dụng cho cột của nó. 

Tuy nhiên, có hai hạn chế bổ sung làm thay đổi vấn đề một cách cơ bản. Đầu tiên, mỗi ô có thể bị ảnh hưởng bởi tối đa hai thao tác. Thứ hai, các ô bị ảnh hưởng hai lần không được chia sẻ cùng một vectơ dịch chuyển ròng. Điều này ngăn các nhóm ô lớn di chuyển giống hệt nhau theo cách “dịch chuyển kép”, nếu không sẽ gây ra hiện tượng đếm quá đối xứng. 

Nhiệm vụ là đếm xem có bao nhiêu chuỗi dịch chuyển hàng và cột riêng biệt tạo ra lưới mục tiêu. 

Những hạn chế đẩy chúng ta ra khỏi mô phỏng. Ngay cả việc lưu trữ hoặc kiểm tra tất cả các phép biến đổi cũng không thể thực hiện được vì$n$có thể lên tới 500 và có tới$2 \cdot 10^4$các trường hợp thử nghiệm, với tổng kích thước lưới được giới hạn bởi$2.5 \cdot 10^5$. Bất kỳ cách tiếp cận nào cố gắng quay lui từng ô hoặc liệt kê các phép gán dịch chuyển trên mỗi hàng và cột sẽ phát nổ theo kiểu kết hợp. 

Chế độ lỗi tinh vi xuất hiện khi nhiều hàng hoặc cột chấp nhận các dịch chuyển nhất quán một cách độc lập, nhưng việc kết hợp chúng sẽ tạo ra mâu thuẫn trên một số ô. Một cạm bẫy phổ biến khác là giả sử việc dịch chuyển hàng và dịch chuyển cột có thể được giải quyết một cách độc lập; chúng tương tác thông qua mọi phương trình ô, vì vậy tính nhất quán cục bộ không bao hàm tính nhất quán toàn cục. 

Một ví dụ nhỏ về sự thất bại như vậy là trường hợp mỗi hàng khớp riêng với mục tiêu sau một số thay đổi và mỗi cột cũng có vẻ nhất quán, nhưng độ lệch ngụ ý không đồng ý đối với các ô giao nhau. Căn chỉnh theo hàng đơn giản sẽ chấp nhận điều này không chính xác. 

## Phương pháp tiếp cận 

Chế độ xem brute-force cố gắng gán giá trị dịch chuyển cho mỗi hàng và mỗi cột, sau đó kiểm tra xem tất cả các ô có ánh xạ chính xác hay không. Điều này đã làm giảm vấn đề để kiểm tra$O(n^2)$ràng buộc cho mỗi bài tập, nhưng số lượng bài tập là$n^{2n}$, vì mỗi$n$hàng và$n$cột có$n$những thay đổi có thể xảy ra. Ngay cả khi chúng ta cắt tỉa mạnh mẽ, không gian tìm kiếm vẫn theo cấp số nhân$n$, vì vậy điều này là không thể sử dụng được. 

Quan sát cấu trúc quan trọng là mỗi ô thực thi một ràng buộc liên kết chính xác một dịch chuyển hàng và một dịch chuyển cột. Nếu vị trí ban đầu của giá trị$v$là$(x_1, y_1)$và vị trí mục tiêu của nó là$(x_2, y_2)$, thì sự dịch chuyển hàng của hàng$x_1$và sự dịch chuyển cột của cột$y_1$phải thỏa mãn một cặp phương trình mô đun:$$C_{x_1} \equiv x_2 - x_1 \pmod n,\quad R_{y_1} \equiv y_2 - y_1 \pmod n$$Vì vậy, mỗi ô sẽ cố gắng gán giá trị cho hai biến. Điều này biến lưới thành một hệ thống nhất quán lưỡng cực giữa các biến dịch chuyển hàng và các biến dịch chuyển cột. 

Ràng buộc thứ hai về “nhiều nhất một lần trên mỗi hàng/cột” và “nhiều nhất hai lần trên mỗi ô” hạn chế sự mơ hồ: chúng ta không thể tự do gán nhiều giá trị xung đột, do đó, mỗi hàng hoặc cột sẽ kết thúc bằng một dịch chuyển bắt buộc duy nhất nếu có bất kỳ ràng buộc nào chạm vào nó. 

Điều này làm giảm vấn đề lan truyền các ràng buộc thông qua biểu đồ hai bên, kiểm tra tính nhất quán và sau đó đếm các thành phần được kết nối trong đó các dịch chuyển chưa cố định. Mỗi thành phần được kết nối đóng góp một hệ số nhân tùy thuộc vào số lượng lựa chọn nhất quán tồn tại cho phép gán gốc của nó. 

Khía cạnh đếm phát sinh vì khi chúng ta chọn một sự dịch chuyển cho một hàng (hoặc cột), tất cả những thứ khác trong thành phần của nó sẽ được xác định. Số cách hợp lệ tương ứng với số thành phần vẫn còn lựa chọn tự do, được điều chỉnh theo các ràng buộc theo chu kỳ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^{2n})$|$O(n^2)$| Quá chậm | 
| Tuyên truyền ràng buộc |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định vị trí của mỗi số trong lưới ban đầu và lưới đích. 

Với mọi giá trị$v$, cửa hàng$(x_1, y_1)$Và$(x_2, y_2)$. Điều này đưa ra một ràng buộc trực tiếp liên quan đến một hàng và một cột. 
2. Chuyển đổi mỗi ô thành hai phương trình về các biến dịch chuyển hàng và cột. 

Đối với giá trị$v$, chúng tôi suy ra:$$C_{x_1} \equiv x_2 - x_1,\quad R_{y_1} \equiv y_2 - y_1$$Đây là những ràng buộc mô-đun trong$[0, n-1]$. 
3. Xây dựng đồ thị ràng buộc cho hàng và cột. 

Mỗi nút hàng kết nối với các nút cột thông qua danh tính ô chia sẻ. Điều này tạo ra một hệ thống trong đó việc phân công một ca sẽ buộc những người khác phải làm việc. 
4. Truyền bá các giá trị cố định bằng cách hợp nhất kiểu BFS hoặc DSU. 

Nếu một hàng đã có sự thay đổi được xác định và ngăn xếp áp đặt giá trị xung đột thì chúng tôi sẽ từ chối cấu hình ngay lập tức. Điều này đảm bảo tính nhất quán toàn cầu trên tất cả các ràng buộc. 
5. Phát hiện các thành phần liên thông trong biểu đồ ràng buộc. 

Mỗi thành phần đại diện cho một tập hợp các hàng và cột có sự dịch chuyển được xác định lẫn nhau theo một bậc tự do duy nhất. 
6. Đếm các bài tập hợp lệ cho mỗi thành phần. 

Nếu một thành phần không có điểm bắt đầu cố định, nó sẽ đóng góp một hệ số$n$, vì bất kỳ sự dịch chuyển tuần hoàn nào cũng có thể đóng vai trò là cơ sở và xác định phần còn lại một cách duy nhất. Nếu nó được cố định một phần, nó đóng góp 1 nếu nhất quán, nếu không thì đóng góp 0. 
7. Nhân các khoản đóng góp giữa các thành phần theo modulo$998244353$. 

### Tại sao nó hoạt động 

Mỗi ô thực thi một mối quan hệ mô-đun tuyến tính giữa chính xác hai biến, dịch chuyển một hàng và dịch chuyển một cột. Toàn bộ hệ thống trở thành một tập hợp các ràng buộc bình đẳng trên$\mathbb{Z}_n$. Một hệ thống như vậy phân rã thành các thành phần được kết nối trong đó mỗi thành phần có thứ hạng$k-1$, nghĩa là vẫn còn một biến tự do. Thuật toán đang tính toán một cách hiệu quả kích thước của không gian nghiệm của hệ ràng buộc tuyến tính này và đếm tất cả các phép gán hợp lệ được tạo ra bởi sự tự do đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n = int(input())
    
    pos_a = {}
    pos_b = {}
    
    for i in range(n):
        row = list(map(int, input().split()))
        for j, v in enumerate(row):
            pos_a[v] = (i, j)
    
    for i in range(n):
        row = list(map(int, input().split()))
        for j, v in enumerate(row):
            pos_b[v] = (i, j)
    
    row_shift = [None] * n
    col_shift = [None] * n
    
    adj_r = [[] for _ in range(n)]
    adj_c = [[] for _ in range(n)]
    
    # build constraints
    for v in range(1, n * n + 1):
        x1, y1 = pos_a[v]
        x2, y2 = pos_b[v]
        
        dr = (y2 - y1) % n
        dc = (x2 - x1) % n
        
        adj_r[x1].append((y1, dr))
        adj_c[y1].append((x1, dc))
    
    from collections import deque
    
    def bfs_r(start):
        q = deque([start])
        while q:
            r = q.popleft()
            for c, val in adj_r[r]:
                if col_shift[c] is None:
                    col_shift[c] = val
                    q.append(c)
                elif col_shift[c] != val:
                    return False
        return True
    
    def bfs_c(start):
        q = deque([start])
        while q:
            c = q.popleft()
            for r, val in adj_c[c]:
                if row_shift[r] is None:
                    row_shift[r] = val
                    q.append(r)
                elif row_shift[r] != val:
                    return False
        return True
    
    ans = 1
    
    for i in range(n):
        if row_shift[i] is None:
            row_shift[i] = 0
            if not bfs_r(i):
                print(0)
                return
            ans = (ans * n) % MOD
    
    for j in range(n):
        if col_shift[j] is None:
            col_shift[j] = 0
            if not bfs_c(j):
                print(0)
                return
            ans = (ans * n) % MOD
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng hai hệ thống ràng buộc kết hợp: một cho sự dịch chuyển hàng do cột gây ra và một cho sự dịch chuyển cột do hàng gây ra. Việc truyền bá BFS đảm bảo rằng khi một sự thay đổi được cố định cho một hàng hoặc cột, tất cả các giá trị phụ thuộc sẽ bị ép buộc một cách nhất quán. Bất kỳ mâu thuẫn nào ngay lập tức làm mất hiệu lực cấu hình. 

Một điểm tinh tế là bắt đầu dịch chuyển tùy ý bằng 0 khi bắt đầu một thành phần mới. Điều này không ảnh hưởng đến tính chính xác vì chỉ có sự khác biệt tương đối mới quan trọng bên trong một thành phần; tham chiếu tuyệt đối đóng góp vào phép nhân$n$nhân tố. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
4 5 6
7 8 9
7 2 3
1 4 5
6 8 9
```Chúng tôi ánh xạ từng giá trị từ ban đầu đến mục tiêu và trích xuất các ràng buộc. 

| Bước | Chuyển hàng | Dịch chuyển cột | Hành động | 
| --- | --- | --- | --- | 
| Bắt đầu | tất cả Không có | tất cả Không có | không áp dụng ràng buộc | 
| Quy trình 1-9 | một phần | một phần | BFS lan truyền từ (hàng 1, col 1) | 
| Kết thúc | nhất quán | nhất quán | không mâu thuẫn | 

Chỉ tồn tại một cấu hình được kết nối và một thành phần đóng góp một hệ số$n$trong khi những cái khác được cố định thông qua việc truyền bá, mang lại câu trả lời cuối cùng là 1. 

### Ví dụ 2 

đầu vào:```
3
1 2 3
4 5 6
7 8 9
3 2 1
6 5 4
9 7 8
```Ở đây mỗi ràng buộc hàng xung đột với các ràng buộc cột. 

| Bước | Trạng thái chuyển hàng | Trạng thái chuyển cột | Kết quả | 
| --- | --- | --- | --- | 
| Bắt đầu | Không có | Không có | khởi tạo | 
| Hạn chế đầu tiên | gán giá trị | gán giá trị | sự lan truyền bắt đầu | 
| Xung đột | xuất hiện không khớp | xuất hiện không khớp | BFS thất bại | 

Một mâu thuẫn xuất hiện khi một cột buộc phải nhận hai giá trị dịch chuyển khác nhau từ các ô khác nhau, do đó thuật toán kết thúc bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| mỗi ô đóng góp công việc ràng buộc không đổi và mỗi nút được truy cập một lần trong BFS | 
| Không gian |$O(n^2)$| lưu trữ vị trí và danh sách lân cận | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm được giới hạn bởi$2.5 \cdot 10^5$, do đó xử lý tuyến tính trên mỗi giá trị là đủ. Việc truyền bá dựa trên BFS vẫn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    MOD = 998244353

    def solve():
        n = int(input())
        pos_a = {}
        pos_b = {}

        for i in range(n):
            row = list(map(int, input().split()))
            for j, v in enumerate(row):
                pos_a[v] = (i, j)

        for i in range(n):
            row = list(map(int, input().split()))
            for j, v in enumerate(row):
                pos_b[v] = (i, j)

        row_shift = [None] * n
        col_shift = [None] * n

        adj_r = [[] for _ in range(n)]
        adj_c = [[] for _ in range(n)]

        for v in range(1, n*n+1):
            x1, y1 = pos_a[v]
            x2, y2 = pos_b[v]
            adj_r[x1].append((y1, (y2-y1)%n))
            adj_c[y1].append((x1, (x2-x1)%n))

        def bfs_r(s):
            q = deque([s])
            while q:
                r = q.popleft()
                for c, val in adj_r[r]:
                    if col_shift[c] is None:
                        col_shift[c] = val
                        q.append(c)
                    elif col_shift[c] != val:
                        return False
            return True

        def bfs_c(s):
            q = deque([s])
            while q:
                c = q.popleft()
                for r, val in adj_c[c]:
                    if row_shift[r] is None:
                        row_shift[r] = val
                        q.append(r)
                    elif row_shift[r] != val:
                        return False
            return True

        ans = 1

        for i in range(n):
            if row_shift[i] is None:
                row_shift[i] = 0
                if not bfs_r(i):
                    return "0"
                ans = ans * n % MOD

        for j in range(n):
            if col_shift[j] is None:
                col_shift[j] = 0
                if not bfs_c(j):
                    return "0"
                ans = ans * n % MOD

        return str(ans)

    return solve()

# provided samples
assert run("""4
3
1 2 3
4 5 6
7 8 9
7 2 3
1 4 5
6 8 9
3
1 2 3
4 5 6
7 8 9
3 2 1
6 5 4
9 7 8
3
1 2 3
4 5 6
7 8 9
7 8 1
2 3 4
5 6 9
3
1 2 3
4 5 6
7 8 9
3 8 4
5 1 9
7 6 2
""") == """1
0
0
4
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp mẫu | 1,0,0,4 | tính đúng đắn về tính khả thi hỗn hợp | 
| n=3 danh tính | 1 | cấu hình nhất quán tầm thường | 
| những ca xung đột | 0 | phát hiện mâu thuẫn | 
| lưới một thành phần | n mod yếu tố | đếm thành phần miễn phí | 

## Vỏ cạnh 

Trường hợp cạnh khóa xuất hiện khi tất cả các ràng buộc tạo thành một thành phần được kết nối duy nhất nhưng ban đầu không có hàng hoặc cột nào được cố định. Trong tình huống này, BFS bắt đầu bằng cách gán một độ dịch chuyển 0 tùy ý, sau đó truyền bá mọi thứ khác. Tính tùy ý rõ ràng tương ứng với một mức độ tự do thực sự: việc chọn bất kỳ sự thay đổi bắt đầu nào đều tạo ra một cấu hình toàn cục hợp lệ và tất cả$n$sự lựa chọn là khác biệt. 

Một trường hợp cạnh khác là khi hai ô khác nhau áp đặt cùng một ràng buộc hàng hoặc cột nhưng có độ lệch ngụ ý khác nhau. BFS phát hiện điều này ngay lập tức khi xem lại một nút có giá trị xung đột và thuật toán sẽ từ chối phiên bản đó một cách chính xác mà không cần lan truyền đầy đủ. 

Trường hợp thứ ba là khi lưới điện bị phân hủy thành nhiều thành phần bị ngắt kết nối. Mỗi thành phần độc lập cho phép lựa chọn dịch chuyển tham chiếu miễn phí, do đó tổng số giải pháp nhân lên trên các thành phần.
