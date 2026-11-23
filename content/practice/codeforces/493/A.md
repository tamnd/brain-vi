---
title: "CF 493A - Vasya và bóng đá"
description: "Chúng ta được cung cấp tên của đội nhà và đội khách, theo sau là danh sách theo thứ tự thời gian các sự kiện thẻ trong một trận đấu bóng đá. Mỗi sự kiện chỉ định phút, cầu thủ thuộc đội nào, số áo của cầu thủ đó và trọng tài rút thẻ vàng hay thẻ đỏ."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 493
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 281 (Div. 2)"
rating: 1300
weight: 493
solve_time_s: 98
verified: true
draft: false
---

[CF 493A - Vasya và Bóng đá](https://codeforces.com/problemset/problem/493/A) 

**Đánh giá:** 1300 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp tên của đội nhà và đội khách, theo sau là danh sách theo thứ tự thời gian các sự kiện thẻ trong một trận đấu bóng đá. 

Mỗi sự kiện chỉ định phút, cầu thủ thuộc đội nào, số áo của cầu thủ đó và trọng tài rút thẻ vàng hay thẻ đỏ. 

Một cầu thủ bị đuổi khỏi sân trong hai tình huống. Đầu tiên là nhận thẻ đỏ trực tiếp. Thứ hai là nhận một thẻ vàng khi họ đã có một thẻ vàng, vì thẻ vàng thứ hai tự động trở thành thẻ đỏ sa thải. 

Đầu ra phải chứa lần sa thải đầu tiên của mọi cầu thủ, theo thứ tự thời gian. Nếu một người chơi đã bị đuổi khỏi sân, mọi thẻ sau đó dành cho người chơi đó sẽ bị bỏ qua. Một người chơi có thể xuất hiện ở đầu ra nhiều nhất một lần. 

Những hạn chế là rất nhỏ. Có tối đa 90 sự kiện, phù hợp với độ dài của một trận bóng đá. Ngay cả một giải pháp bậc hai cũng sẽ hoạt động thoải mái trong giới hạn. Thử thách không phải là hiệu quả mà là mô phỏng chính xác các quy tắc và xử lý mọi trường hợp góc cạnh. 

Một số tình huống có thể dễ dàng gây ra câu trả lời sai. 

Một cầu thủ có thể nhận một thẻ đỏ trực tiếp và sau đó nhận một thẻ khác trong đầu vào. Sự kiện sau đó phải bỏ qua vì chúng ta chỉ quan tâm đến lần sa thải đầu tiên. 

Ví dụ:```
HOME
AWAY
2
10 h 7 r
20 h 7 y
```Đầu ra đúng:```
HOME 7 10
```Việc triển khai bất cẩn có thể in cùng một trình phát hai lần. 

Thẻ vàng thứ hai được tính là truất quyền thi đấu ngay lập tức. 

Ví dụ:```
HOME
AWAY
2
15 a 9 y
30 a 9 y
```Đầu ra đúng:```
AWAY 9 30
```Nếu chỉ tính thẻ vàng mà không chuyển thẻ thứ 2 thành thẻ đỏ thì coi như bị truất quyền thi đấu. 

Các cầu thủ từ các đội khác nhau có thể có cùng số áo. 

Ví dụ:```
HOME
AWAY
2
10 h 10 r
20 a 10 r
```Đầu ra đúng:```
HOME 10 10
AWAY 10 20
```Chỉ theo dõi người chơi theo số lượng sẽ hợp nhất không chính xác hai người chơi khác biệt này. 

## Phương pháp tiếp cận 

Giải pháp đơn giản nhất là mô phỏng trận đấu chính xác như mô tả. 

Đối với mỗi trận đấu, chúng tôi cần biết một cầu thủ đã nhận bao nhiêu thẻ vàng và liệu họ đã bị đuổi khỏi sân hay chưa. Khi có thẻ vàng, chúng tôi tăng số thẻ vàng lên. Nếu số lượng đạt đến hai, chúng tôi ghi nhận việc sa thải. Khi có thẻ đỏ trực tiếp, chúng tôi lập tức ghi hình sa thải. 

Việc triển khai rất đơn giản có thể quét liên tục tất cả các sự kiện trước đó bất cứ khi nào có sự kiện mới xuất hiện để xác định trạng thái hiện tại của người chơi. Với tối đa 90 sự kiện, điều này sẽ yêu cầu khoảng$90^2 = 8100$hoạt động, mà vẫn còn dễ dàng đủ nhanh. 

Cách tiếp cận rõ ràng hơn là lưu trữ trạng thái hiện tại của mỗi người chơi vào từ điển. Vì một cầu thủ được xác định duy nhất bởi cả đội và số áo nên chúng tôi sử dụng cặp`(team, number)`như chìa khóa. 

Quan sát quan trọng là mọi sự kiện chỉ ảnh hưởng đến một người chơi. Chúng ta không bao giờ cần phải tính toán lại thông tin từ các sự kiện trước đó. Bằng cách duy trì số thẻ vàng và trạng thái sa thải hiện tại cho mỗi cầu thủ, mọi sự kiện có thể được xử lý trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tên đội nhà và tên đội khách. 
2. Đọc số sự kiện. 
3. Tạo từ điển lưu trữ số thẻ vàng của mỗi cầu thủ. 
4. Tạo một nhóm gồm những người chơi đã bị đuổi khỏi sân. 
5. Xử lý các sự kiện theo thứ tự chúng xuất hiện. Vì đầu vào đã được sắp xếp theo trình tự thời gian nên việc này sẽ tự động duy trì thứ tự đầu ra được yêu cầu. 
6. Đối với mỗi sự kiện, hãy tạo một mã định danh người chơi duy nhất`(team_side, jersey_number)`. 
7. Nếu người chơi đã ở trong bộ bị loại, hãy bỏ qua sự kiện và tiếp tục. Một cầu thủ chỉ có thể bị đuổi khỏi sân một lần. 
8. Trường hợp thẻ đỏ trực tiếp: 

một. In tên đội, số cầu thủ và số phút hiện tại. 

b. Đánh dấu người chơi là bị loại. 
9. Nếu thẻ là thẻ vàng: 

một. Tăng số thẻ vàng của cầu thủ đó. 

b. Nếu số đếm trở thành hai, hãy in tên đội, số cầu thủ và số phút hiện tại. 

c. Đánh dấu người chơi là bị loại. 
10. Sau khi tất cả các sự kiện được xử lý, tất cả các yêu cầu sa thải đã được báo cáo theo trình tự thời gian. 

### Tại sao nó hoạt động 

Thuật toán duy trì hai thông tin cho mỗi cầu thủ: họ đã nhận bao nhiêu thẻ vàng cho đến nay và liệu họ đã bị đuổi hay chưa. 

Trước khi xử lý một sự kiện, trạng thái được lưu trữ khớp chính xác với tất cả các sự kiện trước đó. Khi có thẻ vàng, việc tăng số lượng vàng sẽ phản ánh chính xác trạng thái trận đấu mới. Cầu thủ bị đuổi chính xác khi nhận được thẻ vàng thứ hai hoặc khi thẻ đỏ trực tiếp được rút ra. Sau khi bị loại, người chơi sẽ được thêm vào nhóm bị loại, khiến tất cả các sự kiện trong tương lai của người chơi đó bị bỏ qua. 

Bởi vì các sự kiện được xử lý theo trình tự thời gian nên mỗi lần loại bỏ đều được xuất ra vào đúng phút khi nó xảy ra lần đầu tiên và thứ tự đầu ra sẽ tự động chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

home = input().strip()
away = input().strip()

n = int(input())

yellow = {}
sent_off = set()

out = []

for _ in range(n):
    t, side, num, card = input().split()
    t = int(t)
    num = int(num)

    player = (side, num)

    if player in sent_off:
        continue

    team_name = home if side == "h" else away

    if card == "r":
        out.append(f"{team_name} {num} {t}")
        sent_off.add(player)
    else:
        yellow[player] = yellow.get(player, 0) + 1

        if yellow[player] == 2:
            out.append(f"{team_name} {num} {t}")
            sent_off.add(player)

sys.stdout.write("\n".join(out))
```Từ điển`yellow`lưu trữ số thẻ vàng hiện tại của mỗi cầu thủ. Chìa khóa là`(side, num)`bởi vì số lượng người chơi chỉ là duy nhất trong một đội. 

bộ`sent_off`ngăn chặn việc sa thải trùng lặp. Đây là chi tiết triển khai quan trọng nhất. Khi một cầu thủ nhận thẻ đỏ, dù trực tiếp hay qua thẻ vàng thứ hai, mọi tình huống sau đó liên quan đến cầu thủ đó đều phải được bỏ qua. 

Dữ liệu đầu vào đã được sắp xếp theo thời gian, vì vậy việc thêm phần loại bỏ vào`out`ngay lập tức tạo ra thứ tự thời gian cần thiết. Không cần phân loại bổ sung. 

Một điểm tinh tế khác là kiểm tra việc loại bỏ sau khi tăng số lượng màu vàng. Một người chơi bị loại chính xác khi số đếm trở thành hai. Việc kiểm tra các giá trị lớn hơn hoặc bằng hai cũng có tác dụng, nhưng việc kiểm tra sự bằng nhau khớp với các quy tắc chính xác hơn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
MC
CSKA
9
28 a 3 y
62 h 25 y
66 h 42 y
70 h 25 y
77 a 4 y
79 a 25 y
82 h 42 r
89 h 16 y
90 a 13 r
```| Phút | Người chơi | Thẻ | Đếm Vàng Sau Sự Kiện | Bị sa thải? | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 28 | một,3 | y | 1 | Không | | 
| 62 | h,25 | y | 1 | Không | | 
| 66 | h,42 | y | 1 | Không | | 
| 70 | h,25 | y | 2 | Có | MC 25 70 | 
| 77 | một,4 | y | 1 | Không | | 
| 79 | a,25 | y | 1 | Không | | 
| 82 | h,42 | r | 1 | Có | MC 42 82 | 
| 89 | h,16 | y | 1 | Không | | 
| 90 | a,13 | r | 0 | Có | CSKA 13 90 | 

Đầu ra:```
MC 25 70
MC 42 82
CSKA 13 90
```Ví dụ này thể hiện cả hai cơ chế sa thải. Cầu thủ 25 bị đuổi khỏi sân do thẻ vàng thứ hai, trong khi cầu thủ 42 và 13 nhận thẻ đỏ trực tiếp. 

### Ví dụ tùy chỉnh 

đầu vào:```
HOME
AWAY
5
10 h 7 r
20 h 7 y
30 a 7 y
40 a 7 y
50 a 7 r
```| Phút | Người chơi | Thẻ | Đếm Vàng Sau Sự Kiện | Bị sa thải? | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 10 | h,7 | r | 0 | Có | TRANG CHỦ 7 10 | 
| 20 | h,7 | y | Bỏ qua | Đã được gửi đi | | 
| 30 | một,7 | y | 1 | Không | | 
| 40 | một,7 | y | 2 | Có | ĐI 7 40 | 
| 50 | một,7 | r | Bỏ qua | Đã được gửi đi | | 

Đầu ra:```
HOME 7 10
AWAY 7 40
```Dấu vết này cho thấy cả hai trường hợp góc quan trọng. Người chơi từ các đội khác nhau có thể có cùng số lượng và các sự kiện sau khi bị loại phải được bỏ qua. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi sự kiện được xử lý một lần với các thao tác từ điển và tập hợp thời gian không đổi | 
| Không gian | O(n) | Tối đa một mục từ điển và một mục nhập tập hợp cho mỗi người chơi xuất hiện trong đầu vào | 

Với tối đa 90 sự kiện, thời gian chạy rất nhỏ. Giải pháp dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    home = input().strip()
    away = input().strip()
    n = int(input())

    yellow = {}
    sent_off = set()
    out = []

    for _ in range(n):
        t, side, num, card = input().split()
        t = int(t)
        num = int(num)

        player = (side, num)

        if player in sent_off:
            continue

        team = home if side == "h" else away

        if card == "r":
            out.append(f"{team} {num} {t}")
            sent_off.add(player)
        else:
            yellow[player] = yellow.get(player, 0) + 1
            if yellow[player] == 2:
                out.append(f"{team} {num} {t}")
                sent_off.add(player)

    return "\n".join(out)

# provided sample
assert run(
"""MC
CSKA
9
28 a 3 y
62 h 25 y
66 h 42 y
70 h 25 y
77 a 4 y
79 a 25 y
82 h 42 r
89 h 16 y
90 a 13 r
"""
) == """MC 25 70
MC 42 82
CSKA 13 90"""

# minimum input, single yellow
assert run(
"""A
B
1
1 h 1 y
"""
) == ""

# second yellow causes dismissal
assert run(
"""A
B
2
10 h 5 y
20 h 5 y
"""
) == "A 5 20"

# same number on different teams
assert run(
"""A
B
2
10 h 10 r
20 a 10 r
"""
) == """A 10 10
B 10 20"""

# events after dismissal ignored
assert run(
"""A
B
3
10 h 7 r
20 h 7 y
30 h 7 r
"""
) == "A 7 10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Thẻ vàng đơn | Đầu ra trống | Không xảy ra sự sa thải | 
| Hai màu vàng | Bị sa thải vì vàng thứ hai | Quy tắc thẻ đỏ tự động | 
| Cùng số áo của cả hai đội | Hai đầu ra riêng biệt | Danh tính người chơi bao gồm đội | 
| Thẻ sau khi ra trường | Chỉ một đầu ra | Những sự kiện sau này phải được bỏ qua | 

## Vỏ cạnh 

### Đỏ trực tiếp theo sau là các sự kiện sau đó 

đầu vào:```
HOME
AWAY
2
10 h 7 r
20 h 7 y
```Phút thứ 10, cầu thủ`(h, 7)`nhận thẻ đỏ trực tiếp và bước vào hiệp đấu bị loại. Vào phút thứ 20, thuật toán nhận thấy người chơi đã bị loại và bỏ qua sự kiện. 

Đầu ra:```
HOME 7 10
```Người chơi xuất hiện đúng một lần. 

### Rút thẻ vàng thứ hai 

đầu vào:```
HOME
AWAY
2
15 a 9 y
30 a 9 y
```Sau sự kiện đầu tiên, số lượng màu vàng trở thành 1. Sau sự kiện thứ hai, số lượng màu vàng trở thành 2, dẫn đến việc loại bỏ và thêm người chơi vào nhóm bị loại bỏ. 

Đầu ra:```
AWAY 9 30
```Việc sa thải xảy ra vào đúng phút. 

### Cùng số áo của các đội khác nhau 

đầu vào:```
HOME
AWAY
2
10 h 10 r
20 a 10 r
```Thuật toán lưu trữ người chơi dưới dạng`(side, number)`. Như vậy`(h, 10)`Và`(a, 10)`là các khóa riêng biệt. 

Sự kiện đầu tiên loại bỏ người chơi chủ nhà. Sự kiện thứ hai đuổi cầu thủ đội khách. 

Đầu ra:```
HOME 10 10
AWAY 10 20
```Không xảy ra va chạm giữa các cầu thủ có cùng số áo nhưng thuộc các đội khác nhau.
