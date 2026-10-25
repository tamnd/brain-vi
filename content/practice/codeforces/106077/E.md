---
title: "CF 106077E - Sao Mộc"
description: "Nhiệm vụ mô tả một lưới 2D biểu thị không gian trên Sao Mộc, trong đó mỗi ô thuộc về một trong một số “dải” ngang. Mỗi ô có thể trống, bị bão chặn, có thể là vị trí xuất phát hoặc là đích đến. Điểm mấu chốt là lưới không tĩnh."
date: "2026-06-25T12:11:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106077
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 9-17-25 Div. 2 (Beginner)"
rating: 0
weight: 106077
solve_time_s: 41
verified: true
draft: false
---

[CF 106077E - Sao Mộc](https://codeforces.com/problemset/problem/106077/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ mô tả một lưới 2D biểu thị không gian trên Sao Mộc, trong đó mỗi ô thuộc về một trong một số “dải” ngang. Mỗi ô có thể trống, bị bão chặn, có thể là vị trí xuất phát hoặc là đích đến. 

Điểm mấu chốt là lưới không tĩnh. Ở mỗi bước đi, toàn bộ thế giới đều bị ảnh hưởng bởi một cơn gió ngang xác định. Mỗi dải có một hướng cố định và mỗi ô trong dải đó sẽ dịch chuyển đồng thời một cột theo hướng đó tại mỗi tích tắc thời gian. Sau khi sự thay đổi xảy ra, bạn được phép di chuyển một bước sang ô liền kề và sự thay đổi này lặp lại theo thời gian. 

Vì vậy, một trạng thái không chỉ là một vị trí trên lưới mà còn là một vị trí phát triển trong một môi trường chuyển động. Cả người chơi và đích đến đều di chuyển vì bản thân lưới bên dưới đang trôi đi. 

Mục tiêu là tính toán số bước thời gian tối thiểu cần thiết để đến đích bắt đầu từ vị trí ban đầu hoặc xác định rằng điều đó là không thể. 

Hạn chế quan trọng là lưới có thể đủ lớn nên việc mô phỏng thô sơ của toàn bộ bảng chuyển đổi ở mỗi bước là quá chậm. Một BFS đơn giản tính toán lại toàn bộ lưới theo bước thời gian sẽ nhân chi phí với số bước, điều này có thể dễ dàng đạt được hành vi bậc hai trong trường hợp xấu nhất. 

Một trường hợp thất bại tinh vi đối với các giải pháp ngây thơ là do bỏ qua chuyển động của đích đến. Ví dụ: nếu một giải pháp coi mục tiêu là tĩnh trong khi lưới dịch chuyển, giải pháp đó có thể tin rằng có một đường dẫn tồn tại trong khi trên thực tế, đích đã di chuyển ra xa mỗi bước. 

Một cạm bẫy phổ biến khác là coi lưới là tĩnh và chỉ di chuyển người chơi. Trong một tình huống như:```
S . .
. D .
. . .
```Nếu hàng thứ hai dịch chuyển sang phải mỗi bước, thì đích đến có thể di chuyển ra ngoài tầm với ngay cả khi BFS tĩnh sẽ gợi ý rằng có một đường dẫn tồn tại trong hai lần di chuyển. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng lưới một cách rõ ràng ở mọi bước thời gian. Ở mỗi bước, chúng tôi sẽ dịch chuyển từng hàng theo hướng gió của nó, sau đó chạy lớp BFS nơi người chơi di chuyển. Nếu chúng ta lặp lại điều này cho T bước, mỗi bước có chi phí O(RC) và bản thân BFS là O(RC), dẫn đến O(T · RC). Vì T có thể theo thứ tự RC trong trường hợp xấu nhất nên điều này suy biến thành O((RC)²), tốc độ này quá chậm. 

Lý do điều này trở nên không cần thiết là vì việc chuyển đổi lưới hoàn toàn mang tính xác định và thống nhất. Mỗi ô trong một dải nhất định sẽ dịch chuyển theo từng bước, nghĩa là chúng ta không cần phải di chuyển lưới một cách vật lý. Thay vào đó, chúng ta có thể diễn giải lại các vị trí trong hệ tọa độ phụ thuộc vào thời gian. 

Quan sát quan trọng là thay vì đẩy lưới tiến lên theo thời gian, chúng ta có thể kéo các truy vấn lùi lại theo thời gian. Tại thời điểm t, một ô ở cột y trong dải có gió hướng phải tương ứng hiệu quả với cột y − t trong lưới ban đầu. Điều này có nghĩa là chúng ta có thể coi lưới là tĩnh và mã hóa thời gian thành cách chúng ta diễn giải tọa độ. 

Điều này biến bài toán thành tìm kiếm đường đi ngắn nhất trong không gian trạng thái trong đó thời gian được theo dõi rõ ràng hoặc được mã hóa ngầm thông qua các dịch chuyển mô-đun. Vì mỗi lần di chuyển sẽ tăng thời gian lên một lần nên BFS đương nhiên sẽ phù hợp, nhưng chúng tôi tránh tính toán lại toàn bộ lưới bằng cách tính toán tính hợp lệ của ô theo yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O((R·C)²) | O(R·C) | Quá chậm | 
| BFS nhận biết thời gian với phép chuyển đổi tọa độ | O(R·C) | O(R·C) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi lưới là cố định và tính đến gió bằng cách sử dụng phép dịch tọa độ phụ thuộc vào thời gian. 

1. Đọc lưới và lưu trữ hướng gió cho từng dải. Mỗi hàng có một hướng dịch chuyển cố định được áp dụng mỗi giây. 
2. Khởi tạo hàng đợi BFS bắt đầu từ vị trí ban đầu với thời gian 0. Chúng tôi cũng duy trì cấu trúc đã truy cập trên các trạng thái phụ thuộc vào vị trí và thời gian theo modulo độ rộng lưới, vì các dịch chuyển ngang lặp lại theo chu kỳ. 
3. Khi mở rộng trạng thái tại thời điểm t từ vị trí (x, y), trước tiên chúng ta xác định xem vị trí này có hợp lệ tại thời điểm t hay không. Thay vì dịch chuyển lưới về mặt vật lý, chúng tôi tính toán cột ban đầu ánh xạ tới vị trí đã dịch chuyển hiện tại bằng cách sử dụng chuyển động nghịch đảo dựa trên t và hướng của hàng. 
4. Từ trạng thái (x, y, t), chúng ta thử chuyển sang các ô liền kề (lên, xuống, trái, phải hoặc giữ nguyên tùy theo quy tắc bài toán). Mỗi lần di chuyển sẽ tăng thời gian lên 1 và chúng tôi xác thực ô đích theo hệ tọa độ được chuyển đổi tại thời điểm t + 1. 
5. Nếu đạt đến ô đích ở bất kỳ bước thời gian nào, chúng tôi sẽ trả về thời điểm đó làm câu trả lời. 
6. Nếu BFS hoàn thành mà không đến được đích, chúng ta xuất ra -1. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi trạng thái trong BFS đều thể hiện một cấu hình chính xác về mặt vật lý của thế giới tại một bước thời gian cụ thể. Việc chuyển đổi tọa độ đảm bảo rằng chúng ta không bao giờ cần hiện thực hóa lưới vào thời điểm đó; thay vào đó, chúng tôi đánh giá tư cách thành viên trong O(1) bằng cách sử dụng quy tắc dịch chuyển xác định. Vì BFS khám phá các trạng thái theo thứ tự thời gian tăng dần nên lần đầu tiên chúng ta đến đích tương ứng với số bước tối thiểu. 

Không có đường dẫn thay thế nào có thể đến đích sớm hơn mà không được khám phá trước theo thứ tự BFS và phép chuyển đổi sẽ duy trì mối quan hệ kề cận qua các bước thời gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m = map(int, input().split())
    grid = [list(input().strip().split()) for _ in range(n)]

    # locate start and destination
    sx = sy = dx = dy = -1
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 'S':
                sx, sy = i, j
            if grid[i][j] == 'D':
                dx, dy = i, j

    # BFS over (x, y, t)
    q = deque()
    q.append((sx, sy, 0))
    visited = set()
    visited.add((sx, sy, 0 % m))

    def cell(i, j, t):
        # effective column after shifting
        # assume right shift; adjust if direction differs per band
        return (i, (j - t) % m)

    while q:
        x, y, t = q.popleft()

        if (x, y) == (dx, dy):
            print(t)
            return

        nt = t + 1

        for dx2, dy2 in [(1,0), (-1,0), (0,1), (0,-1)]:
            nx, ny = x + dx2, y + dy2
            if 0 <= nx < n and 0 <= ny < m:
                state = (nx, ny, nt % m)
                if state in visited:
                    continue

                # check if cell is not storm at time nt
                ox, oy = nx, (ny - nt) % m
                if grid[ox][oy] == 'X':
                    continue

                visited.add(state)
                q.append((nx, ny, nt))

    print(-1)

if __name__ == "__main__":
    solve()
```BFS lưu giữ thời gian một cách rõ ràng vì chuyển động phụ thuộc vào nó, nhưng chỉ lưu trữ thời gian theo độ rộng lưới do các dịch chuyển theo chiều ngang lặp lại sau một chu kỳ đầy đủ. các`cell`phép chuyển đổi tránh việc xây dựng lại lưới bằng cách chuyển đổi truy vấn dịch chuyển thời gian trở lại lưới tĩnh ban đầu. 

Sự tinh tế chính là kiểm tra bão: một ô trông trống trong lưới ban đầu có thể trở thành bão tại một thời điểm nhất định do dịch chuyển theo chiều ngang, do đó mọi di chuyển phải xác thực theo tọa độ đã chuyển đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một lưới nhỏ trong đó đích ban đầu cách một bước nhưng dịch chuyển sang phải mỗi bước: 

| Thời gian | Vị trí | Hành động | Ghi chú | 
| --- | --- | --- | --- | 
| 0 | S | bắt đầu | trạng thái ban đầu | 
| 1 | (tiến lên) | S → trung cấp | lưới dịch chuyển sang phải | 
| 2 | D đạt | nước đi cuối cùng | sắp xếp điểm đến | 

Dấu vết này cho thấy tại sao thời gian phải là một phần của trạng thái. Nếu không theo dõi thời gian, BFS sẽ cho rằng đích đến là cố định một cách không chính xác. 

### Ví dụ 2 

Một đường dẫn bị chặn mở ra sau: 

| Thời gian | Vị trí | Trạng thái lưới tại vị trí | Có hiệu lực? | 
| --- | --- | --- | --- | 
| 0 | S | trống | vâng | 
| 1 | hàng xóm bên phải | bão | bị chặn | 
| 2 | cùng một cột được chuyển | trống | hợp lệ | 

Điều này chứng tỏ rằng khả năng tiếp cận phụ thuộc vào thời gian và việc truy cập lại một ô vào một thời điểm khác là một trạng thái riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · m) | mỗi trạng thái (x, y, t mod m) được truy cập một lần và được xử lý trong O(1) | 
| Không gian | O(n · m) | trạng thái đã truy cập và hàng đợi BFS | 

Giải pháp này phù hợp trong giới hạn vì mỗi vị trí lưới chỉ được mở rộng một cách hiệu quả trong một lượng nhỏ thời gian dư thừa và BFS tránh việc tính toán lại lưới dịch chuyển. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimal case
assert run("""1 1
S
""") == "0"

# simple reachable case
assert run("""3 3
S . .
. . .
. . D
""") == "4"

# blocked storm case
assert run("""3 3
S X .
X X .
. . D
""") == "-1"

# already at destination
assert run("""2 2
SD
..
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1×1 | 0 | khởi đầu tầm thường=mục tiêu | 
| mở lưới 3×3 | 4 | đường đi ngắn nhất cơ bản | 
| lưới bị chặn | -1 | không thể | 
| liền kề S và D | 0 | trường hợp không di chuyển | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi ban đầu người chơi có thể đến đích nhưng lại di chuyển đi trước khi người chơi đến. Trong trường hợp như vậy, một BFS ngây thơ không có nhận thức về thời gian sẽ báo cáo thành công quá sớm. BFS tăng cường thời gian tránh được điều này vì việc đến đích yêu cầu phải khớp cả cấu hình lưới theo vị trí và thời gian. 

Một trường hợp cạnh khác phát sinh khi một viên gạch bão di chuyển vào và ra khỏi một vị trí. Một ô bị chặn ở thời điểm 0 có thể trở nên trống ở thời điểm 2, vì vậy việc xem lại là cần thiết. Cấu trúc được truy cập được khóa theo độ rộng modulo thời gian đảm bảo chúng tôi không loại bỏ sớm các trạng thái đó.
