---
title: "CF 102302D - Tin nhắn đoán"
description: "Văn bản của Samuelo là một chuỗi s, và thông điệp ẩn mà Roppa đoán là một chuỗi t khác. Dự đoán được coi là đúng nếu chúng ta có thể xóa một số ký tự khỏi s trong khi vẫn giữ các ký tự còn lại theo thứ tự ban đầu và thu được chính xác t."
date: "2026-08-13T07:35:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102302
codeforces_index: "D"
codeforces_contest_name: "2019 USP-ICMC"
rating: 0
weight: 102302
solve_time_s: 113
verified: true
draft: false
---

[CF 102302D - Tin nhắn đoán](https://codeforces.com/problemset/problem/102302/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Văn bản của Samuelo là một chuỗi`s`, và tin nhắn ẩn được đoán của Roppa là một chuỗi khác`t`. Dự đoán được coi là đúng nếu chúng ta có thể xóa một số ký tự khỏi`s`trong khi vẫn giữ các ký tự còn lại theo thứ tự ban đầu và thu được chính xác`t`. Nói cách khác,`t`phải là dãy con của`s`. 

Ví dụ, trong`threeyellowfurryfiends`, các ký tự cần thiết cho`hellofriend`xuất hiện theo đúng thứ tự, mặc dù có nhiều ký tự không liên quan xuất hiện giữa chúng. Chúng ta chỉ quan tâm đến thứ tự tương đối chứ không quan tâm đến các vị trí liền kề. 

Cả hai chuỗi có thể chứa tối đa`10^6`nhân vật. Kích thước đó loại trừ bất kỳ phương pháp nào kiểm tra các cặp vị trí lặp đi lặp lại trong một vòng lặp bậc hai. Với một triệu ký tự, một`O(nm)`thuật toán có thể thực hiện tới`10^12`hoạt động, vượt xa những gì giới hạn 1 giây có thể đáp ứng. Giải pháp dự định phải xử lý các chuỗi về cơ bản một lần, đưa ra thời gian tuyến tính trong tổng chiều dài của chúng. Giới hạn bộ nhớ cũng tạo ra`O(nm)`bảng lập trình động không thực tế, vì một triệu x một triệu mục sẽ yêu cầu một lượng bộ nhớ khổng lồ. 

Một số trường hợp cạnh rất dễ xử lý sai. Đầu tiên, hai chuỗi có thể có cùng độ dài. Ví dụ,```
abc
abc
```có câu trả lời`YES`, bởi vì mọi ký tự phải được sử dụng và các chuỗi giống hệt nhau. Việc triển khai vô tình yêu cầu một ký tự không được sử dụng sau kết quả khớp cuối cùng có thể trả về không chính xác`NO`. 

Đoạn đoán cũng có thể dài hơn văn bản gốc. Ví dụ,```
ab
abc
```có câu trả lời`NO`. Không có đủ ký tự trong`s`hình thành`t`. Việc triển khai bất cẩn chỉ kiểm tra xem mọi ký tự của`t`xảy ra ở đâu đó trong`s`, nếu không tôn trọng tính đa dạng và trật tự, có thể mắc sai lầm này. 

Các ký tự lặp lại là một nguồn lỗi phổ biến khác. Vì```
aab
aaa
```câu trả lời là`NO`, bởi vì`s`chỉ chứa hai`a`nhân vật. Chỉ kiểm tra xem lá thư`a`xuất hiện sẽ không đủ. 

Cuối cùng, các ký tự trùng khớp có thể được phân tách bằng văn bản tùy ý. Vì```
axbycz
abc
```câu trả lời là`YES`. Các ký tự không cần phải liền kề nhau, do đó việc triển khai tìm kiếm`t`vì một chuỗi con liền kề sẽ từ chối nó một cách không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp nhưng tốn kém không cần thiết là xây dựng một bảng lập trình động. Cho phép`dp[i][j]`mô tả liệu lần đầu tiên`j`nhân vật của`t`có thể thu được dưới dạng dãy con của dãy đầu tiên`i`nhân vật của`s`. Đối với mỗi cặp`(i, j)`, chúng tôi quyết định có nên bỏ qua`s[i - 1]`hoặc khi các ký tự khớp nhau, hãy sử dụng nó làm ký tự tiếp theo của`t`. Điều này đúng vì mọi dãy con có thể đều sử dụng ký tự hiện tại hoặc không. 

Vấn đề là số lượng trạng thái. Có khoảng`n * m`cặp, ở đâu`n = len(s)`Và`m = len(t)`. Ở giới hạn tối đa, đó là khoảng`10^12`tiểu bang. Ngay cả việc lưu trữ một bảng như vậy cũng không thể thực hiện được trong giới hạn bộ nhớ và việc tính toán nó quá chậm. 

Quan sát loại bỏ toàn bộ bảng là chúng ta không cần phải nhớ tất cả các cách có thể để khớp với tiền tố của`t`. Giả sử chúng ta hiện đang tìm kiếm nhân vật`t[j]`. Trong số tất cả các vị trí trong`s`trong đó nhân vật đó có thể phù hợp, việc chiếm vị trí sớm nhất có thể ít nhất luôn tốt bằng việc chiếm vị trí sau. Trận đấu sớm hơn để lại ít nhất phần còn lại của chuỗi còn lại cho phần còn lại của`t`. 

Điều này cho phép quét tham lam. Chúng tôi xử lý`s`từ trái sang phải trong khi vẫn giữ con trỏ tới ký tự tiếp theo của`t`mà chúng tôi cần. Bất cứ khi nào nhân vật hiện tại của`s`khớp với ký tự đích đó, chúng ta sử dụng nó và tiến tới con trỏ đích. Nếu con trỏ đích đến cuối`t`, toàn bộ dự đoán đã được nhúng thành công. Nếu như`s`kết thúc trước, không có nội dung nhúng hợp lệ nào tồn tại. 

DP brute-force hoạt động vì nó xem xét rõ ràng mọi kết hợp tiền tố, nhưng không thành công vì có quá nhiều kết hợp. Việc quan sát tham lam cho phép chúng tôi chỉ giữ vị trí khả thi sớm nhất cho nhân vật mục tiêu hiện tại, giảm vấn đề xuống còn một lần đi qua`s`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DP | O(nm) | O(nm) | Quá chậm và quá nhiều bộ nhớ | 
| Quét tham lam tối ưu | O(n + m) | O(1) không gian phụ trợ | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`s`Và`t`, sau đó tạo một con trỏ`j`ban đầu bằng 0. Con trỏ đại diện cho ký tự đầu tiên của`t`điều đó vẫn chưa được khớp. 
2. Quét từng ký tự`c`của`s`từ trái sang phải. Nếu như`j`vẫn còn ở bên trong`t`Và`c == t[j]`, sử dụng sự xuất hiện này của`c`để khớp với ký tự được yêu cầu tiếp theo và số gia tăng`j`. 
3. Nếu`j`trở nên bằng`len(t)`, mọi ký tự của`t`đã được kết hợp ở vị trí ngày càng tăng của`s`, vậy câu trả lời là`YES`. Chúng ta có thể dừng ngay lập tức vì không còn nhân vật nào có liên quan nữa. 
4. Nếu quá trình quét đến hết`s`trong khi`j < len(t)`, ít nhất một ký tự bắt buộc không bao giờ được tìm thấy sau các trận đấu trước đó. Câu trả lời là`NO`. 

Sự lựa chọn tham lam là an toàn vì khi`s[i] == t[j]`, sử dụng vị trí`i`không bao giờ tệ hơn việc bỏ qua nó và sử dụng lần xuất hiện sau của cùng một ký tự. Phần còn lại của`t`chỉ có thể được hưởng lợi từ việc có nhiều vị trí hơn`s`có sẵn sau trận đấu hiện tại. 

### Tại sao nó hoạt động 

Sau khi xử lý bất kỳ tiền tố nào của`s`,`j`là số ký tự liên tiếp tối đa tính từ đầu`t`tiền tố được xử lý có thể khớp bằng cách sử dụng các lựa chọn tham lam. Cụ thể hơn, mọi ký tự trùng khớp của`t`được giao một vị trí ngày càng cao trong`s`, vì vậy bất cứ khi nào`j`đạt tới`len(t)`, chúng ta đã xây dựng một dãy con hợp lệ một cách rõ ràng. 

Ngược lại, bất cứ khi nào thuật toán bỏ qua một ký tự của`s`, ký tự đó khác với ký tự bắt buộc tiếp theo hoặc không có yêu cầu nào còn lại. Nếu nó khớp với ký tự được yêu cầu tiếp theo, việc sử dụng lần xuất hiện sớm nhất như vậy không thể khiến việc khớp sau đó không thể thực hiện được, bởi vì mọi vị trí có thể được sử dụng sau nó vẫn có sẵn. Do đó, nếu quá trình quét tham lam kết thúc trước khi khớp tất cả`t`, không có lựa chọn thay thế nào cho các kết quả khớp trước đó có thể tạo ra một dãy con hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    t = input().strip()

    j = 0

    for c in s:
        if j < len(t) and c == t[j]:
            j += 1
            if j == len(t):
                print("YES")
                return

    print("NO")

if __name__ == "__main__":
    solve()
```Đầu vào được đọc với`readline`, phù hợp với các chuỗi có độ dài lên tới một triệu. các`.strip()`cuộc gọi sẽ xóa dòng mới mà không thay đổi bất kỳ chữ cái tiếng Anh viết thường nào trong chuỗi thực tế. 

Biến`j`là trạng thái duy nhất cần thiết. Nó luôn trỏ đến ký tự tiếp theo được yêu cầu từ`t`. Khi`s`chứa ký tự đó, chúng tôi tiến lên`j`; mặt khác, nhân vật hiện tại của`s`có thể được bỏ qua một cách an toàn. 

điều kiện`j < len(t)`ngăn chặn truy cập`t[j]`sau khi toàn bộ dự đoán đã được khớp. Ngay lập tức`YES`return cũng tránh quét phần còn lại của`s`, mặc dù ngay cả khi không có sự tối ưu hóa này thì thuật toán vẫn duy trì tuyến tính. 

Không có vấn đề tràn số nguyên vì số nguyên Python có độ chính xác tùy ý và thuật toán chỉ duy trì một con trỏ được giới hạn bởi độ dài của`t`. Quan trọng hơn, không có bảng hai chiều nên mức sử dụng bộ nhớ vẫn rất nhỏ so với giới hạn 256 MB. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là:```
threeyellowfurryfiends
hellofriend
```Con trỏ bắt đầu ở ký tự đầu tiên của`t`, đó là`h`. 

| Nhân vật từ`s`| Ký tự bắt buộc tiếp theo | Cuộc thi đấu? |`j`sau khi xử lý | 
| --- | --- | --- | --- | 
|`t`|`h`| Không | 0 | 
|`h`|`h`| Có | 1 | 
|`r`|`e`| Không | 1 | 
|`e`|`e`| Có | 2 | 
|`e`|`l`| Không | 2 | 
|`y`|`l`| Không | 2 | 
|`e`|`l`| Không | 2 | 
|`l`|`l`| Có | 3 | 
|`l`|`o`| Không | 3 | 
|`o`|`o`| Có | 4 | 
|`w`|`f`| Không | 4 | 
|`f`|`f`| Có | 5 | 
|`u`|`r`| Không | 5 | 
|`r`|`r`| Có | 6 | 
|`r`|`i`| Không | 6 | 
|`y`|`i`| Không | 6 | 
|`f`|`i`| Không | 6 | 
|`i`|`i`| Có | 7 | 
|`e`|`e`| Có | 8 | 
|`n`|`n`| Có | 9 | 
|`d`|`d`| Có | 10 | 

Ở trận đấu cuối cùng,`j`bằng`len(t)`, do đó thuật toán in`YES`. Dấu vết chứng tỏ rằng các ký tự không liên quan có thể được bỏ qua một cách đơn giản trong khi các vị trí phù hợp vẫn tăng lên một cách nghiêm ngặt. 

Đối với Mẫu 2, đầu vào là:```
hardcontest
easyac
```Quá trình quét cố gắng tìm`e`Đầu tiên. 

| Nhân vật từ`s`| Ký tự bắt buộc tiếp theo | Cuộc thi đấu? |`j`sau khi xử lý | 
| --- | --- | --- | --- | 
|`h`|`e`| Không | 0 | 
|`a`|`e`| Không | 0 | 
|`r`|`e`| Không | 0 | 
|`d`|`e`| Không | 0 | 
|`c`|`e`| Không | 0 | 
|`o`|`e`| Không | 0 | 
|`n`|`e`| Không | 0 | 
|`t`|`e`| Không | 0 | 
|`e`|`e`| Có | 1 | 
|`s`|`a`| Không | 1 | 
|`t`|`a`| Không | 1 | 

Chuỗi kết thúc trong khi thuật toán vẫn đang chờ`a`. Như vậy`j`chỉ là`1`trong khi`len(t)`là`6`, vậy câu trả lời là`NO`. Điều này cũng chứng tỏ tại sao chỉ cần tìm mọi ký tự ở đâu đó trong`s`là không đủ. Thứ tự cần thiết phải được bảo tồn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Quá trình quét thăm từng ký tự của`s`nhiều nhất một lần và con trỏ đích chỉ di chuyển về phía trước qua`t`. | 
| Không gian | O(1) phụ trợ | Chỉ có con trỏ đích và một vài giá trị vô hướng được duy trì bên cạnh các chuỗi đầu vào. | 

Với cả hai chuỗi được giới hạn ở một triệu ký tự, thuật toán thực hiện tối đa khoảng một triệu lần quét và không bao giờ xây dựng cấu trúc bậc hai. Bản thân các chuỗi đầu vào yêu cầu bộ nhớ tuyến tính, trong khi bộ nhớ bổ sung của thuật toán là không đổi, do đó nó vừa vặn với giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

input = sys.stdin.readline

def solve_data(inp: str) -> str:
    data = inp.splitlines()
    s = data[0]
    t = data[1]

    j = 0

    for c in s:
        if j < len(t) and c == t[j]:
            j += 1
            if j == len(t):
                return "YES\n"

    return "NO\n"

def run(inp: str) -> str:
    return solve_data(inp)

# Provided samples
assert run(
    "threeyellowfurryfiends\n"
    "hellofriend\n"
) == "YES\n", "sample 1"

assert run(
    "hardcontest\n"
    "easyac\n"
) == "NO\n", "sample 2"

# Minimum-size inputs
assert run("a\na\n") == "YES\n", "single equal character"
assert run("a\nb\n") == "NO\n", "single different character"

# Same length, but wrong order
assert run("abc\nacb\n") == "NO\n", "same length requires exact order"

# All equal characters, not enough copies
assert run("aaaa\naaaaa\n") == "NO\n", "target has too many equal characters"

# All equal characters, enough copies
assert run("aaaaa\naaa\n") == "YES\n", "repeated characters"

# Boundary case where the last character is needed
assert run("xabc\na\n") == "YES\n", "match appears after irrelevant prefix"
assert run("abcx\nabc\n") == "YES\n", "target ends exactly at final needed character"

# Maximum-size inputs
assert run("a" * 1_000_000 + "\n" + "a" * 1_000_000 + "\n") == "YES\n", \
    "maximum equal strings"

assert run("a" * 999_999 + "b\n" + "a" * 1_000_000 + "\n") == "NO\n", \
    "maximum-size target requires one unavailable character"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`/`a`|`YES`| Chuỗi tối thiểu có thể và trận chung kết thành công | 
|`a`/`b`|`NO`| Chuỗi tối thiểu có thể không có kết quả khớp | 
|`abc`/`acb`|`NO`| Thứ tự quan trọng ngay cả khi độ dài bằng nhau | 
|`aaaa`/`aaaaa`|`NO`| Ký tự lặp đi lặp lại và số lần xuất hiện không đủ | 
|`aaaaa`/`aaa`|`YES`| Ký tự lặp đi lặp lại với số lần xuất hiện đủ | 
|`abcx`/`abc`|`YES`| Mục tiêu kết thúc trước khi chuỗi nguồn kết thúc | 
|`a^999999b`/`a^1000000`|`NO`| Đầu vào lớn và thiếu một ký tự ở ranh giới | 
|`a^1000000`/`a^1000000`|`YES`| Đầu vào thành công có kích thước tối đa | 

## Vỏ cạnh 

Khi các chuỗi có độ dài bằng 1, thuật toán sẽ thực hiện chính xác một phép so sánh. Đối với đầu vào```
a
a
```

`j`bắt đầu từ số 0, ký tự duy nhất khớp`t[0]`, Và`j`trở thành một, bằng`len(t)`. Kết quả là`YES`. Vì```
a
b
```sự so sánh thất bại,`j`vẫn bằng 0 và quá trình quét kết thúc, đưa ra`NO`. 

Độ dài bằng nhau yêu cầu mọi ký tự của`s`được sử dụng. Vì```
abc
acb
```thuật toán phù hợp`a`, sau đó khớp`c`, sau đó nó cần`b`. Phần còn lại của`s`chỉ là`c`, không thể cung cấp`b`, vậy kết quả là`NO`. Thuật toán không bao giờ giả định rằng độ dài bằng nhau có nghĩa là bằng nhau. 

Các ký tự lặp lại cần có đủ số lần xuất hiện khác biệt. Vì```
aaaa
aaaaa
```con trỏ tiến lên một lần cho mỗi ký tự trong số bốn ký tự trong`s`, đạt`j = 4`. Mục tiêu có độ dài năm, do đó quá trình quét kết thúc với một mục tiêu chưa từng có`a`và trả về`NO`. Vì```
aaaaa
aaa
```thứ ba`a`tiến bộ`j`đến ba, hoàn thành ngay mục tiêu và sản xuất`YES`. 

Mục tiêu có thể kết thúc chính xác ở ký tự cuối cùng của nguồn. Vì```
abcx
abc
```con trỏ tiến triển từ`0`ĐẾN`1`, sau đó`2`, sau đó`3`trong khi đọc`a`,`b`, Và`c`. Từ`j == len(t)`ngay sau khi xử lý`c`, thuật toán trả về`YES`mà không cần một ký tự nguồn khác. Điều kiện biên này là lý do tại sao việc kiểm tra hoàn thành phải diễn ra ngay sau khi di chuyển con trỏ. 

Trường hợp kích thước tối đa hoạt động giống hệt với trường hợp nhỏ. Với một triệu`a`các ký tự trong cả hai chuỗi, mọi ký tự nguồn khớp với ký tự đích hiện tại và con trỏ đích đạt tới một triệu sau lần so sánh cuối cùng. Thuật toán thực hiện công việc tuyến tính thay vì cố gắng so sánh mọi vị trí nguồn với mọi vị trí đích, điều này làm cho giải pháp khả thi ở các giới hạn đã nêu.
