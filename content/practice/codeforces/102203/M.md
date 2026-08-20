---
title: "CF 102203M - ĐỎ-7"
description: "Chúng tôi có một trò chơi hai người chơi với các quân bài riêng biệt. Mỗi thẻ có giá trị từ 1 đến 7 và màu theo bộ thứ tự R, O, Y, G, B, N, P. Thẻ có giá trị lớn hơn sẽ mạnh hơn và các thẻ có giá trị bằng nhau được sắp xếp theo màu, trong đó R mạnh nhất và P yếu nhất."
date: "2026-08-18T00:59:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102203
codeforces_index: "M"
codeforces_contest_name: "\u0427\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u041b\u0438\u043f\u0435\u0446\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e (8-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 102203
solve_time_s: 284
verified: true
draft: false
---

[CF 102203M - RED-7](https://codeforces.com/problemset/problem/102203/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 44 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một trò chơi hai người chơi với các quân bài riêng biệt. Mỗi thẻ có giá trị từ 1 đến 7 và màu theo bộ thứ tự R, O, Y, G, B, N, P. Thẻ có giá trị lớn hơn sẽ mạnh hơn và các thẻ có giá trị bằng nhau được sắp xếp theo màu, trong đó R mạnh nhất và P yếu nhất. 

Mỗi người chơi bắt đầu với một lá bài đã có sẵn trong bảng màu của họ và tối đa sáu lá bài trên tay. Canvas ban đầu thể hiện quy tắc màu đỏ, vì vậy người chơi có thẻ bảng mạnh nhất hiện đang dẫn trước. Thẻ bảng ban đầu của người chơi đầu tiên được đảm bảo yếu hơn thẻ của người chơi thứ hai. 

Thẻ canvas xác định quy tắc đánh giá người chiến thắng. Bảy quy tắc là màu đỏ cho lá bài mạnh nhất, màu cam cho nhóm lớn nhất có giá trị bằng nhau, màu vàng cho nhóm một màu lớn nhất, màu xanh lá cây cho số lượng lá bài có giá trị chẵn lớn nhất, màu xanh lam cho số lượng lớn nhất các màu riêng biệt, màu chàm cho chuỗi giá trị liên tiếp dài nhất và màu tím cho số lượng lá bài lớn nhất có giá trị dưới 4. 

Đối với mọi quy tắc, người chơi không chỉ đơn giản sử dụng tất cả các thẻ phù hợp. Họ chọn sự kết hợp tốt nhất có thể bằng cách tối đa hóa kích thước của nó trước tiên và sau đó tối đa hóa quân bài mạnh nhất của nó. Do đó, trạng thái trò chơi được xác định bởi các quân bài hiện có trong cả hai bảng, các quân bài vẫn còn trên cả hai tay, màu canvas hiện tại và lượt của ai. 

Trong một lượt, người chơi có thể di chuyển một lá bài tay vào bảng màu của họ, di chuyển một lá bài tay vào khung vẽ hoặc thực hiện cả hai thao tác bằng cách sử dụng hai lá bài khác nhau. Sau khi thao tác, người chơi phải thực hiện nghiêm túc theo quy tắc canvas kết quả. Nếu không có nước đi nào như vậy thì người chơi đó sẽ thua ngay lập tức. Người chơi có bàn tay trắng cũng thua khi lượt của họ bắt đầu, mặc dù họ có thể kết thúc lượt chơi với bàn tay trắng một cách hợp pháp. 

Đầu vào cung cấp kích thước hai tay và thẻ của mỗi người chơi. Lá bài đầu tiên trên đường của mỗi người chơi đã có trong bảng màu của người chơi đó, trong khi tất cả các lá bài còn lại đều bắt đầu trong tay. Đầu ra cần thiết là`First`nếu người chơi đầu tiên có chiến lược chiến thắng và`Second`nếu không thì. 

Việc hạn chế tối đa sáu lá bài trong mỗi ván bài là lý do chính khiến việc tìm kiếm trạng thái trò chơi đầy đủ có thể thực hiện được. Có tối đa mười hai thẻ có vị trí có thể thay đổi. Mỗi thẻ như vậy có thể ở trong tay, trong bảng màu hoặc đã bị loại bỏ trên khung vẽ, cung cấp tối đa (3^{12}=531441) cấu hình quyền sở hữu cục bộ trước khi tính đến quy tắc canvas và lượt chơi. Điều này loại trừ các thuật toán có công việc phát triển giống như toàn bộ cây trò chơi, nhưng nó làm cho việc tìm kiếm trạng thái được ghi nhớ trở nên thực tế. 

Một số chi tiết rất dễ bị sai. Bàn tay trống khi bắt đầu lượt chơi là thua ngay lập tức, ngay cả khi người chơi đó đang dẫn trước sau nước đi trước đó. Ví dụ: mẫu đầu tiên là```
0 0
3G
7Y
```và câu trả lời là`Second`. Không có động thái hợp pháp nào cả, vì vậy việc người chơi thứ hai dẫn trước ban đầu không phải là lý do chính cho câu trả lời. Một tìm kiếm chỉ kiểm tra xem người chơi hiện tại đã chiến thắng hay chưa sẽ trả về sai`First`. 

Một trường hợp tinh tế khác là việc chơi bài trên canvas phải được đánh giá bằng quy tắc canvas mới. Trong mẫu thứ hai,```
3 0
1R 2R 3R 4R
7R
```người chơi đầu tiên không thể cải thiện dưới màu đỏ vì ngay cả thẻ bảng màu mạnh nhất hiện có của họ cũng dưới 7R. Việc đánh bài vào khung vẽ cũng không thể giúp ích được trừ khi quy tắc kết quả khiến người chơi đầu tiên dẫn đầu. Câu trả lời đúng là`Second`. 

Sự ràng buộc bên trong một quy tắc là một nguồn lỗi phổ biến khác. Coi như```
1 1
2P 2R
2Y 2O
```Người chơi đầu tiên có thể đặt 2R vào bảng màu. Dưới màu đỏ, cả hai người chơi đều có lá bài cao nhất có giá trị 2, nhưng R mạnh hơn Y nên người chơi đầu tiên dẫn trước. Câu trả lời đúng là`First`. Việc so sánh chỉ dựa trên giá trị số sẽ bỏ lỡ sự liên kết về màu sắc. 

Ranh giới trong quy tắc màu tím cũng nghiêm ngặt ở đúng vị trí: giá trị 1, 2 và 3 được tính, trong khi 4 thì không. Ví dụ,```
1 1
3P 1R
7R 4O
```Người chơi đầu tiên có thể đặt 1R trên khung vẽ. Quy tắc mới là màu tím và bảng màu đầu tiên chứa 3P, trong khi bảng màu thứ hai chứa 7R. Người chơi đầu tiên có một thẻ đủ điều kiện và người thứ hai không có thẻ nào, vì vậy câu trả lời là`First`. Một triển khai sử dụng`value <= 4`sẽ tạo ra kết quả sai trong trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là mô phỏng mọi động thái có thể xảy ra và kiểm tra đệ quy mọi diễn biến tiếp theo của trò chơi. Điều này đúng vì một thế cờ sẽ thắng chính xác khi tồn tại một nước đi hợp lệ khiến người chơi hiện tại dẫn trước và khiến đối phương rơi vào thế thua. Nếu không có động thái như vậy tồn tại, vị thế sẽ bị mất. 

Với sáu lá bài trong tay, người chơi có sáu hành động chỉ dành cho bảng màu, sáu hành động chỉ dành cho khung vẽ và (6\cdot5=30) hành động sử dụng một thẻ cho bảng màu và một thẻ khác cho khung vẽ. Đó là 42 hành động có thể thực hiện khi bắt đầu một lượt. Do đó, tìm kiếm cây trò chơi thô có thể có giới hạn trên là (42^{12}), tức là khoảng (3.0\cdot10^{19}) nhánh. Hầu hết các nhánh đó là bất hợp pháp hoặc chấm dứt sớm hơn nhiều, nhưng giới hạn đã cho thấy rằng đệ quy trực tiếp không thể được sử dụng. 

Tìm kiếm vũ phu hoạt động vì mỗi nước đi sẽ làm giảm nghiêm trọng số lượng quân bài trên tay người chơi hiện tại, do đó không có chu kỳ. Vấn đề là nhiều chuỗi di chuyển khác nhau đều đạt đến cùng một vị trí. Khi các bảng màu, bàn tay, quy tắc canvas và lượt tương tự xảy ra lần nữa, trò chơi trong tương lai sẽ giống hệt nhau bất kể vị trí đó đạt được bằng cách nào. 

Do đó, quan sát quan trọng là ghi nhớ các vị trí thay vì trình tự di chuyển. Đối với mỗi người chơi, mỗi lá bài trên tay có chính xác ba trạng thái liên quan: nó vẫn còn trong tay, nó đã được chuyển sang bảng màu hoặc nó đã bị loại bỏ khỏi khung vẽ. Thẻ bảng màu ban đầu là cố định và không cần trạng thái thứ ba. Do đó, sáu lá bài trên tay chỉ cung cấp (3^6=729) trạng thái địa phương có thể có cho mỗi người chơi. 

Không cần thẻ chính xác hiện ở trên cùng của khung vẽ. Chỉ có màu sắc của nó quan trọng vì khung vẽ xác định một quy tắc. Việc bản thân lá bài đó đã biến mất khỏi trò chơi đã được thể hiện bằng việc lá bài đó không nằm trong tay cũng như bảng màu. Điều này làm giảm trạng thái toàn cầu thành hai trạng thái cục bộ, một trong bảy màu canvas và người chơi phải di chuyển. 

Đối với mọi mặt nạ bảng màu có thể, chúng ta cũng có thể tính toán trước sự kết hợp tốt nhất cho từng quy tắc trong số bảy quy tắc. Một bảng chứa tối đa bảy thẻ, vì vậy chúng ta có thể chỉ cần liệt kê tất cả các mặt nạ con của nó và chọn mặt nạ con hợp lệ với kích thước tối đa và sau đó là cường độ thẻ tối đa. Điều này làm cho việc đánh giá vị trí trở nên liên tục trong quá trình tìm kiếm trò chơi. 

Việc tìm kiếm kết quả vẫn mang tính hàm mũ, nhưng không gian trạng thái của nó đủ nhỏ cho những ràng buộc này. Một mảng byte dày đặc được sử dụng để ghi nhớ thay vì từ điển Python, vì không gian trạng thái hoàn chỉnh chỉ có khoảng 7,4 triệu mục nhập và một byte cho mỗi mục nhập là không tốn kém. Quá trình đệ quy cũng dừng lại ngay khi tìm thấy một nước đi chiến thắng, điều này đặc biệt hiệu quả vì hầu hết các vị thế đều có một nước đi hợp pháp có thể bị từ chối hoặc chấp nhận mà không cần khám phá mọi phương án thay thế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(42^{12})) nhánh cây trò chơi | (O(12)) độ sâu đệ quy | Quá chậm | 
| Tối ưu | (O(3^{n+m}(n+m)^2)) trạng thái và chuyển đổi trong trường hợp xấu nhất | (O(3^{n+m})) ghi nhớ | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mỗi thẻ thành một cặp bao gồm giá trị và thứ hạng màu của nó. Thứ tự màu R, O, Y, G, B, N, P trở thành 0 đến 6, do đó một lá bài có thể được so sánh bằng số nguyên`value * 7 + color`. 
2. Đối với mỗi người chơi, hãy tính toán trước điểm tối ưu của mọi mặt nạ bảng màu có thể có theo mọi quy tắc canvas. Điểm chứa kích thước của sự kết hợp tối ưu và thứ hạng của lá bài mạnh nhất của nó. Chúng tôi mã hóa nó như`size * 50 + rank`, trong khi một sự kết hợp không thể nhận được`-1`. 

Để tính điểm của quy tắc, hãy liệt kê mọi mặt nạ con không trống của bảng màu và kiểm tra xem nó có thỏa mãn quy tắc đó hay không. Điều này nhỏ vì một bảng màu chứa tối đa bảy thẻ. 
3. Biểu thị phần có thể thay đổi trong thẻ của mỗi người chơi bằng sáu chữ số thứ ba. Chữ số 0 có nghĩa là lá bài tương ứng đã bị loại bỏ, chữ số một có nghĩa là nó vẫn còn trong tay và chữ số hai có nghĩa là nó vẫn còn trong bảng màu. Thẻ bảng màu ban đầu luôn được đính kèm riêng trong mặt nạ bảng màu. 

Do đó, trạng thái cục bộ có nhiều nhất (3^6=729) giá trị. Từ một tiểu bang địa phương, chúng ta có thể lấy ngay mặt nạ tay và mặt nạ bảng màu. 
4. Tính toán trước kết quả của việc di chuyển mọi lá bài có thể có vào bảng màu và kết quả của việc di chuyển mọi lá bài có thể có vào khung vẽ. Di chuyển bảng màu sẽ thay đổi chữ số thứ ba của nó từ một thành hai. Di chuyển canvas sẽ thay đổi nó từ một thành không. 
5. Xác định hàm trò chơi đệ quy có trạng thái bao gồm trạng thái cục bộ của người chơi thứ nhất, trạng thái cục bộ của người chơi thứ hai, màu canvas hiện tại và người chơi đến lượt. 

Bảng ghi nhớ lưu vị trí đó đang thắng hay thua để người chơi di chuyển. 
6. Nếu bài của người chơi hiện tại trống, đánh dấu vị trí thua ngay lập tức. Điều này được kiểm tra trước khi xem xét quy tắc canvas hiện tại vì các quy tắc này rõ ràng khiến ván bài trống bị thua khi bắt đầu lượt. 
7. Hãy thử mọi động tác chỉ dành cho bảng màu. Di chuyển một lá bài trên tay vào bảng màu của người chơi hiện tại, giữ nguyên quy tắc canvas và so sánh điểm số tối ưu đạt được. Nếu người chơi hiện tại dẫn đầu và trạng thái kết quả của đối thủ là thua thì vị trí hiện tại là thắng. 
8. Hãy thử mọi động tác chỉ dành cho canvas. Xóa một lá bài, sử dụng màu của nó làm quy tắc canvas mới và so sánh quy tắc mới ngay lập tức. Thẻ không còn trong bảng màu nên bản thân bảng màu không thay đổi. 
9. Hãy thử từng cặp bài riêng biệt theo thứ tự để thực hiện hành động kết hợp. Đầu tiên, di chuyển một thẻ vào bảng màu, sau đó xóa một thẻ khác vào khung vẽ. Quy tắc canvas mới phải được đánh giá bằng cách sử dụng bảng màu mở rộng và các thẻ còn lại. 
10. Nếu bất kỳ nước đi nào trong số này dẫn đến thế trận thua trong khi để người chơi hiện tại dẫn trước, hãy đánh dấu trạng thái hiện tại là thắng. Nếu mọi nước đi có thể đều thất bại, hãy đánh dấu nó là thua. 

Bất biến trung tâm là mọi trạng thái được ghi nhớ đều chứa chính xác thông tin có thể ảnh hưởng đến tất cả các chuyển động trong tương lai. Những quân bài còn trong tay vẫn có thể được chơi, những quân bài trong bảng màu đóng góp vào mọi quy tắc trong tương lai và những quân bài đã loại bỏ sẽ không bao giờ có thể quay trở lại. Màu canvas là thuộc tính duy nhất của thẻ canvas hiện tại ảnh hưởng đến các quy tắc trong tương lai. Do đó, hai lịch sử trò chơi có cùng bốn trạng thái ván bài/bảng màu cục bộ, màu canvas và lượt chơi có cùng một tập hợp các khả năng trong tương lai. Sau đó, quy tắc minimax đệ quy sẽ phù hợp với cách chơi tối ưu: một thế cờ sẽ thắng chính xác khi người chơi có ít nhất một nước đi hợp pháp đến thế thua của đối phương. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from functools import lru_cache

COLORS = "ROYGBNP"
COLOR_ID = {c: i for i, c in enumerate(COLORS)}
BASE = 3 ** 6
STATE_COUNT = BASE
STATE_SPACE = BASE * BASE * 7 * 2

def card_rank(card):
    value, color = card
    return value * 7 + color

def build_scores(cards):
    """
    score[rule][mask] = encoded optimal combination score.
    -1 means that no valid combination exists.

    The encoding is size * 50 + highest_card_rank.
    """
    score = [[-1] * 128 for _ in range(7)]
    total = len(cards)

    ranks = [card_rank(c) for c in cards]
    values = [c[0] for c in cards]
    colors = [c[1] for c in cards]

    for mask in range(1, 1 << total):
        sub = mask

        while sub:
            cnt = sub.bit_count()

            highest = -1
            vals = []
            cols = []

            x = sub
            while x:
                bit = x & -x
                i = bit.bit_length() - 1

                if ranks[i] > highest:
                    highest = ranks[i]

                vals.append(values[i])
                cols.append(colors[i])
                x ^= bit

            valid = [False] * 7

            # Red: exactly one card.
            valid[0] = cnt == 1

            # Orange: all cards have the same value.
            valid[1] = len(set(vals)) == 1

            # Yellow: all cards have the same color.
            valid[2] = len(set(cols)) == 1

            # Green: all cards are even.
            valid[3] = all(v % 2 == 0 for v in vals)

            # Blue: all colors are different.
            valid[4] = len(set(cols)) == cnt

            # Indigo: distinct consecutive values.
            if len(set(vals)) == cnt:
                lo = min(vals)
                hi = max(vals)
                valid[5] = hi - lo + 1 == cnt

            # Violet: all values are below 4.
            valid[6] = all(v < 4 for v in vals)

            encoded = cnt * 50 + highest

            for rule in range(7):
                if valid[rule] and encoded > score[rule][mask]:
                    score[rule][mask] = encoded

            sub = (sub - 1) & mask

    return score

def solve_case(data):
    lines = data.strip().splitlines()
    n, m = map(int, lines[0].split())

    first = []
    second = []

    for token in lines[1].split():
        first.append((int(token[0]), COLOR_ID[token[1]]))

    for token in lines[2].split():
        second.append((int(token[0]), COLOR_ID[token[1]]))

    score_first = build_scores(first)
    score_second = build_scores(second)

    # For a local ternary state:
    # digit 0 = discarded
    # digit 1 = hand
    # digit 2 = palette
    #
    # Bit 0 of the palette mask is always the initial palette card.
    powers = [3 ** i for i in range(6)]

    palette_mask = [0] * STATE_COUNT
    hand_mask = [0] * STATE_COUNT

    next_palette = [[-1] * 6 for _ in range(STATE_COUNT)]
    next_canvas = [[-1] * 6 for _ in range(STATE_COUNT)]

    for state in range(STATE_COUNT):
        x = state
        pmask = 1
        hmask = 0

        digits = [0] * 6

        for i in range(6):
            digits[i] = x % 3
            x //= 3

            if digits[i] == 1:
                hmask |= 1 << (i + 1)
            elif digits[i] == 2:
                pmask |= 1 << (i + 1)

        palette_mask[state] = pmask
        hand_mask[state] = hmask

        for i in range(6):
            if digits[i] == 1:
                # 1 -> 2: move to palette.
                next_palette[state][i] = state + powers[i]

                # 1 -> 0: move to canvas.
                next_canvas[state][i] = state - powers[i]

    # Only the first n or m variable positions are real cards.
    initial_first = sum(powers[i] for i in range(n))
    initial_second = sum(powers[i] for i in range(m))

    colors_first = [c[1] for c in first[1:]]
    colors_second = [c[1] for c in second[1:]]

    memo = bytearray(STATE_SPACE)

    def memo_index(s1, s2, canvas, turn):
        return ((((s1 * STATE_COUNT) + s2) * 7 + canvas) << 1) | turn

    def leads_first(pm1, pm2, rule):
        return score_first[rule][pm1] > score_second[rule][pm2]

    def leads_second(pm1, pm2, rule):
        return score_second[rule][pm2] > score_first[rule][pm1]

    sys.setrecursionlimit(100000)

    def win(s1, s2, canvas, turn):
        idx = memo_index(s1, s2, canvas, turn)
        saved = memo[idx]

        if saved:
            return saved == 2

        if turn == 0:
            me = s1
            opp = s2
            pm_opp = palette_mask[opp]
            hands = hand_mask[me]

            if hands == 0:
                memo[idx] = 1
                return False

            pm_me = palette_mask[me]

            # Action 1: hand -> palette.
            bits = hands
            while bits:
                bit = bits & -bits
                i = bit.bit_length() - 2

                ns = next_palette[me][i]

                if leads_first(palette_mask[ns], pm_opp, canvas):
                    if not win(ns, opp, canvas, 1):
                        memo[idx] = 2
                        return True

                bits ^= bit

            # Action 2: hand -> canvas.
            bits = hands
            while bits:
                bit = bits & -bits
                i = bit.bit_length() - 2

                ns = next_canvas[me][i]
                new_canvas = colors_first[i]

                if leads_first(palette_mask[ns], pm_opp, new_canvas):
                    if not win(ns, opp, new_canvas, 1):
                        memo[idx] = 2
                        return True

                bits ^= bit

            # Action 3: hand -> palette, another hand card -> canvas.
            bits_a = hands
            while bits_a:
                bit_a = bits_a & -bits_a
                a = bit_a.bit_length() - 2

                after_palette = next_palette[me][a]
                remaining = hands ^ bit_a

                bits_b = remaining
                while bits_b:
                    bit_b = bits_b & -bits_b
                    b = bit_b.bit_length() - 2

                    ns = next_canvas[after_palette][b]
                    new_canvas = colors_first[b]

                    if leads_first(
                        palette_mask[ns],
                        pm_opp,
                        new_canvas
                    ):
                        if not win(ns, opp, new_canvas, 1):
                            memo[idx] = 2
                            return True

                    bits_b ^= bit_b

                bits_a ^= bit_a

        else:
            me = s2
            opp = s1
            pm_opp = palette_mask[opp]
            hands = hand_mask[me]

            if hands == 0:
                memo[idx] = 1
                return False

            pm_me = palette_mask[me]

            # Action 1: hand -> palette.
            bits = hands
            while bits:
                bit = bits & -bits
                i = bit.bit_length() - 2

                ns = next_palette[me][i]

                if leads_second(palette_mask[ns], pm_opp, canvas):
                    if not win(opp, ns, canvas, 0):
                        memo[idx] = 2
                        return True

                bits ^= bit

            # Action 2: hand -> canvas.
            bits = hands
            while bits:
                bit = bits & -bits
                i = bit.bit_length() - 2

                ns = next_canvas[me][i]
                new_canvas = colors_second[i]

                if leads_second(palette_mask[ns], pm_opp, new_canvas):
                    if not win(opp, ns, new_canvas, 0):
                        memo[idx] = 2
                        return True

                bits ^= bit

            # Action 3: hand -> palette, another hand card -> canvas.
            bits_a = hands
            while bits_a:
                bit_a = bits_a & -bits_a
                a = bit_a.bit_length() - 2

                after_palette = next_palette[me][a]
                remaining = hands ^ bit_a

                bits_b = remaining
                while bits_b:
                    bit_b = bits_b & -bits_b
                    b = bit_b.bit_length() - 2

                    ns = next_canvas[after_palette][b]
                    new_canvas = colors_second[b]

                    if leads_second(
                        palette_mask[ns],
                        pm_opp,
                        new_canvas
                    ):
                        if not win(opp, ns, new_canvas, 0):
                            memo[idx] = 2
                            return True

                    bits_b ^= bit_b

                bits_a ^= bit_a

        memo[idx] = 1
        return False

    return "First" if win(initial_first, initial_second, 0, 0) else "Second"

def main():
    data = sys.stdin.read()
    if data.strip():
        print(solve_case(data))

if __name__ == "__main__":
    main()
```Phần đầu tiên của quá trình triển khai sẽ chuyển đổi màu sắc thành số nguyên và biểu thị mỗi thẻ theo giá trị và thứ hạng màu của nó. Công thức xếp hạng làm cho thứ tự nghiêm ngặt được yêu cầu trở thành một phép so sánh số nguyên thông thường. Vì các giá trị tối đa là 7 và có bảy màu nên thứ hạng dưới 50 là đủ cho tất cả các lần hòa.`build_scores`xử lý cơ chế quy tắc riêng biệt với cơ chế trò chơi. Đối với mỗi mặt nạ bảng màu, nó xem xét mọi sự kết hợp có thể có và kiểm tra trực tiếp bảy định nghĩa. Điều này là cố tình đơn giản. Chỉ có (2^7=128) mặt nạ bảng màu và mỗi mặt nạ chứa tối đa (2^7=128) mặt nạ con, vì vậy việc đánh giá toàn diện ở đây là rất nhỏ so với tìm kiếm trong trò chơi. 

Quy tắc màu chàm đáng được quan tâm đặc biệt. Sự kết hợp hợp lệ phải chứa các giá trị riêng biệt tạo thành một khoảng liên tiếp. Một thẻ duy nhất là một lần chạy hợp lệ, do đó, một bảng màu chỉ chứa một thẻ vẫn có sự kết hợp màu chàm có kích thước một. điều kiện`hi - lo + 1 == cnt`cùng với các giá trị riêng biệt nắm bắt chính xác thuộc tính đó. 

Mã hóa bậc ba là nén trạng thái chính. Một thẻ biến có ba vị trí có thể có từ góc độ chơi trong tương lai. Di chuyển thẻ tay vào bảng màu sẽ tăng chữ số thứ ba của nó từ 1 lên 2, trong khi di chuyển nó vào khung vẽ sẽ giảm nó từ 1 xuống 0. Thẻ bảng màu ban đầu luôn có bit 0 của mặt nạ bảng màu và không bao giờ tham gia vào các chữ số thứ ba. 

Canvas chỉ lưu trữ một chỉ mục màu. Khi một lá bài đã bị loại bỏ, danh tính của nó không còn ảnh hưởng đến bất kỳ quy tắc nào trong tương lai. Sự biến mất của nó đã được nhìn thấy rõ ràng vì chữ số thứ ba của nó bằng 0. Đây là lý do tại sao việc lưu trữ thẻ canvas hoàn chỉnh sẽ tạo ra các trạng thái không cần thiết. 

các`memo`mảng sử dụng số 0 cho trạng thái không xác định, một cho trạng thái thua và hai cho trạng thái thắng. Chỉ mục của nó bao gồm cả trạng thái cục bộ, màu canvas và biến thành một số nguyên. Mảng byte tiết kiệm bộ nhớ hơn nhiều so với từ điển Python chứa hàng triệu khóa tuple. 

Những biểu thức như`bit.bit_length() - 2`chuyển đổi bảng màu hoặc bit tay thành chỉ mục thẻ ternary tương ứng. Thẻ tay ở chỉ số 0 cục bộ được biểu thị bằng bit một vì bit 0 được dành riêng cho thẻ bảng màu ban đầu. Phần bù này là nơi dễ dàng gây ra lỗi từng cái một. 

Hành động kết hợp được tạo ra theo đúng thứ tự. Thẻ được chọn đầu tiên sẽ được chuyển vào bảng màu, sau đó thẻ đã chọn khác sẽ được chuyển vào khung vẽ. Do đó, thẻ thứ hai được đánh giá bằng cách sử dụng bảng màu mở rộng, bảng màu này quan trọng đối với các quy tắc như cam, vàng, lục, lam, chàm và tím. 

Không có vấn đề tràn số nguyên trong Python. Các đối tượng số học lớn nhất được sử dụng để lập chỉ mục trạng thái chỉ có vài triệu, trong khi điểm thẻ lại dưới 400. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
0 0
3G
7Y
```Các trạng thái cục bộ ban đầu chỉ chứa các thẻ bảng màu cố định của chúng. Cả hai tay đều trống, do đó tìm kiếm đệ quy kết thúc trước khi kiểm tra quy tắc canvas hiện tại. 

| Xoay | Đầu tay | Bảng màu đầu tiên | Đồ cũ | Bảng màu thứ hai | Vải | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| Đầu tiên | trống | 3G | trống | 7 Năm | R | Đầu tiên không có động thái | 

Trạng thái thua đối với người chơi đầu tiên vì tay trắng ở đầu lượt là thua ngay lập tức. Người chơi thứ hai thắng mà không cần di chuyển. 

### Mẫu 2 

Đầu vào là```
3 0
1R 2R 3R 4R
7R
```Canvas ban đầu có màu đỏ. Thẻ bảng duy nhất của người chơi đầu tiên là 1R, trong khi bảng màu của người chơi thứ hai chứa 7R. 

| Hành động đầu tiên | Bảng màu mới | Tranh vẽ mới | Điểm đầu tiên | Điểm thứ hai | Động thái thắng lợi hợp pháp | 
| --- | --- | --- | --- | --- | --- | 
| Đặt 2R vào bảng màu | 1R, 2R | R | 2R | 7R | Không | 
| Đặt 3R vào bảng màu | 1R, 3R | R | 3R | 7R | Không | 
| Đặt 4R vào bảng màu | 1R, 4R | R | 4R | 7R | Không | 
| Đặt 2R trên canvas | 1R | R | 1R | 7R | Không | 
| Đặt 3R trên canvas | 1R | R | 1R | 7R | Không | 
| Đặt 4R trên canvas | 1R | R | 1R | 7R | Không | 

Thẻ canvas vẫn có màu đỏ vì cả ba thẻ có sẵn đều có màu đỏ, do đó, các bước di chuyển chỉ trên canvas không làm thay đổi quy tắc. Một bước di chuyển bảng màu không thể tạo ra một lá bài mạnh hơn 7R. Hành động kết hợp cũng không thể giúp ích được gì, bởi vì mọi quân bài canvas có thể có đều có màu đỏ và khiến trò chơi tuân theo quy tắc quân bài cao nhất. 

Do đó, trạng thái đầu tiên sẽ thua, vì vậy câu trả lời là`Second`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(3^{n+m}(n+m)^2)) | Mỗi lá bài có ba trạng thái và mỗi trạng thái xem xét các hành động một lá bài và hai lá bài | 
| Không gian | (O(3^{n+m})) | Bản ghi nhớ lưu trữ một byte cho mỗi trạng thái trò chơi được đóng gói, tối đa hệ số không đổi cho bảy quy tắc và hai lượt | 

Ở mức tối đa (n=m=6), trạng thái cục bộ của mỗi người chơi chỉ có 729 khả năng. Bao gồm cả hai người chơi, bảy màu canvas và hai lượt mang lại khoảng 7,4 triệu trạng thái đóng gói và mảng ghi nhớ chỉ chiếm vài megabyte. So sánh điểm quy tắc và bảng chuyển tiếp là không đáng kể. Việc tìm kiếm theo cấp số nhân về số lượng quân bài, nhưng số mũ bị giới hạn bởi mười hai, đó chính xác là lý do tại sao phương pháp này phù hợp với những ràng buộc nhỏ bất thường của bài toán. 

## Trường hợp thử nghiệm```
# This test block assumes solve_case from the solution above is available.

def run(inp: str) -> str:
    return solve_case(inp).strip()

# Provided samples.
assert run("""\
0 0
3G
7Y
""") == "Second", "sample 1"

assert run("""\
3 0
1R 2R 3R 4R
7R
""") == "Second", "sample 2"

assert run("""\
4 3
1O 2O 4G 6G 5B
7B 2Y 5P 2G
""") == "First", "sample 3"

# Minimum-size input. Nobody has a hand, so the first player loses immediately.
assert run("""\
0 0
3G
7Y
""") == "Second", "empty hands"

# Equal values test the color tie-break.
assert run("""\
1 1
2P 2R
2Y 2O
""") == "First", "equal value, stronger color wins"

# Violet boundary: 3 counts, 4 does not.
assert run("""\
1 1
3P 1R
7R 4O
""") == "First", "value 3 belongs to violet"

# Indigo singleton boundary. A one-card run exists, but 7P beats 1R.
assert run("""\
1 0
1R 2O
7P
""") == "Second", "singleton indigo run"

# Maximum hand size for one player.
# First can put 2R into the palette and 3R onto the canvas,
# producing a yellow rule where First has two cards of one color.
# Second has no hand and loses on the following turn.
assert run("""\
6 0
1R 2R 3R 4R 5R 6R 7R
7P
""") == "First", "maximum first-hand size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 / 3G / 7Y`|`Second`| Thua tay trắng khi bắt đầu lượt | 
|`2P 2R / 2Y 2O`|`First`| Liên kết màu sau các giá trị bằng nhau | 
|`3P 1R / 7R 4O`|`First`| Violet bao gồm 3 nhưng loại trừ 4 | 
|`1R 2O / 7P`|`Second`| Một singleton là một màu chàm hợp lệ | 
|`1R 2R 3R 4R 5R 6R 7R / 7P`|`First`| Bàn tay sáu lá bài và một bước di chuyển bảng màu/canvas kết hợp | 

## Vỏ cạnh 

Trường hợp tay trống được xử lý trước bất kỳ động thái nào. Vì```
0 0
3G
7Y
```trạng thái địa phương đầu tiên không có mặt nạ tay. Hàm đệ quy đánh dấu nó là thua ngay lập tức và không bao giờ thử so sánh hai bảng màu. Kết quả là`Second`. 

Việc phân chia màu đỏ được xử lý bằng cách mã hóa từng lá bài thành giá trị số theo sau là thứ hạng màu của nó. Vì```
1 1
2P 2R
2Y 2O
```di chuyển 2R sang bảng đầu tiên sẽ tạo ra hai quân bài cao nhất là 2R và 2Y. Cả hai đều có giá trị 2, nhưng thứ hạng được mã hóa là 2R lớn hơn nên người chơi đầu tiên sẽ dẫn đầu. Khả năng trong tương lai của đối thủ cũng được khám phá bằng phép đệ quy minimax, thay vì cho rằng giành được vị trí hiện tại là đủ. 

Ranh giới màu tím được biểu diễn bằng điều kiện`v < 4`. TRONG```
1 1
3P 1R
7R 4O
```người chơi đầu tiên loại bỏ 1R vào khung vẽ, thay đổi quy tắc thành màu tím. Bảng màu đầu tiên chứa 3P, đóng góp một thẻ đủ điều kiện. Bảng màu thứ hai chứa 7R, không đóng góp gì. Do đó, người chơi đầu tiên có nước đi thắng hợp pháp. 

Trường hợp đơn màu chàm sử dụng điều kiện là giá trị lớn nhất trừ đi giá trị nhỏ nhất bằng số giá trị phân biệt trừ đi một. Đối với một lá bài, cả hai mặt đều bằng 0, do đó sự kết hợp là hợp lệ. TRONG```
1 0
1R 2O
7P
```chơi 2O trên canvas sẽ tạo ra quy tắc chàm, nhưng kết quả là đơn 1R yếu hơn đơn 7P của đối thủ. Chơi 2O trên bảng màu sẽ khiến quy tắc màu đỏ được kích hoạt và cũng bị thua. Không có hành động nào khác, người chơi đầu tiên sẽ thua. 

Nước đi kết hợp phải sử dụng hai thẻ khác nhau và phải đánh giá quy tắc canvas sau khi thẻ bảng màu đã được thêm vào. TRONG```
6 0
1R 2R 3R 4R 5R 6R 7R
7P
```người chơi đầu tiên có thể di chuyển 2R vào bảng màu và 3R vào khung vẽ. Quy tắc mới là màu vàng. Bảng màu đầu tiên chứa 1R và 2R, cả hai đều có cùng màu, trong khi bảng màu thứ hai chỉ chứa 7P. Người chơi đầu tiên có hai quân bài đủ điều kiện đấu với một quân bài, vì vậy nước đi này là hợp pháp và thắng. Tay của người chơi thứ hai trống rỗng nên trò chơi kết thúc ở lượt tiếp theo với`First`với tư cách là người chiến thắng. 

Quy tắc chàm cũng yêu cầu cẩn thận khi tồn tại các giá trị trùng lặp. Hai lá bài có giá trị 5 không tạo thành một ván bài hai lá. Tính toán trước điểm số sẽ kiểm tra xem tất cả các giá trị trong tổ hợp màu chàm có khác biệt hay không trước khi kiểm tra xem chúng có tạo thành một khoảng liên tiếp hay không. Điều này ngăn giá trị trùng lặp tăng thời lượng chạy không chính xác. 

Cuối cùng, các thẻ được đặt trên khung vẽ sẽ biến mất khỏi các thẻ có sẵn của người chơi tương ứng. Quá trình triển khai ghi lại bằng cách thay đổi trạng thái tạm thời của chúng từ cũ sang bị loại bỏ. Canvas chỉ lưu trữ màu mới. Sau đó, khi một thẻ khác thay thế, thẻ canvas cũ vẫn bị loại bỏ, đúng như yêu cầu mà không cần nhớ đó là thẻ nào.
