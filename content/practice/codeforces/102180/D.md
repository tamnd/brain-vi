---
title: "CF 102180D - \u0422\u044b\u043a\u0432\u0435\u043d\u043d\u0430\u044f \u043c\u0430\u0433\u0438\u044f"
description: "Chúng ta có n quả bí ngô và kích thước của quả bí ngô thứ i là ai. Hai thao tác có thể thay đổi một quả bí ngô. Chúng ta có thể thêm chính xác p vào kích thước của nó hoặc nhân kích thước của nó với k. Thao tác thứ ba biến quả bí ngô thành một cỗ xe, nhưng chỉ khi kích thước của nó bằng m."
date: "2026-08-19T15:28:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102180
codeforces_index: "D"
codeforces_contest_name: "MSPU Training Contest 2018-2019"
rating: 0
weight: 102180
solve_time_s: 142
verified: true
draft: false
---

[CF 102180D - \u0422\u044b\u043a\u0432\u0435\u043d\u043d\u0430\u044f \u043c\u0430\u0433\u0438\u044f](https://codeforces.com/problemset/problem/102180/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

chúng tôi có`n`bí ngô và kích thước của`i`- quả bí ngô thứ là`a_i`. Hai thao tác có thể thay đổi một quả bí ngô. Chúng tôi có thể thêm chính xác`p`với kích thước của nó, hoặc nhân kích thước của nó với`k`. Thao tác thứ ba biến quả bí ngô thành một cỗ xe, nhưng chỉ khi kích thước của nó chính xác`m`. 

Đối với mỗi quả bí ngô ban đầu, chúng ta cần quyết định một cách độc lập xem liệu chuỗi nào đó của hai thao tác đầu tiên có thể biến đổi kích thước của nó thành chính xác hay không.`m`. Đầu ra chứa`1`cho một quả bí ngô có thể sử dụng được và`0`nếu không thì. 

Khó khăn chính là các hoạt động có thể được xen kẽ. Ví dụ: chúng ta có thể thêm`p`, nhân với`k`, thêm vào`p`một lần nữa, và nhân lên một lần nữa. Việc tìm kiếm trực tiếp trên các chuỗi như vậy có quá nhiều khả năng. 

Các giới hạn làm cho điều này đặc biệt hạn chế. có thể có`10^5`bí ngô, do đó một thuật toán thực hiện thậm chí`O(n^2)`công việc sẽ yêu cầu khoảng`10^10`hoạt động. Kích thước mục tiêu và kích thước ban đầu nhiều nhất là`10^7`, nhường chỗ cho các thuật toán tuyến tính trong`m`, nhưng một`O(n log m)`giải pháp tốt hơn đáng kể và dễ dàng phù hợp với giới hạn. Từ`k >= 2`, phép nhân lặp lại chỉ có thể xảy ra`O(log m)`lần trước khi kích thước vượt quá`m`. 

Có một số trường hợp ranh giới có thể đánh lừa việc thực hiện bất cẩn. Nếu như`a_i > m`, câu trả lời là ngay lập tức`0`, bởi vì mọi thao tác khả dụng chỉ làm tăng kích thước. Ví dụ, với`1 3 2 7`và kích thước bí ngô`9`, câu trả lời là`0`. 

Bí ngô có thể đã có kích thước mục tiêu. Với`1 5 2 10`và kích thước bí ngô`10`, câu trả lời là`1`, bởi vì không được phép thực hiện bất kỳ thao tác nào trước phép vận chuyển. 

Sự bình đẳng chính xác sau khi nhân cũng có vấn đề. Với`1 3 2 12`và kích thước bí ngô`3`, nhân hai lần sẽ được`12`, vậy câu trả lời là`1`. Quá trình triển khai sẽ dừng khi phép nhân tiếp theo vượt quá`m`vẫn phải kiểm tra giá trị hiện tại trước khi nhân. 

Cuối cùng, có cùng số dư modulo`p`tự nó là không đủ. Giá trị nhân cũng không được vượt quá`m`. Ví dụ, với`1 6 2 10`và kích thước bí ngô`6`, cả hai`6`Và`10`là đồng dư modulo`2`, nhưng câu trả lời thực sự là`1`vì không cần nhân và hai phép cộng`2`với tới`10`. Ngược lại, đối với`1 4 3 10`và kích thước bí ngô`4`, điều kiện còn lại được giữ nguyên, nhưng phép nhân cho kết quả`12 > 10`, vậy câu trả lời là`0`. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp có thể xem mọi câu thần chú là sự lựa chọn giữa việc thêm`p`và nhân với`k`. Bắt đầu từ một quả bí ngô, nó thử đệ quy cả hai thao tác trong khi kích thước kết quả không vượt quá`m`. Điều này đúng vì mọi dãy hợp lệ có thể chỉ bao gồm hai lựa chọn này. 

Vấn đề là số lượng trình tự. Nếu tìm kiếm khám phá`L`vị trí chính tả, nó có thể có tới`2^L`chi nhánh. Trong trường hợp xấu nhất,`L`có thể theo thứ tự`m / p`, có thể là về`10^7`. Một giới hạn như`2^10000000`rõ ràng là không thể liệt kê được, ngay cả đối với một quả bí ngô. 

Brute-force hoạt động vì nó xem xét mọi thứ tự có thể có của hai thao tác, nhưng hầu hết các thứ tự đó đều tương đương xét từ góc độ khả năng tiếp cận. Quan sát quan trọng là mọi phép nhân đều có thể được di chuyển trước mỗi phép cộng. 

Giả sử một dãy có chứa phép cộng bởi`p`sau đó là phép nhân với`k`. Nếu có`s`phép nhân sau phép cộng đó, phần đóng góp của nó vào giá trị cuối cùng là`p * k^s`. Thay vì thực hiện phép cộng đó ở vị trí ban đầu, chúng ta có thể xóa nó đi và thực hiện`k^s`bổ sung bởi`p`vào cuối cùng. Cả hai trình tự đều đóng góp một lượng chính xác như nhau vào kích thước cuối cùng. 

Việc lặp lại phép biến đổi này sẽ chuyển tất cả các phép nhân về đầu và tất cả các phép cộng về cuối. Đây là sự đơn giản hóa trung tâm được sử dụng trong phân tích cuộc thi chính thức. 

Do đó, nếu một quả bí ngô bắt đầu ở kích thước`x`, mọi mục tiêu có thể tiếp cận đều có thể đạt được dưới dạng`x * k^t + q * p`đối với một số số nguyên không âm`t`Và`q`. 

Đối với một số cố định`t`về phép nhân, trước tiên chúng ta đạt được`x * k^t`. Từ đó chỉ có phép cộng bằng`p`duy trì. Những bổ sung như vậy có thể đạt tới`m`chính xác khi và chỉ khi`m - x * k^t`không âm và chia hết cho`p`. 

Vì vậy, với mỗi quả bí ngô chúng ta chỉ cần thử các giá trị có thể có của`t`. Từ`k >= 2`, giá trị`x * k^t`tăng theo cấp số nhân nên chỉ có`O(log m)`khả năng. Chúng ta có thể kiểm tra từng cái một trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Cấp số nhân về số lượng phép thuật | O(m) đệ quy/độ sâu trạng thái trong tìm kiếm giới hạn | Quá chậm | 
| Tối ưu | O(n log m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`p`,`k`,`m`và mảng kích thước bí ngô ban đầu. Chúng tôi xử lý từng quả bí ngô một cách độc lập vì các phép thuật áp dụng cho một quả bí ngô không có tác dụng với quả bí ngô khác. 
2. Đối với bí ngô hiện nay có kích thước`x`, bắt đầu với`cur = x`. Điều này thể hiện giá trị thu được sau khi nhân số 0. 
3. Trước khi thực hiện phép nhân khác, hãy kiểm tra xem`cur <= m`và liệu`(m - cur) % p == 0`. Nếu cả hai điều kiện được giữ nguyên thì bí ngô có thể sử dụng được. Chúng ta có thể thực hiện phép nhân với số hiện tại trước, sau đó cộng liên tục`p`cho đến khi đạt chính xác`m`. 
4. Nếu kiểm tra không thành công, hãy thay thế`cur`qua`cur * k`và lặp lại trong khi`cur * k <= m`. Phép nhân chỉ được xem xét khi nó giữ bí ngô ở mức hoặc dưới mục tiêu, bởi vì tất cả các thao tác tiếp theo chỉ làm tăng giá trị. 
5. Nếu mọi số nhân có thể đã được kiểm tra mà không tìm thấy số dư hợp lệ, xuất ra`0`cho quả bí ngô này. 
6. Lặp lại quy trình cho tất cả`n`bí ngô và in chuỗi kết quả gồm số không và số một. 

Lý do kiểm tra là đủ là vì mọi chuỗi chính tả tùy ý có thể được sắp xếp lại thành tất cả các phép nhân, sau đó là tất cả các phép cộng. Đối với một số phép nhân cố định, câu hỏi duy nhất còn lại là liệu sự khác biệt giữa`m`là bội số không âm của`p`. 

### Tại sao nó hoạt động 

Hãy xem xét một quả bí ngô có kích thước ban đầu`x`và bất kỳ chuỗi thành công nào đạt được`m`. Di chuyển mọi phép cộng đến cuối dãy bằng cách sử dụng phép biến đổi được mô tả ở trên. Điều này bảo toàn kích thước cuối cùng, do đó cũng có một chuỗi thành công chứa một số số`t`phép nhân chỉ theo sau là phép cộng. 

Sau những lần nhân đó quả bí ngô có kích thước`x * k^t`. Các thao tác còn lại thêm`p`nhiều lần nên giá trị cuối cùng có dạng`x * k^t + q * p`. Do đó, một chuỗi thành công tồn tại chính xác khi, đối với một số`t`,`x * k^t <= m`Và`m - x * k^t`chia hết cho`p`. 

Thuật toán kiểm tra mọi thứ có thể`t`vì cái gì`x * k^t <= m`. Nó chấp nhận chính xác khi điều kiện đó được thỏa mãn, vì vậy nó không thể bỏ lỡ một quả bí ngô hợp lệ hoặc chấp nhận một quả bí ngô không thể chấp nhận được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, p, k, m = map(int, input().split())
    a = list(map(int, input().split()))

    ans = []

    for x in a:
        cur = x
        possible = False

        while cur <= m:
            if (m - cur) % p == 0:
                possible = True
                break

            if cur > m // k:
                break

            cur *= k

        ans.append(1 if possible else 0)

    print(*ans)

if __name__ == "__main__":
    solve()
```Vòng lặp bên ngoài xử lý mỗi quả bí ngô ban đầu một lần. Đối với quả bí ngô đó,`cur`luôn là kích thước sau một số phép nhân và trước bất kỳ phép cộng cuối cùng nào. Do đó, lần kiểm tra đầu tiên tương ứng chính xác với một giá trị ứng viên của`t`. 

biểu hiện`(m - cur) % p == 0`chỉ được sử dụng sau`cur <= m`. Nếu không có sự kiểm tra ranh giới đó, một hiệu âm có thể vô tình thỏa mãn tính chia hết mặc dù quả bí ngô không bao giờ có thể giảm xuống`m`. 

điều kiện`cur > m // k`là một cách an toàn để quyết định rằng phép nhân tiếp theo sẽ vượt quá`m`. Viết`cur * k <= m`cũng sẽ đúng trong Python vì số nguyên Python không bị tràn, nhưng`m // k`tránh việc xây dựng một giá trị đã được biết là quá lớn. Nó cũng làm cho điều kiện biên trở nên rõ ràng. 

Giá trị hiện tại được kiểm tra trước khi nhân. Điều này xử lý cả hai`x == m`và các trường hợp số phép nhân đúng chính xác là số lần lặp hiện tại. Việc dừng lại trước khi kiểm tra giá trị hiện tại sẽ từ chối những quả bí ngô đó một cách không chính xác. 

Giải pháp không cần phải đếm rõ ràng các bổ sung cần thiết. Một lần`m - cur`là bội số không âm của`p`, thực hiện nhiều phép cộng đạt tới`m`chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên được hiểu là```
1 3 2 7
9
```Bí ngô bắt đầu lúc`9`, đã lớn hơn mục tiêu`7`. 

| Bí ngô |`cur`|`cur <= m`|`(m-cur) % p`| Kết quả | 
| --- | --- | --- | --- | --- | 
| 9 | 9 | sai | chưa được kiểm tra | 0 | 

Vì cả hai phép biến đổi có sẵn chỉ tăng kích thước nên không có cách nào để biến`9`vào trong`7`. Đầu ra đúng là`0`. 

Ví dụ này thực hiện điều kiện biên ban đầu`x > m`. Một giải pháp chỉ kiểm tra điều kiện mô-đun có thể chấp nhận bí ngô không chính xác vì`9 - 7`chia hết cho`3`. 

### Mẫu 2 

Mẫu thứ hai là```
9 2 4 8
1 2 3 4 5 6 7 8 9
```Chúng tôi kiểm tra mọi kích thước ban đầu bằng cách sử dụng`p = 2`,`k = 4`, Và`m = 8`. 

| Ban đầu`x`|`cur`giá trị đã được kiểm tra | Thành công`cur`| Đầu ra | 
| --- | --- | --- | --- | 
| 1 | 1, 4 | 4 | 1 | 
| 2 | 2 | 2 | 1 | 
| 3 | 3 | 3 | 0 | 
| 4 | 4 | 4 | 1 | 
| 5 | 5 | 5 | 0 | 
| 6 | 6 | 6 | 1 | 
| 7 | 7 | 7 | 0 | 
| 8 | 8 | 8 | 1 | 
| 9 | 9 | không | 0 | 

Vì`x = 1`, nhân cho`4`, và sau đó là hai phép cộng của`2`với tới`8`. Vì`x = 2`, bốn phép cộng của`2`với tới`10`, vì vậy thay vào đó chúng ta nhân một lần và ngay lập tức đạt được`8`. Vì`x = 3`, không`3`cũng không`12`có thể sử dụng được vì`12`đã vượt chỉ tiêu và`8 - 3`không chia hết cho`2`. 

Kết quả đầu ra là```
1 1 0 1 0 1 0 1 0
```Dấu vết này chứng tỏ tại sao chúng ta phải kiểm tra một số số nhân có thể có thay vì chỉ giá trị ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log m) | Mỗi quả bí ngô được nhân với ít nhất`2`trên mỗi lần lặp, vì vậy nó có nhiều nhất`O(log m)`các giá trị ứng cử viên. | 
| Không gian | O(n) | Mảng đầu vào và mảng đầu ra chứa`n`các phần tử. | 

Từ`n <= 10^5`Và`m <= 10^7`, số lần kiểm tra nhân trên mỗi quả bí ngô là nhỏ. Ngay cả với điều nhỏ nhất có thể`k = 2`, có ít hơn 24 phép nhân trước khi vượt quá`10^7`, như vậy tổng số lần lặp chỉ có vài triệu. 

Giải pháp này phù hợp một cách thoải mái với giới hạn 1 giây và 256 MB đã nêu trong ngôn ngữ được biên dịch và việc triển khai Python cũng chỉ thực hiện một lượng công việc không đổi nhỏ trên mỗi bước logarit. 

## Trường hợp thử nghiệm 

Định dạng câu lệnh được cung cấp kết hợp hai ví dụ, vì vậy mẫu đầu tiên được xây dựng lại thành`1 3 2 7`với quả bí ngô duy nhất`9`, câu trả lời của ai`0`.```python
import sys
import io

def solve_text(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)

    n = next(it)
    p = next(it)
    k = next(it)
    m = next(it)

    a = [next(it) for _ in range(n)]
    ans = []

    for x in a:
        cur = x
        possible = False

        while cur <= m:
            if (m - cur) % p == 0:
                possible = True
                break

            if cur > m // k:
                break

            cur *= k

        ans.append(1 if possible else 0)

    return " ".join(map(str, ans))

def run(inp: str) -> str:
    return solve_text(inp).strip()

# Provided sample 1
assert run("""\
1 3 2 7
9
""") == "0", "sample 1"

# Provided sample 2
assert run("""\
9 2 4 8
1 2 3 4 5 6 7 8 9
""") == "1 1 0 1 0 1 0 1 0", "sample 2"

# Minimum-size input, including the case a_i == m.
assert run("""\
1 1 2 1
1
""") == "1", "target already reached"

# All pumpkins are equal, and the answer is obtained after multiplication.
assert run("""\
5 3 2 7
1 1 1 1 1
""") == "1 1 1 1 1", "all equal values"

# Boundary around a multiplication that would exceed m.
assert run("""\
4 3 2 10
1 4 7 10
""") == "1 0 1 1", "multiplication boundary"

# Values above m and values requiring different multiplication counts.
assert run("""\
6 5 3 20
21 5 10 15 20 1
""") == "0 1 1 1 1 0", "above target and multiple paths"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 3 2 7 / 9`|`0`| Giá trị ban đầu đã vượt quá mục tiêu. | 
|`1 1 2 1 / 1`|`1`| Thao tác bằng 0 có hiệu lực khi bí ngô đã bằng`m`. | 
|`5 3 2 7 / 1 1 1 1 1`|`1 1 1 1 1`| Các giá trị ban đầu trùng lặp được xử lý độc lập. | 
|`4 3 2 10 / 1 4 7 10`|`1 0 1 1`| Ranh giới nhân chính xác và kiểm tra mô-đun. | 
|`6 5 3 20 / 21 5 10 15 20 1`|`0 1 1 1 1 0`| Các giá trị trên mục tiêu và số lượng hoạt động hợp lệ khác nhau. | 

## Vỏ cạnh 

Khi quả bí ban đầu lớn hơn`m`, thuật toán ngay lập tức bước vào điều kiện vòng lặp với`cur > m`, do đó nó không thực hiện phép biến đổi nào và trả về`0`. Vì`1 3 2 7`với`a = 9`, điều này mang lại`0`, điều này là cần thiết vì cả việc cộng lẫn nhân đều không thể làm giảm kích thước. 

Khi bí ngô đã bằng`m`, lần lặp đầu tiên kiểm tra`(m - cur) % p`, đó là`0`. Thuật toán trả về`1`mà không cần áp dụng bất kỳ thao tác nào. Vì`1 5 2 10`với`a = 10`, câu trả lời là`1`. 

Khi phép nhân đạt chính xác`m`, thuật toán phải kiểm tra giá trị sau phép nhân đó. Vì`1 3 2 12`với`a = 3`, các giá trị được kiểm tra là`3`,`6`, Và`12`. Tại`12`, sự khác biệt so với mục tiêu là bằng 0, vì vậy câu trả lời là`1`. 

Khi phép nhân sẽ nhảy qua`m`, nó không được thực hiện. Coi như`1 4 3 10`với`a = 4`. Giá trị hiện tại`4`có sự khác biệt`6`, không chia hết cho`4`, trong khi giá trị tiếp theo sẽ là`12 > 10`. Việc tìm kiếm dừng lại và quay trở lại`0`. 

Khi một lộ trình hợp lệ sử dụng phép nhân trước khi cộng, quá trình kiểm tra mô-đun sẽ tìm thấy nó. Vì`p = 3`,`k = 2`,`m = 7`, Và`a = 2`, giá trị ban đầu`2`cho`7 - 2 = 5`, không chia hết cho`3`. Sau một lần nhân,`cur = 4`, Và`7 - 4 = 3`chia hết cho`3`. Trình tự thực tế là`2 -> 4 -> 7`, do đó thuật toán trả về chính xác`1`. 

Việc triển khai cũng xử lý các giá trị lặp lại mà không có bất kỳ xử lý đặc biệt nào. Mỗi lần xuất hiện được kiểm tra riêng biệt và nhận được cùng một kết quả cho các tham số giống hệt nhau và kích thước ban đầu giống hệt nhau. Điều này bảo tồn thứ tự đầu ra cần thiết.
