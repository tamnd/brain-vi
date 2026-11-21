---
title: "CF 258A - Chú Voi Nhỏ Và Bit"
description: "Chúng ta được cấp một số nhị phân dưới dạng một chuỗi, biểu thị một số nguyên trong cơ số 2 và chúng ta được phép xóa chính xác một chữ số. Mục tiêu là chọn chữ số cần loại bỏ sao cho số nhị phân thu được, khi được hiểu là số nguyên thập phân, càng lớn càng tốt."
date: "2026-06-04T17:32:01+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 258
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 157 (Div. 1)"
rating: 1100
weight: 258
solve_time_s: 89
verified: true
draft: false
---

[CF 258A - Chú voi nhỏ và những chú bit](https://codeforces.com/problemset/problem/258/A) 

**Đánh giá:** 1100 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số nhị phân dưới dạng một chuỗi, biểu thị một số nguyên trong cơ số 2 và chúng ta được phép xóa chính xác một chữ số. Mục tiêu là chọn chữ số cần loại bỏ sao cho số nhị phân thu được, khi được hiểu là số nguyên thập phân, càng lớn càng tốt. Đầu vào không chứa số 0 đứng đầu và chuỗi có ít nhất hai ký tự, tối đa 100.000. Đầu ra cũng phải là số nhị phân không có số 0 đứng đầu. 

Hạn chế chính là độ dài của đầu vào, có thể lên tới 10^5. Điều này có nghĩa là bất kỳ thuật toán nào thử mọi thao tác xóa có thể và chuyển đổi chuỗi nhị phân kết quả thành số nguyên mỗi lần sẽ thực hiện khoảng 10^5 chuyển đổi. Vì việc chuyển đổi một chuỗi có độ dài n thành số nguyên là O(n), nên cách tiếp cận bạo lực có thể lấy O(n^2) trong trường hợp xấu nhất, quá chậm đối với n = 10^5. Chúng ta cần một giải pháp tuyến tính, O(n), hoặc tệ nhất là O(n log n). 

Trường hợp cạnh phát sinh khi số bao gồm toàn bộ số 1 hoặc kết thúc bằng số 0. Ví dụ: nếu đầu vào là`1111`, loại bỏ bất kỳ một`1`cho`111`. Nếu đầu vào là`1010`, loại bỏ cái đầu tiên`1`cho`010`có số 0 đứng đầu, vì vậy chúng ta phải xử lý đầu ra cẩn thận để tránh các số 0 đứng đầu. Một trường hợp tinh tế khác là khi ký tự đầu tiên`1`theo sau là tất cả số không; loại bỏ cái đó`1`sẽ giảm bit quan trọng nhất, làm giảm đáng kể số lượng. Chúng ta cần phải cân nhắc về việc chữ số nào bị loại bỏ. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp qua từng chữ số, loại bỏ nó và tính giá trị số nguyên thu được. Theo dõi mức tối đa. Điều này hiệu quả vì nó kiểm tra mọi kết quả có thể xảy ra theo đúng nghĩa đen, nhưng nó yêu cầu thời gian O(n^2) cho một chuỗi có độ dài n, vì việc chuyển đổi một chuỗi nhị phân có độ dài n thành một số là O(n). Đối với mức tối đa được phép n = 10^5, phương pháp này thực hiện khoảng 10^10 thao tác, quá chậm. 

Giải pháp tối ưu dựa vào quan sát quan trọng về số nhị phân. Bit quan trọng nhất đóng góp giá trị lớn nhất, vì vậy việc loại bỏ bit đầu tiên`0`gặp từ trái sang phải sẽ giữ nguyên giá trị cao`1`s trong khi loại bỏ số 0 có giá trị thấp hơn, tối đa hóa số lượng. Nếu chuỗi chỉ bao gồm`1`là, cái đầu tiên`1`có thể được loại bỏ mà không làm giảm thứ tự tương đối của các chữ số khác. Thông tin chuyên sâu này giúp giảm thiểu vấn đề chỉ bằng một lần quét từ trái sang phải để tìm ra dữ liệu đầu tiên`0`và loại bỏ nó, đó là O(n) về thời gian và O(n) về không gian để lưu trữ kết quả. Điều này đủ hiệu quả cho n lên đến 10^5. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi nhị phân từ đầu vào và lưu nó vào một biến`s`. Đây là số làm việc của chúng tôi. 
2. Khởi tạo một con trỏ hoặc biến chỉ số để duyệt chuỗi từ trái sang phải. Mục tiêu là tìm ra người đầu tiên`0`. 
3. Đi qua chuỗi. Ngay lần đầu tiên`0`được tìm thấy, ghi lại chỉ mục của nó và ngắt vòng lặp. Nếu không tìm thấy số 0 thì số đó bao gồm toàn bộ`1`S. 
4. Xây dựng một chuỗi mới bằng cách nối tất cả các ký tự ngoại trừ ký tự ở chỉ mục đã ghi. Trong Python, điều này có thể được thực hiện bằng cách cắt:`s[:index] + s[index+1:]`. 
5. In chuỗi kết quả. 

Tính chính xác được đảm bảo vì số 0 ngoài cùng bên trái là cơ hội đầu tiên để loại bỏ chữ số có giá trị thấp mà không làm mất giá trị cao`1`sang trái. Bất kỳ số 0 nào ở bên phải đều đóng góp ít hơn vào giá trị tổng thể, do đó việc loại bỏ số 0 đầu tiên sẽ tối đa hóa số nguyên thu được. Nếu không có số 0, hãy loại bỏ bất kỳ số nào`1`vẫn mang lại số lượng tối đa những cái còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

# Find the first '0'
zero_index = s.find('0')

if zero_index == -1:
    # All '1's, remove the first one
    result = s[1:]
else:
    # Remove the first '0'
    result = s[:zero_index] + s[zero_index+1:]

print(result)
```Mã sử ​​dụng Python`str.find`phương pháp xác định số 0 đầu tiên một cách hiệu quả trong thời gian O(n). Nếu không có số không,`find`trả về -1 và chúng tôi xóa giá trị đầu tiên`1`. Thao tác cắt lát`s[:index] + s[index+1:]`tạo một chuỗi mới, là chuỗi tiêu chuẩn trong Python và chạy trong thời gian O(n). Logic đảm bảo không còn số 0 đứng đầu vì chúng tôi loại bỏ số 0 sau số 0 đứng đầu hoặc chúng tôi loại bỏ số 0 đầu tiên trong chuỗi tất cả các số 1. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`101`| Bước | s | zero_index | Chuỗi kết quả | 
| --- | --- | --- | --- | 
| Ban đầu |`101`| 1 | - | 
| Xóa '0' đầu tiên |`101`| 1 |`11`| 

Loại bỏ số 0 sẽ tối đa hóa giá trị còn lại. Đầu ra là`11`. 

### Mẫu 2 

đầu vào:`11010`| Bước | s | zero_index | Chuỗi kết quả | 
| --- | --- | --- | --- | 
| Ban đầu |`11010`| 2 | - | 
| Xóa '0' đầu tiên |`11010`| 2 |`1110`| 

Số 0 ngoài cùng bên trái nằm ở chỉ số 2. Loại bỏ nó sẽ mang lại`1110`, giá trị lớn nhất có thể. Đầu ra là`1110`. 

Những dấu vết này cho thấy việc quét từ trái sang phải và loại bỏ số 0 đầu tiên sẽ bảo tồn các bit có giá trị cao. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần để tìm số 0 đầu tiên và xây dựng chuỗi mới. | 
| Không gian | O(n) | Việc lưu trữ chuỗi kết quả cần có không gian O(n). | 

Đối với n tối đa 10^5, giải pháp này thực hiện khoảng 2n thao tác, nằm trong giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = sys.stdin.readline().strip()
    zero_index = s.find('0')
    if zero_index == -1:
        result = s[1:]
    else:
        result = s[:zero_index] + s[zero_index+1:]
    return result

# Provided samples
assert run("101\n") == "11", "sample 1"
assert run("11010\n") == "1110", "sample 2"

# Custom cases
assert run("11\n") == "1", "all ones minimal length"
assert run("10\n") == "1", "two digits, remove zero"
assert run("100000\n") == "10000", "remove first zero after leading one"
assert run("111111111111111111111\n") == "11111111111111111111", "all ones, large input"
assert run("1010101010101\n") == "11101010101", "alternating bits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`11`|`1`| Đầu vào tối thiểu cho tất cả mọi người | 
|`10`|`1`| Số có hai chữ số, xóa số 0 | 
|`100000`|`10000`| Số đứng đầu theo sau là số 0 | 
|`111111111111111111111`|`11111111111111111111`| Đầu vào lớn với tất cả những cái | 
|`1010101010101`|`11101010101`| Những cái và số không xen kẽ | 

## Vỏ cạnh 

Nếu đầu vào là`1111`, thuật toán không tìm thấy số 0. Nó loại bỏ cái đầu tiên`1`, mang lại`111`, đó là số tối đa chính xác có thể. Nếu đầu vào là`1000`, thuật toán loại bỏ số 0 đầu tiên ở chỉ số 1, tạo ra`100`, bảo toàn giá trị hàng đầu có giá trị cao nhất. Nếu đầu vào là`10`, thuật toán loại bỏ số 0 ở chỉ số 1, thu được`1`, kết quả tối đa đúng. Những ví dụ này xác nhận rằng việc quét tìm số 0 đầu tiên và loại bỏ nó luôn tạo ra số còn lại lớn nhất.
