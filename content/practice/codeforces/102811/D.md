---
title: "CF 102811D - \u0422\u0430\u0431\u043b\u0438\u0446\u0430"
description: "Chúng ta có một lưới vô hạn có các hàng và cột bắt đầu từ 1. Các ô chứa đầy các số nguyên liên tiếp bằng cách đi quanh viền của các ô vuông lớn hơn và lớn hơn. Nhiệm vụ của bạn là tìm tọa độ của ô chứa số n cho trước."
date: "2026-07-26T16:12:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102811
codeforces_index: "D"
codeforces_contest_name: "\u0428\u043a\u043e\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0432\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u043e\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, 9-11 \u043a\u043b\u0430\u0441\u0441\u044b, \u041c\u043e\u0441\u043a\u0432\u0430  (\u0432\u0435\u0440\u0441\u0438\u044f CF)"
rating: 0
weight: 102811
solve_time_s: 67
verified: true
draft: false
---

[CF 102811D - \u0422\u0430\u0431\u043b\u0438\u0446\u0430](https://codeforces.com/problemset/problem/102811/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới vô hạn có các hàng và cột bắt đầu từ 1. Các ô chứa đầy các số nguyên liên tiếp bằng cách đi quanh viền của các ô vuông lớn hơn và lớn hơn. Nhiệm vụ của bạn là tìm tọa độ của ô chứa số n cho trước. 

Quan sát quan trọng là việc điền vào không phải là tùy ý. Sau khi hoàn thành một hình vuông có độ dài cạnh k, chính xác k2 ô đã được lấp đầy. Điều này có nghĩa là mỗi số thuộc về một lớp hình vuông và thách thức chính là tìm ra lớp đó một cách nhanh chóng và sau đó xác định vị trí bên trong đường viền của nó. 

Giá trị của n có thể lớn tới 10¹⁸ nên việc mô phỏng quá trình điền là không thể. Ngay cả mô phỏng O(√n) cũng không cần thiết vì câu trả lời chỉ phụ thuộc vào lớp hình vuông và một vài phép tính số học. Giải pháp phải hoạt động trong thời gian không đổi hoặc gần với thời gian đó, sử dụng số học số nguyên 64 bit. 

Một lỗi phổ biến là sử dụng căn bậc hai dấu phẩy động. Đối với các giá trị gần 10¹⁸, phép tính dấu phẩy động có thể làm tròn không chính xác và đặt n vào sai lớp. Ví dụ: một số chính xác là hình vuông, chẳng hạn như 16, phải thuộc đường viền 4 x 4. Việc triển khai bất cẩn có thể tính toán căn bậc hai nhỏ hơn một chút và coi nó thuộc về lớp trước đó, tạo ra tọa độ sai. 

Một trường hợp cạnh khác là giá trị nhỏ nhất có thể. Đối với đầu vào`1`, câu trả lời là`1 1`. Mã giả định lớp có hình vuông trước đó sẽ không thành công vì lớp đầu tiên không có ô nào trước đó. 

Ranh giới quan trọng khác là điểm cuối của một lớp. Ví dụ, số`9`là giá trị cuối cùng của hình vuông 3 x 3. tọa độ của nó là`1 3`. Nếu việc triển khai sử dụng các phép so sánh nghiêm ngặt thay vì các phép so sánh bao hàm, thì nó có thể chuyển giá trị này sang lớp tiếp theo. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xây dựng từng ô xoắn ốc. Bắt đầu từ ô đầu tiên, chúng ta tiếp tục di chuyển dọc theo các đường viền hình vuông và đếm cho đến khi đạt được n. Điều này đúng vì nó tuân theo chính xác thứ tự các số được viết. Tuy nhiên, đầu vào lớn nhất chứa các giá trị lên tới 10¹⁸, do đó số lượng ô được truy cập cũng có thể vào khoảng 10¹⁸. Việc mô phỏng như vậy không thể hoàn thành trong bất kỳ khoảng thời gian thực tế nào. 

Cấu trúc xoắn ốc giúp tiếp cận nhanh hơn. Hình vuông có độ dài cạnh k chứa chính xác k2 ô, vì vậy lớp chứa n là k nhỏ nhất mà k2 ít nhất là n. Khi lớp này được biết đến, các số (k-1)² đầu tiên đã ở phía sau chúng ta. Phần bù còn lại cho chúng ta biết n nằm ở đâu trên đường viền hình vuông hiện tại. 

Hướng biên chỉ phụ thuộc vào việc k là lẻ hay chẵn. Trong các lớp lẻ, hình vuông mới bắt đầu ở góc dưới bên trái và di chuyển sang bên phải, sau đó hướng lên dọc theo cạnh phải. Trong các lớp chẵn, nó bắt đầu ở góc trên bên phải và di chuyển xuống dưới, sau đó sang trái dọc theo cạnh dưới. Điều này cho phép chúng tôi tính toán câu trả lời mà không cần truy cập vào bất kỳ ô nào khác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tìm số nguyên k nhỏ nhất sao cho k² ít nhất là n. Đây là độ dài cạnh của đường viền hình vuông chứa n. Số ô trước đường viền này là`(k - 1)²`, vì vậy mọi số trong lớp có thể được xác định bằng phần bù của nó so với giá trị đó. 
2. Tính toán`offset = n - (k - 1)²`. Độ lệch nằm trong khoảng từ 1 đến`2k - 1`, vì đường viền hình vuông sẽ thêm chính xác số ô mới đó. 
3. Nếu k lẻ, xử lý đường viền bắt đầu từ góc dưới bên trái. Khi offset nhiều nhất là k thì số nằm ở hàng dưới cùng nên hàng là k và cột là offset. Ngược lại, số ở cột bên phải, di chuyển lên từ dưới lên nên hàng giảm đi trong khi cột giữ nguyên k. 
4. Nếu k chẵn, xử lý đường viền bắt đầu từ góc trên bên phải. Khi offset nhiều nhất là k thì số nằm ở cột bên phải nên hàng là offset và cột là k. Ngược lại, số nằm ở hàng dưới cùng, di chuyển sang trái nên hàng giữ nguyên k trong khi cột giảm dần. 

Tại sao nó hoạt động: 

Mỗi lớp k chứa chính xác các ô mở rộng hình vuông trước đó từ độ dài cạnh k-1 đến độ dài cạnh k. Thuật toán trước tiên xác định lớp duy nhất này vì tất cả các lớp nhỏ hơn chứa chính xác`(k-1)²`các ô và lớp hiện tại kết thúc ở k². Bên trong lớp, phần bù xác định duy nhất vị trí vì thứ tự truyền qua đường viền được cố định. Vì mọi vị trí có thể có trên đường viền đều được bao phủ chính xác một lần nên tọa độ tính toán phải khớp với vị trí của n. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    k = int(n ** 0.5)
    while k * k < n:
        k += 1
    while (k - 1) * (k - 1) >= n:
        k -= 1

    offset = n - (k - 1) * (k - 1)

    if k % 2 == 1:
        if offset <= k:
            row = k
            col = offset
        else:
            row = 2 * k - offset
            col = k
    else:
        if offset <= k:
            row = offset
            col = k
        else:
            row = k
            col = 2 * k - offset

    print(row, col)

if __name__ == "__main__":
    solve()
```Tính toán lớp tìm thấy hình vuông đầu tiên đủ lớn để chứa n. Các vòng hiệu chỉnh được sử dụng vì căn bậc hai ban đầu xuất phát từ số học dấu phẩy động và phạm vi đầu vào đủ lớn nên có thể xảy ra lỗi làm tròn. 

Phép tính bù chuyển đổi số toàn cầu thành một vị trí chỉ bên trong một đường viền hình vuông. Điều này tránh mọi sự phụ thuộc vào kích thước của các lớp trước đó. 

Kiểm tra tính chẵn lẻ kiểm soát hướng truyền tải. Kích thước hình vuông chẵn và lẻ có các góc bắt đầu đối diện nhau, do đó, việc sử dụng sai nhánh sẽ phản ánh câu trả lời trên hình vuông và không thực hiện được khoảng một nửa số đầu vào. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn mặc dù các giá trị trung gian như k² có thể đạt tới 10¹⁸. 

## Ví dụ đã hoạt động 

Đối với đầu ra mẫu, đầu vào bị thiếu tương ứng với`n = 15`. 

| n | k | bù đắp | trường hợp | hàng | col | 
| --- | --- | --- | --- | --- | --- | 
| 15 | 4 | 6 | chẵn, mặt dưới | 4 | 2 | 

Lớp 4 x 4 bắt đầu sau 9. Giá trị mới thứ sáu trên đường viền này đạt được sau khi di chuyển xuống phía bên phải rồi sang trái dọc theo hàng dưới cùng, đưa ra tọa độ`(4,2)`. 

Một ví dụ khác: 

đầu vào`1`| n | k | bù đắp | trường hợp | hàng | col | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | lẻ, mặt dưới | 1 | 1 | 

Lớp đầu tiên chỉ chứa ô đầu tiên nên thuật toán ngay lập tức trả về vị trí bắt đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học và hiệu chỉnh kích thước không đổi được thực hiện. | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ một số biến số nguyên. | 

Giới hạn đầu vào là 10¹⁸ loại trừ mọi cách tiếp cận đi qua bảng. Giải pháp thời gian không đổi dễ dàng phù hợp trong giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())

    k = int(n ** 0.5)
    while k * k < n:
        k += 1
    while (k - 1) * (k - 1) >= n:
        k -= 1

    offset = n - (k - 1) * (k - 1)

    if k % 2 == 1:
        if offset <= k:
            row, col = k, offset
        else:
            row, col = 2 * k - offset, k
    else:
        if offset <= k:
            row, col = offset, k
        else:
            row, col = k, 2 * k - offset

    sys.stdin = old_stdin
    return f"{row} {col}"

assert solve_case("15\n") == "4 2", "sample"
assert solve_case("1\n") == "1 1", "minimum value"
assert solve_case("9\n") == "1 3", "perfect square boundary"
assert solve_case("16\n") == "4 1", "even layer end"
assert solve_case("25\n") == "1 5", "maximum corner of odd layer"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 15 | 4 2 | Cung cấp ví dụ và thậm chí truyền tải lớp | 
| 1 | 1 1 | Đầu vào nhỏ nhất có thể | 
| 9 | 1 3 | Kết thúc một lớp lẻ | 
| 16 | 4 1 | Ranh giới giữa hai hướng đi qua | 
| 25 | 1 5 | Góc lớn nhất của một lớp lẻ | 

## Vỏ cạnh 

Đối với đầu vào`1`, thuật toán tìm k = 1 và offset = 1. Vì k là số lẻ và offset nằm trong k ô đầu tiên nên thuật toán đặt giá trị ở hàng 1, cột 1. Điều này tránh việc cố gắng truy cập vào ô vuông trước đó không tồn tại. 

Đối với đầu vào`9`, thuật toán tìm k = 3 vì 9 chính xác là một hình vuông. Độ lệch là 5 vì tám ô thuộc về các lớp trước đó. Vì k là số lẻ và độ lệch lớn hơn k nên số này nằm ở cột bên phải với hàng`2 * 3 - 5 = 1`, cho`(1,3)`. Điều này xác nhận rằng các giá trị bình phương chính xác được gán cho lớp chính xác. 

Đối với đầu vào`16`, thuật toán chuyển sang lớp tiếp theo với k = 4. Độ lệch là 7, vượt quá bốn ô đầu tiên của phía bên phải. Chuyển động còn lại dọc theo hàng dưới cùng, tạo cho cột`2 * 4 - 7 = 1`và hàng 4. Câu trả lời là`(4,1)`, ô đầu tiên của đường viền mới.
