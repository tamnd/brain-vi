---
title: "CF 143B - Trợ Giúp Vương Quốc Xa Xót 2"
description: "Chúng ta được cung cấp một số dưới dạng chuỗi và phải in số đó ở định dạng tiền kiểu ngân hàng. Các quy tắc định dạng kết hợp một số phép biến đổi độc lập. Phần nguyên phải chứa ba chữ số có dấu phẩy, tính từ bên phải."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 143
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 102 (Div. 2)"
rating: 1200
weight: 143
solve_time_s: 139
verified: true
draft: false
---

[CF 143B - Trợ giúp Vương quốc Xa xôi 2](https://codeforces.com/problemset/problem/143/B) 

**Đánh giá:** 1200 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số dưới dạng chuỗi và phải in số đó ở định dạng tiền kiểu ngân hàng. 

Các quy tắc định dạng kết hợp một số phép biến đổi độc lập. Phần nguyên phải chứa ba chữ số có dấu phẩy, tính từ bên phải. Phần phân số phải chứa chính xác hai chữ số. Nếu số ban đầu có ít hơn hai chữ số phân số, chúng ta sẽ thêm số 0 vào. Nếu nó có nhiều hơn hai, chúng ta chỉ cần cắt bỏ các chữ số thừa mà không làm tròn. 

Số âm được hiển thị bằng dấu ngoặc đơn thay vì dấu trừ. Ký hiệu đô la phải luôn xuất hiện trước giá trị số và đối với giá trị âm, ký hiệu này phải được đặt bên trong dấu ngoặc đơn. 

Độ dài đầu vào tối đa là 100 ký tự, vì vậy đây hoàn toàn là vấn đề thao tác chuỗi. Không cần số học và việc sử dụng số dấu phẩy động thực sự nguy hiểm vì chúng ta phải cắt bớt các chữ số thay vì làm tròn. Vì đầu vào rất nhỏ nên thậm chí nhiều lần truyền qua chuỗi cũng hoàn toàn ổn. Bất kỳ giải pháp thời gian tuyến tính nào cũng dễ dàng đủ nhanh. 

Một số trường hợp nguy hiểm có thể âm thầm phá vỡ việc triển khai bất cẩn. 

Một số có thể không chứa dấu thập phân nào cả. 

đầu vào:```
2012
```Đầu ra đúng:```
$2,012.00
```Nếu chúng ta mù quáng chia tay`"."`không kiểm tra xem nó có tồn tại hay không, chúng ta có thể gặp sự cố hoặc tạo ra phần phân số trống không chính xác. 

Phần phân số phải được cắt ngắn, không được làm tròn. 

đầu vào:```
1.999
```Đầu ra đúng:```
$1.99
```Sử dụng định dạng dấu phẩy động như`"{:.2f}"`sẽ sản xuất`$2.00`, điều đó là sai. 

Không thể có các giá trị giống số 0 âm trong đầu vào. Câu lệnh đảm bảo rằng các giá trị bằng 0 không bao giờ có dấu trừ. Điều đó quan trọng vì dấu được xác định từ chuỗi gốc chứ không phải từ giá trị được định dạng sau khi cắt bớt. 

đầu vào:```
-0.001
```Đầu vào này bị cấm bởi sự cố, vì vậy chúng tôi không cần xử lý đặc biệt cho nó. 

Một trường hợp tinh vi khác là các số có phần nguyên rất ngắn. 

đầu vào:```
12.3
```Đầu ra đúng:```
$12.30
```Vòng lặp tạo dấu phẩy bất cẩn có thể vô tình thêm dấu phẩy vào các số nhỏ. 

Số lượng lớn cũng yêu cầu nhóm cẩn thận. 

đầu vào:```
123456789
```Đầu ra đúng:```
$123,456,789.00
```Việc phân nhóm phải bắt đầu từ phía bên phải, không phải từ bên trái. 

## Phương pháp tiếp cận 

Một giải pháp kiểu brute-force sẽ liên tục loại bỏ ba chữ số cuối của phần nguyên và xây dựng câu trả lời ngược lại. Điều này đã hoạt động hiệu quả vì kích thước đầu vào rất nhỏ. Chúng tôi cũng có thể mô phỏng định dạng thập phân theo cách thủ công theo từng ký tự. 

Mối nguy hiểm chính không phải là hiệu suất mà là tính chính xác. Sau khi chuyển đổi dấu phẩy động được đưa vào giải pháp, hành vi cắt bớt sẽ trở nên không đáng tin cậy. Ví dụ, chuyển đổi`"999999999999999999.999"`vào một cái phao sẽ mất độ chính xác ngay lập tức. Ngay cả những giá trị nhỏ như`"1.999"`sẽ làm tròn thay vì cắt bớt nếu sử dụng các chức năng định dạng tiêu chuẩn. 

Điều quan trọng cần lưu ý là mọi quy tắc trong bài toán đều hoạt động trực tiếp trên biểu diễn văn bản. Chúng tôi không bao giờ cần giá trị số. Phần nguyên chỉ được sắp xếp lại bằng dấu phẩy, phần phân số được đệm hoặc cắt bớt dưới dạng văn bản và âm được phát hiện từ ký tự đầu tiên. 

Điều đó có nghĩa là giải pháp sạch nhất là xử lý chuỗi thuần túy: 

Đầu tiên loại bỏ dấu trừ tùy chọn. 

Sau đó chia chuỗi còn lại thành phần nguyên và phần phân số. 

Định dạng phần nguyên với các nhóm dấu phẩy từ bên phải. 

Chuẩn hóa phần phân số thành chính xác hai ký tự. 

Cuối cùng đính kèm ký hiệu đô la và dấu ngoặc đơn tùy chọn. 

Toàn bộ thuật toán là tuyến tính theo độ dài đầu vào và tránh mọi vấn đề về độ chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với chuyển đổi số | O(n) | O(n) | Sai vì làm tròn và chính xác | 
| Giải pháp xử lý chuỗi tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào. 
2. Kiểm tra xem số có âm hay không bằng cách kiểm tra xem ký tự đầu tiên có phải là`'-'`. 

Chúng ta phải ghi nhớ điều này một cách riêng biệt vì dấu trừ biến mất ở định dạng cuối cùng và được thay thế bằng dấu ngoặc đơn. 
3. Nếu số âm, hãy xóa phần đầu`'-'`. 
4. Chia chuỗi còn lại thành phần nguyên và phần phân số. 

Nếu chuỗi chứa dấu thập phân, hãy chia nhỏ`"."`. 

Ngược lại, toàn bộ chuỗi là phần nguyên và phần phân số bắt đầu trống. 
5. Chuẩn hóa phần phân số thành đúng hai chữ số. 

Nếu nó có ít hơn hai ký tự, hãy thêm số 0 cho đến khi độ dài của nó trở thành hai. 

Nếu nó có nhiều hơn hai ký tự, chỉ giữ lại hai ký tự đầu tiên. 
6. Định dạng phần nguyên bằng dấu phẩy. 

Duyệt các chữ số từ phải sang trái theo từng đoạn có ba ký tự. Đảo ngược các đoạn đã thu thập và nối chúng bằng dấu phẩy. 

Việc nhóm từ bên phải là cần thiết vì các chữ số có ý nghĩa nhỏ nhất xác định ranh giới nhóm. 
7. Xây dựng chuỗi tiền tệ có định dạng như sau:```
$<formatted_integer>.<fraction>
```8. Nếu số ban đầu là số âm, hãy đặt toàn bộ kết quả vào trong ngoặc đơn. 
9. In chuỗi cuối cùng. 

### Tại sao nó hoạt động 

Thuật toán trực tiếp thực hiện từng quy tắc định dạng một cách độc lập. 

Phép biến đổi phần nguyên giữ nguyên tất cả các chữ số và chỉ chèn dấu phẩy vào mỗi ba vị trí tính từ bên phải, khớp chính xác với thông số kỹ thuật. 

Việc chuẩn hóa phân số luôn tạo ra chính xác hai chữ số vì nó cắt bớt các ký tự thừa hoặc nối thêm các số 0 bị thiếu. 

Việc xử lý ký hiệu là chính xác vì ký hiệu gốc được lưu trữ trước bất kỳ sửa đổi nào và sau đó được chuyển đổi thành dấu ngoặc đơn đúng một lần. 

Vì mọi chuyển đổi đều mang tính xác định và cục bộ đối với một phần của chuỗi nên đầu ra cuối cùng khớp với định dạng tài chính bắt buộc cho mọi đầu vào hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()

    negative = s[0] == '-'

    if negative:
        s = s[1:]

    if '.' in s:
        integer_part, fractional_part = s.split('.')
    else:
        integer_part = s
        fractional_part = ""

    fractional_part = (fractional_part + "00")[:2]

    groups = []

    i = len(integer_part)
    while i > 0:
        start = max(0, i - 3)
        groups.append(integer_part[start:i])
        i -= 3

    formatted_integer = ",".join(reversed(groups))

    result = f"${formatted_integer}.{fractional_part}"

    if negative:
        result = f"({result})"

    print(result)

solve()
```Giải pháp bắt đầu bằng cách lưu giữ thông tin dấu hiệu trước khi sửa đổi chuỗi. Điều này tránh nhầm lẫn sau này khi dấu trừ biến mất khỏi định dạng đầu ra. 

Việc phân chia giữa phần nguyên và phần phân số được thực hiện hoàn toàn bằng chuỗi. Không có chuyển đổi số nào xảy ra ở bất kỳ đâu trong chương trình, điều này tránh được cả hành vi tràn và làm tròn không mong muốn. 

dòng```
fractional_part = (fractional_part + "00")[:2]
```là một cách nhỏ gọn để chuẩn hóa phần phân số. Đang bổ sung`"00"`đảm bảo chuỗi có ít nhất hai ký tự và việc cắt giữ chính xác hai ký tự. Điều này xử lý chính xác cả ba trường hợp:```
""     -> "00"
"3"    -> "30"
"987"  -> "98"
```Vòng lặp định dạng số nguyên xử lý các chữ số từ phải sang trái vì các nhóm dấu phẩy được xác định tương ứng với các chữ số có ý nghĩa nhỏ nhất. Mỗi lần lặp sẽ trích xuất một nhóm có tối đa ba chữ số. 

sử dụng`reversed(groups)`khôi phục các nhóm theo thứ tự từ trái sang phải bình thường trước khi nối chúng bằng dấu phẩy. 

Dấu ngoặc đơn chỉ được thêm vào cuối. Điều này đảm bảo ký hiệu đô la vẫn ở bên trong chúng, điều này được yêu cầu trong câu lệnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2012
```| Bước | Giá trị | 
| --- | --- | 
| Chuỗi gốc |`2012`| 
| Tiêu cực |`False`| 
| Phần nguyên |`2012`| 
| Phần phân số trước khi chuẩn hóa | `` | 
| Phần phân số sau khi chuẩn hóa |`00`| 
| Nhóm được trích xuất |`["012", "2"]`trong quá trình di chuyển ngược lại | 
| Số nguyên được định dạng |`2,012`| 
| Kết quả cuối cùng |`$2,012.00`| 

Ví dụ này cho thấy cách nhóm hoạt động đối với một số có bốn chữ số. Nhóm đầu tiên từ bên trái có thể chứa ít hơn ba chữ số. 

### Ví dụ 2 

đầu vào:```
-12345678.9
```| Bước | Giá trị | 
| --- | --- | 
| Chuỗi gốc |`-12345678.9`| 
| Tiêu cực |`True`| 
| Chuỗi sau khi xóa dấu |`12345678.9`| 
| Phần nguyên |`12345678`| 
| Phần phân số trước khi chuẩn hóa |`9`| 
| Phần phân số sau khi chuẩn hóa |`90`| 
| Nhóm được trích xuất |`["678", "345", "12"]`trong quá trình di chuyển ngược lại | 
| Số nguyên được định dạng |`12,345,678`| 
| Chuỗi tiền tệ trước khi định dạng ký hiệu |`$12,345,678.90`| 
| Kết quả cuối cùng |`($12,345,678.90)`| 

Dấu vết này thể hiện đồng thời hai hành vi quan trọng: đệm phân số và định dạng phủ định bằng dấu ngoặc đơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một số lần không đổi | 
| Không gian | O(n) | Kết quả được định dạng và nhóm dấu phẩy yêu cầu thêm khoảng trống tuyến tính | 

Với tối đa 100 ký tự trong đầu vào, thuật toán sẽ chạy ngay lập tức. Việc sử dụng bộ nhớ cũng không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        s = input().strip()

        negative = s[0] == '-'

        if negative:
            s = s[1:]

        if '.' in s:
            integer_part, fractional_part = s.split('.')
        else:
            integer_part = s
            fractional_part = ""

        fractional_part = (fractional_part + "00")[:2]

        groups = []

        i = len(integer_part)
        while i > 0:
            start = max(0, i - 3)
            groups.append(integer_part[start:i])
            i -= 3

        formatted_integer = ",".join(reversed(groups))

        result = f"${formatted_integer}.{fractional_part}"

        if negative:
            result = f"({result})"

        print(result)

    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = old_stdout

    return out.getvalue()

# provided sample
assert run("2012\n") == "$2,012.00\n", "sample 1"

# custom cases
assert run("0\n") == "$0.00\n", "single zero"

assert run("12.3\n") == "$12.30\n", "fractional padding"

assert run("1.999\n") == "$1.99\n", "truncate instead of round"

assert run("-123456789.01\n") == "($123,456,789.01)\n", "negative with grouping"

assert run("1000000\n") == "$1,000,000.00\n", "multiple comma groups"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`$0.00`| Số nguyên hợp lệ nhỏ nhất | 
|`12.3`|`$12.30`| Phần đệm phân số | 
|`1.999`|`$1.99`| Cắt bớt thay vì làm tròn | 
|`-123456789.01`|`($123,456,789.01)`| Định dạng phủ định bằng dấu phẩy | 
|`1000000`|`$1,000,000.00`| Chèn nhiều dấu phẩy | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1.999
```Thuật toán chia chuỗi thành phần nguyên`"1"`và phần phân số`"999"`. Sau khi bình thường hóa:```
("999" + "00")[:2]
```sản xuất`"99"`. Không làm tròn số vì thuật toán không bao giờ diễn giải giá trị bằng số. Câu trả lời cuối cùng trở thành:```
$1.99
```Điều này trực tiếp phù hợp với yêu cầu tuyên bố. 

Bây giờ hãy xem xét:```
12
```Không có dấu thập phân nên phần phân số bắt đầu bằng một chuỗi trống. Sau khi bình thường hóa:```
("" + "00")[:2]
```trở thành`"00"`. Phần nguyên có ít hơn bốn chữ số nên không chèn dấu phẩy. Đầu ra trở thành:```
$12.00
```Điều này xác nhận thuật toán xử lý các phần phân số bị thiếu một cách chính xác. 

Hãy xem xét một số lượng lớn được nhóm:```
1234567890
```Vòng lặp nhóm trích xuất:```
890
567
234
1
```theo thứ tự ngược lại. Đảo ngược danh sách tạo ra:```
1,234,567,890
```nhóm các chữ số từ phía bên phải một cách chính xác. 

Cuối cùng, kiểm tra một giá trị âm:```
-5.2
```Dấu hiệu được ghi lại trước, sau đó được xóa trước khi định dạng. Phần phân số được chuẩn hóa trở thành`"20"`, sản xuất:```
$5.20
```Cuối cùng, dấu ngoặc đơn được thêm vào:```
($5.20)
```Điều này xác nhận logic xử lý dấu hiệu vẫn chính xác một cách độc lập với giá trị số sau khi định dạng.
