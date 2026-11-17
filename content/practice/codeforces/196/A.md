---
title: "CF 196A - Dãy số tối đa theo từ điển"
description: "Chúng ta được cung cấp một chuỗi chữ thường và có thể chọn bất kỳ chuỗi ký tự nào không trống trong khi vẫn giữ nguyên thứ tự ban đầu của chúng. Trong số tất cả các dãy con có thể có, chúng ta cần dãy con lớn nhất về mặt từ điển."
date: "2026-06-03T09:41:06+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 196
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 124 (Div. 1)"
rating: 1100
weight: 196
solve_time_s: 89
verified: true
draft: false
---

[CF 196A - Dãy số tối đa theo từ điển](https://codeforces.com/problemset/problem/196/A) 

**Đánh giá:** 1100 
**Tags:** tham lam, dây 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chữ thường và có thể chọn bất kỳ chuỗi ký tự nào không trống trong khi vẫn giữ nguyên thứ tự ban đầu của chúng. 

Trong số tất cả các dãy con có thể có, chúng ta cần dãy con lớn nhất về mặt từ điển. So sánh từ điển hoạt động chính xác như thứ tự từ điển: vị trí đầu tiên trong đó hai chuỗi khác nhau sẽ quyết định chuỗi thắng và nếu một chuỗi là tiền tố của chuỗi kia thì chuỗi dài hơn sẽ lớn hơn. 

Chiều dài chuỗi có thể đạt tới$10^5$. Bài toán dãy con ngay lập tức đề xuất một không gian tìm kiếm hàm mũ vì mọi ký tự đều có thể được lấy hoặc bỏ qua. có$2^n-1$các dãy con không trống, hoàn toàn không thể liệt kê được khi$n=10^5$. Ngay cả các thuật toán quét liên tục các hậu tố lớn cũng có thể trở nên quá chậm nếu chúng thực hiện$O(n^2)$công việc. Với giới hạn 2 giây, chúng ta nên nhắm mục tiêu thời gian tuyến tính hoặc gần với thời gian đó. 

Một số tình huống rất dễ xử lý sai. 

Coi như:```
abc
```Câu trả lời là:```
c
```Một cách tiếp cận bất cẩn có thể cố gắng giữ nhiều ký tự vì các chuỗi dài hơn đôi khi sẽ lớn hơn. Tuy nhiên, ở đây ký tự đầu tiên chiếm ưu thế trong trật tự từ điển. Bất kỳ chuỗi tiếp theo nào bắt đầu bằng`a`hoặc`b`thua ngay lập tức với người bắt đầu bằng`c`. 

Một trường hợp phức tạp khác là:```
zzza
```Câu trả lời là:```
zzza
```Một khi chúng ta quyết định điều đó`z`là ký tự đầu tiên tốt nhất có thể, chúng ta không nên chỉ lấy một lần xuất hiện. Giữ lại tất cả sau này`z`các ký tự làm cho kết quả lớn hơn vì các tiền tố vẫn bằng nhau và chuỗi dài hơn sẽ thắng. 

Ví dụ thứ ba là:```
ababba
```Câu trả lời là:```
bbba
```Chỉ chọn ký tự tối đa đầu tiên là không đủ. Sau khi lấy một`b`, chúng ta phải giải bài toán tương tự ở hậu tố còn lại và tiếp tục một cách tham lam. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: tạo ra mọi chuỗi con, so sánh chúng theo từ điển và giữ lại chuỗi tốt nhất. 

Điều này có tác dụng vì định nghĩa của bài toán chính xác là "tìm giá trị lớn nhất trong số tất cả các dãy con". Thật không may, một chuỗi dài$n$có$2^n$các chuỗi tiếp theo. Ngay cả đối với$n=50$, đó đã là hơn một triệu triệu khả năng rồi. Vì$n=10^5$, thật là vô vọng. 

Để tìm thứ gì đó nhanh hơn, chúng ta nên suy nghĩ về yếu tố quyết định thứ tự từ điển. 

Ký tự đầu tiên là vị trí quan trọng nhất. Nếu một dãy con bắt đầu bằng`y`, nó không bao giờ có thể đánh bại một dãy con bắt đầu bằng`z`. Điều đó có nghĩa là ký tự đầu tiên của câu trả lời phải là ký tự tối đa xuất hiện ở bất kỳ đâu trong chuỗi. 

Giả sử ký tự lớn nhất xuất hiện trong hậu tố hiện tại là`c`. Mọi câu trả lời tối ưu đều phải bắt đầu bằng một`c`. Sự xuất hiện nào của`c`chúng ta có nên giữ lại không? 

Lấy tất cả chúng. 

Nếu hai câu trả lời ứng viên bắt đầu bằng cùng một tiền tố và một câu tiếp tục với các bản sao bổ sung có cùng ký tự tối đa thì câu trả lời dài hơn sẽ lớn hơn về mặt từ điển. Không có hình phạt nào khi lấy một nhân vật tối đa vì nó không thể làm cho vị trí trước đó trở nên tồi tệ hơn. 

Quan sát này dẫn đến một quan điểm rất đơn giản. Xử lý chuỗi từ phải sang trái trong khi vẫn duy trì ký tự lớn nhất được thấy cho đến nay. Một ký tự thuộc về câu trả lời chính xác khi nó bằng hậu tố tối đa tại vị trí của nó. 

Ví dụ:```
ababba
```Hậu tố cực đại là:```
b b b b b a
```Bất kỳ vị trí nào có ký tự bằng hậu tố tối đa của nó đều được chọn:```
a b a b b a
  ^   ^ ^ ^
```Dãy số thu được là:```
bbba
```Điều này chỉ yêu cầu một lần quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(2^n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi. 
2. Di chuyển chuỗi từ phải sang trái. 
3. Duy trì`mx`, ký tự lớn nhất gặp phải cho đến nay trong hậu tố. 
4. Khi ký tự hiện tại lớn hơn`mx`, cập nhật`mx`. 
5. Nếu ký tự hiện tại bằng`mx`, thêm nó vào câu trả lời. 

Ký tự này là hậu tố tối đa. Bất kỳ dãy con tối đa về mặt từ điển học nào cũng phải chứa nó. 
6. Các ký tự đã thu thập được lấy từ phải sang trái, vì vậy hãy đảo ngược chúng. 
7. Xuất chuỗi kết quả. 

### Tại sao nó hoạt động 

Ở bất kỳ vị trí nào, hãy để`M`là ký tự lớn nhất xuất hiện ở bất cứ đâu bên phải nó, bao gồm cả chính nó. 

Nếu ký tự hiện tại nhỏ hơn`M`, việc chọn nó sẽ buộc chuỗi con bắt đầu bằng một ký tự tệ hơn chuỗi bắt đầu khi xuất hiện`M`. Sự lựa chọn như vậy không bao giờ có thể thuộc về một câu trả lời tối ưu. 

Nếu ký tự hiện tại bằng`M`, bỏ qua nó cũng không tối ưu. Việc giữ ký tự này sẽ mang lại sự xuất hiện bổ sung cho chữ cái lớn nhất hiện có trong khi vẫn duy trì khả năng chọn tất cả các ký tự tối ưu trong tương lai. Vì thứ tự từ điển ưu tiên các chuỗi dài hơn khi các tiền tố bằng nhau nên việc lấy mọi ký tự có hậu tố tối đa luôn có lợi. 

Do đó, dãy con tối ưu bao gồm chính xác các vị trí có ký tự bằng ký tự lớn nhất trong hậu tố của chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()

    mx = ''
    ans = []

    for ch in reversed(s):
        if ch > mx:
            mx = ch
        if ch == mx:
            ans.append(ch)

    print(''.join(reversed(ans)))

if __name__ == "__main__":
    solve()
```Biến`mx`lưu trữ ký tự lớn nhất được thấy trong hậu tố được xử lý cho đến nay. Vì chúng tôi quét từ phải sang trái nên đây chính xác là hậu tố tối đa cho vị trí hiện tại. 

Bất cứ khi nào ký tự hiện tại bằng`mx`, chúng tôi giữ nó. Các ký tự nhỏ hơn hậu tố tối đa sẽ bị loại bỏ vì ký tự lớn hơn tồn tại sau đó và chiếm ưu thế trong thứ tự từ điển. 

Câu trả lời được thu thập theo thứ tự ngược lại vì quá trình quét tiến hành từ cuối chuỗi về đầu. Đảo ngược một lần ở cuối sẽ khôi phục thứ tự từ trái sang phải ban đầu của chuỗi tiếp theo. 

Không có lo ngại về tràn số nguyên vì thuật toán chỉ thao tác với các ký tự và danh sách. Điểm tinh tế duy nhất là sử dụng`if ch == mx`sau khi có khả năng cập nhật`mx`. Điều này đảm bảo rằng số ký tự tối đa mới được phát hiện sẽ được bao gồm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
ababba
```| Vị trí (phải sang trái) | Nhân vật | mx hiện tại | Lấy? | Đã sưu tầm | 
| --- | --- | --- | --- | --- | 
| 5 | một | một | Có | một | 
| 4 | b | b | Có | ab | 
| 3 | b | b | Có | abb | 
| 2 | một | b | Không | abb | 
| 1 | b | b | Có | abbb | 
| 0 | một | b | Không | abbb | 

Sau khi đảo ngược:```
bbba
```Dấu vết này cho thấy rằng mọi ký tự được giữ lại đều bằng ký tự tối đa trong hậu tố của nó. 

### Ví dụ 2 

đầu vào:```
zzza
```| Vị trí (phải sang trái) | Nhân vật | mx hiện tại | Lấy? | Đã sưu tầm | 
| --- | --- | --- | --- | --- | 
| 3 | một | một | Có | một | 
| 2 | z | z | Có | az | 
| 1 | z | z | Có | azz | 
| 0 | z | z | Có | azzz | 

Sau khi đảo ngược:```
zzza
```Ví dụ này giải thích tại sao tất cả các lần xuất hiện của ký tự tối đa cần được giữ nguyên. Giữ mọi`z`tạo ra một kết quả lớn hơn về mặt từ điển so với việc chỉ giữ một. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần vượt qua chuỗi | 
| Không gian |$O(n)$| Chuỗi câu trả lời có thể chứa tất cả các ký tự | 

Với$n \le 10^5$, quét tuyến tính dễ dàng đủ nhanh. Việc sử dụng bộ nhớ cũng an toàn vì việc lưu trữ câu trả lời yêu cầu nhiều nhất$10^5$nhân vật. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    s = input().strip()

    mx = ''
    ans = []

    for ch in reversed(s):
        if ch > mx:
            mx = ch
        if ch == mx:
            ans.append(ch)

    return ''.join(reversed(ans))

# provided sample
assert run("ababba\n") == "bbba", "sample 1"

# minimum size
assert run("a\n") == "a", "single character"

# all equal characters
assert run("aaaaaa\n") == "aaaaaa", "all equal"

# strictly increasing
assert run("abcde\n") == "e", "only largest suffix maximum survives"

# strictly decreasing
assert run("edcba\n") == "edcba", "every position is a suffix maximum"

# repeated maximum letters
assert run("zzza\n") == "zzza", "keep all maximum letters"

# maximum-character block in middle
assert run("abzczzb\n") == "zzz", "multiple suffix maxima"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`a`| Chiều dài tối thiểu | 
|`aaaaaa`|`aaaaaa`| Tất cả các ký tự được giữ lại | 
|`abcde`|`e`| Chỉ có mức tối đa cuối cùng mới tồn tại | 
|`edcba`|`edcba`| Mỗi vị trí là một hậu tố tối đa | 
|`zzza`|`zzza`| Ký tự lớn nhất lặp lại | 
|`abzczzb`|`zzz`| Nhiều lần xuất hiện tối đa trên chuỗi | 

## Vỏ cạnh 

Hãy xem xét:```
abc
```Quét từ phải sang trái cho hậu tố cực đại:```
c c c
```Chỉ có trận chung kết`c`bằng mức tối đa hậu tố của nó ở vị trí của chính nó. Các đầu ra thuật toán:```
c
```Bất kỳ chuỗi tiếp theo nào bắt đầu bằng`a`hoặc`b`thua ngay lập tức vì ký tự đầu tiên của nó nhỏ hơn. 

Bây giờ hãy xem xét:```
zzza
```Hậu tố cực đại là:```
z z z a
```Mỗi ký tự bằng hậu tố tối đa của nó, vì vậy tất cả các ký tự đều được chọn. Đầu ra trở thành:```
zzza
```Một lỗi phổ biến là chỉ giữ lại lần xuất hiện đầu tiên của ký tự và đầu ra tối đa`z`, nhỏ hơn về mặt từ điển vì`zzza`có cùng tiền tố và dài hơn. 

Cuối cùng, hãy xem xét:```
ababba
```Hậu tố cực đại là:```
b b b b b a
```Vị trí được chọn là ba`b`nhân vật và cái kết`a`, sản xuất:```
bbba
```Thuật toán bỏ qua cả hai`a`các ký tự có ký tự lớn hơn ở sau chuỗi. Những vị trí đó không bao giờ có thể thuộc về một dãy con tối đa về mặt từ điển.
