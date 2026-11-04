---
title: "CF 113A - Bài học ngữ pháp"
description: "Chúng ta được cung cấp một dòng duy nhất chứa một số từ viết thường được phân tách bằng dấu cách. Mỗi từ hợp lệ trong ngôn ngữ của Petya thuộc về đúng một phạm trù ngữ pháp và đúng một giới tính, được xác định hoàn toàn bởi hậu tố của nó."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 113
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 86 (Div. 1 Only)"
rating: 1600
weight: 113
solve_time_s: 281
verified: true
draft: false
---

[CF 113A - Bài học ngữ pháp](https://codeforces.com/problemset/problem/113/A) 

**Đánh giá:** 1600 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 4 phút 41 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dòng duy nhất chứa một số từ viết thường được phân tách bằng dấu cách. Mỗi từ hợp lệ trong ngôn ngữ của Petya thuộc về đúng một phạm trù ngữ pháp và đúng một giới tính, được xác định hoàn toàn bởi hậu tố của nó. 

Chỉ có sáu hậu tố pháp lý: 

-`lios`cho tính từ nam tính 
-`liala`cho tính từ nữ tính 
-`etr`đối với danh từ nam tính 
-`etra`đối với danh từ giống cái 
-`initis`cho động từ nam tính 
-`inites`cho động từ nữ tính 

Một câu hợp lệ có cấu trúc rất cứng nhắc. Nó có thể chứa: 

- không có hoặc nhiều tính từ, 
- theo sau đúng một danh từ, 
- theo sau là 0 hoặc nhiều động từ. 

Tất cả các từ phải có cùng giới tính. 

Nhiệm vụ là quyết định xem toàn bộ dòng đầu vào có tạo thành chính xác một câu hợp lệ hay không. 

Tổng kích thước đầu vào tối đa là$10^5$ký tự, do đó giải pháp phải xử lý chuỗi theo thời gian tuyến tính. Bất kỳ cách tiếp cận nào liên tục quét các tiền tố hoặc tính toán lại các phân loại cho cùng một từ vẫn có thể đạt được trong thực tế, nhưng không cần bất cứ điều gì ngoài một lần chuyển từ trái sang phải. Sạch sẽ$O(n)$giải pháp là đủ. 

Phần khó khăn không phải là hiệu suất, mà là việc xác thực ngữ pháp một cách chính xác. 

Một sai lầm dễ mắc phải là chấp nhận đầu vào có nhiều danh từ. Một câu phải chứa đúng một danh từ. 

Ví dụ:```
etr etr
```Đầu ra đúng là:```
NO
```Việc triển khai bất cẩn chỉ kiểm tra tính hợp lệ của hậu tố và tính nhất quán về giới tính sẽ chấp nhận nó một cách sai lầm. 

Một trường hợp tế nhị khác là đặt hàng. Tính từ phải đứng trước danh từ và động từ phải đứng sau nó. 

Ví dụ:```
initis etr
```Đầu ra đúng là:```
NO
```Mặc dù cả hai từ đều giống đực và có giá trị riêng lẻ nhưng động từ vẫn xuất hiện trước danh từ. 

Giới tính hỗn hợp cũng phải bị từ chối ngay lập tức. 

Ví dụ:```
lios etra
```Tính từ là nam tính còn danh từ là nữ tính nên đáp án là:```
NO
```Trường hợp cuối cùng là bản thân một từ đã là một câu hợp lệ, miễn là từ đó thuộc về ngôn ngữ. 

Ví dụ:```
inites
```Điều này hợp lý vì chỉ một động từ vẫn phù hợp với ngữ pháp: không tính từ, không danh từ? Không. Một câu lệnh yêu cầu chính xác một danh từ, nhưng bài toán cũng cho biết một câu có thể chỉ là một từ ngôn ngữ hợp lệ. Từ`inites`là một từ hợp lệ, câu trả lời là:```
YES
```Quy tắc đặc biệt này rất dễ bị bỏ qua. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp nhất là phân loại từng từ, sau đó thử mọi vị trí có thể có của danh từ. Đối với mỗi vị trí danh từ ứng cử viên, chúng tôi sẽ xác minh rằng tất cả các từ trước đó là tính từ, tất cả các từ sau là động từ và mọi từ đều có cùng giới tính. 

Nếu có$n$từ, điều này đòi hỏi phải kiểm tra lên đến$n$vị trí của danh từ và mỗi lần kiểm tra có thể quét lại toàn bộ câu. Tổng độ phức tạp trở thành$O(n^2)$. 

Với$10^5$ký tự, số lượng từ cũng có thể lớn. Hành vi bậc hai là không cần thiết và có nguy cơ gây ra các vấn đề về thời gian trong Python nếu đầu vào chứa nhiều từ ngắn. 

Điều quan trọng cần lưu ý là ngữ pháp xác định một trật tự chặt chẽ giữa ba phần của lời nói:```
adjective < noun < verb
```Một câu hợp lệ phải chứa đúng một danh từ và thứ tự các loại không bao giờ được giảm đi. 

Khi mỗi từ được phân loại thành: 

- giới tính, 
- loại ngữ pháp, 

vấn đề trở thành một phép quét tuyến tính đơn giản. 

Chúng tôi xử lý các từ từ trái sang phải đồng thời kiểm tra ba điều kiện: 

- mọi từ đều thuộc về ngôn ngữ, 
- tất cả các giới tính phù hợp, 
- Các loại ngữ pháp xuất hiện theo thứ tự không giảm dần và chứa đúng một danh từ. 

Điều này làm giảm vấn đề xuống$O(n)$thời gian với bộ nhớ bổ sung liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia dòng đầu vào thành các từ. 

Mỗi từ phải được phân tích độc lập vì hậu tố của nó xác định đầy đủ vai trò của nó trong ngôn ngữ. 
2. Đối với mỗi từ, hãy xác định xem nó có khớp với một trong sáu hậu tố hợp pháp hay không. 

Nếu không có hậu tố nào phù hợp thì toàn bộ câu sẽ vô hiệu ngay lập tức. 
3. Chuyển mỗi từ thành hai thuộc tính: 

- Giới tính: nam hay nữ, 
- Loại: tính từ, danh từ hoặc động từ. 

Chúng ta có thể mã hóa các loại bằng số: 

- tính từ = 0 
- danh từ = 1 
- động từ = 2 

Điều này làm cho việc kiểm tra đơn hàng trở nên rất đơn giản. 
4. Nếu câu có đúng một từ và từ đó đúng thì in ra`"YES"`. 

Bài toán cho phép một từ hợp lệ trở thành một câu hoàn chỉnh một cách rõ ràng, bất kể đó có phải là danh từ hay không. 
5. Mặt khác, hãy xác minh rằng tất cả các từ đều có cùng giới tính. 

Nếu thậm chí một từ khác nhau, hãy in`"NO"`. 
6. Đếm xem có bao nhiêu danh từ xuất hiện. 

Một câu lệnh nhiều từ phải chứa chính xác một danh từ. 
7. Quét theo thứ tự các loại ngữ pháp từ trái sang phải. 

Các loại không bao giờ được giảm. Nói cách khác: 

- Theo sau tính từ có thể là tính từ hoặc danh từ 
- theo sau danh từ có thể là động từ, 
- Động từ chỉ được theo sau bởi động từ. 

Nếu loại nhỏ hơn xuất hiện sau loại lớn hơn thì trật tự ngữ pháp sẽ bị hỏng. 
8. Nếu tất cả các bước kiểm tra đều đạt, hãy in`"YES"`. 

### Tại sao nó hoạt động 

Mọi câu lệnh hợp lệ trong ngôn ngữ đều có cấu trúc chính xác như sau:```
(adjectives) noun (verbs)
```Mã hóa tính từ, danh từ và động từ dưới dạng`0`,`1`, Và`2`biến quy tắc ngữ pháp thành điều kiện đơn điệu. Một câu hợp lệ tương ứng chính xác với một chuỗi không giảm có chứa một danh từ. 

Quy tắc về giới tính là độc lập và mang tính toàn cầu, vì vậy việc kiểm tra xem tất cả các từ có cùng giới tính là đủ hay không. 

Ngoại lệ một từ được xử lý riêng vì ngữ pháp cho phép bất kỳ từ hợp lệ riêng lẻ nào tạo thành một câu, ngay cả khi không có danh từ. 

Vì mọi sự không hợp lệ có thể xảy ra đều được phát hiện bởi một trong những lần kiểm tra này nên thuật toán là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    words = input().split()

    suffixes = [
        ("lios", "M", 0),
        ("liala", "F", 0),
        ("etr", "M", 1),
        ("etra", "F", 1),
        ("initis", "M", 2),
        ("inites", "F", 2),
    ]

    parsed = []

    for word in words:
        found = False

        for suf, gender, typ in suffixes:
            if word.endswith(suf):
                parsed.append((gender, typ))
                found = True
                break

        if not found:
            print("NO")
            return

    if len(words) == 1:
        print("YES")
        return

    genders = [g for g, _ in parsed]
    types = [t for _, t in parsed]

    if len(set(genders)) != 1:
        print("NO")
        return

    if types.count(1) != 1:
        print("NO")
        return

    for i in range(1, len(types)):
        if types[i] < types[i - 1]:
            print("NO")
            return

    print("YES")

solve()
```Phần đầu tiên của quá trình triển khai sẽ phân loại từng từ bằng cách kiểm tra xem nó kết thúc bằng hậu tố nào. Vì danh sách hậu tố rất nhỏ và cố định nên chỉ cần một vòng lặp đơn giản là đủ. 

các`parsed`mảng lưu trữ các cặp`(gender, type)`cho mỗi từ. Việc sử dụng số nguyên cho các loại sẽ đơn giản hóa việc đặt hàng kiểm tra sau này. 

Trường hợp đặc biệt gồm một từ phải xuất hiện trước khi xác thực số danh từ. Nếu không thì những từ như`inites`hoặc`lios`sẽ thất bại một cách không chính xác vì chúng không phải là danh từ. 

Việc kiểm tra giới tính sử dụng một bộ. Nếu có nhiều hơn một giới tính riêng biệt tồn tại thì câu đó không hợp lệ. 

Số lượng danh từ phải chính xác bằng một đối với các câu có nhiều từ. Điều này bắt những trường hợp như có hai danh từ hoặc không có danh từ. 

Cuối cùng, điều kiện thứ tự được xác minh bằng cách kiểm tra xem chuỗi loại không bao giờ giảm. Điều này thực thi chặt chẽ cấu trúc ngữ pháp mà không chia câu một cách rõ ràng thành các phân đoạn tính từ, danh từ và động từ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
petr
```| Lời | Kết hợp hậu tố | Giới tính | Loại | 
| --- | --- | --- | --- | 
| petr | vv | M | danh từ | 

Câu chỉ chứa một từ hợp lệ nên sẽ tự động được chấp nhận. 

Đầu ra:```
YES
```Ví dụ này thể hiện quy tắc một từ. Mặc dù ngữ pháp câu lệnh chung yêu cầu chính xác một danh từ được bao quanh bởi các tính từ và động từ tùy chọn, nhưng vấn đề riêng biệt cho phép bất kỳ từ hợp lệ nào tạo thành một câu. 

### Ví dụ 2 

đầu vào:```
lios lios etr initis
```| Lời | Kết hợp hậu tố | Giới tính | Loại | 
| --- | --- | --- | --- | 
| sư tử | sư tử | M | tính từ | 
| sư tử | sư tử | M | tính từ | 
| vv | vv | M | danh từ | 
| viêm | viêm | M | động từ | 

Trình tự loại được trích xuất là:```
0 0 1 2
```Dãy này không giảm và chứa đúng một danh từ. 

Đầu ra:```
YES
```Dấu vết này xác nhận tính bất biến trung tâm của nghiệm. Các câu lệnh hợp lệ tương ứng với một chuỗi các phạm trù ngữ pháp không giảm dần. 

### Ví dụ 3 

đầu vào:```
etr lios
```| Lời | Kết hợp hậu tố | Giới tính | Loại | 
| --- | --- | --- | --- | 
| vv | vv | M | danh từ | 
| sư tử | sư tử | M | tính từ | 

Trình tự loại là:```
1 0
```Trình tự giảm dần, nghĩa là tính từ xuất hiện sau danh từ. 

Đầu ra:```
NO
```Ví dụ này cho thấy tại sao chỉ kiểm tra giới tính và số lượng danh từ là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi từ được phân loại một lần | 
| Không gian | O(n) | lưu trữ thông tin từ được phân tích cú pháp | 

Đây,$n$là số từ trong câu. Vì tổng chiều dài đầu vào tối đa là$10^5$, quét tuyến tính dễ dàng phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ cũng nhỏ vì chúng tôi chỉ lưu trữ một vài số nguyên và chuỗi cho mỗi từ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        words = input().split()

        suffixes = [
            ("lios", "M", 0),
            ("liala", "F", 0),
            ("etr", "M", 1),
            ("etra", "F", 1),
            ("initis", "M", 2),
            ("inites", "F", 2),
        ]

        parsed = []

        for word in words:
            found = False

            for suf, gender, typ in suffixes:
                if word.endswith(suf):
                    parsed.append((gender, typ))
                    found = True
                    break

            if not found:
                return "NO"

        if len(words) == 1:
            return "YES"

        genders = [g for g, _ in parsed]
        types = [t for _, t in parsed]

        if len(set(genders)) != 1:
            return "NO"

        if types.count(1) != 1:
            return "NO"

        for i in range(1, len(types)):
            if types[i] < types[i - 1]:
                return "NO"

        return "YES"

    return solve()

# provided sample
assert run("petr\n") == "YES", "sample 1"

# custom cases
assert run("inites\n") == "YES", "single valid non-noun word"

assert run("lios etr initis\n") == "YES", "valid full sentence"

assert run("etr etr\n") == "NO", "multiple nouns"

assert run("etr lios\n") == "NO", "wrong grammatical order"

assert run("lios etra\n") == "NO", "mixed genders"

assert run("abc\n") == "NO", "invalid suffix"

long_input = " ".join(["lios"] * 50000 + ["etr"]) + "\n"
assert run(long_input) == "YES", "large input stress test"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`inites`|`YES`| ngoại lệ một từ | 
|`lios etr initis`|`YES`| cấu trúc tính từ-danh từ-động từ hợp lệ | 
|`etr etr`|`NO`| chính xác một danh từ được yêu cầu | 
|`etr lios`|`NO`| hạn chế đặt hàng | 
|`lios etra`|`NO`| tính nhất quán về giới tính | 
|`abc`|`NO`| từ ngôn ngữ không hợp lệ | 
| tính từ lặp đi lặp lại lớn |`YES`| hiệu suất tuyến tính | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
etr etr
```Thuật toán phân loại cả hai từ là danh từ nam tính, tạo ra chuỗi loại:```
1 1
```Điều kiện thứ tự được thỏa vì dãy không giảm. Việc kiểm tra quan trọng là số lượng danh từ. Vì có hai danh từ thay vì một nên thuật toán sẽ in chính xác:```
NO
```Bây giờ hãy xem xét:```
initis etr
```Các loại trở thành:```
2 1
```Trong quá trình quét từ trái sang phải, thuật toán sẽ phát hiện ra rằng`1 < 2`, nghĩa là trình tự giảm dần. Điều này vi phạm thứ tự tính từ-danh từ-động từ bắt buộc, nên câu trả lời là:```
NO
```Đối với giới tính hỗn hợp:```
lios etra
```Biểu diễn được phân tích cú pháp là:```
(M, adjective)
(F, noun)
```Bộ giới tính chứa cả hai`M`Và`F`, nên câu bị bác bỏ ngay lập tức. 

Cuối cùng, hãy kiểm tra trường hợp từ đơn đặc biệt:```
inites
```Từ này được công nhận là một động từ nữ tính. Thông thường, một câu lệnh sẽ yêu cầu một danh từ, nhưng bài toán riêng biệt cho phép bất kỳ từ hợp lệ nào cũng có thể trở thành một câu. Thuật toán kiểm tra`len(words) == 1`trước khi thực thi việc đếm danh từ, đưa ra câu trả lời đúng:```
YES
```
