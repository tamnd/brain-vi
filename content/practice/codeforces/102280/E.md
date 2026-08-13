---
title: "CF 102280E - \u0428\u0442\u0440\u0430\u0444"
description: "Chúng tôi có một bộ sưu tập tiền giấy riêng lẻ. Mỗi tờ tiền có một mệnh giá và mỗi tờ tiền chỉ được sử dụng tối đa một lần. Cho một số tiền p nhỏ, chúng ta cần chọn một số tờ tiền có sẵn có tổng giá trị càng nhỏ càng tốt trong khi vẫn ít nhất bằng p."
date: "2026-08-13T09:48:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102280
codeforces_index: "E"
codeforces_contest_name: "2010, \u0422\u0440\u0435\u043d\u0438\u0440\u043e\u0432\u043a\u0430 \u0421\u0413\u0410\u0423 aka \u041a\u043e\u043d\u0442\u0435\u0441\u0442 \u043f\u0440\u043e \u043c\u0430\u0440\u0448\u0440\u0443\u0442\u043a\u0438"
rating: 0
weight: 102280
solve_time_s: 189
verified: true
draft: false
---

[CF 102280E - \u0428\u0442\u0440\u0430\u0444](https://codeforces.com/problemset/problem/102280/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập tiền giấy riêng lẻ. Mỗi tờ tiền có một mệnh giá và mỗi tờ tiền chỉ được sử dụng tối đa một lần. Đưa ra một số tiền phạt`p`, chúng ta cần chọn một số tờ tiền có sẵn có tổng giá trị càng nhỏ càng tốt trong khi vẫn ít nhất`p`. 

Đầu ra phải chứa số tiền phải trả tối thiểu đó và mệnh giá thực tế được sử dụng để có được số tiền đó. Nếu không có tập hợp con của tiền giấy có thể đạt tới`p`, câu trả lời là`-1`. 

Các giới hạn định hình giải pháp khá mạnh mẽ. Có nhiều nhất`1000`tiền giấy, trong khi mức phạt cao nhất là`100000`và một mệnh giá có thể lớn như`1000000`. Một chương trình động tổng tập con thông thường với các trạng thái từ`0`ĐẾN`p`và sự chuyển đổi cho mọi chi phí tiền giấy`O(n p)`, có thể đạt tới khoảng`10^8`cập nhật trạng thái. Với giới hạn 1,5 giây, điều đó quá đắt, đặc biệt là trong Python. Chúng ta cần khai thác thực tế rằng tập trạng thái là một tập bit, vì vậy nhiều chuyển đổi tổng tập hợp con có thể được thực hiện đồng thời. 

Giới hạn giáo phái lớn cũng có vấn đề. Chúng ta không thể đơn giản tạo ra phạm vi DP`0..sum(q)`, vì tổng giá trị có thể lớn bằng`10^9`. May mắn thay, số tiền lớn hơn`p`không cần phải được biểu diễn trong giai đoạn tổng hợp con. Tờ tiền cuối cùng có thể là tờ tiền đẩy tổng số tiền vượt quá`p`. 

Có một số trường hợp đặc biệt có thể âm thầm phá vỡ việc triển khai hợp lý. Nếu như`p = 0`, tập trống đã là một khoản thanh toán hợp lệ, vì vậy câu trả lời là`0`, không có tiền giấy. Ví dụ,`0 2`với mệnh giá`5 10`phải sản xuất`0`, không`5`. 

Mệnh giá trùng lặp cũng là tiền giấy riêng biệt. Vì`p = 6`Và`n = 3`với`3 3 10`, câu trả lời đúng là`6`, sử dụng cả hai`3`tiền giấy. Việc coi đầu vào là một tập hợp các mệnh giá sẽ làm mất đi một trong số chúng một cách không chính xác. 

Một tờ tiền lớn hơn số tiền phạt có thể là câu trả lời tối ưu. Vì`p = 7`và mệnh giá`10 20`, câu trả lời là`10`. Một DP bị giới hạn tối đa ở số tiền`p`không thể đại diện`10`, do đó thuật toán phải coi tiền giấy là bước cuối cùng riêng biệt. 

Cuối cùng cũng đạt`p`chính xác phải đánh bại mọi giải pháp lớn hơn`p`. Vì`p = 10`và mệnh giá`6 4 20`, câu trả lời là`10`, sử dụng`6 + 4`. Một cách tiếp cận chỉ tìm kiếm tổng đầu tiên lớn hơn`p`sẽ bỏ lỡ trường hợp tối ưu này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là lập trình động tổng tập hợp con 0/1 thông thường. Cho phép`dp[s]`cho chúng tôi biết liệu một số tờ tiền đã xử lý có tổng giá trị chính xác hay không`s`. Đối với mỗi tờ tiền có giá trị`q`, chúng tôi lặp lại tất cả các khoản tiền và đánh dấu`s + q`có thể tiếp cận được. Điều này đúng vì mọi tập hợp con đều loại trừ hoặc bao gồm tờ tiền hiện tại và việc lặp lại các tổng theo thứ tự giảm dần sẽ ngăn việc sử dụng cùng một tờ tiền nhiều lần. 

Vấn đề là số lượng hoạt động. Có tới`1000`tiền giấy và lên đến`100000`số tiền liên quan, đưa ra khoảng`100000000`chuyển tiếp trong trường hợp xấu nhất. Như vậy là quá nhiều so với giới hạn thời gian và Python sẽ đặc biệt không phù hợp với vòng lặp như vậy. 

Quan sát quan trọng là trạng thái DP chỉ là một tập hợp các số nguyên. Chúng ta có thể biểu diễn tập hợp đó bằng các bit của một số nguyên lớn. Nếu bit`s`được thiết lập, tổng`s`có thể truy cập được. Thêm một tờ tiền có giá trị`q`sau đó trở thành một sự thay đổi số nguyên duy nhất:`bits | (bits << q)`Sự thay đổi thể hiện việc lấy tờ tiền hiện tại, trong khi bản gốc`bits`đại diện cho việc bỏ qua nó. Các số nguyên có độ chính xác tùy ý của Python thực hiện thao tác này trên các từ máy bên trong, vì vậy thay vì xử lý`p`trạng thái riêng biệt, quá trình chuyển đổi xử lý nhiều trạng thái song song. 

Còn một vấn đề nữa là chúng ta cần tiền giấy thực tế chứ không chỉ đơn thuần là số tiền tối thiểu. Chúng tôi giải quyết việc tái thiết cùng một lúc. Bất cứ khi nào có thể truy cập được một số tiền lần đầu tiên, chúng tôi sẽ lưu trữ tờ tiền nào đã tạo ra số tiền đó. Vì một số tiền chỉ được ghi lại khi trước đó không thể truy cập được nên số tiền trước đó đã có thể truy cập được trước khi tờ tiền hiện tại được xử lý. Tiếp theo các phần trước được lưu trữ này sẽ xây dựng lại một tập hợp con hợp lệ. 

Chúng ta cũng cần xử lý khả năng câu trả lời lớn hơn`p`. Trước khi thêm từng tờ tiền vào DP, chúng tôi xem xét tất cả số tiền hiện có thể tiếp cận được và có thể kết hợp với tờ tiền này để đạt ít nhất`p`. Vì chúng tôi xử lý từng tờ tiền một nên số tiền đó chỉ sử dụng những tờ tiền trước đó, nên tờ tiền hiện tại không thể vô tình được sử dụng hai lần. Chúng tôi chọn số tiền nhỏ nhất có thể tiếp cận được, đưa ra ứng cử viên nhỏ nhất liên quan đến tờ tiền hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n p)`|`O(p)`| Quá chậm | 
| Tập bit tối ưu DP |`O(n p / W + p)`thao tác từ |`O(p)`bit cộng với dữ liệu tái thiết | Đã chấp nhận | 

Đây`W`là kích thước từ máy được sử dụng nội bộ bởi các số nguyên lớn của Python. Chi phí triển khai chính xác bị chi phối bởi kích thước của các số nguyên liên quan thay vì bởi một thao tác trên mỗi trạng thái DP. 

## Hướng dẫn thuật toán 

1. Đọc`p`và danh sách tiền giấy. Nếu như`p = 0`, xuất ngay`0`bởi vì không phải trả gì đã là số tiền tối thiểu có thể. 
2. Biểu thị tất cả các tổng tập hợp con hiện có thể truy cập bằng một số nguyên`bits`. Chút`s`được đặt chính xác khi tổng`s`có thể được hình thành từ các tờ tiền được xử lý cho đến nay. Ban đầu chỉ tính tổng`0`có thể truy cập được, vì vậy`bits = 1`. 
3. Tạo một`parent`mảng được lập chỉ mục theo tổng từ`0`bởi vì`p`. Đối với mỗi số tiền mới có thể tiếp cận`s`, lưu trữ chỉ mục của tờ tiền đã tạo ra nó. Người tiền nhiệm của`s`vậy thì`s - q[index]`. 
4. Xử lý tiền giấy từ trái sang phải. Trước khi thêm tiền giấy hiện tại`q`, tìm kiếm hiện tại`bits`với số tiền nhỏ nhất có thể đạt được`s`thỏa mãn`s + q >= p`. Nếu số tiền đó tồn tại thì`s + q`là câu trả lời ứng cử viên hợp lệ sử dụng tờ tiền hiện tại và chỉ những tờ tiền trước đó. 
5. Giữ lại ứng viên nhỏ nhất được tìm thấy. Nếu ứng viên chính xác`p`, nó tối ưu toàn cục, do đó hãy xây dựng lại nó ngay lập tức. Không có số tiền lớn hơn có thể cải thiện trên một khoản thanh toán chính xác. 
6. Cập nhật tập hợp bit tổng con với tờ tiền hiện tại bằng cách sử dụng`shifted = bits << q`. Giới hạn kết quả ở mức tối đa`p`, bởi vì số tiền trung gian lớn hơn là không cần thiết. Số tiền mới có thể đạt được là`shifted & ~bits`. 
7. Đối với mỗi số tiền mới có thể truy cập, hãy lưu chỉ mục tiền giấy hiện tại vào`parent`. Những khoản tiền này được tạo lần đầu tiên nên trạng thái trước đó của chúng không thể chứa chúng được. 
8. Hợp nhất các trạng thái đã dịch chuyển thành`bits`. Nếu bit`p`được thiết lập, xây dựng lại khoản thanh toán chính xác từ`parent[p]`bởi vì`p`bây giờ có thể truy cập được. 
9. Nếu tất cả tiền giấy đã được xử lý mà không đạt`p`và không tìm được ứng viên ở trên`p`, đầu ra`-1`. Mặt khác, hãy xây dựng lại ứng cử viên tốt nhất bằng cách liên tục lấy tờ tiền gốc được lưu trữ và trừ mệnh giá của nó khỏi số tiền hiện tại. 

Tại sao nó hoạt động: trước khi xử lý tiền giấy`i`,`bits`chứa chính xác số tiền có thể đạt được bằng cách sử dụng tiền giấy có chỉ số nhỏ hơn`i`. Khi chúng tôi kiểm tra một ứng viên`s + q[i]`, tờ tiền`i`không phải là một phần của`s`, do đó tập hợp con kết quả là hợp lệ. Việc chọn ứng cử viên nhỏ nhất như vậy cho mỗi tờ tiền sẽ xem xét mọi giải pháp khả thi theo tờ tiền có chỉ số lớn nhất của nó. Số tiền chính xác`p`được xử lý trực tiếp bởi tập hợp con DP. Để xây dựng lại, mỗi cha mẹ được lưu trữ sẽ trỏ đến một số tiền đã có thể truy cập được trước khi thêm tiền giấy tương ứng, do đó, cha mẹ đi theo nhiều lần cuối cùng cũng đạt được tổng`0`và tạo ra một tập hợp con hợp lệ. Vì mọi khoản thanh toán có thể đều chính xác`p`hoặc có tờ tiền cuối cùng có phần bổ sung vượt qua`p`, ứng cử viên tối thiểu được tìm thấy là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    first = input().split()
    if not first:
        return

    p, n = map(int, first)

    if n:
        q = list(map(int, input().split()))
    else:
        q = []

    if p == 0:
        print(0)
        print()
        return

    # Bit s is 1 iff sum s is reachable.
    # We only need sums from 0 through p.
    limit_mask = (1 << (p + 1)) - 1
    bits = 1  # sum 0

    # parent[s] = index of the banknote that first made s reachable.
    parent = [-1] * (p + 1)

    best_sum = None
    best_last = -1
    best_base = -1

    for i, value in enumerate(q):
        # Find the smallest currently reachable s such that
        # s + value >= p.
        threshold = max(0, p - value)

        candidates = bits >> threshold
        if candidates:
            # Position of the lowest set bit in candidates.
            offset = (candidates & -candidates).bit_length() - 1
            base = threshold + offset
            candidate = base + value

            if best_sum is None or candidate < best_sum:
                best_sum = candidate
                best_last = i
                best_base = base

        # If we already have an exact payment, it is optimal.
        if best_sum == p:
            break

        # Add this banknote to the subset-sum DP.
        shifted = (bits << value) & limit_mask
        new_bits = shifted & ~bits

        # Record reconstruction information for sums that are
        # becoming reachable for the first time.
        x = new_bits
        while x:
            low = x & -x
            s = low.bit_length() - 1
            parent[s] = i
            x -= low

        bits |= shifted

        # An exact payment is always better than every payment > p.
        if (bits >> p) & 1:
            best_sum = p
            best_last = -1
            best_base = p
            break

    if best_sum is None:
        print(-1)
        return

    answer = []

    if best_sum == p and best_last == -1:
        # Reconstruct an exact subset ending at sum p.
        cur = p
        while cur > 0:
            i = parent[cur]
            if i == -1:
                print(-1)
                return
            answer.append(q[i])
            cur -= q[i]
    else:
        # The last banknote is best_last, and the earlier banknotes
        # form best_base.
        answer.append(q[best_last])

        cur = best_base
        while cur > 0:
            i = parent[cur]
            if i == -1:
                print(-1)
                return
            answer.append(q[i])
            cur -= q[i]

    print(best_sum)
    print(*answer)

if __name__ == "__main__":
    solve()
```các`bits`số nguyên là cấu trúc DP trung tâm. Bit 0 ban đầu được đặt vì tập hợp con trống có tổng bằng 0. Đối với một giáo phái`value`, dịch chuyển`bits`để lại bởi`value`tạo ra mọi số tiền có thể kiếm được bằng cách lấy tờ tiền đó. HOẶC giá trị đã dịch chuyển với bitset cũ thể hiện cả hai lựa chọn cho tờ tiền. 

Mặt nạ`(1 << (p + 1)) - 1`loại bỏ số tiền lớn hơn`p`. Những khoản tiền như vậy không cần thiết ở trạng thái trung gian vì bất kỳ giải pháp tối ưu nào ở trên đều`p`có thể được xem như một khoản tiền có thể truy cập dưới đây`p`tiếp theo là tờ tiền cuối cùng của nó. 

biểu hiện`bits >> threshold`loại bỏ mọi số tiền có thể tiếp cận nhỏ hơn`threshold`. Bit được đặt thấp nhất của nó sau đó tương ứng với giá trị nhỏ nhất có thể truy cập được`s >= threshold`. Điều này mang lại tổng số nhỏ nhất có thể`s + value`cho tờ tiền giấy cuối cùng hiện tại. 

Mảng tái thiết chỉ được điền từ`new_bits`, không phải từ mọi tập hợp bit trong`shifted`. Điều này là cần thiết. Một số tiền đã có thể truy cập được sẽ giữ lại số tiền gốc trước đó vì số tiền gốc đó mô tả một tập hợp con được hình thành mà không có tiền giấy hiện tại. Chỉ xử lý các khoản tiền mới có thể truy cập cũng đảm bảo rằng tiền thân của mọi trạng thái được lưu trữ đã được thiết lập. 

Số nguyên Python không bị tràn nên bản thân tổng mệnh giá vẫn an toàn. Số nguyên DP được che giấu rõ ràng`p + 1`bit, giữ cho kích thước của nó bị giới hạn và ngăn các mệnh giá lớn tạo ra các số nguyên trung gian lớn không cần thiết. 

Việc tái thiết sử dụng các chỉ số tiền giấy một cách gián tiếp thông qua`parent`. Các mệnh giá trùng lặp không gây ra vấn đề gì vì mỗi lần xuất hiện có một chỉ mục riêng biệt trong DP, mặc dù đầu ra chỉ chứa các giá trị mệnh giá. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
15 8
20 10 5 5 3 2 1 1
```Thanh toán tối ưu là`15`, Ví dụ`10 + 5`. 

| Bước | Tiền giấy | Ngưỡng | Ứng cử viên vượt biên xuất sắc nhất | Số tiền có thể tiếp cận sau khi cập nhật | 
| --- | --- | --- | --- | --- | 
| 1 | 20 | 0 | 20 | 0 | 
| 2 | 10 | 5 | không | 0, 10 | 
| 3 | 5 | 10 | 15 | 0, 5, 10, 15 | 

Ở tờ tiền thứ ba, tổng`10`đã có thể truy cập được bằng tờ tiền thứ hai. Thêm`5`đưa ra chính xác`15`, do đó thuật toán có thể dừng lại. Cha mẹ được lưu trữ sẽ xây dựng lại`10`Và`5`. 

Điều này chứng tỏ tại sao tờ tiền hiện tại được kiểm tra trước khi nó được đưa vào bitset. Ứng viên`10 + 5`sử dụng hai tờ tiền riêng biệt. 

### Mẫu 2 

đầu vào:```
2 3
10 3 3
```Câu trả lời là`3`, bởi vì một`3`ít nhất là số tiền nhỏ nhất có sẵn`2`. 

| Bước | Tiền giấy | Ngưỡng | Ứng cử viên vượt biên xuất sắc nhất | Số tiền có thể tiếp cận sau khi cập nhật | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 0 | 10 | 0 | 
| 2 | 3 | 0 | 3 | 0 | 
| 3 | 3 | 0 | 3 | 0 | 

Tờ tiền đầu tiên đưa cho ứng cử viên`10`. Người thứ hai đưa ra cho ứng viên`3`, cái nào tốt hơn. Thứ ba cũng cho`3`, nhưng không cải thiện câu trả lời. 

Chi tiết thú vị là số tiền trên`p`không được lưu trữ trong`bits`. Giáo phái`10`vẫn được coi là chính xác như tờ tiền cuối cùng, mặc dù bit`10`không bao giờ xuất hiện trong DP. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n p / W + p)`các thao tác với từ, cộng`O(p)`tổng số bài tập tái thiết-cha mẹ | Mỗi quá trình chuyển đổi bitset`p`bit trong khối từ máy, trong khi mỗi tổng có thể truy cập sẽ nhận được cha mẹ nhiều nhất một lần | 
| Không gian |`O(p)`bit cho DP và`O(p)`số nguyên cho cha mẹ | Chỉ số tiền từ`0`bởi vì`p`được lưu trữ | 

Với`p <= 100000`, bản thân bitset chỉ khoảng 12,5 KB khi được biểu diễn dưới dạng bit thô. Mảng cha lớn hơn vì số nguyên Python là đối tượng, nhưng nó vẫn chỉ lưu trữ`p + 1`mục nhập. Thuật toán tránh được`O(n p)`Vòng lặp lồng nhau ở cấp độ Python sẽ là vấn đề hiệu năng chính dưới giới hạn 1,5 giây. 

## Trường hợp thử nghiệm 

Đầu ra chính xác có thể khác với mẫu một cách hợp pháp vì câu lệnh cho phép bất kỳ tập hợp con tối ưu nào. Vì lý do đó, trình trợ giúp kiểm tra bên dưới xác thực kết quả đầu ra về mặt ngữ nghĩa thay vì yêu cầu một thứ tự hoặc tập hợp con cụ thể.```python
import sys
import io

def solve_io(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def validate(inp: str, out: str):
    data = inp.split()
    p = int(data[0])
    n = int(data[1])
    bills = list(map(int, data[2:2 + n]))

    lines = out.strip().splitlines()

    if p == 0:
        assert lines[0] == "0"
        return

    if lines[0] == "-1":
        # Verify that no subset can reach p by brute force.
        reachable = {0}
        for x in bills:
            reachable |= {s + x for s in list(reachable)}
        assert all(s < p for s in reachable)
        return

    total = int(lines[0])
    used = list(map(int, lines[1].split())) if len(lines) > 1 and lines[1].strip() else []

    assert total == sum(used)
    assert total >= p

    remaining = bills[:]
    for x in used:
        assert x in remaining
        remaining.remove(x)

    # Verify optimality independently for these small test cases.
    reachable = {0}
    for x in bills:
        reachable |= {s + x for s in list(reachable)}

    optimum = min((s for s in reachable if s >= p), default=None)
    assert optimum == total

def solve():
    first = input().split()
    if not first:
        return

    p, n = map(int, first)
    q = list(map(int, input().split())) if n else []

    if p == 0:
        print(0)
        print()
        return

    limit_mask = (1 << (p + 1)) - 1
    bits = 1
    parent = [-1] * (p + 1)

    best_sum = None
    best_last = -1
    best_base = -1

    for i, value in enumerate(q):
        threshold = max(0, p - value)

        candidates = bits >> threshold
        if candidates:
            offset = (candidates & -candidates).bit_length() - 1
            base = threshold + offset
            candidate = base + value

            if best_sum is None or candidate < best_sum:
                best_sum = candidate
                best_last = i
                best_base = base

        if best_sum == p:
            break

        shifted = (bits << value) & limit_mask
        new_bits = shifted & ~bits

        x = new_bits
        while x:
            low = x & -x
            s = low.bit_length() - 1
            parent[s] = i
            x -= low

        bits |= shifted

        if (bits >> p) & 1:
            best_sum = p
            best_last = -1
            best_base = p
            break

    if best_sum is None:
        print(-1)
        return

    answer = []

    if best_sum == p and best_last == -1:
        cur = p
        while cur:
            i = parent[cur]
            assert i != -1
            answer.append(q[i])
            cur -= q[i]
    else:
        answer.append(q[best_last])
        cur = best_base
        while cur:
            i = parent[cur]
            assert i != -1
            answer.append(q[i])
            cur -= q[i]

    print(best_sum)
    print(*answer)

# Provided sample 1
sample1 = """15 8
20 10 5 5 3 2 1 1
"""
out = solve_io(sample1)
validate(sample1, out)

# Provided sample 2
sample2 = """2 3
10 3 3
"""
out = solve_io(sample2)
validate(sample2, out)

# p = 0, empty payment is optimal.
case3 = """0 0
"""
out = solve_io(case3)
validate(case3, out)

# Exact boundary, requires two equal banknotes.
case4 = """6 3
3 3 10
"""
out = solve_io(case4)
validate(case4, out)

# No possible payment.
case5 = """100 3
20 30 40
"""
out = solve_io(case5)
validate(case5, out)

# Large denomination should be considered as a final banknote.
case6 = """7 2
10 20
"""
out = solve_io(case6)
validate(case6, out)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`15 8 / 20 10 5 5 3 2 1 1`| Tổng của bất kỳ tập hợp con nào`15`| Cung cấp mẫu và tái thiết thanh toán chính xác | 
|`2 3 / 10 3 3`|`3`với một`3`| Một mệnh giá lớn hơn giá trị phạt và trùng lặp | 
|`0 0`|`0`| Tập hợp con trống và mịn tối thiểu | 
|`6 3 / 3 3 10`|`6`với cả hai`3`tiền giấy | Nhiều bản cùng tên | 
|`100 3 / 20 30 40`|`-1`| Mục tiêu bất khả thi | 
|`7 2 / 10 20`|`10`với một`10`| Trả lời đúng lớn hơn`p`| 

## Vỏ cạnh 

cho`p = 0`, đầu vào`0 0`được xử lý trước khi DP bắt đầu. Tập con trống có tổng`0`, do đó thuật toán in`0`và dòng thứ hai trống. Cố gắng ép ít nhất một tờ tiền sẽ tạo ra một câu trả lời không tối thiểu. 

Đối với các mệnh giá trùng lặp, hãy xem xét`6 3`với`3 3 10`. đầu tiên`3`kiếm tiền`3`có thể truy cập được và thứ hai`3`sau đó tính tổng`6`có thể truy cập được. Cha mẹ của tổng`6`chỉ vào tờ tiền thứ hai, trong khi số tiền gốc của số tiền`3`chỉ vào điều đầu tiên. Theo chuỗi cho ra hai tờ tiền riêng biệt, cả hai đều có giá trị`3`. 

Đối với một mệnh giá lớn hơn mục tiêu, hãy xem xét`7 2`với`10 20`. Trước khi tờ tiền đầu tiên được thêm vào, tổng`0`có thể truy cập được. Từ`0 >= 7 - 10`, ứng viên`0 + 10 = 10`ngay lập tức được ghi lại. Không có trạng thái DP cho tổng`10`được yêu cầu. Đây chính xác là lý do tại sao việc kiểm tra tiền giấy cuối cùng được thực hiện trước khi chuyển đổi bitset. 

Để có khoản thanh toán chính xác, hãy xem xét`10 3`với`6 4 20`. Trước khi xử lý`4`, tổng`6`đã có thể truy cập được. Ngưỡng cho`4`là`6`, do đó thuật toán tìm được ứng viên`6 + 4 = 10`. Nó chính xác và thuật toán dừng lại với tiền giấy`6`Và`4`. Một giải pháp như`20`không bao giờ được phép thay thế nó vì mọi tổng đều bằng`p`là tối ưu. 

Đối với một khoản thanh toán không thể thực hiện được, hãy xem xét`100 3`với`20 30 40`. Mọi tập hợp con có tổng nhiều nhất`90`, vì vậy bitset không bao giờ đặt bit`100`và không có ứng cử viên tiền giấy cuối cùng nào đạt được`100`. Do đó, thuật toán in`-1`.
