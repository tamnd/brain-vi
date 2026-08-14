---
title: "CF 102386G - \u0423\u0440\u0430\u043b\u044c\u0441\u043a\u0438\u0435 \u0431\u043b\u0438\u043d\u0447\u0438\u043a\u0438"
description: "Hãy coi mọi ô không bị cháy như một đỉnh của đồ thị. Hai đỉnh được kết nối khi các ô của chúng có chung một cạnh. Câu lệnh đảm bảo rằng biểu đồ này được kết nối."
date: "2026-08-12T21:44:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102386
codeforces_index: "G"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0442\u0443\u0440 \u0423\u0440\u0430\u043b\u044c\u0441\u043a\u043e\u0433\u043e \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b\u0430 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u043c\u0438\u0440\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2019"
rating: 0
weight: 102386
solve_time_s: 530
verified: true
draft: false
---

[CF 102386G - \u0423\u0440\u0430\u043b\u044c\u0441\u043a\u0438\u0435 \u0431\u043b\u0438\u043d\u0447\u0438\u043a\u0438](https://codeforces.com/problemset/problem/102386/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8 phút 50 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hãy coi mọi ô không bị cháy như một đỉnh của đồ thị. Hai đỉnh được kết nối khi các ô của chúng có chung một cạnh. Câu lệnh đảm bảo rằng biểu đồ này được kết nối. Mỗi chiếc bánh ban đầu chiếm một đỉnh riêng của nó và mỗi lần di chuyển sẽ đưa chiếc bánh trên cùng từ đỉnh này sang đỉnh liền kề. Một nước đi sẽ lật chiếc bánh đó nên sau một số nước đi lẻ nó sẽ được định hướng chính xác, trong khi sau một nước đi chẵn nó sẽ trở về hướng ban đầu. 

Ngăn xếp làm cho vấn đề trở nên lừa đảo. Một chiếc bánh có thể tạm thời ở chung một ô với những chiếc bánh khác, nhưng chỉ chiếc bánh trên cùng mới có thể di chuyển trở lại. Cuối cùng, mỗi chiếc bánh phải ở một mình và mỗi chiếc bánh phải di chuyển một số lần lẻ. 

Biểu đồ lưới là lưỡng cực. Tô màu ô màu đen khi tổng chỉ số hàng và cột của ô đó là số chẵn và nếu không thì tô màu trắng. Mỗi bước di chuyển đều diễn ra giữa các màu đối lập nhau. Do đó, một chiếc bánh kếp bắt đầu có màu đen và cuối cùng được định hướng chính xác phải kết thúc bằng màu trắng, và một chiếc bánh kếp bắt đầu có màu trắng phải kết thúc bằng màu đen. Các vị trí cuối cùng là khác nhau nên số lượng bánh kếp ban đầu trên màu đen không thể vượt quá số lượng ô trắng và về mặt đối xứng, số lượng bánh kếp ban đầu trên màu trắng không thể vượt quá số lượng ô đen. 

Với tối đa 100 hàng và 100 cột, có tối đa 10.000 ô có thể sử dụng được. Thuật toán đồ thị tuyến tính hoặc gần tuyến tính dễ dàng đủ nhanh cho giới hạn một giây. Một thuật toán phụ thuộc theo cấp số nhân vào số lượng bánh kếp là hoàn toàn không khả thi, vì số lượng bánh kếp tối đa cũng là 10.000. 

Có một trường hợp cạnh cấu trúc mà chỉ riêng điều kiện đếm màu đã bỏ sót. Coi như```
1 2
PP
```Có một ô đen và một ô trắng nên số lượng trông hoàn toàn cân bằng. Tuy nhiên câu trả lời là`NO`. Chỉ với hai ô liền kề, chiếc bánh nào được chuyển lên ô kia sẽ trở thành chiếc bánh trên cùng. Nó phải lùi lại thì mới có thể lấy được chiếc bánh bên dưới nên hai chiếc bánh không thể hoán đổi cho nhau. Tổng quát hơn, khi mọi ô có thể sử dụng đều bị chiếm dụng, chúng ta cần một chu trình trong biểu đồ ô có thể sử dụng để cung cấp bộ đệm thực sự. 

Một trường hợp dễ xử lý sai khác là mẫu```
1 3
P.P
```Hai chiếc bánh đều nằm trên các ô cùng màu, trong khi chỉ có một ô có màu đối diện. Cả hai chiếc bánh sẽ phải hoàn thành trên các ô có màu sắc đối lập riêng biệt, điều này là không thể. Câu trả lời là`NO`. 

Trường hợp cạnh cuối cùng là một chiếc bánh kếp. Ví dụ,```
1 2
P.
```có một chiếc bánh đen và một ô đích màu trắng, vậy câu trả lời là`YES`. Chiếc bánh chỉ cần di chuyển một lần. Bất kỳ giải pháp nào yêu cầu ít nhất hai chiếc bánh sẽ từ chối trường hợp này một cách không chính xác. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ coi toàn bộ sự sắp xếp như một trạng thái. Một tiểu bang phải ghi lại vị trí của mỗi chiếc bánh kếp, hướng của nó và thứ tự của những chiếc bánh bên trong mỗi chồng bánh. Từ mỗi trạng thái, chúng ta có thể thử di chuyển chiếc bánh kếp trên cùng của mọi ô không trống sang từng ô có thể sử dụng liền kề. Tìm kiếm theo chiều rộng sẽ chính xác vì mọi hoạt động hợp pháp được biểu thị bằng một chuyển đổi và việc đạt đến trạng thái có tất cả các bánh kếp được định hướng và phân tách chính xác chứng tỏ rằng một chuỗi hợp lệ tồn tại. 

Vấn đề là kích thước của không gian trạng thái đó. Nếu có (V) ô có thể sử dụng và (k) bánh kếp, chỉ cần chọn một ô cho mỗi ô được gắn nhãn sẽ mang lại khả năng (V^k). Các hướng nhân số này với (2^k) và các đơn hàng có thể có trong ngăn xếp sẽ thêm một thành phần có kích thước giai thừa khác. Ngay cả giới hạn trên nhỏ hơn nhiều (2^kV^k) cũng đã vô vọng đối với (k,V) khoảng 10.000. Lực lượng vũ phu rất hữu ích để hiểu các quy tắc, nhưng không hữu ích để giải quyết các ràng buộc thực tế. 

Quan sát quan trọng là trình tự di chuyển chính xác là không cần thiết. Mỗi lần di chuyển sẽ thay đổi cả màu vị trí và hướng của bánh kếp. Do đó, một chiếc bánh kếp bắt đầu bằng màu đen chỉ có thể đúng khi nó kết thúc bằng màu trắng và một chiếc bánh kếp bắt đầu bằng màu trắng chỉ có thể đúng khi nó kết thúc bằng màu đen. Điều này mang lại sự bất bình đẳng về công suất màu cần thiết ngay lập tức. 

Điều đáng ngạc nhiên là những bất đẳng thức này cũng đủ bất cứ khi nào có ít nhất một ô trống có thể sử dụng được. Vì biểu đồ khả dụng được kết nối nên ô trống đó có thể được truyền qua biểu đồ và được sử dụng làm vùng đệm. Ngăn xếp cho phép bánh kếp đi qua các đỉnh bị chiếm dụng trong khi bộ đệm được di chuyển xung quanh. Do đó, chúng ta có thể nhận ra sự phân phối lại bánh kếp cần thiết giữa hai lớp màu. 

Nếu mọi ô có thể sử dụng đều đã chứa một chiếc bánh kếp thì sẽ không có bộ đệm trống. Trong trường hợp đó một chu kỳ là cần thiết và đủ. Trong một chu kỳ, những chiếc bánh kếp có thể được dịch chuyển xung quanh nó, vì vậy mỗi chiếc bánh kếp trong chu kỳ có thể di chuyển một số lần lẻ mà không để lại một chồng bánh nào phía sau. Sau đó, chu trình đóng vai trò là không gian làm việc còn thiếu để sắp xếp lại phần còn lại của biểu đồ được kết nối. Một đồ thị liên thông không có chu trình là một cây và với mỗi đỉnh bị chiếm giữ thì không có cách nào để tạo một không gian làm việc cố định và thay đổi sự sắp xếp cần thiết. 

Do đó, toàn bộ vấn đề được rút gọn thành hai bước kiểm tra: khả năng màu lưỡng cực và chỉ khi mọi ô có thể sử dụng đều bị chiếm giữ thì biểu đồ ô có thể sử dụng có chứa chu trình hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^kV^k k!)) trong trường hợp xấu nhất | (O(2^kV^k k!)) | Quá chậm | 
| Tối ưu | (O(nm)) | (O(nm)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy đối xử với từng tế bào khác nhau`#`như một đỉnh của đồ thị có thể sử dụng. Tô màu đen hoặc trắng theo ((i+j)\bmod 2). Đây là một phép chia hai phần hợp lệ vì mỗi nước đi hợp lệ sẽ thay đổi chính xác một hàng hoặc một cột. 
2. Đếm tổng số ô đen trắng có thể sử dụng được và đếm riêng xem ban đầu có bao nhiêu chiếc bánh kếp chiếm các ô đen trắng. Nếu số lượng bánh đen lớn hơn số ô màu trắng có thể sử dụng được, hãy in`NO`. Nếu số lượng bánh trắng lớn hơn số ô màu đen có thể sử dụng được, hãy in`NO`. Mỗi chiếc bánh phải có màu đối lập và các ô cuối cùng không được chứa hai chiếc bánh. 
3. Đếm số ô có thể sử dụng và số bánh kếp. Nếu số lượng bánh nhỏ hơn số ô có thể sử dụng thì ít nhất một ô trống. Sau đó, biểu đồ được kết nối có thể sử dụng ô trống đó làm bộ đệm di chuyển, do đó sự bất bình đẳng về màu sắc là đủ. In`YES`. 
4. Nếu mỗi ô có thể sử dụng đều chứa một chiếc bánh kếp thì sự bất bình đẳng về màu sắc có nghĩa là hai lớp màu có kích thước bằng nhau. Bây giờ chúng ta cần xác định xem đồ thị có thể sử dụng được có chứa một chu trình hay không. Đếm mỗi cặp ô có thể sử dụng liền kề là cạnh vô hướng. Bởi vì đồ thị là liên thông nên nó chứa một chu trình chính xác khi số cạnh ít nhất bằng số đỉnh. 
5. Nếu đồ thị đầy đủ có thể sử dụng được chứa một chu trình, hãy in`YES`; nếu không thì in`NO`. Một đồ thị tuần hoàn được kết nối là một cái cây và khi mọi đỉnh đều bị chiếm thì không có ô trống hoặc chu trình nào có sẵn để sắp xếp lại các bánh kếp. 

Tại sao nó hoạt động: mọi nước đi hợp pháp đều đi qua phần chia đôi và lật chiếc bánh kếp, do đó, một chiếc bánh kếp được định hướng chính xác phải kết thúc có màu đối diện với ô bắt đầu của nó. Điều này chứng tỏ hai sự bất bình đẳng về năng lực là cần thiết. Khi tồn tại một ô trống, khả năng kết nối cho phép chúng tôi sử dụng nó làm bộ đệm trong khi di chuyển các bánh kếp qua biểu đồ và quy tắc ngăn xếp cho phép xung đột tạm thời. Khi không có ô trống, chu trình chính xác là cấu trúc cung cấp tuyến đệm kín. Cây không có tuyến đường như vậy, trong khi một chu trình cho phép xoay cấu hình đã sử dụng và các phần được kết nối còn lại được xử lý thông qua nó. Do đó, việc kiểm tra ở trên mô tả chính xác khi nào một chuỗi hợp lệ tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    cells = 0
    pancakes = 0

    black_cells = 0
    white_cells = 0
    black_pancakes = 0
    white_pancakes = 0

    edges = 0

    for i in range(n):
        for j in range(m):
            if grid[i][j] == '#':
                continue

            cells += 1

            if (i + j) % 2 == 0:
                black_cells += 1
            else:
                white_cells += 1

            if grid[i][j] == 'P':
                pancakes += 1
                if (i + j) % 2 == 0:
                    black_pancakes += 1
                else:
                    white_pancakes += 1

            if i + 1 < n and grid[i + 1][j] != '#':
                edges += 1
            if j + 1 < m and grid[i][j + 1] != '#':
                edges += 1

    if black_pancakes > white_cells:
        print("NO")
        return

    if white_pancakes > black_cells:
        print("NO")
        return

    if pancakes < cells:
        print("YES")
        return

    # Every usable cell is occupied.
    # The usable graph is connected, so it has a cycle iff E >= V.
    if edges >= cells:
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên phân loại mọi ô có thể sử dụng theo tính chẵn lẻ của tọa độ của nó. Việc sử dụng các chỉ số dựa trên số 0 không làm thay đổi phân vùng, vì việc thêm một vào cả hai tọa độ sẽ thay đổi tổng của chúng bằng hai. 

Hai quầy bánh pancake ghi lại màu sắc của các vị trí ban đầu. Chúng được so sánh với số lượng ô có sẵn có màu đối diện, vì mỗi chiếc bánh hoàn thành đúng cách phải vượt qua phân vùng một số lần lẻ. 

Bộ đếm cạnh chỉ nhìn xuống và sang phải. Do đó, mỗi cạnh ngang hoặc dọc được tính chính xác một lần, tránh cả việc tính hai lần và cấu trúc dữ liệu biểu đồ riêng biệt. 

các`pancakes < cells`kiểm tra là sự khác biệt quan trọng giữa hai trường hợp cấu trúc. Nếu có ít nhất một ô trống có thể sử dụng được thì đối số bộ đệm sẽ được áp dụng. Nếu tất cả các ô có thể sử dụng đều bị chiếm, chúng ta cần kiểm tra biểu đồ trong một chu kỳ. 

Bởi vì vùng khả dụng được đảm bảo được kết nối, nên đặc tính tiêu chuẩn của cây được áp dụng: đồ thị được kết nối với các đỉnh (V) là không có chu trình chính xác khi nó có các cạnh (V-1). Như vậy`edges >= cells`phát hiện một chu trình không có DFS hoặc cấu trúc tập hợp rời rạc. 

Không có số học nào có thể vượt quá vài chục nghìn, vì vậy việc tràn số nguyên không phải là vấn đề trong Python hoặc trong cách triển khai C++ thông thường. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
1 3
P.P
```Sử dụng tọa độ dựa trên số 0, các ô 0 và 2 có cùng màu. 

| Biến | Sau khi quét | 
| --- | --- | 
|`cells`| 3 | 
|`pancakes`| 2 | 
|`black_cells`| 2 | 
|`white_cells`| 1 | 
|`black_pancakes`| 2 | 
|`white_pancakes`| 0 | 

điều kiện`black_pancakes > white_cells`là (2 > 1) nên thuật toán in ngay`NO`. 

Điều này chứng tỏ tại sao chỉ kiểm tra xem có ô trống hay không là không đủ. Có một ô trống, nhưng cả hai chiếc bánh đều cần các ô riêng biệt có màu đối lập sau khi được lật. 

### Mẫu 2 

Đầu vào là```
2 2
PP
PP
```Bốn ô tạo thành 4 chu kỳ, với hai ô mỗi màu. 

| Biến | Sau khi quét | 
| --- | --- | 
|`cells`| 4 | 
|`pancakes`| 4 | 
|`black_cells`| 2 | 
|`white_cells`| 2 | 
|`black_pancakes`| 2 | 
|`white_pancakes`| 2 | 
|`edges`| 4 | 

Cả hai sự bất bình đẳng về công suất màu đều được giữ nguyên. Từ`pancakes == cells`, thuật toán kiểm tra một chu trình. Đây`edges >= cells`, vì (4 \ge 4) nên đáp án là`YES`. 

Chu trình cung cấp chính xác không gian làm việc bị thiếu trong trường hợp hai ô. Bốn chiếc bánh có thể được dịch chuyển xung quanh hình vuông, chuyển từng chiếc sang màu đối diện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(nm)) | Mỗi ô lưới được kiểm tra một lần và chỉ thực hiện công việc liên tục cho mỗi ô. | 
| Không gian | (O(nm)) | Bản thân lưới sử dụng bộ nhớ (O(nm)); tất cả các bộ đếm bổ sung đều sử dụng (O(1)). | 

Ở kích thước tối đa, lưới chỉ chứa 10.000 ô, do đó thuật toán thực hiện một vài lần kiểm tra liên tục trên mỗi ô. Nó thoải mái nằm trong giới hạn một giây và 256 MB của bài toán cuộc thi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    cells = 0
    pancakes = 0
    black_cells = 0
    white_cells = 0
    black_pancakes = 0
    white_pancakes = 0
    edges = 0

    for i in range(n):
        for j in range(m):
            if grid[i][j] == '#':
                continue

            cells += 1

            if (i + j) % 2 == 0:
                black_cells += 1
            else:
                white_cells += 1

            if grid[i][j] == 'P':
                pancakes += 1
                if (i + j) % 2 == 0:
                    black_pancakes += 1
                else:
                    white_pancakes += 1

            if i + 1 < n and grid[i + 1][j] != '#':
                edges += 1
            if j + 1 < m and grid[i][j + 1] != '#':
                edges += 1

    if black_pancakes > white_cells:
        print("NO")
    elif white_pancakes > black_cells:
        print("NO")
    elif pancakes < cells:
        print("YES")
    elif edges >= cells:
        print("YES")
    else:
        print("NO")

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

# provided samples
assert run("1 3\nP.P\n") == "NO", "sample 1"
assert run("2 2\nPP\nPP\n") == "YES", "sample 2"
assert run("2 2\nPP\nP#\n") == "NO", "sample 3"

# minimum-size board, one pancake and one available neighbor
assert run("1 2\nP.\n") == "YES", "single pancake"

# two cells, both occupied, balanced colors but no cycle
assert run("1 2\nPP\n") == "NO", "occupied tree with two vertices"

# full 1 x 4 path, balanced colors but still no cycle
assert run("1 4\nPPPP\n") == "NO", "full occupied tree"

# maximum-size grid, all cells occupied, many cycles and balanced colors
grid = "\n".join(["P" * 100 for _ in range(100)])
assert run("100 100\n" + grid + "\n") == "YES", "maximum grid"

# a connected region with an empty cell and valid color capacities
assert run("2 3\nPP.\n...\n") == "YES", "empty buffer"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 / P.`|`YES`| Bảng tối thiểu không cần thiết và một chiếc bánh kếp duy nhất. | 
|`1 2 / PP`|`NO`| Số lượng màu cân bằng là không đủ khi mọi đỉnh của cây hai đỉnh đều bị chiếm giữ. | 
|`1 4 / PPPP`|`NO`| Chiếm toàn bộ biểu đồ chu kỳ lớn hơn, nắm bắt các giải pháp chỉ kiểm tra số lượng màu. | 
|`100 100 / all P`|`YES`| Kích thước lưới tối đa và điều kiện chu kỳ trên một vùng được kết nối lớn. | 
|`2 3 / PP. / ...`|`YES`| Sự hiện diện của một ô đệm trống, nơi có đủ sự bất bình đẳng về màu sắc. | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là một tấm bảng có hai ô có thể sử dụng được và hai chiếc bánh kếp:```
1 2
PP
```Có một ô của mỗi màu và một chiếc bánh của mỗi màu, vì vậy cả hai sự bất bình đẳng về công suất đều được giải quyết. Tuy nhiên, tất cả các ô đều bị chiếm và đồ thị có một cạnh và hai đỉnh, vì vậy`edges >= cells`là sai. Thuật toán in`NO`. Chu trình bị thiếu không thể được thay thế bằng cách xếp chồng vì chiếc bánh trên cùng sẽ phải rời khỏi ô thứ hai trước khi chiếc bánh dưới cùng có thể di chuyển. 

Trường hợp cạnh thứ hai là bản gốc`1 x 3`ví dụ:```
1 3
P.P
```Các ô sử dụng được có màu đen, trắng, đen. Có hai chiếc bánh đen nhưng chỉ có một ô trắng. Lần kiểm tra dung lượng đầu tiên thất bại ngay lập tức, khiến`NO`. Không có mức độ xếp chồng tạm thời nào có thể giải quyết được tình trạng thiếu các ô cuối cùng có thể xảy ra. 

Trường hợp cạnh thứ ba là một chiếc bánh pancake:```
1 2
P.
```Có một chiếc bánh đen và một chiếc bánh trắng. Bất đẳng thức dung lượng đầu tiên là (1 \le 1), bất đẳng thức thứ hai là tầm thường và có một ô trống vì hai ô có thể sử dụng chỉ chứa một chiếc bánh pancake. Thuật toán in`YES`, tương ứng với việc di chuyển chiếc bánh kếp một lần sang ô lân cận. 

Trường hợp cạnh thứ tư là một đường dẫn được chiếm hoàn toàn với bốn ô:```
1 4
PPPP
```Hai màu, mỗi màu chứa hai ô và hai bánh kếp, do đó số lượng chẵn lẻ được cân bằng. Tuy nhiên, đồ thị có bốn đỉnh và chỉ có ba cạnh nên nó là một cây. Vì không có ô trống và không có chu trình nên thuật toán sẽ in`NO`. Đây là phiên bản lớn hơn của vật cản hai ô. 

Trường hợp cạnh thứ năm là bảng đã được chiếm dụng hoàn toàn (2 \times 2):```
2 2
PP
PP
```Có hai ô và bánh kếp mỗi màu, và bốn ô có thể sử dụng được chứa bốn cạnh xung quanh một chu kỳ. Thuật toán tiếp cận nhánh chiếm chỗ đầy đủ và tìm thấy`edges >= cells`, vì vậy nó in`YES`. Chu kỳ cho phép cấu hình bị sử dụng xoay và đưa ra số lần di chuyển lẻ cần thiết.
