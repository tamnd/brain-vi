---
title: "CF 102203E - \u042d\u043d\u0435\u0440\u0433\u0435\u0442\u0438\u0447\u0435\u0441\u043a\u0438\u0439 \u0441\u043f\u0435\u043a\u0442\u0440"
description: "Chúng ta có một chuỗi chữ thường s. Với mọi số nguyên dương i, xác định một chuỗi đặc biệt fi. Cái đầu tiên chỉ đơn giản là a. Để có được chuỗi tiếp theo, hãy lấy chuỗi trước đó, đặt chữ cái trong bảng chữ cái tiếp theo vào giữa và lặp lại chuỗi trước đó ở bên phải."
date: "2026-08-18T00:46:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102203
codeforces_index: "E"
codeforces_contest_name: "\u0427\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u041b\u0438\u043f\u0435\u0446\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e (8-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 102203
solve_time_s: 245
verified: true
draft: false
---

[CF 102203E - \u042d\u043d\u0435\u0440\u0433\u0435\u0442\u0438\u0447\u0435\u0441\u043a\u0438\u0439 \u0441\u043f\u0435\u043a\u0442\u0440](https://codeforces.com/problemset/problem/102203/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi chữ thường`s`. Với mọi số nguyên dương`i`, xác định một chuỗi đặc biệt`f_i`. Điều đầu tiên chỉ đơn giản là`a`. Để có được chuỗi tiếp theo, hãy lấy chuỗi trước đó, đặt chữ cái trong bảng chữ cái tiếp theo vào giữa và lặp lại chuỗi trước đó ở bên phải. Vì vậy, một vài chuỗi đầu tiên là`a`,`aba`,`abacaba`, vân vân. 

Ta cần đếm xem có bao nhiêu dãy con`s`bằng với bất kỳ chuỗi đặc biệt nào. Các lựa chọn khác nhau về chỉ số được tính là các dãy con khác nhau, ngay cả khi chúng tạo ra các chữ cái giống nhau. Câu trả lời được lấy modulo`998244353`. 

Chiều dài tăng lên như 

[ 
|f_i|=2|f_{i-1}|+1, 
] 

vậy 

[ 
|f_i|=2^i-1. 
]

Từ`|s| <= 5000`, chỉ một`f_1`bởi vì`f_12`có thể xảy ra, bởi vì`f_12`có chiều dài`4095`, trong khi`f_13`đã có chiều dài rồi`8191`. Do đó, giới hạn rõ ràng là 26 chữ cái trong bảng chữ cái không liên quan đến kích thước đầu vào thực tế. 

Việc liệt kê trực tiếp các dãy con là vô vọng. Một chuỗi có độ dài 5000 có (2^{5000}) tập hợp con chỉ mục khác nhau. Ngay cả việc kiểm tra từng tập hợp con trong thời gian không đổi cũng đã là không thể và việc xây dựng dãy con tương ứng sẽ khiến công việc thậm chí còn lớn hơn. 

Ngoài ra còn có một số trường hợp ranh giới có thể âm thầm phá vỡ quá trình triển khai. Vì`a`, câu trả lời là`1`, bởi vì`f_1 = "a"`đã xảy ra một lần. Việc triển khai bắt đầu từ`f_2`sẽ in sai số 0. Vì`b`, câu trả lời là`0`, bởi vì không`f_i`chỉ chứa các chữ cái bắt đầu từ`b`, và mọi`f_i`chứa ít nhất một`a`. Việc triển khai đếm các chữ cái đơn lẻ tùy ý sẽ đếm không chính xác`b`. 

Vì`aba`, câu trả lời là`3`. Có hai trường hợp xảy ra`f_1 = "a"`và một lần xuất hiện`f_2 = "aba"`. Điều này nắm bắt các triển khai chỉ tính mẫu lớn nhất có thể. Vì`abcde`, câu trả lời là`1`: có một`a`, Nhưng`f_2`cần hai`a`nhân vật và`f_3`đã dài hơn toàn bộ chuỗi. Việc triển khai mà quên đi độ dài mẫu đang tăng nhanh có thể lãng phí công việc hoặc truy cập vào các trạng thái không hợp lệ. 

Hộp có kích thước tối đa`a`lặp lại 5000 lần mới có đáp án`5000`. Chỉ một`f_1`có thể xảy ra, vì mỗi lần lớn hơn`f_i`chứa một chữ cái trong bảng chữ cái khác. Đây cũng là một trường hợp căng thẳng hữu ích vì bản thân câu trả lời đủ nhỏ để có thể kiểm tra trực tiếp trong khi đầu vào có độ dài tối đa. 

## Phương pháp tiếp cận 

Phương pháp bạo lực trực tiếp nhất là liệt kê mọi tập hợp con các vị trí trong`s`, xây dựng chuỗi con tương ứng và kiểm tra xem nó có bằng một trong các chuỗi đặc biệt hay không. Điều này đúng vì mỗi dãy con được xác định duy nhất bởi các chỉ số được chọn từ`s`. Tuy nhiên, có (2^n) tập hợp con, vì vậy đối với`n = 5000`phương thức này có (2^{5000}) ứng viên. Nếu việc xây dựng hoặc so sánh một ứng cử viên mất (O(n)), thì kết quả trong trường hợp xấu nhất là (O(n2^n)), vượt xa giới hạn. 

Nỗ lực đầu tiên hợp lý hơn là xây dựng mọi`f_i`và đếm các dãy con của nó với dãy con chuẩn DP. Đối với một mẫu`p`chiều dài`m`, duy trì`dp[j]`, số cách lấy được số thứ nhất`j`nhân vật của`p`từ tiền tố được xử lý của`s`. Việc xử lý một ký tự đầu vào sẽ cập nhật tất cả các vị trí mẫu có chứa ký tự đó. Đây đã là đa thức, nhưng cách triển khai đơn giản sẽ quét tất cả`m`vị trí mẫu cho mỗi nhân vật. 

Quan sát hữu ích là các mô hình phát triển theo cấp số nhân. Bởi vì`f_i`có độ dài (2^i-1), chỉ cần xem xét khoảng (\log_2 n) mẫu và tổng độ dài của chúng chỉ là chính nó (O(n)). Chúng ta cũng có thể tính toán trước vị trí của từng ký tự bên trong mẫu hiện tại. Khi một nhân vật`x`từ`s`đến, chúng tôi chỉ cập nhật những vị trí mẫu có ký tự`x`, thay vì quét các vị trí không liên quan. 

Dãy con chuẩn DP có một yêu cầu tinh vi. Trạng thái mẫu phải được cập nhật từ phải sang trái. Nếu chúng tôi xử lý chúng từ trái sang phải, ký tự mới đọc có thể được sử dụng nhiều lần trong cùng một lần lặp, cho phép một vị trí của`s`để thể hiện nhiều ký tự của mẫu. Đảo ngược trật tự sẽ ngăn chặn điều đó. 

Đối với đầu vào lớn nhất có thể, tổng số vị trí mẫu trên tất cả các cấp độ liên quan là 

[ 
(2^1-1)+(2^2-1)+\cdots+(2^{12}-1)=8178. 
] 

Do đó, việc triển khai thực hiện tối đa (O(n\cdot 8178)) cập nhật cơ bản, tức là (O(n^2)) theo ràng buộc đã cho. Việc tối ưu hóa vị trí ký tự làm cho số lượng cập nhật thực tế nhỏ hơn đáng kể, đặc biệt khi đầu vào không chứa các chữ cái được yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n2^n)) | (O(n)) | Quá chậm | 
| DP tiêu chuẩn với tính năng lọc vị trí ký tự | (O(n^2)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`s`và để`n = len(s)`. Đếm xem mỗi chữ cái trong bảng chữ cái xuất hiện bao nhiêu lần`s`. Các tần số này cho phép chúng tôi dừng sớm khi một mẫu yêu cầu nhiều bản sao của một số ký tự hơn số lượng đầu vào chứa. 
2. Bắt đầu với`pattern = "a"`. Độ dài của nó là 1, vì vậy đây là mẫu đầu tiên có thể xảy ra. 
3. Trước khi đếm một mẫu, hãy xác minh rằng bội số ký tự được yêu cầu của nó có sẵn trong`s`. TRONG`f_i`, nhân vật`a`xảy ra (2^{i-1}) lần, ký tự`b`xảy ra (2^{i-2}) lần, v.v., trong khi ký tự mới nhất xuất hiện một lần. Nếu không có số lượng bắt buộc nào thì cũng không có mẫu nào lớn hơn có thể xảy ra, do đó vòng lặp có thể dừng lại. 
4. Xây dựng một mảng`positions`chứa các vị trí dựa trên một của mỗi ký tự bên trong mẫu hiện tại. Ví dụ, đối với`aba`, các vị trí cho`a`là`[1, 3]`, và vị trí của`b`là`[2]`. Điều này cho phép DP bỏ qua các vị trí mẫu có ký tự không thể khớp với ký tự đầu vào hiện tại. 
5. Khởi tạo dãy DP với`dp[0] = 1`. Điều này thể hiện mẫu trống, mẫu này luôn có thể được hình thành theo đúng một cách. Mọi trạng thái khác đều bắt đầu từ con số 0. 
6. Xử lý ký tự của`s`từ trái sang phải. Giả sử ký tự đầu vào hiện tại là`x`. Đảm nhận mọi vị trí của`x`trong mẫu theo thứ tự giảm dần. Đối với mỗi vị trí như vậy`j`, thực hiện 

[ 
dp[j] \mathrel{+}= dp[j-1]. 
] 

Nhà nước`dp[j-1]`mô tả các cách để hình thành đầu tiên`j-1`ký tự mẫu trước khi sử dụng ký tự đầu vào hiện tại làm ký tự mẫu`j`. 
7. Thêm`dp[len(pattern)]`để trả lời. Đây chính xác là số dãy con của`s`bằng hiện tại`f_i`. 
8. Xây dựng mẫu tiếp theo bằng cách sử dụng 

[ 
f_{i+1}=f_i+c_{i+1}+f_i. 
] 

Tiếp tục trong khi độ dài mẫu không vượt quá`n`Và`i <= 26`. 

Tại sao nó hoạt động: sau khi xử lý bất kỳ tiền tố nào của`s`,`dp[j]`là số cách chọn chỉ mục từ tiền tố đó có ký tự tạo thành đầu tiên`j`các ký tự của mẫu hiện tại. Cập nhật vị trí từ phải sang trái có nghĩa là ký tự hiện tại được sử dụng nhiều nhất một lần. Như vậy sau khi toàn bộ chuỗi đã được xử lý,`dp[len(pattern)]`đếm mọi lựa chọn chỉ mục tạo ra chính xác mẫu đó một lần. Chúng tôi thực hiện việc này một cách độc lập trong mọi trường hợp có thể`f_i`và mọi chuỗi con hợp lệ đều thuộc về chính xác một trong các chuỗi mục tiêu này vì độ dài của chúng khác nhau. Do đó, tổng hợp số lượng sẽ đưa ra câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def count_subsequences(s, pattern):
    m = len(pattern)

    positions = [[] for _ in range(26)]
    for j, ch in enumerate(pattern, 1):
        positions[ord(ch) - 97].append(j)

    dp = [0] * (m + 1)
    dp[0] = 1

    for ch in s:
        pos = positions[ord(ch) - 97]

        for j in reversed(pos):
            value = dp[j] + dp[j - 1]
            if value >= MOD:
                value -= MOD
            dp[j] = value

    return dp[m]

def solve(s):
    n = len(s)

    freq = [0] * 26
    for ch in s:
        freq[ord(ch) - 97] += 1

    answer = 0
    pattern = "a"

    for i in range(1, 27):
        length = len(pattern)
        if length > n:
            break

        feasible = True
        for j in range(i):
            # In f_i, character j appears 2^(i-j-1) times.
            need = 1 << (i - j - 1)
            if freq[j] < need:
                feasible = False
                break

        if not feasible:
            break

        answer += count_subsequences(s, pattern)
        if answer >= MOD:
            answer -= MOD

        if i == 26:
            break

        pattern = pattern + chr(97 + i) + pattern

    return answer

def main():
    s = input().strip()
    print(solve(s))

if __name__ == "__main__":
    main()
```các`count_subsequences`hàm thực hiện chuỗi con DP tiêu chuẩn cho một mẫu mục tiêu cố định.`dp[0]`được khởi tạo thành một vì có chính xác một cách để chọn một dãy con trống. Đối với mỗi ký tự đầu vào, chỉ các vị trí mẫu phù hợp mới được cập nhật. 

Các vị trí được lưu trữ dưới dạng các chỉ số dựa trên một vì`dp[j]`đương nhiên đại diện cho lần đầu tiên`j`các ký tự của mẫu. Việc lặp lại ngược lại là chi tiết triển khai quan trọng. Vì`pattern = "aa"`và ký tự đầu vào`a`, ví dụ: bản cập nhật trước tiên phải thay đổi`dp[2]`sử dụng cái cũ`dp[1]`, sau đó thay đổi`dp[1]`. Nếu không thì giống nhau`a`có thể đóng góp sai hai lần. 

Phép cộng mô-đun sử dụng một phép trừ có điều kiện duy nhất thay vì`% MOD`cho mỗi lần chuyển đổi. Cả hai toán hạng đều đã ở dưới mô đun, vì vậy tổng của chúng thấp hơn`2 * MOD`, thực hiện một phép trừ là đủ. 

Việc kiểm tra tính khả thi sử dụng cấu trúc đệ quy trực tiếp. Khi di chuyển từ`f_i`ĐẾN`f_{i+1}`, mỗi số ký tự hiện có sẽ tăng gấp đôi và ký tự mới xuất hiện một lần. Công thức`1 << (i - j - 1)`thể hiện chính xác sự đa dạng đó. Khi một mẫu không thể được nhúng do thiếu một số ký tự, mọi mẫu sau đó cũng không thể được nhúng, do đó vòng lặp bên ngoài có thể kết thúc. 

Bản thân mẫu không bao giờ được xây dựng vượt quá độ dài đầu vào. Vì kích thước của nó tăng gấp đôi sau mỗi lần lặp nên có nhiều nhất 12 mẫu phù hợp cho`n <= 5000`. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,`s = "abacaba"`, các mẫu có liên quan là`a`,`aba`, Và`abacaba`. Bảng sau đây hiển thị số lần xuất hiện hoàn thành của từng mẫu sau mỗi ký tự được xử lý. 

| Vị trí | Nhân vật |`f1 = a`|`f2 = aba`|`f3 = abacaba`| 
| --- | --- | --- | --- | --- | 
| 0 | trống | 0 | 0 | 0 | 
| 1 | một | 1 | 0 | 0 | 
| 2 | b | 1 | 0 | 0 | 
| 3 | một | 2 | 1 | 0 | 
| 4 | c | 2 | 1 | 0 | 
| 5 | một | 3 | 2 | 0 | 
| 6 | b | 3 | 2 | 0 | 
| 7 | một | 4 | 6 | 1 | 

Số đếm cuối cùng là`4`,`6`, Và`1`, cho`11`. Ví dụ, khi trận chung kết`a`được xử lý trong khi đếm`aba`, nó mở rộng mọi thứ hiện có`ab`tiếp theo. Có bốn cái như vậy`ab`dãy con, do đó tổng số`aba`dãy số trở thành sáu. 

Đối với mẫu thứ hai,`s = "b"`. Mẫu đầu tiên là`a`, nhưng ký tự đầu vào duy nhất là`b`, do đó DP của nó vẫn bằng không. 

| Vị trí | Nhân vật |`f1 = a`|`f2 = aba`| 
| --- | --- | --- | --- | 
| 0 | trống | 0 | 0 | 
| 1 | b | 0 | 0 | 

Vì thậm chí`f1`không xảy ra, các mẫu lớn hơn cũng không thể xảy ra. Câu trả lời là`0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^2)) | Tổng độ dài mẫu có liên quan là (O(n)) và mọi ký tự đầu vào chỉ xử lý các vị trí khớp. | 
| Không gian | (O(n)) | Mẫu hiện tại, danh sách vị trí ký tự và mảng DP đều có tổng kích thước (O(n)). | 

Vì`n <= 5000`, mục tiêu lớn nhất có thể có độ dài 4095 và chỉ có 12 chuỗi mục tiêu có liên quan. Sự tăng trưởng theo cấp số nhân của`f_i`là yếu tố giữ cho số lượng mẫu ở mức nhỏ, đồng thời tính năng lọc vị trí ký tự tránh dành thời gian cho các ký tự mẫu không thể khớp với ký tự đầu vào hiện tại. Giải pháp sử dụng bộ nhớ tuyến tính và phù hợp với giới hạn bộ nhớ 256 MB đã nêu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 998244353

def count_subsequences(s, pattern):
    m = len(pattern)

    positions = [[] for _ in range(26)]
    for j, ch in enumerate(pattern, 1):
        positions[ord(ch) - 97].append(j)

    dp = [0] * (m + 1)
    dp[0] = 1

    for ch in s:
        for j in reversed(positions[ord(ch) - 97]):
            value = dp[j] + dp[j - 1]
            if value >= MOD:
                value -= MOD
            dp[j] = value

    return dp[m]

def solve(s):
    n = len(s)

    freq = [0] * 26
    for ch in s:
        freq[ord(ch) - 97] += 1

    answer = 0
    pattern = "a"

    for i in range(1, 27):
        if len(pattern) > n:
            break

        for j in range(i):
            if freq[j] < (1 << (i - j - 1)):
                return answer

        answer = (answer + count_subsequences(s, pattern)) % MOD

        if i == 26:
            break

        pattern = pattern + chr(97 + i) + pattern

    return answer

def run(inp: str) -> str:
    return str(solve(inp.strip()))

# provided samples
assert run("abacaba") == "11", "sample 1"
assert run("b") == "0", "sample 2"

# minimum-size input
assert run("a") == "1", "single-character input"

# f2 occurs once, and f1 occurs twice
assert run("aba") == "3", "basic recursive pattern"

# repeated pattern, four occurrences of aba and three occurrences of a
assert run("ababa") == "7", "multiple f2 occurrences"

# f2 already cannot occur because there are not two a's
assert run("abcde") == "1", "pattern-length and frequency boundary"

# maximum input size, only f1 can occur
assert run("a" * 5000) == "5000", "maximum-size all-equal input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`1`| Kích thước tối thiểu và`f1`ranh giới | 
|`aba`|`3`| Đếm đồng thời`f1`Và`f2`| 
|`ababa`|`7`| Nhiều dãy con của cùng một mẫu đệ quy | 
|`abcde`|`1`| Số lượng ký tự không đủ và các mẫu dài hơn đầu vào | 
|`a`lặp lại 5000 lần |`5000`| Kích thước đầu vào tối đa và các ký tự hoàn toàn bằng nhau | 

## Vỏ cạnh 

cho`s = "a"`, thuật toán bắt đầu bằng`f1 = "a"`. Việc kiểm tra tần số thành công vì có một`a`. DP thay đổi từ`[1, 0]`ĐẾN`[1, 1]`, vậy phần đóng góp là`1`. Mẫu tiếp theo có chiều dài bằng ba và không vừa, đưa ra câu trả lời cuối cùng`1`. 

Vì`s = "b"`, việc kiểm tra tần số`f1`thất bại ngay lập tức vì`s`chứa số không`a`nhân vật. Câu trả lời vẫn còn`0`và thuật toán không cố gắng xử lý bất kỳ mẫu nào lớn hơn. 

Vì`s = "aba"`, đếm`f1`đưa ra hai lần xuất hiện. Khi đếm`f2 = "aba"`, lựa chọn chỉ mục hợp lệ duy nhất là`(1, 2, 3)`, vậy đóng góp thứ hai là một. Tổng cộng là`2 + 1 = 3`. 

Vì`s = "abcde"`,`f1`xảy ra một lần. Mẫu`f2 = "aba"`cần hai bản sao của`a`, nhưng đầu vào chỉ chứa một. Kiểm tra tần số phát hiện điều này trước khi chạy DP cho`f2`, và tất cả các mẫu sau này cũng không thể thực hiện được. Câu trả lời là chính xác`1`. 

Để có đầu vào tối đa`s = "a" * 5000`,`f1`xảy ra ở mọi vị trí, mang lại`5000`. Mẫu thứ hai yêu cầu`b`, không có nên quá trình kiểm tra tính khả thi sẽ dừng ngay lập tức. Không có DP lớn nào được thực hiện và câu trả lời vẫn là`5000`.
