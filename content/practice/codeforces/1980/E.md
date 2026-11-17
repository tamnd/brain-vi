---
title: "CF 1980E - Hoán vị hàng và cột"
description: "Chúng ta có hai lưới số hình chữ nhật, cả hai đều có kích thước $n nhân m$, và chúng cùng chứa chính xác các số từ $1$ đến $n cdot m$, mỗi số xuất hiện một lần. Vì vậy, mỗi ma trận chỉ là sự sắp xếp lại của cùng một bộ ô."
date: "2026-06-08T16:53:12+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "greedy", "hashing", "implementation", "math", "matrices", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1980
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 950 (Div. 3)"
rating: 1600
weight: 1980
solve_time_s: 80
verified: true
draft: false
---

[CF 1980E - Hoán vị hàng và cột](https://codeforces.com/problemset/problem/1980/E) 

**Đánh giá:** 1600 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, tham lam, băm, triển khai, toán học, ma trận, sắp xếp 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai lưới số hình chữ nhật, cả hai đều có kích thước$n \times m$và cùng nhau chúng chứa chính xác các số từ$1$ĐẾN$n \cdot m$, mỗi cái xuất hiện một lần. Vì vậy, mỗi ma trận chỉ là sự sắp xếp lại của cùng một bộ ô. 

Các bước di chuyển duy nhất được phép là hoán đổi toàn bộ hàng hoặc hoán đổi toàn bộ cột. Chúng ta có thể làm điều này bao nhiêu lần cũng được. Nhiệm vụ là xác định xem liệu chúng ta có thể chuyển đổi lưới đầu tiên thành lưới thứ hai chỉ bằng cách hoán đổi hàng và cột này hay không. 

Ràng buộc chính là việc hoán đổi hàng không thay đổi thứ tự bên trong một hàng, chúng chỉ sắp xếp lại các hàng và hoán đổi cột không thay đổi thứ tự bên trong một cột mà chỉ sắp xếp lại các cột. Vì vậy, cấu trúc của lưới được bảo toàn cho đến các hoán vị độc lập của chỉ số hàng và chỉ số cột. 

Kích thước đầu vào lớn: trong tất cả các trường hợp thử nghiệm, tổng số ô nhiều nhất là$2 \cdot 10^5$. Điều này gợi ý mạnh mẽ một$O(nm)$cho mỗi trường hợp thử nghiệm hoặc giải pháp tổng thể tuyến tính. Bất cứ điều gì liên quan đến việc sắp xếp lặp lại cho mỗi lần kiểm tra hoặc mô phỏng các hoạt động chỉ tốt nếu nó có tổng kích thước đầu vào tuyến tính. 

Một sự hiểu lầm ngây thơ sẽ là nghĩ rằng chúng ta cần mô phỏng các giao dịch hoán đổi hoặc cố gắng “căn chỉnh” các hàng và cột một cách tham lam. Điều đó sẽ thất bại trong những trường hợp như: 

Ví dụ:$$a = \begin{pmatrix} 1 & 2 \\ 3 & 4 \end{pmatrix}, \quad b = \begin{pmatrix} 4 & 3 \\ 2 & 1 \end{pmatrix}$$Cách tiếp cận so khớp hàng đơn giản có thể kết luận là không khớp vì không có hàng nào khớp trực tiếp, nhưng việc hoán đổi cả hàng và cột sẽ giúp chuyển đổi có thể thực hiện được. 

Một trường hợp thất bại tinh vi khác là giả sử các tập hợp nhiều hàng phải khớp với từng hàng. Điều đó sai vì bản thân các hàng được hoán vị một cách tùy ý. 

Khó khăn thực sự là cả hoán vị hàng và cột đều tương tác trên toàn cầu chứ không phải cục bộ. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là nghĩ đến việc áp dụng hoán đổi hàng và cột như tạo ra tất cả các hoán vị của hàng và cột. Điều đó có nghĩa là bất kỳ phép biến đổi nào cũng tương ứng với việc chọn hoán vị các hàng và hoán vị các cột. Về nguyên tắc, chúng ta có thể thử tất cả các hoán vị hàng và hoán vị cột và kiểm tra xem ma trận kết quả có khớp hay không. Điều đó sẽ liên quan đến$n!\cdot m!$những khả năng hoàn toàn không thể thực hiện được ngay cả đối với$n=5$. 

Thông tin chi tiết quan trọng là việc hoán đổi hàng và cột hoạt động độc lập trên các chỉ mục. Nếu chúng ta gán vị trí của mỗi giá trị trong cả hai ma trận, chúng ta có thể nghĩ về tọa độ. Mỗi số$x$trong ma trận$a$có tọa độ$(r_a[x], c_a[x])$, và trong ma trận$b$, tọa độ$(r_b[x], c_b[x])$. 

Nếu một phép biến đổi hợp lệ tồn tại thì phải tồn tại một hoán vị của các hàng và một hoán vị của các cột sao cho với mọi giá trị$x$, chỉ mục hàng của nó ánh xạ nhất quán và chỉ mục cột của nó ánh xạ nhất quán. Nói cách khác, phép biến đổi gây ra hai song ánh: 

một trên chỉ mục hàng và một trên chỉ mục cột và chúng phải giải thích đồng thời tất cả các vị trí. 

Điều này làm giảm vấn đề kiểm tra tính nhất quán của các ánh xạ cảm ứng này. Một cách rõ ràng để thực thi điều này là neo các mối quan hệ hàng bằng cách sử dụng vị trí của các giá trị: các giá trị xuất hiện trong cùng một hàng trong$a$phải ánh xạ tới các giá trị xuất hiện trong cùng một hàng trong$b$, sau khi lập chỉ mục lại các hàng một cách nhất quán. Điều tương tự cũng áp dụng cho các cột. 

Một cách cải cách đơn giản và tiêu chuẩn hơn là coi mỗi giá trị là một cặp$(row, col)$, sau đó quan sát rằng thứ tự tương đối của các hàng giữa hai giá trị bất kỳ phải được giữ nguyên giữa$a$Và$b$và tương tự cho cột. Điều này tạo ra một biểu đồ ràng buộc trong đó sự không nhất quán ngay lập tức bộc lộ sự không thể thực hiện được. 

Một đặc tính thân thiện với việc triển khai hơn là chuẩn hóa cả hai ma trận bằng cách thay thế các giá trị bằng vị trí của chúng và kiểm tra xem thứ tự hàng và thứ tự cột cảm ứng của tất cả các giá trị có giống hệt với hoán vị hay không. Việc sắp xếp các giá trị theo vị trí của chúng trong một ma trận và xác minh tính nhất quán trong ma trận kia sẽ dẫn đến kiểm tra tuyến tính hoặc gần tuyến tính bằng cách sử dụng hàm băm hoặc so sánh chữ ký trên mỗi hàng và cột. 

Giải pháp thiết thực nhất là sử dụng mã hóa vị trí: đối với mỗi giá trị, ghi lại hàng và cột của nó trong cả hai ma trận, sau đó xác minh rằng thứ tự tương đối của các hàng và cột trong số tất cả các giá trị là giống hệt nhau trong cả hai cách biểu diễn. Điều này có thể được giảm xuống để kiểm tra xem ánh xạ từ các hàng trong$a$đến các hàng trong$b$nhất quán và tương tự cho các cột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị bạo lực của hàng/cols |$O(n! \cdot m!)$|$O(nm)$| Quá chậm | 
| Ánh xạ nhất quán vị trí |$O(nm)$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng ánh xạ trực tiếp giữa các vị trí của các giá trị trong cả hai ma trận và xác minh rằng các hoán vị hàng và cột được ngụ ý bởi các ánh xạ này là nhất quán. 

1. Đọc cả hai ma trận và lưu trữ mọi giá trị$x$, tọa độ của nó trong$a$:$(r_a[x], c_a[x])$và trong$b$:$(r_b[x], c_b[x])$. 

Bước này chuyển đổi bài toán lưới thành bài toán so khớp điểm trên các hoán vị. 
2. Với mỗi giá trị$x$, chúng tôi diễn giải phép biến đổi dưới dạng hàng ánh xạ$r_a[x]$ĐẾN$r_b[x]$và cột$c_a[x]$ĐẾN$c_b[x]$. 

Nếu tồn tại một hoán vị hàng toàn cục hợp lệ thì tất cả các giá trị chia sẻ một hàng trong$a$phải ánh xạ nhất quán tới cùng một cấu trúc hàng trong$b$. 
3. Chúng tôi thực thi tính nhất quán của ánh xạ hàng bằng cách chọn một giá trị đại diện cho mỗi hàng trong$a$và đảm bảo tất cả các giá trị trong hàng đó ánh xạ tới cùng một hoán vị hàng trong$b$. 

Nếu bất kỳ hàng nào trong$a$cố gắng ánh xạ tới nhiều hàng trong$b$, sự biến đổi là không thể. 
4. Lặp lại logic tương tự cho các cột, đảm bảo mỗi cột nằm trong$a$liên tục ánh xạ tới một cột duy nhất trong$b$. 
5. Sau khi thiết lập ánh xạ, chúng tôi xác minh tính nội tại: không có hai hàng riêng biệt trong$a$ánh xạ tới cùng một hàng trong$b$và tương tự cho cột. 

Điều này đảm bảo ánh xạ là một hoán vị thực sự chứ không phải là sự thu gọn. 
6. Nếu cả hai ánh xạ hàng và cột đều là hoán vị hợp lệ, thì xuất CÓ; nếu không thì xuất ra NO. 

### Tại sao nó hoạt động 

Hoán đổi hàng chỉ hoán vị các chỉ mục hàng trên toàn cầu, nghĩa là danh tính của các giá trị chia sẻ một hàng là bất biến trong bất kỳ chuỗi thao tác nào. Điều tương tự cũng xảy ra với các cột. Vì vậy, việc phân chia các giá trị do hàng (và theo cột) tạo ra phải được giữ nguyên giữa$a$Và$b$, cho đến đổi tên chỉ mục hàng và chỉ mục cột. 

Nếu hai giá trị ở cùng một hàng trong$a$, chúng phải ở cùng một hàng trong$b$sau khi áp dụng hoán vị hàng. Điều này buộc một ánh xạ được xác định rõ ràng từ các hàng$a$đến hàng$b$. Lập luận tương tự được áp dụng ngược lại, đảm bảo tính khách quan. 

Do đó, thuật toán đang kiểm tra xem liệu phân vùng hàng và phân vùng cột được tạo ra bởi sự hoán vị của các giá trị có thể được gắn nhãn lại một cách nhất quán hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())

        pos_a = {}
        pos_b = {}

        for i in range(n):
            row = list(map(int, input().split()))
            for j, x in enumerate(row):
                pos_a[x] = (i, j)

        for i in range(n):
            row = list(map(int, input().split()))
            for j, x in enumerate(row):
                pos_b[x] = (i, j)

        row_map = {}
        col_map = {}

        ok = True

        for x in pos_a:
            ra, ca = pos_a[x]
            rb, cb = pos_b[x]

            if ra in row_map and row_map[ra] != rb:
                ok = False
                break
            if ca in col_map and col_map[ca] != cb:
                ok = False
                break

            row_map[ra] = rb
            col_map[ca] = cb

        if ok:
            if len(set(row_map.values())) != len(row_map):
                ok = False
            if len(set(col_map.values())) != len(col_map):
                ok = False

        out.append("YES" if ok else "NO")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên nén cả hai ma trận vào bảng tra cứu vị trí để mỗi giá trị hiển thị trực tiếp tọa độ của nó. Sau đó, nó xây dựng hai ánh xạ một phần: một cho hàng và một cho cột. Việc kiểm tra tính nhất quán đảm bảo rằng một hàng trong ma trận đầu tiên không bao giờ cố gắng ánh xạ tới hai hàng khác nhau trong ma trận thứ hai, điều này sẽ mâu thuẫn với sự tồn tại của hoán vị hàng chung. 

Kiểm tra tính tiêm nhiễm cuối cùng đảm bảo chúng tôi thực sự có các hoán vị chứ không phải ánh xạ nhiều-một, điều này sẽ vi phạm khả năng đảo ngược của việc hoán đổi hàng và cột. 

Một cạm bẫy phổ biến là quên rằng tính nhất quán phải được thực thi trên toàn cầu trên tất cả các giá trị chứ không phải riêng lẻ từng hàng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
2 2
1 2
3 4
4 3
2 1
```| giá trị | vị trí trong | vị trí trong b | bản đồ hàng | bản đồ col | 
| --- | --- | --- | --- | --- | 
| 1 | (0,0) | (1,1) | 0→1 | 0→1 | 
| 2 | (0,1) | (1,0) | 0→1 | 1→0 | 
| 3 | (1,0) | (0,1) | 1→0 | 0→1 | 
| 4 | (1,1) | (0,0) | 1→0 | 1→0 | 

Tất cả các ánh xạ đều nhất quán và khách quan. 

Điều này cho thấy rằng ngay cả khi cả hàng và cột được đảo ngược hoàn toàn, ánh xạ vẫn hợp lệ miễn là tính nhất quán được duy trì. 

### Ví dụ 2 

đầu vào:```
1
2 2
1 2
3 4
4 1
2 3
```| giá trị | vị trí trong | vị trí trong b | bản đồ hàng | bản đồ col | 
| --- | --- | --- | --- | --- | 
| 1 | (0,0) | (1,1) | 0→1 | 0→1 | 
| 2 | (0,1) | (0,1) | 0→0 | 1→1 | 
| 3 | (1,0) | (1,0) | 1→1 | 0→0 | 
| 4 | (1,1) | (0,0) | 1→0 | 1→0 | 

Ánh xạ hàng trở nên không nhất quán: hàng 0 ánh xạ tới cả 1 và 0. Điều này vi phạm yêu cầu hoán vị hàng toàn cầu duy nhất. 

Điều này chứng tỏ một xung đột trong ánh xạ cảm ứng sẽ vô hiệu hóa phép biến đổi như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Mỗi giá trị được xử lý một lần để xây dựng vị trí và xác minh ánh xạ | 
| Không gian |$O(nm)$| Lưu trữ bản đồ vị trí cho tất cả các giá trị | 

Giải pháp chia tỷ lệ tuyến tính với tổng số phần tử trong tất cả các trường hợp thử nghiệm, phù hợp thoải mái với ràng buộc của$2 \cdot 10^5$tổng số tế bào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, m = map(int, input().split())
            pa, pb = {}, {}

            for i in range(n):
                for j, x in enumerate(map(int, input().split())):
                    pa[x] = (i, j)
            for i in range(n):
                for j, x in enumerate(map(int, input().split())):
                    pb[x] = (i, j)

            rm, cm = {}, {}
            ok = True
            for x in pa:
                ra, ca = pa[x]
                rb, cb = pb[x]
                if ra in rm and rm[ra] != rb:
                    ok = False
                    break
                if ca in cm and cm[ca] != cb:
                    ok = False
                    break
                rm[ra] = rb
                cm[ca] = cb

            if ok:
                if len(set(rm.values())) != len(rm):
                    ok = False
                if len(set(cm.values())) != len(cm):
                    ok = False

            out.append("YES" if ok else "NO")

        return "\n".join(out)

    return solve()

# provided samples
assert run("""7
1 1
1
1
2 2
1 2
3 4
4 3
2 1
2 2
1 2
3 4
4 3
1 2
3 4
1 5 9 6
12 10 4 8
7 11 3 2
1 5 9 6
12 10 4 8
7 11 3 2
3 3
1 5 9
6 4 2
3 8 7
9 5 1
2 4 6
7 8 3
2 3
1 2 6
5 4 3
6 1 2
3 4 5
1 5
5 1 2 3 4
4 2 5 1 3
""") == """YES
YES
NO
YES
YES
NO
YES"""

# custom cases

# minimum size
assert run("""1
1 1
1
1
""") == "YES"

# impossible mapping
assert run("""1
2 2
1 2
3 4
1 3
2 4
""") == "NO"

# row-only swap
assert run("""1
2 3
1 2 3
4 5 6
4 5 6
1 2 3
""") == "YES"

# column-only swap
assert run("""1
3 2
1 2
3 4
5 6
2 1
4 3
6 5
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Danh tính 1×1 | CÓ | trường hợp cơ bản tầm thường | 
| ánh xạ không nhất quán | KHÔNG | phát hiện xung đột hàng/cột | 
| chỉ hoán vị hàng | CÓ | hoán đổi hàng một mình là đủ | 
| chỉ hoán vị cột | CÓ | hoán đổi cột một mình là đủ | 

## Vỏ cạnh 

Một trường hợp khó khăn là khi ánh xạ hàng nhất quán cục bộ nhưng xung đột trên toàn cầu. Ví dụ: nếu hai hàng khác nhau trong$a$cả hai đều cố gắng ánh xạ vào cùng một hàng trong$b$, thuật toán nắm bắt được điều này thông qua kiểm tra tính tiêm. 

Coi như:```
a:
1 2
3 4

b:
1 2
3 4
```Ở đây ánh xạ nhận dạng là hợp lệ. 

Bây giờ sửa đổi:```
b:
3 4
1 2
```Ánh xạ hàng trở thành 0→1 và 1→0, điều này ổn. Nhưng nếu hàng thứ ba tồn tại khi cố gắng ánh xạ không nhất quán, thì xung đột sẽ chỉ xuất hiện khi kiểm tra tính duy nhất toàn cầu của các giá trị được ánh xạ chứ không phải trên mỗi nhiệm vụ. 

Thuật toán xử lý chính xác vấn đề này bằng cách thực thi cả các ràng buộc nhất quán và song ánh trên toàn bộ không gian ánh xạ, thay vì dựa vào so sánh hàng cục bộ.
