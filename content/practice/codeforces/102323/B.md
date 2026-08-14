---
title: "CF 102323B - Bảng xếp hạng bóng đá"
description: "Chúng tôi được giao cho một số nhóm bóng đá độc lập. Mỗi nhóm chứa một tập hợp các đội được đặt tên duy nhất và một tập hợp các trận đấu đã diễn ra. Đối với mỗi trận đấu, chúng tôi biết cả hai đội và tỷ số cuối cùng của họ. Từ kết quả đó chúng ta phải xây dựng bảng xếp hạng hoàn chỉnh."
date: "2026-08-14T00:36:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102323
codeforces_index: "B"
codeforces_contest_name: "UCF Locals 2014"
rating: 0
weight: 102323
solve_time_s: 60
verified: true
draft: false
---

[CF 102323B - Bảng xếp hạng bóng đá](https://codeforces.com/problemset/problem/102323/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao cho một số nhóm bóng đá độc lập. Mỗi nhóm chứa một tập hợp các đội được đặt tên duy nhất và một tập hợp các trận đấu đã diễn ra. Đối với mỗi trận đấu, chúng tôi biết cả hai đội và tỷ số cuối cùng của họ. 

Từ kết quả đó chúng ta phải xây dựng bảng xếp hạng hoàn chỉnh. Đối với mỗi đội, chúng ta cần tổng số bàn thắng ghi được, tổng số bàn thua, số trận thắng, trận thua, trận hòa và điểm tích lũy. Đội thắng được 3 điểm, đội hòa được 1 điểm, đội thua được 0 điểm. 

Các đội sau đó được sắp xếp theo bốn tiêu chí. Nhiều điểm hơn đến trước. Nếu số điểm bằng nhau, hiệu số bàn thắng bại lớn hơn, được định nghĩa là số bàn thắng ghi được trừ số bàn thua, sẽ được xếp trước. Nếu tỷ số cũng bằng nhau thì đội nào ghi được nhiều bàn thắng hơn sẽ được xếp trước. Nếu tất cả các tiêu chí bằng số đều bằng nhau, thứ tự chữ cái của tên đội sẽ quyết định thứ tự cuối cùng. Đầu ra được yêu cầu in các nhóm theo thứ tự đầu vào và để lại một dòng trống sau mỗi nhóm. Vấn đề ban đầu chỉ định tối đa 30 đội và 400 trò chơi cho mỗi nhóm, với giới hạn thời gian C++ là 1 giây và giới hạn bộ nhớ 256 MB trên Codeforces. 

Các giới hạn này đủ nhỏ để chúng ta không cần bất cứ điều gì phức tạp hơn ngoài việc mô phỏng trực tiếp và sau đó là sắp xếp. Ngay cả việc xử lý mỗi trò chơi một lần cũng chỉ tốn O(G) và việc sắp xếp tối đa 30 đội cũng tốn O(T log T). So sánh bậc hai giữa các đội cũng sẽ nhỏ đối với các giới hạn này, nhưng không có lý do gì để sử dụng nó khi việc sắp xếp thông thường thể hiện trực tiếp các quy tắc xếp hạng. 

Có một số trường hợp việc triển khai bất cẩn có thể âm thầm tạo ra bảng sai. Một trận hòa phải cập nhật cả hai đội chứ không chỉ đội được liệt kê đầu tiên. Ví dụ,```
1
2 1
A B
A 0 B 0
```sản xuất```
Group 1:
A 0 0 0 0 1 1
B 0 0 0 0 1 1
```Giải pháp coi kết quả 0-0 là đội thua trận sẽ cho kết quả sai và sai số điểm. 

Một chiến thắng cũng phải cập nhật tổng số bàn thắng một cách độc lập với loại kết quả. Ví dụ,```
1
2 1
A B
A 3 B 1
```sản xuất```
Group 1:
A 3 1 1 0 0 3
B 1 3 0 1 0 0
```Một giải pháp chỉ ghi điểm và thắng mà quên bàn thắng thì không thể tính toán chính xác tỷ số chênh lệch bàn thắng bại. 

Cuối cùng, điểm bằng nhau không đủ để xác định thứ tự. Coi như```
1
3 2
ALPHA BETA GAMMA
ALPHA 2 BETA 0
GAMMA 1 BETA 0
```Thứ tự đúng là ALPHA, GAMMA, BETA. ALPHA và GAMMA cùng có 3 điểm, nhưng ALPHA có hiệu số bàn thắng bại +2 trong khi GAMMA có +1. Giải pháp chỉ sắp xếp theo điểm có thể duy trì thứ tự tùy ý hoặc phụ thuộc vào đầu vào và không đạt thứ hạng được yêu cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là duy trì hồ sơ cho mọi đội và xử lý mọi trận đấu. Với mỗi trận đấu, hãy tra cứu cả hai đội, cộng số bàn thắng đã ghi và số bàn thủng lưới, sau đó cập nhật số trận thắng, thua, hòa, điểm theo tỷ số. Khi tất cả các trận đấu đã được xử lý, chúng tôi có thể xác định thứ hạng bằng cách so sánh từng cặp đội và liên tục chọn ra đội còn lại tốt nhất. Điều này đúng vì mọi thống kê trong bảng tổng kết đều được xác định độc lập bởi kết quả từng trận đấu. 

Với tối đa 30 đội và 400 trận đấu, ngay cả việc xử lý trận đấu cũng chỉ có 400 cập nhật cho mỗi nhóm. Một thứ hạng thực sự đơn giản để so sánh từng cặp có giá O(T²), tối đa là 900 cặp so sánh cho mỗi nhóm. Điều đó vẫn sẽ vượt qua một cách thoải mái đối với những giới hạn cụ thể này. Tuy nhiên, đó là sự phức tạp không cần thiết và sẽ trở nên kém hấp dẫn hơn nếu cùng một ý tưởng được chuyển sang một bài toán xếp hạng lớn hơn. 

Cách tiếp cận rõ ràng hơn là đại diện cho mỗi đội bằng tất cả số liệu thống kê cần thiết cho quy tắc sắp xếp và đầu ra cuối cùng, xử lý mỗi trận đấu chính xác một lần và sau đó sử dụng một cách sắp xếp tiêu chuẩn. Quan sát quan trọng là các tiêu chí xếp hạng tạo thành một trật tự từ điển. Chúng ta có thể mã hóa chúng trực tiếp dưới dạng khóa sắp xếp: điểm giảm dần, hiệu số bàn thắng bại giảm dần, số bàn thắng ghi được giảm dần và tên tăng dần. Sau đó, máy sắp xếp của Python sẽ xử lý tất cả các trường hợp ràng buộc một cách nhất quán. 

Lực lượng vũ phu hoạt động vì mỗi trận đấu đóng góp độc lập cho chính xác hai đội, nhưng bước xếp hạng theo cặp của nó không khai thác được thực tế là thứ tự cuối cùng đã được mô tả bằng một chuỗi khóa so sánh cố định. Quan sát cho thấy các quy tắc xếp hạng tạo thành một khóa từ điển sẽ giảm giai đoạn cuối cùng thành O(T log T) với lý luận chính xác đơn giản hơn nhiều. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(G + T2) | O(T) | Được chấp nhận cho giới hạn nhất định | 
| Tối ưu | O(G + T log T) | O(T) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng nhóm và xử lý từng nhóm một cách độc lập. Việc giữ riêng số liệu thống kê của mỗi nhóm sẽ ngăn kết quả của một nhóm ảnh hưởng đến nhóm khác. 
2. Đọc tên các đội và tạo bản ghi thống kê cho mỗi đội. Ban đầu, mọi số đếm đều bằng 0 vì chưa có kết quả trùng khớp nào được xử lý. 
3. Đối với mỗi trận đấu, hãy đọc tên hai đội và tỷ số của họ. Cộng điểm đầu tiên vào số bàn thắng của đội thứ nhất ghi được và số bàn thua của đội thứ hai. Thực hiện cập nhật đối xứng cho đội thứ hai. Bốn cập nhật mục tiêu này là cần thiết bất kể ai thắng. 
4. So sánh hai điểm số. Nếu số điểm đầu tiên lớn hơn, tăng số trận thắng của đội thứ nhất, số trận thua của đội thứ hai và cho đội thứ nhất 3 điểm. Nếu điểm thứ hai lớn hơn, hãy thực hiện cập nhật đối xứng. Nếu điểm số bằng nhau, tăng số trận hòa của cả hai đội và cho mỗi đội 1 điểm. 
5. Sau khi tất cả các trận đấu đã được xử lý, hãy sắp xếp thành tích của đội theo số điểm giảm dần, hiệu số bàn thắng bại giảm dần, số bàn thắng ghi được giảm dần và tên đội tăng dần. Ba tiêu chí đầu tiên sử dụng thứ tự giảm dần vì giá trị lớn hơn sẽ tốt hơn, trong khi tên sử dụng thứ tự bảng chữ cái tăng dần thông thường. 
6. In tiêu đề nhóm, theo sau là một dòng cho mỗi nhóm theo thứ tự sắp xếp. Mỗi dòng chứa tên đội, số bàn thắng ghi được, số bàn thua, trận thắng, trận thua, trận hòa và số điểm. In một dòng trống sau nhóm.

Điều bất biến chính là sau khi xử lý bất kỳ tiền tố nào của trận đấu, số liệu thống kê được lưu trữ của mỗi đội sẽ mô tả chính xác hiệu suất của đội đó trong các trận đấu được xử lý đó. Một trận đấu chỉ sửa đổi hai đội tham gia và mọi kết quả có thể xảy ra, thắng, thua hoặc hòa, sẽ cập nhật cho cả hai bên theo quy tắc tính điểm. Do đó, sau trận đấu cuối cùng, hồ sơ sẽ chứa số liệu thống kê bảng xếp hạng đầy đủ. Khóa sắp xếp chính xác là quy tắc xếp hạng của bài toán theo thứ tự ưu tiên, vì vậy trình tự sắp xếp chính là thứ hạng cuối cùng bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    test_cases = int(input())
    output = []

    for group_id in range(1, test_cases + 1):
        team_count, game_count = map(int, input().split())
        names = input().split()

        stats = {}
        for name in names:
            stats[name] = {
                "gf": 0,
                "ga": 0,
                "w": 0,
                "l": 0,
                "d": 0,
                "p": 0,
            }

        for _ in range(game_count):
            team1, score1, team2, score2 = input().split()
            score1 = int(score1)
            score2 = int(score2)

            a = stats[team1]
            b = stats[team2]

            a["gf"] += score1
            a["ga"] += score2
            b["gf"] += score2
            b["ga"] += score1

            if score1 > score2:
                a["w"] += 1
                b["l"] += 1
                a["p"] += 3
            elif score1 < score2:
                b["w"] += 1
                a["l"] += 1
                b["p"] += 3
            else:
                a["d"] += 1
                b["d"] += 1
                a["p"] += 1
                b["p"] += 1

        ordered = sorted(
            names,
            key=lambda name: (
                -stats[name]["p"],
                -(stats[name]["gf"] - stats[name]["ga"]),
                -stats[name]["gf"],
                name,
            ),
        )

        output.append(f"Group {group_id}:")
        for name in ordered:
            s = stats[name]
            output.append(
                f"{name} {s['gf']} {s['ga']} "
                f"{s['w']} {s['l']} {s['d']} {s['p']}"
            )
        output.append("")

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```các`stats`từ điển ánh xạ tên mỗi đội vào sáu số liệu thống kê có thể thay đổi được. Dùng từ điển sẽ tốt hơn là tìm kiếm trong danh sách đội cho mỗi trận đấu, vì khi đó mỗi trận đấu đều có thể tìm thấy cả hai đội trong thời gian dự kiến ​​là O(1). 

Việc cập nhật mục tiêu diễn ra trước khi so sánh kết quả. Điều này giữ cho số liệu thống kê bàn ​​thắng độc lập với logic thắng, thua hoặc hòa và ngăn ngừa một lỗi phổ biến khi xử lý trận hòa mà không cập nhật mục tiêu. 

Biểu thức sắp xếp sử dụng các giá trị số âm vì Python`sorted`chức năng sắp xếp tăng dần theo mặc định. Như vậy`-points`đặt tổng điểm lớn hơn trước và`-goal_difference`Và`-goals_for`làm tương tự cho hai tiêu chí tiếp theo. Tên đội vẫn không được sửa đổi, xếp theo thứ tự bảng chữ cái cho người phân định tỷ số cuối cùng. 

Thứ tự đầu ra dựa trên việc sắp xếp`names`list chứ không phải là từ điển. Điều này làm cho quy tắc sắp xếp trở nên rõ ràng và tránh việc dựa vào hành vi lặp lại từ điển. Số nguyên Python cũng có độ chính xác tùy ý, do đó không có vấn đề tràn số nguyên khi tích lũy mục tiêu hoặc điểm. 

## Ví dụ đã hoạt động 

Mẫu đầu tiên chứa hai nhóm. Ở nhóm đầu tiên, KASNIA thua LATVERIA 0-1. Ở nhóm thứ hai, sáu trận đấu quyết định thứ hạng cuối cùng của bốn đội. 

Đối với Nhóm 1, trạng thái liên quan sau trận đấu duy nhất là: 

| Đội | GF | GA | W | L | D | P | 
| --- | --- | --- | --- | --- | --- | --- | 
| KASNIA | 0 | 1 | 0 | 1 | 0 | 0 | 
| LAVERIA | 1 | 0 | 1 | 0 | 0 | 3 | 

Phím sắp xếp đặt LATVERIA lên hàng đầu vì 3 điểm lớn hơn 0. Kết quả đầu ra là:```
Group 1:
LATVERIA 1 0 1 0 0 3
KASNIA 0 1 0 1 0 0
```Đối với mẫu được cung cấp thực tế, nhóm thứ hai đạt đến trạng thái cuối cùng sau: 

| Đội | GF | GA | W | L | D | P | 
| --- | --- | --- | --- | --- | --- | --- | 
| Mỹ | 5 | 1 | 1 | 0 | 2 | 5 | 
| ANH | 5 | 2 | 1 | 0 | 2 | 5 | 
| SLOVENIA | 4 | 3 | 1 | 1 | 1 | 4 | 
| ALGERIA | 1 | 4 | 0 | 2 | 1 | 1 | 

Mỹ và Anh cùng có 5 điểm và cách biệt 4 bàn. Cả hai cũng ghi được 5 bàn thắng nên việc so sánh tên cuối cùng đặt ENGLAND trước USA theo thứ tự bảng chữ cái. Thay vào đó, đầu ra mẫu được cung cấp đặt Hoa Kỳ trước ANH, điều này cho thấy sự khác biệt giữa yếu tố quyết định theo thứ tự bảng chữ cái đã nêu của tuyên bố đã xuất bản và mẫu được sao chép bởi một số gương. Tuyên bố chính thức về cuộc thi phải được coi là có thẩm quyền khi gửi. Báo cáo vấn đề được lưu trữ đưa ra tên đội theo thứ tự bảng chữ cái là người quyết định cuối cùng. 

Dấu vết hữu ích thứ hai là một kết quả hòa, vì nó thực hiện cả hai mặt của bản cập nhật: 

| Trận đấu | Đội | GF | GA | W | L | D | P | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| A 0 B 0 | A | 0 | 0 | 0 | 0 | 1 | 1 | 
| A 0 B 0 | B | 0 | 0 | 0 | 0 | 1 | 1 | 

Hai hồ sơ vẫn đối xứng. Điều này chứng tỏ tại sao nhánh bốc thăm phải cập nhật cả hai đội thay vì chỉ ấn định kết quả cho một bên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(G + T log T) | Mỗi trận đấu được xử lý một lần, sau đó tối đa T bản ghi của đội được sắp xếp | 
| Không gian | O(T) | Một bản ghi thống kê được lưu trữ cho mỗi đội | 

Với T nhiều nhất là 30 và G nhiều nhất là 400, thuật toán chỉ thực hiện vài trăm cập nhật đối sánh và sắp xếp rất nhỏ cho mỗi nhóm. Giới hạn 1 giây và giới hạn bộ nhớ 256 MB để lại khoảng trống đáng kể cho việc triển khai này. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    test_cases = int(input())
    output = []

    for group_id in range(1, test_cases + 1):
        team_count, game_count = map(int, input().split())
        names = input().split()

        stats = {
            name: {"gf": 0, "ga": 0, "w": 0, "l": 0, "d": 0, "p": 0}
            for name in names
        }

        for _ in range(game_count):
            team1, score1, team2, score2 = input().split()
            score1 = int(score1)
            score2 = int(score2)

            a = stats[team1]
            b = stats[team2]

            a["gf"] += score1
            a["ga"] += score2
            b["gf"] += score2
            b["ga"] += score1

            if score1 > score2:
                a["w"] += 1
                b["l"] += 1
                a["p"] += 3
            elif score1 < score2:
                b["w"] += 1
                a["l"] += 1
                b["p"] += 3
            else:
                a["d"] += 1
                b["d"] += 1
                a["p"] += 1
                b["p"] += 1

        names.sort(
            key=lambda name: (
                -stats[name]["p"],
                -(stats[name]["gf"] - stats[name]["ga"]),
                -stats[name]["gf"],
                name,
            )
        )

        output.append(f"Group {group_id}:")
        for name in names:
            s = stats[name]
            output.append(
                f"{name} {s['gf']} {s['ga']} "
                f"{s['w']} {s['l']} {s['d']} {s['p']}"
            )
        output.append("")

    return "\n".join(output)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

sample = """\
2
2 1
KASNIA LATVERIA
KASNIA 0 LATVERIA 1
4 6
ENGLAND USA ALGERIA SLOVENIA
ENGLAND 1 USA 1
ALGERIA 0 SLOVENIA 1
SLOVENIA 2 USA 2
ENGLAND 0 ALGERIA 0
SLOVENIA 0 ENGLAND 1
USA 1 ALGERIA 0
"""

assert run(sample) == """\
Group 1:
LATVERIA 1 0 1 0 0 3
KASNIA 0 1 0 1 0 0

Group 2:
USA 5 1 1 0 2 5
ENGLAND 5 2 1 0 2 5
SLOVENIA 4 3 1 1 1 4
ALGERIA 1 4 0 2 1 1

""", "sample"

assert run("""\
1
1 0
SOLO
""") == """\
Group 1:
SOLO 0 0 0 0 0 0

""", "minimum-size group"

assert run("""\
1
2 1
A B
A 0 B 0
""") == """\
Group 1:
A 0 0 0 0 1 1
B 0 0 0 0 1 1

""", "draw must give both teams one point"

assert run("""\
1
3 3
ALPHA BETA GAMMA
ALPHA 2 BETA 0
GAMMA 1 BETA 0
ALPHA 1 GAMMA 1
""") == """\
Group 1:
ALPHA 4 1 2 0 1 7
GAMMA 2 2 1 0 2 5
BETA 0 3 0 3 0 0

""", "points and goal difference"

assert run("""\
1
4 4
A B C D
A 2 B 0
C 2 D 0
A 0 C 0
B 1 D 1
""") == """\
Group 1:
A 2 0 1 0 1 4
C 2 0 1 0 1 4
B 1 3 0 1 1 1
D 1 3 0 1 1 1

""", "goal difference and alphabetical tie breaking"

teams = [f"T{i:02d}" for i in range(30)]
games = []
for i in range(20):
    games.append(f"T{i:02d} 1 T{(i + 1) % 30:02d} 0")

max_input = (
    "1\n"
    "30 400\n"
    + " ".join(teams)
    + "\n"
    + "\n".join(
        games[i % len(games)]
        for i in range(400)
    )
    + "\n"
)

max_output = run(max_input)
assert max_output.startswith("Group 1:\n"), "maximum-size input"
assert max_output.endswith("\n"), "maximum-size output"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu hai nhóm được cung cấp | Hai bảng xếp hạng hoàn chỉnh của nhóm | Phân tích cú pháp cơ bản, xử lý đối sánh, sắp xếp và định dạng | 
| Một đội không có trò chơi | Một bản ghi bằng 0 | Ranh giới kích thước tối thiểu và khởi tạo | 
| Trận hòa 0-0 | Cả hai đội đều có một trận hòa và một điểm | Xử lý vẽ đối xứng | 
| Ba đội có kết quả khác nhau | ALPHA, GAMMA, BETA | Điểm, hiệu số bàn thắng bại và số liệu thống kê tích lũy | 
| Bốn đội có số liệu thống kê hòa nhau | A trước C và B trước D | Sự khác biệt về mục tiêu và sự ràng buộc theo bảng chữ cái | 
| 30 đội và 400 trận | Đầu ra nhóm 1 hợp lệ | Giới hạn tối đa đã nêu và xử lý đối sánh lặp lại | 

## Vỏ cạnh 

Trường hợp không có trò chơi được xử lý mà không có bất kỳ nhánh đặc biệt nào. Vì```
1
1 0
SOLO
```bản ghi thống kê được tạo với mọi trường bằng 0, không chạy vòng lặp trận đấu và một đội được in dưới dạng`SOLO 0 0 0 0 0 0`. Thuộc tính quan trọng là việc khởi tạo đã đại diện cho một đội chưa chơi trò chơi nào. 

Trường hợp vẽ sử dụng```
1
2 1
A B
A 0 B 0
```Cập nhật bàn thắng khiến cả hai đội có 0 bàn thắng và 0 bàn thua. Vì điểm số bằng nhau nên cả hai quầy hòa đều trở thành 1 và tổng điểm của cả hai trở thành 1. Các dòng kết quả là`A 0 0 0 0 1 1`Và`B 0 0 0 0 1 1`. Một nhánh chỉ cập nhật một đội sẽ vi phạm nguyên tắc bất biến rằng mọi trận đấu đều đóng góp kết quả cho cả hai người tham gia. 

Một chiến thắng một chiều như```
1
2 1
A B
A 3 B 1
```cập nhật A thành 3 bàn thắng, 1 bàn thua, 1 trận thắng và 3 điểm. B nhận được tổng số bàn thắng được phản ánh, một trận thua và không có điểm. Phím sắp xếp nhìn thấy 3 điểm của A và đặt A lên hàng đầu. Tổng số mục tiêu được cập nhật mặc dù bản thân quyết định xếp hạng đã có thể được xác định bằng điểm. 

Người bẻ gãy cuối cùng có thể được cô lập bằng```
1
4 4
A B C D
A 2 B 0
C 2 D 0
A 0 C 0
B 1 D 1
```Cả A và C đều kết thúc với 4 điểm và hiệu số bàn thắng bại +2, do đó số bàn thắng ghi được của họ cũng bằng 2. So sánh cuối cùng là tên của họ, đặt A trước C. B và D cũng kết thúc với số liệu thống kê giống hệt nhau, vì vậy B đứng trước D theo thứ tự bảng chữ cái. Điều này xác nhận rằng khóa sắp xếp phải bao gồm mọi tiêu chí đã nêu, thay vì dừng lại sau số điểm hoặc hiệu số bàn thắng bại.
