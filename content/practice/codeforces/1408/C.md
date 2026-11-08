---
title: "CF 1408C - Gia tốc rời rạc"
description: "Hai ô tô xuất phát ở hai đầu đoạn đường có chiều dài l. Xe bên trái xuất phát ở vị trí 0 và di chuyển sang phải. Xe bên phải xuất phát ở vị trí l và di chuyển sang trái. Cả hai xe đều xuất phát với tốc độ 1. Dọc đường có cắm cờ theo tọa độ cố định."
date: "2026-06-11T07:41:45+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "implementation", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1408
codeforces_index: "C"
codeforces_contest_name: "Grakn Forces 2020"
rating: 1500
weight: 1408
solve_time_s: 147
verified: false
draft: false
---

[CF 1408C - Gia tốc rời rạc](https://codeforces.com/problemset/problem/1408/C) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, dp, triển khai, toán học, hai con trỏ 
**Thời gian giải:** 2m 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Hai ô tô xuất phát ở hai đầu đoạn đường dài`l`. Xe bên trái xuất phát tại vị trí`0`và di chuyển sang bên phải. Xe bên phải xuất phát tại vị trí`l`và di chuyển sang trái. 

Cả hai xe đều xuất phát với vận tốc`1`. Dọc đường có cờ đặt ở tọa độ cố định. Mỗi khi một ô tô vượt qua một lá cờ, chỉ có tốc độ của ô tô đó tăng thêm`1`. Hai xe chuyển động liên tục và đồng thời. 

Ta phải xác định chính xác thời điểm hai xe chiếm cùng một vị trí. 

Điều đầu tiên cần chú ý là các ô tô không thay đổi tốc độ một cách tùy tiện. Tốc độ của chúng chỉ thay đổi ở vị trí cờ. Giữa hai lần thay đổi tốc độ liên tiếp, mỗi ô tô chuyển động với vận tốc không đổi. Điều đó mang lại cho chuyển động một hình thức rất có cấu trúc. 

Số lượng cờ trong một bài kiểm tra có thể đạt tới`10^5`, nhưng tổng của tất cả các trường hợp thử nghiệm cũng bị giới hạn bởi`10^5`. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào sử dụng các bước thời gian nhỏ. Ngay cả việc mô phỏng mọi tương tác có thể có giữa cả hai chiếc xe cũng sẽ trở nên khó khăn nếu mỗi truy vấn yêu cầu phép tính bậc hai. 

Câu trả lời là một số thực, vì vậy chúng ta không tìm kiếm một đại lượng tổ hợp thuần túy. Chuyển động là liên tục. Điều đó thường gợi ý tìm kiếm hình học trực tiếp hoặc tìm kiếm nhị phân đúng thời gian. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
1
1 10
5
```Những chiếc xe đến cờ vào những thời điểm khác nhau. Xe bên trái đến sau`5`giây và tăng tốc, trong khi xe bên phải không bao giờ chạm vào lá cờ đó trước khi cuộc họp diễn ra. Giải pháp giả sử cả hai xe đều tăng tốc ở cùng một sự kiện sẽ đưa ra câu trả lời sai. 

Coi như:```
1
2 10
1 9
```Cả hai chiếc xe đều chạm cờ sau đúng một giây. Tốc độ của chúng trở nên`2`, và họ gặp nhau tại vị trí`5`sau hai giây nữa. Câu trả lời là chính xác`3`. Lỗi tích lũy dấu phẩy động sẽ hiển thị nếu quá trình triển khai cố gắng mô phỏng lặp lại các khoảng thời gian nhỏ. 

Một trường hợp tinh vi khác là khi cuộc gặp diễn ra trước khi một trong hai ô tô đến lá cờ tiếp theo. Ví dụ:```
1
2 10
4 6
```Sau khi cả hai ô tô đạt đến lá cờ đầu tiên, chúng di chuyển về phía nhau với tốc độ tăng dần. Cuộc họp có thể diễn ra trong một khoảng thời gian chứ không phải ở một lá cờ. Bất kỳ giải pháp nào chỉ kiểm tra thời gian đến của cờ sẽ bỏ lỡ điều này. 

Thách thức chính là tính toán các vị trí sau một khoảng thời gian tùy ý trong khi xử lý chính xác tốc độ không đổi từng phần. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ tự nhiên là mô phỏng các sự kiện. Mỗi sự kiện là thời điểm một trong những chiếc xe đạt đến lá cờ tiếp theo. Chúng ta có thể liên tục xác định sự kiện nào xảy ra trước, nâng cao thời gian cho sự kiện đó, cập nhật vị trí, tăng một tốc độ và tiếp tục cho đến khi hai xe gặp nhau. 

Điều này hoạt động về mặt khái niệm vì thay đổi tốc độ chỉ xảy ra ở cờ. Trạng thái chỉ thay đổi tại một số thời điểm hữu hạn. 

Khó khăn là cuộc gặp gỡ có thể xảy ra giữa hai sự kiện. Chúng tôi phải liên tục kiểm tra xem khoảng cách còn lại giữa các xe có gần nhau hay không trước sự kiện cờ tiếp theo. Việc thực hiện trở nên khá tinh tế. 

Một khả năng khác là mô phỏng chuyển động với khoảng thời gian rất nhỏ. Điều đó rõ ràng là quá chậm. Ngay cả độ chính xác của`10^-7`trên con đường dài`10^9`sẽ yêu cầu một số bước vô cùng lớn. 

Quan sát quan trọng là nếu chúng ta ấn định một thời gian`T`, chúng ta có thể xác định chính xác vị trí của mỗi chiếc xe sau đó`T`giây. 

Đối với ô tô bên trái, chúng ta biết trình tự các đoạn nó đi qua:```
0 -> a1 -> a2 -> ... -> an -> l
```Trên đoạn`i`, tốc độ của nó đã biết. Chúng ta có thể dành nhiều thời gian có sẵn`T`càng tốt khi di chuyển qua các phân đoạn hoàn chỉnh. Khi thời gian còn lại không đủ để hoàn thành đoạn tiếp theo, chúng tôi dừng lại ở đâu đó bên trong đoạn đó và tính toán vị trí chính xác. 

Ý tưởng tương tự hoạt động độc lập cho chiếc xe phù hợp. 

Bây giờ xác định:```
f(T) = position_of_left_car_after_T
g(T) = position_of_right_car_after_T
```Vị trí bên trái tăng theo thời gian. Vị trí bên phải giảm dần theo thời gian. 

Những chiếc xe đã gặp nhau đúng lúc`T`chính xác khi nào:```
f(T) >= g(T)
```Tình trạng này là đơn điệu. Nếu nó đúng trong một thời điểm nào đó thì nó vẫn đúng trong mọi thời điểm lớn hơn. Điều đó làm cho tìm kiếm nhị phân có thể thực hiện được. 

Chúng tôi tìm kiếm nhị phân trong thời gian nhỏ nhất mà hai vị trí giao nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng sự kiện | O(n) mỗi lần kiểm tra | O(1) | Được chấp nhận, nhưng phức tạp hơn | 
| Tìm kiếm nhị phân + Đánh giá vị trí | O(n log L) | O(1) | Đã chấp nhận | 

Đây`L`đại diện cho phạm vi tìm kiếm theo thời gian. Khoảng 60 lần lặp tìm kiếm nhị phân là đủ để có độ chính xác gấp đôi. 

## Hướng dẫn thuật toán 

### Tính toán vị trí trong một thời gian cố định 

Giả sử chúng ta muốn vị trí của xe bên trái sau thời gian`T`. 

Xe xuất phát tại vị trí`0`với tốc độ`1`. 

Bất cứ khi nào nó vượt qua một lá cờ, tốc độ sẽ tăng thêm`1`. 

Chúng tôi xử lý các đoạn đường từ trái sang phải. 

Nếu độ dài đoạn hiện tại là`d`và tốc độ hiện tại là`v`, khi đó đi qua toàn bộ đoạn đường cần có:```
d / v
```giây. 

Nếu còn đủ thời gian, chúng ta sẽ tiêu tốn lượng thời gian đó, di chuyển về cuối đoạn đường và tăng tốc độ. 

Nếu không, chiếc xe vẫn nằm trong phân khúc. Vị trí của nó là:```
current_position + remaining_time * v
```và chúng tôi dừng lại. 

Xe bên phải được tính đối xứng từ đầu bên phải. 

### Tìm kiếm nhị phân 

1. Đặt`low = 0`. 
2. Đặt`high = l`. 

Thời gian họp không bao giờ có thể vượt quá`l`, vì ngay cả khi không tăng tốc thì các ô tô vẫn tiếp cận nhau với vận tốc tổng hợp`2`, cuộc họp trong`l/2`giây. 
3. Lặp lại khoảng 60 lần. 
4. Hãy để`mid = (low + high) / 2`. 
5. Tính vị trí xe bên trái sau`mid`giây. 
6. Tính toán vị trí ô tô bên phải sau`mid`giây. 
7. Nếu vị trí bên trái ít nhất là vị trí bên phải thì các xe đã gặp nhau theo thời gian`mid`, vì vậy hãy di chuyển giới hạn trên sang`mid`. 
8. Nếu không cuộc họp sẽ diễn ra muộn hơn, vì vậy hãy chuyển giới hạn dưới sang`mid`. 
9. Sau khi lặp lại xong, xuất ra`high`. 

### Tại sao nó hoạt động 

Đối với bất kỳ thời gian cố định`T`, các tính toán vị trí tái tạo chính xác chuyển động vật lý vì mọi thay đổi tốc độ chỉ xảy ra khi đi ngang qua một đoạn hoàn chỉnh. Do đó, việc mô phỏng một chiếc ô tô là chính xác. 

Định nghĩa:```
h(T) = left_position(T) - right_position(T)
```Ban đầu`h(0) < 0`. Khi thời gian tăng lên, vị trí bên trái không bao giờ giảm và vị trí bên phải không bao giờ tăng, do đó`h(T)`đơn điệu ngày càng tăng. 

Hai xe gặp nhau đúng lúc`h(T) >= 0`. 

Vì vị từ là đơn điệu nên tìm kiếm nhị phân sẽ tìm thấy điểm chuyển tiếp duy nhất giữa "chưa gặp" và "đã gặp". Giảm một nửa lặp đi lặp lại hội tụ về thời gian gặp mặt thực sự với sai số nhỏ tùy ý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def left_pos(t, a, l):
    pos = 0.0
    speed = 1.0

    for flag in a:
        dist = flag - pos
        need = dist / speed

        if t >= need:
            t -= need
            pos = flag
            speed += 1.0
        else:
            return pos + speed * t

    dist = l - pos
    need = dist / speed

    if t >= need:
        return float(l)

    return pos + speed * t

def right_pos(t, a, l):
    pos = float(l)
    speed = 1.0

    for flag in reversed(a):
        dist = pos - flag
        need = dist / speed

        if t >= need:
            t -= need
            pos = float(flag)
            speed += 1.0
        else:
            return pos - speed * t

    dist = pos
    need = dist / speed

    if t >= need:
        return 0.0

    return pos - speed * t

def solve():
    t = int(input())

    ans = []

    for _ in range(t):
        n, l = map(int, input().split())
        a = list(map(int, input().split()))

        lo = 0.0
        hi = float(l)

        for _ in range(70):
            mid = (lo + hi) / 2.0

            lp = left_pos(mid, a, l)
            rp = right_pos(mid, a, l)

            if lp >= rp:
                hi = mid
            else:
                lo = mid

        ans.append(f"{hi:.15f}")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Các chức năng`left_pos`Và`right_pos`thực hiện truy vấn vị trí thời gian cố định. Mỗi chức năng sẽ đi qua trình tự các phân đoạn, tiêu tốn thời gian miễn là có thể hoàn thành toàn bộ phân đoạn. 

Một lỗi phổ biến là tăng tốc độ trước khi chạm cờ. Việc triển khai trước tiên sẽ kiểm tra xem có đủ thời gian để hoàn thành phân đoạn hay không. Chỉ sau khi đến cờ, tốc độ mới tăng lên. 

Một chi tiết tinh tế khác là đoạn cuối cùng. Sau lá cờ cuối cùng, ô tô tiếp tục đi về điểm cuối với tốc độ cuối cùng. Mã xử lý phân đoạn đó giống hệt như mọi phân đoạn khác. 

Tìm kiếm nhị phân sử dụng 70 lần lặp. Vì mỗi lần lặp lại giảm một nửa khoảng thời gian tìm kiếm nên điều này là quá đủ để đạt được yêu cầu`10^-6`độ chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
2 10
1 9
```Tìm kiếm nhị phân cuối cùng hội tụ đến`3`. 

Tại`T = 3`: 

####Xe trái 

| Phân đoạn | Tốc độ | Thời gian cần thiết | Thời gian còn lại trước khi phân đoạn | Vị trí Sau | 
| --- | --- | --- | --- | --- | 
| 0 → 1 | 1 | 1 | 3 | 1 | 
| 1 → 9 | 2 | 4 | 2 | phân khúc bên trong | 

Vị trí cuối cùng:```
1 + 2 × 2 = 5
```#### Đúng xe 

| Phân đoạn | Tốc độ | Thời gian cần thiết | Thời gian còn lại trước khi phân đoạn | Vị trí Sau | 
| --- | --- | --- | --- | --- | 
| 10 → 9 | 1 | 1 | 3 | 9 | 
| 9 → 1 | 2 | 4 | 2 | phân khúc bên trong | 

Vị trí cuối cùng:```
9 - 2 × 2 = 5
```Cả hai xe đều ở vị trí`5`, xác nhận câu trả lời. 

### Ví dụ 2 

đầu vào:```
1
1 10
1
```Câu trả lời là`11/3 ≈ 3.666666666667`. 

####Xe trái 

| Phân đoạn | Tốc độ | Thời gian cần thiết | 
| --- | --- | --- | 
| 0 → 1 | 1 | 1 | 

Sau một giây xe bên trái tới cờ và tốc độ của nó trở thành`2`. 

#### Đúng xe 

Không có lá cờ nào đứng về phía nó trước khi gặp nhau nên tốc độ của nó vẫn được giữ nguyên`1`. 

Khoảng cách còn lại giữa các vị trí`1`Và`10`là:```
9
```Tốc độ tổng hợp của họ là:```
2 + 1 = 3
```Thời gian bổ sung:```
9 / 3 = 3
```Tổng cộng:```
1 + 3 = 11/3
```Ví dụ này chứng tỏ rằng gia tốc của mỗi ô tô chỉ phụ thuộc vào cờ mà nó đạt được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log L) | Mỗi lần lặp tìm kiếm nhị phân sẽ quét tất cả các cờ một lần từ mỗi phía | 
| Không gian | O(1) | Chỉ có một vài biến ngoài mảng đầu vào | 

Với khoảng 70 lần lặp tìm kiếm nhị phân và tổng cộng nhiều nhất là`10^5`cờ trên tất cả các trường hợp thử nghiệm, tổng công việc là khoảng`70 × 2 × 10^5`, vừa vặn một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def left_pos(t, a, l):
        pos = 0.0
        speed = 1.0

        for flag in a:
            dist = flag - pos
            need = dist / speed

            if t >= need:
                t -= need
                pos = flag
                speed += 1.0
            else:
                return pos + speed * t

        return min(float(l), pos + speed * t)

    def right_pos(t, a, l):
        pos = float(l)
        speed = 1.0

        for flag in reversed(a):
            dist = pos - flag
            need = dist / speed

            if t >= need:
                t -= need
                pos = float(flag)
                speed += 1.0
            else:
                return pos - speed * t

        return max(0.0, pos - speed * t)

    tc = int(input())
    out = []

    for _ in range(tc):
        n, l = map(int, input().split())
        a = list(map(int, input().split()))

        lo, hi = 0.0, float(l)

        for _ in range(70):
            mid = (lo + hi) / 2

            if left_pos(mid, a, l) >= right_pos(mid, a, l):
                hi = mid
            else:
                lo = mid

        out.append(f"{hi:.6f}")

    return "\n".join(out)

# sample-style checks
res = run("1\n2 10\n1 9\n").strip()
assert abs(float(res) - 3.0) < 1e-5

res = run("1\n1 10\n1\n").strip()
assert abs(float(res) - 3.666666666666667) < 1e-5

# minimum case
res = run("1\n1 2\n1\n").strip()
assert abs(float(res) - 1.0) < 1e-5

# symmetric flags
res = run("1\n2 10\n4 6\n").strip()
assert float(res) > 0

# large coordinates
res = run("1\n2 1000000000\n413470354 982876160\n").strip()
assert float(res) > 0

# meeting exactly after simultaneous accelerations
res = run("1\n2 10\n1 9\n").strip()
assert abs(float(res) - 3.0) < 1e-5
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 / 1`|`1`| Con đường nhỏ ý nghĩa nhất | 
|`2 10 / 1 9`|`3`| Tăng tốc đồng thời ở cả hai bên | 
|`2 10 / 4 6`| Câu trả lời hữu hạn tích cực | Gặp gỡ trong một khoảng thời gian | 
| Tọa độ lớn gần`10^9`| Câu trả lời hữu hạn tích cực | Độ ổn định điểm nổi | 
| Cờ đơn gần một điểm cuối | Câu trả lời bất đối xứng đúng | Thay đổi tốc độ độc lập | 

## Vỏ cạnh 

###Họp mặt trước lá cờ tiếp theo 

đầu vào:```
1
2 10
4 6
```Sau khi cả hai ô tô đến lá cờ gần nhất, tốc độ của chúng trở thành`2`. Cuộc họp diễn ra ở đâu đó giữa các lá cờ chứ không phải chính xác ở vị trí lá cờ. 

Các hàm truy vấn vị trí xử lý việc này một cách tự nhiên vì chúng dừng bên trong một phân đoạn bất cứ khi nào thời gian còn lại không đủ để đến cờ tiếp theo. 

### Xe này tăng tốc còn xe kia thì không 

đầu vào:```
1
1 10
1
```Xe bên trái đến cờ sau một giây và tăng tốc`2`. Xe bên phải giữ tốc độ`1`. 

Thuật toán tính toán từng chiếc xe một cách độc lập. Không có giả định nào được đưa ra về các sự kiện tăng tốc xảy ra đồng thời, vì vậy câu trả lời vẫn đúng. 

### Gặp nhau đúng cùng tọa độ sau khi vượt cờ 

đầu vào:```
1
2 10
1 9
```Cả hai xe đều chạm cờ sau một giây và đồng thời tăng tốc. Vào thời điểm`3`, cả hai vị trí đều bằng nhau`5`. 

Tìm kiếm nhị phân sử dụng điều kiện:```
left_position >= right_position
```vì vậy sự bình đẳng được coi là trạng thái gặp gỡ hợp lệ. Điểm chuyển tiếp được tìm thấy chính xác ngay cả khi các ô tô gặp nhau đúng chứ không phải vượt qua một cách nghiêm ngặt. 

### Tọa độ rất lớn 

đầu vào:```
1
2 1000000000
413470354 982876160
```Khoảng cách và thời gian có thể gần bằng`10^9`. Thuật toán không bao giờ thực hiện mô phỏng thời gian nguyên. Nó chỉ sử dụng số học về độ dài phân đoạn và tìm kiếm nhị phân gấp đôi, vẫn chính xác trong phạm vi dung sai yêu cầu.
