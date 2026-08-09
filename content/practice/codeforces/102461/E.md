---
title: "CF 102461E - Thứ Sáu Đen"
description: "Chúng tôi có n loại card màn hình khác nhau. Đối với loại i, việc mua loại đó với giá chiết khấu chỉ được phép nếu chúng ta không mua gì hoặc mua một số nguyên thẻ từ li đến ri. Tổng cộng Misha có thể mang tối đa s lá bài và mục tiêu là tối đa hóa tổng số đó."
date: "2026-08-08T09:53:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102461
codeforces_index: "E"
codeforces_contest_name: "Innopolis Open 2019-2020, qualification, contest 2"
rating: 0
weight: 102461
solve_time_s: 174
verified: true
draft: false
---

[CF 102461E - Thứ Sáu Đen](https://codeforces.com/problemset/problem/102461/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`n`các loại card màn hình khác nhau. Đối với loại`i`, việc mua loại đó với giá chiết khấu chỉ được phép nếu chúng ta không mua gì hoặc mua một số lượng thẻ nguyên giữa`l_i`Và`r_i`. Misha có thể mang nhiều nhất`s`tổng số thẻ và mục tiêu là tối đa hóa tổng số đó. 

Đối với một tập hợp cố định các loại đã chọn, số tiền mua nhỏ nhất có thể là tổng giá trị của chúng`l_i`, trong khi lớn nhất là tổng của chúng`r_i`. Vì mọi số nguyên giữa hai tổng này đều có thể đạt được nên một tập hợp được chọn là khả thi chính xác khi tổng giới hạn dưới của nó không vượt quá`s`. Đóng góp tốt nhất có thể của nó là phần đóng góp nhỏ hơn`s`và tổng giới hạn trên của nó. 

Phần khó khăn là lựa chọn các loại. Một phép liệt kê tập hợp con trực tiếp xem xét`2^n`bộ, đó là về`2^100000`khả năng ở kích thước đầu vào lớn nhất. Ngay cả một chương trình động được lập chỉ mục bởi`s`là không thể bởi vì`s`có thể`10^13`. Giải pháp dự định phải phụ thuộc chủ yếu vào`n`và sắp xếp theo sau là quét tuyến tính hoặc logarit là thích hợp cho`n = 10^5`. 

Có ba tình huống ranh giới có thể dễ dàng phá vỡ việc thực hiện bất cẩn. Đầu tiên, một loại có thể có`l_i > s`, nên nó không bao giờ có thể mua được. Ví dụ, với`s = 5`và khoảng đơn`[6, 10]`, kết quả đúng là`0`và số lượng đầu ra là`0`. điều trị`r_i >= s`nếu đủ sẽ chọn sai loại này. 

Thứ hai, một số loại khả thi riêng lẻ có thể không khả thi cùng nhau. Ví dụ,```
3 20
1 2
10 17
11 16
```có tổng tối ưu`19`, thu được từ hai loại đầu tiên. Việc chọn loại thứ hai và thứ ba có vẻ hấp dẫn nếu nhìn từ giới hạn trên của chúng, nhưng tổng số tối thiểu của chúng là`21`, đã vượt quá khả năng rồi. 

Thứ ba, tối đa hóa tổng số giới hạn dưới là không đủ. Với`s = 10`, khoảng`[3, 12]`Và`[8, 12]`không thể chọn cả hai, nhưng chỉ một trong hai người mới có thể đạt được`10`. Một giải pháp chỉ cố gắng đóng gói càng nhiều thẻ bắt buộc càng tốt có thể bỏ sót một thực tế là loại có kích thước lớn`r_i`có thể lấp đầy dung lượng còn lại. 

Giải pháp chính thức sử dụng cách rút gọn cấu trúc tương tự được mô tả bên dưới: phân chia các loại ở ngưỡng`2s/7`, giải quyết riêng biệt trường hợp có ba giới hạn dưới đủ lớn và nếu không thì giảm lựa chọn còn lại xuống tối đa hai loại lớn. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản về mặt khái niệm. Liệt kê mọi tập hợp con của các loại card màn hình, tính tổng giới hạn dưới và giới hạn trên của nó, loại bỏ nó nếu tổng giới hạn dưới vượt quá`s`, còn ngược lại thì lấy`min(s, sum_r)`là tổng số tốt nhất cho tập hợp con đó. Điều này đúng vì mọi số nguyên giữa hai tổng điểm cuối đều có thể được lấy từ các khoảng đã chọn. Trong trường hợp xấu nhất nó sẽ kiểm tra`2^n`tập hợp con và mỗi tập hợp con cần tới`O(n)`làm việc, cống hiến`O(n 2^n)`. Vì`n = 10^5`, điều này không khả thi chút nào. 

Quan sát chính xuất phát từ yếu tố`1.4`. Viết nó như`7/5`. Nếu tập được chọn có tổng giới hạn dưới`L`, tổng giới hạn trên của nó ít nhất là`7L/5`. Theo đó, một lần`L >= 5s/7`, tập hợp luôn có thể đạt chính xác`s`. 

Bây giờ hãy phân loại một loại là nhỏ khi`l_i <= 2s/7`và lớn khác. Ba loại lớn có tổng giới hạn dưới lớn hơn`6s/7`. Nếu ba loại lớn nhỏ nhất phù hợp với`s`, tổng giới hạn trên của chúng ít nhất là`7/5 * 6s/7 = 6s/5 > s`, 

vậy là 3 loại đó cho ngay câu trả lời tối ưu`s`. 

Nếu ba loại lớn đó không phù hợp thì không có giải pháp khả thi nào có thể chứa ba loại lớn, bởi vì mọi bộ ba loại lớn khác đều có tổng giới hạn dưới thậm chí còn lớn hơn. Do đó, mọi giải pháp khả thi đều chứa tối đa hai loại lớn. 

Điều đó để lại một vấn đề tổ hợp nhỏ hơn nhiều. Chúng ta cần một hoặc hai loại lớn tốt nhất, trong khi những loại nhỏ có thể được thêm vào xung quanh chúng. Nếu việc cộng tất cả các loại đã chọn làm cho tổng giới hạn dưới vượt quá`s`, trước tiên chúng tôi loại bỏ các giới hạn dưới nhỏ nhất. Mọi loại bị loại bỏ đều có kích thước nhỏ nên giới hạn dưới của nó nhiều nhất là`2s/7`. Thời điểm tổng số trở nên khả thi trở lại, tổng giới hạn dưới của nó lớn hơn`5s/7`. Điều đó đã đảm bảo một khoản tiền giới hạn trên`s`, vì vậy trường hợp này tự động tối ưu với tổng`s`. 

Do đó, tìm kiếm không cần thiết duy nhất là tìm cặp loại lớn có giá trị tối đa`r_i + r_j`tùy thuộc vào`l_i + l_j <= s`. Sau khi sắp xếp theo`l_i`, điều này có thể được thực hiện bằng tìm kiếm nhị phân và cực đại tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n 2^n)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mỗi khoảng cùng với chỉ mục ban đầu của nó và sắp xếp các khoảng theo`l_i`. Việc sắp xếp cung cấp cho chúng ta một tiền tố của các loại nhỏ và một hậu tố của các loại lớn, đây chính xác là sự phân tách cần thiết cho`2s/7`lý lẽ. 
2. Tìm loại đầu tiên có giới hạn dưới thỏa mãn`7l_i > 2s`. Tất cả các loại trước đều nhỏ và tất cả các loại sau đều lớn. Chúng tôi sử dụng so sánh số nguyên chính xác`7l_i <= 2s`thay vì số học dấu phẩy động, vì giá trị đầu vào là số nguyên và ranh giới là quan trọng. 
3. Nếu có ít nhất ba loại lớn và ba giới hạn dưới lớn nhỏ nhất phù hợp với`s`, hãy chọn ba cái đó. Tổng giới hạn dưới của chúng lớn hơn`6s/7`, do đó tổng giới hạn trên của chúng lớn hơn`s`. Câu trả lời là chính xác`s`. 
4. Nếu không, hãy chọn từng loại nhỏ làm bộ khởi đầu. Tại thời điểm này, các loại lớn không thể đóng góp ba loại cho bất kỳ giải pháp khả thi nào, do đó, một giải pháp tối ưu cần nhiều nhất là hai loại lớn. 
5. Trong số các loại lớn, hãy tìm cặp khả thi với giá trị lớn nhất`r_i + r_j`. Đối với mọi loại thứ hai có thể, hãy tìm kiếm nhị phân chỉ mục lớn nhất có giới hạn dưới có thể vừa với nó. Tiền tố tối đa là`r`các giá trị sau đó mang lại cho đối tác tương thích tốt nhất. Cũng nên xem xét một loại lớn duy nhất, vì đôi khi không có cặp nào vừa vặn. 
6. Thêm một hoặc hai loại lớn đã chọn vào bộ khởi đầu nhỏ. Bản thân cặp lớn đã được chọn sao cho tổng giới hạn dưới của nó nhiều nhất là`s`, vì vậy nếu tập kết hợp quá lớn thì chỉ cần loại bỏ những loại nhỏ. Xóa giới hạn dưới nhỏ nhất cho đến khi tổng giới hạn dưới tối đa`s`. 
7. Để các loại được chọn còn lại có tổng giới hạn dưới`L`và tổng giới hạn trên`R`. Câu trả lời là`min(s, R)`. Nếu như`L <= s`, chúng ta có thể xây dựng câu trả lời chính xác bằng cách ban đầu chỉ định giới hạn dưới của mỗi loại đã chọn và sau đó phân bổ các thẻ còn lại trong các khoảng đã chọn cho đến khi đạt được mục tiêu. 
8. Nếu một loại bị loại bỏ, hãy ghi số 0 cho loại đó. Đối với mỗi loại được giữ lại, hãy chỉ định số lượng cuối cùng bằng cách sử dụng khoảng thời gian ban đầu để thứ tự đầu vào ban đầu được giữ nguyên. 

### Tại sao nó hoạt động 

Bất biến đằng sau thuật toán là sau khi phân chia loại lớn, mọi giải pháp khả thi đều đã chứa ba loại lớn, trong trường hợp đó, ba loại đầu tiên như vậy sẽ cho`s`, hoặc chứa tối đa hai loại lớn. Trường hợp đầu tiên được phát hiện trực tiếp. 

Trong trường hợp thứ hai, hãy xem xét bất kỳ giải pháp tối ưu nào. Phần lớn của nó không có, một hoặc hai loại. Nếu tồn tại loại lớn thứ hai tương thích, thì việc thêm nó chỉ làm tăng giới hạn trên có thể đạt được, do đó, tối ưu sử dụng một loại lớn sẽ bị chi phối bởi một số cặp khả thi trừ khi không tồn tại cặp như vậy. Do đó, thuật toán tìm phần lớn nhất có thể bằng cách tối đa hóa tổng của`r_i`. 

Đối với phần nhỏ, nếu tất cả các loại nhỏ đều khớp với phần lớn đã chọn thì việc giữ từng loại nhỏ luôn có lợi vì việc thêm một loại không thể làm giảm tổng số có thể đạt được. Nếu chúng không vừa, thuật toán sẽ loại bỏ các giới hạn dưới nhỏ nhất. Vì mỗi loại bị loại bỏ nhiều nhất là`2s/7`, tiền tố khả thi đầu tiên có tổng giới hạn dưới lớn hơn`5s/7`. Tổng giới hạn trên của nó khi đó lớn hơn`s`, sao cho việc xây dựng đạt đến mức tối đa tuyệt đối`s`. Danh tính của các loại nhỏ không còn quan trọng trong trường hợp này. Hai trường hợp này bao gồm mọi điều tối ưu có thể. 

## Giải pháp Python```python
import sys
from bisect import bisect_right

input = sys.stdin.readline

def solve():
    n, s = map(int, input().split())

    items = []
    for i in range(n):
        l, r = map(int, input().split())
        items.append((l, r, i))

    items.sort()

    # Small: 7 * l <= 2 * s
    cnt = 0
    while cnt < n and 7 * items[cnt][0] <= 2 * s:
        cnt += 1

    # If three smallest large types fit, they already force answer s.
    if cnt + 2 < n:
        l0 = items[cnt][0]
        l1 = items[cnt + 1][0]
        l2 = items[cnt + 2][0]

        if l0 + l1 + l2 <= s:
            selected = [cnt, cnt + 1, cnt + 2]
        else:
            selected = None
    else:
        selected = None

    # Otherwise we need all small types plus the best one/two large types.
    if selected is None:
        selected = list(range(cnt))

        large = items[cnt:]

        if large:
            m = len(large)
            ls = [x[0] for x in large]
            rs = [x[1] for x in large]

            # Prefix maximum r and its index.
            pref_r = [0] * m
            pref_id = [-1] * m

            best_r = -1
            best_id = -1

            for i in range(m):
                if rs[i] > best_r:
                    best_r = rs[i]
                    best_id = i
                pref_r[i] = best_r
                pref_id[i] = best_id

            # Best single large type that fits.
            k = bisect_right(ls, s) - 1
            best_value = -1
            best_pair = None

            if k >= 0:
                best_value = pref_r[k]
                best_pair = (pref_id[k],)

            # Best pair of large types.
            for i in range(m):
                if ls[i] > s:
                    break

                remaining = s - ls[i]
                k = bisect_right(ls, remaining) - 1

                if k < 0:
                    continue

                # We need a partner with index < i.
                k = min(k, i - 1)
                if k < 0:
                    continue

                value = rs[i] + pref_r[k]
                if value > best_value:
                    best_value = value
                    best_pair = (pref_id[k], i)

            if best_pair is not None:
                for x in best_pair:
                    selected.append(cnt + x)

        # Remove the smallest lower bounds until the set fits.
        selected.sort(key=lambda i: items[i][0])

        lower_sum = sum(items[i][0] for i in selected)

        while selected and lower_sum > s:
            x = selected.pop(0)
            lower_sum -= items[x][0]

    # The selected set is now feasible by lower bounds.
    lower_sum = sum(items[i][0] for i in selected)
    upper_sum = sum(items[i][1] for i in selected)

    target = min(s, upper_sum)

    ans = [0] * n
    remaining = target - lower_sum

    # Start from all lower bounds and distribute the remaining amount.
    for pos in selected:
        l, r, original = items[pos]
        add = min(r - l, remaining)
        ans[original] = l + add
        remaining -= add

    print(target)
    print(*ans)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai sắp xếp các khoảng và tìm ngưỡng bằng cách sử dụng số học số nguyên. Phép nhân với`7`an toàn trong Python và tránh được tất cả các vấn đề về độ chính xác liên quan đến việc biểu diễn`1.4`dưới dạng giá trị dấu phẩy động. 

Nhánh ba loại lớn được cố tình kiểm tra trước nhánh chung. Nếu thành công thì việc xây dựng đã đủ để đạt tới`s`, do đó không cần tìm kiếm theo cặp. 

Nhánh chung xây dựng cực đại tiền tố trên các loại lớn. Đối với loại lớn cố định`i`,`bisect_right`tìm mọi loại lớn có giới hạn dưới nhiều nhất`s - l_i`. Hạn chế chỉ số đối tác ở bên dưới`i`đảm bảo rằng cùng một loại không bao giờ được sử dụng hai lần. Tiền tố tối đa sau đó sẽ đưa ra loại tương thích với giới hạn trên lớn nhất. 

Ứng cử viên loại đơn là cần thiết vì có thể không có cặp khả thi nào. Nó cũng hoạt động như một phương án dự phòng khi chỉ tồn tại một loại lớn. 

Sau khi chọn phần lớn, mã sẽ sắp xếp các chỉ mục đã chọn theo giới hạn dưới của chúng và loại bỏ những chỉ mục nhỏ nhất nếu cần. Việc ra lệnh này là có chủ ý. Tất cả các loại lớn đều có giới hạn dưới lớn hơn mọi loại nhỏ, do đó, một cặp lớn khả thi sẽ được giữ nguyên trong khi các loại nhỏ sẽ bị loại bỏ trước tiên. 

Cuối cùng, việc xây dựng bắt đầu mọi loại được chọn tại`l_i`. Vì mục tiêu nhiều nhất là tổng giới hạn trên nên số tiền còn lại luôn có thể được phân phối mà không vượt quá bất kỳ mức nào.`r_i`. Mã xử lý các loại đã chọn theo thứ tự được sắp xếp nhưng ghi vào`ans[original]`, do đó đầu ra vẫn giữ nguyên thứ tự đầu vào. 

## Ví dụ đã hoạt động 

Mẫu được cung cấp là```
3 20
1 2
10 17
11 16
```Ngưỡng là`2s/7 = 40/7`, nên chỉ có loại thứ nhất là nhỏ. 

| Sân khấu | Giới hạn dưới được chọn | Giới hạn trên được chọn | Tiểu bang | 
| --- | --- | --- | --- | 
| Phân loại |`[1]`|`[2]`| Một nhỏ, hai lớn | 
| Séc ba lớn | không có sẵn | không có sẵn | Ít hơn ba loại lớn | 
| Sự lựa chọn lớn nhất |`[10]`|`[17]`| Đôi`10 + 11`không phù hợp | 
| Thêm loại nhỏ |`[1, 10]`|`[2, 17]`| tổng thấp hơn`11`| 
| Mục tiêu cuối cùng |`[1, 10]`|`[2, 17]`|`min(20, 19) = 19`| 

Nhiệm vụ cuối cùng có thể là`2 17 0`, đưa ra chính xác`19`thẻ. Ví dụ này chứng minh tại sao việc tối đa hóa giới hạn trên của hai loại lớn là không đủ: giới hạn dưới của chúng đã có tổng bằng`21`, vì vậy chỉ có thể sử dụng một trong số chúng. 

Một ví dụ thứ hai là```
5 10
1 2
2 3
2 3
6 9
7 10
```Ngưỡng là`20/7`, vậy ba loại đầu tiên là nhỏ và hai loại cuối cùng là lớn. 

| Sân khấu | Chỉ số được chọn | Tổng thấp hơn | Tổng trên | 
| --- | --- | --- | --- | 
| Loại nhỏ |`1, 2, 3`|`5`|`8`| 
| Kiểm tra cặp lớn |`6 + 7 = 13`| quá lớn | không khả thi | 
| Loại lớn tốt nhất |`7`|`7`|`10`| 
| Thêm loại nhỏ |`1, 2, 3, 5`|`12`|`18`| 
| Xóa giới hạn dưới nhỏ nhất |`2, 3, 5`|`11`|`16`| 
| Xóa giới hạn dưới nhỏ nhất tiếp theo |`3, 5`|`9`|`13`| 
| Mục tiêu cuối cùng |`3, 5`|`9`|`13`| 

Mục tiêu cuối cùng là`10`. Một bài tập hợp lệ là`0 0 3 0 7`. Phần quan trọng của dấu vết này là bản thân loại lớn được giữ nguyên trong khi loại nhỏ bị loại bỏ. Khi số tiền thấp hơn ở trên`5s/7`, bộ còn lại đảm bảo có đủ công suất trên để đạt`s`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Sắp xếp mất`O(n log n)`và mỗi loại lớn thực hiện một tìm kiếm nhị phân | 
| Không gian |`O(n)`| Các khoảng được sắp xếp, mảng tiền tố, chỉ mục được chọn và mảng câu trả lời đều là tuyến tính | 

Với`n <= 10^5`,`O(n log n)`có nghĩa là khoảng vài triệu phép tính cơ bản, trong khi tất cả số học liên quan đến`s`,`l_i`, Và`r_i`được xử lý trực tiếp dưới dạng số nguyên. Thuật toán không phân bổ bất cứ thứ gì tỷ lệ thuận với`s`, điều này là cần thiết bởi vì`s`có thể lớn như`10^13`. 

## Trường hợp thử nghiệm 

Bộ khai thác sau đây kiểm tra chính xác mẫu được cung cấp và sử dụng tính năng tìm kiếm toàn diện cho các trường hợp tùy chỉnh nhỏ. Vì được phép có nhiều đầu ra tối ưu nên các thử nghiệm tùy chỉnh sẽ xác thực phép gán được tạo và so sánh tổng của nó với giá trị tối ưu thực sự thay vì yêu cầu một vectơ cụ thể.```python
import sys
import io
from bisect import bisect_right
from itertools import product

def solve_io(data: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(data)
    sys.stdout = io.StringIO()

    # Paste the solve() function from the solution here.
    # In a real test file, import solve from the submitted solution instead.
    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

def brute_force(inp: str):
    it = iter(inp.split())
    n = int(next(it))
    s = int(next(it))

    a = []
    for _ in range(n):
        l = int(next(it))
        r = int(next(it))
        a.append((l, r))

    best = 0

    for mask in range(1 << n):
        low = 0
        high = 0

        for i in range(n):
            if mask >> i & 1:
                low += a[i][0]
                high += a[i][1]

        if low <= s:
            best = max(best, min(s, high))

    return best

def check(inp: str):
    out = solve_io(inp)
    tokens = list(map(int, out.split()))

    n, s = map(int, inp.splitlines()[0].split())

    w = tokens[0]
    ans = tokens[1:1 + n]

    assert len(ans) == n
    assert 0 <= w <= s
    assert sum(ans) == w

    lines = inp.splitlines()
    intervals = [tuple(map(int, line.split())) for line in lines[1:]]

    for x, (l, r) in zip(ans, intervals):
        assert x == 0 or l <= x <= r

    assert w == brute_force(inp)

# Provided sample.
sample1 = """\
3 20
1 2
10 17
11 16
"""

assert solve_io(sample1) == "19\n2 17 0\n", "sample 1"

# Minimum-size input.
sample2 = """\
1 5
3 5
"""
check(sample2)

# No type can be bought.
sample3 = """\
2 5
6 10
7 12
"""
check(sample3)

# Three large types fit and immediately force answer s.
sample4 = """\
4 20
1 2
6 9
7 10
8 12
"""
check(sample4)

# Pair of large types is impossible, but one large type plus
# some small types gives the optimum.
sample5 = """\
4 20
1 2
2 3
10 15
11 16
"""
check(sample5)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 20; 1 2; 10 17; 11 16`|`19`, Ví dụ`2 17 0`| Cung cấp mẫu và từ chối một cặp lớn không khả thi | 
|`1 5; 3 5`|`5`, với`0 5`| tối thiểu`n`và công suất chính xác | 
|`2 5; 6 10; 7 12`|`0`, với`0 0`| Loại có giới hạn dưới vượt quá dung lượng | 
|`4 20; 1 2; 6 9; 7 10; 8 12`|`20`| Nhánh ba loại lớn | 
|`4 20; 1 2; 2 3; 10 15; 11 16`|`20`| Ranh giới cặp lớn và tái thiết | 

Các thử nghiệm tùy chỉnh chỉ sử dụng phép liệt kê đầy đủ bên trong khai thác thử nghiệm, nơi các phiên bản rất nhỏ. Thuật toán được gửi không bao giờ thực hiện phép liệt kê này. 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là loại có số lượng mua tối thiểu đã vượt quá dung lượng. Coi như```
1 5
6 10
```Việc phân loại đặt loại vào danh mục lớn, nhưng giới hạn dưới của nó lớn hơn`s`, vì vậy nó không thể là một phần của bất kỳ lựa chọn cặp hoặc loại đơn khả thi nào. Bộ đã chọn vẫn trống và đầu ra là```
0
0
```Trường hợp cạnh thứ hai là mẫu được cung cấp, trong đó hai loại lớn khớp riêng lẻ nhưng số lượng tối thiểu của chúng không khớp với nhau:```
3 20
1 2
10 17
11 16
```Cặp lớn có tổng nhỏ hơn`21`, vì vậy việc tìm kiếm cặp từ chối nó. Sự lựa chọn lớn tốt nhất là`[10,17]`và thêm loại nhỏ`[1,2]`cho số tiền thấp hơn`11`và tổng trên`19`. Tổng kết quả đầu ra của thuật toán`19`. 

Trường hợp cạnh thứ ba là khi có ba loại lớn phù hợp:```
4 20
1 2
6 9
7 10
8 12
```Ba giới hạn dưới lớn tổng hợp thành`21`, vì vậy đầu vào cụ thể này không thực sự đi vào nhánh ba lớn. Ranh giới này rất hữu ích vì nó xác nhận việc so sánh là bao hàm: thuật toán yêu cầu tổng của ba giới hạn dưới nhiều nhất là`s`. Nếu đầu vào được thay đổi thành```
4 20
1 2
6 9
6 10
7 11
```ba giới hạn dưới lớn tổng hợp thành`19`. Giới hạn trên của chúng tổng cộng là`30`, vậy đáp án chính xác là`20`. 

Trường hợp cạnh thứ tư là khi cộng tất cả các loại nhỏ ban đầu vượt quá`s`. Giả định```
4 10
1 100
2 3
6 9
7 10
```Loại nhỏ có tổng nhỏ hơn`3`. Lựa chọn lớn nhất có thể chứa loại có giới hạn dưới`7`và cộng mọi loại nhỏ sẽ có tổng thấp hơn`10`, điều này đã khả thi rồi. Nếu có một loại nhỏ khác xuất hiện và gây ra tràn, thuật toán sẽ loại bỏ các giới hạn dưới nhỏ nhất trước tiên. Bởi vì mọi giới hạn dưới nhỏ bị loại bỏ nhiều nhất là`2s/7`, tập còn lại khả thi đầu tiên sẽ có tổng nhỏ hơn`5s/7`, làm cho tổng trên của nó đủ để đạt`s`. 

Trường hợp cạnh cuối cùng liên quan đến số học số nguyên ở ngưỡng. Việc phân loại sử dụng```
7 * l_i <= 2 * s
```còn hơn là`l_i <= 2 * s / 7`với dấu phẩy động. Một giá trị chính xác bằng`2s/7`thuộc về nhóm nhỏ. Việc phân loại sai ranh giới đẳng thức có thể thay đổi xem ba loại lớn có được xem xét hay không, do đó, dạng số nguyên sẽ tránh được cả lỗi làm tròn và lỗi sai lệch một.
