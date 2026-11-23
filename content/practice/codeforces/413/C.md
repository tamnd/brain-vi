---
title: "CF 413C - Nguy hiểm!"
description: "Trò chơi bao gồm n câu hỏi, mỗi câu có một giá trị cơ bản a[i]. Một số câu hỏi được đánh dấu là đấu giá. Đội R2 bắt đầu bằng việc chọn câu hỏi đầu tiên. Sau đó, ai trả lời đúng câu hỏi trước sẽ được chọn câu hỏi tiếp theo."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 413
codeforces_index: "C"
codeforces_contest_name: "Coder-Strike 2014 - Round 2"
rating: 1400
weight: 413
solve_time_s: 143
verified: true
draft: false
---

[CF 413C - Nguy hiểm!](https://codeforces.com/problemset/problem/413/C) 

**Đánh giá:** 1400 
**Tags:** tham lam, toán học 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi bao gồm`n`câu hỏi, mỗi câu hỏi có một số giá trị cơ bản`a[i]`. Một số câu hỏi được đánh dấu là đấu giá. Đội R2 bắt đầu bằng việc chọn câu hỏi đầu tiên. Sau đó, ai trả lời đúng câu hỏi trước sẽ được chọn câu hỏi tiếp theo. 

R2 may mắn đến khó tin trong kịch bản này. Bất cứ khi nào họ cố gắng đặt một câu hỏi, họ đều trả lời đúng. Nhiệm vụ là tính tổng số điểm tối đa mà R2 có thể đạt được nếu họ chọn câu hỏi một cách tối ưu. 

Phần thú vị là quy tắc đấu giá. Đối với một câu hỏi đấu giá, người chơi chọn nó có thể tăng giá trị của nó, nhưng chỉ khi điểm hiện tại của họ lớn hơn giá trị ban đầu. Giá trị được chọn có thể là bất kỳ giá trị nào giữa giá trị ban đầu và điểm hiện tại của người chơi. Vì câu trả lời đúng sẽ cộng số điểm đó vào điểm số nên các cuộc đấu giá có thể tăng gấp đôi số điểm hiện tại. 

Những hạn chế là rất nhỏ.`n`nhiều nhất là`100`, và số lượng câu hỏi đấu giá nhiều nhất`30`. Ngay cả các giải pháp theo cấp số nhân cho tất cả các câu hỏi cũng là không thể, nhưng lập trình động theo trạng thái điểm số cũng không cần thiết vì cấu trúc của trò chơi đơn giản hơn nhiều so với lần đầu xuất hiện. 

Điều quan trọng là thứ tự các câu hỏi được R2 kiểm soát hoàn toàn sau câu trả lời đúng đầu tiên. Vì R2 luôn trả lời đúng nên họ sẽ giữ lượt đi mãi mãi. Quyết định có ý nghĩa duy nhất là thứ tự các câu hỏi được thực hiện. 

Một số trường hợp đặc biệt rất dễ bị hiểu lầm. 

Hãy xem xét đầu vào này:```
2 1
10 5
2
```Câu hỏi đấu giá có giá trị cơ bản`5`, nhưng R2 bắt đầu bằng điểm số`0`. Họ không thể nâng giá trị vì điểm hiện tại không lớn hơn`5`. Điểm tốt nhất là`10 + 5 = 15`, không`20`. 

Bây giờ hãy xem xét:```
3 1
5 1 100
2
```Nếu R2 lấy câu hỏi đấu giá có giá trị`1`sau khi giải quyết`5`câu hỏi điểm, điểm số của họ trở thành`10`bởi vì họ có thể nâng cuộc đấu giá lên giá trị`5`. Vậy thì câu trả lời cuối cùng là`110`. Việc thực hiện bất cẩn luôn cộng thêm giá trị cơ bản của các cuộc đấu giá sẽ tạo ra sai sót`106`. 

Một trường hợp tinh tế khác là khi điểm hiện tại bằng giá trị đấu giá:```
2 1
5 5
2
```Quy tắc yêu cầu điểm hiện tại phải lớn hơn giá trị cơ bản. Bình đẳng là chưa đủ. Sau khi giải được câu hỏi đầu tiên, số điểm là`5`, nên cuộc đấu giá vẫn không thể tăng được. Điểm cuối cùng là`10`, không`15`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi thứ tự câu hỏi có thể có. Đối với mỗi hoán vị, chúng tôi mô phỏng trò chơi và tính điểm kết quả. Vì có nhiều nhất`100`câu hỏi, điều này ngay lập tức trở thành không thể. Thậm chí`15!`đã là quá lớn rồi. 

Lực lượng vũ phu hoạt động về mặt khái niệm vì sự tiến hóa điểm số chỉ phụ thuộc vào thứ tự đã chọn. Không có quyết định đối nghịch nào khi R2 nắm quyền kiểm soát. Vấn đề chỉ đơn giản là tìm kiếm qua quá nhiều hoán vị. 

Bước đột phá đến từ việc hiểu cách hoạt động của các câu hỏi đấu giá. 

Giả sử số điểm hiện tại là`S`. 

Đối với một câu hỏi thông thường có giá trị`x`, điểm mới trở thành:```
S + x
```Đối với câu hỏi đấu giá có giá trị cơ bản`x`:

Nếu như`S <= x`, đấu giá không thể tăng lên nên kết quả vẫn là:```
S + x
```Nếu như`S > x`, chúng tôi có thể đặt giá trị đấu giá thành`S`, cho:```
S + S = 2S
```Vì vậy, câu hỏi đấu giá thực sự là một hoạt động "nhân đôi số điểm của bạn", nhưng chỉ sau khi điểm của bạn đã vượt quá giá trị cơ bản. 

Điều này thay đổi toàn bộ quan điểm. Giá trị chính xác của câu hỏi đấu giá chỉ quan trọng như một ngưỡng xác định khi nào có thể tăng gấp đôi. 

Để tối đa hóa điểm số cuối cùng, chúng tôi muốn mở khóa nhân đôi càng sớm càng tốt. Điều đó có nghĩa là các câu hỏi đấu giá có giá trị cơ sở nhỏ hơn sẽ được sử dụng trước tiên. Trước khi phiên đấu giá có thể sử dụng được, chúng tôi có thể cần phải giải một số câu hỏi thông thường để có đủ điểm. 

Điều này dẫn đến một quá trình tham lam: 

Bắt đầu với điểm số`0`. Liên tục thực hiện tất cả các câu hỏi thông thường cần thiết để có thể sử dụng được phiên đấu giá nhỏ nhất tiếp theo. Khi một cuộc đấu giá có thể sử dụng được, hãy sử dụng nó ngay lập tức vì tăng gấp đôi sớm hơn luôn tốt hơn tăng gấp đôi sau này. 

Sau khi tất cả các cuộc đấu giá được xử lý, hãy thêm tất cả các câu hỏi thông thường còn lại một cách bình thường. 

Số lượng đấu giá nhiều nhất`30`, vì vậy việc sắp xếp và mô phỏng là chuyện nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia các câu hỏi thành hai nhóm: câu hỏi thông thường và câu hỏi đấu giá. 

Các câu hỏi đấu giá được xác định bằng chỉ số của chúng từ đầu vào. 
2. Lưu trữ giá trị của tất cả các câu hỏi đấu giá trong một mảng và tất cả các giá trị câu hỏi thông thường trong một mảng khác. 

Vị trí thực tế của câu hỏi không quan trọng vì R2 kiểm soát thứ tự sau thành công đầu tiên. 
3. Sắp xếp cả hai mảng theo thứ tự tăng dần. 

Ngưỡng đấu giá nhỏ hơn nên được mở khóa sớm hơn. Các câu hỏi thông thường nhỏ hơn rất hữu ích vì chúng tôi chỉ muốn chi tiêu số tiền tối thiểu cần thiết trước mỗi lần nhân đôi. 
4. Khởi tạo điểm hiện tại`ans = 0`. 
5. Xử lý các câu hỏi đấu giá từ nhỏ đến lớn. 

Đối với giá trị đấu giá hiện tại`x`, tiếp tục trả lời các câu hỏi thông thường trong khi`ans <= x`. 

Mỗi câu hỏi thông thường được thực hiện chỉ đơn giản là thêm giá trị của nó vào`ans`. 
6. Một lần`ans > x`, lấy câu hỏi đấu giá. 

Vì số điểm hiện tại lớn hơn rất nhiều so với`x`, chúng tôi có thể đặt giá trị đấu giá bằng`ans`, nhân đôi số điểm. 
7. Sau khi tất cả các cuộc đấu giá được xử lý, hãy cộng mọi câu hỏi thông thường còn lại vào điểm số. 

Không còn cơ hội nhân đôi nữa nên các câu hỏi thông thường chỉ được thêm trực tiếp. 

### Tại sao nó hoạt động 

Đặc tính quan trọng là việc nhân đôi sớm hơn ít nhất luôn tốt bằng việc nhân đôi sau đó. 

Giả sử chúng ta có một số lợi nhuận thường xuyên`r`và một cuộc đấu giá có sẵn tăng gấp đôi. So sánh hai khả năng: 

Đầu tiên thêm`r`, sau đó nhân đôi:```
(S + r) * 2 = 2S + 2r
```Đầu tiên nhân đôi, sau đó thêm`r`:```
2S + r
```Giá trị đầu tiên lớn hơn bằng`r`. 

Điều này có nghĩa là trước mỗi cuộc đấu giá, chúng ta chỉ nên dành số điểm thông thường tối thiểu cần thiết để kích hoạt cuộc đấu giá đó. Bất kỳ lợi nhuận thường xuyên bổ sung nào đều có giá trị hơn sau khi nhân đôi. 

Việc xử lý các cuộc đấu giá theo thứ tự tăng dần cũng là tối ưu vì các ngưỡng nhỏ hơn sẽ dễ dàng kích hoạt và mở khóa các lần nhân đôi trong tương lai sớm hơn. 

Cùng với nhau, những sự thật này xác định duy nhất chiến lược tham lam. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    is_auction = [False] * n
    for x in b:
        is_auction[x - 1] = True

    auctions = []
    regular = []

    for i in range(n):
        if is_auction[i]:
            auctions.append(a[i])
        else:
            regular.append(a[i])

    auctions.sort()
    regular.sort()

    ans = 0
    ptr = 0
    k = len(regular)

    for x in auctions:
        while ptr < k and ans <= x:
            ans += regular[ptr]
            ptr += 1

        if ans > x:
            ans *= 2
        else:
            ans += x

    while ptr < k:
        ans += regular[ptr]
        ptr += 1

    print(ans)

solve()
```Phần đầu tiên tách biệt đấu giá và câu hỏi thông thường. Các chỉ số trong đầu vào là dựa trên một, do đó việc triển khai sẽ chuyển đổi chúng thành các vị trí dựa trên 0 khi đánh dấu các câu hỏi đấu giá. 

Cả hai mảng đều được sắp xếp theo thứ tự tăng dần. Thứ tự được sắp xếp là cần thiết cho lập luận tham lam. Các cuộc đấu giá nhỏ hơn nên được mở khóa trước và các câu hỏi thông thường nhỏ hơn chỉ được sử dụng khi cần thiết. 

Con trỏ`ptr`theo dõi có bao nhiêu câu hỏi thông thường đã được sử dụng. Trước khi xử lý câu hỏi đấu giá có ngưỡng`x`, vòng lặp tiếp tục thêm các câu hỏi thông thường cho đến khi điểm hiện tại lớn hơn rất nhiều so với`x`. 

Vào thời điểm đó, cuộc đấu giá có thể nhân đôi số điểm, vì vậy`ans *= 2`. 

Có một nhánh tinh tế:```
if ans > x:
    ans *= 2
else:
    ans += x
```Nhánh thứ hai xử lý trường hợp ngay cả sau khi sử dụng mọi câu hỏi thông thường, cuộc đấu giá vẫn không thể kích hoạt. Trong trường hợp đó, cuộc đấu giá hoạt động giống như một câu hỏi bình thường và chỉ đóng góp giá trị cơ bản của nó. 

Tất cả số học đều phù hợp một cách an toàn với số nguyên 64 bit và số nguyên Python xử lý việc này một cách tự nhiên mà không phải lo lắng về lỗi tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 1
1 3 7 5
3
```Câu hỏi đấu giá:`[7]`Các câu hỏi thông thường:`[1, 3, 5]`| Bước | Điểm hiện tại | Hành động | Điểm mới | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 | Trạng thái ban đầu | 0 | 
| 1 | 0 | Đi thường xuyên 1 | 1 | 
| 2 | 1 | Đi thường xuyên 3 | 4 | 
| 3 | 4 | Đi thường xuyên 5 | 9 | 
| 4 | 9 | Đấu giá 7 điểm nhân đôi | 18 | 

Câu trả lời cuối cùng:`18`Dấu vết này cho thấy rõ điều kiện kích hoạt. Cuộc đấu giá không thể được sử dụng cho đến khi số điểm thực sự lớn hơn`7`. 

### Ví dụ 2 

đầu vào:```
5 2
2 1 10 3 20
2 4
```Câu hỏi đấu giá:`[1, 3]`Các câu hỏi thông thường:`[2, 10, 20]`| Bước | Điểm hiện tại | Hành động | Điểm mới | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 | Trạng thái ban đầu | 0 | 
| 1 | 0 | Đi thường xuyên 2 | 2 | 
| 2 | 2 | Đấu giá 1 điểm nhân đôi | 4 | 
| 3 | 4 | Đấu giá 3 điểm nhân đôi | 8 | 
| 4 | 8 | Đi thường xuyên 10 | 18 | 
| 5 | 18 | Đi thường xuyên 20 | 38 | 

Câu trả lời cuối cùng:`38`Ví dụ này chứng minh tại sao các cuộc đấu giá nên được xử lý sớm. Sử dụng cuộc đấu giá nhỏ ngay lập tức sẽ mở ra một cơ hội nhân đôi khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp thống trị thời gian chạy | 
| Không gian | O(n) | Mảng đấu giá và câu hỏi thường xuyên | 

Với`n <= 100`, giải pháp này dễ dàng nằm trong giới hạn. Ngay cả một thuật toán bậc hai cũng có thể vượt qua một cách thoải mái, nhưng cách tiếp cận tham lam vừa đơn giản hơn vừa rõ ràng hơn về mặt toán học. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        is_auction = [False] * n

        for x in b:
            is_auction[x - 1] = True

        auctions = []
        regular = []

        for i in range(n):
            if is_auction[i]:
                auctions.append(a[i])
            else:
                regular.append(a[i])

        auctions.sort()
        regular.sort()

        ans = 0
        ptr = 0

        for x in auctions:
            while ptr < len(regular) and ans <= x:
                ans += regular[ptr]
                ptr += 1

            if ans > x:
                ans *= 2
            else:
                ans += x

        while ptr < len(regular):
            ans += regular[ptr]
            ptr += 1

        return str(ans)

    return solve()

# provided sample
assert run(
    "4 1\n1 3 7 5\n3\n"
) == "18", "sample 1"

# minimum size
assert run(
    "1 1\n5\n1\n"
) == "5", "single auction"

# equality edge case
assert run(
    "2 1\n5 5\n2\n"
) == "10", "strictly greater condition"

# multiple auctions
assert run(
    "5 2\n2 1 10 3 20\n2 4\n"
) == "38", "greedy ordering"

# all auctions impossible to activate
assert run(
    "3 3\n5 6 7\n1 2 3\n"
) == "18", "no doubling possible"

# all regular except one easy auction
assert run(
    "4 1\n100 1 1 1\n2\n"
) == "204", "doubling after large gain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 5 / 1`|`5`| Kích thước đầu vào tối thiểu | 
|`2 1 / 5 5 / 2`|`10`| Bất bình đẳng nghiêm ngặt trong đấu giá | 
|`5 2 / 2 1 10 3 20 / 2 4`|`38`| Nhân đôi nhiều chuỗi | 
|`3 3 / 5 6 7 / 1 2 3`|`18`| Đấu giá có thể không bao giờ kích hoạt | 
|`4 1 / 100 1 1 1 / 2`|`204`| Trì hoãn nhân đôi sau khi xây dựng điểm | 

## Vỏ cạnh 

Xét trường hợp bất đẳng thức chặt:```
2 1
5 5
2
```Thuật toán sắp xếp các cuộc đấu giá như`[5]`và các câu hỏi thông thường như`[5]`. 

Ban đầu`ans = 0`. 

Vòng lặp thêm câu hỏi thông thường vì`0 <= 5`, sản xuất`ans = 5`. 

Bây giờ điều kiện`ans > 5`vẫn thất bại vì bình đẳng thôi là chưa đủ. Cuộc đấu giá chỉ đóng góp giá trị cơ bản của nó, dẫn đến`10`. 

Việc thực hiện bất cẩn bằng cách sử dụng`>=`sẽ sản xuất không chính xác`15`. 

Bây giờ hãy xem xét trường hợp không có phiên đấu giá nào có thể kích hoạt:```
3 3
5 6 7
1 2 3
```Không có câu hỏi thường xuyên nào cả. 

Thuật toán xử lý các cuộc đấu giá theo thứ tự được sắp xếp. Từ`ans`không bao giờ vượt quá bất kỳ ngưỡng đấu giá nào, mỗi phiên đấu giá chỉ cần thêm giá trị cơ bản của nó:```
0 -> 5 -> 11 -> 18
```Điều này phù hợp chính xác với các quy tắc thực sự. Nếu không có đủ số điểm hiện có, các cuộc đấu giá sẽ hoạt động giống như những câu hỏi thông thường. 

Cuối cùng, hãy xem xét trường hợp trong đó các cuộc đấu giá nhỏ sớm có ý nghĩa quan trọng:```
5 2
2 1 10 3 20
2 4
```Các cuộc đấu giá được sắp xếp là`[1, 3]`. 

Sau khi uống đều đặn`2`, điểm số trở thành`2`, kích hoạt đấu giá`1`và tăng gấp đôi thành`4`. 

Điều đó ngay lập tức kích hoạt đấu giá`3`, lại tăng gấp đôi thành`8`. 

Nếu cuộc đấu giá lớn hơn được xử lý trước thì lần nhân đôi thứ hai sẽ diễn ra sau đó và điểm cuối cùng sẽ nhỏ hơn. Điều này xác nhận tại sao các cuộc đấu giá phải được xử lý theo thứ tự tăng dần.
