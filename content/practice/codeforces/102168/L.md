---
title: "CF 102168L - \u041f\u0435\u0440\u0435\u0432\u043e\u0440\u043e\u0442\u044b"
description: "Chúng ta có hai chuỗi chữ thường không trống, s và t. Trong một thao tác, chúng ta có thể chọn bất kỳ hai vị trí nào trong s và đảo ngược toàn bộ chuỗi con giữa chúng. Chúng tôi có thể thực hiện thao tác bất kỳ số lần nào, kể cả số lần không."
date: "2026-08-19T07:30:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102168
codeforces_index: "L"
codeforces_contest_name: "\u041b\u0438\u0447\u043d\u044b\u0439 \u0447\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0421\u0430\u043c\u0430\u0440\u0441\u043a\u043e\u0433\u043e \u0443\u043d\u0438\u0432\u0435\u0440\u0441\u0438\u0442\u0435\u0442\u0430 \u0441\u0440\u0435\u0434\u0438 \u043d\u043e\u0432\u0438\u0447\u043a\u043e\u0432 2018-2019"
rating: 0
weight: 102168
solve_time_s: 93
verified: true
draft: false
---

[CF 102168L - \u041f\u0435\u0440\u0435\u0432\u043e\u0440\u043e\u0442\u044b](https://codeforces.com/problemset/problem/102168/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai chuỗi chữ thường không trống,`s`Và`t`. Trong một thao tác, chúng ta có thể chọn bất kỳ hai vị trí nào trong`s`và đảo ngược toàn bộ chuỗi con giữa chúng. Chúng tôi có thể thực hiện thao tác bất kỳ số lần nào, kể cả số lần không. Nhiệm vụ là xác định xem liệu`s`có thể được chuyển đổi thành chính xác`t`. 

Câu hỏi quan trọng không phải là việc đảo ngược riêng lẻ nào sẽ được thực hiện mà là việc sắp xếp lại các ký tự nào có thể được tạo ra bằng cách đảo ngược lặp đi lặp lại. Vì cả hai chuỗi chứa tối đa 200.000 ký tự nên thuật toán kiểm tra nhiều chuỗi đảo ngược có thể xảy ra sẽ không thể hoạt động. Với giới hạn hai giây, giải pháp dự kiến ​​về cơ bản cần phải tuyến tính theo độ dài chuỗi. Các thuật toán bậc hai đã yêu cầu khoảng 40 tỷ lần lặp cơ bản ở kích thước tối đa, vượt xa mức thực tế. 

Trường hợp cạnh đầu tiên có độ dài khác nhau. Ví dụ,`s = "abcd"`Và`t = "abc"`phải sản xuất`NO`. Việc đảo ngược không bao giờ thay đổi số lượng ký tự, do đó, bất kỳ cách tiếp cận nào chỉ so sánh tần số ký tự mà không xem xét độ dài trước tiên sẽ cần phải xử lý vấn đề này một cách rõ ràng. 

Trường hợp cạnh thứ hai là các ký tự lặp lại. Vì`s = "xxx"`Và`t = "yyy"`, câu trả lời là`NO`, mặc dù cả hai chuỗi đều có cùng độ dài. Một giải pháp bất cẩn chỉ kiểm tra độ dài sẽ chấp nhận nó. Tính đa dạng của nhân vật rất quan trọng. 

Trường hợp cạnh thứ ba là các chuỗi không nhất thiết phải bằng nhau hoặc có thể đạt được chỉ bằng một lần đảo ngược. Vì`s = "abac"`Và`t = "acba"`, câu trả lời là`YES`. Sự chuyển đổi có thể đạt được thông qua một số lần đảo ngược, vì vậy hãy kiểm tra xem liệu`t`chính xác là một chuỗi con bị đảo ngược cách xa`s`là không đủ. 

Trường hợp cạnh thứ tư là một chuỗi đã bằng đích. Ví dụ,`s = "abc"`Và`t = "abc"`phải sản xuất`YES`, bởi vì thực hiện các thao tác bằng 0 được cho phép. Việc triển khai giả định ít nhất một lần đảo ngược là bắt buộc có thể từ chối trường hợp hợp lệ này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ coi mọi sự sắp xếp có thể có của các nhân vật như một trạng thái có thể tiếp cận được. Đối với mỗi trạng thái, chúng ta có thể thử mọi khả năng đảo ngược có thể và tiếp tục khám phá cho đến khi`t`được tìm thấy hoặc mọi sắp xếp có thể tiếp cận đã hết. Điều này đúng vì mọi hoạt động hợp pháp đều được thể hiện rõ ràng nhưng nó nhanh chóng trở nên không sử dụng được. Với`n`có thể có những ký tự riêng biệt`n!`các hoán vị khác nhau, và thậm chí kiểm tra một hoán vị với`t`chi phí`O(n)`. Do đó, một tìm kiếm dựa trên hoán vị hoàn chỉnh cần ít nhất`O(n · n!)`làm việc, có thêm`O(n^2)`yếu tố nếu mọi trạng thái kiểm tra rõ ràng tất cả các khoảng thời gian đảo chiều có thể xảy ra. 

Quan sát hữu ích đến từ việc xem xét khả năng đảo chiều nhỏ nhất có thể. Nếu chúng ta đảo ngược một chuỗi con có độ dài hai, vị trí`i`Và`i + 1`chỉ đơn giản là trao đổi nhân vật của họ. Nói cách khác, hoạt động được phép bao gồm một giao dịch hoán đổi liền kề. 

Hoán đổi liền kề là đủ để tạo ra mọi hoán vị của chuỗi. Chúng ta có thể di chuyển bất kỳ ký tự mong muốn nào từng vị trí một cho đến khi nó đạt đến vị trí mục tiêu, lặp lại quá trình này cho mọi vị trí. Do đó, việc đảo ngược chuỗi con lặp đi lặp lại có thể sắp xếp lại các ký tự trong`s`theo những cách hoàn toàn tùy ý. 

Khi có thể hoán vị tùy ý, thứ tự chính xác của các ký tự gốc không còn quan trọng nữa. Bất biến duy nhất là mỗi ký tự xuất hiện bao nhiêu lần. Một sự chuyển đổi từ`s`ĐẾN`t`tồn tại chính xác khi hai chuỗi có cùng độ dài và cùng tần số cho mọi chữ cái viết thường. 

Vì bảng chữ cái chỉ có 26 chữ cái viết thường nên chúng ta thậm chí không cần đến từ điển đa năng. Chúng ta có thể giữ hai mảng gồm 26 bộ đếm và so sánh chúng. Điều này đưa ra một giải pháp thời gian tuyến tính đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n · n!)`tối thiểu |`O(n)`để lưu trữ một trạng thái, có khả năng theo cấp số nhân đối với các trạng thái được truy cập | Quá chậm | 
| Tối ưu |`O(n)`|`O(1)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`s`Và`t`và so sánh độ dài của chúng. Nếu độ dài khác nhau, hãy in`NO`ngay lập tức vì sự đảo chiều không bao giờ thay đổi số lượng vị thế. 
2. Tạo một mảng gồm 26 bộ đếm cho`s`và một dãy 26 bộ đếm khác cho`t`. chỉ số`0`đại diện cho`'a'`, chỉ số`1`đại diện cho`'b'`, vân vân. 
3. Quét`s`và tăng bộ đếm tương ứng với từng ký tự. Làm tương tự cho`t`. Điều này ghi lại chính xác nhiều bộ ký tự mà mỗi chuỗi chứa. 
4. So sánh hai mảng tần số. Nếu mọi số đếm đều bằng nhau, hãy in`YES`; nếu không thì in`NO`. 

Lý do kiểm tra tần số này là đủ là vì sự đảo ngược chiều dài hai mang lại cho chúng ta một hoán đổi liền kề. Vì các hoán đổi liền kề tùy ý tạo ra mọi hoán vị, nên có thể đạt được bất kỳ thứ tự nào có cùng nhiều ký tự. 

### Tại sao nó hoạt động 

Mọi thao tác đều bảo toàn nhiều tập ký tự, do đó các tần số khác nhau không bao giờ có thể được chuyển đổi thành nhau. Ngược lại, giả sử`s`Và`t`có tần số ký tự giống hệt nhau. Vì các giao dịch hoán đổi liền kề được cho phép nên chúng ta có thể sắp xếp lại`s`vào bất kỳ hoán vị nào của các ký tự của nó. Chúng ta có thể xây dựng`t`từ trái sang phải bằng cách di chuyển lần xuất hiện của ký tự được yêu cầu vào từng vị trí mục tiêu bằng cách sử dụng các phép hoán đổi liền kề. Việc đảo ngược chuỗi con lặp đi lặp lại có thể thực hiện các hoán đổi đó, vì vậy`t`có thể truy cập được. Như vậy điều kiện tần số vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    t = input().strip()

    if len(s) != len(t):
        print("NO")
        return

    cnt_s = [0] * 26
    cnt_t = [0] * 26

    for ch in s:
        cnt_s[ord(ch) - ord('a')] += 1

    for ch in t:
        cnt_t[ord(ch) - ord('a')] += 1

    print("YES" if cnt_s == cnt_t else "NO")

if __name__ == "__main__":
    solve()
```Kiểm tra đầu tiên xử lý ranh giới độ dài trước khi xử lý ký tự. Điều này vừa cần thiết về mặt logic vừa rẻ hơn một chút đối với những đầu vào không thể thực hiện được. 

Hai mảng có chính xác 26 mục vì bảng chữ cái đầu vào bị hạn chế ở các chữ cái tiếng Anh viết thường. biểu hiện`ord(ch) - ord('a')`ánh xạ mỗi ký tự thành một số nguyên từ 0 đến 25. 

Sự so sánh`cnt_s == cnt_t`kiểm tra tất cả các bội số ký tự cùng một lúc. Không cần so sánh vị thế vì đảo chiều có thể thay đổi thứ tự tùy ý. 

Không thể tràn số nguyên trong Python và bộ đếm lớn nhất chỉ là 200.000. Bản thân các dây cũng vừa vặn thoải mái trong bộ nhớ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho`s = "abac"`Và`t = "acba"`, cả hai chuỗi đều có bốn ký tự. Các vectơ tần số của chúng giống hệt nhau. 

| Nhân vật | Đếm vào`s`| Đếm vào`t`| 
| --- | --- | --- | 
|`a`| 2 | 2 | 
|`b`| 1 | 1 | 
|`c`| 1 | 1 | 
| các chữ cái khác | 0 | 0 | 

Do đó, thuật toán in`YES`. 

Ví dụ này cũng chứng minh tại sao việc chỉ kiểm tra xem một đảo ngược có hoạt động hay không sẽ là một sự trừu tượng hóa sai lầm. Bộ thao tác đủ mạnh để thực hiện các hoán vị tùy ý, do đó bản thân lệnh cuối cùng không áp đặt một hạn chế bổ sung nào. 

### Mẫu 2 

cho`s = "xxx"`Và`t = "yyy"`, độ dài bằng nhau nên thuật toán tiến hành so sánh tần số. 

| Nhân vật | Đếm vào`s`| Đếm vào`t`| 
| --- | --- | --- | 
|`x`| 3 | 0 | 
|`y`| 0 | 3 | 
| các chữ cái khác | 0 | 0 | 

Các mảng tần số khác nhau nên thuật toán in ra`NO`. 

Điều này khẳng định rằng chỉ độ dài bằng nhau thôi là chưa đủ. Đảo ngược chỉ sắp xếp lại các ký tự hiện có và không thể biến ký tự này thành ký tự khác. 

### Mẫu 3 

cho`s = "abcd"`Và`t = "abc"`, độ dài khác nhau. 

| Biến | Giá trị | 
| --- | --- | 
|`len(s)`| 4 | 
|`len(t)`| 3 | 
| Quyết định |`NO`| 

Thuật toán trả về ngay lập tức mà không cần xây dựng mảng tần số. Không có chuỗi đảo ngược nào có thể loại bỏ ký tự thứ tư. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n)`| Mỗi chuỗi đầu vào được quét một lần và so sánh tần số cuối cùng chỉ kiểm tra 26 mục. | 
| Không gian |`O(1)`| Hai mảng gồm 26 số nguyên được sử dụng, độc lập với`n`. | 

Vì`n <= 200000`, thuật toán chỉ thực hiện vài trăm nghìn thao tác ký tự. Điều này hoàn toàn thoải mái trong giới hạn hai giây nhất định, trong khi không gian hoán vị vũ phu tăng theo giai thừa và trở nên không thể thực hiện được ngay cả đối với các chuỗi rất nhỏ. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    s = input().strip()
    t = input().strip()

    if len(s) != len(t):
        print("NO")
        return

    cnt_s = [0] * 26
    cnt_t = [0] * 26

    for ch in s:
        cnt_s[ord(ch) - ord('a')] += 1

    for ch in t:
        cnt_t[ord(ch) - ord('a')] += 1

    print("YES" if cnt_s == cnt_t else "NO")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("abac\nacba\n") == "YES\n", "sample 1"
assert run("xxx\nyyy\n") == "NO\n", "sample 2"
assert run("abcd\nabc\n") == "NO\n", "sample 3"

# Minimum-size input
assert run("a\na\n") == "YES\n", "single equal character"

# Minimum-size mismatch
assert run("a\nb\n") == "NO\n", "single different character"

# All characters equal
assert run("aaaaaa\naaaaaa\n") == "YES\n", "all equal characters"

# Same multiset, very different order
assert run("abcabc\nccbbaa\n") == "YES\n", "same frequencies"

# Same length, but one character count differs
assert run("aabb\nabac\n") == "NO\n", "different multiplicities"

# Maximum-size input
n = 200000
assert run("a" * n + "\n" + "a" * n + "\n") == "YES\n", "maximum length"

# Maximum-size mismatch at the boundary
assert run("a" * (n - 1) + "b\n" + "a" * n + "\n") == "NO\n", "maximum length mismatch"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a / a`|`YES`| Độ dài chuỗi tối thiểu và hoạt động bằng 0 | 
|`a / b`|`NO`| Ký tự có kích thước tối thiểu không khớp | 
|`aaaaaa / aaaaaa`|`YES`| Tất cả các ký tự giống hệt nhau | 
|`abcabc / ccbbaa`|`YES`| Nhiều bộ giống nhau nhưng thứ tự khác nhau đáng kể | 
|`aabb / abac`|`NO`| Đa nhân vật khác nhau | 
|`200000 × 'a' / 200000 × 'a'`|`YES`| Kích thước đầu vào tối đa và hiệu suất tuyến tính | 
|`199999 × 'a' + 'b' / 200000 × 'a'`|`NO`| Trường hợp ranh giới lớn có tần số không khớp duy nhất | 

## Vỏ cạnh 

Đối với các độ dài khác nhau, hãy xem xét`s = "abcd"`Và`t = "abc"`. Thuật toán so sánh`4`Và`3`, in ngay lập tức`NO`và không bao giờ cố gắng so sánh tần số ký tự của chúng. Điều này đúng vì mọi đảo chiều đều bảo toàn chính xác độ dài. 

Đối với các ký tự lặp lại có bội số khác nhau, hãy xem xét`s = "aabb"`Và`t = "abac"`. Cả hai chuỗi đều có độ dài bằng 4, do đó bài kiểm tra độ dài đã vượt qua. Tần số của`a`là hai trong cả hai chuỗi, nhưng`s`chứa hai`b`nhân vật trong khi`t`chứa một`b`và một`c`. Các mảng khác nhau ở những vị trí đó, vì vậy kết quả là`NO`. 

Đối với các phép biến đổi yêu cầu nhiều hơn một lần đảo ngược, hãy xem xét`s = "abcabc"`Và`t = "ccbbaa"`. Tần số của`a`,`b`, Và`c`là hai trong cả hai chuỗi. Thuật toán trả về`YES`mà không cố gắng tìm ra một chuỗi đảo ngược cụ thể. Bằng chứng ở trên đảm bảo rằng chuỗi như vậy tồn tại vì các hoán đổi liền kề có thể nhận ra bất kỳ hoán vị nào. 

Đối với các chuỗi đã bằng nhau, hãy xem xét`s = "abc"`Và`t = "abc"`. Độ dài trùng khớp và cả ba số ký tự đều khớp nhau, vì vậy kết quả là`YES`. Điều này tương ứng với việc sử dụng các thao tác bằng 0, được phép. 

Đối với các chuỗi nhỏ nhất có thể,`s = "a"`Và`t = "b"`hoàn toàn không thể thay đổi được vì không có cặp vị trí riêng biệt nào để thực hiện đảo chiều có ý nghĩa. Tần số của chúng khác nhau nên thuật toán trả về`NO`. Vì`s = "a"`Và`t = "a"`, tần số khớp và thuật toán trả về`YES`. 

Kiểu chữ có kích thước tối đa chứa 200.000 ký tự. Thuật toán vẫn thực hiện một lần chuyển qua mỗi chuỗi và chỉ giữ 52 bộ đếm, do đó việc sử dụng tài nguyên của nó không phụ thuộc vào số lượng hoán vị có thể có. Đây chính xác là thuộc tính cần thiết để làm cho giải pháp trở nên thực tế dưới những ràng buộc nhất định.
