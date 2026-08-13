---
title: "CF 102365B - Máy bay chiến đấu cân bằng"
description: "Chúng tôi có tới 100 máy bay chiến đấu. Mỗi võ sĩ được mô tả bằng tên và ba số liệu thống kê: sức khỏe, tấn công và phòng thủ. Khi hai võ sĩ gặp nhau, mỗi hiệp sẽ gây sát thương cố định cho cả hai bên."
date: "2026-08-12T23:45:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102365
codeforces_index: "B"
codeforces_contest_name: "UBC Programming Contest 2019 (UBCPC 2019)"
rating: 0
weight: 102365
solve_time_s: 92
verified: true
draft: false
---

[CF 102365B - Máy bay chiến đấu cân bằng](https://codeforces.com/problemset/problem/102365/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có tới 100 máy bay chiến đấu. Mỗi võ sĩ được mô tả bằng tên và ba số liệu thống kê: sức khỏe, tấn công và phòng thủ. Khi hai võ sĩ gặp nhau, mỗi hiệp sẽ gây sát thương cố định cho cả hai bên. Sát thương sắp tới của võ sĩ là đòn tấn công của đối thủ trừ đi khả năng phòng thủ của chính họ, bị giới hạn ở mức 0. Cả hai giá trị thiệt hại được áp dụng đồng thời. 

Nhiệm vụ là tìm từng nhóm ba chiến binh có kết quả theo cặp tạo thành một chu kỳ có định hướng. Đối với ba võ sĩ A, B và C, điều này có nghĩa là một võ sĩ đánh bại võ sĩ thứ hai, võ sĩ thứ hai đánh bại võ sĩ thứ ba và võ sĩ thứ ba đánh bại võ sĩ thứ nhất. Một trận hòa không được tính là một trận thắng nên mỗi cạnh trong vòng đấu phải thể hiện một chiến thắng thực sự. 

Dòng đầu tiên cung cấp N, theo sau là N mô tả máy bay chiến đấu. Đầu ra bắt đầu bằng số bộ ba hợp lệ, theo sau là một dòng cho mỗi bộ ba như vậy. Thứ tự của các bộ ba và thứ tự của ba tên trong mỗi bộ ba là không hạn chế. 

Ràng buộc N <= 100 đủ nhỏ cho O(N^3), có nghĩa là chúng ta có thể kiểm tra mọi nhóm ba có thể có. Điều chúng tôi không đủ khả năng là mô phỏng liên tục hàng nghìn hiệp đấu cho mỗi cặp trong mỗi bộ ba. Với 100 máy bay chiến đấu, có C(100, 3) = 161.700 bộ ba và có thể có hàng triệu hoặc hàng tỷ lượt hoạt động nếu mỗi trận chiến được mô phỏng trực tiếp. Do đó, mục tiêu hữu ích là làm cho mỗi trận đấu theo cặp có thời gian liên tục, sau đó chỉ dành O(N^3) để kiểm tra bộ ba. 

Giá trị sức khỏe, tấn công và phòng thủ tối đa là 10.000. Số nguyên Python dễ dàng xử lý tất cả các sản phẩm có liên quan, do đó không có vấn đề tràn. Quan trọng hơn, lượng máu tối đa giới hạn số viên đạn cần thiết để tiêu diệt một chiến binh sau khi gây ra sát thương tích cực, nhưng việc dựa vào thực tế đó để mô phỏng trực tiếp vẫn sẽ quá đắt. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai bất cẩn trở nên sai lầm. Đầu tiên là một cuộc chiến mà không võ sĩ nào có thể gây sát thương cho đối phương. Ví dụ,```
1
Solo 500 500 500
```không có đối thủ nên câu trả lời đơn giản là số không. Tổng quát hơn, nếu cả hai giá trị sát thương nhận vào đều bằng 0, trận chiến không bao giờ kết thúc và phải được coi là hòa. Một mô phỏng chờ đợi một giá trị sức khỏe trở thành không dương mà không kiểm tra mức thiệt hại bằng 0 sẽ lặp lại mãi mãi. 

Trường hợp thứ hai là cái chết đồng thời. Hãy xem xét hai máy bay chiến đấu với số liệu thống kê sau:```
2
A 4 6 1
B 10 3 1
```A gây 5 sát thương mỗi hiệp cho B, trong khi B gây 2 sát thương mỗi hiệp cho A. B chết sau 2 hiệp, trong khi A cũng về 0 sau 2 hiệp. Kết quả là hòa chứ không phải là thắng A. Điều kiện chiến thắng rất khắt khe: sau vòng giết chóc, người chiến thắng vẫn phải có sức khỏe tốt. 

Trường hợp cạnh thứ ba xảy ra khi một võ sĩ cần vài hiệp để đánh bại đối thủ. Giả sử A gây 5 sát thương mỗi hiệp cho B, B bắt đầu với 10 HP và B gây 2 sát thương mỗi hiệp cho A. Nếu A có 5 HP, cả hai võ sĩ đều chết sau hiệp thứ hai. Thay vào đó, nếu A có 6 HP, A sẽ sống sót trong vòng đó và giành chiến thắng. Sử dụng một so sánh không nghiêm ngặt như`<=`trong bài kiểm tra sức khỏe cuối cùng sẽ phân loại sai trường hợp đầu tiên là thắng lợi. 

Cuối cùng, các trận hòa không được vô tình trở thành các cạnh trong biểu đồ kết quả đấu sĩ. Bộ ba có kết quả hòa không thể là bộ ba nội động, ngay cả khi hai bộ còn lại theo cặp thắng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê từng bộ ba võ sĩ và mô phỏng ba trận đấu cần thiết để xác định xem liệu nó có phải là nội động hay không. Điều này đúng vì định nghĩa của bộ ba nội động chỉ phụ thuộc vào ba kết quả theo cặp đó. Nếu một trận chiến được mô phỏng từng hiệp một, thì mỗi hiệp sẽ cập nhật cả hai giá trị sức khỏe cho đến khi một đấu sĩ chết hoặc trận đấu được coi là hòa. 

Vấn đề là công việc lặp đi lặp lại. Có thể có 161.700 bộ ba khi N là 100. Mỗi bộ ba cần ba trận chiến và một trận chiến có thể cần tới 10.000 hiệp khi sát thương mỗi hiệp chỉ là một điểm. Điều đó đưa ra giới hạn trên trong trường hợp xấu nhất là khoảng 4,85 tỷ vòng mô phỏng. Con số thực tế có thể nhỏ hơn đối với nhiều đầu vào, nhưng con số này gần như không phù hợp với giới hạn một giây. 

Nhận xét quan trọng là một cuộc chiến không thực sự yêu cầu mô phỏng từng vòng một. Khi đấu với một đối thủ cố định, cả hai võ sĩ đều nhận sát thương như nhau trong mỗi hiệp. Chúng ta có thể tính toán mỗi võ sĩ cần chết bao nhiêu hiệp và so sánh trực tiếp hai con số đó. 

Giả sử A đang đánh B. Hãy để`damage_to_A = max(0, AT_B - DF_A)`Và`damage_to_B = max(0, AT_A - DF_B)`. 

Nếu như`damage_to_B`là dương, B chết sau`ceil(HP_B / damage_to_B)`vòng. Đúng vòng đó, A thắng chính xác khi lượng máu còn lại của A dương. Vậy A thắng B khi`ceil(HP_B / damage_to_B) * damage_to_A < HP_A`. 

Nếu B không thể gây sát thương lên đối thủ của A, nghĩa là`damage_to_B`bằng 0, B không bao giờ có thể chết nên A không thể thắng. Lý do tương tự xử lý theo hướng ngược lại. 

Điều này biến mọi trận chiến theo cặp thành O(1). Chúng ta có thể tính toán trước cặp thắng cuộc trong mỗi cặp có thứ tự một lần, lưu kết quả vào ma trận boolean. Sau đó, việc kiểm tra bộ ba chỉ cần một vài thao tác boolean. Ý tưởng vũ phu vẫn tồn tại ở cấp độ bên ngoài, nhưng phần đắt tiền đã bị loại bỏ. 

Do đó, mối quan hệ giữa hai cách tiếp cận này rất đơn giản. Giải pháp vũ lực hoạt động vì mọi bộ ba đều có thể được kiểm tra độc lập, nhưng nó không thành công vì nó liên tục thực hiện cùng một mô phỏng chiến đấu. Nhận thấy rằng trận chiến có sát thương không đổi mỗi hiệp cho phép chúng tôi thay thế mọi mô phỏng bằng một phép tính số học. Khi tất cả các kết quả theo cặp được lưu vào bộ nhớ đệm, việc kiểm tra tất cả các bộ ba trong O(N^3) là đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^3 · H) | O(1) | Quá chậm | 
| Tối ưu | O(N^2 + N^3) = O(N^3) | O(N^2) | Đã chấp nhận | 

Ở đây H là số vòng mô phỏng tối đa, có thể lên tới 10.000. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các chiến binh và lưu trữ tên, sức khỏe, đòn tấn công và phòng thủ của họ. Chúng tôi giữ các đấu ngư theo thứ tự đầu vào sao cho mỗi tổ hợp chỉ số`i < j < k`đại diện cho chính xác một bộ ba máy bay chiến đấu. 
2. Tạo ma trận boolean N by N`win`. giá trị`win[i][j]`có nghĩa là võ sĩ i đánh bại võ sĩ j. Giá trị bị thiếu hoặc sai có nghĩa là tôi không thắng, bao gồm cả thua và hòa. 
3. Đối với mỗi cặp đấu sĩ riêng biệt A và B theo thứ tự, hãy tính sát thương mà A nhận được từ B và sát thương B nhận được từ A. Những giá trị này không bao giờ thay đổi trong suốt trận đấu, vì vậy không có lý do gì để mô phỏng từng hiệp đấu riêng lẻ. 
4. Nếu thiệt hại của B đối với A bằng 0 thì A không thể đánh bại B, vì sức khỏe của A không bao giờ có thể đạt đến 0. Ngược lại hãy tính số vòng B cần chết là`(HP_B + damage_to_B - 1) // damage_to_B`. A thắng chính xác khi sát thương mà A nhận được trong các hiệp đấu đó vẫn nhỏ hơn rất nhiều so với lượng máu ban đầu của A. 
5. Lưu kết quả vào`win[A][B]`. Lặp lại điều này cho mỗi cặp được đặt hàng. Vì kết quả của một trận đấu không nhất thiết phải đối xứng nên cần phải xem xét cả hai hướng, mặc dù trên thực tế, việc tính toán cho một cặp có thể xác định cả hai. 
6. Liệt kê từng bộ ba`i < j < k`. Bộ ba có giá trị nếu kết quả tạo thành một chu kỳ theo một trong hai hướng. Chúng tôi kiểm tra`i beats j`,`j beats k`,`k beats i`, hoặc chu kỳ ngược lại`i beats k`,`k beats j`,`j beats i`. 

Việc kiểm tra cả hai hướng đều quan trọng vì đầu ra không quy định chiến binh nào phải xuất hiện trước. Đối với ba máy bay chiến đấu bất kỳ tạo thành một chu kỳ có hướng, chính xác một trong hai hướng này sẽ khớp nhau. 
7. Lưu trữ mọi bộ ba hợp lệ và cuối cùng in số lượng của nó, theo sau là tên ba máy bay chiến đấu tương ứng. Bởi vì các chỉ số chỉ được xem xét với`i < j < k`, cùng một bầy đấu ngư không bao giờ được xuất ra hai lần. 

Tại sao nó hoạt động: sau khi tiền xử lý,`win[A][B]`đúng khi A có máu dương sau hiệp đấu mà B không còn máu. Công thức tính số vòng đó là chính xác vì B thua số tiền dương như nhau ở mỗi vòng. Nếu B không thể nhận sát thương, kết quả được lưu trữ là sai, thể hiện chính xác một trận hòa hoặc tình huống A không thể thắng. Vì vậy mỗi cạnh trong`win`ma trận thể hiện chính xác một chiến thắng thực sự. Đối với mỗi ba chỉ số, thuật toán chấp nhận chính xác khi ba cạnh đó tạo thành một chu trình có hướng, đây chính xác là định nghĩa của bộ ba nội động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    fighters = []
    for _ in range(n):
        name, hp, atk, defense = input().split()
        fighters.append((name, int(hp), int(atk), int(defense)))

    win = [[False] * n for _ in range(n)]

    for i in range(n):
        name_a, hp_a, atk_a, def_a = fighters[i]

        for j in range(n):
            if i == j:
                continue

            name_b, hp_b, atk_b, def_b = fighters[j]

            damage_to_a = max(0, atk_b - def_a)
            damage_to_b = max(0, atk_a - def_b)

            if damage_to_b == 0:
                continue

            rounds_to_kill_b = (
                hp_b + damage_to_b - 1
            ) // damage_to_b

            if rounds_to_kill_b * damage_to_a < hp_a:
                win[i][j] = True

    answer = []

    for i in range(n):
        for j in range(i + 1, n):
            for k in range(j + 1, n):
                cycle_1 = (
                    win[i][j]
                    and win[j][k]
                    and win[k][i]
                )

                cycle_2 = (
                    win[i][k]
                    and win[k][j]
                    and win[j][i]
                )

                if cycle_1 or cycle_2:
                    answer.append(
                        (fighters[i][0], fighters[j][0], fighters[k][0])
                    )

    print(len(answer))
    for a, b, c in answer:
        print(a, b, c)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào lưu trữ mỗi máy bay chiến đấu dưới dạng`(name, HP, AT, DF)`. Việc chuyển đổi ba số liệu thống kê thành số nguyên ngay lập tức giúp cho phép tính số học sau này trở nên đơn giản. 

Quá trình tiền xử lý theo cặp tuân theo các bước thuật toán thứ tư và thứ năm. Vì A chống lại B,`damage_to_b`là số tiền B thua mỗi hiệp. Nếu nó bằng 0, B không bao giờ có thể đạt HP bằng 0, vì vậy A không thể thắng và mục nhập ma trận vẫn sai. 

Khi`damage_to_b`là dương, phép chia trần tính toán chính xác vòng đầu tiên mà sức khỏe của cuối B không dương. biểu hiện`(hp_b + damage_to_b - 1) // damage_to_b`là dạng chia trần tiêu chuẩn chỉ có số nguyên. Các số nguyên có độ chính xác tùy ý trong Python cũng thực hiện phép nhân`rounds_to_kill_b * damage_to_a`an toàn mà không cần bất kỳ xử lý đặc biệt nào. 

Sự so sánh có chủ ý chặt chẽ. Nếu sản phẩm bằng`hp_a`, A cũng về 0 ở hiệp đó nên trận đấu hòa. Một chiến thắng đòi hỏi`rounds_to_kill_b * damage_to_a < hp_a`. 

Ba vòng sử dụng`i < j < k`, do đó mỗi bộ ba chiến binh không theo thứ tự xuất hiện đúng một lần. Hai biểu thức chu trình bao gồm cả hai hướng có thể có của ba chu kỳ có hướng. Một trận hòa không bao giờ thỏa mãn cả hai biểu thức vì các trận hòa được thể hiện bằng các mục nhập sai trong`win`. 

Không có mô phỏng chiến đấu nào xuất hiện trong chương trình cuối cùng. Mỗi cặp được rút gọn thành một vài phép tính số học và mỗi bộ ba được rút gọn thành sáu phép tra cứu boolean. 

## Ví dụ đã hoạt động 

Mẫu đầu tiên chứa năm máy bay chiến đấu:```
5
TheStrong 90 60 10
TheInvincible 10000 10000 10000
TheTough 70 50 25
TheBrick 3 1 4159
TheResilient 160 40 10
```Các kết quả theo cặp có liên quan có thể được tìm thấy như sau. 

| Cặp | Thiệt hại đầu tiên | Thiệt hại thứ hai | Vòng giết thứ hai | Đầu tiên sống sót? | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| TheStrong vs TheTough | 40 | 35 | 2 | 90 - 70 = 20 > 0 | Thắng mạnh | 
| TheTough vs TheResilient | 15 | 40 | 4 | 70 - 60 = 10 > 0 | Chiến thắng khó khăn | 
| TheResilient vs TheStrong | 30 | 50 | 2 | 160 - 60 = 100 > 0 | Chiến thắng kiên cường | 

Ba kết quả này hình thành nên chu trình`TheStrong -> TheTough -> TheResilient -> TheStrong`. Các máy bay chiến đấu khác không tạo ra một chu kỳ hợp lệ khác, vì vậy sản lượng cuối cùng là một bộ ba.```
1
TheStrong TheTough TheResilient
```Mẫu chính thức cho phép mọi thứ tự của tên, vì vậy điều này tương đương với thứ tự của mẫu. 

Mẫu thứ hai chỉ chứa một đấu ngư:```
1
TheLonely 500 500 500
```Bộ liệt kê ba không có sự kết hợp thỏa mãn`i < j < k`, vì vậy không cần phải thực hiện chiến đấu theo cặp nào cả. 

| tôi | j | k | Đã kiểm tra ba lần? | Kết quả | 
| --- | --- | --- | --- | --- | 
| không | không | không | Không, N < 3 | Không có bộ ba | 

Do đó, đầu ra là:```
0
```Dấu vết này thực hiện đầu vào nhỏ nhất có thể và xác nhận rằng thuật toán không cho rằng có ít nhất ba máy bay chiến đấu tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N^3) | Tiền xử lý theo cặp O(N^2) cộng với phép liệt kê ba lần O(N^3) | 
| Không gian | O(N^2) | Ma trận người chiến thắng lưu trữ một kết quả cho mỗi cặp đã đặt hàng | 

Với N = 100, phép liệt kê ba chỉ kiểm tra 161.700 kết hợp. Quá trình tiền xử lý theo cặp kiểm tra 10.000 cặp có thứ tự và mọi thao tác bên trong các vòng lặp đó đều có thời gian không đổi. Điều này là thoải mái trong giới hạn đã nêu. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    fighters = []

    for _ in range(n):
        name, hp, atk, defense = input().split()
        fighters.append((name, int(hp), int(atk), int(defense)))

    win = [[False] * n for _ in range(n)]

    for i in range(n):
        _, hp_a, atk_a, def_a = fighters[i]

        for j in range(n):
            if i == j:
                continue

            _, hp_b, atk_b, def_b = fighters[j]

            damage_to_a = max(0, atk_b - def_a)
            damage_to_b = max(0, atk_a - def_b)

            if damage_to_b == 0:
                continue

            rounds = (hp_b + damage_to_b - 1) // damage_to_b

            if rounds * damage_to_a < hp_a:
                win[i][j] = True

    answer = []

    for i in range(n):
        for j in range(i + 1, n):
            for k in range(j + 1, n):
                if (
                    (win[i][j] and win[j][k] and win[k][i])
                    or
                    (win[i][k] and win[k][j] and win[j][i])
                ):
                    answer.append((
                        fighters[i][0],
                        fighters[j][0],
                        fighters[k][0]
                    ))

    result = [str(len(answer))]
    for a, b, c in answer:
        result.append(f"{a} {b} {c}")

    return "\n".join(result)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

sample1 = """\
5
TheStrong 90 60 10
TheInvincible 10000 10000 10000
TheTough 70 50 25
TheBrick 3 1 4159
TheResilient 160 40 10
"""

assert run(sample1) == """\
1
TheStrong TheTough TheResilient
""", "sample 1"

assert run("""\
1
TheLonely 500 500 500
""") == """\
0
""", "sample 2"

assert run("""\
3
A 10 10 10
B 10 10 10
C 10 10 10
""") == """\
0
""", "all equal values"

assert run("""\
2
A 4 6 1
B 10 3 1
""") == """\
0
""", "simultaneous death must be a draw"

assert run("""\
3
A 6 6 1
B 10 3 1
C 100 1 100
""") == """\
0
""", "boundary and no-damage cases"

max_input = ["100"]
for i in range(100):
    max_input.append(f"F{i} 10000 10000 10000")

assert run("\n".join(max_input) + "\n") == """\
0
""", "maximum N with all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / TheLonely 500 500 500`|`0`| Đầu vào có kích thước tối thiểu không thể có ba | 
| Ba máy bay chiến đấu giống hệt nhau |`0`| Tất cả các trận đấu đều hòa vì mọi đòn tấn công đều bị phòng thủ hấp thụ | 
|`A 4 6 1`,`B 10 3 1`|`0`| Cả hai võ sĩ đều chết trong cùng một hiệp nên bình đẳng không được tính là thắng | 
| Ba máy bay chiến đấu trong đó có một máy bay chiến đấu có khả năng phòng thủ 100 chống lại đòn tấn công 1 |`0`| Trận chiến không gây sát thương và số học ranh giới | 
| 100 chiến binh giống hệt nhau |`0`| Bảng liệt kê N và O(N^3) tối đa theo các ràng buộc thực tế | 

Thử nghiệm mẫu sử dụng thứ tự xác định được tạo ra bởi`i < j < k`. Vì vấn đề chấp nhận thứ tự tùy ý, nên việc triển khai hợp lệ khác có thể in cùng một bộ ba theo thứ tự khác. 

## Vỏ cạnh 

Trường hợp không hư hỏng sẽ được xử lý trước khi chia trần. Hãy xem xét một cuộc chiến mà A không thể gây sát thương cho B vì`AT_A <= DF_B`. Sau đó`damage_to_b`bằng 0 nên sức khỏe của B không bao giờ giảm. Thuật toán ghi lại ngay lập tức`win[A][B] = False`. Ví dụ, với`A 100 10 100`Và`B 100 10 100`, cả hai giá trị sát thương đều bằng 0, vì vậy trận đấu là hòa. Thuật toán không bao giờ thử mô phỏng vô hạn. 

Vụ án chết cùng lúc được xử lý nghiêm minh`<`so sánh. Với```
2
A 4 6 1
B 10 3 1
```một giao dịch`6 - 1 = 5`gây sát thương cho B, trong khi B gây sát thương`3 - 1 = 2`thiệt hại cho A. B đạt đến mức 0 sau`ceil(10 / 5) = 2`vòng. A đã lấy`2 * 2 = 4`thiệt hại, chính xác là sức khỏe ban đầu của nó. Từ`4 < 4`là sai,`win[A][B]`vẫn sai. Chiều ngược lại cũng sai nên kết quả được coi là hòa. 

Ranh giới chính xác của vòng chung kết là sự so sánh tương tự từ phía bên kia. Nếu A có 5 HP thay vì 4 trong ví dụ đó thì A sẽ có`5 - 4 = 1`HP sau khi B chết nên`4 < 5`sẽ đúng và A sẽ thắng. Thay đổi một HP sẽ thay đổi kết quả chính xác theo đúng quy định của trò chơi. 

Đấu ngư không nhận sát thương từ đối thủ cũng được xử lý chính xác. Giả sử A có phòng thủ 100 và B có đòn tấn công 1. Khi đó sát thương sắp tới của A là`max(0, 1 - 100) = 0`. A có thể tồn tại vô thời hạn, nhưng chỉ điều đó không có nghĩa là A chiến thắng. Thuật toán kiểm tra riêng xem liệu A cuối cùng có thể giết được B hay không. Nếu A cũng không gây sát thương thì kết quả là hòa. Nếu A gây sát thương dương, B cuối cùng sẽ chết trong khi A vẫn còn sống, nên A thắng. 

Cuối cùng, bộ ba được kiểm tra theo cả hai hướng. Giả sử kết quả là`A beats B`,`B beats C`, Và`C beats A`. Nếu các chỉ số được sắp xếp theo thứ tự A, C, B thì biểu thức chu trình đầu tiên sẽ không khớp với thứ tự chỉ mục đó, nhưng biểu thức ngược lại thì có. Việc kiểm tra cả hai hướng làm cho kết quả không phụ thuộc vào thứ tự đầu vào của máy bay chiến đấu. Bởi vì mỗi bộ ba vẫn chỉ được tạo một lần với`i < j < k`, điều này không tạo ra đầu ra trùng lặp.
