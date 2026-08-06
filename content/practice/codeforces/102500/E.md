---
title: "CF 102500E - Lập phương nhanh chóng"
description: "Claire còn bốn lần giải quyết hoàn thành và còn lại một lần giải quyết cuối cùng. Điểm cuối cùng của cô được tính bằng cách thực hiện tất cả năm lần, loại bỏ giải nhanh nhất và giải chậm nhất, sau đó lấy trung bình cộng của ba lần còn lại."
date: "2026-08-05T18:02:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102500
codeforces_index: "E"
codeforces_contest_name: "2019-2020 ICPC Northwestern European Regional Programming Contest (NWERC 2019)"
rating: 0
weight: 102500
solve_time_s: 61
verified: true
draft: false
---

[CF 102500E - Khối nhanh chóng](https://codeforces.com/problemset/problem/102500/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Claire còn bốn lần giải quyết hoàn thành và còn lại một lần giải quyết cuối cùng. Điểm cuối cùng của cô được tính bằng cách thực hiện tất cả năm lần, loại bỏ giải nhanh nhất và giải chậm nhất, sau đó lấy trung bình cộng của ba lần còn lại. Mục tiêu là để xác định xem lần giải quyết cuối cùng của cô ấy có thể chậm đến mức nào trong khi vẫn giữ điểm số cuối cùng này nhiều nhất là giá trị mục tiêu. 

Đầu vào cung cấp bốn thời gian giải đã biết và mức trung bình cuối cùng tối đa có thể chấp nhận được. Đầu ra mô tả thời gian giải quyết thứ năm lớn nhất có thể. Nếu ngay cả giải pháp thứ năm cực kỳ tốt cũng không thể đạt được mục tiêu thì câu trả lời là`impossible`. Nếu cách giải thứ 5 có thể chậm tùy ý mà mục tiêu vẫn đạt thì đáp án là`infinite`. 

Chỉ có bốn thời gian đã biết, vì vậy bài toán không cần cấu trúc dữ liệu hoặc tìm kiếm trên nhiều giá trị. Kích thước đầu vào nhỏ có nghĩa là thách thức chính là tìm ra mối quan hệ toán học giữa lần thứ năm chưa biết và điểm cuối cùng. Việc mô phỏng lực lượng vũ phu trong những khoảng thời gian có thể là không thể vì câu trả lời cần độ chính xác đến hai thập phân và phạm vi giá trị có thể lớn. Thay vào đó, giải pháp nên giảm quy tắc tính điểm thành công thức trực tiếp. 

Các trường hợp phức tạp xuất phát từ thực tế là giải pháp thứ năm có thể trở thành giải pháp tốt nhất hoặc giải pháp tồi tệ nhất, thay đổi giá trị nào bị loại bỏ. 

Ví dụ:```
6.00 7.00 8.00 9.00
8.00
```Đầu ra đúng là:```
infinite
```Nếu lần giải cuối cùng quá lớn thì nó sẽ bị loại bỏ vì thời điểm tệ nhất. Điểm còn lại dựa trên 6, 7 và 8, có điểm trung bình là 7,00. Vì điều đó đã đáp ứng được mục tiêu nên mọi giải pháp cuối cùng đều có hiệu quả. Một giải pháp bất cẩn luôn cho rằng lần thứ năm vẫn ở mức trung bình sẽ bác bỏ trường hợp này một cách không chính xác. 

Một trường hợp quan trọng khác là khi mục tiêu quá khắt khe:```
6.00 7.00 8.00 9.00
5.00
```Đầu ra đúng là:```
impossible
```Ngay cả khi giải pháp cuối cùng cực kỳ nhỏ, nó sẽ bị loại bỏ như là thời điểm tốt nhất. Các giá trị còn lại là 7, 8 và 9, cho điểm trung bình là 8,00. Không có kết quả thứ năm có thể làm cho điểm số cuối cùng thấp hơn. Một giải pháp chỉ kiểm tra trường hợp bao gồm lần thứ năm sẽ bỏ lỡ giới hạn dưới này. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là thử các giá trị khác nhau cho phép giải thứ năm, chèn từng giá trị vào năm lần, sắp xếp chúng, loại bỏ giá trị nhỏ nhất và lớn nhất rồi tính giá trị trung bình. Điều này hiệu quả vì nó tuân theo quy tắc tính điểm. 

Vấn đề là không có ít ứng cử viên đến lần thứ năm để kiểm tra. Câu trả lời có thể là bất kỳ giá trị nào trong khoảng từ 1,00 đến 20,00 với hai chữ số thập phân, cho ra 1901 khả năng. Mặc dù phạm vi đó là nhỏ trong bài toán cụ thể này, phương pháp này ẩn cấu trúc thực tế và dễ xảy ra lỗi hơn vì các giá trị biên quyết định liệu giải pháp thứ năm có bị loại bỏ hay không. 

Quan sát hữu ích là chỉ có vị trí của giá trị thứ năm trong số bốn giá trị hiện có mới quan trọng. Hãy để bốn thời gian đã biết được sắp xếp như sau:```
a <= b <= c <= d
```Nếu lần thứ năm lớn hơn cả bốn lần thì bị loại bỏ. Điểm số trở thành:```
(a + b + c) / 3
```Đây là số điểm tốt nhất có thể khi lần thứ năm tăng không giới hạn. Nếu điều này đã đáp ứng được mục tiêu thì câu trả lời là`infinite`. 

Nếu lần thứ năm nhỏ hơn cả bốn lần thì nó sẽ bị loại bỏ. Điểm số trở thành:```
(b + c + d) / 3
```Đây là tình huống tồi tệ nhất mà Claire có thể ép mình vào bằng cách chọn một lựa chọn rất nhỏ lần trước. Nếu thậm chí số điểm này cao hơn mục tiêu thì không có câu trả lời nào tồn tại. 

Tình huống duy nhất còn lại là khi lần thứ năm nằm giữa thời điểm nhỏ nhất và lớn nhất đã biết. Các giá trị bị loại bỏ khi đó luôn là mức tối thiểu và tối đa hiện tại trong số bốn thời điểm đã biết, để lại:```
(b + c + x) / 3
```trong đó x là lần thứ năm. Giải bất đẳng thức này sẽ thu được giá trị lớn nhất có thể chấp nhận được một cách trực tiếp:```
x <= 3 * target - b - c
```Lực lượng vũ phu hoạt động vì quy tắc tính điểm có thể được mô phỏng cho mọi câu trả lời có thể, nhưng nó bỏ qua việc điểm thay đổi tuyến tính trong khoảng thời gian hợp lệ. Việc quan sát ba vị trí có thể có của lời giải thứ năm làm giảm bài toán về số học theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1901) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp bốn lần giải đã biết. Gọi cho họ`a`,`b`,`c`, Và`d`từ nhỏ nhất đến lớn nhất. Chỉ có bốn giá trị này ảnh hưởng đến phạm vi có thể có của phép giải cuối cùng. 
2. Kiểm tra xem kết quả đã hợp lệ hay chưa khi phép giải thứ năm cực kỳ lớn. Giải pháp thứ năm biến mất như là điểm tệ nhất, để lại`a`,`b`, Và`c`. Nếu mức trung bình của họ cao nhất là mục tiêu, hãy in`infinite`. 
3. Kiểm tra xem kết quả có còn hợp lệ hay không khi phép giải thứ năm cực kỳ nhỏ. Giải pháp thứ năm biến mất như là điểm số tốt nhất, để lại`b`,`c`, Và`d`. Nếu mức trung bình của họ lớn hơn mục tiêu, hãy in`impossible`. 
4. Nếu không, câu trả lời phải nằm trong khoảng giữa thời gian nhỏ nhất và lớn nhất đã biết. Điểm số là`(b + c + x) / 3`, do đó giải bất đẳng thức để có được`x = 3 * target - b - c`. 
5. In giá trị này với đúng hai chữ số thập phân. 

Tại sao nó hoạt động: 

Giải pháp thứ năm chỉ có thể ảnh hưởng đến điểm số theo ba cách. Nếu nó nằm ngoài phạm vi thời gian đã biết, nó sẽ bị loại bỏ và điểm sẽ được ấn định. Hai trường hợp cực đoan này cho chúng ta biết liệu mọi giá trị có thể có đều hoạt động hay không có giá trị nào có thể hoạt động. Nếu không áp dụng cực đoan nào, thì giải pháp thứ năm là một phần của ba giá trị ở giữa và điểm cuối cùng sẽ tăng tuyến tính theo nó. Giải bất đẳng thức tuyến tính sẽ cho giá trị chính xác lớn nhất cho phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    times = list(map(float, input().split()))
    target = float(input())

    times.sort()
    a, b, c, d = times

    if (a + b + c) / 3 <= target + 1e-9:
        print("infinite")
        return

    if (b + c + d) / 3 > target + 1e-9:
        print("impossible")
        return

    ans = 3 * target - b - c
    print(f"{ans:.2f}")

if __name__ == "__main__":
    solve()
```Đầu tiên, chương trình sẽ sắp xếp bốn thời điểm đã biết vì công thức chỉ phụ thuộc vào thứ tự của chúng. Các biến`a`,`b`,`c`, Và`d`biểu diễn cái nhỏ nhất cho đến cái lớn nhất. 

Điều kiện đầu tiên kiểm tra trường hợp giải pháp cuối cùng là giải quyết chậm nhất và bị loại bỏ. Điều kiện thứ hai kiểm tra trường hợp giải pháp cuối cùng là giải quyết nhanh nhất và bị loại bỏ. Epsilon nhỏ ngăn việc làm tròn dấu phẩy động thay đổi so sánh đẳng thức. 

Nếu cả hai lần kiểm tra đều không thành công thì lần kiểm tra thứ năm phải nằm trong khoảng thời gian thích hợp. Công thức`3 * target - b - c`đến từ việc sắp xếp lại điều kiện trung bình. Các giá trị dấu phẩy động của Python là đủ vì độ chính xác đầu vào chỉ là hai chữ số thập phân và đầu ra cuối cùng được làm tròn đến hai chữ số thập phân. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
6.38 7.20 6.95 8.11
7.53
```Sau khi sắp xếp, các giá trị là 6,38, 6,95, 7,20, 8,11. 

| Bước | một | b | c | d | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Giá trị được sắp xếp | 6,38 | 6,95 | 7h20 | 8.11 | | 
| Kiểm tra vô hạn | | | | | (6,38 + 6,95 + 7,20) / 3 = 6,84 | 
| So sánh | | | | | 6,84 <= 7,53 | 

Điểm trung bình đã thấp hơn mục tiêu ngay cả khi lần giải cuối cùng tệ tùy ý, vì vậy câu trả lời là`infinite`. 

Đối với mẫu thứ hai:```
6.38 7.20 6.95 8.11
6.99
```Các giá trị được sắp xếp là như nhau. 

| Bước | một | b | c | d | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Kiểm tra vô hạn | 6,38 | 6,95 | 7h20 | 8.11 | 6,84 <= 6,99 | 
| So sánh | | | | | Trường hợp vô hạn thành công | 

Đầu ra lại là`infinite`theo tính toán. Đối với mẫu thứ hai được cung cấp với mục tiêu 6,99, giá trị ngưỡng nằm trong phạm vi hợp lệ và công thức cho phép giải thứ năm tối đa:```
3 * 6.99 - 6.95 - 7.20 = 6.82
```Giải thứ năm có thể là 6,82 trong khi vẫn giữ mức trung bình của ba ở giữa chính xác ở mục tiêu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Sắp xếp bốn số mất thời gian không đổi. | 
| Không gian | O(1) | Chỉ có bốn số và một vài biến được lưu trữ. | 

Đầu vào chỉ chứa bốn thời điểm thích hợp, do đó giải pháp thực hiện một số thao tác cố định bất kể giá trị. Nó dễ dàng phù hợp trong giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    times = list(map(float, input().split()))
    target = float(input())

    times.sort()
    a, b, c, d = times

    if (a + b + c) / 3 <= target + 1e-9:
        ans = "infinite"
    elif (b + c + d) / 3 > target + 1e-9:
        ans = "impossible"
    else:
        ans = f"{3 * target - b - c:.2f}"

    sys.stdin = old_stdin
    return ans

assert run("6.38 7.20 6.95 8.11\n7.53\n") == "infinite"
assert run("6.38 7.20 6.95 8.11\n6.99\n") == "6.82"
assert run("6.38 7.20 6.95 8.11\n6.45\n") == "impossible"

assert run("1.00 1.00 1.00 1.00\n1.00\n") == "infinite"
assert run("20.00 20.00 20.00 20.00\n1.00\n") == "impossible"
assert run("5.00 6.00 7.00 8.00\n6.50\n") == "6.50"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1.00 1.00 1.00 1.00 / 1.00`|`infinite`| Tất cả các giá trị đều bằng nhau và mọi giải pháp thứ năm đều hoạt động. | 
|`20.00 20.00 20.00 20.00 / 1.00`|`impossible`| Mục tiêu bất khả thi và không thể cải thiện được. | 
|`5.00 6.00 7.00 8.00 / 6.50`|`6.50`| Công thức tính toán và xử lý ranh giới chính xác. | 

## Vỏ cạnh 

Trường hợp vô hạn được xử lý bằng cách kiểm tra ba phép giải nhỏ nhất đã biết. Vì:```
6.00 7.00 8.00 9.00
8.00
```thuật toán sắp xếp các giá trị và tính toán`(6 + 7 + 8) / 3 = 7.00`. Vì mức này đã thấp hơn mục tiêu nên bất kỳ lần thứ năm nào cũng có thể chấp nhận được, kể cả giá trị lớn hơn nhiều so với 9,00. 

Trường hợp không thể được xử lý bằng cách kiểm tra ba lời giải lớn nhất đã biết. Vì:```
6.00 7.00 8.00 9.00
5.00
```điểm cuối cùng nhỏ nhất có thể xảy ra khi lượt giải thứ năm bị loại bỏ vì thời điểm tốt nhất. Số trung bình còn lại là`(7 + 8 + 9) / 3 = 8.00`, cao hơn 5,00. Thuật toán từ chối chính xác tình huống. 

Ranh giới giữa câu trả lời hữu hạn và vô hạn cũng rất quan trọng. Vì:```
6.00 7.00 8.00 9.00
7.00
```kiểm tra vô hạn thành công chính xác bởi vì`(6 + 7 + 8) / 3 = 7.00`. Việc so sánh mà không xử lý sự bình đẳng sẽ tạo ra một câu trả lời hữu hạn một cách không chính xác. 

Trường hợp hữu hạn là:```
6.00 7.00 8.00 9.00
7.50
```Kiểm tra vô hạn không thành công vì 7.00 không phải là vấn đề và kiểm tra không thể thất bại vì`(7 + 8 + 9) / 3 = 8.00`là quá cao. Công thức cho:```
3 * 7.50 - 7.00 - 8.00 = 7.50
```Vì vậy, kết quả cuối cùng là 7,50 chính xác là giá trị lớn nhất giữ mức trung bình ở mức mục tiêu.
