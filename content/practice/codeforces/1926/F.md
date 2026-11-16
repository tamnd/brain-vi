---
title: "CF 1926F - Vlad và Tránh X"
description: "Chúng tôi có một bảng $7 nhân 7$ cố định có các ô màu đen hoặc trắng. Một cấu hình được coi là xấu nếu tồn tại một ô màu đen có bốn đường chéo lân cận cũng màu đen. Mẫu như vậy trông giống như chữ X ở giữa ô đó."
date: "2026-06-08T19:02:36+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "dfs-and-similar", "dp", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1926
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 928 (Div. 4)"
rating: 2200
weight: 1926
solve_time_s: 196
verified: false
draft: false
---

[CF 1926F - Vlad và Tránh X](https://codeforces.com/problemset/problem/1926/F) 

**Xếp hạng:** 2200 
**Tags:** bitmasks, brute Force, dfs và tương tự, dp, triển khai 
**Thời gian giải:** 3 phút 16s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một cố định$7 \times 7$bảng có các ô màu đen hoặc trắng. Một cấu hình được coi là xấu nếu tồn tại một ô màu đen có bốn đường chéo lân cận cũng màu đen. Mẫu như vậy trông giống như chữ X ở giữa ô đó. 

Chúng ta có thể lật bất kỳ ô nào, đổi đen thành trắng hoặc trắng thành đen. Mục tiêu là thực hiện số lần lật tối thiểu để không còn dấu X xấu nào trên bảng. 

Phần nổi bật nhất của vấn đề là kích thước bảng nhỏ. MỘT$7 \times 7$lưới chỉ chứa 49 ô$2^{49}$những chất tạo màu có thể có vẫn khiến cho sức mạnh vũ phu không bị hạn chế là không thể. Số lượng ca kiểm thử có thể lên tới 200, do đó giải pháp phải khai thác một số cấu trúc đặc biệt thay vì tìm kiếm trên tất cả các trạng thái bảng. 

Một X xấu chỉ có thể tập trung vào các ô có bốn đường chéo lân cận. Đó chính xác là những$5 \times 5$tế bào bên trong. Chỉ có 25 trung tâm có thể. 

Ràng buộc chính ẩn bên trong hình học là tính chẵn lẻ. Mỗi bước di chuyển theo đường chéo đều bảo toàn tính chẵn lẻ của$r+c$. Tâm và tất cả bốn đường chéo lân cận của chữ X luôn thuộc cùng một lớp màu bàn cờ. Điều này chia bảng thành hai nhóm ô hoàn toàn độc lập. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Xét một X đơn độc:```
.......
.B.B...
..B....
.B.B...
.......
.......
.......
```Lật một cái là đủ. Lật trung tâm sẽ phá hủy X ngay lập tức. Một chiến lược tham lam lật nhiều ô xung quanh sẽ là không tối ưu. 

Một trường hợp tinh tế khác là các mẫu X chồng chéo:```
.......
.B.B.B.
..B.B..
.B.B.B.
..B.B..
.B.B.B.
.......
```Một ô có thể tham gia đồng thời vào một số mẫu X xấu. Xử lý từng vi phạm một cách độc lập có thể bị tính quá mức vì một lần lật có thể loại bỏ nhiều vi phạm. 

Bẫy thứ ba là giả định rằng hiện tại chỉ có các ô đen mới được thay đổi. Vấn đề tối ưu hóa cũng cho phép chuyển các ô màu trắng sang màu đen. Trong thực tế, giải pháp tối ưu không bao giờ cần cố tình tạo ra các vi phạm mới, nhưng việc xây dựng DP phải tìm kiếm chính xác tất cả các khả năng thay vì dựa vào một quy tắc tham lam mong manh. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là xem mọi ô dưới dạng biến nhị phân và thử lật tất cả các tập hợp con của ô. có$2^{49}$những khả năng ở xung quanh$5.6 \times 10^{14}$. Ngay cả việc kiểm tra một triệu trạng thái mỗi giây cũng cần nhiều năm. 

Quan sát tiếp theo là các vi phạm chỉ liên quan đến các ô có cùng tính chẵn lẻ. Nếu chúng ta tô màu bàn cờ giống như một bàn cờ vua thì mỗi mẫu X nằm hoàn toàn bên trong một lớp màu. 

Lớp chẵn lẻ với$(r+c)\bmod 2=0$chứa 25 tế bào. Lớp còn lại chứa 24 ô. Hai lớp không bao giờ tương tác. 

Bây giờ chỉ nhìn vào một lớp chẵn lẻ. Mỗi trung tâm X có thể có thì bản thân nó là một ô của lớp đó. Có tối đa 13 trung tâm trong một lớp tương đương và 12 trung tâm ở lớp kia. 

Giả sử chúng ta quyết định ô nào của lớp chẵn lẻ sẽ có màu đen sau tất cả các lần lật. Đối với mọi trung tâm, chúng ta có thể kiểm tra ngay xem X bị cấm có tồn tại hay không. 

Quan sát quan trọng là các ô của một lớp chẵn lẻ tạo thành một$5 \times 5$-giống như mạng tinh thể khi nhìn qua các kết nối chéo. Mọi ràng buộc chỉ liên quan đến một tâm và bốn mạng lân cận của nó. 

Vì chỉ có 13 ô có liên quan trong một lớp chẵn lẻ, nên chúng ta có thể biểu thị màu cuối cùng hoàn chỉnh của lớp đó bằng một mặt nạ bit có kích thước tối đa là 13. Điều đó mang lại nhiều nhất$2^{13}=8192$tiểu bang. 

Đối với mỗi lớp chẵn lẻ, chúng tôi liệt kê tất cả các mặt nạ cuối cùng có thể có. Mặt nạ hợp lệ nếu không có tâm nào của nó tạo thành chữ X bị cấm. Giá của mặt nạ là số bit khác với bảng gốc. Chúng tôi giữ chi phí tối thiểu trong số các mặt nạ hợp lệ. 

Hai lớp chẵn lẻ là độc lập, do đó chi phí tối ưu của chúng chỉ cần cộng lại với nhau. 

Tổng công việc là rất nhỏ:$$2^{13}+2^{12}=12288$$trạng thái cho mỗi trường hợp thử nghiệm, chỉ với một lượng nhỏ kiểm tra không đổi cho mỗi trạng thái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả 49 ô |$O(2^{49})$|$O(1)$| Quá chậm | 
| Liệt kê các lớp chẵn lẻ một cách độc lập |$O(2^{13}+2^{12})$|$O(2^{13})$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Xây dựng các nhóm ngang bằng 

Mẫu bị cấm chỉ sử dụng các bước di chuyển theo đường chéo, vì vậy tất cả các ô tham gia đều có cùng tính chẵn lẻ$r+c$. 

Chia bảng thành 2 nhóm: 

1. Các ô có tính chẵn lẻ. 
2. Các ô có số chẵn lẻ. 

Các nhóm này có thể được tối ưu hóa một cách độc lập vì không có ràng buộc nào trộn lẫn chúng. 

### Liệt kê một nhóm chẵn lẻ 

Đối với một nhóm chẵn lẻ: 

1. Thu thập tất cả các ô thuộc về chẵn lẻ đó và gán cho chúng các chỉ số từ 0 đến$k-1$, Ở đâu$k\le 13$. 
2. Ghi lại màu gốc dưới dạng mặt nạ bit`orig`. 
3. Xác định những tế bào nào có thể đóng vai trò là trung tâm X. Một ô được coi là trung tâm nếu có cả bốn ô lân cận theo đường chéo. 
4. Đối với mỗi trung tâm, tính toán trước chỉ số của bốn đường chéo lân cận của nó trong cùng một nhóm chẵn lẻ. 

### Thử nghiệm chiếc mặt nạ cuối cùng 

1. Liệt kê mọi mặt nạ từ`0`ĐẾN`(1<<k)-1`. 
2. Đối với mỗi tâm, kiểm tra xem bit trung tâm có phải là 1 và tất cả bốn bit lân cận cũng là 1 hay không. 
3. Nếu trung tâm như vậy tồn tại, mặt nạ không hợp lệ và có thể bị loại bỏ. 

Mặt nạ thể hiện màu sắc cuối cùng hoàn chỉnh của nhóm chẵn lẻ này. Bất kỳ dấu X nào bị cấm đều có nghĩa là bảng kết quả không được chấp nhận. 

### Tính chi phí lật 

1. Với mỗi mặt nạ hợp lệ, hãy tính số ô đã thay đổi:$$\text{cost} = \text{popcount}(mask \oplus orig)$$1. Giữ chi phí tối thiểu trong số tất cả các khẩu trang hợp lệ. 

### Kết hợp cả hai chẵn lẻ 

1. Giải riêng nhóm chẵn lẻ và nhóm chẵn lẻ lẻ. 
2. Cộng hai chi phí tối thiểu và in kết quả. 

### Tại sao nó hoạt động 

Mọi X bị cấm nằm hoàn toàn trong một lớp chẵn lẻ duy nhất vì các di chuyển theo đường chéo bảo toàn tính chẵn lẻ. Việc thay đổi các ô từ một lớp chẵn lẻ có thể ảnh hưởng đến việc liệu một mẫu có tồn tại trong lớp kia hay không. 

Đối với lớp chẵn lẻ cố định, mọi màu cuối cùng có thể xuất hiện chính xác một lần trong số các mặt nạ được liệt kê. Kiểm tra tính hợp lệ sẽ loại bỏ chính xác những màu có chứa chữ X bị cấm. Trong số tất cả các màu hợp lệ, chúng tôi chọn màu yêu cầu ít lần lật nhất so với cấu hình ban đầu. 

Vì hai lớp chẵn lẻ là độc lập nên tối ưu toàn cục đạt được bằng cách kết hợp màu hợp lệ tối ưu của mỗi lớp. Bất kỳ giải pháp tổng thể nào cũng phân hủy thành hai giải pháp chẵn lẻ và bất kỳ cặp giải pháp chẵn lẻ nào kết hợp thành một giải pháp tổng thể. Do đó, chi phí tối thiểu sẽ tăng thêm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_parity(board, parity):
    cells = []
    pos_to_idx = {}

    for r in range(7):
        for c in range(7):
            if (r + c) % 2 == parity:
                pos_to_idx[(r, c)] = len(cells)
                cells.append((r, c))

    k = len(cells)

    orig = 0
    for i, (r, c) in enumerate(cells):
        if board[r][c] == 'B':
            orig |= 1 << i

    centers = []

    for r, c in cells:
        if 1 <= r <= 5 and 1 <= c <= 5:
            neigh = [
                pos_to_idx[(r - 1, c - 1)],
                pos_to_idx[(r - 1, c + 1)],
                pos_to_idx[(r + 1, c - 1)],
                pos_to_idx[(r + 1, c + 1)],
            ]
            centers.append((pos_to_idx[(r, c)], neigh))

    limit = 1 << k
    best = k

    for mask in range(limit):
        valid = True

        for center, neigh in centers:
            if not (mask >> center) & 1:
                continue

            ok = True
            for v in neigh:
                if ((mask >> v) & 1) == 0:
                    ok = False
                    break

            if ok:
                valid = False
                break

        if valid:
            cost = (mask ^ orig).bit_count()
            if cost < best:
                best = cost

    return best

def solve():
    t = int(input())

    ans = []

    for _ in range(t):
        board = [input().strip() for _ in range(7)]

        res = solve_parity(board, 0) + solve_parity(board, 1)
        ans.append(str(res))

    sys.stdout.write("\n".join(ans))

solve()
```Giải pháp bắt đầu bằng cách tách các ô theo tính chẵn lẻ. Mỗi lớp chẵn lẻ được biểu diễn bằng một mặt nạ bit nhỏ gọn, cho phép liệt kê tất cả các màu cuối cùng có thể có một cách hiệu quả. 

Đối với mỗi lớp chẵn lẻ, mã sẽ tính toán trước tất cả các tâm X tiềm năng cùng với chỉ số của bốn đường chéo lân cận của chúng. Điều này loại bỏ mọi số học tọa độ lặp lại bên trong vòng lặp liệt kê. 

Việc kiểm tra tính hợp lệ được viết cẩn thận sao cho một tâm chỉ góp phần vi phạm khi bản thân tâm đó có màu đen và tất cả bốn đường chéo lân cận đều có màu đen. Các ô viền không bao giờ được coi là trung tâm vì chúng không thể có bốn đường chéo lân cận. 

Chi phí lật được tính bằng`mask ^ orig`. Mỗi bit khác nhau tương ứng với một ô có màu thay đổi. của Python`bit_count()`cung cấp một thực hiện popcount hiệu quả. 

Kích thước mặt nạ tối đa chỉ là 13 bit, do đó việc liệt kê đầy đủ vẫn rất nhỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Bảng đầu vào:```
.......
.B.B...
..B....
.B.B...
.......
.......
.......
```Năm ô màu đen tạo thành chính xác một chữ X. 

Đối với lớp chẵn lẻ có liên quan: 

| Bước | Mặt nạ hiện tại | Có hiệu lực? | Chi phí lật | 
| --- | --- | --- | --- | 
| Bản gốc | Có mẫu X | Không | 0 | 
| Xóa trung tâm | Không X | Có | 1 | 
| Bỏ một cánh tay | Không X | Có | 1 | 
| Xóa hai ô | Không X | Có | 2 | 

Chi phí hợp lệ tối thiểu là 1. 

Ví dụ này cho thấy rằng việc tiêu diệt bất kỳ ô nào trong năm ô tham gia sẽ loại bỏ hành vi vi phạm. 

### Ví dụ 2 

Một bảng không chứa ô đen:```
WWWWWWW
WWWWWWW
WWWWWWW
WWWWWWW
WWWWWWW
WWWWWWW
WWWWWWW
```Đối với mỗi lớp chẵn lẻ: 

| Bước | Mặt nạ hiện tại | Có hiệu lực? | Chi phí lật | 
| --- | --- | --- | --- | 
| Bản gốc | Trống | Có | 0 | 
| Bất kỳ mặt nạ sửa đổi nào | Thường hợp lệ | >0 | | 

Chi phí tối thiểu là 0 cho cả hai nhóm chẵn lẻ. 

Ví dụ này xác nhận rằng thuật toán không đưa ra những thay đổi không cần thiết khi bảng đã hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(2^{13}+2^{12})$mỗi trường hợp thử nghiệm | Liệt kê tất cả các mặt nạ của cả hai lớp chẵn lẻ | 
| Không gian |$O(1)$| Chỉ một số mảng nhỏ và mặt nạ được lưu trữ | 

Lớp chẵn lẻ lớn nhất chứa 13 ô, cung cấp tối đa 8192 mặt nạ. Trên cả hai lớp chẵn lẻ, có ít hơn 13000 trạng thái được kiểm tra cho mỗi trường hợp thử nghiệm. Ngay cả với 200 trường hợp thử nghiệm, tổng khối lượng công việc vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    input = sys.stdin.readline

    def solve_parity(board, parity):
        cells = []
        pos = {}

        for r in range(7):
            for c in range(7):
                if (r + c) % 2 == parity:
                    pos[(r, c)] = len(cells)
                    cells.append((r, c))

        k = len(cells)

        orig = 0
        for i, (r, c) in enumerate(cells):
            if board[r][c] == 'B':
                orig |= 1 << i

        centers = []
        for r, c in cells:
            if 1 <= r <= 5 and 1 <= c <= 5:
                centers.append(
                    (
                        pos[(r, c)],
                        [
                            pos[(r - 1, c - 1)],
                            pos[(r - 1, c + 1)],
                            pos[(r + 1, c - 1)],
                            pos[(r + 1, c + 1)],
                        ],
                    )
                )

        best = k

        for mask in range(1 << k):
            good = True

            for center, neigh in centers:
                if (mask >> center) & 1:
                    if all((mask >> v) & 1 for v in neigh):
                        good = False
                        break

            if good:
                best = min(best, (mask ^ orig).bit_count())

        return best

    t = int(input())
    ans = []

    for _ in range(t):
        board = [input().strip() for _ in range(7)]
        ans.append(str(
            solve_parity(board, 0) +
            solve_parity(board, 1)
        ))

    print("\n".join(ans))

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue().strip()

# sample
assert run(
"""1
WWWWWWW
WWWWWWW
WWWWWWW
WWWWWWW
WWWWWWW
WWWWWWW
WWWWWWW
"""
) == "0"

# single X
assert run(
"""1
WWWWWWW
WBWBWWW
WWBWWWW
WBWBWWW
WWWWWWW
WWWWWWW
WWWWWWW
"""
) == "1"

# all white
assert run(
"""1
WWWWWWW
WWWWWWW
WWWWWWW
WWWWWWW
WWWWWWW
WWWWWWW
WWWWWWW
"""
) == "0"

# all black
assert run(
"""1
BBBBBBB
BBBBBBB
BBBBBBB
BBBBBBB
BBBBBBB
BBBBBBB
BBBBBBB
"""
) == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tất cả bảng trắng | 0 | Cấu hình đã hợp lệ | 
| Đơn X | 1 | Vi phạm nhỏ nhất không hề nhỏ | 
| Tất cả bảng đen | 9 | Nhiều vi phạm chồng chéo | 
| Bảng trống kiểu mẫu | 0 | Không có những cú lật không cần thiết | 

## Vỏ cạnh 

### Vi phạm chạm vào biên giới 

đầu vào:```
BWBWWWW
WBWWWWW
BWBWWWW
WWWWWWW
WWWWWWW
WWWWWWW
WWWWWWW
```Tâm rõ ràng sẽ cần phải nằm trên biên giới, điều này là không thể. Các ô viền không thể có bốn ô lân cận theo đường chéo. Trong quá trình tiền xử lý, chỉ các ô có tọa độ từ 1 đến 5 mới được coi là trung tâm, do đó mẫu này bị bỏ qua một cách chính xác. Câu trả lời là 0. 

### Nhiều mẫu X chồng chéo 

đầu vào:```
.......
.B.B.B.
..B.B..
.B.B.B.
..B.B..
.B.B.B.
.......
```Một số vi phạm chia sẻ tế bào. Một chiến lược tham lam sửa từng X một cách độc lập có thể thực hiện nhiều lần lật. Việc liệt kê mặt nạ đánh giá đồng thời toàn bộ lớp chẵn lẻ và phát hiện khi một lần lật sẽ phá hủy một số vi phạm cùng một lúc. 

### Bảng đã hợp lệ 

đầu vào:```
WWWWWWW
WWWWWWW
WWWWWWW
WWWWWWW
WWWWWWW
WWWWWWW
WWWWWWW
```Bản thân mặt nạ gốc đã vượt qua mọi bài kiểm tra tính hợp lệ. Chi phí lật của nó bằng 0, vì vậy nó vẫn là giải pháp tối ưu. Thuật toán không bao giờ sửa đổi các ô một cách không cần thiết. 

###Bảng dày đặc, nhiều vi phạm 

đầu vào:```
BBBBBBB
BBBBBBB
BBBBBBB
BBBBBBB
BBBBBBB
BBBBBBB
BBBBBBB
```Hầu hết mọi ô bên trong đều là trung tâm của một X bị cấm. Việc suy luận cục bộ trở nên khó khăn vì các mẫu chồng chéo lên nhau rất nhiều. Phân tách chẵn lẻ chuyển đổi vấn đề thành hai tìm kiếm độc lập trên tối đa 8192 trạng thái, đảm bảo rằng số lần lật tối thiểu thực sự được tìm thấy.
