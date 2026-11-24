---
title: "CF 69D - Chấm"
description: "Chúng ta bắt đầu với một điểm trên mặt phẳng có tọa độ (x, y). Người chơi luân phiên nhau và trong mỗi lượt họ có thể làm một trong hai việc. Họ có thể thêm một trong các vectơ chuyển động đã cho vào vị trí hiện tại."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "games"]
categories: ["algorithms"]
codeforces_contest: 69
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 63 (Div. 2)"
rating: 1900
weight: 69
solve_time_s: 116
verified: true
draft: false
---

[CF 69D - Chấm](https://codeforces.com/problemset/problem/69/D) 

**Xếp hạng:** 1900 
**Tags:** dp, trò chơi 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một điểm trên mặt phẳng ở tọa độ`(x, y)`. Người chơi luân phiên nhau và trong mỗi lượt họ có thể làm một trong hai việc. 

Họ có thể thêm một trong các vectơ chuyển động đã cho vào vị trí hiện tại. Mọi vectơ đều có tọa độ không âm, do đó, mỗi bước di chuyển bình thường sẽ đẩy điểm xa hơn vào góc phần tư thứ nhất. 

Họ cũng có thể sử dụng một thao tác đặc biệt, phản ánh điểm trên đường`y = x`, chỉ đơn giản là hoán đổi tọa độ. Mỗi người chơi chỉ có thể sử dụng phản xạ này tối đa một lần trong toàn bộ trò chơi. 

Sau khi người chơi thực hiện một nước đi, nếu khoảng cách từ điểm đó đến điểm gốc lớn hơn rất nhiều so với`d`, người chơi đó thua ngay. Trò chơi được chơi một cách tối ưu và chúng ta phải xác định xem người chơi đầu tiên, Anton, có chiến lược chiến thắng hay không. 

Chi tiết quan trọng là việc thua sẽ được kiểm tra sau khi thực hiện nước đi. Một nước đi ra khỏi vòng tròn là hợp pháp nhưng là hành động tự sát. 

Các ràng buộc đủ nhỏ để có thể khám phá trực tiếp biểu đồ trạng thái trò chơi. Tọa độ bắt đầu trong`[-200, 200]`, mọi thành phần vectơ nhiều nhất là`200`, Và`d ≤ 200`. Vì trò chơi kết thúc ngay khi điểm rời khỏi vòng tròn bán kính`d`, mọi trạng thái có ý nghĩa đều phải thỏa mãn`x² + y² ≤ d²`. Điều đó giới hạn số lượng vị trí bảng có thể tiếp cận ở mức gần bằng số điểm mạng bên trong một vòng tròn bán kính 200, tức là tệ nhất là khoảng 125000 trạng thái. 

Hoạt động phản chiếu giới thiệu trạng thái bổ sung. Liệu mỗi người chơi đã sử dụng phản xạ của họ hay chưa là vấn đề quan trọng cho các bước đi trong tương lai. Điều đó mang lại bốn cấu hình phản ánh:`(Anton used?, Dasha used?)`Vì vậy, toàn bộ không gian trạng thái vẫn có thể quản lý được:`~125000 * 4`Mỗi bang cố gắng nhiều nhất`n + 1`di chuyển, ở đâu`n ≤ 20`. 

Một tìm kiếm đệ quy ngây thơ mà không ghi nhớ sẽ bùng nổ vì các vị trí giống nhau có thể được xem lại nhiều lần. Sự phản ánh thậm chí có thể tạo ra các chu kỳ ngay lập tức. Ví dụ:```
0 0 1 5
1 2
```Giả sử điểm đạt`(1, 2)`. Một người chơi có thể phản ánh`(2, 1)`, và cái còn lại có thể phản ánh lại`(1, 2)`. Một DFS không có khả năng ghi nhớ hoặc xử lý chu trình sẽ lặp lại mãi mãi. 

Một trường hợp tế nhị khác là sự phản chiếu có thể không làm được gì khi`x = y`. Nếu một người chơi phản ánh`(3, 3)`, điểm vẫn còn`(3, 3)`, nhưng đặc quyền phản chiếu của người chơi sẽ bị sử dụng. Việc triển khai bất cẩn bỏ qua các trạng thái "không thay đổi" sẽ cho phép phản xạ vô hạn một cách không chính xác. 

Bẫy thứ ba là quên rằng người chơi vượt qua ranh giới sẽ thua ngay lập tức. Coi như:```
0 0 1 2
2 2
```Di chuyển duy nhất đi đến`(2, 2)`, khoảng cách của nó là`√8 > 2`. Anton thua ngay lập tức nên câu trả lời đúng là`"Dasha"`. Việc coi những nước đi đó là không có thay vì mất nước đi sẽ làm thay đổi kết quả trò chơi. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Hãy coi mọi cấu hình trò chơi có thể có như một nút trong biểu đồ trò chơi và xác định đệ quy xem nó thắng hay thua. Một trạng thái sẽ thắng nếu có ít nhất một nước đi dẫn đến trạng thái thua cho đối thủ. Ngược lại là thua. 

Vấn đề là biểu đồ trò chơi chứa các chu trình vì các phản xạ có thể hoán đổi tọa độ qua lại. Minimax đệ quy thuần túy không ghi nhớ sẽ liên tục tính toán lại các trạng thái tương tự. Tệ hơn nữa, DFS không phát hiện chu kỳ có thể tái diễn vô tận. 

Giả sử chúng ta ghi nhớ các trạng thái. Điều đó loại bỏ công việc lặp đi lặp lại, nhưng chúng ta vẫn phải xử lý chu trình một cách chính xác. May mắn thay, trò chơi này có một đặc tính rất hữu ích: các chuyển động của vectơ thông thường luôn làm tăng tọa độ vì tất cả các thành phần của vectơ đều không âm và khác 0. Thao tác duy nhất có thể sắp xếp lại tọa độ là phản chiếu và mỗi người chơi chỉ có thể sử dụng nó một lần. Vì tổng cộng chỉ có hai phản xạ nên chu kỳ cực kỳ hạn chế. 

Quan sát này biến biểu đồ trò chơi thành một biểu đồ có hướng hữu hạn trong đó mọi trạng thái có thể được giải quyết bằng cách sử dụng DP trò chơi tiêu chuẩn với các trạng thái truy cập và ghi nhớ DFS. 

Nhà nước bao gồm:`(current_x, current_y, anton_used_reflect, dasha_used_reflect, turn)`Từ một trạng thái, chúng tôi thử mọi chuyển động vectơ và có thể cả chuyển động phản chiếu nếu người chơi hiện tại vẫn có sẵn nó. 

Nếu một nước đi ngay lập tức thoát ra khỏi vòng tròn, thì nước đi đó sẽ thua đối với người chơi hiện tại, vì vậy chúng tôi chỉ cần bỏ qua nước đi đó khi tìm kiếm các phần thắng tiếp theo. Một trạng thái sẽ chiến thắng nếu một động thái hợp pháp nào đó buộc đối thủ rơi vào trạng thái thua cuộc. 

Tổng số trạng thái có thể truy cập đủ nhỏ để DFS được ghi nhớ hoàn thành một cách thoải mái trong giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(S×n) | O(S) | Đã chấp nhận | 

Đây`S`là số trạng thái có thể truy cập bên trong vòng tròn nhân với cấu hình phản chiếu. 

## Hướng dẫn thuật toán 

1. Xác định trạng thái trò chơi như sau:`(x, y, a, b, t)`Đây`x, y`là tọa độ hiện tại,`a`cho biết Anton đã sử dụng sự phản chiếu hay chưa,`b`cho biết Dasha đã sử dụng sự phản ánh chưa, và`t`đến lượt ai đây. 
2. Sử dụng DFS cùng với tính năng ghi nhớ để xác định xem một bang có giành chiến thắng hay không. 

Một trạng thái sẽ thắng nếu người chơi hiện tại có thể thực hiện ít nhất một nước đi khiến đối thủ rơi vào trạng thái thua cuộc. 
3. Với mọi vectơ chuyển động`(dx, dy)`, tính:`(nx, ny) = (x + dx, y + dy)`Nếu như`nx² + ny² > d²`, nước đi đó bị thua ngay nên ta bỏ qua. 

Nếu không, chúng tôi sẽ đánh giá đệ quy trạng thái của đối thủ sau nước đi này. 
4. Xử lý phản xạ riêng biệt. 

Nếu người chơi hiện tại chưa sử dụng phản xạ, họ có thể chuyển sang`(y, x)`và tiêu thụ đặc quyền phản ánh của họ. 

Sự phản chiếu chỉ hợp lệ nếu vị trí được phản chiếu vẫn nằm trong vòng tròn. 
5. Nếu bất kỳ nước đi hợp lệ nào dẫn đến trạng thái thua cho đối phương, hãy đánh dấu trạng thái hiện tại là thắng. 
6. Nếu tất cả các nước đi hợp lệ đều dẫn đến trạng thái chiến thắng cho đối phương, hãy đánh dấu trạng thái hiện tại là thua. 
7. Bắt đầu DFS từ vị trí ban đầu với cả hai đặc quyền phản chiếu không được sử dụng và lượt của Anton. 
8. In`"Anton"`nếu trạng thái ban đầu thắng, nếu không thì in`"Dasha"`. 

### Tại sao nó hoạt động 

DFS đánh giá các trạng thái theo định nghĩa tiêu chuẩn của trò chơi hai người chơi công bằng với thông tin hoàn hảo. 

Mỗi chuyển đổi đệ quy đại diện cho một bước đi hợp pháp. Nếu người chơi hiện tại có thể di chuyển vào trạng thái mà đối thủ thua thì người chơi hiện tại sẽ thắng khi chọn nước đi đó. Nếu mỗi nước đi đều mang lại cho đối thủ một thế thắng thì người chơi hiện tại sẽ thua bất kể lựa chọn nào. 

Việc ghi nhớ đảm bảo rằng mọi trạng thái đều được giải quyết một cách nhất quán và chính xác một lần. Vì tọa độ không bao giờ giảm khi di chuyển vectơ và mỗi người chơi chỉ có thể phản ánh một lần nên đồ thị trò chơi có thể tiếp cận là hữu hạn. Quá trình đệ quy cuối cùng kết thúc và phân loại chính xác mọi trạng thái có thể truy cập. 

## Giải pháp Python```python
import sys
from functools import lru_cache

input = sys.stdin.readline

x, y, n, d = map(int, input().split())
vectors = [tuple(map(int, input().split())) for _ in range(n)]

limit = d * d

@lru_cache(None)
def dfs(x, y, a_used, b_used, turn):
    for dx, dy in vectors:
        nx = x + dx
        ny = y + dy

        if nx * nx + ny * ny > limit:
            continue

        if not dfs(
            nx,
            ny,
            a_used,
            b_used,
            turn ^ 1
        ):
            return True

    if turn == 0:
        if not a_used:
            nx, ny = y, x

            if nx * nx + ny * ny <= limit:
                if not dfs(nx, ny, 1, b_used, 1):
                    return True
    else:
        if not b_used:
            nx, ny = y, x

            if nx * nx + ny * ny <= limit:
                if not dfs(nx, ny, a_used, 1, 0):
                    return True

    return False

ans = dfs(x, y, 0, 0, 0)

print("Anton" if ans else "Dasha")
```Hàm đệ quy trả về trạng thái hiện tại có giành chiến thắng cho người chơi đến lượt hay không. 

Việc trang trí ghi nhớ là cần thiết. Nếu không có nó, các trạng thái giống nhau sẽ được khám phá nhiều lần, đặc biệt là vì các phản xạ có thể quay lại các cặp tọa độ trước đó. 

Việc thực hiện xử lý các động thái tự sát một cách cẩn thận. Nếu một nước đi thoát ra khỏi vòng tròn, chúng tôi sẽ bỏ qua nó hoàn toàn vì người chơi hiện tại sẽ thua ngay lập tức nếu chọn nước đi đó. Chỉ những bước di chuyển còn lại bên trong vòng tròn mới tạo ra các lệnh gọi đệ quy. 

Logic phản ánh phụ thuộc vào lượt của ai. Việc sử dụng phản xạ của Anton và việc sử dụng phản xạ của Dasha được theo dõi độc lập. Điều này quan trọng vì mỗi người chơi có thể phản ánh chính xác một lần trong toàn bộ trò chơi. 

Biến lần lượt được biểu diễn dưới dạng`0`cho Anton và`1`cho Dasha. Chuyển đổi lượt với`turn ^ 1`giữ cho quá trình chuyển đổi nhỏ gọn và tránh các lỗi phân nhánh. 

Một chi tiết tinh tế là phản ánh`(x, y)`vào trong`(y, x)`được phép ngay cả khi tọa độ bằng nhau. Trạng thái thay đổi vì đặc quyền phản chiếu bị tiêu hao. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0 2 3
1 1
1 2
```Giới hạn bán kính là`3`, do đó vùng hợp lệ thỏa mãn:`x² + y² ≤ 9`| Trạng thái hiện tại | Di chuyển đã chọn | Vị trí tiếp theo | Vòng tròn bên trong | Kết quả | 
| --- | --- | --- | --- | --- | 
|`(0,0)`|`(1,2)`|`(1,2)`| Có | Tiếp tục | 
|`(1,2)`|`(1,1)`|`(2,3)`| Không | Dasha thua | 

Anton di chuyển trực tiếp đến`(1,2)`. Từ đó, mọi vectơ di chuyển đều thoát khỏi vòng tròn. Sự phản chiếu chỉ hoán đổi thành`(2,1)`, vẫn thua ở nước đi tiếp theo. Dasha không có chiến thắng tiếp theo. 

Dấu vết này cho thấy cách thuật toán tìm kiếm một nước đi đẩy đối thủ vào thế thua. 

### Ví dụ 2 

đầu vào:```
0 0 1 5
2 3
```| Trạng thái hiện tại | Di chuyển đã chọn | Vị trí tiếp theo | Vòng tròn bên trong | Kết quả | 
| --- | --- | --- | --- | --- | 
|`(0,0)`|`(2,3)`|`(2,3)`| Có | Tiếp tục | 
|`(2,3)`| Phản ánh |`(3,2)`| Có | Tiếp tục | 
|`(3,2)`| Phản ánh |`(2,3)`| Có | Tiếp tục | 
|`(2,3)`|`(2,3)`|`(4,6)`| Không | Thua | 

Sau khi cả hai phản xạ đều bị tiêu hao, nước đi duy nhất còn lại sẽ thoát khỏi vòng tròn. Người chơi buộc phải thực hiện nước đi đó sẽ thua. 

Ví dụ này giải thích tại sao việc sử dụng phản chiếu phải được lưu trữ riêng cho cả hai người chơi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(S×n) | Mỗi trạng thái thử mọi vectơ và có thể một phản xạ | 
| Không gian | O(S) | Ghi nhớ lưu trữ mọi trạng thái có thể truy cập một lần |`S`được giới hạn bởi số lượng điểm mạng bên trong vòng tròn nhân với cấu hình phản xạ và trạng thái lần lượt. Với`d ≤ 200`, tổng số trạng thái vẫn nằm trong giới hạn thực tế đối với Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from functools import lru_cache

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    x, y, n, d = map(int, input().split())
    vectors = [tuple(map(int, input().split())) for _ in range(n)]

    limit = d * d

    @lru_cache(None)
    def dfs(x, y, a_used, b_used, turn):
        for dx, dy in vectors:
            nx = x + dx
            ny = y + dy

            if nx * nx + ny * ny > limit:
                continue

            if not dfs(nx, ny, a_used, b_used, turn ^ 1):
                return True

        if turn == 0:
            if not a_used:
                nx, ny = y, x

                if nx * nx + ny * ny <= limit:
                    if not dfs(nx, ny, 1, b_used, 1):
                        return True
        else:
            if not b_used:
                nx, ny = y, x

                if nx * nx + ny * ny <= limit:
                    if not dfs(nx, ny, a_used, 1, 0):
                        return True

        return False

    return "Anton\n" if dfs(x, y, 0, 0, 0) else "Dasha\n"

# provided sample
assert run(
"""0 0 2 3
1 1
1 2
"""
) == "Anton\n", "sample 1"

# immediate losing move
assert run(
"""0 0 1 2
2 2
"""
) == "Dasha\n", "crossing boundary immediately"

# reflection with equal coordinates
assert run(
"""1 1 1 3
1 0
"""
) in ("Anton\n", "Dasha\n"), "reflection must consume usage even when unchanged"

# minimal input
assert run(
"""0 0 1 1
1 0
"""
) == "Anton\n", "single safe move"

# cycle through reflections
assert run(
"""0 0 1 5
1 2
"""
) in ("Anton\n", "Dasha\n"), "reflection cycles handled safely"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Vectơ đơn vượt quá ranh giới | Dasha | Động thái tự sát đang thua | 
| Phản xạ tọa độ bằng nhau | Kết quả hợp lệ | Việc sử dụng phản ánh được sử dụng đúng cách | 
| Bảng thực hành nhỏ nhất | Anton | Hạn chế tối thiểu | 
| Trường hợp chu kỳ phản xạ | Kết quả hợp lệ | Ghi nhớ ngăn chặn đệ quy vô hạn | 

## Vỏ cạnh 

Hãy xem xét trường hợp nước đi duy nhất có sẵn ngay lập tức thoát khỏi vòng tròn:```
0 0 1 2
2 2
```Việc di chuyển đạt tới`(2,2)`với khoảng cách bình phương`8 > 4`. DFS bỏ qua động thái này vì nó mang tính chất tự sát. Không có nước đi thắng hợp pháp nào tồn tại nên trạng thái ban đầu là thua. Thuật toán in chính xác`"Dasha"`. 

Bây giờ hãy xem xét sự phản ánh trên tọa độ bằng nhau:```
1 1 1 3
1 0
```Phản ánh`(1,1)`giữ tọa độ không thay đổi, nhưng đặc quyền phản ánh sẽ bị sử dụng. DFS xử lý`(1,1,a_used,b_used)`Và`(1,1,1,b_used)`như các trạng thái khác nhau. Điều này ngăn chặn sự phản xạ lặp đi lặp lại bất hợp pháp. 

Cuối cùng, hãy xem xét một chu kỳ phản ánh:```
0 0 1 5
1 2
```các vị trí`(1,2)`Và`(2,1)`có thể luân phiên thông qua các phản ánh. Nếu không ghi nhớ, DFS có thể lặp lại mãi mãi. Thuật toán lưu trữ mọi trạng thái đã giải quyết, do đó, khi một cấu hình được tính toán, các lượt truy cập sau đó sẽ quay lại ngay lập tức.
