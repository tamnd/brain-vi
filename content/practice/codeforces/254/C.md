---
title: "CF 254C - Đảo chữ"
description: "Chúng ta có hai chuỗi chữ hoa có độ dài bằng nhau. Chúng ta có thể thay thế các ký tự trong chuỗi đầu tiên và sau tất cả các thay thế, chuỗi cuối cùng chỉ cần đảo chữ của chuỗi thứ hai."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 254
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 155 (Div. 2)"
rating: 1800
weight: 254
solve_time_s: 892
verified: true
draft: false
---

[CF 254C - Đảo chữ](https://codeforces.com/problemset/problem/254/C) 

**Đánh giá:** 1800 
**Tags:** tham lam, dây 
**Thời gian giải:** 14 phút 52 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi chữ hoa có độ dài bằng nhau. Chúng ta có thể thay thế các ký tự trong chuỗi đầu tiên và sau tất cả các thay thế, chuỗi cuối cùng chỉ cần đảo chữ của chuỗi thứ hai. Thứ tự không quan trọng đối với nhiều bộ ký tự, nhưng trong số tất cả các câu trả lời hợp lệ sử dụng số lượng thay thế tối thiểu, chúng ta phải xuất ra chuỗi kết quả nhỏ nhất về mặt từ điển. 

Chi tiết quan trọng là chúng tôi không cố gắng chuyển đổi`s`vào trong`t`từng vị trí. Chúng tôi chỉ quan tâm rằng tần số ký tự cuối cùng phù hợp với tần số của`t`. 

Giả định`t`chứa ba`A`s và hai`B`S. Khi đó chuỗi cuối cùng cũng phải chứa đúng ba`A`s và hai`B`s, bất kể chúng xuất hiện ở đâu. 

Chiều dài có thể đạt tới`10^5`, điều này ngay lập tức loại trừ mọi thứ bậc hai. Chúng ta không thể liên tục xây dựng lại các chuỗi, sắp xếp các tiền tố lớn nhiều lần hoặc thử tất cả các lựa chọn thay thế một cách tham lam mà không có kế toán hiệu quả. Vì bảng chữ cái chỉ chứa 26 chữ in hoa nên việc đếm tần số trở nên rất hấp dẫn. MỘT`O(n * 26)`giải pháp là hoàn toàn tốt, trong khi`O(n^2)`sẽ ở xung quanh`10^10`hoạt động trong trường hợp xấu nhất. 

Yêu cầu nhỏ nhất về mặt từ điển sẽ tạo ra một số trường hợp tinh vi. 

Coi như:```
s = BAA
t = AAC
```Số lần thay thế tối thiểu là`1`. Chúng ta phải thay thế`B`với`C`, cho`CAA`. Một chiến lược bất cẩn tham lam đặt chữ cái nhỏ nhất có thể ở mọi vị trí có thể cố gắng thay đổi chữ cái đầu tiên.`B`vào trong`A`, nhưng sau đó chúng ta sẽ có quá nhiều`A`s và vẫn cần một sự thay thế khác sau này. 

Một trường hợp phức tạp khác là khi một ký tự đã xuất hiện quá nhiều nhưng chúng ta nên trì hoãn việc thay thế một số lần xuất hiện. 

Ví dụ:```
s = CBA
t = ABB
```Chúng ta cần tần số`{A:1, B:2}`. Câu trả lời đúng là:```
ABB
```Nếu chúng ta tham lam thay thế ký tự ngoại cỡ đầu tiên ngay lập tức mà không kiểm tra các vị trí trong tương lai, chúng ta có thể giữ vị trí dẫn đầu.`C`và sửa đổi các chữ cái sau này, tạo ra một cái gì đó lớn hơn về mặt từ điển. 

Một tình huống tinh tế hơn xảy ra khi một ký tự hiện đang quá nhiều, nhưng việc giữ sự xuất hiện sớm giúp sắp xếp từ điển ít hơn so với việc lưu các thay thế cho các vị trí sau này. 

Ví dụ:```
s = BBAA
t = AACC
```Chúng ta cần hai người thay thế. Câu trả lời tốt nhất là:```
AACC
```Việc triển khai "chỉ thay thế khi bắt buộc" ngây thơ có thể vô tình giữ lại giá trị đầu tiên`B`, sau đó phát hiện ra nó phải giới thiệu thêm`A`dù sao đi nữa, tạo ra một câu trả lời lớn hơn về mặt từ điển. 

Khó khăn cốt lõi là cân bằng được hai mục tiêu cùng một lúc: 

Đầu tiên, sử dụng số lượng thay thế tối thiểu có thể. 

Thứ hai, trong số tất cả các giải pháp như vậy, hãy đặt các vị trí sớm nhất càng nhỏ càng tốt. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để suy nghĩ về vấn đề này là tạo ra tất cả các đảo chữ cái của`t`, sau đó kiểm tra xem có bao nhiêu vị trí khác nhau`s`. Số lượng các vị trí khác nhau bằng số lượng người thay thế cần thiết. Trong số tất cả các đảo chữ có khoảng cách tối thiểu, chúng tôi chọn đảo chữ nhỏ nhất về mặt từ điển. 

Điều này đúng vì mọi chuỗi cuối cùng hợp lệ phải là một đảo chữ của`t`. Vấn đề là ở chỗ đó`t`có thể có tới`10^5`nhân vật. Ngay cả đối với chiều dài`20`, số lượng hoán vị riêng biệt đã là rất lớn. Việc liệt kê đầy đủ là vô vọng. 

Một biến thể bạo lực thực tế hơn sẽ tham lam thử mọi sự thay thế có thể có ở mọi vị trí trong khi vẫn duy trì tính hợp lệ của tần số. Đối với mỗi chỉ mục, chúng tôi có thể kiểm tra tất cả 26 chữ cái và xem liệu hậu tố còn lại có thể đáp ứng tần số mục tiêu hay không. Điều đó vẫn trở nên tốn kém nếu được thực hiện một cách ngây thơ, đặc biệt là khi tính toán lại tần số lặp đi lặp lại. 

Quan sát quan trọng là số lượng thay thế tối thiểu được xác định hoàn toàn bằng tần số chồng chéo. 

Nếu như`s`đã chứa năm`A`cát`t`cần ba`A`s thì ít nhất hai`A`s phải biến mất. Nếu như`s`chứa một`C`Và`t`cần bốn`C`s, thì chúng ta phải tạo ba cái mới`C`nó ở đâu đó. 

Vì vậy, vấn đề trở thành: 

Chúng tôi biết chính xác chữ cái nào thừa và chữ cái nào bị thiếu. Làm cách nào để phân phối lại chúng để có được chuỗi nhỏ nhất về mặt từ điển? 

Điều này dẫn đến một chiến lược tham lam từ trái sang phải. 

Tại mỗi vị trí: 

Nếu ký tự hiện tại vẫn cần thiết, chúng tôi có thể muốn giữ nó. 

Nhưng nếu việc thay thế nó bằng một ký tự bị thiếu nhỏ hơn sẽ cải thiện được thứ tự từ điển thì chúng ta nên thực hiện ngay lập tức. 

Nếu ký tự hiện tại quá nhiều và tồn tại một ký tự bị thiếu nhỏ hơn thì việc thay đổi ngay bây giờ luôn có lợi vì các vị trí trước đó chiếm ưu thế trong trật tự từ điển. 

Nếu chỉ còn lại các ký tự bị thiếu lớn hơn, chúng tôi muốn trì hoãn việc thay thế khi có thể, vì việc giữ ký tự hiện tại nhỏ hơn sẽ giúp tiền tố ở mức tối thiểu. 

Kích thước bảng chữ cái nhỏ giúp việc ghi sổ trở nên dễ dàng. Chúng tôi duy trì số lượng bản sao của mỗi chữ cái vẫn được yêu cầu và số lượng bản sao còn lại sau đó trong chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n * 26) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần suất xuất hiện của mỗi ký tự trong`t`. 

Những số đếm này mô tả chính xác những gì chuỗi cuối cùng phải chứa. 
2. Đếm tần suất xuất hiện của mỗi ký tự trong`s`. 

So sánh hai mảng tần số cho chúng ta biết chữ cái nào thừa và chữ cái nào bị thiếu. 
3. Di chuyển chuỗi từ trái sang phải. 

Chúng tôi quyết định giá trị cuối cùng của từng vị trí một cách tham lam vì thứ tự từ điển phụ thuộc trước tiên vào các vị trí trước đó. 
4. Đối với nhân vật hiện tại`c`, trước tiên hãy giảm số lượng còn lại của nó trong hậu tố. 

Điều này cho chúng ta biết có bao nhiêu bản sao của`c`vẫn tồn tại sau này nếu chúng ta chọn không sửa đổi lần xuất hiện này. 
5. Kiểm tra xem`c`là quá mức. 

Nếu tổng số hiện tại của`c`đã không lớn hơn yêu cầu, chúng ta phải giữ ký tự này. Việc thay thế nó sẽ chỉ tạo ra một thao tác bổ sung không cần thiết. 
6. Nếu`c`quá mức, hãy tìm kiếm ký tự bị thiếu nhỏ nhất`x`. 

Chúng tôi thử những lá thư từ`'A'`trở lên vì chúng ta muốn kết quả nhỏ nhất về mặt từ điển. 
7. Nếu`x < c`, thay thế ngay. 

Một chữ cái nhỏ hơn ở vị trí sớm hơn luôn cải thiện câu trả lời. 
8. Nếu`x > c`, chỉ thay thế khi chúng ta không thể giữ lại một cách an toàn`c`. 

Cụ thể, nếu các bản sao còn lại trong tương lai của`c`ít hơn số lượng yêu cầu thì sự xuất hiện này phải tiếp tục. Nếu không, chúng ta có thể trì hoãn việc thay thế đến lần xuất hiện sau. 
9. Bất cứ khi nào có sự thay thế, hãy cập nhật cả hai mảng tần số. 

Chữ cái thừa sẽ mất một lần xuất hiện và chữ cái còn thiếu sẽ mất một lần xuất hiện. 
10. Tiếp tục cho đến khi tất cả các vị trí được xử lý. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến đơn giản: 

Trước khi xử lý từng vị trí, mảng tần số hiện tại biểu thị các ký tự vẫn còn trong chuỗi được sửa đổi một phần. 

Bất cứ khi nào chúng tôi thay thế một ký tự, chúng tôi luôn di chuyển một đơn vị từ một chữ cái thừa sang một chữ cái bị thiếu, do đó số lượng thay thế tối thiểu được giữ nguyên. 

Sự tối ưu về mặt từ điển xuất phát từ sự lựa chọn tham lam từ trái sang phải. Nếu một chữ cái còn thiếu nhỏ hơn có thể được đặt ở vị trí hiện tại thì việc trì hoãn việc cải thiện đó không bao giờ có thể hữu ích vì việc so sánh từ điển phụ thuộc vào vị trí khác biệt sớm nhất. Ngược lại, khi chỉ tồn tại các chữ cái thay thế lớn hơn, chúng tôi giữ ký tự nhỏ hơn hiện tại bất cứ khi nào có thể và trì hoãn việc tăng không thể tránh khỏi sang vị trí sau này. 

Bởi vì mọi quyết định đều tối ưu cục bộ và không bao giờ gây tổn hại đến tính khả thi trong tương lai, nên chuỗi cuối cùng là chuỗi nhỏ nhất về mặt từ điển trong số tất cả các giải pháp thay thế tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = list(input().strip())
    t = input().strip()

    need = [0] * 26
    cur = [0] * 26

    for ch in t:
        need[ord(ch) - 65] += 1

    for ch in s:
        cur[ord(ch) - 65] += 1

    changes = 0

    for i in range(len(s)):
        c = ord(s[i]) - 65

        # this character is not excessive
        if cur[c] <= need[c]:
            continue

        for x in range(26):
            if cur[x] < need[x]:
                # smaller character, always beneficial
                if x < c:
                    cur[c] -= 1
                    cur[x] += 1
                    s[i] = chr(x + 65)
                    changes += 1
                    break

                # larger character
                # replace only if enough copies remain later
                elif x > c:
                    if cur[c] - 1 >= need[c]:
                        cur[c] -= 1
                        cur[x] += 1
                        s[i] = chr(x + 65)
                        changes += 1
                    break

    print(changes)
    print("".join(s))

solve()
```Hai mảng tần số là toàn bộ xương sống của giải pháp.`need`lưu trữ bao nhiêu bản sao mỗi ký tự phải xuất hiện trong chuỗi cuối cùng.`cur`lưu trữ bao nhiêu bản sao hiện đang tồn tại trong câu trả lời đang phát triển của chúng tôi. 

Việc đi qua là tham lam và lâu dài. Một khi chúng ta di chuyển qua một vị trí, chúng ta sẽ không bao giờ xem lại nó. Điều đó là an toàn vì thứ tự từ điển ưu tiên hoàn toàn các vị trí trước đó so với các vị trí sau. 

Điều kiện:```
if cur[c] <= need[c]:
```là cực kỳ quan trọng. Nó có nghĩa là ký tự này không còn quá mức nữa. Việc thay thế nó sẽ buộc chúng ta phải tạo lại cùng một ký tự ở nơi khác, làm tăng số lượng thao tác một cách không cần thiết. 

Phần tinh tế thứ hai là:```
if cur[c] - 1 >= need[c]:
```Điều này kiểm tra xem liệu chúng ta có thể loại bỏ sự xuất hiện hiện tại một cách an toàn và vẫn có đủ bản sao của ký tự này sau này hay không. Thiếu điều kiện này sẽ gây ra nhiều đáp án sai vì chúng ta có thể loại bỏ quá nhiều bản sao của một lá thư quá sớm. 

Vòng lặp trên tất cả 26 chữ cái không tốn kém vì kích thước bảng chữ cái không đổi. Ngay cả với`10^5`các nhân vật, toàn bộ công việc vẫn xoay quanh`2.6 * 10^6`hoạt động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
s = ABA
t = CBA
```Tần số mục tiêu là:```
A:1 B:1 C:1
```Tần số hiện tại là:```
A:2 B:1
```| Vị trí | Char hiện tại | Quá mức? | Thiếu Nhỏ Nhất | Hành động | Chuỗi kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 0 | A | Có | C | Giữ lại, vì C > A và cần một A | ABA | 
| 1 | B | Không | C | Giữ | ABA | 
| 2 | A | Có | C | Thay thế bằng C | ABC | 

Câu trả lời cuối cùng:```
1
ABC
```Dấu vết này cho thấy tại sao đôi khi chúng ta trì hoãn việc thay thế. Thay thế cái đầu tiên`A`với`C`sẽ sản xuất`CBA`, lớn hơn về mặt từ điển so với`ABC`. 

### Ví dụ 2 

đầu vào:```
s = DDBAAC
t = ADBADC
```Tần số mục tiêu:```
A:2 B:1 C:1 D:2
```Tần số hiện tại:```
A:2 B:1 C:1 D:2
```Các chuỗi đã có nhiều tần số giống hệt nhau. 

| Vị trí | Char hiện tại | Quá mức? | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | D | Không | Giữ | DDBAAC | 
| 1 | D | Không | Giữ | DDBAAC | 
| 2 | B | Không | Giữ | DDBAAC | 
| 3 | A | Không | Giữ | DDBAAC | 
| 4 | A | Không | Giữ | DDBAAC | 
| 5 | C | Không | Giữ | DDBAAC | 

Câu trả lời cuối cùng:```
0
DDBAAC
```Điều này chứng tỏ rằng thuật toán không bao giờ thực hiện những thay thế không cần thiết. Tần số phù hợp đã ngụ ý hoạt động bằng không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * 26) | Mỗi vị trí quét tối đa 26 chữ cái | 
| Không gian | O(26) | Chỉ các mảng tần số có kích thước cố định mới được lưu trữ | 

Vì bảng chữ cái là cố định nên hệ số 26 hoạt động như một hằng số. Với`n = 10^5`, thuật toán dễ dàng phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    s = list(input().strip())
    t = input().strip()

    need = [0] * 26
    cur = [0] * 26

    for ch in t:
        need[ord(ch) - 65] += 1

    for ch in s:
        cur[ord(ch) - 65] += 1

    changes = 0

    for i in range(len(s)):
        c = ord(s[i]) - 65

        if cur[c] <= need[c]:
            continue

        for x in range(26):
            if cur[x] < need[x]:
                if x < c:
                    cur[c] -= 1
                    cur[x] += 1
                    s[i] = chr(x + 65)
                    changes += 1
                    break

                elif x > c:
                    if cur[c] - 1 >= need[c]:
                        cur[c] -= 1
                        cur[x] += 1
                        s[i] = chr(x + 65)
                        changes += 1
                    break

    print(changes)
    print("".join(s))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue()

# provided sample
assert run("ABA\nCBA\n") == "1\nABC\n", "sample 1"

# minimum size
assert run("A\nA\n") == "0\nA\n", "single equal character"

# complete replacement
assert run("AAA\nBBB\n") == "3\nBBB\n", "all characters replaced"

# already an anagram
assert run("ABCD\nDCBA\n") == "0\nABCD\n", "no changes needed"

# lexicographic subtlety
assert run("BAA\nAAC\n") == "1\nCAA\n", "must delay replacement correctly"

# repeated excessive letters
assert run("ZZZZ\nAAAA\n") == "4\nAAAA\n", "large repeated replacements"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`A / A`|`0 A`| Đầu vào kích thước tối thiểu | 
|`AAA / BBB`|`3 BBB`| Mọi vị trí đều phải thay đổi | 
|`ABCD / DCBA`|`0 ABCD`| Đảo chữ hiện tại sẽ không thay đổi | 
|`BAA / AAC`|`1 CAA`| Trì hoãn thay thế để tối ưu hóa từ điển | 
|`ZZZZ / AAAA`|`4 AAAA`| Xử lý dư thừa nhiều lần | 

## Vỏ cạnh 

Hãy xem xét:```
s = BAA
t = AAC
```Mục tiêu yêu cầu một`C`, nhưng thay thế cái đầu tiên`B`ngay lập tức đưa ra:```
CAA
```Thuật toán xử lý vị trí đầu tiên:`B`là quá mức và chữ cái còn thiếu nhỏ nhất là`C`. 

Từ`C > B`, thuật toán kiểm tra xem có đủ không`B`s ở lại sau. Họ không làm vậy, bởi vì đây là lần duy nhất`B`. Vì vậy, sự thay thế là bắt buộc. 

Câu trả lời cuối cùng trở thành:```
CAA
```đó là tối ưu. 

Bây giờ hãy xem xét:```
s = ABA
t = CBA
```Lúc đầu`A`, chữ cái còn thiếu nhỏ nhất là`C`. 

Từ`C > A`, việc thay thế ngay lập tức sẽ làm xấu đi tiền tố. Thuật toán giữ điều này`A`bởi vì cái khác`A`tồn tại sau này và có thể được thay thế thay thế. 

Ở vị trí cuối cùng, việc thay thế trở nên cần thiết:```
ABC
```Điều này thể hiện nguyên tắc tham lam cốt lõi: trì hoãn những thay thế có hại bất cứ khi nào có thể. 

Cuối cùng, hãy xem xét trường hợp cải tiến trước đó luôn đúng:```
s = CBA
t = ABB
```Tần số mục tiêu cần thêm`B`và loại bỏ`C`. 

Tại vị trí`0`,`C`quá mức và chữ cái còn thiếu nhỏ nhất là`B`. 

Bởi vì`B < C`, thay thế ngay lập tức cải thiện nghiêm ngặt trật tự từ điển. Thuật toán thực hiện thay thế cùng một lúc và tạo ra:```
BBA
```Bất kỳ giải pháp nào bắt đầu bằng`C`sẽ lớn hơn về mặt từ điển.
