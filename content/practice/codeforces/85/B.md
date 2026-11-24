---
title: "CF 85B - Xếp hàng Đại sứ quán"
description: "Mỗi người đến thăm đại sứ quán phải trải qua ba giai đoạn liên tiếp. Giai đoạn đầu tiên có k1 cửa sổ giống nhau, mỗi dịch vụ mất thời gian t1. Giai đoạn thứ hai có cửa sổ k2 với thời gian phục vụ t2 và giai đoạn thứ ba có cửa sổ k3 với thời gian phục vụ t3."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 85
codeforces_index: "B"
codeforces_contest_name: "Yandex.Algorithm 2011: Round 1"
rating: 1800
weight: 85
solve_time_s: 121
verified: true
draft: false
---

[CF 85B - Xếp hàng Đại sứ quán](https://codeforces.com/problemset/problem/85/B) 

**Đánh giá:** 1800 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi người đến thăm đại sứ quán phải trải qua ba giai đoạn liên tiếp. Giai đoạn đầu tiên có`k1`cửa sổ giống hệt nhau, mỗi dịch vụ sử dụng`t1`thời gian. Giai đoạn thứ hai có`k2`cửa sổ với thời gian phục vụ`t2`, và giai đoạn thứ ba có`k3`cửa sổ với thời gian phục vụ`t3`. 

Người`i`đến đúng lúc`ci`. Từ lúc đó cho đến khi kết thúc chặng thứ ba, họ vẫn ở trong đại sứ quán. Chúng tôi được phép quyết định chính xác thời điểm mỗi người bước vào từng giai đoạn, miễn là thứ tự giai đoạn được giữ nguyên và không có cửa sổ nào phục vụ nhiều người cùng một lúc. 

Mục tiêu không phải là giảm thiểu tổng thời gian chờ đợi hoặc thời gian chờ đợi trung bình. Chúng tôi muốn giảm thiểu thời gian lưu trú tối đa của tất cả mọi người. 

Nếu một người đến vào lúc`c`và kết thúc vào thời điểm`f`, thì tổng thời gian họ ở bên trong là`f - c`. Chúng ta phải tổ chức tất cả các hàng đợi sao cho giá trị lớn nhất càng nhỏ càng tốt. 

Các ràng buộc ngay lập tức loại trừ mô phỏng ở mức độ chi tiết của đơn vị thời gian. Thời gian đến lên tới`10^9`và số lượng dịch vụ cũng có thể trở nên rất lớn vì có thể có`10^5`mọi người. Bất kỳ thuật toán nào tỷ lệ thuận với thời gian đều không thể thực hiện được. 

Số người nhiều nhất là`10^5`, điều này gợi ý rõ ràng điều gì đó xung quanh`O(n log n)`hoặc có thể`O(n log^2 n)`được mong đợi. Vì bản thân các cửa sổ có thể nhiều như`10^9`, chúng tôi cũng không thể biểu diễn rõ ràng từng cửa sổ trừ khi thuật toán chỉ lưu trữ các cửa sổ thực sự được sử dụng. 

Điều khó khăn là việc giảm thiểu thời gian hoàn thành tối đa không giống như việc tham lam thúc đẩy mọi người vượt qua càng sớm càng tốt ở mọi giai đoạn. Việc triển khai ngây thơ có thể vô tình tạo ra tắc nghẽn ở hạ lưu. 

Hãy xem xét ví dụ này:```
k1 = 2, k2 = 1, k3 = 1
t1 = 1, t2 = 100, t3 = 1
arrivals = [0, 0]
```Nếu cả hai người ngay lập tức bước vào giai đoạn 1, họ sẽ hoàn thành giai đoạn 1 vào thời điểm 1. Sau đó, một người sẽ đợi gần như toàn bộ thời gian.`100`khối thời gian ở giai đoạn 2. Nút thắt thực sự là giai đoạn 2, không phải giai đoạn 1. 

Một trường hợp tế nhị khác xuất hiện khi có nhiều người đến cùng một lúc.```
1 1 1
1 1 1
5
1 1 1 1 1
```Câu trả lời đúng là`7`, không`3`. Mặc dù bản thân dịch vụ chỉ mất tổng cộng ba đơn vị, nhưng người thứ năm vẫn đợi bốn đơn vị trước khi bắt đầu. 

Một sai lầm phổ biến là giảm thiểu thời gian hoàn thành của mỗi người một cách độc lập. Điều đó không nhất thiết phải giảm thiểu thời gian lưu trú tối đa đối với tất cả mọi người. Chiến lược tối ưu phải cân bằng đường ống trên toàn cầu. 

## Phương pháp tiếp cận 

Quan điểm vũ phu là đơn giản. Giả sử chúng ta đoán một số câu trả lời của ứng viên`T`, nghĩa là không ai được phép ở lại quá`T`đơn vị thời gian bên trong đại sứ quán. 

Đối với mỗi người đến`ci`, họ phải hoàn thành cả ba giai đoạn trước thời hạn`ci + T`. 

Chúng ta có thể cố gắng tìm kiếm một cách rõ ràng các lịch trình phân công từng người vào các khoảng thời gian và thời gian cụ thể. Việc tìm kiếm toàn bộ không gian trạng thái sẽ bùng nổ ngay lập tức. Ngay cả khi chúng ta chỉ theo dõi các bài tập trên sân khấu một cách tham lam, mỗi người có thể chọn trong số nhiều cửa sổ vào nhiều thời điểm có thể. Số lượng các khả năng sẽ trở thành cấp số nhân. 

Một mô phỏng lực lượng vũ phu có cấu trúc chặt chẽ hơn sẽ xử lý mọi người theo thứ tự và luôn chỉ định họ vào thời điểm sớm nhất có sẵn ở mỗi giai đoạn. Điều này có tác dụng để tính toán một lịch trình khả thi chứ không phải để tối ưu hóa thời gian lưu trú tối đa. Chúng ta vẫn cần phải tìm kiếm trên nhiều lịch trình có thể hoặc nhiều giá trị có thể có của`T`. 

Quan sát quan trọng là tính khả thi của một cơ chế cố định`T`có cấu trúc đơn điệu. 

Nếu chúng ta có thể sắp xếp thời gian cho mọi người để không ai chi tiêu nhiều hơn`T`thời gian bên trong, thì bất kỳ giá trị lớn hơn nào cũng có tác dụng. Điều đó ngay lập tức gợi ý tìm kiếm nhị phân cho câu trả lời. 

Bây giờ vấn đề trở thành: 

Chúng ta có thể kiểm tra tính khả thi của một giải pháp cố định`T`một cách hiệu quả? 

Đối với một người đến`ci`, toàn bộ quá trình ba giai đoạn phải nằm trong khoảng`[ci, ci + T]`. 

Thay vì lập kế hoạch chuyển tiếp từ thời điểm đến, việc lập kế hoạch lùi thời hạn sẽ dễ dàng hơn nhiều. 

Giả sử một người bắt đầu giai đoạn 3 vào thời điểm`x`. Sau đó:```
x + t3 <= ci + T
```Vì thế:```
x <= ci + T - t3
```Tương tự, nếu giai đoạn 2 bắt đầu lúc`y`:```
y + t2 <= x
```và nếu giai đoạn 1 bắt đầu lúc`z`:```
z + t1 <= y
```Điều này tạo ra một chuỗi thời gian bắt đầu muộn nhất có thể. 

Chiến lược khả thi tối ưu là lập kế hoạch lùi tham lam. Chúng tôi xử lý mọi người từ người đến muộn nhất đến người đến sớm nhất và luôn đặt từng giai đoạn càng muộn càng tốt trên một số cửa sổ có sẵn. 

Tại sao điều này lại giúp ích? Bởi vì việc lập kế hoạch muộn hơn sẽ đảm bảo các khoảng thời gian sớm hơn cho những người có thời hạn chặt chẽ hơn. 

Đối với mỗi giai đoạn, mỗi cửa sổ hoạt động giống như một cỗ máy có các khoảng thời gian được sử dụng là các khối có chiều dài cố định. Nếu chúng ta lập lịch lùi, mỗi cửa sổ chỉ duy trì điểm kết thúc chưa sử dụng mới nhất. 

Điều này biến vấn đề thành một cuộc kiểm tra tham lam rõ ràng với các hàng đợi ưu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Theo cấp số nhân hoặc tệ hơn | Rất lớn | Quá chậm | 
| Tối ưu | O(n log n log Trả lời) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tìm kiếm nhị phân câu trả lời khả thi tối thiểu`T`. 

Nếu một số`T`hoạt động, mọi giá trị lớn hơn cũng hoạt động, vì vậy tính khả thi là đơn điệu. 
2. Thực hiện kiểm tra tính khả thi của một giải pháp cố định`T`. 

Chúng tôi xử lý mọi người theo thứ tự ngược lại, từ thời gian đến lớn nhất đến nhỏ nhất. 
3. Duy trì thời gian sẵn có cho từng giai đoạn. 

Đối với giai đoạn 3, mọi cửa sổ ban đầu đều miễn phí mãi mãi trong tương lai. Chúng tôi biểu thị từng cửa sổ theo thời điểm muộn nhất mà khối dịch vụ mới có thể kết thúc. 
4. Đối với người có thời gian đến`ci`, tính toán các ràng buộc hoàn thiện mới nhất có thể. 

Giai đoạn 3 phải bắt đầu không muộn hơn:```
ci + T - t3
```5. Trong số tất cả các cửa sổ của giai đoạn 3, hãy chọn cửa sổ có thời gian khả dụng hiện tại lớn nhất nhưng vẫn khả thi. 

Nếu một cửa sổ có sẵn cho đến thời điểm`a`, thì chúng ta có thể đặt giai đoạn 3 kết thúc tại`min(a, ci + T)`. 

Sự khởi đầu trở thành:```
end - t3
```Chúng tôi tham lam đặt dịch vụ càng muộn càng tốt. 
6. Sau khi sửa chữa giai đoạn 3, hãy tìm ra vị trí mới nhất có thể cho giai đoạn 2. 

Giai đoạn 2 phải kết thúc không muộn hơn thời gian bắt đầu của giai đoạn 3. 
7. Lặp lại vị trí tham lam tương tự cho giai đoạn 2 và sau đó cho giai đoạn 1. 
8. Nếu ở bất kỳ giai đoạn nào không có cửa sổ nào có thể chứa khối được yêu cầu thì`T`là không thể thực hiện được. 
9. Nếu tất cả mọi người đều được lên lịch thành công thì`T`là khả thi. 
10. Tìm kiếm nhị phân tiếp tục cho đến khi khả thi tối thiểu`T`được tìm thấy. 

### Tại sao nó hoạt động 

Bất biến tham lam là sau khi lên lịch cho một số hậu tố người, mọi vị trí chưa sử dụng còn lại sẽ càng sớm càng tốt. Bằng cách luôn đặt mỗi khối dịch vụ mới càng muộn càng tốt, chúng tôi tránh lãng phí năng lực ban đầu mà mọi người trong tương lai có thể yêu cầu. 

Việc xử lý những người đến theo thứ tự giảm dần là rất quan trọng vì những người đến sau có thời hạn hiệu lực chặt chẽ hơn. Nếu chúng ta lên lịch cho những người đến sớm hơn trước, họ có thể chiếm những chỗ trống cần thiết cho những người đến sau. 

Chiến lược tham lam lạc hậu là tối ưu bằng lập luận trao đổi. Bất kỳ lịch trình khả thi nào cũng có thể được chuyển đổi thành lịch trình trong đó mỗi khối được chỉ định được đẩy càng muộn càng tốt mà không ảnh hưởng đến tính khả thi của những người còn lại. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def possible(T, k, t, arrivals):
    n = len(arrivals)

    # For each stage we store available ending times.
    # Negative values simulate max-heaps.
    heaps = []

    INF = 10**30

    for cnt in k:
        h = [-INF] * cnt
        heapq.heapify(h)
        heaps.append(h)

    for c in reversed(arrivals):
        latest = c + T

        for stage in range(2, -1, -1):
            duration = t[stage]

            avail = -heapq.heappop(heaps[stage])

            end_time = min(avail, latest)

            start_time = end_time - duration

            if start_time < c:
                return False

            heapq.heappush(heaps[stage], -start_time)

            latest = start_time

    return True

def solve():
    k = list(map(int, input().split()))
    t = list(map(int, input().split()))

    n = int(input())
    arrivals = list(map(int, input().split()))

    lo = sum(t)
    hi = 10**18

    while lo < hi:
        mid = (lo + hi) // 2

        if possible(mid, k, t, arrivals):
            hi = mid
        else:
            lo = mid + 1

    print(lo)

solve()
```Tìm kiếm nhị phân bắt đầu từ`sum(t)`bởi vì ngay cả khi không phải chờ đợi, mỗi người ít nhất phải dành tổng thời gian phục vụ bên trong đại sứ quán. 

Chức năng khả thi là cốt lõi của giải pháp. Mỗi heap đại diện cho tất cả các cửa sổ của một giai đoạn. Thay vì lưu trữ khi một cửa sổ trở nên trống trong tương lai, chúng tôi lưu trữ thời gian chưa sử dụng mới nhất trước thời điểm đó mà chúng tôi vẫn có thể đặt một khối dịch vụ khác trong khi lập lịch lùi. 

Giả sử một giai đoạn hiện có thời gian sẵn sàng`A`. Điều đó có nghĩa là khối dịch vụ được gán tiếp theo cho cửa sổ này phải kết thúc không muộn hơn`A`. Nếu chúng ta gán một khối có độ dài`d`, điểm bắt đầu của nó trở thành`A - d`và các nhiệm vụ trong tương lai trên cửa sổ này phải hoàn thành trước khi bắt đầu. 

Chi tiết triển khai tinh tế đang được cập nhật`latest`sau mỗi giai đoạn. Khi giai đoạn 3 bắt đầu vào một thời điểm nào đó`x`, giai đoạn 2 phải kết thúc trước`x`, do đó thời gian kết thúc muộn nhất của nó trở thành chính xác`x`. 

Một sai lầm dễ mắc phải khác là quên rằng khoảng thời gian phục vụ phải nằm hoàn toàn sau thời gian người đó đến. Việc kiểm tra:```
if start_time < c:
```phát hiện lịch trình không thể. 

Tất cả số học đều sử dụng số nguyên Python, xử lý các giá trị lớn một cách tự nhiên một cách an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1 1
1 1 1
5
1 1 1 1 1
```Chúng tôi kiểm tra`T = 7`. 

| Người | Bắt đầu Giai đoạn 3 | Bắt đầu Giai đoạn 2 | Bắt đầu Giai đoạn 1 | 
| --- | --- | --- | --- | 
| 5 | 7 | 6 | 5 | 
| 4 | 6 | 5 | 4 | 
| 3 | 5 | 4 | 3 | 
| 2 | 4 | 3 | 2 | 
| 1 | 3 | 2 | 1 | 

Người đầu tiên đến đúng lúc`1`và kết thúc tại`4`, chi tiêu`3`đơn vị. Người thứ năm về đích lúc`8`, chi tiêu`7`đơn vị. 

Đang cố gắng`T = 6`không thành công vì việc sắp xếp khả thi sớm nhất cho người thứ năm sẽ yêu cầu giai đoạn 1 phải bắt đầu trước thời gian đến`1`. 

Dấu vết này chứng tỏ tại sao việc đến đồng thời lại tạo ra sự tích tụ hàng đợi ngay cả khi thời lượng phục vụ nhỏ. 

### Ví dụ 2 

đầu vào:```
2 1 1
5 1 1
5
1 2 3 4 5
```Giả sử chúng ta kiểm tra`T = 13`. 

| Người | Bắt đầu Giai đoạn 3 | Bắt đầu Giai đoạn 2 | Bắt đầu Giai đoạn 1 | 
| --- | --- | --- | --- | 
| 5 | 17 | 16 | 11 | 
| 4 | 13 | 12 | 7 | 
| 3 | 12 | 11 | 6 | 
| 2 | 8 | 7 | 2 | 
| 1 | 7 | 6 | 1 | 

Hai cửa sổ ở giai đoạn đầu tiên cho phép chồng chéo ở đó, nhưng các giai đoạn sau sẽ tuần tự hóa luồng. 

Dấu vết này thể hiện hành vi của đường ống chính. Ngay cả khi giai đoạn đầu có nhiều cửa sổ, nút cổ chai sau này vẫn chiếm ưu thế trong câu trả lời tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log(k1 + k2 + k3) log Trả lời) | Tìm kiếm nhị phân với các phép toán heap | 
| Không gian | O(k1 + k2 + k3) | Heap cho tất cả các cửa sổ | 

Mỗi lần kiểm tra tính khả thi sẽ xử lý mỗi người chính xác một lần và thực hiện ba thao tác heap. Tìm kiếm nhị phân trên số nguyên 64 bit yêu cầu khoảng 60 lần lặp trong trường hợp xấu nhất, đủ nhanh để`10^5`mọi người. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def possible(T, k, t, arrivals):
        heaps = []
        INF = 10**30

        for cnt in k:
            h = [-INF] * cnt
            heapq.heapify(h)
            heaps.append(h)

        for c in reversed(arrivals):
            latest = c + T

            for stage in range(2, -1, -1):
                duration = t[stage]

                avail = -heapq.heappop(heaps[stage])

                end_time = min(avail, latest)

                start_time = end_time - duration

                if start_time < c:
                    return False

                heapq.heappush(heaps[stage], -start_time)

                latest = start_time

        return True

    k = list(map(int, input().split()))
    t = list(map(int, input().split()))
    n = int(input())
    arrivals = list(map(int, input().split()))

    lo = sum(t)
    hi = 10**18

    while lo < hi:
        mid = (lo + hi) // 2

        if possible(mid, k, t, arrivals):
            hi = mid
        else:
            lo = mid + 1

    return str(lo)

# provided sample
assert run(
"""1 1 1
1 1 1
5
1 1 1 1 1
"""
) == "7"

# minimum size
assert run(
"""1 1 1
5 5 5
1
10
"""
) == "15"

# many simultaneous arrivals
assert run(
"""1 1 1
2 2 2
3
1 1 1
"""
) == "10"

# wide first stage, narrow later stages
assert run(
"""100 1 1
1 10 10
2
0 0
"""
) == "31"

# staggered arrivals
assert run(
"""1 1 1
3 3 3
3
1 10 20
"""
) == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Người độc thân | 15 | Câu trả lời tối thiểu có thể bằng tổng thời gian phục vụ | 
| Tất cả lượt đến đều bằng nhau | 10 | Xây dựng hàng đợi qua tất cả các giai đoạn | 
| Công suất giai đoạn đầu rất lớn | 31 | Nút thắt sau này chiếm ưu thế | 
| Khoảng cách đến lớn | 9 | Không phải chờ đợi không cần thiết | 

## Vỏ cạnh 

Hãy xem xét trường hợp tất cả mọi người đến cùng một lúc.```
1 1 1
1 1 1
5
1 1 1 1 1
```Thuật toán lập lịch lùi. Người cuối cùng chiếm những khoảng thời gian muộn nhất có thể, sau đó người thứ tư chiếm những khoảng thời gian muộn nhất tiếp theo, v.v. Mỗi nhiệm vụ duy trì sự linh hoạt tối đa trong tương lai. Câu trả lời kết quả là`7`, phù hợp với chiều dài đường ống thực sự cộng với việc chờ đợi. 

Bây giờ hãy xem xét một nút thắt nghiêm trọng ở giai đoạn giữa.```
2 1 2
1 100 1
2
0 0
```Giai đoạn 1 có đủ năng lực phục vụ cả hai ngay lập tức, nhưng giai đoạn 2 chỉ xử lý một người mỗi lần.`100`đơn vị. Bộ lập lịch lùi sẽ tự động giãn cách các nhiệm vụ ở giai đoạn hai vì thời gian khả dụng của mỗi cửa sổ sẽ lùi lại sau khi phân công. Câu trả lời chính xác sẽ bị chi phối bởi giai đoạn giữa. 

Một trường hợp phức tạp khác là khi những người đến đã bị phân tán rộng rãi.```
1 1 1
2 2 2
3
1 100 200
```Ở đây không cần chờ đợi gì cả. Mỗi người có thể ngay lập tức vượt qua mọi giai đoạn. Thuật toán vẫn hoạt động vì mọi nhiệm vụ lùi lại chỉ sử dụng vị trí được phép mới nhất, điều này tự nhiên trùng khớp với dịch vụ ngay lập tức. Câu trả lời trở nên chính xác`6`, tổng thời gian phục vụ.
