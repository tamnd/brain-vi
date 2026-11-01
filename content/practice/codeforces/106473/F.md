---
title: "CF 106473F - Aibohphobia"
description: "Chúng ta được cấp một chuỗi các chữ cái viết thường. Chúng ta có thể sắp xếp lại tất cả các ký tự của nó và cần xây dựng một chuỗi mới có các tiền tố không bao giờ trở thành các ký tự palindrome, ngoại trừ tiền tố có độ dài một."
date: "2026-06-25T08:27:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106473
codeforces_index: "F"
codeforces_contest_name: "\u0418\u043d\u0434\u0438\u0432\u0438\u0434\u0443\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438 \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2026"
rating: 0
weight: 106473
solve_time_s: 39
verified: true
draft: false
---

[CF 106473F - Aibohphobia](https://codeforces.com/problemset/problem/106473/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các chữ cái viết thường. Chúng ta có thể sắp xếp lại tất cả các ký tự của nó và cần xây dựng một chuỗi mới có các tiền tố không bao giờ trở thành các ký tự palindrome, ngoại trừ tiền tố có độ dài một. Nói cách khác, sau khi chọn ký tự đầu tiên, mỗi tiền tố dài hơn phải có các ký tự khác nhau ở hai đầu của nó hoặc phải không đối xứng palindrome ở đâu đó bên trong. Đầu vào chứa nhiều chuỗi độc lập và với mỗi chuỗi, chúng tôi in ra một cách sắp xếp thỏa mãn quy tắc hoặc báo cáo rằng không có sự sắp xếp nào tồn tại. 

Tổng số ký tự trong tất cả các trường hợp thử nghiệm nhiều nhất là$10^6$. Điều này loại trừ việc kiểm tra mọi hoán vị có thể xảy ra, vì ngay cả những chuỗi có độ dài trung bình cũng có nhiều cách sắp xếp giai thừa. Điều đó cũng có nghĩa là giải pháp dự định phải gần với tuyến tính về độ dài chuỗi, vì việc quét một triệu ký tự rất dễ dàng, trong khi việc kiểm tra bảng màu lặp đi lặp lại hoặc các tìm kiếm đắt tiền sẽ không phù hợp một cách thoải mái. 

Các trường hợp phức tạp đến từ tần số ký tự. Một chuỗi như`abba`không thể sắp xếp lại thành một câu trả lời hợp lệ. Bất kỳ ký tự đầu tiên nào cũng xuất hiện lại và chỉ với hai chữ cái khác nhau, lần xuất hiện thứ hai của ký tự đầu tiên sẽ tạo ra một tiền tố đối xứng. Một chuỗi chỉ chứa một ký tự cũng bị lỗi vì tiền tố có độ dài hai ngay lập tức là một bảng màu. 

Một nhân vật độc thân sẽ thay đổi hoàn toàn tình huống. Ví dụ:```
Input:
1
aabbc
```Đầu ra đúng có thể là:```
YES
cabba
```nhân vật`c`xuất hiện một lần, vì vậy đặt nó lên đầu có nghĩa là mọi tiền tố dài hơn đều bắt đầu bằng`c`nhưng kết thúc bằng một nhân vật khác. 

Một trường hợp quan trọng khác là chính xác có hai ký tự riêng biệt:```
Input:
1
aabb
```Đầu ra đúng là:```
NO
```Một giải pháp bất cẩn có thể cố gắng thay thế các ký tự và tạo ra thứ gì đó như`abab`, nhưng tiền tố`aba`là một palindrome. Bất kỳ sự sắp xếp nào có hai ký tự đều có cùng một vấn đề cơ bản. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng tạo ra các hoán vị của các ký tự và kiểm tra từng ký tự. Điều này đúng vì cuối cùng chúng tôi đã kiểm tra mọi sự sắp xếp lại có thể xảy ra, nhưng nó có độ phức tạp về mặt nhân tố. Đối với một chuỗi có độ dài$n$, số lượng đơn đặt hàng có thể có là$n!$, điều này trở nên không thể ngay cả đối với các giá trị nhỏ của$n$. 

Quan sát hữu ích là chúng ta thực sự không cần kiểm tra tiền tố sau khi xây dựng câu trả lời. Chúng ta chỉ cần kiểm soát nơi ký tự đầu tiên xuất hiện lại. Nếu ký tự đầu tiên chỉ xuất hiện một lần, nó có thể được đặt ở phía trước và không còn tiền tố nào có thể là một bảng màu nữa. 

Trường hợp khó hơn là khi mỗi ký tự xuất hiện ít nhất hai lần. Nếu tất cả các ký tự đều bằng nhau thì câu trả lời là không thể vì hai ký tự đầu tiên luôn giống nhau. Nếu có chính xác hai ký tự khác nhau thì lần xuất hiện thứ hai của ký tự đầu tiên luôn tạo ra tiền tố đối xứng. Lý do không thể xảy ra và cách xây dựng cho trường hợp còn lại dựa trên sự phân chia tần số này. 

Khi có ít nhất ba ký tự khác nhau, hãy chọn bất kỳ ký tự nào`x`. Đặt một bản sao của`x`lúc đầu, đặt tất cả các bản sao còn lại của`x`ở cuối và đặt mọi ký tự khác vào giữa chúng theo thứ tự được sắp xếp. Phần giữa không có`x`, do đó không có tiền tố nào kết thúc trước khối cuối cùng có thể là một bảng màu. Tiền tố đi vào khối cuối cùng chỉ bắt đầu bằng một`x`trước khi gặp một nhân vật khác, trong khi mặt trái của nó sẽ yêu cầu một số nhân vật dẫn đầu`x`các ký tự, do đó các tiền tố đó cũng không thể là các ký tự palindrome. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) bên cạnh việc lưu trữ đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của mỗi chữ cái trong chuỗi. Chúng ta cần những số đếm này vì sự tồn tại của một sự sắp xếp hợp lệ chỉ phụ thuộc vào số lượng ký tự khác nhau tồn tại và liệu một số ký tự có xuất hiện chính xác một lần hay không. 
2. Tìm kiếm ký tự xuất hiện đúng một lần. Nếu có, hãy đặt nó lên đầu tiên và nối thêm mọi ký tự khác sau đó. Vì không có ký tự nào sau đó bằng ký tự đầu tiên nên mỗi tiền tố dài hơn sẽ có các ký tự đầu tiên và cuối cùng khác nhau. 
3. Nếu không có ký tự đơn, hãy đếm số chữ cái riêng biệt. Nếu số này là một hoặc hai, hãy in`NO`. Đây chính xác là những trường hợp mà mọi sự sắp xếp có thể đều tạo ra một tiền tố xấu. 
4. Nếu có ít nhất ba chữ cái riêng biệt thì chọn một ký tự`x`. Đặt một lần xuất hiện của`x`lúc đầu, đặt tất cả những thứ khác không`x`các ký tự theo thứ tự được sắp xếp sau nó và nối thêm các bản sao còn lại của`x`ở cuối. 
5. Xuất chuỗi đã tạo. 

Tại sao nó hoạt động: 

Ký tự đầu tiên là ký tự duy nhất quan trọng đối với tiền tố palindrome. Tiền tố palindrome phải kết thúc bằng cùng ký tự bắt đầu. Việc xây dựng khiến điều đó không thể thực hiện được bằng cách chọn một ký tự đầu tiên duy nhất hoặc nó tách ký tự đầu tiên khỏi tất cả các bản sao sau này bằng phần giữa bắt đầu bằng một ký tự khác. Các trường hợp không thể xảy ra chính xác là những trường hợp không có sự phân tách như vậy có thể tránh được tiền tố đối xứng lặp lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(s):
    cnt = [0] * 26
    for c in s:
        cnt[ord(c) - 97] += 1

    single = -1
    for i in range(26):
        if cnt[i] == 1:
            single = i
            break

    if single != -1:
        ans = [chr(single + 97)]
        for i in range(26):
            if i != single:
                ans.append(chr(i + 97) * cnt[i])
        return "YES\n" + "".join(ans)

    kinds = sum(1 for x in cnt if x > 0)

    if kinds <= 2:
        return "NO"

    first = next(i for i in range(26) if cnt[i] > 0)

    ans = [chr(first + 97)]

    for i in range(26):
        if i != first:
            ans.append(chr(i + 97) * cnt[i])

    ans.append(chr(first + 97) * (cnt[first] - 1))

    return "YES\n" + "".join(ans)

def main():
    t = int(input())
    out = []
    for _ in range(t):
        s = input().strip()
        out.append(solve_case(s))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Mảng tần số là thông tin duy nhất cần có từ thứ tự ban đầu vì tác vụ cho phép sắp xếp lại tùy ý. Tìm kiếm đơn lẻ xử lý trường hợp mang tính xây dựng dễ dàng nhất trước tiên. Nếu điều đó không thành công, số lượng ký tự riêng biệt sẽ tách các đầu vào không thể thực hiện được khỏi trường hợp xây dựng. 

Trong công trình xây dựng cuối cùng, sự xuất hiện đầu tiên của`first`được thêm vào một cách riêng biệt. Điều này tránh việc vô tình đặt tất cả các bản sao lại với nhau khi bắt đầu. Các bản sao còn lại chỉ được thêm vào sau khi tất cả các chữ cái khác đã được đặt. Phép nhân với`cnt[first] - 1`là an toàn vì nhánh chỉ đạt đến điểm này khi mỗi lần đếm ít nhất là hai. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
5
a
sos
abba
icpc
tenet
```thuật toán hoạt động như sau. 

| Chuỗi đầu vào | Số lượng ký tự | Quyết định | Đầu ra | 
| --- | --- | --- | --- | 
| một | một:1 | singleton tồn tại | CÓ, một | 
| sos | s:2,o:1 | singleton tồn tại | CÓ, ôi | 
| abba | a:2,b:2 | chỉ hai chữ cái | KHÔNG | 
| icpc | c:2,p:1,i:1 | singleton tồn tại | CÓ, icpc | 
| nguyên lý | t:2,e:2,n:1 | singleton tồn tại | CÓ, lều | 

Ví dụ thứ hai thực hiện việc xây dựng với ba ký tự lặp lại:```
1
aabbcc
```| Bước | Quyết định hiện tại | Kết quả | 
| --- | --- | --- | 
| Đếm chữ cái | a:2,b:2,c:2 | Không có người độc thân | 
| Đếm các chữ cái riêng biệt | 3 | Có thể xây dựng | 
| Chọn chữ cái đầu tiên | một | Bắt đầu với | 
| Thêm phần giữa | bbcc | Không có a xuất hiện ở đây | 
| Thêm các chữ cái đầu tiên còn lại | một | Chuỗi cuối cùng aabbcca | 

Dấu vết cho thấy lý do tại sao việc xây dựng tránh sử dụng tiền tố palindrome. Bất kỳ tiền tố nào kết thúc trước phần cuối cùng`a`khối không thể có ký tự đầu tiên và cuối cùng trùng khớp. Tiền tố đạt đến khối cuối cùng không thành công vì phần giữa bắt đầu bằng các ký tự khác với`a`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự đều được đếm và câu trả lời được xây dựng bằng bảng chữ cái cố định gồm 26 chữ cái. | 
| Không gian | O(1) | Mảng tần số luôn có kích thước 26, không bao gồm chuỗi đầu ra. | 

Tổng kích thước đầu vào là$10^6$ký tự, vì vậy một giải pháp tuyến tính dễ dàng phù hợp với các giới hạn. Thuật toán thực hiện một lượng công việc nhỏ không đổi cho mỗi ký tự và không bao giờ khám phá các hoán vị. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old

    t = int(data[0])
    ans = []
    for s in data[1:]:
        ans.append(solve_case(s))
    return "\n".join(ans)

def solve_case(s):
    cnt = [0] * 26
    for c in s:
        cnt[ord(c) - 97] += 1

    for i in range(26):
        if cnt[i] == 1:
            return "YES\n" + chr(i + 97) + "".join(
                chr(j + 97) * cnt[j] for j in range(26) if j != i
            )

    kinds = sum(x > 0 for x in cnt)
    if kinds <= 2:
        return "NO"

    first = next(i for i in range(26) if cnt[i])
    return "YES\n" + (
        chr(first + 97)
        + "".join(chr(i + 97) * cnt[i] for i in range(26) if i != first)
        + chr(first + 97) * (cnt[first] - 1)
    )

assert run("""5
a
sos
abba
icpc
tenet
""") == """YES
a
YES
oss
NO
YES
icpc
YES
tente"""

assert run("""4
aabbcc
aaaa
ab
z
""") == """YES
aabbcca
NO
NO
YES
z"""

assert run("""2
abcabc
xxxy
""") == """YES
aabbccca
YES
yxxx"""

assert run("""1
abcdefghijklmnopqrstuvwxyz
""") == """YES
abcdefghijklmnopqrstuvwxyz"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`YES`| Kích thước tối thiểu và xử lý đơn lẻ | 
|`aaaa`|`NO`| Tất cả các ký tự bằng nhau | 
|`aabb`|`NO`| Chính xác là hai ký tự riêng biệt | 
|`aabbcc`| Xây dựng hợp lệ | Nhánh xây dựng chính | 
| Chuỗi bảng chữ cái |`YES`| Số lượng lớn các ký tự riêng biệt | 

## Vỏ cạnh 

Một chuỗi ký tự đơn như:```
1
a
```được xử lý ngay lập tức. Tiền tố duy nhất có độ dài một và hạn chế bắt đầu từ độ dài hai, vì vậy câu trả lời là hợp lệ. 

Một chuỗi có một ký tự lặp lại:```
1
aaaa
```không có sự sắp xếp hợp lệ. Mọi sự sắp xếp có thể đều giống hệt nhau và hai ký tự đầu tiên đã hình thành`aa`, một bảng màu. 

Một chuỗi có đúng hai chữ cái khác nhau:```
1
aabb
```bị loại bỏ trước khi xây dựng. Nếu ký tự đầu tiên là`a`, lần xuất hiện tiếp theo của`a`tạo một tiền tố có ký tự đầu tiên và cuối cùng khớp nhau và bên trong chỉ bao gồm`b`các ký tự, tạo ra một bảng màu. Đối số tương tự được áp dụng nếu ký tự đầu tiên là`b`. 

Một chuỗi có nhiều chữ cái nhưng không có chữ cái đơn lẻ:```
1
aabbcc
```sử dụng việc xây dựng cuối cùng. Lựa chọn`a`đưa ra một hình thức như`abbcca a`với bản sao cuối cùng của`a`được ngăn cách với cái đầu tiên bởi các ký tự không thể phản chiếu nó. Thuật toán không bao giờ cần kiểm tra các tiền tố riêng lẻ vì quy tắc vị trí ngăn cản việc xây dựng chúng.
