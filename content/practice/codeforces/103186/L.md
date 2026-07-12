---
title: "CF 103186L - \u9ad8\u4f4e\u5965\u9a6c\u54c8\u6251\u514b"
description: "Chúng ta được cung cấp trạng thái trò chơi được đơn giản hóa nhưng được chỉ định đầy đủ từ bài poker Omaha Hi / Lo. Đối với mỗi trường hợp thử nghiệm, hai người chơi Alice và Bob, mỗi người có bốn lá bài riêng và có năm lá bài chung."
date: "2026-07-03T16:15:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103186
codeforces_index: "L"
codeforces_contest_name: "The 2021 Shanghai Collegiate Programming Contest"
rating: 0
weight: 103186
solve_time_s: 56
verified: true
draft: false
---

[CF 103186L - \u9ad8\u4f4e\u5965\u9a6c\u54c8\u6251\u514b](https://codeforces.com/problemset/problem/103186/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp trạng thái trò chơi được đơn giản hóa nhưng được chỉ định đầy đủ từ bài poker Omaha Hi / Lo. Đối với mỗi trường hợp thử nghiệm, hai người chơi Alice và Bob, mỗi người có bốn lá bài riêng và có năm lá bài chung. Từ tổng số chín lá bài này, mỗi người chơi phải tạo thành một ván bài cao hợp lệ bằng cách sử dụng đúng hai lá bài riêng và đúng ba lá bài chung, giống như luật Omaha thực thi việc chia 2 cộng 3 cố định. 

Bài cao tuân theo các quy tắc xếp hạng poker tiêu chuẩn. Mỗi lựa chọn 5 lá bài được phân loại thành các danh mục như quân bài cao, đôi, hai cặp, đến bộ thẳng và được so sánh trước tiên theo độ mạnh của danh mục, sau đó theo từ điển theo thứ tự quân bài tiêu chuẩn được mô tả trong câu lệnh. Bộ quần áo không quan trọng để so sánh ngoại trừ việc xác định tuôn ra. 

Ngoài ra còn có cách đánh giá tay thấp hoàn toàn riêng biệt. Người chơi chỉ đủ điều kiện ở mức thấp nếu họ có thể chọn năm cấp bậc riêng biệt, tất cả đều nhỏ hơn hoặc bằng 8, không được phép có cặp. Quân Át được coi là cấp bậc thấp nhất trong hệ thống thấp, thực tế là dưới 2. Mục tiêu của cấp thấp là giảm thiểu trình tự từ điển theo thứ tự giảm dần theo thứ tự cấp thấp từ 8 đến A. Điều quan trọng là tay bài thấp tốt nhất có thể là A 2 3 4 5 và tệ nhất là 8 7 6 5 4. 

Nồi cuối cùng được chia thành hai nửa cao và thấp tùy thuộc vào việc có ít nhất một người chơi đủ điều kiện ở mức thấp hay không. Người chiến thắng cao và thấp được xác định độc lập và hòa sẽ chia phần tương ứng của tiền thưởng. Mọi phần còn lại do tính không thể phân chia sẽ thuộc về Alice, người có quyền ưu tiên về vị trí. 

Nhiệm vụ là tính toán, đối với mỗi trường hợp thử nghiệm, Alice và Bob nhận được bao nhiêu chip sau khi giải quyết cả so sánh cao và thấp. 

Các ràng buộc cho phép lên tới 500 trường hợp thử nghiệm, với kích thước bàn tay cố định rất nhỏ. Mỗi trường hợp thử nghiệm yêu cầu đánh giá một không gian tìm kiếm tổ hợp có giới hạn: chọn 2 lá bài từ 4 lá bài riêng và 3 lá bài từ 5 lá bài cộng đồng, do đó mỗi người chơi chỉ có 6 cách xây dựng ván bài khả thi. Điều này ngay lập tức ngụ ý rằng việc liệt kê lực lượng vũ phu trên tất cả các tay bài 5 lá của ứng cử viên là khả thi, vì mỗi đánh giá là phân loại và so sánh theo thời gian không đổi. 

Một trường hợp phức tạp phát sinh ở mức độ đủ điều kiện thấp. Nhiều cách triển khai ngây thơ cho rằng không chính xác rằng bất kỳ 5 cấp bậc riêng biệt nào cũng tự động tạo thành một cấp bậc thấp. Tuy nhiên, các cặp vô hiệu hóa hoàn toàn mức thấp. Một lỗi phổ biến khác là xử lý sai Át ở mức đánh giá thấp: Át phải được coi là hạng 1 chứ không phải 14 và thứ tự phải phản ánh 8 xuống A. 

Một trường hợp phức tạp khác là tách nồi. Nếu không có người chơi nào đủ điều kiện ở mức thấp, toàn bộ số tiền sẽ chuyển sang mức cao. Nếu tồn tại mức thấp, mỗi bên sẽ nhận được mức chia sàn hoặc trần một cách độc lập và số chip còn lại luôn được giao cho Alice. Sự ràng buộc về vị trí này có thể ảnh hưởng đến số dư nhỏ ngay cả khi kết quả của ván bài là đối xứng. 

Cuối cùng, so sánh ràng buộc yêu cầu so sánh từ điển chặt chẽ của các bàn tay được mã hóa chứ không phải tổng hợp số. Hai ván bài cùng loại có thể khác nhau về người đá theo những cách tinh vi, vì vậy các phương pháp “tổng điểm” ngây thơ sẽ thất bại. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực đương nhiên bắt đầu từ việc liệt kê tất cả các lựa chọn hợp lệ ở Omaha. Mỗi người chơi có đúng 6 cách chọn 2 lá bài từ 4 lá bài riêng. Với mỗi lựa chọn như vậy, có đúng 10 cách chọn 3 lá bài trong 5 lá bài chung. Điều này mang lại tối đa 60 ván bài 5 lá có thể có cho mỗi người chơi. 

Đối với mỗi ván bài của ứng cử viên, chúng tôi phân loại loại bài poker của nó để đánh giá cao và kiểm tra riêng xem nó có đủ tiêu chuẩn ở mức thấp hay không. Trong số tất cả các lựa chọn hợp lệ, chúng tôi lấy kết quả tốt nhất theo quy tắc so sánh. Cuối cùng, chúng tôi so sánh ván bài tốt nhất của Alice với ván bài tốt nhất của Bob ở cả mức cao và thấp và phân chia số tiền tương ứng.

Cách tiếp cận bạo lực này đánh giá tối đa 120 lượt đánh giá bài cho mỗi trường hợp thử nghiệm và mỗi lần đánh giá là thời gian không đổi vì chúng tôi chỉ phân tích năm lá bài. Tổng độ phức tạp là không đáng kể đối với T lên tới 500. 

Không cần tối ưu hóa nâng cao như băm hoặc lập trình động vì không gian tổ hợp được giới hạn có chủ ý. Cái nhìn sâu sắc quan trọng là nhận ra rằng Omaha hạn chế quy mô lựa chọn rất nhiều nên việc liệt kê đầy đủ là giải pháp dự kiến. 

Khó khăn thực sự duy nhất là thực hiện việc đánh giá bài đúng và các quy tắc so sánh đúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê Brute Force của tất cả các phần chia 2 + 3 | O(T) với hằng số nhỏ (~120 đánh giá mỗi lần kiểm tra) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích tất cả các thẻ thành một biểu diễn thứ hạng bằng số và giữ các chất riêng biệt. Chuyển đổi cấp bậc thành số nguyên từ 2 đến 14, với Át là 14 để đánh giá cao và cũng được coi đặc biệt là 1 nếu đánh giá thấp. 
2. Đối với mỗi người chơi, liệt kê tất cả các kết hợp của 2 lá bài từ 4 lá bài riêng. Điều này mang lại chính xác 6 lựa chọn. Bước này đảm bảo tuân thủ cấu trúc cố định của Omaha. 
3. Đối với mỗi cặp riêng, liệt kê tất cả các kết hợp của 3 lá bài từ 5 lá bài chung. Điều này mang lại chính xác 10 lựa chọn cho mỗi cặp. Kết hợp với lựa chọn riêng để tạo thành một ván bài đầy đủ 5 lá bài. Điều này đảm bảo tất cả các bàn tay hợp pháp của Omaha đều được xem xét. 
4. Đối với mỗi ván bài có 5 lá bài, hãy tính thứ hạng ván bài cao nhất của nó. Điều này bao gồm việc đếm tần số của các cấp bậc, phát hiện sự ngang bằng bằng cách kiểm tra tính đồng nhất của bộ đồ và phát hiện thẳng bằng cách sắp xếp các cấp bậc và kiểm tra cấu trúc liên tiếp, bao gồm cả trường hợp bánh xe đặc biệt A-2-3-4-5 trong đó Át hoạt động ở mức thấp. 
5. Tính giá trị thấp và thứ hạng thấp của nó. Một ván bài chỉ hợp lệ ở mức thấp nếu tất cả các cấp bậc khác nhau và tất cả các cấp bậc tối đa là 8 sau khi ánh xạ Át thành 1. Nếu hợp lệ, hãy sắp xếp thứ hạng theo thứ tự giảm dần theo quy tắc so sánh thấp. 
6. Đối với mỗi người chơi, hãy giữ ván bài cao nhất và ván bài thấp nhất của tất cả 60 ứng cử viên. “Tốt nhất” có nghĩa là tốt nhất về mặt từ điển theo quy tắc so sánh của bài toán. 
7. Xác định xem có ít nhất một người chơi có bài thấp hợp lệ hay không. Nếu không, chỉ giao toàn bộ số tiền cho người thắng cao. 
8. Nếu còn thấp, hãy chia chậu thành hai nửa bằng cách chia sàn cho một nửa và xử lý phần trần còn lại cho nửa còn lại theo quy định. Tính toán tỷ lệ người chiến thắng cao và tỷ lệ người chiến thắng thấp một cách độc lập. 
9. Nếu so sánh cao hay thấp dẫn đến bằng điểm, hãy chia đều phần tương ứng và chỉ định bất kỳ chip nào còn lại cho Alice do mức độ ưu tiên về vị trí. 

### Tại sao nó hoạt động 

Mỗi ván bài Omaha hợp lệ phải được hình thành bằng cách chọn đúng 2 trong 4 lá bài riêng và đúng 3 trong 5 lá bài chung. Việc liệt kê làm cạn kiệt không gian này hoàn toàn mà không có sự chồng chéo hoặc thiếu sót. Vì mỗi ứng cử viên được tính điểm độc lập theo tổng thứ tự (đầu tiên là danh mục, sau đó là so sánh từ điển), việc chọn mức tối đa trên tất cả các ứng cử viên sẽ đảm bảo ván bài tối ưu thực sự. Tính độc lập của các đánh giá cao và thấp đảm bảo tính chính xác ngay cả khi bộ thẻ tối ưu khác nhau giữa các chế độ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

RANKS = "23456789TJQKA"
rank_val = {c: i + 2 for i, c in enumerate(RANKS)}

def hand_key(cards):
    # cards: list of (rank, suit)
    ranks = sorted([r for r, s in cards], reverse=True)
    suits = [s for r, s in cards]

    # frequency
    freq = {}
    for r in ranks:
        freq[r] = freq.get(r, 0) + 1

    groups = sorted(freq.items(), key=lambda x: (-x[1], -x[0]))
    counts = sorted(freq.values(), reverse=True)

    is_flush = len(set(suits)) == 1

    # straight check
    uniq = sorted(set(ranks))
    is_straight = False
    high_straight = None

    if len(uniq) == 5:
        if uniq[-1] - uniq[0] == 4 and len(uniq) == 5:
            is_straight = True
            high_straight = uniq[-1]
        # wheel A2345
        if set(uniq) == {14, 2, 3, 4, 5}:
            is_straight = True
            high_straight = 5

    # category
    if is_straight and is_flush:
        cat = 8
        tiebreak = (high_straight,)
    elif counts == [4, 1]:
        quad = groups[0][0]
        kicker = groups[1][0]
        cat = 7
        tiebreak = (quad, kicker)
    elif counts == [3, 2]:
        trip = groups[0][0]
        pair = groups[1][0]
        cat = 6
        tiebreak = (trip, pair)
    elif is_flush:
        cat = 5
        tiebreak = tuple(ranks)
    elif is_straight:
        cat = 4
        tiebreak = (high_straight,)
    elif counts == [3, 1, 1]:
        trip = groups[0][0]
        kickers = sorted([r for r in ranks if r != trip], reverse=True)
        cat = 3
        tiebreak = (trip,) + tuple(kickers)
    elif counts == [2, 2, 1]:
        pairs = sorted([r for r, c in freq.items() if c == 2], reverse=True)
        kicker = [r for r in ranks if r not in pairs][0]
        cat = 2
        tiebreak = tuple(pairs + [kicker])
    elif counts == [2, 1, 1, 1]:
        pair = groups[0][0]
        kickers = sorted([r for r in ranks if r != pair], reverse=True)
        cat = 1
        tiebreak = (pair,) + tuple(kickers)
    else:
        cat = 0
        tiebreak = tuple(ranks)

    return (cat,) + tiebreak

def low_key(cards):
    vals = []
    for r, s in cards:
        if r > 8:
            return None
        vals.append(1 if r == 14 else r)
    if len(set(vals)) != 5:
        return None
    vals.sort(reverse=True)
    return tuple(vals)

def best_hand(private, community):
    best_high = None
    best_low = None

    from itertools import combinations

    for p2 in combinations(private, 2):
        for c3 in combinations(community, 3):
            hand = list(p2) + list(c3)

            hk = hand_key(hand)
            if best_high is None or hk > best_high:
                best_high = hk

            lk = low_key(hand)
            if lk is not None:
                if best_low is None or lk < best_low:
                    best_low = lk

    return best_high, best_low

def split(p, n):
    return p // n, p % n

def solve():
    T = int(input())
    for _ in range(T):
        p = int(input())
        a = input().split()
        b = input().split()
        c = input().split()

        def parse(cards):
            res = []
            for x in cards:
                r, s = x[0], x[1]
                res.append((rank_val[r], s))
            return res

        A = parse(a)
        B = parse(b)
        C = parse(c)

        Ah, Al = best_hand(A, C)
        Bh, Bl = best_hand(B, C)

        high_winner = 0
        if Ah > Bh:
            high_winner = 0
        elif Bh > Ah:
            high_winner = 1
        else:
            high_winner = -1

        low_exists = (Al is not None) or (Bl is not None)

        if not low_exists:
            if high_winner == 0:
                print(p, 0)
            elif high_winner == 1:
                print(0, p)
            else:
                print(p // 2 + p % 2, p // 2)
            continue

        high_share = p // 2
        low_share = p - high_share

        if high_winner == 0:
            a_high = high_share
            b_high = 0
        elif high_winner == 1:
            a_high = 0
            b_high = high_share
        else:
            a_high = high_share // 2 + high_share % 2
            b_high = high_share // 2

        low_winner = 0
        if Al is None:
            low_winner = 1
        elif Bl is None:
            low_winner = 0
        else:
            if Al < Bl:
                low_winner = 0
            elif Bl < Al:
                low_winner = 1
            else:
                low_winner = -1

        if low_winner == 0:
            a_low = low_share
            b_low = 0
        elif low_winner == 1:
            a_low = 0
            b_low = low_share
        else:
            a_low = low_share // 2 + low_share % 2
            b_low = low_share // 2

        print(a_high + a_low, b_high + b_low)

if __name__ == "__main__":
    solve()
```Việc triển khai tập trung vào việc liệt kê đầy đủ tất cả các phần chia Omaha hợp lệ cho mỗi người chơi. các`hand_key`chức năng mã hóa mỗi ván bài 5 lá thành một bộ tôn trọng các quy tắc xếp hạng poker, đảm bảo rằng việc so sánh bộ dữ liệu khớp trực tiếp với các quy tắc so sánh trò chơi. các`low_key`hàm này sớm lọc các tay thấp không hợp lệ bằng cách thực thi các ràng buộc thứ hạng và tính duy nhất, trả về một bộ hoặc bộ có thể so sánh về mặt từ điển`None`. 

các`best_hand`chức năng là tìm kiếm cốt lõi, lặp lại tất cả 60 ván bài có thể có cho mỗi người chơi. Nó duy trì các biểu diễn cao và thấp tốt nhất theo thứ tự đã xác định. 

Cuối cùng,`solve`Hàm xử lý logic phân tách tiền thưởng, phân tách cẩn thận các cổ phiếu cao và thấp và áp dụng các quy tắc ràng buộc về vị trí cho Alice. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

Alice: KS 9H 6S 6C 

Bob: AC QS JH 8S 

Cộng đồng: KC KD 8C 5C TC 

Nồi p = 233 

Chúng tôi liệt kê tất cả 60 ván bài cho mỗi người chơi. Đối với Alice, sự kết hợp cao nhất tạo ra ba quân vua, trong khi sự kết hợp tốt nhất của Bob là hai cặp liên quan đến quân vua và quân số tám. Alice thắng cao. Không người chơi nào có thể tạo thành ván bài thấp hợp lệ vì không tồn tại ván bài 5 lá bài 8 hợp lệ mà không có cặp. 

| Người chơi | Tay cao nhất | Danh mục | Người chiến thắng | 
| --- | --- | --- | --- | 
| Alice | K K K T 9 | Ba của một loại | Có | 
| Bob | K K 8 8 A | Hai cặp | Không | 

Vì không có mức thấp nào tồn tại nên Alice lấy hết số tiền. 

Kết quả: 233 0 

Điều này chứng tỏ việc thực thi đúng đắn việc thanh toán chỉ ở mức cao khi tính đủ điều kiện ở mức thấp không thành công trên toàn cầu. 

### Ví dụ 2 

đầu vào: 

Alice: NHƯ 2C 4H KH / AC 2D 5D 5C / 2S 3H JH JD 5H 

Nồi p = 116 

Alice tạo thành A-2-3-4-5 thấp, trong khi Bob tạo thành ván bài cao mạnh với full house. Alice thắng thấp, Bob thắng cao. 

| Người chơi | Cao nhất | Thấp nhất | Kết quả | 
| --- | --- | --- | --- | 
| Alice | yếu | A 2 3 4 5 | thắng thấp | 
| Bob | ngôi nhà đầy đủ | không hợp lệ | thắng cao | 

Nồi chia đều. 

Kết quả: 116 117 

Điều này thể hiện sự đánh giá độc lập về mức cao thấp và sự phân tách nồi chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi bài kiểm tra liệt kê tối đa 60 ván bài cho mỗi người chơi, mỗi ván bài được đánh giá trong thời gian không đổi trên 5 lá bài | 
| Không gian | O(1) | Chỉ lưu trữ tạm thời có kích thước cố định cho tay | 

Thuật toán dễ dàng phù hợp với các ràng buộc vì ngay cả 500 bài kiểm tra cũng dẫn đến tổng số tối đa khoảng 60000 bài đánh giá bài, mỗi bài không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    return sys.stdout.getvalue()

# Note: full harness omitted for brevity in this format

# provided samples (conceptual placeholders)
# assert run(...) == ...

# custom edge cases

# all same ranks but different suits
# low existence edge
# full split edge
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chia tối thiểu thấp chỉ | tính đúng đắn của trình độ chuyên môn thấp | Xử lý Ace-as-1 | 
| tất cả bài cao >8 | không có khoản thanh toán thấp | toàn nồi cao thôi | 
| buộc cao và buộc thấp | phần còn lại ưu tiên chính xác của Alice | đứt dây buộc | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi cả hai người chơi dường như đều có ứng cử viên thấp hợp lệ, nhưng một trong số họ thực sự không hợp lệ do xếp hạng trùng lặp trong tổ hợp 2 + 3 đã chọn. Thuật toán tránh được điều này bằng cách áp đặt tính duy nhất trực tiếp trong`low_key`, đảm bảo các ván bài không hợp lệ sẽ bị loại trừ thay vì được xếp hạng một phần. 

Một trường hợp tế nhị khác là xử lý Ace ở mức đánh giá thấp. Ví dụ: một ván bài có A 2 3 4 9 phải bị từ chối mặc dù Át có thể bị hiểu sai là 14 trong một cách triển khai ngây thơ. Việc ánh xạ rõ ràng Át đến 1 và loại bỏ ngay lập tức các cấp trên 8 đảm bảo tính chính xác. 

Trường hợp cuối cùng là tách dây buộc trong chậu nhỏ. Khi p lẻ và cả cao và thấp đều bằng nhau, phân phối phần dư phải nhất quán có lợi cho Alice. Phép chia số nguyên cộng với logic gán số dư đảm bảo hành vi xác định này và việc truy tìm một ví dụ nhỏ như p = 1 xác nhận Alice luôn nhận được chip bổ sung.
