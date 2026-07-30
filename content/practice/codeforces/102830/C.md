---
title: "CF 102830C - Kevin's Meme Reacts"
description: "Kevin bắt đầu chuỗi phản ứng meme bằng một phản ứng của riêng mình. Mỗi đêm số lượng phản ứng hiện tại sẽ tự động tăng gấp đôi. Anh ta cũng có thể tự thêm phản ứng vào bất kỳ buổi sáng nào và những phản ứng thủ công đó sẽ xảy ra ngay lập tức."
date: "2026-07-26T15:23:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102830
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 11-06-20 Div. 2 (Beginner)"
rating: 0
weight: 102830
solve_time_s: 35
verified: true
draft: false
---

[CF 102830C - Phản ứng Meme của Kevin](https://codeforces.com/problemset/problem/102830/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Kevin bắt đầu chuỗi phản ứng meme bằng một phản ứng của riêng mình. Mỗi đêm số lượng phản ứng hiện tại sẽ tự động tăng gấp đôi. Anh ta cũng có thể tự thêm phản ứng vào bất kỳ buổi sáng nào và những phản ứng thủ công đó sẽ xảy ra ngay lập tức. Mục tiêu là đạt được chính xác`n`phản ứng trong khi thực hiện ít phản ứng thủ công nhất có thể, đếm phản ứng đầu tiên bắt đầu chuỗi. 

Đầu vào là một số nguyên duy nhất`n`, đại diện cho số lượng phản ứng cuối cùng cần thiết. Đầu ra là số phản ứng thủ công nhỏ nhất mà Kevin phải thực hiện. 

Giới hạn cho phép`n`lớn như`10^9`. Điều này loại trừ các mô phỏng phụ thuộc vào số ngày hoặc việc thử nhiều lịch trình có thể. Một giải pháp sẽ hoạt động trong thời gian không đổi hoặc nhiều nhất là thời gian logarit vì`10^9`chỉ có khoảng 30 chữ số nhị phân. 

Khó khăn chính là nhận ra rằng phản ứng thủ công không phải lúc nào cũng chỉ đóng góp một phản ứng trong lần đếm cuối cùng. Nếu Kevin phản ứng sớm thì phản ứng đó sẽ nhân đôi lên nhiều lần. Nếu anh ta phản ứng muộn hơn, số lần đó sẽ ít hơn gấp đôi. Mỗi phản ứng thủ công có thể được coi là việc chọn một lũy thừa của hai để góp phần vào tổng số cuối cùng. 

Một số trường hợp đặc biệt bộc lộ sai sót trong quá trình triển khai. 

Vì`n = 1`, câu trả lời là`1`. Kevin phải thực hiện phản ứng đầu tiên, và không có cách nào có được phản ứng đầu tiên nếu không thực hiện. 

Đối với đầu vào:```
8
```đầu ra đúng là:```
1
```Một cách tiếp cận bất cẩn có thể cho rằng Kevin cần một vài phản ứng vì số lượng tăng lên trong nhiều ngày, nhưng riêng phản ứng đầu tiên đã trở thành`2`,`4`, và sau đó`8`. 

Đối với đầu vào:```
5
```đầu ra đúng là:```
2
```Một phản ứng có thể phát triển thành`4`, sau đó một phản ứng thủ công nữa sẽ tạo ra`5`. Cố gắng đếm ngày thay vì thực hiện thủ công sẽ cho kết quả sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ mô phỏng quá trình này. Chúng ta có thể bắt đầu với một phản ứng, liên tục nhân đôi số lượng phản ứng mỗi ngày và thử thêm các phản ứng vào những thời điểm khác nhau cho đến khi đạt được`n`. Điều này hiệu quả vì mọi lịch trình khả thi đều có thể được mô tả bằng cách quyết định thời điểm mỗi phản ứng thủ công xảy ra. 

Vấn đề là điều này khám phá một số lượng lớn các lịch trình có thể. Đối với một số có nhiều bit được đặt, có thể có nhiều lựa chọn về thời điểm thêm phản ứng. Không gian tìm kiếm tăng theo cấp số nhân, mặc dù`n`bản thân nó chỉ có khoảng 30 bit. Điều này là không cần thiết vì quá trình nhân đôi có cấu trúc toán học đơn giản. 

Quan sát hữu ích là mọi phản ứng thủ công đều đóng góp lũy thừa hai cho câu trả lời cuối cùng. Phản ứng được thực hiện lúc đầu góp phần`2^k`sau đó`k`đêm. Phản ứng được thực hiện một ngày sau đó góp phần`2^(k-1)`, vân vân. Con số cuối cùng chính xác là tổng lũy ​​thừa của hai tương ứng với thời điểm Kevin phản ứng. 

Biểu diễn nhị phân của một số là cách duy nhất để viết nó dưới dạng tổng lũy ​​thừa của hai. Vì mỗi bit được đặt có nghĩa là một phản ứng thủ công cần thiết nên câu trả lời chỉ đơn giản là số lượng`1`các bit trong biểu diễn nhị phân của`n`. 

Brute-force hoạt động vì mọi lịch trình đều tương ứng với tổng lũy ​​thừa hợp lệ của hai, nhưng không thành công vì nó tìm kiếm thông qua các lịch trình mà biểu diễn nhị phân đã mô tả trực tiếp. Việc đếm các bit đã đặt sẽ loại bỏ toàn bộ tìm kiếm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ của số lần phản ứng có thể xảy ra | O(1) | Quá chậm | 
| Tối ưu | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng phản ứng mục tiêu`n`. 

Giá trị của`n`chứa tất cả thông tin cần thiết vì thời gian của phản ứng chỉ ảnh hưởng đến lũy thừa nào của hai được thêm vào. 
2. Nhìn vào biểu diễn nhị phân của`n`. 

Mỗi chữ số nhị phân biểu thị liệu có cần một lũy thừa cụ thể của hai trong tổng cuối cùng hay không. Một chữ số bằng`1`có nghĩa là Kevin cần một phản ứng thủ công tồn tại nhờ sự đóng góp đó. 
3. Đếm xem có bao nhiêu chữ số nhị phân bằng`1`. 

Số lượng này là số lượng phản ứng thủ công tối thiểu vì biểu diễn nhị phân sử dụng ít lũy thừa nhất có thể của hai để tạo ra số. 
4. Xuất số đếm. 

Số đếm bao gồm phản ứng ban đầu vì bit nhị phân thấp nhất tương ứng với phản ứng được thực hiện trước khi nhân đôi. 

Tại sao nó hoạt động: 

Mỗi phản ứng thủ công đều có đóng góp cuối cùng bằng chính xác một lũy thừa hai. Nếu Kevin phản ứng vào một lúc nào đó và có`k`đêm còn lại, phản ứng đó góp phần`2^k`phản ứng ở cuối. Do đó, bất kỳ chiến lược hợp lệ nào cũng là một tập hợp lũy thừa của hai mà tổng của nó là`n`. Biểu diễn nhị phân mang lại sự phân tách duy nhất của`n`thành lũy thừa của hai và sử dụng từng bit được đặt một lần sẽ giảm thiểu số lượng thuật ngữ. Số lượng bit được đặt chính xác là số lượng phản ứng thủ công tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    print(n.bit_count())

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng tích hợp sẵn của Python`bit_count()`phương pháp đếm số bit thiết lập trong biểu diễn số nguyên. Điều này phù hợp trực tiếp với quan sát toán học từ thuật toán. 

Đầu vào chỉ chứa một số nguyên, do đó, đầu vào nhanh là không cần thiết để thực hiện, nhưng kiểu đầu vào lập trình cạnh tranh tiêu chuẩn được sử dụng. Đầu ra là số lượng phản ứng thủ công cần thiết. 

Không có vấn đề về ranh giới vì`bit_count()`xử lý trực tiếp tất cả các số nguyên dương. Đầu vào nhỏ nhất`1`, tạo ra một bit được đặt. Đầu vào được phép lớn nhất`10^9`, vẫn dễ dàng phù hợp với kiểu số nguyên của Python và chỉ có một số lượng nhỏ bit. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
```Dạng nhị phân:`101`| Bước | Giá trị hiện tại | Dạng nhị phân | Đặt số bit được tính | 
| --- | --- | --- | --- | 
| Bắt đầu | 5 | 101 | 0 | 
| Đọc bit thấp nhất | 5 | 101 | 1 | 
| Đọc bit giữa | 5 | 101 | 1 | 
| Đọc bit cao nhất | 5 | 101 | 2 | 

Các chữ số nhị phân được`1`tương ứng với`4 + 1`. Kevin cần một phản ứng phát triển thành`4`và một phản ứng vẫn giữ nguyên`1`, đưa ra hai phản ứng thủ công. 

### Ví dụ 2 

đầu vào:```
8
```Dạng nhị phân:`1000`| Bước | Giá trị hiện tại | Dạng nhị phân | Đặt số bit được tính | 
| --- | --- | --- | --- | 
| Bắt đầu | 8 | 1000 | 0 | 
| Đếm bit 0 | 8 | 1000 | 0 | 
| Đếm bit 1 | 8 | 1000 | 0 | 
| Đếm bit 2 | 8 | 1000 | 0 | 
| Đếm bit 3 | 8 | 1000 | 1 | 

Chỉ cần một sức mạnh của hai. Phản ứng ban đầu tăng gấp đôi ba lần và trở thành tám, vì vậy câu trả lời là một. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Số chữ số nhị phân được xử lý là số bit trong`n`. | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ số đầu vào và câu trả lời. | 

Giá trị tối đa của`n`có ít hơn 31 chữ số nhị phân, do đó giải pháp dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())
    return str(n.bit_count()) + "\n"

# provided samples
assert solve("5\n") == "2\n", "sample 1"
assert solve("8\n") == "1\n", "sample 2"

# custom cases
assert solve("1\n") == "1\n", "minimum value"
assert solve("3\n") == "2\n", "two consecutive bits"
assert solve("1023\n") == "10\n", "all bits set"
assert solve("1000000000\n") == "13\n", "large boundary value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Đầu vào nhỏ nhất có thể vẫn yêu cầu phản ứng ban đầu. | 
|`3`|`2`| Xác nhận các số cần lũy thừa của hai. | 
|`1023`|`10`| Kiểm tra một giá trị trong đó có nhiều bit được đặt. | 
|`1000000000`|`13`| Kiểm tra hành vi gần giới hạn trên. | 

## Vỏ cạnh 

cho`n = 1`:```
1
```Biểu diễn nhị phân là`1`. Thuật toán đếm một bit được đặt và xuất ra`1`. Điều này phù hợp với quy trình vì Kevin phải tự mình tạo ra phản ứng đầu tiên. 

Vì`n = 8`:```
8
```Biểu diễn nhị phân là`1000`. Thuật toán chỉ nhìn thấy một bit được đặt và xuất ra`1`. Phản ứng ban đầu đơn lẻ nhân đôi ba lần, tạo ra chính xác tám phản ứng mà không cần thực hiện thêm bất kỳ thao tác thủ công nào. 

Vì`n = 5`:```
5
```Biểu diễn nhị phân là`101`. Thuật toán đếm hai bit được đặt và đầu ra`2`. Những điều này tương ứng với sự đóng góp của`4`Và`1`, phù hợp với một lịch trình trong đó một phản ứng xảy ra sớm và một phản ứng được thêm vào sau. 

Đối với một giá trị có mọi bit thấp được đặt, chẳng hạn như:```
1023
```biểu diễn nhị phân là`1111111111`. Đầu ra của thuật toán`10`. Mỗi trong số mười lũy thừa của hai là bắt buộc và không có chiến lược nào có thể sử dụng ít phản ứng thủ công hơn vì mỗi hành động thủ công chỉ đóng góp một lũy thừa của hai.
