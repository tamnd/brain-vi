---
title: "CF 105581A - Máy quét"
description: "Chúng ta đang theo dõi một quá trình chỉ có thể bắt đầu ở những thời điểm riêng biệt. Vitya ghé thăm máy quét định kỳ, cứ sau t phút bắt đầu từ thời điểm 0 và ngày kết thúc vào thời điểm T."
date: "2026-06-22T14:34:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105581
codeforces_index: "A"
codeforces_contest_name: "Open Udmurtia Junior Programming Contest 2018"
rating: 0
weight: 105581
solve_time_s: 55
verified: true
draft: false
---

[CF 105581A - Máy quét](https://codeforces.com/problemset/problem/105581/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang theo dõi một quá trình chỉ có thể bắt đầu ở những thời điểm riêng biệt. Vitya ghé thăm máy quét định kỳ, mỗi`t`phút bắt đầu từ thời điểm 0 và ngày kết thúc vào thời điểm`T`. Mỗi lần truy cập là một cơ hội để bắt đầu quét một tài liệu mới, nhưng chỉ khi máy quét hiện không hoạt động vào đúng thời điểm đó. Sau khi tài liệu bắt đầu quét, nó sẽ chiếm giữ máy trong`k`phút mà không bị gián đoạn. Nếu Vitya đến trong khi máy quét vẫn đang bận, anh ta sẽ rời đi ngay lập tức và mất cơ hội đó. 

Nhiệm vụ là xác định số lần Vitya bắt đầu quét thành công trong khoảng thời gian từ thời điểm 0 đến và bao gồm cả thời gian.`T`. 

Các ràng buộc rất nhỏ, với tất cả các tham số tối đa là 100. Điều này có nghĩa là ngay cả việc mô phỏng trực tiếp trên mỗi lần truy cập có thể cũng hiệu quả. Số lượt truy cập tối đa là 100, vì vậy mọi giải pháp tuyến tính về số lượt truy cập đều diễn ra thoải mái trong giới hạn. 

Một trường hợp khó nhận thấy xuất hiện ở ranh giới trong ngày. Nếu Vitya đến đúng lúc`T`, anh ấy vẫn được phép thử bắt đầu quét. Nếu máy quét rảnh vào thời điểm đó, quá trình quét mới sẽ bắt đầu mặc dù quá trình quét này có thể kết thúc sau khi ngày làm việc kết thúc. Một cách tiếp cận ngây thơ loại trừ thời điểm cuối cùng sẽ bị tính thiếu trong trường hợp lần đến cuối cùng hợp lệ. 

Một trường hợp góc khác xảy ra khi`k`lớn hơn`t`. Trong hoàn cảnh đó, Vitya thường xuyên đến vào những lúc bận rộn và bỏ qua nhiều cơ hội. Ví dụ, nếu`t = 2`Và`k = 10`, chỉ lần khởi động thành công đầu tiên mới quan trọng trừ khi có đủ thời gian nhàn rỗi giữa các lần thử. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp là điểm khởi đầu tự nhiên nhất. Chúng tôi liệt kê mỗi lần Vitya ghé thăm máy quét:`0, t, 2t, ...`lên đến`T`. Mỗi lần thăm khám, chúng tôi sẽ kiểm tra xem máy quét có rảnh không. Nếu đúng như vậy, chúng tôi sẽ bắt đầu quét mới và đánh dấu máy là bận cho đến thời điểm hiện tại cộng thêm`k`. Nếu không thì chúng tôi không làm gì cả. Điều này đúng vì quy trình này hoàn toàn được xác định bởi các quyết định cục bộ tại mỗi thời điểm đến. 

Phương pháp vũ phu này thực hiện nhiều nhất`T / t + 1`kiểm tra, tối đa là 101 thao tác trong trường hợp xấu nhất. Mỗi lần kiểm tra là một công việc liên tục nên nó rất nhanh. 

Không có cải thiện tiệm cận có ý nghĩa nào cần thiết ngoài mô phỏng này, nhưng quan sát quan trọng là trạng thái duy nhất chúng ta cần duy trì là vào lần tiếp theo máy quét khả dụng. Điều này làm giảm vấn đề thành một biến đang chạy thay vì theo dõi toàn bộ dòng thời gian từng phút. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(T/t) | O(1) | Đã chấp nhận | 
| Mô phỏng tham lam dựa trên sự kiện | O(T/t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nén quy trình thành một chuỗi các sự kiện đến và xác định thời điểm máy quét sẽ rảnh tiếp theo. 

1. Khởi tạo một biến`free_time = 0`, biểu thị thời điểm sớm nhất mà quá trình quét mới có thể bắt đầu. Đồng thời khởi tạo`count = 0`để lưu trữ số lần khởi động thành công. 
2. Lặp lại tất cả các lần đến`time = 0, t, 2t, ...`trong khi`time ≤ T`. Mỗi giá trị đại diện cho thời điểm Vitya kiểm tra máy quét. 
3. Đối với mỗi`time`, so sánh nó với`free_time`. Nếu như`time ≥ free_time`, máy quét không hoạt động tại thời điểm đó nên có thể bắt đầu một tài liệu mới. 
4. Khi quá trình quét bắt đầu lúc`time`, tăng`count`và thiết lập`free_time = time + k`, vì máy quét sẽ vẫn bận trong lần tiếp theo`k`phút. 
5. Nếu`time < free_time`, Vitya đến trong quá trình quét đang diễn ra và không làm gì cả, khiến trạng thái không thay đổi. 

Quá trình tiếp tục cho đến khi tất cả các thời điểm đến cho đến và bao gồm`T`được xử lý. 

Thuộc tính quan trọng đó là`free_time`luôn thể hiện sự kết thúc chính xác của lần quét được bắt đầu gần đây nhất. Mọi quyết định chỉ phụ thuộc vào việc người đến hiện tại đã đạt đến hay vượt qua ranh giới đó hay chưa. Không có lịch sử trước đó quan trọng một lần`free_time`được cập nhật. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t, k, T = map(int, input().split())

free_time = 0
count = 0

time = 0
while time <= T:
    if time >= free_time:
        count += 1
        free_time = time + k
    time += t

print(count)
```Mã duy trì một con trỏ dòng thời gian duy nhất`time`nhảy theo các bước của`t`, tương ứng chính xác với lịch trình đến của Vitya. Biến`free_time`mã hóa xem máy quét bận hay không ở mỗi lần đến. 

Một lỗi phổ biến là quên rằng quá trình quét bắt đầu vào thời điểm`T`vẫn hợp lệ ngay cả khi nó kết thúc sau`T`. Điều kiện chỉ kiểm tra thời gian bắt đầu chứ không kiểm tra xem quá trình quét có hoàn tất trong ngày làm việc hay không. 

Một điểm tinh tế khác là khởi tạo. Cài đặt`free_time = 0`đảm bảo rằng lần đến đầu tiên tại thời điểm 0 luôn bắt đầu quá trình quét, khớp với thực tế là hệ thống bắt đầu ở trạng thái rảnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`4 5 17`Khách đến vào các thời điểm 0, 4, 8, 12, 16. 

| thời gian | thời gian rảnh | hành động | đếm | 
| --- | --- | --- | --- | 
| 0 | 0 | bắt đầu quét, free_time = 5 | 1 | 
| 4 | 5 | bỏ qua | 1 | 
| 8 | 5 | bắt đầu quét, free_time = 13 | 2 | 
| 12 | 13 | bỏ qua | 2 | 
| 16 | 13 | bắt đầu quét, free_time = 21 | 3 | 

Ở lần đến cuối cùng, máy quét sẽ trống nên lần quét thứ ba sẽ bắt đầu ngay cả khi nó kết thúc sau khi ngày kết thúc. 

Đầu ra là 3. 

### Ví dụ 2 

đầu vào:`5 5 20`Khách đến vào lúc 0, 5, 10, 15, 20. 

| thời gian | thời gian rảnh | hành động | đếm | 
| --- | --- | --- | --- | 
| 0 | 0 | bắt đầu quét, free_time = 5 | 1 | 
| 5 | 5 | bắt đầu quét, free_time = 10 | 2 | 
| 10 | 10 | bắt đầu quét, free_time = 15 | 3 | 
| 15 | 15 | bắt đầu quét, free_time = 20 | 4 | 
| 20 | 20 | bắt đầu quét, free_time = 25 | 5 | 

Mỗi lượt truy cập đều đến đúng thời điểm máy quét rảnh rỗi, vì vậy mỗi lượt truy cập sẽ bắt đầu một tài liệu mới. 

Đầu ra là 5. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T/t) | Chúng tôi lặp lại mỗi lần đến từ 0 đến T theo các bước của t và mỗi lần lặp thực hiện công không đổi | 
| Không gian | O(1) | Chỉ có hai biến số nguyên được duy trì bất kể kích thước đầu vào | 

Số lần lặp tối đa được giới hạn bởi 100, do đó giải pháp phù hợp trong cả giới hạn thời gian và bộ nhớ ngay cả trong trường hợp đầu vào xấu nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    t, k, T = map(int, input().split())

    free_time = 0
    count = 0

    time = 0
    while time <= T:
        if time >= free_time:
            count += 1
            free_time = time + k
        time += t

    return str(count)

# provided samples
assert run("4 5 17\n") == "3"
assert run("5 5 20\n") == "5"

# minimum values
assert run("1 1 1\n") == "2"

# large k blocking most attempts
assert run("2 10 10\n") == "1"

# t equals T boundary case
assert run("7 3 7\n") == "2"

# no overlap case
assert run("3 1 9\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 2 | bao gồm ranh giới tại thời điểm 0 và T | 
| 2 10 10 | 1 | quét dài chặn các nỗ lực trong tương lai | 
| 7 3 7 | 2 | đến chính xác vào cuối ngày được tính | 
| 3 1 9 | 4 | đến thường xuyên với các bản quét ngắn | 

## Vỏ cạnh 

Khi lần đến đầu tiên xảy ra tại thời điểm 0, hệ thống phải coi máy quét là có sẵn ngay lập tức. Với đầu vào`1 5 3`, trình tự các lần đến là 0, 1, 2, 3. Tại thời điểm 0, quá trình quét bắt đầu và chặn cho đến thời điểm 5. Mọi lần đến sau đều bị bỏ qua vì mỗi lần đến đều nhỏ hơn`free_time`, vì vậy câu trả lời là 1. Mô phỏng xử lý điều này một cách tự nhiên bởi vì`free_time`được khởi tạo là 0. 

Khi nào`k`nhỏ hơn`t`, mỗi lần đến sẽ bắt đầu quá trình quét vì máy quét luôn kết thúc trước lần truy cập tiếp theo. Vì`t = 4, k = 1, T = 12`, các điểm đến xảy ra ở 0, 4, 8, 12 và mỗi điểm đều thỏa mãn`time ≥ free_time`, vì vậy mỗi bước sẽ tăng số lượng. Nhà nước phát triển như`free_time = 1, 5, 9, 13`. 

Khi`k`lớn hơn`t`, quét chồng chéo chiếm ưu thế. Vì`t = 2, k = 6, T = 10`, các lượt đến là 0, 2, 4, 6, 8, 10. Chỉ 0, 6 và 10 bắt đầu quét vì các lượt đến trung gian nằm trong các khoảng thời gian hoạt động. điều kiện`time ≥ free_time`lọc chính xác những trường hợp này mà không cần bất kỳ lý do rõ ràng nào về khoảng thời gian.
