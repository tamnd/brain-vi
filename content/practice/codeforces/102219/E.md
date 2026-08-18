---
title: "CF 102219E - Khe cắm tối ưu"
description: "Chúng tôi có giới hạn thời gian T cho một ngày cuối tuần và một dãy N thời lượng sự kiện được sắp xếp theo thứ tự. Mỗi sự kiện có thể được chấp nhận một lần hoặc bị từ chối. Các sự kiện được chấp nhận phải có tổng thời lượng tối đa là T và mục tiêu chính là làm cho tổng thời lượng đó càng lớn càng tốt."
date: "2026-08-17T22:51:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102219
codeforces_index: "E"
codeforces_contest_name: "2019 ICPC Malaysia National"
rating: 0
weight: 102219
solve_time_s: 209
verified: false
draft: false
---

[CF 102219E - Khe cắm tối ưu](https://codeforces.com/problemset/problem/102219/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 29s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có giới hạn thời gian`T`trong một ngày cuối tuần và một loạt các thứ tự`N`thời lượng sự kiện. Mỗi sự kiện có thể được chấp nhận một lần hoặc bị từ chối. Các sự kiện được chấp nhận phải có tổng thời lượng tối đa`T`và mục tiêu chính là làm cho tổng số đó càng lớn càng tốt. Tương tự, chúng tôi muốn giảm thiểu thời gian hội trường không sử dụng. 

Khi một số tập hợp con khác nhau có tổng thời lượng tối đa như nhau, thứ tự đặt trước sẽ phá vỡ ràng buộc. Các sự kiện trước đó có mức độ ưu tiên, do đó, ở vị trí đầu tiên có hai lựa chọn hợp lệ khác nhau, chúng tôi ưu tiên lựa chọn bao gồm sự kiện trước đó. 

Đầu vào chứa một số trường hợp thử nghiệm độc lập. Mỗi cái đều bắt đầu bằng`T`Và`N`, theo sau là`N`thời lượng. Một dòng chứa`0`kết thúc việc nhập liệu. Tuyên bố chính thức cho biết có thể có tới 50 lượt đặt trước và đưa ra giới hạn thời gian là 2 giây với bộ nhớ 256 MB. Câu lệnh được kết xuất không cung cấp giới hạn trên bằng số riêng biệt cho`T`, mặc dù`T`rõ ràng là đủ nhỏ để phục vụ như là kích thước năng lực của một chương trình năng động. 

Hệ quả quan trọng là một thuật toán hàm mũ trong`N`là không thích hợp. Với`N = 50`, đã có rồi`2^50`, Về`1.13 * 10^15`, các tập con có thể có. Một chương trình động có chiều thứ hai là`T`là sự lựa chọn tự nhiên vì mục tiêu là tổng số được giới hạn bởi thời gian sẵn có. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai có vẻ đúng đắn không thành công. 

Coi như`5 3 6 7 8`. Mỗi sự kiện dài hơn năm đơn vị có sẵn, vì vậy không thể chọn gì. Đầu ra đúng là`0`. Một quy trình tái thiết bất cẩn có thể cho rằng ít nhất một sự kiện đã được chọn và in ra khoảng thời gian không hợp lệ. 

Coi như`10 4 6 4 5 5`. Cả hai`6 4`Và`5 5`điền vào công suất chính xác. Đầu ra đúng là`6 4 10`, bởi vì sự kiện có thời lượng`6`xuất hiện trước các sự kiện với thời lượng`5`. Việc triển khai chỉ tối đa hóa tổng và giữ bất kỳ tập hợp con nào nó tìm thấy trước tiên có thể trả về lựa chọn phá vỡ ràng buộc sai. 

Coi như`5 3 1 4 5`. Tập hợp con tốt nhất là`1 4`, tổng của nó chính xác là`5`, vì vậy đầu ra là`1 4 5`. trận chung kết`5`là tổng số, không phải một sự kiện được chọn khác. Trình phân tích cú pháp hoặc quy trình xây dựng lại xử lý mọi số được in dưới dạng khoảng thời gian đã chọn có thể hiểu sai định dạng đầu ra được yêu cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi tập hợp con của`N`sự kiện, tính toán thời lượng của nó, từ chối nó nếu thời lượng vượt quá`T`và giữ tập hợp con có tổng hợp lệ lớn nhất. Điều này đúng vì mọi lựa chọn có thể đều tương ứng với chính xác một tập hợp con, do đó việc tìm kiếm toàn diện không thể bỏ sót lựa chọn tối ưu. Nếu tổng được tích lũy tăng dần trong khi tạo các tập hợp con thì công việc sẽ`O(2^N)`tập hợp con. Với`N = 50`, đó là về`1.13 * 10^15`trạng thái tập hợp con, vượt xa những gì chương trình cuộc thi 2 giây có thể xử lý. Nếu mọi tập hợp con cũng quét tất cả`N`các sự kiện để tính tổng của nó, giới hạn sẽ trở thành`O(N * 2^N)`, đại khái`5.6 * 10^16`kiểm tra sự kiện cơ bản trong trường hợp xấu nhất. 

Lực lượng vũ phu hoạt động vì thông tin có ý nghĩa duy nhất về lựa chọn một phần là tổng thời lượng hiện tại của nó. Nhiều tập hợp con khác nhau có thể đạt được cùng một dung lượng còn lại và một khi chúng ta biết kết quả tốt nhất cho dung lượng đó thì việc khám phá nhiều lần tất cả các trạng thái từng phần tương đương đó là không cần thiết. 

Đây chính xác là cấu trúc của chiếc ba lô 0/1. Với mỗi công suất`j`, chúng ta chỉ cần biết tổng thời lượng tốt nhất có thể đạt được từ các sự kiện còn lại. Dành cho sự kiện`i`với thời lượng`a[i]`, chỉ có hai khả năng: bỏ qua nó, đưa ra giá trị hiện có cho dung lượng`j`, hoặc lấy nó, cho`a[i]`cộng với giá trị tốt nhất cho công suất`j - a[i]`. 

Quy tắc ràng buộc phù hợp một cách tự nhiên với sự tái diễn này. Xử lý các sự kiện từ cuối danh sách đặt trước về đầu. Khi sự kiện hiện tại và việc bỏ qua nó tạo ra tổng số tối ưu như nhau, hãy chọn sự kiện hiện tại. Vì sự kiện hiện tại diễn ra sớm hơn mọi sự kiện đã được trạng thái DP đại diện, nên nó sẽ cung cấp chính xác mức độ ưu tiên cần thiết. 

Chúng ta có thể giữ DP số ở một chiều, giảm bộ nhớ, đồng thời lưu trữ một byte cho mỗi sự kiện và khả năng ghi nhớ xem sự kiện đó có được chọn hay không. DP được xử lý ngược thông qua các sự kiện và xử lý ngược thông qua các năng lực, điều này ngăn cản việc sử dụng cùng một sự kiện nhiều lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(2^N)`với số tiền tăng dần,`O(N2^N)`với việc quét lại |`O(N)`| Quá chậm | 
| Tối ưu |`O(NT)`|`O(NT)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc thời gian có sẵn`T`, số sự kiện`N`và mảng thời lượng`a`. 

Mỗi sự kiện được chọn đều tiêu tốn chính xác thời lượng của nó, do đó, các giá trị năng lực phù hợp duy nhất là`0`bởi vì`T`. 
2. Tạo`dp[j]`, ban đầu bằng 0, ở đâu`dp[j]`đại diện cho tổng thời lượng tốt nhất có thể đạt được với năng lực`j`sử dụng các sự kiện được xử lý cho đến nay. 

Chúng tôi xử lý các sự kiện từ phải sang trái. Trước khi xử lý sự kiện`i`,`dp`chỉ mô tả các sự kiện sau`i`, điều này giúp bạn có thể quyết định xem có nên thêm sự kiện hiện tại mà không vô tình sử dụng nó hai lần hay không. 
3. Tạo`take[i]`dưới dạng một mảng byte được lập chỉ mục theo dung lượng.`take[i][j] = 1`có nghĩa là, sau khi xem xét sự kiện`i`, giải pháp tối ưu về công suất`j`chọn sự kiện`i`. 

Chúng ta cần thông tin này vì chỉ biết tổng số tối ưu là không đủ để tái tạo lại những sự kiện nào đã tạo ra nó. 
4. Quy trình`i`từ`N - 1`xuống tới`0`. Trong khoảng thời gian hiện tại`w = a[i]`, năng lực xử lý`j`từ`T`xuống tới`w`. 

Ứng cử viên có được bằng cách tham gia sự kiện hiện tại là`dp[j - w] + w`. Sự thay thế là hiện tại`dp[j]`, tương ứng với việc bỏ qua sự kiện. 
5. Nếu ứng viên ít nhất giỏi bằng`dp[j]`, thay thế`dp[j]`với thí sinh và cho điểm`take[i][j] = 1`. 

Việc sử dụng`>=`còn hơn là`>`là chìa khóa của quy tắc hòa. Khi cả hai lựa chọn đều tạo ra tổng số như nhau, sự kiện hiện tại trong mảng ban đầu sẽ sớm hơn mọi sự kiện đã được biểu thị trong mảng`dp`, vì vậy việc chọn nó được ưu tiên. 
6. Lặp lại qua mảng sự kiện ban đầu trong quá trình xây dựng lại, bắt đầu bằng`remaining = T`. 

Nếu như`take[i][remaining]`được thiết lập, đầu ra`a[i]`và trừ nó khỏi`remaining`. Nếu không, hãy bỏ qua sự kiện. Việc xây dựng lại tuân theo chính xác các quyết định được ghi lại trong khi DP được xây dựng. 
7. Cuối cùng, xuất ra`dp[T]`dưới dạng tổng thời lượng đã chọn. 

Khoảng thời gian đã chọn đã có sẵn theo thứ tự ban đầu trong quá trình xây dựng lại, do đó không cần sắp xếp hoặc đảo ngược. 

### Tại sao nó hoạt động 

Bất biến là trước khi xử lý sự kiện`i`,`dp[j]`là tổng thời lượng tối đa có thể đạt được từ các sự kiện`i + 1`bởi vì`N - 1`sử dụng nhiều nhất`j`thời gian. Dành cho sự kiện`i`, mọi giải pháp tối ưu đều loại trừ nó, được biểu thị bằng phương án cũ`dp[j]`, hoặc bao gồm nó, được đại diện bởi`a[i] + dp[j-a[i]]`. Lấy giá trị lớn hơn bảo toàn bất biến. Khi hai giá trị hòa nhau, việc chọn sự kiện`i`là đúng vì đây là sự kiện chưa được giải quyết sớm nhất, vì vậy mọi giải pháp chứa nó đều có mức độ ưu tiên cao hơn giải pháp tương đương loại trừ nó. Vòng công suất giảm dần đảm bảo rằng`dp[j-a[i]]`vẫn chỉ mô tả các sự kiện sau nên không có sự kiện nào được sử dụng hai lần. Việc tái thiết tuân theo các quyết định tối ưu đã được ghi lại, đưa ra cả tập hợp con có độ ưu tiên sớm nhất và tổng tối đa được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    out = []

    while True:
        line = input().split()
        if not line:
            break

        T = int(line[0])
        if T == 0:
            break

        N = int(line[1])
        a = list(map(int, line[2:]))

        # If input lines are ever wrapped, keep reading until all N durations exist.
        while len(a) < N:
            a.extend(map(int, input().split()))

        # dp[j] = maximum total duration achievable with capacity j
        # using the events processed so far from right to left.
        dp = [0] * (T + 1)

        # take[i][j] tells us whether event i is selected in the
        # optimal solution for capacity j after considering event i.
        take = [bytearray(T + 1) for _ in range(N)]

        for i in range(N - 1, -1, -1):
            w = a[i]

            for j in range(T, w - 1, -1):
                candidate = dp[j - w] + w

                # On equality, prefer the current event because it
                # appears earlier than all events represented by dp.
                if candidate >= dp[j]:
                    dp[j] = candidate
                    take[i][j] = 1

        remaining = T
        selected = []

        for i in range(N):
            if take[i][remaining]:
                selected.append(str(a[i]))
                remaining -= a[i]

        selected.append(str(dp[T]))
        out.append(" ".join(selected))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào đọc mỗi lần một trường hợp kiểm thử và dừng ngay lập tức khi giá trị đầu tiên bằng 0. Thông thường, các khoảng thời gian đều xuất hiện trên cùng một dòng, nhưng vòng lặp nhỏ theo sau cũng giúp trình phân tích cú pháp an toàn nếu trường hợp kiểm thử được bọc vật lý trên các dòng đầu vào. 

các`dp`mảng chỉ có`T + 1`tiểu bang. Việc xử lý các sự kiện từ phải sang trái mang lại cho mỗi trạng thái ý nghĩa chỉ sử dụng các sự kiện sau đó, trong khi khả năng xử lý từ`T`hướng xuống ngăn không cho đọc sự kiện hiện tại từ trạng thái đã được cập nhật với cùng sự kiện đó. 

Việc so sánh sử dụng`candidate >= dp[j]`. Một sự nghiêm khắc`>`vẫn sẽ tìm được số tiền tối đa chính xác, nhưng nó không nhất thiết phải đáp ứng mức độ ưu tiên của đơn hàng đặt trước được yêu cầu. Bình đẳng phải ủng hộ sự kiện hiện tại. 

các`take`sử dụng mảng`bytearray`, lưu trữ một byte cho mỗi trạng thái thay vì một đối tượng số nguyên Python đầy đủ. Điều này giữ cho thông tin tái thiết được nhỏ gọn trong khi vẫn cho phép lập chỉ mục trực tiếp. 

Trong quá trình tái thiết,`remaining`là năng lực phải được giải thích bằng các sự kiện chưa được xem xét. Nếu sự kiện`i`được ghi lại là đã chọn cho dung lượng đó, trừ đi`a[i]`di chuyển đến chính xác trạng thái đại diện cho vấn đề hậu tố còn lại. Vì quá trình xây dựng lại quét từ trái sang phải nên đầu ra đã có thứ tự đặt trước. 

Số nguyên Python không gặp phải vấn đề tràn chiều rộng cố định của C hoặc C++ và tối đa tất cả các giá trị DP đều`T`vì không có tổng số được chọn có thể vượt quá khả năng. 

## Ví dụ đã hoạt động 

### Trường hợp mẫu 1 

cho`T = 5`và thời lượng`[1, 2, 3, 4, 5]`, DP xử lý mảng từ phải sang trái. Bảng hiển thị trạng thái cho toàn bộ công suất`5`. Các quyết định ở mức dung lượng nhỏ hơn cũng được lưu trữ vì việc tái thiết có thể làm giảm dung lượng sau khi chọn một sự kiện trước đó. 

| Chỉ số sự kiện | Thời lượng |`dp[5]`sau khi xử lý |`take[i][5]`| 
| --- | --- | --- | --- | 
| 4 | 5 | 5 | vâng | 
| 3 | 4 | 5 | không | 
| 2 | 3 | 5 | không | 
| 1 | 2 | 5 | vâng | 
| 0 | 1 | 5 | vâng | 

Quá trình tái thiết cuối cùng bắt đầu với dung lượng`5`. Sự kiện`1`được chọn, giảm dung lượng còn lại xuống`4`. Sự kiện`2`không thể cải thiện giải pháp đã ghi về dung lượng`4`, sự kiện`3`được chọn, làm giảm khả năng`0`, và tất cả các quyết định sau đó đều bị bỏ qua. Khoảng thời gian được chọn là`1`Và`4`, cho đầu ra`1 4 5`. 

Sự thật sự kiện đó`5`đã được lựa chọn tạm thời khi xem xét năng lực`5`không có nghĩa là nó phải xuất hiện trong câu trả lời cuối cùng. Khi một sự kiện trước đó được chọn ở trạng thái hòa, quá trình tái thiết sẽ chuyển sang trạng thái dung lượng nhỏ hơn liên quan đến lựa chọn đó. Đây chính xác là lý do tại sao việc lưu trữ các quyết định cho mọi`(event, capacity)`nhà nước là cần thiết. 

### Trường hợp mẫu 2 

cho`T = 10`và thời lượng`[9, 11, 9, 3, 5, 8, 4, 9, 3, 2]`, tổng số lớn nhất có thể là`10`. 

| Chỉ số sự kiện | Thời lượng |`dp[10]`sau khi xử lý |`take[i][10]`| 
| --- | --- | --- | --- | 
| 9 | 2 | 2 | không | 
| 8 | 3 | 5 | không | 
| 7 | 9 | 9 | không | 
| 6 | 4 | 9 | không | 
| 5 | 8 | 10 | vâng | 
| 4 | 5 | 10 | vâng | 
| 3 | 3 | 10 | vâng | 
| 2 | 9 | 10 | không | 
| 1 | 11 | 10 | không | 
| 0 | 9 | 10 | không | 

Quá trình tái thiết cuối cùng bắt đầu với công suất`10`. Sự kiện`3`, với thời lượng`3`, được chọn, để lại dung lượng`7`. Sự kiện`4`, với thời lượng`5`, được chọn, để lại dung lượng`2`. Các sự kiện sau đó không thể cải thiện dung lượng còn lại đó ngoại trừ sự kiện`9`, với thời lượng`2`, vì vậy nó cũng được chọn. Kết quả là`3 5 2`, với tổng số`10`. 

Ví dụ này cũng thể hiện quy tắc ràng buộc. bộ`8 2`đạt tới`10`, nhưng bộ`3 5 2`có một sự kiện được chọn trước đó nên nó được ưu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(NT)`| Mỗi trong số`N`sự kiện quét năng lực từ`T`xuống đến thời hạn của nó. | 
| Không gian |`O(NT)`|`dp`công dụng`O(T)`trí nhớ và`take`lưu trữ một byte cho mỗi cặp dung lượng sự kiện. | 

Với tối đa 50 sự kiện, kích thước sự kiện rất nhỏ. Thuật toán tránh hàm mũ`2^N`tìm kiếm hoàn toàn và thực hiện một số lượng giới hạn các chuyển đổi DP đơn giản cho từng dung lượng. Việc biểu diễn bộ nhớ cũng nhỏ gọn vì thông tin tái tạo được lưu trữ dưới dạng byte thay vì số nguyên Python. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution():
    input = sys.stdin.readline
    out = []

    while True:
        line = input().split()
        if not line:
            break

        T = int(line[0])
        if T == 0:
            break

        N = int(line[1])
        a = list(map(int, line[2:]))

        while len(a) < N:
            a.extend(map(int, input().split()))

        dp = [0] * (T + 1)
        take = [bytearray(T + 1) for _ in range(N)]

        for i in range(N - 1, -1, -1):
            w = a[i]
            for j in range(T, w - 1, -1):
                candidate = dp[j - w] + w
                if candidate >= dp[j]:
                    dp[j] = candidate
                    take[i][j] = 1

        remaining = T
        selected = []

        for i in range(N):
            if take[i][remaining]:
                selected.append(str(a[i]))
                remaining -= a[i]

        selected.append(str(dp[T]))
        out.append(" ".join(selected))

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solution()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

sample = """\
5 5 1 2 3 4 5
10 9 11 9 3 5 8 4 9 3 2
16 8 12 6 11 11 13 1 10 7
13 5 10 12 2 13 10
28 14 18 19 26 15 18 24 7 21 14 25 2 12 9 6
0
"""

sample_expected = """\
1 4 5
3 5 2 10
6 10 16
13 13
19 7 2 28
"""

assert run(sample) == sample_expected, "provided sample"

assert run("1 1 1\n0\n") == "1 1\n", "minimum-size input"

assert run("5 3 6 7 8\n0\n") == "0\n", "no event fits"

assert run("10 4 6 4 5 5\n0\n") == "6 4 10\n", "tie-breaking priority"

max_input = "100 50 " + " ".join(["2"] * 50) + "\n0\n"
max_expected = " ".join(["2"] * 50 + ["100"]) + "\n"
assert run(max_input) == max_expected, "maximum event count and exact capacity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`1 1`| Công suất tối thiểu và một sự kiện duy nhất sẽ lấp đầy chính xác nó | 
|`5 3 6 7 8`|`0`| Không có sự kiện nào phù hợp, kể cả việc xây dựng lại vùng chọn trống | 
|`10 4 6 4 5 5`|`6 4 10`| Giải pháp có tổng bằng nhau và mức độ ưu tiên của sự kiện sớm nhất | 
|`100 50`theo sau là năm mươi`2`giá trị | Năm mươi`2`các giá trị theo sau là`100`| Số lượng sự kiện tối đa, thời lượng bằng nhau lặp lại và lấp đầy dung lượng chính xác | 

## Vỏ cạnh 

cho`5 3 6 7 8`, mọi thời lượng đều lớn hơn dung lượng. Vòng lặp DP bên trong không bao giờ chạy cho bất kỳ sự kiện nào vì`w > T`, vì vậy mỗi`dp[j]`vẫn bằng không và mọi`take[i][j]`vẫn chưa được đặt. Do đó, quá trình tái thiết không chọn gì cả và thêm vào`dp[5] = 0`, sản xuất chính xác`0`. 

Vì`10 4 6 4 5 5`, hai sự kiện sớm nhất tạo thành tổng cộng`10`, trong khi hai sự kiện sau đó cũng hình thành`10`. Khi sự kiện`1`với thời lượng`4`được xử lý trong DP ngược, sự bao gồm của nó gắn với giá trị tốt nhất hiện có về dung lượng`10`, Vì thế`take[1][10]`trở thành sự thật. Sau này, sự kiện`0`với thời lượng`6`cũng liên kết giá trị tốt nhất ở khả năng`10`và được ưa chuộng hơn vì nó xuất hiện sớm hơn. Do đó, tái thiết lựa chọn`6`, công suất lá`4`, chọn cái trước đó`4`, và tạo ra`6 4 10`. 

Vì`5 3 1 4 5`, tổng số tối đa chính xác là`5`. DP có thể đạt được tổng số đó bằng cách sử dụng sự kiện kéo dài`5`, nhưng khi thời lượng sớm hơn`1`được xem xét, trạng thái của năng lực`5`cũng có thể đạt được`5`thông qua công suất còn lại`4`, do đó hòa chọn thời lượng`1`. Tái thiết sau đó chuyển sang công suất`4`, trong đó thời lượng`4`được chọn. Đầu ra là`1 4 5`, chứng tỏ rằng con số cuối cùng là tổng số chứ không phải một sự kiện khác. 

Đối với trường hợp kích thước tối đa có thời lượng 50 sự kiện`2`và năng lực`100`, mọi sự kiện đều có thể được chọn, đưa ra tổng số chính xác`100`. DP đạt`100`không vượt quá dung lượng và vì tất cả các sự kiện đều giống hệt nhau nên mức độ ưu tiên bắt buộc sẽ tự nhiên chọn chúng theo thứ tự ban đầu. Việc tái thiết tiêu thụ hai đơn vị công suất còn lại ở mỗi sự kiện cho đến khi công suất còn lại bằng 0, tạo ra tất cả 50 khoảng thời gian tiếp theo là`100`.
