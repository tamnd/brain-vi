---
title: "CF 102361G - Trò chơi trên bàn cờ"
description: "Chúng ta có một bàn cờ (n lần n) chứa nhiều nhất một quân cờ trong mỗi ô. Mỗi mảnh đều có màu trắng hoặc đen và có giá trị loại bỏ dương. Một quân trắng và một quân đen có thể bị loại bỏ cùng nhau nếu cả hai quân này hiện đang lộ ra từ hướng dưới cùng bên trái."
date: "2026-08-14T02:51:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102361
codeforces_index: "G"
codeforces_contest_name: "2019 China Collegiate Programming Contest Qinhuangdao Onsite"
rating: 0
weight: 102361
solve_time_s: 189
verified: true
draft: false
---

[CF 102361G - Trò chơi trên bàn cờ](https://codeforces.com/problemset/problem/102361/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bàn cờ (n\times n) chứa nhiều nhất một quân cờ trong mỗi ô. Mỗi mảnh đều có màu trắng hoặc đen và có giá trị loại bỏ dương. Một quân trắng và một quân đen có thể bị loại bỏ cùng nhau nếu cả hai quân này hiện đang lộ ra từ hướng dưới cùng bên trái. Giá của một cặp như vậy là sự khác biệt tuyệt đối về giá trị của chúng. Một mảnh cũng có thể bị loại bỏ bất cứ lúc nào để giữ giá trị riêng của nó. 

Điều kiện tiếp xúc là phần bất thường. Đối với một quân tại ((x,y)), quân khác tại ((x',y')) chặn quân đó bất cứ khi nào (x'\ge x) và (y'\le y). Nói cách khác, không có gì có thể còn yếu ở bên dưới và yếu ở bên trái của mảnh. Chúng tôi cần tổng chi phí tối thiểu cần thiết để loại bỏ từng phần. 

Cuộc thi ban đầu có (n\le12), với giới hạn thời gian một giây và bộ nhớ 1024 MB. Bảng có thể chứa tối đa (12^2=144) quân cờ, do đó, tập hợp con DP trên các quân cờ sẽ có tối đa (2^{144}) trạng thái. Điều đó vượt xa những gì có thể. Giá trị nhỏ của (n), thay vì số lượng lớn các mảnh, gợi ý rằng trạng thái hữu ích nên mô tả một ranh giới của bảng. Chỉ có (\binom{24}{12}=2,704,156) ranh giới đơn điệu khi (n=12), ranh giới này lớn nhưng có thể quản lý được bằng ngôn ngữ cấp thấp và vừa vặn thoải mái trong giới hạn bộ nhớ nhất định. Quan điểm đường viền-DP này cũng là hướng giải chuẩn cho bài toán. 

Có một số trường hợp đặc biệt có thể đánh bại lối giải thích tham lam trực tiếp. Một mảnh duy nhất phải được xử lý bởi chính nó. Vì```
1
W
7
```đáp án là (7), vì không có quân đen nào ghép với nó. Giải pháp chỉ tìm kiếm các cặp sẽ trả về 0 không chính xác hoặc để lại phần chưa được xử lý. 

Một cặp quân có giá trị bằng nhau có thể có giá bằng 0, nhưng cả hai vẫn phải được trưng bày cùng một lúc. Coi như```
2
W.
.B
5 0
0 5
```Cả hai phần đều đã lộ ra ngoài nên có thể tháo rời cùng nhau với chi phí (0). Một giải pháp giả định rằng mỗi cặp đều yêu cầu chi phí loại bỏ một lần dương sẽ bỏ lỡ điều này. 

Tình huống ngược lại tinh tế hơn. Coi như```
2
WB
B.
5 7
7 0
```Quân đen ở ((2,1)) chặn cả hai quân phía trên nó, vì vậy ban đầu nó là quân tiếp xúc duy nhất có liên quan. Loại bỏ nó một mình chi phí (7). Các quân trắng và quân đen ở hàng đầu tiên sau đó sẽ lộ ra đồng thời và có thể được ghép nối với giá (2). Câu trả lời là (9). Một thuật toán bất cẩn có thể ghép nối ngay lập tức hai phần ở hàng đầu tiên vì các ô của chúng trông có vẻ tương thích, mặc dù điều kiện chặn phía dưới bên trái ngăn cản việc sử dụng cặp đó ngay từ đầu. 

Quyền tự do loại bỏ một phần tùy ý không yêu cầu chúng ta phải đưa các phần loại bỏ tùy ý vào trạng thái đường viền. Giả sử một giải pháp tối ưu loại bỏ một mảnh bị chặn (p) trong khi một mảnh khác (q) vẫn chặn (p). Việc loại bỏ (p) không thể cần thiết để làm cho (q) có sẵn, bởi vì bản thân (p) là một trong những mảnh chặn (q). Bất kỳ thao tác nào liên quan đến các phần không liên quan đến sự phụ thuộc này đều có thể được thực hiện theo cùng thứ tự mà không cần (p). Chúng ta có thể trì hoãn việc loại bỏ (p) một lần cho đến khi (p) đạt đến đường viền lộ ra, trả số tiền chính xác như nhau. Do đó, luôn có một giải pháp tối ưu trong đó mỗi lần loại bỏ đều xảy ra khi mảnh đó nằm trên đường viền hiện tại. 

## Phương pháp tiếp cận 

Một giải pháp brute-force tự nhiên coi mọi tập hợp các mảnh còn lại là một trạng thái. Từ một trạng thái, nó có thể thử từng quân trắng hiện đang lộ ra với mọi quân đen hiện đang lộ ra và nó cũng có thể thử loại bỏ từng quân cờ đơn lẻ. Việc ghi nhớ sẽ biến tập hợp con DP này thành (m) phần. Ngay cả khi bỏ qua chi phí kiểm tra phần nào được hiển thị, không gian trạng thái là (2^m) và kiểm tra tất cả các cặp có thể có chi phí (O(m^2)) cho mỗi trạng thái. Với (m=144), điều đó cho ta giới hạn trên của 

[ 
2^{144}\cdot144^2 \khoảng 4,6\times10^{47} 
] 

kiểm tra trạng thái cặp. Lực lượng vũ phu là đúng về mặt khái niệm vì nó trực tiếp mô hình hóa mọi hoạt động pháp lý, nhưng cơ quan đại diện của nhà nước chứa quá nhiều thông tin. 

Quan sát hữu ích là hình học. Các mảnh hiện đang được trưng bày luôn nằm trên một cầu thang đơn điệu chạy từ phía trên bên trái về phía dưới bên phải. Khi mọi thứ ở phía dưới bên trái của cầu thang này đã được gỡ bỏ, bất kỳ phần nào trên cầu thang đều có thể được gỡ bỏ và sau khi tháo nó ra, cầu thang sẽ di chuyển qua ô đó. 

Biểu diễn cầu thang bằng một đường đi chứa chính xác (n) bước đi xuống và (n) bước đi sang phải. Mã hóa bước đi xuống theo (0) và bước đi sang phải (1). Có chính xác 

[ 
\binom{2n}{n} 
] 

những con đường như vậy. Mẫu cục bộ (01) là một góc của cầu thang. Ô ở góc đó chính xác là ô hiện có thể xóa được. Xóa ô đó sẽ thay đổi đường dẫn cục bộ từ (01) thành (10). 

Một góc trống không tốn kém gì và chỉ cần di chuyển đường viền qua một ô trống. Một góc bị chiếm có thể được gỡ bỏ một mình, trả giá trị của nó. Nếu hai góc chiếm có màu khác nhau thì cả hai mảnh đều được phơi sáng đồng thời nên có thể loại bỏ chúng cùng nhau và có thể thực hiện hai thay đổi tương ứng (01\to10) cùng một lúc. Chi phí chuyển đổi của chúng là sự khác biệt tuyệt đối của các giá trị của chúng. 

Điều này mang lại DP kiểu đường đi ngắn nhất trên tất cả các trạng thái đường viền. Sự đơn giản hóa quan trọng là mọi chuyển đổi sẽ di chuyển đường viền theo một hướng, do đó các trạng thái tạo thành một DAG. Không cần thuật toán đường đi ngắn nhất chung. Chúng ta có thể xử lý mặt nạ theo thứ tự tôpô. 

Cách tiếp cận đường viền có trạng thái (\binom{2n}{n}). Đối với một trạng thái có nhiều nhất (n) góc và việc thử tất cả các cặp góc sẽ có giá (O(n^2)). Điều này mang lại độ phức tạp (O(n^2\binom{2n}{n})) được chấp nhận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force DP | (O(2^{n^2}n^4)) | (O(2^{n^2})) | Quá chậm | 
| Đường viền DP | (O(n^2\binom{2n}{n})) | (O(2^{2n})) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn đường viền bằng một chuỗi nhị phân có độ dài (2n). Số 0 có nghĩa là đường viền di chuyển xuống và số 1 có nghĩa là nó di chuyển sang phải. Chính xác (n) bit là một, vì vậy mọi đường viền hợp lệ tương ứng với một trong các mặt nạ (\binom{2n}{n}).

Chúng tôi sử dụng đường viền để tách các ô đã bị xóa khỏi các ô có thể vẫn còn chứa các phần. Một góc được biểu thị bằng mẫu (01) xác định ô tiếp theo có thể lộ ra. 
2. Bắt đầu với đường viền đi xuống (n) lần rồi sang phải (n) lần. Theo thứ tự bit ban đầu thì đây là (0^n1^n). Góc liên quan duy nhất của nó là ranh giới phía dưới bên trái của bàn cờ, chính xác là nơi quân cờ lộ ra đầu tiên phải xuất hiện. 

Đường viền cuối cùng là (1^n0^n). Tại thời điểm đó toàn bộ bảng đã được xử lý. 
3. Đối với mỗi đường viền, tìm tất cả các vị trí chứa mẫu cục bộ (01). Chuyển đổi từng vị trí như vậy thành ô bảng của nó ((x,y)). Các ô trống cũng được đưa vào vì đường viền có thể đi qua chúng mà không mất phí. 
4. Đối với mỗi ô góc, thực hiện chuyển đổi loại bỏ một lần. Nếu ô trống thì chi phí tăng thêm bằng 0. Nếu nó chứa một mảnh, chi phí gia tăng là giá trị của nó. 

Quá trình chuyển đổi này thể hiện việc loại bỏ phần đó một mình khi nó lộ ra hoặc đơn giản là đẩy đường viền qua một ô trống. 
5. Thu thập tất cả các ô góc bị chiếm đóng. Đối với mỗi cặp có màu khác nhau, thực hiện chuyển đổi lật cả hai (01) góc thành (10) cùng một lúc. Thêm sự khác biệt tuyệt đối của các giá trị của chúng. 

Cả hai phần đều hợp pháp cùng một lúc vì chúng đều là các góc của cùng một đường viền trước khi chuyển đổi. Vì hai (01) mẫu không thể trùng nhau nên hai lần lật cục bộ là độc lập. 
6. Lưu trữ chi phí tối thiểu cho mỗi đường viền. Quá trình chuyển đổi luôn thay đổi (01) thành (10), do đó nó di chuyển đường viền một cách đơn điệu về phía đường viền cuối cùng. Điều này cung cấp DAG và cho phép chúng tôi xử lý các trạng thái theo thứ tự mà không cần xem lại chúng. 
7. Trong quá trình thực hiện, hãy đảo ngược thứ tự bit đường viền. Sau đó, quá trình chuyển đổi ban đầu (01\to10) trở thành (10\to01), làm tăng giá trị nguyên của mặt nạ. Do đó, chúng ta có thể liệt kê tất cả các mặt nạ với (n) bit được đặt chính xác bằng cách sử dụng thủ thuật tạo kết hợp của Gosper theo thứ tự số tăng dần. Mặt nạ thu được chính xác là trạng thái đường viền, nhưng bây giờ hướng chuyển tiếp của chúng đồng ý với hướng liệt kê. 
8. Câu trả lời là giá trị DP ở dạng đảo ngược của đường viền cuối cùng. Các bảng trống cũng hoạt động một cách tự nhiên vì mọi chuyển đổi đường viền qua một ô trống đều không tốn phí. 

Tại sao nó hoạt động: duy trì bất biến rằng giá trị DP của đường viền là chi phí tối thiểu để xóa mọi thứ ở phía đã được xử lý của đường viền đó trong khi không chạm vào phần còn lại. Mọi thao tác hợp pháp đều có thể được chuyển đổi thành thao tác được thực hiện khi các phần của nó chạm đến đường viền. Một thao tác duy nhất tương ứng với một lần lật góc, trong khi cặp trắng-đen hợp lệ tương ứng với hai lần lật góc được thực hiện đồng thời. Ngược lại, mọi chuyển đổi do DP tạo ra đều là một hoạt động hợp pháp, bởi vì các ô góc của nó được hiển thị trên đường viền hiện tại. Do đó, mọi trình tự loại bỏ hợp lệ được biểu thị bằng một đường dẫn qua DP và mọi đường dẫn DP mô tả một trình tự loại bỏ hợp lệ với chi phí chính xác như nhau. Do đó, lấy giá trị DP tối thiểu ở đường viền cuối cùng sẽ mang lại giá trị tối ưu. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    n = int(input())

    board = [input().strip() for _ in range(n)]
    color = [[0] * n for _ in range(n)]

    for i in range(n):
        for j in range(n):
            if board[i][j] == 'W':
                color[i][j] = 1
            elif board[i][j] == 'B':
                color[i][j] = 2

    weight = []
    for _ in range(n):
        weight.append(list(map(int, input().split())))

    length = 2 * n
    size = 1 << length

    # We store the contour in reversed bit order.
    #
    # Original start:  0^n 1^n
    # Reversed start:  1^n 0^n
    #
    # Original end:    1^n 0^n
    # Reversed end:    0^n 1^n
    start = (1 << n) - 1
    end = start << n

    # int32 is sufficient:
    # at most 144 single removals, each <= 1e6.
    dp = array('i', [-1]) * size
    dp[start] = 0

    # Bit j of r is the lower bit of an original 01 corner.
    corner_mask = (1 << (length - 1)) - 1

    r = start
    limit = 1 << length

    while r < limit:
        cur = dp[r]

        if cur != -1:
            corners = r & ~(r >> 1) & corner_mask

            pieces = []

            c = corners
            while c:
                low = c & -c
                j = low.bit_length() - 1

                # In the reversed contour, this corner corresponds
                # to original bit positions i and i+1.
                i = length - 2 - j

                # Bits strictly above j+1 in the reversed mask
                # correspond to the original prefix before i.
                y = (r >> (j + 2)).bit_count()
                x = i - y

                col = color[x][y]

                if col:
                    pieces.append((j, col, weight[x][y]))
                    add = weight[x][y]
                else:
                    add = 0

                # 10 -> 01 in the reversed mask.
                # This increases the mask by exactly 2^j.
                nxt = r + low
                nv = cur + add

                old = dp[nxt]
                if old == -1 or nv < old:
                    dp[nxt] = nv

                c -= low

            # Remove two exposed pieces together.
            m = len(pieces)
            for a in range(m):
                j1, c1, w1 = pieces[a]
                for b in range(a):
                    j2, c2, w2 = pieces[b]

                    if c1 == c2:
                        continue

                    low1 = 1 << j1
                    low2 = 1 << j2

                    nxt = r + low1 + low2
                    nv = cur + abs(w1 - w2)

                    old = dp[nxt]
                    if old == -1 or nv < old:
                        dp[nxt] = nv

        # Gosper's hack: next mask with exactly n set bits.
        low = r & -r
        nxt = r + low
        r = (((nxt ^ r) >> 2) // low) | nxt

    print(dp[end])

if __name__ == "__main__":
    solve()
```Đầu vào đầu tiên được chuyển đổi thành hai mảng.`color[i][j]`lưu trữ số 0 cho một ô trống, một cho màu trắng và hai cho màu đen. Sự riêng biệt`weight`mảng lưu trữ giá trị loại bỏ. 

Mảng DP sử dụng số nguyên 32 bit có dấu thông qua`array('i')`. Câu trả lời tối đa có thể là nhiều nhất (144\cdot10^6=144.000.000), vì vậy 32 bit là đủ. Sử dụng danh sách Python thông thường sẽ đắt hơn nhiều vì mọi số nguyên sẽ là một đối tượng Python. Tại (n=12), DP dày đặc có (2^{24}) mục nhập, do đó mảng số nguyên nhỏ gọn giữ mức tiêu thụ bộ nhớ ở khoảng 64 MB. 

Việc thực hiện lưu trữ đường viền ngược. Trong biểu diễn ban đầu, việc loại bỏ một góc lộ ra sẽ thay đổi`01`ĐẾN`10`, giảm mặt nạ số nguyên. Việc xử lý các mặt nạ giảm dần sẽ hiệu quả, nhưng việc liệt kê tất cả các mặt nạ đó một cách hiệu quả sẽ kém thuận tiện hơn trong Python. Sau khi đảo ngược thứ tự bit, quá trình chuyển đổi tương tự sẽ trở thành`10`ĐẾN`01`và tăng mặt nạ. Bản hack của Gosper sau đó liệt kê chính xác các mặt nạ chứa (n) cái, theo thứ tự tăng dần. 

Đối với một góc đảo ngược ở vị trí bit`j`, góc ban đầu tương ứng xảy ra tại`i = 2*n - 2 - j`. Số lần di chuyển phải ban đầu trước góc đó là số bit được đặt trong hậu tố đảo ngược ở trên`j + 1`, được tính bằng```
y = (r >> (j + 2)).bit_count()
```và số bước đi xuống là`x = i - y`. Đây là lý do tại sao không cần quét đường dẫn theo bước (2n) rõ ràng cho mọi trạng thái. 

biểu hiện```
corners = r & ~(r >> 1) & corner_mask
```tìm thấy mọi thứ đảo ngược`10`mẫu. các`corner_mask`loại bỏ vị trí cuối cùng không tồn tại, tránh góc ngoài giới hạn ở bit (2n-1). 

Đối với một góc, thay đổi`10`ĐẾN`01`tăng mặt nạ chính xác (2^j), vì vậy`nxt = r + low`là đủ. Hai góc khác nhau không thể chồng lên nhau, vì hai`10`các mẫu không thể chia sẻ một chút, vì vậy hai lần lật có thể được viết tương tự như`r + low1 + low2`. 

Việc chuyển đổi cặp chỉ được xem xét khi hai màu ở góc khác nhau. Chi phí chuyển đổi chính xác là`abs(w1 - w2)`, phù hợp với quy tắc loại bỏ cặp. Các chuyển tiếp đơn lẻ được xem xét cho mọi góc, kể cả các góc trống, bởi vì một ô đường viền trống phải có thể đi qua được mặc dù nó không thể hiện sự loại bỏ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với mẫu chính thức, mọi ô bị chiếm giữ đều có giá trị (1), vì vậy mọi cặp hợp pháp đều có giá bằng 0. Chi phí tích cực duy nhất đến từ những phần phải được loại bỏ một mình trước khi có thể ghép nối mong muốn. Trình tự tối ưu đã cho có ba lần loại bỏ như vậy. 

Đường viền DP có thể được xem qua các chuyển tiếp có liên quan sau đây. Các mặt nạ được thể hiện trong quy ước đường viền ban đầu, trong đó`0`có nghĩa là xuống và`1`có nghĩa là đúng. 

| Sân khấu | Hoạt động đại diện | Hiệu ứng đường viền | Chi phí DP | 
| --- | --- | --- | --- | 
| 0 | Bắt đầu |`00001111`| 0 | 
| 1 | Chỉ xóa ((4,1)) | lật nó`01`góc | 1 | 
| 2 | Chỉ xóa ((2,1)) | lật góc của nó | 2 | 
| 3 | Chỉ xóa ((2,4)) | lật góc của nó | 3 | 
| 4 | Cặp ((4,2),(3,1)) | lật hai góc | 3 | 
| 5 | Cặp ((1,1),(3,2)) | lật hai góc | 3 | 
| 6 | Cặp ((2,2),(4,3)) | lật hai góc | 3 | 
| 7 | Cặp ((1,2),(3,3)) | lật hai góc | 3 | 
| 8 | Cặp ((2,3),(4,4)) | lật hai góc | 3 | 
| 9 | Cặp ((1,3),(3,4)) | lật hai góc | 3 | 
| 10 | Đường viền trống cuối cùng |`11110000`| 3 | 

Dấu vết này chứng tỏ tại sao DP phải xem xét cả chuyển đổi một góc và hai góc. Khi ba lần loại bỏ không thể tránh khỏi đã được thanh toán, tất cả các hoạt động hữu ích còn lại có thể ghép các phần có giá trị bằng nhau và không tốn phí. 

### Ví dụ chặn 2x2 

Hãy xem xét```
2
WB
B.
5 7
7 0
```Các trạng thái liên quan nhỏ hơn nhiều. 

| Sân khấu | Mảnh tiếp xúc | Chuyển tiếp | Chi phí | 
| --- | --- | --- | --- | 
| 0 | Chỉ ((2,1)) | Chỉ xóa màu đen ((2,1)) | 7 | 
| 1 | ((1,1)) và ((1,2)) | Cặp trắng 5 với đen 7 | 9 | 
| 2 | Không có miếng | Kết thúc qua các góc trống | 9 | 

Ví dụ này thực hiện quy tắc chặn một cách trực tiếp. Ban đầu, hai quân ở hàng đầu tiên không thể ghép được vì quân đen ở ((2,1)) chặn chúng. Đường viền DP chỉ nhìn thấy quân đen phía dưới bên trái là một góc bị chiếm hợp lệ tại đường viền ban đầu, vì vậy nó không thể tạo thành cặp không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^2\binom{2n}{n})) | Có (\binom{2n}{n}) đường viền, tối đa (n) góc và (O(n^2)) cặp góc trên mỗi đường viền. | 
| Không gian | (O(2^{2n})) | Mảng DP dày đặc có (2^{2n}) mục nhập 32 bit. | 

Tại (n=12), số trạng thái đường viền là 

[ 
\binom{24}{12}=2,704,156. 
] 

Mảng DP có (2^{24}=16,777,216) mục nhập, yêu cầu khoảng 64 MB với số nguyên 32 bit. Giới hạn thời gian lý thuyết là giới hạn đường viền-DP tiêu chuẩn cho bài toán này. Việc triển khai Python cũng tránh quét tất cả (2^{24}) số nguyên bằng cách chỉ liệt kê các mặt nạ có đúng (12) bit được đặt, giúp giảm số lượng trạng thái được xử lý từ khoảng 16,8 triệu xuống còn khoảng 2,7 triệu. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả định giải pháp đã gửi được lưu dưới dạng`solution.py`. Người trợ giúp đặt lại mô-đun`input`ràng buộc sau khi thay thế`sys.stdin`, bởi vì giải pháp cố ý sử dụng`sys.stdin.readline`để nhập liệu nhanh.```python
import sys
import io
import solution

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solution.input = sys.stdin.readline

    try:
        solution.solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
sample1 = """\
4
WBB.
BWBW
WBWW
BBBW
1 1 1 0
1 1 1 1
1 1 1 1
1 1 1 1
"""
assert run(sample1) == "3", "sample 1"

# Minimum-size board, one piece.
case2 = """\
1
W
7
"""
assert run(case2) == "7", "single piece"

# Two exposed equal-valued opposite colors.
case3 = """\
2
W.
.B
5 0
0 5
"""
assert run(case3) == "0", "zero-cost exposed pair"

# Equal values, but the pair is initially blocked.
case4 = """\
2
W.
B.
1 0
1 0
"""
assert run(case4) == "2", "blocking must be respected"

# Pairing is possible only after removing the bottom-left blocker.
case5 = """\
2
WB
B.
5 7
7 0
"""
assert run(case5) == "9", "blocked first-row pair"

# Maximum n, but only one occupied cell.
case6 = """\
12
W...........
............
............
............
............
............
............
............
............
............
............
............
1000000 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0
"""
assert run(case6) == "1000000", "maximum n with one piece"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 3 | Trình tự tối ưu chính thức có cả loại bỏ đơn và loại bỏ theo cặp | 
|`1 / W / 7`| 7 | Bảng tối thiểu và không có cặp nào có thể có | 
|`2 / W. / .B`với giá trị 5 và 5 | 0 | Ghép nối hai màu đối lập không tốn chi phí | 
|`2 / W. / B.`với cả hai giá trị 1 | 2 | Các giá trị bằng nhau không ngụ ý một cặp miễn phí khi mảnh này chặn mảnh kia | 
|`2 / WB / B.`với giá trị 5 và 7 | 9 | Một cặp chỉ có sẵn sau một lần xóa trước đó | 
| (12\times12) bảng một mảnh | 1000000 | Chuyển đổi tối đa (n), giá trị lớn và đường viền trống | 

## Vỏ cạnh 

Trường hợp một mảnh```
1
W
7
```bắt đầu và kết thúc với sự chuyển đổi đường viền duy nhất. Ô này là một góc bị chiếm dụng, do đó DP thêm (7), di chuyển đến đường viền cuối cùng và trả về (7). Không có mã trường hợp đặc biệt cho màu hoặc màu đối diện bị thiếu. 

Cặp chi phí bằng 0```
2
W.
.B
5 0
0 5
```đầu tiên đi qua góc dưới bên trái trống. Điều đó làm lộ ra cả hai góc bị chiếm dụng, một màu trắng và một màu đen. Quá trình chuyển đổi cặp lật cả hai góc lại với nhau và cộng (|5-5|=0). Chuyển động của đường viền còn lại đi qua các ô trống với chi phí bằng 0, vì vậy câu trả lời cuối cùng là (0). 

Trường hợp chặn```
2
W.
B.
1 0
1 0
```chỉ bắt đầu với quân đen phía dưới bên trái ở góc đã chiếm. DP phải loại bỏ nó một mình đối với (1). Quân trắng sau đó lộ ra và được lấy ra riêng cho quân khác (1), cho (2). Không có sự chuyển tiếp nào ghép nối chúng vì chúng không bao giờ có màu sắc khác nhau ở hai góc đồng thời. 

Ví dụ chặn thú vị hơn```
2
WB
B.
5 7
7 0
```bắt đầu với mảnh màu đen phía dưới bên trái lộ ra. Loại bỏ nó một mình chi phí (7). Sau đó, đường viền đạt đến trạng thái mà hai mảnh ở hàng đầu tiên đều là các góc. Chúng có màu sắc khác nhau nên DP có thể ghép chúng thành (|5-7|=2). Tổng số là (9). Đây chính xác là kiểu phụ thuộc khiến cho việc ghép đôi tham lam trở nên không đáng tin cậy. 

Cuối cùng, trường hợp thưa thớt có kích thước tối đa có (n=12) nhưng chỉ có một mảnh. Đường viền DP vẫn liệt kê đầy đủ họ các trạng thái đường viền, nhưng mọi chuyển đổi có thể tiếp cận ngoại trừ chuyển đổi qua ô bị chiếm dụng đều có giá bằng không. Đóng góp tích cực duy nhất là việc loại bỏ một quân trắng có giá trị (1.000.000), vì vậy câu trả lời là (1.000.000). Kiểm tra xác nhận rằng việc triển khai xử lý biểu diễn đường viền 24-bit đầy đủ và các giá trị ô lớn mà không bị tràn số nguyên.
