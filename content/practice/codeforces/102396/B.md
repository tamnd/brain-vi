---
title: "CF 102396B - Khoảng cách tiền mặt"
description: "Chúng ta có số dư tài khoản ban đầu và n giao dịch đều phải diễn ra trong m ngày tiếp theo. Giao dịch i thay đổi số dư theo số [i], nhưng ngày chính xác của nó có thể là bất kỳ ngày nào trong khoảng [từ [i], đến [i]]."
date: "2026-08-11T15:26:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102396
codeforces_index: "B"
codeforces_contest_name: "2019-2020 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 19)"
rating: 0
weight: 102396
solve_time_s: 658
verified: true
draft: false
---

[CF 102396B - Khoảng trống tiền mặt](https://codeforces.com/problemset/problem/102396/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 10 phút 58 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có số dư tài khoản ban đầu`s`Và`n`tất cả các giao dịch phải xảy ra trong thời gian tiếp theo`m`ngày. Giao dịch`i`thay đổi số dư bằng cách`count[i]`, nhưng ngày chính xác của nó có thể là bất kỳ ngày nào trong khoảng thời gian bao gồm`[from[i], to[i]]`. Nếu một số giao dịch diễn ra trong cùng một ngày, trật tự nội bộ của chúng sẽ không bị hạn chế. 

Chúng ta cần quyết định xem có ít nhất một lựa chọn hợp lệ về ngày giao dịch và đặt hàng trong ngày khiến số dư âm tại một thời điểm nào đó hay không. Số dư âm là chênh lệch tiền mặt. Nếu số dư bằng 0 thì điều đó vẫn an toàn vì công ty có đủ tiền để thanh toán. 

Phần hữu ích của đầu vào là`n`Và`m`nhiều nhất là cả hai`1000`. MỘT`O(nm)`giải pháp thực hiện tối đa khoảng một triệu lượt kiểm tra trong ngày giao dịch, điều này dễ dàng hợp lý trong thời hạn. Quan trọng hơn, các giới hạn loại trừ việc liệt kê các lịch trình có thể xảy ra, bởi vì mỗi giao dịch có thể có nhiều ngày có thể xảy ra. Với khoảng thời gian kéo dài tất cả`m`ngày, chỉ cần ấn định một ngày cho mỗi giao dịch sẽ tạo ra`m^n`khả năng. Vì`n = m = 1000`, đó là`1000^1000`nhiệm vụ trước khi chúng ta xem xét thứ tự các giao dịch trong một ngày. 

Một số trường hợp biên có thể làm sai một giải pháp hợp lý. Đầu tiên, đạt đến mức 0 không phải là chênh lệch tiền mặt. Ví dụ,```
1 1 5
-5 1 1
```có đầu ra`NO`, vì giao dịch duy nhất khiến số dư ở mức 0. Một tấm séc sử dụng`balance <= 0`thay vì`balance < 0`sẽ trả lời sai`YES`. 

Thứ hai, một giao dịch có khoảng thời gian kết thúc trước ngày hiện tại buộc phải xảy ra, trong khi một giao dịch có khoảng thời gian bao gồm ngày hiện tại có thể được lên lịch có chủ ý vào ngày đó. Ví dụ,```
2 2 5
5 1 1
-6 2 2
```có đầu ra`YES`. Giao dịch đầu tiên phải diễn ra vào ngày 1, tạo ra số dư là 10. Giao dịch thứ hai xảy ra vào ngày thứ 2 và giảm số dư xuống còn 4, vì vậy ví dụ cụ thể này thực sự an toàn. Để hiển thị ranh giới một cách chính xác, hãy thay đổi số dư ban đầu thành 0, nhưng các ràng buộc yêu cầu`s >= 1`. Thay vào đó, một ví dụ hợp lệ là```
2 2 5
0 1 1
-6 2 2
```có đầu ra`YES`, vì số dư trở thành`-1`vào ngày thứ 2. Giải pháp vô tình coi các giao dịch kết thúc vào ngày hiện tại là đã hoàn thành trước khi kiểm tra ngày đó có thể phân loại sai những trường hợp như vậy. 

Thứ ba, các giao dịch trong cùng ngày có thể được đặt lệnh đối nghịch. Coi như```
2 1 5
-3 1 1
-3 1 1
```có đầu ra`YES`. Khoản thanh toán đầu tiên để lại 2 euro và khoản thanh toán thứ hai sau đó yêu cầu 3 euro. Ở đây tổng hợp hai khoản thanh toán là đủ, nhưng việc triển khai giả định một thứ tự đầu vào cố định tùy ý có thể không thành công trong trường hợp các giao dịch tích cực và tiêu cực chia sẻ trong một ngày. 

Cuối cùng, một giao dịch có giá trị bằng 0 không có tác dụng gì cả. Ví dụ,```
2 1 5
0 1 1
0 1 1
```có đầu ra`NO`. Những giao dịch như vậy không nên vô tình bị coi là thanh toán tiêu cực khi duy trì tập hợp các giao dịch nguy hiểm đang hoạt động. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ chọn một ngày được phép cho mỗi giao dịch, sau đó mô phỏng lịch trình kết quả. Nếu mỗi khoảng là`[1, m]`, có`m^n`các nhiệm vụ có thể có trong ngày. Nếu chúng ta cũng thử một cách rõ ràng mọi thứ tự có thể có của các giao dịch thì có thể có một yếu tố khác của`n!`, cho`m^n n!`kết hợp ứng cử viên, với`O(n)`làm việc để mô phỏng từng cái một. Đối với những hạn chế tối đa, điều này là vượt xa khả thi. 

Lực lượng vũ phu hoạt động vì nó thực sự khám phá mọi cách hợp lệ mà các giao dịch có thể xảy ra. Nó thất bại vì hầu hết tất cả thông tin đó đều không liên quan đến câu hỏi. Chúng ta không cần phải xây dựng một lịch trình hoàn chỉnh. Chúng ta chỉ cần biết liệu một thời điểm nào đó có thể có số dư âm hay không. 

Sửa một ngày cụ thể`d`và hỏi số dư có thể nhỏ đến mức nào ngay trước hoặc trong khi giao dịch vào ngày đó. Mỗi giao dịch với`to < d`buộc phải xảy ra trước ngày`d`, bất kể chúng ta chọn ngày chính xác như thế nào. Đóng góp của nó phải được tính vào số dư. 

Bây giờ hãy xem xét một giao dịch có khoảng thời gian chứa`d`. Nếu giá trị của nó âm, chúng ta có thể lên lịch vào ngày`d`và thực hiện nó trước tất cả các giao dịch tích cực vào ngày hôm đó. Vì chúng ta đang tìm kiếm số dư nhỏ nhất có thể nên mọi giao dịch âm như vậy phải được đưa vào trước khi kiểm tra chênh lệch tiền mặt. Các giao dịch tích cực có khoảng thời gian chứa`d`có thể bị hoãn lại cho đến sau thời điểm nguy hiểm nên chúng không giúp ngăn chặn khoảng cách. 

Điều này mang lại một đặc tính đơn giản. Cho mỗi ngày`d`, số dư nhỏ nhất có thể đạt được vào thời điểm đó là`initial balance + sum of all transactions with to < d + sum of all negative transactions with from <= d <= to`. 

Nếu giá trị này âm đối với bất kỳ`d`, câu trả lời là`YES`. Ngược lại, nếu nó không bao giờ âm thì không có đơn đặt hàng hợp lệ nào có thể tạo ra chênh lệch tiền mặt, bởi vì biểu thức đã bao gồm mọi giao dịch có thể làm cho số dư nhỏ hơn trước thời điểm đó. 

Chúng ta có thể đánh giá biểu thức này theo thời gian tuyến tính. Đối với số tiền đầu tiên, nhóm các giá trị giao dịch theo ngày kết thúc. Đối với tổng thứ hai, hãy duy trì các giao dịch âm hiện đang hoạt động bằng cách sử dụng một mảng chênh lệch. Giao dịch tiêu cực`[from, to]`đóng góp giá trị của nó bắt đầu từ`from`và ngừng đóng góp sau`to`, vì vậy chúng tôi thêm giá trị của nó tại`from`và trừ nó tại`to + 1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(m^n n! n)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n + m)`|`O(m)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`ending`Ở đâu`ending[d]`lưu trữ tổng của tất cả các thay đổi giao dịch có ngày gần nhất có thể chính xác`d`. Khi chúng tôi chuẩn bị kiểm tra ngày`d`, mọi giao dịch với`to < d`đã buộc phải xảy ra, vì vậy chúng ta có thể tích lũy`ending[d - 1]`thành một giá trị đang chạy được gọi là`forced`. 
2. Tạo mảng khác biệt`active`. Đối với mọi giao dịch âm có khoảng thời gian`[from, to]`và giá trị`count`, thêm vào`count`Tại`active[from]`và trừ`count`Tại`active[to + 1]`. Tổng tiền tố của nó sau đó sẽ bằng tổng giá trị của tất cả các giao dịch âm có khoảng thời gian chứa ngày hiện tại. 
3. Quét ngày từ`1`bởi vì`m`. Vào ngày`d`, đầu tiên thêm`ending[d - 1]`ĐẾN`forced`. Điều này làm cho`forced`bằng với sự đóng góp của mọi giao dịch phải diễn ra nghiêm ngặt trước ngày`d`. 
4. Thêm`active[d]`đến một biến đang chạy`dangerous`. Sau khi cập nhật tiền tố,`dangerous`chính xác là tổng của tất cả các giao dịch âm có thể được thực hiện trong ngày`d`. 
5. Tính toán`balance = s + forced + dangerous`. Đây là số dư nhỏ nhất có thể đạt được trong ngày xử lý`d`, bởi vì mọi giao dịch bắt buộc đều đã xảy ra nên mọi giao dịch tiêu cực hiện có đều có thể được thực hiện ngay bây giờ và các giao dịch tích cực có thể vẫn bị trì hoãn vẫn chưa được thực hiện. 
6. Nếu`balance < 0`, in`YES`ngay lập tức. Nếu không thì tiếp tục vào ngày hôm sau. Nếu mỗi ngày đều an toàn, hãy in`NO`. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ ngày nào`d`. Mỗi giao dịch với`to < d`phải xảy ra trước ngày`d`, vì vậy sự đóng góp của nó là không thể tránh khỏi. Trong số các giao dịch có khoảng thời gian chứa`d`, mọi giao dịch tiêu cực có thể được thực hiện vào ngày`d`và được thực hiện trước các giao dịch tích cực. Bao gồm tất cả chúng sẽ tạo ra số dư tối thiểu có thể đạt được tại thời điểm đó. Không có giao dịch nào khác có thể hạ số dư sớm hơn biểu thức này. Do đó, thuật toán tìm ra số dư âm chính xác khi lịch giao dịch hợp lệ nào đó có thể tạo ra chênh lệch tiền mặt. 

Bất biến trong quá trình quét là`forced + dangerous`thể hiện sự thay đổi giao dịch tích lũy nhỏ nhất có thể có vào ngày hiện tại. Những đóng góp cuối cùng giải thích cho mọi thứ vốn đã không thể tránh khỏi, trong khi những đóng góp tiêu cực tích cực giải thích cho mọi thứ hiện có có thể được cố tình đặt trước thời điểm nguy hiểm. Nếu mức tối thiểu này không bao giờ âm thì mọi lịch trình hợp lệ đều an toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, s = map(int, input().split())

    ending = [0] * (m + 2)
    active = [0] * (m + 2)

    for _ in range(n):
        count, left, right = map(int, input().split())

        ending[right] += count

        if count < 0:
            active[left] += count
            active[right + 1] -= count

    forced = 0
    dangerous = 0

    for day in range(1, m + 1):
        forced += ending[day - 1]
        dangerous += active[day]

        if s + forced + dangerous < 0:
            print("YES")
            return

    print("NO")

if __name__ == "__main__":
    solve()
```các`ending`mảng lưu trữ các đóng góp giao dịch hoàn chỉnh, bao gồm các giá trị dương và âm. Một giao dịch với`to = d`không được thêm vào`forced`cho đến ngày`d + 1`, bởi vì vào ngày`d`nó vẫn có thể được thực thi trong ngày đó và có khả năng là một phần của lệnh nguy hiểm. 

các`active`mảng chỉ chứa các giao dịch tiêu cực. Đối với giao dịch có giá trị`count < 0`và khoảng thời gian`[left, right]`, thêm`count`Tại`left`làm cho nó hoạt động bắt đầu vào đúng ngày pháp lý đầu tiên. Trừ nó tại`right + 1`loại bỏ nó ngay sau ngày pháp lý cuối cùng. Tổng tiền tố vào ngày`d`do đó chứa chính xác các giao dịch tiêu cực thỏa mãn`left <= d <= right`. 

Thứ tự cập nhật trong vòng lặp ngày xử lý các ranh giới một cách chính xác.`ending[day - 1]`được thêm vào trước ngày kiểm tra`day`, trong khi`active[day]`được thêm vào cho chính ngày hiện tại. Sự khác biệt này là điểm phân biệt các giao dịch vốn đã không thể tránh khỏi với các giao dịch vẫn có thể được lên lịch có chủ ý vào ngày hiện tại. 

Số nguyên Python có độ chính xác tùy ý, do đó, tổng trung gian có thể lớn sẽ không bị tràn. Mặc dù tổng số tuyệt đối tối đa có thể đạt tới khoảng`10^9`, việc sử dụng số nguyên Python làm cho việc triển khai độc lập với giới hạn đó. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
4 3 100
100 1 2
-100 1 2
1 2 3
0 3 3
```Giao dịch tiêu cực`-100`hoạt động vào ngày 1 và 2. Giao dịch tích cực kết thúc vào ngày thứ 2 không bị ép buộc trước ngày thứ 2, vì vậy nó không thể được coi là biện pháp bảo vệ khỏi khoảng trống ở đó. 

| Ngày | Buộc trước ngày | Tích cực thay đổi tiêu cực | Số dư tối thiểu | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | -100 | 0 | An toàn | 
| 2 | 0 | -100 | 0 | An toàn | 
| 3 | 1 | 0 | 101 | An toàn | 

Số dư tối thiểu đạt chính xác bằng 0 vào ngày 1 và 2. Số tiền đó không âm, do đó không tồn tại chênh lệch tiền mặt và câu trả lời là`NO`. Ví dụ này cũng giải thích tại sao việc kiểm tra`<= 0`sẽ không chính xác. 

Đối với mẫu 2,```
4 3 100
100 1 2
-100 1 2
1 2 3
-1 2 2
```Có hai giao dịch âm đều có thể được thực hiện vào ngày thứ 2.`-100`giao dịch được thực hiện từ ngày 1 đến ngày 2, trong khi`-1`chỉ hoạt động vào ngày thứ 2. 

| Ngày | Buộc trước ngày | Tích cực thay đổi tiêu cực | Số dư tối thiểu | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | -100 | 0 | An toàn | 
| 2 | 0 | -101 | -1 | Khoảng trống | 
| 3 | 1 | 0 | 101 | Chưa đạt | 

Vào ngày thứ 2, cả hai khoản thanh toán âm có thể được đặt trước số tiền dương`+100`Và`+1`giao dịch. Bắt đầu từ 100 euro, số dư sẽ trở thành`100 - 100 - 1 = -1`. Thuật toán phát hiện điều này và in`YES`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n + m)`| Mỗi giao dịch được xử lý một lần và mỗi giao dịch`m`ngày được quét một lần. | 
| Không gian |`O(m)`| Hai mảng có kích thước tỷ lệ thuận với số ngày được duy trì. | 

Với`n, m <= 1000`, thuật toán chỉ thực hiện vài nghìn phép tính mảng và sử dụng bộ nhớ không đáng kể so với giới hạn 512 MB. Nó thoải mái trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_text(inp: str) -> str:
    data = inp.split()
    it = iter(data)

    n = int(next(it))
    m = int(next(it))
    s = int(next(it))

    ending = [0] * (m + 2)
    active = [0] * (m + 2)

    for _ in range(n):
        count = int(next(it))
        left = int(next(it))
        right = int(next(it))

        ending[right] += count

        if count < 0:
            active[left] += count
            active[right + 1] -= count

    forced = 0
    dangerous = 0

    for day in range(1, m + 1):
        forced += ending[day - 1]
        dangerous += active[day]

        if s + forced + dangerous < 0:
            return "YES\n"

    return "NO\n"

assert solve_text("""\
4 3 100
100 1 2
-100 1 2
1 2 3
0 3 3
""") == "NO\n", "sample 1"

assert solve_text("""\
4 3 100
100 1 2
-100 1 2
1 2 3
-1 2 2
""") == "YES\n", "sample 2"

assert solve_text("""\
1 1 1
-1 1 1
""") == "NO\n", "minimum input and exact zero"

assert solve_text("""\
3 1 1
1 1 1
1 1 1
1 1 1
""") == "NO\n", "all equal positive values"

assert solve_text("""\
2 2 5
0 1 1
-6 2 2
""") == "YES\n", "transaction starting and ending on boundary day"

assert solve_text("""\
1000 1000 1000000
""" + "\n".join(["0 1 1000"] * 1000) + "\n") == "NO\n", "maximum-size zero transactions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`với`-1 1 1`|`NO`| Kích thước tối thiểu và sự khác biệt giữa số dư bằng 0 và số dư âm | 
| Ba`+1`giao dịch ngày 1 |`NO`| Giá trị hoàn toàn bình đẳng và giao dịch tích cực vô hại | 
|`0 1 1`,`-6 2 2`với`s = 5`|`YES`| Ranh giới khoảng thời gian chính xác và giao dịch chỉ hoạt động trong một ngày | 
| 1000 giao dịch bằng 0 với khoảng thời gian`[1,1000]`|`NO`| Kích thước đầu vào tối đa và giao dịch trung lập | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là tài khoản đạt chính xác bằng 0. Vì```
1 1 1
-1 1 1
```quá trình quét bắt đầu với`forced = 0`Và`dangerous = -1`. Số dư ứng viên là`1 - 1 = 0`, vậy điều kiện`balance < 0`là sai và thuật toán in ra`NO`. Điều này ngăn ngừa sai lầm phổ biến khi coi tài khoản đã cạn kiệt là tài khoản đang thiếu tiền mặt. 

Trường hợp cạnh thứ hai liên quan đến khoảng thời gian kết thúc vào ngày hiện tại. Coi như```
2 2 5
0 1 1
-6 2 2
```Vào ngày thứ 1,`-6`giao dịch không hoạt động nên số dư tối thiểu là 5. Vào ngày thứ 2, nó sẽ hoạt động thông qua`active[2]`, trong khi nó không được bao gồm trong`forced`, bởi vì nó`to`giá trị chính xác là 2 chứ không phải nhỏ hơn 2. Số dư của ứng viên là`5 - 6 = -1`, sản xuất`YES`. Điều này chứng tỏ tại sao tổng bắt buộc phải sử dụng`to < day`, không`to <= day`. 

Trường hợp thứ ba là nhiều giao dịch âm trong cùng một ngày. Trong Mẫu 2, ngày thứ 2 có đóng góp âm tích cực`-100`Và`-1`, Vì thế`dangerous = -101`. Thuật toán đặt cả hai khoản thanh toán trước tất cả các giao dịch tích cực vào ngày đó một cách hiệu quả, mang lại số dư nhỏ nhất có thể. Thứ tự đầu vào cố định sẽ không nắm bắt được khả năng này. 

Trường hợp cạnh thứ tư là một giao dịch không có thay đổi. Đối với một giao dịch như`0 1 1`, mã không thêm gì vào`active`, bởi vì số 0 không nguy hiểm cũng như không có ích cho việc hạ thấp số dư. Sự hiện diện của nó không thể thay đổi câu trả lời và thử nghiệm kích thước tối đa xác nhận rằng nhiều giao dịch như vậy được xử lý mà không có trường hợp đặc biệt nào.
