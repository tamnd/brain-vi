---
title: "CF 102330G - \u0421\u0430\u0448\u0430 \u0438 \u0441\u0442\u0430\u0436\u0438\u0440\u043e\u0432\u043a\u0438"
description: "Chúng ta được cấp một số nhiệm vụ được viết dưới dạng một chuỗi các chữ số thập phân. Việc phân chia chọn một số vị trí cắt giữa các chữ số, do đó mỗi phần kết quả được hiểu là một số nhiệm vụ riêng biệt."
date: "2026-08-13T04:06:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102330
codeforces_index: "G"
codeforces_contest_name: "\u0421\u0438\u0440\u0438\u0443\u0441.2019.\u041d\u043e\u044f\u0431\u0440\u044c.\u041e\u0447\u043d\u044b\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 102330
solve_time_s: 80
verified: true
draft: false
---

[CF 102330G - \u0421\u0430\u0448\u0430 \u0438 \u0441\u0442\u0430\u0436\u0438\u0440\u043e\u0432\u043a\u0438](https://codeforces.com/problemset/problem/102330/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số nhiệm vụ được viết dưới dạng một chuỗi các chữ số thập phân. Việc phân chia chọn một số vị trí cắt giữa các chữ số, do đó mỗi phần kết quả được hiểu là một số nhiệm vụ riêng biệt. Một mảnh hợp lệ phải thỏa mãn hai điều kiện: biểu diễn thập phân của nó có nhiều nhất`k`chữ số và giá trị số của nó là số nguyên tố. Các số 0 đứng đầu bị cấm, vì vậy một đoạn như`03`không hợp lệ ngay cả khi giá trị số của nó là 3. 

Mục tiêu là đếm mọi cách có thể để đặt các vết cắt sao cho tất cả các mảnh kết quả đều hợp lệ. Các vị trí cắt khác nhau tạo ra các phân tách khác nhau, ngay cả khi một số phần có cùng giá trị số. Câu trả lời được in theo modulo`10^9 + 7`. 

Giới hạn đầu vào`|n| <= 10^6`là hạn chế bất thường đối với bài toán phân vùng chữ số. Giá trị tuyệt đối có nhiều nhất bảy chữ số thập phân, trong khi`k <= 6`. Điều đó có nghĩa là có nhiều nhất sáu vị trí cắt có thể xảy ra và do đó có nhiều nhất`2^6 = 64`phân vùng hoàn chỉnh. Một giải pháp bạo lực đã đủ nhỏ để giải quyết những hạn chế này. Tuy nhiên, phép truy toán tương tự có thể được viết dưới dạng một chương trình động giúp tránh việc tính toán lại các trạng thái tiền tố giống nhau và chia tỷ lệ tốt hơn nhiều nếu độ dài chữ số được tăng lên. 

Giá trị tuyệt đối được sử dụng khi xây dựng chuỗi chữ số. Dấu trừ không phải là một chữ số và không thể tham gia vào số nhiệm vụ thập phân nên cách biểu diễn có ý nghĩa cho việc chia là dãy các chữ số của`|n|`. 

Có một số trường hợp nhỏ mà việc triển khai bất cẩn có thể thất bại. Đối với đầu vào`2`với`k = 1`, câu trả lời là`1`, vì toàn bộ số tác vụ là số nguyên tố 2. Việc triển khai bắt đầu kiểm tra tính nguyên tố từ 2 và vô tình yêu cầu ước số có thể từ chối trường hợp này. 

Đối với đầu vào`11`với`k = 1`, câu trả lời là`0`. Mỗi phần có một chữ số là`1`, không phải là số nguyên tố. Một giải pháp coi mọi chữ số ngoại trừ số 0 là số nguyên tố có thể sẽ trả về không chính xác`1`. 

Đối với đầu vào`103`với`k = 2`, câu trả lời là`0`. Tiền tố có hai chữ số duy nhất có thể là`10`, còn chữ số còn lại là`3`;`10`không phải là nguyên tố. Sự thay thế`1|03`bị cấm vì`03`có số 0 đứng đầu. Một giải pháp chuyển đổi trực tiếp mọi chuỗi con thành số nguyên trước khi kiểm tra nó sẽ âm thầm chấp nhận cách biểu diễn không hợp lệ này. 

Đối với đầu vào`23`với`k = 2`, câu trả lời là`2`. Cả hai`23`chính nó và`2|3`là hợp lệ. Một giải pháp chỉ xem xét các phần có độ dài chính xác`k`hoặc quên rằng phần cuối cùng ngắn hơn được cho phép, bỏ lỡ một trong những phân tách này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi mức cắt giảm có thể có. Với`L`có chữ số`L - 1`khoảng trống và mọi khoảng trống có thể chứa vết cắt hoặc không, mang lại`2^(L-1)`phân vùng. Đối với mỗi phân vùng, chúng tôi kiểm tra từng mảnh được sản xuất, loại bỏ các mảnh dài hơn`k`, loại bỏ các số 0 đứng đầu và kiểm tra tính nguyên tố của các giá trị còn lại. Điều này đúng vì mọi phân tách có thể tương ứng với chính xác một tập con của các khoảng trống, do đó phép liệt kê không bỏ sót hoặc trùng lặp một phân tách. 

Dưới sự ràng buộc thực tế`L <= 7`, lực lượng vũ phu này không bao giờ trở nên quá chậm. Có nhiều nhất 64 phân vùng hoàn chỉnh và mỗi phân vùng có tối đa bảy phần, do đó có nhiều nhất 448 phần kiểm tra. Vì mỗi số được kiểm tra có nhiều nhất sáu chữ số nên phép chia thử cần ít hơn 1000 phép thử ước số cho mỗi phần. Giới hạn trên của trường hợp xấu nhất thu được là khoảng 448.000 phép kiểm ước số cơ bản, có thể dễ dàng quản lý được. Do đó, phương pháp brute-force đã được chấp nhận cho vấn đề này. 

Cách tiếp cận có thể tái sử dụng nhiều hơn là quan sát thấy rằng một phép phân rã có thể được xây dựng từ trái sang phải. Giả sử lần đầu tiên`i`các chữ số đã được chia thành các phần nguyên tố hợp lệ. Chúng ta chỉ cần quyết định phần hợp lệ nào sẽ đến tiếp theo. Có nhiều nhất`k`độ dài có thể có cho đoạn tiếp theo đó, vì vậy chúng ta có thể chuyển từ vị trí`i`đến vị trí`i + 1`bởi vì`i + k`. Điều này mang lại một chương trình động một chiều. 

Cho phép`dp[i]`là số phân rã hợp lệ của phân số đầu tiên`i`chữ số. Ban đầu`dp[0] = 1`, đại diện cho tiền tố trống. Đối với mọi vị trí có thể tiếp cận`i`, chúng tôi lấy một, hai, lên tới`k`chữ số. Nếu chuỗi con kết quả không có số 0 đứng đầu và đại diện cho một số nguyên tố thì nó sẽ đóng góp`dp[i]`cách để`dp[j]`, Ở đâu`j`là vị trí ngay sau chuỗi con đó. 

Phương pháp brute-force hoạt động vì số lượng vị trí cắt rất nhỏ nhưng nó liên tục khám phá các tiền tố hợp lệ giống nhau. Quan sát cho thấy mọi phân tách là một chuỗi các chuyển đổi hợp lệ giữa các vị trí chữ số cho phép chúng ta hợp nhất tất cả các tiền tố lặp lại đó thành một trạng thái DP duy nhất. 

Kiểm tra tính nguyên thủy ở đây không cần sàng lớn. Có nhiều nhất`7 * 6 = 42`các ứng cử viên chuỗi con khác nhau được DP xem xét và mỗi ứng cử viên có tối đa sáu chữ số. Phép chia thử nghiệm đến căn bậc hai là quá đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(2^L * L * sqrt(10^k))`|`O(L)`| Được chấp nhận cho`L <= 7`| 
| Tối ưu |`O(L * k * sqrt(10^k))`|`O(L)`| Đã chấp nhận | 

Đây`L`là số chữ số trong`|n|`. DP được ưa chuộng hơn vì cấu trúc của nó vẫn hữu ích nếu độ dài đầu vào tăng lên trong khi việc triển khai vẫn còn rất nhỏ. 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`k`, và chuyển đổi`abs(n)`đến một chuỗi`s`. Việc biểu diễn chuỗi là cần thiết vì việc phân tách được thực hiện giữa các chữ số thập phân, không theo số học. 
2. Xác định phép kiểm tra tính nguyên tố cho số nguyên dương. Các giá trị nhỏ hơn 2 bị từ chối. Đối với giá trị ít nhất là 2, hãy kiểm tra khả năng chia hết cho các số nguyên đến căn bậc hai của nó. Nếu không tìm thấy ước số, giá trị đó là số nguyên tố. 
3. Tạo một mảng`dp`chiều dài`len(s) + 1`và thiết lập`dp[0] = 1`. Nhà nước`dp[i]`có nghĩa là lần đầu tiên`i`các chữ số đã có thể được chia hoàn toàn thành các phần hợp lệ trong`dp[i]`những cách khác nhau. 
4. Xử lý các vị trí từ trái qua phải. Nếu như`dp[i]`bằng 0, không có phân tách hợp lệ nào đạt đến vị trí này, do đó không có gì để mở rộng từ nó. 
5. Từ vị trí`i`, thử từng đoạn có độ dài từ 1 đến`k`, dừng ở cuối chuỗi. Điều này bao gồm mọi phần tiếp theo có thể được cho phép bởi giới hạn độ dài tối đa. 
6. Từ chối ứng viên ngay lập tức khi chữ số đầu tiên của ứng viên đó là`0`. Chuỗi con như vậy sẽ có số 0 đứng đầu và không thể biểu thị số tác vụ hợp lệ. 
7. Chuyển chuỗi con ứng viên thành số nguyên và kiểm tra xem nó có phải là số nguyên tố hay không. Nếu là số nguyên tố thì thêm`dp[i]`đến trạng thái tương ứng với sự kết thúc của ứng cử viên. Quá trình chuyển đổi này có nghĩa là mọi phân tách hợp lệ của tiền tố đều có thể được mở rộng bằng phần nguyên tố cụ thể này. 
8. Sau khi tất cả các vị trí đã được xử lý, hãy quay lại`dp[len(s)]`. Trạng thái này đếm chính xác các phân tách tiêu thụ từng chữ số, do đó không có tiền tố chưa hoàn thành nào được đưa vào câu trả lời. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý vị trí`i`,`dp[i]`chứa chính xác số lượng phân tách hợp lệ của phân tách đầu tiên`i`chữ số. Mỗi sự phân rã như vậy đều có một phần cuối cùng duy nhất. Việc loại bỏ phần cuối cùng đó sẽ để lại sự phân rã hợp lệ của một số vị trí trước đó`j`và phần bị loại bỏ chính xác là một trong những chuỗi con được xem xét khi chuyển đổi từ`j`ĐẾN`i`. Ngược lại, mọi chuyển đổi được thuật toán chấp nhận sẽ gắn thêm một phần nguyên tố hợp lệ vào một phân tách đã hợp lệ. Do đó, mọi phân tách được đếm đều hợp lệ và mọi phân tách hợp lệ được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1_000_000_007

def is_prime(x):
    if x < 2:
        return False
    if x == 2:
        return True
    if x % 2 == 0:
        return False

    d = 3
    while d * d <= x:
        if x % d == 0:
            return False
        d += 2
    return True

def solve():
    n = int(input())
    k = int(input())

    s = str(abs(n))
    m = len(s)

    dp = [0] * (m + 1)
    dp[0] = 1

    for i in range(m):
        if dp[i] == 0:
            continue

        value = 0

        for length in range(1, k + 1):
            j = i + length
            if j > m:
                break

            if length > 1 and s[i] == '0':
                break

            value = value * 10 + (ord(s[j - 1]) - ord('0'))

            if is_prime(value):
                dp[j] = (dp[j] + dp[i]) % MOD

    print(dp[m])

if __name__ == "__main__":
    solve()
```các`is_prime`tay cầm chức năng`0`Và`1`trước khi kiểm tra khả năng chia hết và xử lý`2`riêng biệt để phím tắt số chẵn không loại bỏ số nguyên tố nhỏ nhất. Sau đó, chỉ cần xem xét các ước số lẻ. 

DP sử dụng`dp[0] = 1`như trường hợp cơ sở có tiền tố trống. Nếu không có giá trị này, phần đầu tiên hợp lệ sẽ không có trạng thái để nhận phần đóng góp của nó. 

Giá trị ứng cử viên được xây dựng tăng dần thay vì chuyển đổi liên tục các chuỗi con. Ví dụ, trong khi mở rộng từ vị trí`i`, các giá trị`3`,`37`, Và`377`lần lượt thu được. Điều này vừa đơn giản hơn cho vòng lặp chuyển tiếp vừa tránh được việc cắt chuỗi không cần thiết. 

Việc kiểm tra số 0 đứng đầu được thực hiện có chủ ý trước khi chấp nhận một ứng cử viên có nhiều chữ số. Một chữ số`0`cũng bị từ chối một cách tự nhiên bởi`is_prime`, Vì thế`0`không cần có trường hợp đặc biệt riêng. Nếu như`s[i] == '0'`, không còn chuỗi con bắt đầu từ đó có thể hợp lệ vì mọi chuỗi con dài hơn vẫn sẽ có số 0 đứng đầu. Đó là lý do tại sao vòng lặp có thể dừng ngay lập tức. 

Ứng cử viên dài nhất có nhiều nhất sáu chữ số, vì vậy`value`nhiều nhất là`999999`. Các số nguyên Python không có vấn đề tràn ở đây và phép toán modulo trên mỗi chuyển đổi DP tuân theo quy ước đầu ra bắt buộc mặc dù số lượng phân tách thực tế nhiều nhất là 64 cho vấn đề này. 

## Ví dụ đã hoạt động 

Đối với mẫu được cung cấp, đầu vào là`37735`với`k = 2`. DP có thể chọn một hoặc hai chữ số ở mọi vị trí, tùy theo tính nguyên thủy. 

| Chức vụ`i`|`dp[i]`| Ứng viên | Xuất sắc? | Trạng thái cập nhật | 
| --- | --- | --- | --- | --- | 
| 0 | 1 |`3`| Có |`dp[1] += 1`| 
| 0 | 1 |`37`| Có |`dp[2] += 1`| 
| 1 | 1 |`7`| Có |`dp[2] += 1`| 
| 1 | 1 |`77`| Không | không | 
| 2 | 2 |`7`| Có |`dp[3] += 2`| 
| 2 | 2 |`73`| Có |`dp[4] += 2`| 
| 3 | 2 |`3`| Có |`dp[4] += 2`| 
| 3 | 2 |`35`| Không | không | 
| 4 | 4 |`5`| Có |`dp[5] += 4`| 

Giá trị cuối cùng là`dp[5] = 4`. Bốn con đường này tương ứng với`3|7|7|3|5`,`37|7|3|5`,`37|73|5`, Và`3|7|73|5`. Dấu vết cho thấy tại sao nhiều cách có thể đạt đến cùng một vị trí và tại sao những cách đó phải được tích lũy chứ không phải thay thế. 

Đối với ví dụ thứ hai, hãy xem xét`23`với`k = 2`. 

| Chức vụ`i`|`dp[i]`| Ứng viên | Xuất sắc? | Trạng thái cập nhật | 
| --- | --- | --- | --- | --- | 
| 0 | 1 |`2`| Có |`dp[1] = 1`| 
| 0 | 1 |`23`| Có |`dp[2] = 1`| 
| 1 | 1 |`3`| Có |`dp[2] = 2`| 

Câu trả lời là`2`, đại diện`2|3`Và`23`. Ví dụ chứng minh rằng quá trình chuyển đổi phải xem xét mọi độ dài từ 1 đến`k`, kể cả trường hợp toàn bộ hậu tố còn lại tạo thành một phần nguyên tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(L * k * sqrt(10^k))`| Nhiều nhất`k`các phần ứng cử viên được kiểm tra từ mỗi`L`vị trí và việc kiểm tra tính nguyên thủy cần có thời gian căn bậc hai | 
| Không gian |`O(L)`| Mảng DP chứa một trạng thái cho mỗi vị trí chữ số | 

Đây`L <= 7`Và`k <= 6`. Ngay cả giới hạn lỏng lẻo cũng chỉ đưa ra 42 bài kiểm tra tính nguyên tố, với mỗi bài kiểm tra liên quan đến tối đa khoảng 500 ước số lẻ cho một số có sáu chữ số. Thuật toán thoải mái trong giới hạn một giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 1_000_000_007

def is_prime(x):
    if x < 2:
        return False
    if x == 2:
        return True
    if x % 2 == 0:
        return False

    d = 3
    while d * d <= x:
        if x % d == 0:
            return False
        d += 2
    return True

def solution(inp):
    data = inp.split()
    n = int(data[0])
    k = int(data[1])

    s = str(abs(n))
    m = len(s)

    dp = [0] * (m + 1)
    dp[0] = 1

    for i in range(m):
        if dp[i] == 0:
            continue

        value = 0

        for length in range(1, k + 1):
            j = i + length
            if j > m:
                break

            if length > 1 and s[i] == '0':
                break

            value = value * 10 + int(s[j - 1])

            if is_prime(value):
                dp[j] = (dp[j] + dp[i]) % MOD

    return str(dp[m])

def run(inp: str) -> str:
    return solution(inp).strip()

# Provided sample
assert run("37735\n2\n") == "4", "provided sample"

# Minimum-size prime task number
assert run("2\n1\n") == "1", "single-digit prime"

# All equal digits, every digit is prime but every two-digit block is composite
assert run("777777\n2\n") == "1", "all-equal values"

# Both the whole number and the one-digit split are valid
assert run("23\n2\n") == "2", "boundary on k"

# Leading zero must not be accepted as a multi-digit prime
assert run("103\n2\n") == "0", "leading zero"

# Maximum absolute value allowed by the statement
assert run("1000000\n6\n") == "0", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1`|`1`| Số nguyên tố nhỏ nhất có thể và`2`ranh giới nguyên tố | 
|`777777 / 2`|`1`| Các chữ số lặp đi lặp lại và sự phân biệt giữa`7`và tổng hợp`77`| 
|`23 / 2`|`2`| Cả cách chia một chữ số và độ dài mảnh tối đa được phép | 
|`103 / 2`|`0`| Từ chối số 0 hàng đầu và các phần có nhiều chữ số tổng hợp | 
|`1000000 / 6`|`0`| Đầu vào tuyệt đối tối đa được phép và nhiều ứng cử viên chứa số 0 | 

## Vỏ cạnh 

cho`2`với`k = 1`, DP bắt đầu bằng`dp[0] = 1`. Nó hình thành ứng cử viên`2`, Và`is_prime(2)`trả về đúng, vì vậy`dp[1]`trở thành 1. Câu trả lời cuối cùng là`1`. Việc xử lý đặc biệt 2 trong kiểm tra tính nguyên tố sẽ ngăn chặn việc loại bỏ không chính xác dưới dạng số chẵn. 

Vì`11`với`k = 1`, ứng cử viên duy nhất là hai chữ số riêng lẻ`1`Và`1`. Cả hai đều thất bại`x < 2`tình trạng trong`is_prime`, do đó không có chuyển đổi nào đạt đến vị trí 1 hoặc vị trí 2. Câu trả lời cuối cùng là`0`. 

Vì`103`với`k = 2`, vị trí đầu tiên cho phép`1`Và`10`, cả hai đều không phải là số nguyên tố. Quá trình chuyển đổi từ vị trí 1 sẽ bắt đầu ở chữ số`0`, nhưng vì nó là ứng cử viên có nhiều chữ số nên việc kiểm tra số 0 đứng đầu sẽ dừng vòng lặp ngay lập tức. Số đầy đủ không thể được sử dụng vì độ dài của nó là ba trong khi`k = 2`. Câu trả lời là do đó`0`. 

Vì`23`với`k = 2`, vị trí đầu tiên tạo ra cả hai`2`Và`23`. Sự chuyển đổi đầu tiên mang lại`dp[1] = 1`, trong khi cái thứ hai cho`dp[2] = 1`. Từ vị trí 1, phần còn lại`3`là số nguyên tố nên có một cách khác để đạt được`dp[2]`. Trạng thái cuối cùng chứa`2`, khớp chính xác với hai phân tách hợp lệ. 

Vì`1000000`với`k = 6`, mọi ứng cử viên bắt đầu từ chữ số đầu tiên đều là`1`,`10`,`100`, vân vân. Chỉ một`1`có thể là tiền tố của một phân tách hợp lệ, nhưng nó không phải là số nguyên tố. Khi DP đạt đến chữ số 0 thì không có phần có nhiều chữ số hợp lệ nào bắt đầu ở đó và bản thân số 0 không phải là số nguyên tố. Không có sự chuyển tiếp nào đạt đến vị trí cuối cùng nên đáp án là`0`.
