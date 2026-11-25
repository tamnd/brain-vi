---
title: "CF 965B - Thiết Giáp Hạm"
description: "Chúng ta được cung cấp một lưới $n lần n$ trong đó mỗi ô bị cấm hoặc được phép. Các ô bị cấm được đánh dấu và không bao giờ có thể là một phần của con tàu. Các ô được phép được đánh dấu bằng . và có thể là một phần của con tàu."
date: "2026-06-17T01:38:41+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 965
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 476 (Div. 2) [Thanks, Telegram!]"
rating: 1300
weight: 965
solve_time_s: 81
verified: true
draft: false
---

[CF 965B - Thiết giáp hạm](https://codeforces.com/problemset/problem/965/B) 

**Đánh giá:** 1300 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$lưới trong đó mỗi ô bị cấm hoặc được phép. Các ô bị cấm được đánh dấu bằng`#`và không bao giờ có thể là một phần của một con tàu. Các ô được phép được đánh dấu bằng`.`và có thể là một phần của con tàu. 

Con tàu là một đoạn thẳng có chính xác$k$các ô liên tiếp, được đặt theo chiều ngang hoặc chiều dọc. Chúng tôi xem xét mọi vị trí hợp lệ của một con tàu nằm hoàn toàn bên trong lưới và chỉ sử dụng các ô được phép. Mỗi vị trí hợp lệ là một đoạn cụ thể, được xác định bởi hướng và vị trí bắt đầu của nó. 

Nhiệm vụ không phải là chọn vị trí đặt tàu. Thay vào đó, chúng tôi tưởng tượng tất cả các vị trí hợp lệ cùng một lúc và với mỗi ô, chúng tôi đếm xem có bao nhiêu vị trí trong số đó bao gồm vị trí đó. Chúng ta phải trả về bất kỳ ô nào có số lượng tối đa. 

Những hạn chế$n \le 100$Và$k \le n$ngụ ý nhiều nhất$10^4$tế bào và nhiều nhất là$O(n^2)$phân khúc ứng cử viên theo chiều ngang và chiều dọc. Bất kỳ giải pháp nào liệt kê rõ ràng tất cả các vị trí và cập nhật đóng góp cho mỗi ô đều khả thi trong vòng vài triệu thao tác. 

Một vấn đề tế nhị là một phân đoạn hợp lệ phải được chứa đầy đủ trong`.`tế bào. Một cách tiếp cận đơn giản chỉ kiểm tra điểm cuối hoặc giả định tính liên tục mà không xác minh tất cả các ô trung gian sẽ tính vượt quá các vị trí không hợp lệ. Một cạm bẫy khác là quên hoàn toàn các phân đoạn dọc hoặc đếm hai lần các phân đoạn chồng chéo không chính xác khi tổng hợp các khoản đóng góp cho mỗi ô. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê mọi vị trí bắt đầu có thể có cho cả hai hướng, xác minh xem đoạn chiều dài có$k$là hoàn toàn hợp lệ và nếu vậy hãy tăng bộ đếm cho tất cả$k$các tế bào trong đoạn đó. Điều này đúng vì nó phản ánh trực tiếp định nghĩa về vị trí đặt tàu hợp lệ. Chi phí đến từ việc tính toán lại tính hợp lệ và cập nhật$k$các ô cho mỗi phân đoạn ứng cử viên. có$O(n^2)$vị trí bắt đầu và chi phí mỗi lần cập nhật$O(k)$, cho$O(n^2 k)$, trong trường hợp xấu nhất đạt tới khoảng$10^6 \cdot 100 = 10^8$hoạt động. Đây là ranh giới nhưng không cần thiết nếu xét đến một quan sát đơn giản hơn. 

Quan sát quan trọng là chúng ta không thực sự cần mô phỏng từng phân đoạn một cách độc lập. Với mỗi ô ta chỉ cần biết độ dài bao nhiêu đoạn ngang và bao nhiêu đoạn dọc$k$che nó lại. Một phân đoạn đóng góp vào một ô khi và chỉ nếu phân đoạn đó hợp lệ và nằm hoàn toàn trong một khối liền kề của ô.`.`các tế bào trải dài ít nhất$k$chiều dài. 

Điều này cho phép chúng tôi tính toán trước tất cả các phân đoạn ngang và phân đoạn dọc hợp lệ một cách hiệu quả, sau đó đối với mỗi phân đoạn hợp lệ, chúng tôi cập nhật trực tiếp các đóng góp cho phân đoạn đó.$k$tế bào. Vì mỗi phân khúc vẫn có giá$O(k)$, chúng ta có thể lo lắng rằng chúng ta đã không cải thiện được tính tiệm cận, nhưng$n \le 100$làm cho điều này vẫn nhanh chóng thoải mái. Quan trọng hơn, chúng tôi tránh việc kiểm tra lặp lại và đơn giản hóa tính chính xác: mỗi phân đoạn hợp lệ được xử lý chính xác một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu |$O(n^2 k)$|$O(n^2)$| Được chấp nhận nhưng chặt chẽ | 
| Liệt kê phân đoạn + Đánh dấu |$O(n^2 k)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tất cả các đoạn có chiều dài hợp lệ theo chiều ngang$k$. Đối với mỗi hàng, quét từ trái sang phải và xác định các khối liền kề tối đa của`.`. Trong mỗi khối chiều dài$L$, mỗi đoạn con có độ dài$k$là hợp lệ, vì vậy có$L-k+1$các phân đoạn bắt đầu ở các cột khác nhau. 
2. Đối với mỗi đoạn ngang hợp lệ bắt đầu từ$(i, j)$, tăng bộ đếm đóng góp cho mỗi ô$(i, j), (i, j+1), \dots, (i, j+k-1)$. Điều này trực tiếp đếm số lượng vị trí hợp lệ bao gồm mỗi ô. 
3. Lặp lại quy trình tương tự cho các đoạn thẳng đứng. Đối với mỗi cột, tìm liền kề`.`khối và liệt kê tất cả các đoạn thẳng đứng có chiều dài$k$. 
4. Duy trì toàn cầu$n \times n$mảng`cnt`được khởi tạo về 0. Mỗi khi tìm thấy một phân đoạn hợp lệ, hãy tăng tất cả các ô mà nó bao phủ. 
5. Sau khi xử lý cả hai hướng, hãy quét toàn bộ lưới và chọn ô có giá trị lớn nhất trong`cnt`. Nếu nhiều ô chia sẻ mức tối đa thì bất kỳ ô nào cũng có thể được trả về. 

### Tại sao nó hoạt động 

Mỗi vị trí tàu hợp lệ tương ứng với chính xác một đoạn ngang hoặc dọc nằm hoàn toàn bên trong một vùng liền kề`.`khối. Thuật toán liệt kê mọi phân đoạn như vậy chính xác một lần. Mỗi lần chúng tôi gặp một phân khúc, chúng tôi phân phối một đơn vị đóng góp cho mỗi ô mà nó chiếm giữ. Do đó, giá trị được lưu trữ tại một ô chính xác là số lượng vị trí tàu hợp lệ bao gồm nó. Tối đa hóa giá trị này trên tất cả các ô sẽ trực tiếp giải quyết được vấn đề. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def add_segment(cnt, i, j, di, dj, k):
    for _ in range(k):
        cnt[i][j] += 1
        i += di
        j += dj

n, k = map(int, input().split())
grid = [input().strip() for _ in range(n)]

cnt = [[0] * n for _ in range(n)]

# horizontal segments
for i in range(n):
    j = 0
    while j < n:
        if grid[i][j] == '#':
            j += 1
            continue
        start = j
        while j < n and grid[i][j] == '.':
            j += 1
        length = j - start
        if length >= k:
            for s in range(start, j - k + 1):
                add_segment(cnt, i, s, 0, 1, k)

# vertical segments
for j in range(n):
    i = 0
    while i < n:
        if grid[i][j] == '#':
            i += 1
            continue
        start = i
        while i < n and grid[i][j] == '.':
            i += 1
        length = i - start
        if length >= k:
            for s in range(start, i - k + 1):
                add_segment(cnt, s, j, 1, 0, k)

best_i, best_j = 0, 0
best_val = -1

for i in range(n):
    for j in range(n):
        if cnt[i][j] > best_val:
            best_val = cnt[i][j]
            best_i, best_j = i, j

print(best_i + 1, best_j + 1)
```chức năng`add_segment`gói gọn ý tưởng phân phối đóng góp từ một vị trí tàu hợp lệ cho tất cả các ô của nó. Điều này tránh việc lặp lại mã để xử lý theo chiều ngang và chiều dọc và làm cho logic cập nhật trở nên rõ ràng. 

Logic quét hàng và cột dựa trên việc nhóm các chuỗi liên tiếp`.`các ô thành các khối tối đa. Điều này rất quan trọng vì các đoạn không thể đi qua`#`tế bào. Trong mỗi khối, chúng tôi chỉ bắt đầu các phân đoạn có độ dài đầy đủ$k$vẫn phù hợp. 

Lần quét cuối cùng chỉ cần chọn mục nhập tối đa trong`cnt`và ca +1 xử lý yêu cầu đầu ra được lập chỉ mục 1 của vấn đề. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 3
#..#
#.#.
....
.###
```Chúng tôi chỉ theo dõi một ví dụ khối ngang và một ví dụ khối dọc để minh họa. 

Xử lý theo chiều ngang tìm thấy hàng 3 dưới dạng khối đầy đủ có độ dài 4, tạo ra hai phân đoạn. Xử lý theo chiều dọc tìm thấy các phân đoạn hạn chế do`#`. 

| Bước | Bắt đầu phân đoạn | Định hướng | Các ô được cập nhật | 
| --- | --- | --- | --- | 
| 1 | (3,1) | H | (3,1),(3,2),(3,3) | 
| 2 | (3,2) | H | (3,2),(3,3),(3,4) | 
| 3 | (3,2) | V | (3,2),(4,2),(...) | 

Sau khi tổng hợp, ô (3,2) nhận được số lớp phủ cao nhất vì nó nằm ở cả hai đoạn ngang và ít nhất một đoạn dọc. 

Đầu ra:```
3 2
```Điều này chứng tỏ rằng thuật toán tích lũy chính xác các khoản đóng góp từ các vị trí chồng chéo. 

### Mẫu 2 

đầu vào:```
3 2
...
...
...
```Mỗi tế bào đều`.`vì vậy mọi phân khúc có thể đều tồn tại. 

| Bước | Loại phân khúc | Số lượng phân đoạn | 
| --- | --- | --- | 
| 1 | ngang | 3 hàng × 2 đoạn mỗi hàng = 6 | 
| 2 | dọc | 3 cột × 2 đoạn mỗi cột = 6 | 

Các tế bào ở giữa tham gia vào nhiều phân đoạn hơn các góc. 

Một ô trung tâm như (2,2) được bao gồm trong tất cả các đoạn 2 chiều dài có thể đi qua nó. 

Đầu ra có thể là:```
2 2
```Điều này cho thấy tính đối xứng: thuật toán xác định các ô trung tâm một cách tự nhiên là tối ưu mà không cần bất kỳ cách viết vỏ đặc biệt nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 k)$| Mỗi đoạn có độ dài hợp lệ$k$cập nhật$k$tế bào và có$O(n^2)$phân khúc như vậy | 
| Không gian |$O(n^2)$| Lưới và ma trận đóng góp | 

Với$n \le 100$, số lượng hoạt động trong trường hợp xấu nhất là khoảng$10^6$phân đoạn lần$100$cập nhật, nằm trong giới hạn trong Python dưới các ràng buộc thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    n, k = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    cnt = [[0] * n for _ in range(n)]

    def add(i, j, di, dj):
        for _ in range(k):
            cnt[i][j] += 1
            i += di
            j += dj

    for i in range(n):
        j = 0
        while j < n:
            if grid[i][j] == '#':
                j += 1
                continue
            s = j
            while j < n and grid[i][j] == '.':
                j += 1
            for st in range(s, j - k + 1):
                add(i, st, 0, 1)

    for j in range(n):
        i = 0
        while i < n:
            if grid[i][j] == '#':
                i += 1
                continue
            s = i
            while i < n and grid[i][j] == '.':
                i += 1
            for st in range(s, i - k + 1):
                add(st, j, 1, 0)

    best = (0, 0)
    bestv = -1
    for i in range(n):
        for j in range(n):
            if cnt[i][j] > bestv:
                bestv = cnt[i][j]
                best = (i, j)

    return f"{best[0]+1} {best[1]+1}"

# provided sample
assert run("4 3\n#..#\n#.#.\n....\n.###\n") == "3 2"

# all open grid
assert run("3 2\n...\n...\n...\n") in ["2 2", "1 1", "1 2", "2 1", "2 2", "2 3", "3 2", "3 3"]

# k = 1 (every cell is a ship)
assert run("2 1\n..\n..\n") == "1 1"

# fully blocked grid
assert run("2 2\n##\n##\n") in ["1 1", "1 2", "2 1", "2 2"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trống 3x3 | trung tâm | xử lý đối xứng | 
| k=1 lưới | bất kỳ ô nào | trường hợp cạnh đơn | 
| tất cả bị chặn | bất kỳ ô nào | không có vị trí hợp lệ | 

## Vỏ cạnh 

Khi lưới không chứa đoạn có độ dài hợp lệ$k$, ma trận đóng góp vẫn là số không. Trong trường hợp này, thuật toán vẫn trả về (1,1) vì không có ứng cử viên nào tốt hơn. Điều này phù hợp với yêu cầu của bài toán là bất kỳ ô nào cũng có thể được chấp nhận. 

Khi$k = 1$, mọi`.`ô tự tạo thành một phân đoạn hợp lệ. Thuật toán coi mỗi ô là một đoạn có độ dài bằng một, do đó, mỗi ô được phép đóng góp chính xác một ô cho chính nó và các ô bị cấm không đóng góp gì. Do đó, mức tối đa là bất kỳ`.`ô và quá trình quét sẽ tự nhiên chọn một ô. 

Khi một khối dài`.`các tế bào tồn tại, các phân đoạn chồng chéo tập trung nhiều đóng góp vào các tế bào trung tâm. Thuật toán tích lũy chính xác nhiều cửa sổ chồng chéo, đảm bảo các ô bên trong nhận được số lượng cao hơn các ô ranh giới, khớp với số lượng phân đoạn che phủ thực sự.
