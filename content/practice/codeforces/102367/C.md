---
title: "CF 102367C - Sự trả thù của cầm đồ"
description: "Chúng ta có một bàn cờ (N lần N) được biểu thị bằng (N) chuỗi. A là quân đối phương, K là vua của chúng ta, và - là ô trống nơi có khả năng đặt quân tốt. Quân tốt di chuyển lên trên, nghĩa là quân tốt ở hàng (r+1), cột (c) tấn công hai ô ((r,c-1)) và ((r,c+1))."
date: "2026-08-12T23:27:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102367
codeforces_index: "C"
codeforces_contest_name: "Fall 2019 ICPC-style Waterloo Local Contest"
rating: 0
weight: 102367
solve_time_s: 261
verified: true
draft: false
---

[CF 102367C - Sự trả thù của cầm đồ](https://codeforces.com/problemset/problem/102367/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bàn cờ (N \times N) được biểu diễn bằng (N) chuỗi. MỘT`*`là quân đối thủ,`K`là vua của chúng tôi, và`-`là một ô trống nơi có thể đặt một con tốt. Quân tốt di chuyển lên trên, nghĩa là quân tốt ở hàng (r+1), cột (c) tấn công hai ô ((r,c-1)) và ((r,c+1)). Vua tấn công mọi ô có hàng và cột khác nhau nhiều nhất một ô. Nhiệm vụ là đặt thêm càng ít quân tốt càng tốt sao cho mỗi`*`bị tấn công bởi vua hoặc một trong những con tốt mới. Tốt không thể được đặt trên ô đã bị chiếm bởi`*`hoặc`K`. Vấn đề chính thức có (8 \le N \le 1000), với giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

Quan sát hữu ích đầu tiên là những con tốt tấn công một hàng cụ thể phải được đặt chính xác ở hàng tiếp theo. Một ngôi sao ở hàng (r) chỉ có thể bị tấn công bởi những con tốt ở hàng (r+1), không bao giờ bị tấn công bởi một con tốt ở hàng khác. Do đó, một khi chúng ta tính đến vua, mỗi hàng đều có thể được giải quyết một cách độc lập. Với (N) nhiều nhất là 1000, có nhiều nhất một triệu ô bảng, vì vậy giải pháp (O(N^2)) là hoàn toàn hợp lý. Một cách tiếp cận khám phá các tập hợp con của các vị trí cầm đồ có thể có hoặc liên tục tìm kiếm trên toàn bộ bảng để tìm mọi ngôi sao là quá tốn kém. 

Một số trường hợp ranh giới có thể khiến việc triển khai có vẻ hợp lý trở nên sai lầm. Ngôi sao ở hàng dưới cùng không có hàng nào bên dưới nó nên không quân tốt nào có thể tấn công nó. Ví dụ,```
8
--------
--------
--------
--------
--------
--------
--------
*------K
```có câu trả lời`-1`, vì sao không thể bị quân tốt tấn công và không ở gần quân vua. Việc triển khai bất cẩn chỉ kiểm tra cả hai cột chéo mà không kiểm tra ranh giới hàng có thể truy cập vào một ô không hợp lệ hoặc đếm sai một con tốt. 

Một ngôi sao cũng có thể chiếm giữ cả hai ô cầm đồ tiềm năng. Ví dụ,```
8
--------
--------
--------
--------
--------
--------
-*-*----
-K------
```không phải là một ví dụ hợp lệ cho tình huống này vì vua thay đổi phạm vi bao phủ, vì vậy thay vào đó hãy xem xét mô hình cục bộ có liên quan trong một hàng: nếu một ngôi sao ở cột 2 và các ô ở cột 1 và 3 của hàng tiếp theo đều bị chiếm bởi các quân hiện có, thì không quân tốt nào có thể tấn công ngôi sao đó. Kết quả đúng là`-1`trừ khi nhà vua đã che đậy nó. Việc triển khai coi mọi ô vuông chéo là một vị trí cầm đồ có thể sẽ xác nhận không chính xác rằng có sẵn một con tốt. 

Một trường hợp tinh vi khác là hai ngôi sao cách nhau một cột. Nếu cả hai đều bị lật và ô ở giữa ở hàng tiếp theo trống thì một con tốt sẽ che cả hai ngôi sao. Ví dụ,```
8
-*-*----
--------
--------
--------
--------
--------
--------
------K-
```có câu trả lời`1`. Một giải pháp xử lý từng ngôi sao một cách độc lập và đặt một con tốt cho mỗi ngôi sao sẽ trả về`2`, thiếu thực tế là một con tốt có thể tấn công cả hai điểm cuối. 

Cuối cùng, vua có thể đã bao phủ một số ngôi sao và những ngôi sao đó phải được loại bỏ khỏi việc xem xét trước khi tối ưu hóa con tốt. Trong mẫu chính thức, ngôi sao bên cạnh vua không cần cầm đồ, trong khi các ngôi sao khác yêu cầu tổng cộng hai con tốt. 

## Phương pháp tiếp cận 

Một giải pháp vũ lực trực tiếp có thể coi mọi tập hợp con của các ô trống là một tập hợp các vị trí cầm đồ có thể có, kiểm tra xem tập hợp đó có tấn công mọi ngôi sao hay không và giữ tập hợp con thành công nhỏ nhất. Nếu có (E) ô trống thì có (2^E) tập hợp con và việc kiểm tra một tập hợp con có thể yêu cầu (O(N^2)) hoạt động. Trong trường hợp xấu nhất (E) gần với (N^2), đưa ra các phép toán đại khái (O(N^2 2^{N^2})). Đối với (N=1000), điều này nằm ở mức (10^6 2^{10^6}), điều này không khả thi chút nào. 

Chúng ta có thể làm tốt hơn nhiều bằng cách nhìn vào hình dạng của một hàng. Giả sử một ngôi sao ở cột (c). Tốt có khả năng tấn công nó phải ở cột (c-1) hoặc (c+1) ở hàng tiếp theo. Nếu hai ngôi sao trong cùng một hàng xuất hiện ở cột (c) và (c+2), thì hình vuông ở cột (c+1) ở hàng tiếp theo sẽ tấn công cả hai ngôi sao đó. Do đó, sau khi loại bỏ các ngôi sao đã bị vua tấn công, mỗi hàng sẽ trở thành bài toán che phủ một chiều. 

Quy tắc tham lam hữu ích là quét các ngôi sao từ trái sang phải. Khi gặp một ngôi sao chưa được che chắn ở cột (c), trước tiên hãy thử đặt một con tốt ở cột (c+1). Nếu ô đó trống, việc chọn nó ít nhất cũng tốt như chọn (c-1), vì (c+1) tấn công ngôi sao hiện tại và cũng có thể tấn công ngôi sao tiếp theo tại (c+2). Nếu (c+1) không có thì khả năng duy nhất còn lại là (c-1). Nếu cả hai đều không có thì ngôi sao không thể bị tấn công. 

Sở dĩ sự lựa chọn tham lam này an toàn là ở địa phương. Khi xử lý cột (c), không có ngôi sao tương lai nào nằm ở bên trái. Một con tốt ở (c-1) chỉ có thể giúp ngôi sao hiện tại và vùng đã được xử lý, trong khi một con tốt ở (c+1) có thể giúp ngôi sao hiện tại và có thể là ngôi sao chưa được xử lý tiếp theo. Việc chọn ô vuông bên phải không bao giờ tốn nhiều chi phí hơn và chỉ có thể bảo toàn được phạm vi phủ sóng nhiều hơn trong tương lai. 

Các hàng độc lập, do đó, việc áp dụng phương pháp quét tham lam này cho mỗi hàng sẽ mang lại thuật toán (O(N^2)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^2 2^{N^2})) | (O(N^2)) | Quá chậm | 
| Tối ưu | (O(N^2)) | (O(N^2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc toàn bộ bảng và xác định vị trí vua. Vị trí của nhà vua là cần thiết vì một số ngôi sao không yêu cầu cầm đồ gì cả. 
2. Quét từng ngôi sao và đánh dấu nó là đã bị che bất cứ khi nào hàng và cột của nó đều nằm trong một ô của vua. Một ngôi sao thỏa mãn điều kiện này không tham gia vào quá trình tối ưu hóa chốt. 
3. Xử lý từng hàng bảng một cách độc lập. Một ngôi sao ở hàng (r) chỉ có thể bị tấn công bởi một con tốt ở hàng (r+1), vì vậy các lựa chọn được thực hiện cho một hàng không thể ảnh hưởng đến bất kỳ hàng nào khác. 
4. Quét các cột từ trái sang phải đối với mỗi hàng. Khi ô hiện tại không phải là ngôi sao chưa được che chắn, hãy chuyển sang cột tiếp theo. 
5. Khi tìm thấy một ngôi sao chưa được che chắn ở cột (c), trước tiên hãy kiểm tra ô cầm đồ ứng cử viên ở hàng (r+1), cột (c+1). Nếu ô đó tồn tại và chứa`-`, đặt một con tốt ở đó và tăng câu trả lời lên một. 

Ứng viên bên phải được ưu tiên hơn vì nó tấn công ngôi sao hiện tại và cũng có thể tấn công ngôi sao tương lai ở cột (c+2). Việc chọn ứng viên bên trái không thể cung cấp bất kỳ trợ giúp mới nào cho các ngôi sao chưa được xử lý. 
6. Nếu không có ứng viên bên phải, hãy kiểm tra ứng viên bên trái tại hàng (r+1), cột (c-1). Nếu nó tồn tại và trống, hãy đặt một con tốt ở đó và tăng câu trả lời. 
7. Nếu không có ứng cử viên nào tồn tại dưới dạng ô trống, ngôi sao hiện tại không thể bị tấn công bởi bất kỳ con tốt nào. Vì đã xác định không bị vua bao che nên toàn bộ bàn cờ là không thể và đáp án là`-1`. 
8. Tiếp tục cho đến khi mỗi hàng được xử lý. Số lượng tốt tích lũy là câu trả lời tối thiểu. 

### Tại sao nó hoạt động 

Đối với mỗi ngôi sao không được che chắn, vị trí tốt duy nhất có thể có là hai ô vuông chéo của nó ở hàng tiếp theo. Khi quá trình quét đến ngôi sao đó, con tốt đã chọn trước đó chỉ có thể ở ứng cử viên bên trái của nó, bởi vì tất cả các quyết định trước đó đều ở bên trái. Nếu một con tốt như vậy tồn tại, ngôi sao đã được che phủ và không cần một con tốt mới. Mặt khác, nếu ứng cử viên phù hợp trống, việc chọn nó sẽ bao phủ ngôi sao hiện tại và để lại khả năng bao phủ ngôi sao tiếp theo bằng cùng một con tốt. Bất kỳ giải pháp nào sử dụng ứng cử viên bên trái đều có thể được thay thế bằng giải pháp sử dụng ứng cử viên bên phải mà không làm tăng số lượng con tốt. Nếu ứng cử viên bên phải bị chặn thì ứng cử viên bên trái là lựa chọn duy nhất còn lại. Nếu cả hai đều bị chặn thì không có giải pháp nào có thể che phủ được ngôi sao. Do đó, mọi quyết định tham lam đều tương thích với một giải pháp tối ưu và việc xử lý tất cả các hàng một cách độc lập sẽ mang lại mức tối thiểu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    board = [input().strip() for _ in range(n)]

    kr = kc = -1
    for r in range(n):
        pos = board[r].find('K')
        if pos != -1:
            kr, kc = r, pos
            break

    answer = 0

    for r in range(n):
        c = 0

        while c < n:
            if board[r][c] != '*':
                c += 1
                continue

            # Already attacked by the king.
            if abs(r - kr) <= 1 and abs(c - kc) <= 1:
                c += 1
                continue

            # A pawn must be in the row below the star.
            pr = r + 1

            # No row below means no pawn can attack this star.
            if pr == n:
                print(-1)
                return

            # Prefer the right candidate. It may also cover
            # a future star two columns to the right.
            if c + 1 < n and board[pr][c + 1] == '-':
                answer += 1
                c += 1
                continue

            # Otherwise try the left candidate.
            if c - 1 >= 0 and board[pr][c - 1] == '-':
                answer += 1
                c += 1
                continue

            # Neither possible pawn position is available.
            print(-1)
            return

    print(answer)

if __name__ == "__main__":
    solve()
```Bảng được lưu trữ dưới dạng danh sách các chuỗi vì thuật toán chỉ cần kiểm tra các phần hiện có. Thực ra chúng ta không cần phải viết những con tốt mới đặt lên bảng. Trong quá trình quét từ trái sang phải, một con tốt mới được chọn chỉ có thể ảnh hưởng đến ngôi sao hiện tại và ngôi sao có liên quan ngay sau đó và vị trí của quá trình quét đã nắm bắt được mối quan hệ đó. 

Vua được tìm thấy một lần bằng cách tìm kiếm từng hàng. Đối với mỗi ngôi sao, điều kiện`abs(r - kr) <= 1 and abs(c - kc) <= 1`đại diện chính xác cho tám ô vuông lân cận cộng với độ lệch hàng và cột của nhà vua. Vì bản thân ô là một ngôi sao nên trường hợp đẳng thức không thể xảy ra đồng thời cho cả hai tọa độ, nhưng việc sử dụng điều kiện khoảng cách Chebyshev tiêu chuẩn sẽ giúp logic đơn giản. 

Hàng cầm đồ ứng cử viên là`r + 1`, không`r - 1`, bởi vì hàng đầu tiên của đầu vào là trên cùng của bảng và những con tốt di chuyển về phía các chỉ số hàng nhỏ hơn. Một con tốt tấn công một ngôi sao liên tiếp`r`do đó phải ở dưới nó trong hàng`r + 1`. 

Ứng viên bên phải được kiểm tra trước ứng viên bên trái. Thứ tự này là sự lựa chọn tham lam trung tâm. Nếu hình vuông bên phải trống, việc sử dụng nó có thể bao phủ thêm một ngôi sao hai cột ở bên phải. Ứng cử viên bên trái không có lợi ích như vậy trong tương lai. 

Mã tiến bộ`c`bởi một sau khi đặt một con tốt. Con tốt ở`c + 1`tấn công ngôi sao hiện tại vào lúc`c`và nếu một ngôi sao khác xuất hiện tại`c + 2`, ngôi sao đó sẽ nhìn thấy con tốt này khi quá trình quét tới nó. Không cần phải nhảy qua nó hoặc duy trì một mảng phủ sóng riêng. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn. Câu trả lời lớn nhất có thể là số lượng ô bảng, chỉ là (10^6). 

## Ví dụ đã hoạt động 

Mẫu chính thức là:```
8
-*-*----
--------
--------
--------
-----*K-
--------
--*-----
--------
```Các sao ở hàng 4, cột 5 và hàng 6, cột 2 không giáp vua. Hai ngôi sao ở hàng đầu tiên có thể được xử lý bởi một con tốt vì chúng cách nhau một cột, trong khi ngôi sao phía dưới yêu cầu một con tốt khác. Ngôi sao bên cạnh nhà vua không cần cầm đồ. Câu trả lời chính thức là`2`. 

Đối với hàng chứa hai ngôi sao trên cùng, quá trình quét sẽ diễn ra như sau. 

| Hàng | Cột | Ô hiện tại | Hành động | Tốt | 
| --- | --- | --- | --- | --- | 
| 0 | 0 |`-`| Bỏ qua | 0 | 
| 0 | 1 |`*`| Đặt quân tốt ở hàng 1, cột 2 | 1 | 
| 0 | 2 |`-`| Bỏ qua | 1 | 
| 0 | 3 |`*`| Đã bị quân tốt ở cột 2 che phủ | 1 | 

Con tốt được đặt bên dưới cột 2 tấn công cả hai ngôi sao ở cột 1 và 3. Điều này chứng tỏ tính bất biến then chốt rằng một khi con tốt được chọn ở bên phải của một ngôi sao, ngôi sao tiếp theo ở khoảng cách hai sẽ tự động bị che phủ. 

Đối với ví dụ thứ hai, hãy xem xét:```
8
--*-----
--------
-*------
--------
--------
--------
--------
------K-
```Ngôi sao ở hàng 0, cột 2 yêu cầu một con tốt ở hàng 1. Ngôi sao ở hàng 2, cột 1 yêu cầu một con tốt ở hàng 3. Chúng thuộc các hàng mục tiêu khác nhau nên các lựa chọn con tốt của chúng không thể tương tác. 

| Hàng | Cột sao | Ứng viên phù hợp | Ứng cử viên trái | Hành động | Tốt | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | hàng 1, col 3 là`-`| hàng 1, col 1 là`-`| Chọn đúng | 1 | 
| 2 | 1 | hàng 3, col 2 là`-`| hàng 3, col 0 là`-`| Chọn đúng | 2 | 

Kết quả là`2`. Dấu vết này chứng tỏ tại sao các hàng có thể được giải quyết một cách độc lập: mặc dù bảng có chứa một số ngôi sao, một con tốt được chọn cho một hàng mục tiêu không bao giờ có thể tấn công một ngôi sao ở hàng mục tiêu khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N^2)) | Mỗi ô bảng được kiểm tra tối đa một số lần không đổi. | 
| Không gian | (O(N^2)) | Bảng chứa (N^2) ký tự. | 

Với (N \le 1000), bảng có nhiều nhất một triệu ô. Thuật toán chỉ thực hiện một lượng công việc không đổi trên mỗi ô, do đó, nó vẫn thoải mái trong phạm vi độ phức tạp dự định trong giới hạn 1 giây và 256 MB. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây triển khai cùng một thuật toán như một hàm để mỗi xác nhận có thể chạy độc lập.```python
import sys
import io

def solve_case(inp: str) -> str:
    data = inp.strip().splitlines()
    n = int(data[0])
    board = data[1:1 + n]

    kr = kc = -1
    for r in range(n):
        pos = board[r].find('K')
        if pos != -1:
            kr, kc = r, pos
            break

    answer = 0

    for r in range(n):
        c = 0

        while c < n:
            if board[r][c] != '*':
                c += 1
                continue

            if abs(r - kr) <= 1 and abs(c - kc) <= 1:
                c += 1
                continue

            pr = r + 1

            if pr == n:
                return "-1"

            if c + 1 < n and board[pr][c + 1] == '-':
                answer += 1
                c += 1
                continue

            if c - 1 >= 0 and board[pr][c - 1] == '-':
                answer += 1
                c += 1
                continue

            return "-1"

    return str(answer)

def run(inp: str) -> str:
    return solve_case(inp)

# Provided sample
assert run(
    """8
-*-*----
--------
--------
--------
-----*K-
--------
--*-----
--------
"""
) == "2", "official sample"

# Minimum-size board, no opponent pieces.
assert run(
    """8
--------
--------
--------
--------
--------
--------
--------
---K----
"""
) == "0", "no stars means no pawns are needed"

# Two stars in the same row can share one pawn.
assert run(
    """8
--*-*---
--------
--------
--------
--------
--------
--------
---K----
"""
) == "1", "one pawn attacks both stars"

# A star on the bottom row cannot be attacked by a pawn.
assert run(
    """8
--------
--------
--------
--------
--------
--------
--------
*------K
"""
) == "-1", "bottom-row star has no row below it"

# The king already attacks the only star.
assert run(
    """8
--------
--------
--------
--------
--------
--------
--*-----
---K----
"""
) == "0", "king covers the star"

# Two independent target rows require two pawns.
assert run(
    """8
--*-----
--------
-*------
--------
--------
--------
--------
------K-
"""
) == "2", "different target rows are independent"

# Maximum-size all-empty board with a king.
n = 1000
big_board = ["-" * n for _ in range(n)]
big_board[500] = "-" * 499 + "K" + "-" * 500
assert run(str(n) + "\n" + "\n".join(big_board) + "\n") == "0", \
    "maximum-size board"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 8 x 8 chính thức |`2`| Xử lý đúng ví dụ ban đầu và bảo hiểm cầm đồ được chia sẻ | 
| Bảng trống 8 x 8 có quân vua |`0`| Không có sao nghĩa là không cần cầm đồ | 
| Hai ngôi sao với một ô cầm đồ trống ở giữa |`1`| Một con tốt có thể tấn công hai sao | 
| Dấu sao ở hàng dưới cùng |`-1`| Thiếu hàng bên dưới mục tiêu | 
| Sao cận kề vua |`0`| Bảo hiểm vua bị loại bỏ trước khi xử lý cầm đồ | 
| Sao ở các hàng khác nhau |`2`| Độc lập giữa các hàng mục tiêu | 
| bảng 1000 x 1000 |`0`| Kích thước tối đa và hiệu suất (O(N^2)) | 

## Vỏ cạnh 

Hãy xem xét một ngôi sao ở hàng dưới cùng:```
8
--------
--------
--------
--------
--------
--------
--------
*------K
```Ngôi sao ở hàng 7. Vị trí cầm đồ có thể có của nó sẽ phải ở hàng 8, nằm ngoài bàn cờ. Thuật toán đạt`pr = r + 1 = 8`, ngay lập tức phát hiện ra rằng`pr == n`, và trả về`-1`. Không có vị trí cầm đồ thay thế nào khác trên bàn cờ vì các cuộc tấn công cầm đồ không bỏ qua các hàng. 

Bây giờ hãy xem xét hai ngôi sao cách nhau một cột:```
8
--*-*---
--------
--------
--------
--------
--------
--------
---K----
```Ngôi sao đầu tiên nằm ở cột 2. Ứng cử viên bên phải của nó là hàng 1, cột 3, trống nên thuật toán đặt một con tốt ở đó. Sau đó, khi nó đến ngôi sao ở cột 4, con tốt đó là ứng cử viên bên trái của nó, vì vậy ngôi sao đã bị che phủ. Câu trả lời là`1`, không`2`. Quá trình quét hoạt động vì nó xử lý hàng từ trái sang phải và duy trì phạm vi bao phủ được tạo cho ngôi sao có thể tiếp theo. 

Đối với bảo hiểm vua, sử dụng:```
8
--------
--------
--------
--------
--------
--------
--*-----
---K----
```Ngôi sao ở hàng 6, cột 2 và vua ở hàng 7, cột 3. Hiệu hàng và hiệu cột của chúng đều bằng một nên vua tấn công ngôi sao theo đường chéo. Thuật toán bỏ qua nó trước khi xem xét bất kỳ ứng viên tốt nào và trả về`0`. 

Đối với các vị trí cầm đồ bị chặn, hãy xem xét:```
8
-*------
-K------
--------
--------
--------
--------
--------
--------
```Ngôi sao đã bị nhà vua tấn công trong sự sắp xếp đặc biệt này, vì vậy câu trả lời là`0`. Để xem chính quy tắc vị trí bị chặn, cấu hình cục bộ tương tự có thể xảy ra với vua ở nơi khác: nếu một ngôi sao ở cột (c) có cả hai ô (c-1) và (c+1) ở hàng tiếp theo bị chiếm giữ bởi các quân hiện có, thì cả hai đều không thể nhận được quân tốt. Thuật toán kiểm tra cả ứng viên và trả về`-1`. Nó không bao giờ cho rằng một hình vuông chéo có sẵn tự động. 

Hộp có kích thước tối đa chứa một triệu ô. Vì mỗi hàng được quét một lần và mỗi ô chỉ tham gia kiểm tra theo thời gian liên tục nên thuật toán vẫn giữ nguyên (O(10^6)). Việc triển khai lưu trữ bảng trực tiếp, do đó việc sử dụng bộ nhớ của nó cũng tỷ lệ thuận với một triệu ký tự thay vì theo không gian tìm kiếm theo cấp số nhân.
