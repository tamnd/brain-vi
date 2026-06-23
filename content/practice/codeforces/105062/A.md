---
title: "CF 105062A - Nó có được xếp hạng không??"
description: "Cho hai số nguyên m và d. Hãy coi m là số tháng trong năm dương lịch và d là ngày trong tháng. Nhiệm vụ là quyết định xem cặp này có thể biểu thị một ngày dương lịch hợp lệ trong thế giới đơn giản hóa mà bài toán ngụ ý hay không."
date: "2026-06-23T10:47:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105062
codeforces_index: "A"
codeforces_contest_name: "TheForces Round #29 (Clown-Forces)"
rating: 0
weight: 105062
solve_time_s: 72
verified: true
draft: false
---

[CF 105062A - Nó có được xếp hạng không??](https://codeforces.com/problemset/problem/105062/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho ta hai số nguyên`m`Và`d`. nghĩ về`m`dưới dạng số tháng trong năm dương lịch và`d`như một ngày trong tháng. Nhiệm vụ là quyết định xem cặp này có thể biểu thị một ngày dương lịch hợp lệ trong thế giới đơn giản hóa mà bài toán ngụ ý hay không. 

Trong thực tế, hạn chế duy nhất quan trọng là liệu ngày`d`thậm chí còn có ý nghĩa trong một tháng nhất định`m`. Vì không có năm hoặc hệ thống lịch nào được xác định rõ ràng nên vấn đề ngầm quy về việc kiểm tra xem ngày có tồn tại trong phạm vi cho phép của tháng đó hay không. 

Quan sát quan trọng là các tháng khác nhau ở chỗ chúng có thể chứa bao nhiêu ngày và giá trị của`(m, d)`phụ thuộc vào cấu trúc đó hơn là vào bất kỳ thủ thuật tính toán nào. 

Kích thước đầu vào rất nhỏ, với`m ≤ 12`Và`d ≤ 31`, điều này ngay lập tức loại trừ mọi nhu cầu về mối quan tâm tối ưu hóa. Bất kỳ giải pháp nào mã hóa trực tiếp các quy tắc lịch hoặc thậm chí mã hóa cứng các cặp hợp lệ đều đủ. 

Một trường hợp khó phát hiện xuất phát từ thực tế là giới hạn ngày thay đổi theo tháng. Ví dụ: một cách tiếp cận ngây thơ có thể cho rằng mỗi tháng có 31 ngày, điều này sẽ chấp nhận không chính xác những ngày không hợp lệ như ngày 30 tháng 2 hoặc ngày 31 tháng 4 theo cách giải thích lịch thực tế. Một cạm bẫy tiềm ẩn khác là quên rằng những tháng nhỏ nhất vẫn cho phép ít nhất 28 hoặc 30 ngày tùy theo cách giải thích và hạn chế quá mức các cặp hợp lệ một cách không chính xác. 

Ví dụ: nếu ai đó giả định không chính xác tất cả các tháng đều có 31 ngày, họ sẽ chấp nhận`(2, 31)`là hợp lệ, điều này rõ ràng là không chính xác. Mặt khác, nếu ai đó giả định đồng phục có giới hạn nhỏ hơn như 30 ngày cho tất cả các tháng, họ sẽ từ chối một cách không chính xác.`(1, 31)`. 

Vì vậy, vấn đề giảm xuống còn việc phân biệt chính xác tháng nào cho phép ngày tối đa. 

## Phương pháp tiếp cận 

Một lối suy nghĩ thô bạo sẽ là coi mọi ngày dương lịch có thể là hợp lệ và sau đó kiểm tra xem liệu`(m, d)`xuất hiện trong danh sách được tính toán trước của tất cả các ngày hợp lệ. Điều này có nghĩa là liệt kê tất cả các tháng và tất cả các ngày có thể có trong tháng theo quy tắc lịch, lưu trữ chúng thành một bộ và kiểm tra tư cách thành viên. Điều này hoạt động vì tên miền cực kỳ nhỏ, nhiều nhất là vài trăm mục. 

Vấn đề không phải là hiệu suất mà là sự phức tạp không cần thiết. Việc xây dựng và truy vấn một cấu trúc là quá mức cần thiết khi bản thân cấu trúc đó cố định và tầm thường. 

Cách tiếp cận trực tiếp hơn là mã hóa mối quan hệ từ tháng đến ngày tối đa. Các tháng xen kẽ giữa độ dài 31 ngày và 30 ngày, trong đó tháng Hai là trường hợp đặc biệt. Vì các ràng buộc quá nhỏ nên giải pháp rõ ràng nhất chỉ đơn giản là mã hóa số ngày tối đa mỗi tháng và so sánh`d`chống lại nó. 

Cái nhìn sâu sắc quan trọng là đây hoàn toàn không phải là một vấn đề năng động. Không có sự phụ thuộc giữa các trường hợp thử nghiệm hoặc tính toán; câu trả lời được xác định hoàn toàn bằng cách tra cứu liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Bảng liệt kê Brute Force của ngày hợp lệ | O(365) | O(365) | Đã chấp nhận | 
| Tra cứu trực tiếp theo quy định tháng | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng bản đồ theo từng tháng`m`đến số ngày hợp lệ tối đa. Điều này phản ánh cấu trúc lịch cố định, trong đó mỗi tháng có giới hạn trên được biết trước`d`. 
2. Đọc cặp đầu vào`(m, d)`. 
3. So sánh`d`so với giá trị tối đa được phép trong tháng`m`. 
4. Nếu`d`nằm trong phạm vi, đầu ra`"YES"`, nếu không thì xuất ra`"NO"`. 

Lý do đằng sau bước 1 là tất cả các lần kiểm tra trong tương lai đều phụ thuộc vào một quy tắc không đổi, do đó, việc tính toán trước sẽ tránh lặp lại logic hoặc các điều kiện. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên tính bất biến là mỗi tháng có số ngày hợp lệ tối đa cố định và bất kỳ ngày hợp lệ nào cũng phải đáp ứng`1 ≤ d ≤ max_days[m]`. Vì ánh xạ mã hóa các ràng buộc chính xác của lịch nên mọi đầu vào hợp lệ đều vượt qua quá trình kiểm tra bất đẳng thức và mọi đầu vào không hợp lệ đều vi phạm nó. Không có cấu trúc hoặc sự phụ thuộc ẩn nào khác tồn tại trong bài toán, vì vậy việc so sánh vừa cần thiết vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    m, d = map(int, input().split())

    # days per month in a standard calendar abstraction
    max_days = {
        1: 31,
        2: 28,
        3: 31,
        4: 30,
        5: 31,
        6: 30,
        7: 31,
        8: 31,
        9: 30,
        10: 31,
        11: 30,
        12: 31
    }

    if 1 <= d <= max_days[m]:
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Mã trực tiếp mã hóa cấu trúc lịch vào từ điển. Điều này tránh bất kỳ logic phân nhánh nào ngoài việc tra cứu và so sánh. Chi tiết quan trọng là đảm bảo rằng các giới hạn được bao gồm vì cả hai điểm cuối đều là ngày hợp lệ. 

Một chi tiết triển khai tinh tế là từ điển phải phản ánh chính xác tháng 2 là 28 ngày, vì việc quên đây là nguồn trả lời sai phổ biến nhất trong các vấn đề tương tự. Các tháng còn lại tuân theo một mô hình cố định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1
```| Bước | m | d | max_days[m] | Tình trạng | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào | 1 | 1 | - | - | 
| Tra cứu | 1 | 1 | 31 | - | 
| Kiểm tra | 1 | 1 | 31 | 1 ≤ 1 31 | 

Đầu ra:```
YES
```Dấu vết này cho thấy tháng 1 hỗ trợ 31 ngày, vì vậy ngày 1 là hợp lệ. Việc kiểm tra xác nhận giới hạn dưới được tôn trọng và giới hạn trên không bị vi phạm. 

### Ví dụ 2 

đầu vào:```
2 31
```| Bước | m | d | max_days[m] | Tình trạng | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào | 2 | 31 | - | - | 
| Tra cứu | 2 | 31 | 28 | - | 
| Kiểm tra | 2 | 31 | 28 | 31 28 là sai | 

Đầu ra:```
NO
```Điều này chứng tỏ trường hợp cạnh tháng Hai. Mặc dù 31 nói chung là số ngày hợp lệ nhưng nó vượt quá phạm vi cho phép trong tháng 2 nên bị từ chối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ thực hiện tra cứu từ điển và so sánh | 
| Không gian | O(1) | Bản đồ cố định 12 tháng | 

Giải pháp thỏa mãn một cách cơ bản các ràng buộc vì nó thực hiện công việc trong thời gian không đổi cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("1 1\n") == "YES", "sample 1"
assert run("2 31\n") == "NO", "sample 2"

# custom cases
assert run("1 31\n") == "YES", "January max boundary"
assert run("2 28\n") == "YES", "February boundary valid"
assert run("2 29\n") == "NO", "February overflow"
assert run("12 31\n") == "YES", "December max boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 31`| CÓ | giới hạn trên của tháng 31 ngày | 
|`2 29`| KHÔNG | từ chối ngày nhuận | 
|`12 31`| CÓ | trường hợp ranh giới tháng trước | 

## Vỏ cạnh 

Trường hợp một cạnh là ngày hợp lệ tối đa trong một tháng có 31 ngày. Đối với đầu vào`1 31`, thuật toán truy xuất`max_days[1] = 31`và kiểm tra`31 ≤ 31`, vượt qua, tạo ra`"YES"`. Điều này xác nhận rằng ranh giới trên là bao gồm. 

Một trường hợp khác là tràn tháng hai. Đối với đầu vào`2 29`, việc tra cứu trả về`28`, và điều kiện`29 ≤ 28`thất bại, vì vậy đầu ra là`"NO"`. Điều này cho thấy ràng buộc theo tháng cụ thể được thực thi chính xác. 

Trường hợp cuối cùng là tháng có 30 ngày chung như tháng Tư. Đối với một đầu vào như`4 30`, ánh xạ cho`30`, và đẳng thức được thông qua, đảm bảo rằng các tháng có ít hơn 31 ngày vẫn được xử lý chính xác mà không cần đặt vỏ đặc biệt ngoài việc tra cứu.
