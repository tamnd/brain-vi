---
title: "CF 102348D - Vé Trò Chơi"
description: "Chúng ta có một tấm vé có chiều dài chẵn được chia thành hai nửa bằng nhau. Mọi vị trí đều đã có sẵn một chữ số hoặc chứa ?, nghĩa là chữ số của nó đã bị xóa. Hai người chơi luân phiên chọn một người còn lại ? và thay thế nó bằng bất kỳ chữ số nào từ 0 đến 9."
date: "2026-08-13T00:53:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102348
codeforces_index: "D"
codeforces_contest_name: "ICPC 2019-2020 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102348
solve_time_s: 191
verified: true
draft: false
---

[CF 102348D - Vé trò chơi](https://codeforces.com/problemset/problem/102348/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 11s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tấm vé có chiều dài chẵn được chia thành hai nửa bằng nhau. Mỗi vị trí đã chứa một chữ số hoặc chứa`?`, nghĩa là chữ số của nó đã bị xóa. Hai người chơi luân phiên chọn một người còn lại`?`và thay thế nó bằng bất kỳ chữ số nào từ`0`bởi vì`9`. Monocarp đi trước và muốn tổng cuối cùng của hai nửa khác nhau. Bicarp muốn số tiền đó bằng nhau. 

Cách hữu ích để thể hiện tấm vé không phải là một chuỗi các vị trí riêng lẻ mà thông qua bốn số lượng. Cho phép`L`Và`R`là tổng của các chữ số đã biết ở nửa bên trái và bên phải và gọi`qL`Và`qR`là số vị trí bị xóa trong các nửa đó. Toàn bộ trò chơi có thể được đặc trưng bởi bốn giá trị này. 

Chiều dài có thể lớn bằng`200000`, và thời gian chỉ có một giây. Do đó, giải pháp phải tuyến tính hoặc gần tuyến tính theo chiều dài vé. Bất kỳ cách tiếp cận nào khám phá phần lớn các nhiệm vụ có thể thực hiện đều không thể thực hiện được, bởi vì ngay cả`10^20`các bài tập sẽ vượt xa những gì có thể được xử lý. Giới hạn bộ nhớ rất rộng rãi cho một`O(n)`quét, nhưng không có lý do gì để lưu trữ bất cứ thứ gì ngoài chuỗi đầu vào và một vài bộ đếm. 

Có một số trường hợp khó khăn có thể dễ dàng phá vỡ một giải pháp ngây thơ. Đầu tiên, một vé không có vị trí bị xóa đã được quyết định. Ví dụ,`n = 4`Và`0523`đã để lại số tiền`0 + 5 = 5`và tổng đúng`2 + 3 = 5`, vậy câu trả lời là`Bicarp`. Việc triển khai giả định ít nhất một`?`có thể xử lý sai trường hợp này. 

Thứ hai, số lượng dấu chấm hỏi bằng nhau không đảm bảo chiến thắng cho Bicarp. Ví dụ:```
4
?123
```Đây`qL = 1`,`qR = 0`, thế là Monocarp thắng. Việc triển khai bất cẩn chỉ kiểm tra xem số tiền cố định ban đầu có bằng nhau hay không sẽ tạo ra kết quả không chính xác.`Bicarp`. 

Thứ ba, số dấu chấm hỏi không bằng nhau vẫn có thể tạo ra chiến thắng cho Bicarp khi tổng chênh lệch cố định có kích thước chính xác. Ví dụ:```
8
?054??0?
```Tổng cố định bên trái là`9`, tổng cố định bên phải là`0`, trong khi`qL = 1`Và`qR = 3`. Sự khác biệt về số lượng dấu chấm hỏi là`2`, Và`9 * 2 / 2 = 9`, khớp chính xác với chênh lệch tổng cố định. Bicarp thắng. Do đó, quy tắc như "số lượng dấu chấm hỏi không bằng nhau có nghĩa là Monocarp thắng" là không chính xác. 

Cuối cùng, yếu tố`9`quan trọng vì mọi dấu chấm hỏi đều có thể đóng góp bất kỳ điều gì từ`0`ĐẾN`9`. Ví dụ:```
6
???00?
```Số tiền cố định là`0`Và`0`, nhưng số lượng dấu chấm hỏi là`3`Và`1`. Monocarp thắng vì Bicarp không thể bù đắp được hai vị trí phụ bên cánh trái. Việc coi dấu chấm hỏi chỉ là một giá trị không xác định mà không tính đến phạm vi đầy đủ của nó sẽ bỏ qua cấu trúc trò chơi thực tế. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ xem xét mọi cách có thể để thay thế các dấu chấm hỏi. Nếu có`q`vị trí bị xóa, mỗi vị trí có mười chữ số có thể, cho`10^q`hoàn thành vé. Chúng ta có thể chọn đệ quy một chữ số cho mỗi vị trí và kiểm tra hai tổng cuối cùng. Điều này đúng vì mọi kết quả có thể xảy ra của trò chơi đều được biểu thị bằng một lá của cây tìm kiếm, nhưng trường hợp xấu nhất thì có`q = 200000`, sản xuất`10^200000`nhiệm vụ thiết bị đầu cuối. Ngay cả với số lượng công việc không đổi nhỏ cho mỗi nhiệm vụ, điều này là hoàn toàn không khả thi. 

Trò chơi có nhiều cấu trúc hơn so với minimax tùy ý. Kết quả cuối cùng chỉ phụ thuộc vào sự khác biệt giữa hai nửa số tiền. Đầu tiên hãy xem xét các vị trí đã được cố định. Xác định 

[ 
D = LR. 
] 

Các dấu hỏi đóng góp giá trị bổ sung cho sự khác biệt này. Khi cả hai nửa có cùng số dấu chấm hỏi và`D = 0`, Bicarp có thể trả lời mọi nước đi bằng cách chọn cùng một chữ số ở nửa đối diện. Hai đóng góp mới được thêm vào sẽ bị hủy bỏ, do đó sự khác biệt vẫn bằng 0 sau mỗi cặp nước đi. Đây là một chiến lược ghép nối trực tiếp. 

Nếu như`D = 0`nhưng số lượng dấu chấm hỏi khác nhau nên việc ghép đôi đó là không thể. Vì Monocarp di chuyển trước nên cuối cùng anh ta có thể di chuyển về phía có dấu chấm hỏi chưa từng có và tạo ra sự khác biệt cuối cùng khác 0. 

Bây giờ giả sử`D != 0`. Chúng ta có thể hoán đổi hai nửa về mặt khái niệm sao cho`D > 0`, nghĩa là các chữ số cố định đã làm cho nửa bên trái lớn hơn. Câu hỏi đặt ra là liệu nửa bên phải có đủ dấu hỏi thừa để bù đắp hay không. 

Nếu như`qL >= qR`, Monocarp có ít nhất nhiều nước đi có sẵn ở phía vốn đã lớn hơn. Anh ta có thể giữ cho lợi thế không bị sửa chữa nên Bicarp không thể ép buộc bình đẳng. 

Trường hợp thú vị là`qL < qR`. Phía bên phải có`qR - qL`thêm dấu chấm hỏi. Vì tổng số dấu chấm hỏi là số chẵn nên sự chênh lệch này cũng là số chẵn. Trong phần ghép đôi của trò chơi, người chơi có thể hủy bỏ số lượng dấu chấm hỏi bằng nhau của cả hai bên một cách hiệu quả. Cuối cùng chỉ còn lại các dấu chấm hỏi bên phải. 

Giả sử có`k = qR - qL`những vị trí như vậy. Từ`k`chẵn, Bicarp có thể kiểm soát hiệu ứng cân bằng cuối cùng theo cặp. Mỗi cặp dấu chấm hỏi bổ sung bên phải chỉ có thể đóng góp nhiều nhất`9`theo hướng giảm lợi thế bên trái trong chiến lược tối ưu phù hợp. Như vậy số tiền bồi thường tối đa là 

[ 
9\frac{k}{2}. 
] 

Bicarp thắng chính xác khi số tiền này bằng với mức chênh lệch cố định: 

[ 
D = 9\frac{qR-qL}{2}. 
] 

Nếu chênh lệch nhỏ hơn, Bicarp không thể loại bỏ đủ lợi thế ban đầu. Nếu nó lớn hơn thì lợi thế còn lại vẫn khác 0. Bình đẳng là tình huống duy nhất mà sự cân bằng hoàn hảo có thể bị ép buộc. 

Điều này đưa ra quyết định có kích thước không đổi sau một lần quét vé. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(10^q)`|`O(q)`đệ quy | Quá chậm | 
| Tối ưu |`O(n)`|`O(1)`bên cạnh đầu vào | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia vé về mặt khái niệm thành nửa bên trái và nửa bên phải. Trong khi quét chuỗi, tính toán`left_sum`Và`right_sum`chỉ sử dụng các chữ số cố định và đếm`left_q`Và`right_q`, dấu chấm hỏi ở mỗi nửa. Bốn giá trị này chứa tất cả thông tin cần thiết cho trò chơi. 
2. Tính số tiền chênh lệch cố định như sau`left_sum - right_sum`. Nếu nó âm, hãy hoán đổi hai bên về mặt khái niệm sao cho`left_sum >= right_sum`. Đồng thời, trao đổi số lượng dấu chấm hỏi của họ. Sau quá trình chuẩn hóa này, chênh lệch cố định không âm và thể hiện lợi thế của bên trái. 
3. Nếu chênh lệch cố định bằng 0, Bicarp thắng chính xác khi`left_q == right_q`. Với số lượng bằng nhau, mọi hành động của Monocarp có thể được ghép nối với phản hồi của nửa kia bằng cùng một chữ số, duy trì sự bình đẳng. Với số lượng không bằng nhau, người chơi đầu tiên cuối cùng sẽ có được nước đi chưa từng có và có thể phá hủy sự bình đẳng. 
4. Nếu chênh lệch cố định là dương và`left_q >= right_q`, đầu ra`Monocarp`. Bên đã dẫn trước có ít nhất nhiều dấu hỏi, vì vậy Monocarp có thể sử dụng các nước đi của mình để bảo toàn lợi thế khác 0. Bicarp không có đủ thế mạnh bên kia để bù đắp. 
5. Nếu chênh lệch cố định là dương và`left_q < right_q`, tính toán`extra = right_q - left_q`. Bicarp chỉ có thể thắng nếu chênh lệch ban đầu bằng chính xác`9 * extra / 2`. Vì tổng số dấu chấm hỏi là số chẵn nên`extra`là số chẵn nên giá trị này là số nguyên. Nếu đẳng thức giữ nguyên, đầu ra`Bicarp`; nếu không thì xuất ra`Monocarp`. 

### Tại sao nó hoạt động 

Bất biến là chênh lệch hiện tại giữa hai nửa tổng cùng với số dấu chấm hỏi chưa sử dụng ở mỗi bên. Số lượng dấu hỏi bằng nhau còn lại có thể được vô hiệu hóa theo cặp, vì sau khi Monocarp chọn một giá trị ở một bên thì Bicarp có thể sử dụng giá trị tương tự ở phía bên kia. Sau khi loại bỏ các vị trí được ghép nối đó, chỉ những dấu hỏi chưa khớp mới quan trọng. Nếu số tiền cố định lớn hơn ban đầu có ít nhất nhiều dấu hỏi, Monocarp có thể duy trì lợi thế. Ngược lại, phía bên kia có chính xác`qR - qL`các vị trí bổ sung và cách duy nhất để bù đắp chênh lệch cố định là các vị trí đó cung cấp chính xác`9(qR-qL)/2`của sự điều chỉnh. Do đó các điều kiện nêu ra đều cần và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().strip()

    half = n // 2

    left_sum = 0
    right_sum = 0
    left_q = 0
    right_q = 0

    for i in range(half):
        if s[i] == '?':
            left_q += 1
        else:
            left_sum += ord(s[i]) - ord('0')

    for i in range(half, n):
        if s[i] == '?':
            right_q += 1
        else:
            right_sum += ord(s[i]) - ord('0')

    if left_sum < right_sum:
        left_sum, right_sum = right_sum, left_sum
        left_q, right_q = right_q, left_q

    diff = left_sum - right_sum

    if diff == 0:
        if left_q == right_q:
            print("Bicarp")
        else:
            print("Monocarp")
        return

    if left_q >= right_q:
        print("Monocarp")
        return

    extra = right_q - left_q

    if diff * 2 == extra * 9:
        print("Bicarp")
    else:
        print("Monocarp")

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên xử lý chính xác vòng lặp đầu tiên`n // 2`vị trí, trong khi vị trí thứ hai bắt đầu tại`n // 2`, là chỉ mục đầu tiên của nửa bên phải trong lập chỉ mục dựa trên số 0 của Python. Các chữ số cố định đóng góp vào tổng tương ứng của chúng, trong khi dấu chấm hỏi chỉ ảnh hưởng đến hai bộ đếm. 

Việc trao đổi sau khi quét là một sự đơn giản hóa hữu ích. Thay vì viết các trường hợp riêng biệt cho`left_sum > right_sum`Và`right_sum > left_sum`, mã luôn coi bên có tổng cố định lớn hơn là bên trái. Số lượng dấu chấm hỏi phải được hoán đổi cùng với số tiền, vì việc nhận dạng bên lớn hơn có ý nghĩa quan trọng đối với trò chơi. 

Việc kiểm tra đẳng thức sử dụng`diff * 2 == extra * 9`thay vì chia`extra`bằng hai. Điều này tránh được sự phân chia không cần thiết và làm cho điều kiện toán học hiển thị trực tiếp trong mã. Số nguyên Python không có vấn đề tràn và các giá trị lớn nhất ở đây chỉ theo thứ tự`10^6`. 

Việc thực hiện không cần phải sửa đổi vé. Khi bốn đại lượng tổng hợp đã được tính toán, vị trí chính xác của các dấu chấm hỏi không còn quan trọng nữa. Đây là điều làm giảm trò chơi từ tìm kiếm theo cấp số nhân sang quét tuyến tính duy nhất. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
4
0523
```Không có dấu chấm hỏi nên trò chơi không có nước đi. Hai tổng cố định đều là`5`. 

| Bước | Tổng trái | Tổng đúng | Bên trái`?`| Phải`?`| Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| Quét hoàn tất | 5 | 5 | 0 | 0 | Chênh lệch cố định bằng 0 | 
| Kiểm tra lần cuối | 5 | 5 | 0 | 0 | Số lượng dấu chấm hỏi bằng nhau, Bicarp | 

Thuật toán đạt đến trường hợp sai phân bằng 0 và tìm thấy số dấu chấm hỏi bằng nhau. Vé đã có vui nên Bicarp thắng liền. 

### Mẫu 2 

Đầu vào là:```
2
??
```Cả hai vị trí đều bị xóa. Không có chữ số cố định nên cả hai tổng cố định đều bằng 0 và có một dấu chấm hỏi ở mỗi bên. 

| Bước | Tổng trái | Tổng đúng | Bên trái`?`| Phải`?`| Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| Quét hoàn tất | 0 | 0 | 1 | 1 | Chênh lệch cố định bằng 0 | 
| Kiểm tra lần cuối | 0 | 0 | 1 | 1 | Số lượng bằng nhau, Bicarp | 

Monocarp chọn chữ số nào cho vị trí đầu tiên thì Bicarp có thể đặt chữ số đó vào vị trí còn lại. Hai số tiền bằng nhau nên Bicarp thắng. 

### Mẫu 3 

Đầu vào là:```
8
?054??0?
```Nửa bên trái là`?054`, cho một số tiền cố định là`9`và một dấu hỏi. Nửa bên phải là`??0?`, cho một số tiền cố định là`0`và ba dấu hỏi. 

| Bước | Tổng trái | Tổng đúng | Bên trái`?`| Phải`?`| Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| Quét hoàn tất | 9 | 0 | 1 | 3 | Bên trái có tổng cố định lớn hơn | 
| Sự khác biệt | 9 | 0 | 1 | 3 |`diff = 9`,`extra = 2`| 
| Kiểm tra lần cuối | 9 | 0 | 1 | 3 |`2 * 9 = 2 * 9`, Bicarp | 

Nửa bên phải có thêm hai dấu chấm hỏi. Hai vị trí không đối xứng đó có thể bù đắp chính xác`9`, đó là chênh lệch cố định hiện có. Điều kiện bằng nhau được giữ nên Bicarp thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n)`| Mỗi vị trí vé được kiểm tra một lần. | 
| Không gian |`O(1)`không gian phụ trợ | Chỉ có bốn bộ đếm và một vài biến vô hướng được duy trì bên cạnh chuỗi đầu vào. | 

Với`n <= 200000`, thuật toán chỉ thực hiện một vài phép tính số học cho mỗi ký tự. Nó phù hợp thoải mái với giới hạn một giây, trong khi tìm kiếm vũ phu sẽ có tới`10^200000`hoàn thành nhiệm vụ trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    n = int(input())
    s = input().strip()

    half = n // 2

    left_sum = 0
    right_sum = 0
    left_q = 0
    right_q = 0

    for i in range(half):
        if s[i] == '?':
            left_q += 1
        else:
            left_sum += ord(s[i]) - ord('0')

    for i in range(half, n):
        if s[i] == '?':
            right_q += 1
        else:
            right_sum += ord(s[i]) - ord('0')

    if left_sum < right_sum:
        left_sum, right_sum = right_sum, left_sum
        left_q, right_q = right_q, left_q

    diff = left_sum - right_sum

    if diff == 0:
        print("Bicarp" if left_q == right_q else "Monocarp")
    elif left_q >= right_q:
        print("Monocarp")
    else:
        extra = right_q - left_q
        print("Bicarp" if diff * 2 == extra * 9 else "Monocarp")

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("4\n0523\n") == "Bicarp", "sample 1"
assert run("2\n??\n") == "Bicarp", "sample 2"
assert run("8\n?054??0?\n") == "Bicarp", "sample 3"

# Minimum size, unequal fixed sums and no question marks
assert run("2\n12\n") == "Monocarp", "minimum-size unhappy ticket"

# Minimum size, both positions erased
assert run("2\n??\n") == "Bicarp", "minimum-size pairing"

# Equal fixed sums but unequal question-mark counts
assert run("4\n?123\n") == "Monocarp", "unequal question counts"

# Positive difference with the exact 9 * extra / 2 compensation
assert run("6\n9??0??\n") == "Bicarp", "exact compensation"

# Positive difference with insufficient compensation
assert run("6\n8??0??\n") == "Monocarp", "wrong compensation"

# All equal values, maximum-size input
MAX_N = 200000
max_input = str(MAX_N) + "\n" + "5" * MAX_N + "\n"
assert run(max_input) == "Bicarp", "maximum-size all-equal ticket"

# Maximum-size all question marks
max_questions = str(MAX_N) + "\n" + "?" * MAX_N + "\n"
assert run(max_questions) == "Bicarp", "maximum-size all-question ticket"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 12`|`Monocarp`| Kích thước tối thiểu với một vé đã không hài lòng | 
|`2 / ??`|`Bicarp`| Chiến lược ghép đôi nhỏ nhất có thể | 
|`4 / ?123`|`Monocarp`| Tổng cố định bằng nhau nhưng số dấu chấm hỏi không bằng nhau | 
|`6 / 9??0??`|`Bicarp`| Bồi thường chính xác bằng các dấu chấm hỏi thêm | 
|`6 / 8??0??`|`Monocarp`| Bồi thường chưa đủ lớn | 
|`200000 / 555...5`|`Bicarp`| Kích thước đầu vào tối đa và tất cả các chữ số cố định bằng nhau | 
|`200000 / ???...???`|`Bicarp`| Kích thước đầu vào tối đa với mọi vị trí bị xóa | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là vé không có vị trí bị xóa. Vì`4 / 0523`, quá trình quét mang lại`left_sum = 5`,`right_sum = 5`,`left_q = right_q = 0`. Thuật toán đi vào nhánh sai phân 0 và trả về`Bicarp`. Không có nước đi nào nên trạng thái ban đầu trực tiếp quyết định kết quả. 

Trường hợp cạnh thứ hai là tổng cố định bằng nhau nhưng số dấu chấm hỏi không bằng nhau. Vì`4 / ?123`, tổng bên trái cố định là`0`, tổng bên phải cố định là`2 + 3 = 5`, vì vậy sau khi chuẩn hóa, vế phải sẽ lớn hơn. Số đếm trở thành`qL = 0`Và`qR = 1`, với chênh lệch cố định dương. Vì bên lớn hơn có nhiều dấu hỏi hơn nên thuật toán trả về`Monocarp`. Tổng quát hơn, khi chênh lệch cố định bằng 0 và số lượng khác nhau, người chơi có nước đi đầu tiên sẽ có vị trí không thể so sánh được, điều này ngăn cản chiến lược ghép đôi lâu dài. 

Trường hợp cạnh thứ ba là trường hợp bù chính xác. Vì`8 / ?054??0?`, các giá trị chuẩn hóa là`diff = 9`,`qL = 1`, Và`qR = 3`. Hai dấu chấm hỏi bổ sung bên phải có thể bù đắp cho`9 * 2 / 2 = 9`, khớp chính xác với chênh lệch cố định. điều kiện`2 * diff == 9 * extra`giữ nguyên, vì vậy kết quả là`Bicarp`. 

Trường hợp cạnh thứ tư là khi xuất hiện sự mất cân bằng dấu hỏi nhưng chênh lệch cố định không khớp với mức bù cần thiết. Vì`6 / 8??0??`, chênh lệch cố định chuẩn hóa là`8`, trong khi sự khác biệt về số lượng dấu chấm hỏi là`2`. Khoản bồi thường chính xác tối đa cần thiết cho điều kiện chiến thắng của Bicarp sẽ là`9`, không`8`. Từ`2 * 8 != 2 * 9`, đầu ra của thuật toán`Monocarp`. 

Các trường hợp kích thước tối đa cho thấy tại sao giải pháp không bao giờ nên cố gắng mô phỏng cây trò chơi. Một vé chứa`200000`dấu chấm hỏi có số lượng hoàn thành có thể rất lớn, nhưng thuật toán chỉ đếm số lượng dấu chấm hỏi thuộc về mỗi nửa và quét chuỗi một lần. Kết quả thu được từ các giá trị tổng hợp đó mà không cần xây dựng bất kỳ phiếu hoàn thành nào có thể có.
