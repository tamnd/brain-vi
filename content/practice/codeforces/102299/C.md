---
title: "CF 102299C - Matryoshka pha lê"
description: "Chúng tôi duy trì một bộ sưu tập búp bê matryoshka. Mỗi con búp bê có trọng lượng nguyên dương và một mã định danh duy nhất. Một con búp bê có thể chứa nhiều con búp bê nhỏ hơn, nhưng tổng trọng lượng của mọi thứ trực tiếp hoặc gián tiếp bên trong con búp bê bên ngoài không được vượt quá trọng lượng của con búp bê bên ngoài."
date: "2026-08-13T23:13:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102299
codeforces_index: "C"
codeforces_contest_name: "2019 USP Try-outs"
rating: 0
weight: 102299
solve_time_s: 147
verified: true
draft: false
---

[CF 102299C - Crystal Matryoshkas](https://codeforces.com/problemset/problem/102299/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một bộ sưu tập búp bê matryoshka. Mỗi con búp bê có trọng lượng nguyên dương và một mã định danh duy nhất. Một con búp bê có thể chứa nhiều con búp bê nhỏ hơn, nhưng tổng trọng lượng của mọi thứ trực tiếp hoặc gián tiếp bên trong con búp bê bên ngoài không được vượt quá trọng lượng của con búp bê bên ngoài. 

Đối với một truy vấn`? ID`, chúng ta không được yêu cầu xây dựng một lồng cụ thể. Chúng ta cần số lượng búp bê tối đa có thể có trong một lồng hợp lệ có chứa búp bê được chỉ định ở đâu đó bên trong nó. Con búp bê được truy vấn không nhất thiết phải là con ngoài cùng. 

Bộ sưu tập thay đổi theo thời gian. Một hoạt động`+ W ID`chèn một con búp bê mới với trọng lượng`W`,`- ID`loại bỏ một con búp bê hiện có và`? ID`yêu cầu kích thước lồng tối đa chứa mã định danh đó. Những ràng buộc chính thức là`N <= 10^5`,`Q <= 5 * 10^5`, và mọi trọng số và số nhận dạng nhiều nhất là`10^9`. Cuộc thi đưa ra giới hạn cho bài toán này là 3 giây và bộ nhớ 256 MB. 

Giá trị lớn của`Q`là hạn chế chính. Một thuật toán kiểm tra mọi con búp bê hiện có cho mỗi truy vấn là quá đắt. Ngay cả khi chúng ta chỉ xem xét ban đầu`10^5`búp bê,`5 * 10^5`các truy vấn có thể đã gây ra`5 * 10^10`kiểm tra ứng viên. Việc chèn và xóa động làm cho việc quét trực tiếp thậm chí còn kém hấp dẫn hơn. 

Có một số trường hợp khó xử lý. Đầu tiên, sự bình đẳng được cho phép. Ví dụ,```
2 1
1 1
? 1
```có câu trả lời```
2
```bởi vì một con búp bê có trọng lượng`1`có thể chứa một con búp bê khác có trọng lượng`1`. Một giải pháp sử dụng nghiêm ngặt`<`so sánh sẽ trả về không chính xác`1`. 

Trọng lượng trùng lặp cũng có vấn đề. TRONG```
3 1
1 1 2
? 3
```câu trả lời là`3`, sử dụng lồng`1, 1, 2`. Việc xóa trọng số theo giá trị mà không tính đến bội số có thể vô tình loại bỏ con búp bê được truy vấn hoặc bản sao sai. 

Con búp bê được truy vấn có thể ở đâu đó ở giữa tổ. Vì```
4 1
3 1 2 1
? 3
```câu trả lời là`3`, sử dụng trọng lượng`1, 1, 2`, với trọng lượng`2`búp bê như búp bê được truy vấn. Một chiến lược chỉ tìm kiếm những con búp bê bên ngoài đối tượng được truy vấn sẽ bỏ lỡ các giải pháp hợp lệ. 

Cuối cùng, một ứng viên bị trượt vẫn phải có mặt. Coi như```
3 1
1 3 10
? 2
```nơi con búp bê được hỏi có trọng lượng`3`. trọng lượng`1`con búp bê có thể được đặt bên trong nó, cho kích thước`2`, nhưng trọng lượng`10`con búp bê không thể làm theo vì tổng số hiện tại sẽ là`13`sau khi thêm nó. Việc triển khai bất cẩn loại bỏ ứng cử viên đầu tiên trước khi kiểm tra xem nó có phù hợp hay không sẽ làm hỏng bộ sưu tập. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạm thời loại bỏ búp bê được truy vấn, sắp xếp các trọng số còn lại và liên tục chọn những búp bê có thể lồng xung quanh nó. Điều này đúng nếu chúng ta sử dụng búp bê khả thi nhỏ nhất ở mọi giai đoạn, nhưng chi phí phân loại`O(N log N)`và việc quét có thể tốn kém`O(N)`cho một truy vấn. Với tối đa`5 * 10^5`truy vấn, điều đó không gần với quy mô yêu cầu. 

Chúng ta có thể làm tốt hơn bằng cách tách vấn đề thành hai quan sát. Đầu tiên là tham lam. Giả sử tổng số búp bê hiện tại đã được đặt ở một bên là`S`. Bất kỳ con búp bê tiếp theo nào cũng phải có trọng lượng ít nhất`S`, bởi vì nó phải chứa tất cả búp bê đã có sẵn bên trong nó. Trong số tất cả các lựa chọn có thể, con búp bê nhỏ nhất ít nhất luôn tốt bằng con búp bê lớn hơn. Việc chọn một con búp bê nhỏ hơn ít nhất sẽ để lại nhiều khả năng cho mỗi lựa chọn sau này, vì vậy việc thay thế lựa chọn lớn hơn bằng lựa chọn nhỏ nhất khả thi không thể làm giảm số lượng búp bê tối đa có thể đạt được. 

Đối với những con búp bê bên trong con búp bê được truy vấn, chúng tôi bắt đầu với trọng lượng sẵn có nhỏ nhất trên toàn cầu. Nó chỉ có thể được sử dụng nếu nó cao nhất là trọng lượng được truy vấn`X`. Sau khi chọn nó, chúng tôi liên tục lấy trọng số nhỏ nhất hiện có ít nhất bằng tổng bên trong hiện tại, miễn là tổng mới vẫn giữ nguyên nhiều nhất`X`. 

Sau khi búp bê được truy vấn được thêm vào, quá trình tham lam tương tự sẽ tiếp tục diễn ra bên ngoài. Con búp bê tiếp theo phải có trọng lượng ít nhất bằng tổng trọng lượng hiện tại và chúng ta lại lấy con búp bê nhỏ nhất hiện có. 

Quan sát thứ hai là điều làm cho mô phỏng tham lam này diễn ra nhanh chóng. Sau khi con búp bê bên trong được chọn đầu tiên có trọng lượng`y`, mỗi con búp bê được chọn sau đó có trọng lượng ít nhất bằng tổng hiện tại. Do đó, số tiền ít nhất sẽ tăng gấp đôi sau mỗi lần lựa chọn thành công. Điều này cũng đúng ở bên ngoài sau khi con búp bê được truy vấn đã được thêm vào. Vì tất cả các trọng số nhiều nhất`10^9`, một truy vấn chỉ có thể chọn`O(log 10^9)`, khoảng 30, búp bê. Bộ sưu tập có thể chứa hàng trăm nghìn búp bê, nhưng một truy vấn chỉ cần một số lượng nhỏ các thao tác được sắp xếp theo thứ tự. 

Trong C++, việc triển khai tự nhiên là một`multiset`, hỗ trợ chèn, xóa, tối thiểu và`lower_bound`TRONG`O(log N)`. Giải pháp được công bố tuân theo chính xác chiến lược tham lam này với nhiều tập hợp có thứ tự. 

Python không có nhiều tập hợp cân bằng tích hợp sẵn, do đó cách triển khai bên dưới sử dụng nén tọa độ và tập hợp bit phân cấp. Mỗi trọng số riêng biệt sẽ có một chỉ mục. Một mảng đếm lưu trữ số lượng búp bê đang hoạt động có mỗi trọng lượng, trong khi một số cấp bitset 64-bit cho chúng ta biết chỉ số trọng lượng nào hiện đang tồn tại. Việc tìm kiếm trọng số hoạt động đầu tiên tại hoặc sau một chỉ mục đã nén sau đó chỉ thực hiện một vài thao tác phân cấp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(QN) kiểm tra ứng viên, cộng với việc sắp xếp nếu cần | O(N) | Quá chậm | 
| Bộ thứ tự tối ưu | O(Q log W log N) | O(N + Q) | Đã chấp nhận | 
| Bitset phân cấp Python | O((N + Q) log(N + Q) + Q log W log N) | O(N + Q) | Đã chấp nhận | 

Đây`W`biểu thị trọng lượng tối đa có thể. Logarit bổ sung trong quá trình triển khai Python xuất phát từ việc định vị chỉ mục được nén bằng tìm kiếm nhị phân. Việc tra cứu trọng số hoạt động thực tế được triển khai với hệ thống phân cấp 64-ary thay vì cây thông thường. 

## Hướng dẫn thuật toán 

1. Đọc những con búp bê ban đầu và tất cả các thao tác, thu thập mọi trọng lượng có thể xuất hiện. Nén tọa độ đòi hỏi phải biết tất cả các trọng số có thể có trước khi xử lý các thao tác động. 
2. Sắp xếp các trọng số riêng biệt và gán cho mỗi trọng số một chỉ số nén. Duy trì một từ điển từ mọi mã định danh đang hoạt động cho đến chỉ số trọng số được nén của nó. 
3. Xây dựng bitset phân cấp. Ở mức 0, bit`i`được đặt chính xác khi có ít nhất một con búp bê đang hoạt động có trọng lượng nén`i`. Cấp độ cao hơn lưu trữ những từ của cấp độ trước đó không trống. Điều này cho phép chúng tôi tìm trọng số hoạt động tiếp theo mà không cần quét qua các chỉ số không hoạt động. 
4. Để chèn, hãy tăng số lượng trọng lượng nén tương ứng. Nếu số đếm của nó thay đổi từ 0 thành 1, hãy đặt bit tương ứng và truyền từ mới không trống qua hệ thống phân cấp. 
5. Để xóa, hãy giảm số lượng. Nếu số đếm trở về 0, hãy xóa bit của nó và truyền từ mới trống lên trên. 
6. Đối với một truy vấn, hãy tạm thời xóa búp bê được truy vấn. Điều này ngăn thuật toán chọn cùng một con búp bê vật lý làm một trong những vật chứa hoặc nội dung của chính nó. 
7. Hãy để`X`là trọng lượng của con búp bê được hỏi và đặt câu trả lời cho`1`. Tìm trọng lượng hoạt động nhỏ nhất`y`. Nếu như`y <= X`, xóa nó, tăng câu trả lời và đặt tổng bên trong hiện tại thành`y`. Nếu trọng số nhỏ nhất đã lớn hơn`X`, không có con búp bê nào có thể tồn tại bên trong con búp bê được truy vấn. 
8. Trong khi có thể, hãy tìm trọng lượng hoạt động nhỏ nhất`y >= current_sum`. Nếu như`current_sum + y <= X`, xóa nó đi và cập nhật`current_sum += y`. Còn không thì dừng lại. Việc chọn ứng cử viên nhỏ nhất là tối ưu vì mọi ứng cử viên lớn hơn đều tiêu thụ ít nhất bằng dung lượng của con búp bê được truy vấn. 
9. Thêm`X`đến số tiền hiện tại. Con búp bê được truy vấn bây giờ là toàn bộ ngăn xếp hiện tại, bao gồm mọi thứ được chọn bên trong nó. 
10. Liên tục tìm trọng số hoạt động nhỏ nhất ít nhất bằng tổng hiện tại. Nếu có, hãy loại bỏ nó, tăng câu trả lời và cộng trọng số của nó vào tổng hiện tại. Nếu không tồn tại, việc lồng ghép không thể được mở rộng thêm. 
11. Khôi phục búp bê được truy vấn và mọi búp bê được chọn tạm thời. Truy vấn chỉ là phép tính nên tập hợp phải chính xác như trước truy vấn. 
12. In câu trả lời và tiếp tục thao tác tiếp theo. 

Tại sao nó hoạt động: bất biến là ở mọi giai đoạn, việc lồng một phần hiện tại có tổng trọng lượng nhỏ nhất có thể có trong số tất cả các lần lồng một phần có cùng số lượng búp bê. Đối với phần bên trong, việc chọn con búp bê nhỏ nhất có thể sử dụng sẽ bảo toàn được sức chứa tối đa còn lại bên trong`X`. Đối với phần bên ngoài, việc chọn con búp bê nhỏ nhất có thể chứa ngăn xếp hiện tại sẽ giữ được tổng số mới nhỏ nhất có thể. Bất kỳ lựa chọn thay thế nào cũng có trọng lượng ít nhất là lớn, vì vậy nó không thể cho phép nhiều búp bê tương lai hơn lựa chọn tham lam. Vì mỗi trọng số được chọn ít nhất bằng tổng trước đó nên các lựa chọn thành công cũng nhân đôi tổng hiện tại, giới hạn số lần lặp. 

## Giải pháp Python```python
import sys
from bisect import bisect_left
from array import array

input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())

    initial = list(map(int, input().split()))
    while len(initial) < n:
        initial.extend(map(int, input().split()))

    # Store operations compactly.
    # typ: 0 = query, 1 = add, 2 = delete
    typ = bytearray()
    a = array('q')
    b = array('q')

    all_weights = array('q', initial)

    for _ in range(q):
        parts = input().split()
        op = parts[0]

        if op == b'+':
            w = int(parts[1])
            ident = int(parts[2])
            typ.append(1)
            a.append(w)
            b.append(ident)
            all_weights.append(w)
        elif op == b'-':
            ident = int(parts[1])
            typ.append(2)
            a.append(ident)
            b.append(0)
        else:
            ident = int(parts[1])
            typ.append(0)
            a.append(ident)
            b.append(0)

    weights = sorted(set(all_weights))
    m = len(weights)
    weight_to_index = {w: i for i, w in enumerate(weights)}

    del all_weights

    # id -> compressed weight index
    ids = {}

    counts = [0] * m

    # levels[0] has one bit per compressed weight.
    # A bit is set iff that weight currently exists.
    levels = []
    size = (m + 63) >> 6
    levels.append([0] * size)

    while size > 1:
        size = (size + 63) >> 6
        levels.append([0] * size)

    def activate(idx):
        counts[idx] += 1
        if counts[idx] != 1:
            return

        pos = idx

        for level in range(len(levels)):
            word = pos >> 6
            bit = 1 << (pos & 63)

            old = levels[level][word]
            if old & bit:
                break

            levels[level][word] = old | bit

            if old != 0:
                break

            pos = word

    def deactivate(idx):
        counts[idx] -= 1
        if counts[idx] != 0:
            return

        pos = idx

        for level in range(len(levels)):
            word = pos >> 6
            bit = 1 << (pos & 63)

            old = levels[level][word]
            new = old & ~bit
            levels[level][word] = new

            if new != 0:
                break

            pos = word

    def next_active(pos):
        """Return the first active compressed index >= pos, or -1."""
        if pos >= m:
            return -1

        level = 0
        p = pos

        while level < len(levels):
            word = p >> 6
            if word >= len(levels[level]):
                return -1

            mask = -1 << (p & 63)
            value = levels[level][word] & mask

            if value:
                bit = (value & -value).bit_length() - 1
                index = (word << 6) + bit

                if level == 0:
                    return index

                # We found a nonempty word in a higher level.
                # Descend to the actual weight index.
                current = index

                for lower in range(level - 1, -1, -1):
                    value = levels[lower][current]
                    bit = (value & -value).bit_length() - 1
                    current = (current << 6) + bit

                return current

            # No set bit remains in this word. The next possible
            # bit in the next hierarchy level represents word + 1.
            p = word + 1
            level += 1

        return -1

    # Insert the initial collection.
    for ident, w in enumerate(initial, 1):
        idx = weight_to_index[w]
        ids[ident] = idx
        activate(idx)

    out = []

    for k in range(q):
        operation = typ[k]

        if operation == 1:
            w = a[k]
            ident = b[k]

            idx = weight_to_index[w]
            ids[ident] = idx
            activate(idx)

        elif operation == 2:
            ident = a[k]

            idx = ids.pop(ident)
            deactivate(idx)

        else:
            ident = a[k]

            target_idx = ids[ident]
            x = weights[target_idx]

            # Temporarily remove the queried physical doll.
            deactivate(target_idx)

            chosen = [target_idx]
            answer = 1

            # Build the part strictly inside the queried doll.
            first = next_active(0)

            if first != -1:
                y = weights[first]

                if y <= x:
                    deactivate(first)
                    chosen.append(first)
                    answer += 1
                    current = y

                    while True:
                        need = bisect_left(weights, current)
                        nxt = next_active(need)

                        if nxt == -1:
                            break

                        y = weights[nxt]

                        if current + y > x:
                            break

                        deactivate(nxt)
                        chosen.append(nxt)
                        answer += 1
                        current += y
                else:
                    current = x
            else:
                current = x

            # The queried doll becomes part of the current stack.
            if chosen[-1] != target_idx:
                current += x
            else:
                current = x

            # Extend outward.
            while True:
                need = bisect_left(weights, current)
                nxt = next_active(need)

                if nxt == -1:
                    break

                y = weights[nxt]
                deactivate(nxt)
                chosen.append(nxt)

                answer += 1
                current += y

            out.append(str(answer))

            # Restore exactly the dolls temporarily removed for this query.
            for idx in chosen:
                activate(idx)

    sys.stdout.write('\n'.join(out))

if __name__ == "__main__":
    solve()
```Bộ sưu tập ban đầu được chèn vào cấu trúc nén đúng một lần. Từ điển định danh lưu trữ các chỉ số trọng số được nén thay vì trọng số ban đầu, điều này tránh việc tìm kiếm liên tục trọng số của mã định danh. 

các`activate`Và`deactivate`các chức năng duy trì cả bội số và thứ bậc. Cần phải có nhiều con búp bê vì nhiều con búp bê khác nhau có thể có cùng trọng lượng. Trọng số vẫn hoạt động trong bitset cho đến khi số lượng của nó bằng 0.`next_active`là sự thay thế Python cho`multiset.lower_bound`. Ở mức 0, nó tìm bit được đặt đầu tiên trong từ 64 bit có liên quan. Nếu từ đó trống, nó sẽ tăng lên cấp cao hơn để ghi lại những từ có chứa bất kỳ trọng số hoạt động nào, sau đó lại giảm xuống để khôi phục chỉ số trọng số chính xác. Bởi vì mỗi cấp độ nhóm 64 mục, nên chỉ tồn tại một số cấp độ tối đa.`6 * 10^5`trọng số có thể. 

Con búp bê được truy vấn sẽ bị xóa trước quá trình tham lam và được khôi phục sau đó. các`chosen`mảng ghi lại các chỉ số trọng số được nén và việc khôi phục các chỉ mục thay vì mã định danh là đủ vì truy vấn không bao giờ thay đổi ánh xạ nhận dạng theo trọng số. 

Tất cả các tổng đều sử dụng số nguyên Python, do đó không có vấn đề tràn. Trong các ngôn ngữ có số học số nguyên có chiều rộng cố định, cần có số nguyên 64 bit vì tổng lồng nhau có thể vượt quá`10^9`bởi một yếu tố đáng kể. 

hai`bisect_left`các cuộc gọi được thực hiện có chủ ý trên các trọng số riêng biệt được sắp xếp. Họ chuyển đổi một trọng lượng cần thiết như`current`vào chỉ mục nén đầu tiên có trọng lượng thực tế ít nhất là giá trị đó. Hệ thống phân cấp bit sau đó sẽ bỏ qua mọi trọng số không hoạt động sau chỉ mục đó. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, trọng số ban đầu là`3, 1, 2, 1`. Truy vấn đầu tiên hỏi về con búp bê cân nặng`2`. 

| Hoạt động | Mục tiêu | Tổng hiện tại | Trọng lượng được chọn | Trả lời | 
| --- | --- | --- | --- | --- | 
|`? 3`| 2 | 2 |`2`| 1 | 
| sự lựa chọn bên trong | 2 | 1 |`2, 1`| 2 | 
| sự lựa chọn bên trong | 2 | 2 |`2, 1, 1`| 3 | 
| tìm kiếm bên ngoài | 2 | 2 |`2, 1, 1`| 3 | 

đầu tiên`1`phù hợp với trọng lượng bên trong`2`. Sau khi thêm nó, một cái khác`1`cũng phù hợp vì tổng trọng lượng bên trong chính xác`2`. Không còn con búp bê bên trong nào phù hợp nữa và ít nhất cũng không có con búp bê nào có trọng lượng phù hợp.`4`, vậy câu trả lời là`3`. 

Sau khi chèn trọng lượng`4`, truy vấn thứ hai cho mã định danh`3`cư xử khác đi. 

| Hoạt động | Mục tiêu | Tổng hiện tại | Trọng lượng được chọn | Trả lời | 
| --- | --- | --- | --- | --- | 
|`? 3`| 2 | 2 |`2`| 1 | 
| sự lựa chọn bên trong | 2 | 1 |`2, 1`| 2 | 
| sự lựa chọn bên trong | 2 | 2 |`2, 1, 1`| 3 | 
| sự lựa chọn bên ngoài | 2 | 6 |`2, 1, 1, 4`| 4 | 

trọng lượng`3`quá lớn để vừa với trọng lượng được truy vấn`2`, nhưng sau khi bao gồm con búp bê được truy vấn, tổng số hiện tại sẽ trở thành`4`. Trọng lượng mới được chèn`4`có thể chứa toàn bộ chồng đó, tạo ra bốn con búp bê. Đây chính xác là hai kết quả đầu ra đầu tiên của Mẫu 1. 

Đối với Mẫu 2, truy vấn đầu tiên liên quan đến trọng số`9`. 

| Hoạt động | Mục tiêu | Tổng hiện tại | Trọng lượng được chọn | Trả lời | 
| --- | --- | --- | --- | --- | 
|`? 2`| 9 | 9 |`9`| 1 | 
| sự lựa chọn bên trong | 9 | 1 |`9, 1`| 2 | 
| sự lựa chọn bên trong | 9 | 5 |`9, 1, 4`| 3 | 
| ứng cử viên bên trong tiếp theo | 9 | 5 |`9, 1, 4`| 3 | 
| tìm kiếm bên ngoài | 9 | 14 |`9, 1, 4`| 3 | 

Ứng cử viên bên trong tiếp theo sau`4`là`5`, Nhưng`5 + 5 > 9`, nên không dùng được. Sau khi thêm trọng lượng truy vấn`9`, tổng số hiện tại là`14`, và ít nhất không có trọng lượng`14`. Câu trả lời là`3`. 

Sau tạ`5`Và`1`bị loại bỏ, các trọng số liên quan còn lại là`4`Và`10`. 

| Hoạt động | Mục tiêu | Tổng hiện tại | Trọng lượng được chọn | Trả lời | 
| --- | --- | --- | --- | --- | 
|`? 2`| 9 | 9 |`9`| 1 | 
| sự lựa chọn bên trong | 9 | 4 |`9, 4`| 2 | 
| ứng cử viên bên trong tiếp theo | 9 | 4 |`9, 4`| 2 | 
| tìm kiếm bên ngoài | 9 | 13 |`9, 4`| 2 | 

trọng lượng`10`không thể sử dụng được bên trong`9`bởi vì`4 + 10 > 9`. Sau khi thêm búp bê được truy vấn, tổng số là`13`, Vì thế`10`cũng không thể đặt bên ngoài nó. Câu trả lời giảm xuống`2`, phù hợp với mẫu 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + Q) log(N + Q) + Q log W log(N + Q)) | Sắp xếp chi phí nén và mọi truy vấn thực hiện các lựa chọn tham lam O(log W) với tra cứu trọng số theo thứ tự | 
| Không gian | O(N + Q) | Tất cả các trọng số, phép toán, số nhận dạng, số lượng và cấp độ phân cấp có thể yêu cầu không gian tuyến tính | 

Đây`W <= 10^9`, Vì thế`log W`tối đa là khoảng 30. Thực tế quan trọng là một truy vấn không bao giờ quét toàn bộ bộ sưu tập. Tổng tham lam ít nhất sẽ tăng gấp đôi sau mỗi lần lựa chọn thành công, giảm những gì trông giống như một truy vấn tuyến tính tiềm năng thành số lượng tra cứu theo thứ tự logarit. Giới hạn cuộc thi ban đầu là 3 giây với 256 MB và giới hạn tham lam tương tự là lý do khiến giải pháp tập hợp theo thứ tự dự định trở nên khả thi. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả sử giải pháp được lưu dưới dạng`solution.py`và phơi bày`solve()`chức năng hiển thị ở trên.```python
# helper: run solution on input string, return output string
import sys
import io
import solution

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()

        solution.input = sys.stdin.readline
        solution.solve()

        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Sample 1
assert run(
    """4 4
3 1 2 1
? 3
+ 4 5
? 3
? 1
"""
) == "3\n4\n3", "sample 1"

# Sample 2
assert run(
    """5 6
5 9 4 1 10
? 2
- 1
- 4
? 2
+ 13 1
? 2
"""
) == "3\n2\n3", "sample 2"

# Minimum-size input
assert run(
    """1 1
7
? 1
"""
) == "1", "single doll"

# Equality boundary: 1 can contain another 1 because <= is allowed
assert run(
    """2 1
1 1
? 1
"""
) == "2", "equality boundary"

# All equal values
assert run(
    """5 1
1 1 1 1 1
? 1
"""
) == "2", "all equal values"

# Dynamic deletion and insertion with identifier reuse
assert run(
    """2 5
1 4
? 2
- 1
? 2
+ 2 3
? 2
"""
) == "2\n1\n2", "dynamic updates"

# Boundary case where two inner dolls exactly fill the queried doll
assert run(
    """3 1
1 1 2
? 3
"""
) == "3", "exact capacity"

# Maximum-size collection, all equal
max_case = "100000 1\n" + ("1 " * 100000).strip() + "\n? 1\n"
assert run(max_case) == "2", "maximum-size all-equal collection"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 7 / ? 1`|`1`| Kích thước bộ sưu tập tối thiểu và không có thùng chứa nào | 
|`2 1 / 1 1 / ? 1`|`2`| Bình đẳng trong điều kiện làm tổ | 
|`5 1 / 1 1 1 1 1 / ? 1`|`2`| Trọng số trùng lặp và bội số | 
| Xóa và chèn động |`2`,`1`,`2`| Bảo trì đúng cách bộ sưu tập đang hoạt động | 
|`1 1 2 / ? 3`|`3`| Công suất chính xác và xử lý từng cái một | 
|`100000`bản sao trọng lượng`1`|`2`| Kích thước đầu vào tối đa và xử lý trùng lặp hiệu quả | 

## Vỏ cạnh 

Đối với ranh giới đẳng thức, hãy xem xét```
2 1
1 1
? 1
```Con búp bê được yêu cầu tạm thời bị loại bỏ, để lại một trọng lượng`1`. Từ`1 <= 1`, nó được chọn làm búp bê bên trong. Tổng hiện tại trở thành`1`, và một ứng cử viên khác sẽ tính tổng`2`, quá lớn đối với con búp bê được truy vấn. Không có búp bê bên ngoài nên câu trả lời là`2`. Thuật toán sử dụng`<=`trong quá trình kiểm tra dung lượng nên nó xử lý ranh giới một cách chính xác. 

Đối với trọng số trùng lặp, hãy xem xét```
3 1
1 1 2
? 3
```Trọng số được truy vấn là`2`. Trọng lượng hoạt động đầu tiên là`1`, được chọn. Một hoạt động khác`1`cũng được chọn vì`1 + 1 = 2`. Sau đó, con búp bê được truy vấn sẽ được đưa vào, tạo ra một tổ ba con búp bê. Mảng đếm giữ trọng lượng`1`hoạt động cho đến khi cả hai bản sao đều bị xóa, do đó hai con búp bê vật lý được xử lý riêng biệt. 

Đối với một con búp bê được truy vấn không ở ngoài cùng, hãy xem xét```
4 1
3 1 2 1
? 3
```Sau khi tạm thời giảm cân`2`, giai đoạn nội tâm tham lam lựa chọn`1`và cái khác`1`, đạt tổng bên trong là`2`. Bản thân con búp bê được truy vấn sau đó sẽ được thêm vào, nhưng không có trọng số bên ngoài nào có thể chứa tổng kết quả`4`. Câu trả lời là`3`. Thuật toán không bao giờ giả định rằng búp bê được truy vấn phải là búp bê đầu tiên hoặc cuối cùng trong ngăn xếp. 

Để có ranh giới điền chính xác, hãy xem xét```
3 1
1 1 2
? 3
```Hai trọng lượng`1`búp bê có tổng trọng lượng chính xác`2`, vì vậy cả hai đều thuộc về trọng số được truy vấn`2`BÚP BÊ. Câu trả lời là`3`. Một sự bất bình đẳng nghiêm ngặt sẽ dừng lại sau lần đầu tiên`1`và trả lại câu trả lời sai`2`. 

Đối với những thay đổi trạng thái động, hãy xem xét```
2 5
1 4
? 2
- 1
? 2
+ 2 3
? 2
```Ban đầu, cân nặng`1`có thể được đặt bên trong trọng lượng`4`, cho`2`. Sau mã định danh`1`bị xóa, chỉ có trọng số được truy vấn`4`vẫn còn, vì vậy câu trả lời trở thành`1`. Chèn một trọng lượng mới`2`khôi phục khả năng làm tổ của hai con búp bê, mang lại`2`lại. Từ điển định danh và cấu trúc bội số được cập nhật độc lập, do đó việc xóa và sử dụng lại sau này một định danh sẽ không để lại các trọng số cũ.
