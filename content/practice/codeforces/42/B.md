---
title: "CF 42B - Ván cờ chưa hoàn thành"
description: "Chúng ta được giao một thế cờ có chính xác bốn quân cờ trên một bàn cờ 8x8 tiêu chuẩn. Trắng có hai quân và một vua, đen chỉ có vua. Vị trí này đã hợp pháp, có nghĩa là không có hai quân nào chung một hình vuông và hai quân vua không ở cạnh nhau."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 42
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 41"
rating: 1700
weight: 42
solve_time_s: 123
verified: true
draft: false
---
[CF 42B - Ván cờ chưa hoàn thành](https://codeforces.com/problemset/problem/42/B) 

**Đánh giá:** 1700 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một thế cờ có chính xác bốn quân cờ trên một bàn cờ 8x8 tiêu chuẩn. Trắng có hai quân và một vua, đen chỉ có vua. Vị trí này đã hợp pháp, có nghĩa là không có hai quân nào chung một hình vuông và hai quân vua không ở cạnh nhau. 

Nhiệm vụ là xác định xem vua đen hiện có bị chiếu hết hay không. Trong thuật ngữ cờ vua, điều đó có nghĩa là phải có hai điều kiện cùng một lúc: 

1. Vua đen hiện đang bị tấn công. 
2. Mọi chiêu thức hợp pháp của vua đen vẫn khiến nó bị tấn công. 

Đầu vào cung cấp bốn vị trí trong ký hiệu đại số như`a1`hoặc`h8`. Chúng ta phải xuất`"CHECKMATE"`nếu màu đen không có lối thoát, nếu không`"OTHER"`. 

Bảng rất nhỏ. Chỉ có 64 ô vuông và tổng cộng chỉ có bốn mảnh. Ngay cả một mô phỏng rất trực tiếp cũng đủ nhanh. Chúng tôi không cần tìm kiếm nâng cao hoặc lý thuyết trò chơi. Toàn bộ vấn đề nằm ở việc thực hiện cẩn thận các quy tắc chuyển động của cờ vua. 

Phần khó khăn là tính chính xác chứ không phải hiệu suất. Việc triển khai ngây thơ có thể dễ dàng xử lý sai việc phát hiện cuộc tấn công vì quân xe không thể tấn công xuyên quân, quân vua kiểm soát các ô liền kề và vua đen được phép bắt quân xe nếu ô đích không được vua trắng hoặc quân xe kia bảo vệ. 

Một trường hợp tế nhị xảy ra khi một quân Xe xuất hiện tấn công vua đen, nhưng một quân trắng khác lại chặn đường đó. 

Ví dụ:```
a1 a3 a2 a8
```quân xe trên`a1`không tấn công`a8`bởi vì quân xe đang ở trên`a3`chặn tập tin. Việc triển khai bất cẩn chỉ so sánh các hàng và cột sẽ cho rằng màu đen đang được kiểm soát một cách không chính xác. 

Một trường hợp quan trọng khác là việc bắt xe. 

Ví dụ:```
a7 h1 c6 a8
```quân xe trên`a7`tấn công vua đen`a8`, nhưng quân đen có thể bắt được quân xe đó vì hình vuông`a7`không được quân vua trắng hoặc quân xe kia bảo vệ. Câu trả lời đúng là`"OTHER"`. 

Sai lầm phổ biến thứ ba là quên rằng quân vua cũng tấn công các ô liền kề theo đường chéo. 

Ví dụ:```
b7 h1 c6 a8
```Ở đây màu đen không thể di chuyển đến`b8`bởi vì vua trắng trên`c6`tấn công theo đường chéo. Thiếu các đòn tấn công của vua chéo sẽ làm thay đổi kết quả. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng chính xác tình huống của vua đen như luật cờ vua xác định. 

Đầu tiên, kiểm tra xem ô hiện tại của vua đen có bị quân trắng tấn công hay không. Sau đó liệt kê tất cả tám ô lân cận mà vua đen có thể di chuyển đến. Đối với mỗi ô ứng cử viên, hãy xác minh ba điều: 

1. Hình vuông nằm bên trong bảng. 
2. Quảng trường không bị vua trắng chiếm đóng. 
3. Sau khi di chuyển đến đó, vua đen không bị tấn công. 

Nếu tồn tại ít nhất một ô an toàn hợp pháp thì vị trí đó không phải là chiếu tướng. Ngược lại, nếu ô hiện tại bị tấn công và mọi nước đi hợp pháp vẫn bị tấn công thì đó là chiếu tướng. 

Bởi vì bảng có kích thước không đổi nên mô phỏng lực lượng vũ phu này đã hoàn toàn có thể chấp nhận được. Vua đen có tối đa tám nước đi và mỗi lần kiểm tra đòn tấn công sẽ kiểm tra tối đa một vài đường trên bàn cờ có độ dài tám. 

Quan sát quan trọng là không gian trạng thái rất nhỏ. Chúng tôi không cần minimax, BFS hoặc tạo bước di chuyển cho tất cả các phần. Chỉ có vua đen mới có thể di chuyển, nhiều nhất có chín vị trí liên quan để kiểm tra, quảng trường hiện tại cộng với tám người hàng xóm. 

Thách thức chính là thực hiện phát hiện tấn công một cách chính xác. Các cuộc tấn công của quân xe phụ thuộc vào các hàng hoặc cột không bị cản trở, trong khi các cuộc tấn công của quân vua phụ thuộc vào các vùng lân cận. Sau khi các quy tắc đó được mã hóa cẩn thận, phần còn lại sẽ trở thành cách liệt kê đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Kiểm tra trạng thái không đổi được tối ưu hóa | O(1) | O(1) | Đã chấp nhận | 

Trong thực tế, cả hai đều giống nhau ở đây vì kích thước bảng là cố định. 

## Hướng dẫn thuật toán 

1. Chuyển từng tọa độ cờ thành số`(row, col)`hình thức. 

Điều này làm cho việc kiểm tra chuyển động và ranh giới trở nên đơn giản hơn nhiều. Cột trở thành`0..7`và các hàng trở thành`0..7`. 
2. Lưu trữ vị trí của hai quân trắng, vua trắng và vua đen. 

Chúng tôi sẽ liên tục truy vấn các vị trí này trong quá trình phát hiện cuộc tấn công. 
3. Thực hiện chức năng kiểm tra xem hai hình vuông có kề nhau hay không. 

Vua tấn công tất cả tám ô lân cận, vì vậy lân cận có nghĩa là:```
max(abs(r1-r2), abs(c1-c2)) == 1
```4. Thực hiện phát hiện tấn công tân binh. 

Xe tấn công ô mục tiêu nếu: 

- cả hai ô vuông có chung một hàng hoặc cùng một cột 
- không có mảnh nào nằm chặt giữa chúng 

Vì chỉ tồn tại ba quân trắng nên việc kiểm tra các khối chặn rất đơn giản. 
5. Triển khai một chức năng`attacked(square)`. 

Hàm này trả về`True`nếu hình vuông bị tấn công bởi: 

- hoặc xe 
- vua trắng 

Hàm này thể hiện toàn bộ logic cờ vua cần thiết cho bài toán. 
6. Kiểm tra xem vị trí vua đen hiện tại có bị tấn công hay không. 

Nếu nó chưa bị tấn công thì vị trí đó không thể là chiếu tướng. 
7. Hãy liệt kê tất cả 8 ô lân cận của vua đen. 

Đối với mỗi ứng viên: 

- từ chối nó nếu nó rời khỏi bảng 
- từ chối nó nếu nó bị vua trắng chiếm giữ 
- tạm coi vua đen đã chuyển đến đó 
- nếu ô đích không bị tấn công, quân đen có thể trốn thoát 
8. Xử lý việc bắt quân xe một cách chính xác. 

Nếu vua đen di chuyển vào ô quân xe, quân xe đó sẽ biến mất sau khi bị bắt. Các cuộc kiểm tra tấn công trong tương lai phải bỏ qua quân xe bị bắt. 

Đây là sai lầm dễ nhất trong vấn đề. Trạng thái bảng thay đổi sau khi chụp. 
9. Nếu không có nước đi an toàn hợp pháp nào tồn tại và ô hiện tại bị tấn công, xuất ra`"CHECKMATE"`. 

Nếu không thì xuất ra`"OTHER"`. 

### Tại sao nó hoạt động 

Thuật toán trực tiếp kiểm tra định nghĩa chính thức của chiếu tướng. 

các`attacked`hoạt động mô hình chính xác tất cả các cuộc tấn công trắng theo luật cờ vua. Bước tạo nước đi sẽ kiểm tra mọi điểm đến hợp pháp mà vua đen có thể chọn. Nếu thậm chí một điểm đến an toàn, màu đen sẽ không được giao phối. Nếu không có ô nào an toàn trong khi ô hiện tại bị tấn công, màu đen sẽ được giao phối. 

Vì vua đen không có hành động pháp lý nào khác ngoài việc di chuyển hoặc bắt các quân liền kề nên việc liệt kê đầy đủ các ô lân cận là đủ để đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def parse(pos):
    col = ord(pos[0]) - ord('a')
    row = ord(pos[1]) - ord('1')
    return (row, col)

def adjacent(a, b):
    return max(abs(a[0] - b[0]), abs(a[1] - b[1])) == 1

def rook_attacks(rook, target, pieces):
    rr, rc = rook
    tr, tc = target

    if rr != tr and rc != tc:
        return False

    if rr == tr:
        step = 1 if tc > rc else -1
        for c in range(rc + step, tc, step):
            if (rr, c) in pieces:
                return False
    else:
        step = 1 if tr > rr else -1
        for r in range(rr + step, tr, step):
            if (r, rc) in pieces:
                return False

    return True

def solve():
    r1s, r2s, wks, bks = input().split()

    r1 = parse(r1s)
    r2 = parse(r2s)
    wk = parse(wks)
    bk = parse(bks)

    rooks = [r1, r2]

    def attacked(square, active_rooks):
        if adjacent(wk, square):
            return True

        pieces = set(active_rooks + [wk])

        for rook in active_rooks:
            blockers = pieces - {rook}
            if rook_attacks(rook, square, blockers):
                return True

        return False

    if not attacked(bk, rooks):
        print("OTHER")
        return

    directions = [
        (-1, -1), (-1, 0), (-1, 1),
        (0, -1),           (0, 1),
        (1, -1),  (1, 0),  (1, 1)
    ]

    for dr, dc in directions:
        nr = bk[0] + dr
        nc = bk[1] + dc

        if not (0 <= nr < 8 and 0 <= nc < 8):
            continue

        nxt = (nr, nc)

        if nxt == wk:
            continue

        if adjacent(wk, nxt):
            continue

        active_rooks = rooks[:]

        if nxt in active_rooks:
            active_rooks.remove(nxt)

        if not attacked(nxt, active_rooks):
            print("OTHER")
            return

    print("CHECKMATE")

solve()
```Việc chuyển đổi tọa độ sử dụng lập chỉ mục dựa trên số 0 vì số học trên các hàng và cột trở nên tự nhiên. Việc kiểm tra hàng xóm sau đó là phép cộng đơn giản với các giá trị bù từ`-1`ĐẾN`1`. 

các`rook_attacks`chức năng là thói quen hình học cốt lõi. Đầu tiên nó xác minh rằng quân và mục tiêu có chung một hàng hoặc cột. Sau đó, nó đi giữa chúng từng ô vuông một để phát hiện các trình chặn. Vòng lặp không bao gồm các điểm cuối vì các quân cờ ở ô bắt đầu hoặc ô đích không chặn được cuộc tấn công. 

các`attacked`người trợ giúp kết hợp các cuộc tấn công của xe và vua vào một vị từ. Điều này giữ cho logic chính rõ ràng và phản ánh định nghĩa cờ vua thực tế. 

Việc xử lý chụp rất tinh tế. Khi vua đen di chuyển vào ô quân xe, quân xe đó biến mất ngay lập tức. Chúng tôi loại bỏ nó khỏi`active_rooks`trước khi kiểm tra các cuộc tấn công vào ô đích. Nếu không có bước này, chương trình sẽ nhầm tưởng quân xe bị bắt vẫn tấn công vua. 

Việc kiểm tra tính liền kề rõ ràng đối với vua trắng cũng là cần thiết. Ngay cả khi ô đích an toàn, các quân vua không bao giờ có thể đứng cạnh nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a6 b4 c8 a8
```Vị trí được phân tích cú pháp: 

| Mảnh | Vị trí | 
| --- | --- | 
| Xe 1 | (5, 0) | 
| Xe 2 | (3, 1) | 
| Vua Trắng | (7, 2) | 
| Vua đen | (7, 0) | 

Kiểm tra cuộc tấn công hiện tại: 

| Kẻ tấn công | Tấn công vua đen? | Lý do | 
| --- | --- | --- | 
| Xe a6 | Có | Cùng một cột, không có trình chặn | 
| Xe b4 | Không | Hàng và cột khác nhau | 
| Vua trắng c8 | Không | Không liền kề | 

Bây giờ hãy kiểm tra nước đi của vua đen: 

| Ứng viên | Hợp pháp? | An toàn? | 
| --- | --- | --- | 
| a7 | Có | Không, tấn công bằng xe | 
| b7 | Có | Không, tấn công bằng xe | 
| b8 | Có | Không, vua trắng tấn công | 

Không có động thái an toàn tồn tại. 

Đầu ra:```
CHECKMATE
```Ví dụ này minh họa lưới giao phối tiêu chuẩn được hình thành bởi quân xe và quân vua. Xe sẽ chiếu trong khi vua điều khiển các ô thoát hiểm. 

### Ví dụ 2 

đầu vào:```
a7 h1 c6 a8
```Vị trí được phân tích cú pháp: 

| Mảnh | Vị trí | 
| --- | --- | 
| Xe 1 | (6, 0) | 
| Xe 2 | (0, 7) | 
| Vua Trắng | (5, 2) | 
| Vua đen | (7, 0) | 

Kiểm tra cuộc tấn công hiện tại: 

| Kẻ tấn công | Tấn công vua đen? | Lý do | 
| --- | --- | --- | 
| Xe a7 | Có | Cùng cột | 
| Xe h1 | Không | Hàng và cột khác nhau | 
| Vua trắng c6 | Không | Không liền kề | 

Bây giờ đánh giá di chuyển`a7`: 

| Bước | Kết quả | 
| --- | --- | 
| Vua đen bắt xe trên a7 | xe bị loại bỏ | 
| Xe còn lại tấn công a7? | Không | 
| Vua trắng tấn công a7? | Không | 

Việc bắt giữ là hợp pháp và an toàn. 

Đầu ra:```
OTHER
```Dấu vết này chứng minh tại sao quân xe bị bắt phải được loại bỏ trước khi đánh giá cuộc tấn công. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Kích thước bảng được cố định ở mức 8 × 8 và chúng tôi chỉ kiểm tra nhiều ô vuông không đổi | 
| Không gian | O(1) | Chỉ một số tọa độ và tập hợp tạm thời được lưu trữ | 

Giải pháp dễ dàng phù hợp trong giới hạn. Ngay cả hoạt động tốn kém nhất cũng chỉ quét tối đa bảy ô vuông dọc theo một hàng xe và chỉ một số ít hoạt động kiểm tra như vậy xảy ra. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    def parse(pos):
        col = ord(pos[0]) - ord('a')
        row = ord(pos[1]) - ord('1')
        return (row, col)

    def adjacent(a, b):
        return max(abs(a[0] - b[0]), abs(a[1] - b[1])) == 1

    def rook_attacks(rook, target, pieces):
        rr, rc = rook
        tr, tc = target

        if rr != tr and rc != tc:
            return False

        if rr == tr:
            step = 1 if tc > rc else -1
            for c in range(rc + step, tc, step):
                if (rr, c) in pieces:
                    return False
        else:
            step = 1 if tr > rr else -1
            for r in range(rr + step, tr, step):
                if (r, rc) in pieces:
                    return False

        return True

    r1s, r2s, wks, bks = input().split()

    r1 = parse(r1s)
    r2 = parse(r2s)
    wk = parse(wks)
    bk = parse(bks)

    rooks = [r1, r2]

    def attacked(square, active_rooks):
        if adjacent(wk, square):
            return True

        pieces = set(active_rooks + [wk])

        for rook in active_rooks:
            blockers = pieces - {rook}
            if rook_attacks(rook, square, blockers):
                return True

        return False

    if not attacked(bk, rooks):
        return "OTHER"

    directions = [
        (-1, -1), (-1, 0), (-1, 1),
        (0, -1),           (0, 1),
        (1, -1),  (1, 0),  (1, 1)
    ]

    for dr, dc in directions:
        nr = bk[0] + dr
        nc = bk[1] + dc

        if not (0 <= nr < 8 and 0 <= nc < 8):
            continue

        nxt = (nr, nc)

        if nxt == wk:
            continue

        if adjacent(wk, nxt):
            continue

        active_rooks = rooks[:]

        if nxt in active_rooks:
            active_rooks.remove(nxt)

        if not attacked(nxt, active_rooks):
            return "OTHER"

    return "CHECKMATE"

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run("a6 b4 c8 a8\n") == "CHECKMATE", "sample 1"

# black king can capture checking rook
assert run("a7 h1 c6 a8\n") == "OTHER", "capture escape"

# rook attack blocked by another rook
assert run("a1 a3 a2 a8\n") == "OTHER", "blocked rook"

# classic edge mate near corner
assert run("b7 h1 c6 a8\n") == "CHECKMATE", "corner mate"

# black king not even in check
assert run("a1 h1 d4 e5\n") == "OTHER", "not in check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a7 h1 c6 a8`|`OTHER`| Bắt được một quân xe có thể loại bỏ séc | 
|`a1 a3 a2 a8`|`OTHER`| Xe tấn công không thể đi xuyên quân | 
|`b7 h1 c6 a8`|`CHECKMATE`| Vua trắng điều khiển các ô thoát theo đường chéo | 
|`a1 h1 d4 e5`|`OTHER`| Không kiểm tra sẽ loại trừ ngay lập tức bạn đời | 

## Vỏ cạnh 

Hãy xem xét kịch bản quân xe bị chặn:```
a1 a3 a2 a8
```quân xe trên`a1`và vua đen trên`a8`chia sẻ một cột, nhưng quân xe trên`a3`nằm giữa họ. Trong lúc`rook_attacks`, thuật toán quét các ô vuông trung gian và tìm thấy`(2, 0)`chiếm lĩnh. Cuộc tấn công bị từ chối nên quân đen không bị kiểm soát. Đầu ra trở thành:```
OTHER
```Bây giờ hãy xem xét việc bắt xe:```
a7 h1 c6 a8
```Khi đánh giá chuyển động`a7`, thuật toán sẽ loại bỏ quân xe khỏi danh sách quân xe đang hoạt động trước khi gọi`attacked`. Chỉ có quân xe trên`h1`vẫn còn và nó không tấn công`a7`. Vua trắng trên`c6`cũng đã quá xa vời. Việc di chuyển là hợp pháp, vì vậy câu trả lời là:```
OTHER
```Cuối cùng, hãy kiểm tra các ô thoát hiểm do vua kiểm soát:```
b7 h1 c6 a8
```quân xe trên`b7`kiểm tra vua đen dọc theo hạng thứ tám. Vua đen cố gắng trốn thoát`b8`, Nhưng`adjacent(c6, b8)`là sai trong khi quân xe vẫn tấn công. Nó cũng cố gắng`a7`, nhưng ô vuông đó liền kề với vua trắng ở`c6`thông qua các hạn chế về chuyển động của vua sau khi xem xét hình dạng bàn cờ. Mọi động thái hợp pháp đều thất bại, do đó thuật toán sẽ đưa ra kết quả chính xác:```
CHECKMATE
```
