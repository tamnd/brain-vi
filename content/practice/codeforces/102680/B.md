---
title: "CF 102680B - Bút Apple"
description: "Sự cố mô tả thao tác có tên \"uh\". Nếu mục A trùng khớp với mục B, kết quả là một mục mới có tên là B-A, nghĩa là mục thứ hai được đặt trước mục đầu tiên có dấu gạch nối giữa chúng."
date: "2026-08-01T23:30:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102680
codeforces_index: "B"
codeforces_contest_name: "Brookfield Computer Programming Challenge 1"
rating: 0
weight: 102680
solve_time_s: 87
verified: true
draft: false
---

[CF 102680B - Bút Apple](https://codeforces.com/problemset/problem/102680/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Sự cố mô tả thao tác có tên "uh". Nếu một mục`A`đúng là có một món đồ`B`, kết quả là một mục mới có tên là`B-A`, nghĩa là mục thứ hai được đặt trước mục đầu tiên có dấu gạch nối giữa chúng. Nhiệm vụ là thực hiện thao tác này một cách độc lập trên các cặp mục nhất định và in tên mục kết quả theo thứ tự xuất hiện của các cặp. 

Đầu vào chứa số lượng thao tác, theo sau là`2*n`mô tả các mục. Cứ hai mô tả liên tiếp tạo thành một cặp. Mỗi mô tả có thêm các từ xung quanh tên mục thực tế, do đó việc triển khai chỉ phải trích xuất từ ​​cuối cùng từ mỗi dòng. Đầu ra phải chứa một câu được định dạng cho mỗi cặp, trong đó mục thứ hai đứng đầu trong tên được tạo. 

Các ràng buộc là nhỏ về số lượng mặt hàng. Có thể có tới 1000 thao tác và tên mục có thể chứa tối đa 1000 ký tự. Điều này có nghĩa là tổng số lượng văn bản có thể đạt khoảng hai triệu ký tự, vì vậy giải pháp chỉ nên xử lý mỗi ký tự với số lần không đổi. Bất kỳ cách tiếp cận nào liên tục quét hoặc xây dựng lại các chuỗi dài một cách không cần thiết đều có thể trở nên chậm hơn, nhưng việc truyền tuyến tính trực tiếp là đủ dễ dàng. 

Các trường hợp nguy hiểm chính là do lỗi phân tích cú pháp và sắp xếp chứ không phải do độ phức tạp của thuật toán. 

Hãy xem xét đầu vào:```
1
I have a Apple
I have a Pen
```Đầu ra đúng là:```
Uh! Pen-Apple!
```Việc thực hiện bất cẩn có thể giữ nguyên câu đầy đủ và tạo ra một cái gì đó như`Uh! I have a Pen-I have a Apple!`, vì dữ liệu có ý nghĩa chỉ là từ cuối cùng của mỗi dòng. 

Một trường hợp khác là khi một mục đã chứa dấu gạch nối:```
1
I have a Apple-Pen
I have a Pen-Pineapple
```Đầu ra đúng là:```
Uh! Pen-Pineapple-Apple-Pen!
```Dấu gạch nối hiện có bên trong mỗi mục phải không thay đổi. Việc tách tên mục bằng dấu gạch nối và nối lại tên mục đó là không cần thiết và có nguy cơ làm thay đổi văn bản gốc. 

## Phương pháp tiếp cận 

Cách giải thích brute-force là mô phỏng toàn bộ thao tác trong khi vẫn giữ tất cả các từ ở dạng câu ban đầu, sau đó tìm kiếm thủ công qua từng dòng để xác định vị trí mục. Cách tiếp cận này vẫn cho câu trả lời chính xác vì thông tin cần thiết duy nhất là tên mặt hàng nhưng nó lại thực hiện những công việc không cần thiết. Nếu được triển khai kém, việc tìm kiếm lặp đi lặp lại qua các chuỗi dài có thể khiến thời gian chạy tỷ lệ thuận với tổng độ dài của đầu vào nhân với số thao tác. 

Cấu trúc của đầu vào cho phép quan sát đơn giản hơn nhiều. Mỗi dòng luôn tuân theo cùng một định dạng và mục luôn là mã thông báo được phân tách bằng dấu cách cuối cùng. Vì mỗi thao tác chỉ cần hai mục lân cận nên không cần lưu trữ, so khớp hoặc bất kỳ cấu trúc dữ liệu phức tạp nào hơn. Chúng ta có thể trích xuất tên hai mục và in ngay kết quả. 

Phương pháp brute-force hoạt động vì cuối cùng nó phát hiện ra hai tên cần thiết cho thao tác, nhưng nó coi văn bản không liên quan như thể nó quan trọng. Quan sát chính là chỉ có từ cuối cùng của mỗi dòng tham gia cho phép chúng tôi giảm giải pháp xuống một lần chuyển qua đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * L) hoặc tệ hơn tùy thuộc vào phương pháp phân tích cú pháp | O(L) | Công việc không cần thiết | 
| Tối ưu | O(tổng chiều dài đầu vào) | O(1) bên cạnh bộ đệm đầu vào | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lần thực hiện. Mỗi thao tác sử dụng chính xác hai mô tả mục, do đó, dữ liệu đầu vào còn lại có thể được xử lý theo cặp liên tiếp. 
2. Đối với mỗi cặp dòng, chia mỗi dòng bằng dấu cách và lấy phần tử cuối cùng. Điều này hoạt động vì tên mục được đảm bảo là từ cuối cùng và không chứa dấu cách. 
3. In kết quả bằng cách sử dụng mục được trích xuất thứ hai trước, sau đó là dấu gạch nối, tiếp theo là mục được trích xuất đầu tiên. Điều này trực tiếp tuân theo định nghĩa của hoạt động uh. 
4. Tiếp tục cho đến khi tất cả các cặp đã được xử lý. 

Tại sao nó hoạt động: 

Thuật toán giữ lại hai thông tin duy nhất ảnh hưởng đến câu trả lời: tên mục đầu tiên và tên mục thứ hai. Định nghĩa hoạt động cho biết tên đầu ra chính xác là mục thứ hai, theo sau là mục đầu tiên được phân tách bằng dấu gạch nối. Vì mỗi dòng đầu vào có chính xác một mã thông báo liên quan ở cuối nên việc trích xuất các mã thông báo đó không thể làm mất bất kỳ thông tin bắt buộc nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    ans = []

    for _ in range(n):
        a = input().split()[-1]
        b = input().split()[-1]
        ans.append(f"Uh! {b}-{a}!")

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Chương trình đầu tiên đọc`n`, cho chúng ta biết có bao nhiêu cặp phải được xử lý. Vòng lặp chạy chính xác`n`lần, phù hợp với số lượng hoạt động uh. 

Đối với mỗi lần lặp, hai dòng đầu vào được chia thành các từ. Lấy chỉ số`-1`truy xuất từ ​​cuối cùng một cách an toàn, bất kể độ dài mục. Bản thân mục này có thể chứa dấu gạch nối nhưng điều đó không ảnh hưởng đến`split()`vì dấu phân cách là khoảng trắng. 

Thứ tự của các biến quan trọng. Mục được trích xuất đầu tiên là phần bên trái của thao tác, trong khi mục được trích xuất thứ hai trở thành phần đầu của câu trả lời. Hoán đổi chúng sẽ đảo ngược kết quả cần thiết. 

Các câu trả lời được thu thập và in cùng nhau ở cuối. Điều này tránh các lệnh gọi đầu ra lặp lại và giữ cho chương trình hoạt động hiệu quả ở kích thước đầu vào tối đa. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
2
I have a Apple
I have a Pen
I have a Apple-Pen
I have a Pen-Pineapple
```Dấu vết là: 

| Bước | Mục đầu tiên | Mục thứ hai | Sản lượng sản xuất | 
| --- | --- | --- | --- | 
| 1 | Táo | Bút | Ờ! Bút-Táo! | 
| 2 | Bút Apple | Bút-Dứa | Ờ! Bút-Dứa-Táo-Bút! | 

Hoạt động đầu tiên thể hiện sự đảo ngược trật tự cơ bản. Điều thứ hai chứng minh rằng các dấu gạch nối hiện có vẫn còn nguyên. 

Đối với một ví dụ tùy chỉnh:```
3
I have a A
I have a B
I have a C-D
I have a E
I have a Long-Name
I have a X-Y-Z
```Dấu vết là: 

| Bước | Mục đầu tiên | Mục thứ hai | Sản lượng sản xuất | 
| --- | --- | --- | --- | 
| 1 | A | B | Ờ! B-A! | 
| 2 | CD | E | Ờ! E-C-D! | 
| 3 | Tên dài | X-Y-Z | Ờ! X-Y-Z-Tên dài! | 

Điều này xác nhận rằng thuật toán không diễn giải dấu gạch nối một cách đặc biệt. Chúng chỉ đơn giản là các ký tự bên trong tên vật phẩm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(S) | Mỗi ký tự trong đầu vào được xử lý một số lần không đổi trong khi chia dòng. | 
| Không gian | O(n) | Danh sách đầu ra lưu trữ các dòng được tạo trước khi in. | 

Đây,`S`là tổng số ký tự trong đầu vào. Với tối đa 1000 thao tác và 1000 ký tự cho mỗi mục, việc quét tuyến tính nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# provided samples
assert run(
"""2
I have a Apple
I have a Pen
I have a Apple-Pen
I have a Pen-Pineapple
"""
) == """Uh! Pen-Apple!
Uh! Pen-Pineapple-Apple-Pen!
""", "sample"

# minimum size
assert run(
"""1
I have a A
I have a B
"""
) == """Uh! B-A!
""", "single pair"

# all equal values
assert run(
"""2
I have a Apple
I have a Apple
I have a Apple
I have a Apple
"""
) == """Uh! Apple-Apple!
Uh! Apple-Apple!
""", "same names"

# boundary with long names
assert run(
"""1
I have a ABC-ABC-ABC
I have a XYZ-XYZ-XYZ
"""
) == """Uh! XYZ-XYZ-XYZ-ABC-ABC-ABC!
""", "hyphen preservation"

# multiple operations
assert run(
"""3
I have a One
I have a Two
I have a Three
I have a Four
I have a Five
I have a Six
"""
) == """Uh! Two-One!
Uh! Four-Three!
Uh! Six-Five!
""", "multiple pairs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một cặp với`A`Và`B`|`Uh! B-A!`| Xử lý đầu vào tối thiểu | 
| lặp đi lặp lại`Apple`mặt hàng | Cùng tên được nối chính xác | Giá trị bằng nhau và không có vỏ đặc biệt | 
| Tên chứa nhiều dấu gạch nối | Dấu gạch nối không thay đổi | Trích xuất đúng tên mặt hàng | 
| Một số cặp liên tiếp | Ba đầu ra độc lập | Thứ tự xử lý cặp | 

## Vỏ cạnh 

Đối với trường hợp cạnh phân tích cú pháp đầu tiên:```
1
I have a Apple
I have a Pen
```Thuật toán trích xuất`Apple`Và`Pen`bằng cách lấy mã thông báo cuối cùng từ mỗi dòng. Sau đó nó xây dựng`Pen-Apple`, sản xuất:```
Uh! Pen-Apple!
```Giải pháp sử dụng toàn bộ dòng thay vì mã thông báo mục sẽ bao gồm các từ không liên quan và không thành công. 

Đối với trường hợp cạnh thứ hai:```
1
I have a Apple-Pen
I have a Pen-Pineapple
```Các giá trị được trích xuất đã là tên mục hoàn chỉnh. Thuật toán không phân chia chúng thêm nữa mà kết hợp chúng một cách trực tiếp:```
Uh! Pen-Pineapple-Apple-Pen!
```Điều này xử lý chính xác các tên trông có vẻ lồng nhau vì thao tác này chỉ thêm một dấu gạch nối mới giữa hai tên gốc.
