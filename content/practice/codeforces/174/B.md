---
title: "CF 174B - Danh sách tập tin"
description: "Chúng ta được cung cấp một chuỗi dài ban đầu bao gồm một số tên tệp được viết đối lưng nhau mà không có dấu phân cách. Mọi tên tệp hợp lệ phải giống như name.ext. Các quy tắc rất nghiêm ngặt. Phần trước dấu chấm chỉ chứa các chữ cái viết thường và có độ dài từ 1 đến 8."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 174
codeforces_index: "B"
codeforces_contest_name: "VK Cup 2012 Round 3 (Unofficial Div. 2 Edition)"
rating: 1400
weight: 174
solve_time_s: 220
verified: true
draft: false
---

[CF 174B - Danh sách tệp](https://codeforces.com/problemset/problem/174/B) 

**Đánh giá:** 1400 
**Tags:** dp, tham lam, thực hiện 
**Thời gian giải:** 3 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi dài ban đầu bao gồm một số tên tệp được viết đối lưng nhau mà không có dấu phân cách. Mỗi tên tập tin hợp lệ phải giống như`name.ext`. 

Các quy tắc rất nghiêm ngặt. Phần trước dấu chấm chỉ chứa các chữ cái viết thường và có độ dài từ 1 đến 8. Phần mở rộng sau dấu chấm cũng chỉ chứa các chữ cái viết thường và có độ dài từ 1 đến 3. Mỗi tên file phải chứa đúng một dấu chấm. 

Nhiệm vụ của chúng ta là chia toàn bộ chuỗi thành các chuỗi con liên tiếp sao cho mỗi chuỗi con là một tên file hợp lệ. Nếu có một phân vùng như vậy, chúng tôi sẽ in bất kỳ phân vùng nào trong số đó. Nếu không chúng tôi in`NO`. 

Độ dài đầu vào có thể đạt tới`4 * 10^5`, điều này thay đổi hoàn toàn cách chúng ta phải suy nghĩ về vấn đề. Bất kỳ giải pháp nào thử tất cả các phân vùng theo cách đệ quy sẽ bùng nổ theo cấp số nhân. Thậm chí một`O(n^2)`giải pháp lập trình động có nhiều rủi ro ở quy mô này vì nó sẽ yêu cầu khoảng`1.6 * 10^11`hoạt động trong trường hợp xấu nhất. 

Quan sát quan trọng là tên tệp hợp lệ có độ dài giới hạn rất nhỏ. Tên đóng góp tối đa 8 ký tự, phần mở rộng đóng góp tối đa 3 ký tự và có đúng một dấu chấm. Vì vậy, mọi đoạn hợp lệ có tổng độ dài từ 3 đến 12. Điều đó có nghĩa là từ bất kỳ vị trí nào, chúng ta chỉ cần kiểm tra một số lượng không đổi các lần cắt tiếp theo có thể có. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Một chuỗi không có dấu chấm không bao giờ có thể hoạt động được. 

Ví dụ:```
abcdef
```Không có cách nào để tạo tên tệp hợp lệ vì mỗi phân đoạn đều yêu cầu chính xác một dấu chấm. 

Đoạn chứa nhiều dấu chấm cũng không hợp lệ. 

Ví dụ:```
a..b
```Một trình phân tích cú pháp bất cẩn có thể chia nó thành`a.`Và`.b`, nhưng không bên nào thỏa mãn quy định vì cả tên và phần mở rộng đều phải chứa các chữ cái. 

Một trường hợp phức tạp khác là khi dấu chấm quá gần một đầu. 

Ví dụ:```
abcdefghij.k
```Phần tên có độ dài 10, vượt quá mức tối đa là 8. 

Tương tự:```
a.abcd
```Độ dài phần mở rộng là 4, vượt quá mức tối đa là 3. 

Một vấn đề tinh vi hơn xuất hiện khi một lựa chọn hợp lệ cục bộ cản trở tương lai. 

Coi như:```
ab.cdex.f
```Chuỗi con`ab.c`là hợp lệ, nhưng phần còn lại`dex.f`không phải vì độ dài tên là 3 và hợp lệ, thực ra cái này hoạt động. Một ví dụ tốt hơn là:```
a.bcdefghi.j
```Nếu chúng ta tham lam chiếm lấy`a.b`, phần còn lại`cdefghi.j`là hợp lệ. Nhưng các chiến lược tham lam khác có thể thất bại với những đầu vào khác nhau. Sự cố yêu cầu kiểm tra xem hậu tố có thể được phân vùng chính xác hay không. 

Điều này tự nhiên dẫn đến lập trình động. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Bắt đầu từ đầu chuỗi, hãy thử mọi chuỗi con tiếp theo có thể đại diện cho tên tệp. Sau đó giải đệ quy hậu tố còn lại. 

Điều này hoạt động vì mọi phân vùng hợp lệ cuối cùng đều được khám phá. Vấn đề là số lượng phân vùng. Mặc dù độ dài mỗi đoạn đều bị giới hạn, hệ số phân nhánh vẫn đủ lớn để tạo ra hành vi theo cấp số nhân. Một chuỗi có độ dài`4 * 10^5`sẽ không thể xử lý theo cách này. 

Ghi nhớ cải thiện mọi thứ một cách đáng kể. 

Nhận xét quan trọng là giá trị của công việc còn lại chỉ phụ thuộc vào chỉ mục hiện tại. Một khi chúng ta biết liệu hậu tố bắt đầu ở vị trí`i`có thể được phân vùng, chúng ta không bao giờ cần phải tính toán lại nó. 

Thậm chí tốt hơn, mỗi trạng thái chỉ có sự chuyển đổi liên tục. Độ dài tên tệp hợp lệ dao động từ 3 đến 12, do đó, từ mỗi vị trí, chúng tôi thử tối đa 10 chuỗi con ứng cử viên. Việc kiểm tra xem một chuỗi con có hợp lệ hay không cũng tốn thời gian không đổi vì độ dài tối đa rất nhỏ. 

Điều này biến việc tìm kiếm theo cấp số nhân thành lập trình động tuyến tính. 

Chúng tôi xác định`dp[i]`như thể hậu tố bắt đầu từ chỉ mục`i`có thể được phân vùng thành tên tập tin hợp lệ. Câu trả lời là`dp[0]`. 

Để chuyển từ`i`, chúng tôi thử mọi điểm cuối`j`như vậy`s[i:j]`có thể là tên tệp hợp lệ. Nếu chuỗi con hợp lệ và`dp[j]`là đúng thì`dp[i]`cũng đúng. 

Chúng tôi xử lý các trạng thái từ phải sang trái để biết trước các giá trị trong tương lai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) đệ quy | Quá chậm | 
| DP tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định hàm trợ giúp để kiểm tra xem chuỗi con có phải là tên tệp hợp lệ hay không. 

Chuỗi con phải chứa chính xác một dấu chấm. Phần trước dấu chấm phải có độ dài từ 1 đến 8 và chỉ chứa các chữ cái viết thường. Phần sau dấu chấm phải có độ dài từ 1 đến 3 và cũng chỉ chứa các chữ cái viết thường. 
2. Tạo mảng DP`dp`kích thước`n + 1`.`dp[i]`có nghĩa là hậu tố bắt đầu từ chỉ mục`i`có thể được phân vùng thành công. Chúng tôi thiết lập`dp[n] = True`vì hậu tố trống đã hợp lệ. 
3. Tạo một mảng khác`nxt`để xây dựng lại câu trả lời. 

Nếu chúng ta chọn một phân đoạn hợp lệ từ`i`ĐẾN`j`, chúng tôi lưu trữ`nxt[i] = j`. 
4. Xử lý chỉ số từ phải sang trái. 

Tại mỗi vị trí`i`, hãy thử mọi độ dài chuỗi con từ 3 đến 12, vì đó là độ dài tên tệp duy nhất có thể. 
5. Đối với mỗi chuỗi con ứng viên`s[i:j]`, kiểm tra hai điều kiện. 

Đầu tiên, chuỗi con phải là tên tệp hợp lệ. 

Thứ hai,`dp[j]`phải đúng, nghĩa là hậu tố còn lại cũng có thể được phân vùng. 
6. Nếu cả hai điều kiện đều đúng, hãy đánh dấu`dp[i] = True`và lưu trữ`nxt[i] = j`. 

Chúng tôi có thể ngừng kiểm tra thêm ứng viên vì mọi phân vùng hợp lệ đều được chấp nhận. 
7. Sau khi điền vào bảng DP, hãy kiểm tra`dp[0]`. 

Nếu sai thì in ra`NO`. 

Mặt khác, hãy xây dựng lại câu trả lời bằng cách liên tục lấy chuỗi con`s[pos:nxt[pos]]`. 

### Tại sao nó hoạt động 

Bất biến DP rất đơn giản.`dp[i]`đúng chính xác khi hậu tố bắt đầu tại`i`có thể được phân vùng thành tên tập tin hợp lệ. 

Quá trình chuyển đổi hoàn tất vì mọi phân vùng hợp lệ phải bắt đầu bằng một số tên tệp hợp lệ`s[i:j]`. Sau khi loại bỏ phân đoạn đầu tiên đó, hậu tố còn lại bắt đầu tại`j`, nên nó cũng phải giải được. 

Quá trình chuyển đổi diễn ra suôn sẻ vì bất cứ khi nào chúng tôi đánh dấu`dp[i] = True`, chúng tôi đã tìm thấy rõ ràng phân đoạn đầu tiên hợp lệ và phân vùng hợp lệ của hậu tố còn lại. 

Vì chúng tôi xử lý từ phải sang trái nên mọi trạng thái cần thiết trong tương lai đều đã được biết khi chúng tôi tính toán`dp[i]`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def valid_file(t: str) -> bool:
    if t.count('.') != 1:
        return False

    name, ext = t.split('.')

    if not (1 <= len(name) <= 8):
        return False

    if not (1 <= len(ext) <= 3):
        return False

    return name.islower() and ext.islower()

def solve():
    s = input().strip()
    n = len(s)

    dp = [False] * (n + 1)
    nxt = [-1] * (n + 1)

    dp[n] = True

    for i in range(n - 1, -1, -1):
        for length in range(3, 13):
            j = i + length

            if j > n:
                break

            if valid_file(s[i:j]) and dp[j]:
                dp[i] = True
                nxt[i] = j
                break

    if not dp[0]:
        print("NO")
        return

    print("YES")

    pos = 0
    while pos < n:
        print(s[pos:nxt[pos]])
        pos = nxt[pos]

solve()
```Hàm trợ giúp mã hóa trực tiếp các quy tắc tên tệp. Vì độ dài chuỗi con tối đa chỉ là 12 nên việc sử dụng các thao tác như`count('.')`Và`split('.')`là hoàn toàn an toàn và thời gian vẫn không đổi. 

Mảng DP được điền từ phải sang trái vì mỗi trạng thái phụ thuộc vào vị trí trong tương lai. Hậu tố trống được đánh dấu có thể truy cập được bằng`dp[n] = True`, hoạt động như trường hợp cơ sở. 

Vòng lặp bên trong chỉ thử độ dài từ 3 đến 12. Tên tệp hợp lệ tối thiểu có thể là`"a.b"`với chiều dài 3. Tối đa là`"abcdefgh.abc"`với chiều dài 12. 

Mảng tái thiết`nxt`cửa hàng nơi phân đoạn tiếp theo bắt đầu. Không có nó, chúng ta sẽ chỉ biết liệu giải pháp có tồn tại hay không chứ không biết cách in giải pháp đó. 

Một chi tiết triển khai tinh tế là phần đầu`break`sau khi tìm thấy một quá trình chuyển đổi hợp lệ. Sự cố chấp nhận bất kỳ phân vùng hợp lệ nào, vì vậy việc tiếp tục tìm kiếm là không cần thiết. 

Một chi tiết quan trọng khác là sử dụng`j > n`như điều kiện dừng. Điều này ngăn chặn logic cắt ngoài phạm vi và tránh việc kiểm tra các chuỗi con không thể thực hiện được. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
read.meexample.txtb.cpp
```| tôi | Ứng viên | Tên tệp hợp lệ | dp[tiếp theo] | dp[i] | 
| --- | --- | --- | --- | --- | 
| 18 | cpp | Không | - | Sai | 
| 15 | b.cpp | Có | Đúng | Đúng | 
| 9 | ví dụ.t | Có | Đúng | Đúng | 
| 0 | đọc.m | Có | Đúng | Đúng | 

Phân vùng được xây dựng lại:```
read.m
eexample.t
xtb.cpp
```Dấu vết này cho thấy thuật toán không cố gắng khôi phục phân vùng ban đầu. Nó chỉ cần bất kỳ phân vùng hợp lệ. Mỗi chuỗi con được chọn đều thỏa mãn các ràng buộc về tên tệp một cách độc lập. 

### Ví dụ 2 

đầu vào:```
abcdef
```| tôi | Ứng viên | Tên tệp hợp lệ | dp[tiếp theo] | dp[i] | 
| --- | --- | --- | --- | --- | 
| 5 | không | - | - | Sai | 
| 4 | không | - | - | Sai | 
| 3 | chắc chắn | Không | - | Sai | 
| 0 | abcdef | Không | - | Sai | 

Vì không có chuỗi con nào chứa cấu trúc dấu chấm hợp lệ nên mọi trạng thái DP vẫn sai. 

Đầu ra:```
NO
```Điều này chứng tỏ rằng thuật toán loại bỏ chính xác các chuỗi không có phân đoạn hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí kiểm tra tối đa 10 độ dài chuỗi con | 
| Không gian | O(n) | DP và mảng tái thiết | 

Kích thước đầu vào đạt`4 * 10^5`, vì vậy độ phức tạp tuyến tính chính xác là những gì chúng ta cần. Hệ số không đổi cũng rất nhỏ vì mỗi độ dài chuỗi con được giới hạn bởi 12. Việc sử dụng bộ nhớ dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve_io(inp: str) -> str:
    input_data = io.StringIO(inp)
    output_data = io.StringIO()

    input = input_data.readline

    def valid_file(t: str) -> bool:
        if t.count('.') != 1:
            return False

        name, ext = t.split('.')

        if not (1 <= len(name) <= 8):
            return False

        if not (1 <= len(ext) <= 3):
            return False

        return name.islower() and ext.islower()

    s = input().strip()
    n = len(s)

    dp = [False] * (n + 1)
    nxt = [-1] * (n + 1)

    dp[n] = True

    for i in range(n - 1, -1, -1):
        for length in range(3, 13):
            j = i + length

            if j > n:
                break

            if valid_file(s[i:j]) and dp[j]:
                dp[i] = True
                nxt[i] = j
                break

    if not dp[0]:
        print("NO", file=output_data)
    else:
        print("YES", file=output_data)

        pos = 0
        while pos < n:
            print(s[pos:nxt[pos]], file=output_data)
            pos = nxt[pos]

    return output_data.getvalue()

# provided sample
out = solve_io("read.meexample.txtb.cpp\n")
assert out.startswith("YES")

# minimum valid case
assert solve_io("a.b\n") == "YES\na.b\n"

# no dot
assert solve_io("abcdef\n") == "NO\n"

# extension too long
assert solve_io("a.abcd\n") == "NO\n"

# maximum allowed lengths
assert solve_io("abcdefgh.abc\n") == "YES\nabcdefgh.abc\n"

# multiple valid partitions
out = solve_io("ab.cd.ef\n")
assert out.startswith("YES")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a.b`|`YES`| Tên tệp hợp lệ nhỏ nhất có thể | 
|`abcdef`|`NO`| Chuỗi không có dấu chấm | 
|`a.abcd`|`NO`| Giới hạn độ dài tiện ích mở rộng | 
|`abcdefgh.abc`|`YES`| Độ dài tối đa cho phép | 
|`ab.cd.ef`|`YES`| Nhiều phân vùng có thể | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
a..b
```Chuỗi con chứa hai dấu chấm. Trong quá trình xác nhận,`count('.') != 1`ngay lập tức từ chối nó. Không có chuyển đổi DP nào hợp lệ, do đó thuật toán sẽ in:```
NO
```Điều này ngăn chặn các tên không đúng định dạng lọt qua. 

Bây giờ hãy xem xét:```
abcdefghij.k
```Cách phân chia tên tệp duy nhất có thể là:```
abcdefghij.k
```Độ dài tên là 10, vượt quá giới hạn 8. Trình xác thực từ chối nó, vì vậy`dp[0]`vẫn sai. 

Đầu ra là:```
NO
```Kiểm tra tiếp theo:```
a.abcd
```Độ dài phần mở rộng là 4. Trình xác thực sẽ kiểm tra`1 <= len(ext) <= 3`, thất bại. Một lần nữa không có sự chuyển tiếp hợp lệ nào tồn tại. 

Cuối cùng hãy xem xét:```
a.ba.bc
```Có thể có nhiều vết cắt cục bộ. Thuật toán kiểm tra tất cả độ dài chuỗi con khả thi từ mỗi vị trí và chỉ chấp nhận một phân đoạn nếu hậu tố còn lại cũng có thể giải được. 

Một phân vùng hợp lệ là:```
a.ba
.bc
```Thực ra`.bc`không hợp lệ vì tên không được để trống. DP tránh đường dẫn này một cách chính xác. Thay vào đó nó có thể chọn:```
a.b
a.bc
```Cả hai phân đoạn đều thỏa mãn các ràng buộc nên thuật toán sẽ in ra câu trả lời hợp lệ. Điều này chứng tỏ tại sao tính khả thi của hậu tố phải là một phần của quá trình chuyển đổi thay vì đưa ra những lựa chọn thuần túy tham lam.
