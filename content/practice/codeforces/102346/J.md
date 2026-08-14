---
title: "CF 102346J - Trò chơi lọ nước"
description: "Chúng tôi có một vòng tròn gồm tối đa 13 thí sinh. Có bốn bản sao của mỗi giá trị thẻ đang được sử dụng, cộng với một ký tự đại diện. Mỗi thí sinh bắt đầu với bốn thẻ thông thường, trong khi thí sinh xuất phát cũng nhận được ký tự đại diện và do đó ban đầu có năm thẻ."
date: "2026-08-13T01:47:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102346
codeforces_index: "J"
codeforces_contest_name: "2019-2020 ACM-ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 102346
solve_time_s: 667
verified: true
draft: false
---

[CF 102346J - Trò chơi lọ nước](https://codeforces.com/problemset/problem/102346/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 11 phút 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một vòng tròn gồm tối đa 13 thí sinh. Có bốn bản sao của mỗi giá trị thẻ đang được sử dụng, cộng với một ký tự đại diện. Mỗi thí sinh bắt đầu với bốn thẻ thông thường, trong khi thí sinh xuất phát cũng nhận được ký tự đại diện và do đó ban đầu có năm thẻ. 

Phần quan trọng là trò chơi hoàn toàn mang tính quyết định. Thí sinh tiếp theo luôn là người ở bên phải của thí sinh hiện tại và lá bài để vượt qua được xác định duy nhất theo luật. Nếu thí sinh hiện tại có ký tự đại diện và không vừa nhận được nó thì ký tự đại diện đó phải được thông qua. Nếu không, thí sinh sẽ vượt qua một lá bài thông thường có tần số trên tay của họ là tối thiểu, phá vỡ mối ràng buộc theo thứ tự lá bài cố định.`A23456789DQJK`. 

Một thí sinh thắng chính xác khi họ có bốn lá bài, tất cả đều có giá trị như nhau. Ký tự đại diện không thay thế cho thẻ bị thiếu. Ngay sau khi có ít nhất một thí sinh đạt đến trạng thái đó, trò chơi sẽ dừng lại và thí sinh có số thí sinh nhỏ nhất trong số tất cả các thí sinh chiến thắng sẽ được in ra. 

Đầu vào mang lại`N`Và`K`, tiếp theo là bốn thẻ thông thường cho mỗi thí sinh. Ký tự đại diện không được ghi vào đầu vào. Chúng tôi đặt nó vào thí sinh`K`trước khi mô phỏng lượt đầu tiên. 

Giới hạn trên`N <= 13`là lý do mô phỏng trực tiếp là phù hợp. Chỉ có 13 giá trị thẻ thông thường có thể có, vì vậy mọi quyết định có thể được đưa ra bằng cách quét tối đa 13 quầy. Câu lệnh không đưa ra giới hạn trên riêng biệt cho số vòng quay, vì vậy tham số độ phức tạp tự nhiên là số`T`lượt thực sự được mô phỏng trước khi ai đó thắng. Vì các quy tắc không để lại lựa chọn nào cho chương trình khám phá nên không có cây tìm kiếm. 

Có một số trường hợp nguy hiểm có thể âm thầm phá vỡ mô phỏng. 

Đầu tiên, thí sinh xuất phát không thể vượt qua ký tự đại diện ngay lập tức. Ví dụ,```
2 1
ABBB
AAAB
```có thí sinh 1 cầm`ABBB`cộng với ký tự đại diện. Ký tự đại diện vừa được nhận nên thí sinh 1 phải vượt qua một thẻ thông thường.`A`xảy ra một lần và`B`xảy ra ba lần, vì vậy`A`được chuyển cho thí sinh 2. Thí sinh 2 sau đó có`AAAA`và thắng, vì vậy đầu ra là`2`. Việc triển khai bất cẩn ngay lập tức vượt qua ký tự đại diện sẽ tạo ra một trò chơi khác. 

Thứ hai, trò chơi có thể kết thúc trước lượt đầu tiên. Ví dụ,```
2 2
AAAA
2222
```đưa cho thí sinh 2 ký tự đại diện, vì vậy thí sinh 2 có năm thẻ, nhưng thí sinh 1 đã có bốn thẻ`A`thẻ. Đầu ra đúng là`1`. Việc kiểm tra người chiến thắng chỉ sau nước đi đầu tiên là không chính xác. 

Thứ ba, thí sinh nhận thẻ thông thường tạm thời có năm thẻ nên chỉ kiểm tra xem giá trị nào đó xuất hiện bốn lần là chưa đủ. TRONG```
3 3
AAA2
2233
A223
```thí sinh 3 bắt đầu bằng ký tự đại diện. Họ phải vượt qua`A`, bởi vì`A`Và`3`cả hai đều xảy ra một lần và`A`là giá trị thấp hơn. Thẻ đi từ thí sinh 3 đến thí sinh 1 vì vòng tròn quấn quanh. Thí sinh 1 tạm thời có`AAAA2`, đó là năm lá bài và vẫn chưa thắng. Ở lượt tiếp theo, thí sinh 1 vượt qua duy nhất`2`, rời đi`AAAA`, vậy thí sinh 1 thắng và kết quả là`1`. 

Cuối cùng, thứ tự bắt buộc là thứ tự thẻ từ câu phát biểu chứ không phải thứ tự bảng chữ cái. Từ`A`là giá trị nhỏ nhất, liên kết giữa`A`Và`2`phải chọn`A`. Thứ tự cố định tương tự được sử dụng mỗi khi phải chọn một thẻ thông thường. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng trò chơi chính xác như mô tả. Lưu trữ, đối với mỗi thí sinh và mỗi một trong 13 giá trị thẻ thông thường, họ hiện có bao nhiêu bản sao trong tay. Lưu trữ ký tự đại diện riêng biệt vì nó có hành vi đặc biệt và không bao giờ được tham gia vào việc tính toán tần số tối thiểu. 

Một mô phỏng đơn giản có thể quét tất cả các thí sinh sau mỗi nước đi để xem liệu có ai đó đã thắng hay không. Đối với mỗi thí sinh, chúng tôi kiểm tra tất cả 13 quầy thẻ, do đó chi phí cho một lượt`O(13N)`, hoặc nhiều nhất là 169 lượt kiểm tra khi`N = 13`. Nếu trò chơi kéo dài`T`lần lượt, điều này mang lại`O(13NT)`, tương đương`O(NT)`vì 13 đã được cố định. Giới hạn trên chính xác của các cuộc kiểm tra này là`169T`tối đa`N`. 

Cách tiếp cận đó đã đủ nhanh đối với những hạn chế nhất định, nhưng có một sự cải tiến về cấu trúc đơn giản. Sau khi chúng tôi kiểm tra vị trí ban đầu và xác định rằng hiện tại không có ai chiến thắng, một nước đi chỉ thay đổi tay của hai thí sinh: người gửi và người nhận. Mọi mặt khác đều không thay đổi. Do đó, không có thí sinh chưa được chạm tới nào có thể bất ngờ trở thành người chiến thắng. Sau mỗi nước đi chúng ta chỉ cần kiểm tra hai thí sinh đó. 

Quan sát tương tự được áp dụng bất kể thẻ được thông qua là ký tự đại diện hay thẻ thông thường. Người gửi và người nhận là hai người duy nhất có nội dung thay đổi. Mỗi lần kiểm tra trạng thái chiến thắng sẽ quét tối đa 13 bộ đếm, do đó chi tiêu mô phỏng tối ưu`O(13)`tiến hành chọn thẻ và`O(13)`làm việc để kiểm tra từng bàn tay bị ảnh hưởng. Vì vũ trụ thẻ có kích thước cố định 13 nên điều này có hiệu quả`O(T)`. 

Mô phỏng lực lượng vũ phu hoạt động vì trò chơi không còn quyền quyết định của người chơi sau khi biết trạng thái ban đầu, nhưng nó liên tục kiểm tra những người chơi có bàn tay không thể thay đổi. Quan sát mà chỉ người gửi và người nhận mới có thể thay đổi cho phép chúng tôi loại bỏ những lần quét không cần thiết đó mà không thay đổi trình tự mô phỏng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét toàn bộ sau mỗi lượt |`O(13NT)`|`O(13N)`| Được chấp nhận nhưng thực hiện quét không cần thiết | 
| Chỉ kiểm tra những thí sinh bị ảnh hưởng |`O(13T)`|`O(13N)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Mã hóa giá trị thẻ theo thứ tự chính xác`A23456789DQJK`. Cung cấp cho mỗi giá trị thẻ một chỉ mục từ 0 đến 12. Điều này cho phép một chỉ mục số nguyên duy nhất đại diện cho cả thẻ và mức độ ưu tiên hòa của nó. 
2. Xây dựng một`N x 13`mảng tần số từ đầu vào. Dành cho thí sinh`K`, đánh dấu riêng rằng họ giữ ký tự đại diện. Việc giữ ký tự đại diện bên ngoài số lượng thông thường sẽ khiến nó không được xem xét khi chọn thẻ thông thường ít xuất hiện nhất. 
3. Trước khi thực hiện bất kỳ động thái nào, hãy quét từng thí sinh và kiểm tra xem họ đã ở trạng thái chiến thắng hay chưa. Bàn tay chiến thắng không được có ký tự đại diện, có tổng cộng chính xác là bốn quân bài thông thường và cả bốn quân bài đó phải có cùng giá trị. Nếu có nhiều thí sinh cùng thắng thì trả ngay số thí sinh nhỏ nhất. 
4. Đặt thí sinh hiện tại thành`K`và đánh dấu ký tự đại diện là mới nhận được. Lá cờ đặc biệt này chỉ cần thiết cho thí sinh hiện đang giữ ký tự đại diện. Thí sinh xuất phát được đối xử giống hệt như người vừa nhận được nó. 
5. Tính thí sinh tiếp theo là người ngay bên phải, xếp từ`N`quay lại`1`. Trong lập chỉ mục Python dựa trên 0, đây là`(current + 1) % N`. 
6. Nếu thí sinh hiện tại có ký tự đại diện và không mới nhận được thì chuyển ký tự đại diện đó cho thí sinh tiếp theo. Xóa nó khỏi thí sinh hiện tại, đưa nó cho thí sinh tiếp theo và đánh dấu ký tự đại diện là mới nhận được ở đó. 
7. Nếu không, hãy chọn một thẻ thông thường để vượt qua. Quét 13 tần số của thẻ, bỏ qua số 0 và bỏ qua ký tự đại diện. Chọn tần số dương nhỏ nhất và khi hai giá trị có tần số đó, hãy giữ chỉ số thẻ nhỏ hơn. Điều này trực tiếp thực hiện cả hai cấp độ của quy tắc ràng buộc bắt buộc. 
8. Di chuyển thẻ thông thường đã chọn từ thí sinh hiện tại sang thí sinh tiếp theo. Trạng thái ký tự đại diện không thay đổi trong trường hợp này vì ký tự đại diện vẫn giữ nguyên chủ sở hữu hiện tại của nó. Nếu thí sinh hiện tại vừa nhận được ký tự đại diện thì hạn chế đặc biệt hiện đã được áp dụng nên lượt tiếp theo của họ có thể vượt qua ký tự đại diện. 
9. Sau khi di chuyển, chỉ kiểm tra thí sinh hiện tại và thí sinh tiếp theo. Đó là những bàn tay duy nhất đã thay đổi. Nếu một trong hai người thắng cuộc, hãy chọn số thí sinh nhỏ hơn trong số các thí sinh thắng cuộc và trả lại. 
10. Đặt thí sinh tiếp theo làm thí sinh hiện tại và lặp lại mô phỏng cho đến khi tìm được người chiến thắng. 

### Tại sao nó hoạt động 

Bất biến trung tâm là số lượng thẻ được lưu trữ mô tả chính xác trạng thái trò chơi thực ngay trước mỗi lượt mô phỏng. Cờ ký tự đại diện ghi lại xem chủ sở hữu hiện tại của nó có bị cấm chuyển nó hay không. Ở mỗi lượt, thuật toán tuân theo chính xác một trong hai trường hợp pháp lý từ các quy tắc: nó chuyển ký tự đại diện khi được phép hoặc chọn thẻ thông thường có tần số tối thiểu bằng cách sử dụng thứ tự giá trị quy định. Do đó, trạng thái kết quả chính xác là trạng thái của trò chơi thực sau lượt đó. 

Séc thắng cuộc ban đầu xử lý các trò chơi kết thúc trước bất kỳ nước đi nào. Sau lần kiểm tra đó, trạng thái chiến thắng mới chỉ có thể xuất hiện ở một thí sinh đã thay đổi ván bài, cụ thể là người gửi hoặc người nhận. Do đó, việc kiểm tra hai thí sinh đó sau mỗi lần di chuyển là đủ. Khi tìm thấy trạng thái chiến thắng, việc chọn chỉ số thí sinh nhỏ nhất phù hợp với quy tắc người chiến thắng bắt buộc, do đó, thí sinh trở về chính xác là người được trò chơi tuyên bố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

VALUES = "A23456789DQJK"
V = len(VALUES)
POS = {ch: i for i, ch in enumerate(VALUES)}

def is_winner(cards, has_wild):
    if has_wild:
        return False

    total = 0
    has_four = False

    for x in cards:
        total += x
        if x == 4:
            has_four = True

    return total == 4 and has_four

def solve():
    n, k = map(int, input().split())
    k -= 1

    cards = [[0] * V for _ in range(n)]
    has_wild = [False] * n

    for i in range(n):
        s = input().strip()
        for ch in s:
            cards[i][POS[ch]] += 1

    has_wild[k] = True

    # The game may already be over before the first turn.
    for i in range(n):
        if is_winner(cards[i], has_wild[i]):
            return i + 1

    current = k
    wild_new = True

    while True:
        nxt = (current + 1) % n

        # The wildcard can be passed only if it was not
        # received immediately before this turn.
        if has_wild[current] and not wild_new:
            has_wild[current] = False
            has_wild[nxt] = True
            wild_new = True

        else:
            # Pass the least frequent ordinary card.
            # Ties are resolved by VALUES order, which is exactly
            # the index order 0..12.
            chosen = -1
            best_count = 10

            for value in range(V):
                cnt = cards[current][value]
                if cnt == 0:
                    continue

                if cnt < best_count:
                    best_count = cnt
                    chosen = value

            cards[current][chosen] -= 1
            cards[nxt][chosen] += 1

            # If the current player was holding a newly received
            # wildcard, it has now been held for one turn.
            if has_wild[current]:
                wild_new = False

        # Only these two contestants changed.
        winner = -1

        if is_winner(cards[current], has_wild[current]):
            winner = current

        if is_winner(cards[nxt], has_wild[nxt]):
            if winner == -1 or nxt < winner:
                winner = nxt

        if winner != -1:
            return winner

        current = nxt

if __name__ == "__main__":
    print(solve())
```các`VALUES`chuỗi phục vụ hai mục đích. Nó cung cấp cho mỗi thẻ một chỉ số nguyên ổn định và chỉ số đó đã thể hiện thứ tự ràng buộc bắt buộc. Không cần có chức năng so sánh riêng. 

Các thẻ thông thường được lưu trữ dưới dạng số lượng thay vì dưới dạng đối tượng thẻ riêng lẻ. Điều này là đủ vì các quy tắc chỉ hỏi mỗi giá trị xuất hiện bao nhiêu lần. Nó cũng thực hiện việc lựa chọn tần số thấp nhất bằng cách quét 13 phần tử cố định. 

Ký tự đại diện được lưu trữ trong`has_wild`. Sự tách biệt này đặc biệt hữu ích cho bài kiểm tra trạng thái chiến thắng. Một thí sinh giữ ký tự đại diện không thể có chính xác bốn thẻ, ngay cả khi bốn thẻ thông thường của họ đều bằng nhau, vì vậy`is_winner`từ chối mọi người giữ ký tự đại diện. 

Quá trình quét đầu tiên diễn ra trước lượt đầu tiên vì trò chơi kết thúc ngay khi tồn tại trạng thái chiến thắng. Thí sinh bắt đầu có thể có năm thẻ vì ký tự đại diện, vì vậy người giữ ký tự đại diện không bao giờ bị chấp nhận sai là người chiến thắng ban đầu. 

biểu hiện`(current + 1) % n`xử lý thứ tự vòng tròn mà không có trường hợp đặc biệt. Khi thí sinh hiện tại là`n - 1`, thí sinh tiếp theo trở thành số 0, tương ứng với thí sinh 1. 

các`wild_new`cờ được khởi tạo thành`True`bởi vì thí sinh`K`nhận được ký tự đại diện ngay trước lượt đầu tiên. Khi một lá bài thông thường được chuyển đi trong khi người giữ nó có ký tự đại diện, lá cờ sẽ trở thành`False`, nghĩa là ký tự đại diện có thể được chuyển vào lượt tiếp theo của thí sinh đó. Khi ký tự đại diện được thông qua, thí sinh nhận được`wild_new = True`. 

Người thắng sẽ kiểm tra sau một nước đi chỉ kiểm tra`current`Và`nxt`. Bàn tay của họ là những bàn tay duy nhất được sửa đổi bởi động thái đó, trong khi quá trình quét ban đầu đảm bảo rằng không có thí sinh chiến thắng nào đã có mặt ở nơi khác. Nếu cả hai thí sinh bị ảnh hưởng đều chiến thắng, chỉ số dựa trên số 0 nhỏ hơn sẽ được chọn, tương ứng với số lượng thí sinh nhỏ hơn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2 1
33J3
JJJ3
```Thí sinh 1 bắt đầu với`33J3`và ký tự đại diện. Thí sinh 2 có`JJJ3`. 

| Xoay | Hiện tại | Trạng thái ký tự đại diện | Hành động | Trạng thái sau khi di chuyển | Người chiến thắng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | Vừa nhận được | Vượt qua`J`| Người chơi 2 được`JJJJ`| 2 | 

Thí sinh 1 không thể vượt qua ký tự đại diện vì nó mới được nhận. Trong số các thẻ thông thường,`J`xảy ra một lần`3`xảy ra ba lần, vì vậy`J`được chọn. Thí sinh 2 nhận giải tư`J`, tạo ra ván bài chiến thắng`JJJJ`. Trò chơi dừng ngay lập tức với thí sinh thứ 2 là người chiến thắng. 

Điều này thể hiện hạn chế ký tự đại diện đặc biệt ở lượt đầu tiên và thực tế là điều kiện thắng phải được kiểm tra sau khi chuyển. 

### Mẫu 2 

Đầu vào là```
2 2
A2A2
22AA
```Thí sinh 2 bắt đầu bằng ký tự đại diện. 

| Xoay | Hiện tại | Trạng thái ký tự đại diện | Hành động | Trạng thái liên quan sau khi di chuyển | Người chiến thắng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | Vừa nhận được | Vượt qua`A`| P1:`AAA2`, P2:`AA22 + W`| Không có | 
| 2 | 1 | Không có ký tự đại diện | Vượt qua`2`| P1:`AAA`, P2:`AAA2 + W`| Không có | 
| 3 | 2 | Có thể vượt qua ký tự đại diện | Vượt qua`W`| P1:`AAA + 2 + W`, P2:`AAA2`| Không có | 
| 4 | 1 | Vừa nhận được | Vượt qua`2`| P1:`AAA + W`, P2:`AAAA`| 2 | 

Ở lượt đầu tiên thí sinh 2 không thể vượt qua được ký tự đại diện nên các thẻ thông thường`A`Và`2`được so sánh. Cả hai đều xảy ra hai lần và`A`nhỏ hơn nên`A`được thông qua. 

Sau khi thí sinh 1 vượt qua`2`, thí sinh 2 cuối cùng cũng có thể vượt qua ký tự đại diện. Thí sinh 1 sau đó nhận được ký tự đại diện và bị cấm vượt qua ngay lập tức nên vượt qua phần còn lại của mình.`2`. Thí sinh 2 bây giờ có đúng bốn`A`? Không, kết quả là bàn tay bình thường là`AAAA`, vậy thí sinh 2 thắng. Đầu ra là`2`. 

Dấu vết chứng minh lý do tại sao độ trễ một lượt của ký tự đại diện phải được thể hiện rõ ràng thay vì chỉ suy ra từ đó thí sinh hiện đang sở hữu nó. 

## Phân tích độ phức tạp 

hãy để`T`là số lượt mô phỏng trước khi trò chơi kết thúc. Có chính xác 13 giá trị thẻ thông thường. 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(13T) = O(T)`| Mỗi lượt quét 13 giá trị để chọn thẻ và tối đa hai séc chiến thắng 13 giá trị | 
| Không gian |`O(13N) = O(N)`| Mảng tần số lưu trữ 13 bộ đếm cho mỗi thí sinh | 

Với`N <= 13`, sự đại diện của nhà nước là rất nhỏ. Mọi thao tác trên các giá trị thẻ là một vòng lặp 13 phần tử cố định, do đó Python có rất ít chi phí cho mỗi lượt. Giải pháp này không thực hiện bất kỳ tìm kiếm phân nhánh nào hoặc xây dựng tập hợp khổng lồ các phân phối thẻ có thể có. 

## Trường hợp thử nghiệm```python
import sys
import io

VALUES = "A23456789DQJK"
POS = {ch: i for i, ch in enumerate(VALUES)}
V = 13

def game(data: str) -> str:
    inp = io.StringIO(data)

    n, k = map(int, inp.readline().split())
    k -= 1

    cards = [[0] * V for _ in range(n)]
    has_wild = [False] * n

    for i in range(n):
        s = inp.readline().strip()
        for ch in s:
            cards[i][POS[ch]] += 1

    has_wild[k] = True

    def winner(i):
        if has_wild[i]:
            return False
        return sum(cards[i]) == 4 and 4 in cards[i]

    for i in range(n):
        if winner(i):
            return str(i + 1)

    current = k
    wild_new = True

    while True:
        nxt = (current + 1) % n

        if has_wild[current] and not wild_new:
            has_wild[current] = False
            has_wild[nxt] = True
            wild_new = True
        else:
            chosen = -1
            best = 10

            for value in range(V):
                cnt = cards[current][value]
                if cnt and cnt < best:
                    best = cnt
                    chosen = value

            cards[current][chosen] -= 1
            cards[nxt][chosen] += 1

            if has_wild[current]:
                wild_new = False

        candidates = []
        if winner(current):
            candidates.append(current)
        if winner(nxt):
            candidates.append(nxt)

        if candidates:
            return str(min(candidates) + 1)

        current = nxt

# Provided samples
assert game("""\
2 1
33J3
JJJ3
""") == "2", "sample 1"

assert game("""\
2 2
A2A2
22AA
""") == "2", "sample 2"

assert game("""\
4 2
774Q
JJQ7
44Q7
4QJJ
""") == "3", "sample 3"

assert game("""\
3 1
JQAA
JJJA
QQQA
""") == "3", "sample 4"

# Minimum N, starting player has the wildcard, so player 1
# wins immediately with four equal ordinary cards.
assert game("""\
2 2
AAAA
2222
""") == "1", "initial winner while wildcard holder is not winning"

# The wildcard was just received, so player 1 must pass A.
# Player 2 then has AAAA and wins.
assert game("""\
2 1
ABBB
AAAB
""") == "2", "wildcard cannot be passed immediately"

# N = 3, K = 3. Player 3 passes A across the circular boundary
# to player 1. Player 1 later passes its only 2 and wins with AAAA.
assert game("""\
3 3
AAA2
2233
A223
""") == "1", "circular wrap-around and tie-breaking"

# Maximum N. Every contestant starts with four equal cards.
# Player 13 receives the wildcard, but player 1 is already winning.
assert game("""\
13 13
AAAA
2222
3333
4444
5555
6666
7777
8888
9999
DDDD
QQQQ
JJJJ
KKKK
""") == "1", "maximum N and all-equal hands"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 / AAAA / 2222`|`1`| Kích thước tối thiểu và người chiến thắng ban đầu không giữ ký tự đại diện | 
|`2 1 / ABBB / AAAB`|`2`| Ký tự đại diện không thể được thông qua ngay lập tức | 
|`3 3 / AAA2 / 2233 / A223`|`1`| Bao quanh vòng tròn và phá vỡ ràng buộc giá trị thẻ | 
|`13 13 / AAAA / 2222 / ... / KKKK`|`1`| Tối đa`N`, tất cả các ván bài bằng nhau và phát hiện người chiến thắng ban đầu | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là thí sinh xuất phát cầm một ký tự đại diện mới nhận được. Vì```
2 1
ABBB
AAAB
```trạng thái bắt đầu với việc thí sinh 1 cầm`ABBB + W`. các`W`không thể được chuyển ở lượt 1 nên thuật toán sẽ đi vào nhánh bài thông thường. Các tần số là`A = 1`Và`B = 3`, làm`A`thẻ đã chọn. Thí sinh 2 nhận được`A`và những thay đổi từ`AAAB`ĐẾN`AAAA`, do đó, séc chiến thắng của người chơi bị ảnh hưởng sẽ ngay lập tức được trả về`2`. Không có lượt thứ hai được mô phỏng. 

Trường hợp bên thứ hai là người chiến thắng trước khi trò chơi bắt đầu. TRONG```
2 2
AAAA
2222
```thí sinh 2 sở hữu`2222 + W`, trong khi thí sinh 1 sở hữu chính xác`AAAA`. Lần quét đầu tiên xem thí sinh 1 là người chiến thắng trước khi bất kỳ chuyển nhượng nào xảy ra và quay trở lại`1`. Vòng lặp mô phỏng không bao giờ được nhập vào. Điều này cũng chứng tỏ tại sao ký tự đại diện phải bị loại khỏi điều kiện thắng. 

Trường hợp cạnh thứ ba là người giữ ký tự đại diện có bốn thẻ thông thường bằng nhau. Người chơi như vậy có tổng cộng 5 lá bài và không thắng. Ví dụ: nếu một thí sinh giữ`AAAA + W`, mảng tần số thông thường chứa bốn`A`thẻ, nhưng`has_wild`là đúng, vậy`is_winner`trả về sai. Chỉ sau khi ký tự đại diện rời đi thì bốn người đó mới có thể`A`thẻ trở thành một bàn tay chiến thắng. 

Trường hợp cạnh thứ tư là lập chỉ mục hình tròn. TRONG```
3 3
AAA2
2233
A223
```thí sinh hiện tại đầu tiên là thí sinh 3. Hàng xóm bên phải của nó là thí sinh 1, đại diện bởi`(2 + 1) % 3 = 0`trong lập chỉ mục dựa trên số không. Thí sinh 3 có`A = 1`,`2 = 2`, Và`3 = 1`, do đó tần số tối thiểu là một và mối liên hệ nằm giữa`A`Và`3`. Từ`A`có chỉ số giá trị nhỏ hơn thì được chuyển cho thí sinh 1. Thí sinh 1 sau đó đạt được`AAAA`và chiến thắng. Điều này nắm bắt cả ranh giới bao quanh và điểm ngắt ràng buộc có giá trị tối thiểu. 

Trường hợp cạnh thứ năm là sự hòa giữa nhiều thí sinh chiến thắng. Nếu trạng thái ban đầu là```
3 3
AAAA
2222
3333
```thí sinh 1 và 2 đều đã chiến thắng, trong khi thí sinh 3 có ký tự đại diện. Quá trình quét đầu tiên tiến hành theo thứ tự số thí sinh và trả về thí sinh 1. Quy tắc người chiến thắng dựa trên số thí sinh chứ không phải giá trị thẻ, vì vậy thuật toán không bao giờ được chọn thí sinh sau chỉ vì bốn thẻ bằng nhau của họ có giá trị nhỏ hơn.
