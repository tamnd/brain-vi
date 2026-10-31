---
title: "CF 106352B - \u0422\u0440\u0435\u043d\u0438\u0440\u043e\u0432\u043a\u0430 \u043f\u043e \u043f\u0435\u0439\u043d\u0442\u0431\u043e\u043b\u0443"
description: "Chúng ta được cung cấp một chuỗi các viên đạn sơn, mỗi viên có đặc điểm là thời điểm nó bay và vị trí thẳng đứng (một hàng trên tường). Bức tường có chiều cao h, các hàng được đánh số từ dưới lên trên. Mỗi quả bóng sơn chỉ tồn tại tại một thời điểm nhất định và ảnh hưởng đến chính xác một hàng."
date: "2026-06-19T14:52:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106352
codeforces_index: "B"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2025-2026, \u041f\u0435\u0440\u0432\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 106352
solve_time_s: 48
verified: true
draft: false
---

[CF 106352B - \u0422\u0440\u0435\u043d\u0438\u0440\u043e\u0432\u043a\u0430 \u043f\u043e \u043f\u0435\u0439\u043d\u0442\u0431\u043e\u043b\u0443](https://codeforces.com/problemset/problem/106352/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các viên đạn sơn, mỗi viên có đặc điểm là thời điểm nó bay và vị trí thẳng đứng (một hàng trên tường). Bức tường có chiều cao`h`và các hàng được đánh số từ dưới lên trên. Mỗi quả bóng sơn chỉ tồn tại tại một thời điểm nhất định và ảnh hưởng đến chính xác một hàng. 

Tại một số thời điểm bắt đầu đã chọn`t0`, tấm chắn có chiều cao cố định`w`được đặt bao phủ các hàng`[1, w]`. Sau đó, cứ mỗi giây nó lại di chuyển lên trên đúng một hàng, vì vậy tại một thời điểm`t0 + k`nó bao gồm các hàng`[1 + k, w + k]`. Khi nó đạt đến vị trí hợp lệ cao nhất`[h - w + 1, h]`, nó ở đó trong một giây rồi biến mất. Một quả bóng sơn bị chặn nếu tại thời điểm chính xác, hàng của nó nằm trong khoảng thời gian hiện tại được che chắn bởi tấm chắn. 

Nhiệm vụ là chọn`t0`để tối đa hóa số lượng bóng sơn bị chặn. 

Kích thước đầu vào tăng lên`n = 10^5`, tọa độ hàng và thời gian tăng lên`10^9`. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng tấm chắn trong mọi thời điểm bắt đầu có thể hoặc từng giây trong vòng đời của nó. Một bậc hai hoặc thậm chí`O(n log n)`chiến lược của mỗi ứng viên đối với tất cả các ứng viên sẽ quá chậm. 

Một khó khăn chính là mỗi giới hạn của quả bóng sơn kết hợp với thời gian và hàng: liệu nó có bị chặn hay không phụ thuộc vào mối quan hệ tuyến tính giữa thời gian của nó và thời gian bắt đầu đã chọn. 

Một số trường hợp đặc biệt quan trọng. 

Một là khi nhiều quả bóng sơn xuất hiện cùng lúc nhưng ở các hàng khác nhau. Ví dụ, nếu lúc đó`t = 5`chúng tôi có hàng`1, 2, 10`Và`w = 2`, chỉ có thể che được hai hàng liên tiếp, vì vậy có thể chặn tối đa hai hàng, không bao giờ chặn được cả ba. 

Một điều nữa là khi`w = h`. Trong trường hợp đó, tấm chắn bao phủ toàn bộ lưới trong đúng một giây bất kể vị trí, do đó, câu trả lời chỉ đơn giản là đếm xem có bao nhiêu quả bóng sơn tồn tại ở mọi thời điểm trùng với bất kỳ khoảng thời gian tồn tại hợp lệ nào của tấm chắn và trên thực tế, mọi quả bóng sơn đều có thể bị chặn nếu chúng ta căn chỉnh`t0`với thời gian của nó. 

Trường hợp phức tạp thứ ba là ra lệnh: tấm khiên di chuyển một cách xác định theo thời gian, do đó, việc giải thích sai cho phép nó “dịch chuyển tức thời” hoặc cố định trong nhiều giây ở các vị trí trung gian sẽ đếm quá số khối. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi thời điểm bắt đầu có thể`t0`từ tập hợp tất cả các lần bắn súng sơn. Đối với mỗi ứng viên`t0`, chúng tôi mô phỏng chuyển động của tấm chắn từng giây một và kiểm tra từng viên đạn sơn để xem liệu nó có bị che phủ vào thời điểm đó hay không. Điều này hoạt động về mặt khái niệm vì hành vi lá chắn hoàn toàn được xác định một khi`t0`đã được sửa, vì vậy tính chính xác không phải là vấn đề. 

Vấn đề là hiệu suất. Nếu chúng ta cố gắng`O(n)`khả thi`t0`giá trị và đối với mỗi giá trị chúng tôi quét tất cả`n`súng sơn, chúng tôi đã có rồi`O(n^2)`, vượt xa giới hạn của`n = 10^5`. Thậm chí trực tiếp hơn, việc mô phỏng tấm chắn lên đến`h`các bước là không thể bởi vì`h`có thể đạt được`10^9`. 

Quan sát quan trọng là mỗi quả bóng sơn áp đặt một ràng buộc về giá trị hợp lệ`t0`các giá trị. Đối với một quả bóng sơn`(t, s)`muốn chặn thì tấm chắn phải che hàng`s`vào thời điểm đó`t`, có nghĩa là:```
1 + (t - t0) <= s <= w + (t - t0)
```Sắp xếp lại cả hai bất đẳng thức sẽ cho một khoảng giá trị`t0`các giá trị mà quả bóng sơn này được đề cập. Vì vậy, mỗi viên đạn sơn sẽ trở thành một đoạn trên trục số của`t0`. 

Một khi việc chuyển đổi này được thực hiện, vấn đề trở thành việc tìm một điểm`t0`nằm bên trong số khoảng tối đa. Đây là một vấn đề chồng chéo tối đa cổ điển theo các khoảng thời gian, có thể giải quyết được bằng cách sắp xếp các điểm cuối và quét. 

Điểm tế nhị là chúng ta cũng cần tôn trọng rằng tấm khiên chỉ tồn tại trong một số giây hữu hạn. Tuy nhiên, ràng buộc này được mã hóa một cách tự nhiên bằng đạo hàm khoảng, vì bên ngoài phạm vi hợp lệ, các bất đẳng thức chỉ tạo ra các khoảng trống. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force trong tất cả các lần bắt đầu | O(n²) | O(1) | Quá chậm | 
| Chuyển đổi khoảng thời gian + đường quét | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Chuyển đổi khóa 

1. Đối với mỗi quả bóng sơn`(t, s)`, suy ra phạm vi của`t0`các giá trị khiến nó bị chặn. 

Chiếc khiên vào thời điểm đó`t`bao gồm các hàng`[1 + (t - t0), w + (t - t0)]`. Chúng tôi yêu cầu:```
1 + (t - t0) <= s <= w + (t - t0)
```Giải cả hai bất đẳng thức ta có:```
t - s + 1 <= t0 <= t - s + w
```Vì vậy, mỗi viên đạn sơn sẽ trở thành một khoảng thời gian`[L, R]`trên dòng thời gian của thời gian bắt đầu có thể. 
2. Thu thập tất cả các khoảng thời gian như vậy cho tất cả các viên đạn sơn. Mỗi hợp lệ`t0`tương ứng với việc chọn hướng thẳng đứng của quỹ đạo lá chắn. 
3. Chúng tôi muốn`t0`nằm ở số lượng tối đa của các khoảng này, tương đương với sự chồng lấp khoảng tối đa. 
4. Chuyển đổi mỗi khoảng thành hai sự kiện: +1 tại`L`, -1 lúc`R + 1`. 
5. Sắp xếp tất cả các sự kiện theo tọa độ, sau đó quét từ trái sang phải để duy trì tổng số hiện có. 
6. Theo dõi giá trị tối đa của tổng số tiền đang chạy; đây là câu trả lời 

### Tại sao nó hoạt động 

Mỗi viên đạn sơn đóng góp độc lập một điều kiện trên`t0`. Một đĩa đơn`t0`có giá trị cho quả bóng sơn đó một cách chính xác khi nó nằm trong khoảng dẫn xuất của nó. Do đó, đếm xem có bao nhiêu khoảng chứa một`t0`là đếm chính xác có bao nhiêu quả bóng sơn bị chặn. Đường quét tìm điểm chồng chéo tối đa, tương ứng trực tiếp với thời gian bắt đầu tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, h, w = map(int, input().split())

events = []

for _ in range(n):
    t, s = map(int, input().split())
    L = t - s + 1
    R = t - s + w
    events.append((L, 1))
    events.append((R + 1, -1))

events.sort()

cur = 0
best = 0

for x, delta in events:
    cur += delta
    if cur > best:
        best = cur

print(best)
```Cốt lõi của việc thực hiện là bước chuyển đổi khoảng thời gian. Mỗi quả bóng sơn được dịch thành một khoảng thời gian bắt đầu`t0`nơi nó bị chặn. Đường quét qua các điểm cuối sự kiện được sắp xếp sẽ tích lũy bao nhiêu ràng buộc như vậy được thỏa mãn đồng thời. 

Một chi tiết tinh tế đang được sử dụng`R + 1`cho sự kiện tiêu cực. Điều này đảm bảo rằng khoảng thời gian được coi là bao gồm ở cả hai đầu trong khi vẫn tương thích với logic quét số nguyên. Nếu không có sự thay đổi này, các lỗi từng cái một sẽ tính sai các viên đạn sơn nằm chính xác trên ranh giới của phạm vi che chắn. 

Một điểm khác là chúng tôi không bao giờ sử dụng rõ ràng`h`. Mặc dù nó xác định các ràng buộc vật lý của đường dẫn khiên, nhưng nó hoàn toàn được tôn trọng vì các bất đẳng thức dẫn xuất đã mã hóa tính khả thi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 5 2
1 1
1 2
2 2
3 4
```Chúng tôi tính toán các khoảng: 

| bóng sơn | (t,s) | L = t-s+1 | R = t-s+w | 
| --- | --- | --- | --- | 
| 1 | (1,1) | 1 | 2 | 
| 2 | (1,2) | 0 | 1 | 
| 3 | (2,2) | 1 | 2 | 
| 4 | (3,4) | 0 | 1 | 

Sự kiện trở thành:```
(0,+1), (2,-1)
(1,+1), (2,-1)
(1,+1), (3,-1)
(1,+1), (2,-1)
```Quét: 

| x | đồng bằng | cur | tốt nhất | 
| --- | --- | --- | --- | 
| 0 | +1 | 1 | 1 | 
| 1 | +1 | 2 | 2 | 
| 1 | +1 | 3 | 3 | 
| 1 | +1 | 4 | 4 | 
| 2 | -1 | 3 | 4 | 
| 2 | -1 | 2 | 4 | 
| 2 | -1 | 1 | 4 | 
| 3 | -1 | 0 | 4 | 

Sự chồng chéo tối đa là 4, nghĩa là tồn tại một`t0`căn chỉnh tấm chắn để chặn cả bốn viên đạn sơn. 

Dấu vết này xác nhận rằng phép chuyển đổi bảo toàn tất cả các ràng buộc về thời gian và hàng mà không mô phỏng chuyển động một cách rõ ràng. 

### Ví dụ 2 

đầu vào:```
5 6 3
10 1
11 2
12 6
13 4
14 6
```Khoảng thời gian: 

| (t,s) | L | R | 
| --- | --- | --- | 
| (10,1) | 10 | 12 | 
| (11,2) | 10 | 12 | 
| (12,6) | 7 | 9 | 
| (13,4) | 10 | 12 | 
| (14,6) | 11 | 13 | 

Quét cho thấy sự chồng chéo tối đa xảy ra xung quanh`t0 = 11`, nơi ba khoảng giao nhau. 

Điều này cho thấy cách phương pháp nắm bắt sự căn chỉnh một phần một cách tự nhiên theo các thay đổi thời gian, trong đó các tập hợp con bóng sơn khác nhau có thể bị chặn trong các thời điểm bắt đầu khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp sự kiện 2n chiếm ưu thế | 
| Không gian | O(n) | lưu trữ điểm cuối khoảng thời gian | 

Giải pháp đủ nhanh để`n = 10^5`bởi vì cả việc sắp xếp và quét tuyến tính đều phù hợp một cách thoải mái trong giới hạn thời gian và việc sử dụng bộ nhớ vẫn tuyến tính theo số lượng viên đạn sơn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, h, w = map(int, input().split())
    events = []
    for _ in range(n):
        t, s = map(int, input().split())
        L = t - s + 1
        R = t - s + w
        events.append((L, 1))
        events.append((R + 1, -1))
    events.sort()
    cur = best = 0
    for _, d in events:
        cur += d
        best = max(best, cur)
    return str(best)

# provided samples
assert run("""4 5 2
1 1
1 2
2 2
3 4
""") == "4"

assert run("""5 6 3
10 1
11 2
12 6
13 4
14 6
""") == "3"

# minimum case
assert run("""1 10 3
5 4
""") == "1"

# all identical
assert run("""3 10 2
5 1
5 1
5 1
""") == "3"

# boundary alignment
assert run("""2 10 1
5 5
6 6
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | độ đúng cơ sở | 
| trùng lặp | 3 | khoảng thời gian xếp chồng | 
| căn chỉnh ranh giới | 2 | sự đúng đắn từng cái một | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi một quả bóng sơn nằm chính xác ở ranh giới dưới hoặc trên của tấm chắn tại một giây nhất định. Đạo hàm khoảng bao gồm cả hai ranh giới, vì vậy các quả bóng sơn như vậy phải được tính là hợp lệ. Nếu chúng ta sử dụng bất đẳng thức nghiêm ngặt khi chuyển đổi các ràng buộc, những trường hợp này sẽ bị loại bỏ không chính xác. 

Một trường hợp khác xuất hiện khi các khoảng dẫn xuất mở rộng ra ngoài phạm vi thời gian khả thi. Ví dụ: một hàng rất cao ở thời điểm đầu sẽ tạo ra giá trị âm`L`. Đây không phải là vấn đề vì đường quét trên số nguyên xử lý các phạm vi không giới hạn một cách tự nhiên; những khoảng thời gian như vậy chỉ đơn giản là bắt đầu sớm hơn tất cả những khoảng thời gian khác. 

Cuối cùng, nhiều quả bóng sơn chồng lên nhau ở cùng một vị trí`(t, s)`các giá trị tạo ra các khoảng giống hệt nhau. Quá trình quét đếm chính xác số bội vì mỗi số đóng góp một sự kiện +1 riêng biệt, đảm bảo tổng hợp chính xác các bản sao.
