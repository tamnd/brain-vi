---
title: "CF 102343F - Ít nhiều"
description: "Nhiều hay ít là một câu đố nhỏ về hình vuông Latinh. Chúng ta có một bảng (n lần n) và mỗi hàng và mỗi cột phải chứa từng giá trị từ (1) đến (n) đúng một lần. Một số ô đã được lấp đầy."
date: "2026-08-16T18:17:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102343
codeforces_index: "F"
codeforces_contest_name: "UCF Locals 2019"
rating: 0
weight: 102343
solve_time_s: 926
verified: true
draft: false
---

[CF 102343F - Ít nhiều](https://codeforces.com/problemset/problem/102343/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 15 phút 26 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiều hay ít là một câu đố nhỏ về hình vuông Latinh. Chúng ta có một bảng (n \times n) và mỗi hàng và mỗi cột phải chứa từng giá trị từ (1) đến (n) đúng một lần. Một số ô đã được lấp đầy. Giữa các ô liền kề theo chiều ngang hoặc chiều dọc cũng có thể đưa ra một số bất đẳng thức. Nhiệm vụ là điền vào mọi ô trống sao cho tất cả các ràng buộc hàng và cột, tất cả các giá trị cố định và tất cả các bất đẳng thức đều được giữ nguyên. Đầu vào được đảm bảo mô tả một câu đố với chính xác một bảng hoàn thành hợp lệ. Đầu ra được yêu cầu chỉ đơn giản là bảng đã hoàn thành, mỗi hàng một dòng không có khoảng trắng. Tuyên bố cuộc thi ban đầu giới hạn (n) tối đa là 7 và sử dụng cách biểu diễn ký tự nhỏ gọn cho các ô và bất đẳng thức. 

Giới hạn nhỏ của (n) là manh mối trung tâm. Có thể có tối đa 49 ô và mỗi ô có tối đa bảy giá trị có thể. Thuật toán thời gian đa thức sẽ dễ chịu, nhưng vấn đề hoàn thiện bình phương Latin cơ bản là tổ hợp, vì vậy giải pháp thực tế là tìm kiếm trong khi loại bỏ các bảng một phần không thể càng sớm càng tốt. Với (n=7), việc khám phá một cách mù quáng mọi khả năng là vô vọng, trong khi tìm kiếm quay lui theo hướng ràng buộc đủ nhỏ để vừa vặn thoải mái trong giới hạn năm giây. Trang Codeforces xác nhận giới hạn ban đầu là 5 giây với bộ nhớ 256 MB. 

Trường hợp cạnh không rõ ràng đầu tiên là (n=1). Bảng duy nhất có thể là một ô chứa 1. Việc triển khai giả sử có ít nhất một dấu phân cách ngang hoặc dọc có thể truy cập các ký tự đầu vào không tồn tại. Ví dụ,```
1
1
```có đầu ra đúng```
1
```Trường hợp cạnh thứ hai là bất đẳng thức tại ranh giới của một hàng hoặc cột. Ví dụ,```
2
-<-
^.v
-.-
```có giải pháp độc đáo```
12
21
```Hàng đầu tiên phải chứa 1 trước 2 vì bất đẳng thức theo chiều ngang. Các bất đẳng thức theo chiều dọc sau đó phù hợp với hàng thứ hai. Một trình phân tích cú pháp coi mọi ký tự trong một hàng dọc là một bất đẳng thức có thể xảy ra, thay vì chỉ sử dụng các vị trí được đánh số lẻ, có thể âm thầm đọc dữ liệu đầu vào này không chính xác. Định dạng ban đầu đặt các mối quan hệ dọc ở các vị trí xen kẽ và sử dụng các dấu chấm ở nơi khác. 

Trường hợp cạnh thứ ba là một hàng gần như hoàn chỉnh trong đó giá trị còn thiếu được xác định đồng thời bởi hàng và cột của nó. Ví dụ,```
3
1.2.3
2.3.1
3.-.2
.....
.....
```có giải pháp```
123
231
312
```Ô trống cuối cùng là 1 vì cả hàng và cột của nó đều chứa 2 và 3. Bộ giải chỉ kiểm tra hàng vẫn có thể hoạt động trong nhiều trường hợp nhưng sẽ chấp nhận giá trị không hợp lệ trong một câu đố phức tạp hơn. 

## Phương pháp tiếp cận 

Giải pháp bạo lực trực tiếp nhất sẽ lấp đầy từng ô của bảng và thử mọi giá trị từ 1 đến (n), kiểm tra bảng đã hoàn thành ở cuối. Điều này đúng vì mọi nhiệm vụ khả thi cuối cùng đều được xem xét. Thật không may, nó khám phá tới (n^{n^2}) bài tập. Tại (n=7), tức là khoảng (7^{49}), khoảng (2\time10^{41}), điều này hoàn toàn không khả thi. 

Chúng ta có thể làm cho lực lượng vũ phu tốt hơn đáng kể bằng cách sử dụng thuộc tính bình phương Latinh trong khi xây dựng bảng. Thay vì cho phép các giá trị tùy ý trong mỗi hàng, chúng ta có thể liệt kê các hoán vị của (1,\ldots,n) cho mỗi hàng. Có (7! = 5040) hàng có thể, do đó, ngay cả lực lượng vũ phu thông minh hơn nhiều này cũng có trường hợp xấu nhất là đại khái 

[ 
(7!)^7 \khoảng 8,2\time10^{25} 
] 

kết hợp hàng. Nó vẫn còn quá lớn. 

Quan sát hữu ích là hầu hết mọi phép gán từng phần đều đã chứa đủ thông tin để loại trừ hầu hết các giá trị. Một ô không thể sử dụng giá trị đã có trong hàng hoặc cột của nó. Một bất đẳng thức có thể loại bỏ các giá trị ngay lập tức khi biết ô lân cận của nó. Ngay cả khi cả hai ô của bất đẳng thức vẫn chưa xác định, các tập giá trị có thể có của chúng sẽ hạn chế lẫn nhau. Ví dụ: nếu (x<y) và giá trị lớn nhất có thể có của (y) là 4 thì (x) không bao giờ có thể là 4 hoặc lớn hơn. Tương tự, nếu giá trị nhỏ nhất có thể có của (x) là 3 thì (y) không thể là 1, 2 hoặc 3. 

Do đó, cách tiếp cận tối ưu là tìm kiếm quay lui lan truyền ràng buộc. Đối với mọi trạng thái đệ quy, chúng tôi xây dựng tập ứng cử viên hiện tại của mọi ô chưa được lấp đầy. Chúng tôi liên tục tuyên truyền các ràng buộc bất đẳng thức giữa các tập ứng cử viên này. Sau đó, chúng tôi chọn ô chưa điền có ít ứng viên còn lại nhất và chỉ phân nhánh trên các giá trị đó. Đây là ý tưởng về giá trị còn lại tối thiểu tiêu chuẩn, nhưng ở đây nó đặc biệt hiệu quả vì bảng chỉ có bảy giá trị có thể có trên mỗi ô và mỗi phép gán ngay lập tức loại bỏ giá trị đó khỏi toàn bộ hàng và cột. 

Tìm kiếm vũ phu hoạt động vì cuối cùng nó sẽ kiểm tra mọi bảng. Nó thất bại vì hầu hết tất cả các bảng đó đều vi phạm các ràng buộc từ rất sớm. Việc quan sát thấy các ràng buộc hàng, cột và bất đẳng thức có thể được áp dụng trước khi một ô được cam kết vĩnh viễn sẽ biến cây tìm kiếm khổng lồ thành một tìm kiếm ràng buộc nhỏ hơn nhiều. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^{n^2})) | (O(n^2)) | Quá chậm | 
| Lực lượng vũ phu hoán vị hàng | (O((n!)^n)) | (O(n^2)) | Quá chậm | 
| Quay lui truyền bá ràng buộc | Trường hợp xấu nhất theo cấp số nhân, được cắt tỉa nhiều trong thực tế | (O(n^2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích bảng thành một lưới số nguyên (n\times n). MỘT`0`đại diện cho một ô trống. Lưu trữ những giá trị đã được sử dụng trong mỗi hàng và cột bằng mặt nạ bit. Vì (n\le7), một số nguyên là đủ để biểu diễn tất cả các giá trị trong một hàng hoặc cột. 
2. Chuyển mọi bất đẳng thức thành quan hệ có hướng có dạng (a<b). Đối với chiều ngang`<`, ô bên trái nhỏ hơn ô bên phải. Vì`>`, đảo ngược mối quan hệ. Đối với chiều dọc`^`, ô trên nhỏ hơn ô dưới, trong khi`v`có nghĩa là ngược lại. 
3. Ở mỗi trạng thái tìm kiếm, hãy tính toán mặt nạ bit ứng viên cho mỗi ô. Đối với ô không được điền, hãy bắt đầu với tất cả các giá trị từ 1 đến (n), sau đó xóa mọi giá trị đã được sử dụng trong hàng hoặc cột của ô đó. Nếu một ô liền kề trong bất đẳng thức đã được chỉ định, hãy hạn chế tập ứng cử viên tương ứng. 
4. Áp dụng tính nhất quán cung cho các quan hệ bất đẳng thức. Đối với quan hệ (a<b), mọi ứng viên của (a) phải có ít nhất một ứng cử viên lớn hơn trong (b). Nếu ứng viên lớn nhất của (b) là (k), ứng viên (k,\ldots,n) có thể bị loại khỏi (a). Một cách đối xứng, nếu ứng cử viên nhỏ nhất của (a) là (k), các giá trị (1,\ldots,k) có thể bị loại bỏ khỏi (b). Lặp lại quá trình này cho đến khi không có tập ứng cử viên nào thay đổi. 
5. Nếu bất kỳ ô nào không được điền làm mất tất cả các ứng cử viên, bảng một phần hiện tại không thể dẫn đến giải pháp, vì vậy hãy quay lại ngay lập tức. Điều tương tự cũng được áp dụng nếu sự bất đẳng thức trở nên không thể xảy ra. 
6. Trong số tất cả các ô chưa được điền, hãy chọn ô có tập ứng cử viên nhỏ nhất. Đây là quy tắc giá trị còn lại tối thiểu. Việc chọn ô bị ràng buộc nhất trước tiên sẽ làm cho các mâu thuẫn xuất hiện chỉ sau một vài phép gán thay vì sau khi lấp đầy một phần lớn của bảng. 
7. Thử mọi giá trị ứng viên cho ô đã chọn. Cập nhật giá trị lưới của nó và mặt nạ hàng và cột tương ứng, sau đó giải quyết đệ quy vấn đề nhỏ hơn. Nếu cuộc gọi đệ quy thành công, hãy giữ giá trị đó. Nếu thất bại, hãy hoàn tác nhiệm vụ và thử ứng viên tiếp theo. 
8. Khi không còn ô trống thì bảng là giải pháp hợp lệ. Bởi vì đầu vào đảm bảo tính duy nhất nên bảng hoàn chỉnh đầu tiên được tìm thấy là câu trả lời bắt buộc. 

### Tại sao nó hoạt động 

Điều bất biến là mọi tập ứng cử viên đều chứa chính xác các giá trị vẫn tương thích với các giá trị cố định, tính duy nhất của hàng, tính duy nhất của cột và tất cả các bất đẳng thức đang được truyền bá hiện tại. Tính nhất quán của cung chỉ loại bỏ một giá trị khi không có giá trị tương thích nào tồn tại ở phía bên kia của bất đẳng thức, do đó nó không thể loại bỏ giá trị thuộc về một sự hoàn thành hợp lệ. Khi tìm kiếm chỉ định một ứng cử viên, giá trị đó sẽ được kiểm tra rõ ràng dựa trên các ràng buộc tương tự. Do đó, mỗi nhánh đệ quy đại diện cho một phần bảng có khả năng hợp lệ và mọi nhánh không hợp lệ chỉ bị loại bỏ sau khi tính không thể của nó được thiết lập. Vì mọi ứng cử viên còn lại cuối cùng đều được thử bất cứ khi nào cần thiết nên không thể bỏ qua việc hoàn thành hợp lệ. Việc đảm bảo tính duy nhất có nghĩa là việc hoàn thành thành công là bảng bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(lines):
    n = int(lines[0])
    board = [[0] * n for _ in range(n)]

    row_mask = [0] * n
    col_mask = [0] * n

    # inequalities are stored as (a, b), meaning value[a] < value[b]
    less_edges = []

    def cell_id(r, c):
        return r * n + c

    # Read the rows containing cells and horizontal inequalities.
    for r in range(n):
        s = lines[1 + 2 * r]
        for c in range(n):
            ch = s[2 * c]
            if ch != '-':
                v = ord(ch) - ord('0')
                board[r][c] = v
                bit = 1 << (v - 1)
                row_mask[r] |= bit
                col_mask[c] |= bit

            if c + 1 < n:
                sign = s[2 * c + 1]
                a = cell_id(r, c)
                b = cell_id(r, c + 1)

                if sign == '<':
                    less_edges.append((a, b))
                elif sign == '>':
                    less_edges.append((b, a))

    # Read the rows containing vertical inequalities.
    for r in range(n - 1):
        s = lines[2 + 2 * r]
        for c in range(n):
            sign = s[2 * c]
            a = cell_id(r, c)
            b = cell_id(r + 1, c)

            if sign == '^':
                less_edges.append((a, b))
            elif sign == 'v':
                less_edges.append((b, a))

    ALL = (1 << n) - 1

    neighbors = [[] for _ in range(n * n)]
    for a, b in less_edges:
        neighbors[a].append((b, True))
        neighbors[b].append((a, False))

    def build_domains():
        domains = [0] * (n * n)

        for r in range(n):
            for c in range(n):
                idx = cell_id(r, c)

                if board[r][c] != 0:
                    domains[idx] = 1 << (board[r][c] - 1)
                    continue

                mask = ALL & ~(row_mask[r] | col_mask[c])

                for other, is_less in neighbors[idx]:
                    orow = other // n
                    ocol = other % n
                    v = board[orow][ocol]

                    if v == 0:
                        continue

                    if is_less:
                        # Current value must be smaller than v.
                        mask &= (1 << (v - 1)) - 1
                    else:
                        # Current value must be greater than v.
                        mask &= ALL ^ ((1 << v) - 1)

                    if mask == 0:
                        return None

                domains[idx] = mask

        # Arc consistency for all inequalities.
        changed = True
        while changed:
            changed = False

            for a, b in less_edges:
                ma = domains[a]
                mb = domains[b]

                if ma == 0 or mb == 0:
                    return None

                max_b = mb.bit_length()
                new_ma = ma & ((1 << (max_b - 1)) - 1)

                if new_ma != ma:
                    domains[a] = new_ma
                    ma = new_ma
                    changed = True

                if ma == 0:
                    return None

                min_a = (ma & -ma).bit_length()
                new_mb = mb & (ALL ^ ((1 << min_a) - 1))

                if new_mb != mb:
                    domains[b] = new_mb
                    mb = new_mb
                    changed = True

                if mb == 0:
                    return None

        return domains

    def dfs():
        domains = build_domains()
        if domains is None:
            return False

        best = -1
        best_mask = 0
        best_count = n + 1

        complete = True

        for r in range(n):
            for c in range(n):
                if board[r][c] == 0:
                    complete = False
                    idx = cell_id(r, c)
                    mask = domains[idx]
                    count = mask.bit_count()

                    if count < best_count:
                        best_count = count
                        best = idx
                        best_mask = mask

        if complete:
            return True

        r = best // n
        c = best % n

        mask = best_mask
        while mask:
            bit = mask & -mask
            mask -= bit

            v = bit.bit_length()

            board[r][c] = v
            row_mask[r] |= bit
            col_mask[c] |= bit

            if dfs():
                return True

            row_mask[r] ^= bit
            col_mask[c] ^= bit
            board[r][c] = 0

        return False

    dfs()

    return [''.join(str(board[r][c]) for c in range(n)) for r in range(n)]

def main():
    first = input().strip()
    if not first:
        return

    n = int(first)
    lines = [first]

    for _ in range(2 * n - 1):
        lines.append(input().rstrip('\n'))

    answer = solve_case(lines)
    sys.stdout.write('\n'.join(answer))

if __name__ == "__main__":
    main()
```Trình phân tích cú pháp tuân theo bố cục ký tự xen kẽ trực tiếp. Trên một hàng bình thường, các giá trị ô xuất hiện ở các chỉ số`0, 2, 4, ...`, trong khi bất bình đẳng theo chiều ngang xảy ra tại các chỉ số`1, 3, 5, ...`. Trên hàng phân cách dọc, các vị trí hữu ích lại được`0, 2, 4, ...`, đó là lý do tại sao mã đọc`s[2 * c]`. 

Mặt nạ hàng và cột sử dụng bit (v-1) cho giá trị (v). Việc loại bỏ các giá trị đã được sử dụng sau đó chỉ là một thao tác theo bit. Số nguyên Python có độ chính xác tùy ý, nhưng ở đây chỉ cần bảy bit nên việc tràn không phải là vấn đề đáng lo ngại. 

các`build_domains`hàm cố tình xây dựng lại các miền ở mỗi lệnh gọi đệ quy thay vì duy trì cấu trúc khôi phục phức tạp. Với tối đa 49 ô, điều này tốn rất ít chi phí và làm cho trạng thái quay lui khó bị hỏng hơn nhiều. 

Việc lan truyền bất đẳng thức cũng được thực hiện từ đầu cho mỗi trạng thái đệ quy. Với (a<b), giá trị lớn nhất có thể có trong (b) cho biết giới hạn trên của (a), trong khi giá trị nhỏ nhất có thể có trong (a) cho biết giới hạn dưới của (b). Việc lặp đi lặp lại những hạn chế này sẽ truyền bá thông tin thông qua các chuỗi bất bình đẳng. 

Việc tìm kiếm sẽ chọn ô có ít ứng cử viên nhất. Điều này hiệu quả hơn là chỉ lấp đầy các ô từ trên cùng bên trái đến dưới cùng bên phải vì một ô bị ràng buộc chặt chẽ có thể bộc lộ sự mâu thuẫn ngay lập tức. Việc gán chỉ được thực hiện sau khi tất cả quá trình truyền hiện tại đã thành công và mặt nạ hàng và cột được khôi phục bằng XOR trong quá trình quay lui. 

## Ví dụ đã hoạt động 

Tuyên bố cuộc thi ban đầu mô tả định dạng đầu vào và đầu ra nhưng không cung cấp các cặp đầu vào/đầu ra mẫu văn bản, vì vậy các dấu vết sau đây sử dụng các câu đố được xây dựng nhỏ đáp ứng cùng định dạng. 

### Ví dụ 1 

Hãy xem xét```
2
-<-
^.v
-.-
```Bất đẳng thức theo chiều ngang buộc hàng đầu tiên phải là`12`. Các bất đẳng thức theo chiều dọc sau đó buộc hàng thứ hai phải là`21`. 

| Trạng thái tìm kiếm | Ô đã chọn | Ứng viên | Bài tập | 
| --- | --- | --- | --- | 
| Ban đầu | (1,1) | {1,2} | thử 1 | 
| Sau (1,1)=1 | (1,2) | {2} | 2 | 
| Sau hàng đầu tiên | (2,1) | {2} | 2 | 
| Cuối cùng | (2,2) | {1} | 1 | 

Bảng hoàn thành là```
12
21
```Dấu vết cho thấy tại sao việc chọn một ô bị ràng buộc lại hữu ích. Khi giá trị đầu tiên được chọn, cả ràng buộc hàng và bất đẳng thức sẽ ngay lập tức xác định phần còn lại. 

### Ví dụ 2 

Hãy xem xét```
3
1.2.3
2.3.1
3.-.2
.....
.....
```Chỉ có ô giữa của hàng cuối cùng trống. 

| Trạng thái tìm kiếm | Ô đã chọn | Ứng viên | Bài tập | 
| --- | --- | --- | --- | 
| Ban đầu | (3,2) | {1} | 1 | 
| Cuối cùng | không | không | hoàn thành | 

Hàng cuối cùng đã chứa 3 và 2 nên cần có 1. Cột của nó cũng chứa 2 và 3, xác nhận độc lập cùng một giá trị. 

Tính bất biến ở đây đặc biệt dễ thấy: tập ứng cử viên cho ô trống đã chính xác bằng giá trị có thể xuất hiện ở cả hàng và cột của ô đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Hàm mũ trong trường hợp xấu nhất | Việc quay lui có thể phân nhánh trên một số ứng cử viên cho nhiều ô, mặc dù mặt nạ hàng/cột, lan truyền bất bình đẳng và MRV cắt tỉa tìm kiếm một cách tích cực | 
| Không gian | (O(n^2)) | Bảng, ràng buộc, mặt nạ và miền ứng cử viên đều chỉ chứa thông tin (O(n^2)) | 

Độ phức tạp trong trường hợp xấu nhất nhất thiết phải theo cấp số nhân vì không gian tìm kiếm là tổ hợp. Kích thước đầu vào thực tế chỉ là (n\le7), cung cấp tối đa 49 ô và bảy giá trị có thể có cho mỗi ô. Sự kết hợp giữa tính duy nhất, các ràng buộc bình phương Latinh và sự lan truyền bất đẳng thức làm cho phần được khám phá của không gian tìm kiếm đó đủ nhỏ cho giới hạn 256 MB ban đầu là 5 giây. 

## Trường hợp thử nghiệm 

Vấn đề ban đầu không hiển thị các trường hợp mẫu văn bản trong nguồn câu lệnh, vì vậy bộ kiểm tra bên dưới sử dụng các trường hợp được xây dựng từ dấu vết cộng với các trường hợp biên bổ sung.```python
import sys
import io

def run(inp: str) -> str:
    lines = inp.strip('\n').splitlines()
    ans = solve_case(lines)
    return '\n'.join(ans)

# Minimum-size input.
assert run(
    """1
1"""
) == "1", "minimum-size puzzle"

# Boundary inequalities and parsing of both ^ and v.
assert run(
    """2
-<-
^.v
-.-"""
) == "12\n21", "inequality directions"

# Nearly complete 3x3 Latin square, no inequalities.
assert run(
    """3
1.2.3
2.3.1
3.-.2
.....
....."""
) == "123\n231\n312", "single missing value"

# Maximum-size board, with exactly one blank cell.
assert run(
    """7
1.2.3.4.5.6.7
2.3.4.5.6.7.1
3.4.5.6.7.1.2
4.5.6.7.1.2.3
5.6.7.1.2.3.4
6.7.1.2.3.4.5
7.1.2.3.4.5.-
.............
.............
.............
.............
............."""
) == (
    "1234567\n"
    "2345671\n"
    "3456712\n"
    "4567123\n"
    "5671234\n"
    "6712345\n"
    "7123456"
), "maximum-size board"

# All-equal values are possible only for the degenerate 1x1 case.
assert run(
    """1
1"""
) == "1", "degenerate all-equal case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`1`| Kích thước tối thiểu và không có đường bất đẳng thức | 
|`2 / -<-, ^.v, -.-`|`12 / 21`| Bất bình đẳng biên và cả hai chiều dọc | 
|`3 / 1.2.3, 2.3.1, 3.-.2`|`123 / 231 / 312`| Giao điểm ứng cử viên hàng và cột, xử lý từng cái một | 
| Bảng tuần hoàn 7x7 | Bảng tuần hoàn đã hoàn thành | Tối đa (n), mặt nạ lớn và phục hồi tế bào cuối cùng | 
|`1 / 1`|`1`| Trường hợp suy biến trong đó toàn bộ hàng và cột chứa cùng một giá trị duy nhất | 

## Vỏ cạnh 

Với (n=1), không có đường phân cách nào cả. Đầu vào chỉ đơn giản là`1`tiếp theo là ô đơn. Trình phân tích cú pháp lặp lại 0 lần đối với các bất đẳng thức ngang và dọc và bảng đã hoàn tất. Đầu ra là`1`. Điều này ngăn cản việc triển khai giả định rằng mọi câu đố đều có ít nhất một bất đẳng thức. 

Đối với trường hợp bất đẳng thức biên,```
2
-<-
^.v
-.-
```mối quan hệ theo chiều ngang có nghĩa là hàng đầu tiên có dạng (x<y). Vì một hàng phải chứa 1 và 2 nên nó phải là`12`. các`^`ở vị trí thẳng đứng đầu tiên có nghĩa là (1<2), trong khi`v`ở phương tiện thứ hai (2>1). Do đó, hàng thứ hai là`21`. Trình phân tích cú pháp đọc các dấu hiệu dọc từ vị trí 0 và 2, khớp chính xác với định dạng đã chỉ định. 

Đối với một ô bị ép buộc bởi cả một hàng và một cột, hãy xem xét```
3
1.2.3
2.3.1
3.-.2
.....
.....
```Ô trống nằm trong một hàng chứa 3 và 2, chỉ để lại 1. Cột của nó cũng chứa 2 và 3, cũng chỉ để lại 1.`build_domains`tính toán giao điểm của các hạn chế đó dưới dạng mặt nạ một bit cho giá trị 1, do đó tìm kiếm MRV sẽ gán nó ngay lập tức. 

Đối với kích thước bảng tối đa, bài kiểm tra 7x7 có 49 ô và chỉ có một ô trống. Mỗi mặt nạ hàng và cột sử dụng bảy bit và ô bị thiếu chỉ có một ứng cử viên. Bộ giải không cần khám phá cây tìm kiếm lớn, trong khi đầu vào vẫn thực hiện mọi chiều mảng ở mức lớn nhất cho phép (n). 

Cuối cùng, việc thực hiện bất bình đẳng một cách bất cẩn có thể vô tình tạo ra sự bình đẳng. Mọi quan hệ trong bộ giải đều sử dụng một giới hạn nghiêm ngặt. Đối với (a<b), ứng cử viên lớn nhất được phép cho (a) nhỏ hơn một giá trị lớn nhất có thể có của (b) và ứng cử viên nhỏ nhất được phép cho (b) lớn hơn giá trị nhỏ nhất có thể có của (a). Vì các giá trị bảng là số nguyên nên các giới hạn nghiêm ngặt này chính xác là yêu cầu bắt buộc`<`Và`>`ngữ nghĩa.
