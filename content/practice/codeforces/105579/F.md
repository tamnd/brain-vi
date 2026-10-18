---
title: "CF 105579F - Chuỗi số học tối ưu"
description: "Chúng ta bắt đầu từ số 1 và được cung cấp nhiều tập hợp các phép tính số học nhỏ. Mỗi phép toán là một cặp bao gồm một trong bốn ký hiệu và một chữ số từ 1 đến 9."
date: "2026-06-22T06:15:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105579
codeforces_index: "F"
codeforces_contest_name: "Udmurtia High School Programming Contest (Qualification for VKOSHP 2012)"
rating: 0
weight: 105579
solve_time_s: 50
verified: true
draft: false
---

[CF 105579F - Chuỗi số học tối ưu](https://codeforces.com/problemset/problem/105579/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu từ số 1 và được cung cấp nhiều tập hợp các phép tính số học nhỏ. Mỗi thao tác là một cặp bao gồm một trong bốn ký hiệu và một chữ số từ 1 đến 9. Chúng ta phải sắp xếp các thao tác này theo thứ tự nào đó và áp dụng chúng một cách tuần tự cho giá trị hiện tại, tạo ra kết quả cuối cùng. Nhiệm vụ là chọn thứ tự làm cho giá trị cuối cùng này càng lớn càng tốt và xuất ra danh sách được sắp xếp lại. 

Mỗi thao tác biến đổi giá trị hiện tại theo một cách rất trực tiếp: phép cộng tăng giá trị đó lên một lượng cố định, phép trừ làm giảm giá trị đó một lượng cố định, phép nhân chia tỷ lệ cho giá trị đó và phép chia làm giảm giá trị đó. Khó khăn chính là hiệu quả của một thao tác phụ thuộc rất nhiều vào thời điểm nó được áp dụng, bởi vì các thao tác sau đó sẽ tác động lên giá trị đã được sửa đổi. 

Ràng buộc n 100 có nghĩa là chúng ta được phép xem xét các chiến lược dựa trên lý luận hoặc sắp xếp O(n²) một cách thoải mái. Bất cứ điều gì theo cấp số nhân trên các hoán vị đều không thể xảy ra vì n! tăng quá nhanh ngay cả với n = 10. Cấu trúc gợi ý rằng giải pháp là sắp xếp thứ tự các loại phép toán thay vì thử tất cả các hoán vị. 

Một vấn đề tế nhị phát sinh từ sự tương tác giữa các hoạt động chia tỷ lệ và phụ gia. Ví dụ: một phép cộng nhỏ được áp dụng sớm có thể trở thành lớn sau các phép nhân sau, trong khi phép cộng tương tự được áp dụng muộn chỉ đóng góp giá trị thô của nó. Tương tự, phép trừ được áp dụng sớm có thể bị khuếch đại thành tổn thất lớn hơn nhiều nếu được nhân lên sau đó. 

Các trường hợp cạnh bộc lộ các chiến lược ngây thơ bao gồm các hoạt động trộn lẫn các loại khác nhau mà không xem xét ảnh hưởng vị trí của chúng. Ví dụ, nếu chúng ta tham lam đặt tất cả các phép cộng trước rồi đến các phép nhân, chúng ta sẽ bỏ lỡ một thực tế là phép nhân sẽ khuếch đại các phép cộng, vì vậy thứ tự ngược lại sẽ tốt hơn. Một trường hợp thất bại khác là xử lý tất cả các hoạt động độc lập với trật tự và bị hỏng ngay lập tức. Ví dụ, với`+ 5`Và`* 2`, áp dụng`+ 5`đầu tiên mang lại 7 rồi 14, trong khi đảo ngược mang lại 2 rồi 7, một kết quả nhỏ hơn. 

Trường hợp góc cuối cùng là phép chia. Vì phép chia làm giảm giá trị nên việc đặt nó sớm sẽ vĩnh viễn thu nhỏ mọi thứ theo sau. Một cách tiếp cận ngây thơ kết hợp phép chia một cách tùy tiện có thể dễ dàng làm mất đi những lợi ích lớn được tạo ra bởi các phép nhân hoặc phép cộng trước đó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các hoán vị của n thẻ, mô phỏng quy trình cho từng thứ tự và theo dõi kết quả tối đa. Điều này đúng vì nó đánh giá trực tiếp mọi chuỗi có thể. Tuy nhiên, nó đòi hỏi n! đánh giá và mỗi lần đánh giá tốn O(n), dẫn đến thời gian O(n · n!), điều này trở nên không khả thi ngay cả khi n = 10. 

Quan sát quan trọng là chúng ta thực sự không cần phải hoán đổi các phép toán một cách tùy ý. Mỗi thao tác sẽ chia tỷ lệ giá trị hiện tại hoặc dịch chuyển giá trị đó và việc chia tỷ lệ có tác động cấu trúc mạnh hơn vì nó thay đổi độ lớn của mọi thứ tiếp theo. Điều này có nghĩa là các phép toán phải được nhóm theo tác động định tính của chúng: số nhân phải ở đầu tiên để khuếch đại mọi thứ, các phép tính cộng phải ở giữa và các ước số phải ở cuối để tránh làm giảm mức tăng trước đó. 

Trong các số nhân, các yếu tố lớn hơn sẽ có lợi sớm hơn vì chúng khuếch đại các số nhân tiếp theo cũng như tất cả các tác động cộng gộp theo sau. Trong các phép chia, thứ tự không quan trọng vì phép chia tổng hợp theo cấp số nhân, nhưng tất cả chúng phải được trì hoãn càng nhiều càng tốt. Các phép cộng và phép trừ tốt nhất nên được đặt sau các số nhân để chúng đã được khuếch đại và trước các phép chia để chúng không bị giảm. 

Điều này làm giảm vấn đề sắp xếp các hoạt động thành một số ít danh mục và sắp xếp chúng một cách thích hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng thứ tự tối ưu bằng cách khai thác cách mỗi thao tác tương tác với các hiệu ứng chia tỷ lệ. 

1. Chia tất cả các thẻ thành bốn nhóm: nhân, chia, cộng và trừ. Điều này là cần thiết vì mỗi loại ảnh hưởng đến giá trị theo một cách cơ bản khác nhau và việc trộn chúng một cách tùy tiện sẽ làm mất cấu trúc. 
2. Sắp xếp các thẻ nhân theo giá trị số theo thứ tự giảm dần. Các số nhân lớn hơn nên được áp dụng sớm hơn vì chúng khuếch đại mọi thứ theo sau chúng, bao gồm các số nhân khác và tất cả các đóng góp cộng. 
3. Đặt tất cả các thẻ nhân ở đầu dãy. Tại thời điểm này, chúng tôi đã tối đa hóa hiệu ứng chia tỷ lệ trước khi đưa ra bất kỳ biến dạng phụ gia nào. 
4. Thu thập tất cả các thẻ cộng và thẻ trừ cùng nhau cho phần giữa. Ở giai đoạn này, sẽ không có phép nhân nào xảy ra sau chúng, do đó thứ tự bên trong của chúng không ảnh hưởng đến cách chúng được chia tỷ lệ sau này theo số nhân. 
5. Nối tất cả các thẻ chia vào cuối. Phép chia làm giảm toàn bộ kết quả tích lũy, do đó việc trì hoãn nó sẽ bảo toàn tất cả giá trị thu được trước đó càng lâu càng tốt. 
6. Xuất ra chuỗi nối: các phép nhân được sắp xếp, sau đó là phép cộng và phép trừ, rồi phép chia. 

Quyết định mang tính cấu trúc quan trọng là tách các hoạt động mở rộng quy mô khỏi các hoạt động bổ sung. Sau khi các số nhân được cố định ở phía trước, tất cả các hiệu ứng cộng sẽ trở thành đóng góp tuyến tính và khi các phép chia được cố định ở cuối, chúng sẽ trở thành tỷ lệ thống nhất cuối cùng. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi đặt các số nhân ở phía trước, các thao tác còn lại không bao giờ ảnh hưởng đến độ khuếch đại tương đối giữa các số nhân trước đó và sau đó, vì không có tỷ lệ nào được đưa ra sau khối đó. Tương tự, khi các phép chia được đặt ở cuối, chúng sẽ áp dụng hệ số thống nhất cho toàn bộ kết quả và không tương tác với thứ tự bên trong khối ở giữa.

Điều này có nghĩa là bất kỳ sự sắp xếp lại nào bên trong khối phụ gia đều không thể cải thiện hoặc làm xấu đi mức độ khuếch đại của nó, vì mức khuếch đại đã được xác định đầy đủ bởi khối nhân. Logic tương tự cũng áp dụng cho các phép chia, vốn chỉ áp dụng sự thu gọn toàn cục cuối cùng. Nơi duy nhất có vấn đề về thứ tự là ở số nhân, trong đó việc sắp xếp theo hệ số giảm dần đảm bảo sự lan truyền tăng trưởng tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
mul = []
add = []
sub = []
div = []

for _ in range(n):
    op, val = input().split()
    val = int(val)
    if op == '*':
        mul.append((val, op, val))
    elif op == '/':
        div.append((val, op, val))
    elif op == '+':
        add.append((val, op, val))
    else:
        sub.append((val, op, val))

# sort multiplications descending
mul.sort(key=lambda x: x[0], reverse=True)

res = []

for _, op, val in mul:
    res.append((op, val))
for _, op, val in add:
    res.append((op, val))
for _, op, val in sub:
    res.append((op, val))
for _, op, val in div:
    res.append((op, val))

for op, val in res:
    print(op, val)
```Việc thực hiện phản ánh trực tiếp sự phân rã lý thuyết. Nhóm nhân được sắp xếp theo cường độ và được đặt đầu tiên vì nó xác định cấu trúc khuếch đại của toàn bộ biểu thức. Các phép cộng và phép trừ sau đó được thêm vào mà không cần sắp xếp bên trong vì thứ tự tương đối của chúng không thay đổi kiểu chia tỷ lệ cuối cùng sau khi số nhân được cố định. Các bộ phận được xếp cuối cùng vì chúng thống nhất giảm kết quả cuối cùng bất kể sự sắp xếp nội bộ. 

Một cạm bẫy triển khai phổ biến là suy nghĩ quá nhiều về thứ tự bên trong khối phụ gia. Vì không còn số nhân nào tồn tại sau giai đoạn đó nên việc hoán đổi hai phép cộng hoặc hai phép trừ không làm thay đổi kết quả cuối cùng. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu: 

| Bước | Số nhân | Khối Thêm/Sub | Phòng | 
| --- | --- | --- | --- | 
| Ban đầu | 2, 4 | +1, +5, -1, -2 | /2, /3 | 
| Sau khi sắp xếp | 4, 2 | +1, +5, -1, -2 | /2, /3 | 

Sau khi áp dụng thuật toán, đầu ra trước tiên sẽ trở thành số nhân, tiếp theo là các phép toán cộng và cuối cùng là phép chia. Hành vi chính được minh họa ở đây là`* 4`được đặt trước`* 2`, đảm bảo rằng tất cả các hiệu ứng phụ tiếp theo được khuếch đại tối đa. 

Bây giờ hãy xem xét một trường hợp tùy chỉnh: 

đầu vào:```
* 3
+ 10
* 2
- 5
/ 2
```| Bước | Số nhân | Khối Thêm/Sub | Phòng | 
| --- | --- | --- | --- | 
| Ban đầu | 3, 2 | +10, -5 | /2 | 
| Đã sắp xếp | 3, 2 | +10, -5 | /2 | 

Đây là kết quả của`+10`được hưởng lợi từ cả hai số nhân, trong khi`-5`được đặt an toàn mà không bị khuếch đại một cách không cần thiết. Phép chia được áp dụng sau cùng, chỉ thu hẹp kết quả cuối cùng sau khi tất cả lợi nhuận đã được tích lũy. 

Những ví dụ này cho thấy thuật toán thực thi một cấu trúc khuếch đại nhất quán trong đó tất cả sự tăng trưởng được tối đa hóa trước khi áp dụng bất kỳ thao tác thu nhỏ nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Chỉ sắp xếp thẻ nhân chiếm ưu thế | 
| Không gian | O(n) | Lưu trữ cho các hoạt động được nhóm | 

Các ràng buộc cho phép thực hiện tối đa 100 thao tác, do đó việc sắp xếp không đáng kể về mặt hiệu suất. Giải pháp này chạy thoải mái trong cả giới hạn thời gian và bộ nhớ vì tất cả các hoạt động khác đều là quét tuyến tính và ghép nối đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    n = int(input())
    mul, add, sub, div = [], [], [], []

    for _ in range(n):
        op, val = input().split()
        val = int(val)
        if op == '*':
            mul.append((val, op, val))
        elif op == '/':
            div.append((val, op, val))
        elif op == '+':
            add.append((val, op, val))
        else:
            sub.append((val, op, val))

    mul.sort(reverse=True)

    for _, op, val in mul:
        print(op, val)
    for _, op, val in add:
        print(op, val)
    for _, op, val in sub:
        print(op, val)
    for _, op, val in div:
        print(op, val)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# sample-like test
assert run("""8
+ 1
+ 5
- 1
- 2
* 2
* 4
/ 2
/ 3
""") == """* 4
* 2
+ 1
+ 5
- 1
- 2
/ 2
/ 3"""

# minimum case
assert run("""1
+ 9
""") == """+ 9"""

# only multipliers
assert run("""3
* 1
* 5
* 2
""") == """* 5
* 2
* 1"""

# divisions only
assert run("""2
/ 2
/ 3
""") == """/ 2
/ 3"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | nhóm được sắp xếp lại | tính đúng đắn của cấu trúc đầy đủ | 
| hoạt động đơn lẻ | cùng một lựa chọn | xử lý ranh giới tối thiểu | 
| chỉ * | sắp xếp giảm dần | thứ tự nhân | 
| chỉ / | thứ tự không thay đổi | phân chia trung lập | 

## Vỏ cạnh 

Một đầu vào hoạt động duy nhất là ranh giới đơn giản nhất. Thuật toán đặt nó vào danh mục của nó và xuất ra trực tiếp, điều này duy trì tính chính xác vì không tồn tại quyết định thứ tự tương đối nào. 

Khi tất cả các phép toán đều là phép nhân, việc sắp xếp giảm dần đảm bảo rằng các số nhân lớn hơn xuất hiện đầu tiên, tối đa hóa hiệu ứng xếp tầng trên các số nhân tiếp theo. Ví dụ,`* 5, * 2, * 1`chính xác đảm bảo khuếch đại sớm. 

Khi chỉ có phép chia tồn tại, mọi thứ tự đều hợp lệ vì tất cả các phép chia đều giao hoán theo cấp số nhân. Thuật toán vẫn đưa ra một thứ tự nhất quán, đảm bảo tính chính xác. 

Khi các phép cộng và phép trừ được trộn lẫn mà không có số nhân, thứ tự của chúng không quan trọng vì không có sự khác biệt về tỷ lệ nào được đưa ra sau bước nhóm. Việc nhóm thuật toán đảm bảo chúng vẫn ở trong một khối ổn định, tạo ra sự sắp xếp tối ưu hợp lệ.
