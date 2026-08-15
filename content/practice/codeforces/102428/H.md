---
title: "CF 102428H - Giữ hay tiếp tục?"
description: "Tại mọi thời điểm quyết định, Catelyn có điểm cố định C và tổng lượt chơi tạm thời X. Hoster có điểm vĩnh viễn H. Catelyn phải lựa chọn giữa việc tích lũy tổng lượt hiện tại hoặc tung xúc xắc lại."
date: "2026-08-12T07:18:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102428
codeforces_index: "H"
codeforces_contest_name: "2019-2020 ACM-ICPC Latin American Regional Programming Contest"
rating: 0
weight: 102428
solve_time_s: 175
verified: true
draft: false
---

[CF 102428H - Giữ hay tiếp tục?](https://codeforces.com/problemset/problem/102428/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ở mỗi thời điểm quyết định, Catelyn đều có điểm cố định`C`và tổng lượt tạm thời`X`. Hoster có điểm vĩnh viễn`H`. Catelyn phải lựa chọn giữa việc đánh cược tổng lượt hiện tại hoặc tung xúc xắc lần nữa. Mục tiêu không phải là tối đa hóa điểm số ngay lập tức mà là xác suất để Catelyn cuối cùng đạt chính xác 75 trước Hoster, giả sử cả hai người chơi đều đưa ra quyết định tối ưu. 

Nếu Catelyn giữ vững, điểm của cô ấy sẽ trở thành`C + X`. Nếu đúng là 75 thì cô ấy thắng ngay. Nếu nhỏ hơn 75, Hoster sẽ được lượt tiếp theo. Nếu Catelyn tiếp tục, lượt tung 1 sẽ ngay lập tức kết thúc lượt mà không làm thay đổi điểm cố định. Một cuộn từ 2 đến 6 tăng lên`X`. Nếu tổng số mới sẽ làm`C + X`vượt quá 75 thì lượt đó bị thua thay vì được tính điểm. 

Có tối đa 74 điểm vĩnh viễn phù hợp cho một người chơi trước khi đạt 75 và tổng lượt chơi cũng bị giới hạn bởi khoảng cách là 75. Trang vấn đề chính thức đưa ra giới hạn thời gian 5 giây và giới hạn bộ nhớ 1024 MB. Các giới hạn này rất rộng rãi về mặt tuyệt đối, nhưng trò chơi ngẫu nhiên có sự phụ thuộc theo chu kỳ giữa hai người chơi, do đó việc tìm kiếm đệ quy đơn giản là không đủ. Giá trị mục tiêu nhỏ là 75 là lý do chính khiến chương trình năng động hoàn toàn theo cặp điểm có thể thực hiện được. 

Trường hợp tinh tế đầu tiên là một cú đánh chính xác. Ví dụ,```
1
73 0 2
```phải sản xuất`H`. Giữ điểm chính xác 75 và thắng ngay. Việc thực hiện bất cẩn coi việc giữ chỉ là "chuyển lượt cho đối thủ" sẽ làm mất đi quá trình chuyển đổi chiến thắng này một cách không chính xác. 

Trường hợp tinh tế thứ hai là đạt 74. Hãy xem xét```
1
72 0 2
```Nếu Catelyn giữ nguyên, điểm của cô ấy sẽ trở thành 74. Cô ấy không bao giờ có thể đạt chính xác 75 vì mỗi lần tung không phải 1 sẽ cộng ít nhất 2, vì vậy quyết định này có xác suất chiến thắng cuối cùng bằng 0. Việc thực hiện bất cẩn có thể coi 74 như một điểm chưa hoàn thành thông thường và cho phép cuộn 1 trong tương lai hoặc một số mức tăng nhân tạo nào đó đạt tới 75. 

Trường hợp tinh tế thứ ba là một cú tung bóng làm hỏng lượt. Nếu Catelyn có`C = 70`Và`X = 4`, lượt tung thêm 2 sẽ tạo thành tổng số tạm thời là 6 và điểm vĩnh viễn sẽ là 76. Kết quả đó không ghi được điểm gì và vượt qua lượt chơi. Đó không phải là một trạng thái có điểm 76 và việc coi nó như một trạng thái sẽ làm hỏng sự tái diễn. 

Trường hợp tinh tế thứ tư là sự phụ thuộc mang tính chu kỳ giữa những người chơi. Ngay cả sau khi tất cả các trạng thái rẽ tạm thời được sắp xếp bằng cách giảm dần`X`, giá trị ở đầu lượt của Catelyn phụ thuộc vào giá trị đầu lượt khi Hoster đang chơi. Một DP không tuần hoàn đơn giản không thể giải quyết hai giá trị đó một cách độc lập. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ mở rộng toàn bộ cây trò chơi. Ở mỗi lần đổ có sáu kết quả có thể xảy ra và sau mỗi kết quả không phải là 1 sẽ có hai lựa chọn, giữ hoặc tiếp tục. Trò chơi thậm chí không được đảm bảo sẽ kết thúc sau một số lượt quay cố định, bởi vì người chơi có thể liên tục đổ 1 hoặc đổ mà không thay đổi điểm số của mình. Nếu chúng ta dừng lại một cách giả tạo sau`D`cuộn, riêng số lần cuộn là`6^D`. Vì`D = 20`, đó là về`3.66 × 10^15`rời đi trước khi xem xét lựa chọn chiến lược. Vì vậy, mô phỏng toàn diện không thể đưa ra giải pháp chính xác. 

Một cách tiếp cận có cấu trúc hơn là lập trình động. Cho phép`dp[c][h][x]`là xác suất chiến thắng của Catelyn khi người chơi đến lượt đó ghi được điểm`c`, đối thủ có điểm`h`, và tổng lượt hiện tại là`x`. Khi đã biết xác suất ở lượt tiếp theo của đối thủ, giá trị của mọi tổng tạm thời có thể được tính từ số lớn`x`xuống còn nhỏ`x`, bởi vì tiếp tục chỉ di chuyển`x`trở lên. 

Khó khăn là tình trạng với`x = 0`. Cho phép`A = dp[c][h][0]`. Sau khi giữ hoặc tung 1 thì ván cờ chuyển sang đối thủ nên chúng ta cũng cần`dp[h][c][0]`. Điều này tạo ra một chu kỳ giữa hai trạng thái bắt đầu lượt. 

Nhận xét quan trọng là trò chơi có tổng bằng 0. Bắt đầu từ hai trạng thái điểm số thông thường, không kết thúc, cuối cùng có chính xác một người chơi thắng với xác suất 1, vì vậy`dp[c][h][0] + dp[h][c][0] = 1`. 

Như vậy, thay vì phải đoán hai xác suất chưa biết, ta chỉ cần tìm một giá trị`A`. Nếu tạm đoán xác suất thắng của Catelyn ở đầu lượt là`A`, thì xác suất tương ứng của Hoster là`1 - A`. Với giá trị đó được cố định, tất cả các trạng thái lượt tạm thời của Catelyn có thể được tính toán một cách xác định từ tổng số lượt lớn hơn đến các trạng thái nhỏ hơn. Điều này mang lại một chức năng`F(A)`, Ở đâu`F(A)`là xác suất đạt được bằng cách tung xúc xắc đầu tiên theo những quyết định tối ưu. 

Giá trị thực thỏa mãn`A = F(A)`. 

Vì giá trị thu được từ lượt là đơn điệu so với xác suất đoán được của đối thủ nên chúng ta có thể tìm thấy điểm cố định này bằng tìm kiếm nhị phân. Giới hạn số điểm nhỏ khiến điều này trở nên thiết thực. Chúng tôi xử lý các cặp điểm theo thứ tự giảm dần`c + h`, vì vậy bất cứ khi nào việc giữ di chuyển điểm cố định hiện tại từ`c`ĐẾN`c + x`, trạng thái bắt đầu lượt bắt buộc có tổng điểm lớn hơn rất nhiều và đã được tính toán. 

Điều này mang lại một sự tiến triển rõ ràng từ phương pháp vũ phu đến phương pháp cuối cùng. Lực lượng vũ phu hoạt động vì mọi tương lai có thể xảy ra đều được thể hiện rõ ràng, nhưng cây phát triển theo cấp số nhân và có thể không kết thúc. Lập trình động loại bỏ các cây con lặp lại, trong khi quan sát điểm cố định loại bỏ chu trình hai trạng thái còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(6^D)`cho một`D`-roll cắt ngắn |`O(D)`với DFS | Quá chậm và không chính xác | 
| Tối ưu |`O(75^3 log(1/ε))`|`O(75^2)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định`dp[c][h]`là xác suất để người chơi đến lượt đó thắng khi điểm cố định của họ là`c`và điểm cố định của đối phương là`h`, với một lối rẽ tạm thời trống rỗng. 

Trò chơi có tính đối xứng, vì vậy với mọi cặp không kết thúc chúng ta có`dp[c][h] = 1 - dp[h][c]`. Do đó, điểm bằng nhau có giá trị chính xác`0.5`. 
2. Xử lý tất cả các cặp`(c, h)`theo thứ tự giảm dần`c + h`. 

Giả sử chúng ta đang giải một cặp có tổng số điểm`c + h`. Nếu Catelyn giữ sau khi tích lũy`x > 0`, điểm mới của cô ấy là`c + x`, do đó trạng thái đầu lượt của đối thủ có tổng điểm`h + c + x`, nghĩa là lớn hơn. Trạng thái đó đã được tính toán. 
3. Để đoán cố định`A`vì`dp[c][h]`, sử dụng`1 - A`như xác suất thắng đầu lượt của đối thủ. 

Bây giờ chúng ta biết điều gì sẽ xảy ra bất cứ khi nào lượt hiện tại kết thúc. Lần tung 1 hoặc lần đổ lượt bị hỏng sẽ mang lại cho đối thủ lượt tiếp theo, vì vậy giá trị của nó đối với người chơi hiện tại là`1 - (1 - A) = A`khi trạng thái của đối thủ được thể hiện bằng xác suất chiến thắng của chính nó. Trực tiếp hơn trong việc thực hiện, nếu xác suất chiến thắng của đối thủ là`p`, mọi mất mát ngay lập tức đều có giá trị`1 - p`. 
4. Tính các giá trị lần lượt tạm thời từ tổng giá trị lớn nhất có thể xuống còn 2. 

hãy để`v[x]`là xác suất chiến thắng tối ưu sau khi người chơi hiện tại đã tích lũy chính xác`x`điểm trong lượt. Giữ mang lại cho`1 - dp[h][c+x]`, ngoại trừ khi`c+x = 75`, trong đó việc giữ sẽ thắng ngay lập tức và có giá trị 1. 

Tiếp tục cho kết quả trung bình của sáu kết quả. Cuộn 1 mang lại`1 - p`. Cuộn từ 2 đến 6 hoặc chuyển sang`v[x+d]`khi điểm kết quả không vượt quá 75, hoặc đưa ra`1 - p`khi lượt chuyển tiếp bị hỏng. 

Vì thế,`v[x] = max(hold, continue)`. 

Vì mọi`continue`quá trình chuyển đổi tiến tới tổng số tạm thời lớn hơn, giảm dần`x`làm cho sự tái phát này không theo chu kỳ. 
5. Sau khi tính toán tất cả các trạng thái tạm thời, hãy đánh giá thời điểm bắt đầu lượt. 

Catelyn phải lăn lộn một lần. Kết quả 1 kết thúc lượt chơi ngay. Mỗi kết quả từ 2 đến 6 đều đạt đến một trong các kết quả đã được tính toán`v[d]`tiểu bang hoặc phá sản. Giá trị trung bình của chúng là giá trị được tạo ra bởi dự đoán hiện tại. 
6. Tìm kiếm nhị phân xác suất bắt đầu lượt. 

Hãy để dự đoán hiện tại là`A`. Tính xác suất kết quả`F(A)`. Nếu như`F(A) > A`, dự đoán quá nhỏ, do đó hãy di chuyển giới hạn dưới lên trên. Nếu không thì di chuyển giới hạn trên xuống dưới. 

Năm mươi lần lặp làm giảm khoảng số xuống thấp hơn nhiều so với yêu cầu`10^-5`sự tách biệt giữa hai hành động. 
7. Lưu trữ giá trị kết quả cho cặp điểm. 

Vì`c < h`, lưu trữ giá trị tính toán trong`dp[c][h]`và phần bổ sung của nó trong`dp[h][c]`. Vì`c = h`, cửa hàng`0.5`trực tiếp. 
8. Sau khi có bảng hoàn chỉnh, hãy trả lời từng truy vấn bằng cách xây dựng lại các giá trị lần lượt tạm thời cho`(C, H)`đôi. 

Giá trị giữ là`1`khi`C + X = 75`, nếu không thì là`1 - dp[H][C+X]`. Giá trị tiếp tục là giá trị trung bình trong sáu lần cuộn tiếp theo có thể xảy ra, sử dụng các giá trị lượt quay tạm thời được tính toán trước và coi mỗi lần phá sản là một lượt thua ngay lập tức. đầu ra`H`khi giá trị giữ lớn hơn và`C`nếu không thì. 

Bất biến đằng sau phép tính là khi giải`(c, h)`, mọi trạng thái điểm cố định mà khoản giữ yêu cầu đều có tổng điểm lớn hơn rất nhiều và đã chính xác. Sự phụ thuộc duy nhất chưa được giải quyết là xác suất bắt đầu lượt của đối thủ và mối quan hệ đối xứng làm giảm sự phụ thuộc đó thành một vô hướng. Tìm kiếm nhị phân hội tụ đến điểm cố định duy nhất, do đó các giá trị tạm thời được tính toán từ đó chính xác là giá trị tối ưu cho cặp điểm đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAX_SCORE = 73
TARGET = 75

def solve(data):
    it = iter(data.split())
    q = int(next(it))
    queries = []

    for _ in range(q):
        c = int(next(it))
        h = int(next(it))
        x = int(next(it))
        queries.append((c, h, x))

    # dp[c][h] = probability that the player to move wins
    # with permanent scores c and h and an empty turn.
    #
    # Columns 74 and 75 are useful boundary states:
    # dp[c][74] = 1 for c < 74, because the opponent at 74
    # can never reach exactly 75.
    # dp[c][75] = 0 because the opponent has already won.
    dp = [[0.0] * 76 for _ in range(74)]

    for c in range(74):
        dp[c][74] = 1.0
        dp[c][75] = 0.0

    def turn_value(c, h, opponent_win):
        """
        Compute the optimal value of the current turn when:
          current permanent score = c
          opponent permanent score = h
          opponent's beginning-of-turn win probability = opponent_win

        Returns the beginning-of-turn value for the current player.
        """
        max_x = TARGET - c
        lose_turn = 1.0 - opponent_win

        v = [0.0] * (max_x + 8)
        suffix = [0.0] * (max_x + 9)

        # Reaching exactly 75 means the player can hold and win.
        v[max_x] = 1.0
        suffix[max_x] = 1.0

        row = dp[h]

        for x in range(max_x - 1, 1, -1):
            # Holding scores c + x.
            hold = 1.0 - row[c + x]

            # Continue:
            # roll 1 is always a turn loss.
            # Rolls 2..6 either reach a known state or bust.
            left = x + 2
            right = min(x + 6, max_x)

            if left <= right:
                known = right - left + 1
                future = suffix[left] - suffix[right + 1]
            else:
                known = 0
                future = 0.0

            continue_value = (
                future + (6 - known) * lose_turn
            ) / 6.0

            v[x] = max(hold, continue_value)
            suffix[x] = suffix[x + 1] + v[x]

        # First roll of a new turn.
        left = 2
        right = min(6, max_x)

        if left <= right:
            known = right - left + 1
            future = suffix[left] - suffix[right + 1]
        else:
            known = 0
            future = 0.0

        return (future + (6 - known) * lose_turn) / 6.0

    # Solve score pairs from larger c+h to smaller c+h.
    for total in range(146, -1, -1):
        lo_c = max(0, total - 73)
        hi_c = min(73, total)

        for c in range(lo_c, hi_c + 1):
            h = total - c

            if c > h:
                continue

            if c == h:
                dp[c][h] = 0.5
                continue

            # We solve for A = dp[c][h].
            # The swapped state has value 1-A.
            lo = 0.0
            hi = 1.0

            for _ in range(50):
                mid = (lo + hi) * 0.5

                # If dp[c][h] = mid, then the opponent's
                # beginning-of-turn probability is 1-mid.
                got = turn_value(c, h, 1.0 - mid)

                if got > mid:
                    lo = mid
                else:
                    hi = mid

            a = (lo + hi) * 0.5
            dp[c][h] = a
            dp[h][c] = 1.0 - a

    def turn_states(c, h, opponent_win):
        """
        Same recurrence as turn_value, but keeps all temporary
        turn states because queries need v[X].
        """
        max_x = TARGET - c
        lose_turn = 1.0 - opponent_win

        v = [0.0] * (max_x + 8)
        suffix = [0.0] * (max_x + 9)

        v[max_x] = 1.0
        suffix[max_x] = 1.0

        row = dp[h]

        for x in range(max_x - 1, 1, -1):
            hold = 1.0 - row[c + x]

            left = x + 2
            right = min(x + 6, max_x)

            if left <= right:
                known = right - left + 1
                future = suffix[left] - suffix[right + 1]
            else:
                known = 0
                future = 0.0

            continue_value = (
                future + (6 - known) * lose_turn
            ) / 6.0

            v[x] = max(hold, continue_value)
            suffix[x] = suffix[x + 1] + v[x]

        return v

    answer = []

    for c, h, x in queries:
        opponent_win = dp[h][c]
        v = turn_states(c, h, opponent_win)

        if c + x == TARGET:
            hold = 1.0
        else:
            hold = 1.0 - dp[h][c + x]

        lose_turn = 1.0 - opponent_win

        left = x + 2
        right = min(x + 6, TARGET - c)

        if left <= right:
            known = right - left + 1
            future = sum(v[d] for d in range(left, right + 1))
        else:
            known = 0
            future = 0.0

        continue_value = (
            future + (6 - known) * lose_turn
        ) / 6.0

        answer.append("H" if hold > continue_value else "C")

    return "\n".join(answer)

if __name__ == "__main__":
    data = sys.stdin.buffer.read()
    print(solve(data))
```cái bàn`dp`chỉ lưu trữ xác suất đầu lượt. Tổng số tạm thời được tính theo yêu cầu vì việc giữ mọi`(c, h, x)`giá trị sẽ tăng bộ nhớ mà không giúp tính toán cặp điểm. 

Cột ranh giới 75 đại diện cho một trò chơi đã hoàn thành, vì vậy nó có giá trị bằng 0 đối với người chơi có lượt bắt đầu. Cột 74 tinh tế hơn. Nếu đối thủ có 74 điểm, đối thủ đó không bao giờ có thể ghi chính xác 75, do đó, người chơi có bất kỳ điểm nào dưới 74 cuối cùng sẽ thắng với xác suất 1. Hai giá trị biên này ngăn các trường hợp đặc biệt rò rỉ vào đợt tái phát chính. 

Bên trong`turn_value`,`max_x = 75 - c`là tổng số tiền tạm thời lớn nhất vẫn còn hợp pháp. Chính xác là nhà nước`max_x`có giá trị 1 vì số nắm giữ đạt tới 75. Tổng số lớn hơn không bao giờ xuất hiện dưới dạng trạng thái, bởi vì những kết quả đó là bán thân và ngay lập tức vượt qua lượt chơi. 

các`suffix`mảng là một tối ưu hóa nhỏ. Sự tiếp nối từ`x`cần năm giá trị cho`x+2`bởi vì`x+6`. Vì phép truy hồi được xử lý ngược nên tất cả chúng đều đã được biết. Tổng hậu tố làm giảm phần này từ năm phép cộng cho mỗi trạng thái thành thời gian không đổi. 

Tìm kiếm nhị phân sử dụng 50 lần lặp, chính xác hơn nhiều so với tìm kiếm nhị phân.`10^-5`sự phân biệt được yêu cầu bởi tuyên bố. Số nguyên Python không tràn ở đây và tất cả số học liên quan đến xác suất đều sử dụng dấu phẩy động có độ chính xác kép. 

Đánh giá truy vấn cuối cùng cố tình so sánh trực tiếp hai giá trị hành động thay vì so sánh với một ngưỡng tùy ý. Điều này quan trọng vì quyết định đúng đắn phụ thuộc vào cả tổng lượt chơi hiện tại và phản ứng tối ưu của đối thủ. 

## Ví dụ đã hoạt động 

Mẫu được cung cấp là```
15 0 3
35 50 40
15 0 30
```và đầu ra là```
C
H
H
```Đối với truy vấn đầu tiên, trạng thái liên quan là`(C,H,X) = (15,0,3)`. Quá trình tính toán trước đã xác định mọi trạng thái bắt đầu lượt có tổng điểm lớn hơn`15`. Sau đó, thuật toán sẽ xây dựng lại các giá trị lần lượt tạm thời của Catelyn cho cặp điểm`(15,0)`. 

| Tiểu bang | Ý nghĩa | So sánh quyết định | Kết quả | 
| --- | --- | --- | --- | 
|`(15, 0, 3)`| Catelyn có 15 điểm cố định và 3 điểm ở lượt |`continue_value > hold`|`C`| 
|`(35, 50, 40)`| Việc nắm giữ sẽ khiến Catelyn ghi điểm 75 |`hold = 1`|`H`| 
|`(15, 0, 30)`| Tổng lượt lớn khiến nguy cơ tiếp tục chiếm ưu thế |`hold > continue_value`|`H`| 

Truy vấn thứ hai thực hiện ranh giới mục tiêu chính xác. Catelyn có 35 điểm cố định và 40 điểm ở lượt hiện tại, vì vậy việc giữ sẽ mang lại chính xác 75. Không tính toán xác suất nào có thể cải thiện được chiến thắng ngay lập tức, khiến`H`bị ép. 

Đối với dấu vết thứ hai, hãy xem xét đầu vào tùy chỉnh```
2
73 0 2
72 0 3
```Cả hai truy vấn đều là lượt truy cập chính xác. 

| Truy vấn | C | H | X | C + X | Giữ giá trị | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 73 | 0 | 2 | 75 | 1 |`H`| 
| 2 | 72 | 0 | 3 | 75 | 1 |`H`| 

Truy vấn đầu tiên cũng đưa ra tổng số tạm thời nhỏ nhất có thể giành chiến thắng ngay lập tức. Thứ hai xác nhận rằng việc thực hiện sử dụng`C + X == 75`, chứ không phải là một điều kiện riêng biệt như`C + X >= 75`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(75^3 log(1/ε))`| có`O(75^2)`cặp điểm, mỗi cặp sử dụng tìm kiếm nhị phân với`O(log(1/ε))`lần lặp và mỗi lần lặp lại tính toán`O(75)`trạng thái tạm thời | 
| Không gian |`O(75^2)`| Bảng DP điểm cố định chỉ chứa các giá trị đầu lượt | 

Với 50 lần lặp tìm kiếm nhị phân, độ chính xác về mặt số học chặt chẽ hơn nhiều so với yêu cầu`10^-5`. Kích thước lớn nhất chỉ là 75, do đó số cặp điểm bậc hai và tính toán trạng thái tạm thời tuyến tính phù hợp thoải mái trong giới hạn 5 giây và 1024 MB chính thức. 

## Trường hợp thử nghiệm```python
import io
import sys

def solve(inp: str) -> str:
    data = inp.encode()
    it = iter(data.split())

    q = int(next(it))
    queries = []

    for _ in range(q):
        c = int(next(it))
        h = int(next(it))
        x = int(next(it))
        queries.append((c, h, x))

    TARGET = 75

    dp = [[0.0] * 76 for _ in range(74)]

    for c in range(74):
        dp[c][74] = 1.0
        dp[c][75] = 0.0

    def turn_value(c, h, opponent_win):
        max_x = TARGET - c
        lose_turn = 1.0 - opponent_win

        v = [0.0] * (max_x + 8)
        suffix = [0.0] * (max_x + 9)

        v[max_x] = 1.0
        suffix[max_x] = 1.0

        row = dp[h]

        for x in range(max_x - 1, 1, -1):
            hold = 1.0 - row[c + x]

            left = x + 2
            right = min(x + 6, max_x)

            if left <= right:
                known = right - left + 1
                future = suffix[left] - suffix[right + 1]
            else:
                known = 0
                future = 0.0

            cont = (future + (6 - known) * lose_turn) / 6.0
            v[x] = max(hold, cont)
            suffix[x] = suffix[x + 1] + v[x]

        left = 2
        right = min(6, max_x)

        if left <= right:
            known = right - left + 1
            future = suffix[left] - suffix[right + 1]
        else:
            known = 0
            future = 0.0

        return (future + (6 - known) * lose_turn) / 6.0

    for total in range(146, -1, -1):
        lo_c = max(0, total - 73)
        hi_c = min(73, total)

        for c in range(lo_c, hi_c + 1):
            h = total - c

            if c > h:
                continue

            if c == h:
                dp[c][h] = 0.5
                continue

            lo = 0.0
            hi = 1.0

            for _ in range(50):
                mid = (lo + hi) * 0.5
                got = turn_value(c, h, 1.0 - mid)

                if got > mid:
                    lo = mid
                else:
                    hi = mid

            a = (lo + hi) * 0.5
            dp[c][h] = a
            dp[h][c] = 1.0 - a

    def turn_states(c, h, opponent_win):
        max_x = TARGET - c
        lose_turn = 1.0 - opponent_win

        v = [0.0] * (max_x + 8)
        suffix = [0.0] * (max_x + 9)

        v[max_x] = 1.0
        suffix[max_x] = 1.0

        row = dp[h]

        for x in range(max_x - 1, 1, -1):
            hold = 1.0 - row[c + x]

            left = x + 2
            right = min(x + 6, max_x)

            if left <= right:
                known = right - left + 1
                future = suffix[left] - suffix[right + 1]
            else:
                known = 0
                future = 0.0

            cont = (future + (6 - known) * lose_turn) / 6.0
            v[x] = max(hold, cont)
            suffix[x] = suffix[x + 1] + v[x]

        return v

    ans = []

    for c, h, x in queries:
        opponent_win = dp[h][c]
        v = turn_states(c, h, opponent_win)

        if c + x == TARGET:
            hold = 1.0
        else:
            hold = 1.0 - dp[h][c + x]

        lose_turn = 1.0 - opponent_win

        left = x + 2
        right = min(x + 6, TARGET - c)

        if left <= right:
            known = right - left + 1
            future = sum(v[d] for d in range(left, right + 1))
        else:
            known = 0
            future = 0.0

        cont = (future + (6 - known) * lose_turn) / 6.0

        ans.append("H" if hold > cont else "C")

    return "\n".join(ans)

def run(inp: str) -> str:
    return solve(inp)

# Provided sample
assert run(
    """3
15 0 3
35 50 40
15 0 30
"""
) == "C\nH\nH", "sample 1"

# Minimum-size input and exact-hit boundary
assert run(
    """1
73 0 2
"""
) == "H", "minimum query, exact 75"

# Off-by-one boundary: 72 + 3 is exactly 75
assert run(
    """2
72 0 3
73 0 2
"""
) == "H\nH", "exact-hit boundaries"

# Equal permanent scores, including the maximum allowed input scores
assert run(
    """2
73 73 2
73 73 2
"""
) == "H\nH", "equal scores and maximum scores"

# Maximum Q
big_input = "1000\n" + "73 73 2\n" * 1000
assert run(big_input) == "\n".join(["H"] * 1000), "maximum Q"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 15 0 3 / 35 50 40 / 15 0 30`|`C H H`| Cung cấp mẫu và cả hai hành động | 
|`1 / 73 0 2`|`H`| Truy vấn tối thiểu và giành chiến thắng ngay lập tức | 
|`2 / 72 0 3 / 73 0 2`|`H H`| Chính xác 75 ranh giới và xử lý từng cái một | 
|`2 / 73 73 2 / 73 73 2`|`H H`| Điểm bằng nhau và giá trị điểm tối đa | 
| 1000 bản`73 73 2`| 1000 dòng`H`| Số lượng truy vấn tối đa và xử lý trạng thái lặp lại | 

## Vỏ cạnh 

Để giành chiến thắng chính xác ngay lập tức, đầu vào```
1
73 0 2
```có`C + X = 75`. Trình đánh giá truy vấn lấy nhánh truy cập chính xác và đặt giá trị giữ thành 1 mà không tham khảo trạng thái DP khác. Tiếp tục không thể cải thiện xác suất 1, vì vậy câu trả lời là`H`. 

Đối với điểm không thể đạt được là 74, hãy xem xét việc chuyển đổi từ```
1
72 0 2
```Giữ tạo ra điểm vĩnh viễn 74. Trạng thái đối thủ tương ứng là`dp[0][74] = 1`, nghĩa là đối thủ cuối cùng sẽ thắng vì người chơi ở 74 không bao giờ có thể đạt chính xác 75. Do đó, giá trị giữ là`1 - 1 = 0`. Ranh giới này được thể hiện rõ ràng trong bảng DP, vì vậy 74 không bao giờ vô tình được coi là tiền thân hợp lệ của 75. 

Đối với một bức tượng bán thân, giả sử trạng thái hiện tại có`C = 70`Và`X = 4`. Một cuộn 6 làm cho tổng số điểm tạm thời là 10 và số điểm vĩnh viễn sẽ là 80. Sự lặp lại không truy cập được`v[10]`, vì trạng thái đó nằm ngoài phạm vi pháp lý. Thay vào đó, nó góp phần làm mất đi lượt hiện tại,`1 - opponent_win`. Cách xử lý tương tự áp dụng cho mọi cuộn vượt quá 75. 

Đối với điểm bằng nhau, trạng thái là đối xứng. Nếu cả hai người chơi có cùng số điểm cố định và đây là lúc bắt đầu một lượt, việc hoán đổi danh tính của họ không thay đổi gì. Do đó, mỗi người chơi có xác suất chiến thắng là 0,5. Việc triển khai sử dụng tính đối xứng chính xác này thay vì chạy tìm kiếm nhị phân cho trạng thái đã biết. 

Đối với sự phụ thuộc tuần hoàn, khi giải`(c,h)`, tìm kiếm nhị phân không bao giờ cố gắng giải quyết đệ quy`(h,c)`. Nó sử dụng danh tính tổng bằng không`dp[h][c] = 1 - dp[c][h]`và mọi phần phụ thuộc khác được tạo bằng cách giữ đều có tổng điểm lớn hơn. Đây là thứ chuyển đổi trò chơi từ một quá trình ngẫu nhiên theo chu kỳ thành một chuỗi các phép tính điểm cố định một chiều.
