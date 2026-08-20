---
title: "CF 102191D - Ngày Hình Ảnh"
description: "Chúng ta có số học sinh chẵn và mỗi hai học sinh gắn bó với nhau như một cặp tình bạn. Hàng cuối cùng phải là bitonic: chiều cao có thể giữ nguyên hoặc tăng đối với một số tiền tố và sau đó chúng có thể giữ nguyên hoặc giảm đi."
date: "2026-08-20T01:11:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102191
codeforces_index: "D"
codeforces_contest_name: "PSUT Coding Marathon 2019"
rating: 0
weight: 102191
solve_time_s: 448
verified: false
draft: false
---

[CF 102191D - Ngày hội hình ảnh](https://codeforces.com/problemset/problem/102191/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 28 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có số học sinh chẵn và mỗi hai học sinh gắn bó với nhau như một cặp tình bạn. Hàng cuối cùng phải là bitonic: chiều cao có thể giữ nguyên hoặc tăng đối với một số tiền tố và sau đó chúng có thể giữ nguyên hoặc giảm đi. Hai học sinh thuộc một cặp phải xếp ở các vị trí liên tiếp nhau, nhưng được phép xếp thứ tự trong cặp của mình. 

Đối với mỗi cặp tình bạn, hãy sắp xếp nội bộ hai chiều cao của nó và biểu thị nó dưới dạng một khoảng`[l, r]`, Ở đâu`l`là học sinh thấp hơn và`r`là học sinh cao hơn. Nhiệm vụ là sắp xếp các khoảng này và chọn hướng của chúng sao cho chuỗi kết quả là bitonic. 

Quan sát quan trọng là một bức tranh hợp lệ có thể được xem như hai chuỗi khoảng cách độc lập. Một chuỗi được đặt ở phía tăng dần, với mỗi khoảng được viết là`l, r`. Chuỗi còn lại được đặt ở phía giảm dần, với các khoảng được lấy theo thứ tự ngược lại và được viết là`r, l`. 

Hai khoảng có thể thuộc về cùng một chuỗi một cách chính xác khi khoảng đầu tiên kết thúc trước khi chuỗi thứ hai bắt đầu. Vì`[l1, r1]`theo sau là`[l2, r2]`, điều này có nghĩa`r1 <= l2`. Sự bình đẳng được cho phép vì hình ảnh không giảm hoặc không tăng, không hoàn toàn đơn điệu. 

Ràng buộc`n <= 3 * 10^5`cho nhiều nhất`150000`cặp đôi tình bạn. Một thuật toán bậc hai sẽ thực hiện khoảng`2.25 * 10^10`so sánh cặp trong trường hợp xấu nhất, quá nhiều so với giới hạn 2 giây. Chúng tôi cần một`O(n log n)`giải pháp, trong đó hệ số logarit xuất phát từ việc sắp xếp. 

Có một số trường hợp ranh giới dễ bỏ sót. Đầu tiên, những khoảng thời gian chỉ chạm vào là tương thích. Ví dụ,```
3
1 3
3 5
5 7
```có thể sản xuất`1 3 3 5 5 7`, bởi vì chiều cao liền kề bằng nhau được cho phép. Việc thực hiện bất cẩn bằng cách sử dụng`r < l`sẽ từ chối nó một cách không chính xác. 

Thứ hai, chiều cao bằng nhau không gây khó khăn gì đặc biệt. Vì```
4
3 3
3 3
```câu trả lời`3 3 3 3`là hợp lệ. Mã giả định mọi khoảng thời gian đều có độ dài dương sẽ bị lỗi một cách không cần thiết. 

Thứ ba, không thể xếp ba cặp chồng lên nhau thành hai mặt đơn điệu. Ví dụ,```
6
1 10
2 9
3 8
```là không thể bởi vì mỗi cặp chồng lên nhau. Thuật toán tham lam phải phát hiện cả hai chuỗi có sẵn đều bị chặn khi xử lý`[3, 8]`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực coi mỗi cặp tình bạn là một khối, thử mọi hoán vị của`n/2`chặn và thử cả hai hướng cho mọi khối. có`(n/2)! * 2^(n/2)`khả năng sắp xếp của các khối và hướng của chúng. Kiểm tra một sự sắp xếp mất`O(n)`thời gian, vậy tổng công việc trong trường hợp xấu nhất là`O(n * (n/2)! * 2^(n/2))`. Ở kích thước đầu vào tối đa, điều này có nghĩa là gần như`300000 * 150000! * 2^150000`so sánh chiều cao, điều này hoàn toàn không khả thi. 

Lực lượng vũ phu hoạt động vì nó khám phá rõ ràng mọi sự phân chia có thể có giữa phần tăng và phần giảm. Nó thất bại vì số lượng lệnh chặn có thể tăng lên theo giai đoạn. 

Quan sát cấu trúc hữu ích là chúng ta thực sự không cần phải quyết định đỉnh trước. Khi mỗi cặp được viết dưới dạng một khoảng`[l, r]`, hãy cân nhắc việc đặt một số cặp ở phía tăng dần. Các khoảng của chúng phải xuất hiện từ trái sang phải mà không trùng nhau nên chúng tạo thành một chuỗi thỏa mãn`r_previous <= l_current`. Điều tương tự cũng đúng đối với mặt giảm nếu chúng ta đọc mặt đó từ đỉnh về cuối. 

Do đó, toàn bộ vấn đề trở thành: phân chia tất cả các khoảng thành tối đa hai chuỗi không chồng chéo. 

Sau khi tìm thấy một phân vùng như vậy, giả sử một chuỗi được`[l1, r1], [l2, r2], ...`với`r1 <= l2 <= ...`. Chúng tôi viết nó trực tiếp, đưa ra`l1, r1, l2, r2, ...`đó là không giảm. 

Đối với chuỗi còn lại, giả sử các khoảng của nó theo thứ tự tăng dần là`[a1, b1], [a2, b2], ...`. 

Chúng tôi đảo ngược chuỗi và đảo ngược từng cặp, cho`b_k, a_k, ..., b2, a2, b1, a1`. 

Dãy số đó không tăng. Tại ranh giới giữa hai chuỗi, một giá trị được theo sau bởi một giá trị khác. Nếu giá trị bên trái nhỏ hơn thì đỉnh nằm ở bên phải; nếu nó lớn hơn thì đỉnh nằm ở bên trái. Dù bằng cách nào thì chuỗi hoàn chỉnh là bitonic. 

Vấn đề còn lại là phân chia các khoảng thành hai chuỗi một cách hiệu quả. Sắp xếp chúng theo điểm cuối bên trái của chúng. Trong khi xử lý một khoảng`[l, r]`, mỗi chuỗi được biểu thị bằng điểm cuối bên phải của khoảng cuối cùng của nó. Một chuỗi khả dụng nếu điểm cuối bên phải cuối cùng của nó nhiều nhất là`l`. 

Nếu cả hai chuỗi đều có sẵn, chúng tôi sẽ đặt khoảng mới vào chuỗi có điểm cuối bên phải cuối cùng lớn hơn. Điều này bảo toàn chuỗi có điểm cuối nhỏ hơn cho các khoảng thời gian trong tương lai, mang lại cho các khoảng thời gian trong tương lai sự linh hoạt lớn nhất có thể. Nếu chỉ có một chuỗi thì chúng ta phải sử dụng nó. Nếu không có chuỗi nào thì ba khoảng sẽ chồng lên nhau ở vị trí hiện tại, do đó hai chuỗi là không đủ và không có câu trả lời nào tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n * (n/2)! * 2^(n/2))`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biến mọi cặp tình bạn thành khoảng thời gian`[l, r]`với`l <= r`. Thứ tự ban đầu của hai học sinh là không liên quan, vì vậy chỉ có chiều cao ngắn hơn và cao hơn mới quan trọng khi dựng chuỗi. 
2. Sắp xếp tất cả các khoảng bằng cách tăng dần`l`, sử dụng`r`làm khóa phụ nếu muốn. Điều này có nghĩa là khi`[l, r]`được xử lý, mọi khoảng thời gian có thể đứng trước nó trong cùng một chuỗi đều đã được xem xét. 
3. Duy trì hai chuỗi. Đối với mỗi chuỗi, lưu trữ điểm cuối bên phải của khoảng thời gian cuối cùng của nó. Ban đầu cả hai điểm cuối đều có giá trị âm vô cùng vì một trong hai chuỗi có thể chấp nhận khoảng đầu tiên. 
4. Đối với khoảng thời gian hiện tại`[l, r]`, kiểm tra chuỗi nào thỏa mãn`last_right <= l`. Một chuỗi như vậy có thể nối thêm khoảng thời gian một cách an toàn mà không phá vỡ tính đơn điệu. 
5. Nếu có sẵn cả hai dây chuyền, hãy chọn dây chuyền có kích thước lớn hơn`last_right`. Điểm cuối nhỏ hơn sẽ hữu ích hơn cho các khoảng thời gian trong tương lai, do đó, việc giữ nguyên điểm cuối sẽ giúp các khoảng thời gian còn lại có nhiều khoảng trống hơn để phù hợp. 
6. Nếu có chính xác một chuỗi, hãy nối thêm khoảng thời gian vào đó. Không có giải pháp thay thế hữu ích nào vì đặt nó vào chuỗi khác sẽ ngay lập tức tạo ra sự chồng chéo. 
7. Nếu không có dây chuyền nào, hãy trả lại`-1`. Cả hai chuỗi đã kết thúc sau`l`, do đó khoảng hiện tại chồng lên một khoảng trong mỗi chuỗi. Ba khoảng chồng chéo yêu cầu ba chuỗi, trong khi một bức ảnh hợp lệ chỉ cung cấp hai cạnh của đỉnh. 
8. Lưu trữ các chỉ số khoảng được gán cho mỗi chuỗi. Khi tất cả các khoảng đã được chỉ định, hãy xuất chuỗi đầu tiên theo thứ tự được sắp xếp của nó dưới dạng`l, r`cho mỗi khoảng thời gian. 
9. Xuất chuỗi thứ hai theo thứ tự ngược lại, viết mỗi khoảng là`r, l`. Chiều cao của nó bây giờ giảm dần về cuối bức tranh. 
10. Nối hai chuỗi. Mỗi cặp tình bạn vẫn liền kề, phần thứ nhất không giảm và phần thứ hai không tăng. 

### Tại sao nó hoạt động 

Bất biến là sau khi xử lý bất kỳ tiền tố nào của các khoảng được sắp xếp theo`l`, mỗi chuỗi được duy trì là một chuỗi hợp lệ không chồng chéo và trong số tất cả các lựa chọn tham lam, thuật toán bảo toàn điểm cuối chuỗi có sẵn nhỏ nhất có thể. Khi cả hai chuỗi có thể chấp nhận một khoảng, việc gán nó cho chuỗi có điểm cuối lớn hơn sẽ giữ nguyên điểm cuối nhỏ hơn, điều này chỉ có thể giúp việc đặt các khoảng trong tương lai dễ dàng hơn. Nếu cả hai chuỗi đều không thể chấp nhận khoảng thời gian thì mọi phân vùng hai chuỗi có thể có đều phải có một khoảng kéo dài qua`l`trong cả hai chuỗi, vì vậy khoảng thời gian hiện tại không thể được chỉ định ở bất kỳ đâu. Do đó, quy trình tham lam thành công chính xác khi tồn tại một phân vùng hai chuỗi. 

Phân vùng hai chuỗi cũng chính là thứ mà một bức ảnh hợp lệ cần. Đọc phía tăng từ trái sang phải sẽ cho ra một chuỗi khoảng không chồng chéo, trong khi đọc phía giảm dần từ đỉnh ra ngoài sẽ cho một chuỗi khác. Ngược lại, hai chuỗi như vậy luôn có thể được kết hợp thành một chuỗi bit bằng cách viết một chuỗi tiến và một chuỗi ngược. Ranh giới giữa chúng có thể tiếp tục tăng hoặc bắt đầu giảm, do đó một trong hai vị trí nhất thiết phải là đỉnh hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    m = n // 2

    pairs = []
    for idx in range(m):
        a, b = map(int, input().split())
        if a <= b:
            pairs.append((a, b, idx))
        else:
            pairs.append((b, a, idx))

    pairs.sort()

    chains = [[], []]
    last = [-1, -1]

    for l, r, idx in pairs:
        can0 = last[0] <= l
        can1 = last[1] <= l

        if not can0 and not can1:
            print(-1)
            return

        if can0 and can1:
            if last[0] >= last[1]:
                c = 0
            else:
                c = 1
        elif can0:
            c = 0
        else:
            c = 1

        chains[c].append((l, r))
        last[c] = r

    ans = []

    for l, r in chains[0]:
        ans.append(l)
        ans.append(r)

    for l, r in reversed(chains[1]):
        ans.append(r)
        ans.append(l)

    print(*ans)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào trước tiên bình thường hóa từng cặp tình bạn. Việc giữ chỉ mục cặp ban đầu là không cần thiết vì đầu ra chỉ yêu cầu chiều cao chứ không yêu cầu danh tính học sinh. 

Sau khi sắp xếp,`last[0]`Và`last[1]`là điểm cuối bên phải của các khoảng cuối cùng trong hai chuỗi. điều kiện`last[c] <= l`chính xác là điều kiện không chồng chéo. Việc sử dụng`<=`, còn hơn là`<`, xử lý các khoảng thời gian chạm như`[1, 3]`Và`[3, 5]`. 

Khi cả hai chuỗi đều có sẵn, so sánh`last[0]`Và`last[1]`chọn điểm cuối lớn hơn. Đây là sự lựa chọn tham lam nhằm bảo toàn điểm cuối nhỏ hơn cho những khoảng thời gian sau này. Nhiệm vụ được lưu trữ dưới dạng các khoảng thời gian chuẩn hóa thực tế, do đó việc xây dựng lại không cần phải quay lại đầu vào đã sắp xếp. 

Chuỗi đầu tiên được phát ra trực tiếp. Vì các khoảng của nó được sắp xếp theo điểm cuối bên trái của chúng và không chồng chéo theo cặp nên trình tự của nó không giảm. Chuỗi thứ hai được đi ngược lại và mỗi cặp được phát ra theo chiều cao hơn rồi ngắn hơn. Điều này làm cho toàn bộ phần đó không tăng. 

Số nguyên Python có độ chính xác tùy ý, do đó giới hạn chiều cao của`10^9`không cần xử lý số nguyên đặc biệt. 

## Ví dụ đã hoạt động 

Đối với mẫu được cung cấp, khoảng thời gian chuẩn hóa là`[1, 3]`,`[2, 4]`,`[6, 7]`, Và`[5, 7]`. Chúng đã được sắp xếp theo điểm cuối bên trái của chúng. 

| Khoảng thời gian | Chuỗi 0 cuối | Chuỗi 1 đầu | Chuỗi được chọn | 
| --- | --- | --- | --- | 
|`[1, 3]`|`-1`|`-1`| 0 | 
|`[2, 4]`|`3`|`-1`| 1 | 
|`[5, 7]`|`3`|`4`| 1 | 
|`[6, 7]`|`3`|`7`| 0 | 

Chuỗi đầu tiên trở thành`[[1,3], [6,7]]`, sản xuất`1 3 6 7`. Thứ hai trở thành`[[2,4], [5,7]]`; đảo ngược nó và đảo ngược từng cặp mang lại`7 5 4 2`. Trình tự kết hợp là`1 3 6 7 7 5 4 2`, đó là một câu trả lời hợp lệ. Đầu ra mẫu sử dụng một phân vùng hợp lệ khác và được chấp nhận như nhau. 

Đối với ví dụ thứ hai, hãy xem xét```
6
1 3
3 5
5 7
```Các khoảng đã không chồng chéo lên nhau, vì vậy thuật toán tham lam có thể giữ cả ba khoảng trong một chuỗi. 

| Khoảng thời gian | Chuỗi 0 cuối | Chuỗi 1 đầu | Chuỗi được chọn | 
| --- | --- | --- | --- | 
|`[1,3]`|`-1`|`-1`| 0 | 
|`[3,5]`|`3`|`-1`| 0 | 
|`[5,7]`|`5`|`-1`| 0 | 

Kết quả là`1 3 3 5 5 7`. Mỗi cặp vẫn liền kề và chuỗi không giảm. Kiểm tra tính bằng nhau là điều làm cho điểm cuối chạm hợp lệ. 

Đối với một trường hợp không thể,```
6
1 10
2 9
3 8
```dấu vết là: 

| Khoảng thời gian | Chuỗi 0 cuối | Chuỗi 1 đầu | Chuỗi được chọn | 
| --- | --- | --- | --- | 
|`[1,10]`|`-1`|`-1`| 0 | 
|`[2,9]`|`10`|`-1`| 1 | 
|`[3,8]`|`10`|`9`| không | 

Tại`[3,8]`, cả hai khoảng thời gian trước đó đều kéo dài hơn`3`, nên không dây chuyền nào có thể chấp nhận nó. Thuật toán in`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| có`n/2`khoảng thời gian, việc sắp xếp chiếm ưu thế trong đường chuyền tham lam tuyến tính. | 
| Không gian |`O(n)`| Khoảng thời gian chuẩn hóa, hai chuỗi và đầu ra chứa`O(n)`các giá trị. | 

Với nhiều nhất`150000`cặp, sắp xếp`150000`các khoảng thời gian dễ dàng nằm trong phạm vi dự kiến ​​​​với giới hạn 2 giây trong Python, trong khi mức sử dụng bộ nhớ vẫn tuyến tính và dưới 256 MB. 

## Trường hợp thử nghiệm 

Bộ khai thác thử nghiệm bên dưới sử dụng cùng một thuật toán thông qua giao diện chức năng và xác thực cách sắp xếp được trả về thay vì so sánh với một cách sắp xếp cố định. Điều này là cần thiết vì bài toán chấp nhận bất kỳ hình ảnh hợp lệ nào.```python
# helper: run solution on input string, return output string
import sys
import io

def solve_data(inp: str) -> str:
    data = inp.split()
    it = iter(data)

    n = int(next(it))
    m = n // 2

    pairs = []
    for idx in range(m):
        a = int(next(it))
        b = int(next(it))
        if a <= b:
            pairs.append((a, b, idx))
        else:
            pairs.append((b, a, idx))

    pairs.sort()

    chains = [[], []]
    last = [-1, -1]

    for l, r, idx in pairs:
        can0 = last[0] <= l
        can1 = last[1] <= l

        if not can0 and not can1:
            return "-1\n"

        if can0 and can1:
            c = 0 if last[0] >= last[1] else 1
        elif can0:
            c = 0
        else:
            c = 1

        chains[c].append((l, r))
        last[c] = r

    ans = []

    for l, r in chains[0]:
        ans.extend((l, r))

    for l, r in reversed(chains[1]):
        ans.extend((r, l))

    return " ".join(map(str, ans)) + "\n"

def run(inp: str) -> str:
    return solve_data(inp)

def valid_output(inp: str, out: str) -> bool:
    tokens = inp.split()
    n = int(tokens[0])
    vals = list(map(int, tokens[1:]))

    if out.strip() == "-1":
        return False

    ans = list(map(int, out.split()))
    if len(ans) != n:
        return False

    # Check that every input pair appears as adjacent heights.
    pairs = []
    for i in range(n // 2):
        a = vals[2 * i]
        b = vals[2 * i + 1]
        pairs.append(tuple(sorted((a, b))))

    used = [False] * (n // 2)
    for i in range(0, n, 2):
        p = tuple(sorted((ans[i], ans[i + 1])))
        found = False
        for j, q in enumerate(pairs):
            if not used[j] and p == q:
                used[j] = True
                found = True
                break
        if not found:
            return False

    # Check bitonicity.
    direction = 1
    for i in range(1, n):
        if direction == 1:
            if ans[i] < ans[i - 1]:
                direction = -1
        else:
            if ans[i] > ans[i - 1]:
                return False

    return True

# Provided sample
sample1 = """\
8
1 3
4 2
6 7
5 7
"""
out = run(sample1)
assert valid_output(sample1, out), "sample 1"

# Minimum-size input
case2 = """\
2
1000000000 1
"""
out = run(case2)
assert valid_output(case2, out), "minimum size"

# All equal values
case3 = """\
8
3 3
3 3
3 3
3 3
"""
out = run(case3)
assert valid_output(case3, out), "all equal"

# Touching interval boundaries
case4 = """\
6
1 3
3 5
5 7
"""
out = run(case4)
assert valid_output(case4, out), "touching boundaries"

# Three mutually overlapping intervals, impossible
case5 = """\
6
1 10
2 9
3 8
"""
assert run(case5).strip() == "-1", "three overlapping intervals"

# Maximum-size input
m = 150000
parts = [str(2 * m)]
for i in range(m):
    parts.append(f"{2 * i + 1} {2 * i + 2}")
case6 = "\n".join(parts) + "\n"

out = run(case6)
assert valid_output(case6, out), "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1000000000 1`| Bất kỳ sự sắp xếp hai yếu tố hợp lệ nào | Kích thước tối thiểu và cặp đầu vào đảo ngược | 
| Bốn cặp`3 3`|`3 3 3 3 3 3 3 3`theo một thứ tự hợp lệ nào đó | Khoảng thời gian bằng 0 và đẳng thức | 
|`[1,3], [3,5], [5,7]`| Bất kỳ sự sắp xếp không giảm hợp lệ nào |`<=`điều kiện biên | 
|`[1,10], [2,9], [3,8]`|`-1`| Thất bại khi hai chuỗi không đủ | 
|`150000`cặp rời rạc | Bất kỳ sự sắp xếp hợp lệ nào của tất cả`300000`độ cao | Kích thước đầu vào tối đa và`O(n log n)`hiệu suất | 

## Vỏ cạnh 

Đối với trường hợp kích thước tối thiểu```
2
1000000000 1
```chỉ có một khoảng,`[1,1000000000]`. Chuỗi đầu tiên ban đầu có sẵn nên cặp này được đặt ở đó và phát ra dưới dạng`1 1000000000`. Một cặp duy nhất luôn là một chuỗi bitonic hợp lệ. 

Để có độ cao bằng nhau,```
8
3 3
3 3
3 3
3 3
```mỗi khoảng thời gian là`[3,3]`. Mỗi khoảng có thể được thêm vào một trong hai chuỗi vì`3 <= 3`. Thủ tục tham lam tiếp tục đặt chúng vào một chuỗi có sẵn và chuỗi kết quả bao gồm toàn bộ`3`S. Cả hai điều kiện tăng và giảm đều đồng thời tồn tại. 

Đối với khoảng thời gian chạm,```
6
1 3
3 5
5 7
```khoảng thời gian đầu tiên kết thúc tại`3`, chính xác là điểm cuối bên trái của giây. điều kiện`last_right <= l`chấp nhận nó, do đó các khoảng tạo thành một chuỗi. Trình tự được tạo ra là`1 3 3 5 5 7`, chứng minh tại sao bất đẳng thức nghiêm ngặt sẽ không chính xác. 

Đối với trường hợp không thể,```
6
1 10
2 9
3 8
```cặp đầu tiên chiếm chuỗi 0, tạo cho nó điểm cuối`10`. Cặp thứ hai không thể vừa với chuỗi 0 vì`10 > 2`, vì vậy nó đi tới chuỗi 1 và cung cấp cho nó điểm cuối`9`. Khi`[3,8]`đến, cả hai điểm cuối đều lớn hơn`3`. Cả hai chuỗi đều không thể chấp nhận nó nên thuật toán sẽ in chính xác`-1`. 

Đối với trường hợp kích thước tối đa, tất cả`150000`các khoảng rời rạc và được sắp xếp theo điểm cuối bên trái của chúng. Đường chuyền tham lam gán chúng vào một chuỗi và cấu trúc cuối cùng tạo ra tất cả`300000`độ cao theo một chuỗi đơn điệu hợp lệ. Phần tốn kém nhất là việc phân loại, việc này đòi hỏi`O(n log n)`, trong khi bản thân việc xây dựng là tuyến tính.
