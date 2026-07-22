---
title: "CF 103665A - \u0420\u0435\u0448\u0435\u043d\u0438\u0435 \u0437\u0430\u0434\u0430\u0447"
description: "Hai thí sinh đang theo dõi xem họ giải quyết được bao nhiêu vấn đề lập trình theo thời gian. Mỗi người trong số họ đều đã có sẵn một số bài toán đã được giải và sau đó họ tiếp tục giải với tốc độ không đổi hàng ngày."
date: "2026-07-02T21:43:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103665
codeforces_index: "A"
codeforces_contest_name: "\u0422\u0443\u0440\u043d\u0438\u0440 \u0410\u0440\u0445\u0438\u043c\u0435\u0434\u0430 2018"
rating: 0
weight: 103665
solve_time_s: 50
verified: true
draft: false
---

[CF 103665A - \u0420\u0435\u0448\u0435\u043d\u0438\u0435 \u0437\u0430\u0434\u0430\u0447](https://codeforces.com/problemset/problem/103665/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hai thí sinh đang theo dõi xem họ giải quyết được bao nhiêu vấn đề lập trình theo thời gian. Mỗi người trong số họ đều đã có sẵn một số bài toán đã được giải và sau đó họ tiếp tục giải với tốc độ không đổi hàng ngày. Nhiệm vụ là xác định, sau một số ngày cố định, ai sẽ có tổng số điểm lớn hơn hoặc liệu họ có hòa hay không. 

Cụ thể, một người bắt đầu với số đếm ban đầu`a`và tăng nó lên bằng`x`mỗi ngày cho`c`ngày. Cái khác bắt đầu bằng`b`và tăng nó lên bằng`y`mỗi ngày trong cùng một số ngày. Chúng tôi được yêu cầu so sánh tổng số cuối cùng: 

giá trị đầu tiên trở thành`a + x * c`, và thứ hai trở thành`b + y * c`, và chúng tôi xuất ra cái nào lớn hơn. 

Các ràng buộc cực kỳ nhỏ, tất cả đầu vào đều nằm trong khoảng từ 1 đến 10. Điều này ngay lập tức loại bỏ mọi nhu cầu cân nhắc về hiệu suất ngoài số học thời gian không đổi. Ngay cả một cách tiếp cận tính toán lại tổng số trong một vòng lặp`c`ngày ở đây không đáng kể vì số lần lặp tối đa là 10. 

Vấn đề tinh tế duy nhất là đảm bảo rằng việc so sánh được thực hiện sau khi tích lũy chính xác cả hai cấp số tuyến tính. Một sai lầm điển hình là so sánh tỷ giá hàng ngày`x`Và`y`mà không xem xét các giá trị ban đầu hoặc vô tình nhân sai như`(a + x) * c`thay vì`a + x * c`. 

Không có trường hợp cạnh ẩn nào về mặt tràn hoặc kích thước đầu vào lớn, nhưng về mặt khái niệm, trường hợp ranh giới là khi cả hai tổng đều bằng nhau. Ví dụ, nếu`a = 3, x = 2, b = 5, y = 1, c = 2`, thì cả hai đều kết thúc ở số 7 và kết quả đầu ra phải là kết quả hòa. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực mô phỏng quá trình này từng ngày. Bắt đầu từ`a`Và`b`, chúng tôi liên tục thêm`x`Và`y`vì`c`lần lặp lại. Điều này đơn giản và phản ánh chính xác câu chuyện. Sau mỗi ngày, chúng tôi cập nhật cả hai quầy. Sau đó`c`ngày, chúng tôi so sánh kết quả. 

Điều này hoạt động chính xác vì nó trực tiếp mô hình hóa quy trình được mô tả. Tuy nhiên, mặc dù nó đã có hiệu quả đối với vấn đề này nhưng nó không phải là cách thể hiện rõ ràng nhất về cấu trúc. Quan sát quan trọng là cả hai chuỗi đều là cấp số cộng với kích thước bước không đổi. Thay vì mô phỏng tất cả các bước, chúng ta có thể tính giá trị cuối cùng ở dạng đóng bằng cách sử dụng một phép nhân duy nhất. 

Lý do đơn giản hóa này có tác dụng là vì phép cộng lặp đi lặp lại của một giá trị không đổi tương đương với phép nhân. Đang làm`x`đã thêm`c`thời gian là chính xác`x * c`, và tương tự cho`y`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(c) | O(1) | Đã chấp nhận | 
| Công thức trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán cả hai kết quả cuối cùng và so sánh chúng trực tiếp. 

1. Đọc năm số nguyên`a, x, b, y, c`. Chúng xác định điểm số ban đầu, tốc độ tăng trưởng hàng ngày và số ngày xảy ra tăng trưởng. 
2. Tính điểm cuối cùng của người thứ nhất là`a + x * c`. Điều này thể hiện sự bắt đầu từ`a`và thêm`x`một lần mỗi ngày cho`c`ngày. 
3. Tính điểm cuối cùng của người thứ hai là`b + y * c`, áp dụng logic tương tự. 
4. So sánh hai giá trị cuối cùng. Nếu số đầu tiên lớn hơn thì người đầu tiên thắng. Nếu số thứ hai lớn hơn thì người thứ hai thắng. Nếu không thì chúng bằng nhau. 
5. Xuất ra chuỗi tương ứng`"Feefa"`,`"Foofa"`, hoặc`"Draw"`. 

### Tại sao nó hoạt động 

Sự tiến triển của mỗi người là tuyến tính với mức tăng không đổi mỗi ngày. Tổng số sau`c`ngày chỉ phụ thuộc vào giá trị ban đầu và tổng các số gia giống hệt nhau. Vì phép cộng có tính chất kết hợp và giao hoán nên việc nhóm các số gia tăng hàng ngày thành một phép nhân không làm thay đổi kết quả. Do đó, việc so sánh tổng số cuối cùng tương đương với việc so sánh kết quả mô phỏng đầy đủ hàng ngày. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a = int(input())
x = int(input())
b = int(input())
y = int(input())
c = int(input())

first = a + x * c
second = b + y * c

if first > second:
    print("Feefa")
elif second > first:
    print("Foofa")
else:
    print("Draw")
```Mã này tuân theo cách tiếp cận công thức trực tiếp. Mỗi đầu vào được đọc riêng theo yêu cầu của định dạng câu lệnh. Phép nhân được thực hiện một lần cho mỗi người tham gia, đảm bảo thời gian tính toán không đổi. 

Điểm tinh tế duy nhất là đảm bảo quyền ưu tiên và nhóm toán tử chính xác. Viết`a + x * c`đúng vì phép nhân được đánh giá trước phép cộng, nhưng có thể thêm dấu ngoặc đơn để rõ ràng mà không làm thay đổi ý nghĩa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2
5
1
2
```Chúng tôi theo dõi cả hai giá trị: 

| Bước | Giá trị Feefa | Giá trị Foofa | 
| --- | --- | --- | 
| ban đầu | 3 | 5 | 
| sau 2 ngày | 3 + 2·2 = 7 | 5 + 1·2 = 7 | 

Cả hai tổng đều bằng nhau nên kết quả đầu ra là`"Draw"`. 

Ví dụ này cho thấy các giá trị ban đầu và tốc độ tăng trưởng khác nhau có thể cân bằng chính xác sau một số bước nhỏ. 

### Ví dụ 2 

đầu vào:```
1
3
2
1
4
```| Bước | Giá trị Feefa | Giá trị Foofa | 
| --- | --- | --- | 
| ban đầu | 1 | 2 | 
| sau 4 ngày | 1 + 3·4 = 13 | 2 + 1·4 = 6 | 

Feefa rõ ràng kết thúc với tổng số lớn hơn, vì vậy kết quả đầu ra đúng là`"Feefa"`. 

Điều này chứng tỏ rằng lãi suất hàng ngày cao hơn sẽ chiếm ưu thế ngay cả khi giá trị ban đầu nhỏ hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học cố định được thực hiện | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Các ràng buộc đủ nhỏ để thậm chí mô phỏng lặp lại cũng sẽ nhanh, nhưng công thức thời gian không đổi là cách thể hiện trực tiếp và rõ ràng nhất của quy trình. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    a = int(input())
    x = int(input())
    b = int(input())
    y = int(input())
    c = int(input())

    first = a + x * c
    second = b + y * c

    if first > second:
        return "Feefa"
    elif second > first:
        return "Foofa"
    else:
        return "Draw"

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided samples
assert run("3\n2\n5\n1\n2\n") == "Draw"
assert run("1\n3\n2\n1\n4\n") == "Feefa"

# custom cases
assert run("1\n1\n1\n1\n10\n") == "Draw", "all equal growth"
assert run("10\n1\n1\n1\n1\n") == "Feefa", "initial dominance"
assert run("1\n1\n10\n1\n1\n") == "Foofa", "single day comparison"
assert run("2\n2\n2\n3\n5\n") == "Foofa", "different rates over multiple days"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều tăng trưởng như nhau | Vẽ | thông số đối xứng | 
| sự thống trị ban đầu | Phífa | tác động giá trị ban đầu | 
| so sánh một ngày | Foofa | sự cân bằng giữa tỷ giá và cơ sở | 
| mức giá khác nhau trong nhiều ngày | Foofa | hiệu ứng tăng trưởng tích lũy | 

## Vỏ cạnh 

Trường hợp quan trọng là khi cả hai người tham gia đều kết thúc với tổng số điểm như nhau. Ví dụ,`a = 3, x = 2, b = 5, y = 1, c = 2`dẫn đến cả hai đều kết thúc ở số 7. Thuật toán tính toán cả hai biểu thức một cách độc lập và xác định chính xác đẳng thức, tạo ra`"Draw"`. 

Một trường hợp tinh vi khác là khi một người tham gia xuất phát muộn hơn nhưng có tỷ lệ hàng ngày cao hơn. Ví dụ,`a = 1, x = 3, b = 2, y = 1, c = 4`kết quả là giá trị cuối cùng`13`Và`6`. Công thức nắm bắt chính xác lợi thế tích lũy vì nó tổng hợp tất cả các khoản tăng hàng ngày thay vì chỉ so sánh các giá trị ban đầu. 

Mặc dù quy mô vấn đề rất nhỏ nhưng những trường hợp này củng cố rằng giải pháp phải so sánh sự tăng trưởng tuyến tính đầy đủ chứ không phải các ảnh chụp nhanh trung gian hoặc mỗi ngày.
