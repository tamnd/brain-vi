---
title: "CF 4B - Trước kỳ thi"
description: "Chúng ta có d ngày và tổng số giờ học mục tiêu, sumTime. Mỗi ngày, Peter phải học ít nhất minTime[i] giờ và nhiều nhất là maxTime[i] giờ. Nhiệm vụ là xây dựng bất kỳ lịch trình hợp lệ nào có tổng chính xác là sumTime."
date: "2026-05-27T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 4
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 4 (Div. 2 Only)"
rating: 1200
weight: 4
solve_time_s: 72
verified: true
draft: false
---
[CF 4B - Trước kỳ thi](https://codeforces.com/problemset/problem/4/B) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao`d`ngày và tổng số giờ học mục tiêu,`sumTime`. Mỗi ngày, Peter phải học ít nhất`minTime[i]`giờ và nhiều nhất`maxTime[i]`giờ. Nhiệm vụ là xây dựng bất kỳ lịch trình hợp lệ nào có tổng số chính xác`sumTime`. 

Đây không phải là một vấn đề tối ưu hóa. Chúng ta không cần lịch trình tối thiểu hay tối đa, chỉ cần bất kỳ lịch trình nào thỏa mãn mọi ràng buộc. 

Điều đầu tiên cần chú ý là mỗi ngày đóng góp một khoảng giá trị có thể có. Trong tất cả các ngày, tổng số nhỏ nhất có thể đạt được là:```
sum(minTime[i])
```và tổng số lớn nhất có thể đạt được là:```
sum(maxTime[i])
```Nếu như`sumTime`nằm ngoài phạm vi này thì không có nghiệm nào tồn tại. 

Những hạn chế là rất nhỏ.`d`tối đa là 30 và mỗi giới hạn hàng ngày nhiều nhất là 8. Ngay cả các giải pháp theo cấp số nhân thoạt đầu cũng có thể trông hấp dẫn vì không gian tìm kiếm không lớn so với các vấn đề điển hình của Codeforces. Nhưng số lượng lịch trình có thể vẫn có thể bùng nổ. Nếu mỗi ngày có 9 lựa chọn từ 0 đến 8 thì tổng số bài tập sẽ là:```
9^30 ≈ 4.2 × 10^28
```mà hoàn toàn không thể dùng vũ lực được. 

Thay vào đó, những ràng buộc nhỏ gợi ý rằng giải pháp dự định mang tính xây dựng. Chúng ta phải trực tiếp xây dựng một lịch trình hợp lệ hơn là tìm kiếm một lịch trình. 

Trường hợp cạnh tinh tế xuất hiện khi tổng mục tiêu nhỏ hơn tổng của tất cả các giá trị tối thiểu. Ví dụ:```
2 3
2 5
2 5
```Tổng số tối thiểu có thể đạt được đã là 4 nên không có lịch trình nào phù hợp. Một giải pháp tham lam bất cẩn bắt đầu từ số 0 và tăng giá trị có thể vô tình tạo ra các giá trị hàng ngày không hợp lệ. 

Một trường hợp phức tạp khác xảy ra khi mục tiêu bằng tổng số tối thiểu một cách chính xác:```
3 6
1 4
2 5
3 7
```Ở đây lịch trình cần thiết duy nhất là:```
1 2 3
```Một số triển khai tiếp tục phân bổ thêm số giờ mặc dù không còn số giờ bổ sung, điều này có thể đẩy một ngày vượt quá mức tối đa. 

Ranh giới đối diện cũng có vấn đề. Giả định:```
2 10
3 5
4 5
```Tổng số tối đa có thể đạt được chính xác là 10, vì vậy lịch trình hợp lệ duy nhất là:```
5 5
```Nếu thuật toán phân phối số giờ không chính xác hoặc dừng sớm, thuật toán có thể không đạt được số tiền yêu cầu. 

Ngoài ra còn có trường hợp một ngày:```
1 6
5 7
```Câu trả lời đơn giản là`6`. Nếu không có:```
1 48
5 7
```mục tiêu nằm ngoài khoảng cho phép nên câu trả lời là`NO`. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ thử mọi lịch trình học tập có thể. Đối với mỗi ngày, chúng tôi chọn một số giá trị giữa`minTime[i]`Và`maxTime[i]`, sau đó kiểm tra xem tổng cuối cùng có bằng không`sumTime`. 

Điều này hoạt động hợp lý vì nó khám phá toàn bộ không gian tìm kiếm. Nếu một lịch trình hợp lệ tồn tại, cuối cùng vũ lực sẽ tìm thấy nó. 

Vấn đề là số lượng kết hợp. Trong trường hợp xấu nhất, mỗi ngày có 9 giá trị có thể, từ 0 đến 8. Với 30 ngày, giá trị đó trở thành:```
9^30
```lớn về mặt thiên văn. 

Điểm mấu chốt là vấn đề chỉ quan tâm đến tổng số tiền chứ không quan tâm đến bất kỳ sự sắp xếp đặc biệt nào giữa các ngày. Khi chúng ta biết mục tiêu nằm giữa tổng tối thiểu và tối đa có thể có trên toàn cầu, chúng ta luôn có thể xây dựng một lịch trình hợp lệ một cách tham lam. 

Bắt đầu với lịch trình hợp lệ tối thiểu:```
schedule[i] = minTime[i]
```Điều này đảm bảo mỗi ngày đã đáp ứng được giới hạn dưới của nó. 

Bây giờ hãy tính xem còn cần thêm bao nhiêu giờ nữa:```
remaining = sumTime - sum(minTime)
```Đối với mỗi ngày, chúng ta có thể tăng giá trị của nó một cách an toàn tối đa:```
maxTime[i] - minTime[i]
```Vì vậy, chúng tôi tham lam thêm càng nhiều giờ càng tốt cho ngày hiện tại mà không vượt quá mức tối đa hoặc vượt quá mục tiêu. 

Điều này hoạt động vì điều kiện chung duy nhất là tổng cuối cùng. Mọi dung lượng bổ sung chưa sử dụng từ một ngày này luôn có thể được chuyển sang ngày khác sau đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(9^d) | O(d) | Quá chậm | 
| Tối ưu | O(d) | O(d) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số ngày`d`và tổng mục tiêu`sumTime`. 

Chúng ta cần cả những ràng buộc hàng ngày và số tiền cuối cùng mong muốn trước khi xây dựng bất cứ thứ gì. 
2. Lưu trữ tất cả`(minTime, maxTime)`cặp và tính:```
min_total = sum(minTime[i])
max_total = sum(maxTime[i])
```Những giá trị này xác định toàn bộ phạm vi có thể đạt được của tổng số giờ học. 
3. Kiểm tra xem`sumTime`nằm trong phạm vi này. 

Nếu như:```
sumTime < min_total
```hoặc```
sumTime > max_total
```sau đó in`NO`và dừng lại. 

Không có lịch trình nào có thể thỏa mãn được các ràng buộc trong những trường hợp đó. 
4. Khởi tạo lịch trình bằng cách sử dụng tất cả các giá trị tối thiểu.```
schedule[i] = minTime[i]
```Điều này đã đưa ra một lịch trình hợp lệ với tổng số`min_total`. 
5. Tính xem còn cần thêm bao nhiêu giờ nữa.```
remaining = sumTime - min_total
```6. Đi qua từng ngày một. 

Đối với mỗi ngày, hãy tính xem vẫn có thể thêm bao nhiêu giờ:```
extra = maxTime[i] - minTime[i]
```Thêm vào:```
add = min(remaining, extra)
```đến ngày hiện tại. 

Điều này tham lam đẩy lịch trình về phía mục tiêu trong khi luôn duy trì hiệu lực. 
7. Giảm`remaining`bằng số tiền thêm vào. 

Sau cùng`remaining`trở thành số 0, nghĩa là tổng số tiền là chính xác. 
8. In`YES`và tiến độ đã xây dựng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

d, sumTime = map(int, input().split())

days = []
min_total = 0
max_total = 0

for _ in range(d):
    mn, mx = map(int, input().split())
    days.append((mn, mx))
    min_total += mn
    max_total += mx

if sumTime < min_total or sumTime > max_total:
    print("NO")
    sys.exit()

schedule = [mn for mn, mx in days]
remaining = sumTime - min_total

for i in range(d):
    mn, mx = days[i]

    extra_capacity = mx - mn
    add = min(remaining, extra_capacity)

    schedule[i] += add
    remaining -= add

print("YES")
print(*schedule)
```Phần đầu tiên đọc tất cả các khoảng và tính toán tổng số tối thiểu và tối đa có thể đạt được. Điều đó ngay lập tức cho chúng ta biết liệu một giải pháp có khả thi hay không. 

Lịch trình bắt đầu với tất cả các giá trị tối thiểu vì chúng luôn an toàn. Mỗi ngày đều đã đáp ứng được giới hạn dưới của nó nên chúng tôi chỉ cần phân bổ thêm số giờ. 

Sự phân phối tham lam hoạt động vì mỗi ngày có thêm năng lực độc lập. Thêm nhiều giờ hơn vào một ngày không bao giờ làm giảm giá trị của một ngày khác. 

Dòng:```
add = min(remaining, extra_capacity)
```là chi tiết quan trọng. Nó ngăn ngừa hai sai lầm phổ biến cùng một lúc. Chúng tôi không bao giờ vượt quá tổng mục tiêu và chúng tôi không bao giờ vượt quá giới hạn tối đa trong ngày. 

Thuật toán không cần quay lui. Khi một giá trị được gán, nó không bao giờ phải được xem xét lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 48
5 7
```Tổng số ban đầu: 

| Giá trị | Kết quả | 

|---|---|---| 

| tổng cộng tối thiểu | 5 | 

| max_total | 7 | 

| mục tiêu | 48 | 

Từ:```
48 > 7
```mục tiêu là không thể. 

Đầu ra:```
NO
```Ví dụ này xác nhận việc kiểm tra phạm vi là đủ để loại bỏ các trường hợp không thể ngay lập tức. 

### Ví dụ 2 

đầu vào:```
3 10
1 3
2 5
3 8
```Trạng thái ban đầu: 

| Ngày | phút | tối đa | 
| --- | --- | --- | 
| 1 | 1 | 3 | 
| 2 | 2 | 5 | 
| 3 | 3 | 8 | 

Chúng tôi tính toán:```
min_total = 1 + 2 + 3 = 6
max_total = 3 + 5 + 8 = 16
```Từ`10`nằm bên trong`[6, 16]`, có một giải pháp. 

Lịch trình ban đầu: 

| Ngày | Giá trị hiện tại | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 3 | 

Số giờ còn lại:```
10 - 6 = 4
```Phân phối tham lam: 

| Ngày | Công suất bổ sung | Đã thêm | Còn lại | Lịch trình | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 2 | [3, 2, 3] | 
| 2 | 3 | 2 | 0 | [3, 4, 3] | 
| 3 | 5 | 0 | 0 | [3, 4, 3] | 

Đầu ra cuối cùng:```
YES
3 4 3
```Dấu vết này cho thấy phương pháp tham lam dần dần đạt được mục tiêu mà không vi phạm bất kỳ giới hạn nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(d) | Một lượt để đọc đầu vào và một lượt để xây dựng lịch trình | 
| Không gian | O(d) | Lưu trữ các khoảng thời gian và lịch trình kết quả | 

Với tối đa 30 ngày, giải pháp này dễ dàng đủ nhanh. Thuật toán chỉ thực hiện một vài phép tính số học đơn giản mỗi ngày và mức sử dụng bộ nhớ rất nhỏ. 

## Trường hợp thử nghiệm 

### Trường hợp thử nghiệm 1 

đầu vào:```
1 0
0 0
```Sản lượng dự kiến:```
YES
0
```Điều này kiểm tra đầu vào hợp lệ nhỏ nhất có thể. 

### Trường hợp thử nghiệm 2 

đầu vào:```
2 3
2 5
2 5
```Sản lượng dự kiến:```
NO
```Tổng số tối thiểu có thể đạt được đã là 4, vì vậy mục tiêu không thể đạt được. 

### Trường hợp thử nghiệm 3 

đầu vào:```
4 12
3 3
3 3
3 3
3 3
```Sản lượng dự kiến:```
YES
3 3 3 3
```Mọi giá trị đều cố định nên thuật toán phải nhận ra lịch trình duy nhất có thể. 

### Trường hợp thử nghiệm 4 

đầu vào:```
5 20
0 8
0 8
0 8
0 8
0 8
```Sản lượng dự kiến:```
YES
8 8 4 0 0
```Mọi phân phối hợp lệ đều hoạt động. Trường hợp này kiểm tra xem phân bổ tham lam có xử lý chính xác các giá trị lớn còn lại hay không. 

## Vỏ cạnh 

Hãy xem xét trường hợp giới hạn dưới không thể:```
2 3
2 5
2 5
```Thuật toán tính toán:```
min_total = 4
max_total = 10
```Kể từ khi mục tiêu`3`nhỏ hơn`4`, nó in ngay:```
NO
```Thậm chí không có kế hoạch xây dựng nào bắt đầu, điều này tránh được những điều chỉnh tiêu cực không hợp lệ sau này. 

Bây giờ hãy xem xét ranh giới tối thiểu chính xác:```
3 6
1 4
2 5
3 7
```Thuật toán bắt đầu bằng:```
schedule = [1, 2, 3]
remaining = 0
```Vì không cần thêm giờ nên vòng lặp tham lam không thêm gì cả. Câu trả lời cuối cùng vẫn là:```
1 2 3
```Điều này xác nhận thuật toán không vô tình sửa đổi lịch trình đã chính xác. 

Đối với ranh giới tối đa chính xác:```
2 10
3 5
4 5
```Chúng tôi nhận được:```
min_total = 7
remaining = 3
```Ngày đầu tiên có thể chấp nhận làm thêm 2 giờ, tạo ra:```
5 4
remaining = 1
```Ngày thứ hai chấp nhận thêm giờ cuối cùng:```
5 5
remaining = 0
```Tiến độ xây dựng đạt tổng tối đa một cách chính xác. 

Cuối cùng, trường hợp một ngày:```
1 6
5 7
```Lịch trình ban đầu là`[5]`với:```
remaining = 1
```Ngày duy nhất có thêm 2 giờ nữa nên thuật toán cộng thêm 1 và tạo ra:```
6
```Điều này xác nhận logic tham lam cũng hoạt động khi chỉ có một khoảng thời gian.
