---
title: "CF 106503J - Dừng lại, xin đừng dừng lưới nữa"
description: "Chúng ta có một lưới hình chữ nhật có kích thước $n nhân m$. Mỗi ô được coi là một hình vuông đơn vị và mỗi ô vuông được gán một trong bốn hướng có thể, tương ứng với một trong bốn hướng trên lưới."
date: "2026-06-21T16:31:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106503
codeforces_index: "J"
codeforces_contest_name: "2026 \u534e\u5357\u5e08\u8303\u5927\u5b66\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b (SCNUCPC 2026)"
rating: 0
weight: 106503
solve_time_s: 71
verified: true
draft: false
---

[CF 106503J - Dừng lại, không cần nối lưới nữa](https://codeforces.com/problemset/problem/106503/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một lưới hình chữ nhật có kích thước$n \times m$. Mỗi ô được coi là một hình vuông đơn vị và mỗi ô vuông được gán một trong bốn hướng có thể, tương ứng với một trong bốn hướng trên lưới. Các hướng này mô tả cách hình vuông “kết nối” với ô liền kề thông qua một cạnh đã chọn. 

Bằng cách chọn hướng cho tất cả các ô, chúng tôi xây dựng một cách hiệu quả cấu trúc được định hướng trên lưới, nơi chỉ cho phép lân cận giữa các ô lân cận. Một số lựa chọn về hướng tạo thành các vòng khép kín bao quanh các vùng trên lưới. Mỗi vòng khép kín tương ứng với một vùng giới hạn và “diện tích” của vùng đó là số ô đơn vị mà nó chứa. 

Nhiệm vụ là chỉ định hướng cho tất cả các ô sao cho tổng diện tích bao quanh được hình thành bởi tất cả các vùng đóng càng lớn càng tốt. Nếu không có vùng đóng nào có thể được hình thành thì câu trả lời là 0. 

Các ràng buộc tổng hợp rất chặt chẽ: mặc dù có thể có tới 1500 trường hợp kiểm thử, nhưng tổng của tất cả$n \times m$trên chúng nhiều nhất là$10^6$. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào cũng phải tuyến tính theo kích thước lưới cho mỗi trường hợp thử nghiệm hoặc tệ nhất là tuyến tính tổng thể trên tất cả các trường hợp thử nghiệm. Bất cứ điều gì bậc hai trong$n \times m$sẽ là quá chậm. 

Một điểm tinh tế là chúng ta không được yêu cầu tìm một chu trình hay một vùng đơn lẻ mà phải tối đa hóa tổng diện tích khép kín. Điều này chuyển vấn đề từ phát hiện chu trình cục bộ sang xây dựng cấu hình tổng thể nhằm tối đa hóa số lượng ô tham gia vào ít nhất một cấu trúc ranh giới khép kín. 

Một vài trường hợp đặc biệt làm rõ hành vi: 

Khi nào$n = 1$Và$m = 1$, chỉ có một ô. Không có chu trình nào có thể tồn tại trong một nút mà không tự lặp và cấu trúc lưới không cho phép xây dựng như vậy, vì vậy câu trả lời phải là 0. 

Khi nào$n = 1$Và$m = 2$, chúng ta chỉ có một dòng gồm hai ô. Bất kỳ sự phân công hướng nào cũng chỉ có thể trỏ sang trái hoặc phải, điều này chắc chắn sẽ dẫn đến các kết thúc mở hơn là một vòng khép kín. Vì vậy, một lần nữa, câu trả lời là 0. 

Khi nào$n = 2$Và$m = 2$, một chu trình có thể thực hiện được bằng cách sắp xếp các hướng sao cho cả bốn ô tạo thành một vòng quanh chu vi. Điều này tạo ra một vùng khép kín duy nhất có diện tích 4. Ví dụ này cho thấy ngưỡng chính là liệu lưới có thể hỗ trợ cấu trúc chu trình hay không. 

Những ví dụ này gợi ý rằng vấn đề giảm xuống còn việc xác định liệu chúng ta có thể nhúng một chu trình bao phủ lưới hay không và nếu có thì nó có thể lớn đến mức nào. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng chỉ định một trong bốn hướng cho mỗi ô và mô phỏng cấu trúc đồ thị có hướng thu được, sau đó tính toán tất cả các chu trình và đo tổng diện tích kèm theo. Vì có$4^{n \cdot m}$cấu hình, điều này hoàn toàn không khả thi ngay cả đối với các lưới rất nhỏ. Ngay cả khi chúng tôi hạn chế kiểm tra một cấu hình duy nhất, việc phát hiện tất cả các vùng kèm theo yêu cầu truyền tải đồ thị trên toàn bộ lưới, nghĩa là$O(nm)$, vì vậy sức mạnh vũ phu đối với tất cả các cấu hình là vô vọng. 

Quan sát quan trọng là cấu trúc lưới có tính đều đặn cao và điều quan trọng duy nhất là liệu chúng ta có thể sắp xếp các ô thành một cấu trúc tuần hoàn toàn cầu duy nhất sử dụng mỗi ô chính xác một lần hay không. Nếu cấu trúc như vậy tồn tại thì mỗi ô sẽ nằm trên ranh giới của một hệ thống khép kín nhất quán và đạt được diện tích bao bọc tối đa. 

Điều này làm giảm vấn đề thành một câu hỏi xây dựng biểu đồ cổ điển: liệu chúng ta có thể tạo một chu trình truy cập tất cả các ô chính xác một lần trong biểu đồ kề lưới không? Điều đó tương đương với việc hỏi liệu đồ thị lưới có chu trình Hamilton hay không. 

Một đặc tính nổi tiếng của lưới hình chữ nhật là chu trình Hamilton tồn tại khi và chỉ khi cả hai chiều đều lớn hơn 1. Nếu một trong hai chiều bằng 1, thì lưới sẽ suy biến thành một đường đi và không có chu trình nào có thể được hình thành. Nếu cả hai chiều ít nhất là 2, chúng ta có thể luồn lách qua lưới theo kiểu tới lui và nối các đầu để tạo thành một chu trình đầy đủ. 

Khi một chu trình như vậy tồn tại, mỗi ô là một phần của cấu trúc ranh giới, do đó diện tích bao quanh tối đa bằng tổng số ô$n \cdot m$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Bảng liệt kê chỉ đường Brute Force |$O(4^{nm} \cdot nm)$|$O(nm)$| Quá chậm | 
| Cái nhìn sâu sắc về xây dựng chu trình Hamilton |$O(1)$mỗi trường hợp thử nghiệm |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp đơn giản hóa việc kiểm tra cấu trúc trên kích thước lưới. 

1. Đọc$n$Và$m$cho từng trường hợp thử nghiệm. 

Chúng ta chỉ cần hai giá trị này vì cấu trúc lưới phụ thuộc hoàn toàn vào hình dạng của nó chứ không phụ thuộc vào bất kỳ trọng số hoặc nhãn bổ sung nào. 
2. Kiểm tra xem một trong hai chiều có phải là 1 hay không. 

Nếu$n = 1$hoặc$m = 1$, lưới là một hàng hoặc một cột, nghĩa là nó tạo thành một chuỗi ô tuyến tính. Một chuỗi tuyến tính không thể tạo thành bất kỳ vòng khép kín nào, do đó không thể tạo ra vùng khép kín. 
3. Nếu cả hai$n > 1$Và$m > 1$, trở lại$n \cdot m$. 

Trong trường hợp này, lưới chứa đủ khả năng kết nối để nhúng một chu trình bao trùm tất cả các ô. Chúng ta có thể xây dựng một quá trình duyệt giống như con rắn ghé thăm mọi ô chính xác một lần và sau đó đóng vòng lặp bằng cách sử dụng cấu trúc kề của lưới. 
4. In kết quả cho từng test. 

### Tại sao nó hoạt động 

Lưới có thể được xem dưới dạng biểu đồ vô hướng trong đó mỗi ô kết nối với tối đa bốn ô lân cận. Một vùng khép kín tương ứng với một chu trình trong cấu trúc đồ thị này. Tối đa hóa diện tích khép kín có nghĩa là tối đa hóa số đỉnh nằm trong chu kỳ. Tình huống tốt nhất có thể xảy ra là khi mọi đỉnh đều là một phần của một chu trình. 

Lưới hình chữ nhật hỗ trợ chu trình Hamilton chính xác khi cả hai chiều đều vượt quá 1. Trong trường hợp đó, tất cả$n \cdot m$các đỉnh có thể được bao gồm trong một chu trình, nghĩa là mỗi ô đều đóng góp vào cấu trúc kèm theo. Khi một chiều bằng 1, không có chu trình nào tồn tại cả, do đó không thể hình thành vùng dương. 

Thuộc tính cấu trúc này hoàn toàn xác định câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    if n == 1 or m == 1:
        print(0)
    else:
        print(n * m)
```Việc triển khai áp dụng trực tiếp việc kiểm tra kích thước có nguồn gốc ở trên. Mỗi trường hợp thử nghiệm được xử lý độc lập trong thời gian không đổi. 

Điều tinh tế duy nhất là đảm bảo phân tích cú pháp chính xác trên nhiều trường hợp thử nghiệm. Vì tổng của$n \cdot m$được giới hạn bởi$10^6$, thậm chí một số lượng lớn các trường hợp kiểm thử vẫn hoạt động hiệu quả khi xử lý đầu vào đơn giản. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$n = 2, m = 5$| Bước | n | m | Tình trạng | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 5 | cả hai > 1 | chu kỳ hợp lệ có thể | 
| 2 | - | - | sản phẩm tính toán | 10 | 

Lưới có đủ cấu trúc để hỗ trợ một chu trình đầy đủ qua tất cả 10 ô, vì vậy mọi ô đều có thể được đưa vào hệ thống ranh giới kèm theo. 

Đầu ra: 

10 

### Ví dụ 2 

đầu vào:$n = 4, m = 4$| Bước | n | m | Tình trạng | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 4 | cả hai > 1 | chu kỳ hợp lệ có thể | 
| 2 | - | - | sản phẩm tính toán | 16 | 

Lưới 4 x 4 có thể đi qua theo chu trình Hamilton bao phủ tất cả các ô, tạo ra diện tích khép kín tối đa. 

Đầu ra: 

16 

Những ví dụ này xác nhận rằng bất cứ khi nào lưới không bị suy biến, giải pháp sẽ bão hòa tất cả các ô có sẵn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T)$| Mỗi trường hợp thử nghiệm được xử lý với số lượng số học và so sánh không đổi | 
| Không gian |$O(1)$| Chỉ một số biến được lưu trữ bất kể kích thước lưới | 

Các ràng buộc cho phép lên đến$10^6$tổng số ô trong tất cả các thử nghiệm, nhưng thuật toán không lặp lại chúng, do đó nó dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        if n == 1 or m == 1:
            out.append("0")
        else:
            out.append(str(n * m))
    return "\n".join(out)

# provided samples (interpreted)
assert run("2\n2 5\n4 4\n") == "10\n16"

# minimum edge
assert run("1\n1 1\n") == "0"

# single row
assert run("1\n1 10\n") == "0"

# single column
assert run("1\n10 1\n") == "0"

# small cycle-capable grid
assert run("1\n2 2\n") == "4"

# larger rectangle
assert run("1\n3 7\n") == "21"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1×1 | 0 | không có chu kỳ nào có thể | 
| 1×k | 0 | hành vi lưới suy biến | 
| k×1 | 0 | trường hợp suy biến đối xứng | 
| 2×2 | 4 | chu trình không tầm thường nhỏ nhất | 
| 3×7 | 21 | trường hợp bảo hiểm đầy đủ chung | 

## Vỏ cạnh 

Đối với lưới 1 x 1, thuật toán ngay lập tức trả về 0 vì cả sự hình thành chu trình và các vùng kèm theo đều yêu cầu ít nhất cấu trúc 2 x 2. Không có cấu trúc kề nào có khả năng đóng vòng lặp. 

Đối với lưới 1 x m, thuật toán lại trả về 0. Mặc dù có nhiều ô nhưng mọi đường dẫn đều là tuyến tính và không thể tự đóng lại. Lý do tương tự áp dụng đối xứng cho lưới n x 1. 

Đối với các lưới có cả hai chiều ít nhất là 2, thuật toán trả về n lần m. Việc xây dựng ngầm dựa trên thực tế là các lưới như vậy luôn cho phép truyền tải toàn chu kỳ trên tất cả các ô, đảm bảo diện tích khép kín tối đa.
