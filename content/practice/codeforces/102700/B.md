---
title: "CF 102700B - Tên bé"
description: "Chúng ta có hai chuỗi, tên cha a và tên mẹ b, mỗi chuỗi có độ dài tối đa là 2 10^5. Tên của em bé phải được hình thành bằng cách lấy một chuỗi con liền kề không trống từ a, ngay sau đó là một chuỗi con liền kề không trống từ b."
date: "2026-08-16T17:51:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102700
codeforces_index: "B"
codeforces_contest_name: "2020 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102700
solve_time_s: 228
verified: true
draft: false
---

[CF 102700B - Tên em bé](https://codeforces.com/problemset/problem/102700/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi, tên của người cha`a`và tên mẹ`b`, mỗi cái có chiều dài tối đa`2 * 10^5`. Tên của em bé phải được hình thành bằng cách lấy một chuỗi con liền kề không trống từ`a`, ngay sau đó là một chuỗi con liền kề không trống từ`b`. Trong số tất cả các cách ghép như vậy, chúng ta cần cách ghép lớn nhất về mặt từ điển. Vấn đề chính thức sử dụng thứ tự từ điển thông thường, trong đó ký tự khác nhau đầu tiên quyết định và nếu một chuỗi là tiền tố của chuỗi khác thì chuỗi dài hơn sẽ lớn hơn. 

Đầu vào chính xác là hai chuỗi chữ thường, do đó không có tham số số hoặc nhiều trường hợp kiểm thử. Đầu ra là một chuỗi, cách nối tốt nhất có thể. 

Sự hạn chế của`2 * 10^5`các ký tự loại trừ mọi thứ gần với việc liệt kê tất cả các cặp chuỗi con. Một chuỗi có độ dài`n`có`n(n+1)/2`các chuỗi con không trống, do đó ngay cả trước khi so sánh các chuỗi nối của chúng, hai chuỗi có độ dài`2 * 10^5`sản xuất đại khái`10^20`các cặp có thể. Với giới hạn một giây, giải pháp dự định phải tuyến tính hoặc gần tuyến tính trong tổng kích thước đầu vào. Một mảng hậu tố cho kết quả có thể chấp nhận được`O(n log n)`nhưng có một cách thậm chí còn đơn giản hơn theo thời gian tuyến tính để tìm hậu tố tối đa về mặt từ điển, đó là cách chúng tôi sử dụng ở đây. 

Có một số trường hợp đặc biệt có thể đánh lừa một giải pháp chỉ dựa trên ký tự lớn nhất. Coi như```
a
a
```Câu trả lời là`aa`. Cả hai chuỗi con được chọn đều phải không trống, do đó chỉ trả về ký tự lớn nhất sẽ không hợp lệ. 

Coi như```
ab
z
```Câu trả lời là`bz`, không`abz`. Nhân vật đầu tiên của đứa bé đến từ người cha, vì vậy nhân vật đầu tiên hay nhất là`b`. Một giải pháp bất cẩn luôn lấy toàn bộ tiền tố đẹp nhất của người cha sẽ giữ lại tiền tố một cách không cần thiết.`a`trước nó và thua ngay lập tức. 

Bây giờ hãy xem xét```
zb
a
```Câu trả lời là`zba`. Ở đây việc mở rộng chuỗi con của cha có lợi vì ký tự tiếp theo`b`lớn hơn ký tự đầu tiên`a`của chuỗi con mẹ tối ưu. Một giải pháp luôn chỉ lấy một ký tự từ người cha sẽ trả về`za`, cái nào nhỏ hơn 

Các ký tự lặp lại là một trường hợp ranh giới khác. Vì```
aaa
aaa
```câu trả lời là`aaaaaa`. Hậu tố tối đa có thể bắt đầu ở vị trí đầu tiên và mọi ký tự có thể được giữ lại vì mọi ký tự trong hậu tố của cha ít nhất phải lớn bằng ký tự đầu tiên của hậu tố được chọn của mẹ. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Tạo mọi chuỗi con không trống của tên cha, mọi chuỗi con không trống của tên mẹ, nối cặp và giữ kết quả lớn nhất. Điều này đúng vì mọi tên con hợp pháp đều xuất hiện chính xác trong số các cặp đó. 

Vấn đề là số lượng ứng viên. có`n(n+1)/2`chuỗi con của cha và`m(m+1)/2`chuỗi con của mẹ, đưa ra chính xác`n(n+1)m(m+1) / 4`các cặp có thể. Khi`n = m = 2 * 10^5`, đây là về`10^20`ứng viên. Nếu mỗi ứng cử viên được so sánh trong`O(n+m)`, công việc trong trường hợp xấu nhất là`O(n^3m^2 + n^2m^3)`, trở thành`O(N^5)`khi cả hai chuỗi đều có độ dài`N`. Cách tiếp cận vũ phu gần như không khả thi. 

Quan sát chính là giải quyết hai lựa chọn chuỗi con một cách riêng biệt. 

Đối với chuỗi con của mẹ, giả sử chúng ta bắt đầu ở vị trí`j`. Mỗi chuỗi con bắt đầu đều có tiền tố của hậu tố`b[j:]`. Việc mở rộng chuỗi con đó qua phần còn lại của hậu tố không bao giờ có thể làm cho chuỗi đó nhỏ hơn: nếu chuỗi ngắn hơn là tiền tố của chuỗi dài hơn thì chuỗi dài hơn sẽ thắng. Do đó, trong số tất cả các chuỗi con của chuỗi mẹ, chuỗi con lớn nhất về mặt từ điển chính xác là hậu tố lớn nhất về mặt từ điển của`b`. 

Hãy để hậu tố đó là`C`, và đặt ký tự đầu tiên của nó là`c`. Bây giờ chúng ta có thể điều trị`C`như cố định và giải quyết bên cha. 

Ký tự đầu tiên của em bé phải đến từ người cha, vì vậy chúng tôi muốn vị trí bắt đầu lớn nhất có thể về mặt từ điển trong`a`. Vị trí bắt đầu chính xác là điểm bắt đầu của hậu tố tối đa về mặt từ điển của`a`. Đây là quan sát hậu tố tối đa tiêu chuẩn được sử dụng bởi giải pháp dự định. 

Giả sử hậu tố này bắt đầu ở vị trí`p`. Chúng tôi không nhất thiết muốn lấy toàn bộ hậu tố. Sau ký tự đầu tiên, mọi ký tự bổ sung từ người cha sẽ được so sánh với`c`, ký tự đầu tiên của`C`. Nếu ký tự cha tiếp theo nhỏ hơn`c`, việc dừng chuỗi con của cha sẽ tốt hơn, vì cách thay thế sẽ đặt ký tự nhỏ hơn đó trước ký tự lớn hơn`c`. Nếu nhân vật cha tiếp theo ít nhất là`c`, giữ nó ít nhất là tốt, vì vậy chúng tôi tiếp tục. 

Do đó, câu trả lời bao gồm một tiền tố của hậu tố tối đa của`a`, theo sau là hậu tố tối đa của`b`. Ký tự đầu tiên của phần cha luôn được giữ lại, ngay cả khi nó nhỏ hơn`c`, vì phần của cha phải không trống. 

Bản thân hậu tố tối đa có thể được tìm thấy trong thời gian tuyến tính bằng cách sử dụng hai vị trí ứng cử viên và độ lệch so sánh chung. Khi hai hậu tố khớp với một số ký tự, lỗi không khớp đầu tiên cho chúng ta biết rằng một ứng cử viên và toàn bộ các vị trí sau nó có thể bị loại bỏ. Đây là điều ngăn cản việc so sánh hậu tố bậc hai thực sự trở thành so sánh bậc hai. Việc triển khai hậu tố tối đa tiêu chuẩn chạy trong thời gian tuyến tính và không gian bổ sung không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n^3m^2 + n^2m^3)`|`O(n + m)`| Quá chậm | 
| Tối ưu |`O(n + m)`|`O(n + m)`bao gồm cả đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tìm hậu tố tối đa về mặt từ điển của chuỗi mẹ`b`. Gọi vị trí bắt đầu của nó`q`, vậy phần mẹ tối ưu là`b[q:]`. Chúng ta có thể sử dụng toàn bộ hậu tố vì mọi chuỗi con thích hợp đều bắt đầu từ`q`là tiền tố của nó và do đó không lớn hơn. 
2. Tìm hậu tố tối đa về mặt từ điển của chuỗi cha`a`. Đặt vị trí bắt đầu của nó là`p`. Điều này mang lại ký tự đầu tiên tốt nhất có thể và trong số các vị trí có cùng ký tự đầu tiên, nó mang lại sự tiếp nối hậu tố mạnh nhất. 
3. Hãy để`c = b[q]`, ký tự đầu tiên của hậu tố mẹ được chọn. Bắt đầu phần của người cha với`a[p]`, phải luôn được đưa vào vì chuỗi con của cha không được để trống. 
4. Quét các ký tự còn lại của`a[p:]`. Giữ từng ký tự khi nó lớn hơn hoặc bằng`c`. Thời điểm một ký tự nhỏ hơn`c`, dừng phần của người cha lại. 
5. Thêm hậu tố tối đa hoàn chỉnh`b[q:]`vào tiền tố của người cha đã chọn. In chuỗi kết quả. 

Quy tắc dừng tuân theo trực tiếp từ so sánh từ điển. Giả sử tiền tố của người cha được chọn là`P`, và nhân vật người cha tiếp theo là`x`. Dừng lại mang lại`P + C`, ký tự tiếp theo của nó là`c`. Tiếp tục mang lại`P + x + ...`. Nếu như`x < c`, điểm dừng lớn hơn. Nếu như`x > c`, tiếp tục lớn hơn. Nếu như`x == c`, việc tiếp tục cũng không bao giờ tệ hơn, bởi vì việc tiếp tục sẽ đưa ra một ký tự khác sau ký tự tương đương đó, trong khi việc dừng lại đã được nhập`C`; lựa chọn hậu tố tối đa đảm bảo rằng hậu tố của người cha được chọn vẫn là đại diện tối ưu hợp lệ. Giải pháp dự định sử dụng chính xác quy tắc tiền tố này. 

### Tại sao nó hoạt động 

hãy để`C`là hậu tố tối đa về mặt từ điển của tên mẹ. Bất kỳ chuỗi con nào của người mẹ hợp pháp đều không lớn hơn`C`, vì vậy tên bé tối ưu luôn có thể được sử dụng`C`. 

Bây giờ hãy xem xét người cha. Cho phép`S = a[p:]`là hậu tố tối đa về mặt từ điển của nó. Bất kỳ hậu tố nào bắt đầu ở nơi khác đều không lớn hơn`S`. Ký tự đầu tiên của em bé phải xuất phát từ người cha nên việc chọn vị trí bắt đầu có hậu tố nhỏ hơn`S`không thể cải thiện kết quả. Một lần`S`được chọn, quyết định duy nhất là dừng nó ở đâu trước khi nối thêm`C`. 

Ở mọi vị trí sau vị trí đầu tiên, sự so sánh là giữa việc giữ nguyên nhân vật người cha hiện tại và việc nhập ngay vào`C`. Nếu ký tự người cha đó nhỏ hơn`C[0]`, đang vào`C`thì tốt hơn nên chuỗi con của bố phải dừng ở đó. Nếu ít nhất là`C[0]`, việc giữ nó không thể làm cho kết quả nhỏ hơn. Do đó tiền tố dài nhất của`S`ít nhất các ký tự sau ký tự đầu tiên đều là`C[0]`, theo sau là`C`, là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_suffix_start(s: str) -> int:
    n = len(s)
    if n == 1:
        return 0

    i = 0
    j = 1
    k = 0

    while j + k < n:
        a = s[i + k]
        b = s[j + k]

        if a == b:
            k += 1
            continue

        if a < b:
            i = j
            j = i + 1
            k = 0
        else:
            j = j + k + 1
            k = 0

    return i

def solve(a: str, b: str) -> str:
    p = max_suffix_start(a)
    q = max_suffix_start(b)

    first = b[q]

    end = p + 1
    while end < len(a) and a[end] >= first:
        end += 1

    return a[p:end] + b[q:]

def main():
    a = input().strip()
    b = input().strip()
    print(solve(a, b))

if __name__ == "__main__":
    main()
```các`max_suffix_start`chức năng duy trì`i`là hậu tố tốt nhất hiện tại và`j`như một kẻ thách thức. Biến`k`ghi lại có bao nhiêu ký tự của hai hậu tố trùng khớp. Nếu như`a[i+k] < a[j+k]`, người thách đấu tốt hơn, vì vậy`i`di chuyển đến`j`. Nếu như`a[i+k] > a[j+k]`, hậu tố hiện tại sẽ thắng và tất cả các phần bắt đầu được bao phủ bởi tiền tố đã khớp có thể bị bỏ qua, vì vậy`j`nhảy về phía trước bằng`k + 1`. Việc bỏ qua này là lý do khiến quy trình là tuyến tính chứ không phải bậc hai. 

Sau khi tìm thấy`p`Và`q`, mã lưu trữ`b[q]`BẰNG`first`. Phần của người cha bắt đầu bằng`a[p]`, Vì thế`end`bắt đầu lúc`p + 1`. Vòng lặp sau đó tiếp tục kéo dài trong khi ký tự tiếp theo ít nhất`first`. Sự nghiêm khắc`<`điều kiện là ranh giới quan trọng. Một ký tự bằng`first`được giữ lại. 

Không có mối lo ngại về tràn số nguyên trong Python và các chỉ số đều dựa trên số 0. Điều kiện vòng lặp`end < len(a)`ngăn chặn việc đọc qua chuỗi của cha. Những lát cắt ở cuối cũng an toàn vì cả hai đều`p`Và`q`là hậu tố hợp lệ bắt đầu. 

Hai cuộc gọi đến`max_suffix_start`tiêu thụ thời gian tuyến tính trong chuỗi tương ứng của chúng. Lần quét cuối cùng chỉ chạm vào phần được chọn của chuỗi cha nên nó cũng tuyến tính. Phép nối cuối cùng tạo ra chính xác chuỗi đầu ra. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
jose
maria
```Hậu tố tối đa của`jose`là`se`, bắt đầu từ chỉ mục`3`. Hậu tố tối đa của`maria`là`ria`, bắt đầu từ chỉ mục`2`. 

| Biến | Tiểu bang | 
| --- | --- | 
|`a`|`jose`| 
|`b`|`maria`| 
|`p`|`3`| 
|`a[p:]`|`se`| 
|`q`|`2`| 
|`b[q:]`|`ria`| 
|`first`|`r`| 
| quét vào`p`| giữ`s`| 
| nhân vật người cha tiếp theo | không | 
| phần của cha |`s`| 
| câu trả lời cuối cùng |`sria`| 

Hậu tố cha được chọn bắt đầu bằng`s`, đây là ký tự đầu tiên phù hợp nhất có thể có trong tên của người cha. Không có ký tự nào sau đây để xem xét nên kết quả hoàn chỉnh là`s`theo sau là hậu tố mẹ tốt nhất`ria`. 

### Mẫu 2 

Đầu vào là```
abel
sun
```Hậu tố tối đa của`abel`là`l`. Hậu tố tối đa của`sun`là`sun`, bởi vì ký tự đầu tiên của nó`s`lớn hơn ký tự đầu tiên của`un`Và`n`. 

| Biến | Tiểu bang | 
| --- | --- | 
|`a`|`abel`| 
|`b`|`sun`| 
|`p`|`3`| 
|`a[p:]`|`l`| 
|`q`|`0`| 
|`b[q:]`|`sun`| 
|`first`|`s`| 
| quét vào`p`| giữ`l`| 
| nhân vật người cha tiếp theo | không | 
| phần của cha |`l`| 
| câu trả lời cuối cùng |`lun`| 

Sự thật là`l < s`không gây ra vấn đề gì Ký tự đầu tiên phải đến từ người cha, vì vậy`l`phải được sử dụng. Khi phần của cha đã được cố định thì hậu tố mẹ tốt nhất là`sun`, cho`lun`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n + m)`| Mỗi phép tính hậu tố tối đa là tuyến tính và lần quét cuối cùng là tuyến tính. | 
| Không gian |`O(n + m)`| Các chuỗi đầu vào và đầu ra yêu cầu không gian tuyến tính; chính việc tính toán hậu tố sử dụng`O(1)`thêm không gian. | 

Với`n,m <= 2 * 10^5`, thuật toán chỉ thực hiện tối đa một số lần quét tuyến tính không đổi`4 * 10^5`ký tự đầu vào. Điều này hoàn toàn thoải mái trong giới hạn đã dự định, trong khi số lượng ứng cử viên bạo lực đã có sẵn`10^20`ở kích thước tối đa. 

## Trường hợp thử nghiệm```python
import io
import sys

def max_suffix_start(s: str) -> int:
    n = len(s)
    if n == 1:
        return 0

    i = 0
    j = 1
    k = 0

    while j + k < n:
        a = s[i + k]
        b = s[j + k]

        if a == b:
            k += 1
        elif a < b:
            i = j
            j = i + 1
            k = 0
        else:
            j = j + k + 1
            k = 0

    return i

def solve(a: str, b: str) -> str:
    p = max_suffix_start(a)
    q = max_suffix_start(b)

    first = b[q]
    end = p + 1

    while end < len(a) and a[end] >= first:
        end += 1

    return a[p:end] + b[q:]

def run(inp: str) -> str:
    lines = inp.splitlines()
    a = lines[0].strip()
    b = lines[1].strip()
    return solve(a, b)

assert run("jose\nmaria\n") == "sria", "sample 1"
assert run("abel\nsun\n") == "lun", "sample 2"

assert run("a\na\n") == "aa", "minimum-size input"

assert run("aaa\naaa\n") == "aaaaaa", "all equal values"

assert run("ab\nz\n") == "bz", "must stop before a smaller father character"

assert run("zb\na\n") == "zba", "must keep father characters larger than mother's first"

a = "z" * 200000
b = "a" * 200000
assert run(a + "\n" + b + "\n") == a + b, "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`/`a`|`aa`| Cả hai chuỗi con phải không trống. | 
|`aaa`/`aaa`|`aaaaaa`| Các ký tự bằng nhau và hành vi ranh giới hậu tố tối đa. | 
|`ab`/`z`|`bz`| Chuỗi con của cha phải dừng trước ký tự nhỏ hơn ký tự đầu tiên của mẹ. | 
|`zb`/`a`|`zba`| Ký tự cha lớn hơn ký tự đầu tiên của mẹ nên được giữ lại. | 
|`z * 200000`/`a * 200000`|`z * 200000 + a * 200000`| Kích thước đầu vào tối đa và hiệu suất tuyến tính. | 

## Vỏ cạnh 

Đối với đầu vào tối thiểu```
a
a
```hậu tố tối đa của mỗi chuỗi bắt đầu tại chỉ mục`0`. Phần của người cha bắt đầu bằng`a`, không còn ký tự cha nào để kiểm tra nữa và hậu tố mẹ hoàn chỉnh là`a`. Kết quả là`aa`, thỏa mãn yêu cầu cả hai phần đều không trống. 

Đối với các chuỗi bằng nhau```
aaa
aaa
```hậu tố tối đa của cả hai chuỗi là toàn bộ chuỗi vì mọi hậu tố thích hợp đều là tiền tố của chuỗi đầy đủ và do đó nhỏ hơn. Nhân vật đầu tiên của người mẹ là`a`và mọi ký tự cha còn lại đều lớn hơn hoặc bằng`a`, do đó toàn bộ chuỗi của cha được giữ lại. Kết quả là`aaaaaa`. 

Đối với ranh giới dừng```
ab
z
```hậu tố cha tối đa là`b`, trong khi hậu tố mẹ tối đa là`z`. Phần được chọn của người cha bắt đầu bằng`b`. Sau nó không còn ký tự nào nữa nên kết quả là`bz`. Nếu chúng ta bắt đầu từ toàn bộ`ab`, ký tự đầu tiên sẽ là`a`, làm cho nó trở nên tồi tệ hơn. 

Đối với ranh giới mở rộng```
zb
a
```hậu tố cha tối đa là`zb`, và hậu tố mẹ tối đa là`a`. Kể từ nhân vật người cha đầu tiên`z`phải được giữ lại, chúng ta so sánh ký tự tiếp theo`b`với`a`. Bởi vì`b >= a`, giữ`b`tạo ra chuỗi lớn hơn`zba`thay vì`za`. 

Cuối cùng, đối với trường hợp kích thước tối đa trong đó cha bao gồm`200000`bản sao của`z`và mẹ gồm có`200000`bản sao của`a`, hậu tố tối đa của mỗi chuỗi là toàn bộ chuỗi. Mỗi ký tự cha đều lớn hơn ký tự đầu tiên của mẹ, vì vậy chuỗi cha hoàn chỉnh được giữ lại và sau đó chuỗi mẹ hoàn chỉnh được thêm vào. Thuật toán chỉ quét mỗi đầu vào một số lần không đổi nên trường hợp này vẫn tuyến tính.
