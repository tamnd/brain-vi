---
title: "CF 102261D - \u0420\u0430\u0441\u043a\u043e\u0434\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u0435"
description: "Chúng ta có một chuỗi ký tự. Vòng giải mã tìm kiếm mọi lần xuất hiện của ba ký tự liên tiếp có dạng ~XY, trong đó X và Y là các chữ số thập lục phân và thay thế mỗi bộ ba như vậy bằng một ký tự đơn có mã ASCII là số thập lục phân XY."
date: "2026-08-17T20:39:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102261
codeforces_index: "D"
codeforces_contest_name: "\u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e - \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u044f (\u042f\u043d\u0434\u0435\u043a\u0441)"
rating: 0
weight: 102261
solve_time_s: 163
verified: true
draft: false
---

[CF 102261D - \u0420\u0430\u0441\u043a\u043e\u0434\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u0435](https://codeforces.com/problemset/problem/102261/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi ký tự. Vòng giải mã tìm kiếm mỗi lần xuất hiện của ba ký tự liên tiếp có dạng`~XY`, Ở đâu`X`Và`Y`là các chữ số thập lục phân và thay thế mỗi bộ ba như vậy bằng một ký tự đơn có mã ASCII là số thập lục phân`XY`. Tất cả các lần thay thế trong một vòng đều diễn ra đồng thời. 

Chúng tôi không được yêu cầu xây dựng lại chuỗi được giải mã cuối cùng. Chúng ta chỉ cần số lượng vòng không tầm thường liên tiếp tối đa, nghĩa là các vòng trong đó tồn tại ít nhất một bộ ba như vậy. 

Điều thú vị là việc thay thế có thể tạo ra một dấu ngã khác. Đặc biệt,`~7e`trở thành ký tự có mã ASCII`0x7e`, cái nào khác`~`. Dấu ngã mới đó có thể tham gia vào một mô hình trong vòng tiếp theo. Việc thay thế cũng có thể tạo ra một chữ số thập lục phân, sau này có thể được sử dụng bởi dấu ngã ở bên trái. 

Độ dài đầu vào tối đa là 300.000, do đó thuật toán quét toàn bộ chuỗi một lần là phù hợp. Một thuật toán lấy thời gian bậc hai là quá chậm. Điều này quan trọng vì bản thân số vòng giải mã có thể là tuyến tính. Ví dụ, chuỗi`~7e7e7e`được giảm đi như`~7e7e7e`, sau đó`~7e7e`, sau đó`~7e`, sau đó`~`, do đó số vòng tỷ lệ thuận với độ dài đầu vào. 

Do đó, việc triển khai đơn giản xây dựng chuỗi một cách rõ ràng sau mỗi vòng có thể thực hiện theo thứ tự 300.000 lần quét toàn bộ. Vì mỗi vòng không cần thiết sẽ giảm độ dài ít nhất hai vòng, nên trường hợp xấu nhất chứa khoảng 150.000 vòng và tổng số ký tự được kiểm tra có thể đạt tới Θ(n²), khoảng n²/4 trong một chuỗi chỉ giảm một mẫu mỗi vòng. 

Có một số trường hợp khó có thể bỏ sót. 

Vì`~7e7e`, vòng đầu tiên thay đổi`~7e`vào trong`~`, rời đi`~7e`. Vòng thứ hai thay đổi điều đó thành`~`, vì vậy câu trả lời là 2. Việc triển khai chỉ tìm kiếm chuỗi gốc sẽ tìm thấy một mẫu và bỏ lỡ dấu ngã mới được tạo. 

Vì`~~37~45~fF`, ba mẫu`~37`,`~45`, Và`~fF`tất cả đều tồn tại ở trạng thái ban đầu và phải được giải mã trong cùng một vòng. Sau vòng đó, chuỗi chứa`~7E`tiếp theo là giải mã`0xFF`nhân vật, vì vậy một vòng khác có thể xảy ra. Câu trả lời đúng là 2. Việc triển khai tuần tự coi mỗi lần thay thế là một vòng riêng biệt có thể tính các thao tác này không chính xác. 

Vì`~00`, sự thay thế sẽ tạo ra mã ASCII 0, không thể in được và nằm ngoài bảng chữ cái đầu vào. Nó vẫn phải được biểu diễn nội bộ dưới dạng giá trị ký tự. Câu trả lời đúng là 1 và ký tự kết quả không thể bắt đầu mã hóa khác. 

Vì`~7g`, bộ ba không hợp lệ vì`g`không phải là chữ số thập lục phân. Không có gì xảy ra nên câu trả lời là 0. Chỉ kiểm tra xem hai ký tự có phải là chữ cái hay không sẽ chấp nhận trường hợp này sai. 

Các chữ số thập lục phân viết hoa và viết thường đều hợp lệ. Như vậy`~7E`là một mẫu hợp lệ và có câu trả lời 1. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng chính xác quá trình giải mã. Trong một vòng, quét chuỗi hiện tại từ trái sang phải. Bất cứ khi nào`~XY`được tìm thấy, nối ký tự đã giải mã vào chuỗi tiếp theo và bỏ qua ba vị trí. Nếu không thì nối thêm ký tự hiện tại không thay đổi. Sau khi quét, thay thế chuỗi hiện tại bằng chuỗi mới được tạo và lặp lại cho đến khi không tìm thấy mẫu nào. 

Mô phỏng này đúng vì nó tuân theo chính xác định nghĩa đồng thời của một vòng đấu. Các mẫu không thể chồng lên nhau: sau phần dẫn đầu`~`, hai ký tự tiếp theo phải là chữ số thập lục phân, do đó mẫu khác không thể bắt đầu ở một trong hai vị trí đó. Vấn đề là thời gian chạy của nó. Một vòng không cần thiết làm giảm độ dài chuỗi ít nhất hai, do đó có thể có Θ(n) vòng. Nếu chỉ có một mẫu biến mất mỗi vòng thì quá trình quét có độ dài xấp xỉ`n, n-2, n-4, ...`, cho phép xử lý ký tự Θ(n2). Với n = 300.000 thì đây là hàng chục tỷ phép tính. 

Quan sát quan trọng là chúng ta không thực sự cần cụ thể hóa mọi chuỗi trung gian. Mỗi lần thay thế sẽ tạo ra một ký tự mới và ký tự đó có số tròn được xác định rõ ràng để nó xuất hiện. 

Hãy tưởng tượng rằng mỗi ký tự hiện tại là một đối tượng chứa hai giá trị: mã ASCII của nó và vòng mà ký tự đó được tạo. Các ký tự gốc có vòng 0. Giả sử ba đối tượng liền kề nhau tạo thành`~XY`. Việc thay thế chỉ có thể xảy ra sau khi cả ba đối tượng đã tồn tại, do đó ký tự mới xuất hiện trong vòng`1 + max(depth of the three objects)`. 

Điều này biến toàn bộ quá trình thành một cây rút gọn. Thay vì mô phỏng các vòng trên toàn cầu, chúng ta có thể xây dựng các mức rút gọn này cục bộ trong khi quét chuỗi gốc. 

Một ngăn xếp là đủ vì mỗi bộ ba được mã hóa sẽ tiêu thụ ba ký tự hiện tại liền kề và tạo ra một ký tự. Khi một ký tự mới được thêm vào, mẫu mới duy nhất có thể có là một mẫu kết thúc ở ký tự đó hoặc một mẫu được tạo bằng cách rút gọn ngay trước nó. Chúng ta có thể giảm liên tục các mẫu như vậy ở đầu ngăn xếp. Độ sâu được lưu trữ với mỗi ký tự kết quả chính xác là vòng mà nó sẽ xuất hiện. 

Lý do quan trọng khiến tính năng này hoạt động với các vòng đồng thời là vì hai mẫu hợp lệ không bao giờ có thể trùng nhau. Nếu một mẫu hiện có sẵn, việc giảm mẫu đó không thể làm mất hiệu lực của mẫu khác hiện có. Mẫu sau sử dụng ký tự mới chỉ nhận được độ sâu lớn hơn, tương ứng chính xác với việc chờ đợi vòng giải mã tiếp theo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Ngăn xếp tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước giá trị thập lục phân của mọi byte có thể. Dành cho ký tự`0`bởi vì`9`,`A`bởi vì`F`, Và`a`bởi vì`f`, lưu trữ các giá trị từ 0 đến 15. Mọi ký tự khác đều nhận được điểm đánh dấu không hợp lệ. Điều này tránh việc tìm kiếm chuỗi lặp lại và xử lý trực tiếp cả hai trường hợp ký hiệu thập lục phân. 
2. Quét đầu vào từ trái sang phải. Đối với mỗi ký tự gốc, đẩy mã ASCII của nó vào ngăn xếp cùng với độ sâu 0. Các ký tự gốc đã tồn tại trước vòng giải mã đầu tiên. 
3. Sau khi đẩy một ký tự, hãy kiểm tra xem ba mục ngăn xếp cuối cùng có dạng không`~XY`. Các mục nhập biểu thị các ký tự liền kề của chuỗi rút gọn hiện tại, bởi vì mỗi lần rút gọn trước đó đã thay thế một khối liên tiếp bằng một mục nhập ngăn xếp. 
4. Nếu ba mục cuối cùng tạo thành một mẫu hợp lệ, hãy xóa chúng và tính mã ASCII đã giải mã như sau:`16 * value(X) + value(Y)`. Độ sâu của nó lớn hơn một độ sâu tối đa của ba mục đã bị xóa. 
5. Đẩy ký tự kết quả và độ sâu đã tính toán của nó trở lại ngăn xếp. Ký tự mới đại diện cho toàn bộ khối đầu vào ban đầu đã được rút gọn. 
6. Lặp lại việc kiểm tra rút gọn trong khi phần trên cùng của ngăn xếp chứa mẫu hợp lệ. Một ký tự mới được tạo có thể tạo một mẫu có hai ký tự ngay trước nó, vì vậy chỉ kiểm tra một lần sẽ bỏ lỡ chuỗi rút gọn. 
7. Giữ độ sâu tối đa từng được tạo ra. Độ sâu đó là số vòng giải mã không cần thiết liên tiếp tối đa. 

### Tại sao nó hoạt động 

Duy trì bất biến rằng mỗi mục nhập ngăn xếp đại diện cho một khối liền kề của chuỗi gốc sau khi tất cả việc rút gọn bên trong khối đó đã được thực hiện và độ sâu được lưu trữ của nó chính xác là vòng mà ký tự hiện tại của nó xuất hiện. 

Khi ba mục liền kề hình thành`~XY`, các ký tự tương ứng tồn tại đồng thời sau khi độ sâu được lưu trữ của chúng đã hết. Vòng sớm nhất có thể mà cả ba đều có mặt là một vòng lớn hơn độ sâu tối đa của chúng, vì vậy việc thay thế chúng sẽ tạo ra chính xác đặc điểm mà quy trình thực sự tạo ra trong vòng đó. Bởi vì các mẫu hợp lệ không thể trùng lặp nên việc thực hiện việc giảm cục bộ này không thể ảnh hưởng đến việc giảm khác sẽ xảy ra trong cùng một vòng. 

Mỗi lần giảm do ngăn xếp thực hiện đều tương ứng với một thao tác giải mã thực và mọi thao tác giải mã thực cuối cùng sẽ hiển thị dưới dạng bộ ba hợp lệ trên ngăn xếp. Do đó, độ sâu được lưu trữ lớn nhất chính xác là vòng không cần thiết cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def decode_rounds(s: str) -> int:
    hex_value = [-1] * 256

    for i in range(10):
        hex_value[ord('0') + i] = i

    for i in range(6):
        hex_value[ord('A') + i] = 10 + i
        hex_value[ord('a') + i] = 10 + i

    stack_char = []
    stack_depth = []

    answer = 0

    for ch in s:
        stack_char.append(ord(ch))
        stack_depth.append(0)

        while len(stack_char) >= 3:
            c0 = stack_char[-3]
            c1 = stack_char[-2]
            c2 = stack_char[-1]

            if c0 != 126:
                break

            v1 = hex_value[c1]
            v2 = hex_value[c2]

            if v1 == -1 or v2 == -1:
                break

            d = max(
                stack_depth[-3],
                stack_depth[-2],
                stack_depth[-1]
            ) + 1

            value = 16 * v1 + v2

            stack_char.pop()
            stack_char.pop()
            stack_char.pop()

            stack_depth.pop()
            stack_depth.pop()
            stack_depth.pop()

            stack_char.append(value)
            stack_depth.append(d)

            if d > answer:
                answer = d

    return answer

def main() -> None:
    s = input().rstrip('\n')
    print(decode_rounds(s))

if __name__ == "__main__":
    main()
```Bảng tra cứu thập lục phân được xây dựng một lần. Việc sử dụng một mảng được lập chỉ mục bởi mã ASCII giúp việc kiểm tra một ký tự có thời gian không đổi và nó xử lý các giá trị được giải mã từ 0 đến 255 một cách tự nhiên. 

Hai ngăn xếp được giữ song song.`stack_char`lưu trữ ký tự hiện tại của từng khối rút gọn, trong khi`stack_depth`lưu trữ vòng mà ký tự đó tồn tại. Việc giữ các mảng riêng biệt sẽ tránh việc phân bổ một bộ dữ liệu mỗi khi một ký tự được đẩy hoặc giảm. 

Mỗi ký tự đầu vào được đẩy chính xác một lần. Việc giảm thành công sẽ loại bỏ ba mục và thêm một mục, do đó, mỗi lần giảm sẽ giảm kích thước ngăn xếp xuống hai. Có thể có nhiều nhất`floor(n / 2)`sự giảm bớt. Bên trong`while`do đó, vòng lặp chỉ được thực hiện tổng cộng O(n) lần, mặc dù nó được lồng bên trong quá trình quét. 

Kiểm tra ranh giới`len(stack_char) >= 3`là cần thiết trước khi đọc ba yếu tố cuối cùng. Bảng thập lục phân cũng xử lý các ký tự được giải mã dưới 33 và trên 126 mà không có bất kỳ trường hợp đặc biệt nào, vì tất cả các ký tự bên trong được biểu diễn đơn giản bằng các giá trị nguyên ASCII của chúng. 

Số nguyên Python có độ chính xác tùy ý, do đó việc tính toán độ sâu không gặp phải vấn đề tràn. Trong thực tế độ sâu tối đa là nhiều nhất`floor((n - 1) / 2)`. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu`~7eFf`. Hình thức ba ký tự đầu tiên`~7e`, giải mã thành một dấu ngã khác. Dấu ngã mới đó ngay sau đó là`Ff`, do đó có thể giảm lần thứ hai. 

| Ký tự đầu vào | Xếp chồng ký tự sau khi giảm | Độ sâu tối đa | 
| --- | --- | --- | 
|`~`|`~`| 0 | 
|`7`|`~7`| 0 | 
|`e`|`~`| 1 | 
|`F`|`~F`| 1 | 
|`f`|`ÿ`| 2 | 

Sau khi xử lý`e`, ngăn xếp chứa dấu ngã có độ sâu 1. Đang xử lý`F`Và`f`hoàn thành một bộ ba hợp lệ khác, do đó ký tự được giải mã có độ sâu 2. Không thể rút gọn sau này, đưa ra câu trả lời 2. Điều này thể hiện ý tưởng trung tâm rằng một ký tự được tạo trong một vòng có thể tham gia vào vòng sau. 

Bây giờ hãy xem xét mẫu`~~37~45~fF`. 

| Ký tự đầu vào | Xếp chồng ký tự sau khi giảm | Độ sâu tối đa | 
| --- | --- | --- | 
|`~`|`~`| 0 | 
|`~`|`~~`| 0 | 
|`3`|`~~3`| 0 | 
|`7`|`~7`| 1 | 
|`~`|`~7~`| 1 | 
|`4`|`~7~4`| 1 | 
|`5`|`~`| 2 | 
|`~`|`~~`| 2 | 
|`f`|`~~f`| 2 | 
|`F`|`~ÿ`| 2 | 

Sau khi đọc`7`, chuỗi con`~37`trở thành nhân vật`7`, cho chiều sâu 1. Sau khi đọc`5`, ngăn xếp tạm thời hình thành`~7E`, bản thân nó là một mã hóa hợp lệ và do đó giảm xuống còn`~`ở độ sâu 2. Trận chung kết`~fF`trở thành`0xFF`ở độ sâu 1 và không tạo ra dấu ngã khác. Độ sâu tối đa là 2. 

Bảng này cũng cho thấy tại sao ngăn xếp có thể thực hiện các phép rút gọn không có trong chuỗi gốc. Khi`~7E`xuất hiện, nó biểu thị trạng thái của chuỗi sau một vòng giải mã thực, do đó việc giảm nó ngay lập tức trong khi xây dựng cây rút gọn tương đương với việc mô phỏng vòng thứ hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự đầu vào được đẩy một lần và mỗi lần rút gọn sẽ loại bỏ ba mục nhập và tạo một mục nhập. | 
| Không gian | O(n) | Ngăn xếp có thể chứa một mục nhập cho mỗi ký tự gốc. | 

Với`n <= 300000`, giải pháp tuyến tính chỉ thực hiện một lượng công việc không đổi trên mỗi ký tự đầu vào và trên mỗi lần rút gọn. Mức tiêu thụ bộ nhớ cũng tuyến tính và duy trì ở mức thoải mái trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def decode_rounds(s: str) -> int:
    hex_value = [-1] * 256

    for i in range(10):
        hex_value[ord('0') + i] = i

    for i in range(6):
        hex_value[ord('A') + i] = 10 + i
        hex_value[ord('a') + i] = 10 + i

    stack_char = []
    stack_depth = []
    answer = 0

    for ch in s:
        stack_char.append(ord(ch))
        stack_depth.append(0)

        while len(stack_char) >= 3:
            c0 = stack_char[-3]
            c1 = stack_char[-2]
            c2 = stack_char[-1]

            if c0 != ord('~'):
                break

            v1 = hex_value[c1]
            v2 = hex_value[c2]

            if v1 == -1 or v2 == -1:
                break

            depth = max(
                stack_depth[-3],
                stack_depth[-2],
                stack_depth[-1]
            ) + 1

            value = 16 * v1 + v2

            stack_char.pop()
            stack_char.pop()
            stack_char.pop()

            stack_depth.pop()
            stack_depth.pop()
            stack_depth.pop()

            stack_char.append(value)
            stack_depth.append(depth)

            answer = max(answer, depth)

    return answer

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return str(decode_rounds(inp.rstrip('\n')))

# Provided samples
assert run("~7e") == "1", "sample 1"
assert run("~~37~45~fF") == "2", "sample 2"
assert run("~~30~30~7e7E") == "2", "sample 3"
assert run("~7eFf") == "2", "sample 4"
assert run("~hello") == "0", "sample 5"

# Minimum-size input
assert run("a") == "0", "minimum length"

# No valid hexadecimal pair
assert run("~7g") == "0", "invalid hexadecimal digit"

# A decoded non-printable character must still be handled
assert run("~00") == "1", "decoded character below input alphabet"

# Newly created tilde enables another round
assert run("~7e7e") == "2", "new tilde creates another round"

# Maximum-size input, with the maximum possible number of rounds.
# Length is exactly 300000.
max_input = "~" + "7e" * 149999 + "!"
assert len(max_input) == 300000
assert run(max_input) == "149999", "maximum length and maximum depth"

# All characters equal
assert run("~" * 300000) == "0", "all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`| 0 | Kích thước đầu vào tối thiểu và không có mẫu | 
|`~7g`| 0 | Ký tự thập lục phân không hợp lệ | 
|`~00`| 1 | Giá trị được giải mã ngoài bảng chữ cái đầu vào có thể in được | 
|`~7e7e`| 2 | Dấu ngã được tạo ra sẽ tham gia vào vòng tiếp theo | 
|`~`+`7e`lặp lại 149999 lần +`!`| 149999 | Kích thước đầu vào tối đa và độ sâu tối đa có thể | 
|`~`lặp đi lặp lại 300000 lần | 0 | Đầu vào lớn không giảm | 

## Vỏ cạnh 

Đối với chuỗi`~7e7e`, ngăn xếp đọc đầu tiên`~7e`và thay thế nó bằng dấu ngã có độ sâu 1. Phần còn lại`7e`bây giờ nằm ​​ngay sau dấu ngã được tạo đó, do đó ngăn xếp tạo thành một bộ ba hợp lệ khác và thay thế nó bằng dấu ngã có độ sâu 2. Ngăn xếp cuối cùng chỉ chứa dấu ngã đó và câu trả lời là 2. Điều này nắm bắt các triển khai chỉ kiểm tra các mẫu đầu vào ban đầu. 

Vì`~~37~45~fF`, các mẫu ban đầu là đồng thời. Ngăn xếp giảm`~37`ĐẾN`7`với độ sâu 1 thì`~45`ĐẾN`E`với độ sâu 1. Hai mức giảm đó phơi bày`~7E`, trở thành dấu ngã có độ sâu 2. Trận chung kết`~fF`trở thành`0xFF`với độ sâu 1. Câu trả lời là 2. Điều này nắm bắt các triển khai gây nhầm lẫn giữa các thay thế riêng lẻ với các vòng giải mã. 

Vì`~00`, ngăn xếp nhận dạng cả hai chữ số là chữ số thập lục phân hợp lệ và tạo ra giá trị nguyên 0. Độ sâu trở thành 1 và kết quả là byte 0 không thể tạo thành mẫu mới vì nó không phải là dấu ngã. Câu trả lời là 1. Thuật toán không bao giờ giả định rằng các ký tự trung gian có thể in được, điều này cần thiết vì các giá trị được giải mã có thể chiếm toàn bộ phạm vi byte. 

Vì`~7g`, ký tự đầu tiên là dấu ngã và`7`là một chữ số thập lục phân hợp lệ, nhưng`g`có giá trị tra cứu`-1`. Ngăn xếp không thay đổi cả ba ký tự, do đó không tính mức giảm và câu trả lời là 0. Điều này chứng tỏ tại sao xác thực thập lục phân phải chấp nhận chính xác`0`bởi vì`9`,`A`bởi vì`F`, Và`a`bởi vì`f`. 

Đối với chuỗi kích thước tối đa`~`tiếp theo là 149999 bản sao của`7e`và sau đó`!`, độ dài chính xác là 300000. Mỗi vòng sẽ loại bỏ vòng đầu tiên`7e`khỏi chuỗi hoạt động và để lại một chuỗi khác`~7e`mẫu, vậy có chính xác 149999 vòng không tầm thường. trận chung kết`!`ngăn chặn bất kỳ vòng bổ sung nào. Đây là trường hợp cấu trúc tồi tệ nhất cho câu trả lời và cũng chứng minh tại sao mô phỏng từng vòng có thể yêu cầu thời gian bậc hai. 

Đối với chuỗi bao gồm 300000 dấu ngã, không có dấu ngã nào được theo sau bởi hai chữ số thập lục phân, do đó ngăn xếp không bao giờ thực hiện rút gọn. Mọi mục nhập đều có độ sâu 0 và câu trả lời vẫn là 0. Điều này xác nhận rằng thuật toán không vô tình coi dấu ngã là mã hóa.
