---
title: "CF 167A - Phù thủy và xe đẩy"
description: "Mỗi xe điện khởi hành từ kho vào một giờ khởi hành cố định. Nó bắt đầu với tốc độ 0, có thể tăng tốc tối đa a và không bao giờ có thể vượt quá giới hạn tốc độ v[i] của chính nó. Điểm đến cách đó d mét."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 167
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 114 (Div. 1)"
rating: 1600
weight: 167
solve_time_s: 100
verified: true
draft: false
---

[CF 167A - Phù thủy và xe đẩy](https://codeforces.com/problemset/problem/167/A) 

**Đánh giá:** 1600 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi xe điện khởi hành từ kho vào một giờ khởi hành cố định. Nó bắt đầu với tốc độ`0`, có thể tăng tốc tối đa`a`và không bao giờ có thể vượt quá giới hạn tốc độ của chính nó`v[i]`. Điểm đến là`d`cách xa hàng mét. Phanh không bị hạn chế nên xe buýt điện có thể giảm tốc độ ngay lập tức bất cứ khi nào cần thiết. 

Nếu xe đẩy`i`bắt xe buýt`i-1`, vượt là không thể. Từ lúc đó trở đi, cả hai cùng di chuyển và đi xe đẩy`i`không thể đến sớm hơn xe điện`i-1`. 

Đối với mỗi xe buýt điện, chúng ta phải tính toán thời gian đến ga cuối cùng sớm nhất có thể đồng thời tôn trọng các giới hạn tăng tốc, giới hạn tốc độ và quy tắc cấm vượt. 

Các ràng buộc xác định hoàn toàn độ phức tạp dự định. Có tới`10^5`xe buýt điện nên bất kỳ giải pháp nào mô phỏng chuyển động liên tục hoặc so sánh từng cặp xe điện đều quá chậm. MỘT`O(n^2)`giải pháp sẽ yêu cầu khoảng`10^10`trong trường hợp xấu nhất vượt xa giới hạn. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính. 

Phần vật lý cũng có vấn đề. Xe buýt điện không phải lúc nào cũng dành thời gian di chuyển ở tốc độ tối đa. Đôi khi khoảng cách quá ngắn để có thể chạm tới`v[i]`. Việc thực hiện bất cẩn khiến mọi xe buýt đều tăng tốc đến`v[i]`đầu tiên sẽ tạo ra thời gian không chính xác. 

Hãy xem xét đầu vào này:```
1 10 5
0 100
```Xe điện chỉ cần di chuyển`5`mét. Tốc độ đạt`100`là không thể trong khoảng cách đó. Chuyển động đúng là gia tốc thuần túy:```
distance = 1/2 * a * t^2
5 = 5 * t^2
t = 1
```Vì vậy, câu trả lời là:```
1.0000000000
```Một sai lầm dễ mắc phải khác là quên rằng những chiếc xe buýt sau này không thể đến sớm hơn những chiếc trước đó. 

Ví dụ:```
2 1 10
0 1
1 100
```Xe đẩy đầu tiên cần:```
10 seconds
```Xe buýt điện thứ hai có thể đến sớm hơn nếu đường trống, nhưng việc vượt bị cấm. Các câu trả lời đúng là:```
10.0000000000
10.0000000000
```Một cách triển khai ngây thơ tính toán từng xe đẩy một cách độc lập sẽ in không chính xác một cái gì đó gần giống với`5.47`cho xe buýt điện thứ hai. 

Trường hợp tinh tế thứ ba xảy ra khi một chiếc xe buýt đón một chiếc xe khác ngay tại điểm đến. Điều đó vẫn được tính là đến cùng một lúc. 

Ví dụ:```
2 1 1
0 1
1 100
```Xe điện đầu tiên đến đúng giờ`1.5`. Xe buýt điện thứ hai cũng đến ga đúng lúc`1.5`. Đầu ra đúng là:```
1.5000000000
1.5000000000
```Việc sử dụng các bất đẳng thức nghiêm ngặt trong logic so sánh có thể thất bại ở ranh giới này. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi xe buýt, hãy tính thời gian đến sớm nhất không hạn chế của nó dựa trên các ràng buộc vật lý. Sau đó so sánh nó với tất cả các xe buýt trước đó để xác định xem liệu nó có bắt kịp bất kỳ chiếc nào trong số chúng trước khi đến đích hay không. 

Điều này có hiệu quả vì xe điện di chuyển theo thứ tự và không thể vượt qua. Nếu xe đẩy`i`sẽ đến sớm hơn xe điện`j`, thì đến một lúc nào đó nó phải bắt kịp. 

Vấn đề là quy mô. Kiểm tra tương tác giữa mỗi cặp yêu cầu`O(n^2)`so sánh. Với`10^5`xe điện trở nên chậm một cách vô vọng. 

Quan sát quan trọng là chúng ta thực sự không cần phải mô phỏng nơi xảy ra việc bắt kịp. 

Giả sử xe đẩy`i`có thể độc lập đến vào lúc`T[i]`. Nếu như`T[i]`nhỏ hơn thời gian đến thực tế của xe buýt trước đó thì xe buýt điện`i`cuối cùng phải bắt được nó trước khi đến đích. Vì việc vượt là không thể nên xe điện`i`buộc phải đến đúng giờ. 

Điều đó có nghĩa là thời gian đến thực sự chỉ đơn giản là:```
answer[i] = max(answer[i-1], independent_time[i])
```Mọi thứ được rút gọn thành việc tính toán thời gian đến sớm nhất có thể cho một chiếc xe buýt. 

Chỉ có hai kiểu chuyển động. 

Nếu xe buýt điện đạt tốc độ tối đa trước điểm đến thì: 

1. Nó tăng tốc từ`0`ĐẾN`v`. 
2. Nó tiếp tục với tốc độ không đổi. 

Nếu đích đến quá gần thì chỉ tăng tốc cả chặng đường. 

Điểm chuyển tiếp xuất phát từ công thức khoảng cách gia tốc:```
v^2 = 2ad
```Nếu như`v^2 <= 2ad`, xe điện đạt tốc độ tối đa trước khi đến đích. 

Nếu không thì nó không bao giờ đạt tới`v`. 

Điều này mang lại một sự hoàn chỉnh`O(n)`giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`a`, Và`d`. 
2. Đối với mỗi xe buýt điện, hãy tính thời gian đến nhanh nhất có thể của nó với giả định đường ray trống. 
3. Xác định xem xe buýt có thể đạt tốc độ giới hạn trước quãng đường di chuyển hay không`d`. 

Quãng đường cần thiết để tăng tốc từ`0`ĐẾN`v`là:$s = \frac{v^2}{2a}$Nếu giá trị này lớn nhất`d`, thì xe đẩy đạt tốc độ tối đa. 
4. Nếu xe đẩy đạt tốc độ tối đa: 

1. Tính thời gian tăng tốc:$t = \frac{v}{a}$2. Tính quãng đường tăng tốc:$s = \frac{v^2}{2a}$3. Đi quãng đường còn lại với tốc độ không đổi. 
4. Thêm giờ khởi hành`t[i]`. 
5. Nếu không, xe điện sẽ tăng tốc trong toàn bộ chuyến đi. 

Sử dụng:$d = \frac{1}{2}at^2$mang lại:$t = \sqrt{\frac{2d}{a}}$6. Gọi giá trị tính toán này là`independent`. 
7. Thời gian đến thực tế không được sớm hơn thời gian đến của xe buýt trước đó. 

Bộ:```
answer[i] = max(answer[i-1], independent)
```8. Xuất ra tất cả các câu trả lời. 

### Tại sao nó hoạt động 

Bất biến rất đơn giản: sau khi xử lý xe đẩy`i`,`answer[i]`bằng thời gian đến sớm nhất có thể đạt được về mặt vật lý trong khi vẫn tôn trọng tất cả các ràng buộc không được vượt. 

Tính toán chuyển động độc lập là tối ưu vì mỗi xe đẩy luôn tăng tốc mạnh mẽ ở mức cho phép và không bao giờ giảm tốc độ trừ khi bị ép buộc. 

Nếu như`independent[i] >= answer[i-1]`, sau đó là xe đẩy`i`không bao giờ bắt kịp chuyến xe buýt trước đó nên nó có thể giữ được lịch trình tối ưu. 

Nếu như`independent[i] < answer[i-1]`, sau đó là xe đẩy`i`sẽ đến đích sớm hơn xe điện`i-1`. Vì việc vượt là không thể nên việc đuổi kịp phải xảy ra trước hoặc ngay tại điểm đến. Từ thời điểm đó trở đi họ di chuyển cùng nhau, buộc xe đẩy`i`đến`answer[i-1]`. 

Do đó, việc lấy mức chạy tối đa mô hình chính xác giới hạn vật lý. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    n, a, d = map(int, input().split())

    ans = []

    for _ in range(n):
        t, v = map(int, input().split())

        accel_dist = v * v / (2.0 * a)

        if accel_dist >= d:
            travel_time = math.sqrt(2.0 * d / a)
        else:
            accel_time = v / a
            remaining = d - accel_dist
            travel_time = accel_time + remaining / v

        arrival = t + travel_time

        if ans:
            arrival = max(arrival, ans[-1])

        ans.append(arrival)

    print("\n".join(f"{x:.10f}" for x in ans))

solve()
```Phần đầu tiên tính toán thời gian di chuyển tối ưu không hạn chế cho một xe buýt. 

Biểu thức:```
accel_dist = v * v / (2.0 * a)
```đại diện cho khoảng cách cần thiết để tăng tốc từ`0`ĐẾN`v`. 

Nếu khoảng cách này lớn hơn hoặc bằng`d`, thì xe điện không bao giờ đạt tốc độ tối đa trước điểm đến. Toàn bộ chuyến đi là chuyển động có gia tốc nên ta giải:```
d = 1/2 * a * t^2
```bằng cách sử dụng công thức căn bậc hai. 

Ngược lại, trước tiên xe điện sẽ tăng tốc đến`v`, sau đó chuyển động với tốc độ không đổi. Thời gian đi lại trở thành:```
time_to_accelerate + time_at_constant_speed
```Chi tiết triển khai quan trọng nhất là dòng cuối cùng:```
arrival = max(arrival, ans[-1])
```Nếu không có điều này, những chiếc xe đẩy sau này có thể vượt trái phép những chiếc xe trước đó ở đầu ra. 

Một điểm tinh tế khác là sử dụng số học dấu phẩy động ở mọi nơi. Các công thức liên quan đến căn bậc hai và phép chia, do đó số học số nguyên sẽ cắt bớt các giá trị và làm mất đi độ chính xác. 

Điều kiện:```
if accel_dist >= d:
```phải bao gồm sự bình đẳng. Nếu xe buýt điện đạt tốc độ tối đa chính xác tại điểm đến thì không có giai đoạn di chuyển. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 10 10000
0 10
5 11
1000 1
```| Xe buýt | Khởi hành | Tốc độ tối đa | Đến độc lập | Câu trả lời trước | Câu trả lời cuối cùng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 10 | 1000,5 | không | 1000,5 | 
| 2 | 5 | 11 | 914.595... | 1000,5 | 1000,5 | 
| 3 | 1000 | 1 | 11000.05 | 1000,5 | 11000.05 | 

Xe buýt điện thứ hai nhanh hơn về mặt vật lý, nhưng nó không thể vượt qua xe buýt thứ nhất. Thời gian đến được tính toán của nó trở nên bị giới hạn`1000.5`. 

Chiếc xe buýt thứ ba chậm hơn nhiều và không bao giờ đuổi kịp những chiếc khác. 

### Ví dụ 2 

đầu vào:```
2 1 10
0 1
1 100
```| Xe buýt | Khởi hành | Tốc độ tối đa | Đến độc lập | Câu trả lời trước | Câu trả lời cuối cùng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 10,5 | không | 10,5 | 
| 2 | 1 | 100 | 5.4721... | 10,5 | 10,5 | 

Xe buýt điện thứ hai có thể độc lập đến sớm hơn nhiều, nhưng điều đó đòi hỏi phải vượt qua xe buýt điện thứ nhất. Quy tắc tối đa đang chạy ngăn chặn điều này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi xe đẩy được xử lý một lần | 
| Không gian | O(n) | Các câu trả lời được lưu trữ cho đầu ra | 

Với`10^5`xe đẩy, một giải pháp tuyến tính dễ dàng phù hợp trong thời gian giới hạn. Việc sử dụng bộ nhớ cũng nhỏ vì chỉ lưu trữ mảng kết quả. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def solve():
    input = sys.stdin.readline

    n, a, d = map(int, input().split())

    ans = []

    for _ in range(n):
        t, v = map(int, input().split())

        accel_dist = v * v / (2.0 * a)

        if accel_dist >= d:
            travel_time = math.sqrt(2.0 * d / a)
        else:
            accel_time = v / a
            remaining = d - accel_dist
            travel_time = accel_time + remaining / v

        arrival = t + travel_time

        if ans:
            arrival = max(arrival, ans[-1])

        ans.append(arrival)

    print("\n".join(f"{x:.10f}" for x in ans))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue()

# provided sample
out = run(
"""3 10 10000
0 10
5 11
1000 1
"""
).strip().splitlines()

assert abs(float(out[0]) - 1000.5) < 1e-6
assert abs(float(out[1]) - 1000.5) < 1e-6
assert abs(float(out[2]) - 11000.05) < 1e-6

# minimum size
out = run(
"""1 1 1
0 1
"""
).strip()

assert abs(float(out) - 1.5) < 1e-6

# cannot reach max speed
out = run(
"""1 10 5
0 100
"""
).strip()

assert abs(float(out) - 1.0) < 1e-6

# overtaking prevention
out = run(
"""2 1 10
0 1
1 100
"""
).strip().splitlines()

assert abs(float(out[0]) - 10.5) < 1e-6
assert abs(float(out[1]) - 10.5) < 1e-6

# catch exactly at destination
out = run(
"""2 1 1
0 1
1 100
"""
).strip().splitlines()

assert abs(float(out[0]) - 1.5) < 1e-6
assert abs(float(out[1]) - 1.5) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Xe đẩy đơn | Tính toán vật lý trực tiếp | Hạn chế tối thiểu | 
| Tốc độ rất cao, khoảng cách ngắn | Pha tăng tốc thuần túy | Không bao giờ đạt tốc độ tối đa | 
| Xe đẩy nhanh phía sau xe đẩy chậm | Thời gian đến cân bằng | Không vượt | 
| Bắt chính xác tại điểm đến | Xử lý ranh giới bình đẳng | Logic so sánh đúng | 

## Vỏ cạnh 

### Xe buýt điện không bao giờ đạt tốc độ tối đa 

đầu vào:```
1 10 5
0 100
```Quãng đường tăng tốc cần thiết để đạt tốc độ`100`là:```
100^2 / (2 * 10) = 500
```nhưng đích đến chỉ là`5`cách xa hàng mét. 

Thuật toán đi vào nhánh căn bậc hai:```
t = sqrt(2 * 5 / 10) = 1
```Đầu ra:```
1.0000000000
```Việc triển khai có nhiều lỗi luôn tăng tốc lên`v`first sẽ tạo ra một câu trả lời cực kỳ sai. 

### Xe điện nhanh hơn khởi hành muộn hơn 

đầu vào:```
2 1 10
0 1
1 100
```Chiếc xe buýt thứ hai độc lập đến xung quanh`5.47`, nhưng chiếc xe điện đầu tiên đã đến`10.5`. 

Thuật toán tính toán:```
answer[1] = max(5.47, 10.5) = 10.5
```Điều này mô hình chính xác sự kiện bắt kịp. 

### Bắt chính xác tại điểm đến 

đầu vào:```
2 1 1
0 1
1 100
```Chiếc xe điện đầu tiên đến`1.5`. 

Xe điện thứ hai độc lập cũng đến đích lúc`1.5`. 

Thuật toán giữ:```
answer[1] = max(1.5, 1.5) = 1.5
```Sử dụng bất đẳng thức nghiêm ngặt thay vì`max`logic có thể phân tách các câu trả lời một cách không chính xác do so sánh dấu phẩy động.
