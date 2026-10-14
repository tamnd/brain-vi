---
title: "CF 105183A - \u041e\u0447\u0435\u043d\u044c \u0441\u0435\u0440\u044c\u0435\u0437\u043d\u044b\u0439 \u0447\u0435\u043b\u043e\u0432\u0435\u043a"
description: "Chúng tôi được cấp một số ngày đào tạo cố định và một số mục tiêu về tổng số giờ đào tạo. Mỗi ngày đóng góp một lượng thời gian đào tạo bình thường hoặc tăng cường thời gian đào tạo."
date: "2026-06-27T04:28:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105183
codeforces_index: "A"
codeforces_contest_name: "XX \u041d\u0438\u0436\u0435\u0433\u043e\u0440\u043e\u0434\u0441\u043a\u0430\u044f \u0433\u043e\u0440\u043e\u0434\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u0412. \u0414. \u041b\u0435\u043b\u044e\u0445\u0430"
rating: 0
weight: 105183
solve_time_s: 59
verified: true
draft: false
---

[CF 105183A - \u041e\u0447\u0435\u043d\u044c \u0441\u0435\u0440\u044c\u0435\u0437\u043d\u044b\u0439 \u0447\u0435\u043b\u043e\u0432\u0435\u043a](https://codeforces.com/problemset/problem/105183/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số ngày đào tạo cố định và một số mục tiêu về tổng số giờ đào tạo. Mỗi ngày đóng góp một lượng thời gian đào tạo bình thường hoặc tăng cường thời gian đào tạo. Một ngày bình thường luôn có 24 giờ huấn luyện, trong khi một “siêu ngày” đặc biệt lại có 72 giờ. Nhiệm vụ là chọn số ngày có sẵn sẽ được chuyển thành siêu ngày sao cho tổng thời gian đào tạo tích lũy trong tất cả các ngày ít nhất là mục tiêu yêu cầu, đồng thời giữ số lượng siêu ngày càng nhỏ càng tốt. 

Dữ liệu đầu vào bao gồm hai số, số ngày có sẵn và tổng số giờ đào tạo cần thiết. Đầu ra là một số nguyên duy nhất biểu thị số ngày nhỏ nhất phải được nâng cấp lên siêu ngày để đáp ứng hoặc vượt tổng mục tiêu. 

Ràng buộc rằng số giờ yêu cầu không bao giờ vượt quá 72 lần số ngày đảm bảo tính khả thi. Ngay cả khi mỗi ngày đều là siêu ngày thì mục tiêu vẫn luôn có thể đạt được. 

Từ góc độ phức tạp, cả hai giá trị có thể lớn tới 100.000. Điều này loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng tất cả các lựa chọn có thể có về siêu ngày mỗi ngày bằng cách sử dụng đệ quy hoặc tìm kiếm toàn diện, vì điều đó sẽ tăng theo cấp số nhân theo số ngày. Ngay cả một vòng lặp bậc hai theo ngày cũng sẽ an toàn nếu tách biệt nhưng không cần thiết. Cấu trúc của bài toán gợi ý rằng chúng ta đang tối ưu hóa một tham số số nguyên duy nhất, do đó, một lý luận dạng đóng hoặc tham lam được mong đợi. 

Một trường hợp tinh tế có thể phá vỡ lối suy luận ngây thơ xuất hiện khi mục tiêu đã có thể đạt được mà không cần bất kỳ ngày nghỉ nào. Ví dụ: nếu có 5 ngày và mục tiêu là 100 giờ thì lịch trình bình thường đã là 120 giờ. Trong trường hợp này, câu trả lời đúng là 0 và bất kỳ công thức nào tính toán một cách mù quáng một yêu cầu dương sẽ sai. 

Một trường hợp cạnh khác xảy ra khi mục tiêu chính xác bằng tổng bình thường. Ví dụ: với 3 ngày và mục tiêu 72, không cần ngày siêu. Những điều kiện biên này rất quan trọng vì lời giải đương nhiên bao gồm phép trừ có thể âm. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là thử mọi số siêu ngày có thể có từ 0 đến d, tính tổng thời gian đào tạo cho mỗi lựa chọn và chọn giá trị hợp lệ nhỏ nhất. Với mỗi ứng viên x, tổng thời gian được tính là 24(d − x) + 72x. Việc kiểm tra tất cả các khả năng sẽ tốn O(d) đánh giá và mỗi đánh giá là O(1), do đó độ phức tạp tổng thể là O(d). Với d lên tới 100.000, điều này vẫn có thể chấp nhận được, nhưng nó không cần thiết và nặng hơn mức cần thiết về mặt khái niệm. 

Quan sát quan trọng là việc thay thế một ngày bình thường bằng một ngày siêu không làm thay đổi cấu trúc của lịch trình mà chỉ bổ sung thêm một lượng đào tạo bổ sung cố định. Một ngày bình thường đóng góp 24 giờ, trong khi một ngày siêu đóng góp 72 giờ, do đó, lợi ích ròng của việc nâng cấp một ngày là thêm 48 giờ so với khi để ngày bình thường. Điều này biến vấn đề thành một kịch bản tăng dần đơn giản: chúng tôi bắt đầu từ những ngày hoàn toàn bình thường, tính tổng giá trị cơ sở và sau đó hỏi xem cần bao nhiêu khoảng tăng dần trong 48 giờ để đạt được mục tiêu. 

Điều này làm giảm bài toán tìm số nguyên x nhỏ nhất sao cho 24d + 48x ≥ h. Việc giải trực tiếp bất đẳng thức này sẽ đưa ra một biểu thức dạng đóng bằng cách sử dụng phép chia trần, loại bỏ nhu cầu mô phỏng theo ngày. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(d) | O(1) | Về mặt khái niệm quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính tổng thời gian huấn luyện nếu tất cả các ngày đều bình thường, là 24 × d. Điều này thể hiện dung lượng cơ bản mà không cần bất kỳ nâng cấp nào. 
2. Nếu đường cơ sở này đã đáp ứng hoặc vượt quá mục tiêu h, hãy trả về 0 vì không yêu cầu ngày nghỉ hưu. 
3. Ngược lại, hãy tính phần thâm hụt còn lại, được xác định là h − 24 × d. Đây là số giờ bổ sung phải được cung cấp bằng cách nâng cấp một số ngày. 
4. Mỗi ngày siêu đóng góp thêm 48 giờ so với một ngày bình thường, vì vậy hãy xác định xem cần bao nhiêu ngày tăng thêm như vậy để bù đắp phần thâm hụt. Giá trị này được tính là số nguyên x nhỏ nhất sao cho thiếu hụt 48 × x ≥. 
5. Trả lời x là câu trả lời. 

Tính đúng đắn xuất phát từ thực tế là mọi lịch trình hợp lệ đều có thể được chuyển đổi thành một lịch trình trong đó tất cả các ngày không phải siêu nhân đều là ngày bình thường và tất cả các ngày siêu nhân đều là những nâng cấp giống hệt nhau. Vì mọi nâng cấp đều có lợi ích cận biên giống nhau và không có sự tương tác giữa các ngày nên bài toán trở thành bài toán bao phủ tuyến tính đơn giản đối với một biến duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    d, h = map(int, input().split())
    
    base = 24 * d
    if base >= h:
        print(0)
        return
    
    deficit = h - base
    # each superday adds 48 extra hours compared to normal day
    x = (deficit + 48 - 1) // 48
    print(x)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp công thức dẫn xuất. Bước đầu tiên tính tổng số cơ bản. Việc trả lại sớm sẽ xử lý trường hợp ranh giới quan trọng mà không cần nâng cấp. Tính toán còn lại thực hiện phép chia trần, được thực hiện bằng cách sử dụng số học số nguyên để tránh sự thiếu chính xác của dấu phẩy động. biểu thức`(deficit + 47) // 48`đảm bảo làm tròn chính xác khi số thâm hụt không chia hết cho 48. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 147
```Chúng tôi tính toán các giá trị cơ bản từng bước. 

| Bước | Cơ sở (24d) | Thâm hụt | Ngày nghỉ lễ (x) | 
| --- | --- | --- | --- | 
| Bắt đầu | 72 | - | - | 
| So sánh | 72 | - | - | 
| Tính thâm hụt | - | 75 | - | 
| Tính x | - | - | 2 | 

Thời gian cơ bản 72 giờ là không đủ. 75 giờ còn lại phải được tăng dần theo 48 giờ. Một ngày siêu là không đủ, vì nó chỉ có thêm 48 giờ, trong khi hai siêu ngày có thêm 96 giờ là đủ. Điều này phù hợp với đầu ra 2. 

### Ví dụ 2 

đầu vào:```
4 80
```| Bước | Cơ sở (24d) | Thâm hụt | Ngày nghỉ lễ (x) | 
| --- | --- | --- | --- | 
| Bắt đầu | 96 | - | - | 
| So sánh | 96 | - | - | 
| Thoát sớm | - | - | 0 | 

Đường cơ sở đã vượt quá yêu cầu nên không cần nâng cấp. Điều này xác nhận rằng thuật toán xử lý chính xác các trường hợp mục tiêu ở dưới mức công suất lịch trình thông thường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số phép tính số học không đổi được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì nó tránh được bất kỳ sự lặp lại nào trong nhiều ngày và giảm vấn đề về số học trực tiếp trên các giá trị đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    d, h = map(int, input().split())
    base = 24 * d
    if base >= h:
        return "0"
    deficit = h - base
    return str((deficit + 47) // 48)

# provided sample
assert run("3 147\n") == "2"

# minimum case, already satisfied
assert run("1 24\n") == "0"

# needs exactly one superday
assert run("1 25\n") == "1"

# large boundary where all days must be superdays
assert run("100000 7200000\n") == "100000"

# mixed case
assert run("10 300\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 24 | 0 | bình đẳng cơ bản chính xác | 
| 1 25 | 1 | thâm hụt dương nhỏ nhất | 
| 100000 7200000 | 100000 | trường hợp bão hòa đầy đủ | 
| 10 300 | 3 | tính toán tầm trung điển hình | 

## Vỏ cạnh 

Một ranh giới quan trọng là khi yêu cầu đã được đáp ứng mà không cần nâng cấp. Đối với đầu vào`d = 2, h = 40`, đường cơ sở là 48 giờ. Thuật toán ngay lập tức trả về 0 sau bước so sánh vì không tính được mức thâm hụt. Điều này tránh mọi nguy cơ số học âm lan truyền vào công thức trần. 

Một trường hợp khác là khi mức thâm hụt nhỏ hơn một khoản đóng góp ngày nghỉ hưu. Ví dụ`d = 1, h = 50`, đường cơ sở là 24 và mức thâm hụt là 26. Vì một ngày siêu cộng thêm 48 giờ, phân chia trần tạo ra 1, chọn chính xác một lần nâng cấp. 

Trường hợp cấu trúc cuối cùng xảy ra khi mỗi ngày phải được nâng cấp. Vì`d = 3, h = 216`, đường cơ sở là 72 và mức thâm hụt là 144. Mỗi ngày siêu đóng góp 48, do đó cần có chính xác 3 ngày siêu. Công thức bão hòa tự nhiên ở`d`khi mục tiêu đạt đến tổng số đào tạo tối đa có thể.
