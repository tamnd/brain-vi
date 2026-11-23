---
title: "CF 470H - Sắp xếp mảng"
description: "Chúng tôi được cung cấp một mảng các số nguyên. Số đầu tiên trong dữ liệu đầu vào là kích thước mảng n và n số nguyên tiếp theo là các phần tử mảng. Nhiệm vụ đơn giản là xuất ra các phần tử giống nhau được sắp xếp theo thứ tự không giảm."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 470
codeforces_index: "H"
codeforces_contest_name: "Surprise Language Round 7"
rating: 2300
weight: 470
solve_time_s: 78
verified: true
draft: false
---

[CF 470H - Sắp xếp mảng](https://codeforces.com/problemset/problem/470/H) 

**Đánh giá:** 2300 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên. Số đầu tiên trong đầu vào là kích thước mảng`n`, và tiếp theo`n`số nguyên là các phần tử mảng. Nhiệm vụ đơn giản là xuất ra các phần tử giống nhau được sắp xếp theo thứ tự không giảm. 

Mặc dù vấn đề này xuất hiện như một vấn đề đùa liên quan đến ngôn ngữ lập trình FALSE, nhưng từ góc độ thuật toán, nó chỉ là một nhiệm vụ sắp xếp. 

Những hạn chế là cực kỳ nhỏ. Mảng chứa tối đa 10 phần tử và mỗi giá trị nằm trong khoảng từ 1 đến 100. Chỉ với 10 số, ngay cả các phương pháp sắp xếp rất kém hiệu quả cũng đủ nhanh. Một thuật toán có độ phức tạp bậc hai thực hiện tối đa khoảng 100 phép so sánh, con số này không đáng kể. Việc sắp xếp thư viện tiêu chuẩn cũng dễ dàng đủ. 

Thách thức chính thực sự là phân tích cú pháp đầu vào một cách chính xác. Toàn bộ thông tin đầu vào được cung cấp trên một dòng duy nhất, do đó, giải pháp yêu cầu nhiều dòng có thể bị lỗi nếu được viết bất cẩn. 

Trường hợp đặc biệt phổ biến là khi mảng đã xuất hiện theo thứ tự được sắp xếp. 

đầu vào:```
4 1 2 3 4
```Đầu ra đúng:```
1 2 3 4
```Thuật toán sẽ không thay đổi thứ tự sau khi sắp xếp. 

Một trường hợp cạnh khác là khi tất cả các giá trị đều bằng nhau. 

đầu vào:```
5 7 7 7 7 7
```Đầu ra đúng:```
7 7 7 7 7
```Một số cách triển khai không chính xác vô tình xóa các bản sao bằng cách sử dụng một tập hợp trước khi sắp xếp. 

Trường hợp thứ ba là kích thước tối thiểu được phép. 

đầu vào:```
1 42
```Đầu ra đúng:```
42
```Giải pháp phải xử lý mảng một phần tử mà không cố gắng truy cập không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là phân loại vũ phu. Vì mảng chứa tối đa mười phần tử nên chúng ta có thể liên tục tìm giá trị nhỏ nhất còn lại hoặc sử dụng thuật toán bậc hai như sắp xếp bong bóng hoặc sắp xếp lựa chọn. Những phương pháp như vậy là đúng vì chúng dần dần di chuyển các phần tử vào vị trí được sắp xếp thích hợp. Vì`n = 10`, thậm chí là một`O(n²)`thuật toán chỉ thực hiện khoảng một trăm phép so sánh. 

Cải tiến tự nhiên là sử dụng quy trình sắp xếp có sẵn của ngôn ngữ. của Python`sort()`Và`sorted()`các hàm được tối ưu hóa cao và đảm bảo tạo ra mảng theo thứ tự không giảm. Độ phức tạp tiệm cận của chúng là`O(n log n)`, mặc dù với kích thước đầu vào nhỏ như vậy thì độ phức tạp chính xác là không liên quan. 

Cách tiếp cận brute-force hoạt động hiệu quả vì đầu vào rất nhỏ nhưng việc sắp xếp tích hợp lại đơn giản hơn, ngắn hơn và ít xảy ra lỗi hơn. Vì tác vụ chỉ yêu cầu một mảng được sắp xếp và không áp đặt thêm ràng buộc nào nên sử dụng hàm sắp xếp tiêu chuẩn là giải pháp rõ ràng nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (Sắp xếp bong bóng/lựa chọn) | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu (Sắp xếp tích hợp) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các số nguyên từ đầu vào. 
2. Trích xuất giá trị đầu tiên dưới dạng`n`. 
3. Đi tiếp theo`n`các giá trị như mảng. 
4. Sắp xếp mảng theo thứ tự không giảm. 
5. Xuất ra các giá trị được sắp xếp cách nhau bằng dấu cách. 

Bước sắp xếp trực tiếp tạo ra thứ tự cần thiết nên không cần xử lý thêm. 

### Tại sao nó hoạt động 

Sắp xếp sắp xếp lại các phần tử sao cho mỗi cặp vị trí liền kề`i`Và`i + 1`, điều kiện`a[i] ≤ a[i + 1]`nắm giữ. Vì một thuật toán sắp xếp đúng sẽ bảo toàn tất cả các phần tử và sắp xếp chúng theo thứ tự này, nên chuỗi kết quả chính xác là sự sắp xếp không giảm mà bài toán yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = list(map(int, sys.stdin.read().split()))
    if not data:
        return

    n = data[0]
    arr = data[1:1 + n]

    arr.sort()

    print(*arr)

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả các số nguyên từ đầu vào tiêu chuẩn và lưu trữ chúng trong danh sách. Số đầu tiên là kích thước mảng và số tiếp theo`n`các số tạo thành chính mảng đó. 

Cuộc gọi tới`arr.sort()`sắp xếp lại các phần tử theo thứ tự không giảm. Việc triển khai sắp xếp tích hợp sẵn của Python hoàn toàn đủ cho các giới hạn nhỏ trong bài toán này. 

Khi in,`print(*arr)`tự động đặt khoảng trắng giữa các phần tử, khớp với định dạng đầu ra được yêu cầu. 

Một điểm tinh tế là cắt lát chính xác`n`giá trị với`data[1:1 + n]`. Điều này tuân theo đặc điểm kỹ thuật đầu vào một cách chính xác và tránh việc vô tình bao gồm các giá trị bổ sung nếu có đầu vào không đúng định dạng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3 1 2
```| Bước | n | Mảng | 
| --- | --- | --- | 
| Đọc đầu vào | 3 | [3, 1, 2] | 
| Sắp xếp | 3 | [1, 2, 3] | 
| Đầu ra | 3 | [1, 2, 3] | 

Mảng được sắp xếp lại theo thứ tự tăng dần, đó chính xác là kết quả cần thiết. 

### Ví dụ 2 

đầu vào:```
5 7 7 3 1 7
```| Bước | n | Mảng | 
| --- | --- | --- | 
| Đọc đầu vào | 5 | [7, 7, 3, 1, 7] | 
| Sắp xếp | 5 | [1, 3, 7, 7, 7] | 
| Đầu ra | 5 | [1, 3, 7, 7, 7] | 

Ví dụ này chứng minh rằng các bản sao được bảo tồn. Việc sắp xếp chỉ thay đổi thứ tự chứ không thay đổi bội số của giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Tính năng sắp xếp tích hợp chi phối thời gian chạy | 
| Không gian | O(n) | Việc triển khai sắp xếp của Python có thể sử dụng bộ nhớ phụ | 

Với`n ≤ 10`, thời gian chạy thực sự là tức thời. Cả thời gian và mức sử dụng bộ nhớ đều thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    data = list(map(int, sys.stdin.read().split()))
    n = data[0]
    arr = data[1:1 + n]
    arr.sort()

    return " ".join(map(str, arr))

# provided sample
assert run("3 3 1 2\n") == "1 2 3", "sample 1"

# minimum size
assert run("1 42\n") == "42", "single element"

# already sorted
assert run("4 1 2 3 4\n") == "1 2 3 4", "already sorted"

# all equal
assert run("5 7 7 7 7 7\n") == "7 7 7 7 7", "duplicates preserved"

# maximum size
assert run("10 10 9 8 7 6 5 4 3 2 1\n") == "1 2 3 4 5 6 7 8 9 10", "reverse order"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 42`|`42`| Kích thước mảng tối thiểu | 
|`4 1 2 3 4`|`1 2 3 4`| Đã sắp xếp đầu vào | 
|`5 7 7 7 7 7`|`7 7 7 7 7`| Bảo quản trùng lặp | 
|`10 10 9 8 7 6 5 4 3 2 1`|`1 2 3 4 5 6 7 8 9 10`| Mảng đảo ngược thứ tự và kích thước tối đa | 

## Vỏ cạnh 

Hãy xem xét đầu vào kích thước tối thiểu:```
1 42
```Thuật toán đọc`n = 1`và trích xuất mảng`[42]`. Việc sắp xếp một mảng một phần tử sẽ không thay đổi. Đầu ra là:```
42
```Không cần xử lý đặc biệt. 

Hãy xem xét một mảng trong đó tất cả các giá trị giống hệt nhau:```
5 7 7 7 7 7
```Mảng được trích xuất là`[7, 7, 7, 7, 7]`. Sau khi sắp xếp, mảng vẫn giữ nguyên như cũ. Đầu ra là:```
7 7 7 7 7
```Điều này xác nhận rằng các bản sao được bảo tồn. 

Hãy xem xét một mảng đã được sắp xếp:```
4 1 2 3 4
```Thuật toán đọc`[1, 2, 3, 4]`, thực hiện sắp xếp và thu được trình tự tương tự. Đầu ra là:```
1 2 3 4
```Điều này chứng tỏ thuật toán không làm xáo trộn thứ tự đúng mà vẫn cho kết quả như yêu cầu.
