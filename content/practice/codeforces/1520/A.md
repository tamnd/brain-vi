---
title: "CF 1520A - Đừng phân tâm!"
description: "Mỗi trường hợp thử nghiệm đưa ra một chuỗi ngày, trong đó mỗi ngày được gắn nhãn nhiệm vụ mà Polycarp đã làm trong ngày đó. Quy tắc rất đơn giản: một khi anh ta ngừng thực hiện một nhiệm vụ và chuyển sang nhiệm vụ khác, anh ta sẽ không bao giờ được phép quay lại nhiệm vụ cũ."
date: "2026-06-10T18:04:53+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1520
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 719 (Div. 3)"
rating: 800
weight: 1520
solve_time_s: 150
verified: true
draft: false
---

[CF 1520A - Đừng phân tâm!](https://codeforces.com/problemset/problem/1520/A) 

**Đánh giá:** 800 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm đưa ra một chuỗi ngày, trong đó mỗi ngày được gắn nhãn nhiệm vụ mà Polycarp đã làm trong ngày đó. Quy tắc rất đơn giản: một khi anh ta ngừng thực hiện một nhiệm vụ và chuyển sang nhiệm vụ khác, anh ta sẽ không bao giờ được phép quay lại nhiệm vụ cũ. 

Nhìn theo cách khác, mỗi chữ cái nhiệm vụ phải xuất hiện trong một khối liên tục duy nhất. Một nhiệm vụ có thể kéo dài một ngày hoặc nhiều ngày liên tiếp nhưng tất cả các lần xuất hiện của nhiệm vụ đó phải liền kề nhau. 

Ví dụ,`"AABBBCC"`hợp lệ vì mỗi chữ cái tạo thành một đoạn không bị gián đoạn. Mặt khác,`"ABA"`không hợp lệ vì nhiệm vụ`A`xuất hiện, biến mất rồi lại xuất hiện. 

Đối với mỗi trường hợp thử nghiệm, chúng ta phải xác định xem trình tự được ghi có tuân theo quy tắc này hay không. Nếu mọi tác vụ xuất hiện trong đúng một phân đoạn liền kề, chúng ta sẽ in`"YES"`. Nếu không chúng tôi in`"NO"`. 

Những hạn chế là cực kỳ nhỏ. Có tối đa 1000 trường hợp kiểm thử và mỗi chuỗi có độ dài tối đa là 50. Ngay cả một$O(n^2)$giải pháp sẽ thực hiện tối đa khoảng 2,5 triệu thao tác trong tất cả các thử nghiệm, đủ nhanh. Điều này có nghĩa là tính đúng đắn và đơn giản quan trọng hơn việc tối ưu hóa tích cực. 

Trường hợp phức tạp là sự lặp lại liên tiếp của cùng một nhiệm vụ. Coi như:```
4
AAAA
```Câu trả lời đúng là`"YES"`bởi vì Polycarp không bao giờ rời bỏ nhiệm vụ`A`. Việc triển khai bất cẩn xử lý từng lần xuất hiện riêng biệt thay vì từng phân đoạn có thể từ chối nó một cách không chính xác. 

Một lỗi dễ dàng khác xuất hiện khi một nhiệm vụ quay trở lại sau một khoảng trống:```
3
ABA
```Câu trả lời đúng là`"NO"`. Polycarp đã làm việc trên`A`, chuyển sang`B`, sau đó quay trở lại`A`. Việc triển khai chỉ kiểm tra xem các ký tự liền kề có khác nhau hay không sẽ bỏ sót vi phạm này. 

Một trường hợp ít rõ ràng hơn là:```
6
AABBCC
```Câu trả lời đúng là`"YES"`. Khi khối nhiệm vụ kết thúc, nó sẽ không bao giờ xuất hiện nữa. Một giải pháp giả định không chính xác mỗi chữ cái có thể chỉ xuất hiện một lần trong chuỗi sẽ từ chối các đầu vào hợp lệ như thế này. 

Cuối cùng, hãy xem xét:```
7
ABCDCBA
```Câu trả lời đúng là`"NO"`. Một số chữ cái xuất hiện lại sau khi các nhiệm vụ khác đã được bắt đầu. Hành vi vi phạm không chỉ giới hạn ở việc quay lại ngay sau một nhiệm vụ; quay trở lại sau bất kỳ số lượng nhiệm vụ trung gian nào đều bị cấm. 

## Phương pháp tiếp cận 

Một ý tưởng đơn giản là kiểm tra từng chữ cái và xác minh rằng tất cả các lần xuất hiện của nó tạo thành một khoảng thời gian liên tục. Đối với mỗi ký tự, chúng ta có thể tìm vị trí đầu tiên và cuối cùng của nó, sau đó kiểm tra xem mọi vị trí giữa chúng có chứa cùng ký tự đó hay không. Vì có nhiều nhất 26 chữ cái và nhiều nhất 50 vị trí nên điều này hoạt động thoải mái trong giới hạn. 

Tuy nhiên, có một cách rõ ràng hơn để suy nghĩ về vấn đề. Những khoảnh khắc quan trọng duy nhất là chuyển đổi nhiệm vụ. Các ký tự bằng nhau liên tiếp thuộc cùng một phân đoạn công việc và không cung cấp thông tin mới. 

Giả sử chúng ta nén chuỗi bằng cách chỉ giữ lại ký tự đầu tiên của mỗi phân đoạn. Ví dụ:```
AABBBCCAA  ->  ABCA
```Bây giờ mỗi ký tự trong chuỗi nén đại diện cho một phân đoạn nhiệm vụ riêng biệt. Quy tắc trở nên rất đơn giản: không có chữ cái nào được xuất hiện hai lần trong chuỗi nén. 

Nếu một chữ cái xuất hiện hai lần sau khi nén thì có ít nhất hai phân đoạn riêng biệt của nhiệm vụ đó, điều đó có nghĩa là Polycarp sẽ quay lại nó sau. Nếu mỗi chữ cái xuất hiện nhiều nhất một lần thì mỗi tác vụ sẽ chiếm đúng một khối liền kề. 

Cách tiếp cận bạo lực kiểm tra sự tiếp giáp một cách trực tiếp. Cách tiếp cận được tối ưu hóa nhận thấy rằng chỉ có ranh giới phân đoạn mới quan trọng và giảm thiểu vấn đề phát hiện các chữ cái trùng lặp giữa các phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một tập hợp trống có tên`seen`. 
2. Xử lý chuỗi từ trái sang phải. 
3. Đối với mỗi vị trí, hãy xác định xem nó có bắt đầu một phân đoạn mới hay không. Vị trí bắt đầu một phân đoạn mới nếu đó là ký tự đầu tiên của chuỗi hoặc ký tự của nó khác với ký tự trước đó. 
4. Bất cứ khi nào một phân đoạn mới bắt đầu, hãy kiểm tra xem chữ cái của nó đã có chưa`seen`. 
5. Nếu thư đã có sẵn, hãy gửi lại`"NO"`ngay lập tức. Điều này có nghĩa là nhiệm vụ đã xuất hiện trong phân đoạn trước đó và hiện đang được xem lại. 
6. Nếu không, hãy thêm chữ cái vào`seen`và tiếp tục quét. 
7. Nếu toàn bộ chuỗi được xử lý mà không tìm thấy ký tự phân đoạn lặp lại, hãy trả về`"YES"`. 

### Tại sao nó hoạt động 

bộ`seen`chứa chính xác các chữ cái nhiệm vụ có phân đoạn đã bắt đầu. 

Bất cứ khi nào một phân đoạn mới bắt đầu, chỉ có hai khả năng. Đây là phân đoạn đầu tiên của nhiệm vụ đó, trong trường hợp đó việc thêm nó vào`seen`hợp lệ hoặc tác vụ đã xuất hiện trong phân đoạn trước đó. Trong trường hợp thứ hai, Polycarp chắc chắn đã rời bỏ nhiệm vụ đó và sau đó quay lại làm nhiệm vụ đó, điều này vi phạm quy tắc. 

Ngược lại, nếu không có chữ cái bắt đầu phân đoạn nào được lặp lại thì mỗi tác vụ sẽ xuất hiện trong đúng một phân đoạn. Vì mỗi phân đoạn được xây dựng liền kề nhau nên mỗi nhiệm vụ chiếm một khối liên tục và lịch trình là hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        s = input().strip()

        seen = set()
        ok = True

        for i in range(n):
            if i == 0 or s[i] != s[i - 1]:
                if s[i] in seen:
                    ok = False
                    break
                seen.add(s[i])

        print("YES" if ok else "NO")

solve()
```Việc thực hiện theo thuật toán trực tiếp. 

bộ`seen`lưu trữ các chữ cái có phân đoạn đã bắt đầu. điều kiện```
i == 0 or s[i] != s[i - 1]
```phát hiện sự bắt đầu của một đoạn mới. Các chữ cái giống nhau liên tiếp sẽ bị bỏ qua vì chúng thuộc cùng một nhiệm vụ không bị gián đoạn. 

Thứ tự của các hoạt động quan trọng. Trước tiên, chúng tôi kiểm tra xem ký tự phân đoạn đã xuất hiện trước đó chưa và chỉ sau đó thêm nó vào tập hợp. Đảo ngược thứ tự đó sẽ từ chối không chính xác lần xuất hiện đầu tiên của mọi nhiệm vụ. 

Không cần xử lý đặc biệt đối với các chuỗi có độ dài bằng một. Ký tự đầu tiên bắt đầu một phân đoạn một cách tự nhiên, được thêm vào tập hợp và câu trả lời trở thành`"YES"`. 

Vì chỉ xuất hiện các chữ cái viết hoa nên tập hợp không bao giờ chứa nhiều hơn 26 phần tử. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
ABA
```| tôi | s[i] | Phân đoạn mới? | Đã thấy trước đây? | nhìn thấy sau bước | 
| --- | --- | --- | --- | --- | 
| 0 | A | Có | Không | {A} | 
| 1 | B | Có | Không | {A, B} | 
| 2 | A | Có | Có | vi phạm | 

Ký tự thứ ba bắt đầu một phân đoạn mới cho nhiệm vụ`A`. Từ`A`đã bắt đầu một phân đoạn trước đó, Polycarp quay lại nhiệm vụ đã hoàn thành. Câu trả lời là`"NO"`. 

### Ví dụ 2 

đầu vào:```
FFGZZZY
```| tôi | s[i] | Phân đoạn mới? | Đã thấy trước đây? | nhìn thấy sau bước | 
| --- | --- | --- | --- | --- | 
| 0 | F | Có | Không | {F} | 
| 1 | F | Không | - | {F} | 
| 2 | G | Có | Không | {F, G} | 
| 3 | Z | Có | Không | {F, G, Z} | 
| 4 | Z | Không | - | {F, G, Z} | 
| 5 | Z | Không | - | {F, G, Z} | 
| 6 | Y | Có | Không | {F, G, Z, Y} | 

Không có chữ cái bắt đầu phân đoạn lặp lại. Mỗi nhiệm vụ chiếm chính xác một khối, vì vậy câu trả lời là`"YES"`. 

Ví dụ này cho thấy tại sao phải bỏ qua các chữ cái bằng nhau liên tiếp. Việc lặp đi lặp lại`F`Và`Z`ngày thuộc cùng một phân đoạn và không thể hiện việc quay lại một nhiệm vụ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần | 
| Không gian | O(26) | Bộ lưu trữ tối đa một mục nhập cho mỗi chữ cái nhiệm vụ | 

Với$n \le 50$, thậm chí các giải pháp chậm hơn cũng sẽ phù hợp một cách thoải mái. Quét tuyến tính là không đáng kể đối với các giới hạn nhất định và chỉ sử dụng một lượng bộ nhớ nhỏ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def input():
        return sys.stdin.readline()

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        s = input().strip()

        seen = set()
        ok = True

        for i in range(n):
            if i == 0 or s[i] != s[i - 1]:
                if s[i] in seen:
                    ok = False
                    break
                seen.add(s[i])

        ans.append("YES" if ok else "NO")

    return "\n".join(ans)

# provided sample
assert run(
"""5
3
ABA
11
DDBBCCCBBEZ
7
FFGZZZY
1
Z
2
AB
"""
) == """NO
NO
YES
YES
YES""", "sample 1"

# minimum size
assert run(
"""1
1
A
"""
) == "YES", "single task"

# all equal
assert run(
"""1
6
AAAAAA
"""
) == "YES", "single continuous segment"

# returns to a task
assert run(
"""1
5
ABCBA
"""
) == "NO", "task reappears"

# boundary between segments
assert run(
"""1
6
AABBCC
"""
) == "YES", "multiple valid segments"

# maximum-length style case
assert run(
"""1
50
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
"""
) == "YES", "long uniform string"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`A`|`YES`| Đầu vào tối thiểu có thể | 
|`AAAAAA`|`YES`| Ngày lặp đi lặp lại trong một phân đoạn | 
|`ABCBA`|`NO`| Quay lại nhiệm vụ đã hoàn thành trước đó | 
|`AABBCC`|`YES`| Nhiều phân đoạn liền kề hợp lệ | 
| 50 bản sao của`A`|`YES`| Độ dài lớn nhất với một nhiệm vụ liên tục | 

## Vỏ cạnh 

Hãy xem xét:```
1
4
AAAA
```Quá trình quét bắt đầu một phân đoạn ở chỉ số 0 và thêm`A`ĐẾN`seen`. Các vị trí còn lại không bắt đầu phân đoạn mới vì mỗi ký tự đều bằng ký tự trước đó. Không tìm thấy ký tự phân đoạn lặp lại, vì vậy câu trả lời là`"YES"`. Điều này xác nhận rằng công việc liên tục trong thời gian dài trên một nhiệm vụ là hợp lệ. 

Coi như:```
1
3
ABA
```Tại chỉ số 0,`A`bắt đầu một phân đoạn và được chèn vào`seen`. Tại chỉ số 1,`B`bắt đầu một phân đoạn và được chèn vào. Tại chỉ số 2, một phân đoạn khác bắt đầu bằng`A`. Từ`A`đã ở trong rồi`seen`, thuật toán trả về ngay`"NO"`. Đây chính xác là tình huống bị cấm quay lại nhiệm vụ trước đó. 

Coi như:```
1
6
AABBCC
```Đoạn bắt đầu là`A`,`B`, Và`C`. Mỗi cái xuất hiện chính xác một lần trong số các phần đầu của phân đoạn, vì vậy tập hợp này sẽ phát triển khi`{A}`,`{A,B}`,`{A,B,C}`. Không có sự lặp lại nào xảy ra và câu trả lời là`"YES"`. Điều này chứng tỏ rằng nhiều tác vụ được cho phép miễn là mỗi tác vụ chiếm một khối liền kề. 

Coi như:```
1
7
ABCDCBA
```Đoạn bắt đầu là`A`,`B`,`C`,`D`,`C`,`B`,`A`. Khi thứ hai`C`phân đoạn bắt đầu,`C`đã ở trong rồi`seen`, do đó kết quả đầu ra của thuật toán`"NO"`ngay lập tức. Vi phạm được phát hiện ở nhiệm vụ đầu tiên xuất hiện lại, ngay cả khi có liên quan đến nhiều nhiệm vụ khác nhau.
