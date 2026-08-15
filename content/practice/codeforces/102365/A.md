---
title: "CF 102365A - Từ bất thường"
description: "Chúng ta cần chuyển đổi một từ viết thường bằng mật mã Caesar. Đầu vào đầu tiên cho chúng ta biết nên mã hóa hay giải mã. Quá trình mã hóa sẽ di chuyển từng chữ cái về phía trước một độ dịch chuyển cố định, trong khi quá trình giải mã sẽ di chuyển từng chữ cái về phía sau một lượng như nhau."
date: "2026-08-14T02:55:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102365
codeforces_index: "A"
codeforces_contest_name: "UBC Programming Contest 2019 (UBCPC 2019)"
rating: 0
weight: 102365
solve_time_s: 78
verified: true
draft: false
---

[CF 102365A - Từ bất thường](https://codeforces.com/problemset/problem/102365/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần chuyển đổi một từ viết thường bằng mật mã Caesar. Đầu vào đầu tiên cho chúng ta biết nên mã hóa hay giải mã. Mã hóa di chuyển từng chữ cái về phía trước bằng một sự dịch chuyển cố định`s`, trong khi giải mã sẽ di chuyển mọi chữ cái lùi lại một lượng như nhau. Bảng chữ cái có tính tuần hoàn nên di chuyển qua`z`tiếp tục từ`a`, và chuyển động trước`a`tiếp tục từ`z`. 

Ví dụ, với sự dịch chuyển`4`, lá thư`x`trở thành`b`trong quá trình mã hóa vì bốn vị trí chuyển tiếp là`y`,`z`,`a`,`b`. Trong quá trình giải mã,`b`trở thành`x`vì lý do tương tự ngược lại. 

Dòng đầu vào đầu tiên là`E`hoặc`D`, thứ hai chứa sự thay đổi`s`, và từ thứ ba chứa từ đó. Từ này chứa từ 1 đến 100 chữ cái viết thường. Vì từ này quá ngắn nên ngay cả việc mô phỏng đơn giản từng ký tự cũng dễ dàng đủ nhanh. Sự thay đổi nhiều nhất là 25, do đó, ngay cả việc triển khai di chuyển một vị trí bảng chữ cái tại một thời điểm cũng sẽ hoạt động tối đa`100 * 25 = 2500`chuyển động của nhân vật. 

Phần thú vị không phải là hiệu suất mà là việc xử lý chính xác bảng chữ cái tuần hoàn. Một so sánh nhân vật trực tiếp như`ord(c) + s`có thể tạo ra một giá trị ngoài phạm vi cho các chữ cái viết thường khi kết quả đạt`z`. Việc chuyển đổi chữ cái thành số từ 0 đến 25 và sử dụng modulo 26 sẽ tránh được hoàn toàn vấn đề ranh giới đó. 

Một từ có một chữ cái là một trường hợp có kích thước tối thiểu hữu ích. Đối với đầu vào`E`, sự thay đổi`1`, và từ`a`, câu trả lời là`b`. Việc triển khai vô tình chỉ xử lý các từ có độ dài lớn hơn một sẽ không thành công ở đây. 

Bao bọc ở hai đầu là trường hợp ranh giới chính. Ví dụ, mã hóa`z`qua`1`cho`a`, không phải ký tự sau`z`bằng Unicode. Tương tự, giải mã`a`qua`1`cho`z`. Việc triển khai bất cẩn mà chỉ thêm hoặc bớt mã ký tự mà không có modulo 26 sẽ tạo ra ký tự không hợp lệ. 

Sự dịch chuyển cũng có thể lớn hơn khoảng cách đến ranh giới bảng chữ cái. Với`E`, sự thay đổi`25`, và từ`b`, kết quả là`a`. Việc coi phép dịch như một phép cộng số nguyên thông thường không có số học tuần hoàn sẽ khiến những trường hợp này dễ bị xử lý sai. 

## Phương pháp tiếp cận 

Việc triển khai bạo lực trực tiếp có thể xử lý từng ký tự bằng cách liên tục di chuyển ký tự đó một vị trí trong bảng chữ cái. Đối với mỗi ký tự, chúng tôi thực hiện dịch chuyển từng bước một, gói từ`z`ĐẾN`a`hoặc từ`a`ĐẾN`z`khi cần thiết. Điều này đúng vì việc áp dụng một lần chuyển đổi bảng chữ cái hợp lệ sẽ tạo ra chính xác sự dịch chuyển Caesar được yêu cầu. 

Với tối đa 100 ký tự và độ dịch chuyển tối đa 25 ký tự, thao tác này thực hiện tối đa 2500 chuyển động ở một vị trí. Đó không phải là gần giới hạn cho chương trình một giây, vì vậy cách tiếp cận này thực sự được chấp nhận đối với các ràng buộc nhất định. Điểm yếu của nó là thực hiện những công việc không cần thiết và che khuất cấu trúc toán học đơn giản của thao tác. 

Quan sát quan trọng là bảng chữ cái có chính xác 26 vị trí và những vị trí đó tạo thành một chu kỳ. Đại diện`a`bằng 0,`b`là 1, thông qua`z`là 25. Mã hóa sau đó trở thành`(value + s) % 26`, trong khi giải mã trở thành`(value - s) % 26`. Hoạt động modulo của Python xử lý các giá trị âm một cách chính xác, do đó việc giải mã`a`với sự thay đổi`1`tự nhiên tạo ra vị trí 25, đó là`z`. 

Phương pháp vũ phu hoạt động vì các bước di chuyển một bước lặp đi lặp lại cuối cùng cũng đến được cùng một đích. Nhận xét rằng bảng chữ cái là một chu trình cố định cho phép chúng ta thay thế tất cả các bước di chuyển riêng lẻ đó bằng một phép toán số học mô-đun. Thuật toán kết quả cần chính xác một phép biến đổi cho mỗi ký tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chuyển đổi một bước lặp đi lặp lại | O( | w | s) | O( | w | ) | Đã chấp nhận nhưng công việc không cần thiết | 
| Số học mô-đun | O( | w | ) | O( | w | ) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kiểu thao tác, ca`s`, và từ đó. Loại thao tác xác định xem nên thêm hoặc bớt dịch chuyển khỏi mỗi ký tự. 
2. Chuyển đổi từng ký tự sang vị trí bảng chữ cái dựa trên số 0 bằng cách sử dụng`ord(c) - ord('a')`. Điều này ánh xạ bảng chữ cái đến phạm vi số thuận tiện`0`bởi vì`25`. 
3. Nếu thao tác là mã hóa, hãy thêm`s`đến vị trí. Nếu hoạt động là giải mã, hãy trừ`s`. Hướng trực tiếp phù hợp với định nghĩa của hai hoạt động. 
4. Áp dụng`% 26`đến vị trí kết quả. Điều này làm cho bảng chữ cái có tính tuần hoàn, vì vậy các giá trị vượt quá 25 sẽ nằm ở đầu và các giá trị âm sẽ nằm ở cuối. 
5. Chuyển đổi vị trí kết quả trở lại ký tự chữ thường bằng`chr(position + ord('a'))`và thêm nó vào câu trả lời. 
6. In từ đã chuyển đổi. Mỗi ký tự đầu vào đã được chuyển đổi độc lập, do đó việc xử lý tất cả các ký tự sẽ tạo ra từ được mã hóa hoặc giải mã hoàn chỉnh. 

### Tại sao nó hoạt động 

Ở mỗi lần lặp, ký tự trả lời hiện tại thể hiện chính xác phép biến đổi Caesar của ký tự đầu vào tương ứng. Vị trí bảng chữ cái dựa trên số 0 của nó được tăng lên bởi`s`để mã hóa hoặc giảm đi`s`để giải mã và modulo 26 xác định vị trí duy nhất trên bảng chữ cái tuần hoàn sau chuyển động đó. Vì phép biến đổi là chính xác cho từng ký tự một cách độc lập nên việc ghép tất cả các ký tự được biến đổi sẽ mang lại chính xác từ được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

operation = input().strip()
s = int(input())
word = input().strip()

result = []

for c in word:
    pos = ord(c) - ord('a')

    if operation == 'E':
        pos = (pos + s) % 26
    else:
        pos = (pos - s) % 26

    result.append(chr(pos + ord('a')))

print(''.join(result))
```Ba dòng đầu tiên đọc thao tác, dịch chuyển và từ theo thứ tự giống như định dạng đầu vào.`strip()`xóa dòng mới khỏi mỗi dòng, trong khi vẫn giữ nguyên chữ thường thực tế. 

Đối với mỗi nhân vật,`ord(c) - ord('a')`tạo ra một giá trị từ 0 đến 25. Điều này tốt hơn là thao tác trực tiếp các giá trị ASCII vì phép toán modulo hiện tương ứng chính xác với các vị trí trong bảng chữ cái. 

Nhánh mã hóa thêm sự dịch chuyển trước khi lấy modulo 26. Để giải mã, phép trừ được sử dụng thay thế. của Python`%`toán tử ánh xạ kết quả âm trở lại phạm vi từ 0 đến 25, do đó`(0 - 1) % 26`là`25`. Điều này xử lý việc giải mã`a`vào trong`z`không có điều kiện biên đặc biệt nào. 

Kết quả được tích lũy trong một danh sách vì các chuỗi là bất biến trong Python. Việc thêm mỗi ký tự và nối một lần rất đơn giản và chạy theo thời gian tuyến tính. 

Không có vấn đề tràn số nguyên vì vị trí lớn nhất có liên quan là rất nhỏ. Chi tiết triển khai chính quan trọng là lấy modulo 26 sau phép cộng hoặc phép trừ, thay vì quên phần bao quanh ở ranh giới bảng chữ cái. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mã hóa yêu cầu đầu vào bằng shift`3`và từ`hello`. 

| Nhân vật | Vị trí | Hoạt động | Vị Trí Mới | Ký tự đầu ra | 
| --- | --- | --- | --- | --- | 
|`h`| 7 |`7 + 3`| 10 |`k`| 
|`e`| 4 |`4 + 3`| 7 |`h`| 
|`l`| 11 |`11 + 3`| 14 |`o`| 
|`l`| 11 |`11 + 3`| 14 |`o`| 
|`o`| 14 |`14 + 3`| 17 |`r`| 

Từ kết quả là`khoor`. Không có ký tự nào vượt qua phần cuối của bảng chữ cái, vì vậy modulo 26 không thay đổi rõ ràng bất kỳ vị trí nào trong số này. Dấu vết thể hiện sự chuyển đổi mã hóa cơ bản. 

### Mẫu 2 

Giải mã yêu cầu đầu vào bằng shift`3`và từ`jreeohghbjrrn`. 

| Nhân vật | Vị trí | Hoạt động | Vị Trí Mới | Ký tự đầu ra | 
| --- | --- | --- | --- | --- | 
|`j`| 9 |`9 - 3`| 6 |`g`| 
|`r`| 17 |`17 - 3`| 14 |`o`| 
|`e`| 4 |`4 - 3`| 1 |`b`| 
|`e`| 4 |`4 - 3`| 1 |`b`| 
|`o`| 14 |`14 - 3`| 11 |`l`| 
|`h`| 7 |`7 - 3`| 4 |`e`| 
|`g`| 6 |`6 - 3`| 3 |`d`| 
|`h`| 7 |`7 - 3`| 4 |`e`| 
|`b`| 1 |`1 - 3`| 24 |`y`| 
|`j`| 9 |`9 - 3`| 6 |`g`| 
|`r`| 17 |`17 - 3`| 14 |`o`| 
|`r`| 17 |`17 - 3`| 14 |`o`| 
|`n`| 13 |`13 - 3`| 10 |`k`| 

Kết quả là`gobbledeygook`. các`b`ở vị trí thứ chín đặc biệt hữu ích vì vị trí của nó trở thành`-1`trước khi áp dụng modulo và`-1 % 26`cho`25`, ánh xạ chính xác tới`y`. Điều này chứng tỏ tại sao số học mô-đun xử lý ranh giới tuần hoàn mà không có trường hợp đặc biệt riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | w | ) | Mỗi ký tự được xử lý một lần với số học theo thời gian không đổi. | 
| Không gian | O( | w | ) | Các ký tự được chuyển đổi sẽ được lưu trữ trước khi nối chúng. | 

Từ này có nhiều nhất 100 ký tự nên thuật toán chỉ thực hiện vài trăm phép tính cơ bản trong trường hợp xấu nhất. Nó thoải mái trong giới hạn thời gian một giây và sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    operation = input().strip()
    s = int(input())
    word = input().strip()

    result = []

    for c in word:
        pos = ord(c) - ord('a')

        if operation == 'E':
            pos = (pos + s) % 26
        else:
            pos = (pos - s) % 26

        result.append(chr(pos + ord('a')))

    print(''.join(result))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()
    output = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return output

assert run("E\n3\nhello\n") == "khoor", "sample 1"
assert run("D\n3\njreeohghbjrrn\n") == "gobbledeygook", "sample 2"

assert run("E\n1\na\n") == "b", "minimum-size encoding"
assert run("D\n1\na\n") == "z", "minimum-size decoding with wraparound"
assert run("E\n1\nzzzzzzzzzzzzzzzzzzzz\n") == "aaaaaaaaaaaaaaaaaaaa", "all-equal boundary case"
assert run("D\n25\nabcdefghijklmnopqrstuvwxyz\n") == "bcdefghijklmnopqrstuvwxyza", "maximum shift and full alphabet"
assert run("E\n25\nb\n") == "a", "large shift crossing the alphabet boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`E`,`1`,`a`|`b`| Kích thước từ tối thiểu và mã hóa thông thường | 
|`D`,`1`,`a`|`z`| Bao bọc ngược ở đầu bảng chữ cái | 
|`E`,`1`, lặp lại`z`| lặp đi lặp lại`a`| Chuyển tiếp các ký tự bao quanh và hoàn toàn bằng nhau | 
|`D`,`25`, bảng chữ cái |`bcdefghijklmnopqrstuvwxyza`| Sự thay đổi tối đa và mọi vị trí bảng chữ cái | 
|`E`,`25`,`b`|`a`| Sự thay đổi lớn với sự vượt qua ranh giới | 

## Vỏ cạnh 

Kích thước đầu vào tối thiểu`E`, sự thay đổi`1`, từ`a`sản xuất`b`. Thuật toán chuyển đổi`a`đến vị trí 0, thêm 1 và nhận vị trí 1, chuyển đổi trở lại`b`. Không có giả định rằng từ có chứa nhiều ký tự. 

Để bao bọc phía trước, hãy xem xét`E`, sự thay đổi`1`, từ`z`. nhân vật`z`có vị trí 25. Cộng ca sẽ được 26, và`26 % 26`là 0 nên kết quả là`a`. Việc bổ sung mã ký tự trực tiếp sẽ không có hành vi này. 

Để bao bọc ngược lại, hãy xem xét`D`, sự thay đổi`1`, từ`a`. Vị trí là 0 và trừ đi sự thay đổi sẽ cho`-1`. Python đánh giá`-1 % 26`là 25, do đó kết quả trở thành`z`. Đây là trường hợp chính giúp phân biệt số học mô-đun chính xác với việc triển khai chỉ xử lý các vị trí không âm. 

Đối với một sự thay đổi lớn, hãy xem xét`E`, sự thay đổi`25`, từ`b`. Vị trí của`b`là 1, do đó vị trí được biến đổi là`(1 + 25) % 26 = 0`. Đầu ra là`a`. Điều này phát hiện các triển khai vô tình sử dụng quy tắc đặc biệt chỉ dành cho những ca gần với ranh giới bảng chữ cái. 

Cuối cùng, hãy xem xét một từ chỉ chứa`z`các ký tự, chẳng hạn như`E`, sự thay đổi`1`, từ`zzzz`. Mỗi ký tự ánh xạ độc lập từ vị trí 25 đến vị trí 0, tạo ra`aaaa`. Bất biến trên mỗi ký tự vẫn hợp lệ ngay cả khi mọi ký tự thực hiện cùng một điều kiện biên.
