---
title: "CF 490E - Khôi phục dãy tăng dần"
description: "Chúng tôi được cung cấp một chuỗi các chuỗi. Mỗi chuỗi đại diện cho một số nguyên dương, nhưng một số chữ số đã được thay thế bằng '?'. Trình tự ban đầu đã tăng lên một cách nghiêm ngặt. Nhiệm vụ của chúng ta là thay thế mọi dấu '?"
date: "2026-06-07T17:42:35+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 490
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 279 (Div. 2)"
rating: 2000
weight: 490
solve_time_s: 126
verified: true
draft: false
---

[CF 490E - Khôi phục dãy tăng dần](https://codeforces.com/problemset/problem/490/E) 

**Xếp hạng:** 2000 
**Tags:** tìm kiếm nhị phân, vũ phu, tham lam, thực hiện 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi các chuỗi. Mỗi chuỗi đại diện cho một số nguyên dương, nhưng một số chữ số đã được thay thế bằng`'?'`. Trình tự ban đầu đã tăng lên một cách nghiêm ngặt. Nhiệm vụ của chúng ta là thay thế mọi`'?'`bằng một chữ số thập phân sao cho tất cả các số kết quả đều là số nguyên dương hợp lệ không có số 0 đứng đầu và toàn bộ chuỗi trở nên tăng nghiêm ngặt. 

Độ dài chuỗi có thể đạt tới$10^5$, trong khi mỗi số chứa tối đa 8 ký tự. Độ dài nhỏ của mỗi số riêng lẻ là quan sát quan trọng. Mặc dù có nhiều phần tử, mỗi phần tử thuộc về một không gian tìm kiếm nhỏ vì độ dài của nó không bao giờ vượt quá 8. Bất kỳ giải pháp nào chỉ thực hiện một lượng công việc không đổi trên mỗi chuỗi đều đủ nhanh. Mặt khác, việc thử mọi sự thay thế có thể là không thể. Một chuỗi có độ dài 8 bao gồm toàn bộ dấu chấm hỏi đã đại diện cho$10^8$ứng viên. 

Phần tế nhị nhất của vấn đề là những lựa chọn cục bộ ảnh hưởng đến những con số sau này. Việc chọn sớm một giá trị lớn không cần thiết có thể khiến các vị thế trong tương lai không thể thực hiện được. 

Coi như:```
2
??
10
```Số đầu tiên có thể là 99, nhưng số thứ hai không thể vượt quá số đó. Lựa chọn đúng là số hợp lệ nhỏ nhất lớn hơn phần tử trước đó, cụ thể là 10 là không thể vì dãy phải tăng nghiêm ngặt nên ta chọn 01? Không được phép có số 0 đứng đầu. Câu trả lời thực tế là:```
YES
9
10
```Một trường hợp tế nhị khác xảy ra khi độ dài khác nhau.```
2
??
???
```Số có ba chữ số đều lớn hơn số có hai chữ số. Một chiến lược tham lam nên khai thác điều này và chọn số có hai chữ số hợp lệ nhỏ nhất, không lãng phí công sức khi thử các giá trị lớn hơn. 

Các số 0 đứng đầu là một nguồn sai lầm phổ biến khác.```
1
?5
```Câu trả lời có thể là 15, 25, ..., 95, nhưng không bao giờ là 05. Chữ số đầu tiên phải khác 0. 

Cuối cùng, một số trường hợp là không thể.```
2
99
??
```Bất kỳ sự hoàn thành hai chữ số nào đều có nhiều nhất là 99, do đó không tồn tại giá trị nào lớn hơn. Đầu ra đúng là:```
NO
```Thử thách ở đây là tìm ra, đối với mọi vị trí, số lần hoàn thành hợp lệ nhỏ nhất lớn hơn con số đã chọn trước đó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra mọi khả năng hoàn thành có thể có của mỗi mẫu và sau đó tìm kiếm một chuỗi tăng dần. Ngay cả đối với một chuỗi có độ dài 8, có thể có$10^8$ứng viên. Nhân số đó với$10^5$vị trí là hoàn toàn không thể thực hiện được. 

Cấu trúc của vấn đề gợi ý một quan điểm khác. Sau khi giá trị trước đó được cố định, vị trí hiện tại chỉ cần một điều: số nhỏ nhất khớp với mẫu của nó lớn hơn giá trị trước đó. Nếu chúng ta luôn có thể tìm thấy giá trị khả thi tối thiểu đó, thì chúng ta sẽ không bao giờ gây tổn hại đến các vị thế trong tương lai vì bất kỳ lựa chọn nào lớn hơn chỉ khiến những ràng buộc sau này trở nên khó khăn hơn. 

Điều này biến vấn đề thành một vấn đề con lặp lại: 

Cho một chuỗi mẫu và giới hạn dưới$L$, tìm số tối thiểu khớp với mẫu có giá trị lớn hơn$L$. 

Giới hạn độ dài là 8 giúp việc này dễ dàng hơn nhiều. Bất kỳ số ứng cử viên nào cũng phù hợp với một số nguyên tiêu chuẩn. Thay vì tìm kiếm trong phạm vi số lớn, chúng ta có thể xây dựng câu trả lời theo từng chữ số. 

Giả sử độ dài mẫu nhỏ hơn số chữ số của$L+1$. Khi đó không có nghiệm nào tồn tại vì mọi ứng cử viên sẽ có ít chữ số hơn. 

Giả sử chiều dài mẫu lớn hơn. Khi đó mọi ứng viên sẽ tự động vượt quá$L$và chúng tôi chỉ cần xây dựng số hợp lệ nhỏ nhất phù hợp với mẫu. 

Trường hợp thú vị là khi cả hai độ dài đều bằng nhau. Chúng ta cần sự hoàn thành nhỏ nhất về mặt từ điển ít nhất là$L+1$. Vì độ dài tối đa là 8 nên chúng ta có thể sử dụng cấu trúc tham lam kiểu chữ số-DP. Tại mỗi vị trí, chúng tôi thử các chữ số được phép từ nhỏ nhất đến lớn nhất trong khi theo dõi xem chúng tôi đã lớn hơn tiền tố bị ràng buộc hay chưa. Bởi vì chỉ có 8 vị trí tồn tại, ngay cả một tìm kiếm đệ quy đơn giản cũng có hiệu quả là thời gian không đổi. 

Điều này đưa ra một giải pháp tuyến tính trên chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số mũ của số '?' | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

Hằng số ẩn xuất phát từ độ dài tối đa 8, được cố định. 

## Hướng dẫn thuật toán 

### Tìm mức hoàn thành hợp lệ tối thiểu trên giới hạn 

Đối với một chuỗi mẫu`s`và giá trị trước đó`prev`, chúng ta phải tìm số phù hợp nhỏ nhất lớn hơn`prev`. 

1. Tính toán`target = prev + 1`. 
2. Nếu`len(s) < len(str(target))`, trả về thất bại. 

Mọi sự hoàn thành đều có ít chữ số hơn`target`, do đó nó không thể đạt được giá trị yêu cầu. 
3. Nếu`len(s) > len(str(target))`, xây dựng số hợp lệ nhỏ nhất phù hợp với mẫu. 

Số nào có nhiều chữ số hơn thì tự động lớn hơn`prev`. 
4. If the lengths are equal, build the lexicographically smallest completion that is at least`target`. 
5. Xử lý các chữ số từ trái sang phải bằng đệ quy. 

Theo dõi xem tiền tố đã chọn có lớn hơn tiền tố tương ứng của`target`. 
6. Tại vị trí`i`, liệt kê tất cả các chữ số được mẫu cho phép. 

Nếu vị trí đầu tiên là dấu chấm hỏi thì chữ số 0 bị cấm. 
7. Nếu tiền tố vẫn bằng tiền tố đích thì không được phép sử dụng các chữ số nhỏ hơn chữ số đích ở vị trí này. 
8. Thử các chữ số theo thứ tự tăng dần. 

Lần tiếp tục thành công đầu tiên mang lại số hợp lệ nhỏ nhất. 
9. Nếu không có chữ số nào hoạt động, trả về lỗi. 
10. Bắt đầu với`prev = 0`, xử lý chuỗi từ trái sang phải. 

Lưu trữ từng giá trị đã chọn và sử dụng nó làm giới hạn dưới tiếp theo. 
11. Nếu bất kỳ vị trí nào bị lỗi, xuất ra`"NO"`. 
12. Nếu không thì xuất ra`"YES"`và trình tự đã xây dựng. 

### Tại sao nó hoạt động 

Sau khi xử lý vị trí`i`, thuật toán lưu trữ giá trị nhỏ nhất có thể phù hợp với mẫu đó và vẫn vượt quá số đã chọn trước đó. Bất kỳ lựa chọn hợp lệ thay thế nào cũng sẽ lớn hơn hoặc bằng lựa chọn đã chọn. 

Giả sử tồn tại một giải pháp hoàn chỉnh. Việc thay thế bất kỳ giá trị đã chọn nào bằng một giá trị hợp lệ lớn hơn không thể làm cho các vị trí trong tương lai trở nên dễ dàng hơn vì mọi phần tử sau này phải lớn hơn phần tử hiện tại. Lựa chọn khả thi nhỏ nhất ít nhất luôn tốt bằng mọi lựa chọn khả thi khác. 

Việc xây dựng chữ số cũng là tối ưu. Bất cứ khi nào có thể có nhiều chữ số ở một vị trí, hãy thử chúng theo thứ tự tăng dần và chấp nhận lần tiếp tục thành công đầu tiên sẽ tạo ra sự hoàn thành hợp lệ nhỏ nhất về mặt từ điển. Đối với các số có độ dài cố định, thứ tự từ điển và thứ tự số trùng nhau. 

Bằng quy nạp theo trình tự, nếu một nghiệm tồn tại thì thuật toán sẽ không bao giờ loại bỏ nó bằng cách đưa ra một lựa chọn lớn không cần thiết. Nếu thuật toán báo cáo lỗi thì không có sự hoàn thành hợp lệ nào cho vị trí đó, do đó không có giải pháp tổng thể nào tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def smallest_any(pattern):
    res = []
    m = len(pattern)

    for i, ch in enumerate(pattern):
        if ch != '?':
            if i == 0 and ch == '0':
                return None
            res.append(ch)
        else:
            if i == 0:
                res.append('1')
            else:
                res.append('0')

    return ''.join(res)

def build_ge(pattern, target):
    m = len(pattern)
    t = str(target)

    if len(t) > m:
        return None

    if len(t) < m:
        return smallest_any(pattern)

    memo = {}

    def dfs(pos, greater):
        if pos == m:
            return ""

        key = (pos, greater)
        if key in memo:
            return memo[key]

        if pattern[pos] == '?':
            digits = range(10)
        else:
            digits = [int(pattern[pos])]

        for d in digits:
            if pos == 0 and d == 0:
                continue

            td = int(t[pos])

            if not greater and d < td:
                continue

            ngreater = greater or (d > td)

            suffix = dfs(pos + 1, ngreater)
            if suffix is not None:
                memo[key] = str(d) + suffix
                return memo[key]

        memo[key] = None
        return None

    return dfs(0, False)

def solve():
    n = int(input())
    patterns = [input().strip() for _ in range(n)]

    ans = []
    prev = 0

    for s in patterns:
        cur = build_ge(s, prev + 1)
        if cur is None:
            print("NO")
            return

        val = int(cur)
        ans.append(cur)
        prev = val

    print("YES")
    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Người trợ giúp`smallest_any`được sử dụng khi độ dài mẫu đã lớn hơn độ dài giới hạn yêu cầu. Trong tình huống đó, mọi sự hoàn thành hợp lệ đều tự động thỏa mãn điều kiện tăng dần, vì vậy chúng ta chỉ cần xây dựng số pháp lý nhỏ nhất. 

chức năng`build_ge`xử lý công việc thực tế. Khi độ dài khớp nhau, nó sẽ thực hiện tìm kiếm từng chữ số. Trạng thái bao gồm vị trí hiện tại và một boolean cho biết tiền tố được xây dựng đã lớn hơn tiền tố đích hay chưa. Vì có tối đa 8 vị trí và chỉ có hai giá trị có thể có của boolean nên bảng ghi nhớ chứa tối đa 16 trạng thái. 

Tìm kiếm đệ quy luôn thử các chữ số theo thứ tự tăng dần. Nhánh thành công đầu tiên là lần hoàn thành hợp lệ tối thiểu. Đây là chi tiết quan trọng giúp thực hiện lựa chọn tham lam một cách chính xác. 

Vòng lặp chính xử lý chuỗi từ trái sang phải. Khi một giá trị được chọn, nó sẽ trở thành giới hạn dưới cho vị trí tiếp theo. Vì mọi giá trị được chọn đều là tối thiểu trong số tất cả các lựa chọn khả thi nên các vị trí trong tương lai vẫn không bị ràng buộc ở mức tối đa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
?
18
1?
```| Vị trí | Mẫu | Bắt buộc > | Được chọn | 
| --- | --- | --- | --- | 
| 1 | ? | 0 | 1 | 
| 2 | 18 | 1 | 18 | 
| 3 | 1? | 18 | 19 | 

Đầu ra:```
YES
1
18
19
```Mẫu cuối cùng chỉ có thể tạo ra 19. Các giá trị từ 10 đến 18 không thành công vì chuỗi phải tăng nghiêm ngặt. 

### Ví dụ 2 

đầu vào:```
3
??
??
??
```| Vị trí | Mẫu | Bắt buộc > | Được chọn | 
| --- | --- | --- | --- | 
| 1 | ?? | 0 | 10 | 
| 2 | ?? | 10 | 11 | 
| 3 | ?? | 11 | 12 | 

Đầu ra:```
YES
10
11
12
```Ví dụ này minh họa tính chất tham lam trung tâm. Việc chọn giá trị khả thi nhỏ nhất ở mỗi bước đương nhiên sẽ để lại khoảng trống tối đa cho các vị trí sau này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi mẫu được xử lý trong O(8 × 2 × 10), không đổi | 
| Không gian | O(1) | DP chỉ chứa một số trạng thái không đổi | 

Vì mỗi độ dài chuỗi được giới hạn bởi 8 nên công việc trên mỗi phần tử không bao giờ tăng theo kích thước đầu vào. Ngay cả đối với$n = 10^5$, thuật toán chỉ thực hiện vài triệu thao tác nguyên thủy, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve_io(inp: str) -> str:
    input = io.StringIO(inp).readline

    def smallest_any(pattern):
        res = []
        for i, ch in enumerate(pattern):
            if ch != '?':
                if i == 0 and ch == '0':
                    return None
                res.append(ch)
            else:
                res.append('1' if i == 0 else '0')
        return ''.join(res)

    def build_ge(pattern, target):
        m = len(pattern)
        t = str(target)

        if len(t) > m:
            return None

        if len(t) < m:
            return smallest_any(pattern)

        memo = {}

        def dfs(pos, greater):
            if pos == m:
                return ""

            key = (pos, greater)
            if key in memo:
                return memo[key]

            digits = range(10) if pattern[pos] == '?' else [int(pattern[pos])]

            for d in digits:
                if pos == 0 and d == 0:
                    continue

                td = int(t[pos])

                if not greater and d < td:
                    continue

                suf = dfs(pos + 1, greater or d > td)
                if suf is not None:
                    memo[key] = str(d) + suf
                    return memo[key]

            memo[key] = None
            return None

        return dfs(0, False)

    n = int(input())
    prev = 0
    ans = []

    for _ in range(n):
        s = input().strip()
        cur = build_ge(s, prev + 1)
        if cur is None:
            return "NO\n"
        ans.append(cur)
        prev = int(cur)

    return "YES\n" + "\n".join(ans) + "\n"

def run(inp: str) -> str:
    return solve_io(inp)

# provided sample
assert run("3\n?\n18\n1?\n") == "YES\n1\n18\n19\n"

# minimum size
assert run("1\n?\n") == "YES\n1\n"

# impossible case
assert run("2\n99\n??\n") == "NO\n"

# increasing through same-length patterns
assert run("3\n??\n??\n??\n") == "YES\n10\n11\n12\n"

# length increase
assert run("2\n9\n???\n") == "YES\n9\n100\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 ?`|`1`| Kích thước đầu vào tối thiểu | 
|`99, ??`|`NO`| Điều kiện tăng không thể | 
|`??, ??, ??`|`10,11,12`| Sự lựa chọn tham lam lặp đi lặp lại | 
|`9, ???`|`9,100`| Chuyển sang độ dài chữ số lớn hơn | 

## Vỏ cạnh 

Hãy xem xét:```
2
99
??
```Giá trị đầu tiên được cố định là 99. Mẫu thứ hai chỉ có thể tạo số có hai chữ số. Thuật toán tính toán`target = 100`. Vì độ dài mục tiêu là 3 trong khi độ dài mẫu là 2 nên ngay lập tức nó trả về lỗi. Đầu ra:```
NO
```Bây giờ hãy xem xét:```
2
?
100
```Mẫu đầu tiên trở thành 1. Đối với mẫu thứ hai, giới hạn bắt buộc chỉ là 2. Độ dài của nó lớn hơn độ dài mục tiêu, do đó thuật toán xây dựng mức hoàn thành hợp lệ nhỏ nhất, cụ thể là 100. Đầu ra:```
YES
1
100
```Hãy xem xét bẫy số 0 đứng đầu:```
1
?5
```Thuật toán thử các chữ số cho vị trí đầu tiên bắt đầu từ 1 vì số 0 bị cấm ở vị trí đầu tiên. Mức hoàn thành nhỏ nhất là 15, không phải 05. Đầu ra:```
YES
15
```Cuối cùng:```
2
1?
1?
```Mẫu đầu tiên trở thành 10. Đối với mẫu thứ hai, mục tiêu là 11. Trong quá trình xây dựng chữ số, chữ số đầu tiên phải giữ nguyên là 1. Chữ số thứ hai không thể bằng 0 vì chữ số đó sẽ tạo ra 10, thấp hơn mục tiêu. Việc tìm kiếm chọn 1, tạo ra 11. Kết quả:```
YES
10
11
```Ví dụ này cho thấy cách so sánh từng chữ số với giới hạn dưới thực thi mức tăng nghiêm ngặt mà không bao giờ liệt kê tất cả các số có thể.
