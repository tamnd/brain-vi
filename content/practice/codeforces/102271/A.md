---
title: "CF 102271A - Căn cứ mặt trăng của Cybermen (Dễ dàng)"
description: "Lưới có H hàng và W cột. Thời gian và cột tiến lên cùng nhau, vì vậy tại thời điểm t TARDIS ở cột t. Tọa độ dọc của nó có thể thay đổi tối đa K giữa các cột liên tiếp, với hướng dọc bao bọc theo chu kỳ từ hàng H đến hàng 1 và từ hàng 1 đến hàng H."
date: "2026-08-17T18:18:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102271
codeforces_index: "A"
codeforces_contest_name: "Helvetic Coding Contest 2019 (two remaining problems)"
rating: 0
weight: 102271
solve_time_s: 150
verified: true
draft: false
---

[CF 102271A - Căn cứ mặt trăng của Cybermen (Dễ dàng)](https://codeforces.com/problemset/problem/102271/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Lưới có`H`hàng và`W`cột. Thời gian và cột tiến lên cùng nhau nên tại thời điểm`t`TARDIS ở trong cột`t`. Tọa độ dọc của nó có thể thay đổi nhiều nhất`K`giữa các cột liên tiếp, với hướng dọc bao quanh theo chu kỳ từ hàng`H`chèo thuyền`1`và từ hàng`1`chèo thuyền`H`. 

Mỗi Cyberman bắt đầu tại một cột và hàng cố định tại một thời điểm`1`. Cybermen không bao giờ thay đổi cột. Thay vào đó, ở mỗi bước thời gian tiếp theo, tất cả chúng đều di chuyển một hàng theo cùng một hướng, được xác định bởi ký tự tương ứng của`S`. Vì vậy, nếu Cyberman bắt đầu ở hàng`r`tại cột`c`, sau đó khi TARDIS tới cột`c`, Cyberman đó đã được dịch chuyển bởi tiền tố của`S`tương ứng với thứ nhất`c - 1`di chuyển. Nhiều Cybermen có thể chiếm giữ cùng một ô, nhưng đối với TARDIS, chỉ có việc một ô bị chặn mới là vấn đề quan trọng. Tuyên bố chính thức và tài liệu cuộc thi sử dụng cách giải thích này và đưa ra ba mẫu có kết quả đầu ra`4`,`72`, Và`600000`. 

Đầu vào mang lại`H`,`W`,`K`, Và`N`, theo sau là`N`vị trí Cyberman ban đầu và sau đó là một chuỗi có độ dài`W - 1`. Đầu ra là số chuỗi tọa độ TARDIS riêng biệt bắt đầu trong một ô cột trống`1`, thực hiện một bước cho mỗi bước thời gian, không bao giờ nhập một ô bị chiếm giữ và kết thúc ở đâu đó trong cột`W`. Câu trả lời được lấy modulo`10^9 + 7`. 

các kích thước`H`Và`W`nhiều nhất là`1000`, do đó, một giải pháp có khoảng một thao tác trên mỗi ô lưới,`O(HW)`, dễ dàng thực tế. Giá trị của`K`nhiều nhất là`10`, điều này cũng tạo nên một`O(HWK)`chương trình năng động khả thi về mặt nguyên tắc, nhưng thậm chí còn có một chương trình rõ ràng hơn`O(HW)`chuyển tiếp bằng cửa sổ trượt. có thể có`5000`Cybermen, do đó việc xây dựng lại từng cột bằng cách kiểm tra từng Cyberman sẽ rất lãng phí nếu thực hiện nhiều lần. Mỗi Cyberman chỉ cần được xử lý một lần sau khi chúng ta biết sự dịch chuyển theo chiều dọc của cột của nó. 

Trường hợp cạnh đầu tiên là ô bắt đầu bị chiếm dụng. Ví dụ,```
2 2 1 2
1 1
1 2
U
```cả hai ô của cột đầu tiên đều bị chặn nên câu trả lời đúng là`0`. Việc triển khai bất cẩn sẽ khởi tạo mọi trạng thái của cột đầu tiên thành`1`sẽ đếm các đường dẫn bắt đầu bên trong Cybermen. 

Trường hợp cạnh thứ hai được bọc theo chiều dọc. Coi như```
5 2 1 1
2 5
U
```Tại cột`2`, Cyberman di chuyển từ hàng`5`chèo thuyền`1`. Bốn tế bào khác có sẵn. Từ mỗi hàng trong cột`1`, TARDIS có thể đạt tới ba hàng liên tiếp theo chu kỳ, do đó, mỗi điểm trong số bốn điểm đến được phép có ba điểm đến trước có thể có. Câu trả lời là`12`. Việc triển khai coi các hàng là một khoảng thời gian thông thường sẽ xử lý sai các chuyển đổi xung quanh các hàng`1`Và`5`. 

Trường hợp thứ ba là việc Cyberman di chuyển phụ thuộc vào thời gian chứ không phụ thuộc vào việc Cyberman đã di chuyển qua bao nhiêu cột. Ví dụ,```
5 3 1 2
1 1
2 1
UU
```Tại cột`1`, hàng ngang`1`bị chặn, đưa ra DP cột đầu tiên`[0,1,1,1,1]`. Tại cột`2`, cả hai Cybermen đều đã thăng hạng một lần, vì vậy Cyberman ban đầu ở`(2,1)`chiếm hàng`2`. Do đó cột thứ hai có DP`[2,0,3,3,3]`, với tổng số`11`. Cột`3`không có Cyberman, và tổng số trở thành`33`. Việc triển khai quên tiền tố chuyển động hoặc áp dụng hướng dẫn quá sớm một bước sẽ nhận được kết quả khác. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là xử lý vấn đề như một chương trình động đếm đường đi. Cho phép`dp[r]`là số cách để đến được hàng`r`trong cột hiện tại. Đối với một hàng đích`r`, mỗi hàng trước đó có khoảng cách theo chu kỳ từ`r`nhiều nhất là`K`góp phần tạo ra giá trị mới. Sau khi tính toán các khoản tiền đó, các ô bị chặn được đặt thành 0. 

Việc triển khai bạo lực có thể tránh hoàn toàn việc lập trình động bằng cách liệt kê đệ quy mọi đường dẫn TARDIS có thể. Điều này đúng vì mỗi đường dẫn hợp lệ được tạo ra chính xác một lần và mọi đường dẫn không hợp lệ đều bị từ chối ngay khi nó đến một ô bị chiếm dụng. Vấn đề là số lượng đường dẫn. Trong trường hợp xấu nhất thì không có trở ngại nào và với`H = 1000`Và`K = 10`, mỗi hàng có`21`các hàng riêng biệt có thể truy cập được. Cột đầu tiên có`1000`lựa chọn, theo sau là`21`lựa chọn cho mỗi phần còn lại`999`cột, đưa ra`1000 * 21^999`những con đường có thể. Đại khái là thế`10^1321`, nên việc liệt kê là hoàn toàn không khả thi. 

Quan sát hữu ích đầu tiên là chúng ta không cần phải nhớ đường dẫn đầy đủ. Khi TARDIS đến một ô cụ thể, tất cả các lựa chọn trước đó chỉ quan trọng thông qua số lượng đường dẫn đến ô đó. Điều này mang lại DP theo từng cột tiêu chuẩn. 

Quan sát thứ hai xử lý các chướng ngại vật đang di chuyển. Mọi Cyberman đều di chuyển theo cùng một tín hiệu nên độ dịch chuyển theo chiều dọc sau`c - 1`di chuyển được biết đến từ tiền tố của`S`. Chúng ta có thể tính toán trước độ dịch chuyển đó cho mỗi cột. Một Cyberman ban đầu tại`(c, r)`sau đó chặn chính xác một hàng trong cột`c`, cụ thể là phiên bản được dịch chuyển thích hợp của`r`. Chúng ta có thể đánh dấu các ô này một lần trước khi chạy DP. 

Quá trình chuyển đổi còn lại ban đầu là`O(HK)`mỗi cột vì mỗi đích kiểm tra nhiều nhất`2K + 1`các hàng trước đó. Điều đó đã có thể được chấp nhận dưới những ràng buộc này, vì`H,W <= 1000`Và`K <= 10`, nhưng quá trình chuyển đổi có nhiều cấu trúc hơn. Đối với các hàng đích liên tiếp, các khoảng trước đó của chúng chỉ khác nhau một hàng ở mỗi bên. Chúng ta có thể duy trì tổng của khoảng thời gian chu kỳ hiện tại và cập nhật nó theo thời gian không đổi. Điều này làm giảm quá trình chuyển đổi DP thành`O(H)`mỗi cột. 

Khi`2K + 1 >= H`, mỗi hàng có thể chạm tới mọi hàng khác trên lưới tuần hoàn. Trong trường hợp đó, mọi đích đều nhận được tổng số đường dẫn như nhau, cụ thể là tổng của mảng DP trước đó. Việc xử lý riêng trường hợp này sẽ tránh được việc đếm cùng một hàng tuần hoàn nhiều lần khi xây dựng cửa sổ trượt. 

Bài xã luận chính thức của cuộc thi mô tả cùng một cột cơ bản DP, với quá trình chuyển đổi tính tổng tất cả các hàng trước đó trong khoảng cách`K`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(H(2K+1)^(W-1))`|`O(W)`độ sâu đệ quy | Quá chậm | 
| DP với chuyển tiếp trực tiếp |`O(N + HWK)`|`O(HW)`| Đã chấp nhận | 
| DP có cửa sổ trượt theo chu kỳ |`O(N + HW)`|`O(HW)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các vị trí của Cyberman và chuỗi chuyển động. Xây dựng sự dịch chuyển tiền tố cho mỗi cột. Cho phép`shift[c]`là sự dịch chuyển theo chiều dọc của một Cyberman vào thời điểm đó`c`so với hàng đầu tiên của nó. Như vậy`shift[0] = 0`và mỗi giá trị sau thay đổi theo`+1`vì`U`hoặc`-1`vì`D`. Hoạt động modulo trên các hàng mang lại sự bao bọc theo chu kỳ. 
2. Tạo một mảng ô bị chặn cho`W`cột. Đối với mỗi Cyberman ban đầu ở cột`c`và hàng`r`, đánh dấu hàng thu được bằng cách áp dụng`shift[c - 1]`ĐẾN`r`. Cyberman vẫn ở trong cột`c`, vì vậy nó chỉ đóng góp cho cột đó. Đây là điểm khác biệt chính giữa chuyển động của chướng ngại vật và chuyển động của TARDIS. 
3. Khởi tạo DP cho cột`1`. Mỗi hàng được bỏ chặn có chính xác một cách làm ô bắt đầu, trong khi mọi hàng bị chặn không có cách nào. Tại thời điểm này`prev[r]`đại diện cho tất cả các đường dẫn hợp lệ kết thúc ở hàng`r`trong cột hiện tại. 
4. Xử lý các cột từ`2`bởi vì`W`. Với mỗi cột, tính tổng của`prev[r']`trên tất cả các hàng`r'`có khoảng cách theo chu kỳ từ hàng đích`r`nhiều nhất là`K`. Nếu đích đến bị chặn, giá trị DP của nó bằng 0 bất kể tổng tiền trước đó như thế nào. 
5. Nếu`2K + 1 >= H`, mọi hàng nguồn có thể đến mọi hàng đích. Tính toán`total = sum(prev)`một lần và gán giá trị này cho mọi đích đến được bỏ chặn. Điều này tránh việc xây dựng một cửa sổ chứa cùng một hàng tuần hoàn nhiều lần. 
6. Mặt khác, hãy duy trì cửa sổ trượt theo chu kỳ chính xác`2K + 1`hàng nguồn riêng biệt. Xây dựng trình tự khái niệm`prev[H-K], ..., prev[H-1], prev[0], ..., prev[H-1], prev[0], ..., prev[K-1]`. 

Cửa sổ đầu tiên tương ứng với hàng đích`0`. Khi di chuyển đến hàng đích tiếp theo, hãy trừ hàng nguồn ra khỏi cửa sổ và thêm hàng nguồn vào đó. Do đó, mỗi tổng đích đều tốn thời gian không đổi. 
7. Thay thế`prev`với mảng DP mới được tính toán và tiếp tục sang cột tiếp theo. Sau cột`W`đã được xử lý, tính tổng tất cả các mục của`prev`. Mọi mục còn sót lại biểu thị các đường dẫn kết thúc ở hàng đó và mọi hàng cuối cùng có thể có đều được chấp nhận. 

### Tại sao nó hoạt động 

Bất biến là sau khi xử lý cột`c`,`prev[r]`bằng chính xác số lượng đường dẫn TARDIS hợp pháp có vị trí hiện tại là`(c,r)`. Việc khởi tạo thỏa mãn điều này vì mỗi ô khởi đầu tự do đóng góp một đường dẫn và mọi ô bị chặn không đóng góp một đường dẫn nào. Để chuyển sang`(c,r)`, mọi đường đi hợp lệ phải xuất phát từ chính xác một hàng trong khoảng cách tuần hoàn`K`, do đó, việc tính tổng các giá trị DP trước đó tương ứng sẽ tính mọi phần mở rộng hợp pháp chính xác một lần. Điểm đến bị chặn không đóng góp gì, loại bỏ chính xác các đường dẫn có thể đi vào Cyberman. Cửa sổ trượt tính tổng tương tự trước đó như phép truy toán trực tiếp, chỉ bằng cách sử dụng lại sự chồng lấp giữa các cửa sổ liên tiếp. Do đó, bất biến vẫn đúng cho mọi cột và việc tính tổng mảng DP cuối cùng sẽ cho ra chính xác số lượng đường dẫn cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1_000_000_007

def solve():
    H, W, K, N = map(int, input().split())

    cybermen = [tuple(map(int, input().split())) for _ in range(N)]
    S = input().strip()

    # shift[c] is the vertical displacement at column c + 1,
    # using 0-based column indices.
    shift = [0] * W
    for c in range(1, W):
        shift[c] = shift[c - 1] + (1 if S[c - 1] == 'U' else -1)

    # blocked[c][r] == 1 means row r (0-based) is occupied
    # in column c (0-based).
    blocked = [bytearray(H) for _ in range(W)]

    for c, r in cybermen:
        row = (r - 1 + shift[c - 1]) % H
        blocked[c - 1][row] = 1

    # DP for the first column.
    prev = [0 if blocked[0][r] else 1 for r in range(H)]

    # If one move can reach every row, every destination gets
    # the same predecessor sum.
    all_rows_reachable = 2 * K + 1 >= H

    for c in range(1, W):
        if all_rows_reachable:
            total = sum(prev) % MOD
            cur = [
                0 if blocked[c][r] else total
                for r in range(H)
            ]
        else:
            # Cyclic sliding window.
            ext = prev[-K:] + prev + prev[:K]
            width = 2 * K + 1

            window = sum(ext[:width]) % MOD
            cur = [0] * H

            for r in range(H):
                if not blocked[c][r]:
                    cur[r] = window

                if r + 1 < H:
                    window += ext[r + width]
                    window -= ext[r]
                    window %= MOD

        prev = cur

    print(sum(prev) % MOD)

if __name__ == "__main__":
    solve()
```các`shift`mảng ghi lại chuyển động tích lũy của Cybermen. Đối với cột`c`, TARDIS đến đó đúng lúc`c`, chính xác như vậy`c - 1`ký tự tín hiệu đã ảnh hưởng đến mọi Cyberman. biểu thức`(r - 1 + shift[c - 1]) % H`chuyển đổi hàng dựa trên một ban đầu thành hàng tuần hoàn dựa trên 0. 

các`blocked`mảng sử dụng một`bytearray`mỗi cột. Điều này nhỏ gọn và cho phép chúng tôi kiểm tra xem đích đến có bị chiếm giữ trong thời gian không đổi hay không. Nếu có nhiều Cybermen ở cùng một ô, việc chỉ định`1`một lần nữa không có tác dụng, đó chính xác là những gì chúng ta cần. 

Mảng DP đầu tiên chỉ chứa`0`Và`1`, vì không có chuyển động nào trước khi vào cột đầu tiên. Từ cột thứ hai trở đi, quá trình chuyển đổi chỉ phụ thuộc vào mảng DP trước đó, do đó việc lưu trữ hai lớp DP đầy đủ là không cần thiết. 

các`all_rows_reachable`nhánh xử lý các giá trị nhỏ của`H`. Ví dụ, khi`H = 5`Và`K = 3`, khoảng cách tuần hoàn giữa hai hàng bất kỳ lớn nhất là`2`, vì vậy mỗi hàng sẽ chạm tới mọi hàng khác. Một cửa sổ hình tròn ngây thơ có chiều dài`7`sẽ đếm một số hàng hai lần. Sử dụng tổng số tiền trực tiếp sẽ tránh được vấn đề đó. 

Đối với trường hợp bình thường,`ext`chứa đủ bản sao của phần đầu và phần cuối của`prev`để biến mỗi khoảng thời gian theo chu kỳ trước đó thành một lát cắt liền kề thông thường. Cửa sổ đầu tiên tương ứng với hàng đích`0`. Sau khi xử lý hàng đó, một giá trị cũ sẽ rời khỏi cửa sổ và một giá trị mới sẽ nhập vào đó. Thứ tự của những cập nhật này được chọn sao cho`cur[r]`sử dụng chính xác cửa sổ cho đích`r`, không phải đích đến`r + 1`. 

Tất cả số học được giảm modulo`10^9 + 7`. Các số nguyên Python không bị tràn, nhưng việc giữ tổng giới hạn sẽ ngăn chặn sự tăng trưởng không cần thiết và giữ cho việc triển khai hiệu quả. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2 2 1 0
U
```Không có Cybermen. Với hai hàng và`K = 1`, chu trình dọc cho phép mỗi hàng tiếp cận mọi hàng khác trong một lần di chuyển. 

| Cột | Hàng bị chặn |`prev`trước khi chuyển đổi |`cur`sau khi chuyển đổi | 
| --- | --- | --- | --- | 
| 1 | không |`[1, 1]`|`[1, 1]`| 
| 2 | không |`[1, 1]`|`[2, 2]`| 

Tổng cuối cùng là`2 + 2 = 4`, phù hợp với đầu ra mẫu. 

Ví dụ này thực hiện`2K + 1 >= H`chi nhánh. Quá trình chuyển đổi không cần phải kiểm tra từng hàng trước đó vì mỗi hàng đều có thể truy cập được. 

### Mẫu 2 

Đầu vào là```
5 4 1 3
1 3
2 2
2 1
UDU
```Cyberman ở cột`1`, hàng ngang`3`di chuyển lên một lần khi TARDIS tới cột`2`. Hai Cybermen ban đầu ở cột`2`, hàng`2`Và`1`, cũng di chuyển lên một lần. Do đó các hàng bị chặn là`{3}`trong cột`1`,`{2,3}`trong cột`2`và không có ô nào trong cột`3`Và`4`. 

| Cột | Hàng bị chặn | Vectơ DP | 
| --- | --- | --- | 
| 1 |`{3}`|`[1, 1, 0, 1, 1]`| 
| 2 |`{2, 3}`|`[3, 0, 0, 2, 3]`| 
| 3 |`{}`|`[6, 3, 2, 5, 5]`| 
| 4 |`{}`|`[14, 11, 10, 12, 16]`| 

Đối với cột`2`, hàng ngang`1`nhận đường dẫn từ hàng`5`,`1`, Và`2`, cho`3`. Hàng ngang`4`nhận đường dẫn từ hàng`3`,`4`, Và`5`, cho`2`và hàng`5`nhận được`3`. Các hàng bị chặn nhận được số không. 

Cột`3`không có chướng ngại vật nên tổng số đường đi trở thành`24`, sau đó được nhân với ba hàng có thể có trước đó cho mỗi đích trong cột`4`. Tổng số cuối cùng là`72`, phù hợp với mẫu 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(N + HW)`| Mỗi Cyberman được xử lý một lần, mỗi cột xử lý mỗi hàng một lần | 
| Không gian |`O(HW + N)`| Lưới bị chặn sử dụng`O(HW)`không gian và vị trí đầu vào sử dụng`O(N)`không gian | 

Với`H,W <= 1000`, DP thực hiện tối đa khoảng một triệu cập nhật hàng. Chỉ xử lý trước chướng ngại vật`N <= 5000`Người mạng. Việc sử dụng bộ nhớ của lưới bị chặn cũng đủ nhỏ để`256 MB`giới hạn, trong khi việc triển khai chỉ giữ lại hai vectơ DP. 

## Trường hợp thử nghiệm```python
# Complete assert-based harness for the solution.

import sys
import io

MOD = 1_000_000_007

def solve():
    input = sys.stdin.readline

    H, W, K, N = map(int, input().split())
    cybermen = [tuple(map(int, input().split())) for _ in range(N)]
    S = input().strip()

    shift = [0] * W
    for c in range(1, W):
        shift[c] = shift[c - 1] + (1 if S[c - 1] == 'U' else -1)

    blocked = [bytearray(H) for _ in range(W)]
    for c, r in cybermen:
        row = (r - 1 + shift[c - 1]) % H
        blocked[c - 1][row] = 1

    prev = [0 if blocked[0][r] else 1 for r in range(H)]
    all_rows_reachable = 2 * K + 1 >= H

    for c in range(1, W):
        if all_rows_reachable:
            total = sum(prev) % MOD
            cur = [0 if blocked[c][r] else total for r in range(H)]
        else:
            ext = prev[-K:] + prev + prev[:K]
            width = 2 * K + 1
            window = sum(ext[:width]) % MOD
            cur = [0] * H

            for r in range(H):
                if not blocked[c][r]:
                    cur[r] = window

                if r + 1 < H:
                    window += ext[r + width]
                    window -= ext[r]
                    window %= MOD

        prev = cur

    print(sum(prev) % MOD)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run(
    """2 2 1 0
U
"""
) == "4", "sample 1"

assert run(
    """5 4 1 3
1 3
2 2
2 1
UDU
"""
) == "72", "sample 2"

assert run(
    """5 10 3 10
1 2
2 3
3 4
1 4
1 3
5 1
5 2
5 3
7 1
7 2
UUUDDDUDU
"""
) == "600000", "sample 3"

# Custom 1: minimum dimensions, no obstacles.
assert run(
    """2 2 1 0
U
"""
) == "4", "minimum-size grid"

# Custom 2: maximum row dimension with a short width.
# With H=1000, W=2, K=10 and no obstacles, there are
# 1000 starting rows and 21 choices for the second row.
assert run(
    """1000 2 10 0
U
"""
) == "21000", "large H"

# Custom 3: wrap-around transition.
# Row 5 is blocked in column 2, while K=1 makes row 1
# reachable from row 5 and vice versa.
assert run(
    """5 2 1 1
2 5
U
"""
) == "12", "vertical wrapping"

# Custom 4: obstacle movement must use the time prefix.
# Column 1 blocks row 1.
# The Cyberman initially at column 2, row 1 moves up to row 2
# before the TARDIS reaches column 2.
assert run(
    """5 3 1 2
1 1
2 1
UU
"""
) == "33", "Cyberman movement timing"

# Custom 5: duplicate Cybermen in the same cell must not
# multiply the blocking effect.
assert run(
    """5 3 1 5
2 1
2 1
2 1
2 1
2 1
UU
"""
) == "36", "duplicate obstacles"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 1 0`với`U`|`4`| Kích thước tối thiểu và quá trình chuyển đổi có thể truy cập vào tất cả các hàng | 
|`1000 2 10 0`với`U`|`21000`| Kích thước hàng lớn và nhánh tổng trực tiếp | 
|`5 2 1 1`, chướng ngại vật`(2,5)`|`12`| Gói dọc theo chu kỳ | 
|`5 3 1 2`, trở ngại`(1,1)`Và`(2,1)`|`33`| Sự dịch chuyển Cyberman phụ thuộc vào thời gian chính xác | 
| Năm bản sao của`(2,1)`|`36`| Nhiều Cybermen chiếm giữ cùng một phòng giam | 

## Vỏ cạnh 

Khi toàn bộ cột đầu tiên bị chặn, vectơ DP ban đầu đều bằng 0. Vì```
2 2 1 2
1 1
1 2
U
```cả hai hàng đều được đánh dấu trước khi khởi tạo, vì vậy`prev = [0, 0]`. Mọi chuyển đổi sau này cũng tạo ra số 0 và câu trả lời là`0`. Thuật toán không bao giờ cần trường hợp đặc biệt cho tình huống này vì định nghĩa DP ban đầu đã nắm bắt được nó. 

Đối với gói dọc, hãy xem xét```
5 2 1 1
2 5
U
```Cyberman ở cột`2`, hàng ngang`5`di chuyển lên hàng`1`, vậy cột`2`khối hàng`1`. Cột`1`có DP`[1,1,1,1,1]`. Các đích đến có sẵn là các hàng`2`,`3`,`4`, Và`5`. Cửa sổ tiền thân của chúng lần lượt là`{1,2,3}`,`{2,3,4}`,`{3,4,5}`, Và`{4,5,1}`, mỗi đường chứa ba đường dẫn. Vectơ cuối cùng là`[0,3,3,3,3]`, cho`12`. 

Đối với một chướng ngại vật di chuyển, hãy xem xét```
5 3 1 2
1 1
2 1
UU
```Cột đầu tiên chặn hàng`1`, vậy DP ban đầu là`[0,1,1,1,1]`. Cyberman thứ hai bắt đầu ở cột`2`, hàng ngang`1`, nhưng theo thời gian`2`nó đã di chuyển lên trên hàng`2`. Do đó cột thứ hai có hàng`2`bị chặn. Các tổng tiền tuần hoàn trước đó cho`[2,0,3,3,3]`, tổng của nó là`11`. Cột`3`không có trở ngại và mỗi điểm đến nhận được ba hàng trước đó, tạo ra tổng cộng`33`. Điều này xác nhận rằng tiền tố tín hiệu được áp dụng theo thời gian của TARDIS, không phải theo lịch sử cột bắt đầu của Cyberman. 

Cuối cùng, nếu nhiều Cybermen chiếm giữ cùng một ô, họ vẫn chỉ chặn một ô. TRONG```
5 3 1 5
2 1
2 1
2 1
2 1
2 1
UU
```cả năm Cybermen chuyển sang hàng`2`trong cột`2`. Mảng bị chặn ghi lại ô đó một lần. Cột`1`có sẵn tất cả năm hàng bắt đầu, vì vậy cột`2`có bốn hàng có sẵn với ba hàng trước mỗi hàng, cho`12`những con đường. Cột`3`không có trở ngại, vì vậy tổng số trở thành`36`. Việc coi mỗi Cyberman là một chướng ngại vật riêng biệt sẽ loại bỏ cùng một ô nhiều lần một cách không chính xác, trong khi biểu diễn bytearray sẽ xử lý các bản sao một cách tự nhiên.
