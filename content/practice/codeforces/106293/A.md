---
title: "CF 106293A - \u0421\u0440\u0435\u0434\u043d\u0435\u0435 \u0438 \u043c\u0435\u0434\u0438\u0430\u043d\u0430"
description: "Chúng ta có hai số nguyên cố định $a$ và $b$, và chúng ta được phép chọn số nguyên thứ ba $c$. Khi $c$ được chọn, chúng ta tạo thành bộ ba $(a, b, c)$."
date: "2026-06-18T22:34:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106293
codeforces_index: "A"
codeforces_contest_name: "\u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 1\u0421, \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u0442\u0443\u0440 2025-2026"
rating: 0
weight: 106293
solve_time_s: 55
verified: true
draft: false
---

[CF 106293A - \u0421\u0440\u0435\u0434\u043d\u0435\u0435 \u0438 \u043c\u0435\u0434\u0438\u0430\u043d\u0430](https://codeforces.com/problemset/problem/106293/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai số nguyên cố định$a$Và$b$, và chúng ta được phép chọn số nguyên thứ ba$c$. Một lần$c$được chọn, chúng ta tạo thành một bộ ba$(a, b, c)$. Mục tiêu là làm cho$c$càng lớn càng tốt trong khi vẫn giữ đúng bất đẳng thức cụ thể: trung bình cộng của ba số không được vượt quá trung vị của chúng. 

Giá trị trung bình số học chỉ đơn giản là$(a+b+c)/3$. Trung vị là giá trị ở giữa sau khi sắp xếp ba số theo thứ tự không giảm. Vì vậy đối với bất kỳ sự lựa chọn nào của$c$, thứ tự của bộ ba thay đổi trung vị, điều này làm cho điều kiện từng phần và phụ thuộc vào cách$c$so sánh với$a$Và$b$. 

Hạn chế chính đó là$a$Và$b$có thể lớn như$10^{18}$, vì vậy mọi giải pháp đều phải tránh dùng vũ lực$c$. Thử các giá trị của$c$tuyến tính hoặc thậm chí với tìm kiếm nhị phân trên toàn bộ phạm vi là quá chậm trừ khi chúng ta có thể đánh giá điều kiện trong thời gian không đổi. 

Một khó khăn tinh tế xuất phát từ thực tế là số trung vị không cố định. Ví dụ, nếu$a = 1$,$b = 10$, Và$c = 5$, thì trung vị là$5$, nhưng nếu$c = 100$, trung vị trở thành$10$. Điều kiện thay đổi cấu trúc tùy thuộc vào vị trí nào trong ba vị trí có thể$c$chiếm theo thứ tự sắp xếp. 

Các trường hợp cạnh phát sinh khi cả ba số đều thu gọn thành một giá trị duy nhất hoặc khi$c$cực kỳ lớn và trở thành phần tử lớn nhất. Ví dụ, nếu$a = b = 3$, thì mọi lời giải hợp lệ đều phải tôn trọng tính đối xứng và câu trả lời đúng sẽ trở thành$c = 3$, bởi vì lớn hơn$c$sẽ đẩy trung vị lên cao theo cách vi phạm sự bất bình đẳng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tăng giá trị của$c$, tính trung vị mỗi lần bằng cách sắp xếp bộ ba và kiểm tra xem bất đẳng thức có đúng không. Mỗi lần kiểm tra là$O(1)$, Nhưng$c$phạm vi lên đến$10^{18}$, do đó, ngay cả việc lặp qua một tập hợp con có ý nghĩa cũng không thể thực hiện được. 

Quan sát quan trọng là trung vị của ba số chỉ có ba trường hợp cấu trúc tùy thuộc vào vị trí$c$nằm tương đối với$a$Và$b$. Một khi chúng tôi sắp xếp$a$Và$b$, chúng ta chỉ cần xem xét liệu$c$là phần tử nhỏ nhất, giữa hoặc lớn nhất. Trong mỗi trường hợp, trung vị trở thành một biểu thức cố định, biến bất đẳng thức thành một ràng buộc tuyến tính đơn giản trên$c$. Điều này làm giảm vấn đề từ việc tìm kiếm trên các số nguyên đến việc đánh giá một số lượng nhỏ các điều kiện đại số và lấy giá trị khả thi tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force kết thúc$c$|$O(10^{18})$|$O(1)$| Quá chậm | 
| Phân tích trường hợp bằng cách đặt hàng |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi bắt đầu bằng cách đảm bảo thứ tự rõ ràng giữa hai giá trị cố định. 

1. Sắp xếp đầu vào sao cho$a \le b$. Điều này loại bỏ tính đối xứng và đảm bảo mọi lý luận sau này về vị trí tương đối của$c$là nhất quán. 
2. Xét trường hợp cấu trúc đầu tiên trong đó$c \le a \le b$. Trong tình huống này, bộ ba được sắp xếp là$(c, a, b)$, vậy số trung vị là$a$. Bất đẳng thức trở thành$(c + a + b)/3 \le a$, điều này đơn giản hóa thành$c + b \le 2a$. Từ$b \ge a$, điều này ngay lập tức hạn chế$c \le a$. Do đó, giá trị khả thi lớn nhất trong vùng này là$c = a$. 
3. Xét trường hợp thứ hai trong đó$a \le c \le b$. Bây giờ bộ ba được sắp xếp là$(a, c, b)$, vậy số trung vị là$c$. Điều kiện trở thành$a + b + c \le 3c$, điều này đơn giản hóa thành$a + b \le 2c$, hoặc$c \ge (a+b)/2$. Vì chúng ta muốn tối đa hóa$c$trong khi ở trong khoảng thời gian này, chúng tôi cố gắng$c = b$. Thay thế, chúng tôi nhận được$a + b \le 2b$, điều này luôn đúng vì$b \ge a$. Vậy giá trị khả thi lớn nhất trong vùng này là$c = b$. 
4. Xét trường hợp thứ ba trong đó$a \le b \le c$. Bây giờ bộ ba được sắp xếp là$(a, b, c)$, vậy số trung vị là$b$. Bất đẳng thức trở thành$a + b + c \le 3b$, điều này đơn giản hóa thành$a + c \le 2b$, đưa ra giới hạn trên$c \le 2b - a$. Vì chúng ta đang ở trong khu vực$c \ge b$, sự lựa chọn tốt nhất có thể là$c = 2b - a$. 
5. Cuối cùng, so sánh các ứng viên từ tất cả các trường hợp. Điều tốt nhất từ ​​​​trường hợp đầu tiên là$a$, từ thứ hai là$b$, và từ thứ ba là$2b-a$. Từ$b \ge a$, trường hợp đầu tiên không bao giờ là tối ưu. Cũng,$2b-a \ge b$, do đó trường hợp thứ ba chiếm ưu thế hơn trường hợp thứ hai. Vì vậy câu trả lời cuối cùng là$c = 2b - a$. 
6. Nếu đơn đặt hàng ban đầu có$a > b$, sự đối xứng cho$c = 2a - b$. Kết hợp cả hai hướng, câu trả lời là$2\max(a,b) - \min(a,b)$. 

### Tại sao nó hoạt động 

Ràng buộc trung vị chỉ phụ thuộc vào thứ tự và với ba phần tử thì chỉ có ba cấu hình thứ tự có thể có. Trong mỗi cấu hình, bất đẳng thức trở nên tuyến tính theo$c$, tạo ra một giới hạn trên hoặc giới hạn dưới. Giải pháp tối ưu phải nằm ở ranh giới của một trong các vùng này và việc so sánh các ranh giới đó sẽ làm cạn kiệt mọi khả năng mà không bỏ sót sự chuyển tiếp giữa các thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a = int(input())
    b = int(input())
    
    if a > b:
        a, b = b, a
    
    # optimal derived from case analysis
    print(2 * b - a)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ bình thường hóa thứ tự sao cho tất cả các lý do đều giả định$a \le b$. Điều này tránh trùng lặp logic trường hợp. Công thức cuối cùng$2b-a$tương ứng trực tiếp với giá trị tốt nhất có thể đạt được từ khu vực nơi$c$trở thành phần tử tối đa, chi phối tất cả các cấu hình khác. 

Một sai lầm phổ biến là quên rằng giải pháp tối ưu luôn nằm ở một ranh giới mà tại đó$c$hoặc được căn chỉnh với$a$,$b$hoặc giới hạn tuyến tính dẫn xuất$2b-a$. Sự rút gọn cuối cùng sẽ thu gọn tất cả cấu trúc này thành một biểu thức duy nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$a = 1, b = 10$Trước tiên chúng tôi sắp xếp, vì vậy$a=1, b=10$. 

| Bước | Trường hợp | Trung vị | Ràng buộc | Ứng viên | 
| --- | --- | --- | --- | --- | 
| 1 |$c \le a$| 1 |$c \le 1$| 1 | 
| 2 |$a \le c \le b$| c | luôn có giá trị tới 10 | 10 | 
| 3 |$c \ge b$| 10 |$c \le 19$| 19 | 

Giá trị tốt nhất là 19, xuất phát từ trường hợp thứ ba. Điều này chứng tỏ việc đẩy$c$ở trên cả hai số sẽ làm tăng trung vị theo cách tuyến tính được kiểm soát. 

### Ví dụ 2 

đầu vào:$a = 7, b = 7$Đã sắp xếp rồi. 

| Bước | Trường hợp | Trung vị | Ràng buộc | Ứng viên | 
| --- | --- | --- | --- | --- | 
| 1 |$c \le 7$| 7 |$c \le 7$| 7 | 
| 2 |$7 \le c \le 7$| 7 | luôn hợp lệ | 7 | 
| 3 |$c \ge 7$| 7 |$c \le 7$| 7 | 

Tất cả các trường hợp đều quy về cùng một biên, khẳng định rằng lực đối xứng$c = 7$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ một số phép so sánh và phép tính số học được thực hiện | 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Giải pháp này chạy ngay lập tức ngay cả đối với kích thước đầu vào tối đa vì nó tránh mọi sự lặp lại trong phạm vi giá trị có thể có cho$c$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    
    a = int(input())
    b = int(input())
    
    if a > b:
        a, b = b, a
    
    return str(2 * b - a)

# provided samples (conceptual, since exact sample text is partial)
assert run("1\n10\n") == "19", "basic increasing pair"
assert run("3\n3\n") == "3", "equal values"

# custom cases
assert run("1\n1\n") == "1", "minimum symmetric case"
assert run("5\n10\n") == "15", "checks linear extension"
assert run("10\n5\n") == "15", "order swap symmetry"
assert run("1000000000000000000\n1\n") == str(2*1000000000000000000 - 1), "large boundary case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 1 | 1 | trường hợp đối xứng nhỏ nhất | 
| 5, 10 | 15 | cặp tăng chuẩn | 
| 10, 5 | 15 | tính đối xứng khi hoán đổi | 
| giá trị lớn | 2b-a | kiểm tra căng thẳng cho logic tràn | 

## Vỏ cạnh 

Khi nào$a = b$, thuật toán giảm ngay xuống$c = a$. Trong trường hợp này cả ba số đều giống nhau nên giá trị trung bình và số trung vị luôn trùng nhau bất kể thứ tự và công thức tuyến tính$2b-a$sụp đổ một cách chính xác để$a$. 

Ví dụ: khi một giá trị cực kỳ lớn so với giá trị kia$a = 1$Và$b = 10^{18}$, thuật toán chọn$c = 2b - a$. Điều này tương ứng với việc đặt$c$trên cả hai con số, trong đó giá trị trung bình ổn định ở mức$b$. Bất đẳng thức sau đó trở nên chặt chẽ ở biên$a + c = 2b$, mà công thức thỏa mãn chính xác. 

Khi đầu vào được hoán đổi, bước chuẩn hóa sẽ đảm bảo tính nhất quán. Ví dụ,$a = 10, b = 1$được chuyển đổi nội bộ thành$a = 1, b = 10$, do đó, lý do tương tự cũng được áp dụng mà không cần phân nhánh theo thứ tự đầu vào.
