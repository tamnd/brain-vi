---
title: "CF 106157I - Bits Itsy"
description: "Nhiệm vụ là chọn kích thước của đơn vị lưu trữ cho một số nguyên không dấu. Hệ thống biết giá trị lớn nhất sẽ được lưu trữ và nó muốn dự trữ một số bit tuân theo quy tắc phần cứng: bản thân số bit đó phải là lũy thừa của hai."
date: "2026-06-25T11:19:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106157
codeforces_index: "I"
codeforces_contest_name: "2025 United Kingdom and Ireland Programming Contest (UKIEPC 2025)"
rating: 0
weight: 106157
solve_time_s: 25
verified: true
draft: false
---

[CF 106157I - Itsy Bits](https://codeforces.com/problemset/problem/106157/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là chọn kích thước của đơn vị lưu trữ cho một số nguyên không dấu. Hệ thống biết giá trị lớn nhất sẽ được lưu trữ và nó muốn dự trữ một số bit tuân theo quy tắc phần cứng: bản thân số bit đó phải là lũy thừa của hai. Do đó, kích thước lưu trữ hợp lệ là 1, 2, 4, 8, 16, 32, 64, v.v. 

Đầu vào là một số nguyên biểu thị giá trị tối đa phải vừa trong bộ nhớ. Đầu ra là kích thước bit hợp lệ nhỏ nhất có thể lưu trữ giá trị đó, theo sau là từ ngữ số ít hoặc số nhiều chính xác. 

Một thiết bị lưu trữ có`b`bit có thể đại diện cho các giá trị từ`0`ĐẾN`2^b - 1`. Bởi vì đầu vào có thể lớn bằng`10^18`, câu trả lời phải được tìm thấy mà không cần lặp qua tất cả các giá trị có thể có cho đến đầu vào. Bất kỳ cách tiếp cận nào với công việc tỷ lệ thuận với`n`sẽ yêu cầu khoảng một triệu tỷ thao tác trong trường hợp xấu nhất, vượt xa giới hạn thời gian cuộc thi thông thường cho phép. Chúng ta chỉ cần kiểm tra tập hợp nhỏ các kích thước bit có thể có. 

Các trường hợp đặc biệt xuất phát từ ranh giới giữa hai kích thước lưu trữ. Ví dụ, đầu vào`1`phải sản xuất`1 bit`, bởi vì một bit có thể lưu trữ các giá trị`0`Và`1`. Một giải pháp bắt đầu kiểm tra từ hai bit sẽ xuất ra không chính xác`2 bits`. 

Một trường hợp biên khác là giá trị chính xác bằng dung lượng lưu trữ trừ đi một. Đối với đầu vào`255`, đầu ra đúng là`8 bits`, bởi vì tám bit lưu trữ các giá trị từ`0`bởi vì`255`. Một giải pháp bất cẩn để kiểm tra xem`n < 255`thay vì`n <= 255`sẽ di chuyển không chính xác đến`16 bits`. 

Trường hợp quan trọng khác là khi giá trị vượt quá ranh giới lũy thừa hai chỉ một. Đối với đầu vào`256`, tám bit không còn đủ vì giá trị tối đa của chúng là`255`. Câu trả lời trở thành`16 bits`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử mọi số bit có thể có, bắt đầu từ một bit và kiểm tra xem liệu nhiều bit đó có thể lưu trữ giá trị đã cho hay không. Kiểm tra quy mô ứng viên`b`đơn giản là liệu`2^b - 1`ít nhất là giá trị tối đa được yêu cầu. Phương pháp này đúng vì nó kiểm tra kích thước theo thứ tự tăng dần và dừng ở kích thước hợp lệ đầu tiên. 

Vấn đề với cách tiếp cận này phụ thuộc vào cách tạo ra các ứng viên. Nếu chúng ta tưởng tượng việc kiểm tra mọi số bit có thể có từ`1`lên tới giá trị đầu vào, trường hợp đầu vào xấu nhất gần`10^18`sẽ yêu cầu về`10^18`kiểm tra, điều đó là không thể. Quan sát hữu ích là chỉ có lũy thừa của hai mới được phép làm câu trả lời. Chỉ có một số ít giá trị như vậy trước khi vượt quá đầu vào lớn nhất có thể. Chúng ta có thể nhân đôi câu trả lời hiện tại nhiều lần cho đến khi dung lượng đủ lớn. 

Ý tưởng vũ phu hoạt động vì câu trả lời là kích thước hợp lệ đầu tiên. Việc quan sát thấy các kích thước hợp lệ tạo thành một chuỗi hình học nhỏ cho phép chúng ta bỏ qua tất cả các kích thước không hợp lệ giữa chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Tối ưu | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với kích thước lưu trữ là`1`chút. Đây là mức phân bổ nhỏ nhất có thể và phải được xem xét vì số lượng rất nhỏ có thể phù hợp ở đó. 
2. Kiểm tra xem số bit này có thể lưu trữ giá trị tối đa được yêu cầu hay không. Một kích thước của`bits`hoạt động khi`2^bits - 1`ít nhất là giá trị đầu vào. 
3. Nếu kích thước hiện tại quá nhỏ, hãy tăng gấp đôi. Điều này sẽ chuyển sang kích thước lưu trữ hợp lệ tiếp theo vì tất cả các kích thước được phép đều là lũy thừa của hai. 
4. Tiếp tục cho đến khi dung lượng đủ lớn. Kích thước đầu tiên hoạt động là câu trả lời bắt buộc. 
5. In kích thước với hậu tố chính xác. Trường hợp duy nhất là`1`, trong đó đầu ra sẽ chứa`bit`; mọi kích thước khác đều sử dụng`bits`. 

Tại sao nó hoạt động: thuật toán chỉ xem xét kích thước lưu trữ hợp lệ và kiểm tra chúng theo thứ tự tăng dần. Vì câu trả lời được xác định là kích thước hợp lệ nhỏ nhất nên kích thước đầu tiên có dung lượng đạt giá trị yêu cầu phải là lựa chọn tối ưu. Không có giá trị bị bỏ qua có thể là câu trả lời tốt hơn vì tất cả các giá trị bị bỏ qua đều không được phép có kích thước lưu trữ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    bits = 1
    while (1 << bits) - 1 < n:
        bits *= 2

    if bits == 1:
        print("1 bit")
    else:
        print(f"{bits} bits")

if __name__ == "__main__":
    solve()
```Biến`bits`lưu trữ số bit hiện tại được phân bổ. Nó bắt đầu lúc`1`bởi vì lưu trữ một bit là một khả năng hợp lệ. 

Điều kiện vòng lặp kiểm tra giá trị lớn nhất được biểu thị bằng kích thước lưu trữ hiện tại. biểu hiện`(1 << bits) - 1`chính xác là`2^bits - 1`, giúp tránh các phép tính dấu phẩy động và giữ tất cả các phép toán dưới dạng số học số nguyên. 

nhân`bits`bằng hai là bước quan trọng. Câu trả lời không được phép là một con số tùy ý như`5`hoặc`12`, do đó việc tăng thêm một đơn vị sẽ lãng phí thời gian kiểm tra các ứng viên không hợp lệ. Nhân đôi bước nhảy trực tiếp giữa các kích thước lưu trữ hợp lệ. 

Điều kiện cuối cùng xử lý ngoại lệ ngữ pháp duy nhất. Kích thước lưu trữ`1`sử dụng từ số ít, trong khi tất cả các kích thước lớn hơn đều sử dụng dạng số nhiều. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
1
```| Bước | bit | Công suất | 
| --- | --- | --- | 
| Bắt đầu | 1 | 1 | 

Dung lượng của một bit là đủ vì nó có thể biểu diễn các giá trị từ`0`ĐẾN`1`. Thuật toán dừng ngay lập tức và in`1 bit`. Điều này chứng tỏ trường hợp nhỏ nhất có thể xảy ra. 

Đối với đầu vào:```
1100586419201
```| Bước | bit | Công suất | 
| --- | --- | --- | 
| Bắt đầu | 1 | 1 | 
| Đôi | 2 | 3 | 
| Đôi | 4 | 15 | 
| Đôi | 8 | 255 | 
| Đôi | 16 | 65535 | 
| Đôi | 32 | 4294967295 | 
| Đôi | 64 | 18446744073709551615 | 

Một số kích thước lưu trữ đầu tiên quá nhỏ. Bộ lưu trữ 64 bit là kích thước được phép đầu tiên có dung lượng đạt tới đầu vào, do đó đầu ra là`64 bits`. Dấu vết này cho thấy tại sao câu trả lời không thể đơn giản là số nguyên bit tiếp theo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Số lần nhân đôi tỷ lệ thuận với số bit cần thiết để biểu diễn đầu vào. | 
| Không gian | O(1) | Chỉ có kích thước ứng viên hiện tại được lưu trữ. | 

Giới hạn đầu vào của`10^18`chỉ yêu cầu kiểm tra một số lượng nhỏ lũy thừa của hai. Thuật toán thực hiện ít thao tác hơn nhiều so với quét tuyến tính và dễ dàng phù hợp với giới hạn của cuộc thi. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    bits = 1
    while (1 << bits) - 1 < n:
        bits *= 2

    return f"{bits} {'bit' if bits == 1 else 'bits'}"

# provided samples
assert solve_case("1\n") == "1 bit", "sample 1"
assert solve_case("37\n") == "8 bits", "sample 2"
assert solve_case("1100586419201\n") == "64 bits", "sample 3"

# custom cases
assert solve_case("0\n") == "1 bit", "zero still fits in one bit"
assert solve_case("255\n") == "8 bits", "maximum value of 8 bits"
assert solve_case("256\n") == "16 bits", "just above 8 bit capacity"
assert solve_case("18446744073709551615\n") == "64 bits", "largest 64 bit value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`1 bit`| Kích thước lưu trữ tối thiểu và ranh giới dưới | 
|`255`|`8 bits`| Dung lượng tối đa chính xác của kích thước hợp lệ | 
|`256`|`16 bits`| Chuyển sang kích thước lũy thừa hai tiếp theo | 
|`18446744073709551615`|`64 bits`| Xử lý giá trị lớn | 

## Vỏ cạnh 

Đối với đầu vào:```
1
```Thuật toán bắt đầu với`bits = 1`. Việc tính toán công suất cho`2^1 - 1 = 1`, thế là đủ nên vòng lặp được bỏ qua. Đầu ra là`1 bit`, xử lý dạng số ít một cách chính xác. 

Đối với đầu vào:```
255
```Các thử nghiệm thuật toán`8`bit và tìm thấy rằng`2^8 - 1 = 255`. Vì phép so sánh mang tính bao hàm nên giá trị khớp chính xác và câu trả lời vẫn giữ nguyên`8 bits`. 

Đối với đầu vào:```
256
```Thuật toán tìm thấy rằng`8`bit cung cấp dung lượng`255`, điều đó là không đủ. Nó tăng gấp đôi kích thước để`16`, công suất ở đâu`65535`và đầu ra`16 bits`. 

Đối với đầu vào:```
1100586419201
```Thuật toán tiếp tục tăng gấp đôi thông qua kích thước lưu trữ được phép. Giá trị từ`1`bởi vì`32`bit không thể đại diện cho đầu vào, trong khi`64`bit có thể. Vì mọi kích thước hợp lệ nhỏ hơn đều bị từ chối trước tiên nên kết quả được đảm bảo là tối thiểu.
