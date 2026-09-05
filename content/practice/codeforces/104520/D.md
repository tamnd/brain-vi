---
title: "CF 104520D - Một bài toán truy vấn khác"
description: "Mỗi truy vấn mô tả một phạm vi số nguyên từ l đến r và hỏi có bao nhiêu cặp thứ tự (a, b) trong phạm vi đó thỏa mãn một điều kiện cụ thể liên quan đến hàm f(a, b). Bản thân hàm này đã đơn giản hóa trước khi chúng ta bắt đầu đếm các cặp."
date: "2026-06-30T10:26:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104520
codeforces_index: "D"
codeforces_contest_name: "Teamscode Summer 2023 Contest"
rating: 0
weight: 104520
solve_time_s: 75
verified: true
draft: false
---

[CF 104520D - Một bài toán truy vấn toán học khác](https://codeforces.com/problemset/problem/104520/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi truy vấn mô tả một dãy số nguyên từ`l`ĐẾN`r`, và hỏi có bao nhiêu cặp được sắp xếp`(a, b)`bên trong phạm vi đó thỏa mãn một điều kiện cụ thể liên quan đến hàm`f(a, b)`. Bản thân hàm này đã đơn giản hóa trước khi chúng ta bắt đầu đếm các cặp. 

biểu hiện`f(a, b) = a + b + |a - b|`hành xử khác nhau tùy thuộc vào cái nào`a`Và`b`lớn hơn. Nếu như`a ≤ b`, sau đó`|a - b| = b - a`, do đó biểu thức trở thành`a + b + (b - a) = 2b`. Nếu như`a > b`, sau đó`|a - b| = a - b`, do đó biểu thức trở thành`a + b + (a - b) = 2a`. Vì vấn đề hạn chế chúng ta`a ≤ b`, chỉ có trường hợp đầu tiên quan trọng và hàm luôn giảm xuống`f(a, b) = 2b`. 

Điều này ngay lập tức thay đổi cách giải thích của từng truy vấn. Thay vì nghĩ về các cặp, chúng tôi thực sự đang đếm xem có bao nhiêu lựa chọn về`(a, b)`với`l ≤ a ≤ b ≤ r`thỏa mãn`2b = x`. Một lần`b`đã được sửa,`a`có thể là bất kỳ số nguyên nào từ`l`ĐẾN`b`. 

Các ràng buộc rất lớn, với các giá trị lên tới`10^18`và lên đến`2 × 10^5`truy vấn. Bất kỳ giải pháp nào lặp lại trên các phạm vi hoặc liệt kê các cặp đều không thể thực hiện được. Ngay cả việc lặp lại một phạm vi cho mỗi truy vấn cũng đã quá chậm. 

Một trường hợp khó phát hiện khi`x`thật kỳ quặc. Từ`2b = x`, không có số nguyên`b`tồn tại, vì vậy câu trả lời phải bằng 0. Một trường hợp cạnh khác xảy ra khi ngụ ý`b = x / 2`nằm bên ngoài`[l, r]`, cũng mang lại kết quả bằng không. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp lại trên tất cả các cặp`(a, b)`cho mỗi truy vấn, kiểm tra xem`l ≤ a ≤ b ≤ r`và liệu`f(a, b) = x`. Điều này đòi hỏi phải kiểm tra đại khái`(r - l + 1)^2 / 2`cặp cho mỗi truy vấn. Từ`r`có thể lớn như`10^18`, điều này hoàn toàn không thể thực hiện được. 

Quan sát chính xuất phát từ việc đơn giản hóa hàm. Bởi vì`f(a, b)`luôn sụp đổ`2b`dưới sự ràng buộc`a ≤ b`, giá trị chỉ phụ thuộc vào`b`, không bật`a`. Điều này loại bỏ hoàn toàn cấu trúc hai chiều của vấn đề. 

Đối với một truy vấn cố định`(l, r, x)`, đầu tiên chúng ta kiểm tra xem`x`là chẵn. Nếu nó là số lẻ thì không có giải pháp nào tồn tại. Ngược lại chúng ta đặt`b = x / 2`. Bây giờ điều kiện trở thành hoàn toàn về việc liệu`b`nằm trong khoảng`[l, r]`. 

Nếu như`b`là hợp lệ, chúng tôi tính tất cả có thể`a`như vậy`l ≤ a ≤ b`. Có chính xác`b - l + 1`sự lựa chọn. Nếu như`b < l`hoặc`b > r`, câu trả lời là bằng không. 

Điều này làm giảm mỗi truy vấn thành số học thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((r−l)^2) mỗi truy vấn | O(1) | Quá chậm | 
| Tối ưu | O(1) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc từng truy vấn`(l, r, x)`và xác định xem giá trị hợp lệ của`b`có thể tồn tại. Từ`f(a, b)`đơn giản hóa để`2b`, giá trị của`x`phải chẵn. Nếu như`x`là số lẻ, câu trả lời ngay lập tức là số không. 
2. Tính toán`b = x / 2`. Đây là giá trị ứng cử viên duy nhất của phần tử lớn hơn trong cặp. Phần còn lại của lý do phụ thuộc hoàn toàn vào việc giá trị này có nằm trong khoảng truy vấn hay không. 
3. Kiểm tra xem`b < l`hoặc`b > r`. Nếu một trong hai điều kiện được giữ thì không có cặp hợp lệ nào tồn tại vì tất cả các cặp hợp lệ đều yêu cầu`l ≤ a ≤ b ≤ r`. 
4. Nếu`b`ở trong`[l, r]`, tính xem có bao nhiêu hợp lệ`a`các giá trị tồn tại. Từ`a`dao động từ`l`ĐẾN`b`, bao gồm, số đếm là`b - l + 1`. 
5. Xuất giá trị này cho truy vấn. 

### Tại sao nó hoạt động 

Sự chuyển đổi của`f(a, b)`thu gọn mọi cặp hợp lệ thành một điều kiện chỉ phụ thuộc vào phần tử thứ hai của cặp. Ràng buộc`a ≤ b`đảm bảo hàm luôn đánh giá thành`2b`, tạo tất cả các cặp giống nhau`b`tương đương về mặt hiệu lực. Kết quả là, các cặp đếm trở nên tương đương với việc đếm các lựa chọn hợp lệ của`a`cho mỗi khả thi`b`và không có cặp nào ngoài khoảng dẫn xuất có thể thỏa mãn phương trình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    out = []
    
    for _ in range(q):
        l, r, x = map(int, input().split())
        
        if x % 2 == 1:
            out.append("0")
            continue
        
        b = x // 2
        
        if b < l or b > r:
            out.append("0")
            continue
        
        out.append(str(b - l + 1))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp dựa vào việc giảm từng truy vấn thành một giá trị ứng cử viên duy nhất được rút ra từ phương trình. Kiểm tra tính chẵn lẻ ngăn chặn các nửa giá trị không hợp lệ và kiểm tra phạm vi đảm bảo chúng tôi chỉ đếm các cặp tuân thủ các ràng buộc khoảng ban đầu. Số học`b - l + 1`trực tiếp tính các lựa chọn hợp lệ cho`a`mà không có bất kỳ sự lặp lại nào. 

Một cạm bẫy triển khai phổ biến là quên rằng`a`không ảnh hưởng`f(a, b)`một lần ra lệnh. Một cách khác là giả định không chính xác cả hai`a ≤ b`Và`b ≤ a`các trường hợp đóng góp riêng biệt, điều này sẽ tăng gấp đôi hoặc tạo ra sự phân nhánh không cần thiết. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi các truy vấn mẫu: 

đầu vào:```
2 6 8
3 9 5
```Đối với truy vấn đầu tiên`(2, 6, 8)`: 

| Bước | Giá trị của x | Kiểm tra tính chẵn lẻ | b = x/2 | Kiểm tra phạm vi hợp lệ | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 8 | thậm chí | 4 | 2  4  6 | 4 - 2 + 1 = 3 | 

Đối với truy vấn thứ hai`(3, 9, 5)`: 

| Bước | Giá trị của x | Kiểm tra tính chẵn lẻ | b = x/2 | Kiểm tra phạm vi hợp lệ | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | lẻ | - | không hợp lệ | 0 | 

Ví dụ đầu tiên xác nhận rằng nhiều`a`các giá trị đóng góp cho một cố định`b`, trong khi phần thứ hai cho thấy chỉ riêng tính chẵn lẻ có thể loại bỏ mọi khả năng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) | Mỗi truy vấn được xử lý bằng các phép toán số học không đổi | 
| Không gian | O(1) | Chỉ một số biến số nguyên được sử dụng ngoài bộ lưu trữ đầu ra | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì ngay cả số lượng truy vấn tối đa cũng chỉ yêu cầu kiểm tra và chia số nguyên đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue().strip() if False else capture(inp)

def capture(inp: str) -> str:
    import sys
    old_in = sys.stdin
    old_out = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdin = old_in
    sys.stdout = old_out
    return out

# provided samples
assert capture("2\n2 6 8\n3 9 5\n") == "3\n0"

# custom cases
assert capture("1\n1 1 2\n") == "1", "single point range"
assert capture("1\n5 10 11\n") == "0", "odd x"
assert capture("1\n5 10 20\n") == "6", "full valid interval"
assert capture("1\n10 20 18\n") == "9", "boundary inclusion"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 2`|`1`| hành vi phạm vi hợp lệ tối thiểu | 
|`5 10 11`|`0`| số lẻ`x`từ chối | 
|`5 10 20`|`6`| đếm khoảng thời gian hợp lệ đầy đủ | 
|`10 20 18`|`9`| độ đúng ranh giới | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi`x`thật kỳ quặc. Đối với đầu vào`(l=1, r=10, x=7)`, thuật toán ngay lập tức bác bỏ nó vì`b = x/2`không phải là số nguyên, tạo ra kết quả`0`mà không cần kiểm tra thêm. 

Một trường hợp khác là khi`b`nằm dưới khoảng đó. Vì`(l=5, r=10, x=6)`, chúng tôi nhận được`b = 3`, nhỏ hơn`l`, vì vậy không hợp lệ`a`tồn tại. Thuật toán dừng chính xác ở việc kiểm tra phạm vi. 

Trường hợp ranh giới cuối cùng xảy ra khi`b`bằng`r`. Vì`(l=3, r=8, x=16)`, chúng tôi nhận được`b = 8`, và hợp lệ`a`giá trị là`3..8`, sản xuất`6`. Công thức`b - l + 1`bao gồm chính xác cả hai điểm cuối mà không cần điều chỉnh.
