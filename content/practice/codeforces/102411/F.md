---
title: "CF 102411F - Foreach"
description: "Chúng tôi có một mảng số nguyên a có độ dài n và chúng tôi muốn chuyển đổi nó thành mảng mục tiêu b. Hướng dẫn duy nhất chúng tôi được phép in là hai vòng lặp foreach đặc biệt."
date: "2026-08-12T00:15:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102411
codeforces_index: "F"
codeforces_contest_name: "ICPC 2019-2020 North-Western Russia Regional Contest"
rating: 0
weight: 102411
solve_time_s: 164
verified: true
draft: false
---

[CF 102411F - Foreach](https://codeforces.com/problemset/problem/102411/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng số nguyên`a`chiều dài`n`và chúng tôi muốn chuyển đổi nó thành một mảng mục tiêu`b`. Hướng dẫn duy nhất chúng tôi được phép in là hai hướng dẫn đặc biệt`foreach`vòng lặp. Vòng lặp tham chiếu ghi nhớ một phần tử của mảng thông qua`⟦PROTECT_12⟧x`. Bởi vì PHP không giới thiệu một phạm vi mới cho`$x`, tham chiếu cũ vẫn tồn tại giữa hai vòng lặp. Hành vi trông có vẻ ngẫu nhiên đó là cơ chế cho phép chương trình bị hạn chế sửa đổi mảng. 

Hiệu quả hữu ích có thể được hiểu mà không cần suy nghĩ về cú pháp PHP sau một vài ví dụ đầu tiên. Giả sử mảng hiện tại chứa`x`và chúng tôi thực hiện một vòng lặp tham chiếu dừng ở lần xuất hiện đầu tiên của`x`.`⟦PROTECT_13⟧x`đề cập đến phần tử cuối cùng, do đó, vòng lặp không tham chiếu sau có thể thay thế phần tử cuối cùng bằng bất kỳ giá trị nào đã có. 

Mảng có tối đa 50 phần tử và mọi giá trị nằm trong khoảng từ 1 đến 100. Phần tử nhỏ`n`làm cho việc xây dựng bậc hai hoàn toàn hợp lý. MỘT`O(n^2)`thuật toán thực hiện tối đa vài nghìn lần quét cơ bản, trong khi tìm kiếm theo cấp số nhân thông qua các chương trình khả thi sẽ có hệ số phân nhánh rất lớn. Bản thân đầu ra có thể chứa tới 10.000 dòng, do đó việc xây dựng cũng phải nằm trong giới hạn đó. 

Có hai trường hợp đặc biệt dễ xử lý sai. Đầu tiên, một giá trị không thể xuất hiện trong mục tiêu nếu nó không xuất hiện ban đầu. Ví dụ,```
2
1 2
1 3
```phải sản xuất```
-1
```bởi vì không có thao tác nào được phép có thể tạo ra giá trị`3`. 

Thứ hai, nếu mục tiêu chỉ chứa các giá trị riêng biệt và khác với mảng ban đầu thì việc chuyển đổi là không thể. Ví dụ,```
3
1 2 3
2 3 1
```có các giá trị mục tiêu xuất hiện ban đầu, nhưng mọi thao tác không cần thiết nhất thiết phải tạo ra một giá trị trùng lặp ở đâu đó. Khi bản thân mục tiêu yêu cầu ba giá trị riêng biệt thì không thể đạt được trạng thái cuối cùng sau khi thay đổi như vậy. Giải pháp chính thức sử dụng chính xác tiêu chí bất khả thi này. 

Ngoài ra còn có một trường hợp ranh giới tinh vi khi giá trị cuối cùng của mục tiêu chỉ xuất hiện một lần. Cấu trúc sử dụng phần tử cuối cùng làm nơi lưu trữ tạm thời có thể vô tình phá hủy bản sao duy nhất của giá trị đó. Giải pháp xử lý vấn đề này bằng cách sửa đổi tạm thời mục tiêu trước tiên để giá trị cuối cùng của nó xuất hiện lần thứ hai, chuyển đổi thành mục tiêu được sửa đổi đó, sau đó khôi phục vị trí đã thay đổi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực có thể coi mọi đường lối pháp lý là một sự lựa chọn và tìm kiếm trình tự đạt được mục tiêu. Mỗi dòng có tối đa 200 dạng có thể, vì giá trị điều kiện có thể là số nguyên bất kỳ từ 1 đến 100 và vòng lặp có thể là tham chiếu hoặc không tham chiếu. Tìm kiếm theo chiều sâu`k`do đó có số lượng phân nhánh trong trường hợp xấu nhất là`200^k`và độ sâu cho phép có thể đạt tới 10.000. Ngay cả đối với các mảng nhỏ, điều này hoàn toàn không khả thi. 

Một ý tưởng ngây thơ hữu ích hơn là cố gắng thao túng trực tiếp mọi vị trí một cách độc lập. Vấn đề là hoạt động nguyên thủy không giải quyết được một chỉ mục tùy ý. Nó giải quyết sự xuất hiện đầu tiên của một giá trị hoặc phần tử cuối cùng. Nếu giá trị chúng ta muốn sửa đổi xuất hiện sớm hơn trong mảng thì một thao tác được cho là cục bộ sẽ thay đổi sai vị trí. Công trình trước hết phải kiểm soát sự việc nào xảy ra đầu tiên. 

Quan sát quan trọng là hai phép toán mảng đơn giản hơn nhiều có thể được xây dựng từ lạ`foreach`ngữ nghĩa. Chúng ta có thể thay thế lần xuất hiện đầu tiên của bất kỳ giá trị hiện có nào`x`bởi một giá trị hiện có khác`y`. Chúng ta cũng có thể thay thế phần tử cuối cùng bằng bất kỳ giá trị hiện có nào`y`. Mỗi thao tác trừu tượng này tốn chính xác hai vòng lặp được in. Hướng dẫn chính thức xác định hai nguyên thủy giống nhau. 

Bây giờ hãy xem xét việc sửa mảng từ phải sang trái. Khi vị trí`i`cần thay đổi từ`x`ĐẾN`z`, trước tiên chúng tôi loại bỏ mọi lần xuất hiện trước đó của`x`. Điều đó tạo nên vị thế`i`sự xuất hiện đầu tiên của`x`, do đó, nguyên hàm xuất hiện lần đầu tiên có thể giải quyết chính xác vị trí này. Chúng tôi tạm thời sao chép`x`vào phần tử cuối cùng, sau đó thay thế phần tử đầu tiên`x`qua`z`. Phần tử cuối cùng đóng vai trò là bản sao tạm thời của giá trị chúng ta đang di chuyển. 

Mối nguy hiểm duy nhất là mất giá trị được lưu trữ trong phần tử cuối cùng. Chúng tôi tránh điều đó bằng cách duy trì bản sao của giá trị cuối cùng bất cứ khi nào chúng tôi vẫn còn vị trí cần xử lý. Nếu giá trị cuối cùng chỉ xuất hiện một lần, chúng tôi sẽ sao chép nó vào vị trí an toàn trong tiền tố chưa được xử lý trước khi tiếp tục. Kiểm tra an toàn sẽ kiểm tra xem giá trị bị ghi đè có còn tồn tại ở đâu đó hay không, đã được cố định trong hậu tố hay không được tiền tố mục tiêu còn lại yêu cầu. 

Có một nhóm mục tiêu khó xử trong đó giá trị cuối cùng là duy nhất và có giá trị lặp lại ở nơi khác. Thay vì làm cho logic bảo toàn phụ thuộc vào sự sắp xếp đặc biệt của mục tiêu, chúng ta có thể tạm thời sửa đổi mục tiêu. Chúng tôi lấy một lần xuất hiện của bất kỳ giá trị lặp lại nào và thay thế nó bằng giá trị cuối cùng duy nhất. Mục tiêu được sửa đổi hiện có hai bản sao của giá trị cuối cùng. Sau khi đạt được mục tiêu đã sửa đổi đó, vị trí đã thay đổi là lần xuất hiện đầu tiên của giá trị cuối cùng, do đó, một thao tác lần xuất hiện đầu tiên cuối cùng sẽ khôi phục mục tiêu ban đầu. 

Nếu mục tiêu không có giá trị lặp lại và khác với nguồn, thì việc kiểm tra tính không thể thực hiện được đã từ chối nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(200^k)`trong trường hợp xấu nhất |`O(nk)`hoặc tệ hơn | Quá chậm | 
| Tối ưu |`O(n^2)`hoạt động/quét trừu tượng |`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng ban đầu`a`và mảng mục tiêu`b`. Nếu một số giá trị xảy ra trong`b`nhưng không bao giờ xảy ra ở`a`, không chương trình nào có thể tạo ra giá trị đó nên hãy in`-1`. 
2. Nếu`a`đã bằng rồi`b`, in các hoạt động bằng không. Không cần xử lý đặc biệt. 
3. Nếu mọi giá trị trong`b`xảy ra đúng một lần, từ chối phép chuyển đổi. Bất kỳ thao tác không cần thiết nào cũng tạo ra một bản sao, trong khi mảng cuối cùng được yêu cầu không có bản sao. Đây cũng là lý do tại sao điều kiện chỉ đúng khi`a != b`. 
4. Nếu giá trị cuối cùng của`b`chỉ xảy ra một lần, xây dựng mục tiêu tạm thời`c`. Tìm sự xuất hiện của một số giá trị xuất hiện ít nhất hai lần trong`b`và thay thế sự xuất hiện đó bằng`b[n-1]`. Để lại một lần xuất hiện khác của giá trị lặp lại không bị ảnh hưởng. Mục tiêu tạm thời hiện chứa hai bản sao giá trị cuối cùng của nó. 
5. Duy trì hai nguyên thủy trừu tượng.`first_to(x, y)`thay thế lần xuất hiện đầu tiên của`x`qua`y`.`last_to(y)`thay thế phần tử cuối cùng bằng`y`. Cả hai đều hợp pháp vì`y`được đảm bảo tồn tại khi chúng được gọi. 
6. Xử lý các vị trí từ`n-2`xuống`0`. Vị trí cuối cùng được cố tình để lại cho đến hết vì đó là vị trí lưu trữ tạm thời của chúng ta. 
7. Nếu`a[i]`đã bằng rồi`c[i]`, hãy để yên vị trí này. Nếu không hãy để`x = a[i]`Và`z = c[i]`. 
8. Loại bỏ mọi lần xuất hiện của`x`trước vị trí`i`. Mỗi lần xuất hiện như vậy được thay thế bằng một giá trị an toàn đã có trong mảng. Nếu phần tử cuối cùng không`x`, giá trị của nó là một lựa chọn thuận tiện. Nếu phần tử cuối cùng là`x`, sau đó`z`khác với`x`bởi vì vị trí này cần phải thay đổi, vì vậy`z`có thể được sử dụng thay thế. 
9. Sau tất cả các bản sao trước đó của`x`đã biến mất, vị trí`i`là lần xuất hiện đầu tiên của`x`. Sao chép`x`đến vị trí cuối cùng và sau đó thay thế vị trí đầu tiên`x`qua`z`. Chức vụ`i`bây giờ là chính xác, trong khi cái cũ`x`cuối cùng vẫn có sẵn. 
10. Nếu giá trị cuối cùng trở thành duy nhất, hãy tìm một giá trị`v`trong tiền tố vẫn chưa được xử lý có thể được thay thế một cách an toàn bằng giá trị cuối cùng. Một giá trị là an toàn nếu một bản sao khác tồn tại trong tiền tố, nó đã xuất hiện trong hậu tố được xử lý hoặc không bắt buộc ở bất kỳ đâu trong tiền tố đích còn lại. Sao chép giá trị cuối cùng vào lần xuất hiện đầu tiên của`v`. Giá trị cuối cùng hiện có ít nhất hai bản sao. 
11. Sau khi tất cả các vị trí trước vị trí cuối cùng đều đúng, hãy thay phần tử cuối cùng bằng`c[n-1]`. Mục tiêu tạm thời được xây dựng sao cho giá trị này vẫn có sẵn ở đâu đó trong tiền tố. 
12. Nếu mục tiêu tạm thời được sử dụng, hãy khôi phục vị trí mục tiêu đã thay đổi. Tại thời điểm đó, giá trị tạm thời là lần xuất hiện đầu tiên của giá trị cuối cùng ban đầu, do đó, một giá trị duy nhất`first_to`hoạt động thay đổi nó trở lại giá trị mục tiêu ban đầu. 

### Tại sao nó hoạt động 

Bất biến trung tâm là mọi vị trí đã được xử lý từ phải sang trái đều bằng mục tiêu tạm thời và mọi giá trị mà tiền tố chưa được xử lý hoặc vị trí cuối cùng cần vẫn tồn tại ở đâu đó trong mảng. Trước khi thay đổi vị trí`i`, tất cả các bản sao trước đó của giá trị hiện tại của nó sẽ bị xóa, vì vậy`i`trở thành lần xuất hiện đầu tiên của nó. Điều đó làm cho hành động nguyên thủy lần đầu tiên diễn ra chính xác ở vị trí đã định. Phần tử cuối cùng cung cấp bản sao tạm thời của giá trị đang được di chuyển và bước sửa chữa sẽ ngăn bản sao tạm thời đó trở thành bản sao duy nhất của giá trị vẫn cần thiết. Khi tiền tố hoàn tất, thao tác phần tử cuối cùng sẽ tạo ra chính xác mục tiêu tạm thời và quá trình khôi phục tùy chọn sẽ chuyển đổi mục tiêu đó thành mục tiêu được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_program(a, target):
    n = len(a)
    ops = []

    def first_to(x, y):
        if x == y:
            return
        p = a.index(x)
        ops.append(("ref", x))
        ops.append(("val", y))
        a[p] = y

    def last_to(y):
        if a[-1] == y:
            return

        # n <= 50 and all values are <= 100, so an absent
        # value among 1..100 always exists when this is needed.
        used = set(a)
        absent = next(v for v in range(1, 101) if v not in used)

        ops.append(("ref", absent))
        ops.append(("val", y))
        a[-1] = y

    for i in range(n - 2, -1, -1):
        if a[i] == target[i]:
            continue

        x = a[i]
        z = target[i]

        # Make i the first occurrence of x.
        while True:
            p = -1
            for j in range(i):
                if a[j] == x:
                    p = j
                    break

            if p == -1:
                break

            spare = a[-1]
            if spare == x:
                spare = z

            # z != x here, so spare is different from x.
            first_to(x, spare)

        # Preserve x at the last position, then make i correct.
        if a[-1] != x:
            last_to(x)

        first_to(x, z)

        # If the last value is unique, duplicate it somewhere safe
        # in the still-unprocessed prefix.
        if i > 0:
            last_value = a[-1]
            if a.count(last_value) == 1:
                prefix = a[:i]
                suffix = a[i + 1:]

                safe = None
                for v in prefix:
                    if v == last_value:
                        continue

                    cnt_prefix = prefix.count(v)
                    in_suffix = v in suffix
                    needed = v in target[:i]

                    if cnt_prefix >= 2 or in_suffix or not needed:
                        safe = v
                        break

                if safe is None:
                    # With the temporary-target construction below,
                    # this case cannot occur.
                    return None

                first_to(safe, last_value)

    # The last position was intentionally skipped.
    if a[-1] != target[-1]:
        last_to(target[-1])

    return ops

def solve_case(n, s, b):
    if s == b:
        return []

    if any(x not in set(s) for x in b):
        return None

    freq = {}
    for x in b:
        freq[x] = freq.get(x, 0) + 1

    if all(freq[x] == 1 for x in b):
        return None

    target = b[:]
    restore_pos = -1
    restore_value = -1

    # If the last target value is unique, temporarily make it
    # appear twice by replacing one occurrence of a repeated value.
    last_value = target[-1]

    if freq[last_value] == 1:
        repeated_pos = -1
        for i in range(n - 1):
            if freq[target[i]] >= 2:
                repeated_pos = i
                break

        if repeated_pos == -1:
            return None

        restore_pos = repeated_pos
        restore_value = target[repeated_pos]
        target[repeated_pos] = last_value

    a = s[:]
    ops = build_program(a, target)

    if ops is None:
        return None

    # Restore the temporary target modification.
    if restore_pos != -1:
        x = target[-1]
        y = restore_value

        # target[restore_pos] == x, and x was unique in the
        # original target. Hence restore_pos is the first x.
        if a[restore_pos] != x:
            return None

        ops.append(("ref", x))
        ops.append(("val", y))
        a[restore_pos] = y

    if a != b or len(ops) > 10000:
        return None

    return ops

def format_ops(ops):
    out = [str(len(ops))]
    for typ, value in ops:
        if typ == "ref":
            out.append(
                f"foreach ($a as &$x) if ($x == {value}) break;"
            )
        else:
            out.append(
                f"foreach ($a as  $x) if ($x == {value}) break;"
            )
    return "\n".join(out)

def solve():
    n = int(input())
    s = list(map(int, input().split()))
    b = list(map(int, input().split()))

    ops = solve_case(n, s, b)

    if ops is None:
        print(-1)
        return

    print(format_ops(ops))

if __name__ == "__main__":
    solve()
```các`first_to`helper là việc thực hiện nguyên thủy trừu tượng đầu tiên. Vòng lặp in đầu tiên rời đi`$x`đề cập đến lần đầu tiên`x`, và vòng lặp thứ hai đi cho đến khi gặp`y`, viết từng giá trị gặp phải thông qua tham chiếu cũ. Phần tử được tham chiếu kết quả trở thành`y`. 

các`last_to`người trợ giúp cần một điều kiện không xảy ra trong mảng hiện tại. Vì mảng chứa tối đa 50 giá trị trong khi phạm vi hợp lệ chứa 100 giá trị nên giá trị như vậy luôn tồn tại. Do đó, vòng tham chiếu kết thúc bình thường với`$x`đề cập đến phần tử mảng cuối cùng. Vòng lặp không tham chiếu sau sẽ sao chép giá trị được yêu cầu vào vị trí đó. 

Vòng lặp từ phải sang trái là trái tim của công trình. các`while`vòng lặp loại bỏ các bản sao trước đó của giá trị hiện tại để vị trí mong muốn trở thành lần xuất hiện đầu tiên. Sự lựa chọn của`spare`được cố tình làm khác với`x`, nếu không thao tác xảy ra lần đầu sẽ không tiến triển. 

Bước sửa chữa chỉ chạm vào tiền tố chưa được xử lý. Một giá trị có bản sao khác ở đó có thể mất đi một lần xuất hiện một cách an toàn. Một giá trị đã có trong hậu tố được xử lý cũng an toàn vì lần xuất hiện cuối cùng bắt buộc của nó đã được cố định. Cuối cùng, một giá trị vắng mặt trong tiền tố mục tiêu còn lại không còn cần thiết ở đó nữa. 

Số nguyên Python không bị tràn và bộ sưu tập có liên quan lớn nhất chỉ có 50 phần tử. Phần có thể tinh tế nhất là khoảng cách chính xác trong vòng lặp không tham chiếu. Tuyên bố rõ ràng yêu cầu hai khoảng trắng giữa`as`Và`⟦PROTECT_14⟧a as  $x)`thay vì bình thường hóa khoảng trắng đó. Tuyên bố chính thức coi lỗi định dạng là câu trả lời sai. 

## Ví dụ đã hoạt động 

### Mẫu 1 

mẫu là```
3
1 2 3
1 3 3
```Mục tiêu đã có hai bản sao của`3`, vì vậy không cần có mục tiêu tạm thời. Chỉ số quá trình xây dựng`1`và chỉ số lá`2`như nơi lưu trữ tạm thời. 

| Bước | Hành động | Mảng | 
| --- | --- | --- | 
| 0 | Bắt đầu |`[1, 2, 3]`| 
| 1 |`last_to(2)`|`[1, 2, 2]`| 
| 2 |`first_to(2, 3)`|`[1, 3, 2]`| 
| 3 |`last_to(3)`|`[1, 3, 3]`| 

Hai thay đổi trừu tượng ở giữa chính xác là cơ chế đằng sau mẫu chính thức, mặc dù việc xây dựng có thể tự do xuất ra một chương trình hợp lệ khác vì câu lệnh không yêu cầu giảm thiểu số lượng dòng. 

### Mẫu 2 

Mẫu thứ hai là```
2
1 2
1 3
```giá trị`3`không tồn tại trong mảng ban đầu. 

| Bước | Kiểm tra | Kết quả | 
| --- | --- | --- | 
| 0 | Giá trị ban đầu là`{1, 2}`|`{1, 2}`| 
| 1 | Mục tiêu yêu cầu`3`|`3`không có sẵn | 
| 2 | Dừng lại |`-1`| 

Dấu vết chứng tỏ rằng sự bất khả thi có thể được phát hiện trước bất kỳ công trình xây dựng nào. Không có thao tác nào được phép có thể đưa ra một giá trị không có trong mảng ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n^2)`| Mỗi vị trí có thể gây ra`O(n)`quét mảng và`n <= 50`. | 
| Không gian |`O(n^2)`| Chương trình đầu ra có thể chứa`O(n^2)`dòng, trong khi bản thân mảng làm việc sử dụng`O(n)`không gian. | 

Giới hạn bậc hai rất nhỏ đối với`n <= 50`. Việc xây dựng cũng ở dưới mức 10.000 dòng đầu ra cần thiết, với những trường hợp xấu nhất chỉ cần vài nghìn thao tác nguyên thủy. Phân tích chính thức cũng cho kết quả tương tự`O(n^2)`bị ràng buộc và quan sát thấy rằng điều này là tối ưu tiệm cận cho các mảng xen kẽ. 

## Trường hợp thử nghiệm 

Trình kiểm tra chấp nhận bất kỳ chương trình hợp lệ nào, vì vậy các thử nghiệm bên dưới sẽ xác thực chương trình được tạo bằng cách mô phỏng ngữ nghĩa vòng lặp tham chiếu chính xác và không tham chiếu thay vì so sánh đầu ra văn bản với một chuỗi cố định.```python
# Save the submitted solution as solution.py before running this file.

from solution import solve_case

def execute_program(a, ops):
    a = a[:]
    ref = None
    n = len(a)

    for typ, value in ops:
        if typ == "ref":
            ref = None
            for i in range(n):
                ref = i
                if a[i] == value:
                    break

        else:
            assert ref is not None
            for i in range(n):
                a[ref] = a[i]
                if a[ref] == value:
                    break

    return a

def run(inp: str) -> str:
    import io

    data = inp.strip().splitlines()
    n = int(data[0])
    s = list(map(int, data[1].split()))
    b = list(map(int, data[2].split()))

    ops = solve_case(n, s, b)

    if ops is None:
        return "-1"

    result = execute_program(s, ops)
    assert result == b
    assert len(ops) <= 10000

    return str(len(ops))

# Sample 1
assert run(
    """3
1 2 3
1 3 3
"""
) != "-1", "sample 1"

# Sample 2
assert run(
    """2
1 2
1 3
"""
) == "-1", "sample 2"

# Minimum size, already equal.
assert run(
    """1
7
7
"""
) == "0", "minimum size"

# Minimum size, different values.
assert run(
    """1
7
8
"""
) == "-1", "single element cannot change"

# All values equal in the target.
assert run(
    """3
1 2 2
2 2 2
"""
) != "-1", "all-equal target"

# Last target value is unique, so temporary target modification is needed.
assert run(
    """5
5 1 2 3 4
1 2 2 3 4
"""
) != "-1", "unique last target value"

# Alternating values, a case that exercises many first-occurrence changes.
n = 50
s = [1 if i % 2 == 0 else 2 for i in range(n)]
b = [2 if i % 2 == 0 else 1 for i in range(n)]
inp = f"{n}\n{' '.join(map(str, s))}\n{' '.join(map(str, b))}\n"
assert run(inp) != "-1", "maximum-size alternating case"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7 / 7`|`0`| Mảng không thay đổi kích thước tối thiểu | 
|`1 / 7 / 8`|`-1`| Mảng một phần tử không thể sửa đổi | 
|`3 / 1 2 2 / 2 2 2`| Chương trình hợp lệ | Giá trị lặp lại và giá trị mục tiêu lặp lại | 
|`5 / 5 1 2 3 4 / 1 2 2 3 4`| Chương trình hợp lệ | Giá trị mục tiêu cuối cùng duy nhất và mục tiêu tạm thời | 
| Các mảng có chiều dài xen kẽ`50`| Chương trình hợp lệ | Kích thước tối đa và xây dựng bậc hai | 

## Vỏ cạnh 

Đối với một giá trị mục tiêu không có sẵn, chẳng hạn như```
2
1 2
1 3
```người giải quyết kiểm tra tư cách thành viên trước khi thử bất kỳ thao tác nào. Từ`3`vắng mặt trong mảng ban đầu, nó ngay lập tức trả về`-1`. Điều này ngăn cản việc xây dựng đạt đến trạng thái thiếu giá trị nguồn được cho là có sẵn. 

Đối với mảng một phần tử,```
1
7
7
```câu trả lời là không có hoạt động nào. Nếu mục tiêu là`8`, câu trả lời sẽ là`-1`. Không có phần tử mảng thứ hai nào có thể hoạt động như một nguồn cho một giá trị khác và các vòng lặp hợp pháp duy nhất chỉ có thể viết lại phần tử đơn lẻ với chính nó. 

Đối với mục tiêu chứa tất cả các giá trị riêng biệt,```
3
1 2 3
2 3 1
```người giải quyết từ chối nó ngay lập tức. Hoạt động không cần thiết đầu tiên sẽ sao chép một số giá trị hiện có vào một vị trí khác, tạo ra một bản sao, trong khi mục tiêu được yêu cầu không có bản sao. Mảng không thể trở về một hoán vị hoàn toàn khác biệt bằng cách sử dụng các thao tác bị hạn chế này. 

Đối với giá trị mục tiêu cuối cùng duy nhất,```
5
5 1 2 3 4
1 2 2 3 4
```giá trị`4`chỉ xảy ra ở vị trí mục tiêu cuối cùng. Một chiến lược lưu trữ tạm thời đơn giản có thể ghi đè lên`4`và sau đó phát hiện ra rằng nó không có cách nào để tạo lại nó. Bộ giải tạm thời thay đổi một lần xuất hiện của giá trị đích lặp lại`2`vào trong`4`, tạo ra mục tiêu phụ`[1, 4, 2, 3, 4]`. Giá trị cuối cùng hiện có hai bản sao, vì vậy nó có thể được sử dụng làm nơi lưu trữ tạm thời một cách an toàn. Khi đã đạt được mục tiêu phụ đó, mục tiêu đầu tiên`4`chính xác là vị trí đã được thay đổi tạm thời và`first_to(4, 2)`khôi phục mục tiêu ban đầu. 

Đối với trường hợp kích thước tối đa xen kẽ,```
50
1 2 1 2 1 2 ...
2 1 2 1 2 1 ...
```cơ chế xuất hiện lần đầu tương tự được thực hiện lặp đi lặp lại. Các bản sao trước đó của một giá trị phải được di chuyển ra khỏi vị trí trước khi có thể xác định được vị trí mong muốn. Điều này tạo ra hành vi bậc hai được mô tả bởi phân tích chính thức và chứng minh tại sao một`O(n^2)`xây dựng là mục tiêu tự nhiên của`n = 50`. 

Một cảnh báo nhỏ: cách xây dựng ở trên tuân theo chiến lược hai nguyên thủy chính thức và bao gồm một biến thể mục tiêu tạm thời nhằm đơn giản hóa lập luận bảo toàn. Tính bất biến và độ phức tạp chính của bài xã luận phù hợp với phân tích chính thức.
