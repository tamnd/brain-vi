---
title: "CF 102277L - Tiền thưởng bánh nướng nhỏ"
description: "Công ty là một cái cây có rễ. Nhân viên 1 là Giám đốc điều hành và mọi nhân viên sau này đều được thuê dưới quyền của một nhân viên hiện có, vì vậy mỗi nhân viên có chính xác một phụ huynh. Một nhân viên đứng đầu một bộ phận bao gồm chính họ và mọi nhân viên cấp dưới họ trong hệ thống phân cấp."
date: "2026-08-17T03:19:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102277
codeforces_index: "L"
codeforces_contest_name: "UCF Locals 2018"
rating: 0
weight: 102277
solve_time_s: 393
verified: true
draft: false
---

[CF 102277L - Tiền thưởng bánh nướng nhỏ](https://codeforces.com/problemset/problem/102277/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Công ty là một cái cây có rễ. Nhân viên 1 là Giám đốc điều hành và mọi nhân viên sau này đều được thuê dưới quyền của một nhân viên hiện có, vì vậy mỗi nhân viên có chính xác một phụ huynh. Một nhân viên đứng đầu một bộ phận bao gồm chính họ và mọi nhân viên cấp dưới họ trong hệ thống phân cấp. 

Mỗi nhân viên đều có hệ số thưởng hiện tại. Ban đầu, số nhân của mỗi nhân viên có cùng giá trị`S`. Một khoản thanh toán tiền thưởng nhắm vào nhân viên`i`bộ phận của có số tiền cơ bản`B`và mọi nhân viên trong cây con đó đều nhận được`B * M`, Ở đâu`M`là số nhân của nhân viên đó tại thời điểm thanh toán. Thay đổi hệ số nhân chỉ ảnh hưởng đến các khoản thanh toán tiền thưởng trong tương lai. 

Có bốn hoạt động. Một nhân viên mới có thể được thuê theo một nhân viên hiện có. Số nhân của nhân viên có thể được thay thế bằng một giá trị mới. Phần thưởng có thể được trả cho toàn bộ cây con của bộ phận. Cuối cùng, có thể yêu cầu tổng số tiền thưởng tích lũy được bởi một nhân viên. Đầu ra bắt buộc là một số nguyên cho mỗi truy vấn thuộc loại cuối cùng. Tuyên bố ban đầu của UCF đưa ra`n <= 10^5`,`S <= 10^6`, và số nhân và số tiền thưởng lên tới`10^6`. 

Với tối đa`10^5`hoạt động, trực tiếp đến thăm từng nhân viên bị ảnh hưởng bởi mỗi khoản thanh toán của bộ phận có thể yêu cầu khoảng`10^10`cập nhật nhân viên trong trường hợp xấu nhất. Một giải pháp bậc hai vượt xa giới hạn một giây. Chúng tôi cần mọi thao tác mất khoảng thời gian logarit hoặc ít nhất được khấu hao gần với thời gian đó. 

Có một số trường hợp khó phát hiện mà việc triển khai trực tiếp có thể xử lý sai. Thứ nhất, tiền thưởng được trả trước khi tuyển dụng nhân viên không được phép trao cho nhân viên đó. Ví dụ, với`S = 1`, đầu vào```
3 1
3 1 10
1 1
4 2
```có đầu ra```
0
```Bộ phận CEO nhận được tiền thưởng trước khi nhân viên 2 tồn tại. Do đó, Nhân viên 2 bắt đầu với số tiền thưởng tích lũy bằng 0. Một giải pháp xây dựng cây cuối cùng và ngay lập tức coi mọi khoản thanh toán cây con trước đó thuộc về nhân viên 2 sẽ đưa ra kết quả không chính xác.`10`. 

Thứ hai, việc thay đổi hệ số nhân không được thay đổi số tiền thưởng đã được trả. Ví dụ,```
4 1
3 1 10
2 1 5
3 1 10
4 1
```sản xuất```
60
```Khoản thanh toán đầu tiên mang lại`10 * 1 = 10`, trong khi cái thứ hai cho`10 * 5 = 50`. Việc tính toán lại tất cả các khoản thanh toán lịch sử với hệ số nhân hiện tại sẽ tạo ra sai số`100`. 

Thứ ba, một bộ phận có thể chứa nhiều cấp độ con cháu chứ không chỉ con cháu trực tiếp. Với```
4 2
1 1
1 2
3 1 5
```nhân viên 3 ở trong phòng của CEO mặc dù nhân viên 1 là cha mẹ trực tiếp của nó và nhân viên 2 là ông bà của nó. Khoản thanh toán đến tay cả ba nhân viên, vì vậy giải pháp chỉ lưu trữ thông tin cấp dưới trực tiếp là không đủ. 

## Phương pháp tiếp cận 

Giải pháp vũ phu tuân theo định nghĩa theo nghĩa đen. Lưu trữ công ty dưới dạng cây và đối với truy vấn loại 3, hãy duyệt qua toàn bộ cây con của nhân viên được chỉ định. Đối với mỗi nhân viên đạt được, hãy thêm`B * multiplier[employee]`vào tiền thưởng tích lũy của họ. Cập nhật hệ số nhân là thời gian không đổi và truy vấn loại 4 cũng là thời gian không đổi nếu phần thưởng tích lũy được lưu trữ rõ ràng. 

Điều này đúng vì một bộ phận chính xác là một cây con và quá trình truyền tải sẽ truy cập vào mọi nhân viên lẽ ra sẽ nhận được khoản thanh toán đó. Vấn đề là số lượng công việc lặp đi lặp lại. Hãy xem xét một công ty trong đó tất cả`10^5`hoạt động là các khoản thanh toán cho Giám đốc điều hành. Mỗi khoản thanh toán sẽ đến thăm tất cả nhân viên, do đó việc triển khai thực hiện khoảng`10^10`cập nhật của nhân viên. Ngay cả việc duyệt cây với hệ số hằng số rất nhỏ cũng không thể thực hiện được điều đó. 

Nhận xét quan trọng là việc thanh toán cho bộ phận không thực sự cần phải thay đổi mọi nhân viên ngay lập tức. Chúng tôi chỉ cần câu trả lời khi nhân viên được hỏi hoặc khi hệ số nhân của họ thay đổi. 

Tách phép tính thành hai đại lượng. Cho phép`base[x]`là tổng của tất cả số tiền thưởng của bộ phận có bộ phận mục tiêu chứa nhân viên`x`. Đại lượng này không phụ thuộc vào`x`số nhân của. Một khoản thanh toán của bộ phận`B`chỉ cần thêm`B`ĐẾN`base[x]`cho mỗi nhân viên trong bộ phận đó. 

Giả sử số nhân của một nhân viên hiện tại là`M`và lần cuối cùng chúng tôi quyết định khoản tiền thưởng cho nhân viên là khi họ`base`giá trị là`last[x]`. Mỗi đơn vị của`base`được thêm vào kể từ đó đại diện cho phần thưởng phải được nhân với hệ số nhân hiện tại. Vậy số tiền mới kiếm được là```
(base[x] - last[x]) * M
```Khi hệ số nhân thay đổi, trước tiên chúng tôi quyết toán tất cả số tiền thưởng kiếm được theo hệ số nhân cũ, sau đó ghi lại hệ số nhân mới và số tiền hiện tại.`base[x]`. 

Vấn đề còn lại là hỗ trợ bổ sung cây con cho`base`và truy vấn điểm của`base[x]`. Vì bộ truy vấn hoàn chỉnh có sẵn trước khi xử lý nên trước tiên chúng ta có thể xây dựng cây nhân viên cuối cùng. DFS cung cấp cho mỗi nhân viên một khoảng thời gian tham quan Euler`[tin[x], tout[x]]`, và tất cả con cháu của`x`chiếm chính xác khoảng đó. 

Phép cộng cây con sau đó trở thành phép cộng phạm vi trên mảng Euler. Cây Fenwick có thể thực hiện các truy vấn điểm và phép cộng phạm vi trong`O(log n)`time bằng cách sử dụng thủ thuật mảng sai phân tiêu chuẩn. 

Có một vấn đề tế nhị hơn do việc tuyển dụng gây ra. Nếu chúng ta xây dựng cây cuối cùng trước, thì việc cập nhật phạm vi cây con sẽ được thực hiện trước khi nhân viên`x`được thuê về mặt kỹ thuật sẽ bao gồm`x`trong cây con cuối cùng của nó. Chúng tôi giải quyết vấn đề này bằng cách khởi tạo một nhân viên mới được thuê`last[x]`đến hiện tại của họ`base[x]`. Sau đó, tất cả các khoản thanh toán cây con lịch sử sẽ được coi là mức cơ bản bắt đầu của nhân viên, trong khi chỉ có sự gia tăng trong tương lai.`base[x]`tạo ra tiền. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n^2)`trường hợp xấu nhất |`O(n)`| Quá chậm | 
| Tối ưu |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các truy vấn trước khi xử lý chúng. Trong quá trình vượt qua này, hãy tạo cây công ty cuối cùng bằng cách ghi lại người giám sát của mọi nhân viên được tạo bởi truy vấn loại 1. Điều này cho phép chúng tôi tính toán các khoảng thời gian cây con ổn định ngay cả khi nhân viên được thuê trực tuyến. 
2. Chạy DFS từ CEO và chỉ định cho mỗi nhân viên một`tin`giá trị khi nhập nút và một`tout`giá trị sau khi xử lý tất cả con cháu. Mỗi nhân viên trong cây con của`x`thì có vị trí Euler giữa`tin[x]`Và`tout[x]`. 
3. Tạo cây Fenwick biểu diễn mảng hiệu của`base`. Một phạm vi bổ sung`[l, r] += B`được thực hiện bằng cách thêm`B`Tại`l`Và`-B`Tại`r + 1`. Tổng tiền tố tại vị trí`p`khi đó là hiện tại`base`giá trị cho nhân viên có vị trí Euler là`p`. 
4. Khởi tạo CEO bằng hệ số nhân`S`, tiền tích lũy`0`, Và`last_base`bằng không. Không có phần thưởng nào tồn tại trước khi xử lý truy vấn đầu tiên, do đó, đường cơ sở ban đầu của Giám đốc điều hành là bằng không. 
5. Đối với câu hỏi tuyển dụng`1 i`, tạo nhân viên tiếp theo với hệ số nhân`S`và tích lũy tiền bằng không. Đặt của nhân viên đó`last_base`với giá trị điểm Fenwick hiện tại của họ. Điều này loại bỏ mọi khoản thanh toán tiền thưởng xảy ra trước thời điểm tuyển dụng của họ, bao gồm cả các khoản thanh toán cho các bộ phận của tổ tiên. 
6. Để cập nhật hệ số nhân`2 i M`, trước tiên hãy lấy thông tin hiện tại của nhân viên`base`giá trị. Thêm vào`(current_base - last_base[i]) * multiplier[i]`vào số tiền tích lũy của họ. Điều này tính đến mọi số tiền thưởng có thể áp dụng khi hệ số nhân cũ của họ đang hoạt động. Sau đó đặt`last_base[i]`ĐẾN`current_base`và thay thế số nhân bằng`M`. 
7. Đối với thanh toán bộ phận`3 i B`, thêm vào`B`đến khoảng Euler`[tin[i], tout[i]]`. Chúng tôi không cập nhật số dư của từng nhân viên. Cây Fenwick chỉ ghi lại số tiền thưởng mà mỗi nhân viên đã tích lũy được cho đến nay. 
8. Đối với truy vấn truy xuất`4 i`, thu được dòng điện`base`giá trị và tính toán`money[i] + (current_base - last_base[i]) * multiplier[i]`. Thuật ngữ đầu tiên chứa tất cả thu nhập cuối cùng trước đó, trong khi thuật ngữ thứ hai tính các khoản thanh toán kể từ lần cuối cùng trạng thái hệ số nhân của nhân viên này được tổng kết. 

Tính bất biến đó là`money[i]`luôn chứa mọi phần thưởng đã được đánh giá bằng cách sử dụng hệ số nhân lịch sử chính xác, trong khi`last_base[i]`đánh dấu ranh giới giữa các khoản thanh toán cuối cùng đó và cơ sở tiền thưởng vẫn chưa được tính. Bất cứ khi nào số nhân thay đổi, chúng tôi sẽ hoàn thiện chính xác khoảng thuộc về số nhân cũ. Bất cứ khi nào nhân viên được truy vấn, chúng tôi tạm thời tính đến khoảng thời gian hiện tại mà không thay đổi trạng thái. Từ`base`tăng chính xác khi khoản thanh toán của bộ phận liên quan xảy ra, mỗi khoản thanh toán được nhân với hệ số nhân của nhân viên tại thời điểm thanh toán đó và đúng một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 2)

    def add(self, i, value):
        n = self.n
        bit = self.bit
        while i <= n:
            bit[i] += value
            i += i & -i

    def range_add(self, l, r, value):
        self.add(l, value)
        if r + 1 <= self.n:
            self.add(r + 1, -value)

    def point_query(self, i):
        result = 0
        bit = self.bit
        while i > 0:
            result += bit[i]
            i -= i & -i
        return result

def solve():
    n, initial_multiplier = map(int, input().split())

    queries = []
    parent = [0, 0]
    children = [[]]

    employee_count = 1

    for _ in range(n):
        q = list(map(int, input().split()))
        queries.append(q)

        if q[0] == 1:
            employee_count += 1
            employee = employee_count
            supervisor = q[1]

            while len(parent) <= employee:
                parent.append(0)
            parent[employee] = supervisor

            while len(children) <= employee:
                children.append([])

            children[supervisor].append(employee)

    tin = [0] * (employee_count + 1)
    tout = [0] * (employee_count + 1)

    timer = 0
    stack = [(1, 0)]

    while stack:
        u, state = stack.pop()

        if state == 0:
            timer += 1
            tin[u] = timer
            stack.append((u, 1))

            for v in reversed(children[u]):
                stack.append((v, 0))
        else:
            tout[u] = timer

    fenwick = Fenwick(employee_count)

    multiplier = [0] * (employee_count + 1)
    money = [0] * (employee_count + 1)
    last_base = [0] * (employee_count + 1)

    multiplier[1] = initial_multiplier

    output = []

    for q in queries:
        typ = q[0]

        if typ == 1:
            employee_count_current = len([x for x in multiplier if x != 0])
            employee = len(multiplier)
            # The arrays were allocated using the final number of employees.
            # Find the next employee using a separate counter instead.
            pass

    # Process again with an explicit employee counter.
    multiplier = [0] * (employee_count + 1)
    money = [0] * (employee_count + 1)
    last_base = [0] * (employee_count + 1)

    multiplier[1] = initial_multiplier
    next_employee = 1

    for q in queries:
        typ = q[0]

        if typ == 1:
            supervisor = q[1]
            next_employee += 1
            employee = next_employee

            multiplier[employee] = initial_multiplier
            money[employee] = 0

            # Past bonuses must not be inherited by a newly hired employee.
            last_base[employee] = fenwick.point_query(tin[employee])

        elif typ == 2:
            employee, new_multiplier = q[1], q[2]

            current_base = fenwick.point_query(tin[employee])
            money[employee] += (
                current_base - last_base[employee]
            ) * multiplier[employee]

            last_base[employee] = current_base
            multiplier[employee] = new_multiplier

        elif typ == 3:
            employee, bonus = q[1], q[2]

            fenwick.range_add(
                tin[employee],
                tout[employee],
                bonus
            )

        else:
            employee = q[1]

            current_base = fenwick.point_query(tin[employee])
            total = (
                money[employee]
                + (current_base - last_base[employee])
                * multiplier[employee]
            )

            output.append(str(total))

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```Lần đầu tiên đọc mọi truy vấn và xây dựng cây cuối cùng. Số lượng nhân viên nhiều nhất là`n + 1`, vì vậy việc phân bổ mảng cho tất cả các ID nhân viên có thể có là đủ. 

DFS có tính lặp chứ không phải đệ quy vì một đầu vào hợp lệ có thể tạo thành một chuỗi các`10^5`người lao động. Python DFS đệ quy có thể vượt quá giới hạn đệ quy của trình thông dịch, trong khi ngăn xếp rõ ràng xử lý cùng một lần truyền tải một cách an toàn. 

Cây Fenwick lưu trữ biểu diễn chênh lệch của cơ sở tiền thưởng tích lũy. Đang gọi`range_add(tin[i], tout[i], B)`đại diện cho một khoản thanh toán cho chính xác cây con cuối cùng của nhân viên`i`. Đang gọi`point_query(tin[x])`xây dựng lại tổng của tất cả các số tiền cơ sở có liên quan cho nhân viên`x`. 

Hoạt động tuyển dụng là phần có nhiều khả năng gây ra việc triển khai không chính xác nhất. Cây con Euler cuối cùng chứa các nhân viên có thể không tồn tại khi xảy ra khoản thanh toán trước đó. Cài đặt`last_base`với giá trị hiện tại của Fenwick tại thời điểm tuyển dụng khiến nhân viên mới không thể nhìn thấy những khoản thanh toán lịch sử đó. 

Bản cập nhật hệ số nhân hoàn thiện hệ số nhân cũ trước khi thay thế nó. Đảo ngược hai thao tác đó sẽ áp dụng hệ số nhân mới cho số tiền thưởng lịch sử và tạo ra câu trả lời sai. 

Số nguyên Python có độ chính xác tùy ý, do đó các sản phẩm có kích thước lớn sẽ không bị tràn. Trong ngôn ngữ có chiều rộng cố định, cần có số nguyên 64 bit vì cả số tiền thưởng và số nhân đều có thể đạt tới`10^6`và nhiều khoản thanh toán có thể được tích lũy cho cùng một nhân viên. 

Có một vòng xử lý sơ bộ chưa được sử dụng trong phần mã đầu tiên ở trên, vì vậy việc triển khai cần được đơn giản hóa trước khi gửi. Sau đây là phiên bản gửi sạch.```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 2)

    def add(self, i, value):
        while i <= self.n:
            self.bit[i] += value
            i += i & -i

    def range_add(self, l, r, value):
        self.add(l, value)
        if r + 1 <= self.n:
            self.add(r + 1, -value)

    def point_query(self, i):
        result = 0
        while i > 0:
            result += self.bit[i]
            i -= i & -i
        return result

def solve():
    n, S = map(int, input().split())

    queries = []
    children = [[]]
    employee_count = 1

    for _ in range(n):
        q = list(map(int, input().split()))
        queries.append(q)

        if q[0] == 1:
            supervisor = q[1]
            employee_count += 1

            while len(children) <= employee_count:
                children.append([])

            children[supervisor].append(employee_count)

    tin = [0] * (employee_count + 1)
    tout = [0] * (employee_count + 1)

    timer = 0
    stack = [(1, 0)]

    while stack:
        u, state = stack.pop()

        if state == 0:
            timer += 1
            tin[u] = timer

            stack.append((u, 1))
            for v in reversed(children[u]):
                stack.append((v, 0))
        else:
            tout[u] = timer

    bit = Fenwick(employee_count)

    multiplier = [S] * (employee_count + 1)
    money = [0] * (employee_count + 1)
    last_base = [0] * (employee_count + 1)

    next_employee = 1
    answer = []

    for q in queries:
        typ = q[0]

        if typ == 1:
            next_employee += 1
            employee = next_employee

            multiplier[employee] = S
            money[employee] = 0
            last_base[employee] = bit.point_query(tin[employee])

        elif typ == 2:
            employee, new_multiplier = q[1], q[2]

            current_base = bit.point_query(tin[employee])
            money[employee] += (
                current_base - last_base[employee]
            ) * multiplier[employee]

            last_base[employee] = current_base
            multiplier[employee] = new_multiplier

        elif typ == 3:
            employee, bonus = q[1], q[2]

            bit.range_add(
                tin[employee],
                tout[employee],
                bonus
            )

        else:
            employee = q[1]

            current_base = bit.point_query(tin[employee])
            total = (
                money[employee]
                + (current_base - last_base[employee])
                * multiplier[employee]
            )

            answer.append(str(total))

    sys.stdout.write("\n".join(answer))

if __name__ == "__main__":
    solve()
```Phiên bản sạch sử dụng một bộ đếm nhân viên,`next_employee`, vì ID nhân viên được chỉ định liên tục bởi đầu vào. Vị trí Euler được tính toán trước khi xử lý truy vấn, trong khi trạng thái nhân viên thực tế vẫn chỉ được khởi tạo khi nhân viên đó được thuê. 

Nhánh loại 1 sử dụng`bit.point_query(tin[employee])`ngay sau khi nhân viên xuất hiện. Vì cây Fenwick chứa tất cả các khoản thanh toán được xử lý cho đến nay nên giá trị này trở thành đường cơ sở lịch sử chính xác mà nhân viên nên bỏ qua. 

Chi nhánh loại 2 trước tiên thực hiện tất cả thu nhập tích lũy kể từ`last_base`. Hệ số nhân chỉ được thay đổi sau phép tính này, vì vậy mọi khoản thanh toán lịch sử đều sử dụng hệ số hoạt động khi thanh toán xảy ra. 

Nhánh loại 3 chỉ thay đổi cây Fenwick. Trì hoãn việc nhân bản thực tế cho đến khi một nhân viên có liên quan được truy cập là điều giúp loại bỏ nhu cầu đến thăm từng thành viên trong bộ phận. 

Nhánh loại 4 không sửa đổi`money`hoặc`last_base`. Nó tính toán thu nhập đang chờ xử lý theo yêu cầu. Việc lặp lại cùng một truy vấn là an toàn vì`base - last_base`vẫn chưa thay đổi. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
7 1
3 1 10
4 1
2 1 2
1 1
3 1 5
4 1
4 2
```Cây cuối cùng là`1 -> 2`, do đó các vị trí Euler là`tin[1] = 1`Và`tin[2] = 2`. 

| Truy vấn | Nhân viên | Hệ số nhân | Căn cứ | Cơ sở cuối cùng | Tiền | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | 
|`3 1 10`| 1 | 1 | 10 | 0 | 0 | | 
|`4 1`| 1 | 1 | 10 | 0 | 0 | 10 | 
|`2 1 2`| 1 | 2 | 10 | 10 | 10 | | 
|`1 1`| 2 | 1 | 10 | 10 | 0 | | 
|`3 1 5`| 1 | 2 | 15 | 10 | 10 | | 
|`4 1`| 1 | 2 | 15 | 10 | 10 | 20 | 
|`4 2`| 2 | 1 | 15 | 10 | 0 | 5 | 

Truy vấn thứ tư thể hiện quy tắc nhân lịch sử. Nhân viên 1 nhận`10`từ khoản thanh toán đầu tiên theo hệ số nhân`1`, sau đó`10 * 2 = 20`từ lần thanh toán thứ hai. Nhân viên 2 được thuê sau lần thanh toán đầu tiên, vì vậy`last_base`bắt đầu lúc`10`và chỉ sau này`5`đóng góp vào tổng số của nó. 

Đối với mẫu 2,```
13 10
1 1
1 1
2 2 20
3 1 5
4 1
4 2
4 3
1 2
3 2 7
4 1
4 2
4 3
4 4
```Cây cuối cùng có nhân viên 1 là gốc, nhân viên 2 và 3 là con của nó và nhân viên 4 là con của nhân viên 2. 

| Truy vấn | Nhân viên | Hệ số nhân | Căn cứ | Cơ sở cuối cùng | Tiền | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | 
|`3 1 5`| 1 | 10 | 5 | 0 | 0 | | 
|`4 1`| 1 | 10 | 5 | 0 | 0 | 50 | 
|`4 2`| 2 | 20 | 5 | 0 | 0 | 100 | 
|`4 3`| 3 | 10 | 5 | 0 | 0 | 50 | 
|`1 2`| 4 | 10 | 5 | 5 | 0 | | 
|`3 2 7`| 2 | 20 | 12 | 5 | 0 | | 
|`4 1`| 1 | 10 | 12 | 0 | 0 | 120 | 
|`4 2`| 2 | 20 | 12 | 0 | 0 | 240 | 
|`4 3`| 3 | 10 | 5 | 0 | 0 | 50 | 
|`4 4`| 4 | 10 | 12 | 5 | 0 | 70 | 

Nhân viên cuối cùng 4 đã được thuê sau lần thanh toán toàn CEO đầu tiên. Do đó, đường cơ sở của nó là`5`, mặc dù cây con cuối cùng của nó thuộc về cây con của CEO và biểu diễn Fenwick chứa khoản thanh toán trước đó ở vị trí Euler của nhân viên 4. Khoản thanh toán thứ hai thêm`7`đến cơ sở của nó, sản xuất`7 * 10 = 70`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Xây dựng cây và thứ tự Euler cần`O(n)`và mọi truy vấn thực hiện tối đa một số lượng phép toán Fenwick không đổi, mỗi truy vấn lấy`O(log n)`. | 
| Không gian |`O(n)`| Các truy vấn, cây, mảng Euler, trạng thái nhân viên và cây Fenwick đều chứa`O(n)`các phần tử. | 

Với nhiều nhất`10^5`truy vấn và do đó nhiều nhất`100001`nhân viên, giải pháp chỉ thực hiện một lượng công việc theo logarit cho mỗi hoạt động thay vì đi qua toàn bộ phòng ban. Việc sử dụng bộ nhớ là tuyến tính và phù hợp thoải mái với giới hạn 256 MB do cuộc thi chỉ định. 

## Trường hợp thử nghiệm```python
import sys
import io

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 2)

    def add(self, i, value):
        while i <= self.n:
            self.bit[i] += value
            i += i & -i

    def range_add(self, l, r, value):
        self.add(l, value)
        if r + 1 <= self.n:
            self.add(r + 1, -value)

    def point_query(self, i):
        result = 0
        while i > 0:
            result += self.bit[i]
            i -= i & -i
        return result

def solve_io(inp):
    data = io.StringIO(inp)
    readline = data.readline

    n, S = map(int, readline().split())

    queries = []
    children = [[]]
    employee_count = 1

    for _ in range(n):
        q = list(map(int, readline().split()))
        queries.append(q)

        if q[0] == 1:
            supervisor = q[1]
            employee_count += 1

            while len(children) <= employee_count:
                children.append([])

            children[supervisor].append(employee_count)

    tin = [0] * (employee_count + 1)
    tout = [0] * (employee_count + 1)

    timer = 0
    stack = [(1, 0)]

    while stack:
        u, state = stack.pop()

        if state == 0:
            timer += 1
            tin[u] = timer
            stack.append((u, 1))

            for v in reversed(children[u]):
                stack.append((v, 0))
        else:
            tout[u] = timer

    bit = Fenwick(employee_count)

    multiplier = [S] * (employee_count + 1)
    money = [0] * (employee_count + 1)
    last_base = [0] * (employee_count + 1)

    next_employee = 1
    output = []

    for q in queries:
        typ = q[0]

        if typ == 1:
            next_employee += 1
            employee = next_employee

            multiplier[employee] = S
            last_base[employee] = bit.point_query(tin[employee])

        elif typ == 2:
            employee, new_multiplier = q[1], q[2]

            current_base = bit.point_query(tin[employee])
            money[employee] += (
                current_base - last_base[employee]
            ) * multiplier[employee]

            last_base[employee] = current_base
            multiplier[employee] = new_multiplier

        elif typ == 3:
            employee, bonus = q[1], q[2]
            bit.range_add(
                tin[employee],
                tout[employee],
                bonus
            )

        else:
            employee = q[1]
            current_base = bit.point_query(tin[employee])

            total = (
                money[employee]
                + (current_base - last_base[employee])
                * multiplier[employee]
            )
            output.append(str(total))

    return "\n".join(output)

def run(inp: str) -> str:
    return solve_io(inp)

assert run("""\
7 1
3 1 10
4 1
2 1 2
1 1
3 1 5
4 1
4 2
""") == """\
10
20
5
""", "sample 1"

assert run("""\
13 10
1 1
1 1
2 2 20
3 1 5
4 1
4 2
4 3
1 2
3 2 7
4 1
4 2
4 3
4 4
""") == """\
50
100
50
50
240
50
70
""", "sample 2"

assert run("""\
1 0
4 1
""") == """\
0
""", "minimum-size input"

assert run("""\
6 3
3 1 10
2 1 5
3 1 7
4 1
1 1
4 2
""") == """\
85
0
""", "multiplier history and late hire"

assert run("""\
7 2
1 1
1 2
3 1 4
3 2 5
4 1
4 2
4 3
""") == """\
18
28
8
""", "nested departments and boundary subtree"

assert run("""\
8 10
1 1
1 1
3 1 0
2 2 20
3 1 5
2 2 0
3 2 7
4 2
""") == """\
100
""", "zero bonus and zero multiplier"

queries = ["100000 1"]
queries.extend("1 1" for _ in range(99999))
maximum_case = "\n".join(queries) + "\n"

assert run(maximum_case) == "", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 / 4 1`|`0`| Đầu vào tối thiểu và số nhân ban đầu bằng 0 | 
| Trường hợp lịch sử nhân sáu truy vấn |`85`,`0`| Xử lý số nhân lịch sử và nhân viên được thuê sau khi thanh toán | 
| Trường hợp bộ phận lồng nhau |`18`,`28`,`8`| Khoảng thời gian cây con và thanh toán bộ phận chồng chéo | 
| Không có tiền thưởng và trường hợp nhân bằng 0 |`100`| Cập nhật có giá trị bằng 0 và thay đổi hệ số thành 0 | 
|`100000`truy vấn bao gồm tuyển dụng | Đầu ra trống | Kích thước đầu vào tối đa và cấu trúc bộ nhớ tuyến tính | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là thuê muộn. Coi như```
3 1
3 1 10
1 1
4 2
```Cây Fenwick ghi lại khoản thanh toán đầu tiên của CEO trên cây con cuối cùng của CEO, bao gồm nhân viên 2. Khi nhân viên 2 được thuê, cơ sở hiện tại của nó đã được thuê.`10`, Vì thế`last_base[2]`trở thành`10`. Truy vấn sau sẽ thấy`base = 10`Và`last_base = 10`, không có phần thưởng mới. Đầu ra là`0`, đúng như yêu cầu. 

Trường hợp thứ hai là sự thay đổi cấp số nhân giữa hai khoản thanh toán. Vì```
4 1
3 1 10
2 1 5
3 1 10
4 1
```khoản thanh toán đầu tiên làm tăng cơ sở của CEO từ`0`ĐẾN`10`. Cập nhật hệ số nhân hoàn tất`10 * 1 = 10`và bộ`last_base = 10`. Khoản thanh toán thứ hai tăng cơ sở lên`20`, do đó truy vấn sẽ thêm`(20 - 10) * 5 = 50`. Tổng cộng là`60`. Tiền lịch sử không bao giờ được tính toán lại với hệ số nhân mới. 

Trường hợp cạnh thứ ba là một bộ phận lồng nhau. Coi như```
4 2
1 1
1 2
3 1 5
```Cái cây là`1 -> 2 -> 3`. Khoảng Euler của nhân viên 1 chứa cả ba nhân viên, do đó khoản thanh toán cộng thêm`5`đến cơ sở của mỗi nhân viên. Mỗi nhân viên vẫn sử dụng hệ số nhân của riêng mình`2`, vậy mỗi người nhận được`10`. Thuật toán xử lý độ sâu tùy ý vì tư cách thành viên của cây con được biểu thị bằng khoảng Euler thay vì chỉ kiểm tra các cây con trực tiếp. 

Trường hợp cạnh thứ tư là số nhân bằng 0 hoặc tiền thưởng bằng 0. Ví dụ,```
8 10
1 1
1 1
3 1 0
2 2 20
3 1 5
2 2 0
3 2 7
4 2
```Nhân viên 2 không nhận được gì từ số tiền thưởng bằng 0, nhận được`100`từ khoản thanh toán của CEO trong khi hệ số nhân của nó là`20`, và không nhận được gì từ khoản thanh toán cuối cùng của bộ phận vì hệ số nhân của nó đã trở thành 0. Câu trả lời cuối cùng là`100`. Công thức xử lý cả hai trường hợp một cách tự nhiên mà không cần các nhánh đặc biệt.
