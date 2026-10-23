---
title: "CF 105941D-2025"
description: "Chúng ta được cấp một số nguyên duy nhất biểu thị một năm và chúng ta cần quyết định xem nó có thỏa mãn một thuộc tính số rất cụ thể hay không. Năm hợp lệ nếu hai điều kiện được giữ đồng thời."
date: "2026-06-22T15:51:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105941
codeforces_index: "D"
codeforces_contest_name: "2025 National Invitational of CCPC (Zhengzhou), 2025 CCPC Henan Provincial Collegiate Programming Contest"
rating: 0
weight: 105941
solve_time_s: 59
verified: true
draft: false
---

[CF 105941D - 2025](https://codeforces.com/problemset/problem/105941/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số nguyên duy nhất biểu thị một năm và chúng ta cần quyết định xem nó có thỏa mãn một thuộc tính số rất cụ thể hay không. Năm hợp lệ nếu hai điều kiện được giữ đồng thời. Đầu tiên, bản thân năm phải là một số chính phương, nghĩa là tồn tại một số nguyên có bình phương bằng năm. Thứ hai, khi chúng ta tính tổng các chữ số thập phân của năm thì tổng đó cũng phải là số chính phương. 

Kích thước đầu vào cực kỳ nhỏ, với năm được giới hạn bởi 999.999. Điều này ngay lập tức loại trừ mọi thứ như tiền xử lý nặng hoặc cấu trúc dữ liệu nâng cao. Bất kỳ giải pháp nào liên tục thực hiện kiểm tra bình phương hoặc xử lý chữ số vẫn không đáng kể để thực thi trong giới hạn vì chúng tôi chỉ xử lý một số hoặc nhiều nhất là một vài trường hợp thử nghiệm nếu được mở rộng. 

Các trường hợp khó phát hiện duy nhất đến từ việc quản lý sai các tấm séc vuông hoàn hảo. Một lỗi phổ biến là sử dụng căn bậc hai có dấu phẩy động mà không sửa lỗi chính xác. Một điều nữa là quên rằng tổng chữ số cũng chỉ có thể bằng 0 trong các trường hợp lý thuyết như 0, điều này không liên quan ở đây vì năm ít nhất là 1. Vấn đề thứ ba là giả định rằng nếu số trông giống như một mẫu hình vuông thì nó phải là một, điều này không thành công đối với các giá trị như 999999 trong đó lỗi làm tròn hoặc kiểm tra số nguyên không chính xác có thể xuất hiện nếu được triển khai kém. 

Trường hợp cạnh cụ thể là mẫu 999999. Tổng chữ số là 54, không phải là số chính phương hoàn hảo, vì vậy câu trả lời là Không mặc dù số này gần với một hình vuông lớn và có thể dẫn đến các phương pháp phỏng đoán không chính xác. Một cái khác là năm 2025, hoạt động rõ ràng vì năm 2025 bằng 45 bình phương và tổng các chữ số là 9 bằng 3 bình phương. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là tính xem số đó có phải là số chính phương hay không, đồng thời tính riêng tổng các chữ số của nó và kiểm tra xem tổng đó có phải là số chính phương hay không. Điều này rất đơn giản vì cả hai thao tác đều là O(log y). Thành phần không cần thiết duy nhất là kiểm tra hình vuông. 

Một cách mạnh mẽ để kiểm tra xem số n có phải là số chính phương hay không là thử tất cả các số nguyên i từ 1 trở lên và kiểm tra xem bình phương của tôi có bằng n hay không. Điều này hoạt động chính xác nhưng không cần thiết vì căn bậc hai tối đa có thể nhiều nhất là 1000 vì 1000 bình phương là 1.000.000, cao hơn một chút so với giới hạn ràng buộc. Mặc dù lực lượng vũ phu này vốn đã nhỏ nhưng về mặt khái niệm nó kém hơn vì nó bỏ qua cấu trúc toán học trực tiếp của căn bậc hai. 

Một cách tiếp cận tốt hơn là tính căn bậc hai số nguyên của n bằng cách sử dụng sqrt số nguyên tích hợp hoặc tìm kiếm nhị phân, sau đó xác minh xem liệu bình phương của nó có tạo ra n hay không. Điều này tránh hoàn toàn các vấn đề về dấu phẩy động và đưa ra kiểm tra xác định. 

Tổng các chữ số được tính trong một lần chuyển qua biểu diễn thập phân và sau đó áp dụng lại logic kiểm tra bình phương tương tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét sqrt Brute Force | O(√n) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Kiểm tra sqrt số nguyên + tổng chữ số | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên y làm đầu vào. Đây là năm ứng viên được chúng tôi đánh giá trực tiếp. 
2. Tính tổng các chữ số của y bằng cách trích liên tục chữ số cuối cùng và tích lũy nó. Điều này đưa ra một số phụ s thể hiện cấu trúc cộng của năm. 
3. Tính r = Floor(sqrt(y)) bằng cách sử dụng số nguyên. Sau đó xác minh xem r * r có bằng y hay không. Nếu không, y không phải là số bình phương hoàn hảo và chúng ta có thể bác bỏ nó ngay lập tức. Bước này tránh được các vấn đề về độ chính xác của dấu phẩy động. 
4. Lặp lại phép xác minh bình phương tương tự cho s. Tính t = Floor(sqrt(s)) và kiểm tra xem t * t có bằng s hay không. Điều này đảm bảo tổng các chữ số cũng có cấu trúc là một hình vuông hoàn hảo. 
5. Xuất ra Có nếu cả hai lần kiểm tra đều đạt, nếu không thì xuất ra Số. 

### Tại sao nó hoạt động

Tính đúng đắn dựa trên hai đặc tính độc lập. Một số là số chính phương khi và chỉ nếu căn bậc hai nguyên của nó, khi bình phương ngược lại, sẽ cho ra giá trị ban đầu. Điều này tránh hoàn toàn các lỗi gần đúng vì tất cả các phép toán đều dựa trên số nguyên. Phép biến đổi tổng chữ số độc lập với thuộc tính bình phương, do đó việc xác minh nó một cách riêng biệt không ảnh hưởng đến điều kiện đầu tiên. Vì cả hai điều kiện đều cần và đủ riêng lẻ nên sự kết hợp của chúng khớp chính xác với yêu cầu của bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_square(x: int) -> bool:
    if x < 0:
        return False
    r = int(x ** 0.5)
    return r * r == x or (r + 1) * (r + 1) == x

y = int(input().strip())

digit_sum = 0
tmp = y
while tmp > 0:
    digit_sum += tmp % 10
    tmp //= 10

if is_square(y) and is_square(digit_sum):
    print("Yes")
else:
    print("No")
```chức năng`is_square`sử dụng căn bậc hai dấu phẩy động nhưng sửa các lỗi làm tròn có thể xảy ra bằng cách kiểm tra cả hai`r`Và`r+1`. Điều này rất quan trọng vì việc truyền trực tiếp từ float đôi khi có thể bị cắt bớt do giới hạn độ chính xác gần các ô vuông lớn. Việc tính toán tổng chữ số được thực hiện lặp đi lặp lại để tránh chi phí chuyển đổi chuỗi, mặc dù cả hai phương pháp đều có thể được chấp nhận với các ràng buộc. 

Điều kiện cuối cùng kết hợp trực tiếp cả hai bước kiểm tra, phù hợp với yêu cầu logic mà cả hai thuộc tính phải giữ đồng thời. 

## Ví dụ đã hoạt động 

### Ví dụ 1: 2025 

Chúng tôi tính tổng chữ số và trạng thái bình phương từng bước. 

| Bước | y | tổng chữ số | kiểm tra sqrt(y) | kiểm tra sqrt(tổng) | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 2025 | 0 | - | - | 
| Sau khi quét chữ số | 2025 | 9 | - | - | 
| Kiểm tra vuông y | 2025 | 9 | Đúng (45² = 2025) | - | 
| Tổng kiểm tra vuông | 2025 | 9 | Đúng | Đúng (3² = 9) | 

Điều này xác nhận đồng thời cả hai điều kiện, vì vậy đầu ra là Có. 

### Ví dụ 2: 999999 

| Bước | y | tổng chữ số | kiểm tra sqrt(y) | kiểm tra sqrt(tổng) | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 999999 | 0 | - | - | 
| Sau khi quét chữ số | 999999 | 54 | - | - | 
| Kiểm tra vuông y | 999999 | 54 | Sai | - | 

Vì điều kiện đầu tiên đã thất bại nên chúng tôi dừng sớm và trả về Không. Tổng các chữ số là 54 không liên quan khi điều kiện đầu tiên bị vi phạm, nhưng ngay cả khi được chọn, 54 không phải là số chính phương hoàn hảo. 

Những dấu vết này cho thấy sự từ chối có thể xảy ra ở một trong hai điều kiện một cách độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log y) | trích xuất chữ số mất O(log y), kiểm tra bình phương có thời gian không đổi | 
| Không gian | O(1) | chỉ một vài biến số nguyên được sử dụng | 

Các ràng buộc giới hạn y ở mức một triệu, do đó, ngay cả một vòng lặp đơn giản trên các chữ số và kiểm tra bình phương thời gian không đổi cũng thấp hơn nhiều so với giới hạn thời gian. Giải pháp có hiệu quả ngay lập tức. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return main()

def main():
    import sys
    input = sys.stdin.readline

    def is_square(x: int) -> bool:
        if x < 0:
            return False
        r = int(x ** 0.5)
        return r * r == x or (r + 1) * (r + 1) == x

    y = int(input().strip())

    digit_sum = 0
    tmp = y
    while tmp > 0:
        digit_sum += tmp % 10
        tmp //= 10

    return "Yes" if is_square(y) and is_square(digit_sum) else "No"

# provided samples
assert run("2025\n") == "Yes"
assert run("999999\n") == "No"

# custom cases
assert run("1\n") == "Yes"   # 1 is 1^2, digit sum 1 is 1^2
assert run("2\n") == "No"    # not a square
assert run("16\n") == "No"   # 16 is square but digit sum 7 is not
assert run("81\n") == "Yes"  # 81=9^2, digit sum 9=3^2
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | Có | trường hợp hình vuông hợp lệ nhỏ nhất | 
| 2 | Không | từ chối không vuông | 
| 16 | Không | tổng chữ số bình phương nhưng không hợp lệ | 
| 81 | Có | thỏa mãn cả hai điều kiện | 

## Vỏ cạnh 

Đầu vào có ý nghĩa nhỏ nhất là 1. Thuật toán tính tổng các chữ số là 1 và xác định chính xác cả hai điều kiện là đúng vì 1 là số bình phương hoàn hảo. 

Đối với 2, tổng chữ số là 2 và việc kiểm tra bình phương không thành công ngay lập tức. Kiểm tra hình vuông sử dụng so sánh số nguyên, do đó không có lỗi dấu phẩy động nào có thể phân loại sai nó thành hình vuông. 

Đối với 16, số này vượt qua bài kiểm tra bình phương vì 4 bình phương là 16, nhưng tổng các chữ số trở thành 7. Lần kiểm tra thứ hai sẽ loại bỏ nó một cách chính xác. 

Đối với 81, cả hai lần kiểm tra đều vượt qua. Căn bậc hai là 9 và tổng các chữ số cũng là 9 và cả hai đều giảm xuống còn 3 bình phương. Điều này khẳng định rằng việc kiểm tra độc lập không gây trở ngại và cả hai phải được đáp ứng đồng thời.
