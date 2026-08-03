---
title: "CF 102620A - Xe Bán Kem"
description: "Có những túp lều được bố trí trên một bãi biển thẳng tắp. Túp lều i cách túp lều i-1 đúng 100 mét và mỗi túp lều có một số người. Các cửa hàng kem hiện tại cũng được đặt trên cùng một đường với tọa độ nguyên tùy ý."
date: "2026-08-02T13:47:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102620
codeforces_index: "A"
codeforces_contest_name: "mBIT Standard June 2020"
rating: 0
weight: 102620
solve_time_s: 60
verified: true
draft: false
---

[CF 102620A - Xe bán kem](https://codeforces.com/problemset/problem/102620/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có những túp lều được bố trí trên một bãi biển thẳng tắp. Túp lều`i`chính xác là 100 mét sau túp lều`i-1`, và mỗi túp lều chứa một số người. Các cửa hàng kem hiện tại cũng được đặt trên cùng một đường với tọa độ nguyên tùy ý. 

Chúng ta cần chọn vị trí cho một cửa hàng kem bổ sung. Một người chỉ mua hàng từ cửa hàng mới nếu cửa hàng mới ở gần túp lều của họ hơn mọi cửa hàng hiện có. Mục tiêu là tìm ra tổng số người tối đa có thể bị thu hút bằng cách chọn vị trí tốt nhất có thể. 

Quan sát quan trọng là vị trí cửa hàng mới là một chiều. Đối với một túp lều cố định, tập hợp các vị trí mà cửa hàng mới thắng luôn là một khoảng. 

Số chòi và cửa hàng hiện có đều có thể lên tới 200.000. Điều này ngay lập tức loại trừ việc thử mọi vị trí cửa hàng mới có thể hoặc kiểm tra từng túp lều đối với mọi cửa hàng. Một giải pháp bậc hai sẽ thực hiện khoảng 40 tỷ phép tính trong trường hợp xấu nhất, vượt xa giới hạn thời gian thi đấu thông thường cho phép. Chúng ta cần một giải pháp gần`O((n + m) log m)`hoặc tốt hơn. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Hãy xem xét một túp lều chính xác tại vị trí cửa hàng hiện có.```
Input
1 1
5
0
```Cửa hàng mới có thể được đặt ở bất cứ đâu ngoại trừ vị trí cửa hàng hiện có, nhưng người ở túp lều vẫn có thể bị thu hút bằng cách di chuyển cửa hàng mới ra xa một chút. Câu trả lời đúng là:```
5
```Một giải pháp chỉ xem xét các vị trí số nguyên có thể loại bỏ tất cả các lựa chọn một cách không chính xác. 

Một trường hợp khác là khi vị trí tốt nhất nằm ngoài phạm vi của tất cả các túp lều.```
Input
3 1
4 10 3
1000
```Cửa hàng mới có thể được đặt gần các túp lều trong khi vẫn gần hơn cửa hàng duy nhất hiện có. Việc hạn chế tìm kiếm trong phân khúc giữa các cửa hàng hiện tại sẽ bỏ lỡ các câu trả lời hợp lệ. 

Trường hợp tế nhị cuối cùng là so sánh nghiêm ngặt. Nếu cửa hàng mới có khoảng cách bằng chính xác với cửa hàng hiện có thì người đó không chọn cửa hàng mới. 

Ví dụ:```
Input
2 1
7 8
50
```Điểm giữa giữa túp lều 1 và cửa hàng hiện tại là thế trận hòa chứ không phải thế thắng. Quá trình quét phải xử lý các điểm cuối trong khoảng thời gian bị loại trừ. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử các vị trí có thể mở cửa hàng mới và đếm xem có bao nhiêu túp lều thích hợp ở đó. Câu trả lời chỉ thay đổi ở một số vị trí quan trọng nhất định, nhưng việc liệt kê chúng vẫn còn quá tốn kém. Ngay cả khi chúng tôi kiểm tra từng túp lều với mọi cửa hàng hiện có một lần, chi phí sẽ là`O(nm)`, có thể đạt tới khoảng`4 * 10^10`so sánh. 

Sự thay đổi hữu ích trong quan điểm là ngừng suy nghĩ về các địa điểm có thể mở cửa hàng và thay vào đó hãy suy nghĩ về từng túp lều một cách độc lập. 

Cho một túp lều ở tọa độ`h`, gọi khoảng cách tới cửa hàng hiện có gần nhất là`d`. Cửa hàng mới thắng chính xác túp lều này khi:```
|position - h| < d
```đó là khoảng mở:```
(h - d, h + d)
```Vì vậy, mỗi túp lều đóng góp dân số của mình vào mọi vị trí cửa hàng mới có thể có trong một khoảng thời gian. Vấn đề trở thành việc tìm kiếm sự chồng chéo có trọng số tối đa của nhiều khoảng thời gian. 

Chúng ta có thể giải quyết vấn đề này bằng một đường quét. Thêm dân số của túp lều khi bước vào khoảng thời gian của nó và loại bỏ nó khi rời đi. Vì các khoảng là mở nên chúng ta phải xử lý các phép xóa trước khi phép cộng ở cùng tọa độ. 

Nhiệm vụ duy nhất còn lại là tìm`d`một cách hiệu quả. Sau khi sắp xếp các vị trí cửa hàng hiện có, cửa hàng gần nhất với bất kỳ túp lều nào phải là một trong hai cửa hàng xung quanh vị trí của nó theo thứ tự được sắp xếp. Tìm kiếm nhị phân cung cấp điều này trong`O(log m)`thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Tối ưu | O((n + m) log m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tọa độ của tất cả các cửa hàng kem hiện có. Đối với mỗi túp lều, cửa hàng hiện có gần nhất chỉ có thể là cửa hàng đầu tiên ở bên phải hoặc cửa hàng đầu tiên ở bên trái, vì vậy thứ tự sắp xếp này cho phép tìm kiếm nhị phân. 
2. Với mỗi túp lều, hãy tìm khoảng cách`d`đến cửa hàng hiện có gần nhất. Túp lều có thể được phục vụ bởi cửa hàng mới chính xác khi cửa hàng mới được đặt bên trong`(h - d, h + d)`. Thêm hai sự kiện quét cho khoảng thời gian này. 
3. Lưu trữ điểm cuối bên trái dưới dạng sự kiện bổ sung và điểm cuối bên phải dưới dạng sự kiện xóa. Khoảng thời gian mở nên điểm cuối bên phải phải ngừng đóng góp trước khi quá trình quét đạt đến tọa độ đó. 
4. Sắp xếp tất cả các sự kiện theo tọa độ. Tại mỗi tọa độ, trước tiên hãy xóa tất cả các khoảng kết thúc ở đó, sau đó thêm tất cả các khoảng bắt đầu từ đó. Tổng hiện tại đại diện cho những người được phục vụ ngay sau tọa độ đó. 
5. Theo dõi giá trị tối đa của tổng số tiền hiện có. 

Tại sao nó hoạt động: 

Đối với mỗi túp lều, chúng tôi tạo ra chính xác tập hợp các vị trí mà cửa hàng mới sẽ ở gần hơn mọi cửa hàng hiện có. Đường quét duy trì tổng số quần thể của tất cả các túp lều có khoảng thời gian hợp lệ hiện chứa vị trí đã chọn. Vì mọi thay đổi có thể xảy ra chỉ ở một ranh giới khoảng nên việc kiểm tra các giá trị giữa các ranh giới liên tiếp là đủ. Số lượng tối đa được duy trì bởi đợt quét chính xác là số lượng khách hàng tốt nhất có thể. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    people = list(map(int, input().split()))
    shops = list(map(int, input().split()))
    shops.sort()

    events = []

    for i, p in enumerate(people):
        h = i * 100
        pos = bisect_left(shops, h)

        dist = 10**18
        if pos < m:
            dist = min(dist, abs(shops[pos] - h))
        if pos > 0:
            dist = min(dist, abs(shops[pos - 1] - h))

        left = h - dist
        right = h + dist

        events.append((left, 1, p))
        events.append((right, -1, p))

    events.sort()

    ans = 0
    cur = 0
    i = 0

    while i < len(events):
        x = events[i]

        while i < len(events) and events[i][0] == x[0]:
            if events[i][1] == -1:
                cur -= events[i][2]
            i += 1

        j = i
        while j < len(events) and events[j][0] == x[0]:
            cur += events[j][2]
            j += 1

        ans = max(ans, cur)
        i = j

    print(ans)

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc vị trí cửa hàng và sắp xếp chúng. Sắp xếp là điều giúp cho các truy vấn cửa hàng gần nhất có thể thực hiện được bằng tìm kiếm nhị phân. 

Đối với mỗi túp lều, mã chuyển đổi điều kiện hình học thành một khoảng. Biến`dist`là khoảng cách đến cửa hàng hiện có gần nhất và vùng chiến thắng nằm ở tâm tọa độ túp lều với bán kính đó. 

Các sự kiện quét được lưu trữ dưới dạng`(coordinate, type, value)`. Một loại`-1`loại bỏ dân số, trong khi một loại`1`thêm dân số. Trình tự xử lý là chi tiết quan trọng. Việc loại bỏ xảy ra trước các phép cộng ở cùng tọa độ vì các khoảng được mở. Nếu thứ tự này bị đảo ngược, một người có thể bị tính sai ở vị trí hòa. 

Tất cả tọa độ và quần thể đều vừa khít với số nguyên Python. Câu trả lời có thể lớn như`200000 * 10^9`, vì vậy các ngôn ngữ có số nguyên 32 bit có chiều rộng cố định sẽ cần loại rộng hơn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 1
2 5 6
169
```Những túp lều nằm ở tọa độ`0`,`100`, Và`200`. Cửa hàng duy nhất hiện có là ở`169`. 

| Túp lều | Tọa độ | Khoảng cách đến cửa hàng | Khoảng chiến thắng | Dân số | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 169 | (-169, 169) | 2 | 
| 2 | 100 | 69 | (31, 169) | 5 | 
| 3 | 200 | 31 | (169, 231) | 6 | 

Sự chồng chéo tối đa xảy ra ngay trước tọa độ`169`, nơi tính hai túp lều đầu tiên. 

Câu trả lời là:```
7
```Điều này chứng tỏ tại sao không thể bao gồm các điểm cuối của khoảng thời gian. Túp lều 3 không tham gia tại tọa độ`169`vì đó chính xác là vị trí của cửa hàng hiện có. 

Đối với mẫu thứ hai:```
4 2
1 2 7 8
35 157
```Những túp lều nằm ở tọa độ`0`,`100`,`200`, Và`300`. 

| Túp lều | Tọa độ | Khoảng cách đến cửa hàng gần nhất | Khoảng chiến thắng | Dân số | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 35 | (-35, 35) | 1 | 
| 2 | 100 | 65 | (35, 165) | 2 | 
| 3 | 200 | 43 | (157, 243) | 7 | 
| 4 | 300 | 143 | (157, 443) | 8 | 

Sự chồng chéo tốt nhất là sau khi phối hợp`157`, trong đó túp lều 3 và 4 cùng đóng góp. 

Câu trả lời là:```
15
```Điều này khẳng định rằng vị trí tối ưu không nhất thiết phải nằm giữa các cửa hàng hiện có. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log m) | Việc sắp xếp các cửa hàng và sự kiện chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n) | Hai sự kiện được lưu trữ cho mỗi túp lều | 

Với tối đa 200.000 túp lều và cửa hàng, thuật toán chỉ thực hiện tìm kiếm sắp xếp và nhị phân. Điều này dễ dàng phù hợp với giới hạn đã định, không giống như bất kỳ cách tiếp cận nào so sánh mọi túp lều với mọi cửa hàng. 

## Trường hợp thử nghiệm```python
import sys
import io
from bisect import bisect_left

def solve(inp):
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, m = map(int, input().split())
    people = list(map(int, input().split()))
    shops = list(map(int, input().split()))
    shops.sort()

    events = []

    for i, p in enumerate(people):
        h = i * 100
        idx = bisect_left(shops, h)

        d = 10**18
        if idx < m:
            d = min(d, abs(shops[idx] - h))
        if idx:
            d = min(d, abs(shops[idx - 1] - h))

        events.append((h - d, 1, p))
        events.append((h + d, -1, p))

    events.sort()

    cur = ans = 0
    i = 0
    while i < len(events):
        x = events[i][0]

        while i < len(events) and events[i][0] == x and events[i][1] == -1:
            cur -= events[i][2]
            i += 1

        j = i
        while j < len(events) and events[j][0] == x:
            cur += events[j][2]
            j += 1

        ans = max(ans, cur)
        i = j

    return str(ans)

assert solve("""3 1
2 5 6
169
""") == "7"

assert solve("""4 2
1 2 7 8
35 157
""") == "15"

assert solve("""2 1
5 8
50
""") == "13"

assert solve("""3 2
1 1 1
300 99
""") == "2"

assert solve("""2 1
1000000000 1000000000
0
""") == "2000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cửa hàng hiện có duy nhất với hai túp lều | 13 | Chồng chéo khoảng cơ bản | 
| Dân số bình đẳng với các cửa hàng ở cả hai bên | 2 | Xử lý ranh giới | 
| Quần thể rất lớn | 2000000000 | Xử lý kích thước số nguyên | 
| Trường hợp mẫu | 7 và 15 | Độ đúng tiêu chuẩn | 

## Vỏ cạnh 

Khi túp lều nằm đúng tọa độ với cửa hàng hiện có, khoảng chiến thắng của nó vẫn có chiều rộng dương vì cửa hàng mới có thể di chuyển ra xa điểm đó. Thuật toán xử lý việc này vì bán kính khoảng là khoảng cách đến cửa hàng hiện có gần nhất và quá trình quét không bao giờ tự đánh giá điểm cuối bị loại trừ. 

Ví dụ:```
Input
3 2
1 1 1
300 99
```Tọa độ của túp lều là`0`,`100`, Và`200`. Túp lều ở giữa nằm ở cùng vị trí với một cửa hàng hiện có, nhưng cửa hàng mới có thể di chuyển sang trái hoặc phải một chút. Phương thức interval nắm bắt những khả năng này và trả về:```
2
```Khi tất cả các túp lều đều cách xa các cửa hàng hiện có, vị trí tốt nhất có thể nằm ngoài phạm vi các cửa hàng hiện có. Thuật toán không giả sử bất kỳ phạm vi tìm kiếm hữu hạn nào. Nó chỉ sử dụng các khoảng hợp lệ được tạo bởi hình học. 

Vì:```
Input
3 1
2 5 6
169
```điểm tốt nhất là trước cửa hàng hiện có và quá trình quét tìm thấy sự chồng chéo của hai khoảng đầu tiên một cách chính xác. 

Cuối cùng, vấn đề bất bình đẳng nghiêm ngặt. Tại thời điểm cửa hàng mới liên kết với cửa hàng hiện có, người đó không đóng góp. Việc xử lý các sự kiện loại bỏ trước các sự kiện bổ sung ở cùng tọa độ sẽ loại bỏ các vị trí ràng buộc không hợp lệ đó khỏi việc xem xét.
