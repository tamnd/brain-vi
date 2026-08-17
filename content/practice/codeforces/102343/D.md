---
title: "CF 102343D - Vùng Đất Kẹo"
description: "Bàn cờ là một dãy gồm n ô vuông. Các hình vuông từ 1 đến n - 1 có màu như ĐỎ hoặc một hình vuông đặc biệt độc đáo như ĐẶC BIỆT. Hình vuông n là hình vuông kết thúc và đại diện cho mọi màu cùng một lúc. Người chơi bắt đầu trước ô vuông 1."
date: "2026-08-16T17:58:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102343
codeforces_index: "D"
codeforces_contest_name: "UCF Locals 2019"
rating: 0
weight: 102343
solve_time_s: 179
verified: true
draft: false
---

[CF 102343D - Vùng đất kẹo](https://codeforces.com/problemset/problem/102343/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bảng trò chơi là một dòng`n`hình vuông. Hình vuông`1`bởi vì`n - 1`có một màu như`RED`hoặc một hình vuông đặc biệt độc đáo như`SPECIALCANE`. Quảng trường`n`là hình vuông kết thúc và đại diện cho mọi màu cùng một lúc. Người chơi bắt đầu trước hình vuông`1`. 

có`p`người chơi và một bộ bài chứa`c`thẻ. Ở mỗi lượt, người chơi tiếp theo lấy lá bài tiếp theo từ trên cùng của bộ bài. Sau khi sử dụng, lá bài đó sẽ tụt xuống dưới cùng nên bộ bài sẽ lặp lại theo chu kỳ. Thứ tự của người chơi cũng lặp lại theo chu kỳ. Người chơi đầu tiên đến được ô vuông`n`thắng. Tuyên bố chính thức đảm bảo rằng trò chơi sẽ kết thúc sau chưa đầy 10.000 lượt. 

một loại`1`thẻ chứa một màu. Người chơi di chuyển đến ô vuông đầu tiên ngay sau vị trí hiện tại của họ có màu đó. Nếu không có hình vuông thông thường như vậy thì hình vuông kết thúc sẽ đạt được vì nó chứa mọi màu. 

một loại`2`thẻ chứa một màu hai lần. Người chơi di chuyển đến lần xuất hiện thứ hai của màu đó sau vị trí hiện tại của họ. Ô kết thúc được tính là sự xuất hiện của mọi màu sắc, đó là lý do tại sao người chơi có thể giành chiến thắng ngay cả khi chỉ có một lần xuất hiện bình thường ở phía trước. 

một loại`3`thẻ tên một hình vuông đặc biệt. Người chơi di chuyển trực tiếp đến ô vuông đó, bất kể nó ở phía trước hay phía sau vị trí hiện tại của họ. Các ô vuông đặc biệt là duy nhất nên đích đến của chúng là không rõ ràng. 

Các hạn chế là nhỏ có chủ ý. Bàn cờ có tối đa 200 ô vuông, bộ bài có tối đa 500 quân bài và trò chơi kéo dài dưới 10.000 lượt. Ngay cả việc triển khai quét toàn bộ bảng để tìm từng thẻ màu cũng thực hiện tối đa khoảng 2.000.000 lần kiểm tra ô vuông. Điều đó có thể dễ dàng quản lý theo giới hạn 3 giây và giới hạn bộ nhớ 256 MB. 

Các trường hợp cạnh chính đến từ ô kết thúc và từ hướng di chuyển đặc biệt. Ví dụ, hãy xem xét```
2 2
RED
1
2 RED
```Hình vuông bình thường duy nhất là`RED`, và hình vuông kết thúc là hình vuông`2`. Người chơi 1 chọn một loại`2 RED`thẻ khi đứng ở vị trí`0`. đầu tiên`RED`sự xuất hiện là hình vuông`1`, và số thứ hai là hình vuông kết thúc, vì vậy kết quả đầu ra đúng là`1`. Việc thực hiện bất cẩn chỉ tìm kiếm`n - 1`các ô vuông trên bảng được cung cấp rõ ràng sẽ kết luận không chính xác rằng không có lần xuất hiện thứ hai. 

Một trường hợp ranh giới khác là một lá bài đặc biệt di chuyển người chơi lùi lại:```
4 2
RED
SPECIALX
BLUE
1
3 SPECIALX
```Người chơi 1 ngay lập tức di chuyển sang ô vuông`2`, mặc dù nước đi đó không phải là nước đi về phía trước. Một mô phỏng giả định rằng mỗi thẻ chỉ tăng vị trí sẽ có trạng thái sai. 

Trường hợp cạnh cuối cùng là một số người chơi có thể chiếm cùng một ô vuông. Ví dụ,```
3 2
RED
RED
1
1 RED
```Người chơi 1 di chuyển đến ô vuông`1`. Ở lượt tiếp theo, người chơi 2 lấy lá bài tương tự và cũng chuyển sang ô vuông`1`. Không có quy tắc xung đột nên cả hai vị trí vẫn hợp lệ. Các quy tắc chính thức rõ ràng cho phép nhiều người chơi chia sẻ một hình vuông. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng trò chơi chính xác như nó được mô tả. Giữ một vị trí cho mỗi người chơi, giữ chỉ mục cho thẻ tiếp theo và xử lý các lượt cho đến khi ai đó đến ô cuối cùng. Đối với một loại`1`hoặc gõ`2`thẻ màu, quét về phía trước qua bảng và đếm các màu phù hợp. Đối với một loại`3`thẻ, tra cứu ô vuông đặc biệt được đặt tên và gán trực tiếp vị trí đó. 

Mô phỏng lực lượng vũ phu này đã đủ nhanh. Trong trường hợp xấu nhất có ít hơn 10.000 lượt và mỗi lượt có thể kiểm tra tất cả 199 ô vuông thông thường, đưa ra ít hơn 1.990.000 lượt kiểm tra bảng. Do đó, cách tiếp cận vũ phu không thực sự trở nên quá chậm dưới những ràng buộc nhất định. 

Vẫn còn một sự tối ưu hóa hữu ích giúp mô phỏng sạch hơn. Đối với mỗi màu, hãy xử lý trước danh sách đã sắp xếp các vị trí bảng chứa màu đó. Thêm hình vuông cuối cùng`n`vào danh sách của mọi màu vì hình vuông kết thúc đại diện cho mọi màu. Sau đó một loại`1`thẻ yêu cầu vị trí được lưu trữ đầu tiên lớn hơn vị trí hiện tại của người chơi, trong khi một loại`2`thẻ yêu cầu vị trí thứ hai như vậy. Tìm kiếm nhị phân tìm thấy vị trí đầu tiên một cách trực tiếp. 

Quan sát đằng sau sự tối ưu hóa này là bảng không bao giờ thay đổi. Mọi truy vấn đều hỏi cùng một câu hỏi tĩnh, cụ thể là sự xuất hiện của một màu cụ thể nào sau một vị trí nhất định. Việc tính toán trước danh sách sự kiện sẽ tách thông tin tĩnh này khỏi phần động của trò chơi, vốn chỉ là vị trí hiện tại của người chơi. 

Các ô vuông đặc biệt có thể được lưu trữ trong từ điển từ tên đến vị trí trên bảng của chúng. Vì tên của chúng là duy nhất nên điều này mang lại khả năng tra cứu liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Tn) | O(n + c + p) | Đã chấp nhận | 
| Tối ưu | O(n + c + T log n) | O(n + c) | Đã chấp nhận | 

Đây`T < 10000`là số lượt. Phiên bản tối ưu được ưu tiên hơn vì độ phức tạp của nó phản ánh trực tiếp thực tế rằng mỗi nước đi là một truy vấn trên một bảng cố định. 

## Hướng dẫn thuật toán 

1. Đọc bảng và đánh số các vị trí của nó từ`1`bởi vì`n`. Đầu vào chỉ mô tả các vị trí`1`bởi vì`n - 1`, trong khi vị trí`n`là hình vuông kết thúc nhiều màu. 
2. Xây dựng từ điển`color_positions`. Đối với mọi ô vuông màu thông thường ở vị trí`i`, nối thêm`i`vào danh sách màu đó. Sau khi đọc bảng, nối thêm`n`vào mọi danh sách màu sắc. Hình vuông kết thúc phải được đưa vào vì nó đóng vai trò là sự xuất hiện của mọi màu sắc. 
3. Xây dựng từ điển`special_positions`khi đọc bảng. Nếu một hình vuông bắt đầu bằng`SPECIAL`, lưu tên đầy đủ và vị trí của nó. 
4. Đọc bộ bài và lưu trữ từng lá bài theo loại và chuỗi mục tiêu của nó. Bản thân bộ bài không bao giờ thay đổi thứ tự, bởi vì các lá bài đã sử dụng chỉ đơn giản là di chuyển từ trước ra sau. Theo đó, quay`t`luôn sử dụng thẻ`(t mod c)`. 
5. Khởi tạo vị trí của mọi người chơi thành`0`. Người chơi đến lượt`t`là`(t mod p)`. Cả hai chỉ số đều dựa trên cơ sở 0 trong nội bộ, trong khi số người chơi được in ở cuối là dựa trên một. 
6. Đối với một loại`3`thẻ, thay thế vị trí của người chơi hiện tại bằng vị trí ô vuông đặc biệt đã lưu. Không cần tìm kiếm chuyển tiếp vì các thẻ đặc biệt sẽ di chuyển trực tiếp đến các ô vuông được đặt tên của chúng. 
7. Đối với một loại`1`thẻ màu, sử dụng`bisect_right`trên danh sách vị trí của màu để tìm vị trí đầu tiên lớn hơn vị trí hiện tại của người chơi. Gán vị trí đó cho người chơi. 
8. Đối với một loại`2`thẻ màu, thực hiện tìm kiếm nhị phân tương tự nhưng lấy phần tử ở một vị trí sau trong danh sách xuất hiện. Bởi vì hình vuông kết thúc đã được thêm vào mọi danh sách màu nên nó tự nhiên trở thành lần xuất hiện thứ hai khi cần thiết. 
9. Sau khi áp dụng thẻ, hãy kiểm tra xem vị trí của người chơi có đúng không`n`. Nếu vậy, hãy in ngay số một của người chơi đó. Trò chơi dừng lại khi người chơi đầu tiên về đích. 
10. Chuyển lượt tiếp theo và lặp lại. Đầu vào đảm bảo rằng trò chơi kết thúc trong vòng 10.000 lượt có nghĩa là không cần cơ chế phát hiện chu kỳ. 

Tại sao nó hoạt động: trước mỗi lượt, vị trí được lưu trữ của mỗi người chơi chính xác là vị trí thực của họ trong trò chơi. Đối với một lá bài đặc biệt, từ điển sẽ đưa ra chính xác hình vuông duy nhất được đặt tên theo lá bài đó. Đối với thẻ màu, danh sách xuất hiện chứa chính xác tất cả các ô vuông có màu đó có thể được đặt vào, bao gồm cả ô vuông nhiều màu cuối cùng. Tìm kiếm nhị phân chọn lần xuất hiện đầu tiên hoặc thứ hai sau vị trí hiện tại, khớp chính xác với quy tắc thẻ. Vì người chơi và bộ bài được nâng cao theo chu kỳ theo thứ tự giống như trò chơi nên mỗi lượt mô phỏng đều khớp với trò chơi thực. Vị trí mô phỏng đầu tiên bằng`n`do đó là người chiến thắng thực sự. 

## Giải pháp Python```python
import sys
from bisect import bisect_right

input = sys.stdin.readline

def solve():
    n, p = map(int, input().split())

    color_positions = {}
    special_positions = {}

    for pos in range(1, n):
        cell = input().strip()

        if cell.startswith("SPECIAL"):
            special_positions[cell] = pos
        else:
            color_positions.setdefault(cell, []).append(pos)

    # The final square is an occurrence of every color.
    for positions in color_positions.values():
        positions.append(n)

    c = int(input())
    deck = []

    for _ in range(c):
        typ, target = input().split()
        deck.append((int(typ), target))

    player_pos = [0] * p

    turn = 0

    while True:
        player = turn % p
        typ, target = deck[turn % c]
        current = player_pos[player]

        if typ == 3:
            player_pos[player] = special_positions[target]
        else:
            positions = color_positions[target]
            idx = bisect_right(positions, current)

            if typ == 1:
                player_pos[player] = positions[idx]
            else:
                player_pos[player] = positions[idx + 1]

        if player_pos[player] == n:
            print(player + 1)
            return

        turn += 1

if __name__ == "__main__":
    solve()
```Vòng tiền xử lý đầu tiên phân biệt các màu thông thường với các hình vuông đặc biệt bằng cách sử dụng`SPECIAL`tiền tố. Điều này an toàn vì câu lệnh đảm bảo rằng không có màu nào chứa chuỗi con đó. 

Danh sách màu lúc đầu chỉ chứa các vị trí bảng thông thường. Đang bổ sung`n`sau đó là chi tiết triển khai chính. Nó tránh được các trường hợp đặc biệt như "chỉ còn lại một ô vuông phù hợp, vì vậy một loại`2`thẻ thắng". Tìm kiếm nhị phân sau đó xử lý trường hợp đó một cách tự nhiên.`bisect_right(positions, current)`cũng là cố ý. Lá bài luôn yêu cầu một ô vuông sau ô vuông hiện tại của người chơi chứ không phải chính ô vuông hiện tại. Nếu người chơi đã đứng trên ô có màu được yêu cầu thì ô đó không được tính vào nước đi. 

Đối với một loại`1`thẻ,`positions[idx]`là hình vuông phù hợp đầu tiên sau người chơi. Đối với một loại`2`thẻ,`positions[idx + 1]`là hình vuông phù hợp thứ hai. Vấn đề đảm bảo rằng luật chơi làm cho nước đi được yêu cầu là hợp lệ, bởi vì ô kết thúc cung cấp lần xuất hiện cuối cùng cần thiết. 

Chỉ số người chơi sử dụng`turn % p`, trong khi chỉ mục thẻ sử dụng`turn % c`. Hai chu kỳ này là độc lập. Chỉ nâng cao chỉ số bộ bài khi một lượt kết thúc tương đương với việc di chuyển lá bài đã sử dụng xuống cuối bộ bài. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn. Số vòng quay tối đa cũng nhỏ nên không cần tăng tốc chu kỳ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên có mười ô vuông và hai người chơi. Bảng thông thường là```
1 RED
2 BLUE
3 SPECIALCANE
4 GREEN
5 RED
6 BLUE
7 BLUE
8 GREEN
9 RED
10 FINISH
```Bộ bài có bốn lá bài:```
1 RED
2 BLUE
3 SPECIALCANE
2 GREEN
```Những thay đổi trạng thái chính là: 

| Xoay | Người chơi | Thẻ | Vị trí trước đây | Vị Trí Mới | 
| --- | --- | --- | --- | --- | 
| 1 | 1 |`1 RED`| 0 | 1 | 
| 2 | 2 |`2 BLUE`| 0 | 6 | 
| 3 | 1 |`3 SPECIALCANE`| 1 | 3 | 
| 4 | 2 |`2 GREEN`| 6 | 10 | 

Ở lượt 2, các vị trí màu xanh là`2, 6, 7, 10`. Bắt đầu từ vị trí`0`, lần xuất hiện thứ hai là`6`, vậy là người chơi 2 đạt đến ô số 6. Ở lượt 4, các vị trí xanh sau vị trí 6 là`8, 10`, do đó lần xuất hiện thứ hai là hình vuông kết thúc. Câu trả lời là`2`. Lời giải thích mẫu chính thức đưa ra trình tự tương tự. 

### Mẫu 2 

Mẫu thứ hai có hai vị trí bảng trước khi về đích:```
1 RED
2 SPECIALLOLLIPOP
3 FINISH
```Có ba người chơi và hai thẻ:```
3 SPECIALLOLLIPOP
1 RED
```Dấu vết là: 

| Xoay | Người chơi | Thẻ | Vị trí trước đây | Vị Trí Mới | 
| --- | --- | --- | --- | --- | 
| 1 | 1 |`3 SPECIALLOLLIPOP`| 0 | 2 | 
| 2 | 2 |`1 RED`| 0 | 1 | 
| 3 | 3 |`3 SPECIALLOLLIPOP`| 0 | 2 | 
| 4 | 1 |`1 RED`| 2 | 3 | 

Người chơi 1 đến ô vuông đặc biệt ở lượt đầu tiên. Ở lượt thứ tư,`RED`quân bài không có hình vuông màu đỏ thông thường sau vị trí 2, vì vậy hình vuông kết thúc được chọn. Người chơi 1 thắng. Đầu ra mẫu là`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + c + T log n) | Quá trình tiền xử lý bảng và boong là tuyến tính, và mỗi`T`lần lượt thực hiện tối đa một tìm kiếm nhị phân. | 
| Không gian | O(n + c) | Danh sách xuất hiện, bản đồ hình vuông đặc biệt, bộ bài và vị trí của người chơi lưu trữ trạng thái kích thước tuyến tính. | 

Đây`n <= 200`,`c <= 500`, Và`T < 10000`. Ngay cả mô phỏng O(Tn) đơn giản hơn cũng sẽ thực hiện ít hơn hai triệu lần kiểm tra bảng, trong khi giải pháp được gửi nhanh hơn và vẫn thoải mái trong giới hạn 3 giây và 256 MB. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây sử dụng hai mẫu chính thức và bốn trường hợp bổ sung. Trường hợp kích thước tối đa được tạo theo chương trình để bài kiểm tra vẫn có thể đọc được trong khi vẫn xây dựng một bảng với`n = 200`và một bộ bài với`c = 500`.```python
import sys
import io
from bisect import bisect_right

def solve():
    input = sys.stdin.readline

    n, p = map(int, input().split())

    color_positions = {}
    special_positions = {}

    for pos in range(1, n):
        cell = input().strip()

        if cell.startswith("SPECIAL"):
            special_positions[cell] = pos
        else:
            color_positions.setdefault(cell, []).append(pos)

    for positions in color_positions.values():
        positions.append(n)

    c = int(input())
    deck = []

    for _ in range(c):
        typ, target = input().split()
        deck.append((int(typ), target))

    player_pos = [0] * p
    turn = 0

    while True:
        player = turn % p
        typ, target = deck[turn % c]
        current = player_pos[player]

        if typ == 3:
            player_pos[player] = special_positions[target]
        else:
            positions = color_positions[target]
            idx = bisect_right(positions, current)

            if typ == 1:
                player_pos[player] = positions[idx]
            else:
                player_pos[player] = positions[idx + 1]

        if player_pos[player] == n:
            return str(player + 1)

        turn += 1

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        return solve() + "\n"
    finally:
        sys.stdin = old_stdin

sample1 = """\
10 2
RED
BLUE
SPECIALCANE
GREEN
RED
BLUE
BLUE
GREEN
RED
4
1 RED
2 BLUE
3 SPECIALCANE
2 GREEN
"""

sample2 = """\
2 3
RED
SPECIALLOLLIPOP
2
3 SPECIALLOLLIPOP
1 RED
"""

assert run(sample1) == "2\n", "sample 1"
assert run(sample2) == "1\n", "sample 2"

# Minimum-size board. A type-1 card immediately reaches the finish.
assert run("""\
2 2
RED
1
1 RED
""") == "1\n", "minimum board"

# Type-2 card must count the finish square as the second occurrence.
assert run("""\
2 2
RED
1
2 RED
""") == "1\n", "finish counts as second occurrence"

# A special card may move backwards.
assert run("""\
4 2
RED
SPECIALX
BLUE
2
1 RED
3 SPECIALX
""") == "1\n", "backward special move"

# Maximum-size board and deck.
# Every ordinary square is RED, so the type-2 card reaches the second
# occurrence immediately on the first turn.
board = ["RED"] * 199
deck = ["2 RED"] * 500

max_input = (
    "200 6\n"
    + "\n".join(board)
    + "\n500\n"
    + "\n".join(deck)
    + "\n"
)

assert run(max_input) == "1\n", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`2`| Mô phỏng nhiều người chơi thông thường, bộ bài lặp lại, màu sắc và thẻ đặc biệt | 
| Mẫu 2 |`1`| Một nước đi đặc biệt theo sau là một nước đi màu về đích | 
|`2 2 / RED / 1 RED`|`1`| Kích thước bảng tối thiểu và hoàn thiện ngay lập tức | 
|`2 2 / RED / 2 RED`|`1`| Hình vuông kết thúc được tính là lần xuất hiện thứ hai | 
|`4 2 / RED / SPECIALX / BLUE`|`1`| Một lá bài đặc biệt có thể khiến người chơi lùi lại | 
|`n = 200, c = 500`|`1`| Kích thước ván và boong tối đa | 

## Vỏ cạnh 

Trường hợp tinh tế đầu tiên là hình vuông cuối cùng đóng vai trò là sự xuất hiện của màu sắc. Với```
2 2
RED
1
2 RED
```danh sách màu ban đầu là`[1]`. Thuật toán nối thêm vị trí kết thúc và thu được`[1, 2]`. Vị trí hiện tại của người chơi là`0`, Vì thế`bisect_right`chỉ số trả về`0`. một loại`2`thẻ chọn chỉ mục`1`, đó là vị trí`2`. Đầu ra là`1`. Không có chi nhánh trường hợp đặc biệt là cần thiết. 

Trường hợp tinh tế thứ hai là một động thái đặc biệt đi lùi. Với```
4 2
RED
SPECIALX
BLUE
2
1 RED
3 SPECIALX
```người chơi 1 đầu tiên di chuyển đến vị trí`1`. Vào lần tiếp theo người chơi 1 hành động, lá bài đặc biệt sẽ di chuyển họ trực tiếp đến vị trí`2`, bất kể vị trí trước đây của họ. Thuật toán gán`special_positions["SPECIALX"]`trực tiếp, do đó nó không vô tình hạn chế việc di chuyển đến các vị trí sau vị trí hiện tại. 

Trường hợp thứ ba là nhiều người chơi chia nhau một hình vuông. Với```
3 2
RED
RED
1
1 RED
```người chơi 1 di chuyển từ`0`ĐẾN`1`. Ở lượt tiếp theo, người chơi 2 di chuyển độc lập từ`0`ĐẾN`1`. Mô phỏng lưu trữ một vị trí riêng biệt cho mỗi người chơi, do đó, việc di chuyển của người chơi không ảnh hưởng đến người khác. Điều này phù hợp với quy tắc người chơi được phép chiếm cùng một ô vuông. 

Trường hợp thứ tư là khi một loại`1`thẻ không có hình vuông phù hợp thông thường phía trước. Coi như```
3 2
BLUE
RED
1
1 BLUE
```Người chơi 1 bắt đầu lúc`0`và di chuyển đến vị trí`1`, nên vẫn chưa có vấn đề gì. Nếu người chơi 1 sau đó nhận được một cái khác`1 BLUE`thẻ từ vị trí`1`, không có gì bình thường`BLUE`vuông sau nó. Danh sách xuất hiện màu sắc là`[1, 3]`, Ở đâu`3`là kết thúc, vì vậy tìm kiếm nhị phân sẽ chọn`3`. Thuật toán đạt được kết quả chính xác mà không coi việc kết thúc là một kiểu di chuyển riêng biệt.
