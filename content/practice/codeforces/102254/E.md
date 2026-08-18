---
title: "CF 102254E - Giờ viết luận"
description: "Chúng ta có một dãy n từ, theo đúng thứ tự chúng xuất hiện trong bài luận. Một từ chỉ quan trọng khi độ dài của nó ít nhất là bốn. Đối với mỗi từ như vậy, sự xuất hiện đầu tiên được cho phép, trong khi mọi lần xuất hiện sau đó của cùng một từ phải bị xóa."
date: "2026-08-17T21:10:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102254
codeforces_index: "E"
codeforces_contest_name: "IME++ Starters Try-outs 2019"
rating: 0
weight: 102254
solve_time_s: 214
verified: false
draft: false
---

[CF 102254E - Thời gian viết luận](https://codeforces.com/problemset/problem/102254/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 34 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi`n`các từ, theo đúng thứ tự chúng xuất hiện trong bài luận. Một từ chỉ quan trọng khi độ dài của nó ít nhất là bốn. Đối với mỗi từ như vậy, sự xuất hiện đầu tiên được cho phép, trong khi mọi lần xuất hiện sau đó của cùng một từ phải bị xóa. 

Đầu ra là số lần xuất hiện phải xóa, theo sau là các từ đó theo thứ tự xuất hiện lặp lại của chúng. Nếu không có gì phải xóa, đầu ra được yêu cầu là`SAFO`thay vì. 

Sự khác biệt chính là giữa một từ được lặp lại và một sự việc được lặp lại. Ví dụ: nếu đầu vào chứa`clean clean clean`, từ`clean`xuất hiện ba lần, nhưng chỉ có lần xuất hiện thứ hai và thứ ba mới có trong câu trả lời. Do đó, đầu ra là`2`, theo sau là hai bản sao của`clean`. 

Các ràng buộc làm cho cách tiếp cận bậc hai không thể thực hiện được. Có thể có nhiều như`8 * 10^6`từ đầu vào và tổng số ký tự cũng nhiều nhất`8 * 10^6`. Do đó, giải pháp sẽ xử lý từng từ đầu vào với số lần không đổi thay vì quét liên tục tất cả các từ trước đó. Giới hạn tổng ký tự đặc biệt hữu ích vì các hoạt động như đọc, băm và so sánh các từ có thể được thực hiện tỷ lệ thuận với tổng kích thước đầu vào cho đến chi phí băm thông thường. 

Có một số trường hợp nguy hiểm có thể âm thầm phá vỡ việc triển khai bất cẩn. Đầu tiên, những từ ngắn hơn bốn ký tự không tham gia vào quy tắc lặp lại. Ví dụ,```
3
cat
cat
dog
```không có từ lặp lại theo quy tắc của bài toán, vì vậy kết quả đầu ra là```
SAFO
```Giải pháp chèn từng từ vào cấu trúc phát hiện trùng lặp sẽ báo cáo sai`cat`. 

Thứ hai, không được in lần xuất hiện đầu tiên của một từ dài. Vì```
3
clean
bad
clean
```đầu ra là```
1
clean
```đầu tiên`clean`xác định rằng từ đó đã xuất hiện, trong khi chỉ có từ thứ hai là một lần xuất hiện phải xóa. 

Thứ ba, một từ xuất hiện ba lần trở lên phải được in một lần cho mỗi lần xuất hiện sau lần đầu tiên. Vì```
4
enough
enough
enough
enough
```đầu ra là```
3
enough
enough
enough
```Một giải pháp chỉ ghi lại những từ trùng lặp sẽ in không chính xác`enough`chỉ một lần. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là so sánh từng từ dài với tất cả các từ dài trước đó. Khi xử lý các`i`-từ thứ, quét vị trí`1`bởi vì`i-1`và kiểm tra xem một từ giống hệt đã xuất hiện chưa. Nếu tìm thấy, hãy in hoặc ghi lại từ hiện tại dưới dạng lặp lại. Điều này đúng vì một từ được lặp lại chính xác khi một từ tương đương tồn tại ở đâu đó trước đó trong đầu vào. 

Vấn đề là số lượng so sánh. Nếu tất cả`n`các từ dài và khác biệt, thuật toán sẽ thực hiện`0 + 1 + 2 + ... + (n - 1) = n(n - 1)/2`so sánh từ ngữ. Với`n = 8 * 10^6`, đây là về`3.2 * 10^13`so sánh. Ngay cả khi mỗi phép so sánh được coi là thời gian không đổi thì thời gian đó vẫn vượt xa giới hạn thời gian hai giây. So sánh chuỗi thực cũng có thể kiểm tra nhiều ký tự, khiến tình hình trở nên tồi tệ hơn. 

Quan sát giúp mở ra giải pháp nhanh hơn là chúng ta không cần biết vị trí nào trước đó có chứa một từ. Chúng ta chỉ cần một chút thông tin: từ này đã xuất hiện trước đây chưa? Một bộ băm cung cấp chính xác hoạt động đó. Đối với mỗi từ dài, hãy kiểm tra xem nó đã có trong bộ chưa. Nếu đúng như vậy thì lần xuất hiện hiện tại là sự xuất hiện lặp lại. Nếu không, hãy chèn nó vào tập hợp để các lần xuất hiện trong tương lai sẽ được nhận ra. 

Những từ ngắn có thể bị bỏ qua hoàn toàn. Đây không chỉ là một sự tối ưu hóa nhỏ: nó trực tiếp thực hiện quy tắc rằng chỉ những từ có độ dài từ bốn trở lên mới bị hạn chế. 

Vì đầu vào phải được xử lý theo thứ tự nên chúng ta có thể phát hiện sự xuất hiện lặp lại tại thời điểm nó được đọc. Trước tiên, chúng tôi vẫn cần in số lần xuất hiện lặp lại để quá trình triển khai rõ ràng ghi lại các từ lặp lại trong khi quét. Tổng chiều dài đầu vào tối đa là`8 * 10^6`, do đó việc lưu trữ các từ lặp lại trong bộ đệm byte sẽ không tốn kém so với việc lưu trữ tất cả các từ riêng biệt trong tập hợp. 

Kết quả so sánh là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | So sánh từ O(n2), lên đến O(n2S) với so sánh cấp độ ký tự | O(nS) | Quá chậm | 
| Bộ băm | Tổng số băm và xử lý đầu vào O(S) dự kiến ​​| O(S) cho các từ riêng biệt được lưu trữ và bộ đệm đầu ra | Đã chấp nhận | 

Đây`S`biểu thị tổng số ký tự trong tất cả các từ đầu vào, với`S <= 8 * 10^6`. Giới hạn tuyến tính dự kiến ​​cho giải pháp tập hợp hàm băm giả định hành vi trong trường hợp trung bình thông thường của bảng băm. 

## Hướng dẫn thuật toán 

1. Đọc số từ và tạo một tập hợp trống có tên`seen`. Bộ này sẽ chứa chính xác những từ dài đã xảy ra. 
2. Tạo bộ đếm`repeated_count`và một bộ đệm byte đầu ra. Bộ đếm cho chúng ta biết số lần xuất hiện phải xóa, trong khi bộ đệm cho phép chúng ta trì hoãn việc in cho đến khi biết bộ đếm. 
3. Đọc từng từ theo thứ tự ban đầu. Chỉ xóa phần cuối dòng để bản thân từ đó không thay đổi. 
4. Nếu từ có ít hơn bốn ký tự, hãy bỏ qua nó. Một từ như vậy được phép xuất hiện nhiều lần và không bao giờ được phép nhập`seen`. 
5. Đối với một từ có độ dài ít nhất là bốn, hãy kiểm tra xem nó đã có sẵn chưa`seen`. Nếu có thì tăng`repeated_count`và nối thêm lần xuất hiện này vào bộ đệm đầu ra. Chúng tôi nối thêm mỗi lần xuất hiện lặp lại, không chỉ là bản sao đầu tiên. 
6. Nếu từ đó chưa có trong`seen`, chèn nó. Từ thời điểm này trở đi, bất kỳ từ tương đương nào sau này sẽ được phân loại chính xác là từ lặp lại. 
7. Sau khi tất cả các từ đã được xử lý, hãy in`SAFO`nếu như`repeated_count`là số không. Nếu không, hãy in số đếm theo sau là các từ lặp lại được đệm, mỗi từ một dòng. 

### Tại sao nó hoạt động 

Sau khi xử lý bất kỳ tiền tố nào của đầu vào, bất biến là`seen`chứa chính xác các từ riêng biệt có độ dài ít nhất bốn từ xuất hiện trong tiền tố đó. Đối với từ dài tiếp theo, tư cách thành viên trong`seen`do đó tương đương với việc có sự xuất hiện bằng nhau trước đó. Nếu có từ đó thì sự xuất hiện hiện tại phải bị xóa và thêm vào câu trả lời. Nếu nó vắng mặt thì đây là lần xuất hiện đầu tiên của nó nên việc chèn nó vào là đúng. Các từ ngắn không bao giờ ảnh hưởng đến bất biến vì quy tắc không áp dụng cho chúng. Vì các từ được xử lý từ trái sang phải nên mọi lần xuất hiện được báo cáo sẽ xuất hiện theo đúng thứ tự bắt buộc và mọi lần xuất hiện sau lần đầu tiên được báo cáo chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    seen = set()
    repeated_count = 0
    repeated = bytearray()

    for _ in range(n):
        word = input().strip()

        if len(word) < 4:
            continue

        if word in seen:
            repeated_count += 1
            repeated.extend(word)
            repeated.append(10)  # '\n'
        else:
            seen.add(word)

    out = sys.stdout.buffer

    if repeated_count == 0:
        out.write(b"SAFO\n")
    else:
        out.write(str(repeated_count).encode())
        out.write(b"\n")
        out.write(repeated)

if __name__ == "__main__":
    solve()
```các`seen`tập hợp tương ứng trực tiếp với phần đầu tiên của thuật toán. Bộ của Python cung cấp khả năng chèn và kiểm tra tư cách thành viên theo thời gian không đổi dự kiến, vì vậy mọi từ có liên quan đều có thể được xử lý mà không cần quét các từ trước đó. 

Giải pháp sử dụng`input = sys.stdin.readline`theo yêu cầu, tránh việc sử dụng lặp lại các cơ chế đầu vào cấp cao hơn với tốc độ chậm hơn nhiều. Các từ được giữ dưới dạng chuỗi trong tập hợp, trong khi đầu ra lặp lại được tích lũy trong một`bytearray`. Việc sử dụng bộ đệm byte sẽ tránh tạo chuỗi Python riêng cho đầu ra hoàn chỉnh. 

Thứ tự của các hoạt động là đáng kể. Việc kiểm tra tư cách thành viên diễn ra trước khi chèn. Nếu chúng ta chèn từ trước rồi kiểm tra tư cách thành viên thì mọi từ dài sẽ xuất hiện như một bản sao của chính nó. Trình tự đúng là kiểm tra xem nó có được nhìn thấy hay không, báo cáo nếu có và nếu không thì chèn nó vào. 

Việc kiểm tra độ dài sử dụng`< 4`, bởi vì một từ có độ dài đúng bằng bốn sẽ bị hạn chế. Một từ có độ dài bằng ba sẽ bị bỏ qua. 

Không có vấn đề tràn số nguyên trong Python. Bộ đếm cũng có thể biểu diễn một cách an toàn số lần xuất hiện lặp lại tối đa có thể. 

Điều đặc biệt`SAFO`đầu ra được xử lý riêng vì đầu ra được yêu cầu không`0`theo sau là một danh sách trống. Khi có ít nhất một lần lặp lại, dòng đầu tiên là số đếm và các dòng tiếp theo chứa số lần lặp lại. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, trạng thái xử lý là: 

| Lời | Chiều dài ít nhất là 4? | TRONG`seen`trước khi xử lý? | Hành động |`repeated_count`| 
| --- | --- | --- | --- | --- | 
|`not`| Không | Không | Bỏ qua | 0 | 
|`clean`| Có | Không | Chèn | 0 | 
|`bad`| Không | Không | Bỏ qua | 0 | 
|`posture`| Có | Không | Chèn | 0 | 
|`clean`| Có | Có | Ghi lại sự lặp lại | 1 | 
|`enough`| Có | Không | Chèn | 1 | 

đầu tiên`clean`được chèn vào`seen`. Khi thứ hai`clean`đến, tư cách thành viên thành công, do đó chỉ lần xuất hiện thứ hai được ghi lại. Đầu ra cuối cùng là:```
1
clean
```Đối với Mẫu 2, trạng thái trở thành: 

| Lời | Chiều dài ít nhất là 4? | TRONG`seen`trước khi xử lý? | Hành động |`repeated_count`| 
| --- | --- | --- | --- | --- | 
|`not`| Không | Không | Bỏ qua | 0 | 
|`clean`| Có | Không | Chèn | 0 | 
|`enough`| Có | Không | Chèn | 0 | 
|`bad`| Không | Không | Bỏ qua | 0 | 
|`posture`| Có | Không | Chèn | 0 | 
|`clean`| Có | Có | Ghi lại sự lặp lại | 1 | 
|`enough`| Có | Có | Ghi lại sự lặp lại | 2 | 
|`enough`| Có | Có | Ghi lại sự lặp lại | 3 | 

Hai lần xuất hiện sau đó của`enough`đều được báo cáo. Bộ này không loại bỏ hoặc thay đổi`enough`sau lần trùng lặp đầu tiên, do đó mọi lần xuất hiện tiếp theo đều tiếp tục được ghi nhận. Đầu ra là:```
3
clean
enough
enough
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(S) dự kiến ​​| Mỗi từ được đọc một lần và mọi từ có liên quan đều được băm và kiểm tra một lần. | 
| Không gian | O(S) | Những từ dài khác biệt trong`seen`chứa tối đa O(S) ký tự và đầu ra lặp lại cũng có nhiều nhất O(S) ký tự. | 

Đây`S`là tổng độ dài của tất cả các từ và nhiều nhất là`8 * 10^6`. Do đó, giải pháp sẽ chia tỷ lệ theo kích thước đầu vào thực tế thay vì bình phương số từ. Giới hạn bộ nhớ là 1024 MB, cung cấp cho Python đủ chỗ cho bộ băm và bộ đệm đầu ra nhỏ gọn cho giới hạn đầu vào này. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    n = int(input())

    seen = set()
    repeated_count = 0
    repeated = bytearray()

    for _ in range(n):
        word = input().strip()

        if len(word) < 4:
            continue

        if word in seen:
            repeated_count += 1
            repeated.extend(word.encode())
            repeated.append(10)
        else:
            seen.add(word)

    out = sys.stdout
    if repeated_count == 0:
        out.write("SAFO\n")
    else:
        out.write(str(repeated_count) + "\n")
        out.write(repeated.decode())

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = sys.stdin.readline

# Provided sample 1
assert run(
    """6
not
clean
bad
posture
clean
enough
"""
) == "1\nclean\n", "sample 1"

# Provided sample 2
assert run(
    """8
not
clean
enough
bad
posture
clean
enough
enough
"""
) == "3\nclean\nenough\nenough\n", "sample 2"

# Minimum size
assert run(
    """1
abcd
"""
) == "SAFO\n", "minimum-size input"

# Short words are never considered repeated
assert run(
    """6
a
a
abc
abc
abcd
abcd
"""
) == "1\nabcd\n", "short words must be ignored"

# All equal long words: every occurrence after the first is repeated
assert run(
    """5
word
word
word
word
word
"""
) == "4\nword\nword\nword\nword\n", "all equal values"

# Boundary around length four, including several distinct words
assert run(
    """7
aaa
aaaa
aaa
aaaa
bbbb
bbbb
ccc
"""
) == "2\naaaa\nbbbb\n", "length-four boundary"

# Maximum n permitted by the constraints. All words have length one,
# so the total input length is 8 * 10^6 and none of them are relevant.
max_n = 8_000_000
maximum_input = str(max_n) + "\n" + ("a\n" * max_n)
assert run(maximum_input) == "SAFO\n", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / abcd`|`SAFO`| Xử lý đầu vào tối thiểu và lần xuất hiện đầu tiên | 
| lặp đi lặp lại`a`Và`abc`, lặp lại`abcd`|`1 / abcd`| Những từ ngắn hơn bốn ký tự phải được bỏ qua | 
| Năm bản sao của`word`| Bốn bản sao của`word`| Mỗi lần xảy ra sau lần đầu tiên đều phải được báo cáo | 
|`aaa`,`aaaa`,`bbbb`,`ccc`kết hợp |`2 / aaaa / bbbb`| Ranh giới chính xác gồm bốn ký tự | 
| Tám triệu từ một ký tự |`SAFO`| Tối đa`n`và ràng buộc tổng chiều dài | 

Xác nhận kích thước tối đa có chủ ý sử dụng các từ có một ký tự. Tám triệu từ có độ dài bốn sẽ vi phạm giới hạn tổng chiều dài, vì điều đó sẽ yêu cầu 32 triệu ký tự. Tám triệu từ một ký tự thỏa mãn giới hạn và cũng xác minh rằng các từ ngắn không liên quan có thể được xử lý mà không cần nhập bộ băm. 

## Vỏ cạnh 

Một từ ngắn lặp đi lặp lại không được phép báo cáo. Vì```
3
cat
cat
dog
```cả hai bản sao của`cat`có độ dài bằng ba, do đó thuật toán đạt đến việc kiểm tra độ dài và ngay lập tức bỏ qua từng cái.`seen`vẫn trống rỗng,`repeated_count`vẫn bằng 0 và đầu ra là:```
SAFO
```Điều này mắc phải lỗi phổ biến khi diễn giải "từ lặp lại" mà không áp dụng giới hạn bốn ký tự. 

Một từ có chính xác bốn ký tự phải được coi là một từ có liên quan. Vì```
2
aaaa
aaaa
```cái đầu tiên`aaaa`được chèn vào`seen`. Thứ hai được tìm thấy ở đó và được ghi lại. Kết quả là:```
1
aaaa
```các`< 4`điều kiện là điều làm cho độ dài ba và độ dài bốn hoạt động khác nhau. 

Một từ được lặp lại nhiều hơn hai lần phải tạo ra nhiều mục đầu ra. Vì```
4
same
same
same
same
```lần xuất hiện đầu tiên đi vào`seen`, trong khi ba người tiếp theo đều tìm thấy`same`đã có mặt rồi. Bộ đếm đạt tới ba và bộ đệm chứa ba bản sao, tạo ra:```
3
same
same
same
```Đây là lý do tại sao chỉ riêng bộ dữ liệu là không đủ để mô tả đầu ra. Nó cho chúng ta biết một từ đã xuất hiện hay chưa, trong khi bộ đếm và bộ đệm đầu ra theo dõi mỗi lần xuất hiện sau đó. 

Cuối cùng, thứ tự đầu ra theo trực tiếp từ quá trình quét từ trái sang phải. Vì```
5
alpha
beta
alpha
beta
alpha
```cái đầu tiên`alpha`Và`beta`được chèn vào. Từ đầu vào thứ ba tạo ra`alpha`, thứ tư tạo ra`beta`, và thứ năm tạo ra`alpha`lại. Kết quả là:```
3
alpha
beta
alpha
```Thuật toán không bao giờ sắp xếp các từ lặp lại và không bao giờ nhóm các từ giống nhau lại với nhau. Nó ghi lại chúng tại thời điểm chúng xảy ra lặp đi lặp lại, giúp duy trì chính xác thứ tự mà vấn đề yêu cầu.
