---
title: "CF 188F - Ký hiệu nhị phân"
description: "Nhiệm vụ là lấy một số nguyên dương duy nhất và biểu thị nó trong cơ số 2, nghĩa là chúng ta viết lại nó chỉ sử dụng lũy ​​thừa hai với hệ số 0 hoặc 1. Thay vì biểu diễn thập phân thông thường, chúng ta muốn chuỗi nhị phân cho chúng ta biết lũy thừa nào của hai tổng của số đó."
date: "2026-06-03T01:07:40+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 188
codeforces_index: "F"
codeforces_contest_name: "Surprise Language Round 6"
rating: 1400
weight: 188
solve_time_s: 60
verified: true
draft: false
---

[CF 188F - Ký hiệu nhị phân](https://codeforces.com/problemset/problem/188/F) 

**Đánh giá:** 1400 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là lấy một số nguyên dương duy nhất và biểu thị nó trong cơ số 2, nghĩa là chúng ta viết lại nó chỉ sử dụng lũy thừa hai với hệ số 0 hoặc 1. Thay vì biểu diễn thập phân thông thường, chúng ta muốn chuỗi nhị phân cho chúng ta biết lũy thừa nào của hai tổng của số đó. 

Ví dụ: số 5 tương ứng với việc chọn 4 và 1 nên biểu diễn nhị phân của nó là`101`. Mỗi vị trí trong chuỗi nhị phân biểu thị liệu một lũy thừa cụ thể của hai có được bao gồm trong tổng hay không. 

Ràng buộc rất nhỏ, với n lên tới 1.000.000. Điều đó ngay lập tức có nghĩa là chúng ta không bao giờ gặp nguy hiểm về các vấn đề về hiệu suất ngay cả khi thao tác bit đơn giản hoặc phép chia lặp đi lặp lại. Bất kỳ giải pháp nào chạy theo thời gian logarit hoặc thậm chí tuyến tính theo số bit là đủ. Vì độ dài nhị phân 1.000.000 tối đa là 20 bit nên bản thân đầu ra rất nhỏ. 

Không có trường hợp góc khuất nào về mặt cấu trúc, nhưng một số cạm bẫy triển khai thường xuyên xuất hiện. Người ta vô tình tạo ra các số 0 đứng đầu, ví dụ như in một cái gì đó như`000101`thay vì`101`. Một cách khác là đảo ngược các chữ số nhị phân không chính xác nếu sử dụng phép chia cho 2 và nối các bit sai thứ tự. Thứ ba là xử lý sai giá trị 1, giá trị này sẽ xuất ra trực tiếp`1`mà không cần bất kỳ định dạng bổ sung nào. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để suy nghĩ về vấn đề này là mô phỏng việc xây dựng nhị phân bằng cách liên tục kiểm tra lũy thừa của hai. Người ta có thể bắt đầu từ 1, 2, 4, 8, v.v., kiểm tra xem mỗi lũy thừa có khớp với giá trị còn lại hay không, trừ đi nếu có. Cách tiếp cận này đúng vì biểu diễn nhị phân về cơ bản là sự phân rã tham lam trên lũy thừa của hai. Tuy nhiên, nó là gián tiếp không cần thiết và đòi hỏi lũy thừa hoặc lặp đi lặp lại trên tất cả các lũy thừa lên đến n, ở đây vẫn còn nhỏ nhưng không hiệu quả về mặt khái niệm. 

Một cách tiếp cận thô bạo hơn là lặp lại phép chia cho 2. Chúng tôi chia số đó cho 2, ghi lại số dư mỗi lần và thu thập các bit. Điều này hoạt động vì chia cho 2 sẽ trích xuất bit có ý nghĩa nhỏ nhất ở mỗi bước. Vấn đề không phải là tính chính xác mà là thứ tự đầu ra. Phần còn lại xuất phát từ bit ít quan trọng nhất đến bit quan trọng nhất, vì vậy chúng phải được đảo ngược ở cuối. 

Quan sát quan trọng là biểu diễn nhị phân chính xác là chuỗi số dư được lặp lại khi chia cho 2. Điều này làm giảm vấn đề thành một vòng lặp đơn giản có độ dài logarit, tạo ra nhiều nhất là khoảng 20 lần lặp cho đầu vào tối đa. Chúng tôi không cần bất kỳ cấu trúc dữ liệu bổ sung nào ngoài trình tạo chuỗi hoặc danh sách ký tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (sức mạnh của hai lần quét) | O(log n) | O(1) | Đã chấp nhận | 
| Tối ưu (chia lặp lại cho 2) | O(log n) | O(log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lấy số nguyên đầu vào n làm số cần chuyển đổi. 
2. Liên tục chia n cho 2 trong khi lưu trữ số dư mỗi lần, vì số dư cho chúng ta biết bit có trọng số nhỏ nhất hiện tại là 0 hay 1. 
3. Sau mỗi lần chia, thay n bằng n // 2 để chúng ta tiến tới các bit có ý nghĩa hơn. 
4. Lưu trữ từng phần còn lại trong một danh sách vì phần còn lại đầu tiên tương ứng với bit có trọng số thấp nhất. 
5. Khi n trở thành 0, dừng vòng lặp vì tất cả các bit đã được xử lý. 
6. Đảo ngược phần dư đã thu thập vì chúng được tạo từ bit ít quan trọng nhất đến bit quan trọng nhất. 
7. Nối chúng thành một chuỗi duy nhất và xuất ra. 

Lý do bước đặt hàng này là cần thiết vì việc mở rộng nhị phân có tính chất vị trí. Mỗi bộ phận trích xuất thông tin từ vị trí ít quan trọng nhất hiện tại, do đó thứ tự tạo tự nhiên bị đảo ngược so với biểu diễn cuối cùng. 

### Tại sao nó hoạt động 

Ở mỗi bước, thuật toán phân tích n thành n = 2q + r, trong đó r là 0 hoặc 1. Phần dư r đó chính xác là chữ số nhị phân thấp nhất hiện tại. Loại bỏ chữ số đó bằng cách thay thế n bằng q sẽ bảo toàn bất biến rằng số còn lại vẫn biểu thị các bit bậc cao hơn của giá trị ban đầu. Lặp lại điều này cho đến khi n bằng 0 đảm bảo rằng mọi lũy thừa của hai phần đóng góp được trích xuất chính xác một lần, do đó chuỗi đảo ngược cuối cùng là biểu diễn nhị phân duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())

bits = []

while n > 0:
    bits.append(str(n % 2))
    n //= 2

print(''.join(reversed(bits)))
```Giải pháp đọc số nguyên, sau đó liên tục trích xuất bit có trọng số nhỏ nhất bằng cách sử dụng modulo 2. Mỗi bit được trích xuất được lưu trữ dưới dạng một chuỗi. Vì các bit này được thu thập theo thứ tự ngược lại nên chúng tôi đảo ngược danh sách ở cuối trước khi nối. 

Một chi tiết tinh tế nhưng quan trọng là chúng ta dừng lại khi n trở thành 0. Nếu tiếp tục, chúng ta sẽ nối thêm các số 0 đứng đầu, vốn không phải là một phần của biểu diễn nhị phân chuẩn. Một chi tiết khác là sớm chuyển đổi bit thành chuỗi, giúp đơn giản hóa thao tác nối cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 5 

| Bước | n | n % 2 | bit | 
| --- | --- | --- | --- | 
| 1 | 5 | 1 | [1] | 
| 2 | 2 | 0 | [1, 0] | 
| 3 | 1 | 1 | [1, 0, 1] | 
| 4 | 0 | dừng lại | [1, 0, 1] | 

Sau khi đảo ngược, chúng ta nhận được`101`. Điều này xác nhận rằng thuật toán xây dựng lại chính xác biểu diễn nhị phân từ bit ít quan trọng nhất trở lên. 

### Ví dụ 2: n = 6 

| Bước | n | n % 2 | bit | 
| --- | --- | --- | --- | 
| 1 | 6 | 0 | [0] | 
| 2 | 3 | 1 | [0, 1] | 
| 3 | 1 | 1 | [0, 1, 1] | 
| 4 | 0 | dừng lại | [0, 1, 1] | 

Đảo ngược mang lại`110`, khớp với 6 = 4 + 2. 

Dấu vết này cho thấy rằng ngay cả khi bit có trọng số nhỏ nhất bằng 0, nó vẫn được ghi lại và bảo tồn chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Mỗi lần lặp lại giảm một nửa n, vì vậy chúng tôi xử lý tối đa số bit trong n | 
| Không gian | O(log n) | Chúng tôi lưu trữ một ký tự mỗi bit | 

Kích thước đầu vào tối đa tạo ra tối đa 20 bit, do đó cả thời gian và mức sử dụng bộ nhớ đều không đổi đối với vấn đề này và nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input().strip())

    bits = []
    while n > 0:
        bits.append(str(n % 2))
        n //= 2

    return ''.join(reversed(bits))

# provided sample
assert run("5\n") == "101", "sample 1"

# minimum value
assert run("1\n") == "1", "minimum case"

# power of two
assert run("8\n") == "1000", "power of two"

# all bits set
assert run("7\n") == "111", "all ones case"

# random mid value
assert run("10\n") == "1010", "alternating pattern"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | đầu vào hợp lệ nhỏ nhất | 
| 8 | 1000 | bit cao đơn | 
| 7 | 111 | liên tiếp | 
| 10 | 1010 | mô hình nhị phân xen kẽ | 

## Vỏ cạnh 

Đối với đầu vào`1`, thuật toán bắt đầu với n = 1, nối thêm`1 % 2 = 1`, sau đó giảm n xuống 0 và dừng lại. Danh sách bit trở thành`[1]`, và đảo ngược nó vẫn mang lại`1`. Điều này xác nhận rằng trường hợp bit đơn được xử lý mà không tạo ra đầu ra trống hoặc phần đệm thêm. 

Đối với đầu vào`8`, vòng lặp tạo ra số dư`[0, 0, 0, 1]`. Đảo ngược mang lại`1000`. Điều này chứng tỏ rằng các số 0 ở cuối ở dạng nhị phân được bảo toàn chính xác và không bị loại bỏ sớm.
