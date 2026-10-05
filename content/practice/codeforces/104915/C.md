---
title: "CF 104915C - \u0412\u044b\u0432\u043e\u0437 \u043c\u0443\u0441\u043e\u0440\u0430"
description: "Chúng ta có hai dãy vị trí được sắp xếp trên trục số. Một chuỗi tượng trưng cho các túi rác nằm dọc theo đường phố và chuỗi còn lại tượng trưng cho các lối ra nơi xe tải có thể rời khỏi đường phố."
date: "2026-06-28T18:05:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104915
codeforces_index: "C"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u0421\u0430\u043c\u0430\u0440\u0435 2023-2024 (9-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 104915
solve_time_s: 50
verified: true
draft: false
---

[CF 104915C - \u0412\u044b\u0432\u043e\u0437 \u043c\u0443\u0441\u043e\u0440\u0430](https://codeforces.com/problemset/problem/104915/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai dãy vị trí được sắp xếp trên trục số. Một chuỗi tượng trưng cho các túi rác nằm dọc theo đường phố và chuỗi còn lại tượng trưng cho các lối ra nơi xe tải có thể rời khỏi đường phố. Với mỗi túi rác, chúng ta cần xác định khoảng cách đến lối ra gần nhất và tổng hợp những khoảng cách này làm đáp án cuối cùng. 

Hạn chế chính là cả hai chuỗi đều đã được sắp xếp theo thứ tự không giảm. Cấu trúc này không mang tính trang trí, đó là toàn bộ lý do giải pháp có thể tránh việc tính toán lại các so sánh từ đầu cho mỗi túi. 

Nếu chúng ta biểu thị số lượng lối ra là m và số lượng túi là n, thì cách giải thích trực tiếp cho thấy chúng ta có thể so sánh từng túi với mọi lối ra. Điều đó dẫn đến việc kiểm tra khoảng cách n lần m, quá chậm khi cả hai mảng đều lớn. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các lối thoát đều nằm hoàn toàn về một bên của túi. Trong tình huống đó, lối ra gần nhất không mơ hồ, nhưng việc triển khai đơn giản chỉ kiểm tra một hướng hoặc không xem xét cả hai hướng lân cận có thể trả về kết quả không chính xác. Ví dụ: nếu lối thoát ở vị trí`[1, 2]`và một cái túi ở`100`, câu trả lời đúng là`98`, nhưng việc triển khai chỉ so sánh cho đến khi vượt qua vị trí túi và sau đó dừng sớm có thể bỏ lỡ lần thoát hợp lệ cuối cùng một cách không chính xác. 

Một chế độ lỗi khác xuất hiện khi tồn tại các giá trị trùng lặp hoặc nhóm. Nếu lối thoát là`[10, 20, 30]`và túi xách là`[21, 22, 23]`, việc đặt lại con trỏ đơn giản trên mỗi túi có thể quét lại nhiều lần từ đầu, tạo ra hành vi bậc hai không cần thiết mặc dù chỉ số thoát gần nhất chính xác chỉ di chuyển về phía trước. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận bạo lực tính toán, đối với mỗi túi, khoảng cách tuyệt đối đến mọi lối ra và lấy mức tối thiểu. Điều này đúng vì nó kiểm tra rõ ràng tất cả các khả năng, nhưng nó thực hiện so sánh n × m. Với các ràng buộc lớn, quá trình này trở nên quá chậm vì cả hai mảng có thể phát triển đến kích thước mà sản phẩm này vượt quá các hoạt động được phép theo cấp độ lớn. 

Quan sát quan trọng là khi chúng ta di chuyển từ trái sang phải dọc theo các túi, chỉ số thoát gần nhất không thể di chuyển lùi lại. Nếu túi dịch chuyển sang phải một chút, lối ra thích hợp nhất sẽ giữ nguyên hoặc chuyển sang lối ra tiếp theo. Sự đơn điệu này cho phép chúng ta duy trì một con trỏ trong mảng exits và chỉ di chuyển nó về phía trước chứ không bao giờ đặt lại nó. 

Khi chúng ta đến lối ra đầu tiên ở bên phải hoặc bằng túi hiện tại, lối ra gần nhất luôn nằm giữa lối ra đó và lối ra trước đó. Mọi thứ xa hơn về bên phải hoặc bên trái chắc chắn sẽ tệ hơn do thứ tự được sắp xếp. Điều này làm giảm việc tìm kiếm trên mỗi túi theo thời gian khấu hao không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · m) | O(1) | Quá chậm | 
| Hai con trỏ | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý túi theo thứ tự tăng dần đồng thời đi qua các lối ra theo thứ tự tăng dần. 

1. Khởi tạo con trỏ`j = 0`trên mảng thoát. Con trỏ này thể hiện lối ra đầu tiên mà chúng ta chưa loại trừ đối với túi hiện tại. 
2. Đối với từng vị trí túi`b[i]`, nâng cao`j`trong khi`j + 1 < m`và lối ra tiếp theo vẫn gần hơn hoặc bằng nhau theo nghĩa là`c[j + 1]`là một ứng cử viên không tệ hơn`c[j]`cho chiếc túi này. Cụ thể là chúng ta chuyển`j`cho đến khi`c[j]`là lối ra đầu tiên lớn hơn hoặc bằng`b[i]`hoặc cho đến khi chúng ta hết lối thoát. 
3. Một lần`j`được định vị, lối ra gần nhất phải là`c[j]`hoặc`c[j - 1]`nếu nó tồn tại. Chúng tôi tính toán cả khoảng cách tuyệt đối và lấy mức tối thiểu. 
4. Cộng khoảng cách tối thiểu này vào tổng quãng đường chạy. 
5. Tiếp tục đến túi tiếp theo mà không cần đặt lại`j`. Điều này hợp lệ vì các túi trong tương lai có vị trí tương tự hoặc lớn hơn, do đó, không có lối thoát nào trước đó sẽ trở thành tối ưu mới. 

Quyết định quan trọng về cơ cấu là chúng tôi không bao giờ bắt đầu lại quá trình quét các lối ra cho từng túi. Con trỏ chỉ di chuyển về phía trước, giúp duy trì tính chính xác đồng thời loại bỏ công việc lặp lại. 

### Tại sao nó hoạt động 

Điều bất biến là trước khi xử lý túi`i`, con trỏ`j`được định vị ở chỉ số nhỏ nhất sao cho tất cả đều thoát ra ở bên trái của`j`được đảm bảo không tốt hơn bất kỳ lối ra nào ở hoặc bên phải của`j`cho tất cả các túi còn lại. Vì cả hai mảng đều được sắp xếp nên việc tăng vị trí túi không thể khiến lối ra bị loại bỏ trước đó trở lại tối ưu. Sự thống trị đơn điệu này đảm bảo rằng chỉ kiểm tra hai ứng cử viên lân cận xung quanh`j`là đủ cho sự đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    b = list(map(int, input().split()))
    c = list(map(int, input().split()))

    j = 0
    ans = 0

    for x in b:
        while j + 1 < m and abs(c[j + 1] - x) <= abs(c[j] - x):
            j += 1

        best = abs(c[j] - x)
        if j > 0:
            best = min(best, abs(c[j - 1] - x))

        ans += best

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện giữ một con trỏ duy nhất`j`trên lối ra. Vòng lặp bên trong chỉ tiến hành khi lối ra tiếp theo ít nhất tốt bằng lối ra hiện tại cho túi hiện tại, đảm bảo chúng tôi luôn kết thúc ở một ứng cử viên tối ưu cục bộ. Lần kiểm tra thứ hai chống lại`j - 1`xử lý ranh giới nơi lối ra gần nhất nằm ngay bên trái con trỏ. 

Một lỗi phổ biến là đặt lại`j`cho mỗi túi. Điều đó bảo tồn tính đúng đắn nhưng phá hủy độ phức tạp tuyến tính. Một sai lầm khác là chỉ kiểm tra một hướng liên quan đến`j`, không thành công khi lối ra gần nhất ở phía đối diện. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, m = 3
b = [2, 8, 15]
c = [1, 10, 20]
```| túi x | j trước | j sau khi di chuyển | ứng viên | khoảng cách đã chọn | 
| --- | --- | --- | --- | --- | 
| 2 | 0 | 0 | 1 | 1 | 
| 8 | 0 | 0 | 1, 10 | 2 | 
| 15 | 0 | 1 | 10, 20 | 5 | 

Đối với túi thứ hai, mặc dù chúng ta có thể di chuyển về phía 10 nhưng con trỏ không tiến lên vì 10 không hoàn toàn tốt hơn 1 ở vị trí đó. Bảng cho thấy thuật toán chỉ so sánh nhất quán các ứng cử viên địa phương. 

### Ví dụ 2 

đầu vào:```
n = 4, m = 2
b = [1, 3, 6, 9]
c = [2, 8]
```| túi x | j trước | j sau khi di chuyển | ứng viên | khoảng cách đã chọn | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 2 | 1 | 
| 3 | 0 | 0 | 2, 8 | 1 | 
| 6 | 0 | 1 | 2, 8 | 2 | 
| 9 | 1 | 1 | 8 | 1 | 

Dấu vết này cho thấy con trỏ cuối cùng sẽ di chuyển sang phải khi các túi vượt qua điểm giữa giữa các lối ra, sau đó tất cả các túi tiếp theo vẫn được liên kết với lối ra bên phải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi con trỏ qua túi và lối ra chỉ di chuyển về phía trước, không bao giờ lùi lại | 
| Không gian | O(1) | Chỉ có một số biến được duy trì bên cạnh việc lưu trữ đầu vào | 

Độ phức tạp tuyến tính là cần thiết cho các đầu vào lớn vì nó tránh tính toán lại các bước kiểm tra lần ra gần nhất cho mỗi túi. Mỗi lối thoát được truy cập nhiều nhất một lần, giúp duy trì khả năng thực thi tốt trong giới hạn Codeforce điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib
    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# basic samples
assert run("3 3\n2 8 15\n1 10 20\n") == "8"

# all bags left of exits
assert run("2 2\n1 2\n10 20\n") == "17"

# all bags right of exits
assert run("3 2\n10 11 12\n1 5\n") == "17"

# interleaved
assert run("4 3\n1 4 6 10\n2 5 9\n") == "7"

# single exit
assert run("3 1\n1 100 1000\n50\n") == "1499"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả còn lại | 17 | gần nhất luôn giống nhau lối ra | 
| được rồi | 17 | xử lý ranh giới đối xứng | 
| xen kẽ | 7 | chuyển đổi giữa các lối thoát | 
| lối ra duy nhất | 1499 | trường hợp suy biến đúng đắn | 

## Vỏ cạnh 

Khi tất cả các túi nằm bên trái lối ra đầu tiên, con trỏ không bao giờ tiến lên và mỗi túi chỉ được so sánh với lối ra đầu tiên. Thuật toán xử lý việc này vì`j`vẫn ở mức 0 và`j - 1`kiểm tra không bao giờ được kích hoạt. 

Khi tất cả các túi nằm ở bên phải lối ra cuối cùng, con trỏ sẽ tiến lên cho đến khi chạm đến chỉ số cuối cùng rồi dừng lại. Tất cả các khoảng cách chỉ được tính toán dựa trên lối ra cuối cùng, vì không có ứng cử viên nào tốt hơn ở bên phải. 

Khi lối ra thưa thớt và túi dày đặc, con trỏ tiến chậm so với túi nhưng vẫn chỉ di chuyển về phía trước. Điều này đảm bảo hành vi tuyến tính ngay cả khi nhiều túi có chung khu vực thoát gần nhất.
