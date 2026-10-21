---
title: "CF 105819B - Hy Sinh Xe"
description: "Chúng ta đang chơi một ván cờ tàn đơn giản trên bàn cờ 8 x 8. Chúng ta điều khiển một quân vua và một quân xe, trong khi đối thủ chỉ điều khiển một quân vua. Câu hỏi đặt ra là liệu chúng ta có thể thực hiện chính xác một nước đi hợp pháp để ở lượt tiếp theo của đối thủ, vua của họ có thể bắt quân của chúng ta một cách hợp pháp hay không."
date: "2026-06-25T15:05:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105819
codeforces_index: "B"
codeforces_contest_name: "TeamsCode Spring 2025 Novice Division"
rating: 0
weight: 105819
solve_time_s: 54
verified: true
draft: false
---

[CF 105819B - Hy sinh quân Xe](https://codeforces.com/problemset/problem/105819/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang chơi một ván cờ tàn đơn giản trên bàn cờ 8 x 8. Chúng ta điều khiển một quân vua và một quân xe, trong khi đối thủ chỉ điều khiển một quân vua. Câu hỏi đặt ra là liệu chúng ta có thể thực hiện chính xác một nước đi hợp pháp để ở lượt tiếp theo của đối thủ, vua của họ có thể bắt quân của chúng ta một cách hợp pháp hay không. Nói cách khác, sau nước đi của chúng ta, quân xe phải được đặt vào ô mà vua đối phương có quyền bắt giữ hợp pháp. 

Đầu vào chứa một số vị trí độc lập. Mỗi vị trí cung cấp hình vuông của quân vua, quân của chúng ta và quân vua của đối thủ. Đối với mọi vị trí, chúng tôi in ra liệu có thể hy sinh một nước đi như vậy hay không. Kích thước bảng là cố định nên kích thước đầu vào chỉ ảnh hưởng đến số lượng ca kiểm thử. Giới hạn lên tới 10000 trường hợp thử nghiệm có nghĩa là một giải pháp sẽ dễ dàng xử lý vài trăm nghìn thao tác đơn giản, nhưng việc tìm kiếm khám phá tất cả các trò chơi có thể có hoặc nhiều chuỗi di chuyển sẽ không cần thiết. 

Bảng nhỏ là hạn chế chính. Vì chỉ có 64 ô vuông nên chúng ta không cần mô tả toán học phức tạp. Chúng ta có thể lập mô hình trực tiếp các quy tắc cờ vua và kiểm tra mọi nước đi đầu tiên có thể xảy ra. Một lượng công việc không đổi cho mỗi trường hợp thử nghiệm là đủ. 

Những trường hợp phức tạp là do tính hợp pháp của cờ vua chứ không phải do chuyển động đơn thuần. Vua của đối phương có thể tiếp cận được một quân Xe, nhưng việc bắt giữ có thể thất bại vì vua của chúng ta bảo vệ ô đó. Ví dụ:```
1
d2 e6 f1
```Câu trả lời là`NO`. Xe có thể di chuyển đến e2 hoặc e1, cả hai đều ở gần vua đối phương, nhưng vua đối phương không thể bắt được ở đó vì vua của chúng ta sẽ tấn công ô đích. Giải pháp bất cẩn chỉ kiểm tra khoảng cách giữa vua và xe sẽ trả lời sai`YES`. 

Một trường hợp khác là vua của chúng ta chặn quân xe. Ví dụ:```
1
d4 b4 f4
```Câu trả lời là`NO`. Xe không thể đi qua quân vua của ta nên dù vua đối phương có ở gần hàng của quân xe thì không có nước đi nào của quân xe hợp lệ có thể tạo ra vật hiến tế. 

Trường hợp lợi thế cuối cùng là hy sinh bằng cách di chuyển quân vua thay vì quân xe. Ví dụ:```
1
f2 g2 h1
```Câu trả lời là`YES`. Xe không di chuyển. Thay vào đó, vua chuyển sang f3, khiến quân xe có thể bị bắt. Chỉ kiểm tra nước đi của xe sẽ bỏ lỡ khả năng này. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là tạo ra mọi động thái hợp pháp mà chúng ta có thể thực hiện. Đối với mỗi nước đi vua và nước đi có thể xảy ra, chúng tôi mô phỏng vị trí kết quả. Nếu vua đối phương bắt được quân ở vị trí đó thì chúng ta chấp nhận. 

Lực lượng vũ phu này đã nhanh chóng vì bàn cờ không bao giờ phát triển. Có nhiều nhất 64 ô đích cho quân vua và nhiều nhất 64 ô đích cho quân xe. Đối với mỗi ứng cử viên, chúng tôi chỉ cần một vài lần kiểm tra thời gian liên tục để tìm các cuộc tấn công và các ô bị chiếm đóng. Trường hợp xấu nhất là khoảng 10000 lần 128 bước di chuyển của ứng viên, rất nhỏ. 

Một cách tiếp cận phức tạp hơn sẽ cố gắng rút ra các công thức về vị trí tương đối của các mảnh. Điều đó có thể hiệu quả, nhưng rất dễ bỏ lỡ các quy tắc đặc biệt chẳng hạn như vua chặn quân xe hoặc vua đối phương từ chối bắt giữ vì ô được phòng thủ. Kích thước bảng cố định giúp mô phỏng trở thành giải pháp rõ ràng hơn. 

Lực lượng vũ phu hoạt động vì mọi động thái đầu tiên hợp pháp đều được thể hiện và kiểm tra. Nó sẽ thất bại trên một bảng lớn hơn vì việc liệt kê tất cả các ô vuông sẽ không còn cố định nữa. Ở đây, nhận xét rằng không gian trạng thái là cố định cho phép chúng ta thay thế lý luận cờ vua khó bằng xác minh hoàn chỉnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(T * 64 * 64) | O(1) | Đã chấp nhận | 
| Tối ưu | O(T) với hằng số lớn bị ẩn bởi kích thước bảng cố định | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mọi hình vuông từ một chữ cái và số thành tọa độ dựa trên số 0. Giữ mọi thứ dưới dạng cặp số nguyên giúp việc kiểm tra chuyển động dễ dàng hơn. 
2. Tạo mọi nước đi có thể cho vua của chúng ta. Nước đi của vua chỉ có hiệu lực nếu đích đến nằm trong bàn cờ, không chứa quân của chúng ta và quân vua không di chuyển vào ô bị vua hoặc quân của đối phương tấn công. 
3. Thực hiện mọi nước đi có thể cho quân của chúng ta. Xe có thể di chuyển theo chiều ngang hoặc chiều dọc cho đến khi chạm tới cạnh bàn cờ hoặc quân khác. Nó không thể nhảy qua vua của chúng ta hoặc vua của đối phương. 
4. Sau mỗi nước đi của ứng cử viên, hãy kiểm tra xem vua của đối phương có thể bắt quân xe một cách hợp pháp hay không. Vua đối phương phải ở cạnh quân xe, ô đích không được vua của chúng ta bảo vệ và ô đó không được được quân của chúng ta bảo vệ sau khi nước đi. 
5. Nếu bất kỳ nước đi nào của ứng cử viên vượt qua quá trình kiểm tra bắt giữ, hãy xuất ra`YES`. Nếu tất cả các bước di chuyển đều thất bại, xuất ra`NO`. 

Lý do điều này có tác dụng là vì mọi động thái đầu tiên có thể xảy ra đều được kiểm tra. Có thể hy sinh chính xác khi tồn tại ít nhất một nước đi hợp pháp mà sau đó vua đối phương có quyền bắt hợp pháp. Vì việc liệt kê bao gồm tất cả các nước đi hợp pháp, việc thất bại ở mọi nước đi được kiểm tra chứng tỏ rằng không có sự hy sinh nào tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def inside(x, y):
    return 0 <= x < 8 and 0 <= y < 8

def king_attack(a, b):
    return max(abs(a[0] - b[0]), abs(a[1] - b[1])) == 1

def rook_attack(r, target, king):
    if r[0] == target[0]:
        step = 1 if target[1] > r[1] else -1
        y = r[1] + step
        while y != target[1]:
            if (r[0], y) == king:
                return False
            y += step
        return True
    if r[1] == target[1]:
        step = 1 if target[0] > r[0] else -1
        x = r[0] + step
        while x != target[0]:
            if (x, r[1]) == king:
                return False
            x += step
        return True
    return False

def can_capture(k, r, enemy):
    if not king_attack(enemy, r):
        return False
    if king_attack(k, r):
        return False
    if rook_attack(r, enemy, k):
        return False
    return True

def legal_king_moves(k, r, enemy):
    res = []
    for dx in (-1, 0, 1):
        for dy in (-1, 0, 1):
            if dx == 0 and dy == 0:
                continue
            nx, ny = k[0] + dx, k[1] + dy
            if not inside(nx, ny):
                continue
            if (nx, ny) == r:
                continue
            nk = (nx, ny)
            if king_attack(enemy, nk):
                continue
            if rook_attack(r, nk, nk):
                continue
            res.append(nk)
    return res

def legal_rook_moves(k, r, enemy):
    res = []
    for dx, dy in ((1, 0), (-1, 0), (0, 1), (0, -1)):
        x, y = r
        while True:
            x += dx
            y += dy
            if not inside(x, y):
                break
            if (x, y) == k:
                break
            if (x, y) == enemy:
                break
            res.append((x, y))
    return res

def solve_case(k, r, e):
    for nk in legal_king_moves(k, r, e):
        if can_capture(nk, r, e):
            return True
    for nr in legal_rook_moves(k, r, e):
        if can_capture(k, nr, e):
            return True
    return False

def main():
    t = int(input())
    ans = []
    for _ in range(t):
        a, b, c = input().split()

        def conv(s):
            return (ord(s[0]) - ord('a'), int(s[1]) - 1)

        k = conv(a)
        r = conv(b)
        e = conv(c)

        ans.append("YES" if solve_case(k, r, e) else "NO")

    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Các hàm trợ giúp tách biệt các quy tắc cờ vua.`king_attack`xử lý vùng lân cận, đó là cách duy nhất để vua kiểm soát một hình vuông.`rook_attack`kiểm tra xem hai ô có chung một hàng hay một cột hay không và xác minh rằng đường của quân xe không bị chặn. 

Kiểm tra chụp được tách biệt khỏi việc tạo di chuyển. Điều này ngăn ngừa một lỗi phổ biến trong đó một nước đi được coi là thành công chỉ vì vua đối phương ở gần quân xe. Vua của đối phương cũng phải được phép đứng trên ô của quân xe. 

Hai bộ tạo nước đi bao gồm những lựa chọn duy nhất mà chúng ta có: di chuyển quân vua hoặc di chuyển quân xe. Quá trình quét quân xe dừng lại khi đến quân khác, quân này xử lý quy tắc chặn. Vì tọa độ nằm trong khoảng từ 0 đến 7 nên không có vấn đề tràn hoặc đầu vào lớn. 

## Ví dụ đã hoạt động 

Đối với vị trí mẫu đầu tiên:```
c5 e2 f4
```Tọa độ được chuyển đổi là vua`(2,4)`, tân binh`(4,1)`, kẻ thù`(5,3)`. 

| Bước | Vua | Xe | Hành động | Kẻ địch có thể bắt được không? | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | (2,4) | (4,1) | Kiểm tra di chuyển | Không | 
| Hãy thử rook tới e4 | (2,4) | (4,4) | Xe di chuyển | Có | 

Nước đi của quân xe đặt quân xe cạnh vua đối phương. Vua đối phương không di chuyển vào cuộc tấn công của vua chúng ta, vì vậy vật hiến tế có tác dụng. 

Đối với vị trí mẫu thứ ba:```
f2 g2 h1
```Nhà nước là vua`(5,1)`, tân binh`(6,1)`, kẻ thù`(7,0)`. 

| Bước | Vua | Xe | Hành động | Kẻ địch có thể bắt được không? | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | (5,1) | (6,1) | Xe không thể hy sinh | Không | 
| Chuyển vua sang f3 | (5,2) | (6,1) | Vua chuyển đi | Có | 

Dấu vết này cho thấy tại sao việc chỉ kiểm tra nước đi của quân xe là không đầy đủ. Xe đã ở đúng vị trí, nước đi của vua làm thay đổi cục diện phòng thủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T * 64 * 64) | Mọi bài kiểm tra đều kiểm tra số lượng nước đi không đổi có thể có trên bàn cờ 8 x 8 | 
| Không gian | O(1) | Chỉ một số tọa độ và danh sách di chuyển tạm thời được lưu trữ | 

Kích thước bảng là cố định nên hệ số hằng số nhỏ. Ngay cả với 10000 trường hợp thử nghiệm, số lượng thao tác vẫn thấp hơn nhiều so với giới hạn cuộc thi thông thường. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    
    def inside(x, y):
        return 0 <= x < 8 and 0 <= y < 8

    def king_attack(a, b):
        return max(abs(a[0] - b[0]), abs(a[1] - b[1])) == 1

    def rook_attack(r, target, king):
        if r[0] == target[0]:
            step = 1 if target[1] > r[1] else -1
            y = r[1] + step
            while y != target[1]:
                if (r[0], y) == king:
                    return False
                y += step
            return True
        if r[1] == target[1]:
            step = 1 if target[0] > r[0] else -1
            x = r[0] + step
            while x != target[0]:
                if (x, r[1]) == king:
                    return False
                x += step
            return True
        return False

    def can_capture(k, r, e):
        return king_attack(e, r) and not king_attack(k, r) and not rook_attack(r, e, k)

    def solve(k, r, e):
        for dx in (-1, 0, 1):
            for dy in (-1, 0, 1):
                if (dx, dy) != (0, 0):
                    nk = (k[0] + dx, k[1] + dy)
                    if inside(*nk) and nk != r and not king_attack(e, nk) and not rook_attack(r, nk, nk):
                        if can_capture(nk, r, e):
                            return "YES"
        for dx, dy in ((1,0),(-1,0),(0,1),(0,-1)):
            x, y = r
            while True:
                x += dx
                y += dy
                if not inside(x, y) or (x, y) == k or (x, y) == e:
                    break
                if can_capture(k, (x, y), e):
                    return "YES"
        return "NO"

    t = int(input())
    out = []
    for _ in range(t):
        a, b, c = input().split()
        f = lambda s: (ord(s[0])-97, int(s[1])-1)
        out.append(solve(f(a), f(b), f(c)))
    sys.stdin = old
    return "\n".join(out)

assert run("""6
c5 e2 f4
c3 d5 h4
f2 g2 h1
b4 d6 f3
d2 e6 f1
d4 b4 f4
""") == """YES
YES
YES
NO
NO
NO"""

assert run("""1
a1 b2 c3
""") == "NO"

assert run("""1
f2 g2 h1
""") == "YES"

assert run("""1
d4 b4 f4
""") == "NO"

assert run("""1
a1 h8 h7
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu chính thức | CÓ CÓ CÓ KHÔNG KHÔNG KHÔNG | Các trường hợp cờ vua thông thường | 
|`a1 b2 c3`| KHÔNG | Vua bảo vệ và xử lý góc | 
|`f2 g2 h1`| CÓ | Hy sinh bằng cách di chuyển nhà vua | 
|`d4 b4 f4`| KHÔNG | Xe bị vua mình chặn | 
|`a1 h8 h7`| CÓ | Chuyển động của quân cờ và quân xe | 

## Vỏ cạnh 

Đối với trường hợp xe được bảo vệ:```
1
d2 e6 f1
```Thuật toán cố gắng di chuyển quân xe để đặt nó gần vua đối phương. Khi kiểm tra việc chụp, nó thấy rằng ô đích được kiểm soát bởi vua của chúng ta. Vì vua đối phương không thể kiểm tra nên mọi hy sinh đều thất bại và câu trả lời là`NO`. 

Đối với trường hợp xe bị chặn:```
1
d4 b4 f4
```Trình tạo nước đi của quân xe quét từ quân xe ra ngoài. Khi nó đến vua của chúng ta ở d4, quá trình quét dừng lại, do đó không có hành động di chuyển bất hợp pháp nào qua vua được tạo ra. Thuật toán không bao giờ xem xét một nước đi mà quân Xe thực sự không thể thực hiện được. 

Đối với trường hợp nước đi vua:```
1
f2 g2 h1
```Xe đã ở gần vua đối phương nhưng vị trí vua hiện tại không cho phép bắt giữ. Việc di chuyển vua sẽ làm thay đổi các ô bị tấn công và làm cho quân xe có thể bị bắt, do đó thuật toán tìm ra nước đi hợp lệ và trả về`YES`.
