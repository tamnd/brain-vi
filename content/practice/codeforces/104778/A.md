---
title: "CF 104778A - \u0422\u0440\u0435\u0443\u0433\u043e\u043b\u044c\u043d\u0438\u043a"
description: "Chúng ta được cho ba số nguyên dương biểu thị độ dài của ba đoạn thẳng. Trong một lần di chuyển, chúng ta được phép chọn bất kỳ một đoạn nào và thay đổi độ dài của nó đúng một đơn vị, tăng hoặc giảm nó, miễn là đoạn đó vẫn dương sau khi thay đổi."
date: "2026-06-28T15:26:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104778
codeforces_index: "A"
codeforces_contest_name: "2023-2024 \u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e, \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 (\u0412\u041a\u041e\u0428\u041f 23, \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u0438\u0439 \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f)"
rating: 0
weight: 104778
solve_time_s: 40
verified: true
draft: false
---

[CF 104778A - \u0422\u0440\u0435\u0443\u0433\u043e\u043b\u044c\u043d\u0438\u043a](https://codeforces.com/problemset/problem/104778/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho ba số nguyên dương biểu thị độ dài của ba đoạn thẳng. Trong một lần di chuyển, chúng ta được phép chọn bất kỳ một đoạn nào và thay đổi độ dài của nó đúng một đơn vị, tăng hoặc giảm nó, miễn là đoạn đó vẫn dương sau khi thay đổi. 

Mục tiêu là biến ba độ dài này thành một bộ ba có thể tạo thành một tam giác không suy biến. Điều đó có nghĩa là ba độ dài kết quả phải thỏa mãn tất cả các bất đẳng thức tam giác một cách chặt chẽ, do đó mỗi cạnh phải nhỏ hơn tổng của hai cạnh còn lại. Chúng tôi được yêu cầu số lượng thay đổi đơn vị tối thiểu như vậy được yêu cầu. 

Mỗi thao tác ảnh hưởng đến chính xác một phân đoạn ±1, do đó chi phí chúng tôi phải trả chính xác bằng tổng khoảng cách L1 giữa bộ ba ban đầu và bộ ba được chọn cuối cùng. 

Các ràng buộc lên tới 10^6 cho mỗi giá trị, do đó, bất kỳ giải pháp nào cố gắng khám phá tất cả các bộ ba cuối cùng có thể có hoặc mô phỏng các thay đổi từng bước đều không khả thi ngay lập tức. Một BFS ngây thơ về các trạng thái hoặc liệt kê lực lượng vũ phu về độ dài cuối cùng có thể sẽ liên quan đến không gian tìm kiếm theo thứ tự hàng triệu trên mỗi tọa độ, trở thành ít nhất 10^18 kết hợp, điều này hoàn toàn không thể. 

Một số tình huống nguy hiểm cần được loại bỏ sớm. 

Nếu ba đoạn thẳng đã thỏa mãn bất đẳng thức tam giác, ví dụ 3, 4, 5 thì câu trả lời là 0. Một giải pháp bất cẩn cố gắng “điều chỉnh bằng mọi cách” sẽ đưa ra những thay đổi không cần thiết một cách không chính xác. 

Nếu hai cạnh đã có tổng chính xác bằng cạnh thứ ba, ví dụ 1, 2, 3, thì hình dạng đó bị suy biến. Người ta phải phá vỡ sự bình đẳng một cách nghiêm ngặt, nghĩa là cần phải sửa đổi ít nhất một đơn vị. Một giải pháp chỉ kiểm tra các bất đẳng thức không nghiêm ngặt sẽ chấp nhận trường hợp này là hợp lệ một cách không chính xác. 

Một trường hợp tinh vi khác là khi một bên lớn hơn nhiều so với tổng của hai bên còn lại, chẳng hạn như 1, 1, 100. Để khắc phục điều này một cách tối ưu đòi hỏi phải hiểu rằng cả hai bên nhỏ đều có thể tăng lên hoặc bên lớn có thể giảm đi và chiến lược tối ưu không thể rõ ràng nếu không có cấu trúc. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo là xem xét mọi bộ ba số nguyên dương cuối cùng có thể có và tính chi phí chuyển đổi bộ ba ban đầu thành bộ ba đó, đồng thời kiểm tra xem nó có tạo thành một tam giác hợp lệ hay không. Đối với mỗi bộ ba ứng cử viên, chi phí là |a − a'| + |b − b'| + |c − c'|. Về nguyên tắc, điều này đúng vì chúng tôi trực tiếp đánh giá tất cả các mục tiêu có thể đạt được. 

Vấn đề là quy mô. Mỗi bên có thể di chuyển từ 1 lên đến gần tối đa (a, b, c) cộng hoặc trừ sự mất cân bằng trong các trường hợp cực đoan, điều này đã mang lại khoảng 10^6 khả năng cho mỗi tọa độ. Tổng không gian trở thành hình khối và ngay cả khi chúng ta giới hạn ở một cửa sổ hợp lý, số lượng ứng cử viên vẫn vượt xa những gì có thể liệt kê kịp thời. 

Nhận xét quan trọng là điều kiện của tam giác cực kỳ đơn giản khi xem xét qua thứ tự. Nếu chúng ta sắp xếp các cạnh cuối cùng là x ≤ y ≤ z, điều kiện sẽ trở thành x + y > z. Bất đẳng thức này đơn điệu theo một cách hữu ích: nếu bộ ba vi phạm nó, cách khắc phục duy nhất là tăng x hoặc y hoặc giảm z và làm như vậy theo các bước đơn vị có nghĩa là chi phí hành xử tuyến tính đối với mức độ chúng ta cần để “thu hẹp khoảng cách” z − (x + y). 

Điều này gợi ý rằng chúng ta không nên tìm kiếm trên tất cả các bộ ba. Thay vào đó, chúng tôi cố gắng sửa một cấu trúc: chúng tôi sẽ kết thúc ở cấu hình trong đó hai số không thay đổi hoặc được điều chỉnh tối thiểu và số thứ ba được điều chỉnh vừa đủ để thỏa mãn bất đẳng thức. Vì chi phí là tuyến tính theo lượng chuyển động nên giải pháp tối ưu luôn tương ứng với việc đẩy hệ thống đến đúng biên x + y = z + 1 (số nguyên nhỏ nhất thỏa mãn bất đẳng thức nghiêm ngặt), bởi vì bất kỳ sự điều chỉnh nào thêm nữa chỉ làm tăng chi phí một cách không cần thiết.

Vì vậy, vấn đề giảm xuống còn việc kiểm tra xem chúng ta phải điều chỉnh mỗi bên bao nhiêu để đảm bảo rằng sau khi sắp xếp, bên lớn nhất hoàn toàn nhỏ hơn tổng của hai bên còn lại và thực hiện theo hướng rẻ nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^3) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Sắp xếp ba độ dài 

Chúng ta sắp xếp lại các giá trị sao cho a `b `c. Điều này tách biệt ràng buộc có ý nghĩa duy nhất, đó là liệu c có quá lớn so với a và b hay không. Việc sắp xếp đảm bảo chúng ta không cần phải xem xét nhiều hoán vị bất đẳng thức tam giác một cách riêng biệt. 

### 2. Kiểm tra xem đã hợp lệ chưa 

Nếu a + b > c thì bộ ba đã tạo thành một tam giác chặt. Không cần thực hiện thao tác nào, vì vậy câu trả lời là 0. Bước này cho thấy thực tế là bất kỳ sửa đổi nào nữa sẽ chỉ làm tăng chi phí. 

### 3. Tính số tiền thâm hụt 

Nếu a + b ≤ c thì điều kiện tam giác không đạt. Mức độ sai sót chính xác là mức c vượt quá mức tối đa cho phép. Chúng tôi xác định một giá trị khoảng cách: 

khoảng cách = c − (a + b) + 1 

Điều này thể hiện mức độ chúng ta cần giảm c hoặc tăng a + b để bất đẳng thức nghiêm ngặt trở thành đúng. 

+1 xuất hiện vì chúng ta cần a + b phải lớn hơn c, không bằng. 

### 4. Chuyển khoảng trống vào hoạt động 

Mỗi thao tác thay đổi 1 bên nên mỗi đơn vị cải thiện trong a + b − c tương ứng với đúng một nước đi. Tăng a hoặc b sẽ làm tăng tổng, giảm c sẽ làm giảm ngưỡng và cả hai đều tốn kém như nhau tính theo đơn vị. Do đó, số lượng hoạt động tối thiểu chính xác là khoảng cách. 

### 5. Kết quả đầu ra 

Trả về 0 hoặc khoảng cách tùy thuộc vào điều kiện tam giác đã được giữ hay chưa. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, tính khả thi của một tam giác chỉ phụ thuộc vào một bất đẳng thức tuyến tính duy nhất. Bất kỳ chuỗi thao tác nào cũng thay đổi biểu thức a + b − c nhiều nhất là 1 mỗi lần di chuyển và mọi trạng thái cuối cùng hợp lệ đều yêu cầu biểu thức này ít nhất là 1. Do đó, số lần di chuyển tối thiểu là chi phí tối thiểu để đẩy biểu thức này từ giá trị ban đầu của nó lên đến 1, chính xác là max(0, c − a − b + 1). Không có sự sắp xếp điều chỉnh thay thế nào có thể cải thiện chi phí vì mỗi bước đi đều đóng góp chính xác một đơn vị cải tiến nhằm thỏa mãn sự bất bình đẳng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b, c = map(int, input().split())
a, b, c = sorted([a, b, c])

if a + b > c:
    print(0)
else:
    print(c - a - b + 1)
```Giải pháp bắt đầu bằng cách đọc ba độ dài đoạn và sắp xếp chúng sao cho giá trị lớn nhất được tách ra. Điều này cho phép chúng ta giảm điều kiện tam giác xuống một lần kiểm tra duy nhất. 

Điều kiện trực tiếp kiểm tra xem bất đẳng thức nghiêm ngặt có đúng hay không. Nếu có thì không cần sửa đổi. 

Ngược lại, chênh lệch c - (a + b) + 1 sẽ được tính toán. Biểu thức này là số lần điều chỉnh đơn vị chính xác cần thiết để đưa hệ thống đến ranh giới nơi tam giác trở nên hợp lệ. Không cần vòng lặp hoặc mô phỏng vì mỗi thao tác đóng góp chính xác một đơn vị cải tiến nhằm thỏa mãn sự bất bình đẳng. 

## Ví dụ đã hoạt động 

### Ví dụ 1: 3 2 6 

Sau khi sắp xếp ta có a = 2, b = 3, c = 6. 

| Bước | một | b | c | a + b | Tình trạng | Hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 3 | 6 | 5 | 5 6 | không hợp lệ | 
| Tính toán | - | - | - | - | khoảng cách = 6 − 5 + 1 = 2 | trả lời | 

Khoảng cách là 2, nghĩa là chúng tôi cần cải tiến hai đơn vị. Một phép biến đổi có thể xảy ra là giảm 6 xuống 5 và tăng 2 lên 3, đạt 3, 3, 5 thỏa mãn điều kiện tam giác. 

Dấu vết này cho thấy chúng ta không cần phải suy luận xem nên thay đổi bên nào; chỉ có vấn đề mất cân bằng ròng. 

### Ví dụ 2: 13 111 57 

Sau khi sắp xếp ta được a = 13, b = 57, c = 111. 

| Bước | một | b | c | a + b | Tình trạng | Hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 13 | 57 | 111 | 70 | 70 111 | không hợp lệ | 
| Tính toán | - | - | - | - | khoảng cách = 111 − 70 + 1 = 42 | trả lời | 

Thuật toán cho thấy sự mất cân bằng lớn chiếm ưu thế và câu trả lời chỉ phụ thuộc vào việc cạnh lớn nhất vượt quá tổng của hai cạnh còn lại bao xa. 

Những ví dụ này xác nhận rằng chỉ có sự bất bình đẳng được sắp xếp mới quan trọng chứ không phải sự phân bố cụ thể của các phép toán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Sắp xếp ba số và tính công thức | 
| Không gian | O(1) | Chỉ sử dụng một số lượng biến không đổi | 

Giải pháp là thời gian không đổi và phù hợp một cách tầm thường trong mọi ràng buộc. Ngay cả đối với giá trị đầu vào tối đa, việc tính toán chỉ thực hiện một số phép tính số học. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    a, b, c = map(int, sys.stdin.readline().split())
    a, b, c = sorted([a, b, c])
    if a + b > c:
        print(0)
    else:
        print(c - a - b + 1)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

# provided samples
assert run("3 2 6") == "2"
assert run("250 100 200") == "0"
assert run("13 111 57") == "42"

# custom cases
assert run("1 1 2") == "1"
assert run("1 1 3") == "2"
assert run("5 5 5") == "0"
assert run("1 2 1000000") == "999998"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 2 | 1 | sửa tam giác suy biến tối thiểu | 
| 1 1 3 | 2 | khoảng cách lớn hơn về vi phạm ranh giới | 
| 5 5 5 | 0 | trường hợp đẳng thức đã hợp lệ | 
| 1 2 1000000 | 999998 | tỉ lệ mất cân bằng cực độ | 

## Vỏ cạnh 

Đối với đầu vào 1 1 2, việc sắp xếp sẽ cho 1, 1, 2. Tổng của hai số nhỏ hơn bằng giá trị lớn nhất, do đó khoảng cách là 2 − 2 + 1 = 1. Thuật toán đưa ra 1, tương ứng với bất kỳ thay đổi đơn vị nào chẳng hạn như giảm 2 xuống 1 hoặc tăng một trong các đơn vị lên 2. 

Đối với 1 1 3, việc sắp xếp sẽ cho kết quả 1, 1, 3. Khoảng cách trở thành 3 − 2 + 1 = 2. Thuật toán xác định chính xác rằng cần có hai điều chỉnh vì một bước đi không thể khôi phục lại sự bất bình đẳng nghiêm ngặt. 

Đối với 5 5 5, bất đẳng thức đúng ngay lập tức vì 5 + 5 > 5, do đó thuật toán trả về 0 mà không cần nhập nhánh tính toán khoảng cách. Điều này xác nhận rằng không có sự điều chỉnh không cần thiết nào được đưa ra khi hình tam giác đã tồn tại.
