---
title: "CF 102279A - Phiên bản đơn giản của Amsopoly"
description: "Bàn cờ có (N+1) vị trí xếp thành vòng tròn. Vị trí (0), tương ứng với vị trí đầu tiên trong bảng kê, thuộc về chính phủ và không bao giờ có thể thực hiện thanh toán. Các vị trí (N) khác là tài sản có thể sở hữu được. Có ba người chơi là Seo, B21 và Lowie."
date: "2026-08-17T10:08:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102279
codeforces_index: "A"
codeforces_contest_name: "HCW 19 Team Round (ICPC format)"
rating: 0
weight: 102279
solve_time_s: 90
verified: true
draft: false
---

[CF 102279A - Phiên bản đơn giản của Amsopoly](https://codeforces.com/problemset/problem/102279/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bàn cờ có (N+1) vị trí xếp thành vòng tròn. Vị trí (0), tương ứng với vị trí đầu tiên trong bảng kê, thuộc về chính phủ và không bao giờ có thể thực hiện thanh toán. Các vị trí (N) khác là tài sản có thể sở hữu được. 

Có ba người chơi là Seo, B21 và Lowie. Mỗi khi người chơi di chuyển, độ dịch chuyển của họ là cố định: Seo luôn di chuyển (V_s), B21 luôn di chuyển (V_b) và Lowie luôn di chuyển (V_l). Họ di chuyển theo thứ tự đó nhiều lần. Khi người chơi đến một tài sản chưa được sở hữu, họ sẽ lấy nó. Việc tiếp cận tài sản của họ không có tác dụng gì, trong khi việc tiếp cận tài sản của người chơi khác sẽ ngay lập tức tạo ra sự kiện mà chúng tôi đang tìm kiếm. 

Nhiệm vụ là tìm số lần tung xúc xắc đã xuất hiện khi lần thanh toán đầu tiên diễn ra. Nếu không có khoản thanh toán nào xảy ra trong (10^9) nước đi được phép của mỗi người chơi thì kết quả đầu ra được yêu cầu là (3\cdot10^9). 

Các giới hạn khiến việc mô phỏng trực tiếp tất cả (10^9) lượt cho mỗi người chơi là không thể. Ngay cả một lượng công việc không đổi rất nhỏ mỗi lượt cũng sẽ cần khoảng ba tỷ lần lặp. Tuy nhiên, kích thước bảng chỉ là (N+1\le100001), điều này gợi ý rõ ràng rằng hành vi liên quan sẽ lặp lại sau một số lượt tỷ lệ thuận với (N). 

Trường hợp tế nhị đầu tiên là đạt được chức vụ trong chính phủ. Ví dụ, với`3 1 1 1`, bảng có bốn vị trí. Seo đạt thuộc tính 1 ở lần tung đầu tiên, B21 đạt thuộc tính tương tự ở lần tung thứ hai nên đáp án là`2`. Việc triển khai bất cẩn coi vị trí bắt đầu như một tài sản thông thường sẽ báo cáo khoản thanh toán không chính xác. 

Một trường hợp biên khác là chuyển động bằng (N). Vì`3 3 3 3`, kích thước bàn cờ là bốn, vì vậy mỗi người chơi sẽ di chuyển ba vị trí mỗi lượt. Seo giành vị trí thứ 3 ở lượt 1 và B21 đạt được vị trí đó ở lượt 2. Câu trả lời là`2`. Sử dụng modulo (N) thay vì modulo (N+1) sẽ âm thầm đưa ra hình dạng bảng sai. 

Thứ tự di chuyển cũng quan trọng. Vì`3 1 2 3`, Seo khẳng định vị trí 1 ở lượt 1, B21 giành vị trí 2 ở lượt 2 và Lowie giành vị trí 3 ở lượt 3. Ở nước đi thứ hai của Seo, anh ta đạt đến vị trí 2 mà B21 đã sở hữu nên câu trả lời là`4`. Một mô phỏng xử lý toàn bộ vòng trước khi kiểm tra quyền sở hữu sẽ bỏ lỡ thực tế là quyền sở hữu thay đổi ngay sau mỗi lần quay riêng lẻ. 

Cuối cùng, trò chơi có thể lặp lại mãi mãi mà không cần thanh toán. mẫu`29 6 10 15`là một trường hợp như vậy và câu trả lời đúng là`3000000000`. Việc dừng lại chỉ vì mọi người chơi đã xem lại một số vị trí là không đủ trừ khi sự lặp lại của toàn bộ trạng thái trò chơi đã được thiết lập. 

## Phương pháp tiếp cận 

Giải pháp đơn giản là mô phỏng trò chơi chính xác như nó diễn ra. Giữ vị trí hiện tại của mỗi người chơi và một mảng lưu trữ người chơi nào sở hữu từng thuộc tính. Xử lý Seo, rồi B21, rồi Lowie, cập nhật vị trí hiện tại với modulo (N+1) và ngay lập tức kiểm tra đích. Nếu nó trống, chỉ định chủ sở hữu của nó. Nếu nó thuộc về người khác thì trả về số cuộn hiện tại. 

Mô phỏng này hoàn toàn chính xác vì nó tuân theo chính xác quá trình chuyển đổi trạng thái được trò chơi mô tả. Vấn đề của nó là điều kiện dừng. Tuyên bố cho phép (10^9) nước đi cho mỗi người chơi, do đó, trường hợp xấu nhất sẽ yêu cầu (3\cdot10^9) lượt mô phỏng, vượt xa giới hạn một giây. 

Quan sát quan trọng là bảng chứa chính xác (N+1) vị trí. Hãy xem xét một người chơi sau (N+1) nước đi của chính họ. Tổng độ dời của chúng là 

[ 
(N+1)V. 
] 

Tức là (V) hoàn thành các vòng quanh một bàn cờ có kích thước (N+1), vì vậy người chơi sẽ ở chính xác nơi họ bắt đầu. Quan trọng hơn, vì số lượng chuyển động là cố định nên trình tự các vị trí đạt được trong các lần di chuyển (N+1) tiếp theo giống hệt với trình tự của các lần di chuyển (N+1) trước đó. Đây là quan sát định kỳ trung tâm được sử dụng bởi bài xã luận chính thức. 

Sau khi (N+1) lượt đi của mỗi người chơi, cả ba người chơi đều quay lại vị trí ban đầu. Thứ tự di chuyển của họ cũng quay trở lại khi bắt đầu vòng và mọi tài sản có thể được yêu cầu trong khối đó đều gặp phải theo cách tương tự ở khối tiếp theo. Nếu không có khoản thanh toán nào xảy ra trong các lần quay đầu tiên (3(N+1)) này thì khối tiếp theo sẽ lặp lại chính xác, do đó khoản thanh toán không bao giờ có thể xảy ra sau đó. 

Vì (N\le100000), (3(N+1)\le300003). Do đó, chúng tôi có thể thay thế mô phỏng ba tỷ bước bằng nhiều nhất là khoảng ba trăm nghìn bước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(10^9)) cuộn | (O(N)) | Quá chậm | 
| Tối ưu | (O(N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy để`m = N + 1`, bởi vì bảng tròn hoàn chỉnh chứa vị trí chính phủ cộng với thuộc tính (N). Lưu trữ mọi vị trí bằng cách sử dụng các chỉ số từ`0`bởi vì`m-1`, Ở đâu`0`là quan điểm của chính phủ. 
2. Tạo một mảng sở hữu có độ dài`m`. Ban đầu mọi mục đều bằng 0, nghĩa là không có chủ sở hữu. Mục nhập tại vị trí`0`không bao giờ được yêu cầu bởi vì nó thuộc về chính phủ. 
3. Giữ ba vị trí hiện tại, ban đầu tất cả đều bằng 0. Đồng thời giữ ba giá trị chuyển động cố định (V_s,V_b,V_l). 
4. Mô phỏng tối đa`m`hoàn thành các vòng đấu. Trong mỗi vòng, xử lý ba người chơi theo đúng thứ tự Seo, B21, Lowie. Người chơi tiến lên theo giá trị di chuyển cố định của họ bằng cách sử dụng modulo`m`. 
5. Tăng bộ đếm cuộn toàn cầu ngay sau mỗi lần di chuyển. Bộ đếm này biểu thị số lần tung xúc xắc đã thực sự xảy ra, vì vậy nếu đích đến hiện tại thuộc về người chơi khác, việc trả lại nó sẽ đưa ra câu trả lời chính xác được yêu cầu. 
6. Nếu đích đến bằng 0, hãy bỏ qua vì chính phủ sở hữu vị trí đó. Nếu chủ sở hữu của nó bằng 0, hãy chỉ định người chơi hiện tại làm chủ sở hữu của nó. Nếu chủ sở hữu của nó đã là người chơi hiện tại thì sẽ không có gì xảy ra. Nếu chủ sở hữu của nó là người chơi khác, hãy trả lại số lần quay hiện tại vì đây là lần thanh toán đầu tiên. 
7. Nếu tất cả`3*m`cuộn có thể được xử lý mà không cần thanh toán, trả lại`3000000000`. Tình trạng sau này`m`các bước di chuyển của mỗi người chơi hoàn toàn giống với trạng thái ban đầu, vì vậy mọi khối trong tương lai sẽ lặp lại các chuyển đổi tương tự và không thể thực hiện thanh toán. 

### Tại sao nó hoạt động 

Điều bất biến là ngay trước mỗi lần quay mô phỏng, các vị trí được lưu trữ và mảng quyền sở hữu sẽ mô tả chính xác trò chơi thực sau cùng một số lần quay. Mỗi chuyển động sử dụng độ dịch chuyển và modulo cố định chính xác (N+1) và quyền sở hữu được cập nhật ngay sau khi đến đích. Do đó, lần đầu tiên thuật toán nhìn thấy chủ sở hữu của người chơi khác chính là khoản thanh toán đầu tiên trong trò chơi thực. 

Nếu không có khoản thanh toán nào xuất hiện trong (N+1) nước đi của mỗi người chơi, thì cả ba vị trí sẽ trở về 0 vì mỗi người chơi đã di chuyển qua một số nguyên vòng bảng hoàn chỉnh. Các giá trị chuyển động cố định giống nhau và thứ tự rẽ giống nhau sau đó sẽ tái tạo cùng một chuỗi các điểm đến. Vì trạng thái sở hữu cũng được tạo ra bởi trình tự tương tự mà không có xung đột nên trò chơi trong tương lai sẽ lặp lại vô thời hạn. Do đó quay trở lại`3000000000`là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, vs, vb, vl = map(int, input().split())

    m = n + 1
    moves = (vs, vb, vl)

    # owner[pos] = 0 if the property is unowned,
    # otherwise 1, 2, or 3 for the corresponding player.
    owner = [0] * m

    # All players start at the government position.
    pos = [0, 0, 0]

    rolls = 0

    # After m moves by each player, the whole game state repeats.
    for _ in range(m):
        for player in range(3):
            pos[player] = (pos[player] + moves[player]) % m
            rolls += 1

            p = pos[player]

            # Position 0 belongs to the government.
            if p == 0:
                continue

            if owner[p] == 0:
                owner[p] = player + 1
            elif owner[p] != player + 1:
                print(rolls)
                return

    print(3000000000)

if __name__ == "__main__":
    solve()
```Phần đầu tiên chuyển đổi đầu vào thành kích thước bảng`n + 1`. Đây là sự lựa chọn lập chỉ mục quan trọng nhất trong quá trình thực hiện. Vị trí của chính phủ được biểu thị bằng 0, trong khi thuộc tính (N) chiếm vị trí`1`bởi vì`n`. 

các`owner`mảng lưu trữ chủ sở hữu hiện tại của mọi tài sản. Không cần phải lưu trữ bất cứ điều gì đặc biệt cho vị trí chính phủ bởi vì`p == 0`kiểm tra bỏ qua nó trước khi mảng quyền sở hữu được tham khảo. 

Vòng lặp bên ngoài chạy chính xác`m`lần. Trong mỗi lần lặp, mỗi người chơi có một lượt, do đó mô phỏng bao gồm chính xác (N+1) lượt cho mỗi người chơi hoặc tổng cộng (3(N+1)) lượt. 

Vòng lặp bên trong sử dụng`player`giá trị`0`,`1`, Và`2`, tương ứng với Seo, B21 và Lowie. Chủ sở hữu được lưu trữ trong mảng là`player + 1`, cho phép số 0 vẫn là giá trị đặc biệt cho thuộc tính chưa được sở hữu. 

Việc cập nhật vị trí sử dụng`(pos[player] + moves[player]) % m`. Modulo phải là`N+1`, không`N`, vì có (N+1) vị trí trên bảng tròn. 

Bộ đếm cuộn được tăng lên trước khi kiểm tra đích. Nếu một khoản thanh toán xảy ra ở nước đi đó, câu trả lời được yêu cầu sẽ bao gồm vòng quay gây ra khoản thanh toán đó, vì vậy giá trị hiện tại của`rolls`chính xác là câu trả lời 

Số nguyên Python có độ chính xác tùy ý, do đó giá trị trọng điểm được yêu cầu`3000000000`không cần loại số nguyên đặc biệt. Mô phỏng thực tế hoạt động tối đa`300003`cuộn, vì vậy cả thời gian và mức sử dụng bộ nhớ đều nằm trong giới hạn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho`7 3 4 5`, bảng có`8`vị trí, được đánh số`0`bởi vì`7`. Người chơi lần lượt di chuyển 3, 4 và 5. 

| Cuộn | Người chơi | Vị trí | Chủ sở hữu sau khi chuyển đi | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | Seo | 3 | 3 = Seo | Khiếu nại 3 | 
| 2 | B21 | 4 | 4 = B21 | Khiếu nại 4 | 
| 3 | Lowie | 5 | 5 = Lowie | Khiếu nại 5 | 
| 4 | Seo | 6 | 6 = Seo | Khiếu nại 6 | 
| 5 | B21 | 0 | không thay đổi | Chính phủ | 
| 6 | Lowie | 2 | 2 = Lowie | Khiếu nại 2 | 
| 7 | Seo | 1 | 1 = Seo | Khiếu nại 1 | 
| 8 | B21 | 4 | không thay đổi | Sở hữu tài sản | 
| 9 | Lowie | 7 | 7 = Lowie | Khiếu nại 7 | 
| 10 | Seo | 4 | 4 = B21 | Thanh toán | 

Lần tung thứ mười là nước đi thứ tư của Seo. Anh ta đạt đến vị trí 4 mà B21 đã giành được ở lượt đổ 2, vì vậy câu trả lời là`10`. Điều này cũng chứng tỏ tại sao quyền sở hữu phải được kiểm tra sau mỗi lượt quay riêng lẻ thay vì sau toàn bộ vòng chơi có ba người chơi. 

### Mẫu 2 

cho`29 6 10 15`, bảng có`30`các vị trí. Mỗi người chơi trở về vị trí 0 sau 30 nước đi của mình vì tổng độ dịch chuyển của họ là bội số của 30. 

| Người chơi | Phong trào | Vị trí đạt modulo 30 | 
| --- | --- | --- | 
| Seo | 6 | 6, 12, 18, 24, 0, ... | 
| B21 | 10 | 10, 20, 0, 10, 20, ... | 
| Lowie | 15 | 15, 0, 15, 0, ... | 

Các vị trí khác 0 được ba chuỗi này truy cập là rời rạc. Do đó, không ai trong số người chơi đạt được tài sản thuộc sở hữu của người chơi khác. Sau 30 nước đi của mỗi người chơi, cả ba đều trở về 0 và mô hình quyền sở hữu lặp lại, do đó, không khối nào sau đó có thể tạo khoản thanh toán. 

Do đó, thuật toán hoàn thành tất cả`3 * 30 = 90`cuộn mô phỏng và đầu ra`3000000000`. Mô phỏng không cần biểu thị hàng tỷ lượt được phép vì chu kỳ 30 nước đi đã chứng minh rằng trò chơi sẽ lặp lại mãi mãi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N)) | Tối đa (3(N+1)) nước đi của người chơi được mô phỏng. | 
| Không gian | (O(N)) | Mảng quyền sở hữu chứa (N+1) mục nhập. | 

Với (N\le100000), thuật toán thực hiện tối đa`300003`cuộn. Mỗi cuộn chỉ sử dụng một lượng công việc không đổi, do đó tổng thời gian chạy dễ dàng phù hợp với giới hạn một giây. Mảng quyền sở hữu sử dụng bộ nhớ tuyến tính, chỉ có khoảng một trăm nghìn mục. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        n, vs, vb, vl = map(int, sys.stdin.readline().split())

        m = n + 1
        moves = (vs, vb, vl)
        owner = [0] * m
        pos = [0, 0, 0]
        rolls = 0

        for _ in range(m):
            for player in range(3):
                pos[player] = (pos[player] + moves[player]) % m
                rolls += 1

                p = pos[player]

                if p == 0:
                    continue

                if owner[p] == 0:
                    owner[p] = player + 1
                elif owner[p] != player + 1:
                    print(rolls)
                    return sys.stdout.getvalue().strip()

        print(3000000000)
        return sys.stdout.getvalue().strip()

    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1.
assert solve_data("7 3 4 5\n") == "10", "sample 1"

# Provided sample 2.
assert solve_data("29 6 10 15\n") == "3000000000", "sample 2"

# Minimum N, equal movements. Seo claims position 1,
# then B21 immediately reaches it.
assert solve_data("3 1 1 1\n") == "2", "minimum size and equal values"

# Minimum N, movement equal to N.
# The board has 4 positions, so all players first reach position 3.
assert solve_data("3 3 3 3\n") == "2", "boundary movement N"

# The collision happens only after the first three rolls.
assert solve_data("3 1 2 3\n") == "4", "turn-order and off-by-one case"

# Maximum N with maximum movement. The same property is reached
# by Seo and B21 on their first moves.
assert solve_data("100000 100000 100000 100000\n") == "2", "maximum N"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`7 3 4 5`|`10`| Cung cấp mẫu và kiểm tra quyền sở hữu mỗi cuộn ngay lập tức | 
|`29 6 10 15`|`3000000000`| Tính định kỳ và trường hợp không thanh toán | 
|`3 1 1 1`|`2`| Kích thước bảng tối thiểu và chuyển động bằng nhau | 
|`3 3 3 3`|`2`| Đúng modulo (N+1) và chuyển động tối đa | 
|`3 1 2 3`|`4`| Thứ tự người chơi chính xác và đếm cuộn | 
|`100000 100000 100000 100000`|`2`| Giá trị chuyển động tối đa (N) và tối đa | 

## Vỏ cạnh 

cho`3 1 1 1`, kích thước bảng là bốn. Seo di chuyển từ vị trí 0 đến vị trí 1 và khẳng định điều đó. Sau đó B21 di chuyển từ vị trí 0 lên vị trí 1 và tìm được quyền sở hữu của Seo nên lần đổ thứ 2 là lần thanh toán đầu tiên. Thuật toán trả về`2`, trong khi xử lý chính xác vị trí số 0 là vị trí của chính phủ. 

Vì`3 3 3 3`, cơ số modulo là bốn chứ không phải ba. Seo di chuyển từ vị trí số 0 đến vị trí thứ ba và khẳng định điều đó. B21 cũng tiến lên vị trí thứ 3 và trả ngay cho Seo, sản xuất`2`. Trường hợp này phát hiện các triển khai sử dụng nhầm`n`bằng chiều dài tấm ván tròn. 

Vì`3 1 2 3`, ba lượt quay đầu tiên tạo ra ba thuộc tính riêng biệt: Seo sở hữu vị trí 1, B21 sở hữu vị trí 2 và Lowie sở hữu vị trí 3. Nước đi thứ hai của Seo đạt đến vị trí 2 nên việc thanh toán diễn ra ở lượt 4. Mảng quyền sở hữu được cập nhật sau mỗi nước đi, giúp duy trì chính xác thứ tự thời gian mà trò chơi yêu cầu. 

Vì`29 6 10 15`, cả ba người chơi liên tục chỉ ghé thăm những vị trí mà những người khác không ghé thăm. Sau 30 nước đi của mỗi người chơi, tất cả các vị trí và chuyển đổi quyền sở hữu lại quay lại cùng một chu kỳ. Thuật toán phát hiện không có khoản thanh toán nào trong số đó`90`cuộn và trả lại`3000000000`, thay vì cố gắng mô phỏng hàng tỷ lượt còn lại của mỗi người chơi.
