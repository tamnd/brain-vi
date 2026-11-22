---
title: "CF 339A - Toán hữu ích"
description: "Đầu vào là một chuỗi biểu thị một tổng trong đó mỗi số là 1, 2 hoặc 3 và các số được phân tách bằng dấu cộng. Ví dụ: chuỗi 3+2+1+3 đại diện cho bốn lệnh triệu tập. Xenia chỉ có thể đánh giá biểu thức nếu các số xuất hiện theo thứ tự không giảm."
date: "2026-06-06T17:14:15+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings", "strings"]
categories: ["algorithms"]
codeforces_contest: 339
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 197 (Div. 2)"
rating: 800
weight: 339
solve_time_s: 84
verified: true
draft: false
---

[CF 339A - Toán học hữu ích](https://codeforces.com/problemset/problem/339/A) 

**Đánh giá:** 800 
**Tags:** tham lam, triển khai, sắp xếp, chuỗi 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào là một chuỗi biểu thị một tổng trong đó mọi số đều là`1`,`2`, hoặc`3`và các số cách nhau bằng dấu cộng. Ví dụ, chuỗi`3+2+1+3`đại diện cho bốn lời triệu tập. 

Xenia chỉ có thể đánh giá biểu thức nếu các số xuất hiện theo thứ tự không giảm. Nhiệm vụ của chúng ta không phải là tính tổng. Thay vào đó, chúng ta phải sắp xếp lại các số hiện có sao cho tất cả`1`hãy đến trước, sau đó là tất cả`2`s, vậy thì tất cả`3`s và in biểu thức kết quả theo cùng một cách`a+b+c`định dạng. 

Độ dài đầu vào tối đa là 100 ký tự. Vì mỗi số chiếm một ký tự và mỗi dấu phân cách chiếm một ký tự nên có nhiều nhất khoảng 50 số. Hạn chế nhỏ như vậy có nghĩa là hầu hết mọi cách tiếp cận hợp lý đều đủ nhanh. Ngay cả giải pháp dựa trên sắp xếp cũng có thể chạy ngay lập tức. 

Thách thức chính là xử lý định dạng chuỗi một cách chính xác. Chúng ta cần sắp xếp các số chứ không phải toàn bộ chuỗi. Việc triển khai bất cẩn sắp xếp tất cả các ký tự sẽ trộn lẫn các chữ số và dấu cộng với nhau và tạo ra biểu thức không hợp lệ. 

Trường hợp một cạnh là khi biểu thức chỉ chứa một số duy nhất. 

đầu vào:```
2
```Đầu ra:```
2
```Không có dấu cộng nào để chèn ngoài cấu trúc ban đầu. Việc tách và nối vẫn hoạt động chính xác. 

Một trường hợp đặc biệt khác là khi tất cả các số đã được sắp xếp. 

đầu vào:```
1+1+2+3
```Đầu ra:```
1+1+2+3
```Thuật toán nên giữ nguyên biểu thức. 

Một trường hợp khác là khi tất cả các số đều giống hệt nhau. 

đầu vào:```
3+3+3
```Đầu ra:```
3+3+3
```Việc sắp xếp không được làm thay đổi kết quả và các giá trị trùng lặp phải được giữ nguyên. 

## Phương pháp tiếp cận 

Một tư duy vũ phu sẽ là tạo ra mọi thứ tự có thể có của các số, kiểm tra xem thứ tự đó có giảm không và in thứ tự hợp lệ đầu tiên. Điều này có hiệu quả vì câu trả lời đúng chỉ đơn giản là một trong những hoán vị của các lệnh đã cho. 

Vấn đề là hoán vị tăng theo giai thừa. Nếu có 50 số thì có`50!`những thứ tự có thể xảy ra, có quy mô lớn về mặt thiên văn và hoàn toàn không khả thi. 

Cấu trúc của vấn đề đưa ra một quan sát đơn giản hơn nhiều. Các giá trị duy nhất xuất hiện là`1`,`2`, Và`3`, và Xenia muốn chúng theo thứ tự không giảm. Một dãy không giảm chính xác khi tất cả các giá trị nhỏ hơn xuất hiện trước các giá trị lớn hơn. Điều đó có nghĩa là chúng ta không cần phải tìm kiếm giữa các hoán vị. Chúng ta có thể trích xuất các số, sắp xếp chúng và xây dựng lại biểu thức. 

Vì việc sắp xếp trực tiếp tạo ra sự sắp xếp không giảm duy nhất nên bài toán trở thành một nhiệm vụ thực hiện đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào. 
2. Tách chuỗi bằng cách sử dụng`'+'`như dấu phân cách. 

Điều này trích xuất tất cả các số dưới dạng các chuỗi riêng biệt, chẳng hạn như`["3", "2", "1"]`. 
3. Sắp xếp danh sách kết quả. 

Sắp xếp tất cả các địa điểm`1`đầu tiên, sau đó là tất cả`2`s, vậy thì tất cả`3`s, đó chính xác là thứ tự được yêu cầu. 
4. Nối các số đã sắp xếp lại với nhau bằng cách sử dụng`'+'`. 

Điều này xây dựng lại một biểu thức hợp lệ ở định dạng ban đầu. 
5. In kết quả. 

### Tại sao nó hoạt động 

Biểu thức chỉ bao gồm các số`1`,`2`, Và`3`. Một câu trả lời hợp lệ phải chứa chính xác nhiều bộ số giống như đầu vào, chỉ được sắp xếp lại. 

Việc sắp xếp các số được trích xuất sẽ tạo ra một chuỗi trong đó mọi phần tử đều nhỏ hơn hoặc bằng phần tử tiếp theo. Đó chính xác là định nghĩa của thứ tự không giảm. Vì việc sắp xếp không loại bỏ hay sao chép bất kỳ số nào nên biểu thức thu được chứa chính xác các lệnh tính ban đầu và đáp ứng yêu cầu của Xenia. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

numbers = s.split('+')
numbers.sort()

print('+'.join(numbers))
```Bước đầu tiên đọc biểu thức dưới dạng một chuỗi. sử dụng`strip()`loại bỏ ký tự dòng mới ở cuối. 

Cuộc gọi tới`split('+')`là hoạt động phân tích cú pháp chính. Thay vì xử lý các ký tự riêng lẻ, nó trích xuất trực tiếp các số dưới dạng các phần tử riêng biệt. 

Danh sách sau đó được sắp xếp theo từ điển. Vì các giá trị duy nhất có thể là`"1"`,`"2"`, Và`"3"`, thứ tự từ điển khớp với thứ tự số. 

Cuối cùng,`'+'.join(numbers)`xây dựng lại biểu thức với các dấu phân cách cần thiết. Điều này tự động xử lý cả biểu thức số đơn và nhiều số mà không có bất kỳ trường hợp đặc biệt nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3+2+1
```| Bước | Số | 
| --- | --- | 
| Sau khi chia tay |`["3", "2", "1"]`| 
| Sau khi sắp xếp |`["1", "2", "3"]`| 
| Sau khi tham gia |`"1+2+3"`| 

Đầu ra:```
1+2+3
```Ví dụ này cho thấy ý tưởng cốt lõi. Các số được trích xuất, sắp xếp và xây dựng lại theo thứ tự yêu cầu. 

### Ví dụ 2 

đầu vào:```
1+3+1+2
```| Bước | Số | 
| --- | --- | 
| Sau khi chia tay |`["1", "3", "1", "2"]`| 
| Sau khi sắp xếp |`["1", "1", "2", "3"]`| 
| Sau khi tham gia |`"1+1+2+3"`| 

Đầu ra:```
1+1+2+3
```Ví dụ này chứng minh rằng các giá trị trùng lặp được giữ nguyên. Sắp xếp các nhóm có giá trị bằng nhau trong khi vẫn duy trì số đếm chính xác của từng số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các số được trích xuất chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n) | Thao tác phân chia lưu trữ các số trong danh sách | 

Độ dài đầu vào tối đa là 100 ký tự nên ngay cả bước sắp xếp cũng chỉ xử lý được vài chục số. Thời gian chạy và mức sử dụng bộ nhớ thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    s = input().strip()
    numbers = s.split('+')
    numbers.sort()
    print('+'.join(numbers))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
assert run("3+2+1\n") == "1+2+3", "sample 1"

# minimum-size input
assert run("1\n") == "1", "single number"

# already sorted
assert run("1+1+2+3\n") == "1+1+2+3", "already sorted"

# all equal values
assert run("3+3+3\n") == "3+3+3", "all equal"

# reverse order
assert run("3+3+2+2+1+1\n") == "1+1+2+2+3+3", "full sorting"

# mixed duplicates
assert run("2+1+3+1+2\n") == "1+1+2+2+3", "duplicates preserved"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Đầu vào kích thước tối thiểu | 
|`1+1+2+3`|`1+1+2+3`| Trình tự đã được sắp xếp | 
|`3+3+3`|`3+3+3`| Giá trị hoàn toàn bằng nhau | 
|`3+3+2+2+1+1`|`1+1+2+2+3+3`| Hoàn thành sắp xếp lại | 
|`2+1+3+1+2`|`1+1+2+2+3`| Xử lý trùng lặp | 

## Vỏ cạnh 

Hãy xem xét đầu vào một số:```
2
```Thuật toán thực hiện:```
"2".split('+')
```sản xuất:```
["2"]
```Việc sắp xếp không làm gì cả và việc tham gia trả về:```
2
```Đầu ra vẫn chính xác mà không cần xử lý đặc biệt. 

Hãy xem xét một biểu thức đã được sắp xếp:```
1+1+2+3
```Sau khi tách:```
["1", "1", "2", "3"]
```Sắp xếp khiến danh sách không thay đổi. Đầu ra cuối cùng giống hệt với đầu vào, đó là hành vi mong muốn. 

Hãy xem xét một biểu thức chỉ chứa các giá trị giống hệt nhau:```
3+3+3
```Việc chia tách mang lại:```
["3", "3", "3"]
```Sắp xếp bảo tồn cả ba lần xuất hiện. Tham gia xây dựng lại:```
3+3+3
```Điều này xác nhận rằng thuật toán không bao giờ mất hoặc trùng lặp các lệnh triệu tập và luôn bảo toàn nhiều tập hợp số ban đầu.
