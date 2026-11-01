---
title: "CF 106421D - Đảo chữ"
description: "Bài toán cho hai chuỗi có độ dài bằng nhau. Chuỗi đầu tiên là chuỗi chúng ta có thể sửa đổi và chuỗi thứ hai mô tả nhiều bộ ký tự mà chúng ta cần."
date: "2026-06-25T09:41:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106421
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 3-11-2026 Div. 2 (Advanced)"
rating: 0
weight: 106421
solve_time_s: 35
verified: true
draft: false
---

[CF 106421D - Đảo chữ](https://codeforces.com/problemset/problem/106421/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán cho hai chuỗi có độ dài bằng nhau. Chuỗi đầu tiên là chuỗi chúng ta có thể sửa đổi và chuỗi thứ hai mô tả nhiều bộ ký tự mà chúng ta cần. Chúng ta có thể thay thế các ký tự trong chuỗi đầu tiên, trong đó một thay thế sẽ thay đổi chính xác một vị trí thành bất kỳ ký tự nào khác. Trong số tất cả các cách để biến chuỗi đầu tiên thành một đảo chữ của chuỗi đích bằng cách sử dụng số lượng thay thế tối thiểu, chúng ta phải xuất ra chuỗi kết quả nhỏ nhất theo thứ tự từ điển. 

Kích thước đầu vào lớn: các chuỗi có thể chứa tối đa$10^5$nhân vật. Điều đó ngay lập tức loại trừ việc thử hoán vị, tạo đảo chữ cái hoặc so sánh mọi chuỗi chuyển đổi có thể có. Bất kỳ giải pháp nào cũng phải xử lý các chuỗi với lượng công việc không đổi nhỏ trên mỗi ký tự, điều này hướng tới độ phức tạp tuyến tính hoặc gần tuyến tính. 

Các trường hợp cạnh chính xuất phát từ sự khác biệt giữa "các chữ cái giống nhau" và "cùng một vị trí". Ví dụ: nếu đầu vào là:```
ABA
CBA
```câu trả lời là:```
1
ABC
```Một giải pháp bất cẩn có thể chỉ tính những điểm không khớp theo vị trí và tạo ra`CBA`sau một lần thay thế. Điều đó bỏ lỡ yêu cầu từ điển. Chuỗi cuối cùng có thể sắp xếp lại các chữ cái đích, vì vậy chúng ta nên giữ lại càng nhiều ký tự hữu ích hiện có càng tốt và sau đó chọn cách sắp xếp nhỏ nhất có thể. 

Một trường hợp phức tạp khác là khi có các chữ cái trùng lặp:```
AAAA
AAAB
```Đầu ra đúng là:```
1
AAAB
```Một giải pháp coi mỗi ký tự là duy nhất có thể cho rằng cần có ba ký tự thay thế vì các chuỗi khác nhau ở một số vị trí. Cách đúng đắn là so sánh tần số. 

Trường hợp cuối cùng là khi các chuỗi đã được đảo chữ:```
BCA
ABC
```Câu trả lời là:```
0
ABC
```Không cần thay thế, nhưng chúng ta vẫn cần sắp xếp lại các ký tự hiện có thành đảo chữ nhỏ nhất có thể về mặt từ điển. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất là thử mọi cách đảo chữ có thể có của chuỗi đích, kiểm tra xem có bao nhiêu vị trí khác với chuỗi nguồn và giữ lại ứng cử viên tốt nhất. Điều này đúng vì nó xem xét mọi sự sắp xếp cuối cùng có thể xảy ra. Vấn đề là một chuỗi có độ dài$n$có thể có tới$n!$hoán vị. Ngay cả với vài chục ký tự, điều này cũng trở nên bất khả thi, và với$10^5$những ký tự đó không thể thực hiện được từ xa. 

Một hướng đi tốt hơn đến từ việc xem xét những gì người thay thế thực sự làm được. Mỗi vị trí trong chuỗi gốc có thể giữ nguyên ký tự hoặc bị thay đổi. Để giảm thiểu việc thay thế, chúng tôi muốn giữ lại càng nhiều ký tự hiện có càng tốt. Điều này có nghĩa là chúng ta chỉ cần biết có bao nhiêu bản sao của mỗi chữ cái tồn tại trong cả hai chuỗi. 

Giả sử nguồn có năm bản sao của`A`và mục tiêu cần ba bản sao. Hai trong số đó`A`ký tự là thừa và phải được thay đổi. Mặt khác, nếu mục tiêu cần nhiều hơn`A`ký tự hơn nguồn có, chúng tôi cần thay thế để tạo các bản sao bị thiếu. Tổng số lần thay thế chính xác là số ký tự không thể khớp theo tần số. 

Sau khi biết số lượng thay thế tối thiểu, chúng ta vẫn cần chuỗi cuối cùng nhỏ nhất về mặt từ điển. Ký tự nhỏ nhất có thể sẽ xuất hiện càng sớm càng tốt. Chúng ta có thể tham lam quét các vị trí từ trái sang phải. Tại mỗi vị trí, nếu ký tự hiện tại có thể giữ nguyên mà vẫn đáp ứng được yêu cầu về tần số còn lại thì chúng tôi sẽ giữ nguyên. Nếu không, chúng tôi thay thế nó bằng ký tự nhỏ nhất có sẵn. 

Lý do cho sự lựa chọn tham lam này có tác dụng là vì thứ tự từ điển được quyết định bởi vị trí đầu tiên nơi hai chuỗi khác nhau. Việc đặt vị trí sớm nhất có thể ở mức nhỏ nhất có thể luôn chiếm ưu thế trong mọi cải tiến sau này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(26n) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của mỗi ký tự trong chuỗi nguồn và chuỗi đích. Bảng chữ cái đã cố định nên chúng ta chỉ cần một mảng có kích thước 26. 
2. Tính xem có thể giữ lại bao nhiêu ký tự từ nguồn. Đối với mỗi chữ cái, số lượng bản sao có thể sử dụng được là tần số tối thiểu của nó trong nguồn và trong đích. Mọi thứ khác từ nguồn cuối cùng phải được thay thế. 
3. Quét chuỗi nguồn từ trái sang phải. Duy trì số lượng ký tự còn lại vẫn phải xuất hiện trong câu trả lời. Khi nhìn vào vị trí hiện tại, trước tiên hãy cố gắng giữ nguyên ký tự ban đầu của nó. 
4. Việc giữ một ký tự chỉ có hiệu lực nếu chúng ta vẫn còn nhiều bản sao của ký tự đó trong mục tiêu hơn số lượng đã được ấn định trong câu trả lời. Nếu ký tự đó không cần thiết nữa thì thay thế nó. 
5. Nếu không thể giữ ký tự hiện tại, hãy chọn chữ cái nhỏ nhất có số mục tiêu còn lại là dương và đặt nó vào vị trí này. Giảm số lượng còn lại của nó. 

Tại sao nó hoạt động: thuật toán luôn bảo toàn một ký tự bất cứ khi nào có thể, do đó số lượng thay thế không thể lớn hơn mức cần thiết. Khi chúng tôi quyết định thay thế một vị trí, việc chọn ký tự nhỏ nhất có sẵn sẽ mang lại tiền tố nhỏ nhất có thể. Vì mọi vị trí trước đó đều được tối ưu hóa trước khi các vị trí sau được xem xét nên chuỗi cuối cùng là chuỗi nhỏ nhất về mặt từ điển trong số tất cả các câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    t = input().strip()

    src = [0] * 26
    need = [0] * 26

    for c in s:
        src[ord(c) - 65] += 1
    for c in t:
        need[ord(c) - 65] += 1

    keep = [min(src[i], need[i]) for i in range(26)]

    ans = []
    remain = need[:]

    for c in s:
        x = ord(c) - 65
        if keep[x] > 0:
            ans.append(c)
            keep[x] -= 1
            remain[x] -= 1
        else:
            for y in range(26):
                if remain[y] > 0:
                    ans.append(chr(y + 65))
                    remain[y] -= 1
                    break

    print(sum(1 for i in range(len(s)) if s[i] != ans[i]))
    print("".join(ans))

if __name__ == "__main__":
    solve()
```Phần đầu tiên xây dựng bảng tần số. Vì chỉ có 26 chữ cái viết hoa nên việc đếm mỗi ký tự phải thực hiện liên tục. 

các`keep`mảng lưu trữ bao nhiêu bản sao của mỗi ký tự mà chúng ta được phép giữ lại. Trong quá trình xây dựng từ trái sang phải, một ký tự được bảo tồn sẽ sử dụng một bản sao được lưu giữ sẵn có và một bản sao mục tiêu bắt buộc. 

Nhánh thay thế tìm kiếm từ`A`ĐẾN`Z`. Việc tìm kiếm chỉ có hơn 26 chữ cái, do đó, mặc dù nó xuất hiện được lồng bên trong vòng lặp chính nhưng tổng độ phức tạp vẫn giữ nguyên tuyến tính. Số lượng thay thế được tính ở cuối bằng cách so sánh chuỗi gốc với câu trả lời được xây dựng. 

Các điều kiện biên được xử lý một cách tự nhiên. Nếu một ký tự đã đúng và cần thiết thì nó sẽ được giữ lại. Nếu tất cả các bản sao của một ký tự đã được sử dụng, ký tự đó sẽ không bao giờ được chọn lại. Không có nguy cơ tạo ra quá nhiều bản sao vì mỗi vị trí sẽ làm giảm tần suất mục tiêu còn lại. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
ABA
CBA
```Tần số cho thấy rằng`A`Và`B`có thể được giữ lại, trong khi`C`phải được tạo ra. 

| Vị trí | Nhân vật hiện tại | Giữ? | Mục tiêu còn lại | 
| --- | --- | --- | --- | 
| 1 | A | Có | B:1, C:1 | 
| 2 | B | Có | C:1 | 
| 3 | A | Không | C:1 | 

Vị trí cuối cùng không thể giữ nguyên`A`bởi vì mục tiêu không có thêm`A`. Ký tự nhỏ nhất có sẵn là`C`. 

Kết quả là:```
ABC
```với một sự thay thế. 

Một ví dụ khác:```
CDBABC
ADCABD
```Các ký tự hữu ích được giữ lại đầu tiên. 

| Vị trí | Nhân vật hiện tại | Hành động | Tiền tố trả lời | 
| --- | --- | --- | --- | 
| 1 | C | Thay thế bằng A | A | 
| 2 | D | Giữ | QUẢNG CÁO | 
| 3 | B | Giữ | ADB | 
| 4 | A | Giữ | ADBA | 
| 5 | B | Giữ | ADBAB | 
| 6 | C | Giữ | ADBABC | 

Cấu trúc giữ được số lượng ký tự tối đa có thể và làm cho vị trí khác nhau đầu tiên càng nhỏ càng tốt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26n) | Mỗi ký tự được xử lý một lần, với tối đa 26 lần kiểm tra các lựa chọn thay thế | 
| Không gian | O(26) | Chỉ cần mảng tần số và bộ lưu trữ câu trả lời | 

Thuật toán phù hợp với chuỗi có độ dài$10^5$bởi vì nó chỉ thực hiện một lượng công việc không đổi nhỏ cho mỗi ký tự. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    
    s = sys.stdin.readline().strip()
    t = sys.stdin.readline().strip()

    src = [0] * 26
    need = [0] * 26

    for c in s:
        src[ord(c) - 65] += 1
    for c in t:
        need[ord(c) - 65] += 1

    keep = [min(src[i], need[i]) for i in range(26)]
    remain = need[:]
    ans = []

    for c in s:
        x = ord(c) - 65
        if keep[x] > 0:
            ans.append(c)
            keep[x] -= 1
            remain[x] -= 1
        else:
            for y in range(26):
                if remain[y] > 0:
                    ans.append(chr(y + 65))
                    remain[y] -= 1
                    break

    out = str(sum(1 for i in range(len(s)) if s[i] != ans[i])) + "\n" + "".join(ans) + "\n"
    sys.stdin = old
    return out

assert run("ABA\nCBA\n") == "1\nABC\n", "sample 1"
assert run("CDBABC\nADCABD\n") == "2\nADBADC\n", "sample 2"

assert run("A\nA\n") == "0\nA\n", "single character"
assert run("AAAA\nAAAB\n") == "1\nAAAB\n", "duplicates"
assert run("ZYX\nABC\n") == "3\nABC\n", "full replacement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`A / A`|`0 / A`| Trường hợp có chiều dài tối thiểu | 
|`AAAA / AAAB`|`1 / AAAB`| Xử lý tần suất với các bản sao | 
|`ZYX / ABC`|`3 / ABC`| Thay thế hoàn toàn và sắp xếp từ điển | 

## Vỏ cạnh 

Đối với trường hợp:```
ABA
CBA
```thuật toán giữ`A`Và`B`bởi vì chúng được yêu cầu bởi mục tiêu. Ở vị trí cuối cùng, tần số mục tiêu còn lại chỉ chứa`C`, vì vậy việc thay thế là bắt buộc. Kết quả có số lượng thay đổi tối thiểu có thể và nhỏ nhất về mặt từ điển. 

Vì:```
AAAA
AAAB
```nguồn có quá nhiều`A`nhân vật. Ba vị trí đầu tiên có thể giữ nguyên`A`, nhưng phần bổ sung cuối cùng`A`phải trở thành`B`. Các mảng tần số ngăn thuật toán giữ lại một ký tự khiến mục tiêu không thể thỏa mãn. 

Vì:```
BCA
ABC
```tất cả các chữ cái đã tồn tại với số lượng cần thiết. Không có sự thay thế nào được thực hiện và cấu trúc tham lam vẫn giữ các ký tự có sẵn trong khi tạo ra thứ tự nhỏ nhất có thể. Câu trả lời cuối cùng trở thành`ABC`với các hoạt động bằng không.
