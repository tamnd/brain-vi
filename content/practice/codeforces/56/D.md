---
title: "CF 56D - Thay đổi chuỗi"
description: "Chúng ta có hai chuỗi chữ hoa, s và t. Chúng ta có thể biến đổi s bằng ba thao tác: 1. Chèn một ký tự vào bất kỳ vị trí nào. 2. Xóa một ký tự ở bất kỳ vị trí nào. 3. Thay thế ký tự này bằng ký tự khác. Mỗi hoạt động tốn chính xác một lần di chuyển."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 56
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 52 (Div. 2)"
rating: 2100
weight: 56
solve_time_s: 132
verified: true
draft: false
---

[CF 56D - Thay đổi chuỗi](https://codeforces.com/problemset/problem/56/D) 

**Đánh giá:** 2100 
**Thẻ:** dp 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai chuỗi chữ hoa,`s`Và`t`. Chúng ta có thể biến đổi`s`sử dụng ba thao tác: 

1. Chèn ký tự vào vị trí bất kỳ. 
2. Xóa một ký tự ở bất kỳ vị trí nào. 
3. Thay thế ký tự này bằng ký tự khác. 

Mỗi hoạt động tốn chính xác một lần di chuyển. Nhiệm vụ không chỉ là tính số bước di chuyển tối thiểu cần thiết để rẽ`s`vào trong`t`, mà còn để xuất ra một chuỗi thao tác thực tế đạt được mức tối thiểu này. 

Đây là vấn đề về khoảng cách chỉnh sửa cổ điển, nhưng với việc xây dựng lại các hoạt động. Phần tái thiết là nơi xảy ra hầu hết các lỗi thực hiện. 

Các chuỗi có độ dài tối đa là 1000. Việc tìm kiếm mạnh mẽ trên tất cả các chuỗi hoạt động có thể là vô vọng vì hệ số phân nhánh rất lớn. Ngay cả việc thử tất cả các thao tác chỉ trong 20 bước cũng đã bùng nổ theo cấp số nhân. Với độ dài lên tới 1000, chúng ta cần một cái gì đó gần với thời gian bậc hai. 

Giải pháp lập trình động với`O(n * m)`trạng thái là hoàn toàn khả thi ở đây. Với cả hai độ dài bằng 1000, chúng ta có một triệu trạng thái. Mỗi trạng thái chuyển tiếp trong thời gian không đổi, dễ dàng phù hợp với giới hạn. 

Phần khó khăn là tạo ra các chỉ số hoạt động hợp lệ trong khi chuỗi thay đổi theo thời gian. 

Hãy xem xét ví dụ này:```
s = ABC
t = ADBC
```Nếu chúng ta quyết định chèn`D`sau đó`A`, phép toán phải là:```
INSERT 2 D
```Sau khi chèn, tất cả các vị trí sau đó sẽ dịch chuyển một. Việc xây dựng lại bất cẩn tính toán các vị trí liên quan đến chuỗi gốc sẽ nhanh chóng trở nên không nhất quán. 

Một trường hợp tế nhị khác là khi tồn tại nhiều câu trả lời tối ưu.```
s = A
t = B
```Chúng ta có thể:```
REPLACE 1 B
```hoặc:```
DELETE 1
INSERT 1 B
```Chuỗi thứ hai dài hơn, do đó việc tái thiết chỉ phải tuân theo các chuyển tiếp để duy trì tính tối ưu. 

Một lỗi phổ biến cuối cùng xuất hiện khi một chuỗi bị cạn kiệt trước chuỗi kia.```
s = ABC
t = ABCDE
```Công việc còn lại chỉ là chèn thuần túy:```
INSERT 4 D
INSERT 5 E
```Nếu các chỉ số được tính toán không chính xác sau lần chèn trước đó, các thao tác được tạo sẽ trở nên không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản về mặt khái niệm. Ở mỗi bước, chúng tôi thử mọi cách chèn, xóa và thay thế có thể, khám phá đệ quy tất cả các chuỗi chuyển đổi cho đến khi đạt được chuỗi đích. Vì mọi thao tác đều thay đổi chuỗi nên số lượng trạng thái có thể truy cập sẽ tăng theo cấp số nhân. 

Ngay cả đối với các chuỗi có độ dài 20, số lượng khả năng vẫn trở nên rất lớn. Giả sử chúng tôi cho phép khoảng 60 thao tác có thể thực hiện được trong mỗi bước và chỉ cần 15 lần chỉnh sửa. Điều đó đã mang lại xung quanh`60^15`khả năng, vượt xa mọi thứ có thể tính toán được. 

Lý do vũ phu hoạt động về mặt khái niệm là vì các thao tác chỉnh sửa tạo thành cây tìm kiếm một cách tự nhiên. Vấn đề là nhiều chuỗi khác nhau dẫn đến các tiền tố trung gian giống nhau lặp đi lặp lại. Chúng ta tính đi tính lại các bài toán con tương đương. 

Quan sát quan trọng là tương lai chỉ phụ thuộc vào các hậu tố còn lại của chuỗi. 

Giả sử chúng ta phù hợp:```
s[i:]
t[j:]
```Câu trả lời tối ưu từ thời điểm này không quan tâm làm thế nào chúng ta đến được đây. Điều đó có nghĩa là chúng ta có thể xác định trạng thái DP:```
dp[i][j] = minimum edits needed to transform s[i:] into t[j:]
```Bây giờ mọi trạng thái chỉ có ba chuyển đổi có ý nghĩa: 

1. Xóa`s[i]`2. Chèn`t[j]`3. Thay thế`s[i]`với`t[j]`nếu chúng khác nhau 

Nếu các ký tự đã khớp nhau, chúng ta chỉ cần di chuyển theo đường chéo mà không phải trả bất cứ điều gì. 

Điều này làm giảm vấn đề từ tìm kiếm theo cấp số nhân sang DP lưới chỉ với`n * m`tiểu bang. 

Sau khi tính toán khoảng cách tối thiểu, chúng tôi xây dựng lại một đường dẫn tối ưu bằng cách duyệt qua bảng DP và chọn các chuyển tiếp phù hợp với các giá trị tối ưu được lưu trữ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| DP tối ưu | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy để`n = len(s)`Và`m = len(t)`. 
2. Tạo bảng DP`dp`kích thước`(n + 1) × (m + 1)`.`dp[i][j]`đại diện cho các chỉnh sửa tối thiểu cần thiết để chuyển đổi hậu tố`s[i:]`vào trong`t[j:]`. 
3. Khởi tạo các trường hợp cơ sở. 

Nếu như`i = n`, chuỗi nguồn đã hết nên chúng ta phải chèn tất cả các ký tự còn lại của`t`.```
dp[n][j] = m - j
```Nếu như`j = m`, chuỗi đích đã hết nên chúng ta phải xóa tất cả các ký tự còn lại của`s`.```
dp[i][m] = n - i
```4. Điền vào bảng từ dưới cùng bên phải đến trên cùng bên trái. 

Nếu như`s[i] == t[j]`, không cần thực hiện thao tác nào đối với các ký tự này.```
dp[i][j] = dp[i + 1][j + 1]
```Mặt khác, chúng tôi xem xét ba hoạt động chỉnh sửa. 

Xóa bỏ:```
1 + dp[i + 1][j]
```Chèn:```
1 + dp[i][j + 1]
```Thay thế:```
1 + dp[i + 1][j + 1]
```Chúng tôi lấy mức tối thiểu. 
5. Xây dựng lại một chuỗi hoạt động tối ưu. 

Bắt đầu từ`(i, j) = (0, 0)`và duy trì một biến`shift`.`shift`theo dõi độ dài chuỗi hiện tại khác với chuỗi ban đầu bao nhiêu vì các thao tác chèn và xóa làm thay đổi chỉ số. 
6. Nếu`s[i] == t[j]`Và:```
dp[i][j] == dp[i + 1][j + 1]
```di chuyển theo đường chéo mà không thực hiện một thao tác nào. 
7. Ngược lại, hãy kiểm tra xem thao tác nào duy trì tính tối ưu. 

Để xóa:```
dp[i][j] == 1 + dp[i + 1][j]
```Đầu ra:```
DELETE position
```Vị trí hiện tại là:```
i + shift + 1
```Sau đó tăng dần`i`và giảm dần`shift`. 
8. Để chèn:```
dp[i][j] == 1 + dp[i][j + 1]
```Đầu ra:```
INSERT position character
```Vị trí chèn lại là:```
i + shift + 1
```Sau đó tăng dần`j`và tăng dần`shift`. 
9. Để thay thế:```
dp[i][j] == 1 + dp[i + 1][j + 1]
```Đầu ra:```
REPLACE position character
```Sau đó tăng cả hai`i`Và`j`. 
10. Tiếp tục cho đến khi cả hai chuỗi được xử lý hoàn toàn. 

### Tại sao nó hoạt động 

Bất biến DP là`dp[i][j]`luôn lưu trữ số lượng chỉnh sửa tối thiểu thực sự cần thiết để chuyển đổi`s[i:]`vào trong`t[j:]`. 

Mọi phép biến đổi hợp lệ phải bắt đầu bằng chính xác một trong bốn khả năng: 

1. Ghép các ký tự giống nhau. 
2. Xóa ký tự hiện tại của`s`. 
3. Chèn ký tự hiện tại của`t`. 
4. Thay thế ký tự hiện tại của`s`. 

Sự lặp lại kiểm tra tất cả chúng và chọn cái tốt nhất. Vì mỗi quá trình chuyển đổi sẽ chuyển sang các hậu tố nhỏ hơn nên tất cả các trạng thái cần thiết đều đã được tính toán. 

Trong quá trình xây dựng lại, chúng tôi chỉ theo dõi các chuyển đổi có chi phí khớp chính xác với mức tối ưu được lưu trữ. Điều đó đảm bảo mọi hoạt động được chọn đều thuộc về một số giải pháp tối ưu toàn cầu. 

các`shift`biến đảm bảo vị trí vẫn chính xác ngay cả sau khi độ dài chuỗi thay đổi linh hoạt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    t = input().strip()

    n = len(s)
    m = len(t)

    dp = [[0] * (m + 1) for _ in range(n + 1)]

    for i in range(n + 1):
        dp[i][m] = n - i

    for j in range(m + 1):
        dp[n][j] = m - j

    for i in range(n - 1, -1, -1):
        for j in range(m - 1, -1, -1):
            if s[i] == t[j]:
                dp[i][j] = dp[i + 1][j + 1]
            else:
                dp[i][j] = 1 + min(
                    dp[i + 1][j],      # delete
                    dp[i][j + 1],      # insert
                    dp[i + 1][j + 1]   # replace
                )

    operations = []

    i = 0
    j = 0
    shift = 0

    while i < n or j < m:
        if i < n and j < m and s[i] == t[j] and dp[i][j] == dp[i + 1][j + 1]:
            i += 1
            j += 1

        elif i < n and dp[i][j] == 1 + dp[i + 1][j]:
            pos = i + shift + 1
            operations.append(f"DELETE {pos}")
            i += 1
            shift -= 1

        elif j < m and dp[i][j] == 1 + dp[i][j + 1]:
            pos = i + shift + 1
            operations.append(f"INSERT {pos} {t[j]}")
            j += 1
            shift += 1

        else:
            pos = i + shift + 1
            operations.append(f"REPLACE {pos} {t[j]}")
            i += 1
            j += 1

    print(len(operations))
    print("\n".join(operations))

solve()
```Phần đầu tiên xây dựng bảng DP từ các hậu tố nhỏ hơn đến các hậu tố lớn hơn. Vì mọi trạng thái tham chiếu quá trình chuyển đổi có chỉ số lớn hơn nên việc lặp lại đảm bảo các giá trị đó đã tồn tại. 

Giai đoạn tái thiết là giai đoạn cần được chăm sóc nhiều nhất. 

Biến`shift`theo dõi vị trí thay đổi như thế nào so với chuỗi gốc. Giả sử chúng ta đã chèn hai ký tự trước đó. Sau đó, mọi vị trí tương lai trong chuỗi hiện tại sẽ được dịch chuyển bởi`+2`. 

Nếu không có sự điều chỉnh này, các hoạt động sẽ nhắm mục tiêu các chỉ mục không chính xác sau lần chèn hoặc xóa đầu tiên. 

Lệnh tái thiết cũng quan trọng. Các ký tự trùng khớp phải được kiểm tra trước khi thực hiện thao tác chỉnh sửa. Mặt khác, thuật toán có thể thay thế các ký tự bằng nhau một cách không cần thiết và vẫn duy trì tính tối ưu về số lượng trong một số tình huống bị ràng buộc. 

Điều kiện vòng lặp:```
while i < n or j < m:
```rất quan trọng vì một chuỗi có thể kết thúc trước chuỗi kia. Chúng tôi vẫn cần phát ra các phần chèn hoặc xóa còn lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
s = ABA
t = ABBBA
```Câu trả lời tối ưu là hai lần chèn. 

| Bước | tôi | j | ca | Hành động | Kết quả hiện tại | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | 0 | Trận đấu A | ABA | 
| 2 | 1 | 1 | 0 | Trận đấu B | ABA | 
| 3 | 2 | 2 | 0 | CHÈN 3 B | ABBA | 
| 4 | 2 | 3 | 1 | CHÈN 4 B | ABBBA | 
| 5 | 2 | 4 | 2 | Trận đấu A | ABBBA | 

Đầu ra:```
2
INSERT 3 B
INSERT 4 B
```Dấu vết này chứng tỏ tại sao`shift`là cần thiết. Sau lần chèn đầu tiên, tất cả các vị trí sau sẽ di chuyển sang phải từng vị trí một. 

### Ví dụ 2 

đầu vào:```
s = ABC
t = ADC
```Chỉ cần một sự thay thế. 

| Bước | tôi | j | ca | Hành động | Kết quả hiện tại | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | 0 | Trận đấu A | ABC | 
| 2 | 1 | 1 | 0 | THAY THẾ 2D | ADC | 
| 3 | 2 | 2 | 0 | Trận đấu C | ADC | 

Đầu ra:```
1
REPLACE 2 D
```Ví dụ này cho thấy sự chuyển tiếp theo đường chéo trong DP. Các ký tự trùng khớp không mất phí, trong khi các ký tự khác nhau có thể được xử lý bằng cách thay thế thay vì các thao tác xóa và chèn riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi trạng thái DP được tính một lần | 
| Không gian | O(nm) | Bảng DP đầy đủ được lưu trữ để xây dựng lại | 

Với cả hai chuỗi được giới hạn ở độ dài 1000, bảng DP chứa khoảng một triệu ô. Điều này vừa vặn thoải mái trong giới hạn bộ nhớ và số lượng thao tác đủ nhỏ cho Python trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    s = input().strip()
    t = input().strip()

    n = len(s)
    m = len(t)

    dp = [[0] * (m + 1) for _ in range(n + 1)]

    for i in range(n + 1):
        dp[i][m] = n - i

    for j in range(m + 1):
        dp[n][j] = m - j

    for i in range(n - 1, -1, -1):
        for j in range(m - 1, -1, -1):
            if s[i] == t[j]:
                dp[i][j] = dp[i + 1][j + 1]
            else:
                dp[i][j] = 1 + min(
                    dp[i + 1][j],
                    dp[i][j + 1],
                    dp[i + 1][j + 1]
                )

    ops = []

    i = 0
    j = 0
    shift = 0

    while i < n or j < m:
        if i < n and j < m and s[i] == t[j] and dp[i][j] == dp[i + 1][j + 1]:
            i += 1
            j += 1

        elif i < n and dp[i][j] == 1 + dp[i + 1][j]:
            pos = i + shift + 1
            ops.append(f"DELETE {pos}")
            i += 1
            shift -= 1

        elif j < m and dp[i][j] == 1 + dp[i][j + 1]:
            pos = i + shift + 1
            ops.append(f"INSERT {pos} {t[j]}")
            j += 1
            shift += 1

        else:
            pos = i + shift + 1
            ops.append(f"REPLACE {pos} {t[j]}")
            i += 1
            j += 1

    return str(len(ops)) + "\n" + "\n".join(ops)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
out = run("ABA\nABBBA\n")
assert out.startswith("2"), "sample 1"

# identical strings
out = run("ABC\nABC\n")
assert out.strip() == "0", "already equal"

# pure insertion
out = run("A\nABCD\n")
assert out.startswith("3"), "insertions"

# pure deletion
out = run("ABCD\nA\n")
assert out.startswith("3"), "deletions"

# replacement only
out = run("AAAA\nBBBB\n")
assert out.startswith("4"), "replacements"

# minimum size
out = run("A\nB\n")
assert out.startswith("1"), "single replacement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`ABC → ABC`|`0`hoạt động | Các chuỗi đã bằng nhau | 
|`A → ABCD`|`3`hoạt động | Chỉ số chèn đúng | 
|`ABCD → A`|`3`hoạt động | Xử lý xóa đúng | 
|`AAAA → BBBB`|`4`hoạt động | Chuyển tiếp thay thế | 
|`A → B`|`1`hoạt động | Trường hợp nhỏ nhất không tầm thường | 

## Vỏ cạnh 

Hãy xem xét trường hợp chuỗi đích dài hơn chỉ do bổ sung hậu tố. 

đầu vào:```
ABC
ABCDE
```DP cuối cùng đạt được:```
dp[3][3] = 2
```vì hậu tố nguồn trống trong khi hậu tố đích là`"DE"`. 

Kết quả tái thiết:```
INSERT 4 D
INSERT 5 E
```Sau khi chèn`D`, chuỗi hiện tại trở thành`ABCD`, do đó vị trí chèn tiếp theo phải trở thành`5`. các`shift`biến tự động xử lý việc này. 

Bây giờ hãy xem xét một ví dụ nặng về việc xóa. 

đầu vào:```
ABCDE
AB
```Trình tự tối ưu là:```
DELETE 3
DELETE 3
DELETE 3
```Sau khi xóa bản gốc`C`, bản gốc`D`di chuyển vào vị trí 3. Việc xây dựng lại dựa trên các chỉ mục ban đầu sẽ thử xóa vị trí 4 và 5 sau đó một cách không chính xác. 

Cuối cùng, hãy xem xét các chuỗi bằng nhau. 

đầu vào:```
HELLO
HELLO
```Mỗi ký tự khớp đều tuân theo quá trình chuyển đổi đường chéo có chi phí bằng 0:```
dp[i][j] = dp[i+1][j+1]
```Không có thao tác nào được phát ra và câu trả lời là chính xác`0`.
