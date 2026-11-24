---
problem: 926I
contest_id: 926
problem_index: I
name: "A Vital Problem"
contest_name: "VK Cup 2018 - Wild-card Round 1"
rating: 1700
tags: []
answer: passed_samples
verified: true
solve_time_s: 67
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
---

#CF 926I - Một vấn đề sống còn 

**Đánh giá:** 1700 
**Thẻ:** - 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có (1/1 mẫu) 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta có một ngày lặp lại vô thời hạn, được biểu diễn dưới dạng chu kỳ 24 giờ. Mỗi báo thức chiếm đúng một phút thời gian mỗi ngày và những khoảnh khắc báo thức này được cố định và lặp lại hàng ngày tại cùng một vị trí trên đồng hồ. Trong những phút đó Polycarp không thể ngủ được và ngay sau khi chuông báo thức vang lên, anh ấy thức cho đến khi phút đó kết thúc. 

Nhiệm vụ là tìm khoảng thời gian liên tục dài nhất mà trong đó không có chuông báo thức nào vang lên, có tính đến tính chất tuần hoàn của thời gian. Khoảng thời gian được phép kéo dài khoảng nửa đêm, nghĩa là nó có thể bắt đầu vào gần cuối ngày và tiếp tục đến đầu ngày hôm sau. 

Một cách hữu ích để suy nghĩ về vấn đề này là đặt tất cả thời gian báo thức vào một vòng tròn 1440 phút. Mỗi báo thức chặn một điểm trên vòng tròn này và chúng tôi muốn có khoảng cách lớn nhất giữa các điểm bị chặn liên tiếp khi di chuyển theo chiều kim đồng hồ. 

Ràng buộc n ≤ 100 có nghĩa là chúng ta có thể chấp nhận được các giải pháp O(n²) hoặc thậm chí O(n log n) một cách thoải mái. Việc sắp xếp thống trị mọi thứ tối ưu ở đây và thậm chí cả việc sử dụng vũ lực đối với tất cả các cặp sau khi sắp xếp vẫn là chuyện nhỏ. 

Vỏ ngoài tinh tế xuất phát từ tính chất tuần hoàn của thời đại. Nếu chúng ta chỉ xem xét sự khác biệt giữa các cảnh báo được sắp xếp liên tiếp mà không xử lý tình trạng bao trùm, thì chúng ta sẽ bỏ lỡ khoảng cách giữa cảnh báo cuối cùng trong ngày và cảnh báo đầu tiên của ngày hôm sau. Ví dụ: nếu chỉ có một cảnh báo lúc 05:43, phép tính chênh lệch đơn giản có thể trả về 0 không chính xác thay vì nhận ra khoảng cách đầy đủ 23:59. 

Một trường hợp khác xuất hiện khi các báo động ở rất gần nhau. Ví dụ: nếu báo thức lúc 00:00 và 00:01 thì câu trả lời đúng là 00:58 hoặc 23:58 tùy thuộc vào cách giải thích số phút bao gồm và việc xử lý bất cẩn từng “khoảng thời gian đổ chuông một phút” có thể làm thay đổi kết quả một phút. 

## Phương pháp tiếp cận 

Ý tưởng brute-force bắt đầu bằng cách chuyển đổi tất cả thời gian báo thức thành phút kể từ nửa đêm và sau đó, đối với mọi cặp báo thức có thể, tính toán khoảng cách chuyển tiếp giữa chúng xung quanh vòng tròn. Đối với mỗi cặp (i, j), chúng tôi đo thời gian từ báo thức i đến báo thức j tiến về phía trước, bao quanh ranh giới ngày khi cần thiết và lấy mức tối đa. 

Điều này có hiệu quả vì mọi khoảng thời gian ngủ tối đa đều phải bắt đầu ngay sau khi một số báo thức kết thúc và kết thúc ngay khi một báo thức khác bắt đầu, do đó điểm cuối của nó trùng với ranh giới báo động. Với n cảnh báo, có các cặp ứng cử viên O(n²) và tính toán mỗi khoảng cách là O(1), dẫn đến tổng công việc là O(n²), vẫn ổn ở mức n ≤ 100. 

Sự cải thiện đến từ việc nhận thấy rằng chúng ta không cần tất cả các cặp. Sau khi thời gian được sắp xếp, khoảng cách tốt nhất phải xuất hiện giữa các cảnh báo liên tiếp theo thứ tự tuần hoàn. Nếu có một khoảng cách lớn hơn trải rộng trên nhiều cảnh báo trung gian thì các cảnh báo trung gian đó sẽ nằm trong khoảng đó và mâu thuẫn với mức tối đa. Điều này làm giảm vấn đề quét các khác biệt liền kề trong một mảng tròn được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu (sắp xếp + khoảng trống liền kề) | O(n log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi từng thời gian báo thức từ "hh:mm" thành tổng số phút tính từ 00:00. Điều này đưa ra một biểu diễn tuyến tính của ngày tròn. 
2. Sắp xếp tất cả thời gian được chuyển đổi theo thứ tự tăng dần sao cho các phần tử liên tiếp thể hiện các sự kiện báo động liên tiếp trên đồng hồ. 
3. Tính toán khoảng cách giữa mỗi cặp liền kề trong danh sách đã sắp xếp. Đối với hai cảnh báo liên tiếp a[i] và a[i+1], thời gian rảnh giữa chúng là a[i+1] - a[i] - 1, vì cả hai điểm cuối đều bị cảnh báo chiếm giữ. 
4. Đồng thời tính toán khoảng cách bao quanh từ lần báo thức cuối cùng đến lần báo thức đầu tiên trong suốt nửa đêm. Đây là (1440 - a[cuối]) + a[0] - 1. 
5. Lấy mức tối đa trong số tất cả các khoảng trống được tính toán. 
6. Chuyển số phút rảnh tối đa này về định dạng "hh:mm".

Việc trừ 1 trong mỗi khoảng trống là cần thiết vì mỗi báo thức chiếm trọn khoảng thời gian một phút. Nếu báo thức xảy ra vào thời điểm t thì phút [t, t+1) bị chặn, do đó chế độ ngủ chỉ có thể bắt đầu lúc t+1 và tương tự phải kết thúc trước khi phút báo thức tiếp theo bắt đầu. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, mọi khoảng thời gian ngủ tối đa phải nằm giữa hai thời điểm bắt đầu báo thức liên tiếp theo thứ tự vòng tròn. Nếu một khoảng thời gian kéo dài qua một báo thức trung gian thì số phút chiếm dụng của báo thức đó sẽ phá vỡ tính liên tục của giấc ngủ. Do đó, mọi khoảng thời gian tối đa hợp lệ sẽ tương ứng chính xác với một trong các khoảng trống giữa các lần báo động liên tiếp trên vòng tròn. Việc kiểm tra tất cả các khoảng trống như vậy, bao gồm cả ranh giới bao quanh, sẽ làm cạn kiệt tất cả các ứng viên mà không bỏ sót hoặc trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def to_minutes(s):
    h, m = map(int, s.split(":"))
    return h * 60 + m

n = int(input())
a = [to_minutes(input().strip()) for _ in range(n)]
a.sort()

best = 0

for i in range(n - 1):
    gap = a[i + 1] - a[i] - 1
    best = max(best, gap)

wrap_gap = (1440 - a[-1]) + a[0] - 1
best = max(best, wrap_gap)

h = best // 60
m = best % 60
print(f"{h:02d}:{m:02d}")
```Bước chuyển đổi đảm bảo tất cả các so sánh được thực hiện trên thang đo tuyến tính thống nhất. Sắp xếp các vị trí báo động theo thứ tự thời gian sao cho những khác biệt liền kề có ý nghĩa. Vòng lặp trên các cặp liền kề nắm bắt tất cả các khoảng trống bên trong, trong khi tính toán bao quanh xử lý tính chất vòng tròn của thời gian. Việc trừ đi một phút là cần thiết vì cả hai điểm cuối của khoảng cách đều tương ứng với số phút báo thức, không thể sử dụng được cho giấc ngủ. 

Bước định dạng cuối cùng đảm bảo số phút nằm trong [0, 59], phù hợp với định dạng đầu ra được yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
05:43
```Số phút được sắp xếp: 

| Bước | Giờ báo thức | Khoảng cách được tính toán | Tốt nhất | 
| --- | --- | --- | --- | 
| ban đầu | [343] | chỉ bọc | 0 | 
| bọc | (1440 - 343 - 1) | 1096 | 1096 | 

Khoảng thời gian duy nhất là khoảng thời gian bao bọc, tương ứng với việc ngủ từ 05:44 đến 05:42 ngày hôm sau, tổng cộng là 23 giờ 59 phút. Điều này xác nhận rằng một cảnh báo duy nhất luôn mang lại kết quả cả ngày trừ đi một phút. 

### Ví dụ 2 

đầu vào:```
3
00:00
12:00
23:59
```Số phút được sắp xếp: [0, 720, 1439] 

| tôi | một [tôi] | a[i+1] | khoảng cách | 
| --- | --- | --- | --- | 
| 0 | 0 | 720 | 719 | 
| 1 | 720 | 1439 | 718 | 
| bọc | 1439 | 0 | 0 | 

Khoảng cách tối đa là 719, tương ứng với giấc ngủ từ 00:01 đến 11:59 hoặc đối xứng qua các ranh giới tùy theo cách diễn giải. Bảng này cho thấy các khoảng trống bên trong chi phối kết quả như thế nào, trong khi phần bao bọc ở đây là không đáng kể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; quét tuyến tính đơn sau đó | 
| Không gian | O(1) thêm | Chỉ có một số biến cố định ngoài bộ nhớ đầu vào | 

Ràng buộc n 100 làm cho việc sắp xếp không đáng kể và thậm chí một cách tiếp cận kém tối ưu hơn cũng có thể vượt qua một cách thoải mái. Giải pháp tốt trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import floor

    input = _sys.stdin.readline

    def to_minutes(s):
        h, m = map(int, s.split(":"))
        return h * 60 + m

    n = int(input())
    a = [to_minutes(input().strip()) for _ in range(n)]
    a.sort()

    best = 0
    for i in range(n - 1):
        best = max(best, a[i + 1] - a[i] - 1)

    best = max(best, (1440 - a[-1]) + a[0] - 1)

    return f"{best//60:02d}:{best%60:02d}"

assert run("1\n05:43\n") == "23:59", "sample 1"

assert run("2\n00:00\n00:01\n") == "23:58", "adjacent early alarms"
assert run("2\n00:00\n23:59\n") == "23:58", "wrap-heavy boundary"
assert run("3\n00:00\n12:00\n23:00\n") == "11:59", "spread across day"
assert run("4\n01:00\n02:00\n03:00\n04:00\n") == "20:59", "uniform spacing"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| báo động đơn | 23:59 | hộp đựng hình tròn đầy đủ | 
| 00:00, 00:01 | 23:58 | cạnh khoảng cách tối thiểu | 
| 00:00, 23:59 | 23:58 | độ kề cận ranh giới đúng đắn | 
| cách đều nhau | 20:59 | lựa chọn khoảng cách tối đa nội bộ | 

## Vỏ cạnh 

Một cảnh báo duy nhất làm nổi bật cách giải thích vòng tròn. Với đầu vào`05:43`, việc sắp xếp tạo ra một phần tử và thuật toán bỏ qua các khoảng trống bên trong. Tính toán gói cho (1440 - 343 - 1) = 1096 phút, tương ứng chính xác với 23:59. Thuật toán xử lý chính xác ngày liên tục vào khoảng nửa đêm. 

Hai báo động liền kề lúc`00:00`Và`00:01`kiểm tra logic từng cái một. Sau khi chuyển đổi, khoảng cách bên trong là 0 - 1 = -1, được giới hạn ở mức tối đa là 0. Khoảng cách bao bọc trở thành (1440 - 1) + 0 - 1 = 1438 phút, tức là 23:58. Điều này xác nhận rằng việc loại trừ cả hai phút cảnh báo được xử lý chính xác và tính toán gói vẫn nhất quán với các khoảng trống tuyến tính.
