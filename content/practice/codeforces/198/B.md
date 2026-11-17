---
title: "CF 198B - Nhảy Tường"
description: "Chúng ta có hai bức tường thẳng đứng, mỗi bức tường được biểu thị bằng một sợi dây có độ dài n. Vị trí i trên tường là an toàn (-) hoặc bị chặn (X). Ninja bắt đầu ở vị trí 0 của bức tường bên trái. Mỗi giây anh ta có thể di chuyển đến một trong ba vị trí: 1. Một ô hướng lên trên cùng một bức tường. 2."
date: "2026-06-03T09:49:22+07:00"
tags: ["codeforces", "competitive-programming", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 198
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 125 (Div. 1)"
rating: 1400
weight: 198
solve_time_s: 97
verified: true
draft: false
---

[CF 198B - Nhảy tường](https://codeforces.com/problemset/problem/198/B) 

**Đánh giá:** 1400 
**Thẻ:** con đường ngắn nhất 
**Thời gian giải:** 1 phút 37 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai bức tường thẳng đứng, mỗi bức tường được biểu thị bằng một chuỗi dài`n`. Chức vụ`i`trên tường là an toàn (`-`) hoặc bị chặn (`X`). Ninja bắt đầu ở vị trí`0`của bức tường bên trái. 

Mỗi giây anh ta có thể di chuyển đến một trong ba vị trí: 

1. Một ô hướng lên trên cùng một bức tường. 
2. Một ô hướng xuống trên cùng một bức tường. 
3. Bức tường đối diện,`k`tế bào cao hơn. 

Vượt quá vị trí`n - 1`có nghĩa là thoát khỏi hẻm núi ngay lập tức. 

Sự phức tạp là nước dâng cao. Vào thời điểm`t`, tất cả các vị trí có chỉ số`<= t - 1`đã bị nhấn chìm rồi. Sau khi thực hiện`t`-lần thứ hai, nước lại dâng lên. Một vị trí chỉ có thể sử dụng được nếu nó chưa bị ngập. 

Nhiệm vụ là xác định xem có thể trốn thoát được hay không. 

Chiều cao của bức tường có thể đạt tới`100000`, điều này ngay lập tức loại trừ mọi tìm kiếm theo cấp số nhân. Ngay cả một thuật toán bậc hai cũng sẽ yêu cầu khoảng`10^10`trong trường hợp xấu nhất, vượt xa giới hạn. Chúng ta cần một cái gì đó gần với thời gian tuyến tính. 

Phần tinh tế nhất của vấn đề là sự tương tác giữa chuyển động và thời gian. Đạt đến cùng một vị trí ở những thời điểm khác nhau là không tương đương, vì nước liên tục loại bỏ những vị trí thấp hơn. 

Coi như:```
3 1
---
---
```Câu trả lời đúng là`YES`. Từ vị trí`0`, nhảy sang bức tường khác tại vị trí`1`, sau đó lại nhảy ra ngoài bức tường. 

Việc thực hiện bất cẩn chỉ kiểm tra các chướng ngại vật trên tường và bỏ qua thời gian sẽ có tác dụng ở đây, nhưng sai lầm tương tự đó sẽ không xảy ra trong trường hợp con đường duy nhất yêu cầu phải ghé thăm các vị trí đã bị ngập nước. 

Một sai lầm dễ mắc phải khác là cho phép di chuyển vào các vị trí đã ở dưới nước.```
4 2
----
----
```Giả sử chúng ta đang ở vị trí`2`vào thời điểm đó`3`. Chức vụ`1`đã bị ngập lụt nên việc di chuyển xuống đó là bất hợp pháp mặc dù nó đã được đánh dấu là an toàn. 

Điều kiện đúng không chỉ đơn thuần là "trong giới hạn và không bị chặn". Chỉ số đích cũng phải lớn hơn thời điểm hiện tại. 

Cái bẫy thứ ba là trốn thoát bằng cách nhảy.```
5 3
-----
-----
```Từ vị trí`3`, cú nhảy chạm tới vị trí`6`, ở bên ngoài bức tường. Ninja trốn thoát ngay lập tức. Một số triển khai từ chối không chính xác các bước di chuyển có chỉ số đích vượt quá`n - 1`, thiếu lối thoát hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực là mô hình hóa mọi chuỗi chuyển động có thể có. Từ mỗi tiểu bang, chúng tôi cố gắng leo lên, leo xuống và nhảy qua. Vì các vị trí giống nhau có thể được xem lại qua nhiều con đường khác nhau nên số lượng chuỗi di chuyển tăng theo cấp số nhân theo thời gian. Ngay cả đối với chiều cao tường vừa phải, điều này trở nên không khả thi. 

Quan sát quan trọng là trò chơi tạo thành một biểu đồ một cách tự nhiên. Một trạng thái hoàn toàn được xác định bởi hai thông tin: chúng ta đang ở trên bức tường nào và chúng ta chiếm giữ vị trí nào. Thời gian không cần phải được lưu trữ riêng vì mỗi lần di chuyển tốn đúng một giây. Trong tìm kiếm theo chiều rộng, khoảng cách từ điểm bắt đầu chính xác là thời gian trôi qua khi chúng ta đạt đến trạng thái. 

Khi chúng tôi biết thời gian đến của một tiểu bang, chúng tôi có thể quyết định liệu việc di chuyển có hợp pháp hay không. Vị trí đích đến phải an toàn, không có người ghé thăm và vẫn ở trên mực nước tại thời điểm đến. 

Điều này biến vấn đề thành tìm kiếm đường đi ngắn nhất`2n`tiểu bang. Mỗi trạng thái có tối đa ba cạnh đi ra, do đó toàn bộ đồ thị có thể được khám phá theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| BFS tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Cất giữ hai dây treo tường. 
2. Biểu diễn một trạng thái dưới dạng`(wall, position)`Ở đâu`wall`là`0`cho bức tường bên trái và`1`cho bức tường bên phải. 
3. Bắt đầu BFS từ`(0, 0)`vào thời điểm đó`0`. 
4. Khi một trạng thái`(wall, pos)`bị xóa khỏi hàng đợi, khoảng cách BFS của nó là thời gian hiện tại`t`. 
5. Tạo ba bước di chuyển có thể: 

-`(wall, pos + 1)`-`(wall, pos - 1)`-`(1 - wall, pos + k)`6. Nếu bất kỳ chỉ số đích nào ít nhất`n`, ninja trốn thoát ngay lập tức và chúng tôi in`YES`. 
7. Đối với các điểm đến bên trong bức tường: 

- Phòng giam phải an toàn (`'-'`). 
- Bang này chắc chắn chưa từng đến thăm trước đây. 
- Chỉ số đích phải lớn hơn`t + 1`. 

Điều kiện cuối cùng mô hình nước. Sau khi thực hiện bước đi tiếp theo, thời gian sẽ trở thành`t + 1`. Bất kỳ vị trí nào có chỉ số nhiều nhất`t + 1`đã bị ngập lụt và không thể chiếm được. 
8. Mọi điểm đến hợp lệ sẽ được thêm vào hàng đợi BFS với khoảng cách`t + 1`. 
9. Nếu BFS kết thúc mà không thoát, hãy in`NO`. 

### Tại sao nó hoạt động 

BFS khám phá các trạng thái theo thứ tự thời gian trôi qua tăng dần. Khi một trạng thái được truy cập lần đầu tiên, chúng ta đã tìm được thời gian đến sớm nhất có thể của trạng thái đó. 

Việc hạn chế nước chỉ phụ thuộc vào thời gian đến. Nếu một vị trí không an toàn khi đến vào thời điểm nó đến sớm nhất thì việc đến được vị trí đó muộn hơn cũng không thể giúp ích gì vì mực nước chỉ dâng cao. Vì tính chất đơn điệu này nên việc đến thăm mỗi tiểu bang một lần là đủ. 

Mọi chuỗi di chuyển hợp pháp đều tương ứng với một đường dẫn được BFS khám phá và mọi chuyển đổi BFS đều tương ứng với một bước di chuyển hợp pháp trong trò chơi. Việc trốn thoát được báo cáo chính xác khi một nước đi rời khỏi bức tường, do đó thuật toán sẽ quay trở lại`YES`nếu và chỉ khi tồn tại một lối thoát hợp lệ. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    walls = [input().strip(), input().strip()]

    visited = [[False] * n for _ in range(2)]
    visited[0][0] = True

    q = deque()
    q.append((0, 0, 0))  # wall, position, time

    while q:
        wall, pos, t = q.popleft()

        moves = [
            (wall, pos + 1),
            (wall, pos - 1),
            (1 - wall, pos + k)
        ]

        for nw, np in moves:
            if np >= n:
                print("YES")
                return

            if np < 0:
                continue

            if np <= t + 1:
                continue

            if walls[nw][np] == 'X':
                continue

            if visited[nw][np]:
                continue

            visited[nw][np] = True
            q.append((nw, np, t + 1))

    print("NO")

solve()
```Hàng đợi lưu trữ bức tường hiện tại, vị trí và thời gian đã trôi qua. BFS đảm bảo rằng các trạng thái được xử lý theo thứ tự thời gian tăng dần. 

điều kiện`np <= t + 1`là dòng quan trọng nhất trong giải pháp. Chúng tôi đang xem xét một động thái đến sau một giây. Tại thời điểm đó, các vị trí có chỉ số nhiều nhất`t + 1`đã biến mất dưới nước nên những hành động như vậy là bất hợp pháp. 

Việc thoát ra được kiểm tra trước khi xác nhận chướng ngại vật vì rời khỏi bức tường ngay lập tức sẽ thắng trò chơi. Một cú nhảy vượt qua đỉnh sẽ thành công bất kể điều gì sẽ tồn tại ở đó. 

Mảng đã truy cập ngăn cản việc xem lại các trạng thái. Vì BFS đến mọi trạng thái vào thời điểm sớm nhất có thể nên những người đến muộn hơn không thể tạo ra bất kỳ lợi thế nào. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
7 3
---X--X
-X--XX-
```| Thời gian | Tiểu bang | Di chuyển đã chọn | Kết quả | 
| --- | --- | --- | --- | 
| 0 | (L,0) | Nhảy | (R,3) | 
| 1 | (R,3) | Xuống | (R,2) | 
| 2 | (R,2) | Nhảy | (L,5) | 
| 3 | (L,5) | Nhảy | Vị trí 8 | 
| 4 | Thoát | - | CÓ | 

Dấu vết này cho thấy tại sao việc nhảy qua tường là điều cần thiết. Đường leo thẳng bị chặn bởi các ô nguy hiểm, nhưng các bức tường xen kẽ sẽ tạo ra đường lên đỉnh. 

### Ví dụ thứ hai```
5 1
--XXX
XXXXX
```| Thời gian | Tiểu bang | Các quốc gia có thể tiếp cận | 
| --- | --- | --- | 
| 0 | (L,0) | (L,1) | 
| 1 | (L,1) | không | 
| Kết thúc | - | BFS cạn kiệt | 

Đầu ra:```
NO
```Bức tường bên phải bị chặn hoàn toàn và việc leo xa hơn trên bức tường bên trái cuối cùng sẽ gặp phải chướng ngại vật. BFS khám phá mọi trạng thái pháp lý và không tìm thấy lối thoát. 

Ví dụ chứng minh rằng lỗi được phát hiện một cách tự nhiên khi hàng đợi trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trong số`2n`tiểu bang được truy cập nhiều nhất một lần | 
| Không gian | O(n) | Xếp hàng và truy cập nhiều nhất vào cửa hàng mảng`2n`tiểu bang | 

Với`n ≤ 100000`, thuật toán chỉ xử lý vài trăm nghìn chuyển đổi trạng thái. Điều này thoải mái phù hợp trong giới hạn thời gian và chỉ sử dụng bộ nhớ tuyến tính. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n, k = map(int, input().split())
    walls = [input().strip(), input().strip()]

    visited = [[False] * n for _ in range(2)]
    visited[0][0] = True

    q = deque([(0, 0, 0)])

    while q:
        wall, pos, t = q.popleft()

        for nw, np in (
            (wall, pos + 1),
            (wall, pos - 1),
            (1 - wall, pos + k),
        ):
            if np >= n:
                return "YES"

            if np < 0:
                continue

            if np <= t + 1:
                continue

            if walls[nw][np] == 'X':
                continue

            if visited[nw][np]:
                continue

            visited[nw][np] = True
            q.append((nw, np, t + 1))

    return "NO"

# sample from statement
assert run("7 3\n---X--X\n-X--XX-\n") == "YES", "sample"

# minimum size
assert run("1 1\n-\n-\n") == "YES", "immediate jump escapes"

# blocked everywhere else
assert run("5 1\n--XXX\nXXXXX\n") == "NO", "no usable continuation"

# jump directly out
assert run("5 5\n-----\n-----\n") == "YES", "first move escapes"

# water prevents moving back
assert run("4 1\n----\nXXXX\n") == "NO", "cannot survive rising water"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / - / -`| CÓ | Ví dụ nhỏ nhất có thể | 
|`5 1 / --XXX / XXXXX`| KHÔNG | Ngõ cụt do chướng ngại vật | 
|`5 5 / ----- / -----`| CÓ | Thoát ngay lập tức bằng cách nhảy | 
|`4 1 / ---- / XXXX`| KHÔNG | Xử lý lũ lụt đúng cách | 

## Vỏ cạnh 

Hãy xem xét hẻm núi nhỏ nhất có thể:```
1 1
-
-
```BFS bắt đầu lúc`(0,0)`. Vị trí mục tiêu nhảy di chuyển`1`, ở bên ngoài bức tường. Thuật toán kiểm tra`np >= n`đầu tiên và ngay lập tức trở lại`YES`. Điều này xử lý việc thoát ngay lập tức một cách chính xác. 

Hãy xem xét trường hợp di chuyển xuống dưới sẽ đi vào vùng ngập nước:```
4 2
----
----
```Giả sử BFS đạt đến vị trí`3`vào thời điểm đó`2`. Di chuyển xuống sẽ nhắm mục tiêu vị trí`2`. Thời gian đến sẽ là`3`, và điều kiện`np <= t + 1`trở thành`2 <= 3`, do đó hành động này bị từ chối. Thuật toán không bao giờ cho phép đứng trên một ô ngập nước. 

Hãy xem xét một cú nhảy thoát khỏi hẻm núi:```
5 3
-----
-----
```Trạng thái ở vị trí`3`tạo ra một bước nhảy đến vị trí`6`. Từ`6 >= 5`, phát hiện thoát ngay lập tức. Không cố gắng tra cứu bức tường, tránh các lỗi ngoài giới hạn và mô hình hóa chính xác các quy tắc trò chơi. 

Cuối cùng, hãy xem xét việc xem lại một trạng thái sau:```
6 2
------
------
```Một vị trí có thể đạt được thông qua một số tuyến đường. BFS truy cập nó vào thời điểm sớm nhất có thể và đánh dấu nó đã truy cập. Bất kỳ ai đến muộn hơn sẽ phải đối mặt với mực nước bằng hoặc cao hơn, không bao giờ tạo ra cơ hội mới. Mảng đã truy cập sẽ loại bỏ dữ liệu khám phá dư thừa một cách an toàn trong khi vẫn đảm bảo tính chính xác.
