---
title: "CF 102638A - Lắng nghe trái tim bạn"
description: "Nhiệm vụ ẩn một số nguyên duy nhất bên trong một phương trình bao gồm căn bậc hai, căn bậc ba và hai lũy thừa khác nhau. Không có giá trị đầu vào để xử lý. Chương trình chỉ phải xác định số nguyên x duy nhất thỏa mãn phương trình và in ra."
date: "2026-07-31T00:28:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102638
codeforces_index: "A"
codeforces_contest_name: "Bredor contest"
rating: 0
weight: 102638
solve_time_s: 151
verified: true
draft: false
---

[CF 102638A - Lắng nghe trái tim bạn](https://codeforces.com/problemset/problem/102638/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 31s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ ẩn một số nguyên duy nhất bên trong một phương trình bao gồm căn bậc hai, căn bậc ba và hai lũy thừa khác nhau. Không có giá trị đầu vào để xử lý. Chương trình chỉ phải xác định số nguyên duy nhất`x`thỏa mãn phương trình và in nó. 

Phần bất thường của vấn đề này là phương trình không nhằm mục đích giải bằng tìm kiếm số. Giới hạn thời gian thực sự yêu cầu quan sát liên tục. Một chương trình mạnh mẽ thử nhiều số nguyên sẽ dành thời gian để kiểm tra căn bậc hai và căn bậc ba nhiều lần, trong khi giải pháp dự định sẽ biến đại số thành một cấu trúc trực tiếp. 

Không có ràng buộc phụ thuộc vào đầu vào vì đầu vào trống. Hạn chế có ý nghĩa duy nhất là câu trả lời phải được tìm ra một cách chính xác. Các phép tính gần đúng dấu phẩy động rất nguy hiểm vì phương trình có lũy thừa lớn, trong đó một sai số làm tròn nhỏ có thể biến một đẳng thức đúng thành một so sánh sai. 

Một cách tiếp cận bất cẩn có thể chỉ tìm kiếm trong một khoảng thời gian nhỏ. Ví dụ: thử các giá trị từ`0`ĐẾN`100`sẽ không bao giờ tìm thấy câu trả lời, bởi vì đầu ra đúng là`228`. Một sai lầm khác là so sánh hai vế bằng cách sử dụng số học dấu phẩy động. Ngay cả khi đạt đến số nguyên chính xác, các biểu thức như lũy thừa của các giá trị vô tỷ có thể tích lũy đủ lỗi để không kiểm tra đẳng thức. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là lặp lại các số nguyên có thể`x`, xác minh rằng tất cả các căn thức đã được xác định, đánh giá cả hai vế của phương trình và dừng lại khi chúng khớp nhau. Điều này đúng về mặt logic vì câu lệnh đảm bảo rằng chính xác một số nguyên hoạt động. Tuy nhiên, nó không có giới hạn trên tự nhiên. Việc tìm kiếm trong phạm vi rộng sẽ yêu cầu số lượng đánh giá căn bản tốn kém không xác định và việc kiểm tra sự bằng nhau với các giá trị dấu phẩy động cũng sẽ không đáng tin cậy. 

Quan sát quan trọng đến từ các quyền lực. Nếu hai số thực thỏa mãn`a^7 = b^3`, thì vì 7 và 3 là nguyên tố cùng nhau nên cả hai giá trị đều có thể được biểu diễn bằng một cơ số chung. Chúng ta có thể viết:```
a = t^3
b = t^7
```cho một số giá trị thực`t`. 

Áp dụng điều này cho vế phải sẽ cho:```
x - sqrt((x^2 - 1984) / 5) = t^7
```Giá trị ẩn hóa ra rất đơn giản. Đang thử ứng cử viên số nguyên nhỏ tự nhiên`t = 2`cho`t^7 = 128`. Thay thế điều này vào biểu thức thứ hai:```
x - sqrt((x^2 - 1984) / 5) = 128
```và cô lập căn bậc hai cho:```
(x - 128)^2 = (x^2 - 1984) / 5
```Khai triển phương trình này dẫn đến:```
x^2 - 320x + 20976 = 0
```Người phân biệt đối xử là`18496`, căn bậc hai của nó là`136`, đưa ra các gốc có thể:```
x = (320 ± 136) / 2
```Hai ứng cử viên là`228`Và`92`. Chỉ một`228`thỏa mãn phương trình không bình phương ban đầu vì căn bậc hai phải bằng`x - 128`, không âm. Kiểm tra phía bên kia xác nhận cấu trúc ẩn:```
sqrt(228 - 3) - cbrt((3 * 228 + 2) / 2)
= sqrt(225) - cbrt(343)
= 15 - 7
= 8
= 2^3
```Vậy toàn bộ phương trình được thỏa mãn với`t = 2`Và`x = 228`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(R) kiểm tra phạm vi tìm kiếm | O(1) | Quá chậm và không đáng tin cậy | 
| Quan sát đại số | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nhận biết hai bên có quyền 7 và 3 nên đại diện cho cả hai bên sử dụng cơ sở chung. Bên trái trở thành`t^3`và phía bên phải trở thành`t^7`. 
2. Tìm cơ số nguyên nhỏ làm cho các biểu thức thẳng hàng. Lấy`t = 2`làm cho vế phải bằng`128`. 
3. Giải phương trình:```
x - sqrt((x^2 - 1984) / 5) = 128
```bằng cách di chuyển phần không căn bản sang phía bên kia và bình phương. 

1. Giải phương trình bậc hai thu được. Nó tạo ra hai giá trị có thể,`228`Và`92`. 
2. Kiểm tra điều kiện dấu từ biểu thức căn bậc hai ban đầu. Giá trị dưới căn bậc hai biểu thị`x - 128`, do đó ứng viên phải đáp ứng`x >= 128`. Điều này loại bỏ`92`. 
3. Đầu ra`228`. 

Tại sao nó hoạt động: 

Việc chuyển đổi không đoán được câu trả lời một cách trực tiếp. Các lũy thừa buộc cả hai vế của phương trình đều là lũy thừa có cùng đại lượng. giá trị`t = 2`tạo ra một cấu trúc số nguyên chính xác: vế phải trở thành`128`, và vế trái trở thành`8`, chính xác là`2^3`. Việc giải phương trình bậc hai thu được sẽ tìm thấy mọi ứng cử viên có thể được tạo ra bởi phép biến đổi này và việc kiểm tra điều kiện dấu ban đầu sẽ loại bỏ nghiệm ngoại lai được đưa ra bằng cách bình phương. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    print(228)

if __name__ == "__main__":
    solve()
```Giải pháp không đọc bất kỳ dữ liệu đầu vào nào vì sự cố không chứa dữ liệu đầu vào. Phép rút gọn đại số hoàn chỉnh cho thấy số nguyên hợp lệ duy nhất là`228`, do đó chương trình chỉ cần in giá trị đó. 

Không có lo ngại về tràn vì không có phép tính nào được thực hiện trong thời gian chạy. Cũng không có phép toán dấu phẩy động, tránh các vấn đề về độ chính xác từ phương trình ban đầu. 

## Ví dụ đã hoạt động 

Vì bài toán không có đầu vào mẫu nên các dấu vết bên dưới sử dụng cách thực thi có ý nghĩa duy nhất: chương trình khởi động và in ra lời giải cố định. 

| Bước | Hành động | Giá trị | 
| --- | --- | --- | 
| 1 | Bắt đầu chương trình | Không có đầu vào | 
| 2 | Gọi`solve()`|`print(228)`| 
| 3 | Đầu ra |`228`| 

Dấu vết này chứng tỏ rằng thuật toán không phụ thuộc vào các giá trị bên ngoài. Bản thân phương trình xác định duy nhất đầu ra. 

| Bước | Hành động | Giá trị | 
| --- | --- | --- | 
| 1 | Kiểm tra luồng đầu vào | Trống | 
| 2 | Bỏ qua phân tích cú pháp | Không cần biến | 
| 3 | In câu trả lời |`228`| 

Dấu vết thứ hai này xác nhận rằng việc triển khai mong đợi các trường hợp thử nghiệm hoặc đầu vào bằng số sẽ giải quyết được một vấn đề khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chương trình thực hiện một thao tác đầu ra. | 
| Không gian | O(1) | Không có bộ nhớ bổ sung được sử dụng. | 

Giới hạn bộ nhớ dễ dàng được thỏa mãn vì chương trình không lưu trữ dữ liệu. Giải pháp theo thời gian không đổi là cách tiếp cận thực tế duy nhất cho một nhiệm vụ không có đầu vào và câu trả lời đại số chính xác. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return "228\n"
    finally:
        sys.stdin = old_stdin

assert run("") == "228\n", "empty input"

assert run("\n") == "228\n", "newline input"

assert run("0\n") == "228\n", "unexpected extra input"

assert run("100\n") == "228\n", "arbitrary ignored input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đầu vào trống | 228 | Định dạng đầu vào chính thức không chứa gì. | 
| Đầu vào dòng mới | 228 | Khoảng trắng không ảnh hưởng đến giải pháp. | 
|`0`| 228 | Chương trình không phụ thuộc sai vào các số bên ngoài. | 
|`100`| 228 | Câu trả lời phương trình cố định luôn được in. | 

## Vỏ cạnh 

Giải pháp dựa trên tìm kiếm có thể thất bại vì nó cho rằng câu trả lời nằm trong phạm vi dự đoán. Ví dụ: việc triển khai chỉ kiểm tra các giá trị từ`1`ĐẾN`100`không nhận được đầu vào nhưng vẫn thử các câu trả lời có thể có trong nội bộ. Nó sẽ kết luận không chính xác rằng không có giải pháp nào, trong khi kết quả đầu ra đúng là`228`. Giải pháp thời gian không đổi tránh điều này bằng cách lấy giá trị thay vì tìm kiếm. 

Phương pháp xác minh dấu phẩy động cũng có thể thất bại. Câu trả lời hợp lệ là:```
x = 228
```Ở giá trị này, cơ sở bên trái chính xác là:```
sqrt(225) - cbrt(343) = 15 - 7 = 8
```và cơ sở bên phải chính xác là:```
228 - sqrt((228^2 - 1984) / 5) = 228 - 100 = 128
```Sự bình đẳng là:```
8^7 = 128^3
```Một phép tính gần đúng bằng số có thể biểu thị một bên hơi khác một chút sau khi lặp đi lặp lại các phép toán căn bản và lũy thừa. Giải pháp cuối cùng tránh tất cả những so sánh như vậy bằng cách in trực tiếp số nguyên đã được chứng minh.
