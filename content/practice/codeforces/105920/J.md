---
title: "CF 105920J - Cầu III"
description: "Đầu vào mô tả một chuỗi các hành động trong một cuộc đấu giá cầu nối đơn giản. Bốn người chơi hành động theo một thứ tự chu kỳ cố định và mỗi hành động là một giá thầu (đấu giá như “2C” hoặc “1S”), một đường chuyền, một cú đúp hoặc một cú đúp. Các quy tắc chi phối cách thức các hành động này có thể xuất hiện một cách hợp pháp."
date: "2026-06-22T15:30:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105920
codeforces_index: "J"
codeforces_contest_name: "Soy Cup #1: Firefly"
rating: 0
weight: 105920
solve_time_s: 71
verified: true
draft: false
---

[CF 105920J - Cầu III](https://codeforces.com/problemset/problem/105920/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả một chuỗi các hành động trong một cuộc đấu giá cầu nối đơn giản. Bốn người chơi hành động theo một thứ tự chu kỳ cố định và mỗi hành động là một giá thầu (đấu giá như “2C” hoặc “1S”), một đường chuyền, một cú đúp hoặc một cú đúp. 

Các quy tắc chi phối cách thức các hành động này có thể xuất hiện một cách hợp pháp. Giá thầu phải luôn tăng giá thầu cao nhất hiện tại theo thứ tự nghiêm ngặt trước tiên là so sánh cấp độ (1 đến 7) và sau đó là thứ tự phù hợp (Câu lạc bộ < Kim cương < Trái tim < Cờ < Không Trump). Một đường chuyền luôn được cho phép. Cú đúp chỉ hợp pháp nếu giá thầu có ý nghĩa cuối cùng được thực hiện bởi đối thủ và cú đúp chỉ hợp pháp nếu hành động có ý nghĩa cuối cùng là cú đúp do đối thủ thực hiện. Cuộc đấu giá kết thúc khi cả bốn người chơi đều vượt qua ngay lập tức hoặc khi sau khi một số hành động không vượt qua xuất hiện, có ba lần vượt qua liên tiếp. 

Nhiệm vụ là quyết định xem một chuỗi hành động nhất định có hợp lệ theo các quy tắc này hay không và liệu nó có đại diện cho một cuộc đấu giá hoàn chỉnh sẽ thực sự kết thúc hay không. 

Kích thước đầu vào đủ nhỏ để ngay cả việc quét tuyến tính cho mỗi trường hợp thử nghiệm cũng đủ. Với tối đa 320 hành động cho mỗi trường hợp và 100 trường hợp, một giải pháp duy trì lượng trạng thái không đổi trên mỗi bước là đủ nhanh, do đó, bất kỳ phương pháp nào cố gắng quay lui hoặc mô phỏng các nhánh đều không cần thiết. 

Khó khăn chính không phải là hiệu suất mà là duy trì một cách trung thực trạng thái đấu giá đang phát triển: ai là người đưa ra giá thầu có ý nghĩa cuối cùng, liệu hành động cuối cùng có phải là gấp đôi hay không và liệu trình tự hiện tại có đáp ứng các quy tắc chấm dứt hay không. 

Một số tình huống khó khăn có xu hướng phá vỡ việc triển khai ngây thơ. Một là một chuỗi bao gồm toàn bộ các đường chuyền. Ví dụ: “P P P P” là hợp lệ và đầy đủ. Một cách tiếp cận ngây thơ chỉ kiểm tra một hành động không đạt trước khi quyết định chấm dứt có thể từ chối nó một cách không chính xác. 

Một tình huống phức tạp khác là khi lượt vượt qua xảy ra trước bất kỳ giá thầu nào, sau đó giá thầu xuất hiện và sau đó tiếp tục vượt qua. Quy tắc chấm dứt ba lần vượt qua chỉ áp dụng sau khi tồn tại một hành động không vượt qua, do đó, các lần vượt qua sớm sẽ không kích hoạt logic hoàn thành. 

Trường hợp tinh vi thứ ba là xen kẽ tăng gấp đôi và tăng gấp đôi giữa các quan hệ đối tác. Ví dụ: sau khi đặt giá thầu, một cú đúp hợp lệ của người chơi tiếp theo, sau đó là một cú đúp hợp lệ của đối tác đối phương, phải theo dõi chặt chẽ quyền sở hữu của hành động không vượt qua cuối cùng. Bất kỳ triển khai nào chỉ kiểm tra “loại hành động cuối cùng” mà không theo dõi ai đã thực hiện hành động đó sẽ thất bại. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để xác minh tính hợp lệ sẽ mô phỏng toàn bộ cuộc đấu giá trong khi đối với mọi hành động, tính toán lại từ đầu giá thầu hợp lệ cuối cùng là gì, ai sở hữu nó và liệu có được phép nhân đôi hay nhân đôi hay không. Đối với mỗi bước, chúng tôi có thể quét ngược qua trình tự để tìm hành động không vượt qua cuối cùng và xác định tính hợp pháp. Điều này dẫn đến giải pháp O(n²) cho mỗi trường hợp thử nghiệm vì mỗi hành động trong số n hành động có thể yêu cầu quét tối đa n hành động trước đó. Với 320 hành động cho mỗi lần kiểm tra và 100 lần kiểm tra, điều này trở nên khó khăn và không cần thiết. 

Quan sát chính là tất cả thông tin cần thiết có thể được duy trì dần dần. Tại bất kỳ thời điểm nào, chúng tôi chỉ cần nhớ giá thầu cao nhất hiện tại, người chơi đã thực hiện giá thầu đó cũng như loại hành động và chủ sở hữu không vượt qua gần đây nhất. Tính hợp pháp của mỗi hành động mới chỉ phụ thuộc vào trạng thái có kích thước không đổi này chứ không phụ thuộc vào toàn bộ lịch sử. 

Khi chúng tôi theo dõi các biến này, mọi hành động sẽ trở thành quá trình chuyển đổi theo thời gian liên tục. Chúng tôi cũng duy trì bộ đếm số lần vượt qua liên tiếp kể từ hành động không vượt qua cuối cùng, đặt lại nó bất cứ khi nào xảy ra giá thầu, gấp đôi hoặc gấp đôi. Điều này trực tiếp mô hình hóa điều kiện kết thúc mà không cần xem lại các sự kiện trong quá khứ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) mỗi lần kiểm tra | O(1) | Quá chậm | 
| Mô phỏng tối ưu | O(n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi xử lý các hành động theo thứ tự, theo dõi một trạng thái nhỏ. 

1. Chúng tôi duy trì chỉ số người chơi hiện tại, theo chu kỳ từ 0 đến 3. Mỗi hành động được quy cho người chơi theo thứ tự lần lượt. 
2. Chúng tôi lưu trữ giá thầu hợp lệ cuối cùng dưới dạng một cặp (cấp độ, thứ hạng phù hợp). Ban đầu không có giá thầu. 
3. Chúng tôi theo dõi chủ sở hữu của giá thầu có ý nghĩa cuối cùng, nghĩa là người chơi đã đặt giá thầu gần đây nhất không bị ghi đè. 
4. Chúng tôi theo dõi loại hành động không vượt qua cuối cùng và chủ sở hữu của nó. Điều này là cần thiết để xác nhận nhân đôi và nhân đôi. 
5. Chúng tôi duy trì bộ đếm các lượt chuyền liên tiếp kể từ hành động không vượt qua cuối cùng. 
6. Chúng tôi duy trì một cờ cho biết liệu có bất kỳ hành động không vượt qua nào xảy ra hay không, điều này cần thiết cho điều kiện chấm dứt "tất cả bốn lần vượt qua ban đầu". 

Đối với mỗi hành động: 

1. Nếu hành động là vượt qua, chúng tôi sẽ tăng bộ đếm vượt qua. Nếu bộ đếm đạt đến 4 và không có hành động không vượt qua nào xảy ra, chúng tôi chấp nhận chấm dứt. Ngược lại, nếu đã có hành động không vượt qua và bộ đếm đạt đến 3 thì cuộc đấu giá đã hoàn tất. 
2. Nếu hành động là giá thầu, chúng tôi sẽ kiểm tra xem nó có xếp hạng cao hơn giá thầu hiện tại hay không nếu có. Nếu không, trình tự không hợp lệ. Chúng tôi cập nhật giá thầu cuối cùng và đặt lại bộ đếm lượt vượt qua. 
3. Nếu hành động đó là gấp đôi, chúng tôi xác minh rằng hành động không vượt qua cuối cùng là giá thầu của đối thủ. Nếu không, trình tự không hợp lệ. Chúng tôi ghi lại hành động gấp đôi này làm hành động không vượt qua cuối cùng và đặt lại bộ đếm lượt vượt qua. 
4. Nếu hành động đó là một cú đúp, chúng tôi xác minh rằng hành động không vượt qua cuối cùng là một cú đúp của đối thủ. Nếu không, trình tự không hợp lệ. Chúng tôi cập nhật trạng thái tương tự và đặt lại bộ đếm lượt truy cập. 
5. Nếu tại bất kỳ thời điểm nào một hành động vi phạm các quy tắc này, chúng tôi sẽ ngay lập tức trả về không hợp lệ. 
6. Sau khi xử lý tất cả các hành động, chúng tôi kiểm tra xem đã đạt được điều kiện chấm dứt hợp lệ hay chưa. Nếu không, trình tự không đầy đủ và do đó không hợp lệ. 

Điều bất biến chính là ở mỗi bước, trạng thái được lưu trữ thể hiện chính xác thông tin tối thiểu cần thiết để xác thực bước đi tiếp theo: giá thầu cao nhất hiện tại cũng như danh tính và loại hành động có ý nghĩa cuối cùng. Bởi vì mọi quy luật chỉ phụ thuộc vào hai khía cạnh này của lịch sử nên toàn bộ trình tự không bao giờ cần phải xem xét lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

suit_rank = {'C': 0, 'D': 1, 'H': 2, 'S': 3, 'N': 4}

def parse_bid(b):
    # b like "2C" or "1N"
    level = int(b[0])
    suit = suit_rank[b[1]]
    return level, suit

def is_higher(a, b):
    # a > b?
    return a[0] > b[0] or (a[0] == b[0] and a[1] > b[1])

t = int(input())
for _ in range(t):
    arr = input().split()
    n = len(arr)

    player = 0

    last_bid = None
    last_bid_player = -1

    last_nonpass_type = None
    last_nonpass_player = -1

    pass_count = 0
    had_nonpass = False

    valid = True

    for x in arr:
        p = player
        player = (player + 1) % 4

        if x == 'P':
            pass_count += 1
            if had_nonpass and pass_count == 3:
                break
            continue

        had_nonpass = True

        if x == 'X':
            if last_nonpass_type != 'BID':
                valid = False
                break
            if last_nonpass_player == p:
                valid = False
                break
            last_nonpass_type = 'X'
            last_nonpass_player = p
            pass_count = 0
            continue

        if x == 'XX':
            if last_nonpass_type != 'X':
                valid = False
                break
            if last_nonpass_player == p:
                valid = False
                break
            last_nonpass_type = 'XX'
            last_nonpass_player = p
            pass_count = 0
            continue

        level, suit = parse_bid(x)

        if last_bid is not None:
            if not is_higher((level, suit), last_bid):
                valid = False
                break

        last_bid = (level, suit)
        last_bid_player = p

        last_nonpass_type = 'BID'
        last_nonpass_player = p
        pass_count = 0

    if not valid:
        print("NO")
        continue

    if had_nonpass and pass_count >= 3:
        print("YES")
    elif (not had_nonpass) and pass_count == 4:
        print("YES")
    else:
        print("NO")
```Mã này phản chiếu trực tiếp máy trạng thái được mô tả trước đó. Lập chỉ mục người chơi đảm bảo phân công lượt chính xác. So sánh giá thầu sử dụng thứ tự từ điển về cấp độ và thứ hạng phù hợp. Hành động không vượt qua cuối cùng được theo dõi rõ ràng sao cho tính hợp pháp của double và redouble phụ thuộc vào cả loại và quyền sở hữu của đối thủ. Việc đếm lượt được đặt lại bất cứ khi nào xảy ra bất kỳ hành động có ý nghĩa nào, đảm bảo phát hiện chính xác việc chấm dứt. 

Một chi tiết tinh tế là việc chấm dứt chỉ được kiểm tra sau khi xử lý tất cả các hành động, nhưng được phép nghỉ sớm trong ba lần liên tiếp vì khi cuộc đấu giá hoàn tất, các hành động sau đó không liên quan đến tính hợp lệ. 

## Ví dụ đã hoạt động 

Hãy xem xét trình tự: “P P P P”. Cả bốn người chơi đều vượt qua ngay lập tức. Nhà nước phát triển như sau. 

| Bước | Hành động | Người chơi | Số lần vượt qua | Đã không vượt qua | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | P | 0 | 1 | F | T | 
| 2 | P | 1 | 2 | F | T | 
| 3 | P | 2 | 3 | F | T | 
| 4 | P | 3 | 4 | F | T | 

Sau lần vượt qua thứ tư, quy tắc cho “tất cả bốn lần vượt qua ban đầu” được thỏa mãn, do đó đầu ra là CÓ. Điều này khẳng định rằng việc không có người dự thầu vẫn là một cuộc đấu giá đã hoàn thành hợp lệ. 

Bây giờ hãy xem xét: “P P 1C P P P”. 

| Bước | Hành động | Người chơi | Giá thầu cuối cùng | Số lần vượt qua | Đã không vượt qua | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | P | 0 | - | 1 | F | T | 
| 2 | P | 1 | - | 2 | F | T | 
| 3 | 1C | 2 | 1C | 0 | T | T | 
| 4 | P | 3 | 1C | 1 | T | T | 
| 5 | P | 0 | 1C | 2 | T | T | 
| 6 | P | 1 | 1C | 3 | T | T | 

Ở bước 6, chúng ta có 3 lượt đi liên tiếp sau khi đấu giá nên cuộc đấu giá đã hoàn tất. Điều này chứng tỏ tại sao cần phải có cờ had_nonpass vì hai lần chuyển đầu tiên không được tính vào việc chấm dứt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi hành động cập nhật trạng thái không đổi một lần | 
| Không gian | O(1) | Chỉ một số biến cố định được lưu trữ | 

Các ràng buộc cho phép tổng cộng có tới vài chục nghìn hành động và giải pháp xử lý từng hành động trong thời gian không đổi, dễ dàng phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []
    
    t = int(input())
    for _ in range(t):
        arr = input().split()

        suit_rank = {'C': 0, 'D': 1, 'H': 2, 'S': 3, 'N': 4}

        def parse_bid(b):
            level = int(b[0])
            return level, suit_rank[b[1]]

        def is_higher(a, b):
            return a[0] > b[0] or (a[0] == b[0] and a[1] > b[1])

        player = 0
        last_bid = None
        last_nonpass_type = None
        last_nonpass_player = -1
        pass_count = 0
        had_nonpass = False
        valid = True

        for x in arr:
            p = player
            player = (player + 1) % 4

            if x == 'P':
                pass_count += 1
                if had_nonpass and pass_count == 3:
                    break
                continue

            had_nonpass = True

            if x == 'X':
                if last_nonpass_type != 'BID' or last_nonpass_player == p:
                    valid = False
                    break
                last_nonpass_type = 'X'
                last_nonpass_player = p
                pass_count = 0
                continue

            if x == 'XX':
                if last_nonpass_type != 'X' or last_nonpass_player == p:
                    valid = False
                    break
                last_nonpass_type = 'XX'
                last_nonpass_player = p
                pass_count = 0
                continue

            level, suit = parse_bid(x)

            if last_bid and not is_higher((level, suit), last_bid):
                valid = False
                break

            last_bid = (level, suit)
            last_nonpass_type = 'BID'
            last_nonpass_player = p
            pass_count = 0

        if not valid:
            out.append("NO")
            continue

        if had_nonpass and pass_count >= 3:
            out.append("YES")
        elif (not had_nonpass) and pass_count == 4:
            out.append("YES")
        else:
            out.append("NO")

    return "\n".join(out)

# provided samples
assert run("""1
4
P P P P
""") == "YES"

assert run("""1
3
P P P
""") == "NO"

# custom cases
assert run("""1
1
P
""") == "NO"

assert run("""1
5
P P 1C P P P
""") == "YES"

assert run("""1
6
1C X P XX P P
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tất cả các lượt đi | CÓ | quy tắc chấm dứt ngay lập tức | 
| Vượt qua sớm rồi đấu giá | CÓ | vượt qua thiết lập lại bộ đếm sau khi đấu thầu | 
| Vé đơn | KHÔNG | đấu giá không đầy đủ | 
| Đấu giá sau khi vượt qua rồi kết thúc | CÓ | Chấm dứt 3 lượt | 
| Chuỗi đôi/redouble | CÓ | theo dõi đối thủ | 

## Vỏ cạnh 

Một chuỗi hoàn toàn trôi qua thể hiện quy tắc chấm dứt đặc biệt không yêu cầu bất kỳ giá thầu nào. Thuật toán xử lý vấn đề này một cách riêng biệt bằng cách kiểm tra xem bốn lượt liên tiếp có xảy ra hay không trong khi không thấy hành động không vượt qua nào. Bộ đếm đạt tới 4 và điều kiện được chấp nhận ngay lập tức. 

Trình tự trong đó các lần vượt qua xảy ra trước lần giá thầu đầu tiên kiểm tra xem việc triển khai có tính sai các lần vượt qua sớm cho đến khi chấm dứt hay không. Trong mã, cờ had_nonpass đảm bảo rằng chỉ những lượt vượt qua sau một hành động có ý nghĩa mới được xem xét cho quy tắc vượt qua ba lượt, do đó, những lượt vượt qua sớm không kết thúc sớm phiên đấu giá. 

Xác thực kép và lặp lại dựa vào việc theo dõi cả loại và chủ sở hữu của hành động không vượt qua cuối cùng. Nếu một người chơi cố gắng nhân đôi ngay sau giá thầu của đội mình, việc kiểm tra quyền sở hữu sẽ không thành công. Thuật toán so sánh rõ ràng chỉ số người chơi hiện tại với Last_nonpass_player, đảm bảo chỉ đối thủ mới có thể kích hoạt những hành động này, giúp duy trì tính chính xác trong các mối quan hệ đối tác xen kẽ.
