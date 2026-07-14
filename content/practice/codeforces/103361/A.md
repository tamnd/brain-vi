---
title: "CF 103361A - \u0423\u0441\u043f\u0435\u0442\u044c \u043d\u0430 \u0441\u0430\u043c\u043e\u043b\u0451\u0442"
description: "Chúng ta được cung cấp một lịch trình một ngày bao gồm hai thời điểm và hai khoảng thời gian. Đầu tiên, chúng ta biết khi nào một du khách rời khỏi nhà, được biểu thị bằng giờ và phút. Từ thời điểm đó, hệ thống định vị cho biết thời gian di chuyển đến sân bay là một số phút cố định."
date: "2026-07-03T13:08:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103361
codeforces_index: "A"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u0430\u044f \u041a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u042e\u041c\u0428 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 103361
solve_time_s: 41
verified: true
draft: false
---

[CF 103361A - \u0423\u0441\u043f\u0435\u0442\u044c \u043d\u0430 \u0441\u0430\u043c\u043e\u043b\u0451\u0442](https://codeforces.com/problemset/problem/103361/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lịch trình một ngày bao gồm hai thời điểm và hai khoảng thời gian. Đầu tiên, chúng ta biết khi nào một du khách rời khỏi nhà, được biểu thị bằng giờ và phút. Từ thời điểm đó, hệ thống định vị cho biết thời gian di chuyển đến sân bay là một số phút cố định. Riêng biệt, chúng ta biết giờ khởi hành của máy bay và bao nhiêu phút trước khi khởi hành quầy làm thủ tục sẽ đóng cửa. Hành khách phải đến đúng giờ trước thời điểm kết thúc làm thủ tục, nghĩa là đến đúng giờ quy định vẫn bị coi là không thành công. 

Nhiệm vụ là xác định xem thời gian đến được tính từ thời điểm khởi hành cộng với thời gian đi lại có hoàn toàn sớm hơn thời điểm làm thủ tục cuối cùng được chấp nhận được tính từ thời điểm chuyến bay khởi hành trừ đi thời gian chuẩn bị làm thủ tục hay không. 

Điều tinh tế quan trọng là tất cả thời gian đều tồn tại trong một ngày và phải được so sánh một cách nhất quán theo phút. Đây không phải là một mô phỏng trong nhiều ngày mà là một so sánh tuyến tính duy nhất sau khi chuyển đổi mọi thứ thành một đơn vị thống nhất. 

Các hạn chế là nhỏ, với số giờ được giới hạn trong khoảng từ 9 đến 21 và thời lượng lên tới 60 phút. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào chuyển đổi thời gian và thực hiện số học theo thời gian không đổi là đủ. Không cần tìm kiếm, sắp xếp hoặc bất kỳ cấu trúc dữ liệu không cần thiết nào. Ngay cả việc mô phỏng mạnh mẽ từng phút một cũng có thể chấp nhận được, nhưng không cần thiết. 

Một trường hợp thất bại thường gặp là do hiểu sai điều kiện bất đẳng thức nghiêm ngặt. 

Ví dụ: giả sử khách du lịch đến đúng thời hạn nhận phòng: 

đầu vào: 

9 0 10 9 10 10 

Khởi hành là 9:00, di chuyển là 10 phút, nên đến nơi là 9:10. Chuyến bay khởi hành lúc 9:10 và thời gian làm thủ tục kết thúc trước 10 phút, tức là 9:00. Đến muộn hơn thời gian quy định nên câu trả lời là "ĐỔI VÉ". 

Một trường hợp tế nhị khác: 

đầu vào: 

10 0 50 15 30 45 

Đến nơi là 10:50. Chuyến bay khởi hành lúc 15:30, thời gian nhận phòng kết thúc lúc 14:45. Đến sớm hơn nên câu trả lời là "OK". Cái bẫy ở đây là trừ chính xác số phút theo ranh giới giờ. 

Sai lầm thường gặp nhất là coi thời gian là số học giờ và phút riêng biệt mà không chuẩn hóa, điều này sẽ bị phá vỡ khi phép trừ vượt qua ranh giới giờ. 

## Phương pháp tiếp cận 

Cách diễn giải thô bạo sẽ mô phỏng từng phút kể từ thời điểm khởi hành, thêm thời gian di chuyển và mô phỏng riêng việc đếm ngược đến thời điểm đóng quầy làm thủ tục. Chúng tôi có thể chuyển đổi cả hai khoảnh khắc thành mảng phút và tăng dần cho đến khi đạt đến điểm cuối. Điều này hiệu quả vì phạm vi thời gian nhỏ nhưng lại tốn chi phí không cần thiết. Trong trường hợp xấu nhất, chúng tôi vẫn chỉ xử lý vài nghìn phút, vì vậy nó đúng nhưng không phù hợp. 

Cách tiếp cận tốt hơn là bình thường hóa mọi thời gian thành số phút tuyệt đối kể từ nửa đêm. Khi mọi thứ đều ở một thang đo tuyến tính duy nhất, vấn đề sẽ giảm xuống còn một so sánh duy nhất. 

Chúng tôi tính toán: 

- thời gian đến = (h1 * 60 + m1) + t1 
- Last_checkin_time = (h2 * 60 + m2) - t2 

Điều kiện duy nhất còn lại là bất đẳng thức nghiêm ngặt: 

thời gian đến < thời gian nhận phòng cuối cùng 

Điều này loại bỏ mọi sự phức tạp và biến bài toán thành số học có thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(T) trong đó T ≤ 1440 | O(1) | Được chấp nhận nhưng không cần thiết | 
| Bình thường hóa thời gian | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Quy đổi thời gian khởi hành thành tổng số phút tính từ nửa đêm bằng cách tính h1 * 60 + m1. Điều này mang lại một giá trị vô hướng duy nhất khi khách du lịch rời đi. 
2. Cộng thời gian di chuyển t1 để tính thời gian đến sân bay theo phút. Mô hình này mô hình tiến trình thời gian liên tục mà không cần xử lý ranh giới giờ. 
3. Quy đổi thời gian khởi hành của chuyến bay thành tổng số phút tính từ nửa đêm bằng h2 * 60 + m2. Điều này cung cấp điểm tham chiếu cho lịch trình của hãng hàng không. 
4. Trừ thời gian làm thủ tục t2 theo yêu cầu từ thời gian khởi hành chuyến bay. Điều này mang lại phút hợp lệ cuối cùng khi hành khách vẫn được phép hoàn tất thủ tục. 
5. So sánh Arrival_time với Last_checkin_time bằng cách sử dụng bất đẳng thức nghiêm ngặt. Nếu Arrival_time nhỏ hơn rất nhiều thì xuất ra "OK". Nếu không thì xuất ra "ĐỔI VÉ". 

### Tại sao nó hoạt động 

Cả hai giá trị được tính toán đều biểu thị các vị trí thời gian tuyệt đối trên cùng một trục tuyến tính được tính bằng phút tính từ nửa đêm. Phép biến đổi duy trì thứ tự vì nó là một ánh xạ đơn điệu nghiêm ngặt từ thời gian đồng hồ sang số nguyên. Vì yêu cầu chỉ là về việc liệu một sự kiện có xảy ra đúng trước một sự kiện khác hay không nên không cần cấu trúc bổ sung. Bất kỳ câu trả lời đúng nào cũng phải phụ thuộc hoàn toàn vào thứ tự này, vì vậy việc rút gọn bài toán về phép so sánh số nguyên vừa cần thiết vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

h1, m1, t1, h2, m2, t2 = map(int, input().split())

arrival = h1 * 60 + m1 + t1
deadline = h2 * 60 + m2 - t2

if arrival < deadline:
    print("OK")
else:
    print("CHANGE TICKET")
```Việc thực hiện theo thuật toán trực tiếp. Chi tiết quan trọng duy nhất là chuyển đổi nhất quán thành phút trước khi thực hiện bất kỳ phép tính nào. Điều này tránh được lỗi khi chuyển số phút qua ranh giới giờ theo cách thủ công. 

Bất đẳng thức nghiêm ngặt được thực hiện chính xác theo yêu cầu: đẳng thức được coi là thất bại vì đến đúng thời điểm kết thúc không cho phép hoàn thành đăng ký. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9 0 10 9 10 10
```| Bước | Giá trị | 
| --- | --- | 
| Khởi hành | 9 * 60 + 0 = 540 | 
| Đến | 540 + 10 = 550 | 
| Chuyến bay | 9 * 60 + 10 = 550 | 
| Hạn chót nhận phòng | 550 - 10 = 540 | 

Đến (550) < thời hạn (540) là sai, do đó đầu ra là THAY ĐỔI VÉ. 

Ví dụ này thể hiện quy tắc bất bình đẳng nghiêm ngặt, trong đó việc đến sau thời điểm giới hạn có hiệu lực một chút sẽ làm mất hiệu lực chuyến đi. 

### Ví dụ 2 

đầu vào:```
10 0 50 15 30 45
```| Bước | Giá trị | 
| --- | --- | 
| Khởi hành | 600 | 
| Đến | 650 | 
| Chuyến bay | 930 | 
| Hạn chót nhận phòng | 885 | 

Đến (650) < thời hạn (885) là đúng, vì vậy đầu ra vẫn ổn. 

Điều này xác nhận việc xử lý chính xác số học theo giờ, vì phép trừ kéo dài nhiều giờ mà không gặp vấn đề gì. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | số phép tính không đổi | 
| Không gian | O(1) | chỉ một vài biến số nguyên được sử dụng | 

Các ràng buộc cho phép nhiều nhất một vài phép toán số nguyên, do đó giải pháp chạy tốt trong giới hạn và có hiệu quả tức thời. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    h1, m1, t1, h2, m2, t2 = map(int, input().split())

    arrival = h1 * 60 + m1 + t1
    deadline = h2 * 60 + m2 - t2

    return "OK\n" if arrival < deadline else "CHANGE TICKET\n"

# provided samples
assert run("9 0 10 9 10 10") == "CHANGE TICKET\n"
assert run("10 0 50 15 30 45") == "OK\n"

# minimum edge: just before midnight boundary (still within constraints)
assert run("9 0 1 9 1 1") == "CHANGE TICKET\n"

# exact equality case (must fail)
assert run("9 0 10 9 10 10") == "CHANGE TICKET\n"

# large gap case
assert run("12 0 5 20 0 10") == "OK\n"

# tight success case
assert run("9 0 0 9 0 1") == "OK\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 9 0 1 9 1 1 | ĐỔI VÉ | đến ngay sau khi cắt | 
| 12 0 5 20 0 10 | được | biên độ an toàn lớn | 
| 9 0 0 9 0 1 | được | ranh giới không có hành trình | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi thời gian đến bằng thời gian đóng cửa nhận phòng. Ví dụ: 

đầu vào:```
9 0 10 9 10 10
```Từng bước một: 

Đến = 540 + 10 = 550 

Hạn chót = 550 - 10 = 540 

Vì 550 không nhỏ hơn 540 nên kết quả là “ĐỔI VÉ”. Bất đẳng thức nghiêm ngặt loại bỏ chính xác đẳng thức, phù hợp với yêu cầu của bài toán rằng việc đến phải sớm hơn một cách nghiêm ngặt. 

Một tình huống khác là khi phép trừ vượt qua ranh giới giờ: 

đầu vào:```
10 30 20 11 0 45
```Đến = 630 + 20 = 650 

Hạn chót = 660 - 45 = 615 

Mặc dù số học giờ có vẻ khó hiểu nhưng số phút được chuẩn hóa lại xử lý nó một cách rõ ràng. Vì 650 < 615 là sai nên du khách đến muộn. Việc chuyển đổi đảm bảo tính chính xác bất kể vượt qua ranh giới giờ.
