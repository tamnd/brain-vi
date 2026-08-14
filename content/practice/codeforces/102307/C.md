---
title: "CF 102307C - Dãy số chung"
description: "Chúng ta có hai chuỗi DNA A và B, cả hai đều có độ dài n. Các ký tự duy nhất có thể xuất hiện là A, T, G và C. Chúng ta không cần xây dựng dãy con chung. Chúng ta chỉ cần quyết định xem độ dài tối đa có thể của nó ít nhất là 0,99n hay không."
date: "2026-08-13T23:39:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102307
codeforces_index: "C"
codeforces_contest_name: "2019 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102307
solve_time_s: 155
verified: true
draft: false
---

[CF 102307C - Dãy số chung](https://codeforces.com/problemset/problem/102307/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai chuỗi DNA`A`Và`B`, cả hai đều có chiều dài`n`. Các nhân vật duy nhất có thể xuất hiện là`A`,`T`,`G`, Và`C`. Ta không cần xây dựng dãy con chung. Chúng ta chỉ cần quyết định xem độ dài tối đa có thể của nó ít nhất là`0.99n`. 

Một cách hữu ích để đọc điều kiện là đếm những gì có thể bị thiếu trong dãy con chung. Nếu chiều dài của nó là`L`, thì mỗi chuỗi gốc có`n - L`các ký tự không được sử dụng bởi dãy con chung đó. Yêu cầu`L >= 0.99n`tương đương với`n - L <= 0.01n`. 

Từ`n - L`là số nguyên nên ta được phép thua nhiều nhất`floor(n / 100)`các ký tự từ mỗi chuỗi. Cho phép`k = floor(n / 100)`. 

Độ dài LCS mục tiêu sau đó chính xác là`n - k`. Nếu chúng ta có thể tìm được một dãy con chung có độ dài đó thì câu trả lời là khẳng định. 

Chiều dài giới hạn của`10^5`loại trừ những điều bình thường`O(n^2)`Lập trình động LCS. Ở kích thước tối đa có nghĩa là khoảng`10^10`Các tế bào DP, vượt xa giới hạn một giây có thể xử lý. Tham số hữu ích không phải là toàn bộ chiều dài của LCS, gần như`n`, nhưng số lượng nhỏ`k`các ký tự mà chúng ta được phép loại bỏ. Tại`n = 10^5`,`k`chỉ là`1000`. 

Có một số trường hợp ranh giới rất dễ bị xử lý sai. Khi`n < 100`, chúng tôi có`k = 0`, do đó không có ký tự nào có thể bị loại bỏ. Ví dụ,```
A
T
```có độ dài LCS`0`, trong khi độ dài cần thiết là`1`, vì vậy đầu ra đúng là`Not brothers :(`. Một giải pháp vô tình làm tròn`0.99n`down sẽ chấp nhận nó một cách không chính xác. 

Ngưỡng chính xác cũng quan trọng khi`n`là bội số của`100`. Ví dụ,```
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAATA
```có chiều dài`100`, và LCS của nó là`99`. Từ`99 = 0.99 * 100`, câu trả lời đúng là`Long lost brothers D:`. Yêu cầu độ dài lớn hơn`0.99n`sẽ từ chối một trường hợp hợp lệ. 

Thêm một trường hợp ranh giới nữa là nằm ngay dưới ngưỡng. Với`n = 100`, nếu LCS là`98`, hai ký tự phải bị loại bỏ khỏi mỗi chuỗi, vốn đã quá nhiều rồi. Việc triển khai bất cẩn để kiểm tra xem số lượng không khớp có nhiều nhất hay không sẽ gây nhầm lẫn giữa sự không khớp về vị trí với việc xóa và có thể thất bại đối với các kết quả khớp bị dịch chuyển. 

## Phương pháp tiếp cận 

Giải pháp đơn giản là lập trình động LCS tiêu chuẩn. Định nghĩa`lcs[i][j]`là dãy con chung dài nhất của dãy đầu tiên`i`nhân vật của`A`và lần đầu tiên`j`nhân vật của`B`. Nếu các ký tự hiện tại khớp nhau, chúng ta có thể mở rộng trạng thái đường chéo. Mặt khác, chúng tôi thu được kết quả tốt hơn bằng cách bỏ qua một ký tự từ một trong hai chuỗi. Điều này đúng vì mọi dãy con chung đều sử dụng cặp ký tự hiện tại hoặc bỏ qua ít nhất một trong số chúng. 

Vấn đề là kích thước của bảng. có`(n + 1)^2`tiểu bang, vì vậy đối với`n = 10^5`chúng tôi nhận được đại khái`10^10`hoạt động. Yêu cầu bộ nhớ cũng sẽ là bậc hai nếu toàn bộ bảng được lưu trữ. 

Quan sát quan trọng là một dãy con chung có thể chấp nhận được chỉ xóa`k = floor(n / 100)`ký tự từ một trong hai chuỗi. Thay vì yêu cầu LCS trên tất cả các tiền tố có thể có, chúng ta có thể mô tả trạng thái bằng số lượng ký tự đã bị xóa khỏi mỗi chuỗi. 

Cho phép`dp[i][j]`đại diện cho số lượng ký tự lớn nhất có thể khớp sau khi xóa`i`nhân vật từ`A`Và`j`nhân vật từ`B`. Giả sử trạng thái hiện có chứa`p = dp[i][j]`các ký tự trùng khớp. Các ký tự trùng khớp chiếm vị trí đầu tiên`i + p`vị trí của`A`và lần đầu tiên`j + p`vị trí của`B`. Do đó, các ký tự tiếp theo để so sánh là`A[i + p]`Và`B[j + p]`. 

Nếu chúng bằng nhau thì không có lý do gì để ngừng kết hợp chúng. Chúng ta có thể mở rộng dãy con chung ngay lập tức và tiếp tục trong khi cặp tiếp theo bằng nhau. Nếu chúng khác nhau thì bất kỳ dãy con chung nào tiếp tục từ trạng thái này đều phải loại bỏ ký tự tiếp theo của`A`hoặc ký tự tiếp theo của`B`. Đó chính xác là hai sự chuyển tiếp`dp[i + 1][j] = max(dp[i + 1][j], dp[i][j])`Và`dp[i][j + 1] = max(dp[i][j + 1], dp[i][j])`. 

Điều này biến bảng LCS được lập chỉ mục tiền tố lớn thành một bảng chỉ được lập chỉ mục bằng số lần xóa. chỉ có`(k + 1)^2`các trạng thái như vậy và pha khớp sẽ di chuyển trực tiếp qua các lần chạy bằng nhau thay vì xử lý từng cặp vị trí chuỗi. 

Giải pháp brute-force hoạt động vì mọi cách bỏ qua ký tự có thể được biểu thị bằng phép lặp LCS thông thường, nhưng nó không thành công vì nó coi việc xóa được tính đến tận`n`. Quan sát rằng một câu trả lời được chấp nhận chỉ có thể xóa`1%`của các ký tự cho phép chúng tôi chỉ giữ lại ranh giới xóa nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n^2)`|`O(n^2)`| Quá chậm | 
| Tối ưu |`O(nk)`khấu hao ở đâu`k = floor(n/100)`|`O(k^2)`| Đã chấp nhận | 

Vì`n = 10^5`, tham số hữu ích là`k <= 1000`, do đó DP hoạt động trên khoảng một triệu trạng thái xóa thay vì mười tỷ cặp tiền tố. Số lần vận hành phù hợp được khấu hao theo giới hạn, tạo ra giới hạn cần thiết cho phương pháp này. 

## Hướng dẫn thuật toán 

1. Đọc hai chuỗi DNA và cho`n`là độ dài chung của chúng. Tính toán`k = n // 100`, bởi vì một dãy con chung được chấp nhận có thể bỏ qua tối đa bấy nhiêu ký tự trong một trong hai chuỗi. 
2. Tính độ dài dãy con chung cần thiết là`target = n - k`. Dạng số nguyên này tránh so sánh dấu phẩy động với`0.99`. 
3. Nếu`k`bằng 0, không được phép xóa. Dãy con chung duy nhất có thể có của độ dài`n`là toàn bộ chuỗi, vì vậy câu trả lời đơn giản là liệu`A`Và`B`giống hệt nhau. 
4. Tạo bảng DP được lập chỉ mục theo số lần xóa. Ban đầu`dp[0][0] = 0`, nghĩa là trước khi xóa bất cứ thứ gì, chưa có ký tự nào được khớp. 
5. Trạng thái tiến trình`(i, j)`theo thứ tự tăng dần. Tại một trạng thái, hãy`p = dp[i][j]`. đầu tiên`i`ký tự bị xóa khỏi`A`và lần đầu tiên`j`ký tự bị xóa khỏi`B`, cùng với`p`các ký tự trùng khớp, đặt hai vị trí hiện tại tại`i + p`Và`j + p`. 
6. Trong khi cả hai vị trí đều nằm trong chuỗi của chúng và các ký tự bằng nhau, hãy tăng`p`. Việc so khớp các ký tự bằng nhau này một cách tham lam là an toàn vì chúng là các ký tự tiếp theo ngay lập tức ở cả hai bên, do đó, việc giữ chúng không bị mất chi phí và chỉ có thể di chuyển chuỗi con chung về phía trước. 
7. Lưu lại giá trị mở rộng vào`dp[i][j]`. Nếu nó đã đạt tới`target`, một dãy con chung chấp nhận được đã tìm được và ta có thể chấp nhận ngay. 
8. Nếu`i < k`, truyền bá giá trị hiện tại tới`dp[i + 1][j]`. Điều này thể hiện việc xóa ký tự chưa từng có tiếp theo của`A`. Nếu như`j < k`, truyền bá nó tới`dp[i][j + 1]`, đại diện cho việc xóa ký tự chưa từng có tiếp theo của`B`. Chúng tôi không bao giờ cần các trạng thái ngoài`k`, bởi vì một giải pháp sử dụng nhiều hơn`k`việc xóa không thể đáp ứng độ dài LCS cần thiết. 

Tính bất biến đó là`dp[i][j]`lưu trữ điểm xa nhất đạt được bằng kết quả khớp hợp lệ đã sử dụng nhiều nhất`i`xóa từ`A`và nhiều nhất`j`xóa từ`B`. Khi các ký tự tiếp theo khớp, việc mở rộng khớp luôn là tối ưu cho trạng thái đó. Khi chúng không khớp, mọi phần tiếp theo có thể phải bỏ qua ít nhất một trong hai ký tự đó và hai phần chuyển tiếp liệt kê chính xác những khả năng đó. Do đó, mọi dãy con chung sử dụng tối đa`k`việc xóa được biểu thị bằng một số đường dẫn DP và mọi đường dẫn DP tương ứng với một chuỗi chung hợp lệ. Tiếp cận`n - k`do đó các ký tự trùng khớp tương đương với việc thỏa mãn điều kiện ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

YES = "Long lost brothers D:"
NO = "Not brothers :("

def is_brothers(a: str, b: str) -> bool:
    n = len(a)
    k = n // 100
    target = n - k

    # If k == 0, we need an LCS of length n.
    if k == 0:
        return a == b

    # dp[i][j] = furthest number of characters matched after
    # deleting i characters from a and j characters from b.
    size = k + 2
    dp = [[0] * size for _ in range(size)]

    for i in range(k + 1):
        row = dp[i]

        for j in range(k + 1):
            p = row[j]

            x = i + p
            y = j + p

            while x < n and y < n and a[x] == b[y]:
                p += 1
                x += 1
                y += 1

            row[j] = p

            if p >= target:
                return True

            if i < k and p > dp[i + 1][j]:
                dp[i + 1][j] = p

            if j < k and p > row[j + 1]:
                row[j + 1] = p

    return False

def main() -> None:
    a = input().strip()
    b = input().strip()

    print(YES if is_brothers(a, b) else NO)

if __name__ == "__main__":
    main()
```Phần đầu tiên của`is_brothers`chuyển đổi yêu cầu phần trăm thành ngân sách xóa số nguyên. Từ`n - LCS`là không thể thiếu,`k = n // 100`chính xác là số ký tự tối đa có thể bị xóa. 

các`k == 0`nhánh xử lý tất cả các chuỗi ngắn hơn`100`trực tiếp. Trong trường hợp này, LCS được yêu cầu là toàn bộ chuỗi, do đó sự bằng nhau của hai đầu vào là cần thiết và đủ. 

Bảng DP có kích thước`(k + 2) x (k + 2)`. Hàng và cột bổ sung giúp quá trình chuyển đổi an toàn khi`i`hoặc`j`bằng với`k`. Chúng tôi vẫn chỉ xử lý các trạng thái từ`0`bởi vì`k`, vì số lần xóa lớn hơn không thể tạo ra câu trả lời có thể chấp nhận được. 

Các biến`x = i + p`Và`y = j + p`là vị trí hiện tại thực tế trong hai chuỗi. Sau khi sử dụng một ký tự trùng khớp, cả vị trí và`p`cùng nhau tiến lên. Điều này ít xảy ra lỗi hơn so với việc tính toán lại các vị trí từ đầu bên trong vòng lặp. 

Séc`i < k`Và`j < k`là cần thiết trước khi truyền sang trạng thái xóa khác. Nếu không có chúng, mã có thể truy cập vào hàng hoặc cột ranh giới bổ sung như thể đó là trạng thái hợp lệ và vô tình sử dụng nhiều hơn số lần xóa cho phép. 

Không có số học dấu phẩy động được sử dụng. Kiểm tra`p >= n - n // 100`hoàn toàn tương đương với thử nghiệm`p >= 0.99n`, kể cả khi`n`không chia hết cho`100`. 

Số nguyên Python không có vấn đề tràn ở đây và tối đa mọi giá trị DP đều`n`. Chi phí bộ nhớ chính đến từ khoảng một triệu mục DP khi`n = 100000`, vẫn thoải mái dưới giới hạn bộ nhớ. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
A = GAATTGCGTACAATGC
B = GAATTGCGTACAATGC
```chiều dài là`16`, Vì thế`k = 16 // 100 = 0`. Độ dài LCS cần thiết là`16`. 

| n | k | mục tiêu | Trạng thái ban đầu | Kết quả | 
| --- | --- | --- | --- | --- | 
| 16 | 0 | 16 |`dp[0][0] = 0`|`A == B`, chấp nhận | 

Trường hợp xóa 0 được xử lý trực tiếp vì cách duy nhất để có được dãy con chung có độ dài`16`là giữ lại mọi nhân vật. Hai chuỗi giống hệt nhau nên kết quả đầu ra là`Long lost brothers D:`. 

Đối với mẫu 2,```
A = CCATAGAGAA
B = CGATAGAGAA
```chiều dài là`10`, vậy một lần nữa`k = 0`và mục tiêu là`10`. 

| n | k | mục tiêu | So sánh | Kết quả | 
| --- | --- | --- | --- | --- | 
| 10 | 0 | 10 |`CCATAGAGAA != CGATAGAGAA`| từ chối | 

Các chuỗi khác nhau ở ký tự thứ hai của chúng. Bởi vì ngân sách xóa bằng 0 nên sự khác biệt duy nhất đó không thể được loại bỏ hoặc bỏ qua. LCS của họ nhiều nhất là`9`, dưới mức yêu cầu`10`, vì vậy đầu ra đúng là`Not brothers :(`. 

Một ví dụ lớn hơn cho thấy việc xóa DP thực tế. Coi như```
A = AAAAAAAAAA
B = AAAATAAAAA
```Đây`n = 10`, vì vậy vấn đề ban đầu vẫn có`k = 0`. Để minh họa cơ chế DP, hãy tưởng tượng cấu trúc tương tự ở`n = 100`, với một ký tự khác nhau. Sau đó`k = 1`và xóa ký tự phụ cho phép hai chuỗi chia sẻ`99`nhân vật. 

| Tình trạng`(i,j)`|`p`trước khi gia hạn | Vị trí tiếp theo | Hành động |`p`sau khi gia hạn | 
| --- | --- | --- | --- | --- | 
|`(0,0)`| 0 |`(0,0)`| Khớp tiền tố bằng nhau | 4 | 
|`(1,0)`| 4 |`(5,4)`| Tiếp tục khớp | 99 | 
|`(0,1)`| 4 |`(4,5)`| Tiếp tục khớp | 99 | 

Sự không phù hợp đầu tiên có thể được xử lý theo hai cách. Xóa ký tự khỏi`A`sản xuất trạng thái`(1,0)`, trong khi xóa ký tự khỏi`B`sản xuất`(0,1)`. Một trong những con đường đó đạt được yêu cầu`99`các ký tự trùng khớp. Điều này chứng tỏ tại sao DP cần cả hai quá trình chuyển đổi xóa thay vì coi sự không khớp là sự không khớp vị trí đơn giản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(nk)`| có`O(k^2)`trạng thái xóa và phần mở rộng có ký tự bằng nhau được khấu hao theo thời gian`k`các lớp xóa. | 
| Không gian |`O(k^2)`| Bảng DP chứa`(k + 2)^2`các trạng thái số nguyên. | 

Đây`k = floor(n / 100)`, vậy ở mức tối đa`n = 100000`, chúng tôi có`k = 1000`. Thuật toán hoạt động với khoảng một triệu trạng thái xóa thay vì khoảng mười tỷ trạng thái của LCS thông thường. Mức tiêu thụ bộ nhớ cũng thấp hơn nhiều so với mức đầy đủ`n x n`bảng LCS. 

## Trường hợp thử nghiệm```python
import sys
import io

YES = "Long lost brothers D:"
NO = "Not brothers :("

def is_brothers(a: str, b: str) -> bool:
    n = len(a)
    k = n // 100
    target = n - k

    if k == 0:
        return a == b

    size = k + 2
    dp = [[0] * size for _ in range(size)]

    for i in range(k + 1):
        row = dp[i]

        for j in range(k + 1):
            p = row[j]

            x = i + p
            y = j + p

            while x < n and y < n and a[x] == b[y]:
                p += 1
                x += 1
                y += 1

            row[j] = p

            if p >= target:
                return True

            if i < k and p > dp[i + 1][j]:
                dp[i + 1][j] = p

            if j < k and p > row[j + 1]:
                row[j + 1] = p

    return False

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        a = sys.stdin.readline().strip()
        b = sys.stdin.readline().strip()
        return YES if is_brothers(a, b) else NO
    finally:
        sys.stdin = old_stdin

# Provided sample 1
assert run(
    "GAATTGCGTACAATGC\n"
    "GAATTGCGTACAATGC\n"
) == YES, "sample 1"

# Provided sample 2
assert run(
    "CCATAGAGAA\n"
    "CGATAGAGAA\n"
) == NO, "sample 2"

# Minimum size, equal strings.
assert run("A\nA\n") == YES, "minimum equal strings"

# Minimum size, different strings. k = 0, so no deletion is allowed.
assert run("A\nT\n") == NO, "minimum different strings"

# n = 100 and exactly one deletion is enough.
a = "A" * 100
b = "A" * 50 + "T" + "A" * 49
assert run(a + "\n" + b + "\n") == YES, "one deletion boundary"

# n = 100 and two deletions are necessary, which exceeds the budget.
a = "A" * 100
b = "A" * 50 + "TT" + "A" * 48
assert run(a + "\n" + b + "\n") == NO, "two deletions boundary"

# Maximum-size all-equal input.
a = "A" * 100000
assert run(a + "\n" + a + "\n") == YES, "maximum all-equal input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`A / A`|`Long lost brothers D:`| Kích thước tối thiểu và chấp nhận không xóa | 
|`A / T`|`Not brothers :(`| Kích thước tối thiểu và từ chối không xóa | 
| Chiều dài`100`, một cái được chèn vào`T`|`Long lost brothers D:`| Chính xác`0.99n`ranh giới | 
| Chiều dài`100`, hai cái được chèn vào`T`nhân vật |`Not brothers :(`| Không được vượt quá ngân sách một lần xóa | 
| Hai chuỗi có độ dài bằng nhau`100000`|`Long lost brothers D:`| Kích thước đầu vào tối đa và ranh giới DP lớn | 

## Vỏ cạnh 

Khi nào`n < 100`, ngân sách xóa bằng không. Đối với đầu vào bê tông```
A
T
```chúng tôi nhận được`k = 1 // 100 = 0`Và`target = 1`. Thuật toán ngay lập tức so sánh hai chuỗi hoàn chỉnh và thấy chúng khác nhau nên trả về`Not brothers :(`. Không có nỗ lực nào được thực hiện để giải thích`0.99`ngưỡng sử dụng làm tròn dấu phẩy động. 

Chính xác`n = 100`, được phép xóa một lần. Coi như```
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAATAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
```Chuỗi thứ hai chứa thêm một`T`liên quan đến tất cả-`A`sợi dây. DP có thể đạt đến sự không phù hợp với trạng thái`(0,0)`, sau đó xóa`T`từ chuỗi thứ hai hoặc xóa chuỗi tương ứng`A`từ chuỗi đầu tiên. Trạng thái kết quả có một lần xóa và có thể khớp với phần còn lại`99`nhân vật, vì vậy`p`đạt tới`target = 99`và thuật toán chấp nhận. 

Nếu cần phải xóa hai lần, cơ chế tương tự chỉ đạt được`98`các ký tự phù hợp trong phạm vi ngân sách xóa được phép. Ví dụ, với```
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAATTAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
```dãy con chung tốt nhất có độ dài`98`. Từ`k = 1`, DP chỉ khám phá các trạng thái có nhiều nhất một lần xóa khỏi một trong hai chuỗi, do đó, DP không thể chấp nhận sai một giải pháp cần xóa hai lần. Kết quả là`Not brothers :(`. 

Các dây giống hệt nhau thực hiện thái cực ngược lại. Đối với đầu vào kích thước tối đa bao gồm`100000`bản sao của`A`trong cả hai chuỗi, trạng thái ban đầu tiêu thụ toàn bộ chuỗi, tạo ra`p = 100000`. Vì mục tiêu cũng là`99000`, thuật toán trả về ngay lập tức. Điều này vừa xác nhận tính bất biến phù hợp vừa tránh việc thăm dò không cần thiết các trạng thái xóa còn lại. 

Sự khác biệt giữa chuỗi con và chuỗi con cũng có vấn đề. Việc không khớp không tự động có nghĩa là các chuỗi không tương thích, vì việc xóa một ký tự có thể căn chỉnh lại hai hậu tố. DP thể hiện rõ ràng cả hai khả năng bằng cách xóa khỏi`A`hoặc từ`B`. Đó là lý do tại sao việc kiểm tra khoảng cách Hamming theo vị trí không phải là sự thay thế hợp lệ cho LCS ở đây.
