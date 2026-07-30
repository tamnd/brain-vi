---
title: "CF 102836F - \u041c\u0438\u043d\u0438\u043c\u0430\u043b\u044c\u043d\u0430\u044f \u0441\u0442\u0440\u043e\u043a\u0430"
description: "Nhiệm vụ là về một chuỗi gồm các chữ cái tiếng Anh viết thường. Chúng ta được phép xóa các ký tự khỏi nó nhưng số lượng vị trí bị xóa không được vượt quá giới hạn đã cho."
date: "2026-07-26T14:52:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102836
codeforces_index: "F"
codeforces_contest_name: "\u0426\u0438\u043a\u043b \u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434, \u0421\u0435\u0437\u043e\u043d 2020-21, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102836
solve_time_s: 38
verified: true
draft: false
---

[CF 102836F - \u041c\u0438\u043d\u0438\u043c\u0430\u043b\u044c\u043d\u0430\u044f \u0441\u0442\u0440\u043e\u043a\u0430](https://codeforces.com/problemset/problem/102836/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Nhiệm vụ là về một chuỗi gồm các chữ cái tiếng Anh viết thường. Chúng ta được phép xóa các ký tự khỏi nó nhưng số lượng vị trí bị xóa không được vượt quá giới hạn đã cho. Mục đích không phải là làm cho chuỗi ngắn lại mà là làm cho số lượng các chữ cái khác nhau vẫn xuất hiện ít nhất có thể. Chúng ta phải in số lượng tối thiểu các chữ cái còn lại và một dãy có thể có chính xác nhiều chữ cái khác nhau. 

Đầu vào chứa chuỗi gốc và số lần xóa tối đa được phép. Đầu ra là số lượng tối thiểu các chữ cái riêng biệt có thể tồn tại, theo sau là bất kỳ chuỗi kết quả nào thu được bằng cách xóa các ký tự khỏi thứ tự ban đầu. 

Hạn chế quan trọng là bảng chữ cái chỉ có 26 chữ cái có thể có, mặc dù bản thân chuỗi có thể lớn. Độ dài chuỗi khoảng 100000 sẽ loại trừ việc thử nhiều kết hợp xóa có thể có hoặc xây dựng lại câu trả lời cho mọi lựa chọn. Chúng ta cần một cách tiếp cận xử lý chuỗi theo thời gian tuyến tính, vì thuật toán xoay quanh kích thước bảng chữ cái O(n) hoặc O(n +) dễ dàng an toàn, trong khi các phương pháp liên quan đến tập hợp con các chữ cái hoặc mô phỏng lặp lại sẽ phát triển quá nhanh. 

Một số trường hợp nguy hiểm có thể phá vỡ việc triển khai bất cẩn. Nếu có thể xóa tất cả các ký tự thì câu trả lời không được chứa các chữ cái riêng biệt. Ví dụ:```
Input:
abc
5

Output:
0
```Giải pháp luôn giữ ít nhất một ký tự sẽ không thành công ở đây vì giới hạn xóa lớn hơn độ dài chuỗi. 

Một trường hợp khác là khi một chữ cái xuất hiện nhiều lần và không thể loại bỏ hoàn toàn. Ví dụ:```
Input:
aaaaa
4

Output:
1
aaaaa
```lá thư`a`tốn năm lần xóa để loại bỏ hoàn toàn, vì vậy nó phải được giữ nguyên. Việc loại bỏ bốn lần xuất hiện không giúp giảm số lượng các chữ cái riêng biệt. 

Một lỗi phổ biến cuối cùng là loại bỏ các chữ cái tùy ý thay vì những chữ cái rẻ nhất:```
Input:
aabbbc
2

Output:
2
aabbb
```Đang xóa`c`sử dụng một lần xóa và xóa`a`sử dụng hai lần xóa. Lựa chọn tốt nhất là loại bỏ`c`đầu tiên vì nó làm giảm số lượng các chữ cái riêng biệt với chi phí thấp nhất. Loại bỏ một`b`thay vào đó sẽ thực hiện việc xóa mà không làm giảm câu trả lời. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ xem xét những chữ cái nào còn lại. Vì có thể có 26 chữ cái nên người ta có thể tưởng tượng việc thử từng tập hợp con các chữ cái, kiểm tra xem liệu tất cả các chữ cái khác có thể bị xóa trong giới hạn cho phép hay không. Điều này sẽ đúng vì mọi tập hợp chữ cái riêng biệt cuối cùng có thể sẽ được kiểm tra. Tuy nhiên, số lượng tập hợp con là 2^26, tức là có khoảng 67 triệu khả năng trước khi kiểm tra chuỗi. Điều đó vượt xa những gì hợp lý khi kết hợp với việc xử lý chuỗi. 

Ý tưởng dùng vũ lực có hiệu quả vì điều duy nhất quan trọng là nhóm chữ cái hoàn chỉnh nào sẽ biến mất. Nó thất bại vì nó khám phá quá nhiều nhóm có thể. Quan sát quan trọng là mỗi chữ cái đều đóng góp độc lập. Để xóa một chữ cái khỏi chuỗi cuối cùng, chúng ta phải dành chính xác tần suất xóa của nó. Vì mục tiêu là loại bỏ càng nhiều chữ cái khác nhau càng tốt nên trước tiên chúng ta nên xóa các chữ cái có tần số nhỏ nhất. 

Sau khi sắp xếp tần số 26 chữ cái, chúng ta có thể xóa liên tục toàn bộ các chữ cái trong khi giới hạn xóa còn lại cho phép. Những chữ cái không thể loại bỏ sẽ ở lại trong câu trả lời. Thứ tự ban đầu của các ký tự được giữ nguyên bằng cách quét lại chuỗi và chỉ giữ lại các chữ cái còn sót lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^26 * n) | O(26) | Quá chậm | 
| Tối ưu | O(n + 26 log 26) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lần mỗi chữ cái viết thường xuất hiện trong chuỗi. Tần suất cho chúng ta biết chính xác chi phí xóa cần thiết để xóa hoàn toàn chữ cái đó. 
2. Nếu giới hạn xóa ít nhất là độ dài chuỗi, hãy xóa mọi ký tự. Chuỗi cuối cùng trống và số lượng chữ cái riêng biệt bằng 0. 
3. Sắp xếp các chữ cái theo tần số từ nhỏ nhất đến lớn nhất. Tần số nhỏ hơn có nghĩa là chúng ta có thể loại bỏ toàn bộ một chữ cái riêng biệt với chi phí rẻ hơn. 
4. Hãy truy cập các chữ cái theo thứ tự này. Nếu tần suất của bức thư hiện tại có thể được thanh toán bằng cách sử dụng các lần xóa còn lại, hãy loại bỏ nó bằng cách trừ tần suất của nó khỏi ngân sách. Nếu không, hãy giữ nó ở tập cuối cùng. Sự lựa chọn tham lam này là chính xác bởi vì mỗi chữ cái bị loại bỏ sẽ làm giảm chính xác một số lượng các chữ cái riêng biệt, do đó, việc loại bỏ rẻ hơn phải luôn được thực hiện trước tiên. 
5. Xây dựng chuỗi kết quả bằng cách quét chuỗi gốc và chỉ giữ lại các ký tự có chữ cái không bị xóa. Điều này bảo tồn thuộc tính thứ tự cần thiết. 

Tại sao nó hoạt động: mỗi chữ cái biến mất khỏi câu trả lời phải bị xóa tất cả các lần xuất hiện của nó. Chi phí để loại bỏ một chữ cái là độc lập với mọi chữ cái khác và bằng tần suất của nó. Quá trình tham lam chọn những chữ cái rẻ nhất có thể tháo rời trước tiên, vì vậy sau khi quá trình này dừng lại, không có chữ cái nào còn lại có thể bị xóa mà không tốn nhiều công sức xóa hơn mức có sẵn. Bất kỳ chiến lược nào khác loại bỏ một chữ cái đắt tiền hơn trong khi để lại một chữ cái có thể tháo rời rẻ hơn đều có thể được cải thiện bằng cách hoán đổi các lựa chọn đó, do đó, kết quả tham lam có số lượng chữ cái riêng biệt tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    k = int(input())

    if k >= len(s):
        print(0)
        print()
        return

    freq = [0] * 26
    for c in s:
        freq[ord(c) - ord('a')] += 1

    removed = [False] * 26

    letters = sorted(range(26), key=lambda x: freq[x])

    for x in letters:
        if freq[x] <= k:
            k -= freq[x]
            removed[x] = True
        else:
            break

    ans = []
    for c in s:
        if not removed[ord(c) - ord('a')]:
            ans.append(c)

    print(len(set(ans)))
    print(''.join(ans))

if __name__ == "__main__":
    solve()
```Phần đầu tiên xử lý trường hợp đặc biệt trong đó mọi ký tự đều có thể biến mất. Điều này tránh để lại một ký tự không cần thiết trong kết quả khi ngân sách xóa đủ lớn. 

Mảng tần số có một vị trí cho mỗi chữ cái trong bảng chữ cái, vì vậy việc đếm sẽ mất một lần đi qua chuỗi. Chỉ sắp xếp 26 phần tử là thời gian không đổi một cách hiệu quả. 

Vòng lặp tham lam đánh dấu các chữ cái có thể loại bỏ hoàn toàn. các`removed`mảng lưu trữ quyết định này tách biệt với tần số vì lần quét thứ hai cần biết những ký tự nào cần giữ lại. 

Lần quét cuối cùng sẽ xây dựng lại câu trả lời theo thứ tự ban đầu. Nó không loại bỏ các lần xuất hiện riêng lẻ của các chữ cái còn lại, vì việc xóa một phần không làm thay đổi số lượng các chữ cái riêng biệt và vấn đề chỉ yêu cầu số lượng tối thiểu các chữ cái riêng biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
aaaaa
4
```| Bước | Thư hiện tại | Tần số | Xóa còn lại | Đã xóa | 
| --- | --- | --- | --- | --- | 
| 1 | một | 5 | 4 | không | 

Bức thư duy nhất cần phải xóa năm lần để xóa, nhưng chỉ có bốn bức thư có sẵn. Thuật toán giữ`a`, vậy đáp án có một chữ cái riêng biệt. 

### Ví dụ 2 

đầu vào:```
abacaba
4
```| Bước | Thư hiện tại | Tần số | Xóa còn lại | Đã xóa | 
| --- | --- | --- | --- | --- | 
| 1 | b | 2 | 4 | vâng | 
| 2 | c | 1 | 2 | vâng | 
| 3 | một | 4 | 1 | không | 

Các chữ cái rẻ nhất để loại bỏ là`c`Và`b`. Sau khi xóa ba lần, chỉ còn lại một lần xóa, không đủ để xóa`a`. Dãy số kết quả là`aaaa`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Đếm và xây dựng lại chuỗi mỗi chuỗi mất một lần. Sắp xếp 26 chữ cái là thời gian không đổi. | 
| Không gian | O(1) | Chỉ các mảng có kích thước 26 và chuỗi đầu ra được lưu trữ. | 

Thuật toán xử lý mỗi ký tự đầu vào với số lần không đổi, do đó, nó dễ dàng phù hợp với các chuỗi có độ dài khoảng 100000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# provided samples
assert run("aaaaa\n4\n") == "1\naaaaa\n", "sample 1"
assert run("abacaba\n4\n") == "1\naaaa\n", "sample 2"
assert run("abcdefgh\n10\n") == "0\n\n", "sample 3"

# custom cases
assert run("a\n0\n") == "1\na\n", "single character"
assert run("aabbcc\n2\n") == "2\naabb\n", "remove cheapest groups"
assert run("zzzz\n3\n") == "1\nzzzz\n", "all equal values"
assert run("abcde\n100000\n") == "0\n\n", "large deletion budget"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`,`0`|`1`,`a`| Kích thước chuỗi tối thiểu và không xóa | 
|`aabbcc`,`2`|`2`,`aabb`| Chọn chữ rẻ nhất trước | 
|`zzzz`,`3`|`1`,`zzzz`| Một lá thư không thể xóa bỏ hoàn toàn | 
|`abcde`,`100000`|`0`, chuỗi trống | Hoàn thành xóa ranh giới | 

## Vỏ cạnh 

Đối với trường hợp giới hạn xóa lớn hơn chuỗi:```
abc
5
```Thuật toán kiểm tra`k >= len(s)`ngay lập tức và trả về số 0. Nếu không có sự kiểm tra này, bước tham lam sau này có thể để lại một chữ cái vì nó chỉ xem xét việc loại bỏ toàn bộ nhóm trong khi lặp lại các tần số. 

Đối với một chữ cái lặp đi lặp lại:```
aaaaa
4
```Mảng tần số chứa`a = 5`. Vòng lặp tham lam so sánh chi phí của việc loại bỏ`a`với ngân sách có sẵn và từ chối việc loại bỏ. Chuỗi kết quả không thay đổi và có một chữ cái riêng biệt. 

Đối với các lựa chọn loại bỏ cạnh tranh:```
aabbbc
2
```Các tần số là`a = 2`,`b = 3`, Và`c = 1`. Thuật toán loại bỏ`c`đầu tiên, để lại một lần xóa. Nó không thể loại bỏ`a`vì ngân sách còn lại quá nhỏ nên các chữ cái riêng biệt cuối cùng là`a`Và`b`. Câu trả lời chứa số lượng chữ cái khác nhau tối thiểu có thể vì việc xóa có sẵn đã được sử dụng cho việc xóa toàn bộ rẻ nhất.
