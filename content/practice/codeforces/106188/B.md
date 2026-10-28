---
title: "CF 106188B - MLG 360 Không có phạm vi"
description: "Chúng ta có một người chơi đang đứng ở tọa độ (ví dụ: ey) trên lưới. Người chơi đang quay mặt về một trong tám hướng la bàn và bắn một viên đạn sau một vòng quay hoàn toàn. Việc quay không làm thay đổi hướng cuối cùng, do đó viên đạn chỉ đơn giản là di chuyển mãi mãi theo hướng ban đầu."
date: "2026-06-25T10:46:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106188
codeforces_index: "B"
codeforces_contest_name: "UTPC x WiCS Contest 11-12-2025"
rating: 0
weight: 106188
solve_time_s: 37
verified: true
draft: false
---

[CF 106188B - MLG 360 Không có phạm vi](https://codeforces.com/problemset/problem/106188/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một người chơi đang đứng ở tọa độ`(ex, ey)`trên một lưới. Người chơi đang quay mặt về một trong tám hướng la bàn và bắn một viên đạn sau một vòng quay hoàn toàn. Việc quay không làm thay đổi hướng cuối cùng, do đó viên đạn chỉ đơn giản là di chuyển mãi mãi theo hướng ban đầu. Trong số tất cả các vị trí nhất định của kẻ thù, chúng ta cần tìm kẻ thù đầu tiên mà viên đạn chạm tới. Nếu đường đạn không bao giờ chứa kẻ thù, chúng ta sẽ in`-1`. 

Đầu vào cung cấp số lượng kẻ thù, vị trí của người chơi, hướng đối mặt và tọa độ của mọi kẻ thù. Đầu ra là tọa độ của kẻ thù gần người chơi nhất dọc theo tia đó. 

Tọa độ được giới hạn bởi`100000`, và số lượng kẻ thù cũng có thể lên tới`100000`. Một giải pháp kiểm tra mọi vị trí lưới trong đường dấu đầu dòng có thể hoạt động tới khoảng`100000`kiểm tra theo hướng, điều này là không cần thiết. Vì có thể có`100000`kẻ thù, chúng tôi muốn một thuật toán xử lý từng kẻ thù một số lần không đổi, đưa ra`O(n)`giải pháp. Cũng có thể sắp xếp, nhưng quét tuyến tính là đủ vì số lượng hướng có thể rất nhỏ. 

Các trường hợp phức tạp xuất phát từ việc nhầm lẫn "cùng một dòng" với "trong đường đạn". Ví dụ: nếu người chơi đang ở`(1,1)`đối mặt`NORTHEAST`, kẻ thù ở`(3,3)`bị đánh và câu trả lời là:```
3 3
```Giải bất cẩn chỉ kiểm tra phương trình đường chéo mà quên hướng có thể chấp nhận sai`(0,0)`trong một phiên bản của bài toán cho phép tọa độ âm, vì nó nằm trên cùng một đường chéo nhưng ở phía sau người chơi. 

Một trường hợp khác là có nhiều kẻ thù trên cùng một tia. Đối với đầu vào:```
2 1 1
EAST
3 1
5 1
```đầu ra đúng là:```
3 1
```Kẻ thù đầu tiên chặn kẻ thù thứ hai. Chỉ kiểm tra xem có kẻ địch nào nằm cùng hướng hay không và giữ lại kẻ cuối cùng được tìm thấy có thể đánh trả sai mục tiêu. 

Một lỗi phổ biến cuối cùng là xử lý ô của chính người chơi. Ví dụ:```
1 1 1
NORTH
1 1
```Câu trả lời là:```
1 1
```Kẻ địch ở vị trí xuất phát và là thực thể đầu tiên chạm trán. Một giải pháp chỉ tìm kiếm chuyển tiếp nghiêm ngặt từ ô tiếp theo sẽ bỏ lỡ nó. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là mô phỏng đường đạn. Bắt đầu từ vị trí của người chơi, di chuyển từng ô một theo hướng đã chọn và kiểm tra xem có kẻ thù ở mọi tọa độ đã ghé thăm hay không. Nếu chúng ta lưu trữ kẻ thù trong một tập hợp, điều này có thể được thực hiện dễ dàng và chính xác vì tọa độ đầu tiên gặp trên đường đi chính xác là kẻ thù đầu tiên tấn công. 

Vấn đề là lượng không gian trống mà viên đạn có thể đi qua. Phạm vi tọa độ đạt`100000`, vì vậy một mô phỏng đơn lẻ có thể yêu cầu khoảng`100000`các bước. Việc lặp lại điều này trên các đầu vào lớn vẫn có thể quản lý được trong một số trường hợp, nhưng nó lãng phí công sức vì hầu hết các ô được truy cập không chứa kẻ thù. Lưới rất lớn trong khi số lượng kẻ thù chỉ có`100000`. 

Quan sát quan trọng là chúng ta chỉ quan tâm đến tọa độ của kẻ thù. Mọi hướng đều có một điều kiện toán học đơn giản. Ví dụ, di chuyển về phía bắc có nghĩa là kẻ địch phải có cùng`x`phối hợp và một`y`giá trị không nhỏ hơn giá trị của người chơi. Di chuyển về phía đông bắc có nghĩa là sự khác biệt giữa`x`Và`y`phải phù hợp với sự khác biệt của người chơi. Điều kiện tia cho chúng ta biết kẻ thù nào là ứng cử viên và khoảng cách với người chơi cho chúng ta biết ứng cử viên nào được tiếp cận trước. 

Trong một lần quét qua tất cả kẻ thù, chúng tôi kiểm tra xem kẻ thù có nằm trên tia hay không. Nếu đúng như vậy, chúng tôi sẽ so sánh khoảng cách của nó với ứng cử viên tốt nhất được tìm thấy cho đến nay. Vì tia chỉ đi theo một hướng nên ứng cử viên gần nhất chính xác là người đánh trúng đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(max_coordine) | O(n) | Về mặt khái niệm quá chậm vì nó quét các ô trống | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc vị trí, hướng đi của người chơi và tất cả các vị trí của kẻ thù. Chuyển đổi hướng thành một vectơ chuyển động`(dx, dy)`. Ví dụ, phía bắc trở thành`(0, 1)`và phía tây nam trở thành`(-1, -1)`. Điều này cho phép mọi hướng sử dụng cùng một logic. 
2. Đối với mỗi kẻ thù, tính toán sự khác biệt của người chơi:`(tx, ty) = (enemy_x - ex, enemy_y - ey)`Kẻ địch chỉ có thể bị tấn công nếu nó ở đâu đó dọc theo vectơ`(dx, dy)`. 
3. Kiểm tra xem kẻ địch có cùng tia không. một điểm`(tx, ty)`nằm trên tia khi có một khoảng cách không âm`k`như vậy:`tx = k * dx`Và`ty = k * dy`Các dấu hiệu phải phù hợp với hướng và tỷ lệ giữa các tọa độ phải phù hợp. Vì các vectơ chuyển động chỉ chứa`0`,`1`, hoặc`-1`, điều này có thể được đơn giản hóa bằng việc kiểm tra tình trạng và hướng của đường dây. 
4. Nếu kẻ địch hợp lệ, hãy tính xem nó còn cách bao xa. Khoảng cách bình phương là đủ vì chúng ta chỉ so sánh các giá trị. Thí sinh có khoảng cách xa nhất là người đánh đầu tiên. 
5. Sau khi tất cả kẻ thù được xử lý, hãy in câu trả lời đã lưu. Nếu không tìm thấy ứng viên nào, hãy in`-1`. 

Tại sao nó hoạt động: mọi kẻ thù có thể bị bắn trúng phải thỏa mãn phương trình tia, do đó thuật toán không bao giờ bỏ qua một mục tiêu khả thi. Trong số tất cả các mục tiêu hợp lệ, viên đạn sẽ chạm tới mục tiêu có khoảng cách nhỏ nhất trước tiên vì viên đạn di chuyển liên tục ra phía ngoài người chơi. Việc giữ khoảng cách tối thiểu duy trì sự bất biến rằng câu trả lời được lưu trữ luôn là kẻ thù hợp lệ gần nhất được nhìn thấy cho đến nay. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, ex, ey = map(int, input().split())
    direction = input().strip()

    enemies = []
    for _ in range(n):
        x, y = map(int, input().split())
        enemies.append((x, y))

    moves = {
        "NORTH": (0, 1),
        "NORTHEAST": (1, 1),
        "EAST": (1, 0),
        "SOUTHEAST": (1, -1),
        "SOUTH": (0, -1),
        "SOUTHWEST": (-1, -1),
        "WEST": (-1, 0),
        "NORTHWEST": (-1, 1),
    }

    dx, dy = moves[direction]

    best_dist = float("inf")
    ans = None

    for x, y in enemies:
        tx = x - ex
        ty = y - ey

        if dx == 0:
            if tx != 0:
                continue
            if ty * dy < 0:
                continue
        elif dy == 0:
            if ty != 0:
                continue
            if tx * dx < 0:
                continue
        else:
            if tx * dy != ty * dx:
                continue
            if tx * dx < 0 or ty * dy < 0:
                continue

        dist = tx * tx + ty * ty
        if dist < best_dist:
            best_dist = dist
            ans = (x, y)

    if ans is None:
        print(-1)
    else:
        print(ans[0], ans[1])

if __name__ == "__main__":
    solve()
```Từ điển hướng ánh xạ từng hướng có thể có vào một vectơ. Điều này tránh việc viết logic riêng biệt cho tám trường hợp và làm cho việc kiểm tra hình học trở nên thống nhất. 

Vòng lặp chỉ kiểm tra vị trí của kẻ thù. Các biến`tx`Và`ty`đại diện cho vị trí của kẻ thù so với người chơi, điều này giúp việc kiểm tra tia đơn giản hơn nhiều so với làm việc với tọa độ tuyệt đối. 

Đối với hướng thẳng đứng, chỉ`x`tọa độ phải phù hợp. Đối với hướng ngang, chỉ`y`tọa độ phải phù hợp. Đối với các hướng chéo, tọa độ tương đối phải có cùng tỷ lệ, trở thành đẳng thức`tx * dy == ty * dx`. 

Việc so sánh khoảng cách sử dụng khoảng cách bình phương vì căn bậc hai là không cần thiết. So sánh`a^2`Và`b^2`đưa ra thứ tự giống như so sánh`a`Và`b`. Các tọa độ đủ nhỏ để số nguyên Python dễ dàng xử lý phép nhân. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
1 1 1
NORTHEAST
3 3
```Trạng thái thay đổi như sau: 

| Kẻ thù | Vị trí tương đối | Trên tia | Khoảng cách | Câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| (3,3) | (2,2) | Có | 8 | (3,3) | 

Kẻ địch có cùng hướng chéo với viên đạn nên trở thành mục tiêu đầu tiên. 

Đối với ví dụ thứ hai:```
2 1 1
NORTHEAST
3 4
4 3
```| Kẻ thù | Vị trí tương đối | Trên tia | Khoảng cách | Câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| (3,4) | (2,3) | Không | bỏ qua | không | 
| (4,3) | (3,2) | Không | bỏ qua | không | 

Không có kẻ thù nào ở trên đường chéo`y = x`mở rộng từ người chơi, vì vậy kết quả là`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi kẻ thù được kiểm tra một lần với số học thời gian không đổi | 
| Không gian | O(1) | Chỉ câu trả lời tốt nhất hiện tại và một vài biến được lưu trữ | 

Giải pháp phù hợp với các ràng buộc bởi vì`100000`Kẻ thù chỉ yêu cầu`100000`lần lặp lại. Các phép tính số học bên trong mỗi lần lặp là thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(data):
    input = io.StringIO(data).readline

    n, ex, ey = map(int, input().split())
    direction = input().strip()

    moves = {
        "NORTH": (0, 1),
        "NORTHEAST": (1, 1),
        "EAST": (1, 0),
        "SOUTHEAST": (1, -1),
        "SOUTH": (0, -1),
        "SOUTHWEST": (-1, -1),
        "WEST": (-1, 0),
        "NORTHWEST": (-1, 1),
    }

    dx, dy = moves[direction]

    best = float("inf")
    ans = None

    for _ in range(n):
        x, y = map(int, input().split())
        tx, ty = x - ex, y - ey

        if dx == 0:
            if tx != 0 or ty * dy < 0:
                continue
        elif dy == 0:
            if ty != 0 or tx * dx < 0:
                continue
        else:
            if tx * dy != ty * dx or tx * dx < 0 or ty * dy < 0:
                continue

        dist = tx * tx + ty * ty
        if dist < best:
            best = dist
            ans = (x, y)

    if ans is None:
        return "-1"
    return f"{ans[0]} {ans[1]}"

# samples
assert solve("""1 1 1
NORTHEAST
3 3
""") == "3 3"

assert solve("""2 1 1
NORTHEAST
3 4
4 3
""") == "-1"

# custom cases
assert solve("""1 1 1
NORTH
1 1
""") == "1 1"

assert solve("""3 5 5
EAST
10 5
7 5
8 6
""") == "7 5"

assert solve("""4 10 10
SOUTHWEST
8 8
5 5
9 9
1 2
""") == "9 9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 / NORTH / 1 1`|`1 1`| Xử lý vị trí bắt đầu | 
|`3 5 5 / EAST / 10 5, 7 5, 8 6`|`7 5`| Chọn kẻ thù gần nhất trên một tia | 
|`4 10 10 / SOUTHWEST / 8 8, 5 5, 9 9, 1 2`|`9 9`| Hướng chéo và bỏ qua các điểm không liên quan | 

## Vỏ cạnh 

Đối với trường hợp kẻ địch ở phía sau người chơi, việc kiểm tra hướng tia sẽ ngăn chặn việc khớp sai. Nếu người chơi đang ở`(1,1)`đối mặt`NORTHEAST`, kẻ thù ở`(0,0)`sẽ thỏa mãn phương trình đường chéo nhưng có khoảng cách di chuyển âm nên bị bác bỏ. 

Đối với nhiều kẻ thù trên cùng một chiến tuyến, việc so sánh khoảng cách tối thiểu sẽ giữ kẻ thù gần nhất. Với:```
2 1 1
EAST
3 1
5 1
```kẻ thù đầu tiên có khoảng cách`4`, trong khi thứ hai có khoảng cách`16`. Thuật toán lưu trữ`(3,1)`và không bao giờ thay thế nó bằng kẻ thù ở xa hơn. 

Đối với một hướng không chuyển động trong một tọa độ, các kiểm tra đặc biệt sẽ tránh logic kiểu phân chia. Hướng về phía bắc có nghĩa là tất cả kẻ thù hợp lệ đều phải có cùng một`x`, và chỉ kẻ thù có`y >= ey`có thể đạt được. Điều này xử lý các tia dọc và ngang mà không có bất kỳ trường hợp góc nào so sánh tỷ lệ.
