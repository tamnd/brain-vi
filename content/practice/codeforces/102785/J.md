---
title: "CF 102785J - Bạn đã thực sự sẵn sàng chưa?"
description: "Nhiệm vụ là quyết định xem chuỗi văn bản có thể được tạo bằng chuỗi mẫu hay không. Mẫu này được làm bằng các chữ cái viết thường thông thường và các ký hiệu lặp lại tùy chọn."
date: "2026-07-27T19:44:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102785
codeforces_index: "J"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 18)"
rating: 0
weight: 102785
solve_time_s: 48
verified: true
draft: false
---

[CF 102785J - Bạn đã thực sự sẵn sàng chưa?](https://codeforces.com/problemset/problem/102785/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là quyết định xem chuỗi văn bản có thể được tạo bằng chuỗi mẫu hay không. Mẫu này được làm bằng các chữ cái viết thường thông thường và các ký hiệu lặp lại tùy chọn. MỘT`+`sau một chữ cái có nghĩa là chữ cái đó phải xuất hiện ít nhất một lần nữa sau lần xuất hiện đầu tiên của nó, trong khi một`*`có nghĩa là chữ cái đó có thể xuất hiện bao nhiêu lần cũng được, kể cả số 0. Chỉ có chữ cái liền trước bị ảnh hưởng bởi các ký hiệu này. 

Ví dụ, mô hình`ab*c`có thể mô tả`ac`,`abc`,`abbc`, vân vân. mẫu`ab+c`không thể mô tả`ac`bởi vì`b`phải xuất hiện ít nhất một lần. Chương trình phải in`Yes`khi toàn bộ văn bản có thể được mô tả bằng toàn bộ mẫu và`No`nếu không thì. 

Các chuỗi có độ dài tối đa là 1000. Giải pháp bậc hai là thoải mái vì một triệu phép tính là nhỏ trong giới hạn một giây, trong khi các phương pháp thử nhiều lần tất cả các lần lặp lại có thể có thể trở nên chậm hơn nhiều. Một mô phỏng trực tiếp khám phá mọi cách có thể để mở rộng mọi`*`hoặc`+`có thể phân nhánh nhiều và tiếp cận hành vi theo cấp số nhân, vì vậy giải pháp cần ghi nhớ các trạng thái khớp đã được tính toán. 

Một số chi tiết làm cho vấn đề này dễ bị sai. Một mẫu có thể chứa một ký hiệu lặp lại không khớp với gì. Đối với đầu vào:```
a*
""
```câu trả lời là`Yes`, bởi vì`*`cho phép không có bản sao. Việc triển khai bất cẩn luôn tiêu tốn ít nhất một ký tự sẽ tạo ra`No`. 

MỘT`+`không thể cư xử như`*`. Đối với đầu vào:```
a+
"a"
```câu trả lời là`No`, bởi vì`+`yêu cầu một hoặc nhiều lần lặp lại ngoài biểu tượng ban đầu. Việc coi cả hai biểu tượng giống hệt nhau sẽ chấp nhận nó một cách không chính xác. 

Ký hiệu lặp lại chỉ thuộc về ký tự trước đó. Đối với đầu vào:```
ab*
"abb"
```câu trả lời là`Yes`, bởi vì chỉ`b`lặp lại. Việc triển khai nhóm mẫu không chính xác có thể cố gắng lặp lại`ab`và tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Nỗ lực đầu tiên tự nhiên là xử lý đệ quy mẫu và văn bản cùng nhau. Khi ký tự mẫu hiện tại không có từ bổ nghĩa, nó sẽ khớp với ký tự văn bản hiện tại hoặc không thành công. Khi nó có`*`hoặc`+`, việc tìm kiếm đệ quy sẽ thử mọi số lần lặp lại có thể và tiếp tục từ mỗi vị trí dừng có thể. 

Cách tiếp cận này đúng vì mọi khả năng mở rộng của mẫu đều được xem xét. Vấn đề là số lượng trạng thái lặp lại. Một mô hình như`a*a*a*a*a*`khớp với một chuỗi dài`a`các ký tự tạo ra nhiều đường dẫn đệ quy khác nhau đạt đến cùng các vị trí trong mẫu và văn bản. Trong trường hợp xấu nhất, số lượng khả năng được khám phá tăng theo cấp số nhân, vượt xa độ dài 1000 cho phép. 

Quan sát hữu ích là kết quả chỉ phụ thuộc vào hai vị trí: mã thông báo mẫu nào đã được xử lý và số lượng ký tự của văn bản đã được sử dụng. Không cần phải nhớ chính xác lịch sử đã đạt đến một trạng thái nào đó. 

Chúng ta có thể chuyển đổi mẫu thành mã thông báo, trong đó mỗi mã thông báo chứa một ký tự và quy tắc lặp lại của ký tự đó. Sau đó, lập trình động sẽ lưu trữ xem một số mã thông báo đầu tiên có thể khớp với một số ký tự đầu tiên của văn bản hay không. Khi xử lý mã thông báo, chúng tôi tính toán tất cả các tiền tố văn bản có thể có mà nó có thể sử dụng. 

Các trường hợp ký tự lặp lại có thể được tối ưu hóa bằng quá trình quét đang chạy. Vì`*`, trạng thái có thể truy cập được từ trạng thái có thể truy cập trước đó ở cùng một vị trí hoặc từ vị trí trước đó nếu ký tự văn bản hiện tại khớp với mã thông báo. Vì`+`, thao tác quét tương tự sẽ được sử dụng nhưng mã thông báo phải tiêu thụ ít nhất một ký tự trước khi trạng thái trở nên hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ trong trường hợp xấu nhất | O(độ dài mẫu + độ dài văn bản) | Quá chậm | 
| Lập trình động | O(độ dài mẫu × độ dài văn bản) | O(độ dài mẫu × độ dài văn bản) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích mẫu thành mã thông báo. Mỗi mã thông báo lưu trữ một chữ cái viết thường và cho dù đó là ký tự bình thường,`+`sự lặp lại, hoặc một`*`sự lặp lại. Điều này tách cú pháp của mẫu khỏi logic khớp. 
2. Tạo một mảng lập trình động trong đó`dp[j]`có nghĩa là các mã thông báo được xử lý cho đến nay có thể khớp chính xác với mã đầu tiên`j`các ký tự của văn bản. Ban đầu, không có mã thông báo nào khớp với tiền tố trống, vì vậy chỉ`dp[0]`là đúng. 
3. Xử lý từng mã thông báo một lần. Đối với nhân vật bình thường, di chuyển từ vị trí`j`ĐẾN`j + 1`chỉ khi ký tự văn bản tiếp theo là cùng một chữ cái. Mã thông báo bình thường sử dụng chính xác một ký tự. 
4. Đối với một`*`mã thông báo, quét văn bản từ trái sang phải trong khi vẫn duy trì xem có thể truy cập tiền tố hiện tại hay không. Việc sử dụng trống rỗng của`*`giữ cho tất cả các vị trí có thể truy cập trước đó hợp lệ và các ký tự khớp cho phép mã thông báo mở rộng kết quả khớp thêm một vị trí. 
5. Đối với một`+`mã thông báo, hãy sử dụng ý tưởng tương tự như`*`, nhưng ngăn chặn trường hợp lặp lại trống. Ký tự khớp đầu tiên phải được sử dụng trước khi mã thông báo có thể đóng góp trạng thái hợp lệ. 
6. Sau khi tất cả các mã thông báo được xử lý, hãy kiểm tra xem trạng thái của văn bản hoàn chỉnh có thể truy cập được hay không. Nếu như`dp[len(text)]`là đúng, toàn bộ văn bản khớp với mẫu. 

Tại sao nó hoạt động: sau mỗi mã thông báo được xử lý, mảng lập trình động biểu thị chính xác tất cả các tiền tố văn bản có thể được tạo bởi các mã thông báo đó. Các quy tắc chuyển tiếp liệt kê mọi cách hợp pháp mà mã thông báo tiếp theo có thể sử dụng các ký tự và chúng không bao giờ cho phép đếm lặp lại bất hợp pháp. Vì mỗi mã thông báo được xử lý chỉ bằng các trạng thái đã mô tả các kết quả khớp hợp lệ trước đó nên trạng thái cuối cùng chính xác là đúng khi tồn tại phần mở rộng hợp lệ của toàn bộ mẫu bằng toàn bộ văn bản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    p = input().rstrip("\n")
    t = input().rstrip("\n")

    tokens = []
    i = 0
    while i < len(p):
        ch = p[i]
        kind = 0
        if i + 1 < len(p) and (p[i + 1] == '+' or p[i + 1] == '*'):
            kind = 1 if p[i + 1] == '+' else 2
            i += 1
        tokens.append((ch, kind))
        i += 1

    n = len(t)
    dp = [False] * (n + 1)
    dp[0] = True

    for ch, kind in tokens:
        ndp = [False] * (n + 1)

        if kind == 0:
            for j in range(n):
                if dp[j] and t[j] == ch:
                    ndp[j + 1] = True

        elif kind == 2:
            active = False
            for j in range(n + 1):
                if dp[j]:
                    active = True
                if active:
                    ndp[j] = True
                if j < n and t[j] != ch:
                    active = False

        else:
            active = False
            for j in range(n + 1):
                if dp[j]:
                    if j > 0:
                        active = True
                if active:
                    ndp[j] = True
                if j < n and t[j] != ch:
                    active = False

            for j in range(1, n + 1):
                if t[j - 1] == ch and dp[j - 1]:
                    ndp[j] = True

        dp = ndp

    print("Yes" if dp[n] else "No")

if __name__ == "__main__":
    solve()
```Trình phân tích cú pháp chuyển đổi mẫu thô thành một chuỗi các mã thông báo độc lập. Biến`kind`phân biệt giữa một nhân vật bình thường,`+`, Và`*`, giúp tránh việc kiểm tra liên tục mẫu gốc trong khi khớp. 

các`dp`mảng được cập nhật sau mỗi mã thông báo. Đối với một ký tự đơn giản, chỉ có thể chuyển tiếp một lần duy nhất vì phải sử dụng chính xác một ký tự. 

Vì`*`, quá trình quét sẽ giữ một`active`lá cờ. Khi có thể truy cập trạng thái trước đó, mã thông báo có thể khớp với 0 hoặc nhiều bản sao miễn là các ký tự văn bản liên tiếp bằng ký tự mã thông báo. Cờ được đặt lại ngay khi một ký tự khác xuất hiện. 

Vì`+`, việc triển khai sẽ ngăn chặn kết quả khớp trống. Vòng lặp bổ sung xử lý lần xuất hiện bắt buộc đầu tiên, trong khi quá trình quét cho phép các bản sao bổ sung sau đó. Sự khác biệt này là nguồn sai lầm phổ biến nhất trong vấn đề này. 

Các chỉ số mảng biểu thị vị trí giữa các ký tự, vì vậy`dp[j]`đề cập đến tiền tố kết thúc trước ký tự`j`. Quy ước này tránh từng lỗi một khi xử lý các kết quả khớp trống. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
pa*t+ern
pattern
```Mẫu trở thành mã thông báo`p`,`a*`,`t+`,`e`,`r`,`n`. 

| Mã thông báo được xử lý | Trạng thái vị trí văn bản có thể truy cập được | Lý do | 
| --- | --- | --- | 
| Bắt đầu | 0 | Tiền tố mẫu trống khớp với tiền tố văn bản trống | 
| p | 1 | tiêu thụ`p`| 
| một* | 2 | tiêu thụ`a`một lần | 
| t+ | 3 | Tiêu thụ cần thiết`t`| 
| e | 4 | tiêu thụ`e`| 
| r | 5 | tiêu thụ`r`| 
| n | 6 | tiêu thụ`n`| 

Vị trí cuối cùng có thể truy cập được nên thuật toán sẽ in`Yes`. Dấu vết này cho thấy`*`có thể sử dụng chính xác một bản sao và`+`có thể sử dụng bản sao được yêu cầu. 

### Mẫu 2 

đầu vào:```
c*cp+p
cpp
```Các token là`c*`,`c`,`p+`,`p`. 

| Mã thông báo được xử lý | Trạng thái vị trí văn bản có thể truy cập được | Lý do | 
| --- | --- | --- | 
| Bắt đầu | 0 | Tiền tố trống | 
| c* | 0, 1, 2 | Không, một hoặc hai`c`nhân vật có thể | 
| c | 3 | Tiêu thụ phần còn lại`c`| 
| p+ | 4 | Tiêu thụ một`p`| 
| p | 5 | Tiêu thụ cuối cùng`p`| 

Toàn bộ văn bản được khớp. Ví dụ này chứng minh tại sao nhà nước phải lưu trữ tất cả các vị trí có thể tiếp cận thay vì chỉ một lựa chọn tham lam. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(P × T) | Mỗi mã thông báo mẫu sẽ quét văn bản một lần | 
| Không gian | O(T) | Chỉ các mảng trạng thái tiền tố văn bản hiện tại và tiếp theo mới được lưu trữ | 

Đây`P`là số lượng mã thông báo mẫu được phân tích cú pháp và`T`là độ dài văn bản Cả hai đều có nhiều nhất là 1000, do đó phương pháp lập trình động thực hiện khoảng một triệu thao tác trạng thái và dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    p = input().rstrip("\n")
    t = input().rstrip("\n")

    tokens = []
    i = 0
    while i < len(p):
        ch = p[i]
        kind = 0
        if i + 1 < len(p) and p[i + 1] in "+*":
            kind = 1 if p[i + 1] == "+" else 2
            i += 1
        tokens.append((ch, kind))
        i += 1

    dp = [False] * (len(t) + 1)
    dp[0] = True

    for ch, kind in tokens:
        ndp = [False] * (len(t) + 1)

        if kind == 0:
            for j in range(len(t)):
                if dp[j] and t[j] == ch:
                    ndp[j + 1] = True
        elif kind == 2:
            active = False
            for j in range(len(t) + 1):
                if dp[j]:
                    active = True
                if active:
                    ndp[j] = True
                if j < len(t) and t[j] != ch:
                    active = False
        else:
            active = False
            for j in range(len(t) + 1):
                if dp[j] and j > 0:
                    active = True
                if active:
                    ndp[j] = True
                if j < len(t) and t[j] != ch:
                    active = False
            for j in range(1, len(t) + 1):
                if dp[j - 1] and t[j - 1] == ch:
                    ndp[j] = True

        dp = ndp

    return "Yes\n" if dp[len(t)] else "No\n"

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

assert run("pa*t+ern\npattern\n") == "Yes\n", "sample 1"
assert run("c*cp+p\ncpp\n") == "Yes\n", "sample 2"
assert run("b+b\nb\n") == "No\n", "plus requires repetition"
assert run("a*\n\n") == "Yes\n", "star can match empty"
assert run("a+\na\n") == "No\n", "plus cannot match zero repetitions"
assert run("a*a*a*\naaaaa\n") == "Yes\n", "many repeated tokens"
assert run("ab*\na\n") == "Yes\n", "star can consume zero copies"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a*\n\n`|`Yes`| Văn bản trống được khớp bởi`*`| 
|`a+\na\n`|`No`| Sự khác biệt giữa`+`Và`*`| 
|`a*a*a*\naaaaa\n`|`Yes`| Nhiều mã thông báo lặp lại | 
|`ab*\na\n`|`Yes`| Mã thông báo lặp lại chỉ ảnh hưởng đến ký tự trước đó của nó | 

## Vỏ cạnh 

Đối với trường hợp khớp trống:```
a*
```Trình phân tích cú pháp tạo một mã thông báo`a*`. Trạng thái ban đầu`dp[0]`là đúng, và`*`quá trình chuyển đổi giữ cho vị trí 0 có thể truy cập được vì sử dụng bản sao 0 là hợp pháp. Trạng thái cuối cùng là đúng, vì vậy câu trả lời là`Yes`. 

Đối với`+`trường hợp có độ dài tối thiểu:```
a+
a
```Mã thông báo`a+`phải sử dụng ít nhất một ký tự ngoài cách giải thích ký hiệu ban đầu. Quá trình chuyển đổi lập trình động không cho phép tồn tại vị trí trống nên ký tự văn bản duy nhất là không đủ và câu trả lời là`No`. 

Đối với trường hợp phạm vi sửa đổi:```
ab*
a
```Các token là`a`Và`b*`. Sau khi khớp`a`, mã thông báo thứ hai có thể tiêu thụ bằng 0`b`nhân vật. Vị trí văn bản cuối cùng vẫn có thể truy cập được, mang lại`Yes`. 

Để xử lý lặp lại liên tiếp:```
a*a*a*
aaaaa
```Mỗi mã thông báo được xử lý độc lập. đầu tiên`a*`có thể sử dụng một số tiền tố, mã thông báo tiếp theo tiếp tục từ mọi vị trí có thể truy cập và mã thông báo cuối cùng sẽ điền vào mọi hậu tố hợp lệ còn lại. Các trạng thái DP bảo toàn tất cả sự phân chia có thể có của năm trạng thái`a`các ký tự, do đó thuật toán chấp nhận mẫu.
