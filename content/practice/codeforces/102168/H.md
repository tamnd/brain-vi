---
title: "CF 102168H - \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u0430\u044f \u0434\u0438\u043b\u0435\u043c\u043c\u0430"
description: "Chúng tôi có n người và hai loại giường. Có một giường đơn, nơi bất cứ ai cũng có thể ngủ một mình, và b giường đôi, nơi hai người có thể ngủ chung giường. Mỗi người được mô tả bằng một giá trị nhị phân."
date: "2026-08-19T07:27:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102168
codeforces_index: "H"
codeforces_contest_name: "\u041b\u0438\u0447\u043d\u044b\u0439 \u0447\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0421\u0430\u043c\u0430\u0440\u0441\u043a\u043e\u0433\u043e \u0443\u043d\u0438\u0432\u0435\u0440\u0441\u0438\u0442\u0435\u0442\u0430 \u0441\u0440\u0435\u0434\u0438 \u043d\u043e\u0432\u0438\u0447\u043a\u043e\u0432 2018-2019"
rating: 0
weight: 102168
solve_time_s: 98
verified: true
draft: false
---

[CF 102168H - \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u0430\u044f \u0434\u0438\u043b\u0435\u043c\u043c\u0430](https://codeforces.com/problemset/problem/102168/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

chúng tôi có`n`người và hai loại giường. có`a`giường đơn, nơi ai cũng có thể ngủ một mình, và`b`giường đôi, nơi hai người có thể ngủ chung giường. Mỗi người được mô tả bằng một giá trị nhị phân. Một người có giá trị`1`đồng ý chia sẻ giường đôi với người khác, trong khi người có giá trị`0`không chịu chia sẻ và chỉ có thể ngủ một mình. 

Chúng ta cần xây dựng một bài tập nếu có. Mỗi giường đơn nhận được một người hoặc`0`, và mỗi giường đôi chỉ nhận được không, một hoặc hai người. Một người phải xuất hiện đúng một lần và`0`người đó không bao giờ được đặt cùng với người khác trên một chiếc giường đôi. 

Các ràng buộc cho phép lên đến`200000`mọi người và lên đến`200000`tổng số giường. Do đó, một giải pháp phải tuyến tính hoặc gần tuyến tính. Bất cứ điều gì bậc hai trong`n`có thể đã yêu cầu xung quanh`4 * 10^10`hoạt động ở giới hạn trên, vượt xa giới hạn hai giây. Cũng không có lý do gì để sử dụng các thuật toán đồ thị phức tạp ở đây, bởi vì quy tắc tương thích chỉ có hai loại và mọi người sẵn sàng đều tương thích với mọi người sẵn sàng khác. 

Một số trường hợp ranh giới rất dễ bị xử lý sai. Coi như```
1 0 1
0
```Câu trả lời là`YES`, với giường đôi có`0 0`, vì người duy nhất không thể ở chung và không có giường đơn nên thực ra trường hợp này là`NO`. Việc thực hiện bất cẩn coi giường đôi như có hai nơi độc lập có thể khiến người bệnh ở đó một mình một cách không chính xác. 

Bây giờ hãy xem xét```
2 1 0
00
```Câu trả lời là`NO`, vì cả hai người đều yêu cầu giường đơn nhưng chỉ có một chiếc. Chỉ kiểm tra tổng số tư thế ngủ vật lý ít nhất là`n`sẽ không đủ nếu các vị trí giường đôi được tính độc lập. 

Một trường hợp ranh giới khác là```
3 1 1
011
```Điều này là khả thi. Người`1`phải sử dụng giường đơn, trong khi người`2`Và`3`có thể chia sẻ giường đôi. Việc thực hiện tham lam đặt một người sẵn sàng vào giường đơn trước tiên có thể khiến người không sẵn lòng không có chỗ đứng, mặc dù đã có sự phân công hợp lệ. 

Cuối cùng, hãy xem xét```
3 0 2
111
```Điều này là khả thi. Một giường đôi chứa hai người và giường kia chứa một người. Một giường đôi không nhất thiết phải được lấp đầy hoàn toàn, vì vậy việc yêu cầu mỗi giường đôi chứa đúng hai người sẽ từ chối sự sắp xếp hợp lệ. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu có thể thử phân công mọi người vào giường và kiểm tra xem mọi phân công có tôn trọng các quy tắc chia sẻ hay không. Ngay cả khi chúng tôi đơn giản hóa việc tìm kiếm một cách triệt để và chỉ chọn những người ở giường đơn, thì vẫn có`2^n`tập hợp con có thể. Vì`n = 200000`, tức là xấp xỉ`2^200000`những khả năng vượt xa bất cứ điều gì có thể được liệt kê. Cố gắng liệt kê các cặp hoàn chỉnh sẽ còn tệ hơn. 

Lý do vũ lực là không cần thiết là vì mối quan hệ tương thích cực kỳ đơn giản. Một người với`0`có chính xác một hạn chế: họ không thể ngủ chung giường đôi. Một người với`1`không có hạn chế khi đặt trên giường đôi. Theo đó, mọi`0`người nên được dành riêng cho một giường đơn. Một khi đã bố trí xong tất cả những người như vậy thì những người còn lại đều sẵn sàng chia sẻ nên dung lượng còn lại có thể được lấp đầy một cách tham lam. 

Điều này ngay lập tức đưa ra hai điều kiện khả thi. Thứ nhất, số lượng người không sẵn lòng không thể vượt quá`a`, bởi vì mỗi người trong số họ cần một chiếc giường đơn riêng biệt. Thứ hai, tổng số người không được vượt quá tổng số chỗ ngủ, tức là`a + 2b`. Những điều kiện này cũng đủ. Đặt mọi người không muốn vào một giường đơn, sau đó sử dụng bất kỳ giường đơn nào còn lại cho những người sẵn sàng, và cuối cùng phân phát tất cả những người sẵn sàng còn lại trên các giường đôi, mỗi lần hai giường. 

Quan sát quan trọng là không có sự tương tác giữa những người có thiện chí khác nhau. Một khi tất cả những người không muốn đều đã được bảo vệ trên những chiếc giường đơn, thì hai người còn lại bất kỳ đều tương thích, vì vậy không bao giờ cần phải xem xét lại lựa chọn trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) hoặc tệ hơn | O(n) | Quá chậm | 
| Xây dựng tham lam | O(n + a + b) | O(n + a + b) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`a`,`b`và chia người thành hai nhóm theo sợi dây. Lưu trữ chỉ số của những người không muốn và những người sẵn sàng để việc xây dựng có thể xử lý trực tiếp từng danh mục. 
2. Nếu số người không muốn lớn hơn`a`, in`NO`. Mỗi người không muốn đều cần một chiếc giường đơn riêng nên không có sự sắp xếp nào có thể tránh được yêu cầu này. 
3. Nếu`n > a + 2b`, in`NO`. chỉ có`a + 2b`chỗ ngủ thực tế và mỗi người đều cần một trong số đó. 
4. Tạo đầu ra cho`a`giường đơn. Đầu tiên hãy đặt tất cả những người không muốn vào giường đơn. Sau đó, sử dụng số giường đơn còn lại cho những người có thiện chí. 
5. Đưa những người sẵn sàng vẫn chưa được phân công vào làm việc`b`giường đôi hai người một lúc. Nếu chỉ còn lại một người sẵn sàng ở giường cuối cùng, hãy đặt người đó vào một vị trí và`0`trong cái khác. 
6. Mỗi vị trí giường đơn chưa sử dụng sẽ nhận được`0`, và mọi vị trí không sử dụng trên giường đôi cũng nhận được`0`. In`YES`tiếp theo là nhiệm vụ được xây dựng. 

### Tại sao nó hoạt động 

Điều bất biến là mọi người đã được phân vào một giường đơn sẽ được an toàn vĩnh viễn vì một giường đơn không bao giờ cần phải dùng chung. Quan trọng hơn, sau khi tất cả những người không muốn được phân công, mọi người không được phân công đều sẵn sàng chia sẻ. Vì vậy, bất kỳ cặp nào được chọn cho giường đôi đều hợp lệ. 

Nếu thuật toán từ chối vì có nhiều người không muốn hơn giường đơn, thì không có giải pháp nào tồn tại vì mỗi người trong số đó đều cần một chiếc giường riêng. Nếu nó từ chối vì`n > a + 2b`, không có đủ chỗ ngủ vật lý bất kể khả năng tương thích. Ngược lại, khi cả hai điều kiện đều đạt, tất cả những người không muốn đều nằm trên giường đơn, những người còn lại đều sẵn lòng, nên sức chứa đơn và đôi còn lại đủ để đặt tất cả mọi người. Do đó, việc xây dựng thành công chính xác khi có giải pháp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, a, b = map(int, input().split())
    s = input().strip()

    unwilling = []
    willing = []

    for i, c in enumerate(s, 1):
        if c == '0':
            unwilling.append(i)
        else:
            willing.append(i)

    if len(unwilling) > a or n > a + 2 * b:
        print("NO")
        return

    single = [0] * a
    double = [[0, 0] for _ in range(b)]

    pos = 0

    # Unwilling people must use single beds.
    for person in unwilling:
        single[pos] = person
        pos += 1

    # Use remaining single beds for willing people.
    wi = 0
    while pos < a and wi < len(willing):
        single[pos] = willing[wi]
        pos += 1
        wi += 1

    # Put the remaining willing people into double beds.
    di = 0
    while wi < len(willing):
        double[di][0] = willing[wi]
        wi += 1

        if wi < len(willing):
            double[di][1] = willing[wi]
            wi += 1

        di += 1

    print("YES")

    for person in single:
        print(person)

    for x, y in double:
        print(x, y)

if __name__ == "__main__":
    solve()
```Hai mảng`unwilling`Và`willing`chứa các chỉ số người dựa trên 1, phù hợp với việc đánh số đầu ra được yêu cầu. Lần kiểm tra tính khả thi đầu tiên sẽ bảo vệ loại người duy nhất không thể sử dụng giường đôi. Lần kiểm tra thứ hai tính số chỗ ngủ thực tế, với mỗi giường đôi sẽ đóng góp hai chỗ. 

Việc xây dựng giường đơn có chủ ý xử lý`unwilling`Đầu tiên. Trật tự này là sự lựa chọn tham lam thiết yếu. Một khi tất cả những người không muốn đã được bố trí chỗ ở, việc sử dụng một chiếc giường đơn cho một người sẵn sàng không bao giờ có hại gì, bởi vì người đó cũng có thể đã sử dụng một chiếc giường đôi. 

Vòng giường đôi cần một hoặc hai người sẵn lòng. Vị trí thứ hai chỉ được lấp đầy khi còn lại một người khác, do đó, giường đôi được sử dụng một phần cuối cùng được thể hiện chính xác. Tất cả các mảng đều được khởi tạo bằng 0, tự động đưa ra biểu diễn cần thiết cho các giường chưa sử dụng. 

Không có vấn đề tràn số nguyên trong Python và ngay cả trong các ngôn ngữ có số nguyên có chiều rộng cố định, biểu thức có liên quan lớn nhất,`a + 2b`, chỉ ở xung quanh`600000`. Việc lập chỉ mục sử dụng`enumerate(s, 1)`để số người chính xác như yêu cầu`1`bởi vì`n`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hãy xem xét```
7 3 2
0111111
```Có một người không muốn và sáu người có lòng. Ba chiếc giường đơn trước tiên được sử dụng, bảo vệ người không muốn, sau đó chứa hai người có thiện chí. Bốn người sẵn sàng còn lại vừa vặn với hai chiếc giường đôi. 

| Bước | Không muốn | Sẵn sàng còn lại | Giường đơn | Giường đôi | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 1 | 6 |`[0, 0, 0]`|`[(0,0),(0,0)]`| 
| Đưa người 1 vào đơn | 0 | 6 |`[1,0,0]`|`[(0,0),(0,0)]`| 
| Xếp người 2 vào đơn | 0 | 5 |`[1,2,0]`|`[(0,0),(0,0)]`| 
| Xếp người 3 vào đơn | 0 | 4 |`[1,2,3]`|`[(0,0),(0,0)]`| 
| Đôi đầu tiên | 0 | 2 |`[1,2,3]`|`[(4,5),(0,0)]`| 
| Đôi thứ hai | 0 | 0 |`[1,2,3]`|`[(4,5),(6,7)]`| 

Sự bất biến được thể hiện trong suốt quá trình xây dựng: người duy nhất không muốn đã bị cô lập và mọi người sau đó được chỉ định vào giường đôi đều sẵn lòng. Sự sắp xếp cuối cùng là hợp lệ. 

### Mẫu 2 

Hãy xem xét```
7 3 2
0000000
```Cả bảy người đều từ chối chia sẻ. Chỉ có ba giường đơn nên điều kiện khả thi đầu tiên thất bại ngay lập tức. 

| Bước | Không muốn | Có sẵn giường đơn | Yêu cầu giường đơn | Kết quả | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 7 | 3 | 7 |`NO`| 

Thuật toán không cố gắng đưa bất kỳ ai trong số những người này vào giường đôi. Đây chính xác là sự khác biệt giữa một chiếc giường đôi có hai vị trí vật lý và một người không muốn sử dụng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + a + b) | Mỗi người, mỗi giường đầu ra được xử lý một số lần không đổi. | 
| Không gian | O(n + a + b) | Các nhóm người và phân công giường đã xây dựng sẽ được lưu trữ trước khi in. | 

Với tất cả các tham số được giới hạn bởi`200000`, thuật toán chỉ thực hiện một vài lần tuyến tính trên các mảng đầu vào và đầu ra. Điều này hoàn toàn thoải mái trong giới hạn dự định, trong khi bất kỳ cách xây dựng hàm mũ hoặc bậc hai nào cũng sẽ không khả thi ở kích thước đầu vào tối đa. 

## Trường hợp thử nghiệm 

Đầu ra của một giải pháp hợp lệ không phải là duy nhất, do đó, bộ khai thác thử nghiệm phải xác thực phép gán được tạo ra thay vì so sánh nó với một chuỗi cố định. Các thử nghiệm sau đây sử dụng logic xây dựng tương tự và xác minh các yêu cầu ngữ nghĩa của đầu ra.```python
import sys
import io

def solve_data(inp: str) -> str:
    data = inp.split()
    it = iter(data)

    n = int(next(it))
    a = int(next(it))
    b = int(next(it))
    s = next(it)

    unwilling = []
    willing = []

    for i, c in enumerate(s, 1):
        if c == '0':
            unwilling.append(i)
        else:
            willing.append(i)

    if len(unwilling) > a or n > a + 2 * b:
        return "NO\n"

    single = [0] * a
    double = [[0, 0] for _ in range(b)]

    pos = 0
    for person in unwilling:
        single[pos] = person
        pos += 1

    wi = 0
    while pos < a and wi < len(willing):
        single[pos] = willing[wi]
        pos += 1
        wi += 1

    di = 0
    while wi < len(willing):
        double[di][0] = willing[wi]
        wi += 1

        if wi < len(willing):
            double[di][1] = willing[wi]
            wi += 1

        di += 1

    out = ["YES"]
    out.extend(map(str, single))
    out.extend(f"{x} {y}" for x, y in double)
    return "\n".join(out) + "\n"

def run(inp: str) -> str:
    return solve_data(inp)

def validate(inp: str, out: str) -> bool:
    data = inp.split()
    n, a, b = map(int, data[:3])
    s = data[3]

    lines = out.strip().splitlines()

    if not lines:
        return False

    if lines[0] == "NO":
        zeros = s.count('0')
        return zeros > a or n > a + 2 * b

    if lines[0] != "YES":
        return False

    if len(lines) != 1 + a + b:
        return False

    used = []

    for i in range(1, 1 + a):
        x = int(lines[i])
        if x != 0:
            if not (1 <= x <= n):
                return False
            used.append(x)

    for i in range(1 + a, 1 + a + b):
        x, y = map(int, lines[i].split())

        if x != 0:
            if not (1 <= x <= n):
                return False
            if s[x - 1] == '0' and y != 0:
                return False
            used.append(x)

        if y != 0:
            if not (1 <= y <= n):
                return False
            if s[y - 1] == '0' and x != 0:
                return False
            used.append(y)

    return sorted(used) == list(range(1, n + 1))

# Provided sample 1, one valid interpretation of the missing formatting.
sample1 = "7 3 2\n0111111\n"
assert validate(sample1, run(sample1)), "sample 1"

# Provided sample 2, all people refuse to share.
sample2 = "7 3 2\n0000000\n"
assert not validate(sample2, run(sample2)), "sample 2"

# Minimum size, one person and one single bed.
case1 = "1 1 0\n0\n"
assert validate(case1, run(case1)), "minimum case"

# Boundary: every person is willing and exactly all double-bed places are used.
case2 = "4 0 2\n1111\n"
assert validate(case2, run(case2)), "exact double capacity"

# All unwilling people exactly fit into single beds.
case3 = "3 3 0\n000\n"
assert validate(case3, run(case3)), "all unwilling"

# Physical capacity is enough, but there are too many unwilling people.
case4 = "4 1 2\n0001\n"
assert not validate(case4, run(case4)), "too many unwilling people"

# Maximum-size style test with all willing people.
n = 200000
case5 = f"{n} 0 {n // 2}\n" + "1" * n + "\n"
assert validate(case5, run(case5)), "large input"
```Định dạng mẫu trong câu lệnh được cung cấp sẽ mất một số ngắt dòng, do đó, các bài kiểm tra sẽ sử dụng dữ liệu đầu vào tương ứng và xác thực điều kiện toán học của bài tập được tạo thay vì phụ thuộc vào một đầu ra mẫu cụ thể. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`7 3 2 / 0111111`|`YES`với một bài tập hợp lệ | Xây dựng khả thi bình thường | 
|`7 3 2 / 0000000`|`NO`| Nhiều người không muốn hơn giường đơn | 
|`1 1 0 / 0`|`YES`| Ví dụ kích thước tối thiểu | 
|`4 0 2 / 1111`|`YES`| Công suất giường đôi chính xác | 
|`3 3 0 / 000`|`YES`| Tất cả mọi người đều không sẵn lòng | 
|`4 1 2 / 0001`|`NO`| Công suất tồn tại, nhưng việc hạn chế giường đơn không thành công | 
|`200000 0 100000 / 111...1`|`YES`| Xây dựng tuyến tính kích thước tối đa | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là người không chịu chia sẻ khi không có giường đơn. Vì```
1 0 1
0
```số người không muốn là`1`, trong khi`a = 0`. Thuật toán không thành công trong lần kiểm tra tính khả thi đầu tiên và in`NO`. Một chiếc giường đôi không thể giải cứu được người này vì việc đặt họ ở đó cùng với bất kỳ ai sẽ vi phạm hạn chế của họ, trong khi việc đặt họ một mình chỉ được phép nếu vị trí giường đôi của vấn đề không được sử dụng, không có người chiếm giữ. Như vậy người đó không có giường hợp pháp. 

Trường hợp thứ hai là không đủ công suất giường đơn mặc dù có đủ tổng công suất vật lý. Vì```
2 1 0
00
```có hai người không muốn và chỉ có một chiếc giường đơn. Séc`len(unwilling) > a`đánh giá để`2 > 1`, do đó thuật toán in`NO`. Chỉ tính tổng số vị trí giường cũng sẽ cho`1`, vì vậy trường hợp này nắm bắt được các giải pháp quên đi hạn chế về khả năng tương thích. 

Trường hợp thứ ba là một người sẵn sàng bị bỏ lại một mình trên chiếc giường đôi. Vì```
3 1 1
011
```người`1`chiếm chiếc giường đơn duy nhất. Người`2`Và`3`đều sẵn lòng và chiếm giữ hai vị trí trên giường đôi. Thay vào đó, nếu có bốn người sẵn lòng với hai giường đôi, thuật toán sẽ lấp đầy cả hai giường. Nếu có ba người sẵn lòng và hai chiếc giường đôi thì chiếc giường cuối cùng sẽ chứa một người và một số không. Điều này là hợp pháp vì người còn lại sẵn sàng chia sẻ. 

Trường hợp cạnh thứ tư là tổng công suất chính xác. Vì```
4 1 2
0001
```tổng năng lực thể chất là`1 + 2 * 2 = 5`, đủ cho bốn người, nhưng có ba người không muốn và chỉ có một chiếc giường đơn. Kiểm tra đầu tiên từ chối phiên bản trước khi xây dựng. Điều này chứng tỏ tại sao chỉ tổng công suất thôi thì chưa đủ điều kiện khả thi. 

Hộp đựng cạnh cuối cùng là phần trống hoặc chưa được sử dụng trong kho giường. Vì```
2 3 2
11
```cả hai người có thể được đặt trên hai giường đơn đầu tiên, trong khi giường đơn thứ ba và cả hai giường đôi vẫn chưa được sử dụng. Các mảng được khởi tạo bằng 0, do đó, đầu ra đương nhiên chứa các số 0 cho mọi vị trí không sử dụng. Không cần có thẻ dọn dẹp đặc biệt.
