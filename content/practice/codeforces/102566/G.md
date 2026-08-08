---
title: "CF 102566G - PokerStars"
description: "Chúng ta sẽ có trạng thái bàn poker sau khi mỗi người chơi nhận được năm lá bài riêng. Những quân bài chưa biết duy nhất là hai quân bài chung trên bàn. Nhiệm vụ là tính toán cho mỗi người chơi xác suất để họ trở thành người chiến thắng sau khi hai lá bài đó được tiết lộ."
date: "2026-08-06T21:00:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102566
codeforces_index: "G"
codeforces_contest_name: "AGM 2020, Qualification Round"
rating: 0
weight: 102566
solve_time_s: 96
verified: true
draft: false
---

[CF 102566G - PokerStars](https://codeforces.com/problemset/problem/102566/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta sẽ có trạng thái bàn poker sau khi mỗi người chơi nhận được năm lá bài riêng. Những quân bài chưa biết duy nhất là hai quân bài chung trên bàn. Nhiệm vụ là tính toán cho mỗi người chơi xác suất để họ trở thành người chiến thắng sau khi hai lá bài đó được tiết lộ. 

Một trường hợp thử nghiệm cho biết số lượng người chơi và sau đó là năm lá bài của mỗi người chơi. Các lá bài còn lại tạo thành bộ bài từ đó hai lá bài chung được chọn. Đối với mỗi cặp bài còn lại có thể có, chúng tôi xác định ván bài poker năm lá mạnh nhất mà mỗi người chơi có thể kiếm được từ bảy lá bài có sẵn của họ, quyết định người chiến thắng bằng cách sử dụng quy tắc đặt hàng bài poker và đếm tần suất mỗi người chơi thắng. Câu trả lời là số lần thắng của mỗi người chơi chia cho số cặp thẻ có thể có trên bàn, được in theo modulo số nguyên tố đã cho. 

Hạn chế quan trọng được ẩn giấu trong thiết kế trò chơi. Luôn chỉ có hai thẻ chưa biết. Ngay cả khi mọi người chơi đều được xem xét, tổng số bàn có thể chọn tối đa là hai lá bài từ bộ bài 52 lá, tức là chỉ có 1326 khả năng. Điều này có nghĩa là phần tốn kém không phải là liệt kê kết quả mà là đánh giá các ván bài poker một cách chính xác. Một giải pháp cố gắng mô phỏng tất cả các giao dịch trong tương lai của bộ bài sẽ không cần thiết và quá lớn, trong khi việc liệt kê trực tiếp tất cả các bảng có thể có lại có thể dễ dàng quản lý được. 

Những phần khó khăn chủ yếu nằm ở việc so sánh poker. Quân Át thấp như A,2,3,4,5 phải được coi là có quân bài cao 5 chứ không phải quân Át. So sánh tuôn ra sẽ bỏ qua chất và so sánh thứ hạng của quân bài đã được sắp xếp. Một người chơi có thể có một số tay bài năm lá bài trong bảy lá bài của họ, vì vậy chỉ đánh giá năm lá bài đầu tiên hoặc chọn lá bài tham lam sẽ đưa ra câu trả lời sai. 

Ví dụ: nếu người chơi có quân A, quân K, quân Q, quân J, 10 quân và bàn cờ có 9 quân và 2 quân thì ván bài đúng chỉ là quân bài tuôn ra chứ không phải quân bài. Việc thực hiện bất cẩn mà chỉ kiểm tra thứ hạng mà quên rằng cả năm lá bài phải có cùng chất sẽ đánh giá quá cao người chơi. 

Một sai lầm phổ biến khác là bỏ qua quy tắc hòa cuối cùng. Nếu hai người chơi có bài poker giống hệt nhau, người chơi có chỉ số thấp hơn sẽ thắng. Đối với trường hợp thử nghiệm trong đó hai người chơi nhận được sức mạnh tay giống nhau sau mỗi ván có thể, người chơi đầu tiên phải nhận được xác suất 1 và xác suất thứ hai là 0. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là thử mọi cặp thẻ cộng đồng có thể. Đối với mỗi cặp, thêm hai quân bài vào tay mỗi người chơi, kiểm tra tất cả năm lựa chọn quân bài có thể có từ bảy quân bài thu được và giữ lại quân bài mạnh nhất. Điều này đúng vì phần chưa biết của trò chơi chỉ bao gồm hai lá bài đó. 

Số lượng bảng có thể có nhiều nhất là 1326. Mỗi người chơi yêu cầu kiểm tra 21 tập hợp con năm lá bài có thể có, bởi vì bảy lá bài chứa chính xác các lựa chọn C(7,5). Vì 21 là một hằng số nhỏ nên việc đánh giá toàn diện là đủ nhanh. 

Cái nhìn sâu sắc chính là không gian xác suất là rất nhỏ. Bản thân trò chơi poker có vẻ phức tạp vì hệ thống phân cấp bài, nhưng số lượng trạng thái trong tương lai là rất ít. Chúng ta không cần các công thức xác suất nâng cao, mô phỏng hoặc lập trình động. Chúng tôi chỉ cần một người đánh giá poker đáng tin cậy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C(52,2) * N * 21) | O(N) | Được chấp nhận sau khi tối ưu hóa đánh giá bài | 
| Tối ưu | O(C(52,2) * N * 21) | O(N) | Đã chấp nhận | 

Lực lượng vũ phu và cách tiếp cận tối ưu là cùng một ý tưởng liệt kê. Việc tối ưu hóa nhận ra rằng điều này đã đủ nhỏ và tập trung nỗ lực vào các yếu tố không đổi và tính chính xác. 

## Hướng dẫn thuật toán

1. Chuyển đổi mỗi thẻ thành giá trị số cho cấp bậc và chất. Lưu trữ năm thẻ riêng của mỗi người chơi và đánh dấu tất cả các thẻ đã sử dụng. 
2. Tạo danh sách các thẻ còn lại. Mỗi cặp có thể có trong danh sách này đại diện cho một trạng thái bảng có khả năng xảy ra như nhau trong tương lai. 
3. Đối với mỗi cặp bàn có thể, hãy đánh giá từng người chơi. Kết hợp năm quân bài riêng của họ với hai quân bài trên bàn và liệt kê tất cả 21 tay bài có thể có năm quân bài. Giữ ván bài tốt nhất theo xếp hạng poker. 
4. So sánh ván bài tốt nhất của tất cả người chơi ở trạng thái bàn này. Người chơi có ván bài cao nhất sẽ thắng. Nếu nhiều người chơi có giá trị bài bằng nhau, hãy chọn chỉ số nhỏ nhất. 
5. Thêm một chiến thắng vào quầy của người chơi đã chọn. Sau khi tất cả các cặp bảng được xử lý, nhân mỗi bộ đếm với nghịch đảo mô đun của số cặp có thể có. 

Tại sao nó hoạt động: mọi trạng thái trò chơi cuối cùng có thể có được thể hiện chính xác một lần vì mọi cặp thẻ còn lại có thể có đều được liệt kê. Người đánh giá ván bài trả về thứ tự giống như quy tắc poker vì mỗi ván bài được chuyển đổi thành một danh mục và có giá trị hòa. Vì người chiến thắng ở mọi trạng thái có thể đều được tính nên tỷ lệ cuối cùng chính xác là xác suất cần thiết. 

## Giải pháp Python```python
import sys
from itertools import combinations

input = sys.stdin.readline

MOD = 100055128505716009

rank_map = {
    "2": 2, "3": 3, "4": 4, "5": 5, "6": 6,
    "7": 7, "8": 8, "9": 9, "10": 10,
    "J": 11, "Q": 12, "K": 13, "A": 14
}

suits = {
    "clubs": 0,
    "diamonds": 1,
    "hearts": 2,
    "spades": 3
}

def evaluate_five(cards):
    ranks = sorted([x[0] for x in cards], reverse=True)
    cnt = {}
    for r in ranks:
        cnt[r] = cnt.get(r, 0) + 1

    groups = sorted(((c, r) for r, c in cnt.items()), reverse=True)

    flush = len({x[1] for x in cards}) == 1

    unique = sorted(set(ranks))
    straight = False
    high = 0
    if len(unique) == 5:
        if unique == [2, 3, 4, 5, 14]:
            straight = True
            high = 5
        elif unique[-1] - unique[0] == 4:
            straight = True
            high = unique[-1]

    if straight and flush:
        return (8, high)

    if groups[0][0] == 4:
        return (7, groups[0][1], groups[1][1])

    if groups[0][0] == 3 and groups[1][0] == 2:
        return (6, groups[0][1], groups[1][1])

    if flush:
        return (5, *ranks)

    if straight:
        return (4, high)

    if groups[0][0] == 3:
        kickers = sorted([r for r in ranks if r != groups[0][1]], reverse=True)
        return (3, groups[0][1], *kickers)

    if groups[0][0] == 2 and groups[1][0] == 2:
        pairs = sorted([groups[0][1], groups[1][1]], reverse=True)
        return (2, pairs[0], pairs[1], groups[2][1])

    if groups[0][0] == 2:
        kickers = sorted([r for r in ranks if r != groups[0][1]], reverse=True)
        return (1, groups[0][1], *kickers)

    return (0, *ranks)

def evaluate_seven(cards):
    best = None
    for comb in combinations(cards, 5):
        cur = evaluate_five(comb)
        if best is None or cur > best:
            best = cur
    return best

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        players = []
        used = set()

        for _ in range(n):
            hand = []
            for _ in range(5):
                r, s = input().split()
                card = (rank_map[r], suits[s])
                hand.append(card)
                used.add(card)
            players.append(hand)

        deck = []
        for r in range(2, 15):
            for s in range(4):
                if (r, s) not in used:
                    deck.append((r, s))

        wins = [0] * n
        total = 0

        for a, b in combinations(deck, 2):
            total += 1
            board = [a, b]
            best = None
            winner = -1

            for i in range(n):
                cur = evaluate_seven(players[i] + board)
                if best is None or cur > best:
                    best = cur
                    winner = i

            wins[winner] += 1

        inv = pow(total, MOD - 2, MOD)
        ans.append(" ".join(str(x * inv % MOD) for x in wins))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Mã hóa thẻ biến thứ hạng và chất thành số nguyên, giúp cho việc so sánh trở nên đơn giản và tránh việc xử lý chuỗi trong phần tốn kém của thuật toán. 

chức năng`evaluate_five`tuân theo hệ thống phân cấp poker trực tiếp. Bộ dữ liệu được trả về bắt đầu với danh mục bàn tay, do đó danh mục mạnh hơn sẽ tự động so sánh lớn hơn. Các mục trong bộ còn lại được sắp xếp theo thứ tự các bộ ngắt kết nối, phù hợp với các quy tắc cho danh mục đó. 

Người đánh giá bảy thẻ sẽ kiểm tra mọi lựa chọn năm thẻ có thể. Chỉ có 21 thẻ trong số đó, vì vậy phương pháp trực tiếp này rõ ràng và an toàn hơn so với việc cố gắng xây dựng logic dựa trên trường hợp phức tạp cho bảy thẻ. 

Nghịch đảo mô đun được tính bằng định lý Fermat vì mô đun là số nguyên tố. Số nguyên Python không bị tràn nên phép nhân với nghịch đảo là an toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(C(R,2) * N * 21 * 5) | R là số quân bài còn lại, nhiều nhất là 52 | 
| Không gian | O(N) | Lưu trữ người chơi và danh sách thẻ tạm thời | 

Số lượng bảng lớn nhất có thể là 1326, vì vậy ngay cả khi có nhiều người chơi, thuật toán vẫn nằm trong giới hạn đã định. Việc sử dụng bộ nhớ bị chi phối bởi việc lưu trữ các đầu vào. 

## Ví dụ đã hoạt động 

Đối với trường hợp chơi đơn đơn giản: 

đầu vào:```
1
1
A hearts
K hearts
Q hearts
J hearts
10 hearts
```Dấu vết là: 

| Bảng đã được kiểm tra | Tay người chơi | Hạng mục hay nhất | Thắng | 
| --- | --- | --- | --- | 
| bảng đầu tiên | đã có hoàng gia tuôn ra | xả thẳng | 1 | 
| tất cả các bảng | người chiến thắng không thay đổi | xả thẳng | tất cả | 

Người chơi luôn thắng vì chỉ có một người tham gia. Xác suất là 1. 

Dành cho hai người chơi: 

| Bảng đã được kiểm tra | Người chơi 1 | Người chơi 2 | Người chiến thắng | 
| --- | --- | --- | --- | 
| bảng 1 | cặp át chủ bài | thẻ cao | Người chơi 1 | 
| bảng 2 | cặp át chủ bài | cặp vua | Người chơi 1 | 
| tất cả các bảng | so sánh bình thường | so sánh bình thường | tính | 

Điều này chứng tỏ rằng thuật toán không bao giờ cho rằng chỉ những quân bài riêng mới quyết định người chiến thắng. Mỗi cặp thẻ bảng có thể có đều có thể thay đổi kết quả. 

## Trường hợp thử nghiệm```
# These tests illustrate the expected behavior of the algorithm.
# They are intended for use with the solve() function.

sample = """1
4
2 clubs
4 diamonds
7 hearts
J spades
Q clubs
2 diamonds
4 hearts
7 spades
J clubs
Q diamonds
2 hearts
4 spades
7 clubs
J diamonds
Q hearts
2 spades
4 clubs
7 diamonds
J hearts
Q spades
"""

# Expected output:
# 1 0 0 0

single = """1
1
A hearts
K hearts
Q hearts
J hearts
10 hearts
"""

# Expected output:
# 1
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Người chơi đơn với Royal Flush | 1 | Xử lý số lượng người chơi tối thiểu | 
| Mẫu bốn người chơi | 1 0 0 0 | Xử lý nhiều người chơi và phá hòa | 

## Vỏ cạnh 

Một quân Át thấp được xử lý bằng cách kiểm tra rõ ràng trình tự A,2,3,4,5. Nếu không có trường hợp đặc biệt này, một ván bài như quân A, 2 gậy, 3 viên kim cương, 4 quân bích, 5 trái tim sẽ nhận nhầm quân Át cao thay vì quân bài cao thẳng năm. 

Sự hòa giữa những người chơi được xử lý sau khi so sánh các bộ bài hoàn chỉnh. Nếu hai người chơi có hạng giống hệt nhau và có điểm hòa, thì việc so sánh sẽ giữ chỉ số trước đó là người chiến thắng. Điều này phù hợp với quy tắc chỉ số người chơi nhỏ nhất sẽ thắng các trận đấu chưa được giải quyết. 

Một người chơi có bảy lá bài có thể có nhiều sự kết hợp mạnh mẽ. Ví dụ: cầm quân A bích, quân A, quân A, gậy K, quân K với một bàn cờ có một quân Át khác sẽ tạo ra cả một nhà cái đầy đủ và khả năng ba đồng loại. Việc liệt kê tất cả năm tập hợp con thẻ đảm bảo rằng toàn bộ ngôi nhà được chọn.
